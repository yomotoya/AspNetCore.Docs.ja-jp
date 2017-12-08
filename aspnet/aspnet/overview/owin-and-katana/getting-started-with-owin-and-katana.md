---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: "OWIN と Katana の概要 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/27/2013
ms.topic: article
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 8922aada723da9b149ec111902fcd883c8241dfb
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="getting-started-with-owin-and-katana"></a>OWIN と Katana の概要
====================
によって[Mike Wasson](https://github.com/MikeWasson)

[.NET (OWIN) の Web インターフェイスを開く](http://owin.org/).NET web サーバーと web アプリケーション間で抽象型を定義します。 アプリケーションから web サーバーを分離することにより OWIN やすく .NET web 開発用のミドルウェアを作成します。 また、OWIN しやすくその他のホスト &#8212; web アプリケーションを移植するなど、自己ホスト型 Windows サービスまたは他のプロセスにします。

OWIN は、実装ではなく、コミュニティが所有する仕様です。 Katana プロジェクトは、Microsoft によって開発されたオープン ソース OWIN コンポーネントのセットです。 OWIN と Katana の両方の一般的な概要については、次を参照してください。[プロジェクト Katana の概要を、](an-overview-of-project-katana.md)です。 この記事ではすぐに作業を開始するコードです。

このチュートリアルでは使用[Visual Studio 2013 のリリース候補](https://go.microsoft.com/fwlink/?LinkId=306566)、Visual Studio 2012 を使用することもできます。 いくつかの手順は、Visual Studio 2012 では、下には注意してくださいで異なります。

## <a name="host-owin-in-iis"></a>IIS で OWIN のホスト

ここでは、IIS の OWIN おをホストします。 このオプションでは、柔軟性と IIS の完成度の高い機能セットと共に OWIN パイプラインの構成可能性を提供します。 OWIN アプリケーションはこのオプションを使用して、ASP.NET の要求パイプラインで実行されます。

最初に、新しい ASP.NET Web アプリケーション プロジェクトを作成します。 (Visual Studio 2012 では、空の ASP.NET Web アプリケーション プロジェクトの種類を使用します)。

![](getting-started-with-owin-and-katana/_static/image1.png)

**新しい ASP.NET プロジェクト**ダイアログで、選択、**空**テンプレート。

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>NuGet パッケージを追加します。

次に、必要な NuGet パッケージを追加します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>スタートアップ クラスを追加します。

次に、OWIN スタートアップ クラスを追加します。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、**追加**選択してから、**新しい項目の**します。 **新しい項目の追加**ダイアログで、 **Owin スタートアップ クラス**です。 スタートアップ クラスの構成の詳細については、次を参照してください。 [OWIN スタートアップ クラス検出](owin-startup-class-detection.md)です。

![](getting-started-with-owin-and-katana/_static/image4.png)

`Startup1.Configuration` メソッドに次のコードを追加します。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

このコードを受け取る関数として実装されている、OWIN パイプラインにミドルウェアの単純な部分を追加する、 **Microsoft.Owin.IOwinContext**インスタンス。 サーバーが HTTP 要求を受信すると、OWIN パイプラインにミドルウェアを呼び出されます。 ミドルウェアは、応答のコンテンツの種類を設定し、応答本文を書き込みます。

> [!NOTE]
> OWIN 起動クラス テンプレートは、Visual Studio 2013 で使用します。 Visual Studio 2012 を使用している場合は、名前付き、新しい空のクラスを追加だけ`Startup1`、次のコードに貼り付けます。


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>アプリケーションを実行する

F5 キーを押してデバッグを開始します。 Visual Studio はブラウザー ウィンドウを開いて`http://localhost:*port*/`です。 ページは、次のようになります。

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>コンソール アプリケーションで自己ホストの OWIN

カスタム プロセスにおける自己ホストをホストして IIS からこのアプリケーションに変換する簡単です。 IIS ホスティングでは、IIS としても動作 HTTP サーバーとプロセスをホストするサーバー。 、自己ホスト型で、アプリケーションが、プロセスを作成し、を使用して、 **HttpListener** HTTP サーバーとしてのクラスです。

Visual Studio で、新しいコンソール アプリケーションを作成します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`Install-Package Microsoft.Owin.SelfHost -Pre`

追加、`Startup1`このチュートリアルのパート 1 からクラスをプロジェクトにします。 このクラスを変更する必要はありません。

アプリケーションの実装`Main`メソッドを次のようにします。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

コンソール アプリケーションを実行すると、サーバー起動をリッスンしている`http://localhost:9000`です。 Web ブラウザーでは、このアドレスに移動する場合は、"Hello world"ページが表示されます。

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>OWIN Diagnostics を追加します。

Microsoft.Owin.Diagnostics パッケージには、未処理の例外をキャッチし、エラーの詳細を HTML ページを表示するミドルウェアが含まれています。 このページの機能とほぼ同様に呼び出される場合があります ASP.NET エラー ページ、"[死亡の黄色の画面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)"(YSOD)。 YSOD などには、実稼働モードで無効にすることをお勧めは開発中は、Katana エラー ページが役立ちます。

プロジェクトで診断パッケージをインストールするには、パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`install-package Microsoft.Owin.Diagnostics –Pre`

コードを変更、`Startup1.Configuration`メソッドを次のようにします。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

これで Visual Studio が例外で中断されないようにアプリケーションを実行、デバッグなし ctrl キーを押しながら f5 キーを使用します。 アプリケーションの動作は、同じ以前と同様に移動するまで`http://localhost/fail`、この時点で、アプリケーションが例外をスローします。 エラー ページ ミドルウェアは例外をキャッチし、エラーに関する情報を使用する HTML ページを表示します。 スタック、クエリ文字列、cookie、要求ヘッダーおよび OWIN 環境変数を表示するタブをクリックします。

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>次の手順

- [OWIN 起動クラス検出](owin-startup-class-detection.md)
- [OWIN を使用して ASP.NET Web API を自己ホスト](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [使用して OWIN セルフ ホスト SignalR](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
