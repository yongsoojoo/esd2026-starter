# esd2026-starter — 임베디드시스템설계 기말 프로젝트 시작 템플릿

AI 코딩 에이전트와 함께 Raspberry Pi 프로젝트를 진행할 때 쓰는 **규칙 파일 · 인수인계 · 기록 틀**이다.
에이전트는 세션이 끝나면 대화를 잊는다. 그래서 **기억을 파일에 둔다** — 규칙은 `AGENTS.md`, 다음 할 일은 `handoff.md`,
무슨 일이 있었는지는 회의록과 git 이력에 둔다.

- 튜토리얼: https://yongsoojoo.github.io/esd2026-agent-workflow/
- 사례 저장소 (이 규칙들이 왜 생겼는지 보여 주는 재구성 사례): https://github.com/yongsoojoo/esd2026-example-beam

## 빠른 시작

1. GitHub 에서 **Use this template → Create a new repository** (팀 대표 1 명). 팀원을 collaborator 로 초대한다
2. 각자 clone 한다. Pi 에도 같은 저장소를 clone 한다
3. `AGENTS.md` §0 을 채우고, `topics/` 에서 팀 주제 파일 하나를 골라 그 내용을 §8 에 붙여 넣는다
4. 자기 에이전트 도구가 규칙을 읽는지 확인한다 (아래 「규칙 적재 확인」)
5. 첫 세션을 아래 「세션 시작 프롬프트」 로 연다

## 폴더 구조

```
AGENTS.md                    규칙 원본 — 모든 에이전트가 읽는다
handoff.md                   다음 세션의 단일 진입점 — 세션 끝마다 덮어쓴다
CLAUDE.md                    Claude Code 용: "@AGENTS.md" 한 줄
.gemini/settings.json        Gemini CLI 용: AGENTS.md 를 읽게 하는 설정
.github/copilot-instructions.md   JetBrains 등의 Copilot Chat 용 안내
.kiro/steering/hw-edit.md    Kiro 용 보조 규칙 예시 (구동 코드를 열 때만 적재)
topics/                      주제별 규칙 3 종 — 하나를 골라 AGENTS.md §8 에 붙여 넣는다
docs/meeting-notes/          회의록 (TEMPLATE.md)
docs/ai-usage.md             AI 사용 기록 (필수)
docs/writing-guide.md        보고서 작성 가이드
logs/experiments/            실험 기록 (TEMPLATE.md) — 로그 CSV 는 logs/ 아래에
hw/limits.py                 구동기 한계 상수 — 이 값들은 이 파일에만 둔다
tools/check_log.py           로그 무결성 검사 (샘플 수 · 간격 · tick)
```

코드는 `src/` (제어 · 센싱), `hw/` (구동기 드라이버 · 한계), `sim/` (시뮬레이터), `tests/` 에 두기를 권한다.

## 도구별 설정

| 도구 | 읽는 파일 | 이 템플릿에서 할 일 |
|---|---|---|
| **Kiro** (IDE · CLI) | `AGENTS.md` 를 기본으로 항상 읽음 + `.kiro/steering/*.md` | 저장소를 처음 열 때 **Trust** 를 누른다 (누르지 않으면 steering 이 적재되지 않는다) |
| **Claude Code** | `CLAUDE.md` (→ `@AGENTS.md` 로 원본을 불러옴) | 없음 |
| **Codex CLI** | `AGENTS.md` | 없음 |
| **Gemini CLI** | 기본은 `GEMINI.md` 만 | 이 템플릿의 `.gemini/settings.json` 이 `AGENTS.md` 를 읽게 한다 |
| **GitHub Copilot** (VS Code · CLI) | `AGENTS.md` | VS Code 설정 `chat.useAgentsMdFile` 이 켜져 있는지 확인 |
| **Cursor** | `AGENTS.md` | 없음 |
| 웹 채팅만 쓰는 경우 | 없음 | 세션 시작 때 `AGENTS.md` 와 `handoff.md` 를 직접 붙여 넣는다 |

**Kiro 사용자 유의**
- 학생 프로그램 (Kiro for Students) 계정은 약관상 Free Tier 로 간주되어, 기본 설정에서는 대화 내용이 서비스 개선에 쓰일 수 있다.
  원하지 않으면 설정 → User → Application → **Telemetry and Content** 에서 끈다
- 구동 코드를 다루는 동안에는 **Supervised** 모드를 쓴다 (`AGENTS.md` §3)
- `product.md` · `tech.md` · `structure.md` 를 자동 생성하면 `AGENTS.md` §0 과 내용이 겹친다. 생성했다면 둘 중 하나만 남긴다

## 규칙 적재 확인 (양성 대조)

「규칙 파일을 만들었다」 와 「에이전트가 그 파일을 읽었다」 는 다르다. 새 세션을 열고 이렇게 묻는다:

> 파일을 새로 열지 말고, 이미 받은 지시만으로 답해 줘. AGENTS.md §3 의 1 번 조항 제목은 무엇이야?

도구 호출 (파일 읽기) 없이 「구동 명령은 사람 입회 하에」 라고 답하면 적재된 것이다. 파일을 읽으러 가거나 모른다고 하면
적재되지 않은 것이다 — 위 표의 설정을 다시 확인한다. 도구가 적재 목록을 보여 주는 명령도 있다
(Kiro CLI `/context show`, Claude Code `/memory`, Gemini CLI `/memory show`).

## 세션 시작 프롬프트

> handoff.md 와 지난 cycle 이후의 git log 를 읽고, 다음 작업을 계획 (목표 / 단계 / 검증 방법) 으로 보고해 줘. 승인 전에는 착수하지 마.

세션을 닫을 때:

> 오늘 작업을 회의록으로 정리하고, handoff.md 를 덮어쓰고, docs/ai-usage.md 에 한 줄 추가한 뒤 commit · push 해 줘. 시각은 date 로 읽어.

## 이 템플릿을 만든 과정의 AI 사용

이 템플릿은 지도교수가 AI 코딩 에이전트 (Claude Code) 와 함께 작성하고 검토했다. 규칙의 원형은 지도교수 연구실의 실제 연구 프로젝트에서
에이전트와 일하며 생긴 규칙이며, 학부 프로젝트 규모에 맞게 줄였다.
