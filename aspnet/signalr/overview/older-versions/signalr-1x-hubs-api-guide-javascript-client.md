---
uid: signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
title: SignalR 1.x ハブ API ガイド - JavaScript クライアント |Microsoft Docs
author: bradygaster
description: このドキュメントでは、ブラウザーや Windows ストア (WinJS) アプリなどの JavaScript クライアントではバージョン 1.1 SignalR ハブ API を使用するように紹介しています.
ms.author: bradyg
ms.date: 04/17/2013
ms.assetid: dcd4593b-1118-418a-af71-d12ff33fb36d
msc.legacyurl: /signalr/overview/older-versions/signalr-1x-hubs-api-guide-javascript-client
msc.type: authoredcontent
ms.openlocfilehash: eb40648ca06adcceaa613ba86abfcf7459369c7e
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836754"
---
<a name="signalr-1x-hubs-api-guide---javascript-client"></a>SignalR 1.x ハブ API ガイド - JavaScript クライアント
====================
によって[Patrick Fletcher](https://github.com/pfletcher)、 [Tom Dykstra](https://github.com/tdykstra)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> このドキュメントでは、ブラウザーや Windows ストア (WinJS) アプリケーションなどの JavaScript クライアントではバージョン 1.1 SignalR ハブ API を使用するように紹介します。
> 
> SignalR ハブの API では、サーバーからに接続されているクライアントとサーバーのクライアントからのリモート プロシージャ コール (Rpc) を作成することができます。 サーバー コードで、クライアントから呼び出すことができるメソッドを定義して、クライアント上で実行されるメソッドを呼び出します。 クライアント コードで、サーバーから呼び出すことができるメソッドを定義して、サーバー上で実行されるメソッドを呼び出します。 SignalR は、のすべてのクライアントとサーバーが処理されます。
> 
> SignalR では、永続的な接続と呼ばれる下位レベル API も提供します。 概要については、SignalR、ハブ、および永続的な接続は、または完全な SignalR アプリケーションを構築する方法を示すチュートリアルについてを参照してください。 [SignalR - Getting Started](../getting-started/index.md)します。


## <a name="overview"></a>概要

このドキュメントは、次のトピックに分かれています。

- [生成されたプロキシとは何をします。](#genproxy)

    - [生成されたプロキシを使用する場合](#cantusegenproxy)
- [クライアントのセットアップ](#clientsetup)

    - [動的に生成されたプロキシを参照する方法](#dynamicproxy)
    - [生成されたプロキシの SignalR の物理ファイルを作成する方法](#manualproxy)
- [接続を確立する方法](#establishconnection)

    - [$connection.hub が同じオブジェクトのその $.hubConnection() を作成します。](#connequivalence)
    - [Start メソッドの非同期実行](#asyncstart)
- [ドメイン間の接続を確立する方法](#crossdomain)
- [接続を構成する方法](#configureconnection)

    - [クエリ文字列パラメーターを指定する方法](#querystring)
    - [トランスポート メソッドを指定する方法](#transport)
- [ハブ クラスのプロキシを取得する方法](#getproxy)
- [サーバーが呼び出すことができるクライアントでメソッドを定義する方法](#callclient)
- [クライアントからサーバーのメソッドを呼び出す方法](#callserver)
- [接続の有効期間イベントを処理する方法](#connectionlifetime)
- [エラーを処理する方法](#handleerrors)
- [クライアント側のログ記録を有効にする方法](#logging)

サーバーや .NET クライアントをプログラミングする方法に関するドキュメントについては、次のリソースを参照してください。

- [SignalR ハブ API ガイド - サーバー](../guide-to-the-api/hubs-api-guide-server.md)
- [SignalR ハブ API ガイド - .NET クライアント](../guide-to-the-api/hubs-api-guide-net-client.md)

.NET 4.5 バージョンの API は API のリファレンス トピックへのリンクです。 .NET 4 を使用している場合は、次を参照してください。 [.NET 4 のバージョンを API のトピックの](https://msdn.microsoft.com/library/jj891075(v=vs.100).aspx)します。

<a id="genproxy"></a>

## <a name="the-generated-proxy-and-what-it-does-for-you"></a>生成されたプロキシとは何をします。

SignalR で生成されるプロキシの有無は、SignalR サービスと通信する JavaScript クライアントをプログラミングできます。 プロキシが何は、接続、サーバーを呼び出すと、書き込みメソッドを使用して、サーバーでメソッドを呼び出すコードの構文を簡略化します。

サーバー メソッドを呼び出すコードを記述する場合、生成されたプロキシを使用すると、次のローカル関数を実行した場合と同様の構文を使用: 書き込める`serverMethod(arg1, arg2)`の代わりに`invoke('serverMethod', arg1, arg2)`します。 生成されたプロキシ構文では、サーバーのメソッド名を入力し間違えた場合は、イミディ エイト ウィンドウと判読クライアント側のエラーもできます。 サーバーのメソッドを呼び出すコードを記述するため、IntelliSense のサポートも取得できます、プロキシを定義するファイルを手動で作成する場合。

たとえば、次のようなハブ クラスをサーバー上にあるとします。

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample1.cs?highlight=1,3,5)]

などを呼び出すための JavaScript コードは次のコード例を表示する、`NewContosoChatMessage`サーバーとの呼び出しを受信するメソッド、`addContosoChatMessageToPage`サーバーからのメソッド。

**生成されたプロキシを使用しました。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample2.js?highlight=1-2,8)]

**生成されたプロキシを使用せず**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample3.js?highlight=2-3,9)]

<a id="cantusegenproxy"></a>

### <a name="when-to-use-the-generated-proxy"></a>生成されたプロキシを使用する場合

サーバーを呼び出すクライアント メソッドの複数のイベント ハンドラーを登録する場合は、生成されたプロキシを使用できません。 それ以外の場合、生成されたプロキシを使用することができますか、コーディングの基本設定に基づいていません。 これを使用しないように選択した場合は、「signalr ハブ」の URL を参照する必要はありません、`script`クライアント コード内の要素。

<a id="clientsetup"></a>

## <a name="client-setup"></a>クライアントのセットアップ

JavaScript クライアントでは、jQuery、および SignalR core の JavaScript ファイルへの参照が必要です。 JQuery バージョンは、1.6.4 または 1.7.2、1.8.2、1.9.1 など、主要な以降のバージョンである必要があります。 生成されたプロキシを使用する場合は、SignalR が生成されたプロキシの JavaScript ファイルへの参照を必要もあります。 次の例では、生成されたプロキシを使用して HTML ページに表示される参照を示します。

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample4.html)]

この順序でこれらの参照を含める必要があります: jQuery 最初に、その後、SignalR core と SignalR プロキシが最後です。

<a id="dynamicproxy"></a>

### <a name="how-to-reference-the-dynamically-generated-proxy"></a>動的に生成されたプロキシを参照する方法

前の例では、SignalR が生成されたプロキシへの参照は、物理ファイルが、動的に生成された JavaScript コードが。 SignalR では、その場でプロキシの JavaScript コードを作成し、それを「/signalr ハブ」URL への応答でクライアントに提供します。 サーバーでは、SignalR 接続の場合は、さまざまなベース URL を指定したかどうか、`MapHubs`メソッドを動的に生成されるプロキシ ファイルの URL は、カスタムの URL に"/ハブ"が追加されます。

> [!NOTE]
> Windows 8 (Windows ストア) JavaScript クライアントは、動的に生成されたものではなく、物理プロキシ ファイルを使用します。 詳細については、次を参照してください。 [SignalR の物理ファイルを作成する方法は、プロキシを生成](#manualproxy)このトピックで後述します。


ASP.NET MVC 4 Razor ビューで、チルダを使用して、アプリケーション ルートに、プロキシ ファイルのリファレンスを参照してください。

[!code-html[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample5.html)]

MVC 4 で SignalR を使用する方法の詳細については、次を参照してください。 [SignalR と MVC 4 の概要](tutorial-getting-started-with-signalr-and-mvc-4.md)します。

ASP.NET MVC 3 Razor ビューで使用して`Url.Content`ファイル参照用プロキシに。

[!code-cshtml[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample6.cshtml)]

ASP.NET Web フォーム アプリケーションで使用`ResolveClientUrl`プロキシ ファイルの参照または (ティルダ以降)、アプリケーション ルートの相対パスを使用して、scriptmanager コントロールを使用して登録します。

[!code-aspx[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample7.aspx)]

一般的な規則としては、CSS または JavaScript ファイルを使用する「/signalr ハブ」の URL を指定するため、同じメソッドを使用します。 URL を指定するには、チルダを使用せず、一部のシナリオで、アプリケーションは正しく動作 IIS Express を使用して Visual Studio でのテストが完全な IIS に展開するときは、404 エラーで失敗する際にします。 詳細については、次を参照してください。**ルート レベルのリソースへの参照を解決する**で[ASP.NET Web プロジェクト用の Visual Studio で Web サーバー](https://msdn.microsoft.com/library/58wxa9w5.aspx) MSDN サイト。

デバッグ モードで Visual Studio 2012 で web プロジェクトを実行すると、お使いのブラウザーとして Internet Explorer を使用する場合は、プロキシ ファイルを確認できます**ソリューション エクスプ ローラー** **スクリプト ドキュメント**のように、次の図。

![ソリューション エクスプ ローラーでの JavaScript 生成されるプロキシ ファイル](signalr-1x-hubs-api-guide-javascript-client/_static/image1.png)

ファイルの内容を表示するをダブルクリックして**hubs**します。 Visual Studio 2012 および Internet Explorer を使用しない場合、またはデバッグ モードでない場合は、「/signalR ハブ」の URL を参照して、ファイルの内容を取得することもできます。 サイトが実行されている場合など`http://localhost:56699`に移動して、`http://localhost:56699/SignalR/hubs`お使いのブラウザーでします。

<a id="manualproxy"></a>

### <a name="how-to-create-a-physical-file-for-the-signalr-generated-proxy"></a>生成されたプロキシの SignalR の物理ファイルを作成する方法

動的に生成されたプロキシを代わりは、プロキシ コードを含む物理ファイルを作成し、そのファイルを参照します。 そのためにキャッシュまたはバンドルの動作を制御する場合またはサーバー メソッドの呼び出しをコーディングするときに、IntelliSense を取得することができます。

プロキシ ファイルを作成するには、次の手順を実行します。

1. インストール、 [Microsoft.AspNet.SignalR.Utils](https://nuget.org/packages/Microsoft.AspNet.SignalR.Utils/) NuGet パッケージ。
2. コマンド プロンプトを開きを参照、*ツール*SignalR.exe ファイルを含むフォルダー。 [ツール] フォルダーは、次の場所には。

    `[your solution folder]\packages\Microsoft.AspNet.SignalR.Utils.1.0.1\tools`
3. 次のコマンドを入力します。

    `signalr ghp /path:[path to the .dll that contains your Hub class]`

    パス、 *.dll*は通常、 *bin*プロジェクト フォルダー内のフォルダー。

    このコマンドは、という名前のファイルを作成します。 *server.js*と同じフォルダーに*signalr.exe*します。
4. 配置、 *server.js*プロジェクトの適切なフォルダーにファイル、アプリケーションの適切な名前を変更および「signalr ハブ」参照の代わりにへの参照を追加します。

<a id="establishconnection"></a>

## <a name="how-to-establish-a-connection"></a>接続を確立する方法

接続を確立する前に、接続オブジェクトを作成、プロキシを作成し、サーバーから呼び出すことができるメソッドのイベント ハンドラーを登録する必要があります。 プロキシとイベント ハンドラーが設定されているときに呼び出すことによって、接続の確立、`start`メソッド。

生成されたプロキシを使用している場合は、生成されたプロキシ コードによって処理されるため、独自のコードで接続オブジェクトを作成する必要はありません。

<a id="nogenconnection"></a>

**(生成されたプロキシ) との接続を確立します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample8.js?highlight=5)]

**(なし、生成されたプロキシ) の接続を確立します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample9.js?highlight=1,6)]

サンプル コードは、既定値を使用して"/signalr"SignalR サービスに接続するための URL。 別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)します。

> [!NOTE]
> 呼び出しの前にイベント ハンドラーを登録する通常、`start`メソッドは、接続を確立します。 接続の確立後にいくつかのイベント ハンドラーを登録すると、それを行うことができますが、少なくとも 1 つの呼び出しの前に、イベントを負いますを登録する必要があります、`start`メソッド。 この理由の 1 つがあります多くのハブ アプリケーションをトリガーする必要はありませんが、`OnConnected`うち 1 つを使用しようとするのみの場合は、すべてハブのイベント。 ハブのプロキシのクライアント メソッドの存在では、SignalR をトリガーするコードを指定は、接続が確立されたときに、`OnConnected`イベント。 呼び出しの前に、イベント ハンドラーを登録しない場合、`start`メソッド、ことができます、ハブはハブのメソッドを呼び出す`OnConnected`メソッドは呼び出されません、サーバーからクライアントのメソッドは呼び出されません。


<a id="connequivalence"></a>

### <a name="connectionhub-is-the-same-object-that-hubconnection-creates"></a>$connection.hub が同じオブジェクトのその $.hubConnection() を作成します。

生成されたプロキシを使用する場合の例では、ご覧のとおり`$.connection.hub`接続オブジェクトを参照します。 これは、同じオブジェクトを呼び出して取得する`$.hubConnection()`生成されたプロキシを使用していない場合。 生成されたプロキシ コードは、次のステートメントを実行することによって自動的に接続を作成します。

![生成されるプロキシ ファイルで接続の作成](signalr-1x-hubs-api-guide-javascript-client/_static/image3.png)

生成されたプロキシを使用しているときに操作を実行すると`$.connection.hub`生成されたプロキシを使用していないときに、接続オブジェクトで実行できます。

<a id="asyncstart"></a>

### <a name="asynchronous-execution-of-the-start-method"></a>Start メソッドの非同期実行

`start`メソッドを非同期的に実行します。 返します、 [jQuery 遅延オブジェクト](http://api.jquery.com/category/deferred-object/)などのメソッドを呼び出すことによってコールバック関数を追加するには、つまり`pipe`、`done`と`fail`します。 接続が確立された後に実行するコードがあればなど、サーバー メソッドへの呼び出し、コールバック関数でそのコードを配置またはコールバック関数から呼び出すことです。 `.done`接続が確立されているしにあるいずれかのコードを後でコールバック メソッドが実行される、`OnConnected`サーバー上のイベント ハンドラー メソッドの実行が終了します。

後のコードの次の行として前の例の「接続されているようになりました」ステートメントを配置した場合、`start`メソッドの呼び出し (ではなく、`.done`コールバック)、`console.log`行は、接続が確立される前に、次に示すように、実行されます例:

![接続が確立された後に実行されるコードを記述する方法を正しくないです。](signalr-1x-hubs-api-guide-javascript-client/_static/image5.png)

<a id="crossdomain"></a>

## <a name="how-to-establish-a-cross-domain-connection"></a>ドメイン間の接続を確立する方法

通常、ブラウザーがからページを読み込む場合`http://contoso.com`、SignalR 接続が同じドメイン内では`http://contoso.com/signalr`します。 場合、ページから`http://contoso.com`への接続は、`http://fabrikam.com/signalr`ドメイン間の接続されています。 セキュリティ上の理由から、ドメイン間の接続が既定で無効になります。 ドメイン間の接続を確立するには、サーバーで、ドメイン間の接続が有効であるかどうかを確認し、接続オブジェクトを作成するときに、接続 URL を指定します。 SignalR はテクノロジを使用して、適切なドメイン間の接続など[JSONP](http://en.wikipedia.org/wiki/JSONP)または[CORS](http://en.wikipedia.org/wiki/Cross-origin_resource_sharing)します。

サーバーで、ドメイン間の接続を有効に呼び出すときにそのオプションを選択して、`MapHubs`メソッド。

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample10.cs?highlight=2)]

クライアントでは、(せずに、生成されたプロキシ) 接続オブジェクトを作成するとき、または (生成されたプロキシ) を使用して start メソッドを呼び出す前に、URL を指定します。

**(生成されたプロキシ) を使用してドメイン間の接続を指定するクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample11.js?highlight=1)]

**ドメイン間の接続 (せずに、生成されたプロキシ) を指定するクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample12.js?highlight=1)]

使用すると、`$.hubConnection`コンス トラクターを含める必要はありません`signalr`URL に自動的に追加されるため (指定しない限り`useDefaultUrl`として`false`)。

複数の接続をさまざまなエンドポイントを作成できます。

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample13.js)]

> [!NOTE] 
> 
> - 設定しない`jQuery.support.cors`コードで true に設定します。
> 
>     ![JQuery.support.cors を true に設定しないでください。](signalr-1x-hubs-api-guide-javascript-client/_static/image7.png)
> 
>     SignalR では、JSONP または CORS の使用を処理します。 設定`jQuery.support.cors`に SignalR、ブラウザーは、CORS をサポートするいると仮定する原因になるので true JSONP を無効にします。
> - に localhost の URL に接続するときに Internet Explorer 10 は見なされませんが、ドメイン間の接続を、アプリケーションは、ローカルで IE 10 サーバー上のドメイン間の接続を有効にしていない場合でも、。
> - Internet Explorer 9 を使用したドメイン間の接続方法の詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/13573397/siganlr-ie9-cross-domain-request-dont-work)します。
> - Chrome を使用したドメイン間の接続方法の詳細については、次を参照してください。[この StackOverflow スレッド](http://stackoverflow.com/questions/15467373/signalr-1-0-1-cross-domain-request-cors-with-chrome)します。
> - サンプル コードは、既定値を使用して"/signalr"SignalR サービスに接続するための URL。 別の基本 URL を指定する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバー -/signalr URL](../guide-to-the-api/hubs-api-guide-server.md#signalrurl)します。


<a id="configureconnection"></a>

## <a name="how-to-configure-the-connection"></a>接続を構成する方法

接続を確立する前に、クエリ文字列パラメーターを指定またはトランスポート メソッドを指定できます。

<a id="querystring"></a>

### <a name="how-to-specify-query-string-parameters"></a>クエリ文字列パラメーターを指定する方法

クライアントが接続するときに、サーバーにデータを送信する場合は、接続オブジェクトをクエリ文字列パラメーターを追加できます。 次の例では、クライアント コードで、クエリ文字列パラメーターを設定する方法を示します。

**(生成されたプロキシ) を使用して start メソッドを呼び出す前に、クエリ文字列値を設定します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample14.js?highlight=1)]

**(生成されたプロキシ) なしの start メソッドを呼び出す前に、クエリ文字列値を設定します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample15.js?highlight=2)]

次の例では、サーバー コードでクエリ文字列パラメーターを読み取る方法を示します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample16.cs?highlight=5)]

<a id="transport"></a>

### <a name="how-to-specify-the-transport-method"></a>トランスポート メソッドを指定する方法

接続するプロセスの一環として、SignalR クライアントは、通常サーバーとクライアントの両方でサポートされている最適なトランスポートを決定する、サーバーとネゴシエートします。 呼び出すときに、トランスポート メソッドを指定することによってこのネゴシエーション プロセスをバイパスするには使用するトランスポートを既に知っている場合、`start`メソッド。

**クライアント コード (生成されたプロキシ) を使用したトランスポート メソッドを指定します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample17.js?highlight=1)]

**クライアント コード (せずに、生成されたプロキシ) トランスポート メソッドを指定します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample18.js?highlight=2)]

代わりに、SignalR を試したい順序で複数のトランスポート メソッドを指定できます。

**クライアント コード (生成されたプロキシ) を使用したカスタム トランスポート フォールバック スキームを指定します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample19.js?highlight=1)]

**カスタム トランスポート フォールバック スキーム (なし、生成されたプロキシ) を指定するクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample20.js?highlight=2)]

トランスポート メソッドを指定するため、次の値を使用できます。

- "webSockets"
- "foreverFrame"
- "serverSentEvents"
- "longPolling"

次の例では、どのトランスポート メソッドは、接続で使用されていることを見つける方法を示します。

**クライアント コード (生成されたプロキシ) との接続で使用されるトランスポート メソッドを表示します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample21.js?highlight=2)]

**クライアント コード (せずに、生成されたプロキシ) の接続で使用されるトランスポート メソッドを表示します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample22.js?highlight=3)]

サーバー コードでの転送方法を確認する方法については、次を参照してください。 [ASP.NET SignalR ハブ API ガイド - サーバーのコンテキスト プロパティからのクライアントに関する情報を取得する方法](../guide-to-the-api/hubs-api-guide-server.md#contextproperty)します。 トランスポートとフォールバックの詳細については、次を参照してください。 [SignalR のトランスポートとフォールバックの概要](../getting-started/introduction-to-signalr.md#transports)します。

<a id="getproxy"></a>

## <a name="how-to-get-a-proxy-for-a-hub-class"></a>ハブ クラスのプロキシを取得する方法

各接続オブジェクトを作成するには、1 つまたは複数のハブ クラスを含む SignalR サービスへの接続に関する情報がカプセル化します。 ハブ クラスを通信するには、(この場合、生成されたプロキシを使用していない) 自分で作成または生成するプロキシ オブジェクトを使用します。

クライアントでは、プロキシの名前は、ハブ クラス名の camel 形式のバージョンです。 SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。

**サーバー上のハブ クラス**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample23.cs?highlight=1)]

**ハブの生成されたクライアント プロキシへの参照を取得します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample24.js?highlight=1)]

**生成されたプロキシ) を含めず、ハブ クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample25.cs?highlight=1)]

使用してハブ クラスを修飾する場合、`HubName`属性、ケースを変更することがなく正確な名前を使用します。

**HubName 属性を持つサーバー上のハブ クラス**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample26.cs?highlight=1)]

**ハブの生成されたクライアント プロキシへの参照を取得します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample27.js?highlight=1)]

**生成されたプロキシ) を含めず、ハブ クラス用のクライアント プロキシを作成します。**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample28.cs?highlight=1)]

<a id="callclient"></a>

## <a name="how-to-define-methods-on-the-client-that-the-server-can-call"></a>サーバーが呼び出すことができるクライアントでメソッドを定義する方法

サーバーはハブから呼び出すことができるメソッドを定義するを使用してハブ プロキシにイベント ハンドラーを追加します。、 `client` 、生成されたプロキシ、または呼び出しのプロパティ、`on`メソッド、生成されたプロキシを使用していない場合。 パラメーターには、複雑なオブジェクトを指定できます。

呼び出す前に、イベント ハンドラーを追加、`start`メソッドは、接続を確立します。 (呼び出した後のイベント ハンドラーを追加する場合、`start`メソッドの注を参照してください[接続を確立する方法](#establishconnection)この前のドキュメントし、生成されたプロキシを使用せず、メソッドを定義するための構文を使用します)。

大文字と小文字が一致するメソッドの名前です。 たとえば、`Clients.All.addContosoChatMessageToPage`サーバーで実行`AddContosoChatMessageToPage`、 `addContosoChatMessageToPage`、または`addcontosochatmessagetopage`クライアント。

**(生成されたプロキシ) を使用するクライアントでメソッドを定義します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample29.js?highlight=2)]

**(生成されたプロキシ) を使用するクライアントでメソッドを定義する別の方法**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample30.js?highlight=1-2)]

**生成されたプロキシなし (start メソッドを呼び出した後に追加するときに) クライアントでメソッドを定義します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample31.js?highlight=3)]

**サーバー コードをクライアント メソッドを呼び出す**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample32.cs?highlight=5)]

次の例には、メソッド パラメーターとして、複雑なオブジェクトが含まれます。

**(生成されたプロキシ) を使用した複雑なオブジェクトを取得するクライアントでメソッドを定義します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample33.js?highlight=2-3)]

**(なし、生成されたプロキシ) の複雑なオブジェクトを取得するクライアントでメソッドを定義します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample34.js?highlight=3-4)]

**複雑なオブジェクトを定義するサーバー コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample35.cs?highlight=1)]

**サーバー コードを複雑なオブジェクトを使用して、クライアント メソッドを呼び出す**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample36.cs?highlight=3)]

<a id="callserver"></a>

## <a name="how-to-call-server-methods-from-the-client"></a>クライアントからサーバーのメソッドを呼び出す方法

クライアントからサーバー メソッドを呼び出しを使用して、 `server` 、生成されたプロキシのプロパティまたは`invoke`生成されたプロキシを使用していない場合は、ハブ プロキシのメソッド。 戻り値またはパラメーターには、複雑なオブジェクトを指定できます。

ハブのメソッド名の camel 形式でバージョンを渡します。 SignalR では、JavaScript コードは、JavaScript の規則に従うことができるように、この変更が自動的に作成します。

次の例では、戻り値を持たないサーバー メソッドを呼び出す方法と戻り値がサーバー メソッドを呼び出す方法を示します。

**HubMethodName 属性を持たないサーバー メソッド**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample37.cs?highlight=3)]

**パラメーターに渡される複雑なオブジェクトを定義するサーバー コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample38.cs)]

**(生成されたプロキシ) を使用して、サーバーのメソッドを呼び出すクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample39.js?highlight=1)]

**(なし、生成されたプロキシ) サーバーのメソッドを呼び出すクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample40.js?highlight=1)]

ハブ メソッドを修飾する場合、`HubMethodName`属性、ケースを変更することがなく、その名前を使用します。

**サーバー メソッド**HubMethodName 属性を持つ

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample41.cs?highlight=3)]

**(生成されたプロキシ) を使用して、サーバーのメソッドを呼び出すクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample42.js?highlight=1)]

**(なし、生成されたプロキシ) サーバーのメソッドを呼び出すクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample43.js?highlight=1)]

上記の例では、戻り値がサーバー メソッドを呼び出す方法を示しません。 次の例では、戻り値を持つサーバー メソッドを呼び出す方法を示します。

**メソッドの戻り値を持つサーバー コード**

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample44.cs?highlight=3)]

**使用される Stock クラス、** 戻り値

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample45.cs?highlight=1)]

**(生成されたプロキシ) を使用して、サーバーのメソッドを呼び出すクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample46.js?highlight=2,4-5)]

**(なし、生成されたプロキシ) サーバーのメソッドを呼び出すクライアント コード**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample47.js?highlight=2,4-5)]

<a id="connectionlifetime"></a>

## <a name="how-to-handle-connection-lifetime-events"></a>接続の有効期間イベントを処理する方法

SignalR は、次の接続に処理できる有効期間イベントを提供します。

- `starting`:すべてのデータが、接続経由で送信される前に発生します。
- `received`:接続でデータを受信したときに発生します。 受信したデータを提供します。
- `connectionSlow`:クライアントが低速または削除が頻繁に接続を検出したときに発生します。
- `reconnecting`:基になるトランスポートの再接続を開始するときに発生します。
- `reconnected`:基になるトランスポートが再接続されたときに発生します。
- `stateChanged`:接続状態が変更されたときに発生します。 以前の状態と新しい状態 (接続、接続、再接続、または切断) を提供します。
- `disconnected`:接続が切断されたときに発生します。

たとえば、次のように顕著な遅延を引き起こす可能性のある接続の問題が発生する警告メッセージを表示する場合、処理、`connectionSlow`イベント。

**(生成されたプロキシ) を使用した connectionSlow イベントを処理します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample48.js?highlight=1)]

**(なし、生成されたプロキシ) connectionSlow イベントを処理します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample49.js?highlight=2)]

詳細については、次を参照してください。 [SignalR の接続の有効期間イベントの処理と理解](index.md)します。

<a id="handleerrors"></a>

## <a name="how-to-handle-errors"></a>エラーを処理する方法

SignalR JavaScript クライアントは、提供、`error`イベントのハンドラーを追加することができます。 サーバー メソッドの呼び出しに起因するエラー ハンドラーを追加するのに fail メソッドを使用することもできます。

場合は、サーバー上の詳細なエラー メッセージを明示的に有効にしない、SignalR が返すエラーが発生した例外オブジェクトには、エラーに関する最小限の情報が含まれています。 呼び出しなど`newContosoChatMessage`失敗した場合、エラー オブジェクトにエラー メッセージが含まれています"`There was an error invoking Hub method 'contosoChatHub.newContosoChatMessage'.`"送信の詳細なエラー メッセージを有効にする場合は、セキュリティ上の理由の詳細なエラー メッセージを運用環境でクライアントには推奨されませんトラブルシューティングのため、サーバーで次のコードを使用します。

[!code-csharp[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample50.cs?highlight=2)]

次の例では、エラー イベントのハンドラーを追加する方法を示します。

**(生成されたプロキシ) を使用して、エラー ハンドラーを追加します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample51.js?highlight=1)]

**生成されたプロキシ) (なし、エラー ハンドラーを追加します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample52.js?highlight=2)]

次の例では、メソッドの呼び出しからエラーを処理する方法を示します。

**(生成されたプロキシ) を使用したメソッドの呼び出しからエラーを処理します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample53.js?highlight=2)]

**(なし、生成されたプロキシ) メソッドの呼び出しからエラーを処理します。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample54.js?highlight=2)]

メソッドの呼び出しに失敗した場合、`error`イベントが発生してもためで自分のコード、`error`メソッドのハンドラーと、`.fail`メソッドのコールバックが実行されます。

<a id="logging"></a>

## <a name="how-to-enable-client-side-logging"></a>クライアント側のログ記録を有効にする方法

接続でクライアント側のログ記録を有効にするには設定、`logging`を呼び出す前に、接続オブジェクトのプロパティ、`start`メソッドは、接続を確立します。

**(生成されたプロキシ) を使用してログ記録を有効にします。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample55.js?highlight=1)]

**(なし、生成されたプロキシ) のログ記録を有効にします。**

[!code-javascript[Main](signalr-1x-hubs-api-guide-javascript-client/samples/sample56.js?highlight=2)]

ログを表示するには、ブラウザーの開発者ツールを開きコンソール タブに移動します。これを行う方法を示すショット画面および詳細な手順を表示するチュートリアルについては、次を参照してください。[ログ記録を有効にする - ASP.NET Signalr によるサーバー ブロードキャスト](index.md)します。
