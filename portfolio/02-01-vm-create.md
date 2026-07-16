# Azure：仮想マシン作成（Azure Portal / GUI）

## 1. 目的
Azure Portal（日本語版）を使用して、仮想マシン（VM）を最新 UI で作成する一連の流れを理解する。

## 2. 設計（What）
- リソースグループ：`rg-vm-demo-01`
- リージョン：Japan East
- VM 名：`vm-win-01`
- OS イメージ：Windows Server 2022 Datacenter: Azure Edition
- サイズ：Standard_B2s
- 管理者アカウント：azureuser
- ネットワーク構成
  - VNet：`vnet-vm-demo-01`
  - Subnet：`subnet-vm-01`
  - NIC：`nic-vm-win-01`
  - Public IP：`pip-vm-win-01`
- 受信ポート：RDP(3389)

## 3. 手順（How）

### 3.1 リソースグループ作成

<img src="../images/02-01-azure-vm-01-rg-create.png" width="300">

1. Azure Portal にサインイン  
2. 左メニュー → 「リソース グループ」  
3. 「＋ 作成」  
4. 名前：`rg-vm-demo-01`、リージョン：Japan East  
5. 「確認および作成」→「作成」

### 3.2 仮想マシン作成（基本）

<img src="../images/02-01-azure-vm-02-basic.png" width="300">

1. 上部検索バー → 「仮想マシン」  
2. 「＋ 作成」→「仮想マシン」  
3. 基本タブ  
   - リソースグループ：`rg-vm-demo-01`  
   - VM 名：`vm-win-01`  
   - リージョン：Japan East  
   - イメージ：Windows Server 2022 Datacenter: Azure Edition  
   - サイズ：Standard_B2s  
4. 管理者アカウント  
   - ユーザー名：azureuser  
   - パスワード：複雑な 12 文字以上  
5. 受信ポート：RDP(3389)

### 3.3 ディスク

<img src="../images/02-01-azure-vm-03-disk.png" width="300">

- OS ディスク：Standard HDD  
- 「VM とともに削除」をオン

### 3.4 ネットワーク

<img src="../images/02-01-azure-vm-04-network.png" width="300">

- VNet：`vnet-vm-demo-01`  
- Subnet：`subnet-vm-01`  
- Public IP：`pip-vm-win-01`  
- NIC：自動作成  
- 削除時に関連リソースも削除をオン

### 3.5 管理・監視

<img src="../images/02-01-azure-vm-05-autoshutdown.png" width="300">

- 自動シャットダウン（UTC+09:00）  
- 監視は既定のまま

### 3.6 作成

<img src="../images/02-01-azure-vm-06-review.png" width="300">

- 「確認および作成」→「作成」  
- デプロイ完了後、「リソースに移動」

### 3.7 接続（RDP）

<img src="../images/02-01-azure-vm-07-rdp.png" width="300">

1. VM 概要 → 「接続」→ 「RDP」  
2. RDP ファイルをダウンロード  
3. ローカル PC で開く  
4. azureuser でサインイン

### 3.8 動作確認（任意：IIS）

<img src="../images/02-01-azure-vm-08-iis.png" width="300">

PowerShell（管理者）で次を実行：

Install-WindowsFeature -name Web-Server -IncludeManagementTools

ブラウザで Public IP を開き、IIS 初期ページを確認。

### 3.9 クリーンアップ

<img src="../images/02-01-azure-vm-09-rg-delete.png" width="300">

1. VM → リソースグループへ移動  
2. 「リソース グループの削除」  
3. `rg-vm-demo-01` と入力 → 削除

## 4. 結果（Output）
- Windows Server 2022 VM が Azure 上に作成  
- RDP 接続可能  
- ネットワーク・ディスク・IP が自動構成  
- リソースグループ単位で削除可能

## 5. 学び（Insight）
- VM は「ネットワーク・ディスク・IP」を含む複合リソース  
- 自動シャットダウンや削除設定は運用コスト削減に有効  
- GUI 操作だけで基盤構築の流れを理解できる