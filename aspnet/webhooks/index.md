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
# <a name="aspnet-webhooks-overview"></a><span data-ttu-id="aaefe-103">ASP.NET Webhook の概要</span><span class="sxs-lookup"><span data-stu-id="aaefe-103">ASP.NET WebHooks overview</span></span>

<span data-ttu-id="aaefe-104">Webhook は、Web Api と SaaS サービスをまとめて配線の単純なパブリッシュ/サブスクライブ モデルを提供する軽量な HTTP パターンです。</span><span class="sxs-lookup"><span data-stu-id="aaefe-104">WebHooks is a lightweight HTTP pattern providing a simple pub/sub model for wiring together Web APIs and SaaS services.</span></span> <span data-ttu-id="aaefe-105">サービスで、イベントが発生したときに通知が登録されているサブスクライバーに HTTP POST 要求の形式で送信されます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-105">When an event happens in a service, a notification is sent in the form of an HTTP POST request to registered subscribers.</span></span> <span data-ttu-id="aaefe-106">POST 要求には、それに従って動作する受信者を表すことができます、イベントに関する情報が含まれています。</span><span class="sxs-lookup"><span data-stu-id="aaefe-106">The POST request contains information about the event which makes it possible for the receiver to act accordingly.</span></span>

<span data-ttu-id="aaefe-107">Webhook を既になどのサービスの数が多いによって公開のためのわかりやすいように、 [Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、その他。</span><span class="sxs-lookup"><span data-stu-id="aaefe-107">Because of their simplicity, WebHooks are already exposed by a large number of services including [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/), and many more.</span></span> <span data-ttu-id="aaefe-108">たとえば、WebHook がのファイルが変更されたことを指定することができます[Dropbox](http://dropbox.com/)、または GitHub でコード変更がコミットされたまたはで支払いが開始された[PayPal](http://www.paypal.com/)、カードが作成されてまたは[Trello](http://www.trello.com/)します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-108">For example, a WebHook can indicate that a file has changed in [Dropbox](http://dropbox.com/), or a code change has been committed in GitHub, or a payment has been initiated in [PayPal](http://www.paypal.com/), or a card has been created in [Trello](http://www.trello.com/).</span></span> <span data-ttu-id="aaefe-109">可能性は無限です。</span><span class="sxs-lookup"><span data-stu-id="aaefe-109">The possibilities are endless!</span></span>

<span data-ttu-id="aaefe-110">Microsoft ASP.NET Webhook 簡単に送信および ASP.NET アプリケーションの一部として Webhook を受信します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-110">Microsoft ASP.NET WebHooks makes it easier to both send and receive WebHooks as part of your ASP.NET application:</span></span>

* <span data-ttu-id="aaefe-111">受信側では、受信および処理の任意の数の WebHook プロバイダーからの Webhook の一般的なモデルを提供します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-111">On the receiving side, it provides a common model for receiving and processing WebHooks from any number of WebHook providers.</span></span> <span data-ttu-id="aaefe-112">対応のボックスからの復帰[Dropbox](http://dropbox.com/)、 [GitHub](http://www.github.com/)、 [Bitbucket](https://bitbucket.org/)、 [MailChimp](http://www.mailchimp.com/)、 [PayPal](http://www.paypal.com/)、 [Pusher](http://www.pusher.com)、 [Salesforce](http://www.salesforce.com)、 [Slack](http://www.slack.com)、[ストライプ](http://www.stripe.com)、 [Trello](http://www.trello.com/)、[WordPress](http://www.wordpress.com)と[Zendesk](https://www.zendesk.com/)が簡単に複数のサポートを追加します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-112">It comes out of the box with support for [Dropbox](http://dropbox.com/), [GitHub](http://www.github.com/), [Bitbucket](https://bitbucket.org/), [MailChimp](http://www.mailchimp.com/), [PayPal](http://www.paypal.com/), [Pusher](http://www.pusher.com), [Salesforce](http://www.salesforce.com), [Slack](http://www.slack.com), [Stripe](http://www.stripe.com), [Trello](http://www.trello.com/),[WordPress](http://www.wordpress.com) and [Zendesk](https://www.zendesk.com/) but it is easy to add support for more.</span></span>

* <span data-ttu-id="aaefe-113">送信側での管理とサブスクライバーの適切なセットをイベント通知を送信する場合にもサブスクリプションを保管のサポートを提供します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-113">On the sending side it provides support for managing and storing subscriptions as well as for sending event notifications to the right set of subscribers.</span></span> <span data-ttu-id="aaefe-114">これにより、独自の一連のイベント サブスクライバーがサブスクライブして、処理が発生したときに通知ことができますを定義することができます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-114">This allows you to define your own set of events that subscribers can subscribe to and notify them when things happens.</span></span>

<span data-ttu-id="aaefe-115">2 つの部分は、シナリオに応じて分解でまたは組み合わせて使用できます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-115">The two parts can be used together or apart depending on your scenario.</span></span> <span data-ttu-id="aaefe-116">のみ、; 受信側の部分のみを使用して、他のサービスから Webhook を受信する必要がある場合使用する他のユーザーが Webhook を公開する場合は、まさにそのを実行できます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-116">If you only need to receive WebHooks from other services then you can use just the receiver part; if you only want to expose WebHooks for others to consume, then you can do just that.</span></span>

<span data-ttu-id="aaefe-117">コードは、ASP.NET Web API 2 および ASP.NET MVC 5 を対象とされ、としては[GitHub での OSS](https://github.com/aspnet/WebHooks)します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-117">The code targets ASP.NET Web API 2 and ASP.NET MVC 5 and is available as [OSS on GitHub](https://github.com/aspnet/WebHooks).</span></span>

## <a name="webhooks-overview"></a><span data-ttu-id="aaefe-118">Webhook の概要</span><span class="sxs-lookup"><span data-stu-id="aaefe-118">WebHooks Overview</span></span>

<span data-ttu-id="aaefe-119">Webhook は、パターンが異なるサービス サービスからには、使用する方法は、基本的な考え方は同じです。</span><span class="sxs-lookup"><span data-stu-id="aaefe-119">WebHooks is a pattern which means that it varies how it is used from service to service but the basic idea is the same.</span></span> <span data-ttu-id="aaefe-120">考えることができます Webhook 単純なパブリッシュ/サブスクライブ モデルとして、ユーザーが他の場所で発生したイベントにサブスクライブできます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-120">You can think of WebHooks as a simple pub/sub model where a user can subscribe to events happening elsewhere.</span></span> <span data-ttu-id="aaefe-121">イベント通知は、イベント自体に関する情報を含む HTTP POST 要求として反映されます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-121">The event notifications are propagated as HTTP POST requests containing information about the event itself.</span></span>

<span data-ttu-id="aaefe-122">通常、HTTP POST 要求には、JSON オブジェクトまたは HTML 形式のデータをトリガーする WebHook を引き起こすイベントに関する情報を含む WebHook 送信者によって決定されますが含まれています。</span><span class="sxs-lookup"><span data-stu-id="aaefe-122">Typically the HTTP POST request contains a JSON object or HTML form data determined by the WebHook sender including information about the event causing the WebHook to trigger.</span></span> <span data-ttu-id="aaefe-123">WebHook POST 要求本文の例ではたとえば、 [GitHub](http://www.github.com/)のように、特定のリポジトリで開かれる新しい懸案事項の結果として。</span><span class="sxs-lookup"><span data-stu-id="aaefe-123">For example, an example of a WebHook POST request body from [GitHub](http://www.github.com/) looks like this as a result of a new issue being opened in a particular repository:</span></span>

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

<span data-ttu-id="aaefe-124">確実に目的の送信者から、WebHook が実際にはを POST 要求が何らかの方法でセキュリティで保護し、受信側で確認すること。</span><span class="sxs-lookup"><span data-stu-id="aaefe-124">To ensure that the WebHook is indeed from the intended sender, the POST request is secured in some way and then verified by the receiver.</span></span> <span data-ttu-id="aaefe-125">たとえば、 [GitHub Webhook](https://developer.github.com/webhooks/)が含まれています、 *X-ハブ-署名*HTTP ヘッダーを心配する必要があるないため、受信側の実装によってチェックされている要求の本体のハッシュを使用します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-125">For example, [GitHub WebHooks](https://developer.github.com/webhooks/) includes an *X-Hub-Signature* HTTP header with a hash of the request body which is checked by the receiver implementation so you don't have to worry about it.</span></span>

<span data-ttu-id="aaefe-126">WebHook のフローでは、このような何か通常は。</span><span class="sxs-lookup"><span data-stu-id="aaefe-126">The WebHook flow generally goes something like this:</span></span>

* <span data-ttu-id="aaefe-127">WebHook の送信者は、クライアントがサブスクライブできるイベントを公開します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-127">The WebHook sender exposes events that a client can subscribe to.</span></span> <span data-ttu-id="aaefe-128">イベント監視可能なに対する変更を記述、システムでは、たとえば挿入、プロセスが完了したこと、またはその他新しいデータ項目が表示されています。</span><span class="sxs-lookup"><span data-stu-id="aaefe-128">The events describe observable changes to the system, for example that a new data item has been inserted, that a process has completed, or something else.</span></span>

* <span data-ttu-id="aaefe-129">WebHook レシーバーは、次の 4 つの WebHook を登録することによってサブスクライブします。</span><span class="sxs-lookup"><span data-stu-id="aaefe-129">The WebHook receiver subscribes by registering a WebHook consisting of four things:</span></span>

     1. <span data-ttu-id="aaefe-130">HTTP POST 要求; の形式でのイベント通知の送信先の URI</span><span class="sxs-lookup"><span data-stu-id="aaefe-130">A URI for where the event notification should be posted in the form of an HTTP POST request;</span></span>

     2. <span data-ttu-id="aaefe-131">一連のフィルターを WebHook を起動する; 特定のイベントを記述します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-131">A set of filters describing the particular events for which the WebHook should be fired;</span></span>

     3. <span data-ttu-id="aaefe-132">HTTP POST 要求の署名に使用される秘密キー</span><span class="sxs-lookup"><span data-stu-id="aaefe-132">A secret key which is used to sign the HTTP POST request;</span></span>

     4. <span data-ttu-id="aaefe-133">これは、HTTP POST 要求に含まれる追加のデータです。</span><span class="sxs-lookup"><span data-stu-id="aaefe-133">Additional data which is to be included in the HTTP POST request.</span></span> <span data-ttu-id="aaefe-134">追加の HTTP ヘッダー フィールドまたはプロパティの HTTP POST 要求の本文に含めるなどできます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-134">This can for example be additional HTTP header fields or properties included in the HTTP POST request body.</span></span>

* <span data-ttu-id="aaefe-135">イベントの発生し、一致する WebHook の登録が検出された HTTP POST 要求が送信されます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-135">Once an event happens, the matching WebHook registrations are found and HTTP POST requests are submitted.</span></span> <span data-ttu-id="aaefe-136">通常、HTTP POST 要求の生成、受信者が応答していない何らかの理由またはエラー応答の HTTP POST 要求の結果の場合、複数回が再試行されます。</span><span class="sxs-lookup"><span data-stu-id="aaefe-136">Typically, the generation of the HTTP POST requests are retried several times if for some reason the recipient is not responding or the HTTP POST request results in an error response.</span></span>

## <a name="webhooks-processing-pipeline"></a><span data-ttu-id="aaefe-137">Webhook の処理パイプライン</span><span class="sxs-lookup"><span data-stu-id="aaefe-137">WebHooks Processing Pipeline</span></span>

<span data-ttu-id="aaefe-138">受信した Webhook 用の Microsoft ASP.NET Webhook 処理パイプラインのようになります。</span><span class="sxs-lookup"><span data-stu-id="aaefe-138">The Microsoft ASP.NET WebHooks processing pipeline for incoming WebHooks looks like this:</span></span>

![ASP.NET Webhook 処理パイプライン](_static/WebHookReceivers.png)

<span data-ttu-id="aaefe-140">2 つの主要概念についてここでは*レシーバー*と*ハンドラー*:</span><span class="sxs-lookup"><span data-stu-id="aaefe-140">The two key concepts here are *Receivers* and *Handlers*:</span></span>

* <span data-ttu-id="aaefe-141">*受信側*と WebHook の特定の送信者からの特定のフレーバーを処理するために確実に WebHook 要求が目的の送信者から実際にはセキュリティ チェックを適用するかどうかを担当します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-141">*Receivers* are responsible for handling the particular flavor of WebHook from a given sender and for enforcing security checks to ensure that the WebHook request indeed is from the intended sender.</span></span>

* <span data-ttu-id="aaefe-142">*ハンドラー*は通常、ユーザー コードが特定の WebHook の処理を実行します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-142">*Handlers* are typically where user code runs processing the particular WebHook.</span></span>

<span data-ttu-id="aaefe-143">次のノードでこれらの概念は詳細に説明します。</span><span class="sxs-lookup"><span data-stu-id="aaefe-143">In the following nodes these concepts are described in more details.</span></span>
