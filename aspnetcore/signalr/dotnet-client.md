---
title: ASP.NET Core SignalR .NET クライアント
author: tdykstra
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/10/2018
uid: signalr/dotnet-client
ms.openlocfilehash: 205ca8ca228dcc2cc77f7e9b6431943851a3b152
ms.sourcegitcommit: 1a2fc47fb5d3da0f2a3c3269613ab20eb3b0da2c
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/11/2018
ms.locfileid: "44373320"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET クライアント

作成者: [Rachel Appel](http://twitter.com/rachelappel)

ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。

> [!NOTE]
> Xamarin には、Visual Studio バージョンについての特別な要件があります。 詳細については、次を参照してください。 [Xamarin で SignalR クライアント 2.1.1](https://github.com/aspnet/Announcements/issues/305)します。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:tutorials/index#how-to-download-a-sample))。

この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET クライアント パッケージをインストールします。

`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。 クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>ハブへの接続します。

接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。 接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。 挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。 接続を開始`StartAsync`します。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=14-16,32)]

## <a name="call-hub-methods-from-client"></a>クライアントからのハブ メソッドの呼び出し

`InvokeAsync` ハブのメソッドを呼び出します。 ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。 SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

## <a name="call-client-methods-from-hub"></a>ハブからのクライアント メソッドを呼び出す

ハブ呼び出しを使用してメソッドを一切定義`connection.On`しますが、接続を開始する前に、ビルド後にします。

[!code-csharp[Define client methods](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ConnectionOn)]

上記のコードで`connection.On`を使用してサーバー側コードから呼び出すときに実行される、`SendAsync`メソッド。

[!code-csharp[Call client method](dotnet-client/sample/signalrchat/hubs/chathub.cs?name=snippet_SendMessage)]

## <a name="error-handling-and-logging"></a>エラー処理とログ記録

Try-catch ステートメントでエラーを処理します。 検査、`Exception`エラーが発生した後に実行する適切なアクションを決定するオブジェクト。

[!code-csharp[Logging](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ErrorHandling)]

## <a name="additional-resources"></a>その他の技術情報

* [ハブ](xref:signalr/hubs)
* [JavaScript クライアント](xref:signalr/javascript-client)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
