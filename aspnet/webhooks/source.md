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
# <a name="aspnet-webhooks-source-code-and-nuget-packages"></a>ASP.NET の Webhook のソース コードと NuGet パッケージ

Microsoft ASP.NET の Webhook はモジュールの Microsoft ASP.NET ファミリーの一部でありとしてホストされます、 [GitHub のオープン ソース プロジェクト](https://github.com/aspnet/WebHooks)です。 つまりは、投稿を受け入れていますを参照してください、[貢献ガイドライン](https://github.com/aspnet/Home/blob/master/CONTRIBUTING.md)プル要求を送信する前にします。

このオンライン ドキュメントをお読みになっているようになりましたがもとしてホストされる[GitHub のオープン ソース](http://docs.asp.net/en/latest/contribute/style-guide.html#style-guide)しも貢献を受け入れます。

## <a name="nuget-packages"></a>Nuget パッケージの管理

Microsoft ASP.NET Webhook もプレビューすることを確認するために Visual Studio でプレビュー フラグを選択する必要があることを意味する Nuget パッケージとして使用できます。

[Nuget パッケージの](https://nuget.org/packages?q=Microsoft.AspNet.WebHooks)3 つの部分に devided:

* [一般的な](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Common): 送信者と受信者の間で共有される共通のパッケージです。

* [送信者](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Custom): 一連のパッケージの他のユーザーに、独自の Webhook の送信をサポートします。 Webhook を送信するための機能がで詳しく説明されている[を送信する Webhook](sending/index.md)です。

* [レシーバー](https://www.nuget.org/packages?q=Microsoft.AspNet.WebHooks.Receivers): 一連のパッケージの他のユーザーからの Webhook の受信をサポートします。 Webhook を受信するための機能がで詳しく説明されている[受信 Webhook](receiving/index.md)です。
