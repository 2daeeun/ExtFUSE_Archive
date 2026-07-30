# ExtFUSE ATC'19 코드 아카이브

이 저장소는 논문 **“Extension Framework for File Systems in User space”**
(USENIX ATC '19)의 ExtFUSE 관련 코드를 한곳에서 비교·분석하기 위한
아카이브다.

- 논문: [USENIX 발표 페이지](https://www.usenix.org/conference/atc19/presentation/bijlani), [PDF](https://www.usenix.org/system/files/atc19-bijlani.pdf)
- 원본 GitHub 조직: [github.com/extfuse](https://github.com/extfuse)
- 분석 기준일: **2026-07-14**

이 문서의 Git 정보와 통계는 각 하위 저장소의 `.git`을 제거하기 전에
직접 `git log`, `git merge-base`, `git diff`, 실제 파일 비교로 확인한
결과다. 조사 당시 다섯 하위 작업 트리는 모두 clean 상태였고, 추적되지
않은 파일이나 빌드 산출물도 없었다.

> **삭제 후 상태:** 2026-07-14에 다섯 하위 `.git`이 모두 제거되었고
> 현재는 최상위 `ExtFUSE_Code/.git`만 남아 있다. 남은 다섯 source가 삭제
> 전 Git tree와 정확히 일치하고 patch·bundle·checksum이 유효함을
> [삭제 후 검증 기록](diffs/POST_DELETE_VERIFICATION.md)으로 다시 확인했다.

> 여기서 `origin` 디렉터리는 “현재 최신 upstream”이 아니라
> **ExtFUSE 브랜치가 갈라진 정확한 기준 스냅샷**을 뜻한다. 특히
> libfuse 분석에서 이 구분이 중요하다.

## 디렉터리 구성

| 디렉터리 | 역할 |
|---|---|
| [`pdf`](pdf/) | ExtFUSE 논문과 관련 발표자료 |
| [`linux_origin/linux`](linux_origin/linux) | ExtFUSE 패치 전 Linux 기준 스냅샷 |
| [`linux_ExtFUSE-1.0/linux`](linux_ExtFUSE-1.0/linux) | ExtFUSE 커널 fast path와 전용 BPF ABI가 추가된 Linux |
| [`libfuse_origin/libfuse`](libfuse_origin/libfuse) | ExtFUSE용 libfuse가 분기한 기준 스냅샷 |
| [`libfuse_ExtFUSE-1.0/libfuse`](libfuse_ExtFUSE-1.0/libfuse) | ExtFUSE capability와 BPF 프로그램 FD 전달 기능이 추가된 libfuse |
| [`extfuse`](extfuse) | BPF 정책 프로그램과 이를 적재·관리하는 `libextfuse.so` 소스 |
| [`diffs`](diffs) | 하위 `.git` 삭제 전에 생성한 세 코드 계열의 patch, commit URL·raw object, Git metadata와 복원 bundle |

`linux_*`와 `libfuse_*`는 각각 같은 원격 저장소의 기준점/수정본 쌍이다.
반면 `extfuse/`는 기존 프로젝트를 수정한 포크가 아니라 ExtFUSE를 위해
새로 작성된 지원 코드이므로 대응하는 `extfuse_origin/`이 없다.

## 보존된 Git 기준점

| 디렉터리 | 삭제 전 checkout | 삭제 전 HEAD | 설명 |
|---|---|---|---|
| `linux_origin/linux` | `master` | [`1d039859330b`](https://github.com/extfuse/linux/commit/1d039859330b874d48080885eb31f4f129c246f1) | 2019-07-14, `v5.2-8274-g1d039859330b` |
| `linux_ExtFUSE-1.0/linux` | `ExtFUSE-1.0` | [`d95ca8642fce`](https://github.com/extfuse/linux/commit/d95ca8642fce0214cf2ebaaa792ba538d4d8381d) | 2020-09-08, `v5.2-8283-gd95ca8642fce` |
| `libfuse_origin/libfuse` | detached HEAD | [`386b1b6e3d0f`](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | 2015-09-29, `fuse_2_9_3-105-g386b1b6` |
| `libfuse_ExtFUSE-1.0/libfuse` | `ExtFUSE-1.0` | [`5b381d81f0d2`](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | 2020-07-25, `fuse_2_9_3-108-g5b381d8` |
| `extfuse` | `master` | [`d2acdb1c8e45`](https://github.com/extfuse/extfuse/commit/d2acdb1c8e4531562fc77fe566896e771d03026e) | 2023-05-02, 현재 아카이브의 standalone 코드 |

원격 저장소는 각각 다음과 같다.

- Linux: <https://github.com/extfuse/linux>
- libfuse: <https://github.com/extfuse/libfuse>
- ExtFUSE library/BPF: <https://github.com/extfuse/extfuse>

## 보존된 Git diff와 commit 원본 URL

하위 `.git`을 삭제한 뒤에도 변경 시작점과 이후 이력을 모두 추적할 수
있도록 [`diffs/`](diffs)에 source patch, commit metadata와 원본 URL을
보존했다.

| 대상 | 변경 시작점 | 모든 commit 원본 URL | 누적 patch | 커밋별/파일별 기록 |
|---|---|---|---|---|
| Linux | [원본 `1d039859330b`](https://github.com/extfuse/linux/commit/1d039859330b874d48080885eb31f4f129c246f1) · [당시 코드](https://github.com/extfuse/linux/tree/1d039859330b874d48080885eb31f4f129c246f1) | [10개 행: 원본+변경 9개](diffs/linux/COMMITS.md) | [`changes.patch`](diffs/linux/changes.patch) | [mailbox](diffs/linux/series.mbox), [파일→commit URL](diffs/linux/files.tsv), [그래프](diffs/linux/history.graph.txt) |
| libfuse | [원본 `386b1b6e3d0f`](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) · [당시 코드](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [4개 행: 원본+변경 3개](diffs/libfuse/COMMITS.md) | [`changes.patch`](diffs/libfuse/changes.patch) | [mailbox](diffs/libfuse/series.mbox), [파일→commit URL](diffs/libfuse/files.tsv), [그래프](diffs/libfuse/history.graph.txt) |
| extfuse | 별도 vanilla 원본 없음 · [최초 `5462699b5414`](https://github.com/extfuse/extfuse/commit/5462699b5414a739bc6c3221157827ac42250334) · [당시 코드](https://github.com/extfuse/extfuse/tree/5462699b5414a739bc6c3221157827ac42250334) | [전체 29개](diffs/extfuse/COMMITS.md) | [현재 tree patch](diffs/extfuse/changes.patch) | [mailbox](diffs/extfuse/series.mbox), [파일→commit URL](diffs/extfuse/files.tsv), [그래프](diffs/extfuse/history.graph.txt) |

각 `commits.tsv`에는 commit/tree/parent SHA, author/committer, GPG signature
존재 여부뿐 아니라 다음 URL을 기계 판독 가능한 열로 기록했다.

- 원본 GitHub commit와 해당 commit의 전체 source snapshot
- raw tree API
- 각 parent commit 및 parent→commit compare
- 변경 시작점→해당 commit compare

[Linux](diffs/linux/raw-commits), [libfuse](diffs/libfuse/raw-commits),
[extfuse](diffs/extfuse/raw-commits)의 raw commit 디렉터리에는 사람이 읽는
본문과 byte-for-byte 정확한 Base64 object가 있다. Linux 10개, libfuse
4개, extfuse 29개, 총 43개 object를 decode했을 때 원래 commit SHA와
모두 일치한다.

```bash
base64 --decode diffs/linux/raw-commits/<SHA>.commit.b64 |
  git hash-object -t commit --stdin
```

추가로 다음을 보존했다.

- [다섯 checkout의 refs/tag/reflog/config/object inventory](diffs/git-metadata/README.md)
- [extfuse/libfuse 완전 bundle과 Linux 변경 전용 bundle](diffs/bundles/README.md)
- [모든 보존 파일의 SHA-256](diffs/SHA256SUMS)
- extfuse merge의 [양쪽 parent diff와 combined diff](diffs/extfuse/merges)

삭제 전 Linux 전체 upstream object database는 clone마다 약 2.9 GB였고
두 clone의 pack도 동일했다. 변경 이후 기록에 집중하기 위해 전체 이력을
세 번째로 복제하지 않고, 원본/수정 source tree 두 벌과 변경 구간의
URL·raw commit·patch 및 9개 commit/tree/blob을 담은 prerequisite bundle을
보존했다. 상세 범위, 생성·복원·검증 방법은
[`diffs/README.md`](diffs/README.md)에 있다.

```bash
less diffs/linux/COMMITS.md
git apply --stat diffs/linux/changes.patch
git apply --numstat diffs/libfuse/changes.patch
sha256sum -c diffs/SHA256SUMS
```

Linux와 libfuse 누적 patch는 원본에 정방향, 수정본에 역방향
`git apply --check`를 통과했다. 커밋별 patch를 base에 적용한 최종
tree도 원래 ExtFUSE HEAD와 일치하며, extfuse 누적 patch 역시 empty
tree에서 정확한 현재 tree를 재구성한다.

## 전체 연결 구조

세 코드 계열은 다음처럼 함께 동작한다.

```text
FUSE filesystem daemon
  ├─ libextfuse.so가 bpf/extfuse.o를 적재해 BPF program FD 획득
  └─ 수정 libfuse의 init callback에서 capability와 FD를 전달
                         │ FUSE_INIT reply
                         ▼
수정 Linux FUSE driver가 BPF_PROG_TYPE_EXTFUSE 프로그램을 연결
                         │
일반 FUSE 요청 ──> main BPF ──opcode tail call──> 개별 handler
                         │
            ┌────────────┴────────────┐
            │ -ENOSYS                 │ 그 밖의 반환값
            ▼                         ▼
   기존 userspace upcall       커널에서 응답/오류 처리
```

핵심은 libfuse가 BPF 로직을 실행하는 것이 아니라, 파일시스템 daemon이
준비한 **BPF 프로그램 FD를 FUSE 초기 협상에서 커널로 전달**한다는 점이다.
실제 요청 처리와 helper/verifier는 수정 Linux에 있고, 캐시 정책은
`extfuse/bpf/extfuse.c`에 있다.

## Linux: 원본과 ExtFUSE 비교

### 기준 관계와 통계

원본 HEAD가 ExtFUSE 브랜치의 정확한 merge-base다.

```text
base: 1d039859330b874d48080885eb31f4f129c246f1
head: d95ca8642fce0214cf2ebaaa792ba538d4d8381d

reachable commits: 0 original-only / 9 ExtFUSE-only
diff: 16 files changed, 508 insertions(+), 4 deletions(-)
```

[GitHub에서 전체 Linux 변경 보기](https://github.com/extfuse/linux/compare/1d039859330b874d48080885eb31f4f129c246f1...d95ca8642fce0214cf2ebaaa792ba538d4d8381d)

변경 직전 기준 commit과 이후 9개 commit의 원본 URL·snapshot·parent는
[`diffs/linux/COMMITS.md`](diffs/linux/COMMITS.md)에 모두 연결해 두었다.

추가된 9개 커밋의 구조는 다음과 같다. PR merge 때문에 first-parent
기준으로는 3개지만, 도달 가능한 고유 커밋은 9개다.

```text
* d95ca8642fce Add extfuse prog type and helpers to tools/bpf
*   2473946e1ce9 Merge pull request #1 from balsini/extfuse-refactor
|\
| * 2fab85b8c7e0 ExtFUSE: fix printk format
| * 164663eb9f6b ExtFUSE: update fc_priv when BPF program is loaded
| * 80a1aa49bf53 ExtFUSE: refactor read_args code
| * 532b9f6a88f3 ExtFUSE: add callbacks to scope
| * 29e12e0e8346 ExtFUSE: update signature and comments for callbacks
| * 985ed2ef4735 ExtFUSE: update code style
|/
* b7923d63a0e5 ExtFUSE: Extension Framework for FUSE
* 1d039859330b original master HEAD
```

### 변경 파일과 역할

| 영역 | 파일 | 변경 의미 |
|---|---|---|
| 핵심 구현 | [`fs/fuse/extfuse.c`](linux_ExtFUSE-1.0/linux/fs/fuse/extfuse.c), [`fs/fuse/extfuse_i.h`](linux_ExtFUSE-1.0/linux/fs/fuse/extfuse_i.h) | BPF 프로그램 load/run, 요청 context, 전용 helper와 verifier/program ops |
| FUSE 요청 경로 | [`fs/fuse/dev.c`](linux_ExtFUSE-1.0/linux/fs/fuse/dev.c), [`fs/fuse/inode.c`](linux_ExtFUSE-1.0/linux/fs/fuse/inode.c), [`fs/fuse/fuse_i.h`](linux_ExtFUSE-1.0/linux/fs/fuse/fuse_i.h) | userspace 전송 전 BPF 실행, INIT에서 FD load, connection private data 보관 |
| FUSE UAPI | [`include/uapi/linux/fuse.h`](linux_ExtFUSE-1.0/linux/include/uapi/linux/fuse.h), [`include/uapi/linux/extfuse.h`](linux_ExtFUSE-1.0/linux/include/uapi/linux/extfuse.h) | bit 26 capability, `extfuse_prog_fd`, BPF가 읽고 쓸 요청 인자 종류 |
| BPF ABI | [`include/uapi/linux/bpf.h`](linux_ExtFUSE-1.0/linux/include/uapi/linux/bpf.h), [`include/linux/bpf_types.h`](linux_ExtFUSE-1.0/linux/include/linux/bpf_types.h), [`tools/include/uapi/linux/bpf.h`](linux_ExtFUSE-1.0/linux/tools/include/uapi/linux/bpf.h), [`tools/testing/selftests/bpf/bpf_helpers.h`](linux_ExtFUSE-1.0/linux/tools/testing/selftests/bpf/bpf_helpers.h) | `BPF_PROG_TYPE_EXTFUSE`, read/write helper ID, kernel/tools header 동기화 |
| BPF loader | [`samples/bpf/bpf_load.c`](linux_ExtFUSE-1.0/linux/samples/bpf/bpf_load.c) | ELF section `extfuse`와 `extfuse/<opcode>` 인식 및 program-array 구성 |
| 빌드·문서 | [`fs/fuse/Kconfig`](linux_ExtFUSE-1.0/linux/fs/fuse/Kconfig), [`fs/fuse/Makefile`](linux_ExtFUSE-1.0/linux/fs/fuse/Makefile), [`Documentation/filesystems/extfuse.txt`](linux_ExtFUSE-1.0/linux/Documentation/filesystems/extfuse.txt), [`MAINTAINERS`](linux_ExtFUSE-1.0/linux/MAINTAINERS) | `CONFIG_EXTFUSE`와 빌드 항목, 짧은 문서/관리자 정보 |

파일 상태는 **신규 4개, 수정 12개**이며 삭제, rename, binary 변경은 없다.

### 커널 동작의 핵심

1. `CONFIG_EXTFUSE`는 `FUSE_FS && BPF_SYSCALL`에 의존한다.
2. 커널이 `FUSE_INIT`에서 `FUSE_FS_EXTFUSE`(bit 26)를 광고한다.
3. daemon이 같은 flag와 `extfuse_prog_fd`를 회신하면
   `extfuse_load_prog()`가 FD의 타입을 `BPF_PROG_TYPE_EXTFUSE`로 확인한다.
4. `fuse_request_send()`는 정상 userspace queue 전송 전에
   `extfuse_request_send()`를 실행한다.
5. BPF가 `-ENOSYS`를 반환하면 기존 FUSE daemon으로 fallback한다.
   그 외에는 userspace 전송을 건너뛴다.
6. 전용 GPL-only helper `bpf_extfuse_read_args()`와
   `bpf_extfuse_write_args()`가 opcode, nodeid, 최대 3개 입력 인자와
   최대 2개 출력 인자의 안전한 복사를 제공한다.

`struct fuse_init_out`의 기존 reserved 영역 일부를 FD 필드로 재사용하므로
wire 구조체 전체 크기는 유지된다.

## libfuse: 원본과 ExtFUSE 비교

### 기준 관계와 통계

`libfuse_origin`의 detached HEAD가 ExtFUSE 첫 커밋의 직접 부모이자 정확한
merge-base다.

```text
386b1b6 original snapshot
├─ (521 upstream commits) ───────────── 86ebe1d origin/master
└─ 7c88efb ─ f945fbc ─ 5b381d8 ExtFUSE-1.0

correct diff: 3 files changed, 27 insertions(+), 2 deletions(-)
```

[GitHub에서 전체 libfuse 변경 보기](https://github.com/extfuse/libfuse/compare/386b1b6e3d0fcf7e2dfd5473e867284410dfa624...5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d)

변경 직전 기준 commit과 이후 3개 commit의 원본 URL·snapshot·parent는
[`diffs/libfuse/COMMITS.md`](diffs/libfuse/COMMITS.md)에 모두 연결해 두었다.

| 커밋 | 날짜 | 의미 |
|---|---|---|
| [`7c88efb`](https://github.com/extfuse/libfuse/commit/7c88efb9d92d64289795e43b2948977c07ccda7f) | 2019-02-28 | capability 감지, FD 필드와 INIT 협상 최초 추가 |
| [`f945fbc`](https://github.com/extfuse/libfuse/commit/f945fbc0b6ba9c9291229f6e209d82168908cc94) | 2020-07-11 | capability bit를 충돌하던 bit 21에서 최종 bit 26으로 수정 |
| [`5b381d8`](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | 2020-07-25 | `fuse_init_out`의 padding/FD 위치를 당시 수정 커널 ABI와 동기화 |

### 변경 파일

| 파일 | 통계 | 변경 의미 |
|---|---:|---|
| [`include/fuse_common.h`](libfuse_ExtFUSE-1.0/libfuse/include/fuse_common.h) | +6/-1 | `FUSE_CAP_EXTFUSE`, `fuse_conn_info.extfuse_prog_fd` 추가 |
| [`include/fuse_kernel.h`](libfuse_ExtFUSE-1.0/libfuse/include/fuse_kernel.h) | +5/-1 | `FUSE_FS_EXTFUSE`, INIT reply의 padding/FD 필드 추가 |
| [`lib/fuse_lowlevel.c`](libfuse_ExtFUSE-1.0/libfuse/lib/fuse_lowlevel.c) | +16/-0 | `do_init()`에서 capability 감지 및 flag/FD 회신 |

공개 구조체와 wire 구조체는 reserved/unused 슬롯을 줄여 전체 크기를
유지한다. libfuse는 지원 여부를 `conn.capable`에 표시할 뿐 ExtFUSE를
자동으로 켜지 않는다. 파일시스템의 `init` callback이 다음 두 값을
명시적으로 설정해야 한다.

```c
conn->want |= FUSE_CAP_EXTFUSE;
conn->extfuse_prog_fd = bpf_program_fd;
```

### 잘못된 비교를 피하는 방법

`git diff origin/master ExtFUSE-1.0`은 사용하지 않는 것이 좋다. 현재
`origin/master`는 기준점에서 521개 커밋 진행한 별도 upstream 계열이므로,
이 명령은 ExtFUSE 변경 3개와 대규모 upstream 역차이를 섞는다. 실제로
이 잘못된 기준은 111개 파일, +9,350/-11,258의 diff를 만든다.

정확한 기준은 항상 다음 두 SHA다.

```text
386b1b6e3d0fcf7e2dfd5473e867284410dfa624
5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d
```

## `extfuse/`: BPF 정책과 지원 라이브러리

이 디렉터리는 29개 커밋으로 구성되며 태그는 없다. 최초 커밋은
[`5462699`](https://github.com/extfuse/extfuse/commit/5462699b5414a739bc6c3221157827ac42250334)(2018-06-10),
구현 전체가 들어온 커밋은
[`179c03c`](https://github.com/extfuse/extfuse/commit/179c03cebbadb209d7e6b75fcc832751d762811d)(2020-01-19),
마지막 기능 변경은
[`2fbf376`](https://github.com/extfuse/extfuse/commit/2fbf37608be28269f2597cb148f1d8a03d78d834)(2020-09-08)이다.
이후 현재 HEAD까지는 기능 변경 없이 README만 갱신되었다. 최초부터
현재까지 29개 commit의 모든 원본 URL은
[`diffs/extfuse/COMMITS.md`](diffs/extfuse/COMMITS.md)에 있다.

주요 이력은 다음과 같다.

| 커밋 | 의미 |
|---|---|
| [`179c03c`](https://github.com/extfuse/extfuse/commit/179c03cebbadb209d7e6b75fcc832751d762811d) | BPF 프로그램과 userspace library 구현 추가 |
| [`5c8601d`](https://github.com/extfuse/extfuse/commit/5c8601d74527bc42d2a6b5c31443eda00b9c9fad) | 코드 정리 및 passthrough 기본 비활성화 |
| [`85db013`](https://github.com/extfuse/extfuse/commit/85db013f1c162d02e3005c9a7553c4894f18ec12)~[`022fb65`](https://github.com/extfuse/extfuse/commit/022fb65e7bf0d3d0b5a20378793215ec58c29167) | kbuild 경로, helper, include, libbpf source 관련 빌드 수정 |
| [`632336a`](https://github.com/extfuse/extfuse/commit/632336a462b0763adce80f3738886f1739be47c4) | library 결과물 이름을 `libextfuse.so`로 변경 |
| [`541f931`](https://github.com/extfuse/extfuse/commit/541f9318cd5843423ee838240b9bc1378a378062) | BPF ELF section 이름을 `fuse*`에서 `extfuse*`로 변경 |
| [`2fbf376`](https://github.com/extfuse/extfuse/commit/2fbf37608be28269f2597cb148f1d8a03d78d834) | main handler가 실제 양수 FUSE opcode만 dispatch하도록 수정 |
| [`d2acdb1`](https://github.com/extfuse/extfuse/commit/d2acdb1c8e4531562fc77fe566896e771d03026e) | 현재 HEAD; README 연락 안내만 추가 |

### 파일별 역할

| 파일 | 역할 |
|---|---|
| [`Makefile`](extfuse/Makefile) | 실행 중인 패치 커널의 kbuild로 `bpf/extfuse.o`와 루트 `libextfuse.so` 빌드 |
| [`bpf/extfuse.c`](extfuse/bpf/extfuse.c) | main dispatcher, opcode handler, entry/attribute/prog-array BPF map |
| [`bpf/lookup.h`](extfuse/bpf/lookup.h) | `(parent nodeid, name)` 기반 directory-entry 캐시 구조 |
| [`bpf/attr.h`](extfuse/bpf/attr.h) | `nodeid` 기반 `fuse_attr_out` 캐시 구조 |
| [`include/ebpf.h`](extfuse/include/ebpf.h), [`src/ebpf.c`](extfuse/src/ebpf.c) | 반환 규약, BPF ELF load와 map FD 조회·갱신·삭제 API |
| [`include/extfuse.h`](extfuse/include/extfuse.h) | FUSE opcode 목록; 수정 커널 UAPI와 내부 `extfuse_i.h`에 직접 의존 |
| [`include/utils.h`](extfuse/include/utils.h), [`src/utils.c`](extfuse/src/utils.c) | namespace/capability 유틸; 현재 library link 목록에 없고 내부 호출도 없는 잔여 코드 |
| [`LICENSE`](extfuse/LICENSE) | LGPL-2.1 전문; BPF ELF 자체는 GPL helper 사용을 위해 `GPL` license section 선언 |

Makefile은 수정 커널의 `samples/bpf/bpf_load.c`와 libbpf 구현을 `src/`로
복사해 library에 포함한다. 따라서 이 코드는 독립적인 안정 ABI를 쓰는
library가 아니라 **해당 ExtFUSE 커널 트리와 강하게 결합된 코드**다.

### 예제 BPF 정책의 실제 범위

| opcode | 동작 |
|---|---|
| `LOOKUP`, `GETATTR` | 캐시 hit이면 커널에서 응답; miss이면 upcall |
| `READ`, `WRITE` | 관련 attribute를 stale 처리한 뒤 upcall |
| `SETATTR` | attribute 캐시를 삭제한 뒤 upcall |
| `RENAME`, `RMDIR`, `UNLINK` | entry/attribute 캐시를 무효화한 뒤 upcall |
| `GETXATTR` | 즉시 `-ENODATA` 반환 |
| `FLUSH` | 즉시 성공 반환 |
| 나머지 | 등록 handler가 없어 upcall |

세 BPF map은 `entry_map`, `attr_map`, `handlers` 순서이며 `handlers`는
마지막이어야 한다는 주석이 있다. userspace가 map을 숫자 index로 다루므로
이 선언 순서는 사실상 ABI의 일부다. 이 정책은 범용 FUSE 가속기보다는
논문과 StackFS 실험을 위한 metadata-cache 예제로 보는 편이 정확하다.

## 빌드·실행 시 주의점

- 저장된 Linux는 5.2 기반 연구용 커널이다. 현대 vanilla 커널에는
  `BPF_PROG_TYPE_EXTFUSE`, 전용 helper와 `extfuse_i.h`가 없으므로
  `extfuse/`만 따로 빌드·실행할 수 없다.
- `CONFIG_BPF_SYSCALL`, `CONFIG_FUSE_FS`, `CONFIG_EXTFUSE`가 필요하다.
  원본 안내대로 FUSE는 module이 아니라 커널에 built-in하는 편을 전제로
  한다.
- `extfuse/Makefile`은 `/lib/modules/$(uname -r)/build`를 사용한다. 즉
  소스만 가지고 있는 것으로는 부족하고, 대응하는 수정 커널로 부팅한
  상태와 그 build tree가 필요하다.
- upstream `extfuse/README.md` 일부는 결과물을 `src/extfuse.o`라고 쓰지만,
  현재 Makefile과 뒤쪽 복사 예제에 따른 실제 경로는 **`bpf/extfuse.o`**다.
- README의 LLVM 예시는 `clang-3.8`, `llc-3.8`이다. 현대 LLVM과의
  호환성은 별도 검증이 필요하다.
- 이 저장소에는 실행 가능한 파일시스템과 자동 테스트가 없다. 원본
  실험은 외부 [StackFS](https://github.com/ashishbijlani/StackFS)에 의존한다.
- `PASSTHRU=1` 상수는 남아 있지만 `HAVE_PASSTHRU`가 비활성이고, 현재
  커널 경로는 `-ENOSYS`와 나머지만 구분한다. 현재 지원되는 독립 경로로
  해석하면 안 된다.

## 후속 정적 분석 체크포인트

다음은 실행으로 버그를 확정한 내용이 아니라, 현재 코드에서 발견한
추가 검토 대상이다.

- 커널의 `extfuse_unload_prog()`는 정의·선언되어 있지만 현재 트리에서
  호출부가 검색되지 않는다. `process_init_reply()`도
  `extfuse_load_prog()` 반환값을 확인하지 않는다.
- `LOOKUP` handler는 attribute가 stale인 경우 로그를 남기지만 이후
  응답 생성을 계속한다. stale cache 처리 의도를 다시 확인할 필요가 있다.
- `RENAME`의 두 이름 key가 요청 header의 같은 parent nodeid를 사용하므로
  cross-directory rename을 올바르게 표현하는지 확인할 필요가 있다.
- `ebpf_data_next()`는 `bpf_map_get_next_key()`에 `&key`, `&next`를 넘긴다.
  함수 인자가 이미 pointer이므로 pointer level이 의도와 맞는지 확인할
  필요가 있다.
- `include/utils.h`/`src/utils.c`는 현재 build graph 밖에 있다.
- `MAINTAINERS`는 `include/linux/extfuse.h`를 가리키지만 실제 신규 파일은
  `include/uapi/linux/extfuse.h`다.

연구 프로토타입의 동작과 당시 환경을 보존한 코드이므로, 현재 배포용
코드로 사용하기 전에는 수명주기, verifier context, error handling,
동시성 및 최신 커널 ABI를 별도로 검증해야 한다.

## 비교 재현 방법

이미 생성한 전체 patch와 메타데이터는 [`diffs/`](diffs)에 보존되어
있다. 아래 이력 기반 명령은 삭제 전에 사용한 생성·검증 절차의 기록이다.
현재 source 디렉터리에는 하위 `.git`이 없으므로 직접 실행할 수 없으며,
필요하면 보존 bundle 또는 공개 원격에서 Git 이력을 복원한 뒤 사용한다.

### 삭제 전에 사용한 이력 기반 비교 명령

삭제 전 저장소 최상위에서 실행했다.

```bash
LINUX_BASE=1d039859330b874d48080885eb31f4f129c246f1
LINUX_EXT=d95ca8642fce0214cf2ebaaa792ba538d4d8381d
LINUX_REPO=linux_ExtFUSE-1.0/linux

git -C "$LINUX_REPO" merge-base "$LINUX_BASE" "$LINUX_EXT"
git -C "$LINUX_REPO" rev-list --left-right --count "$LINUX_BASE...$LINUX_EXT"
git -C "$LINUX_REPO" log --graph --oneline "$LINUX_BASE..$LINUX_EXT"
git -C "$LINUX_REPO" diff --stat "$LINUX_BASE..$LINUX_EXT"
git -C "$LINUX_REPO" diff --name-status "$LINUX_BASE..$LINUX_EXT"

LIBFUSE_BASE=386b1b6e3d0fcf7e2dfd5473e867284410dfa624
LIBFUSE_EXT=5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d
LIBFUSE_REPO=libfuse_ExtFUSE-1.0/libfuse

git -C "$LIBFUSE_REPO" merge-base "$LIBFUSE_BASE" "$LIBFUSE_EXT"
git -C "$LIBFUSE_REPO" rev-list --left-right --count "$LIBFUSE_BASE...$LIBFUSE_EXT"
git -C "$LIBFUSE_REPO" log --reverse --oneline "$LIBFUSE_BASE..$LIBFUSE_EXT"
git -C "$LIBFUSE_REPO" diff --stat "$LIBFUSE_BASE..$LIBFUSE_EXT"
git -C "$LIBFUSE_REPO" diff --name-status "$LIBFUSE_BASE..$LIBFUSE_EXT"
```

특정 영역만 볼 때는 마지막에 pathspec을 붙인다.

```bash
git -C linux_ExtFUSE-1.0/linux diff \
  1d039859330b874d48080885eb31f4f129c246f1..d95ca8642fce0214cf2ebaaa792ba538d4d8381d \
  -- fs/fuse include/uapi/linux samples/bpf

git -C libfuse_ExtFUSE-1.0/libfuse diff \
  386b1b6e3d0fcf7e2dfd5473e867284410dfa624..5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d \
  -- include/fuse_common.h include/fuse_kernel.h lib/fuse_lowlevel.c
```

### 현재 사용 가능: 파일 기반 비교

Git 이력은 사라져도 원본/수정본 디렉터리가 모두 남으므로 다음 명령으로
현재 파일 차이를 계속 볼 수 있다.

```bash
diff -qr \
  linux_origin/linux \
  linux_ExtFUSE-1.0/linux

diff -ruN \
  libfuse_origin/libfuse \
  libfuse_ExtFUSE-1.0/libfuse

git diff --no-index --stat -- \
  linux_origin/linux \
  linux_ExtFUSE-1.0/linux

git diff --no-index -- \
  libfuse_origin/libfuse/include/fuse_kernel.h \
  libfuse_ExtFUSE-1.0/libfuse/include/fuse_kernel.h
```

`diff`와 `git diff --no-index`는 차이가 있으면 종료 코드 `1`을 반환한다.
이 비교에서는 정상이다.

## 중첩 `.git` 제거 후 보존 상태

다음 다섯 하위 Git metadata는 제거가 완료되었다.

```text
extfuse/.git
linux_origin/linux/.git
linux_ExtFUSE-1.0/linux/.git
libfuse_origin/libfuse/.git
libfuse_ExtFUSE-1.0/libfuse/.git
```

현재 `find . -type d -name .git`으로 확인되는 것은 보존해야 할 최상위
`ExtFUSE_Code/.git` 하나뿐이다. [`diffs/`](diffs)에는 변경 patch뿐 아니라
43개 정확한 raw commit object, 모든 관련 commit URL, 삭제 직전
refs/config/reflog inventory, libfuse/extfuse 완전 bundle과 Linux 변경
전용 bundle, 전체 SHA-256 목록이 남아 있다.

삭제 후 source tree ID, patch, bundle과 checksum 검증 결과는
[`diffs/POST_DELETE_VERIFICATION.md`](diffs/POST_DELETE_VERIFICATION.md)에
기록했다. 이제 전체 source, `diffs/`, 이 README를 최상위 저장소에
commit하면 고정 SHA/원격 링크로 원래 이력을 추적하고, 보존 patch와 두
디렉터리 쌍으로 실제 파일을 계속 비교할 수 있다. Linux 기준점 이전 전체
upstream object database만 변경 중심 보존 범위에서 제외되며, 그 범위는
공개 원격 URL로 추적한다.

## 라이선스

이 통합 저장소가 각 구성 요소의 라이선스를 대체하지 않는다.

- Linux: [`COPYING`](linux_origin/linux/COPYING) 및 파일별 SPDX 표기
- libfuse: [`COPYING`](libfuse_origin/libfuse/COPYING), [`COPYING.LIB`](libfuse_origin/libfuse/COPYING.LIB)
- extfuse library: [`LICENSE`](extfuse/LICENSE) (LGPL-2.1)

코드를 재사용하거나 배포할 때는 각 파일과 구성 요소의 원래 라이선스를
따라야 한다.
