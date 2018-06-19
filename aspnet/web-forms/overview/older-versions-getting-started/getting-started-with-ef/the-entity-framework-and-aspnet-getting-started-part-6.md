---
uid: web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
title: データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォームの第 6 部 |Microsoft ドキュメント
author: tdykstra
description: Contoso 大学でサンプル web アプリケーションでは、Entity Framework を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 サンプル アプリケーションは、.
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/03/2010
ms.topic: article
ms.assetid: 994a5496-c648-4830-b03c-55bb43f325d2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/getting-started-with-ef/the-entity-framework-and-aspnet-getting-started-part-6
msc.type: authoredcontent
ms.openlocfilehash: b76be25501275ba676c9a9acca8e73333439ee70
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888800"
---
<a name="getting-started-with-entity-framework-40-database-first-and-aspnet-4-web-forms---part-6"></a>データベースの概要 Entity Framework 4.0 最初および ASP.NET 4 Web フォームの第 6 部
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> Contoso 大学でサンプル web アプリケーションでは、Entity Framework 4.0 および Visual Studio 2010 を使用して ASP.NET Web フォーム アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください[系列内の最初のチュートリアル。](the-entity-framework-and-aspnet-getting-started-part-1.md)


## <a name="implementing-table-per-hierarchy-inheritance"></a>Table-Per-Hierarchy 継承の実装

前のチュートリアルで使用していた関連するデータを追加してリレーションシップを削除して、既存のエンティティとの関係のある新しいエンティティを追加することによりします。 このチュートリアルでは、データ モデルで継承を実装する方法を示します。

オブジェクト指向プログラミングでは、関連するクラスを使用しやすく継承を使用できます。 たとえば、作成した`Instructor`と`Student`から派生したクラス、`Person`基本クラスです。 Entity Framework では、同じ種類のエンティティ間の継承構造を作成できます。

このチュートリアルでは、新しい web ページを作成できません。 代わりに、データ モデルにエンティティを派生を追加して、新しいエンティティを使用して既存のページを変更します。

## <a name="table-per-hierarchy-versus-table-per-type-inheritance"></a>テーブルの種類ごとの継承ではなくテーブルは階層ごと

データベースは、1 つのテーブルまたは複数のテーブルに関連するオブジェクトに関する情報を格納できます。 たとえば、`School`データベース、`Person`テーブルには、受講者と講習においてインストラクターに 1 つのテーブルの両方の情報が含まれています。 講習においてインストラクターにのみ適用の一部の列 (`HireDate`)、受講者にのみ一部 (`EnrollmentDate`)、および両方にいくつか (`LastName`、 `FirstName`)。

[![image11](the-entity-framework-and-aspnet-getting-started-part-6/_static/image2.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image1.png)

作成するエンティティ フレームワークを構成することができます`Instructor`と`Student`から継承するエンティティ、`Person`エンティティです。 1 つのデータベース テーブルからエンティティ継承構造を生成するには、このパターンが呼び出された*テーブルの階層あたり*(TPH) 継承します。

コース、`School`データベースは、さまざまなパターンを使用します。 オンライン コースとオンサイト コースがそれぞれを指す外部キーを持つ個別のテーブルに格納されている、`Course`テーブル。 コースの両方の種類に共通の情報は格納されているだけで、`Course`テーブル。

[![image12](the-entity-framework-and-aspnet-getting-started-part-6/_static/image4.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image3.png)

Entity Framework データ モデルを構成することができるように`OnlineCourse`と`OnsiteCourse`からエンティティを継承、`Course`エンティティです。 種類ごとに、すべての種類に共通のデータを格納するテーブルを参照する独立した各テーブルの個別のテーブルからエンティティ継承構造を生成するには、このパターンが呼び出された*型ごとにテーブル*(TPT) 継承します。

一般に TPH 継承パターンでは TPT パターンが複雑な結合クエリのためにパフォーマンスを向上させる TPT 継承のパターンよりも、Entity Framework でに配信しています。 このチュートリアルでは、TPH 継承の実装方法を示します。 次の手順を実行することによってを実行してみましょう。

- 作成`Instructor`と`Student`から派生したエンティティ型`Person`です。
- 派生エンティティに関連するプロパティを移動、`Person`派生エンティティをエンティティです。
- 派生型では、プロパティに制約を設定します。
- ように、`Person`エンティティ抽象エンティティです。
- 各マップの派生エンティティを`Person`を決定する方法を指定する条件を持つテーブルかどうか、`Person`行の派生型を表します。

## <a name="adding-instructor-and-student-entities"></a>インストラクターと学生エンティティの追加

開く、 <em>SchoolModel.edmx</em>ファイルをデザイナーで使用されていない領域を右クリックして<strong>追加</strong>選択してから、<strong>エンティティ</strong><em>です。</em>

[![image01](the-entity-framework-and-aspnet-getting-started-part-6/_static/image6.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image5.png)

**エンティティの追加** ダイアログ ボックスに、名前、エンティティ`Instructor`設定とその**基本型**オプションを`Person`です。

[![image02](the-entity-framework-and-aspnet-getting-started-part-6/_static/image8.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image7.png)

**[OK]** をクリックします。 デザイナーを作成、`Instructor`から派生するエンティティ、`Person`エンティティです。 新しいエンティティがないすべてのプロパティです。

[![image03](the-entity-framework-and-aspnet-getting-started-part-6/_static/image10.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image9.png)

作成する手順を繰り返して、`Student`からも派生エンティティ`Person`です。

のみからそのプロパティに移動する必要があります、インストラクターがある雇用日、`Person`エンティティを`Instructor`エンティティです。 `Person` 、エンティティを右クリックし、`HireDate`プロパティをクリックして**切り取り**です。 右クリックし、**プロパティ**で、`Instructor`エンティティとクリック**貼り付け**です。

[![image04](the-entity-framework-and-aspnet-getting-started-part-6/_static/image12.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image11.png)

入社日、`Instructor`エンティティを null にすることはできません。 右クリックし、`HireDate`プロパティ、をクリックして**プロパティ**、し、、**プロパティ**ウィンドウ変更`Nullable`に`False`です。

[![image05](the-entity-framework-and-aspnet-getting-started-part-6/_static/image14.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image13.png)

移動する手順を繰り返して、`EnrollmentDate`プロパティから、`Person`エンティティを`Student`エンティティです。 設定することを確認してください`Nullable`に`False`の`EnrollmentDate`プロパティです。

これで、`Person`エンティティに共通するプロパティのみ`Instructor`と`Student`エンティティ (ナビゲーション プロパティ、移動していない) とは別のエンティティのみ使用できます、継承構造の基本エンティティにします。 そのため、個別のエンティティとして扱われますしないことを確認する必要があります。 右クリックし、`Person`エンティティで、**プロパティ**、し、**プロパティ**ウィンドウの値を変更する、**抽象**プロパティを**True**です。

[![image06](the-entity-framework-and-aspnet-getting-started-part-6/_static/image16.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image15.png)

## <a name="mapping-instructor-and-student-entities-to-the-person-table"></a>Person テーブルをインストラクターと学生エンティティのマッピング

Entity Framework を区別する方法を指示する必要があります`Instructor`と`Student`データベース内のエンティティです。

右クリックし、`Instructor`エンティティと選択**テーブル マッピング**です。 **マッピングの詳細**ウィンドウで、をクリックして**テーブルまたはビューの追加**を選択し、**人**です。

[![image07](the-entity-framework-and-aspnet-getting-started-part-6/_static/image18.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image17.png)

をクリックして**条件を追加する**、し、 **HireDate**です。

[![image09](the-entity-framework-and-aspnet-getting-started-part-6/_static/image20.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image19.png)

変更**演算子**に**は**と**値/プロパティ**に**Not Null**です。

[![image10](the-entity-framework-and-aspnet-getting-started-part-6/_static/image22.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image21.png)

手順を繰り返して、`Students`にこのエンティティがマップされるを指定するエンティティ、`Person`時にテーブル、`EnrollmentDate`列は null ではありません。 保存して、データ モデルを終了します。

クラスの新しいエンティティを作成し、デザイナーで使用できるようにするためにプロジェクトをビルドします。

## <a name="using-the-instructor-and-student-entities"></a>講師および学生エンティティを使用

Student とインストラクター データでデータをバインドする作業に web ページを作成したときに、`Person`エンティティ セットとでフィルター処理、`HireDate`または`EnrollmentDate`受講者または講習においてインストラクターに返されるデータを制限するプロパティです。 ただし、ここでバインドすると各データ ソースの管理に、`Person`エンティティ セットにのみ指定できます`Student`または`Instructor`エンティティの種類を選択する必要があります。 Entity Framework は、受講者および講習においてインストラクターに区別する方法を認識しているため、`Person`エンティティ セットを削除する、`Where`プロパティの設定を行うには手動で入力しました。

Visual Studio デザイナーで、エンティティの種類を指定できます、`EntityDataSource`でコントロールを選択する必要があります、 **EntityTypeFilter**のドロップダウン ボックス、`Configure Data Source`ウィザードで、次の例で示すようにします。

[![image13](the-entity-framework-and-aspnet-getting-started-part-6/_static/image24.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image23.png)

および、**プロパティ**を除去するウィンドウ`Where`不要になったために必要な次の例で示すように句の値。

[![image14](the-entity-framework-and-aspnet-getting-started-part-6/_static/image26.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image25.png)

ただしのマークアップを変更したため`EntityDataSource`使用するコントロールを`ContextTypeName`実行することはできません、属性、**データ ソースの構成**でウィザード`EntityDataSource`を既に作成済みのコントロールです。 そのため、代わりにマークアップを変更することによって必要な変更を行うします。

開く、 *Students.aspx*ページ。 `StudentsEntityDataSource`コントロールを削除、`Where`属性を追加、`EntityTypeFilter="Student"`属性。 マークアップは、次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample1.aspx)]

設定、`EntityTypeFilter`属性により、`EntityDataSource`コントロールは、指定されたエンティティ型だけを選択します。 両方を取得する場合は`Student`と`Instructor`エンティティ型の場合は、この属性を設定しません。 (いずれかで複数のエンティティ型を取得するオプションがある`EntityDataSource`読み取り専用データ アクセスのため、コントロールを使用している場合にのみを制御します。 使用する場合、`EntityDataSource`挿入、更新、またはエンティティの削除を制御しにバインドされているエンティティ セットには、複数の種類を含めることができる場合、のみ、1 つのエンティティ型を使用することができ、この属性を設定する必要があります)。

手順を繰り返して、`SearchEntityDataSource`制御の一部のみを削除する点を除いて、`Where`属性を選択する`Student`プロパティを完全に削除せずにエンティティです。 コントロールの開始タグは、次の例のようになります。

[!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample2.aspx)]

動作の確認、引き続き以前と同じようにページを実行します。

[![image15](the-entity-framework-and-aspnet-getting-started-part-6/_static/image28.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image27.png)

新しいを使用するように、前のチュートリアルで作成した次のページを更新`Student`と`Instructor`の代わりにエンティティ`Person`エンティティ、それらを実行する前に、同様に動作することを確認します。

- *StudentsAdd.aspx*、追加`EntityTypeFilter="Student"`を`StudentsEntityDataSource`コントロール。 マークアップは、次の例のようになります。 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample3.aspx)]

    [![image16](the-entity-framework-and-aspnet-getting-started-part-6/_static/image30.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image29.png)
- *About.aspx*、追加`EntityTypeFilter="Student"`を`StudentStatisticsEntityDataSource`を制御し、削除`Where="it.EnrollmentDate is not null"`です。 マークアップは、次の例のようになります。 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample4.aspx)]

    [![image17](the-entity-framework-and-aspnet-getting-started-part-6/_static/image32.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image31.png)
- *Instructors.aspx*と*InstructorsCourses.aspx*、追加`EntityTypeFilter="Instructor"`を`InstructorsEntityDataSource`を制御し、削除`Where="it.HireDate is not null"`です。 内のマークアップ*Instructors.aspx*次の例のようになります。 

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample5.aspx)]

    [![image18](the-entity-framework-and-aspnet-getting-started-part-6/_static/image34.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image33.png)

    内のマークアップ*InstructorsCourses.aspx*は次の例のようになります。

    [!code-aspx[Main](the-entity-framework-and-aspnet-getting-started-part-6/samples/sample6.aspx)]

    [![image19](the-entity-framework-and-aspnet-getting-started-part-6/_static/image36.png)](the-entity-framework-and-aspnet-getting-started-part-6/_static/image35.png)

これらの変更の結果として、いくつかの方法で、Contoso 大学アプリケーションの保守容易性を改善しました。 UI レイヤーからの選択と検証ロジックを移動した (*.aspx*マークアップ) と、データ アクセス レイヤーの不可欠な部分です。 これにより、アプリケーション コードに加えた変更する可能性があります将来的に、データベース スキーマやデータ モデルから分離します。 たとえば、受講者が教師の補助として採用可能性があり、入社日が得られるためことを決定する可能性があります。 インストラクターから受講者を区別し、データ モデルを更新する新しいプロパティを追加できます。 受講者、雇用された日付を表示したい以外を変更する必要は web アプリケーションのコードではありません。 追加の利点の 1 つ`Instructor`と`Student`エンティティは、コードがより容易にするときに参照されているよりも理解できるものである`Person`実際に受講者をしたオブジェクトを教師です。

Entity Framework での継承パターンを実装する方法を見てきましたようになりました。 次のチュートリアルでは、Entity Framework がデータベースにアクセスするより詳細に制御するためにストアド プロシージャを使用する方法を学習します。

> [!div class="step-by-step"]
> [前へ](the-entity-framework-and-aspnet-getting-started-part-5.md)
> [次へ](the-entity-framework-and-aspnet-getting-started-part-7.md)
