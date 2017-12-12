---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: "ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションのアップグレード |Microsoft ドキュメント"
author: rick-anderson
description: "このドキュメントでは、両方について説明し、ウィザードを使用して、手動で ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションをアップグレードする方法です。 このドキュメントも d を使用しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a><span data-ttu-id="16fc2-104">ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションのアップグレード</span><span class="sxs-lookup"><span data-stu-id="16fc2-104">Upgrading an ASP.NET MVC 1.0 Application to ASP.NET MVC 2</span></span>
====================
> <span data-ttu-id="16fc2-105">このドキュメントでは、両方について説明し、ウィザードを使用して、手動で ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションをアップグレードする方法です。</span><span class="sxs-lookup"><span data-stu-id="16fc2-105">This document describes both how to upgrade manually and with a wizard an ASP.NET MVC 1.0 Application to ASP.NET MVC 2.</span></span> <span data-ttu-id="16fc2-106">このドキュメントも[ダウンロード](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span><span class="sxs-lookup"><span data-stu-id="16fc2-106">This document is also available for [Download](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)</span></span>


## <a name="introduction"></a><span data-ttu-id="16fc2-107">はじめに</span><span class="sxs-lookup"><span data-stu-id="16fc2-107">Introduction</span></span>

<span data-ttu-id="16fc2-108">ASP.NET MVC 2 は、同じサーバー上の ASP.NET MVC 1.0 サイド バイ サイドでインストールできます。</span><span class="sxs-lookup"><span data-stu-id="16fc2-108">ASP.NET MVC 2 can be installed side by side with ASP.NET MVC 1.0 on the same server.</span></span> <span data-ttu-id="16fc2-109">これにより、アプリケーション開発者柔軟に ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションをアップグレードするタイミングを選択します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-109">This gives application developers flexibility in choosing when to upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2.</span></span>

<span data-ttu-id="16fc2-110">Visual Studio 2010 には、ASP.NET MVC 2 を Visual Studio 2008 でビルドされた既存の ASP.NET MVC 1.0 プロジェクトをアップグレードにはウィザードが含まれています。</span><span class="sxs-lookup"><span data-stu-id="16fc2-110">Visual Studio 2010 includes a wizard that upgrades existing ASP.NET MVC 1.0 projects built with Visual Studio 2008 to ASP.NET MVC 2.</span></span> <span data-ttu-id="16fc2-111">アップグレード ウィザードは、Visual Studio 2010 の ASP.NET MVC 1.0 プロジェクトを開くことによって開始されます。</span><span class="sxs-lookup"><span data-stu-id="16fc2-111">The upgrade wizard is initiated by opening an ASP.NET MVC 1.0 project in Visual Studio 2010.</span></span>

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a><span data-ttu-id="16fc2-112">Visual Studio 2008 SP1 での ASP.NET MVC 1.0 用のウィザードをアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="16fc2-112">Upgrade Wizard for ASP.NET MVC 1.0 on Visual Studio 2008 SP1</span></span>

<span data-ttu-id="16fc2-113">Visual Studio 2008 SP1 での ASP.NET MVC 2 に ASP.NET MVC 1.0 アプリケーションをアップグレードするには、(サポートされていない) MvcAppConverter アプリケーションを使用します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-113">To upgrade an ASP.NET MVC 1.0 application to ASP.NET MVC 2 in Visual Studio 2008 SP1, use the (unsupported) MvcAppConverter application.</span></span> <span data-ttu-id="16fc2-114">このアプリケーションは、次の URL からダウンロードできます。</span><span class="sxs-lookup"><span data-stu-id="16fc2-114">You can download this application from the following URL:</span></span>

[<span data-ttu-id="16fc2-115">https://go.microsoft.com/fwlink/?LinkID=185351</span><span class="sxs-lookup"><span data-stu-id="16fc2-115">https://go.microsoft.com/fwlink/?LinkID=185351</span></span>](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a><span data-ttu-id="16fc2-116">ASP.NET MVC 1.0 プロジェクトを手動でアップグレードします。</span><span class="sxs-lookup"><span data-stu-id="16fc2-116">Manually Upgrading an ASP.NET MVC 1.0 Project</span></span>

<span data-ttu-id="16fc2-117">バージョン 2 に既存の ASP.NET MVC 1.0 アプリケーションを手動でアップグレードするには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="16fc2-117">To manually upgrade an existing ASP.NET MVC 1.0 application to version 2, follow these steps:</span></span>

1. <span data-ttu-id="16fc2-118">既存のプロジェクトのバックアップを作成します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-118">Make a backup of the existing project.</span></span>
2. <span data-ttu-id="16fc2-119">テキスト エディターで、プロジェクト ファイル (.csproj または .vbproj ファイル拡張子を持つファイル) を開き ProjectTypeGuid 要素を検索します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-119">In a text editor, open the project file (the file with the .csproj or .vbproj file extension) and find the ProjectTypeGuid element.</span></span> <span data-ttu-id="16fc2-120">、その要素の値として、GUID {603c0e0b-db56-11dc-be95-000d561079b0} {f85e285d-a4e0-4152-9332-ab1d724d3325} を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16fc2-120">As the value of that element, replace the GUID {603c0e0b-db56-11dc-be95-000d561079b0} with {F85E285D-A4E0-4152-9332-AB1D724D3325}.</span></span> <span data-ttu-id="16fc2-121">その要素の値が次のようにする必要があります。</span><span class="sxs-lookup"><span data-stu-id="16fc2-121">When you are done, the value of that element should be as follows:</span></span> 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. <span data-ttu-id="16fc2-122">Web アプリケーションのルート フォルダーには、Web.config ファイルを編集します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-122">In the Web application root folder, edit the Web.config file.</span></span> <span data-ttu-id="16fc2-123">System.Web.Mvc、バージョンの検索 1.0.0.0 を = し、すべてのインスタンスを置き換えます System.Web.Mvc、バージョン 2.0.0.0 を = です。</span><span class="sxs-lookup"><span data-stu-id="16fc2-123">Search for System.Web.Mvc, Version=1.0.0.0 and replace all instances with System.Web.Mvc, Version=2.0.0.0.</span></span>
4. <span data-ttu-id="16fc2-124">Views フォルダーにある Web.config ファイルを前の手順を繰り返します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-124">Repeat the previous step for the Web.config file located in the Views folder.</span></span>
5. <span data-ttu-id="16fc2-125">Visual Studio を使用してプロジェクトを開くと**ソリューション エクスプ ローラー**、展開、**参照**ノード。</span><span class="sxs-lookup"><span data-stu-id="16fc2-125">Open the project using Visual Studio, and in **Solution Explorer**, expand the **References** node.</span></span> <span data-ttu-id="16fc2-126">System.Web.Mvc (を version 1.0 のアセンブリを指す) への参照を削除します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-126">Delete the reference to System.Web.Mvc (which points to the version 1.0 assembly).</span></span> <span data-ttu-id="16fc2-127">System.Web.Mvc (v2.0.0.0) への参照を追加します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-127">Add a reference to System.Web.Mvc (v2.0.0.0).</span></span>
6. <span data-ttu-id="16fc2-128">構成セクションの下のアプリケーション ルートの Web.config ファイルに次の bindingRedirect 要素を追加します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-128">Add the following bindingRedirect element to the Web.config file in the application root under the configuraton section:</span></span>   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. <span data-ttu-id="16fc2-129">新しい空の ASP.NET MVC 2 アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-129">Create a new empty ASP.NET MVC 2 application.</span></span> <span data-ttu-id="16fc2-130">既存のアプリケーションの Scripts フォルダーに新しいアプリケーションの Scripts フォルダーからファイルをコピーします。</span><span class="sxs-lookup"><span data-stu-id="16fc2-130">Copy the files from the Scripts folder of the new application into the Scripts folder of the existing application.</span></span>
8. <span data-ttu-id="16fc2-131">更新の既存のアプリケーションの â €™ Site.css ファイルで CSS スタイル定義を含む s CSS ファイル。</span><span class="sxs-lookup"><span data-stu-id="16fc2-131">Update the existing applicationâ€™s CSS file with the CSS style definitions in the Site.css file.</span></span>
9. <span data-ttu-id="16fc2-132">アプリケーションをコンパイルして実行します。</span><span class="sxs-lookup"><span data-stu-id="16fc2-132">Compile the application and run it.</span></span> <span data-ttu-id="16fc2-133">エラーが発生した場合の重大な変更」セクションを参照してください。、 [ASP.NET MVC 2 の新](https://go.microsoft.com/fwlink/?LinkID=185038)ページ。</span><span class="sxs-lookup"><span data-stu-id="16fc2-133">If any errors occur, refer to the Breaking Changes section of the [What's New in ASP.NET MVC 2](https://go.microsoft.com/fwlink/?LinkID=185038) page.</span></span>
