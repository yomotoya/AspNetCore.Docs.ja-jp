---
title: ASP.NET Core の SignalR 概要
author: rachelappel
description: このチュートリアルでは、ASP.NET Core の SignalR を使用してアプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/22/2018
uid: tutorials/signalr
ms.openlocfilehash: 6b8222ee04573ca7157b4e1125ed5a4453b2b9a9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37830556"
---
# <a name="get-started-with-signalr-on-aspnet-core"></a>ASP.NET Core の SignalR 概要

作成者: [Rachel Appel](https://twitter.com/rachelappel)

このチュートリアルでは、ASP.NET Core の SignalR を使用してリアルタイム アプリをビルドするための基本について説明します。

   ![ソリューション](signalr/_static/signalr-get-started-finished.png)

このチュートリアルでは、次の SignalR 開発タスクを行います。

> [!div class="checklist"]
> * ASP.NET Core Web アプリで SignalR を作成します。
> * クライアントにコンテンツをプッシュする SignalR ハブを作成します。
> * `Startup` クラスを変更し、アプリを構成します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

以下のソフトウェアをインストールします。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)
* **ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7.3 以降
* [npm](https://www.npmjs.com/get-npm)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)
* [Visual Studio Code](https://code.visualstudio.com/download)
* [Visual Studio Code 用 C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm)

-----

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a>SignalR クライアントとサーバーをホストする ASP.NET Core プロジェクトを作成する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. **[ファイル]** > **[新しいプロジェクト]** メニュー オプション、**[ASP.NET Core Web アプリケーション]** の順に選択します。 プロジェクトに "*SignalRChat*" という名前を付けます。

   ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

2. **[Web アプリケーション]** を選択し、Razor Pages を使用してプロジェクトを作成します。 **[OK]** を選択します。 SignalR は .NET. のより前のバージョンで動作しますが、フレームワークには **[ASP.NET Core 2.1]** を選択してください。

   ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

Visual Studio には、その **ASP.NET Core Web アプリケーション** テンプレートの一部としてそのサーバー ライブラリを含む `Microsoft.AspNetCore.SignalR` パッケージが含まれています。 ただし、SignalR の JavaScript クライアント ライブラリは *npm* を使用してインストールする必要があります。

3. **パッケージ マネージャー コンソール** ウィンドウでプロジェクト ルートから次のコマンドを実行します。

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

4. プロジェクトの *lib* フォルダー内に "signalr" という名前のフォルダーを新しく作成します。 *node_modules\\@aspnet\signalr\dist\browser* からこのフォルダーに *signalr.js* ファイルをコピーします。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. **統合端末**から次のコマンドを実行します。

    ```console
    dotnet new webapp -o SignalRChat
    ```

    [!INCLUDE[](~/includes/webapp-alias-notice.md)]

2. *npm* を使用して JavaScript クライアント ライブラリをインストールします。

    ```console
    npm init -y
    npm install @aspnet/signalr
    ```

3. プロジェクトの *lib* フォルダー内に "signalr" という名前のフォルダーを新しく作成します。 *node_modules\\@aspnet\signalr\dist\browser* からこのフォルダーに *signalr.js* ファイルをコピーします。

---

## <a name="create-the-signalr-hub"></a>SignalR ハブを作成する

ハブはハイレベル パイプラインとして機能するクラスであり、クライアントとサーバーが相互にメソッドを呼び出すことを可能にします。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

1. **[ファイル]** > **[新規]** > **[ファイル]**、**[Visual C# クラス]** の順に選択し、プロジェクトにクラスを追加します。 クラスに `ChatHub`、ファイルに *ChatHub.cs* と名前を付けます。

2. `Microsoft.AspNetCore.SignalR.Hub` から継承します。 `Hub` クラスには、接続とグループを管理し、データを送受信するためのプロパティとイベントが含まれています。

3. 接続されているすべてのチャット クライアントにメッセージを送信する `SendMessage` メソッドを作成します。 SignalR は非同期であるため、[Task](https://msdn.microsoft.com/library/system.threading.tasks.task(v=vs.110).aspx) が返されます。 非同期コードは拡張性に優れています。

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

1. Visual Studio Code で *SignalRChat* フォルダーを開きます。

2. メニューから **[ファイル]** > **[新しいファイル]** の順に選択し、プロジェクトにクラスを追加します。 クラスに `ChatHub`、ファイルに *ChatHub.cs* と名前を付けます。

3. `Microsoft.AspNetCore.SignalR.Hub` から継承します。 `Hub` クラスには、接続とグループを管理し、クライアントとの間でデータを送受信するためのプロパティとイベントが含まれています。

4. `SendMessage` メソッドをクラスに追加します。 `SendMessage` メソッドは、接続されているすべてのチャット クライアントにメッセージを送信します。 SignalR は非同期であるため、[Task](/dotnet/api/system.threading.tasks.task) が返されます。 非同期コードは拡張性に優れています。

   [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

-----

## <a name="configure-the-project-to-use-signalr"></a>SignalR を使用するようにプロジェクトを構成する

SignalR に要求を渡すように SignalR のサーバーを構成する必要があります。

1. SignalR プロジェクトを構成するには、プロジェクトの `Startup.ConfigureServices` メソッドを変更します。

   `services.AddSignalR` は、[ 依存関係の挿入](xref:fundamentals/dependency-injection)システムで SignalR サービスを使用できるようにします。

1. `Configure` メソッドで `UseSignalR` を使用して、ハブへのルートを構成します。 `app.UseSignalR` は、[ミドルウェア](xref:fundamentals/middleware/index) パイプラインに SignalR を追加します。

   [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=37,57-60)]

## <a name="create-the-signalr-client-code"></a>SignalR クライアント コードを作成する

1. "*chat.js*" という名前の JavaScript ファイルを *wwwroot\js* フォルダーに追加します。 これに次のコードを追加します。

   [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

2. *Pages\Index.cshtml* のコンテンツを次のコードに変更します。

   [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

   上記の HTML では、名前フィールド、メッセージ フィールド、送信ボタンが表示されます。 一番下のスクリプト参照をご覧ください。SignalR と *chat.js* が参照されています。

## <a name="run-the-app"></a>アプリを実行する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. **[デバッグ]** > **[デバッグなしで開始]** の順に選択し、ブラウザーを起動し、Web サイトをローカルで読み込みます。 アドレス バーから URL をコピーします。

1. 別のブラウザー インスタンスを開き (どのブラウザーでもかまいません)、アドレス バーに URL を貼り付けます。

1. いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンをクリックします。 次の瞬間、両方のページに名前とメッセージが表示されます。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

1. **[デバッグ]** (F5) を押してプログラムをビルドし、実行します。 プログラムを実行すると、ブラウザー ウィンドウが開きます。

1. 別のブラウザー ウィンドウを開き、ローカルで Web サイトを読み込みます。

1. いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンをクリックします。 次の瞬間、両方のページに名前とメッセージが表示されます。

---

  ![ソリューション](signalr/_static/signalr-get-started-finished.png)

## <a name="related-resources"></a>関連資料

[ASP.NET Core SignalR の概要](xref:signalr/introduction)
