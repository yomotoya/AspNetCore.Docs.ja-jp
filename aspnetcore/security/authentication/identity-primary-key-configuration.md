---
title: "Id の主キーのデータ型を構成します。"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a><span data-ttu-id="c1efa-102">Id の主キーのデータ型を構成します。</span><span class="sxs-lookup"><span data-stu-id="c1efa-102">Configure Identity primary keys data type</span></span>

<span data-ttu-id="c1efa-103">ASP.NET Core の Id では、主キーの目的のデータ型を簡単に構成することができます。</span><span class="sxs-lookup"><span data-stu-id="c1efa-103">ASP.NET Core Identity allows you to easily configure the data type you want for the primary keys.</span></span> <span data-ttu-id="c1efa-104">既定では、身元は文字列データ型が非常に短時間の動作をオーバーライドすることができます。</span><span class="sxs-lookup"><span data-stu-id="c1efa-104">By default, Identity uses string data type but you can very quickly override this behavior.</span></span>

## <a name="how-to"></a><span data-ttu-id="c1efa-105">方法</span><span class="sxs-lookup"><span data-stu-id="c1efa-105">How to</span></span>

1.  <span data-ttu-id="c1efa-106">最初の手順をその Id のモデルを実装し、必要なデータ型と文字列型をオーバーライドします。</span><span class="sxs-lookup"><span data-stu-id="c1efa-106">The first step is to implement the Identity's model, and override the string type with the data type you want.</span></span>

    <span data-ttu-id="c1efa-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span><span class="sxs-lookup"><span data-stu-id="c1efa-107">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]</span></span>

    <span data-ttu-id="c1efa-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span><span class="sxs-lookup"><span data-stu-id="c1efa-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]</span></span>
    
2.  <span data-ttu-id="c1efa-109">Id のデータベース コンテキストのモデルと主キーの目的のデータ型の実装します。</span><span class="sxs-lookup"><span data-stu-id="c1efa-109">Implement the database context of Identity with your models and the data type you want for primary keys</span></span>

    <span data-ttu-id="c1efa-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span><span class="sxs-lookup"><span data-stu-id="c1efa-110">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]</span></span>
    
3.  <span data-ttu-id="c1efa-111">使用して、モデルとする主キーの id サービスで、アプリケーションのスタートアップ クラスを宣言するときに、データ型</span><span class="sxs-lookup"><span data-stu-id="c1efa-111">Use your models and the data type you want for primary keys when you declare the identity service in your application's startup class</span></span>

    <span data-ttu-id="c1efa-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span><span class="sxs-lookup"><span data-stu-id="c1efa-112">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]</span></span>
