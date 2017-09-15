---
title: "ASP.NET Core でのデータの操作"
author: rick-anderson
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: a8fb7eb7-e0e5-4394-84f3-1f1dbe0ba2ef
ms.technology: aspnet
ms.prod: asp.net-core
ms.openlocfilehash: 3566127476289ae085a9161132b103638bc9b068
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="working-with-data-in-aspnet-core"></a><span data-ttu-id="62fd3-103">ASP.NET Core でのデータの操作</span><span class="sxs-lookup"><span data-stu-id="62fd3-103">Working with Data in ASP.NET Core</span></span> 

*   [<span data-ttu-id="62fd3-104">Visual Studio を使用した ASP.NET Core と Entity Framework Core の概要</span><span class="sxs-lookup"><span data-stu-id="62fd3-104">Getting started with ASP.NET Core and Entity Framework Core using Visual Studio</span></span>](ef-mvc/index.md)
    *   [<span data-ttu-id="62fd3-105">はじめに</span><span class="sxs-lookup"><span data-stu-id="62fd3-105">Getting started</span></span>](ef-mvc/intro.md)
    *   [<span data-ttu-id="62fd3-106">作成、読み取り、更新、削除の操作</span><span class="sxs-lookup"><span data-stu-id="62fd3-106">Create, Read, Update, and Delete operations</span></span>](ef-mvc/crud.md)
    *   [<span data-ttu-id="62fd3-107">並べ替え、フィルター、ページング、グループ化</span><span class="sxs-lookup"><span data-stu-id="62fd3-107">Sorting, filtering, paging, and grouping</span></span>](ef-mvc/sort-filter-page.md)
    *   [<span data-ttu-id="62fd3-108">移行</span><span class="sxs-lookup"><span data-stu-id="62fd3-108">Migrations</span></span>](ef-mvc/migrations.md)
    *   [<span data-ttu-id="62fd3-109">複合データ モデルの作成</span><span class="sxs-lookup"><span data-stu-id="62fd3-109">Creating a complex data model</span></span>](ef-mvc/complex-data-model.md)
    *   [<span data-ttu-id="62fd3-110">関連データの読み取り</span><span class="sxs-lookup"><span data-stu-id="62fd3-110">Reading related data</span></span>](ef-mvc/read-related-data.md)
    *   [<span data-ttu-id="62fd3-111">関連データの更新</span><span class="sxs-lookup"><span data-stu-id="62fd3-111">Updating related data</span></span>](ef-mvc/update-related-data.md)
    *   [<span data-ttu-id="62fd3-112">同時実行の競合の処理</span><span class="sxs-lookup"><span data-stu-id="62fd3-112">Handling concurrency conflicts</span></span>](ef-mvc/concurrency.md)
    *   [<span data-ttu-id="62fd3-113">継承</span><span class="sxs-lookup"><span data-stu-id="62fd3-113">Inheritance</span></span>](ef-mvc/inheritance.md)
    *   [<span data-ttu-id="62fd3-114">高度なトピック</span><span class="sxs-lookup"><span data-stu-id="62fd3-114">Advanced topics</span></span>](ef-mvc/advanced.md)
* <span data-ttu-id="62fd3-115">[ASP.NET Core と EF Core - 新しいデータベース](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core ドキュメント サイト)</span><span class="sxs-lookup"><span data-stu-id="62fd3-115">[ASP.NET Core with EF Core - new database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/new-db) (Entity Framework Core documentation site)</span></span>
* <span data-ttu-id="62fd3-116">[ASP.NET Core と EF Core - 既存のデータベース](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core ドキュメント サイト)</span><span class="sxs-lookup"><span data-stu-id="62fd3-116">[ASP.NET Core with EF Core - existing database](https://docs.microsoft.com/ef/core/get-started/aspnetcore/existing-db) (Entity Framework Core documentation site)</span></span>
*   [<span data-ttu-id="62fd3-117">ASP.NET Core と Entity Framework 6 の概要</span><span class="sxs-lookup"><span data-stu-id="62fd3-117">Getting Started with ASP.NET Core and Entity Framework 6</span></span>](entity-framework-6.md)
*   [<span data-ttu-id="62fd3-118">Azure Storage</span><span class="sxs-lookup"><span data-stu-id="62fd3-118">Azure Storage</span></span>](azure-storage/index.md)
    *   [<span data-ttu-id="62fd3-119">Visual Studio の接続済みサービスを使用した Azure Storage の追加</span><span class="sxs-lookup"><span data-stu-id="62fd3-119">Adding Azure Storage by Using Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-azure-tools-connected-services-storage/)
    *   [<span data-ttu-id="62fd3-120">Azure Blob Storage と Visual Studio の接続済みサービスの概要</span><span class="sxs-lookup"><span data-stu-id="62fd3-120">Get Started with Azure Blob storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-blobs/)
    *   [<span data-ttu-id="62fd3-121">Queue Storage と Visual Studio の接続済みサービスの概要</span><span class="sxs-lookup"><span data-stu-id="62fd3-121">Get Started with Queue Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-queues/)
    *   [<span data-ttu-id="62fd3-122">Azure Table Storage と Visual Studio の接続済みサービスの概要</span><span class="sxs-lookup"><span data-stu-id="62fd3-122">How to Get Started with Azure Table Storage and Visual Studio Connected Services</span></span>](https://azure.microsoft.com/documentation/articles/vs-storage-aspnet5-getting-started-tables/)
