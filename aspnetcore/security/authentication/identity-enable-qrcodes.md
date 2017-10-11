---
title: "ASP.NET Core でのアプリの認証子の QR コード生成を有効にします。"
author: rick-anderson
description: "ASP.NET Core でのアプリの認証子の QR コード生成を有効にします。"
keywords: "ASP.NET Core、MVC、QR コードの生成、認証システム、2 fa"
ms.author: riande
manager: wpickett
ms.date: 09/24/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 01bb5597033fef7e1cb08e980c81d37d88ed253e
ms.sourcegitcommit: ab91aad2680efc4eb5c0642746e2b981db7f81b8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/10/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a><span data-ttu-id="88437-104">ASP.NET Core でのアプリの認証子の QR コード生成を有効にします。</span><span class="sxs-lookup"><span data-stu-id="88437-104">Enabling QR Code generation for authenticator apps in ASP.NET Core</span></span>

<span data-ttu-id="88437-105">注: このトピックの対象は ASP.NET Core 2.x</span><span class="sxs-lookup"><span data-stu-id="88437-105">Note: This topic applies to ASP.NET Core 2.x</span></span>

<span data-ttu-id="88437-106">ASP.NET Core は、個別の認証の認証システム アプリケーションのサポートに付属します。</span><span class="sxs-lookup"><span data-stu-id="88437-106">ASP.NET Core ships with support for authenticator applications for individual authentication.</span></span> <span data-ttu-id="88437-107">使用して、時間ベース ワンタイム パスワード アルゴリズム (TOTP)、2 要素認証 (2 fa) 認証アプリとは、2 fa のアプローチをお勧め業界です。</span><span class="sxs-lookup"><span data-stu-id="88437-107">Two factor authentication (2FA) authenticator apps, using a Time-based One-time Password Algorithm (TOTP), are the industry recommended approach for 2FA.</span></span> <span data-ttu-id="88437-108">2 fa TOTP を使用してを SMS 2 fa をお勧めします。</span><span class="sxs-lookup"><span data-stu-id="88437-108">2FA using TOTP is preferred to SMS 2FA.</span></span> <span data-ttu-id="88437-109">認証アプリでは、ユーザー名とパスワードを確認した後どのユーザーが入力する必要があります、6 ~ 8 桁のコードを提供します。</span><span class="sxs-lookup"><span data-stu-id="88437-109">An authenticator app provides a 6 to 8 digit code which users must enter after confirming their username and password.</span></span> <span data-ttu-id="88437-110">通常、認証アプリがスマート フォンにインストールされます。</span><span class="sxs-lookup"><span data-stu-id="88437-110">Typically an authenticator app is installed on a smart phone.</span></span>

<span data-ttu-id="88437-111">ASP.NET Core web アプリ テンプレートでは、認証のサポートは、QRCode 生成のサポートを提供しません。</span><span class="sxs-lookup"><span data-stu-id="88437-111">The ASP.NET Core web app templates support authenticators, but do not provide support for QRCode generation.</span></span> <span data-ttu-id="88437-112">QRCode ジェネレーター 2 fa のセットアップが容易になります。</span><span class="sxs-lookup"><span data-stu-id="88437-112">QRCode generators ease the setup of 2FA.</span></span> <span data-ttu-id="88437-113">このドキュメントのガイドの追加方法[QR コード](https://wikipedia.org/wiki/QR_code)2 fa の構成 ページを生成します。</span><span class="sxs-lookup"><span data-stu-id="88437-113">This document will guide you through adding [QR Code](https://wikipedia.org/wiki/QR_code) generation to the 2FA configuration page.</span></span>

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a><span data-ttu-id="88437-114">2 fa 構成 ページに QR コードを追加します。</span><span class="sxs-lookup"><span data-stu-id="88437-114">Adding QR Codes to the 2FA configuration page</span></span>

<span data-ttu-id="88437-115">これらの手順を使用して*qrcode.js* https://davidshimjs.github.io/qrcodejs/ リポジトリからです。</span><span class="sxs-lookup"><span data-stu-id="88437-115">These instructions use *qrcode.js* from the https://davidshimjs.github.io/qrcodejs/ repo.</span></span>

* <span data-ttu-id="88437-116">ダウンロード、 [qrcode.js javascript ライブラリ](https://davidshimjs.github.io/qrcodejs/)を`wwwroot\lib`プロジェクトのフォルダーにします。</span><span class="sxs-lookup"><span data-stu-id="88437-116">Download the [qrcode.js javascript library](https://davidshimjs.github.io/qrcodejs/) to the `wwwroot\lib` folder in your project.</span></span>

* <span data-ttu-id="88437-117">*Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor ページ) または*Views\Manage\EnableAuthenticator.cshtml* (MVC)、検索、`Scripts`ファイルの最後のセクション。</span><span class="sxs-lookup"><span data-stu-id="88437-117">In *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor Pages) or *Views\Manage\EnableAuthenticator.cshtml* (MVC), locate the `Scripts` section at the end of the file:</span></span>

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* <span data-ttu-id="88437-118">更新プログラム、`Scripts`への参照を追加するセクション、 `qrcodejs` QR コードの生成を呼び出すと追加するライブラリ。</span><span class="sxs-lookup"><span data-stu-id="88437-118">Update the `Scripts` section to add a reference to the `qrcodejs` library you added and a call to generate the QR Code.</span></span> <span data-ttu-id="88437-119">次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88437-119">It should look as follows:</span></span>

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

* <span data-ttu-id="88437-120">これらの手順にリンクする段落を削除します。</span><span class="sxs-lookup"><span data-stu-id="88437-120">Delete the paragraph which links you to these instructions.</span></span>

<span data-ttu-id="88437-121">アプリを実行して、QR コードをスキャンおよび性は、認証コードを検証できることを確認してください。</span><span class="sxs-lookup"><span data-stu-id="88437-121">Run your app and ensure that you can scan the QR code and validate the code the authenticator proves.</span></span>

## <a name="change-the-site-name-in-the-qr-code"></a><span data-ttu-id="88437-122">QR コード内で、サイト名を変更します。</span><span class="sxs-lookup"><span data-stu-id="88437-122">Change the site name in the QR Code</span></span>

<span data-ttu-id="88437-123">QR コード内で、サイト名は、最初に、プロジェクトの作成時に選択したプロジェクト名から取得されます。</span><span class="sxs-lookup"><span data-stu-id="88437-123">The site name in the QR Code is taken from the project name you choose when initially creating your project.</span></span> <span data-ttu-id="88437-124">探して変更することができます、`GenerateQrCodeUri(string email, string unformattedKey)`メソッドで、 *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor ページ) ファイルまたは*Controllers\AccountController.cs* (MVC) ファイル。</span><span class="sxs-lookup"><span data-stu-id="88437-124">You can change it by looking for the `GenerateQrCodeUri(string email, string unformattedKey)` method in the *Pages\Account\Manage\EnableAuthenticator.cshtml.cs* (Razor Pages) file or the *Controllers\AccountController.cs* (MVC) file.</span></span> 

<span data-ttu-id="88437-125">テンプレートから既定のコードは次のようになります。</span><span class="sxs-lookup"><span data-stu-id="88437-125">The default code from the template looks as follows:</span></span>

```c#
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

<span data-ttu-id="88437-126">呼び出しで 2 番目のパラメーター`string.Format`は、ソリューションの名前から取得された、サイト名です。</span><span class="sxs-lookup"><span data-stu-id="88437-126">The second parameter in the call to `string.Format` is your site name, taken from your solution name.</span></span> <span data-ttu-id="88437-127">任意の値に変更できますが、URL エンコードは常にあります。</span><span class="sxs-lookup"><span data-stu-id="88437-127">It can be changed to any value, but it must always be URL encoded.</span></span>

## <a name="using-a-different-qr-code-library"></a><span data-ttu-id="88437-128">別の QR コード ライブラリを使用します。</span><span class="sxs-lookup"><span data-stu-id="88437-128">Using a different QR Code library</span></span>

<span data-ttu-id="88437-129">QR コード ライブラリを優先されるライブラリと置き換えることができます。</span><span class="sxs-lookup"><span data-stu-id="88437-129">You can replace the QR Code library with your preferred library.</span></span> <span data-ttu-id="88437-130">HTML が含まれています、`qrCode`要素にどのようなメカニズムによって QR コードを配置することができます、ライブラリを提供します。</span><span class="sxs-lookup"><span data-stu-id="88437-130">The HTML contains a `qrCode` element into which you can place a QR Code by whatever mechanism your library provides.</span></span>

<span data-ttu-id="88437-131">QR コードを正しく書式設定された URL はで使用します。</span><span class="sxs-lookup"><span data-stu-id="88437-131">The correctly formatted URL for the QR Code is available in the:</span></span>

* <span data-ttu-id="88437-132">`AuthenticatorUri`モデルのプロパティです。</span><span class="sxs-lookup"><span data-stu-id="88437-132">`AuthenticatorUri` property of the model.</span></span>
* <span data-ttu-id="88437-133">`data-url`プロパティに、`qrCodeData`要素。</span><span class="sxs-lookup"><span data-stu-id="88437-133">`data-url` property in the `qrCodeData` element.</span></span> 

<span data-ttu-id="88437-134">使用して`@Html.Raw`ビューのモデル プロパティにアクセスする (それ以外の場合、url のアンパサンドは倍精度浮動小数点でエンコードされたおよび QR コードのラベル パラメーターは無視されます)。</span><span class="sxs-lookup"><span data-stu-id="88437-134">Use `@Html.Raw` to access the model property in a view (otherwise the ampersands in the url will be double encoded and the label parameter of the QR Code will be ignored).</span></span>

## <a name="totp-client-and-server-time-skew"></a><span data-ttu-id="88437-135">TOTP クライアントとサーバー時間のずれ</span><span class="sxs-lookup"><span data-stu-id="88437-135">TOTP client and server time skew</span></span>

<span data-ttu-id="88437-136">TOTP 認証は、正確な時間のあるサーバーと認証子の両方のデバイスに依存します。</span><span class="sxs-lookup"><span data-stu-id="88437-136">TOTP authentication depends on both the server and authenticator device having an accurate time.</span></span> <span data-ttu-id="88437-137">トークンの寿命は 30 秒です。</span><span class="sxs-lookup"><span data-stu-id="88437-137">Tokens only last for 30 seconds.</span></span> <span data-ttu-id="88437-138">TOTP 2 fa ログインが失敗した場合は、サーバーの時刻が、精度は、可能であれば正確な NTP サービスに同期されることを確認します。</span><span class="sxs-lookup"><span data-stu-id="88437-138">If TOTP 2FA logins are failing, check that the server time is accurate, and preferably synchronized to an accurate NTP service.</span></span>
