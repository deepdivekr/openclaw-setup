# CHANGELOG

이 문서는 온보딩/설치 스크립트의 주요 수정 사항을 기록합니다.

## 2026-02-21

### 1) 스킬 설치 단계 실패
- 버그 종류: `Installing skills...`에서 다수 스킬 설치 실패
- 원인: `openclaw skill install` 기반 가정이 실제 배포 CLI와 불일치
- 해결방안: `clawhub install` 경로로 전환하고, 설치 후 `openclaw config set "skills.entries.{id}.enabled" true`로 활성화 처리

### 2) Node.js / OpenClaw 설치 실패
- 버그 종류: WSL 환경에서 Node/OpenClaw 설치 단계 중단
- 원인: `nvm install --lts`/구버전 설치 경로와 환경별 해석 차이, 구버전 `openclaw` 글로벌 설치 경로 사용
- 해결방안: Node 설치를 `nvm install 22`로 고정하고 OpenClaw 설치를 `npm install -g openclaw@latest`로 통일

### 3) 크론 등록 실패
- 버그 종류: 스케줄 등록 후 반복 경고 또는 실제 미등록
- 원인: 기존 `cron add ... --agent` 포맷이 최신 CLI 스펙과 불일치
- 해결방안: `openclaw cron add --name --cron --session isolated --message --tz` 형식으로 변경

### 4) 에이전트/채널 설정 단계의 오탐 성공 로그
- 버그 종류: 내부 경고가 있었는데도 최종 `[OK]` 출력
- 원인: 개별 실패를 로그로만 소비하고 단계 결과에 반영하지 않음
- 해결방안: `CHANNEL_CONFIG_WARN` 플래그를 도입해 경고 발생 시 `[WARN]` 상태를 출력

### 5) 멀티 에이전트 텔레그램 토큰 충돌
- 버그 종류: 신규 에이전트 설치 후 기존 에이전트 대화/크론 알림 경로 꼬임
- 원인: 채널 토큰을 글로벌 `~/.openclaw/.env`에 저장하여 default account fallback 충돌 발생
- 해결방안: 채널 토큰의 env 저장을 제거하고, `openclaw channels add --channel <type> --account <agent> --token <token>` + `openclaw.json` account/binding 기반으로 분리

### 6) BAT/SH 경로 불일치로 인한 플랫폼별 재발
- 버그 종류: Windows에서는 해결됐지만 SH 경로에서 동일 문제 재발
- 원인: BAT와 SH 생성 로직이 비대칭으로 유지됨
- 해결방안: BAT/SH 모두 동일 정책으로 정렬(스킬 설치, 인증 저장 위치, 크론 등록, 채널 계정 바인딩, 상태 출력)

