---
uid: webhooks/index
title: ASP.NET の Webhook の概要 |Microsoft ドキュメント
author: rick-anderson
description: ASP.NET の Webhook の概要です。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5e2843f0-f499-448f-a712-33d4e9858321
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: 52399c23cdf393a2f7f94661fd48098ced65948c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530051"
---
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="e2a4b-103">ASP.NET の Webhook の概要</span><span class="sxs-lookup"><span data-stu-id="e2a4b-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="e2a4b-104">Webhook は、Web Api および SaaS サービスを一緒に配線の単純なパブリッシュ/サブスクライブ モデルを提供する簡易 HTTP パターンです。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="e2a4b-105">イベント発生すると、サービスで HTTP POST 要求の形式で登録されたサブスクライバーに通知が送信されます。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="e2a4b-106">POST 要求には、これによりを適宜動作し、受信側のイベントに関する情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="e2a4b-107">ため、わかりやすくするため、Webhook は既にによって公開されているなどのサービスの数が多い[Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、し、さらに多くです。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="e2a4b-108">たとえば、WebHook がでファイルが変更されたことを指定することができます[Dropbox](http://dropbox.com/)、または、GitHub でコードの変更がコミットされてまたはで支払いが開始されて[PayPal](http://www.paypal.com/)、またはで作成されて、カード[Trello](http://www.trello.com/)です。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="e2a4b-109">表示される値は無限です。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-109">The possibilities are endless!</span></span>

<span data-ttu-id="e2a4b-110">Microsoft ASP.NET Webhook やすく送信し、受信の両方 Webhook、ASP.NET アプリケーションの一部として。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="e2a4b-111">受信側では、受信と処理の任意の数の WebHook プロバイダーからの Webhook の共通モデルを提供します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="e2a4b-112">Out of box のサポートに関して[Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Pusher](http://www.pusher.com)、 [Salesforce](http://www.salesforce.com)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、[WordPress](http://www.wordpress.com)と[Zendesk](https://www.zendesk.com/)が、簡単に複数のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="e2a4b-113">送信側には、管理およびイベント通知サブスクライバーの適切なセットを送信する場合と同様のサブスクリプションを保存するためのサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="e2a4b-114">これにより、独自のサブスクライバーがサブスクライブおよび処理が行われる場合に通知できるイベントのセットを定義できます。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="e2a4b-115">2 つの部分は、シナリオに応じて、分解または一緒に使用できます。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="e2a4b-116">受信側の部分のみを使用することができますし、その他のサービスから Webhook を受信するためだけの場合消費する他のユーザーのための Webhook を公開する場合は、のみを行うことができます。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="e2a4b-117">コードが ASP.NET Web API 2 と ASP.NET MVC 5 を対象し、は[github OSS](https://github.com/aspnet/WebHooks)です。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="e2a4b-118">Webhook の概要</span><span class="sxs-lookup"><span data-stu-id="e2a4b-118">WebHooks Overview</span></span>

<span data-ttu-id="e2a4b-119">Webhook は、サービス サービスからには使用する方法が異なりますが、基本的な考え方は同じことを意味するパターンです。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="e2a4b-120">のに Webhook の単純なパブリッシュ/サブスクライブ モデルとして、ユーザーが別の場所で発生しているイベントにサブスクライブできます。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="e2a4b-121">イベント通知は、イベント自体についての情報を含む HTTP POST 要求として反映されます。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="e2a4b-122">通常、HTTP POST 要求には、JSON オブジェクトまたは HTML フォームのデータをトリガーする WebHook を引き起こすイベントに関する情報など、WebHook 送信者によって決定が含まれています。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="e2a4b-123">WebHook の POST 要求本文の例ではたとえば、 [GitHub](http://www.github.com/)次のよう特定のリポジトリで開かれる新しい懸案事項の結果として。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="e2a4b-124">WebHook が目的の送信者から実際にあることを確認するにはするには、POST 要求は何らかの方法でセキュリティで保護し、その後、受信側で検証します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="e2a4b-125">たとえば、 [GitHub Webhook](https://developer.github.com/webhooks/)が含まれています、 *X ハブ署名*HTTP ヘッダー心配する必要はありませんので、受信者の実装によってチェックされている要求の本体のハッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="e2a4b-126">WebHook フローでは、次のような一般的には。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="e2a4b-127">WebHook の送信者は、クライアントがサブスクライブできるイベントを表示します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="e2a4b-128">イベント監視可能なに対する変更を記述、システムでは、たとえば新規データ項目が挿入された、プロセスが完了したこと、または、それ以外にされています。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="e2a4b-129">WebHook の受信者は、4 つの項目から成る WebHook を登録することによってサブスクライブします。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="e2a4b-130">HTTP POST 要求; の形式でのイベント通知の送信先の URI</span><span class="sxs-lookup"><span data-stu-id="e2a4b-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="e2a4b-131">一連のフィルターを WebHook を発生させる必要があります。 特定イベントの説明</span><span class="sxs-lookup"><span data-stu-id="e2a4b-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="e2a4b-132">HTTP POST 要求の署名に使用される秘密キー</span><span class="sxs-lookup"><span data-stu-id="e2a4b-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="e2a4b-133">これは、HTTP POST 要求に含まれる追加のデータです。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="e2a4b-134">追加の HTTP ヘッダー フィールドまたは HTTP POST 要求本文に含まれているプロパティの指定できます。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="e2a4b-135">イベントが発生した後は、一致する WebHook 登録が検出され、HTTP POST 要求を送信します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="e2a4b-136">通常は、HTTP POST 要求の生成では、受信者が応答していない何らかの理由またはエラー応答の HTTP POST 要求の結果の場合数回を再試行します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="e2a4b-137">Webhook 処理パイプライン</span><span class="sxs-lookup"><span data-stu-id="e2a4b-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="e2a4b-138">着信の Webhook の Microsoft ASP.NET Webhook 処理パイプラインは、このようになります。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET Webhook 処理パイプライン](_static/WebHookReceivers.png)

<span data-ttu-id="e2a4b-140">2 つの主要概念についてここでは、*レシーバー*と*ハンドラー*:</span><span class="sxs-lookup"><span data-stu-id="e2a4b-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="e2a4b-141">*レシーバー* WebHook の特定の差出人からの特定のフレーバーを処理するためとなるように、WebHook 要求実際に目的の送信者からのセキュリティ チェックを適用するために担当します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="e2a4b-142">*ハンドラー*は通常、ユーザー コードが実行されている特定の WebHook を処理します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="e2a4b-143">次のノードの詳細については、これらの概念を説明します。</span><span class="sxs-lookup"><span data-stu-id="e2a4b-143">In the following nodes these concepts are described in more details.</span></span>
