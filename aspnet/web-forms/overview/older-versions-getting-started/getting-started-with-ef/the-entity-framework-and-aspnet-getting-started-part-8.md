---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォームの第 8 部 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: f5ad2c1caf6036a0d8ee2ebbd07de60009090f1b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374053"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 8
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Dynamic Data 機能を使用して書式設定およびデータを検証するには

前のチュートリアルでは、ストアド プロシージャを実装します。 このチュートリアルでは説明 Dynamic Data 機能が次の利点を提供する方法。

- フィールドは自動的に、それらのデータ型に基づいて表示の書式設定します。
- フィールドは、それらのデータ型に基づいて自動的に検証されます。
- 書式設定と検証の動作をカスタマイズするデータ モデルにメタデータを追加することができます。 これを行うし、書式設定と検証ルールを追加するには、1 つだけで、すべての場所で自動的に適用する動的データ コントロールを使用してフィールドにアクセスします。

この動作を確認するには、表示し、編集、既存のフィールドを使用するコントロールを変更します*Students.aspx*の名前と日付のフィールドを書式設定と検証のメタデータを追加します ページとする、`Student`エンティティ型。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>DynamicField および DynamicControl コントロールの使用

開く、 *Students.aspx*ページし、`StudentsGridView`コントロール置換、**名前**と**加入契約日**`TemplateField`を次のマークアップ要素。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

このマークアップでは、`DynamicControl`の代わりに制御`TextBox`と`Label`、学生のコントロール テンプレートのフィールドの名前を使用して、`DynamicField`登録日付コントロール。 書式指定文字列が指定されていません。

追加、`ValidationSummary`制御した後、`StudentsGridView`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

`SearchGridView`コントロールのマークアップを置き換える、**名前**と**加入契約日**で実行したとする列、`StudentsGridView`省略点を除いて、制御、`EditItemTemplate`要素。 `Columns`の要素、`SearchGridView`コントロールが次のマークアップを含むようになりました。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

開いている*Students.aspx.cs*し、以下の追加`using`ステートメント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

ページのハンドラーを追加`Init`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

このコードでは、動的なデータが書式設定とこれらのフィールドのデータ バインド コントロールで検証を提供するを指定します、`Student`エンティティ。 次の例のようなエラー メッセージが表示されるは、ページを実行すると、通常場合を呼び出す忘れてしまった場合、`EnableDynamicData`メソッド`Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

ページを実行します。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

**加入契約日**列、プロパティの型があるため、日付と時刻が表示されます`DateTime`します。 後でを修正します。

ここでは、動的なデータが自動的に検証の基本的なデータを提供することを確認します。 たとえば、をクリックして**編集**、日付フィールドをクリア、 をクリックして**Update**としていること動的データに自動的にこの必須のフィールド値がデータ モデルで許容されないためです。 ページには、フィールドと、エラー メッセージの後にアスタリスクが表示されます、`ValidationSummary`コントロール。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

省略することもできます、`ValidationSummary`もエラー メッセージが表示するには、アスタリスクをマウス ポインターを格納できるため、制御します。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

動的データで入力したデータの検証も、**加入契約日**フィールドが有効な日付。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

ご覧のとおり、これは、一般的なエラー メッセージ。 次のセクションでは、メッセージ、検証、および書式指定規則をカスタマイズする方法を確認します。

## <a name="adding-metadata-to-the-data-model"></a>データ モデルにメタデータの追加

通常、動的なデータによって提供される機能をカスタマイズします。 たとえば、データの表示方法、およびエラー メッセージの内容を変更する可能性があります。 通常もカスタマイズする動的なデータより多くの機能を提供するデータの検証規則のデータ型に基づいて自動的に。 これを行うには、エンティティ型に対応する部分クラスを作成します。

**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトで、**参照の追加**への参照を追加および`System.ComponentModel.DataAnnotations`します。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

*DAL*フォルダー、新しいクラス ファイルを作成、名前を付けます*Student.cs*、し、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

このコードの部分クラスを作成し、`Student`エンティティ。 `MetadataType`この部分クラスに適用される属性を使用してメタデータを指定するクラスを識別します。 メタデータ クラスは、任意の名前を持つことができますが、一般的な方法は、エンティティ名と「メタデータ」を使用します。

メタデータ クラスのプロパティに適用される属性は、検証、ルール、およびエラー メッセージの書式設定を指定します。 ここに示す属性は、次の結果になります。

- `EnrollmentDate` 日付 (時刻) のないとして表示されます。
- 両方の名前フィールドは 25 文字である必要があります。 または小さい長、およびカスタム エラー メッセージで提供されます。
- 両方の名前フィールドは必須であり、カスタム エラー メッセージを提供します。

実行、 *Students.aspx*ページをもう一度、および、時刻なしの日付が表示されますを参照してください。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

行を編集し、名前フィールドの値のクリアを試します。 クリックする前に、フィールドのままにすると、すぐに表示されるフィールドのエラーを示すアスタリスク**Update**します。 クリックすると**Update**ページには、指定したエラー メッセージ テキストが表示されます。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

25 文字より長い名前を入力すると、クリックして**更新**、し、ページには、指定したエラー メッセージ テキストが表示されます。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

ようになりましたデータ モデルのメタデータでこれらの書式設定と検証ルールを設定すると、規則が自動的に適用されることを表示したり、これらのフィールドを変更できるようにするすべてのページを使用する限り、`DynamicControl`または`DynamicField`コントロール。 これは、量が減り、プログラミングおよびテストしやすいため、冗長なコードを記述する必要があるし、データの書式設定と検証が、アプリケーション全体で一貫性のあることになります。

## <a name="more-information"></a>説明

これには、Entity Framework の概要チュートリアルのこのシリーズで終了です。 Entity Framework を使用する方法の学習に役立つその他のリソースを引き続き[Entity Framework の次のチュートリアル シリーズの最初のチュートリアル](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)か、次のサイトを参照してください。

- [Entity Framework に関する FAQ](http://www.ef-faq.org/introduction.html)
- [Entity Framework チームのブログ](https://blogs.msdn.com/b/adonet/)
- [MSDN ライブラリの entity Framework](https://msdn.microsoft.com/library/bb399572.aspx)
- [MSDN データ デベロッパー センターで entity Framework](https://msdn.microsoft.com/data/ef.aspx)
- [EntityDataSource Web サーバー コントロール、MSDN ライブラリの概要](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource コントロール、MSDN ライブラリの API リファレンス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [MSDN の entity Framework フォーラム](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman のブログ](http://thedatafarm.com/blog/)

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-7.md)
