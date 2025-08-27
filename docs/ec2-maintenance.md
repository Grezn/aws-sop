# EC2 維護 SOP（安全更新與重啟）

- **目的**：例行套用 OS 更新並在維護窗重啟
- **適用範圍**：標籤 `PatchGroup=Linux-Std` 的實例
- **角色與權限**：`ssm:SendCommand`, `ec2:DescribeInstances`

## 1. 前置檢查
- [ ] 維護窗公告完成
- [ ] 該實例已安裝 SSM Agent 並連線正常
- [ ] Auto Scaling 組合已設健康檢查（避免同時重啟所有節點）

## 2. 操作步驟
1. 以 SSM Patch Manager 套用安全更新（Baseline：`Linux-Security`）
2. 檢視補丁結果為 `Installed`
3. 於低峰期逐台執行重啟：
   ```bash
   aws ssm send-command      --document-name "AWS-RunShellScript"      --instance-ids i-xxxxxxxx      --parameters commands="sudo reboot"
   ```
4. 監控應用健康指標與 ALB Target 健康狀態

## 3. 驗證
- [ ] 版本與補丁清單符合基線要求
- [ ] 應用與監控恢復正常（CPU、錯誤率、延遲）

## 4. 回滾方案
- 將該實例自 ALB 暫時移出、還原 AMI 或前一版本快照，必要時縮容流量至健康節點。