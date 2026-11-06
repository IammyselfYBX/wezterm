# WezTerm 配置部署说明

仓库使用 Git SSH 协议：

```bash
git@github.com:IammyselfYBX/wezterm.git
```

配置入口是仓库根目录的 `wezterm.lua`，其中的模块通过
`require("utils/keys")` 等相对路径加载。因此应软链接整个仓库目录，
不要只软链接单个 `wezterm.lua` 文件。

## Linux / macOS

首次克隆并应用：

```bash
# 1. 克隆（推荐放在用户主目录）
git clone git@github.com:IammyselfYBX/wezterm.git "$HOME/wezterm"

# 2. 如果已有旧配置，先备份
if [ -e "$HOME/.config/wezterm" ] || [ -L "$HOME/.config/wezterm" ]; then
    mv "$HOME/.config/wezterm" "$HOME/.config/wezterm.bak.$(date +%Y%m%d-%H%M%S)"
fi

# 3. 链接整个配置目录
mkdir -p "$HOME/.config"
ln -s "$HOME/wezterm" "$HOME/.config/wezterm"
```

应用后：

```bash
# 检查配置语法和键位加载
wezterm --config-file "$HOME/wezterm/wezterm.lua" show-keys >/dev/null

# 完全退出旧 WezTerm 后重新启动
wezterm
```

当前机器 `192.168.0.154` 已配置为：

```text
/home/tony/.config/wezterm -> /home/tony/wezterm
```

## Windows PowerShell

```powershell
git clone git@github.com:IammyselfYBX/wezterm.git "$HOME\wezterm"
New-Item -ItemType Directory -Force "$HOME\.config" | Out-Null
if (Test-Path "$HOME\.config\wezterm") {
    Rename-Item "$HOME\.config\wezterm" "wezterm.bak.$(Get-Date -Format yyyyMMdd-HHmmss)"
}
New-Item -ItemType SymbolicLink -Path "$HOME\.config\wezterm" -Target "$HOME\wezterm"
```

Windows 创建符号链接如果提示权限不足，请开启“开发者模式”，或以管理员权限执行一次。
也可以用 `wezterm --config-file "$HOME\wezterm\wezterm.lua"` 验证配置。

## 已有克隆的更新流程

软链接建立后，后续只需更新仓库：

```bash
cd "$HOME/wezterm"
git pull --ff-only
```

然后重启 WezTerm。软链接无需重复创建。

## 常见检查

```bash
# 查看配置链接
ls -ld "$HOME/.config/wezterm"

# 查看实际配置路径
readlink -f "$HOME/.config/wezterm/wezterm.lua"

# 检查配置能否被 WezTerm 解析
wezterm --config-file "$HOME/wezterm/wezterm.lua" show-keys >/dev/null
```

如果 WezTerm 中字体显示为方框，请安装仓库推荐的 Nerd Font，并重启 WezTerm。
不要将 SSH 私钥提交到本仓库。
