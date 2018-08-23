---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: ASP.NET AJAX Web サービスを理解する |Microsoft Docs
author: scottcate
description: Web サービスは、分散システムの間でデータを交換するためのクロスプラット フォーム ソリューションを提供する .NET framework の不可欠な部分です。 ただし Web.
ms.author: riande
ms.date: 03/28/2008
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 5e59077373b68b907391eff5349e1925222792a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41835838"
---
<a name="understanding-aspnet-ajax-web-services"></a>ASP.NET AJAX Web サービスを理解します。
====================
によって[Scott Cate](https://github.com/scottcate)

[PDF のダウンロード](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Web サービスは、分散システムの間でデータを交換するためのクロスプラット フォーム ソリューションを提供する .NET framework の不可欠な部分です。 Web サービスは、さまざまなオペレーティング システム、オブジェクト モデルおよびデータを送受信するプログラミング言語に通常使用される、動的に ASP.NET AJAX ページにデータを挿入またはバックエンド システムにページからデータを送信する使用できます。 このすべては、ポストバックの操作を使用しなくても実行できます。


## <a name="calling-web-services-with-aspnet-ajax"></a>ASP.NET AJAX を使用した Web サービスの呼び出し

Dan Wahlin

Web サービスは、分散システムの間でデータを交換するためのクロスプラット フォーム ソリューションを提供する .NET framework の不可欠な部分です。 Web サービスは、さまざまなオペレーティング システム、オブジェクト モデルおよびデータを送受信するプログラミング言語に通常使用される、動的に ASP.NET AJAX ページにデータを挿入またはバックエンド システムにページからデータを送信する使用できます。 このすべては、ポストバックの操作を使用しなくても実行できます。

ASP.NET AJAX UpdatePanel コントロールには、AJAX を使用する簡単な方法が用意されています。 いずれかの ASP.NET ページを有効にする、UpdatePanel を使用せず、サーバー上のデータを動的にアクセスする必要がある場合があります。 この記事では、作成および ASP.NET AJAX ページ内で Web サービスを使用してこれを実現する方法を確認します。

この記事で、AutoCompleteExtender と呼ばれる ASP.NET AJAX toolkit Web サービスを有効になっているコントロールと同様に、core の ASP.NET AJAX Extensions で利用できる機能に焦点を当てます。 AJAX 対応 Web サービスの定義、クライアント プロキシの作成および JavaScript による Web サービスの呼び出しについて書かれています。 また、ASP.NET ページ メソッドに直接 Web サービス呼び出しを行う方法も表示されます。

## <a name="web-services-configuration"></a>Web サービスの構成

Visual Studio 2008 で新しい Web サイト プロジェクトが作成されると、web.config ファイルは、さまざまな Visual Studio の以前のバージョンのユーザーになじみがない可能性がありますが、新しい追加機能が。 HttpHandlers、HttpModules は、必要な他のユーザー定義のページで使用できるように、"asp"プレフィックスを ASP.NET AJAX コントロール変更の一部マップします。 リスト 1 に加えられた変更を示しています、 `<httpHandlers>` Web サービス呼び出しに影響を与える web.config 内の要素。 HttpHandler .asmx 呼び出しを処理するために使用する既定値は削除され、System.Web.Extensions.dll アセンブリ内にある ScriptHandlerFactory クラスに置き換えられます。 System.Web.Extensions.dll には、すべての ASP.NET AJAX で使用されるコア機能が含まれます。

**1 を一覧表示します。ASP.NET AJAX Web サービス ハンドラーの構成**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

この HttpHandler の置換が JavaScript の Web サービス プロキシを使用して .NET Web サービスに対する ASP.NET AJAX ページから JavaScript Object Notation (JSON) の呼び出しを許可するために行われます。 ASP.NET AJAX では、通常 Web サービスに関連付けられている標準的な簡易オブジェクト アクセス プロトコル (SOAP) の呼び出しではなく、JSON メッセージを Web サービスに送信します。 これにより、小さな要求と応答メッセージの全体的です。 ASP.NET AJAX JavaScript ライブラリの最適化の JSON オブジェクトを操作するためもデータのより効率的なクライアント側処理できます。 2 とリスト 3 の表示例を JSON 形式にシリアル化された Web サービス要求と応答メッセージの一覧表示します。 リスト 2 に示すように要求メッセージは、リスト 3 の応答メッセージには、Customer オブジェクトの配列とその関連プロパティの中に、「ベルギー」の値を持つ国パラメーターを渡します。

**2 を一覧表示します。Web サービス要求メッセージが JSON にシリアル化します。**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] 操作名が web サービスの URL の一部として定義されています。さらに、要求メッセージは、JSON を使用して常には送信されません。Web サービスは UseHttpGet パラメーター経由で渡されるパラメーターが true に設定、ScriptMethod 属性を利用することができます、クエリ文字列パラメーター。*


**3 を一覧表示します。Web サービスの応答メッセージが JSON にシリアル化します。**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

次のセクションでは、JSON 要求メッセージの処理と単純かつ複雑な型に対応できる Web サービスを作成する方法を確認します。

## <a name="creating-ajax-enabled-web-services"></a>AJAX 対応 Web サービスを作成します。

ASP.NET AJAX フレームワークでは、Web サービスを呼び出すいくつかの方法を提供します。 (ASP.NET AJAX Toolkit で使用可能) AutoCompleteExtender コントロールまたは JavaScript を使用することができます。 ただし、サービスを呼び出す前にある AJAX を有効にする、クライアント スクリプト コードによって呼び出すことができるようにします。

ASP.NET Web サービスを初めてかどうかは、簡単に作成、および AJAX 対応のサービスがあります。 .NET framework には、2002 年の最初のリリース以降、ASP.NET Web サービスの作成がサポートされているし、ASP.NET AJAX Extensions は、.NET framework の既定の機能のセットに基づいて作成された追加の AJAX 機能を提供します。 Visual Studio .NET 2008 Beta 2 は、.asmx Web サービス ファイルを作成するための組み込みサポートを備え、System.Web.Services.WebService クラスからクラスの横にある関連付けられているコードを自動的に派生します。 クラスにメソッドを追加するに Web サービスのコンシューマーによって呼び出されるには、WebMethod 属性を適用する必要があります。

リスト 4 GetCustomersByCountry() という名前のメソッドに WebMethod 属性を適用する例を示します。

**4 を一覧表示します。Web サービスの WebMethod 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

GetCustomersByCountry() メソッドは、国のパラメーターを受け取り、顧客オブジェクトの配列が返されます。 メソッドに渡された国の値は、データベースからデータを取得するデータ層のクラスは、データを顧客オブジェクトのプロパティを入力し、配列を返す呼び出しますビジネス層のクラスに転送されます。

## <a name="using-the-scriptservice-attribute"></a>ScriptService 属性を使用します。

属性は、Web サービスに標準の SOAP メッセージを送信するクライアントによって呼び出される GetCustomersByCountry() メソッドを使用できます。 web メソッドを追加するときに JSON の呼び出しすぐに ASP.NET AJAX アプリケーションからにすることはできません。 されている ASP.NET AJAX 拡張機能の適用することができる JSON 呼び出しを許可する`ScriptService`属性を Web サービス クラス。 これにより、JSON を使用して書式設定された応答メッセージを送信する Web サービスとクライアント側スクリプトは、JSON メッセージを送信することによって、サービスの呼び出しを使用できます。

リスト 5. CustomersService をという名前の Web サービス クラスに ScriptService 属性を適用する例を示します。

**5 を一覧表示します。Web サービスを AJAX を有効にするための ScriptService 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

ScriptService 属性は、AJAX スクリプト コードから呼び出せることを示すマーカーとして機能します。 バック グラウンドで発生する JSON のシリアル化または逆シリアル化のタスクのいずれかを実際に処理がしません。 (Web.config で構成されている) ScriptHandlerFactory と関連するその他のクラスは、大量の JSON の処理を実行します。

## <a name="using-the-scriptmethod-attribute"></a>ScriptMethod 属性を使用します。

ScriptService 属性は、ASP.NET AJAX ページが使用するために .NET Web サービスで定義する必要のある唯一の ASP.NET AJAX 属性です。 ただし、ScriptMethod という名前の別の属性は、サービスの Web メソッドに直接も適用できます。 含む 3 つのプロパティを定義する ScriptMethod `UseHttpGet`、`ResponseFormat`と`XmlSerializeString`します。 これらのプロパティの値を変更するは、Web メソッドが受け入れる要求の種類が Web メソッドの形式で生の XML データを返す必要があるときに、GET に変更する必要がある場合に役立ちます、`XmlDocument`または`XmlElement`オブジェクトまたはから返されたデータを サービスは、JSON ではなく XML として常にシリアル化する必要があります。

プロパティは、Web メソッドが受け入れる必要があるときに、使用できる UseHttpGet では、POST 要求ではなく要求を取得します。 要求を送信するには、Web メソッドの入力パラメーターを持つ URL を使用して、クエリ文字列パラメーターに変換されます。 設定するプロパティの既定値は false とのみ UseHttpGet`true`操作の安全な機密データが Web サービスに渡されない場合が知られています。 リスト 6. UseHttpGet プロパティを使用して、ScriptMethod 属性を使用する例を示します。

**6 を一覧表示します。UseHttpGet プロパティでは、ScriptMethod 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

リスト 6 に示す HttpGetEcho Web メソッドが呼び出されたときに送信されるヘッダーの例を次に表示されます。

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Web メソッドを HTTP GET 要求を受け入れるようにできるだけでなく、ScriptMethod 属性こともできます XML 応答を JSON ではなく、サービスから返される必要がある場合。 たとえば、Web サービスはリモート サイトから RSS フィードを取得し、XmlDocument または XmlElement オブジェクトとして返す可能性があります。 XML の処理のデータにし、クライアントで発生します。

リスト 7. ResponseFormat プロパティを使用して XML データを Web メソッドから返されることを指定する例を示します。

**7 を一覧表示します。ResponseFormat プロパティでは、ScriptMethod 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

ResponseFormat プロパティは、XmlSerializeString プロパティと共にも使用できます。 XmlSerializeString プロパティが false で Web メソッドから返される文字列以外の型が XML としてシリアル化を返すすべての既定値を持つときに、`ResponseFormat`プロパティに設定されて`ResponseFormat.Xml`します。 ときに`XmlSerializeString`に設定されている`true`、Web メソッドから返されるすべての型が文字列型を含む XML としてシリアル化されます。 ResponseFormat プロパティの値を持つ場合`ResponseFormat.Json`XmlSerializeString プロパティは無視されます。

リスト 8. XmlSerializeString プロパティを使用して、強制的に文字列を XML としてシリアル化する例を示します。

**8 を一覧表示します。XmlSerializeString プロパティを使用して、ScriptMethod 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

一覧の 8 に示す GetXmlString Web メソッドの呼び出しから返される値は、[次へ] に表示されます。

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

ResponseFormat と XmlSerializeString プロパティを指定できますが、既定の JSON 形式では、要求および応答メッセージの全体的なサイズを最小限に抑えるし、クロス ブラウザーの方法で ASP.NET AJAX クライアントによってより簡単に使用される、使用するときにクライアントアプリケーションなど、Internet Explorer 5 以降では、Web メソッドから返される XML データが期待しています。

## <a name="working-with-complex-types"></a>複雑な型の使用

5 を一覧表示するには、Web サービスから Customer という名前の複合型を返すことの例で説明しました。 Customer クラスは、FirstName と LastName などのプロパティとして内部的にはいくつかの単純な種類を定義します。 入力パラメーターとして使用される複合型または AJAX 対応の Web メソッドの戻り値の型が JSON に、クライアント側に送信される前に自動的にシリアル化します。 ただし、(別の種類内で内部的に定義されたもの)、入れ子になった複合型いないで利用可能なクライアントにスタンドアロン オブジェクトとして既定値。

使用ケースが入れ子になった複合型、Web サービスで使用する必要がありますもクライアント ページで、Web サービスに ASP.NET AJAX GenerateScriptType 属性を追加できます。 リスト 9 に示すように CustomerDetails クラスがアドレス、性別のプロパティを格納するなどを***入れ子になった複合型を表します。***

**9 を一覧表示します。次のとおり、CustomerDetails クラスには、2 つの入れ子になった複合型が含まれています。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

リスト 9 に示すように CustomerDetails クラス内で定義されているアドレスと 性別のオブジェクトはありません自動的にできる使用可能な JavaScript を使用してクライアント側で入れ子にされた型であるため (アドレスは、クラス、性別は列挙型)。 Web サービス内で使用される入れ子にされた型がクライアント側で使用可能なあります状況では、前に説明した GenerateScriptType 属性を使用できます (10 の一覧を参照してください)。 この属性は、別の入れ子になった複合型がサービスから返される位置の場合に複数回追加することができます。 特定の Web メソッドの上または Web サービス クラスに直接適用できます。

**10 を一覧表示します。GenerateScriptService 属性を使用して、クライアントに利用できるように入れ子にされた型を定義します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

適用することで、`GenerateScriptType`属性 Web サービスでは、アドレス、性別の種類を自動的に利用可能になります使用するためクライアント側の ASP.NET AJAX の JavaScript コード。 自動的に生成され、Web サービスで GenerateScriptType 属性を追加することで、クライアントに送信する JavaScript の例は、一覧の 11 に示します。 記事の後半で入れ子になった複合型を使用する方法を確認します。

**11 を一覧表示します。入れ子になった複合型が ASP.NET AJAX ページを利用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

Web サービスを作成し、ASP.NET AJAX ページにアクセスできるようにする方法を確認したら、作成および JavaScript プロキシを使用してデータを取得または Web サービスに送信できるようにする方法を見てをみましょう。

## <a name="creating-javascript-proxies"></a>JavaScript プロキシの作成

通常、標準の Web サービス (.NET または別のプラットフォーム) を呼び出すには、送信 SOAP 要求と応答メッセージの複雑さからユーザを保護するプロキシ オブジェクトを作成する必要があります。 、ASP.NET AJAX Web サービス呼び出しで、JavaScript プロキシを作成して使用を簡単にシリアル化して、JSON メッセージを逆シリアル化について悩むことがなくサービスを呼び出すすることができます。 ASP.NET AJAX ScriptManager コントロールを使用して、JavaScript プロキシを自動的に生成できます。

Web サービスを呼び出すことができる JavaScript プロキシの作成は、scriptmanager コントロールのプロパティを使用して行われます。 このプロパティを使用すると、ASP.NET AJAX ページがポストバックの操作を必要とせず、データの送受信に非同期的に呼び出すことができる 1 つまたは複数のサービスを定義できます。 ASP.NET AJAX を使用してサービスを定義する`ServiceReference`コントロールとコントロールの Web サービスの URL を割り当てる`Path`プロパティ。 12 を一覧表示する CustomersService.asmx という名前のサービスを参照する例を示します。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**12 を一覧表示します。ASP.NET AJAX ページで、Web サービスを定義します。**

ScriptManager コントロールを介して CustomersService.asmx への参照を追加するが動的に生成され、ページによって参照される JavaScript プロキシ。 使用して、プロキシが埋め込まれた、&lt;スクリプト&gt;タグし、CustomersService.asmx ファイルを呼び出すの末尾に/js を追加することによって動的に読み込まれます。 次の例では、JavaScript プロキシがページに埋め込まれている web.config でデバッグを無効にする方法を示しています。

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] かどうかは、生成される実際の JavaScript プロキシ コードを参照してください。 たい Internet Explorer のアドレス ボックスに目的の .NET Web サービスの URL を入力して、の末尾に/js を追加します。*


Web.config としてページに埋め込まれる JavaScript プロキシのデバッグ バージョンでのデバッグが有効になっている場合は、[次へ] を表示します。

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Scriptmanager コントロールによって作成された JavaScript プロキシもページに直接埋め込むことではなくを使用して参照、&lt;スクリプト&gt;タグの src 属性。 これは、true (既定値は false) に ServiceReference コントロールの InlineScript のプロパティを設定して実行できます。 これは、複数のページを使用して、サーバーへのネットワーク呼び出しの数を削減するには、プロキシが共有されていない場合に役立ちます。 ASP.NET AJAX アプリケーションで複数のページで、プロキシが使用されている場合、既定値は false がお勧めしますがブラウザーで InlineScript プロキシ スクリプトを true に設定した場合をキャッシュしません。 InlineScript プロパティを使用しての例は、次に示すは。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>JavaScript プロキシを使用します。

Web サービスが ScriptManager コントロールを使用して、ASP.NET AJAX ページによって参照され、Web サービスへの呼び出しができるコールバック関数を使用して、返されるデータを処理できます。 Web サービスは、(存在する場合は、その名前空間を参照する、クラス名、および Web メソッド名によって呼び出されます。 返されるデータを処理するコールバック関数と共に、Web サービスに渡されるパラメーターを定義できます。

GetCustomersByCountry() という名前の Web メソッドを呼び出す JavaScript プロキシを使用する例は、13 の一覧に示します。 GetCustomersByCountry() 関数は、エンドユーザーが、ページ上のボタンをクリックしたときに呼び出されます。

**13 を一覧表示します。JavaScript プロキシを使用した Web サービスの呼び出し。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

この呼び出しは InterfaceTraining 名前空間を参照、CustomersService クラスと GetCustomersByCountry Web メソッドは、サービスで定義します。 テキスト ボックスから取得した国の値だけでなく、非同期 Web サービス呼び出しが戻るときに呼び出す必要がある OnWSRequestComplete をという名前のコールバック関数が渡されます。 OnWSRequestComplete は、サービスから返された Customer オブジェクトの配列を処理し、ページに表示されるテーブルに変換します。 図 1 に、呼び出しから生成された出力が表示されます。


[![Web サービスへの非同期 AJAX 呼び出しを行うことで取得されたデータをバインドします。](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**図 1**: Web サービスへの非同期 AJAX 呼び出しを行うことで取得したデータをバインドします。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-web-services/_static/image3.png))。


JavaScript プロキシは、Web メソッドを呼び出す必要がありますが、プロキシは、応答を待つべきではない場合に Web サービスへの一方向の呼び出しをこともできます。 たとえば、作業フローなどのプロセスの開始が、サービスからの戻り値を待たずに、Web サービスを呼び出したい場合があります。 一方向の呼び出しがサービスにする必要がある場合、リスト 13 に示すように、コールバック関数を単に省略できます。 コールバック関数が定義されていないため、プロキシ オブジェクト データを返す Web サービスは待機しません。

## <a name="handling-errors"></a>エラー処理

Web サービスへの非同期コールバックには、さまざまな種類の Web サービスが利用できないか、例外が返されるダウンされているネットワークなどのエラーが発生する可能性が。 さいわい、scriptmanager コントロールによって生成された JavaScript プロキシ オブジェクトは、エラーと前に示した成功のコールバックだけでなく、障害を処理するために定義する複数のコールバックを許可します。 14 の一覧で示すように、Web メソッドの呼び出しで標準的なコールバック関数の後にすぐに、コールバック関数がエラーを定義できます。

**14 を一覧表示します。エラーのコールバック関数を定義して、エラーを表示します。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Web サービスが呼び出されたときに発生したエラーをパラメーターとしてエラーを表すオブジェクトを受け取りますが呼び出される OnWSRequestFailed() コールバック関数がトリガーされます。 エラー オブジェクトは、呼び出しがタイムアウトしたかどうかも、エラーの原因を特定するさまざまな機能を公開します。別のエラー関数を使用しての例を示します 14 を一覧表示して、図 2 は、関数によって生成される出力の例を示します。


[![ASP.NET AJAX エラー関数を呼び出すことによって生成された出力。](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**図 2**: ASP.NET AJAX エラー関数を呼び出すことによって生成された出力。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-web-services/_static/image6.png))。


## <a name="handling-xml-data-returned-from-a-web-service"></a>Web サービスから返された XML データの処理

以前、Web メソッドがその ResponseFormat プロパティと共に ScriptMethod 属性を使用して生の XML データを返すことができる方法を説明しました。 ResponseFormat が ResponseFormat.Xml に設定されている場合、Web サービスから返されるデータは JSON ではなく XML としてシリアル化します。 これは、XML データは、JavaScript または XSLT を使用して処理するために、クライアントに直接渡される必要がある場合に役立ちます。 現在の時間には、Internet Explorer 5 以降は、解析と MSXML の組み込みサポートのための XML データをフィルター処理の最適なクライアント側オブジェクト モデルを提供します。

Web サービスから XML データの取得は、他のデータ型を取得するよりも同じです。 適切な関数を呼び出すし、コールバック関数を定義する JavaScript プロキシを呼び出すことで開始します。 呼び出しから戻ると、コールバック関数でデータを処理できます。

15 を一覧表示するには、XmlElement オブジェクトを返す GetRssFeed() をという名前の Web メソッドを呼び出す例については示しています。 GetRssFeed() では、RSS フィードを取得するための URL を表す 1 つのパラメーターを受け入れます。

**15 を一覧表示します。Web サービスから返された XML データを操作します。**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

この例では、RSS フィードに URL を渡し、OnWSRequestComplete() 関数で返された XML データを処理します。 OnWSRequestComplete() は、まず、ブラウザーが Internet Explorer、MSXML パーサーが使用できるかどうかを把握するかどうかを確認します。 すべての検索に XPath ステートメントを使用する場合は、&lt;項目&gt;RSS フィード内のタグ。 各項目が反復と、関連付けられた&lt;タイトル&gt;と&lt;リンク&gt;タグがあるし、各項目のデータを表示するために処理します。 図 3 は、ASP.NET AJAX が GetRssFeed() Web メソッドに JavaScript プロキシ経由の呼び出しを行うから生成された出力の例を示します。

## <a name="handling-complex-types"></a>複合型の処理

承認されるか、Web サービスによって返される、複合型は、JavaScript プロキシを自動的に公開されます。 ただし、既に説明したようにサービスに GenerateScriptType 属性が適用される場合を除き、入れ子になった複合型はクライアント側で直接アクセスできません。 クライアント側で入れ子になった複合型を使用する理由はしますか。

この質問に答える、ASP.NET AJAX ページが顧客データが表示され、エンドユーザーが顧客の住所を更新することを想定しています。 Web サービスでは、アドレスの種類 (CustomerDetails クラス内で定義された複合型) をクライアントに送信できることを指定する場合、更新プロセスはより優れたコードの再利用の個別の関数に分割できます。


[![出力から RSS データを返す Web サービスの呼び出しを作成します。](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**図 3**: RSS データを返す Web サービスの呼び出しからの作成を出力します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-web-services/_static/image9.png))。


16 を一覧表示するには、モデルの名前空間で定義されているアドレス オブジェクトを呼び出すクライアント側コードの例は、更新されたデータを読み込み、CustomerDetails オブジェクトのアドレスのプロパティに割り当てられますが表示されます。 CustomerDetails オブジェクトは、処理し、Web サービスに渡されます。

**16 を一覧表示します。入れ子になった複合型の使用**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>作成して、ページ メソッドを使用します。

Web サービスは、さまざまな ASP.NET AJAX ページを含むクライアントに再利用可能なサービスを公開する優れた方法を提供します。 ただし、ページが使用またはその他のページで共有されませんはこれまでデータを取得する必要がある場合があります。 ここでは、ページ データへのアクセスを許可する .asmx ファイルを作成するように思えるかもしれません過剰サービスが 1 つのページでのみ使用されるためです。

ASP.NET AJAX では、スタンドアロンの .asmx ファイルを作成しなくても Web サービスのような呼び出しを行うための別のメカニズムを提供します。 これは、「ページのメソッド」と呼ばれる手法を使用して行います。 ページ メソッドは、それらに適用された WebMethod 属性を持つように、ページやコード側ファイルに直接埋め込まれた静的 (VB.NET では shared) メソッドです。 WebMethod 属性を適用することで呼び出すことができますを取得、実行時に動的に作成された内容をという名前の特殊な JavaScript オブジェクトを使用します。 PageMethods オブジェクトは、JSON のシリアル化/逆シリアル化プロセスからユーザを保護するためのプロキシとして機能します。 PageMethods オブジェクトを使用するために設定することする必要があります、scriptmanager コントロールの EnablePageMethods プロパティを true に注意してください。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

17 を一覧表示する ASP.NET 分離コード クラスの 2 つのページ メソッドを定義する例を示します。 これらのメソッドは、アプリにあるビジネス層のクラスからデータを取得\_web サイトのコードのフォルダー。

**17 を一覧表示します。ページ メソッドを定義します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

Scriptmanager コントロールがページに Web メソッドの存在を検出したときは、前述の PageMethods オブジェクトへの動的参照を生成します。 Web メソッドを呼び出すと、後に、名前、メソッドを渡す必要があるために必要なパラメーター データの PageMethods クラスを参照することによって実現されます。 18 を一覧表示する前に示した 2 つのページ メソッドの呼び出しの例を示します。

**18 を一覧表示します。ページ メソッドを呼び出す PageMethods JavaScript オブジェクトを使用します。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

PageMethods オブジェクトを使用することは、JavaScript プロキシ オブジェクトを使用してによく似ています。 最初に、すべてのページ メソッドに渡す必要があり、非同期呼び出しが戻るときに呼び出す必要があるコールバック関数を定義するパラメーターのデータを指定します。 エラーのコールバックを指定することも (障害の処理の例については 14 の一覧を参照してください)。

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender と ASP.NET AJAX Toolkit

ASP.NET AJAX Toolkit (から使用可能な[ http://ajax.asp.net ](http://ajax.asp.net)) Web サービスへのアクセスに使用できるいくつかのコントロールを提供します。 具体的には、ツールキットがという名前の便利なコントロールを含む`AutoCompleteExtender`を使用して、Web サービスを呼び出すし、すべての JavaScript コードをまったく記述しなくても、ページ内のデータを表示できます。

テキスト ボックスの既存の機能を拡張して、ユーザーの詳細について簡単に探しているデータを見つけるには、AutoCompleteExtender コントロールを使用できます。 テキスト ボックスに入力するとき、コントロールが Web サービスを照会するために使用してテキスト ボックスの下の結果が動的に表示されます。 図 4 は、サポートのアプリケーションの顧客 id を表示する AutoCompleteExtender コントロールを使用する例を示します。 ように、テキスト ボックスに別の文字を入力すると、次の入力に基づいてさまざまな項目が表示されます。 ユーザーは、目的の顧客 id を選択できます。

ASP.NET AJAX ページ内で AutoCompleteExtender を使用するには、AjaxControlToolkit.dll アセンブリは、web サイトの bin フォルダーに追加する必要があります。 ツールキットのアセンブリが追加されると、コントロールが含まれていますが、アプリケーションのすべてのページを使用できるように、web.config で参照します。 Web.config のでは、次のタグを追加することによってこれできます&lt;コントロール&gt;タグ。

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

だけする必要がある特定のページでコントロールを使用する場合に、web.config の更新ではなく次のように、ページの上部に Reference ディレクティブを追加することで参照できます。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![AutoCompleteExtender コントロールを使用します。](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**図 4**: AutoCompleteExtender コントロールを使用します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-web-services/_static/image12.png))。


ASP.NET の AJAX Toolkit を使用して、web サイトを構成すると、AutoCompleteExtender コントロールを追加できますページにはるか通常の ASP.NET サーバー コントロールを追加する感覚です。 19 を一覧表示する Web サービスを呼び出すコントロールを使用する例を示します。

**19 を一覧表示します。ASP.NET AJAX Toolkit AutoCompleteExtender コントロールを使用します。**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender には、サーバー コントロールの標準的な ID と runat プロパティを含むいくつかの異なるプロパティがあります。 これらに加えて、Web サービスの前に、エンド ユーザー型がデータの照会文字の数を定義するためです。 MinimumPrefixLength プロパティを一覧表示する 19 に示すようにすると、文字がテキスト ボックスに入力されるたびに呼び出されるサービス。 注意のテキスト ボックス内の文字に一致する値を検索する呼び出されるたびに、ユーザーが文字、Web サービスを入力するために、この値を設定します。 Web サービスを呼び出すと、ターゲット Web メソッドは、それぞれ、ServicePath および ServiceMethod プロパティを使用して定義されます。 最後に、TargetControlID プロパティは、どちらのテキスト ボックスにフックする AutoCompleteExtender コントロールを識別します。

呼び出される Web サービスは既に説明したように適用された ScriptService 属性が必要し、ターゲットの Web メソッドが prefixText と数という 2 つのパラメーターを受け入れる必要があります。 PrefixText パラメーターは、エンドユーザーが入力した文字を表し、count パラメーターの値が返される項目の数を表します (既定値は 10)。 20 を一覧表示するには、19 の一覧の前半で示した AutoCompleteExtender コントロールによって呼び出された GetCustomerIDs Web メソッドの例は示しています。 Web メソッドをさらに呼び出し、データ層がデータをフィルター処理と一致する結果を返すを処理するメソッドをビジネス層のメソッドを呼び出します。 データ層のメソッドのコードは、21 の一覧に表示されます。

**20 を一覧表示します。AutoCompleteExtender コントロールから送信されたデータをフィルター処理します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**21 を一覧表示します。エンド ユーザー入力に基づいて結果をフィルター処理します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>まとめ

ASP.NET AJAX は、大量の要求と応答メッセージを処理するカスタムの JavaScript コードを記述することがなく Web サービスを呼び出すための優れたサポートを提供します。 この記事で説明した JSON メッセージを処理および、ScriptManager コントロールを使用して JavaScript プロキシを定義する方法を有効にする .NET Web サービスを AJAX を有効にする方法。 Web サービスを呼び出すにはプロキシを使用できる JavaScript の単純および複雑な型を処理およびエラーが処理も説明しました。 最後に、ページ メソッドを使用して、作成、および Web サービス呼び出しのプロセスを簡略化する方法と AutoCompleteExtender コントロールできるようにする最後のユーザーにヘルプを入力するときに説明しました。 ASP.NET AJAX で使用可能な UpdatePanel はその簡潔さのための多くの AJAX プログラマのための任意のコントロールできますが確かに、JavaScript プロキシ経由の Web サービスを呼び出す方法を知ることは多くのアプリケーションで役に立ちます。

## <a name="bio"></a>自己紹介

Dan Wahlin (Microsoft Most Valuable Professional ASP.NET と XML Web サービス) は、技術トレーニングをインターフェイスで .NET の開発インストラクターとアーキテクチャ コンサルタント ([http://www.interfacett.com](http://www.interfacett.com))。 Dan は ASP.NET 開発者 Web サイトの XML を設立 ([www.XMLforASP.NET](http://www.XMLforASP.NET))、INETA の講演者の局に、いくつかのカンファレンスで講演しています。 Dan は、Professional Windows DNA (Wrox)、ASP.NET を共同執筆: ヒント、チュートリアルとコード (Sam)、ASP.NET 1.1 Insider ソリューション、Professional ASP.NET 2.0 AJAX (Wrox)、ASP.NET 2.0 の MVP のハッキングおよび ASP.NET 開発者向け (Sam) の XML を作成します。 コード、記事や書籍を書き込んで彼がいない、ときに書き込み、音楽の録音およびゴルフと彼の妻と子供のバスケット ボールの再生 Dan を楽しんでいます。

1997 年からマイクロソフトの Web テクノロジで働いてあり myKB.com プレジデント、Scott Cate ([www.myKB.com](http://www.myKB.com)) ベースのナレッジ ベースのソフトウェア ソリューションに重点を置いてアプリケーションを ASP.NET の記述を専門としています。 Scott は時に電子メールが接続可能[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)またはで彼のブログ[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-localization.md)
> [次へ](understanding-asp-net-ajax-debugging-capabilities.md)
