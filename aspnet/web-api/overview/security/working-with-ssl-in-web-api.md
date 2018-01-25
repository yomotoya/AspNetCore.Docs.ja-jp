---
uid: web-api/overview/security/working-with-ssl-in-web-api
title: "Web API での SSL の操作 |Microsoft ドキュメント"
author: MikeWasson
description: "SSL クライアント証明書の使用など、ASP.NET Web API で SSL を使用する方法を示します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 97f6164f-59cf-45c0-b820-e4aa29b45396
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/working-with-ssl-in-web-api
msc.type: authoredcontent
ms.openlocfilehash: 127b336cb628e55bd59481ecb1c4df83960dc25b
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="working-with-ssl-in-web-api"></a>Web API での SSL の操作
====================
によって[Mike Wasson](https://github.com/MikeWasson)

いくつかの一般的な認証方式は、プレーンな HTTP では安全ではありません。 具体的には、基本認証とフォーム認証は、暗号化されていない資格情報を送信します。 をセキュリティで保護するこれらの認証スキーム*必要があります*SSL を使用します。 さらに、SSL クライアント証明書は、クライアントの認証に使用できます。

## <a name="enabling-ssl-on-the-server"></a>サーバーで SSL を有効にします。

IIS 7 またはそれ以降の SSL を設定します。

- 作成または証明書を取得します。 テストは、自己署名証明書を作成できます。
- HTTPS バインドを追加します。

詳細については、「 [IIS 7 で SSL を設定する方法](https://www.iis.net/learn/manage/configuring-security/how-to-set-up-ssl-on-iis)です。

ローカル テストでは、Visual Studio からの IIS Express では SSL を有効にできます。 [プロパティ] ウィンドウで次のように設定します。 **SSL が有効になっている**に**True**です。 値に注意してください**SSL URL**; この URL を使用して、HTTPS 接続をテストします。

![](working-with-ssl-in-web-api/_static/image1.png)

### <a name="enforcing-ssl-in-a-web-api-controller"></a>Web API コント ローラーでの SSL を適用します。

HTTPS と HTTP バインドの両方がある場合は、サイトへのアクセス、クライアントも HTTP を使用することができます。 一部のリソース、その他のリソースには、SSL が必要とする、HTTP を使用することができます。 その場合は、アクション フィルターを使用して、保護されたリソースに対して SSL を要求します。 次のコードは、SSL をチェックする Web API 認証フィルターを示しています。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample1.cs)]

SSL を必要とする Web API アクションには、このフィルターを追加します。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample2.cs)]

## <a name="ssl-client-certificates"></a>SSL クライアント証明書

SSL は、公開キー基盤の証明書を使用して認証を提供します。 サーバーは、クライアントにサーバーを認証する証明書を提供する必要があります。 あまり一般的では、サーバーに証明書を提供するためのクライアントが、これは、クライアントを認証するための 1 つのオプションです。 クライアント証明書を使用して、SSL を使用して、ユーザーに署名証明書を配布する方法が必要です。 多くのアプリケーションの種類、これは、優れたユーザー エクスペリエンスをしないでが一部の環境 (たとえば、enterprise) で可能な場合があります。

| 長所 | 短所 |
| --- | --- |
| -証明書資格情報は、ユーザー名/パスワードよりも強力です。 SSL は、認証、メッセージの整合性、およびメッセージの暗号化との完全なセキュリティで保護されたチャネルを提供します。 | 取得して、PKI 証明書の管理-必要があります。 -クライアントのプラットフォームでは、SSL クライアント証明書をサポートする必要があります。 |

クライアント証明書を受け入れるように IIS を構成するのに、IIS マネージャーを開き、次の手順を実行します。

1. ツリー ビューで、[サイト] ノードをクリックします。
2. ダブルクリックして、 **SSL 設定**中央のペインで機能します。
3. **クライアント証明書**、これらのオプションのいずれかを選択します。 

    - **受け入れる**: IIS は、クライアントからの証明書を受け取りますが、1 つは必要ありません。
    - **必要な**: クライアント証明書を要求します。 (このオプションを有効にするには、必要がありますも選択する"SSL")

ApplicationHost.config ファイルにこれらのオプションを設定することもできます。

[!code-xml[Main](working-with-ssl-in-web-api/samples/sample3.xml)]

**SslNegotiateCert**フラグは、IIS は、クライアントからの証明書を受け入れますは必要ありません (IIS マネージャーで「受け入れる」オプションに相当) のいずれかを意味します。 証明書を要求するには、設定、 **SslRequireCert**フラグ。 テストは、ローカル applicationhost でこれらのオプション IIS Express で、設定することもできます。"Documents\IISExpress\config"にある構成ファイル。

### <a name="creating-a-client-certificate-for-testing"></a>テスト用クライアント証明書を作成します。

テストの目的で使用することができます[MakeCert.exe](https://msdn.microsoft.com/library/bfsktky3.aspx)クライアント証明書を作成します。 まず、テスト ルート証明機関を作成します。

[!code-console[Main](working-with-ssl-in-web-api/samples/sample4.cmd)]

Makecert は、秘密キーのパスワードを入力することを求められます。

次に、テストの次のようにサーバーの"信頼されたルート証明機関"ストアに証明書を追加します。

1. MMC を開きます。
2. **ファイル****スナップインの追加と削除**です。
3. 選択**コンピューター アカウント**です。
4. 選択**ローカル コンピューター**ウィザードを完了します。
5. ナビゲーション ペインの下には、「信頼されたルート証明機関」ノードを展開します。
6. **アクション** メニューのをポイント**すべてのタスク**、クリックして**インポート**証明書のインポート ウィザードを起動します。
7. TempCA.cer を証明書ファイルを参照します。
8. をクリックして**開く**、順にクリックして**[次へ]**ウィザードを完了します。 (求められます、パスワードを再入力します。)

最初の証明書によって署名されているクライアント証明書を作成します。

[!code-console[Main](working-with-ssl-in-web-api/samples/sample5.cmd)]

### <a name="using-client-certificates-in-web-api"></a>Web API でのクライアント証明書の使用

呼び出して、クライアント証明書を取得するサーバー側で[GetClientCertificate](https://msdn.microsoft.com/library/system.net.http.httprequestmessageextensions.getclientcertificate.aspx)要求メッセージにします。 メソッドは、クライアント証明書がない場合は null を返します。 返しますそれ以外の場合、 **X509Certificate2**インスタンス。 このオブジェクトを使用して、証明書の発行者や件名などから情報を取得します。 認証および承認のこの情報を使用することができます。

[!code-csharp[Main](working-with-ssl-in-web-api/samples/sample6.cs)]
