---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: OWIN を使用して、ASP.NET Web API 2 をセルフホスト |Microsoft Docs
author: rick-anderson
description: このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。 Web Interface for .NET (OWIN) d を開く.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: 73757b50c15c6c65dbde4b61179b2d453673cfad
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389561"
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>OWIN を使用して、ASP.NET Web API 2 を自己ホスト
====================
によって[Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。
> 
> [.NET 用 Web インターフェイスを開き](http://owin.org)(OWIN) .NET web サーバーおよび web アプリケーション間の抽象化を定義します。 OWIN により、OWIN の IIS の外部の独自のプロセスで web アプリケーションを自己ホストするために最適ですが、サーバーから web アプリケーションを分離します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (また、Visual Studio 2012 で動作する)
> - Web API 2


> [!NOTE]
> 完全なソース コードを検索するにはこのチュートリアルで[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)します。


## <a name="create-a-console-application"></a>コンソール アプリケーションを作成します。

**ファイル** メニューのをクリックして**新規**、 をクリックし、**プロジェクト**します。 **インストールされたテンプレート**、Visual c# では、クリックして**Windows**  をクリックし、**コンソール アプリケーション**します。 プロジェクトに"OwinSelfhostSample"という名前にして**OK**します。

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API と OWIN パッケージを追加します。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

これは、WebAPI を OWIN 自己ホスト型のパッケージと必要なすべての OWIN パッケージでインストールされます。

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>用の Web API を構成する自己ホスト

ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。 クラスに `Startup` という名前を付けます。

![](use-owin-to-self-host-web-api/_static/image5.png)

このファイルの定型コードのすべて、次に置き換えます。

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Web API コント ローラーを追加します。

次に、Web API コント ローラー クラスを追加します。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加** / **クラス**新しいクラスを追加します。 クラスに `ValuesController` という名前を付けます。

このファイルの定型コードのすべて、次に置き換えます。

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>OWIN ホストを起動し、HttpClient を使用して要求を行う

次のようにすべての Program.cs ファイルの定型コードに置き換えます。

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>アプリケーションの実行

アプリケーションを実行するには、Visual Studio で F5 キーを押します。 出力の内容は次のようになります。

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>その他のリソース

[プロジェクト Katana の概要](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Azure Worker ロールにおける ASP.NET Web API をホストします。](host-aspnet-web-api-in-an-azure-worker-role.md)
