---
title: "Id の主キー データの種類を構成します。"
author: AdrienTorris
description: "この記事では、ASP.NET Core Id 主キーの使用目的のデータ型を構成する手順について説明します。"
keywords: "ASP.NET Core、Identity、主キー"
ms.author: scaddie
manager: wpickett
ms.date: 09/28/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 5734a9aa86fb2831bd054593ad41c3e3bda4729e
ms.sourcegitcommit: 13291956ad858d465dfd46caa384df58f08e286b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2017
---
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a><span data-ttu-id="c037b-104">ASP.NET Core Id の主キー データの種類を構成します。</span><span class="sxs-lookup"><span data-stu-id="c037b-104">Configure the ASP.NET Core Identity primary key data type</span></span>

<span data-ttu-id="c037b-105">ASP.NET Core の Id では、主キーを表すために使用するデータの種類を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="c037b-105">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="c037b-106">Id は、`string`既定ではデータ型。</span><span class="sxs-lookup"><span data-stu-id="c037b-106">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="c037b-107">この動作をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="c037b-107">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="c037b-108">主キー データの種類をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="c037b-108">Customize the primary key data type</span></span>

1. <span data-ttu-id="c037b-109">カスタム実装を作成、 [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1)クラスです。</span><span class="sxs-lookup"><span data-stu-id="c037b-109">Create a custom implementation of the [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="c037b-110">ユーザー オブジェクトの作成に使用する種類を表します。</span><span class="sxs-lookup"><span data-stu-id="c037b-110">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="c037b-111">次の例では、既定値で`string`型に置き換えられる`Guid`です。</span><span class="sxs-lookup"><span data-stu-id="c037b-111">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. <span data-ttu-id="c037b-112">カスタム実装を作成、 [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1)クラスです。</span><span class="sxs-lookup"><span data-stu-id="c037b-112">Create a custom implementation of the [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="c037b-113">ロール オブジェクトを作成するために使用する種類を表します。</span><span class="sxs-lookup"><span data-stu-id="c037b-113">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="c037b-114">次の例では、既定値で`string`型に置き換えられる`Guid`です。</span><span class="sxs-lookup"><span data-stu-id="c037b-114">In the following example, the default `string` type is replaced with `Guid`.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. <span data-ttu-id="c037b-115">データベースのカスタム コンテキスト クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="c037b-115">Create a custom database context class.</span></span> <span data-ttu-id="c037b-116">Id に使用される、Entity Framework データベースのコンテキスト クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="c037b-116">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="c037b-117">`TUser`と`TRole`引数はそれぞれ、前の手順で作成したカスタムのユーザーおよびロールのクラスを参照します。</span><span class="sxs-lookup"><span data-stu-id="c037b-117">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="c037b-118">`Guid`主キーのデータ型を定義します。</span><span class="sxs-lookup"><span data-stu-id="c037b-118">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. <span data-ttu-id="c037b-119">アプリのスタートアップ クラスで、Id サービスを追加するときに、データベースのカスタム コンテキスト クラスを登録します。</span><span class="sxs-lookup"><span data-stu-id="c037b-119">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="c037b-120">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="c037b-120">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x)
    
    <span data-ttu-id="c037b-121">`AddEntityFrameworkStores`メソッドを使用しない、 `TKey` ASP.NET Core では引数が行っていた 1.x です。</span><span class="sxs-lookup"><span data-stu-id="c037b-121">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="c037b-122">分析することによって、主キーのデータ型を推論、`DbContext`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="c037b-122">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="c037b-123">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="c037b-123">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x)
    
    <span data-ttu-id="c037b-124">`AddEntityFrameworkStores`メソッドを受け入れる、`TKey`主キーのデータ型を示す引数。</span><span class="sxs-lookup"><span data-stu-id="c037b-124">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a><span data-ttu-id="c037b-125">変更をテストします</span><span class="sxs-lookup"><span data-stu-id="c037b-125">Test the changes</span></span>

<span data-ttu-id="c037b-126">完了すると、構成の変更、主キーを表すプロパティには、新しいデータ型が反映されます。</span><span class="sxs-lookup"><span data-stu-id="c037b-126">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="c037b-127">次の例では、MVC コント ローラーのプロパティへのアクセスを示します。</span><span class="sxs-lookup"><span data-stu-id="c037b-127">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
