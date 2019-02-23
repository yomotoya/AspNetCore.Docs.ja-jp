---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: Web API の SSL の使用 |Microsoft Docs
author: MikeWasson
description: SSL クライアント証明書の使用など、ASP.NET Web API を使用した SSL を使用する方法を示します。
ms.author: riande
ms.date: 02/22/2019
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 69c0d217f605096d968435c062ee9931f8dff75f
ms.sourcegitcommit: b3894b65e313570e97a2ab78b8addd22f427cac8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 02/23/2019
ms.locfileid: "56744106"
---
<a name="working-with-ssl-in-web-api"></a>Web API の SSL の使用
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

いくつかの一般的な認証方式は、プレーンな HTTP は安全ではありません。 具体的には、基本認証とフォーム認証、暗号化されていない資格情報を送信します。 セキュリティ保護、これらの認証スキーム*する必要があります*SSL を使用します。 さらに、SSL クライアント証明書は、クライアントの認証に使用できます。

## <a name="enabling-ssl-on-the-server"></a>サーバーで SSL を有効にします。

IIS 7 またはそれ以降の SSL を設定します。

- 作成または証明書を取得します。 テストは、自己署名証明書を作成できます。
- HTTPS バインドを追加します。

詳細については、次を参照してください。 [IIS 7 で SSL の設定方法](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)します。

ローカル テストでは、Visual Studio からの IIS Express での SSL を有効にできます。 [プロパティ] ウィンドウで次のように設定します。 **SSL が有効になっている**に**True**します。 値に注意してください**SSL URL**; HTTPS 接続をテストするため、この URL を使用します。

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Web API コント ローラーでの SSL の適用

HTTPS と HTTP バインディングの両方があれば、クライアントがサイトにアクセスするのに HTTP を使用できます。 その他のリソースには SSL が必要ですが、HTTP を利用する一部のリソースを許可する場合があります。 その場合は、アクション フィルターを使用して、保護されたリソースに対して SSL を要求します。 次のコードは、SSL をチェックする Web API 認証フィルターを示しています。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

SSL を必要とする Web API アクションすべてには、このフィルターを追加します。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL クライアント証明書

SSL は、公開キー インフラストラクチャ証明書を使用して認証を提供します。 サーバーは、クライアントにサーバーを認証する証明書を提供する必要があります。 あまり一般的で、サーバーに証明書を提供するクライアントですが、これはクライアントを認証するための 1 つのオプションです。 Ssl クライアント証明書を使用するには、ユーザーに署名証明書を配布する方法が必要です。 多くのアプリケーションの種類、これは、優れたユーザー エクスペリエンスになりませんが一部の環境 (たとえば、enterprise) で可能な場合があります。

| 長所 | 短所 |
| --- | --- |
| -証明書資格情報では、ユーザー名/パスワードよりも強力です。 SSL は、認証、メッセージの整合性、およびメッセージの暗号化との完全なセキュリティで保護されたチャネルを提供します。 | 取得して、PKI 証明書の管理-する必要があります。 -クライアントのプラットフォームでは、SSL クライアント証明書をサポートする必要があります。 |

でクライアント証明書を受け入れるように IIS を構成するには、IIS マネージャーを開き、次の手順を実行します。

1. ツリー ビューで [サイト] ノードをクリックします。
2. ダブルクリックして、 **SSL 設定**中央のペインで機能します。
3. **クライアント証明書**、これらのオプションのいずれかを選択します。 

    - **受け入れる**:IIS は、クライアントからの証明書を受け取りますが、1 つは必要ありません。
    - **必要な**:クライアント証明書が必要です。 (このオプションを有効にするには、必要がありますも選択する"SSL")

ApplicationHost.config ファイルでこれらのオプションを設定することもできます。

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert**フラグは、IIS は、クライアントからの証明書を受け取りますが、(IIS マネージャーで"Accept"オプションに相当) の 1 つは必要ありません。 証明書を要求するには、設定、 **SslRequireCert**フラグ。 テストは、ローカル applicationhost でこれらのオプションで IIS Express を設定することもできます。"Documents\IISExpress\config"にある構成ファイル。

### <a name="creating-a-client-certificate-for-testing"></a>テスト用クライアント証明書の作成

テスト目的で、使用することができます[MakeCert.exe](/windows/desktop/SecCrypto/makecert)クライアント証明書を作成します。 まず、テスト ルート証明機関を作成します。

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert は、秘密キーのパスワードを入力することを求められます。

次に、サーバーの"信頼されたルート証明機関"ストアには、次のように、テストに、証明書を追加します。

1. MMC を開きます。
2. **ファイル**、**スナップインの追加と削除**します。
3. 選択**コンピューター アカウント**します。
4. 選択**ローカル コンピューター**ウィザードを完了します。
5. ナビゲーション ウィンドウの下には、「信頼されたルート証明機関」ノードを展開します。
6. **アクション**メニューで、**すべてのタスク**、順にクリックします**インポート**証明書のインポート ウィザードを開始します。
7. TempCA.cer を証明書ファイルを参照します。
8. をクリックして**オープン**、順にクリックします**次**ウィザードを完了します。 (求め、パスワードを再入力します。)

今すぐ最初の証明書によって署名されているクライアント証明書を作成します。

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Web API のクライアント証明書の使用

サーバー側では、呼び出すことによって、クライアント証明書を取得できます[GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx)要求メッセージ。 クライアント証明書がない場合は、null、メソッドを返します。 それ以外を返します、 **X509Certificate2**インスタンス。 このオブジェクトを使用して、証明書の発行者とサブジェクトなどから情報を取得します。 認証や承認のこの情報を使用することができます。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
