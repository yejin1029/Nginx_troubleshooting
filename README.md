# Nginx 접속 불가 트러블슈팅 런북 (WSL2 Ubuntu)

WSL2(Ubuntu) 환경에서 Nginx를 설치/기동한 뒤,
의도적으로 장애를 재현하고(서비스 중지, 방화벽 차단, 설정 오류),
증거 수집 → 원인 분석 → 복구까지 수행한 트러블슈팅 런북입니다.

## 목표(산출물)
- Nginx 정상 접속 확인
- 장애 3종 재현 및 원인별 증거 수집
- 복구 및 재발 방지 포인트 정리
- tcpdump로 패킷 레벨 정상 동작 검증

---

## 환경
- OS: Ubuntu (WSL2)
- Nginx: `nginx/1.24.0 (Ubuntu)` (예시)
- 테스트 대상: `http://localhost:80`

> 참고: WSL2에서는 `localhost`가 IPv6(::1)로 우선 해석될 수 있어,
> IPv4/IPv6에 따라 차단 규칙 적용 여부가 달라질 수 있습니다.

---

## 1. 설치 및 정상 동작 확인

### 1) 설치/기동
```bash
sudo apt update && sudo apt -y upgrade
sudo apt -y install nginx
sudo systemctl enable --now nginx

### 2) 상태/리스닝/응답 확인
```bash
systemctl status nginx --no-pager
sudo ss -lntp | grep ':80'
curl -I http://localhost
