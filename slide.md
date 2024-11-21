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

# 本セッションのゴール

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# 本セッションのゴール

<div class="center-goal-container">
  <h2>マッチングアプリ『Omiai』を題材に<br>大規模アプリの Flutter へのリプレイスを<br>成功に導くアプローチを考える</h2>
</div>

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

## &#x2705; 目標

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

## &#x2705; 負債化を繰り返さず、高い開発生産性を維持し続ける

- アーキテクチャ、CI、テスト、ドキュメンテーションなどで多くの工夫

## &#x2705; Flutter 経験の無いメンバーや新規参画メンバーが早期に活躍できる

- Flutter 経験者ゼロ・業務委託中心組織から正社員採用によるチームの拡大へ

## &#x2705; リプレイス中も機能追加は止めない

- Add-to-App を利用して既存アプリから Flutter をモジュールとして呼び出す
- 主要なユーザー体験から順に、新しい機能を追加しながら Flutter にリプレイスする

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 陥りがちな現実

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

# PR レビューで防ぐしかないコード規約違反

- UI 層から直接 Repository のメソッドを呼び出すのを禁止している場合
- コード規約で縛り、PR レビューで防ぐしか方法がない
- 見逃されたコードが生き残ってしまうと、新規参画メンバーの迷いに繋がる

```dart
ElevatedButton(
  onPressed: () async {
    await someRepositoryMethod();
  },
  /** 省略 */
);
```

---

# UI とロジックの分離の失敗

- 業務ロジックの実装に `BuildContext` が依存する
- ロジックのユニットテストが Dart のユニットテストで書けない

```dart
import 'package:flutter/material.dart';
import 'package:flutter_localizations/flutter_localizations.dart';

Future<void> someLogic(BuildContext context) async {
  await doSomething();
  ScaffoldMessenger.of(context).showSnackBar(
    SnackBar(content: Text(AppLocalizations.of(context).success)),
  );
}
```

---

# 不自然な依存

- UI 層で HTTP 通信が起因の例外を流れてくる・捕捉する
- UI 層が特定の HTTP クライアントパッケージに依存するのは不自然（知るべきでない）

```dart
import 'package:dio/dio.dart';

ElevatedButton(
  onPressed: () async {
    try {
      await doSomething();
    } on DioException catch (e) {
      /** 省略 */
    },
  },
  /** 省略 */
),
```

---

# 曖昧な例外ハンドリングの方針

- どこでどんな例外ハンドリングをするべきかの方針が明確でない
- 捕捉した例外をどのようにユーザー体験に反映するかの方針が明確でない
- 図らずエラーコードなどの情報をユーザーに見せてしまう

```dart
ElevatedButton(
  onPressed: () async {
    try {
      await doSomething();
    } on APIException catch (e) {
      await showDialog<void>(
        context: context,
        builder: (context) => AlertDialog(content: Text('${e.statusCode}')),
      );
    },
  },
  /** 省略 */
),
```

---

# 依存を誤って負債化

- 通信層が、業務知識としての認証情報に依存する
- 通信層のユニットテストを書くのに、業務知識をモックする必要がある
- もしこの依存が許されるなら、あらゆる業務知識に同様に依存し始める危険がある

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

# 依存を誤って負債化

- 当初の汎用的な基盤実装が、いつの間にか意図しない対象に依存するようになる
- 汎用性は失われ、責務が曖昧な負債コードに

```dart
class PaginatedFetcher<T> {
  /** 省略 */

  Future<List<T>> fetchNextPage(int page, int perPage) async {
    /** 省略 */
    final items = await fetch(/** 省略 */);
    /** 省略 */
    await ref.read(someUseCaseProvider).doSomething();
    return items;
  }
}
```

---

# クライアント・サーバサイドアプリの密結合

- ユーザーに知らせるエラーメッセージに、サーバサイドからのレスポンスデータがそのまま利用される
- クライアントアプリとサーバサイドアプリの境界が曖昧

```dart
ElevatedButton(
  onPressed: () async {
    try {
      await doSomething();
    } on APIException catch (e) {
      await showDialog<void>(
        context: context,
        builder: (context) => AlertDialog(content: Text(e.message)),
      );
    },
  },
  /** 省略 */
),
```

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# どのように防ぐ？

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 可能な限り仕組みで防ぐ！

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

# 方針

## &#x2705; マルチパッケージ構成

- レイヤーごとにパッケージを分ける
- 各パッケージの責務と許可される依存を厳密にする
- Flutter の世界と Dart の世界、クライアントアプリとサーバサイドアプリの境界を明確にする

## &#x2705; ユニットテストを十分に継続的に書く

- 業務ロジック層およびそれより抽象的な層で、ユニットテストがカバレッジ 100% で記述できる

## &#x2705; 実装を可能な限りパターン化する

- 典型的な実装をパターン化し、新規参画メンバーの早期の活躍を実現する

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# パッケージ一覧（抜粋）

- レイヤーごとにパッケージを分ける
- 各パッケージに許される直接的な依存は矢印の先のパッケージのみに限定される
- 他にも Unimplemented なインターフェースのみを定義し、各パッケージに利用させるパッケージなどもある

<p class="note">※ ことば遣いは Flutter 公式の"Architecting Flutter apps" や Android Developers の "Guide to app architecture" に類似</p>

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-all.svg)

---

# パッケージ一覧 (app)

- Flutter アプリのエントリポイントとしての実装
  - `dart-define` などを通じたアプリの設定値の取得・決定
  - `ProviderScope.overrides` による依存の注入
- UI 層としてのユーザー体験を実装
  - ウィジェットツリーの構築、画面遷移、各画面やコンポーネントの描画とインタラクション

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-app.svg)

---

# パッケージ一覧 (base_ui)

- Flutter アプリ内で共通利用される テキストスタイル、色などのスタイルガイドの実装
- 画像、ボタン、AppBar、TextField などの基礎的な UI 部品の実装

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-base-ui.svg)

---

# パッケージ一覧 (domain)

- クライアントアプリの業務概念を表すエンティティの定義
- app がユーザー体験として利用する例外型の定義
- Repository を通じてのデータの取得や保存などを含む業務ロジックの記述

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-domain.svg)

---

# パッケージ一覧 (repository)

- データソースとのやり取りの記述
- 接続先のデータソース（自社のバックエンドサーバやローカルストレージなど）の情報は表出させない

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-repository.svg)

---

# パッケージ一覧 (system)

- 3rd パーティのツールをラップして基礎的な実装を記述
  - 例：HTTP クライアント、Shared Preferences, Firebase Analytics など
- 3rd パーティのツールとの依存は例外も含めて閉じ込めて外部に表出させない
- 不要な機能のインターフェースは塞いでシンプルにする

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-system.svg)

---

# パッケージ一覧（Flutter の世界）

- app, base_ui パッケージは Flutter に直接依存する
- Flutter エンジニアとしてコードを書く

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-app-base-ui.svg)

---

# パッケージ一覧（Dart の世界）

- domain, repository, system パッケージは Flutter には直接依存しない
  - SharedPreferences などは内部で Flutter には依存しているので、transitive には依存する
- Dart エンジニアとしてコードを書く
  - Dart 言語と riverpod や freezed にさえ慣れれば Flutter 経験が浅くても書ける

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-domain-repository-system.svg)

---

# パッケージ一覧（サーバサイドとの境界）

- repository パッケージがサーバサイドとの境界として働く
- サーバサイドの負債は repository パッケージで吸収し切る
- app, domain はサーバサイドの存在や業務知識を意識せずに実装する

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-repository.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 各パッケージの実装内容

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# base_ui

- Flutter アプリ内で共通利用される テキストスタイル、色などのスタイルガイドの実装
- 画像、ボタン、AppBar、TextField などの基礎的な UI 部品の実装

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-base-ui.svg)

---

# base_ui が依存するパッケージの例

- Flutter への依存: direct
- 画像のベースの実装のために extended_image、静的アセットのコード生成のために flutter_gen などを利用

```yaml
dependencies:
  extended_image:
  flutter:
    sdk: flutter
  flutter_svg:

dev_dependencies:
  build_runner:
  flutter_gen_runner:
```

---

# base_ui の実装例

- Figma と同名で定義された TextStyle, Color などのスタイルガイド
- Material のテーマ定義とは一致していない

```dart
/// アプリで使用する色一覧。
abstract interface class AppColor {
  /// プライマリーの 01 の赤色。
  static const Color primary01 = Color(0xFFFF3159);

  /// プライマリーの 02 の青色。
  static const Color primary02 = Color(0xFF3F9AD1);

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
}
```

---

# system

- 3rd パーティのツールをラップして基礎的な実装を記述
- 外部パッケージへの依存を閉じ込めて、外部に表出させない

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-system.svg)

---

# system が依存するパッケージの例

- Flutter への依存: transitive
- 各種の 3rd パーティのツールに依存する

```yaml
dependencies:
  dio:
  firebase_analytics:
  firebase_remote_config:
  shared_preferences:
```

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

- `HTTPClient` を通じた HTTP リクエストの結果を表現する `HttpResponse` クラスの例（一部省略・簡略化）
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

- HTTP リクエストの結果として `HttpResponse` を返す
- 3rd パーティーパッケージへの依存は例外も含めて外部には表出させない

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

- データソースとのやり取りを記述する
- 接続先のデータソース（自社のバックエンドサーバやローカルストレージなど）の情報は表出させない

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-repository.svg)

---

# repository が依存するパッケージ

- Flutter への依存: transitive
- system パッケージに依存にして HTTP リクエストなどを行う
- json_serializable, freezed による HTTP レスポンスのパースと Dto の定義を行う

```yaml
dependencies:
  freezed_annotation:
  json_annotation:
  riverpod:
  system:
    path: ../system

dev_dependencies:
  build_runner:
  freezed:
  json_serializable:
  riverpod_generator:
```

---

# repository の実装例

- Repository による通信結果を表現する `RepositoryResult` クラスの例（一部省略・簡略化）
- Result 型のような形で成功・失敗を表現

```dart
@freezed
sealed class RepositoryResult<T> with _$RepositoryResult<T> {
  const factory RepositoryResult.success(T data) = SuccessRepositoryResult<T>;

  const factory RepositoryResult.failure(
    Object e, {
    FailureRepositoryResultReason? reason,
    ErrorDto? errorDto,
  }) = FailureRepositoryResult;
}
```

---

# repository の実装例

- `HttpResponse` の成功・失敗を switch 文で分岐して、`RepositoryResult` を返す。

```dart
class AccountDetailRepository {
  /** 省略 */

  Future<RepositoryResult<AccountDetailDto>> fetchAccountDetail() async {
    final response = await _ref.read(httpClientProvider).request(/** 省略 */);
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

- Dto として HTTP レスポンスの型定義をする
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

- クライアントアプリの業務概念を表すエンティティの定義
- app がユーザー体験として利用する例外型の定義
- Repository を通じてのデータの取得や保存などを含む業務ロジックの記述

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-domain.svg)

---

# domain が依存するパッケージ

- Flutter への依存: transitive
- repository パッケージに依存してデータにアクセスする
- freezed で業務概念のエンティティを定義する
- riverpod で状態管理ロジックを実装する

```yaml
dependencies:
  freezed_annotation:
  repository: 
    path: ../repository
  riverpod:
  riverpod_annotation:

dev_dependencies:
  build_runner:
  freezed:
  riverpod_generator:
```

---

# domain の実装例

- マッチングアプリにおけるお相手メンバーの業務概念の定義の例
- HTTP レスポンスに対応する Dto からエンティティを生成する

```dart
@freezed
class Member with _$Member {
  const factory Member({
    required int userId,
    /** 省略 */
    required LogInStatus loginStatus,
  }) = _Member;

  factory Member.fromDto(MemberDto dto) => Member(
    userId: dto.userId,
    /** 省略 */
    loginStatus: LogInStatus.fromDateTime(dto.lastAccessedAt),
  );
}
```

---

# domain の実装例

- お相手メンバーのログインステータス（直近どのくらいアクティブか）の定義の例
- 最終ログイン日時というサーバサイド (DB) 上の事実から、ログインステータスというクライアントアプリにおける業務概念を計算する

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

- riverpod の関数で取得処理のロジックを記述する
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

- riverpod のクラス (Notifier) で状態管理のロジックを記述する

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
    final result = await _ref.read(likeRepositoryProvider).sendLike(/** 省略 */);
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

- Flutter アプリのエントリポイントとしての実装
  - `dart-define` などを通じたアプリの設定値の取得・決定
  - `ProviderScope.overrides` による依存の注入
- UI 層としてのユーザー体験を実装
  - ウィジェットツリーの構築、画面遷移、各画面やコンポーネントの描画、ユーザー操作とのインタラクション

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-app.svg)

---

# app が依存するパッケージ

- Flutter への依存: direct

```yaml
dependencies:
  auto_route:
  base_ui:
    path: ../base_ui
  domain:
    path: ../domain
  flutter:
    sdk: flutter
  flutter_hooks:
  hooks_riverpod:

dev_dependencies:
  auto_route_generator:
  build_runner:
```

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
    } on InsufficientPointsToSendLikeException catch (e) {
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

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-repository-system.svg)

## repository - system 間

- dio パッケージを通じて HTTP リクエストを行う
- dio パッケージへの依存は例外も含めて repository に表出しない
- Result 型のような `HttpResponse` 型をインターフェースとすることで、repository 層での `switch` による成功・失敗のハンドリングが強制、画一化される

---

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-domain-repository.svg)

## domain - repository 間

- repository のメソッドを呼び出してデータを読み書きする
- Result 型のような `RepositoryResult` 型をインターフェースとすることで、domain 層での `switch` による成功・失敗のハンドリングが強制、画一化される
- domain 層では Dto から業務知識を表すエンティティを生成し、サーバサイドの負債は持ち込まない

---

# パッケージ構成

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/package-app-domain.svg)

## app - domain 間

- app はユーザー体験の実現のために domain のロジックを呼び出し、業務概念のデータを読み書きする
- 業務知識として取り扱うべき例外を捕捉して、ユーザー体験に反映する

---

# パッケージを分けることで得られる恩恵

## &#x2705; 誤った依存や実装が静的解析のレベルで防がれる

- 誤った依存や実装が仕組み上できなくなり、PR レビューの負担も少ない

## &#x2705; 各タスクに必要な実装が明確で集中できる

- 各レイヤーの責務や、許される依存が明確で、実装内容に悩まない
- 各レイヤーのユニットテストも容易に書ける

## &#x2705; 新たに参画したメンバーが開発しやすい

- 各レイヤーで実装すべき内容や例外ハンドリングの方法がパターン化されている
- Flutter に慣れていないメンバーも、domain や repository 層の実装は Dart エンジニアとして活躍できる

---

# パッケージを分けることで被る不利益？

## &#x1F914; build_runner を各パッケージで実行するのが面倒

- 仕方なし。エイリアスやタスクランナーなどの工夫を！

## &#x1F914; 同じ機能に関する実装が Explorer 上で遠く見える

- ファイル検索の仕方を工夫すると Explorer 上でファイルを探すことがなくなる

## &#x1F914; 冗長なコードが増える可能性がある

- サーバサイドの概念、命名、テーブル定義、HTTP インターフェース、クライアントアプリで取り扱う概念に差がない場合など

## &#x1F914; 難しそう・うまく運用できるか不安

- 単一パッケージの構成に戻すのは簡単！

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# ここまでのまとめ

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/background-black.svg)

---

# ここまでのまとめ

パッケージを分けることで...

- &#x2705; 依存に関する誤った実装を仕組み上防ぐことができる
- &#x2705; 各レイヤーの責務が明確になり、ユニットテストも容易に書ける
- &#x2705; Result 相当のインターフェース定義と合わせて、レイヤーを跨ぐ例外ハンドリングがパターン化される
- &#x2705; 「いま自分は Flutter エンジニアなのか、Dart エンジニアなのか」の意識を明確にして開発できる
- &#x2705; クライアントアプリとサーバサイドアプリとの境界が明確になり、サーバサイドの負債をクライアントアプリに持ち込まなくなる

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# リプレイスの成功のための更なる試み

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# リプレイスの成功のための更なる試み

## &#x2705; ユニットテスト

- domain および、それより抽象的なレイヤーでテストカバレッジ 100%
- CI で継続的にカバレッジを計測

## &#x2705; ドキュメンテーション

- public_member_api_docs を全面的に有効化して doc comment を 100% 記述

## &#x2705; 実装の可能な限りのパターン化

- マッチングアプリはそんなに難しくない！
- 生成 AI を活用する試み

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# ユニットテスト

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# ユニットテストの対象

<img src="https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/test-scope.png" style="display: block; margin: auto;" alt="test-scope" width="75%"/>

<div style="font-size: 14px; color: #666; text-align: center; margin-top: 16px;">
出典：<a href="https://logmi.jp/technology/329184" style="color: #666;">「自動テストの種類の曖昧さが少ない「テストサイズ」という分類」（ログミーTech）</a>
</div>

---

# ユニットテスト

- matrix を組んで各パッケージのユニットテストを CI で継続的に実行する

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/ci-test.svg)

---

# ユニットテスト

- 業務ロジック層およびそれより抽象的な層でのユニットテストのカバレッジが 100% であることを PR 上で確認する

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/ci-coverage-report.svg)

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# ドキュメンテーションコメント

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# public_member_api_docs

<!-- ![bg cover](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/public-member-api-docs.png) -->

<img src="https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/public-member-api-docs.png" alt="public_member_api_docs" width="100%"/>

---

# ドキュメンテーションコメント

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

# doc comment を 100% 記述

- public_member_api_docs の lint ルールを全面的に有効化
- パッケージ分割と合わせて、「パッケージを作るようにアプリケーションを作る」意識を重視
- ソースコードが「実行可能な仕様書」としての理想に近づく

---

# 実装の可能な限りのパターン化

- repository
  - Dto の定義と Repository クラスの実装
- domain
  - エンティティの定義と fromDto の factory コンストラクタ
  - 関数の riverpod の実装
  - class の riverpod の実装
  - UseCase の実装
- ... など

## → ユニットテスト、doc comment も含めてすべてスニペット化！

---

# 例：Repository の実装

<video src="assets/snippet-repository.mp4" muted autoplay loop width="100%"></video>

---

# 例：Repository のテスト

<video src="assets/snippet-repository-test.mp4" muted autoplay loop width="100%"></video>

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# 実装の可能な限りのパターン化

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# 実装の可能な限りのパターン化

- 合計約 20 個のスニペットを `.code-snippets` として共有
- ファイル名などの一部を正規表現で抽出して、一部動的な内容も生成できる
- 各レイヤーの典型的な実装すべてをパターン化してスニペット化
  - スニペットで対応できない実装があれば、不確実性の高い対象として認識できる
- ユニットテストの記述対象は、テストコードもパターン化してスニペット化

---

# 試み：生成 AI の活用

<video src="assets/ai-repository-test.mp4" muted autoplay loop width="100%"></video>

---

<!-- _class: lead -->

<style scoped>h1 { margin-top: 80px; }</style>

# まとめ

![bg](https://cdn.kosukesaigusa.com/flutter-kaigi-2024/assets/center-title-background.svg)

---

# まとめ

- &#x2705; パッケージ分割による堅牢な設計
- &#x2705; テストカバレッジの継続的な計測とドキュメンテーションによる品質担保
- &#x2705; スニペットと生成 AI の活用による開発効率化
