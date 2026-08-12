# ExtFUSE_Code 작업 지침

## 적용 범위

- 이 파일은 `/home/leedaeeun/Documents/github/ExtFUSE_Code`와 그 모든 하위 경로에 적용한다.
- 이 저장소는 원본 참조용이며 **읽기 전용**으로 취급한다. 실제 파일 시스템 권한이 쓰기를 허용하더라도 이 규칙은 바뀌지 않는다.
- 사용자가 이번 요청에서 명시적으로 허용한 이 `AGENTS.md`의 생성 자체만 예외이다. 이후에는 별도의 명시적 지시 없이 이 파일을 포함한 원본 저장소의 어떤 파일도 변경하지 않는다.

## 원본 저장소에서 허용되는 작업

- 소스와 문서의 열람, 검색, 비교 및 정적 분석
- 상태를 바꾸지 않는 Git 조회(예: `git status`, `git diff`, `git log`, `git show`, `git grep`, `git rev-parse`)
- 파일 메타데이터와 디렉터리 구조의 읽기 전용 확인

## 원본 저장소에서 금지되는 작업

- 파일 생성, 수정, 삭제, 이름 변경 및 권한 변경
- `apply_patch`, 편집기, 포매터 또는 코드 생성기를 원본에 적용하는 작업
- 빌드, 테스트, 정적 분석 등 캐시·로그·오브젝트·임시 파일을 만들 가능성이 있는 명령
- `git add`, `commit`, `checkout`, `switch`, `restore`, `reset`, `clean`, `stash`, `merge`, `rebase`, `cherry-pick`, `pull`, `push` 등 Git 상태나 외부 저장소를 변경하는 작업
- 원본 저장소 내부를 작업 디렉터리, 빌드 디렉터리 또는 결과 저장 경로로 사용하는 작업

## 수정·빌드·검증이 필요한 경우

1. `/home/leedaeeun/Documents/github/research_exp` 아래에 작업마다 고유한 새 임시 디렉터리를 만든다.
2. 필요한 코드 또는 저장소 전체를 그 디렉터리에 복사한다. 수정할 파일을 원본으로 연결하는 심볼릭 링크는 사용하지 않는다.
3. `pwd -P`로 현재 경로가 `/home/leedaeeun/Documents/github/research_exp/` 아래인지 확인한 뒤에만 수정, 빌드, 테스트, 포매팅 또는 생성 작업을 수행한다.
4. 빌드 산출물, 캐시, 로그, 패치 및 분석 결과도 모두 해당 임시 작업 공간 안에 저장한다.
5. 결과를 원본 `ExtFUSE_Code`로 다시 복사하거나 동기화하지 않는다. 필요한 변경은 임시 작업 공간의 경로와 diff로 사용자에게 보고한다.

예시:

```bash
EXTFUSE_WORKDIR="$(mktemp -d /home/leedaeeun/Documents/github/research_exp/ExtFUSE_Code.XXXXXX)"
cp -a /home/leedaeeun/Documents/github/ExtFUSE_Code/. "$EXTFUSE_WORKDIR"/
cd -- "$EXTFUSE_WORKDIR"
pwd -P
```

- `/home/leedaeeun/Documents/github/research_exp`가 없거나 쓸 수 없으면 원본이나 다른 경로로 우회하지 말고 작업을 멈춘 뒤 사용자에게 알린다.
- 작업을 시작하기 전에 원본의 커밋 ID와 복사한 작업 공간의 경로를 기록한다.
- 사용자가 명시적으로 요청하지 않은 커밋, 푸시, 설치, 권한 상승 또는 외부 시스템 변경은 복사본에서도 수행하지 않는다.

## 최우선 불변조건

**`/home/leedaeeun/Documents/github/ExtFUSE_Code`의 내용과 Git 상태는 작업 전후에 동일해야 한다.**
