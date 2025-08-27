# ALB / NLB 建立與設定指南

## 1. 前置檢查
- [ ] 確認流量需求（L4 or L7）
- [ ] 確認目標 VPC / Subnet
- [ ] 準備好 Target Group（EC2 / ECS / Lambda）

## 2. 建立 ALB
1. Console → EC2 → Load Balancers → Create Load Balancer
2. 選 Application Load Balancer
3. 配置：
   - Scheme: Internet-facing / Internal
   - Listener: HTTP 80 / HTTPS 443
   - Target group: `tg-<service-name>`

## 3. 建立 NLB
1. Console → EC2 → Load Balancers → Create Load Balancer
2. 選 Network Load Balancer
3. 配置：
   - Scheme: Internet-facing / Internal
   - Protocol: TCP
   - Target group: `tg-<service-name>`

## 4. 驗證
- [ ] `curl` 測試端點
- [ ] CloudWatch 指標：`HealthyHostCount` > 0
- [ ] 確認 ACM 憑證（HTTPS）

## 5. 回滾
- 刪除 ALB / NLB
- 移除 Target Group
