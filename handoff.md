# Handoff: 착수 — 규칙 파일 채우기와 첫 센서 로그

**Last cycle close**: <YYYY-MM-DD HH:MM> (commit <sha>)   ← `date` 와 `git log -1` 로 실측해 적는다
**상태**: ready

## 세션 시작의 순서
1. `git pull` → `git log --oneline <위 sha>..HEAD` 로 지난 cycle 이후 커밋을 **전부** 확인 (팀원 커밋 포함)
2. 이 파일 정독 → 「다음 작업 시퀀스」 를 계획으로 보고 → 승인 후 착수

## 다음 작업 시퀀스 (단일)
1. `AGENTS.md` §0 프로젝트 개요를 채우고 §8 에 주제 규칙 (`topics/`) 을 붙여 넣는다 → `rules: fill in project overview and topic rules`
2. 각자 쓰는 에이전트 도구가 `AGENTS.md` 를 읽는지 확인한다 (README 의 「규칙 적재 확인」)
3. Pi: Raspberry Pi OS 64-bit 설치 · SSH · 사용자 그룹 (`gpio` `i2c` `video`) · 저장소 clone
4. 센서 1 개를 읽어 `logs/` 에 CSV 로 저장하고 `tools/check_log.py` 를 통과시킨다

## 핵심 진입점
- `AGENTS.md` — 규칙 원본 · `docs/meeting-notes/TEMPLATE.md` — 회의록 틀 · `logs/experiments/TEMPLATE.md` — 실험 기록 틀

## 확정 (재론 불요)
- (아직 없음. 형식: `<결정> — 결정자 · YYYY-MM-DD HH:MM · 회의록 경로`)

## 미결 (결정할 사람)
- 커밋 trailer (`Assisted-by:`) 사용 여부 — <팀장>
- 커밋 메시지 언어 (영어 / 한국어) — <팀 전체>

## 환경 유의
- Pi 접속 정보 (IP · 계정) 는 `.env` 에만 둔다. 커밋하지 않는다
