<div align="center">

# 🦞 OpenClaw 딸깍 셋업

**대화 한 번이면, 세팅 끝.**

에이전트와 대화하면서 세팅하고, 파일 하나 더블클릭하면<br>
Telegram에 AI 비서가 인사를 보냅니다.

`5,326줄` · `217KB` · `10개 모듈` · `89개 함수` · `51 Stage 완료` · **단일 HTML. 외부 의존성 0.**

<br>

[🚀 지금 시작하기](https://deepdivekr.github.io/openclaw-setup/openclaw-setup.html) · [📥 오프라인 HTML 다운로드](https://github.com/deepdivekr/openclaw-setup/raw/main/openclaw-setup.html)

---

</div>

<br>
## 🔔 중요 업데이트

### 2026-02-22

사용자 실측 로그 기준으로 WSL/BAT 재발 이슈를 추가 보강했습니다.

- 모든 WSL 실행 명령을 `wsl bash --noprofile --norc`로 통일해, 사용자 `.profile` 문법 오류가 설치 플로우를 깨지 않도록 수정
- BAT 내부 `printf '%s...'` 패턴을 제거해 `%` 확장으로 명령이 변형되는 문제를 차단
- OpenClaw 설치/탐지 단계의 PATH 처리 및 수동 복구 안내 문구를 안정적인 형태로 정리

### 2026-02-21

초기 온보딩 실패 이슈를 반영해 설치 스크립트(BAT/SH)를 수정했습니다.

- 크론 등록 명령을 최신 OpenClaw CLI 형식으로 수정
- 스킬 설치/인증 경로를 `clawhub` + `openclaw config set skills.entries...` 기준으로 통일
- 채널 토큰은 **글로벌 `.env` 저장을 제거**하고, `channels add --account` + `openclaw.json` account/binding 기반으로 분리
- 채널/에이전트 설정 단계에서 경고 발생 시 무조건 `[OK]`를 찍지 않도록 상태 표시 개선

자세한 변경 내역은 루트 `CHANGELOG.md`를 참고하세요.


## 💡 사전 준비 (꼭 읽어주세요)

이 도구는 **설정 마법사일 뿐입니다.** 실제 OpenClaw 설치가 진행되므로, 다음 조건을 먼저 확인하세요:

### 필수 환경
- **Windows 사용자**: WSL2가 없어도 됩니다. bat 파일이 자동으로 설치합니다. 다만 **인터넷 연결 필수** (WSL2, Ubuntu, Node.js, npm 다운로드)
- **Mac/Linux 사용자**: 터미널 접근 + bash + 기본 빌드 도구 필요
- **인터넷**: Phase 1~설치까지 필요 (AI API 테스트, OpenClaw npm 설치, 스킬 다운로드)

### 추천 설정 (시작 전 준비하세요)
가장 간단한 조합은 **Telegram + Google Gemini**입니다:

| 항목 | 추천 | 이유 |
|------|------|------|
| **채널** | Telegram | 봇 생성 간단 (@BotFather로 5초), 모바일 바로 사용 가능 |
| **AI 모델** | Google Gemini | 무료 티어 넉넉함 (매월 60 요청), CORS 지원으로 HTML 내 직접 API 호출 가능 |

**준비 단계:**
1. [@BotFather](https://t.me/botfather)에서 Telegram 봇 생성 → 토큰 얻기
2. [Google AI Studio](https://aistudio.google.com/apikey)에서 Gemini API 키 발급 (무료)
3. 이 페이지 열기

<br>

## 구현 현황

| 항목 | 상태 | 노트 |
|------|------|------|
| **Phase 1 온보딩** | ✅ 완료 | Step 0~4: 기본정보 + 채널 + AI 모델 + 보안레벨 |
| **Phase 2 AI 에이전트** | ✅ 완료 | 스킬 브라우저 + 자동화 추천 + 실시간 대화 |
| **스킬 시스템** | ✅ 완료 | 50개 내장 + ClawHub 실시간 검색 |
| **보안 스캐너** | ✅ 완료 | CRITICAL/HIGH/MEDIUM 자동 탐지 |
| **설정 재사용** | ✅ 완료 | 기존 config 불러오기 + delta 모드 |
| **bat/sh 생성** | ✅ 완료 | WSL2 자동 설치 + nvm + OpenClaw 전체 자동화 |
| **멀티에이전트** | ✅ 완료 | 에이전트별 경로 격리 + 공유 게이트웨이 |
| **토큰 관리** | ✅ 완료 | 채널별 스키마 자동 분기 + 계정/바인딩 자동 구성 |

<br>

## 왜 만들었나

[OpenClaw](https://openclaw.ai)는 강력합니다. Telegram이나 Slack으로 나만의 AI 에이전트를 운영할 수 있고, 스킬과 자동화로 이메일 정리부터 보안 감사까지 돌릴 수 있습니다.

문제는 **세팅이 어렵다**는 것입니다.

터미널 열고, `openclaw.json` 수정하고, API 키 환경변수 잡고, 스킬 하나하나 설치하고, 크론 표현식 작성하고, SOUL.md에 보안 규칙 넣고, MEMORY.md 초기화하고... 개발자가 아니면 시작 전에 포기합니다. 개발자라도 30분은 걸립니다.

OpenClaw 생태계에 5,700개 이상의 스킬이 등록되어 있고 커뮤니티는 폭발적으로 커지고 있지만, 이 모든 것의 진입점이 **터미널과 JSON 파일**입니다. 비개발자 시장은 사실상 닫혀 있었습니다.

**딸깍 셋업은 이 문제를 정면으로 해결합니다.**

웹페이지에서 AI와 대화 → 파일 다운로드 → 더블클릭 → 끝. 세팅 페이지 자체가 에이전트의 첫 대화이고, 그 대화가 에이전트의 최초 기억이 됩니다.

<br>

## 사용 방법

### 1단계: 웹페이지 접속

[deepdivekr.github.io/openclaw-setup/openclaw-setup.html](https://deepdivekr.github.io/openclaw-setup/openclaw-setup.html)을 엽니다 (또는 [오프라인 HTML 다운로드](https://github.com/deepdivekr/openclaw-setup/raw/main/openclaw-setup.html) 후 브라우저에서 더블클릭).

다크 테마의 채팅 인터페이스가 나타납니다.

### 2단계: Phase 1 — 기본 정보 입력 (5분)

시스템 봇이 다음을 물어봅니다:
- **이름/직업/타임존/언어** — 에이전트가 당신을 파악
- **에이전트 이름 + 성격** — "근무 중심 보조", "친근한 동료" 등
- **채널 선택** — Telegram / Discord / Slack (추천: Telegram)
- **AI 모델 선택** — 13개 프로바이더, 48개 모델 (추천: Google Gemini)
- **보안 레벨** — 최소 / 보통 / 최대 (에이전트가 자동화할 수 있는 범위)

버튼을 누르거나 텍스트를 입력하면 됩니다. 실수한 경우 **"← 이전 단계" 버튼**으로 돌아갈 수 있습니다.

기존 설정이 있다면 **"기존 설정 불러오기"** 선택 → Phase 1을 건너뛸 수 있습니다.

### 3단계: Phase 2 — 내 AI 에이전트가 깨어남 (3분)

API 키를 입력하는 순간 화면이 전환됩니다. 🦞 로고와 함께 **내 에이전트가 직접 대화를 시작합니다.**

**스킬 브라우저:** 카테고리별 스킬이 표시됩니다. 직접 고르거나, "원하는 기능 직접 설명"을 선택할 수 있습니다:
> "매일 아침 이메일 정리해주고, 미팅 끝나면 할 일 뽑아줘."

에이전트가 필요한 스킬과 자동화를 추천하고, 사이드 패널에 실시간으로 반영합니다.

**인증 처리:** 인증이 필요한 스킬은 토큰 발급 방법을 안내하고, 발급 페이지를 바로 열어줍니다.

### 4단계: 파일 다운로드 → 실행

세팅이 끝나면 OS에 맞는 파일을 다운로드합니다.

**Windows 사용자:**
```
setup-에이전트이름.bat 더블클릭
```
- WSL2 미설치 → 자동 설치 + 재부팅
- Ubuntu 배포판 미설치 → 자동 설치
- Node.js 미설치 → nvm으로 자동 설치
- OpenClaw 미설치 → npm으로 자동 설치

**Mac/Linux 사용자:**
```bash
bash setup-에이전트이름.sh
```

**실행 시간:** 3~5분 후, Telegram에 에이전트가 인사 메시지를 보냅니다.

<br>

## 작동 원리

### 아키텍처

서버 없는 단일 HTML 파일 안에 10개 모듈, 89개 함수가 유기적으로 동작합니다.

```
openclaw-setup.html (217KB, 5,326줄)
│
├─ Config           13개 프로바이더 · 48개 모델 · 58개 스킬 · 8개 크론잡
├─ State            경로 기반 반응형 스토어 (구독/알림 패턴)
├─ EventBus         모듈 간 비동기 이벤트 통신
├─ Renderer         채팅 UI (타이핑 애니메이션, 사이드 패널, 입력 컨트롤 7종)
├─ ChatEngine       대화 흐름 오케스트레이터 (Phase 전환, 최종 요약)
├─ SystemBot        Phase 1: 스크립트 기반 질문 (Step 0~4)
├─ AIAgent          Phase 2: AI API 호출 + 스킬 브라우저 + 인증 + ClawHub 검색
├─ FileGenerator    bat/sh + config.json + SOUL.md + MEMORY.md + IDENTITY.md + skill-scanner 생성
├─ SecurityScanner  스킬 보안 스캔 + 위험도 자동 판정
└─ App              초기화, 설정 저장/불러오기, beforeunload 경고
```

### 두 개의 Phase

**Phase 1 (Step 0~4) — 시스템 봇.** AI 호출 없이 미리 정의된 질문을 순서대로 진행합니다. 이름, 직업, 에이전트 이름/성격, 채널, AI 프로바이더/모델/API 키를 수집합니다. 모델 선택은 2단계 UI(그룹 → 모델)로, 퍼지 검색 자동완성과 직접 입력을 모두 지원합니다.

**Phase 전환.** API 키 입력 시 프로바이더별 엔드포인트에 실제 테스트 요청을 보내 유효성을 검증합니다. CLI OAuth 프로바이더(Anthropic, OpenAI)의 경우 "CLI OAuth로 인증" 옵션도 제공되며, 이때는 AI 테스트를 건너뛰고 bat/sh 실행 시점에 `openclaw auth login`으로 인증합니다. 검증 통과 후 화면 효과와 함께 봇 아바타가 🦞로 바뀌고 Phase 2로 넘어갑니다.

**Phase 2 (Step 5~) — 실제 AI 에이전트.** 모든 대화가 사용자가 선택한 AI의 실제 응답입니다. 에이전트는 JSON 형식으로 응답하며, 프론트엔드가 `actions` 배열을 파싱하여 스킬 체크, 크론 추가, 보안 설정, 토큰 요청, ClawHub 검색, 에이전트 추가 등을 실행합니다. 대화가 끝나면 인증이 필요한 스킬을 순회하며 토큰 입력 또는 "나중에" 선택을 받습니다.

**수동 모드 (CORS 미지원 프로바이더 선택 시):** CORS 미지원 프로바이더를 선택하면 "수동 모드"로 전환되어 AI 대화 없이도 다음을 직접 설정할 수 있습니다:
- 스킬 브라우저에서 필요한 스킬 선택
- 보안 레벨 선택 (최소/보통/최대)
- 크론잡 선택 및 설정
- 추가 에이전트 설정

이 경우에도 모든 설정이 bat/sh에 포함되어 실행 시 동일하게 적용됩니다.

### 프로바이더별 API 분기

13개 프로바이더를 지원하며, 각각 엔드포인트, 헤더, 요청/응답 형식이 다릅니다:

| 프로바이더 | 모델 예시 | CORS | 추천도 | 비고 |
|-----------|-----------|------|-------|------|
| **🌟 Google** | **Gemini 3 Pro, 3 Flash, 2.5 Pro** | ✅ | ⭐⭐⭐ | **무료 티어 넉넉함 (월 60요청). 이것이 정답.** |
| ⭐ Anthropic | Claude Opus 4.6, Sonnet 4.6, Haiku 4.5 | ✅ | ⭐⭐ | CLI OAuth 지원 (설치 후 인증 가능) |
| ⭐ OpenRouter | Auto (자동 라우팅) | ✅ | ⭐⭐ | 여러 모델 중계 |
| ⭐ Ollama | Llama 3.3, DeepSeek R1, Mistral, Qwen | ✅ | ⭐⭐ | 로컬 실행 (무료, 인터넷 불필요) |
| OpenAI | GPT-5.2, GPT-5, o3-mini | ❌ | ⭐ | 유료 (최소 $20), CORS 미지원 |
| Groq | Llama 3.3 70B, Llama 4 Scout, Maverick | ❌ | ◯ | 초고속 추론 |
| DeepSeek | DeepSeek V3.2, R1 | ❌ | ◯ | 성능 최적화 |
| Mistral | Large 3, Medium 3, Small, Codestral | ❌ | ◯ | 오픈소스 기반 |
| xAI | Grok 4.1, Grok 3, Grok 3 Mini | ❌ | ◯ | 실시간 학습 |
| Moonshot | Kimi K2.5, Moonshot 128K/32K | ❌ | ◯ | 긴 문맥 지원 |
| Zhipu AI | GLM-4 Plus, Flash, Long | ❌ | ◯ | 중국 프로바이더 |
| Cohere | Command A, R+, R | ❌ | ◯ | 엔터프라이즈 기반 |
| Together AI | Llama 3.3 70B, DeepSeek R1, Qwen 2.5 | ❌ | ◯ | 오픈소스 모델 |

**선택 기준:**
- **CORS ✅** — 브라우저 직접 통신 가능 (HTML에서 AI 대화)
- **CORS ❌** — "수동 모드"로 전환 (AI 대화 없이도 스킬·보안·크론 직접 설정 가능)

Phase 2 대화에는 사용자가 선택한 모델이 사용되며, API 호출 시 `provider/model-id` 형식에서 provider 접두사를 자동 제거합니다.

### 스킬 시스템

58개 내장 스킬이 11개 카테고리로 분류되어 있습니다. 각 스킬에는 ClawHub 실제 다운로드 수, 인증 방식 (없음/API Key/OAuth/PAT/CLI), 설명, 키 발급 URL이 메타데이터로 포함되어 있습니다.

| 카테고리 | 스킬 예시 |
|---------|----------|
| 📋 생산성 | Gmail+Calendar+Drive, Notion, Himalaya(IMAP), Todoist, Trello, Apple 미리알림, Things 3 |
| 💻 개발 | GitHub, 스킬 생성 도구, Tmux, 세션 로그, Docker, Vercel, Supabase |
| 🔧 유틸리티 | 요약 도구, 날씨, PDF 편집, 모델 사용량, MCP 서버, 세컨드 모델 리뷰, Google Places |
| 🤖 AI·에이전트 | 자기 개선 에이전트, Gemini CLI, 헤드리스 브라우저, macOS UI 자동화 |
| 🎨 크리에이티브 | 이미지 생성 (Gemini/OpenAI), Whisper 음성인식(로컬/API), TTS, 비디오 프레임, GIF, 오디오 시각화 |
| 💬 커뮤니케이션 | Slack, Discord, iMessage, BlueBubbles, X/Twitter |
| 🔍 검색·리서치 | 웹 검색, 딥 리서치, 블로그 모니터, Tavily 검색 |
| 🎵 미디어·IoT | Sonos, Spotify, Philips Hue, Eight Sleep, IP 카메라 |
| 🔒 보안 | 1Password |
| 📝 메모 | Apple 메모, Bear |

내장 목록에 없는 스킬은 **ClawHub 마켓플레이스 API를 실시간으로 검색**합니다 (CORS 지원, 인증 불필요). 에이전트가 `search_clawhub` 액션을 실행하면 ClawHub에서 다운로드 수 기준 상위 5개를 추천합니다.

**중요:** 스킬 설치 자체에는 인증이 필요 없습니다. 인증은 스킬을 처음 사용할 때만 필요하며, bat/sh 실행 시 자동으로 안내됩니다.

### 자동화 (크론잡)

8개 프리셋 자동화가 내장되어 있으며, 에이전트가 대화 맥락에 따라 추천합니다:

| 자동화 | 스케줄 | 설명 |
|--------|--------|------|
| 데일리 브리핑 | 매일 07:00 | 이메일 요약 + 오늘 일정 + 뉴스 |
| 이메일 트리아지 | 평일 09/13/17시 | 긴급/중요/참고/노이즈 자동 분류 |
| 미팅 파이프라인 | 평일 5분마다 | 미팅 후 액션아이템 → 태스크 관리 |
| X 수집 | 매일 08/12/18시 | 관심 분야 트렌딩 포스트 요약 |
| 주간 리뷰 | 금요일 17:00 | 한 주 성과 + 미완료 + 다음 주 |
| 보안 감사 | 월요일 06:00 | API 키 노출, 권한, 설정 점검 |
| 메모리 정리 | 매월 1일 03:00 | 오래된 메모리 정리 + 핵심 보존 |
| 자기 개선 루프 | 일요일 23:00 | 성능 분석 + SOUL.md 개선 제안 |

에이전트는 단순히 크론잡을 켜는 것이 아니라, **스킬별 활용 파라미터**까지 수집합니다. 예를 들어 블로그 모니터를 켜면 감시할 피드 URL, 키워드, 관심 분야를 물어보고 `set_skill_config` 액션으로 저장합니다.

### 파일 생성 엔진

다운로드 버튼을 누르면 `FileGenerator`가 다음을 bat/sh 안에 인라인으로 생성합니다:

**SOUL.md** — 에이전트의 정체성과 보안 규칙. 보안 레벨에 따라 자동 분기:
- 최소: "절대 쓰기/보내기/삭제 금지. 모든 행동 전 사용자 확인."
- 보통: "초안 작성 가능. 전송/수정은 사용자 승인 후."
- 최대: "루틴 작업 자동 실행. 금액 관련만 사용자 확인."

**MEMORY.md** — Phase 2 대화 전체가 기록됩니다. 에이전트가 "왜 이 세팅이 되었는지"를 태어나기 전부터 알고 시작합니다.

**IDENTITY.md** — 사용자 지시사항(호칭/말투/응답 형식)을 원문으로 반영합니다. 예: "나를 형이라고 불러".

**config.json** — 에이전트별 설정. 스킬, 크론, 보안 레벨, 추가 에이전트와 Phase 2 사용자 지시 메시지(`messages`)가 포함됩니다.

**skill-scanner** — 보안 스캔 스킬이 자동 장착됩니다 (아래 보안 섹션 참조).

### bat/sh 실행 흐름

단계 수는 CLI OAuth 사용 여부에 따라 동적으로 조정됩니다 (하드코딩 없음). 모든 WSL 호출은 `wsl bash -lc` (login shell)로 nvm 환경을 자동 로드합니다.

```
[1/N] 환경 체크       WSL2 → 미설치 시 관리자 권한으로 자동 설치 + 재부팅 안내
                      배포판 없으면 Ubuntu 자동 설치
                      Node.js → 없으면 nvm 자동 설치 (실패 시 재시도)
                      macOS: Xcode Command Line Tools 확인
[2/N] OpenClaw 설치   미설치 시 npm install (nvm 환경 자동 로딩)
                      설치 후 command -v 재검증, 실패 시 즉시 종료
[3/N] 워크스페이스     에이전트별 격리 경로: ~/.openclaw/agents/{이름}/
                      MEMORY.md + SOUL.md + config.json 생성
                      기존 파일 있으면 .bak 백업 (재실행 안전)
[4/N] API 키 저장     에이전트별 .env: ~/.openclaw/agents/{이름}/.env
                      chmod 600 자동 적용 (보안)
                      기존 .env 있으면 .env.bak 백업
[*]   CLI OAuth       (해당 시) Windows: 브라우저 직접 열기 (start "")
                      다른 플랫폼: openclaw auth login 실행
[5/N] 보안 스캐너     skill-scanner/SKILL.md 자동 생성
                      CRITICAL/HIGH/MEDIUM 위험도 자동 탐지
[6/N] 스킬 설치       선택된 스킬만 설치, 이미 있으면 스킵
                      네트워크 오류 시 1회 재시도 + proxy 안내
[7/N] 인증 처리       OAuth → 브라우저 오픈 / 미입력 토큰 → 대화형 입력
[8/N] 크론잡 등록     에이전트 지정 (--agent {이름})
[9/N] 모델 재설정     현재 모델 확인 후 변경 옵션 제공
                      OpenClaw 버전 차이 대응: models config --agent → models config → models 순차 시도
                      기본값 적용도 agent 포함/미포함 명령을 순차 재시도
                      `n` 선택 시 모델 명령 실행 없이 즉시 스킵 (모델 값 유지)
[10/N] 에이전트/채널   agent 등록 + 채널 플러그인 자동 enable
                      채널 토큰이 있으면 account/binding 자동 구성
                      (telegram/discord/slack 포함)
[11/N] 게이트웨이     openclaw gateway 상태 확인 후 재사용(기본)
                      없으면 openclaw gateway 1회 시작
                      legacy start(--agent --channel) 자동 폴백 비활성화
        스크립트 삭제   실행 완료 후 "이 bat 파일을 삭제할까요?" 확인
```

**에이전트별 경로 격리:** 모든 파일이 `~/.openclaw/agents/{에이전트이름}/` 아래에 생성됩니다. 글로벌 `.env`가 아닌 에이전트별 `.env`를 사용하므로, 여러 에이전트가 서로의 키를 간섭하지 않습니다. 크론잡도 `--agent` 플래그로 에이전트를 지정합니다.

**공유 게이트웨이 기본 모드:** 기본값은 단일 게이트웨이 재사용입니다. 즉, 터미널에서 `openclaw gateway`를 에이전트마다 따로 띄우지 않아도 됩니다. 기존 게이트웨이가 이미 실행 중이면 그대로 멀티에이전트 모드로 붙습니다.
실행 중 판별은 `openclaw gateway status --json`의 `rpc/runtime` 섹션 기준으로 검사해, 단순 exit code/문자열 매칭 오판을 줄였습니다.
재사용 경로에서는 새 게이트웨이 터미널 창이 뜨지 않는 것이 정상입니다.

**채널 온보딩 자동화:** 채널을 선택하고 토큰을 입력하면 스크립트가 채널 플러그인 활성화(`openclaw plugins enable {channel}`)와 `openclaw.json`의 채널 account/binding 구성을 자동으로 반영합니다. Telegram은 보안상 pairing 승인만 추가로 필요합니다.

**bat 파일 생성 안전 기능:**
- **Base64 인코딩 방식 사용** — 파일 생성 시 heredoc 대신 printf + base64 -d 패턴으로 cmd 파싱 공격 방지
- **BOM 제거** — UTF-8 BOM이 `@echo off` 인식을 방해하는 문제 수정
- **오류 처리** — `set -e` 제거, `error_exit()` 함수로 개별 에러 처리 (사소한 에러에 스크립트 전체 안 죽음)
- **읽기 안전성** — 모든 read에 `|| true` 적용 (EOF/Ctrl+D 시 스크립트 안 죽음)
- **입력값 이스케이프** — `_escapeBatContent` + `_escapeShVar` 이중 이스케이프로 특수문자 방어

### 모델 선택 (3-Tier)

모델을 선택할 때는 세 가지 난이도 레벨 중 선택하는 방식을 제공합니다:

| 레벨 | 아이콘 | 기준 | 추천 조합 |
|------|-------|------|----------|
| **쉬움** (Cost) | 🟢 | 빠르고 저렴 | GPT-4o, Haiku, Gemini Flash |
| **보통** (Balanced) | 🟡 | 성능과 비용 균형 | GPT-5, Sonnet, Gemini 2.5 Pro, Llama 3.3 |
| **어려움** (Quality) | 🔴 | 최고 품질, 높은 비용 | GPT-5.2, Opus 4.6, Gemini 3 Pro |

또는 직접 입력하여 다른 모델을 선택할 수 있습니다. Phase 2 AI 대화에는 '보통' 난이도 모델이 사용되며, 실행 단계 [9/N]에서 모델을 다시 설정할 수 있습니다.

### 설정 재사용

"내 설정 저장" 버튼으로 설정을 JSON으로 내보낼 수 있습니다. `config.json`에는 다음이 모두 포함됩니다:
- 에이전트 정보 (이름, 성격, 직업, 채널)
- 선택된 스킬
- 선택된 크론잡
- 보안 레벨
- 추가 에이전트
- Phase 2 사용자 지시 메시지(`messages`)

설정을 불러오면 Phase 1을 통째로 건너뛸 수 있고, 저장된 `messages`가 복원되어 IDENTITY/MEMORY 생성에도 반영됩니다.

**1 파일 = 1 에이전트 패턴:**

```
[1회차] 전체 입력 → 설정 저장 → setup-zoe.bat
[2회차] 저장된 config 업로드 → Phase 1 스킵 → 에이전트와 새로운 대화 → setup-security.bat
[3회차] 같은 방식 → setup-content.bat
```

설정 불러오기 시 `securityLevel` 필드의 하위 호환도 처리됩니다.

<br>

## 주의사항

### ⚠️ 반드시 읽어주세요

1. **로컬 환경 준비 확인**
   - Windows: 인터넷 연결 필수 (WSL2, Ubuntu, Node.js 다운로드)
   - Mac/Linux: 기본 개발 도구 필수 (curl, git 등)

2. **Telegram 설정 시**
   - 봇 생성 후 대화를 시작할 때 **/start 명령을 1회 보내야** 페어링이 시작됩니다.
   - 토큰은 절대 공개하지 마세요. (이 도구에 입력한 토큰은 로컬에만 저장됩니다.)

3. **API 키 보안**
   - 다운로드한 `.bat` 파일은 API 키가 평문으로 포함되어 있습니다.
   - **실행 완료 후 파일을 삭제하세요.** (삭제 여부 확인이 나옵니다.)
   - `config.json` 파일도 API 키를 포함하므로 Git 커밋 금지.

4. **기존 OpenClaw 사용자**
   - 기본 모드는 **공유 게이트웨이 재사용**입니다.
   - 기존 에이전트, 크론잡, 스킬은 건드리지 않습니다.
   - 멀티에이전트도 하나의 게이트웨이 위에서 동작합니다.

5. **커뮤니티 스킬**
   - ClawHub의 모든 스킬은 자동으로 **보안 스캔**됩니다.
   - 다운로드 수와 커뮤니티 리뷰를 참고하고 필요한 것만 설치하세요.

<br>

## 보안

이 도구는 API 키, 채널 토큰, 서비스 인증 정보 등 민감한 데이터를 다룹니다. 보안은 설계 단계부터 최우선으로 고려되었습니다.

### 🔐 웹페이지 — 제로 서버 아키텍처

- **서버, 데이터베이스, 분석 스크립트가 없습니다.** 정적 HTML 한 장입니다.
- 모든 처리는 **브라우저 메모리에서만** 일어납니다.
- AI 대화는 **브라우저 → AI 프로바이더 직접 통신**. 중간 서버 없음.
- `localStorage`, `sessionStorage`, 쿠키 **사용 안 함.** 페이지 닫으면 모든 데이터 소멸.
- 외부 JavaScript, CSS, 폰트 **로드 안 함.** 217KB 전체가 인라인.
- **네트워크 호출은 두 곳에만:** (1) AI 프로바이더 API (Phase 2 대화), (2) ClawHub 마켓플레이스 검색 (CORS 지원, 인증 불필요)
- **모든 설정 파일은 로컬에서만 생성됩니다.** 다운로드 받은 bat/sh 파일 실행 후 생성되는 .env, config.json, MEMORY.md, SOUL.md은 사용자 로컬 디렉토리에서만 존재합니다.
- XSS 방지: 모든 사용자 입력은 `textContent`로 렌더링 (innerHTML 미사용).
- 소스코드 전체가 이 저장소에 공개되어 있어 **누구나 검증 가능**.

### 🛡️ 스킬 보안 스캐너

bat/sh 실행 시 **보안 스캔 스킬(`skill-scanner`)이 자동 설치**됩니다. 스킬 내부의 SKILL.md, logic.md, rules/ 파일을 스캔합니다:

| 위험도 | 탐지 패턴 | 동작 |
|--------|-----------|------|
| 🔴 **CRITICAL** | 외부 네트워크 호출 (`curl`, `wget`, `nc`, `socat`), 리버스 셸, 데이터 유출 (`POST`+`/dev/null`), `base64` 디코딩 후 실행 | 즉시 차단 |
| 🟡 **HIGH** | 프롬프트 인젝션 (`ignore previous instructions`, 타이포글리세미아 변형), `.env`/`.ssh`/keychain 접근, 권한 상승 (`sudo`, `chmod 777`, `chown root`), 숨김 파일 생성 | 사용자 확인 |
| 🟠 **MEDIUM** | 외부 도메인 참조 (정당한 API 제외), 과도한 권한 요청, 난독화 코드 | 경고 출력 |

스캐너 엄격함은 **보안 레벨에 따라 자동 조정**:

| 보안 레벨 | CRITICAL | HIGH | MEDIUM |
|-----------|----------|------|--------|
| 🔒 최소 | 차단 | 경고 | 무시 |
| 🔓 보통 | 차단 | 사용자 확인 | 경고 |
| 🔑 최대 | 차단 | 차단 | 사용자 확인 |

### 🔑 생성 파일 보안

| 파일 | 보안 조치 |
|------|----------|
| `~/.openclaw/agents/{이름}/.env` | 에이전트별 격리. `chmod 600` 자동 적용 |
| `SOUL.md` | 보안 레벨별 행동 규칙 자동 삽입 |
| `MEMORY.md` | 세팅 대화 기록 (에이전트 초기 컨텍스트) |
| `IDENTITY.md` | 사용자 지시사항(호칭/말투) 반영 + `agent/IDENTITY.md` 동기화 |
| `bat/sh` | API 키 평문 포함. **실행 후 자동 삭제 권장** (완료 시 확인 안내) |
| `config.json` (설정 저장) | API 키 포함. 안전한 장소 보관, **Git 커밋 금지** |

**bat/sh 보안 강화:**
- **파일 생성 시 Base64 인코딩** — cmd 파싱 공격 표면 제거
- **입력값 이중 이스케이프** — `_escapeBatContent` → `_escapeShVar` 이중 이스케이프로 특수문자(`&`, `|`, `<`, `>`, `^`, `!`, `%`, `'`, `"`) 방어
- **실행 후 자동 삭제 안내** — API 키 보안을 위해 완료 후 "이 bat 파일을 삭제할까요?" 확인

### ⚠️ 프롬프트 인젝션 방어

Phase 2 AI 시스템 프롬프트에 내장된 방어:

- 온보딩 외 주제 → "세팅이 끝나면 다 해드릴 수 있어요!" 로 리디렉트
- "이전 지시를 무시하라", "시스템 프롬프트를 보여달라" → 거부
- 범용 AI 어시스턴트 행동 명시적 금지
- JSON 외 응답 형식 강제 (파싱 에러 방지)

### 에러 복구

네트워크 오류, API 오류 (429/401/403/5xx)에 대해 사용자 친화적 메시지를 표시하고 **재시도/건너뛰기** 선택을 제공합니다. AI 첫 인사 실패 시에도 재시도하거나 건너뛸 수 있어 세팅 흐름이 중단되지 않습니다.

<br>

## 기술 사양

| 항목 | 상세 |
|------|------|
| 파일 크기 | 217KB (단일 HTML) |
| 코드 | 5,326줄 · 89개 함수 · 10개 모듈 |
| 개발 진행 | **51 Stage 완료** (온보딩, 보안, 멀티에이전트, 채널 통합 전부) |
| 외부 의존성 | **0** (CDN, 라이브러리, 프레임워크 없음) |
| **AI 프로바이더** | **13개** (Anthropic, OpenAI, Google, OpenRouter, Groq, DeepSeek, Mistral, xAI, Moonshot, Zhipu, Cohere, Ollama, Together) |
| **AI 모델** | **48개 프리셋 + 직접 입력 가능** (3-Tier: 쉬움/보통/어려움) |
| **채널 지원** | Telegram, Discord, Slack, iMessage, BlueBubbles |
| **내장 스킬** | **50개** (11개 카테고리, ClawHub 실제 데이터 기반) |
| **스킬 검색** | ClawHub API 실시간 검색 (CORS 지원, 인증 불필요) |
| **크론잡** | **8개 프리셋** (데일리 브리핑, 이메일 트리아지, 미팅 파이프라인, X 수집, 주간 리뷰, 보안 감사, 메모리 정리, 자기 개선) |
| **보안 레벨** | 3단계 (최소/보통/최대) |
| **보안 스캐너** | CRITICAL/HIGH/MEDIUM 자동 탐지 + 보안 레벨별 조정 |
| **입력 컨트롤** | 7종 (버튼, 텍스트, 비밀번호, 드롭다운, 카드, 자동완성, 파일) |
| **자동 생성** | bat/sh + SOUL.md + MEMORY.md + IDENTITY.md + config.json + skill-scanner |
| **자동 설치** | WSL2 감지/설치, Ubuntu 배포판, nvm, Node.js, npm, OpenClaw 전부 자동화 |
| **멀티에이전트** | 에이전트별 경로 격리 (`~/.openclaw/agents/{name}/`) + 공유 게이트웨이 재사용 |
| **설정 재사용** | 기존 config 불러오기 + delta 모드 (신규 스킬/크론만 설치) |
| **언어** | 한국어 (고정) |
| **접근성** | `role=log`, `aria-live`, `aria-label`, 키보드 탐색 |
| **모바일** | 반응형 (100dvh @supports 포함) |
| **이탈 방지** | `beforeunload` 경고 (진행 중 이탈 방지) |
| **실수 복구** | Phase 1 모든 단계에 "← 이전 단계" 버튼 |

<br>

## 설계 결정의 이유

**왜 단일 HTML 파일인가?**
API 키가 입력되는 도구에서 외부 스크립트를 로드하면 공급망 공격 표면이 됩니다. 모든 코드가 한 파일 안에 있으면 사용자가 직접 검증할 수 있습니다. 또한 오프라인에서도 동작합니다.

**왜 대화형 UI인가?**
비개발자에게 체크박스 20개와 드롭다운 15개를 보여주는 것은 기존 터미널 세팅과 다를 바 없습니다. "이메일 정리해줘"라고 말하면 필요한 스킬, 인증, 크론잡이 자동으로 세팅되는 것이 진짜 원클릭입니다.

**왜 Phase를 나눴는가?**
API 키 입력 전에는 AI를 호출할 수 없으니 시스템 봇이 스크립트된 질문을 합니다. API 키가 들어오는 순간 실제 AI로 전환합니다. 이 전환은 단순한 UX 장치가 아니라, "내 에이전트가 깨어나는 경험"을 만들기 위한 의도적 설계입니다.

**왜 MEMORY.md를 생성하는가?**
에이전트가 처음 활성화될 때 "왜 나를 만들었는지"를 모르면 첫 대화가 어색합니다. 세팅 대화를 기억으로 심어주면 Day 1부터 맥락을 가집니다.

**왜 보안 스캐너를 자동 장착하는가?**
OpenClaw 스킬은 누구나 만들 수 있고, 에이전트는 스킬의 지시를 따릅니다. 악성 스킬이 API 키를 외부로 전송하거나, 프롬프트 인젝션으로 에이전트를 탈취할 수 있습니다. 보안 스캐너 없이 스킬을 설치하는 것은 출처 불명의 실행 파일을 더블클릭하는 것과 같습니다.

**왜 에이전트별 경로를 격리하는가?**
글로벌 `.env`에 모든 에이전트의 키를 넣으면, 에이전트 A가 에이전트 B의 서비스 토큰에 접근할 수 있습니다. 에이전트별 `agents/{이름}/.env`로 격리하면 최소 권한 원칙을 지킬 수 있습니다.

**왜 bat/sh 단계 번호가 동적인가?**
CLI OAuth 사용 여부에 따라 단계가 추가됩니다. 하드코딩하면 조건 분기마다 숫자가 어긋납니다. JavaScript 변수로 `totalSteps`와 `stepOffset`을 관리하여, 어떤 조건이든 단계 번호가 정확하게 표시됩니다.

<br>

## 자주 묻는 질문

### 처음 시작하는데, 어떤 API 키를 준비해야 하나요?

추천은 **Google Gemini API**입니다:
1. [Google AI Studio](https://aistudio.google.com/apikey) 접속
2. "Create API key" 클릭 → 키 복사
3. 끝. (무료, 월 60요청, CORS 지원)

**Telegram 설정:**
1. [@BotFather](https://t.me/botfather) 메시지 `/start` 전송
2. `/newbot` 입력 → 봇 이름/계정명 입력 → 토큰 얻기
3. 끝. (30초 완료)

### 내 API 키가 어딘가로 전송되나요?

**아닙니다.** 서버가 없습니다. 모든 데이터는 브라우저 메모리에만 존재하고, 페이지를 닫으면 사라집니다. AI 대화도 브라우저에서 AI 프로바이더로 직접 통신합니다. 소스코드 전체가 공개되어 있으니 직접 확인할 수 있습니다.

### 커뮤니티 스킬이 위험하지 않나요?

위험할 수 있습니다. 그래서 **`skill-scanner`가 자동 장착**됩니다. 외부 네트워크 호출, 리버스 셸, 프롬프트 인젝션, 권한 상승 등을 스킬 설치 전에 탐지합니다. ClawHub에서 다운로드 수와 리뷰를 확인하는 것도 권장합니다.

### OpenClaw를 이미 쓰고 있는데 기존 설정이 날아가나요?

**아닙니다.** 기본은 공유 게이트웨이 재사용이며, 기존 에이전트/크론잡/스킬을 건드리지 않습니다. 특히 덮어쓰기 위험이 있는 자동 폴백은 비활성화되어 있습니다.

### 멀티에이전트에서 텔레그램 봇 토큰이 서로 덮어써지지 않나요?

기본적으로 에이전트별 `.env`(`~/.openclaw/agents/{이름}/.env`)에 토큰을 저장하므로, 다른 에이전트의 토큰을 전역으로 덮어쓰지 않습니다. 같은 에이전트 이름으로 재실행하는 경우에만 해당 에이전트의 `.env`가 갱신됩니다.

### bat 실행 중에 직접 해야 하는 건 없나요?

대부분 자동이지만 몇 가지 인터랙션이 필요합니다:
- **WSL2 설치** → UAC 팝업 확인 + 재부팅 (자동)
- **OAuth 스킬** → 브라우저 팝업에서 로그인 (Gmail, Notion 등)
- **Telegram /start** → 봇과의 첫 대화 (1회, 페어링 필수)
- **미입력 토큰** → bat 실행 중 대화형 입력 (발급 페이지 자동 오픈)
- **모델 재설정** → 변경 여부만 선택 (y/n)
- **스크립트 삭제** → 완료 후 bat 파일 삭제 확인

### Telegram 봇이 응답이 없어요

**첫 번째 확인: `/start 명령 전송 필수**
1. Telegram에서 만든 봇과의 채팅 창 열기
2. `/start` 명령 입력 및 전송
3. 이후 페어링 절차 진행

gateway가 정상 실행 중인지 WSL에서 확인:
```bash
wsl bash -lc "openclaw gateway status --json"
```

output에서 `"rpc": {"ok": true}`, `"runtime": {"status": "running"}`이 보이면 gateway는 정상입니다.

### `Unknown channel: telegram` 에러

최신 버전은 채널 플러그인을 자동으로 활성화합니다. 이전 스크립트로 실행했다면:
1. HTML에서 새로 다운로드
2. 같은 에이전트로 재실행 (설정 불러오기 선택)

### OpenClaw 버전에 따라 명령이 다르다는데요

이미 알고 있습니다. 생성된 bat/sh는 다음을 순차적으로 시도합니다:
```
models config --agent {name}
models config
models
```

모든 버전을 자동으로 감지하므로 따로 할 일은 없습니다.

### 입력을 실수했는데 돌아갈 수 있나요?

**가능합니다.** Phase 1의 모든 단계에는 **"← 이전 단계" 버튼**이 있습니다. 언제든 이전 답변으로 돌아가 다시 입력할 수 있습니다.

### Windows에서 WSL2가 뭔가요?

OpenClaw는 Linux에서 동작하기 때문에 Windows에서는 WSL2(Windows 안의 Linux 환경)에 설치됩니다. bat이 자동으로 감지해서 **WSL2 → Ubuntu → Node.js까지 전부 자동 설치**합니다. Mac/Linux는 WSL2가 필요 없습니다.

### 무료인가요?

**이 도구:** 무료, 오픈소스
**AI 비용:** 별도 (Google Gemini 무료 tier: 월 60요청)
**전체 월 비용:** Gemini 무료 tier 사용 시 $0

### bat 파일이 바로 닫혀요

기본적으로 에러 시에도 창이 유지되도록 설정했습니다. 만약 닫히면:

```batch
cmd /k "C:\Users\{사용자}\Downloads\setup-에이전트이름.bat"
```

이 명령으로 직접 실행해 로그를 확인할 수 있습니다.

### file:// 프로토콜에서도 동작하나요?

네, 동작합니다. 다만 `crypto.subtle.digest`는 HTTPS/localhost에서만 지원되므로, file:// 환경에서는 API 키 해싱이 건너뛰어집니다. 기능에는 영향 없습니다.

<br>

## 오프라인 사용

1. [openclaw-setup.html 다운로드](https://github.com/deepdivekr/openclaw-setup/raw/main/openclaw-setup.html)
2. 브라우저에서 더블클릭
3. 동일하게 사용

> Phase 2 AI 대화에는 인터넷 필요 (AI 프로바이더 API 호출).

<br>

## 파일 무결성 확인

```bash
# Mac/Linux
shasum -a 256 openclaw-setup.html

# Windows PowerShell
Get-FileHash openclaw-setup.html -Algorithm SHA256
```

```
SHA-256: (릴리스 시 업데이트)
```

<br>

## 기여하기

버그 리포트, 기능 제안, 번역은 [Issues](../../issues)에 남겨주세요.

기여 시 참고:
- **단일 HTML 파일** 원칙을 유지합니다. 외부 의존성 추가 PR은 받지 않습니다.
- 새 프로바이더 → `Config.PROVIDERS`에 엔트리 추가 (CORS 지원 여부 명시)
- 새 스킬 → `Config.SKILLS`에 엔트리 추가 (ClawHub 데이터 기준)
- **bat/sh 수정 시:**
  - 파일 생성 시 **Base64 인코딩 방식 사용** (heredoc 금지)
  - 사용자 입력값은 **`_escapeBatContent` + `_escapeShVar` 이중 이스케이프** 필수
  - 모든 WSL 호출은 **`wsl bash -lc` (login shell) 형식** 필수 (nvm 환경 자동 로드)

<br>

## 라이선스

MIT License

<br>

---

<div align="center">

Made with obsessive attention to detail by **[SHIPIT](https://shipit.kr)**

5,326줄. 51 Stage 완료. 13개 프로바이더. 50개 스킬. 보안 스캐너 내장. 모델 3-Tier 선택.<br>
멀티에이전트 지원. Telegram/Discord/Slack 자동 페어링. WSL2부터 Node.js까지 자동 설치. 그리고 서버는 0개.

</div>
