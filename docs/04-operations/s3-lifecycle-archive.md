# S3 生命週期與封存 SOP

## 1. 前置檢查
- [ ] 確認桶名稱與 Region
- [ ] 驗證 IAM 權限：`s3:PutLifecycleConfiguration`
- [ ] 與業務部門確認封存週期（例如：30 天 → IA，90 天 → Glacier）

## 2. 設定封存規則
1. 登入 AWS Console → S3 → 選擇 Bucket
2. 選單 → Management → Lifecycle rules → Create rule
3. 命名：`archive-rule-YYYYMMDD`
4. 條件：所有物件或指定 prefix
5. 動作：
   - 30 天 → 移至 **S3 Standard-IA**
   - 90 天 → 移至 **Glacier Flexible Retrieval**

## 3. 驗證
- [ ] 上傳測試檔案
- [ ] 手動調整日期（模擬檔案存放天數）
- [ ] 確認自動轉換 Storage Class 成功

## 4. 回滾
- 刪除 Lifecycle 規則
- 驗證 Bucket 回到預設行為
