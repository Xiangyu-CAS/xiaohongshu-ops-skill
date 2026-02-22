# OpenClaw 一键安装（改进版，已踩坑修正）

## 这次遇到的问题总结

1. `brew` 路线在无管理员权限的 macOS 上会失败（需要 `sudo`）。
2. OpenClaw 新版本（如 `2026.2.21-2`）下，`OpenAI Codex OAuth` 不能走 `--non-interactive`，会报 `OAuth requires interactive mode.`。
3. 用 `nvm` 安装后，旧终端或非交互 shell 里可能出现 `npm/openclaw command not found`（环境未加载 `nvm`）。
4. 只做 onboarding 但不启动 gateway 时，`openclaw status` 可能显示 `Gateway unreachable`。

## 目标

- 新机器 0 到 1 安装 OpenClaw
- 使用 OpenAI Codex OAuth
- 尽可能自动化，碰到版本差异时自动回退到可用流程

## 一键脚本（推荐，直接粘贴执行）

```bash
#!/usr/bin/env bash
set -euo pipefail

if [[ ! -t 0 || ! -t 1 ]]; then
  echo "请在交互式终端中运行（OAuth 需要浏览器 + 终端交互）。"
  exit 1
fi

export NVM_DIR="${NVM_DIR:-$HOME/.nvm}"

ensure_nvm_loaded() {
  if [[ -s "$NVM_DIR/nvm.sh" ]]; then
    # shellcheck disable=SC1090
    . "$NVM_DIR/nvm.sh"
    return
  fi

  echo "[1/6] 安装 nvm..."
  curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
  # shellcheck disable=SC1090
  . "$NVM_DIR/nvm.sh"
}

ensure_node_npm() {
  if command -v node >/dev/null 2>&1 && command -v npm >/dev/null 2>&1; then
    return
  fi

  echo "[2/6] 安装 Node LTS + npm..."
  ensure_nvm_loaded
  nvm install --lts
  nvm alias default lts/* >/dev/null || true
}

use_node_lts() {
  ensure_nvm_loaded
  nvm use --lts >/dev/null || nvm install --lts >/dev/null
}

install_openclaw() {
  if command -v openclaw >/dev/null 2>&1; then
    return
  fi
  echo "[3/6] 安装 openclaw..."
  npm i -g openclaw
}

run_onboard() {
  local args=(
    --auth-choice openai-codex
    --flow quickstart
    --skip-channels
    --skip-skills
    --skip-ui
    --skip-daemon
  )

  echo "[4/6] 尝试非交互 onboarding（若当前版本不支持 OAuth 非交互会自动回退）..."
  set +e
  openclaw onboard --non-interactive --accept-risk "${args[@]}"
  local rc=$?
  set -e

  if [[ $rc -ne 0 ]]; then
    echo "检测到当前版本 OAuth 需交互，切换到交互模式继续..."
    echo "你只需要完成浏览器 OAuth；若出现 'Enable hooks?'，选择 'Skip for now' 并回车。"
    openclaw onboard "${args[@]}"
  fi
}

start_gateway() {
  echo "[5/6] 安装并启动 gateway 服务..."
  openclaw gateway install || true
  openclaw gateway start || true
}

verify_and_print() {
  echo "[6/6] 验证安装..."
  local model token
  model="$(openclaw config get agents.defaults.model.primary || true)"
  token="$(openclaw config get gateway.auth.token | tr -d '\"' || true)"

  openclaw status | sed -n '1,35p'
  echo
  echo "Model: ${model:-<unknown>}"
  echo "Token: ${token:-<unknown>}"
  echo "Dashboard: http://127.0.0.1:18789/"
}

ensure_node_npm
use_node_lts
install_openclaw
run_onboard
start_gateway
verify_and_print
```

## 为什么这个版本更稳

1. 默认走 `nvm`，避免 `brew/sudo` 权限问题。
2. 先尝试 `--non-interactive`，失败后自动回退到交互 OAuth，兼容新版本行为。
3. 脚本内始终显式加载 `nvm`，避免 `npm/openclaw` 路径丢失。
4. 安装后直接 `gateway install + start`，减少 `status` 显示 unreachable 的情况。

## OAuth 手动步骤（保留）

1. 浏览器打开授权页后登录 OpenAI。
2. 同意授权，等待终端显示 `OpenAI OAuth complete`。
3. 若提示 `Enable hooks?`，选 `Skip for now` 并回车。

## 安装后常用命令

```bash
openclaw status
openclaw dashboard
OPENCLAW_TOKEN="$(openclaw config get gateway.auth.token | tr -d '\"')"
openclaw tui --token "$OPENCLAW_TOKEN" --url ws://127.0.0.1:18789
```

## 如果又出现 `npm/openclaw command not found`

在当前终端执行：

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && . "$NVM_DIR/nvm.sh"
nvm use --lts
```

## Dashboard 报错：`disconnected (1008): unauthorized: device token mismatch`

现象：页面反复断开，并提示 `rotate/reissue device token`。

原因：浏览器里缓存的 device token 与网关当前 token/权限状态不一致（常见于重装、重配、scope 升级后）。

按顺序执行下面三步：

```bash
# 1) 批准最新修复配对请求（repair/scope-upgrade）
openclaw devices approve --latest

# 2) 重新生成并打开当前有效 dashboard 链接
openclaw dashboard --no-open

# 3) 若仍不通，清理设备记录并重新配对
openclaw devices clear --pending --yes
openclaw dashboard --no-open
```

判断修复成功的标志：

- `openclaw devices list` 里无 pending 或 pending 很快被批准
- gateway 日志出现 `webchat connected`
- 页面不再出现 `1008 unauthorized`
