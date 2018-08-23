---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 2: ビジネス ロジック層と単体テストの追加 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズでは、Entity Framework 4.0 のチュートリアル シリーズの概要を作成した Contoso University web アプリケーションに基づいています。 ここには.
ms.author: riande
ms.date: 01/26/2011
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: 6517f037a03bb520ee4f3b3185a255b0eaa9de9a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831937"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Entity Framework 4.0 と ObjectDataSource コントロールを使用して、パート 2: ビジネス ロジック層と単体テストの追加
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズは、Contoso University web アプリケーションによって作成される、 [、Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)チュートリアル シリーズです。 前のチュートリアルを完了していない場合は、このチュートリアルの開始点としてできます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)に、作成します。 できます[アプリケーションをダウンロードする](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)完全なチュートリアル シリーズで作成します。 チュートリアルについて質問等がございましたらを投稿できます、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)します。


前のチュートリアルでは、Entity Framework を使用して n 層 web アプリケーションを作成し、`ObjectDataSource`コントロール。 このチュートリアルでは、ビジネス ロジック層 (BLL) とデータ アクセス層 (DAL) を個別に維持しながら、ビジネス ロジックを追加する方法と、BLL の自動化された単体テストを作成する方法を示します。

このチュートリアルでは、次のタスクを完了します。

- 必要なデータ アクセス メソッドを宣言するリポジトリ インターフェイスを作成します。
- リポジトリ クラスには、リポジトリ インターフェイスを実装します。
- データ アクセス機能を実行するリポジトリ クラスを呼び出すビジネス ロジック クラスを作成します。
- 接続、`ObjectDataSource`にリポジトリ クラスの代わりに、ビジネス ロジック クラスを制御します。
- 単体テスト プロジェクトとそのデータ ストアのメモリ内コレクションを使用するリポジトリ クラスを作成します。
- テストを実行して失敗を確認し、ビジネス ロジック クラスに追加するビジネス ロジックの単体テストを作成します。
- ビジネス ロジック クラスでビジネス ロジックを実装し、テストし、か確認して、ユニットを再実行します。

使用する、 *Departments.aspx*と*DepartmentsAdd.aspx*前のチュートリアルで作成したページです。

## <a name="creating-a-repository-interface"></a>リポジトリ インターフェイスを作成します。

リポジトリ インターフェイスの作成から始めます。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

*DAL*フォルダー、新しいクラス ファイルを作成、名前を付けます*ISchoolRepository.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

インターフェイスの各 CRUD の 1 つのメソッドを定義します (作成、読み取り、更新、削除)、リポジトリ クラスで作成したメソッド。

`SchoolRepository`クラス*SchoolRepository.cs*、このクラスで実装するかを示す、`ISchoolRepository`インターフェイス。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>ビジネス ロジック クラスを作成します。

次に、ビジネス ロジック クラスを作成します。 によって実行されるビジネス ロジックを追加できるように、これは、`ObjectDataSource`まだ行いますされませんが、制御します。 ここでは、新しいビジネス ロジック クラスは、リポジトリは通常の CRUD 操作のみを実行します。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

新しいフォルダーを作成し、名前*BLL*します。 (実際のアプリケーションでこれらのビジネス ロジック層は通常、クラス ライブラリとして実装する、別のプロジェクト: に、このチュートリアルを簡潔にするには、BLL クラスをプロジェクト フォルダーに保存されますが、)。

*BLL*フォルダー、新しいクラス ファイルを作成、名前を付けます*SchoolBL.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

このコードは、前のリポジトリ クラス、見た同じ CRUD メソッドを作成しますが、Entity Framework メソッドに直接アクセスするのではなく、リポジトリ クラス メソッドを呼び出します。

リポジトリ クラスへの参照を保持するクラスの変数がインターフェイス型として定義されているし、2 つのコンス トラクターに、リポジトリ クラスをインスタンス化するコードが含まれています。 パラメーターなしのコンス トラクターで使用される、`ObjectDataSource`コントロール。 インスタンスを作成、`SchoolRepository`先ほど作成したクラス。 その他のコンス トラクターは、リポジトリ インターフェイスを実装する任意のオブジェクトを渡すためのビジネス ロジック クラスをインスタンス化するコードをすべてできます。

リポジトリのクラスと 2 つのコンス トラクターを呼び出す CRUD メソッドを使用すれば、どのようなバックエンド データ ストアを選択すると、ビジネス ロジック クラスを使用できます。 ビジネス ロジック クラスは、呼び出し元のクラスが、データを保持する方法を認識する必要はありません。 (これは多くの場合に呼び出されます*永続化非依存*)。単純なものを使用するリポジトリの実装にビジネス ロジック クラスを接続できるため、単体テストを容易にインメモリとして`List`データを格納するコレクション。

> [!NOTE]
> Entity Framework の継承クラスからそれらをインスタンス化しているため、エンティティ オブジェクトがまだない永続化非依存には技術的には、`EntityObject`クラス。 使用することができますの完了の永続化非依存、 *plain old CLR object*、または*Poco*から継承されるオブジェクトの代わりに、`EntityObject`クラス。 このチュートリアルの範囲を超えては Poco を使用します。 詳細については、次を参照してください[テストの容易性と Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) 、MSDN web サイトです。)。


接続できるので、`ObjectDataSource`コントロールをビジネス ロジックは、クラスの代わりに、リポジトリにし前に、と同様の動作を確認します。

*Departments.aspx*と*DepartmentsAdd.aspx*、出現するたびに変更`TypeName="ContosoUniversity.DAL.SchoolRepository"`に`TypeName="ContosoUniversity.BLL.SchoolBL`"。 (4 つのインスタンスにあるすべてです。)

実行、 *Departments.aspx*と*DepartmentsAdd.aspx*ページまでと同じように動作することを確認します。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>単体テスト プロジェクトとリポジトリの実装を作成します。

ソリューションを使用して、新しいプロジェクトを追加、**テスト プロジェクト**テンプレート、名前を付けます`ContosoUniversity.Tests`します。

参照を追加、テスト プロジェクトで`System.Data.Entity`への参照をプロジェクトに追加し、`ContosoUniversity`プロジェクト。

単体テストで使用するリポジトリ クラスを作成できます。 このリポジトリのデータ ストアは、クラス内になります。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

新しいクラス ファイルを作成、テスト プロジェクトで、名前を付けます*MockSchoolRepository.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

このリポジトリのクラスは Entity Framework に直接アクセスすると同じ CRUD メソッドを含んでいますが、それらが扱う`List`データベースではなく、メモリ内コレクション。 これにより、テスト クラスを設定し、ビジネス ロジック クラスの単体テストを検証しやすくなります。

## <a name="creating-unit-tests"></a>単体テストの作成

**テスト**プロジェクト テンプレートが、スタブの単体テスト クラスを作成し、次のタスクにビジネス ロジック クラスに追加するビジネス ロジックの単体テスト メソッドを追加してこのクラスを変更するのには。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso 大学、個々 の講師が 1 つの部門の管理者をできるだけと、この規則を適用するビジネス ロジックを追加する必要があります。 テストを追加し、失敗することを確認するテストを実行しているから始めます。 コードを追加し、期待どおりに渡すこと、テストを再実行します。

開く、 *UnitTest1.cs*ファイルし、追加`using`ContosoUniversity プロジェクトで作成したビジネス ロジックとデータ アクセス層のステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

置換、`TestMethod1`次のメソッドを持つメソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL`メソッドは、単体テスト プロジェクトで、その後、ビジネス ロジック クラスの新しいインスタンスを渡します用に作成したリポジトリ クラスのインスタンスを作成します。 メソッドは、ビジネス ロジック クラスを使用してテスト メソッドで使用できる 3 つの部門を挿入します。

テスト メソッドは、新しい部門は、既存の部門と同じ管理者の挿入を試行した場合、または部門の管理者をユーザーの ID に設定することで更新を試行した場合、ビジネス ロジック クラスは例外をスローします。 を確認します。既に別の部門の管理者は誰です。

まだ、このコードはコンパイルされませんので例外クラスを作成していません。 コンパイルすることを取得するを右クリックして`DuplicateAdministratorException`選択**生成**、し**クラス**します。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

削除することができるテスト プロジェクトにクラスが作成されます例外クラスは、メインのプロジェクトで作成した後。 ビジネス ロジックを実装します。

テスト プロジェクトを実行します。 予想どおり、テストが失敗します。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>テスト成功するビジネス ロジックの追加

次に、部門の管理者は既に別の部門の管理者ユーザーに設定することはできませんが、ビジネス ロジックを実装します。 ビジネス ロジック層から例外をスローし、それをキャッチ、プレゼンテーション層の場合は、ユーザーは、部署を編集しをクリックして**Update**既に管理者であるユーザーが選択した後。 (Instructors ページをレンダリングする前に、管理者は既にドロップダウン リストから削除することも可能性がありますが、今回の目的は、ビジネス ロジック層を使用する)。

まずユーザーがインストラクターに 1 つ以上の部門の管理者を作成するときにスローします例外クラスを作成します。 メインのプロジェクトで新しいクラス ファイルを作成、 *BLL*フォルダー、という名前を付けます*DuplicateAdministratorException.cs*、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

一時的なを削除するようになりました*DuplicateAdministratorException.cs*先ほど作成したテスト プロジェクトでコンパイルするにはファイル。

メインのプロジェクトで開き、 *SchoolBL.cs*ファイルを開き、検証ロジックを含む次のメソッドを追加します。 (コードは、後で作成するメソッドを参照)。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

挿入または更新する場合は、このメソッドを呼び出すことが`Department`別の部署が既に同じ管理者かどうかを確認するためにエンティティ。

コードは、のデータベースを検索するメソッドを呼び出す、`Department`同じエンティティ`Administrator`エンティティとプロパティの値が挿入または更新されました。 1 つが見つかった場合、コードは例外をスローします。 検証チェックは必要ありません、エンティティを挿入または更新されたにない場合`Administrator`値、および例外は、更新中に、メソッドが呼び出された場合にスローされると、`Department`エンティティの一致が見つかりませんでした、`Department`更新されるエンティティ。

新しいメソッドを呼び出し、`Insert`と`Update`メソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

*ISchoolRepository.cs*、新しいデータ アクセス メソッドの次の宣言を追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

*SchoolRepository.cs*、次の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

*SchoolRepository.cs*、次の新しいデータ アクセス メソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

このコードを取得`Department`を指定した管理者を持つエンティティ。 1 つだけの部門はする必要があります (ある場合) にあります。 ただし、データベースに組み込まれている制約はありません、ためには、複数の部門が見つかった場合に戻り値の型コレクションは。

既定では、オブジェクト コンテキストは、データベースからエンティティを取得するときの記録に、オブジェクトの状態マネージャーでします。 `MergeOption.NoTracking`パラメーターを指定する、この追跡はこのクエリの実行できません。 これは、クエリを更新しようとしている正確なエンティティを返す可能性がありますので、必要としないことができますにそのエンティティをアタッチします。 履歴の部署を編集する場合など、 *Departments.aspx*ページと、管理者が変更されないように、このクエリは史学部を返します。 場合`NoTracking`がオブジェクト コンテキストは既に存在して履歴 department エンティティ、オブジェクトの状態マネージャーで、設定されていません。 ビューステートから再作成される履歴 department エンティティをアタッチするときにオブジェクト コンテキストはことを示す例外をスローし、`"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`します。

(を指定する代わりとして`MergeOption.NoTracking`、このクエリのためだけに新しいオブジェクト コンテキストを作成できます。 新しいオブジェクト コンテキストが必要で、独自のオブジェクト状態マネージャーがあります、ためにがなくなるの競合を呼び出すとき、`Attach`メソッド。 新しいオブジェクト コンテキスト共有の場合メタデータとデータベースの接続元のオブジェクト コンテキストにためこの代替アプローチのパフォーマンスの低下を最小限になります。 ただし、ここに示したアプローチが導入されて、`NoTracking`を他のコンテキストで便利なオプションです。 `NoTracking`オプションが説明したこのシリーズの後のチュートリアルでさらにします)。

テスト プロジェクトで追加する新しいデータ アクセス メソッド*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

このコードでは、LINQ を使用して、同じデータの選択を実行する、`ContosoUniversity`プロジェクト リポジトリが使用する LINQ to Entities の。

テスト プロジェクトをもう一度実行します。 今回はテストに合格します。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>ObjectDataSource の例外の処理

`ContosoUniversity`プロジェクト、実行、 *Departments.aspx*ページし、は既に別の部門の管理者ユーザーに部門の管理者を変更しようとしています。 (部門は、このチュートリアルでは、中に追加した無効なデータを事前に読み込まれたデータベースが含まれているために編集のみできることに注意してください)。次のサーバー エラー ページを取得します。

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

エラー処理コードを追加する必要があるために、この種のエラー ページを参照してくださいさせない。 開いている*Departments.aspx*のハンドラーを指定し、`OnUpdated`のイベント、`DepartmentsObjectDataSource`します。 `ObjectDataSource`次の例では、開始タグを今すぐに似ています。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

*Departments.aspx.cs*、次の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

次のハンドラーを追加、`Updated`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

場合、`ObjectDataSource`コントロールは、更新を実行するときに例外をキャッチ、例外イベントの引数を渡します (`e`) このハンドラーにします。 ハンドラーのコードは、例外が重複する管理者の例外かどうかを確認します。 コードが、エラー メッセージを含む検証コントロールを作成する場合は、`ValidationSummary`を表示するコントロール。

ページを実行し、ユーザーが 2 つの部門の管理者を再びようにしようとしてください。 この時間、`ValidationSummary`コントロールには、エラー メッセージが表示されます。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

同様に変更する、 *DepartmentsAdd.aspx*ページ。 *DepartmentsAdd.aspx*のハンドラーを指定、`OnInserted`のイベント、`DepartmentsObjectDataSource`します。 結果として得られるマークアップは、次の例では、ようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

*DepartmentsAdd.aspx.cs*、同じ追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

次のイベント ハンドラーを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

今すぐテストすることができます、 *DepartmentsAdd.aspx.cs*ページ 1 人のユーザーには、複数の部門の管理者を実行する試行も正しく処理することを確認します。

使用するリポジトリ パターンの実装概要これが完了すると、 `ObjectDataSource` Entity Framework でのコントロール。 リポジトリ パターンとテストの容易性の詳細については、MSDN ホワイト ペーパーを参照してください。[テストの容易性と Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx)します。

次のチュートリアルでは、並べ替えとフィルター機能をアプリケーションに追加する方法を確認します。

> [!div class="step-by-step"]
> [前へ](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [次へ](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
