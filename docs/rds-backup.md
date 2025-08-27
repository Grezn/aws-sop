# RDS 備份 SOP

- **目的**：建立資料庫 Snapshot 並定期驗證可還原性  
- **適用範圍**：生產與測試帳號（請依標籤過濾）  
- **角色與權限**：具備最小權限的 `rds:CreateDBSnapshot`, `rds:Describe*`, `ssm:GetParameter`

## 1. 前置檢查
- [ ] 已公告維護時段（如需）
- [ ] 目標實例狀態為 `available`
- [ ] 標籤 `Backup=Daily` 已配置
- [ ] 參數 `/{env}/rds/backup-window` 已存在（Parameter Store）

## 2. 操作步驟
1. 於 AWS Console 前往 **RDS → Databases**
2. 選擇目標實例，**Actions → Take snapshot**
3. 命名規則：`{env}-{db}-{YYYYmmddHHMM}`
4. 於 **Snapshots** 監看狀態為 `available`

::: info 可選：以 SSM 自動化
可將此流程改為 SSM Automation（自訂 Runbook），只需輸入 DB Instance ID 與環境標籤即可執行。
:::

## 3. 驗證
- [ ] 隨機抽樣 1 份 Snapshot 還原至測試 VPC/子網
- [ ] 應用可成功啟動並連線

## 4. 回滾方案
- 如備份作業影響服務，先**停止新備份**，保留近期 Snapshot，必要時以前一份可用 Snapshot 還原。

## 5. 記錄
- 變更單編號 / 審批人 / 執行人
- 執行時間（UTC+8）、耗時
- 產出物：Snapshot ID、CloudTrail/SSM 執行記錄連結