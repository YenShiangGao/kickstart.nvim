# Henry's Neovim 設定

我的個人 Neovim 設定，以 [kickstart.nvim](https://github.com/nvim-lua/kickstart.nvim) 為基礎修改而成。
這個 repo 是我實際使用中設定的線上備份（source of truth）。

架構為 lazy.nvim 單檔設定，所有客製化都集中在 `init.lua`。
`lazy-lock.json` 有納入版控，用來鎖定插件版本。

## 安裝

```sh
# 備份既有設定（如果有的話）
mv ~/.config/nvim ~/.config/nvim.bak
mv ~/.local/share/nvim ~/.local/share/nvim.bak

git clone git@github.com:YenShiangGao/kickstart.nvim.git ~/.config/nvim
nvim
```

第一次啟動時 lazy.nvim 會自動安裝所有插件。

### 外部相依

- 基本工具：`git`、`make`、`unzip`、C compiler（`gcc` / `clang`）
- [ripgrep](https://github.com/BurntSushi/ripgrep)、[fd](https://github.com/sharkdp/fd)（Telescope 搜尋用）
- Node.js / `npm`（markdown-preview.nvim 的 build 需要）
- Go（gopls、neotest-golang）
- Nerd Font（terminal 需選用）

## 相對於 kickstart 的客製內容

### 外觀

- 主題改用 [cyberdream.nvim](https://github.com/scottmckendry/cyberdream.nvim)（透明背景、飽和度 0.7、註解斜體），停用預設的 tokyonight
- Treesitter 程式碼摺疊（`foldmethod=expr`，預設全部展開）

### Go 開發

- 啟用 `gopls`（透過 Mason 安裝）
- `*.go` 存檔時自動以 LSP format
- `go.tmpl` / `go.work` filetype 對應
- [neotest](https://github.com/nvim-neotest/neotest) + neotest-golang 跑測試

### 工作流

- `autoread` + `checktime` autocmd：檔案被外部修改（例如 AI 工具改 code）時自動刷新 buffer
- [oil.nvim](https://github.com/stevearc/oil.nvim)：用 buffer 的方式編輯檔案系統，`-` 開啟上層目錄
- [persistence.nvim](https://github.com/folke/persistence.nvim)：session 管理
- [markdown-preview.nvim](https://github.com/iamcco/markdown-preview.nvim)：`:MarkdownPreview` 瀏覽器即時預覽
- gitsigns 完整 hunk 操作 keymaps

## 自訂 Keymaps

Leader 鍵是 `<space>`。

| 按鍵 | 功能 |
| --- | --- |
| `-` | Oil 開啟上層目錄 |
| `<leader>qs` / `<leader>ql` | 還原 cwd session / 還原上一個 session |
| `<leader>qd` | 本次不儲存 session |
| `<leader>tt` / `<leader>tf` / `<leader>ta` | 跑最近的測試 / 檔案測試 / 全部測試 |
| `<leader>ts` / `<leader>to` / `<leader>tO` | 測試摘要 / 輸出 / 輸出面板 |
| `<leader>tx` | 停止測試 |
| `]c` / `[c` | 下一個 / 上一個 git hunk |
| `<leader>hs` / `<leader>hr` | stage / reset hunk（支援 visual mode） |
| `<leader>hS` / `<leader>hR` | stage / reset 整個 buffer |
| `<leader>hp` / `<leader>hb` / `<leader>hd` | 預覽 hunk / blame 該行 / diff |

其餘 keymaps（Telescope、LSP 等）沿用 kickstart 預設，見 `init.lua` 內的註解，或按 `<space>` 後透過 which-key 查看。

## 同步流程

本機設定在 `~/.config/nvim`，改完後 commit 並 push 到這個 repo：

```sh
cd ~/.config/nvim
git add -A && git commit -m "..." && git push
```

## License

MIT，見 [LICENSE.md](LICENSE.md)。原始範本版權屬 kickstart.nvim 作者群。
