---
title: ASP.NET Core MVC と EF Core - 継承 - 9/10
author: rick-anderson
description: このチュートリアルでは、ASP.NET Core アプリケーションで Entity Framework Core を使用して、データ モデル内の継承を実装する方法を説明します。
manager: wpickett
ms.author: tdykstra
ms.date: 03/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: data/ef-mvc/inheritance
ms.openlocfilehash: 1ddca97d0a68311e8c6fa793ec4245c7ffef1337
ms.sourcegitcommit: a19261eb82b948af6e4a1664fcfb8dabb16150e3
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/14/2018
---
# <a name="aspnet-core-mvc-with-ef-core---inheritance---9-of-10"></a>ASP.NET Core MVC と EF Core - 継承 - 9/10

作成者: [Tom Dykstra](https://github.com/tdykstra)、[Rick Anderson](https://twitter.com/RickAndMSFT)

Contoso University のサンプル Web アプリケーションでは、Entity Framework Core と Visual Studio を使用して ASP.NET Core MVC Web アプリケーションを作成する方法を示します。 チュートリアル シリーズについては、[シリーズの最初のチュートリアル](intro.md)を参照してください。

前のチュートリアルでは、同時実行制御の例外を処理しました。 このチュートリアルでは、データ モデルで継承を実装する方法を示します。

オブジェクト指向プログラミングでは、継承を使用してコードの再利用を容易にします。 このチュートリアルでは、`Instructor` と `Student` クラスを `Person` 基底クラスから派生するように変更します。この基底クラスはインストラクターと受講者の両方に共通な `LastName` などのプロパティを含んでいます。 どの Web ページも追加または変更しませんが、コードの一部を変更し、それらの変更はデータベースに自動的に反映されます。

## <a name="options-for-mapping-inheritance-to-database-tables"></a>継承をデータベース テーブルにマップするためのオプション

School データ モデル内の `Instructor` および `Student` クラスにはいくつかの同じプロパティがあります。

![Student クラスと Instructor クラス](inheritance/_static/no-inheritance.png)

`Instructor` エンティティと `Student` エンティティで共有されるプロパティの冗長なコードを削除すると仮定します。 または、インストラクターと学生のどちらから名前を取得したかに関係なく、名前をフォーマットできるサービスを記述するとします。 次の図に示すように、それらの共有プロパティのみが含まれる `Person` 基底クラスを作成し、`Instructor` クラスと `Student`クラスがその基底クラスから継承するようにすることができます。

![Person クラスから派生する Student クラスと Instructor クラス](inheritance/_static/inheritance.png)

データベースでこの継承構造を表すことができるいくつかの方法があります。 受講者とインストラクターの両方に関する情報を 1 つのテーブル内に含む Person テーブルを使用できます。 一部の列 (HireDate) はインストラクターのみに適用され、一部 (EnrollmentDate) は受講者のみに適用され、一部 (LastName、FirstName) は両方に適用される可能性があります。 通常、各行がどの種類を表すかを示す識別子の列があります。 たとえば、識別子列にインストラクターを示す "Instructor" と受講者を示す "Student" がある場合があります。

![Table-per-Hierarchy の例](inheritance/_static/tph.png)

1 つのデータベース テーブルからエンティティの継承構造を生成するこのパターンは、Table-per-Hierarchy (TPH) 継承と呼ばれます。

代わりに、継承構造と同じように見えるデータベースを作成することもできます。 たとえば、Person テーブルに名前フィールドのみを含め、データ フィールドが含まれる別の Instructor テーブルと Student テーブルを使用できます。

![Table-Per-Type 継承](inheritance/_static/tpt.png)

このエンティティ クラスごとにデータベース テーブルを作成するパターンは、Table-Per-Type (TPT) 継承と呼ばれます。

他のオプションとして、個々のテーブルにすべての非抽象型をマップすることもできます。 継承されたプロパティを含むクラスのすべてのプロパティは、対応するテーブルの列にマップされます。 このパターンは、Table-per-Concrete Class (TPC) 継承と呼ばれます。 前に示したように、Person、Student、および Instructor クラスの TPC 継承を実装した場合、Student テーブルと Instructor テーブルは、継承を実装した後がその前とまったく同じに見えます。

TPC および TPH 継承パターンは、一般的に TPT 継承パターンよりも高いパフォーマンスを実現します。これは、TPT パターンの結果として複雑な結合クエリになる可能性があるためです。

このチュートリアルでは、TPH 継承の実装方法を示します。 TPH は、Entity Framework Core がサポートする唯一の継承パターンです。  実行する作業として、`Person` クラスを作成し、`Instructor` および `Student` クラスを `Person` から派生するように変更し、新しいクラスを `DbContext` に追加して、移行を作成します。

> [!TIP] 
> 次の変更を加える前に、プロジェクトのコピーを保存することを検討してください。  問題が発生して最初からやり直す必要がある場合、このチュートリアルで実行した手順を逆に実行したり、すべてのシリーズの最初に戻ったりするよりも、保存したプロジェクトから開始する方が簡単です。

## <a name="create-the-person-class"></a>Person クラスの作成

[モデル] フォルダーで、Person.cs を作成し、テンプレートのコードを次のコードに変更します。

[!code-csharp[](intro/samples/cu/Models/Person.cs)]

## <a name="make-student-and-instructor-classes-inherit-from-person"></a>Person クラスから Student クラスと Instructor クラスを派生させる

*Instructor.cs* で、Person クラスから Instructor を派生させ、キーと名前のフィールドを削除します。 コードは次の例のように表示されます。

[!code-csharp[](intro/samples/cu/Models/Instructor.cs?name=snippet_AfterInheritance&highlight=8)]

*Student.cs* に同じ変更を加えます。

[!code-csharp[](intro/samples/cu/Models/Student.cs?name=snippet_AfterInheritance&highlight=8)]

## <a name="add-the-person-entity-type-to-the-data-model"></a>Person エンティティ型をデータ モデルに追加します。

Person エンティティ型を *SchoolContext.cs* に追加します。 新しい行が強調表示されます。

[!code-csharp[](intro/samples/cu/Data/SchoolContext.cs?name=snippet_AfterInheritance&highlight=19,30)]

Table-per-Hierarchy 継承を構成するために Entity Framework に必要なのことはこれですべてです。 ご覧のように、データベースが更新されたときに、Student テーブルと Instructor テーブルの代わりに、Person テーブルがあります。

## <a name="create-and-customize-migration-code"></a>移行コードの作成とカスタマイズ

変更を保存し、プロジェクトをビルドします。 次に、プロジェクト フォルダーでコマンド ウィンドウを開き、次のコマンドを入力します。

```console
dotnet ef migrations add Inheritance
```

`database update` コマンドはまだ実行しないでください。 このコマンドは、Instructor テーブルを削除し、Student テーブルの名前を Person に変更するので、コマンドの結果としてデータが失われます。 既存のデータを保持するためにカスタム コードを提供する必要があります。

*Migrations/\<timestamp>_Inheritance.cs* を開き、`Up` メソッドを次のコードに置き換えます。

[!code-csharp[](intro/samples/cu/Migrations/20170216215525_Inheritance.cs?name=snippet_Up)]

このコードは、次のデータベースの更新タスクを処理します。

* 外部キー制約と Student テーブルをポイントするインデックスを削除します。

* Instructor テーブルの名前の Person に変更し、Student データを格納するために必要な変更を加えます。

* 受講者の null 許容 EnrollmentDate を追加します。

* 行が、受講者かインストラクターかを示すために識別子列を追加します。

* 受講者行には雇用日がないので HireDate を nul 許容にします。

* 受講者をポイントする外部キーの更新に使用する一時的なフィールドを追加します。 Person テーブルに受講者をコピーするときに新しい主キー値を受け取ります。

* Student テーブルから Person テーブルにデータをコピーします。 これにより、受講者に新しい主キー値が割り当てられます。

* 受講者をポイントする外部キー値を修正します。

* 今は Person テーブルをポイントしている外部キー制約とインデックスを再作成します 

(主キーの型として整数の代わりに GUID を使用した場合は、受講者の主キー値を変更する必要はなく、これらの手順のいくつかを省略できます)。

`database update` コマンドを実行します。

```console
dotnet ef database update
```

(実稼働システムでは、以前のデータベースバージョンに戻すために `Down` メソッドを使用する必要があった場合、このメソッドに対応する変更を行います。 このチュートリアルでは、`Down` メソッドは使用しません)

> [!NOTE] 
> データが存在するデータベースでスキーマの変更を行うと、他のエラーが発生する場合があります。 解決できない移行エラーが発生した場合は、接続文字列のデータベース名を変更するか、データベースを削除できます。 新しいデータベースには移行するデータが存在しないため、update-database コマンドがエラーなしで完了する可能性が高くなります。 データベースを削除するには、SSOX を使用するか `database drop` CLI コマンドを実行します。

## <a name="test-with-inheritance-implemented"></a>継承を実装したテスト

アプリを実行して、さまざまなページを試してください。 すべてが前と同じように動作します。

**SQL Server オブジェクト エクスプローラー**で、**[データ接続/SchoolContext]** を展開し、**[テーブル]** を展開すると、Student テーブルと Instructor テーブルが Person テーブルに置き換えられていることを確認できます。 Person テーブル デザイナーを開くと、Student テーブルと Student テーブルに存在していたすべての列が表示されます。

![SSOX の Person テーブル](inheritance/_static/ssox-person-table.png)

Person テーブルを右クリックし、**[テーブル データの表示]** をクリックして識別子列を表示します。

![SSOX の Person テーブル - テーブル データ](inheritance/_static/ssox-person-data.png)

## <a name="summary"></a>まとめ

`Person`、`Student`、および `Instructor` クラスの Table-per-Hierarchy 継承を実装しました。 Entity Framework Core での継承の詳細については、「[継承](https://docs.microsoft.com/ef/core/modeling/inheritance)」を参照してください。 次のチュートリアルでは、比較的高度なさまざまな Entity Framework のシナリオを処理する方法を説明します。

> [!div class="step-by-step"]
> [前へ](concurrency.md)
> [次へ](advanced.md)  
