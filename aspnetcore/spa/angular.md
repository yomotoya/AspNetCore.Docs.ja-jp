---
title: "Angular プロジェクト テンプレートを使用します。"
author: SteveSandersonMS
description: "角速度と角運動の CLI for ASP.NET Core 単一ページ アプリケーション (SPA) リリース候補プロジェクト テンプレートを使って開始する方法を説明します。"
manager: wpickett
ms.author: scaddie
ms.custom: mvc
ms.date: 12/06/2017
ms.devlang: csharp
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: spa/angular
ms.openlocfilehash: 4162b1c26e9d278c811f691c4277d4de25adb204
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
# <a name="use-the-angular-project-template-release-candidate"></a>Angular プロジェクト テンプレート (リリース候補) を使用します。

> [!NOTE]
> このドキュメントは、リリースの角度プロジェクト テンプレートではありません。 **このドキュメントは、角度のテンプレートのリリース候補です。** 初期 2018 でリリースされたバージョンを出荷する予定です。

更新された角度のプロジェクト テンプレートは、角運動の 5 と角運動の CLI を使用して、クライアント側の豊富なユーザー インターフェイス (UI) を実装するアプリケーション、ASP.NET Core の便利な開始点を提供します。

テンプレートは、ASP.NET Core プロジェクト API バックエンドとして機能して、UI として機能する角度 CLI プロジェクトの作成と同じです。 テンプレートは、1 つのアプリ プロジェクトの両方のプロジェクト タイプをホストしているの利便性を提供します。 その結果、アプリ プロジェクトをビルドおよび 1 つの単位として公開します。

## <a name="create-a-new-app"></a>新しいアプリを作成します。

確認した結果を作業を開始する[角運動の更新されたプロジェクト テンプレートがインストールされている](xref:spa/index#installation)です。 これらの手順は、.NET Core で含まれている前の角度のプロジェクト テンプレートを適用しない 2.0.x SDK。

コマンドを使用してコマンド プロンプトから、新しいプロジェクトを作成する`dotnet new angular`空のディレクトリにします。 次のコマンドがでアプリを作成するなど、 *my-新しい-アプリ*ディレクトリとそのディレクトリへの切り替え。

```console
dotnet new angular -o my-new-app
cd my-new-app
```

Visual Studio または .NET Core CLI からアプリを実行します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

開く、生成された*.csproj*ファイル、およびそこから通常の方法でアプリを実行します。

ビルド プロセスでは、数分かかることが、最初の実行で npm の依存関係を復元します。 後続のビルドは非常に高速です。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

という環境変数があることを確認`ASPNETCORE_Environment`の値を持つ`Development`します。 Windows PowerShell 以外の入力を要求) の「、で実行される`SET ASPNETCORE_Environment=Development`です。 Linux または macOS、実行`export ASPNETCORE_Environment=Development`です。

実行`dotnet build`正しくビルドするのには、アプリを確認してください。 最初の実行には、ビルド プロセスは、数分かかることが npm の依存関係を復元します。 後続のビルドは非常に高速です。

実行`dotnet run`でアプリを起動します。 次のようなメッセージが記録されます。

```console
Now listening on: http://localhost:<port>
```

ブラウザーでこの URL に移動します。

アプリは、バック グラウンドで角度 CLI サーバーのインスタンスを開始します。 次のようなメッセージが記録されます: *NG ライブ開発サーバーは、localhost でリッスンしている:&lt;otherport&gt;、http://localhost にブラウザーを開きます:&lt;otherport&gt; /*. このメッセージは無視&mdash;が**いない**結合された ASP.NET Core と角運動 CLI アプリの URL。

---

プロジェクト テンプレートは、ASP.NET Core アプリケーションと角運動のアプリを作成します。 ASP.NET Core アプリケーションがデータ アクセス、承認、およびその他のサーバー側の懸念事項に使用するためのものです。 内に存在する角度のアプリ、 *ClientApp*サブディレクトリがすべての UI に関する注意事項を使用するためです。

## <a name="add-pages-images-styles-modules-etc"></a>ページ、画像、スタイル、モジュールなどを追加します。

*ClientApp*ディレクトリには、標準的な角度 CLI アプリが含まれています。 公式を参照してください[Angular ドキュメント](https://github.com/angular/angular-cli/wiki)詳細についてはします。

このテンプレートで作成した角度のアプリと角運動の CLI 自体によって作成された 1 つのわずかな違いがあります (を介して`ng new`)。 ただし、アプリの機能は変更されていません。 テンプレートによって作成されたアプリが含まれています、[ブートス トラップ](https://getbootstrap.com/)-ベースのレイアウトとルーティングの基本的な例です。

## <a name="run-ng-commands"></a>Ng コマンドを実行します

コマンド プロンプトでに切り替えると、 *ClientApp*サブディレクトリ。

```console
cd ClientApp
```

ある場合、`ng`グローバルにインストールされているツールを実行してそのコマンドのです。 たとえば、実行`ng lint`、 `ng test`、またはその他の[Angular CLI コマンド](https://github.com/angular/angular-cli/wiki#additional-commands)です。 実行する必要はありません`ng serve`しかし、ASP.NET Core アプリケーションがサービス中のアプリのサーバー側とクライアント側の両方の部分を処理するためです。 使用して内部的には、`ng serve`開発します。

いない場合、`ng`ツールをインストールするには、実行`npm run ng`代わりにします。 たとえば、実行`npm run ng lint`または`npm run ng test`です。

## <a name="install-npm-packages"></a>Npm パッケージをインストールします。

サード パーティ製の npm パッケージをインストールするでコマンド プロンプトを使用して、 *ClientApp*サブディレクトリです。 例:

```console
cd ClientApp
npm install --save <package_name>
```

## <a name="publish-and-deploy"></a>発行と配置

開発中のアプリは、開発者の利便性の最適化モードで実行されます。 たとえば、JavaScript のバンドルは、(をデバッグする場合は、元の TypeScript コードを確認できます) をソース マップを含めます。 アプリはディスク上の TypeScript、HTML および CSS ファイルの変更を監視し、自動的にコンパイルされそれらのファイルの変更が発生したときに再読み込みされます。

実稼働環境でパフォーマンスが最適化された、アプリのバージョンを提供します。 これは、自動的に実行される構成されます。 パブリッシュすると、ビルド構成を生成、縮小された、先行時間 (AoT) は、クライアント側コードのビルドをコンパイルします。 運用環境ビルド開発ビルドとは異なり、サーバーにインストールされている Node.js を必要としない (有効にしている場合を除き、[サーバー側の事前](#server-side-rendering))。

標準を使用する[ASP.NET Core のホストと展開方法](xref:host-and-deploy/index)です。

## <a name="run-ng-serve-independently"></a>"Ng serve"を個別に実行します。

プロジェクトが ASP.NET Core アプリケーションの開発モードの開始時に、バック グラウンドで角度 CLI サーバーの独自のインスタンスを開始するように構成します。 これは、機能は、別のサーバーを手動で実行する必要がないため便利です。

この既定の設定の欠点があります。 C# コードとアプリを再起動する必要があります、ASP.NET Core を変更するたびに、角運動 CLI サーバーを再起動します。 バックアップを開始するには、約 10 秒が必要です。 頻繁に c# コードの編集を加えようとしての角度の CLI を再起動するまで待機しない場合は、ASP.NET Core プロセスとは無関係に Angular CLI server 外部でを実行します。 これを行うには。

1. コマンド プロンプトでに切り替えると、 *ClientApp*サブディレクトリ、および角度 CLI 開発サーバーの起動。

    ```console
    cd ClientApp
    npm start
    ```

    > [!IMPORTANT]
    > 使用して`npm start`いない Angular CLI 開発サーバーを起動する`ng serve`いるためで構成*package.json*尊重します。 Angular CLI サーバーに追加のパラメーターを渡すにそれらに関連する追加`scripts`線、 *package.json*ファイル。

2. 独自のいずれかを起動せずに外部の角度 CLI インスタンスを使用する ASP.NET Core アプリケーションを変更します。 *スタートアップ*クラス、置換、`spa.UseAngularCliServer`を次の呼び出し。

    ```csharp
    spa.UseProxyToSpaDevelopmentServer("http://localhost:4200");
    ```

ASP.NET Core アプリを起動するときに Angular CLI サーバーを起動ことはありません。 手動で開始したインスタンスが使用されます。 これにより、起動し、再起動を高速にできます。 Angular CLI たびに、クライアント アプリを再構築するを待機してが不要になった。

## <a name="server-side-rendering"></a>サーバー側のレンダリング

パフォーマンス機能としては、あらかじめサーバーだけでなく、クライアントで実行することで角運動のアプリを表示するために選択できます。 これは、ブラウザーがダウンロードして、JavaScript のバンドルを実行する前に表示するようにするために、アプリの初期の UI を表す HTML マークアップを受信することを意味します。 この実装の大部分と呼ばれる角度の機能から取得[Angular ユニバーサル](https://universal.angular.io/)です。

> [!TIP]
> サーバー側のレンダリングを有効にする (SSR) には、開発および展開時に余分な複雑さを両方の数が導入されています。 読み取り[SSR の欠点](#drawbacks-of-ssr)SSR が適合、要件を満たすかどうかを決定します。

SSR を有効にするには、多数のプロジェクトへの追加を行う必要があります。

*スタートアップ*クラス、*後*を構成する行`spa.Options.SourcePath`、および*する前に*への呼び出し`UseAngularCliServer`または`UseProxyToSpaDevelopmentServer`次の追加。

[!code-csharp[](sample/AngularServerSideRendering/Startup.cs?name=snippet_Call_UseSpa&highlight=5-12)]

開発モードでこのコードがスクリプトを実行して、SSR バンドルが作成されます`build:ssr`で定義されている*ClientApp\package.json*です。 これにより、ビルドという名前の角度アプリ`ssr`がまだ定義されていません。 

最後に、`apps`配列*ClientApp/.angular-cli.json*、名前の余分なアプリを定義する`ssr`です。 次のオプションを使用します。

[!code-json[](sample/AngularServerSideRendering/ClientApp/.angular-cli.json?range=24-41)]

この新しい SSR 対応アプリの構成には、さらに 2 つのファイルが必要です: *tsconfig.server.json*と*main.server.ts*です。 *Tsconfig.server.json*ファイルは、TypeScript のコンパイル オプションを指定します。 *Main.server.ts* SSR 中に、ファイルがコードのエントリ ポイントとして機能します。

という名前の新しいファイルを追加*tsconfig.server.json*内*ClientApp/src* (既存と共に*tsconfig.app.json*)、次を含みます。

[!code-json[](sample/AngularServerSideRendering/ClientApp/src/tsconfig.server.json)]

このファイルの構成と呼ばれるモジュールを検索する角の AoT コンパイラ`app.server.module`です。 これで新しいファイルを作成して追加*ClientApp/src/app/app.server.module.ts* (既存と共に*app.module.ts*)、次を含みます。 

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/app/app.server.module.ts)]

このモジュールは、クライアント側から継承`app.module`SSR 中に使用可能な余分な Angular モジュールを定義しています。

注意してください、新しい`ssr`内のエントリ*.angular cli.json*というエントリ ポイント ファイルを参照*main.server.ts*です。 そのファイルをまだ追加していないし、ここは、これを行うにします。 新しいファイルを作成*ClientApp/src/main.server.ts* (既存と共に*main.ts*)、次を含みます。

[!code-typescript[](sample/AngularServerSideRendering/ClientApp/src/main.server.ts)]

このファイルのコードはどのような ASP.NET Core 実行要求ごとに実行すると、`UseSpaPrerendering`に追加したミドルウェア、*スタートアップ*クラスです。 受信を扱う`params`(など、URL 要求されている)、.NET コードと結果の HTML を取得する角度 SSR Api を呼び出すからです。 

厳密に言うと、これは開発モードで SSR を有効にするだけで十分です。 1 つの最終的な変更がようにするアプリが正常に発行する際に重要です。 アプリのメインの*.csproj*ファイル、設定、`BuildServerSideRenderer`プロパティの値を`true`:

[!code-xml[](sample/AngularServerSideRendering/AngularServerSideRendering.csproj?name=snippet_EnableBuildServerSideRenderer)]

これにより、構成、ビルド プロセスの実行を`build:ssr`発行時に SSR ファイルをサーバーに配置するとします。 これを有効にしない場合は、実稼働環境で SSR が失敗します。

アプリを開発または実稼働モードで実行すると角運動のコードでは、事前としてレンダリング HTML サーバーにします。 クライアント側のコードは、通常どおり実行します。

### <a name="pass-data-from-net-code-into-typescript-code"></a>TypeScript コードに .NET コードからデータを渡す

SSR、中に、要求ごとのデータを ASP.NET Core アプリから Angular アプリに渡すことができます。 たとえば、cookie 情報を渡すことがまたはデータベースから読み取られたものです。 これを行うには、編集、*スタートアップ*クラスです。 コールバックで`UseSpaPrerendering`の値を設定`options.SupplyData`次など。

```csharp
options.SupplyData = (context, data) =>
{
    // Creates a new value called isHttpsRequest that's passed to TypeScript code
    data["isHttpsRequest"] = context.Request.IsHttps;
};
```

`SupplyData`任意渡すコールバックにより、要求ごとの JSON でシリアル化データ (例、文字列、ブール値、または数値)。 *Main.server.ts*コードを受け取るとして`params.data`です。 たとえば、上記のコード サンプルにとしてブール値が渡されます`params.data.isHttpsRequest`に、`createServerRenderer`コールバック。 これは、角でサポートされる任意の方法で、アプリの他の部分を渡すことができます。 たとえばを参照してください方法*main.server.ts*渡します、`BASE_URL`それを受信するコンス トラクターが宣言されたどのコンポーネントの値。

### <a name="drawbacks-of-ssr"></a>SSR の欠点

すべてのアプリ SSR を享受できます。 主な利点は、見かけ上のパフォーマンスです。 フェッチまたは JavaScript のバンドルを解析する、時間がかかる場合でも、訪問者が低速ネットワーク接続または低速のモバイル デバイスでアプリに到達するを迅速に、初期の UI 参照してください。 ただし、多くの SPAs は主に使用高速なコンピューターに高速、内部の会社のネットワーク経由でアプリがほぼ瞬時に表示します。

同時には、SSR を有効にする重要な欠点があります。 複雑さを開発プロセスに追加します。 2 つの環境でコードを実行する必要があります。 クライアント側およびサーバー側 (Node.js 環境で ASP.NET Core から呼び出されます。 念頭にいくつかの点を次に示します。

* SSR には、実稼働サーバー上の Node.js のインストールが必要です。 これは、Azure App Services などの一部の展開シナリオについては、Azure Service Fabric など、他のケースでは自動的にです。
* 有効にすると、`BuildServerSideRenderer`フラグの原因をビルド、 *node_modules*を公開するディレクトリ。 このフォルダーには、20,000 + ファイル、配置時間が増加するが含まれています。
* Node.js 環境で、コードを実行することに依存できないブラウザー固有の JavaScript Api の存在など`window`または`localStorage`です。 コード (または参照するいくつかのサード パーティ製ライブラリ) は、これらの Api を使用すると、SSR 中にエラーが表示されます。 多くの場所でブラウザー固有の Api を参照しているために、jQuery がなど、使用しないでください。 エラーを防ぐためには、SSR を回避するか、または、ブラウザーに固有の Api またはライブラリを回避する必要があります。 SSR 中に呼び出すことがないことを確認するためのチェックでこのような Api への呼び出しをラップすることができます。 たとえば、JavaScript または TypeScript コードで、次のようなチェックを使用します。

    ```javascript
    if (typeof window !== 'undefined') {
        // Call browser-specific APIs here
    }
    ```
