---
uid: web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
title: ASP.NET Web Pages 2 開発者プレビューのリリース ノート |Microsoft ドキュメント
author: microsoft
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/14/2011
ms.topic: article
ms.assetid: 159a92e2-e011-4da7-b61d-2edde2a967da
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/releases/aspnet-web-pages-2-developer-preview-readme
msc.type: authoredcontent
ms.openlocfilehash: 1a43b2b12af9cd223d8a3622239743f7c431f617
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-pages-2-developer-preview-readme"></a><span data-ttu-id="74ba2-102">ASP.NET Web Pages 2 開発者プレビューのリリース ノート</span><span class="sxs-lookup"><span data-stu-id="74ba2-102">ASP.NET Web Pages 2 Developer Preview ReadMe</span></span>
====================
<span data-ttu-id="74ba2-103">によって[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="74ba2-103">by [Microsoft](https://github.com/microsoft)</span></span>

## <a name="aspnet-web-pages-2-developer-preview-readme"></a><span data-ttu-id="74ba2-104">ASP.NET Web Pages 2 開発者プレビューのリリース ノート</span><span class="sxs-lookup"><span data-stu-id="74ba2-104">ASP.NET Web Pages 2 Developer Preview ReadMe</span></span>

<span data-ttu-id="74ba2-105">2011 年 9 月 14日</span><span class="sxs-lookup"><span data-stu-id="74ba2-105">14 September 2011</span></span>

### <a name="contents"></a><span data-ttu-id="74ba2-106">目次</span><span class="sxs-lookup"><span data-stu-id="74ba2-106">Contents</span></span>

#### <a id="_Toc303701284"></a>  <span data-ttu-id="74ba2-107">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="74ba2-107">Installation Notes</span></span>

<span data-ttu-id="74ba2-108">Web ページ 2 Developer Preview をインストールするには、これらのオプションがあります。</span><span class="sxs-lookup"><span data-stu-id="74ba2-108">To install Web Pages 2 Developer Preview, you have these options:</span></span>

- <span data-ttu-id="74ba2-109">WebMatrix 2 のベータ版を使用してインストール、 [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883)です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-109">Install WebMatrix 2 Beta using the [Web Platform Installer](https://go.microsoft.com/fwlink/?LinkId=226883).</span></span> <span data-ttu-id="74ba2-110">WebMatrix は、ASP.NET Web Pages を含んだ無料の web 開発ツールのセットです。</span><span class="sxs-lookup"><span data-stu-id="74ba2-110">WebMatrix is a set of free web development tools that includes ASP.NET Web Pages.</span></span> <span data-ttu-id="74ba2-111">詳細については、インストールに関するセクションを参照してください。[上部機能によって、ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824)です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-111">For more information, see the installation section in [The Top Features in ASP.NET Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).</span></span>

- <span data-ttu-id="74ba2-112">使用して直接 Web Pages 2 Developer Preview をインストール、[ダウンロード リンク](https://go.microsoft.com/fwlink/?LinkID=226335)です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-112">Install Web Pages 2 Developer Preview directly using the [download link](https://go.microsoft.com/fwlink/?LinkID=226335).</span></span> <span data-ttu-id="74ba2-113">メモ帳などのエディター、テキストを使用して Web Pages アプリケーションを作成する場合は、このアプローチを使用します。</span><span class="sxs-lookup"><span data-stu-id="74ba2-113">Use this approach if you want to create Web Pages applications using a text editor such as Notepad.</span></span> <span data-ttu-id="74ba2-114">Web Pages 2 アプリケーションを実行するのには、IIS Express 7.5 が必要です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-114">In order to run Web Pages 2 applications, you must have IIS Express 7.5.</span></span> <span data-ttu-id="74ba2-115">(これは WebMatrix で自動的に含まれる) です。IIS Express を使用する Web ページのページをテストする方法に関するヒントを参照して、サイドバー「を作成し、テスト ASP.NET ページを使用して、独自テキスト エディター」 [WebMatrix と ASP.NET Web Pages の概要](https://go.microsoft.com/fwlink/?LinkId=202889)です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-115">(This is included automatically with WebMatrix.) For tips on how to test a Web Pages page using IIS Express, see the sidebar "Creating and Testing ASP.NET Pages Using Your Own Text Editor" in [Getting Started with WebMatrix and ASP.NET Web Pages](https://go.microsoft.com/fwlink/?LinkId=202889).</span></span>

<span data-ttu-id="74ba2-116">ASP.NET Web Pages 2 Developer Preview をインストールしてサイド バイ サイドで実行できる ASP.NET Web ページの 1。</span><span class="sxs-lookup"><span data-stu-id="74ba2-116">ASP.NET Web Pages 2 Developer Preview can be installed and can run side-by-side with ASP.NET Web Pages 1.</span></span> <a id="a"></a><span data-ttu-id="74ba2-117">詳細についてを参照してください「実行されている Web ページ アプリケーション サイド バイ サイド」で[上部機能によって、Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824)です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-117">For details, see the section "Running Web Pages Applications Side-by-Side" in [The Top Features in Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).</span></span>

#### <a id="_Toc303701285"></a>  <span data-ttu-id="74ba2-118">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="74ba2-118">Documentation</span></span>

<span data-ttu-id="74ba2-119">チュートリアルおよびその他の情報については、ASP.NET Web Pages は、ASP.NET web サイトの Web ページのページで使用可能な ([https://www.asp.net/web-pages/](../../index.md))。</span><span class="sxs-lookup"><span data-stu-id="74ba2-119">Tutorials and other information about ASP.NET Web Pages are available on the Web Pages page of the ASP.NET website ([https://www.asp.net/web-pages/](../../index.md)).</span></span> <span data-ttu-id="74ba2-120">新機能および Web Pages 2 での機能強化については、次を参照してください。[上部機能によって、Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824)です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-120">For information about new features and enhancements in Web Pages 2, see [The Top Features in Web Pages 2 Developer Preview](https://go.microsoft.com/fwlink/?LinkID=227824).</span></span>

#### <a id="_Toc303701286"></a>  <span data-ttu-id="74ba2-121">サポート</span><span class="sxs-lookup"><span data-stu-id="74ba2-121">Support</span></span>

<a id="_Toc209852135"></a><span data-ttu-id="74ba2-122"><a id="_Toc255833657"></a> これは、プレビュー リリースであり、公式にサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="74ba2-122"><a id="_Toc255833657"></a> This is a preview release and is not officially supported.</span></span> <span data-ttu-id="74ba2-123">このリリースの操作についての質問がある場合、ASP.NET Web Pages のフォーラムに投稿 ([ https://forums.asp.net/1224.aspx/1?WebMatrix ](https://forums.asp.net/1224.aspx/1?WebMatrix) ) では、ASP.NET コミュニティのメンバーは頻繁に、非公式のサポートを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="74ba2-123">If you have questions about working with this release, post them to the ASP.NET Web Pages forum ([https://forums.asp.net/1224.aspx/1?WebMatrix](https://forums.asp.net/1224.aspx/1?WebMatrix) ), where members of the ASP.NET community are frequently able to provide informal support.</span></span>

#### <a id="_Toc303701287"></a>  <span data-ttu-id="74ba2-124">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="74ba2-124">Software Requirements</span></span>

<span data-ttu-id="74ba2-125">ASP.NET Web Pages 2 には、.NET Framework 4 が必要です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-125">ASP.NET Web Pages 2 requires the .NET Framework 4.</span></span> <span data-ttu-id="74ba2-126">.NET Framework 4.5 Developer Preview のリリースでも動作します。</span><span class="sxs-lookup"><span data-stu-id="74ba2-126">It also works with the .NET Framework 4.5 Developer Preview release.</span></span>

<a id="_Toc303701288"></a><a id="_Breaking_Changes"></a>

#### <a name="fixes-known-issues-and-breaking-changes"></a><span data-ttu-id="74ba2-127">修正プログラム、既知の問題、および重大な変更</span><span class="sxs-lookup"><span data-stu-id="74ba2-127">Fixes, Known Issues, and Breaking Changes</span></span>

<a id="_Toc224729061"></a><a id="_Toc238051347"></a>

- <span data-ttu-id="74ba2-128">**\*メソッド (たとえば、IsDateTime) ここで正しい値を返す、すべてのカルチャ。**</span><span class="sxs-lookup"><span data-stu-id="74ba2-128">**Is\* methods (for example, IsDateTime) now return correct values for all cultures.**</span></span> <span data-ttu-id="74ba2-129">などのメソッド*IsDateTime*以前に返された*false*ときに戻っている必要があります*true*に以前のカルチャ固有のチェックを実行していたためです。</span><span class="sxs-lookup"><span data-stu-id="74ba2-129">Some methods like *IsDateTime* previously returned *false* when they should have returned *true* because they were previously performing culture-specific checks.</span></span> <span data-ttu-id="74ba2-130">これらのメソッドは、現在のカルチャを考慮に入れる修正されています。</span><span class="sxs-lookup"><span data-stu-id="74ba2-130">These methods have been fixed to now take culture into account.</span></span> <span data-ttu-id="74ba2-131">これは、互換性に影響する変更です。アプリケーションは、従来の動作に依存、分割されます。</span><span class="sxs-lookup"><span data-stu-id="74ba2-131">This is a breaking change; if your application relies on the old behavior, it will break.</span></span>
- <span data-ttu-id="74ba2-132">**Href メソッドの動作が変更されました。**</span><span class="sxs-lookup"><span data-stu-id="74ba2-132">**The behavior of the Href method has changed.**</span></span> <span data-ttu-id="74ba2-133">以前は、Href("~/SomeFile") を呼び出すには、現在実行中のファイルに相対 URL は返します。</span><span class="sxs-lookup"><span data-stu-id="74ba2-133">Previously, calling Href("~/SomeFile") would return a URL relative to the currently executing file.</span></span> <span data-ttu-id="74ba2-134">今すぐ Href("~/SomeFile") は、アプリケーションのルートからの絶対パスを常に返します。</span><span class="sxs-lookup"><span data-stu-id="74ba2-134">Now Href("~/SomeFile") always returns an absolute path from the root of the application.</span></span> <span data-ttu-id="74ba2-135">ほとんどの場合、この動作は、戻り値の違いを作成しません。</span><span class="sxs-lookup"><span data-stu-id="74ba2-135">For most cases, this behavior won't make a difference in the return value.</span></span> <span data-ttu-id="74ba2-136">この変更は、特定の Ajax シナリオを修正しました。</span><span class="sxs-lookup"><span data-stu-id="74ba2-136">This change was made to fix certain Ajax scenarios.</span></span> <span data-ttu-id="74ba2-137">たとえば、次のコード例を考えてみます。</span><span class="sxs-lookup"><span data-stu-id="74ba2-137">For example, consider the following example code:</span></span> 

    [!code-cshtml[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample1.cshtml)]

    <span data-ttu-id="74ba2-138">このコード以前に解決される Images/Logo.jpg、そのページに Ajax 要求に対して正しいとなります。</span><span class="sxs-lookup"><span data-stu-id="74ba2-138">This code previously would resolve to Images/Logo.jpg, which would be incorrect for an Ajax request to that page.</span></span> <span data-ttu-id="74ba2-139">ルートを解決するようになりましたが、(/MySite/Images/Logo.jpg)。</span><span class="sxs-lookup"><span data-stu-id="74ba2-139">It will now resolve to the root of the (/MySite/Images/Logo.jpg).</span></span>
- <span data-ttu-id="74ba2-140">**HttpContext.RedirectLocal メソッドが変更された**です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-140">**The HttpContext.RedirectLocal method has changed**.</span></span> <span data-ttu-id="74ba2-141">このメソッドは、現在のアプリケーションに対する相対 Url のみを受け入れるようになりました。</span><span class="sxs-lookup"><span data-stu-id="74ba2-141">This method now accepts only URLs that are relative to the current application.</span></span> <span data-ttu-id="74ba2-142">完全修飾 Url は拒否されます。</span><span class="sxs-lookup"><span data-stu-id="74ba2-142">Fully qualified URLs are rejected.</span></span>
- <span data-ttu-id="74ba2-143">**ModelState.IsValid メソッド今すぐする必要がありますまず Validate を呼び出して**です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-143">**The ModelState.IsValid method now requires you to call Validate first**.</span></span> <span data-ttu-id="74ba2-144">新しい入力の検証メソッドを使用するアプリケーションを変換してを呼び出している場合、 *ModelState.IsValid*メソッド、ここで呼び出す必要があります*Validation.Validate*事前です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-144">If you are converting your application to use the new input validation methods and are calling the *ModelState.IsValid* method, you must now call *Validation.Validate* beforehand.</span></span> <span data-ttu-id="74ba2-145">たとえば、このパターンを今すぐに従う必要があります。</span><span class="sxs-lookup"><span data-stu-id="74ba2-145">For example, you must now follow this pattern:</span></span> 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample2.cs)]

  <span data-ttu-id="74ba2-146">ただし、ことをお勧めする場合、新しい入力の検証メソッドを使用するを使用しないで*ModelState.IsValid*です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-146">However, we recommend that if you use the new input validation methods, don't use *ModelState.IsValid*.</span></span> <span data-ttu-id="74ba2-147">代わりに、次のようにコードを構造化します。</span><span class="sxs-lookup"><span data-stu-id="74ba2-147">Instead, structure your code like this:</span></span> 

    [!code-csharp[Main](aspnet-web-pages-2-developer-preview-readme/samples/sample3.cs)]
- <span data-ttu-id="74ba2-148">**Internet Explorer 7 および Internet Explorer 8 では、クライアント側検証は機能しません**です。</span><span class="sxs-lookup"><span data-stu-id="74ba2-148">**On Internet Explorer 7 and Internet Explorer 8, client-side validation does not work**.</span></span> <span data-ttu-id="74ba2-149">クライアント側の検証は機能しません jQuery 1.6.2 でした、との非互換性のため、既定のプロジェクト テンプレートに含まれています。</span><span class="sxs-lookup"><span data-stu-id="74ba2-149">Client-side validation does not work due to incompatibilities with jQuery 1.6.2, which is included with the default project template.</span></span> <span data-ttu-id="74ba2-150">(サーバー側の検証は動作します。)。</span><span class="sxs-lookup"><span data-stu-id="74ba2-150">(Server-side validation works.).</span></span>

#### <a id="_Toc303701289"></a>  <span data-ttu-id="74ba2-151">免責事項</span><span class="sxs-lookup"><span data-stu-id="74ba2-151">Disclaimer</span></span>

<span data-ttu-id="74ba2-152">© 2011 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="74ba2-152">© 2011 Microsoft Corporation.</span></span> <span data-ttu-id="74ba2-153">All rights reserved.</span><span class="sxs-lookup"><span data-stu-id="74ba2-153">All rights reserved.</span></span> <span data-ttu-id="74ba2-154">このドキュメントは提供される"としてでは、します"。</span><span class="sxs-lookup"><span data-stu-id="74ba2-154">This document is provided "as-is."</span></span> <span data-ttu-id="74ba2-155">情報および見解 URL および他のインターネット Web サイトの参照を含む、このドキュメントでは、通知なく変更可能性があります。</span><span class="sxs-lookup"><span data-stu-id="74ba2-155">Information and views expressed in this document, including URL and other Internet Web site references, may change without notice.</span></span> <span data-ttu-id="74ba2-156">お客様は、その使用に関するリスクを負うものとします。</span><span class="sxs-lookup"><span data-stu-id="74ba2-156">You bear the risk of using it.</span></span>
