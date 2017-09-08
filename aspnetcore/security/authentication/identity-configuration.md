---
title: "Id を構成します。"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a>Id を構成します。

ASP.NET Core Id は、簡単に、アプリケーションのスタートアップ クラスでオーバーライドできるいくつかの既定の動作を持っています。

## <a name="passwords-policy"></a>パスワード ポリシー

既定では、Id は、パスワードに、大文字、小文字、および数字を含めることが必要です。 その他のいくつかの制限もあります。 パスワードの制限を簡略化する場合は、アプリケーションのスタートアップ クラスのことができます。

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a>アプリケーションの cookie の設定

パスワード ポリシーと同様に、スタートアップ クラスで、アプリケーションの cookie のすべての設定を変更できます。

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a>ユーザーのロックアウト

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
