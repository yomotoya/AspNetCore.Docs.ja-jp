---
title: ASP.NET Core SignalR でサポートされているプラットフォーム
author: bradygaster
description: ASP.NET Core SignalR サポートされているプラットフォームについて説明します。
monikerRange: '>= aspnetcore-2.1'
ms.author: bradyg
ms.custom: mvc
ms.date: 04/06/2019
uid: signalr/supported-platforms
ms.openlocfilehash: fefaaf97de3f1fabf8f3154bf5b4ccb37195ccff
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/08/2019
ms.locfileid: "59068223"
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

[.NET クライアント](https://www.nuget.org/packages/Microsoft.AspNetCore.SignalR/)は ASP.NET Core でサポートされている任意のプラットフォームで実行されます。 たとえば、 [Xamarin 開発者は、SignalR を使用できます](https://github.com/aspnet/Announcements/issues/305)8.4.0.1 Xamarin.Android を使用して Android アプリを構築するためと後で、Xamarin.iOS 11.14.0.4 を使用して iOS アプリ以降。

サーバーは、IIS を実行する場合、Websocket トランスポートには、IIS 8.0 または Windows Server 2012 以降、またはそれ以降が必要です。 他のトランスポートは、すべてのプラットフォームでサポートされます。

## <a name="java-client"></a>Java クライアント

[Java クライアント](https://search.maven.org/artifact/com.microsoft.aspnet/signalr)Java 8 と以降のバージョンをサポートしています。

## <a name="unsupported-clients"></a>サポートされていないクライアント

次のクライアントは使用できますが、試験段階または非公式には。 現在サポートされていない、ならない可能性があります。

* [C++ クライアント](https://github.com/aspnet/SignalR/tree/master/clients/cpp)

* [Swift クライアント](https://github.com/moozzyk/SignalR-Client-Swift)
