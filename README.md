# aws-study-cloudformation

## 概要
本リポジトリは、AWSマネジメントコンソール上で手動作成した
インフラ構成を、CloudFormation（YAML）で再現することを目的とした
学習用リポジトリです。

「コンソール操作で理解した構成を、Infrastructure as Code（IaC）として
定義できるようになる」ことをゴールとしています。

---

## 作成するインフラ構成

- VPC
  - 2AZ構成
  - パブリックサブネット ×2
  - プライベートサブネット ×2
- Internet Gateway
- ルートテーブル
- EC2
  - Amazon Linux 2023
  - Apache / PHP / Java（検証用途）
- RDS
  - MySQL
- Application Load Balancer（ALB）
  - ターゲットグループ
  - ヘルスチェック設定

---

## マネジメントコンソールで実施済みの内容

- VPC / サブネット / ルートテーブル作成
- EC2作成およびSSH接続確認
- RDS（MySQL）作成
- Spring Bootアプリのデプロイ
- ALB経由でのアクセス確認

---

## CloudFormationで再現する範囲

- VPC / Subnet / IGW / RouteTable
- Security Group
- EC2
- RDS
- ALB / Target Group / Listener

---

