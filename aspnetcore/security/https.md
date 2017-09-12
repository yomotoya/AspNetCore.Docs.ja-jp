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
ms.openlocfilehash: 7c366ffbac71152c2f29901ff12bac2962e83e3e
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="setting-up-https-for-development-in-aspnet-core"></a><span data-ttu-id="8e44c-104">ASP.NET Core での開発の HTTPS を設定します。</span><span class="sxs-lookup"><span data-stu-id="8e44c-104">Setting up HTTPS for development in ASP.NET Core</span></span>

> [!NOTE] 
> <span data-ttu-id="8e44c-105">このトピックの対象は ASP.NET Core 2.0 Preview 1</span><span class="sxs-lookup"><span data-stu-id="8e44c-105">This topic applies to ASP.NET Core 2.0 Preview 1</span></span>

<span data-ttu-id="8e44c-106">運用環境で HTTPS をシミュレートする開発中に HTTPS を使用するようアプリケーションを構成することができます。</span><span class="sxs-lookup"><span data-stu-id="8e44c-106">You can configure your application to use HTTPS during development to simulate HTTPS in your production environment.</span></span> <span data-ttu-id="8e44c-107">HTTPS を有効にするは、さまざまな id プロバイダーとの統合を有効にする必要があります (のような[Azure AD](https://azure.microsoft.com/services/active-directory)と[Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/))。</span><span class="sxs-lookup"><span data-stu-id="8e44c-107">Enabling HTTPS may be required to enable integration with various identity providers (like [Azure AD](https://azure.microsoft.com/services/active-directory) and [Azure AD B2C](https://azure.microsoft.com/services/active-directory-b2c/)).</span></span>

<a name="iisxpress"></a>

<span data-ttu-id="8e44c-108">Windows で Visual Studio または IIS Express をインストールしている場合、IIS Express 開発証明書は LocalMachine 証明書ストアになります。</span><span class="sxs-lookup"><span data-stu-id="8e44c-108">On Windows if you’ve installed Visual Studio or IIS Express, the IIS Express Development Certificate will be in your LocalMachine certificate store.</span></span> <span data-ttu-id="8e44c-109">IIS Express 内側で実行されるときに、この証明書を使用する Visual Studio でプロジェクトのプロパティを更新することができます。</span><span class="sxs-lookup"><span data-stu-id="8e44c-109">You can update your project properties in Visual Studio to use this certificate when running behind IIS Express.</span></span>

   * <span data-ttu-id="8e44c-110">ソリューション エクスプ ローラーでプロジェクトを右クリックし、**プロパティ**です。</span><span class="sxs-lookup"><span data-stu-id="8e44c-110">In Solution Explorer, right-click the project and select **Properties**.</span></span>
   * <span data-ttu-id="8e44c-111">左側のウィンドウで次のように選択します。**デバッグ**です。</span><span class="sxs-lookup"><span data-stu-id="8e44c-111">On the left pane, select **Debug**.</span></span>
   * <span data-ttu-id="8e44c-112">確認**SSL を有効にします。**</span><span class="sxs-lookup"><span data-stu-id="8e44c-112">Check **Enable SSL**</span></span>
   * <span data-ttu-id="8e44c-113">SSL URL をコピーして貼り付けます、**アプリの URL**</span><span class="sxs-lookup"><span data-stu-id="8e44c-113">Copy the SSL URL and paste it into the **App URL**</span></span>

![[デバッグの web アプリケーションのプロパティ] タブ](enforcing-ssl/_static/ssl.png)

<span data-ttu-id="8e44c-115">開発がある場合は、IIS Express 開発証明書を使用したり、開発用に新しい証明書を作成できます。</span><span class="sxs-lookup"><span data-stu-id="8e44c-115">For development you can use the IIS Express Development Certificate if it is available, or create a new certificate for development purposes.</span></span> <span data-ttu-id="8e44c-116">開発証明書を構成する必要があります、`appsettings.Development.json`ファイルを実稼働環境では使用されません。</span><span class="sxs-lookup"><span data-stu-id="8e44c-116">The development certificate should be configured in the `appsettings.Development.json` file so that it is not used in production:</span></span>

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

<span data-ttu-id="8e44c-117">この構成で実稼働環境で実行されているアプリ「証明書がありません 'HTTPS'、現在の環境 (運用) 構成内に見つからないという名前」というメッセージが例外がスローされます。</span><span class="sxs-lookup"><span data-stu-id="8e44c-117">An app with this configuration running in production will throw an exception saying "No certificate named 'HTTPS' found in configuration for the current environment (Production)".</span></span> <span data-ttu-id="8e44c-118">切り替えるには、[環境](xref:fundamentals/environments)に`Development`、設定、`ASPNETCORE_ENVIRONMENT`環境変数を`Development`です。</span><span class="sxs-lookup"><span data-stu-id="8e44c-118">To switch the [environment](xref:fundamentals/environments) to `Development`, set the `ASPNETCORE_ENVIRONMENT` environment variable to `Development`.</span></span>

<span data-ttu-id="8e44c-119">場合は、IIS Express 開発証明書をインストールすることはありません、することができます開発証明書自分で作成します。</span><span class="sxs-lookup"><span data-stu-id="8e44c-119">If you do not have the IIS Express Development Certificate installed, you can create a development certificate yourself.</span></span> <span data-ttu-id="8e44c-120">Windows では、開発の証明書を作成し、管理者特権のプロンプトで次の PowerShell コマンドを実行して、現在のユーザーの信頼されたルート ストアに追加。</span><span class="sxs-lookup"><span data-stu-id="8e44c-120">On Windows you can create a development certificate and add it to the trusted root store for the current user by running the following PowerShell commands in an elevated prompt:</span></span>

```powershell
$cert = New-SelfSignedCertificate -Subject localhost -DnsName localhost -FriendlyName "ASP.NET Core Development" -KeyUsage DigitalSignature -TextExtension @("2.5.29.37={text}1.3.6.1.5.5.7.3.1") 
Export-Certificate -Cert $cert -FilePath cert.cer
Import-Certificate -FilePath cert.cer -CertStoreLocation cert:/CurrentUser/Root
```

<a name="OpenSSL"></a>

## <a name="kestrel-on--macos-and-linux"></a><span data-ttu-id="8e44c-121">MacOS および Linux で kestrel</span><span class="sxs-lookup"><span data-stu-id="8e44c-121">Kestrel on  macOS and Linux</span></span>

<span data-ttu-id="8e44c-122">必要な IP アドレス、ポート、および証明書でエンドポイントを構成して HTTPS 経由でリッスンするように Kestrel を構成できます。</span><span class="sxs-lookup"><span data-stu-id="8e44c-122">You can  configure Kestrel to listen over HTTPS by configuring an endpoint with the desired IP address, port, and certificate.</span></span> <span data-ttu-id="8e44c-123">証明書が構成済みのインラインを指定できますまたはトップレベル`Certificates`セクションし、名前によって参照します。</span><span class="sxs-lookup"><span data-stu-id="8e44c-123">The certificate can be configured inline, or in the top-level `Certificates` section and then referenced by name:</span></span>

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

<span data-ttu-id="8e44c-124">MacOS および Linux では、HTTPS を使用するための自己署名証明書を作成することができます[OpenSSL](https://www.openssl.org/):</span><span class="sxs-lookup"><span data-stu-id="8e44c-124">On macOS and Linux you can create a self-signed certificate for HTTPS using [OpenSSL](https://www.openssl.org/):</span></span>

```bash
openssl req -new -x509 -newkey rsa:2048 -keyout localhost.key -out localhost.cer -days 365 -subj /CN=localhost
openssl pkcs12 -export -out certificate.pfx -inkey localhost.key -in localhost.cer
```

<span data-ttu-id="8e44c-125">1 回、`certificate.pfx`ファイルが生成されてで HTTPS 証明書を構成、`appsettings.Development.json`ファイル。</span><span class="sxs-lookup"><span data-stu-id="8e44c-125">Once the `certificate.pfx` file has been generated, configure the HTTPS certificate in your `appsettings.Development.json` file:</span></span>

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

<span data-ttu-id="8e44c-126">"証明書: HTTPS:Password"構成プロパティを設定して、証明書のパスフレーズを指定する必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e44c-126">You will also need to specify the passphrase for the certificate by setting the “Certificates:HTTPS:Password” config property.</span></span> <span data-ttu-id="8e44c-127">パスワードをプレーン テキストで格納されません必要があります。</span><span class="sxs-lookup"><span data-stu-id="8e44c-127">Passwords should not be stored in plain text.</span></span> <span data-ttu-id="8e44c-128">参照してください[セーフである記憶域のアプリ シークレット開発時に](app-secrets.md)証明書のパスフレーズを適切な処理です。</span><span class="sxs-lookup"><span data-stu-id="8e44c-128">See [Safe Storage of App Secrets During Development](app-secrets.md) for appropriate handling of the certificate passphrase.</span></span>

<span data-ttu-id="8e44c-129">Macos することができます[、キーチェーンに証明書を追加する](https://support.apple.com/kb/PH20129?locale=en_US)と[信頼設定を変更](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US)開発中に HTTPS に対して信頼されているようにします。</span><span class="sxs-lookup"><span data-stu-id="8e44c-129">On macOS you can [add the certificate to your keychain](https://support.apple.com/kb/PH20129?locale=en_US) and [change its trust settings](https://support.apple.com/kb/PH20127?locale=en_US&viewlocale=en_US) so that it is trusted for HTTPS during development.</span></span> <span data-ttu-id="8e44c-130">キーチェーンに証明書を追加する (の該当するショートカットは、 `CurrentUser/My` Windows に格納)、次のコマンドを実行します。</span><span class="sxs-lookup"><span data-stu-id="8e44c-130">To add the certificate to your keychain (the equivalent of the `CurrentUser/My` store on Windows) run the following command:</span></span>

```bash
security import certificate.pfx -k ~/Library/Keychains/login.keychain-db
```

<span data-ttu-id="8e44c-131">証明書を信頼する:</span><span class="sxs-lookup"><span data-stu-id="8e44c-131">And then to trust the certificate:</span></span>

```bash
security add-trusted-cert localhost.cer
```

<span data-ttu-id="8e44c-132">次のように開発でこの証明書を使用するアプリを構成できます。</span><span class="sxs-lookup"><span data-stu-id="8e44c-132">You can then configure your app to use this certificate in development like this:</span></span>

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
