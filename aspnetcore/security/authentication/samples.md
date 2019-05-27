---
title: ASP.NET Core 用の認証のサンプル
author: rick-anderson
description: ASP.NET Core リポジトリでの認証のサンプルへのリンクを提供します。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897159"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="f8d9f-103">ASP.NET Core 用の認証のサンプル</span><span class="sxs-lookup"><span data-stu-id="f8d9f-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="f8d9f-104">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f8d9f-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f8d9f-105">[ASP.NET Core リポジトリ](https://github.com/aspnet/AspNetCore)の*AspNetCore/src/Security/samples*フォルダーには次の通り認証についてのサンプルが含まれています 。</span><span class="sxs-lookup"><span data-stu-id="f8d9f-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="f8d9f-106">クレームの変換</span><span class="sxs-lookup"><span data-stu-id="f8d9f-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="f8d9f-107">Cookie 認証</span><span class="sxs-lookup"><span data-stu-id="f8d9f-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="f8d9f-108">カスタム ポリシー プロバイダー - IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="f8d9f-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="f8d9f-109">動的な認証スキームとオプション</span><span class="sxs-lookup"><span data-stu-id="f8d9f-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="f8d9f-110">外部クレーム</span><span class="sxs-lookup"><span data-stu-id="f8d9f-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="f8d9f-111">要求に基づくCookieと別の認証スキームの選択</span><span class="sxs-lookup"><span data-stu-id="f8d9f-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="f8d9f-112">静的ファイルへのアクセス制限</span><span class="sxs-lookup"><span data-stu-id="f8d9f-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="f8d9f-113">サンプルの実行</span><span class="sxs-lookup"><span data-stu-id="f8d9f-113">Run the samples</span></span>

* <span data-ttu-id="f8d9f-114">[ブランチ](https://github.com/aspnet/AspNetCore)を選択します。</span><span class="sxs-lookup"><span data-stu-id="f8d9f-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="f8d9f-115">例、`release/2.2`</span><span class="sxs-lookup"><span data-stu-id="f8d9f-115">For example, `release/2.2`</span></span>
* <span data-ttu-id="f8d9f-116">[ASP.NET Core リポジトリ](https://github.com/aspnet/AspNetCore)を複製またはダウンロードします。</span><span class="sxs-lookup"><span data-stu-id="f8d9f-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="f8d9f-117">複製したASP.NET Core リポジトリと一致するバージョンの[.NET Core SDK](https://www.microsoft.com/net/download/all)がインストールされていることを確認します。</span><span class="sxs-lookup"><span data-stu-id="f8d9f-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="f8d9f-118">*AspNetCore/src/Security/samples*以下のサンプルに移動し、`dotnet run`でサンプルを実行します。</span><span class="sxs-lookup"><span data-stu-id="f8d9f-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>
