---
title: ASP.NET Core SignalR .NET クライアント
author: bradygaster
description: ASP.NET Core SignalR .NET クライアントに関する情報
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/17/2019
uid: signalr/dotnet-client
ms.openlocfilehash: b59af0f9c84a008f778709970dba2273abdfcd4f
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087719"
---
# <a name="aspnet-core-signalr-net-client"></a>ASP.NET Core SignalR .NET クライアント

ASP.NET Core SignalR .NET クライアント ライブラリでは、.NET アプリからの SignalR ハブと通信できます。

[サンプル コードを表示またはダウンロード](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/dotnet-client/sample)します ([ダウンロード方法](xref:index#how-to-download-a-sample))。

この記事のサンプル コードは、ASP.NET Core SignalR .NET クライアントを使用する WPF アプリです。

## <a name="install-the-signalr-net-client-package"></a>SignalR .NET クライアント パッケージをインストールします。

`Microsoft.AspNetCore.SignalR.Client` SignalR ハブに接続するための .NET クライアント パッケージが必要です。 クライアント ライブラリをインストールする、次のコマンドを実行、**パッケージ マネージャー コンソール**ウィンドウ。

```powershell
Install-Package Microsoft.AspNetCore.SignalR.Client
```

## <a name="connect-to-a-hub"></a>ハブへの接続します。

接続を確立するには、作成、`HubConnectionBuilder`を呼び出すと`Build`します。 接続の作成中には、ハブの URL、プロトコル、トランスポートの種類、ログ レベル、ヘッダー、およびその他のオプションを構成できます。 挿入することで、必要なオプションを構成、`HubConnectionBuilder`メソッドに`Build`します。 接続を開始`StartAsync`します。

[!code-csharp[Build hub connection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_MainWindowClass&highlight=15-17,39)]

## <a name="handle-lost-connection"></a>接続が失われたハンドル

使用して、<xref:Microsoft.AspNetCore.SignalR.Client.HubConnection.Closed>失われた接続に応答するイベントです。 たとえば、再接続を自動化することがあります。

`Closed`イベントを返すデリゲートが必要です、 `Task`、非同期コードを使用せずに実行することができます`async void`します。 デリゲートのシグネチャを満たすために、`Closed`イベント ハンドラーの実行を同期的に、返す`Task.CompletedTask`:

```csharp
connection.Closed += (error) => {
    // Do your close logic.
    return Task.CompletedTask;
};
```

非同期サポートの主な理由とは、接続を再起動するためです。 非同期アクションは、接続を開始します。

`Closed`ハンドラー、接続の再起動時は、次の例に示すように、サーバーのオーバー ロードを防ぐためにランダムな時間の待機を検討してください。

[!code-csharp[Use Closed event handler to automate reconnection](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_ClosedRestart)]

## <a name="call-hub-methods-from-client"></a>クライアントからのハブ メソッドの呼び出し

`InvokeAsync` ハブのメソッドを呼び出します。 ハブ メソッドの名前およびハブ メソッドで定義されている引数を渡す`InvokeAsync`します。 SignalR が非同期でこれを使用して、`async`と`await`呼び出しを行うときにします。

[!code-csharp[InvokeAsync method](dotnet-client/sample/signalrchatclient/MainWindow.xaml.cs?name=snippet_InvokeAsync)]

`InvokeAsync`メソッドを返します。 を`Task`サーバー メソッドが戻るときに完了します。 戻り値は、存在する場合の結果として提供されます、`Task`します。 サーバー上のメソッドによってスローされた例外を生成するエラーが発生した`Task`します。 使用`await`サーバー メソッドが完了するまで待機するための構文と`try...catch`構文エラーを処理します。

`SendAsync`メソッドを返します。 を`Task`メッセージがサーバーに送信されたときにこれが完了するとします。 この戻り値が指定されていない`Task`サーバー メソッドが完了するまで待機しません。 メッセージの送信中に、クライアントでスローされた例外を生成、エラーが発生した`Task`します。 使用`await`と`try...catch`処理するために構文エラーを送信します。

> [!NOTE]
> Azure SignalR サービスを使用している場合*サーバーレス モード*、クライアントからハブ メソッドを呼び出すことはできません。 詳細については、次を参照してください。、 [SignalR サービスのドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)します。

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
* [Azure SignalR サービスのサーバーレス ドキュメント](/azure/azure-signalr/signalr-concept-serverless-development-config)
