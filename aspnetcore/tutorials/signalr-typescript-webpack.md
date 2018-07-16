---
title: TypeScript と Webpack で ASP.NET Core SignalR を使用する
author: ssougnez
description: このチュートリアルでは、クライアントが TypeScript で記述された ASP.NET Core SignalR Web アプリをバンドルおよびビルドするために Webpack を構成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: scaddie
ms.custom: mvc
ms.date: 06/29/2018
uid: tutorials/signalr-typescript-webpack
ms.openlocfilehash: 6d52e915ab20aa69179585eceb497e8abd72c997
ms.sourcegitcommit: 661d30492d5ef7bbca4f7e709f40d8f3309d2dac
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/10/2018
ms.locfileid: "37938185"
---
# <a name="use-aspnet-core-signalr-with-typescript-and-webpack"></a>TypeScript と Webpack で ASP.NET Core SignalR を使用する

作成者: [Sébastien Sougnez](https://twitter.com/ssougnez)、[Scott Addie](https://twitter.com/Scott_Addie)

[Webpack](https://webpack.js.org/) を使用すると、開発者は Web アプリのクライアント側のリソースをバンドルおよびビルドすることができます。 このチュートリアルでは、クライアントが [TypeScript](https://www.typescriptlang.org/) で記述された ASP.NET Core SignalR Web アプリでの Webpack の使用法を示します。

このチュートリアルでは、次の作業を行う方法について説明します。

> [!div class="checklist"]
> * スターター ASP.NET Core SignalR アプリをスキャフォールディングする
> * SignalR TypeScript クライアントを構成する
> * Webpack を使用してビルド パイプラインを構成する
> * SignalR サーバーを構成する
> * クライアントとサーバー間の通信を有効にする

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/signalr-typescript-webpack/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

以下のソフトウェアをインストールします。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) ([npm](https://www.npmjs.com/) 使用)
* **ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7.3 以降

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)
* [Node.js](https://nodejs.org/) ([npm](https://www.npmjs.com/) 使用)

---

## <a name="create-the-aspnet-core-web-app"></a>ASP.NET Core Web アプリを作成する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

*PATH* 環境変数で npm を検索するように Visual Studio を構成します。 既定では、Visual Studio は、そのインストール ディレクトリ内で見つかった npm のバージョンを使用します。 Visual Studio のこれらの説明に従ってください。

1. **[ツール]** > **[オプション]** > **[プロジェクトおよびソリューション]** > **[Web パッケージ管理]** > **[外部 Web ツール]** の順に移動します。
1. リストから *[$(PATH)]* エントリを選択します。 上向き矢印をクリックして、エントリをリストの 2 番目の位置に移動します。 余談ですが、最初のエントリは、プロジェクトのローカル パッケージを参照しています。

    ![Visual Studio の構成](signalr-typescript-webpack/_static/signalr-configure-path-visual-studio.png)

Visual Studio の構成が完了しました。 次はプロジェクトを作成します。

1. **[ファイル]** > **[新規]** > **[プロジェクト]** メニュー オプション、**[ASP.NET Core Web アプリケーション]** テンプレートの順に選択します。
1. プロジェクトに *SignalRWebPack* という名前を付け、**[OK]** ボタンをクリックします。
1. ターゲット フレームワークのドロップダウンから、*[.NET Core]* を選択し、フレームワーク セレクターのドロップダウンから *[ASP.NET Core 2.1]* を選択します。 **空**のテンプレートを選択して、**[OK]** ボタンをクリックします。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

**統合端末**で次のコマンドを実行します。

```console
dotnet new web -o SignalRWebPack
```

.NET Core をターゲットとする、空の ASP.NET Core Web アプリが *SignalRWebPack* ディレクトリ内に作成されます。

---

## <a name="configure-webpack-and-typescript"></a>WebPack および TypeScript の構成

次の手順では、TypeScript の JavaScript への変換と、クライアント側のリソースのバンドルを構成します。

1. プロジェクト ルートで次のコマンドを実行して、*package.json* ファイルを作成します。

    ```console
    npm init -y
    ```

1. 強調表示されているプロパティを *package.json* ファイルに追加します。

    [!code-json[package.json](signalr-typescript-webpack/sample/snippets/package1.json?highlight=4)]

    `private` プロパティを `true` に設定して、次の手順でパッケージのインストールの警告が表示されないようにします。

1. 必要な npm パッケージをインストールします。 プロジェクト ルートで、次のコマンドを実行します。

    ```console
    npm install -D -E clean-webpack-plugin@0.1.19 css-loader@0.28.11 html-webpack-plugin@3.2.0 mini-css-extract-plugin@0.4.0 ts-loader@4.4.1 typescript@2.9.2 webpack@4.12.0 webpack-cli@3.0.6
    ```

    注目するべきコマンドの詳細:

    * バージョン番号は、各パッケージ名の `@` 記号の後に続きます。 npm によって、これらの特定のパッケージ バージョンがインストールされます。
    * `-E` オプションは、[セマンティック バージョニング](https://semver.org/)範囲演算子を *package.json* に書き込む npm の既定の動作を無効にします。 たとえば、`"webpack": "4.12.0"` が `"webpack": "^4.12.0"` の代わりに使用されています。 このオプションにより、新しいパッケージ バージョンへの予期しないアップグレードが防止されます。

    詳細については、[npm-install](https://docs.npmjs.com/cli/install) の公式ドキュメントを参照してください。

1. *package.json* ファイルの `scripts` プロパティを次のスニペットで置き換えます。

    ```json
    "scripts": {
      "build": "webpack --mode=development --watch",
      "release": "webpack --mode=production",
      "publish": "npm run release && dotnet publish -c Release"
    },
    ```

    スクリプトの説明 (一部):

    * `build`: 開発モードでクライアント側のリソースをバンドルし、ファイルの変更を監視します。 ファイル監視により、プロジェクト ファイルを変更するたびにバンドルが再生成されます。 `mode` オプションは、ツリー シェイキングや縮小などの運用環境の最適化を無効にします。 `build` は開発でのみ使用します。
    * `release`: 運用モードでクライアント側のリソースをバンドルします。
    * `publish`: `release` スクリプトを実行して、運用モードでクライアント側のリソースをバンドルします。 .NET Core CLI の [publish](/dotnet/core/tools/dotnet-publish) コマンドを呼び出してアプリを公開します。

1. プロジェクト ルートに、次の内容を持つ *webpack.config.js* という名前のファイルを作成します。

    [!code-javascript[webpack.config.js](signalr-typescript-webpack/sample/webpack.config.js)]

    上記のファイルは、Webpack コンパイルを構成します。 注目するべき構成の詳細 (一部):

    * `output` プロパティにより、*dist* の既定値がオーバーライドされます。 代わりにバンドルが *wwwroot* ディレクトリ内に生成されます。
    * `resolve.extensions` 配列には、SignalR クライアント JavaScript をインポートするための *.js* が含まれています。

1. プロジェクト ルートに新しい *src* プロジェクトを作成します。 その目的は、プロジェクトのクライアント側アセットを格納することです。

1. 次のコンテンツを含む *src/index.html* を作成します。

    [!code-html[index.html](signalr-typescript-webpack/sample/src/index.html)]

    上記の HTML では、ホームページの定型マークアップが定義されます。

1. 新しい *src/css* ディレクトリを作成します。 その目的は、プロジェクトの *.css* ファイルを格納することです。

1. 次のコンテンツを含む *src/css/main.css* を作成します。

    [!code-css[main.css](signalr-typescript-webpack/sample/src/css/main.css)]

    上記の *main.css* ファイルは、アプリをスタイル設定します。

1. 次のコンテンツを含む *src/tsconfig.json* を作成します。

    [!code-json[tsconfig.json](signalr-typescript-webpack/sample/src/tsconfig.json)]

    上記のコードは、[ECMAScript](https://wikipedia.org/wiki/ECMAScript) 5 対応の JavaScript を生成するように TypeScript コンパイラを構成します。

1. 次のコンテンツを含む *src/index.ts* を作成します。

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index1.ts?name=snippet_IndexTsPhase1File)]

    上記の TypeScript は、DOM 要素への参照を取得し、次の 2 つのイベント ハンドラーをアタッチします。

    * `keyup`: ユーザーがテキスト ボックスに `tbMessage` として識別されるものを入力すると、このイベントが発生します。 ユーザーが **Enter** キーを押すと、`send` 関数が呼び出されます。
    * `click`: ユーザーが **[送信]** ボタンをクリックすると、このイベントが発生します。 `send` 関数が呼び出されます。

## <a name="configure-the-aspnet-core-app"></a>ASP.NET Core アプリを構成する

1. `Startup.Configure` メソッドで提供されたコードにより、*Hello World!* が表示されます。 `app.Run` メソッド呼び出しを、[UseDefaultFiles](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) と [UseStaticFiles](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) の呼び出しで置き換えます。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseStaticDefaultFiles)]

    上記のコードにより、サーバーが *index.html* ファイルを見つけて提供することができます。ユーザーがファイルの完全な URL または Web アプリのルート URL を入力するかどうかは関係ありません。

1. `Startup.ConfigureServices` メソッドで [AddSignalR](/dotnet/api/microsoft.extensions.dependencyinjection.signalrdependencyinjectionextensions.addsignalr#Microsoft_Extensions_DependencyInjection_SignalRDependencyInjectionExtensions_AddSignalR_Microsoft_Extensions_DependencyInjection_IServiceCollection_) を呼び出します。 これにより SignalR サービスがプロジェクトに追加されます。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_AddSignalR)]

1. */hub* ルートを `ChatHub` ハブにマップします。 `Startup.Configure` メソッドの末尾に、次の行を追加します。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_UseSignalR)]

1. プロジェクト ルートに *Hubs* という新しいディレクトリを作成します。 その目的は、次の手順で作成される SignalR ハブを格納することです。

1. 次のコードを使用して、*Hubs/ChatHub.cs* を作成します。

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/snippets/ChatHub.cs?name=snippet_ChatHubStubClass)]

1. `ChatHub` 参照を解決するには、次のコードを *Startup.cs* ファイルの先頭に追加します。

    [!code-csharp[Startup](signalr-typescript-webpack/sample/Startup.cs?name=snippet_HubsNamespace)]

## <a name="enable-client-and-server-communication"></a>クライアントとサーバーの通信を有効にする

アプリには現在、メッセージを送信するための単純なフォームが表示されています。 送信しようとしても、何も起こりません。 サーバーは特定のルートをリッスンしていますが、メッセージの送信については何も行いません。

1. プロジェクト ルートで、次のコマンドを実行します。

    ```console
    npm install @aspnet/signalr
    ```

    上記のコマンドにより [SignalR TypeScript クライアント](https://www.npmjs.com/package/@aspnet/signalr) がインストールされ、クライアントがサーバーにメッセージを送信できるようになります。

1. 強調表示されたコードを *src/index.ts* ファイルに追加します。

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/snippets/index2.ts?name=snippet_IndexTsPhase2File&highlight=2,9-23)]

    上記のコードは、サーバーからのメッセージの受信をサポートします。 `HubConnectionBuilder` クラスは、サーバー接続を構成するための新しいビルダーを作成します。 `withUrl` 関数は、ハブ URL を構成します。

    SignalR により、クライアントとサーバー間でのメッセージのやり取りが可能になります。 各メッセージには特定の名前があります。 たとえば、メッセージ ゾーンに新しいメッセージを表示するためのロジックを実行する `messageReceived` という名前を持つメッセージを作成することができます。 特定のメッセージをリッスンするには、`on` 関数を使用します。 任意の数のメッセージ名をリッスンできます。 作成者の名前や受信したメッセージの内容など、パラメーターをメッセージに渡すこともできます。 クライアントがメッセージを受信すると、`innerHTML` 属性に作成者の名前とメッセージ コンテンツを持つ新しい `div` 要素が作成されます。 これはメッセージを表示する主要な `div` 要素に追加されます。

1. これでクライアントがメッセージを受信できるようになったので、メッセージを送信するように構成します。 強調表示されたコードを *src/index.ts* ファイルに追加します。

    [!code-typescript[index.ts](signalr-typescript-webpack/sample/src/index.ts?highlight=34-35)]

    WebSocket 接続を介してメッセージを送信するには、`send` メソッドを呼び出す必要があります。 メソッドの最初のパラメーターは、メッセージ名です。 メッセージ データは、他のパラメーターに存在しています。 この例では、`newMessage` として識別されたメッセージがサーバーに送信されます。 メッセージは、ユーザー名と、テキスト ボックスへのユーザー入力で構成されます。 送信が機能していると、テキスト ボックスの値はクリアされます。

1. 強調表示されているメソッドを `ChatHub` クラスに追加します。

    [!code-csharp[ChatHub](signalr-typescript-webpack/sample/Hubs/ChatHub.cs?highlight=8-11)]

    上記のコードは、サーバーがメッセージを受信すると、受信したメッセージをすべての接続されているユーザーにブロードキャストします。 すべてのメッセージを受信するのに、ジェネリック `on` メソッドは必要ありません。 メソッドはメッセージ名のサフィックスに基づいて名前が付けられています。

    この例では、TypeScript クライアントが `newMessage` として識別されるメッセージを送信します。 C# `NewMessage` メソッドは、データがクライアントから送信されることを想定しています。 [Clients.All](/dotnet/api/microsoft.aspnetcore.signalr.ihubclients-1.all) で [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync) メソッドが呼び出されます。 受信したメッセージは、ハブに接続されているすべてのクライアントに送信されます。

## <a name="test-the-app"></a>アプリのテスト

次の手順で、アプリの動作を確認します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. *リリース* モードで Webpack を実行します。 **パッケージ マネージャー コンソール** ウィンドウを使用して、プロジェクト ルートで次のコマンドを実行します。

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. **[デバッグ]** > **[デバッグなしで開始]** の順に選択して、デバッガーをアタッチせずに、ブラウザーでアプリを起動します。 *wwroot/index.html* ファイルは `http://localhost:<port_number>` で提供されます。

1. 別のブラウザー インスタンスを開きます (任意のブラウザー)。 アドレス バーに URL を貼り付けます。

1. いずれかのブラウザーを選択し、**[メッセージ]** テキスト ボックスにメッセージを入力し、**[送信]** ボタンをクリックします。 次の瞬間、両方のページに一意のユーザー名とメッセージが表示されます。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

1. プロジェクト ルートで次のコマンドを実行して、*リリース* モードで Webpack を実行します。

    [!INCLUDE [npm-run-release](../includes/signalr-typescript-webpack/npm-run-release.md)]

1. プロジェクト ルートで次のコマンドを実行して、アプリをビルドして実行します。

    ```console
    dotnet run
    ```

    Web サーバーは、アプリを起動して、localhost 上で使用できるようにします。

1. ブラウザーを開いて `http://localhost:<port_number>` にアクセスします。 *wwwroot/index.html* ファイルが提供されます。 アドレス バーから URL をコピーします。

1. 別のブラウザー インスタンスを開きます (任意のブラウザー)。 アドレス バーに URL を貼り付けます。

1. いずれかのブラウザーを選択し、**[メッセージ]** テキスト ボックスにメッセージを入力し、**[送信]** ボタンをクリックします。 次の瞬間、両方のページに一意のユーザー名とメッセージが表示されます。

---

![両方のブラウザー ウィンドウに表示されるメッセージ](signalr-typescript-webpack/_static/browsers-message-broadcast.png)

## <a name="additional-resources"></a>その他の技術情報

* <xref:signalr/javascript-client>
* <xref:signalr/hubs>
