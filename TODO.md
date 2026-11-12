# TODO — 封存前

> 工作筆記，對應 PR「Archive notice」。封存 repo 前完成；這個檔案可以留著當紀錄，或在封存前刪除。

客戶端（mobile-app、WebClients）預設已不需要這個後端。程式碼面只差把 README 的封存聲明合併進 main（`**.md` 變更不會觸發部署）。

## 關閉服務前

- [ ] 行動端無後端版本已上架，舊用戶已收到「先到 home.privy.io 匯出私鑰」的通知
- [ ] 網頁端已用空的 `NEXT_PUBLIC_BACKEND_URL` 重新部署
- [ ] 匯出 `User.walletAddress` / `User.scaAddress`，留作用戶回報「地址不見了」時的對照
- [ ] 取消所有 Stripe 訂閱
- [ ] 移除所有 Plaid items（否則持續計費）
- [ ] Bridge：確認沒有在途的入金／出金，處理虛擬帳戶與 liquidation address
- [ ] 結清推薦返現餘額（`User.cashbackBalance`、`ReferralCashbackWithdrawal`）
- [ ] 刪除伺服器保存的用戶交易所 API key（`ExchangeAccount`）與 Plaid access token（`PlaidItem`）
- [ ] 依資料保存政策處理或刪除資料庫

## 關閉

- [ ] 關閉 Cloud Run 服務與 Cloud SQL（GCP 由你另外管理）
- [ ] 撤銷各合作方的 API key：Plaid、Stripe、Bridge、Dinari、DeBank、Resend、logo.dev
- [ ] Privy app 最後再關：留到沒有人需要匯出私鑰為止
- [ ] 在 GitHub 把 repo 設為 Archived
