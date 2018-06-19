---
uid: web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
title: 'Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 2: ビジネス ロジック層と単体テストを追加する |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、Entity Framework 4.0 チュートリアル シリーズの概要を作成した Contoso 大学 web アプリケーションに基づいています。 I...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/26/2011
ms.topic: article
ms.assetid: efb0e677-10b8-48dc-93d3-9ba3902dd807
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/continuing-with-ef/using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests
msc.type: authoredcontent
ms.openlocfilehash: ecdfb2bdc546f55778ec4cc1f61aa66e129134ea
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888316"
---
<a name="using-the-entity-framework-40-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests"></a>Entity Framework 4.0 ObjectDataSource コントロールを使用して、パート 2: ビジネス ロジック層と単体テストを追加します。
====================
によって[Tom Dykstra](https://github.com/tdykstra)

> このチュートリアル シリーズのビルドによって作成される Contoso 大学 web アプリケーションで、 [Entity Framework 4.0 の概要](https://asp.net/entity-framework/tutorials#Getting%20Started)一連のチュートリアルです。 前のチュートリアルを完了していない場合このチュートリアルの開始点とすることができます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-97f8ee9a)作成したとします。 こともできます[アプリケーションをダウンロードして](https://code.msdn.microsoft.com/ASPNET-Web-Forms-6c7197aa)一連の完全なチュートリアルで作成します。 チュートリアルについて質問がある場合を投稿、 [ASP.NET Entity Framework フォーラム](https://forums.asp.net/1227.aspx)です。


前のチュートリアルでは、Entity Framework を使用して n 層の web アプリケーションを作成し、`ObjectDataSource`コントロール。 このチュートリアルは独立した (BLL) ビジネス ロジック層とデータ アクセス層 (DAL) を維持しながら、ビジネス ロジックを追加する方法を示し、BLL の自動化された単体テストを作成する方法を示します。

このチュートリアルでは、次のタスクを完了します。

- 必要なデータ アクセス メソッドを宣言しているリポジトリ インターフェイスを作成します。
- リポジトリ クラスには、リポジトリ インターフェイスを実装します。
- クラスを呼び出し、リポジトリをデータ アクセス機能を実行するビジネス ロジックのクラスを作成します。
- 接続、`ObjectDataSource`リポジトリ クラスの代わりに、ビジネス ロジック クラスを制御します。
- 単体テスト プロジェクトとをそのデータ ストアのメモリ内コレクションを使用してリポジトリ クラスを作成します。
- テストを実行して失敗することを確認し、ビジネス ロジックのクラスに追加するビジネス ロジックの単体テストを作成します。
- クラスではビジネス ロジック、ビジネス ロジックを実装し、単体テストおよびを通過するを参照してくださいを再実行します。

演習では、 *Departments.aspx*と*DepartmentsAdd.aspx*前のチュートリアルで作成したページ。

## <a name="creating-a-repository-interface"></a>リポジトリ インターフェイスを作成します。

リポジトリ インターフェイスの作成から始めます。

[![Image08](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image2.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image1.png)

*DAL*フォルダーで、新しいクラス ファイルを作成、名前を付けます*ISchoolRepository.cs*、し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample1.cs)]

インターフェイスでは、1 つのメソッドを定義ごと、CRUD (作成、読み取り、更新、削除) リポジトリ クラスで作成したメソッド。

`SchoolRepository`クラス*SchoolRepository.cs*、このクラスで実装するかを示す、`ISchoolRepository`インターフェイス。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample2.cs)]

## <a name="creating-a-business-logic-class"></a>ビジネス ロジックのクラスを作成します。

次に、ビジネス ロジックのクラスを作成します。 これを行うによって実行されるビジネス ロジックを追加できるように、`ObjectDataSource`ではありませんをまだを制御します。 ここでは、新しいビジネス ロジックのクラスは、リポジトリは同じの CRUD 操作のみを実行します。

[![Image09](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image4.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image3.png)

新しいフォルダーを作成し、名前を付けます*BLL*です。 (実際のアプリケーションでこれらのビジネス ロジック層は通常、クラス ライブラリとして実装する: 別のプロジェクト: このチュートリアルを簡潔にするに BLL クラスをプロジェクト フォルダーに保存されますが、します)。

*BLL*フォルダーで、新しいクラス ファイルを作成、名前を付けます*SchoolBL.cs*、し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample3.cs)]

このコードは、リポジトリ クラスの前半という同じ CRUD メソッドを作成しますが、Entity Framework のメソッドを直接アクセスするには、代わりに、リポジトリ クラス メソッドを呼び出します。

リポジトリ クラスへの参照を保持するクラスの変数は、インターフェイス型として定義され、2 つのコンス トラクターでリポジトリ クラスをインスタンス化するコードが含まれます。 パラメーターなしのコンス トラクターで使用される、`ObjectDataSource`コントロール。 インスタンスを作成、`SchoolRepository`先ほど作成したクラスです。 その他のコンス トラクターでは、リポジトリ インターフェイスを実装する任意のオブジェクトを渡すビジネス ロジックのクラスをインスタンス化するコードをすべてを使用します。

リポジトリのクラスと 2 つのコンス トラクターを呼び出す CRUD メソッドを使用すればを選択する任意のバックエンド データ ストアとビジネス ロジックのクラスを使用できます。 ビジネス ロジックのクラスは、クラスを呼び出しているが、データを保持する方法について注意する必要はありません。 (これは多くの場合と呼ばれる*持続性の無視が適用*)。ビジネス ロジックのクラスを単純なものを使用してリポジトリ実装に接続するため、単体テストを容易にインメモリとして`List`データを格納するコレクション。

> [!NOTE]
> Entity Framework から継承するクラスからそれらをインスタンス化しているためエンティティ オブジェクトがまだない永続化非依存には技術的には、`EntityObject`クラスです。 使用できる完全な持続性の無視が適用、 *plain old CLR object*、または*した poco から*から継承するオブジェクトの代わりに、`EntityObject`クラスです。 このチュートリアルの範囲を超えてを使用した poco からです。 詳細については、次を参照してください[テストの容易性と Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx) MSDN web サイトです。)。


これで、接続することができます、`ObjectDataSource`にビジネス ロジックのコントロール クラスの代わりに、リポジトリへの動作を確認すべて以前と同じようです。

*Departments.aspx*と*DepartmentsAdd.aspx*、変更するたびに`TypeName="ContosoUniversity.DAL.SchoolRepository"`に`TypeName="ContosoUniversity.BLL.SchoolBL`"です。 (が 4 つのインスタンスすべて。)

実行、 *Departments.aspx*と*DepartmentsAdd.aspx*ページがまだ動作する前に、と同じことを確認します。

[![Image01](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image6.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image5.png)

[![Image02](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image8.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image7.png)

## <a name="creating-a-unit-test-project-and-repository-implementation"></a>単体テスト プロジェクトとリポジトリの実装を作成します。

新しいプロジェクトを使用して、ソリューションを追加、**テスト プロジェクト**テンプレート、名前を付けます`ContosoUniversity.Tests`です。

テスト プロジェクトに参照を追加`System.Data.Entity`への参照をプロジェクトに追加し、`ContosoUniversity`プロジェクト。

単体テストで使用するリポジトリ クラスを作成できます。 このリポジトリのデータ ストアは、クラスになります。

[![Image12](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image10.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image9.png)

テスト プロジェクトで、新しいクラス ファイルを作成、名前を付けます*MockSchoolRepository.cs*、し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample4.cs)]

このリポジトリ クラスは、Entity Framework を直接にアクセスするものと同じ CRUD メソッドを含んでいますが、扱う`List`の代わりに、データベースとのメモリ内のコレクション。 設定し、ビジネス ロジックのクラスの単体テストを検証するテスト クラスのやすくなります。

## <a name="creating-unit-tests"></a>単体テストの作成

**テスト**プロジェクト テンプレートが、スタブ単体テスト クラスを作成し、次のタスクは、ビジネス ロジックのクラスに追加するビジネス ロジックに単体テスト メソッドを追加してこのクラスを変更することです。

[![Image13](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image12.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image11.png)

Contoso 大学で、個々 の講師が 1 つの部門の管理者をできるだけと、この規則を強制するビジネス ロジックを追加する必要があります。 テストを追加し、失敗することを確認するテストを実行してから始めます。 コードを追加し、期待どおりに渡すこと、テストを再実行します。

開く、 *UnitTest1.cs*ファイルし、追加`using`ContosoUniversity プロジェクトで作成したビジネス ロジックとデータ アクセス レイヤーのステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample5.cs)]

置換、`TestMethod1`以下の方法でメソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample6.cs)]

`CreateSchoolBL`メソッドは、単体テスト プロジェクトで、その後、ビジネス ロジックのクラスの新しいインスタンスを渡します用に作成したリポジトリ クラスのインスタンスを作成します。 メソッドは、ビジネス ロジックのクラスを使用してテスト メソッドで使用できる 3 つの部門を挿入します。

テスト メソッドは、ビジネス ロジックのクラスは、新しい部門、同じ管理者が既存の部署、としてを挿入しようとしています。 場合、またはユーザーの ID に設定することによって、部門の管理者を更新しようとしました。 例外をスローを確認してください。ユーザーは、別の部門の管理者では既にです。

このコードはコンパイルされません例外クラスをまだ作成されていません。 コンパイルすることを得るを右クリックして`DuplicateAdministratorException`選択**生成**、し**クラス**です。

[![Image14](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image14.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image13.png)

これは、削除することができるテスト プロジェクトでクラスが作成されますメイン プロジェクトでの例外クラスを作成した後にします。 ビジネス ロジックを実装します。

テスト プロジェクトを実行します。 期待どおりに、テストが失敗します。

[![Image03](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image16.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image15.png)

## <a name="adding-business-logic-to-make-a-test-pass"></a>テストを作成するビジネス ロジックの追加

次に、不可能では既に別の部門の管理者ユーザー、部門の管理者として設定するビジネス ロジックを実装します。 ビジネス ロジック層から例外をスローし、それをキャッチ プレゼンテーション層で、ユーザーの部門を編集してクリックすると**更新**誰か、管理者を選択した後です。 (、ページを表示する前に、管理者は既にドロップ ダウン リストから講習においてインストラクターを削除することもできますが、目的をここでは、ビジネス ロジック層を使用する)。

まず、ユーザーが、インストラクターに 1 つ以上の部門の管理者を作成しようとするときにスローするを例外クラスを作成します。 メイン プロジェクトで、新しいクラス ファイルを作成、 *BLL*フォルダー、名前を付けます*DuplicateAdministratorException.cs*、し、既存のコードを次のコードに置き換えます。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample7.cs)]

一時的な削除*DuplicateAdministratorException.cs*先ほど作成したテストのプロジェクトをコンパイルできるようにファイル。

メイン プロジェクトで、開く、 *SchoolBL.cs*ファイルし、検証ロジックを含む次のメソッドを追加します。 (コードは、後で作成するメソッドを指します)。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample8.cs)]

挿入または更新するときは、このメソッドを呼び出すあります`Department`別の部門は既に同じ管理者かどうかを確認するためにエンティティです。

コードは、データベースで検索するメソッドを呼び出す、`Department`が同じエンティティ`Administrator`エンティティとプロパティの値が挿入または更新されました。 1 つが見つかった場合、コードは例外をスローします。 検証チェックは必要ありません挿入または更新されたエンティティを持たない場合`Administrator`値、および例外はありませんは、更新中に、メソッドが呼び出された場合にスローされると、`Department`エンティティの一致が見つかりませんでした、`Department`更新されるエンティティです。

新しいメソッドを呼び出して、`Insert`と`Update`メソッド。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample9.cs)]

*ISchoolRepository.cs*、新しいデータ アクセス メソッドの次の宣言を追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample10.cs)]

*SchoolRepository.cs*、次の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample11.cs)]

*SchoolRepository.cs*、次の新しいデータ アクセス メソッドを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample12.cs)]

このコードを取得`Department`を指定した管理者を持つエンティティ。 (存在する場合) は、1 つだけの部門を探す必要があります。 ただし、データベースに組み込まれている制約はありません、ため戻り値の型は、コレクション複数部門にある場合です。

既定では、オブジェクト コンテキストは、データベースからエンティティを取得するときの追跡にそのオブジェクトの状態マネージャーにします。 `MergeOption.NoTracking`パラメーターは、この追跡がこのクエリに対して実行されませんされることを指定します。 これは、クエリを更新しようとしている厳密なエンティティを返す場合がありしていないことができますをそのエンティティをアタッチします。 履歴の部門を編集する場合など、 *Departments.aspx*ページと、管理者が変更されないように、このクエリは履歴部門を返します。 場合`NoTracking`設定された場合、オブジェクト コンテキストは既に存在して履歴部門 エンティティのオブジェクト状態マネージャーではありません。 オブジェクト コンテキストという例外をスローした場合、状態の表示から再作成される履歴 department エンティティをアタッチする際に、`"An object with the same key already exists in the ObjectStateManager. The ObjectStateManager cannot track multiple objects with the same key"`です。

(代わりを指定する`MergeOption.NoTracking`、このクエリにのみ新しいオブジェクト コンテキストを作成する可能性があります。 新しいオブジェクト コンテキストは、独自のオブジェクト状態マネージャーがあることはない競合を呼び出すとき、`Attach`メソッドです。 新しいオブジェクト コンテキストはこの代替方法のパフォーマンスの低下が最小限に抑えるため、元のオブジェクト コンテキストにメタデータとデータベースの接続を共有するは。 ただし、ここに示す方法が導入されて、`NoTracking`オプション、他のコンテキストで役に立ちますを見つけることができます。 `NoTracking`オプションが説明したこのシリーズの後のチュートリアルでこれ以上です)。

テスト プロジェクトで、新しいデータ アクセスにメソッドを追加*MockSchoolRepository.cs*:

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample13.cs)]

このコードでは、LINQ を使用して、同じデータの選択を実行する、`ContosoUniversity`プロジェクト リポジトリが使用する LINQ to Entities のです。

テスト プロジェクトを再度実行します。 今回はテストに合格します。

[![Image04](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image18.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image17.png)

## <a name="handling-objectdatasource-exceptions"></a>ObjectDataSource 例外を処理

`ContosoUniversity`プロジェクト、実行、 *Departments.aspx*ページに変更しようとする部門の管理者は、ユーザーは既に別の部門の管理者にします。 (データベースが無効なデータを事前に読み込んでになるために、のみ、このチュートリアル中に追加した部門を編集することに注意してください)。次のサーバー エラー ページを取得します。

[![Image05](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image20.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image19.png)

しないようにするため、エラー処理コードを追加する必要があります。 この種のエラー ページを参照してください。 開いている*Departments.aspx*のハンドラーを指定し、`OnUpdated`のイベント、`DepartmentsObjectDataSource`です。 `ObjectDataSource`開始タグを今すぐには、次の例と似ています。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample14.aspx)]

*Departments.aspx.cs*、次の追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample15.cs)]

次のハンドラーを追加、`Updated`イベント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample16.cs)]

場合、`ObjectDataSource`コントロールは、更新を実行するときに例外をキャッチ、イベント引数は、例外を渡します (`e`) をこのハンドラーにします。 ハンドラーのコードは、かどうかは、例外が重複している管理者は、例外を確認します。 コードがエラー メッセージを含む検証コントロールを作成する場合は、`ValidationSummary`コントロールに表示します。

ページを実行し、ユーザーが 2 つの部門の管理者は、再度するしようとしてください。 この時間、`ValidationSummary`コントロールには、エラー メッセージが表示されます。

[![Image06](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image22.png)](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/_static/image21.png)

同様に変更を*DepartmentsAdd.aspx*ページ。 *DepartmentsAdd.aspx*のハンドラーを指定、`OnInserted`のイベント、`DepartmentsObjectDataSource`です。 結果として得られるマークアップは、次の例ようになります。

[!code-aspx[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample17.aspx)]

*DepartmentsAdd.aspx.cs*、同じ追加`using`ステートメント。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample18.cs)]

次のイベント ハンドラーを追加します。

[!code-csharp[Main](using-the-entity-framework-and-the-objectdatasource-control-part-2-adding-a-business-logic-layer-and-unit-tests/samples/sample19.cs)]

テストして、 *DepartmentsAdd.aspx.cs*ページを 1 つ以上の部門の管理者は、1 人のユーザーを実行する試行も正しく処理することを確認します。

これで完了、リポジトリ パターンを使用するための実装の概要、 `ObjectDataSource` Entity Framework でのコントロールです。 詳細については、リポジトリ パターンとテストの容易性は、MSDN のホワイト ペーパーを参照してください。[テストの容易性と Entity Framework 4.0](https://msdn.microsoft.com/library/ff714955.aspx)です。

次のチュートリアルでは、並べ替えおよびフィルター処理をアプリケーションに機能を追加する方法が表示されます。

> [!div class="step-by-step"]
> [前へ](using-the-entity-framework-and-the-objectdatasource-control-part-1-getting-started.md)
> [次へ](using-the-entity-framework-and-the-objectdatasource-control-part-3-sorting-and-filtering.md)
