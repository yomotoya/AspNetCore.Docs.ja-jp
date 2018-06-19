---
uid: webhooks/receiving/dependencies
title: ASP.NET の Webhook の受信者の依存関係 |Microsoft ドキュメント
author: rick-anderson
description: 受信者の依存関係と ASP.NET の Webhook の依存関係の挿入します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/17/2012
ms.topic: article
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.technology: ''
ms.prod: .net-framework
ms.openlocfilehash: f9726c746c8934594e26f2871f9b867c192374bb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26529911"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>ASP.NET の Webhook の受信者の依存関係

依存関係の挿入を念頭には、Microsoft ASP.NET の Webhook は設計されています。 システムの依存関係のほとんどは、依存関係の挿入エンジンを使用して別の実装で置き換えられます。

参照してください[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)受信機の依存関係の一覧についてはします。 依存関係が登録されていない場合は、既定の実装が使用されます。 参照してください[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)既定の実装の一覧についてはします。
