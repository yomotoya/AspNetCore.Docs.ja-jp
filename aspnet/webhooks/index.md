---
uid: webhooks/index
title: "ASP.NET の Webhook の概要 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET の Webhook の概要です。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-overview"></a>ASP.NET の Webhook の概要

Webhook は、Web Api および SaaS サービスを一緒に配線の単純なパブリッシュ/サブスクライブ モデルを提供する簡易 HTTP パターンです。 イベント発生すると、サービスで HTTP POST 要求の形式で登録されたサブスクライバーに通知が送信されます。 POST 要求には、これによりを適宜動作し、受信側のイベントに関する情報が含まれています。

ため、わかりやすくするため、Webhook は既にによって公開されているなどのサービスの数が多い[Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、し、さらに多くです。 たとえば、WebHook がでファイルが変更されたことを指定することができます[Dropbox](http://dropbox.com/)、または、GitHub でコードの変更がコミットされてまたはで支払いが開始されて[PayPal](http://www.paypal.com/)、またはで作成されて、カード[Trello](http://www.trello.com/)です。 表示される値は無限です。

Microsoft ASP.NET Webhook やすく送信し、受信の両方 Webhook、ASP.NET アプリケーションの一部として。

* 受信側では、受信と処理の任意の数の WebHook プロバイダーからの Webhook の共通モデルを提供します。 Out of box のサポートに関して[Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Pusher](http://www.pusher.com)、 [Salesforce](http://www.salesforce.com)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、[WordPress](http://www.wordpress.com)と[Zendesk](https://www.zendesk.com/)が、簡単に複数のサポートを追加します。

* 送信側には、管理およびイベント通知サブスクライバーの適切なセットを送信する場合と同様のサブスクリプションを保存するためのサポートを提供します。 これにより、独自のサブスクライバーがサブスクライブおよび処理が行われる場合に通知できるイベントのセットを定義できます。

2 つの部分は、シナリオに応じて、分解または一緒に使用できます。 受信側の部分のみを使用することができますし、その他のサービスから Webhook を受信するためだけの場合消費する他のユーザーのための Webhook を公開する場合は、のみを行うことができます。

コードが ASP.NET Web API 2 と ASP.NET MVC 5 を対象し、は[github OSS](https://github.com/aspnet/WebHooks)です。

## <a name="webhooks-overview"></a>Webhook の概要

Webhook は、サービス サービスからには使用する方法が異なりますが、基本的な考え方は同じことを意味するパターンです。 のに Webhook の単純なパブリッシュ/サブスクライブ モデルとして、ユーザーが別の場所で発生しているイベントにサブスクライブできます。 イベント通知は、イベント自体についての情報を含む HTTP POST 要求として反映されます。

通常、HTTP POST 要求には、JSON オブジェクトまたは HTML フォームのデータをトリガーする WebHook を引き起こすイベントに関する情報など、WebHook 送信者によって決定が含まれています。 WebHook の POST 要求本文の例ではたとえば、 [GitHub](http://www.github.com/)次のよう特定のリポジトリで開かれる新しい懸案事項の結果として。

```json
{
  "action": "opened",
  "issue": {
      "url": "https://api.github.com/repos/octocat/Hello-World/issues/1347",
      "number": 1347,
      ...
  },
  "repository": {
      "id": 1296269,
      "full_name": "octocat/Hello-World",
      "owner": {
          "login": "octocat",
          "id": 1
          ...
      },
      ...
  },
  "sender": {
      "login": "octocat",
      "id": 1,
      ...
  }
}
```

WebHook が目的の送信者から実際にあることを確認するにはするには、POST 要求は何らかの方法でセキュリティで保護し、その後、受信側で検証します。 たとえば、 [GitHub Webhook](https://developer.github.com/webhooks/)が含まれています、 *X ハブ署名*HTTP ヘッダー心配する必要はありませんので、受信者の実装によってチェックされている要求の本体のハッシュを使用します。

WebHook フローでは、次のような一般的には。

* WebHook の送信者は、クライアントがサブスクライブできるイベントを表示します。 イベント監視可能なに対する変更を記述、システムでは、たとえば新規データ項目が挿入された、プロセスが完了したこと、または、それ以外にされています。

* WebHook の受信者は、4 つの項目から成る WebHook を登録することによってサブスクライブします。

     1. HTTP POST 要求; の形式でのイベント通知の送信先の URI

     2. 一連のフィルターを WebHook を発生させる必要があります。 特定イベントの説明

     3. HTTP POST 要求の署名に使用される秘密キー

     4. これは、HTTP POST 要求に含まれる追加のデータです。 追加の HTTP ヘッダー フィールドまたは HTTP POST 要求本文に含まれているプロパティの指定できます。

* イベントが発生した後は、一致する WebHook 登録が検出され、HTTP POST 要求を送信します。 通常は、HTTP POST 要求の生成では、受信者が応答していない何らかの理由またはエラー応答の HTTP POST 要求の結果の場合数回を再試行します。

## <a name="webhooks-processing-pipeline"></a>Webhook 処理パイプライン

着信の Webhook の Microsoft ASP.NET Webhook 処理パイプラインは、このようになります。

![ASP.NET Webhook 処理パイプライン](_static/WebHookReceivers.png)

2 つの主要概念についてここでは、*レシーバー*と*ハンドラー*:

* *レシーバー* WebHook の特定の差出人からの特定のフレーバーを処理するためとなるように、WebHook 要求実際に目的の送信者からのセキュリティ チェックを適用するために担当します。

* *ハンドラー*は通常、ユーザー コードが実行されている特定の WebHook を処理します。

次のノードの詳細については、これらの概念を説明します。
