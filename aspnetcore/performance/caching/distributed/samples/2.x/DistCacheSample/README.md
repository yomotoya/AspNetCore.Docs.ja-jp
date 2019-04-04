---
ms.openlocfilehash: 841913e61b08eada29ddd38cd6b049f757221675
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/05/2019
ms.locfileid: "57345860"
---
# <a name="aspnet-core-distributed-cache-sample"></a><span data-ttu-id="4d41b-101">ASP.NET Core の分散キャッシュのサンプル</span><span class="sxs-lookup"><span data-stu-id="4d41b-101">ASP.NET Core Distributed Cache Sample</span></span>

<span data-ttu-id="4d41b-102">このサンプルでは、分散キャッシュの使用を示します。</span><span class="sxs-lookup"><span data-stu-id="4d41b-102">This sample illustrates the use of a distributed cache.</span></span> <span data-ttu-id="4d41b-103">このサンプルで説明するシナリオ、[で ASP.NET Core での分散キャッシュ操作](https://docs.microsoft.com/aspnet/core/performance/caching/distributed)トピック。</span><span class="sxs-lookup"><span data-stu-id="4d41b-103">This sample demonstrates the scenario described in the [Work with a distributed cache in ASP.NET Core](https://docs.microsoft.com/aspnet/core/performance/caching/distributed) topic.</span></span>

<span data-ttu-id="4d41b-104">運用環境では、サンプル アプリを構成して、SQL Server の分散キャッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="4d41b-104">In the Production environment, the sample app is configured to use a distributed SQL Server cache.</span></span> <span data-ttu-id="4d41b-105">分散の Redis cache を使用するアプリを再構成するには、変更、プリプロセッサ ディレクティブの上部にある、 *Startup.cs* Redis を使用するファイル (`#define Redis // SQLServer`)。</span><span class="sxs-lookup"><span data-stu-id="4d41b-105">To reconfigure the app to use a distributed Redis cache, change the preprocessor directive at the top of the *Startup.cs* file to use Redis (`#define Redis // SQLServer`).</span></span> <span data-ttu-id="4d41b-106">詳細については、[サンプル コードでプリプロセッサ ディレクティブ](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code)を参照してください。</span><span class="sxs-lookup"><span data-stu-id="4d41b-106">For more information, see [Preprocessor directives in sample code](https://docs.microsoft.com/aspnet/core/#preprocessor-directives-in-sample-code).</span></span>
