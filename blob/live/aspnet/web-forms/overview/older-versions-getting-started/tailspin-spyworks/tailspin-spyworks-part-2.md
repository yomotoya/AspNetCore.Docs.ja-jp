---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: "パート 2: データ アクセス層 |Microsoft ドキュメント"
author: JoeStagner
description: "このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、データ アクセス レイヤーの追加について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 8b07b320640c1bb0074a4d3a04ca7c5b7e7bb6cd
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="5ce9e-104">パート 2: データ アクセス層</span><span class="sxs-lookup"><span data-stu-id="5ce9e-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="5ce9e-105">によって[行える](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="5ce9e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="5ce9e-106">Tailspin Spyworks では、.NET プラットフォーム用の強力な拡張性の高いアプリケーションを作成するはどのように非常に単純なを示しています。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="5ce9e-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアをビルドする方法を示します。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="5ce9e-108">このチュートリアルの系列では、すべて Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="5ce9e-109">第 2 部では、データ アクセス レイヤーの追加について説明します。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a><span data-ttu-id="5ce9e-110">データ アクセス レイヤーの追加</span><span class="sxs-lookup"><span data-stu-id="5ce9e-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="5ce9e-111">電子商取引アプリケーションは、2 つのデータベースに左右されます。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="5ce9e-112">顧客情報は、標準の ASP.NET メンバーシップ データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="5ce9e-113">弊社のショッピング カートや製品カタログのおを実装 SQL Express データベースには、次のようにします。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="5ce9e-114">データベース (Commerce.mdf) アプリケーションのアプリで作成\_データ フォルダーを続けていく中 .NET Entity Framework を使用して、データ アクセス レイヤーを作成することができます。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="5ce9e-115">という名前のフォルダーを作成します"データ\_アクセス"し、そのフォルダーを右クリックし、「新しい項目の追加」を選択します。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="5ce9e-116">「インストールされたテンプレート」の項目と選択し、「ADO.NET エンティティ データ モデル」入力 EDM\_Commerce.edmx 名として「追加」ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="5ce9e-117">「データベースから生成」を選択します。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="5ce9e-118">保存し、ビルドします。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-118">Save and build.</span></span>

<span data-ttu-id="5ce9e-119">これで、最初の機能: 製品カテゴリ メニューを追加する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="5ce9e-119">Now we are ready to add our first feature – a product category menu.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="5ce9e-120">[前へ](tailspin-spyworks-part-1.md)
[次へ](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="5ce9e-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
