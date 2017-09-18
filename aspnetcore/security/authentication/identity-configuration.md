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
# <a name="configure-identity"></a><span data-ttu-id="4bf6c-104">Id を構成します。</span><span class="sxs-lookup"><span data-stu-id="4bf6c-104">Configure Identity</span></span>

<span data-ttu-id="4bf6c-105">ASP.NET Core Id は、簡単に、アプリケーションのスタートアップ クラスでオーバーライドできるいくつかの既定の動作を持っています。</span><span class="sxs-lookup"><span data-stu-id="4bf6c-105">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="4bf6c-106">パスワード ポリシー</span><span class="sxs-lookup"><span data-stu-id="4bf6c-106">Passwords policy</span></span>

<span data-ttu-id="4bf6c-107">既定では、Id は、パスワードに、大文字、小文字、および数字を含めることが必要です。</span><span class="sxs-lookup"><span data-stu-id="4bf6c-107">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="4bf6c-108">その他のいくつかの制限もあります。</span><span class="sxs-lookup"><span data-stu-id="4bf6c-108">There are also some other restrictions.</span></span> <span data-ttu-id="4bf6c-109">パスワードの制限を簡略化する場合は、アプリケーションのスタートアップ クラスのことができます。</span><span class="sxs-lookup"><span data-stu-id="4bf6c-109">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]

## <a name="applications-cookie-settings"></a><span data-ttu-id="4bf6c-110">アプリケーションの cookie の設定</span><span class="sxs-lookup"><span data-stu-id="4bf6c-110">Application's cookie settings</span></span>

<span data-ttu-id="4bf6c-111">パスワード ポリシーと同様に、スタートアップ クラスで、アプリケーションの cookie のすべての設定を変更できます。</span><span class="sxs-lookup"><span data-stu-id="4bf6c-111">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]

## <a name="users-lockout"></a><span data-ttu-id="4bf6c-112">ユーザーのロックアウト</span><span class="sxs-lookup"><span data-stu-id="4bf6c-112">User's lockout</span></span>

[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]
