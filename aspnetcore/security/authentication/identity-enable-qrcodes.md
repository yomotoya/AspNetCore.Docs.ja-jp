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
ms.openlocfilehash: 36a3dc542f3321c5e6ebaa078efd8bde3f50948f
ms.sourcegitcommit: e4a1df2a5a85f299322548809e547a79b380bb92
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/29/2017
---
# <a name="enabling-qr-code-generation-for-authenticator-apps-in-aspnet-core"></a>ASP.NET Core でのアプリの認証子の QR コード生成を有効にします。

注: このトピックの対象は ASP.NET Core 2.x

ASP.NET Core は、個別の認証の認証システム アプリケーションのサポートに付属します。 使用して、時間ベース ワンタイム パスワード アルゴリズム (TOTP)、2 要素認証 (2 fa) 認証アプリとは、2 fa の approch をお勧め業界です。 2 fa TOTP を使用してを SMS 2 fa をお勧めします。 認証アプリでは、ユーザー名とパスワードを確認した後どのユーザーが入力する必要があります、6 ~ 8 桁のコードを提供します。 通常、認証アプリがスマート フォンにインストールされます。

ASP.NET Core web アプリ テンプレートでは、認証のサポートは、QRCode 生成のサポートを提供しません。 QRCode ジェネレーター 2 fa のセットアップが容易になります。 このドキュメントのガイドの追加方法[QR コード](https://wikipedia.org/wiki/QR_code)2 fa の構成 ページを生成します。

## <a name="adding-qr-codes-to-the-2fa-configuration-page"></a>2 fa 構成 ページに QR コードを追加します。

これらの手順を使用して*qrcode.js* https://davidshimjs.github.io/qrcodejs/ リポジトリからです。

* ダウンロード、 [qrcode.js javascript ライブラリ](https://davidshimjs.github.io/qrcodejs/)を`wwwroot\lib`プロジェクトのフォルダーにします。

* *Pages\Account\Manage\EnableAuthenticator.cshtml* (Razor ページ) または*Views\Account\Manage\EnableAuthenticator.cshtml* (MVC)、検索、`Scripts`ファイルの最後のセクション。

```cshtml
@section Scripts {
    @await Html.PartialAsync("_ValidationScriptsPartial")
}
```

* 更新プログラム、`Scripts`への参照を追加するセクション、 `qrcodejs` QR コードの生成を呼び出すと追加するライブラリ。 次のようになります。

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

* これらの手順にリンクする段落を削除します。

アプリを実行して、QR コードをスキャンおよび性は、認証コードを検証できることを確認してください。

## <a name="change-the-site-name-in-the-qr-code"></a>QR コード内で、サイト名を変更します。

QR コード内で、サイト名は、最初に、プロジェクトの作成時に選択したプロジェクト名から取得されます。 探して変更することができます、`GenerateQrCodeUri(string email, string unformattedKey)`メソッドで、 *EnableAuthenticator.cshtml.cs*ファイル。 

テンプレートから既定のコードは次のようになります。

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

呼び出しで 2 番目のパラメーター`string.Format`は、ソリューションの名前から取得された、サイト名です。 任意の値に変更できますが、URL エンコードは常にあります。

## <a name="using-a-different-qr-code-library"></a>別の QR コード ライブラリを使用します。

QR コード ライブラリを優先されるライブラリと置き換えることができます。 HTML が含まれています、`qrCode`要素にどのようなメカニズムによって QR コードを配置することができます、ライブラリを提供します。

QR コードを正しく書式設定された URL はで使用します。

* `AuthenticatorUri`モデルのプロパティです。
* `data-url`プロパティに、`qrCodeData`要素。 

使用して`@Html.Raw`ビューのモデル プロパティにアクセスする (それ以外の場合、url のアンパサンドは倍精度浮動小数点でエンコードされたおよび QR コードのラベル パラメーターは無視されます)。

## <a name="totp-client-and-server-time-skew"></a>TOTP クライアントとサーバー時間のずれ

TOTP 認証は、正確な時間のあるサーバーと認証子の両方のデバイスに依存します。 トークンの寿命は 30 秒です。 TOTP 2 fa ログインが失敗した場合は、サーバーの時刻が、精度は、可能であれば正確な NTP サービスに同期されることを確認します。
