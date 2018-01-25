---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: "ASP.NET Web API 2 のルーティングを属性 |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/20/2014
ms.topic: article
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: 173add73a150d3e13ae243d6548463da912dadee
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 での属性のルーティング
====================
によって[Mike Wasson](https://github.com/MikeWasson)

*ルーティング*Web API がアクションへの URI に一致します。 新しい型をサポートする web API 2 のルーティングでは、次のように呼び出されます。*属性がルーティング*です。 名前からわかるようには、ルートを定義するのに属性を使用する属性がルーティングされます。 属性がルーティングすること、Uri より詳細に制御での web API。 たとえば、リソースの階層構造を記述する Uri を簡単に作成できます。

以前のルーティングと呼ばれるスタイル規則に基づくルーティングが引き続きサポートされます。 実際には、同じプロジェクト内の両方の方法を組み合わせることができます。

このトピックでは、属性のルーティングを有効にする方法について説明し、属性のルーティングのさまざまなオプションについて説明します。 属性のルーティングを使用するエンド ツー エンド チュートリアルでは、次を参照してください。[属性のルーティングが Web API 2 の REST API の作成](create-a-rest-api-with-attribute-routing.md)です。


## <a name="prerequisites"></a>必須コンポーネント

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional、または Enterprise Edition

または、NuGet Package Manager を使用して、必要なパッケージをインストールします。 **ツール**選択、Visual Studio のメニュー**ライブラリ パッケージ マネージャー**選択してから、 **Package Manager Console**です。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>ルーティング属性なぜですか。

使用される Web API の最初のリリース*規約に基づく*ルーティングします。 その型のルーティングでは、いずれかを定義または基本的には複数のルート テンプレート文字列をパラメーター化します。 フレームワークは、要求を受信したときに、ルート テンプレートに対して URI が一致します。 (規則ベースのルーティングの詳細については、次を参照してください。 [ASP.NET Web API でルーティング](routing-in-aspnet-web-api.md)です。

規則ベースのルーティングの利点の 1 つは、テンプレートが 1 つの場所で定義されていることと、ルーティング ルールは、すべてのコント ローラー間で一貫して適用されます。 残念ながら、規則ベースのルーティングを困難に RESTful Api で共通している特定の URI パターンをサポートします。 リソースが多くの場合、子リソースを含める例: 顧客が注文をして、ムービーのアクター、ブック、作成者があるなどです。 これらの関係を反映する Uri を作成する自然です。

`/customers/1/orders`

この種類の URI は、規則ベースのルーティングを使用して作成するが困難です。 これを実行できますが、多くのコント ローラーやリソースの種類がある場合にも結果しないスケールします。

属性のルーティングが、この URI のルートを定義も簡単です。 単に、属性を追加するには、コント ローラーのアクションにします。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

属性のルーティングでは簡単なその他のパターンを示します。

**API のバージョン管理**

この例では「/api/v1/製品」は、別のコント ローラーよりもルーティング「/api/v2/製品」になります。

`/api/v1/products`  
`/api/v2/products`

**オーバー ロードされた URI セグメント**

この例では「1」は、注文番号がコレクションに「保留中」にマップします。

`/orders/1`  
`/orders/pending`

**複数のパラメーター型**

この例では「1」は、注文番号けれども、「16/2013/06」は、日を指定します。

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>属性のルーティングを有効にします。

属性のルーティングを有効にするを呼び出す**MapHttpAttributeRoutes**構成時にします。 この拡張メソッドが定義されている、 **System.Web.Http.HttpConfigurationExtensions**クラスです。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

属性のルーティングと組み合わせて使用できます[規約に基づく](routing-in-aspnet-web-api.md)ルーティングします。 呼び出し規約に基づくルートを定義する、 **MapHttpRoute**メソッドです。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Web API の構成の詳細については、次を参照してください。 [ASP.NET Web API 2 の構成](../advanced/configuring-aspnet-web-api.md)です。

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>注: が Web API 1 から移行します。

Web API 2 の前に、Web API プロジェクト テンプレートには、このようなコードが生成されます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

属性のルーティングを有効にすると、このコードで例外がスローされます。 属性のルーティングを使用する既存の Web API プロジェクトをアップグレードする場合を次にこの構成コードを更新することを確認してください。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> 詳細については、次を参照してください。[ホスティングで ASP.NET Web API を構成する](../advanced/configuring-aspnet-web-api.md#webhost)です。


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>ルート属性を追加します。

属性を使用して定義されたルートの例を次に示します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

文字列&quot;顧客/{customerId} orders&quot;はルートの URI テンプレートです。 Web API は、テンプレートには、要求 URI の一致を試みます。 この例では"customers"および"orders"は、リテラルのセグメントと"{customerId}"が変数のパラメーターです。 次の Uri には、このテンプレートと一致します。

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

使用して照合を制限する[制約](#constraints)、このトピックの後で説明します。

注意して、 &quot;{customerId}&quot;ルート テンプレートのパラメーターの名前に一致する、 *customerId*メソッドのパラメーターです。 Web API コント ローラーのアクションを呼び出すと、ルート パラメーターをバインドしようとします。 たとえば、次の URI が`http://example.com/customers/1/orders`、Web API しよう値「1」をバインドする、 *customerId*アクションのパラメーターです。

URI テンプレートは、いくつかのパラメーターを持つことができます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

ルート属性がないコント ローラーのメソッドは、規則ベースのルーティングを使用します。 このように、同じプロジェクト内のルーティングの両方の種類を組み合わせることができます。

## <a name="http-methods"></a>HTTP メソッド

Web API には、要求 (GET、POST など) の HTTP メソッドに基づいた操作も選択します。 既定では、Web API コント ローラーのメソッド名の先頭の大文字と小文字を探します。 という名前のコント ローラー メソッドなど、 `PutCustomers` HTTP PUT 要求と一致します。

次の属性で、いずれかを持つメソッドを修飾することをこの規則を上書きできます。

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

次の例では、HTTP POST 要求に CreateBook メソッドをマップします。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

標準メソッドなど、他のすべての HTTP メソッドを使用して、 **AcceptVerbs**属性は、HTTP メソッドの一覧です。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>ルート プレフィックス

多くの場合、コント ローラーが同じプレフィックスで始まるすべてのルートです。 例:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

使用して、全体のコント ローラーの共通プレフィックスを設定することができます、 **[RoutePrefix]**属性。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

メソッドの属性をティルダ (~) を使用して、そのルート プレフィックスをオーバーライドします。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

ルート プレフィックスは、パラメーターを含めることができます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>ルート制約

ルート制約を使用して、ルート テンプレートでは、パラメーターの照合方法を制限できます。 一般的な構文は&quot;{パラメーター: 制約}&quot;です。 例:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

ここでは、最初のルートは場合にのみオン、 &quot;id&quot; URI のセグメントは整数です。 それ以外の場合、2 番目のルートが選択されます。

次の表は、サポートされている制約を一覧表示します。

| 制約 | 説明 | 例 |
| --- | --- | --- |
| アルファ | 一致の大文字またはラテン語アルファベットの小文字 (a ~ z、A ~ Z) | {x:alpha} |
| bool | ブール値と一致します。 | {x:bool} |
| datetime | 一致する、 **DateTime**値。 | {x:datetime} |
| decimal | 10 進値と一致します。 | {x:decimal} |
| double | 64 ビットの浮動小数点値に一致します。 | {x:double} |
| float | 32 ビット浮動小数点値に一致します。 | {x:float} |
| guid | GUID 値に一致します。 | {x:guid} |
| int | 32 ビット整数値に一致します。 | {x:int} |
| 長さ | 指定した長さで、または、指定された範囲の長さの文字列と一致します。 | {x:length(6)} {x:length(1,20)} |
| long | 64 ビット整数値に一致します。 | {x:long} |
| max | 整数で最大値と一致します。 | {x:max(10)} |
| maxlength | 最大長を持つ文字列と一致します。 | {x:maxlength(10)} |
| 分 | 整数、最小値と一致します。 | {x:min(10)} |
| minlength | 最小長の文字列と一致します。 | {x:minlength(10)} |
| range | 整数値の範囲内に一致します。 | {x:range(10,50)} |
| regex | 正規表現と一致します。 | {x:regex(^\d{3}-\d{3}-\d{4}$)} |

通知、制約の一部など&quot;min&quot;かっこで囲まれた引数を受け取ります。 コロンで区切られた、パラメーターには、複数の制約を適用できます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>カスタム ルートの制約

実装することによってカスタム ルート制約を作成したり、 **IHttpRouteConstraint**インターフェイスです。 たとえば、次の制約は、0 以外の整数値にパラメーターを制限します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

次のコードは、制約を登録する方法を示しています。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

これで、ルートで制約を適用できます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

全体を置換することもできます。 **DefaultInlineConstraintResolver**クラスを実装することによって、 **IInlineConstraintResolver**インターフェイスです。 これはすべて置換、組み込みの制約の場合を除きの実装**IInlineConstraintResolver**具体的にはそれらを追加します。

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>省略可能な URI パラメーターと既定値

ルートのパラメーターに疑問符 () を追加することで行うと、URI パラメーターが省略可能です。 ルート パラメーターが省略可能な場合は、メソッドのパラメーターの既定値を定義する必要があります。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

この例では`/api/books/locale/1033`と`/api/books/locale`同じリソースを返します。

または、次のようにルート テンプレート内の既定値を指定することができます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

これは、前の例とほぼ同じですが、既定値を適用するときの動作のわずかな違いがあります。

- ("{Lcid?}") 最初の例では、パラメーターがこの正確な値を反映するため、メソッド パラメーターに直接 1033 の既定値が割り当てられます。
- 2 番目の例 ("{lcid 1033 の =}")、「1033」の既定値が、モデル バインディング プロセスを通過します。 既定のモデル バインダーは、対応する数値 1033 に「1033」に変換されます。 ただし、別の処理を実行するカスタム モデル バインダーにプラグインする可能性があります。

(ほとんどの場合、パイプラインにカスタム モデル バインダーを持っていない場合、2 つの形式と同じになります。)

<a id="route-names"></a>
## <a name="route-names"></a>ルート名

Web API では、すべてのルートは、名を持ちます。 ルート名を HTTP 応答でのリンクを含めることができるように、リンクの生成に役立ちます。

ルート名を指定するには、設定、**名前**属性のプロパティです。 次の例では、リンクを生成するときに、ルート名を使用する方法とルート名を設定する方法を示します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>ルートの順序

フレームワークは、ルートを持つ URI と照合するときは、特定の順序でルートを評価します。 順序を指定するには、設定、 **RouteOrder**ルート属性のプロパティです。 下位の値は、最初に評価されます。 既定の順序の値は 0 です。

次に、完全な順序付けの決定方法を示します。

1. 比較、 **RouteOrder**ルート属性のプロパティです。
2. ルート テンプレート内の各 URI セグメントを見てください。 各セグメントのとおりの順序します。 

    1. リテラルのセグメント。
    2. ルート制約を含むパラメーター。
    3. 制約なしのルートのパラメーターです。
    4. 制約のワイルドカード パラメーター セグメント。
    5. 制約なしのワイルドカード パラメーター セグメント。
3. 同数の場合の場合は、ルートは、大文字と小文字の序数に基づく文字列比較で順序付け ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) のルート テンプレート。

次に例を示します。 次のコント ローラーを定義するとします。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

これらのルートの順序が次のとおりです。

1. orders/details
2. orders/{id}
3. orders/{customerName}
4. orders/{\*date}
5. orders/保留中

その"details"がリテラル セグメントと"{id}"、前に表示されますが、「保留中」が表示される最後ために注意してください、 **RouteOrder**プロパティは 1 です。 (この例は、customers という名前の「詳細」または「保留中」です。 一般に、あいまいなルートを避けるために再試行してください。 この例では、優れたルート テンプレートで`GetByCustomer`は"顧客/{customerName}")
