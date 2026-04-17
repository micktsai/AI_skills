---
name: unit-test
description: 撰寫unit test，當使用著有製作unit test的需求時一定要使用這個skill
---

# Create PR Skill

協助開發者撰寫符合 Golang 慣例的高品質單元測試，確保邊界條件被完整覆蓋。

## 工作流程

### Step 0: 確認工作範圍

先使用git 確認這次需要測試的範圍與項目，包含需要新增或修改的檔案以及功能

### Step 1: 

撰寫相關的unit test。
以下是要求：
- 如果沒有相關的測試檔案存在，創建一個_test.go的檔案用以撰寫unit test。
- coding style要跟現有的維持一致。
- 測試的覆蓋範圍要盡可能廣。
- 測試用的參數先從其他檔案找，實在找不到就先亂數產生並且標記TODO讓我知道。
- 如果發現測試的功能有異常就直接修正。
- 如果遇到連線問題就先不用測試，先專心完成其餘的unit test。
- 覆蓋率要盡可能高，fail case也要。