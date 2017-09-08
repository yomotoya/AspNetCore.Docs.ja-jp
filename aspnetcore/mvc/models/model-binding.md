---
title: "モデル バインディング"
author: rick-anderson
description: 
keywords: ASP.NET Core
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: b355a48e-a15c-4d58-b69c-899763613a97
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/model-binding
ms.openlocfilehash: 930ea062ffb914cbd4f1500308b813167c1f601b
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/11/2017
---
# <a name="model-binding"></a>モデル バインディング

によって[Rachel Appel](http://github.com/rachelappel)

## <a name="introduction-to-model-binding"></a>モデル バインディングの概要

ASP.NET Core mvc モデル バインディングでは、アクション メソッド パラメーターに HTTP 要求からデータをマップします。 パラメーターが文字列、整数、浮動小数点数などの単純型か複合型があります。 これは、データのサイズまたは複雑さに関係なく、多くの場合、繰り返しのシナリオには、対応する受信データにマッピングするための MVC の優れた機能です。 MVC では、開発者のすべてのアプリで同じコードを若干異なるバージョンの再書き換えを保持する必要はありませんので、バインドを取り除くによってこの問題を解決します。 コンバーター コードを入力するテキストの書き込みは時間がかかり、およびエラーが発生します。

## <a name="how-model-binding-works"></a>モデル バインディングのしくみ

HTTP 要求を受信すると、MVC コント ローラーの特定のアクション メソッドにルーティングします。 ルート データはに基づいて実行するアクション メソッドが決定し、そのアクション メソッドのパラメーターに HTTP 要求からの値をバインドします。 たとえば、次の URL を考慮してください。

`http://contoso.com/movies/edit/2`

ルート テンプレートは、次のように見えるため`{controller=Home}/{action=Index}/{id?}`、`movies/edit/2`にルーティング、`Movies`コント ローラーとその`Edit`アクション メソッド。 呼ばれるオプションのパラメーターも受け入れます`id`です。 アクション メソッドのコードは、次のようになります。

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public IActionResult Edit(int? id)
   ```

メモ: URL ルート内の文字列は、大文字小文字を区別できません。

MVC は要求データをアクション パラメーターを名前でバインドしようとします。 MVC は、パラメーター名とパブリック、設定可能なプロパティの名前を使用して各パラメーターの値を探します。 上記の例でのみアクション パラメーターの名前は`id`MVC は、ルートの値に同じ名前の値にバインドします。 ルートの値だけでなく MVC は、要求のさまざまな部分からデータをバインドは、セットの順序で。 モデル バインディングがそれらを検索する順序でデータ ソースの一覧を次に示します。

1. `Form values`: これらは、POST メソッドを使用して HTTP 要求のフォーム値です。 (jQuery POST 要求を含む)。

2. `Route values`: 一連のルートの値によって提供される[ルーティング](../../fundamentals/routing.md)

3. `Query strings`: クエリ文字列の一部の URI。

<!-- DocFX BUG
The link works but generates an error when building with DocFX
@fundamentals/routing
[Routing](xref:fundamentals/routing)
-->

注: フォームの値、ルート データ、およびクエリ文字列はすべての名前と値のペアとして格納します。

モデル バインディングがという名前のキーを求められた後`id`nothing を使用する必要があるとという名前`id`フォーム値のに移動されたルート値をそのキーを探しています。 この例では、一致するものを勧めします。 バインディングが発生し、値が 2 の整数に変換します。 編集 (文字列 id) を使用して要求を同じには「2」の文字列に変換します。

これまでの例は、単純型を使用します。 MVC では、単純型は、任意の .NET プリミティブ型または文字列型コンバーターを使用した型をします。 アクション メソッドのパラメーターがクラスをなどがあった場合、`Movie`型で、MVC のモデル バインディングは、プロパティは、それを適切に処理を両方単純型と複合型を含んでいます。 一致を探して複合型のプロパティを走査するのにリフレクションと再帰を使用します。 モデル バインディングが、パターンを探して*parameter_name.property_name*プロパティに値をバインドします。 このフォームの一致する値が見つからない場合は、プロパティ名だけを使用してバインドを試みます。 などの種類に対して`Collection`型、モデル バインディングで一致検索を*parameter_name [index]*または*[index]*です。 モデル バインディング扱います`Dictionary`を求める同様に、型*parameter_name [キー]*または*[キー]*キーは、単純型限り、します。 サポートされているキーは、HTML のフィールド名と同じモデルの種類に対して生成されるタグ ヘルパーと一致します。 維持されるようはフォームのフィールドの利便性のため、ユーザーの入力が入力など、ときに、作成または編集からバインドされたデータの検証に合格しませんでしたラウンド トリップの値が有効にします。

バインディングが発生するためにクラスにパブリックの既定のコンス トラクターが必要し、バインドされるメンバーはパブリックの書き込み可能なプロパティである必要があります。 クラスはパブリックの既定のコンス トラクターを使用するインスタンスのみをモデル バインディングが発生したときにプロパティを設定することができます。

パラメーターがバインドされている場合は、モデル バインディングがその名前を持つ値を探してを停止し、次のパラメーターをバインドする上を移動します。 バインドが失敗する、MVC では、エラーはスローされません。 照会できますモデル状態エラーをチェックして、`ModelState.IsValid`プロパティです。

注意: コント ローラーのエントリの各`ModelState`プロパティは、`ModelStateEntry`を含む、`Errors property`です。 このコレクションを照会することはほとんどありません必要があります。 代わりに、 `ModelState.IsValid` を使用してください。

また、モデル バインディングを実行するときに、MVC は考慮する必要がありますのあるいくつかの特別なデータ型があります。

* `IFormFile`、 `IEnumerable<IFormFile>`: HTTP 要求の一部である 1 つまたは複数のアップロードされたファイルです。

* `CancelationToken`: 非同期コント ローラーでアクティビティをキャンセルするために使用します。

これらの型は、クラス型またはプロパティをアクション パラメーターにバインドできます。

モデル バインディングが完了したら、[検証](validation.md)に発生します。 モデル バインディングの既定では、膨大な開発シナ リオの優れたは動作します。 独自のニーズがある場合は、組み込みのビヘイビアーをカスタマイズするためにも拡張可能です。

## <a name="customize-model-binding-behavior-with-attributes"></a>属性を持つモデル バインディング動作をカスタマイズします。

MVC には、別のソースにその既定のモデル バインディング動作を送信するために使用できるいくつかの属性が含まれています。 たとえば、バインディングのプロパティが必要なかどうか、またはを使用してすべての発生する必要がありますしない場合を指定できます、`[BindRequired]`または`[BindNever]`属性。 または、既定のデータ ソースを上書きし、モデル バインダーのデータ ソースを指定できます。 以下には、モデル バインディング属性の一覧を示します。

* `[BindRequired]`: この属性は、バインドできませんが発生した場合、モデル状態エラーを追加します。

* `[BindNever]`: このパラメーターにバインドしないモデル バインダーに指示します。

* `[FromHeader]`、 `[FromQuery]`、 `[FromRoute]`、 `[FromForm]`: これらを使用して適用する正確なバインディング ソースを指定します。

* `[FromServices]`: この属性を使用して[依存性の注入](../../fundamentals/dependency-injection.md)services からパラメーターをバインドします。

* `[FromBody]`: 要求の本文からデータをバインドに構成されたフォーマッタを使用します。 フォーマッタが要求のコンテンツの種類に基づいて選択されます。

* `[ModelBinder]`: 既定のモデル バインダー、バインディング ソース名を変更するために使用します。

属性は、モデル バインディングの既定の動作をオーバーライドする必要がある場合に非常に便利なツールです。

## <a name="binding-formatted-data-from-the-request-body"></a>バインディングから書式付きデータ要求の本文

要求データは、さまざまな JSON、XML、およびその他の多くを含む形式で取得できます。 要求本文内のデータにパラメーターをバインドすることを示すために、[FromBody] 属性を使用する場合、MVC は、コンテンツの種類に基づく要求データを処理する構成済みのフォーマッタのセットを使用します。 既定では、MVC が含まれます、`JsonInputFormatter`クラス XML とその他のカスタム形式の処理に関するその他のフォーマッタを追加できますが JSON データを処理します。

> [!NOTE]
> ある多くてで修飾されたアクション パラメーターを 1 つ`[FromBody]`です。 ASP.NET Core MVC の実行時では、フォーマッタに要求ストリームを読み取る責任を委任します。 要求ストリームは、パラメーターの読み取りは後、は一般に他のバインド用にもう一度要求ストリームを読み取る`[FromBody]`パラメーター。

> [!NOTE]
> `JsonInputFormatter`は、既定のフォーマッタとに基づいて[Json.NET](http://www.newtonsoft.com/json)です。

ASP.NET 選択に基づいて入力フォーマッタ、[コンテンツの種類](https://www.w3.org/Protocols/rfc1341/4_Content-Type.html)ヘッダーおよびパラメーターの型には、それ以外の場合を指定して適用する属性がない限り、します。 XML を使用するか、または別の形式をする必要があります構成で、 *Startup.cs*への参照を取得するファイルの最初が`Microsoft.AspNetCore.Mvc.Formatters.Xml`NuGet を使用します。 スタートアップ コードは、次のようになります。

<!-- literal_block {"ids": [], "linenos": true, "xml:space": "preserve", "language": "csharp"} -->

```csharp
public void ConfigureServices(IServiceCollection services)
   {
       services.AddMvc()
          .AddXmlSerializerFormatters();
   }
   ```

内のコード、 *Startup.cs*ファイルが含まれています、`ConfigureServices`メソッドを`services`ASP.NET アプリのサービスの構築に使用できる引数。 サンプルでは、MVC は、このアプリに提供するサービスとして XML フォーマッタを追加おされます。 `options`に渡される引数、`AddMvc`メソッドでは、追加およびアプリの起動時に、MVC のフィルター、フォーマッタ、およびその他のシステム オプションを管理することができます。 適用し、`Consumes`属性をコント ローラー クラスまたは希望の形式を使用するアクション メソッド。

### <a name="custom-model-binding"></a>カスタム モデル バインディング

モデル バインディングを拡張するには、独自のカスタム モデル バインダーを記述します。 詳細については[カスタム モデル バインディング](../advanced/custom-model-binding.md)です。
