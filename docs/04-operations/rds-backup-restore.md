# RDS 備份與還原 SOP

## 1. 前置檢查
- [ ] 確認 IAM 權限（至少具備 `rds:CreateDBSnapshot` 與 `rds:RestoreDBInstanceFromDBSnapshot`）
- [ ] 確認目標 RDS 狀態為 `available`
- [ ] 驗證備份儲存區域（快照是否在正確 Region）

## 2. 建立備份
1. 登入 AWS Console → RDS → Snapshots
2. 選擇目標資料庫，點擊「Take Snapshot」
3. 命名：`rds-<db-name>-backup-YYYYMMDD`

## 3. 還原備份
1. RDS → Snapshots → 選擇快照
2. 點擊「Restore Snapshot」
3. 指定新 DB Instance ID（避免覆蓋原本）
4. 選擇 VPC / Subnet / Security Group
5. 建立後確認狀態為 `available`

## 4. 驗證
- [ ] 登入新還原的 RDS 測試環境
- [ ] 驗證資料一致性（抽樣查詢）
- [ ] CloudWatch Logs 是否正常

## 5. 回滾
- 若還原失敗，刪除該快照並重試
- 通知 DBA 團隊進行人工檢查
