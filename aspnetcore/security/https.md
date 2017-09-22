---
title: "ASP.NET Core での開発の HTTPS を設定します。"
author: Rick-Anderson
description: "ASP.NET Core 2.0 での開発の HTTPS を設定する方法を示します。"
keywords: "ASP.NET Core、SSL は、HTTPS"
ms.author: riande
manager: wpickett
ms.date: 05/10/2017
ms.topic: article
ms.assetid: 94f2f1a4-7d46-45e2-a085-a57916e41724
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/https
ms.openlocfilehash: 7913758ffa045dcf884d73eed9bab223b8d2c3fe
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a>ASP.NET Core での開発の HTTPS を設定します。

> [!NOTE] 
> このトピックの対象は ASP.NET Core 2.0 Preview 1

運用環境で HTTPS をシミュレートする開発中に HTTPS を使用するようアプリケーションを構成することができます。 HTTPS を有効にするは、さまざまな id プロバイダーとの統合を有効にする必要があります (のような[Azure AD](https://azure.microsoft.com/services/active-directory)と[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/))。

<a name="iisxpress"></a>

Windows で Visual Studio または IIS Express をインストールしている場合、IIS Express 開発証明書は LocalMachine 証明書ストアになります。 IIS Express 内側で実行されるときに、この証明書を使用する Visual Studio でプロジェクトのプロパティを更新することができます。

   * ソリューション エクスプ ローラーでプロジェクトを右クリックし、**プロパティ**です。
   * 左側のウィンドウで次のように選択します。**デバッグ**です。
   * 確認**SSL を有効にします。**
   * SSL URL をコピーして貼り付けます、**アプリの URL**

![[デバッグの web アプリケーションのプロパティ] タブ](enforcing-ssl/_static/ssl.png)

開発がある場合は、IIS Express 開発証明書を使用したり、開発用に新しい証明書を作成できます。 開発証明書を構成する必要があります、`appsettings.Development.json`ファイルを実稼働環境では使用されません。

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "LocalMachine",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```

この構成で実稼働環境で実行されているアプリ「証明書がありません 'HTTPS'、現在の環境 (運用) 構成内に見つからないという名前」というメッセージが例外がスローされます。 切り替えるには、[環境](xref:fundamentals/environments)に`Development`、設定、`ASPNETCORE_ENVIRONMENT`環境変数を`Development`です。

場合は、IIS Express 開発証明書をインストールすることはありません、することができます開発証明書自分で作成します。 Windows では、開発の証明書を作成し、管理者特権のプロンプトで次の PowerShell コマンドを実行して、現在のユーザーの信頼されたルート ストアに追加。

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a>MacOS および Linux で kestrel

必要な IP アドレス、ポート、および証明書でエンドポイントを構成して HTTPS 経由でリッスンするように Kestrel を構成できます。 証明書が構成済みのインラインを指定できますまたはトップレベル`Certificates`セクションし、名前によって参照します。

```json
{
  "Kestrel": {
    "Endpoints": {
      "LocalhostHttps": {
        "Address": "127.0.0.1",
        "Port": "43434",
        "Certificate": "HTTPS"
      }
    }
  }
}

```

MacOS および Linux では、HTTPS を使用するための自己署名証明書を作成することができます[OpenSSL](https://www.openssl.org/):

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

1 回、`certificate.pfx`ファイルが生成されてで HTTPS 証明書を構成、`appsettings.Development.json`ファイル。

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "File",
      "Path": "certificate.pfx"
    }
  }
}
```

"証明書: HTTPS:Password"構成プロパティを設定して、証明書のパスフレーズを指定する必要があります。 パスワードをプレーン テキストで格納されません必要があります。 参照してください[セーフである記憶域のアプリ シークレット開発時に](app-secrets.md)証明書のパスフレーズを適切な処理です。

Macos することができます[、キーチェーンに証明書を追加する](https://support.apple.com/kb/PH20129?locale=en_US)と[信頼設定を変更](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US)開発中に HTTPS に対して信頼されているようにします。 キーチェーンに証明書を追加する (の該当するショートカットは、 `CurrentUser/My` Windows に格納)、次のコマンドを実行します。

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

証明書を信頼する:

```bash
security add-trusted-cert localhost.cer
```

次のように開発でこの証明書を使用するアプリを構成できます。

```json
{
  "Certificates": {
    "HTTPS": {
      "Source": "Store",
      "StoreLocation": "CurrentUser",
      "StoreName": "My",
      "Subject": "CN=localhost",
      "AllowInvalid": true
    }
  }
}
```
