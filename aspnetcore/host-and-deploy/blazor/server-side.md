---
title: サーバー側 Blazor をホストおよび展開する
author: guardrex
description: ASP.NET Core を使用して Blazor サーバー側アプリをホストおよびデプロイする方法について説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/15/2019
uid: host-and-deploy/blazor/server-side
ms.openlocfilehash: 940020ee44d72d50395aad64bc924413c1bbecfb
ms.sourcegitcommit: 017b673b3c700d2976b77201d0ac30172e2abc87
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/16/2019
ms.locfileid: "59614714"
---
# <a name="host-and-deploy-blazor-server-side"></a>サーバー側 Blazor をホストおよび展開する

作成者: [Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com)、[Daniel Roth](https://github.com/danroth27)

## <a name="host-configuration-values"></a>ホストの構成値

[サーバー側のホスティング モデル](xref:blazor/hosting-models#server-side-hosting-model)を使用するサーバー側アプリでは、[汎用ホスト構成値](xref:fundamentals/host/generic-host#host-configuration)を受け入れることができます。

## <a name="deployment"></a>配置

[サーバー側のホスティング モデル](xref:blazor/hosting-models#server-side-hosting-model)では、Blazor はサーバー上で ASP.NET Core アプリ内から実行されます。 UI の更新、イベント処理、JavaScript の呼び出しは、[SignalR](xref:signalr/introduction) 接続経由で処理されます。

アプリは、発行された出力に ASP.NET Core アプリと共に含まれており、2 つのアプリが一緒に展開されます。 ASP.NET Core アプリをホストできる Web サーバーが必要です。 サーバー側の展開の場合、Visual Studio には **Blazor コンポーネント** プロジェクト テンプレートが含まれています ([dotnet new](/dotnet/core/tools/dotnet-new) コマンドを使用する場合は `razorcomponents` テンプレート)。

<!--

**INSERT: Concerns are the same as publishing an ASP.NET Core SignalR app**

**INSERT: Content on the Azure SignalR Service**

**INSERT: Manually turn on WebSockets support**

-->

ASP.NET Core アプリでのホストと展開の詳細については、「<xref:host-and-deploy/index>」を参照してください。

Azure App Service の展開については、「<xref:tutorials/publish-to-azure-webapp-using-vs>」を参照してください。
