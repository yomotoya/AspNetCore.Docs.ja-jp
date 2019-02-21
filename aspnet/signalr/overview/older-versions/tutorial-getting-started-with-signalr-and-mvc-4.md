---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 'チュートリアル: SignalR の概要 1.x と MVC 4 |Microsoft Docs'
author: bradygaster
description: ASP.NET SignalR、ASP.NET MVC 4 を使用して、リアルタイムのチャット アプリケーションを構築します。
ms.author: bradyg
ms.date: 03/29/2013
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: dd55ca22004b7e3899f6a8789494c842b984787f
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837794"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>チュートリアル: SignalR の概要 1.x と MVC 4
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tim Teebken](https://github.com/timlt)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このチュートリアルでは、ASP.NET SignalR を使用して、リアルタイムのチャット アプリケーションを作成する方法を示します。 SignalR を MVC 4 アプリケーションに追加し、チャットを送信し、メッセージを表示するビューを作成します。


## <a name="overview"></a>概要

このチュートリアルでは、ASP.NET SignalR、ASP.NET MVC 4 とリアルタイムの web アプリケーションの開発について説明します。 チュートリアルと同じのチャット アプリケーションのコードを使用して、 [SignalR 作業開始のチュートリアル](tutorial-getting-started-with-signalr.md)が、インターネットのテンプレートに基づく、MVC 4 アプリケーションに追加する方法を示しています。

このトピックでは、次の SignalR 開発タスクを学習します。

- MVC 4 アプリケーション、SignalR ライブラリに追加します。
- クライアントにコンテンツをプッシュするハブ クラスを作成します。
- Web ページで、SignalR jQuery ライブラリを使用して、メッセージを送信し、ハブから更新プログラムを表示します。

次のスクリーン ショットは、ブラウザーで実行されている完成したチャット アプリケーションを示しています。

![チャット インスタンス](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

セクション:

- [プロジェクトを設定します。](#setup)
- [サンプルを実行します。](#run)
- [コードを確認します](#code)
- [次の手順](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>プロジェクトを設定します。

必要条件:

- Visual Studio 2010 SP1、Visual Studio 2012、または Visual Studio 2012 Express。 Visual Studio がないを参照してください。 [ASP.NET ダウンロード](https://www.asp.net/downloads)無料 Visual Studio 2012 Express 開発ツールを取得します。
- Visual Studio 2010 では、インストール[ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)します。

このセクションでは、ASP.NET MVC 4 アプリケーションを作成し、SignalR ライブラリの追加、チャット アプリケーションを作成する方法を示します。

1. 1. Visual Studio で ASP.NET MVC 4 アプリケーションを作成し、SignalRChat、という名前を付けます [ok] をクリックします。

        > [!NOTE]
        > VS 2010 では、次のように選択します。 **.NET Framework 4** Framework のバージョンのドロップダウン コントロール。 SignalR のコードは、.NET Framework version 4 および 4.5 で実行されます。

        ![Mvc の web を作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. インターネット アプリケーション テンプレートを選択して、オプションをオフに**単体テスト プロジェクトを作成**、[ok] をクリックします。

         ![Mvc のインターネット サイトを作成します。](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. 開く、**ツール > NuGet パッケージ マネージャー > パッケージ マネージャー コンソール**し、次のコマンドを実行します。 この手順では、一連のスクリプト ファイルと SignalR の機能を有効にするアセンブリ参照をプロジェクトに追加します。

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. **ソリューション エクスプ ローラー** Scripts フォルダーを展開します。 SignalR のスクリプト ライブラリがプロジェクトに追加されていることに注意してください。

         ![ライブラリの参照](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. **ソリューション エクスプ ローラー**、プロジェクトを右クリックし、選択**追加 |新しいフォルダー**、という名前の新しいフォルダーを追加および**Hubs**します。
      6. 右クリックし、 **Hubs**フォルダー、をクリックして**追加 |クラス**、新しい C# という名前のクラスを作成および**ChatHub.cs**します。 このクラスは、すべてのクライアントにメッセージを送信する SignalR サーバー ハブとして使用されます。

> [!NOTE]
> Visual Studio 2012 を使用して、インストールされている場合、 [ASP.NET and Web Tools 2012.2 update](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)、ハブ クラスを作成する新しい SignalR 項目テンプレートを使用することができます。 右クリックし、 **Hubs**フォルダー、をクリックして**追加 |新しい項目の**、 **SignalR ハブ クラス (v1)**、クラスの名前と**ChatHub.cs**します。


1. コードに置き換えます、 **ChatHub**クラスを次のコード。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. 開く、 **Global.asax** 、プロジェクトのファイルを開き、メソッドの呼び出しを追加`RouteTable.Routes.MapHubs();`内のコードの最初の行として、`Application_Start`メソッド。 このコードでは、SignalR ハブの既定のルートを登録し、他のルートを登録する前に呼び出す必要があります。 完成した`Application_Start`メソッドは次のようになります。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. 編集、`HomeController`クラス**Controllers/HomeController.cs**クラスに次のメソッドを追加します。 このメソッドが戻る、**チャット**後の手順で作成するビュー。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. 内で右クリック、`Chat`メソッドだけに作成されるとクリックして**ビューの追加**ビューの新しいファイルを作成します。
5. **ビューの追加**ダイアログ ボックスで、チェック ボックスが選択されていることを確認します**レイアウトまたはマスター ページを使用して**(他のチェック ボックスをオフ)、順にクリックします**追加**します。

    ![ビューを追加する](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. という名前の新しいビュー ファイルを編集**Chat.cshtml**します。 後に、 &lt;h2&gt;タグに、次を貼り付けます&lt;div&gt;セクションと`@section scripts`ページにコード ブロックです。 このスクリプトでは、チャット メッセージを送信し、サーバーからメッセージを表示するページが有効にします。 チャット ビューの完全なコードは、次のコード ブロックに表示されます。

    > [!IMPORTANT]
    > SignalR およびその他のスクリプト ライブラリを Visual Studio プロジェクトに追加するとパッケージ マネージャー可能性がありますが、このトピックで示されているバージョンよりも新しいスクリプトのバージョンをインストールします。 コード内のスクリプト参照がプロジェクトにインストールされているスクリプト ライブラリのバージョンと一致することを確認します。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **すべて保存**プロジェクト。

<a id="run"></a>

## <a name="run-the-sample"></a>サンプルを実行します。

1. F5 キーを押して、デバッグ モードで、プロジェクトを実行します。
2. ブラウザーのアドレスの行の追加 **/ホーム/チャット**プロジェクトの既定のページの URL にします。 ブラウザーのインスタンスとユーザー名のプロンプトで、チャット ページが読み込まれます。

    ![ユーザー名の入力](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. ユーザー名を入力します。
4. ブラウザーのアドレスの行から URL をコピーし、2 つのブラウザー インスタンスを開くために使用します。 各ブラウザー インスタンスでは、一意のユーザー名を入力します。
5. 各ブラウザー インスタンスでコメントを追加し、をクリックして**送信**します。 すべてのブラウザー インスタンスで、コメントが表示されます。

    > [!NOTE]
    > この簡単なチャット アプリケーションでは、サーバー上のディスカッション コンテキストは保持されません。 ハブは、現在のすべてのユーザーへのコメントをブロードキャストします。 後で、チャットに参加するユーザーは、参加する時点から追加されたメッセージに表示されます。
6. 次のスクリーン ショットは、ブラウザーで実行されているチャット アプリケーションを示しています。

    ![チャット ブラウザー](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. **ソリューション エクスプ ローラー**、検査、**スクリプト ドキュメント**実行中のアプリケーションのノード。 お使いのブラウザーとして Internet Explorer を使用している場合、このノードはデバッグ モードに表示されます。 という名前のスクリプト ファイルがある**hubs** SignalR ライブラリは、実行時に動的に生成します。 このファイルは、jQuery スクリプトとサーバー側コード間の通信を管理します。 Internet Explorer 以外のブラウザーを使用する場合は、動的もアクセスできる**hubs**ファイルを参照して直接、たとえば http://mywebsite/signalr/hubsします。

    ![生成されたハブのスクリプト](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>コードを確認します

SignalR チャット アプリケーションを 2 つの基本的な SignalR 開発タスクを示します。 サーバーで、主要な調整オブジェクトとしてハブを作成し、メッセージを送受信する SignalR jQuery ライブラリを使用します。

### <a name="signalr-hubs"></a>SignalR Hubs

コード サンプルでは、 **ChatHub**クラスから派生、 **Microsoft.AspNet.SignalR.Hub**クラス。 派生する、**ハブ**クラスは、SignalR アプリケーションを構築する便利な方法です。 ハブ クラスにパブリック メソッドを作成し、web ページに jQuery スクリプトから呼び出すことによってこれらのメソッドにアクセスできます。

クライアントが呼び出すチャットのコードで、 **ChatHub.Send**メソッドを新しいメッセージを送信します。 ハブに送信メッセージのすべてのクライアントを呼び出して**Clients.All.addNewMessageToPage**します。

**送信**メソッドがいくつかのハブの概念を示します。

- クライアントが呼び出すことができるように、ハブ上のパブリック メソッドを宣言します。
- 使用して、 **Microsoft.AspNet.SignalR.Hub.Clients**この hub に接続されているすべてのクライアントにアクセスするプロパティ。
- クライアントの jQuery 関数を呼び出す (など、`addNewMessageToPage`関数) クライアントを更新します。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR と jQuery

**Chat.cshtml**ビュー ファイルのコード サンプルでは、SignalR jQuery ライブラリを使用して、SignalR hub と通信する方法を示しています。 コードで基本的なタスクでは、自動生成されたプロキシをハブをクライアントにコンテンツをプッシュするサーバーを呼び出すことができる関数を宣言して、接続を開始、ハブにメッセージを送信への参照を作成します。

次のコードは、ハブのプロキシを宣言します。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> JQuery でサーバー クラスとそのメンバーへの参照がキャメル ケースです。 コード サンプルを参照して、C# **ChatHub**クラスとしての jquery **chatHub**します。 参照する場合、`ChatHub`クラス jquery では通常 pascal 形式で、C# の場合と大文字小文字の区別 ChatHub.cs クラス ファイルを編集します。 追加、`using`を参照するステートメント、`Microsoft.AspNet.SignalR.Hubs`名前空間。 追加し、`HubName`属性を`ChatHub`クラス、たとえば`[HubName("ChatHub")]`します。 参照を jQuery を最後に、更新、`ChatHub`クラス。


次のコードでは、スクリプトでコールバック関数を作成する方法を示します。 サーバー上のハブ クラスは、各クライアントにコンテンツの更新をプッシュするには、この関数を呼び出します。 省略可能な呼び出し、 `htmlEncode`  ページで、スクリプト インジェクションを防止する手段として表示する前に関数を HTML に方法が表示されますが、メッセージの内容をエンコードします。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

次のコードでは、ハブとの接続を開く方法を示します。 コードが接続を開始しのクリック イベントを処理する関数に渡します、**送信**チャット ページ ボタン。

> [!NOTE]
> このアプローチにより、イベント ハンドラーが実行される前に、接続が確立されていること。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>次の手順

SignalR がリアルタイムの web アプリケーションを構築するためのフレームワークについて説明しました。 いくつかの SignalR 開発タスクも学習しました。 SignalR を ASP.NET アプリケーションを追加する方法、ハブ クラスを作成する方法と、ハブからメッセージを送受信する方法。

高度な SignalR 開発の概念については、SignalR のソース コードおよびリソースは、次のサイトを参照してください。

- [SignalR プロジェクト](http://signalr.net)
- [SignalR Github とサンプル](https://github.com/SignalR/SignalR)
- [SignalR の Wiki](https://github.com/SignalR/SignalR/wiki)
