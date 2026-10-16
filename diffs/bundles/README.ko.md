# 보존된 Git bundle

[English](README.md) | **한국어**

이 디렉터리의 bundle은 하위 `.git`을 삭제한 뒤에도 원래 Git 객체와
refs를 복원하기 위한 보조 아카이브다.

## 포함 파일

- `extfuse.bundle`: `extfuse`의 29개 commit 전체와 모든 refs를 담은
  완전한 bundle이다.
- `libfuse.bundle`: `libfuse`의 1,799개 commit, 114개 tag와 두 checkout의
  합집합인 130개 named ref를 담은 완전한 bundle이다. bundle의 `HEAD`는
  `ExtFUSE-1.0`이다.
- `linux-extfuse-changes.bundle`: Linux 원본 기준점 이후의 ExtFUSE 변경
  9 commit, 38 tree, 23 blob을 원래 Git object로 보존한다. 12,647-byte
  변경 전용 bundle이며 기준 commit `1d039859...`가 prerequisite다.

extfuse와 libfuse bundle은 `git bundle verify`에서 “complete history”로
검증되었다. Linux 변경 bundle도 기준 commit이 있는 저장소에서 검증과
fetch를 통과했고, 복원된 branch는 9개 commit과 원래 HEAD tree
`0b6b2080e7922a8301df156cd947aeddffd49145`를 정확히 재현했다.

## 복원

저장소 최상위에서 다음처럼 복원할 수 있다.

```bash
git clone diffs/bundles/extfuse.bundle restored-extfuse
git clone diffs/bundles/libfuse.bundle restored-libfuse
```

복원 후 원래 공개 remote를 다시 지정하려면 다음 URL을 사용한다.

```text
https://github.com/extfuse/extfuse.git
https://github.com/extfuse/libfuse.git
```

### Linux 변경 이력의 오프라인 복원

Linux bundle은 독립 clone용 완전 bundle이 아니라 base prerequisite가
있는 증분 bundle이다. 하위 `.git` 삭제 후에도 보존된 원본 source tree와
raw base commit으로 prerequisite를 만들 수 있다. 저장소 최상위에서 다음
순서로 복원한다. `git add -f`는 원본 Git에서 추적했지만 ignore 규칙에
걸리는 빈 파일까지 포함해 정확한 base tree를 만드는 데 필요하다.

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

위 절차를 실제 별도 object database에서 실행해 connectivity 검사
(`git fsck --connectivity-only --no-dangling`), 9개 변경 commit 수, 최종
tree를 모두 검증했다. 공개 원격을 사용할 수 있으면 `extfuse/linux`
clone에 이 bundle을 fetch하는 방식으로도 prerequisite를 만족할 수 있다.

### Linux 변경 bundle 생성 명령

삭제 전 원래 저장소에서 다음 명령으로 생성했다.

```bash
git -C linux_ExtFUSE-1.0/linux bundle create \
  "$PWD/diffs/bundles/linux-extfuse-changes.bundle" \
  ExtFUSE-1.0 \
  ^1d039859330b874d48080885eb31f4f129c246f1

git -C linux_ExtFUSE-1.0/linux bundle verify \
  "$PWD/diffs/bundles/linux-extfuse-changes.bundle"
```

bundle SHA-256은
`0f9907267abb2b48f8682da5bdf2963f32e2d88df59d598555d47d430d6a5cef`다.

## Linux 전체 bundle을 두지 않은 이유

삭제 전 두 Linux clone에는 6,815,470개 객체가 든 동일한
2,754,194,655-byte pack이 각각 있었다. 전체 이력은 850,176개 commit이며
`.git` 한 벌이 약 2.9 GB였다. 사용자가 지정한 대로 ExtFUSE 변경 이후
기록에 집중하기 위해 이 거대한 upstream 전체 객체를 다시 복제하지 않았다.
대신 위 변경 전용 bundle에 `BASE..HEAD` 범위의 commit/tree/blob object를
모두 넣었다.

Linux 분석에 필요한 다음 자료를 저장소 자체에 보존한다.

- 변경 직전 원본과 최종 ExtFUSE source tree 두 벌
- 원본 기준 commit과 이후 9개 commit의 원본 GitHub URL
- 이후 9개 commit의 정확한 commit/tree/blob Git object bundle
- commit/tree/parent ID, 작성자·커미터, GPG signature를 포함한 정확한
  raw commit object
- 커밋별 mailbox, 누적 full-index patch, 파일별 통계와 commit mapping
- refs/tag/reflog/config/object inventory

따라서 ExtFUSE 변경 코드를 분석하고 각 중간 상태를 재구성하는 데는
Linux 전체 upstream object database가 필요하지 않다. 원본 기준점 이전의
전체 Linux upstream bundle은 현재 아카이브에 포함되어 있지 않다. 그
범위까지 다시 탐색하려면 공개 원격에서 이력을 가져와야 한다.

GitHub는 일반 Git object 하나의 크기를 100 MiB로 제한하고 push 한도를
2 GB로 적용하므로, 약 2.9 GB인 Linux 전체 bundle은 이 통합 저장소에
일반 파일 하나로 넣기에도 적합하지 않다.

- GitHub 공식 문서: <https://docs.github.com/en/repositories/creating-and-managing-repositories/repository-limits>
