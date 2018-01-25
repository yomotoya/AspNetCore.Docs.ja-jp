---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
title: "データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォームの一部 8 |Microsoft ドキュメント"
author: tdykstra
description: "Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、."
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: aaadd9bb-5508-448c-ad57-5497dff90e13
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-8
msc.type: authoredcontent
ms.openlocfilehash: 323ee44f43f6d4081bd9ba50791755696bc9128f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-8"></a>データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォームの一部 8
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="using-dynamic-data-functionality-to-format-and-validate-data"></a>Dynamic Data 機能を使用する書式設定し、データの検証

前のチュートリアルでは、ストアド プロシージャを実装します。 このチュートリアルを紹介する Dynamic Data 機能が、次の利点を提供する方法。

- フィールドは、そのデータ型に基づいて表示の書式が自動的にします。
- フィールドは、そのデータ型に基づいて自動的に検証されます。
- メタデータは、書式設定と検証の動作をカスタマイズするデータ モデルに追加できます。 これを行う、書式設定と検証規則を追加するには 1 か所にまとめて、およびすべての場所に自動的に適用するときに動的データ コントロールを使用してフィールドにアクセスします。

このしくみを表示するには、表示し、編集、既存のフィールドを使用するコントロールを変更します*Students.aspx*の名前と日付のフィールドを書式設定と検証のメタデータを追加します ページとして、`Student`エンティティ型。

[![Image01](the-entity-framework-and-aspnet-getting-started-part-8/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image1.png)

## <a name="using-dynamicfield-and-dynamiccontrol-controls"></a>DynamicField および DynamicControl コントロールの使用

開く、 *Students.aspx*ページし、、`StudentsGridView`コントロールの置換、**名前**と**登録日**`TemplateField`次のマークアップ要素。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample1.aspx)]

このマークアップを使用して`DynamicControl`の代わりに制御`TextBox`と`Label`受講者にコントロール テンプレート フィールドという名前を使用して、`DynamicField`登録日を制御します。 書式指定文字列が指定されていません。

追加、`ValidationSummary`後制御、`StudentsGridView`コントロール。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample2.aspx)]

`SearchGridView`コントロールのマークアップを置き換える、**名前**と**登録日**で行っていたときと列、`StudentsGridView`を制御する省略点を除いて、`EditItemTemplate`要素。 `Columns`の要素、`SearchGridView`コントロールには、次のマークアップが含まれています。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample3.aspx)]

開いている*Students.aspx.cs*し、以下の追加`using`ステートメント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample4.cs)]

ページのハンドラーを追加`Init`イベント。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample5.cs)]

このコードは、動的なデータが書式設定とこれらのデータ バインド コントロールでのフィールドの検証を実現するを指定します、`Student`エンティティです。 次の例のようなエラー メッセージが表示されるは、ページを実行すると、通常場合を呼び出すを忘れてしまった、`EnableDynamicData`メソッド`Page_Init`:

`Could not determine a MetaTable. A MetaTable could not be determined for the data source 'StudentsEntityDataSource' and one could not be inferred from the request URL.`

ページを実行します。

[![Image03](the-entity-framework-and-aspnet-getting-started-part-8/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image3.png)

**登録日**列、プロパティの型があるため、日付と時刻が表示されます`DateTime`です。 後で修正します。

ここでは、動的なデータが自動的に基本的なデータの検証を提供することを確認します。 たとえば、をクリックして**編集**日付フィールドをクリアしてクリックし、**更新**、している動的データに自動的にできるためこの必須フィールド値は、データ モデルに null 値ではありません。 ページには、フィールドと、エラー メッセージの後にアスタリスクが表示されます、`ValidationSummary`コントロール。

[![Image05](the-entity-framework-and-aspnet-getting-started-part-8/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image5.png)

省略することも、`ValidationSummary`もエラー メッセージを表示するには、アスタリスク経由でマウス ポインターを格納できるため、制御します。

[![Image06](the-entity-framework-and-aspnet-getting-started-part-8/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image7.png)

動的なデータで入力したデータの検証も、**登録日**フィールドは、有効な日付。

[![Image04](the-entity-framework-and-aspnet-getting-started-part-8/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image9.png)

ご覧のように、これは、一般的なエラー メッセージ。 次のセクションでは、メッセージ、検証、および書式指定規則をカスタマイズする方法が表示されます。

## <a name="adding-metadata-to-the-data-model"></a>データ モデルにメタデータを追加します。

通常、動的なデータによって提供される機能をカスタマイズします。 たとえば、データの表示方法、およびエラー メッセージの内容を変更する可能性があります。 通常もカスタマイズする動的なデータが提供する機能よりも多くの機能を提供するデータの検証規則のデータ型に基づいて自動的にします。 これを行うには、エンティティ型に対応する部分クラスを作成します。

**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトで、**参照の追加**への参照を追加および`System.ComponentModel.DataAnnotations`です。

[![Image11](the-entity-framework-and-aspnet-getting-started-part-8/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image11.png)

*DAL*フォルダーで、新しいクラス ファイルを作成、名前を付けます*Student.cs*とで、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](the-entity-framework-and-aspnet-getting-started-part-8/samples/sample6.cs)]

このコードの部分クラスを作成する、`Student`エンティティです。 `MetadataType`この部分クラスに適用される属性は、メタデータを指定しているクラスを識別します。 メタデータ クラスは、任意の名前を持つことができますが、一般的な方法は、エンティティ名と「メタデータ」を使用します。

メタデータ クラスのプロパティに適用される属性は、検証、ルール、およびエラー メッセージの書式設定を指定します。 次に示す属性は、次の結果が得られます。

- `EnrollmentDate`(時刻) のない日付として表示されます。
- 両方の名前フィールドは 25 文字である必要があります。 またはでは、長さ、およびカスタム エラー メッセージが指定されています。
- 両方の名前フィールドは必須で、カスタム エラー メッセージが提供されます。

実行、 *Students.aspx*ページをもう一度、および時刻なしの日付が表示されるようになりましたことを参照してください。

[![Image08](the-entity-framework-and-aspnet-getting-started-part-8/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image13.png)

行を編集して、名前のフィールドに値を消去する再試行してください。 クリックする前に、フィールドのままにするとすぐに、フィールドのエラーを示すアスタリスクが表示される**更新**です。 クリックすると、**更新**ページには、指定したエラー メッセージ テキストが表示されます。

[![Image10](the-entity-framework-and-aspnet-getting-started-part-8/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image15.png)

25 文字より長い名前を入力すると、をクリックして**更新**、し、ページには、指定したエラー メッセージ テキストが表示されます。

[![Image09](the-entity-framework-and-aspnet-getting-started-part-8/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-8/_static/image17.png)

これで、データ モデルのメタデータでこれらの書式設定と検証ルールを設定したら、規則に自動的に適用されることにより、これらのフィールドへの変更または表示されるページごとを使用する限り、`DynamicControl`または`DynamicField`コントロール。 これは、量が減り、プログラミングおよびテストしやすいため、重複するコードを記述する必要があるし、データの書式設定と検証が、アプリケーション全体で一貫していることを確認します。

## <a name="more-information"></a>説明

これは、この一連の Entity Framework の概要についてのチュートリアルを終了します。 Entity Framework を使用する方法を学習するためのリソースに引き続き[次の Entity Framework チュートリアル シリーズの最初のチュートリアル](../continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)または、次のサイトを参照してください。

- [Entity Framework に関する FAQ](http://www.ef-faq.org/introduction.html)
- [Entity Framework チーム ブログ](https://blogs.msdn.com/b/adonet/)
- [MSDN ライブラリの entity Framework](https://msdn.microsoft.com/library/bb399572.aspx)
- [Entity Framework では、MSDN データ デベロッパー センター](https://msdn.microsoft.com/data/ef.aspx)
- [MSDN ライブラリの EntityDataSource Web サーバー コントロールの概要](https://msdn.microsoft.com/library/cc488502.aspx)
- [EntityDataSource コントロール、MSDN ライブラリの API リファレンス](https://msdn.microsoft.com/library/system.web.ui.webcontrols.entitydatasource.aspx)
- [MSDN の entity Framework フォーラム](https://social.msdn.microsoft.com/forums/adodotnetentityframework/)
- [Julie Lerman のブログ](http://thedatafarm.com/blog/)

>[!div class="step-by-step"]
[前へ](the-entity-framework-and-aspnet-getting-started-part-7.md)
