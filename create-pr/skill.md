---
name: create-pr
description: 建立 Pull Request 的完整工作流程。當使用者想要發 PR、建立 Pull Request、提交程式碼審查請求、或說「幫我開一個 PR」、「發 PR」、「submit PR」時，一定要使用這個 skill。也適用於需要準備 PR 描述、branch 命名、commit message 整理的場景。
---

# Create PR Skill

協助開發者完成從 branch 整理到發出 Pull Request 的完整流程。

## 工作流程

### Step 0: 確認參數

如果我有給你 target branch 就把develop換成 target branch。
我會給你reviewers跟labels
如果有缺漏就先問我

### Step 1：確認當前狀態

先執行以下指令了解現況：

```bash
git status
git log develop..HEAD --oneline   # 查看本 branch 的 commits
git diff develop --stat           # 查看異動檔案摘要
```

如果還在 target/develop branch 上，先詢問使用者要建立什麼功能的 branch。

---

### Step 2：整理 Branch 名稱

Branch 命名規則（依類型選擇前綴）：

| 類型 | 前綴 | 範例 |
|------|------|------|
| 新功能 | `feat/` | `feat/user-login` |
| 修 bug | `fix/` | `fix/null-pointer-crash` |
| 重構 | `refactor/` | `refactor/auth-module` |
| 文件 | `docs/` | `docs/api-readme` |
| 緊急修復 | `hotfix/` | `hotfix/payment-broken` |

若 branch 名稱不符合規範，詢問使用者是否要 rename：
```bash
git branch -m old-name new-name
```

---

### Step 3：整理 Commits（選做）

若 commits 很零碎（如多個 "WIP"、"fix"），建議 squash：

```bash
# 互動式 rebase，整理最近 N 個 commits
git rebase -i HEAD~N
```

整理後建議的 commit message 格式：
```
<type>(<scope>): <簡短描述>

<選填：詳細說明>

<選填：Related issue / breaking changes>
```

type 可選：`feat`, `fix`, `refactor`, `docs`, `test`, `chore`

範例：
```
feat(auth): 新增 Google OAuth 登入功能

- 整合 Google OAuth 2.0
- 儲存 token 至 localStorage
- 加入登出功能

Closes #42
```

---

### Step 4：Push Branch

```bash
git push -u origin HEAD
```

若遇到衝突，提示使用者先 rebase：
```bash
git fetch origin
git rebase origin/develop
```

---

### Step 5：產生 PR 描述

根據 diff 與 commits 自動產生 PR 描述，格式如下：

```markdown
## 📋 Summary
<!-- 這個 PR 做了什麼？一句話說明 -->

## 🔍 Changes
<!-- 主要變更列表 -->
- 
- 

## 🧪 Testing
<!-- 如何測試這個變更？ -->
- [ ] Unit tests 通過
- [ ] 手動測試：（描述測試步驟）
```

執行這個指令取得 diff 摘要供產生描述用：
```bash
git diff develop --stat
git log develop..HEAD --pretty=format:"%s" 
```

---

### Step 6：建立 PR

**使用 GitHub CLI（推薦）：**
```bash
gh pr create \
  --title "<PR 標題>" \
  --body "<PR 描述>" \
  --base develop \
  --head $(git branch --show-current)
```

加上 labels / reviewer（若有需要）：
reviewer 根據我給你的名字去找出對應的人。
```bash
gh pr create \
  --title "<PR 標題>" \
  --body "<PR 描述>" \
  --reviewer teammate-username \
  --label "feature"
```

**若沒有 gh CLI，輸出瀏覽器連結：**
```bash
git remote get-url origin
# 組成 URL：https://github.com/<org>/<repo>/compare/develop...<branch>
```