---
title: "ASP.NET Core 1.1 の新機能"
author: rick-anderson
description: "ASP.NET Core 1.1 の新機能"
keywords: ASP.NET Core, bower
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 062f8353-d1bc-4e99-a821-c1d1bb162c47
ms.technology: aspnet
ms.prod: aspnet-core
uid: aspnetcore-1.1
ms.openlocfilehash: f014bdb41447f0354938c719ab37104b2d4b08e5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="whats-new-in-aspnet-core-11"></a><span data-ttu-id="0ee50-104">ASP.NET Core 1.1 の新機能</span><span class="sxs-lookup"><span data-stu-id="0ee50-104">What's new in ASP.NET Core 1.1</span></span>

<span data-ttu-id="0ee50-105">ASP.NET Core 1.1 には次の新機能があります。</span><span class="sxs-lookup"><span data-stu-id="0ee50-105">ASP.NET Core 1.1 includes the following new features:</span></span>

- [<span data-ttu-id="0ee50-106">URL リライト ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="0ee50-106">URL Rewriting Middleware</span></span>](xref:fundamentals/url-rewriting)
- [<span data-ttu-id="0ee50-107">応答キャッシュ ミドルウェア</span><span class="sxs-lookup"><span data-stu-id="0ee50-107">Response Caching Middleware</span></span>](xref:performance/caching/middleware)
- [<span data-ttu-id="0ee50-108">タグ ヘルパーとしてのビュー コンポーネント</span><span class="sxs-lookup"><span data-stu-id="0ee50-108">View Components as Tag Helpers</span></span>](xref:mvc/views/view-components#invoking-a-view-component-as-a-tag-helper)
- [<span data-ttu-id="0ee50-109">MVC フィルターとしてのミドルウェア</span><span class="sxs-lookup"><span data-stu-id="0ee50-109">Middleware as MVC filters</span></span>](xref:mvc/controllers/filters#using-middleware-in-the-filter-pipeline)
- [<span data-ttu-id="0ee50-110">Cookie ベースの TempData プロバイダー</span><span class="sxs-lookup"><span data-stu-id="0ee50-110">Cookie-based TempData provider</span></span>](xref:fundamentals/app-state#tempdata)
- [<span data-ttu-id="0ee50-111">Azure App Service ログ プロバイダー</span><span class="sxs-lookup"><span data-stu-id="0ee50-111">Azure App Service logging provider</span></span>](xref:fundamentals/logging/index#appservice)
- [<span data-ttu-id="0ee50-112">Azure Key Vault 構成プロバイダー</span><span class="sxs-lookup"><span data-stu-id="0ee50-112">Azure Key Vault configuration provider</span></span>](xref:security/key-vault-configuration)
- [<span data-ttu-id="0ee50-113">Azure と Redis のストレージ データ保護キー リポジトリ</span><span class="sxs-lookup"><span data-stu-id="0ee50-113">Azure and Redis Storage Data Protection Key Repositories</span></span>](xref:security/data-protection/implementation/key-storage-providers#azure-and-redis)
- [<span data-ttu-id="0ee50-114">Windows 用 WebListener Server</span><span class="sxs-lookup"><span data-stu-id="0ee50-114">WebListener Server for Windows</span></span>](xref:fundamentals/servers/weblistener)
- [<span data-ttu-id="0ee50-115">WebSocket のサポート</span><span class="sxs-lookup"><span data-stu-id="0ee50-115">WebSockets support</span></span>](xref:fundamentals/websockets)

## <a name="choosing-between-versions-10-and-11-of-aspnet-core"></a><span data-ttu-id="0ee50-116">ASP.NET Core のバージョン 1.0 と 1.1 の選択</span><span class="sxs-lookup"><span data-stu-id="0ee50-116">Choosing between versions 1.0 and 1.1 of ASP.NET Core</span></span>

<span data-ttu-id="0ee50-117">ASP.NET Core 1.1 には 1.0 より多くの機能があります。</span><span class="sxs-lookup"><span data-stu-id="0ee50-117">ASP.NET Core 1.1 has more features than 1.0.</span></span> <span data-ttu-id="0ee50-118">一般に、最新バージョンを使うことをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="0ee50-118">In general, we recommend you use the latest version.</span></span>

## <a name="additional-information"></a><span data-ttu-id="0ee50-119">追加情報</span><span class="sxs-lookup"><span data-stu-id="0ee50-119">Additional Information</span></span>

- [<span data-ttu-id="0ee50-120">ASP.NET Core 1.1.0 リリース ノート</span><span class="sxs-lookup"><span data-stu-id="0ee50-120">ASP.NET Core 1.1.0 Release Notes</span></span>](https://github.com/aspnet/Home/releases/tag/1.1.0)
- <span data-ttu-id="0ee50-121">ASP.NET Core 開発チームの進捗状況や計画とつながるには、週次の [ASP.NET Community Standup](https://live.asp.net/) にアクセスしてください。</span><span class="sxs-lookup"><span data-stu-id="0ee50-121">If you’d like to connect with the ASP.NET Core development team’s progress and plans, tune in to the weekly [ASP.NET Community Standup](https://live.asp.net/).</span></span>
