---
title: ASP.NET Core SignalR でサポートされているプラットフォーム
author: tdykstra
description: ASP.NET Core SignalR サポートされているプラットフォームについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: tdykstra
ms.custom: mvc
ms.date: 10/31/2018
uid: signalr/supported-platforms
ms.openlocfilehash: 773f6c020dbb2982911e177b55855473c750d52a
ms.sourcegitcommit: fc2486ddbeb15ab4969168d99b3fe0fbe91e8661
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/01/2018
ms.locfileid: "50758181"
---
# <a name="aspnet-core-signalr-supported-platforms"></a>ASP.NET Core SignalR でサポートされているプラットフォーム

## <a name="server-system-requirements"></a>サーバーのシステム要件

ASP.NET core SignalR は ASP.NET Core をサポートする任意のサーバー プラットフォームをサポートします。

## <a name="javascript-client"></a>JavaScript クライアント

[JavaScript クライアント](https://www.npmjs.com/package/@aspnet/signalr)NodeJS 8 以降のバージョンと、次のブラウザーで実行されます。

| ブラウザー                         | Version |
| ------------------------------- | ------- |
| Microsoft Edge                  | 現在の |
| Mozilla Firefox                 | 現在の |
| Google Chrome;Android が含まれています | 現在の |
| Safari;iOS が含まれています            | 現在の |
| Microsoft Internet Explorer     | 11      |
 
## <a name="net-client"></a>.NET クライアント

[.NET クライアント](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)は ASP.NET Core でサポートされている任意のサーバー プラットフォームで実行されます。

サーバーは、IIS を実行する場合、Websocket トランスポートには、IIS 8.0 または Windows Server 2012 で高い以上が必要です。 他のトランスポートは、すべてのプラットフォームでサポートされます。

## <a name="java-client"></a>Java クライアント

[Java クライアント](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)Java 8 と以降のバージョンをサポートしています。

## <a name="unsupported-clients"></a>サポートされていないクライアント

次のクライアントは使用できますが、試験段階または非公式には。 現在サポートされていない、ならない可能性があります。

* [C++ クライアント](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Swift クライアント](https://github.com/moozzyk/SignalR-Client-Swift)
