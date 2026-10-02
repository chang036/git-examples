# ccc: git Branch 怎麼用

Git Branch（分支）是 Git 非常核心且強大的功能，它允許你在不影響主要程式碼（通常是 `main` 或 `master`）的情況下，進行新功能開發、測試或修復錯誤。

以下為你整理最常用、最重要的 Git Branch 操作指令與流程：

---

### 一、 查看分支

* **查看目前有哪些分支**（前方有 `*` 號代表你目前所在的分支）：
```bash
git branch

```


* **查看所有分支**（包含遠端的分支）：
```bash
git branch -a

```



---

### 二、 建立與切換分支

* **建立新分支：**
```bash
git branch <分支名稱>

```


* **切換到指定分支：**
```bash
git checkout <分支名稱>

```


* **【常用捷徑】建立並立即切換到新分支：**
```bash
git checkout -b <分支名稱>

```


*(註：新版的 Git 也常用 `git switch -c <分支名稱>` 來建立並切換)*

---

### 三、 合併分支 (Merge)

當你在自己的分支完成功能開發，且確認沒問題後，通常會切換回主分支（如 `main`）將程式碼合並回來：

1. **先切換回主分支：**
```bash
git checkout main

```


2. **更新主分支**（確保是最新狀態）：
```bash
git pull

```


3. **將功能分支合併到主分支：**
```bash
git merge <你的分支名稱>

```



---

### 四、 刪除分支

* **刪除已經合併的分支**（安全刪除）：
```bash
git branch -d <分支名稱>

```


* **強制刪除分支**（未合併的修改會遺失）：
```bash
git branch -D <分支名稱>

```



---

### 五、 標準工作流程範例

假設今天要開發一個「登入功能 (`login`)」：

1. 確保目前在主分支並且是最新狀態：
```bash
git checkout main
git pull

```


2. 建立並切換到新分支 `feature-login`：
```bash
git checkout -b feature-login

```


3. 開始寫程式碼，完成後加入版控並提交：
```bash
git add .
git commit -m "完成登入功能"

```


4. 切換回 `main` 分支：
```bash
git checkout main

```


5. 更新 `main` 並合併 `feature-login`：
```bash
git pull
git merge feature-login

```


6. 推送到遠端倉庫（如 GitHub）：
```bash
git push origin main

```


7. 刪除本地的 `feature-login` 分支：
```bash
git branch -d feature-login

```



---

> **💡 小提示：** 如果在合併時遇到 **Conflict（衝突）**，不用緊張，Git 會標示出衝突的檔案。手動打開檔案修改掉衝突的部分（保留想要的程式碼），接著再次執行 `git add .` 與 `git commit` 就能完成合併！

請問你在使用分支時遇到了什麼特定的狀況，或是想了解更進階的技巧（例如 `rebase` 或 `git stash`）嗎？

# ccc: git checkout -b 和 branch 有何關係

`git checkout -b` 和 `git branch` 其實是**相輔相成**的親兄弟，它們在底層都在處理「分支」這件事，但分工有些不同。

簡單來說：

* **`git branch`** 是用來**管理分支**（建立、查看、刪除）的基礎工具。
* **`git checkout -b`** 是一個**組合技**（捷徑），等於「建立分支」加「切換過去」兩個動作一次完成。

---

### 一、 拆解兩者的關係與運作

如果我們把指令拆開來看：

1. **單獨使用 `git branch <分支名稱>`：**
* **作用：** 單純在目前的提交（Commit）點上**生出一個新的分支指標**。
* **缺點：** 執行後，你**依然停留在原本的分支**上，指標沒有跟著過去。


2. **單獨使用 `git checkout <分支名稱>`：**
* **作用：** 切換到已經存在的分支。


3. **結合起來 `git checkout -b <分支名稱>`：**
* **作用：**
1. 先執行 `git branch <分支名稱>`（建立分支）
2. 再立刻執行 `git checkout <分支名稱>`（切換過去）





---

### 二、 對比範例

#### 寫法 A：分兩步完成

```bash
# 1. 建立名為 feature 的分支（人還停留在 main）
git branch feature

# 2. 切換到 feature 分支
git checkout feature

```

#### 寫法 B：用 `-b` 一步到位（最常用）

```bash
# 同時完成「建立」與「切換」
git checkout -b feature

```

---

> **💡 現代 Git 的補充小知識：**
> 因為 `checkout` 這個指令同時能做「切換分支」和「還原檔案」兩種不同任務，容易讓新手混淆，所以新版的 Git 引入了更直覺的新指令：
> * 用 **`git switch`** 來代替切換分支（例如 `git switch main`）
> * 用 **`git switch -c`** 來代替 `git checkout -b`（例如 `git switch -c feature`）
> 
> 
> 不過大家習慣上還是很常用 `git checkout -b`，兩者效果完全一樣！