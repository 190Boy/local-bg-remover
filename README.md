# Local BG Remover｜瀏覽器本機 AI 去背工具

這是一個純前端的圖片去背工具。圖片會在使用者自己的瀏覽器中處理，不會上傳到我的伺服器。

## 正式使用網址

正式版請使用 Cloudflare Workers / Pages 網址：

https://local-bg-remover.zooyoungboy.workers.dev/

## 備份 / 原始碼網址（已關閉）

GitHub Pages 版：

https://190boy.github.io/local-bg-remover/

這個網址主要作為備份或舊版參考，不建議當正式使用網址。

## 為什麼正式版使用 Cloudflare？

這個工具使用瀏覽器端 AI / WASM 進行去背。

Cloudflare 版已設定 COOP / COEP headers，可以讓瀏覽器進入 cross-origin isolated 狀態：

```js
self.crossOriginIsolated === true
```

這樣 WASM 多執行緒才有機會啟用，速度會比 GitHub Pages 版更好。

GitHub Pages 版通常無法自由設定這些 HTTP headers，因此可能只能用 WASM 單執行緒，速度會比較慢。

## 重要提醒：不要誤刪

請不要刪除這個 GitHub repo。

Cloudflare 版是從這個 GitHub repo 部署的。如果刪除 repo，Cloudflare 之後將無法重新部署或更新。

可以做的事情：

- 可以關閉 GitHub Pages
- 可以保留 GitHub Pages 當備份
- 可以修改 `index.html`
- 可以更新 Cloudflare 部署

不要做的事情：

- 不要刪除整個 GitHub repo
- 不要刪除 `index.html`
- 不要刪除 Cloudflare 部署設定檔，例如 `wrangler.jsonc`
- 不要隨便移除 `_headers` 或與 COOP / COEP 相關設定

## 目前專案用途

這個工具主要用於：

- 上傳圖片
- 在瀏覽器本機進行 AI 去背
- 輸出透明背景 PNG
- 避免圖片上傳到第三方後端

## Cloudflare 相關設定

如果需要啟用 WASM 多執行緒，Cloudflare 需要保留類似以下 headers：

```txt
/*
  Cross-Origin-Opener-Policy: same-origin
  Cross-Origin-Embedder-Policy: require-corp
  Cross-Origin-Resource-Policy: cross-origin
```

如果日後部署後速度變慢，可以打開瀏覽器 Console 檢查：

```js
self.crossOriginIsolated
```

正常正式版應該回傳：

```js
true
```

如果是 `false`，代表 COOP / COEP 設定可能沒有生效。

## R2 注意事項

目前這個去背工具本身不需要使用 R2。

請不要把 AI 模型或使用者上傳圖片放到 R2，除非確認流量與費用風險。

目前建議保持：

- HTML / JS：由 Cloudflare Workers / Pages 提供
- AI 模型：由原本的外部 CDN 載入
- 圖片處理：在使用者瀏覽器本機完成

## 日後維護流程

如果要更新工具：

1. 修改 GitHub repo 裡的 `index.html`
2. Commit 到 `main`
3. Cloudflare 會自動重新部署
4. 打開正式網址測試
5. 在 Console 輸入：

```js
self.crossOriginIsolated
```

確認結果仍然是：

```js
true
```

## 簡短結論

正式使用：

https://local-bg-remover.zooyoungboy.workers.dev/

GitHub Pages 只是備份：

https://190boy.github.io/local-bg-remover/

不要刪除 GitHub repo，因為 Cloudflare 部署來源仍然依賴它。
