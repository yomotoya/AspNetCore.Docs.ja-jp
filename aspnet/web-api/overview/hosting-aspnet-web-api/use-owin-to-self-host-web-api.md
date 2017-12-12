---
uid: web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
title: "使用して OWIN セルフ ホスト ASP.NET Web API 2 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。 .NET (OWIN) d 用 Web インターフェイスを開く."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/09/2013
ms.topic: article
ms.assetid: a90a04ce-9d07-43ad-8250-8a92fb2bd3d5
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api
msc.type: authoredcontent
ms.openlocfilehash: fda0db8155c3303907331a690af35f619b589154
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="use-owin-to-self-host-aspnet-web-api-2"></a>OWIN を使用して ASP.NET Web API 2 を自己ホスト
====================
によって[Kanchan Mehrotra](https://twitter.com/kanchanmeh)

> このチュートリアルでは、OWIN を使用して Web API フレームワークを自己ホストするコンソール アプリケーションで ASP.NET Web API をホストする方法を示します。
> 
> [.NET 用 Web インターフェイスを開く](http://owin.org)(OWIN) .NET web サーバーと web アプリケーション間で抽象型を定義します。 OWIN には、web アプリケーション、サーバーは、OWIN のため、IIS の外部で独自のプロセス内の web アプリケーションを自己ホストするために最適ですから切り離します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されているソフトウェアのバージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads) (Visual Studio 2012 でも動作します)
> - Web API 2


> [!NOTE]
> このチュートリアルでの完全なソース コードを検索する[aspnet.codeplex.com](https://aspnet.codeplex.com/SourceControl/latest#Samples/WebApi/OwinSelfhostSample/ReadMe.txt)です。


## <a name="create-a-console-application"></a>コンソール アプリケーションを作成します。

**ファイル** メニューのをクリックして**新規**をクリックし、**プロジェクト**です。 **インストールされたテンプレート**、Visual c# をクリックして**Windows**  をクリックし、**コンソール アプリケーション**です。 プロジェクト"OwinSelfhostSample"の名前し、をクリックして**OK**です。

[![](use-owin-to-self-host-web-api/_static/image2.png)](use-owin-to-self-host-web-api/_static/image1.png)

## <a name="add-the-web-api-and-owin-packages"></a>Web API と OWIN パッケージを追加します。

**ツール** メニューのをクリックして**ライブラリ パッケージ マネージャー**をクリックし、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`Install-Package Microsoft.AspNet.WebApi.OwinSelfHost`

WebAPI OWIN selfhost パッケージと必要なすべての OWIN パッケージがインストールされます。

[![](use-owin-to-self-host-web-api/_static/image4.png)](use-owin-to-self-host-web-api/_static/image3.png)

## <a name="configure-web-api-for-self-host"></a>Web API を構成する自己ホスト

ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加** / **クラス**新しいクラスを追加します。 クラスに `Startup` という名前を付けます。

![](use-owin-to-self-host-web-api/_static/image5.png)

次のようにすべてのこのファイルに定型コードに置き換えます。

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample1.cs)]

## <a name="add-a-web-api-controller"></a>Web API コント ローラーを追加します。

次に、Web API コント ローラー クラスを追加します。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加** / **クラス**新しいクラスを追加します。 クラスに `ValuesController` という名前を付けます。

次のようにすべてのこのファイルに定型コードに置き換えます。

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample2.cs)]

## <a name="start-the-owin-host-and-make-a-request-using-httpclient"></a>OWIN ホストを起動して、HttpClient を使用して、要求

次のようにすべての Program.cs ファイルに定型コードに置き換えます。

[!code-csharp[Main](use-owin-to-self-host-web-api/samples/sample3.cs)]

## <a name="running-the-application"></a>アプリケーションの実行

アプリケーションを実行するには、Visual Studio で f5 キーを押します。 出力の内容は次のようになります。

[!code-console[Main](use-owin-to-self-host-web-api/samples/sample4.cmd)]

![](use-owin-to-self-host-web-api/_static/image6.png)

## <a name="additional-resources"></a>その他のリソース

[プロジェクトの Katana の概要](../../../aspnet/overview/owin-and-katana/an-overview-of-project-katana.md)

[Azure のワーカー ロールで ASP.NET Web API をホストします。](host-aspnet-web-api-in-an-azure-worker-role.md)
