# <a name="aspnet-core-web-api-controller-sample"></a><span data-ttu-id="1e086-101">ASP.NET Core Web API コントローラーのサンプル</span><span class="sxs-lookup"><span data-stu-id="1e086-101">ASP.NET Core Web API Controller Sample</span></span>

<span data-ttu-id="1e086-102">このサンプル アプリは、次のプロジェクトで構成されています。</span><span class="sxs-lookup"><span data-stu-id="1e086-102">This sample app consists of the following projects:</span></span>

- <span data-ttu-id="1e086-103">**WebApiSample.Api**: .NET Core 2.1 を対象とする ASP.NET Core 2.1 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="1e086-103">**WebApiSample.Api**: An ASP.NET Core 2.1 project targeting .NET Core 2.1.</span></span>
- <span data-ttu-id="1e086-104">**WebApiSample.Api.Pre21**: .NET Core 2.0 を対象とする ASP.NET Core 2.0 プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="1e086-104">**WebApiSample.Api.Pre21**: An ASP.NET Core 2.0 project targeting .NET Core 2.0.</span></span>
- <span data-ttu-id="1e086-105">**WebApiSample.DataAccess**: 2 つの Web API プロジェクトのデータ アクセス層として機能する .NET Standard 2.0 クラス ライブラリ。</span><span class="sxs-lookup"><span data-stu-id="1e086-105">**WebApiSample.DataAccess**: A .NET Standard 2.0 class library serving as a data access tier for the 2 Web API projects.</span></span>

<span data-ttu-id="1e086-106">このサンプルでは、Web API コントローラー作成のバリエーションを示します。</span><span class="sxs-lookup"><span data-stu-id="1e086-106">This sample illustrates variations of Web API controller creation:</span></span>

- [<span data-ttu-id="1e086-107">ControllerBase からクラスを派生する</span><span class="sxs-lookup"><span data-stu-id="1e086-107">Derive class from ControllerBase</span></span>](https://docs.microsoft.com/aspnet/core/web-api#derive-class-from-controllerbase)
- [<span data-ttu-id="1e086-108">ApiControllerAttribute でクラスに注釈を付ける</span><span class="sxs-lookup"><span data-stu-id="1e086-108">Annotate class with ApiControllerAttribute</span></span>](https://docs.microsoft.com/aspnet/core/web-api#annotate-class-with-apicontrollerattribute)
