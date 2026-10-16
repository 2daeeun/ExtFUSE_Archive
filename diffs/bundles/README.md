# Preserved Git Bundles

**English** | [한국어](README.ko.md)

The bundles in this directory are supplementary archives for restoring the
original Git objects and refs after the nested `.git` directories have been
deleted.

## Included files

- `extfuse.bundle`: complete bundle containing all 29 `extfuse` commits and
  all refs.
- `libfuse.bundle`: complete bundle containing 1,799 `libfuse` commits,
  114 tags, and 130 named refs from the union of the two checkouts. Its `HEAD`
  points to `ExtFUSE-1.0`.
- `linux-extfuse-changes.bundle`: preserves the original Git objects for
  nine commits, 38 trees, and 23 blobs covering the ExtFUSE changes after the
  Linux baseline. This changes-only bundle is 12,647 bytes and requires the
  baseline commit `1d039859...` as a prerequisite.

The extfuse and libfuse bundles were verified by `git bundle verify` as having
“complete history.” The Linux changes bundle also passed verification and
fetching in a repository containing the prerequisite baseline commit. The
restored branch reproduced all nine commits and the original HEAD tree
`0b6b2080e7922a8301df156cd947aeddffd49145` exactly.

## Restoration

The examples below record the restoration procedures. Follow the
[working guidelines](../../AGENTS.md) and run commands that create files in
a separate workspace containing copies of the required archive files. Paths
in the examples are relative to the copied archive root.

```bash
git clone diffs/bundles/extfuse.bundle restored-extfuse
git clone diffs/bundles/libfuse.bundle restored-libfuse
```

To restore the original public remotes afterward, use these URLs:

```text
https://github.com/extfuse/extfuse.git
https://github.com/extfuse/libfuse.git
```

### Offline restoration of the Linux change history

The Linux bundle is incremental and requires a base prerequisite. The
preserved baseline source tree and raw base commit can reconstruct that
prerequisite even after the nested `.git` directories are deleted. Use the
following sequence from the copied archive root. `git add -f` is required to
include empty files tracked by the original Git repository but matched by
ignore rules, so that the exact base tree is reproduced.

```bash
BASE=1d039859330b874d48080885eb31f4f129c246f1
BASE_TREE=5988dc49e1534ec6e1fadca6c5b5231248a63b41
HEAD_TREE=0b6b2080e7922a8301df156cd947aeddffd49145

cp -a linux_origin/linux restored-linux
git -C restored-linux init
git -C restored-linux add -f -A
test "$(git -C restored-linux write-tree)" = "$BASE_TREE"

test "$(base64 --decode diffs/linux/raw-commits/$BASE.commit.b64 |
  git -C restored-linux hash-object -w -t commit --stdin)" = "$BASE"
printf '%s\n' "$BASE" > restored-linux/.git/shallow
git -C restored-linux update-ref refs/heads/baseline "$BASE"

git -C restored-linux fetch "$PWD/diffs/bundles/linux-extfuse-changes.bundle" \
  refs/heads/ExtFUSE-1.0:refs/heads/ExtFUSE-1.0
git -C restored-linux checkout ExtFUSE-1.0
test "$(git -C restored-linux write-tree)" = "$HEAD_TREE"
```

This procedure was run in a separate object database and verified for
connectivity (`git fsck --connectivity-only --no-dangling`), the nine changed
commits, and the final tree. When the public remote is available, fetching this
bundle into a clone of `extfuse/linux` can also satisfy the prerequisite.

### Command used to generate the Linux changes bundle

Before deletion, the bundle was generated in the original repository with:

```bash
git -C linux_ExtFUSE-1.0/linux bundle create \
  "$PWD/diffs/bundles/linux-extfuse-changes.bundle" \
  ExtFUSE-1.0 \
  ^1d039859330b874d48080885eb31f4f129c246f1

git -C linux_ExtFUSE-1.0/linux bundle verify \
  "$PWD/diffs/bundles/linux-extfuse-changes.bundle"
```

The bundle's SHA-256 is
`0f9907267abb2b48f8682da5bdf2963f32e2d88df59d598555d47d430d6a5cef`.

## Why a complete Linux bundle is not included

Before deletion, both Linux clones had identical 2,754,194,655-byte packs
containing 6,815,470 objects. The full history comprised 850,176 commits, and
each `.git` occupied about 2.9 GB. Following the user's request to focus on
the ExtFUSE changes, the entire upstream object database was not duplicated
again. Instead, the changes-only bundle above contains every commit/tree/blob
object in `BASE..HEAD`.

The repository preserves the following materials needed for Linux analysis:

- Both the baseline source tree immediately before the changes and the final
  ExtFUSE source tree
- Original GitHub URLs for the baseline commit and nine subsequent commits
- A bundle of the exact commit/tree/blob Git objects for those nine commits
- Exact raw commit objects, including commit/tree/parent IDs, author/committer
  information, and GPG signatures
- Per-commit mailbox, cumulative full-index patch, per-file statistics, and
  commit mappings
- Refs/tags/reflogs/config/object inventories

The complete upstream Linux object database is therefore unnecessary for
analyzing the ExtFUSE changes and reconstructing intermediate states. This
archive omits the complete upstream Linux bundle before the baseline. Obtain
that history from the public remote to explore the earlier range.

The original archive rationale also cited GitHub's limits of 100 MiB per
ordinary Git object and 2 GB per push, making a roughly 2.9 GB Linux bundle
unsuitable as a single ordinary file in this combined repository.

- Referenced GitHub documentation: <https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits>
