---
uid: mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
title: "この章のビルドのダウンロードを EF 5 MVC 4 のチュートリアル |Microsoft ドキュメント"
author: Rick-Anderson
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio を使用して ASP.NET MVC 4 アプリケーションを作成する方法について説明しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/30/2013
ms.topic: article
ms.assetid: d0a89089-eed8-4f61-a478-c5ffa30186f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-ef-5-using-mvc-4/building-the-ef5-mvc4-chapter-downloads
msc.type: authoredcontent
ms.openlocfilehash: 912a1383ed170b49782657372abc1801140df8dd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="building-the-chapter-downloads-for-the-ef-5-mvc-4-tutorials"></a><span data-ttu-id="0321f-103">この章のビルドのダウンロードを EF 5 MVC 4 のチュートリアル</span><span class="sxs-lookup"><span data-stu-id="0321f-103">Building the Chapter Downloads for the EF 5 MVC 4 Tutorials</span></span>
====================
<span data-ttu-id="0321f-104">によって[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0321f-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

[<span data-ttu-id="0321f-105">完成したプロジェクトをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="0321f-105">Download Completed Project</span></span>](http://code.msdn.microsoft.com/Getting-Started-with-dd0e2ed8)

> <span data-ttu-id="0321f-106">Contoso 大学でサンプル web アプリケーションでは、Entity Framework 5 Code First と Visual Studio 2012 を使用して ASP.NET MVC 4 アプリケーションを作成する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="0321f-106">The Contoso University sample web application demonstrates how to create ASP.NET MVC 4 applications using the Entity Framework 5 Code First and Visual Studio 2012.</span></span> <span data-ttu-id="0321f-107">一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)です。</span><span class="sxs-lookup"><span data-stu-id="0321f-107">For information about the tutorial series, see [the first tutorial in the series](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md).</span></span>


## <a name="building-the-chapter-downloads"></a><span data-ttu-id="0321f-108">章のダウンロードの構築</span><span class="sxs-lookup"><span data-stu-id="0321f-108">Building the Chapter Downloads</span></span>

1. <span data-ttu-id="0321f-109">ダウンロードし、プロジェクトのサンプルの zip ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="0321f-109">Download and unzip the  project sample zip file.</span></span> <span data-ttu-id="0321f-110">解凍したダウンロード パッケージでは、各章の完了のいずれかの追加の zip ファイルが見つかります。</span><span class="sxs-lookup"><span data-stu-id="0321f-110">In the unzipped download package, you will find additional zip files, one for the completion of each chapter.</span></span>
2. <span data-ttu-id="0321f-111">目的の zip ファイルを右クリックし、をクリックして**プロパティ**、 をクリックし、**ブロックを解除する**ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="0321f-111">Right click on the desired zip file, click **Properties**, and click the **Unblock** button.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image1.png)
3. <span data-ttu-id="0321f-112">ファイルを解凍します。</span><span class="sxs-lookup"><span data-stu-id="0321f-112">Unzip the file.</span></span>
4. <span data-ttu-id="0321f-113">ダブルクリックして、 *CUx.sln*ファイルを Visual Studio を起動します。</span><span class="sxs-lookup"><span data-stu-id="0321f-113">Double-click the *CUx.sln* file to launch Visual Studio.</span></span>
5. <span data-ttu-id="0321f-114">**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**、し**Package Manager Console**です。</span><span class="sxs-lookup"><span data-stu-id="0321f-114">From the **Tools** menu, click **Library Package Manager**, then **Package Manager Console**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image2.png)
6. <span data-ttu-id="0321f-115">パッケージ マネージャー コンソール (PMC) をクリックして**復元**です。</span><span class="sxs-lookup"><span data-stu-id="0321f-115">In the Package Manager Console (PMC), click **Restore**.</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image3.png)
7. <span data-ttu-id="0321f-116">Visual Studio を終了します。</span><span class="sxs-lookup"><span data-stu-id="0321f-116">Exit Visual Studio.</span></span>
8. <span data-ttu-id="0321f-117">上記の手順で閉じられたか、ソリューション ファイルを開いて、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="0321f-117">Restart Visual Studio, opening the solution file you closed in the step above.</span></span>
9. <span data-ttu-id="0321f-118">パッケージ マネージャー コンソール (PMC) を入力してください、`Update-Database`コマンド。</span><span class="sxs-lookup"><span data-stu-id="0321f-118">In the Package Manager Console (PMC), enter the `Update-Database` command:</span></span>  
  
    ![](building-the-ef5-mvc4-chapter-downloads/_static/image4.png)  

    > [!NOTE]
    > <span data-ttu-id="0321f-119">: 次のエラーが発生した場合</span><span class="sxs-lookup"><span data-stu-id="0321f-119">If you get the following error:</span></span>  
    >   
    >  <span data-ttu-id="0321f-120">*用語 'データベースの更新' は、コマンドレット、関数、スクリプト ファイル、または操作可能なプログラムの名前としては認識されません。名のスペルを確認するかパスが含まれている場合、パスが正しいことを確認し、もう一度やり直してください。*</span><span class="sxs-lookup"><span data-stu-id="0321f-120">*The term 'Update-Database' is not recognized as the name of a cmdlet, function, script file, or operable program. Check the spelling of the name, or if a path was included, verify that the path is correct and try again.*</span></span>  
    > <span data-ttu-id="0321f-121">終了し、Visual Studio を再起動します。</span><span class="sxs-lookup"><span data-stu-id="0321f-121">Exit and restart Visual Studio.</span></span>

    <span data-ttu-id="0321f-122">各移行を実行してから、シード メソッドが実行されます。</span><span class="sxs-lookup"><span data-stu-id="0321f-122">Each migration will run, then the seed method will run.</span></span> <span data-ttu-id="0321f-123">今すぐアプリを実行することができます。</span><span class="sxs-lookup"><span data-stu-id="0321f-123">You can now run the app.</span></span>

    ![](building-the-ef5-mvc4-chapter-downloads/_static/image5.png)

>[!div class="step-by-step"]
[<span data-ttu-id="0321f-124">前へ</span><span class="sxs-lookup"><span data-stu-id="0321f-124">Previous</span></span>](advanced-entity-framework-scenarios-for-an-mvc-web-application.md)
