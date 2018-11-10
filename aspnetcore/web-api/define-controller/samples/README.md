# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="d04cb-101">ASP.NET Core Web API コントローラーのサンプル</span><span class="sxs-lookup"><span data-stu-id="d04cb-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="d04cb-102">このサンプル アプリは、次のプロジェクトで構成されています。</span><span class="sxs-lookup"><span data-stu-id="d04cb-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="d04cb-103">\**WebApiSample.Api.22*: .NET Core 2.2 を対象とする ASP.NET Core 2.2 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="d04cb-103">\**WebApiSample.Api.22*: An ASP.NET Core 2.2 project targeting .NET Core 2.2.</span></span>
- <span data-ttu-id="d04cb-104">**WebApiSample.Api.21**: .NET Core 2.1 を対象とする ASP.NET Core 2.1 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="d04cb-104">**WebApiSample.Api.21**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="d04cb-105">**WebApiSample.Api.Pre21**: .NET Core 2.0 を対象とする ASP.NET Core 2.0 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="d04cb-105">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="d04cb-106">**WebApiSample.DataAccess**: 2 つの Web API プロジェクトのデータ アクセス層として機能する .NET Standard 2.0 クラス ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="d04cb-106">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="d04cb-107">このサンプルでは、Web API コントローラー作成のバリエーションを示します。</span><span class="sxs-lookup"><span data-stu-id="d04cb-107">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="d04cb-108">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="d04cb-108">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="d04cb-109">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="d04cb-109">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
