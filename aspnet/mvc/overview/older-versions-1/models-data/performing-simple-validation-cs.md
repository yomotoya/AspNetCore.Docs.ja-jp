---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: 単純な検証 (c#) を実行する |Microsoft ドキュメント
author: StephenWalther
description: ASP.NET MVC アプリケーションで検証を実行する方法を説明します。 このチュートリアルでは、Stephen Walther は、モデルの状態にして検証 HTML ヘルパーを紹介しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 7fc1dcc6935841382215f67a519cd241ac68931a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869661"
---
<a name="performing-simple-validation-c"></a>単純な検証 (c#) を実行します。
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> ASP.NET MVC アプリケーションで検証を実行する方法を説明します。 このチュートリアルでは、Stephen Walther は、モデルの状態にして、検証 HTML ヘルパーを紹介します。


このチュートリアルの目的では、ASP.NET MVC アプリケーション内での検証を実行する方法について説明します。 たとえば、必要なフィールドの値を含まないフォームの送信を禁止する方法を説明します。 モデルの状態および検証する HTML ヘルパーを使用する方法を学びます。

## <a name="understanding-model-state"></a>モデルの状態について

検証エラーを表すモデルの状態、またはより正確には、モデル状態ディクショナリを使用するとします。 たとえば、リスト 1 の Create() アクションは、Product クラスをデータベースに追加する前に製品クラスのプロパティを検証します。


コント ローラーに、検証やデータベースのロジックを追加することを推奨してはしていません。 コント ローラーには、アプリケーションのフロー制御に関連するロジックのみを含める必要があります。 複雑にならないへのショートカットを実行しています。


**1 - Controllers\ProductController.cs を一覧表示します。**

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

リストの 1 では、Product クラスの名前、説明、および UnitsInStock プロパティを検証します。 これらのプロパティのいずれかの検証テストに失敗した場合、エラーは、(コント ローラー クラスの ModelState プロパティによって表される)、モデル状態ディクショナリに追加されます。

モデル状態エラーがある場合、ModelState.IsValid プロパティは false を返します。 その場合は、新しい製品を作成するための HTML フォームが再表示されます。 それ以外の場合、検証エラーがない場合は、新しい製品がデータベースに追加されます。

## <a name="using-the-validation-helpers"></a>検証ヘルパーの使用

ASP.NET MVC フレームワークには、次の 2 つの検証ヘルパーが含まれています: Html.ValidationMessage() ヘルパーと Html.ValidationSummary() ヘルパー。 検証エラー メッセージを表示するのにには、ビューでこれら 2 つのヘルパーを使用します。

Html.ValidationMessage() と Html.ValidationSummary() ヘルパーは、ASP.NET MVC のスキャフォールディングによって自動的に生成される作成および編集ビューで使用されます。 作成ビューを生成するこれらの手順に従います。

1. 製品のコント ローラーの Create() アクションを右クリックし、メニュー オプションを選択**ビューの追加**(図 1 を参照してください)。
2. **ビューの追加**ダイアログ ボックスで、チェック ボックス ラベルの付いた**厳密に型指定されたビューを作成**(図 2 を参照してください)。
3. **データ クラスを表示** ドロップダウン リストで、Product クラスを選択します。
4. **コンテンツを表示**ドロップダウン リストで、選択を作成します。
5. **[追加]** ボタンをクリックします。


ビューを追加する前に、アプリケーションをビルドすることを確認します。 クラスの一覧には表示されませんそれ以外の場合、**データ クラスを表示**ボックスの一覧です。


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)

**図 01**: ビューの追加 ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image2.png))


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)

**図 02**: 厳密に型指定されたビューを作成する ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image4.png))


次の手順を完了すると、リスト 2 で作成ビューが表示されます。

**Listing 2 - Views\Product\Create.aspx**

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

リストの 2 では、HTML フォームの上、Html.ValidationSummary() ヘルパーが直ちに呼び出さです。 このヘルパーを使用して、検証エラー メッセージの一覧を表示します。 Html.ValidationSummary() ヘルパーは、箇条書きリスト内のエラーを表示します。

各 HTML フォーム フィールドの横にある Html.ValidationMessage() ヘルパーと呼びます。 このヘルパーを使用して、フォーム フィールドの横にエラー メッセージを表示します。 一覧表示する 2 の場合、Html.ValidationMessage() ヘルパーでは、エラーがある場合にアスタリスクが表示されます。

図 3 に、ページは、不足しているフィールドと無効な値で、フォームが送信された場合、検証ヘルパーによって表示されるエラー メッセージを示しています。


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)

**図 03**: 問題と共に送信される、Create view ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image6.png))


HTML の外観入力フィールドは、検証エラーがある場合にも変更にすることに注意してください。 Html.TextBox() ヘルパー レンダリング、*クラス =「の入力の検証エラー」* Html.TextBox() ヘルパーによって表示されるプロパティに関連付けられている検証エラーが発生するときに属性。

検証エラーの外観の制御に使用される 3 つのカスケード スタイル シート クラスにがあります。

- 適用される入力の検証エラー -、&lt;入力&gt;Html.TextBox() ヘルパーによって表示されるタグ。
- 適用されるフィールドの検証エラー -、 &lt;span&gt; Html.ValidationMessage() ヘルパーによって表示されるタグ。
- 適用される検証の概要-エラー -、 &lt;ul&gt; Html.ValidationSumamry() ヘルパーによって表示されるタグ。

これらカスケード スタイル シートのクラスを変更し、そのため、コンテンツ フォルダーにある Site.css ファイルを変更することによって検証エラーの外観を変更できます。

> [!NOTE] 
> 
> HtmlHelper クラスが含まれています読み取り専用の静的プロパティには検証の名前を取得して関連する CSS クラス。 これらの静的プロパティには、ValidationInputCssClassName、ValidationFieldCssClassName、ValidationSummaryCssClassName 名前です。


## <a name="prebinding-validation-and-postbinding-validation"></a>検証および Postbinding prebinding

場合は、製品を作成するための HTML フォームを送信する、price フィールドおよび UnitsInStock フィールドの値に無効な値を入力して、図 4 に表示される検証メッセージを取得します。 ここではこれらの検証エラー メッセージから取得されるか


[![[新しいプロジェクト] ダイアログ ボックス](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)

**図 04**: Prebinding 妥当性確認エラー ([フルサイズのイメージを表示するをクリックして](performing-simple-validation-cs/_static/image8.png))


実際には 2 種類の検証エラー メッセージは HTML フォームのフィールドは、クラスにバインドされはフォームのフィールドは、クラスにバインドされた後に生成される前に生成されるものがあります。 つまり、ある prebinding 検証エラーと検証エラーを postbinding です。

1 の一覧で製品のコント ローラーによって公開される Create() アクションは、Product クラスのインスタンスを受け入れます。 Create メソッドのシグネチャは、このようになります。

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

フォームの作成から HTML フォーム フィールドの値は、モデル バインダーと呼ばれるもので、productToCreate クラスにバインドされます。 既定のモデル バインダー エラー メッセージを追加モデルの状態を自動的に、フォーム フィールド、フォームのプロパティにバインドできないときにします。

既定のモデル バインダーは、Product クラスの価格プロパティに文字列"apple"をバインドできません。 Decimal プロパティに文字列を割り当てることはできません。 そのため、モデル バインダーは、モデルの状態をエラーを追加します。

既定のモデル バインダーは、null 値を受け付けないことプロパティに null 値を割り当てることはできません。 具体的には、モデル バインダーは、UnitsInStock プロパティに null 値を割り当てることはできません。 もう一度、モデル バインダーを放棄し、モデルの状態をエラー メッセージを追加します。

エラー メッセージを prebinding これらの外観をカスタマイズする場合は、これらのメッセージのリソース文字列を作成する必要があります。

## <a name="summary"></a>まとめ

このチュートリアルの目的は、ASP.NET MVC フレームワークでの検証の基本的なしくみを説明することでした。 モデルの状態および検証する HTML ヘルパーを使用する方法を学習しました。 私たちも prebinding と検証を postbinding の違いを説明します。 他のチュートリアルでは、コント ローラー アウトし、モデルのクラスに検証コードを移動するためのさまざまな方法について説明します。

> [!div class="step-by-step"]
> [前へ](displaying-a-table-of-database-data-cs.md)
> [次へ](validating-with-the-idataerrorinfo-interface-cs.md)
