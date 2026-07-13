# 0xd669/skills

코딩 에이전트를 위한 세 가지 핵심 skill 모음입니다.

## 설치

### Skills CLI

[Skills CLI](https://skills.sh)로 간편하게 설치할 수 있습니다.

```bash
bunx skills add 0xd669/skills
npx skills add 0xd669/skills
```

설치하면 `/prompt-doctor`, `/commit`, `/pr` 명령을 바로 사용할 수 있습니다.

### Claude Code Marketplace

Claude Code의 plugin marketplace를 통해 설치할 수도 있습니다.

```bash
# marketplace 등록 (최초 1회)
/plugin marketplace add 0xd669/skills

# plugin 설치
/plugin install jace@harness
```

Plugin으로 설치하면 skill 명령에 네임스페이스가 붙습니다 (예: `/jace:commit`).

## Skills

| Skill | 설명 |
|-------|------|
| prompt-doctor | LLM 프롬프트 진단 및 최소·고효율 개선 |
| commit | 컨벤션에 맞는 커밋 메시지로 git commit 생성 |
| pr | GitHub PR 생성 및 업데이트 |

## 개발 및 테스트

```bash
claude --plugin-dir ./
```
