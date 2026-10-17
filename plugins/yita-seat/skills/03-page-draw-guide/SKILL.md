---
name: 03-page-draw-guide
description: "畫的時候翻：一頁草稿頁上每種元件能改什麼、畫稿怎麼寫才收得回、怎麼自驗、③ 收回怎麼問站主。窮舉查表，不必背；工具怎麼叫、失敗碼看 playbook。"
---

# 畫頁手冊 — 畫稿怎麼寫才收得回

> ⭐ **先讀 [01-seat-rules](../01-seat-rules/SKILL.md)**（要牢記的那一頁）。**本頁任何一句與它衝突，以它為準**，並回報那一句。
> **這本講「畫稿怎麼寫」；工具怎麼叫、參數、失敗碼、工具的坑一律 `playbook-get`**（本頁只指路，不重抄）。
>
> 你是「座位」。先用既有 block 把一頁排出來（①），再把它畫成想要的樣子（②）。內容填進 block 的欄位，長相寫成 CSS（草稿層）。畫對了，長相最後會寫回全站的 design token（③，**你跑工具、站主答題**，見 §C-3）。**照 §0 的四段走，每一段走完停下來給站主看；聽不準就先問。**
>
> 表上有的照寫；表上沒有的，照 §A-6 記下來 —— 那是「該補的字」，不是你的錯。

---

## 0. 流程（先照這個走，每一段走完都停下來給站主看）

每一段走完，給站主前台網址、等他說話，沒說「繼續」不往下走。

⭐ **聽不準就先問，⛔ 不要猜了再畫。** 常見聽不準的話：「參考某頁」（**同一個形狀**，還是**同樣的完成度**？）／「擁擠」（**塊與塊之間**、**字與字之間**，還是**卡片內距**？）／「那個標題」（有兩個候選 —— 列出來讓站主指）。只是提醒，不是必問清單。

| 段 | 你做什麼 | 用什麼 | 不做什麼 | 給站主看什麼 |
|---|---|---|---|---|
| **0 骨架** | 列段落清單：每段一句用途 + 候選 block 1–2 顆（沿用還是新開不用你問，見 §A-1） | `block-browse`（六軸交集篩到 ≤10 顆；叫法 `guide/block/browse`） | 不插、不建頁 | 清單。站主點頭才開始 |
| **① 形狀** | 建頁（**建頁即公開**；不想被搜到設 noindex，看 `guide/rank-math`）、插 block、調 repeater 格數、藏多餘格、欄數、錨點；**站主已給的文字**一起填 | `page-batch` 一趟送完（op 名就是工具名，看 `guide/page`「批次寫入」） | **不寫新文案**；沒給文字 / 圖的槽留模板示範內容（這段不是問題，見 §B-7b）；不寫草稿層 | 前台網址 + 「哪幾顆請你看形狀」。站主逐顆說**留 / 換** |
| **② 感覺** | 寫草稿層（色、字、留白、按鈕、動效、漸層與純色底）；背景**圖 / 影片**走 `block-set-surface`；送出後看 `lint_summary` + `design-get-canvas` 回讀（開瀏覽器的時機見 §C-1） | §A–§C | 不改 HTML、不 `!important`、不碰 token | 網址 + lint summary。站主逐顆說**好 / 不好**。⚠️ 畫不出來就回報，⛔ 不要自己翻別的 block 來套；但站主說換 block 就換、換完重畫。**動到 header / footer 要單獨列一段「H/F 改了什麼」**（§B-1） |
| **內容** | 補齊文案、圖、連結（站主沒給的文案 / 圖 / 連結 ⇒ 照 [01-seat-rules](../01-seat-rules/SKILL.md)〈內容沒給、需求和 block 對不上時〉填一眼可辨的佔位、回報逐條列出，⛔ 不停下等）；跑 `page-audit` 到 PASS；**到前台逐槽看一次**（§B-7b） | `page-batch`、`block-set-image`、`block-set-button` | 不再動形狀與草稿層（要動就回到那一段說一聲） | 完整頁。站主說 **yes** |
| **③ 收回** | **③ 收回請站主到後台 Stylebook Merge 頁做**：把**Stylebook Merge 頁網址**給站主，出題、作答、寫入、撤銷寫入都在那一頁（§C-3） | （無 —— 座位不呼叫任何收回工具） | ⛔ 不在對話裡逐題問、⛔ 不替站主選、⛔ 不自己用 `design-update-token` / `block-set-style` 把草稿層一筆一筆「手動收回」、⛔ 站主收完之前不再改這頁草稿層 | Stylebook Merge 頁網址；站主收完後看一次前台 |

- **讀手冊與字典用 `Read` 工具，⛔ 不要 `cat`**（超過 Bash 輸出上限會被截斷，你會以為讀完了）。
- **文字一句話**：有給的就填，沒給的不編。形狀沒定之前不寫新字。
- **內容列表類 block（文章 / Podcast / 商品 / 影片）資料來自後台**：用 `block-set-listing` 設查詢條件（`guide/block/listing`）；後台沒內容就照實回報，⛔ 不拿別的 block 手填假清單。要建示範內容先問站主。
- **接手別人做到一半的頁**：先並行叫三支唯讀工具 —— `page-get-structure`（空著 / 示範 / 藏起來）、`design-get-canvas`（草稿層調過什麼、有沒有沒生效的）、`design-get-page-style`（永久自訂段）—— 各看哪幾個欄位見 `guide/page`「接手別人做到一半的頁」。要截整頁圖先停進場動效（`guide/design/verify`）。
- **優先挑有門牌的 block**（最外層 `<section>` 帶 `data-hz-gap`，`block-get-template` 看得到）。沒門牌的只能整頁一起改，視為「換 block」訊號。
- **每一段的回報固定三行**：做了什麼 / 前台網址 / 請你看哪幾顆。細節放 §D。
- **順手記帳**：到站主第一次看到畫面共幾趟、哪幾趟是補呼叫。

---

## A. 原則

### A-0 精神（四句話）

1. **① 是名詞、② 是形容詞。** ① 決定「這頁有什麼」；② 決定「它們長什麼樣」。
2. **先看現況（canvas），再改；改完看回讀，交件前量一次。** ⛔ 不憑截圖或印象判斷現在是什麼值、也不憑「我寫了」判斷改到了。畫之前先叫一次 `design-get-canvas`：每個元素的選擇器、token、草稿層現值一次拿到（`guide/design/canvas`）；送出後看 `lint_summary` 與 canvas 的 `draft` 回讀；什麼時候還要開瀏覽器量 computed 見 §C-1（量法 `playbook-get name="guide/design/verify"`）。
3. **字典是查「現在是什麼」，不是菜單。** 要什麼值直接寫，收斂是 ③ 的事。
4. **限制不是「不給你做」，是「③ 收不回」。** 撞到限制就回報，那是系統的缺口，不是你做錯。

### A-1 一頁草稿頁 = 你的畫布，兩個通道

| 你要改的是 | 寫在哪 | 動到全站嗎 | 最後 |
|---|---|---|---|
| **這顆 block 自己的資料**：文字、連結、圖、圖示、影片、**背景圖／影片**、顯示幾格、比例裁切、間距級 | block 的欄位（`block-*` 工具） | 不會 | 本來就在家 |
| **長相**：顏色、字級字重字距行高、圓角、陰影、框線、按鈕外觀、留白、動效、hover、**漸層與純色底** | 這頁的**樣式草稿層**（一段 CSS） | 不會 | ③ 寫回 token 或綁定 |

判準：**元素身上有 inline `style="…"` 的 = block 自己的資料 → 工具；其他 = 草稿層。**

先問自己:要的是『換成另一款』,還是『改這一款的幾個值』?
- 字典裡已經有一款**整組**就是你要的(按鈕、面、Badge、字的 preset)→ 用工具換(block-set-style / block-set-surface)。
- 只想改其中幾個值(底色、字級、圓角…),或最接近的那一款還差一截(換過去連字型、行高都跟著變)→ 在草稿層寫你要的值;③ 收回時由站主決定它要不要變成全站的字。
- 拿不準 → 寫草稿層:寫錯只影響這一頁,而且 ③ 收回時會再問一次站主。
- 同一個地方別兩邊都改:草稿畫過的面或間距,之後再用工具換,前台仍以草稿為準,直到 ③ 收回。
⭐ **② 的主要動作是「寫一段 CSS、送一次」。** 發現自己用一長串 MCP 呼叫做「幾行 CSS 就好」的事，停下來回報。
⭐ **除了背景圖 / 影片與內容以外，看得到的東西一律用普通 CSS 直接寫**；純色寫 hex、漸層直接畫（§B-1），③ 會替你開色票、翻成 block 資料。收不回是系統缺口，回報就好。

**沿用還是新開：在 Stylebook Merge 頁開始回收時選（併進現有 Stylebook / 這頁全部新開一本）。** 由站主在那一頁選，座位不用問、也不用記；畫法兩種都一樣，直接寫你要的值。站主若先跟你說「這頁要一套自己的長相」，交網址時提醒他在那一頁選「這頁全部新開一本」。**scope 你不用想**：哪些影響全站、哪些只這頁、要不要開新字，全由 ③ 看值分類後問站主。

### A-2 選擇器：你怎麼寫都行，對位會翻（2026-09-07 起）

寫你熟悉的 CSS 指著畫面上的東西就好：

```css
.hz-typo-typo-06                          { letter-spacing: 0.03em; }
section:nth-of-type(2) .hz-surface-surface-05 { background-color: #EDECDD; }
```

送出時先跑**對位**（§C-1），它把選擇器翻成系統認得的形狀，回一張對位表：「你寫的 X → 翻成 Y、命中 N 個」。

- ⚠️ **對位表回的是「我命中了誰」，不是「這一類都改到了」。** 要改一整類之前先數全頁（`document.querySelectorAll(...).length`）。
- **末端沒有地址就整條退到祖先** —— 看它翻成的 `to` 是誰，⛔ 不要只看命中數就放行。
- **後代選擇器碰上 repeater 是乘法**（格數 × 槽數）；數字突然很大多半是你指到模板了。
- **屬性選擇器只支援 `=`**；**一條規則一個選擇器**（逗號並列不合法）。
- **`[data-a2ui-component="X"]` 是限縮到某一顆 block 的主力寫法**（`:nth-of-type` 會隨插拔位移）。⚠️ 地址常就掛在 block 根上（`<section>` 同時帶 `data-a2ui-component` 與 `data-hz-surface`）→ 寫**複合選擇器**（兩個屬性黏在一起、不空格）；`design-get-canvas` 的 `on_root` 會告訴你。
- 翻不動的它會指名（第幾行、原文、為什麼）。**有一條對不到就整份不送。** 失敗碼與警示碼 → `guide/design/locate`。
- **狀態偽類**（`:hover` / `:focus` / `:focus-visible` / `:active`）要寫在**地址**上、放在選擇器**最後**才翻得動（例 `[data-hz-surface="…"]:hover`，③ 收回時搬進自訂樣式段）。仍翻不動的：偽元素（`::before` / `::after`）、掛在整頁字義 class 上（`.hz-typo-typo-NN:hover`）、放在中間（`.a:hover .b`）、`:not(:hover)`、`:visited` —— 改寫法或拿掉那條、回報一聲（說法以 `guide/design/locate` 的 `unsupported_selector` 那一列為準）。
- **整頁的字寫不進系統**：寫在 `body` / `html` / `*` / `:root` 上的裸屬性（`font-size` / `color` / `background-color`…）系統沒有「全站預設」這一層可收，對位會擋 —— 寫在具體元素上（`p`、`h1`、`.card`），效果一樣、③ 收得回。
- 結構偽類都可以用：`:first-child` / `:last-child` / `:first-of-type` / `:last-of-type` / `:only-child` / `:only-of-type` / `:nth-child(N)` / `:nth-of-type(N)` / `:nth-last-child(N)` / `:nth-last-of-type(N)`（**N 只收整數**，不收 `2n+1`）/ `:not(選擇器)`。
- ⚠️ 結構偽類要寫在**地址**上（例 `[data-hz-text="…"]:first-child`）。⛔ 不要掛在字義 class 上（`.hz-typo-typo-03:not(.x)`）—— 字義 class 的意思是「這個字整站一起變」，掛了偽類就變成指某幾顆元素，而那個字也掛在 header / 手機抽屜這些沒地址的地方，對位會擋。要指某幾顆就寫地址，要改字義就只寫 class。
- **字型只能寫在字義 class 上**（`:root body .hz-typo-typo-NN { font-family: … }`，整頁這個字一起換）；寫在地址底下（`[data-hz-text="…"] { font-family: … }`）會被擋 —— 目前不能只換某一句。
- ⚠️ 同一顆 block 裡多顆同地址時，結構偽類**選不到其中一顆**，會出 `target_retargeted` / `pseudo_narrows_shared_slot` —— 停下回報，不要送。
- ⚠️ `:nth-of-type` 是相對**父層**算的，不是整頁；不確定就看對位表實際命中誰。

⛔ **不准 `!important`** —— 你也不用繞路。被 block 的 inline style 釘住的屬性，**對位會自己判斷、能補強就補**（輸出裡的 `/* hz:pinned */` 是機器寫的）；補不了會當場擋下並指名該走哪支 `block-set-*`。寫了沒反應、對位也沒說什麼，才是異常，回報。⛔ 不改 HTML。

### A-3 值從字典選

token 字典用 `design-get-dictionary`（[02-seat-setup](../02-seat-setup/SKILL.md) §3）。⭐ **寫你要的值**（hex／帶單位的長度）；字典是查「這塊現在綁哪個字、現值多少」的。引用 `var(--hz-…)` 合法但不必要。
值域：字距九檔（−.05 / −.025 / 0 / .025 / .05 / .1 / .15 / .2 / .3em）、字重 100–900 整百、行高 1.1–1.8 —— ③ 會吸附，你不用先算。
⚠️ **字典的 name / label 可能跟值對不上**（token 改過、名字沒改），判斷一律看 value。上一輪 ③ 寫回之後**重新拿一次字典**。

### A-4 怎麼認出畫面上的哪一句

看這頁前台 HTML（網址加 `?x=<隨機數>` 避快取）。元素身上的 class 就夠用：
`hz-typo-typo-03` = 用 typo.03；`hz-btn-button-01` = button.01；`hz-text-color-07` = color.07；`hz-surface-surface-05` = surface.05。
標籤 + typo 認得出是哪一句：`h1`+`typo-01` 主標、`h2`+`typo-03` 區段標題、`p`+`typo-05` 副標、`typo-06` 內文、`typo-11` 上標。

→ 只改「大隱塾光」那一句：`section:nth-of-type(1) h2.hz-typo-typo-03 { … }`
→ 全站所有區段標題一起變：`.hz-typo-typo-03 { … }`（= 改那個字的意思；站上沒有同身份的格可比時，③ 會直接開新格，見 §B-14）

### A-5 坑

1. h1–h6 字級直接寫 `font-size` / `font-weight` / `line-height`，對位會補內部變數（`--_hz-fs` 那三個是輸出，⛔ 不是你寫的）。只寫那三個變數、不寫真屬性會被擋（`heading_var_unpaired`）—— 寫在標題本身、用真屬性。
2. ⭐ **改了桌機字級，就一定要交代平板 / 手機**，不寫會被擋（`ladder_undecided`）。二選一：
   - **① 自己寫**：同一條再包進字典宣告的兩個斷點（目前 `@media (max-width: 1023px)` / `(max-width: 767px)`，以字典 typography 章的 `breakpoints` 為準）。寫別的寬度會被擋（`breakpoint_not_wp`）。
   - **② 交給機器**：畫稿**最前面加一行** `/* hz-ladder: follow */`（整份畫稿的字級都跟階梯），或在某一條規則裡寫 `--hz-ladder: follow;`（只管那一條，優先於檔頭）。③ 照這格原本的桌機:平板:手機比例推。⚠️ ② 階段前台的平板 / 手機仍吃桌機字級，是正常的；要在 ② 就看手機樣子就用 ①。
   - ⛔ 不要為了過關隨便補 `@media` 值 —— 收之前自己把瀏覽器拉到手機寬度看一眼。
3. **改完看不到變化：先懷疑快取（換 `?x=`），再懷疑寫法。**
4. **`--hz-gap-NN` 寫在某個地址上會繼承給所有後代。**
5. **「對位放行 + lint 全綠 + 畫面零反應」會發生** —— 三個綠燈只證明規則送進去了，不證明它贏過別人。站主說畫面沒變時，開瀏覽器量 computed（§C-1 的 (c)、§C-2）。
6. **③ 收回之後 typo class 會換號**，上一輪抄的 class 作廢；下一輪開工重抓前台，跨輪要穩定指同一個東西用**地址**（`[data-hz-text="…"]`）。
7. **字斷行，調字級與間距一輪沒解 → 是容器寬度的問題、草稿層碰不到 → 停手回報**（換 block 或改文案）。⛔ 不要再調第二、三輪。

### A-6 表上沒有的東西

想做但手冊沒列、字典沒字：**用有地址的原始值寫下來**（`:root body [data-hz-…] { 屬性: 值 }`），lint 放行並標記，回報時列出來。沒有地址可掛的（例如整頁內容寬度）→ 回報說明，不要硬寫。

---

## B. 每種元件能怎麼畫

格式：**地址** ｜ **內容**（block 工具）｜ **長相**（草稿層）｜ **常見目標** ｜ **不能**
> 範例是「機器那一側的形狀」（帶 `data-hz-*`）。你不必照抄，照 §A-2 指到那個元素就行；地址欄告訴你這個元件有哪些槽、哪些是內容哪些是長相。

### B-1 面 surface（section 底、卡片底）

- **地址** `[data-hz-surface="hero.bg"]`（section 級多半叫 `section.bg`）
- **內容**（`block-set-surface`）：背景**圖 / 影片**、清背景、響應式 `media`、遮罩 `overlay`。叫法與坑 → `guide/block/surface-modes`（換 preset 與背景的關係、遮罩要半透明、換圖要帶遮罩都在那）。
- ⚠️ **背景圖前台沒有 alt**（它是 CSS 背景，不是 `<img>`）。有 alt 時，`page-get-structure` 回讀與 `block-set-surface` 回應會在同一層帶 `alt_rendered: false` + `alt_note` 提醒（站台外掛太舊時回讀只列 alt、不會帶 `alt_note`，別把沒提醒當成前台有 alt）；只有用上傳圖片的工具上傳時不會提醒（正式連線目前是 builder 的 `site-upload-media`；拆線後在 desk 連線叫 `upload-media`）。圖上有要讓人讀到的資訊，改放內容圖槽（B-6，前台有 alt）或寫進旁邊的文字。
- **遮罩（`block-set-surface` 的 `overlay`）只收一個顏色**（`rgba(…)`、8 碼 hex 或 token），不收漸層。
- **長相**（`surface.NN` 看 class `hz-surface-surface-NN`）：`--hz-surface-NN-bg` / `-border` / `-border-width` / `-radius` / `-shadow` / `-overlay` / `-overlay-alpha`
  - 整頁：`:root { --hz-surface-09-bg: #F6F1E8; }`
  - 只這個面：`:root body [data-hz-surface="hero.bg"] { --hz-surface-09-bg: #F6F1E8; }`
- **常見目標**：換底色 → 直接寫 hex（③ 替你開色票）；圓角陰影 → `-radius` / `-shadow: var(--hz-shadow-NN)`；壓深 → `-overlay-alpha`。
- **邊框分邊、圓角分角**：寫純屬性（`border-left: 3px solid #hex`、`border-top-left-radius: 0`、`border-style: dashed`），③ 拆成分邊 / 分角欄位。⛔ 不寫 `--hz-surface-NN-border-left-width` 這種變數（寫了零反應）。
- **漸層：直接畫**，③ 收回時翻成 block 的背景資料：
  ```css
  :root body [data-hz-surface="hero.bg"] { background: linear-gradient(135deg, #F6F1E8, #DCDBBD); }
  ```
  ⚠️ 漸層要落在一個面上：寫**面的地址**，或 `:root body .hz-surface-surface-NN`（帶前綴 = 這頁掛那格的面全換）。裸寫 `.hz-surface-surface-NN`、指到 `:root` 或非 surface 的槽 → `gradient_no_slot`；太複雜 → `gradient_unsupported`。草稿層寫 `url(...)` 背景圖會被擋（`background_media`）。
- ⚠️⚠️ **底色動了色相或明度，前景要全部重量一次**（文字、次要文字、框線、圖示、按鈕、hover），**而且往下遞迴**：面裡常還有第二、三層面（卡片裡的標籤底、引言塊）。
- ⚠️ **同一個面要換 preset 又要畫草稿：先用 `block-set-surface` 換面，再畫草稿，草稿寫換完之後那一格的變數**（例：`surface.03` 換成 `surface.05` 後寫 `--hz-surface-05-bg`）。反過來做，草稿寫的是元素已經不掛的那一格 → 零反應。
- ⚠️ **同一個 surface 編號同時當 hero 底與內容底時，只能寫地址**（寫 class = 整頁掛它的都改、互相打架）。
- ⚠️ **面的底色多半接到色票**（`--hz-surface-09-bg: var(--hz-color-01)`）—— 改色票，掛它的面一起變。不想連動就寫那個面的地址。
- ⚠️ **草稿層一定贏**：草稿層畫了漸層 / 底色，又用 `block-set-surface` 換 preset 或設圖，前台看到的仍是草稿層；工具回應的 `warnings` 會指名是草稿層哪一條擋著。要讓工具的值生效，就從畫稿拿掉那條再對位送一次。
  - **前提**：surface 載體與 gap 載體上、工具會寫成 inline 的背景 / 間距屬性（清單見 locate-core `TOOL_INLINE_PROPS`），在 2.2 對位時由機器一律補強，**不看先畫還是先寫** —— 所以就算之後才用工具把背景或間距寫成 inline，草稿照樣贏。舊規則下對位的草稿（2026-09-19 以前）不在這個前提內：畫面顯示的是工具寫的值時，把草稿重新送一次即可。
  - **已知代價**：草稿畫過的面或間距，之後才用工具 / 編輯器改 → 前台**仍顯示草稿**，直到 ③ 收回或清掉那條草稿。
- ⛔ **整頁最底層（body 底色）是寫死的純白**，改色票 37 沒用。要整頁鋪深 → 把每個 section 的面鋪滿。
- **header / footer**：H/F 用專屬色格 `color.22–36` 與尺寸 `--hz-hf-*`（字典 `zone_hf: true` 的格、`hf-sizing` 段）。⚠️ **只有站主明說「H/F 也要改」才動** —— ③ 寫回的是站級 token，全站每一頁都會變；動了就在回報單獨列「H/F 改了什麼」。H/F 尺寸的斷點與 typography 不同，查字典 `domains['hf-sizing'].breakpoints`；⛔ 不寫 `--hz-header-*` / `--hz-footer-*`（下游、寫了沒用）。

### B-2 文字（一般文字）

- **地址** `[data-hz-text="hero.desc"]`
- **內容**（`block-update-content`）：鍵就是 `page-get-structure` 的 `slot`，原樣抄（`guide/block/content-rules`）。
- **長相**：顏色 `--hz-color-NN`；字級／字重／行高／字距 → 整頁 `:root body .hz-typo-typo-NN { … }`，只這句 → 地址底下同四個屬性（記得 A-5-2）。
- **常見目標**：內文柔和 → `:root { --hz-color-09: … }`；內文呼吸 → `.hz-typo-typo-06 { line-height: 1.8 }`；只改某一類文字的色 → `:root body .hz-typo-typo-11 { --hz-color-14: var(--hz-color-16) }`（比逐個地址不會漏）。
- ⚠️ 字級只能靠「換一個字」表達；其餘欄位（字型／字重／行高／字距）對不上時 ③ 落殘留而**不會**偷換別的字。
- **排錯：改 token 沒反應** → 看元素 class 有沒有 `cv:…-[var(--hz-…)]!`，有的話它才是真主人。⛔ 不改那個 token 的全域值，在最近的門牌上重新定義：`:root body [data-hz-gap="門牌"] { --hz-color-20: var(--hz-color-14); }`（值寫 `var(…)` ③ 才寫得回綁定）。
- **字型只能挑站上已有的**：`:root body .hz-typo-typo-NN { font-family: var(--hz-font-<歐文>), var(--hz-font-<中文>); }`，清單看字典「可用字型」。⛔ 不寫字面字型名（`font_not_in_library`）；字型只能寫在字義 class 上（§A-2）。

### B-3 標題（h1–h6，帶 `data-hz-text`）

- 與 B-2 相同，直接寫 `font-size` / `font-weight` / `line-height`（A-5 第 1、2 條）。
- 整頁：`:root body .hz-typo-typo-01 { font-size: 3.75rem; font-weight: 400; line-height: 1.5; letter-spacing: .05em; }`；只這顆：寫地址 ＋ 平板 / 手機交代。
- ⚠️ **同一個 `typo.NN` 常同時掛在標題與非標題上** —— 改它就兩邊一起改。先看前台那個 class 出現在哪些元素上。

### B-4 按鈕

- **地址** `[data-hz-button="hero.cta"]`
- **內容**：文案、連結用 `block-update-content { "hero.cta": { "text": "…", "url": "…" } }`，跟其他文字一趟填（連結收 http(s) 或同頁錨點 `#booking`，錨點用 `block-set-anchor` 設；沒帶 `target` 沿用原本的，要同分頁就明講 `_self`）。`block-set-button` 留給 `target` / 按鈕專屬設定（`guide/block/buttons`）。⚠️ 按鈕 `url: ""` 會回到**模板示範連結**，不是沒有連結；要按鈕消失就藏槽。
- **長相**（class `hz-btn-button-NN`）：`--hz-button-NN-bg` / `-text` / `-border` / `-border-width` / `-radius` / `-padding-x` / `-padding-y` / `-hover-bg` / `-hover-text`
- **常見目標**：空心 → `-bg: transparent; -border: …; -border-width: 1px; -text: …`；藥丸 → `-radius: 999px; -padding-x: 32px`；換成另一款 → `block-set-style { slot, button_preset: "button.05" }`。
- ⚠️ **按鈕內距只收 px**（`-padding-x` / `-padding-y` 寫 em，③ 收不回、留成殘留）；**徽章內距收 em**（§B-5）—— 兩種單位不一樣，別照抄。
- ⚠️⚠️ **改按鈕底色，字色與 hover 要一起看**：只換 `-bg` 很容易撞到原本的 `-text`（例：橘底配棕字）或 `-hover-bg`（hover 變回別的色系）。現值先看 `design-get-canvas`：summary 的 `button` 給底色 / 字色（空心款另帶 `border`），hover、圓角、內距要帶 `detail: "full"` 看 `resolved.button_hover` 等欄位。畫稿裡 `-bg`、`-text`、`-hover-bg`、`-hover-text` 一起交代。
- ⚠️ 同一個 preset 的兩顆按鈕只改一個地址 → 兩顆不一致，那是設計；要全站一致寫 class / `:root`。
- **不能**：在按鈕上直接改字型 / 字級（跟著 `button.NN` 的 typo 走，寫了落殘留 `slot_has_no_facet`）。

### B-5 Badge（藥丸標籤）

- **地址** `[data-hz-badge="hero.badge"]`；**內容**：文字 `block-update-content`，文字 ↔ 藥丸 `block-set-style { display_mode }`
- **長相**（class `hz-badge-badge-NN`）：`--hz-badge-NN-bg` / `-text` / `-border` / `-border-width` / `-radius` / `-padding-x` / `-padding-y`（padding 單位 em）

### B-6 圖片

- **地址** `[data-hz-image="hero.image"]`；**內容**：`block-set-image`；**版面**：`block-set-geometry`（比例、裁切、焦點）。叫法與坑（alt 綁在圖上、清圖 `url:""` 回到示範圖、只改 alt、上傳）→ `guide/block/images`、`guide/block/geometry-style`。
- **長相**兩條路，別混：① 換綁外框 `block-set-style { frame: "surface.NN" }`（block 自己的欄位，③ 不寫回；只借圓角、邊框、陰影三樣，底色與遮罩不跟來）② 草稿層：地址底下 `border-radius` / `box-shadow`（③ 收走）。
- **想要字典沒有的圓角** → 直接寫數字（`border-radius: 8px`），照 §A-6 記一筆；⛔ 不為遷就現有的面改設計。
- ⚠️ 沒設過圖的槽印**模板示範圖**（刻意的設計）。`media_summary.images` 的 `filled` = 你給的圖、`demo` = 前台正在印的示範圖。要它消失就藏槽。（清圖、只改 alt、`filled` / `demo` 分開算要站點外掛 ≥ 2.6.9）
- ⚠️ **前景圖（`<img>`）加不了遮罩**。要「圖 + 一層色 + 上面壓字」→ 改用 B-1 的面當背景，遮罩走 `overlay`。

### B-7 圖示 icon

- **地址** `[data-hz-icon="grp-01.sec-01.icon"]`；**內容**：`block-set-icon`（`guide/block/icons`）；**長相**：顏色 `--hz-color-NN`（地址底下）。
- ⚠️ **沒填的 icon / badge / 按鈕 / 標題群，前台一律印模板示範內容**（不是空白）—— 畫面上一顆「了解更多」看起來完全正常，其實是模板自帶的。不要它就藏槽。

### B-7b 重複格 repeater（卡片牆、清單、tier）

- **現在開了哪幾格**：`page-get-structure` 的 `repeater.visible_now`。⛔ 不從預設格數推。
- **形狀（① 就做）**：顯示幾格 → `block-set-repeater-visibility`（mode 寫法 `guide/block/visibility`）。全部藏光 → `show_first_n` 給 `0`。
- ⭐ **沒填的格，前台印的是模板示範內容 —— repeater 一律是第 1 格那一份**（刻意的設計：block 沒填字也要看得出它的意義）。新打開的格、清空（`""`）的格、沒存過資料的格都一樣：feat 多開三格 = 三張第 1 格的卡；**pricing 多開一個方案 = 方案一的複本（含價錢）**。
  ⇒ 這些「開著但沒填」的槽會列在 `page-get-structure` 的 `empty_fields`（沒帶 `hidden_on` 的 = 前台正在印示範內容）。**開格前先把那幾格填好；交件前 `empty_fields` 裡不該有顯示中的格**。（要站點外掛 ≥ 2.6.9；舊外掛不列 → 到前台逐格看）
- **刪掉第 N 格**：直接藏那一格（`hide_specific`，例 `["grp-01.sec-03"]`）。前台照順序只印開著的格、不留空洞，⛔ 不必把後面的內容往前搬；藏起來的格內容還在（`repeater.hidden_with_content` 會列），要找回來就再打開。
- **想換成別家族的版型**（例：兩欄方案換三欄）：不必刪掉重建，用 `block-upgrade { page_id, block_index, to: "<新 block 名>" }` 先看計畫、再執行（`guide/block/visibility`「常見需求」、`block-upgrade` 工具說明）。
- **巢狀（tier 裡還有 feature）是兩層、各做一次**：藏整個方案寫 `grp-02`，藏方案裡的一條功能寫 `grp-02.sec-05`，兩層互不連動。`show_only` 點名某方案的功能列 → **沒點名的方案整張被藏**（回應會帶 warning `unnamed_groups_hidden` 列出被藏的方案）。⇒ **「某方案只留 N 條」用 `hide_specific` 點名要藏的那幾條**。
- **某一格只藏手機**：repeater 工具沒有 responsive → 對那一格的**面槽**下 `block-set-slot-visibility { slot: "grp-01.sec-02.card", visibility: "show", responsive: { mobile: "hide" } }`。面槽名每顆不同（`.card` / `.item`），看 `block-get-template`。回讀看得到：`page-get-structure` 那顆 block 的 `hidden_slots` 會列 `{ slot, on: ["mobile"] }`（外掛 ≥ 2.6.9）。
- **內容**：第幾格就寫第幾格的地址（`grp-01.sec-03.title`），與顯隱的格名、前台地址同一個字。
- **長相（全部格一致）**：改這顆 block 用到的字（class 或 `:root`）。要**用工具**把每一格的卡片面 / 字色一起換（`block-set-surface` / `block-set-style`），對其中一格送、加 `apply_to_siblings: true`，同名的槽每一格一起寫（含藏起來的格）。⚠️ `design-get-canvas` 各格一樣時只列第 1 格的地址（`repeats: N`）—— 照那個地址畫只改到第 1 格；要全改寫 class，或看對位表的 `note`（「這條會改到 N 個……如果本來就要全部一起改，還有 N 個沒涵蓋到」，只在選擇器沒指定單一顆、又只命中部分同類時出）。別格跟第 1 格不一樣時會另列一筆，`cells` 標是哪幾格。
- **長相（只改某一格）**：做得到，格號在路徑裡：`[data-hz-text="grp-01.sec-01.title"]`、卡片底 `[data-hz-surface="grp-01.sec-01.card"]`。
- **長相（只改某一格的留白）**：**做不到**（格內 gap 載體每格共用同一個號碼），照 §A-6 回報。
- ⚠️ 格內地址不是頁面唯一鍵（別顆 block 也有 `grp-01.sec-01.title`）→ 用 block 根（`[data-a2ui-component]`）或門牌限縮，看命中數。

### B-7c 讓一段東西消失：清空 / 填空白 / 藏槽

| 寫法 | 前台 | 回讀 | 用不用 |
|---|---|---|---|
| 文字寫 `""`（清空） | 印回**模板示範字**（repeater 是第 1 格的） | 列在 `empty_fields`（repeater 顯示中的格也列） | ⛔ 不是「消失」 |
| 文字寫 `" "`（一個空白） | 真的空 | 當成有內容 | 不建議 |
| 按鈕 `url: ""` | 回到模板示範連結 | 列 `empty_fields` | ⛔ 不是「沒有連結」 |
| **藏槽** `block-set-slot-visibility` hide | 不出現 | `page-get-structure`：文字 / 按鈕帶 `hidden_on`，圖 / icon / 面 / 整格進 `hidden_slots` | ⭐ **要它消失就藏槽** |

⚠️ 藏掉一個槽，**它旁邊的留白不會跟著消失**（留白在載體上）。藏完問一句「這段留白還隔開什麼？」，沒有就調小或改 `gap.17`（零）。

### B-8 間距 gap（不是元件，是載體）

- **地址** `[data-hz-gap="kchwd0"]`；載體 class 說它吃哪級：`hz-pad-14`（section 上下留白）、`hz-gap-06`（上方 margin）、`hz-flow-08`（容器 gap，縱橫同值）、`hz-flow-row-08` / `hz-flow-col-06`（單軸）
- **尺**：`gap.01–10` 元素間距（小→大）、`gap.11–16` section 留白（緊→寬）、`gap.17` = 零。
- 整頁改某級的大小：`:root { --hz-gap-12: 5rem; }`
- **改某一格的上下留白（直覺寫法）**：地址底下 `padding-block-start: var(--hz-gap-13); padding-block-end: var(--hz-gap-13);`
- 整格換級（備案）：`:root body [data-hz-gap="kchwd0"] { --hz-gap-14: var(--hz-gap-12); }` —— 會繼承給後代，只在整格換一級時用。
- **block 與 block 之間的距離不是外距**（上下外距釘死為 0，寫了會擋）→ 寫那一段的 `padding`。
- **內距永遠只改這一頁**：對面的 class 寫 `padding`，③ 展開成這頁每張卡各自的內距。
- **容器 gap 分三個詞**：`hz-flow-NN` = `gap`（兩個方向）；`hz-flow-row-NN` = `row-gap`、`hz-flow-col-NN` = `column-gap`。畫單軸 ③ 各寫一筆；但**只有新 block 有分軸 class**，舊 block 畫單軸會落殘留（是 block 的年紀問題）。⚠️ `column-gap` 是子元素**之間**，不是容器左右留白。
- **也可寫進 block 資料**：`block-set-geometry { gaps }`，gap_id 從 `block-inspect` 的 `gap_carriers` 拿（`guide/block/geometry-style`）。
- ⚠️⚠️ **沒有 `as: "pad"` 載體的 block，區段上下留白寫死、調不到**（它可能有好幾個 `margin` 載體，那些是元素間距）。例：`hero-simple-eyebrow-center-v1`；`block-set-geometry` 送 `as: "pad"` 會回 `section_padding_fixed`。照 §A-6 回報，⛔ 不用 surface spacing、custom HTML 或硬塞 margin 去繞（spacing 會疊加、越調越高）。
- ⚠️ 內距常在**內層容器**不在 section 自己 —— 逐層量 `paddingTop` 找到是誰再改。

### B-9 / B-10 動效 motion 與 Hover —— **直接寫標準 CSS**（2026-09-12 起）

想要什麼效果就寫：`:hover`、`transition`、`animation`、`@keyframes`、`@media` 都可以（狀態偽類寫在地址上、放最後，§A-2）。

```css
:root body [data-hz-surface="grp-01.sec-01.card"]:hover { transform: translateY(-4px); transition: transform 250ms ease-out; }
@keyframes hz-p54077-rise { from { opacity: 0; transform: translateY(24px) } to { opacity: 1; transform: none } }
:root body [data-hz-gap="kchwd0"].hz-inview [data-hz-text="sh.title"] { animation: hz-p54077-rise 500ms ease-out both; }
```

**③ 不收成 token** —— 搬進**這一頁的自訂樣式段**（永久留在頁上、跟著頁面複製，報告標「自訂保留」）。
兩條要記：**每個選擇器都要含這一頁的地址 / 門牌 / block 根**；**`@keyframes` 名字以 `hz-p<page_id>-` 開頭**。其餘範圍規則（全站選擇器、`@import`…）看 `guide/design/locate`「hover / 動效」段，違反會當場擋並指名。
要改「全站的動效預設」仍是改 `hz-motion-motion-NN` / `hz-hover-NN` 那族的字（值域見字典）。

### B-11 裝飾線 decor

- 地址 `[data-hz-decor="divider.tint"]`，顏色印在 inline style，地址選擇器蓋不過。**整批改 → 改上游色票**（`:root { --hz-color-20: … }`，會連帶動到共用那格色的東西，例如卡片邊框）；**只改一根 → `block-set-style { slot: "decor-01.tint", decor: "color.16" }`**。

### B-12 顯隱

單一槽：`block-set-slot-visibility`；repeater 整格：`block-set-repeater-visibility`（§B-7b、§B-7c）。

### B-13 v1 不教

- **草稿層碰不到、也沒有工具**：整頁內容寬度、容器寬度（字斷行見 A-5 第 7 條）。
- **畫得出來、③ 收不回**：`max-width` / `margin-inline` / `text-wrap` 這類排版屬性 —— 前台當下生效，③ 列殘留、留在草稿層。照常寫、回報列出來。
- **欄數（`grid-template-columns`）會被擋**（`changes_shape`）→ 先 `block-inspect` 看 `block_attributes` 有沒有欄數屬性（如 `listing_cols_desktop`），有就 `block-set-attribute`，沒有才回 ① 換 block。
- **不走草稿層、有工具**：背景圖 / 影片 → `block-set-surface`；面的內距、圖片比例焦點 → `block-set-geometry`；插座、影片、地圖 → 各自的 block 工具。
- **v1 真的不做**：元素靜止時的位移（translate）、縮放（scale）。hover 的 `-scale` / `-translate-y` 不在此限（§B-10）。
- 整站深色目前做不到閉環（第三方外掛容器會露白帶），回報就好。

### B-14 說 yes 之後會發生什麼

草稿是暫時的；③ 把它**翻成系統裡的設定**寫回、清掉草稿。③ 只問一件事：**你寫的值，字典裡有沒有一模一樣的字？**

| 你寫的 | ③ 怎麼判 | 問站主嗎 |
|---|---|---|
| `var(--hz-…)` 引用既有的字 | 換字 | 不問 |
| 原始值，**剛好等於**既有的字 | 換字（③ 自己對上） | 不問 |
| 原始值，字典沒有相等的字 | 新字義：改那個字 / 開新字 / 不寫 | **會問** |
| 寫在字義 class 上（例：改字族），站上**一格同身份的都沒有**可比 | 出題：開新字（建議）/ 改那個字 / 不寫；答了開新字才開一格、改綁過去（計畫書標 `reason: no_peer`）；自訂區滿了改問「已滿」題 | **會問** |
| 尺上的值沒有相等的級 | **吸附**到最近的級（數字可能被挪一點點） | 不問 |

- ⭐ 你不用擔心「該不該引用」—— ③ **看值**分類，正確性不靠你。
- ③ 從不偷偷改一個字的定義；計畫書上一堆問題，是那些值本來就是新的，不是你做錯。
- 對不到任何一條 → **殘留**（畫面上有、寫不回系統）。殘留清單是你的產出，回答「這個設計系統還缺哪些字」。
- 各類值的待遇（snap / exact / add）與開關 → `guide/design/writeback`。

### B-15 custom HTML —— 逃生口，不是工具箱

> 站主 2026-09-09 定調、2026-09-10 收窄。[01-seat-rules](../01-seat-rules/SKILL.md) 只放一句指過來，**規矩的主人在這一節**。

- **什麼情況可以用、誰能用 —— 先留白**（等看過實際用法再定）。⛔ 不要自己補一套規則。
- **它不吃 token**（顏色 / 字級是死值，③ 收不回）—— 已知的路，⛔ 不必每次提醒站主。顏色仍盡量寫 `var(--hz-color-NN)`。
- **用了就記一行**（硬要求，那一行就是日後轉成正式 block 的需求單），寫在回報裡：
  ```
  custom-html | <站> | <頁 id / slug> | <第幾段> | 為什麼沒有合適的 block（一句）
  ```
- ⛔ 不准 `100vw`（會多一條橫向捲軸），滿寬用 `width: 100%`；RWD 用 `@container`，不用 `@media`。
- **轉成本站私有 block**：轉換一律請站主到後台 Stylebook Convert 頁做（座位沒有轉換工具；轉換、存入 Stylebook、撤銷都在那一頁）；轉完怎麼用一般 block 工具改，見 `guide/block/demo-convert`。

---

## C. 寫入與自驗

### C-1 寫進站台

1. **畫稿用對位送**：先試跑、看對位表，沒問題再加 `apply: true`（叫法、對位表欄位、失敗碼 → `guide/design/locate`）：
   ```
   design-locate-style-layer { page_id: <PAGE_ID>, css: "<畫稿全文>" }
   design-locate-style-layer { page_id: <PAGE_ID>, css: "<畫稿全文>", apply: true }
   ```
   - ⚠️ **頁面要先公開**（`page-clone` 與 `page-batch` 建頁沒指定 `status` 時預設是草稿；要接著畫就帶 `status: "publish"`）。
   - ⚠️ **每次送的是整份**，會取代原本的草稿層；上一輪要留的一起帶上。
   - 清空草稿層：`design-update-style-layer { page_id, css: "" }`（③ 收完會自己清）。
   - ⛔ **草稿層只收對位過的規則**（2026-09-19 起由工具守）：畫稿原文直接交給 `design-update-style-layer` 會整份被拒收（`draft_not_located`，沒對位的那幾條列在 `details.unlocated_rules`）；用 `wp-rest` 直接寫這頁的 `meta._hz_style_layer` 也會被擋。一律走上面的 `design-locate-style-layer`。③ 收回時遇到沒對位的舊規則也不收（`reason_code: "not_located"`），同樣回去重新對位。
2. **試跑與送出成功的回應都附 `lint_summary`** —— 看它，`ok: true` 才算過，**不用再另外叫 lint**。有 error 照 `reason` 修；同一種 reason 修三次還在 → 停，原樣回報。要在送出**之前**檢查一段 CSS，才用 `design-lint-style-layer { page_id, css }`。
3. **每輪送出後不開瀏覽器**：看送出回應的 `lint_summary` + `design-get-canvas` 的 `draft` 回讀，對得上就算這輪過了（`@media` 的值看 `draft_by_device`；repeater 別格跟第 1 格不一樣時會另列一筆、`cells` 標是哪幾格）。仍要開瀏覽器量 `getComputedStyle` 的情況：
   - (c) lint 綠、canvas 也對，但站主說畫面沒變（→ §C-2）。
   - **交件前開一次瀏覽器總對帳**：桌機 + 手機寬度各量一次 —— 這輪改過的元素的 computed，以及斷行。寬度照字典的斷點（目前 `1023px` / `767px`）：桌機量 ≥ 1024（例 1440）、手機量 ≤ 767（例 390），有另外畫平板才加一個 768–1023 之間的。沒有現成的對帳腳本，逐條手動步驟見 `guide/design/verify`「交件前總對帳怎麼做」。

   外部座位開瀏覽器量，⛔ 不靠 `curl`。怎麼量、截圖前要停動效、哪些東西 `fullPage` 截不到 → `guide/design/verify`。截圖前停進場動效的寫法照抄 `guide/design/verify`「截圖」段，⛔ 不憑印象寫。

   ⚠️ **要在網址上掛自己的識別參數（認分頁用的那種），⛔ 名字不可以 `hz_` 開頭**（例 `?hz_debug=1`）—— WordPress 會把它當成在查某個自訂內容型別的 slug，查無資料 → **整站每一頁都回 404**。症狀是「前台看起來壞了」，最容易誤判成自己剛剛做壞了。用 `hzdevtab=` 這種不以 `hz_` 開頭的名字。
4. **截圖的時機**：
   - 站主要你**發揮美感**（「弄好看一點」）→ **動手前**截一張整頁看整體（跳色、斷行、孤兒卡這類量值看不出來）。
   - 站主給**具體數值**（「標題改 40px」）→ 不用截。
   - 截圖是你自己看的，給站主的是網址，截圖只在站主要求時給。
5. **要開一頁新的**：`page-batch` 的 `create_page`（帶 `return_structure: true` 連 `slot` 一起回來；`status` 要畫就給 `"publish"`）。只要一頁**空頁**（例如接著 `block-clone` 進去）→ `page-batch { create_page: { title, status: "publish" }, ops: [] }`。叫法 `guide/page`「批次寫入」。
6. **`block-clone` 之後，草稿層不跟著走**（回應會有 `source_has_page_styles` 警告）。要「樣子一樣」→ 把來源頁畫稿裡指到那顆 block 的規則挑出來，改成新頁的地址，在新頁重新對位送出一次；沒畫過的不用管。

### C-2 ② 怎麼驗收：站主說「不對勁」的時候先量什麼

**截圖看得出「醜」，看不出「21px 比 22px 小」。** 站主給的是症狀，直覺會跳到自己最會做的動作。這就是 §C-1 的 (c)：**開瀏覽器，先量再改、改完再量一次**，不靠回讀（量法 `guide/design/verify`）。

| 站主說 | ⛔ 直覺（多半錯） | ⭐ 先量這個 |
|---|---|---|
| 呆板 / 差一點點 | 調字級 | **斷行**：最後一行剩幾個字（例：標題斷成 19 + 2） |
| 分不清輕重 / 沒節奏 | 加粗、放大 | **列出這一區每個角色的字級**，看是不是全擠在幾 px 之內 |
| 擁擠 | 加區塊留白 | 🔴 **先分清是塊與塊之間，還是塊內字與字之間** |
| 看不到 / 整個很怪 | 換顏色 | **往下遞迴走每一層的非透明底色**（深底段裡可能藏一張近白卡） |
| 沒有區隔性 | 加留白 | **相鄰兩塊底色明度是不是一樣** |
| 要有份量感 | 放大字 | **這一段有沒有自己的色場** |
| 太平 / 沒質感 | 加陰影、加漸層 | **層次**：列出這一區眉標 / 標題 / 內文 / 按鈕各自的字級、字重、顏色，看是不是差不多（全擠在一起就是平） |

- 「小一號」= 字典 `font_size` 章（`size.text.NN`）比現在那一格小一格的值（例：2.5rem → 2.375rem）；⛔ 不自己定減幾 px。

- ⭐ **每放大一個字，當場檢查它上下鄰居的間距**（至少跟被放大的字級同量級：標題 32px 底下留 22px 就會擠）。
- **兩個層級**：塊與塊之間 = section padding（有的在 block 元素本身、有的在內層載體 → 往下找第一個 `paddingTop !== 0` 的 `[data-hz-gap]`）；塊內字與字之間 = 各文字元素自己的 `margin-top` 載體。
- **「工具說成功、畫面沒變」三種樣態**（對位放行、lint 綠、apply 成功都會發生）：`--_hz-fs` 那三個寫在非 heading 上；對位翻出父子相反的選擇器；背景走過 `block-set-surface` 後 `--hz-surface-NN-bg` 的修飾 class 消失了。⇒ **這時唯一可靠的是 `getComputedStyle` 逐項對帳。**
- **一顆 block 可能有兩層面**（外層 section 面 + 內層卡片）：換外層的面，裡面那層不會跟著走 —— 換深底時最致命（淺卡留在深底上），順便看它的內距。


### C-3 ③ 收回：請站主到後台 Stylebook Merge 頁做

> 規矩（誰決定）在 [01-seat-rules](../01-seat-rules/SKILL.md)；同一句說法也在 `guide/design/writeback`。**這一節只講你和站主怎麼交接。**

**③ 收回請站主到後台 Stylebook Merge 頁做** —— 出題、作答、寫入、撤銷寫入都在那一頁；**座位不呼叫任何收回工具。**

**交網址之前**：頁面已公開、草稿層還在（畫完別自己清）。

**每一頁的交接都一樣**：

1. 把**Stylebook Merge 頁網址**給站主（`<站台網域>/wp-admin/admin.php?page=hauzii-token-decision&page_id=<N>`；WordPress 裝在子目錄時請站主從後台 **YITA 選單 → Stylebook Merge** 進去）。
2. 跟站主說：「這一頁的樣式可以收回了，請到 Stylebook Merge 頁收回；題目在那頁答，寫入、撤銷寫入也都在那一頁。」
3. 站主收完之前，⛔ 不要再改這一頁的草稿層（題目會對不上）。收完後看一次前台。

**⛔ 不在對話裡逐題問，⛔ 不替站主選。** 選項的差別是「改全站」還是「只改這頁」，站主看得到的東西（別頁、之後還想不想共用）你看不到。

**站主問起那一頁的選項時**，照這三個去向講（⛔ 不唸 `overwrite` / `merge:` 這些代號）：

- **寫進 Stylebook**：全站共用 —— 併進現有那一格，或開一格新的、寫目前畫面上的值
- **只有這頁**：留在這一頁自己的自訂段，別頁不受影響
- **不要了**：這一條拿掉，畫面回到原樣

> ⭐⭐ **「不要了」會把一組設計拆一半**：hero 的深色漸層底寫進去了，三個淺色文字卻選「不要了」⇒ 深底深字。拿不定就選「只有這頁」。

收完之後草稿層會清空；沒收成 token 的規則原文搬進這頁的自訂段（標「③ 搬來」），前台長相不變。

## D. 回報

0a. 呼叫記帳：到站主第一次看到畫面共幾趟；哪幾趟是補呼叫、為什麼
0b. **③ 交接了沒**：Stylebook Merge 頁網址給了站主沒（③ 由站主在那一頁做，座位不呼叫收回工具）
1. 改了哪幾條、各是為了什麼。⭐ **每一輪送之前先兩三句講「這輪要改哪幾條」＋ 貼草稿層全文原樣**；做完再貼最終版。量測表放附錄。
2. `lint_summary`（最後一次送出的）＋ 交件前總對帳的量值（桌機 / 手機寬度）
3. 想做但表上沒有、字典沒字的（最重要的一項）
4. 哪顆 block 光改長相救不了（結構問題，含斷行停手的）
5. 手冊哪裡看不懂、哪裡靠猜
6. **動效與 hover 的歸屬**：哪些是模板**自己帶的**、哪些是**你設的**（你的草稿層裡有那一條才是你的）。
7. 用了 custom HTML 的那一行（§B-15）

---

## 附錄

- 字典用 `design-get-dictionary`（[02-seat-setup](../02-seat-setup/SKILL.md) §3）；③ 寫回之後一定要重拿一次。
- ⚠️ **builder 部署之後，session 要整個重開才看得到新工具 / 新參數**（`/clear` 不會重載工具定義）。「playbook 說有這個參數，我的工具沒有」多半是 session 太舊。
- **同一頁有 2 個以上寫入就用 `page-batch`**（op 名就是工具名、參數一樣；`guide/page`「批次寫入」）。
