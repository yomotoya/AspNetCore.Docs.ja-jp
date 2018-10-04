---
title: ASP.NET Core SignalR でサポートされているプラットフォーム
author: tdykstra
description: ASP.NET Core SignalR でサポートされるプラットフォーム
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 09/26/2018
uid: signalr/supported-platforms
ms.openlocfilehash: d6d74a55d35ddb34a6f66a171bfe3f343dd61b63
ms.sourcegitcommit: 7b4e3936feacb1a8fcea7802aab3e2ea9c8af5b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 10/04/2018
ms.locfileid: "48577627"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR でサポートされているプラットフォーム

## <a name="server-system-requirements"></a>サーバーのシステム要件

ASP.NET core SignalR は ASP.NET Core をサポートする任意のサーバー プラットフォームをサポートします。

## <a name="javascript-client"></a>JavaScript クライアント

[JavaScript クライアント](https://www.npmjs.com/package/@aspnet/signalr)NodeJS 8 以降のバージョンと、次のブラウザーで実行されます。

| ブラウザー | Version |
| ------- | ------- |
| Microsoft Edge | 現在の |
| Mozilla Firefox | 現在の |
| Google Chrome;Android が含まれています | 現在の |
| Safari;iOS が含まれています | 現在の |
| Microsoft Internet Explorer | 11 |
 
## <a name="net-client"></a>.NET クライアント

[.NET クライアント](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)は ASP.NET Core でサポートされている任意のサーバー プラットフォームで実行されます。

サーバーが IIS を実行すると、Windows Server 2012 またはそれ以降は Websocket トランスポートが IIS 8.0 以降必要です。 他のトランスポートは、すべてのプラットフォームでサポートされます。

## <a name="java-client"></a>Java クライアント

[Java クライアント](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)Java 8 と以降のバージョンをサポートしています。

## <a name="unsupported-clients"></a>サポートされていないクライアント

次のクライアントは使用できますが、試験段階または非公式には。 ここではサポートされていません、これまでサポートされていません。

* [C++ クライアント](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Swift クライアント](https://github.com/moozzyk/SignalR-Client-Swift)
