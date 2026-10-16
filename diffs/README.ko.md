# ExtFUSE 변경 이력 보존 자료

[English](README.md) | **한국어**

이 디렉터리는 다섯 하위 저장소의 `.git`을 삭제한 뒤에도
`ExtFUSE_Code`만으로 원본과 ExtFUSE 변경을 분석할 수 있도록 만든
아카이브다. 생성 기준일은 2026-07-14이다.

같은 날 다섯 하위 `.git`의 제거가 완료되었다. 남은 source tree와 이
아카이브의 삭제 후 무결성 검사는
[`POST_DELETE_VERIFICATION.md`](POST_DELETE_VERIFICATION.ko.md)에 기록했다.

보존 범위는 사용자의 요청대로 **변경 직전 원본 commit과 그 이후
ExtFUSE 관련 commit**에 집중한다. Linux의 85만여 upstream commit 전체를
복제하는 대신 원본/수정 source tree, 정확한 변경 patch, commit object와
원본 URL을 함께 남긴다.

## 변경 시작점과 최종 시점

| 대상 | 변경 직전 원본 또는 최초 시점 | 최종 시점 | 전체 비교 |
|---|---|---|---|
| Linux | [`1d039859330b`](https://github.com/extfuse/linux/commit/1d039859330b874d48080885eb31f4f129c246f1) · [원본 코드](https://github.com/extfuse/linux/tree/1d039859330b874d48080885eb31f4f129c246f1) | [`d95ca8642fce`](https://github.com/extfuse/linux/commit/d95ca8642fce0214cf2ebaaa792ba538d4d8381d) | [compare](https://github.com/extfuse/linux/compare/1d039859330b874d48080885eb31f4f129c246f1..d95ca8642fce0214cf2ebaaa792ba538d4d8381d) |
| libfuse | [`386b1b6e3d0f`](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) · [원본 코드](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [`5b381d81f0d2`](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | [compare](https://github.com/extfuse/libfuse/compare/386b1b6e3d0fcf7e2dfd5473e867284410dfa624..5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) |
| extfuse | 별도 vanilla 원본 없음 · [최초 `5462699b5414`](https://github.com/extfuse/extfuse/commit/5462699b5414a739bc6c3221157827ac42250334) · [최초 코드](https://github.com/extfuse/extfuse/tree/5462699b5414a739bc6c3221157827ac42250334) | [`d2acdb1c8e45`](https://github.com/extfuse/extfuse/commit/d2acdb1c8e4531562fc77fe566896e771d03026e) | [최초→현재](https://github.com/extfuse/extfuse/compare/5462699b5414a739bc6c3221157827ac42250334..d2acdb1c8e4531562fc77fe566896e771d03026e) |

Linux와 libfuse의 원본 commit은 각 ExtFUSE branch의 정확한 merge-base다.
`extfuse`는 새로 작성된 library/BPF 저장소이므로 최초 commit부터 전체
29개를 기록한다.

## commit URL과 원본 object

| 대상 | 사람이 읽는 commit URL 목록 | 기계 판독 metadata | 정확한 raw object |
|---|---|---|---|
| Linux | [`linux/COMMITS.md`](linux/COMMITS.ko.md) | [`linux/commits.tsv`](linux/commits.tsv) | [`linux/raw-commits/`](linux/raw-commits) |
| libfuse | [`libfuse/COMMITS.md`](libfuse/COMMITS.ko.md) | [`libfuse/commits.tsv`](libfuse/commits.tsv) | [`libfuse/raw-commits/`](libfuse/raw-commits) |
| extfuse | [`extfuse/COMMITS.md`](extfuse/COMMITS.ko.md) | [`extfuse/commits.tsv`](extfuse/commits.tsv) | [`extfuse/raw-commits/`](extfuse/raw-commits) |

`COMMITS.md`의 모든 commit SHA는 원래 GitHub commit URL과 연결된다.
`commits.tsv`에는 다음을 함께 기록한다.

- commit/tree/parent의 full SHA
- 원본 commit URL과 해당 시점 전체 source snapshot URL
- raw tree API URL
- 각 parent URL과 parent→commit compare URL
- 원본 시작점→해당 commit compare URL
- author/committer 이름·이메일·ISO 8601 시각
- GPG signature header 존재 여부, 제목과 raw object 경로

사람이 읽는 `.commit` 파일과 별도로 `.commit.b64`가 원본 commit
object를 마지막 byte까지 보존한다. 43개 Base64 object는 다음 명령으로
decode한 SHA가 파일명의 원본 SHA와 모두 일치함을 확인했다.

```bash
base64 --decode diffs/linux/raw-commits/<SHA>.commit.b64 |
  git hash-object -t commit --stdin
```

## 코드 변경 자료

| 대상 | 누적 full-index patch | 커밋별 mailbox | 파일→commit URL | 그래프 | 통계 |
|---|---|---|---|---|---|
| Linux | [`linux/changes.patch`](linux/changes.patch) | [`linux/series.mbox`](linux/series.mbox) | [`linux/files.tsv`](linux/files.tsv) | [`linux/history.graph.txt`](linux/history.graph.txt) | [diffstat](linux/diffstat.txt), [name-status](linux/name-status.tsv), [numstat](linux/numstat.tsv) |
| libfuse | [`libfuse/changes.patch`](libfuse/changes.patch) | [`libfuse/series.mbox`](libfuse/series.mbox) | [`libfuse/files.tsv`](libfuse/files.tsv) | [`libfuse/history.graph.txt`](libfuse/history.graph.txt) | [diffstat](libfuse/diffstat.txt), [name-status](libfuse/name-status.tsv), [numstat](libfuse/numstat.tsv) |
| extfuse | [`extfuse/changes.patch`](extfuse/changes.patch) | [`extfuse/series.mbox`](extfuse/series.mbox) | [`extfuse/files.tsv`](extfuse/files.tsv) | [`extfuse/history.graph.txt`](extfuse/history.graph.txt) | [diffstat](extfuse/diffstat.txt), [name-status](extfuse/name-status.tsv), [numstat](extfuse/numstat.tsv) |

- `changes.patch`: `git diff --binary --full-index` 누적 결과다.
- `series.mbox`: 비-merge commit별 작성자, 날짜, message와 patch다.
- `files.tsv`: 각 변경 파일과 그 파일을 건드린 commit SHA/원본 URL의
  역색인이다.
- `history.graph.txt`: merge topology, full commit/tree/parent ID와
  author/committer 정보를 보존한다.

누적 통계는 Linux 16 files(+508/-4), libfuse 3 files(+27/-2),
extfuse 현재 tree 11 files(+1,710)다.

### merge 기록

Linux merge `2473946e1ce9...`는 merge tree가 second-parent tree와 같아
별도 conflict-resolution 코드는 없다. 원래 두 parent와 raw signed
commit object는 `linux/commits.tsv`와 `raw-commits`에 있다.

extfuse merge `b853870db92b...`는 두 parent를 결합하면서 최종 tree에
영향을 준다. 다음 세 관점을 모두 보존했다.

- [first-parent diff](extfuse/merges/b853870.first-parent.patch)
- [second-parent diff](extfuse/merges/b853870.second-parent.patch)
- [combined metadata/diff](extfuse/merges/b853870.combined.diff)

일반 `format-patch`는 merge commit 자체는 생략하지만, 이 이력에서는 양쪽
branch의 비-merge commit 28개를 `series.mbox` 순서대로 empty history에
적용하면 정확한 HEAD tree `d59ddb65225c9b7dc492939e74d229d21640a09c`가
재구성된다. merge object와 두-parent topology는 `raw-commits`,
`history.graph.txt`, 위 세 merge diff에 별도로 보존했다. 누적
`extfuse/changes.patch`도 처음부터 정확한 최종 tree를 만든다.

## Git metadata와 bundle

- [`git-metadata/`](git-metadata/README.ko.md): 삭제 전 다섯 checkout의 HEAD,
  branch/detached 상태, refs/tags, reflog, sanitized config, status,
  object 통계, hook/worktree/special-ref 상태
- [`git-metadata/object-integrity.txt`](git-metadata/object-integrity.txt):
  pack 크기·SHA-256, clone 쌍의 동일성, connectivity 검사
- [`bundles/extfuse.bundle`](bundles/extfuse.bundle): extfuse 29개 commit
  전체의 완전한 Git bundle
- [`bundles/libfuse.bundle`](bundles/libfuse.bundle): libfuse 1,799개
  commit과 114개 tag 전체의 완전한 Git bundle
- [`bundles/linux-extfuse-changes.bundle`](bundles/linux-extfuse-changes.bundle):
  원본 기준점 이후 Linux의 9 commit, 38 tree, 23 blob을 보존한 변경 전용
  bundle; 기준 commit을 prerequisite로 요구한다.
- [bundle 복원 및 Linux 전체 이력 제외 근거](bundles/README.ko.md)

삭제 전 Linux 두 clone의 `.git`은 각각 약 2.9 GB였고 동일한 850,176개
upstream commit을 담고 있었다. 변경 이후 기록에 집중한다는 요청에 따라
2.9 GB짜리 전체 bundle을 다시 복사하지 않았다. 대신 원본/최종 source
tree, 변경 구간의 commit/tree/blob bundle, raw
commit·patch·URL·refs metadata를 보존했다.

## 빠른 확인

저장소 최상위에서 실행한다.

```bash
# 원본 URL과 commit별 변경
less diffs/linux/COMMITS.md
less diffs/libfuse/COMMITS.md
less diffs/extfuse/COMMITS.md

# 누적/파일별 변경
git apply --stat diffs/linux/changes.patch
git apply --numstat diffs/libfuse/changes.patch
rg '^diff --git' diffs/extfuse/changes.patch

# 전체 무결성
sha256sum -c diffs/SHA256SUMS
```

## patch 검증

다음 명령은 파일을 수정하지 않는다.

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

네 검사는 모두 종료 코드 0으로 통과했다. Linux와 libfuse
`series.mbox`를 각각 base tree 위에 적용해 만든 tree ID도 원래
ExtFUSE HEAD tree와 일치했다.

extfuse 누적 patch는 empty tree 위에 적용했을 때 현재 HEAD tree와
일치했다. `series.mbox`의 28개 비-merge patch만 empty history에 적용한
tree도 같은 값으로 일치했다.

```text
empty tree:   4b825dc642cb6eb9a060e54bf8d69288fbee4904
extfuse HEAD: d59ddb65225c9b7dc492939e74d229d21640a09c
```

## 생성 명령

Linux와 libfuse 누적 patch의 기본 형식은 다음과 같다.

```bash
git -C REPOSITORY diff \
  --no-color --no-ext-diff --no-textconv \
  --binary --full-index --find-renames=50% \
  BASE..HEAD > changes.patch
```

커밋별 mailbox는 다음 형식으로 생성했다.

```bash
git -C REPOSITORY format-patch \
  --stdout --topo-order --no-merges --no-signature \
  --binary --full-index --no-ext-diff --no-textconv \
  --find-renames=50% BASE..HEAD > series.mbox
```

정확한 commit object는 `git cat-file commit <SHA>` 결과를 Base64로
인코딩해 보존했다.

## 무결성과 한계

[`SHA256SUMS`](SHA256SUMS)는 `diffs/` 아래 모든 보존 파일(bundle과
문서 포함)의 SHA-256을 기록한다. 이 파일 자체는 checksum 대상에서
제외한다.

이 자료만으로 ExtFUSE 변경 전/후 코드, 모든 변경 commit의 내용·서명,
중간 patch, 파일별 이력과 원본 웹 URL을 분석할 수 있다. 다만 Linux
원본 기준점보다 오래된 85만여 upstream 역사를 완전히 오프라인 탐색하는
용도는 아니며, 그 범위는 공개 원격 URL을 사용한다.

### 문서 언어 변경과 체크섬

영어·한글 문서 제공을 위해 번역 전 체크섬 목록을 [`SHA256SUMS.archive`](SHA256SUMS.archive)에 바이트 그대로 보존했다. 이는 번역 전 문서의 해시를 담은 역사적 기록이다. 현재 파일 확인에는 [`SHA256SUMS`](SHA256SUMS)를 사용한다. 기존 마크다운 7개 항목의 해시를 갱신하고, `diffs/` 아래 한글판 7개와 원래 체크섬 목록을 추가했다. 기존 소스·patch·raw object·bundle 등 문서 외 자료의 경로와 해시는 그대로다. 루트 README와 `AGENTS.md`는 이 목록의 범위 밖이다.
