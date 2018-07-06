---
uid: web-api/overview/releases/whats-new-in-aspnet-web-api-22
title: ASP.NET Web API 2.2 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: aspnetcontent
ms.date: 12/25/2014
ms.assetid: 99c59ae4-167e-4f66-a6cd-d3f1098c4e4a
msc.legacyurl: /web-api/overview/releases/whats-new-in-aspnet-web-api-22
msc.type: authoredcontent
ms.openlocfilehash: 0f0794988da712897092ab808a08fca5eeebb6d9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37813279"
---
<a name="whats-new-in-aspnet-web-api-22"></a><span data-ttu-id="68ca4-102">ASP.NET Web API 2.2 の新機能新機能</span><span class="sxs-lookup"><span data-stu-id="68ca4-102">What's New in ASP.NET Web API 2.2</span></span>
====================
<span data-ttu-id="68ca4-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="68ca4-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="68ca4-104">このトピックでは、ASP.NET Web API 2.2 の新機能新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-104">This topic describes what's new for ASP.NET Web API 2.2.</span></span>

- [<span data-ttu-id="68ca4-105">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="68ca4-105">Download</span></span>](#download)
- [<span data-ttu-id="68ca4-106">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="68ca4-106">Documentation</span></span>](#documentation)
- [<span data-ttu-id="68ca4-107">ASP.NET Web API 2.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="68ca4-107">New Features in ASP.NET Web API 2.2</span></span>](#newf)

    - [<span data-ttu-id="68ca4-108">OData v4</span><span class="sxs-lookup"><span data-stu-id="68ca4-108">OData v4</span></span>](#OData)
    - [<span data-ttu-id="68ca4-109">属性のルーティングが強化されました</span><span class="sxs-lookup"><span data-stu-id="68ca4-109">Attribute Routing Improvements</span></span>](#ARI)
    - [<span data-ttu-id="68ca4-110">Windows Phone 8.1 用の web API クライアントのサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-110">Web API Client support for Windows Phone 8.1</span></span>](#phone)
- [<span data-ttu-id="68ca4-111">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="68ca4-111">Known Issues and Breaking Changes</span></span>](#known-issues)
- [<span data-ttu-id="68ca4-112">バグの修正</span><span class="sxs-lookup"><span data-stu-id="68ca4-112">Bug Fixes</span></span>](#bug-fixes)
- [<span data-ttu-id="68ca4-113">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="68ca4-113">Microsoft.AspNet.OData 5.2.1</span></span>](#odata521)
- [<span data-ttu-id="68ca4-114">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="68ca4-114">Microsoft.AspNet.WebAPI 5.2.2</span></span>](#522RC)
- [<span data-ttu-id="68ca4-115">Microsoft.AspNet.WebAPI 5.2.3 ベータ版</span><span class="sxs-lookup"><span data-stu-id="68ca4-115">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>](#523)

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="68ca4-116">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="68ca4-116">Download</span></span>

<span data-ttu-id="68ca4-117">ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-117">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="68ca4-118">次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様。</span><span class="sxs-lookup"><span data-stu-id="68ca4-118">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="68ca4-119">ASP.NET Web API 2.2 の最新のパッケージは、次のバージョン:「5.2.0」。</span><span class="sxs-lookup"><span data-stu-id="68ca4-119">The latest ASP.NET Web API 2.2 package has the following version: "5.2.0".</span></span> <span data-ttu-id="68ca4-120">インストールまたはを通じてこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/)します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-120">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.WebApi/).</span></span> <span data-ttu-id="68ca4-121">リリースには、NuGet での対応するローカライズされたパッケージも含まれています。</span><span class="sxs-lookup"><span data-stu-id="68ca4-121">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="68ca4-122">インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-122">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](whats-new-in-aspnet-web-api-22/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="68ca4-123">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="68ca4-123">Documentation</span></span>

<span data-ttu-id="68ca4-124">チュートリアルと ASP.NET Web API 2.2 に関する他の情報は、ASP.NET web サイトから入手できます ([https://www.asp.net/web-api](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="68ca4-124">Tutorials and other information about ASP.NET Web API 2.2 are available from the ASP.NET web site ([https://www.asp.net/web-api](../../index.md)).</span></span>

<a id="newf"></a>
## <a name="new-features-in-aspnet-web-api-22"></a><span data-ttu-id="68ca4-125">ASP.NET Web API 2.2 の新機能</span><span class="sxs-lookup"><span data-stu-id="68ca4-125">New Features in ASP.NET Web API 2.2</span></span>

<a id="OData"></a>
### <a name="odata-v4"></a><span data-ttu-id="68ca4-126">OData v4</span><span class="sxs-lookup"><span data-stu-id="68ca4-126">OData v4</span></span>

<span data-ttu-id="68ca4-127">このリリースでは、OData v4 のプロトコルのサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-127">This release adds support for the OData v4 protocol.</span></span> <span data-ttu-id="68ca4-128">詳細については、次を参照してください。、 [Web API OData v4 のドキュメント。](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span><span class="sxs-lookup"><span data-stu-id="68ca4-128">For more information, see the [Web API OData v4 documentation.](../odata-support-in-aspnet-web-api/odata-v4/create-an-odata-v4-endpoint.md)</span></span>

<span data-ttu-id="68ca4-129">ここで、主要な機能と OData v4 の変更の一部を示します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-129">Here are some of the key features and changes for OData v4:</span></span>

- [<span data-ttu-id="68ca4-130">OData モデルでエイリアス プロパティのサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-130">Support for aliasing properties in OData model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataModelAliasingSample/)
- [<span data-ttu-id="68ca4-131">ComplexTypeAttribute、AssociationAttribute、TimesTampAttribute ODataConventionModelBuilder で ConcurrencyCheckAttribute のサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-131">Support for ComplexTypeAttribute, AssociationAttribute, TimesTampAttribute and ConcurrencyCheckAttribute in ODataConventionModelBuilder</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEtagSample/)
- [<span data-ttu-id="68ca4-132">アクションのわかりやすいタイトルを指定することができます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-132">Provide ability to supply friendly Title for actions</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataActionsSample/)
- <span data-ttu-id="68ca4-133">ODL UriParser との統合します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-133">Integrate with ODL UriParser</span></span>
- <span data-ttu-id="68ca4-134">サポート[enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/)、[コンテインメント](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/)と[シングルトン](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span><span class="sxs-lookup"><span data-stu-id="68ca4-134">Support for [enum](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataEnumTypeSample/ODataEnumTypeSample/), [containment](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataContainmentSample/) and [singleton](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataSingletonSample/)</span></span>
- <span data-ttu-id="68ca4-135">プリミティブ型のキャストのサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-135">Support cast for primitive types</span></span>
- [<span data-ttu-id="68ca4-136">OData 関数のサポートが追加されました</span><span class="sxs-lookup"><span data-stu-id="68ca4-136">Added OData function support</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="68ca4-137">パラメーターのエイリアスを関数呼び出しをサポートします。</span><span class="sxs-lookup"><span data-stu-id="68ca4-137">Support parameter aliases for function calls</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataFunctionSample/)
- [<span data-ttu-id="68ca4-138">モデルで camel 形式の大文字と小文字の名前付け規則をサポートします。</span><span class="sxs-lookup"><span data-stu-id="68ca4-138">Support camel case naming convention in model</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataCamelCaseSample/)
- <span data-ttu-id="68ca4-139">$Filter で cast() のサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-139">Support for cast() in $filter</span></span>
- <span data-ttu-id="68ca4-140">開いている複合型のサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-140">Support for open complex type</span></span>
- <span data-ttu-id="68ca4-141">削除された EntitySetController と AsyncEntitySetController</span><span class="sxs-lookup"><span data-stu-id="68ca4-141">Removed EntitySetController and AsyncEntitySetController</span></span>
- [<span data-ttu-id="68ca4-142">$Link $ref に変更されました。</span><span class="sxs-lookup"><span data-stu-id="68ca4-142">Changed $link to $ref</span></span>](http://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataServiceSample/)
- [<span data-ttu-id="68ca4-143">属性ルーティングのサポートが追加されました</span><span class="sxs-lookup"><span data-stu-id="68ca4-143">Added Attribute routing support</span></span>](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OData/v4/ODataAttributeRoutingSample/)
- <span data-ttu-id="68ca4-144">OData のコア ライブラリ 6.4.0 を使用してください。</span><span class="sxs-lookup"><span data-stu-id="68ca4-144">Uses OData Core Libraries 6.4.0</span></span>

<a id="ARI"></a>
### <a name="attribute-routing-improvements"></a><span data-ttu-id="68ca4-145">属性のルーティングが強化されました</span><span class="sxs-lookup"><span data-stu-id="68ca4-145">Attribute Routing Improvements</span></span>

<span data-ttu-id="68ca4-146">属性ルーティングを今すぐ IDirectRouteProvider で、属性ルートが検出され、構成する方法を完全に制御できますと呼ばれる機能拡張ポイントを提供します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-146">Attribute Routing now provides an extensibility point called IDirectRouteProvider, which allows full control over how attribute routes are discovered and configured.</span></span> <span data-ttu-id="68ca4-147">IDirectRouteProvider はアクションとコント ローラーとアクションのどのようなルーティングの構成が必要なだけを指定する関連付けられたルート情報の一覧を提供します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-147">An IDirectRouteProvider is responsible for providing a list of actions and controllers along with associated route information to specify exactly what routing configuration is desired for those actions.</span></span> <span data-ttu-id="68ca4-148">MapAttributes/MapHttpAttributeRoutes を呼び出すときに、IDirectRouteProvider 実装を指定することがあります。</span><span class="sxs-lookup"><span data-stu-id="68ca4-148">An IDirectRouteProvider implementation may be specified when calling MapAttributes/MapHttpAttributeRoutes.</span></span>

<span data-ttu-id="68ca4-149">IDirectRouteProvider のカスタマイズになります最も簡単な既定の実装、DefaultDirectRouteProvider を拡張することによって。</span><span class="sxs-lookup"><span data-stu-id="68ca4-149">Customizing IDirectRouteProvider will be easiest by extending our default implementation, DefaultDirectRouteProvider.</span></span> <span data-ttu-id="68ca4-150">このクラスは、属性の検出、ルートのエントリを作成するルート プレフィックス領域プレフィックスを検出するためのロジックを変更する別のオーバーライド可能な仮想メソッドを提供します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-150">This class provides separate overridable virtual methods to change the logic for discovering attributes, creating route entries, and discovering route prefix and area prefix.</span></span>

<span data-ttu-id="68ca4-151">次は、この新しい機能拡張ポイントで行うことで例を示します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-151">Following are some examples on what you could do with this new extensibility point:</span></span>

1. <span data-ttu-id="68ca4-152">ルート属性の継承のサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-152">Support inheritance of Route attributes</span></span>

    <span data-ttu-id="68ca4-153">例:</span><span class="sxs-lookup"><span data-stu-id="68ca4-153">Example:</span></span>

    <span data-ttu-id="68ca4-154">ここで like「/api/値/10」の要求では「成功: 10」を返しますが正常に</span><span class="sxs-lookup"><span data-stu-id="68ca4-154">Here a request like "/api/values/10" would successfully return "Success:10"</span></span>

    [!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample2.cs)]
2. <span data-ttu-id="68ca4-155">ようにいくつかの規則に従うことによって、属性ルートの既定のルート名を提供します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-155">Provide a default route name for your attribute routes by following some convention you like.</span></span> <span data-ttu-id="68ca4-156">既定では、属性ルーティングは自動的に作成属性ルートの名前。</span><span class="sxs-lookup"><span data-stu-id="68ca4-156">By default, attribute routing doesn't automatically create names for attribute routes.</span></span>
3. <span data-ttu-id="68ca4-157">ルート テーブルで終了する前に、1 つの一元的な場所にある属性ルートのルート テンプレートを変更します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-157">Modify attribute routes' route template at one central place before they end up in the route table.</span></span>

<a id="phone"></a>
### <a name="web-api-client-support-for-windows-phone-81"></a><span data-ttu-id="68ca4-158">Windows Phone 8.1 用の web API クライアントのサポート</span><span class="sxs-lookup"><span data-stu-id="68ca4-158">Web API Client Support for Windows Phone 8.1</span></span>

<span data-ttu-id="68ca4-159">Windows Phone 8.1 を対象とする場合、またはから Web API クライアント ロジックを実装する Web API クライアントの NuGet パッケージを使用できます、ユニバーサル アプリ内で。</span><span class="sxs-lookup"><span data-stu-id="68ca4-159">You can now use the Web API Client NuGet package to implement your Web API client logic when targeting Windows Phone 8.1 or from within a Universal App.</span></span>

<a id="known-issues"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="68ca4-160">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="68ca4-160">Known Issues and Breaking Changes</span></span>

<span data-ttu-id="68ca4-161">このセクションでは、既知の問題と、ASP.NET Web API 2.2 における重大な変更について説明します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-161">This section describes known issues and breaking changes in the ASP.NET Web API 2.2.</span></span>

### <a name="odata-v4"></a><span data-ttu-id="68ca4-162">OData v4</span><span class="sxs-lookup"><span data-stu-id="68ca4-162">OData v4</span></span>

#### <a name="model-builder"></a><span data-ttu-id="68ca4-163">モデル ビルダー</span><span class="sxs-lookup"><span data-stu-id="68ca4-163">Model builder</span></span>

<span data-ttu-id="68ca4-164">問題: オーバー ロードされた関数がいないとして公開する FunctionImport</span><span class="sxs-lookup"><span data-stu-id="68ca4-164">Issue: Overloaded Functions could not be exposed as FunctionImport</span></span>

<span data-ttu-id="68ca4-165">2 つのオーバー ロードされた関数があり、FunctionImport System.InvalidOperationException で ~/GetAllConventionCustomers(CustomerName={customerName}) 結果を要求し、次に示すようもします。</span><span class="sxs-lookup"><span data-stu-id="68ca4-165">If there are 2 overloaded functions and they are also FunctionImport as shown below then requesting ~/GetAllConventionCustomers(CustomerName={customerName}) results in System.InvalidOperationException.</span></span>

[!code-xml[Main](whats-new-in-aspnet-web-api-22/samples/sample3.xml)]

<span data-ttu-id="68ca4-166">回避策: FunctionImports として、両方の関数オーバー ロードを追加するのにはこの問題を回避します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-166">Workaround: The workaround for this issue is to add both the function overloads as FunctionImports.</span></span>

#### <a name="odata-routing"></a><span data-ttu-id="68ca4-167">OData のルーティング</span><span class="sxs-lookup"><span data-stu-id="68ca4-167">OData Routing</span></span>

<span data-ttu-id="68ca4-168">URL を含む文字列リテラルは、スラッシュ (%2f) をエンコードし、OData リソース パスで使用すると、backslash(%5C) が 404 エラーを発生します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-168">String literals that include the URL encoded slash (%2F), and backslash(%5C) cause a 404 error when they are used in the OData resource paths.</span></span>

<span data-ttu-id="68ca4-169">たとえば、関数のパラメーターまたはエンティティ セットのキー値として、OData リソース パスで文字列リテラルを使用できます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-169">For example, string literals can be used in the OData resource paths as parameters of functions or key values of entity sets.</span></span>

<span data-ttu-id="68ca4-170">/Employees/Total.GetCount(Name='Name%2F')</span><span class="sxs-lookup"><span data-stu-id="68ca4-170">/Employees/Total.GetCount(Name='Name%2F')</span></span>

<span data-ttu-id="68ca4-171">/Employees('Name%5C')</span><span class="sxs-lookup"><span data-stu-id="68ca4-171">/Employees('Name%5C')</span></span>

<span data-ttu-id="68ca4-172">サービス ホストは解除エスケープこのような要求を受信するときに、シーケンスを Web API ランタイムに渡す前にエスケープします。</span><span class="sxs-lookup"><span data-stu-id="68ca4-172">When services receive such requests the hosts will un-escape those escape sequences before passing them to the Web API runtime.</span></span> <span data-ttu-id="68ca4-173">これは、次のような攻撃から保護します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-173">This protects against attacks like the following:</span></span>  
  
`http://www.contoso.com/..%2F..%2F/Windows/System32/cmd.exe?/c+dir+c:`

<span data-ttu-id="68ca4-174">これにより、Web API OData スタックは、404 (Not Found) エラーが返されます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-174">This causes the Web API OData stack to return a 404 error (Not Found).</span></span> <span data-ttu-id="68ca4-175">このエラーを防ぐためには、クライアントは、スラッシュ (%252f) のエスケープ シーケンスと円記号 (%255 C) を使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="68ca4-175">To prevent this error, your client should use the double escape sequences for slash (%252F) and backslash (%255C).</span></span> <span data-ttu-id="68ca4-176">/Employees などのクエリ文字列には発生しませんか? $filter = Name eq '名 %2f'</span><span class="sxs-lookup"><span data-stu-id="68ca4-176">This does not happen for query strings such as /Employees?$filter=Name eq 'Name%2F'</span></span>

<span data-ttu-id="68ca4-177">**エスケープ解除されたスラッシュ (/) をメモし、円記号 (") は OData リソース パスの文字列リテラルで宣言できません。スラッシュはパスの区切り記号としてのみ表示して、バック スラッシュがまったく OData リソース パスに表示されません。(両方とも、OData クエリ文字列の一部で使用できます。)**</span><span class="sxs-lookup"><span data-stu-id="68ca4-177">**Note un-escaped slashes ('/') and backslashes ('') are not legal in OData resource path string literals. Slashes should appear only as path separators and backslashes should not appear in the OData resource path at all. (Both are usable in some portions of an OData query string.)**</span></span>

<span data-ttu-id="68ca4-178">回避策: DefaultODataPathHandler を実際にそれらを解析する前にスラッシュと文字列リテラルのバック スラッシュをエスケープするための Parse メソッドをオーバーライドできます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-178">Workaround: You could override the Parse method of DefaultODataPathHandler to escape the slash and backslash in string literals before actually parsing them.</span></span> <span data-ttu-id="68ca4-179">ここで、このアプローチのサンプルを見つけることができます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-179">You can find a sample of this approach here.</span></span>

### <a name="odata-v3"></a><span data-ttu-id="68ca4-180">OData v3</span><span class="sxs-lookup"><span data-stu-id="68ca4-180">OData v3</span></span>

#### <a name="queryable"></a><span data-ttu-id="68ca4-181">[クエリ]</span><span class="sxs-lookup"><span data-stu-id="68ca4-181">[Queryable]</span></span>

<span data-ttu-id="68ca4-182">[Queryable] 属性が非推奨とされます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-182">The [Queryable] attribute is deprecated.</span></span> <span data-ttu-id="68ca4-183">新しい OData v3 のアプリケーションを使用する必要があります**System.Web.Http.OData.EnableQueryAttribute**します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-183">New OData v3 applications should use **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="68ca4-184">**ODataHttpConfigurationExtensions.EnableQuerySupport**拡張メソッドを今すぐ追加、 **EnableQueryAttribute**にグローバル フィルターのコレクション。</span><span class="sxs-lookup"><span data-stu-id="68ca4-184">The **ODataHttpConfigurationExtensions.EnableQuerySupport** extension method now adds an **EnableQueryAttribute** to the global filter collection.</span></span> <span data-ttu-id="68ca4-185">すべてのコント ローラーがある場合、 **[Queryable]** 属性は、呼び出す`config.EnableQuerySupport()`により、 **[Queryable]** 属性が失敗するには</span><span class="sxs-lookup"><span data-stu-id="68ca4-185">If any controllers have the **[Queryable]** attribute, calling `config.EnableQuerySupport()` will cause the **[Queryable]** attribute to fail</span></span>

<span data-ttu-id="68ca4-186">この問題を解決するのには推奨される方法は、のすべてのインスタンスを置換する**QueryableAttribute**で**System.Web.Http.OData.EnableQueryAttribute**します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-186">The recommended way to resolve this issue is to replace all instances of **QueryableAttribute** with **System.Web.Http.OData.EnableQueryAttribute**.</span></span>

<span data-ttu-id="68ca4-187">別の回避策では、Web API の構成で、次のコードを使用します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-187">An alternative workaround is to use the following code in your Web API configuration:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-web-api-22/samples/sample4.cs)]

### <a name="attribute-routing"></a><span data-ttu-id="68ca4-188">属性ルーティング</span><span class="sxs-lookup"><span data-stu-id="68ca4-188">Attribute Routing</span></span>

<span data-ttu-id="68ca4-189">問題: FromUri 属性で装飾された複合型のモデル バインド動作が異なります属性ルーティングを使用する場合。</span><span class="sxs-lookup"><span data-stu-id="68ca4-189">Issue: Model binding of complex type which is decorated with FromUri attribute behaves differently when using Attribute Routing.</span></span>

<span data-ttu-id="68ca4-190">次のリンクは、問題を追跡と回避策の詳細があります。</span><span class="sxs-lookup"><span data-stu-id="68ca4-190">Following link is tracking the issue and also has details about a workaround.</span></span>  
[http://aspnetwebstack.codeplex.com/workitem/1944](http://aspnetwebstack.codeplex.com/workitem/1944)

<span data-ttu-id="68ca4-191">問題: スキャフォールディングの MVC または Web API をプロジェクトに 5.2.0 に 5.1.2 でパッケージの結果のパッケージのプロジェクトにまだ存在しません。</span><span class="sxs-lookup"><span data-stu-id="68ca4-191">Issue: Scaffolding MVC/Web API into a project with 5.2.0 packages results in 5.1.2 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="68ca4-192">ASP.NET MVC 5.2 の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなどの Visual Studio のツールや、ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。</span><span class="sxs-lookup"><span data-stu-id="68ca4-192">Updating NuGet packages for ASP.NET MVC 5.2 does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="68ca4-193">ASP.NET ランタイム パッケージ (例: 更新プログラム 2 で 5.1.2) の以前のバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-193">They use the previous version of the ASP.NET runtime packages (e.g. 5.1.2 in Update 2).</span></span> <span data-ttu-id="68ca4-194">その結果、プロジェクトで使用されていない場合は、ASP.NET スキャフォールディングは、必要なパッケージの以前のバージョン (例: 更新プログラム 2 で 5.1.2) がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-194">As a result, the ASP.NET scaffolding will install the previous version (e.g. 5.1.2 in Update 2) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="68ca4-195">ただし、Visual Studio 2013 RTM や Update 1 での ASP.NET スキャフォールディングでは、プロジェクトで最新のパッケージは上書きされません。</span><span class="sxs-lookup"><span data-stu-id="68ca4-195">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="68ca4-196">Web API 2.2 または ASP.NET MVC 5.2 に、プロジェクトのパッケージを更新した後、ASP.NET スキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-196">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.2 or ASP.NET MVC 5.2, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span>

<a id="bug-fixes"></a>
## <a name="bug-fixes-and-minor-feature-updates"></a><span data-ttu-id="68ca4-197">バグの修正と軽微な機能の更新</span><span class="sxs-lookup"><span data-stu-id="68ca4-197">Bug Fixes and Minor Feature Updates</span></span>

<span data-ttu-id="68ca4-198">このリリースではいくつかのバグ修正と軽微な機能も更新します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-198">This release also includes several bug fixes and minor feature updates.</span></span> <span data-ttu-id="68ca4-199">完全な一覧をここにあります。</span><span class="sxs-lookup"><span data-stu-id="68ca4-199">You can find the complete list here:</span></span>

- [<span data-ttu-id="68ca4-200">5.2 パッケージ</span><span class="sxs-lookup"><span data-stu-id="68ca4-200">5.2 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&status=All&type=All&priority=All&release=v5.2%20RC|v5.2%20RTM&assignedTo=All&component=Web%20API|Web%20API%20OData&sortField=AssignedTo&sortDirection=Ascending&page=0&reasonClosed=Fixed)

<a id="odata521"></a>
## <a name="microsoftaspnetodata-521"></a><span data-ttu-id="68ca4-201">Microsoft.AspNet.OData 5.2.1</span><span class="sxs-lookup"><span data-stu-id="68ca4-201">Microsoft.AspNet.OData 5.2.1</span></span>

<span data-ttu-id="68ca4-202">Microsoft.AspNet.OData 5.2.1 パッケージには、NuGet 依存関係の更新プログラムがバグの修正が含まれていません。</span><span class="sxs-lookup"><span data-stu-id="68ca4-202">The Microsoft.AspNet.OData 5.2.1 package contains NuGet dependency updates but no bug fixes.</span></span> <span data-ttu-id="68ca4-203">この更新で、Microsoft.OData.Core 6.4.0 に厳密な依存関係が不要になったが 1 つは 6.4.0 と 7.0.0 間の任意のバージョンにアップグレードできます。</span><span class="sxs-lookup"><span data-stu-id="68ca4-203">With this update, there is no longer a strict dependency on Microsoft.OData.Core 6.4.0, but one can upgrade to any version between 6.4.0 and 7.0.0.</span></span>

<a id="522RC"></a>
## <a name="microsoftaspnetwebapi-522"></a><span data-ttu-id="68ca4-204">Microsoft.AspNet.WebAPI 5.2.2</span><span class="sxs-lookup"><span data-stu-id="68ca4-204">Microsoft.AspNet.WebAPI 5.2.2</span></span>

<span data-ttu-id="68ca4-205">このリリースでは、依存関係が変更を行いましたが`Json.Net 6.0.4`します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-205">In this release we have made a dependency change for `Json.Net 6.0.4`.</span></span> <span data-ttu-id="68ca4-206">このリリースの新機能新機能の詳細については`Json.NET`を参照してください[Json.NET 6.0 Release 4 - JSON マージ、依存関係の注入](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection)します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-206">For more information on what is new in this release of `Json.NET`, see [Json.NET 6.0 Release 4 - JSON Merge, Dependency Injection](http://james.newtonking.com/archive/2014/08/04/json-net-6-0-release-4-json-merge-dependency-injection).</span></span> <span data-ttu-id="68ca4-207">このリリースでは、その他の新機能やバグ修正を Web api がありません。</span><span class="sxs-lookup"><span data-stu-id="68ca4-207">This release doesn't have any other new features or bug fixes in Web API.</span></span> <span data-ttu-id="68ca4-208">その後に、この新しいバージョンの Web API に依存する私たちが所有の他のすべての従属パッケージを更新しました。</span><span class="sxs-lookup"><span data-stu-id="68ca4-208">We have subsequently updated all other dependent packages we own to depend on this new version of Web API.</span></span>

<a id="523"></a>
## <a name="microsoftaspnetwebapi-523-beta"></a><span data-ttu-id="68ca4-209">Microsoft.AspNet.WebAPI 5.2.3 ベータ版</span><span class="sxs-lookup"><span data-stu-id="68ca4-209">Microsoft.AspNet.WebAPI 5.2.3 Beta</span></span>

<span data-ttu-id="68ca4-210">リリースについて[ここ](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="68ca4-210">You can read about the release [here](https://blogs.msdn.com/b/webdev/archive/2014/12/17/asp-net-mvc-5-2-3-web-pages-5-2-3-and-web-api-5-2-3-beta-releases.aspx).</span></span> <span data-ttu-id="68ca4-211">このリリースには、バグの修正プログラムのみが含まれています。</span><span class="sxs-lookup"><span data-stu-id="68ca4-211">This release contains only bug fixes.</span></span> <span data-ttu-id="68ca4-212">使用することができます[このクエリ](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed)をこのリリースで修正された問題の一覧を参照してください。</span><span class="sxs-lookup"><span data-stu-id="68ca4-212">You can use [this query](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.2.3%20Beta&amp;assignedTo=All&amp;component=Web%20API&amp;sortField=LastUpdatedDate&amp;sortDirection=Descending&amp;page=0&amp;reasonClosed=Fixed) to see the list of issues fixed in this release.</span></span>
