# Git 工作流程與版號規則

本文件定義 LumasharpChecker 後續功能開發、版本遞增及正式發行方式。它不追溯改寫既有歷史；舊版號僅作歷史記錄。

## 分支

- `main` 是 V3 整合主線，合併進來的內容應可建置，並附上適用的驗證結果。
- 每項工作使用短期分支，完成後透過 GitHub Pull Request 合併：
  - `feat/<area>-<change>`：新增功能，例如 `feat/inspection-station-source`。
  - `fix/<area>-<change>`：錯誤修正，例如 `fix/retest-clear-results`。
  - `docs/<area>-<change>`：文件，例如 `docs/release-workflow`。
  - `chore/<area>-<change>`：建置、依賴或維護工作。
- Codex 建立的工作分支使用 `codex/` 前綴，後面仍描述工作內容，例如 `codex/docs-git-workflow`。
- 分支名稱描述工作內容；`V2`、`V3` 是歷史產品／資料夾名稱，不作為新的長期功能分支。
- 不直接推送功能提交到 `main`，不對已推送的提交做 rebase、amend 或 force-push。緊急修正仍開 `fix/*` 分支並留下審查紀錄。
- V2 遠端是舊開發線的歷史位置；V3 功能只合併、推送至本倉庫的 V3 遠端。

## 提交訊息

使用 `type(scope): 描述`，一個提交聚焦一項可理解的功能或修正：

- `feat(inspection): add per-station image source`
- `fix(retest): clear previous results before rerun`
- `docs(version): document release policy`
- `chore(build): update release metadata`

`feat`、`fix` 會影響使用者功能；`docs`、`chore` 不能取代功能提交。描述用動詞說明變更結果，避免只寫「update」或「修正」。

## 產品版號與 .NET 版本欄位

使用 `MAJOR.MINOR.PATCH` 代表產品發行版號：

- `MAJOR`：不相容的大改動，例如既有配方、資料格式或操作流程不能沿用。
- `MINOR`：增加向下相容的功能。
- `PATCH`：修正錯誤，不改變既有使用方式。
- 本產品中，若 UI 重製改變操作人員的既有流程或配方使用方式，按 `MAJOR` 處理；純視覺調整不自動升主版。
- 驗收前候選版使用 `2.0.0-rc.1` 這類預發布後綴；正式版去掉後綴。

原始 .NET 8 程式為 `1.0.0.69`。若保留操作方式、只移植到 .NET Framework 4.7.2，按本規則記為產品版 `1.0.1`、Windows／組件版 `1.0.1.1`。目前介面與操作流程重製版是產品 `2.0.0`，Windows／組件版 `2.0.0.1`。

V2 開發專案曾把欄位設為 `2.0.0.1`，但該版只供內部開發，沒有交付產線。它不算正式發行；本規則將 `2.0.0.1` 指定給目前重製版作為第一個正式版候選。舊提交與歷史文件保留原狀並加註，不改寫其 Git 歷史。

版本欄位對照及後續例子：

| 變更 | 產品版號 | `AssemblyVersion` / `FileVersion` |
|---|---|---|
| 本次介面／流程重製候選版 | `2.0.0` | `2.0.0.1` |
| 新增相容功能 | `2.1.0` | `2.1.0.1` |
| 一般錯誤修正 | `2.0.1` | `2.0.1.1` |
| 不相容的大改版 | `3.0.0` | `3.0.0.1` |

`.NET Framework 4.7.2` 是建置目標，不因產品版號遞增而改變。`AssemblyVersion`、`FileVersion` 與產品版號用途不同；目前桌面主程式保持同步遞增。`InformationalVersion` 由 .NET SDK 產生，建置時有 Git 資訊便帶入來源提交識別，方便追查實際建置來源。

既有 `1.0.0.69`、V2 開發時的 `2.0.0.1`、V3 開發時的 `3.0.0.0` 沒有沿用一致規則。本政策從目前重製版開始整理正式產品版號；不把目錄名當成主版號，也不把未交付的 V2 開發編號當成正式發行。

## 發行與追溯

1. 功能經 Pull Request 審查，更新 `CHANGELOG.md` 並確認產品版號。
2. 合併到 `main` 後，從乾淨的提交建置 Release x64；執行相關自動化／人工驗證。涉及產線行為的變更須記錄線上或代表性機台驗收結果。
3. 驗收通過後，為剛才驗證的同一個提交建立標籤（例如 `v2.0.0`）及 GitHub Release，附上建置產物。
4. Release 說明記錄版號、提交雜湊、目標框架、主要功能與驗證範圍。標籤發布後不移動、不重用。

GitHub 的 `main` 分支保護應要求 Pull Request，並在套用前確認團隊有可用的建置／驗收檢查；本文件只定義流程，分支保護設定需由倉庫管理員另行啟用。

目前沒有替既有提交補打正式版標籤；先完成對應驗收，再發布第一個依此規則建立的標籤。
