---
uid: identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
title: 'ASP.NET Identity: EntityFramework MySQL プロバイダーでは (c#) での MySQL の Storage の使用 |Microsoft Docs'
author: maumar
description: このチュートリアルでは、MySQL または EntityFramework (SQL クライアント プロバイダー) で ASP.NET Identity の既定のデータ ストレージ機構を置き換える方法を紹介しています.
ms.author: riande
ms.date: 12/10/2013
ms.assetid: 15253312-a92c-43ba-908e-b5dacd3d08b8
msc.legacyurl: /identity/overview/getting-started/aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider
msc.type: authoredcontent
ms.openlocfilehash: 3856b40b31a3deb6ad1c6c5d2cd678e183f012d7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41837440"
---
<a name="aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider-c"></a><span data-ttu-id="8ab4e-103">ASP.NET Identity: EntityFramework MySQL プロバイダー (c#) で MySQL ストレージを使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-103">ASP.NET Identity: Using MySQL Storage with an EntityFramework MySQL Provider (C#)</span></span>
====================
<span data-ttu-id="8ab4e-104">によって[Maurycy Markowski](https://github.com/maumar)、 [Raquel Soares De Almeida](https://github.com/raquelsa)、 [Robert McMurray](https://github.com/rmcmurray)</span><span class="sxs-lookup"><span data-stu-id="8ab4e-104">by [Maurycy Markowski](https://github.com/maumar), [Raquel Soares De Almeida](https://github.com/raquelsa), [Robert McMurray](https://github.com/rmcmurray)</span></span>

> <span data-ttu-id="8ab4e-105">このチュートリアルでは、既定のデータ ストレージ メカニズムを置き換える方法[ **ASP.NET Identity** ](introduction-to-aspnet-identity.md) (SQL クライアント プロバイダー) を EntityFramework MySQL プロバイダーを使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-105">This tutorial shows you how to replace the default data storage mechanism for [**ASP.NET Identity**](introduction-to-aspnet-identity.md) with EntityFramework (SQL client provider) with a MySQL provider.</span></span>


<span data-ttu-id="8ab4e-106">このチュートリアルでは、次のトピックを取り上げます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-106">The following topics will be covered in this tutorial:</span></span>

- <span data-ttu-id="8ab4e-107">Azure で MySQL データベースの作成</span><span class="sxs-lookup"><span data-stu-id="8ab4e-107">Creating a MySQL database on Azure</span></span>
- <span data-ttu-id="8ab4e-108">Visual Studio 2013 の MVC テンプレートを使用して MVC アプリケーションを作成します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-108">Creating an MVC application using Visual Studio 2013 MVC template</span></span>
- <span data-ttu-id="8ab4e-109">MySQL データベース プロバイダーを使用する entity Framework を構成します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-109">Configuring EntityFramework to work with a MySQL database provider</span></span>
- <span data-ttu-id="8ab4e-110">結果を確認するアプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-110">Running the application to verify the results</span></span>

<span data-ttu-id="8ab4e-111">このチュートリアルの終わりには、必要があります格納 ASP.NET Identity と MVC アプリケーションが Azure でホストされている MySQL データベースを使用しています。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-111">At the end of this tutorial, you will have an MVC application with the ASP.NET Identity store that is using a MySQL database that is hosted in Azure.</span></span>

## <a name="creating-a-mysql-database-instance-on-azure"></a><span data-ttu-id="8ab4e-112">Azure で MySQL データベース インスタンスを作成します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-112">Creating a MySQL database instance on Azure</span></span>

1. <span data-ttu-id="8ab4e-113">[Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409) にログインします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-113">Log in to the [Azure Portal](https://go.microsoft.com/fwlink/?linkid=529715&amp;clcid=0x409).</span></span>
2. <span data-ttu-id="8ab4e-114">クリックして**新規**クリックして、ページの下部にある**ストア**:</span><span class="sxs-lookup"><span data-stu-id="8ab4e-114">Click **NEW** at the bottom of the page, and then select **STORE**:</span></span>  
  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.png)
3. <span data-ttu-id="8ab4e-115">**アドオンと選択**ウィザードで、 **ClearDB MySQL Database**、順にクリックします、**次**フレームの下部にある矢印。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-115">In the **Choose and Add-on** wizard, select **ClearDB MySQL Database**, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="8ab4e-116">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-116">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-117">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-117">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.png)
4. <span data-ttu-id="8ab4e-118">既定値を保持**Free**計画、変更、**名前**に**IdentityMySQLDatabase**は、最も近いリージョンを選択し、クリックして、 **次へ** 、フレームの下部にある矢印。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-118">Keep the default **Free** plan, change the **NAME** to **IdentityMySQLDatabase**, select the region that is nearest to you, and then click the **Next** arrow at the bottom of the frame:</span></span>  
  
   <span data-ttu-id="8ab4e-119">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-119">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-120">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-120">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.png)
5. <span data-ttu-id="8ab4e-121">をクリックして、**購入**チェック マークをデータベースの作成を完了します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-121">Click the **PURCHASE** checkmark to complete the database creation.</span></span>  
  
   <span data-ttu-id="8ab4e-122">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-122">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-123">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-123">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.png)
6. <span data-ttu-id="8ab4e-124">データベースが作成されたら後からを管理できます、**アドオン** タブで、管理ポータル。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-124">After your database has been created, you can manage it from the **ADD-ONS** tab in the management portal.</span></span> <span data-ttu-id="8ab4e-125">データベースの接続情報を取得するには、次のようにクリックします。**接続情報**ページの下部にあります。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-125">To retrieve the connection information for your database, click **CONNECTION INFO** at the bottom of the page:</span></span>  
  
   <span data-ttu-id="8ab4e-126">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-126">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-127">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-127">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image10.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image9.png)
7. <span data-ttu-id="8ab4e-128">コピー ボタンをクリックして、接続文字列をコピー、 **CONNECTIONSTRING** ; フィールドを付けて保存、MVC アプリケーションにこのチュートリアルの後半では、この情報を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-128">Copy the connection string by clicking on the copy button by the **CONNECTIONSTRING** field and save it; you will use this information later in this tutorial for your MVC application:</span></span>  
  
   <span data-ttu-id="8ab4e-129">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-129">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-130">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-130">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image12.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image11.png)

## <a name="creating-an-mvc-application-project"></a><span data-ttu-id="8ab4e-131">MVC アプリケーション プロジェクトを作成します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-131">Creating an MVC application project</span></span>

<span data-ttu-id="8ab4e-132">このチュートリアルのこのセクションで手順を完了する必要がありますをインストールする[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)または[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-132">To complete the steps in this section of the tutorial, you will first need to install [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="8ab4e-133">Visual Studio がインストールされると、新しい MVC アプリケーション プロジェクトを作成するのに、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-133">Once Visual Studio has been installed, use the following steps to create a new MVC application project:</span></span>

1. <span data-ttu-id="8ab4e-134">Visual Studio 2103 を開きます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-134">Open Visual Studio 2103.</span></span>
2. <span data-ttu-id="8ab4e-135">クリックして**新しいプロジェクト**から、**開始** ページで、またはする をクリックして、**ファイル**メニューをクリックし**新しいプロジェクト**:</span><span class="sxs-lookup"><span data-stu-id="8ab4e-135">Click **New Project** from the **Start** page, or you can click the **File** menu and then **New Project**:</span></span>  
  
   <span data-ttu-id="8ab4e-136">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-136">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-137">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-137">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image2.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image1.jpg)
3. <span data-ttu-id="8ab4e-138">ときに、**新しいプロジェクト** ダイアログ ボックスが表示されたら、展開**Visual c#** テンプレートの一覧でをクリックし、 **Web**、選び**のASP.NETWebアプリケーション**.</span><span class="sxs-lookup"><span data-stu-id="8ab4e-138">When the **New Project** dialog box is displayed, expand **Visual C#** in the list of templates, then click **Web**, and select **ASP.NET Web Application**.</span></span> <span data-ttu-id="8ab4e-139">プロジェクトに名前を**IdentityMySQLDemo**し**OK**:</span><span class="sxs-lookup"><span data-stu-id="8ab4e-139">Name your project **IdentityMySQLDemo** and then click **OK**:</span></span>  
  
   <span data-ttu-id="8ab4e-140">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-140">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-141">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-141">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image14.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image13.png)
4. <span data-ttu-id="8ab4e-142">**新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **MVC**され、既定のオプションはこの構成では**個々 のユーザー アカウント**認証方法として。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-142">In the **New ASP.NET Project** dialog, select the **MVC** templatewith the default options; this will configure **Individual User Accounts** as the authentication method.</span></span> <span data-ttu-id="8ab4e-143">**[OK]** をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-143">Click **OK**:</span></span>  
  
   <span data-ttu-id="8ab4e-144">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-144">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-145">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-145">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image16.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image15.png)

## <a name="configure-entityframework-to-work-with-a-mysql-database"></a><span data-ttu-id="8ab4e-146">MySQL データベースを使用する entity Framework を構成します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-146">Configure EntityFramework to work with a MySQL database</span></span>

### <a name="update-the-entity-framework-assembly-for-your-project"></a><span data-ttu-id="8ab4e-147">プロジェクトの Entity Framework のアセンブリを更新します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-147">Update the Entity Framework assembly for your project</span></span>

<span data-ttu-id="8ab4e-148">Visual Studio 2013 のテンプレートから作成された MVC アプリケーションにはへの参照が含まれています、 [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework)をパッケージ化がありますを含む更新プログラムのリリース以来、そのアセンブリへの大幅なされましたパフォーマンスの向上。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-148">The MVC application that was created from the Visual Studio 2013 template contains a reference to the [EntityFramework 6.0.0](http://www.nuget.org/packages/EntityFramework) package, but there have been updates to that assembly since its release which contain significant performance improvements.</span></span> <span data-ttu-id="8ab4e-149">アプリケーションでこれらの最新の更新プログラムを使用するには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-149">In order to use these latest updates in your application, use the following steps.</span></span>

1. <span data-ttu-id="8ab4e-150">Visual Studio 2013 では、MVC プロジェクトを開きます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-150">Open your MVC project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="8ab4e-151">をクリックして**ツール**、 をクリックし、**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**:</span><span class="sxs-lookup"><span data-stu-id="8ab4e-151">Click **TOOLS**, then click **Library Package Manager**, and then click **Package Manager Console**:</span></span>  
  
   <span data-ttu-id="8ab4e-152">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-152">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-153">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-153">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image18.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image17.png)
3. <span data-ttu-id="8ab4e-154">**パッケージ マネージャー コンソール**Visual Studio の下部に表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-154">The **Package Manager Console** will appear in the bottom section of Visual Studio.</span></span> <span data-ttu-id="8ab4e-155">型&quot; **Update-package EntityFramework** &quot; Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-155">Type &quot;**Update-Package EntityFramework**&quot; and press Enter:</span></span>  
  
   <span data-ttu-id="8ab4e-156">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-156">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-157">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-157">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image20.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image19.png)

### <a name="install-the-mysql-provider-for-entityframework"></a><span data-ttu-id="8ab4e-158">EntityFramework MySQL プロバイダーをインストールします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-158">Install the MySQL provider for EntityFramework</span></span>

<span data-ttu-id="8ab4e-159">EntityFramework MySQL database に接続するためには、MySQL プロバイダーをインストールする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-159">In order for EntityFramework to connect to MySQL database, you need to install a MySQL provider.</span></span> <span data-ttu-id="8ab4e-160">これを行うには、開く、**パッケージ マネージャー コンソール**と種類&quot; **Install-package MySql.Data.Entity Pre**&quot;、し、Enter キーを押します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-160">To do so, open the **Package Manager Console** and type &quot;**Install-Package MySql.Data.Entity -Pre**&quot;, and then press Enter.</span></span>

> [!NOTE]
> <span data-ttu-id="8ab4e-161">これは、アセンブリのプレリリース版であり、バグを含めることができますようです。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-161">This is a pre-release version of the assembly, and as such it may contain bugs.</span></span> <span data-ttu-id="8ab4e-162">実稼働環境では、プレリリース バージョンのプロバイダーを使用する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-162">You should not use a pre-release version of the provider in production.</span></span>


<span data-ttu-id="8ab4e-163">[展開の次の画像をクリックします。]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-163">[Click the following image to expand it.]</span></span>  
  
[![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image22.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image21.png)

### <a name="making-project-configuration-changes-to-the-webconfig-file-for-your-application"></a><span data-ttu-id="8ab4e-164">アプリケーションの Web.config ファイルをプロジェクト構成を変更</span><span class="sxs-lookup"><span data-stu-id="8ab4e-164">Making project configuration changes to the Web.config file for your application</span></span>

<span data-ttu-id="8ab4e-165">このセクションでは、インストールした MySQL プロバイダーを使用する Entity Framework を構成するでは、MySQL プロバイダー ファクトリを登録し、Azure から接続文字列を追加します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-165">In this section you will configure the Entity Framework to use the MySQL provider that you just installed, register the MySQL provider factory, and add your connection string from Azure.</span></span>

> [!NOTE]
> <span data-ttu-id="8ab4e-166">次の例には、MySql.Data.dll の特定のアセンブリのバージョンが含まれています。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-166">The following examples contain a specific assembly version for MySql.Data.dll.</span></span> <span data-ttu-id="8ab4e-167">アセンブリのバージョンが変更された場合は、適切なバージョンを使用して、適切な構成設定を変更する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-167">If the assembly version changes, you will need to modify the appropriate configuration settings with the correct version.</span></span>


1. <span data-ttu-id="8ab4e-168">Visual Studio 2013 で、プロジェクトの Web.config ファイルを開きます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-168">Open the Web.config file for your project in Visual Studio 2013.</span></span>
2. <span data-ttu-id="8ab4e-169">次の構成の設定は、Entity Framework の既定のデータベース プロバイダーとファクトリ定義を見つけます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-169">Locate the following configuration settings, which define the default database provider and factory for the Entity Framework:</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample1.xml)]
3. <span data-ttu-id="8ab4e-170">MySQL プロバイダーを使用する Entity Framework を構成するように、次の構成設定を置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-170">Replace those configuration settings with the following, which will configure the Entity Framework to use the MySQL provider:</span></span> 

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample2.xml)]
4. <span data-ttu-id="8ab4e-171">検索、 &lt;connectionStrings&gt;セクションし、Azure でホストされている MySQL データベースの接続文字列を定義する次のコードに置き換えます (providerName 値がから変更されてもことに注意してください、元の)。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-171">Locate the &lt;connectionStrings&gt; section and replace it with the following code, which will define the connection string for your MySQL database that is hosted on Azure (note that providerName value has also been changed from the original):</span></span>

    [!code-xml[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample3.xml?highlight=3-4)]

### <a name="adding-custom-migrationhistory-context"></a><span data-ttu-id="8ab4e-172">カスタムの MigrationHistory コンテキストを追加します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-172">Adding custom MigrationHistory context</span></span>

<span data-ttu-id="8ab4e-173">Entity Framework Code First を使用して、 **MigrationHistory**テーブル モデルの変更を追跡して、データベース スキーマと概念スキーマの一貫性を確保します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-173">Entity Framework Code First uses a **MigrationHistory** table to keep track of model changes and to ensure the consistency between the database schema and conceptual schema.</span></span> <span data-ttu-id="8ab4e-174">ただし、このテーブルは使えません MySQL 既定では、主キーが大きすぎるため。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-174">However, this table does not work for MySQL by default because the primary key is too large.</span></span> <span data-ttu-id="8ab4e-175">このような状況を解決するにはそのテーブルのキーのサイズを縮小する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-175">To remedy this situation, you will need to shrink the key size for that table.</span></span> <span data-ttu-id="8ab4e-176">これを行うには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-176">To do so, use the following steps:</span></span>

1. <span data-ttu-id="8ab4e-177">このテーブルのスキーマ情報がキャプチャされた、 **HistoryContext**とその他の変更される**DbContext**します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-177">The schema information for this table is captured in a **HistoryContext**, which can be modified as any other **DbContext**.</span></span> <span data-ttu-id="8ab4e-178">これを行うには、という名前の新しいクラス ファイルを追加**MySqlHistoryContext.cs**をプロジェクトにし、その内容を次のコードに置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-178">To do so, add a new class file named **MySqlHistoryContext.cs** to the project, and replace its contents with the following code:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample4.cs)]
2. <span data-ttu-id="8ab4e-179">次に、Entity Framework を使用して、変更された構成する必要が**HistoryContext**既定のものではなく。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-179">Next you will need to configure Entity Framework to use the modified **HistoryContext**, rather than default one.</span></span> <span data-ttu-id="8ab4e-180">これは、コード ベースの構成機能を活用することで実行できます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-180">This can be done by leveraging code-based configuration features.</span></span> <span data-ttu-id="8ab4e-181">これを行うには、という名前の新しいクラス ファイルを追加**MySqlConfiguration.cs**をプロジェクトに、内容に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-181">To do so, add new class file named **MySqlConfiguration.cs** to your project and replace its contents with:</span></span>

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample5.cs)]

### <a name="creating-a-custom-entityframework-initializer-for-applicationdbcontext"></a><span data-ttu-id="8ab4e-182">ApplicationDbContext のカスタム EntityFramework の初期化子を作成します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-182">Creating a custom EntityFramework initializer for ApplicationDbContext</span></span>

<span data-ttu-id="8ab4e-183">このチュートリアルでは機能を備えた MySQL プロバイダーは、Entity Framework の移行がデータベースに接続するためにモデルの初期化子を使用する必要があります現在サポートしていません。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-183">The MySQL provider that is featured in this tutorial does not currently support Entity Framework migrations, so you will need to use model initializers in order to connect to the database.</span></span> <span data-ttu-id="8ab4e-184">このチュートリアルは Azure で MySQL インスタンスを使用しているため、カスタムの Entity Framework の初期化子を作成する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-184">Because this tutorial is using a MySQL instance on Azure, you will need to create a custom Entity Framework initializer.</span></span>

> [!NOTE]
> <span data-ttu-id="8ab4e-185">Azure またはオンプレミスでホストされているデータベースを使用しているかどうかに SQL Server インスタンスに接続している場合、この手順は必要ありません。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-185">This step is not required if you are connecting to a SQL Server instance on Azure or if you are using a database that is hosted on premises.</span></span>


<span data-ttu-id="8ab4e-186">Mysql カスタム Entity Framework の初期化子を作成するには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-186">To create a custom Entity Framework initializer for MySQL, use the following steps:</span></span>

1. <span data-ttu-id="8ab4e-187">という名前の新しいクラス ファイルを追加**MySqlInitializer.cs**内容を次のコードは、プロジェクト、および置換します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-187">Add a new class file named **MySqlInitializer.cs** to the project, and replace it's contents with the following code:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample6.cs?highlight=23)]
2. <span data-ttu-id="8ab4e-188">開く、 **IdentityModels.cs**にあるプロジェクトのファイル、**モデル**ディレクトリに、次の内容に置き換えます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-188">Open the **IdentityModels.cs** file for your project, which is located in the **Models** directory, and replace it's contents with the following:</span></span> 

    [!code-csharp[Main](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/samples/sample7.cs)]

## <a name="running-the-application-and-verifying-the-database"></a><span data-ttu-id="8ab4e-189">アプリケーションを実行して、データベースの検証</span><span class="sxs-lookup"><span data-stu-id="8ab4e-189">Running the application and verifying the database</span></span>

<span data-ttu-id="8ab4e-190">前のセクションで手順を完了すると後、データベースをテストする必要があります。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-190">Once you have completed the steps in the preceding sections, you should test your database.</span></span> <span data-ttu-id="8ab4e-191">これを行うには、次の手順を使用します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-191">To do so, use the following steps:</span></span>

1. <span data-ttu-id="8ab4e-192">キーを押して**ctrl キーを押しながら f5 キーを押して**をビルドして、web アプリケーションを実行します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-192">Press **Ctrl + F5** to build and run the web application.</span></span>
2. <span data-ttu-id="8ab4e-193">をクリックして、**登録**ページ上部にあるタブ。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-193">Click the **Register** tab on the top of the page:</span></span>  
  
   <span data-ttu-id="8ab4e-194">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-194">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-195">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-195">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image4.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image3.jpg)
3. <span data-ttu-id="8ab4e-196">新しいユーザー名とパスワードを入力し、クリックして**登録**:</span><span class="sxs-lookup"><span data-stu-id="8ab4e-196">Enter a new user name and password, and then click **Register**:</span></span>  
  
   <span data-ttu-id="8ab4e-197">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-197">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-198">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-198">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image24.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image23.png)
4. <span data-ttu-id="8ab4e-199">この時点で ASP.NET Identity のテーブルが、MySQL データベースを作成し、ユーザーが登録されているし、アプリケーションにログインしています。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-199">At this point the ASP.NET Identity tables are created on the MySQL Database, and the user is registered and logged into the application:</span></span>  
  
   <span data-ttu-id="8ab4e-200">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-200">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-201">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-201">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image6.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image5.jpg)

### <a name="installing-mysql-workbench-tool-to-verify-the-data"></a><span data-ttu-id="8ab4e-202">データを確認するツールを MySQL Workbench のインストール</span><span class="sxs-lookup"><span data-stu-id="8ab4e-202">Installing MySQL Workbench tool to verify the data</span></span>

1. <span data-ttu-id="8ab4e-203">インストール、 **MySQL Workbench**ツールから、 [MySQL のダウンロード ページ](http://dev.mysql.com/downloads/windows/installer/)</span><span class="sxs-lookup"><span data-stu-id="8ab4e-203">Install the **MySQL Workbench** tool from the [MySQL downloads page](http://dev.mysql.com/downloads/windows/installer/)</span></span>
2. <span data-ttu-id="8ab4e-204">インストール ウィザード:**機能の選択** タブで  **MySQL Workbench** **アプリケーション**セクション。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-204">In the installation wizard: **Feature Selection** tab, select **MySQL Workbench** under **applications** section.</span></span>
3. <span data-ttu-id="8ab4e-205">アプリを起動し、このチュートリアルの求めに作成した Azure MySQL データベースからの接続文字列データを使用して新しい接続を追加します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-205">Launch the app and add a new connection using the connection string data from the Azure MySQL database you created at the begging of this tutorial.</span></span>
4. <span data-ttu-id="8ab4e-206">接続を確立した後は、検査、 **ASP.NET Identity**で作成されたテーブル、 **IdentityMySQLDatabase します。**</span><span class="sxs-lookup"><span data-stu-id="8ab4e-206">After establishing the connection, inspect the **ASP.NET Identity** tables created on the **IdentityMySQLDatabase.**</span></span>
5. <span data-ttu-id="8ab4e-207">次の図に示すように、テーブルが作成されたすべての ASP.NET Identity が必要なことが表示されます。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-207">You will see that all ASP.NET Identity required tables are created as shown in the image below:</span></span>  
  
   <span data-ttu-id="8ab4e-208">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-208">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-209">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-209">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image8.jpg)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image7.jpg)
6. <span data-ttu-id="8ab4e-210">検査、 **aspnetusers**インスタンスのテーブルに新しいユーザーを登録すると、エントリを確認します。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-210">Inspect the **aspnetusers** table for instance to check for the entries as you register new users.</span></span>  
  
   <span data-ttu-id="8ab4e-211">[展開の次の画像をクリックします。</span><span class="sxs-lookup"><span data-stu-id="8ab4e-211">[Click the following image to expand it.</span></span> <span data-ttu-id="8ab4e-212">]</span><span class="sxs-lookup"><span data-stu-id="8ab4e-212">]</span></span>  
    [![](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image26.png)](aspnet-identity-using-mysql-storage-with-an-entityframework-mysql-provider/_static/image25.png)
