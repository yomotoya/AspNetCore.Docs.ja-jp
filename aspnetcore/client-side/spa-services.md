---
title: "JavaScriptServices を使用して ASP.NET Core の単一ページ アプリケーションを作成するには"
author: scottaddie
description: "ASP.NET Core 裏付け単一ページ アプリケーション (SPA) を作成する JavaScriptServices を使用する利点について説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: H1Hack27Feb2017
ms.date: 08/02/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: client-side/spa-services
ms.openlocfilehash: c962fc160cf39ad1c69f4269616c993fde420035
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
# <a name="use-javascriptservices-to-create-single-page-applications-in-aspnet-core"></a>JavaScriptServices を使用して ASP.NET Core の単一ページ アプリケーションを作成するには

によって[Scott Addie](https://github.com/scottaddie)と[Fiyaz Hasan](http://fiyazhasan.me/)

単一ページ アプリケーション (SPA) は、その固有の機能豊富なユーザー エクスペリエンスのための web アプリケーションの一般的な型です。 クライアント側 SPA フレームワークまたはライブラリを統合するよう[角](https://angular.io/)または[反応](https://facebook.github.io/react/)、ASP.NET Core を困難になる可能性と同じようにサーバー側フレームワークでします。 [JavaScriptServices](https://github.com/aspnet/JavaScriptServices)を統合プロセスで摩擦を減らすために開発されました。 異なるクライアント/サーバー テクノロジ スタックとの間のシームレスな操作を可能になります。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/spa-services/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

<a name="what-is-js-services"></a>

## <a name="what-is-javascriptservices"></a>JavaScriptServices とは何ですか。

JavaScriptServices は、ASP.NET Core 用のクライアント側のテクノロジのコレクションです。 その目的は SPAs を構築するための開発者の推奨されるサーバー側のプラットフォームとして ASP.NET Core を配置します。

次の 3 つの個別の NuGet パッケージの JavaScriptServices で構成されます。
* [Microsoft.AspNetCore.NodeServices](https://www.nuget.org/packages/Microsoft.AspNetCore.NodeServices/) (NodeServices)
* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) (SpaServices)
* [Microsoft.AspNetCore.SpaTemplates](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaTemplates/) (SpaTemplates)

これらのパッケージは役立つ場合します。
* サーバーでの JavaScript を実行します。
* SPA フレームワークまたはライブラリを使用します。
* Webpack 使用したアセットのクライアント側をビルドします。

この記事でフォーカスの多くは、SpaServices パッケージを使用してに配置されます。

<a name="what-is-spa-services"></a>

## <a name="what-is-spaservices"></a>SpaServices とは何ですか。

SpaServices は SPAs を構築するための開発者の推奨されるサーバー側のプラットフォームとして ASP.NET Core の位置に作成されました。 SpaServices が ASP.NET Core と SPAs を開発するため必要はありませんし、特定のクライアント フレームワークに頼る必要ありません。

SpaServices は、次のような便利インフラストラクチャを提供します。
* [サーバー側の事前](#server-prerendering)
* [Webpack デベロッパー ミドルウェア](#webpack-dev-middleware)
* [ホット モジュールの交換](#hot-module-replacement)
* [ルーティングのヘルパー](#routing-helpers)

集合的に、これらのインフラストラクチャ コンポーネントは、開発ワークフローと、実行時のエクスペリエンスの両方を高めます。 コンポーネントを個別に適用することができます。

<a name="spa-services-prereqs"></a>

## <a name="prerequisites-for-using-spaservices"></a>SpaServices を使用するための前提条件

SpaServices を操作するには次のようにインストールします。
* [Node.js](https://nodejs.org/) (version 6 以降) npm で
    * これらのコンポーネントがインストールされているし、見つかることを確認するには、コマンドラインから、次を実行します。

    ```console
    node -v && npm -v
    ```

注: を Azure の web サイトに配置する場合は、する必要はありませんここでは何も操作&mdash;Node.js がインストールされ、サーバー環境で使用できます。

* [.NET core SDK](https://www.microsoft.com/net/download/core) 1.0 (またはそれ以降)
    * Visual Studio 2017 を選択してインストールする Windows の場合は、この**.NET Core クロスプラット フォーム開発**ワークロード。

* [Microsoft.AspNetCore.SpaServices](https://www.nuget.org/packages/Microsoft.AspNetCore.SpaServices/) NuGet パッケージ

<a name="server-prerendering"></a>

## <a name="server-side-prerendering"></a>サーバー側の事前

ユニバーサル (型とも呼ばれる) アプリケーションは、JavaScript アプリケーションが実行されている両方のサーバーとクライアントの対応です。 角、対応、およびその他の一般的なフレームワークは、このアプリケーションの開発スタイルのユニバーサル プラットフォームを提供します。 つまり、まず、Node.js を使用してサーバー上のフレームワーク コンポーネントを表示し、さらに、クライアントを実行しを委任します。

ASP.NET Core[タグ ヘルパー](xref:mvc/views/tag-helpers/intro)によって提供される SpaServices サーバーに使用する JavaScript 関数を呼び出すことによってサーバー側の事前の実装を簡素化します。

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。
* [aspnet 事前](https://www.npmjs.com/package/aspnet-prerendering)npm パッケージ。

    ```console
    npm i -S aspnet-prerendering
    ```

### <a name="configuration"></a>構成

タグ ヘルパーは、プロジェクトの名前空間登録を使用して探索可能にされて*_ViewImports.cshtml*ファイル。

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/_ViewImports.cshtml?highlight=3)]

これらのタグ ヘルパーは、Razor ビュー内の HTML に似た構文を活用することで、低レベルの Api と直接通信の複雑さを抽象します。

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=5)]

### <a name="the-asp-prerender-module-tag-helper"></a>`asp-prerender-module`ヘルパーにタグ付け

`asp-prerender-module`タグ ヘルパーに渡し、前のコード例で使用される実行*ClientApp/dist/main-server.js* Node.js を使用してサーバーにします。 わかりやすくするためのために、 *main server.js*ファイルが、TypeScript-JavaScript transpilation 内のタスクの成果物、 [Webpack](http://webpack.github.io/)プロセスをビルドします。 Webpack のエントリ ポイントのエイリアスを定義する`main-server`; でこのエイリアスの依存関係グラフの走査を開始し、 *ClientApp/ブート-server.ts*ファイル。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=53)]

次の角度の例で、 *ClientApp/ブート-server.ts*ファイルを利用、`createServerRenderer`関数と`RenderResult`の入力、 `aspnet-prerendering` Node.js を使用してサーバーのレンダリングを構成する npm パッケージです。 サーバー側のレンダリングが厳密に型指定された JavaScript にラップされて解決関数の呼び出しに渡される宛ての HTML マークアップ`Promise`オブジェクト。 `Promise`オブジェクトの有意性がプレース ホルダーの DOM の要素でインジェクションのページに HTML マークアップを非同期的に提供します。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-34,79-)]

### <a name="the-asp-prerender-data-tag-helper"></a>`asp-prerender-data`ヘルパーにタグ付け

組み合わせると、`asp-prerender-module`タグ ヘルパーの`asp-prerender-data`タグ ヘルパーは、Razor ビューから、サーバー側 JavaScript にコンテキスト情報を渡すために使用できます。 たとえば、次のマークアップ合格とユーザー データを`main-server`モジュール。

[!code-cshtml[](../client-side/spa-services/sample/SpaServicesSampleApp/Views/Home/Index.cshtml?range=9-12)]

受信した`UserName`引数は、組み込みの JSON シリアライザーを使用してシリアル化されに格納されて、`params.data`オブジェクト。 内の個人用に設定された応答メッセージを構築するために次の角度の例では、データが使用される、`h1`要素。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,38-52,79-)]

注: タグ ヘルパーで渡されるプロパティ名で表される**PascalCase**表記します。 JavaScript に対して、同じプロパティ名がで表されるコントラストを**キャメル ケース**です。 既定の JSON シリアル化の構成は、この違いを担当します。

前のコード例に展開するにデータをサーバーからに渡せるビュー hydrating によって、`globals`プロパティに提供される、`resolve`関数。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/boot-server.ts?range=6,10-21,57-77,79-)]

`postList`配列内で定義された、`globals`オブジェクトがアタッチされるブラウザーのグローバル`window`オブジェクト。 このグローバル スコープの変数ホイスト サーバー上で実行し、もう一度クライアントでは、同じデータの読み込みに関連するように特に、作業の重複を除去します。

![ウィンドウ オブジェクトにアタッチされているグローバル postList 変数](spa-services/_static/global_variable.png)

<a name="webpack-dev-middleware"></a>

## <a name="webpack-dev-middleware"></a>Webpack デベロッパー ミドルウェア

[Webpack デベロッパー ミドルウェア](https://webpack.github.io/docs/webpack-dev-middleware.html)Webpack が要求時にリソースをビルドするという合理的な開発ワークフローが導入されています。 ミドルウェアは自動的にコンパイルして、ページがブラウザーで再読み込みされるときに、クライアント側のリソースを提供します。 別の方法では、サード パーティの依存関係またはカスタム コードが変更されたときに、プロジェクトの npm ビルド スクリプトを使用して Webpack を手動で起動します。 スクリプトの作成、npm、 *package.json*ファイルを次の例に示します。

[!code-json[](../client-side/spa-services/sample/SpaServicesSampleApp/package.json?range=5)]

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。
* [aspnet webpack](https://www.npmjs.com/package/aspnet-webpack) npm パッケージ。

    ```console
    npm i -D aspnet-webpack
    ```

### <a name="configuration"></a>構成

次のコードを使用して HTTP 要求パイプラインに Webpack デベロッパー ミドルウェアが登録されている、 *Startup.cs*ファイルの`Configure`メソッド。

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=webpack-middleware-registration&highlight=4)]

`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります[静的ファイルをホストしている登録](xref:fundamentals/static-files)を介して、`UseStaticFiles`拡張メソッド。 セキュリティ上の理由には、開発モードでアプリを実行する場合にのみ、ミドルウェアを登録します。

*Webpack.config.js*ファイルの`output.publicPath`プロパティ、ミドルウェアを見るには、通知、`dist`フォルダーの変更。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,13-16)]

<a name="hot-module-replacement"></a>

## <a name="hot-module-replacement"></a>ホット モジュールの交換

Webpack を考えてみてください[ホット モジュールの交換](https://webpack.github.io/docs/hot-module-replacement-with-webpack.html)(HMR) 機能の発展として[Webpack デベロッパー ミドルウェア](#webpack-dev-middleware)です。 HMR で利点ではすべて同じしますが、変更をコンパイルした後、ページのコンテンツを自動的に更新することでさらに、開発ワークフローを合理化します。 現在のインメモリ状態と SPA のデバッグ セッションに影響するブラウザーの更新でこれを混同しないでください。 Webpack デベロッパー ミドルウェア サービスと、ブラウザーに変更をプッシュすることを意味すると、ブラウザーの間のライブ リンクがあります。

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。
* [webpack ホット ミドルウェア](https://www.npmjs.com/package/webpack-hot-middleware)npm パッケージ。

    ```console
    npm i -D webpack-hot-middleware
    ```

### <a name="configuration"></a>構成

MVC の HTTP 要求パイプライン内に HMR コンポーネントを登録する必要があります、`Configure`メソッド。

```csharp
app.UseWebpackDevMiddleware(new WebpackDevMiddlewareOptions {
    HotModuleReplacement = true
});
```

同様にも当てはまる[Webpack デベロッパー ミドルウェア](#webpack-dev-middleware)、`UseWebpackDevMiddleware`する前に、拡張メソッドを呼び出す必要があります、`UseStaticFiles`拡張メソッド。 セキュリティ上の理由には、開発モードでアプリを実行する場合にのみ、ミドルウェアを登録します。

*Webpack.config.js*ファイルを定義する必要があります、`plugins`場合でも、そのまま空の配列。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/webpack.config.js?range=6,25)]

ブラウザーでアプリを読み込んだ後に、開発者ツールのコンソール タブは、HMR アクティブ化の確認を提供します。

![ホット モジュールの交換接続メッセージ](spa-services/_static/hmr_connected.png)

<a name="routing-helpers"></a>

## <a name="routing-helpers"></a>ルーティングのヘルパー

ほとんどの ASP.NET Core ベース SPAs では、サーバー側のルーティングだけでなくクライアント側のルーティングを必要があります。 SPA と MVC ルーティングのシステムは、干渉なし個別に操作できます。 課題ただし、1 つのエッジ ケース装って: 404 HTTP 応答を識別します。

シナリオを検討してください、拡張子のないルートの`/some/page`を使用します。 要求しないパターン一致がサーバー側のルートにはそのパターンがクライアント側のルートを一致と仮定します。 受信した要求を考えてみましょう`/images/user-512.png`が一般に、サーバー上の画像ファイルを検索するが必要です。 要求されたリソース、そのパスと一致していない任意のサーバー側のルートまたは静的ファイル場合可能性は高くありませんそれをクライアント側のアプリケーションで処理と — 通常 404 HTTP ステータス コードを取得します。

### <a name="prerequisites"></a>必須コンポーネント

以下をインストールします。
* クライアント側ルーティング npm パッケージです。 例として角の使用。

    ```console
    npm i -S @angular/router
    ```

### <a name="configuration"></a>構成

名前付き拡張メソッド`MapSpaFallbackRoute`で使用される、`Configure`メソッド。

[!code-csharp[](../client-side/spa-services/sample/SpaServicesSampleApp/Startup.cs?name=mvc-routing-table&highlight=7-9)]

ヒント: ルートは、構成している順序で評価されます。 その結果、`default`パターンに一致する最初の前のコード例にルートが使用されます。

<a name="new-project-creation"></a>

## <a name="creating-a-new-project"></a>新しいプロジェクトを作成します。

JavaScriptServices には、構成済みのアプリケーション テンプレートが用意されています。 SpaServices は、さまざまなフレームワークおよび角、Aurelia、Knockout、対応、および Vue などのライブラリと連携して、これらのテンプレートで使用されます。

これらのテンプレートは、次のコマンドを実行して、.NET Core CLI を使用してインストールできます。

```console
dotnet new --install Microsoft.AspNetCore.SpaTemplates::*
```

使用可能な SPA テンプレートの一覧が表示されます。

| テンプレート                                 | 短い形式の名前 | 言語 | Tags        |
|:------------------------------------------|:-----------|:---------|:------------|
| 角度の MVC の ASP.NET Core             | angular    | [C#]     | Web/MVC/SPA |
| Aurelia で MVC の ASP.NET Core             | aurelia    | [C#]     | Web/MVC/SPA |
| Knockout.js で MVC の ASP.NET Core         | knockout   | [C#]     | Web/MVC/SPA |
| React.js で MVC の ASP.NET Core            | react      | [C#]     | Web/MVC/SPA |
| MVC の ASP.NET Core React.js と (続編)  | reactredux | [C#]     | Web/MVC/SPA |
| Vue.js で MVC の ASP.NET Core              | vue        | [C#]     | Web/MVC/SPA | 

SPA テンプレートのいずれかを使用して新しいプロジェクトを作成するには、**短い名前**でテンプレートの[dotnet 新しい](/dotnet/core/tools/dotnet-new)コマンド。 次のコマンドは、サーバー側用に構成された ASP.NET Core MVC と角運動のアプリケーションを作成します。

```console
dotnet new angular
```

<a name="runtime-config-mode"></a>

### <a name="set-the-runtime-configuration-mode"></a>ランタイム構成モードを設定します。

2 つのプライマリのランタイム構成モードがあります。
* **開発**:
    * デバッグを容易にするソース マップが含まれます。
    * パフォーマンスをクライアント側のコードを最適化しません。
* **実稼働**:
    * ソース マップを除外します。
    * バンドル化と縮小を使用してクライアント側のコードを最適化します。

ASP.NET Core という環境変数を使用して`ASPNETCORE_ENVIRONMENT`構成モードを格納します。 参照してください**[環境の設定](xref:fundamentals/environments#setting-the-environment)**詳細についてはします。

### <a name="running-with-net-core-cli"></a>.NET Core CLI で実行します。

プロジェクトのルートに、次のコマンドを実行して、必要な NuGet と npm パッケージを復元します。

```console
dotnet restore && npm i
```

構築し、アプリケーションを実行します。

```console
dotnet run
```

アプリケーションの起動時によると localhost 上、[ランタイム構成モード](#runtime-config-mode)です。 移動`http://localhost:5000`ブラウザーのランディング ページが表示されます。

### <a name="running-with-visual-studio-2017"></a>Visual Studio 2017 で実行します。

開く、 *.csproj*によって生成されたファイル、 [dotnet 新しい](/dotnet/core/tools/dotnet-new)コマンド。 必要な NuGet と npm パッケージは、プロジェクトを開く時に自動的に復元されます。 この復元処理は、数分かかる場合があります、アプリケーションが、完了時に実行できる状態にします。 実行緑のボタンまたはキーを押します`Ctrl + F5`、し、アプリケーションのランディング ページで、ブラウザーが開きます。 Localhost をに従ってでアプリケーションを実行、[ランタイム構成モード](#runtime-config-mode)です。 

<a name="app-testing"></a>

## <a name="testing-the-app"></a>アプリのテスト

SpaServices テンプレートは、事前構成を使用してクライアント側のテストを実行して[Karma](https://karma-runner.github.io/1.0/index.html)と[Jasmine](https://jasmine.github.io/)です。 Jasmine に対し、Karma テスト ランナーはこれらのテストには、人気のある単体テスト フレームワークを JavaScript 用です。 Karma が使用するよう構成、 [Webpack デベロッパー ミドルウェア](#webpack-dev-middleware)を開発者が停止し、変更されるたびに、テストを実行するため必要はありません。 テスト_ケースまたはテスト ケース自体に対して実行されるコードであるかどうか、テストが自動的に実行されます。

Angular アプリケーションを使用して、例として、2 つのジャスミン テスト_ケースが既に提供されている、`CounterComponent`で、 *counter.component.spec.ts*ファイル。

[!code-typescript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/app/components/counter/counter.component.spec.ts?range=15-28)]

プロジェクトのルートにコマンド プロンプトを開き、次のコマンドを実行します。

```console
npm test
```

スクリプトで定義された設定が読み取られます Karma テスト ランナーを起動する、 *karma.conf.js*ファイル。 その他の設定の間で、 *karma.conf.js*経由で実行するテスト ファイルを識別、`files`配列。

[!code-javascript[](../client-side/spa-services/sample/SpaServicesSampleApp/ClientApp/test/karma.conf.js?range=4-5,8-11)]

<a name="app-publishing"></a>

## <a name="publishing-the-application"></a>アプリケーションの発行

展開の準備完了のパッケージに生成されたクライアント側資産と公開済みの ASP.NET Core 成果物を組み合わせることは面倒なことができます。 さいわい、SpaServices がという名前のカスタム MSBuild ターゲットとそのパブリケーション全体のプロセスを統制`RunWebpack`:

[!code-xml[](../client-side/spa-services/sample/SpaServicesSampleApp/SpaServicesSampleApp.csproj?range=31-45)]

MSBuild ターゲットには、次の責任があります。
1. Npm パッケージを復元します。
1. サード パーティではクライアント側の資産の実稼働と同等のビルドを作成します。
1. カスタムのクライアント側の資産の実稼働と同等のビルドを作成します。
1. Webpack で生成された資産を発行フォルダーにコピーします。

MSBuild ターゲットが実行するときに呼び出されます。

```console
dotnet publish -c Release
```

## <a name="additional-resources"></a>その他の技術情報

* [Angular Docs](https://angular.io/docs)
