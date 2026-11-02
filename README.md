# LumasharpChecker

V3 開發主線，面向 Windows x64 產線環境。使用 WPF 與 .NET Framework 4.7.2，執行時需搭配公司授權的 R3 原廠元件。

## 目前基線

- 產品版本候選：`2.0.0`（Windows 檔案／.NET 組件版本：`2.0.0.1`）。
- 目標框架：`.NET Framework 4.7.2`，與產品版號分開管理。
- 本倉庫沿用 V3 開發目錄名稱；產品版號依功能相容性決定，不依目錄名稱進版。
- Git 主線：`main`。正式發行以 `vX.Y.Z` 標籤指向驗收完成的提交；未標籤的 `main` 仍是開發狀態。
- 目前程式包含多站光源檢測、站別影像來源與張數設定、共用報表日期更新、標準件影像擷取及重測結果重置。細項見 [CHANGELOG.md](CHANGELOG.md)。

版本遞增、分支命名、提交與發行流程見 [GIT_WORKFLOW.md](GIT_WORKFLOW.md)。GitHub Pull Request 請填寫 [.github/PULL_REQUEST_TEMPLATE.md](.github/PULL_REQUEST_TEMPLATE.md)。

## 建置與啟動

先關閉 v3，再執行：

```powershell
powershell -ExecutionPolicy Bypass -File .\建置v3.ps1
```

完成後以 `啟動v3.vbs` 開啟 `App/LumasharpChecker.exe`。建置與執行需要本機 `App/NativeR3` 原廠元件；配方、圖庫及站別設定保存在 App 目錄。`App/`、原廠二進位與現場資料不納入 Git，請勿將它們加入提交。

## 歷史資料

`README-v2.md` 保留 V2 開發與驗證紀錄。V2 是歷史基底；目前功能開發以本倉庫 `main` 為準。既有 Git 提交歷史會保留，不為了整理提交訊息而重寫已推送的紀錄。
