---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665752"
---
# <a name="error-handling-sample-application"></a><span data-ttu-id="e9caf-101">エラー処理のサンプル アプリケーション</span><span class="sxs-lookup"><span data-stu-id="e9caf-101">Error Handling Sample Application</span></span>

<span data-ttu-id="e9caf-102">このサンプル アプリでは、「[ASP.NET Core のエラーを処理する](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling)」のトピックで説明されているシナリオを示します。</span><span class="sxs-lookup"><span data-stu-id="e9caf-102">This sample app demonstrates the scenarios described in the [Handle errors in ASP.NET Core](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling) topic.</span></span>

## <a name="configure-a-custom-exception-handling-page"></a><span data-ttu-id="e9caf-103">カスタム例外処理ページを構成する</span><span class="sxs-lookup"><span data-stu-id="e9caf-103">Configure a custom exception handling page</span></span>

<span data-ttu-id="e9caf-104">アプリを開発環境で実行していない場合は、例外処理ミドルウェア:</span><span class="sxs-lookup"><span data-stu-id="e9caf-104">When the app isn't running in the Development environment, Exception Handling Middleware:</span></span>

* <span data-ttu-id="e9caf-105">例外をキャッチします。</span><span class="sxs-lookup"><span data-stu-id="e9caf-105">Catches exceptions.</span></span>
* <span data-ttu-id="e9caf-106">例外をログに記録します。</span><span class="sxs-lookup"><span data-stu-id="e9caf-106">Logs exceptions.</span></span>
* <span data-ttu-id="e9caf-107">指定したパスの別のパイプラインで要求を再実行します。</span><span class="sxs-lookup"><span data-stu-id="e9caf-107">Re-executes the request in an alternate pipeline at the path provided.</span></span>

## <a name="configure-custom-exception-handling-code"></a><span data-ttu-id="e9caf-108">カスタム例外処理コードを構成する</span><span class="sxs-lookup"><span data-stu-id="e9caf-108">Configure custom exception handling code</span></span>

<span data-ttu-id="e9caf-109">カスタム例外処理ページを使ってエラー用にエンドポイントを提供する方法の代替手段は、`UseExceptionHandler` にラムダを指定することです。</span><span class="sxs-lookup"><span data-stu-id="e9caf-109">An alternative to serving an endpoint for errors with a custom exception handling page is to provide a lambda to `UseExceptionHandler`.</span></span> <span data-ttu-id="e9caf-110">`UseExceptionHandler` と共にラムダを使うと、応答を返す前にエラーにアクセスできます。</span><span class="sxs-lookup"><span data-stu-id="e9caf-110">Using a lambda with `UseExceptionHandler` allows access to the error before returning the response.</span></span>

<span data-ttu-id="e9caf-111">サンプル アプリは、`Startup.Configure` のカスタム例外処理コードを示しています。</span><span class="sxs-lookup"><span data-stu-id="e9caf-111">The sample app demonstrates custom exception handling code in `Startup.Configure`.</span></span> <span data-ttu-id="e9caf-112">*Startup.cs* ファイル (`LambdaErrorHandler`) の上部にある手順に従います。</span><span class="sxs-lookup"><span data-stu-id="e9caf-112">Follow the instructions at the top of the *Startup.cs* file (`LambdaErrorHandler`).</span></span> <span data-ttu-id="e9caf-113">アプリを起動した後、Index ページの **[例外のスロー]** リンクを使って例外をトリガーします。</span><span class="sxs-lookup"><span data-stu-id="e9caf-113">After the app starts, trigger an exception with the **Throw Exception** link on the Index page.</span></span>
