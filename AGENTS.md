# ExtFUSE_Code Working Guidelines

## Scope

- This file applies to `/home/leedaeeun/Documents/github/ExtFUSE_Code` and all of its subpaths.
- This repository is a pristine reference and must be treated as **read-only**. This rule does not change even if the actual filesystem permissions allow writes.
- The only exception is the creation of this `AGENTS.md` itself, which the user explicitly authorized in that request. From this point on, no file in the original repository — including this one — may be modified without a separate, explicit instruction.

## Permitted operations in the original repository

- Viewing, searching, comparing, and statically analyzing sources and documents
- Non-mutating Git queries (e.g. `git status`, `git diff`, `git log`, `git show`, `git grep`, `git rev-parse`)
- Read-only inspection of file metadata and directory structure

## Prohibited operations in the original repository

- Creating, modifying, deleting, renaming files, or changing permissions
- Applying `apply_patch`, editors, formatters, or code generators to the original
- Builds, tests, static analysis, or any command that may produce caches, logs, objects, or temporary files
- Git operations that change repository state or remote repositories, such as `git add`, `commit`, `checkout`, `switch`, `restore`, `reset`, `clean`, `stash`, `merge`, `rebase`, `cherry-pick`, `pull`, `push`
- Using any path inside the original repository as a working directory, build directory, or output location

## When modification, build, or verification is required

1. Create a new, uniquely named temporary directory per task under `/home/leedaeeun/Documents/github/research_exp`.
2. Copy the required code, or the entire repository, into that directory. Do not use symbolic links that point files to be modified back to the original.
3. Verify with `pwd -P` that the current path is under `/home/leedaeeun/Documents/github/research_exp/` before performing any modification, build, test, formatting, or generation.
4. Keep all build artifacts, caches, logs, patches, and analysis results inside that temporary workspace as well.
5. Do not copy or sync results back into the original `ExtFUSE_Code`. Report any needed changes to the user as paths and diffs within the temporary workspace.

Example:

```bash
EXTFUSE_WORKDIR="$(mktemp -d /home/leedaeeun/Documents/github/research_exp/ExtFUSE_Code.XXXXXX)"
cp -a /home/leedaeeun/Documents/github/ExtFUSE_Code/. "$EXTFUSE_WORKDIR"/
cd -- "$EXTFUSE_WORKDIR"
pwd -P
```

- If `/home/leedaeeun/Documents/github/research_exp` does not exist or is not writable, stop and notify the user instead of falling back to the original or any other path.
- Before starting work, record the commit ID of the original and the path of the copied workspace.
- Do not perform commits, pushes, installations, privilege escalation, or any external system change that the user did not explicitly request — not even in the copy.

## Top-priority invariant

**The contents and Git state of `/home/leedaeeun/Documents/github/ExtFUSE_Code` must be identical before and after any work.**
