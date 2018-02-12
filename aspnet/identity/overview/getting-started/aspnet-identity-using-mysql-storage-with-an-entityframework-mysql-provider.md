---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: ": ASP.NET Identity EntityFramework MySQL プロバイダーでは (c#) での MySQL ストレージの使用 |Microsoft ドキュメント"
author: maumar
description: "このチュートリアルでは、MySQL または EntityFramework (SQL クライアント プロバイダー) と ASP.NET Identity の既定のデータ ストレージ機構を置き換える方法を表示しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/10/2013
ms.topic: article
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 82341724286a53f7883df324a391beeae3a9e2bd
ms.sourcegitcommit: 016f4d58663bcd442930227022de23fb3abee0b3
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/12/2018
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="8cdab-103">(C#) EntityFramework MySQL プロバイダーと MySQL の記憶域を使用して ASP.NET Id:</span><span class="sxs-lookup"><span data-stu-id="8cdab-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="8cdab-104">によって[Maurycy Markowski](https://github.com/maumar)、 [Raquel Soares De Almeida](https://github.com/raquelsa)、 [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="8cdab-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="8cdab-105">このチュートリアルの既定のデータ ストレージ機構を置換する方法を示します[ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) MySQL プロバイダーである EntityFramework (SQL クライアント プロバイダー) とします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="8cdab-106">次のトピックは、このチュートリアルで説明されます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="8cdab-107">Azure で MySQL データベースの作成</span><span class="sxs-lookup"><span data-stu-id="8cdab-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="8cdab-108">Visual Studio 2013 の MVC テンプレートを使用して MVC アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="8cdab-109">MySQL データベース プロバイダーと連携する EntityFramework を構成します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="8cdab-110">結果を確認するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-110">Running the application to verify the results</span></span>

<span data-ttu-id="8cdab-111">このチュートリアルの最後は格納 ASP.NET Id を持つ MVC アプリケーションが Azure でホストされている MySQL データベースを使用しています。</span><span class="sxs-lookup"><span data-stu-id="8cdab-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="8cdab-112">Azure の MySQL データベース インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="8cdab-113">ログインに、 [Azure ポータル](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409)です。</span><span class="sxs-lookup"><span data-stu-id="8cdab-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="8cdab-114">をクリックして**新規**クリックし、ページの下部にある**ストア**:</span><span class="sxs-lookup"><span data-stu-id="8cdab-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="8cdab-115">**アドオンと選択**ウィザードで、 **ClearDB MySQL データベース**、をクリックし、**次**フレームの下部にある矢印。</span><span class="sxs-lookup"><span data-stu-id="8cdab-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="8cdab-116">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-116">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-117">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="8cdab-118">既定値を保持**空き**の計画、および変更、**名前**に**IdentityMySQLDatabase**地域は、一番近いレベルを選択し、クリックして、 **次へ**フレームの下部にある矢印。</span><span class="sxs-lookup"><span data-stu-id="8cdab-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
 <span data-ttu-id="8cdab-119">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-119">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-120">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="8cdab-121">クリックして、**購買**データベースの作成を完了するチェック マークします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
 <span data-ttu-id="8cdab-122">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-122">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-123">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="8cdab-124">データベースが作成された後をから管理できる、**アドオン** タブで、管理ポータル。</span><span class="sxs-lookup"><span data-stu-id="8cdab-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="8cdab-125">クリックして、データベースの接続情報を取得する**接続情報**ページの下部にあります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
 <span data-ttu-id="8cdab-126">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-126">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-127">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="8cdab-128">接続文字列をコピーして [コピー] ボタンをクリックすると、 **CONNECTIONSTRING**フィールドし、保存以外の場合は、MVC アプリケーションのこのチュートリアルの後半では、この情報を使用するは。</span><span class="sxs-lookup"><span data-stu-id="8cdab-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
 <span data-ttu-id="8cdab-129">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-129">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-130">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="8cdab-131">MVC アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-131">Creating an MVC application project</span></span>

<span data-ttu-id="8cdab-132">チュートリアルのこのセクションの手順を完了するを最初にインストールする必要は[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)です。</span><span class="sxs-lookup"><span data-stu-id="8cdab-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="8cdab-133">Visual Studio がインストールされると、新しい MVC アプリケーション プロジェクトを作成するのに、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="8cdab-134">Visual Studio 2103 を開きます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="8cdab-135">をクリックして**新しいプロジェクト**から、**開始** ページで、またはする をクリックして、**ファイル**メニューし**新しいプロジェクト**:</span><span class="sxs-lookup"><span data-stu-id="8cdab-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
 <span data-ttu-id="8cdab-136">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-136">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-137">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="8cdab-138">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**Visual c#** 、テンプレートの一覧でクリックして**Web**を選択し、 **のASP.NETWebアプリケーション**.</span><span class="sxs-lookup"><span data-stu-id="8cdab-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="8cdab-139">プロジェクトの名前を付けます**IdentityMySQLDemo**  をクリックし、 **OK**:</span><span class="sxs-lookup"><span data-stu-id="8cdab-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
 <span data-ttu-id="8cdab-140">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-140">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-141">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="8cdab-142">**新しい ASP.NET プロジェクト**ダイアログで、選択、 **MVC**され、既定のオプションです。 この構成には**個々 のユーザー アカウント**の認証方法として。</span><span class="sxs-lookup"><span data-stu-id="8cdab-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="8cdab-143">をクリックして**OK**:</span><span class="sxs-lookup"><span data-stu-id="8cdab-143">Click **OK**:</span></span>  
  
 <span data-ttu-id="8cdab-144">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-144">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-145">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="8cdab-146">MySQL データベースを使用する EntityFramework を構成します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="8cdab-147">プロジェクトの Entity Framework アセンブリを更新します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="8cdab-148">Visual Studio 2013 のテンプレートから作成された MVC アプリケーションにはへの参照が含まれています、 [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework)をパッケージ化がありますが含まれている更新プログラムのリリース以降には、そのアセンブリへの重要なされてパフォーマンスの向上。</span><span class="sxs-lookup"><span data-stu-id="8cdab-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="8cdab-149">アプリケーションでこれらの最新の更新プログラムを使用するために、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="8cdab-150">Visual Studio 2013 では、MVC プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="8cdab-151">をクリックして**ツール**をクリックし、**ライブラリ パッケージ マネージャー**、順にクリック**パッケージ マネージャー コンソール**:</span><span class="sxs-lookup"><span data-stu-id="8cdab-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
 <span data-ttu-id="8cdab-152">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-152">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-153">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="8cdab-154">**Package Manager Console** Visual Studio の下部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="8cdab-155">型&quot;**更新プログラム パッケージ EntityFramework** &quot; Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
 <span data-ttu-id="8cdab-156">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-156">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-157">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="8cdab-158">EntityFramework の MySQL プロバイダーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="8cdab-159">EntityFramework MySQL のデータベースに接続するためには、MySQL プロバイダーをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="8cdab-160">ためには、開く、**パッケージ マネージャー コンソール**と型&quot; **Install-package MySql.Data.Entity Pre**&quot;、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="8cdab-161">これは、アセンブリのプレリリース版であり、ようバグが含まれること。</span><span class="sxs-lookup"><span data-stu-id="8cdab-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="8cdab-162">実稼働環境でプレリリース バージョンのプロバイダーを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="8cdab-163">[展開の次のイメージをクリックします。]</span><span class="sxs-lookup"><span data-stu-id="8cdab-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="8cdab-164">アプリケーションの Web.config ファイルにプロジェクト構成の変更を加える</span><span class="sxs-lookup"><span data-stu-id="8cdab-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="8cdab-165">Entity Framework をインストールした MySQL プロバイダーを使用して構成するこのセクションでは、MySQL プロバイダー ファクトリを登録し、Azure から、接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="8cdab-166">次の例には、MySql.Data.dll の特定のアセンブリのバージョンが含まれます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="8cdab-167">アセンブリのバージョンが変更された場合は、正しいバージョンで適切な構成設定を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="8cdab-168">Visual Studio 2013 で、プロジェクトの Web.config ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="8cdab-169">次の構成の設定は、Entity Framework 用のデータベースの既定のプロバイダーおよびファクトリの定義を見つけます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="8cdab-170">MySQL プロバイダーを使用する Entity Framework を構成するように、次の構成設定を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="8cdab-171">検索、 &lt;connectionStrings&gt;セクションし、Azure でホストされている MySQL データベースの接続文字列を定義する次のコードに置き換えます (providerName 値がから変更されてもことに注意してください、元の)。</span><span class="sxs-lookup"><span data-stu-id="8cdab-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="8cdab-172">カスタムの MigrationHistory コンテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="8cdab-173">Entity Framework Code First を使用して、 **MigrationHistory**テーブル モデルの変更を追跡して、データベース スキーマと概念スキーマの間の一貫性を確保します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="8cdab-174">ただし、このテーブルは機能しません for MySQL 既定では、主キーが大きすぎるため。</span><span class="sxs-lookup"><span data-stu-id="8cdab-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="8cdab-175">このような状況を解決するには、そのテーブルのキーのサイズを縮小する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="8cdab-176">これを行うには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="8cdab-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="8cdab-177">このテーブルのスキーマ情報をキャプチャ、 **HistoryContext**、その他の変更があります**DbContext**です。</span><span class="sxs-lookup"><span data-stu-id="8cdab-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="8cdab-178">という新しいクラス ファイルを追加するには、 **MySqlHistoryContext.cs**をプロジェクトにその内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="8cdab-179">次に、Entity Framework を使用して、変更を構成する必要は**HistoryContext**、いずれかの既定ではなくです。</span><span class="sxs-lookup"><span data-stu-id="8cdab-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="8cdab-180">これは、コード ベースの構成機能を活用することで実行できます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="8cdab-181">という名前の新しいクラス ファイルを追加するには、 **MySqlConfiguration.cs**をプロジェクトとその内容を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="8cdab-182">ApplicationDbContext のカスタム EntityFramework 初期化子を作成します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="8cdab-183">このチュートリアルで取り上げるは MySQL プロバイダーは、Entity Framework での移行がデータベースに接続するためにモデルの初期化子を使用する必要がありますので現在サポートしていません。</span><span class="sxs-lookup"><span data-stu-id="8cdab-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="8cdab-184">このチュートリアルは、MySQL インスタンスを使用して、Azure では、ため、カスタムの Entity Framework の初期化子を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="8cdab-185">Azure またはオンプレミスでホストされているデータベースを使用しているかどうかに SQL Server インスタンスに接続している場合、この手順は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8cdab-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="8cdab-186">MySQL のカスタム Entity Framework の初期化子を作成するには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="8cdab-187">という新しいクラス ファイルを追加**MySqlInitializer.cs**プロジェクト、および置換には次のコードの内容。</span><span class="sxs-lookup"><span data-stu-id="8cdab-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="8cdab-188">開く、 **IdentityModels.cs**にある、プロジェクト ファイルを**モデル**ディレクトリ、し、次のファイルの内容に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8cdab-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="8cdab-189">アプリケーションを実行して、データベースの検証</span><span class="sxs-lookup"><span data-stu-id="8cdab-189">Running the application and verifying the database</span></span>

<span data-ttu-id="8cdab-190">前のセクションの手順を完了すると、データベースをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="8cdab-191">これを行うには、次の手順に従います。</span><span class="sxs-lookup"><span data-stu-id="8cdab-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="8cdab-192">キーを押して**ctrl キーを押しながら f5 キーを押して**をビルドして、web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="8cdab-193">クリックして、**登録**ページの上部タブ。</span><span class="sxs-lookup"><span data-stu-id="8cdab-193">Click the **Register** tab on the top of the page:</span></span>  
  
 <span data-ttu-id="8cdab-194">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-194">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-195">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="8cdab-196">新しいユーザー名とパスワードを入力し、クリックして**登録**:</span><span class="sxs-lookup"><span data-stu-id="8cdab-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
 <span data-ttu-id="8cdab-197">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-197">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-198">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="8cdab-199">この時点で、ASP.NET Identity テーブルは、MySQL データベースにされ、ユーザーが登録されているし、アプリケーションにログインしています。</span><span class="sxs-lookup"><span data-stu-id="8cdab-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
 <span data-ttu-id="8cdab-200">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-200">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-201">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="8cdab-202">データを検証する MySQL Workbench ツールのインストール</span><span class="sxs-lookup"><span data-stu-id="8cdab-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="8cdab-203">インストール、 **MySQL Workbench**ツールから、 [MySQL がダウンロード ページ](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="8cdab-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="8cdab-204">インストール ウィザード:**機能の選択**  タブで、 **MySQL Workbench** **アプリケーション**セクションです。</span><span class="sxs-lookup"><span data-stu-id="8cdab-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="8cdab-205">アプリを起動し、このチュートリアルの求めたで作成した Azure の MySQL データベース接続文字列データを使用して新しい接続を追加します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="8cdab-206">接続を確立すた後には、検査、 **ASP.NET Identity**で作成されたテーブル、 **IdentityMySQLDatabase です。**</span><span class="sxs-lookup"><span data-stu-id="8cdab-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="8cdab-207">すべての ASP.NET Identity のテーブルには、次の図に示すように、作成に必要なことがわかります。</span><span class="sxs-lookup"><span data-stu-id="8cdab-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
 <span data-ttu-id="8cdab-208">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-208">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-209">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="8cdab-210">検査、 **aspnetusers**テーブルのインスタンスの新しいユーザーを登録すると、エントリを確認します。</span><span class="sxs-lookup"><span data-stu-id="8cdab-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
 <span data-ttu-id="8cdab-211">[展開の次のイメージをクリックします。</span><span class="sxs-lookup"><span data-stu-id="8cdab-211">[Click the following image to expand it.</span></span> <span data-ttu-id="8cdab-212">]</span><span class="sxs-lookup"><span data-stu-id="8cdab-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
