# Azure Monitor：メトリック監視（Storage）

**要点**  
Azure Storage アカウントは、プラットフォームメトリックを標準で自動収集しており、  
**追加課金なしでメトリック監視が可能**。  
仮想マシンや NIC を作成する必要はなく、無料環境で学習できる。

## ■ 除外した対象について

本ページは **無料環境で実施可能な範囲のみ** を扱うため、  
以下の監視対象は **Azure の仕様上、無料では実施不可** のため除外している。

### ● VM（仮想マシン）
- CPU / メモリ / ディスク IOPS などのメトリックは **VM が存在することが前提**  
- VM は従量課金のため、**無料環境では作成不可 → 監視不可**

### ● Network（NIC）
- ネットワークメトリック（Bytes / Packets）は **NIC のみに対して公開される**  
- NIC は VM 作成時に自動生成されるため、  
  **VM を作らない無料環境では NIC が存在せず → 監視不可**

## 1. 目的
Storage アカウントの状態を数値（メトリック）で継続的に把握し、  
性能・安定性・エラー傾向を確認できるようにする。

## 2. 設計（Storage のみ）

### 2-1 対象リソース
- **Storage アカウント（Blob / Queue / Table / File）**
  - Total Requests  
  - Success E2E Latency  
  - Server Latency  
  - Availability  
  - Server Errors / Client Errors  

### 2-2 監視の粒度
- 集計間隔：1 分 / 5 分 / 15 分  
- 表示期間：直近 1 時間 / 24 時間 / 7 日  
- 表示方法：折れ線グラフ（時系列）

## 3. 前提条件
- Azure サブスクリプション（無料試用／従量課金）  
- Storage アカウントが存在すること  
- 権限：Reader 以上  

## 4. 手順（GUI）

### 4-1 Azure ポータルで「監視」を開く
1. Azure ポータルにサインイン  
2. 左メニュー → 監視 → メトリック

<img src="../images/02-04-storage-01-open.png" width="300">

### 4-2 スコープで Storage アカウントを選択
1. メトリック画面上部の **スコープ** をクリック  
2. サブスクリプション → リソース グループ → **Storage アカウント** を選択  
3. **適用** をクリック

<img src="../images/02-04-storage-02-scope.png" width="300">

### 4-3 メトリックを選択して表示する
1. 「メトリック」ドロップダウンを開く  
2. 以下のメトリックを選択  
   - Total Requests  
   - Success E2E Latency  
   - Server Latency  
   - Availability  
   - Server Errors / Client Errors  
3. 集計方法（平均 / 合計 / 最大）を選択  
4. 時間範囲を設定（例：過去 24 時間）

<img src="../images/02-04-storage-03-metric-select.png" width="300">

### 4-4 複数メトリックの重ね合わせ
1. **メトリックの追加** をクリック  
2. 別のメトリックを選択  
3. 色分けや軸（左軸／右軸）を調整して見やすくする

<img src="../images/02-04-storage-04-multi-metric.png" width="300">

### 4-5 チャートの保存とダッシュボード化
1. グラフ右上の **ピン留め** をクリック  
2. ダッシュボードに追加  
3. ウィジェットのサイズや配置を調整

<img src="../images/02-04-storage-05-pin-dashboard.png" width="300">

### 4-6 アラートルールの作成（Storage のみで可能）
1. 左メニュー → **アラート**  
2. **アラート ルールの作成**  
3. シグナル → **メトリック**  
4. Storage のメトリックを選択  
5. 条件（閾値・演算子・期間）を設定  
6. 通知先（メールなど）を設定  
7. ルール名を付けて作成

<img src="../images/02-04-storage-06-alert-rule.png" width="300">

### 4-7 診断設定と Log Analytics（必要時）
※無料環境では **設定画面の確認のみ推奨**（取り込みは課金）
1. Storage アカウント → **診断設定**  
2. Log Analytics ワークスペースを選択  
3. 送信するログ種別を選択  
4. 保存期間・取り込み量に注意（課金対象）

<img src="../images/02-04-storage-07-diagnostic.png" width="300">

## 5. メトリック監視で得られる判断材料
- **要求数の急増** → アプリケーション負荷の増加  
- **レイテンシの上昇** → 設計見直し（アクセスパターン、冗長化）  
- **エラー数の増加** → アプリ側の例外処理やリトライ制御の確認  
- **可用性の低下** → Storage 側の問題、またはアプリ側のアクセス失敗  

## 6. 注意点
- Storage のプラットフォームメトリックは無料  
- Log Analytics への取り込みは課金  
- VM / NIC を必要としないため、無料環境で学習可能  

## 付録：よく使う Storage メトリック一覧
- Total Requests  
- Success E2E Latency  
- Server Latency  
- Availability  
- Server Errors / Client Errors
