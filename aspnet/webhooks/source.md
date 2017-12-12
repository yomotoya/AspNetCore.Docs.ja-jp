---
uid: webhooks/source
title: "ASP.NET の Webhook のソース コードと NuGet パッケージの管理 |Microsoft ドキュメント"
author: rick-anderson
description: "ASP.NET の Webhook のソース コードと NuGet パッケージへのリンク"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 91a62bfa-ea3a-41f9-a2e1-e90d2c8fc8ca
ms.technology: 
ms.prod: .net-framework
ms.openlocfilehash: ab5eaaa32a8f678b2aa4e2a0b30b674dbd6d6e9c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a><span data-ttu-id="fbf67-103">ASP.NET の Webhook のソース コードと NuGet パッケージ</span><span class="sxs-lookup"><span data-stu-id="fbf67-103">ASP.NET WebHooks source code and NuGet packages</span></span>

<span data-ttu-id="fbf67-104">Microsoft ASP.NET の Webhook はモジュールの Microsoft ASP.NET ファミリーの一部でありとしてホストされます、 [GitHub のオープン ソース プロジェクト](https://github.com/aspnet/WebHooks)です。</span><span class="sxs-lookup"><span data-stu-id="fbf67-104">Microsoft ASP.NET WebHooks is part of the Microsoft ASP.NET family of modules and is hosted as an [Open Source Project on GitHub](https://github.com/aspnet/WebHooks).</span></span> <span data-ttu-id="fbf67-105">つまりは、投稿を受け入れていますを参照してください、[貢献ガイドライン](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)プル要求を送信する前にします。</span><span class="sxs-lookup"><span data-stu-id="fbf67-105">This means that we accept contributions, but please look at the [Contribution Guidelines](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md) before submitting a pull request.</span></span>

<span data-ttu-id="fbf67-106">このオンライン ドキュメントをお読みになっているようになりましたがもとしてホストされる[GitHub のオープン ソース](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)しも貢献を受け入れます。</span><span class="sxs-lookup"><span data-stu-id="fbf67-106">This online documentation which you are reading now is also hosted as [Open Source on GitHub](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide) and also accepts contributions.</span></span>

## <a name="nuget-packages"></a><span data-ttu-id="fbf67-107">Nuget パッケージの管理</span><span class="sxs-lookup"><span data-stu-id="fbf67-107">Nuget Packages</span></span>

<span data-ttu-id="fbf67-108">Microsoft ASP.NET Webhook もプレビューすることを確認するために Visual Studio でプレビュー フラグを選択する必要があることを意味する Nuget パッケージとして使用できます。</span><span class="sxs-lookup"><span data-stu-id="fbf67-108">Microsoft ASP.NET WebHooks is also available as preview Nuget packages which means that you have to select the Preview flag in Visual Studio in order to see them.</span></span>

<span data-ttu-id="fbf67-109">[Nuget パッケージの](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)3 つの部分に devided:</span><span class="sxs-lookup"><span data-stu-id="fbf67-109">The [Nuget packages](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks) are devided into three parts:</span></span>

* <span data-ttu-id="fbf67-110">[一般的な](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): 送信者と受信者の間で共有される共通のパッケージです。</span><span class="sxs-lookup"><span data-stu-id="fbf67-110">[Common](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): A common package that is shared between senders and receivers.</span></span>

* <span data-ttu-id="fbf67-111">[送信者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): 一連のパッケージの他のユーザーに、独自の Webhook の送信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="fbf67-111">[Sender](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): A set of packages supporting sending your own WebHooks to others.</span></span> <span data-ttu-id="fbf67-112">Webhook を送信するための機能がで詳しく説明されている[を送信する Webhook](sending/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="fbf67-112">The functionality for sending WebHooks is described in more detail in [Sending WebHooks](sending/index.md).</span></span>

* <span data-ttu-id="fbf67-113">[レシーバー](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): 一連のパッケージの他のユーザーからの Webhook の受信をサポートします。</span><span class="sxs-lookup"><span data-stu-id="fbf67-113">[Receivers](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): A set of packages supporting receiving WebHooks from others.</span></span> <span data-ttu-id="fbf67-114">Webhook を受信するための機能がで詳しく説明されている[受信 Webhook](receiving/index.md)です。</span><span class="sxs-lookup"><span data-stu-id="fbf67-114">The functionality for receiving WebHooks is described in more detail in [Receiving WebHooks](receiving/index.md).</span></span>
