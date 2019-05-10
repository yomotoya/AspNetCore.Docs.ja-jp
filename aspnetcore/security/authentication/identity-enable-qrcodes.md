---
title: ASP.NET Core で TOTP authenticator アプリの QR コード生成を有効にします。
author: rick-anderson
description: ASP.NET Core 2 要素認証と連携する TOTP authenticator アプリの QR コードの生成を有効にする方法を説明します。
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: a7fdc86b3fe94e714e5147c89a32fce13757d1c1
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/27/2019
ms.locfileid: "64896729"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="27cc3-103">ASP.NET Core で TOTP authenticator アプリの QR コード生成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="27cc3-103">Enable QR Code generation for TOTP authenticator apps in ASP.NET Core</span></span>

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="27cc3-104">QR コードには、ASP.NET Core 2.0 以降が必要です。</span><span class="sxs-lookup"><span data-stu-id="27cc3-104">QR Codes requires ASP.NET Core 2.0 or later.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="27cc3-105">個人の認証用の authenticator アプリケーション対応の ASP.NET Core が同梱されています。</span><span class="sxs-lookup"><span data-stu-id="27cc3-105">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="27cc3-106">時間ベース ワンタイム パスワード アルゴリズム (TOTP) を使用して 2 要素認証 (2 fa) authenticator アプリは、業界の 2 fa のアプローチをお勧めします。</span><span class="sxs-lookup"><span data-stu-id="27cc3-106">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="27cc3-107">2 fa を SMS 2 fa を TOTP を使用しています。</span><span class="sxs-lookup"><span data-stu-id="27cc3-107">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="27cc3-108">Authenticator アプリは、ユーザー名とパスワードを確認した後にユーザーが入力する必要があります、6 ~ 8 桁のコードを提供します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-108">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="27cc3-109">通常、authenticator アプリは、スマート フォンにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="27cc3-109">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="27cc3-110">ASP.NET Core web アプリ テンプレートを使用するは、認証子をサポートしますが、QRCode 生成のためのサポートを提供しません。</span><span class="sxs-lookup"><span data-stu-id="27cc3-110">The ASP.NET Core web app templates support authenticators, but don't provide support for QRCode generation.</span></span> <span data-ttu-id="27cc3-111">QRCode ジェネレーターには、2 fa のセットアップが容易になります。</span><span class="sxs-lookup"><span data-stu-id="27cc3-111">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="27cc3-112">このドキュメントのガイドでは追加[QR コード](https://wikipedia.org/wiki/QR_code)2 fa の構成 ページを生成します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-112">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

<span data-ttu-id="27cc3-113">2 要素認証は、外部認証プロバイダーを使用して起こりません[Google](xref:security/authentication/google-logins)または[Facebook](xref:security/authentication/facebook-logins)します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-113">Two factor authentication does not happen using an external authentication provider, such as [Google](xref:security/authentication/google-logins) or [Facebook](xref:security/authentication/facebook-logins).</span></span> <span data-ttu-id="27cc3-114">外部ログインが保護されている任意のメカニズムによって、外部ログイン プロバイダーを提供します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-114">External logins are protected by whatever mechanism the external login provider provides.</span></span> <span data-ttu-id="27cc3-115">たとえば、考えてみましょう、 [Microsoft](xref:security/authentication/microsoft-logins)認証プロバイダーは、ハードウェア キーまたは 2 fa の別の方法が必要です。</span><span class="sxs-lookup"><span data-stu-id="27cc3-115">Consider, for example, the [Microsoft](xref:security/authentication/microsoft-logins) authentication provider requires a hardware key or another 2FA approach.</span></span> <span data-ttu-id="27cc3-116">既定のテンプレートは、"local"2 fa を適用する場合は、一般的に使用されるシナリオではありませんが、2 つの 2 fa アプローチを満たすためにユーザーを必要になります。</span><span class="sxs-lookup"><span data-stu-id="27cc3-116">If the default templates enforced "local" 2FA then users would be required to satisfy two 2FA approaches, which is not a commonly used scenario.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="27cc3-117">2 fa の構成 ページに QR コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-117">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="27cc3-118">これらの手順を使用して、 *qrcode.js*から、 https://davidshimjs.github.io/qrcodejs/リポジトリ。</span><span class="sxs-lookup"><span data-stu-id="27cc3-118">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="27cc3-119">ダウンロード、 [qrcode.js javascript ライブラリ](https://davidshimjs.github.io/qrcodejs/)を`wwwroot\lib`プロジェクトのフォルダー。</span><span class="sxs-lookup"><span data-stu-id="27cc3-119">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <span data-ttu-id="27cc3-120">指示に従って[スキャフォールディング Identity](xref:security/authentication/scaffold-identity)を生成する */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-120">Follow the instructions in [Scaffold Identity](xref:security/authentication/scaffold-identity) to generate */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*.</span></span>
* <span data-ttu-id="27cc3-121">*/Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*、検索、`Scripts`ファイルの最後のセクション。</span><span class="sxs-lookup"><span data-stu-id="27cc3-121">In */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*, locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <span data-ttu-id="27cc3-122">*Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor ページ) または*Views/Manage/EnableAuthenticator.cshtml* (MVC) を検索、`Scripts`ファイルの最後のセクション。</span><span class="sxs-lookup"><span data-stu-id="27cc3-122">In *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor Pages) or *Views/Manage/EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="27cc3-123">更新プログラム、`Scripts`への参照を追加するセクション、`qrcodejs`ライブラリを追加して、その QR コードを生成するへの呼び出し。</span><span class="sxs-lookup"><span data-stu-id="27cc3-123">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="27cc3-124">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="27cc3-124">It should look as follows:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")

    <script type="text/javascript" src="~/lib/qrcode.js"></script>
    <script type="text/javascript">
        new QRCode(document.getElementById("qrCode"),
            {
                text: "@Html.Raw(Model.AuthenticatorUri)",
                width: 150,
                height: 150
            });
    </script>
}
```

* <span data-ttu-id="27cc3-125">これらの手順へのリンクと、段落を削除します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-125">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="27cc3-126">アプリを実行し、QR コードをスキャンおよび認証システムを証明するコードを検証できることを確認します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-126">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="27cc3-127">QR コードでは、サイト名を変更します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-127">Change the site name in the QR Code</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="27cc3-128">QR コードでは、サイト名は、最初に、プロジェクトを作成するときに、選択したプロジェクト名から取得されます。</span><span class="sxs-lookup"><span data-stu-id="27cc3-128">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="27cc3-129">探して変更することができます、`GenerateQrCodeUri(string email, string unformattedKey)`メソッドで、 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-129">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml.cs*.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="27cc3-130">QR コードでは、サイト名は、最初に、プロジェクトを作成するときに、選択したプロジェクト名から取得されます。</span><span class="sxs-lookup"><span data-stu-id="27cc3-130">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="27cc3-131">探して変更することができます、`GenerateQrCodeUri(string email, string unformattedKey)`メソッドで、 *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor ページ) ファイルまたは*Controllers/ManageController.cs* (MVC) ファイル。</span><span class="sxs-lookup"><span data-stu-id="27cc3-131">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers/ManageController.cs* (MVC) file.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

<span data-ttu-id="27cc3-132">テンプレートから既定のコードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="27cc3-132">The default code from the template looks as follows:</span></span>

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenticatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="27cc3-133">2 番目のパラメーターへの呼び出しで`string.Format`は、サイト名は、ソリューション名から取得されます。</span><span class="sxs-lookup"><span data-stu-id="27cc3-133">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="27cc3-134">任意の値に変更できますが、エンコードされた URL に常にあります。</span><span class="sxs-lookup"><span data-stu-id="27cc3-134">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="27cc3-135">別の QR コード ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-135">Using a different QR Code library</span></span>

<span data-ttu-id="27cc3-136">QR コード ライブラリを優先されるライブラリと置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="27cc3-136">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="27cc3-137">HTML が含まれています、`qrCode`要素がどのようなメカニズムによって QR コードを配置する先のライブラリで提供します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-137">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="27cc3-138">QR コードを正しく書式設定された URL が表示されます。</span><span class="sxs-lookup"><span data-stu-id="27cc3-138">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="27cc3-139">`AuthenticatorUri` モデルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="27cc3-139">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="27cc3-140">`data-url` プロパティで、`qrCodeData`要素。</span><span class="sxs-lookup"><span data-stu-id="27cc3-140">`data-url` property in the `qrCodeData` element.</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="27cc3-141">TOTP クライアントとサーバー時間のずれ</span><span class="sxs-lookup"><span data-stu-id="27cc3-141">TOTP client and server time skew</span></span>

<span data-ttu-id="27cc3-142">TOTP (時間ベースのワンタイム パスワード) 認証は、正確な時間のあるサーバーと認証システムの両方のデバイスに依存します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-142">TOTP (Time-based One-Time Password) authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="27cc3-143">30 秒間のみ最後のトークン。</span><span class="sxs-lookup"><span data-stu-id="27cc3-143">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="27cc3-144">TOTP 2 fa ログインが失敗する場合は、サーバーの時刻が、精度と正確な NTP サービスに可能であれば同期であることを確認します。</span><span class="sxs-lookup"><span data-stu-id="27cc3-144">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>

::: moniker-end
