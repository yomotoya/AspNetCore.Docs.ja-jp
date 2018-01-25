---
uid: web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
title: "8 アプリケーション (c#) を、Windows Phone から Web API を呼び出して |Microsoft ドキュメント"
author: rmcmurray
description: "Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションで構成される完全なエンド ツー エンド シナリオを作成します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/09/2013
ms.topic: article
ms.assetid: b9775f41-352a-4f82-baa6-23e95b342e20
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/mobile-clients/calling-web-api-from-a-windows-phone-8-application
msc.type: authoredcontent
ms.openlocfilehash: 6e5a936decb27fd2e3b8cdcea44db8db822c98eb
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="calling-web-api-from-a-windows-phone-8-application-c"></a><span data-ttu-id="16d71-103">Windows Phone 8 アプリケーション (c#) からの Web API の呼び出し</span><span class="sxs-lookup"><span data-stu-id="16d71-103">Calling Web API from a Windows Phone 8 Application (C#)</span></span>
====================
<span data-ttu-id="16d71-104">によって[Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="16d71-104">by [Robert McMurray](https://github.com/rmcmurray)</span></span>

<span data-ttu-id="16d71-105">このチュートリアルでは、Windows Phone 8 アプリケーションに書籍のカタログを提供する ASP.NET Web API アプリケーションで構成される完全なエンド ツー エンド シナリオを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="16d71-105">In this tutorial, you will learn how to create a complete end-to-end scenario consisting of an ASP.NET Web API application that provides a catalog of books to a Windows Phone 8 application.</span></span>

### <a name="overview"></a><span data-ttu-id="16d71-106">概要</span><span class="sxs-lookup"><span data-stu-id="16d71-106">Overview</span></span>

<span data-ttu-id="16d71-107">ASP.NET Web API などの rESTful サービスは、サーバー側とクライアント側のアプリケーションのアーキテクチャを抽象化され、開発者のための HTTP ベースのアプリケーションの作成を簡略化します。</span><span class="sxs-lookup"><span data-stu-id="16d71-107">RESTful services like ASP.NET Web API simplify the creation of HTTP-based applications for developers by abstracting the architecture for server-side and client-side applications.</span></span> <span data-ttu-id="16d71-108">通信用にある独自のソケット ベースのプロトコルを作成するには、代わりに Web API の開発者だけをそのアプリケーションの必要条件の HTTP メソッドを公開する (例: GET、POST、PUT、DELETE)、およびクライアント アプリケーションの開発者がのみを使用する必要がありますそのアプリケーションに必要な HTTP メソッド。</span><span class="sxs-lookup"><span data-stu-id="16d71-108">Instead of creating a proprietary socket-based protocol for communication, Web API developers simply need to publish the requisite HTTP methods for their application, (for example: GET, POST, PUT, DELETE), and client application developers only need to consume the HTTP methods that are necessary for their application.</span></span>

<span data-ttu-id="16d71-109">このエンド ツー エンド チュートリアルでは、Web API を使用して、次のプロジェクトを作成する方法を学習します。</span><span class="sxs-lookup"><span data-stu-id="16d71-109">In this end-to-end tutorial, you will learn how to use Web API to create the following projects:</span></span>

- <span data-ttu-id="16d71-110">[このチュートリアルの最初の部分](#STEP1)はすべての書籍のカタログを管理する作成、読み取り、更新、および削除 (CRUD) 操作をサポートする ASP.NET Web API アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="16d71-110">In the [first part of this tutorial](#STEP1), you will create an ASP.NET Web API application that supports all of the Create, Read, Update, and Delete (CRUD) operations to manage a book catalog.</span></span> <span data-ttu-id="16d71-111">このアプリケーションを使用して、[サンプル XML ファイル (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) MSDN からです。</span><span class="sxs-lookup"><span data-stu-id="16d71-111">This application will use the [Sample XML File (books.xml)](https://msdn.microsoft.com/library/windows/desktop/ms762271.aspx) from MSDN.</span></span>
- <span data-ttu-id="16d71-112">[2 番目のこのチュートリアルのパート](#STEP2)は、Web API アプリケーションからデータを取得する対話型の Windows Phone 8 アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="16d71-112">In the [second part of this tutorial](#STEP2), you will create an interactive Windows Phone 8 application that retrieves the data from your Web API application.</span></span>

#### <a name="prerequisites"></a><span data-ttu-id="16d71-113">必須コンポーネント</span><span class="sxs-lookup"><span data-stu-id="16d71-113">Prerequisites</span></span>

- <span data-ttu-id="16d71-114">Visual Studio 2013、Windows Phone 8 SDK がインストールされていると</span><span class="sxs-lookup"><span data-stu-id="16d71-114">Visual Studio 2013 with the Windows Phone 8 SDK installed</span></span>
- <span data-ttu-id="16d71-115">Windows 8 または後でインストールされている HYPER-V と 64 ビット システム</span><span class="sxs-lookup"><span data-stu-id="16d71-115">Windows 8 or later on a 64-bit system with Hyper-V installed</span></span>
- <span data-ttu-id="16d71-116">その他の要件の一覧は、次を参照してください。、*システム要件*セクションで、 [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471)ページをダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="16d71-116">For a list of additional requirements, see the *System Requirements* section on the [Windows Phone SDK 8.0](https://www.microsoft.com/download/details.aspx?id=35471) download page.</span></span>

> [!NOTE]
> <span data-ttu-id="16d71-117">Web API と、ローカル システム上の Windows Phone 8 プロジェクト間の接続をテストすることがで手順に従う必要があります、 *[ローカル上の Web API アプリケーションを Windows Phone 8 エミュレーターの接続コンピューター](https://go.microsoft.com/fwlink/?LinkId=324014)* アーティクルをテスト環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="16d71-117">If you are going to test the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>


<a id="STEP1"></a>
### <a name="step-1-creating-the-web-api-bookstore-project"></a><span data-ttu-id="16d71-118">手順 1: Web API Bookstore プロジェクトの作成</span><span class="sxs-lookup"><span data-stu-id="16d71-118">Step 1: Creating the Web API Bookstore Project</span></span>

<span data-ttu-id="16d71-119">このエンド ツー エンド チュートリアルの最初のステップが、CRUD 操作のすべてをサポートする Web API プロジェクトを作成するにはこのソリューションで Windows Phone アプリケーション プロジェクトを追加することに注意してください[手順 2.](#STEP2)このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="16d71-119">The first step of this end-to-end tutorial is to create a Web API project that supports all of the CRUD operations; note that you will add the Windows Phone application project to this solution in [Step 2](#STEP2) of this tutorial.</span></span>

1. <span data-ttu-id="16d71-120">開いている**Visual Studio 2013**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-120">Open **Visual Studio 2013**.</span></span>
2. <span data-ttu-id="16d71-121">をクリックして**ファイル**、し**新しい**、し**プロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-121">Click **File**, then **New**, and then **Project**.</span></span>
3. <span data-ttu-id="16d71-122">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール**、し**テンプレート**、し**Visual c#**、し**Web**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-122">When the **New Project** dialog box is displayed, expand **Installed**, then **Templates**, then **Visual C#**, and then **Web**.</span></span>

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image2.png)](calling-web-api-from-a-windows-phone-8-application/_static/image1.png) |
    | --- |
    | <span data-ttu-id="16d71-123">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-123">Click image to expand</span></span> |
4. <span data-ttu-id="16d71-124">強調表示**ASP.NET Web アプリケーション**、入力**BookStore**プロジェクト名、およびクリック**[ok]**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-124">Highlight **ASP.NET Web Application**, enter **BookStore** for the project name, and then click **OK**.</span></span>
5. <span data-ttu-id="16d71-125">ときに、**新しい ASP.NET プロジェクト** ダイアログ ボックスが表示されたら、選択、 **Web API**テンプレート、およびクリック**OK**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-125">When the **New ASP.NET Project** dialog box is displayed, select the **Web API** template, and then click **OK**.</span></span>

    | [![](calling-web-api-from-a-windows-phone-8-application/_static/image4.png)](calling-web-api-from-a-windows-phone-8-application/_static/image3.png) |
    | --- |
    | <span data-ttu-id="16d71-126">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-126">Click image to expand</span></span> |
6. <span data-ttu-id="16d71-127">Web API プロジェクトを開くと、プロジェクトから、サンプル コント ローラーを削除します。</span><span class="sxs-lookup"><span data-stu-id="16d71-127">When the Web API project opens, remove the sample controller from the project:</span></span>

    1. <span data-ttu-id="16d71-128">展開して、**コント ローラー**ソリューション エクスプ ローラーでフォルダーです。</span><span class="sxs-lookup"><span data-stu-id="16d71-128">Expand the **Controllers** folder in the solution explorer.</span></span>
    2. <span data-ttu-id="16d71-129">右クリックし、 **ValuesController.cs**ファイルを開き、をクリックして**削除**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-129">Right-click the **ValuesController.cs** file, and then click **Delete**.</span></span>
    3. <span data-ttu-id="16d71-130">をクリックして**OK**削除の確認を求められたらです。</span><span class="sxs-lookup"><span data-stu-id="16d71-130">Click **OK** when prompted to confirm the deletion.</span></span>
7. <span data-ttu-id="16d71-131">XML データ ファイルを Web API プロジェクトに追加します。このファイルには、bookstore カタログの内容が含まれています。</span><span class="sxs-lookup"><span data-stu-id="16d71-131">Add an XML data file to the Web API project; this file contains the contents of the bookstore catalog:</span></span>

    1. <span data-ttu-id="16d71-132">右クリックし、**アプリ\_データ**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、順にクリック**新しい項目の**します。</span><span class="sxs-lookup"><span data-stu-id="16d71-132">Right-click the **App\_Data** folder in the solution explorer, then click **Add**, and then click **New Item**.</span></span>
    2. <span data-ttu-id="16d71-133">ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、強調表示、 **XML ファイル**テンプレート。</span><span class="sxs-lookup"><span data-stu-id="16d71-133">When the **Add New Item** dialog box is displayed, highlight the **XML File** template.</span></span>
    3. <span data-ttu-id="16d71-134">ファイルの名前を付けます**Books.xml**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-134">Name the file **Books.xml**, and then click **Add**.</span></span>
    4. <span data-ttu-id="16d71-135">ときに、 **Books.xml**ファイルを開くと、ファイルのコード サンプルの XML に置き換えます**books.xml** MSDN 上のファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-135">When the **Books.xml** file is opened, replace the code in the file with the XML from the sample **books.xml** file on MSDN:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample1.xml)]
    5. <span data-ttu-id="16d71-136">保存して XML ファイルを閉じます。</span><span class="sxs-lookup"><span data-stu-id="16d71-136">Save and close the XML file.</span></span>
8. <span data-ttu-id="16d71-137">Web API プロジェクトに bookstore モデルを追加します。このモデルには、bookstore アプリケーションの作成、読み取り、更新、および削除 (CRUD) のロジックが含まれています。</span><span class="sxs-lookup"><span data-stu-id="16d71-137">Add the bookstore model to the Web API project; this model contains the Create, Read, Update, and Delete (CRUD) logic for the bookstore application:</span></span>

    1. <span data-ttu-id="16d71-138">右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、クリックして**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-138">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
    2. <span data-ttu-id="16d71-139">ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルの名前を付けます**BookDetails.cs**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-139">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
    3. <span data-ttu-id="16d71-140">ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16d71-140">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample2.cs)]
    4. <span data-ttu-id="16d71-141">保存して閉じます、 **BookDetails.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-141">Save and close the **BookDetails.cs** file.</span></span>
9. <span data-ttu-id="16d71-142">Web API プロジェクトに bookstore コント ローラーを追加します。</span><span class="sxs-lookup"><span data-stu-id="16d71-142">Add the bookstore controller to the Web API project:</span></span>

    1. <span data-ttu-id="16d71-143">右クリックし、**コント ローラー**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、クリックして**コント ローラー**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-143">Right-click the **Controllers** folder in the solution explorer, then click **Add**, and then click **Controller**.</span></span>
    2. <span data-ttu-id="16d71-144">ときに、**追加 Scaffold**  ダイアログ ボックスが表示されたら、強調表示**Web API 2 コント ローラー - 空**、順にクリック**追加**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-144">When the **Add Scaffold** dialog box is displayed, highlight **Web API 2 Controller - Empty**, and then click **Add**.</span></span>
    3. <span data-ttu-id="16d71-145">ときに、**コント ローラーの追加** ダイアログ ボックスが表示されたら、コント ローラーの名前を付けます**BooksController**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-145">When the **Add Controller** dialog box is displayed, name the controller **BooksController**, and then click **Add**.</span></span>
    4. <span data-ttu-id="16d71-146">ときに、 **BooksController.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16d71-146">When the **BooksController.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample3.cs)]
    5. <span data-ttu-id="16d71-147">保存して閉じます、 **BooksController.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-147">Save and close the **BooksController.cs** file.</span></span>
10. <span data-ttu-id="16d71-148">エラーをチェックする Web API アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="16d71-148">Build the Web API application to check for errors.</span></span>

<a id="STEP2"></a>
### <a name="step-2-adding-the-windows-phone-8-bookstore-catalog-project"></a><span data-ttu-id="16d71-149">手順 2: Windows Phone 8 Bookstore カタログ プロジェクトの追加</span><span class="sxs-lookup"><span data-stu-id="16d71-149">Step 2: Adding the Windows Phone 8 Bookstore Catalog Project</span></span>

<span data-ttu-id="16d71-150">このエンド ツー エンド シナリオの次の手順では、Windows Phone 8 用のカタログのアプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="16d71-150">The next step of this end-to-end scenario is to create the catalog application for Windows Phone 8.</span></span> <span data-ttu-id="16d71-151">このアプリケーションで使用、 *Windows Phone のデータ バインド アプリ*で作成した Web API アプリケーションをテンプレートでは、既定のユーザー インターフェイスが使用される[手順 1.](#STEP1)データ ソースとしては、このチュートリアルのです。</span><span class="sxs-lookup"><span data-stu-id="16d71-151">This application will use the *Windows Phone Databound App* template for the default user interface, and it will use the Web API application that you created in [Step 1](#STEP1) of this tutorial as the data source.</span></span>

1. <span data-ttu-id="16d71-152">右クリックし、 **BookStore**でソリューションは、ソリューション エクスプ ローラーで、をクリックして**追加**、し、**新しいプロジェクト**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-152">Right-click the **BookStore** solution in the in the solution explorer, then click **Add**, and then **New Project**.</span></span>
2. <span data-ttu-id="16d71-153">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**インストール**、し**Visual c#**、し**Windows Phone**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-153">When the **New Project** dialog box is displayed, expand **Installed**, then **Visual C#**, and then **Windows Phone**.</span></span>
3. <span data-ttu-id="16d71-154">強調表示**Windows Phone のデータ バインド アプリ**、入力**BookCatalog**  をクリックし、名前の**ok**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-154">Highlight **Windows Phone Databound App**, enter **BookCatalog** for the name, and then click **OK**.</span></span>
4. <span data-ttu-id="16d71-155">Json.NET の NuGet パッケージを追加、 **BookCatalog**プロジェクト。</span><span class="sxs-lookup"><span data-stu-id="16d71-155">Add the Json.NET NuGet package to the **BookCatalog** project:</span></span>

    1. <span data-ttu-id="16d71-156">右クリック**参照**の**BookCatalog**ソリューション エクスプ ローラーでプロジェクトをクリックして**NuGet パッケージの管理**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-156">Right-click **References** for the **BookCatalog** project in the solution explorer, and then click **Manage NuGet Packages**.</span></span>
    2. <span data-ttu-id="16d71-157">ときに、 **NuGet パッケージの管理** ダイアログ ボックスが表示されたら、展開、**オンライン**セクションし、強調表示**nuget.org**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-157">When the **Manage NuGet Packages** dialog box is displayed, expand the **Online** section, and highlight **nuget.org**.</span></span>
    3. <span data-ttu-id="16d71-158">入力**Json.NET**検索フィールドし、検索アイコンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="16d71-158">Enter **Json.NET** in the search field and click the search icon.</span></span>
    4. <span data-ttu-id="16d71-159">強調表示**Json.NET**をクリックして、検索結果**インストール**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-159">Highlight **Json.NET** in the search results, and then click **Install**.</span></span>
    5. <span data-ttu-id="16d71-160">インストールが完了したらをクリックして**閉じる**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-160">When the installation has completed, click **Close**.</span></span>
5. <span data-ttu-id="16d71-161">追加、 **BookDetails**モデルを**BookCatalog**プロジェクト; bookstore クラスの汎用モデルが含まれます。</span><span class="sxs-lookup"><span data-stu-id="16d71-161">Add the **BookDetails** model to the **BookCatalog** project; this contains a generic model of the bookstore class:</span></span>

    1. <span data-ttu-id="16d71-162">右クリックし、 **BookCatalog**プロジェクト、ソリューション エクスプ ローラーで、をクリックして**追加**、クリックして**新しいフォルダー**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-162">Right-click the **BookCatalog** project in the solution explorer, then click **Add**, and then click **New Folder**.</span></span>
    2. <span data-ttu-id="16d71-163">新しいフォルダーの名前を付けます**モデル**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-163">Name the new folder **Models**.</span></span>
    3. <span data-ttu-id="16d71-164">右クリックし、**モデル**ソリューション エクスプ ローラーでフォルダーをクリックし、**追加**、クリックして**クラス**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-164">Right-click the **Models** folder in the solution explorer, then click **Add**, and then click **Class**.</span></span>
    4. <span data-ttu-id="16d71-165">ときに、**新しい項目の追加** ダイアログ ボックスが表示されたら、クラス ファイルの名前を付けます**BookDetails.cs**、クリックして**追加**です。</span><span class="sxs-lookup"><span data-stu-id="16d71-165">When the **Add New Item** dialog box is displayed, name the class file **BookDetails.cs**, and then click **Add**.</span></span>
    5. <span data-ttu-id="16d71-166">ときに、 **BookDetails.cs**ファイルを開くと、ファイル内のコードを次に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16d71-166">When the **BookDetails.cs** file is opened, replace the code in the file with the following:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample4.cs)]
    6. <span data-ttu-id="16d71-167">保存して閉じます、 **BookDetails.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-167">Save and close the **BookDetails.cs** file.</span></span>
6. <span data-ttu-id="16d71-168">更新プログラム、 **MainViewModel.cs**クラス BookStore Web API アプリケーションと通信する機能を含めます。</span><span class="sxs-lookup"><span data-stu-id="16d71-168">Update the **MainViewModel.cs** class to include the functionality to communicate with the BookStore Web API application:</span></span>

    1. <span data-ttu-id="16d71-169">展開して、 **ViewModels**フォルダーをクリックしてソリューション エクスプ ローラーで、 **MainViewModel.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-169">Expand the **ViewModels** folder in the solution explorer, and then double-click the **MainViewModel.cs** file.</span></span>
    2. <span data-ttu-id="16d71-170">ときに、 **MainViewModel.cs**ファイルを開く、ファイル内のコードを次に置き換えますの値を更新する必要があります、`apiUrl`定数と、Web API の実際の URL:。</span><span class="sxs-lookup"><span data-stu-id="16d71-170">When the the **MainViewModel.cs** file is opened, replace the code in the file with the following; note that you will need to update the value of the `apiUrl` constant with the actual URL of your Web API:</span></span> 

        [!code-csharp[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample5.cs)]
    3. <span data-ttu-id="16d71-171">保存して閉じます、 **MainViewModel.cs**ファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-171">Save and close the **MainViewModel.cs** file.</span></span>
7. <span data-ttu-id="16d71-172">更新プログラム、 **MainPage.xaml**アプリケーション名をカスタマイズするファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-172">Update the **MainPage.xaml** file to customize the application name:</span></span>

    1. <span data-ttu-id="16d71-173">ダブルクリックして、 **MainPage.xaml**ソリューション エクスプ ローラーでファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-173">Double-click the **MainPage.xaml** file in the solution explorer.</span></span>
    2. <span data-ttu-id="16d71-174">ときに、 **MainPage.xaml**ファイルを開くと、次のコード行を探します。</span><span class="sxs-lookup"><span data-stu-id="16d71-174">When the the **MainPage.xaml** file is opened, locate the following lines of code:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample6.xml)]
    3. <span data-ttu-id="16d71-175">これらの行を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16d71-175">Replace those lines with the following:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample7.xml)]
    4. <span data-ttu-id="16d71-176">保存して閉じます、 **MainPage.xaml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-176">Save and close the **MainPage.xaml** file.</span></span>
8. <span data-ttu-id="16d71-177">更新プログラム、 **DetailsPage.xaml**表示されている項目をカスタマイズするファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-177">Update the **DetailsPage.xaml** file to customize the displayed items:</span></span>

    1. <span data-ttu-id="16d71-178">ダブルクリックして、 **DetailsPage.xaml**ソリューション エクスプ ローラーでファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-178">Double-click the **DetailsPage.xaml** file in the solution explorer.</span></span>
    2. <span data-ttu-id="16d71-179">ときに、 **DetailsPage.xaml**ファイルを開くと、次のコード行を探します。</span><span class="sxs-lookup"><span data-stu-id="16d71-179">When the the **DetailsPage.xaml** file is opened, locate the following lines of code:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample8.xml)]
    3. <span data-ttu-id="16d71-180">これらの行を次のように置き換えます。</span><span class="sxs-lookup"><span data-stu-id="16d71-180">Replace those lines with the following:</span></span> 

        [!code-xml[Main](calling-web-api-from-a-windows-phone-8-application/samples/sample9.xml)]
    4. <span data-ttu-id="16d71-181">保存して閉じます、 **DetailsPage.xaml**ファイル。</span><span class="sxs-lookup"><span data-stu-id="16d71-181">Save and close the **DetailsPage.xaml** file.</span></span>
9. <span data-ttu-id="16d71-182">エラーをチェックする Windows Phone アプリケーションをビルドします。</span><span class="sxs-lookup"><span data-stu-id="16d71-182">Build the Windows Phone application to check for errors.</span></span>

### <a name="step-3-testing-the-end-to-end-solution"></a><span data-ttu-id="16d71-183">手順 3: エンド ツー エンド ソリューションをテストします。</span><span class="sxs-lookup"><span data-stu-id="16d71-183">Step 3: Testing the End-to-End Solution</span></span>

<span data-ttu-id="16d71-184">説明したように、*の前提条件*このチュートリアルでは、Web API と Windows Phone 8 の間の接続をテストするときのセクションでは、ローカル システムでプロジェクトの場合はで手順に従う必要があります、  *[ローカル コンピューター上の Web API アプリケーションを Windows Phone 8 エミュレーターを接続する](https://go.microsoft.com/fwlink/?LinkId=324014)*アーティクルをテスト環境を設定します。</span><span class="sxs-lookup"><span data-stu-id="16d71-184">As mentioned in the *Prerequisites* section of this tutorial, when you are testing the connectivity between Web API and Windows Phone 8 projects on your local system, you will need to follow the instructions in the *[Connecting the Windows Phone 8 Emulator to Web API Applications on a Local Computer](https://go.microsoft.com/fwlink/?LinkId=324014)* article to set up your testing environment.</span></span>

<span data-ttu-id="16d71-185">構成されているテスト環境を作成したら、Windows Phone アプリケーションをスタートアップ プロジェクトとして設定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="16d71-185">Once you have the testing environment configured, you will need to set the Windows Phone application as the startup project.</span></span> <span data-ttu-id="16d71-186">これを行うには、強調表示、 **BookCatalog**をクリックして、ソリューション エクスプ ローラーで、アプリケーション**スタートアップ プロジェクトとして設定**:</span><span class="sxs-lookup"><span data-stu-id="16d71-186">To do so, highlight the **BookCatalog** application in the solution explorer, and then click **Set as StartUp Project**:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image6.png)](calling-web-api-from-a-windows-phone-8-application/_static/image5.png) |
| --- |
| <span data-ttu-id="16d71-187">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-187">Click image to expand</span></span> |

<span data-ttu-id="16d71-188">F5 キーを押すと、Visual Studio が起動両方 Windows Phone エミュレーターが表示されます、&quot;お待ちください&quot;Web API からアプリケーション データを取得中のメッセージします。</span><span class="sxs-lookup"><span data-stu-id="16d71-188">When you press F5, Visual Studio will start both the Windows Phone Emulator, which will display a &quot;Please Wait&quot; message while the application data is retrieved from your Web API:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image8.png)](calling-web-api-from-a-windows-phone-8-application/_static/image7.png) |
| --- |
| <span data-ttu-id="16d71-189">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-189">Click image to expand</span></span> |

<span data-ttu-id="16d71-190">すべてのものが成功した場合は、表示されるカタログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="16d71-190">If everything is successful, you should see the catalog displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image10.png)](calling-web-api-from-a-windows-phone-8-application/_static/image9.png) |
| --- |
| <span data-ttu-id="16d71-191">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-191">Click image to expand</span></span> |

<span data-ttu-id="16d71-192">場合は、書籍のタイトルをタップすると、アプリケーションにはブックの説明が表示されます。</span><span class="sxs-lookup"><span data-stu-id="16d71-192">If you tap on any book title, the application will display the book description:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image12.png)](calling-web-api-from-a-windows-phone-8-application/_static/image11.png) |
| --- |
| <span data-ttu-id="16d71-193">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-193">Click image to expand</span></span> |

<span data-ttu-id="16d71-194">アプリケーションは、Web API と通信できません、エラー メッセージが表示されます。</span><span class="sxs-lookup"><span data-stu-id="16d71-194">If the application cannot communicate with your Web API, an error message will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image14.png)](calling-web-api-from-a-windows-phone-8-application/_static/image13.png) |
| --- |
| <span data-ttu-id="16d71-195">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-195">Click image to expand</span></span> |

<span data-ttu-id="16d71-196">エラー メッセージでタップすると、エラーに関する詳細が表示されます。</span><span class="sxs-lookup"><span data-stu-id="16d71-196">If you tap on the error message, any additional details about the error will be displayed:</span></span>

| [![](calling-web-api-from-a-windows-phone-8-application/_static/image16.png)](calling-web-api-from-a-windows-phone-8-application/_static/image15.png) |
| --- |
| <span data-ttu-id="16d71-197">画像をクリックすると、展開</span><span class="sxs-lookup"><span data-stu-id="16d71-197">Click image to expand</span></span> |
