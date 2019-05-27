---
title: ASP.NET Core を使用した gRPC サービス
author: juntaoluo
description: ASP.NET Core で gRPC のサービスを記述する場合は、基本的な概念を説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 03/08/2019
uid: grpc/aspnetcore
ms.openlocfilehash: 1f019fac23982a95fa37d43099522f4b3e9d107a
ms.sourcegitcommit: 5d384db2fa9373a93b5d15e985fb34430e49ad7a
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/23/2019
ms.locfileid: "66039282"
---
# <a name="grpc-services-with-aspnet-core"></a>ASP.NET Core を使用した gRPC サービス

このドキュメントでは、gRPC を ASP.NET Core を使用してサービスを開始する方法を示します。

[!INCLUDE[](~/includes/net-core-prereqs-all-3.0.md)]

## <a name="get-started-with-grpc-service-in-aspnet-core"></a>ASP.NET Core の gRPC サービスの概要

[!INCLUDE[View or download sample code](~/includes/grpc/download.md)]

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

参照してください[gRPC サービスの使用開始](xref:tutorials/grpc/grpc-start)gRPC プロジェクトを作成する方法の詳細な手順についてはします。

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[Visual Studio Code / Visual Studio for Mac](#tab/visual-studio-code+visual-studio-mac)

コマンド ラインから `dotnet new grpc -o GrpcGreeter` を実行します。

---

## <a name="add-grpc-services-to-an-aspnet-core-app"></a>GRPC サービス、ASP.NET Core アプリを追加します。

gRPC では、次のパッケージが必要です。

* [Grpc.AspNetCore.Server](https://www.nuget.org/packages/Grpc.AspNetCore.Server)
* [Google.Protobuf](https://www.nuget.org/packages/Google.Protobuf/) protobuf メッセージ Api。
* [Grpc.Tools](https://www.nuget.org/packages/Grpc.Tools/)

### <a name="configure-grpc"></a>GRPC を構成します。

gRPC、有効化され、`AddGrpc`メソッド。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=5)]

各 gRPC サービスを通じてルーティング パイプラインに追加されます、`MapGrpcService`メソッド。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Startup.cs?name=snippet&highlight=21)]

ASP.NET Core のミドルウェアと機能を共有して、ルーティングのパイプライン、ハンドラーの追加の要求を処理するためにそのため、アプリを構成できます。 MVC コント ローラーなど、追加の要求ハンドラーは、gRPC が構成されているサービスと並列で動作します。

## <a name="integration-with-aspnet-core-apis"></a>ASP.NET Core Api との統合

gRPC サービスが ASP.NET Core の機能へのフル アクセスをなどある[依存関係の注入](xref:fundamentals/dependency-injection)(DI) と[ログ](xref:fundamentals/logging/index)します。 たとえば、サービスの実装では、コンス トラクターによって DI コンテナーからロガー サービスを解決できます。

```csharp
public class GreeterService : Greeter.GreeterBase
{
    public GreeterService(ILogger<GreeterService> logger)
    {
    }
}
```

既定では、gRPC サービスの実装は、任意の有効期間 (シングルトン、スコープ ベース、または一時的) では、他の DI サービスを解決できます。

### <a name="resolve-httpcontext-in-grpc-methods"></a>GRPC メソッドで HttpContext を解決します。

GRPC API は、メソッド、ホスト、ヘッダー、およびトレーラーなど、一部の http/2 メッセージ データへのアクセスを提供します。 アクセスはを介して、 `ServerCallContext` gRPC の各メソッドに渡される引数。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?highlight=3-4&name=snippet)]

`ServerCallContext` フル アクセスを提供しない`HttpContext`すべての ASP.NET Api でします。 `GetHttpContext`拡張メソッドへのフル アクセスを提供する、 `HttpContext` ASP.NET Api では、基になる HTTP/2 メッセージを表します。

[!code-cs[](~/tutorials/grpc/grpc-start/samples/GrpcGreeter/Services/GreeterService.cs?name=snippet1)]

## <a name="additional-resources"></a>その他の技術情報

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
