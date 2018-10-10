---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: 属性ルーティングでは、ASP.NET Web API 2 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: d16dcc618bf6c60714179601db14f4dd2a9e41ce
ms.sourcegitcommit: a4dcca4f1cb81227c5ed3c92dc0e28be6e99447b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2018
ms.locfileid: "48912153"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a><span data-ttu-id="3e1bc-102">ASP.NET Web API 2 で属性のルーティング</span><span class="sxs-lookup"><span data-stu-id="3e1bc-102">Attribute Routing in ASP.NET Web API 2</span></span>
====================
<span data-ttu-id="3e1bc-103">作成者[Mike Wasson](https://github.com/MikeWasson)</span><span class="sxs-lookup"><span data-stu-id="3e1bc-103">by [Mike Wasson](https://github.com/MikeWasson)</span></span>

<span data-ttu-id="3e1bc-104">*ルーティング*Web API がアクションへの URI に一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-104">*Routing* is how Web API matches a URI to an action.</span></span> <span data-ttu-id="3e1bc-105">新しい型をサポートする web API 2 のルーティングと呼ばれる*属性ルーティング*します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-105">Web API 2 supports a new type of routing, called *attribute routing*.</span></span> <span data-ttu-id="3e1bc-106">名前が示すようは、ルートを定義するのに属性を使用する属性ルーティングします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-106">As the name implies, attribute routing uses attributes to define routes.</span></span> <span data-ttu-id="3e1bc-107">属性ルーティングでは、Uri の制御、web API でします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-107">Attribute routing gives you more control over the URIs in your web API.</span></span> <span data-ttu-id="3e1bc-108">たとえば、リソースの階層について説明する Uri を簡単に作成することができます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-108">For example, you can easily create URIs that describe hierarchies of resources.</span></span>

<span data-ttu-id="3e1bc-109">規約ベースと呼ばれる、ルーティングの以前のスタイルを引き続き完全にサポートは、ルーティングします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-109">The earlier style of routing, called convention-based routing, is still fully supported.</span></span> <span data-ttu-id="3e1bc-110">実際には、同じプロジェクト内の両方の手法を組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-110">In fact, you can combine both techniques in the same project.</span></span>

<span data-ttu-id="3e1bc-111">このトピックでは、属性ルーティングを有効にする方法を示していて、属性ルーティングのさまざまなオプションについて説明します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-111">This topic shows how to enable attribute routing and describes the various options for attribute routing.</span></span> <span data-ttu-id="3e1bc-112">属性ルーティングを使用するエンド ツー エンド チュートリアルでは、次を参照してください。[属性ルーティングで Web API 2 で REST API の作成](create-a-rest-api-with-attribute-routing.md)です。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-112">For an end-to-end tutorial that uses attribute routing, see [Create a REST API with Attribute Routing in Web API 2](create-a-rest-api-with-attribute-routing.md).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3e1bc-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3e1bc-113">Prerequisites</span></span>

<span data-ttu-id="3e1bc-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community、Professional、または Enterprise edition</span><span class="sxs-lookup"><span data-stu-id="3e1bc-114">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/?utm_medium=microsoft&utm_source=docs.microsoft.com&utm_campaign=button+cta&utm_content=download+vs2017) Community, Professional, or Enterprise edition</span></span>

<span data-ttu-id="3e1bc-115">または、NuGet パッケージ マネージャーを使用して、必要なパッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-115">Alternatively, use NuGet Package Manager to install the necessary packages.</span></span> <span data-ttu-id="3e1bc-116">**ツール** メニューの選択 Visual Studio で**NuGet パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-116">From the **Tools** menu in Visual Studio, select **NuGet Package Manager**, then select **Package Manager Console**.</span></span> <span data-ttu-id="3e1bc-117">パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-117">Enter the following command in the Package Manager Console window:</span></span>

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a><span data-ttu-id="3e1bc-118">なぜ属性ルーティングでしょうか。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-118">Why Attribute Routing?</span></span>

<span data-ttu-id="3e1bc-119">使用する Web API の最初のリリース*規則に基づく*ルーティングします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-119">The first release of Web API used *convention-based* routing.</span></span> <span data-ttu-id="3e1bc-120">その型のルーティングでは、いずれかを定義するかは基本的に、複数のルート テンプレートには、文字列がパラメーター化されました。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-120">In that type of routing, you define one or more route templates, which are basically parameterized strings.</span></span> <span data-ttu-id="3e1bc-121">フレームワークは、要求を受信したときに、ルート テンプレートに対して URI が一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-121">When the framework receives a request, it matches the URI against the route template.</span></span> <span data-ttu-id="3e1bc-122">(規約ベースのルーティングの詳細については、次を参照してください。 [ASP.NET Web API におけるルーティング](routing-in-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-122">(For more information about convention-based routing, see [Routing in ASP.NET Web API](routing-in-aspnet-web-api.md).</span></span>

<span data-ttu-id="3e1bc-123">規約ベースのルーティングの利点の 1 つは、テンプレートが 1 つの場所で定義されていること、およびルーティング規則は、すべてのコント ローラー間で一貫して適用されます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-123">One advantage of convention-based routing is that templates are defined in a single place, and the routing rules are applied consistently across all controllers.</span></span> <span data-ttu-id="3e1bc-124">残念ながら、規約ベースのルーティングを困難に RESTful Api に共通している特定の URI パターンをサポートします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-124">Unfortunately, convention-based routing makes it hard to support certain URI patterns that are common in RESTful APIs.</span></span> <span data-ttu-id="3e1bc-125">多くの場合、子リソースをなどのリソース: 顧客の注文がある、映画アクター、書籍、著者ありなど。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-125">For example, resources often contain child resources: Customers have orders, movies have actors, books have authors, and so forth.</span></span> <span data-ttu-id="3e1bc-126">これらの関係を反映する Uri を作成する自然です。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-126">It's natural to create URIs that reflect these relations:</span></span>

`/customers/1/orders`

<span data-ttu-id="3e1bc-127">この種類の URI では、規則ベースのルーティングを使用して作成を困難です。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-127">This type of URI is difficult to create using convention-based routing.</span></span> <span data-ttu-id="3e1bc-128">これを行うことができます、結果は、多くのコント ローラーやリソースの種類がある場合にもスケールはありません。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-128">Although it can be done, the results don't scale well if you have many controllers or resource types.</span></span>

<span data-ttu-id="3e1bc-129">属性ルーティングは、この URI のルートを定義も簡単です。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-129">With attribute routing, it's trivial to define a route for this URI.</span></span> <span data-ttu-id="3e1bc-130">コント ローラー アクションに属性を追加するだけです。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-130">You simply add an attribute to the controller action:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

<span data-ttu-id="3e1bc-131">属性ルーティングで簡単に他のいくつかのパターンを次に示します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-131">Here are some other patterns that attribute routing makes easy.</span></span>

<span data-ttu-id="3e1bc-132">**API のバージョン管理**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-132">**API versioning**</span></span>

<span data-ttu-id="3e1bc-133">この例では、「/api/v1/製品」より別のコント ローラーにルーティング「/api/v2/製品」になります。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-133">In this example, "/api/v1/products" would be routed to a different controller than "/api/v2/products".</span></span>

`/api/v1/products`
`/api/v2/products`

<span data-ttu-id="3e1bc-134">**オーバー ロードされた URI セグメント**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-134">**Overloaded URI segments**</span></span>

<span data-ttu-id="3e1bc-135">この例では、「1」は、注文番号がコレクションに「保留中」にマップされます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-135">In this example, "1" is an order number, but "pending" maps to a collection.</span></span>

`/orders/1`
`/orders/pending`

<span data-ttu-id="3e1bc-136">**複数のパラメーター型**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-136">**Multiple parameter types**</span></span>

<span data-ttu-id="3e1bc-137">この例では、「1」は、注文番号が「2013/06/16」を日付を指定します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-137">In this example, "1" is an order number, but "2013/06/16" specifies a date.</span></span>

`/orders/1`
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a><span data-ttu-id="3e1bc-138">属性ルーティングを有効にします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-138">Enabling Attribute Routing</span></span>

<span data-ttu-id="3e1bc-139">属性ルーティングを有効にするのには、呼び出す**MapHttpAttributeRoutes**構成中にします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-139">To enable attribute routing, call **MapHttpAttributeRoutes** during configuration.</span></span> <span data-ttu-id="3e1bc-140">この拡張メソッドが定義されている、 **System.Web.Http.HttpConfigurationExtensions**クラス。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-140">This extension method is defined in the **System.Web.Http.HttpConfigurationExtensions** class.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

<span data-ttu-id="3e1bc-141">属性のルーティングを組み合わせる[規則に基づく](routing-in-aspnet-web-api.md)ルーティングします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-141">Attribute routing can be combined with [convention-based](routing-in-aspnet-web-api.md) routing.</span></span> <span data-ttu-id="3e1bc-142">規約ベースのルーティングを定義するには、呼び出し、 **MapHttpRoute**メソッド。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-142">To define convention-based routes, call the **MapHttpRoute** method.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

<span data-ttu-id="3e1bc-143">Web API を構成する方法の詳細については、次を参照してください。 [ASP.NET Web API 2 の構成](../advanced/configuring-aspnet-web-api.md)します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-143">For more information about configuring Web API, see [Configuring ASP.NET Web API 2](../advanced/configuring-aspnet-web-api.md).</span></span>

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a><span data-ttu-id="3e1bc-144">注: は、Web API 1 から移行します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-144">Note: Migrating From Web API 1</span></span>

<span data-ttu-id="3e1bc-145">Web API 2 では、前に、Web API プロジェクト テンプレートには、このようなコードが生成されます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-145">Prior to Web API 2, the Web API project templates generated code like this:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

<span data-ttu-id="3e1bc-146">属性ルーティングを有効にすると、このコードで例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-146">If attribute routing is enabled, this code will throw an exception.</span></span> <span data-ttu-id="3e1bc-147">属性ルーティングを使用する既存の Web API プロジェクトをアップグレードする場合は、次にこの構成コードを更新することを確認してください。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-147">If you upgrade an existing Web API project to use attribute routing, make sure to update this configuration code to the following:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> <span data-ttu-id="3e1bc-148">詳細については、次を参照してください。 [ASP.NET ホストによる Web API を構成する](../advanced/configuring-aspnet-web-api.md#webhost)します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-148">For more information, see [Configuring Web API with ASP.NET Hosting](../advanced/configuring-aspnet-web-api.md#webhost).</span></span>


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a><span data-ttu-id="3e1bc-149">ルート属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-149">Adding Route Attributes</span></span>

<span data-ttu-id="3e1bc-150">属性を使用して定義されているルートの例を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-150">Here is an example of a route defined using an attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

<span data-ttu-id="3e1bc-151">文字列&quot;顧客/{customerId}/orders&quot;はルートの URI テンプレートです。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-151">The string &quot;customers/{customerId}/orders&quot; is the URI template for the route.</span></span> <span data-ttu-id="3e1bc-152">Web API は、テンプレートには、要求 URI の一致を試みます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-152">Web API tries to match the request URI to the template.</span></span> <span data-ttu-id="3e1bc-153">この例では、"customers"と"orders"は、リテラルのセグメントと"{customerId}"が変数のパラメーター。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-153">In this example, "customers" and "orders" are literal segments, and "{customerId}" is a variable parameter.</span></span> <span data-ttu-id="3e1bc-154">次の Uri には、このテンプレートは一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-154">The following URIs would match this template:</span></span>

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

<span data-ttu-id="3e1bc-155">使用して照合を制限する[制約](#constraints)、このトピックの後半で説明します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-155">You can restrict the matching by using [constraints](#constraints), described later in this topic.</span></span>

<span data-ttu-id="3e1bc-156">注意、 &quot;{customerId}&quot;ルート テンプレートのパラメーターの名前に一致する、 *customerId*メソッドのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-156">Notice that the &quot;{customerId}&quot; parameter in the route template matches the name of the *customerId* parameter in the method.</span></span> <span data-ttu-id="3e1bc-157">Web API コント ローラー アクションを呼び出すと、ルート パラメーターをバインドしようとします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-157">When Web API invokes the controller action, it tries to bind the route parameters.</span></span> <span data-ttu-id="3e1bc-158">たとえば、URI は、 `http://example.com/customers/1/orders`、Web API が「1」の値をバインドするを試みます、 *customerId*アクションのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-158">For example, if the URI is `http://example.com/customers/1/orders`, Web API tries to bind the value "1" to the *customerId* parameter in the action.</span></span>

<span data-ttu-id="3e1bc-159">URI テンプレートでは、いくつかのパラメーターを持つことができます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-159">A URI template can have several parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

<span data-ttu-id="3e1bc-160">ルート属性を持たないコント ローラーのメソッドは、規則ベースのルーティングを使用します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-160">Any controller methods that do not have a route attribute use convention-based routing.</span></span> <span data-ttu-id="3e1bc-161">これにより、同じプロジェクトでのルーティングの両方の種類を組み合わせることができます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-161">That way, you can combine both types of routing in the same project.</span></span>

## <a name="http-methods"></a><span data-ttu-id="3e1bc-162">HTTP メソッド</span><span class="sxs-lookup"><span data-stu-id="3e1bc-162">HTTP Methods</span></span>

<span data-ttu-id="3e1bc-163">Web API には、(GET、POST など) の要求の HTTP メソッドに基づいてアクションも選択します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-163">Web API also selects actions based on the HTTP method of the request (GET, POST, etc).</span></span> <span data-ttu-id="3e1bc-164">既定では、Web API コント ローラーのメソッド名の先頭の大文字と小文字を探します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-164">By default, Web API looks for a case-insensitive match with the start of the controller method name.</span></span> <span data-ttu-id="3e1bc-165">という名前のコント ローラー メソッドなど、 `PutCustomers` HTTP PUT 要求と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-165">For example, a controller method named `PutCustomers` matches an HTTP PUT request.</span></span>

<span data-ttu-id="3e1bc-166">次の属性のいずれかのメソッドを修飾することでこの規則をオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-166">You can override this convention by decorating the method with any the following attributes:</span></span>

- <span data-ttu-id="3e1bc-167">**[HttpDelete]**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-167">**[HttpDelete]**</span></span>
- <span data-ttu-id="3e1bc-168">**[HttpGet]**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-168">**[HttpGet]**</span></span>
- <span data-ttu-id="3e1bc-169">**[HttpHead]**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-169">**[HttpHead]**</span></span>
- <span data-ttu-id="3e1bc-170">**[HttpOptions]**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-170">**[HttpOptions]**</span></span>
- <span data-ttu-id="3e1bc-171">**[HttpPatch]**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-171">**[HttpPatch]**</span></span>
- <span data-ttu-id="3e1bc-172">**[HttpPost]**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-172">**[HttpPost]**</span></span>
- <span data-ttu-id="3e1bc-173">**[HttpPut]**</span><span class="sxs-lookup"><span data-stu-id="3e1bc-173">**[HttpPut]**</span></span>

<span data-ttu-id="3e1bc-174">次の例では、HTTP POST 要求に CreateBook メソッドをマップします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-174">The following example maps the CreateBook method to HTTP POST requests.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

<span data-ttu-id="3e1bc-175">標準以外の方法を含む、他のすべての HTTP メソッドを使用して、 **AcceptVerbs**属性には、HTTP メソッドのリストを取得します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-175">For all other HTTP methods, including non-standard methods, use the **AcceptVerbs** attribute, which takes a list of HTTP methods.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a><span data-ttu-id="3e1bc-176">ルート プレフィックス</span><span class="sxs-lookup"><span data-stu-id="3e1bc-176">Route Prefixes</span></span>

<span data-ttu-id="3e1bc-177">多くの場合、同じプレフィックスで始まるすべてのコント ローラーにルーティングされます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-177">Often, the routes in a controller all start with the same prefix.</span></span> <span data-ttu-id="3e1bc-178">例えば:</span><span class="sxs-lookup"><span data-stu-id="3e1bc-178">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

<span data-ttu-id="3e1bc-179">使用してコント ローラー全体の共通のプレフィックスを設定することができます、 **[RoutePrefix]** 属性。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-179">You can set a common prefix for an entire controller by using the **[RoutePrefix]** attribute:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

<span data-ttu-id="3e1bc-180">メソッドの属性をティルダ (~) を使用して、ルート プレフィックスをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-180">Use a tilde (~) on the method attribute to override the route prefix:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

<span data-ttu-id="3e1bc-181">ルートのプレフィックスは、パラメーターを含めることができます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-181">The route prefix can include parameters:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a><span data-ttu-id="3e1bc-182">ルート制約</span><span class="sxs-lookup"><span data-stu-id="3e1bc-182">Route Constraints</span></span>

<span data-ttu-id="3e1bc-183">ルート制約では、ルート テンプレートでパラメーターを照合する方法を制限できます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-183">Route constraints let you restrict how the parameters in the route template are matched.</span></span> <span data-ttu-id="3e1bc-184">一般的な構文は&quot;{0} パラメーター: 制約}&quot;します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-184">The general syntax is &quot;{parameter:constraint}&quot;.</span></span> <span data-ttu-id="3e1bc-185">例えば:</span><span class="sxs-lookup"><span data-stu-id="3e1bc-185">For example:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

<span data-ttu-id="3e1bc-186">ここでは、最初のルートは場合にのみオン、 &quot;id&quot; URI のセグメントは、整数です。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-186">Here, the first route will only be selected if the &quot;id&quot; segment of the URI is an integer.</span></span> <span data-ttu-id="3e1bc-187">それ以外の場合、2 つ目のルートが選択されます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-187">Otherwise, the second route will be chosen.</span></span>

<span data-ttu-id="3e1bc-188">次の表は、サポートされている制約を一覧表示します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-188">The following table lists the constraints that are supported.</span></span>

| <span data-ttu-id="3e1bc-189">制約</span><span class="sxs-lookup"><span data-stu-id="3e1bc-189">Constraint</span></span> | <span data-ttu-id="3e1bc-190">説明</span><span class="sxs-lookup"><span data-stu-id="3e1bc-190">Description</span></span> | <span data-ttu-id="3e1bc-191">例</span><span class="sxs-lookup"><span data-stu-id="3e1bc-191">Example</span></span> |
| --- | --- | --- |
| <span data-ttu-id="3e1bc-192">アルファ</span><span class="sxs-lookup"><span data-stu-id="3e1bc-192">alpha</span></span> | <span data-ttu-id="3e1bc-193">一致の大文字または小文字の英数字 (a ~ z、A ~ Z)</span><span class="sxs-lookup"><span data-stu-id="3e1bc-193">Matches uppercase or lowercase Latin alphabet characters (a-z, A-Z)</span></span> | <span data-ttu-id="3e1bc-194">{アルファ: x}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-194">{x:alpha}</span></span> |
| <span data-ttu-id="3e1bc-195">bool</span><span class="sxs-lookup"><span data-stu-id="3e1bc-195">bool</span></span> | <span data-ttu-id="3e1bc-196">ブール値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-196">Matches a Boolean value.</span></span> | <span data-ttu-id="3e1bc-197">{x:bool}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-197">{x:bool}</span></span> |
| <span data-ttu-id="3e1bc-198">datetime</span><span class="sxs-lookup"><span data-stu-id="3e1bc-198">datetime</span></span> | <span data-ttu-id="3e1bc-199">一致する**DateTime**値。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-199">Matches a **DateTime** value.</span></span> | <span data-ttu-id="3e1bc-200">{x:datetime}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-200">{x:datetime}</span></span> |
| <span data-ttu-id="3e1bc-201">decimal</span><span class="sxs-lookup"><span data-stu-id="3e1bc-201">decimal</span></span> | <span data-ttu-id="3e1bc-202">10 進値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-202">Matches a decimal value.</span></span> | <span data-ttu-id="3e1bc-203">{x:decimal}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-203">{x:decimal}</span></span> |
| <span data-ttu-id="3e1bc-204">double</span><span class="sxs-lookup"><span data-stu-id="3e1bc-204">double</span></span> | <span data-ttu-id="3e1bc-205">64 ビットの浮動小数点値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-205">Matches a 64-bit floating-point value.</span></span> | <span data-ttu-id="3e1bc-206">{x:double}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-206">{x:double}</span></span> |
| <span data-ttu-id="3e1bc-207">float</span><span class="sxs-lookup"><span data-stu-id="3e1bc-207">float</span></span> | <span data-ttu-id="3e1bc-208">32 ビットの浮動小数点値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-208">Matches a 32-bit floating-point value.</span></span> | <span data-ttu-id="3e1bc-209">{x:float}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-209">{x:float}</span></span> |
| <span data-ttu-id="3e1bc-210">guid</span><span class="sxs-lookup"><span data-stu-id="3e1bc-210">guid</span></span> | <span data-ttu-id="3e1bc-211">GUID 値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-211">Matches a GUID value.</span></span> | <span data-ttu-id="3e1bc-212">{x: guid}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-212">{x:guid}</span></span> |
| <span data-ttu-id="3e1bc-213">int</span><span class="sxs-lookup"><span data-stu-id="3e1bc-213">int</span></span> | <span data-ttu-id="3e1bc-214">32 ビット整数値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-214">Matches a 32-bit integer value.</span></span> | <span data-ttu-id="3e1bc-215">{x:int}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-215">{x:int}</span></span> |
| <span data-ttu-id="3e1bc-216">長さ</span><span class="sxs-lookup"><span data-stu-id="3e1bc-216">length</span></span> | <span data-ttu-id="3e1bc-217">指定した長さで、または長さの指定した範囲内の文字列と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-217">Matches a string with the specified length or within a specified range of lengths.</span></span> | <span data-ttu-id="3e1bc-218">{x: length(6)}{x: length(1,20)}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-218">{x:length(6)} {x:length(1,20)}</span></span> |
| <span data-ttu-id="3e1bc-219">long</span><span class="sxs-lookup"><span data-stu-id="3e1bc-219">long</span></span> | <span data-ttu-id="3e1bc-220">64 ビット整数値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-220">Matches a 64-bit integer value.</span></span> | <span data-ttu-id="3e1bc-221">{x:long}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-221">{x:long}</span></span> |
| <span data-ttu-id="3e1bc-222">max</span><span class="sxs-lookup"><span data-stu-id="3e1bc-222">max</span></span> | <span data-ttu-id="3e1bc-223">最大値は整数に一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-223">Matches an integer with a maximum value.</span></span> | <span data-ttu-id="3e1bc-224">{x:max(10)}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-224">{x:max(10)}</span></span> |
| <span data-ttu-id="3e1bc-225">maxlength</span><span class="sxs-lookup"><span data-stu-id="3e1bc-225">maxlength</span></span> | <span data-ttu-id="3e1bc-226">最大長を持つ文字列と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-226">Matches a string with a maximum length.</span></span> | <span data-ttu-id="3e1bc-227">{x:maxlength(10)}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-227">{x:maxlength(10)}</span></span> |
| <span data-ttu-id="3e1bc-228">分</span><span class="sxs-lookup"><span data-stu-id="3e1bc-228">min</span></span> | <span data-ttu-id="3e1bc-229">整数値を最小値と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-229">Matches an integer with a minimum value.</span></span> | <span data-ttu-id="3e1bc-230">{x: min(10)}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-230">{x:min(10)}</span></span> |
| <span data-ttu-id="3e1bc-231">minlength</span><span class="sxs-lookup"><span data-stu-id="3e1bc-231">minlength</span></span> | <span data-ttu-id="3e1bc-232">最小長の文字列と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-232">Matches a string with a minimum length.</span></span> | <span data-ttu-id="3e1bc-233">{x: minlength(10)}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-233">{x:minlength(10)}</span></span> |
| <span data-ttu-id="3e1bc-234">range</span><span class="sxs-lookup"><span data-stu-id="3e1bc-234">range</span></span> | <span data-ttu-id="3e1bc-235">値の範囲内の整数に一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-235">Matches an integer within a range of values.</span></span> | <span data-ttu-id="3e1bc-236">{x:range(10,50)}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-236">{x:range(10,50)}</span></span> |
| <span data-ttu-id="3e1bc-237">regex</span><span class="sxs-lookup"><span data-stu-id="3e1bc-237">regex</span></span> | <span data-ttu-id="3e1bc-238">正規表現と一致します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-238">Matches a regular expression.</span></span> | <span data-ttu-id="3e1bc-239">{x: regex(^\d{3}-\d{3}-\d{4}$)}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-239">{x:regex(^\d{3}-\d{3}-\d{4}$)}</span></span> |

<span data-ttu-id="3e1bc-240">通知、制約の一部など&quot;min&quot;かっこで囲まれた引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-240">Notice that some of the constraints, such as &quot;min&quot;, take arguments in parentheses.</span></span> <span data-ttu-id="3e1bc-241">コロンで区切られた、パラメーターには、複数の制約を適用できます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-241">You can apply multiple constraints to a parameter, separated by a colon.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a><span data-ttu-id="3e1bc-242">カスタム ルート制約</span><span class="sxs-lookup"><span data-stu-id="3e1bc-242">Custom Route Constraints</span></span>

<span data-ttu-id="3e1bc-243">実装することによってカスタム ルート制約を作成することができます、 **IHttpRouteConstraint**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-243">You can create custom route constraints by implementing the **IHttpRouteConstraint** interface.</span></span> <span data-ttu-id="3e1bc-244">たとえば、次の制約は、0 以外の整数値にパラメーターを制限します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-244">For example, the following constraint restricts a parameter to a non-zero integer value.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

<span data-ttu-id="3e1bc-245">次のコードでは、制約を登録する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-245">The following code shows how to register the constraint:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

<span data-ttu-id="3e1bc-246">これで、ルートで制約を適用できます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-246">Now you can apply the constraint in your routes:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

<span data-ttu-id="3e1bc-247">全体を置換することもできます。 **DefaultInlineConstraintResolver**クラスを実装することによって、 **IInlineConstraintResolver**インターフェイス。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-247">You can also replace the entire **DefaultInlineConstraintResolver** class by implementing the **IInlineConstraintResolver** interface.</span></span> <span data-ttu-id="3e1bc-248">そうはすべての組み込みの制約の場合を除いて置き換えるの実装**IInlineConstraintResolver**具体的にはそれらを追加します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-248">Doing so will replace all of the built-in constraints, unless your implementation of **IInlineConstraintResolver** specifically adds them.</span></span>

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a><span data-ttu-id="3e1bc-249">省略可能な URI パラメーターと既定値</span><span class="sxs-lookup"><span data-stu-id="3e1bc-249">Optional URI Parameters and Default Values</span></span>

<span data-ttu-id="3e1bc-250">ルート パラメーターに疑問符を追加することで行うと、URI パラメーターが省略可能です。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-250">You can make a URI parameter optional by adding a question mark to the route parameter.</span></span> <span data-ttu-id="3e1bc-251">ルート パラメーターが省略可能な場合は、メソッド パラメーターの既定値を定義する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-251">If a route parameter is optional, you must define a default value for the method parameter.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

<span data-ttu-id="3e1bc-252">この例で`/api/books/locale/1033`と`/api/books/locale`同じリソースを返します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-252">In this example, `/api/books/locale/1033` and `/api/books/locale` return the same resource.</span></span>

<span data-ttu-id="3e1bc-253">または、次のように、ルート テンプレート内の既定値を指定できます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-253">Alternatively, you can specify a default value inside the route template, as follows:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

<span data-ttu-id="3e1bc-254">これは、前の例とほぼ同じですが、既定値が適用される動作のわずかな違いがあります。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-254">This is almost the same as the previous example, but there is a slight difference of behavior when the default value is applied.</span></span>

- <span data-ttu-id="3e1bc-255">最初の例 ("{lcid?}") で、パラメーターがこの正確な値を反映するため、メソッド パラメーターに直接 1033 の既定値が割り当てられます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-255">In the first example ("{lcid?}"), the default value of 1033 is assigned directly to the method parameter, so the parameter will have this exact value.</span></span>
- <span data-ttu-id="3e1bc-256">2 番目の例 ("{lcid 1033 の =}")、「1033」の既定値は、モデル バインディング プロセスをたどります。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-256">In the second example ("{lcid=1033}"), the default value of "1033" goes through the model-binding process.</span></span> <span data-ttu-id="3e1bc-257">既定のモデル バインダーは、数値の値 1033 に「1033」に変換されます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-257">The default model-binder will convert "1033" to the numeric value 1033.</span></span> <span data-ttu-id="3e1bc-258">ただし、別のものを実行するカスタム モデル バインダーを接続する可能性があります。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-258">However, you could plug in a custom model binder, which might do something different.</span></span>

<span data-ttu-id="3e1bc-259">(ほとんどの場合、パイプラインでカスタム モデル バインダーがない限り、2 つの形式と同じになります。)</span><span class="sxs-lookup"><span data-stu-id="3e1bc-259">(In most cases, unless you have custom model binders in your pipeline, the two forms will be equivalent.)</span></span>

<a id="route-names"></a>
## <a name="route-names"></a><span data-ttu-id="3e1bc-260">ルート名</span><span class="sxs-lookup"><span data-stu-id="3e1bc-260">Route Names</span></span>

<span data-ttu-id="3e1bc-261">Web api では、すべてのルート名を持ちます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-261">In Web API, every route has a name.</span></span> <span data-ttu-id="3e1bc-262">ルート名は、HTTP 応答でのリンクを含めることができるように、リンクを生成するのに役立ちます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-262">Route names are useful for generating links, so that you can include a link in an HTTP response.</span></span>

<span data-ttu-id="3e1bc-263">ルート名を指定するには、設定、**名前**属性のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-263">To specify the route name, set the **Name** property on the attribute.</span></span> <span data-ttu-id="3e1bc-264">次の例では、リンクを生成するときに、ルート名を使用する方法と、ルート名を設定する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-264">The following example shows how to set the route name, and also how to use the route name when generating a link.</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a><span data-ttu-id="3e1bc-265">ルートの順序</span><span class="sxs-lookup"><span data-stu-id="3e1bc-265">Route Order</span></span>

<span data-ttu-id="3e1bc-266">フレームワークでは、ルートの URI と照合しようとして、特定の順序でルートを評価します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-266">When the framework tries to match a URI with a route, it evaluates the routes in a particular order.</span></span> <span data-ttu-id="3e1bc-267">順序を指定するには、設定、 **RouteOrder**ルート属性のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-267">To specify the order, set the **RouteOrder** property on the route attribute.</span></span> <span data-ttu-id="3e1bc-268">下位の値は、最初に評価されます。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-268">Lower values are evaluated first.</span></span> <span data-ttu-id="3e1bc-269">既定の順序の値には 0 です。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-269">The default order value is zero.</span></span>

<span data-ttu-id="3e1bc-270">合計の順序を決定する方法を次に示します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-270">Here is how the total ordering is determined:</span></span>

1. <span data-ttu-id="3e1bc-271">比較、 **RouteOrder**ルート属性のプロパティ。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-271">Compare the **RouteOrder** property of the route attribute.</span></span>
2. <span data-ttu-id="3e1bc-272">ルート テンプレートでは、各 URI セグメントを確認します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-272">Look at each URI segment in the route template.</span></span> <span data-ttu-id="3e1bc-273">セグメントごとに次のように順序します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-273">For each segment, order as follows:</span></span>

    1. <span data-ttu-id="3e1bc-274">リテラルのセグメント。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-274">Literal segments.</span></span>
    2. <span data-ttu-id="3e1bc-275">ルート制約を持つパラメーター。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-275">Route parameters with constraints.</span></span>
    3. <span data-ttu-id="3e1bc-276">ルート制約なしのパラメーター。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-276">Route parameters without constraints.</span></span>
    4. <span data-ttu-id="3e1bc-277">制約のワイルドカード パラメーター セグメント。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-277">Wildcard parameter segments with constraints.</span></span>
    5. <span data-ttu-id="3e1bc-278">制約なしのワイルドカード パラメーター セグメント。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-278">Wildcard parameter segments without constraints.</span></span>
3. <span data-ttu-id="3e1bc-279">同数の場合の場合は、ルートは大文字の序数の文字列の比較によって並べ替えられます ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) のルート テンプレート。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-279">In the case of a tie, routes are ordered by a case-insensitive ordinal string comparison ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) of the route template.</span></span>

<span data-ttu-id="3e1bc-280">次に例を示します。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-280">Here is an example.</span></span> <span data-ttu-id="3e1bc-281">次のコント ローラーを定義するとします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-281">Suppose you define the following controller:</span></span>

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

<span data-ttu-id="3e1bc-282">これらのルートの順序が次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-282">These routes are ordered as follows.</span></span>

1. <span data-ttu-id="3e1bc-283">orders/詳細</span><span class="sxs-lookup"><span data-stu-id="3e1bc-283">orders/details</span></span>
2. <span data-ttu-id="3e1bc-284">orders/{id}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-284">orders/{id}</span></span>
3. <span data-ttu-id="3e1bc-285">orders/{customerName}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-285">orders/{customerName}</span></span>
4. <span data-ttu-id="3e1bc-286">orders/{\*date}</span><span class="sxs-lookup"><span data-stu-id="3e1bc-286">orders/{\*date}</span></span>
5. <span data-ttu-id="3e1bc-287">orders/保留中</span><span class="sxs-lookup"><span data-stu-id="3e1bc-287">orders/pending</span></span>

<span data-ttu-id="3e1bc-288">「詳細」リテラルのセグメントは、および"{id}"、前に表示されますが「保留中」が表示されます最後ため、 **RouteOrder**プロパティが 1 にします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-288">Notice that "details" is a literal segment and appears before "{id}", but "pending" appears last because the **RouteOrder** property is 1.</span></span> <span data-ttu-id="3e1bc-289">(この例ではお客様が指定されていない"details"が「保留中」またはします。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-289">(This example assumes there are no customers named "details" or "pending".</span></span> <span data-ttu-id="3e1bc-290">一般に、あいまいなルートを回避するためにお試しください。</span><span class="sxs-lookup"><span data-stu-id="3e1bc-290">In general, try to avoid ambiguous routes.</span></span> <span data-ttu-id="3e1bc-291">この例では、より優れたルート テンプレートで`GetByCustomer`は、"顧客/{customerName}")</span><span class="sxs-lookup"><span data-stu-id="3e1bc-291">In this example, a better route template for `GetByCustomer` is "customers/{customerName}" )</span></span>
