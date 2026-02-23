# CHANGELOG

OpenClaw Setup 주요 변경 이력을 기록합니다.

## 2026-02-23 (Security Hotfix)

### 11) ClawHub 연동 경로 비활성화
- 온보딩 대화의 `search_clawhub` 액션 경로를 비활성화했습니다.
- 런타임의 ClawHub API 호출 경로를 제거했습니다.
- 생성되는 BAT/SH에서 ClawHub CLI 설치/호출 경로를 제거했습니다.

### 12) ClawHub 없이 스킬 설치 재활성화
- ClawHub API/CLI는 계속 비활성화 상태를 유지합니다.
- 대신 생성되는 BAT/SH에서 OpenClaw CLI로 스킬 설치를 다시 활성화했습니다.
- 설치 fallback 체인:
  - `openclaw skill install ...`
  - `openclaw skills install ...`
  - `npx openclaw@latest skill install ...`
- 스크립트 로그에 `ClawHub disabled, using OpenClaw CLI only` 문구를 명시했습니다.

## 2026-02-22

### WSL/BAT 안정화 보강
- WSL 실행을 `wsl bash --noprofile --norc` 기준으로 통일해 쉘 초기화 충돌을 줄였습니다.
- BAT 문자열 확장/인용 문제로 인한 설치 중단 케이스를 완화했습니다.
- PATH 인용 처리 보강으로 `(x86)` 경로 포함 환경에서의 파싱 오류를 줄였습니다.

## 2026-02-21

### 설치/설정 플로우 보강
- 스킬 설치 및 활성화 경로를 정비했습니다.
- Node/OpenClaw 설치 및 재시도/오류 안내 흐름을 보강했습니다.
- cron 등록 명령 경로를 최신 CLI 규격에 맞게 정리했습니다.
- 채널 토큰/계정 바인딩 처리 흐름을 개선했습니다.

---

참고: 과거 일부 항목은 인코딩 손상으로 원문 복구가 어려워, 핵심 변경 요약으로 재작성했습니다.
