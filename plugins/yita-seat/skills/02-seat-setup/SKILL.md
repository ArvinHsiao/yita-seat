---
name: 02-seat-setup
description: "第一次開工前讀一次：怎麼接 YITA Builder（按客戶端分小節、Google 登入）、開工前拿 token 字典與怎麼挑 block、怎麼開座位、③ 開跑前要備好什麼、卡住時先看哪。不講畫法（畫的時候翻 03-page-draw-guide）。"
---

# 座位開始指南

> ⭐ **先讀 [01-seat-rules](../01-seat-rules/SKILL.md)**。本頁任何一句與它衝突，**以它為準**，並回報那一句。
> **這頁只講「怎麼開始」** —— 接線、開工前拿什麼、開座位、卡住看哪。
> **畫稿怎麼寫**看 [03-page-draw-guide](../03-page-draw-guide/SKILL.md)；**工具怎麼叫、參數、失敗碼**一律 `playbook-get`（不知道從哪查就先 `name="lite-index"`）。
> 座位**只碰 YITA Builder 的 MCP 工具**，不需要下載任何程式、不需要本機環境。

---

## 0. 每輪開工前：確認手冊是最新的

工具與規則幾乎每天都在改。用舊手冊畫，**每一步都照樣做得完、結果卻是照舊規則做的** —— 沒有任何錯誤訊息。

| 你怎麼讀手冊 | 開工前做什麼 |
|---|---|
| Claude Code 裝了 `yita-seat` plugin | 手冊隨 plugin 版本走；不確定是不是最新，就照 §2-1 重新安裝一次 |
| 其他工具用 GitHub 連接器讀 | 每次開工**重新讀** repo 裡的三本手冊，⛔ 不要沿用上次存下來的副本 |


---

## 1. 你要跑的是什麼

一頁網站分三段做，每一段走完停下來讓人看：

| 段 | 做什麼 | 誰做 |
|---|---|---|
| **① 形狀** | 用既有 block 把頁排出來（插 block、決定幾格、填站主給的文字） | **座位**（一個 AI session） |
| **② 感覺** | 寫畫稿改長相（色、字、留白、動效），前台即時看得到 | **座位**（同一個，不換人） |
| **③ 收回** | 把畫對的長相翻成全站的 design token 寫回去，然後清掉草稿層 | **座位跑工具、站主答題**（見 §6） |

**你**（開發者 / 站主）在中間：開座位、每一段走完看畫面、說留 / 換 / 好 / 不好、③ 回答題目。

---

## 2. 接線：接上 YITA Builder（做一次就好）

座位需要的只有**一條 MCP 連線**：`https://builder.hauzii.com/mcp`。**不用 key、不用設定檔** —— 第一次叫工具時會開瀏覽器，用 Google 帳號登入。

⚠️ 你的 Google 帳號要在該站的使用者名單內，登入才會通。登入後說「沒有權限」→ 請站主把你的帳號加進那個站。

### 2-1 Claude Code

**建議：裝手冊 plugin**（會一起帶上 `builder` / `builder-dev` 兩條連線與三本手冊）：

```
/plugin marketplace add https://github.com/ArvinHsiao/yita-seat
/plugin install yita-seat@yita-seat
```

**只要連線、不裝手冊**也可以：

```bash
claude mcp add --scope user --transport http builder https://builder.hauzii.com/mcp
```

⚠️ **不加任何 `--header`** —— 連線靠登入，不靠 key。
⚠️ **不加 `--scope user` 只在當下資料夾生效**。
⚠️ **兩條路擇一，不要都做** —— 已裝外掛又手動加一條同名 `builder`，手動那條會安靜蓋掉外掛那條。
⚠️ **裝之前先清掉同名的舊條目** —— 專案資料夾 `.mcp.json` 裡同名、帶 header（key）的條目，會安靜蓋掉你新裝的連線；user 層手動加過的同名條目也一樣。`disabledMcpjsonServers` 只管得到 `.mcp.json`，管不到 user 層。舊版外掛與舊教學用過的連線名（名字裡帶 `hauzii` 的那兩條）也一併清掉。
⚠️ **清掉舊條目後，先重開 Claude Code 再登入** —— 同一個 session 裡，已經移除的條目還會列在 `/mcp`，登入也會回「Authentication successful」，但實際登入的是舊條目。
⚠️ **裝完要重開 Claude Code** —— 工具清單是 session 啟動時載入的，不重開看不到新工具。

### 2-2 其他工具（Claude Desktop / ChatGPT / Codex …）

- **連線**：設定 → 連接器 → 自訂 → 網址填 `https://builder.hauzii.com/mcp`，一樣 Google 登入。
- **手冊**：用 GitHub 連接器讀這份 repo 的 `01-seat-rules.md` / `02-seat-setup.md` / `03-page-draw-guide.md`。

### 2-3 確認通了

叫一次 `site-get-info`（不帶參數）：

| 回什麼 | 意思 | 接下來 |
|---|---|---|
| 站名與網址 | 你的帳號只管一個站，之後都自動用它 | 開工 |
| 「站不明確」並列出好幾個站（`site_id` / 名稱 / 網址） | 你的帳號管好幾個站 | 開工先跟 AI 講要做哪個站；之後每支工具都帶那個站的 `site_id` |
| 要你登入 | 還沒登入或登入過期 | 照瀏覽器跳出的頁面用 Google 登入 |
| 回了站名，但沒跳出登入、也不是你要的站 | 還在吃舊的 key —— 同名舊條目帶著 key 蓋掉了新連線 | 照 §2-1 清掉同名舊條目 → 重開 Claude Code → 再登入 |

⚠️ **同一個帳號可以在好幾台電腦、好幾個視窗同時登入，不會互踢**（2026-09-15 起）。同一台電腦的多個視窗共用同一次登入；每台電腦要各自登入一次。
⚠️ **登入不會自動續期**（沒有 refresh token）：過期了就照瀏覽器跳出的頁面重新登入一次。這是設計如此，不是壞掉。

### 2-4 權限起手清單

**開工前請站主一次放行**下面這些工具，不要一支一支被擋在半路：

`site-get-info`／`playbook-get`／`design-get-dictionary`／`block-browse`／`block-get-template`／`page-clone`／`page-batch`／`page-get-structure`／
`page-audit`／`block-inspect`／`block-update-content`／`block-set-surface`／`block-set-style`／`block-set-image`／`block-set-button`／
`block-set-geometry`／`block-set-slot-visibility`／`block-set-repeater-visibility`／`wp-rest`／
`design-locate-style-layer`／`design-lint-style-layer`／`design-update-style-layer`／`design-token-usage`／
`design-writeback-plan`／`design-writeback-apply`

權限設定裡要填**完整工具名**：`mcp__plugin_yita-seat_builder__` 加上工具名（例：`mcp__plugin_yita-seat_builder__site-get-info`）；要一次放行整條連線，就填 `mcp__plugin_yita-seat_builder`。

⚠️ 這是**起手清單**，不是權限表 —— 實際工具名以你那條 `builder` 連線列出來的為準。


---

## 3. 開工前拿什麼

| 要什麼 | 是什麼 | 怎麼拿 |
|---|---|---|
| **token 字典**（樣式層字典） | 這個站每個「字」（`color.07`、`typo.03`…）的意思、變數名、現值 | `design-get-dictionary`：先不帶參數看目錄（`format:"index"`），再 `format:"md"` + `domain:"<章名>"` 逐章讀 |
| **挑 block** | 這個站裝了哪些 block、哪顆最合這一段 | `block-browse`：六軸（類別 / 用途 / 版型 / 圖 / 背景 / 項目）交集，篩到 ≤10 顆才列清單。叫法看 `playbook-get name="guide/block/browse"` |

```text
design-get-dictionary {}
design-get-dictionary { "format": "md", "domain": "color" }   # 逐章
block-browse { "filters": {} }                                 # 先看全貌，再一次加一個條件
```

- ⚠️ **token 字典一定要分章讀**：整份很大，超過單次回應上限會**被無聲截斷、不報錯** —— 你以為讀完了其實只看到一半。
- ⚠️ 不帶 `design_set` 拿到的是**站台 default 那本**。那頁若有自己一本 Design Set，要帶 `design_set: "<那本的 slug>"`；拿 default 去畫 page-set 那頁，值會是別本的。不確定某頁指派了哪本 → 問站主，或看 `design-list-sets`。
- 每輪 ③ 寫回之後，**重新呼叫一次**就拿到新值；不必先抓前台讓樣式重生，工具自己會。
- 兩個座位同時讀字典**不用互相通知** —— 讀取不會覆蓋任何東西。

---

## 4. 開座位

**另開一個 session**，第一句話貼這段：

```
你是座位。先讀 01-seat-rules（要牢記），再把 02-seat-setup 讀一次；畫的時候翻 03-page-draw-guide。讀檔用 Read 工具，不要 cat。
排頁、畫、收回這一輪都用 builder-dev 這條連線的工具（見 01-seat-rules 規則下方的「這一輪的現況」）。工具怎麼叫先問 playbook-get name="lite-index"。
開工先拿 token 字典（design-get-dictionary，先 format:"index" 看章再逐章讀）；挑 block 用 block-browse。
卡住就問我。
```


- ⚠️ **手冊與字典要用 `Read` 工具讀，⛔ 不要用 Bash `cat`** —— `cat` 會超過 Bash 輸出上限被截斷，座位以為讀完了其實少一截。這句話已經寫進上面那段開場詞裡了。
- **座位 ①②③ 是同一個 session**（它排的頁、它自己畫、它自己收，中間不換人）。
- 座位**不 commit、不 deploy、不動 plugin**。
- 它會在每一段走完停下來給你前台網址，等你說話（四段定義在 [03-page-draw-guide](../03-page-draw-guide/SKILL.md) §0）。⭐ 它**聽不準會先問你**，那是刻意的 —— 回一句比讓它猜著畫一版便宜。

---

## 5. 跑起來之後你要做的事

> ⭐ **新站的起手式**：先畫**首頁** → 拿首頁收 ③ 當**定調輪**（你畫的值就成為新的預設）→ **再畫其他頁**。
> 後面的頁大多會直接吸附到剛定好的預設、題目自動變少。定調輪怎麼答見 [03-page-draw-guide](../03-page-draw-guide/SKILL.md) §C-3。

1. **聊專案**：這頁要給誰看、要什麼感覺、有沒有現成文案與圖。
2. **回答開場那一題**：座位會問「這頁**沿用**站上的字典，還是**全部新開**一套？」—— 不確定就答沿用（預設）。這個答案 ③ 會用到。
3. **骨架**：看它列的段落清單（每段用途 + 候選 block）。點頭才讓它開始。
4. **① 形狀**：看前台的形狀。逐顆說**留**或**換**。
5. **② 感覺**：看長相。逐顆說**好**或**不好**。這裡還可以換 block。
6. **內容**：文案圖片補齊，跑到 `page-audit` PASS。
7. 你說 **yes** → 座位跑 ③，把題目用白話端給你答。

---

## 6. ③ 收回：開跑前備好什麼

**③ 由座位跑、站主答題**（[01-seat-rules](../01-seat-rules/SKILL.md)）。兩支工具：`design-writeback-plan`（出題，反覆帶答案重產到可以執行）→ `design-writeback-apply`（寫回、清草稿層、驗收）。

開跑前四件事：

1. **頁面已公開** —— 工具抓的是訪客看得到的前台，草稿 / 私人頁抓不到（② 畫之前就要公開，對位也一樣）。
2. **草稿層還在** —— 畫完別自己清，那是 ③ 的材料。
3. **在對話裡告知站主**要收哪一頁；**同站一次一頁**。
4. **怎麼問站主**看 [03-page-draw-guide](../03-page-draw-guide/SKILL.md) §C-3；拒跑、partial、續跑這些工具行為看 `playbook-get name="guide/design/writeback"`。

---

## 7. 卡住時先看這幾條

| 症狀 | 多半是 |
|---|---|
| 找不到 YITA Builder 的工具 | 沒重開 Claude Code（§2-1） |
| 叫工具時跳出要你登入 | 還沒登入、登入過期，或連線名改過（改名後要重新登入）（§2-3） |
| 回了站名，但不是你要的站、也沒跳登入 | 還在吃舊的 key：同名舊條目蓋掉了新連線（§2-1 清掉 → 重開 → 再登入） |
| playbook 說有某個參數，我的工具沒有 | 你的 session 比最近一次 builder 更新舊 → 重開 session |
| 工具回 `page_not_public` | 那頁還是草稿 / 私人 → 請站主（或經同意）把頁面公開再試（§6 第 1 條） |
| 工具回 `plugin_version_too_old` 或 `404 unknown_endpoint` | 站上 YITA plugin 太舊 → 請站主先更新 plugin |
| 工具說錯了、回了一個看不懂的碼 | 先照回應裡的 `hint` / `details` 改；碼的意思問 `playbook-get`（對位的碼在 `guide/design/locate`） |
| 改了草稿層前台沒變 | 整頁快取 → 網址加 `?x=<隨機數>` 再看 |
| 字典裡的值跟前台對不上 | 你手上那份是之前讀的 → 重新呼叫 `design-get-dictionary`（§3） |
| `block-browse` 找不到某顆 block | 那顆沒同步到這站、不是家族最新版、或被標成不可挑（結果是**站級快照**） |
| 座位說「這個做不到」 | 表上沒有的東西要**記下來回報**，不是硬做（[03-page-draw-guide](../03-page-draw-guide/SKILL.md) §A-6） |

---

## 8. 相關文件

- 牢記的那一頁：[01-seat-rules](../01-seat-rules/SKILL.md)
- 畫稿怎麼寫、③ 怎麼問：[03-page-draw-guide](../03-page-draw-guide/SKILL.md)
- 工具怎麼叫：`playbook-get name="lite-index"`

