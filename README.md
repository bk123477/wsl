# WSL 설치 및 사용 가이드

Windows에서 WSL(Windows Subsystem for Linux)을 처음 설치하고, 바로 사용할 수 있을 때까지 필요한 단계를 정리한 문서입니다.

이 문서는 `2026-06-25` 기준 Microsoft Learn의 WSL 공식 문서를 참고해 작성했습니다.

## 1. WSL이 무엇인지 간단히 이해하기

WSL은 Windows 안에서 Linux 환경을 실행할 수 있게 해주는 기능입니다.  
보통 개발용 터미널, 패키지 설치, 쉘 스크립트 실행, Linux 기반 도구 사용을 위해 많이 씁니다.

## 2. 설치 전에 확인할 것

다음 조건을 먼저 확인합니다.

- Windows 11 또는 WSL 2를 지원하는 Windows 10이어야 합니다.
- 관리자 권한으로 PowerShell 또는 Windows Terminal을 열 수 있어야 합니다.
- 설치 도중 재부팅이 필요할 수 있습니다.
- 회사 PC라면 관리자 정책 때문에 Microsoft Store 또는 선택 기능 설치가 막혀 있을 수 있습니다.

Windows 버전을 확인하려면 `Win + R`을 누른 뒤 `winver`를 입력합니다.

## 3. 가장 쉬운 설치 방법

가장 간단한 방법은 관리자 권한 PowerShell에서 아래 명령을 실행하는 것입니다.

```powershell
wsl --install
```

이 명령은 보통 아래 작업을 한 번에 처리합니다.

- WSL 관련 Windows 기능 활성화
- 가상 머신 플랫폼 활성화
- 기본 Linux 배포판 설치

설치가 끝나면 재부팅하라는 안내가 나올 수 있습니다.  
안내가 나오면 Windows를 재부팅합니다.

## 4. 특정 배포판을 선택해서 설치하고 싶을 때

설치 가능한 배포판 목록을 먼저 보려면:

```powershell
wsl --list --online
```

예를 들어 Ubuntu를 직접 지정해서 설치하려면:

```powershell
wsl --install -d Ubuntu
```

다른 배포판 예시:

- `Debian`
- `Ubuntu-24.04`
- `openSUSE-Tumbleweed`

배포판 이름은 반드시 `wsl --list --online` 결과에 나온 이름 그대로 사용합니다.

## 5. 설치가 끝난 뒤 첫 실행

재부팅 후 다음 중 하나로 Linux를 실행합니다.

- 시작 메뉴에서 `Ubuntu` 같은 설치된 배포판 실행
- PowerShell에서 `wsl` 입력
- 특정 배포판을 실행하려면 `wsl -d Ubuntu`

처음 실행하면 Linux 사용자 계정 생성을 묻습니다.

예시:

1. 사용자 이름 입력
2. 비밀번호 입력
3. 비밀번호 한 번 더 입력

여기서 만든 계정은 Linux 내부 계정입니다.  
Windows 로그인 계정과 달라도 됩니다.

## 6. 설치 확인

아래 명령으로 설치 상태를 확인합니다.

```powershell
wsl --status
```

설치된 배포판과 WSL 버전을 확인하려면:

```powershell
wsl --list --verbose
```

또는 짧게:

```powershell
wsl -l -v
```

정상이라면 배포판 이름과 함께 `VERSION 2`가 보이는 경우가 많습니다.

## 7. WSL 업데이트

최신 WSL 구성 요소로 업데이트하려면 관리자 PowerShell에서:

```powershell
wsl --update
```

업데이트 후 WSL을 완전히 종료했다가 다시 시작하려면:

```powershell
wsl --shutdown
```

그 다음 다시 `wsl`을 실행합니다.

## 8. 처음 들어가서 바로 하면 좋은 작업

WSL에 처음 들어간 뒤 패키지 목록과 설치된 패키지를 최신 상태로 맞춥니다.

Ubuntu 또는 Debian 계열이라면:

```bash
sudo apt update
sudo apt upgrade -y
```

기본 도구 몇 가지도 함께 설치해두면 편합니다.

```bash
sudo apt install -y git curl wget unzip build-essential
```

## 9. 자주 쓰는 기본 명령

### Windows PowerShell에서 실행

설치된 배포판 목록 보기:

```powershell
wsl -l -v
```

기본 배포판 변경:

```powershell
wsl --set-default Ubuntu
```

특정 배포판을 WSL 2로 설정:

```powershell
wsl --set-version Ubuntu 2
```

특정 배포판 실행:

```powershell
wsl -d Ubuntu
```

현재 실행 중인 WSL 종료:

```powershell
wsl --shutdown
```

### Linux 터미널 안에서 실행

현재 위치 확인:

```bash
pwd
```

파일 목록 보기:

```bash
ls -la
```

홈 디렉터리로 이동:

```bash
cd ~
```

Windows 드라이브 접근:

```bash
cd /mnt/c
```

예를 들어 현재 프로젝트 폴더로 이동:

```bash
cd /mnt/c/Users/SKTelecom/github/wsl
```

## 10. Windows 파일과 WSL 파일의 차이

WSL에서는 Windows 드라이브가 보통 `/mnt/c`, `/mnt/d`처럼 마운트됩니다.

예시:

- Windows의 `C:\Users\SKTelecom` -> WSL의 `/mnt/c/Users/SKTelecom`

개발할 때는 상황에 따라 두 방식 중 하나를 고를 수 있습니다.

- Windows 쪽 파일을 `/mnt/c/...` 경로로 직접 작업
- Linux 홈 디렉터리(`~/project`)에 프로젝트를 두고 작업

처음에는 이해하기 쉬운 `/mnt/c/Users/SKTelecom/...` 경로부터 시작해도 충분합니다.

## 11. 종료와 재시작

Linux 셸만 종료하려면:

```bash
exit
```

WSL 전체를 종료하려면 Windows PowerShell에서:

```powershell
wsl --shutdown
```

다시 시작하려면:

```powershell
wsl
```

## 12. 잘 안 될 때 먼저 확인할 것

### `wsl --install`이 동작하지 않을 때

관리자 권한 PowerShell인지 먼저 확인합니다.

그래도 안 되면 Windows 기능을 수동으로 켜야 할 수 있습니다.

```powershell
dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
```

그 뒤 Windows를 재부팅하고 다시 시도합니다.

### `WSL 2 requires an update to its kernel component` 비슷한 오류가 날 때

아래 명령을 실행합니다.

```powershell
wsl --update
```

### 설치된 배포판이 안 뜰 때

아래 명령으로 상태를 확인합니다.

```powershell
wsl -l -v
wsl --status
```

## 13. 처음 설치 후 추천 흐름

처음에는 아래 순서로 진행하면 됩니다.

1. 관리자 PowerShell 열기
2. `wsl --install` 실행
3. 재부팅
4. `wsl` 또는 `Ubuntu` 실행
5. Linux 사용자 계정 생성
6. `sudo apt update && sudo apt upgrade -y` 실행
7. 필요하면 `git`, `curl`, `build-essential` 설치
8. 프로젝트 폴더를 `/mnt/c/...`에서 열어보기

## 14. 다음에 해볼 수 있는 것

WSL 설치가 끝나면 보통 다음 작업을 이어서 합니다.

- Git 설정
- SSH 키 생성
- Node.js, Python, Docker 등 개발 도구 설치
- VS Code와 WSL 연동
- `.bashrc` 또는 `.zshrc` 설정

## 15. 참고한 공식 문서

- Microsoft Learn: WSL 설치 가이드  
  https://learn.microsoft.com/en-us/windows/wsl/install
- Microsoft Learn: WSL 기본 명령  
  https://learn.microsoft.com/en-us/windows/wsl/basic-commands
- Microsoft Learn: 수동 설치 가이드  
  https://learn.microsoft.com/en-us/windows/wsl/install-manual

---

다음 단계는 이 문서대로 실제로 하나씩 진행해보는 것입니다.  
진행하면서 막히는 화면, 오류 메시지, 또는 문서에 보완할 점이 보이면 그때 README를 계속 수정하면 됩니다.
