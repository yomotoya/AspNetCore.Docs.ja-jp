---
title: ASP.NET Core でのホスティング
author: guardrex
description: アプリの起動と有効期間の管理を担当する、ASP.NET Core Web ホストと .NET 汎用ホストについて説明します。
ms.author: riande
ms.custom: mvc
ms.date: 08/28/2018
uid: fundamentals/host/index
ms.openlocfilehash: 9927722b5080beb94e5628d9e7b54e6d50a5bff8
ms.sourcegitcommit: a669c4e3f42e387e214a354ac4143555602e6f66
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/31/2018
ms.locfileid: "43336051"
---
# <a name="host-in-aspnet-core"></a>ASP.NET Core でのホスティング

.NET アプリは*ホスト*を構成して起動します。 ホストはアプリの起動と有効期間の管理を担当します。 次の 2 つのホスト API を使用できます。

* [Web ホスト](xref:fundamentals/host/web-host) &ndash; Web アプリのホスティングに適しています。
* [汎用ホスト](xref:fundamentals/host/generic-host) (ASP.NET Core 2.1 以降)&ndash; Web 以外のアプリ (バックグラウンド タスクを実行するアプリなど) のホスティングに適しています。 将来のリリースでは、汎用ホストが Web アプリを含むすべての種類のアプリのホスティングに適するようになります。 Web ホストは最終的に汎用ホストに置き換えられます。

ASP.NET Core *Web アプリ*をホスティングする場合は、<xref:Microsoft.AspNetCore.Hosting.IWebHostBuilder> に基づいた Web ホストを使用する必要があります。 *Web アプリ以外*をホスティングする場合は、<xref:Microsoft.Extensions.Hosting.HostBuilder> に基づいた汎用ホストを使用する必要があります。

<xref:fundamentals/host/hosted-services>  
ASP.NET Core でホステッド サービスを使用するバックグラウンド タスクの実装方法について説明します。

<xref:fundamentals/configuration/platform-specific-configuration>  
<xref:Microsoft.AspNetCore.Hosting.IHostingStartup> 実装を使用して、参照されるアセンブリまたは参照されないアセンブリから ASP.NET Core アプリを拡張する方法について説明します。
