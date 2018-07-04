---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'パート 2: データ アクセス層 |Microsoft Docs'
author: JoeStagner
description: このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。 第 2 部では、データ アクセス層を追加することについて説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 13abb02e76b3af80aa11d09e75dc223403917804
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382570"
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="1293a-104">パート 2: データ アクセス層</span><span class="sxs-lookup"><span data-stu-id="1293a-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="1293a-105">によって[Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="1293a-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="1293a-106">Tailspin Spyworks では、.NET プラットフォーム用の強力でスケーラブルなアプリケーションを作成するはどの非常に単純なを示します。</span><span class="sxs-lookup"><span data-stu-id="1293a-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="1293a-107">ASP.NET 4 の優れた新機能を使用して、ショッピング、チェック アウト、および管理を含む、オンライン ストアを構築する方法を示します。</span><span class="sxs-lookup"><span data-stu-id="1293a-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="1293a-108">このチュートリアル シリーズでは、すべての Tailspin Spyworks サンプル アプリケーションをビルドする手順について説明します。</span><span class="sxs-lookup"><span data-stu-id="1293a-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="1293a-109">第 2 部では、データ アクセス層を追加することについて説明します。</span><span class="sxs-lookup"><span data-stu-id="1293a-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="1293a-110">データ アクセス層を追加します。</span><span class="sxs-lookup"><span data-stu-id="1293a-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="1293a-111">E コマース アプリケーションは、2 つのデータベースに左右されます。</span><span class="sxs-lookup"><span data-stu-id="1293a-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="1293a-112">顧客は、標準の ASP.NET メンバーシップ データベースを使用します。</span><span class="sxs-lookup"><span data-stu-id="1293a-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="1293a-113">ショッピング カート、製品カタログの SQL Express データベースを次のように実装します。</span><span class="sxs-lookup"><span data-stu-id="1293a-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="1293a-114">アプリケーションのアプリ内でのデータベース (Commerce.mdf) を作成しなくて\_データ フォルダーが、.NET Entity Framework を使用して、データ アクセス層の作成に進むことができます。</span><span class="sxs-lookup"><span data-stu-id="1293a-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="1293a-115">という名前のフォルダーを作成します"データ\_アクセス"し、そのフォルダーを右クリックし、「新しい項目の追加」を選択します。</span><span class="sxs-lookup"><span data-stu-id="1293a-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="1293a-116">「インストールされたテンプレート」項目と選択し、"ADO.NET Entity Data Model"を入力 EDM\_Commerce.edmx 名前として、[追加] ボタンをクリックします。</span><span class="sxs-lookup"><span data-stu-id="1293a-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="1293a-117">「データベースから生成」を選択します。</span><span class="sxs-lookup"><span data-stu-id="1293a-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="1293a-118">保存し、ビルドします。</span><span class="sxs-lookup"><span data-stu-id="1293a-118">Save and build.</span></span>

<span data-ttu-id="1293a-119">これで、最初の機能 – 製品カテゴリ メニューに追加する準備が整いました。</span><span class="sxs-lookup"><span data-stu-id="1293a-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="1293a-120">[前へ](tailspin-spyworks-part-1.md)
> [次へ](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="1293a-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
