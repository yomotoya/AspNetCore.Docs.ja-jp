---
title: ホストの ASP.NET Core SignalR のバック グラウンド サービス
author: bradygaster
description: .NET Core BackgroundService クラスから SignalR クライアントにメッセージを送信する方法について説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: bradyg
ms.custom: mvc
ms.date: 02/04/2019
uid: signalr/background-services
ms.openlocfilehash: dcd62f0c7056a3f987291b6c8bb8b87f94160865
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65087752"
---
# <a name="host-aspnet-core-signalr-in-background-services"></a>ホストの ASP.NET Core SignalR のバック グラウンド サービス

によって[Brady Gaster](https://twitter.com/bradygaster)

この記事では、ガイダンスを提供します。

* ASP.NET Core でホストされているバック グラウンド ワーカー プロセスを使用して、SignalR ハブをホストします。
* .NET Core 内からクライアントにメッセージを送信接続されている[BackgroundService](xref:Microsoft.Extensions.Hosting.BackgroundService)します。

[サンプル コードのダウンロードを表示または](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/signalr/background-service/sample/) [(ダウンロードする方法)](xref:index#how-to-download-a-sample)

## <a name="wire-up-signalr-during-startup"></a>スタートアップ中に SignalR を接続します。

バック グラウンド ワーカー プロセスのコンテキストで ASP.NET Core SignalR ハブをホストしているは、ASP.NET Core web アプリでのハブをホストするいると同じです。 `Startup.ConfigureServices`メソッドを呼び出す`services.AddSignalR`SignalR をサポートするために、ASP.NET Core の依存関係挿入 (DI) 層に必要なサービスを追加します。 `Startup.Configure`、`UseSignalR`メソッドが呼び出されたに ASP.NET Core の要求パイプラインで Hub のエンドポイントを設定します。

[!code-csharp[Startup](background-service/sample/Server/Startup.cs?name=Startup)]

上記の例では、`ClockHub`クラスが実装する、`Hub<T>`厳密に型指定されたハブを作成するクラス。 `ClockHub`で構成されている、`Startup`クラスは、エンドポイントで要求に応答する`/hubs/clock`します。

厳密に型指定されたハブの詳細については、次を参照してください。 [for ASP.NET Core SignalR のハブを使用して、](xref:signalr/hubs#strongly-typed-hubs)します。

> [!NOTE]
> この機能に限定されません、[ハブ\<T >](xref:Microsoft.AspNetCore.SignalR.Hub`1)クラス。 任意のクラスから継承する[ハブ](xref:Microsoft.AspNetCore.SignalR.Hub)など[DynamicHub](xref:Microsoft.AspNetCore.SignalR.DynamicHub)も機能します。

[!code-csharp[Startup](background-service/sample/Server/ClockHub.cs?name=ClockHub)]

厳密に型指定によって使用されるインターフェイス`ClockHub`は、`IClock`インターフェイス。

[!code-csharp[Startup](background-service/sample/HubServiceInterfaces/IClock.cs?name=IClock)]

## <a name="call-a-signalr-hub-from-a-background-service"></a>バック グラウンド サービスから SignalR ハブを呼び出す

起動中に、`Worker`クラス、`BackgroundService`を使用してワイヤード (有線) は`AddHostedService`。

```csharp
services.AddHostedService<Worker>();
```

SignalR もワイヤード (有線) の中にあるため、`Startup`フェーズ、によって表される各ハブの各ハブに接続されている ASP.NET Core の HTTP 要求パイプライン内の個々 のエンドポイントで、`IHubContext<T>`サーバー。 ASP.NET Core の使用する DI 機能、他のクラスのようなホストのレイヤーによってインスタンス化`BackgroundService`クラス、MVC コント ローラー クラスまたは Razor ページのモデルのインスタンスをそのまま使用してサーバー側のハブへの参照を取得できます`IHubContext<ClockHub, IClock>`構築時にします。

[!code-csharp[Startup](background-service/sample/Server/Worker.cs?name=Worker)]

として、`ExecuteAsync`メソッドがバック グラウンド サービスに繰り返し呼び出されると、サーバーの現在の日付と時刻はクライアントに送信、接続を使用して、`ClockHub`します。

## <a name="react-to-signalr-events-with-background-services"></a>バック グラウンド サービス使用の SignalR イベントに対応します。

SignalR または .NET デスクトップ アプリが実行できるは、JavaScript クライアントを使用してシングル ページ アプリなどを使用して、使用して、 <xref:signalr/dotnet-client>、`BackgroundService`または`IHostedService`実装を使用して SignalR ハブに接続し、イベントに応答することもできます。

`ClockHubClient`両方を実装するクラス、`IClock`インターフェイスと`IHostedService`インターフェイス。 この方法の中に有線できます`Startup`を継続的に実行し、サーバーからハブのイベントに応答します。 

```csharp
public partial class ClockHubClient : IClock, IHostedService
{
}
```

初期化中に、`ClockHubClient`のインスタンスを作成、`HubConnection`そのに接続して、`IClock.ShowTime`のハブのハンドラーとしてメソッド`ShowTime`イベント。

[!code-csharp[The ClockHubClient constructor](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=ClockHubClientCtor)]

`IHostedService.StartAsync` 、実装、`HubConnection`が非同期的に開始します。

[!code-csharp[StartAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StartAsync)]

中に、`IHostedService.StopAsync`メソッド、`HubConnection`の非同期的に破棄されます。

[!code-csharp[StopAsync method](background-service/sample/Clients.ConsoleTwo/ClockHubClient.cs?name=StopAsync)]

## <a name="additional-resources"></a>その他の技術情報

* [開始するには](xref:tutorials/signalr)
* [ハブ](xref:signalr/hubs)
* [Azure に発行する](xref:signalr/publish-to-azure-web-app)
* [厳密に型指定されたハブ](xref:signalr/hubs#strongly-typed-hubs)
