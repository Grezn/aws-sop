# 災難復原 (DR) Runbook

## 1. 啟動條件
- 當主要 Region 無法運作超過 30 分鐘
- CloudWatch Alarm 觸發：核心服務 3 個以上無回應
- Incident Manager 發布 SEV-1

## 2. 執行步驟
1. 通知 DR 小組
2. 啟動預備 Region 的 VPC 與子系統
3. 從 Cross-Region Snapshot 建立 RDS
4. 啟動次要 ALB/NLB
5. Route53 Failover → 切換至次要 Region

## 3. 驗證
- [ ] 核心服務 API 可回應
- [ ] 前台頁面可登入
- [ ] 資料完整性抽樣比對

## 4. 回復正常
1. 確認主要 Region 恢復
2. 反向同步資料（RDS → Aurora Global DB）
3. Route53 切回主要 Region
4. 關閉次要資源，避免額外成本

## 5. 記錄
- 填寫 DR 報告
- 記錄復原耗時、影響範圍
