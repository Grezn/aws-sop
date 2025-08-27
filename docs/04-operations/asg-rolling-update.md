# Auto Scaling Group 滾動更新 SOP（EC2）

> 目標：對 ASG 逐批替換到新 AMI / 啟動組態，期間維持服務可用。

## 0. 參數
- ASG 名稱：`<your-asg-name>`
- 新 AMI/Launch Template 版本：`<lt-xxx>:<version>`
- 最小健康實例數（目標容量的 %）：例如 70%

## 1. 前置檢查
- [ ] 新 AMI 已完成硬化與測試
- [ ] 新 Launch Template 已可啟動並通過健康檢查
- [ ] 負載均衡（ALB/NLB）健康檢查正常
- [ ] 維護窗公告完成

## 2. 調整 ASG 策略（可選）
- 將 `Max` 容量臨時提高 +1～+N，以便滾動時有緩衝。

## 3. 套用新版本
1. 更新 ASG 使用 **新的 Launch Template 版本**。
2. 啟用 **Instance Refresh**（建議）：
   - Console → Auto Scaling groups → Instance refresh → Start instance refresh
   - 設定：批次換新比例（如 20%）、健康檢查寬限期（如 300s）

## 4. 監控過程
- CloudWatch 指標：
  - `GroupInServiceInstances` 持續 ≥ 最小健康數
  - 應用延遲/錯誤率無異常
- 觀察 ALB Target `HealthyHostCount`。

## 5. 驗證
- [ ] 新實例版本正確（User data / 應用版本）
- [ ] 日誌/監控正常
- [ ] 流量平穩

## 6. 回滾
- 使用先前 Launch Template 版本重新觸發 Instance Refresh
- 必要時調降 Max / Desired 至穩定值

## 7. 記錄
- 變更單 / 時間 / 影響評估
- 產出物：新 AMI、LT 版本、Instance Refresh ID