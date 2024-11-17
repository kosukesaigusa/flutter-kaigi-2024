---
marp: true
theme: base
_class: lead
paginate: true
backgroundColor: fff
backgroundImage: url('https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background.svg')
---

<style scoped>h2 {font-size: 36px; } h3 {font-size: 42px; }</style>

![bg left:40% 80%](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/kosukesaigusa.jpg)

## **FlutterKaigi 2024**

### マッチングアプリ『Omiai』の<br>Flutterへのリプレイスの挑戦

Kosuke Saigusa（株式会社 Omiai）

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# いくつか質問させてください！

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# Flutter での開発が好き！ 🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# Flutter で新規アプリの立ち上げをしたことがある 🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# Flutter で既存アプリをリプレイスしたことがある🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 皆さんの Flutter プロジェクトについて

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 開発体験や開発生産性に概ね満足している🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# アーキテクチャは概ね開発しやすい🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 負債が少なく開発しやすい🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# テストを十分に書けている🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 新規参画メンバーが活躍しやすい🙋

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 本セッションのゴール

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# 本セッションのゴール

<div class="center-goal-container">
  <h2>アーキテクチャを中心に<br>大規模アプリの Flutter へのリプレイスを<br>成功に導くアプローチを考える</h2>
  <p class="note">※ プロジェクト構成、採用するパッケージ、技術スタックなども含めて、<br>広義に「アーキテクチャ」と表現しています</p>
</div>

---

# 自己紹介

![bg right:40% 80%](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/kosukesaigusa.jpg)

- Kosuke Saigusa (@kosukesaigusa)
- 株式会社 Omiai Flutter テックリード
- FlutterNinjas 2024, FlutterKaigi 2023 等で登壇
- FlutterGakkai, 東京 Flutter ハッカソン運営

---

# Omiai について

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/omiai.png)

---

# 昨今のマッチングアプリと市場について

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/matching-service.png)

---

# エニトグループについて

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/enito-01.png)

---

# ホールディングス経営体制

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/enito-02.png)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# なぜ『Omiai』は Flutter にリプレイスするのか？

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# なぜ『Omiai』は Flutter にリプレイスするのか？

## 💬 背景

- 2012 年から長年運営されてきたサービス
- iOS・Android (, Web) の各プラットフォームの古いコードベースが抱える多くの技術的負債や仕様差異
- サーバサイドの負債の解消・仕様変更もクライアントアプリが理由で進めづらい

## ✅ 目標

- Flutter へのリプレイスによる単一コードベース化と技術的負債の返済
- リプレイス中も機能追加は止めない
- toC アプリとしてあるべき素早く・細かい開発サイクルでの開発・検証を実現する

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# どのようにしてリプレイスするのか？

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# どのようにしてリプレイスするのか？

## ✅ Flutter 経験の無いメンバーや新規参画メンバーが早期に活躍できる

- Flutter 経験者ゼロ・業務委託中心組織から正社員採用によるチームの拡大へ

## ✅ 負債化を繰り返さず、高い開発生産性を維持し続ける

- アーキテクチャ、CI、テスト、ドキュメンテーションなどで多くの工夫

## ✅ リプレイス中も機能追加は止めない

- Add-to-App を利用して既存アプリから Flutter をモジュールとして呼び出す
- 主要なユーザー体験から順に、新しい機能を追加しながら Flutter にリプレイスする

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 要件

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# 要件（一部抜粋）

## ✅ 開発スピード

- 〇〇 サイズの「典型的な実装タスク」は、N 日以内に実装を完了し、QA Ready の状態になる
- 新規参画メンバーが N 日以内にアーキテクチャを理解でき、タスクに着手できる
- 開発タスクの根拠をもった見積もりが可能になる

## ✅ 品質

- 規約に従った品質の高いコードをチーム全員で書くことができる
- 業務ロジック層と、それより抽象的な層全てで、ユニットテストがカバレッジ 100% で記述できる
- ソースコードが「実行可能な仕様書」として機能する

---

# 陥りがちな現実と理想

<div class="two-columns">

<div>

## ⛔ 陥りがちな現実

- 次第に負債が蓄積し、その悪影響が広く波及する
- 既存実装の影響箇所を隈なく探さないと、タスクの実装ができない
- 実装の正しさを判断する基準が曖昧で、迷う時間が長い
- コードの品質維持が熟練メンバーの属人的なレビューに頼っている

</div>

<div>

## ✅ 理想

- アーキテクチャにより負債化を未然に防ぎ、品質が維持される
- 影響範囲が明確で、本質的な実装に集中できる
- 実装の正しさを判断する基準が明確で、迷わず開発できる
- コードの品質維持が仕組みとして確立され、誰でも活躍できる

</div>

</div>

---

# 目標を達成するためのアプローチ

## ✅ アーキテクチャによる品質担保

- 誤った実装を防ぐガードレールをアーキテクチャで実現する
- 明確な境界と依存関係の制御により保守性を高める
  1. マルチパッケージ構成による責務の分離
  2. パッケージ間の依存関係の明確化と制御
  3. Flutter の世界・Dart の世界の明確な区別
  4. クライアントアプリとサーバサイドアプリの境界の明確な区別
- 「正しい実装」が自然と導かれる構造を目指す

