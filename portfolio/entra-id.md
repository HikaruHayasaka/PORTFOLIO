# **Entra ID：ユーザー・グループ・ロール管理（GUI）**

## 1. 目的
Entra ID 上で以下の 4 要素を一連の流れとして理解する。  
- ユーザー作成  
- グループ作成  
- ユーザーのグループ紐づけ  
- ロール（権限）付与  
---

## 2. 設計（What）
- ユーザー  
  - Test.User-01
- グループ  
  - TestGroup-01
- 紐づけ  
  - Test.User-01 → TestGroup-01
- ロール
  - Test.User-01 → User Administrator
---

## 3. 手順（How：GUI）
### 3-1. ユーザー作成
1. Microsoft Entra 管理センターへアクセス
2. **Users → New user**を選択 
3. 必要項目を入力し作成する
---

<img src="../images/user-list.png" width="600">

---
### 3-2. グループ作成
1. **Groups → New group** を選択  
2. **Group type：Security** を選択  
3. 名前を入力し作成する
---

<img src="../images/group-list.png" width="600">

---
### 3-3. グループへメンバー追加
1. **Groups → 対象グループ** を選択  
2. **Members → Add members** を選択  
3. 作成したユーザーを追加する
---

<img src="../images/group-members.png" width="600">

---
### 3-4. ロール割り当て
1. Microsoft Entra 管理センターへアクセス  
2. **Users → 対象ユーザー** を選択  
3. **Assigned roles** を選択  
4. **Add assignments** を選択  
5. 付与するロール（例：User Administrator）を選択し追加する
---

<img src="../images/assigned-roles.png" width="600">

---
## 5. 結果（Output）
- ユーザーが作成されている  
- グループが作成されている  
- グループにユーザーが所属している  
- ユーザーにロールが付与されている

## 6. 学び（Insight）
- GUI操作によるユーザー／グループ管理の流れを理解した  
- ユーザー → グループ → ロール の階層構造を把握した  
- ID管理の基礎 4 要素を一連の操作として整理できた  
