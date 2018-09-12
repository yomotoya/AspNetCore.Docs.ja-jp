---
title: 'チュートリアル: ASP.NET Core 上で SignalR の使用を開始する'
author: tdykstra
description: このチュートリアルでは、ASP.NET Core 用 SignalR を使用するチャット アプリを作成します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 08/31/2018
uid: tutorials/signalr
ms.openlocfilehash: 6d96331a4630f766ca11edb056fd3e13b52b6ae4
ms.sourcegitcommit: 4cd8dce371d63a66d780e4af1baab2bcf9d61b24
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/06/2018
ms.locfileid: "43893166"
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

* **ASP.NET および Web 開発**ワークロードを含む [Visual Studio 2017](https://www.visualstudio.com/downloads/) バージョン 15.8 以降
* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

* [Visual Studio Code](https://code.visualstudio.com/download)
* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all)
* [Visual Studio Code 用 C#](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp)

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* [Visual Studio for Mac バージョン 7.5.4 以降](https://www.visualstudio.com/downloads/)
* [.NET Core SDK 2.1 以降](https://www.microsoft.com/net/download/all) (Visual Studio インストールに含まれている)

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

SignalR サーバー ライブラリは、[Microsoft.AspNetCore.App メタパッケージ](xref:fundamentals/metapackage-app) に含まれています。 JavaScript クライアント ライブラリはプロジェクトに自動的に含まれません。 このチュートリアルでは、[ライブラリ マネージャー (LibMan)](xref:client-side/libman/index) を使用して *unpkg* からクライアント ライブラリを取得します。 [unpkg](https://unpkg.com/#/) は、[npm (Node.js パッケージ マネージャー)](https://www.npmjs.com/get-npm) で見つかるものすべてを配信できる[コンテンツ配信ネットワーク](https://wikipedia.org/wiki/Content_delivery_network)です。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio/)

* **ソリューション エクスプローラー**で、プロジェクトを右クリックし、**[追加]** > **[クライアント側のライブラリ]** を選択します。

* **[Add Client-Side Library]\(クライアント側のライブラリの追加\)** ダイアログで、**[プロバイダー]** に対して **[unpkg]** を選択します。 

* **[ライブラリ]** に対して「_@aspnet/signalr@1_」を入力し、プレビューでない最新のバージョンを選択します。

  ![クライアント側のライブラリの追加ダイアログ - ライブラリの選択](signalr/_static/libman1.png)

* **[Choose specific files]\(特定のファイルの選択\)** を選択して *dist/browser* フォルダーを展開し、*signalr.js* と *signalr.min.js* を選択します。

* **[ターゲットの場所]** を *wwwroot/lib/signalr/* に設定して、**[インストール]** を選択します。

  ![クライアント側のライブラリの追加ダイアログ - ファイルと保存先の選択](signalr/_static/libman2.png)

  [LibMan](xref:client-side/libman/index) によって *wwwroot/lib/signalr* フォルダーが作成され、そこに選択したファイルがコピーされます。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code/)

* **[統合端末]** で、次のコマンドを実行して LibMan をインストールします。

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。

* 次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。 出力が表示されるまでに数秒待機する必要がある場合があります。

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  パラメーターによって次のオプションが指定されます。
  * unpkg プロバイダーを使用する。
  * ファイルを *wwwroot/lib/signalr* にコピーする。
  * 指定したファイルのみをコピーする。

  出力は次の例のようになります。

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

* **[端末]** で、次のコマンドを実行して LibMan をインストールします。

  ```console
  dotnet tool install -g Microsoft.Web.LibraryManager.Cli
  ```

* プロジェクト フォルダー (ファイル *SignalRChat.csproj* を含んでいるフォルダー) に移動します。

* 次のコマンドを実行して、LibMan を使用して SignalR クライアント ライブラリを取得します。

  ```console
  libman install @aspnet/signalr -p unpkg -d wwwroot\lib\signalr --files dist/browser/signalr.js --files dist/browser/signalr.min.js
  ```

  パラメーターによって次のオプションが指定されます。
  * unpkg プロバイダーを使用する。
  * ファイルを *wwwroot/lib/signalr* にコピーする。
  * 指定したファイルのみをコピーする。

  出力は次の例のようになります。

  ```console
  wwwroot/lib/signalr/dist/browser/signalr.js written to disk
  wwwroot/lib/signalr/dist/browser/signalr.min.js written to disk
  Installed library "@aspnet/signalr@1.0.3" to "wwwroot\lib\signalr"
  ```

---

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

* *Pages\Index.cshtml* のコンテンツを次のコードに変更します。

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
