# orchestrate-releases

Orchestrate V2 실행 파일 배포본과 서명된 자동 업데이트 목록(manifest)을 올리는 저장소입니다. 소스 코드는 여기에 없습니다.

최신 버전: **[Releases › Latest](https://github.com/HIMMEL95/orchestrate-releases/releases/latest)**

| 파일 | 무엇 |
|---|---|
| `orchestrate-v2-<버전>-<커밋>-windows-setup.exe` | Windows 설치 프로그램 (권장) |
| `orchestrate-v2-<버전>-<커밋>-windows.zip` | Windows — 설치 없이 풀어서 쓰기 |
| `orchestrate-v2-<버전>-<커밋>-linux.tar.gz` | Linux (x64 · arm64) |
| `orchestrate-v2-<버전>-<대상>.tar.gz`(대상 = win-x64 · win-arm64 · linux-x64 · linux-arm64) · `manifest.json` | 앱 안 자동 업데이트용 — 직접 받을 필요 없음 |

macOS 배포본은 아직 없습니다.

## 처음 설치

### Windows — 설치 프로그램 (권장)

1. `orchestrate-v2-<버전>-<커밋>-windows-setup.exe` 를 받아 실행합니다. 관리자 권한은 필요 없습니다(사용자별 설치).
2. 코드 서명이 없어 처음에 **「Windows의 PC 보호」** 창이 뜹니다 → **추가 정보** → **실행**.
3. 설치 위치는 기본 `%LOCALAPPDATA%\Programs\Orchestrate` 입니다. 마지막 화면의 **「Orchestrate 실행」** 으로 앱 창이 열립니다.
4. 다음부터는 바탕 화면 · 시작 메뉴의 **Orchestrate** 바로가기로 엽니다.
   창의 ✕ 는 끄지 않고 트레이로 숨깁니다 — 끄려면 트레이 아이콘 오른쪽 클릭 › **엔진 멈춤** · **종료**.
5. PC 에 로그인할 때 자동으로 켜려면: 설정 › 기기 · 보안 › 설정 파일 › **자동 실행**.

설치 없이 쓰려면 `orchestrate-v2-<버전>-<커밋>-windows.zip` 을 받아 **속성 › 차단 해제** 후 풀고 `start.cmd` 를 더블클릭합니다.
WebView2 런타임이 없다는 메시지 상자가 뜨면 `%USERPROFILE%\.orchestrate\orchestrate.env` 에 `ORCH_OPEN=browser` 를 적고 다시 엽니다(브라우저로 열림).

### Linux

```bash
tar -xzf orchestrate-v2-<버전>-<커밋>-linux.tar.gz
cd orchestrate-v2-<버전>-<커밋>-linux
./start.sh          # 끄기: ./stop.sh · 로그인 값 보기: ./start.sh token
```

### 처음 열면

- 앱 창은 **자동으로 로그인**된 채 열립니다. 브라우저로 직접 `http://127.0.0.1:4711` 에 들어가면 잠금 화면이 뜨는데,
  시작 창의 `토큰` 줄 값(또는 Windows `token.cmd` · Linux `./start.sh token`)을 입력합니다. **이 값은 다른 사람에게 보내지 마세요.**
- 필요한 것은 **Git** 과 쓰려는 AI CLI(Claude Code · Codex 등)뿐입니다. npm 으로 설치한 CLI 를 쓰려면 node 가 PATH 에 있어야 합니다.
- 데이터는 `~/.orchestrate`(Windows `%USERPROFILE%\.orchestrate`)에 저장됩니다.
- 사용 방법은 묶음에 든 `MANUAL.pdf` 를 보세요.

## 업데이트

### 앱 안에서 (권장)

1. 엔진이 시작 1분 뒤, 그다음 **6시간마다** 이 저장소의 최신 `manifest.json` 을 확인합니다. 새 버전이 있으면 화면 위에 **「새 버전 x.y.z · 업데이트」** 칩이 보입니다.
2. 칩이나 설정 › 기기 · 보안 › 설정 파일 › **업데이트** 에서 **「업데이트」** 를 누릅니다(바로 확인하려면 같은 곳의 **「지금 확인」**).
   받기 → 서명 · 파일 지문 확인 → DB 백업 뒤 바꾸기 → 다시 시작 순서로 진행되고, 끝나면 화면이 새로 고쳐집니다.
   **누르기 전에는 아무것도 받거나 바꾸지 않습니다.**
3. **엔진 PC 에서 눌러야 합니다** — 휴대폰 등 원격 기기에서는 업데이트를 시작할 수 없습니다.

- 새 버전이 제때 응답하지 않으면 **이전 버전과 DB 로 자동으로 되돌립니다.** 이전 버전 한 벌은 다음 업데이트가 성공할 때까지 남습니다.
- 자동 확인을 끄려면 같은 「업데이트」 블록에서 끕니다(끄면 「지금 확인」을 누를 때만 확인).

### 새 설치본으로 덮어쓰기 (Windows)

새 `windows-setup.exe` 를 받아 실행하면 **이미 설치된 자리에 덮어** 설치합니다.
떠 있는 앱 · 엔진을 먼저 멈추고 DB 를 백업한 뒤 바꾸며, 데이터 폴더(`%USERPROFILE%\.orchestrate`)는 그대로입니다.
앱 안 업데이트가 없는 아주 옛 버전(0.2.0 이전)은 한 번은 이렇게 새 설치본으로 올려야 합니다.

### 손으로 되돌리기

새 버전에서 나중에 문제가 보이면:

1. 엔진을 멈춥니다(`stop.cmd` · `./stop.sh`).
2. 설치 폴더의 `bin` 을 옆으로 옮기고 `bin.old` 를 `bin` 으로 이름을 바꿉니다.
3. 데이터 폴더의 `updates/engine.db.pre-<버전>` 을 `engine.db` 로 복사합니다(`-wal` · `-shm` 이 있으면 같이, 지금 `engine.db-wal` · `-shm` 은 지웁니다). 백업 뒤에 한 일은 사라집니다.
4. 다시 시작합니다.

## 제거

Windows 설정 › 앱 › **Orchestrate** › 제거. 데이터 폴더(`%USERPROFILE%\.orchestrate`)는 지우지 않고 남깁니다 — 완전히 지우려면 그 폴더를 직접 지웁니다.

## 이 저장소의 파일을 믿는 근거

- `manifest.json` 은 **ed25519 서명**이 붙어 있고, 앱에 들어 있는 공개 키로만 확인합니다. 서명이 맞지 않거나 같거나 낮은 버전이면 설치하지 않습니다.
- 각 업데이트 묶음은 manifest 의 크기 · SHA-256 과 묶음 안 `SHA256SUMS` 로 다시 대조한 뒤에만 바꿉니다.
- 실행 파일 자체에는 코드 서명이 없습니다(SmartScreen 경고가 뜨는 이유).
