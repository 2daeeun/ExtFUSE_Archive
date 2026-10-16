# libfuse 원본 commit URL과 변경 이력

[English](COMMITS.md) | **한국어**

ExtFUSE 변경 직전의 정확한 원본 기준점은 [386b1b6e3d0f](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624)이며, 당시 전체 코드는 [snapshot](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624)에서 볼 수 있다.

최종 ExtFUSE 시점은 [5b381d81f0d2](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d)이고, 원본 이후 전체 범위는 [GitHub compare](https://github.com/extfuse/libfuse/compare/386b1b6e3d0fcf7e2dfd5473e867284410dfa624..5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d)에 고정했다.

각 commit 링크는 원래 `extfuse/libfuse` GitHub 저장소를 가리킨다. `snapshot`은 그 commit의 전체 코드이며, `text`는 사람이 읽는 commit 본문이고, `exact-b64`는 마지막 개행과 GPG 서명까지 원래 commit object를 byte-for-byte 보존한 Base64 사본이다.

| # | 역할 | 원본 commit | snapshot | 부모 commit | 작성 시각 | 변경량 | 서명 | 제목 | object records |
|---:|---|---|---|---|---|---|---|---|---|
| 0 | 원본 기준 | [386b1b6e3d0f](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [code](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [4ae8680](https://github.com/extfuse/libfuse/commit/4ae8680e40b60d65029f4630c21ab182d3c79e8e), [a5a00e9](https://github.com/extfuse/libfuse/commit/a5a00e9b7dd8c8dfef17523dccb3051e1f1dd5a2) | 2015-09-29T17:51:32+02:00 | — | no | Merge branch 'clone_fd' | [text](raw-commits/386b1b6e3d0fcf7e2dfd5473e867284410dfa624.commit), [exact-b64](raw-commits/386b1b6e3d0fcf7e2dfd5473e867284410dfa624.commit.b64) |
| 1 | ExtFUSE 변경 | [7c88efb9d92d](https://github.com/extfuse/libfuse/commit/7c88efb9d92d64289795e43b2948977c07ccda7f) | [code](https://github.com/extfuse/libfuse/tree/7c88efb9d92d64289795e43b2948977c07ccda7f) | [386b1b6](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | 2019-02-28T20:44:38-05:00 | 3 files changed, 24 insertions(+), 2 deletions(-) | no | Add support for detecting/enabling ExtFUSE | [text](raw-commits/7c88efb9d92d64289795e43b2948977c07ccda7f.commit), [exact-b64](raw-commits/7c88efb9d92d64289795e43b2948977c07ccda7f.commit.b64) |
| 2 | ExtFUSE 변경 | [f945fbc0b6ba](https://github.com/extfuse/libfuse/commit/f945fbc0b6ba9c9291229f6e209d82168908cc94) | [code](https://github.com/extfuse/libfuse/tree/f945fbc0b6ba9c9291229f6e209d82168908cc94) | [7c88efb](https://github.com/extfuse/libfuse/commit/7c88efb9d92d64289795e43b2948977c07ccda7f) | 2020-07-11T19:00:54-05:00 | 2 files changed, 4 insertions(+), 2 deletions(-) | no | Use correct FUSE Capability bits and init reply flags for ExtFUSE | [text](raw-commits/f945fbc0b6ba9c9291229f6e209d82168908cc94.commit), [exact-b64](raw-commits/f945fbc0b6ba9c9291229f6e209d82168908cc94.commit.b64) |
| 3 | ExtFUSE 변경 | [5b381d81f0d2](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | [code](https://github.com/extfuse/libfuse/tree/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | [f945fbc](https://github.com/extfuse/libfuse/commit/f945fbc0b6ba9c9291229f6e209d82168908cc94) | 2020-07-25T11:42:57-05:00 | 1 file changed, 2 insertions(+), 1 deletion(-) | no | Synchronize fuse_init_out with current kernel master | [text](raw-commits/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d.commit), [exact-b64](raw-commits/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d.commit.b64) |

## 기계 판독 자료

- [commits.tsv](commits.tsv): commit/tree/parent ID, 모든 원본 URL, 작성자·커미터 정보
- [raw-commits/](raw-commits): 사람이 읽는 commit 본문(`.commit`)과 정확한 Base64 object(`.commit.b64`)
- 원본 commit 검증: `base64 --decode raw-commits/<SHA>.commit.b64 | git hash-object -t commit --stdin`
