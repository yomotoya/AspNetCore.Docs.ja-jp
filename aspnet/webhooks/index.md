---
uid: webhooks/index
title: ASP.NET Webhook の概要 |Microsoft Docs
author: rick-anderson
description: ASP.NET Webhook の紹介です。
ms.author: riande
ms.date: 01/17/2012
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.openlocfilehash: 702cc0bf0d0bb887c64bec19e1faf249bd96617a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "48254960"
---
# <a name="aspnet-webhooks-overview"></a>ASP.NET Webhook の概要

Webhook は、Web Api と SaaS サービスをまとめて配線の単純なパブリッシュ/サブスクライブ モデルを提供する軽量な HTTP パターンです。 サービスで、イベントが発生したときに通知が登録されているサブスクライバーに HTTP POST 要求の形式で送信されます。 POST 要求には、それに従って動作する受信者を表すことができます、イベントに関する情報が含まれています。

Webhook を既になどのサービスの数が多いによって公開のためのわかりやすいように、 [Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、その他。 たとえば、WebHook がのファイルが変更されたことを指定することができます[Dropbox](http://dropbox.com/)、または GitHub でコード変更がコミットされたまたはで支払いが開始された[PayPal](http://www.paypal.com/)、カードが作成されてまたは[Trello](http://www.trello.com/)します。 可能性は無限です。

Microsoft ASP.NET Webhook 簡単に送信および ASP.NET アプリケーションの一部として Webhook を受信します。

* 受信側では、受信および処理の任意の数の WebHook プロバイダーからの Webhook の一般的なモデルを提供します。 対応のボックスからの復帰[Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Pusher](http://www.pusher.com)、 [Salesforce](http://www.salesforce.com)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、[WordPress](http://www.wordpress.com)と[Zendesk](https://www.zendesk.com/)が簡単に複数のサポートを追加します。

* 送信側での管理とサブスクライバーの適切なセットをイベント通知を送信する場合にもサブスクリプションを保管のサポートを提供します。 これにより、独自の一連のイベント サブスクライバーがサブスクライブして、処理が発生したときに通知ことができますを定義することができます。

2 つの部分は、シナリオに応じて分解でまたは組み合わせて使用できます。 のみ、; 受信側の部分のみを使用して、他のサービスから Webhook を受信する必要がある場合使用する他のユーザーが Webhook を公開する場合は、まさにそのを実行できます。

コードは、ASP.NET Web API 2 および ASP.NET MVC 5 を対象とされ、としては[GitHub での OSS](https://github.com/aspnet/WebHooks)します。

## <a name="webhooks-overview"></a>Webhook の概要

Webhook は、パターンが異なるサービス サービスからには、使用する方法は、基本的な考え方は同じです。 考えることができます Webhook 単純なパブリッシュ/サブスクライブ モデルとして、ユーザーが他の場所で発生したイベントにサブスクライブできます。 イベント通知は、イベント自体に関する情報を含む HTTP POST 要求として反映されます。

通常、HTTP POST 要求には、JSON オブジェクトまたは HTML 形式のデータをトリガーする WebHook を引き起こすイベントに関する情報を含む WebHook 送信者によって決定されますが含まれています。 WebHook POST 要求本文の例ではたとえば、 [GitHub](http://www.github.com/)のように、特定のリポジトリで開かれる新しい懸案事項の結果として。

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

確実に目的の送信者から、WebHook が実際にはを POST 要求が何らかの方法でセキュリティで保護し、受信側で確認すること。 たとえば、 [GitHub Webhook](https://developer.github.com/webhooks/)が含まれています、 *X-ハブ-署名*HTTP ヘッダーを心配する必要があるないため、受信側の実装によってチェックされている要求の本体のハッシュを使用します。

WebHook のフローでは、このような何か通常は。

* WebHook の送信者は、クライアントがサブスクライブできるイベントを公開します。 イベント監視可能なに対する変更を記述、システムでは、たとえば挿入、プロセスが完了したこと、またはその他新しいデータ項目が表示されています。

* WebHook レシーバーは、次の 4 つの WebHook を登録することによってサブスクライブします。

     1. HTTP POST 要求; の形式でのイベント通知の送信先の URI

     2. 一連のフィルターを WebHook を起動する; 特定のイベントを記述します。

     3. HTTP POST 要求の署名に使用される秘密キー

     4. これは、HTTP POST 要求に含まれる追加のデータです。 追加の HTTP ヘッダー フィールドまたはプロパティの HTTP POST 要求の本文に含めるなどできます。

* イベントの発生し、一致する WebHook の登録が検出された HTTP POST 要求が送信されます。 通常、HTTP POST 要求の生成、受信者が応答していない何らかの理由またはエラー応答の HTTP POST 要求の結果の場合、複数回が再試行されます。

## <a name="webhooks-processing-pipeline"></a>Webhook の処理パイプライン

受信した Webhook 用の Microsoft ASP.NET Webhook 処理パイプラインのようになります。

![ASP.NET Webhook 処理パイプライン](_static/WebHookReceivers.png)

2 つの主要概念についてここでは*レシーバー*と*ハンドラー*:

* *受信側*と WebHook の特定の送信者からの特定のフレーバーを処理するために確実に WebHook 要求が目的の送信者から実際にはセキュリティ チェックを適用するかどうかを担当します。

* *ハンドラー*は通常、ユーザー コードが特定の WebHook の処理を実行します。

次のノードでこれらの概念は詳細に説明します。
