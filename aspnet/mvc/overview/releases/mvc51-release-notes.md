---
uid: mvc/overview/releases/mvc51-release-notes
title: ASP.NET MVC 5.1 の新機能新機能 |Microsoft Docs
author: microsoft
description: ''
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 9a83a058-9b01-48aa-acce-ec041e694567
msc.legacyurl: /mvc/overview/releases/mvc51-release-notes
msc.type: authoredcontent
ms.openlocfilehash: 6cdf5ae42457ab10e30693a1b77b4aee65570be5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826364"
---
<a name="whats-new-in-aspnet-mvc-51"></a><span data-ttu-id="2fdb7-102">ASP.NET MVC 5.1 の新機能新機能</span><span class="sxs-lookup"><span data-stu-id="2fdb7-102">What's New in ASP.NET MVC 5.1</span></span>
====================
<span data-ttu-id="2fdb7-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="2fdb7-103">by [Microsoft](https://github.com/microsoft)</span></span>

<span data-ttu-id="2fdb7-104">このトピックでは、ASP.NET Web MVC 5.1 の新機能新機能について説明します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-104">This topic describes what's new for ASP.NET Web MVC 5.1.</span></span>

- [<span data-ttu-id="2fdb7-105">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="2fdb7-105">Software Requirements</span></span>](#SoftwareRequirements)
- [<span data-ttu-id="2fdb7-106">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="2fdb7-106">Download</span></span>](#download)
- [<span data-ttu-id="2fdb7-107">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2fdb7-107">Documentation</span></span>](#documentation)
- [<span data-ttu-id="2fdb7-108">ASP.NET MVC 5.1 の新機能</span><span class="sxs-lookup"><span data-stu-id="2fdb7-108">New Features in ASP.NET MVC 5.1</span></span>](#new-features)

    - [<span data-ttu-id="2fdb7-109">属性のルーティングが強化されました</span><span class="sxs-lookup"><span data-stu-id="2fdb7-109">Attribute routing improvements</span></span>](#AttributeRouting)
    - [<span data-ttu-id="2fdb7-110">エディター テンプレートのブートス トラップのサポート</span><span class="sxs-lookup"><span data-stu-id="2fdb7-110">Bootstrap support for editor templates</span></span>](#Bootstrap)
    - [<span data-ttu-id="2fdb7-111">ビューでの列挙型のサポート</span><span class="sxs-lookup"><span data-stu-id="2fdb7-111">Enum support in views</span></span>](#Enum)
    - [<span data-ttu-id="2fdb7-112">控え目な検証 Minlength/maxlength 属性</span><span class="sxs-lookup"><span data-stu-id="2fdb7-112">Unobtrusive validation for MinLength/MaxLength Attributes</span></span>](#Unobtrusive)
    - [<span data-ttu-id="2fdb7-113">控えめな Ajax での 'this' コンテキストのサポート</span><span class="sxs-lookup"><span data-stu-id="2fdb7-113">Supporting the ‘this' context in Unobtrusive Ajax</span></span>](#thisContext)
- <span data-ttu-id="2fdb7-114">[既知の問題と重大な変更](#KnownBreakingChanges)- [バグの修正](#bug-fixes)</span><span class="sxs-lookup"><span data-stu-id="2fdb7-114">[Known Issues and Breaking Changes](#KnownBreakingChanges)- [Bug Fixes](#bug-fixes)</span></span>

<a id="SoftwareRequirements"></a>
## <a name="software-requirements"></a><span data-ttu-id="2fdb7-115">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="2fdb7-115">Software Requirements</span></span>

- <span data-ttu-id="2fdb7-116">Visual Studio 2012: ダウンロード[ASP.NET および Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062)します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-116">Visual Studio 2012: Download [ASP.NET and Web Tools 2013.1 for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkId=390062).</span></span>
- <span data-ttu-id="2fdb7-117">Visual Studio 2013 の場合: ダウンロード[Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064)します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-117">Visual Studio 2013: Download [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=390064).</span></span> <span data-ttu-id="2fdb7-118">ASP.NET MVC 5.1 の Razor ビューを編集するためには、この更新プログラムが必要です。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-118">This update is needed for editing ASP.NET MVC 5.1 Razor Views.</span></span>

<a id="download"></a>
## <a name="download"></a><span data-ttu-id="2fdb7-119">ダウンロード</span><span class="sxs-lookup"><span data-stu-id="2fdb7-119">Download</span></span>

<span data-ttu-id="2fdb7-120">ランタイムの機能は、NuGet ギャラリーでの NuGet パッケージとしてリリースされます。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-120">The runtime features are released as NuGet packages on the NuGet gallery.</span></span> <span data-ttu-id="2fdb7-121">次のすべてのランタイム パッケージ、[セマンティック バージョニング](http://semver.org/)仕様。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-121">All the runtime packages follow the [Semantic Versioning](http://semver.org/) specification.</span></span> <span data-ttu-id="2fdb7-122">ASP.NET MVC 5.1 RTM の最新のパッケージは、次のバージョン:「5.1.2」。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-122">The latest ASP.NET MVC 5.1 RTM package has the following version: "5.1.2".</span></span> <span data-ttu-id="2fdb7-123">インストールまたはを通じてこれらのパッケージを更新することができます[NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/)します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-123">You can install or update these packages through [NuGet](http://www.nuget.org/packages/Microsoft.AspNet.Mvc/).</span></span> <span data-ttu-id="2fdb7-124">リリースには、NuGet での対応するローカライズされたパッケージも含まれています。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-124">The release also includes corresponding localized packages on NuGet.</span></span>

<span data-ttu-id="2fdb7-125">インストールまたは NuGet パッケージ マネージャー コンソールを使用して、リリースされた NuGet パッケージを更新できます。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-125">You can install or update to the released NuGet packages by using the NuGet Package Manager Console:</span></span>

[!code-console[Main](mvc51-release-notes/samples/sample1.cmd)]

<a id="documentation"></a>
## <a name="documentation"></a><span data-ttu-id="2fdb7-126">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2fdb7-126">Documentation</span></span>

<span data-ttu-id="2fdb7-127">チュートリアルと ASP.NET MVC 5.1 RTM に関する他の情報は、ASP.NET web サイトから入手できます (https://www.asp.net)します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-127">Tutorials and other information about ASP.NET MVC 5.1 RTM are available from the ASP.NET web site ( https://www.asp.net).</span></span> 

<a id="new-features"></a>
## <a name="new-features-in-aspnet-mvc-51"></a><span data-ttu-id="2fdb7-128">ASP.NET MVC 5.1 の新機能</span><span class="sxs-lookup"><span data-stu-id="2fdb7-128">New Features in ASP.NET MVC 5.1</span></span>

<a id="AttributeRouting"></a>

### <a name="attribute-routing-improvements"></a><span data-ttu-id="2fdb7-129">属性のルーティングが強化されました</span><span class="sxs-lookup"><span data-stu-id="2fdb7-129">Attribute routing improvements</span></span>

 <span data-ttu-id="2fdb7-130">ルートの選択を属性ルーティング現在のバージョン管理とヘッダーの有効化のサポートの制約に基づいています。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-130">Attribute routing now supports constraints, enabling versioning and header based route selection.</span></span> <span data-ttu-id="2fdb7-131">属性ルートの多くの側面が経由でカスタマイズ可能なようになりました、`IDirectRouteFactory`インターフェイスと`RouteFactoryAttribute`クラス。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-131">Many aspects of attribute routes are now customizable via the `IDirectRouteFactory` interface and `RouteFactoryAttribute` class.</span></span> <span data-ttu-id="2fdb7-132">ルート プレフィックスを使用して拡張が、`IRoutePrefix`インターフェイスと`RoutePrefixAttribute`クラス。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-132">The route prefix is now extensible via the `IRoutePrefix` interface and `RoutePrefixAttribute` class.</span></span> 

<a id="Enum"></a>

### <a name="enum-support-in-views"></a><span data-ttu-id="2fdb7-133">ビューでの列挙型のサポート</span><span class="sxs-lookup"><span data-stu-id="2fdb7-133">Enum support in views</span></span>

1. <span data-ttu-id="2fdb7-134">新しい`@Html.EnumDropDownListFor()`ヘルパー メソッド。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-134">New `@Html.EnumDropDownListFor()` helper methods.</span></span> <span data-ttu-id="2fdb7-135">式を評価する必要がありますに注意して HTML ヘルパーのほとんどのようにこれらを使用する必要があります、 [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx)型または[Nullable&lt;T&gt; ](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx)場所`T`は、[enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx)型。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-135">These should be used like most of the HTML helpers with the caveat that the expression must evaluate to an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type or a [Nullable&lt;T&gt;](https://msdn.microsoft.com/en-us/library/2cf62fcy.aspx) where `T` is an [enum](https://msdn.microsoft.com/en-us/library/cc138362.aspx) type.</span></span> <span data-ttu-id="2fdb7-136">使用`EnumHelper.IsValidForEnumHelper()`をこれらの要件を確認します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-136">Use `EnumHelper.IsValidForEnumHelper()` to check these requirements.</span></span>
2. <span data-ttu-id="2fdb7-137">新しい`EnumHelper.GetSelectList()`返すメソッドを`IList<SelectListItem>`します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-137">New `EnumHelper.GetSelectList()` methods which return an `IList<SelectListItem>`.</span></span> <span data-ttu-id="2fdb7-138">呼び出す、たとえば、前に、選択リストを操作する必要がある場合に便利ですが`@Html.DropDownListFor()`、名前を表示するとき、またはこれ`@Html.EnumDropDownListFor()`を示しています。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-138">This is useful when you need to manipulate a select list prior to calling, for example, `@Html.DropDownListFor()`, or when you wish to display the names which `@Html.EnumDropDownListFor()` shows.</span></span>

<span data-ttu-id="2fdb7-139">次のコードでは、これらの Api を示します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-139">The following code shows these APIs.</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample2.cshtml)]

<span data-ttu-id="2fdb7-140">完全な例を確認できます[ここ](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/)します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-140">You can see a complete example [here](https://aspnet.codeplex.com/SourceControl/latest#Samples/MVC/EnumSample/).</span></span>

<a id="Bootstrap"></a>

### <a name="bootstrap-support-for-editor-templates"></a><span data-ttu-id="2fdb7-141">エディター テンプレートのブートス トラップのサポート</span><span class="sxs-lookup"><span data-stu-id="2fdb7-141">Bootstrap support for editor templates</span></span>

<span data-ttu-id="2fdb7-142">HTML 属性に渡すことを許可しましたようになりました[EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx)として、[匿名オブジェクト](https://msdn.microsoft.com/en-us/library/bb397696.aspx)します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-142">We now allow passing in HTML attributes in [EditorFor](https://msdn.microsoft.com/en-us/library/system.web.mvc.html.editorextensions.editorfor(v=vs.100).aspx) as an [anonymous object](https://msdn.microsoft.com/en-us/library/bb397696.aspx).</span></span>

<span data-ttu-id="2fdb7-143">例えば:</span><span class="sxs-lookup"><span data-stu-id="2fdb7-143">For example:</span></span>

[!code-cshtml[Main](mvc51-release-notes/samples/sample3.cshtml)]

<a id="Unobtrusive"></a>

### <a name="unobtrusive-validation-for-minlengthattribute-and-maxlengthattribute"></a><span data-ttu-id="2fdb7-144">MinLengthAttribute と MaxLengthAttribute 控え目な検証</span><span class="sxs-lookup"><span data-stu-id="2fdb7-144">Unobtrusive validation for MinLengthAttribute and MaxLengthAttribute</span></span>

<span data-ttu-id="2fdb7-145">修飾されたプロパティの文字列および配列型のクライアント側の検証をサポートようになりましたが、 [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx)と[MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx)属性。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-145">Client-side validation for string and array types will now be supported for properties decorated with the [MinLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.minlengthattribute(v=vs.110).aspx) and [MaxLength](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.maxlengthattribute(v=vs.110).aspx) attributes.</span></span>

<a id="thisContext"></a>

### <a name="supporting-the-this-context-in-unobtrusive-ajax"></a><span data-ttu-id="2fdb7-146">控えめな Ajax での 'this' コンテキストのサポート</span><span class="sxs-lookup"><span data-stu-id="2fdb7-146">Supporting the ‘this' context in Unobtrusive Ajax</span></span>

<span data-ttu-id="2fdb7-147">コールバック関数 (`OnBegin, OnComplete, OnFailure, OnSuccess`) を使用して呼び出し元の要素を見つけることになりますが、`this`コンテキスト。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-147">The callback functions (`OnBegin, OnComplete, OnFailure, OnSuccess`) will now be able to locate the invoking element via the `this` context.</span></span> <span data-ttu-id="2fdb7-148">例えば:</span><span class="sxs-lookup"><span data-stu-id="2fdb7-148">For example:</span></span>

[!code-html[Main](mvc51-release-notes/samples/sample4.html)]

<a id="KnownBreakingChanges"></a>

## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="2fdb7-149">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="2fdb7-149">Known Issues and Breaking Changes</span></span>

### <a name="attribute-routing"></a><span data-ttu-id="2fdb7-150">属性ルーティング</span><span class="sxs-lookup"><span data-stu-id="2fdb7-150">Attribute Routing</span></span>

<span data-ttu-id="2fdb7-151">最初の一致を選択するのではなく、エラー、属性ルーティングの一致にあいまいさをレポートします。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-151">Ambiguities in attribute routing matches will now report an error rather than choosing the first match.</span></span>

<span data-ttu-id="2fdb7-152">属性ルートは、使用を禁止して、`{controller}`パラメーターを使用してと、`{action}`ルート パラメーターは、アクションに配置します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-152">Attribute routes are prohibited from using the `{controller}` parameter, and from using the `{action}` parameter on routes placed on actions.</span></span> <span data-ttu-id="2fdb7-153">これらのパラメーターの使用は、あいまいさをられる可能性が高くなります。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-153">Uses of these parameters would very likely lead to ambiguities.</span></span> 

### <a name="scaffolding-mvcweb-api-into-a-project-with-51-packages-results-in-50-packages-for-ones-that-dont-already-exist-in-the-project"></a><span data-ttu-id="2fdb7-154">プロジェクトに 5.0 パッケージで 5.1 のパッケージの結果に MVC または Web API をプロジェクトに既に存在しないもののスキャフォールディング</span><span class="sxs-lookup"><span data-stu-id="2fdb7-154">Scaffolding MVC/Web API into a project with 5.1 packages results in 5.0 packages for ones that don't already exist in the project</span></span>

<span data-ttu-id="2fdb7-155">ASP.NET MVC 5.1 RTM の NuGet パッケージを更新しても、ASP.NET スキャフォールディングなどの Visual Studio のツールや、ASP.NET Web アプリケーション プロジェクト テンプレートには更新されません。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-155">Updating NuGet packages for ASP.NET MVC 5.1 RTM does not update the Visual Studio tools such as ASP.NET scaffolding or the ASP.NET Web Application project template.</span></span> <span data-ttu-id="2fdb7-156">ASP.NET ランタイム パッケージ (5.0.0.0) の以前のバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-156">They use the previous version of the ASP.NET runtime packages (5.0.0.0).</span></span> <span data-ttu-id="2fdb7-157">その結果、プロジェクトで使用されていない場合は、ASP.NET スキャフォールディングは、必要なパッケージの以前のバージョン (5.0.0.0) がインストールされます。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-157">As a result, the ASP.NET scaffolding will install the previous version (5.0.0.0) of the required packages, if they are not already available in your projects.</span></span> <span data-ttu-id="2fdb7-158">ただし、Visual Studio 2013 RTM や Update 1 での ASP.NET スキャフォールディングでは、プロジェクトで最新のパッケージは上書きされません。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-158">However, the ASP.NET scaffolding in Visual Studio 2013 RTM or Update 1 does not overwrite the latest packages in your projects.</span></span> <span data-ttu-id="2fdb7-159">Web API 2.1 または ASP.NET MVC 5.1 に、プロジェクトのパッケージを更新した後、ASP.NET スキャフォールディングを使用する場合は、Web API と ASP.NET MVC のバージョンでは、一貫性を確認します。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-159">If you use ASP.NET scaffolding after updating the packages of your projects to Web API 2.1 or ASP.NET MVC 5.1, make sure the versions of Web API and ASP.NET MVC are consistent.</span></span> 

### <a name="syntax-highlighting-for-razor-views-in-visual-studio-2013"></a><span data-ttu-id="2fdb7-160">Visual Studio 2013 での Razor ビューの構文の強調表示</span><span class="sxs-lookup"><span data-stu-id="2fdb7-160">Syntax Highlighting for Razor Views in Visual Studio 2013</span></span>

<span data-ttu-id="2fdb7-161">Visual Studio 2013 を更新することがなく、ASP.NET MVC 5.1 RTM に更新する場合は、Razor ビューの編集中に構文の強調表示の Visual Studio エディターのサポートを取得するはできません。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-161">If you update to ASP.NET MVC 5.1 RTM without updating Visual Studio 2013, you will not get Visual Studio editor support for syntax highlighting while editing the Razor views.</span></span> <span data-ttu-id="2fdb7-162">このサポートを受ける Visual Studio 2013 を更新する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-162">You will need to update Visual Studio 2013 to get this support.</span></span> 

### <a name="type-renames"></a><span data-ttu-id="2fdb7-163">型名の変更</span><span class="sxs-lookup"><span data-stu-id="2fdb7-163">Type Renames</span></span>

<span data-ttu-id="2fdb7-164">5.1 RTM でいくつか属性のルーティング拡張機能を使用する型の名前が変更されます。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-164">Some of the types used for attribute routing extensibility are renamed in 5.1 RTM.</span></span>

| <span data-ttu-id="2fdb7-165">**以前の型名 (5.1 RC)**</span><span class="sxs-lookup"><span data-stu-id="2fdb7-165">**Old Type Name (5.1 RC)**</span></span> | <span data-ttu-id="2fdb7-166">**新しい型の名前 (5.1 RTM)**</span><span class="sxs-lookup"><span data-stu-id="2fdb7-166">**New Type Name (5.1 RTM)**</span></span> |
| --- | --- |
| <span data-ttu-id="2fdb7-167">IDirectRouteProvider</span><span class="sxs-lookup"><span data-stu-id="2fdb7-167">IDirectRouteProvider</span></span> | <span data-ttu-id="2fdb7-168">IDirectRouteFactory</span><span class="sxs-lookup"><span data-stu-id="2fdb7-168">IDirectRouteFactory</span></span> |
| <span data-ttu-id="2fdb7-169">RouteProviderAttribute</span><span class="sxs-lookup"><span data-stu-id="2fdb7-169">RouteProviderAttribute</span></span> | <span data-ttu-id="2fdb7-170">RouteFactoryAttribute</span><span class="sxs-lookup"><span data-stu-id="2fdb7-170">RouteFactoryAttribute</span></span> |
| <span data-ttu-id="2fdb7-171">DirectRouteProviderContext</span><span class="sxs-lookup"><span data-stu-id="2fdb7-171">DirectRouteProviderContext</span></span> | <span data-ttu-id="2fdb7-172">DirectRouteFactoryContext</span><span class="sxs-lookup"><span data-stu-id="2fdb7-172">DirectRouteFactoryContext</span></span> |

<a id="bug-fixes"></a>
## <a name="bug-fixes"></a><span data-ttu-id="2fdb7-173">バグ修正</span><span class="sxs-lookup"><span data-stu-id="2fdb7-173">Bug Fixes</span></span>

<span data-ttu-id="2fdb7-174">このリリースには、いくつかのバグ修正も含まれています。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-174">This release also includes several bug fixes.</span></span> <span data-ttu-id="2fdb7-175">完全な一覧をここにあります。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-175">You can find the complete list here:</span></span>

- [<span data-ttu-id="2fdb7-176">5.1.0 パッケージ</span><span class="sxs-lookup"><span data-stu-id="2fdb7-176">5.1.0 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=Closed&amp;type=All&amp;priority=All&amp;release=v5.1%20Preview|v5.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)
- [<span data-ttu-id="2fdb7-177">5.1.1 パッケージ</span><span class="sxs-lookup"><span data-stu-id="2fdb7-177">5.1.1 package</span></span>](https://aspnetwebstack.codeplex.com/workitem/list/advanced?keyword=&amp;status=All&amp;type=All&amp;priority=All&amp;release=v5.1.1%20RTM&amp;assignedTo=All&amp;component=MVC&amp;sortField=AssignedTo&amp;sortDirection=Ascending&amp;page=0&amp;reasonClosed=Fixed)

<span data-ttu-id="2fdb7-178">5.1.2、パッケージには、IntelliSense の更新プログラムがないバグの修正が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2fdb7-178">The 5.1.2 package contains IntelliSense updates but no bug fixes.</span></span>
