# 하위 `.git` 삭제 후 검증 기록

[English](POST_DELETE_VERIFICATION.md) | **한국어**

> 이 문서의 저장소 상태와 132개 파일 검증 결과는 2026-07-14 당시의 기록이다.
> 당시 체크섬 목록은 [`SHA256SUMS.archive`](SHA256SUMS.archive)에 보존했으며,
> 현재 [`SHA256SUMS`](SHA256SUMS)는 번역 문서와 한글판도 포함한다.
> 이번 문서 변경은 아래 patch·bundle·source tree 검증을 새로 실행했다는 뜻이 아니다.

검증일은 2026-07-14(Asia/Seoul)이다. 사용자가 다섯 하위 `.git`
디렉터리를 삭제한 뒤, 현재 source와 삭제 전에 만든 보존 자료가 그대로
분석·복원 가능한지 다시 확인했다.

## 삭제 상태

`find . -type d -name .git` 결과는 최상위 `./.git` 하나뿐이다. 삭제된
하위 Git metadata 경로는 다음과 같다.

```text
extfuse/.git
linux_origin/linux/.git
linux_ExtFUSE-1.0/linux/.git
libfuse_origin/libfuse/.git
libfuse_ExtFUSE-1.0/libfuse/.git
```

다섯 source 디렉터리는 모두 남아 있으며 이제 최상위 저장소가 일반
파일로 추적할 수 있는 상태다.

## source tree 일치

각 source 디렉터리를 외부 임시 Git object database에 `git add -f -A`로
읽어 파일 내용, 실행 bit와 symlink를 포함한 tree ID를 계산했다. 삭제 전
기록한 원래 Git tree와 모두 정확히 일치했다.

| source | 삭제 전 commit | 검증된 tree |
|---|---|---|
| `linux_origin/linux` | `1d039859330b874d48080885eb31f4f129c246f1` | `5988dc49e1534ec6e1fadca6c5b5231248a63b41` |
| `linux_ExtFUSE-1.0/linux` | `d95ca8642fce0214cf2ebaaa792ba538d4d8381d` | `0b6b2080e7922a8301df156cd947aeddffd49145` |
| `libfuse_origin/libfuse` | `386b1b6e3d0fcf7e2dfd5473e867284410dfa624` | `d16206c9de6043e78f663b8cfadc835df2981f1e` |
| `libfuse_ExtFUSE-1.0/libfuse` | `5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d` | `500a56d0fd38f2ed1d8c82271d54abbaf1777499` |
| `extfuse` | `d2acdb1c8e4531562fc77fe566896e771d03026e` | `d59ddb65225c9b7dc492939e74d229d21640a09c` |

## 보존 자료 검증

- `diffs/SHA256SUMS`: 이 문서를 포함한 최종 보존 파일 132개 모두 `OK`
- Linux/libfuse 누적 patch: 원본에 정방향, ExtFUSE source에 역방향
  `git apply --check` 네 건 모두 종료 코드 0
- `extfuse.bundle`, `libfuse.bundle`: 삭제 후에도 complete history 검증과
  bare clone 성공
- `linux-extfuse-changes.bundle`: 보존된 Linux 원본 source로 base tree와
  raw base commit을 재구성한 뒤 fetch 성공
- 복원된 Linux 변경 이력: 9 commits, 최종 tree `0b6b2080...`,
  `git fsck --connectivity-only --no-dangling` 종료 코드 0

Linux 기준점보다 앞선 전체 upstream object database는 변경 중심 보존
범위에 포함되지 않는다. 그 밖의 ExtFUSE 변경 구간은 source, patch, raw
commit, URL과 bundle로 분석·재구성할 수 있다.

## 최상위 저장소 상태

최상위 `ExtFUSE_Code/.git`은 유지되었지만 아직 commit이 없는 상태이며,
README, `diffs/`와 다섯 source 디렉터리는 untracked다. `origin/main`도
현재 `[gone]`으로 표시된다. 보존 자료를 영구히 남기려면 이 파일들을
최상위 저장소의 첫 commit에 포함해야 한다.

최상위 `origin` URL에 들어 있던 inline credential은 제거하고 공개 HTTPS
URL `https://github.com/2daeeun/ExtFUSE_Code.git`로 정리했다.
