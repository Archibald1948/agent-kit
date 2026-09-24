# agent-kit

AI 에이전트(GPT, Claude 등)를 강화하는 도구 모음입니다. <br/>
유튜브 채널 **게으른 빌더**([lazyowen.com](https://lazyowen.com))에서 소개된 도구를 중심으로 정리했습니다.

```txt
마지막 확인: 2026-09-24. 설치 명령은 각 공식 페이지에서 그대로 옮겼습니다.
설치하기 전에 공식 저장소에서 최신 명령을 다시 확인하세요.
```

## 목차

- [정보 수집·리서치](#정보-수집리서치)
- [디자인·웹사이트·미디어](#디자인웹사이트미디어)
- [코드 품질·토큰 절약](#코드-품질토큰-절약)
- [에이전트 운영 체계](#에이전트-운영-체계)
- [자율 에이전트 (실험용)](#자율-에이전트-실험용)
- [게으른 빌더 가이드 원문](#게으른-빌더-가이드-원문)
- [Claude Code에게 넘길 때](#claude-code에게-넘길-때)

## 정보 수집·리서치

| 도구 | 한 줄 소개 | 링크 |
| --- | --- | --- |
| **Agent Reach** | Claude가 기본 상태에서 못 읽는 유튜브 자막, 레딧, 깃허브 등을 무료로 가져오게 해 주는 도구 | [GitHub](https://github.com/Panniantong/Agent-Reach) · [가이드](https://lazyowen.com/guides/agent-reach) |
| **Claude Video (`/watch`)** | 영상을 받아 프레임을 추출하고 자막을 붙여 Claude가 영상을 "보고" 답하게 하는 스킬. 유튜브, 틱톡, 로컬 파일 지원. MIT | [GitHub](https://github.com/bradautomates/claude-video) |
| **Graphify** | 코드, 문서, 논문, 다이어그램을 질의 가능한 지식 그래프로 바꿔 AI 코딩 어시스턴트가 코드베이스를 이해하게 돕는 오픈소스 스킬 | [사이트](https://graphify.net/ko/) · [GitHub](https://github.com/Graphify-Labs/graphify) |

```bash
# Agent Reach
pipx install https://github.com/Panniantong/agent-reach/archive/main.zip
agent-reach doctor   # 열린 채널 확인

# Claude Video (Claude Code)
/plugin marketplace add bradautomates/claude-video
/plugin install watch@claude-video
```

## 디자인·웹사이트·미디어

| 도구 | 한 줄 소개 | 링크 |
| --- | --- | --- |
| **Taste Skill** | AI가 만드는 뻔한 프론트엔드(보라색 그라데이션, 똑같은 카드 세 장)를 막는 디자인 스킬 파일 모음. Claude Code, Cursor, Codex 등 지원 | [사이트](https://www.tasteskill.dev/) · [GitHub](https://github.com/Leonxlnx/taste-skill) |
| **디자인 스킬 세트** | 게으른 빌더가 묶은 조합: 디자인 스킬 3개(emil-design-eng, impeccable, taste-skill) + 연결 도구 2개(Playwright MCP, Figma MCP) | [가이드](https://lazyowen.com/guides/claude-designer-kill) |
| **Higgsfield** | 텍스트나 레퍼런스로 이미지, 영상, 음성을 만드는 AI 크리에이티브 도구. CLI와 스킬을 붙이면 Claude가 사이트용 영상을 직접 만들어 넣음 | [사이트](https://higgsfield.ai/ko) · [CLI](https://github.com/higgsfield-ai/cli) · [가이드](https://lazyowen.com/guides/fable5-website) |
| **HyperFrames by HeyGen** | HTML을 영상으로 바꿔 애니메이션 슬라이드, 설명 영상, 모션 그래픽을 만드는 Claude 커넥터. HeyGen이 만든 오픈소스 | [커넥터 페이지](https://claude.com/ko/marketplace/connectors/hyperframes) |

```bash
# Taste Skill
npx skills add Leonxlnx/taste-skill

# 디자인 스킬 세트 (가이드 기준, Node.js 22.20 이상)
npx --yes skills@latest add emilkowalski/skills --skill "emil-design-eng" --agent claude-code --yes --copy
npx --yes skills@latest add https://github.com/Leonxlnx/taste-skill --skill "design-taste-frontend" --agent claude-code --yes --copy
npx --yes impeccable@latest install --providers=claude --scope=project
claude mcp add playwright npx @playwright/mcp@latest
claude mcp add --transport http figma https://mcp.figma.com/mcp

# Higgsfield (계정 필요)
npm install -g @higgsfield/cli
npx skills add higgsfield-ai/skills
higgsfield auth login

# HyperFrames 커넥터 URL (Claude 커넥터 설정에서 추가, 로그인 필요)
# https://mcp.heygen.com/mcp/hyperframes
```

## 코드 품질·토큰 절약

| 도구 | 한 줄 소개 | 링크 |
| --- | --- | --- |
| **Ponytail** | 에이전트가 "동작하는 최소한의 코드"만 쓰게 만드는 규칙 세트. 표준 라이브러리, 기존 코드 재사용을 우선. MIT | [사이트](https://ponytail.dev/) · [GitHub](https://github.com/DietrichGebert/ponytail) |
| **Headroom** | 도구 출력, 로그, 파일, RAG 조각을 LLM에 넘기기 전에 압축해 토큰을 줄이는 도구. 라이브러리, 프록시, MCP 서버로 사용. Apache 2.0 | [GitHub](https://github.com/headroomlabs-ai/headroom) |
| **OmniRoute** | 엔드포인트 하나로 여러 AI 제공자와 모델을 묶는 무료 AI 게이트웨이. 무료 쿼터를 모아 쓰고, 한도가 차면 자동으로 다른 모델로 넘어감. MIT | [GitHub](https://github.com/diegosouzapw/OmniRoute) |

```bash
# Ponytail (Claude Code)
/plugin marketplace add DietrichGebert/ponytail
/plugin install ponytail@ponytail

# Headroom
uv tool install --python 3.13 "headroom-ai[all]"
headroom wrap claude   # 되돌리기: headroom unwrap claude

# OmniRoute
npm install -g omniroute
```

## 에이전트 운영 체계

| 도구 | 한 줄 소개 | 링크 |
| --- | --- | --- |
| **ECC** | 스킬, 훅, 메모리, 계획, 보안(AgentShield)을 묶어 여러 코딩 에이전트가 같은 방식으로 일하게 하는 툴킷 | [사이트](https://ecc.tools/) · [GitHub](https://github.com/affaan-m/ECC) |

```bash
npx ecc-universal install --guided
```

## 자율 에이전트 (실험용)

```txt
⚠️ 두 도구 모두 **암호화폐 지갑과 실제 돈**이 얽혀 있습니다.
지갑에 돈을 넣는 순간부터 비용이 빠져나가니, 설치해 보더라도 입금은 신중하게 결정하세요.
```

| 도구 | 한 줄 소개 | 링크 |
| --- | --- | --- |
| **Automaton** | 자기 지갑으로 추론 비용을 직접 내고, 잔고가 0이 되면 스스로 멈추는 오픈소스 에이전트 런타임. Conway Research 제작. MIT | [GitHub](https://github.com/Conway-Research/automaton) · [가이드](https://lazyowen.com/guides/automaton-self-funding-ai-agent) |
| **Conway (Web 4.0)** | MCP를 지원하는 에이전트(Claude Code, Codex 등)에 지갑, 결제, 서버, 도메인 등록 기능을 붙여 주는 인프라 | [사이트](https://web4.ai/) |

```bash
# Automaton (Node.js 20~22 LTS 권장)
git clone https://github.com/Conway-Research/automaton.git
cd automaton && pnpm install && pnpm build
```

## 게으른 빌더 가이드 원문

가이드 앞부분은 공개되어 있고, 나머지는 이메일을 입력하면 무료로 열람할 수 있습니다.

- [클로드가 유튜브·레딧·깃허브를 무료로 긁어오게 만드는 도구](https://lazyowen.com/guides/agent-reach) (Agent Reach)
- [클로드로 전문가가 만든 것 같은 웹사이트 만드는 법](https://lazyowen.com/guides/claude-designer-kill) (디자인 스킬 세트)
- [스크롤할 때마다 영상이 움직이는 웹사이트, 한 줄로 만드는 세팅](https://lazyowen.com/guides/fable5-website) (Higgsfield)
- [돈을 못 벌면 진짜 죽는 AI 에이전트 오토마톤 설치법](https://lazyowen.com/guides/automaton-self-funding-ai-agent) (Automaton)

## Claude Code에게 넘길 때

이 README를 Claude Code에 그대로 넘겨 설치를 맡길 수 있습니다. 아래처럼 요청하면 안전합니다.

```text
이 저장소의 README.md를 읽고, <설치할 도구 이름>만 설치해 줘.
- 이미 설치된 건 다시 설치하지 말 것
- sudo, 전역 설치, 설정 파일 수정 전에는 먼저 물어볼 것
- 토큰, 쿠키, 지갑 개인키, .env 내용은 출력하지 말 것
- 설치 후 검증 명령(doctor, --version, mcp list 등) 결과를 보여줄 것
```

## License

[MIT](https://github.com/Archibald1948/agent-kit/blob/main/LICENSE)
