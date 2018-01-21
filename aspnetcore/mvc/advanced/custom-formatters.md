---
title: "ASP.NET Core MVC web Api でカスタム フォーマッタ"
author: tdykstra
description: "作成して、web Api ASP.NET Core でのカスタム フォーマッタを使用する方法を説明します。"
ms.author: tdykstra
manager: wpickett
ms.date: 02/08/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/custom-formatters
ms.openlocfilehash: 3a6474fdae29b170978226de74d523b20a16cd0c
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="custom-formatters-in-aspnet-core-mvc-web-apis"></a>ASP.NET Core MVC web Api でカスタム フォーマッタ

によって[Tom Dykstra](https://github.com/tdykstra)

ASP.NET Core MVC には、JSON、XML、またはプレーン テキスト形式を使用してで web Api のデータ交換の組み込みサポートがあります。 この記事では、カスタム フォーマッタを作成することで追加の形式のサポートを追加する方法を示します。

[GitHub からサンプルのダウンロードを表示または](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample)です。

## <a name="when-to-use-custom-formatters"></a>カスタム フォーマッタを使用する場合

カスタム フォーマッタを使用すると、[コンテンツ ネゴシエーション](xref:mvc/models/formatting)組み込みフォーマッタ (JSON、XML、およびプレーン テキスト) でサポートされていないコンテンツの種類をサポートするために処理します。

たとえば、一部の web API のクライアントを処理できる場合、 [Protobuf](https://github.com/google/protobuf)より効率的になっているために、それらのクライアントで Protobuf を使用するとする可能性があります形式です。  連絡先の名前とアドレスを送信する web API をすることもできます[vCard](https://wikipedia.org/wiki/VCard)形式、連絡先データをやり取りするための一般的に使用される形式です。 この記事の付属のサンプル アプリでは、単純な vCard フォーマッタを実装します。

## <a name="overview-of-how-to-use-a-custom-formatter"></a>カスタム フォーマッタを使用する方法の概要

作成し、カスタム フォーマッタを使用する手順を次に示します。

* クライアントに送信するデータをシリアル化する場合は、出力フォーマッタ クラスを作成します。
* クライアントから受信したデータを逆シリアル化する場合は、入力のフォーマッタ クラスを作成します。 
* フォーマッタのインスタンスを追加、`InputFormatters`と`OutputFormatters`コレクション[MvcOptions](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.mvcoptions)です。

次のセクションでは、これらの各手順のガイダンスと、コード例を提供します。

## <a name="how-to-create-a-custom-formatter-class"></a>カスタム フォーマッタ クラスを作成する方法

フォーマッタを作成します。

* 適切な基本クラスからクラスを派生します。
* コンス トラクターでは、有効なメディアの種類とエンコーディングを指定します。
* オーバーライド`CanReadType` / `CanWriteType`メソッド
* オーバーライド`ReadRequestBodyAsync` / `WriteResponseBodyAsync`メソッド
  
### <a name="derive-from-the-appropriate-base-class"></a>適切な基本クラスから派生します。

テキストのメディアの種類 (たとえば、vCard) から取得、 [TextInputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textinputformatter)または[TextOutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.textoutputformatter)基本クラスです。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=classdef)]

派生してバイナリ型の場合、 [InputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.inputformatter)または[OutputFormatter](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformatter)基本クラスです。

### <a name="specify-valid-media-types-and-encodings"></a>有効なメディアの種類とエンコーディングを指定します。

、そのコンス トラクターを追加することで有効なメディアの種類とエンコーディングを指定、`SupportedMediaTypes`と`SupportedEncodings`コレクション。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=ctor&highlight=3,5-6)]

> [!NOTE]  
> フォーマッタ クラスでコンス トラクターの依存関係の挿入を行うことはできません。 たとえば、コンス トラクターにロガーのパラメーターを追加することによって、ロガーを取得できません。 サービスにアクセスするには、メソッドに渡されるコンテキスト オブジェクトを使用する必要があります。 コード例[下](#read-write)これを行う方法を示します。

### <a name="override-canreadtypecanwritetype"></a>CanReadType/CanWriteType をオーバーライドします。 

逆シリアル化したり、オーバーライドすることからシリアル化の種類を指定、`CanReadType`または`CanWriteType`メソッドです。 たとえば、のみできますからテキストを vCard を作成する、`Contact`型およびその逆です。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=canwritetype)]

#### <a name="the-canwriteresult-method"></a>CanWriteResult メソッド

一部のシナリオでは、オーバーライドする必要がある`CanWriteResult`の代わりに`CanWriteType`です。 使用して`CanWriteResult`次の条件に該当する場合。

  * アクション メソッドでは、モデル クラスを返します。
  * 実行時に返される可能性がありますが、派生クラスがあります。
  * クラスには、アクションが返した派生する実行時に把握する必要があります。  

たとえば、アクション メソッドのシグネチャを返します、`Person`型であり、これが返す可能性があります、`Student`または`Instructor`から派生する型`Person`です。 場合は、フォーマッタのみを処理する`Student`、オブジェクトの型を調べて[オブジェクト](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.formatters.outputformattercanwritecontext#Microsoft_AspNetCore_Mvc_Formatters_OutputFormatterCanWriteContext_Object)に提供されたコンテキスト オブジェクトで、`CanWriteResult`メソッドです。 使用する必要はないことに注意してください`CanWriteResult`アクション メソッドが返す場合`IActionResult`以外の場合は、`CanWriteType`メソッドはランタイムの型を受け取ります。

<a id="read-write"></a>
### <a name="override-readrequestbodyasyncwriteresponsebodyasync"></a>ReadRequestBodyAsync/WriteResponseBodyAsync をオーバーライドします。 

逆シリアル化またはでシリアル化の実際の作業を行う`ReadRequestBodyAsync`または`WriteResponseBodyAsync`です。  次の例で強調表示された行は、(コンス トラクターのパラメーターから取得できません) 依存性の注入コンテナーからサービスを取得する方法を示しています。

[!code-csharp[Main](custom-formatters/sample/Formatters/VcardOutputFormatter.cs?name=writeresponse&highlight=3-4)]

## <a name="how-to-configure-mvc-to-use-a-custom-formatter"></a>カスタム フォーマッタを使用する MVC を構成する方法
 
カスタム フォーマッタを使用するフォーマッタ クラスのインスタンスを追加、`InputFormatters`または`OutputFormatters`コレクション。

[!code-csharp[Main](custom-formatters/sample/Startup.cs?name=mvcoptions&highlight=3-4)]

フォーマッタは、挿入した順序で評価されます。 最初の 1 つが優先されます。 

## <a name="next-steps"></a>次の手順

参照してください、[サンプル アプリケーション](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/advanced/custom-formatters/sample)を実装する単純な vCard 入力と出力のフォーマッタ。  アプリケーションは、次の例のように見える Vcard を読み書き。

```
BEGIN:VCARD
VERSION:2.1
N:Davolio;Nancy
FN:Nancy Davolio
UID:20293482-9240-4d68-b475-325df4a83728
END:VCARD
```

VCard 出力、アプリケーションを実行しを Get 要求と共に送信します Accept ヘッダー"テキスト/vcard"を表示する`http://localhost:63313/api/contacts/`(Visual Studio から実行している) 場合、または`http://localhost:5000/api/contacts/`(コマンドラインから実行している) 場合。

VCard をアドレス帳のメモリ内コレクションに追加するには、上記の例のように書式設定、本文にテキストを vCard およびコンテンツ タイプ ヘッダー"テキスト/vcard"とは、同じ URL に Post 要求を送信します。
