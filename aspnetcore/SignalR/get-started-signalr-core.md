---
title: "ASP.NET Core の SignalR を概要します。"
author: rachelappel
description: "このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。"
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/16/2018
ms.prod: aspnet-core
ms.topic: tutorial
ms.technology: aspnet
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 139da5a2d0dadf51fece94b7c54ccd531e0ae8c2
ms.sourcegitcommit: 6548a3dd0cd1e3e92ac2310dee757ddad9fd6456
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/16/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>チュートリアル: SignalR による ASP.NET Core の開始します。

作成者: [Rachel Appel](https://twitter.com/rachelappel)

このチュートリアルでは ASP.NET Core の SignalR を使用してリアルタイム アプリの構築の基礎を説明します。

   ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)

このチュートリアルでは、次の SignalR 開発タスクについて説明します。

> [!div class="checklist"]
> * ASP.NET Core web アプリで、SignalR を作成します。
> * クライアントにコンテンツをプッシュする SignalR ハブを作成します。
> * 変更、`Startup`クラスし、アプリを構成します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

# <a name="prerequisites"></a>必須コンポーネント

次のソフトウェアをインストールします。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 またはそれ以降のバージョン、 **ASP.NET および web 開発**ワークロード
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降
* [Visual Studio Code](https://code.visualstudio.com/download) 
* [Visual Studio のコードの C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成します。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 使用して、**ファイル** > **新しいプロジェクト**メニュー オプションを**ASP.NET Core Web アプリケーション**です。 プロジェクトに名前を*SignalRChat*です。

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. 選択**Web アプリケーション**Razor ページを使用してプロジェクトを作成します。 選択し、 **OK**です。 必ず**ASP.NET Core 2.1** SignalR は、.NET の以前のバージョンが実行される場合は、framework セレクターから選択します。

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

3. プロジェクトを右クリックして**ソリューション エクスプ ローラー** > **追加** > **新しい項目の** > **npm 構成ファイル**. ファイルの名前を付けます*package.json*です。

4. 次のコマンドを実行、 **Package Manager Console**ウィンドウで、プロジェクトのルートから。

    ```console
      npm install @aspnet/signalr
    ```
5. コピー、 *signalr.js*ファイルから*node_modules\\ @aspnet\signalr\dist\browser* を*wwwroot\lib*プロジェクトのフォルダーにします。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. **統合ターミナル**、次のコマンドを実行します。
 
    ```console
      dotnet new razor -o SignalRChat
    ```

2. JavaScript クライアント ライブラリを使用して、インストール*npm*です。

    ```
      npm init -y
      npm install @aspnet/signalr
    ```

-----

## <a name="create-the-signalr-hub"></a>SignalR ハブを作成します。

ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインとして機能するクラスです。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. クラスを選択してプロジェクトに追加**ファイル** > **新規** > **ファイル**を選択して**Visual c# クラス**です。

1. 継承`Microsoft.AspNetCore.SignalR.Hub`です。 `Hub`プロパティおよび接続とグループ、さらに送信側と受信側のデータを管理するためのイベント クラスが含まれています。

1. 作成、`SendMessage`接続されているチャットのすべてのクライアントにメッセージを送信するメソッド。 返すことを確認、[タスク](https://msdn.microsoft.com/en-us/library/system.threading.tasks.task(v=vs.110).aspx)SignalR が非同期であるためです。 非同期コードの拡張性が優れています。

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. 開く、 *SignalRChat* Visual Studio のコード内のフォルダーです。

1. 選択して、プロジェクトにクラスを追加**ファイル** > **新しいファイル** メニューからです。

1. 継承`Microsoft.AspNetCore.SignalR.Hub`です。 `Hub`プロパティおよび接続とグループだけでなくクライアントに送信側と受信側のデータを管理するためのイベント クラスが含まれています。

1. `SendMessage` メソッドをクラスに追加します。 `SendMessage`メソッドは、接続されているチャットのすべてのクライアントにメッセージを送信します。 返すことを確認、[タスク](/dotnet/api/system.threading.tasks.task)SignalR が非同期であるためです。 非同期コードの拡張性が優れています。

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

-----

## <a name="configure-the-project-to-use-signalr"></a>SignalR を使用するプロジェクトを構成します。

SignalR に要求を渡すを認識できるように、SignalR のサーバーを構成する必要があります。

1. SignalR プロジェクトを構成するには、変更、プロジェクトの`Startup.ConfigureServices`メソッドです。

  `services.AddSignalR` 一部として SignalR を追加、[ミドルウェア](xref:fundamentals/middleware/index)パイプライン。

1. 使用して、ハブへのルートを構成する`UseSignalR`です。

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>SignalR クライアント コードを作成します。

1. コンテンツを置き換える*Pages\Index.cshtml*を次のコード。

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  上記の HTML では、名前とメッセージ フィールド、および送信ボタンが表示されます。 下部にあるスクリプト参照に注意してください: SignalR への参照と*chat.js*です。

1. という名前の JavaScript ファイルを追加*chat.js*を*wwwroot\js*フォルダーです。 これに次のコードを追加します。

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>アプリを実行する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 選択**デバッグ** > **デバッグなしで開始**ブラウザーを起動して、ローカルの web サイトを読み込みます。 アドレス バーから URL をコピーします。

1. 別のブラウザー インスタンス (任意のブラウザー) を開き、アドレス バーに URL を貼り付けます。

1. いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。 名前とメッセージ」は即座に両方のページに表示されます。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. **[デバッグ]** (F5) を押してプログラムをビルドし、実行します。 プログラムを実行すると、ブラウザー ウィンドウが開きます。

1. 別のブラウザー ウィンドウを開き、ローカルに web サイトをロードします。

1. いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。 名前とメッセージ」は即座に両方のページに表示されます。

-----

  ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)