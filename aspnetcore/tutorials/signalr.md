---
title: 'チュートリアル: ASP.NET Core 上で SignalR の使用を開始する'
author: tdykstra
description: このチュートリアルでは、ASP.NET Core 用 SignalR を使用するチャット アプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/20/2018
uid: tutorials/signalr
ms.openlocfilehash: a2573e2817a2d8921954264ca17bc3a7e2a010a8
ms.sourcegitcommit: 847cc1de5526ff42a7303491e6336c2dbdb45de4
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/27/2018
ms.locfileid: "43055833"
---
# <a name="tutorial-get-started-with-signalr-on-aspnet-core"></a>チュートリアル: ASP.NET Core 上で SignalR の使用を開始する

このチュートリアルでは、SignalR を使用してリアルタイム アプリをビルドするための基礎について説明します。 以下の方法について説明します。

> [!div class="checklist"]
> * ASP.NET Core 上で SignalR を使用する Web アプリを作成します。
> * サーバー上で SignalR ハブを作成します。
> * JavaScript クライアントから SignalR ハブに接続します。
> * ハブを使用して、任意のクライアントから、接続されているすべてのクライアントにメッセージを送信します。

最後には、次のように動作するチャット アプリが作成されます。

![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/signalr/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

## <a name="prerequisites"></a>必須コンポーネント

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.7.3 以降
* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)
* [npm](https://www.npmjs.com/get-npm) (SignalR JavaScript クライアント ライブラリに使用される、Node.js のパッケージ マネージャー)。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)
* [Visual Studio Code 用 C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)
* [npm](https://www.npmjs.com/get-npm) (SignalR JavaScript クライアント ライブラリに使用される、Node.js のパッケージ マネージャー)。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [Visual Studio for Mac バージョン 7.5.4 以降](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all) (Visual Studio インストールに含まれている)
* [npm](https://www.npmjs.com/get-npm) (SignalR JavaScript クライアント ライブラリに使用される、Node.js のパッケージ マネージャー)。

---

## <a name="create-the-project"></a>プロジェクトの作成

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* メニューから、**[ファイル]、[新規プロジェクト]** の順に選択します。

* **[新しいプロジェクト]** ダイアログで、**[インストール]、[Visual C#]、[Web]、[ASP.NET Core Web アプリケーション]** の順に選択します。 プロジェクトに "*SignalRChat*" という名前を付けます。

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-dialog.png)

* **[Web アプリケーション]** を選択して、Razor Pages を使用するプロジェクトを作成します。

* **.NET Core** のターゲット フレームワークを選択し、**ASP.NET Core 2.1** を選択して、**[OK]** をクリックします。

  ![Visual Studio の [新しいプロジェクト] ダイアログ ボックス](signalr/_static/signalr-new-project-choose-type.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* 新しいプロジェクトのために使用できるフォルダーを開きます。

* **[統合端末]** で、次のコマンドを実行します。

   ```console
   dotnet new webapp -o SignalRChat
   ```

   [!INCLUDE[](~/includes/webapp-alias-notice.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* メニューから、**[ファイル]、[新しいソリューション]** の順に選択します。

* **[.NET Core]、[アプリ]、[ASP.NET Core Web アプリ]** の順に選択します (**[ASP.NET Core Web アプリ (MVC)]** を選択しないでください)。

* **[次へ]** を選択します。

* プロジェクトに *SignalRChat* という名前を付けて、**[作成]** を選択します。

---

## <a name="add-the-signalr-client-library"></a>SignalR クライアント ライブラリを追加する

SignalR サーバー ライブラリは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) に含まれています。 しかし、JavaScript クライアント ライブラリについては、[npm (Node.js パッケージ マネージャー)](https://www.npmjs.com/get-npm) から取得する必要があります。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* **パッケージ マネージャー コンソール** (PMC) 内で、プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。

  ```console
  cd SignalRChat
  ```

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

2. 新しいプロジェクト フォルダーに移動します。

  ```console
  cd SignalRChat
  ``` 

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[端末]** で、プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。

---

* npm 初期化子を実行して、*package.json* ファイルを作成します。

  ```console
  npm init -y
  ```

  このコマンドによって次の例に示すような出力が作成されます。

  ```console
  Wrote to C:\tmp\SignalRChat\package.json:
  {
    "name": "SignalRChat",
    "version": "1.0.0",
    "description": "",
    "main": "index.js",
    "scripts": {
      "test": "echo \"Error: no test specified\" && exit 1"
    },
    "keywords": [],
    "author": "",
    "license": "ISC"0
  }
  ```

* クライアント ライブラリ パッケージをインストールします。

  ```console
  npm install @aspnet/signalr
  ```

  このコマンドによって次の例に示すような出力が作成されます。

  ```
  npm notice created a lockfile as package-lock.json. You should commit this file.
  npm WARN signalrchat@1.0.0 No description
  npm WARN signalrchat@1.0.0 No repository field.

  + @aspnet/signalr@1.0.2
  added 1 package in 0.98s
  ```

`npm install` コマンドでは、*node_modules* の下にあるサブフォルダーに JavaScript クライアント ライブラリがダウンロードされます。 それをそこからコピーして、チャット アプリの Web ページから参照できる *wwwroot* の下のフォルダーにコピーします。

* *wwwroot/lib* 内に *signalr* フォルダーを作成します。

* *signalr.js* ファイルを、*node_modules/@aspnet/signalr/dist/browser* から新しい *wwwroot/lib/signalr* フォルダーにコピーします。

## <a name="create-the-signalr-hub"></a>SignalR ハブを作成する

[ハブ](xref:signalr/hubs)はクライアント サーバー通信を処理するハイレベル パイプラインとして機能するクラスです。

* SignalRChat プロジェクト フォルダー内で、*Hubs* フォルダーを作成します。

* *Hubs* フォルダー内で、次のコードを使用して *ChatHub.cs* ファイルを作成します。

  [!code-csharp[Startup](signalr/sample/Hubs/ChatHub.cs)]

  `ChatHub` クラスは、SignalR [Hub](/dotnet/api/microsoft.aspnetcore.signalr.hub) クラスを継承します。 `Hub` クラスでは、接続、グループ、およびメッセージングが管理されます。

  `SendMessage` メソッドは、接続されている任意のクライアントによって呼び出すことができます。 それによって、受信したメッセージがすべてのクライアントに送信されます。 最大のスケーラビリティを実現するために、SignalR コードは非同期になっています。

## <a name="configure-the-project-to-use-signalr"></a>SignalR を使用するようにプロジェクトを構成する

SignalR 要求が SignalR に渡されるように SignalR サーバーを構成する必要があります。

* 次の強調表示されたコードを *Startup.cs* ファイルに追加します。

  [!code-csharp[Startup](signalr/sample/Startup.cs?highlight=7,33,52-55)]

  これらの変更によって、SignalR が[依存関係挿入](xref:fundamentals/dependency-injection)システムと[ミドルウェア](xref:fundamentals/middleware/index) パイプラインに追加されます。

## <a name="create-the-signalr-client-code"></a>SignalR クライアント コードを作成する

* *Pages\Index.cshtml* のコンテンツを次の内容に変更します。

  [!code-cshtml[Index](signalr/sample/Pages/Index.cshtml)]

  上のコードでは以下の操作が行われます。

  * 名前およびメッセージ テキスト用のテキスト ボックスと送信ボタンを作成します。
  * SignalR ハブから受信したメッセージを表示するために、`id="messagesList"` を使用してリストを作成します。
  * SignalR へのスクリプト参照と、次の手順で作成する *chat.js* アプリケーション コードを含めます。

* *wwwroot/js* フォルダー内に、次のコードを使用して *chat.js* ファイルを作成します。

  [!code-javascript[Index](signalr/sample/wwwroot/js/chat.js)]

  上のコードでは以下の操作が行われます。

  * 接続を作成して開始します。
  * ハブにメッセージを送信するハンドラーを送信ボタンに追加します。
  * ハブからメッセージを受信してからそれをリストに追加するハンドラーを接続オブジェクトに追加します。

## <a name="run-the-app"></a>アプリを実行する

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

* **Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* **Ctrl + F5** キーを押して、デバッグを行わずにアプリを実行します。

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* メニューから、**[実行]、[デバッグなしで開始]** の順に選択します。

---

* アドレス バーから URL をコピーし、別のブラウザー インスタンスまたはタブを開き、アドレス バーに URL を貼り付けます。

* いずれかのブラウザーを選択し、名前とメッセージを入力し、**[送信]** ボタンを選択します。

  次の瞬間、両方のページに名前とメッセージが表示されます。

  ![SignalR のサンプル アプリ](signalr/_static/signalr-get-started-finished.png)

> [!TIP]
> アプリが動作しない場合は、ご利用のブラウザーの開発者ツール (F12) を開き、コンソールに移動します。 HTML および JavaScript コードに関連するエラーが発生している場合があります。 たとえば、*signalr.js* を指示されたフォルダーとは別のフォルダーに配置したとします。 その場合、そのファイルへの参照は機能せず、コンソールに 404 エラーが表示されます。
> ![エラー "signalr.js が見つかりませんでした"](signalr/_static/f12-console.png)

## <a name="next-steps"></a>次の手順

クライアントが別のドメインから SignalR アプリに接続するようにしたい場合は、クロス オリジン リソース共有 (CORS) を有効にする必要です。 詳細については、「[クロス オリジン リソース共有](xref:signalr/security?view=aspnetcore-2.1#cross-origin-resource-sharing)」を参照してください。

SignalR、ハブ、および JavaScript クライアントの詳細については、次のリソースを参照してください。

* [ASP.NET Core 用 SignalR の概要](xref:signalr/introduction)
* [ASP.NET Core 用 SignalR 内でハブを使用する](xref:signalr/hubs)
* [ASP.NET Core SignalR JavaScript クライアント](xref:signalr/javascript-client)
