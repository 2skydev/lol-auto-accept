# 롤 자동 수락

League of Legends 매칭 수락 창이 뜨면 자동으로 수락하는 Windows 트레이 앱입니다.

## 사용자 안내

### 다운로드

[lol-auto-accept.exe](https://github.com/2skydev/lol-auto-accept/releases/latest/download/lol-auto-accept.exe) 링크를 통해 다운로드합니다.


별도 설치 과정은 없습니다. 다운로드한 `lol-auto-accept.exe`를 원하는 폴더에 둔 뒤 실행하면 됩니다.

> 다운로드가 되지 않으면 [최신 릴리스 페이지](https://github.com/2skydev/lol-auto-accept/releases/latest)로 이동한 뒤 Assets 영역에서 `lol-auto-accept.exe`를 다운로드합니다.

### 사용 방법

1. `lol-auto-accept.exe`를 실행합니다.
2. League of Legends 클라이언트를 실행합니다.
3. Windows 작업 표시줄의 트레이 아이콘에서 상태를 확인합니다.
4. 매칭이 잡히면 설정한 대기시간 후 자동으로 수락합니다.

> [!Note]
> `lol-auto-accept.exe`와 League of Legends 클라이언트의 실행 순서는 상관 없습니다.<br>클라이언트가 아직 켜져 있지 않으면 대기하다가, 클라이언트 실행 후 자동으로 연결을 시도합니다.

트레이 메뉴에서 설정할 수 있는 항목:

- `대기시간`: 자동 수락까지 기다릴 시간입니다. `0초`부터 `8초`까지 선택할 수 있습니다.
- `부팅 시 앱 자동 실행`: Windows 로그인 후 앱을 자동 실행할지 설정합니다.
- `앱 종료`: 앱을 종료합니다.

처음 실행하면 부팅 시 자동 실행이 기본으로 켜집니다. 원하지 않으면 트레이 메뉴에서 `부팅 시 앱 자동 실행`을 끄면 됩니다.

### 업데이트

1. 트레이 메뉴에서 `앱 종료`를 누릅니다.
2. 최신 `lol-auto-accept.exe`를 다시 다운로드합니다.
3. 기존 파일을 새 파일로 교체한 뒤 실행합니다.

### 설정 파일

설정은 Windows의 `%APPDATA%\롤 자동 수락\config.toml`에 저장됩니다.

League of Legends가 기본 경로가 아닌 곳에 설치되어 있고 앱이 클라이언트를 찾지 못하면 `config.toml`의 `league_dir`에 League of Legends 설치 폴더를 입력할 수 있습니다.

예시:

```toml
delay_seconds = 0
start_with_windows = true
league_dir = "D:\\Riot Games\\League of Legends"
```

## 개발자 안내

### 요구 사항

- Rust toolchain
- Windows 빌드 환경

이 앱은 Windows 트레이와 Windows 자동 실행 등록을 사용합니다. 최종 실행 파일은 Windows에서 빌드하고 테스트하는 것을 권장합니다.

### 로컬 실행

```bash
cargo run
```

### 테스트

```bash
cargo test
```

### 릴리스 빌드

```bash
cargo build --release
```

빌드 결과물은 `target/release/lol-auto-accept.exe`에 생성됩니다.

### 프로젝트 구조

- `src/app.rs`: 앱 실행 흐름, 단일 인스턴스, 트레이 이벤트 처리
- `src/auto_accept.rs`: 수락 이벤트 감지와 자동 수락 스케줄링
- `src/lcu.rs`: League Client Update API 연결, lockfile 탐색, HTTP/WebSocket 처리
- `src/tray.rs`: 트레이 메뉴와 상태 문구
- `src/startup.rs`: Windows 부팅 시 자동 실행 등록
- `src/config.rs`: 설정 파일 로드, 저장, 정규화
- `build.rs`: Windows 실행 파일 리소스와 아이콘 설정

### 릴리스 체크리스트

1. `cargo test`가 통과하는지 확인합니다.
2. Windows에서 `cargo build --release`를 실행합니다.
3. 생성된 `target/release/lol-auto-accept.exe`를 GitHub Releases의 Assets에 첨부합니다.
