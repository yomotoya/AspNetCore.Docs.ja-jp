---
uid: web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
title: ASP.NET web API 2 OData アクションをサポートしている |Microsoft Docs
author: MikeWasson
description: Odata では、アクションは、エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 アクションの使用が含まれます実装しています.。
ms.author: aspnetcontent
ms.date: 02/25/2014
ms.assetid: 2d7b3aa2-aa47-4e6e-b0ce-3d65a1c6fe02
msc.legacyurl: /web-api/overview/odata-support-in-aspnet-web-api/odata-v3/odata-actions
msc.type: authoredcontent
ms.openlocfilehash: e02ab21b864e328fe6892a00e5d5aca3f88eb9a2
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37837773"
---
<a name="supporting-odata-actions-in-aspnet-web-api-2"></a>ASP.NET web API 2 OData アクションをサポートしています。
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

[完成したプロジェクトのダウンロード](http://code.msdn.microsoft.com/ASPNET-Web-API-OData-cecdb524)

> Odata では、*アクション*エンティティに対する CRUD 操作として簡単に定義されていないサーバー側の動作を追加する方法です。 アクションの使用は次のとおりです。
> 
> - 複雑なトランザクションを実装します。
> - 一度に複数のエンティティを操作します。
> - エンティティの特定のプロパティにのみ更新できるようにします。
> - エンティティで定義されていないサーバーに情報を送信します。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>このチュートリアルで使用されるソフトウェアのバージョン
> 
> 
> - Web API 2
> - OData バージョン 3
> - Entity Framework 6


## <a name="example-rating-a-product"></a>例: 製品の評価

この例ではユーザーが、製品を評価し、各製品の平均評価を公開できるようにします。 データベースでは、評価、製品にキーの一覧を格納します。

表す、Entity Framework での評価を使用するモデルを次に示します。

[!code-csharp[Main](odata-actions/samples/sample1.cs)]

投稿へのクライアントのたくはありませんが、 `ProductRating` 「評価」コレクションにオブジェクト。 直感的な評価は、製品のコレクションに関連付けと、クライアントは、だけ評価値を投稿する必要があります。

そのため、通常の CRUD 操作を使用する代わりに、製品のクライアントが呼び出すことができるアクションを定義します。 OData 用語では、アクションは*バインド*Product エンティティにします。

>アクションでは、サーバー上に副作用があります。 このため、HTTP POST 要求を使用してに呼び出されます。 アクションは、パラメーターと戻り値の型がサービス メタデータで説明されていることができます。 クライアントが要求本文でパラメーターを送信し、サーバーが応答本文で、戻り値を送信します。 「製品の単価」アクションを呼び出すには、クライアントで、次のような URI に POST を送信します。

[!code-console[Main](odata-actions/samples/sample2.cmd)]

POST 要求内のデータは、製品の評価だけです。

[!code-console[Main](odata-actions/samples/sample3.cmd)]

## <a name="declare-the-action-in-the-entity-data-model"></a>エンティティ データ モデルのアクションを宣言します。

Web API 構成には、entity data model (EDM) にアクションを追加します。

[!code-csharp[Main](odata-actions/samples/sample4.cs)]

このコードは、製品のエンティティで実行できるアクションとして"RateProduct"を定義します。 アクションが受け取ることも宣言、 **int**パラメーター「評価」という名前を返します、 **int**値。

## <a name="add-the-action-to-the-controller"></a>コント ローラーに、アクションを追加します。

"RateProduct"アクションは、Product エンティティにバインドされます。 アクションを実装するには、という名前のメソッドを追加`RateProduct`Products コント ローラーにします。

[!code-csharp[Main](odata-actions/samples/sample5.cs)]

メソッド名が、EDM 内のアクションの名前と一致することに注意してください。 メソッドでは、2 つのパラメーターがあります。

- *キー*: レートに製品のキー。
- *パラメーター*: アクション パラメーターの値のディクショナリ。

既定のルーティング規約を使用している場合、キー パラメーターを「キー」名前する必要があります。 必ず、 **[FromOdataUri]** 属性が示すようにします。 この属性では、Web API 要求 URI からキーを解析するときに、OData 構文の規則を使用するように指示します。

使用して、*パラメーター*アクション パラメーターを取得するためのディクショナリ。

[!code-csharp[Main](odata-actions/samples/sample6.cs)]

クライアントが適切で、アクション パラメーターを送信する場合の値の書式**ModelState.IsValid**は true。 その場合は、使用することができます、 **ODataActionParameters**パラメーターの値を取得するためのディクショナリ。 この例で、`RateProduct`アクションは、「評価」という名前の 1 つのパラメーターを受け取る。

## <a name="action-metadata"></a>アクションのメタデータ

サービス メタデータを表示するには、/odata/$ メタデータに、GET 要求を送信します。 宣言するメタデータの一部を次に示します、`RateProduct`アクション。

[!code-xml[Main](odata-actions/samples/sample7.xml)]

**FunctionImport**要素は、アクションを宣言します。 ほとんどのフィールドは、一目瞭然ですが、注目すべき 2 つの。

- **IsBindable**意味、アクションを少なくとも、ターゲット エンティティに呼び出すことが、時間の一部です。
- **IsAlwaysBindable**ターゲット エンティティに常に、操作を呼び出せることを意味します。

違いは、いくつかの操作は、クライアントで使用可能では常が、その他のアクションがエンティティの状態によって異なります。 たとえば、[購入] アクションを定義するとします。 在庫にあるアイテムのみ購入できます。 項目が在庫切れになった場合、クライアントはそのアクションを呼び出すことはできません。

EDM を定義するときに、**アクション**メソッドは、常にバインド可能なアクションを作成します。

[!code-csharp[Main](odata-actions/samples/sample8.cs?highlight=1)]

Not を常に、バインド可能なアクションについて説明します (とも呼ばれる*一時的な*アクション) このトピックで後述します。

## <a name="invoking-the-action"></a>アクションを呼び出す

これで、クライアントはこの操作を呼び出す方法を見てみましょう。 ID の製品への 2 の評価を付与する必要があると 4 を = です。 要求本文の JSON 形式を使用して、例要求メッセージを次に示します。

[!code-console[Main](odata-actions/samples/sample9.cmd)]

応答メッセージを次に示します。

[!code-console[Main](odata-actions/samples/sample10.cmd)]

## <a name="binding-an-action-to-an-entity-set"></a>エンティティ セットへのアクションのバインド

前の例では、アクションが 1 つのエンティティにバインドされて: クライアントは、1 つの製品を評価します。 アクションは、エンティティのコレクションにバインドすることもできます。 次の変更を加えるだけです。

EDM では、アクションを追加するエンティティの**コレクション**プロパティ。

[!code-csharp[Main](odata-actions/samples/sample11.cs?highlight=1)]

コント ローラー メソッドで省略、*キー*パラメーター。

[!code-csharp[Main](odata-actions/samples/sample12.cs)]

クライアントが、製品のエンティティ セットに対して操作を呼び出すようになりました。

[!code-console[Main](odata-actions/samples/sample13.cmd)]

## <a name="actions-with-collection-parameters"></a>コレクションのパラメーターを使って操作

アクションには、パラメーター値のコレクションを取得することができます。 EDM では、次のように使用します。 **CollectionParameter&lt;T&gt;** パラメーターを宣言します。

[!code-csharp[Main](odata-actions/samples/sample14.cs)]

これは、コレクションを受け取り「評価」という名前のパラメーターを宣言します。 **int**値。 パラメーター値の取得も、コント ローラー メソッドで、 **ODataActionParameters**オブジェクトが、値のようになりましたが、 **ICollection&lt;int&gt;** 値。

[!code-csharp[Main](odata-actions/samples/sample15.cs)]

## <a name="transient-actions"></a>一時的なアクション

"RateProduct"例では、アクションが常に使用できるように、製品の場所をユーザーが評価常にできます。 いくつかの操作は、エンティティの状態によって異なります。 たとえば、ビデオ レンタル サービスで「チェック アウト」アクションは常に使用できません。 (依存するビデオのコピーが使用できるかどうか。)この種類のアクションが呼び出されます、*一時的な*アクション。

一時的なアクションには、サービス メタデータで**IsAlwaysBindable**を false に等しい。 メタデータのようになりますので、実際には、既定値です。

[!code-xml[Main](odata-actions/samples/sample16.xml)]

ここのこれが重要な理由: サーバーが、アクションが使用可能な場合、クライアントに指示する必要がありますアクションが一時的である場合。 この機能を使用するには、エンティティのアクションへのリンクを含みます。 ムービーのエンティティの例を次に示します。

[!code-console[Main](odata-actions/samples/sample17.cmd)]

"#CheckOut"プロパティには、チェック アウトの操作へのリンクが含まれています。 アクションが使用できない場合、サーバーは、リンクを省略します。

EDM では一時的なアクションを宣言するには、呼び出し、 **TransientAction**メソッド。

[!code-csharp[Main](odata-actions/samples/sample18.cs)]

またを特定のエンティティのアクション リンクを返す関数を用意する必要があります。 この関数を呼び出すことによって設定**HasActionLink**します。 ラムダ式として関数を記述できます。

[!code-csharp[Main](odata-actions/samples/sample19.cs)]

アクションを使用できる場合、ラムダ式は、アクションにリンクを返します。 OData シリアライザーには、エンティティをシリアル化時に、このリンクが含まれます。 アクションが利用できない場合、関数を返します`null`します。

## <a name="additional-resources"></a>その他のリソース

[OData アクションのサンプル](http://aspnet.codeplex.com/sourcecontrol/latest#Samples/WebApi/OData/v3/ODataActionsSample/)
