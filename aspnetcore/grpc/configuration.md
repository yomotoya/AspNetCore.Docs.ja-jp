---
title: gRPC の ASP.NET Core の構成
author: jamesnk
description: GRPC の ASP.NET Core アプリを構成する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 66dfb9ec136616f10c1b7aaad766e18813b87de4
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/22/2019
ms.locfileid: "59983134"
---
# <a name="grpc-for-aspnet-core-configuration"></a>gRPC の ASP.NET Core の構成

## <a name="configure-services-options"></a>サービスのオプションを構成します。

次の表では、gRPC サービスの構成オプションについて説明します。

| オプション | 既定値 | 説明 |
| ------ | ------------- | ----------- |
| `SendMaxMessageSize` | `null` | サーバーから送信できるバイト単位で最大メッセージ サイズ。 例外で構成されている最大メッセージ サイズの結果を超えるメッセージを送信しようとしています。 |
| `ReceiveMaxMessageSize` | 4 MB | サーバーで受信できるをバイト単位で最大メッセージ サイズ。 サーバーは、この制限を超えるメッセージを受信した場合は、例外をスローします。 この値を増やすことによりより大きなメッセージを受信するサーバーが、メモリ消費量に悪影響を与えることができます。 |
| `EnableDetailedErrors` | `false` | 場合`true`、詳細なサービス メソッドで例外がスローされたときに、例外メッセージがクライアントに返されます。 既定値は `false` です。 これを設定`true`機密情報が漏れることができます。 |
| `CompressionProviders` | gzip | 圧縮し、メッセージを圧縮解除に使用される圧縮プロバイダーのコレクション。 圧縮のカスタム プロバイダーを作成し、コレクションに追加できます。 既定値には、プロバイダーのサポートが構成されている**gzip**圧縮します。 |
| `ResponseCompressionAlgorithm` | `null` | 圧縮アルゴリズムは、サーバーから送信されたメッセージを圧縮するために使用します。 アルゴリズムで圧縮プロバイダーが一致する必要があります`CompressionProviders`します。 クライアントの応答を圧縮する algorthm 送信することによって、アルゴリズムをサポートを示す必要があります、 **grpc の同意のエンコード**ヘッダー。 |
| `ResponseCompressionLevel` | `null` | サーバーから送信されたメッセージを圧縮するために使用する圧縮レベルです。 |

オプションのデリゲートを提供することですべてのサービス オプションを構成でき、`AddGrpc`呼び出す`Startup.ConfigureServices`します。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc(options =>
    {
        options.EnableDetailedErrors = true;
    });
}
```

1 つのサービスのオプションで提供されるグローバル オプションを上書き`AddGrpc`を使用して構成できると`AddServiceOptions<TService>`:

```csharp
services.AddGrpc().AddServiceOptions<MyService>(options =>
{
    options.ReceiveMaxMessageSize = 10 * 1024 * 1024; // 10 megabytes
});
```

## <a name="configure-kestrel-options"></a>Kestrel オプションを構成します。

Kestrel サーバーでは、ASP.NET の gRPC の動作に影響する構成オプションがあります。

### <a name="request-body-data-rate-limit"></a>要求本文データのレート制限

Kestrel サーバーは、既定で、[最小限の要求本文のレート](
<xref:Microsoft.AspNetCore.Server.Kestrel.Core.KestrelServerLimits.MinRequestBodyDataRate>)します。 ストリーミング クライアントと双方向のストリーミング呼び出しは、このレートが満たされていないと、接続がタイムアウトする可能性があります。最小要求の本文はストリーミング クライアントと双方向のストリーミングを呼び出す、gRPC サービスが含まれる場合、データ速度の制限を無効にする必要があります。

```csharp
public class Program
{
    public static void Main(string[] args)
    {
        CreateHostBuilder(args).Build().Run();
    }

    public static IHostBuilder CreateHostBuilder(string[] args) =>
         Host.CreateDefaultBuilder(args)
    .ConfigureWebHostDefaults(webBuilder =>
    {
        webBuilder.UseStartup<Startup>();
        webBuilder.ConfigureKestrel((context, options) =>
        {
            options.Limits.MinRequestBodyDataRate = null;
        });
    });
}
```

## <a name="additional-resources"></a>その他の技術情報

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
