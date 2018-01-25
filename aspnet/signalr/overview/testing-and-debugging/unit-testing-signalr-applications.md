---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: "単体テストの SignalR アプリケーションを |Microsoft ドキュメント"
author: pfletcher
description: "この記事では、SignalR 2.0 の単体テスト機能を使用する方法について説明します。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: d767e1a9d27670387133e5a48a8f92f5bdd39d9e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="unit-testing-signalr-applications"></a>単体テストの SignalR アプリケーション
====================
によって[Patrick Fletcher](https://github.com/pfletcher)

> この記事では、SignalR 2 の単体テスト機能の使用について説明します。 
> 
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されているソフトウェア バージョン
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR バージョン 2
>   
> 
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>単体テストの SignalR アプリケーション

SignalR 2 で単体テスト機能を使用して SignalR アプリケーションの単体テストを作成することができます。 SignalR 2 が含まれています、 [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)インターフェイスで、テストのハブ メソッドをシミュレートするモック オブジェクトを作成するために使用できます。

このセクションで追加で作成したアプリケーションの単体テスト、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)を使用して[XUnit.net](https://github.com/xunit/xunit)と[Moq](https://github.com/Moq/moq4)です。

テストの制御に使用される XUnit.net作成に使用される Moq、[模擬](http://en.wikipedia.org/wiki/Mock_object)テスト用のオブジェクト。 必要な場合、他のモック フレームワークを使用できます。[NSubstitute](http://nsubstitute.github.io/)もをお勧めします。 このチュートリアルは、モック オブジェクトを 2 つの方法で設定する方法を示します: 最初を使用して、`dynamic`オブジェクト (.NET Framework 4 で導入)、および秒のインターフェイスを使用します。

### <a name="contents"></a>目次

このチュートリアルでは、次のセクションでは、含まれています。

- [動的を使用した単体テスト](#dynamic)
- [単体テストの種類](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>動的を使用した単体テスト

このセクションで作成したアプリケーションの単体テストを追加、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)動的オブジェクトを使用します。

1. インストール、 [XUnit ランナー拡張子](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)for Visual Studio 2013。
2. 完了するか、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)から完成したアプリケーションをダウンロードまたは[MSDN コード ギャラリー](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)です。
3. はじめにアプリケーションのダウンロード版を使用している場合は開きます**パッケージ マネージャー コンソール** をクリック**復元**SignalR パッケージをプロジェクトに追加します。

    ![パッケージを復元します。](unit-testing-signalr-applications/_static/image1.png)
4. 単体テストのソリューションにプロジェクトを追加します。 ソリューションを右クリックして**ソリューション エクスプ ローラー**選択**追加**、**新しいプロジェクト.**.下にある、 **c#**ノードで、選択、 **Windows**ノード。 選択**クラス ライブラリ**です。 新しいプロジェクトの名前**TestLibrary**  をクリック**OK**です。

    ![テスト ライブラリを作成します。](unit-testing-signalr-applications/_static/image2.png)
5. テスト ライブラリ プロジェクトで SignalRChat プロジェクトへの参照を追加します。 右クリックし、 **TestLibrary**プロジェクトし、選択**追加**、**参照しています.**.選択、**プロジェクト**ノードの下、**ソリューション**ノード、およびチェック**SignalRChat**です。 **[OK]**をクリックします。

    ![プロジェクト参照を追加します。](unit-testing-signalr-applications/_static/image3.png)
6. SignalR、Moq、XUnit パッケージを追加、 **TestLibrary**プロジェクト。 **Package Manager Console**、設定、**プロジェクトの既定の**ドロップダウンを**TestLibrary**です。 コンソール ウィンドウで、次のコマンドを実行します。

    - `Install-Package Microsoft.AspNet.SignalR`
    - `Install-Package Moq`
    - `Install-Package XUnit`

    ![パッケージをインストールします。](unit-testing-signalr-applications/_static/image4.png)
7. テスト ファイルを作成します。 右クリックし、 **TestLibrary**プロジェクトし、クリックして**追加しています.**、**クラス**です。 新しいクラスの名前を**Tests.cs**です。
8. Tests.cs の内容を次のコードに置き換えます。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    使用して、テスト用クライアントを作成、上記のコードで、`Mock`オブジェクトから、 [Moq](https://github.com/Moq/moq4)型のライブラリ、 [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 に割り当てる`dynamic`の種類パラメーターです。)`IHubCallerConnectionContext`インターフェイスは、プロキシ オブジェクトを使用するクライアントのメソッドを呼び出します。 `broadcastMessage`関数は呼び出すことができるように、次に、モック クライアントの定義、`ChatHub`クラスです。 テスト エンジンは、呼び出し、`Send`のメソッド、`ChatHub`を呼び出して、モック クラス`broadcastMessage`関数。
9. キーを押して、ソリューションをビルド**F6**です。
10. 単体テストを実行します。 Visual Studio で、次のように選択します。**テスト**、 **Windows**、**テスト エクスプ ローラー**です。 テスト エクスプ ローラー ウィンドウで右クリック**HubsAreMockableViaDynamic**選択**選択したテストの実行**です。

    ![Test Explorer](unit-testing-signalr-applications/_static/image5.png)
11. テスト エクスプ ローラー ウィンドウで、下のウィンドウをチェックして、テストが成功したことを確認します。 テストが成功した、ウィンドウが表示されます。

    ![テスト成功](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>単体テストの種類

このセクションで作成したアプリケーションのテストを追加するが、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)をテストするメソッドを含むインターフェイスを使用します。

1. 手順 1. ~ 7. を完了、[単体テストの動的な](#dynamic)上記のチュートリアルです。
2. Tests.cs の内容を次のコードに置き換えます。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    署名を定義するインターフェイスを作成、上記のコードで、`broadcastMessage`メソッドで、テスト エンジンがモック クライアントを作成します。 モック クライアントを使用してを作成し、`Mock`型のオブジェクト[IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 に割り当てる`dynamic`型パラメーターです)。`IHubCallerConnectionContext`インターフェイスは、プロキシ オブジェクトを使用するクライアントのメソッドを呼び出します。

    インスタンスを作成、テスト`ChatHub`、しのモック版を作成、`broadcastMessage`メソッドを呼び出すことによってさらに呼び出される、`Send`ハブのメソッドです。
3. キーを押して、ソリューションをビルド**F6**です。
4. 単体テストを実行します。 Visual Studio で、次のように選択します。**テスト**、 **Windows**、**テスト エクスプ ローラー**です。 テスト エクスプ ローラー ウィンドウで右クリック**HubsAreMockableViaDynamic**選択**選択したテストの実行**です。

    ![Test Explorer](unit-testing-signalr-applications/_static/image7.png)
5. テスト エクスプ ローラー ウィンドウで、下のウィンドウをチェックして、テストが成功したことを確認します。 テストが成功した、ウィンドウが表示されます。

    ![テスト成功](unit-testing-signalr-applications/_static/image8.png)
