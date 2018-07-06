---
uid: aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
title: OWIN と Katana の概要 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 09/27/2013
ms.assetid: 6dae249f-5ac6-4f6e-bc49-13bcd5a54a70
msc.legacyurl: /aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana
msc.type: authoredcontent
ms.openlocfilehash: 58a3d234867821d5e23cce2f01e105dfab88ac33
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826667"
---
<a name="getting-started-with-owin-and-katana"></a>OWIN と Katana の概要
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[Web Interface for .NET (OWIN) を開く](http://owin.org/).NET web サーバーおよび web アプリケーション間の抽象化を定義します。 OWIN は、アプリケーションから web サーバーを分離することで、簡単に .NET web 開発のためのミドルウェアを作成します。 また、OWIN 簡単に他のホストに web アプリケーションを移植&#8212;などの Windows サービスまたはその他のプロセスで自己ホストします。

OWIN は、実装ではなく、コミュニティが所有している仕様です。 Katana プロジェクトには、Microsoft によって開発されたオープン ソース OWIN コンポーネントのセットです。 OWIN と Katana の両方の一般的な概要については、次を参照してください。[プロジェクト Katana の概要を、](an-overview-of-project-katana.md)します。 この記事でが、右をジャンプを開始するコードにします。

このチュートリアルでは[Visual Studio 2013 Release Candidate](https://go.microsoft.com/fwlink/?LinkId=306566)、Visual Studio 2012 を使用することもできます。 いくつかの手順は、Visual Studio 2012 では、下記のメモをここでは異なります。

## <a name="host-owin-in-iis"></a>OWIN を IIS でホストします。

このセクションでは、IIS で OWIN をホストされます。 このオプションは、IIS の完成度の高い機能セットと共に OWIN パイプラインの構成可能性と柔軟性を使用できます。 このオプションを使用して、OWIN アプリケーションは、ASP.NET の要求パイプラインで実行されます。

最初に、新しい ASP.NET Web アプリケーション プロジェクトを作成します。 (Visual Studio 2012 では、空の ASP.NET Web アプリケーション プロジェクトの種類を使用します)。

![](getting-started-with-owin-and-katana/_static/image1.png)

**新しい ASP.NET プロジェクト**ダイアログ ボックスで、**空**テンプレート。

![](getting-started-with-owin-and-katana/_static/image2.png)

### <a name="add-nuget-packages"></a>NuGet パッケージを追加します。

次に、必要な NuGet パッケージを追加します。 **ツール**メニューの **ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`install-package Microsoft.Owin.Host.SystemWeb –Pre`

![](getting-started-with-owin-and-katana/_static/image3.png)

### <a name="add-a-startup-class"></a>Startup クラスを追加します。

次に、OWIN startup クラスを追加します。 ソリューション エクスプ ローラーでプロジェクトを右クリックし、選択**追加**を選択し、**新しい項目の**します。 **新しい項目の追加**ダイアログ ボックスで、 **Owin Startup クラス**します。 スタートアップ クラスの構成の詳細については、次を参照してください。 [OWIN スタートアップ クラス検出](owin-startup-class-detection.md)します。

![](getting-started-with-owin-and-katana/_static/image4.png)

`Startup1.Configuration` メソッドに次のコードを追加します。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample1.cs?highlight=3)]

このコードを受け取る関数として実装されている、OWIN パイプラインにミドルウェアの簡単なピースを追加します、 **Microsoft.Owin.IOwinContext**インスタンス。 サーバーは、HTTP 要求を受信すると、OWIN パイプラインは、ミドルウェアを呼び出します。 ミドルウェアは、応答のコンテンツの種類を設定し、応答本文を書き込みます。

> [!NOTE]
> OWIN Startup クラス テンプレートは、Visual Studio 2013 で使用できます。 Visual Studio 2012 を使用している場合は、という名前の新しい空のクラスを追加だけ`Startup1`、し、次のコードを貼り付けます。


[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample2.cs)]

### <a name="run-the-application"></a>アプリケーションを実行する

F5 キーを押して、デバッグを開始します。 Visual Studio はブラウザー ウィンドウを開いて`http://localhost:*port*/`します。 ページは、次のようになります。

![](getting-started-with-owin-and-katana/_static/image5.png)

## <a name="self-host-owin-in-a-console-application"></a>コンソール アプリケーションで OWIN を自己ホストします。

カスタムのプロセスで自己ホストする IIS のホストからこのアプリケーションを変換する簡単です。 IIS ホスティングでは、IIS は、HTTP サーバーとは、サービスをホストするプロセスとして機能します。 、自己ホストによる、アプリケーションは、プロセスを作成し、使用、 **HttpListener** HTTP サーバーとしてのクラス。

Visual Studio で、新しいコンソール アプリケーションを作成します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`Install-Package Microsoft.Owin.SelfHost -Pre`

追加、`Startup1`このチュートリアルのパート 1 からクラスをプロジェクト。 このクラスを変更する必要はありません。

アプリケーションの実装`Main`メソッドとして、次のとおりです。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample3.cs)]

サーバーでは、リッスンを開始、コンソール アプリケーションを実行すると`http://localhost:9000`します。 Web ブラウザーでこのアドレスに移動する場合は、"Hello world"ページが表示されます。

![](getting-started-with-owin-and-katana/_static/image6.png)

## <a name="add-owin-diagnostics"></a>OWIN の診断を追加します。

Microsoft.Owin.Diagnostics パッケージには、未処理の例外をキャッチし、エラーの詳細に HTML ページを表示するミドルウェアが含まれています。 このページの機能は、ASP.NET エラー ページとも呼ばれるよく似た、"[死の黄色い画面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)"(YSOD)。 YSOD と同様には、Katana のエラー ページは、開発中は、便利ですが、運用モードで無効にすることをお勧めします。

プロジェクトで診断パッケージをインストールするには、パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`install-package Microsoft.Owin.Diagnostics –Pre`

コードを変更、`Startup1.Configuration`メソッドとして、次のとおりです。

[!code-csharp[Main](getting-started-with-owin-and-katana/samples/sample4.cs?highlight=4,9-12)]

今すぐ Visual Studio は、例外で中断しないように、デバッグを行わず、アプリケーションの実行に ctrl キーを押しながら f5 キーを使用します。 アプリケーションの動作と同じと同様に移動するまで`http://localhost/fail`、この時点では、アプリケーションは、例外をスローします。 エラー ページ ミドルウェアが例外をキャッチし、エラーに関する情報を HTML ページを表示します。 スタック、クエリ文字列、cookie、要求ヘッダー、および OWIN 環境変数を確認するには、タブをクリックすることができます。

![](getting-started-with-owin-and-katana/_static/image7.png)

## <a name="next-steps"></a>次の手順

- [OWIN スタートアップ クラス検出](owin-startup-class-detection.md)
- [OWIN を使用して、ASP.NET Web API を自己ホスト](../../../web-api/overview/hosting-aspnet-web-api/use-owin-to-self-host-web-api.md)
- [OWIN を使用して、SignalR セルフホスト](../../../signalr/overview/deployment/tutorial-signalr-self-host.md)
