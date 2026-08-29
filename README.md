# eduwill-property-listing-matcher

공인중개사의 Google Sheets 또는 로컬 Excel 매물장을 만들고 연결해 매물 추가·수정·검색을 돕는 Codex 스킬입니다.
손님 조건에 맞는 매물과 값 확인이 필요한 후보를 구분하고, 거래유형별로 결정적으로 정렬합니다.

## 사전 조건

- Python 3.10 이상
- 로컬 Excel 또는 새 Google 매물장 생성 시 `openpyxl`
- Google Sheets 사용 시 Google Drive 플러그인 설치·활성화 및 Google 계정 연결

Python이 없다면 [Python 공식 다운로드](https://www.python.org/downloads/)에서 설치합니다. Windows는 Microsoft Store 또는
python.org의 Python install manager를 설치한 뒤 `py install default`로 최신 안정 Python을 설치합니다. Google Drive
플러그인은 Google Sheets 모드에만 필요합니다.

## 기본 사용 예시

- “엑셀 매물장 만들어줘” — 저장할 폴더만 받아 빈 표준 매물장 생성
- “이 Google 시트를 매물장으로 연결해줘” — 계정·시트 ID·두 탭·헤더 확인 후 연결
- “이 매물 3건 등록해줘” — 불완전한 핵심값은 보류로 저장
- “마포구, 월세 80 이하, 역 도보 10분 이내 찾아줘” — 적합·확인 필요 후보 분리
- “P001 보증금을 4억 8천으로 바꿔줘” — 변경 전 확인과 쓰기 후 재검증

## 압축파일로 설치

1. `eduwill-property-listing-matcher.zip`을 내려받습니다.
2. 기존 `~/.codex/skills/eduwill-property-listing-matcher` 폴더가 있다면 먼저 별도 위치에 백업합니다.
3. 운영체제에 맞는 명령을 실행합니다.

macOS 터미널:

```bash
mkdir -p ~/.codex/skills
unzip ~/Downloads/eduwill-property-listing-matcher.zip -d ~/.codex/skills
python3 -m pip install --user openpyxl
python3 ~/.codex/skills/eduwill-property-listing-matcher/scripts/maemul_tool.py doctor
```

Windows PowerShell:

```powershell
$skillRoot = Join-Path $HOME ".codex\skills"
New-Item -ItemType Directory -Force $skillRoot
Expand-Archive "$HOME\Downloads\eduwill-property-listing-matcher.zip" -DestinationPath $skillRoot
py -3 -m pip install --user openpyxl
py -3 "$skillRoot\eduwill-property-listing-matcher\scripts\maemul_tool.py" doctor
```

ZIP이 Desktop에 있다면 `Downloads`를 `Desktop`으로 바꿉니다. `doctor`의 `local_excel_ready`가 `true`인지 확인하고 Codex를
재시작한 뒤 `$eduwill-property-listing-matcher`로 호출합니다. 기존 설치 폴더에는 바로 덮어쓰지 않습니다.

## GitHub에서 설치

```bash
git clone https://github.com/eziwork/eduwill-team3-property-listing-matcher-ljw.git \
  ~/.codex/skills/eduwill-property-listing-matcher
python3 -m pip install --user openpyxl
```

위 설치 경로가 이미 있으면 먼저 별도 백업하거나 기존 Git 클론에서 업데이트하세요. 이 명령으로 덮어쓰지 마세요.
Windows에서는 PowerShell에서 `git clone <URL> "$HOME\.codex\skills\eduwill-property-listing-matcher"`를 사용하고
`python3` 대신 `py -3`을 사용합니다.

기존 `edwill-property-listing-matcher` 또는 `maemul-matching` 사용자의 프로필은 처음 실행할 때 새
`~/.codex/eduwill-property-listing-matcher/profiles.json`으로 복사되며, 기존 프로필 파일은 삭제하지 않습니다.

## GitHub 배포 단위

이 저장소 루트 전체가 하나의 스킬 패키지입니다. `SKILL.md`만 따로 올리지 말고 `agents`, `references`, `scripts`,
`tests`를 포함한 `eduwill-property-listing-matcher` 폴더 전체를 한 GitHub 저장소로 배포합니다.

기존 설치를 업데이트할 때는 설치 폴더에서 다음을 실행합니다.

```bash
git pull --ff-only
python3 -m unittest discover -s tests -v
```

## 테스트

```bash
python3 ~/.codex/skills/.system/skill-creator/scripts/quick_validate.py .
python3 -m unittest discover -s tests -v
python3 -m py_compile scripts/maemul_tool.py
```

스키마는 `매물` 28열과 `매물상세` 20열입니다. 세부 운영 규칙은 [SKILL.md](SKILL.md)와
[`references/`](references/)를 참고하세요.
