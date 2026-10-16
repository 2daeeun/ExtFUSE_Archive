# ExtFUSE ATC'19 Code Archive

**English** | [한국어](README.ko.md)

This repository is an archive for comparing and analyzing the ExtFUSE code
associated with **“Extension Framework for File Systems in User space”**
(USENIX ATC '19).

- Paper: [USENIX presentation page](https://www.usenix.org/conference/atc19/presentation/bijlani), [PDF](https://www.usenix.org/system/files/atc19-bijlani.pdf)
- Original GitHub organization: [github.com/extfuse](https://github.com/extfuse)
- Analysis reference date: **2026-07-14**

The Git information and statistics in this document were verified before the
nested repositories' `.git` directories were removed, using `git log`,
`git merge-base`, `git diff`, and direct file comparisons. All five nested
working trees were clean at the time, with no untracked files or build artifacts.

> **After deletion:** All five nested `.git` directories were removed on
> 2026-07-14. Only the top-level `ExtFUSE_Code/.git` remains. The
> [post-deletion verification record](diffs/POST_DELETE_VERIFICATION.md)
> confirms that the five remaining source trees exactly matched their
> pre-deletion Git trees and that the patches, bundles, and checksums were valid.

> Here, an `origin` directory means **the exact baseline snapshot from which
> the ExtFUSE branch diverged**, rather than the latest upstream version.
> This distinction is particularly important for the libfuse comparison.

## Documentation languages

English is the default language for this archive's documentation. The original
Korean text is available in matching `*.ko.md` files. Use the language links at
the top of each document to switch languages; links between Korean documents
keep the selected language where a Korean version is available.

| Document | English | Korean |
|---|---|---|
| Archive overview | [README](README.md) | [한국어](README.ko.md) |
| Preserved change history | [README](diffs/README.md) | [한국어](diffs/README.ko.md) |
| Post-deletion verification | [Verification record](diffs/POST_DELETE_VERIFICATION.md) | [한국어](diffs/POST_DELETE_VERIFICATION.ko.md) |
| Git bundles and restoration | [README](diffs/bundles/README.md) | [한국어](diffs/bundles/README.ko.md) |
| Git metadata inventory | [README](diffs/git-metadata/README.md) | [한국어](diffs/git-metadata/README.ko.md) |
| Linux commits | [Commit history](diffs/linux/COMMITS.md) | [한국어](diffs/linux/COMMITS.ko.md) |
| libfuse commits | [Commit history](diffs/libfuse/COMMITS.md) | [한국어](diffs/libfuse/COMMITS.ko.md) |
| ExtFUSE library/BPF commits | [Commit history](diffs/extfuse/COMMITS.md) | [한국어](diffs/extfuse/COMMITS.ko.md) |

## Directory and key file layout

| Path | Purpose |
|---|---|
| [`pdf`](pdf/) | ExtFUSE paper, related presentations, and Figure 6/9 reference CSVs |
| [`pdf/fig9_requests.csv`](pdf/fig9_requests.csv) | FUSE request counts for the kernel compilation and decompression workloads in Figure 9 |
| [`pdf/fig6_throughput.csv`](pdf/fig6_throughput.csv) | Filebench data workload throughput in Figure 6, in ops/sec |
| [`linux_origin/linux`](linux_origin/linux) | Linux baseline snapshot before the ExtFUSE patch |
| [`linux_ExtFUSE-1.0/linux`](linux_ExtFUSE-1.0/linux) | Linux with the ExtFUSE kernel fast path and dedicated BPF ABI |
| [`libfuse_origin/libfuse`](libfuse_origin/libfuse) | Baseline snapshot from which the ExtFUSE libfuse branch diverged |
| [`libfuse_ExtFUSE-1.0/libfuse`](libfuse_ExtFUSE-1.0/libfuse) | libfuse with the ExtFUSE capability and BPF program FD passing |
| [`extfuse`](extfuse) | BPF policy programs and the `libextfuse.so` source for loading and managing them |
| [`diffs`](diffs) | Patches, commit URLs/raw objects, Git metadata, and restoration bundles for all three code lineages, captured before the nested `.git` directories were deleted |

The `linux_*` and `libfuse_*` directories are baseline/modified pairs from their
respective remote repositories. In contrast, `extfuse/` contains support code
written specifically for ExtFUSE, rather than a fork of an existing project, so
there is no corresponding `extfuse_origin/` directory.

## Paper Figure 6/9 reference CSVs

The two CSVs tabulate reference values corresponding to Figures 6 and 9 in the
[archived paper](pdf/atc19-bijlani.pdf). Each row represents one condition;
these files do not contain results from new experiments on the current ExtFUSE
port.

### `fig9_requests.csv`: Figure 9 request counts

[`pdf/fig9_requests.csv`](pdf/fig9_requests.csv) records the number of requests
delivered to the userspace StackFS daemon during compilation
(`make tinyconfig; make -j4`) and decompression (`tar xf`) of Linux 4.18.0.
It has **42 data rows**, excluding the header: seven opcodes and three
configurations for each workload.

| Column | Meaning |
|---|---|
| `workload` | `compile`: kernel compilation; `decompress`: kernel source decompression |
| `opcode` | FUSE request type counted. Compilation includes `GETATTR`, `GETXATTR`, `LOOKUP`, `OPEN`, `READ`, `SETATTR`, and `WRITE`; decompression uses `CREATE` instead of `OPEN` |
| `config` | The paper's `Opt`, `MDOpt`, and `AllOpt` configurations |
| `requests` | Number of requests delivered to the daemon under the given condition; neither throughput per second nor elapsed time |

These are the request counts shown in Figure 9, rather than total syscall counts
or sums over all FUSE opcodes. A value of `0` does not imply that the kernel
performed no operations of the same type. Elapsed time and EXT4 results are not
included in this CSV.

### `fig6_throughput.csv`: Figure 6 throughput

[`pdf/fig6_throughput.csv`](pdf/fig6_throughput.csv) records **Filebench data
microbenchmark throughput** for xattr-enabled FUSE/ExtFUSE StackFS and EXT4.
It has **128 data rows**, excluding the header: four workloads × two thread/file
settings per workload × four I/O sizes × four configurations.

| Column | Meaning |
|---|---|
| `workload` | `RR`: random read; `RW`: random write; `SR`: sequential read; `SW`: sequential write |
| `setting` | Thread/file counts. `1th,1f` means one thread and one file for every workload; `32th,1f` means 32 threads sharing one file for RR/RW; `32th,32f` means 32 threads and 32 files for SR/SW |
| `io_size` | I/O size per operation: `4KB`, `32KB`, `128KB`, or `1MB` |
| `config` | `EXT4`, `Opt`, `MDOpt`, or `AllOpt` |
| `ops_per_sec` | Operations per second (ops/sec), distinct from byte bandwidth or daemon request counts |

The `setting` values contain commas and are quoted in the CSV, for example
`"1th,1f"`. Use a CSV parser rather than splitting lines on commas.

### Shared configurations and data scope

The configuration definitions below follow Table 5 of the paper.

| Config | Meaning |
|---|---|
| `EXT4` | Native EXT4 baseline; included only in `fig6_throughput.csv` |
| `Opt` | Existing FUSE StackFS optimizations: 128KB writes, splice, writeback cache, and multithreading |
| `MDOpt` | `Opt` plus ExtFUSE lookup, attribute, and xattr response caches |
| `AllOpt` | `MDOpt` plus passthrough of read/write requests to the underlying host filesystem |

The CSVs do not record raw run logs, repeated measurements, uncertainty, or the
numeric extraction method. The precision of individual extracted values cannot
be established from the CSVs alone. Use them as paper-comparison reference values
and distinguish them from measurements obtained in new experiments.

## Preserved Git baselines

| Directory | Checkout before deletion | HEAD before deletion | Description |
|---|---|---|---|
| `linux_origin/linux` | `master` | [`1d039859330b`](https://github.com/extfuse/linux/commit/1d039859330b874d48080885eb31f4f129c246f1) | 2019-07-14, `v5.2-8274-g1d039859330b` |
| `linux_ExtFUSE-1.0/linux` | `ExtFUSE-1.0` | [`d95ca8642fce`](https://github.com/extfuse/linux/commit/d95ca8642fce0214cf2ebaaa792ba538d4d8381d) | 2020-09-08, `v5.2-8283-gd95ca8642fce` |
| `libfuse_origin/libfuse` | detached HEAD | [`386b1b6e3d0f`](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | 2015-09-29, `fuse_2_9_3-105-g386b1b6` |
| `libfuse_ExtFUSE-1.0/libfuse` | `ExtFUSE-1.0` | [`5b381d81f0d2`](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | 2020-07-25, `fuse_2_9_3-108-g5b381d8` |
| `extfuse` | `master` | [`d2acdb1c8e45`](https://github.com/extfuse/extfuse/commit/d2acdb1c8e4531562fc77fe566896e771d03026e) | 2023-05-02, standalone code in this archive |

The original remotes are:

- Linux: <https://github.com/extfuse/linux>
- libfuse: <https://github.com/extfuse/libfuse>
- ExtFUSE library/BPF: <https://github.com/extfuse/extfuse>

## Preserved Git diffs and original commit URLs

[`diffs/`](diffs) preserves source patches, commit metadata, and original URLs
so that the starting points and subsequent histories remain traceable after
the nested `.git` directories have been deleted.

| Component | Starting point | All original commit URLs | Cumulative patch | Per-commit/per-file records |
|---|---|---|---|---|
| Linux | [Baseline `1d039859330b`](https://github.com/extfuse/linux/commit/1d039859330b874d48080885eb31f4f129c246f1) · [code snapshot](https://github.com/extfuse/linux/tree/1d039859330b874d48080885eb31f4f129c246f1) | [10 rows: baseline + 9 changes](diffs/linux/COMMITS.md) | [`changes.patch`](diffs/linux/changes.patch) | [mailbox](diffs/linux/series.mbox), [file → commit URL](diffs/linux/files.tsv), [graph](diffs/linux/history.graph.txt) |
| libfuse | [Baseline `386b1b6e3d0f`](https://github.com/extfuse/libfuse/commit/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) · [code snapshot](https://github.com/extfuse/libfuse/tree/386b1b6e3d0fcf7e2dfd5473e867284410dfa624) | [4 rows: baseline + 3 changes](diffs/libfuse/COMMITS.md) | [`changes.patch`](diffs/libfuse/changes.patch) | [mailbox](diffs/libfuse/series.mbox), [file → commit URL](diffs/libfuse/files.tsv), [graph](diffs/libfuse/history.graph.txt) |
| extfuse | No separate vanilla baseline · [first commit `5462699b5414`](https://github.com/extfuse/extfuse/commit/5462699b5414a739bc6c3221157827ac42250334) · [code snapshot](https://github.com/extfuse/extfuse/tree/5462699b5414a739bc6c3221157827ac42250334) | [All 29 commits](diffs/extfuse/COMMITS.md) | [Current tree patch](diffs/extfuse/changes.patch) | [mailbox](diffs/extfuse/series.mbox), [file → commit URL](diffs/extfuse/files.tsv), [graph](diffs/extfuse/history.graph.txt) |

Each `commits.tsv` includes commit/tree/parent SHAs, author and committer
information, and GPG signature presence, along with machine-readable columns
for these URLs:

- Original GitHub commit and its complete source snapshot
- Raw tree API
- Each parent commit and parent-to-commit comparison
- Starting-point-to-commit comparison

The raw commit directories for [Linux](diffs/linux/raw-commits),
[libfuse](diffs/libfuse/raw-commits), and [extfuse](diffs/extfuse/raw-commits)
contain readable commit text and byte-for-byte exact Base64 objects. Decoding
all 43 objects—10 for Linux, four for libfuse, and 29 for extfuse—reproduced
their original commit SHAs.

```bash
base64 --decode diffs/linux/raw-commits/<SHA>.commit.b64 |
  git hash-object -t commit --stdin
```

The archive also preserves:

- [Refs/tags/reflogs/config/object inventories for all five checkouts](diffs/git-metadata/README.md)
- [Complete extfuse/libfuse bundles and the Linux changes-only bundle](diffs/bundles/README.md)
- [SHA-256 checksums of preserved files](diffs/SHA256SUMS)
- [Both parent diffs and the combined diff for the extfuse merge](diffs/extfuse/merges)

Before deletion, each Linux clone's complete upstream object database occupied
about 2.9 GB, and the two clones had identical packs. To focus preservation on
the ExtFUSE changes, the archive avoids a third copy of the complete history.
It retains both baseline and modified source trees, URLs/raw commits/patches
for the changed range, and a prerequisite bundle containing the nine commits
and their trees/blobs. See [`diffs/README.md`](diffs/README.md) for the detailed
scope and generation, restoration, and verification procedures.

```bash
less diffs/linux/COMMITS.md
git apply --stat diffs/linux/changes.patch
git apply --numstat diffs/libfuse/changes.patch
sha256sum -c diffs/SHA256SUMS
```

The Linux and libfuse cumulative patches passed `git apply --check` in both
directions: forward against the baseline and reverse against the modified tree.
Applying their per-commit patches to the base also reproduced the original
ExtFUSE HEAD trees. The extfuse cumulative patch likewise reconstructs its exact
archived tree from an empty tree.

## How the components connect

The three code lineages work together as follows.

```text
FUSE filesystem daemon
  ├─ libextfuse.so loads bpf/extfuse.o and obtains a BPF program FD
  └─ The modified libfuse init callback passes the capability and FD
                         │ FUSE_INIT reply
                         ▼
Modified Linux FUSE driver attaches a BPF_PROG_TYPE_EXTFUSE program
                         │
Normal FUSE request ──> main BPF ──opcode tail call──> per-opcode handler
                         │
            ┌────────────┴────────────┐
            │ -ENOSYS                 │ Other return values
            ▼                         ▼
   Existing userspace upcall    Kernel handles response/error
```

The key role of libfuse is to **pass the daemon's BPF program FD to the kernel
during FUSE initialization negotiation**. The modified Linux kernel executes
the request logic and provides the helpers and verifier; the cache policy lives
in `extfuse/bpf/extfuse.c`.

## Linux: baseline versus ExtFUSE

### Baseline relationship and statistics

The baseline HEAD is the exact merge-base of the ExtFUSE branch.

```text
base: 1d039859330b874d48080885eb31f4f129c246f1
head: d95ca8642fce0214cf2ebaaa792ba538d4d8381d

reachable commits: 0 original-only / 9 ExtFUSE-only
diff: 16 files changed, 508 insertions(+), 4 deletions(-)
```

[View the complete Linux changes on GitHub](https://github.com/extfuse/linux/compare/1d039859330b874d48080885eb31f4f129c246f1...d95ca8642fce0214cf2ebaaa792ba538d4d8381d)

[`diffs/linux/COMMITS.md`](diffs/linux/COMMITS.md) links the original URLs,
snapshots, and parents for the baseline commit and all nine subsequent commits.

The nine added commits have the following topology. PR merges leave only three
commits on the first-parent path, but there are nine distinct reachable commits.

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

### Changed files and their roles

| Area | Files | Purpose of changes |
|---|---|---|
| Core implementation | [`fs/fuse/extfuse.c`](linux_ExtFUSE-1.0/linux/fs/fuse/extfuse.c), [`fs/fuse/extfuse_i.h`](linux_ExtFUSE-1.0/linux/fs/fuse/extfuse_i.h) | BPF program loading/execution, request context, dedicated helpers, and verifier/program operations |
| FUSE request path | [`fs/fuse/dev.c`](linux_ExtFUSE-1.0/linux/fs/fuse/dev.c), [`fs/fuse/inode.c`](linux_ExtFUSE-1.0/linux/fs/fuse/inode.c), [`fs/fuse/fuse_i.h`](linux_ExtFUSE-1.0/linux/fs/fuse/fuse_i.h) | Run BPF before userspace delivery, load the FD during INIT, and retain connection private data |
| FUSE UAPI | [`include/uapi/linux/fuse.h`](linux_ExtFUSE-1.0/linux/include/uapi/linux/fuse.h), [`include/uapi/linux/extfuse.h`](linux_ExtFUSE-1.0/linux/include/uapi/linux/extfuse.h) | Bit 26 capability, `extfuse_prog_fd`, and request argument types accessible to BPF |
| BPF ABI | [`include/uapi/linux/bpf.h`](linux_ExtFUSE-1.0/linux/include/uapi/linux/bpf.h), [`include/linux/bpf_types.h`](linux_ExtFUSE-1.0/linux/include/linux/bpf_types.h), [`tools/include/uapi/linux/bpf.h`](linux_ExtFUSE-1.0/linux/tools/include/uapi/linux/bpf.h), [`tools/testing/selftests/bpf/bpf_helpers.h`](linux_ExtFUSE-1.0/linux/tools/testing/selftests/bpf/bpf_helpers.h) | `BPF_PROG_TYPE_EXTFUSE`, read/write helper IDs, and synchronized kernel/tools headers |
| BPF loader | [`samples/bpf/bpf_load.c`](linux_ExtFUSE-1.0/linux/samples/bpf/bpf_load.c) | Recognize ELF sections `extfuse` and `extfuse/<opcode>` and populate the program array |
| Build/documentation | [`fs/fuse/Kconfig`](linux_ExtFUSE-1.0/linux/fs/fuse/Kconfig), [`fs/fuse/Makefile`](linux_ExtFUSE-1.0/linux/fs/fuse/Makefile), [`Documentation/filesystems/extfuse.txt`](linux_ExtFUSE-1.0/linux/Documentation/filesystems/extfuse.txt), [`MAINTAINERS`](linux_ExtFUSE-1.0/linux/MAINTAINERS) | `CONFIG_EXTFUSE`, build entries, and brief documentation/maintainer information |

There are **four added files and 12 modified files**, with no deletions, renames,
or binary changes.

### Core kernel behavior

1. `CONFIG_EXTFUSE` depends on `FUSE_FS && BPF_SYSCALL`.
2. The kernel advertises `FUSE_FS_EXTFUSE` (bit 26) during `FUSE_INIT`.
3. If the daemon replies with the same flag and `extfuse_prog_fd`,
   `extfuse_load_prog()` checks that the FD has type `BPF_PROG_TYPE_EXTFUSE`.
4. `fuse_request_send()` calls `extfuse_request_send()` before normal userspace
   queue delivery.
5. If BPF returns `-ENOSYS`, the request falls back to the existing FUSE daemon.
   Other return values skip userspace delivery.
6. The dedicated GPL-only helpers `bpf_extfuse_read_args()` and
   `bpf_extfuse_write_args()` provide safe copying of the opcode, nodeid, up to
   three input arguments, and up to two output arguments.

The FD field reuses part of the reserved area in `struct fuse_init_out`, keeping
the total wire structure size unchanged.

## libfuse: baseline versus ExtFUSE

### Baseline relationship and statistics

The detached HEAD in `libfuse_origin` is both the direct parent of the first
ExtFUSE commit and the exact merge-base.

```text
386b1b6 original snapshot
├─ (521 upstream commits) ───────────── 86ebe1d origin/master
└─ 7c88efb ─ f945fbc ─ 5b381d8 ExtFUSE-1.0

correct diff: 3 files changed, 27 insertions(+), 2 deletions(-)
```

[View the complete libfuse changes on GitHub](https://github.com/extfuse/libfuse/compare/386b1b6e3d0fcf7e2dfd5473e867284410dfa624...5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d)

[`diffs/libfuse/COMMITS.md`](diffs/libfuse/COMMITS.md) links the original URLs,
snapshots, and parents for the baseline commit and all three subsequent commits.

| Commit | Date | Purpose |
|---|---|---|
| [`7c88efb`](https://github.com/extfuse/libfuse/commit/7c88efb9d92d64289795e43b2948977c07ccda7f) | 2019-02-28 | Initially add capability detection, the FD field, and INIT negotiation |
| [`f945fbc`](https://github.com/extfuse/libfuse/commit/f945fbc0b6ba9c9291229f6e209d82168908cc94) | 2020-07-11 | Move the capability from conflicting bit 21 to its final bit 26 |
| [`5b381d8`](https://github.com/extfuse/libfuse/commit/5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d) | 2020-07-25 | Synchronize the padding/FD position in `fuse_init_out` with the modified kernel ABI at that time |

### Changed files

| File | Statistics | Purpose of changes |
|---|---:|---|
| [`include/fuse_common.h`](libfuse_ExtFUSE-1.0/libfuse/include/fuse_common.h) | +6/-1 | Add `FUSE_CAP_EXTFUSE` and `fuse_conn_info.extfuse_prog_fd` |
| [`include/fuse_kernel.h`](libfuse_ExtFUSE-1.0/libfuse/include/fuse_kernel.h) | +5/-1 | Add `FUSE_FS_EXTFUSE` and INIT reply padding/FD fields |
| [`lib/fuse_lowlevel.c`](libfuse_ExtFUSE-1.0/libfuse/lib/fuse_lowlevel.c) | +16/-0 | Detect the capability and return the flag/FD in `do_init()` |

The public and wire structures retain their sizes by reducing reserved/unused
slots. libfuse exposes support through `conn.capable`; the filesystem's `init`
callback must explicitly set both values below to enable ExtFUSE.

```c
conn->want |= FUSE_CAP_EXTFUSE;
conn->extfuse_prog_fd = bpf_program_fd;
```

### Avoiding an incorrect comparison

Avoid `git diff origin/master ExtFUSE-1.0`. At the analysis reference date,
`origin/master` was a separate upstream lineage 521 commits beyond the baseline.
That command mixes the three ExtFUSE changes with a large reverse upstream diff:
111 files, +9,350/-11,258.

Always use these two SHAs for the exact comparison:

```text
386b1b6e3d0fcf7e2dfd5473e867284410dfa624
5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d
```

## `extfuse/`: BPF policies and support library

This directory has 29 commits and no tags. Its first commit is
[`5462699`](https://github.com/extfuse/extfuse/commit/5462699b5414a739bc6c3221157827ac42250334)
(2018-06-10); the complete implementation arrived in
[`179c03c`](https://github.com/extfuse/extfuse/commit/179c03cebbadb209d7e6b75fcc832751d762811d)
(2020-01-19); the last functional change is
[`2fbf376`](https://github.com/extfuse/extfuse/commit/2fbf37608be28269f2597cb148f1d8a03d78d834)
(2020-09-08). Subsequent commits through the archived HEAD only update the
README. [`diffs/extfuse/COMMITS.md`](diffs/extfuse/COMMITS.md) lists the original
URLs for all 29 commits.

Key milestones are:

| Commit | Purpose |
|---|---|
| [`179c03c`](https://github.com/extfuse/extfuse/commit/179c03cebbadb209d7e6b75fcc832751d762811d) | Add BPF programs and the userspace library implementation |
| [`5c8601d`](https://github.com/extfuse/extfuse/commit/5c8601d74527bc42d2a6b5c31443eda00b9c9fad) | Clean up the code and disable passthrough by default |
| [`85db013`](https://github.com/extfuse/extfuse/commit/85db013f1c162d02e3005c9a7553c4894f18ec12)–[`022fb65`](https://github.com/extfuse/extfuse/commit/022fb65e7bf0d3d0b5a20378793215ec58c29167) | Fix kbuild paths, helpers, includes, and libbpf source handling |
| [`632336a`](https://github.com/extfuse/extfuse/commit/632336a462b0763adce80f3738886f1739be47c4) | Rename the library output to `libextfuse.so` |
| [`541f931`](https://github.com/extfuse/extfuse/commit/541f9318cd5843423ee838240b9bc1378a378062) | Rename BPF ELF sections from `fuse*` to `extfuse*` |
| [`2fbf376`](https://github.com/extfuse/extfuse/commit/2fbf37608be28269f2597cb148f1d8a03d78d834) | Dispatch only positive FUSE opcodes in the main handler |
| [`d2acdb1`](https://github.com/extfuse/extfuse/commit/d2acdb1c8e4531562fc77fe566896e771d03026e) | Archived HEAD; only adds contact information to the README |

### File roles

| File | Role |
|---|---|
| [`Makefile`](extfuse/Makefile) | Build `bpf/extfuse.o` and the root-level `libextfuse.so` using the running patched kernel's kbuild |
| [`bpf/extfuse.c`](extfuse/bpf/extfuse.c) | Main dispatcher, opcode handlers, and entry/attribute/program-array BPF maps |
| [`bpf/lookup.h`](extfuse/bpf/lookup.h) | Directory-entry cache keyed by `(parent nodeid, name)` |
| [`bpf/attr.h`](extfuse/bpf/attr.h) | `fuse_attr_out` cache keyed by `nodeid` |
| [`include/ebpf.h`](extfuse/include/ebpf.h), [`src/ebpf.c`](extfuse/src/ebpf.c) | Return conventions, BPF ELF loading, and map FD lookup/update/delete APIs |
| [`include/extfuse.h`](extfuse/include/extfuse.h) | FUSE opcode list; directly depends on the modified kernel UAPI and internal `extfuse_i.h` |
| [`include/utils.h`](extfuse/include/utils.h), [`src/utils.c`](extfuse/src/utils.c) | Remaining namespace/capability utilities, absent from the library link list and unused internally |
| [`LICENSE`](extfuse/LICENSE) | Full LGPL-2.1 text; the BPF ELF declares a `GPL` license section to use GPL helpers |

The Makefile copies `samples/bpf/bpf_load.c` and the libbpf implementation from
the modified kernel into `src/` and includes them in the library. This code is
therefore **tightly coupled to that specific ExtFUSE kernel tree**.

### Actual scope of the example BPF policy

| Opcode | Behavior |
|---|---|
| `LOOKUP`, `GETATTR` | Respond in the kernel on a cache hit; upcall on a miss |
| `READ`, `WRITE` | Mark related attributes stale, then upcall |
| `SETATTR` | Delete cached attributes, then upcall |
| `RENAME`, `RMDIR`, `UNLINK` | Invalidate entry/attribute caches, then upcall |
| `GETXATTR` | Immediately return `-ENODATA` |
| `FLUSH` | Immediately return success |
| Others | Upcall because no handler is registered |

The three BPF maps are declared in the order `entry_map`, `attr_map`, `handlers`,
with a comment requiring `handlers` to be last. Since userspace accesses maps
by numeric index, this declaration order effectively forms part of the ABI.
The policy is best understood as a metadata-cache example for the paper and
StackFS experiments.

## Build and execution considerations

- The archived Linux is a research kernel based on 5.2. Modern vanilla kernels
  lack `BPF_PROG_TYPE_EXTFUSE`, its dedicated helpers, and `extfuse_i.h`, so
  `extfuse/` cannot be built and run independently.
- `CONFIG_BPF_SYSCALL`, `CONFIG_FUSE_FS`, and `CONFIG_EXTFUSE` are required.
  The original instructions assume FUSE is built into the kernel.
- `extfuse/Makefile` uses `/lib/modules/$(uname -r)/build`. Having the source
  alone is insufficient: the corresponding modified kernel must be running,
  with its build tree available.
- Parts of the upstream `extfuse/README.md` call the output `src/extfuse.o`,
  but the Makefile and the later copy example use the actual path
  **`bpf/extfuse.o`**.
- The README's LLVM examples use `clang-3.8` and `llc-3.8`. Compatibility with
  modern LLVM requires separate verification.
- This repository does not include a runnable filesystem or automated tests.
  The original experiments depend on external
  [StackFS](https://github.com/ashishbijlani/StackFS).
- The `PASSTHRU=1` constant remains, but `HAVE_PASSTHRU` is disabled and the
  archived kernel path distinguishes only `-ENOSYS` from other return values.
  Do not interpret it as a currently supported standalone passthrough path.

## Follow-up static analysis checkpoints

The following are candidates for further review found in the archived code;
they are not bugs confirmed by execution.

- The kernel's `extfuse_unload_prog()` is defined and declared, but no caller
  was found in this tree. `process_init_reply()` also ignores the return value
  of `extfuse_load_prog()`.
- The `LOOKUP` handler logs stale attributes but continues constructing the
  response. The intended stale-cache behavior needs review.
- Both name keys in `RENAME` use the same parent nodeid from the request header.
  Check whether this correctly represents cross-directory renames.
- `ebpf_data_next()` passes `&key` and `&next` to `bpf_map_get_next_key()`,
  although its arguments are already pointers. Check the intended pointer level.
- `include/utils.h` and `src/utils.c` are outside the current build graph.
- `MAINTAINERS` points to `include/linux/extfuse.h`, while the actual added file
  is `include/uapi/linux/extfuse.h`.

This archive preserves a research prototype and its original environment.
Before using it in a current deployment, separately verify lifecycle handling,
verifier context, error handling, concurrency, and modern kernel ABI compatibility.

## Reproducing the comparisons

The complete generated patches and metadata are preserved in [`diffs/`](diffs).
The history-based commands below record the generation and verification
procedures used before deletion. They cannot run directly in the source
directories now because those directories no longer have nested `.git`
directories. Restore the histories from the preserved bundles or public remotes
in a separate workspace if needed.

### History-based comparison commands used before deletion

These commands were run from the top-level repository before deletion.

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

To inspect a specific area, append a pathspec.

```bash
git -C linux_ExtFUSE-1.0/linux diff \
  1d039859330b874d48080885eb31f4f129c246f1..d95ca8642fce0214cf2ebaaa792ba538d4d8381d \
  -- fs/fuse include/uapi/linux samples/bpf

git -C libfuse_ExtFUSE-1.0/libfuse diff \
  386b1b6e3d0fcf7e2dfd5473e867284410dfa624..5b381d81f0d2a4a37ca4c73a3c8e5e778138b32d \
  -- include/fuse_common.h include/fuse_kernel.h lib/fuse_lowlevel.c
```

### Available now: file-based comparisons

Both baseline and modified directories remain available, so the following
commands can still compare their files without the nested Git histories.

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

`diff` and `git diff --no-index` return exit code `1` when differences are found.
That is expected for these comparisons.

## Preservation after removing the nested `.git` directories

The following five nested Git metadata directories have been removed:

```text
extfuse/.git
linux_origin/linux/.git
linux_ExtFUSE-1.0/linux/.git
libfuse_origin/libfuse/.git
libfuse_ExtFUSE-1.0/libfuse/.git
```

`find . -type d -name .git` finds only the top-level `ExtFUSE_Code/.git`.
[`diffs/`](diffs) retains the
change patches, 43 exact raw commit objects, all relevant commit URLs,
refs/config/reflog inventories from immediately before deletion, complete
libfuse/extfuse bundles, the Linux changes-only bundle, and SHA-256 checksums.

[`diffs/POST_DELETE_VERIFICATION.md`](diffs/POST_DELETE_VERIFICATION.md) records
the source tree ID, patch, bundle, and checksum checks performed after deletion.
Preserving all source trees, `diffs/`, and this README in the top-level
repository makes the original history traceable through fixed SHAs and remote
links, while the patches and paired directories allow direct file comparisons.
Only the complete upstream Linux object database before the baseline falls
outside this change-focused archive; public remote URLs identify that history.

The English/Korean documentation update preserves the original checksum
manifest as [`diffs/SHA256SUMS.archive`](diffs/SHA256SUMS.archive). The active
[`diffs/SHA256SUMS`](diffs/SHA256SUMS) covers the translated documentation and
Korean copies as well; the archived source, patch, raw object, and bundle
contents are unchanged. See [integrity and limitations](diffs/README.md#integrity-and-limitations).

## Licenses

This combined repository does not replace the licenses of its components.

- Linux: [`COPYING`](linux_origin/linux/COPYING) and per-file SPDX identifiers
- libfuse: [`COPYING`](libfuse_origin/libfuse/COPYING), [`COPYING.LIB`](libfuse_origin/libfuse/COPYING.LIB)
- extfuse library: [`LICENSE`](extfuse/LICENSE) (LGPL-2.1)

Follow the original licenses of each file and component when reusing or
redistributing the code.
