# NOX Releases (nox-releases)

[한국어](#한국어) | [English](#english)

---

## 한국어

### NOX 소개

**NOX**는 마이크로서비스 아키텍처 기반의 차세대 네트워크 비디오 레코더(NVR) / 영상 관리 시스템(VMS)입니다.  
표준 중심(ONVIF, H.264/H.265 등)으로 구성되며, **웹 브라우저에서 플러그인 없이(HLS/WebRTC)** 실시간/재생을 제공하고, **Docker 기반으로 빠르게 설치/운영**할 수 있도록 설계되었습니다.

### 설치 개요

1) 기반 설정(필수 패키지, Docker/Compose, HEVC 디코딩 드라이버, Chrome)  
2) 릴리즈 스크립트(`deploy.sh`)로 설치
3) 토큰은 파트너사별로 별도로 전달됩니다. [Support](#Support) 를 통해 요청해 주세요.

---

## 설치

> **권장 OS**: Ubuntu (x86_64)  
> **주의**: 아래 명령은 시스템에 Docker 및 드라이버를 설치/변경합니다.

### 1) 기반 설정

```bash
# curl 설치
sudo apt install -y curl

# Docker 설치
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
newgrp docker

# Docker Compose 플러그인 설치
sudo apt update
sudo apt install -y docker-compose-plugin

# HEVC 디코딩 문제 해결
#sudo apt install -y mesa-va-drivers vainfo
sudo apt install -y intel-media-va-driver-non-free libva2 vainfo

sudo usermod -aG render nox
sudo usermod -aG video nox

# Chrome 설치
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && sudo apt install -y ./google-chrome-stable_current_amd64.deb
```

### 2) NOX 설치

> 아래 설치는 GitHub Release의 `deploy.sh`를 내려받아 실행합니다.  
> `GITHUB_TOKEN`에는 벤더에서 발급한 토큰(예: Vendor Token)을 넣어주세요.

```bash
export VERSION=26.04-rc1
curl -fsSL "https://github.com/yiyolcorp/nox-releases/releases/download/$VERSION/deploy.sh" \
  | sudo GITHUB_TOKEN=<VENDOR_TOKEN> bash -s -- --clean-install --version $VERSION
```

---

## 설치 후 확인 (예시)

```bash
docker ps
```

- 주요 컨테이너가 `Up` 상태인지 확인하세요.

---

## 접속

- NOX UI 접속 주소는 설치 스크립트 출력(로그)에 안내됩니다.
- 서버/PC에서 Chrome으로 접속해 라이브 모니터링 및 재생 기능을 사용합니다.

---

## 트러블슈팅

### Docker 권한 오류가 나요 (permission denied)

```bash
sudo usermod -aG docker $USER
newgrp docker
```

- 터미널을 새로 열거나 재로그인하면 적용됩니다.

### HEVC(H.265) 재생/디코딩이 이상해요

- 아래 패키지가 설치되어 있는지 확인하세요.

```bash
dpkg -l | egrep "intel-media-va-driver|libva2|vainfo"
vainfo
```

### GITHUB_TOKEN 관련 오류가 나요

- 토큰이 올바른지(문자 누락/공백) 확인하세요.
- 벤더 토큰의 권한/만료 여부를 확인하세요.

---

## 업그레이드 / 재설치

- 버전만 바꿔서 동일하게 실행하면 됩니다.
- `--clean-install`은 기존 구성을 정리하고 재설치할 수 있습니다.

```bash
export VERSION=<NEW_VERSION>
curl -fsSL "https://github.com/yiyolcorp/nox-releases/releases/download/$VERSION/deploy.sh" \
  | sudo GITHUB_TOKEN=<VENDOR_TOKEN> bash -s -- --clean-install --version $VERSION
```

- 업그레이드 시에는 `--upgrade`를 명시해서 실행하면 됩니다.
```bash
export VERSION=<NEW_VERSION>
curl -fsSL "https://github.com/yiyolcorp/nox-releases/releases/download/$VERSION/deploy.sh" \
  | sudo GITHUB_TOKEN=<VENDOR_TOKEN> bash -s -- --upgrade --version $VERSION
```
---

## Support

- 이슈/문의: yiyo@yiyol.com

---

## English

### About NOX

**NOX** is a next-generation Network Video Recorder (NVR) / Video Management System (VMS) built on a microservices architecture.  
It focuses on standards (e.g., ONVIF, H.264/H.265) and provides **plugin-free web playback via HLS/WebRTC**, with **fast installation and operations using Docker**.

### Installation Flow

1) Base setup (packages, Docker/Compose, HEVC decode drivers, Chrome)  
2) Install via the release script (`deploy.sh`)
3) You can get a token via [Support](#Support).

---

## Install

> **Recommended OS**: Ubuntu (x86_64)  
> **Note**: The commands below will install/modify Docker and video driver packages.

### 1) Base Setup

```bash
# Install curl
sudo apt install -y curl

# Install Docker
curl -fsSL https://get.docker.com | sudo sh
sudo usermod -aG docker $USER
newgrp docker

# Install Docker Compose plugin
sudo apt update
sudo apt install -y docker-compose-plugin

# Fix HEVC (H.265) decoding issues
#sudo apt install -y mesa-va-drivers vainfo
sudo apt install -y intel-media-va-driver-non-free libva2 vainfo

sudo usermod -aG render nox
sudo usermod -aG video nox

# Install Chrome
wget https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb && sudo apt install -y ./google-chrome-stable_current_amd64.deb
```

### 2) Install NOX

> This downloads and runs `deploy.sh` from GitHub Releases.  
> Set `GITHUB_TOKEN` to the vendor-issued token.

```bash
export VERSION=26.04-rc1
curl -fsSL "https://github.com/yiyolcorp/nox-releases/releases/download/$VERSION/deploy.sh" \
  | sudo GITHUB_TOKEN=<VENDOR_TOKEN> bash -s -- --clean-install --version $VERSION
```

---

## Verify (example)

```bash
docker ps
```

- Make sure the main containers are `Up`.

---

## Access

- The UI URL is printed in the installer output (logs).
- Open it with Chrome to use live monitoring and playback.

---

## Troubleshooting

### Docker permission denied

```bash
sudo usermod -aG docker $USER
newgrp docker
```

- Reopen the terminal or re-login to apply.

### HEVC (H.265) playback/decoding issues

```bash
dpkg -l | egrep "intel-media-va-driver|libva2|vainfo"
vainfo
```

### GITHUB_TOKEN errors

- Check the token string (missing chars / whitespace).
- Verify the token is valid and not expired, and has required access.

---

## Upgrade / Reinstall

- Run the same command with a different version.
- `--clean-install` can wipe and reinstall.

```bash
export VERSION=<NEW_VERSION>
curl -fsSL "https://github.com/yiyolcorp/nox-releases/releases/download/$VERSION/deploy.sh" \
  | sudo GITHUB_TOKEN=<VENDOR_TOKEN> bash -s -- --clean-install --version $VERSION
```

- Run with `--upgrade` for upgrading.
```bash
export VERSION=<NEW_VERSION>
curl -fsSL "https://github.com/yiyolcorp/nox-releases/releases/download/$VERSION/deploy.sh" \
  | sudo GITHUB_TOKEN=<VENDOR_TOKEN> bash -s -- --upgrade --version $VERSION
```

---

## Support

- Issues / Contact: yiyo@yiyol.com
