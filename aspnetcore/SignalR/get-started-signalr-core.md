---
title: "ASP.NET Core の SignalR を概要します。"
author: rachelappel
ms.author: rachelap
description: "このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。"
manager: wpickett
ms.date: 03/06/2018
ms.topic: tutorial
ms.technology: dotnet-signalr
ms.prod: aspnet-core
ms.custom: mvc
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 4afb9785fc3d0f472226a745537acbc77adefb4c
ms.sourcegitcommit: 9622bdc6326c28c3322c70000468a80ef21ad376
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/12/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a>チュートリアル: SignalR による ASP.NET Core の開始します。

作成者: [Rachel Appel](https://twitter.com/rachelappel)

このチュートリアルでは ASP.NET Core の SignalR を使用してリアルタイム アプリの構築の基礎を説明します。

   ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

このチュートリアルでは、次の SignalR 開発タスクについて説明します。

> [!div class="checklist"]
> * ASP.NET Core web アプリを作成します。
> * クライアントにコンテンツをプッシュする SignalR ハブを作成します。
> * SignalR JavaScript ライブラリを使用すると、メッセージを送信してハブからの更新プログラムを表示します。

## <a name="prerequisites"></a>必須コンポーネント

次のソフトウェアをインストールします。

* [.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)以降
* [Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 以降、ASP.NET と web 開発のワークロードでのバージョン
* [npm](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成します。

1. 使用して、**ファイル** > **新しいプロジェクト**メニュー オプションを**ASP.NET Core Web アプリケーション**です。 プロジェクトに `SignalRChat` という名前を付けます。

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. 選択**Web アプリケーション**Razor ページを使用してプロジェクトを作成します。 選択し、 **Ok**です。 必ず**ASP.NET Core 2.1** SignalR は、.NET の以前のバージョンが実行される場合は、framework セレクターから選択します。

  ![Visual Studio で新しいプロジェクト ダイアログ ボックス](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  プロジェクト テンプレートでは、SignalR のサーバー側コードをホストしているライブラリが含まれています。 インストールを別々 にクライアント側 JavaScript [npm](https://www.npmjs.com/)です。

  ```console
   npm install @aspnet/signalr
  ```

3. コピー、 *signalr.js*から*node_modules\\ @aspnet\signalr\dist\browser* を*wwwroot\lib*プロジェクトのフォルダーにします。

## <a name="create-the-signalr-hub"></a>SignalR ハブを作成します。

ハブは、クライアントとサーバーが互いにメソッドを呼び出すことができる高度なパイプラインとして機能するクラスです。

1. クラスを選択してプロジェクトに追加**ファイル** > **新規** > **ファイル**を選択して**Visual c# クラス**です。 

1. 継承`Microsoft.AspNetCore.SignalR.Hub`です。 `Hub`プロパティおよび接続とグループ、さらに送信側と受信側のデータを管理するためのイベント クラスが含まれています。

1. 作成、`Send`接続されているチャットのすべてのクライアントにメッセージを送信するメソッド。 返すことを確認、 `Task`SignalR が非同期であるためです。 非同期コードの拡張性が優れています。

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a>SignalR を使用するプロジェクトを構成します。

SignalR に要求を渡すを認識できるように、SignalR のサーバーを構成する必要があります。

1. SignalR プロジェクトを構成するのには、変更、`ConfigureServices`メソッドは、アプリケーションの`Startup`クラスへの呼び出しを挿入することで`services.AddSignalR`です。

  `services.AddSignalR` 一部として SignalR を追加、 [ASP.NET Core ミドルウェア](xref:fundamentals/middleware/index)パイプライン。

1. 使用して、ハブへのルートを構成する`UseSignalR`です。

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a>SignalR クライアント コードを作成します。

1. コンテンツを置き換える*Pages\Index.cshtml*を次のコード。

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  上記の HTML では、名前とメッセージ フィールド、および送信ボタンが表示されます。 下部にあるスクリプト参照に注意してください: SignalR への参照と*chat.js*です。

1. JavaScript ファイルを追加、 *wwwroot\js*という名前のフォルダー *chat.js*を次のコードを追加します。

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a>アプリを実行する

1. 選択**デバッグ** > **デバッグなしで開始**ブラウザーを起動して、ローカルの web サイトを読み込みます。 アドレス バーから URL をコピーします。

1. 別のブラウザー インスタンス (任意のブラウザー) を開き、アドレス バーに URL を貼り付けます。

1. いずれかのブラウザーを選択し、名前と、メッセージを入力してをクリックして、**送信**ボタンをクリックします。 名前とメッセージ」は即座に両方のページに表示されます。

  ![ソリューション](get-started-signalr-core/_static/signalr-get-started-finished.png)
