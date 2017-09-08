---
title: "Id の主キーのデータ型を構成します。"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: e6661708d003aa50204e7f79d3070442a3440391
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Id の主キーのデータ型を構成します。

ASP.NET Core の Id では、主キーの目的のデータ型を簡単に構成することができます。 既定では、身元は文字列データ型が非常に短時間の動作をオーバーライドすることができます。

## <a name="how-to"></a>方法

1.  最初の手順をその Id のモデルを実装し、必要なデータ型と文字列型をオーバーライドします。

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Id のデータベース コンテキストのモデルと主キーの目的のデータ型の実装します。

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  使用して、モデルとする主キーの id サービスで、アプリケーションのスタートアップ クラスを宣言するときに、データ型

    [!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
