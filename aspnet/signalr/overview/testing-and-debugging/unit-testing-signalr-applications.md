---
uid: signalr/overview/testing-and-debugging/unit-testing-signalr-applications
title: SignalR アプリケーションの単体テスト |Microsoft Docs
author: bradygaster
description: この記事では、SignalR 2.0 の単体テスト機能を使用する方法について説明します。
ms.author: bradyg
ms.date: 06/10/2014
ms.assetid: d1983524-e0d5-4ee6-9d87-1f552f7cb964
msc.legacyurl: /signalr/overview/testing-and-debugging/unit-testing-signalr-applications
msc.type: authoredcontent
ms.openlocfilehash: cb4eb25aeedfe31ac2606de9fe7d280eb95ce2e6
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836039"
---
<a name="unit-testing-signalr-applications"></a>単体テストの SignalR アプリケーション
====================
提供者: [Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> この記事では、SignalR 2 の単体テスト機能を使用して説明します。
>
> ## <a name="software-versions-used-in-this-topic"></a>このトピックで使用されるソフトウェアのバージョン
>
>
> - [Visual Studio 2013](https://my.visualstudio.com/Downloads?q=visual%20studio%202013)
> - .NET 4.5
> - SignalR 2 のバージョン
>
>
>
> ## <a name="questions-and-comments"></a>意見やご質問
>
> このチュートリアルの良い点に関するフィードバックや、ページ下部にあるコメントで改善できる点をお知らせください。 チュートリアルに直接関係のない質問がある場合は、[ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)にて投稿してください。


<a id="unit"></a>
## <a name="unit-testing-signalr-applications"></a>SignalR アプリケーションの単体テスト

SignalR 2 の単体テストの機能を使用すると、SignalR アプリケーションの単体テストを作成します。 SignalR 2 が含まれています、 [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx)インターフェイスで、テストのハブ メソッドをシミュレートするモック オブジェクトを作成するために使用できます。

このセクションで作成したアプリケーションの単体テストを追加します、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)を使用して[XUnit.net](https://github.com/xunit/xunit)と[Moq](https://github.com/Moq/moq4)します。

XUnit.net; テストの制御に使用します。Moq の作成に使用される、[モック](http://en.wikipedia.org/wiki/Mock_object)をテストするためのオブジェクト。 必要な場合、その他のモック作成フレームワークを使用できます。[NSubstitute](http://nsubstitute.github.io/)もをお勧めします。 このチュートリアルでは、2 つの方法でモック オブジェクトを設定する方法を示します。最初を使用して、`dynamic`オブジェクト (.NET Framework 4 で導入された、)、第 2、インターフェイスを使用します。

### <a name="contents"></a>目次

このチュートリアルには、次のセクションが含まれています。

- [動的を使用した単体テスト](#dynamic)
- [種類による単体テスト](#type)

<a id="dynamic"></a>
### <a name="unit-testing-with-dynamic"></a>動的を使用した単体テスト

このセクションで作成したアプリケーションの単体テストを追加します、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)動的オブジェクトを使用します。

1. インストール、 [XUnit ランナー拡張子](https://visualstudiogallery.msdn.microsoft.com/463c5987-f82b-46c8-a97e-b1cde42b9099)for Visual Studio 2013。
2. 完了するか、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)、完成したアプリケーションをダウンロードまたは[MSDN コード ギャラリー](https://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)します。
3. 作業の開始のアプリケーションのダウンロード版を使用している場合は開きます**パッケージ マネージャー コンソール**クリック**復元**SignalR パッケージをプロジェクトに追加します。

    ![パッケージを復元します。](unit-testing-signalr-applications/_static/image1.png)
4. 単体テストのソリューションにプロジェクトを追加します。 ソリューションを右クリックして**ソリューション エクスプ ローラー**選択**追加**、**新しいプロジェクト.**.で、 **c#** ノードを選択、 **Windows**ノード。 選択**クラス ライブラリ**します。 新しいプロジェクトの名前**TestLibrary**クリック**OK**。

    ![テスト ライブラリを作成します。](unit-testing-signalr-applications/_static/image2.png)
5. SignalRChat プロジェクトに、テスト ライブラリ プロジェクトでの参照を追加します。 右クリックし、 **TestLibrary**順に選択して**追加**、**参照.**.選択、**プロジェクト**ノードの下、**ソリューション**ノード、およびチェック**SignalRChat**します。 **[OK]** をクリックします。

    ![プロジェクト参照を追加します。](unit-testing-signalr-applications/_static/image3.png)
6. SignalR、Moq、および XUnit パッケージを追加、 **TestLibrary**プロジェクト。 **パッケージ マネージャー コンソール**、設定、**プロジェクトの既定の**ドロップダウンを使用して**TestLibrary**します。 コンソール ウィンドウで、次のコマンドを実行します。

   - `Install-Package Microsoft.AspNet.SignalR`
   - `Install-Package Moq`
   - `Install-Package XUnit`

     ![パッケージをインストールします。](unit-testing-signalr-applications/_static/image4.png)
7. テスト ファイルを作成します。 右クリックし、 **TestLibrary**プロジェクトし、クリックして**追加しています.**、**クラス**します。 新しいクラスの名前**Tests.cs**します。
8. Tests.cs の内容を次のコードに置き換えます。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample1.cs)]

    使用して、テスト用クライアントを作成、上記のコードで、`Mock`オブジェクトから、 [Moq](https://github.com/Moq/moq4)型のライブラリ、 [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 を割り当てて`dynamic`型パラメーター。)`IHubCallerConnectionContext`インターフェイスは、クライアントのメソッドを呼び出すが、プロキシ オブジェクト。 `broadcastMessage`呼び出すことができるように、モックのクライアントの関数が定義し、`ChatHub`クラス。 テスト エンジンを呼び出して、`Send`のメソッド、`ChatHub`クラスで、さらに、モックを呼び出す`broadcastMessage`関数。
9. キーを押して、ソリューションをビルド**F6**します。
10. 単体テストを実行します。 Visual Studio で、次のように選択します。**テスト**、 **Windows**、**テスト エクスプ ローラー**します。 テスト エクスプ ローラー ウィンドウで右クリック**HubsAreMockableViaDynamic**選択**選択したテストの実行**します。

    ![Test Explorer](unit-testing-signalr-applications/_static/image5.png)
11. テスト エクスプ ローラー ウィンドウで、下部のウィンドウをチェックして、テストが渡されることを確認します。 テストが成功した、ウィンドウが表示されます。

    ![テスト成功](unit-testing-signalr-applications/_static/image6.png)

<a id="type"></a>
### <a name="unit-testing-by-type"></a>種類による単体テスト

このセクションで作成したアプリケーションのテストを追加します、[チュートリアル入門](../getting-started/tutorial-getting-started-with-signalr.md)をテストするメソッドを含むインターフェイスを使用します。

1. 手順 1. ~ 7. を完了、[単体テストの動的な](#dynamic)上記のチュートリアル。
2. Tests.cs の内容を次のコードに置き換えます。

    [!code-csharp[Main](unit-testing-signalr-applications/samples/sample2.cs)]

    シグネチャを定義するインターフェイスを作成、上記のコードで、`broadcastMessage`メソッド、テスト エンジンがモックのクライアントを作成します。 モックのクライアントを使用して作成し、`Mock`型のオブジェクト、 [IHubCallerConnectionContext](https://msdn.microsoft.com/library/microsoft.aspnet.signalr.hubs.ihubcallerconnectioncontext(v=vs.118).aspx) (SignalR 2.1 を割り当てて`dynamic`型パラメーターです)。`IHubCallerConnectionContext`インターフェイスは、クライアントのメソッドを呼び出すが、プロキシ オブジェクト。

    インスタンスを作成、テスト`ChatHub`、し、モック版を作成、`broadcastMessage`メソッドを呼び出すことによってさらに呼び出され、`Send`ハブのメソッド。
3. キーを押して、ソリューションをビルド**F6**します。
4. 単体テストを実行します。 Visual Studio で、次のように選択します。**テスト**、 **Windows**、**テスト エクスプ ローラー**します。 テスト エクスプ ローラー ウィンドウで右クリック**HubsAreMockableViaDynamic**選択**選択したテストの実行**します。

    ![Test Explorer](unit-testing-signalr-applications/_static/image7.png)
5. テスト エクスプ ローラー ウィンドウで、下部のウィンドウをチェックして、テストが渡されることを確認します。 テストが成功した、ウィンドウが表示されます。

    ![テスト成功](unit-testing-signalr-applications/_static/image8.png)
