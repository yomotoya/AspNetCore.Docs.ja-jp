---
uid: signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
title: "ASP.NET SignalR ハブ API ガイド - JavaScript クライアント |Microsoft ドキュメント"
author: pfletcher
description: "このドキュメントでは、SignalR ブラウザーや Windows ストア (WinJS) applicat など、JavaScript クライアントで、バージョン 2 のハブ API を使用して紹介しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/28/2015
ms.topic: article
ms.assetid: a9fd4dc0-1b96-4443-82ca-932a5b4a8ea4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/guide-to-the-api/hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: 65e369a393a8c5d2d1bba11b5c71347df8f9c69d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="aspnet-signalr-hubs-api-guide---javascript-client"></a>ASP.NET SignalR ハブ API ガイド - JavaScript クライアント
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

> このドキュメントでは、SignalR ブラウザーや Windows ストア (WinJS) アプリケーションなど、JavaScript クライアントで、バージョン 2 のハブ API を使用してに紹介します。
> 
> SignalR ハブ API では、クライアントを接続するようにサーバーおよびサーバーのクライアントからリモート プロシージャ コール (Rpc) を作成することができます。 サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。 クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。 すべてのクライアントからサーバーへの組み込みの SignalR によって行われます。
> 
> SignalR では、永続的な接続と呼ばれる、下位の API も提供します。 SignalR、ハブ、および永続的な接続の概要については、次を参照してください。 [SignalR の概要](../getting-started/introduction-to-signalr.md)です。
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
> ## <a name="previous-versions-of-this-topic"></a>このトピックの以前のバージョン
> 
> SignalR の以前のバージョンについては、次を参照してください。[古いバージョンの SignalR](../older-versions/index.md)です。
> 
> ## <a name="questions-and-comments"></a>質問やコメント
> 
> このチュートリアルをリンクする方法と、ページの下部にあるコメントで改善新機能にフィードバックを送信してください。 チュートリアルに直接関連付けられていない質問がある場合を投稿、 [ASP.NET SignalR フォーラム](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)または[StackOverflow.com](http://stackoverflow.com/)です。


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [生成されたプロキシとそれが何をします。](#genproxy)

    - [生成されたプロキシを使用する場合](#cantusegenproxy)
- [クライアントのセットアップ](#clientsetup)

    - [動的に生成されたプロキシを参照する方法](#dynamicproxy)
    - [生成されたプロキシの SignalR の物理ファイルを作成する方法](#manualproxy)
- [接続を確立する方法](#establishconnection)

    - [$connection.hub は同じオブジェクトのその $.hubConnection() の作成。](#connequivalence)
    - [Start メソッドの非同期実行](#asyncstart)
- [ドメイン間の接続を確立する方法](#crossdomain)
- [接続を構成する方法](#configureconnection)

    - [クエリ文字列パラメーターを指定する方法](#querystring)
    - [転送方法を指定する方法](#transport)
- [ハブ クラスのプロキシを取得する方法](#getproxy)
- [サーバーが呼び出すことができるクライアント上のメソッドを定義する方法](#callclient)
- [クライアントからサーバーのメソッドを呼び出す方法](#callserver)
- [接続の有効期間イベントを処理する方法](#connectionlifetime)
- [エラーを処理する方法](#handleerrors)
- [クライアント側のログ記録を有効にする方法](#logging)

サーバーまたは .NET のクライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。

- [SignalR ハブ API ガイド - サーバー](hubs-api-guide-server.md)
- [SignalR ハブ API ガイド - .NET クライアント](hubs-api-guide-net-client.md)

SignalR の 2 サーバー コンポーネントを .NET 4.5 でできるは、(.NET 4.0 の SignalR 2 用の .NET クライアントがある) 場合だけです。

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>生成されたプロキシとそれが何をします。

SignalR で生成されるプロキシの有無は、SignalR サービスと通信するために、JavaScript クライアントをプログラムすることができます。 プロキシが動作するは、接続する場合、サーバーを呼び出すと、書き込みメソッドを使用して、サーバー上のメソッドを呼び出すコードの構文を簡略化します。

サーバー メソッドを呼び出すコードを記述するときに生成されたプロキシを使用すると、ローカル関数を実行した場合と同様の検索構文を使用する: 書き込めること`serverMethod(arg1, arg2)`の代わりに`invoke('serverMethod', arg1, arg2)`です。 生成されたプロキシ構文では、サーバーのメソッド名を入力し間違えた場合即時および判読クライアント側のエラーも可能です。 および server メソッドを呼び出すコードを記述するため、IntelliSense のサポートも取得できます、プロキシを定義するファイルを手動で作成する場合。

たとえば、サーバーで次の Hub クラスがあるとします。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

呼び出すように JavaScript コードなります次のコード例を表示する、`NewContosoChatMessage`メソッドをサーバーとの呼び出しを受信、`addContosoChatMessageToPage`サーバーからのメソッドです。

**生成されたプロキシと**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**生成されたプロキシなし**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>生成されたプロキシを使用する場合

サーバーを呼び出すクライアント メソッドの複数のイベント ハンドラーを登録する場合は、生成されたプロキシを使うことはできません。 それ以外の場合、生成されたプロキシを使用することもできますか、コーディングの基本設定に基づいていません。 これを使用しないようにする場合は、「signalr/ハブ」URL で参照することも、`script`クライアント コード内の要素。

<a id="clientsetup"></a>

## <a name="client-setup"></a>クライアントのセットアップ

JavaScript クライアントには、jQuery、および SignalR core の JavaScript ファイルへの参照が必要です。 JQuery のバージョンは、1.6.4 または 1.7.2、1.8.2、1.9.1 など、主要な以降のバージョンにする必要があります。 生成されたプロキシを使用する場合は、SignalR 生成されたプロキシの JavaScript ファイルへの参照も必要です。 次の例では、生成されたプロキシを使用する HTML ページに表示される、参照を示します。

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample4.html)]

この順序でこれらの参照を含める必要があります: jQuery 最初に、その後、SignalR core および SignalR プロキシ最後です。

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>動的に生成されたプロキシを参照する方法

前の例では、SignalR 生成されたプロキシへの参照は、物理ファイルが、動的に生成された JavaScript コードがします。 SignalR では、JavaScript コードを実行時にプロキシを作成し、「/signalr ハブ」URL への応答でクライアントに機能します。 サーバー上では、SignalR 接続に対して別の基本 URL を指定したかどうか、`MapSignalR`メソッドを動的に生成されるプロキシ ファイルの URL は、カスタムの URL に"/ハブ"が追加されます。

> [!NOTE]
> Windows 8 (Windows ストア) の JavaScript クライアントは、動的に生成されたものではなく、物理的なプロキシ ファイルを使用します。 詳細については、次を参照してください。 [SignalR の物理ファイルを作成する方法については、プロキシを生成](#manualproxy)このトピックで後述します。


ASP.NET MVC 4 または 5 の Razor ビューでは、チルダを使用して、アプリケーション ルートに、プロキシ ファイル参照を参照してください。

[!code-html[Main](hubs-api-guide-javascript-client/samples/sample5.html)]

SignalR の MVC 5 の使用に関する詳細については、次を参照してください。 [SignalR と MVC 5 の概要](../getting-started/tutorial-getting-started-with-signalr-and-mvc.md)です。

ASP.NET MVC 3 Razor ビューで使用して`Url.Content`プロキシ ファイル参照。

[!code-cshtml[Main](hubs-api-guide-javascript-client/samples/sample6.cshtml)]

ASP.NET Web フォーム アプリケーションで使用`ResolveClientUrl`プロキシの参照または (チルダ以降)、アプリケーション ルートの相対パスを使用して ScriptManager 経由で登録のため。

[!code-aspx[Main](hubs-api-guide-javascript-client/samples/sample7.aspx)]

一般的な規則としては、CSS や JavaScript ファイルに使用する「/signalr ハブ」URL を指定するため、同じメソッドを使用します。 URL を指定するには、チルダを使用せず、一部のシナリオで、アプリケーションが正しく動作 IIS Express を使用して Visual Studio でのテストが、完全な IIS に展開するときは、404 エラーで失敗します。 詳細については、次を参照してください。**ルート レベルのリソースへの参照の解決**で[ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/en-us/library/58wxa9w5.aspx) MSDN サイトです。

デバッグ モードで Visual Studio 2013 で web プロジェクトを実行すると、お使いのブラウザーとして Internet Explorer を使用する場合は、プロキシ ファイルを確認できます**ソリューション エクスプ ローラー** **スクリプト ドキュメント**のように、次の図。

![ソリューション エクスプ ローラーでの JavaScript 生成されるプロキシ ファイル](hubs-api-guide-javascript-client/_static/image1.png)

ファイルの内容を表示するをダブルクリック**ハブ**です。 Visual Studio 2012 または 2013 と Internet Explorer を使用しない場合、またはデバッグ モードになっていない場合は、「/signalR ハブ」URL を参照して、ファイルの内容を取得することもできます。 サイトが実行されている場合など`http://localhost:56699`に進み、`http://localhost:56699/SignalR/hubs`ブラウザーにします。

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>生成されたプロキシの SignalR の物理ファイルを作成する方法

動的に生成されたプロキシする代わりには、プロキシ コードを持つ物理ファイルを作成し、そのファイルを参照することができます。 そのためにキャッシュまたはバンドル化の動作を制御または server メソッドを呼び出すには、コーディングするときに IntelliSense を取得することができます。

プロキシ ファイルを作成するには、次の手順を実行します。

1. インストール、 [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet パッケージです。
2. コマンド プロンプトを開きを参照、*ツール*SignalR.exe ファイルを含むフォルダー。 ツール フォルダーは、次の場所には。

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.2.1.0\tools`
3. 次のコマンドを入力します。

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    パス、 *.dll*は、通常、 *bin*プロジェクト フォルダー内のフォルダーです。

    このコマンドは、という名前のファイルを作成*server.js*と同じフォルダーに*signalr.exe*です。
4. Put、 *server.js*ファイル、プロジェクト内の適切なフォルダーをアプリケーションの適切な名前を変更および「signalr/ハブ」参照の代わりにへの参照を追加します。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>接続を確立する方法

接続を確立することができます、前に、接続オブジェクトを作成、プロキシを作成し、サーバーから呼び出すことができるメソッドのイベント ハンドラーを登録する必要があります。 プロキシとイベント ハンドラーが設定されているときに呼び出すことによって、接続の確立、`start`メソッドです。

生成されたプロキシを使用している場合は、独自のコードで生成されたプロキシ コードは、接続オブジェクトを作成する必要はありません。

<a id="nogenconnection"></a>

**(生成されたプロキシ) との接続を確立します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**(なし、生成されたプロキシ) 接続を確立します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

既定値を使用するサンプル コード"/signalr"SignalR サービスへの接続への URL。 別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](hubs-api-guide-server.md#signalrurl)です。

既定では、ハブの場所は、現在のサーバーです。別のサーバーに接続する場合は、呼び出す前に URL を指定、`start`メソッドを次の例で示すようにします。

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample10.js)]

> [!NOTE]
> 呼び出しの前にイベント ハンドラーを登録する通常、`start`接続を確立する方法です。 接続の確立後にいくつかのイベント ハンドラーを登録すると、ことを行うことができますが、少なくとも 1 つの呼び出しの前に、イベント ハンドラーを登録する必要があります、`start`メソッドです。 この理由の 1 つがあります多くハブ アプリケーションをトリガーする必要はありません、`OnConnected`場合のみにこれらのいずれかを使用するすべてのハブのイベントです。 SignalR をトリガーするように指示新機能が、接続が確立されると、ハブのプロキシのクライアント メソッドの存在していれば、`OnConnected`イベント。 呼び出しの前に、イベント ハンドラーを登録しない場合、`start`メソッド、ことができます、ハブ、ハブのメソッドを呼び出す`OnConnected`メソッドを呼び出すしないし、サーバーからクライアントのメソッドは呼び出されません。


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$connection.hub は同じオブジェクトのその $.hubConnection() の作成。

わかるようにの例については、生成されたプロキシを使用すると、`$.connection.hub`接続オブジェクトを参照します。 これは、同じオブジェクトを呼び出して取得するを`$.hubConnection()`生成されたプロキシを使用していない場合。 生成されたプロキシ コードでは、次のステートメントを実行することによって自動的に接続を作成します。

![生成されたプロキシ ファイルの接続を作成します。](hubs-api-guide-javascript-client/_static/image3.png)

生成されたプロキシを使用しているときに操作を実行すると`$.connection.hub`行えること、接続オブジェクトで生成されたプロキシを使用していないときにします。

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Start メソッドの非同期実行

`start`メソッドが非同期的に実行します。 返します、 [jQuery 延期オブジェクト](http://api.jquery.com/category/deferred-object/)などのメソッドを呼び出すことで、コールバック関数を追加できることを意味する`pipe`、 `done`、および`fail`です。 接続が確立された後に実行するコードがあれば、server メソッドを呼び出しなどには、コールバック関数にそのコードを配置またはコールバック関数から呼び出します。 `.done`接続が確立されると、内にあるコードを後後に、コールバック メソッドは実行される、`OnConnected`サーバー上のイベント ハンドラー メソッドの実行が終了します。

後のコードの次の行として前の例の「接続されているようになりました」ステートメントを配置した場合は、`start`メソッドの呼び出し (ではなく、`.done`コールバック) では、`console.log`行は、接続が確立される前に、次に示すように、実行されます例:

![接続が確立された後に実行するコードを記述する方法を誤った](hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>ドメイン間の接続を確立する方法

通常、ブラウザーからページを読み込む場合`http://contoso.com`、SignalR 接続は、同じドメインに`http://contoso.com/signalr`です。 場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。 セキュリティ上の理由から、ドメイン間の接続は、既定で無効にします。

SignalR で 1.x では、クロス ドメイン要求が単一 EnableCrossDomain フラグによって制御されます。 このフラグは、両方の JSONP および CORS 要求を制御します。 すべての CORS のサポートをより柔軟に行う、SignalR のサーバー コンポーネントから削除されました (JavaScript クライアントも CORS 通常、ブラウザーがサポートすることが検出された場合) を使用し、新しい OWIN ミドルウェアをこれらのシナリオをサポートするために使用されました。

設定を明示的に有効にする必要があります JSONP が必要な場合、クライアントで (古いブラウザーでは、クロス ドメイン要求をサポート)、`EnableJSONP`上、`HubConfiguration`オブジェクトを`true`次のように、します。 CORS より安全であるために、既定では、JSONP が無効です。

**Microsoft.Owin.Cors をプロジェクトに追加する:**このライブラリをインストールするには、パッケージ マネージャー コンソールで、次のコマンドを実行します。

`Install-Package Microsoft.Owin.Cors`

このコマンドは、2.1.0 を追加、プロジェクトにパッケージのバージョン。

### <a name="calling-usecors"></a>通話 UseCors

 次のコード スニペットでは、SignalR 2 で、ドメイン間の接続を実装する方法を示します。 

**SignalR 2 でクロス ドメイン要求を実装します。**

次のコードでは、SignalR 2 プロジェクトで CORS または JSONP を有効にする方法を示します。 このコード サンプルでは使用`Map`と`RunSignalR`の代わりに`MapSignalR`CORS のサポートを必要とする SignalR 要求に対してのみ、CORS ミドルウェアが実行されるように、(で指定されたパスにすべてのトラフィックではなく`MapSignalR`)。マップは、アプリケーション全体に対してではなく、特定の URL プレフィックスを実行する必要があるその他のすべてのミドルウェアにも使用できます。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample11.cs)]

> [!NOTE] 
> 
> - 設定されていない`jQuery.support.cors`コードで true に設定します。
> 
>     ![JQuery.support.cors を true に設定されていません。](hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR では、CORS の使用を処理します。 設定`jQuery.support.cors`SignalR、ブラウザーは、CORS をサポートするいると仮定する原因になるので true JSONP を無効にします。
> - Localhost の URL に接続するときに Internet Explorer 10 は見なされません、ドメイン間の接続、アプリケーションの動作をローカルで IE 10 とサーバー上のドメイン間の接続を有効にしていない場合でも、します。
> - Internet Explorer 9 を使用したクロス ドメインの接続方法の詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)です。
> - Chrome で、ドメイン間の接続を使用する方法については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)です。
> - 既定値を使用するサンプル コード"/signalr"SignalR サービスへの接続への URL。 別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](hubs-api-guide-server.md#signalrurl)です。


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>接続を構成する方法

接続を確立する前にクエリ文字列パラメーターを指定したり、配送方法を指定できます。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>クエリ文字列パラメーターを指定する方法

クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。 次の例では、クライアント コードでクエリ文字列パラメーターを設定する方法を示します。

**(生成されたプロキシ) の start メソッドを呼び出す前にクエリ文字列の値を設定します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

**(なし、生成されたプロキシ) start メソッドを呼び出す前にクエリ文字列の値を設定します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample13.js?highlight=2)]

次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample14.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>転送方法を指定する方法

接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定するサーバーとネゴシエートします。 使用するトランスポートが既に把握している場合を呼び出すときに、トランスポート メソッドを指定することによってこのネゴシエーション プロセスをバイパスできます、`start`メソッドです。

**クライアント コードで生成されたプロキシ) トランスポート メソッドを指定します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample15.js?highlight=1)]

**(なし、生成されたプロキシ) トランスポート メソッドを指定するクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample16.js?highlight=2)]

代わりに、しようとする SignalR 順序で複数のトランスポート メソッドを指定できます。

**クライアント コードで生成されたプロキシ) カスタム トランスポート フォールバック スキームを指定します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**(なし、生成されたプロキシ) カスタム トランスポート フォールバック スキームを指定するクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

転送方法を指定するため、次の値を使用できます。

- "Websocket"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

次の例では、どのトランスポート メソッドは、接続で使用されていることを確認する方法を示します。

**(生成されたプロキシ) との接続で使用されるトランスポート メソッドを表示するクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample19.js?highlight=2)]

**(なし、生成されたプロキシ) 接続で使用するトランスポート メソッドを表示するクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample20.js?highlight=3)]

サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](hubs-api-guide-server.md#contextproperty)です。 トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)です。

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>ハブ クラスのプロキシを取得する方法

各接続オブジェクトを作成するには、1 つまたは複数の Hub クラスが含まれた SignalR サービスへの接続に関する情報がカプセル化します。 ハブ クラスを通信するためにオブジェクトを使用するプロキシ (生成されたプロキシを使用していない) 場合自分で作成またはこれが生成されます。

クライアントでは、プロキシの名前は、ハブ クラス名の camel 形式のバージョンです。 SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。

**サーバーの hub クラス**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample21.cs?highlight=1)]

**ハブの生成されたクライアント プロキシへの参照を取得します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample22.js?highlight=1)]

**(生成されたプロキシは) なしの Hub クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

使用してハブ クラスを装飾する場合、`HubName`属性は、ケースを変更することがなく正確な名前を使用します。

**HubName 属性を持つサーバーの hub クラス**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample24.cs?highlight=1)]

**ハブの生成されたクライアント プロキシへの参照を取得します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample25.js?highlight=1)]

**(生成されたプロキシは) なしの Hub クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>サーバーが呼び出すことができるクライアント上のメソッドを定義する方法

サーバーがハブから呼び出すことができるメソッドを定義するを使用してハブ プロキシにイベント ハンドラーを追加します。、 `client` 、生成されたプロキシ、または呼び出しのプロパティ、`on`メソッド、生成されたプロキシを使用していない場合。 複雑なオブジェクトのパラメーターを使用できます。

呼び出す前に、イベント ハンドラーを追加、`start`接続を確立する方法です。 (を呼び出した後にイベント ハンドラーを追加する場合、`start`メソッドの注を参照[接続を確立する方法](#establishconnection)この前のドキュメント、および生成されたプロキシを使用せず、メソッドを定義するために示す構文を使用します)。

メソッド名に一致する小文字が区別されません。 たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addContosoChatMessageToPage`、または`addcontosochatmessagetopage`クライアントにします。

**(生成されたプロキシ) を使用するクライアントでメソッドを定義します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample27.js?highlight=2)]

**別の (生成されたプロキシ) を使用するクライアントでメソッドを定義する方法**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample28.js?highlight=1-2)]

**生成されたプロキシなし (start メソッドを呼び出した後に追加するときに) クライアントでメソッドを定義します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample29.js?highlight=3)]

**クライアントのメソッドを呼び出すサーバー コード**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample30.cs?highlight=5)]

次の例には、メソッド パラメーターとして複雑なオブジェクトが含まれます。

**メソッドを (生成されたプロキシ) を使用して複雑なオブジェクトを受け取るクライアントで定義します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample31.js?highlight=2-3)]

**メソッドは、複雑なオブジェクト (せずに、生成されたプロキシ) は、クライアントで定義します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample32.js?highlight=3-4)]

**複雑なオブジェクトを定義するサーバー コード**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample33.cs?highlight=1)]

**複雑なオブジェクトを使用してクライアント メソッドを呼び出すサーバー コード**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample34.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>クライアントからサーバーのメソッドを呼び出す方法

クライアントからサーバー メソッドを呼び出すには使用、 `server` 、生成されたプロキシのプロパティまたは`invoke`生成されたプロキシを使用していない場合は、ハブ プロキシのメソッドです。 戻り値またはパラメーターには、複雑なオブジェクトを指定できます。

ハブのメソッド名の camel 形式でバージョンを渡します。 SignalR は、JavaScript コードが JavaScript の規則に従うことができるように、この変更を自動的に作成します。

次の例では、戻り値を持たない server メソッドを呼び出す方法および戻り値が含まれている server メソッドを呼び出す方法を示します。

**HubMethodName 属性を持たないサーバー メソッド**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample35.cs?highlight=3)]

**パラメーターに渡される複雑なオブジェクトを定義するサーバー コード**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample36.cs)]

**(生成されたプロキシ) を使用して server メソッドを呼び出すクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample37.js?highlight=1)]

**(なし、生成されたプロキシ) server メソッドを呼び出すクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample38.js?highlight=1)]

ハブを使用してメソッドを装飾する場合、`HubMethodName`属性、ケースを変更することがなく、その名前を使用します。

**サーバー メソッド**HubMethodName 属性を持つ

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample39.cs?highlight=3)]

**(生成されたプロキシ) を使用して server メソッドを呼び出すクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

**(なし、生成されたプロキシ) server メソッドを呼び出すクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample41.js?highlight=1)]

上記の例では、戻り値を持たない server メソッドを呼び出す方法を示します。 次の例では、戻り値を持つサーバー メソッドを呼び出す方法を示します。

**メソッドの戻り値を持つサーバー コード**

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample42.cs?highlight=3)]

**使用される Stock クラス、**戻り値

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample43.cs?highlight=1)]

**(生成されたプロキシ) を使用して server メソッドを呼び出すクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample44.js?highlight=2,4-5)]

**(なし、生成されたプロキシ) server メソッドを呼び出すクライアント コード**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample45.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>接続の有効期間イベントを処理する方法

SignalR では、有効期間イベントを処理することができますを次の接続を提供します。

- `starting`: すべてのデータは、接続経由で送信する前に発生します。
- `received`: 接続ですべてのデータが受信したときに発生します。 受信したデータを提供します。
- `connectionSlow`: クライアントが低速または頻繁に削除の接続を検出したときに発生します。
- `reconnecting`: 基になるトランスポートの再接続を開始するときに発生します。
- `reconnected`: 基になるトランスポートが再接続されたときに発生します。
- `stateChanged`: 接続の状態が変更されたときに発生します。 以前の状態と新しい状態 (接続、接続、再接続、または切断) を提供します。
- `disconnected`: 接続を切断したときに発生します。

たとえば、顕著な遅延を引き起こす可能性のある接続の問題がある場合に警告メッセージを表示する場合、処理、`connectionSlow`イベント。

**ハンドル (生成されたプロキシ) を持つ connectionSlow イベント**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample46.js?highlight=1)]

**イベントを処理します connectionSlow (、生成されたプロキシなし)**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample47.js?highlight=2)]

詳細については、次を参照してください。 [SignalR で接続の有効期間イベントの処理と理解](handling-connection-lifetime-events.md)です。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>エラーを処理する方法

SignalR JavaScript クライアントは、提供、`error`イベントのハンドラーを追加することができます。 Fail メソッドを使用して、サーバーのメソッドの呼び出しに起因するエラーのハンドラーを追加することができますも。

場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、エラーが発生した SignalR を返す例外オブジェクトには、エラーについての最小限の情報が含まれています。 たとえばへの呼び出し`newContosoChatMessage`失敗した場合、エラー オブジェクト内のエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを実稼働環境でクライアントには推奨されませんトラブルシューティングの目的では、サーバーで次のコードを使用します。

[!code-csharp[Main](hubs-api-guide-javascript-client/samples/sample48.cs?highlight=2)]

次の例では、エラー イベントのハンドラーを追加する方法を示します。

**(生成されたプロキシ) を使用して、エラー ハンドラーを追加します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample49.js?highlight=1)]

**(なし、生成されたプロキシ) エラー ハンドラーを追加します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample50.js?highlight=2)]

次の例では、メソッドの呼び出しからエラーを処理する方法を示します。

**ハンドル (生成されたプロキシ) のメソッドの呼び出しからのエラー**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample51.js?highlight=2)]

**(なし、生成されたプロキシ) のメソッドの呼び出しからエラーを処理します。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

メソッドの呼び出しに失敗した場合、`error`もイベントはためで自分のコード、`error`メソッド ハンドラーし、、`.fail`メソッド コールバックを実行します。

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>クライアント側のログ記録を有効にする方法

接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティを`start`接続を確立する方法です。

**(生成されたプロキシ) を使用してログ記録を有効にします。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample53.js?highlight=1)]

**(なし、生成されたプロキシ) のログ記録を有効にします。**

[!code-javascript[Main](hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

ログを表示するには、ブラウザーの developer tools を開きコンソール タブに移動します。示す詳細な手順とスクリーン ショットこれを行う方法を示すチュートリアルでは、次を参照してください。[サーバーは、ASP.NET Signalr - ログ記録を有効にするとブロードキャスト](../getting-started/tutorial-server-broadcast-with-signalr.md#enablelogging)です。
