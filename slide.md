---
marp: true
theme: base
_class: lead
paginate: true
backgroundColor: fff
backgroundImage: url('https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background.svg')
footer: @kosukesaigusa
---

<style scoped>h2 {font-size: 36px; } h3 {font-size: 42px; }</style>

![bg left:40% 80%](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/kosukesaigusa.jpg)

## **FlutterKaigi 2024**

### マッチングアプリ『Omiai』の<br>Flutterへのリプレイスの挑戦

Kosuke Saigusa（株式会社 Omiai）

---

# 自己紹介

- Kosuke Saigusa (@kosukesaigusa)
- 株式会社 Omiai Flutter テックリード

---

# Omiai について

- このスライドでは会社とサービスの説明をする

---

<!-- 中央にタイトルを表示するスライド -->

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# なぜ『Omiai』は Flutter にリプレイスするのか？

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# なぜ『Omiai』は Flutter にリプレイスするのか？

## 💬 背景

- 2012 年から長年運営されてきたサービス
- iOS・Android (, Web) の各プラットフォームの古いコードベースが抱える多くの技術的負債や仕様差異
- サーバサイドの負債の解消、仕様変更もクライアントアプリが理由で進めづらい

## ✅ 目標

- Flutter へのリプレイスによる単一コードベース化と技術的負債の返済
- リプレイス中も機能追加は止めない
- toC アプリとしてあるべき素早く・細かい開発サイクルでの開発・検証を実現する

---

<!-- 中央にタイトルを表示するスライド -->

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# どのようにしてリプレイスするのか？

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# どのようにしてリプレイスするのか？

## ✅ リプレイス中も機能追加は止めない

- Add-to-App を利用して既存アプリから Flutter をモジュールとして呼び出す
- 主要なユーザー体験から順に、新しい機能を追加しながら Flutter にリプレイスする

---

# どのようにしてリプレイスするのか？

## ✅ 負債化を繰り返さず、できるだけ長く高い開発生産性を保つ

- アーキテクチャの工夫
  - サーバサイドの負債をクライアントアプリに持ち込まないためのレイヤー分離
  - 負債化しがちな誤った依存や実装を静的解析のレベルで防ぐ
- CI やコード規約の工夫
  - 業務ロジック層、および抽象的な層のユニットテストのカバレッジ 100% を強制する
  - ドキュメンテーションコメントを 100% 記述する
  - custom_lint および関連パッケージの導入

---

# どのようにしてリプレイスするのか？

## ✅ Flutter 経験の無いメンバーや新規参画メンバーが早期に活躍できる環境づくり

- コード規約およびスニペットの充実
- Flutter の世界と Dart の世界を切り分けた開発
- 開発タスクを再現性のある形で分解し、見積もり精度を高める工夫
