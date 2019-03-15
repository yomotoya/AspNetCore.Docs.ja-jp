---
title: ASP.NET Core SignalR JavaScript クライアント
author: bradygaster
description: ASP.NET Core SignalR JavaScript クライアントの概要です。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 03/14/2019
uid: signalr/javascript-client
ms.openlocfilehash: a0980dca2eb8d483a9d9f1c5667fb74ee06364f0
ms.sourcegitcommit: d913bca90373c07f89b1d1df01af5fc01fc908ef
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/14/2019
ms.locfileid: "57978343"
---
# <a name="aspnet-core-signalr-javascript-client"></a>ASP.NET Core SignalR JavaScript クライアント

作成者: [Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR JavaScript クライアント ライブラリでは、サーバー側ハブのコードを呼び出すことができます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/javascript-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

## <a name="install-the-signalr-client-package"></a>SignalR クライアント パッケージをインストールします。

SignalR JavaScript クライアント ライブラリとして提供される、 [npm](https://www.npmjs.com/)パッケージ。 Visual Studio を使用している場合は、実行`npm install`から、**パッケージ マネージャー コンソール**ルート フォルダー内で。 Visual Studio Code でからのコマンドを実行、**統合ターミナル**します。

  ```console
  npm init -y
  npm install @aspnet/signalr
  ```

npm のインストール パッケージの内容を *node_modules\\@aspnet\signalr\dist\browser* フォルダー。 という名前の新しいフォルダーを作成する*signalr*下、 *wwwroot\\lib*フォルダー。 コピー、 *signalr.js*ファイルを*wwwroot\lib\signalr*フォルダー。

## <a name="use-the-signalr-javascript-client"></a>SignalR JavaScript クライアントを使用します。

SignalR JavaScript クライアントでの参照、`<script>`要素。

```html
<script src="~/lib/signalr/signalr.js"></script>
```

## <a name="connect-to-a-hub"></a>ハブへの接続します。

次のコードでは、作成し、接続を開始します。 ハブの名前は大文字小文字を区別します。

[!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=9-13,43-45)]

### <a name="cross-origin-connections"></a>クロス オリジンの接続

通常、ブラウザーでは、要求されたページと同じドメインから接続を読み込みます。 ただし、状況が別のドメインへの接続が必要な場合があります。

悪意のあるサイトが別のサイトから機密データを読み取ることを防ぐために[クロス オリジン接続](xref:security/cors)は既定で無効になります。 クロス オリジン要求を許可することで有効にする、`Startup`クラス。

[!code-csharp[Cross-origin connections](javascript-client/sample/Startup.cs?highlight=29-35,56)]

## <a name="call-hub-methods-from-client"></a>クライアントからのハブ メソッドの呼び出し

JavaScript クライアントは、ハブ経由でのパブリック メソッドを呼び出して、[呼び出す](/javascript/api/%40aspnet/signalr/hubconnection#invoke)のメソッド、 [HubConnection](/javascript/api/%40aspnet/signalr/hubconnection)します。 `invoke`メソッドは 2 つの引数を受け取ります。

* ハブ メソッドの名前。 次の例では、ハブのメソッド名は`SendMessage`します。
* ハブ メソッドで定義されている引数。 次の例では引数名は`message`します。 コード例では、現在のバージョンの Internet Explorer を除くすべての主要ブラウザーでサポートされている矢印関数の構文を使用します。

  [!code-javascript[Call hub methods](javascript-client/sample/wwwroot/js/chat.js?range=24)]

> [!NOTE]
> Azure SignalR サービスを使用している場合*サーバーレス モード*、クライアントからハブ メソッドを呼び出すことはできません。 詳細については、次を参照してください。、 [SignalR サービスのドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)します。

## <a name="call-client-methods-from-hub"></a>ハブからのクライアント メソッドを呼び出す

ハブからメッセージを受信する定義を使用して、メソッド、[で](/javascript/api/%40aspnet/signalr/hubconnection#on)のメソッド、`HubConnection`します。

* JavaScript クライアント メソッドの名前。 次の例では、メソッド名は`ReceiveMessage`します。
* 引数は、hub は、メソッドに渡します。 引数の値は、次の例では、`message`します。

[!code-javascript[Receive calls from hub](javascript-client/sample/wwwroot/js/chat.js?range=14-19)]

上記のコードで`connection.on`を使用してサーバー側コードから呼び出すときに実行される、 [SendAsync](/dotnet/api/microsoft.aspnetcore.signalr.clientproxyextensions.sendasync)メソッド。

[!code-csharp[Call client-side](javascript-client/sample/hubs/chathub.cs?range=8-11)]

SignalR を呼び出すメソッド名を照合することによってクライアントの方法を指定してで定義されている引数`SendAsync`と`connection.on`します。

> [!NOTE]
> ベスト プラクティスを呼び出して、[開始](/javascript/api/%40aspnet/signalr/hubconnection#start)メソッドを`HubConnection`後`on`します。 これにより、すべてのメッセージを受信する前に、ハンドラーが登録されます。

## <a name="error-handling-and-logging"></a>エラー処理とログ記録

チェーンを`catch`メソッドの末尾に、`start`クライアント側のエラーを処理するメソッド。 使用`console.error`ブラウザーのコンソールにエラーを出力します。

[!code-javascript[Error handling](javascript-client/sample/wwwroot/js/chat.js?range=49-51)]

接続が行われたときにログに記録するには、logger とイベントの種類を渡すことによって、クライアント側のログ トレースをセットアップします。 指定されたログ レベル以降、メッセージが記録されます。 使用可能なログ レベルは次のとおりです。

* `signalR.LogLevel.Error` &ndash; エラー メッセージ。 ログ`Error`メッセージのみです。
* `signalR.LogLevel.Warning` &ndash; 可能性のあるエラーについての警告メッセージ。 ログ`Warning`、および`Error`メッセージ。
* `signalR.LogLevel.Information` &ndash; ステータス メッセージがエラーなし。 ログ`Information`、 `Warning`、および`Error`メッセージ。
* `signalR.LogLevel.Trace` &ndash; メッセージをトレースします。 ハブとクライアント間で転送されるデータを含め、すべてログに記録します。

使用して、 [configureLogging](/javascript/api/%40aspnet/signalr/hubconnectionbuilder#configurelogging)メソッド[HubConnectionBuilder](/javascript/api/%40aspnet/signalr/hubconnectionbuilder)ログ レベルを構成します。 メッセージは、ブラウザーのコンソールに記録されます。

[!code-javascript[Logging levels](javascript-client/sample/wwwroot/js/chat.js?range=9-12)]

## <a name="reconnect-clients"></a>クライアントを再接続します。

SignalR JavaScript クライアントは自動的に再接続しません。 クライアントを手動で再接続するコードを記述する必要があります。 次のコードでは、再接続の一般的なアプローチを示します。

1. 関数 (ここで、`start`関数)、接続を開始するが作成されます。
1. 呼び出す、`start`関数で、接続の`onclose`イベント ハンドラー。

[!code-javascript[Reconnect the JavaScript client](javascript-client/sample/wwwroot/js/chat.js?range=28-40)]

実際の実装は、指数バックオフを使用して、または断念する前に指定された回数を再試行してください。 

## <a name="additional-resources"></a>その他の技術情報

* [JavaScript API リファレンス](/javascript/api/?view=signalr-js-latest)
* [JavaScript のチュートリアル](xref:tutorials/signalr)
* [WebPack と TypeScript のチュートリアル](xref:tutorials/signalr-typescript-webpack)
* [ハブ](xref:signalr/hubs)
* [.NET クライアント](xref:signalr/dotnet-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
* [クロス オリジン要求 (CORS)](xref:security/cors)
* [Azure SignalR サービスのサーバーレス ドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)
