---
title: ASP.NET Core で TOTP authenticator アプリの QR コード生成を有効にします。
author: rick-anderson
description: ASP.NET Core 2 要素認証と連携する TOTP authenticator アプリの QR コードの生成を有効にする方法を説明します。
ms.author: riande
ms.date: 08/14/2018
uid: security/authentication/identity-enable-qrcodes
ms.openlocfilehash: 437f354f71128a98bae9abdced291e04efc9f48e
ms.sourcegitcommit: fc7eb4243188950ae1f1b52669edc007e9d0798d
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/07/2018
ms.locfileid: "51225383"
---
# <a name="enable-qr-code-generation-for-totp-authenticator-apps-in-aspnet-core"></a>ASP.NET Core で TOTP authenticator アプリの QR コード生成を有効にします。

::: moniker range="<= aspnetcore-2.0"

QR コードには、ASP.NET Core 2.0 以降が必要です。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

個人の認証用の authenticator アプリケーション対応の ASP.NET Core が同梱されています。 時間ベース ワンタイム パスワード アルゴリズム (TOTP) を使用して 2 要素認証 (2 fa) authenticator アプリは、業界の 2 fa のアプローチをお勧めします。 2 fa を SMS 2 fa を TOTP を使用しています。 Authenticator アプリは、ユーザー名とパスワードを確認した後にユーザーが入力する必要があります、6 ~ 8 桁のコードを提供します。 通常、authenticator アプリは、スマート フォンにインストールされます。

ASP.NET Core web アプリ テンプレートを使用するは、認証子をサポートしますが、QRCode 生成のためのサポートを提供しません。 QRCode ジェネレーターには、2 fa のセットアップが容易になります。 このドキュメントのガイドでは追加[QR コード](https://wikipedia.org/wiki/QR_code)2 fa の構成 ページを生成します。

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>2 fa の構成 ページに QR コードを追加します。

これらの手順を使用して、 *qrcode.js*から、 https://davidshimjs.github.io/qrcodejs/リポジトリ。

* ダウンロード、 [qrcode.js javascript ライブラリ](https://davidshimjs.github.io/qrcodejs/)を`wwwroot\lib`プロジェクトのフォルダー。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* 指示に従って[スキャフォールディング Identity](xref:security/authentication/scaffold-identity)を生成する */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*します。
* */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*、検索、`Scripts`ファイルの最後のセクション。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* *Pages/Account/Manage/EnableAuthenticator.cshtml* (Razor ページ) または*Views/Manage/EnableAuthenticator.cshtml* (MVC) を検索、`Scripts`ファイルの最後のセクション。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 更新プログラム、`Scripts`への参照を追加するセクション、`qrcodejs`ライブラリを追加して、その QR コードを生成するへの呼び出し。 次のようになります。

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

* これらの手順へのリンクと、段落を削除します。

アプリを実行し、QR コードをスキャンおよび認証システムを証明するコードを検証できることを確認します。

## <a name="change-the-site-name-in-the-qr-code"></a>QR コードでは、サイト名を変更します。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

QR コードでは、サイト名は、最初に、プロジェクトを作成するときに、選択したプロジェクト名から取得されます。 探して変更することができます、`GenerateQrCodeUri(string email, string unformattedKey)`メソッドで、 */Areas/Identity/Pages/Account/Manage/EnableAuthenticator.cshtml*します。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

QR コードでは、サイト名は、最初に、プロジェクトを作成するときに、選択したプロジェクト名から取得されます。 探して変更することができます、`GenerateQrCodeUri(string email, string unformattedKey)`メソッドで、 *Pages/Account/Manage/EnableAuthenticator.cshtml.cs* (Razor ページ) ファイルまたは*Controllers/ManageController.cs* (MVC) ファイル。

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

テンプレートから既定のコードは次のようになります。

```csharp
private string GenerateQrCodeUri(string email, string unformattedKey)
{
    return string.Format(
        AuthenicatorUriFormat,
        _urlEncoder.Encode("Razor Pages"),
        _urlEncoder.Encode(email),
        unformattedKey);
}
```

2 番目のパラメーターへの呼び出しで`string.Format`は、サイト名は、ソリューション名から取得されます。 任意の値に変更できますが、エンコードされた URL に常にあります。

## <a name="using-a-different-qr-code-library"></a>別の QR コード ライブラリを使用します。

QR コード ライブラリを優先されるライブラリと置き換えることができます。 HTML が含まれています、`qrCode`要素がどのようなメカニズムによって QR コードを配置する先のライブラリで提供します。

QR コードを正しく書式設定された URL が表示されます。

* `AuthenticatorUri` モデルのプロパティです。
* `data-url` プロパティで、`qrCodeData`要素。

## <a name="totp-client-and-server-time-skew"></a>TOTP クライアントとサーバー時間のずれ

TOTP (時間ベースのワンタイム パスワード) 認証は、正確な時間のあるサーバーと認証システムの両方のデバイスに依存します。 30 秒間のみ最後のトークン。 TOTP 2 fa ログインが失敗する場合は、サーバーの時刻が、精度と正確な NTP サービスに可能であれば同期であることを確認します。

::: moniker-end
