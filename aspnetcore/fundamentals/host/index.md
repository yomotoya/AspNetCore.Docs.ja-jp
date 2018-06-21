---
title: ASP.NET Core でのホスティング
author: guardrex
description: アプリの起動と有効期間の管理を担当する、ASP.NET Core Web ホストと .NET 汎用ホストについて説明します。
ms.author: riande
ms.custom: mvc
ms.date: 05/16/2018
uid: fundamentals/host/index
ms.openlocfilehash: 365c679e789c07818c6eb007f40f6aef43b82c44
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/20/2018
ms.locfileid: "36276618"
---
# <a name="host-in-aspnet-core"></a>ASP.NET Core でのホスティング

.NET アプリは*ホスト*を構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。 次の 2 つのホスト API を使用できます。

* [Web ホスト](xref:fundamentals/host/web-host) &ndash; Web アプリのホスティングに適しています。
* [汎用ホスト](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 以降)&ndash; Web 以外のアプリ (バックグラウンド タスクを実行するアプリなど) のホスティングに適しています。 将来のリリースでは、汎用ホストが Web アプリを含むすべての種類のアプリのホスティングに適するようになります。 Web ホストは最終的に汎用ホストに置き換えられます。

ASP.NET Core *Web アプリ*をホスティングする場合は、[IWebHostBuilder](/dotnet/api/microsoft.aspnetcore.hosting.iwebhostbuilder) に基づいた Web ホストを使用する必要があります。 *Web アプリ以外*をホスティングする場合は、[HostBuilder](/dotnet/api/microsoft.extensions.hosting.hostbuilder) に基づいた汎用ホストを使用する必要があります。
