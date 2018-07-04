---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: ビジネス ルール検証とモデルの構築 |Microsoft Docs
author: microsoft
description: 手順 3 では、モデルを作成すること両方のクエリを使用し、NerdDinner アプリケーション データベースを更新する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: 8e871a8c14dce80edbddb9d87dba929bf3356895
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37379022"
---
<a name="build-a-model-with-business-rule-validations"></a>ビジネス ルール検証とモデルを構築します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 3 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 3 では、モデルを作成すること両方のクエリを使用し、NerdDinner アプリケーション データベースを更新する方法を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner 手順 3: モデルの構築

モデル-ビュー-コント ローラー フレームワークでは、「モデル」という用語は、対応するドメイン ロジックと検証とビジネス ルールを統合すると、アプリケーションのデータを表すオブジェクトを指します。 モデルでは、MVC ベースのアプリケーションのさまざまな方法「心臓部」し、後で根本的に表示されるようの動作をドライブします。

ASP.NET MVC フレームワークには、任意のデータ アクセス テクノロジを使用して、開発者は、さまざまななど、モデルを実装するために豊富な .NET データのオプションから選択できます。 LINQ to エンティティ、LINQ to SQL、NHibernate、LLBLGen Pro、SubSonic、WilsonORM、または単なる生 ADO します。NET DataReaders やデータセットです。

NerdDinner アプリケーションは、LINQ to SQL を使用して、データベースの設計に忠実に対応し、いくつかのカスタム検証ロジックとビジネス ルールを追加する単純なモデルを作成するでしょう。 データ永続化の実装を簡単に単体テストにより、アプリケーションの残りの部分から抽象離れたときに役立つリポジトリ クラスを実装しますが、します。

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL は、.NET 3.5 の一部として同梱されている ORM (オブジェクト リレーショナル マッパーです)。

LINQ to SQL では、.NET クラスのコードにはデータベース テーブルにマップする簡単な方法を提供します。 NerdDinner アプリケーションの Dinner および RSVP のクラスに、データベース内で、Dinners および RSVP テーブルにマップするのにに使用します。 Dinners および RSVP のテーブルの列は、Dinner と RSVP クラスのプロパティに対応します。 Dinner および RSVP の各オブジェクトは、データベース内の Dinners または RSVP のテーブル内の個別の行を表します。

LINQ to SQL では、Dinner および RSVP を取得および更新の SQL ステートメントを手動で作成しなくてもすむようににより、データベースのデータ オブジェクト。 代わりにマップする方法との間、データベース、およびそれらの関係は、Dinner および RSVP クラスで定義します。 LINQ to SQL は私たちを操作し、それらを使用時に、実行時に使用する適切な SQL の実行ロジックを生成する処理を引き継ぎます。

VB と c# 内での LINQ の言語サポートを使用して Dinner と RSVP を取得する表現力豊かなクエリの作成、データベースのオブジェクト。 これによりデータのコードを記述する必要がありますの量を最小限にし、本当に優れたアプリケーションを構築することができます。

### <a name="adding-linq-to-sql-classes-to-our-project"></a>追加の LINQ to SQL クラスをプロジェクト

このプロジェクト内で"Models"フォルダーを右クリックして開始を選択し、**追加 -&gt;新しい項目の**メニュー コマンド。

![](build-a-model-with-business-rule-validations/_static/image1.png)

「新しい項目の追加」ダイアログが表示されます。 「データ」カテゴリでフィルター処理、内に"LINQ to SQL Classes"テンプレートを選択します。

![](build-a-model-with-business-rule-validations/_static/image2.png)

項目"NerdDinner"という名前がされ、[追加] ボタンをクリックします。 Visual Studio は、\Models ディレクトリの下で NerdDinner.dbml ファイルを追加し、LINQ to SQL オブジェクト リレーショナル デザイナーを開きます。

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>LINQ to SQL でのデータ モデル クラスの作成

LINQ to SQL では、既存のデータベース スキーマからデータ モデル クラスをすばやく作成できます。 To do がモデル化するこの NerdDinner データベースをサーバー エクスプ ローラーで開くし、テーブルを選択します。

![](build-a-model-with-business-rule-validations/_static/image4.png)

SQL デザイナー画面に、LINQ にテーブルをドラッグしたことができます。 操作の場合この LINQ to SQL と、Dinner が自動的に作成し、(データベース テーブルの列にマップされるクラス プロパティ) を持つテーブルのスキーマを使用して、RSVP クラス。

![](build-a-model-with-business-rule-validations/_static/image5.png)

既定では、LINQ to SQL デザイナー自動的に「複数」テーブルおよび列名、データベース スキーマに基づくクラスを作成するとき。 例: 上記の例で"Dinners"テーブル"Dinner"クラスが発生しました。 このクラスの名前付けでは、モデルに .NET の命名規則に一貫性のあるなってし、そのデザイナーの修正プログラムこのようにする便利な (特に多くのテーブルを追加する) 場合は、通常の検索します。 クラスまたは常にこのメソッドをオーバーライドし、任意の名前に変更できますが、デザイナーが生成されるプロパティの名前を取り消す場合。 これを行う、エンティティ/プロパティ名の行のデザイナー内での編集をするか、プロパティ グリッドを使用して変更します。

既定では、LINQ to SQL デザイナーはまた、テーブルの主キー/外部キーのリレーションシップを検査し、自動的にそれらに基づくには、「リレーションシップ関連付け」デフォルトを作成、間別のモデル クラスを作成します。 たとえば、ドラッグ、Dinners と RSVP テーブル、LINQ to SQL デザイナー上に 2 つの間の関連付けを一対多のリレーションシップが推定された、という事実に基づいて RSVP テーブルには、Dinners テーブルへの外部キーが必要がある (これで、矢印によって示されますが、デザイナーの):

![](build-a-model-with-business-rule-validations/_static/image6.png)

上記のアソシエーションでは、SQL には、開発者は、指定された予約に関連付けられている Dinner へのアクセスに使用できる RSVP クラスに厳密に型指定された"Dinner"プロパティを追加する LINQ が発生します。 開発者を取得し、特定の夕食に関連付けられている RSVP オブジェクトを更新できるようにする"RSVPs"コレクション プロパティを持つ Dinner クラスにもなります。

次に示す新しい RSVP オブジェクトを作成し、Dinner の予約したコレクションに追加するときに、Visual Studio 内での intellisense の例を確認できます。 方法 LINQ to SQL に自動的に「受理」コレクションに追加 Dinner オブジェクトに注意してください。

![](build-a-model-with-business-rule-validations/_static/image7.png)

RSVP オブジェクトを Dinner の予約したコレクションに追加することでは、to、Dinner とデータベースに RSVP 行の間の外部キー リレーションシップに関連付ける SQL、LINQ を指示します。

![](build-a-model-with-business-rule-validations/_static/image8.png)

デザイナーのモデル化やという名前のテーブルの関連付けの方法が気をオーバーライドできます。 デザイナー内での関連付けの矢印をクリックし、名前の変更、削除または変更するには、プロパティ グリッドを使用してそのプロパティにアクセスだけです。 NerdDinner アプリケーションでは、既定のアソシエーション ルールが構築されたデータ モデル クラスにも作業して、次の既定の動作を使用できます。

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext クラス

Visual Studio では、モデルと LINQ to SQL デザイナーを使用して定義されているデータベースのリレーションシップを表す .NET クラスを自動的に作成されます。 各 LINQ to SQL デザイナー ファイルをソリューションに追加の LINQ to SQL DataContext クラスが生成されます。 SQL クラスの項目"NerdDinner"に、LINQ、という名前であるために、作成された DataContext クラスには、"NerdDinnerDataContext"が呼び出されます。 NerdDinnerDataContext クラスは、データベースと対話する主な方法です。

NerdDinnerDataContext クラスは、データベース内でモデル化 2 つのテーブルを表す"Dinners"と"RSVPs"- - 2 つのプロパティを公開します。 C# を使用しますと、Dinner と RSVP のオブジェクトをキャッシュにクエリをデータベースからこれらのプロパティに対する LINQ クエリを記述します。

次のコードでは、NerdDinnerDataContext オブジェクトをインスタンス化し、今後の予定 Dinners のシーケンスを取得することに対して LINQ クエリを実行する方法を示します。 Visual Studio は、LINQ クエリの作成時に intellisense の全機能を提供し、そこから返されるオブジェクトは厳密に型指定されたも intellisense をサポートします。

![](build-a-model-with-business-rule-validations/_static/image9.png)

Dinner および RSVP オブジェクトを照会することを許可するだけでなく、NerdDinnerDataContext も自動的に、その後、Dinner と RSVP オブジェクトを取得しますに加えた変更を追跡します。 この機能を使用して、-明示的な SQL 更新プログラム コードを記述することがなく元のデータベースに簡単に変更を保存することができます。

たとえば、次のコードでは、LINQ クエリを使用して、データベースから 1 つの Dinner オブジェクトを取得、Dinner のプロパティ の 2 つの更新、および元のデータベースに変更を保存する方法を示しています。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

前のコードで NerdDinnerDataContext オブジェクトは、そこから取得した Dinner オブジェクトに対して行われたプロパティの変更を自動的に追跡します。 私たちには、"SubmitChanges()"メソッドが呼び出されると、更新された値を保持するデータベースに適切な SQL「更新」ステートメントが実行されます。

### <a name="creating-a-dinnerrepository-class"></a>ときは必ず DinnerRepository クラスを作成します。

小規模なアプリケーションは、LINQ to SQL DataContext クラスに対して直接動作し、コント ローラー内での LINQ クエリを埋め込むのコント ローラーを用意しても構いませんがあります。 大規模なアプリケーションとして、ただし、このアプローチに維持し、テストする面倒ななります。 これは、場合も複数の場所で同じ LINQ クエリを複製することにつながります。

メンテナンスやテストを容易にアプリケーションをすることが 1 つの方法では、「リポジトリ」のパターンを使用します。 リポジトリのクラスにより、データのクエリを実行して、永続化ロジックと抽象アプリケーションからのデータの永続化の実装の詳細をカプセル化します。 アプリケーション コードを簡潔に行うには、だけでなくリポジトリ パターンを使用してやすく、後では、データ ストレージの実装を変更して、単体テストの実際のデータベースを必要とせず、アプリケーションを容易にするために役立ちます。

NerdDinner アプリケーションを持つときは必ず DinnerRepository クラスを定義します、署名の下。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*注: この章の IDinnerRepository インターフェイスのこのクラスから抽出され、コント ローラーでそれに依存関係の挿入を有効にします。まず、ここを簡単に開始し、ときは必ず DinnerRepository クラスを直接操作だけです。*

"Models"フォルダーを右クリックしを選択しましたが、このクラスを実装するために、**追加 -&gt;新しい項目の**メニュー コマンド。 "新しい項目の追加 ダイアログ ボックスでは、「クラス」テンプレートを選択し、ファイルの名前を"DinnerRepository.cs"しましたします。

![](build-a-model-with-business-rule-validations/_static/image10.png)

次のコードを使用して、DinnerRespository クラスを実装しましたできます。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>取得、更新、挿入およびときは必ず DinnerRepository クラスを使用して削除します。

ときは必ず DinnerRepository クラスを作成しましたので、次のことができるかの一般的なタスクを示すいくつかのコード例を見てみましょう。

#### <a name="querying-examples"></a>例のクエリを実行します。

次のコードは、DinnerID 値を使用して 1 つの夕食を取得します。


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

次のコードは、上にすべての今後の dinners とループを取得します。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>挿入と更新プログラムの例

次のコードでは、2 つの新しい dinners を追加することを示します。 上の"Save()"メソッドが呼び出されるまでの追加/変更をリポジトリには、データベースにコミットはありません。 LINQ to SQL を使用 – データベースのトランザクションのため、すべての変更が発生するか、リポジトリに保存するときに、いずれのすべての変更が自動的に折り返さ。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

次のコードでは、既存の Dinner オブジェクトを取得し、2 つのプロパティを変更します。 当社のリポジトリ上の"Save()"メソッドが呼び出されたときに、変更を元のデータベースにコミットします。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

次のコードを夕食を取得し、予約を追加します。 これは、(データベース内の 2 つのプライマリのキー/外部キー リレーションシップがある) ための LINQ to SQL を作成する Dinner オブジェクトに予約したコレクションを使用します。 この変更 RSVP の新しいテーブル行としてデータベースに永続化、リポジトリ上の"Save()"メソッドが呼び出されたときに。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>削除の例

次のコードでは、既存の Dinner オブジェクトを取得し、削除のマークを付けます。 リポジトリの"Save()"メソッドが呼び出されたときに、削除を元のデータベースにコミットします。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>モデル クラスを検証し、ビジネス ルール ロジックの統合

検証とビジネス ルールのロジックは、データで動作する任意のアプリケーションの重要な部分を統合します。

#### <a name="schema-validation"></a>スキーマの検証

LINQ to SQL デザイナーを使用してモデル クラスを定義したら、データ モデル クラス内のプロパティのデータ型は、データベース テーブルのデータ型に対応します。 例: Dinners テーブル内の"EventDate"列が"datetime"の場合は、LINQ to SQL によって作成されたデータ モデル クラスが型"DateTime"(つまり、組み込みの .NET データ型) になります。 これにして、コードから整数またはブール値を割り当てようとして、エラーが発生、自動的に実行時に有効でない文字列型を暗黙的に変換しようとした場合にコンパイル エラーが発生することを意味します。

どの保護に役立ちます SQL インジェクション攻撃からそれを使用する場合、文字列を使用するときに LINQ to SQL はするのも自動的に SQL のエスケープ値を処理します。

#### <a name="validation-and-business-rule-logic"></a>検証とビジネス ルールのロジック

スキーマの検証は、最初の手順として役に立ちますが、ほとんどだけで十分です。 ほとんどの現実のシナリオを複数のプロパティにまたがる、コードを実行および多くの場合、モデルの状態を認識している高度な検証ロジックを指定する機能が必要 (例: が作成/更新/削除、またはドメイン固有の状態このような「アーカイブ」)。 さまざまなさまざまなパターンや定義およびモデルのクラスに検証規則を適用するのに使用できるフレームワークがあるし、いくつか .NET ベースのフレームワークがこれを支援するために使用できます。 ASP.NET MVC アプリケーション内でそれらのほぼすべてを使用することができます。

NerdDinner アプリケーションのために、使用します比較的単純でわかりやすいパターン Dinner モデル オブジェクトで、IsValid プロパティと GetRuleViolations() メソッドを公開します場所。 True または false、検証とビジネス ルールがすべて有効かどうかに応じて、IsValid プロパティを返します。 GetRuleViolations() メソッドでは、ルール エラーの一覧を返します。

「部分クラス」をプロジェクトに追加することで私たちの Dinner モデルの IsValid と GetRuleViolations() を実装します。 部分クラスは、(LINQ to SQL デザイナーによって生成された Dinner クラス) などの VS デザイナーによって管理されるクラスにメソッド/プロパティ/イベントを追加するために使用して、ツール、コードを使って時間を浪費するを回避します。 \Models フォルダーを右クリックして、プロジェクトに新しい部分クラスを追加し、「新しい項目の追加」メニュー コマンドを選択します。 "新しい項目の追加 ダイアログ ボックスで、「クラス」のテンプレートを選択しを Dinner.cs という名前を付けます。

![](build-a-model-with-business-rule-validations/_static/image11.png)

[追加] ボタンをクリックすると、Dinner.cs ファイルをプロジェクトに追加し、IDE 内で開くこと。 使用して基本的なルールの検証/強制フレームワークを実装できるし、次のコード。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

上記のコードに関するいくつかの注意:

- Dinner クラスには、"partial"キーワード: つまりそれに含まれるコードは LINQ to SQL デザイナーによって生成された管理クラスを使用して結合し、1 つのクラスにコンパイルは付けません。
- RuleViolation クラスは、規則違反に関する詳細を提供できるようにするプロジェクトに追加しますヘルパー クラスです。
- Dinner.GetRuleViolations() メソッドにより、検証とビジネス ルール評価される (すぐ実装にします)。 返さ規則エラーの詳細を提供する RuleViolation オブジェクトのシーケンス。
- Dinner.IsValid プロパティは、Dinner オブジェクトが、アクティブな RuleViolations を持つかどうかを示す便利なヘルパー プロパティを提供します。 Dinner オブジェクトを使用して、いつでも開発者によって事前にチェックすることができます (および例外は発生しません)。
- Dinner.OnValidate() 部分メソッドは、Dinner オブジェクトがデータベース内で永続化するときにいつでも通知できるようにする LINQ to SQL を提供するフックです。 上記 OnValidate() 実装により、Dinner はない RuleViolations 保存されるまで。 無効な状態である場合は、LINQ to SQL トランザクションを中止すると、例外を生成します。

このアプローチでは、検証とにビジネス ルールと統合できるシンプルなフレームワークを提供します。 ここでは追加、GetRuleViolations() 方法では、ルールの下。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

C# の"yield return"機能は使用 RuleViolations 任意のシーケンスを返します。 上記の最初の 6 つのルール チェックは、null または空の文字列プロパティ、夕食がすることはできませんを適用します。 前回のルールは、もう少し興味深いと呼び出しを追加できることを確認するには、このプロジェクトに、ContactPhone PhoneValidator.IsValidNumber() ヘルパー メソッドの番号形式と一致する Dinner の国。

使用できます。このサポートの電話番号の検証を実装するために NET の正規表現のサポート。 追加する単純な PhoneValidator 実装を次に示します、プロジェクトで、国固有の正規表現パターンのチェックを追加することにします。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>処理の検証とビジネス ロジックの違反

上の検証とビジネス ルールのコードで作成または更新を夕食をいつでも追加しました、これで、検証ロジックの規則が評価され、適用されます。

開発者は、事前に決定する Dinner オブジェクトは、有効なかどうかに次のようなコードし、すべての例外は生成せずにすべての違反の一覧を取得を記述できます。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

無効な状態で、夕食を保存しようとしましたがある場合、ときは必ず DinnerRepository の Save() メソッドを呼び出すと例外が生成されます。 これは、LINQ to SQL は、Dinner の変更を保存して、夕食に規則違反が存在しない場合は、例外を発生させる Dinner.OnValidate() にコードを追加しました前に自動的に、Dinner.OnValidate() 部分メソッドを呼び出すために発生します。 この例外をキャッチして事後対応的に修正する違反の一覧を取得します。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

UI 層内ではなく、モデル レイヤー内で、検証とビジネス ルールが実装されるためが適用され、アプリケーション内のすべてのシナリオを使用します。 後で変更してまたはビジネス ルールの追加を Dinner オブジェクトとの連携がそれを利用しているすべてのコードがあります。

1 か所でビジネス ルールを変更する柔軟性を持ち、これらの変更、アプリケーションと、UI ロジック全体に広がる必要はありませんは、適切に記述されたアプリケーションと、MVC フレームワークでは、推奨するのに役立ちの特典。

### <a name="next-step"></a>次の手順

モデルのクエリし、データベースを更新するために使用できるようになりましたね。

みましょういくつかのコント ローラーとビューを追加を使用して、その周囲の HTML UI エクスペリエンスをビルドするプロジェクト。

> [!div class="step-by-step"]
> [前へ](create-a-database.md)
> [次へ](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
