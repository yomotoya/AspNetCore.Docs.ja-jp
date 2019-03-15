---
ms.openlocfilehash: 07abb12af390c0f2a50e98fc5e53545b6635f968
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665510"
---
# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="ceff6-101">ASP.NET Core Web API コントローラーのサンプル</span><span class="sxs-lookup"><span data-stu-id="ceff6-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="ceff6-102">このサンプル アプリは、次のプロジェクトで構成されています。</span><span class="sxs-lookup"><span data-stu-id="ceff6-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="ceff6-103">**WebApiSample.Api.22**:.NET Core 2.2 を対象とする ASP.NET Core 2.2 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="ceff6-103">**WebApiSample.Api.22**: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="ceff6-104">**WebApiSample.Api.21**:.NET Core 2.1 を対象とする ASP.NET Core 2.1 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="ceff6-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="ceff6-105">**WebApiSample.Api.Pre21**:.NET Core 2.0 を対象とする ASP.NET Core 2.0 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="ceff6-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="ceff6-106">**WebApiSample.DataAccess**:2 つの Web API プロジェクトのデータ アクセス層として機能する .NET Standard 2.0 クラス ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="ceff6-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="ceff6-107">このサンプルでは、Web API コントローラー作成のバリエーションを示します。</span><span class="sxs-lookup"><span data-stu-id="ceff6-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="ceff6-108">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="ceff6-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="ceff6-109">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="ceff6-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
