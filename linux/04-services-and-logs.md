# Linux 서비스와 로그 관리

## systemd

`systemd`는 Linux에서 여러 서비스를 시작하고 관리하는 프로그램이다.

systemd가 정상적으로 실행 중인지 확인한다.

```bash
systemctl is-system-running
```

- `running`: 정상 작동 중
- `degraded`: 일부 서비스에 문제가 있지만 systemd는 작동 중

## Nginx

Nginx는 웹 요청을 받아 웹페이지를 보내주는 웹 서버 프로그램이다.

이번 실습에서는 Nginx 자체를 깊게 배우는 것이 아니라, Linux에서 실제 서비스를 설치하고 관리하는 방법을 연습한다.

## 패키지 목록 갱신과 Nginx 설치

```bash
sudo apt update
sudo apt install nginx -y
```

- `sudo`: 관리자 권한으로 명령어 실행
- `apt update`: 설치 가능한 패키지 목록 갱신
- `apt install nginx`: Nginx 설치
- `-y`: 설치 확인 질문에 자동으로 동의

## 서비스 상태 확인

```bash
systemctl status nginx
```

주요 상태:

- `active (running)`: 서비스 실행 중
- `inactive (dead)`: 서비스 중지 상태
- `failed`: 서비스 실행 실패

상태 화면에서 빠져나오려면 `q`를 누른다.

## 서비스 시작, 중지, 재시작

```bash
sudo systemctl start nginx
sudo systemctl stop nginx
sudo systemctl restart nginx
```

- `start`: 서비스 시작
- `stop`: 서비스 중지
- `restart`: 서비스를 중지한 뒤 다시 시작

서비스를 제어한 다음에는 상태를 다시 확인한다.

```bash
systemctl status nginx
```

## 웹 응답 확인

```bash
curl -I http://localhost
```

- `curl`: 터미널에서 웹 요청을 전송
- `-I`: 웹페이지 본문을 제외하고 응답 헤더만 확인
- `localhost`: 현재 사용 중인 컴퓨터 자신
- `HTTP/1.1 200 OK`: 요청이 정상 처리됨
- `Server: nginx`: Nginx가 요청에 응답함

웹페이지의 HTML 내용까지 확인하려면 다음 명령어를 사용한다.

```bash
curl http://localhost
```

HTML 코드가 출력되면 Nginx가 정상적으로 웹페이지를 제공하고 있는 것이다.

## systemd 서비스 로그 확인

```bash
sudo journalctl -u nginx -n 20
```

- `journalctl`: systemd가 수집한 로그 확인
- `-u nginx`: Nginx 서비스의 로그만 확인
- `-n 20`: 최근 로그 20줄 출력

실시간으로 로그를 확인하려면 다음 명령어를 사용한다.

```bash
sudo journalctl -u nginx -f
```

실시간 로그 확인을 종료하려면 `Ctrl + C`를 누른다.

## Nginx 접속 로그 확인

```bash
sudo tail -n 20 /var/log/nginx/access.log
```

- `access.log`: 사용자가 Nginx에 보낸 요청 기록
- `tail -n 20`: 파일 마지막 20줄 확인

직접 요청을 보낸 뒤 로그를 확인할 수 있다.

```bash
curl -I http://localhost
sudo tail -n 20 /var/log/nginx/access.log
```

## Nginx 오류 로그 확인

```bash
sudo tail -n 20 /var/log/nginx/error.log
```

- `error.log`: Nginx 실행 중 발생한 오류 기록
- 문제가 생겼을 때 원인을 찾기 위해 확인한다.

## 서비스 장애 확인 기본 순서

웹 서비스에 문제가 생기면 다음 순서로 확인한다.

```text
1. 서비스 상태 확인
2. 웹 응답 확인
3. systemd 로그 확인
4. 접속 로그와 오류 로그 확인
```

사용하는 명령어:

```bash
systemctl status nginx
curl -I http://localhost
sudo journalctl -u nginx -n 20
sudo tail -n 20 /var/log/nginx/access.log
sudo tail -n 20 /var/log/nginx/error.log
```