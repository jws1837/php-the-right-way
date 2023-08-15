---
title:   리눅스(Linux)에서 사용하기
isChild: true
anchor:  windows_setup
---

## 리눅스(Linux)에서 사용하기 {#linux_setup_title}

대부분의 GNU/Linux 배포판에는 공식 리포지토리에서 사용할 수 있는 PHP가 함께 제공되지만, 이러한 패키지는 일반적으로 현재 안정 버전보다 약간 뒤쳐져 있습니다. 이러한 배포판에서 최신 PHP 버전을 얻는 방법에는 여러 가지가 있습니다. 예를 들어 우분투와 데비안 기반 GNU/Linux 배포판의 경우, 기본 패키지를 대체할 수 있는 가장 좋은 방법은 [Ondřej Surý][Ondrej Sury Blog]가 우분투의 개인 패키지 아카이브(PPA)와 데비안의 DPA/bikeshed를 통해 제공하고 유지 관리하는 것입니다. 아래에서 각각에 대한 지침을 확인하십시오. 즉, 언제든지 컨테이너를 사용하고 PHP 소스 코드를 컴파일하는 등의 작업을 할 수 있습니다.

### 우분투 기반 배포판

우분투 배포판의 경우, [PPA by Ondřej Surý][Ondrej Sury PPA]는 지원되는 PHP 버전들과 많은 PECL 확장들을 제공합니다. 이 PPA를 당신의 시스템에 추가하려면 터미널에서 다음 단계를 수행하십시오:

1. 먼저 다음 명령을 사용하여 당신의 시스템 소프트웨어 소스에 PPA를 추가합니다:

   ```bash 
   sudo add-apt-repository ppa:ondrej/php
   ```

2. PPA를 추가한 후 시스템의 패키지 목록을 업데이트합니다:

   ```bash
   sudo apt update
   ```

이렇게 하면 시스템이 PPA에서 사용 가능한 최신 PHP 패키지에 액세스하고 설치할 수 있습니다.

#### 데비안 기반 배포판

데비안 기반 배포판의 경우, Ondřej Surý는 [bikeshed][bikeshed] (데비안에서 PPA에 해당하는 것)도 제공합니다. 시스템에 bikeshed를 추가하고 업데이트하려면 다음 단계를 따르세요:

1. 루트 액세스 권한이 있는지 확인합니다. 그렇지 않은 경우 다음 명령에 `sudo`를 사용해야 할 수 있습니다.

2. 시스템의 패키지 목록을 업데이트합니다:

   ```bash
   sudo apt-get update
   ```

3. `lsb-release`, `ca-certificates`, `curl`을 설치합니다:

   ```bash
   sudo apt-get -y install lsb-release ca-certificates curl
   ```

4. 리포지토리에 대한 서명 키를 다운로드합니다:

   ```bash
   sudo curl -sSLo /usr/share/keyrings/deb.sury.org-php.gpg https://packages.sury.org/php/apt.gpg
   ```

5. 시스템의 소프트웨어 소스에 리포지토리를 추가합니다:

   ```bash
   sudo sh -c 'echo "deb [signed-by=/usr/share/keyrings/deb.sury.org-php.gpg] https://packages.sury.org/php/ $(lsb_release -sc) main" > /etc/apt/sources.list.d/php.list'
   ```

6. 마지막으로 시스템의 패키지 목록을 다시 업데이트합니다:

   ```bash
   sudo apt-get update
   ```

이 단계를 수행하면 시스템이 bikeshed에서 최신 PHP 패키지를 설치할 수 있습니다.

[Ondrej Sury Blog]: https://deb.sury.org/
[Ondrej Sury PPA]: https://launchpad.net/~ondrej/+archive/ubuntu/php
[bikeshed]: https://packages.sury.org/php/