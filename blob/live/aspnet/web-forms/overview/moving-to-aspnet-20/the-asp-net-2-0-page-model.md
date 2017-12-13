---
uid: web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
title: "ASP.NET 2.0 ページ モデル |Microsoft ドキュメント"
author: microsoft
description: "ASP.NET で 1.x では、開発者には、インライン コード モデルおよびコードの分離コード モデルのいずれかが必要があります。 分離コードは、いずれかの Src 属性を使用して実装可能性があります."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: af4575a3-0ae3-4638-ba4d-218fad7a1642
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/the-asp-net-2-0-page-model
msc.type: authoredcontent
ms.openlocfilehash: e008f197cf08bec81c560018f2d42306598f9e6d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="the-aspnet-20-page-model"></a><span data-ttu-id="da91b-104">ASP.NET 2.0 ページ モデル</span><span class="sxs-lookup"><span data-stu-id="da91b-104">The ASP.NET 2.0 Page Model</span></span>
====================
<span data-ttu-id="da91b-105">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="da91b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="da91b-106">ASP.NET で 1.x では、開発者には、インライン コード モデルおよびコードの分離コード モデルのいずれかが必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-106">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="da91b-107">Src 属性または分離コード属性を使用して分離コードを実装することも、@Pageディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="da91b-107">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="da91b-108">ASP.NET 2.0 では、開発者がまだインライン コードと分離コードのいずれかを含むことが大幅に強化されて、分離コード モデルがあった。</span><span class="sxs-lookup"><span data-stu-id="da91b-108">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>


<span data-ttu-id="da91b-109">ASP.NET で 1.x では、開発者には、インライン コード モデルおよびコードの分離コード モデルのいずれかが必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-109">In ASP.NET 1.x, developers had a choice between an inline code model and a code-behind code model.</span></span> <span data-ttu-id="da91b-110">Src 属性または分離コード属性を使用して分離コードを実装することも、@Pageディレクティブです。</span><span class="sxs-lookup"><span data-stu-id="da91b-110">Code-behind could be implemented using either the Src attribute or the CodeBehind attribute of the @Page directive.</span></span> <span data-ttu-id="da91b-111">ASP.NET 2.0 では、開発者がまだインライン コードと分離コードのいずれかを含むことが大幅に強化されて、分離コード モデルがあった。</span><span class="sxs-lookup"><span data-stu-id="da91b-111">In ASP.NET 2.0, developers still have a choice between inline code and code-behind, but there have been significant enhancements to the code-behind model.</span></span>

## <a name="improvements-in-the-code-behind-model"></a><span data-ttu-id="da91b-112">分離コード モデルの機能強化</span><span class="sxs-lookup"><span data-stu-id="da91b-112">Improvements in the Code-Behind Model</span></span>

<span data-ttu-id="da91b-113">ASP.NET で ASP.NET 2.0 の分離コード モデルの変更を完全に理解するために、最適なモデルをそのままを簡単に確認が存在していた 1.x です。</span><span class="sxs-lookup"><span data-stu-id="da91b-113">In order to fully understand the changes in the code-behind model in ASP.NET 2.0, its best to quickly review the model as it existed in ASP.NET 1.x.</span></span>

## <a name="the-code-behind-model-in-aspnet-1x"></a><span data-ttu-id="da91b-114">Asp.net 分離コード モデル 1.x</span><span class="sxs-lookup"><span data-stu-id="da91b-114">The Code-Behind Model in ASP.NET 1.x</span></span>

<span data-ttu-id="da91b-115">ASP.NET で 1.x では、ASPX ファイル (web フォーム) とプログラミング コードを含む分離コード ファイルの分離コード モデルを行いました。</span><span class="sxs-lookup"><span data-stu-id="da91b-115">In ASP.NET 1.x, the code-behind model consisted of an ASPX file (the Webform) and a code-behind file containing programming code.</span></span> <span data-ttu-id="da91b-116">2 つのファイルを使用して接続されていた、 @Page ASPX ファイルのディレクティブ。</span><span class="sxs-lookup"><span data-stu-id="da91b-116">The two files were connected using the @Page directive in the ASPX file.</span></span> <span data-ttu-id="da91b-117">ASPX ページ上の各コントロールには、インスタンス変数として、分離コード ファイルに対応する宣言が必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-117">Each control on the ASPX page had a corresponding declaration in the code-behind file as an instance variable.</span></span> <span data-ttu-id="da91b-118">また、分離コード ファイルはイベント バインド用のコードが含まれるし、Visual Studio デザイナーに必要なコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="da91b-118">The code-behind file also contained code for event binding and generated code necessary for the Visual Studio designer.</span></span> <span data-ttu-id="da91b-119">コードおよびコンテンツの完全な分離がなかった ASPX ページのすべての ASP.NET 要素には、分離コード ファイルに対応するコードが必要なためではこのモデルがよく、作業します。</span><span class="sxs-lookup"><span data-stu-id="da91b-119">This model worked fairly well, but because every ASP.NET element in the ASPX page required corresponding code in the code-behind file, there was no true separation of code and content.</span></span> <span data-ttu-id="da91b-120">たとえば場合は、デザイナーでは、Visual Studio IDE の外部で ASPX ファイルを新しいサーバー コントロールを追加、アプリケーション使用の分離コード ファイルにそのコントロールの宣言がないのためにできなくなります。</span><span class="sxs-lookup"><span data-stu-id="da91b-120">For example, if a designer added a new server control to an ASPX file outside of the Visual Studio IDE, the application would break due to the absence of a declaration for that control in the code-behind file.</span></span>

## <a name="the-code-behind-model-in-aspnet-20"></a><span data-ttu-id="da91b-121">ASP.NET 2.0 で分離コード モデル</span><span class="sxs-lookup"><span data-stu-id="da91b-121">The Code-Behind Model in ASP.NET 2.0</span></span>

<span data-ttu-id="da91b-122">ASP.NET 2.0 は、このモデルを大幅に向上します。</span><span class="sxs-lookup"><span data-stu-id="da91b-122">ASP.NET 2.0 greatly improves upon this model.</span></span> <span data-ttu-id="da91b-123">ASP.NET 2.0 で分離コードを実装する新しい*部分クラス*ASP.NET 2.0 で提供します。</span><span class="sxs-lookup"><span data-stu-id="da91b-123">In ASP.NET 2.0, code-behind is implemented using the new *partial classes* provided in ASP.NET 2.0.</span></span> <span data-ttu-id="da91b-124">ASP.NET 2.0 では、分離コード クラスは、クラス定義の一部のみが含まれているので、部分クラスとして定義します。</span><span class="sxs-lookup"><span data-stu-id="da91b-124">The code-behind class in ASP.NET 2.0 is definied as a partial class meaning that it contains only part of the class definition.</span></span> <span data-ttu-id="da91b-125">クラス定義の残りの部分は、実行時に、または Web サイトをプリコンパイル ASPX ページを使用して ASP.NET 2.0 によって動的に生成されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-125">The remaining part of the class definition is dynamically generated by ASP.NET 2.0 using the ASPX page at runtime or when the Web site is precompiled.</span></span> <span data-ttu-id="da91b-126">まだ @ Page ディレクティブを使用して、分離コード ファイルと ASPX ページ間のリンクが確立されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-126">The link between the code-behind file and the ASPX page is still established using the @ Page directive.</span></span> <span data-ttu-id="da91b-127">ただしではなく、分離コードまたは Src 属性 ASP.NET 2.0 今すぐ使用 CodeFile 属性。</span><span class="sxs-lookup"><span data-stu-id="da91b-127">However, instead of a CodeBehind or Src attribute, ASP.NET 2.0 now uses the CodeFile attribute.</span></span> <span data-ttu-id="da91b-128">Inherits 属性は、ページのクラス名を指定するも使用されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-128">The Inherits attribute is also used to specify the class name for the page.</span></span>

<span data-ttu-id="da91b-129">一般的な @ Page ディレクティブは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="da91b-129">A typical @ Page directive might look like this:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample1.aspx)]

<span data-ttu-id="da91b-130">ASP.NET 2.0 の分離コード ファイル内の一般的なクラス定義は、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="da91b-130">A typical class definition in an ASP.NET 2.0 code-behind file might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample2.cs)]

> [!NOTE]
> <span data-ttu-id="da91b-131">C# および Visual Basic は、現在部分クラスをサポートしているマネージ言語のみです。</span><span class="sxs-lookup"><span data-stu-id="da91b-131">C# and Visual Basic are the only managed languages that currently support partial classes.</span></span> <span data-ttu-id="da91b-132">したがって、j# を使用する開発者は ASP.NET 2.0 で分離コード モデルを使用できません。</span><span class="sxs-lookup"><span data-stu-id="da91b-132">Therefore, developers using J# will not be able to use the code-behind model in ASP.NET 2.0.</span></span>


<span data-ttu-id="da91b-133">新しいモデルでは、開発者は自分が作成したコードのみを含むコード ファイルを持つようになりましたので、分離コード モデルが強化されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-133">The new model enhances the code-behind model because developers will now have code files that contain only the code that they have created.</span></span> <span data-ttu-id="da91b-134">分離コード ファイル内の変数宣言のインスタンスが存在しないためもコードとコンテンツの完全な分離を提供します。</span><span class="sxs-lookup"><span data-stu-id="da91b-134">It also provides for a true separation of code and content because there are no instance variable declarations in the code-behind file.</span></span>

> [!NOTE]
> <span data-ttu-id="da91b-135">ASPX ページの部分クラスが、イベントのバインドが行われるために、Visual Basic 開発者は、分離コードでイベントをバインドする Handles キーワードを使用して、わずかなパフォーマンスが向上を実感できます。</span><span class="sxs-lookup"><span data-stu-id="da91b-135">Because the partial class for the ASPX page is where event binding takes place, Visual Basic developers can realize a slight performance increase by using the Handles keyword in code-behind to bind events.</span></span> <span data-ttu-id="da91b-136">C# の場合に、同等のキーワードがありません。</span><span class="sxs-lookup"><span data-stu-id="da91b-136">C# has no equivalent keyword.</span></span>


## <a name="new--page-directive-attributes"></a><span data-ttu-id="da91b-137">新しい @ Page ディレクティブ属性</span><span class="sxs-lookup"><span data-stu-id="da91b-137">New @ Page Directive Attributes</span></span>

<span data-ttu-id="da91b-138">ASP.NET 2.0 では、@ Page ディレクティブを多数の新しい属性を追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-138">ASP.NET 2.0 adds many new attributes to the @ Page directive.</span></span> <span data-ttu-id="da91b-139">次の属性は、ASP.NET 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="da91b-139">The following attributes are new in ASP.NET 2.0.</span></span>

## <a name="async"></a><span data-ttu-id="da91b-140">Async</span><span class="sxs-lookup"><span data-stu-id="da91b-140">Async</span></span>

<span data-ttu-id="da91b-141">Async 属性では、非同期的に実行する ページを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-141">The Async attribute allows you to configure page to be executed asynchronously.</span></span> <span data-ttu-id="da91b-142">後でこのモジュールでの非同期のページについても説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-142">Well cover asynchronous pages later in this module.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="da91b-143">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="da91b-143">AsyncTimeout</span></span>

<span data-ttu-id="da91b-144">非同期のページのタイムアウトを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-144">Specified the timeout for asynchronous pages.</span></span> <span data-ttu-id="da91b-145">既定値は 45 秒です。</span><span class="sxs-lookup"><span data-stu-id="da91b-145">The default is 45 seconds.</span></span>

## <a name="codefile"></a><span data-ttu-id="da91b-146">CodeFile</span><span class="sxs-lookup"><span data-stu-id="da91b-146">CodeFile</span></span>

<span data-ttu-id="da91b-147">CodeFile 属性は、Visual Studio の 2002年/2003 での分離コード属性の代わりのです。</span><span class="sxs-lookup"><span data-stu-id="da91b-147">The CodeFile attribute is the replacement for the CodeBehind attribute in Visual Studio 2002/2003.</span></span>

### <a name="codefilebaseclass"></a><span data-ttu-id="da91b-148">CodeFileBaseClass</span><span class="sxs-lookup"><span data-stu-id="da91b-148">CodeFileBaseClass</span></span>

<span data-ttu-id="da91b-149">CodeFileBaseClass 属性は、複数のページを 1 つの基本クラスから派生させる必要がある場合に使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-149">The CodeFileBaseClass attribute is used in cases where you want multiple pages to derive from a single base class.</span></span> <span data-ttu-id="da91b-150">この属性を持たない ASP.NET では、部分クラスの実装のため ASPX ページで宣言されているコントロールを参照する共有の共通のフィールドを使用する基本クラスが正しく機能しないため ASP です。ネット コンパイル エンジンでは、ページのコントロールに基づく新しいメンバーを自動的に作成されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-150">Because of the implementation of partial classes in ASP.NET, without this attribute, a base class that uses shared common fields to reference controls declared in an ASPX page would not work properly because ASP.NETs compilation engine will automatically create new members based on controls in the page.</span></span> <span data-ttu-id="da91b-151">したがって、する場合は、共通の基本クラスの 2 つまたは複数のページを ASP.NET で、必要がありますを定義する CodeFileBaseClass 属性で、基本クラスを指定し、その基底クラスから各ページのクラスを派生します。</span><span class="sxs-lookup"><span data-stu-id="da91b-151">Therefore, if you want a common base class for two or more pages in ASP.NET, you will need to define specify your base class in the CodeFileBaseClass attribute and then derive each pages class from that base class.</span></span> <span data-ttu-id="da91b-152">CodeFile 属性がこの属性を使用する場合にも必要です。</span><span class="sxs-lookup"><span data-stu-id="da91b-152">The CodeFile attribute is also required when this attribute is used.</span></span>

## <a name="compilationmode"></a><span data-ttu-id="da91b-153">compilationMode</span><span class="sxs-lookup"><span data-stu-id="da91b-153">CompilationMode</span></span>

<span data-ttu-id="da91b-154">この属性では、ASPX ページの CompilationMode プロパティを設定することができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-154">This attribute allows you to set the CompilationMode property of the ASPX page.</span></span> <span data-ttu-id="da91b-155">CompilationMode プロパティは、値を含む列挙**常に**、**自動**、および**Never**です。</span><span class="sxs-lookup"><span data-stu-id="da91b-155">The CompilationMode property is an enumeration containing the values **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="da91b-156">既定値は**常に**です。</span><span class="sxs-lookup"><span data-stu-id="da91b-156">The default is **Always**.</span></span> <span data-ttu-id="da91b-157">**自動**設定はコンパイルできないようにする ASP.NET 動的にページ可能な場合です。</span><span class="sxs-lookup"><span data-stu-id="da91b-157">The **Auto** setting will prevent ASP.NET from dynamically compiling the page if possible.</span></span> <span data-ttu-id="da91b-158">動的コンパイルからページを除外すると、パフォーマンスが向上します。</span><span class="sxs-lookup"><span data-stu-id="da91b-158">Excluding pages from dynamic compilation increases performance.</span></span> <span data-ttu-id="da91b-159">ただし、除外されているページをコンパイルする必要があるコードが含まれている場合、エラーがスローされます、ページが参照されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-159">However, if a page that is excluded contains that code that must be compiled, an error will be thrown when the page is browsed.</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="da91b-160">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="da91b-160">EnableEventValidation</span></span>

<span data-ttu-id="da91b-161">この属性は、ポストバックとコールバックのイベントが検証されるかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-161">This attribute specifies whether or not postback and callback events are validated.</span></span> <span data-ttu-id="da91b-162">これを有効にすると、引数をポストバックまたはコールバック イベントはそれらを最初に表示するサーバー コントロールから発生したことを確認するチェックされます。</span><span class="sxs-lookup"><span data-stu-id="da91b-162">When this is enabled, arguments to postback or callback events are checked to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="da91b-163">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="da91b-163">EnableTheming</span></span>

<span data-ttu-id="da91b-164">この属性は、ASP.NET のテーマがページで使用されるかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-164">This attribute specifies whether or not ASP.NET themes are used on a page.</span></span> <span data-ttu-id="da91b-165">既定値は **false** です。</span><span class="sxs-lookup"><span data-stu-id="da91b-165">The default is **false**.</span></span> <span data-ttu-id="da91b-166">ASP.NET のテーマは、「[モジュール 10](profiles-themes-and-web-parts.md)です。</span><span class="sxs-lookup"><span data-stu-id="da91b-166">ASP.NET themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="linepragmas"></a><span data-ttu-id="da91b-167">LinePragmas</span><span class="sxs-lookup"><span data-stu-id="da91b-167">LinePragmas</span></span>

<span data-ttu-id="da91b-168">この属性は、コンパイル時に行プラグマを追加するかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-168">This attribute specifies whether line pragmas should be added during compilation.</span></span> <span data-ttu-id="da91b-169">行プラグマは、コードの特定のセクションをマークする、デバッガーによって使用されるオプションです。</span><span class="sxs-lookup"><span data-stu-id="da91b-169">Line pragmas are options used by debuggers to mark specific sections of code.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="da91b-170">MaintainScrollPositionOnPostback</span><span class="sxs-lookup"><span data-stu-id="da91b-170">MaintainScrollPositionOnPostback</span></span>

<span data-ttu-id="da91b-171">この属性は、ポストバック間でのスクロール位置を維持するために、ページに JavaScript が挿入されるかどうかを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-171">This attribute specifies whether or not JavaScript is injected into the page in order to maintain scroll position between postbacks.</span></span> <span data-ttu-id="da91b-172">この属性は**false**既定です。</span><span class="sxs-lookup"><span data-stu-id="da91b-172">This attribute is **false** by default.</span></span>

<span data-ttu-id="da91b-173">この属性の場合は**true**、ASP.NET は追加、&lt;スクリプト&gt;次のようなポストバック時にブロック。</span><span class="sxs-lookup"><span data-stu-id="da91b-173">When this attribute is **true**, ASP.NET will add a &lt;script&gt; block on postback that looks like this:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample3.html)]

<span data-ttu-id="da91b-174">このスクリプト ブロックの src は WebResource.axd であることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="da91b-174">Note that the src for this script block is WebResource.axd.</span></span> <span data-ttu-id="da91b-175">このリソースは、物理パスではありません。</span><span class="sxs-lookup"><span data-stu-id="da91b-175">This resource is not a physical path.</span></span> <span data-ttu-id="da91b-176">このスクリプトが要求されると、ASP.NET は、スクリプトを動的に構築します。</span><span class="sxs-lookup"><span data-stu-id="da91b-176">When this script is requested, ASP.NET dynamically builds the script.</span></span>

### <a name="masterpagefile"></a><span data-ttu-id="da91b-177">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="da91b-177">MasterPageFile</span></span>

<span data-ttu-id="da91b-178">この属性は、現在のページのマスター ページ ファイルを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-178">This attribute specifies the master page file for the current page.</span></span> <span data-ttu-id="da91b-179">相対パスまたは絶対パスができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-179">The path can be relative or absolute.</span></span> <span data-ttu-id="da91b-180">マスター ページは、「[第 4 章](master-pages.md)です。</span><span class="sxs-lookup"><span data-stu-id="da91b-180">Master pages are covered in [Module 4](master-pages.md).</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="da91b-181">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="da91b-181">StyleSheetTheme</span></span>

<span data-ttu-id="da91b-182">この属性では、ASP.NET 2.0 テーマによって定義されたユーザー インターフェイスの外観プロパティをオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-182">This attribute allows you to override user-interface appearance properties defined by an ASP.NET 2.0 theme.</span></span> <span data-ttu-id="da91b-183">テーマは、「[モジュール 10](profiles-themes-and-web-parts.md)です。</span><span class="sxs-lookup"><span data-stu-id="da91b-183">Themes are covered in [Module 10](profiles-themes-and-web-parts.md).</span></span>

## <a name="theme"></a><span data-ttu-id="da91b-184">テーマ</span><span class="sxs-lookup"><span data-stu-id="da91b-184">Theme</span></span>

<span data-ttu-id="da91b-185">ページのテーマを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-185">Specifies the theme for the page.</span></span> <span data-ttu-id="da91b-186">StyleSheetTheme 属性の値が指定されていない場合、Theme 属性は、ページ上のコントロールに適用されるすべてのスタイルをオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="da91b-186">If a value is not specified for the StyleSheetTheme attribute, the Theme attribute overrides all styles applied to controls on the page.</span></span>

## <a name="title"></a><span data-ttu-id="da91b-187">タイトル</span><span class="sxs-lookup"><span data-stu-id="da91b-187">Title</span></span>

<span data-ttu-id="da91b-188">ページのタイトルを設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-188">Sets the title for the page.</span></span> <span data-ttu-id="da91b-189">ここで指定した値に表示されます、&lt;タイトル&gt;レンダリングされるページの要素。</span><span class="sxs-lookup"><span data-stu-id="da91b-189">The value specified here will appear in the &lt;title&gt; element of the rendered page.</span></span>

### <a name="viewstateencryptionmode"></a><span data-ttu-id="da91b-190">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="da91b-190">ViewStateEncryptionMode</span></span>

<span data-ttu-id="da91b-191">ViewStateEncryptionMode 列挙の値を設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-191">Sets the value for the ViewStateEncryptionMode enumeration.</span></span> <span data-ttu-id="da91b-192">使用できる値は**常に**、**自動**、および**Never**です。</span><span class="sxs-lookup"><span data-stu-id="da91b-192">The available values are **Always**, **Auto**, and **Never**.</span></span> <span data-ttu-id="da91b-193">既定値は**自動**です。値にこの属性を設定すると**自動**、viewstate が暗号化を呼び出して、コントロールが要求したが、 **RegisterRequiresViewStateEncryption**メソッドです。</span><span class="sxs-lookup"><span data-stu-id="da91b-193">The default value is **Auto**. When this attribute is set to a value of **Auto**, viewstate is encrypted is a control requests it by calling the **RegisterRequiresViewStateEncryption** method.</span></span>

## <a name="setting-public-property-values-via-the--page-directive"></a><span data-ttu-id="da91b-194">パブリック プロパティの値を使用して、@ Page ディレクティブの設定</span><span class="sxs-lookup"><span data-stu-id="da91b-194">Setting Public Property Values via the @ Page Directive</span></span>

<span data-ttu-id="da91b-195">ASP.NET 2.0 では、@ Page ディレクティブの別の新機能は、基本クラスのパブリック プロパティの初期値を設定する機能です。</span><span class="sxs-lookup"><span data-stu-id="da91b-195">Another new capability of the @ Page directive in ASP.NET 2.0 is the ability to set the initial value of public properties of a base class.</span></span> <span data-ttu-id="da91b-196">すると、たとえば、パブリック プロパティがあることと呼ばれる**しれません**で基底クラスのせることに初期化された**こんにちは**ページが読み込まれるときにします。</span><span class="sxs-lookup"><span data-stu-id="da91b-196">Suppose, for example, that you have a public property called **SomeText** in your base class and you d like it to be initialized to **Hello** when a page is loaded.</span></span> <span data-ttu-id="da91b-197">これを行う @ Page ディレクティブで値を設定するだけで次のようにします。</span><span class="sxs-lookup"><span data-stu-id="da91b-197">You can accomplish this by simply setting the value in the @ Page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample4.aspx)]

<span data-ttu-id="da91b-198">**しれません**@ Page ディレクティブの属性では、しれませんプロパティの初期値を設定する基本クラスで*こんにちは!*です。</span><span class="sxs-lookup"><span data-stu-id="da91b-198">The **SomeText** attribute of the @ Page directive sets the initial value of the SomeText property in the base class to *Hello!*.</span></span> <span data-ttu-id="da91b-199">次のビデオは、@ Page ディレクティブを使用して、基底クラスのパブリック プロパティの初期値の設定のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="da91b-199">The video below is a walkthrough of setting the initial value of a public property in a base class using the @ Page directive.</span></span>


![](the-asp-net-2-0-page-model/_static/image1.png)


[<span data-ttu-id="da91b-200">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="da91b-200">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/setprop1.wmv)


## <a name="new-public-properties-of-the-page-class"></a><span data-ttu-id="da91b-201">ページのクラスの新しいのパブリック プロパティ</span><span class="sxs-lookup"><span data-stu-id="da91b-201">New Public Properties of the Page Class</span></span>

<span data-ttu-id="da91b-202">次のパブリック プロパティは、ASP.NET 2.0 の新機能です。</span><span class="sxs-lookup"><span data-stu-id="da91b-202">The following public properties are new in ASP.NET 2.0.</span></span>

## <a name="apprelativetemplatesourcedirectory"></a><span data-ttu-id="da91b-203">AppRelativeTemplateSourceDirectory</span><span class="sxs-lookup"><span data-stu-id="da91b-203">AppRelativeTemplateSourceDirectory</span></span>

<span data-ttu-id="da91b-204">アプリケーションの相対パス、ページまたはコントロールを返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-204">Returns the application-relative path to the page or control.</span></span> <span data-ttu-id="da91b-205">たとえば、http://app/folder/page.aspx にある ページで、プロパティを返します ~/フォルダー/です。</span><span class="sxs-lookup"><span data-stu-id="da91b-205">For example, for a page located at http://app/folder/page.aspx, the property returns ~/folder/.</span></span>

## <a name="apprelativevirtualpath"></a><span data-ttu-id="da91b-206">AppRelativeVirtualPath</span><span class="sxs-lookup"><span data-stu-id="da91b-206">AppRelativeVirtualPath</span></span>

<span data-ttu-id="da91b-207">仮想ディレクトリの相対パス、ページまたはコントロールを返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-207">Returns the relative virtual directory path to the page or control.</span></span> <span data-ttu-id="da91b-208">たとえば http://app/folder/page.aspx にある ページで、プロパティを返します ~/folder/page.aspx です。</span><span class="sxs-lookup"><span data-stu-id="da91b-208">For example for a page located at http://app/folder/page.aspx, the property returns ~/folder/page.aspx.</span></span>

## <a name="asynctimeout"></a><span data-ttu-id="da91b-209">AsyncTimeout</span><span class="sxs-lookup"><span data-stu-id="da91b-209">AsyncTimeout</span></span>

<span data-ttu-id="da91b-210">取得または非同期ページを処理するために使用されるタイムアウトを設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-210">Gets or sets the timeout used for asynchronous page handling.</span></span> <span data-ttu-id="da91b-211">(非同期ページとり上げます後述します。)</span><span class="sxs-lookup"><span data-stu-id="da91b-211">(Asynchronous pages will be covered later in this module.)</span></span>

## <a name="clientquerystring"></a><span data-ttu-id="da91b-212">ClientQueryString</span><span class="sxs-lookup"><span data-stu-id="da91b-212">ClientQueryString</span></span>

<span data-ttu-id="da91b-213">要求された URL のクエリ文字列の部分を返す読み取り専用のプロパティです。</span><span class="sxs-lookup"><span data-stu-id="da91b-213">A read-only property that returns the query string portion of the requested URL.</span></span> <span data-ttu-id="da91b-214">この値は、URL エンコードです。</span><span class="sxs-lookup"><span data-stu-id="da91b-214">This value is URL encoded.</span></span> <span data-ttu-id="da91b-215">HttpServerUtility クラスの UrlDecode メソッドを使用すると、デコードします。</span><span class="sxs-lookup"><span data-stu-id="da91b-215">You can use the UrlDecode method of the HttpServerUtility class to decode it.</span></span>

## <a name="clientscript"></a><span data-ttu-id="da91b-216">ClientScript</span><span class="sxs-lookup"><span data-stu-id="da91b-216">ClientScript</span></span>

<span data-ttu-id="da91b-217">このプロパティは、クライアント側スクリプトの ASP.NETs 出力を管理するために使用する ClientScriptManager オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-217">This property returns a ClientScriptManager object that can be used to manage ASP.NETs emission of client-side script.</span></span> <span data-ttu-id="da91b-218">(ClientScriptManager クラスは、このモジュールの後半に記載されて)。</span><span class="sxs-lookup"><span data-stu-id="da91b-218">(The ClientScriptManager class is covered later in this module.)</span></span>

## <a name="enableeventvalidation"></a><span data-ttu-id="da91b-219">EnableEventValidation</span><span class="sxs-lookup"><span data-stu-id="da91b-219">EnableEventValidation</span></span>

<span data-ttu-id="da91b-220">このプロパティは、ポストバックとコールバックのイベントのイベントの検証が有効かどうかを制御します。</span><span class="sxs-lookup"><span data-stu-id="da91b-220">This property controls whether or not event validation is enabled for postback and callback events.</span></span> <span data-ttu-id="da91b-221">有効な場合、引数をポストバックまたはコールバック イベントを確認してからそれらを最初に表示するサーバー コントロールから発生したことを確認してください。</span><span class="sxs-lookup"><span data-stu-id="da91b-221">When enabled, arguments to postback or callback events are verified to ensure that they originated from the server control that originally rendered them.</span></span>

## <a name="enabletheming"></a><span data-ttu-id="da91b-222">EnableTheming</span><span class="sxs-lookup"><span data-stu-id="da91b-222">EnableTheming</span></span>

<span data-ttu-id="da91b-223">このプロパティを取得またはのページに ASP.NET 2.0 テーマが適用されるかどうかを指定するブール値を設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-223">This property gets or sets a Boolean that specifies whether or not an ASP.NET 2.0 theme applies to the page.</span></span>

## <a name="form"></a><span data-ttu-id="da91b-224">フォーム</span><span class="sxs-lookup"><span data-stu-id="da91b-224">Form</span></span>

<span data-ttu-id="da91b-225">このプロパティは、HtmlForm オブジェクトとして、ASPX ページの HTML フォームを返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-225">This property returns the HTML form on the ASPX page as an HtmlForm object.</span></span>

## <a name="header"></a><span data-ttu-id="da91b-226">Header</span><span class="sxs-lookup"><span data-stu-id="da91b-226">Header</span></span>

<span data-ttu-id="da91b-227">このプロパティは、ページ ヘッダーを含む HtmlHead オブジェクトへの参照を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-227">This property returns a reference to an HtmlHead object that contains the page header.</span></span> <span data-ttu-id="da91b-228">返された HtmlHead オブジェクトを使用するにはスタイル シート、メタ タグなどを取得/設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-228">You can use the returned HtmlHead object to get/set style sheets, Meta tags, etc.</span></span>

## <a name="idseparator"></a><span data-ttu-id="da91b-229">IdSeparator</span><span class="sxs-lookup"><span data-stu-id="da91b-229">IdSeparator</span></span>

<span data-ttu-id="da91b-230">この読み取り専用プロパティは、ASP.NET がページ上のコントロールの一意の ID を構築するときに、コントロールの識別子を区切るために使用される文字を取得します。</span><span class="sxs-lookup"><span data-stu-id="da91b-230">This read-only property gets the character that is used to separate control identifiers when ASP.NET is building a unique ID for controls on a page.</span></span> <span data-ttu-id="da91b-231">コードから直接使用するためのものではありません。</span><span class="sxs-lookup"><span data-stu-id="da91b-231">It is not intended to be used directly from your code.</span></span>

## <a name="isasync"></a><span data-ttu-id="da91b-232">IsAsync</span><span class="sxs-lookup"><span data-stu-id="da91b-232">IsAsync</span></span>

<span data-ttu-id="da91b-233">このプロパティは、非同期ページを使用できます。</span><span class="sxs-lookup"><span data-stu-id="da91b-233">This property allows for asynchronous pages.</span></span> <span data-ttu-id="da91b-234">このモジュールでは、非同期ページがについて説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-234">Asynchronous pages are discussed later in this module.</span></span>

## <a name="iscallback"></a><span data-ttu-id="da91b-235">IsCallback</span><span class="sxs-lookup"><span data-stu-id="da91b-235">IsCallback</span></span>

<span data-ttu-id="da91b-236">この読み取り専用プロパティを返します**true**ページがコールバックの結果である場合。</span><span class="sxs-lookup"><span data-stu-id="da91b-236">This read-only property returns **true** if the page is the result of a call back.</span></span> <span data-ttu-id="da91b-237">このモジュールでは、コールバックがについて説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-237">Call backs are discussed later in this module.</span></span>

## <a name="iscrosspagepostback"></a><span data-ttu-id="da91b-238">IsCrossPagePostBack</span><span class="sxs-lookup"><span data-stu-id="da91b-238">IsCrossPagePostBack</span></span>

<span data-ttu-id="da91b-239">この読み取り専用プロパティを返します**true**ページがページ間のポストバックの一部である場合。</span><span class="sxs-lookup"><span data-stu-id="da91b-239">This read-only property returns **true** if the page is part of a cross-page postback.</span></span> <span data-ttu-id="da91b-240">このモジュールでは、クロス ページのポストバックがについて説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-240">Cross-page postbacks are covered later in this module.</span></span>

## <a name="items"></a><span data-ttu-id="da91b-241">項目</span><span class="sxs-lookup"><span data-stu-id="da91b-241">Items</span></span>

<span data-ttu-id="da91b-242">ページ コンテキストに格納されているすべてのオブジェクトを含む IDictionary インスタンスへの参照を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-242">Returns a reference to an IDictionary instance that contains all objects stored in the pages context.</span></span> <span data-ttu-id="da91b-243">コンテキストの有効期間中に使用できるし、この IDictionary オブジェクトに項目を追加できます。</span><span class="sxs-lookup"><span data-stu-id="da91b-243">You can add items to this IDictionary object and they will be available to you throughout the lifetime of the context.</span></span>

## <a name="maintainscrollpositiononpostback"></a><span data-ttu-id="da91b-244">MaintainScrollPositionOnPostBack</span><span class="sxs-lookup"><span data-stu-id="da91b-244">MaintainScrollPositionOnPostBack</span></span>

<span data-ttu-id="da91b-245">このプロパティは、ASP.NET がページのスクロールのポストバックが発生した後、ブラウザー内の位置を保持する JavaScript を生成するかどうかを制御します。</span><span class="sxs-lookup"><span data-stu-id="da91b-245">This property controls whether or not ASP.NET emits JavaScript that maintains the pages scroll position in the browser after a postback occurs.</span></span> <span data-ttu-id="da91b-246">(このプロパティの詳細については、この章で説明した)。</span><span class="sxs-lookup"><span data-stu-id="da91b-246">(Details of this property were discussed earlier in this module.)</span></span>

## <a name="master"></a><span data-ttu-id="da91b-247">マスター</span><span class="sxs-lookup"><span data-stu-id="da91b-247">Master</span></span>

<span data-ttu-id="da91b-248">この読み取り専用プロパティでは、マスター ページが適用されているページの MasterPage インスタンスへの参照を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-248">This read-only property returns a reference to the MasterPage instance for a page to which a master page has been applied.</span></span>

## <a name="masterpagefile"></a><span data-ttu-id="da91b-249">MasterPageFile</span><span class="sxs-lookup"><span data-stu-id="da91b-249">MasterPageFile</span></span>

<span data-ttu-id="da91b-250">取得またはページのマスター ページのファイル名を設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-250">Gets or sets the master page filename for the page.</span></span> <span data-ttu-id="da91b-251">このプロパティは、PreInit メソッドでのみ設定できます。</span><span class="sxs-lookup"><span data-stu-id="da91b-251">This property can only be set in the PreInit method.</span></span>

## <a name="maxpagestatefieldlength"></a><span data-ttu-id="da91b-252">MaxPageStateFieldLength</span><span class="sxs-lookup"><span data-stu-id="da91b-252">MaxPageStateFieldLength</span></span>

<span data-ttu-id="da91b-253">このプロパティを取得またはページの状態の最大長をバイト単位で設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-253">This property gets or sets the maximum length for the pages state in bytes.</span></span> <span data-ttu-id="da91b-254">プロパティが正の数値に設定されている場合ページ ビュー ステートに分割されます。 複数の隠しフィールド指定されたバイト数を超えないようにします。</span><span class="sxs-lookup"><span data-stu-id="da91b-254">If the property is set to a positive number, the pages view state will be broken up into multiple hidden fields so that it doesnt exceed the number of bytes specified.</span></span> <span data-ttu-id="da91b-255">プロパティが負の数値の場合は、ビュー ステートはチャンクに分割されません。</span><span class="sxs-lookup"><span data-stu-id="da91b-255">If the property is a negative number, the view state will not be broken into chunks.</span></span>

## <a name="pageadapter"></a><span data-ttu-id="da91b-256">PageAdapter</span><span class="sxs-lookup"><span data-stu-id="da91b-256">PageAdapter</span></span>

<span data-ttu-id="da91b-257">要求元ブラウザーのページを変更する PageAdapter オブジェクトへの参照を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-257">Returns a reference to the PageAdapter object that modifies the page for the requesting browser.</span></span>

## <a name="previouspage"></a><span data-ttu-id="da91b-258">PreviousPage</span><span class="sxs-lookup"><span data-stu-id="da91b-258">PreviousPage</span></span>

<span data-ttu-id="da91b-259">Server.Transfer のページ間のポストバックの場合、前のページへの参照を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-259">Returns a reference to the previous page in cases of a Server.Transfer or a cross-page postback.</span></span>

## <a name="skinid"></a><span data-ttu-id="da91b-260">SkinID</span><span class="sxs-lookup"><span data-stu-id="da91b-260">SkinID</span></span>

<span data-ttu-id="da91b-261">ASP.NET 2.0 に適用するスキン ページを指定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-261">Specifies the ASP.NET 2.0 skin to apply to the page.</span></span>

## <a name="stylesheettheme"></a><span data-ttu-id="da91b-262">StyleSheetTheme</span><span class="sxs-lookup"><span data-stu-id="da91b-262">StyleSheetTheme</span></span>

<span data-ttu-id="da91b-263">このプロパティを取得または設定ページに適用されるスタイル シートです。</span><span class="sxs-lookup"><span data-stu-id="da91b-263">This property gets or sets the style sheet that is applied to a page.</span></span>

## <a name="templatecontrol"></a><span data-ttu-id="da91b-264">TemplateControl</span><span class="sxs-lookup"><span data-stu-id="da91b-264">TemplateControl</span></span>

<span data-ttu-id="da91b-265">ページの親コントロールへの参照を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-265">Returns a reference to the containing control for the page.</span></span>

## <a name="theme"></a><span data-ttu-id="da91b-266">テーマ</span><span class="sxs-lookup"><span data-stu-id="da91b-266">Theme</span></span>

<span data-ttu-id="da91b-267">取得または設定 ページに適用されている ASP.NET 2.0 のテーマの名前。</span><span class="sxs-lookup"><span data-stu-id="da91b-267">Gets or sets the name of the ASP.NET 2.0 theme applied to the page.</span></span> <span data-ttu-id="da91b-268">PreInit メソッドの前に、この値を設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-268">This value must be set prior to the PreInit method.</span></span>

## <a name="title"></a><span data-ttu-id="da91b-269">タイトル</span><span class="sxs-lookup"><span data-stu-id="da91b-269">Title</span></span>

<span data-ttu-id="da91b-270">このプロパティを取得またはページのヘッダーから取得した、ページのタイトルを設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-270">This property gets or sets the title for the page as obtained from the pages header.</span></span>

## <a name="viewstateencryptionmode"></a><span data-ttu-id="da91b-271">ViewStateEncryptionMode</span><span class="sxs-lookup"><span data-stu-id="da91b-271">ViewStateEncryptionMode</span></span>

<span data-ttu-id="da91b-272">取得またはページの ViewStateEncryptionMode を設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-272">Gets or sets the ViewStateEncryptionMode of the page.</span></span> <span data-ttu-id="da91b-273">このモジュールには、このプロパティの詳細についてを参照してください。</span><span class="sxs-lookup"><span data-stu-id="da91b-273">See a detailed discussion of this property earlier in this module.</span></span>

## <a name="new-protected-properties-of-the-page-class"></a><span data-ttu-id="da91b-274">ページのクラスの新しいプロテクト プロパティ</span><span class="sxs-lookup"><span data-stu-id="da91b-274">New Protected Properties of the Page Class</span></span>

<span data-ttu-id="da91b-275">ASP.NET 2.0 では、ページ クラスの新しい保護されているプロパティを次に示します。</span><span class="sxs-lookup"><span data-stu-id="da91b-275">The following are the new protected properties of the Page class in ASP.NET 2.0.</span></span>

## <a name="adapter"></a><span data-ttu-id="da91b-276">アダプター</span><span class="sxs-lookup"><span data-stu-id="da91b-276">Adapter</span></span>

<span data-ttu-id="da91b-277">デバイス上のページを表示するある ControlAdapter への参照が要求を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-277">Returns a reference to the ControlAdapter that renders the page on the device that requested it.</span></span>

## <a name="asyncmode"></a><span data-ttu-id="da91b-278">AsyncMode</span><span class="sxs-lookup"><span data-stu-id="da91b-278">AsyncMode</span></span>

<span data-ttu-id="da91b-279">このプロパティは、ページが非同期的に処理されるかどうかを示します。</span><span class="sxs-lookup"><span data-stu-id="da91b-279">This property indicates whether or not the page is processed asynchronously.</span></span> <span data-ttu-id="da91b-280">これは、使用するため、ランタイムによってコードで直接します。</span><span class="sxs-lookup"><span data-stu-id="da91b-280">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="clientidseparator"></a><span data-ttu-id="da91b-281">ClientIDSeparator</span><span class="sxs-lookup"><span data-stu-id="da91b-281">ClientIDSeparator</span></span>

<span data-ttu-id="da91b-282">このプロパティは、コントロールの一意のクライアント Id を作成するときに、区切り記号として使用される文字を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-282">This property returns the character used as a separator when creating unique client IDs for controls.</span></span> <span data-ttu-id="da91b-283">これは、使用するため、ランタイムによってコードで直接します。</span><span class="sxs-lookup"><span data-stu-id="da91b-283">It is intended for use by the runtime and not directly in code.</span></span>

## <a name="pagestatepersister"></a><span data-ttu-id="da91b-284">PageStatePersister</span><span class="sxs-lookup"><span data-stu-id="da91b-284">PageStatePersister</span></span>

<span data-ttu-id="da91b-285">このプロパティは、ページの PageStatePersister オブジェクトを返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-285">This property returns the PageStatePersister object for the page.</span></span> <span data-ttu-id="da91b-286">このプロパティは、主に、ASP.NET コントロールの開発者によって使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-286">This property is primarily used by ASP.NET control developers.</span></span>

## <a name="uniquefilepathsuffix"></a><span data-ttu-id="da91b-287">UniqueFilePathSuffix</span><span class="sxs-lookup"><span data-stu-id="da91b-287">UniqueFilePathSuffix</span></span>

<span data-ttu-id="da91b-288">このプロパティは、ブラウザーのキャッシュ ファイルのパスに追加される一意 suffic を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-288">This property returns a unique suffic that is appended to the file path for caching browsers.</span></span> <span data-ttu-id="da91b-289">既定値は\_ \__ufps = と 6 桁の数字です。</span><span class="sxs-lookup"><span data-stu-id="da91b-289">The default value is \_\_ufps= and a 6-digit number.</span></span>

## <a name="new-public-methods-for-the-page-class"></a><span data-ttu-id="da91b-290">ページ クラスの新しいパブリック メソッド</span><span class="sxs-lookup"><span data-stu-id="da91b-290">New Public Methods for the Page Class</span></span>

<span data-ttu-id="da91b-291">次のパブリック メソッドは、ASP.NET 2.0 内のページ クラスにします。</span><span class="sxs-lookup"><span data-stu-id="da91b-291">The following public methods are new to the Page class in ASP.NET 2.0.</span></span>

## <a name="addonprerendercompleteasync"></a><span data-ttu-id="da91b-292">AddOnPreRenderCompleteAsync</span><span class="sxs-lookup"><span data-stu-id="da91b-292">AddOnPreRenderCompleteAsync</span></span>

<span data-ttu-id="da91b-293">このメソッドは、非同期ページの実行のイベント ハンドラー デリゲートを登録します。</span><span class="sxs-lookup"><span data-stu-id="da91b-293">This method registers event handler delegates for asynchronous page execution.</span></span> <span data-ttu-id="da91b-294">このモジュールでは、非同期ページがについて説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-294">Asynchronous pages are discussed later in this module.</span></span>

## <a name="applystylesheetskin"></a><span data-ttu-id="da91b-295">ApplyStyleSheetSkin</span><span class="sxs-lookup"><span data-stu-id="da91b-295">ApplyStyleSheetSkin</span></span>

<span data-ttu-id="da91b-296">ページには、ページのスタイル シートでプロパティを適用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-296">Applies the properties in a pages style sheet to the page.</span></span>

## <a name="executeregisteredasynctasks"></a><span data-ttu-id="da91b-297">ExecuteRegisteredAsyncTasks</span><span class="sxs-lookup"><span data-stu-id="da91b-297">ExecuteRegisteredAsyncTasks</span></span>

<span data-ttu-id="da91b-298">このメソッドの開始、非同期タスク。</span><span class="sxs-lookup"><span data-stu-id="da91b-298">This method beings an asynchronous task.</span></span>

### <a name="getvalidators"></a><span data-ttu-id="da91b-299">GetValidators</span><span class="sxs-lookup"><span data-stu-id="da91b-299">GetValidators</span></span>

<span data-ttu-id="da91b-300">指定されていない場合は、指定された検証グループまたは既定の検証グループの検証コントロールのコレクションを返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-300">Returns a collection of validators for the specified validation group or the default validation group if none is specified.</span></span>

## <a name="registerasynctask"></a><span data-ttu-id="da91b-301">RegisterAsyncTask</span><span class="sxs-lookup"><span data-stu-id="da91b-301">RegisterAsyncTask</span></span>

<span data-ttu-id="da91b-302">このメソッドは、新しい非同期タスクを登録します。</span><span class="sxs-lookup"><span data-stu-id="da91b-302">This method registers a new async task.</span></span> <span data-ttu-id="da91b-303">このモジュールでは、非同期ページがについて説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-303">Asynchronous pages are covered later in this module.</span></span>

## <a name="registerrequirescontrolstate"></a><span data-ttu-id="da91b-304">RegisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="da91b-304">RegisterRequiresControlState</span></span>

<span data-ttu-id="da91b-305">この方法では ASP.NET ページ コントロールの状態を永続化する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-305">This method tells ASP.NET that the pages control state must be persisted.</span></span>

## <a name="registerrequiresviewstateencryption"></a><span data-ttu-id="da91b-306">RegisterRequiresViewStateEncryption</span><span class="sxs-lookup"><span data-stu-id="da91b-306">RegisterRequiresViewStateEncryption</span></span>

<span data-ttu-id="da91b-307">このメソッドは、ページの viewstate が暗号化を要求することを ASP.NET に指示します。</span><span class="sxs-lookup"><span data-stu-id="da91b-307">This method tells ASP.NET that the pages viewstate requires encryption.</span></span>

## <a name="resolveclienturl"></a><span data-ttu-id="da91b-308">ResolveClientUrl</span><span class="sxs-lookup"><span data-stu-id="da91b-308">ResolveClientUrl</span></span>

<span data-ttu-id="da91b-309">画像などのクライアント要求のために使用する相対 URL を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-309">Returns a relative URL that can be used for client requests for images, etc.</span></span>

## <a name="setfocus"></a><span data-ttu-id="da91b-310">SetFocus</span><span class="sxs-lookup"><span data-stu-id="da91b-310">SetFocus</span></span>

<span data-ttu-id="da91b-311">このメソッドは、ページが最初に読み込まれるときに指定されているコントロールにフォーカスが設定されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-311">This method will set the focus to the control that is specified when the page is initially loaded.</span></span>

## <a name="unregisterrequirescontrolstate"></a><span data-ttu-id="da91b-312">UnregisterRequiresControlState</span><span class="sxs-lookup"><span data-stu-id="da91b-312">UnregisterRequiresControlState</span></span>

<span data-ttu-id="da91b-313">このメソッドはコントロールを不要になったコントロールの状態の永続化を必須として渡すときに登録を解除します。</span><span class="sxs-lookup"><span data-stu-id="da91b-313">This method will unregister the control that is passed to it as no longer requiring control state persistence.</span></span>

## <a name="changes-to-the-page-lifecycle"></a><span data-ttu-id="da91b-314">ページのライフ サイクルへの変更</span><span class="sxs-lookup"><span data-stu-id="da91b-314">Changes to the Page Lifecycle</span></span>

<span data-ttu-id="da91b-315">ASP.NET 2.0 のページのライフ サイクルが大幅に変更されていないが認識する必要があるいくつかの新しいメソッドがあります。</span><span class="sxs-lookup"><span data-stu-id="da91b-315">The page lifecycle in ASP.NET 2.0 hasnt changed dramatically, but there are some new methods that you should be aware of.</span></span> <span data-ttu-id="da91b-316">以下を ASP.NET 2.0 ページ ライフ サイクルについて説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-316">The ASP.NET 2.0 page lifecycle is outlined below.</span></span>

## <a name="preinit-new-in-aspnet-20"></a><span data-ttu-id="da91b-317">PreInit (新規の ASP.NET 2.0)</span><span class="sxs-lookup"><span data-stu-id="da91b-317">PreInit (New in ASP.NET 2.0)</span></span>

<span data-ttu-id="da91b-318">PreInit イベントは、開発者がアクセスできるライフ サイクルの早い段階です。</span><span class="sxs-lookup"><span data-stu-id="da91b-318">The PreInit event is the earliest stage in the lifecycle that a developer can access.</span></span> <span data-ttu-id="da91b-319">このイベントを追加できるようになりますプログラムでマスター ページ、ASP.NET 2.0 のテーマの変更、ASP.NET 2.0 プロファイルなどのプロパティにアクセスします。Viewstate がまだ適用されていない、ライフ サイクルでこの時点でのコントロールを実現することは重要ポストバックの状態の場合は。</span><span class="sxs-lookup"><span data-stu-id="da91b-319">The addition of this event makes it possible to programmatically change ASP.NET 2.0 themes, master pages, access properties for an ASP.NET 2.0 profile, etc. If you are in a postback state, its important to realize that Viewstate has not yet been applied to controls at this point in the lifecycle.</span></span> <span data-ttu-id="da91b-320">そのため、開発者は、この段階で、コントロールのプロパティを変更する場合、ページのライフ サイクルの後半可能性があります上書きされます。</span><span class="sxs-lookup"><span data-stu-id="da91b-320">Therefore, if a developer changes a property of a control at this stage, it will likely be overwritten later in the pages lifecycle.</span></span>

## <a name="init"></a><span data-ttu-id="da91b-321">Init</span><span class="sxs-lookup"><span data-stu-id="da91b-321">Init</span></span>

<span data-ttu-id="da91b-322">ASP.NET から Init イベントが変更されていない 1.x です。</span><span class="sxs-lookup"><span data-stu-id="da91b-322">The Init event has not changed from ASP.NET 1.x.</span></span> <span data-ttu-id="da91b-323">これは、読み取りまたはページ上のコントロールのプロパティを初期化する場所です。</span><span class="sxs-lookup"><span data-stu-id="da91b-323">This is where you would want to read or initialize properties of controls on your page.</span></span> <span data-ttu-id="da91b-324">このステージ、マスター ページ、テーマでなどは、ページに既に適用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-324">At this stage, master pages, themes, etc. are already applied to the page.</span></span>

## <a name="initcomplete-new-in-20"></a><span data-ttu-id="da91b-325">(2.0) で新しい InitComplete</span><span class="sxs-lookup"><span data-stu-id="da91b-325">InitComplete (New in 2.0)</span></span>

<span data-ttu-id="da91b-326">InitComplete イベントは、ページの初期化段階の最後に呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-326">The InitComplete event is called at the end of the pages initialization stage.</span></span> <span data-ttu-id="da91b-327">この時点で、ライフ サイクルでアクセスできますが、ページ上のコントロールの状態が設定されていません。</span><span class="sxs-lookup"><span data-stu-id="da91b-327">At this point in the lifecycle, you can access controls on the page, but their state has not yet been populated.</span></span>

## <a name="preload-new-in-20"></a><span data-ttu-id="da91b-328">プリロード (2.0 の新機能)</span><span class="sxs-lookup"><span data-stu-id="da91b-328">PreLoad (New in 2.0)</span></span>

<span data-ttu-id="da91b-329">このイベントは、すべてのポストバック データが適用された後、ページの直前に呼び出されますが\_ロードします。</span><span class="sxs-lookup"><span data-stu-id="da91b-329">This event is called after all postback data has been applied and just prior to Page\_Load.</span></span>

## <a name="load"></a><span data-ttu-id="da91b-330">Load</span><span class="sxs-lookup"><span data-stu-id="da91b-330">Load</span></span>

<span data-ttu-id="da91b-331">ASP.NET から Load イベントが変更されていない 1.x です。</span><span class="sxs-lookup"><span data-stu-id="da91b-331">The Load event has not changed from ASP.NET 1.x.</span></span>

## <a name="loadcomplete-new-in-20"></a><span data-ttu-id="da91b-332">(2.0) で新しい LoadComplete</span><span class="sxs-lookup"><span data-stu-id="da91b-332">LoadComplete (New in 2.0)</span></span>

<span data-ttu-id="da91b-333">LoadComplete イベントは、ページ読み込みの段階での最後のイベントです。</span><span class="sxs-lookup"><span data-stu-id="da91b-333">The LoadComplete event is the last event in the pages load stage.</span></span> <span data-ttu-id="da91b-334">この段階では、ポストバックおよび viewstate すべてのデータがページに適用されました。</span><span class="sxs-lookup"><span data-stu-id="da91b-334">At this stage, all postback and viewstate data has been applied to the page.</span></span>

## <a name="prerender"></a><span data-ttu-id="da91b-335">PreRender</span><span class="sxs-lookup"><span data-stu-id="da91b-335">PreRender</span></span>

<span data-ttu-id="da91b-336">ページに動的に追加されるコントロールを保持する正しく viewstate の場合は、それらを追加する最後の機会は PreRender イベントです。</span><span class="sxs-lookup"><span data-stu-id="da91b-336">If you would like for viewstate to be properly maintained for controls that are added to the page dynamically, the PreRender event is the last opportunity to add them.</span></span>

## <a name="prerendercomplete-new-in-20"></a><span data-ttu-id="da91b-337">(2.0) で新しい PreRenderComplete</span><span class="sxs-lookup"><span data-stu-id="da91b-337">PreRenderComplete (New in 2.0)</span></span>

<span data-ttu-id="da91b-338">PreRenderComplete 段階では、ページに追加されたすべてのコントロールと、ページを描画する準備ができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-338">At the PreRenderComplete stage, all controls have been added to the page and the page is ready to be rendered.</span></span> <span data-ttu-id="da91b-339">PreRenderComplete イベントは、最後のページの viewstate を保存する前に発生するイベントです。</span><span class="sxs-lookup"><span data-stu-id="da91b-339">The PreRenderComplete event is the last event raised before the pages viewstate is saved.</span></span>

## <a name="savestatecomplete-new-in-20"></a><span data-ttu-id="da91b-340">(2.0) で新しい SaveStateComplete</span><span class="sxs-lookup"><span data-stu-id="da91b-340">SaveStateComplete (New in 2.0)</span></span>

<span data-ttu-id="da91b-341">SaveStateComplete イベントには、すべてのページの viewstate とコントロールの状態を保存した後すぐには呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-341">The SaveStateComplete event is called immediately after all page viewstate and control state has been saved.</span></span> <span data-ttu-id="da91b-342">これは、ページがブラウザーに実際に表示される前に、最後のイベントです。</span><span class="sxs-lookup"><span data-stu-id="da91b-342">This is the last event before the page is actually rendered to the browser.</span></span>

## <a name="render"></a><span data-ttu-id="da91b-343">レンダリング</span><span class="sxs-lookup"><span data-stu-id="da91b-343">Render</span></span>

<span data-ttu-id="da91b-344">Render メソッドが ASP.NET 以降変更されていない 1.x です。</span><span class="sxs-lookup"><span data-stu-id="da91b-344">The Render method has not changed since ASP.NET 1.x.</span></span> <span data-ttu-id="da91b-345">これは、ここでは、HtmlTextWriter に初期化され、ページがブラウザーにレンダリングされます。</span><span class="sxs-lookup"><span data-stu-id="da91b-345">This is where the HtmlTextWriter is initialized and the page is rendered to the browser.</span></span>

## <a name="cross-page-postback-in-aspnet-20"></a><span data-ttu-id="da91b-346">ASP.NET 2.0 のページ間のポストバック</span><span class="sxs-lookup"><span data-stu-id="da91b-346">Cross-Page Postback in ASP.NET 2.0</span></span>

<span data-ttu-id="da91b-347">ASP.NET で 1.x では、ポストバックが同じページにポストするために必要です。</span><span class="sxs-lookup"><span data-stu-id="da91b-347">In ASP.NET 1.x, postbacks were required to post to the same page.</span></span> <span data-ttu-id="da91b-348">複数ページのポストバックが許可されていません。</span><span class="sxs-lookup"><span data-stu-id="da91b-348">Cross-page postbacks were not allowed.</span></span> <span data-ttu-id="da91b-349">ASP.NET 2.0 では、IButtonControl インターフェイス経由で別のページへのポストバックをする機能を追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-349">ASP.NET 2.0 adds the ability to post back to a different page via the IButtonControl interface.</span></span> <span data-ttu-id="da91b-350">(ボタン、LinkButton、およびサード パーティのカスタム コントロールのほかの ImageButton) は、新しい IButtonControl インターフェイスを実装する任意のコントロールでは、PostBackUrl 属性を使用してこの新しい機能を利用できます。</span><span class="sxs-lookup"><span data-stu-id="da91b-350">Any control that implements the new IButtonControl interface (Button, LinkButton, and ImageButton in addition to third-party custom controls) can take advantage of this new functionality via the use of the PostBackUrl attribute.</span></span> <span data-ttu-id="da91b-351">次のコードでは、2 番目のページへのポストバックを Button コントロールを示します。</span><span class="sxs-lookup"><span data-stu-id="da91b-351">The following code shows a Button control that posts back to a second page.</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample5.aspx)]

<span data-ttu-id="da91b-352">ページがポストバック時に、ポストバックを開始するページは、2 ページ目で PreviousPage プロパティからアクセス可能です。</span><span class="sxs-lookup"><span data-stu-id="da91b-352">When the page is posted back, the Page that initiates the postback is accessible via the PreviousPage property on the second page.</span></span> <span data-ttu-id="da91b-353">新しい web フォームを使用してこの機能は実装されて\_DoPostBackWithOptions クライアント側の関数は別のページへのコントロールのポストバック時に、ASP.NET 2.0 がページを表示します。</span><span class="sxs-lookup"><span data-stu-id="da91b-353">This functionality is implemented via the new WebForm\_DoPostBackWithOptions client-side function that ASP.NET 2.0 renders to the page when a control posts back to a different page.</span></span> <span data-ttu-id="da91b-354">この JavaScript 関数は、クライアントにスクリプトを生成する新しい WebResource.axd ハンドラーによって提供されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-354">This JavaScript function is provided by the new WebResource.axd handler which emits script to the client.</span></span>

<span data-ttu-id="da91b-355">次のビデオは、ページ間のポストバックのチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="da91b-355">The video below is a walkthrough of a cross-page postback.</span></span>


![](the-asp-net-2-0-page-model/_static/image2.png)


[<span data-ttu-id="da91b-356">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="da91b-356">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/xpage1.wmv)


## <a name="more-details-on-cross-page-postbacks"></a><span data-ttu-id="da91b-357">ポストバックの詳細について</span><span class="sxs-lookup"><span data-stu-id="da91b-357">More Details on Cross-Page Postbacks</span></span>

### <a name="viewstate"></a><span data-ttu-id="da91b-358">Viewstate</span><span class="sxs-lookup"><span data-stu-id="da91b-358">Viewstate</span></span>

<span data-ttu-id="da91b-359">要求がありますが自分で既にページ間のポストバック シナリオでは最初のページから、viewstate をどのようになるか。</span><span class="sxs-lookup"><span data-stu-id="da91b-359">You may have asked yourself already about what happens to the viewstate from the first page in a cross-page postback scenario.</span></span> <span data-ttu-id="da91b-360">結局のところ、IPostBackDataHandler を実装しない任意のコントロールがページ間のポストバックの 2 ページ目では、そのコントロールのプロパティにアクセスする権限が viewstate、経由での状態をように保持されます、ページの viewstate にアクセスする必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-360">After all, any control that does not implement IPostBackDataHandler will persist its state via viewstate, so to have access to the properties of that control on the second page of a cross-page postback, you must have access to the viewstate for the page.</span></span> <span data-ttu-id="da91b-361">ASP.NET 2.0 と呼ばれる 2 番目のページの別の非表示フィールドを使用してこのシナリオの処理\_ \_PREVIOUSPAGE です。</span><span class="sxs-lookup"><span data-stu-id="da91b-361">ASP.NET 2.0 takes care of this scenario using a new hidden field in the second page called \_\_PREVIOUSPAGE.</span></span> <span data-ttu-id="da91b-362">\_ \_PREVIOUSPAGE フォーム フィールドは、最初のページの viewstate を含むため、2 番目のページにすべてのコントロールのプロパティにアクセスすることがあるできます。</span><span class="sxs-lookup"><span data-stu-id="da91b-362">The \_\_PREVIOUSPAGE form field contains the viewstate for the first page so that you can have access to the properties of all controls in the second page.</span></span>

### <a name="circumventing-findcontrol"></a><span data-ttu-id="da91b-363">FindControl を回避する方法</span><span class="sxs-lookup"><span data-stu-id="da91b-363">Circumventing FindControl</span></span>

<span data-ttu-id="da91b-364">ページ間のポストバックのビデオ チュートリアルでは、FindControl メソッドを使用すれば、最初のページにテキスト ボックス コントロールへの参照を取得します。</span><span class="sxs-lookup"><span data-stu-id="da91b-364">In the video walkthrough of a cross-page postback, I used the FindControl method to get a reference to the TextBox control on the first page.</span></span> <span data-ttu-id="da91b-365">そのメソッドは、そのような目的に適してが FindControl 高価なは、追加のコードを記述する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-365">That method works well for that purpose, but FindControl is expensive and it requires writing additional code.</span></span> <span data-ttu-id="da91b-366">幸いにも、ASP.NET 2.0 では、多くのシナリオでは動作をこの目的のため FindControl する代わりを提供します。</span><span class="sxs-lookup"><span data-stu-id="da91b-366">Fortunately, ASP.NET 2.0 provides an alternative to FindControl for this purpose that will work in many scenarios.</span></span> <span data-ttu-id="da91b-367">方法ディレクティブを使用すると、TypeName または VirtualPath 属性を使用して、前のページへの厳密に型指定された参照があることができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-367">The PreviousPageType directive allows you to have a strongly-typed reference to the previous page by using either the TypeName or the VirtualPath attribute.</span></span> <span data-ttu-id="da91b-368">TypeName 属性では、VirtualPath 属性では、仮想パスを使用して、前のページを参照することができます、前のページの種類を指定することができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-368">The TypeName attribute allows you to specify the type of the previous page while the VirtualPath attribute allows you to refer to the previous page using a virtual path.</span></span> <span data-ttu-id="da91b-369">方法ディレクティブを設定すると、後に、コントロールなどのパブリック プロパティを使用してアクセスを許可するを公開する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-369">After you've set the PreviousPageType directive, you must then expose the controls, etc. to which you want to allow access using public properties.</span></span>

## <a name="lab-1-cross-page-postback"></a><span data-ttu-id="da91b-370">ラボ 1 ページ間のポストバック</span><span class="sxs-lookup"><span data-stu-id="da91b-370">Lab 1 Cross-Page Postback</span></span>

<span data-ttu-id="da91b-371">このラボでは、ASP.NET 2.0 の新しいページ間のポストバック機能を使用するアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="da91b-371">In this lab, you will create an application that uses the new cross-page postback functionality of ASP.NET 2.0.</span></span>

1. <span data-ttu-id="da91b-372">Visual Studio 2005 を開き、新しい ASP.NET Web サイトを作成します。</span><span class="sxs-lookup"><span data-stu-id="da91b-372">Open Visual Studio 2005 and create a new ASP.NET Web site.</span></span>
2. <span data-ttu-id="da91b-373">Page2.aspx と呼ばれる新しい web フォームを追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-373">Add a new Webform called page2.aspx.</span></span>
3. <span data-ttu-id="da91b-374">デザイン ビューで、Default.aspx を開き、ボタン コントロールとテキスト ボックス コントロールを追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-374">Open the Default.aspx in Design view and add a Button control and a TextBox control.</span></span> 

    1. <span data-ttu-id="da91b-375">ボタン コントロールの ID を与える**損害賠償**、テキスト ボックス コントロールの ID と**UserName**です。</span><span class="sxs-lookup"><span data-stu-id="da91b-375">Give the Button control an ID of **SubmitButton** and the TextBox control an ID of **UserName**.</span></span>
    2. <span data-ttu-id="da91b-376">Page2.aspx にボタンの PostBackUrl プロパティを設定します。</span><span class="sxs-lookup"><span data-stu-id="da91b-376">Set the PostBackUrl property of the Button to page2.aspx.</span></span>
4. <span data-ttu-id="da91b-377">ソース ビューで page2.aspx を開きます。</span><span class="sxs-lookup"><span data-stu-id="da91b-377">Open page2.aspx in Source view.</span></span>
5. <span data-ttu-id="da91b-378">次に示すように、@ 方法ディレクティブを追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-378">Add a @ PreviousPageType directive as shown below:</span></span>
6. <span data-ttu-id="da91b-379">次のコード ページを追加\_page2.aspx の分離コードの読み込み。</span><span class="sxs-lookup"><span data-stu-id="da91b-379">Add the following code to the Page\_Load of page2.aspx's code-behind:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample6.cs)]
7. <span data-ttu-id="da91b-380">ビルド [ビルド] メニューをクリックして、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="da91b-380">Build the project by clicking on Build on the Build menu.</span></span>
8. <span data-ttu-id="da91b-381">Default.aspx の分離コードに次のコードを追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-381">Add the following code to the code-behind for Default.aspx:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample7.cs)]
9. <span data-ttu-id="da91b-382">ページを変更する\_次 page2.aspx への読み込み。</span><span class="sxs-lookup"><span data-stu-id="da91b-382">Change the Page\_Load in page2.aspx to the following:</span></span> 

    [!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample8.cs)]
10. <span data-ttu-id="da91b-383">プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="da91b-383">Build the project.</span></span>
11. <span data-ttu-id="da91b-384">プロジェクトを実行します。</span><span class="sxs-lookup"><span data-stu-id="da91b-384">Run the project.</span></span>
12. <span data-ttu-id="da91b-385">テキスト ボックスに名前を入力し、ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="da91b-385">Enter your name in the TextBox and click the button.</span></span>
13. <span data-ttu-id="da91b-386">結果は何ですか。</span><span class="sxs-lookup"><span data-stu-id="da91b-386">What is the result?</span></span>

## <a name="asynchronous-pages-in-aspnet-20"></a><span data-ttu-id="da91b-387">ASP.NET 2.0 での非同期のページ</span><span class="sxs-lookup"><span data-stu-id="da91b-387">Asynchronous Pages in ASP.NET 2.0</span></span>

<span data-ttu-id="da91b-388">ASP.NET で多くの競合の問題は、(Web サービスまたはデータベースの呼び出し) などの外部の呼び出し、ファイル IO の待機時間などの待機時間が原因です。ASP.NET アプリケーションに対して要求が行われると、ASP.NET はその要求を処理するのに 1 つのワーカー スレッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-388">Many contention problems in ASP.NET are caused by latency of external calls (such as Web service or database calls), file IO latency, etc. When a request is made against an ASP.NET application, ASP.NET uses one of its worker threads to service that request.</span></span> <span data-ttu-id="da91b-389">その要求では、要求が完了し、応答が送信されたまで、そのスレッドが所有しています。</span><span class="sxs-lookup"><span data-stu-id="da91b-389">That request owns that thread until the request is complete and the response has been sent.</span></span> <span data-ttu-id="da91b-390">ASP.NET 2.0 がページを非同期的に実行する機能を追加することでこの種の問題と待機時間に関する問題を解決しようとするとします。</span><span class="sxs-lookup"><span data-stu-id="da91b-390">ASP.NET 2.0 seeks to resolve latency issues with these types of issues by adding the capability to execute pages asynchronously.</span></span> <span data-ttu-id="da91b-391">つまり、ワーカー スレッドが、要求の開始し、し、それによって、使用可能なスレッド プールにすばやくを返す別のスレッドに追加の実行を渡すことができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-391">That means that a worker thread can start the request and then hand off additional execution to another thread, thereby returning to the available thread pool quickly.</span></span> <span data-ttu-id="da91b-392">ファイル IO、データベースの呼び出しなどが完了したら、新しいスレッドは、要求を終了するスレッド プールから取得されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-392">When the file IO, database call, etc. has completed, a new thread is obtained from the thread pool to finish the request.</span></span>

<span data-ttu-id="da91b-393">ページを非同期的に実行する最初の手順を設定、 **Async**ようページ ディレクティブの属性。</span><span class="sxs-lookup"><span data-stu-id="da91b-393">The first step in making a page execute asynchronously is to set the **Async** attribute of the page directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample9.aspx)]

<span data-ttu-id="da91b-394">この属性では、ASP.NET ページの IHttpAsyncHandler を実装するように指示します。</span><span class="sxs-lookup"><span data-stu-id="da91b-394">This attribute tells ASP.NET to implement the IHttpAsyncHandler for the page.</span></span>

<span data-ttu-id="da91b-395">次の手順は、メソッドを呼び出す AddOnPreRenderCompleteAsync PreRender の前に、ページのライフ サイクルの時点でです。</span><span class="sxs-lookup"><span data-stu-id="da91b-395">The next step is to call the AddOnPreRenderCompleteAsync method at a point in the lifecycle of the page prior to PreRender.</span></span> <span data-ttu-id="da91b-396">(このメソッドと通常呼ばれるページの\_ロードします)。AddOnPreRenderCompleteAsync メソッドは 2 つのパラメーターです。BeginEventHandler し、EndEventHandler します。</span><span class="sxs-lookup"><span data-stu-id="da91b-396">(This method is typically called in Page\_Load.) The AddOnPreRenderCompleteAsync method takes two parameters; a BeginEventHandler and an EndEventHandler.</span></span> <span data-ttu-id="da91b-397">BeginEventHandler、EndEventHandler にパラメーターとして渡される、IAsyncResult を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-397">The BeginEventHandler returns an IAsyncResult which is then passed as a parameter to the EndEventHandler.</span></span>

<span data-ttu-id="da91b-398">下のビデオは、非同期のページ要求のチュートリアルです。</span><span class="sxs-lookup"><span data-stu-id="da91b-398">The video below is a walkthrough of an asynchronous page request.</span></span>


![](the-asp-net-2-0-page-model/_static/image3.png)


[<span data-ttu-id="da91b-399">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="da91b-399">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/async1.wmv)


> [!NOTE]
> <span data-ttu-id="da91b-400">EndEventHandler が完了するまで、非同期ページは、ブラウザーに表示されません。</span><span class="sxs-lookup"><span data-stu-id="da91b-400">An async page does not render to the browser until the EndEventHandler has completed.</span></span> <span data-ttu-id="da91b-401">されることがない状態が不明な一部の開発者に非同期要求を非同期コールバックのようなものとして認識します。</span><span class="sxs-lookup"><span data-stu-id="da91b-401">No doubt but that some developers will think of async requests as being similar to async callbacks.</span></span> <span data-ttu-id="da91b-402">ことが重要であることを実現します。</span><span class="sxs-lookup"><span data-stu-id="da91b-402">It's important to realize that they are not.</span></span> <span data-ttu-id="da91b-403">非同期要求のメリットは、最初のワーカー スレッドなど、IO がバインドされていることによって競合が減少サービス新しい要求をスレッド プールに返されることです。</span><span class="sxs-lookup"><span data-stu-id="da91b-403">The benefit to asynchronous requests is that the first worker thread can be returned to the thread pool to service new requests, thereby reducing contention due to being IO bound, etc.</span></span>


## <a name="script-callbacks-in-aspnet-20"></a><span data-ttu-id="da91b-404">ASP.NET 2.0 のスクリプトのコールバック</span><span class="sxs-lookup"><span data-stu-id="da91b-404">Script Callbacks in ASP.NET 2.0</span></span>

<span data-ttu-id="da91b-405">Web 開発者は、コールバックと関連付けられているちらつきを防ぐための方法については常に説明しました。</span><span class="sxs-lookup"><span data-stu-id="da91b-405">Web developers have always looked for ways to prevent the flickering associated with a callback.</span></span> <span data-ttu-id="da91b-406">ASP.NET で 1.x では、SmartNavigation がちらつき、回避するための最も一般的な方法が SmartNavigation クライアントでその実装の複雑さのため一部の開発者の問題の原因とします。</span><span class="sxs-lookup"><span data-stu-id="da91b-406">In ASP.NET 1.x, SmartNavigation was the most common method for avoiding flickering, but SmartNavigation caused problems for some developers because of the complexity of its implementation on the client.</span></span> <span data-ttu-id="da91b-407">ASP.NET 2.0 では、スクリプトのコールバックでこの問題を説明します。</span><span class="sxs-lookup"><span data-stu-id="da91b-407">ASP.NET 2.0 addresses this issue with script callbacks.</span></span> <span data-ttu-id="da91b-408">スクリプトのコールバックでは、JavaScript を使用して Web サーバーに対する要求に XMLHttp を利用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-408">Script callbacks utilize XMLHttp to make requests against the Web server via JavaScript.</span></span> <span data-ttu-id="da91b-409">XMLHttp 要求は、ブラウザーの DOM を経由して操作できる、XML データを返します</span><span class="sxs-lookup"><span data-stu-id="da91b-409">The XMLHttp request returns XML data that can then be manipulated via the browser's DOM.</span></span> <span data-ttu-id="da91b-410">XMLHttp のコードは、ユーザーから、新しい WebResource.axd ハンドラーによって隠ぺいされます。</span><span class="sxs-lookup"><span data-stu-id="da91b-410">XMLHttp code is hidden from the user by the new WebResource.axd handler.</span></span>

<span data-ttu-id="da91b-411">ASP.NET 2.0 のスクリプトのコールバックを構成するために必要ないくつかの手順があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-411">There are several steps that are necessary in order to configure a script callback in ASP.NET 2.0.</span></span>

## <a name="step-1--implement-the-icallbackeventhandler-interface"></a><span data-ttu-id="da91b-412">手順 1: ICallbackEventHandler インターフェイスを実装します。</span><span class="sxs-lookup"><span data-stu-id="da91b-412">Step 1 : Implement the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="da91b-413">ASP.NET スクリプト コールバックに参加するいると、ページを認識するためには、ICallbackEventHandler インターフェイスを実装する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-413">In order for ASP.NET to recognize your page as participating in a script callback, you must implement the ICallbackEventHandler interface.</span></span> <span data-ttu-id="da91b-414">こうことは、分離コード ファイルで次のようにします。</span><span class="sxs-lookup"><span data-stu-id="da91b-414">You can do this in your code-behind file like so:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample10.cs)]

<span data-ttu-id="da91b-415">できますもこれを行うため、@ Implements ディレクティブ like を使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-415">You can also do this using the @ Implements directive like so:</span></span>

[!code-aspx[Main](the-asp-net-2-0-page-model/samples/sample11.aspx)]

<span data-ttu-id="da91b-416">通常、ASP.NET のインライン コードを使用する場合は、@ Implements ディレクティブを使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-416">You would typically use the @ Implements directive when using inline ASP.NET code.</span></span>

## <a name="step-2--call-getcallbackeventreference"></a><span data-ttu-id="da91b-417">手順 2: 呼び出し GetCallbackEventReference</span><span class="sxs-lookup"><span data-stu-id="da91b-417">Step 2 : Call GetCallbackEventReference</span></span>

<span data-ttu-id="da91b-418">前述のように、XMLHttp の呼び出しは、WebResource.axd ハンドラーでカプセル化されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-418">As mentioned previously, the XMLHttp call is encapsulated in the WebResource.axd handler.</span></span> <span data-ttu-id="da91b-419">ASP.NET が web フォームへの呼び出しを追加、ページが表示されると\_DoCallback、WebResource.axd によって提供されるクライアント スクリプトです。</span><span class="sxs-lookup"><span data-stu-id="da91b-419">When your page is rendered, ASP.NET will add a call to WebForm\_DoCallback, a client script that is provided by WebResource.axd.</span></span> <span data-ttu-id="da91b-420">Web フォーム\_DoCallback 関数は、 \_\_のコールバックを doPostBack 関数。</span><span class="sxs-lookup"><span data-stu-id="da91b-420">The WebForm\_DoCallback function replaces the \_\_doPostBack function for a callback.</span></span> <span data-ttu-id="da91b-421">注意して\_ \_doPostBack がプログラムで、ページのフォームを送信します。</span><span class="sxs-lookup"><span data-stu-id="da91b-421">Remember that \_\_doPostBack programmatically submits the form on the page.</span></span> <span data-ttu-id="da91b-422">そのため、ポストバックを防ぐためにするシナリオでは、コールバック、 \_ \_doPostBack だけでは不十分です。</span><span class="sxs-lookup"><span data-stu-id="da91b-422">In a callback scenario, you want to prevent a postback, so \_\_doPostBack will not suffice.</span></span>

> [!NOTE]
> <span data-ttu-id="da91b-423">\_\_doPostBack は引き続きクライアント スクリプト コールバック シナリオ内のページに表示されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-423">\_\_doPostBack is still rendered to the page in a client script callback scenario.</span></span> <span data-ttu-id="da91b-424">ただし、コールバックは使用されません。</span><span class="sxs-lookup"><span data-stu-id="da91b-424">However, it's not used for the callback.</span></span>


<span data-ttu-id="da91b-425">Web フォームの引数\_DoCallback クライアント側の関数は、サーバー側の関数に通常のページで呼び出されます GetCallbackEventReference を通じて提供\_ロードします。</span><span class="sxs-lookup"><span data-stu-id="da91b-425">The arguments for the WebForm\_DoCallback client-side function are provided via the server-side function GetCallbackEventReference which would normally be called in Page\_Load.</span></span> <span data-ttu-id="da91b-426">GetCallbackEventReference に通常の呼び出しは、次のようになります。</span><span class="sxs-lookup"><span data-stu-id="da91b-426">A typical call to GetCallbackEventReference might look like this:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample12.cs)]

> [!NOTE]
> <span data-ttu-id="da91b-427">この場合、cm、ClientScriptManager のインスタンスです。</span><span class="sxs-lookup"><span data-stu-id="da91b-427">In this case, cm is an instance of ClientScriptManager.</span></span> <span data-ttu-id="da91b-428">ClientScriptManager クラスは、後でこのモジュールで取り上げます。</span><span class="sxs-lookup"><span data-stu-id="da91b-428">The ClientScriptManager class will be covered later in this module.</span></span>


<span data-ttu-id="da91b-429">GetCallbackEventReference のいくつかのオーバー ロードされたバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="da91b-429">There are several overloaded versions of GetCallbackEventReference.</span></span> <span data-ttu-id="da91b-430">この場合、引数は次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="da91b-430">In this case, the arguments are as follows:</span></span>

`this`

<span data-ttu-id="da91b-431">GetCallbackEventReference が呼び出されているコントロールへの参照。</span><span class="sxs-lookup"><span data-stu-id="da91b-431">A reference to the control where GetCallbackEventReference is being called.</span></span> <span data-ttu-id="da91b-432">この場合、ページ自体を勧めします。</span><span class="sxs-lookup"><span data-stu-id="da91b-432">In this case, it's the page itself.</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample13.js)]

<span data-ttu-id="da91b-433">サーバー側のイベントをクライアント側のコードから渡される文字列引数。</span><span class="sxs-lookup"><span data-stu-id="da91b-433">A string argument that will be passed from the client-side code to the server-side event.</span></span> <span data-ttu-id="da91b-434">この場合、Im、ドロップダウンの値を渡すには、ddlCompany が呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-434">In this case, Im passing the value of a dropdown called ddlCompany.</span></span>

`ShowCompanyName`

<span data-ttu-id="da91b-435">サーバー側のコールバック イベントからの (文字列) としての戻り値が許容するクライアント側の関数の名前。</span><span class="sxs-lookup"><span data-stu-id="da91b-435">The name of the client-side function that will accept the return value (as string) from the server-side callback event.</span></span> <span data-ttu-id="da91b-436">この関数が、サーバー側のコールバックが成功した場合にのみ呼び出されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-436">This function will only be called when the server-side callback is successful.</span></span> <span data-ttu-id="da91b-437">したがって、堅牢性、ここでは、通常は推奨エラーが発生した場合に実行するクライアント側の関数の名前を指定する追加の文字列引数を受け取る GetCallbackEventReference のオーバー ロードされたバージョンを使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-437">Therefore, for the sake of robustness, it is generally recommended to use the overloaded version of GetCallbackEventReference that takes an additional string argument specifying the name of a client-side function to execute in the event of an error.</span></span>

`null`

<span data-ttu-id="da91b-438">サーバーにコールバックの前に開始したクライアント側の関数を表す文字列。</span><span class="sxs-lookup"><span data-stu-id="da91b-438">A string representing a client-side function that it initiated before the callback to the server.</span></span> <span data-ttu-id="da91b-439">ここではありません、このようなスクリプトのため、引数は null です。</span><span class="sxs-lookup"><span data-stu-id="da91b-439">In this case, there is no such script, so the argument is null.</span></span>

`true`

<span data-ttu-id="da91b-440">コールバックを非同期的に実行するかどうかを指定するブール値。</span><span class="sxs-lookup"><span data-stu-id="da91b-440">A Boolean specifying whether or not to conduct the callback asynchronously.</span></span>

<span data-ttu-id="da91b-441">Web フォームへの呼び出し\_DoCallback クライアントではこれらの引数を渡します。</span><span class="sxs-lookup"><span data-stu-id="da91b-441">The call to WebForm\_DoCallback on the client will pass these arguments.</span></span> <span data-ttu-id="da91b-442">そのため、クライアントでこのページが表示されると、そのコードになります次のようにします。</span><span class="sxs-lookup"><span data-stu-id="da91b-442">Therefore, when this page is rendered on the client, that code will look like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample14.js)]

<span data-ttu-id="da91b-443">クライアントでの関数のシグネチャが少し異なることに注意してください。</span><span class="sxs-lookup"><span data-stu-id="da91b-443">Notice that the signature of the function on the client is a bit different.</span></span> <span data-ttu-id="da91b-444">クライアント側の関数は、5 つの文字列とブール値を渡します。</span><span class="sxs-lookup"><span data-stu-id="da91b-444">The client-side function passes 5 strings and a Boolean.</span></span> <span data-ttu-id="da91b-445">(これは上記の例では null) 追加の文字列には、サーバー側のコールバックで発生したエラーを処理するクライアント側の関数が含まれています。</span><span class="sxs-lookup"><span data-stu-id="da91b-445">The additional string (which is null in the above example) contains the client-side function that will handle any errors from the server-side callback.</span></span>

## <a name="step-3--hook-the-client-side-control-event"></a><span data-ttu-id="da91b-446">手順 3: クライアント側コントロール イベントをフックします。</span><span class="sxs-lookup"><span data-stu-id="da91b-446">Step 3 : Hook the Client-Side Control Event</span></span>

<span data-ttu-id="da91b-447">GetCallbackEventReference 上記の戻り値が文字列変数に割り当てられたことに注意してください。</span><span class="sxs-lookup"><span data-stu-id="da91b-447">Notice that the return value of GetCallbackEventReference above was assigned to a string variable.</span></span> <span data-ttu-id="da91b-448">その文字列を使用すると、コールバックを開始するコントロールのクライアント側のイベントをフックします。</span><span class="sxs-lookup"><span data-stu-id="da91b-448">That string is used to hook a client-side event for the control that initiates the callback.</span></span> <span data-ttu-id="da91b-449">この例では、コールバックによって開始されるページで、ドロップダウン リストをフックするため、 *OnChange*イベント。</span><span class="sxs-lookup"><span data-stu-id="da91b-449">In this example, the callback is initiated by a dropdown on the page, so I want to hook the *OnChange* event.</span></span>

<span data-ttu-id="da91b-450">次のようにクライアント側のマークアップにハンドラーを追加するだけをクライアント側のイベントにフックします。</span><span class="sxs-lookup"><span data-stu-id="da91b-450">To hook the client-side event, simply add a handler to the client-side markup as follows:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample15.cs)]

<span data-ttu-id="da91b-451">注意してください*cbRef* GetCallbackEventReference への呼び出しからの戻り値です。</span><span class="sxs-lookup"><span data-stu-id="da91b-451">Recall that *cbRef* is the return value from the call to GetCallbackEventReference.</span></span> <span data-ttu-id="da91b-452">Web フォームへの呼び出しが含まれている\_DoCallback 上に表示されました。</span><span class="sxs-lookup"><span data-stu-id="da91b-452">It contains the call to WebForm\_DoCallback that was shown above.</span></span>

## <a name="step-4--register-the-client-side-script"></a><span data-ttu-id="da91b-453">手順 4: クライアント側スクリプトを登録します。</span><span class="sxs-lookup"><span data-stu-id="da91b-453">Step 4 : Register the Client-Side Script</span></span>

<span data-ttu-id="da91b-454">GetCallbackEventReference への呼び出しは、クライアント側スクリプトと呼ばれることを指定すること**ShowCompanyName**サーバー側のコールバックが成功したときに実行されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-454">Recall that the call to GetCallbackEventReference specified that a client-side script called **ShowCompanyName** would be executed when the server-side callback succeeds.</span></span> <span data-ttu-id="da91b-455">そのスクリプトは、ClientScriptManager インスタンスを使用して、ページに追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-455">That script needs to be added to the page using a ClientScriptManager instance.</span></span> <span data-ttu-id="da91b-456">(ClientScriptManager クラスになりますこのモジュールの後で示す)。そのようなのでを実行します。</span><span class="sxs-lookup"><span data-stu-id="da91b-456">(The ClientScriptManager class will be dicussed later in this module.) You do that like so:</span></span>

[!code-javascript[Main](the-asp-net-2-0-page-model/samples/sample16.js)]

## <a name="step-5--call-the-methods-of-the-icallbackeventhandler-interface"></a><span data-ttu-id="da91b-457">手順 5: ICallbackEventHandler インターフェイスのメソッドを呼び出す</span><span class="sxs-lookup"><span data-stu-id="da91b-457">Step 5 : Call the Methods of the ICallbackEventHandler Interface</span></span>

<span data-ttu-id="da91b-458">ICallbackEventHandler には、コードで実装する必要のある 2 つのメソッドが含まれています。</span><span class="sxs-lookup"><span data-stu-id="da91b-458">The ICallbackEventHandler contains two methods that you need to implement in your code.</span></span> <span data-ttu-id="da91b-459">**RaiseCallbackEvent**と**GetCallbackEvent**です。</span><span class="sxs-lookup"><span data-stu-id="da91b-459">They are **RaiseCallbackEvent** and **GetCallbackEvent**.</span></span>

<span data-ttu-id="da91b-460">**RaiseCallbackEvent**を引数として文字列を受け取り、nothing を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-460">**RaiseCallbackEvent** takes a string as an argument and returns nothing.</span></span> <span data-ttu-id="da91b-461">Web フォームのクライアント側呼び出しから文字列引数が渡された\_DoCallback です。</span><span class="sxs-lookup"><span data-stu-id="da91b-461">The string argument is passed from the client-side call to WebForm\_DoCallback.</span></span> <span data-ttu-id="da91b-462">この場合、その値は、*値*ddlCompany と呼ばれる、ドロップダウンの属性です。</span><span class="sxs-lookup"><span data-stu-id="da91b-462">In this case, that value is the *value* attribute of the dropdown called ddlCompany.</span></span> <span data-ttu-id="da91b-463">RaiseCallbackEvent メソッドには、サーバー側コードを配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-463">Your server-side code should be placed in the RaiseCallbackEvent method.</span></span> <span data-ttu-id="da91b-464">など、コールバックが外部のリソースに対して WebRequest を行っている場合は、そのコードを RaiseCallbackEvent に配置する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-464">For example, if your callback is making a WebRequest against an external resource, that code should be placed in RaiseCallbackEvent.</span></span>

<span data-ttu-id="da91b-465">**GetCallbackEvent**をクライアントにコールバックの戻り値を処理します。</span><span class="sxs-lookup"><span data-stu-id="da91b-465">**GetCallbackEvent** is responsible for processing the return of the callback to the client.</span></span> <span data-ttu-id="da91b-466">引数を使用しないと、文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-466">It takes no arguments and returns a string.</span></span> <span data-ttu-id="da91b-467">返される文字列は、引数として関数に渡す、クライアント側、ここでは*ShowCompanyName*です。</span><span class="sxs-lookup"><span data-stu-id="da91b-467">The string that it returns will be passed as an argument to the client-side function, in this case *ShowCompanyName*.</span></span>

<span data-ttu-id="da91b-468">上記の手順を完了すると、ASP.NET 2.0 では、スクリプトのコールバックを実行する準備ができたらです。</span><span class="sxs-lookup"><span data-stu-id="da91b-468">Once you have completed the above steps, you are ready to perform a script callback in ASP.NET 2.0.</span></span>


![](the-asp-net-2-0-page-model/_static/image4.png)


[<span data-ttu-id="da91b-469">開いているビデオを全画面</span><span class="sxs-lookup"><span data-stu-id="da91b-469">Open Full-Screen Video</span></span>](the-asp-net-2-0-page-model/_static/callback1.wmv)


<span data-ttu-id="da91b-470">ASP.NET でのスクリプトのコールバックは、XMLHttp 呼び出すをサポートするブラウザーでサポートされます。</span><span class="sxs-lookup"><span data-stu-id="da91b-470">Script callbacks in ASP.NET are supported in any browser that supports making XMLHttp calls.</span></span> <span data-ttu-id="da91b-471">すべての最新ブラウザーを含むを使用して今日。</span><span class="sxs-lookup"><span data-stu-id="da91b-471">That includes all of the modern browsers in use today.</span></span> <span data-ttu-id="da91b-472">Internet Explorer は、(予定されている IE 7 を含む) の他の最新のブラウザーが組み込み XMLHttp オブジェクトを使用して、XMLHttp ActiveX オブジェクトを使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-472">Internet Explorer uses the XMLHttp ActiveX object while other modern browsers (including the upcoming IE 7) use an intrinsic XMLHttp object.</span></span> <span data-ttu-id="da91b-473">プログラムで使用することができます、ブラウザーがコールバックをサポートする場合を決定する、 **Request.Browser.SupportCallback**プロパティです。</span><span class="sxs-lookup"><span data-stu-id="da91b-473">To programmatically determine if a browser supports callbacks, you can use the **Request.Browser.SupportCallback** property.</span></span> <span data-ttu-id="da91b-474">このプロパティは**true**要求元のクライアントは、スクリプトのコールバックをサポートしている場合。</span><span class="sxs-lookup"><span data-stu-id="da91b-474">This property will return **true** if the requesting client supports script callbacks.</span></span>

## <a name="working-with-client-script-in-aspnet-20"></a><span data-ttu-id="da91b-475">ASP.NET 2.0 内のクライアント スクリプトの操作</span><span class="sxs-lookup"><span data-stu-id="da91b-475">Working with Client Script in ASP.NET 2.0</span></span>

<span data-ttu-id="da91b-476">クライアント スクリプトを ASP.NET 2.0 では、ClientScriptManager クラスを使用して管理されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-476">Client scripts in ASP.NET 2.0 are managed via the use of the ClientScriptManager class.</span></span> <span data-ttu-id="da91b-477">ClientScriptManager クラスの追跡クライアント スクリプトを型および名前を使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-477">The ClientScriptManager class keeps track of client scripts using a type and a name.</span></span> <span data-ttu-id="da91b-478">これは、同じスクリプトがプログラムによって挿入されるページに複数回することを防ぎます。</span><span class="sxs-lookup"><span data-stu-id="da91b-478">This prevents the same script from being programmatically inserted on a page more than once.</span></span>

> [!NOTE]
> <span data-ttu-id="da91b-479">スクリプトがページに正常に登録された後後続しようとすると、同じスクリプトを登録するだけになります、もう一度登録されていないスクリプト。</span><span class="sxs-lookup"><span data-stu-id="da91b-479">After a script has been successfully registered on a page, any subsequent attempt to register the same script will simply result in the script not being registered a second time.</span></span> <span data-ttu-id="da91b-480">スクリプトが重複しては追加されず、例外が発生しません。</span><span class="sxs-lookup"><span data-stu-id="da91b-480">No duplicate scripts are added and no exception occurs.</span></span> <span data-ttu-id="da91b-481">不要な計算を回避するのを決定できるように、複数回登録しないで、スクリプトは既に登録されてかどうかに使用できる方法があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-481">To avoid unnecessary computation, there are methods that you can use to determine if a script is already registered so that you do not attempt to register it more than once.</span></span>


<span data-ttu-id="da91b-482">ClientScriptManager のメソッドは、現在のすべての ASP.NET 開発者にについて理解する必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-482">The methods of the ClientScriptManager should be familiar to all current ASP.NET developers:</span></span>

## <a name="registerclientscriptblock"></a><span data-ttu-id="da91b-483">RegisterClientScriptBlock</span><span class="sxs-lookup"><span data-stu-id="da91b-483">RegisterClientScriptBlock</span></span>

<span data-ttu-id="da91b-484">このメソッドは、スクリプトを表示するページの上部に追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-484">This method adds a script to the top of the rendered page.</span></span> <span data-ttu-id="da91b-485">これは、クライアントで明示的に呼び出される関数を追加するために役立ちます。</span><span class="sxs-lookup"><span data-stu-id="da91b-485">This is useful for adding functions that will be explicitly called on the client.</span></span>

<span data-ttu-id="da91b-486">このメソッドの 2 つのオーバー ロードされたバージョンがあります。</span><span class="sxs-lookup"><span data-stu-id="da91b-486">There are two overloaded versions of this method.</span></span> <span data-ttu-id="da91b-487">3 つの 4 つの引数は、それらの間で共通です。</span><span class="sxs-lookup"><span data-stu-id="da91b-487">Three of four arguments are common among them.</span></span> <span data-ttu-id="da91b-488">それらは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="da91b-488">They are:</span></span>

`type (string)`

<span data-ttu-id="da91b-489">***型***引数は、スクリプトの種類を識別します。</span><span class="sxs-lookup"><span data-stu-id="da91b-489">The ***type*** argument identifies a type for the script.</span></span> <span data-ttu-id="da91b-490">一般に、(このページの種類を使用することをお勧め。型の GetType()) します。</span><span class="sxs-lookup"><span data-stu-id="da91b-490">It is generally a good idea to use the page's type (this.GetType()) for the type.</span></span>

`key (string)`

<span data-ttu-id="da91b-491">***キー***引数は、スクリプトのユーザー定義のキー。</span><span class="sxs-lookup"><span data-stu-id="da91b-491">The ***key*** argument is a user-defined key for the script.</span></span> <span data-ttu-id="da91b-492">これは、スクリプトごとに一意にする必要があります。</span><span class="sxs-lookup"><span data-stu-id="da91b-492">This should be unique for each script.</span></span> <span data-ttu-id="da91b-493">同じキーと、既に追加されたスクリプトの種類のスクリプトを追加しようとする場合に追加されません。</span><span class="sxs-lookup"><span data-stu-id="da91b-493">If you attempt to add a script with the same key and type of an already added script, it will not be added.</span></span>

`script (string)`

<span data-ttu-id="da91b-494">***スクリプト***引数は、追加する実際のスクリプトを含む文字列。</span><span class="sxs-lookup"><span data-stu-id="da91b-494">The ***script*** argument is a string containing the actual script to add.</span></span> <span data-ttu-id="da91b-495">スクリプトを作成し、StringBuilder の ToString() メソッドを使用して割り当てるには、StringBuilder を使用することをお勧めしますが、***スクリプト***引数。</span><span class="sxs-lookup"><span data-stu-id="da91b-495">It's recommended that you use a StringBuilder to create the script and then use the ToString() method on the StringBuilder to assign the ***script*** argument.</span></span>

<span data-ttu-id="da91b-496">のみ次の 3 つの引数を受け取るオーバー ロードされた RegisterClientScriptBlock を使用する場合は、スクリプト要素を含める必要があります (&lt;スクリプト&gt;と&lt;/script&gt;)、スクリプトでします。</span><span class="sxs-lookup"><span data-stu-id="da91b-496">If you use the overloaded RegisterClientScriptBlock that only takes three arguments, you must include script elements (&lt;script&gt; and &lt;/script&gt;) in your script.</span></span>

<span data-ttu-id="da91b-497">4 番目の引数を受け取る RegisterClientScriptBlock のオーバー ロードを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-497">You may choose to use the overload of RegisterClientScriptBlock that takes a fourth argument.</span></span> <span data-ttu-id="da91b-498">4 番目の引数は、ASP.NET でのスクリプト要素を追加する必要があるかどうかを指定するブール値です。</span><span class="sxs-lookup"><span data-stu-id="da91b-498">The fourth argument is a Boolean that specifies whether or not ASP.NET should add script elements for you.</span></span> <span data-ttu-id="da91b-499">この引数は場合**true**スクリプトはスクリプト要素は明示的に含まれません。</span><span class="sxs-lookup"><span data-stu-id="da91b-499">If this argument is **true**, your script should not include the script elements explicitly.</span></span>

<span data-ttu-id="da91b-500">IsClientScriptBlockRegistered メソッドを使用して、スクリプトは既に登録されているかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="da91b-500">Use the IsClientScriptBlockRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="da91b-501">これによりは既に登録されているスクリプトを再登録の試行を回避できます。</span><span class="sxs-lookup"><span data-stu-id="da91b-501">This allows you to avoid an attempt to re-register a script that has already been registered.</span></span>

### <a name="registerclientscriptinclude-new-in-20"></a><span data-ttu-id="da91b-502">(2.0) で新しい RegisterClientScriptInclude</span><span class="sxs-lookup"><span data-stu-id="da91b-502">RegisterClientScriptInclude (New in 2.0)</span></span>

<span data-ttu-id="da91b-503">RegisterClientScriptInclude タグは、外部スクリプト ファイルにリンクしているスクリプト ブロックを作成します。</span><span class="sxs-lookup"><span data-stu-id="da91b-503">The RegisterClientScriptInclude tag creates a script block that links to an external script file.</span></span> <span data-ttu-id="da91b-504">2 つのオーバー ロードがあります。</span><span class="sxs-lookup"><span data-stu-id="da91b-504">It has two overloads.</span></span> <span data-ttu-id="da91b-505">1 つは、キーと URL を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="da91b-505">One takes a key and a URL.</span></span> <span data-ttu-id="da91b-506">2 つ目は、型を指定する 3 番目の引数を追加します。</span><span class="sxs-lookup"><span data-stu-id="da91b-506">The second adds a third argument specifying the type.</span></span>

<span data-ttu-id="da91b-507">たとえば、次のコードが生成されます、スクリプト ブロック jsfunctions.js にリンクしているアプリケーションのスクリプト フォルダーのルートに。</span><span class="sxs-lookup"><span data-stu-id="da91b-507">For example, the following code generates a script block that links to jsfunctions.js in the root of the scripts folder of the application:</span></span>

[!code-csharp[Main](the-asp-net-2-0-page-model/samples/sample17.cs)]

<span data-ttu-id="da91b-508">このコードは、レンダリングされるページで次のコードを生成します。</span><span class="sxs-lookup"><span data-stu-id="da91b-508">This code produces the following code in the rendered page:</span></span>

[!code-html[Main](the-asp-net-2-0-page-model/samples/sample18.html)]

> [!NOTE]
> <span data-ttu-id="da91b-509">スクリプト ブロックは、ページの下部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-509">The script block is rendered at the bottom of the page.</span></span>


<span data-ttu-id="da91b-510">IsClientScriptIncludeRegistered メソッドを使用して、スクリプトは既に登録されているかどうかを判断します。</span><span class="sxs-lookup"><span data-stu-id="da91b-510">Use the IsClientScriptIncludeRegistered method to determine if a script has already been registered.</span></span> <span data-ttu-id="da91b-511">これにより、スクリプトを再登録の試行を回避できます。</span><span class="sxs-lookup"><span data-stu-id="da91b-511">This allows you to avoid an attempt to re-register a script.</span></span>

## <a name="registerstartupscript"></a><span data-ttu-id="da91b-512">RegisterStartupScript</span><span class="sxs-lookup"><span data-stu-id="da91b-512">RegisterStartupScript</span></span>

<span data-ttu-id="da91b-513">RegisterStartupScript メソッドは、RegisterClientScriptBlock メソッドと同じ引数を受け取ります。</span><span class="sxs-lookup"><span data-stu-id="da91b-513">The RegisterStartupScript method takes the same arguments as the RegisterClientScriptBlock method.</span></span> <span data-ttu-id="da91b-514">RegisterStartupScript に登録されているスクリプトは、ページが読み込まれた後、OnLoad クライアント側イベントの前に実行します。</span><span class="sxs-lookup"><span data-stu-id="da91b-514">A script registered with RegisterStartupScript executes after the page loads but before the OnLoad client-side event.</span></span> <span data-ttu-id="da91b-515">終了の直前に配置されていた RegisterStartupScript に登録されたスクリプト 1.X で&lt;/form&gt; RegisterClientScriptBlock に登録されたスクリプトが開始した直後に配置されていたときにタグを付ける&lt;フォーム&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="da91b-515">In 1.X, scripts registered with RegisterStartupScript were placed just before the closing &lt;/form&gt; tag while scripts registered with RegisterClientScriptBlock were placed immediately after the opening &lt;form&gt; tag.</span></span> <span data-ttu-id="da91b-516">ASP.NET 2.0 の両方が終了する直前に配置されます。 &lt;/form&gt;タグ。</span><span class="sxs-lookup"><span data-stu-id="da91b-516">In ASP.NET 2.0, both are placed immediately before the closing &lt;/form&gt; tag.</span></span>

> [!NOTE]
> <span data-ttu-id="da91b-517">RegisterStartupScript で関数を登録した場合、クライアント側のコードで明示的に呼び出されるまでその関数が実行されません。</span><span class="sxs-lookup"><span data-stu-id="da91b-517">If you register a function with RegisterStartupScript, that function will not execute until you explicitly call it in client-side code.</span></span>


<span data-ttu-id="da91b-518">スクリプトは既に登録されているかどうかを確認し、スクリプトを再登録の試行を回避するには、IsStartupScriptRegistered メソッドを使用します。</span><span class="sxs-lookup"><span data-stu-id="da91b-518">Use the IsStartupScriptRegistered method to determine if a script has already been registered and avoid an attempt to re-register a script.</span></span>

## <a name="other-clientscriptmanager-methods"></a><span data-ttu-id="da91b-519">その他の ClientScriptManager メソッド</span><span class="sxs-lookup"><span data-stu-id="da91b-519">Other ClientScriptManager Methods</span></span>

<span data-ttu-id="da91b-520">ClientScriptManager クラスの他の便利なメソッドのいくつか示します。</span><span class="sxs-lookup"><span data-stu-id="da91b-520">Here are some of the other useful methods of the ClientScriptManager class.</span></span>

| <span data-ttu-id="da91b-521">**GetCallbackEventReference**</span><span class="sxs-lookup"><span data-stu-id="da91b-521">**GetCallbackEventReference**</span></span> | <span data-ttu-id="da91b-522">このモジュールで前述のスクリプトのコールバックを参照してください。</span><span class="sxs-lookup"><span data-stu-id="da91b-522">See script callbacks earlier in this module.</span></span> |
| --- | --- |
| <span data-ttu-id="da91b-523">**GetPostBackClientHyperlink**</span><span class="sxs-lookup"><span data-stu-id="da91b-523">**GetPostBackClientHyperlink**</span></span> | <span data-ttu-id="da91b-524">JavaScript の参照を取得 (javascript:&lt;呼び出す&gt;) を使用してクライアント側のイベントからの投稿をことができます。</span><span class="sxs-lookup"><span data-stu-id="da91b-524">Gets a JavaScript reference (javascript:&lt;call&gt;) that can be used to post back from a client-side event.</span></span> |
| <span data-ttu-id="da91b-525">**GetPostBackEventReference**</span><span class="sxs-lookup"><span data-stu-id="da91b-525">**GetPostBackEventReference**</span></span> | <span data-ttu-id="da91b-526">クライアントからの投稿を開始するために使用する文字列を取得します。</span><span class="sxs-lookup"><span data-stu-id="da91b-526">Gets a string that can be used to initiate a post back from the client.</span></span> |
| <span data-ttu-id="da91b-527">**GetWebResourceUrl**</span><span class="sxs-lookup"><span data-stu-id="da91b-527">**GetWebResourceUrl**</span></span> | <span data-ttu-id="da91b-528">アセンブリに埋め込まれているリソースへの URL を返します。</span><span class="sxs-lookup"><span data-stu-id="da91b-528">Returns a URL to a resource that is embedded in an assembly.</span></span> <span data-ttu-id="da91b-529">組み合わせて使用する必要があります**RegisterClientScriptResource**です。</span><span class="sxs-lookup"><span data-stu-id="da91b-529">Must be used in conjunction with **RegisterClientScriptResource**.</span></span> |
| <span data-ttu-id="da91b-530">**RegisterClientScriptResource**</span><span class="sxs-lookup"><span data-stu-id="da91b-530">**RegisterClientScriptResource**</span></span> | <span data-ttu-id="da91b-531">ページに Web リソースを登録します。</span><span class="sxs-lookup"><span data-stu-id="da91b-531">Registers a Web resource with the page.</span></span> <span data-ttu-id="da91b-532">これらは、リソース アセンブリに埋め込まれているし、新しい WebResource.axd ハンドラーによって処理されます。</span><span class="sxs-lookup"><span data-stu-id="da91b-532">These are resources embedded in an assembly and handled by the new WebResource.axd handler.</span></span> |
| <span data-ttu-id="da91b-533">**RegisterHiddenField**</span><span class="sxs-lookup"><span data-stu-id="da91b-533">**RegisterHiddenField**</span></span> | <span data-ttu-id="da91b-534">ページを非表示のフォーム フィールドを登録します。</span><span class="sxs-lookup"><span data-stu-id="da91b-534">Registers a hidden form field with the page.</span></span> |
| <span data-ttu-id="da91b-535">**RegisterOnSubmitStatement**</span><span class="sxs-lookup"><span data-stu-id="da91b-535">**RegisterOnSubmitStatement**</span></span> | <span data-ttu-id="da91b-536">HTML フォームが送信されるときに実行されるクライアント側のコードを登録します。</span><span class="sxs-lookup"><span data-stu-id="da91b-536">Registers client-side code that executes when the HTML form is submitted.</span></span> |
