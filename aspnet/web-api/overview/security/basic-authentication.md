---
uid: web-api/overview/security/basic-authentication
title: ASP.NET Web API で基本認証 |Microsoft ドキュメント
author: MikeWasson
description: ASP.NET Web API で基本認証の使用について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 4b8e6410668b2db289488bb4b6cd26d881e70f4c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508131"
---
<a name="basic-authentication-in-aspnet-web-api"></a>ASP.NET Web API で基本認証
====================
によって[Mike Wasson](https://github.com/MikeWasson)

基本認証が定義されている[RFC 2617、HTTP Authentication: Basic and Digest アクセス Authentication](http://www.ietf.org/rfc/rfc2617.txt)です。

短所

- ユーザーの資格情報は、要求で送信されます。
- 資格情報は、プレーン テキストとして送信されます。
- 資格情報は、すべての要求で送信されます。
- ログアウト、方法はありません以外は、ブラウザー セッションを終了します。
- クロスサイト リクエスト フォージェリ (CSRF); に対する脆弱性アンチ CSRF メジャーが必要です。

長所

- インターネット標準です。
- すべての主要なブラウザーでサポートされます。
- 比較的単純なプロトコルです。

基本認証はように機能します。

1. 要求に認証が必要な場合、サーバーは 401 (Unauthorized) を返します。 応答には、サーバーが基本認証をサポートしていることを示す WWW 認証ヘッダーが含まれています。
2. クライアントは、承認ヘッダーに、クライアント資格情報で、別の要求を送信します。 資格情報は、「名前: パスワード」, base64 でエンコードされた文字列として書式設定します。 資格情報は暗号化されません。

基本認証は、"realm"のコンテキスト内で実行されます。 サーバーには、WWW 認証ヘッダー内の領域の名前が含まれています。 ユーザーの資格情報は、その領域内で有効です。 レルムの正確なスコープは、サーバーによって定義されます。 たとえば、パーティションのリソースへの順序でいくつかの領域を定義する可能性があります。

![](basic-authentication/_static/image1.png)

資格情報が送信されるため暗号化されていない、基本認証は HTTPS 経由でセキュリティで保護されただけです。 参照してください[Web API での SSL の操作](working-with-ssl-in-web-api.md)です。

基本認証は、CSRF 攻撃に対する脆弱性もできます。 資格情報を入力すると後、ブラウザーに自動的に送信に以降の要求で、同じドメイン、セッションの間です。 これには、AJAX 要求が含まれます。 参照してください[クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ](preventing-cross-site-request-forgery-csrf-attacks.md)です。

## <a name="basic-authentication-with-iis"></a>IIS と基本認証

IIS 基本認証をサポートしていますが、注意: ユーザーは自分の Windows 資格情報に対して認証されます。 つまり、ユーザーでは、サーバーのドメインにアカウントが必要です。 公開された web サイトでは、通常する ASP.NET メンバーシップ プロバイダーに対して認証します。

IIS を使用して基本認証を有効にするには、ASP.NET プロジェクトの Web.config の"Windows"に、認証モードを設定します。

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

このモードでは、IIS は認証に Windows 資格情報を使用します。 さらに、IIS で基本認証を有効にする必要があります。 IIS マネージャーは、機能ビューに移動して、認証を選択し、基本認証を有効にします。

![](basic-authentication/_static/image2.png)

Web API プロジェクトで、追加、`[Authorize]`認証を必要とするコント ローラーのアクションの属性です。

クライアントに対して自身を認証要求で Authorization ヘッダーを設定します。 ブラウザー クライアントでは、自動的にこの手順を実行します。 Nonbrowser クライアントは、ヘッダーを設定する必要があります。

## <a name="basic-authentication-with-custom-membership"></a>カスタム メンバーシップと基本認証

前述のように、IIS に組み込まれている基本認証は Windows 資格情報を使用します。 つまり、ホスト サーバー上のユーザー用のアカウントを作成する必要があります。 インターネット アプリケーションのユーザー アカウントは通常、外部データベースに格納します。

次のコードで基本認証を実行する HTTP モジュールの方法です。 簡単に置き換えることで、ASP.NET メンバーシップ プロバイダーに接続できます、`CheckPassword`メソッドで、この例ではダミー メソッドです。

Web API 2 の書き込みをする必要があります、[認証フィルター](authentication-filters.md)または[OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/index.md)、HTTP モジュールの代わりにします。

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

HTTP モジュールを有効にするには、web.config ファイルに次の追加、 **system.webServer**セクション。

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

"YourAssemblyName"("dll"拡張子を含まない) アセンブリの名前に置き換えます。

フォームまたは Windows 認証など、他の認証スキームを無効にする必要があります。
