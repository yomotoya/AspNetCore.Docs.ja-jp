---
title: "ASP.NET Core MVC での応答データを書式設定"
author: ardalis
description: "ASP.NET Core MVC での応答データを書式設定する方法を説明します。"
keywords: "ASP.NET Core、応答データ、IOutputFormatter、IActionResult"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: c056df45-d013-4826-91a1-4a092bae1ea5
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/formatting
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: abc125a093ff2cd5a38a537ecdfc795ff03e23f7
ms.sourcegitcommit: d1d8071d4093bf2444b5ae19d6e45c3d187e338b
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/19/2017
---
# <a name="introduction-to-formatting-response-data-in-aspnet-core-mvc"></a>ASP.NET Core MVC での応答データの書式設定の概要

によって[Steve Smith](https://ardalis.com/)

ASP.NET Core MVC には、クライアントの仕様に応答したり固定形式を使用して、応答データを書式設定するための組み込みサポートがあります。

[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/formatting/sample)です。

## <a name="format-specific-action-results"></a>ファイル形式に固有のアクションの結果

などが特定の形式に固有のいくつかのアクション結果型は`JsonResult`と`ContentResult`です。 アクションは、特定の方法で常にフォーマットされている特定の結果を返すことができます。 たとえばを返す、`JsonResult`クライアント設定に関係なく、JSON 形式のデータが返されます。 同様を返す、 `ContentResult` (単に文字列を返すことが)、プレーン テキスト形式の文字列データが返されます。

> [!NOTE]
> アクションを特定の種類を返す必要はありません。MVC には、任意のオブジェクトの戻り値がサポートしています。 アクションを返す場合、`IActionResult`実装とコント ローラーから継承`Controller`開発者の選択肢の多くに対応する多数のヘルパー メソッドがあります。 オブジェクトを返すアクションからの結果は`IActionResult`、適切な型をシリアル化される`IOutputFormatter`実装します。

継承するコント ローラーから特定の形式でデータを返す、`Controller`基底クラス、組み込みのヘルパー メソッドを使用して`Json`を JSON を返すと`Content`のプレーン テキスト。 アクション メソッドは、特定の結果の型を返す必要があります (たとえば、 `JsonResult`) または`IActionResult`です。

JSON 形式のデータを返す:

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=21-26)]

このアクションから応答のサンプル:

![応答のコンテンツの種類を示す Microsoft edge 開発者ツールの [ネットワーク] タブは、アプリケーション/json です。](formatting/_static/json-response.png)

なお、応答のコンテンツの種類は`application/json`ネットワーク要求の一覧と、応答ヘッダーのセクションの両方が表示される。 要求ヘッダーのセクションの Accept ヘッダーで (ここでは、Microsoft Edge) ブラウザーで表示されるオプションの一覧にも注意してください。 現在の手法では、このヘッダーを無視します。その名実は後ほど説明します。

プレーン テキストの書式設定されたデータを返すには使用`ContentResult`と`Content`ヘルパー。

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=47-52)]

この操作からの応答:

![応答のコンテンツの種類を示す Microsoft edge 開発者ツールの [ネットワーク] タブは、テキスト/プレーンです。](formatting/_static/text-response.png)

この場合、`Content-Type`が返される`text/plain`です。 また、文字列の応答タイプだけを使用して同じ動作を実現できます。

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3,5&range=54-59)]

>[!TIP]
> 複数の重要な操作の種類またはオプション (たとえば、さまざまな HTTP ステータス コードが実行される操作の結果に基づく) を返す、必要に応じて`IActionResult`戻り値の型として。

## <a name="content-negotiation"></a>コンテンツ ネゴシエーション

コンテンツのネゴシエーション (*conneg*略して) クライアントを指定するときに発生、 [Accept ヘッダー](https://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html)です。 ASP.NET Core MVC で使用される既定の形式は、JSON です。 コンテンツ ネゴシエーションはによって実装`ObjectResult`です。 特定のアクション結果のヘルパー メソッドから返されるステータス コードにも組み込まれている (すべてに基づいて`ObjectResult`)。 モデルの種類 (データ転送の種類として定義したクラス) を取得することもでき、フレームワークは自動的にそれを囲みで、`ObjectResult`します。

次のアクション メソッドを使用して、`Ok`と`NotFound`ヘルパー メソッド。

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=8,10&range=28-38)]

ない場合は、別の形式が要求されたサーバーは要求された形式で返すことができます、JSON 形式の応答が返されます。 などのツールを使用する[Fiddler](http://www.telerik.com/fiddler)を Accept ヘッダーが含まれる要求を作成し、別の形式を指定します。 その場合、サーバーがある場合、*フォーマッタ*要求の形式で応答を生成することができます、結果が返されますクライアント優先形式です。

![手動で作成を示す fiddler コンソール アプリケーションまたは xml の Accept ヘッダー値を持つ要求を取得します。](formatting/_static/fiddler-composer.png)

上のスクリーン ショットでは、要求を生成する、Fiddler 作成ツールが使用されています指定`Accept: application/xml`です。 既定では、ASP.NET Core MVC のみをサポートしている JSON でも返される結果は JSON 形式も、別の形式を指定すると、します。 次のセクションの他のフォーマッタを追加する方法が表示されます。

コント ローラーのアクションはした poco から (Plain Old CLR Object) を返すことができる場合に ASP.NET MVC が自動的に作成、`ObjectResult`オブジェクトをラップします。 クライアントが形式のシリアル化されたオブジェクトを取得 (JSON 形式では、既定以外の場合は XML またはその他の形式を構成することができます)。 対象のオブジェクトが返された場合`null`、フレームワークは戻ります、`204 No Content`応答します。

オブジェクトの種類を取得します。

[!code-csharp[Main](./formatting/sample/Controllers/Api/AuthorsController.cs?highlight=3&range=40-45)]

サンプルでは、有効な作成者エイリアスの要求は、著者のデータを 200 OK の応答に表示されます。 無効なエイリアスの要求は、204 No Content 応答に表示されます。 XML および JSON 形式で応答を示すスクリーン ショットは、以下に示します。

### <a name="content-negotiation-process"></a>コンテンツ ネゴシエーションのプロセス

コンテンツ*ネゴシエーション*のみが配置の場合、`Accept`要求のヘッダーが表示されます。 要求には、accept ヘッダーが含まれている、フレームワークの優先順位で accept ヘッダーのメディアの種類を列挙しようし、します accept ヘッダーで指定された形式のいずれかで応答を生成できるフォーマッタを検出します。 フォーマッタが見つからない、クライアントの要求を満たすことができる場合に、フレームワークは、応答を生成する最初のフォーマッタを検索しよう (開発者でオプションを構成しない限り、`MvcOptions`を返す 406 Not Acceptable 代わりに)。 要求は、XML を指定、XML フォーマッタが構成されていない場合は、JSON フォーマッタが使用されます。 一般的には、フォーマッタが構成されていない場合は、要求された形式を提供することができますし、オブジェクトの書式を設定する最初のフォーマッタを使用します。 ヘッダーが指定されていない場合、返されるオブジェクトを処理できる最初のフォーマッタは、応答をシリアル化するために使用されます。 この場合、ネゴシエーションの取得の任意の場所がありません - サーバーには、どのような形式が使用されますが決定することです。

> [!NOTE]
> Accept ヘッダーが含まれている場合`*/*`、しない限り、ヘッダーは無視されます`RespectBrowserAcceptHeader`に設定されている場合は true`MvcOptions`です。

### <a name="browsers-and-content-negotiation"></a>ブラウザーとコンテンツ ネゴシエーション

一般的な API は、クライアントとは異なりを指定する傾向が web ブラウザー`Accept`をさまざまな形式と、ワイルドカードを含むヘッダー。 既定では、フレームワークは、ブラウザーから、要求が送信されたことが検出された場合は無視されます、`Accept`ヘッダーと戻り値の代わりに、アプリケーション内のコンテンツの構成済みの既定の形式 (JSON 設定しない場合)。 これにより、さまざまなブラウザーを使用して、Api を使用する場合より一貫した使用環境が提供します。

場合は、アプリケーションの記念ブラウザー accept ヘッダーを使用する場合、するこの MVC の構成の一部として設定して構成できます`RespectBrowserAcceptHeader`に`true`で、`ConfigureServices`メソッド*Startup.cs*です。

```csharp
services.AddMvc(options =>
{
    options.RespectBrowserAcceptHeader = true; // false by default
});
```

## <a name="configuring-formatters"></a>フォーマッタを構成します。

アプリケーションは、JSON の既定値を超える追加の形式をサポートする必要がある場合、は、NuGet パッケージを追加して、それらをサポートする MVC を構成することができます。 入力と出力に別のフォーマッタがあります。 入力のフォーマッタが使用[モデル バインド](model-binding.md); 出力フォーマッタが応答を書式設定に使用されます。 構成することも[カスタム フォーマッタ](../advanced/custom-formatters.md)です。

### <a name="adding-xml-format-support"></a>XML 形式のサポートを追加します。

XML が書式設定のサポートを追加するには、インストール、 `Microsoft.AspNetCore.Mvc.Formatters.Xml` NuGet パッケージです。

MVC の構成に、XmlSerializerFormatters を追加*Startup.cs*:

[!code-csharp[Main](./formatting/sample/Startup.cs?name=snippet1&highlight=2)]

代わりに、出力フォーマッタだけを追加することができます。

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlSerializerOutputFormatter());
});
```

これら 2 つの方法が使用して結果をシリアル化`System.Xml.Serialization.XmlSerializer`です。 使用することができます、したい場合、`System.Runtime.Serialization.DataContractSerializer`その関連付けられたフォーマッタを追加することで。

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.Add(new XmlDataContractSerializerOutputFormatter());
});
```

コント ローラーのメソッドが要求のに基づいて、適切な形式を返す必要があります XML が書式設定のサポートを追加したら、`Accept`ヘッダーがこの Fiddler を例に示します。

![Fiddler コンソール: Accept ヘッダーの値は、アプリケーションまたは xml 要求の生のタブを示しています。 応答の生のタブには、アプリケーションまたは xml の Content-type ヘッダーの値が表示されます。](formatting/_static/xml-response.png)

生の GET 要求が行われたインスペクター タブで確認できます、`Accept: application/xml`ヘッダーが設定されています。 応答のペインの表示、`Content-Type: application/xml`ヘッダー、および`Author`オブジェクトを XML にシリアル化されました。

指定する要求を変更する作成ツール タブを使用して`application/json`で、`Accept`ヘッダー。 要求を実行して、応答は、JSON として書式設定されます。

![Fiddler コンソール: Accept ヘッダーの値は application/json と要求の生のタブを示しています。 応答の生のタブには、アプリケーションおよび json の Content-type ヘッダーの値が表示されます。](formatting/_static/json-response-fiddler.png)

このスクリーン ショットでは、要求のヘッダーに設定を確認できます`Accept: application/json`応答を指定と同じとその`Content-Type`です。 `Author` JSON 形式で、応答の本文にオブジェクトが示すようにします。

### <a name="forcing-a-particular-format"></a>特定の形式の強制

特定の操作の応答形式を制限したい場合にすることができます、適用することができます、`[Produces]`フィルター。 `[Produces]`フィルターは、特定のアクション (またはコント ローラー) の応答形式を指定します。 などのほとんど[フィルター](../controllers/filters.md)、これは、アクション、コント ローラー、またはグローバル スコープで適用できます。

```csharp
[Produces("application/json")]
public class AuthorsController
```

`[Produces]`フィルター内のすべてのアクションが強制的に実行、`AuthorsController`を JSON 形式の応答を返す、他のフォーマッタは、アプリケーションと指定されたクライアント用に構成された場合でも、`Accept`別、要求ヘッダー使用可能な形式です。 参照してください[フィルター](../controllers/filters.md)全体にフィルターを適用する方法を含む詳細についてはします。

### <a name="special-case-formatters"></a>特殊なケース フォーマッタ

特殊なケースは、組み込みのフォーマッタを使用して実装されます。 既定では、`string`戻り値の型として書式設定*テキスト/プレーン*(*テキスト/html*経由で要求された場合`Accept`ヘッダー)。 この動作を削除することで削除することができます、`TextOutputFormatter`です。 フォーマッタを削除する、`Configure`メソッド*Startup.cs* (下図参照)。 型を返す、204 コンテンツ応答がない場合を返すモデル オブジェクトがあるアクションを返す`null`です。 この動作を削除することで削除することができます、`HttpNoContentOutputFormatter`です。 次のコードを削除、`TextOutputFormatter`と`HttpNoContentOutputFormatter`です。

```csharp
services.AddMvc(options =>
{
    options.OutputFormatters.RemoveType<TextOutputFormatter>();
    options.OutputFormatters.RemoveType<HttpNoContentOutputFormatter>();
});
```

なし、 `TextOutputFormatter`、`string`型を返す 406 を返す例については、許容できません。 XML フォーマッタが存在する場合、これにフォーマットすることに注意してください`string`型を返す場合、`TextOutputFormatter`を削除します。

なし、 `HttpNoContentOutputFormatter`、null オブジェクトは構成済みのフォーマッタを使用して書式設定します。 たとえば、JSON フォーマッタが返されるだけ応答の本体を持つ`null`XML フォーマッタは、属性を持つ空の XML 要素を返すときに、`xsi:nil="true"`を設定します。

## <a name="response-format-url-mappings"></a>応答の形式の URL マッピング

クライアントで要求できますを特定の形式、URL の一部としてなど、クエリ文字列またはまたは .xml または .json などの形式に固有のファイル拡張子を使用して、パスの一部です。 API を使用してルート上では、要求パスからのマッピングを指定してください。 例:

```csharp
[FormatFilter]
public class ProductsController
{
    [Route("[controller]/[action]/{id}.{format?}")]
    public Product GetById(int id)
```

このルートには、省略可能なファイル拡張子として指定する要求の形式ができるようにします。 `[FormatFilter]`属性の形式の値の存在を確認、`RouteData`し、応答の作成時に、応答形式を適切なフォーマッタにマップされます。

| ルート                      | フォーマッタ                          |
| -------------------------- | ---------------------------------- |
| `/products/GetById/5`      | 既定の出力フォーマッタ       |
| `/products/GetById/5.json` | (構成されている) 場合は、JSON フォーマッタ |
| `/products/GetById/5.xml`  | (構成されている) 場合は、XML フォーマッタ  |
