# aws-study-cloudformation

## 概要
本リポジトリは、AWSマネジメントコンソール上で手動作成したインフラ構成を、CloudFormation（YAML）で再現することを目的とした学習用リポジトリです。

「コンソール操作で理解した構成を、Infrastructure as Code（IaC）として定義できるようになる」こと、および「セキュリティや監視を含めた実践的な構成をコード化する」ことをゴールとしています。

---

## 作成するインフラ構成

本テンプレートでは、Web 3層アーキテクチャをベースに、セキュリティと監視機能を付加した構成を作成します。

- **Network**
  - VPC (10.0.0.0/16)
  - 2AZ構成 (ap-northeast-1a / 1c)
  - パブリックサブネット ×2 (ALB, EC2配置)
  - プライベートサブネット ×2 (RDS配置)
  - Internet Gateway
  - ルートテーブル
  - S3 VPC Endpoint (Gateway型)

- **Compute**
  - EC2 (Amazon Linux 2023 / t3.micro)
  - Security Group (ALBからのHTTP、特定IPからのSSHのみ許可)

- **Database**
  - RDS for MySQL (8.0.43 / db.t4g.micro)
  - Security Group (EC2からのアクセスのみ許可)

- **Load Balancing**
  - Application Load Balancer (ALB) - Internet facing
  - Target Group (ヘルスチェック設定済み)

- **Security (WAF)**
  - AWS WAF v2 (ALBに関連付け)
  - AWS Managed Rules (CommonRuleSet)
  - WAF Logging (CloudWatch Logsへ出力)

- **Monitoring & Alerting**
  - CloudWatch Alarms
    - EC2 CPU使用率監視 (10%超過で発報)
    - WAF Blockリクエスト監視
  - SNS Topic & Subscription (Email通知)

---

## マネジメントコンソールで実施済みの内容（参考）

本コード化にあたり、事前にコンソール上で以下の操作・検証を実施済みです。

- VPC / サブネット / ルートテーブル作成
- EC2作成およびSSH接続確認
- RDS（MySQL）作成
- Spring Bootアプリのデプロイ
- ALB経由でのアクセス確認

---

## CloudFormationで再現する範囲

本リポジトリのテンプレート (`template.yaml`) は以下のリソースを自動構築します。

1. **ネットワーク基盤**
   - VPC, Subnet, IGW, RouteTable, Route
   - S3 VPC Endpoint

2. **セキュリティ**
   - Security Group (ALB用, EC2用, RDS用)
   - WAF WebACL, WAF Association, WAF Logging Config

3. **コンピュート & データベース**
   - EC2 Instance
   - RDS Instance, DB Subnet Group

4. **ロードバランサー**
   - ALB, Listener, Target Group

5. **監視・通知**
   - SNS Topic, SNS Subscription
   - CloudWatch Alarm (EC2 CPU, WAF Blocked)
   - CloudWatch Logs Group (WAFログ用)

---

## パラメータについて

デプロイ時に以下のパラメータを指定可能です。

- `AlertEmail`: アラート通知を受け取るメールアドレス (Default設定あり)