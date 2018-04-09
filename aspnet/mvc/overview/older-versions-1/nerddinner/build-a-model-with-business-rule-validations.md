---
uid: mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
title: ビジネス ルールの検証とモデルの作成 |Microsoft ドキュメント
author: microsoft
description: 手順 3 では、両方のクエリを使用してしたり NerdDinner アプリケーション データベースを更新でくモデルを作成する方法を示します。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 0bc191b2-4311-479a-a83a-7f1b1c32e6fe
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/build-a-model-with-business-rule-validations
msc.type: authoredcontent
ms.openlocfilehash: c5a482474fd2f41836f70952306ada5cd9136455
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="build-a-model-with-business-rule-validations"></a>ビジネス ルールの検証とモデルを構築します。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 3 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 3 では、両方のクエリを使用してしたり NerdDinner アプリケーション データベースを更新でくモデルを作成する方法を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-3-building-the-model"></a>NerdDinner 手順 3: モデルの構築

モデル ビュー コント ローラーのフレームワークでは、「モデル」という用語と統合する検証ルールとビジネス ルールに対応するドメイン ロジックと同様に、アプリケーションのデータを表すオブジェクトを指します。 モデルでは、さまざまな方法で「心臓部」MVC ベースのアプリケーションし、ドライブの動作を後で根本的にわかりのようです。

ASP.NET MVC フレームワークでは、任意のデータ アクセス テクノロジを使用してサポートしているし、開発者は、さまざまなを含む、モデルを実装する機能豊富な .NET データ オプションから選択できます。 LINQ to Entities、LINQ to SQL、NHibernate、LLBLGen Pro、SubSonic、WilsonORM、または単なる生 ADO です。NET DataReaders やデータセットです。

NerdDinner アプリケーションに対して行うを LINQ to SQL を使用して、データベースの設計にはかなり密接に対応しており、一部のカスタム検証ロジックとビジネス ルールを追加する単純なモデルを作成します。 概要を離れたときに役立つリポジトリ クラスをテスト単位簡単にすることにより、アプリケーションでは、残りの部分からデータの永続性の実装お実装は。

### <a name="linq-to-sql"></a>LINQ to SQL

LINQ to SQL は、.NET 3.5 の一部として同梱されている ORM (オブジェクト リレーショナル マッパーです)。

LINQ to SQL では、.NET クラスに対してコードを記述できることをデータベース テーブルにマップする簡単な方法を提供します。 NerdDinner アプリケーションの Dinner および RSVP のクラスに、データベース内で、ディナーおよび RSVP テーブルにマップするのにに使用されます。 ディナーおよび RSVP のテーブルの列は、Dinner および RSVP クラスのプロパティに対応します。 Dinner および RSVP の各オブジェクトは、データベース内のディナーまたは RSVP テーブル内の個別の行を表します。

LINQ to SQL では、SQL ステートメントを取得および Dinner および RSVP の更新を手動で作成する必要があるにより、データベースのデータ オブジェクト。 代わりに、それらに対応付ける方法、およびデータベース、およびそれらのリレーションシップから Dinner および RSVP のクラスを定義します。 LINQ to SQL では、対話は、それらを使用すると、実行時に使用する適切な SQL の実行ロジックを生成するを引き継ぎます。

VB と c# 内での LINQ の言語サポートを使用して Dinner および RSVP を取得する優れたクエリを記述、データベースからのオブジェクト。 これには、データのコードを記述する必要がありますの量が最小化し、実際にクリーンなアプリケーションを構築することができます。

### <a name="adding-linq-to-sql-classes-to-our-project"></a>追加する LINQ to SQL クラスをプロジェクト

このプロジェクト内で"Models"フォルダーを右クリックして開始がうまくを選択し、**追加 -&gt;新しい項目の**メニュー コマンド。

![](build-a-model-with-business-rule-validations/_static/image1.png)

これは、"新しい項目の追加 ダイアログ ボックスが表示されます。 「データ」カテゴリでフィルタ リング、内に「SQL クラスを LINQ」テンプレートを選択します。

![](build-a-model-with-business-rule-validations/_static/image2.png)

項目"NerdDinner"という名前を「追加」ボタンをクリックします。 Visual Studio は、当社 \Models ディレクトリにある NerdDinner.dbml ファイルを追加し、LINQ to SQL オブジェクト リレーショナル デザイナーを開きます。

![](build-a-model-with-business-rule-validations/_static/image3.png)

### <a name="creating-data-model-classes-with-linq-to-sql"></a>LINQ to SQL でデータ モデル クラスを作成します。

LINQ to SQL では、既存のデータベース スキーマからデータ モデル クラスをすばやく作成できます。 タスクでモデル化するこのおを NerdDinner データベース、サーバー エクスプ ローラーを開き、テーブルを選択します。

![](build-a-model-with-business-rule-validations/_static/image4.png)

おことができますし、テーブル、LINQ の上に SQL デザイナー画面にドラッグします。 ときにこの LINQ to SQL 行うと、Dinner が自動的に作成し、(データベース テーブルの列にマップされるクラス プロパティ) を持つテーブルのスキーマを使用して、RSVP クラス。

![](build-a-model-with-business-rule-validations/_static/image5.png)

既定では、LINQ to SQL デザイナーに自動的に「を複数化」テーブルおよび列名、データベース スキーマに基づくクラスの作成時にします。 例: 前の例「ディナー」テーブル"Dinner"クラスが発生しました。 このクラスの名前付けには、作成、モデルが .NET の名前付け規則と一致が使用して、その発生しているデザイナーの修正プログラムこの便利なを (特に多くのテーブルの追加) 通常の検索します。 クラスや、常にこのメソッドをオーバーライドし、任意の名前に変更できますが、デザイナーにより生成されたプロパティの名前を取り消す場合。 こうことに、エンティティ/プロパティ名で行のデザイナー内での編集するか、プロパティ グリッドを使用して変更します。

既定では、LINQ to SQL デザイナーはまた、テーブルの主キー/外部キーのリレーションシップを検査し、それに基づいて自動的に関連付けの作成既定"リレーションシップ"を作成する別のモデル クラス間でします。 たとえばと、ディナーをドラッグして、LINQ to SQL デザイナー上に 2 つの一対多リレーションシップの関連テーブルを RSVP 推定された、という事実に基づいて RSVP テーブルには、ディナー テーブルへの外部キーが必要がある (これは、矢印によって示されます、デザイナーの):

![](build-a-model-with-business-rule-validations/_static/image6.png)

上記の関連付けには、sql 開発者特定 RSVP に関連付けられている Dinner へのアクセスに使用できる RSVP クラスを厳密に型指定された"Dinner"プロパティを追加する LINQ が発生します。 開発者の取得し、特定夕食に関連付けられている RSVP オブジェクトを更新することができる"RSVPs"コレクション プロパティを持つ Dinner クラスにもなります。

次に示す新しい RSVP オブジェクトを作成し、Dinner の RSVPs コレクションに追加する場合は、Visual Studio 内での intellisense の使用例を確認できます。 どのように LINQ to SQL 自動的に追加された"RSVPs"コレクション Dinner オブジェクトでに注意してください。

![](build-a-model-with-business-rule-validations/_static/image7.png)

RSVP オブジェクト コレクションに追加して Dinner の RSVPs 指示することに LINQ to SQL に Dinner とデータベースに RSVP 行の間の外部キー リレーションシップを関連付けるには。

![](build-a-model-with-business-rule-validations/_static/image8.png)

デザイナーのモデルまたはテーブルの関連付けをという名前の方法に満足できない場合は、メソッドをオーバーライドすることができます。 デザイナー内での関連付けの矢印をクリックして、名前の変更、削除または変更するには、プロパティ グリッドを使用してそのプロパティにアクセスだけです。 NerdDinner アプリケーションでは、既定のアソシエーション ルールが構築して、データ モデル クラスの適切に動作して、次の既定の動作を使用できますが、です。

### <a name="nerddinnerdatacontext-class"></a>NerdDinnerDataContext Class

Visual Studio では、モデルおよび LINQ to SQL デザイナーを使用して定義されているデータベースのリレーションシップを表す .NET クラスを自動的に作成されます。 各 LINQ to SQL デザイナー ファイルがソリューションに追加の LINQ to SQL DataContext クラスが生成されます。 当社の LINQ to SQL クラスの項目"NerdDinner"という名前を付けて、ために、作成された DataContext クラスには、"NerdDinnerDataContext"が呼び出されます。 この NerdDinnerDataContext クラスは、プライマリ データベースと対話できます。

当社 NerdDinnerDataContext クラスは、データベース内でモデル化して 2 つのテーブルを表す「ディナー」と"RSVPs"- - 2 つのプロパティを公開します。 C# を使用すると、クエリと取得の Dinner および RSVP のオブジェクトをデータベースからこれらのプロパティに対する LINQ クエリを書き込む。

次のコードでは、NerdDinnerDataContext オブジェクトをインスタンス化し、今後の予定ディナーのシーケンスが得られるようにに対して LINQ クエリを実行する方法を示します。 Visual Studio は、LINQ クエリを作成するときにフル intellisense を提供し、そこから返されるオブジェクトが厳密に型指定も intellisense をサポートします。

![](build-a-model-with-business-rule-validations/_static/image9.png)

Dinner と RSVP のオブジェクトのクエリを実行することを許可するだけでなく、NerdDinnerDataContext も自動的に変更を取得するまでお Dinner と RSVP オブジェクトに、その後に加えるおを追跡します。 データベースの明示的な SQL 更新プログラム コードを記述するのにことがなく簡単に変更を保存するのにこの機能を使用できます。

たとえば、次のコードでは、LINQ クエリを使用して単一 Dinner オブジェクトをデータベースから取得、2 つの Dinner プロパティを更新およびデータベースに変更を保存する方法を示しています。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample1.cs)]

前のコードで NerdDinnerDataContext オブジェクトは、そこから取得お Dinner オブジェクトに対して行われたプロパティの変更を自動的に追跡します。 私たちには、"SubmitChanges()"メソッドが呼び出されるには、適切な SQL ステートメントを実行"UPDATE"、データベースに、更新された値を保持します。

### <a name="creating-a-dinnerrepository-class"></a>DinnerRepository クラスを作成します。

小規模なアプリケーションでは、LINQ to SQL DataContext クラスと直接やり取りし、コント ローラー内での LINQ クエリを埋め込むコント ローラーがある問題があります。 大規模なアプリケーションと、ただし、この方法は複雑になりますを維持し、テストします。 複数の場所に同じ LINQ クエリを複製することになることができますも。

簡単にすることができますのアプリケーションを管理およびテストする方法の 1 つでは、"repository"パターンを使用します。 リポジトリ クラスにより、クエリを実行するデータと永続性ロジック、および概要を離れたアプリケーションからデータの永続性の実装の詳細をカプセル化します。 アプリケーション コードを見やすくするだけでなくリポジトリ パターンを使用することができますしやすく、将来データ ストレージの実装を変更し、単体テストの実際のデータベースを必要とせず、アプリケーションを容易にするために役立ちます。

NerdDinner アプリケーションについてを持つ DinnerRepository クラスを定義します、署名の下。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample2.cs)]

*メモ: 後の「おをこのクラスから IDinnerRepository インターフェイスを抽出および、コント ローラーと依存関係挿入有効にします。まず始めに、ただしはここを簡単に開始し、DinnerRepository クラスを直接操作だけです。*

"Models"フォルダーを右クリックしてがうまくを選択し、このクラスを実装する、**追加 -&gt;新しい項目の**メニュー コマンド。 "新しい項目の追加 ダイアログ ボックスでは、"Class"テンプレートを選択し、"DinnerRepository.cs"ファイルの名前おします。

![](build-a-model-with-business-rule-validations/_static/image10.png)

次のコードを使用して、DinnerRespository クラスお実装できます。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample3.cs)]

### <a name="retrieving-updating-inserting-and-deleting-using-the-dinnerrepository-class"></a>取得する、更新、挿入および削除 DinnerRepository クラスを使用します。

これで、DinnerRepository クラスを作成しましたは、一般的なタスクのことができるかを示す、いくつかのコード例を見てみましょう。

#### <a name="querying-examples"></a>例のクエリを実行します。

次のコードは、DinnerID 値を使用して単一 Dinner を取得します。


[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample4.cs)]

次のコードは、上にすべての今後のディナーとループを取得します。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample5.cs)]

#### <a name="insert-and-update-examples"></a>挿入と更新プログラムの例

次のコードでは、次の 2 つの新しいディナーを追加することを示します。 リポジトリへの追加や変更は、"Save()"メソッドの呼び出しはまでデータベースにコミットされません。 LINQ to SQL を使用 – データベースのトランザクションですべての変更が発生するか、全くリポジトリを保存するのすべての変更が自動的に折り返さ。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample6.cs)]

次のコードでは、既存の Dinner オブジェクトを取得しに 2 つのプロパティを変更します。 リポジトリで"Save()"メソッドが呼び出された場合は、変更をデータベースにコミットします。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample7.cs)]

次のコードは、dinner を取得し、RSVP を追加します。 これは、(データベースに 2 つのプライマリのキー/外部キー リレーションシップがある) ために、LINQ to SQL をご利用の米国作成 Dinner オブジェクトのコレクションの RSVPs を使用します。 この変更新しい RSVP テーブルの行としてデータベースに永続化リポジトリで"Save()"メソッドが呼び出されたときに。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample8.cs)]

#### <a name="delete-example"></a>削除の例

次のコードでは、既存の Dinner オブジェクトを取得し、削除のマークを付けます。 リポジトリで"Save()"メソッドが呼び出された場合は、削除をデータベースにコミットします。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample9.cs)]

### <a name="integrating-validation-and-business-rule-logic-with-model-classes"></a>モデルのクラスと検証とビジネス ルール ロジックの統合

検証とビジネス ルールのロジックは、データを操作するすべてのアプリケーションの重要な部分を統合しています。

#### <a name="schema-validation"></a>スキーマの検証

モデル クラスを LINQ to SQL デザイナーを使用して定義したら、データ モデル クラスのプロパティのデータ型は、データベース テーブルのデータ型に対応します。 例: 型 (ある組み込みの .NET データ型)"DateTime"ディナー テーブル内の"EventDate"列が、"datetime"の場合は、LINQ to SQL によって作成されたデータ モデル クラスであります。 つまりにして、コードから整数またはブール値を割り当てようとすると、エラーが発生、自動的に実行時に有効でない文字列型を暗黙的に変換しようとする場合、コンパイル エラーが表示されます。

保護するのに役立つする SQL インジェクション攻撃からそれを使用する場合、文字列を使用するときに LINQ to SQL はのも自動的にエスケープ SQL の値を処理します。

#### <a name="validation-and-business-rule-logic"></a>検証とビジネス ルール ロジック

スキーマの検証は最初の手順として役に立ちますが、ほとんどだけで十分です。 ほとんどの現実のシナリオが複数のプロパティにまたがる、コードを実行したりできる多くの場合、モデルの状態を認識している豊富な検証ロジックを指定することが必要 (例: を作成/更新/削除、またはドメイン固有の状態like「アーカイブ」)。 さまざまなパターンやを定義し、モデルのクラスに検証規則を適用するために使用するフレームワークのさまざまながあるし、いくつかの .NET ベース フレームワークがこれを支援するために使用できます。 ASP.NET MVC アプリケーション内でそれらのほぼすべての操作を行うこともできます。

NerdDinner アプリケーションの目的で、使用されます、比較的単純な単純なパターンを Dinner モデル オブジェクトに IsValid プロパティおよび GetRuleViolations() メソッドを公開しています。 True または false、検証規則とビジネス規則はすべて有効かどうかに応じて IsValid プロパティを返します。 GetRuleViolations() メソッドでは、ルール エラーの一覧を返します。

おを実装 IsValid と GetRuleViolations() Dinner モデルの「クラスを部分」をプロジェクトに追加します。 部分クラス (LINQ to SQL デザイナーによって生成された Dinner クラス) などの VS デザイナーで保持されているクラスにメソッド/プロパティ/イベントを追加するために使用して、コードを及ぼしたりからツールを回避できるようにします。 \Models フォルダーを右クリックして、プロジェクトに新しい部分クラスを追加したり、「新しい項目の追加」メニュー コマンドを選択できます。 "新しい項目の追加 ダイアログ ボックスで「クラスが」テンプレートを選択しを Dinner.cs という名前を付けます。

![](build-a-model-with-business-rule-validations/_static/image11.png)

[追加] ボタンをクリックすると、Dinner.cs ファイルをプロジェクトに追加および IDE 内で開きます。 基本的なルール/検証強制フレームワークを使用して、実装することができますし、コードの下。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample10.cs)]

上記のコードをいくつか説明します。

- Dinner クラスはそれに含まれるコードを LINQ to SQL デザイナーによって生成される管理クラスと組み合わせるし、1 つのクラスにコンパイルされます意味 –"partial"はキーワードで始まります。
- RuleViolation クラスは、規則違反に関する詳細を提供することができるプロジェクトに追加のヘルパー クラスです。
- Dinner.GetRuleViolations() メソッドにより、検証とビジネス ルールに評価される (間もなく実装にします)。 返します戻る規則エラーの詳細を提供する RuleViolation オブジェクトのシーケンス。
- Dinner.IsValid プロパティでは、Dinner オブジェクトが、アクティブな RuleViolations を持つかどうかを示す便利なヘルパー プロパティを提供します。 Dinner オブジェクトを使用して、いつでも、開発者で事前にチェックすることができます (および例外は発生しません)。
- Dinner.OnValidate() 部分メソッドは、Dinner オブジェクトがデータベース内で永続化しようとしてあれば、いつでも通知により、LINQ to SQL を提供するフックがします。 上記 OnValidate() 実装により、Dinner はない RuleViolations 保存されるまで。 無効な状態にある場合は、これにより LINQ to SQL トランザクションを中止する、例外が発生します。

この方法は、検証ルールとビジネス ルールに統合できます単純なフレームワークを提供します。 ここでは追加、当社 GetRuleViolations() メソッドへの規則の下。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample11.cs)]

C# の「の戻り値の生成」機能を使用お任意 RuleViolations のシーケンスを返します。 上記の最初の 6 つのルール チェックは、null または空の文字列のプロパティを Dinner がすることはできませんを強制します。 興味深いは、前回のルールと呼び出すことに追加できることを確認する、プロジェクト、ContactPhone PhoneValidator.IsValidNumber() ヘルパー メソッド番号形式と一致する Dinner の国。

使用できます。この電話検証のサポートを実装する NET の正規表現のサポート。 追加できる単純な PhoneValidator 実装を次に示します国別の正規表現パターンのチェックを追加することにより、プロジェクトに。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample12.cs)]

#### <a name="handling-validation-and-business-logic-violations"></a>検証とビジネス ロジックの違反の処理

したを追加したので、上記の検証とビジネス ルールのコードを作成または、夕食を更新しようといつでも、当社の規則ロジックが評価され、適用されます。

開発者は、Dinner オブジェクトは、有効なかどうかを事前に決定する以下のようなコードし、すべての例外は生成せずにすべての違反の一覧の取得を作成できます。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample13.cs)]

無効な状態で、夕食を保存するおしようとすると、DinnerRepository で Save() メソッドを呼び出すと、例外が発生します。 これは、LINQ to SQL は、Dinner の変更を保存して、夕食に規則違反が存在しない場合、例外を発生させる Dinner.OnValidate() にコードを追加した前に自動的にマイクロソフト Dinner.OnValidate() 部分メソッドを呼び出すために発生します。 この例外をキャッチして、事後対応的違反の修正の一覧を取得します。

[!code-csharp[Main](build-a-model-with-business-rule-validations/samples/sample14.cs)]

当社モデル レイヤー内および UI レイヤー内ではなく、検証規則とビジネス規則が実装されているためにが適用され、アプリケーション内のすべてのシナリオで使用します。 後で変更またはビジネス ルールを追加したりできます Dinner オブジェクトとの連携を無視するすべてのコードがあります。

1 か所でビジネス ルールを変更する柔軟性、これらの変更、アプリケーションと UI ロジック全体に広がる必要はありませんが、適切に記述されたアプリケーションと、MVC フレームワークにより、推奨の効果の記号です。

### <a name="next-step"></a>次の手順

モデルのクエリし、データベースの更新に使用できるようになりましたしました。

みましょういくつかのコント ローラーとビューに追加、HTML UI エクスペリエンスをテーブルの周囲に使用できるプロジェクト。

> [!div class="step-by-step"]
> [前へ](create-a-database.md)
> [次へ](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
