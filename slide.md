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

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/self-introduction.svg)

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

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/enito-01.svg)

---

# ホールディングス経営体制

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/enito-02.svg)

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

# どのようにリプレイスするのか？

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# どのようにリプレイスするのか？

## ✅ Flutter 経験の無いメンバーや新規参画メンバーが早期に活躍できる

- Flutter 経験者ゼロ・業務委託中心組織から正社員採用によるチームの拡大へ

## ✅ 負債化を繰り返さず、高い開発生産性を維持し続ける

- アーキテクチャ、CI、テスト、ドキュメンテーションなどで多くの工夫

## ✅ リプレイス中も機能追加は止めない

- Add-to-App を利用して既存アプリから Flutter をモジュールとして呼び出す
- 主要なユーザー体験から順に、新しい機能を追加しながら Flutter にリプレイスする

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

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# パッケージ一覧

各パッケージに許される直接的な依存は矢印の先のパッケージのみに限定される。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-all.svg)

---

# パッケージ一覧 (app)

Flutter アプリのエントリポイントとしての実装、また、プレゼンテーション層としてのユーザー体験を実装する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-app.svg)

---

# パッケージ一覧 (base_ui)

共通利用される テキストスタイル、色、画像などの基礎的・原始的な UI 部品、それらを組み合わせた共通の UI 部品などを実装する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-base-ui.svg)

---

# パッケージ一覧 (domain)

業務知識や業務ロジックを記述する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-domain.svg)

---

# パッケージ一覧 (repository)

データソース（自社の API サーバーやローカルストレージなど）とのやり取りを記述する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-repository.svg)

---

# パッケージ一覧 (system)

3rd パーティのツールをラップして基礎的な実装を記述する（例：HTTP クライアント、Shared Preferences, Firebase Analytics など）。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-system.svg)

---

# パッケージ一覧 (dependency_provider)

Unimplemented なインターフェースのみを定義する。アプリ実行時、各パッケージのユニットテスト時に振る舞いを上書きする。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-dependency-provider.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 各パッケージの実装内容

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# base_ui

共通利用される テキストスタイル、色、画像などの基礎的・原始的な UI 部品、それらを組み合わせた共通の UI 部品などを実装する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-base-ui.svg)

---

# base_ui パッケージが依存するパッケージ

- Flutter への依存: direct
- 依存するパッケージの例：
  - extended_image
  - flutter_svg
  - flutter_gen

---

# base_ui の実装例

- Figma と同名で定義された TextStyle, Color などのスタイルガイド

```dart
/// アプリで使用する色一覧。
abstract interface class AppColor {
  /// プライマリーの 01 の赤色。
  static const Color primary01 = Color(0xFFFF3159);

  /// プライマリーの 02 の青色。
  static const Color primary02 = Color(0xFF3F9AD1);

  /** 省略 */

  /// セカンダリーの 01 のピンク色。
  static const Color secondary01 = Color(0xFFFE8D8B);

  /** 省略 */
}
```

---

# base_ui の実装例

- ボタン、画像、AppBar、TextField などのコンポーネントやそれらの組み合わせ
- Figma のコンポーネント定義と一致するインターフェース

```dart
/// アプリ内で共通で用いられる [ElevatedButton].
class CommonElevatedButton extends StatelessWidget {
  /// S サイズの [CommonElevatedButton] を生成する。
  const CommonElevatedButton.s({/** 省略 */});

  /// S サイズのアイコン付きの [CommonElevatedButton] を生成する。
  const CommonElevatedButton.sWithIcon({/** 省略 */});

  /** 省略 */

  @override
  Widget build(BuildContext context) {/** 省略 */}
}
```

---

# system

3rd パーティのツールをラップして基礎的な実装を記述する。外部パッケージへの依存を閉じ込めて、外部に表出させない。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-system.svg)

---

# system パッケージが依存するパッケージ

- Flutter への依存: transitive
- 依存するパッケージの例：
  - dio
  - firebase_analytics
  - shared_preferences

---

# system の実装例

- `HTTPClient` クラスの例（一部省略・簡略化）
- 内部で dio パッケージへ依存

```dart
class HttpClient {
  HttpClient({required String baseUrl, Map<String, dynamic>? headers})
      : _client = DioHttpClient(baseUrl: baseUrl, headers: headers);

  Future<HttpResponse<dynamic>> request({
    required HttpMethod method,
    required String path,
    Map<String, dynamic>? queryParameters,
    Object? data,
    Map<String, dynamic>? headers,
  }) async {
    /** 省略 */
  }
}
```

---

# system の実装例

- `HTTPClient` クラスが返す `HttpResponse` クラスの例（一部省略・簡略化）
- Result 型のような形で HTTP リクエストの成功・失敗を表現

```dart
@freezed
sealed class HttpResponse<T> with _$HttpResponse<T> {
  const factory HttpResponse.success({
    required T data,
    required Map<String, List<String>> headers,
  }) = SuccessHttpResponse<T>;

  const factory HttpResponse.failure({
    T? data,
    required Object e,
    required ErrorStatus status,
  }) = FailureHttpResponse<T>;
}
```

---

# system の実装例

- 3rd パーティーのパッケージへの依存は例外も含めて外部には表出させない
- 不要な機能のインターフェースは塞いでシンプルにする

```dart
class HttpClient {
  /** 省略 */

  Future<HttpResponse<dynamic>> request({/** 省略 */}) async {
    try {
      final response = await _client.request<dynamic>(/** 省略 */);
      return HttpResponse.success(/** 省略 */);
    } on DioException catch (e) {
      /** 省略 */
      return HttpResponse.failure(/** 省略 */);
    }
  }
}
```

---

# repository

データソース（自社の API サーバーやローカルストレージなど）とのやり取りを記述する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-repository.svg)

---

# repository パッケージが依存するパッケージ

- Flutter への依存: transitive
- 依存するパッケージの例：
  - system
  - riverpod

---

# repository の実装例

- `HttpResponse` の成功・失敗を switch 文で分岐して、`RepositoryResult` を返す。

```dart
class AccountDetailRepository {
  /** 省略 */

  Future<RepositoryResult<AccountDetailDto>> fetchAccountDetail() async {
    final response = await _ref.read(omiaiHttpRequestHelperProvider).request(/** 省略 */);
    switch (response) {
      case SuccessHttpResponse(:final data):
        final dto = AccountDetailDto.fromJson(data as Map<String, dynamic>);
        return RepositoryResult.success(dto);
      case FailureHttpResponse(/** 省略 */):
        /** 省略 */
        return RepositoryResult.failure(/** 省略 */);
    }
  }
}
```

---

# repository の実装例

- Dto として HTTP レスポンスのデータを定義する
- 不適切なフィールド名や型を変換して、サーバサイドの負債を持ち込まない

```dart
@freezed
class FooDto with _$FooDto {
  const factory FooDto({
    @JsonKey(name: 'inappropriate_field_name') required String someFieldName,
    @Default([]) List<String> things,
    @flexibleBoolConverter required bool isSomething,
    @flexibleDateTimeConverter required DateTime someDateTime,
  }) = _FooDto;

  factory FooDto.fromJson(Map<String, dynamic> json) => _$FooDtoFromJson(json);
}
```

---

# domain

業務知識や業務ロジックを記述する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-domain.svg)

---

# domain パッケージが依存するパッケージ

- Flutter への依存: transitive
- 依存するパッケージの例：
  - repository
  - riverpod

---

# domain の実装例

- マッチングアプリにおけるお相手メンバーの業務概念の定義の例
- HTTP レスポンスに対応する Dto からエンティティを生成する

```dart
@freezed
class Member with _$Member {
  const factory Member({
    /** 省略 */
    required LogInStatus loginStatus,
  }) = _Member;

  factory Member.fromDto(MemberDto dto) => Member(
    /** 省略 */
    loginStatus: LogInStatus.fromDateTime(dto.lastAccessedAt),
  );
}
```

---

# domain の実装例

- お相手メンバーのログインステータスの定義の例
- 最終ログイン日時という事実から、ログインステータス（直近どのくらいアクティブか）という業務概念を計算する

```dart
enum LogInStatus {
  active,

  recent,

  inactive,
  ;

  factory LogInStatus.fromDateTime(DateTime lastAccessedAt) {/** 省略 */}
}
```

---

# domain の実装例

- お相手メンバーの詳細を取得するロジックの例
- riverpod の関数で取得処理を記述する
- repository による取得結果を switch 文で分岐して処理する

```dart
@riverpod
Future<Member> memberDetail(MemberDetailRef ref, {required int userId}) async {
  final result = await ref.read(memberRepositoryProvider).fetchMember(userId);
  switch (result) {
    case SuccessRepositoryResult(:final data):
      return Member.fromDto(data);
    case FailureRepositoryResult(/** 省略 */):
      /** 省略 */
  }
}
```

---

# domain の実装例

- アカウント詳細の状態管理（取得と更新を行うロジック）の例
- riverpod のクラス (Notifier) で状態管理を記述する

```dart
@Riverpod(keepAlive: true)
class AccountDetailNotifier extends _$AccountDetailNotifier {
  /** 省略 */

  Future<void> updateAccountDetail() async {
    final result = await ref.read(accountDetailRepositoryProvider).updateAccountDetail(/** 省略 */);
    switch (result) {
      case SuccessRepositoryResult(/** 省略 */):
        /** 省略 */
      case FailureRepositoryResult(/** 省略 */):
        /** 省略 */
    }
  }
}
```

---

# domain の実装例

- お相手に「いいね！」を送信するロジック（状態管理を伴わない）の例
- 業務ロジックとして取り扱うべき例外を定義し、スローする

```dart
class SendLikeUseCase {
  /** 省略 */

  Future<void> invoke(/** 省略 */) async {
    final result = await _ref.read(interestRepositoryProvider).sendInterest(/** 省略 */);
    switch (result) {
        /** 省略 */
      case FailureRepositoryResult(/** 省略 */):
        /** 省略 */
        throw InsufficientPointsToSendLikeException();
    }
  }
}

class InsufficientPointsToSendLikeException implements Exception {}
```

---

# app

Flutter アプリのエントリポイントとしての実装、また、プレゼンテーション層としてのユーザー体験を実装する。

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-app.svg)

---

# app パッケージが依存するパッケージ

- Flutter への依存: direct
- 依存するパッケージの例：
  - domain
  - base_ui
  - auto_route
  - hooks_riverpod

---

# app の実装例

- auto_route パッケージを利用してルート定義

```dart
@RoutePage()
class MemberProfilePage extends StatelessWidget {
  const MemberProfilePage({
    @PathParam('userId') required this.userId,
    super.key,
  });

  static String resolvePath({required String userId}) => '/users/$userId';

  final int userId;

  @override
  Widget build(BuildContext context) {/** 省略 */}
}
```

---

# app の実装例

- domain 層の取得ロジックを `ref.watch` してユーザーが目にする体験を実装する

```dart
@override
Widget build(BuildContext context, WidgetRef ref) {
  // メンバー情報を取得する。
  final memberAsyncValue = ref.watch(memberDetailProvider(userId: userId));
  return switch (memberAsyncValue) {
    AsyncData(value: final member) => /** 省略 */,
    AsyncError() => /** 省略 */,
    _ => /** 省略 */,
  };
}
```

---

# app の実装例

- UI を通じたインタラクションをトリガーに domain 層のロジックを呼び出す
- domain 層に定義された例外を実現したいユーザー体験に従って処理する
- base_ui パッケージのコンポーネントを利用する

```dart
CommonFilledButton.sWithIcon(
  onPressed: () async {
    try {
      await _ref.read(sendLikeUseCaseProvider).invoke(/** 省略 */);
    } on InsufficientPointsToSendLikeException {
      await _showErrorDialog(/** 省略 */);
    }
    /** 省略 */
  }
  text: 'いいね!',
  icon: OmiaiAssets.svg.like.svg(),
)
```

---

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-vertical-all.svg)

- app は Flutter の世界
- domain 以下は主には Dart の世界
- app, domain はクライアントアプリの世界
- repository はサーバサイドアプリとの境界

---

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-vertical-repository-system.svg)

<div class="content-left-60">

## repository - system 間

- dio パッケージを通じて HTTP リクエストを行う
- dio パッケージ由来の例外は表出させない
- Result 型のような `HttpResponse` 型を返す
- repository 層では `switch` 文による成功・失敗のハンドリングが強制される
- サーバサイドの負債は repository 層で吸収し切る

</div>

---

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-vertical-domain-repository.svg)

<div class="content-left-60">

## domain - repository 間

- domain は repository のメソッドを呼び出してデータを読み書きする
- repository のメソッドは Result 型のような `RepositoryResult` 型を返す
- domain 層では `switch` 文による成功・失敗のハンドリングが強制される
- domain 層では Dto から業務知識を表すエンティティを生成する

</div>

---

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-vertical-app-domain.svg)

<div class="content-left-60">

## app - domain 間

- app はユーザー体験の実現のために domain のロジックを呼び出し、業務概念のデータを読み書きする
- 業務知識として取り扱うべき例外を捕捉して、ユーザー体験に反映する

</div>

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# なぜパッケージを分けるのか

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# こんな課題はありませんか？🤔

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

# BAD: いろいろな境界や依存の混同

- 業務ロジックの実装が、HTTP クライアントにも、UI にも依存する
- サーバサイドアプリとクライアントアプリのプレゼンテーション層の密結合

※ 極端な例：

```dart
import 'package:dio/dio.dart';
import 'package:flutter/material.dart';
import 'package:flutter_localizations/flutter_localizations.dart';

Future<void> someLogic(BuildContext context) async {
  try {
    final response = await someRepositoryMethod();
    ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(response.message)));
  } on DioException catch (e) {
    ScaffoldMessenger.of(context).showSnackBar(SnackBar(content: Text(AppLocalizations.of(context).error)));
  }
}
```

---

# BAD: 意図しないレイヤーへの依存が行われる

- 例えば Presentation 層が直接 repository 層を呼び出すことが禁止されている場合
- コード規約で縛り、PR レビューで防ぐしか方法がない

```dart
ElevatedButton(
  onPressed: () => ref.read(someRepositoryProvider).doSomething(),
  /** 省略 */
);
```

---

# BAD: 意図しないレイヤーへの依存が行われる

- 通信層が、業務知識としての認証情報に依存する
- 通信層のユニットテストを書きたいのに、業務知識をモックする必要がある
- 「この依存が許されるなら...」と、実装の正しさを判断する基準が曖昧になる

```dart
class SomeRepository {
  /** 省略 */

  Future<Something> fetchSomething({/** 省略 */}) async {
    final accessToken = _ref.read(accessTokenProvider);
    final response = await _client.request(/** 省略 */);
    /** 省略 */
  }
}
```

---

# BAD: 意図しないレイヤーへの依存が行われる

- 汎用的な実装として作られたものが、いつの間にか意図しない対象に依存するようになる

```dart
class PaginatedFetcher<T> {
  /** 省略 */

  Future<List<T>> fetchNextPage(/** 省略 */) async {
    /** 省略 */
    final items = await fetch(/** 省略 */);
    /** 省略 */
    await ref.read(analyticsProvider).logEvent(/** 省略 */);
    return items;
  }
}
```

---

# GOOD: 依存性の注入のテクニックの例

別パッケージにアクセストークン取得ロジックのインターフェースのみを定義する：

```dart
@riverpod
Future<String?> Function() extractAccessToken(Ref ref) => throw UnimplementedError();
```

インターフェースに依存して、アクセストークンを取得し、利用する：

```dart
class SomeRepository {
  /** 省略 */

  Future<Something> fetchSomething({/** 省略 */}) async {
    final accessToken = _ref.read(extractAccessTokenProvider);
    final response = await _client.request(/** 省略 */);
    /** 省略 */
  }
}
```

---

# GOOD: 依存性の注入のテクニック

- アプリの実行時には `ProviderScope.overrides` で domain 層からアクセストークンを取得するロジックを注入する
- たとえば、リフレッシュトークンのロジックなども同様に定義する

```dart
ProviderScope(
  overrides: [
    extractAccessTokenProvider.overrideWith(
      (ref) => () async {
        final auth = await ref.read(authNotifierProvider.future);
        return auth.accessToken;
      },
    ),
  ],
)
```

---

# GOOD: 依存性の注入のテクニック

- ユニットテスト時には `ProviderContainer.overrides` でアクセストークンの有無の状況を上書きする。

```dart
ProviderContainer createContainer({
  /** 省略 */
  bool setApiAccessToken = true,
}) {
  final container = ProviderContainer(
    /** 省略 */
    overrides: [
      extractAccessTokenProvider.overrideWith(
        (_) => () async => setApiAccessToken ? 'test-access-token' : null,
      ),
    ],
  );
  /** 省略 */
}
```

---

# パッケージを分けることで得られる恩恵

## ✅ 誤った依存や実装が静的解析のレベルで防がれる

- 誤った依存や実装が仕組み上できなくなり、PR レビューの負担も少ない

## ✅ 各タスクに必要な実装が明確で集中できる

- 各レイヤーの責務や、許される依存が明確で、実装内容が明確である
- 各レイヤーのユニットテストも当然のように書ける

## ✅ 新たに参画したメンバーが開発しやすい

- 各レイヤーで実装すべき内容や例外ハンドリングの方法がパターン化されている
- Flutter に慣れていないメンバーも、domain や repository 層の実装は Dart エンジニアとして活躍できる

---

# パッケージを分けることで被る不利益？

## 🤔 build_runner を各パッケージで実行するのが面倒

- 仕方なし。エイリアスやタスクランナーなどの工夫を！

## 🤔 同じ機能に関する実装が Explorer 上で遠く見える

- ファイル検索の仕方を工夫すると Explorer 上でファイルを探すことがほとんどなくなる

## 🤔 冗長なコードが増える可能性がある

- サーバサイドの概念、命名、テーブル定義、HTTP インターフェース、クライアントアプリで取り扱う概念に差がない場合など

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# ドキュメンテーションコメント

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# doc comment を 100% 記述

- public_member_api_docs を全面的に有効化
- Effective Dart に従って正しい記法・表現を徹底
- 「パッケージを作るようにアプリケーションを作る」意識を重視
- ソースコードが「実行可能な仕様書」としての理想に近づく

---

# doc comment を 100% 記述

```dart
/// firebase_analytics パッケージが提供する、Firebase Analytics の各機能をラップしたクラス。
///
/// ほとんど firebase_analytics のインターフェースの再実装のような内容になっているが、
///
/// - system パッケージ以外が firebase_analytics パッケージに直接依存しないようにする
/// - 分析ログの送信という性質上、利用する側に await させず、例外ハンドリングもさせない（例外が
/// 起きても呼び出し側の処理を止めない）ようにする
/// - 一種の腐敗防止層としての実装とする
///
/// ことなどを目的としている。
class FirebaseAnalyticsClient {
  /// [FirebaseAnalyticsClient] を生成する。
  const FirebaseAnalyticsClient(this._firebaseAnalytics);

  final FirebaseAnalytics _firebaseAnalytics;

  /// Firebase Analytics にログを送信する。
  ///
  /// 本メソッドの返り値型は `void` で定義されており、[unawaited] で [_logEvent] を呼び
  /// 出すことで、もし例外が発生しても呼び出し側の処理を止めない（ただし、グローバルエラー
  /// ハンドラーを定義すれば、キャッチできる）ようにしている。
  void logEvent({required String name, required Map<String, Object>? parameters}) =>
      unawaited(_logEvent(name: name, parameters: parameters));

  /** 省略 */
}
```

---

# ユニットテスト

matrix を組んで各パッケージのユニットテストを CI で継続的に実行する

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/ci-test.svg)

---

# ユニットテスト

業務ロジック層およびそれより抽象的な層でのユニットテストのカバレッジが 100% であることを CI で維持し続ける

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/ci-coverage-report.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 実装のパターン化とスニペットの充実

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# 実装のパターン化とスニペットの充実

- 実装の大部分をパターン化してスニペットに：
  - Dto 定義とそのテスト
  - Repository クラスの実装とそのテスト
  - 業務概念のエンティティ定義とそのテスト
  - 関数 および class Riverpod の実装とそのテスト
  - UseCase の実装とそのテスト
  - ...など
- doc comment もユニットテストも含めてスニペット化
- スニペットの整備と生成 AI の活用は相性が非常に良い

---

# 実装のパターン化とスニペットの充実

```code-snippets
"import 'package:repository/repository.dart';",
"import 'package:riverpod_annotation/riverpod_annotation.dart';",
"import 'package:util/util.dart';",
"",
"import '../../../util/omiai_api/internal/omiai_api_handler.dart';",
"",
"part '${TM_FILENAME_BASE}.g.dart';",
"",
"/// ...を取得する。",
"///",
"/// 取得に失敗した場合は、...。",
"@riverpod",
"Future<Foo> foo(FooRef ref) async {",
"  final result = await ref.watch(omiaiApiHandlerProvider).request(() async => ref.read(fooRepositoryProvider).fetchFoo());",
"  switch (result) {",
"    case SuccessRepositoryResult(:final data):",
"      return Foo.fromDto(data);",
"    case FailureRepositoryResult(:final e, :final errorDto):",
"      throw SomeException();",
"  }",
"}",
"",
"class SomeException implements Exception {}",
```

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# Add-to-App の活用

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)
