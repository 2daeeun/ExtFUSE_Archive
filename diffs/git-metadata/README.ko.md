# 삭제 전 Git metadata inventory

[English](README.md) | **한국어**

이 디렉터리는 다섯 하위 `.git` 디렉터리를 삭제하기 직전의 저장소 상태를
텍스트로 기록한다. 비밀 정보는 포함하지 않았으며 remote URL은 공개
GitHub 주소로 정규화했다.

분석 재현성을 위해 원래 worktree 절대 경로와 Git에 기록된
author/committer/reflog 이름·이메일은 유지했다. 인증정보는 아니지만,
공개 배포 시 개인정보로 볼 수 있으므로 필요하면 별도로 검토한다.

## 파일

- `*.state.txt`: 실제 checkout별 HEAD, branch/detached 상태, upstream,
  describe, status porcelain v2, object 통계, branch/refs, reflog, worktree,
  sanitized local config, hook 목록·checksum과 특수 기능 사용 여부
- `linux.refs.tsv`: Linux의 616개 local/remote/tag ref와 원본 URL
- `libfuse.refs.tsv`: 두 checkout을 합친 libfuse의 130개 ref와 원본 URL
- `extfuse.refs.tsv`: extfuse의 3개 ref와 원본 URL
- `object-integrity.txt`: object pack 크기·SHA-256, 중복 여부와 fsck 결과

Linux와 libfuse의 `*.refs.tsv`는 원본/ExtFUSE 두 checkout의 ref 합집합을
기록하므로, 한쪽 checkout에만 있던 local branch도 빠지지 않는다.

## 실제 checkout 상태

| state 파일 | 원래 경로 | checkout |
|---|---|---|
| `extfuse.state.txt` | `extfuse` | `master` @ `d2acdb1c8e45...` |
| `linux_origin.state.txt` | `linux_origin/linux` | `master` @ `1d039859330b...` |
| `linux_extfuse.state.txt` | `linux_ExtFUSE-1.0/linux` | `ExtFUSE-1.0` @ `d95ca8642fce...` |
| `libfuse_origin.state.txt` | `libfuse_origin/libfuse` | detached @ `386b1b6e3d0f...` |
| `libfuse_extfuse.state.txt` | `libfuse_ExtFUSE-1.0/libfuse` | `ExtFUSE-1.0` @ `5b381d81f0d2...` |

조사 당시 다섯 index/worktree는 모두 clean이었다. shallow clone, partial
clone, alternates, 추가 worktree, custom hook, Git notes, replace ref, stash,
submodule/gitlink, Git LFS 파일은 없었다. hook 파일은 Git이 생성한 기본
`*.sample` 14개뿐이다.

## 원본 remote

```text
https://github.com/extfuse/linux
https://github.com/extfuse/libfuse.git
https://github.com/extfuse/extfuse.git
```

각 ref의 사람용 GitHub URL과 commit target URL은 `*.refs.tsv`에 있다.
ExtFUSE 변경에 직접 관계된 모든 commit URL은 각 구성 요소의
`COMMITS.md`와 `commits.tsv`에 더 자세히 기록했다.
