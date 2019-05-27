---
title: gRPC の ASP.NET Core の構成
author: jamesnk
description: GRPC の ASP.NET Core アプリを構成する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: jamesnk
ms.custom: mvc
ms.date: 04/09/2019
uid: grpc/configuration
ms.openlocfilehash: 851c9ca1f7d62f6f368df66bb38eb4bbaf64bf32
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "66041894"
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

## <a name="additional-resources"></a>その他の技術情報

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
