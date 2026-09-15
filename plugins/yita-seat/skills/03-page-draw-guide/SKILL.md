---
name: 03-page-draw-guide
description: "畫的時候翻：一頁草稿頁上每種元件能改什麼、畫稿怎麼寫才收得回、怎麼自驗、③ 收回怎麼問站主。窮舉查表，不必背；工具怎麼叫、失敗碼看 playbook。"
---

# 畫頁手冊 — 畫稿怎麼寫才收得回

> ⭐ **先讀 [01-seat-rules](../01-seat-rules/SKILL.md)**（要牢記的那一頁）:② 盡量發揮、③ 才收斂;
> 你唯一要守的是「寫在草稿層、值寫清楚」。**本頁任何一句與它衝突，以它為準**，並回報那一句。
> **這本講「畫稿怎麼寫」；工具怎麼叫、參數、失敗碼一律 `playbook-get`**（本頁只指路，不重抄）。
>
> 你是「座位」。你先用既有 block 把一頁排出來（①），再把它畫成想要的樣子（②）。你畫的**每一筆都是 hz 標籤**：內容填進 block 的欄位，長相寫成 token 變數。畫面會即時跟著變；畫對了，長相那部分最後會寫回全站的 design token（③，**你跑工具、站主答題**，見 §C-3）。**先照 §0 的四段走，每一段走完停下來給站主看；聽不準就先問。**
>
> 這份是**窮舉**：每種元件能改什麼、怎麼寫，全在這裡。表上有的照寫；表上沒有的，照 §A-6 記下來——那是「該補的字」，不是你的錯。

---

## 0. 流程（先照這個走，每一段走完都停下來給站主看）

四段。每一段走完，給站主前台網址、等他說話，沒說「繼續」不往下走。

⭐ **一條原則：聽不準就先問，⛔ 不要猜了再畫。** 常見聽不準的話：「參考某頁」（要的是**同一個形狀**，還是**同樣的完成度**？）／
「擁擠」（是**塊與塊之間**、**字與字之間**，還是**卡片的內距**？）／「那個標題」（畫面上有兩個候選 —— 列出來讓站主指）。
問一句十秒，猜錯白畫一版。⚠️ 這三個只是**提醒**，不是必問清單；聽得準就直接做。

| 段 | 你做什麼 | 用什麼 | 不做什麼 | 給站主看什麼 |
|---|---|---|---|---|
| **0 骨架** | 聊需求時**先問一題**：「這頁沿用站上的字典，還是這頁全部新開？」（見 §A-1，沒說就是沿用）→ 再列「段落清單」：每段一句用途 + 候選 block 1–2 顆（附一句理由） | `block-browse`（六軸篩到 ≤10 顆） | 不插、不建頁 | 清單 + 這輪是**沿用**還是**新開**。站主點頭才開始 |
| **① 形狀** | 建頁、插 block、調 repeater 格數、藏多餘格、版面屬性、錨點；**站主已經給的文字**一趟 batch 填進去 | `page-batch`（add-section / fill-page-text / set-item-visibility / set-block-anchor）、`block-set-attribute` | **不寫新文案、不打磨字**；沒給文字的槽留示範文字（示範文字在這一段**不是問題**）；沒給圖的槽同樣留模板示範圖（形狀段不是問題，內容段再換）；不寫草稿層、不碰背景 | 前台網址 + 「哪幾顆請你看形狀」。站主逐顆說**留 / 換**；換的就移除重插，文字跟著搬 |
| **② 感覺** | 寫草稿層（色、字、留白、按鈕、動效、**漸層與純色底**）、背景**圖 / 影片**走 `block-set-surface`；每次寫完跑 lint、自己量 computed 驗 | §A–§C | 不改 HTML、不 `!important`、不碰 token | 網址 + lint summary（截圖只在站主要求時給）。站主逐顆說**好 / 不好**。⚠️ **② 是畫畫，不是選版型**：座位**自己**不要為了畫出某個效果去翻別的 block 來套 —— 畫不出來就回報。但 ① 只是第一版，站主看了畫面**隨時會說換 / 調 block**，那是正常的：站主說換就換，換完那顆重畫它（站主 2026-09-09 定調）。⚠️ **動到 header / footer 的字要在回報裡單獨列一段「H/F 改了什麼」** —— 那是全站共用，不是這一頁的事（見 §B-1） |
| **內容** | 補齊剩下的文案、圖、連結；跑 `page-audit` 到 PASS | `page-batch` 一次送、`block-set-image`、`block-set-button` | 不再動形狀與草稿層（要動就回到對應那一段說一聲） | 完整頁。站主看完說 **yes** |
| **③ 收回** | 站主說 yes 之後**你跑**：`design-writeback-plan` 出題 → 白話問站主 → 帶**全部**答案重產，直到可以執行 → `design-writeback-apply`（問法見 §C-3）。跑前在對話裡告知站主、同站一次一頁 | `design-writeback-plan` / `design-writeback-apply` | ⛔ 題目**不准自己選**（那是「改全站」還是「只改這頁」的差別，你看不到別頁）；⛔ 不自己用 `design-update-token` 照計畫書一筆一筆打 | 站主逐題回答；收完回報 outcome 與前台網址 |

- ⚠️ **讀這份手冊與字典用 `Read` 工具，⛔ 不要 `cat`** —— 手冊 40KB 出頭，超過 Bash 輸出上限會被截斷，你會以為讀完了其實少一截，然後分好幾趟重讀。`Read` 一次整份進得來。
- **文字的規則一句話**：有給的就填，沒給的不編。填字很便宜（一趟 batch），貴的是寫文案和改文案 —— 所以形狀沒定之前不寫新字。
- **① 排頁前先挑 block：用 `block-browse`**（類別 / 用途 / 版型 / 圖 / 背景 / 項目六軸交集，篩到 ≤10 顆才列清單；叫法 `playbook-get name="guide/block/browse"`）。⛔ 不憑印象挑、也不整本讀字典。要看某顆的實際槽位去 `block-get-template`。
- **選 block 優先挑有門牌的**（最外層 `<section>` 帶 `data-hz-gap`，用 `block-get-template` 看得到）。沒門牌的是舊 block，只能整頁一起改、不能單顆定位。撞到舊 block 視為「換 block」訊號。
- **每一段的回報固定三行**：做了什麼 / 前台網址 / 請你看哪幾顆。細節放最後 §D。
- **順手記帳**：從開工到站主第一次看到畫面，你呼叫了幾趟；哪幾趟是補呼叫（工具回的東西不夠、得再叫一次）。這是我們修流程的依據。

---

## A. 原則（先讀這一頁）

### A-0 精神（四句話，記住這四句其他都好辦）


1. **① 是名詞、② 是形容詞。** ① 決定「這頁有什麼」（一段一段是什麼東西）；② 決定「它們長什麼樣」。
   分不清自己在做哪一件事的時候，問自己這一句。
2. **先量，再改；改完，再量。** ⛔ 不要憑截圖或印象判斷現在是什麼值、也不要憑「我寫了」判斷改到了 ——
   兩頭都用 `getComputedStyle` 量（怎麼量見 §C-2）。
3. **字典是查「現在是什麼」，不是菜單。** 要什麼值直接寫，不用去挑最接近的一格 —— 收斂是 ③ 的事。
4. **限制不是「不給你做」，是「③ 收不回」。** 每一條 ⛔ 背後都有一個「寫了會掉」的理由；
   撞到限制就回報，那是系統的缺口，不是你做錯（站主 2026-09-07 定調 A117）。

### A-1 一頁草稿頁 = 你的畫布，兩個通道

| 你要改的是 | 寫在哪 | 動到全站嗎 | 最後怎麼處理 |
|---|---|---|---|
| **這顆 block 自己的資料**：文字、連結、圖、圖示、影片、**背景圖／背景影片**、顯示幾格、比例裁切、間距級 | 這頁 block 的欄位（用 `block-*` 工具） | 不會 | 本來就在家，不用寫回 |
| **token 的值與綁定**：顏色、字級字重字距行高、圓角、陰影、框線、按鈕外觀、留白級數、動效速度、hover | 這頁的**樣式草稿層**（一段 CSS，只放 token 變數） | 不會 | ③ 收回時寫回 token 或綁定 |

判準一句話：**元素身上有 inline `style="…"` 的東西 = block 自己的資料 → 工具；其他 = 草稿層。**

⭐ **② 的主要動作是「寫一段 CSS、送一次」** —— `block-*` 工具只在改這顆 block 自己的資料（上表第一列）時用。
⛔ 如果你發現自己在用一長串 MCP 呼叫做「幾行 CSS 就好」的事，**停下來回報** —— 那通常表示你把長相當成內容在設（A111）。

> ⭐ **只有背景圖 / 影片要走 `block-set-surface`**（A145 站主 2026-09-07 放開純色；2026-09-08 再放開漸層）。
> **純色底色直接在草稿層寫 hex**（`--hz-surface-NN-bg: #F6F1E8`），③ 會替你開色票（A91）。
> **漸層也直接畫**（寫在 `[data-hz-surface="…"]` 上，見 §B-1），③ 收回時才翻成 block 的背景資料。

> **⭐ 站主 2026-09-07 定調（A117）**：**除了背景圖 / 影片與內容以外，你看得到的東西一律用普通 CSS 直接寫。**
> 寫完 ③ 收不回來，那是**系統的缺口，不算你的錯** —— 回報一聲就好，不用為了「怕收不回」而繞路或不寫。

**開場先問一題：沿用還是新開？** 聊專案時問站主／客戶一句：「這頁**沿用**站上現有的字典（Design Token），還是這頁**全部新開**一套？」

| 答案 | 意思 | 你畫的時候差在哪 |
|---|---|---|
| **沿用**（預設，沒說就是這個） | 這頁跟全站同一套字。改一個字，全站跟著變 | **一樣先寫你要的值**（A145）；你明知就是要某個既有的字時，引用 `var(--hz-…)` 也行 |
| **新開** | 這頁要一套自己的長相，不動別頁 | 一樣照 §A–§C 畫，但心裡有數：③ 會把這輪的值收成**這頁自己的一套**，不是改全站 |

- **在排 block（第一步）之前問，最晚第二步開始前要有答案。** 問完把答案寫進 §D 回報的 `mode` 欄位（`沿用` / `新開`）—— ③ 會照它印「這輪模式」並預設建議。
- 兩種模式下**你的畫法完全一樣**（一樣寫草稿層、一樣照 §A-3 從字典選值）。差別在 ③ 收回時怎麼歸帳 —— 那一步你跑（`mode` 就是這個答案，見 §C-3），但每一題都要問過站主。
- 問這題的理由：模式有哪幾種，久了連站主自己都會忘；**規則說要問，就不會漏**。

**scope 你不用想（站主 2026-09-06 定調「②畫、③分類」）。** 你只管好不好看。寫長相時**直接寫你要的值**（A145，站主 2026-09-07）—— 不必先去字典找有沒有現成的字；你明知就是要某個既有的字時引用 `var(--hz-…)` 也行，但**不是必要**。哪些改動影響全站、哪些只這頁、要不要開新字，全由 ③ 看值分類後拿去問站主，不是你的事。

### A-2 選擇器：你怎麼寫都行，對位會翻（2026-09-07 起）

**你不用學選擇器形狀。** 寫你熟悉的 CSS 指著畫面上看得到的東西就好：

```css
.hz-typo-typo-06                          { letter-spacing: 0.03em; }
section:nth-of-type(2) .hz-surface-surface-05 { background-color: #EDECDD; }
article:nth-of-type(1) span.hz-typo-typo-06   { color: #8C7A52; }
section:nth-of-type(5) .hz-spacing:not(.hz-surface-surface-05) { border-left: 1px solid #DCDBBD; }
```

送出時跑一次**對位**（§C-1），它會把你的選擇器翻成系統認得的形狀，
並給你一張**對位表**：「你寫的 X → 我翻成 Y、命中 N 個元素」。

- **命中幾顆、會不會一次動到好幾顆 block，對位表會講**，你看表決定要不要再限縮。

⚠️⚠️ **對位表回的是「我命中了誰」，不是「這一類都改到了」**（2026-09-09 各撞一次）。
它只對你寫的那一條負責 —— 命中 3 個不代表這頁只有 3 個同類元素。**要改一整類之前，先自己掃一次全頁**：

```js
// 在瀏覽器 console / evaluate_script 裡跑，數量對不上就代表你的選擇器漏了
[...document.querySelectorAll('.hz-typo-typo-06')].length
```

- **末端沒有地址就整條退到祖先** —— 對位翻不到落點時會往上找有 `data-hz-*` 的祖先，
  於是「我只想改這一句」變成「整塊都改」。⛔ **不要只看到「命中 N 個」就放行**，要看它翻成的 `to` 是誰。
- **後代選擇器碰上 repeater 是乘法**：`[data-hz-...] .card .title` 在一個 12 格的卡片牆上
  = **格數 × 槽數**。表上的數字會突然變很大，那多半不是 bug，是你指到模板了。
- **屬性選擇器只支援 `=`**（完全相等）—— `^=` / `$=` / `*=` 會被擋。
- **一條規則一個選擇器** —— 逗號並列（`a, b { … }`）**不合法**，拆成兩條寫（字典「五句話」第 5 句同此）。
- **`[data-a2ui-component="X"]` 是限縮到「某一顆 block」的主力寫法**：要只改某一段而不動到別段長得一樣的東西，
  拿它當前綴比 `:nth-of-type` 穩（`:nth-of-type` 會隨著 ① 插拔 block 而位移）。
- **翻不動的它會指名**（第幾行、你寫的原文、為什麼），改那一條再跑一次。**有一條對不到就整份不送。**
  失敗碼與警示碼各是什麼意思、該怎麼改 → `playbook-get name="guide/design/locate"`（那張表是唯一一份，這裡不重複）。
- 目前翻不動的只有**狀態偽類**（`:hover` / `:focus` / `:active`）—— 那是系統還沒有的表達方式，回報一聲即可。
- **整頁的字寫不進系統**：寫在 `body` / `html` / `*` / `:root` 上的裸屬性（`font-size` / `color` / `background-color`…）系統沒有「全站預設」這一層可收，對位會擋 —— 寫在具體元素上（`p`、`h1`、`.card`），效果一樣、③ 收得回。
- 結構偽類都可以用：`:first-child` / `:last-child` / `:only-child` / `:first-of-type` / `:last-of-type` /
  `:only-of-type` / `:nth-child(N)` / `:nth-of-type(N)` / `:nth-last-child(N)` / `:nth-last-of-type(N)`
  （**N 只收整數**，不收 `2n+1`）/ `:not(選擇器)`。
- ⚠️ **`:nth-of-type` 是相對「它的父層」算的**，不是相對整頁 —— 巢狀在別的容器裡的 `<section>` 也會是
  它自己父層的第一個。不確定就多加一層限縮（例如 `main > section:nth-of-type(2)`）；
  **對位表會告訴你實際命中誰**（命中幾個、翻成哪個位置），看表確認比自己數可靠。

⛔ **不准 `!important`** —— 但你不用因此繞路。有些屬性被 block 自己的資料**釘住**（寫成元素上的
inline `style="…"`，例如已調過級的間距），普通 CSS 蓋不過它。**這件事機器會替你處理**（2026-09-08 起）：

- 對位會**逐條判**「這條會不會被更強的東西蓋掉」。會被蓋、而 ③ 接得住的 → **它自己補強**，放行，你不知情。
- 補強不了、③ 也接不住的 → **當場擋下，並指名理由 + 該走哪支工具**（`block-set-*`）。
  不會讓你畫得出來、卻在 ③ 悄悄掉了。

補強長這樣（對位輸出的原文，2026-09-09 實跑）—— **這是機器寫的，不是你寫的**：

```css
:root body [data-hz-surface="section.bg"] {
  background: linear-gradient(135deg, #F6F1E8, #DCDBBD) !important /* hz:pinned */;
}
```

所以你唯一要做的是**照常寫普通 CSS**。⛔ 畫稿裡只要出現 `!important` 就整份不送（失敗碼 `important`）——
那個 `/* hz:pinned */` 標記只有機器產得出來，偷渡不進去。
⚠️ **寫了沒反應、確定不是快取**：先看對位表怎麼說。它既沒擋你、也沒說補強，那才是異常，回報一聲。
⛔ 不改 HTML。


### A-3 值從字典選

**token 字典用 `design-get-dictionary` 拿**（見 [02-seat-setup](../02-seat-setup/SKILL.md) §3）；挑 block 用 `block-browse`（§0 那條）。

token 字典：這個站每個字（`color.07`、`typo.03`、`button.01`…）的意思、變數名、現值。

⭐ **寫你要的值**（hex／帶單位的長度）—— 看畫面決定，不用先挑字典裡最接近的一格（A145，站主 2026-09-07）。
**字典是查「這塊現在綁哪個字、現值多少」用的，不是給你挑的菜單**；要不要變成既有的字，是 ③ 分類、站主決定的事。
引用 `var(--hz-…)` 仍然合法（你明知就是要那個字時很好用），但**不是必要**。

值域參考：字距九檔（−.05 / −.025 / 0 / .025 / .05 / .1 / .15 / .2 / .3em）、字重 100–900 整百、行高 1.1–1.8
—— **③ 會把你寫的值吸附到這些檔位，你不用先算**。

⚠️ **字典的 name / label 跟實際的值可能對不上，以「值」為準**（2026-09-09）。
那些名字是當初取的，token 後來被改過、名字不一定跟著改 —— 例如叫「Accent 強調色」的那格
現在可能是一個很淡的米色。**要判斷「這格現在是什麼」只看 value 欄**，⛔ 別靠名字推。

### A-4 怎麼認出畫面上的哪一句（寫選擇器時用）

看這頁前台 HTML：用你手上能抓網頁的工具（瀏覽器 / 網頁抓取），網址每次加 `?x=<隨機數>` 避開整頁快取。

你要找的是**「怎麼指到它」**，不是它的地址 —— 元素身上的 class 就夠用了：

- `hz-typo-typo-03` = 它現在用 typo.03 這個字；`hz-btn-button-01` = button.01；
  `hz-text-color-07` = color.07；`hz-surface-surface-05` = surface.05。
- **看標籤 + typo class 就認得出這句是什麼**：`h1` + `typo-01` = 主標、`h2` + `typo-03` = 區段標題／品牌名、
  `p` + `typo-05` = 副標、`typo-06` = 內文、`typo-11` = 上標。

```html
<h2 class="… hz-typo-typo-03 …" data-hz-text="text-01.heading">大隱塾光</h2>
<p  class="… hz-typo-typo-05 …" data-hz-text="text-02.body">在城市裡，替自己留一段安靜</p>
```

→ 要改「大隱塾光」那一句：`section:nth-of-type(1) h2.hz-typo-typo-03 { … }`（指得到就行）。
→ 要讓**全站所有區段標題**一起變：`.hz-typo-typo-03 { … }`（整條就是那個字的 class = 改那個字的意思）。

⚠️ **一條規則會動到幾顆，看對位表**，不用自己數 —— 它會說「命中 N 個元素」「這個門牌涵蓋 N 顆 block」。
覺得動太多就把選擇器寫窄一點（加 `section:nth-of-type(N)`、加標籤、加 `:not(...)`），再跑一次。

⚠️ **`block-set-map` 設了真實地址之後，這顆的結構會變**：模板預設是裸 `<iframe data-hz-map>`，
設定後地址跑到 `div.hz-map-focus-container` ＋ `div.hz-map-click-layer` **兩個 wrapper** 上、
iframe 不再帶 `data-hz-map` —— 同一個地址命中數 **1 → 2** 且命中的是 div。**那不是寫壞了。**

> `data-hz-*` 是什麼、門牌是什麼、兩層寫法怎麼運作 —— **你不用知道**。


### A-5 六個坑

1. ~~h1–h6 的字級要寫 `--_hz-fs`~~ —— **2026-09-07 起不用了**：直接寫 `font-size` / `font-weight` / `line-height`，對位會替 heading 補上那三個內部變數（它們是被鎖住的字級的唯一入口）。
2. ⭐ **改了桌機字級，就一定要交代平板 / 手機** —— 2026-09-11 起**不寫會被擋下、送不出去**（站主定調）。
   原因：你只在桌機視角畫，而那個字多半本來就有三裝置階梯（例如 36 / 30 / 28）。只寫桌機 =
   桌機變大、平板手機留在舊值，**比例跑掉而你在畫面上看不到**。二選一（⛔ 不能都不寫）：

   - **① 自己寫**：同一條再包一份在 `@media (max-width: 48.875em)`（平板）／`(max-width: 37.5em)`（手機）。
     **明示值優先** —— 你寫了就照你的。
   - **② 交給機器**：畫稿**最前面加一行** `/* hz-ladder: follow */`（整份畫稿的字級改動都跟階梯），
     或在某一條規則裡寫 `--hz-ladder: follow;`（只管那一條，優先於檔頭）。③ 收回時會照**這格原本的
     桌機:平板:手機比例**推出平板 / 手機，再吸附到最近的尺級。

   ⚠️ **斷點只能寫這兩個寬度**（`48.875em` / `37.5em`，或等值的 `782px` / `600px`）。寫 `1023px`
   這種「專案 3-tier」的數字會被擋 —— plugin 的字級只在那兩個邊界換值，寫別的寬度會有一段
   （783–1023）偷偷吃到桌機值，而 plan / apply / verify **全程綠**。
   ⚠️ 被擋時錯誤訊息會指名是哪一格、現在三裝置各是多少。⛔ 不要為了過關隨便補一個 `@media` 值 ——
   **回報可以收之前，自己把瀏覽器拉窄到手機寬度看一眼**，那才是你要交出去的東西。
3. **改完看不到變化：先懷疑快取（換 `?x=`），再懷疑寫法。** 不要看到沒變就下結論「這個鏈是死的」。
4. **`--hz-gap-NN` 寫在某個地址上會繼承給所有後代**。先看子孫有沒有用同一級。
5. **「對位放行 + lint 全綠 + 畫面零反應」是會發生的**（2026-09-09）。三個綠燈都只證明
   「這條規則送進去了」，**不證明它贏過別人**。唯一驗得出來的是**量 computed**（§C-2）——
   量到沒變，多半是那個屬性被 block 自己的 inline style 釘住；⛔ 你不准寫 `!important`，
   **該補強的機器會自己補**（§A-2），補不了的它會當場擋並指名該走哪支工具。
6. **③ 收回之後，這頁的 typo class 會全面換號**（2026-09-09）：③ 可能替你開新的字
   （`typo.36`…），元素身上的 `hz-typo-typo-NN` 就跟著換 —— **上一輪抄下來的 class 全部作廢**。
   下一輪開工**一定要重抓前台**（抓法見 §A-4 第一段，記得網址加 `?x=<隨機數>`）；要跨輪穩定地指同一個東西，用**地址**（`[data-hz-text="…"]`）
   而不是 typo class。

### A-6 表上沒有的東西

想做但這份手冊沒列、字典沒字：**用有地址的原始值寫下來**（`:root body [data-hz-…] { 屬性: 值 }`），lint 放行並標記。那是「該補字」的紀錄，回報時列出來。沒有地址可掛的（例如整頁內容寬度）→ 回報說明，不要硬寫。

---

## B. 每種元件能怎麼畫

格式：**地址** ｜ **內容**（block 工具）｜ **長相**（草稿層變數）｜ **常見目標** ｜ **不能／不教**

> ⚠️ **這一節的 CSS 範例寫的是「機器那一側的形狀」**（帶 `data-hz-*` 的那種）。
> 2026-09-07 起你**不必照抄那個寫法** —— 照 §A-2，用你熟悉的選擇器指到那個元素就行，對位會翻。
> 這裡的地址欄仍然有用：它告訴你**這個元件有哪些槽、哪些是內容（走 block 工具）哪些是長相（走畫稿）**。

### B-1 面 surface（section 底、卡片底）

- **邊框可分邊、圓角可分角（2026-09-12 起）**：想改某一邊 / 某一角就寫**純屬性**（`border-left: 3px solid #hex`、`border-top-left-radius: 0`、`border-style: dashed`），③ 會拆成 surface 的分邊 / 分角欄位；⛔ 不要寫 `--hz-surface-NN-border-left-width` 這種變數 —— 沒設過分值的格上那條 longhand 根本不在規則裡，寫了零反應。留空的邊 / 角自動跟主值。

- **地址** `[data-hz-surface="hero.bg"]`（section 級多半叫 `section.bg`，三顆 block 可能共用 → 用門牌）
- **內容**（`block-set-surface`）：背景圖 `media.desktop { type:"image", image_id|url }`、影片、去掉背景 `none`；響應式 `media: { desktop, tablet?, mobile? }`。詳 `playbook-get name="guide/block/surface-modes"`。
  （**純色與漸層不在這裡** —— 直接在草稿層畫，見下面「長相」與「常見目標」。）
- **長相**（變數，`surface.NN` 看元素 class `hz-surface-surface-NN`）：`--hz-surface-NN-bg` / `-border` / `-border-width` / `-radius` / `-shadow` / `-overlay` / `-overlay-alpha`
  - 整頁：`:root { --hz-surface-09-bg: var(--hz-color-02); }`
  - 只這個面：`:root body [data-hz-surface="hero.bg"] { --hz-surface-09-bg: var(--hz-color-02); }`
- ⛔ **整頁最底層（body 底色）動不了 —— 它是寫死的純白**（2026-09-10 站主裁決，取代 2026-09-06 的「接線到色票 37」）。就是區塊之間、頁尾下方會露出來的那一層。站主定調它是**基準線不是設計值**：不跟布景主題走、也不跟色票走，所以 `ColorDomain` 永遠吐 `html body { background-color: #ffffff }`。⚠️ **寫 `:root { --hz-color-37: … }` 不會有任何效果**（那格色票已經跟 body 脫鉤），別再用它換整頁底。要整頁鋪深 → **把每個 section 自己的面鋪滿**（上面那條 surface 的寫法），露出來的白帶就是這條規則，交由畫面上的面蓋掉。
- ⚠️ **面的底色多半是接到色票的**（例如 `--hz-surface-09-bg: var(--hz-color-01)`）—— 所以改一個色票，**掛它的那些面會一起變**。不想連動就寫那個面自己的地址（見下面「同一個 surface 編號」那條）。
- **常見目標**：換底色 → **直接寫你要的 hex**（`--hz-surface-NN-bg: #F6F1E8`），③ 會替你開色票（A91、A145）；引用既有色票 `var(--hz-color-NN)` 也可以，但**不是必要**；加圓角陰影 → `-radius` / `-shadow: var(--hz-shadow-NN)`；壓深一點 → `-overlay-alpha`
- ⚠️⚠️ **底色只要動了色相或明度，前景就要全部重量一次**（2026-09-09）：
  文字色、次要文字、框線、圖示、按鈕、hover —— 它們原本是配著舊底色挑的，底色一換對比就變了。
  ⛔ 不要只改底色就交。**而且要往下遞迴**：一個面裡面常常還有第二層、第三層面（卡片裡的標籤底、
  引言塊），每一層都要看它自己的**非透明底色**與**內距**還合不合理。
- ⚠️ **同一個 surface 編號同時當 hero 底與內容底時，⛔ 只能寫地址**（2026-09-09）：
  `hz-surface-surface-05` 這種 class 寫下去是**整頁**掛它的都改 —— hero 想要深、內容區想要淺，
  寫 class 一定會互相打架。這種情況一律寫
  `:root body [data-hz-surface="hero.bg"] { … }` 各寫各的。分不清有幾個地方在用同一格：
  `[...document.querySelectorAll('.hz-surface-surface-05')]` 數一次。
- ⚠️ **換 preset 會清掉背景媒體**（`block-set-surface` 換 preset 時 `media_reset`）→ **先換 preset，再設背景圖**，順序反了圖會被清掉。
- ⚠️ **背景走過 `block-set-surface` 之後，`--hz-surface-NN-bg` / `-overlay` 這兩個修飾 class 會從元素上消失** —— 之後你在草稿層改這兩個變數**不會有任何反應**（不是寫錯，是那個元素已經改由 surface 的背景資料接管）。要換回可用草稿層調的狀態，就把背景設回 `none` / 純色 preset。這一條很難自己查出來，改了沒反應先想到它。
- **背景圖 / 背景影片**：走 `block-set-surface`，**不寫在草稿層**。它是這顆 block 自己的資料（照片是內容，跟前景圖同一族），一寫就在家、不用寫回；原本設什麼都會被你的新值取代。去掉背景 `none`。詳 `playbook-get name="guide/block/surface-modes"`。
  ⚠️ 在草稿層寫 `background-image: url(...)`（或 `background` 裡帶 `url(`）會被擋：失敗碼 `background_media`，整份不送。
- **漸層：直接畫**（2026-09-08 起，站主重啟）。跟顏色一樣寫普通 CSS，③ 收回時才替你翻成這顆 block 的背景資料（`block-set-surface { gradient }`）。色用 `var(--hz-color-NN)` 或 hex 都可以。
  ```css
  :root body [data-hz-surface="hero.bg"] { background: linear-gradient(135deg, #F6F1E8, #DCDBBD); }
  ```
  ⚠️ **漸層要落得到一個面** —— ③ 寫回要一個槽的地址。**面的地址**（`[data-hz-surface="…"]`）或
  **面的 class 寫成 `:root body .hz-surface-surface-NN`**（帶前綴 = 限縮到這頁的元素，對位會把它翻成那些面各自的地址）**都可以**，
  所以「把同一類面一次全換成漸層」是合法的寫法。⚠️ **裸寫 `.hz-surface-surface-NN`（沒有 `:root body`）不行** —— 那是「改那個字的定義」、全站共用、沒有落點 → `gradient_no_slot`（2026-09-09 走一遍實測）。
  指到 `:root`（整頁）、別種 token-class（`.hz-typo-*` 這種「字」）、或不是 surface 的槽
  （`[data-hz-text="…"]`）→ `gradient_no_slot`，訊息會告訴你原本指到的是什麼形狀。
  ⚠️ 太複雜、③ 翻不回 block 資料的漸層會被 `gradient_unsupported` 擋（訊息會說是哪裡看不懂）。兩種都是當場擋、當場指路，不會在 ③ 才掉。
- **不能**：面的內距（spacing atom）v1 不教

- **header / footer 一起換色是預期的，但它是「全站共用」**：H/F 用的是專屬色格 `color.22–36`（字典 label 會標），在 `:root` 改它們就讓頁首頁尾與這頁同調。站主 2026-09-06 定調這不算越界。
  ⚠️ **但只有站主明說「H/F 也要改」時才動它**。你在這一頁改 H/F，只有這一頁看得到；③ 寫回的卻是站級 token，**全站每一頁的頁首頁尾都會跟著變**。所以 ③ 預設**不寫** H/F（站主同意才在 plan 帶 `hf: "include"`），而你若動了，**回報時要單獨列一段「H/F 改了什麼」**，讓站主決定要不要。
  字典 entry 有 `zone_hf: true` 的就是這類，共 66 個：color 15 格、字級 role 9 格、面 preset 7 格，加上 H/F 尺寸 `hf.*` 35 格。
  ⚠️ **寫 H/F 尺寸時，一條沒有 `@media` 的 `:root` = 三個裝置都改**（前台就是這樣，③ 也照這個語意寫回）。只想改某一個裝置就明示寫 `@media` —— 但 **hf 的斷點與 typography 的不一樣**（一個是 px、一個是 em），⛔ 別憑印象抄，查字典 `domains['hf-sizing'].breakpoints`（typography 的在 `domains.typography.breakpoints`，兩處都是從站上 CSS 量出來的）。
- **H/F 的尺寸也改得動**：高度 / 內距 / 間距走 `:root { --hz-hf-{段}-{項}: <長度> }`（如 `--hz-hf-main-height: 88px`、`--hz-hf-footer-mid-padding-y: 40px`），35 個全在字典 `hf-sizing` 段，逐裝置值寫在 `@media` 內。⚠️ **不是每格都有平板 / 手機欄位**（`nav.*` 沒手機、`drawer.*` 只有單值），沒有的格 ③ 會列 residual 不硬寫，不用擔心寫壞。⛔ **不要寫 `--hz-header-*` / `--hz-footer-*`**（那是同一件事的下游，印在 `</head>` 之後，寫了沒用）；**版面比例**（`left-w`、`justify`）草稿層碰不到，想動就回報。

### B-2 文字（一般文字）

- **地址** `[data-hz-text="hero.desc"]`
- **內容**（`block-update-content`）：`{ text }`、加連結 `{ text, url }`、撤連結 `{ url: "" }`。key 用 `page-get-structure` 的 `semantic_key`；**欄位還是空的時候拿不到 semantic_key**（只回 `empty_field_keys`），這時先送一次，看錯誤回應的 `accepted_bare_keys`（例 `01_heading`）照著填——不要用 `text-01.heading` 這種點記法。
- **長相**：
  - 顏色：`--hz-color-NN`（整頁 `:root`；只這句：地址底下 `--hz-color-09: var(--hz-color-10)`）
  - 字級／字重／行高／字距：整頁 `:root body .hz-typo-typo-NN { font-size; font-weight; line-height; letter-spacing }`；只這句：地址底下同四個屬性（記得 A-5-2 的 `@media`）
  - 動效 `animation-duration`、hover `--hz-hover-NN-*`（見 B-9、B-10）
- **常見目標**：內文柔和 → `:root { --hz-color-09: … }`；內文呼吸 → `.hz-typo-typo-06 { line-height: 1.8 }`；小字寬字距 → `.hz-typo-typo-08 { letter-spacing: .1em }`
- **只想改「某一類文字」的顏色**（例如全部小標）：`:root body .hz-typo-typo-11 { --hz-color-14: var(--hz-color-16) }`
  —— 範圍是**掛著 typo.11 的那些元素**，比逐個地址寫穩（不會漏）。
- ⚠️ **字級只能靠「換一個字」表達**，而換字的條件是「字級對得上 **且其餘欄位（字型／字重／行高／字距）與這句現在綁的字一樣**」。
  對不上時 ③ 會落殘留（`字級改不了`）而**不會**偷偷換成別的字 —— 因為那會把字型字重一起換掉。
  要嘛接受開一格新的字（站主在問卷上答「開新格」），要嘛改用「改字義」（`.hz-typo-typo-NN { --_hz-fs: … }`）。
- **顯隱**：`block-set-slot-visibility`
- **排錯：改 token 沒反應** → 看那個元素的 class 有沒有 `cv:…-[var(--hz-…)]!` 這種寫法。有的話**它才是真主人**（同一個元素上的 `hz-text-color-NN` 是死的，改它不會有事）。這時 ⛔ **不要去改那個 token 的全域值** —— 在**最近的門牌**上重新定義它就好：
  ```css
  :root body [data-hz-gap="門牌"] { --hz-color-20: var(--hz-color-14); }
  ```
  `var()` 是在**使用端**解析的，所以繞得過 `!important`；全域語意不動、別的 block 不受污染。值寫成 `var(…)` 而不是 hex，lint 才會記成 binding（③ 也才寫得回綁定）。
  （實測：四段眉標一起同色、全域 color.20 維持原值、其他 block 的邊框沒被波及。）
- **字型改得動，但只能挑站上已有的**（2026-09-06 起）：整頁 `:root body .hz-typo-typo-NN { font-family: var(--hz-font-<歐文>), var(--hz-font-<中文>); }`。可用字型看字典的「可用字型」段（那是站上真的載入了的清單）。⛔ **不准寫字面字型名**（`Georgia`、`"Noto Serif TC"`）—— 站上沒載入的字體前台不會出現，只會 fallback 成系統字，看起來像生效其實沒有；lint 會擋（`font_not_in_library`）。前面放歐文、後面放中文；只想換一種就只給一個。**只能整頁換（token-class），不能只換某一句**（地址底下寫 font-family 會被擋）。

### B-3 標題（h1–h6，帶 `data-hz-text`）

- **與 B-2 完全相同**：直接寫 `font-size` / `font-weight` / `line-height`，**對位會替 heading 補上內部變數**
  （`--_hz-fs` / `--_hz-fw` / `--_hz-lh` —— 它們是**輸出**，你會在計畫書裡看到，⛔ 不是你要寫的東西）。
  字距、顏色照 B-2。詳見 §A-5 第 1 坑（2026-09-07 起的改動，那條是這件事的主人）。
  ⚠️ **改了標題的桌機字級就一定要交代平板 / 手機**（自己寫 `@media`，或標 `/* hz-ladder: follow */`）——
  不寫會被對位擋下、送不出去。判準與兩種答法的主人是 §A-5 第 2 坑。
- 整頁：`:root body .hz-typo-typo-01 { font-size: 3.75rem; font-weight: 400; line-height: 1.5; letter-spacing: .05em; }`
- 只這顆：`:root body [data-hz-text="hero.title"] { font-size: 2.5rem; }` ＋ `@media` 兩份
- **常見目標**：更大更輕 → 字級加大、`font-weight: 400`；沉靜 → 行高 1.4–1.5、字距 .05em
- **字型照 B-2 寫，不必用 `--_hz-`**：`font-family` 沒有鏡像變數的問題（heading 的 `!important` 只鎖字級/字重/行高），直接 `:root body .hz-typo-typo-01 { font-family: var(--hz-font-lora), var(--hz-font-noto-serif); }` 就會生效。⚠️ **同一個 `typo.NN` 很可能同時掛在標題與非標題上** —— 改它就是**兩邊一起改**（不只字型，字級字重行高字距都一樣）。動之前先 grep 前台 HTML 看那個 class 出現在哪些元素上。

### B-4 按鈕

- **地址** `[data-hz-button="hero.cta"]`
- **內容**（`block-set-button`）：文案、連結、`target`。⚠️ **只改文案的話，`fill-page-text` 的 `texts` 也寫得進按鈕**，不必為了一句文案另外叫 `block-set-button`（要動連結 / target 才非它不可）。連結收 http(s) **與同頁錨點 `#booking`**（2026-09-06 起 plugin 與 dev builder 都放行）；錨點目標用 `block-set-anchor` 設在那顆 block 上，兩邊 id 要一致。
- **長相**（`button.NN` 看 class `hz-btn-button-NN`）：`--hz-button-NN-bg` / `-text` / `-border` / `-border-width` / `-radius` / `-padding-x` / `-padding-y` / `-hover-bg` / `-hover-text`
  - 整頁：`:root { --hz-button-01-radius: 999px; }`
  - 只這顆：`:root body [data-hz-button="hero.cta"] { --hz-button-01-bg: var(--hz-color-15); }`
- **常見目標**：
  - 空心：`--hz-button-01-bg: transparent; --hz-button-01-border: var(--hz-color-14); --hz-button-01-border-width: 1px; --hz-button-01-text: var(--hz-color-14);`
  - 藥丸：`--hz-button-01-radius: 999px; --hz-button-01-padding-x: 2em;`
  - 不搶眼：`-bg` 指向次強調色 `var(--hz-color-15)`，或 hover 改「壓深」`-hover-bg: var(--hz-color-05)`
  - 換成另一款（例如本來就有的空心款 button.05）：`block-set-style { slot, button_preset: "button.05" }`（這是換字，寫進 block 資料，也合法）
- ⚠️ **同一個 preset 的兩顆按鈕，只改一個地址 → 前台兩顆不一致，那是設計**（地址級只綁那一顆；實測 hero 變空心藥丸、CTA 還是實心）。想要**全站一致**就寫 token-class / `:root`，⛔ 不要寫地址。
- **不能**：在按鈕上直接換字型（字型跟著文字的 `typo.NN` 走，見 B-2）；按鈕文字字級也走 `button.NN` 的 typo（`typo.09/10`），不直接寫在按鈕上
  —— ⚠️ 真寫了會落殘留 `slot_has_no_facet`（**按鈕／徽章的槽收不下「字」**；意思與該怎麼辦見 `playbook-get name="guide/design/writeback"`）

### B-5 Badge（藥丸標籤）

- **地址** `[data-hz-badge="hero.badge"]`
- **內容**：文字 `block-update-content`；文字 ↔ 藥丸切換 `block-set-style { display_mode: "text"|"badge" }`
- **長相**（`badge.NN` 看 class `hz-badge-badge-NN`）：`--hz-badge-NN-bg` / `-text` / `-border` / `-border-width` / `-radius` / `-padding-x` / `-padding-y`（padding 單位 **em**）
- **常見目標**：淡底深字 → `-bg: var(--hz-color-03); -text: var(--hz-color-08)`；不那麼吵 → 同上 + `-padding-x: 1.15em; -padding-y: .5em`

### B-6 圖片

- **地址** `[data-hz-image="hero.image"]`
- **內容**：換圖 `block-set-image`（`image_id` 或 `url`）
- **版面**（`block-set-geometry`）：比例 `aspect_ratio`、裁切 `object_fit`、焦點 `object_position`（九宮格）
- **長相**：兩條路，**歸屬不同、別混**
  1. **換綁外框**（工具通道）：`block-set-style { frame: "surface.NN" }`。**實測蓋得過模板寫死的圓角**（2026-09-06 站主親驗：8px → 14px；`.hz-frame-surface-NN` 特異性 0,1,2 贏模板的 `cv:rounded-*` 0,1,0）。這是 **block 自己的欄位** → 依 §A-1 判準本來就在家，**③ 不寫回**。
  2. **草稿層**：地址底下 `border-radius: var(--hz-surface-NN-radius); box-shadow: var(--hz-shadow-NN);` —— 這種才是 ③ 會收走的。
- ⚠️ **外框只借三樣：圓角、邊框、陰影。** 借的**不是**整格面 —— 面的**底色、遮罩不會跟來**（`SurfaceDomain` 只為 frame 產出 `border-radius` / `border` / `box-shadow` 三行，就這三行）。所以「圖片要有底色或壓一層色」用外框做不到，別試。
- ⚠️ **挑面要挑「無框無影」的**，否則會連帶多一條邊框或一層陰影。哪幾格無框無影**看字典**（每站不同，字典是唯一準的）。
- **想要字典沒有的圓角**（例如 8px，而字典裡的面只有 0 / 14 / 16）→ **直接寫數字**（`border-radius: 8px` 寫在圖片地址底下），照 §A-6 記一筆。③ 看到這個值會問站主「要不要開一格新的面（無框無影、圓角 8px）」，開了以後才有得借。⛔ 不要為了遷就現有的面把設計改成 14px。
- ⚠️ **外框設了目前清不掉**（`block-set-style` 的 frame 不收空字串）—— 要還原只能換綁另一個面。已知缺口，回報一聲即可。
- ⚠️ **`media_summary.images.filled: 0` 不等於畫面上沒圖** —— 模板預設圖照樣渲染。看到「`filled 0` ＋ 前台有圖」= **那是預設圖，要換不是要補**；交出一張不相干的預設風景照是你的責任。
- **常見目標**：圓角跟卡片一致 → `var(--hz-surface-01-radius)`；補一層淡陰影 → `var(--hz-shadow-02)`
- ⭐ **要一張佔位圖但素材還沒到、上傳不了？**用
  `https://placehold.co/<寬>x<高>/<底色>/<文字色>.png` 直接當圖片網址，不必先上傳到媒體庫。
  **文字色寫成跟底色一模一樣**就會得到一塊乾淨的純色面（例如 `https://placehold.co/1600x900/EDE7DA/EDE7DA.png`），⛔ 不會印出「1600x900」那行字。
  這是暫時的繞法：正式素材還是要走 `site-upload-media` + `block-set-image`。
- ⚠️ **前景圖（`<img>`）加不了遮罩。** 外框只借三樣、`<img>` 本身不能有偽元素，全庫沒有任何 block 做得到。要「圖 + 一層色 + 上面壓字」的效果，改用 **B-1 的面當背景**：`block-set-surface` 設背景圖，同一層的 `overlay` / `overlay-alpha` 就是那層遮罩（只對 image / video 有效，純色與漸層沒有這層）。
- **遮罩畫在 `.hz-bg-override::after`，不是 `::before`** —— 驗收「遮罩有沒有上去」就查 `::after`。
- **`block-set-surface` 的 media 層有 `overlay`**（吃色票 slug / hex / rgba），**只對 image / video 有效**，純色與漸層沒有這層。

### B-7 圖示 icon

- **地址** `[data-hz-icon="grp-01.sec-01.icon"]`
- **內容**：換圖示 `block-set-icon`
- **長相**：顏色 `--hz-color-NN`（地址底下），動效／hover 同 B-9/B-10
- ⚠️ **沒填的 icon / badge 槽，前台會印模板的預設佔位**（不是空白）—— 你以為「沒設就不會出現」，實際上畫面上會多一個看不懂的小圖示或標籤。不要它就用 `block-set-slot-visibility` 把那個槽藏掉，不是放著不管。
- ⚠️⚠️ **出廠示範文案不只在「內文」那種槽，CTA 按鈕與標題群也有**（2026-09-09）——
  最容易漏掉的就是按鈕（畫面上一顆寫著「了解更多」的按鈕看起來完全正常，其實是模板自帶的）。
- ⚠️⚠️ **① 的驗收要抓前台逐槽掃，⛔ 不要信 `empty_field_keys`**：那一欄回的是「欄位是不是空的」，
  而模板的示範文字**根本不佔欄位** —— 槽是空的、畫面上卻有字。所以「`empty_field_keys` 是空的」
  ≠「文案都填好了」。驗收一律：`curl` 抓前台 → 逐段看畫面上每一句是不是你（或站主）給的字。

### B-7b 重複格 repeater（卡片牆、清單、tier）

- **是不是 repeater**：`page-get-structure` 那顆 block 的 `is_repeater: true`，附 `groups`（`grp-01…`）、`sections_per_group`、`default_visible`；巢狀（tier 裡再有 feature）會標 `nested: true`。
- **形狀（① 就做）**：顯示幾格 → `block-set-repeater-visibility`（或 batch 的 `set-item-visibility`）。要幾格是形狀的事，先決定、先藏。
- ⚠️ **巢狀 repeater 要兩步，`show_only` 只管得到外層**（2026-09-09）：對 tier 這種
  「格裡面還有格」的 block 下 `show_only`，它收的是**外層**（留幾個 tier）；
  **內層**（每個 tier 裡的 feature 幾條）要再下一次 `hide_specific` 指名藏哪幾條。
  只做第一步就交 = 外層對了、每格裡面還掛著一長串示範項目。
- **內容（有給才填）**：填第 N 格 → `block-update-content`，key 用 `page-get-structure` 給的 `semantic_key`（`items[1].title`、巢狀 `items[1].items[0].meta`），不要自己拼欄位名。沒給文字的格留示範文字即可；等到「內容」段再補。
- **repeater 還全空的時候，key 從哪來**（2026-09-06 起）：`page-get-structure` 的 `empty_fields` 會給**一格模板**的 `semantic_key`（`items[0].title`、`items[0].desc`…）。那一格就是**每一格的形狀** —— 照它的 key 寫 `items: [{…}, {…}]` 填幾格都行。**其餘幾十格刻意不列**（防爆量），容量看 `repeater.sections_per_group`，不是漏給。
  - 送錯 key 時，`block-update-content` 的失敗回應會回 `accepted_item_keys`（格內可用的欄位名，如 `["name","role"]`）。⚠️ 旁邊那個 `accepted_bare_keys` 對 repeater **仍然**是段落層的清單、不含格內的 key —— 那是刻意的，別把它當成「這顆 block 沒有 key」。
  - **巢狀（pricing 那種 tier 裡再有 feature）不給模板格**，改給 `empty_items_hint` 叫你去 `block-inspect` 拿真正的 key。
  - ⚠️ **deploy 之後你的 session 要重開才看得到**（同附錄那條：MCP schema 是 session 啟動時載入的）。如果你手上的回應還是老樣子「一個 key 都沒有」，先重開 session 再看；還是沒有才回報。舊行為的暫時解法是去 `block-get-template` 讀 `@hz-repeat` 檔頭。
- ⚠️ **`empty_fields` 裡 `_url` 結尾的欄位 `semantic_key` 是 `null`** —— 那是刻意的（url 不是文字、算不出語意 key），**跳過它**，別把 null 當 key 送出去。
- **長相（全部格一致）**：想讓每一格都一樣，就改**這顆 block 用到的字**（token-class 或 `:root`），一次改完。
- **長相（只改某一格的內容物）**：**做得到。** 格內每個槽都有自己的地址，格號在路徑裡：`grp-01.sec-01.title` 是第一格的標題、`grp-01.sec-02.title` 是第二格的。所以「只把第一張卡的標題改成強調色」寫 `:root body [data-hz-text="grp-01.sec-01.title"] { --hz-color-07: …; }` 就成立。
- **長相（只改某一格的「卡片底」）**：**做得到。** 卡片容器那一層多半自己就是一個面，地址帶格號：`[data-hz-surface="grp-01.sec-01.card"]`。所以「只把第一張卡換底色」寫 `:root body [data-hz-surface="grp-01.sec-01.card"] { --hz-surface-NN-bg: var(--hz-color-NN); }` 就成立（`NN` 看那個面的 class `hz-surface-surface-NN`）。
  ⚠️ **slot 名不一定叫 `.card`**（有的 block 叫 `.item`），⛔ 不要用背的 —— `block-get-template` 看那顆 block 印出來的 `data-hz-surface` 是什麼，照抄。
- **長相（只改某一格的「留白」）**：**做不到。** 格內的 gap 載體 id 每格重複，同一顆 block 的每一格共用同一個號碼，選不出「只有第一格」。站主 2026-09-06 定調這是限制，不是缺陷 —— 想要某一格特別寬，照 §A-6 回報。
- ⚠️ **格內地址不是頁面唯一鍵**：`grp-01.sec-01.title` 這種路徑在**別顆 block** 也會出現（實測一頁上有 10 組路徑各命中 2 個元素）。只想動這一顆，先用 A-4 的門牌（該 block 最外層 `<section>` 的 `data-hz-gap` id）確認範圍，或寫完看 lint 回報的命中數對不對。
- **坑**：超過 `default_visible` 的格**沒有預設值**（示範文字、內距預設是空的），但內容填得進、`show_only` 開得起、前台正常渲染；填完看一眼跟前面幾格是否一致。

### B-8 間距 gap（不是元件，是載體）

- **地址** `[data-hz-gap="kchwd0"]`；載體的 class 說它現在吃哪級：`hz-pad-14`（section 上下留白）、`hz-gap-06`（上方 margin）、`hz-flow-08`（容器 gap，縱橫同值）、`hz-flow-row-08` / `hz-flow-col-06`（容器 gap 的單軸，2026-09-12 起）
- **尺**：`gap.01–10` 元素間距（小→大）、`gap.11–16` section 留白（緊→寬）、`gap.17` = 零。**數字越大越寬**。
- **整頁**改某級的大小：`:root { --hz-gap-12: 5rem; }`
- **block 與 block 之間的距離不是外距**：block 最外層寫 `margin` 沒有用（系統把每顆 block 的上下外距釘死為 0），對位會擋；要拉開兩顆 block，寫那一段的 `padding`（③ 收成 section 上下留白）。
- **內距永遠只改這一頁**：對某種面的 class 寫 `padding`（`.hz-surface-surface-05 { padding: 24px }`），③ 展開成這頁掛那個 class 的每張卡各自的內距，不改那種面的定義、不影響別頁（站主的定義：內距是每頁不同的東西）。
- **改某一格的上下留白（先用這個，就是直覺寫法）**：地址底下直接寫 longhand —— `padding-block-start: var(--hz-gap-13);` / `padding-block-end: var(--hz-gap-13);`（兩邊同值就兩行都寫）。合法、③ 收得回。
- **整格換級（備案）**：在門牌底下重新定義那一級 `:root body [data-hz-gap="kchwd0"] { --hz-gap-14: var(--hz-gap-12); }`（載體 class 是 `hz-pad-14`，就改 `--hz-gap-14`）。⚠️ 它會**繼承給所有後代**（見下面的坑），所以只在「這一格整個換一級」時用。
- ⚠️ **容器的 gap 分三個詞，挑對那一個**（2026-09-12 起有分軸詞，A265）：
  - `hz-flow-NN` 落地是 CSS `gap`，**一個數字管兩個方向**。
  - `hz-flow-row-NN` = `row-gap`（只管縱向的縫）、`hz-flow-col-NN` = `column-gap`（只管橫向的縫）。
  - ⇒ **單軸現在寫得進去了**：畫 `row-gap: 6px` / `column-gap: 28px` ③ 會各寫一筆，不再擋。
  - ⚠️ **但只有新做的 block 才有分軸 class**。存量 block 的模板只有 `hz-flow-NN`，
    對它們畫單軸仍然寫不進去 —— ③ 會落殘留說「這顆 block 沒有分軸詞，重跑產線才會長出來」。
    那不是你畫錯，是那顆 block 的年紀問題。
  - ⚠️ **`column-gap` 不是左右留白**：前者是子元素**之間**的橫向距離，後者是容器自己的
    `padding-inline`。挑錯的症狀是「設了沒反應」。
  - 【歷史】在分軸詞出現之前（2026-09-12 之前），只畫一軸會讓另一軸被一起改而畫面上看不出來
    （43091 實測：column-gap 28px→6px、56px→32px），所以當時 ③ 一律擋下不寫。
- **也可寫進 block 資料**：`block-set-geometry { gaps: [{ gap_id, level, level_end?, axis? }] }`（gap_id 從 `block-inspect` 的 `gap_carriers` 拿）
- ⚠️ **`block-set-geometry` 的 `spacing` 在「模板有寫死內距」的 block 上是「疊加」不是取代**：症狀是第一張卡的圖多一圈留白、CTA 框線溢出卡片邊。回應的 `warnings` **只**講「只設一邊其他三邊塌 0」，**疊加這件事它不會講，要自己知道**。
- **坑**：已經用 `block-set-geometry` 調過級的載體，元素上有 inline `margin-block-start: var(--hz-gap-10)`——要改就改 inline 指向的那級（`--hz-gap-10`），寫原始數字無效；`--hz-gap-NN` 寫在地址上會繼承給後代。
- ⚠️ **內距不一定在最外層**（2026-09-09）：你看到的那圈留白常常是**內層某個容器**的
  `padding`，不是 section 自己的。⛔ 別對著最外層一直加減 —— 從畫面上那條邊往內找是誰身上有值
  （`getComputedStyle(el).paddingTop` 逐層量），找到再改。
- ⚠️⚠️ **藏掉一個槽，它旁邊的留白不會跟著消失**（2026-09-09）：留白是**載體**身上的，
  跟被藏的那個槽是兩回事 —— 藏完會留下一段「隔開空氣」的空白。藏槽之後**一定要回頭問一句**：
  「這段留白現在還隔開什麼？」沒有了就把那一級調小或改成 `gap.17`（零）。
- ⭐ **留白掛在哪一層 → 問 `block-inspect` 的 `gap_carriers`**：每一筆的 `element` 就是掛載點
  （例 `section.hz-pad-11.hz-pad-x-08`），`as` 說它是哪一軸 —— `pad` 區段上下留白 /
  `padx`（`as_x`）左右留白 / `margin` 元素上方 / `flow` 容器內子元素。⛔ 別對著最外層猜。
- ⚠️⚠️ **沒有 `as: 'pad'` 的載體 = 這顆的區段上下留白寫死、調不到**（不是「沒有載體」——
  它可能有好幾個 `margin` 載體，那些是元素間距，動不到你看到的那一圈）。實例：
  `hero-simple-eyebrow-center-v1` 有 3 個載體卻全是 `margin`，留白寫死在內層容器。
  遇到就**照 §A-6 回報**，⛔ 不要用 custom HTML 或硬塞 margin 去繞。字典的「不適合」欄也會先講這件事。

### B-9 / B-10 動效 motion 與 Hover —— **直接寫標準 CSS**（2026-09-12 起）

⭐ **這兩樣不再是「填 token 變數」，你就當成在寫普通 CSS。** 想要什麼效果就寫什麼效果 ——
`:hover`、`transition`、`animation`、`@keyframes`、`@media` 全部可以用，值寫你要的。

```css
/* hover：滑過卡片浮起來 */
:root body [data-hz-surface="grp-01.sec-01.card"]:hover { transform: translateY(-4px); transition: transform 250ms ease-out; }

/* 動效：自己定義一個進場 */
@keyframes hz-p54077-rise { from { opacity: 0; transform: translateY(24px) } to { opacity: 1; transform: none } }
:root body [data-hz-gap="kchwd0"].hz-inview [data-hz-text="sh.title"] { animation: hz-p54077-rise 500ms ease-out both; }
```

**③ 不會把它收成 token** —— 它會被搬進**這一頁的「自訂樣式段」**：永久留在頁上、跟著頁面複製走，
報告裡標「**自訂保留**」。所以你寫的東西不會消失，也不會變成別頁的事。
（元素進畫面時會被加上 `hz-inview` class，所以「進畫面才播」照 §上面那個例子寫就行。）

**範圍規則（違反就當場擋下、有聲，並指名是哪一條選擇器）**：

- **每個選擇器都要含「這一頁的東西」**：地址（`[data-hz-text="…"]` / `[data-hz-surface="…"]` …）、
  門牌（`[data-hz-gap="…"]`）、repeater 格根（`[data-hz-repeat]`）、block 根（`[data-a2ui-component="…"]`）、
  custom HTML 根（`[data-hz-custom="hauzii-custom-html-v1"]`），或一個 `#anchor`。
- **`@keyframes` 名字必須以 `hz-p<page_id>-` 開頭**（例：頁 54077 → `hz-p54077-rise`）—— 防跨頁撞名。
- ⛔ **不准全站選擇器**：`body` / `html` / `:root` / `*` 一律擋。
- ⛔ **不准 `!important`**（與 §A-2 同一條紅線）；也擋 `@import`、外部 `url()`。
- ⚠️ **同一頁有多顆 custom HTML block 時，只能靠 `#anchor` 區分**（它們的根屬性一模一樣）。這是已知限制。
- ⚠️ **「減少動態」你不用管** —— plugin 有一條全站規則在處理（作業系統開了減少動態時，token 動畫與你手寫的一起被壓掉）。

⚠️ **token 那一側的字也還在**（`hz-motion-motion-NN` / `hz-hover-NN` 那族的變數），要改「全站的動效預設」
仍然是改字義；但**這一頁想要什麼效果，直接寫 CSS 最快**。值域（八個 motion 欄位 / 八個 hover 欄位）見字典。

### B-11 裝飾線 decor

- 地址 `[data-hz-decor="divider.tint"]`，但顏色印在 inline style，地址選擇器蓋不過。**改上游色票**：`:root { --hz-color-20: … }`。同名 decor 在多顆 block 會一起變。
- **只想改一根裝飾線，就走工具**：`block-set-style { slot: "decor-01.tint", decor: "color.16" }`（實測前台 inline 由 color.14 → color.16）。⚠️ 改上游色票**會連帶動到別的東西** —— decor 常與卡片預設邊框共用同一格色（實測改 `--hz-color-20` 會讓五張卡的邊框一起變）。要單根就用工具、要整批才改色票。

### B-12 顯隱

- 單一槽：`block-set-slot-visibility`；repeater 整格：`block-set-repeater-visibility`。

### B-13 v1 不教

**草稿層碰不到、也沒有工具，想動就回報**：整頁內容寬度（系統裡沒有「寬度」這種字）。

⚠️ **「整站深色」目前做不到閉環，別在這上面耗時間**：
- 色票 user zone 那格 **「Page Background」（color.37）與主題 body 沒有接線** —— 改它不會讓頁面底色變深。看到有這格不代表它通。
- 第三方外掛的容器（例如 LRM 的 `div.lrm-main`，掛在 body 直下、main 與 footer 之間、高約 630px、透明底）在深色頁會露出一條**白帶**。任何深色頁都會中，不是你寫錯。
兩件都**回報就好，不要硬寫**（硬寫要碰主題變數或第三方 class，兩者都在草稿層規則外）。

**字型已經可以了**（2026-09-06 起，見 B-2）——但只能用字典「可用字型」段列出來的，而且只能整頁換（`.hz-typo-typo-NN`），不能只換某一句。想要清單以外的字體，那要先加進平台字型庫，回報給站主。

**不走草稿層，但有工具可以做**（不是不能做，是走另一條路）：
- 背景**圖 / 影片** → `block-set-surface`（⚠️ **漸層與純色不在這裡** —— 直接在草稿層畫，見 §B-1）
- 面的內距 spacing、圖片焦點／比例 → `block-set-geometry` / `block-set-surface`
- 插座（social / menu / contact）、影片、地圖 → 各自的 block 工具

**v1 真的不做**：左右／上下位移（translate）、縮放（scale）。想動 → 回報，不要硬寫。
⚠️ **不含 hover 的 `-scale` / `-translate-y`** —— 那兩個是 **hover 這個字的欄位**（`--hz-hover-NN-scale` / `-translate-y`，見 §B-10），照 B-10 寫就對了。這裡說不做的是「元素靜止時就位移／縮放」那種。

### B-14 說 yes 之後會發生什麼

你交的是一份 CSS 草稿。站主說 yes 之後，③ 會把它**翻譯成系統裡的設定**再寫回去 —— 草稿本身是暫時的，會被清掉。

**③ 只問一個問題：你寫的這個值，字典裡有沒有一模一樣的字？**

| 你寫的 | ③ 怎麼判 | 會不會問站主 |
|---|---|---|
| `var(--hz-…)` 引用既有的字 | **換字**：把用到舊字的元素改綁到新字 | **不問** |
| 原始值，但**剛好等於**某個既有的字（顏色 hex 相等、長度相等） | **換字**（③ 自己對上，你不用刻意去引用） | **不問** |
| 原始值，字典沒有相等的字 | **新字義**：帶「這個字有幾頁在用」問站主 —— 蓋掉 / 開一個新字 + 這頁改綁 / 不寫 | **會問** |
| 尺上的值（間距、字級、動效秒數）沒有相等的級 | **吸附**到最近的級 —— 所以你寫的數字**可能被挪一點點**，這是刻意的 | 不問（會列在「吸附」段） |

三件你該知道的：

1. **⭐ 你不用擔心「該不該引用」**。能引用就引用最好（讀計畫書的人比較好懂），但忘了也沒關係 —— ③ 是**看值**分類的，不是看你有沒有守規則。正確性不靠你。
2. **③ 從不偷偷改一個字的定義**。凡是「這個值是新的」，一定會問。所以你看到計畫書上一堆問題，不是你做錯，是那些值本來就是新的。
3. **「值一樣」還不夠，還要「用途一樣」**。把「深底標題文字」改成白色、而白色剛好等於一個叫「Page Background」的字 —— ③ **不會**幫你綁過去（那是換錯字），它會列出來問。

**對不到任何一條** → 記成**殘留**：畫面上看得到、但寫不回系統的差異。殘留不是失敗，是「這裡缺一個字」的紀錄。

**所以你可能會在報告裡看到「新增了一個 `color.37`」** —— 那是第 3 條的後半：你用了一個色盤裡沒有的顏色，系統幫你在 user zone 開了一格、取名綁好。這是預期行為，不是誰做錯了。

**幾個你該知道的開關**（站主決定，你不用設）：每一類值各有 `snap`（取最接近）／`exact`（必須完全一樣）／`add`（開新格）三種待遇。目前預設是長度與字距字重行高動效走 **snap**、顏色與 preset 走 **add**。站主若關掉 `add`，那些原本會開新格的就會變成殘留 —— 報告會變長，但不會有新格冒出來。

**對你的實際影響**：
- 想精準就直接寫字（`var(--hz-gap-12)`、挑 preset），別寫原始數字 —— 寫了會被 snap。
- 顏色反過來：**不要為了「用現有的色」硬湊**，你要什麼顏色就寫什麼顏色，系統會替你開格。
- 殘留清單是**你的產出的一部分**，不是垃圾。它直接回答「這個設計系統還缺哪些字」。

---

### B-15 custom HTML —— 逃生口，不是工具箱

> 站主 2026-09-09 定調（fix-list C-11），2026-09-10 收窄成下面兩件。
> [01-seat-rules](../01-seat-rules/SKILL.md)「什麼不准做」第 7 條只放一句指過來，**規矩的主人在這一節**。

**什麼情況可以用、誰能用 —— 這一段先留白**（站主 2026-09-10：等看過實際用法再定）。
⛔ 不要自己補一套使用規則進來。

**它現在不吃 token —— 講一次就好**：custom HTML 裡寫的顏色 / 字級是死值，③ 收不回、換色盤也不跟。
**這是已知的路，不是待解的問題** —— 之後產線會把它轉成正式 block。⛔ **不必每次用到就提醒站主一遍**
（顏色仍盡量寫 `var(--hz-color-NN)` 引用既有的字，那個會跟）。

**用了就記一行**（這是硬要求）：寫在回報裡 —— **哪一頁、哪一段、為什麼沒有合適的 block**。
⭐ **那一行就是日後把它轉成正式 block 的需求單** —— 沒記＝這個缺口永遠不會被補，下一個人再手刻一次。

**格式（照這個寫，一行一筆；用了請回報給站主）**：

```
custom-html | <站> | <頁 id / slug> | <第幾段> | 為什麼沒有合適的 block（一句）
```

**寫的時候兩條**：

- ⛔ **不准 `100vw`** 做滿版：頁面有捲軸時 `100vw` 會**寬過內容區**、多出一條橫向捲軸。
  要滿寬直接 `width: 100%`（block 的容器本來就是滿版的）。
- **RWD 用 `@container`，不要 `@media`**：custom HTML 是嵌在 block 裡的一塊，它該跟著**容器**寬度變，
  不是跟著視窗。用 `@media` 會在側欄 / 窄容器裡失準。

## C. 寫入與自驗

### C-1 寫進站台

1. **畫稿用對位送**：先試跑、看對位表，沒問題再加 `apply: true` 真的送（叫法、對位表欄位、失敗碼 → `playbook-get name="guide/design/locate"`）：
   ```
   design-locate-style-layer { page_id: <PAGE_ID>, css: "<畫稿全文>" }
   design-locate-style-layer { page_id: <PAGE_ID>, css: "<畫稿全文>", apply: true }
   ```
   - ⚠️ **頁面要先公開**（草稿頁工具抓不到前台）。
   - ⚠️ **每次送的是整份**：會取代這頁原本的草稿層，上一輪要留的一起帶上。
   - 只有背景**圖 / 影片**是 block 資料，走 `block-set-surface`。**漸層與純色照畫**（見 §B-1）。
   - 要清空這一頁的草稿層：`design-update-style-layer { page_id, css: "" }`（③ 收完工具會自己清，平常不用）。
   內容類用對應的 `block-*` 工具，直接寫。
2. 重抓前台（網址加 `?x=<隨機數>`），確認有 `/* hz-style-layer:start page=<PAGE_ID> */`。
   ⚠️ 用 `?page_id=` 抓要**跟隨轉址**：它會 301 到固定網址，不跟隨會拿到 0 bytes，看起來像草稿層不見了（不是）。
3. **自驗**（每次寫完都跑）：
   ```text
   design-lint-style-layer { page_id: <PAGE_ID> }
   ```
   不帶 `css` = 檢查**已經寫進草稿層**的那段（這頁還沒寫過會回 `style_layer_empty`）；要在送出**之前**先檢查一段 CSS，就加 `css: "<那段 CSS>"`。
   `ok: true` 才算過。有 error 照 `reason` 修；同一種 reason 修三次還在 → 停，原樣寫進回報。
4. **用眼睛看**：Chrome DevTools 截圖（`navigate_page` 換 `?x=` → `take_screenshot` fullPage jpeg → `Read` 看圖），不滿意再改。截圖是**你自己**看用的；給站主的是網址，截圖只在站主要求時給。
   ⚠️ **`new_page` 回 `Navigation timeout` 不等於沒開成** —— 這個站首頁載入常超過 10 秒（Rank Math 自己就吃掉 6 秒多）。收到 timeout 先 `list_pages` 看那個分頁在不在，多半已經開好了；直接重開會累積一堆分頁（實測有人為此多花 7 趟）。
   ⚠️ **要驗動效秒數，得在注入「停動效」的 style 之前讀** `getComputedStyle(el).animationDuration` —— 注入之後讀到的一律是 `0s`（你自己把它關掉了），會誤判成動效沒設定。順序：先讀秒數 → 再注入停動效 → 才截圖。
   ⚠️ **截圖前一定要先把進場動效停掉**，否則 fullPage 截到的是「還沒播到的段落」= 大片空白，你會誤判成內容沒填（兩個座位各自獨立踩過）。**先捲一輪再截沒有用** —— fullPage 是重新合成的，不是把你捲過的畫面拼起來。標準動作：
   ```js
   // evaluate_script：注入後再截圖
   const s = document.createElement('style');
   s.textContent = '[class*="hz-motion-motion"]{animation:none!important;opacity:1!important;transform:none!important}';
   document.head.appendChild(s);
   ```
5. **要開一頁新的**：builder **沒有建頁工具**，走 `wp-rest`：
   ```
   wp-rest { method: "POST", path: "/wp/v2/pages", body: { title: "…", status: "publish" } }
   ```
   回應裡的 `id` 就是之後所有 `block-*` 要用的 `page_id`。
6. **查媒體一律帶 `_fields`**：`wp-rest { method:"GET", path:"/wp/v2/media?_fields=id,title,source_url,media_details&per_page=20" }`。
   不帶的話每一筆會回整包 `sizes` 加一堆 rank_math meta，幾張圖就吃掉一大段 context。
   ⚠️ **`wp-rest` 沒有 `query:{}` 這個參數**（schema 只有 `method` / `path` / `body` / `full`）—— 查詢字串**寫進 `path`**，
   寫成 `query:{…}` 會拿到「GET 不能有 body」這種對不上的錯誤訊息。

### C-2 ② 怎麼驗收：站主說「不對勁」的時候先量什麼

**截圖看得出「醜」，看不出「21px 比 22px 小」。** 站主給的是**症狀**，而人的直覺會直接跳到
「自己最會做的那個動作」（調字級、加留白）—— 2026-09-09 一天內**四次憑直覺調到錯的地方，
四次都是量了才找到真病灶**。所以這一節只有一個規矩：**先量，再改；改完，再量**（§A-0 第 2 句）。

#### 症狀 → 先量什麼

| 站主說 | ⛔ 直覺（多半錯） | ⭐ 先量這個 |
|---|---|---|
| 呆板 / 差一點點 | 調字級 | **斷行**：逐字取 `Range.getBoundingClientRect().top` 分行，看最後一行剩幾個字（實例：標題斷成 19 + 2，「路。」兩字孤零零掉在第二行）|
| 分不清輕重 / 沒節奏 | 加粗、放大最重要的 | **列出這一區每個角色的字級**，看它們散布在幾 px 之內（實例：五個角色全擠在 17–22px，大標 21px 比編號 22px 還小）|
| 擁擠 | 加區塊留白 | 🔴 **先分清是「塊與塊之間」還是「塊內字與字之間」**：量最後一行文字底部到下一段第一行頂部（實例：塊間 368px 很寬，真正窄的是塊內 16 / 22 / 28px）|
| 看不到 / 整個很怪 | 換顏色 | **往下遞迴走每一層的非透明底色**，⛔ 不要只量 block 元素本身（實例：深底段裡藏著一張明度 244 的近白卡）|
| 沒有區隔性 | 加留白 | **相鄰兩塊的底色明度是不是一樣** —— 一樣的話加多少留白都還是同一塊 |
| 要有份量感 | 放大字 | **這一段有沒有自己的色場** —— 跟下一段同色就讀不出「開場」|

#### 最常犯的那一個：字放大了，周圍沒讓開

放大一個標題之後，**它周圍的間距仍是為舊字級設的** → 下一輪就被說「擠」，而修的時候又常常錯層級
（跑去加**區塊**留白，病灶其實在**塊內**字距）→ 同一件事被講好幾次。

⭐ **規則：每放大一個字，當場檢查它上下鄰居的間距。** 經驗值：間距至少要跟被放大那個字的字級同量級
（標題 32px、底下留 22px 就會擠，40px 才對得起來）。

#### 兩個層級很容易搞混 —— 先問自己在哪一層

| 層 | 掛在哪 | 怎麼找 |
|---|---|---|
| **塊與塊之間** | section padding | ⚠️ 有的在 block 元素本身、有的在內層載體 → 往下找**第一個 `paddingTop !== 0`** 的 `[data-hz-gap]`。同一頁兩種都有，⛔ 不能用背的（同 §B-8）|
| **塊內字與字之間** | 各文字元素自己的 `margin-top` 載體 | 列出這一區所有 `[data-hz-gap]`，看誰有 `mt` |

#### 「工具說成功、畫面沒變」的三種樣態

**共同點：對位放行、lint 全綠、apply 回成功 —— 每一個機制都說成功**（同 §A-5 第 5 坑）。

1. `--_hz-fs` / `--_hz-fw` / `--_hz-lh` **寫在非 heading 上** —— 那三個變數只給 `h1`–`h6`，寫在 `<p>` 上完全無效。
2. **對位翻出「父子關係相反」的選擇器** —— 回報「命中 1 個元素」但實際 0 生效（外層其實是內層的祖先）。
3. **變數寫進去、但元素身上沒有消費它的 class** —— 背景走過 `block-set-surface` 之後
   `--hz-surface-NN-bg` 的修飾 class 會從元素上消失（§B-1 有這條）。

⇒ **唯一可靠的驗收是 `getComputedStyle` 逐項對帳；⛔ 不看截圖、⛔ 不信工具回的 success。**

#### 一顆 block 可能有兩層面

`article-chapter` / `article-leadin` 這類是**外層 section 級面 + 內層 item 級卡片**。
🔴 **換了外層的面，裡面那層不會跟著走** —— 換深底時最致命（淺色卡留在深底上）。
那張卡在淺底頁面上**看不出來**（同色），外層一換色它就現形；⇒ 換色之後順便看它的內距（實例：只有 16px，文字貼著卡緣）。

#### 怎麼量（工具面）

- 量值一律 `getComputedStyle`，不要看截圖；截圖只用來看「醜不醜」。
- 要量動效秒數**得在注入「停動效」之前讀**，順序見上面 C-1 第 4 點。
- 響應式要看不同寬度，用 Chrome DevTools 的 **`emulate`**，⛔ 不要用 `resize_page`
  （後者只改視窗、不改 device metrics，媒體查詢不一定跟著換）。
- 要截整段版面時：先停動效 → 等 `document.fonts.ready` → **捲到頂**、截 viewport；
  ⚠️ `fullPage` 會把 sticky header 重複合成進去，看起來像多了一條。
- ⚠️ **驗「某張圖 / 某個嵌入（地圖 iframe）有沒有出來」一律捲到那一段截 viewport**，⛔ 不要信 `fullPage`：
  停了動效之後，地圖與剛換上的圖在 `fullPage` 裡**仍然全白**（`img.complete === true` 也一樣），
  同一次載入捲過去截 viewport 才看得到。（差點誤報「`block-set-map` 沒生效」。）
- ⚠️ **驗 gap 看 section 的 `style=`，不看 class**（級數可能被 inline 釘住）；**驗 token 看 `by_domain`，不只看頂層**。
- ⛔ **要說「這東西一直都是這樣」之前，先拿一份更早的觀測出來** —— 不要從當下狀態反推歷史。


### C-3 ③ 收回：你跑工具、站主答題

> 規矩（誰決定、同站一次一頁、跑前告知站主）在 [01-seat-rules](../01-seat-rules/SKILL.md)；工具行為（拒跑、partial、續跑、題型選項、申報碼）在 `playbook-get name="guide/design/writeback"`。
> **這一節只講怎麼問站主。**

**跑之前**：頁面已公開、草稿層還在（畫完別自己清）、在對話裡跟站主說要收哪一頁。`mode` 帶開場那一題的答案（沿用 = `reuse`、新開 = `fresh`）。

**每一題講三件事，⛔ 不要貼原始 JSON、不要唸題號**：

- **改的是什麼**：用字典裡的人話名字，不要唸 `hover.05` 這種代號（「卡片浮起來的那個效果」）
- **影響多大**：全站有幾頁、幾顆 block 在用同一個字（題目上有算）
- **三個選項**：**全站一起變**（改共用的字）／**只有這頁**（這頁新開一本自己的）／**先不寫**（這次不動它）

同一種改動的題目**併成一組問**，不要一題一題唸（十幾題會問到人放棄）。答完帶**全部**答案再產一次計畫書。

⚠️ **問卷開頭有一句固定預告，照唸、⛔ 別當成出錯**（逐字）：

> ⚠️ 答完這輪要重跑 plan，可能再長出幾題（因為你的答案改變了比對基準）——這是正常的，直到 `unanswered = 0`、也沒有新題才能 apply。

> ⭐ **定調輪 vs 日常輪（站主 2026-09-10 定調）**：計畫書的「建議」偏向「併入 / 開新格」，照建議答會讓**出廠預設永遠不被改到、整站的設計語言全塞進自訂格**。所以一個站**第一次收**的時候，把它當**定調輪**：先收**最能代表這個站的那一頁**（通常是首頁），**預設答「全站一起變」**（plan 帶 `all: "overwrite"`，明顯只屬於這頁的那幾題在 `answers` 裡另答「只有這頁」），畫的值就是新預設。定調輪之後才是**日常輪**：照計畫書建議走，後面的頁大多會直接吸附到剛改好的預設、題目自動變少；同一格若畫了跟定調頁不同的值，那是真的分歧 → 答「只有這頁」，⛔ 不要再答全站把定調頁翻掉。代價要知道：「全站一起變」會動到全站所有用那個格的頁（含沒人畫過的），收完抽幾頁沒畫過的看一眼。這是**行為約定不是程式功能** —— 工具只認答案。

> ⭐⭐ **「不寫」不是安全選項（站主 2026-09-12 定調）**。
> 一頁 hero 的深色漸層底**寫進去了**，三個淺色文字卻被答「不寫」⇒ **深底深字**，整塊看不見。⇒ 兩條規則：
> 1. **值不在字典裡的題（`add` 題），預設建議是「開新格」**；有近似色時建議「併入」。
> 2. **「不寫」只在「整顆元素 / 整區都不寫」時才選** —— 只要這顆元素（或它所在的 block）本輪還有別的東西要寫入，答「不寫」就是**把一組設計拆一半**。問卷會直接標出來，看到那行就別答「不寫」。

**為什麼題目一定要問人**：三個選項的差別是「改全站」還是「只改這頁」，站主看得到的東西（別頁長什麼樣、這個字之後還想不想共用）你看不到。**⛔ 不要自己選。**

**收完回報**：`outcome`（ok / partial / fail / verify_failed）＋前台網址＋殘留清單（寫不回系統的，照實列出，那是「系統還缺哪些字」的紀錄）。partial / fail 先照 playbook 處理，⛔ 不要自己清草稿層。

## D. 回報

0. **`mode`：`沿用` / `新開`** —— 開場問到的答案（§A-1；站主沒說就是 `沿用`）。③ 會照它印「這輪模式」並預設建議，所以這一行**每次都要有**，不能省略或寫「不確定」；真的忘了問就回頭補問。
0a. 呼叫記帳：到站主第一次看到畫面共幾趟；哪幾趟是補呼叫、為什麼
0b. **③ 問了幾題**（跑完 ③ 才知道）—— 題數多代表這一輪用了很多「新的值」，
    不代表你做錯，但可以回頭看看有沒有其實可以引用既有的字
1. 改了哪幾條、各是為了什麼。⭐ **每一輪送草稿層之前，先兩三句講「這輪要改哪幾條」＋ 貼草稿層全文原樣**；
   做完再貼一次最終版。報告以「**改了這 N 條**」為主體，**量測表放附錄** —— 不要讓量測數字淹掉你到底改了什麼。
2. lint 最後一次的 summary
3. 想做但表上沒有、字典沒字的（列出來，這是最重要的一項）
4. 哪顆 block 光改長相救不了（結構問題）——寫哪顆、為什麼
5. 手冊哪裡看不懂、哪裡靠猜
6. **動效與 hover 的歸屬**：哪些是 block 模板**自己帶的**（你沒動、原本就會播），哪些是**你設的**。兩者畫面上長得一樣，但 ③ 只寫回你設的那些；不分清楚，站主會以為某個效果是你加的（或反過來，以為你漏做）。判準：你的草稿層裡有沒有那一條 `animation-duration` / `--hz-hover-NN-*`——有才是你的。

---


## 附錄

- 字典：用 `design-get-dictionary` 拿（見 [02-seat-setup](../02-seat-setup/SKILL.md) §3），每次呼叫拿到的都是現況。
  ⚠️ **上一輪 ③ 寫回之後一定要重新呼叫一次** —— 手上那份若是寫回**之前**讀的，你會照著舊值挑字（實測：typo.01/03 前台已經是 Lora，舊字典還寫 Nunito）。

- ⚠️ **deploy 之後你的 session 要重開才看得到新工具 / 新參數**：MCP schema 是 session 啟動時載入的，中途 deploy 不會更新。所以「playbook 說有這個參數，但我的工具沒有」通常不是 bug，是你的 session 太舊 —— 回報一聲、重開即可。
- **batch 裡填文字，op 名用 `block-update-content`**（與獨立工具**同名同參數**，`get-structure` 的 `semantic_key` 直接送）：
  `{ endpoint: "block-update-content", params: { block_index: 0, content: { title: "…", items: [{ title: "…" }] } } }`
  沒對到的鍵回在該 op 的 `unmatched` + `accepted_bare_keys`，其餘 op 照跑。
  ⚠️ 同一批裡 `add-section` **剛加出來**的 block 打不到（index 是對 batch 開跑前的結構解的）→ 回 `content_needs_existing_block`，把文字放第二批。
  （batch 內另有個 `set-block-content` 吃的是 slot path `items: [{ slot, value, kind }]` —— 文字**一律走 `block-update-content`**，別混用。）
