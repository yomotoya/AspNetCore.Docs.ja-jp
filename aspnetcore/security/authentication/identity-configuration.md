---
title: "ASP.NET Core Id を構成します。"
author: AdrienTorris
description: "ASP.NET Core Id で、既定値を把握し、カスタム値を使用するさまざまな Id プロパティを構成します。"
keywords: "ASP.NET Core、Identity、認証、セキュリティ"
ms.author: scaddie
manager: wpickett
ms.date: 09/18/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-configuration
ms.openlocfilehash: 629fcc2941b2d2fda9604a3eac04be3d0f5294b2
ms.sourcegitcommit: ddefc78270bd9b5ae0b1bd8de6c45f6977e7dceb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/18/2017
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
