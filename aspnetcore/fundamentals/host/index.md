---
title: ASP.NET Core の Web ホストおよび汎用ホスト
author: guardrex
description: アプリの起動と有効期間の管理を担当する、ASP.NET Core Web ホストと .NET 汎用ホストについて説明します。
ms.author: riande
ms.custom: mvc,seodec18
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 3e67d8338aa7ac1b1530d0498ee0126d36a8d72b
ms.sourcegitcommit: 49faca2644590fc081d86db46ea5e29edfc28b7b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/09/2018
ms.locfileid: "53121519"
---
# <a name="web-host-and-generic-host-in-aspnet-core"></a>ASP.NET Core の Web ホストおよび汎用ホスト

.NET アプリは*ホスト*を構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。 次の 2 つのホスト API を使用できます。

* [Web ホスト](xref:fundamentals/host/web-host) &ndash; Web アプリのホスティングに適しています。
* [汎用ホスト](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 以降)&ndash; Web 以外のアプリ (バックグラウンド タスクを実行するアプリなど) のホスティングに適しています。 将来のリリースでは、汎用ホストが Web アプリを含むすべての種類のアプリのホスティングに適するようになります。 Web ホストは最終的に汎用ホストに置き換えられます。

ASP.NET Core *Web アプリ*をホスティングする場合は、<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> に基づいた Web ホストを使用する必要があります。 *Web アプリ以外*をホスティングする場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder> に基づいた汎用ホストを使用する必要があります。

<xref:fundamentals/host/hosted-services>  
ASP.NET Core でホステッド サービスを使用するバックグラウンド タスクの実装方法について説明します。

<xref:fundamentals/configuration/platform-specific-configuration>  
<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 実装を使用して、参照されるアセンブリまたは参照されないアセンブリから ASP.NET Core アプリを拡張する方法について説明します。
