---
uid: webhooks/receiving/dependencies
title: ASP.NET Webhook レシーバー依存関係 |Microsoft Docs
author: rick-anderson
description: 受信側の依存関係と ASP.NET の Webhook の依存関係挿入します。
ms.author: aspnetcontent
ms.date: 01/17/2012
ms.assetid: 5125e483-c2bb-435b-8cd1-21d3499bfaaf
ms.openlocfilehash: 05dcfac121e7974fd83c5b3736616479574944a1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817838"
---
# <a name="aspnet-webhooks-receiver-dependencies"></a>ASP.NET Webhook レシーバー依存関係

Microsoft ASP.NET Webhook は依存関係の挿入を考慮して設計されています。 システムのほとんどの依存関係は、依存関係のインジェクション エンジンを使用して別の実装で置き換えられます。

参照してください[DependencyScopeExtensions](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Extensions/DependencyScopeExtensions.cs)レシーバー依存関係の一覧についてはします。 依存関係が登録されていない場合は、既定の実装が使用されます。 参照してください[ReceiverServices](https://github.com/aspnet/WebHooks/blob/master/src/Microsoft.AspNet.WebHooks.Receivers/Services/ReceiverServices.cs)に対して一連の既定の実装。
