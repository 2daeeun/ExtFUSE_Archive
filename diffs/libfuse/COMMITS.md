# libfuse Original Commit URLs and Change History

**English** | [한국어](COMMITS.ko.md)

The exact baseline immediately before the ExtFUSE changes is
[386b1b6e3d0f](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624); its complete source is available
as a [snapshot](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624).

The final archived ExtFUSE commit is [5b381d81f0d2](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d).
The complete range after the baseline is fixed in the
[GitHub comparison](https://github.com/extfuse/libfuse/compare/386b1b6e3d0fcf7e2dfd5473e867284410dfa624..5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d).

Each commit link points to the original `extfuse/libfuse` GitHub repository.
`snapshot` is the complete source at that commit; `text` is the readable
commit body; `exact-b64` is a byte-for-byte Base64 copy of the original
commit object, including its final newline and any GPG signature.

| # | Role | Original commit | Snapshot | Parent commits | Authored at | Change size | Signature | Subject | Object records |
|---:|---|---|---|---|---|---|---|---|---|
| 0 | Baseline | [386b1b6e3d0f](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [code](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [4ae8680](https://github.com/extfuse/libfuse/commit/4ae8680e40b60d65029f4630c21ab182d3c79e8e), [a5a00e9](https://github.com/extfuse/libfuse/commit/a5a00e9b7dd8c8dfef17523dccb3051e1f1dd5a2) | 2015-09-29T17:51:32+02:00 | — | no | Merge branch 'clone_fd' | [text](raw-commits/386b1b6e3d0fcf7e2dfd5473e867284410dfa624.commit), [exact-b64](raw-commits/386b1b6e3d0fcf7e2dfd5473e867284410dfa624.commit.b64) |
| 1 | ExtFUSE change | [7c88efb9d92d](https://github.com/extfuse/libfuse/commit/7c88efb9d92d64289795e43b2948977c07ccda7f) | [code](https://github.com/extfuse/libfuse/tree/7c88efb9d92d64289795e43b2948977c07ccda7f) | [386b1b6](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | 2019-02-28T20:44:38-05:00 | 3 files changed, 24 insertions(+), 2 deletions(-) | no | Add support for detecting/enabling ExtFUSE | [text](raw-commits/7c88efb9d92d64289795e43b2948977c07ccda7f.commit), [exact-b64](raw-commits/7c88efb9d92d64289795e43b2948977c07ccda7f.commit.b64) |
| 2 | ExtFUSE change | [f945fbc0b6ba](https://github.com/extfuse/libfuse/commit/f945fbc0b6ba9c9291229f6e209d82168908cc94) | [code](https://github.com/extfuse/libfuse/tree/f945fbc0b6ba9c9291229f6e209d82168908cc94) | [7c88efb](https://github.com/extfuse/libfuse/commit/7c88efb9d92d64289795e43b2948977c07ccda7f) | 2020-07-11T19:00:54-05:00 | 2 files changed, 4 insertions(+), 2 deletions(-) | no | Use correct FUSE Capability bits and init reply flags for ExtFUSE | [text](raw-commits/f945fbc0b6ba9c9291229f6e209d82168908cc94.commit), [exact-b64](raw-commits/f945fbc0b6ba9c9291229f6e209d82168908cc94.commit.b64) |
| 3 | ExtFUSE change | [5b381d81f0d2](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | [code](https://github.com/extfuse/libfuse/tree/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | [f945fbc](https://github.com/extfuse/libfuse/commit/f945fbc0b6ba9c9291229f6e209d82168908cc94) | 2020-07-25T11:42:57-05:00 | 1 file changed, 2 insertions(+), 1 deletion(-) | no | Synchronize fuse_init_out with current kernel master | [text](raw-commits/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d.commit), [exact-b64](raw-commits/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d.commit.b64) |

## Machine-readable records

- [commits.tsv](commits.tsv): commit/tree/parent IDs, all original URLs, and author/committer information
- [raw-commits/](raw-commits): readable commit bodies (`.commit`) and exact Base64 objects (`.commit.b64`)
- Verify an original commit: `base64 --decode raw-commits/<SHA>.commit.b64 | git hash-object -t commit --stdin`
