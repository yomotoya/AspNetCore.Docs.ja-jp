---
uid: web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
title: 属性ルーティングでは、ASP.NET Web API 2 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: riande
ms.date: 01/20/2014
ms.assetid: 979d6c9f-0129-4e5b-ae56-4507b281b86d
msc.legacyurl: /web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2
msc.type: authoredcontent
ms.openlocfilehash: f13720c5e9de99fb4ae5b27a757c257cac881f89
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834847"
---
<a name="attribute-routing-in-aspnet-web-api-2"></a>ASP.NET Web API 2 で属性のルーティング
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

*ルーティング*Web API がアクションへの URI に一致します。 新しい型をサポートする web API 2 のルーティングと呼ばれる*属性ルーティング*します。 名前が示すようは、ルートを定義するのに属性を使用する属性ルーティングします。 属性ルーティングでは、Uri の制御、web API でします。 たとえば、リソースの階層について説明する Uri を簡単に作成することができます。

規約ベースと呼ばれる、ルーティングの以前のスタイルを引き続き完全にサポートは、ルーティングします。 実際には、同じプロジェクト内の両方の手法を組み合わせることができます。

このトピックでは、属性ルーティングを有効にする方法を示していて、属性ルーティングのさまざまなオプションについて説明します。 属性ルーティングを使用するエンド ツー エンド チュートリアルでは、次を参照してください。[属性ルーティングで Web API 2 で REST API の作成](create-a-rest-api-with-attribute-routing.md)です。


## <a name="prerequisites"></a>必須コンポーネント

[Visual Studio 2017](https://www.visualstudio.com/vs/) Community、Professional、または Enterprise Edition

または、NuGet パッケージ マネージャーを使用して、必要なパッケージをインストールします。 **ツール** メニューの選択 Visual Studio で**ライブラリ パッケージ マネージャー**を選択し、**パッケージ マネージャー コンソール**します。 パッケージ マネージャー コンソール ウィンドウで、次のコマンドを入力します。

`Install-Package Microsoft.AspNet.WebApi.WebHost`

<a id="why"></a>
## <a name="why-attribute-routing"></a>なぜ属性ルーティングでしょうか。

使用する Web API の最初のリリース*規則に基づく*ルーティングします。 その型のルーティングでは、いずれかを定義するかは基本的に、複数のルート テンプレートには、文字列がパラメーター化されました。 フレームワークは、要求を受信したときに、ルート テンプレートに対して URI が一致します。 (規約ベースのルーティングの詳細については、次を参照してください。 [ASP.NET Web API におけるルーティング](routing-in-aspnet-web-api.md)します。

規約ベースのルーティングの利点の 1 つは、テンプレートが 1 つの場所で定義されていること、およびルーティング規則は、すべてのコント ローラー間で一貫して適用されます。 残念ながら、規約ベースのルーティングを困難に RESTful Api に共通している特定の URI パターンをサポートします。 多くの場合、子リソースをなどのリソース: 顧客の注文がある、映画アクター、書籍、著者ありなど。 これらの関係を反映する Uri を作成する自然です。

`/customers/1/orders`

この種類の URI では、規則ベースのルーティングを使用して作成を困難です。 これを行うことができます、結果は、多くのコント ローラーやリソースの種類がある場合にもスケールはありません。

属性ルーティングは、この URI のルートを定義も簡単です。 コント ローラー アクションに属性を追加するだけです。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample1.cs)]

属性ルーティングで簡単に他のいくつかのパターンを次に示します。

**API のバージョン管理**

この例では、「/api/v1/製品」より別のコント ローラーにルーティング「/api/v2/製品」になります。

`/api/v1/products`  
`/api/v2/products`

**オーバー ロードされた URI セグメント**

この例では、「1」は、注文番号がコレクションに「保留中」にマップされます。

`/orders/1`  
`/orders/pending`

**複数のパラメーター型**

この例では、「1」は、注文番号が「2013/06/16」を日付を指定します。

`/orders/1`  
`/orders/2013/06/16`

<a id="enable"></a>
## <a name="enabling-attribute-routing"></a>属性ルーティングを有効にします。

属性ルーティングを有効にするのには、呼び出す**MapHttpAttributeRoutes**構成中にします。 この拡張メソッドが定義されている、 **System.Web.Http.HttpConfigurationExtensions**クラス。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample2.cs)]

属性のルーティングを組み合わせる[規則に基づく](routing-in-aspnet-web-api.md)ルーティングします。 規約ベースのルーティングを定義するには、呼び出し、 **MapHttpRoute**メソッド。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample3.cs)]

Web API を構成する方法の詳細については、次を参照してください。 [ASP.NET Web API 2 の構成](../advanced/configuring-aspnet-web-api.md)します。

<a id="config"></a>
### <a name="note-migrating-from-web-api-1"></a>注: は、Web API 1 から移行します。

Web API 2 では、前に、Web API プロジェクト テンプレートには、このようなコードが生成されます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample4.cs)]

属性ルーティングを有効にすると、このコードで例外がスローされます。 属性ルーティングを使用する既存の Web API プロジェクトをアップグレードする場合は、次にこの構成コードを更新することを確認してください。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample5.cs?highlight=4)]

> [!NOTE]
> 詳細については、次を参照してください。 [ASP.NET ホストによる Web API を構成する](../advanced/configuring-aspnet-web-api.md#webhost)します。


<a id="add-routes"></a>
## <a name="adding-route-attributes"></a>ルート属性を追加します。

属性を使用して定義されているルートの例を次に示します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample6.cs)]

文字列&quot;顧客/{customerId}/orders&quot;はルートの URI テンプレートです。 Web API は、テンプレートには、要求 URI の一致を試みます。 この例では、"customers"と"orders"は、リテラルのセグメントと"{customerId}"が変数のパラメーター。 次の Uri には、このテンプレートは一致します。

- `http://localhost/customers/1/orders`
- `http://localhost/customers/bob/orders`
- `http://localhost/customers/1234-5678/orders`

使用して照合を制限する[制約](#constraints)、このトピックの後半で説明します。

注意、 &quot;{customerId}&quot;ルート テンプレートのパラメーターの名前に一致する、 *customerId*メソッドのパラメーター。 Web API コント ローラー アクションを呼び出すと、ルート パラメーターをバインドしようとします。 たとえば、URI は、 `http://example.com/customers/1/orders`、Web API が「1」の値をバインドするを試みます、 *customerId*アクションのパラメーター。

URI テンプレートでは、いくつかのパラメーターを持つことができます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample7.cs)]

ルート属性を持たないコント ローラーのメソッドは、規則ベースのルーティングを使用します。 これにより、同じプロジェクトでのルーティングの両方の種類を組み合わせることができます。

## <a name="http-methods"></a>HTTP メソッド

Web API には、(GET、POST など) の要求の HTTP メソッドに基づいてアクションも選択します。 既定では、Web API コント ローラーのメソッド名の先頭の大文字と小文字を探します。 という名前のコント ローラー メソッドなど、 `PutCustomers` HTTP PUT 要求と一致します。

次の属性のいずれかのメソッドを修飾することでこの規則をオーバーライドできます。

- **[HttpDelete]**
- **[HttpGet]**
- **[HttpHead]**
- **[HttpOptions]**
- **[HttpPatch]**
- **[HttpPost]**
- **[HttpPut]**

次の例では、HTTP POST 要求に CreateBook メソッドをマップします。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample8.cs)]

標準以外の方法を含む、他のすべての HTTP メソッドを使用して、 **AcceptVerbs**属性には、HTTP メソッドのリストを取得します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample9.cs)]

<a id="prefixes"></a>
## <a name="route-prefixes"></a>ルート プレフィックス

多くの場合、同じプレフィックスで始まるすべてのコント ローラーにルーティングされます。 例えば:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample10.cs)]

使用してコント ローラー全体の共通のプレフィックスを設定することができます、 **[RoutePrefix]** 属性。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample11.cs)]

メソッドの属性をティルダ (~) を使用して、ルート プレフィックスをオーバーライドします。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample12.cs)]

ルートのプレフィックスは、パラメーターを含めることができます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample13.cs)]

<a id="constraints"></a>
## <a name="route-constraints"></a>ルート制約

ルート制約では、ルート テンプレートでパラメーターを照合する方法を制限できます。 一般的な構文は&quot;{0} パラメーター: 制約}&quot;します。 例えば:

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample14.cs)]

ここでは、最初のルートは場合にのみオン、 &quot;id&quot; URI のセグメントは、整数です。 それ以外の場合、2 つ目のルートが選択されます。

次の表は、サポートされている制約を一覧表示します。

| 制約 | 説明 | 例 |
| --- | --- | --- |
| アルファ | 一致の大文字または小文字の英数字 (a ~ z、A ~ Z) | {アルファ: x} |
| bool | ブール値と一致します。 | {x:bool} |
| datetime | 一致する**DateTime**値。 | {x:datetime} |
| decimal | 10 進値と一致します。 | {x:decimal} |
| double | 64 ビットの浮動小数点値と一致します。 | {x:double} |
| float | 32 ビットの浮動小数点値と一致します。 | {x:float} |
| guid | GUID 値と一致します。 | {x: guid} |
| int | 32 ビット整数値と一致します。 | {x:int} |
| 長さ | 指定した長さで、または長さの指定した範囲内の文字列と一致します。 | {x: length(6)}{x: length(1,20)} |
| long | 64 ビット整数値と一致します。 | {x:long} |
| max | 最大値は整数に一致します。 | {x:max(10)} |
| maxlength | 最大長を持つ文字列と一致します。 | {x:maxlength(10)} |
| 分 | 整数値を最小値と一致します。 | {x: min(10)} |
| minlength | 最小長の文字列と一致します。 | {x: minlength(10)} |
| range | 値の範囲内の整数に一致します。 | {x:range(10,50)} |
| regex | 正規表現と一致します。 | {x: regex(^\d{3}-\d{3}-\d{4}$)} |

通知、制約の一部など&quot;min&quot;かっこで囲まれた引数を受け取ります。 コロンで区切られた、パラメーターには、複数の制約を適用できます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample15.cs)]

### <a name="custom-route-constraints"></a>カスタム ルート制約

実装することによってカスタム ルート制約を作成することができます、 **IHttpRouteConstraint**インターフェイス。 たとえば、次の制約は、0 以外の整数値にパラメーターを制限します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample16.cs)]

次のコードでは、制約を登録する方法を示します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample17.cs)]

これで、ルートで制約を適用できます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample18.cs)]

全体を置換することもできます。 **DefaultInlineConstraintResolver**クラスを実装することによって、 **IInlineConstraintResolver**インターフェイス。 そうはすべての組み込みの制約の場合を除いて置き換えるの実装**IInlineConstraintResolver**具体的にはそれらを追加します。

<a id="optional"></a>
## <a name="optional-uri-parameters-and-default-values"></a>省略可能な URI パラメーターと既定値

ルート パラメーターに疑問符を追加することで行うと、URI パラメーターが省略可能です。 ルート パラメーターが省略可能な場合は、メソッド パラメーターの既定値を定義する必要があります。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample19.cs)]

この例で`/api/books/locale/1033`と`/api/books/locale`同じリソースを返します。

または、次のように、ルート テンプレート内の既定値を指定できます。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample20.cs)]

これは、前の例とほぼ同じですが、既定値が適用される動作のわずかな違いがあります。

- 最初の例 ("{lcid?}") で、パラメーターがこの正確な値を反映するため、メソッド パラメーターに直接 1033 の既定値が割り当てられます。
- 2 番目の例 ("{lcid 1033 の =}")、「1033」の既定値は、モデル バインディング プロセスをたどります。 既定のモデル バインダーは、数値の値 1033 に「1033」に変換されます。 ただし、別のものを実行するカスタム モデル バインダーを接続する可能性があります。

(ほとんどの場合、パイプラインでカスタム モデル バインダーがない限り、2 つの形式と同じになります。)

<a id="route-names"></a>
## <a name="route-names"></a>ルート名

Web api では、すべてのルート名を持ちます。 ルート名は、HTTP 応答でのリンクを含めることができるように、リンクを生成するのに役立ちます。

ルート名を指定するには、設定、**名前**属性のプロパティ。 次の例では、リンクを生成するときに、ルート名を使用する方法と、ルート名を設定する方法を示します。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample21.cs)]

<a id="order"></a>
## <a name="route-order"></a>ルートの順序

フレームワークでは、ルートの URI と照合しようとして、特定の順序でルートを評価します。 順序を指定するには、設定、 **RouteOrder**ルート属性のプロパティ。 下位の値は、最初に評価されます。 既定の順序の値には 0 です。

合計の順序を決定する方法を次に示します。

1. 比較、 **RouteOrder**ルート属性のプロパティ。
2. ルート テンプレートでは、各 URI セグメントを確認します。 セグメントごとに次のように順序します。 

    1. リテラルのセグメント。
    2. ルート制約を持つパラメーター。
    3. ルート制約なしのパラメーター。
    4. 制約のワイルドカード パラメーター セグメント。
    5. 制約なしのワイルドカード パラメーター セグメント。
3. 同数の場合の場合は、ルートは大文字の序数の文字列の比較によって並べ替えられます ([OrdinalIgnoreCase](https://msdn.microsoft.com/library/system.stringcomparer.ordinalignorecase.aspx)) のルート テンプレート。

次に例を示します。 次のコント ローラーを定義するとします。

[!code-csharp[Main](attribute-routing-in-web-api-2/samples/sample22.cs)]

これらのルートの順序が次のとおりです。

1. orders/詳細
2. orders/{id}
3. orders/{customerName}
4. orders/{\*date}
5. orders/保留中

「詳細」リテラルのセグメントは、および"{id}"、前に表示されますが「保留中」が表示されます最後ため、 **RouteOrder**プロパティが 1 にします。 (この例ではお客様が指定されていない"details"が「保留中」またはします。 一般に、あいまいなルートを回避するためにお試しください。 この例では、より優れたルート テンプレートで`GetByCustomer`は、"顧客/{customerName}")
