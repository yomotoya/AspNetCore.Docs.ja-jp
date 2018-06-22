---
title: ASP.NET Core で Angular プロジェクト テンプレートを使用する
author: SteveSandersonMS
description: Angular と Angular CLI 用の ASP.NET Core シングル ページ アプリケーション (SPA) プロジェクト テンプレートの使用を開始する方法を説明します。
monikerRange: '>= aspnetcore-2.0'
ms.author: scaddie
ms.custom: mvc
ms.date: 02/21/2018
uid: spa/angular
ms.openlocfilehash: 811e28af2b67c356ff038d8d673e2164bb56578e
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36291465"
---
# <a name="use-the-angular-project-template-with-aspnet-core"></a>ASP.NET Core で Angular プロジェクト テンプレートを使用する

::: moniker range="= aspnetcore-2.0"

> [!NOTE]
> このドキュメントは、ASP.NET Core 2.0 に含まれている Angular プロジェクト テンプレートについて説明するものではありません。 手動で更新できる新しい Angular テンプレートについて説明するものです。 このテンプレートは、ASP.NET Core 2.1 に既定で含まれています。

::: moniker-end

更新された Angular プロジェクト テンプレートは、Angular と Angular CLI を使用してリッチなクライアント側ユーザー インターフェイス (UI) を実装する ASP.NET Core アプリのための便利な開始点を提供します。

このテンプレートは、API バックエンドとして機能する ASP.NET Core プロジェクトと、UI として機能する Angular CLI プロジェクトの作成に相当します。 このテンプレートは、1 つのアプリ プロジェクトの中で、両方のプロジェクトの種類をホストするという利便性を提供します。 その結果、アプリ プロジェクトを 1 つのユニットとしてビルドして発行できます。

## <a name="create-a-new-app"></a>新しいアプリを作成する

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 を使用している場合は、[更新された React プロジェクト テンプレートをインストールしている](xref:spa/index#installation)ことを確認します。

::: moniker-end
::: moniker range=">= aspnetcore-2.1"

ASP.NET Core 2.1 がインストールされている場合は、角運動のプロジェクト テンプレートをインストールする必要はありません。

::: moniker-end

コマンド プロンプトで `dotnet new angular` コマンドを使用して、空のディレクトリの中に新しいプロジェクトを作成します。 たとえば、次のコマンドは、*my-new-app* ディレクトリにアプリを作成し、そのディレクトリに切り替えます。

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Visual Studio または .NET Core CLI からアプリを実行します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

生成された *.csproj* ファイルを開き、そこから通常の方法でアプリを実行します。

ビルド プロセスは、初回の実行で npm の依存関係を復元します。これには数分かかる可能性があります。 以降のビルドは非常に高速になります。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli/)

値が `Development` である `ASPNETCORE_Environment` という名前の環境変数があることを確認します。 Windows では、PowerShell 以外のプロンプトで `SET ASPNETCORE_Environment=Development` を実行します。 Linux または macOS では、`export ASPNETCORE_Environment=Development` を実行します。

[dotnet build](/dotnet/core/tools/dotnet-build) を実行して、アプリが正しくビルドされていることを確認します。 ビルド プロセスは、初回の実行で npm の依存関係を復元します。これには数分かかる可能性があります。 以降のビルドは非常に高速になります。

[dotnet run](/dotnet/core/tools/dotnet-run) を実行してアプリを起動します。 次のようなメッセージがログに記録されます。

```console
Now listening on: http://localhost:<port>
```

ブラウザーでこの URL に移動します。

アプリは、Angular CLI サーバーのインスタンスをバックグラウンドで開始します。 次のようなメッセージがログに記録されます。*NG Live Development Server is listening on localhost:&lt;otherport&gt;, open your browser on http://localhost:&lt;otherport&gt;/*。 このメッセージは無視してください。それは、結合された ASP.NET Core と Angular CLI アプリの URLでは**ありません**。

---

プロジェクト テンプレートは、ASP.NET Core アプリと Angular アプリを作成します。 ASP.NET Core アプリは、データ アクセス、承認、およびその他のサーバー側の操作で使用することを目的としています。 *ClientApp* サブディレクトリ内の Angular アプリは、UI に関するすべての 操作で使用することを目的としています。

## <a name="add-pages-images-styles-modules-etc"></a>ページ、画像、スタイル、モジュールなどを追加する

*ClientApp* ディレクトリには、標準的な Angular CLI アプリが含まれます。 詳細については、公式の [Angular ドキュメント](https://github.com/angular/angular-cli/wiki)を参照してください。

このテンプレートによって作成される Angular アプリと Angular CLI 自体によって (`ng new` で) 作成されるアプリにはわずかな違いがありますが、アプリの機能は変わりません。 テンプレートによって作成されるアプリには、[ブートストラップ](https://getbootstrap.com/)ベースのレイアウトと基本的なルーティングの例が含まれます。

## <a name="run-ng-commands"></a>ng コマンドを実行する

コマンド プロンプトで、*ClientApp* サブディレクトリに切り替えます。

```console
cd ClientApp
```

`ng` ツールがグローバルにインストールされている場合は、そのコマンドのすべてを実行できます。 たとえば、`ng lint`、`ng test`、またはその他の [Angular CLI コマンド](https://github.com/angular/angular-cli/wiki#additional-commands) を実行できます。 ただし、`ng serve` を実行する必要はありません。これは、アプリのサーバー側とクライアント側の両方に関わる部分は、ASP.NET Core アプリが処理するためです。 内部的には、それは、開発時に `ng serve` を使用します。

`ng` ツールがインストールされていない場合は、代わりに `npm run ng` を実行します。 たとえば、`npm run ng lint` または `npm run ng test` を実行できます。

## <a name="install-npm-packages"></a>npm パッケージをインストールする

サードパーティ製の npm パッケージをインストールするには、*ClientApp* サブディレクトリでコマンド プロンプトを使用します。 例えば:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>発行と配置

開発中、アプリは、開発者に便利なように最適化されたモードで実行されます。 たとえば、JavaScript バンドルには、ソース マップが含まれます (デバッグ時に元の TypeScript コードを確認できるようにするためです)。 アプリは、ディスク上の TypeScript、HTML および CSS ファイルの変更を監視し、これらのファイルの変更が発生した場合は、再コンパイルと再読み込みを自動的に実行します。

運用時は、パフォーマンスが最適化されたバージョンのアプリが提供されます。 これは、自動的に実行されるように構成されています。 発行すると、ビルド構成によって、クライアント側コードの縮小された Ahead Of Time (AoT) コンパイルが行われたビルドが生成されます。 開発ビルドとは異なり、運用ビルドは、サーバーへの Node.js のインストールを必要としません ([サーバー側の事前レンダリング](#server-side-rendering)を有効にしている場合は除きます)。

標準的な [ASP.NET Core のホストと展開方法](xref:host-and-deploy/index)を使用できます。

## <a name="run-ng-serve-independently"></a>"ng serve" を個別に実行する

ASP.NET Core アプリが開発モードで起動された場合、プロジェクトは、Angular CLI サーバーの独自のインスタンスをバックグラウンドで開始するように構成されます。 これが便利なのは、別のサーバーを手動で実行する必要がないためです。

この既定の設定には欠点があります。 C# コードを変更し、ASP.NET Core アプリを再起動する必要がある場合、Angular CLI サーバーが毎回再起動します。 再起動には、約 10 秒必要です。 C# コードを何度も編集するが、Angular CLI が再起動するまで待ちたくない場合は、ASP.NET Core プロセスから独立した Angular CLI サーバーを外部で実行します。 次の手順に従います。

1. コマンド プロンプトで、*ClientApp* サブディレクトリに切り替え、Angular CLI 開発サーバーを起動します。

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > Angular CLI 開発サーバーを起動するには、`ng serve` ではなく `npm start` を使用して、*package.json* の構成が使用されるようにします。 Angular CLI サーバーに追加パラメーターを渡すには、*package.json* ファイルの関連する `scripts` 行にそれらを追加します。

2. 独自のインスタンスを起動する代わりに外部の Angular CLI インスタンスを使用するように ASP.NET Core アプリケーションを変更します。 *Startup* クラスで、`spa.UseAngularCliServer` の呼び出しを以下に置き換えます。

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

ASP.NET Core アプリの起動時に Angular CLI サーバーが起動されなくなります。 代わりに、手動で開始したインスタンスが使用されます。 これにより、起動と再起動を高速化できます。 Angular CLI がクライアント アプリを毎回リビルドするまで待つ必要はなくなります。

## <a name="server-side-rendering"></a>サーバー側のレンダリング

パフォーマンス機能として、クライアントでの実行に加え、サーバーでの Angular アプリの事前レンダリングを選択できます。 これは、ブラウザーがアプリの初期 UI を表す HTML マークアップを受信して、JavaScript バンドルをダウンロードして実行する前に UI を表示できることを意味します。 この実装の大半は、[Angular Universal](https://universal.angular.io/) と呼ばれる Angular 機能によって実現されます。

> [!TIP]
> サーバー側のレンダリング (SSR) を有効にすると、開発時と配置時の両方で作業が複雑になります。 「[SSR の欠点](#drawbacks-of-ssr)」を参照して、SSR が要件に適しているかどうかを判断してください。

SSR を有効にするには、プロジェクトに多数の追加を行う必要があります。

*Startup* クラスで、`spa.Options.SourcePath` を構成する行の "*後ろ*" で、`UseAngularCliServer` または `UseProxyToSpaDevelopmentServer` への呼び出しの "*前*" の位置に、以下を追加します。

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

開発モードでは、このコードは、*ClientApp\package.json* に定義されている `build:ssr` スクリプトを実行して、SSR バンドルのビルドを試みます。 これにより、`ssr` という名前の Angular アプリがビルドされますが、これはまだ定義されていません。

*ClientApp/.angular-cli.json* の `apps` 配列の最後に、`ssr` という名前の追加アプリを定義します。 次のオプションを使用します。

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

この新しい SSR 対応アプリの構成には、さらに 2 つのファイルが必要です (*tsconfig.server.json* と *main.server.ts*)。 *tsconfig.server.json* ファイルは、TypeScript のコンパイル オプションを指定します。 *main.server.ts* ファイルは、SSR 中のコードのエントリ ポイントとして機能します。

*tsconfig.server.json* という名前の新しいファイルを、*ClientApp/src* に (既存の *tsconfig.app.json* のそばに) 追加します。ファイルの内容は以下のとおりです。

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

このファイルは、`app.server.module` という名前のモジュールを検索する Angular の AoT コンパイラを構成します。 *ClientApp/src/app/app.server.module.ts* で (既存の *app.module.ts* のそばに) 以下の内容の新しいファイルを 作成することで、これを追加します。

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

このモジュールは、クライアント側の `app.module` を継承し、SSR 中に使用できる追加の Angular モジュールを定義します。

*.angular cli.json* 内の新しい `ssr` エントリは、*main.server.ts* という名前のエントリ ポイント ファイルを参照することを思い出してください。 そのファイルはまだ追加していません。ここで、それを実行します。 *ClientApp/src/main.server.ts* で (既存の *main.ts* のそばに) 以下の内容の新しいファイルを 作成します。

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

このファイルのコードは、ASP.NET Core が *Startup* クラスに追加された `UseSpaPrerendering` ミドルウェアを実行するときに、各要求で何を実行するかを指定します。 それは、.NET コードから `params` (要求されている URL など) の受信と、結果の HTML を取得するための Angular SSR API を呼び出しを処理します。

厳密に言うと、開発モードで SSR を有効にするにはこれで十分です。 アプリが発行されたときに正常に動作するように、最後に 1 つ変更を行うことが重要です。 アプリのメインの *.csproj* ファイルで、`BuildServerSideRenderer` プロパティの値を `true` に設定します。

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

これにより、発行時に `build:ssr` を実行し、SSR ファイルをサーバーに配置するようにビルド プロセスが構成されます。 これを有効にしない場合、SSR は運用環境では失敗します。

アプリを開発または運用モードで実行す場合、Angular コードは、サーバー上で HTML として事前レンダリングされます。 クライアント側のコードは、通常どおり実行されます。

### <a name="pass-data-from-net-code-into-typescript-code"></a>.NET コードから TypeScript コードに データを渡す

SSR 中は、要求ごとのデータを ASP.NET Core アプリから Angular アプリに渡すことができます。 たとえば、cookie 情報やデータベースから読み取ったデータを渡すことができます。 これを行うには、*Startup* クラスを編集します。 `UseSpaPrerendering` のコールバックに、次のような `options.SupplyData` の値を設定します。

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData` コールバックでは、任意の要求ごとの JSON でシリアル可能なデータ (文字列、ブール値、数値など) を渡すことができます。 *main.server.ts* コードは、これを `params.data` として受け取ります。 たとえば、上記のコード サンプルは、ブール値を `params.data.isHttpsRequest` として `createServerRenderer` コールバックに渡しています。 Angular でサポートされている方法で、アプリの他の部分にこれを渡すことができます。 たとえば、*main.server.ts* が、`BASE_URL` 値をコンストラクターが受け取ることを宣言されているコンポーネントにその値を渡す方法を参照してください。

### <a name="drawbacks-of-ssr"></a>SSR の欠点

すべてのアプリが SSR から利益を得られるわけではありません。 主な利益は、見かけ上のパフォーマンスです。 JavaScript バンドルのフェッチまたは解析にしばらく時間がかかる場合でも、低速のネットワーク接続または低速のモバイル デバイスでアプリに到達したユーザーに、初期 UI がすぐに表示されます。 ただし、多くの SPA は、主に高速の社内ネットワーク コンピューターで使用されているため、アプリはほぼ瞬時に表示されます。

同時に、SSR の有効化には重大な欠点があります。 それは、開発プロセスの複雑さを深めます。 ASP.NET Core から呼び出される Node.js 環境の中で、クライアント側とサーバー側 という 2 つの異なる環境でコードを実行する必要があります。 考慮すべき点を次に示します。

* SSR では、運用サーバー上に Node.js をインストールする必要があります。 これは、Azure App Services などの一部のデプロイ シナリオでは自動的に実行されますが、Azure Service Fabric などの他のシナリオではそうではありません。
* `BuildServerSideRenderer` ビルド フラグを有効にすると、*node_modules* ディレクトリが発行されます。 このフォルダーには、20,000 以上のファイルが含まれているため、配置時間が増加します。
* Node.js 環境でコードを実行するために、`window` や `localStorage` などのブラウザー固有の JavaScript API の存在に頼ることはできません。 コード (または参照された一部のサードパーティ製ライブラリ) がこれらの API を使用しようとすると、SSR 中にエラーが発生します。 たとえば、jQuery は多くの場所でブラウザー固有の API を参照するため、それを使用しないでください。 エラーを防ぐには、SSR を有効にしないか、ブラウザー固有の API またはライブラリを使用しないようにする必要があります。 SSR 中に呼び出されることがないように、このような API への呼び出しをチェックしてラップできます。 たとえば、JavaScript または TypeScript コードで、次のようなチェックを使用します。

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
