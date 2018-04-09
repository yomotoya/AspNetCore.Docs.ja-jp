---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
title: ASP.NET AJAX の Web サービスを理解する |Microsoft ドキュメント
author: scottcate
description: Web サービスは、分散システムの間でデータを交換するため、クロスプラット フォーム ソリューションを提供する .NET framework の不可欠な構成です。 Web しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 3332d6e7-e2e1-4144-b805-e71d51e7e415
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-web-services
msc.type: authoredcontent
ms.openlocfilehash: 0b9f61f895fea1960ebd25780454b86d5c3ba1bb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="understanding-aspnet-ajax-web-services"></a>ASP.NET AJAX の Web サービスを理解します。
====================
によって[Scott カテゴリ](https://github.com/scottcate)

[PDF をダウンロードします。](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial05_Web_Services_with_MS_Ajax_cs.pdf)

> Web サービスは、分散システムの間でデータを交換するため、クロスプラット フォーム ソリューションを提供する .NET framework の不可欠な構成です。 通常、さまざまなオペレーティング システム、オブジェクト モデルおよびデータを送受信するプログラミング言語を許可する Web サービスが使用されます、動的に ASP.NET AJAX ページにデータを挿入またはバックエンド システムにページからデータを送信する使用できます。 このすべては、操作のポストバックを使用しなくても実行できます。


## <a name="calling-web-services-with-aspnet-ajax"></a>ASP.NET AJAX を使用した Web サービスの呼び出し

Dan Wahlin

Web サービスは、分散システムの間でデータを交換するため、クロスプラット フォーム ソリューションを提供する .NET framework の不可欠な構成です。 通常、さまざまなオペレーティング システム、オブジェクト モデルおよびデータを送受信するプログラミング言語を許可する Web サービスが使用されます、動的に ASP.NET AJAX ページにデータを挿入またはバックエンド システムにページからデータを送信する使用できます。 このすべては、操作のポストバックを使用しなくても実行できます。

ASP.NET AJAX UpdatePanel コントロールには、AJAX を簡単な方法が用意されています。 ASP.NET ページを有効にする、UpdatePanel を使用せず、サーバー上のデータを動的にアクセスする必要がある場合があります。 この記事の内容を作成して、ASP.NET AJAX ページ内で Web サービスを利用してこれを実行する方法が表示されます。

この記事コア ASP.NET AJAX 拡張機能だけでなく、AutoCompleteExtender と呼ばれる ASP.NET AJAX ツールキットでの Web サービスを有効にコントロールで利用できる機能に焦点を当てます。 説明のトピックには、AJAX 対応の Web サービスを定義する、クライアント プロキシを作成および JavaScript での Web サービスの呼び出しが含まれます。 ASP.NET ページ メソッドを直接 Web サービスの呼び出しを行う方法も表示されます。

## <a name="web-services-configuration"></a>Web サービスの構成

Visual Studio 2008 に新しい Web サイト プロジェクトが作成されると、web.config ファイルは以前のバージョンの Visual Studio のユーザーになじみがない可能性がありますを新しく追加するメンバーの数がします。 これらの変更の一部"asp"プレフィックスにマップ ASP.NET AJAX コントロール HttpHandlers、HttpModules が必要な他のユーザーを定義するときにページで使用できるようにします。 加えられた変更を示しています 1 を一覧表示する、 `<httpHandlers>` Web サービス呼び出しに影響を与える web.config 内の要素。 HttpHandler .asmx 呼び出しを処理するために使用する既定値は削除され、System.Web.Extensions.dll アセンブリ内にある ScriptHandlerFactory クラスに置き換えられます。 System.Web.Extensions.dll には、すべての ASP.NET AJAX で使用されるコア機能が含まれています。

**1 を一覧表示します。ASP.NET AJAX Web サービスのハンドラーの構成**

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample1.xml)]

この HttpHandler 置換が JavaScript の Web サービス プロキシを使用して JavaScript Object Notation (JSON) 呼び出しを .NET Web サービスに ASP.NET AJAX ページから作成できるようにするために行われます。 ASP.NET AJAX では、通常 Web サービスに関連付けられている標準的な簡易オブジェクト アクセス プロトコル (SOAP) の呼び出しではなく、JSON メッセージを Web サービスに送信します。 これは、結果より小さな要求および応答メッセージ全体にします。 ASP.NET AJAX JavaScript ライブラリの最適化の JSON オブジェクトを操作するためもデータのクライアント側の処理をより効率的にできます。 2 および 3 の一覧表示する表示を JSON 形式にシリアル化された Web サービス要求と応答メッセージの例を示しますの一覧です。 リスト 2 に示すように要求メッセージは、リスト 3 の応答メッセージは、顧客オブジェクトの配列とその関連プロパティの中に「ベルギー」の値を持つ国パラメーターを渡します。

**2 を一覧表示します。Web サービス要求メッセージが JSON にシリアル化します。**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample2.json)]

> *> [!NOTE] 操作名が web サービスの URL の一部として定義されています。さらに、要求メッセージは、JSON を使用して常には送信されません。Web サービスは、ScriptMethod 属性を利用して UseHttpGet パラメーターが true の場合、これにより、経由で渡されるパラメーターを設定して、クエリ文字列パラメーター。*


**3 を一覧表示します。Web サービス応答メッセージが JSON にシリアル化します。**

[!code-json[Main](understanding-asp-net-ajax-web-services/samples/sample3.json)]

次のセクションでは、JSON 要求メッセージを処理し、両方単純型と複合型で応答することのできる Web サービスを作成する方法が表示されます。

## <a name="creating-ajax-enabled-web-services"></a>AJAX 対応 Web サービスを作成します。

ASP.NET AJAX framework には、Web サービスを呼び出すいくつかの方法が用意されています。 (ASP.NET AJAX Toolkit で使用可能) AutoCompleteExtender コントロールまたは JavaScript を使用することができます。 ただし、サービスを呼び出す前にする必要がある AJAX を有効にする、クライアント スクリプト コードから呼び出すことができるようにします。

初めて使用する ASP.NET Web サービス、かどうかに簡単に作成および AJAX を有効にするサービスがあります。 .NET framework は、ASP.NET Web サービスの作成 2002年の初期リリース以降、サポートされ、ASP.NET AJAX 拡張機能は、.NET framework の既定の機能セットに基づいて作成された追加の AJAX 機能を提供します。 Visual Studio .NET 2008 Beta 2 は、.asmx Web サービス ファイルを作成するための組み込みサポートを持ち、System.Web.Services.WebService クラスからクラスの横にある関連付けられているコードを自動的に派生します。 クラスにメソッドを追加すると、Web サービスのコンシューマーによって呼び出されるこれらのために WebMethod 属性を適用する必要があります。

リスト 4 GetCustomersByCountry() をという名前のメソッドに WebMethod 属性を適用する例を示します。

**4 を一覧表示します。Web サービスで WebMethod 属性の使用**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample4.cs)]

GetCustomersByCountry() メソッドでは、国のパラメーターを受け取り、顧客オブジェクトの配列を返します。 メソッドに渡されたの国の値は、データベースからデータを取得するデータ レイヤーのクラスがデータを顧客オブジェクトのプロパティを入力し、配列を返しますの呼び出しになるビジネス レイヤー クラスに転送されます。

## <a name="using-the-scriptservice-attribute"></a>ScriptService 属性を使用します。

Web メソッド属性を使用して、Web サービスに標準的な SOAP メッセージを送信するクライアントによって呼び出される GetCustomersByCountry() メソッドを追加するときに JSON の呼び出しをすぐに ASP.NET AJAX アプリケーションからことはできません。 JSON の ASP.NET AJAX 拡張機能の適用に必要になる呼び出しを許可する`ScriptService`属性を Web サービス クラスにします。 これにより、Web サービスで JSON を使用してフォーマットされた応答メッセージを送信でき、JSON メッセージを送信することによって、サービスを呼び出すためにクライアント側スクリプト。

5 を一覧表示する CustomersService をという名前の Web サービス クラスに ScriptService 属性を適用する例を示します。

**5 を一覧表示します。Web サービスを AJAX を有効にする ScriptService 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample5.cs)]

ScriptService 属性は、AJAX スクリプト コードから呼び出すことを示すマーカーとして機能します。 すべてのバック グラウンドで発生する JSON シリアル化または逆シリアル化のタスクを処理、しない実際にします。 (Web.config で構成されている) ScriptHandlerFactory およびその他の関連するクラスは、JSON の処理の大部分を行います。

## <a name="using-the-scriptmethod-attribute"></a>ScriptMethod 属性を使用します。

ScriptService 属性は、ASP.NET AJAX ページで使用するために .NET Web サービスで定義されている必要のある唯一の ASP.NET AJAX 属性です。 ただし、ScriptMethod をという名前の別の属性はサービスの Web メソッドの直接にも適用できます。 含む 3 つのプロパティを定義する ScriptMethod `UseHttpGet`、`ResponseFormat`と`XmlSerializeString`です。 これらのプロパティの値の変更は、Web メソッドで受け入れられる要求の種類が、Web メソッドの形式の生の XML データを返す必要がある場合、GET に変更する必要がある場合に役に立ちます、`XmlDocument`または`XmlElement`オブジェクトまたはから返されたデータの場合、 サービスは、JSON ではなく XML として常にシリアル化する必要があります。

プロパティは、Web メソッドを受け取らなければなりませんときに使用できる UseHttpGet は、POST 要求ではなく要求を取得します。 要求を送信するには、Web メソッドの入力パラメーターを使用して URL を使用してクエリ文字列パラメーターに変換されます。 設定されているプロパティの既定値は false とのみ UseHttpGet`true`操作の safe と機密データが Web サービスに渡されない場合のことが知られています。 リスト 6 UseHttpGet プロパティを持つ ScriptMethod 属性の使用例を示します。

**6 を一覧表示します。UseHttpGet プロパティを持つ ScriptMethod 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample6.cs)]

リスト 6 に示す HttpGetEcho Web メソッドが呼び出されたときに送信したヘッダーの例を次のとおりです。

`GET /CustomerViewer/DemoService.asmx/HttpGetEcho?input=%22Input Value%22 HTTP/1.1`

Web メソッドを HTTP GET 要求を受け入れるようにできるだけでなく、ScriptMethod 属性も使用できます XML の応答は、JSON ではなく、サービスから返される必要がある場合。 たとえば、Web サービスはリモート サイトから RSS フィードを取得し、XmlDocument または XmlElement オブジェクトとして返す場合があります。 XML の処理をデータにし、クライアントで発生します。

7 を一覧表示する ResponseFormat プロパティを使用して XML データを Web メソッドから返されることを指定する例を示します。

**7 を一覧表示します。ResponseFormat プロパティを持つ ScriptMethod 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample7.cs)]

ResponseFormat プロパティは、XmlSerializeString プロパティと共に使用できます。 XmlSerializeString プロパティが、既定値は false で、Web メソッドから返される文字列以外の型が XML としてシリアル化をすべて返すときに、`ResponseFormat`プロパティに設定されている`ResponseFormat.Xml`です。 ときに`XmlSerializeString`に設定されている`true`、Web メソッドから返されるすべての型は文字列型を含む XML としてシリアル化します。 ResponseFormat プロパティの値を持つ場合`ResponseFormat.Json`XmlSerializeString プロパティは無視されます。

リスト 8 XmlSerializeString プロパティを使って強制的に文字列を XML としてシリアル化する例を示します。

**8 を一覧表示します。XmlSerializeString プロパティを持つ ScriptMethod 属性を使用します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample8.cs)]

リスト 8 に示すように、GetXmlString Web メソッドの呼び出しから返される値は、[次へ] に表示されます。

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample9.cs)]

既定の JSON 形式では、要求および応答メッセージの全体的なサイズを最小化されより簡単に、ブラウザーに依存しない方法で ASP.NET AJAX クライアントによって使用される、ResponseFormat と XmlSerializeString プロパティができる場合に使用率が低いクライアントアプリケーションなど、Internet Explorer 5 以降では、Web メソッドから返される XML データを期待します。

## <a name="working-with-complex-types"></a>複合型の使用

5 を一覧表示する Web サービスから Customer という名前の複合型を返す例を示しました。 Customer クラスは、FirstName および LastName から構成などのプロパティとして内部的に複数の単純な種類を定義します。 入力パラメーターとして使用される複合型または AJAX 対応の Web メソッドの戻り値の型が JSON にクライアント側に送信される前に自動的にシリアル化します。 ただし、(別の種類内で内部的に定義されたもの)、入れ子になった複合型いないで利用可能なクライアントにスタンドアロン オブジェクトとして既定値です。

Web サービスによって使用される入れ子になった複合型必要がありますもする使用されている場合、クライアントのページで、Web サービスに ASP.NET AJAX GenerateScriptType 属性を追加できます。 たとえば、リスト 9 に示すように CustomerDetails クラスにはアドレス] と [性別のプロパティが含まれます***入れ子になった複合型を表します。***

**9 を一覧表示します。次に示す CustomerDetails クラスには、次の 2 つの入れ子になった複合型が含まれています。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample10.cs)]

リスト 9 に示すように CustomerDetails クラス内で定義されているアドレスと性別オブジェクトはありません自動的を可能にするを使用して JavaScript を使用してクライアント側で入れ子にされた型であるため (アドレスは、クラス、性別は列挙型)。 Web サービス内で使用される入れ子にされた型がクライアント側で使用可能なあります状況では、前に述べた GenerateScriptType 属性を使用できます (10 の一覧を参照してください)。 この属性は、別の入れ子になった複合型がサービスから返される位置の場合も複数回追加することができます。 これは、特定の Web メソッドの上または Web サービス クラスに直接適用できます。

**10 を一覧表示します。GenerateScriptService 属性を使用して、クライアントで利用できる必要がある入れ子にされた型を定義します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample11.cs)]

適用することによって、`GenerateScriptType`属性 Web サービス、アドレス、性別の種類を自動的に利用可能になります使用するクライアント側の ASP.NET AJAX JavaScript コードでします。 自動的に生成され、Web サービスに対して GenerateScriptType 属性を追加することによって、クライアントに送信する JavaScript の例は、11 の一覧に表示されます。 記事の後半で入れ子になった複合型を使用する方法が表示されます。

**11 を一覧表示します。入れ子になった複合型が ASP.NET AJAX ページからアクセスできます。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample12.cs)]

これで、Web サービスを作成し、ASP.NET AJAX ページにアクセスできるようにする方法を説明しました、作成および JavaScript プロキシを使用して、データを取得または Web サービスに送信されるようにする方法を見てをみましょう。

## <a name="creating-javascript-proxies"></a>JavaScript プロキシを作成します。

通常、標準の Web サービス (.NET または別のプラットフォーム) を呼び出すには、SOAP 要求と応答メッセージを送信する複雑な作業から明らかにならないようにするプロキシ オブジェクトの作成が含まれます。 、ASP.NET AJAX の Web サービス呼び出しで JavaScript プロキシを作成してシリアル化して、JSON メッセージを逆シリアル化を心配せずに簡単にサービスを呼び出すために使用します。 JavaScript プロキシは、ASP.NET AJAX ScriptManager コントロールを使用して自動的に生成できます。

Web サービスを呼び出すことができる JavaScript プロキシの作成は、ScriptManager のサービス プロパティを使用して行われます。 このプロパティでは、ASP.NET AJAX ページがポストバックの操作を必要とせず、データの送受信に非同期的に呼び出すことができる 1 つまたは複数のサービスを定義することができます。 ASP.NET AJAX を使用してサービスを定義する`ServiceReference`コントロールとコントロールの Web サービスの URL を割り当てる`Path`プロパティです。 12 を一覧表示する CustomersService.asmx をという名前のサービスを参照する例を示します。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample13.aspx)]

**12 を一覧表示します。ASP.NET AJAX ページで、Web サービスを定義します。**

ScriptManager コントロールを介して CustomersService.asmx への参照を追加すると、動的に生成され、ページによって参照されている JavaScript プロキシ。 使用して、プロキシが埋め込まれている、&lt;スクリプト&gt;タグ、および CustomersService.asmx ファイルを呼び出すことと、/js の末尾に追加することによって動的に読み込まれます。 次の例では、web.config でデバッグが無効になっているときに、ページ内に埋め込まれて JavaScript プロキシの方法を示しています。

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample14.html)]

> *> [!NOTE] かどうかは、生成される実際の JavaScript プロキシ コードを参照してくださいたい Internet Explorer のアドレス ボックスに目的の .NET Web サービスの URL を入力して/js の末尾に追加します。*


JavaScript プロキシのデバッグ バージョンとしてそのページに埋め込まれます web.config でデバッグが有効になっている場合は、[次へ] を表示します。

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample15.html)]

Scriptmanager コントロールによって作成された JavaScript プロキシもページに直接埋め込むことが可能ではなくを使用して参照、&lt;スクリプト&gt;タグの src 属性。 これは、ServiceReference コントロールの InlineScript のプロパティを true (既定値は false) に設定して実行できます。 これは、複数のページを使用して、サーバーへのネットワーク呼び出しの数を削減するには、プロキシが共有されていない場合に役立ちます。 InlineScript は、プロキシ スクリプトを true に設定するときにありませんブラウザーをキャッシュする、ため、ASP.NET AJAX アプリケーションの複数のページで、プロキシが使用されている場合、既定値は false をお勧めします。 InlineScript プロパティを使用する例を次に表示されます。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample16.aspx)]

## <a name="using-javascript-proxies"></a>JavaScript プロキシを使用します。

Web サービスは、ScriptManager コントロールを使用して、ASP.NET AJAX ページによって参照され、Web サービスへの呼び出しができるコールバック関数を使用して、返されたデータを処理できます。 Web サービスは、1 つ存在する場合)、名前空間を参照する、クラス名および Web メソッド名によって呼び出されます。 Web サービスに渡されるパラメーターは、返されたデータを処理するコールバック関数と共に定義できます。

JavaScript プロキシを使用する GetCustomersByCountry() をという名前の Web メソッドの呼び出しの例は、13 の一覧に表示されます。 GetCustomersByCountry() 関数は、エンドユーザーが、ページ上のボタンをクリックしたときに呼び出されます。

**13 を一覧表示します。JavaScript プロキシを備えた Web サービスの呼び出しです。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample17.js)]

この呼び出しが InterfaceTraining 名前空間を参照、CustomersService クラスと GetCustomersByCountry Web メソッドが、サービスで定義されています。 テキスト ボックスから取得した国の値だけでなく、非同期の Web サービス呼び出しが戻るときに呼び出すことができる OnWSRequestComplete をという名前のコールバック関数に渡します。 OnWSRequestComplete では、サービスから返される顧客オブジェクトの配列を処理し、ページに表示されているテーブルに変換します。 呼び出しから生成された出力は、図 1 に表示されます。


[![Web サービスへの非同期 AJAX 呼び出しをすることで取得したデータをバインドします。](understanding-asp-net-ajax-web-services/_static/image2.png)](understanding-asp-net-ajax-web-services/_static/image1.png)

**図 1**: Web サービスへの非同期 AJAX 呼び出しをすることで取得したデータをバインドします。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-web-services/_static/image3.png))


JavaScript プロキシは、Web メソッドを呼び出す必要がありますいてもプロキシは応答を待つ場合に Web サービスへの一方向の呼び出しをこともできます。 たとえば、サービスからの戻り値を待たないはなど作業フローの処理を開始する Web サービスを呼び出す可能性があります。 一方向の呼び出しがサービスに対して行う必要がある場合、13 の一覧に示すように、コールバック関数を単に省略できます。 コールバック関数が定義されていないため、プロキシ オブジェクトはデータを返す Web サービスを待機しません。

## <a name="handling-errors"></a>エラー処理

Web サービスへの非同期コールバックには、さまざまな種類の Web サービスが利用できないか、例外が返されるダウンされているネットワークなどのエラーが発生することができます。 さいわい、scriptmanager コントロールによって生成された JavaScript プロキシ オブジェクトは、エラーと、成功コールバックの前に示しただけでなくエラーを処理するために定義する複数のコールバックを許可します。 14 の一覧に示すように、Web メソッドの呼び出しで標準のコールバック関数の後にすぐに、エラーのコールバック関数を定義できます。

**14 を一覧表示します。エラーのコールバック関数を定義し、エラーを表示します。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample18.js)]

Web サービスが呼び出されたときに発生したエラーをパラメーターとしてエラーを表すオブジェクトを受け入れる呼び出せる OnWSRequestFailed() コールバック関数がトリガーされます。 Error オブジェクトは、エラーの原因をできるだけでなく、呼び出しがタイムアウトするかどうかを決定するさまざまな機能を公開します。別のエラー関数を使用する例を示しています 14 を一覧表示して、図 2 は、関数によって生成される出力の例を示します。


[![ASP.NET AJAX エラー関数を呼び出すことによって生成された出力します。](understanding-asp-net-ajax-web-services/_static/image5.png)](understanding-asp-net-ajax-web-services/_static/image4.png)

**図 2**: ASP.NET AJAX エラー関数を呼び出すことによって生成された出力します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-web-services/_static/image6.png))


## <a name="handling-xml-data-returned-from-a-web-service"></a>Web サービスから返された XML データの処理

前述方法 Web メソッドは、ResponseFormat プロパティと共に ScriptMethod 属性を使用して、生の XML データを返す可能性があります。 ResponseFormat が ResponseFormat.Xml に設定されている場合、Web サービスから返されたデータは JSON ではなく XML としてシリアル化されます。 これは、XML データは、JavaScript または XSLT を使用して処理するため、クライアントに直接渡される必要がある場合に役立ちます。 現在の時間には、Internet Explorer 5 以降は、最適なクライアント側オブジェクト モデルの解析と MSXML の組み込みサポートにより、XML データのフィルター処理を提供します。

Web サービスから XML データの取得は、他のデータ型を取得するよりも変わりません。 適切な関数を呼び出すし、コールバック関数を定義する JavaScript プロキシを呼び出すことによって開始します。 呼び出しが復帰し、コールバック関数でデータを処理できます。

15 を一覧表示する XmlElement オブジェクトを返す GetRssFeed() をという名前の Web メソッドの呼び出しの例に示します。 GetRssFeed() は、rss フィードを取得する URL を表す 1 つのパラメーターを受け入れます。

**15 を一覧表示します。Web サービスから返された XML データを操作します。**

[!code-html[Main](understanding-asp-net-ajax-web-services/samples/sample19.html)]

この例では、RSS フィードに URL を渡し、OnWSRequestComplete() 関数で返される XML データを処理します。 OnWSRequestComplete() は、まず、ブラウザーが Internet Explorer、MSXML パーサーが使用できるかどうかを知っているかどうかを確認します。 すべての検索に XPath ステートメントを使用する場合は、&lt;項目&gt;RSS フィード内のタグ。 各項目が反復し、関連付けられた&lt;タイトル&gt;と&lt;リンク&gt;タグがあるし、各項目のデータを表示するために処理します。 図 3 は、GetRssFeed() Web メソッドに JavaScript プロキシを介して呼び出しを ASP.NET AJAX の作成から生成された出力の例を示します。

## <a name="handling-complex-types"></a>複合型の処理

受理されるか、Web サービスによって返される、複合型は、JavaScript プロキシを自動的に公開されます。 ただし、前述したようにサービスに GenerateScriptType 属性が適用される場合を除き、入れ子になった複合型はクライアント側で直接アクセスできません。 クライアント側で入れ子になった複合型を使用する理由を指定ようにしますか。

この質問に答える、ASP.NET AJAX ページは顧客データを表示し、エンドユーザーは、顧客の住所を更新できると仮定します。 Web サービスは、アドレスの種類 (CustomerDetails クラス内で定義された複合型) をクライアントに送信できることを指定する場合、更新プロセスはより優れたコードの再利用の別の機能に分割できます。


[![出力から RSS データを返す Web サービスの呼び出しを作成します。](understanding-asp-net-ajax-web-services/_static/image8.png)](understanding-asp-net-ajax-web-services/_static/image7.png)

**図 3**: RSS データを返す Web サービスの呼び出しからの作成を出力します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-web-services/_static/image9.png))


16 を一覧表示するには、モデルの名前空間で定義されているアドレス オブジェクトを呼び出すクライアント側のコード例が更新されたデータが格納し、CustomerDetails オブジェクトのアドレス プロパティに割り当てますが示しています。 CustomerDetails オブジェクトは、処理のため、Web サービスに渡されます。

**16 を一覧表示します。入れ子になった複合型を使用します。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample20.js)]

## <a name="creating-and-using-page-methods"></a>作成して、ページ メソッドを使用します。

Web サービスは、さまざまな ASP.NET AJAX ページを含む、クライアントに再利用可能なサービスを公開する優れた方法を提供します。 ただし、ページが使用またはその他のページで共有されませんはこれまでデータを取得する必要がある場合があります。 ここでは、ページ データへのアクセスを許可する .asmx ファイルの作成に思えます過剰サービスが 1 つのページでのみ使用されるためです。

ASP.NET AJAX では、スタンドアロンの .asmx ファイルを作成する Web サービスのような呼び出しを行うための別のメカニズムを提供します。 これは、「ページのメソッド」と呼ばれる手法を使用して行います。 ページ メソッドは、それらに適用された WebMethod 属性を持つように、ページやコード側ファイルに直接埋め込む静的 (共有で VB.NET) メソッドです。 WebMethod 属性を適用することで呼び出すことができますを取得、実行時に動的に作成された内容をという名前の特殊な JavaScript オブジェクトを使用します。 内容のオブジェクトは、JSON のシリアル化または逆シリアル化プロセスを明らかにならないようにするプロキシとして機能します。 内容のオブジェクトを使用するために設定することする必要があります ScriptManager の EnablePageMethods プロパティを true に注意してください。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample21.aspx)]

ASP.NET 分離コード クラスの 2 つのページ メソッドを定義する例を示します 17 を一覧表示します。 これらのメソッドは、アプリにあるビジネス レイヤーのクラスからデータを取得\_web サイトのコード フォルダーです。

**17 を一覧表示します。ページのメソッドを定義します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample22.cs)]

ScriptManager がページに Web メソッドの存在を検出したときは、既に説明した内容オブジェクトへの参照を動的が生成されます。 Web メソッドを呼び出すことは、メソッドを渡す必要があるために必要なパラメーター データの名前、内容クラスを参照することによって実現されます。 18 を一覧表示する前に示した 2 つのページのメソッドの呼び出しの例を示しています。

**18 を一覧表示します。内容 JavaScript オブジェクトを持つページ メソッドを呼び出しています。**

[!code-javascript[Main](understanding-asp-net-ajax-web-services/samples/sample23.js)]

内容オブジェクトを使用することは、JavaScript プロキシ オブジェクトを使用してによく似ています。 最初に、すべてのページ メソッドに渡す必要があり、非同期呼び出しが返されたときに呼び出されるコールバック関数を定義するパラメーターのデータを指定します。 エラーのコールバックを指定することも (エラーの処理の例については 14 の一覧を参照してください)。

## <a name="the-autocompleteextender-and-the-aspnet-ajax-toolkit"></a>AutoCompleteExtender および ASP.NET AJAX 用ツールキット

ASP.NET AJAX ツールキット (から使用可能な[ http://ajax.asp.net ](http://ajax.asp.net)) Web サービスにアクセスするために使用するいくつかのコントロールを提供します。 具体的には、ツールキットのという名前に役立ちますコントロールを含む`AutoCompleteExtender`を使用して Web サービスを呼び出すし、すべての JavaScript コードをまったく記述しなくてもページ内のデータの表示をすることができます。

テキスト ボックスの既存の機能を拡張し、探しているデータを簡単に見つける複数ユーザーを支援する AutoCompleteExtender コントロールを使用できます。 テキスト ボックスに入力するとき、コントロールは Web サービスのクエリに使用できるし、動的にテキスト ボックスの下の結果を示します。 図 4 は、サポート アプリケーションの顧客 id を表示する AutoCompleteExtender コントロールを使用する例を示します。 ユーザーには、テキスト ボックスに別の文字が入力、その下にある入力に基づくさまざまなアイテムが表示されます。 ユーザーは、目的の顧客の id を選択できます。

ASP.NET AJAX ページ内で AutoCompleteExtender を使用するには、AjaxControlToolkit.dll アセンブリが、web サイトの bin フォルダーに追加することが必要です。 Toolkit アセンブリが追加されて、コントロールが含まれていますが、アプリケーションのすべてのページを使用できるように、web.config を参照する必要があります。 Web.config の内の次のタグを追加することによってこれ行う&lt;コントロール&gt;タグ。

[!code-xml[Main](understanding-asp-net-ajax-web-services/samples/sample24.xml)]

のみ必要がある場合、特定のページで、コントロールを使用するには、web.config の更新ではなく次に示すように、ページのトップへ参照ディレクティブを追加することで参照することができます。

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample25.aspx)]


[![AutoCompleteExtender コントロールを使用します。](understanding-asp-net-ajax-web-services/_static/image11.png)](understanding-asp-net-ajax-web-services/_static/image10.png)

**図 4**: AutoCompleteExtender コントロールを使用します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-web-services/_static/image12.png))


ASP.NET AJAX ツールキットを使用する web サイトを構成すると、AutoCompleteExtender コントロールを追加できるページにかなり正規 ASP.NET サーバー コントロールを追加すると同じようにします。 19 を一覧表示するコントロールを使用して Web サービスの呼び出しの例に示します。

**19 を一覧表示します。ASP.NET AJAX Toolkit AutoCompleteExtender コントロールを使用します。**

[!code-aspx[Main](understanding-asp-net-ajax-web-services/samples/sample26.aspx)]

AutoCompleteExtender には、サーバー コントロールに標準の ID と runat プロパティを含むいくつかの異なるプロパティがあります。 これらに加えてすることができますの何文字か Web サービスの前に、エンド ユーザー型がデータのクエリを定義します。 一覧表示する 19 に示すように MinimumPrefixLength プロパティは、文字がテキスト ボックスに入力されるたびに呼び出されるサービスをによりします。 注意のテキスト ボックス内の文字に一致する値の検索に呼び出されるたびに、ユーザーが文字、Web サービスを入力するために、この値を設定します。 Web サービスを呼び出すだけでなく、ターゲット Web メソッドは、それぞれ、ServicePath および ServiceMethod プロパティを使用して定義されます。 最後に、TargetControlID プロパティは、AutoCompleteExtender コントロールをフックするためにどの textbox を識別します。

呼び出される Web サービスは、前述したように適用される ScriptService 属性を持つ必要があり、ターゲット Web メソッドが prefixText およびカウントをという名前の 2 つのパラメーターを受け入れる必要があります。 PrefixText パラメーターは、エンドユーザーが入力した文字を表し、count パラメーターの値が返される項目の数を表します (既定値は 10)。 20 を一覧表示する前の一覧表示する 19 で示した AutoCompleteExtender コントロールにより呼び出されます GetCustomerIDs Web メソッドの例に示します。 Web メソッドをさらに呼び出し、データ レイヤーのデータをフィルター処理と一致する結果を返すを処理するメソッド、ビジネス レイヤー メソッドを呼び出します。 データ層のメソッドのコードは、21 の一覧に表示されます。

**20 を一覧表示します。AutoCompleteExtender コントロールから送信されたデータをフィルター処理します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample27.cs)]

**21 を一覧表示します。エンド ユーザー入力に基づく結果をフィルター処理します。**

[!code-csharp[Main](understanding-asp-net-ajax-web-services/samples/sample28.cs)]

## <a name="conclusion"></a>まとめ

ASP.NET AJAX では、大量の要求と応答メッセージを処理するカスタムの JavaScript コードを記述しなくても Web サービスの呼び出しの優れたサポートを提供します。 この記事の内容を見てきました ScriptManager コントロールを使用して JavaScript プロキシを定義する方法と JSON メッセージを処理できるように、.NET Web サービスを AJAX を有効にする方法です。 プロキシは、Web サービスの呼び出しに使用できる JavaScript が単純型と複合型を処理およびがエラーに対処方法も説明しました。 最後に、ページ メソッドを使用して、作成、および Web サービス呼び出しのプロセスを簡略化する方法と AutoCompleteExtender コントロールできますを提供する方法をエンドユーザーのヘルプ入力するときを見てきました。 ASP.NET AJAX で使用できる UpdatePanel では、その簡潔さのための AJAX プログラマの多くの選択肢の制御を確実には、JavaScript プロキシを介して Web サービスを呼び出す方法についての知識は多くのアプリケーションで役に立ちます。

## <a name="bio"></a>略歴

Dan Wahlin (Microsoft 最も有益な Professional ASP.NET と XML Web サービス) は、インターフェイスの技術的なトレーニングで .NET 開発インストラクターとアーキテクチャ コンサルタント ([http://www.interfacett.com](http://www.interfacett.com))。 Dan は ASP.NET 開発者 Web サイトの XML を設立 ([www.XMLforASP.NET](http://www.XMLforASP.NET))、INETA スピーカーの事務所では、いくつかの会議で読みます。 Dan 共同 Professional Windows DNA (Wrox)、ASP.NET で作成: ヒント、チュートリアルとコード (Sam)、ASP.NET 1.1 Insider ソリューション、Professional ASP.NET 2.0 AJAX (Wrox)、ASP.NET 2.0 MVP ハックおよび ASP.NET 開発者向け (Sam) の XML を作成します。 コード、文書またはブックが作成されません、ときに、Dan は書き込みと音楽を記録し、再生するゴルフと自分の子供と妻のバスケット楽しんでいます。

Scott カテゴリは、1997 年以降の Microsoft の Web テクノロジの使用されているがあり、myKB.com の代表者 ([www.myKB.com](http://www.myKB.com))、専門分野は、ASP.NET の書き込みの際にベースのアプリケーションのナレッジ ベースのソフトウェア ソリューションに重点を置きます。 Scott が接続時に電子メール[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)または彼のブログで[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-localization.md)
> [次へ](understanding-asp-net-ajax-debugging-capabilities.md)
