---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: データ注釈検証コントロールに (c#) を使用した検証 |Microsoft Docs
author: microsoft
description: ASP.NET MVC アプリケーション内の検証を実行するには、データ注釈モデル バインダーの活用します。 検証コントロールのさまざまな種類を使用する方法を説明してください.
ms.author: aspnetcontent
ms.date: 05/29/2009
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 47314f3989b90b1f5d59bda135ea1f9836f2be63
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812822"
---
<a name="validation-with-the-data-annotation-validators-c"></a>データ注釈検証コントロールに (c#) を使用した検証
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC アプリケーション内の検証を実行するには、データ注釈モデル バインダーの活用します。 検証属性の種類を使用して Microsoft Entity Framework に処理する方法について説明します。


このチュートリアルでは、データ注釈検証コントロールを使用して、ASP.NET MVC アプリケーションで検証を実行する方法について説明します。 データ注釈検証コントロールを使用する利点は、– などに必要な 1 つまたは複数の属性や StringLength 属性を追加するだけで、– クラス プロパティには、検証を実行することを有効にすることです。

データ注釈検証コントロールを使用するには、データ注釈のモデル バインダーをダウンロードする必要があります。 CodePlex の web サイトからデータ注釈のモデル バインダーのサンプルをダウンロードするにはクリックして[ここ](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)します。


データ注釈のモデル バインダーは、Microsoft ASP.NET MVC フレームワークの公式の一部ではないこと理解に重要です。 データ注釈のモデル バインダーが、Microsoft ASP.NET MVC チームによって作成されますが、データ注釈のモデル バインダーの公式の製品サポートの説明し、このチュートリアルで使用される Microsoft は提供されません。


## <a name="using-the-data-annotation-model-binder"></a>データ注釈のモデル バインダーを使用してください。

ASP.NET MVC アプリケーションでデータ注釈のモデル バインダーを使用するには、まず Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へのアセンブリへの参照を追加する必要があります。 メニュー オプションを選択**プロジェクトで、参照の追加**します。 次に、**参照**タブし、データ注釈モデル バインダーのサンプルのダウンロード (および解凍) どこの場所を参照 (を参照してください**図 1**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**図 1**: データの注釈のモデル バインダーへの参照を追加する ([フルサイズの画像を表示する をクリックします](validation-with-the-data-annotation-validators-cs/_static/image3.png))。

Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へのアセンブリの両方を選択し、クリックして、 **OK**ボタンをクリックします。


データ注釈のモデル バインダーでは、.NET Framework Service Pack 1 に含まれている system.componentmodel.dataannotations.dll へのアセンブリを使用することはできません。 データ注釈モデル バインダーのサンプル ダウンロードに含まれている system.componentmodel.dataannotations.dll へのアセンブリのバージョンを使用する必要があります。


最後に、Global.asax ファイルで、DataAnnotations のモデル バインダーを登録する必要があります。 アプリケーションに次のコード行を追加\_Start() イベント ハンドラーようにアプリケーション\_Start() メソッドは、次のようになります。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

次のコードでは、ASP.NET MVC アプリケーション全体の既定のモデル バインダーとして、ataAnnotationsModelBinder を登録します。

## <a name="using-the-data-annotation-validator-attributes"></a>データ注釈検証コントロールの属性を使用します。

データ注釈のモデル バインダーを使用する場合は、検証を実行する検証属性を使用します。 System.ComponentModel.DataAnnotations 名前空間には、次の検証属性が含まれています。

- 範囲は – プロパティの値が、指定した値の範囲があるかどうかを検証することができます。
- – 正規表現では、プロパティの値が指定した正規表現パターンに一致するかどうかを検証することができます。
- 必要な – 必要に応じてプロパティをマークすることができます。
- StringLength – では、文字列プロパティの最大長を指定することができます。
- 検証: すべての検証属性の基本クラス。

> [!NOTE] 
> 
> 標準の検証コントロールのいずれかによって、検証のニーズが満たされない場合、常がある新しい検証コントロールの属性ベースの検証属性から継承することによって、カスタム検証属性を作成するオプション。


Product クラス**リスト 1**これらの検証属性を使用する方法を示しています。 名前、説明、および UnitPrice プロパティがマークされている必須とします。 Name プロパティには、文字列の長さが 10 より小さい文字である必要があります。 最後に、UnitPrice プロパティは、通貨金額を表す正規表現パターンと一致する必要があります。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**リスト 1**: Models\Product.cs

Product クラスは、1 つの追加属性を使用する方法を示しています。 DisplayName 属性。 DisplayName 属性には、エラー メッセージが表示されたら、プロパティ、プロパティの名前を変更することができます。 「UnitPrice フィールドが必要です」エラー メッセージを表示する代わりには「価格フィールドが必要です」エラー メッセージ表示できます。

> [!NOTE] 
> 
> 検証コントロールによって表示されるエラー メッセージを完全にカスタマイズする場合は、このような検証コントロールのエラー メッセージのプロパティに、カスタム エラー メッセージを割り当てることができます。 `<Required(ErrorMessage:="This field needs a value!")>`


Product クラスを使用する**リスト 1**で Create() コント ローラー アクションで**リスト 2**。 このコント ローラー アクションでは、モデルの状態には、すべてのエラーが含まれている場合に、作成ビューが再表示されます。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**リスト 2**: Controllers\ProductController.vb

ビューを作成する最後に、**リスト 3** Create() アクションを右クリックし、メニュー オプションを選択して**ビューの追加**します。 製品クラスを使用して、モデル クラスとして厳密に型指定されたビューを作成します。 選択**作成**ビューのコンテンツのドロップダウン リストから (を参照してください**図 2**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**図 2**: 作成ビューの追加

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**リスト 3.**: Views\Product\Create.aspx

> [!NOTE] 
> 
> によって生成されるフォームを作成するから、Id フィールドを削除、**ビューの追加**メニュー オプション。 Id フィールドが Id 列に対応していますので、このフィールドの値を入力するユーザーを許可したくないです。


成果物を作成するためのフォームを送信するかどうかと必須のフィールドの値を入力しないでください、検証エラー メッセージが**図 3**が表示されます。

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**図 3**: 必要なフィールドがないです。

無効な金額でエラー メッセージを入力する場合**図 4**が表示されます。

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**図 4**: 無効な通貨金額

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Entity Framework でのデータ注釈検証コントロールの使用

Microsoft Entity Framework を使用すると、データ モデル クラスを生成している場合は、クラスに直接検証属性を適用することはできません。 Entity Framework デザイナーでは、モデル クラスを生成するため、モデル クラスに加えた変更はすべてには、次に、デザイナーで変更を加えるときが上書きされます。

Entity Framework によって生成されたクラスで、検証コントロールを使用する場合は、メタ データ クラスを作成する必要があります。 検証コントロールを実際のクラスに検証コントロールを適用する代わりにメタ データ クラスに適用します。

たとえば、ムービーを Entity Framework を使用してクラスを作成した (を参照してください**図 5**)。 さらに、ディレクター、ムービーのタイトルのプロパティの必須プロパティを作成することに想像してください。 部分クラスとメタ データ クラスを作成する場合、**リスト 4**します。

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**図 5**: Entity Framework によって生成されたムービー クラス

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**リスト 4**: Models\Movie.cs

ファイル**リスト 4**ムービーと MovieMetaData という 2 つのクラスが含まれています。 ムービー クラスは、部分クラスです。 これは、DataModel.Designer.vb ファイル内の Entity Framework によって生成された部分クラスに対応します。

現時点では、.NET framework には、一部のプロパティはできません。 そのため、ファイル内に定義されているムービー クラスのプロパティに検証属性を適用することで、DataModel.Designer.vb ファイルで定義されているムービー クラスのプロパティに検証属性を適用する方法はありません**リスト4**.

ムービーの部分クラスを指し示す MovieMetaData クラス MetadataType 属性で修飾されたことに注意してください。 MovieMetaData クラスには、プロパティ、Movie クラスのプロキシのプロパティが含まれています。

検証属性は、MovieMetaData クラスのプロパティに適用されます。 タイトル、監督、および DateReleased プロパティは、すべての必須プロパティとしてマークされます。 ディレクター プロパティには、5 未満の場合の文字を含む文字列を割り当てる必要があります。 DisplayName 属性を「リリース済みの日付フィールドが必要です」ようなエラー メッセージを表示する DateReleased プロパティに適用する最後に、 エラーではなく「DateReleased フィールドが必要です。」

> [!NOTE] 
> 
> MovieMetaData クラスでプロキシのプロパティが、Movie クラス内の対応するプロパティと同じ型を表す必要がないことに注意してください。 たとえば、ディレクター プロパティは、ムービー クラスの文字列プロパティと MovieMetaData クラスでオブジェクトのプロパティが。


内のページ**図 6**ムービーのプロパティに無効な値を入力するときに返されるエラー メッセージを示しています。

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**図 6**: Entity Framework での検証コントロールの使用 ([フルサイズの画像を表示する をクリックします](validation-with-the-data-annotation-validators-cs/_static/image14.png))。

## <a name="summary"></a>まとめ

このチュートリアルでは、ASP.NET MVC アプリケーション内の検証を実行するには、データ注釈モデル バインダーを活用する方法について説明しました。 など、必要な検証属性や StringLength 属性の種類を使用する方法を学習しました。 Microsoft Entity Framework を使用する場合は、これらの属性を使用する方法も学習しました。

> [!div class="step-by-step"]
> [前へ](validating-with-a-service-layer-cs.md)
> [次へ](creating-model-classes-with-the-entity-framework-vb.md)
