# **NSG / パブリック IP：IIS 公開確認（GUI）**

## **1. 目的**
Azure 仮想マシンに IIS を構成し、  
NSG（Network Security Group）とパブリック IP を用いて外部公開できる状態を確認する。  
ブラウザからパブリック IP を開き、IIS 初期ページが表示されることを証拠として残す。

## **2. 設計（What）**

### **2.1 リソース**
- 仮想マシン  
  - OS：Windows Server  
  - 役割：IIS（Web サーバー）
- ネットワーク  
  - NSG：HTTP(80) を許可  
  - パブリック IP：静的割り当て

### **2.2 構成図（画像挿入）**
`images/nsg-publicip-overview.png`  
![NSG / Public IP 構成概要](images/nsg-publicip-overview.png)

### **2.3 環境情報（Subscription / Resource Group / Region）**
- **サブスクリプション**：Azure-Subscription-Demo（※環境に合わせて変更）
- **リソースグループ**：`rg-vm-demo-01`
- **名前**：`vm-win-01`
- **リージョン**：Japan East

## **3. 手順（How）**

### **3.0 リソースグループの作成**
1. Azure ポータル
2. リソース グループ
3. 「+ 作成」をクリック  
3. 以下を設定  
   - **リソースグループ名**：`rg-vm-demo-01`  
   - **リージョン**：(Asia Pacific)Japan East  
4. 「レビューと作成」→「作成」

`images/rg-create.png`  
![リソースグループ作成](images/rg-create.png)

### **3.1 NSG の作成**
1. **仮想ネットワーク → ネットワーク セキュリティ グループ**
2. **+ 作成**から NSG を新規作成
- サブスクリプション：Azure-Subscription 1
- リソースグループ：rg-vm-demo-01
- 名前：vm-win-01
- リージョン：Japan East
3. 「レビューと作成」→「作成」

`images/nsg-create.png`  
![NSG 作成画面](images/nsg-create.png)

### **3.2 HTTP(80) の受信規則追加**
1. 「NSG」 →「対象の仮想ネットワーク」を選択
2. 左メニュー → 設定 → 受信セキュリティ規則  
3. 「+ 追加」  
4. 以下の設定を入力 → 追加
- ソース：Any
- ソース ポート範囲：*  
- 宛先：Any  
- サービス：Custom  
- 宛先ポート範囲：80  
- プロトコル：TCP  
- アクション：許可  
- 優先度：100  
- 名前：Allow-HTTP-80  
- 説明：任意（空欄で可）

`images/nsg-rule-http80.png`  
![NSG HTTP 受信規則](images/nsg-rule-http80.png)

### 3.3 仮想ネットワークの作成
1. 概要 → 仮想ネットワーク
2. 「+ 作成」をクリック  
3. 以下を設定  
   - **サブスクリプション**：`Azure subscription 1`
   - **リソースグループ**：`rg-vm-demo-01`
   - **仮想ネットワーク名**：`vnet-demo-01`  
   - **リージョン**：(Asia Pacific)Japan East
   4. 「アドレス空間」タブで、サブネット（`default`）が作成されていることを確認  
5. 「レビューと作成」→「作成」

画像を入れる

### **3.4 サブネットの関連付け**
1. 「NSG」 →「対象の仮想ネットワーク」を選択
2. 左メニュー → 設定 → サブネット
3. ＋関連付け
- 仮想ネットワーク：vnet-demo-01(rg-vm-demo-01)
- サブネット：defalt
4. OKをクリック

`images/nsg-associate.png`  
![NSG 関連付け](images/nsg-associate.png)

---

### **3.5 パブリック IP の作成**
1. 「ネットワーク基盤」→「パブリック IP アドレス」
2. 「+ 作成」をクリック
3. 以下の項目を設定する
   - **リソースグループ**：`rg-vm-demo-01`
   - **名前**：`pip-vm-win-01`
   - **リージョン**：(Asia Pacific)Japan East
   - **SKU**：Standard（※画像 UI に合わせる）
   - **IP アドレスの割り当て**：静的
4. その他の項目は既定値のままで問題なし  
   - IP バージョン：IPv4  
   - 可用性ゾーン：既定  
   - レベル：Regional  
   - ルーティングの優先順位：Microsoft ネットワーク  
   - アイドルタイムアウト：4  
   - DNS 名ラベル：任意（空欄で可）  
   - ドメイン名ラベルのスコープ：なし
5. 「レビューと作成」→「作成」

`images/publicip-create.png`  
![パブリック IP 作成](images/publicip-create.png)

---

**ここで、仮想マシンが無いと作れないことが判明。中断**


### **3.5 VM へのパブリック IP 割り当て**
1. 仮想マシン →「ネットワーク」  
2. NIC の「IP 構成」を開く  
3. 「パブリック IP アドレス」→ 作成済みの IP を選択  
4. 保存

`images/vm-attach-publicip.png`  
![VM へのパブリック IP 割り当て](images/vm-attach-publicip.png)

---

### **3.6 IIS 初期ページの表示確認**
1. 仮想マシンの「概要」でパブリック IP を確認  
2. ブラウザで `http://<パブリックIP>` を入力  
3. IIS 初期ページが表示されることを確認

`images/iis-default-page.png`  
![ブラウザで確認した IIS 初期ページ](images/iis-default-page.png)

---

## **4. 結果（Result）**
- パブリック IP にアクセスすると IIS 初期ページが正常に表示された  
- NSG の受信規則（HTTP 80）が正しく適用されていることを確認  
- VM の NIC に静的パブリック IP が割り当てられていることを確認  
- Ping は応答しないが、HTTP は正常に通過することを確認（Azure の既定仕様）

---

## **5. 学び（Learn）**
- **NSG は作成するだけでは機能しない**  
  → サブネットまたは NIC への関連付けが必須  
- **IIS 公開は 80 番ポートの許可だけで成立する**  
  → 443 を開ける必要はない  
- **パブリック IP は静的推奨**  
  → 再起動で IP が変わると検証結果が不安定になる  
- **Azure の Ping は既定で拒否される**  
  → HTTP の疎通確認が正しい検証方法  
- **構成図があると理解が速い**  
  → ポートフォリオの説得力が大きく向上する

---

## **6. README からのリンク例**
```markdown
### NSG / パブリック IP（IIS 公開確認）
[![NSG / Public IP](images/nsg-publicip-thumbnail.png)](/portfolio/nsg-publicip.md)
