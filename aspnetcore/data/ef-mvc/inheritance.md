---
title: "ASP.NET MVC を持つコアを EF コア - 継承 - 9 10"
author: tdykstra
description: "このチュートリアルでは、ASP.NET Core アプリケーションに Entity Framework のコアを使用して、データ モデル内の継承を実装する方法を示します。"
ms.author: tdykstra
manager: wpickett
ms.date: 03/15/2017
ms.topic: get-started-article
ms.technology: aspnet
ms.prod: asp.net-core
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 756f1bbba73bd760f780d18c01597642dd1f7216
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/19/2018
---
# <a name="inheritance---ef-core-with-aspnet-core-mvc-tutorial-9-of-10"></a>継承の ASP.NET Core MVC のチュートリアル (10 の 9) と EF コア

によって[Tom Dykstra](https://github.com/tdykstra)と[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso 大学でサンプル web アプリケーションでは、Entity Framework のコアと Visual Studio を使用して ASP.NET Core MVC web アプリケーションを作成する方法を示します。 一連のチュートリアルについては、次を参照してください。[系列内の最初のチュートリアル](intro.md)です。

前のチュートリアルでは、同時実行例外を処理します。 このチュートリアルでは、データ モデルでの継承を実装する方法を示します。

オブジェクト指向プログラミングでは、コードの再利用を容易に継承を使用できます。 このチュートリアルでは、変更、`Instructor`と`Student`クラスから派生するように、`Person`基底クラスなどのプロパティを含む`LastName`講習においてインストラクターと受講者の両方に共通であります。 されませんを追加または web ページは変更は、コードの一部を変更しますが、し、それらの変更がデータベースに自動的に反映されます。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>継承をデータベース テーブルにマップするためのオプション

`Instructor`と`Student`School データ モデル内のクラスと同じではいくつかのプロパティがあります。

![Student とインストラクター クラス](inheritance/_static/no-inheritance.png)

共有されるプロパティを冗長なコードを削除すると、`Instructor`と`Student`エンティティです。 または、インストラクターまたは受講者から名前が付属しているかどうかをかけることがなく名を書式設定できるサービスを記述します。 作成することが、`Person`基底クラスのプロパティ、その共有だけが含まれているし、`Instructor`と`Student`クラスは、次の図に示すように、その基底クラスから継承します。

![ユーザー クラスから派生する student とインストラクター クラス](inheritance/_static/inheritance.png)

これには、データベースでこの継承構造を表すことができますいくつかの方法があります。 Person テーブル受講者と講習においてインストラクターに 1 つのテーブルの両方に関する情報を含むことができます。 一部の列は、インストラクター (HireDate)、受講者 (EnrollmentDate) 両方 (姓、名) にいくつかにのみ一部にのみ適用可能性があります。 通常、識別子の列を各行を表す種類を示す必要があります。 たとえば、識別子の列は、受講者のインストラクターと「生徒」「インストラクター」があります。

![テーブルの階層あたりの例](inheritance/_static/tph.png)

このパターンの 1 つのデータベース テーブルからエンティティ継承構造を生成するには、テーブルの階層あたり (TPH) の継承が呼び出されます。

代わりに、継承構造と同じように、データベースの作成を開始します。 たとえば、Person テーブルの名前フィールドしかありませんでした、日付フィールドを持つ別のインストラクターと Student テーブルを持つとします。

![Table-Per-Type 継承](inheritance/_static/tpt.png)

このエンティティ クラスごとにデータベース テーブルのパターンは、型 (TPT) の継承ごとにテーブルと呼ばれます。

まだ他のオプションは、個々 のテーブルにすべての非抽象型をマップするです。 継承されたプロパティを含むクラスのすべてのプロパティは、対応するテーブルの列にマップします。 このパターンは、具象型でのテーブルごとのクラス (TPC) の継承と呼ばれます。 前に示したように、ユーザー、学生、およびインストラクター クラスの継承を TPC が実装されている場合、Student テーブルとインストラクター テーブルはよりも長く前に、継承の実装後にまったく同じなります。

TPC と TPH の継承パターンは、TPT パターンが複雑な結合クエリのため通常 TPT 継承のパターンよりも優れたパフォーマンスを提供します。

このチュートリアルでは、TPH 継承の実装方法を示します。 TPH は、Entity Framework のコアをサポートする唯一の継承パターンです。  作成を行いますが、`Person`クラス、変更、`Instructor`と`Student`クラスから派生する`Person`、新しいクラスを追加、 `DbContext`、し、移行を作成します。

> [!TIP] 
> 次の変更を加える前に、プロジェクトのコピーを保存することを検討してください。  問題と最初からやり直す必要性に発生した場合、一連の先頭にこのチュートリアルの完了手順を反転することや継続的ではなく保存されているプロジェクトから開始しやすくがされます。

## <a name="create-the-person-class"></a>ユーザー クラスを作成します。

Models フォルダーに Person.cs を作成し、テンプレート コードを次のコードに置き換えます。

[!code-csharp[Main](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>ユーザーから継承 Student とインストラクター クラスを作成します。

*Instructor.cs*Person クラスから派生して、インストラクター クラス、およびキーと名前のフィールドを削除します。 コードは、次の例のようになります。

[!code-csharp[Main](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

同じ変更を加え*Student.cs*です。

[!code-csharp[Main](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Person エンティティ型をデータ モデルに追加します。

ユーザーのエンティティ型を追加*SchoolContext.cs*です。 新しい行が強調表示されます。

[!code-csharp[Main](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

これは、Entity Framework は、テーブルの階層あたりの継承を構成するために必要なです。 表示されます、データベースが更新されたときに、Student テーブルとインストラクター テーブルの代わりに、Person テーブルがあります。

## <a name="create-and-customize-migration-code"></a>作成し、移行のコードをカスタマイズします。

変更を保存し、プロジェクトをビルドします。 プロジェクト フォルダー内のコマンド ウィンドウを開くし、次のコマンドを入力します。

```console
dotnet ef migrations add Inheritance
```

実行しない、`database update`まだコマンドします。 インストラクター テーブルを削除し、ユーザーに、Student テーブルの名前を変更ことはために、そのコマンドはデータの損失になります。 既存のデータを保持するためにカスタム コードを提供する必要があります。

開いている*移行/\<タイムスタンプ > _Inheritance.cs*と置換、`Up`メソッドを次のコード。

[!code-csharp[Main](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

このコードは、次のデータベースの更新タスクの処理します。

* 外部キー制約と Student テーブルをポイントするインデックスを削除します。

* 個人として教師テーブルの名前を変更し、受講者用のデータを格納するために必要な変更を行います。

* 受講者の null 許容 EnrollmentDate を追加します。

* 行が、受講者または講師がかどうかを示すために識別子列を追加します。

* により HireDate null 許容ため学生行は、雇用日を必要はありません。

* 受講者をポイントする外部キーの更新に使用される一時的なフィールドを追加します。 Person テーブルに受講者をコピーするときに、新しい主キー値が表示されます。

* Person テーブルに、Student テーブルからデータをコピーします。 これにより、受講者が割り当てられている新しい主キー値を取得します。

* 受講者をポイントする外部キー値を修正します。

* 外部キー制約と今すぐ Person テーブルをポイントして、インデックスを再作成されます。

(学生主キーの値を変更する必要はない場合は、プライマリ キーの種類として、整数ではなく GUID を使用した、および省略されていることができいくつかの手順です。)

実行、`database update`コマンド。

```console
dotnet ef database update
```

(実稼働システムで対応する変更を行い、`Down`メソッドの場合も、以前のデータベース バージョンに戻るに使用しなければならなかったことです。 このチュートリアルでは使用しない、`Down`メソッドです)。

> [!NOTE] 
> 既存のデータを含むデータベースでスキーマ変更を行うときにその他のエラーが発生することができます。 解決できない場合は移行エラーが発生した場合か、接続文字列でデータベース名を変更したり、データベースを削除できます。 新しいデータベースを移行するデータが存在しないと、データベースの更新コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除する SSOX を使用してまたはを実行、 `database drop` CLI コマンド。

## <a name="test-with-inheritance-implemented"></a>継承の実装をテストします。

アプリを実行して、さまざまなページを再試行してください。 前に、と同じに動作します。

**SQL Server オブジェクト エクスプ ローラー**、展開**データ接続/SchoolContext**し**テーブル**、Student テーブルとインストラクター テーブルに置換されたことを確認して、Person テーブル。 Person テーブル デザイナーを開くし、Student テーブルとインストラクター テーブルに存在するために使用される列のすべてがあるを参照してください。

![SSOX で person テーブル](inheritance/_static/ssox-person-table.png)

Person テーブルを右クリックし、をクリックして**テーブル データの表示**識別子列を表示します。

![SSOX - テーブルのデータで person テーブル](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>まとめ

テーブルの階層あたりの継承を実装したら、 `Person`、 `Student`、および`Instructor`クラスです。 Entity Framework Core での継承の詳細については、次を参照してください。[継承](https://docs.microsoft.com/ef/core/modeling/inheritance)です。 次のチュートリアルでは、さまざまな Entity Framework の比較的高度なシナリオを処理する方法が表示されます。

>[!div class="step-by-step"]
[前へ](concurrency.md)
[次へ](advanced.md)  
