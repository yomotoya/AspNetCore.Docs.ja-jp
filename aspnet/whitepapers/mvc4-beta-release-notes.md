---
uid: whitepapers/mvc4-beta-release-notes
title: "ASP.NET MVC 4 |Microsoft ドキュメント"
author: rick-anderson
description: "このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/09/2011
ms.topic: article
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: 4af2df61ab4507b1f100d6bb75777da1168c5a75
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-mvc-4"></a><span data-ttu-id="e8112-103">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="e8112-103">ASP.NET MVC 4</span></span>
====================
> <span data-ttu-id="e8112-104">このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="e8112-104">This document describes the release of ASP.NET MVC 4 Beta for Visual Studio 2010.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="e8112-105">これは、最新のリリースではありません。</span><span class="sxs-lookup"><span data-stu-id="e8112-105">This is not the most current release.</span></span> <span data-ttu-id="e8112-106">ASP.NET MVC 4 RC のリリース ノートは[ここ](mvc4-release-notes.md)です。</span><span class="sxs-lookup"><span data-stu-id="e8112-106">The ASP.NET MVC 4 RC release notes are available [here](mvc4-release-notes.md).</span></span>


- [<span data-ttu-id="e8112-107">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="e8112-107">Installation Notes</span></span>](#_Toc303253802)
- [<span data-ttu-id="e8112-108">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="e8112-108">Documentation</span></span>](#_Toc303253803)
- [<span data-ttu-id="e8112-109">サポート</span><span class="sxs-lookup"><span data-stu-id="e8112-109">Support</span></span>](#_Toc303253804)
- [<span data-ttu-id="e8112-110">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="e8112-110">Software Requirements</span></span>](#_Toc303253805)
- [<span data-ttu-id="e8112-111">ASP.NET MVC 4 に ASP.NET MVC 3 プロジェクトをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="e8112-111">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>](#_Toc303253806)
- [<span data-ttu-id="e8112-112">ASP.NET MVC 4 Beta の新機能</span><span class="sxs-lookup"><span data-stu-id="e8112-112">New Features in ASP.NET MVC 4 Beta</span></span>](#_Toc303253807)

    - [<span data-ttu-id="e8112-113">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e8112-113">ASP.NET Web API</span></span>](#_Toc317096197)
    - [<span data-ttu-id="e8112-114">ASP.NET の単一ページ アプリケーション</span><span class="sxs-lookup"><span data-stu-id="e8112-114">ASP.NET Single Page Application</span></span>](#_Toc317096198)
    - [<span data-ttu-id="e8112-115">既定のプロジェクト テンプレートの機能強化</span><span class="sxs-lookup"><span data-stu-id="e8112-115">Enhancements to Default Project Templates</span></span>](#_Toc303253808)
    - [<span data-ttu-id="e8112-116">モバイルのプロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="e8112-116">Mobile Project Template</span></span>](#_Toc303253809)
    - [<span data-ttu-id="e8112-117">表示モード</span><span class="sxs-lookup"><span data-stu-id="e8112-117">Display Modes</span></span>](#_Toc303253810)
    - [<span data-ttu-id="e8112-118">jQuery Mobile、ビュー スイッチャーとブラウザーのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="e8112-118">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>](#_Toc303253811)
    - [<span data-ttu-id="e8112-119">Visual Studio でのコード生成のレシピ</span><span class="sxs-lookup"><span data-stu-id="e8112-119">Recipes for Code Generation in Visual Studio</span></span>](#_Toc303253812)
    - [<span data-ttu-id="e8112-120">非同期コント ローラーのタスクのサポート</span><span class="sxs-lookup"><span data-stu-id="e8112-120">Task Support for Asynchronous Controllers</span></span>](#_Toc303253813)
    - [<span data-ttu-id="e8112-121">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="e8112-121">Azure SDK</span></span>](#_Toc303253814)
    - [<span data-ttu-id="e8112-122">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="e8112-122">Known Issues and Breaking Changes</span></span>](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a><span data-ttu-id="e8112-123">インストールに関する注意事項</span><span class="sxs-lookup"><span data-stu-id="e8112-123">Installation Notes</span></span>

<span data-ttu-id="e8112-124">ASP.NET MVC 4 Beta for Visual Studio 2010 をインストールすることができます、 [ASP.NET MVC 4 のホーム ページ](../mvc/mvc4.md)Web Platform Installer を使用します。</span><span class="sxs-lookup"><span data-stu-id="e8112-124">ASP.NET MVC 4 Beta for Visual Studio 2010 can be installed from the [ASP.NET MVC 4 home page](../mvc/mvc4.md) using the Web Platform Installer.</span></span>

<span data-ttu-id="e8112-125">ASP.NET MVC 4 Beta をインストールする前に、ASP.NET MVC 4 の以前にインストールされたプレビューをアンインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8112-125">You must uninstall any previously installed previews of ASP.NET MVC 4 prior to installing ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="e8112-126">このリリースは、.NET Framework 4.5 Developer Preview と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="e8112-126">This release is not compatible with the .NET Framework 4.5 Developer Preview.</span></span> <span data-ttu-id="e8112-127">ASP.NET MVC 4 Beta をインストールする前に、.NET 4.5 Developer Preview をアンインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8112-127">You must uninstall the .NET 4.5 Developer Preview before installing the ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="e8112-128">ASP.NET MVC 4 をインストールしてサイド バイ サイドで実行できる ASP.NET MVC 3 でします。</span><span class="sxs-lookup"><span data-stu-id="e8112-128">ASP.NET MVC 4 can be installed and can run side-by-side with ASP.NET MVC 3.</span></span>

<a id="_Toc303253803"></a>
## <a name="documentation"></a><span data-ttu-id="e8112-129">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="e8112-129">Documentation</span></span>

<span data-ttu-id="e8112-130">ASP.NET MVC のドキュメントは、次の URL で MSDN web サイトで入手できます。</span><span class="sxs-lookup"><span data-stu-id="e8112-130">Documentation for ASP.NET MVC is available on the MSDN website at the following URL:</span></span>

[<span data-ttu-id="e8112-131">https://go.microsoft.com/fwlink/?LinkID=243043</span><span class="sxs-lookup"><span data-stu-id="e8112-131">https://go.microsoft.com/fwlink/?LinkID=243043</span></span>](https://go.microsoft.com/fwlink/?LinkID=243043)

<span data-ttu-id="e8112-132">チュートリアルと ASP.NET MVC に関する他の情報は、ASP.NET web サイトの MVC 4 ページで使用できる ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。</span><span class="sxs-lookup"><span data-stu-id="e8112-132">Tutorials and other information about ASP.NET MVC are available on the MVC 4 page of the ASP.NET website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).</span></span>

<a id="_Toc303253804"></a>
## <a name="support"></a><span data-ttu-id="e8112-133">Support</span><span class="sxs-lookup"><span data-stu-id="e8112-133">Support</span></span>

<span data-ttu-id="e8112-134">これは、プレビュー リリースであり、公式にサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="e8112-134">This is a preview release and is not officially supported.</span></span> <span data-ttu-id="e8112-135">このリリースの操作についての質問がある場合、ASP.NET MVC フォーラムに投稿 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)) では、ASP.NET コミュニティのメンバーは頻繁に、非公式のサポートを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-135">If you have questions about working with this release, post them to the ASP.NET MVC forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), where members of the ASP.NET community are frequently able to provide informal support.</span></span>

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a><span data-ttu-id="e8112-136">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="e8112-136">Software Requirements</span></span>

<span data-ttu-id="e8112-137">Visual Studio 用の ASP.NET MVC 4 コンポーネントでは、PowerShell 2.0 と Service Pack 1 の Visual Studio 2010 または Visual Web Developer Express 2010 Service Pack 1 のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="e8112-137">The ASP.NET MVC 4 components for Visual Studio require PowerShell 2.0 and either Visual Studio 2010 with Service Pack 1 or Visual Web Developer Express 2010 with Service Pack 1.</span></span>

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a><span data-ttu-id="e8112-138">ASP.NET MVC 4 に ASP.NET MVC 3 プロジェクトをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="e8112-138">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>

<span data-ttu-id="e8112-139">ASP.NET MVC 4 は、する柔軟性が ASP.NET MVC 4 に ASP.NET MVC 3 アプリケーションをアップグレードするときに選択する際に、同じコンピューターに ASP.NET MVC 3 のサイド バイ サイドでインストールできます。</span><span class="sxs-lookup"><span data-stu-id="e8112-139">ASP.NET MVC 4 can be installed side by side with ASP.NET MVC 3 on the same computer, which gives you flexibility in choosing when to upgrade an ASP.NET MVC 3 application to ASP.NET MVC 4.</span></span>

<span data-ttu-id="e8112-140">アップグレードする最も簡単な方法は、新しい ASP.NET MVC 4 プロジェクトおよびコピーを作成するすべてのビュー、コント ローラー、コード、およびコンテンツのファイル既存の MVC 3 プロジェクトからを新しいプロジェクトと、古いプロジェクトと一致する新しいプロジェクトで参照アセンブリを更新するのです。</span><span class="sxs-lookup"><span data-stu-id="e8112-140">The simplest way to upgrade is to create a new ASP.NET MVC 4 project and copy all the views, controllers, code, and content files from the existing MVC 3 project to the new project and then to update the assembly references in the new project to match the old project.</span></span> <span data-ttu-id="e8112-141">MVC 3 プロジェクトの Web.config ファイルに変更を行った場合は、MVC 4 プロジェクトの Web.config ファイルにもそれらの変更をマージする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8112-141">If you have made changes to the Web.config file in the MVC 3 project, you must also merge those changes into the Web.config file in the MVC 4 project.</span></span>

<span data-ttu-id="e8112-142">既存の ASP.NET MVC 3 アプリケーションをバージョン 4 を手動でアップグレードするには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="e8112-142">To manually upgrade an existing ASP.NET MVC 3 application to version 4, do the following:</span></span>

1. <span data-ttu-id="e8112-143">(はビュー フォルダーにし、プロジェクト内の各領域の Views フォルダーのプロジェクトのルートに 1 つ)、プロジェクト内のすべての Web.config ファイルでは、次のテキストのすべてのインスタンスに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e8112-143">In all Web.config files in the project (there is one in the root of the project, one in the Views folder, and one in the Views folder for each area in your project), replace every instance of the following text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    <span data-ttu-id="e8112-144">次の対応するテキスト。</span><span class="sxs-lookup"><span data-stu-id="e8112-144">with the following corresponding text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. <span data-ttu-id="e8112-145">ルートの Web.config ファイルで更新、 *webPages:Version* 「2.0.0.0」に要素を追加、新しい*PreserveLoginUrl* "true"の値を持つキー。</span><span class="sxs-lookup"><span data-stu-id="e8112-145">In the root Web.config file, update the *webPages:Version* element to "2.0.0.0" and add a new *PreserveLoginUrl* key that has the value "true":</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. <span data-ttu-id="e8112-146">ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (どの地点をバージョン 3 の DLL) です。</span><span class="sxs-lookup"><span data-stu-id="e8112-146">In Solution Explorer, delete the reference to *System.Web.Mvc* (which points to the version 3 DLL).</span></span> <span data-ttu-id="e8112-147">参照を追加*System.Web.Mvc* (v4.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="e8112-147">Then add a reference to *System.Web.Mvc* (v4.0.0.0).</span></span> <span data-ttu-id="e8112-148">具体的には、次の変更をアセンブリ参照を更新します。</span><span class="sxs-lookup"><span data-stu-id="e8112-148">In particular, make the following changes to update the assembly references.</span></span> <span data-ttu-id="e8112-149">その詳細な手順を次に示します。</span><span class="sxs-lookup"><span data-stu-id="e8112-149">Here are the details:</span></span>

    1. <span data-ttu-id="e8112-150">ソリューション エクスプ ローラーで、次のアセンブリへの参照を削除します。</span><span class="sxs-lookup"><span data-stu-id="e8112-150">In Solution Explorer, delete the references to the following assemblies:</span></span> 

        - <span data-ttu-id="e8112-151">*System.Web.Mvc*(v3.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-151">*System.Web.Mvc*(v3.0.0.0)</span></span>
        - <span data-ttu-id="e8112-152">*System.Web.WebPages*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-152">*System.Web.WebPages*(v1.0.0.0)</span></span>
        - <span data-ttu-id="e8112-153">*System.Web.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-153">*System.Web.Razor*(v1.0.0.0)</span></span>
        - <span data-ttu-id="e8112-154">*System.Web.WebPages.Deployment*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-154">*System.Web.WebPages.Deployment*(v1.0.0.0)</span></span>
        - <span data-ttu-id="e8112-155">*System.Web.WebPages.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-155">*System.Web.WebPages.Razor*(v1.0.0.0)</span></span>
    2. <span data-ttu-id="e8112-156">次のアセンブリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="e8112-156">Add a references to the following assemblies:</span></span> 

        - <span data-ttu-id="e8112-157">*System.Web.Mvc*(v4.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-157">*System.Web.Mvc*(v4.0.0.0)</span></span>
        - <span data-ttu-id="e8112-158">*System.Web.WebPages*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-158">*System.Web.WebPages*(v2.0.0.0)</span></span>
        - <span data-ttu-id="e8112-159">*System.Web.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-159">*System.Web.Razor*(v2.0.0.0)</span></span>
        - <span data-ttu-id="e8112-160">*System.Web.WebPages.Deployment*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-160">*System.Web.WebPages.Deployment*(v2.0.0.0)</span></span>
        - <span data-ttu-id="e8112-161">*System.Web.WebPages.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="e8112-161">*System.Web.WebPages.Razor*(v2.0.0.0)</span></span>
4. <span data-ttu-id="e8112-162">ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="e8112-162">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="e8112-163">[名前を再度右クリックして編集] を選択*ProjectName*.csproj です。</span><span class="sxs-lookup"><span data-stu-id="e8112-163">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
5. <span data-ttu-id="e8112-164">検索、 *ProjectTypeGuids*要素と置換 {e53f8fea-eae0-44a6-8774-ffd645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。</span><span class="sxs-lookup"><span data-stu-id="e8112-164">Locate the *ProjectTypeGuids* element and replace {E53F8FEA-EAE0-44A6-8774-FFD645390401} with {E3E379DF-F4C6-4180-9B81-6769533ABE47}.</span></span>
6. <span data-ttu-id="e8112-165">変更を保存、編集していたプロジェクト (.csproj) ファイルを閉じて、プロジェクトを右クリックしておよびプロジェクトの再読み込みを選択します。</span><span class="sxs-lookup"><span data-stu-id="e8112-165">Save the changes, close the project (.csproj) file you were editing, right-click the project, and then select Reload Project.</span></span>
7. <span data-ttu-id="e8112-166">ルートの Web.config ファイルを開き、プロジェクトは、ASP.NET MVC の以前のバージョンを使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合、次の 3 つの追加*bindingRedirect*の下の要素、 *構成*セクション。</span><span class="sxs-lookup"><span data-stu-id="e8112-166">If the project references any third-party libraries that are compiled using previous versions of ASP.NET MVC, open the root Web.config file and add the following three *bindingRedirect* elements under the *configuration* section:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a><span data-ttu-id="e8112-167">ASP.NET MVC 4 Beta の新機能</span><span class="sxs-lookup"><span data-stu-id="e8112-167">New Features in ASP.NET MVC 4 Beta</span></span>

<span data-ttu-id="e8112-168">このセクションの内容が導入された機能について説明します、ASP.NET MVC 4 Beta リリースにします。</span><span class="sxs-lookup"><span data-stu-id="e8112-168">This section describes features that have been introduced in the ASP.NET MVC 4 Beta release.</span></span>

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a><span data-ttu-id="e8112-169">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="e8112-169">ASP.NET Web API</span></span>

<span data-ttu-id="e8112-170">ASP.NET MVC 4 には、ASP.NET Web API は、HTTP サービスを作成するための新しいフレームワークは、多様なブラウザーやモバイル デバイスを含む、クライアントに到達できます。</span><span class="sxs-lookup"><span data-stu-id="e8112-170">ASP.NET MVC 4 now includes ASP.NET Web API, a new framework for creating HTTP services that can reach a broad range of clients including browsers and mobile devices.</span></span> <span data-ttu-id="e8112-171">ASP.NET Web API は RESTful サービスを構築するための最適なプラットフォームもです。</span><span class="sxs-lookup"><span data-stu-id="e8112-171">ASP.NET Web API is also an ideal platform for building RESTful services.</span></span>

<span data-ttu-id="e8112-172">ASP.NET Web API には、次の機能のサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e8112-172">ASP.NET Web API includes support for the following features:</span></span>

- <span data-ttu-id="e8112-173">**最新の HTTP プログラミング モデル:**直接アクセスし、HTTP 要求と応答を厳密に型指定された新しい HTTP オブジェクト モデルを使用して、Web Api を操作します。</span><span class="sxs-lookup"><span data-stu-id="e8112-173">**Modern HTTP programming model:** Directly access and manipulate HTTP requests and responses in your Web APIs using a new, strongly typed HTTP object model.</span></span> <span data-ttu-id="e8112-174">同じプログラミング モデルと HTTP パイプラインは新しい HttpClient 型を使用してクライアントで対称的に使用できます。</span><span class="sxs-lookup"><span data-stu-id="e8112-174">The same programming model and HTTP pipeline is symmetrically available on the client through the new HttpClient type.</span></span>
- <span data-ttu-id="e8112-175">**ルートの完全なサポート**: Web Api では、ルート パラメーターや制約など、Web スタックの一部であった常にルート機能の完全なセットをサポートします。</span><span class="sxs-lookup"><span data-stu-id="e8112-175">**Full support for routes**: Web APIs now support the full set of route capabilities that have always been a part of the Web stack, including route parameters and constraints.</span></span> <span data-ttu-id="e8112-176">さらに、アクションへのマッピングがの表記規則の完全なサポート不要になった [HttpPost] などの属性を適用するようにクラスとメソッドをします。</span><span class="sxs-lookup"><span data-stu-id="e8112-176">Additionally, mapping to actions has full support for conventions, so you no longer need to apply attributes such as [HttpPost] to your classes and methods.</span></span>
- <span data-ttu-id="e8112-177">**コンテンツ ネゴシエーション**: クライアントとサーバーを連携を API から返されるデータの適切な形式を決定します。</span><span class="sxs-lookup"><span data-stu-id="e8112-177">**Content negotiation**: The client and server can work together to determine the right format for data being returned from an API.</span></span> <span data-ttu-id="e8112-178">XML、JSON、およびフォームの URL でエンコードされた形式の既定のサポートを提供して、独自のフォーマッタを追加することでこのサポートを拡張またはでも既定コンテンツ ネゴシエーションの戦略を置換することができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-178">We provide default support for XML, JSON, and Form URL-encoded formats, and you can extend this support by adding your own formatters, or even replace the default content negotiation strategy.</span></span>
- <span data-ttu-id="e8112-179">**モデル バインディングと検証:**モデル バインダーは、HTTP 要求のさまざまな部分からデータを抽出し、それらのメッセージ部分を Web API の操作が使用できるように .NET オブジェクトに変換する簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="e8112-179">**Model binding and validation:** Model binders provide an easy way to extract data from various parts of an HTTP request and convert those message parts into .NET objects which can be used by the Web API actions.</span></span>
- <span data-ttu-id="e8112-180">**フィルター:** Web Api が [Authorize] 属性などのよく知られているフィルターを含む、フィルターになりました。</span><span class="sxs-lookup"><span data-stu-id="e8112-180">**Filters:** Web APIs now supports filters, including well-known filters such as the [Authorize] attribute.</span></span> <span data-ttu-id="e8112-181">作成し、独自のフィルター操作、承認、および例外処理用にプラグインします。</span><span class="sxs-lookup"><span data-stu-id="e8112-181">You can author and plug in your own filters for actions, authorization and exception handling.</span></span>
- <span data-ttu-id="e8112-182">**クエリの構成:** IQueryable を単に返すことによって&lt;T&gt;、Web API は OData URL 規則によってクエリをサポートします。</span><span class="sxs-lookup"><span data-stu-id="e8112-182">**Query composition:** By simply returning IQueryable&lt;T&gt;, your Web API will support querying via the OData URL conventions.</span></span>
- <span data-ttu-id="e8112-183">**HTTP の詳細のテストの容易性の向上:**静的コンテキスト オブジェクトの HTTP の詳細を設定するのではなく、Web API 操作は HttpRequestMessage と HttpResponseMessage のインスタンスで使用できるようことができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-183">**Improved testability of HTTP details:** Rather than setting HTTP details in static context objects, Web API actions can now work with instances of HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="e8112-184">これらのオブジェクトのジェネリック バージョンは、HTTP の種類に加えて、カスタムの型を操作するためにも存在します。</span><span class="sxs-lookup"><span data-stu-id="e8112-184">Generic versions of these objects also exist to let you work with your custom types in addition to the HTTP types.</span></span>
- <span data-ttu-id="e8112-185">**制御の反転 (IoC) DependencyResolver 経由での向上:** Web API は、多くのさまざまな機能のインスタンスを取得する MVC の依存関係競合回避モジュールによって実装されたサービス ロケーターのパターンを使用するようになりました。</span><span class="sxs-lookup"><span data-stu-id="e8112-185">**Improved Inversion of Control (IoC) via DependencyResolver:** Web API now uses the service locator pattern implemented by MVC's dependency resolver to obtain instances for many different facilities.</span></span>
- <span data-ttu-id="e8112-186">**コード ベースの構成:**コードを通じてのみ Web API の構成には、ファイルのクリーンアップの設定のままです。</span><span class="sxs-lookup"><span data-stu-id="e8112-186">**Code-based configuration:** Web API configuration is accomplished solely through code, leaving your config files clean.</span></span>
- <span data-ttu-id="e8112-187">**自己ホスト:** Web Api は、ルートの全機能と Web API の他の機能を使用中に IIS だけでなく、独自のプロセスでホストできます。</span><span class="sxs-lookup"><span data-stu-id="e8112-187">**Self-host:** Web APIs can be hosted in your own process in addition to IIS while still using the full power of routes and other features of Web API.</span></span>

<span data-ttu-id="e8112-188">ASP.NET Web API の詳細についてはご参照ください[https://www.asp.net/web-api](../web-api/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="e8112-188">For more details on ASP.NET Web API please visit [https://www.asp.net/web-api](../web-api/index.md).</span></span>

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a><span data-ttu-id="e8112-189">ASP.NET の単一ページ アプリケーション</span><span class="sxs-lookup"><span data-stu-id="e8112-189">ASP.NET Single Page Application</span></span>

<span data-ttu-id="e8112-190">ASP.NET MVC 4 には、さまざまな相互重要なクライアント側 JavaScript および Web Api を使用して単一ページ アプリケーションを構築するためのエクスペリエンスの初期のプレビューが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e8112-190">ASP.NET MVC 4 now includes an early preview of the experience for building single page applications with significant client-side interactions using JavaScript and Web APIs.</span></span> <span data-ttu-id="e8112-191">このサポートは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="e8112-191">This support includes:</span></span>

- <span data-ttu-id="e8112-192">キャッシュされたデータのローカルの相互作用を高度な JavaScript ライブラリのセット</span><span class="sxs-lookup"><span data-stu-id="e8112-192">A set of JavaScript libraries for richer local interactions with cached data</span></span>
- <span data-ttu-id="e8112-193">単位の作業と DAL のサポートの追加の Web API コンポーネント</span><span class="sxs-lookup"><span data-stu-id="e8112-193">Additional Web API components for unit of work and DAL support</span></span>
- <span data-ttu-id="e8112-194">すぐに開始するスキャフォールディングでの MVC プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="e8112-194">An MVC project template with scaffolding to get started quickly</span></span>

<span data-ttu-id="e8112-195">単一ページ アプリケーションの詳細については、ASP.NET MVC 4 のサポートを参照してください[https://www.asp.net/single-page-application](../single-page-application/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="e8112-195">For more details on the Single Page Application support in ASP.NET MVC 4 please visit [https://www.asp.net/single-page-application](../single-page-application/index.md).</span></span>

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a><span data-ttu-id="e8112-196">既定のプロジェクト テンプレートの機能強化</span><span class="sxs-lookup"><span data-stu-id="e8112-196">Enhancements to Default Project Templates</span></span>

<span data-ttu-id="e8112-197">現代の外見の複数の web サイトを作成する新しい ASP.NET MVC 4 プロジェクトの作成に使用されるテンプレートが更新されました。</span><span class="sxs-lookup"><span data-stu-id="e8112-197">The template that is used to create new ASP.NET MVC 4 projects has been updated to create a more modern-looking website:</span></span>

![](mvc4-beta-release-notes/_static/image1.png)

<span data-ttu-id="e8112-198">表面的な機能強化に加え、新しいテンプレートの機能をきました。</span><span class="sxs-lookup"><span data-stu-id="e8112-198">In addition to cosmetic improvements, there's improved functionality in the new template.</span></span> <span data-ttu-id="e8112-199">テンプレートには、デスクトップ ブラウザーとカスタマイズすることがなくモバイル ブラウザーの両方できれいに表示するアダプティブと呼ばれる技術が採用しています。</span><span class="sxs-lookup"><span data-stu-id="e8112-199">The template employs a technique called adaptive rendering to look good in both desktop browsers and mobile browsers without any customization.</span></span>

![](mvc4-beta-release-notes/_static/image2.png)

<span data-ttu-id="e8112-200">アダプティブのレンダリングの動作を表示するには、モバイル エミュレーターを使用するか小さくなるようにデスクトップ ブラウザーのウィンドウのサイズを変更してみてください。</span><span class="sxs-lookup"><span data-stu-id="e8112-200">To see adaptive rendering in action, you can use a mobile emulator or just try resizing the desktop browser window to be smaller.</span></span> <span data-ttu-id="e8112-201">ブラウザーのウィンドウは、大きすぎて取得、ページのレイアウトが変わります。</span><span class="sxs-lookup"><span data-stu-id="e8112-201">When the browser window gets small enough, the layout of the page will change.</span></span>

<span data-ttu-id="e8112-202">別の拡張機能で、既定のプロジェクト テンプレートは、豊富な UI を提供する JavaScript の使用です。</span><span class="sxs-lookup"><span data-stu-id="e8112-202">Another enhancement to the default project template is the use of JavaScript to provide a richer UI.</span></span> <span data-ttu-id="e8112-203">テンプレートで使用されるログインおよび登録のリンクは、jQuery UI ダイアログを使用して、豊富なログイン画面を表示する方法の例を示します。</span><span class="sxs-lookup"><span data-stu-id="e8112-203">The Login and Register links that are used in the template are examples of how to use the jQuery UI Dialog to present a rich login screen:</span></span>

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a><span data-ttu-id="e8112-204">モバイルのプロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="e8112-204">Mobile Project Template</span></span>

<span data-ttu-id="e8112-205">新しいプロジェクトを開始しているモバイル向けサイトおよびタブレット ブラウザーを作成する場合、新しいモバイル アプリケーション プロジェクト テンプレートを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-205">If you're starting a new project and want to create a site specifically for mobile and tablet browsers, you can use the new Mobile Application project template.</span></span> <span data-ttu-id="e8112-206">これは、jQuery Mobile、タッチ最適化の UI を構築するため、オープン ソース ライブラリに基づいています。</span><span class="sxs-lookup"><span data-stu-id="e8112-206">This is based on jQuery Mobile, an open-source library for building touch-optimized UI:</span></span>

![](mvc4-beta-release-notes/_static/image4.png)

<span data-ttu-id="e8112-207">このテンプレートには、インターネット アプリケーションのテンプレートと同じアプリケーション構造が含まれています (およびコント ローラーのコードはほぼ同じです) が、jQuery Mobile を使用して、適切に表示し、モバイル デバイスのタッチ ベースで適切に動作するスタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="e8112-207">This template contains the same application structure as the Internet Application template (and the controller code is virtually identical), but it's styled using jQuery Mobile to look good and behave well on touch-based mobile devices.</span></span> <span data-ttu-id="e8112-208">構造体し、モバイルの UI のスタイルを設定する方法の詳細については、次を参照してください。、 [jQuery モバイル プロジェクトの web サイト](http://jquerymobile.com/)です。</span><span class="sxs-lookup"><span data-stu-id="e8112-208">To learn more about how to structure and style mobile UI, see the [jQuery Mobile project website](http://jquerymobile.com/).</span></span>

<span data-ttu-id="e8112-209">デスクトップ向けのサイトをモバイル用に最適化されたビューを追加する場合や、デスクトップとモバイル ブラウザー スタイルの異なるビューが機能する 1 つのサイトを作成する場合がある場合は、新しい表示モード機能を使用することができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-209">If you already have a desktop-oriented site that you want to add mobile-optimized views to, or if you want to create a single site that serves differently styled views to desktop and mobile browsers, you can use the new Display Modes feature.</span></span> <span data-ttu-id="e8112-210">(詳しくは、次のセクションを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="e8112-210">(See the next section.)</span></span>

<a id="_Toc303253810"></a>
### <a name="display-modes"></a><span data-ttu-id="e8112-211">表示モード</span><span class="sxs-lookup"><span data-stu-id="e8112-211">Display Modes</span></span>

<span data-ttu-id="e8112-212">新しい表示モード機能には、要求を行って、ブラウザーによってビューを選択して、アプリケーションができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-212">The new Display Modes feature lets an application select views depending on the browser that's making the request.</span></span> <span data-ttu-id="e8112-213">たとえば、デスクトップ ブラウザーでは、ホーム ページを要求している場合、アプリケーションは Views\Home\Index.cshtml テンプレートを使用できます。</span><span class="sxs-lookup"><span data-stu-id="e8112-213">For example, if a desktop browser requests the Home page, the application might use the Views\Home\Index.cshtml template.</span></span> <span data-ttu-id="e8112-214">モバイル ブラウザーは、ホーム ページを要求している場合、アプリケーションは Views\Home\Index.mobile.cshtml テンプレートを返す場合があります。</span><span class="sxs-lookup"><span data-stu-id="e8112-214">If a mobile browser requests the Home page, the application might return the Views\Home\Index.mobile.cshtml template.</span></span>

<span data-ttu-id="e8112-215">レイアウトとパーシャルは、特定のブラウザーの種類を無効にできます。</span><span class="sxs-lookup"><span data-stu-id="e8112-215">Layouts and partials can also be overridden for particular browser types.</span></span> <span data-ttu-id="e8112-216">例:</span><span class="sxs-lookup"><span data-stu-id="e8112-216">For example:</span></span>

- <span data-ttu-id="e8112-217">\Shared フォルダーには、両方が含まれている場合、 \_Layout.cshtml と\_Layout.mobile.cshtml テンプレート、既定では、アプリケーションで使用\_モバイル ブラウザーとからの要求中にLayout.mobile.cshtml\_Layout.cshtml 他の要求時にします。</span><span class="sxs-lookup"><span data-stu-id="e8112-217">If your Views\Shared folder contains both the \_Layout.cshtml and \_Layout.mobile.cshtml templates, by default the application will use \_Layout.mobile.cshtml during requests from mobile browsers and \_Layout.cshtml during other requests.</span></span>
- <span data-ttu-id="e8112-218">フォルダーには、両方が含まれている場合\_MyPartial.cshtml と\_MyPartial.mobile.cshtml、命令@Html.Partial("\_MyPartial") はレンダリング\_携帯電話からの要求時に MyPartial.mobile.cshtmlブラウザー、および\_MyPartial.cshtml 他の要求時にします。</span><span class="sxs-lookup"><span data-stu-id="e8112-218">If a folder contains both \_MyPartial.cshtml and \_MyPartial.mobile.cshtml, the instruction @Html.Partial("\_MyPartial") will render \_MyPartial.mobile.cshtml during requests from mobile browsers, and \_MyPartial.cshtml during other requests.</span></span>

<span data-ttu-id="e8112-219">新しいを登録することがより特定のビュー、レイアウト、またはその他のデバイスの部分ビューを作成する場合は、 *DefaultDisplayMode*要求は、特定の条件を満たす場合に検索する名前を指定するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="e8112-219">If you want to create more specific views, layouts, or partial views for other devices, you can register a new *DefaultDisplayMode* instance to specify which name to search for when a request satisfies particular conditions.</span></span> <span data-ttu-id="e8112-220">次のコードを追加するなど、*アプリケーション\_開始*Apple iPhone ブラウザーは要求時に適用される表示モードとして、文字列"iPhone"を登録する Global.asax ファイル内のメソッド。</span><span class="sxs-lookup"><span data-stu-id="e8112-220">For example, you could add the following code to the *Application\_Start* method in the Global.asax file to register the string "iPhone" as a display mode that applies when the Apple iPhone browser makes a request:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

<span data-ttu-id="e8112-221">アプリケーションで、\shared を使用して、Apple iPhone ブラウザーが要求するときに、このコードを実行すた後、\\_Layout.iPhone.cshtml レイアウト (ある場合)。</span><span class="sxs-lookup"><span data-stu-id="e8112-221">After this code runs, when an Apple iPhone browser makes a request, your application will use the Views\Shared\\_Layout.iPhone.cshtml layout (if it exists).</span></span>

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a><span data-ttu-id="e8112-222">jQuery Mobile、ビュー スイッチャーとブラウザーのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="e8112-222">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>

<span data-ttu-id="e8112-223">jQuery Mobile は、タッチ最適化 web UI を構築するため、オープン ソース ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="e8112-223">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="e8112-224">ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用する場合は、ダウンロードして、作業を開始するのに役立つ NuGet パッケージをインストールします。</span><span class="sxs-lookup"><span data-stu-id="e8112-224">If you want to use jQuery Mobile with an ASP.NET MVC 4 application, you can download and install a NuGet package that helps you get started.</span></span> <span data-ttu-id="e8112-225">Visual Studio パッケージ マネージャー コンソールからインストールするには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="e8112-225">To install it from the Visual Studio Package Manager Console, type the following command:</span></span>

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

<span data-ttu-id="e8112-226">これには、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="e8112-226">This installs jQuery Mobile and some helper files, including the following:</span></span>

- <span data-ttu-id="e8112-227">Views/shared/\_Layout.Mobile.cshtml、jQuery Mobile ベースのレイアウトであります。</span><span class="sxs-lookup"><span data-stu-id="e8112-227">Views/Shared/\_Layout.Mobile.cshtml, which is a jQuery Mobile-based layout.</span></span>
- <span data-ttu-id="e8112-228">ビュー/共有から構成されるビュー スイッチャーコンポーネント/\_ViewSwitcher.cshtml 部分ビューと ViewSwitcherController.cs コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="e8112-228">A view-switcher component, which consists of the Views/Shared/\_ViewSwitcher.cshtml partial view and the ViewSwitcherController.cs controller.</span></span>

<span data-ttu-id="e8112-229">パッケージをインストールした後は、モバイル ブラウザーを使用してアプリケーションを実行 (または、Firefox など、同等の[ユーザー エージェント スイッチャー](http://chrispederick.com/work/user-agent-switcher/)アドオン)。</span><span class="sxs-lookup"><span data-stu-id="e8112-229">After you install the package, run your application using a mobile browser (or equivalent, like the Firefox [User Agent Switcher](http://chrispederick.com/work/user-agent-switcher/) add-on).</span></span> <span data-ttu-id="e8112-230">エンドユーザー表示 jQuery Mobile はレイアウトを処理するため、ページがまったく異なることを確認し、スタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="e8112-230">You'll see that your pages look quite different, because jQuery Mobile handles layout and styling.</span></span> <span data-ttu-id="e8112-231">これを利用するには、次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-231">To take advantage of this, you can do the following:</span></span>

- <span data-ttu-id="e8112-232">下の説明に従って、mobile に固有のビューの上書きを作成[表示モード](#_Toc303253810)前 (たとえば、モバイル ブラウザーの Views\Home\Index.cshtml を上書きする Views\Home\Index.mobile.cshtml を作成します)。</span><span class="sxs-lookup"><span data-stu-id="e8112-232">Create mobile-specific view overrides as described under [Display Modes](#_Toc303253810) earlier (for example, create Views\Home\Index.mobile.cshtml to override Views\Home\Index.cshtml for mobile browsers).</span></span>
- <span data-ttu-id="e8112-233">読み取り、 [jQuery Mobile ドキュメント](http://jquerymobile.com/)モバイル ビューでタッチ最適化の UI 要素を追加する方法の詳細についてはします。</span><span class="sxs-lookup"><span data-stu-id="e8112-233">Read the [jQuery Mobile documentation](http://jquerymobile.com/) to learn more about how to add touch-optimized UI elements in mobile views.</span></span>

<span data-ttu-id="e8112-234">モバイルに最適化された web ページの規則では、テキストはデスクトップ ビューまたはユーザーがページのデスクトップ バージョンに切り替えるできるサイトの完全モードと同様に、何かのリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="e8112-234">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="e8112-235">JQuery.Mobile.MVC パッケージには、この目的のためのビュー スイッチャー コンポーネント サンプルにはが含まれています。</span><span class="sxs-lookup"><span data-stu-id="e8112-235">The jQuery.Mobile.MVC package includes a sample view-switcher component for this purpose.</span></span> <span data-ttu-id="e8112-236">既定 \shared で使用されている\\_Layout.Mobile.cshtml 表示し、ページが表示されるときに次のように。</span><span class="sxs-lookup"><span data-stu-id="e8112-236">It's used in the default Views\Shared\\_Layout.Mobile.cshtml view, and it looks like this when the page is rendered:</span></span>

![](mvc4-beta-release-notes/_static/image5.png)

<span data-ttu-id="e8112-237">閲覧者は、リンクをクリックして、同じページのデスクトップ バージョンに切り替えられますしています。</span><span class="sxs-lookup"><span data-stu-id="e8112-237">If visitors click the link, they're switched to the desktop version of the same page.</span></span>

<span data-ttu-id="e8112-238">デスクトップのレイアウトには既定のビュー スイッチャーが含まれていないために、訪問者はモバイル モードを取得する方法を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="e8112-238">Because your desktop layout will not include a view switcher by default, visitors won't have a way to get to mobile mode.</span></span> <span data-ttu-id="e8112-239">これが有効にする次の参照を追加 *\_ViewSwitcher*内だけ使用してデスクトップのレイアウトを*&lt;本文&gt;*要素。</span><span class="sxs-lookup"><span data-stu-id="e8112-239">To enable this, add the following reference to *\_ViewSwitcher* to your desktop layout, just inside the *&lt;body&gt;* element:</span></span>

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

<span data-ttu-id="e8112-240">ビュー スイッチャーは、ブラウザーのオーバーライドと呼ばれる新しい機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="e8112-240">The view switcher uses a new feature called Browser Overriding.</span></span> <span data-ttu-id="e8112-241">この機能により、アプリケーションは、飛び出してきたかのように要求を扱うものの別のブラウザー (ユーザー エージェント) から、あなたが実際にします。</span><span class="sxs-lookup"><span data-stu-id="e8112-241">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they're actually from.</span></span> <span data-ttu-id="e8112-242">次の表は、ブラウザーのオーバーライドを提供する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="e8112-242">The following table lists the methods that Browser Overriding provides.</span></span>

| `HttpContext.SetOverriddenBrowser(userAgentString)` | <span data-ttu-id="e8112-243">指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="e8112-243">Overrides the request's actual user agent value using the specified user agent.</span></span> |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | <span data-ttu-id="e8112-244">オーバーライドが指定されていない場合は、要求のユーザー エージェントの上書き値、または実際のユーザー エージェント文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="e8112-244">Returns the request's user agent override value, or the actual user agent string if no override has been specified.</span></span> |
| `HttpContext.GetOverriddenBrowser()` | <span data-ttu-id="e8112-245">返します、 *HttpBrowserCapabilitiesBase*要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライドされる)。</span><span class="sxs-lookup"><span data-stu-id="e8112-245">Returns an *HttpBrowserCapabilitiesBase* instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="e8112-246">この値を使用するにはプロパティを取得するよう*IsMobileDevice*です。</span><span class="sxs-lookup"><span data-stu-id="e8112-246">You can use this value to get properties such as *IsMobileDevice*.</span></span> |
| `HttpContext.ClearOverriddenBrowser()` | <span data-ttu-id="e8112-247">現在の要求に対するすべてのオーバーライドされたユーザー エージェントを削除します。</span><span class="sxs-lookup"><span data-stu-id="e8112-247">Removes any overridden user agent for the current request.</span></span> |

<span data-ttu-id="e8112-248">ブラウザー オーバーライドは、ASP.NET MVC 4 の中心的な機能し、jQuery.Mobile.MVC パッケージをインストールしない場合でもは有効です。</span><span class="sxs-lookup"><span data-stu-id="e8112-248">Browser Overriding is a core feature of ASP.NET MVC 4 and is available even if you don't install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="e8112-249">ただし、影響ビュー、レイアウト、および部分ビューの選択-その他の ASP.NET 機能に依存するのには影響しません、 *Request.Browser*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="e8112-249">However, it affects only view, layout, and partial-view selection — it does not affect any other ASP.NET feature that depends on the *Request.Browser* object.</span></span>

<span data-ttu-id="e8112-250">既定では、ユーザー エージェントの上書きは、cookie を使用して格納されます。</span><span class="sxs-lookup"><span data-stu-id="e8112-250">By default, the user-agent override is stored using a cookie.</span></span> <span data-ttu-id="e8112-251">既定のプロバイダーを置き換えることができます (たとえば、データベース内の) で他の場所で、上書きを保存する場合は、(*BrowserOverrideStores.Current*)。</span><span class="sxs-lookup"><span data-stu-id="e8112-251">If you want to store the override elsewhere (for example, in a database), you can replace the default provider (*BrowserOverrideStores.Current*).</span></span> <span data-ttu-id="e8112-252">このプロバイダーのマニュアルをできるようにする ASP.NET MVC の以降のリリースに付属します。</span><span class="sxs-lookup"><span data-stu-id="e8112-252">Documentation for this provider will be available to accompany a later release of ASP.NET MVC.</span></span>

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a><span data-ttu-id="e8112-253">Visual Studio でのコード生成のレシピ</span><span class="sxs-lookup"><span data-stu-id="e8112-253">Recipes for Code Generation in Visual Studio</span></span>

<span data-ttu-id="e8112-254">新しいレシピ機能では、NuGet を使用してをインストールするパッケージに基づくソリューション固有のコードを生成する Visual Studio を使用します。</span><span class="sxs-lookup"><span data-stu-id="e8112-254">The new Recipes feature enables Visual Studio to generate solution-specific code based on packages that you can install using NuGet.</span></span> <span data-ttu-id="e8112-255">レシピ framework 簡単開発者用を置換する組み込みのコード ジェネレーターを追加の領域、コント ローラーの追加、およびビューの追加使用することもできます。 コード生成プラグインを作成します。</span><span class="sxs-lookup"><span data-stu-id="e8112-255">The Recipes framework makes it easy for developers to write code-generation plugins, which you can also use to replace the built-in code generators for Add Area, Add Controller, and Add View.</span></span> <span data-ttu-id="e8112-256">レシピは、NuGet パッケージとして展開される、ため、簡単にソース管理にチェックインして自動的にプロジェクトのすべての開発者と共有します。</span><span class="sxs-lookup"><span data-stu-id="e8112-256">Because recipes are deployed as NuGet packages, they can easily be checked into source control and shared with all developers on the project automatically.</span></span> <span data-ttu-id="e8112-257">ソリューションごとに記載されています。</span><span class="sxs-lookup"><span data-stu-id="e8112-257">They are also available on a per-solution basis.</span></span>

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a><span data-ttu-id="e8112-258">非同期コント ローラーのタスクのサポート</span><span class="sxs-lookup"><span data-stu-id="e8112-258">Task Support for Asynchronous Controllers</span></span>

<span data-ttu-id="e8112-259">記述できるように非同期アクション メソッドを型のオブジェクトを返す 1 つのメソッド*タスク*または*タスク&lt;ActionResult&gt;*です。</span><span class="sxs-lookup"><span data-stu-id="e8112-259">You can now write asynchronous action methods as single methods that return an object of type *Task* or *Task&lt;ActionResult&gt;*.</span></span>

<span data-ttu-id="e8112-260">たとえば、Visual C# 5 を使用している場合 (またはを使用して、 [Async CTP](https://msdn.microsoft.com/en-us/vstudio/async.aspx))、次のような非同期アクション メソッドを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="e8112-260">For example, if you're using Visual C# 5 (or using the [Async CTP](https://msdn.microsoft.com/en-us/vstudio/async.aspx)), you can create an asynchronous action method that looks like the following:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

<span data-ttu-id="e8112-261">前のアクション メソッドへの呼び出しで*newsService.GetHeadlinesAsync*と*sportsService.GetScoresAsync*非同期に呼び出され、スレッド プールのスレッドはブロックされません。</span><span class="sxs-lookup"><span data-stu-id="e8112-261">In the previous action method, the calls to *newsService.GetHeadlinesAsync* and *sportsService.GetScoresAsync* are called asynchronously and do not block a thread from the thread pool.</span></span>

<span data-ttu-id="e8112-262">返す非同期アクション メソッド*タスク*インスタンスは、タイムアウトにもサポートできます。</span><span class="sxs-lookup"><span data-stu-id="e8112-262">Asynchronous action methods that return *Task* instances can also support timeouts.</span></span> <span data-ttu-id="e8112-263">アクション メソッドをキャンセル可能にするために、型のパラメーターを追加*CancellationToken*アクション メソッドのシグネチャにします。</span><span class="sxs-lookup"><span data-stu-id="e8112-263">To make your action method cancellable, add a parameter of type *CancellationToken* to the action method signature.</span></span> <span data-ttu-id="e8112-264">次の例では、2500 ミリ秒のタイムアウトを持つし、表示する非同期アクション メソッド、 *TimedOut*タイムアウトが発生した場合、クライアントに表示します。</span><span class="sxs-lookup"><span data-stu-id="e8112-264">The following example shows an asynchronous action method that has a timeout of 2500 milliseconds and that displays a *TimedOut* view to the client if a timeout occurs.</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a><span data-ttu-id="e8112-265">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="e8112-265">Azure SDK</span></span>

<span data-ttu-id="e8112-266">ASP.NET MVC 4 Beta は、Windows Azure SDK の 2011 年 9 月 1.5 のリリースをサポートします。</span><span class="sxs-lookup"><span data-stu-id="e8112-266">ASP.NET MVC 4 Beta supports the September 2011 1.5 release of the Windows Azure SDK.</span></span>

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="e8112-267">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="e8112-267">Known Issues and Breaking Changes</span></span>

- <span data-ttu-id="e8112-268">**ASP.NET MVC 4 Beta をインストールすると、Visual Studio 2010 Service Pack 1 CSHTML/VBHTML エディターで、CSHTML/VBHTML エディターは可能性があります cshtml"や"vbhtml ファイル内のスニペットまたは JavaScript の入力後に長時間一時停止します。**</span><span class="sxs-lookup"><span data-stu-id="e8112-268">**After installing ASP.NET MVC 4 Beta, the CSHTML/VBHTML editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML editor may pause for a long time after typing snippet or JavaScript inside cshtml or vbhtml files.**</span></span> <span data-ttu-id="e8112-269">これは、作成された直後、まだコンパイルされていないと ASP.NET MVC 4 アプリケーションでのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="e8112-269">This occurs only in ASP.NET MVC 4 applications which have just been created and have not yet been compiled.</span></span>

    <span data-ttu-id="e8112-270">回避策では、bin フォルダーにアセンブリを取得するプロジェクトをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="e8112-270">The workaround is to compile the project to get the assemblies in the bin folder.</span></span> <span data-ttu-id="e8112-271">エディターの問題が返される、bin フォルダーからアセンブリが削除され、プロジェクトをクリーンアップする場合に注意してください。</span><span class="sxs-lookup"><span data-stu-id="e8112-271">Note, if you clean the project which removes the assemblies from the bin folder, the editor problem will come back.</span></span>

    <span data-ttu-id="e8112-272">これは、次のリリースで修正される予定です。</span><span class="sxs-lookup"><span data-stu-id="e8112-272">This will be corrected in the next release.</span></span>
- <span data-ttu-id="e8112-273">**Visual Studio 11 Beta 用の c# プロジェクト テンプレートには、Global.asax.cs 内の不適切な接続文字列が含まれます。**</span><span class="sxs-lookup"><span data-stu-id="e8112-273">**C# Project templates for Visual Studio 11 Beta contain an incorrect connection string in Global.asax.cs.**</span></span> <span data-ttu-id="e8112-274">アプリケーションで指定された既定の接続\_Visual Studio 11 Beta で作成したプロジェクトには、エスケープ解除されたバック スラッシュを含んでいる LocalDB 接続文字列が含まれているメソッドを開始 (\)文字です。</span><span class="sxs-lookup"><span data-stu-id="e8112-274">The default connection specified in the Application\_Start method for projects created in Visual Studio 11 Beta contain a LocalDB connection string which contains an unescaped backslash (\) character.</span></span> <span data-ttu-id="e8112-275">これは、結果、接続エラー、SqlException が生成されるエンティティ フレームワーク DbContext にアクセスしようとします。</span><span class="sxs-lookup"><span data-stu-id="e8112-275">This results in a connection error upon attempts to access a Entity Framework DbContext, which generates a SqlException.</span></span>

    <span data-ttu-id="e8112-276">この問題を修正するアプリのバック スラッシュ文字をエスケープ\_Global.asax.cs の方法を開始するように読むことができるようにします。</span><span class="sxs-lookup"><span data-stu-id="e8112-276">To correct this issue, escape the backslash character in the App\_Start method of Global.asax.cs so that it reads as follows:</span></span>

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- <span data-ttu-id="e8112-277">**.NET 4.5 を対象とする ASP.NET MVC 4 アプリケーションでは、.NET 4.0 で実行する場合、System.Net.Http.dll アセンブリにアクセスしようとしたときに FileLoadException をスローします。**</span><span class="sxs-lookup"><span data-stu-id="e8112-277">**ASP.NET MVC 4 applications which target .NET 4.5 will throw a FileLoadException upon attempt to access the System.Net.Http.dll assembly when run under .NET 4.0.**</span></span> <span data-ttu-id="e8112-278">.NET 4.5 で作成した ASP.NET MVC 4 アプリケーションは、バインディングを格納するようになる FileLoadException の原因となるリダイレクト「を読み込めませんでしたファイルまたはアセンブリ 'System.Net.Http' またはその依存関係の 1 つです」。</span><span class="sxs-lookup"><span data-stu-id="e8112-278">ASP.NET MVC 4 applications created under .NET 4.5 contain a binding redirect that will result in a FileLoadException which that states "Could not load file or assembly 'System.Net.Http' or one of its dependencies."</span></span> <span data-ttu-id="e8112-279">ときに、アプリケーションは、.NET 4.0 がインストールされたシステムで実行されます。</span><span class="sxs-lookup"><span data-stu-id="e8112-279">when the application is executed on a system with .NET 4.0 installed.</span></span> <span data-ttu-id="e8112-280">この問題を解決するには、web.config ファイルから次のバインド リダイレクトを削除します。</span><span class="sxs-lookup"><span data-stu-id="e8112-280">To correct this issue, remove the following binding redirect from web.config:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    <span data-ttu-id="e8112-281">変更した web.config でアセンブリのバインディング要素として次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="e8112-281">The assembly binding element in the modified web.config should appear as follows:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <span data-ttu-id="e8112-282">**Visual Basic プロジェクトで「コント ローラーの追加」の項目テンプレートを生成、正しくない名前空間呼び出されたときに * * * から領域内です。**</span><span class="sxs-lookup"><span data-stu-id="e8112-282">**The "Add Controller" item template in Visual Basic projects generates an incorrect namespace when invoked****from inside an area.**</span></span> <span data-ttu-id="e8112-283">Visual Basic を使用する ASP.NET MVC プロジェクト内の領域に、コント ローラーを追加すると、項目テンプレートは、コント ローラーに間違った名前空間を挿入します。</span><span class="sxs-lookup"><span data-stu-id="e8112-283">When you add a controller to an area in an ASP.NET MVC project that uses Visual Basic, the item template inserts the wrong namespace into the controller.</span></span> <span data-ttu-id="e8112-284">コント ローラーのどのアクションに移動するときに、「ファイルが見つかりません」エラーになります。</span><span class="sxs-lookup"><span data-stu-id="e8112-284">The result is a "file not found" error when you navigate to any action in the controller.</span></span>  
  
 <span data-ttu-id="e8112-285">生成された名前空間は、ルート名前空間の後にすべてのものを省略します。</span><span class="sxs-lookup"><span data-stu-id="e8112-285">The generated namespace omits everything after the root namespace.</span></span> <span data-ttu-id="e8112-286">たとえば、生成された名前空間は*RootNamespace*する必要がありますが、 *RootNamespace.Areas.AreaName.Controllers*です。</span><span class="sxs-lookup"><span data-stu-id="e8112-286">For example, the namespace generated is *RootNamespace* but should be *RootNamespace.Areas.AreaName.Controllers* .</span></span>
- <span data-ttu-id="e8112-287">**Razor ビュー エンジンにおける重大な変更です。**</span><span class="sxs-lookup"><span data-stu-id="e8112-287">**Breaking changes in the Razor View Engine.**</span></span> <span data-ttu-id="e8112-288">Razor パーサーの書き換えの一環として、次の種類はから削除された*System.Web.Mvc.Razor*:</span><span class="sxs-lookup"><span data-stu-id="e8112-288">As part of a rewrite of the Razor parser, the following types were removed from *System.Web.Mvc.Razor*:</span></span> 

    - <span data-ttu-id="e8112-289">*ModelSpan*</span><span class="sxs-lookup"><span data-stu-id="e8112-289">*ModelSpan*</span></span>
    - <span data-ttu-id="e8112-290">*MvcVBRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="e8112-290">*MvcVBRazorCodeGenerator*</span></span>
    - <span data-ttu-id="e8112-291">*MvcCSharpRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="e8112-291">*MvcCSharpRazorCodeGenerator*</span></span>
    - <span data-ttu-id="e8112-292">*MvcVBRazorCodeParser*</span><span class="sxs-lookup"><span data-stu-id="e8112-292">*MvcVBRazorCodeParser*</span></span>

 <span data-ttu-id="e8112-293">次の方法も削除されます。</span><span class="sxs-lookup"><span data-stu-id="e8112-293">The following methods were also removed:</span></span> 

    - <span data-ttu-id="e8112-294">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="e8112-294">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
    - <span data-ttu-id="e8112-295">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span><span class="sxs-lookup"><span data-stu-id="e8112-295">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span></span>
    - <span data-ttu-id="e8112-296">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="e8112-296">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
- <span data-ttu-id="e8112-297">**WebMatrix.WebData.dll に含めるときは、ASP.NET MVC 4 アプリケーションの/bin ディレクトリに、フォーム認証の URL になります。**</span><span class="sxs-lookup"><span data-stu-id="e8112-297">**When WebMatrix.WebData.dll is included in in the /bin directory of an ASP.NET MVC 4 apps, it takes over the URL for forms authentication.**</span></span> <span data-ttu-id="e8112-298">(たとえば、展開可能な依存関係の追加 ダイアログを使用する場合は、「Razor 構文で ASP.NET Web ページ」を選択する) を WebMatrix.WebData.dll アセンブリをアプリケーションに追加すると、認証ログインへのリダイレクト/アカウント/ログオンよりも優先されますのではなく/既定の ASP.NET MVC アカウント コント ローラーで期待どおりに、アカウント/ログインします。</span><span class="sxs-lookup"><span data-stu-id="e8112-298">Adding the WebMatrix.WebData.dll assembly to your application (for example, by selecting "ASP.NET Web Pages with Razor Syntax" when using the Add Deployable Dependencies dialog) will override the authentication login redirect to /account/logon rather than /account/login as expected by the default ASP.NET MVC Account Controller.</span></span> <span data-ttu-id="e8112-299">この動作を回避し、web.config ファイルの [認証] セクションで既に指定された URL を使用する PreserveLoginUrl と呼ばれる、appSetting を追加およびを true に設定できます。</span><span class="sxs-lookup"><span data-stu-id="e8112-299">To prevent this behavior and use the URL specified already in the authentication section of web.config, you can add an appSetting called PreserveLoginUrl and set it to true:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- <span data-ttu-id="e8112-300">**NuGet パッケージ マネージャーは、Visual Studio 2010 と Visual Web Developer 2010 のサイド バイ サイド インストールの ASP.NET MVC 4 をインストールしようとするときのインストールに失敗します。**</span><span class="sxs-lookup"><span data-stu-id="e8112-300">**The NuGet package manager fails to install when attempting to install ASP.NET MVC 4 for side by side installations of Visual Studio 2010 and Visual Web Developer 2010.**</span></span> <span data-ttu-id="e8112-301">Visual Studio 2010 と ASP.NET MVC 4 サイド バイ サイドで Visual Web Developer 2010 を実行するには、Visual Studio の両方のバージョンを既にインストールされている後に ASP.NET MVC 4 をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8112-301">To run Visual Studio 2010 and Visual Web Developer 2010 side by side with ASP.NET MVC 4 you must install ASP.NET MVC 4 after both versions of Visual Studio have already been installed.</span></span>
- <span data-ttu-id="e8112-302">**前提条件が既にアンインストールされている場合、ASP.NET MVC 4 のアンインストールが失敗します。**</span><span class="sxs-lookup"><span data-stu-id="e8112-302">**Uninstalling ASP.NET MVC 4 fails if prerequisites have already been uninstalled.**</span></span> <span data-ttu-id="e8112-303">ASP.NET MVC を完全にアンインストールするには、4you は、Visual Studio をアンインストールする前に ASP.NET MVC 4 をアンインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8112-303">To cleanly uninstall ASP.NET MVC 4you must uninstall ASP.NET MVC 4 prior to uninstalling Visual Studio.</span></span>
- <span data-ttu-id="e8112-304">**既定の Web API プロジェクトを実行しているが存在しない、RegisterApis メソッドを使用してルートを追加するユーザーを正しく指示する命令を示しています。**</span><span class="sxs-lookup"><span data-stu-id="e8112-304">**Running a default Web API project shows instructions that incorrectly direct the user to add routes using the RegisterApis method, which doesn't exist.**</span></span> <span data-ttu-id="e8112-305">ASP.NET ルート テーブルを使用して、RegisterRoutes メソッドでは、ルートを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="e8112-305">Routes should be added in the RegisterRoutes method using the ASP.NET route table.</span></span>
- <span data-ttu-id="e8112-306">**ASP.NET MVC 4 Beta をインストールすると、アプリケーションの ASP.NET MVC 3 RTM が中断されます。**</span><span class="sxs-lookup"><span data-stu-id="e8112-306">**Installing ASP.NET MVC 4 Beta breaks ASP.NET MVC 3 RTM applications.**</span></span> <span data-ttu-id="e8112-307">ASP.NET MVC 3 アプリケーションを作成したリリース (ASP.NET MVC 3 Tools Update リリース) ではなく、RTM のサイド バイ サイド ASP.NET MVC 4 Beta で動作するために、次の変更を必要とします。</span><span class="sxs-lookup"><span data-stu-id="e8112-307">ASP.NET MVC 3 applications that were created with the RTM release (not with the ASP.NET MVC 3 Tools Update release) require the following changes in order to work side-by-side with ASP.NET MVC 4 Beta.</span></span> <span data-ttu-id="e8112-308">コンパイル エラーにこれらの更新プログラムの結果を加えずにプロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="e8112-308">Building the project without making these updates results in compilation errors.</span></span> 

    <span data-ttu-id="e8112-309">**必要な更新プログラム**</span><span class="sxs-lookup"><span data-stu-id="e8112-309">**Required updates**</span></span>

    1. <span data-ttu-id="e8112-310">ルートの Web.config ファイルで追加、新しい *&lt;appSettings&gt;* キーを持つエントリ*webPages:Version*および値*1.0.0.0*です。</span><span class="sxs-lookup"><span data-stu-id="e8112-310">In the root Web.config file, add a new *&lt;appSettings&gt;* entry with the key *webPages:Version* and the value *1.0.0.0*.</span></span>

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
    2. <span data-ttu-id="e8112-311">ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="e8112-311">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="e8112-312">[名前を再度右クリックして編集] を選択*ProjectName*.csproj です。</span><span class="sxs-lookup"><span data-stu-id="e8112-312">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
    3. <span data-ttu-id="e8112-313">次のアセンブリ参照を見つけます。</span><span class="sxs-lookup"><span data-stu-id="e8112-313">Locate the following assembly references:</span></span> 

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

        <span data-ttu-id="e8112-314">次のように、それらを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="e8112-314">Replace them with the following:</span></span>

        [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
    4. <span data-ttu-id="e8112-315">変更を保存、閉じるプロジェクト (.csproj) ファイルを編集、およびし、プロジェクトを右クリックし、再読み込みを選択します。</span><span class="sxs-lookup"><span data-stu-id="e8112-315">Save the changes, close the project (.csproj) file you were editing, and then right-click the project and select Reload.</span></span>
