---
uid: whitepapers/mvc4-beta-release-notes
title: ASP.NET MVC 4 |Microsoft Docs
author: rick-anderson
description: このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。
ms.author: aspnetcontent
ms.date: 09/09/2011
ms.assetid: 666407bb-81de-4319-89ba-0302c382a208
msc.legacyurl: /whitepapers/mvc4-beta-release-notes
msc.type: content
ms.openlocfilehash: b9d50114a239b67b1adc263f6ea6d3a811bcc8f7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816692"
---
<a name="aspnet-mvc-4"></a><span data-ttu-id="2df43-103">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="2df43-103">ASP.NET MVC 4</span></span>
====================
> <span data-ttu-id="2df43-104">このドキュメントでは、ASP.NET MVC 4 Beta の Visual Studio 2010 のリリースについて説明します。</span><span class="sxs-lookup"><span data-stu-id="2df43-104">This document describes the release of ASP.NET MVC 4 Beta for Visual Studio 2010.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="2df43-105">これは、最新のリリースではありません。</span><span class="sxs-lookup"><span data-stu-id="2df43-105">This is not the most current release.</span></span> <span data-ttu-id="2df43-106">ASP.NET MVC 4 RC のリリース ノートは[ここ](mvc4-release-notes.md)します。</span><span class="sxs-lookup"><span data-stu-id="2df43-106">The ASP.NET MVC 4 RC release notes are available [here](mvc4-release-notes.md).</span></span>


- [<span data-ttu-id="2df43-107">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="2df43-107">Installation Notes</span></span>](#_Toc303253802)
- [<span data-ttu-id="2df43-108">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2df43-108">Documentation</span></span>](#_Toc303253803)
- [<span data-ttu-id="2df43-109">サポート</span><span class="sxs-lookup"><span data-stu-id="2df43-109">Support</span></span>](#_Toc303253804)
- [<span data-ttu-id="2df43-110">ソフトウェアの要件</span><span class="sxs-lookup"><span data-stu-id="2df43-110">Software Requirements</span></span>](#_Toc303253805)
- [<span data-ttu-id="2df43-111">ASP.NET mvc 4、ASP.NET MVC 3 プロジェクトをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="2df43-111">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>](#_Toc303253806)
- [<span data-ttu-id="2df43-112">ASP.NET MVC 4 Beta の新機能</span><span class="sxs-lookup"><span data-stu-id="2df43-112">New Features in ASP.NET MVC 4 Beta</span></span>](#_Toc303253807)

    - [<span data-ttu-id="2df43-113">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="2df43-113">ASP.NET Web API</span></span>](#_Toc317096197)
    - [<span data-ttu-id="2df43-114">ASP.NET Single Page Application</span><span class="sxs-lookup"><span data-stu-id="2df43-114">ASP.NET Single Page Application</span></span>](#_Toc317096198)
    - [<span data-ttu-id="2df43-115">既定のプロジェクト テンプレートの機能強化</span><span class="sxs-lookup"><span data-stu-id="2df43-115">Enhancements to Default Project Templates</span></span>](#_Toc303253808)
    - [<span data-ttu-id="2df43-116">モバイル プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="2df43-116">Mobile Project Template</span></span>](#_Toc303253809)
    - [<span data-ttu-id="2df43-117">表示モード</span><span class="sxs-lookup"><span data-stu-id="2df43-117">Display Modes</span></span>](#_Toc303253810)
    - [<span data-ttu-id="2df43-118">jQuery Mobile、ビュー スイッチャー、およびブラウザーのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="2df43-118">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>](#_Toc303253811)
    - [<span data-ttu-id="2df43-119">Visual Studio でのコード生成のレシピ</span><span class="sxs-lookup"><span data-stu-id="2df43-119">Recipes for Code Generation in Visual Studio</span></span>](#_Toc303253812)
    - [<span data-ttu-id="2df43-120">非同期コント ローラーのタスクのサポート</span><span class="sxs-lookup"><span data-stu-id="2df43-120">Task Support for Asynchronous Controllers</span></span>](#_Toc303253813)
    - [<span data-ttu-id="2df43-121">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="2df43-121">Azure SDK</span></span>](#_Toc303253814)
    - [<span data-ttu-id="2df43-122">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="2df43-122">Known Issues and Breaking Changes</span></span>](#_Toc303253815)

<a id="_Toc303253802"></a>
## <a name="installation-notes"></a><span data-ttu-id="2df43-123">インストールに関する注記</span><span class="sxs-lookup"><span data-stu-id="2df43-123">Installation Notes</span></span>

<span data-ttu-id="2df43-124">Visual Studio 2010 の ASP.NET MVC 4 Beta をインストールすることができます、[ホーム ページの ASP.NET MVC 4](../mvc/mvc4.md) Web Platform Installer を使用します。</span><span class="sxs-lookup"><span data-stu-id="2df43-124">ASP.NET MVC 4 Beta for Visual Studio 2010 can be installed from the [ASP.NET MVC 4 home page](../mvc/mvc4.md) using the Web Platform Installer.</span></span>

<span data-ttu-id="2df43-125">ASP.NET MVC 4 Beta をインストールする前に、ASP.NET MVC 4 の以前にインストールのプレビューをアンインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-125">You must uninstall any previously installed previews of ASP.NET MVC 4 prior to installing ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="2df43-126">このリリースは、.NET Framework 4.5 Developer Preview と互換性がありません。</span><span class="sxs-lookup"><span data-stu-id="2df43-126">This release is not compatible with the .NET Framework 4.5 Developer Preview.</span></span> <span data-ttu-id="2df43-127">ASP.NET MVC 4 Beta をインストールする前に、.NET 4.5 Developer Preview をアンインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-127">You must uninstall the .NET 4.5 Developer Preview before installing the ASP.NET MVC 4 Beta.</span></span>

<span data-ttu-id="2df43-128">ASP.NET MVC 4 インストールでき、サイド バイ サイドでを実行できる ASP.NET MVC 3 でします。</span><span class="sxs-lookup"><span data-stu-id="2df43-128">ASP.NET MVC 4 can be installed and can run side-by-side with ASP.NET MVC 3.</span></span>

<a id="_Toc303253803"></a>
## <a name="documentation"></a><span data-ttu-id="2df43-129">ドキュメント</span><span class="sxs-lookup"><span data-stu-id="2df43-129">Documentation</span></span>

<span data-ttu-id="2df43-130">ASP.NET MVC に関する次の URL で MSDN web サイトで提供されています。</span><span class="sxs-lookup"><span data-stu-id="2df43-130">Documentation for ASP.NET MVC is available on the MSDN website at the following URL:</span></span>

[https://go.microsoft.com/fwlink/?LinkID=243043](https://go.microsoft.com/fwlink/?LinkID=243043)

<span data-ttu-id="2df43-131">チュートリアルと ASP.NET MVC に関する他の情報は、ASP.NET web サイトの MVC 4 のページで使用可能な ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md))。</span><span class="sxs-lookup"><span data-stu-id="2df43-131">Tutorials and other information about ASP.NET MVC are available on the MVC 4 page of the ASP.NET website ([https://www.asp.net/mvc/mvc4](../mvc/mvc4.md)).</span></span>

<a id="_Toc303253804"></a>
## <a name="support"></a><span data-ttu-id="2df43-132">サポート</span><span class="sxs-lookup"><span data-stu-id="2df43-132">Support</span></span>

<span data-ttu-id="2df43-133">これは、プレビュー リリースであり、正式にサポートされていません。</span><span class="sxs-lookup"><span data-stu-id="2df43-133">This is a preview release and is not officially supported.</span></span> <span data-ttu-id="2df43-134">このリリースの操作について質問等がございましたら、ASP.NET MVC フォーラムに投稿 ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx))、ASP.NET コミュニティのメンバーが頻繁に、非公式のサポートを提供することができます。</span><span class="sxs-lookup"><span data-stu-id="2df43-134">If you have questions about working with this release, post them to the ASP.NET MVC forum ([https://forums.asp.net/1146.aspx](https://forums.asp.net/1146.aspx)), where members of the ASP.NET community are frequently able to provide informal support.</span></span>

<a id="_Toc303253805"></a>
## <a name="software-requirements"></a><span data-ttu-id="2df43-135">ソフトウェア要件</span><span class="sxs-lookup"><span data-stu-id="2df43-135">Software Requirements</span></span>

<span data-ttu-id="2df43-136">Visual Studio の ASP.NET MVC 4 コンポーネントでは、PowerShell 2.0 と Visual Studio 2010 Service Pack 1 または Visual Web Developer Express 2010 Service Pack 1 のいずれかが必要です。</span><span class="sxs-lookup"><span data-stu-id="2df43-136">The ASP.NET MVC 4 components for Visual Studio require PowerShell 2.0 and either Visual Studio 2010 with Service Pack 1 or Visual Web Developer Express 2010 with Service Pack 1.</span></span>

<a id="_Toc303253806"></a>
## <a name="upgrading-an-aspnet-mvc-3-project-to-aspnet-mvc-4"></a><span data-ttu-id="2df43-137">ASP.NET mvc 4、ASP.NET MVC 3 プロジェクトをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="2df43-137">Upgrading an ASP.NET MVC 3 Project to ASP.NET MVC 4</span></span>

<span data-ttu-id="2df43-138">ASP.NET MVC 4 は、ASP.NET MVC 4、ASP.NET MVC 3 アプリケーション アップグレードするときに選択できる柔軟性を提供する同じコンピューター上の ASP.NET MVC 3 サイドでインストールできます。</span><span class="sxs-lookup"><span data-stu-id="2df43-138">ASP.NET MVC 4 can be installed side by side with ASP.NET MVC 3 on the same computer, which gives you flexibility in choosing when to upgrade an ASP.NET MVC 3 application to ASP.NET MVC 4.</span></span>

<span data-ttu-id="2df43-139">アップグレードする最も簡単な方法は、新しい ASP.NET MVC 4 プロジェクトとコピーを作成するすべてのビュー、コント ローラー、コード、およびコンテンツ ファイルの既存の MVC 3 プロジェクトから新しいプロジェクトに、古いプロジェクトと一致する新しいプロジェクトで参照アセンブリを更新するのには、です。</span><span class="sxs-lookup"><span data-stu-id="2df43-139">The simplest way to upgrade is to create a new ASP.NET MVC 4 project and copy all the views, controllers, code, and content files from the existing MVC 3 project to the new project and then to update the assembly references in the new project to match the old project.</span></span> <span data-ttu-id="2df43-140">MVC 3 プロジェクトの Web.config ファイルに変更を加えた場合は、MVC 4 プロジェクトの Web.config ファイルにもこれらの変更をマージする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-140">If you have made changes to the Web.config file in the MVC 3 project, you must also merge those changes into the Web.config file in the MVC 4 project.</span></span>

<span data-ttu-id="2df43-141">既存の ASP.NET MVC 3 アプリケーションをバージョン 4 を手動でアップグレードするには、次の操作を行います。</span><span class="sxs-lookup"><span data-stu-id="2df43-141">To manually upgrade an existing ASP.NET MVC 3 application to version 4, do the following:</span></span>

1. <span data-ttu-id="2df43-142">(は Views フォルダー、およびプロジェクト内の各領域の Views フォルダー内のプロジェクトのルートに 1 つずつ) プロジェクトのすべての Web.config ファイルでは、次のテキストのすべてのインスタンスを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2df43-142">In all Web.config files in the project (there is one in the root of the project, one in the Views folder, and one in the Views folder for each area in your project), replace every instance of the following text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample1.cmd)]

    <span data-ttu-id="2df43-143">次の対応するテキスト。</span><span class="sxs-lookup"><span data-stu-id="2df43-143">with the following corresponding text:</span></span>

    [!code-console[Main](mvc4-beta-release-notes/samples/sample2.cmd)]
2. <span data-ttu-id="2df43-144">ルートの Web.config ファイルで更新、 *webPages:Version*要素「2.0.0.0」を新しい追加*PreserveLoginUrl*値"true"を持つキー。</span><span class="sxs-lookup"><span data-stu-id="2df43-144">In the root Web.config file, update the *webPages:Version* element to "2.0.0.0" and add a new *PreserveLoginUrl* key that has the value "true":</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample3.xml)]
3. <span data-ttu-id="2df43-145">ソリューション エクスプ ローラーへの参照を削除*System.Web.Mvc* (どの地点をバージョン 3 の DLL) です。</span><span class="sxs-lookup"><span data-stu-id="2df43-145">In Solution Explorer, delete the reference to *System.Web.Mvc* (which points to the version 3 DLL).</span></span> <span data-ttu-id="2df43-146">参照を追加し、 *System.Web.Mvc* (v4.0.0.0)。</span><span class="sxs-lookup"><span data-stu-id="2df43-146">Then add a reference to *System.Web.Mvc* (v4.0.0.0).</span></span> <span data-ttu-id="2df43-147">具体的には、アセンブリ参照を更新する次の変更を行います。</span><span class="sxs-lookup"><span data-stu-id="2df43-147">In particular, make the following changes to update the assembly references.</span></span> <span data-ttu-id="2df43-148">その詳細な手順を次に示します。</span><span class="sxs-lookup"><span data-stu-id="2df43-148">Here are the details:</span></span>

    1. <span data-ttu-id="2df43-149">ソリューション エクスプ ローラーで、次のアセンブリへの参照を削除します。</span><span class="sxs-lookup"><span data-stu-id="2df43-149">In Solution Explorer, delete the references to the following assemblies:</span></span> 

        - <span data-ttu-id="2df43-150">*System.Web.Mvc*(v3.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-150">*System.Web.Mvc*(v3.0.0.0)</span></span>
        - <span data-ttu-id="2df43-151">*System.Web.WebPages*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-151">*System.Web.WebPages*(v1.0.0.0)</span></span>
        - <span data-ttu-id="2df43-152">*System.Web.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-152">*System.Web.Razor*(v1.0.0.0)</span></span>
        - <span data-ttu-id="2df43-153">*System.Web.WebPages.Deployment*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-153">*System.Web.WebPages.Deployment*(v1.0.0.0)</span></span>
        - <span data-ttu-id="2df43-154">*System.Web.WebPages.Razor*(v1.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-154">*System.Web.WebPages.Razor*(v1.0.0.0)</span></span>
    2. <span data-ttu-id="2df43-155">次のアセンブリへの参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="2df43-155">Add a references to the following assemblies:</span></span> 

        - <span data-ttu-id="2df43-156">*System.Web.Mvc*(v4.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-156">*System.Web.Mvc*(v4.0.0.0)</span></span>
        - <span data-ttu-id="2df43-157">*System.Web.WebPages*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-157">*System.Web.WebPages*(v2.0.0.0)</span></span>
        - <span data-ttu-id="2df43-158">*System.Web.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-158">*System.Web.Razor*(v2.0.0.0)</span></span>
        - <span data-ttu-id="2df43-159">*System.Web.WebPages.Deployment*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-159">*System.Web.WebPages.Deployment*(v2.0.0.0)</span></span>
        - <span data-ttu-id="2df43-160">*System.Web.WebPages.Razor*(v2.0.0.0)</span><span class="sxs-lookup"><span data-stu-id="2df43-160">*System.Web.WebPages.Razor*(v2.0.0.0)</span></span>
4. <span data-ttu-id="2df43-161">ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="2df43-161">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="2df43-162">名前をもう一度右クリックし、編集*ProjectName*.csproj します。</span><span class="sxs-lookup"><span data-stu-id="2df43-162">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
5. <span data-ttu-id="2df43-163">検索、 *ProjectTypeGuids*要素と置換 {E53F8FEA-EAE0-44A6-8774-FFD645390401} {E3E379DF-F4C6-4180-9B81-6769533ABE47}。</span><span class="sxs-lookup"><span data-stu-id="2df43-163">Locate the *ProjectTypeGuids* element and replace {E53F8FEA-EAE0-44A6-8774-FFD645390401} with {E3E379DF-F4C6-4180-9B81-6769533ABE47}.</span></span>
6. <span data-ttu-id="2df43-164">変更を保存、編集していたプロジェクト (.csproj) ファイルを閉じて、プロジェクトを右クリックしておよびプロジェクトの再読み込みを選択します。</span><span class="sxs-lookup"><span data-stu-id="2df43-164">Save the changes, close the project (.csproj) file you were editing, right-click the project, and then select Reload Project.</span></span>
7. <span data-ttu-id="2df43-165">ルートの Web.config ファイルを開き、プロジェクトでは、ASP.NET MVC の以前のバージョンを使用してコンパイルされたすべてのサード パーティ製ライブラリを参照する場合、次の 3 つの追加*bindingRedirect*下の要素、 *構成*セクション。</span><span class="sxs-lookup"><span data-stu-id="2df43-165">If the project references any third-party libraries that are compiled using previous versions of ASP.NET MVC, open the root Web.config file and add the following three *bindingRedirect* elements under the *configuration* section:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample4.xml)]

<a id="_Toc303253807"></a>
## <a name="new-features-in-aspnet-mvc-4-beta"></a><span data-ttu-id="2df43-166">ASP.NET MVC 4 Beta の新機能</span><span class="sxs-lookup"><span data-stu-id="2df43-166">New Features in ASP.NET MVC 4 Beta</span></span>

<span data-ttu-id="2df43-167">このセクションが導入された機能について説明します、ASP.NET MVC 4 Beta リリースにします。</span><span class="sxs-lookup"><span data-stu-id="2df43-167">This section describes features that have been introduced in the ASP.NET MVC 4 Beta release.</span></span>

<a id="_Toc317096197"></a>
### <a name="aspnet-web-api"></a><span data-ttu-id="2df43-168">ASP.NET Web API</span><span class="sxs-lookup"><span data-stu-id="2df43-168">ASP.NET Web API</span></span>

<span data-ttu-id="2df43-169">HTTP サービスを作成するための新しいフレームワークは、さまざまなブラウザーやモバイル デバイスなどのクライアントに到達できる、ASP.NET MVC 4 には、ASP.NET Web API にはようになりましたが含まれます。</span><span class="sxs-lookup"><span data-stu-id="2df43-169">ASP.NET MVC 4 now includes ASP.NET Web API, a new framework for creating HTTP services that can reach a broad range of clients including browsers and mobile devices.</span></span> <span data-ttu-id="2df43-170">ASP.NET Web API は RESTful サービスを構築するための理想的なプラットフォームではもです。</span><span class="sxs-lookup"><span data-stu-id="2df43-170">ASP.NET Web API is also an ideal platform for building RESTful services.</span></span>

<span data-ttu-id="2df43-171">ASP.NET Web API には、次の機能のサポートが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2df43-171">ASP.NET Web API includes support for the following features:</span></span>

- <span data-ttu-id="2df43-172">**最新の HTTP プログラミング モデル:** 直接アクセスし、HTTP 要求と応答を返す新しい、厳密に型指定された HTTP オブジェクト モデルを使用して、Web Api を操作します。</span><span class="sxs-lookup"><span data-stu-id="2df43-172">**Modern HTTP programming model:** Directly access and manipulate HTTP requests and responses in your Web APIs using a new, strongly typed HTTP object model.</span></span> <span data-ttu-id="2df43-173">同じプログラミング モデルと HTTP パイプラインは、新しい種類の HttpClient を介してクライアントで対称的に使用します。</span><span class="sxs-lookup"><span data-stu-id="2df43-173">The same programming model and HTTP pipeline is symmetrically available on the client through the new HttpClient type.</span></span>
- <span data-ttu-id="2df43-174">**完全にサポート ルート**: Web Api では、常に、ルート パラメーターや制約を含む、Web スタックの一部であったルート機能の完全なセットをサポートします。</span><span class="sxs-lookup"><span data-stu-id="2df43-174">**Full support for routes**: Web APIs now support the full set of route capabilities that have always been a part of the Web stack, including route parameters and constraints.</span></span> <span data-ttu-id="2df43-175">さらに、アクションへのマッピングに、規則の完全なサポート [HttpPost] などの属性を適用するが不要になったため、クラスとメソッド。</span><span class="sxs-lookup"><span data-stu-id="2df43-175">Additionally, mapping to actions has full support for conventions, so you no longer need to apply attributes such as [HttpPost] to your classes and methods.</span></span>
- <span data-ttu-id="2df43-176">**コンテンツ ネゴシエーション**: クライアントとサーバーが連携 API から返されるデータの適切な形式を判断します。</span><span class="sxs-lookup"><span data-stu-id="2df43-176">**Content negotiation**: The client and server can work together to determine the right format for data being returned from an API.</span></span> <span data-ttu-id="2df43-177">XML、JSON、およびフォームの URL でエンコードされた形式では、既定のサポートを提供して、独自のフォーマッタを追加することでこのサポートを拡張または既定のコンテンツ ネゴシエーション戦略を置き換えることもできます。</span><span class="sxs-lookup"><span data-stu-id="2df43-177">We provide default support for XML, JSON, and Form URL-encoded formats, and you can extend this support by adding your own formatters, or even replace the default content negotiation strategy.</span></span>
- <span data-ttu-id="2df43-178">**モデル バインドと検証:** モデル バインダーは、HTTP 要求のさまざまな部分からデータを抽出し、それらのメッセージ部分を Web API の操作が使用できる .NET オブジェクトに変換する簡単な方法を提供します。</span><span class="sxs-lookup"><span data-stu-id="2df43-178">**Model binding and validation:** Model binders provide an easy way to extract data from various parts of an HTTP request and convert those message parts into .NET objects which can be used by the Web API actions.</span></span>
- <span data-ttu-id="2df43-179">**フィルター:** Web Api では、[Authorize] 属性などのよく知られているフィルターなどのフィルターをサポートします。</span><span class="sxs-lookup"><span data-stu-id="2df43-179">**Filters:** Web APIs now supports filters, including well-known filters such as the [Authorize] attribute.</span></span> <span data-ttu-id="2df43-180">作成し、操作、承認、および例外処理フィルターを接続できます。</span><span class="sxs-lookup"><span data-stu-id="2df43-180">You can author and plug in your own filters for actions, authorization and exception handling.</span></span>
- <span data-ttu-id="2df43-181">**クエリの構成:** IQueryable を返すだけで&lt;T&gt;、Web API は OData URL 表記規則を使用してクエリをサポートします。</span><span class="sxs-lookup"><span data-stu-id="2df43-181">**Query composition:** By simply returning IQueryable&lt;T&gt;, your Web API will support querying via the OData URL conventions.</span></span>
- <span data-ttu-id="2df43-182">**HTTP の詳細のテストの容易性の向上:** 静的コンテキスト オブジェクトでは、HTTP の詳細を設定するのではなく、Web API アクションは HttpRequestMessage と HttpResponseMessage のインスタンスを持つ作業ようになりましたことができます。</span><span class="sxs-lookup"><span data-stu-id="2df43-182">**Improved testability of HTTP details:** Rather than setting HTTP details in static context objects, Web API actions can now work with instances of HttpRequestMessage and HttpResponseMessage.</span></span> <span data-ttu-id="2df43-183">これらのオブジェクトのジェネリック バージョンは、HTTP の種類に加えて、カスタムの型を操作するためにも存在します。</span><span class="sxs-lookup"><span data-stu-id="2df43-183">Generic versions of these objects also exist to let you work with your custom types in addition to the HTTP types.</span></span>
- <span data-ttu-id="2df43-184">**制御の反転 (IoC) DependencyResolver 経由での改善:** Web API は、多くのさまざまな機能のインスタンスを取得する MVC の依存関係競合回避モジュールによって実装されるサービス ロケーターのパターンを使用するようになりました。</span><span class="sxs-lookup"><span data-stu-id="2df43-184">**Improved Inversion of Control (IoC) via DependencyResolver:** Web API now uses the service locator pattern implemented by MVC's dependency resolver to obtain instances for many different facilities.</span></span>
- <span data-ttu-id="2df43-185">**コード ベースの構成:** コードを通じてのみ Web API の構成には、ファイルのクリーンアップの設定のままです。</span><span class="sxs-lookup"><span data-stu-id="2df43-185">**Code-based configuration:** Web API configuration is accomplished solely through code, leaving your config files clean.</span></span>
- <span data-ttu-id="2df43-186">**自己ホスト:** Web Api は、ルートの完全な機能と Web API の他の機能を使用中に IIS だけでなく、独自のプロセスでホストできます。</span><span class="sxs-lookup"><span data-stu-id="2df43-186">**Self-host:** Web APIs can be hosted in your own process in addition to IIS while still using the full power of routes and other features of Web API.</span></span>

<span data-ttu-id="2df43-187">ASP.NET Web API の詳細についてを参照してくださいの[ https://www.asp.net/web-api](../web-api/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="2df43-187">For more details on ASP.NET Web API please visit [https://www.asp.net/web-api](../web-api/index.md).</span></span>

<a id="_Toc317096198"></a>
### <a name="aspnet-single-page-application"></a><span data-ttu-id="2df43-188">ASP.NET Single Page Application</span><span class="sxs-lookup"><span data-stu-id="2df43-188">ASP.NET Single Page Application</span></span>

<span data-ttu-id="2df43-189">ASP.NET MVC 4 には、JavaScript、および Web Api を使用して重要なクライアント側の対話をシングル ページ アプリケーションを構築するためのエクスペリエンスの早期プレビュー版が含まれています。</span><span class="sxs-lookup"><span data-stu-id="2df43-189">ASP.NET MVC 4 now includes an early preview of the experience for building single page applications with significant client-side interactions using JavaScript and Web APIs.</span></span> <span data-ttu-id="2df43-190">このサポートは次のとおりです。</span><span class="sxs-lookup"><span data-stu-id="2df43-190">This support includes:</span></span>

- <span data-ttu-id="2df43-191">キャッシュされたデータとローカルのやり取りを高度な JavaScript ライブラリのセット</span><span class="sxs-lookup"><span data-stu-id="2df43-191">A set of JavaScript libraries for richer local interactions with cached data</span></span>
- <span data-ttu-id="2df43-192">作業単位と DAL サポートの追加の Web API コンポーネント</span><span class="sxs-lookup"><span data-stu-id="2df43-192">Additional Web API components for unit of work and DAL support</span></span>
- <span data-ttu-id="2df43-193">すぐに開始するためにスキャフォールディングでの MVC プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="2df43-193">An MVC project template with scaffolding to get started quickly</span></span>

<span data-ttu-id="2df43-194">シングル ページ アプリケーションの詳細については、ASP.NET MVC 4 のサポートを参照してください[ https://www.asp.net/single-page-application](../single-page-application/index.md)します。</span><span class="sxs-lookup"><span data-stu-id="2df43-194">For more details on the Single Page Application support in ASP.NET MVC 4 please visit [https://www.asp.net/single-page-application](../single-page-application/index.md).</span></span>

<a id="_Toc303253808"></a>
### <a name="enhancements-to-default-project-templates"></a><span data-ttu-id="2df43-195">既定のプロジェクト テンプレートの機能強化</span><span class="sxs-lookup"><span data-stu-id="2df43-195">Enhancements to Default Project Templates</span></span>

<span data-ttu-id="2df43-196">現代的な外観の web サイトを作成する、新しい ASP.NET MVC 4 プロジェクトを作成するために使用するテンプレートが更新されました。</span><span class="sxs-lookup"><span data-stu-id="2df43-196">The template that is used to create new ASP.NET MVC 4 projects has been updated to create a more modern-looking website:</span></span>

![](mvc4-beta-release-notes/_static/image1.png)

<span data-ttu-id="2df43-197">表面的な機能強化、だけでなく向上には、新しいテンプレートの機能がいます。</span><span class="sxs-lookup"><span data-stu-id="2df43-197">In addition to cosmetic improvements, there's improved functionality in the new template.</span></span> <span data-ttu-id="2df43-198">テンプレートでは、デスクトップ ブラウザーとモバイル ブラウザーのカスタマイズの両方でスケッチをアダプティブ レンダリングと呼ばれる手法を採用しています。</span><span class="sxs-lookup"><span data-stu-id="2df43-198">The template employs a technique called adaptive rendering to look good in both desktop browsers and mobile browsers without any customization.</span></span>

![](mvc4-beta-release-notes/_static/image2.png)

<span data-ttu-id="2df43-199">アダプティブ レンダリングの動作を表示するはモバイル エミュレーターを使用したり小さくなるように、デスクトップ ブラウザー ウィンドウのサイズを変更してみてください。</span><span class="sxs-lookup"><span data-stu-id="2df43-199">To see adaptive rendering in action, you can use a mobile emulator or just try resizing the desktop browser window to be smaller.</span></span> <span data-ttu-id="2df43-200">ブラウザー ウィンドウが十分に小さいと、ページのレイアウトが変わります。</span><span class="sxs-lookup"><span data-stu-id="2df43-200">When the browser window gets small enough, the layout of the page will change.</span></span>

<span data-ttu-id="2df43-201">既定のプロジェクト テンプレートを別の拡張機能より豊富な UI を提供する JavaScript の使用です。</span><span class="sxs-lookup"><span data-stu-id="2df43-201">Another enhancement to the default project template is the use of JavaScript to provide a richer UI.</span></span> <span data-ttu-id="2df43-202">テンプレートで使用されるログインと登録のリンクは、jQuery UI ダイアログを使用して、豊富なログイン画面を表示する方法の例を示します。</span><span class="sxs-lookup"><span data-stu-id="2df43-202">The Login and Register links that are used in the template are examples of how to use the jQuery UI Dialog to present a rich login screen:</span></span>

![](mvc4-beta-release-notes/_static/image3.png)

<a id="_Toc303253809"></a>
### <a name="mobile-project-template"></a><span data-ttu-id="2df43-203">モバイル プロジェクト テンプレート</span><span class="sxs-lookup"><span data-stu-id="2df43-203">Mobile Project Template</span></span>

<span data-ttu-id="2df43-204">新しいプロジェクトを開始しているモバイル専用のサイトとタブレットのブラウザーを作成する場合、新しいモバイル アプリケーション プロジェクト テンプレートを使用することができます。</span><span class="sxs-lookup"><span data-stu-id="2df43-204">If you're starting a new project and want to create a site specifically for mobile and tablet browsers, you can use the new Mobile Application project template.</span></span> <span data-ttu-id="2df43-205">これは、jQuery Mobile、タッチに最適化された UI を構築するため、オープン ソース ライブラリに基づいています。</span><span class="sxs-lookup"><span data-stu-id="2df43-205">This is based on jQuery Mobile, an open-source library for building touch-optimized UI:</span></span>

![](mvc4-beta-release-notes/_static/image4.png)

<span data-ttu-id="2df43-206">このテンプレートには、インターネット アプリケーション テンプレートと同じアプリケーションの構造が含まれています (およびコント ローラーのコードはほぼ同じです) が、jQuery Mobile を使用して、適切に表示して、タッチ ベースのモバイル デバイスで適切に動作するスタイルを設定します。</span><span class="sxs-lookup"><span data-stu-id="2df43-206">This template contains the same application structure as the Internet Application template (and the controller code is virtually identical), but it's styled using jQuery Mobile to look good and behave well on touch-based mobile devices.</span></span> <span data-ttu-id="2df43-207">構造体し、モバイルの UI のスタイルを設定する方法の詳細については、次を参照してください。、 [jQuery モバイル プロジェクトの web サイト](http://jquerymobile.com/)します。</span><span class="sxs-lookup"><span data-stu-id="2df43-207">To learn more about how to structure and style mobile UI, see the [jQuery Mobile project website](http://jquerymobile.com/).</span></span>

<span data-ttu-id="2df43-208">デスクトップ指向のサイト、モバイルに最適化されたビューを追加するデスクトップとモバイル ブラウザー スタイルの異なるビューを提供する 1 つのサイトを作成する場合に既にある場合は、新しい表示モード機能を使用できます。</span><span class="sxs-lookup"><span data-stu-id="2df43-208">If you already have a desktop-oriented site that you want to add mobile-optimized views to, or if you want to create a single site that serves differently styled views to desktop and mobile browsers, you can use the new Display Modes feature.</span></span> <span data-ttu-id="2df43-209">(詳しくは、次のセクションを参照してください)。</span><span class="sxs-lookup"><span data-stu-id="2df43-209">(See the next section.)</span></span>

<a id="_Toc303253810"></a>
### <a name="display-modes"></a><span data-ttu-id="2df43-210">表示モード</span><span class="sxs-lookup"><span data-stu-id="2df43-210">Display Modes</span></span>

<span data-ttu-id="2df43-211">新しい表示モード機能は、要求を行っているブラウザーによってビューを選択してアプリケーションを使用できます。</span><span class="sxs-lookup"><span data-stu-id="2df43-211">The new Display Modes feature lets an application select views depending on the browser that's making the request.</span></span> <span data-ttu-id="2df43-212">たとえば、デスクトップ ブラウザーでは、ホーム ページを要求する場合に、アプリケーションは Views\Home\Index.cshtml テンプレートを使用可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-212">For example, if a desktop browser requests the Home page, the application might use the Views\Home\Index.cshtml template.</span></span> <span data-ttu-id="2df43-213">モバイル ブラウザーでは、ホーム ページを要求するアプリケーションは Views\Home\Index.mobile.cshtml テンプレートを返す可能性があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-213">If a mobile browser requests the Home page, the application might return the Views\Home\Index.mobile.cshtml template.</span></span>

<span data-ttu-id="2df43-214">レイアウト、パーシャルは、特定のブラウザーの種類を無効にできます。</span><span class="sxs-lookup"><span data-stu-id="2df43-214">Layouts and partials can also be overridden for particular browser types.</span></span> <span data-ttu-id="2df43-215">例えば:</span><span class="sxs-lookup"><span data-stu-id="2df43-215">For example:</span></span>

- <span data-ttu-id="2df43-216">Views \shared フォルダーには、両方が含まれている場合、 \_Layout.cshtml と\_Layout.mobile.cshtml テンプレート、既定では、アプリケーションで使用\_モバイルブラウザーからの要求中にLayout.mobile.cshtml\_Layout.cshtml 他の要求中にします。</span><span class="sxs-lookup"><span data-stu-id="2df43-216">If your Views\Shared folder contains both the \_Layout.cshtml and \_Layout.mobile.cshtml templates, by default the application will use \_Layout.mobile.cshtml during requests from mobile browsers and \_Layout.cshtml during other requests.</span></span>
- <span data-ttu-id="2df43-217">フォルダーには、両方が含まれている場合\_MyPartial.cshtml と\_MyPartial.mobile.cshtml、命令@Html.Partial("\_MyPartial") はレンダリング\_MyPartial.mobile.cshtml 携帯電話からの要求時にブラウザー、および\_MyPartial.cshtml 他の要求中にします。</span><span class="sxs-lookup"><span data-stu-id="2df43-217">If a folder contains both \_MyPartial.cshtml and \_MyPartial.mobile.cshtml, the instruction @Html.Partial("\_MyPartial") will render \_MyPartial.mobile.cshtml during requests from mobile browsers, and \_MyPartial.cshtml during other requests.</span></span>

<span data-ttu-id="2df43-218">新しい登録することがより特定のビュー、レイアウト、またはその他のデバイス部分ビューを作成する場合は、 *DefaultDisplayMode*要求は、特定の条件を満たすときに検索する名前を指定するインスタンス。</span><span class="sxs-lookup"><span data-stu-id="2df43-218">If you want to create more specific views, layouts, or partial views for other devices, you can register a new *DefaultDisplayMode* instance to specify which name to search for when a request satisfies particular conditions.</span></span> <span data-ttu-id="2df43-219">次のコードを追加するなど、*アプリケーション\_開始*Apple iPhone ブラウザー要求を作成するときに適用される表示モードとして文字列"iPhone"を登録する Global.asax ファイル内のメソッド。</span><span class="sxs-lookup"><span data-stu-id="2df43-219">For example, you could add the following code to the *Application\_Start* method in the Global.asax file to register the string "iPhone" as a display mode that applies when the Apple iPhone browser makes a request:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample5.cs)]

<span data-ttu-id="2df43-220">アプリケーションで、views \shared を使用して、Apple iPhone ブラウザー要求を作成するときにこのコードが実行された後、\\_Layout.iPhone.cshtml レイアウト (存在する場合)。</span><span class="sxs-lookup"><span data-stu-id="2df43-220">After this code runs, when an Apple iPhone browser makes a request, your application will use the Views\Shared\\_Layout.iPhone.cshtml layout (if it exists).</span></span>

<a id="_Toc303253811"></a>
### <a name="jquery-mobile-the-view-switcher-and-browser-overriding"></a><span data-ttu-id="2df43-221">jQuery Mobile、ビュー スイッチャー、およびブラウザーのオーバーライド</span><span class="sxs-lookup"><span data-stu-id="2df43-221">jQuery Mobile, the View Switcher, and Browser Overriding</span></span>

<span data-ttu-id="2df43-222">jQuery Mobile は、タッチに最適化された web UI を構築するためのオープン ソース ライブラリです。</span><span class="sxs-lookup"><span data-stu-id="2df43-222">jQuery Mobile is an open source library for building touch-optimized web UI.</span></span> <span data-ttu-id="2df43-223">ASP.NET MVC 4 アプリケーションで jQuery Mobile を使用する場合は、ダウンロードして開始するのに役立つ NuGet パッケージをインストールできます。</span><span class="sxs-lookup"><span data-stu-id="2df43-223">If you want to use jQuery Mobile with an ASP.NET MVC 4 application, you can download and install a NuGet package that helps you get started.</span></span> <span data-ttu-id="2df43-224">Visual Studio パッケージ マネージャー コンソールからインストールするには、次のコマンドを入力します。</span><span class="sxs-lookup"><span data-stu-id="2df43-224">To install it from the Visual Studio Package Manager Console, type the following command:</span></span>

[!code-powershell[Main](mvc4-beta-release-notes/samples/sample6.ps1)]

<span data-ttu-id="2df43-225">これには、jQuery Mobile、および、次のいくつかのヘルパー ファイルがインストールされます。</span><span class="sxs-lookup"><span data-stu-id="2df43-225">This installs jQuery Mobile and some helper files, including the following:</span></span>

- <span data-ttu-id="2df43-226">Views/shared/\_Layout.Mobile.cshtml、jQuery Mobile ベースのレイアウトであります。</span><span class="sxs-lookup"><span data-stu-id="2df43-226">Views/Shared/\_Layout.Mobile.cshtml, which is a jQuery Mobile-based layout.</span></span>
- <span data-ttu-id="2df43-227">ビュー/共有で構成されるビュー スイッチャーのコンポーネント/\_ViewSwitcher.cshtml 部分ビューと ViewSwitcherController.cs コント ローラー。</span><span class="sxs-lookup"><span data-stu-id="2df43-227">A view-switcher component, which consists of the Views/Shared/\_ViewSwitcher.cshtml partial view and the ViewSwitcherController.cs controller.</span></span>

<span data-ttu-id="2df43-228">パッケージをインストールした後は、モバイル ブラウザーを使用してアプリケーションを実行 (または、Firefox など、同等の[ユーザー エージェント切り替え](http://chrispederick.com/work/user-agent-switcher/)アドオン)。</span><span class="sxs-lookup"><span data-stu-id="2df43-228">After you install the package, run your application using a mobile browser (or equivalent, like the Firefox [User Agent Switcher](http://chrispederick.com/work/user-agent-switcher/) add-on).</span></span> <span data-ttu-id="2df43-229">JQuery モバイル レイアウトを処理するため、ページ検索は、まったく異なることがわかりますおよびスタイル設定します。</span><span class="sxs-lookup"><span data-stu-id="2df43-229">You'll see that your pages look quite different, because jQuery Mobile handles layout and styling.</span></span> <span data-ttu-id="2df43-230">これを活用するには、次の操作を行うことができます。</span><span class="sxs-lookup"><span data-stu-id="2df43-230">To take advantage of this, you can do the following:</span></span>

- <span data-ttu-id="2df43-231">モバイル専用ビューの上書きを作成する」の説明に従って[表示モード](#_Toc303253810)以前 (たとえば、モバイル ブラウザーの Views\Home\Index.cshtml をオーバーライドする Views\Home\Index.mobile.cshtml を作成します)。</span><span class="sxs-lookup"><span data-stu-id="2df43-231">Create mobile-specific view overrides as described under [Display Modes](#_Toc303253810) earlier (for example, create Views\Home\Index.mobile.cshtml to override Views\Home\Index.cshtml for mobile browsers).</span></span>
- <span data-ttu-id="2df43-232">読み取り、 [jQuery モバイル ドキュメント](http://jquerymobile.com/)モバイル ビューで、タッチに最適化された UI 要素を追加する方法の詳細を表示します。</span><span class="sxs-lookup"><span data-stu-id="2df43-232">Read the [jQuery Mobile documentation](http://jquerymobile.com/) to learn more about how to add touch-optimized UI elements in mobile views.</span></span>

<span data-ttu-id="2df43-233">モバイルに最適化された web ページ用の規則では、デスクトップ ビューなど、ページのデスクトップ バージョンに切り替えることができますサイトの完全なモードはテキストがリンクを追加します。</span><span class="sxs-lookup"><span data-stu-id="2df43-233">A convention for mobile-optimized web pages is to add a link whose text is something like Desktop view or Full site mode that lets users switch to a desktop version of the page.</span></span> <span data-ttu-id="2df43-234">JQuery.Mobile.MVC パッケージには、この目的のためのサンプル ビュー スイッチャーのコンポーネントが含まれています。</span><span class="sxs-lookup"><span data-stu-id="2df43-234">The jQuery.Mobile.MVC package includes a sample view-switcher component for this purpose.</span></span> <span data-ttu-id="2df43-235">Views \shared 既定で使用される\\_Layout.Mobile.cshtml ビュー、およびページがレンダリングされるようになります。</span><span class="sxs-lookup"><span data-stu-id="2df43-235">It's used in the default Views\Shared\\_Layout.Mobile.cshtml view, and it looks like this when the page is rendered:</span></span>

![](mvc4-beta-release-notes/_static/image5.png)

<span data-ttu-id="2df43-236">ユーザーは、リンクをクリックする、同じページのデスクトップ バージョンに切り替えられますしています。</span><span class="sxs-lookup"><span data-stu-id="2df43-236">If visitors click the link, they're switched to the desktop version of the same page.</span></span>

<span data-ttu-id="2df43-237">デスクトップ レイアウトには既定のビューの切り替え機能が含まれていないために、訪問者はモバイルのモードを取得する方法を必要はありません。</span><span class="sxs-lookup"><span data-stu-id="2df43-237">Because your desktop layout will not include a view switcher by default, visitors won't have a way to get to mobile mode.</span></span> <span data-ttu-id="2df43-238">これを有効にするには、次の参照を追加 *\_ViewSwitcher* 、デスクトップ レイアウトに内部でだけ、 *&lt;本文&gt;* 要素。</span><span class="sxs-lookup"><span data-stu-id="2df43-238">To enable this, add the following reference to *\_ViewSwitcher* to your desktop layout, just inside the *&lt;body&gt;* element:</span></span>

[!code-cshtml[Main](mvc4-beta-release-notes/samples/sample7.cshtml)]

<span data-ttu-id="2df43-239">ビュー スイッチャーは、ブラウザーのオーバーライドと呼ばれる新しい機能を使用します。</span><span class="sxs-lookup"><span data-stu-id="2df43-239">The view switcher uses a new feature called Browser Overriding.</span></span> <span data-ttu-id="2df43-240">この機能により、アプリケーションで送られてきますかのように要求を扱うよりも、別のブラウザー (ユーザー エージェント) から、あなたが実際にします。</span><span class="sxs-lookup"><span data-stu-id="2df43-240">This feature lets your application treat requests as if they were coming from a different browser (user agent) than the one they're actually from.</span></span> <span data-ttu-id="2df43-241">次の表では、ブラウザーのオーバーライドを提供する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="2df43-241">The following table lists the methods that Browser Overriding provides.</span></span>

| `HttpContext.SetOverriddenBrowser(userAgentString)` | <span data-ttu-id="2df43-242">指定されたユーザー エージェントを使用して、要求の実際のユーザー エージェント値をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="2df43-242">Overrides the request's actual user agent value using the specified user agent.</span></span> |
| --- | --- |
| `HttpContext.GetOverriddenUserAgent()` | <span data-ttu-id="2df43-243">オーバーライドが指定されていない場合は、要求のユーザー エージェントの上書き値、または、実際のユーザー エージェント文字列を返します。</span><span class="sxs-lookup"><span data-stu-id="2df43-243">Returns the request's user agent override value, or the actual user agent string if no override has been specified.</span></span> |
| `HttpContext.GetOverriddenBrowser()` | <span data-ttu-id="2df43-244">返します、 *HttpBrowserCapabilitiesBase*要求に設定されているユーザー エージェントに対応するインスタンス (実際またはオーバーライド) します。</span><span class="sxs-lookup"><span data-stu-id="2df43-244">Returns an *HttpBrowserCapabilitiesBase* instance that corresponds to the user agent currently set for the request (actual or overridden).</span></span> <span data-ttu-id="2df43-245">この値を使用して、プロパティを取得します。 *IsMobileDevice*します。</span><span class="sxs-lookup"><span data-stu-id="2df43-245">You can use this value to get properties such as *IsMobileDevice*.</span></span> |
| `HttpContext.ClearOverriddenBrowser()` | <span data-ttu-id="2df43-246">現在の要求でオーバーライドされたユーザー エージェントを削除します。</span><span class="sxs-lookup"><span data-stu-id="2df43-246">Removes any overridden user agent for the current request.</span></span> |

<span data-ttu-id="2df43-247">ブラウザー オーバーライドは ASP.NET MVC 4 のコア機能でありは、jQuery.Mobile.MVC パッケージをインストールしていない場合でも使用できます。</span><span class="sxs-lookup"><span data-stu-id="2df43-247">Browser Overriding is a core feature of ASP.NET MVC 4 and is available even if you don't install the jQuery.Mobile.MVC package.</span></span> <span data-ttu-id="2df43-248">影響ただし、ビュー、レイアウト、および部分ビューの選択-その他の ASP.NET 機能に依存するのには影響しません、 *Request.Browser*オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="2df43-248">However, it affects only view, layout, and partial-view selection — it does not affect any other ASP.NET feature that depends on the *Request.Browser* object.</span></span>

<span data-ttu-id="2df43-249">既定では、cookie を使用して、ユーザー エージェントの上書きが格納されます。</span><span class="sxs-lookup"><span data-stu-id="2df43-249">By default, the user-agent override is stored using a cookie.</span></span> <span data-ttu-id="2df43-250">既定のプロバイダーを置き換えることができます (たとえば、データベース) に別の場所の上書きを保存する場合は、(*BrowserOverrideStores.Current*)。</span><span class="sxs-lookup"><span data-stu-id="2df43-250">If you want to store the override elsewhere (for example, in a database), you can replace the default provider (*BrowserOverrideStores.Current*).</span></span> <span data-ttu-id="2df43-251">このプロバイダーのドキュメントは、ASP.NET MVC の今後のリリースと共に使用できるになります。</span><span class="sxs-lookup"><span data-stu-id="2df43-251">Documentation for this provider will be available to accompany a later release of ASP.NET MVC.</span></span>

<a id="_Toc303253812"></a>
### <a name="recipes-for-code-generation-in-visual-studio"></a><span data-ttu-id="2df43-252">Visual Studio でのコード生成のレシピ</span><span class="sxs-lookup"><span data-stu-id="2df43-252">Recipes for Code Generation in Visual Studio</span></span>

<span data-ttu-id="2df43-253">新しいレシピ機能は、NuGet を使用してをインストールするパッケージに基づくソリューションに固有のコードを生成する Visual Studio を使用できます。</span><span class="sxs-lookup"><span data-stu-id="2df43-253">The new Recipes feature enables Visual Studio to generate solution-specific code based on packages that you can install using NuGet.</span></span> <span data-ttu-id="2df43-254">レシピのフレームワークを簡単に書き込むコード生成のプラグインは、領域の追加、コント ローラーの追加、およびビューの追加の組み込みコード ジェネレーターを置換する使用することもできます。 開発者向け。</span><span class="sxs-lookup"><span data-stu-id="2df43-254">The Recipes framework makes it easy for developers to write code-generation plugins, which you can also use to replace the built-in code generators for Add Area, Add Controller, and Add View.</span></span> <span data-ttu-id="2df43-255">レシピは NuGet パッケージとして展開するため、簡単にソース管理にチェックインでき自動的にプロジェクトのすべての開発者と共有します。</span><span class="sxs-lookup"><span data-stu-id="2df43-255">Because recipes are deployed as NuGet packages, they can easily be checked into source control and shared with all developers on the project automatically.</span></span> <span data-ttu-id="2df43-256">これらは、ソリューションごとに入手できます。</span><span class="sxs-lookup"><span data-stu-id="2df43-256">They are also available on a per-solution basis.</span></span>

<a id="_Toc303253813"></a>
### <a name="task-support-for-asynchronous-controllers"></a><span data-ttu-id="2df43-257">非同期コント ローラーのタスクのサポート</span><span class="sxs-lookup"><span data-stu-id="2df43-257">Task Support for Asynchronous Controllers</span></span>

<span data-ttu-id="2df43-258">これで記述できる非同期アクション メソッドを型のオブジェクトを返す 1 つのメソッド*タスク*または*タスク&lt;ActionResult&gt;* します。</span><span class="sxs-lookup"><span data-stu-id="2df43-258">You can now write asynchronous action methods as single methods that return an object of type *Task* or *Task&lt;ActionResult&gt;*.</span></span>

<span data-ttu-id="2df43-259">たとえば、Visual c# 5 を使用している場合 (またはを使用して、 [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx))、次のような非同期アクション メソッドを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="2df43-259">For example, if you're using Visual C# 5 (or using the [Async CTP](https://msdn.microsoft.com/vstudio/async.aspx)), you can create an asynchronous action method that looks like the following:</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample8.cs)]

<span data-ttu-id="2df43-260">前のアクション メソッドへの呼び出しで*newsService.GetHeadlinesAsync*と*sportsService.GetScoresAsync*非同期に呼び出され、スレッド プールからスレッドをブロックされません。</span><span class="sxs-lookup"><span data-stu-id="2df43-260">In the previous action method, the calls to *newsService.GetHeadlinesAsync* and *sportsService.GetScoresAsync* are called asynchronously and do not block a thread from the thread pool.</span></span>

<span data-ttu-id="2df43-261">非同期アクション メソッドが返す*タスク*インスタンスは、タイムアウトもサポートできます。</span><span class="sxs-lookup"><span data-stu-id="2df43-261">Asynchronous action methods that return *Task* instances can also support timeouts.</span></span> <span data-ttu-id="2df43-262">アクション メソッドをキャンセルするためには、型のパラメーターを追加*CancellationToken*アクション メソッドのシグネチャにします。</span><span class="sxs-lookup"><span data-stu-id="2df43-262">To make your action method cancellable, add a parameter of type *CancellationToken* to the action method signature.</span></span> <span data-ttu-id="2df43-263">次の例では、2500 時間をミリ秒単位のタイムアウトを持つし、表示する非同期アクション メソッドを*TimedOut*タイムアウトが発生した場合、クライアントに表示します。</span><span class="sxs-lookup"><span data-stu-id="2df43-263">The following example shows an asynchronous action method that has a timeout of 2500 milliseconds and that displays a *TimedOut* view to the client if a timeout occurs.</span></span>

[!code-csharp[Main](mvc4-beta-release-notes/samples/sample9.cs)]

<a id="_Toc303253814"></a>
### <a name="azure-sdk"></a><span data-ttu-id="2df43-264">Azure SDK</span><span class="sxs-lookup"><span data-stu-id="2df43-264">Azure SDK</span></span>

<span data-ttu-id="2df43-265">ASP.NET MVC 4 Beta には、Windows Azure SDK の 2011 年 9 月 1.5 のリリースがサポートしています。</span><span class="sxs-lookup"><span data-stu-id="2df43-265">ASP.NET MVC 4 Beta supports the September 2011 1.5 release of the Windows Azure SDK.</span></span>

<a id="_Toc303253815"></a>
## <a name="known-issues-and-breaking-changes"></a><span data-ttu-id="2df43-266">既知の問題と重大な変更</span><span class="sxs-lookup"><span data-stu-id="2df43-266">Known Issues and Breaking Changes</span></span>

- <span data-ttu-id="2df43-267">**ASP.NET MVC 4 Beta をインストールした後、CSHTML と VBHTML エディターが Visual Studio 2010 Service Pack 1 CSHTML と VBHTML エディターでは可能性があります cshtml"や"vbhtml ファイル内で作成するか、JavaScript を入力した後に長時間一時停止します。**</span><span class="sxs-lookup"><span data-stu-id="2df43-267">**After installing ASP.NET MVC 4 Beta, the CSHTML/VBHTML editor in Visual Studio 2010 Service Pack 1 CSHTML/VBHTML editor may pause for a long time after typing snippet or JavaScript inside cshtml or vbhtml files.**</span></span> <span data-ttu-id="2df43-268">これは、作成された直後、まだコンパイルされていないと ASP.NET MVC 4 アプリケーションでのみ発生します。</span><span class="sxs-lookup"><span data-stu-id="2df43-268">This occurs only in ASP.NET MVC 4 applications which have just been created and have not yet been compiled.</span></span>

    <span data-ttu-id="2df43-269">回避策では、bin フォルダーにアセンブリを取得するプロジェクトをコンパイルします。</span><span class="sxs-lookup"><span data-stu-id="2df43-269">The workaround is to compile the project to get the assemblies in the bin folder.</span></span> <span data-ttu-id="2df43-270">エディターの問題が返される、アセンブリを bin フォルダーから削除され、プロジェクトをクリーンアップする場合に注意してください。</span><span class="sxs-lookup"><span data-stu-id="2df43-270">Note, if you clean the project which removes the assemblies from the bin folder, the editor problem will come back.</span></span>

    <span data-ttu-id="2df43-271">これは、次のリリースで修正される予定です。</span><span class="sxs-lookup"><span data-stu-id="2df43-271">This will be corrected in the next release.</span></span>
- <span data-ttu-id="2df43-272">**Visual Studio 11 Beta 用の c# プロジェクト テンプレートには、global.asax.cs、不適切な接続文字列が含まれます。**</span><span class="sxs-lookup"><span data-stu-id="2df43-272">**C# Project templates for Visual Studio 11 Beta contain an incorrect connection string in Global.asax.cs.**</span></span> <span data-ttu-id="2df43-273">アプリケーションで指定された既定の接続\_Start メソッドを Visual Studio 11 Beta で作成されたプロジェクトにエスケープされていない円記号を含む LocalDB 接続文字列が含まれています (\)文字。</span><span class="sxs-lookup"><span data-stu-id="2df43-273">The default connection specified in the Application\_Start method for projects created in Visual Studio 11 Beta contain a LocalDB connection string which contains an unescaped backslash (\) character.</span></span> <span data-ttu-id="2df43-274">これは、結果、接続エラー、SqlException を生成する Entity Framework の DbContext にアクセスしようとします。</span><span class="sxs-lookup"><span data-stu-id="2df43-274">This results in a connection error upon attempts to access a Entity Framework DbContext, which generates a SqlException.</span></span>

    <span data-ttu-id="2df43-275">この問題を修正するには、アプリでバック スラッシュ文字をエスケープ\_Global.asax.cs のメソッドを起動して、そこに次のようにします。</span><span class="sxs-lookup"><span data-stu-id="2df43-275">To correct this issue, escape the backslash character in the App\_Start method of Global.asax.cs so that it reads as follows:</span></span>

    [!code-csharp[Main](mvc4-beta-release-notes/samples/sample10.cs)]
- <span data-ttu-id="2df43-276">**.NET 4.5 を対象とする ASP.NET MVC 4 アプリケーションが、FileLoadException System.Net.Http.dll アセンブリが .NET 4.0 で実行する場合にアクセスしようとするとスローされます。**</span><span class="sxs-lookup"><span data-stu-id="2df43-276">**ASP.NET MVC 4 applications which target .NET 4.5 will throw a FileLoadException upon attempt to access the System.Net.Http.dll assembly when run under .NET 4.0.**</span></span> <span data-ttu-id="2df43-277">.NET 4.5 で作成された ASP.NET MVC 4 アプリケーションには、バインドが含まれているという FileLoadException 原因となるリダイレクト「を読み込めませんでしたファイルまたはアセンブリ 'System.Net.Http' またはその依存関係の 1 つ」。</span><span class="sxs-lookup"><span data-stu-id="2df43-277">ASP.NET MVC 4 applications created under .NET 4.5 contain a binding redirect that will result in a FileLoadException which that states "Could not load file or assembly 'System.Net.Http' or one of its dependencies."</span></span> <span data-ttu-id="2df43-278">ときに、アプリケーションは、.NET 4.0 がインストールされたシステムで実行されます。</span><span class="sxs-lookup"><span data-stu-id="2df43-278">when the application is executed on a system with .NET 4.0 installed.</span></span> <span data-ttu-id="2df43-279">この問題を修正するには、web.config ファイルから次のバインド リダイレクトを削除します。</span><span class="sxs-lookup"><span data-stu-id="2df43-279">To correct this issue, remove the following binding redirect from web.config:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample11.xml)]

    <span data-ttu-id="2df43-280">変更した web.config 内のアセンブリのバインド要素には、次のように表示されます。</span><span class="sxs-lookup"><span data-stu-id="2df43-280">The assembly binding element in the modified web.config should appear as follows:</span></span>

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample12.xml)]
- <span data-ttu-id="2df43-281"><strong>Visual Basic プロジェクトで「コント ローラーの追加」の項目テンプレートが、正しくない名前空間が呼び出されたときに生成されます</strong><strong>から領域内で。</strong></span><span class="sxs-lookup"><span data-stu-id="2df43-281"><strong>The "Add Controller" item template in Visual Basic projects generates an incorrect namespace when invoked</strong><strong>from inside an area.</strong></span></span> <span data-ttu-id="2df43-282">Visual Basic を使用する ASP.NET MVC プロジェクト内の領域に、コント ローラーを追加すると、項目テンプレートは、コント ローラーに間違った名前空間を挿入します。</span><span class="sxs-lookup"><span data-stu-id="2df43-282">When you add a controller to an area in an ASP.NET MVC project that uses Visual Basic, the item template inserts the wrong namespace into the controller.</span></span> <span data-ttu-id="2df43-283">コント ローラーでのアクションに移動すると、「ファイルが見つかりません」エラーになります。</span><span class="sxs-lookup"><span data-stu-id="2df43-283">The result is a "file not found" error when you navigate to any action in the controller.</span></span>  
  
  <span data-ttu-id="2df43-284">生成された名前空間は、ルート名前空間の後にすべてを省略します。</span><span class="sxs-lookup"><span data-stu-id="2df43-284">The generated namespace omits everything after the root namespace.</span></span> <span data-ttu-id="2df43-285">たとえば、生成された名前空間は*RootNamespace*する必要がありますが、 *RootNamespace.Areas.AreaName.Controllers*します。</span><span class="sxs-lookup"><span data-stu-id="2df43-285">For example, the namespace generated is *RootNamespace* but should be *RootNamespace.Areas.AreaName.Controllers* .</span></span>
- <span data-ttu-id="2df43-286">**Razor ビュー エンジンにおける重大な変更。**</span><span class="sxs-lookup"><span data-stu-id="2df43-286">**Breaking changes in the Razor View Engine.**</span></span> <span data-ttu-id="2df43-287">Razor パーサーの書き換えの一環として、次の種類はから削除された*System.Web.Mvc.Razor*:</span><span class="sxs-lookup"><span data-stu-id="2df43-287">As part of a rewrite of the Razor parser, the following types were removed from *System.Web.Mvc.Razor*:</span></span> 

    - <span data-ttu-id="2df43-288">*ModelSpan*</span><span class="sxs-lookup"><span data-stu-id="2df43-288">*ModelSpan*</span></span>
    - <span data-ttu-id="2df43-289">*MvcVBRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="2df43-289">*MvcVBRazorCodeGenerator*</span></span>
    - <span data-ttu-id="2df43-290">*MvcCSharpRazorCodeGenerator*</span><span class="sxs-lookup"><span data-stu-id="2df43-290">*MvcCSharpRazorCodeGenerator*</span></span>
    - <span data-ttu-id="2df43-291">*MvcVBRazorCodeParser*</span><span class="sxs-lookup"><span data-stu-id="2df43-291">*MvcVBRazorCodeParser*</span></span>

  <span data-ttu-id="2df43-292">次のメソッドも削除されます。</span><span class="sxs-lookup"><span data-stu-id="2df43-292">The following methods were also removed:</span></span> 

    - <span data-ttu-id="2df43-293">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="2df43-293">*MvcCSharpRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
    - <span data-ttu-id="2df43-294">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span><span class="sxs-lookup"><span data-stu-id="2df43-294">*MvcWebPageRazorHost.DecorateCodeGenerator(System.Web.Razor.Generator.RazorCodeGenerator)*</span></span>
    - <span data-ttu-id="2df43-295">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span><span class="sxs-lookup"><span data-stu-id="2df43-295">*MvcVBRazorCodeParser.ParseInheritsStatement(System.Web.Razor.Parser.CodeBlockInfo)*</span></span>
- <span data-ttu-id="2df43-296">**WebMatrix.WebData.dll が、ASP.NET MVC 4 アプリケーションの/bin ディレクトリに含まれる、フォーム認証の URL にかかります。**</span><span class="sxs-lookup"><span data-stu-id="2df43-296">**When WebMatrix.WebData.dll is included in the /bin directory of an ASP.NET MVC 4 apps, it takes over the URL for forms authentication.**</span></span> <span data-ttu-id="2df43-297">ログオン/アカウントへのリダイレクトを認証ログインが上書きされます (たとえば、配置可能な依存関係の追加 ダイアログを使用する場合は、「Razor 構文で ASP.NET Web ページ」を選択) して WebMatrix.WebData.dll アセンブリをアプリケーションに追加するのではなく/既定の ASP.NET MVC アカウント コント ローラーで期待どおりに、アカウント/ログインします。</span><span class="sxs-lookup"><span data-stu-id="2df43-297">Adding the WebMatrix.WebData.dll assembly to your application (for example, by selecting "ASP.NET Web Pages with Razor Syntax" when using the Add Deployable Dependencies dialog) will override the authentication login redirect to /account/logon rather than /account/login as expected by the default ASP.NET MVC Account Controller.</span></span> <span data-ttu-id="2df43-298">この動作を回避し、web.config ファイルの [認証] セクションで既に指定された URL を使用して、PreserveLoginUrl と呼ばれる、appSetting を追加および true に設定できます。</span><span class="sxs-lookup"><span data-stu-id="2df43-298">To prevent this behavior and use the URL specified already in the authentication section of web.config, you can add an appSetting called PreserveLoginUrl and set it to true:</span></span> 

    [!code-xml[Main](mvc4-beta-release-notes/samples/sample13.xml)]
- <span data-ttu-id="2df43-299">**NuGet パッケージ マネージャーは、Visual Studio 2010 および Visual Web Developer 2010 のサイド バイ サイド インストールの ASP.NET MVC 4 をインストールするときのインストールに失敗します。**</span><span class="sxs-lookup"><span data-stu-id="2df43-299">**The NuGet package manager fails to install when attempting to install ASP.NET MVC 4 for side by side installations of Visual Studio 2010 and Visual Web Developer 2010.**</span></span> <span data-ttu-id="2df43-300">Visual Studio 2010 と ASP.NET MVC 4 と並んで、Visual Web Developer 2010 を実行するには、両方のバージョンの Visual Studio を既にインストールした後、ASP.NET MVC 4 をインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-300">To run Visual Studio 2010 and Visual Web Developer 2010 side by side with ASP.NET MVC 4 you must install ASP.NET MVC 4 after both versions of Visual Studio have already been installed.</span></span>
- <span data-ttu-id="2df43-301">**前提条件が既にアンインストールされている場合、ASP.NET MVC 4 のアンインストールが失敗します。**</span><span class="sxs-lookup"><span data-stu-id="2df43-301">**Uninstalling ASP.NET MVC 4 fails if prerequisites have already been uninstalled.**</span></span> <span data-ttu-id="2df43-302">ASP.NET MVC を完全にアンインストールするには、4you は Visual Studio をアンインストールする前に、ASP.NET MVC 4 をアンインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-302">To cleanly uninstall ASP.NET MVC 4you must uninstall ASP.NET MVC 4 prior to uninstalling Visual Studio.</span></span>
- <span data-ttu-id="2df43-303">**既定の Web API プロジェクトを実行すると、存在しない RegisterApis メソッドを使用してルートを追加するユーザーを正しく指示する命令が表示されます。**</span><span class="sxs-lookup"><span data-stu-id="2df43-303">**Running a default Web API project shows instructions that incorrectly direct the user to add routes using the RegisterApis method, which doesn't exist.**</span></span> <span data-ttu-id="2df43-304">ASP.NET のルート テーブルを使用して、RegisterRoutes メソッドでは、ルートを追加する必要があります。</span><span class="sxs-lookup"><span data-stu-id="2df43-304">Routes should be added in the RegisterRoutes method using the ASP.NET route table.</span></span>
- <span data-ttu-id="2df43-305">**ASP.NET MVC 4 Beta をインストールするには、ASP.NET MVC 3 RTM アプリケーションが中断されます。**</span><span class="sxs-lookup"><span data-stu-id="2df43-305">**Installing ASP.NET MVC 4 Beta breaks ASP.NET MVC 3 RTM applications.**</span></span> <span data-ttu-id="2df43-306">作成された ASP.NET MVC 3 アプリケーション (ASP.NET MVC 3 Tools Update リリース) ではなくリリースは RTM で、ASP.NET MVC 4 Beta と並行して動作させるには、次の変更を必要とします。</span><span class="sxs-lookup"><span data-stu-id="2df43-306">ASP.NET MVC 3 applications that were created with the RTM release (not with the ASP.NET MVC 3 Tools Update release) require the following changes in order to work side-by-side with ASP.NET MVC 4 Beta.</span></span> <span data-ttu-id="2df43-307">これらの更新プログラムの結果にコンパイル エラーが発生せず、プロジェクトをビルドします。</span><span class="sxs-lookup"><span data-stu-id="2df43-307">Building the project without making these updates results in compilation errors.</span></span> 

    <span data-ttu-id="2df43-308">**必要な更新プログラム**</span><span class="sxs-lookup"><span data-stu-id="2df43-308">**Required updates**</span></span>

  1. <span data-ttu-id="2df43-309">ルートの Web.config ファイルでは、新しい追加*&lt;appSettings&gt;* キーを持つエントリ*webPages:Version*値*1.0.0.0*します。</span><span class="sxs-lookup"><span data-stu-id="2df43-309">In the root Web.config file, add a new *&lt;appSettings&gt;* entry with the key *webPages:Version* and the value *1.0.0.0*.</span></span>

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample14.xml)]
  2. <span data-ttu-id="2df43-310">ソリューション エクスプ ローラーでプロジェクト名を右クリックし、プロジェクトのアンロードを選択します。</span><span class="sxs-lookup"><span data-stu-id="2df43-310">In Solution Explorer, right-click the project name and then select Unload Project.</span></span> <span data-ttu-id="2df43-311">名前をもう一度右クリックし、編集*ProjectName*.csproj します。</span><span class="sxs-lookup"><span data-stu-id="2df43-311">Then right-click the name again and select Edit *ProjectName*.csproj.</span></span>
  3. <span data-ttu-id="2df43-312">次のアセンブリ参照を見つけます。</span><span class="sxs-lookup"><span data-stu-id="2df43-312">Locate the following assembly references:</span></span> 

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample15.xml)]

      <span data-ttu-id="2df43-313">次のように、それらを置き換えます。</span><span class="sxs-lookup"><span data-stu-id="2df43-313">Replace them with the following:</span></span>

      [!code-xml[Main](mvc4-beta-release-notes/samples/sample16.xml)]
  4. <span data-ttu-id="2df43-314">変更を保存、編集していたし、プロジェクトを右クリックして選択して再読み込み、プロジェクト (.csproj) ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="2df43-314">Save the changes, close the project (.csproj) file you were editing, and then right-click the project and select Reload.</span></span>
