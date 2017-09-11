---
title: "ホスティングと展開の概要 - ASP.NET Core"
author: tdykstra
description: "ホスティング環境を設定し、それらに ASP.NET Core アプリを展開する方法の概要。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 08/07/2017
ms.topic: article
ms.assetid: f0930c68-4d17-4748-adbf-801e17601eb6
ms.technology: aspnet
ms.prod: asp.net-core
uid: publishing/index
ms.openlocfilehash: d030b4f16727080488056c9cde48c31a14a166bf
ms.sourcegitcommit: bd05f7ea8f87ad076ef6e8b704698ebcba5ca80c
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/25/2017
---
# <a name="hosting-and-deployment-overview-for-aspnet-core-apps"></a>ASP.NET Core アプリのホスティングと展開の概要

ASP.NET Core アプリをホスティング環境に展開する場合に実行する主な手順を以下に示します。

* ホスティング サーバー上のフォルダーにアプリを発行します。
* 要求の着信時にアプリを開始し、クラッシュ後、またはサーバーの再起動後に再始動するプロセス マネージャーを設定します。
* 要求をアプリに転送するリバース プロキシを設定します。

## <a name="publish-to-a-folder"></a>フォルダーに発行する 

[dotnet publish](https://docs.microsoft.com/dotnet/articles/core/tools/dotnet-publish) CLI コマンドはアプリケーション コードをコンパイルし、アプリケーションを実行するために必要なファイルを *publish* フォルダーにコピーします。 Visual Studio から展開する場合は、ファイルが展開先にコピーされる前に `dotnet publish` ステップが自動的に行われます。

### <a name="folder-contents"></a>フォルダーの内容

*publish* フォルダーには、アプリケーションとその依存関係、および必要に応じて .NET ランタイムの *.exe* ファイルと *.dll* ファイルが含まれています。

.NET Core アプリは、*自己完結型*または*フレームワーク依存*として発行できます。 アプリが自己完結型の場合、.NET ランタイムを含む *.dll* ファイルが *publish* フォルダーに含まれています。  アプリがフレームワークに依存する場合、.NET ランタイムのファイルは含まれていません。これは、コンピューターにインストールされている .NET のバージョンへの参照がアプリに含まれていないためです。 既定の展開モデルはフレームワークに依存します。 詳細については、「[.NET Core アプリケーションの展開](https://docs.microsoft.com/dotnet/articles/core/deploying/index)」を参照してください。

*.exe* ファイルと *.dll* ファイルに加え、ASP.NET Core アプリの *publish* フォルダーには、通常、構成ファイル、静的資産、および MVC ビューが含まれています。  詳細については、[ディレクトリ構造](xref:hosting/directory-structure)に関するページを参照してください。

## <a name="set-up-a-process-manager"></a>プロセス マネージャーを設定する

ASP.NET Core アプリは、サーバーの起動時に開始し、クラッシュ後に再始動する必要があるコンソール アプリです。 開始と再始動を自動化するには、プロセス マネージャーが必要です。 ASP.NET Core の最も一般的なプロセス マネージャーは、Linux の場合は [Nginx](xref:publishing/linuxproduction) と [Apache](xref:publishing/apache-proxy)、Windows の場合は [IIS](xref:publishing/iis) と [Windows Service](xref:hosting/windows-service) です。

## <a name="set-up-a-reverse-proxy"></a>リバース プロキシを設定する

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

アプリで [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーを使用する場合は、リバース プロキシ サーバーとして、[Nginx](xref:publishing/linuxproduction)、[Apache](xref:publishing/apache-proxy)、または [IIS](xref:publishing/iis) を使用できます。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。 詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore2x#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

アプリを [Kestrel](xref:fundamentals/servers/kestrel) Web サーバーを使用して、インターネットに公開する場合は、リバース プロキシ サーバーとして、[Nginx](xref:publishing/linuxproduction)、[Apache](xref:publishing/apache-proxy)、または [IIS](xref:publishing/iis) を使用する必要があります。 リバース プロキシ サーバーはインターネットから HTTP 要求を受け取り、事前にいくつかの処理を行ってから Kestrel に転送します。 リバース プロキシを使用する主な理由はセキュリティです。 詳細については、「[When to use Kestrel with a reverse proxy](xref:fundamentals/servers/kestrel?tabs=aspnetcore1x#when-to-use-kestrel-with-a-reverse-proxy)」 (Kestrel とリバース プロキシを使用するタイミング) を参照してください。

---

## <a name="using-visual-studio-and-msbuild-to-automate-deployment"></a>Visual Studio と MSBuild を使用して展開を自動化する

多くの場合、展開には、`dotnet publish` からサーバーへの出力のコピー以外の追加の作業が必要になります。 たとえば、*publish* フォルダーに対するファイルの追加や除外が必要になります。 Visual Studio では Web 展開で MSBuild を使用します。この MSBuild は、展開時に他の多くの作業を行うためにカスタマイズすることができます。 詳細については、[Visual Studio の発行プロファイル](xref:publishing/web-publishing-vs)に関するページと、『[Using MSBuild and Team Foundation Build](http://msbuildbook.com/)』という書籍を参照してください。

[Web の発行機能](xref:tutorials/publish-to-azure-webapp-using-vs)または[組み込みの Git サポート](xref:publishing/azure-continuous-deployment)を使用して、Visual Studio から Azure App Service に直接展開することができます。 Visual Studio Team Services では、[Azure App Service への継続的な展開](https://www.visualstudio.com/en-us/docs/build/aspnet/core/quick-to-azure)がサポートされています。

## <a name="additional-resources"></a>その他の技術情報

ホスティング環境として Docker を使用する方法については、[Docker での ASP.NET Core アプリのホスティング](xref:publishing/docker)に関するページを参照してください。
