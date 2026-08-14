# ExtFUSE_Code Working Guidelines

## Path definitions

- `ExtFUSE_Code` = `/home/leedaeeun/Documents/github/ExtFUSE_Code` (this repository)
- `linux` = `/home/leedaeeun/Documents/github/linux`
- `fuse_kbuild_tools` = `/home/leedaeeun/Documents/github/fuse_kbuild_tools`
- `research_exp` = `/home/leedaeeun/Documents/github/research_exp`

Every `<name>/<subpath>` used below is anchored to these absolute paths, regardless of
the shell's current working directory.

## Repository overview

- This is a **frozen, read-only research archive** (not an active codebase) collecting the code
  behind the USENIX ATC'19 paper "Extension Framework for File Systems in User space" (ExtFUSE).
  Its purpose is side-by-side comparison and archival integrity, not development or building.
- It bundles an unmodified Linux baseline and the ExtFUSE-patched Linux, an unmodified libfuse
  baseline and the ExtFUSE-patched libfuse, the standalone `ExtFUSE_Code/extfuse/` BPF-policy
  library, and the preserved git history/patches/bundles for all three lineages (after the five
  nested `.git` dirs were intentionally deleted — see
  `ExtFUSE_Code/diffs/POST_DELETE_VERIFICATION.md`).
- **Read `ExtFUSE_Code/README.md` first** (Korean) — it documents the directory layout, the
  preserved git base/HEAD commit pairs for each lineage, and ready-made comparison commands. This
  file only covers write-protection policy; it does not duplicate that orientation content.
- This is the **historical baseline snapshot**, distinct from the active/live ExtFUSE kernel port,
  which lives in the sibling `linux` + `fuse_kbuild_tools` repos. There is no file-path coupling
  between them — they represent the same research lineage at different points in time.

### Directory layout (see `ExtFUSE_Code/README.md` for full detail)

| Path | Contents |
| --- | --- |
| `ExtFUSE_Code/linux_origin/linux` | Linux snapshot immediately before the ExtFUSE kernel patch (merge-base) |
| `ExtFUSE_Code/linux_ExtFUSE-1.0/linux` | Same Linux tree with the ExtFUSE fast-path + BPF ABI applied |
| `ExtFUSE_Code/libfuse_origin/libfuse` | libfuse snapshot immediately before the ExtFUSE patch (merge-base) |
| `ExtFUSE_Code/libfuse_ExtFUSE-1.0/libfuse` | Same libfuse tree with ExtFUSE capability/FD-passing applied |
| `ExtFUSE_Code/extfuse/` | Standalone BPF policy programs + `libextfuse.so` support library (no `_origin` counterpart — new code, not a fork) |
| `ExtFUSE_Code/diffs/` | Preserved patches, commit metadata/URLs, mailboxes, bundles, and SHA-256 sums for all three lineages, captured before the nested `.git` dirs were removed |
| `ExtFUSE_Code/pdf/` | Paper and slides |

## Scope

- This file applies to `ExtFUSE_Code` and all of its subpaths.
- This repository is a pristine reference and must be treated as **read-only**. This rule does not change even if the actual filesystem permissions allow writes.
- No file in the original repository — including this one — may be modified without a separate, explicit instruction that names this repository and the intended file or change. A broad request to work on a sibling repository is not such permission.

## Permitted operations in the original repository

- Viewing, searching, comparing, and performing read-only static analysis that creates no file, cache, log, or other artifact
- Non-mutating Git queries (e.g. `git status`, `git diff`, `git log`, `git show`, `git grep`, `git rev-parse`)
- Read-only inspection of file metadata and directory structure

### Ready-made comparison commands (from `ExtFUSE_Code/README.md`)

These are read-only and run fine directly against the original tree (no copy-out needed):

```bash
# Whole-tree file diff between baseline and ExtFUSE-patched Linux
diff -qr ExtFUSE_Code/linux_origin/linux ExtFUSE_Code/linux_ExtFUSE-1.0/linux

# Whole-tree file diff between baseline and ExtFUSE-patched libfuse
diff -ruN ExtFUSE_Code/libfuse_origin/libfuse ExtFUSE_Code/libfuse_ExtFUSE-1.0/libfuse

# Same comparisons via git (no shared .git needed; --no-index compares two paths directly)
git diff --no-index --stat -- \
  ExtFUSE_Code/linux_origin/linux ExtFUSE_Code/linux_ExtFUSE-1.0/linux
git diff --no-index -- \
  ExtFUSE_Code/libfuse_origin/libfuse/include/fuse_kernel.h \
  ExtFUSE_Code/libfuse_ExtFUSE-1.0/libfuse/include/fuse_kernel.h
```

`diff` and `git diff --no-index` exit `1` when a difference is found — that is expected, not an
error. See `ExtFUSE_Code/README.md` for the exact preserved base/HEAD commit SHAs per lineage and
the full `ExtFUSE_Code/diffs/` layout (patches, commit metadata, mailboxes, bundles).

## Prohibited operations in the original repository

- Creating, modifying, deleting, renaming files, or changing permissions
- Applying `apply_patch`, editors, formatters, or code generators to the original
- Builds, tests, compiler- or analyzer-driven checks, or any command that may produce caches, logs, objects, or temporary files
- Git operations that change repository state or remote repositories, such as `git add`, `commit`, `checkout`, `switch`, `restore`, `reset`, `clean`, `stash`, `merge`, `rebase`, `cherry-pick`, `pull`, `push`
- Using any path inside the original repository as a build, output, cache, or temporary directory. A shell may use the repository as its current directory only for the permitted read-only inspection above.

## When modification, build, or verification is required

1. Create a new, uniquely named temporary directory per task under `research_exp/cleanup/workspace/sources`.
2. Copy the required code, or the entire repository, into that directory. Do not use symbolic links that point files to be modified back to the original.
3. Verify with `pwd -P` that the current path is under `research_exp/cleanup/workspace/sources/` before performing any modification, build, test, formatting, or generation.
4. Keep all build artifacts, caches, logs, patches, and analysis results inside that temporary workspace as well.
5. Do not copy or sync results back into the original `ExtFUSE_Code`. Report any needed changes to the user as paths and diffs within the temporary workspace.

Example:

```bash
EXTFUSE_WORKDIR="$(mktemp -d research_exp/cleanup/workspace/sources/ExtFUSE_Code.XXXXXX)"
cp -a ExtFUSE_Code/. "$EXTFUSE_WORKDIR"/
cd -- "$EXTFUSE_WORKDIR"
pwd -P
```

- If `research_exp/cleanup/workspace/sources` does not exist, is not a physical directory, or is not writable, stop and notify the user instead of falling back to the original or any other path.
- Before starting work, record the commit ID of the original and the path of the copied workspace.
- Do not perform commits, pushes, installations, privilege escalation, or any external system change that the user did not explicitly request — not even in the copy.

## Top-priority invariant

**Unless the user separately and explicitly authorizes a named change in this repository, its tracked files, untracked files, file modes, index, refs, worktrees, and remotes must have no logical change after the work.**
