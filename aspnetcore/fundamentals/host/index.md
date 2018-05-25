---
title: ASP.NET Core でのホスティング
author: guardrex
description: アプリの起動と有効期間の管理を担当する、ASP.NET Core Web ホストと .NET 汎用ホストについて説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: fundamentals/host/index
ms.openlocfilehash: 7ad059e39866f59040c12b7ac15e9fa3405a9aad
ms.sourcegitcommit: a66f38071e13685bbe59d48d22aa141ac702b432
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/17/2018
---
# <a name="host-in-aspnet-core"></a>ASP.NET Core でのホスティング

.NET アプリは*ホスト*を構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。 次の 2 つのホスト API を使用できます。

* [Web ホスト](xref:fundamentals/host/web-host) &ndash; Web アプリのホスティングに適しています。
* [汎用ホスト](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 以降)&ndash; Web 以外のアプリ (バックグラウンド タスクを実行するアプリなど) のホスティングに適しています。 将来のリリースでは、汎用ホストが Web アプリを含むすべての種類のアプリのホスティングに適するようになります。 Web ホストは最終的に汎用ホストに置き換えられます。

現時点で ASP.NET Core アプリをホスティングする場合は、[WebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.webhostbuilder) に基づいた [Web ホスト](xref:fundamentals/host/web-host)を使用する必要があります。
