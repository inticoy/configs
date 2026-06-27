# configs

개인 맥 세팅. `github.com/inticoy/configs`

> 설치 스크립트 대신 **이 README가 런북**이다. 새 맥에서 직접 따라 하거나, Claude Code·Codex에게 "아래 Setup대로 이 맥 세팅해줘"라고 맡기면 된다.

## 구성

| | |
|------|------|
| `Brewfile` | 앱·CLI (`brew bundle`) |
| `git/` | gitconfig · gitignore_global |
| `apps/` | iterm · karabiner · linearmouse · raycast |
| `agents/` | AI 에이전트 설정 (skills · mcp.json) |

## Setup (새 맥)

### 0. 부트스트랩 (수동)
```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
brew install --cask claude        # 또는: npm i -g @openai/codex (이후 단계는 에이전트에 맡겨도 됨)
```

### 1. clone
```bash
git clone git@github.com:inticoy/configs.git ~/Personal/inticoy/configs
cd ~/Personal/inticoy/configs
```

### 2. 패키지
```bash
brew bundle --file=Brewfile
```

### 3. dotfile 심링크
> ⚠️ 대상이 이미 실제 파일이면 먼저 백업(`mv ~/.gitconfig ~/.gitconfig.bak`) 후 링크.
```bash
ln -sfn "$PWD/git/gitconfig"                 ~/.gitconfig
ln -sfn "$PWD/git/gitignore_global"          ~/.gitignore_global
mkdir -p ~/.config/karabiner
ln -sfn "$PWD/apps/karabiner/karabiner.json" ~/.config/karabiner/karabiner.json
```

### 4. git 신원
이름·이메일은 머신별 `~/.gitconfig.local`(gitignore)에 둔다:
```bash
cat > ~/.gitconfig.local <<'EOF'
[user]
    name = <이름>
    email = <이메일>
EOF
```
디렉토리별로 다른 이메일을 쓰려면 `git/gitconfig`의 `includeIf` 주석 참고.

### 5. AI 에이전트 (설치된 도구만)
- `agents/skills/<name>` → `~/.claude/skills/`, `~/.codex/skills/` 심링크
- MCP: `agents/mcp.json`의 서버를 각 도구에 등록 (`claude mcp add …` / Codex `config.toml` / Antigravity `mcp_config.json`)

### 6. 앱 설정 (수동)
- **iTerm**: Settings → Profiles → ⚙️ → "Import JSON Profiles" → `apps/iterm/Profiles.json`
- **LinearMouse**: 앱에서 `apps/linearmouse/` import
- **Raycast**: `apps/raycast/raycast.rayconfig` Import (export 시 비밀번호 필요), 또는 Cloud Sync
- **VS Code · IntelliJ**: 각 앱 Settings Sync (여기서 관리 안 함)

### 7. 🔐 시크릿 (수동, git에 안 올림)
- SSH 키 이전 또는 재생성
- CLI 인증 재로그인 (`gh auth login` 등)
- `.npmrc` · `.yarnrc` 토큰 재설정
