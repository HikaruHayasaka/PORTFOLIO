# Entra ID：ユーザー・グループ作成（GUI）

## 1. 目的
ID管理の基礎として、Entra ID 上でユーザーとグループを作成し、
適切に紐づける手順を理解する。

## 2. 設計（What）
- ユーザー  
  - Test.User-01
- グループ  
  - TestGroup-0l
- 紐づけ  
  - Test.User-01 → TestGroup-0l

## 3. 手順（How：GUI）
### 3-1. ユーザー作成
1. Microsoft Entra 管理センター  
2. Users → New user  
3. 必要項目を入力し作成  
（スクリーンショットを貼る）

### 3-2. グループ作成
1. Groups → New group  
2. Group type：Security  
3. 名前を入力し作成

---

<img src="../images/user-list.png" width="600">

---

### 3-3. グループへメンバー追加
1. Groups → 対象グループ  
2. Members → Add members  
3. 作成したユーザーを追加  

---

<img src="../images/group-list.png" width="600">

---

## 4. 結果（Output）
- ユーザー一覧（作成済み）  
- グループ一覧（作成済み）  
- グループのメンバー構成  

---

<img src="../images/group-members.png" width="600">

---

## 5. 学び（Insight）
- GUI操作でのユーザーとグループの作成方法  
- ユーザー → グループ の階層構造の理解
