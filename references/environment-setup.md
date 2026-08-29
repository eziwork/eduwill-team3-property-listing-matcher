# 실행 환경 확인

첫 사용·도움말 또는 현재 컴퓨터에서 로컬 도구를 처음 실행할 때만 확인한다. 이미 이번 환경에서 통과했다면 구체적인 작업마다
반복하지 않는다. 운영체제에 맞는 한 경로만 안내하고, 시스템 소프트웨어나 Python 패키지는 사용자 확인 없이 설치하지 않는다.

## 1. 운영체제와 Python

macOS 터미널:

```bash
python3 --version
```

Windows PowerShell에서는 먼저 설치 관리자가 있는지만 읽기 전용으로 확인한다.

```powershell
Get-Command py -ErrorAction SilentlyContinue
py list
```

macOS의 버전 또는 Windows `py list`의 설치된 런타임이 3.10 이상이면 다음 단계로 간다. Windows의 구형 Python Launcher가
`py list`를 지원하지 않으면 `py -0p`로 설치된 버전만 확인한다. 명령이 없거나 버전이 낮으면
[Python 공식 다운로드](https://www.python.org/downloads/)를 안내한다. macOS는 공식 `.pkg`, Windows는 Microsoft Store 또는
python.org의 Python install manager를 사용한다. 사용자 확인 후 Windows 최신 안정 런타임은 `py install default`로 설치한다.
설치 후 터미널과 Codex를 다시 시작하고 `py -3 --version`으로 재확인한다.

Python 자체가 없으면 스킬의 Python 도구로 진단할 수 없다. 있다고 확인된 후 아래 명령을 실행한다.

macOS:

```bash
python3 <tool> doctor
```

Windows PowerShell:

```powershell
py -3 <tool> doctor
```

`python.supported`와 `local_excel_ready`를 확인한다.

## 2. openpyxl

로컬 Excel 사용 또는 새 Google 매물장용 `.xlsx` 생성에는 `openpyxl`이 필요하다. `doctor`에서 미설치로 나오면 설치 이유와
명령을 보여주고 확인받은 뒤 실행한다.

macOS:

```bash
python3 -m pip install --user openpyxl
```

Windows PowerShell:

```powershell
py -3 -m pip install --user openpyxl
```

설치 뒤 `doctor`를 다시 실행한다. 권한·관리형 Python 오류가 나면 강제 설치 옵션을 덧붙이지 말고 오류를 보여준 뒤 별도
가상환경 사용 여부를 묻는다. 기존 Google Sheet의 연결·읽기처럼 `openpyxl`이 필요 없는 작업에는 설치를 강요하지 않는다.

## 3. Google Drive 플러그인

사용자가 Google Sheets를 선택했을 때만 확인한다.

1. 현재 도구에 Google Drive 플러그인의 스프레드시트 기능이 있는지 확인한다.
2. 없으면 Codex의 플러그인 검색·설치 화면에서 **Google Drive**를 설치·활성화하도록 안내한다. 가능한 플러그인 관리 기능이
   있으면 Google Drive를 검색해 제안하되, 설치·연결 완료를 확인하기 전에는 설치됐다고 말하지 않는다.
3. 사용자가 Google 로그인과 권한 승인을 마치면 `get_profile`로 실제 연결 계정을 확인한다.
4. 대상 시트에 필요한 조회·편집 권한이 있는지 Google 연결 게이트로 확인한다.

플러그인 설치나 로그인은 사용자 승인이 필요한 외부 작업이다. 실패하면 어느 단계인지와 다음 행동 하나를 말하고 Excel로
자동 전환하지 않는다. macOS와 Windows 모두 Codex 안에서 같은 플러그인 연결 절차를 사용한다.
