---
title: "Id を構成します。"
uid: security/authentication/identity-configuration
ms.openlocfilehash: 7ccd89360a8c7f5c8c6dfac76df42898e18a116a
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="configure-identity"></a><span data-ttu-id="99eed-102">Id を構成します。</span><span class="sxs-lookup"><span data-stu-id="99eed-102">Configure Identity</span></span>

<span data-ttu-id="99eed-103">ASP.NET Core Id は、簡単に、アプリケーションのスタートアップ クラスでオーバーライドできるいくつかの既定の動作を持っています。</span><span class="sxs-lookup"><span data-stu-id="99eed-103">ASP.NET Core Identity has some default behaviors that you can override easily in your application's startup class.</span></span>

## <a name="passwords-policy"></a><span data-ttu-id="99eed-104">パスワード ポリシー</span><span class="sxs-lookup"><span data-stu-id="99eed-104">Passwords policy</span></span>

<span data-ttu-id="99eed-105">既定では、Id は、パスワードに、大文字、小文字、および数字を含めることが必要です。</span><span class="sxs-lookup"><span data-stu-id="99eed-105">By default, Identity requires that passwords contain an uppercase character, lowercase character, and digits.</span></span> <span data-ttu-id="99eed-106">その他のいくつかの制限もあります。</span><span class="sxs-lookup"><span data-stu-id="99eed-106">There are also some other restrictions.</span></span> <span data-ttu-id="99eed-107">パスワードの制限を簡略化する場合は、アプリケーションのスタートアップ クラスのことができます。</span><span class="sxs-lookup"><span data-stu-id="99eed-107">If you want to simplify password restrictions, you can do that in the startup class of your application.</span></span>

<span data-ttu-id="99eed-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span><span class="sxs-lookup"><span data-stu-id="99eed-108">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=60-65)]</span></span>

## <a name="applications-cookie-settings"></a><span data-ttu-id="99eed-109">アプリケーションの cookie の設定</span><span class="sxs-lookup"><span data-stu-id="99eed-109">Application's cookie settings</span></span>

<span data-ttu-id="99eed-110">パスワード ポリシーと同様に、スタートアップ クラスで、アプリケーションの cookie のすべての設定を変更できます。</span><span class="sxs-lookup"><span data-stu-id="99eed-110">Like the passwords policy, all the settings of the application's cookie can be changed in the startup class.</span></span>

<span data-ttu-id="99eed-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span><span class="sxs-lookup"><span data-stu-id="99eed-111">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=72-80)]</span></span>

## <a name="users-lockout"></a><span data-ttu-id="99eed-112">ユーザーのロックアウト</span><span class="sxs-lookup"><span data-stu-id="99eed-112">User's lockout</span></span>

<span data-ttu-id="99eed-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span><span class="sxs-lookup"><span data-stu-id="99eed-113">[!code-csharp[Main](identity/sample/src/ASPET-IdentityDemo-PrimaryKeysConfig/Startup.cs?highlight=2&range=67-70)]</span></span>
