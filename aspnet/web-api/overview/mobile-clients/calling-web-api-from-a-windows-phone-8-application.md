---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: 8 アプリケーション (c#) を、Windows Phone から Web API を呼び出す |Microsoft Docs
author: rmcmurray
description: Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションから成る完全なエンド ツー エンド シナリオを作成します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 40be2935c52e7dcab9e682d4d15e9e75c0260223
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388423"
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="3426a-103">Windows Phone 8 アプリケーション (c#) から Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="3426a-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="3426a-104">によって[Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="3426a-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="3426a-105">このチュートリアルでは、Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションから成る完全なエンド ツー エンド シナリオを作成する方法を学びます。</span><span class="sxs-lookup"><span data-stu-id="3426a-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="3426a-106">概要</span><span class="sxs-lookup"><span data-stu-id="3426a-106">Overview</span></span>

<span data-ttu-id="3426a-107">ASP.NET Web API などの rESTful サービスは、サーバー側とクライアント側のアプリケーションのアーキテクチャを取り除くことで開発者のための HTTP ベースのアプリケーションの作成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="3426a-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="3426a-108">専用ソケット ベースのプロトコルの通信を作成する代わりに Web API の開発者だけを必要な HTTP メソッドを公開する (例: GET、POST、PUT、DELETE)、クライアント アプリケーションの開発者がのみを使用する必要がありますそのアプリケーションに必要な HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="3426a-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="3426a-109">このエンド ツー エンド チュートリアルでは、Web API を使用して、次のプロジェクトを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="3426a-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="3426a-110">[このチュートリアルの最初の部分](#STEP1)はすべての書籍のカタログの管理の作成、読み取り、更新、および削除 (CRUD) 操作をサポートする ASP.NET Web API アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3426a-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="3426a-111">このアプリケーションを使用して、[サンプル XML ファイル (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) MSDN から。</span><span class="sxs-lookup"><span data-stu-id="3426a-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="3426a-112">[このチュートリアルのパート 2 つ目の](#STEP2)は、Web API アプリケーションからデータを取得する対話型の Windows Phone 8 アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3426a-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="3426a-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="3426a-113">Prerequisites</span></span>

- <span data-ttu-id="3426a-114">Windows Phone 8 sdk がインストールされた visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="3426a-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="3426a-115">Windows 8 または後で、HYPER-V がインストールされていると、64 ビット システム</span><span class="sxs-lookup"><span data-stu-id="3426a-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="3426a-116">その他の要件の一覧は、次を参照してください。、*システム要件*セクションで、 [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471)ページをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="3426a-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="3426a-117">Web API と、ローカル システム上の Windows Phone 8 プロジェクトの間の接続をテストしようとするで手順を実行する必要があります、 *[にローカルで Web API アプリケーション、Windows Phone 8 エミュレーターを接続します。コンピューター](https://go.microsoft.com/fwlink/?LinkId=324014)* に関する記事をテスト環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="3426a-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="3426a-118">手順 1: Web API の書店プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="3426a-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="3426a-119">このエンド ツー エンドのチュートリアルの最初の手順は、すべての CRUD 操作; をサポートする Web API プロジェクトを作成するにはこのソリューションで、Windows Phone アプリケーションのプロジェクトが追加されることに注意してください。[手順 2.](#STEP2)このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="3426a-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="3426a-120">開いている**Visual Studio 2013**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="3426a-121">クリックして**ファイル**、し**新しい**、し**プロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="3426a-122">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール済み**、し**テンプレート**、し**Visual c#**、し**Web**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="3426a-123">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-123">Click image to expand</span></span>                                                                |


4. <span data-ttu-id="3426a-124">強調表示**ASP.NET Web アプリケーション**、入力**BookStore**プロジェクト名、およびクリックの **[ok]** します。</span><span class="sxs-lookup"><span data-stu-id="3426a-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="3426a-125">ときに、**新しい ASP.NET プロジェクト** ダイアログ ボックスが表示されたら、選択、 **Web API**テンプレート、およびクリック**OK**。</span><span class="sxs-lookup"><span data-stu-id="3426a-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>


   | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
   |-----------------------------------------------------------------------------------------------------------------------------------------------------|
   |                                                                <span data-ttu-id="3426a-126">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-126">Click image to expand</span></span>                                                                |


6. <span data-ttu-id="3426a-127">Web API プロジェクトが開いたら、プロジェクトから、サンプル コント ローラーを削除します。</span><span class="sxs-lookup"><span data-stu-id="3426a-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="3426a-128">展開、**コント ローラー**ソリューション エクスプ ローラーでフォルダー。</span><span class="sxs-lookup"><span data-stu-id="3426a-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="3426a-129">右クリックし、 **ValuesController.cs**ファイルを開き、をクリックし、**削除**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="3426a-130">クリックして**OK**削除の確認を求められたらします。</span><span class="sxs-lookup"><span data-stu-id="3426a-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="3426a-131">Web API プロジェクトに XML データ ファイルを追加します。このファイルには、bookstore カタログの内容が含まれています。</span><span class="sxs-lookup"><span data-stu-id="3426a-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

   1. <span data-ttu-id="3426a-132">右クリックし、**アプリ\_データ**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、順にクリックします**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
   2. <span data-ttu-id="3426a-133">ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、強調表示、 **XML ファイル**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="3426a-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
   3. <span data-ttu-id="3426a-134">ファイルに名前を**Books.xml**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-134">Name the file **Books.xml**, and then click **Add**.</span></span>
   4. <span data-ttu-id="3426a-135">ときに、 **Books.xml**ファイルを開くと、ファイル内のコードをサンプルから XML に置き換えます**books.xml** MSDN 上のファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
   5. <span data-ttu-id="3426a-136">保存して XML ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="3426a-136">Save and close the XML file.</span></span>

8. <span data-ttu-id="3426a-137">Bookstore モデルを Web API プロジェクトに追加します。このモデルには、書店アプリケーションの作成、読み取り、更新、および削除 (CRUD) のロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="3426a-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

   1. <span data-ttu-id="3426a-138">右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、 をクリックし、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   2. <span data-ttu-id="3426a-139">ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルに名前を**BookDetails.cs**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   3. <span data-ttu-id="3426a-140">ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3426a-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
   4. <span data-ttu-id="3426a-141">保存して閉じます、 **BookDetails.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-141">Save and close the **BookDetails.cs** file.</span></span>

9. <span data-ttu-id="3426a-142">Web API プロジェクトには、bookstore コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="3426a-142">Add the bookstore controller to the Web API project:</span></span>

   1. <span data-ttu-id="3426a-143">右クリックし、**コント ローラー**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、 をクリックし、**コント ローラー**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
   2. <span data-ttu-id="3426a-144">ときに、**スキャフォールディングの追加** ダイアログ ボックスが表示されたら、強調表示**Web API 2 コント ローラー - 空**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
   3. <span data-ttu-id="3426a-145">ときに、**コント ローラーの追加** ダイアログ ボックスが表示されたら、名前、コント ローラー **BooksController**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
   4. <span data-ttu-id="3426a-146">ときに、 **BooksController.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3426a-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
   5. <span data-ttu-id="3426a-147">保存して閉じます、 **BooksController.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-147">Save and close the **BooksController.cs** file.</span></span>

10. <span data-ttu-id="3426a-148">エラーをチェックする Web API アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3426a-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="3426a-149">手順 2: Windows Phone 8 Bookstore カタログのプロジェクトを追加します。</span><span class="sxs-lookup"><span data-stu-id="3426a-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="3426a-150">このエンド ツー エンド シナリオの次の手順では、Windows Phone 8 向けカタログ アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="3426a-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="3426a-151">このアプリケーションを使用して、 *Windows Phone のデータ バインド アプリ*テンプレートでは、既定のユーザー インターフェイスで作成した Web API アプリケーションが使用されます[手順 1.](#STEP1)データ ソースとしては、このチュートリアルの。</span><span class="sxs-lookup"><span data-stu-id="3426a-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="3426a-152">右クリックし、**書店**でソリューション、ソリューション エクスプ ローラーでをクリックし、**追加**、し**新しいプロジェクト**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="3426a-153">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール済み**、し**Visual c#**、し**Windows Phone**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="3426a-154">強調表示**Windows Phone のデータ バインド アプリ**、入力**BookCatalog**名、およびクリック **[ok]** します。</span><span class="sxs-lookup"><span data-stu-id="3426a-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="3426a-155">Json.NET の NuGet パッケージを追加、 **BookCatalog**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="3426a-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="3426a-156">右クリックして**参照**の**BookCatalog**ソリューション エクスプ ローラーでプロジェクトをクリックして**NuGet パッケージの管理**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="3426a-157">ときに、 **NuGet パッケージの管理** ダイアログ ボックスが表示されたら、展開、**オンライン**セクションし、強調表示**nuget.org**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="3426a-158">入力**Json.NET**検索フィールドし、検索アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="3426a-159">強調表示**Json.NET**検索結果、およびクリックで**インストール**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="3426a-160">インストールが完了したら、クリックして**閉じる**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="3426a-161">追加、 **BookDetails**モデルを**BookCatalog**プロジェクト; bookstore クラスの汎用モデルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="3426a-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

   1. <span data-ttu-id="3426a-162">右クリックし、 **BookCatalog**ソリューション エクスプ ローラーでプロジェクトの [**追加**、] をクリックし、**新しいフォルダー**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
   2. <span data-ttu-id="3426a-163">新しいフォルダーの名前**モデル**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-163">Name the new folder **Models**.</span></span>
   3. <span data-ttu-id="3426a-164">右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、 をクリックし、**クラス**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
   4. <span data-ttu-id="3426a-165">ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルに名前を**BookDetails.cs**、 をクリックし、**追加**します。</span><span class="sxs-lookup"><span data-stu-id="3426a-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
   5. <span data-ttu-id="3426a-166">ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3426a-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
   6. <span data-ttu-id="3426a-167">保存して閉じます、 **BookDetails.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-167">Save and close the **BookDetails.cs** file.</span></span>

6. <span data-ttu-id="3426a-168">更新プログラム、 **MainViewModel.cs** BookStore Web API アプリケーションと通信するための機能を含めるクラス。</span><span class="sxs-lookup"><span data-stu-id="3426a-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

   1. <span data-ttu-id="3426a-169">展開、 **ViewModels**フォルダーをダブルクリックして、ソリューション エクスプ ローラーで、 **MainViewModel.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
   2. <span data-ttu-id="3426a-170">ときに、 **MainViewModel.cs**ファイルが開かれる、次のファイルのコードに置き換えます以外の値を更新する必要がありますに注意してください、`apiUrl`定数と、Web API の実際の URL:。</span><span class="sxs-lookup"><span data-stu-id="3426a-170">When the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

       [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
   3. <span data-ttu-id="3426a-171">保存して閉じます、 **MainViewModel.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-171">Save and close the **MainViewModel.cs** file.</span></span>

7. <span data-ttu-id="3426a-172">更新プログラム、 **MainPage.xaml**アプリケーション名をカスタマイズするファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

   1. <span data-ttu-id="3426a-173">ダブルクリックして、 **MainPage.xaml**ソリューション エクスプ ローラーでファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="3426a-174">ときに、 **MainPage.xaml**ファイルを開くと、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="3426a-174">When the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
   3. <span data-ttu-id="3426a-175">次のようにこれらの行に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3426a-175">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
   4. <span data-ttu-id="3426a-176">保存して閉じます、 **MainPage.xaml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-176">Save and close the **MainPage.xaml** file.</span></span>

8. <span data-ttu-id="3426a-177">更新プログラム、 **DetailsPage.xaml**表示されている項目をカスタマイズするファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

   1. <span data-ttu-id="3426a-178">ダブルクリックして、 **DetailsPage.xaml**ソリューション エクスプ ローラーでファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
   2. <span data-ttu-id="3426a-179">ときに、 **DetailsPage.xaml**ファイルを開くと、次のコード行を見つけます。</span><span class="sxs-lookup"><span data-stu-id="3426a-179">When the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
   3. <span data-ttu-id="3426a-180">次のようにこれらの行に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="3426a-180">Replace those lines with the following:</span></span> 

       [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
   4. <span data-ttu-id="3426a-181">保存して閉じます、 **DetailsPage.xaml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="3426a-181">Save and close the **DetailsPage.xaml** file.</span></span>

9. <span data-ttu-id="3426a-182">エラーをチェックする Windows Phone アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="3426a-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="3426a-183">手順 3: エンド ツー エンド ソリューションをテストします。</span><span class="sxs-lookup"><span data-stu-id="3426a-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="3426a-184">説明したように、*の前提条件*Web API と Windows Phone 8 間の接続をテストするときに、このチュートリアルのセクションが、ローカル システムでプロジェクトの場合の手順を実行する必要があります、  *[ローカル コンピューター上の Web API アプリケーションに、Windows Phone 8 エミュレーターを接続する](https://go.microsoft.com/fwlink/?LinkId=324014)* に関する記事をテスト環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="3426a-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="3426a-185">構成テスト環境を作成したら、Windows Phone アプリケーションをスタートアップ プロジェクトとして設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="3426a-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="3426a-186">これを行うには、強調表示、 **BookCatalog**アプリケーションで、ソリューション エクスプ ローラーとクリック**スタートアップ プロジェクトとして設定**:</span><span class="sxs-lookup"><span data-stu-id="3426a-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="3426a-187">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-187">Click image to expand</span></span> |

<span data-ttu-id="3426a-188">F5 キーを押すと、Visual Studio がエミュレーターを起動両方、Windows Phone が表示されます、&quot;お待ちください&quot;メッセージ アプリケーションのデータは、Web API から取得されます。</span><span class="sxs-lookup"><span data-stu-id="3426a-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="3426a-189">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-189">Click image to expand</span></span> |

<span data-ttu-id="3426a-190">すべてが成功した場合は、表示されるカタログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3426a-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="3426a-191">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-191">Click image to expand</span></span> |

<span data-ttu-id="3426a-192">書籍のタイトルをタップすると、アプリケーションにより、ブックの説明が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3426a-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="3426a-193">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-193">Click image to expand</span></span> |

<span data-ttu-id="3426a-194">アプリケーションは、Web API と通信できない、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="3426a-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="3426a-195">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-195">Click image to expand</span></span> |

<span data-ttu-id="3426a-196">エラー メッセージでタップすると、エラーに関する詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="3426a-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>


| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
|-------------------------------------------------------------------------------------------------------------------------------------------------------|
|                                                                 <span data-ttu-id="3426a-197">展開するイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="3426a-197">Click image to expand</span></span>                                                                 |

