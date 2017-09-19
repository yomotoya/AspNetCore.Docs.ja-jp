---
title: "Id の主キーのデータ型を構成します。"
uid: security/authentication/identity-primary-key-configuration
ms.openlocfilehash: 2afcdf89b2a39d82a4ba72dc780be71ac98ab664
ms.sourcegitcommit: 76d42f09f3e0dd2f2105493eca6b29994aa47706
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/19/2017
---
# <a name="configure-identity-primary-keys-data-type"></a>Id の主キーのデータ型を構成します。

ASP.NET Core の Id では、主キーの目的のデータ型を簡単に構成することができます。 既定では、身元は文字列データ型が非常に短時間の動作をオーバーライドすることができます。

## <a name="how-to"></a>方法

1.  最初の手順をその Id のモデルを実装し、必要なデータ型と文字列型をオーバーライドします。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4-6&range=7-13)]

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3-5&range=7-12)]
    
2.  Id のデータベース コンテキストのモデルと主キーの目的のデータ型の実装します。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
3.  使用して、モデルとする主キーの id サービスで、アプリケーションのスタートアップ クラスを宣言するときに、データ型

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-79)]
