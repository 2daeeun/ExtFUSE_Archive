# Verification After Deleting the Nested `.git` Directories

**English** | [한국어](POST_DELETE_VERIFICATION.ko.md)

Verification date: 2026-07-14 (Asia/Seoul). After the user deleted the five
nested `.git` directories, the remaining sources and preservation materials
created before deletion were checked again for analysis and restoration.
This document records the state and checks on that date.

## Deletion status

`find . -type d -name .git` returned only the top-level `./.git`. The following
nested Git metadata directories had been deleted:

```text
extfuse/.git
linux_origin/linux/.git
linux_ExtFUSE-1.0/linux/.git
libfuse_origin/libfuse/.git
libfuse_ExtFUSE-1.0/libfuse/.git
```

All five source directories remained and could now be tracked as ordinary
files by the top-level repository.

## Source tree identity

Each source directory was read into an external temporary Git object database
with `git add -f -A`, and its tree ID was calculated, including file contents,
executable bits, and symlinks. Every tree exactly matched the original Git tree
recorded before deletion.

| Source | Commit before deletion | Verified tree |
|---|---|---|
| `linux_origin/linux` | `1d039859330b874d48080885eb31f4f129c246f1` | `5988dc49e1534ec6e1fadca6c5b5231248a63b41` |
| `linux_ExtFUSE-1.0/linux` | `d95ca8642fce0214cf2ebaaa792ba538d4d8381d` | `0b6b2080e7922a8301df156cd947aeddffd49145` |
| `libfuse_origin/libfuse` | `386b1b6e3d0fcf7e2dfd5473e867284410dfa624` | `d16206c9de6043e78f663b8cfadc835df2981f1e` |
| `libfuse_ExtFUSE-1.0/libfuse` | `5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d` | `500a56d0fd38f2ed1d8c82271d54abbaf1777499` |
| `extfuse` | `d2acdb1c8e4531562fc77fe566896e771d03026e` | `d59ddb65225c9b7dc492939e74d229d21640a09c` |

## Verification of preserved materials

- `diffs/SHA256SUMS`: all 132 final preserved files, including this document,
  reported `OK` in the original verification.
- Linux/libfuse cumulative patches: all four `git apply --check` checks passed
  with exit code 0, forward against the baselines and reverse against the
  ExtFUSE source trees.
- `extfuse.bundle` and `libfuse.bundle`: complete-history verification and bare
  cloning succeeded after deletion.
- `linux-extfuse-changes.bundle`: fetching succeeded after reconstructing the
  base tree and raw base commit from the preserved Linux baseline source.
- Restored Linux change history: nine commits, final tree `0b6b2080...`, and
  `git fsck --connectivity-only --no-dangling` exit code 0.

The complete upstream object database before the Linux baseline is outside
the scope of this change-focused archive. The ExtFUSE change range can be
analyzed and reconstructed from the sources, patches, raw commits, URLs, and
bundles.

The original 132-entry checksum manifest is now preserved as
[`SHA256SUMS.archive`](SHA256SUMS.archive). The active
[`SHA256SUMS`](SHA256SUMS) also covers the translated documents and Korean
copies. This documentation update does not constitute a new run of the
historical patch, bundle, or source-tree verification above.

## Top-level repository state at verification time

On the verification date, the top-level `ExtFUSE_Code/.git` was retained but
had no commits. The README, `diffs/`, and all five source directories were
untracked, and `origin/main` was shown as `[gone]`. At that time, these files
needed to be included in the top-level repository's first commit to preserve
them permanently.

The inline credential formerly present in the top-level `origin` URL was
removed, and the URL was normalized to the public HTTPS address
`https://github.com/2daeeun/ExtFUSE_Code.git`.
