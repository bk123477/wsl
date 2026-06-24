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

### 14-1. Git 설정

WSL 안에서 Git을 처음 쓴다면 사용자 이름과 이메일을 먼저 설정합니다.

```bash
git config --global user.name "Your Name"
git config --global user.email "you@example.com"
```

설정 확인:

```bash
git config --global --list
```

기본 브랜치 이름을 `main`으로 맞추고 싶다면:

```bash
git config --global init.defaultBranch main
```

줄바꿈 문제를 줄이기 위해 WSL 안에서는 아래 설정을 많이 사용합니다.

```bash
git config --global core.autocrlf input
```

이 설정은 WSL/Linux 환경에서 `CRLF`를 자동으로 만들지 않고, 커밋 시점에만 줄바꿈을 정리하는 쪽에 가깝습니다.

### 14-2. SSH 키 생성

GitHub 같은 원격 저장소에 비밀번호 대신 SSH로 접속하려면 SSH 키를 만듭니다.

```bash
ssh-keygen -t ed25519 -C "you@example.com"
```

보통 기본 경로 그대로 진행하면 아래 위치에 생성됩니다.

- 개인 키: `~/.ssh/id_ed25519`
- 공개 키: `~/.ssh/id_ed25519.pub`

SSH 에이전트를 시작하고 키를 등록:

```bash
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_ed25519
```

공개 키 내용 확인:

```bash
cat ~/.ssh/id_ed25519.pub
```

출력된 공개 키를 GitHub의 SSH keys 설정 화면에 등록합니다.

등록 후 연결 테스트:

```bash
ssh -T git@github.com
```

처음 연결할 때 아래처럼 서버 fingerprint 확인 메시지가 나올 수 있습니다.

```text
The authenticity of host '[ssh.github.com]:443' can't be established.
Are you sure you want to continue connecting?
```

이 경우 처음 접속하는 GitHub SSH 서버를 신뢰할 것인지 묻는 것이므로 `yes`를 입력하면 됩니다.

그 다음 아래와 같은 메시지가 나오면 정상입니다.

```text
Warning: Permanently added '[ssh.github.com]:443' ... to the list of known hosts.
Hi bk123477! You've successfully authenticated, but GitHub does not provide shell access.
```

의미는 다음과 같습니다.

- GitHub 계정 SSH 인증에 성공했다
- GitHub는 일반 리눅스 서버처럼 셸 접속은 제공하지 않는다
- 즉 오류가 아니라 정상 성공 메시지다

만약 현재 환경에서 GitHub SSH 접속이 기본 22번 포트가 아니라 443 포트에서만 성공한다면, 아래처럼 SSH 설정 파일을 만들어두면 이후 사용이 편합니다.

```bash
nano ~/.ssh/config
```

아래 내용을 추가합니다.

```sshconfig
Host github.com
  HostName ssh.github.com
  User git
  Port 443
  IdentityFile ~/.ssh/id_ed25519
```

`nano`에서 저장하는 방법:

```text
Ctrl + O
Enter
Ctrl + X
```

설정 후 다시 테스트:

```bash
ssh -T git@github.com
```

다시 아래와 같은 메시지가 나오면 정상입니다.

```text
Hi bk123477! You've successfully authenticated, but GitHub does not provide shell access.
```

기존 저장소의 원격 주소가 HTTPS로 되어 있다면, SSH로 바꿔두면 이후 `git push`가 더 편합니다.

현재 원격 주소 확인:

```bash
git remote -v
```

예를 들어 현재가 아래처럼 HTTPS라면:

```text
origin  https://github.com/bk123477/wsl.git (fetch)
origin  https://github.com/bk123477/wsl.git (push)
```

아래 명령으로 SSH 주소로 변경합니다.

```bash
git remote set-url origin git@github.com:bk123477/wsl.git
```

변경 확인:

```bash
git remote -v
```

정상이라면 아래처럼 보입니다.

```text
origin  git@github.com:bk123477/wsl.git (fetch)
origin  git@github.com:bk123477/wsl.git (push)
```

그 다음부터는 아래처럼 푸시할 수 있습니다.

```bash
git push
```

### 14-3. Node.js 설치

Node.js는 여러 설치 방법이 있지만, 버전 관리를 위해 `nvm` 사용이 편합니다.

먼저 `curl`이 없다면 설치:

```bash
sudo apt update
sudo apt install -y curl
```

그 다음 `nvm` 설치 스크립트 실행:

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/master/install.sh | bash
```

셸 설정을 다시 불러온 뒤:

```bash
source ~/.bashrc
```

LTS 버전 설치:

```bash
nvm install --lts
nvm use --lts
```

설치 확인:

```bash
node -v
npm -v
```

### 14-4. Python 설치

Ubuntu 계열에서는 보통 Python 3가 기본 제공되지만, 필요한 도구를 추가 설치하는 편이 좋습니다.

```bash
sudo apt update
sudo apt install -y python3 python3-pip python3-venv
```

설치 확인:

```bash
python3 --version
pip3 --version
```

프로젝트별 가상환경 예시:

```bash
python3 -m venv .venv
source .venv/bin/activate
```

가상환경을 빠져나오려면:

```bash
deactivate
```

### 14-5. Docker 사용

WSL에서 Docker를 쓰는 가장 쉬운 방법은 보통 Windows에 Docker Desktop을 설치하고 WSL 연동을 켜는 것입니다.

일반적인 흐름:

1. Windows에 Docker Desktop 설치
2. Docker Desktop 실행
3. Settings에서 WSL Integration 활성화
4. 사용할 배포판(Ubuntu 등)을 켜기

그 뒤 WSL 안에서 확인:

```bash
docker --version
docker ps
```

만약 `docker` 명령이 없다면 Docker Desktop이 아직 설치되지 않았거나 WSL 연동이 꺼져 있을 가능성이 큽니다.

### 14-6. VS Code와 WSL 연동

WSL과 VS Code를 같이 쓰면 매우 편합니다.

보통 필요한 준비는 아래와 같습니다.

1. Windows에 VS Code 설치
2. VS Code의 Remote - WSL 확장 설치
3. WSL 터미널에서 프로젝트 폴더로 이동
4. 아래 명령 실행

```bash
code .
```

그러면 VS Code가 WSL 환경에 연결된 상태로 열립니다.

VS Code가 `code` 명령을 인식하지 못하면:

- VS Code가 설치되지 않았거나
- Remote - WSL 확장이 없거나
- PATH 반영이 아직 안 되었을 수 있습니다

### 14-7. `.bashrc` 또는 `.zshrc` 설정

자주 쓰는 별칭(alias), 환경 변수, 시작 명령은 셸 설정 파일에 넣어두면 편합니다.

대부분 Ubuntu 기본 셸은 `bash`이므로 우선 `.bashrc`를 많이 사용합니다.

파일 열기:

```bash
nano ~/.bashrc
```

예시로 아래 내용을 추가할 수 있습니다.

```bash
alias ll="ls -alF"
alias la="ls -A"
alias gs="git status"
alias gp="git pull"
alias gc="git commit"
```

적용:

```bash
source ~/.bashrc
```

현재 기본 셸 확인:

```bash
echo $SHELL
```

만약 `zsh`를 설치해 사용한다면 설정 파일은 보통 `~/.zshrc`입니다.

### 14-8. 추천 순서

처음에는 아래 순서로 진행하면 편합니다.

1. Git 사용자 정보 설정
2. SSH 키 생성 및 GitHub 등록
3. VS Code와 WSL 연결 확인
4. Node.js 또는 Python 중 필요한 런타임 설치
5. Docker Desktop 연동
6. `.bashrc` 또는 `.zshrc`에 자주 쓰는 설정 추가

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
