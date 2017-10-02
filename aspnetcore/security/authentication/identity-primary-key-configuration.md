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
# <a name="configure-the-aspnet-core-identity-primary-key-data-type"></a>ASP.NET Core Id の主キー データの種類を構成します。

ASP.NET Core の Id では、主キーを表すために使用するデータの種類を構成することができます。 Id は、`string`既定ではデータ型。 この動作をオーバーライドすることができます。

## <a name="customize-the-primary-key-data-type"></a>主キー データの種類をカスタマイズします。

1. カスタム実装を作成、 [IdentityUser](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityuser-1)クラスです。 ユーザー オブジェクトの作成に使用する種類を表します。 次の例では、既定値で`string`型に置き換えられる`Guid`です。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationUser.cs?highlight=4&range=7-13)]

1. カスタム実装を作成、 [IdentityRole](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.identity.entityframeworkcore.identityrole-1)クラスです。 ロール オブジェクトを作成するために使用する種類を表します。 次の例では、既定値で`string`型に置き換えられる`Guid`です。
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Models/ApplicationRole.cs?highlight=3&range=7-12)]
    
1. データベースのカスタム コンテキスト クラスを作成します。 Id に使用される、Entity Framework データベースのコンテキスト クラスから継承します。 `TUser`と`TRole`引数はそれぞれ、前の手順で作成したカスタムのユーザーおよびロールのクラスを参照します。 `Guid`主キーのデータ型を定義します。

    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Data/ApplicationDbContext.cs?highlight=3&range=9-26)]
    
1. アプリのスタートアップ クラスで、Id サービスを追加するときに、データベースのカスタム コンテキスト クラスを登録します。

    # <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)
    
    `AddEntityFrameworkStores`メソッドを使用しない、 `TKey` ASP.NET Core では引数が行っていた 1.x です。 分析することによって、主キーのデータ型を推論、`DbContext`オブジェクト。
    
    [!code-csharp[Main](identity/sample/src/ASPNETv2-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=6-8&range=25-37)]
    
    # <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)
    
    `AddEntityFrameworkStores`メソッドを受け入れる、`TKey`主キーのデータ型を示す引数。
    
    [!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=9-11&range=39-55)]
    
    ---

## <a name="test-the-changes"></a>変更をテストします

完了すると、構成の変更、主キーを表すプロパティには、新しいデータ型が反映されます。 次の例では、MVC コント ローラーのプロパティへのアクセスを示します。

[!code-csharp[Main](identity/sample/src/ASPNET-IdentityDemo-PrimaryKeysConfig/Controllers/AccountController.cs?name=snippet_GetCurrentUserId&highlight=6)]
