# AgentSkills

Skills set for `Claude`, `Codex`, and `Gemini`.

This repository contains reusable skill packages for multiple AI coding agents.  
Use the included `skills` script to install the skill sets into each agent's local skills directory.

## English

### Overview

- `claude-skills/`: skills for Claude
- `codex-skills/`: skills for Codex
- `gemini-skills/`: skills for Gemini
- `skills`: install helper script

### Install

```bash
./skills install codex
./skills install claude
./skills install gemini
./skills install all
```

Installed locations:

- Codex: `~/.codex/skills/`
- Claude: `~/.claude/skills/`
- Gemini: `~/.gemini/skills/`

You can also check usage with:

```bash
./skills help
```

If a skill already exists in the destination, the script will ask before replacing it.

## 한글

### 소개

이 저장소는 `Claude`, `Codex`, `Gemini`용 재사용 가능한 skills 세트를 모아둔 저장소입니다.  
포함된 `skills` 스크립트를 사용해서 각 에이전트의 로컬 skills 디렉터리로 간단히 설치할 수 있습니다.

### 구성

- `claude-skills/`: Claude용 skills
- `codex-skills/`: Codex용 skills
- `gemini-skills/`: Gemini용 skills
- `skills`: 설치용 헬퍼 스크립트

### 설치

```bash
./skills install codex
./skills install claude
./skills install gemini
./skills install all
```

설치 경로:

- Codex: `~/.codex/skills/`
- Claude: `~/.claude/skills/`
- Gemini: `~/.gemini/skills/`

사용법 확인:

```bash
./skills help
```

대상 위치에 같은 skill이 이미 있으면, 스크립트가 덮어쓸지 먼저 확인합니다.
