---
ms.openlocfilehash: 41021e30ae85dd0ae42cbe6f1606727e21bd7707
ms.sourcegitcommit: 5995f44e9e13d7e7aa8d193e2825381c42184e47
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/02/2019
ms.locfileid: "58809385"
---
# <a name="aspnet-core-middleware-extensibility-sample"></a><span data-ttu-id="9d118-101">ASP.NET Core のミドルウェアの拡張性の例</span><span class="sxs-lookup"><span data-stu-id="9d118-101">ASP.NET Core Middleware Extensibility Sample</span></span>

<span data-ttu-id="9d118-102">このサンプルでは、「[ASP.NET Core でのファクトリ ベースのミドルウェアのアクティブ化](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility)」で説明されているシナリオについて説明します。</span><span class="sxs-lookup"><span data-stu-id="9d118-102">This sample demonstrates the scenarios described in [Factory-based middleware activation in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/middleware-extensibility).</span></span>

<span data-ttu-id="9d118-103">このサンプル アプリでは、次でアクティブ化されるミドルウェアを示します。</span><span class="sxs-lookup"><span data-stu-id="9d118-103">The sample app demonstrates middleware activated by:</span></span>

* <span data-ttu-id="9d118-104">規則。</span><span class="sxs-lookup"><span data-stu-id="9d118-104">Convention.</span></span> <span data-ttu-id="9d118-105">従来のミドルウェアのアクティブ化については、「[ミドルウェア](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/)」のトピックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="9d118-105">For more information on conventional middleware activation, see the [Middleware](https://docs.microsoft.com/aspnet/core/fundamentals/middleware/) topic.</span></span>
* <span data-ttu-id="9d118-106">[IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) の実装。</span><span class="sxs-lookup"><span data-stu-id="9d118-106">An [IMiddleware](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddleware) implementation.</span></span> <span data-ttu-id="9d118-107">既定の [IMiddlewareFactory クラス](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory)によってミドルウェアはアクティブ化されます。</span><span class="sxs-lookup"><span data-stu-id="9d118-107">The default [IMiddlewareFactory](https://docs.microsoft.com/dotnet/api/microsoft.aspnetcore.http.imiddlewarefactory) class activates the middleware.</span></span>

<span data-ttu-id="9d118-108">ミドルウェアの実装は同一に機能し、クエリ文字列パラメーターで提供された値を記録します (`key`)。</span><span class="sxs-lookup"><span data-stu-id="9d118-108">The middleware implementations function identically and record the value provided by a query string parameter (`key`).</span></span> <span data-ttu-id="9d118-109">ミドルウェアは、メモリ内データベースにクエリ文字列値を記録するのに、挿入されたデータベース コンテキスト (スコープ化されたサービス) を使用します。</span><span class="sxs-lookup"><span data-stu-id="9d118-109">The middlewares use an injected database context (a scoped service) to record the query string value in an in-memory database.</span></span>
