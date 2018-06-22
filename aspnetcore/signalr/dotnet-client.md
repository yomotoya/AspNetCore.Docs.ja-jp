---
title: ASP.NET Core SignalR の .NET クライアント
author: rachelappel
description: ASP.NET Core SignalR の .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: rachelap
ms.custom: mvc
ms.date: 05/29/2018
uid: signalr/dotnet-client
ms.openlocfilehash: faa4368988971a3e7fcdcd1b044971e16d70f19a
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36273296"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR の .NET クライアント

作成者: [Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR の .NET クライアントは、Xamarin、WPF、Windows フォーム、コンソール、および .NET Core のアプリで使用できます。 同様に、 [JavaScript クライアント](xref:signalr/javascript-client)、.NET クライアントでは、受信、送信し、リアルタイムでハブにメッセージを受信することができます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/live/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

この記事のサンプル コードは、ASP.NET Core SignalR の .NET クライアントを使用する WPF アプリです。

## <a name="install-the-signalr-net-client-package"></a>SignalR の .NET クライアント パッケージをインストールします。

`Microsoft.AspNetCore.SignalR.Client`パッケージは、.NET クライアントの SignalR ハブに接続するために必要です。 クライアント ライブラリをインストールする、次のコマンドを実行、 **Package Manager Console**ウィンドウ。

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>ハブに接続します。

接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`です。 接続の構築中には、ハブ URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。 挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`です。 接続を開始`StartAsync`です。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?highlight=15-17,33)]

## <a name="call-hub-methods-from-client"></a>クライアントからのハブ メソッドの呼び出し

`InvokeAsync` ハブのメソッドを呼び出します。 ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`です。 SignalR は非同期ので、使用`async`と`await`呼び出しを行うときにします。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=48-49)]

## <a name="call-client-methods-from-hub"></a>ハブからのクライアント メソッドを呼び出す

ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前にビルドが終了後します。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=22-29)]

上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行、`SendAsync`メソッドです。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?range=8-11)]

## <a name="error-handling-and-logging"></a>エラー処理とログ記録

Try-catch ステートメントによるエラーを処理します。 検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?range=46-54)]

## <a name="additional-resources"></a>その他の技術情報

* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)