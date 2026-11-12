# Kura 後端

> **已封存 — 不再營運與維護。**
> Kura 客戶端（[mobile-app](https://github.com/Kura-Finance/mobile-app)、[WebClients](https://github.com/Kura-Finance/WebClients)）現在預設直通鏈上，不需要這個後端；官方託管服務已關閉。本 repo 保持唯讀，供想自架後端功能的人使用。不接受 issue 與 pull request，依賴套件也不再更新，部署前請自行審查。

**Kura** 多資產理財應用的開源後端 API。

## 封存後自架

後端從未持有錢包金鑰，也不簽署交易；沒有它錢包照常運作。它提供的是鏈下功能：TrackFi（Plaid、交易所、DeBank）、Bridge.xyz 法幣出入金、Dinari 股票、Stripe 訂閱、推薦、卡片候補名單，以及 app 內刪除帳號。

- **把客戶端指向你的部署。** 行動端：`EXPO_PUBLIC_API_BASE_URL`；網頁端：`NEXT_PUBLIC_BACKEND_URL`。兩者預設留空，此時客戶端把錢包金鑰存在裝置上（行動端）或瀏覽器的密碼加密保管庫（網頁端），不使用這個後端，也不使用 Privy。填上後客戶端會改用 Privy 登入，並開啟後端功能。
- **只開你有金鑰的 domain**，見 [src/config/features.ts](src/config/features.ts)。
- **會員等級依賴 Stripe。** 網頁端 API（`webTierGate`）與 Treasury（`requirePaidTier`）需要 Pro 或 Ultimate，而只有 Stripe domain 會寫入 `User.tier`。沒有 Stripe 時所有人都是 Basic：網頁端大部分路由回 403，Treasury 在所有客戶端都無法使用。請自行設定等級，或放寬這兩個 middleware。
- **使用你自己的 Privy app。** `PRIVY_APP_ID`／`PRIVY_APP_SECRET` 必須與客戶端登入用的 app 相同。

技術棧：Node.js 24、TypeScript、Express 5、Prisma、PostgreSQL。可部署至 Google Cloud Run + Cloud SQL。

[English](README.md) · [貢獻指南](CONTRIBUTING.md)

## 授權

[MIT](LICENSE) · Copyright © 2024–2026 Rick Weng

第三方聲明：[docs/legal/THIRD_PARTY_NOTICES.zh-TW.md](docs/legal/THIRD_PARTY_NOTICES.zh-TW.md)

## 文件

完整索引：**[docs/README.zh-TW.md](docs/README.zh-TW.md)**

| 文件 | 說明 |
|------|------|
| [docs/PRODUCT.zh-TW.md](docs/PRODUCT.zh-TW.md) | 產品概述 |
| [docs/ARCHITECTURE.zh-TW.md](docs/ARCHITECTURE.zh-TW.md) | 網域、認證、資料流 |
| [docs/API.zh-TW.md](docs/API.zh-TW.md) | HTTP 掛載與存取規則 |
| [docs/ENVIRONMENT.zh-TW.md](docs/ENVIRONMENT.zh-TW.md) | 環境變數 |
| [.env.example](.env.example) | 環境變數範本 |
| [src/config/features.ts](src/config/features.ts) | Domain 功能開關 |
| [docs/API_KEYS.zh-TW.md](docs/API_KEYS.zh-TW.md) | 夥伴 Key vs 免費公用端點 |
| [docs/OPERATIONS.zh-TW.md](docs/OPERATIONS.zh-TW.md) | 本地／Docker／Cloud Run |
| [docs/SECURITY.zh-TW.md](docs/SECURITY.zh-TW.md) | 安全模型 |

## 快速開始

前置：Node.js 24+、PostgreSQL。

```bash
cp .env.example .env.development
# 在 src/config/features.ts 只開啟你有 Key 的 domain
# 填入 JWT_SECRET、ENCRYPTION_KEY、DB_*，以及已開啟功能所需夥伴金鑰

npm ci
npx prisma migrate deploy   # 或：npx prisma migrate dev
npm run dev
```

健康檢查：`GET http://localhost:8080/health` · 功能開關：`GET http://localhost:8080/api/features`

## Scripts

| 指令 | 用途 |
|------|------|
| `npm run dev` | 開發伺服器（`ts-node-dev`） |
| `npm run build` | 編譯 TypeScript 至 `dist/` |
| `npm start` | 執行編譯後伺服器 |
| `npm run dbml` | 重新產生 Prisma client／DBML |
