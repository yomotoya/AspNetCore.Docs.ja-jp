---
title: C core から ASP.NET Core への移行の gRPC サービス
author: juntaoluo
description: ASP.NET Core スタック上で実行する既存の C core gRPC ベース アプリを移動する方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/31/2019
uid: grpc/migration
ms.openlocfilehash: 4d489b5aecf2e15fbbe3ac472b991a4365cd47c1
ms.sourcegitcommit: 57a974556acd09363a58f38c26f74dc21e0d4339
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/17/2019
ms.locfileid: "59672620"
---
# <a name="migrating-grpc-services-from-c-core-to-aspnet-core"></a>C core から ASP.NET Core への移行の gRPC サービス

作成者: [John Luo](https://github.com/juntaoluo)

基になるスタックの実装により、すべての機能がの間で同じ方法で機能[C コア ベース gRPC](https://grpc.io/blog/grpc-stacks)アプリおよび ASP.NET Core ベースのアプリ。 このドキュメントには、2 つのスタック間を移行するための主な違いが強調表示されます。

## <a name="grpc-service-implementation-lifetime"></a>gRPC サービス実装の有効期間

ASP.NET Core スタック、gRPC、既定ではで作成されたサービス、[有効期間がスコープ](xref:fundamentals/dependency-injection#service-lifetimes)します。 サービスに既定では、gRPC C コアがこれに対し、バインド、[シングルトン有効期間](xref:fundamentals/dependency-injection#service-lifetimes)します。

有効期間がスコープには、有効期間のスコープを持つその他のサービスを解決するサービス実装ができるようにします。 たとえば、有効期間がスコープを解決できるも`DBContext`コンス トラクターの挿入を通じて DI コンテナーから。 スコープを持つ有効期間を使用します。

* 要求ごとに、サービス実装の新しいインスタンスが構築されます。
* 実装の種類のインスタンス メンバーを使用して要求間で状態を共有することはできません。
* DI コンテナー内のシングルトン サービスで共有状態を格納することを見込んでです。 ストアドの共有状態は、gRPC のサービス実装のコンス トラクターで解決されます。

サービスの有効期間の詳細については、次を参照してください。<xref:fundamentals/dependency-injection#service-lifetimes>します。

### <a name="add-a-singleton-service"></a>シングルトン サービスを追加します。

GRPC C core の実装から ASP.NET Core への移行を容易にするには、サービスの実装からのサービスの有効期間を変更することスコープ シングルトンです。 これは、DI コンテナーへのサービス実装のインスタンスの追加が含まれます。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services.AddGrpc();
    services.AddSingleton(new GreeterService());
}
```

ただし、シングルトン有効期間を持つサービスの実装では、コンス トラクターの挿入を通じてスコープを持つサービスを解決することはありません。

## <a name="configure-grpc-services-options"></a>GRPC サービス オプションを構成します。

C コア ベースのアプリなどの設定で`grpc.max_receive_message_length`と`grpc.max_send_message_length`で構成された`ChannelOption`とき[サーバー インスタンスを構築する](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server__ctor_System_Collections_Generic_IEnumerable_Grpc_Core_ChannelOption__)します。

ASP.NET Core で`GrpcServiceOptions`これらの設定を構成する方法を提供します。 設定は、gRPC のすべてのサービスまたは個々 のサービス実装の種類、グローバルに適用できます。 個々 のサービス実装の種類を指定するオプションは、構成されている場合、グローバル設定をオーバーライドします。

```csharp
public void ConfigureServices(IServiceCollection services)
{
    services
        .AddGrpc(globalOptions =>
        {
            // Global settings
            globalOptions.SendMaxMessageSize = 4096
            globalOptions.ReceiveMaxMessageSize = 4096
        })
        .AddServiceOptions<GreeterService>(greeterOptions =>
        {
            // GreeterService settings. These will override global settings
            globalOptions.SendMaxMessageSize = 2048
            globalOptions.ReceiveMaxMessageSize = 2048
        })
}
```

## <a name="logging"></a>ログの記録

C コア ベースのアプリの依存、`GrpcEnvironment`に[ロガーを構成する](https://grpc.io/grpc/csharp/api/Grpc.Core.GrpcEnvironment.html?q=size#Grpc_Core_GrpcEnvironment_SetLogger_Grpc_Core_Logging_ILogger_)デバッグのためです。 ASP.NET Core スタックを通じてこの機能を提供する、[ログ API](xref:fundamentals/logging/index)します。 たとえば、コンス トラクターの挿入を使用して、gRPC サービスにロガーを追加できます。

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

## <a name="https"></a>HTTPS

C コア ベースのアプリからの HTTPS を構成する、 [Server.Ports プロパティ](https://grpc.io/grpc/csharp/api/Grpc.Core.Server.html#Grpc_Core_Server_Ports)します。 同様の概念は、ASP.NET Core でのサーバーの構成に使用されます。 たとえば、Kestrel を使用して[エンドポイント構成](xref:fundamentals/servers/kestrel#endpoint-configuration)この機能のためです。

## <a name="interceptors-and-middleware"></a>インターセプターとミドルウェア

ASP.NET Core[ミドルウェア](xref:fundamentals/middleware/index)gRPC の C コア ベースのアプリケーションでインターセプターと比較すると同様の機能を提供します。 ミドルウェアとインターセプターは概念的には、同じ両方は gRPC 要求を処理するパイプラインの構築に使用されるためです。 どちらは、パイプライン内の次のコンポーネントの前後に実行される処理を許可します。 インターセプターの抽象化を使用して、gRPC レイヤーを操作中にただし、ASP.NET Core のミドルウェアが、基になる http/2 メッセージに対して動作する、 [ServerCallContext](https://grpc.io/grpc/csharp/api/Grpc.Core.ServerCallContext.html)します。

## <a name="additional-resources"></a>その他の技術情報

* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/aspnetcore>
* <xref:tutorials/grpc/grpc-start>
