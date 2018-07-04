---
uid: web-api/overview/security/basic-authentication
title: ASP.NET Web API の基本的な認証 |Microsoft Docs
author: MikeWasson
description: ASP.NET Web API で基本認証の使用について説明します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/02/2014
ms.topic: article
ms.assetid: 41423767-0021-47c3-9e53-0021b457c39f
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/basic-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9d5610eb61088a8e7573ba6399c771f0957ff437
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395332"
---
<a name="basic-authentication-in-aspnet-web-api"></a>ASP.NET Web API の基本認証
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

基本認証が定義されている[RFC 2617、HTTP 認証: 基本認証とダイジェスト アクセス認証](http://www.ietf.org/rfc/rfc2617.txt)します。

短所

- ユーザーの資格情報は、要求で送信されます。
- 資格情報は、プレーン テキストとして送信されます。
- 資格情報は、すべての要求で送信されます。
- ログアウトする方法がない以外のブラウザー セッションを終了します。
- クロスサイト リクエスト フォージェリ (CSRF); に対して脆弱になりますCSRF 対策メジャーが必要です。

長所

- インターネットの標準です。
- すべての主要ブラウザーでサポートされています。
- 比較的単純なプロトコルです。

基本認証はように動作します。

1. 要求に認証が必要な場合、サーバーは 401 (Unauthorized) を返します。 応答には、サーバーが基本認証をサポートしていることを示す WWW 認証ヘッダーが含まれています。
2. クライアントは、Authorization ヘッダーでクライアント資格情報を使って、別の要求を送信します。 資格情報は、「名前: パスワード」, base64 でエンコードされた文字列としてフォーマットされます。 資格情報は暗号化されません。

基本認証は、「領域」のコンテキスト内で実行されます。 サーバーには、WWW 認証ヘッダー内の領域の名前が含まれています。 ユーザーの資格情報は、その領域内で有効です。 レルムの正確なスコープは、サーバーによって定義されます。 たとえば、パーティション リソースへの順序でいくつかの領域を定義する場合があります。

![](basic-authentication/_static/image1.png)

資格情報は暗号化されずに送信されて、ため、基本認証が HTTPS 経由でセキュリティで保護されただけです。 参照してください[Web API の SSL の使用](working-with-ssl-in-web-api.md)します。

基本認証は CSRF 攻撃に対する脆弱性もあります。 ユーザーの資格情報を入力すると、ブラウザーに自動的に送信に後続の要求では同じドメインに、セッションの期間の。 これには、AJAX 要求が含まれます。 参照してください[クロスサイト リクエスト フォージェリ (CSRF) 攻撃を防ぐ](preventing-cross-site-request-forgery-csrf-attacks.md)します。

## <a name="basic-authentication-with-iis"></a>IIS を使用した基本認証

IIS 基本認証をサポートしていますが、警告がある: ユーザーが自分の Windows 資格情報に対して認証します。 つまり、ユーザーでは、サーバーのドメインにアカウントが必要です。 パブリックに公開された web サイトでは、通常、ASP.NET メンバーシップ プロバイダーに対する認証にします。

IIS を使用して基本認証を有効にするには、ASP.NET プロジェクトの Web.config の"Windows"を認証モードを設定します。

[!code-xml[Main](basic-authentication/samples/sample1.xml)]

このモードでは、IIS は、認証に Windows 資格情報を使用します。 さらに、IIS での基本認証を有効にする必要があります。 IIS マネージャーで、機能ビューに移動するには、認証 を選択および基本認証を有効にします。

![](basic-authentication/_static/image2.png)

Web API プロジェクトで、追加、`[Authorize]`認証を必要とする任意のコント ローラー アクションの属性。

クライアントは、要求に Authorization ヘッダーを設定して自身を認証します。 ブラウザー クライアントでは、自動的にこの手順を実行します。 Nonbrowser クライアントは、ヘッダーを設定する必要があります。

## <a name="basic-authentication-with-custom-membership"></a>カスタムのメンバシップを使用した基本認証

前述のように、IIS に組み込まれている基本認証は、Windows 資格情報を使用します。 ホスティング サーバー上のユーザー用のアカウントを作成する必要があることを意味します。 インターネット アプリケーションのユーザー アカウントは通常外部データベースに格納します。

次のコードでどのように基本認証を実行する HTTP モジュールです。 簡単に置き換えることで、ASP.NET メンバーシップ プロバイダーにプラグインできる、`CheckPassword`メソッドで、この例ではダミー メソッドです。

Web API 2 での書き込みをする必要があります、[認証フィルター](authentication-filters.md)または[OWIN ミドルウェア](../../../aspnet/overview/owin-and-katana/index.md)、HTTP モジュールの代わりにします。

[!code-csharp[Main](basic-authentication/samples/sample2.cs)]

HTTP モジュールを有効にするには、web.config ファイルに、次を追加、 **system.webServer**セクション。

[!code-xml[Main](basic-authentication/samples/sample3.xml?highlight=4)]

「アセンブリ名」("dll"拡張子を含まない) アセンブリの名前に置き換えます。

フォームまたは Windows 認証などの他の認証スキームが無効にする必要があります。
