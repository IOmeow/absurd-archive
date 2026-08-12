# 荒謬檔案館

每日自動增生一則百科體條目的維基式網站。以 Jekyll 建置，由 GitHub Pages 自動發佈——**你只要寫 Markdown 並 push，其餘全自動。**

## 結構

```
absurd-archive/
├── _entries/           ← 條目原稿，一則一個 .md（唯一需要編輯的地方）
│   └── <slug>.md
├── _layouts/
│   ├── default.html    外框：頂欄、側欄
│   ├── entry.html      條目版型：資訊框、參考文獻、分類、前後導覽
│   └── type.html       分類頁版型
├── types/              八個分類頁（固定，不需再新增）
├── assets/style.css    樣式
├── index.html          條目索引與檢索
├── about.md            關於本館
├── random.html         隨機跳轉
└── _config.yml         網站設定
```

## 網址

| 頁面 | 路徑 |
|---|---|
| 條目索引 | `/absurd-archive/` |
| 單則條目 | `/absurd-archive/entries/<slug>/` |
| 分類 | `/absurd-archive/types/occupation/` |
| 關於 | `/absurd-archive/about/` |
| 隨機 | `/absurd-archive/random/` |

每則條目有自己的網址，可單獨分享。

## 新增一則條目

在 `_entries/` 放一個新的 `.md`，**檔名就是網址**（請用英文小寫加連字號），然後：

```bash
git add -A && git commit -m "新增條目" && git push
```

約一分鐘後自動上線。不需要在本機建置任何東西。

## 條目格式

````markdown
---
date: 2026-08-12                    # 必填，YYYY-MM-DD，決定排序
type: 荒謬職業                       # 必填，八種類型之一
title: 電梯沉默維護員                 # 必填
tagline: Elevator Silence Officer；職等 7 至 11   # 必填，外文名或學名
hatnote: 本條目介紹的是 1974 年設立之公務職系。…    # 選填，留空會自動套用維護模板
infobox:                            # 選填，右側資訊框
  - label: 設立年份
    value: 1974 年
  - label: 現存人數
    value: 43 人
cats:                               # 選填，頁尾分類標籤
  - 公務職系
  - 1974 年設立
---

**電梯沉默維護員**（Elevator Silence Officer）為一種…[^1]

### 小節標題

- 清單項目
- 清單項目

## 備註

最後一擊的那句話。

[^1]: 陳允中（1998）。《垂直空間中的社會抑制》。台北：建築行為研究會，頁 44–61。
````

參考文獻用標準 Markdown 註腳語法 `[^1]`，Jekyll 會自動編號、產生上標連結與底部的「參考文獻」區塊，點擊可互相跳轉。註腳區塊一律排在正文最後，所以「備註」小節請寫在註腳定義之前。

`type` 必須是下列八種之一，否則側欄與分類頁會連不到：
`荒謬發明`、`荒謬理論`、`荒謬生物`、`荒謬制度`、`荒謬職業`、`荒謬地點`、`荒謬節日`、`荒謬說明書`。

## 首次上線

1. 在 GitHub 建一個名為 `absurd-archive` 的**公開** repo，不要勾選任何初始化檔案。
2. 在本資料夾執行（`<你的帳號>` 換成 GitHub 使用者名稱）：

   ```bash
   git remote add origin https://github.com/<你的帳號>/absurd-archive.git
   git branch -M main
   git push -u origin main
   ```

3. 到 repo 的 **Settings → Pages**，Source 選 `Deploy from a branch`，分支選 `main` / `(root)`，儲存。
4. 約一分鐘後上線：`https://<你的帳號>.github.io/absurd-archive/`

若 repo 不叫 `absurd-archive`，記得同步修改 `_config.yml` 的 `baseurl`。

## 每日更新

排程任務 `daily-absurdity` 於每日凌晨 3:00 執行：寫入新的 `_entries/*.md` → `git commit` → `push`。GitHub Pages 偵測到 push 後自動重新建置。

若 push 需要認證，改用帶 token 的遠端位址：

```bash
git remote set-url origin https://<你的帳號>:<TOKEN>@github.com/<你的帳號>/absurd-archive.git
```

Token 於 GitHub → Settings → Developer settings → Personal access tokens 產生，勾選 `repo` 權限即可。

## 本機預覽（選用）

不預覽也完全沒問題，直接 push 看線上版即可。想預覽需要 Ruby：

```bash
bundle install
bundle exec jekyll serve
```

## 建置失敗怎麼辦

md 的 frontmatter 若有語法錯誤，GitHub 會建置失敗並寄信給你，網站會停在上一個成功版本。到 repo 的 **Actions** 分頁可看到錯誤訊息，最常見的原因是 YAML 縮排不對或冒號後少了空格。
