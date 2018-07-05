---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 6 |Microsoft Docs
author: tdykstra
description: Contoso University のサンプルの web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: 99861197e00a3f2f6811ef13136fac63b993ef32
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37372107"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>Entity Framework 4.0 Database でまず getting Started と ASP.NET 4 Web フォーム - パート 6
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso University のサンプルの web アプリケーションでは、Entity Framework 4.0 と Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、次を参照してください[シリーズの最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Table-Per-Hierarchy 継承の実装

前のチュートリアルで使用した関連するデータを追加してリレーションシップを削除し、既存のエンティティに関係する新しいエンティティを追加することで。 このチュートリアルでは、データ モデルで継承を実装する方法を示します。

オブジェクト指向プログラミングでの関連クラスを使用しやすく継承を使用することができます。 たとえば、作成する`Instructor`と`Student`から派生するクラス、`Person`基本クラス。 Entity Framework では、同じ種類のエンティティ間の継承構造を作成できます。

このチュートリアルでは、新しい web ページを作成しません。 代わりに、派生したエンティティ データ モデルに追加し、新しいエンティティを使用する既存のページを変更します。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>テーブルの型ごとの継承とテーブルは階層ごと

データベースは、1 つのテーブルまたは複数のテーブルに関連するオブジェクトに関する情報を格納できます。 たとえば、 `School` 、データベース、`Person`テーブルに受講者とインストラクターによる 1 つのテーブルの両方に関する情報が含まれています。 インストラクターのみに適用の一部の列 (`HireDate`)、受講者のみに (`EnrollmentDate`)、および両方にいくつか (`LastName`、 `FirstName`)。

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

作成する Entity Framework を構成する`Instructor`と`Student`から継承するエンティティ、`Person`エンティティ。 単一のデータベース テーブルからエンティティの継承構造を生成するには、このパターンと呼ばれます *- table-per-hierarchy* (TPH) 継承します。

コースの受講の`School`データベースが別のパターンを使用します。 オンライン コースとオンサイト コースがそれぞれを指す外部キーを持つ個別のテーブルに格納されている、`Course`テーブル。 両方のコース型に共通の情報は格納されているだけで、`Course`テーブル。

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Entity Framework データ モデルを構成するように`OnlineCourse`と`OnsiteCourse`エンティティが継承、`Course`エンティティ。 すべての種類に共通のデータを格納するテーブルを参照する独立した各テーブルの種類ごとに個別のテーブルからエンティティの継承構造を生成するには、このパターンと呼ばれます*table-per-type* (TPT) 継承します。

TPH 継承パターンは、TPT パターンが複雑な結合クエリのため一般にパフォーマンスを向上させると、TPT 継承パターンよりも、Entity Framework で提供します。 このチュートリアルでは、TPH 継承を実装する方法を示します。 次の手順を実行することによってを実行してみましょう。

- 作成`Instructor`と`Student`から派生したエンティティ型`Person`します。
- 派生エンティティに関連する移動プロパティ、`Person`派生エンティティをエンティティ。
- 派生型のプロパティに対する制約を設定します。
- ように、`Person`エンティティ、抽象エンティティです。
- 各マップの派生エンティティを`Person`を確認する方法を指定する条件を満たすテーブルかどうかを`Person`行の派生型を表します。

## <a name="adding-instructor-and-student-entities"></a>Instructor と Student エンティティの追加

開く、 <em>SchoolModel.edmx</em>ファイルで、デザイナーで使用されていない領域を右クリックして<strong>追加</strong>を選択し、<strong>エンティティ</strong><em>します。</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

**エンティティの追加**ダイアログ ボックスで、エンティティの名前`Instructor`設定とその**基本型**オプションを`Person`します。

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

**[OK]** をクリックします。 デザイナーを作成、`Instructor`から派生するエンティティ、`Person`エンティティ。 新しいエンティティにはまだ、任意のプロパティがありません。

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

作成する手順を繰り返して、`Student`からも派生エンティティ`Person`します。

のみからそのプロパティに移動する必要があるために、インストラクターが雇用日、`Person`エンティティを`Instructor`エンティティ。 `Person` 、エンティティを右クリックし、`HireDate`プロパティをクリックします**切り取り**します。 右クリックし、**プロパティ**で、`Instructor`エンティティをクリックします**貼り付け**します。

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

入社日、`Instructor`エンティティを null にすることはできません。 右クリックし、`HireDate`プロパティ、 をクリックして**プロパティ**、し、**プロパティ**ウィンドウ変更`Nullable`に`False`します。

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

移動する手順を繰り返して、`EnrollmentDate`プロパティから、`Person`エンティティを`Student`エンティティ。 設定することを確認`Nullable`に`False`の`EnrollmentDate`プロパティ。

これで、`Person`エンティティに共通するプロパティのみ`Instructor`と`Student`エンティティ (ナビゲーション プロパティを移動していない) とは別のエンティティのみ使用できますの継承構造の基本エンティティとして。 そのため、個別のエンティティとして扱わはしないことを確認する必要があります。 右クリックし、`Person`エンティティで、**プロパティ**、し、**プロパティ**ウィンドウの値を変更する、**抽象**プロパティを**True**します。

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Instructor と Student エンティティを Person テーブルにマッピング

Entity Framework を区別する方法を指示する必要があります`Instructor`と`Student`データベース内のエンティティ。

右クリックし、`Instructor`エンティティと選択**テーブル マッピング**します。 **マッピングの詳細**ウィンドウで、をクリックして**テーブルまたはビューを追加**選択と**人**。

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

クリックして**条件を追加**、し、 **HireDate**します。

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

変更**演算子**に**は**と**値/プロパティ**に**Not Null**します。

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

手順を繰り返して、`Students`にこのエンティティがマップされるかを指定して、エンティティ、`Person`時にテーブル、`EnrollmentDate`列は null ではありません。 保存して、データ モデルを終了します。

クラスとして新しいエンティティを作成し、デザイナーで利用できるようにするためにプロジェクトをビルドします。

## <a name="using-the-instructor-and-student-entities"></a>Instructor と Student エンティティを使用してください。

データをバインドする student および instructor のデータを処理する web ページを作成するときに、`Person`エンティティ セットとでフィルター処理、`HireDate`または`EnrollmentDate`学生や教員に返されるデータを制限するプロパティ。 ただし、ここでバインドする場合に各データ ソース コントロール、`Person`エンティティ セット、だけを指定できます`Student`または`Instructor`エンティティの種類を選択する必要があります。 Entity Framework は、学生およびインストラクターを区別する方法を知っているため、`Person`削除するエンティティのセット、`Where`そのために手動で入力したプロパティの設定。

Visual Studio デザイナーであるエンティティ型を指定できます、`EntityDataSource`でコントロールを選択する必要があります、 **EntityTypeFilter**のドロップダウン ボックス、`Configure Data Source`ウィザードで、次の例に示すようにします。

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

および、**プロパティ**を除去するウィンドウ`Where`が不要になった次の例に示すように句の値。

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

ただしのマークアップを変更したため、`EntityDataSource`コントロールを使用する、`ContextTypeName`実行することはできません、属性、**データ ソースの構成**ウィザード`EntityDataSource`を既に作成したコントロール。 そのため、代わりにマークアップを変更することで、必要な変更を作成します。

開く、 *Students.aspx*ページ。 `StudentsEntityDataSource`コントロールを削除、`Where`属性を追加、`EntityTypeFilter="Student"`属性。 マークアップは次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

設定、`EntityTypeFilter`属性により、`EntityDataSource`コントロールが指定したエンティティ型のみを選択します。 両方を取得する場合は`Student`と`Instructor`エンティティ型の場合は、この属性を設定しません。 (いずれかで複数のエンティティ型を取得するオプションがある`EntityDataSource`読み取り専用データ アクセスのコントロールを使用している場合にのみを制御します。 使用している場合、`EntityDataSource`コントロールを挿入、更新、またはエンティティを削除しにバインドされているエンティティ セットは、複数の種類を含めることができる場合、のみ、1 つのエンティティ型を使用することができ、この属性を設定する必要があります)。

手順を繰り返して、`SearchEntityDataSource`コントロールの一部のみを削除する点を除いて、`Where`属性を選択する`Student`プロパティを完全に削除する代わりにエンティティ。 コントロールの開始タグは次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

前に、と同様に動作もすることを確認するページを実行します。

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

新しいを使用するように、前のチュートリアルで作成した次のページを更新`Student`と`Instructor`の代わりにエンティティ`Person`エンティティを実行前に、と同じように動作することを確認します。

- *StudentsAdd.aspx*、追加`EntityTypeFilter="Student"`を`StudentsEntityDataSource`コントロール。 マークアップは次の例のようになります。 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- *About.aspx*、追加`EntityTypeFilter="Student"`を`StudentStatisticsEntityDataSource`制御、および削除`Where="it.EnrollmentDate is not null"`します。 マークアップは次の例のようになります。 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- *Instructors.aspx*と*InstructorsCourses.aspx*、追加`EntityTypeFilter="Instructor"`を`InstructorsEntityDataSource`制御、および削除`Where="it.HireDate is not null"`します。 内のマークアップ*Instructors.aspx*次の例のようになります。 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    内のマークアップ*InstructorsCourses.aspx*は次の例のようになります。

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

これらの変更の結果として、いくつかの方法で、Contoso University アプリケーションの保守容易性を改善しました。 UI レイヤーからの選択と検証ロジックを移行した (*.aspx*マークアップ) とデータ アクセス層の不可欠な部分になりました。 これにより、将来的に、データベース スキーマまたはデータ モデルにすることができる変更からアプリケーション コードを分離します。 たとえば、受講者は教師の補助として採用する場合があり、そのため、入社日を取得するようを決定する可能性があります。 インストラクターと受講者を区別し、データ モデルを更新する新しいプロパティを追加できます。 学生向けの入社日を表示したい以外を変更する必要は web アプリケーションのコードではありません。 追加のもう 1 つのメリット`Instructor`と`Student`エンティティは、コードが指すした場合よりもより簡単に理解できること`Person`受講者を実際になったオブジェクトまたはインストラクターです。

これで、Entity Framework での継承パターンを実装する方法の 1 つを見てきました。 次のチュートリアルでは、Entity Framework がデータベースにアクセスする方法をより細かく制御するためにストアド プロシージャを使用する方法を学習します。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-7.md)
