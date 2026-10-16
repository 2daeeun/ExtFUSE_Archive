# Preserved ExtFUSE Change History

**English** | [한국어](README.ko.md)

This directory is an archive that allows the baseline code and ExtFUSE changes
to be analyzed using `ExtFUSE_Code` after the five nested repositories' `.git`
directories have been deleted. It was created on 2026-07-14.

All five nested `.git` directories were removed on the same day. The
[`POST_DELETE_VERIFICATION.md`](POST_DELETE_VERIFICATION.md) record documents the
integrity checks on the remaining source trees and preservation materials.

As requested when the archive was created, preservation focuses on **the exact
baseline commit immediately before the ExtFUSE changes and the subsequent
ExtFUSE-related commits**. It retains baseline/modified source trees, exact
patches, commit objects, and original URLs, rather than another copy of Linux's
entire history of more than 850,000 upstream commits.

## Starting points and endpoints

| Component | Baseline immediately before changes, or first commit | Endpoint | Full comparison |
|---|---|---|---|
| Linux | [`1d039859330b`](https://github.com/extfuse/linux/commit/1d039859330b874d48080885eb31f4f129c246f1) · [baseline code](https://github.com/extfuse/linux/tree/1d039859330b874d48080885eb31f4f129c246f1) | [`d95ca8642fce`](https://github.com/extfuse/linux/commit/d95ca8642fce0214cf2ebaaa792ba538d4d8381d) | [compare](https://github.com/extfuse/linux/compare/1d039859330b874d48080885eb31f4f129c246f1..d95ca8642fce0214cf2ebaaa792ba538d4d8381d) |
| libfuse | [`386b1b6e3d0f`](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) · [baseline code](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [`5b381d81f0d2`](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | [compare](https://github.com/extfuse/libfuse/compare/386b1b6e3d0fcf7e2dfd5473e867284410dfa624..5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) |
| extfuse | No separate vanilla baseline · [first commit `5462699b5414`](https://github.com/extfuse/extfuse/commit/5462699b5414a739bc6c3221157827ac42250334) · [initial code](https://github.com/extfuse/extfuse/tree/5462699b5414a739bc6c3221157827ac42250334) | [`d2acdb1c8e45`](https://github.com/extfuse/extfuse/commit/d2acdb1c8e4531562fc77fe566896e771d03026e) | [compare](https://github.com/extfuse/extfuse/compare/5462699b5414a739bc6c3221157827ac42250334..d2acdb1c8e4531562fc77fe566896e771d03026e) |

The Linux and libfuse baseline commits are the exact merge-bases of their
ExtFUSE branches. Since `extfuse` is a newly written library/BPF repository,
all 29 commits are recorded from its initial commit onward.

## Commit URLs and original objects

| Component | Readable commit URL list | Machine-readable metadata | Exact raw objects |
|---|---|---|---|
| Linux | [`linux/COMMITS.md`](linux/COMMITS.md) | [`linux/commits.tsv`](linux/commits.tsv) | [`linux/raw-commits/`](linux/raw-commits) |
| libfuse | [`libfuse/COMMITS.md`](libfuse/COMMITS.md) | [`libfuse/commits.tsv`](libfuse/commits.tsv) | [`libfuse/raw-commits/`](libfuse/raw-commits) |
| extfuse | [`extfuse/COMMITS.md`](extfuse/COMMITS.md) | [`extfuse/commits.tsv`](extfuse/commits.tsv) | [`extfuse/raw-commits/`](extfuse/raw-commits) |

Every commit SHA in `COMMITS.md` links to its original GitHub commit URL.
Each `commits.tsv` also records:

- Full commit/tree/parent SHAs
- Original commit URL and complete source snapshot URL for that commit
- Raw tree API URL
- Each parent URL and parent-to-commit comparison URL
- Baseline-to-commit comparison URL
- Author/committer names, email addresses, and ISO 8601 timestamps
- GPG signature header presence, subject, and raw object paths

Alongside the readable `.commit` files, `.commit.b64` preserves each original
commit object down to its final byte. All 43 Base64 objects were decoded and
verified to reproduce the original SHAs in their filenames using this command:

```bash
base64 --decode diffs/linux/raw-commits/<SHA>.commit.b64 |
  git hash-object -t commit --stdin
```

## Code change records

| Component | Cumulative full-index patch | Per-commit mailbox | File → commit URL | Graph | Statistics |
|---|---|---|---|---|---|
| Linux | [`linux/changes.patch`](linux/changes.patch) | [`linux/series.mbox`](linux/series.mbox) | [`linux/files.tsv`](linux/files.tsv) | [`linux/history.graph.txt`](linux/history.graph.txt) | [diffstat](linux/diffstat.txt), [name-status](linux/name-status.tsv), [numstat](linux/numstat.tsv) |
| libfuse | [`libfuse/changes.patch`](libfuse/changes.patch) | [`libfuse/series.mbox`](libfuse/series.mbox) | [`libfuse/files.tsv`](libfuse/files.tsv) | [`libfuse/history.graph.txt`](libfuse/history.graph.txt) | [diffstat](libfuse/diffstat.txt), [name-status](libfuse/name-status.tsv), [numstat](libfuse/numstat.tsv) |
| extfuse | [`extfuse/changes.patch`](extfuse/changes.patch) | [`extfuse/series.mbox`](extfuse/series.mbox) | [`extfuse/files.tsv`](extfuse/files.tsv) | [`extfuse/history.graph.txt`](extfuse/history.graph.txt) | [diffstat](extfuse/diffstat.txt), [name-status](extfuse/name-status.tsv), [numstat](extfuse/numstat.tsv) |

- `changes.patch`: cumulative output from `git diff --binary --full-index`.
- `series.mbox`: author, date, message, and patch for each non-merge commit.
- `files.tsv`: reverse index mapping each changed file to the SHAs and original
  URLs of the commits that touched it.
- `history.graph.txt`: merge topology, full commit/tree/parent IDs, and
  author/committer information.

Cumulative statistics are 16 files (+508/-4) for Linux, three files (+27/-2)
for libfuse, and 11 files (+1,710) for the extfuse archived tree.

### Merge records

Linux merge `2473946e1ce9...` has the same tree as its second parent, so it
contains no additional conflict-resolution code. Its two original parents and
raw signed commit object are preserved in `linux/commits.tsv` and `raw-commits`.

The extfuse merge `b853870db92b...` combines its two parents and affects the
final tree. All three views are preserved:

- [First-parent diff](extfuse/merges/b853870.first-parent.patch)
- [Second-parent diff](extfuse/merges/b853870.second-parent.patch)
- [Combined metadata/diff](extfuse/merges/b853870.combined.diff)

Although ordinary `format-patch` omits merge commits, applying the 28 non-merge
commits from both branches in `series.mbox` order to an empty history
reconstructs the exact HEAD tree
`d59ddb65225c9b7dc492939e74d229d21640a09c` for this history. The merge object
and two-parent topology are separately preserved in `raw-commits`,
`history.graph.txt`, and the three merge diffs above. The cumulative
`extfuse/changes.patch` also produces the exact final tree from scratch.

## Git metadata and bundles

- [`git-metadata/`](git-metadata): HEADs, branch/detached states, refs/tags,
  reflogs, sanitized configs, statuses, object statistics, and
  hook/worktree/special-ref states for all five checkouts before deletion.
- [`git-metadata/object-integrity.txt`](git-metadata/object-integrity.txt):
  pack sizes and SHA-256 checksums, clone-pair identity, and connectivity checks.
- [`bundles/extfuse.bundle`](bundles/extfuse.bundle): complete Git bundle of
  all 29 extfuse commits.
- [`bundles/libfuse.bundle`](bundles/libfuse.bundle): complete Git bundle of
  all 1,799 libfuse commits and 114 tags.
- [`bundles/linux-extfuse-changes.bundle`](bundles/linux-extfuse-changes.bundle):
  changes-only bundle preserving nine commits, 38 trees, and 23 blobs after the
  Linux baseline; requires the baseline commit as a prerequisite.
- [Bundle restoration and reasons for excluding the complete Linux history](bundles/README.md)

Before deletion, each Linux clone's `.git` occupied about 2.9 GB and contained
the same 850,176 upstream commits. Following the request to focus on the
ExtFUSE changes, another 2.9 GB complete bundle was not copied. Instead, the
archive preserves the baseline/final source trees, a commit/tree/blob bundle
for the changed range, and raw commits, patches, URLs, and refs metadata.

## Quick inspection

Run these commands from the repository root.

```bash
# Original URLs and per-commit changes
less diffs/linux/COMMITS.md
less diffs/libfuse/COMMITS.md
less diffs/extfuse/COMMITS.md

# Cumulative and per-file changes
git apply --stat diffs/linux/changes.patch
git apply --numstat diffs/libfuse/changes.patch
rg '^diff --git' diffs/extfuse/changes.patch

# Integrity of all files covered by the manifest
sha256sum -c diffs/SHA256SUMS
```

## Patch verification

The following commands do not modify files.

```bash
git apply --check \
  --directory=linux_origin/linux \
  diffs/linux/changes.patch

git apply --check --reverse \
  --directory=linux_ExtFUSE-1.0/linux \
  diffs/linux/changes.patch

git apply --check \
  --directory=libfuse_origin/libfuse \
  diffs/libfuse/changes.patch

git apply --check --reverse \
  --directory=libfuse_ExtFUSE-1.0/libfuse \
  diffs/libfuse/changes.patch
```

All four checks passed with exit code 0. Applying the Linux and libfuse
`series.mbox` files to their respective base trees also produced the original
ExtFUSE HEAD tree IDs.

Applying the extfuse cumulative patch to an empty tree reproduced its archived
HEAD tree. Applying only the 28 non-merge patches in `series.mbox` to an empty
history produced the same tree.

```text
empty tree:   4b825dc642cb6eb9a060e54bf8d69288fbee4904
extfuse HEAD: d59ddb65225c9b7dc492939e74d229d21640a09c
```

## Generation commands

The basic cumulative patch command for Linux and libfuse was:

```bash
git -C REPOSITORY diff \
  --no-color --no-ext-diff --no-textconv \
  --binary --full-index --find-renames=50% \
  BASE..HEAD > changes.patch
```

The per-commit mailbox was generated with:

```bash
git -C REPOSITORY format-patch \
  --stdout --topo-order --no-merges --no-signature \
  --binary --full-index --no-ext-diff --no-textconv \
  --find-renames=50% BASE..HEAD > series.mbox
```

Exact commit objects were preserved by Base64-encoding the output of
`git cat-file commit <SHA>`.

## Integrity and limitations

[`SHA256SUMS`](SHA256SUMS) records SHA-256 checksums for all preserved files
under `diffs/`, including bundles and documentation, excluding the manifest
itself.

For the English/Korean documentation update, the previous manifest was retained
byte-for-byte as [`SHA256SUMS.archive`](SHA256SUMS.archive). It records the
documentation hashes from before translation and is a historical record,
rather than a manifest for the translated files. The active `SHA256SUMS`
updates only the seven existing Markdown entries and adds the seven Korean
counterparts under `diffs/` and the preserved manifest. Every existing
non-document payload retains its original path and checksum. The root READMEs
and `AGENTS.md` are outside this manifest's scope.

The archive supports analysis of the code before/after ExtFUSE, the content and
signatures of all changed commits, intermediate patches, per-file history, and
original web URLs. It does not provide completely offline exploration of the
more than 850,000 upstream Linux commits before the baseline; use the public
remote URLs for that history.
