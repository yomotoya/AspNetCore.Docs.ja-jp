---
title: "Id の主キーのデータ型を構成します。"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="be8bf-102">Id の主キーのデータ型を構成します。</span><span class="sxs-lookup"><span data-stu-id="be8bf-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="be8bf-103">ASP.NET Core の Id では、主キーの目的のデータ型を簡単に構成することができます。</span><span class="sxs-lookup"><span data-stu-id="be8bf-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="be8bf-104">既定では、身元は文字列データ型が非常に短時間の動作をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="be8bf-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="be8bf-105">方法</span><span class="sxs-lookup"><span data-stu-id="be8bf-105">How to</span></span>

1.  <span data-ttu-id="be8bf-106">最初の手順をその Id のモデルを実装し、必要なデータ型と文字列型をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="be8bf-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  <span data-ttu-id="be8bf-107">Id のデータベース コンテキストのモデルと主キーの目的のデータ型の実装します。</span><span class="sxs-lookup"><span data-stu-id="be8bf-107">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  <span data-ttu-id="be8bf-108">使用して、モデルとする主キーの id サービスで、アプリケーションのスタートアップ クラスを宣言するときに、データ型</span><span class="sxs-lookup"><span data-stu-id="be8bf-108">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
