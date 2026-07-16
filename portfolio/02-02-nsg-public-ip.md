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

## **3. 手順（How）**

### **3.1 NSG の作成**

1. Azure ポータル →「ネットワーク セキュリティ グループ」  
2. 「+ 作成」  
3. 名前・リージョンを指定して作成

`images/nsg-create.png`  
![NSG 作成画面](images/nsg-create.png)

### **3.2 HTTP(80) の受信規則追加**

1. NSG →「受信セキュリティ規則」  
2. 「+ 追加」  
3. 以下の設定で作成  
   - ソース：Any  
   - 宛先：Any  
   - ポート：80  
   - プロトコル：TCP  
   - アクション：許可  
   - 優先度：100〜300  
   - 名前：Allow-HTTP-80

`images/nsg-rule-http80.png`  
![NSG HTTP 受信規則](images/nsg-rule-http80.png)

### **3.3 NSG の関連付け**

- NSG →「サブネット」または「ネットワーク インターフェイス」  
- 対象 VM の NIC またはサブネットに関連付ける

`images/nsg-associate.png`  
![NSG 関連付け](images/nsg-associate.png)

### **3.4 パブリック IP の作成**

1. 「パブリック IP アドレス」  
2. 「+ 作成」  
3. 名前、SKU、割り当て（静的）を設定して作成

`images/publicip-create.png`  
![パブリック IP 作成](images/publicip-create.png)

### **3.5 VM へのパブリック IP 割り当て**

1. VM →「ネットワーク」  
2. NIC の「IP 構成」  
3. パブリック IP を選択して保存

`images/vm-attach-publicip.png`  
![VM へのパブリック IP 割り当て](images/vm-attach-publicip.png)

### **3.6 IIS 初期ページの表示確認**

1. VM の「概要」でパブリック IP を確認  
2. ブラウザで `http://<パブリックIP>` を入力  
3. IIS 初期ページが表示されることを確認

`images/iis-default-page.png`  
![ブラウザで確認した IIS 初期ページ](images/iis-default-page.png)

## **4. 結果（Result）**

- パブリック IP にアクセスすると、IIS 初期ページが正常に表示された  
- NSG の受信規則（HTTP 80）が正しく適用されていることを確認  
- VM の NIC に静的パブリック IP が割り当てられていることを確認  
- Ping は応答しないが、HTTP は正常に通過することを確認（Azure の既定仕様）

## **5. 学び（Learn）**

- **NSG は作成するだけでは機能しない**  
  → NIC またはサブネットへの関連付けが必須  
- **IIS 公開は 80 番ポートの許可だけで成立する**  
  → 443 を開ける必要はない  
- **パブリック IP は静的推奨**  
  → 再起動で IP が変わると検証結果が不安定になる  
- **Azure の Ping は既定で拒否される**  
  → HTTP の疎通確認が正しい検証方法  
- **ネットワーク構成図があると理解が速い**  
  → ポートフォリオの説得力が大きく向上する

## **6. README からのリンク例**

```markdown
### NSG / パブリック IP（IIS 公開確認）
[![NSG / Public IP](images/nsg-publicip-thumbnail.png)](/portfolio/nsg-publicip.md)
