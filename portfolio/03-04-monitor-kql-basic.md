# KQL クエリ（基本）
## 1. 目的
Log Analytics ワークスペースで Kusto Query Language（KQL）の基本構文を理解し、ログデータを検索・抽出できるようにする。

## 2. 設計
- 使用サービス：Log Analytics ワークスペース
- 使用機能：ログ（Log）クエリ
- 学習範囲：検索、絞り込み、列抽出、並び替え

## 3. 手順（GUI）
### 3-1. Log Analytics ワークスペースのログを開く
1. Azure ポータルにサインインする。
2. 上部検索ウィンドウ → **Log Analytics ワークスペース** を検索して選択。
3. 対象のワークスペースを開く。
4. 左メニュー → **ログ** を選択する。
(クエリ ハブ画面が表示される場合は、右上の?ボタンで閉じる)
5. 右上に **簡易モード** が表示される場合は、下向き三角から **KQLモード** を選択する。

<img src="../images/03-04-kql-basic-01-log-screen.png" width="300">

### 3-3. 基本クエリの実行
Log Analytics には標準テーブルが含まれており、代表的な例として **Heartbeat** テーブルがある。

#### 3-3-1. データの確認（検索）
```
Heartbeat
```
テーブルの内容をすべて表示する。

#### 3-3-2. 行数の確認（count）
```
Heartbeat
| count
```
行数を確認する。

#### 3-3-3. 条件で絞り込み（where）
```
Heartbeat
| where Computer == "vm-win-01"
```
Computer 列が一致する行のみを表示する。

#### 3-3-4. 必要な列だけを抽出（project）
```
Heartbeat
| project TimeGenerated, Computer, Category
```
指定した列のみを抽出する。

#### 3-3-5. 並び替え（order by）
```
Heartbeat
| order by TimeGenerated desc
```
最新のデータが上に来るように並び替える。

### 3-4. クエリの保存
1. クエリ編集画面右上の **保存** を押す。
2. 任意の名前を入力し保存する。

<img src="../images/03-04-kql-basic-03-save-query.png" width="300">

### 補足：ストレージアカウント診断設定 → Log Analytics 送信手順
1. 左メニュー → ストレージアカウント → 対象のストレージアカウントを選択（例：`saalertdemo01`）
2. 左メニュー → **監視** → **診断設定**
3. **blob** を選択
4. **診断設定を追加** をクリック
5. 診断設定の名前：law-demo-01
6. 以下の項目にチェックを入れる
- **Storage Read**  
- **Storage Write**  
- **Storage Delete**
- **Log Analytics ワークスペースへの送信**
7. 保存をクリック

## 4. 結果
- Log Analytics ワークスペースで基本的な KQL クエリを実行できた。
- 検索、絞り込み、列抽出、並び替えの基本構文を理解した。
- クエリの保存方法を習得した。

## 5. 学び
- KQL はログ分析に特化した構文であり、パイプ（`|`）で処理をつなげていく形式が特徴である。
- 基本構文を組み合わせることで、必要なログ情報を効率的に抽出できる。