---
title: ASP.NET Core でのシングル ページ アプリケーションを作成するのに JavaScriptServices を使用します。
author: scottaddie
description: ASP.NET Core でサポートされるシングル ページ アプリケーション (SPA) を作成する JavaScriptServices の使用の利点について説明します。
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
uid: client-side/spa-services
ms.openlocfilehash: 6d6a92427d5d4b853248e60a12625573c4375515
ms.sourcegitcommit: c12ebdab65853f27fbb418204646baf6ce69515e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/21/2018
ms.locfileid: "46523299"
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>ASP.NET Core でのシングル ページ アプリケーションを作成するのに JavaScriptServices を使用します。

によって[Scott Addie](https://github.com/scottaddie)と[Fiyaz Hasan](http://fiyazhasan.me/)

シングル ページ アプリケーション (SPA) は、その固有の機能豊富なユーザー エクスペリエンスのための web アプリケーションの人気のある型です。 クライアント側 SPA フレームワークやライブラリの統合など[Angular](https://angular.io/)または[React](https://facebook.github.io/react/)、ASP.NET Core は難しいようにサーバー側のフレームワークを使用します。 [JavaScriptServices](https://github.com/aspnet/JavaScriptServices)統合プロセスの手間を削減するために開発されました。 これにより、別のクライアントおよびサーバー テクノロジ スタックとの間のシームレスな操作ができます。

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>JavaScriptServices とは

JavaScriptServices は、ASP.NET Core 用のクライアント側のテクノロジのコレクションです。 その目的は、Spa を構築するための開発者の推奨されるサーバー側プラットフォームとしての ASP.NET Core の位置です。

JavaScriptServices は、次の 3 つの個別の NuGet パッケージで構成されます。

* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

これらのパッケージは便利な場合します。

* サーバーでの JavaScript を実行します。
* SPA フレームワークやライブラリを使用します。
* Webpack とクライアント側アセットをビルドします。

SpaServices パッケージを使用してこの記事内のフォーカスの多くは配置されます。

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>SpaServices とは

SpaServices は、Spa を構築するための開発者の推奨されるサーバー側プラットフォームとしての ASP.NET Core の位置に作成されました。 SpaServices は ASP.NET core で Spa を開発する必要はありませんし、特定のクライアント フレームワークにロックしません。

SpaServices は、次のような便利なインフラストラクチャを提供します。

* [サーバー側の事前](#server-prerendering)
* [Webpack 開発ミドルウェア](#webpack-dev-middleware)
* [ホットなモジュールの交換](#hot-module-replacement)
* [ルーティングのヘルパー](#routing-helpers)

総称して、これらのインフラストラクチャ コンポーネントは、開発ワークフローと、実行時のエクスペリエンスの両方を強化します。 コンポーネントを個別に採用することができます。

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>SpaServices を使用するための前提条件

SpaServices を使用するには、次のようにインストールします。

* [Node.js](https://nodejs.org/) (バージョン 6 以降) で npm
  * これらのコンポーネントがインストールされを検出できることを確認するには、コマンドラインから、次を実行します。

    ```console
    node -v && npm -v
    ```

注: Azure の web サイトにデプロイする場合不要ここで何もする&mdash;Node.js がインストールされ、サーバー環境で使用できます。

* [!INCLUDE [](~/includes/net-core-sdk-download-link.md)]

  * Visual Studio 2017 を使用して Windows の場合は、SDK がインストールを選択して、 **.NET Core クロス プラットフォーム開発**ワークロード。

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet パッケージ

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>サーバー側の事前

ユニバーサル (isomorphic とも呼ばれます) のアプリケーションは、サーバーとクライアントの両方で実行できる JavaScript アプリケーションです。 Angular、React、およびその他の一般的なフレームワークは、このアプリケーションの開発スタイルのユニバーサル プラットフォームを提供します。 考え方は、まず、Node.js を使用して、サーバー上のフレームワーク コンポーネントをレンダリングし、さらに、クライアントを実行しを委任します。

ASP.NET Core[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)によって提供される SpaServices、サーバー上の JavaScript 関数を呼び出すことによってサーバー側の事前の実装を簡略化します。

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。

* [aspnet 事前](https://www.npmjs.com/package/aspnet-prerendering)npm パッケージ。

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>構成

タグ ヘルパーは、プロジェクトの名前空間の登録を使用して探索可能にされて *_ViewImports.cshtml*ファイル。

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

これらのタグ ヘルパーは Razor ビュー内の HTML のような構文を活用することで、低レベルの Api と直接通信の複雑さで抽象化します。

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module`タグ ヘルパー

`asp-prerender-module`タグ ヘルパーの前のコード例で使用される実行*ClientApp/dist/main-server.js* Node.js を使用してサーバーにします。 わかりやすくするためのために、 *main server.js*ファイルは、TypeScript、JavaScript にトランス パイルもタスクの成果物、 [Webpack](http://webpack.github.io/)プロセスを構築します。 Webpack のエントリ ポイントのエイリアスを定義します`main-server`; と、このエイリアスの依存関係グラフのトラバーサルが始まり、 *ClientApp/ブート-server.ts*ファイル。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

次の Angular の例では、 *ClientApp/ブート-server.ts*ファイルを利用、`createServerRenderer`関数と`RenderResult`の入力、 `aspnet-prerendering` npm パッケージを Node.js を使用してサーバー レンダリングを構成します。 サーバー側のレンダリングが厳密に型指定された JavaScript にラップされて解決関数の呼び出しに渡される宛ての HTML マークアップ`Promise`オブジェクト。 `Promise`オブジェクトの重要性は、DOM のプレース ホルダー要素の挿入のページに HTML マークアップを非同期的に提供します。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data`タグ ヘルパー

組み合わせると、`asp-prerender-module`タグ ヘルパーの`asp-prerender-data`タグ ヘルパーは、Razor ビューからサーバー側 JavaScript にコンテキスト情報を渡すために使用できます。 たとえば、次のマークアップは合格ユーザー データを`main-server`モジュール。

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

受信した`UserName`引数の組み込みの JSON シリアライザーを使用してシリアル化し、は、`params.data`オブジェクト。 次の例で Angular 内でパーソナライズされたあいさつ文を構築するデータの使用、`h1`要素。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

注: タグ ヘルパーで渡されたプロパティの名前を付けて表されます**PascalCase**表記します。 同じプロパティ名で表現は JavaScript とは異なり**camelCase**します。 既定の JSON シリアル化の構成では、この違いを担当します。

展開すると、上記のコード例に、データ渡し可能サーバーからビューに hydrating、`globals`プロパティに提供される、`resolve`関数。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList`配列内で定義されている、`globals`オブジェクトはグローバルで、ブラウザーにアタッチされて`window`オブジェクト。 グローバル スコープにこの変数のホイストでは、特に、サーバーで 1 回と、クライアントでは、同じデータの読み込みに関連している、作業の重複がなくなります。

![ウィンドウ オブジェクトにアタッチされているグローバルの postList 変数](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Webpack 開発ミドルウェア

[Webpack 開発ミドルウェア](https://webpack.github.io/docs/webpack-dev-middleware.html)Webpack がオンデマンドでリソースをビルドするための合理的な開発ワークフローが導入されています。 ミドルウェアが自動的にコンパイルし、ページがブラウザーに再読み込みする際、クライアント側のリソースの機能します。 別の方法では、サードパーティの依存関係またはカスタム コードが変更されたときに、プロジェクトの npm ビルド スクリプトを使用して Webpack を手動で起動します。 Npm スクリプトを作成する、 *package.json*ファイルは、次の例に示します。

```json
"build": "npm run build:vendor && npm run build:custom",
```

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。

* [aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm パッケージ。

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>構成

次のコードを使用して HTTP 要求パイプラインに Webpack 開発ミドルウェアが登録されている、 *Startup.cs*ファイルの`Configure`メソッド。

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります[静的ファイルをホストしている登録](xref:fundamentals/static-files)を使用して、`UseStaticFiles`拡張メソッド。 セキュリティ上の理由から、アプリは開発モードで実行時にのみ、ミドルウェアを登録します。

*Webpack.config.js*ファイルの`output.publicPath`プロパティに通知を監視するミドルウェア、`dist`フォルダーの変更。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>ホットなモジュールの交換

Webpack の考える[ホット モジュールの交換](https://webpack.js.org/concepts/hot-module-replacement/)(HMR) 機能の進化したものとして[Webpack 開発ミドルウェア](#webpack-dev-middleware)します。 すべて同じメリットが導入されて HMR が自動的に変更をコンパイルした後、ページのコンテンツを更新することでさらに、開発ワークフローを効率化します。 メモリ内の現在の状態と、SPA のデバッグ セッションに支障をきたすのブラウザーの更新でこれを混同しないでください。 Webpack 開発ミドルウェア サービスと、ブラウザーに変更がプッシュされることを意味すると、ブラウザーの間のライブ リンクがあります。

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。

* [webpack ホット ミドルウェア](https://www.npmjs.com/package/webpack-hot-middleware)npm パッケージ。

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>構成

MVC の HTTP 要求パイプラインに HMR コンポーネントを登録する必要があります、`Configure`メソッド。

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

そうであったとして[Webpack 開発ミドルウェア](#webpack-dev-middleware)、`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります、`UseStaticFiles`拡張メソッド。 セキュリティ上の理由から、アプリは開発モードで実行時にのみ、ミドルウェアを登録します。

*Webpack.config.js*ファイルを定義する必要があります、`plugins`場合でも、それを空の配列。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

ブラウザーでアプリを読み込んだ後は、開発者ツールのコンソール タブは、HMR アクティブ化の確認を提供します。

![ホットのモジュールの交換接続メッセージ](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>ルーティングのヘルパー

ほとんどの ASP.NET Core ベースの Spa では、サーバー側でルーティングだけでなくクライアント側でルーティングを必要があります。 SPA と MVC ルーティング システムは、競合することがなく個別に作業できます。 ただし、課題を 1 つエッジ ケース読んだり: 404 の HTTP 応答を識別します。

シナリオの場合を検討してください、拡張子のないルートの`/some/page`使用されます。 要求がパターン一致、サーバー側ルートがそのパターンのクライアント側ルートが一致するものとします。 ここでの受信要求を考えてみます`/images/user-512.png`、一般に、サーバー上の画像ファイルを検索する要求。 クライアント側のアプリケーションは処理は、ほとんどありません任意のサーバー側のルートまたは静的ファイルにその要求されたリソースのパスが一致しない場合、一般に HTTP 404 状態コードを取得します。

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。

* クライアント側ルーティング npm パッケージです。 Angular を使用して、例として。

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>構成

という名前の拡張メソッド`MapSpaFallbackRoute`で使用されて、`Configure`メソッド。

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

ヒント: ルートは、構成されている順序で評価されます。 その結果、`default`パターン一致の前のコード例ではルートが最初に使用します。

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>新しいプロジェクトを作成します。

JavaScriptServices には、事前構成済みのアプリケーション テンプレートが用意されています。 SpaServices は、さまざまなフレームワークおよびライブラリと、Angular、React、Redux などと連携して、これらのテンプレートで使用されます。

これらのテンプレートは、次のコマンドを実行して .NET Core CLI を使用してインストールできます。

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

使用可能な SPA テンプレートの一覧が表示されます。

| テンプレート                                 | 短い形式の名前 | 言語 | Tags        |
|:------------------------------------------|:-----------|:---------|:------------|
| Angular と ASP.NET Core の MVC             | angular    | [C#]     | Web/MVC/SPA |
| React.js 付きの ASP.NET Core の MVC            | react      | [C#]     | Web/MVC/SPA |
| MVC の ASP.NET Core react.js と Redux  | reactredux | [C#]     | Web/MVC/SPA |

SPA テンプレートのいずれかを使用して新しいプロジェクトを作成するには含める、**短い名前**内のテンプレートの[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンド。 次のコマンドでは、サーバー側用に構成された ASP.NET Core MVC で Angular アプリケーションを作成します。

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>ランタイムの構成モードを設定します。

2 つのプライマリのランタイム構成モードは次のとおりです。

* **開発**:
  * デバッグを容易にするソース マップが含まれています。
  * パフォーマンスのクライアント側のコードを最適化しません。
* **実稼働**:
  * ソース マップを除外します。
  * バンドルと縮小を使用してクライアント側のコードを最適化します。

ASP.NET Core という環境変数を使用して`ASPNETCORE_ENVIRONMENT`構成モードを格納します。 参照してください**[環境を設定](xref:fundamentals/environments#set-the-environment)** 詳細についてはします。

### <a name="running-with-net-core-cli"></a>.NET Core CLI で実行します。

プロジェクトのルートで、次のコマンドを実行してには、必要な NuGet と npm パッケージを復元します。

```console
dotnet restore && npm i
```

ビルドし、アプリケーションを実行します。

```console
dotnet run
```

アプリケーションの起動時に従い、localhost 上で、[ランタイム構成モード](#runtime-config-mode)します。 移動する`http://localhost:5000`ブラウザーにランディング ページが表示されます。

### <a name="running-with-visual-studio-2017"></a>Visual Studio 2017 での実行

開く、 *.csproj*によって生成されたファイル、[新しい dotnet](/dotnet/core/tools/dotnet-new)コマンド。 必要な NuGet と npm パッケージは、プロジェクトを開くと自動的に復元されます。 この復元プロセスは、数分かかる場合があり、アプリケーションが完了するときに実行する準備ができます。 キーを押して、緑色の実行 ボタンをクリックします`Ctrl + F5`、し、アプリケーションのランディング ページで、ブラウザーが開かれます。 アプリケーションによる localhost で実行、[ランタイム構成モード](#runtime-config-mode)します。

<a name="app-testing"></a>

## <a name="testing-the-app"></a>アプリのテスト

SpaServices テンプレートは、事前構成を使用してクライアント側のテストを実行して[Karma](https://karma-runner.github.io/1.0/index.html)と[Jasmine](https://jasmine.github.io/)します。 Jasmine は、Karma テスト ランナーはこれらのテストには、人気のある単位の JavaScript 用のテスト フレームワークです。 Karma を使用するように構成、 [Webpack 開発ミドルウェア](#webpack-dev-middleware)されるよう、開発者が停止し、変更されるたびにテストを実行するために必要はありません。 テスト_ケースまたはテスト ケース自体に対して実行されるコードであるかどうか、テストが自動的に実行されます。

Angular アプリケーションを使用して、例として、2 つの Jasmine テスト_ケースが既に提供されている、`CounterComponent`で、 *counter.component.spec.ts*ファイル。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

コマンド プロンプトを開き、 *ClientApp*ディレクトリ。 次のコマンドを実行します。

```console
npm test
```

スクリプトで定義されている設定を読み取り、Karma テスト ランナーを起動する、 *karma.conf.js*ファイル。 その他の設定の間で、 *karma.conf.js*経由で実行するテスト ファイルを識別します。 その`files`配列。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>アプリケーションの発行

デプロイの準備完了パッケージに生成されたクライアント側アセットと発行された ASP.NET Core の成果物を組み合わせることは面倒なことができます。 さいわいにも、SpaServices がという名前のカスタム MSBuild ターゲットとそのパブリケーション全体のプロセスを調整`RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild ターゲットでは、次の責任があります。

1. Npm パッケージを復元します。
1. サード パーティ製のクライアント側の資産の運用グレードのビルドを作成します。
1. カスタムのクライアント側の資産の運用グレードのビルドを作成します。
1. Webpack で生成された資産を発行フォルダーにコピーします。

MSBuild ターゲットが実行するときに呼び出されます。

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>その他の技術情報

* [Angular Docs](https://angular.io/docs)
