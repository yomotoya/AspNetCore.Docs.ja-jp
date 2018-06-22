---
title: ASP.NET Core の Id の主キー データの種類を構成します。
author: AdrienTorris
description: ASP.NET Core Id 主キーの使用目的のデータ型を構成する手順について説明します。
ms.author: scaddie
ms.date: 09/28/2017
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: cfec91e1194556385bb884ee44cf79c1fcbbcb56
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36274357"
---
# <a name="configure-identity-primary-key-data-type-in-aspnet-core"></a><span data-ttu-id="7ac40-103">ASP.NET Core の Id の主キー データの種類を構成します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-103">Configure Identity primary key data type in ASP.NET Core</span></span>

<span data-ttu-id="7ac40-104">ASP.NET Core の Id では、主キーを表すために使用するデータの種類を構成することができます。</span><span class="sxs-lookup"><span data-stu-id="7ac40-104">ASP.NET Core Identity allows you to configure the data type used to represent a primary key.</span></span> <span data-ttu-id="7ac40-105">Id は、`string`既定ではデータ型。</span><span class="sxs-lookup"><span data-stu-id="7ac40-105">Identity uses the `string` data type by default.</span></span> <span data-ttu-id="7ac40-106">この動作をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="7ac40-106">You can override this behavior.</span></span>

## <a name="customize-the-primary-key-data-type"></a><span data-ttu-id="7ac40-107">主キー データの種類をカスタマイズします。</span><span class="sxs-lookup"><span data-stu-id="7ac40-107">Customize the primary key data type</span></span>

1. <span data-ttu-id="7ac40-108">カスタム実装を作成、 [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1)クラスです。</span><span class="sxs-lookup"><span data-stu-id="7ac40-108">Create a custom implementation of the [IdentityUser](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1) class.</span></span> <span data-ttu-id="7ac40-109">ユーザー オブジェクトの作成に使用する種類を表します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-109">It represents the type to be used for creating user objects.</span></span> <span data-ttu-id="7ac40-110">次の例では、既定値で`string`型に置き換えられる`Guid`です。</span><span class="sxs-lookup"><span data-stu-id="7ac40-110">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

2. <span data-ttu-id="7ac40-111">カスタム実装を作成、 [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1)クラスです。</span><span class="sxs-lookup"><span data-stu-id="7ac40-111">Create a custom implementation of the [IdentityRole](/dotnet/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1) class.</span></span> <span data-ttu-id="7ac40-112">ロール オブジェクトを作成するために使用する種類を表します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-112">It represents the type to be used for creating role objects.</span></span> <span data-ttu-id="7ac40-113">次の例では、既定値で`string`型に置き換えられる`Guid`です。</span><span class="sxs-lookup"><span data-stu-id="7ac40-113">In the following example, the default `string` type is replaced with `Guid`.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]

3. <span data-ttu-id="7ac40-114">データベースのカスタム コンテキスト クラスを作成します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-114">Create a custom database context class.</span></span> <span data-ttu-id="7ac40-115">Id に使用される、Entity Framework データベースのコンテキスト クラスから継承します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-115">It inherits from the Entity Framework database context class used for Identity.</span></span> <span data-ttu-id="7ac40-116">`TUser`と`TRole`引数はそれぞれ、前の手順で作成したカスタムのユーザーおよびロールのクラスを参照します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-116">The `TUser` and `TRole` arguments reference the custom user and role classes created in the previous step, respectively.</span></span> <span data-ttu-id="7ac40-117">`Guid`主キーのデータ型を定義します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-117">The `Guid` data type is defined for the primary key.</span></span>

    [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]

4. <span data-ttu-id="7ac40-118">アプリのスタートアップ クラスで、Id サービスを追加するときに、データベースのカスタム コンテキスト クラスを登録します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-118">Register the custom database context class when adding the Identity service in the app's startup class.</span></span>

   # <a name="aspnet-core-2xtabaspnetcore2x"></a>[<span data-ttu-id="7ac40-119">ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="7ac40-119">ASP.NET Core 2.x</span></span>](#tab/aspnetcore2x/)

   <span data-ttu-id="7ac40-120">`AddEntityFrameworkStores`メソッドを使用しない、 `TKey` ASP.NET Core では引数が行っていた 1.x です。</span><span class="sxs-lookup"><span data-stu-id="7ac40-120">The `AddEntityFrameworkStores` method doesn't accept a `TKey` argument as it did in ASP.NET Core 1.x.</span></span> <span data-ttu-id="7ac40-121">分析することによって、主キーのデータ型を推論、`DbContext`オブジェクト。</span><span class="sxs-lookup"><span data-stu-id="7ac40-121">The primary key's data type is inferred by analyzing the `DbContext` object.</span></span>

   [!code-csharp[](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]

   # <a name="aspnet-core-1xtabaspnetcore1x"></a>[<span data-ttu-id="7ac40-122">ASP.NET Core 1.x</span><span class="sxs-lookup"><span data-stu-id="7ac40-122">ASP.NET Core 1.x</span></span>](#tab/aspnetcore1x/)

   <span data-ttu-id="7ac40-123">`AddEntityFrameworkStores`メソッドを受け入れる、`TKey`主キーのデータ型を示す引数。</span><span class="sxs-lookup"><span data-stu-id="7ac40-123">The `AddEntityFrameworkStores` method accepts a `TKey` argument indicating the primary key's data type.</span></span>

   [!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]

   ---

## <a name="test-the-changes"></a><span data-ttu-id="7ac40-124">変更をテストします</span><span class="sxs-lookup"><span data-stu-id="7ac40-124">Test the changes</span></span>

<span data-ttu-id="7ac40-125">完了すると、構成の変更、主キーを表すプロパティには、新しいデータ型が反映されます。</span><span class="sxs-lookup"><span data-stu-id="7ac40-125">Upon completion of the configuration changes, the property representing the primary key reflects the new data type.</span></span> <span data-ttu-id="7ac40-126">次の例では、MVC コント ローラーのプロパティへのアクセスを示します。</span><span class="sxs-lookup"><span data-stu-id="7ac40-126">The following example demonstrates accessing the property in an MVC controller.</span></span>

[!code-csharp[](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
