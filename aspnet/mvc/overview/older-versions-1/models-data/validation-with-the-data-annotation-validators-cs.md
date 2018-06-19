---
uid: mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
title: データ注釈検証コントロール (c#) を使用した検証 |Microsoft ドキュメント
author: microsoft
description: ASP.NET MVC アプリケーション内での検証を実行するデータの注釈モデル バインダーの手順を利用します。 さまざまな種類の検証コントロールを使用する方法を説明してください.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/29/2009
ms.topic: article
ms.assetid: 7ca8013e-9dfc-4e33-8336-cdccfd5f9414
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/validation-with-the-data-annotation-validators-cs
msc.type: authoredcontent
ms.openlocfilehash: 0aca9472094e6a54c7b7cb4ad4f12df64fe12db2
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870194"
---
<a name="validation-with-the-data-annotation-validators-c"></a>データ注釈検証コントロール (c#) を使用した検証
====================
によって[Microsoft](https://github.com/microsoft)

> ASP.NET MVC アプリケーション内での検証を実行するデータの注釈モデル バインダーの手順を利用します。 さまざまな種類の検証属性を使用し、Microsoft Entity Framework に処理する方法を説明します。


このチュートリアルでは、データ注釈検証コントロールを使用して、ASP.NET MVC アプリケーションで検証を実行する方法を学習します。 データ注釈検証コントロールを使用する利点は、化など、必要な – 1 つまたは複数の属性や StringLength 属性を追加するだけで – クラス プロパティに検証を実行できることです。

データ注釈検証コントロールを使用することができます、前に、データ注釈のモデル バインダーをダウンロードする必要があります。 CodePlex web サイトからデータ注釈のモデル バインダーのサンプルをダウンロードする をクリックして[ここ](http://aspnet.codeplex.com/Release/ProjectReleases.aspx?ReleaseId=24471)です。


ことが重要データ注釈のモデル バインダーは、Microsoft ASP.NET MVC フレームワークの公式の一部ではないことを理解します。 データ注釈のモデル バインダーが、Microsoft ASP.NET MVC チームによって作成されますが、Microsoft がデータ注釈のモデル バインダーの正式な製品のサポートが説明されているし、このチュートリアルで使用される提供しません。


## <a name="using-the-data-annotation-model-binder"></a>データ注釈のモデル バインダーを使用してください。

ASP.NET MVC アプリケーションでデータの注釈のモデル バインダーを使用するのには、まず、Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へのアセンブリへの参照を追加する必要があります。 メニュー オプションを選択**プロジェクトで、参照の追加**です。 次へ をクリックして、**参照** タブを見つけて、データ注釈モデル バインダーのサンプルのダウンロード (および解凍)、(を参照してください**図 1**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image2.png)](validation-with-the-data-annotation-validators-cs/_static/image1.png)

**図 1**: データの注釈のモデル バインダーへの参照を追加する ([フルサイズのイメージを表示するをクリックして](validation-with-the-data-annotation-validators-cs/_static/image3.png))

Microsoft.Web.Mvc.DataAnnotations.dll アセンブリと system.componentmodel.dataannotations.dll へアセンブリの両方を選択し、クリックして、 **OK**ボタンをクリックします。


データ注釈のモデル バインダーを備えた .NET Framework Service Pack 1 に含まれている system.componentmodel.dataannotations.dll へのアセンブリを使用することはできません。 データ注釈モデル バインダー サンプル ダウンロードに含まれている system.componentmodel.dataannotations.dll へアセンブリのバージョンを使用する必要があります。


最後に、Global.asax ファイルで DataAnnotations モデル バインダーを登録する必要があります。 アプリケーションに次のコード行を追加\_Start() イベント ハンドラーをアプリケーション\_Start() メソッドは、次のようになります。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample1.cs)]

次のコード行は、ASP.NET MVC アプリケーション全体の既定のモデル バインダーとして、ataAnnotationsModelBinder を登録します。

## <a name="using-the-data-annotation-validator-attributes"></a>データ注釈検証コントロールの属性の使用

データ注釈のモデル バインダーを使用する場合は、検証を実行するバリデーター属性を使用します。 System.ComponentModel.DataAnnotations 名前空間には、次の検証属性が含まれています。

- 範囲は – 指定された範囲の値の間に、プロパティの値があるかどうかを検証することができます。
- 正規表現-には、プロパティの値が指定した正規表現パターンに一致するかどうかを検証することができます。
- 必要な – 必要に応じてプロパティをマークすることができます。
- StringLength – では、文字列プロパティの最大長を指定することができます。
- 検証: すべての検証属性の基本クラスです。

> [!NOTE] 
> 
> 標準の検証コントロールのいずれかによって、検証ニーズが満たされない場合、常がある基本検証属性を新しいバリデーター属性を継承してカスタム検証属性を作成するオプションです。


製品クラス**リスト 1**これらバリデーター属性を使用する方法を示しています。 名前、説明、および UnitPrice プロパティもマークとして必要です。 Name プロパティには、10 文字未満である文字列の長さが必要です。 最後に、UnitPrice プロパティは、金額を表す正規表現パターンに一致する必要があります。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample2.cs)]

**1.**: Models\Product.cs

Product クラスが 1 つの追加属性を使用する方法を示しています。 DisplayName 属性。 DisplayName 属性では、エラー メッセージが表示されたら、プロパティ、プロパティの名前を変更することができます。 「UnitPrice フィールドが必要」というエラー メッセージを表示する代わりには「Price フィールドが必要」というエラー メッセージ表示できます。

> [!NOTE] 
> 
> 検証コントロールによって表示されるエラー メッセージを完全にカスタマイズする場合は、次のように ErrorMessage プロパティの検証コントロールのカスタム エラー メッセージを割り当てることができます。 `<Required(ErrorMessage:="This field needs a value!")>`


Product クラスを使用することができます**リスト 1**の Create() コント ローラーのアクションと**を一覧表示する 2**です。 このコント ローラーのアクションでは、モデルの状態には、すべてのエラーが含まれている場合、ビューを作成するが再表示されます。

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample3.cs)]

**2 を一覧表示する**: Controllers\ProductController.vb

ビューを作成する最後に、**リスト 3** Create() アクションを右クリックし、メニュー オプションを選択して**ビューの追加**です。 製品クラスを使用したモデル クラスとして厳密に型指定されたビューを作成します。 選択**作成**ビューのコンテンツのドロップダウン リストから (を参照してください**図 2**)。

[![](validation-with-the-data-annotation-validators-cs/_static/image5.png)](validation-with-the-data-annotation-validators-cs/_static/image4.png)

**図 2**: 作成ビューの追加

[!code-aspx[Main](validation-with-the-data-annotation-validators-cs/samples/sample4.aspx)]

**3 を一覧表示する**: Views\Product\Create.aspx

> [!NOTE] 
> 
> によって生成されたフォームの作成から、Id フィールドを削除、**ビューの追加**メニュー オプション。 Id フィールドは、Id 列に対応しているために、このフィールドの値を入力できるようにするしたくありません。


製品を作成するフォームを送信して、必要なフィールドの値を入力しない場合検証エラー メッセージで**図 3**が表示されます。

[![](validation-with-the-data-annotation-validators-cs/_static/image7.png)](validation-with-the-data-annotation-validators-cs/_static/image6.png)

**図 3**: 必要なフィールドがありません

無効な金額、内のエラー メッセージを入力すると**図 4**が表示されます。

[![](validation-with-the-data-annotation-validators-cs/_static/image9.png)](validation-with-the-data-annotation-validators-cs/_static/image8.png)

**図 4**: 無効な金額

## <a name="using-data-annotation-validators-with-the-entity-framework"></a>Entity Framework でのデータ注釈検証コントロールの使用

Microsoft Entity Framework を使用して、データ モデル クラスを生成する場合は、クラスに直接バリデーター属性を適用することはできません。 Entity Framework Designer は、モデル クラスを生成するため、モデルのクラスに加えた変更はすべてには、次回、デザイナーで変更を加えるが上書きされます。

Entity Framework によって生成されたクラスでの検証コントロールを使用する場合は、メタ データ クラスを作成する必要があります。 検証コントロールを実際のクラスに適用する代わりにメタ データ クラスには、検証コントロールを適用します。

たとえば、Entity Framework を使用して、ムービー クラスが作成されたこと (を参照してください**図 5**)。 さらに、ムービー タイトルとディレクター プロパティの必須プロパティを作成することに想像してください。 部分クラスと内のメタ データ クラスを作成する場合は、**リスト 4**です。

[![](validation-with-the-data-annotation-validators-cs/_static/image11.png)](validation-with-the-data-annotation-validators-cs/_static/image10.png)

**図 5**: Entity Framework によって生成されたムービー クラス

[!code-csharp[Main](validation-with-the-data-annotation-validators-cs/samples/sample5.cs)]

**リスト 4**: Models\Movie.cs

内のファイル**リスト 4**ムービーおよび MovieMetaData という名前の 2 つのクラスが含まれています。 ムービー クラスは、部分クラスです。 これは、DataModel.Designer.vb ファイルに含まれている Entity Framework によって生成される部分クラスに対応します。

現時点では、.NET framework は一部のプロパティをサポートしていません。 そのため、ファイルで定義されているムービー クラスのプロパティに検証属性を適用することによって、DataModel.Designer.vb ファイルで定義されているムービー クラスのプロパティに検証属性を適用する方法はありません**リスト 4**.

ムービーの部分クラスが MovieMetaData クラスを指している MetadataType 属性で修飾されたことに注意してください。 MovieMetaData クラスには、ムービー クラスのプロパティのプロキシのプロパティが含まれています。

バリデーター属性は、MovieMetaData クラスのプロパティに適用されます。 タイトルやディレクター、DateReleased プロパティは、すべての必須プロパティとしてマークされます。 ディレクター プロパティには、5 未満の値の文字を含む文字列を割り当てる必要があります。 最後に、DisplayName 属性がプロパティに適用される、DateReleased 必須「リリース済みの日付フィールドを行うことです」と同じように、エラー メッセージを表示するには エラーの代わりに「DateReleased フィールドが必要です。」

> [!NOTE] 
> 
> MovieMetaData クラスのプロキシのプロパティがムービー クラスで対応するプロパティと同じ型を表す必要がないことに注意してください。 たとえば、ディレクター プロパティは、ムービー クラス内の文字列プロパティ MovieMetaData クラス内のオブジェクト プロパティです。


内のページ**図 6**ムービーのプロパティに無効な値を入力するときに返されるエラー メッセージを示しています。

[![](validation-with-the-data-annotation-validators-cs/_static/image13.png)](validation-with-the-data-annotation-validators-cs/_static/image12.png)

**図 6**: Entity Framework での検証コントロールの使用 ([フルサイズのイメージを表示するをクリックして](validation-with-the-data-annotation-validators-cs/_static/image14.png))

## <a name="summary"></a>まとめ

このチュートリアルでは、ASP.NET MVC アプリケーション内での検証を実行するデータの注釈モデル バインダーを活用する方法について学習しました。 など、必要な検証属性や StringLength 属性の種類を使用する方法を学習しました。 また、Microsoft の Entity Framework を使用する場合は、これらの属性を使用する方法も学習しました。

> [!div class="step-by-step"]
> [前へ](validating-with-a-service-layer-cs.md)
> [次へ](creating-model-classes-with-the-entity-framework-vb.md)
