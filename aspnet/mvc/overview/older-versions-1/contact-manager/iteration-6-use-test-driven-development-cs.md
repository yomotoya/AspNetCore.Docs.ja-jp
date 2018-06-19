---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: イテレーション 6-テスト駆動開発 (c#) を使用して |Microsoft ドキュメント
author: microsoft
description: この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。 このイテレーションにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 94502625f66d3eb08a24b8f2a369bf456a3367b1
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876294"
---
<a name="iteration-6--use-test-driven-development-c"></a>イテレーション 6-テスト駆動開発 (c#) を使用します。
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>連絡先管理 ASP.NET MVC アプリケーション (c#) の構築
  

この一連のチュートリアルは、連絡先管理アプリケーション全体が開始されてから完了するを構築します。 お問い合わせのマネージャー アプリケーションでは、人のユーザーの一覧については使用すると、連絡先情報の名前、電話番号、電子メール アドレスを格納できます。

私たちは、複数のイテレーションにおける、アプリケーションを構築します。 各イテレーションで、アプリケーション、徐々 に向上します。 この複数のイテレーション アプローチの目的は、各変更の理由を理解するためです。

- イテレーション 1 には、アプリケーションを作成します。 最初のイテレーションでお連絡先のマネージャー最も簡単な方法で可能なを作成します。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- イテレーション 2 では、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの外観を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- イテレーション 3 - フォーム検証を追加します。 3 番目のイテレーションは、基本フォーム検証を追加します。 おは人が必要なフォームのフィールドを完了しなくても、フォームを送信することを防ぐ。 私たちも電子メール アドレスと電話番号を検証します。

- 4: イテレーションは、疎結合アプリケーションを作成します。 この 3 番目のイテレーションで利用の保守し、連絡先のマネージャー アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかのです。 たとえば、リポジトリ パターンと依存関係の挿入のパターンを使用するようにアプリケーションをリファクターします。

- イテレーション #5 - 単体テストを作成します。 5 番目のイテレーションでおやすく、アプリケーションを維持し、単体テストを追加して変更できます。 データ モデル クラスを模擬表示し、コント ローラーと検証ロジックの単体テストをビルドします。

- イテレーション 6 - テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加おには、まず単体テストを記述し、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- イテレーション #7 - Ajax 機能を追加します。 7 番目のイテレーションでお、応答性およびパフォーマンスの向上、アプリケーションの Ajax のサポートを追加することで。

## <a name="this-iteration"></a>このイテレーション

連絡先のマネージャー アプリケーションの前回のイテレーションでは、コードの安全策を提供する単体テストを作成しました。 単体テストを作成する動機は、コードを変更する回復力を高めることでした。 単体テストの場所に、問題なく、コードを変更したり既存の機能が崩れているかどうかがすぐにわかることがことができます。

このイテレーションで、単体テストを使用して、まったく異なる目的。 このイテレーションは使用して単体テストと呼ばれる、アプリケーション デザインの理念の一部として*テスト駆動開発*です。 テスト駆動開発を練習するときは、まずテストを記述し、テストに対してコードを記述します。

具体的には、テスト駆動開発を来た場合があるコードを作成するときに完了する 3 つの手順 (赤/緑/リファクター)。

1. (赤) に失敗した単体テストを作成します。
2. 単体テスト (緑) に合格するコードを記述します。
3. (リファクター) にコードをリファクタリングします。

最初に、単体テストを記述します。 単体テストでは、方法、コードが期待どおりに動作するという意図を表現があります。 最初に、単体テストを作成するときは、単体テストは失敗します。 テストに合格するアプリケーション コードを記述していないために、テストは失敗する必要があります。

次に、単体テストを成功させるためにのに十分なコードを記述します。 目標は、laziest、sloppiest 可能な最速の方法でコードを記述します。 時間を考えることに、アプリケーションのアーキテクチャの概要を浪費する必要があります。 代わりに、最小限の単体テストによって表される目的を満たすために必要なコードの記述に集中する必要があります。

最後に、十分なコードを記述した後は、もう一度し、アプリケーションの全体的なアーキテクチャを検討してください。 この手順では、(リファクター) を書き換えてソフトウェア設計を活用して、コード パターン----リポジトリ パターンなど、コードがより優れたようにします。 コードは、単体テストでカバーされるために fearlessly、この手順では、コードを書き直すことができます。

テスト駆動開発の実践に起因する多くの利点があります。 まず、テスト駆動開発強制的に実際に書き込まれる必要のあるコードに重点をします。 常に記述された特定のテストに合格するための十分なコードに注目している、ため、雑草にいると、膨大な量の使用していない場合はコードを記述できません。

次に、"test 最初"の設計方法論強制的にコードを記述するコードの使用方法の観点からします。 つまり、テスト駆動開発を来たときに常に、テストを作成するユーザーの観点からです。 そのため、テスト駆動開発と、わかりやすく Api があります。

最後に、テスト駆動開発を強制的を通常のアプリケーションを作成するプロセスの一環として単体テストを記述します。 プロジェクトの期限に近づくにつれて、テストは通常、最初に行う、ウィンドウがなくなることです。 テスト駆動開発を来たときにその一方がテスト駆動開発では、単体テスト サーバーの全体のアプリケーションのビルド プロセスに単体テストの記述に関する好である可能性が高くなります。

> [!NOTE] 
> 
> テスト駆動開発の詳細については、ことをお勧め Michael の受信可能方向 book お読みになる**レガシ コードを効率的に作業**です。


このイテレーションでは、新しい機能を追加お、連絡先のマネージャー アプリケーションにします。 連絡先グループのサポートを追加します。 ビジネスなどのカテゴリに連絡先を整理する連絡先グループと、友人グループを使用することができます。

テスト駆動開発のプロセスに従うことによってこの新機能をアプリケーションに追加します。 最初の単体テストを記述し、すべてのこれらのテストに対して、コードを記述します。

## <a name="what-gets-tested"></a>テストのものを取得

前回のイテレーションで説明したよう通常データ アクセス ロジックの単体テストを記述したりしないロジックを表示します。 比較的低速の操作は、データベースにアクセスするため、データ アクセス ロジックの t 書き込み単体テストがありません。 比較的低速の操作では、web サーバーをスピンアップ ビューにアクセスする必要があるために、ビュー ロジックの t 書き込み単体テストがありません。 T べきでは、テスト実行できる何度も非常に高速しない限り、単体テストを記述します。

テスト駆動開発は単体テストによって促進されて、ためお重点最初にコント ローラーとビジネス ロジックを記述します。 データベースまたはビューを変更することは避けてください。 T 獲得したデータベースを変更したり、このチュートリアルの最後まで、ビューを作成します。 新機能をテストできるように開始します。

## <a name="creating-user-stories"></a>ユーザー ストーリーを作成します。

テスト駆動開発を来たときに常にまずテストを作成します。 これは、直後に疑問が出てきます: 最初に記述するには、どのようなテストを決定するか。 この質問に答えるのセットを記述する必要があります[**ユーザー ストーリー**](http://en.wikipedia.org/wiki/User_stories)です。

ユーザー ストーリーは、ソフトウェアの要件の非常に短く (通常 1 文) 説明です。 ユーザーの観点から書き込まれた要件の技術的説明になります。

ここでは、新しい連絡先グループ機能で必要な機能について説明しているユーザー ストーリーのセット。

1. ユーザーは、連絡先グループの一覧を表示できます。
2. ユーザーは、新しい連絡先グループを作成できます。
3. ユーザーは、既存の連絡先グループを削除できます。
4. 新しい連絡先を作成するときに、ユーザーは連絡先グループを選択できます。
5. 既存の連絡先を編集する場合、ユーザーは連絡先グループを選択できます。
6. 連絡先グループの一覧は、インデックス ビューに表示されます。
7. ユーザーは、連絡先グループをクリックすると、該当する連絡先の一覧が表示されます。

このユーザー ストーリーの一覧が、顧客が完全に理解できるものであることを確認します。 技術的な実装詳細の記述は含まれません。

過程でアプリケーションをビルドして、一連のユーザー ストーリーがより洗練されたになることができます。 複数のストーリー (要件など) をユーザー ストーリーを中断する可能性があります。 たとえば、新しい連絡先グループを作成する必要があります含まれることの検証を決定する可能性があります。 名前のない連絡先グループを送信すると、検証エラーを返す必要があります。

ユーザー ストーリーの一覧を作成した後は、最初の単体テストを記述する準備ができたらです。 連絡先グループの一覧を表示するための単体テストを作成することで始めます。

## <a name="listing-contact-groups"></a>連絡先グループを一覧表示します。

この最初のユーザー ストーリーは、ユーザーが連絡先グループの一覧を表示できることです。 このストーリーをテストして express が必要です。

ContactManager.Tests プロジェクトで、Controllers フォルダーを右クリックして新しい単体テストの作成を選択すると**追加]、[新しいテスト**を選択して、**単体テスト**テンプレート (図 1 を参照してください)。 名前の新しい単位が GroupControllerTest.cs をテストし、をクリックして、 **OK**ボタンをクリックします。


[![GroupControllerTest 単体テストを追加します。](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**図 01**: GroupControllerTest 単体テストを追加する ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image2.png))


最初の単体テストは、1 のリストに含まれます。 このテストでは、グループ コント ローラーの Index() メソッドがグループのセットを返すことを確認します。 テストでは、グループのコレクションがビュー内のデータで返されることを確認します。

**1 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

最初に、Visual Studio でのリストの 1 のコードを入力すると、ときに多数の赤い波線が表示されます。 GroupController またはグループのクラスを作成していません。

この時点では、t でもビルドことができないので、アプリケーションは、最初の単体テストを実行します。 お勧めします。 失敗したテストとしてをカウントします。 そのため、お今すぐアクセス許可を持つをアプリケーション コードの記述を開始します。 このテストを実行するための十分なコードを記述する必要があります。

リスト 2 でグループのコント ローラー クラスには、単体テストに合格するために必要なコードの最低限が含まれています。 Index() アクションでは、グループ (グループ クラスは一覧表示する 3 で定義) の静的にコード化された一覧を返します。

**2 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**3 - Models\Group.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

プロジェクトに GroupController およびグループのクラスを追加お後、最初の単体テストが正常に完了 (図 2 を参照してください)。 テストに合格するために必要な最小限の作業を行っています。 時間を記念することをお勧めします。


[![正常にされました。](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**図 02**: 成功! ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image4.png))


## <a name="creating-contact-groups"></a>連絡先グループを作成します。

これで、2 番目のユーザー ストーリーに移動できます。 新しい連絡先グループを作成できるようにする必要があります。 テストでこの目的を表現する必要があります。

リスト 4 内のテストでは、新しいグループを持つメソッドが Index() メソッドによって返されるグループの一覧に対してグループを追加 Create() を呼び出しているを確認します。 つまり、新しいグループを作成する場合、すべき Index() メソッドによって返されるグループの一覧から戻り、新しいグループを取得できません。

**4 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

リスト 4 内のテストでは、グループ コント ローラーの新しい連絡先グループの Create() メソッドを呼び出します。 次に、テストでは、データの表示で新しいグループを返すグループ コント ローラー Index() メソッドを呼び出すことを確認します。

変更されたグループ内のコント ローラー 5 の一覧表示するには、新しいテストに合格するために必要な変更の最低限が含まれています。

**5 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>コード サンプル 5 グループ コント ローラーは、新しい Create() アクションを持ちます。 この操作は、グループのコレクションにグループを追加します。 グループのコレクションのコンテンツを返す Index() アクションが変更されたことに注意してください。

もう一度おベア最小限の単体テストに合格するために必要な作業を実行しています。 グループ コント ローラーにこれらの変更は後で、すべてのユニットのテストに成功します。

## <a name="adding-validation"></a>検証の追加

この要件は、ユーザー ストーリーで明示的に報告されていません。 ただし、グループが名前を付けることを要求するように適切なは。 それ以外の場合、アドレス帳をグループに編成できません非常に便利です。

6 を一覧表示するには、この目的を表す新しいテストが含まれます。 このテストでは、モデルの状態の検証エラー メッセージに名前の結果を指定せず、グループを作成しようとしています。 ことを確認します。

**6 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

このテストを満たすために、グループのクラス (7 のリストを参照) に、Name プロパティを追加する必要があります。 さらに、このグループのコント ローラー s Create() アクション (8 のリストを参照) にわずかな検証ロジックを追加する必要があります。

**7 - Models\Group.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**8 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

グループのコント ローラー Create() 処理をすぐに検証とデータベースの両方のロジックが含まれていることを確認します。 現時点では、グループのコント ローラーによって使用されるデータベースは、メモリ内コレクションよりも詳細 nothing で構成されます。

## <a name="time-to-refactor"></a>リファクタリングする時間

赤/緑/リファクタリングの 3 番目の手順では、リファクタリングの部分です。 この時点では、そのデザインを向上させるためにアプリケーションをリファクターお方法を検討してください、コードからステップする必要があります。 リファクタリングを行うステージは、ステージを考えてハード ソフトウェア設計の原則、およびパターンを実装する最適な方法です。

コードの設計を向上させるために選択する任意の方法でコードを変更するために解放います。 既存の機能を損なうを妨げている単体テストの安全策があります。

現在、当社グループ コント ローラーは優れたソフトウェア設計の観点から乱雑です。 グループのコント ローラーには、絡み合った乱雑な検証とデータ アクセス コードが含まれています。 単一の責任の原則に違反するを回避するのには、これらの問題をさまざまなクラスに分割する必要があります。

リファクタリングされたグループ コント ローラー クラスは、9 の一覧に含まれます。 ContactManager サービス層を使用するには、コント ローラーを変更されています。 これは、連絡先コント ローラーを使用して、同じサービス レイヤーです。

10 を一覧表示するには、検証、一覧表示、およびグループの作成をサポートするために ContactManager サービス層に追加された新しいメソッドが含まれます。 新しいメソッドを含める IContactManagerService インターフェイスが更新されました。

11 を一覧表示するには、IContactManagerRepository インターフェイスを実装する新しい FakeContactManagerRepository クラスが含まれます。 インターフェイスを実装する、IContactManagerRepository EntityContactManagerRepository クラスとは異なり、新しい FakeContactManagerRepository クラスは、データベースと通信しません。 FakeContactManagerRepository クラスは、データベースのプロキシとして、メモリ内コレクションを使用します。 このクラスの単体テストで偽リポジトリ レイヤーとして使用されます。

**9 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**10 - Controllers\ContactManagerService.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**11 - Controllers\FakeContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

使用して EntityContactManagerRepository クラスで、CreateGroup() および ListGroups() メソッドを実装するインターフェイスが必要です IContactManagerRepository を変更します。 これを行う laziest と最も高速な方法は、次のようにスタブ メソッドを追加してです。   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


最後に、これらの変更をアプリケーションの設計では、単体テストに変更を加えることが必要です。 これで、単体テストを実行するときに、FakeContactManagerRepository を使用する必要があります。 更新後の GroupControllerTest クラスは、12 の一覧に含まれます。

**12 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

これらすべてを作成した後の変更をもう一度すべての単体テストの成功のです。 赤/緑/リファクタリングのサイクル全体が完了しました。 最初の 2 つのユーザー ストーリーが実装されました。 ユーザー ストーリーで示される要件の単体テストをサポートするがあるようになりました。 ユーザー ストーリーの残りの部分を実装するには、赤/緑/リファクタリングの同じサイクルを繰り返す必要があります。

## <a name="modifying-our-database"></a>データベースを変更します。

残念ながら、すべての単体テストで示される要件を満たすお、場合でも、作業は行われません。 このデータベースを変更する必要があります。

新しいグループのデータベース テーブルを作成する必要があります。 この場合は、以下の手順に従ってください。

1. サーバー エクスプ ローラー ウィンドウで テーブル フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**です。
2. 次のテーブル デザイナーで説明する 2 つの列を入力します。
3. Primary key 制約と Identity 列として Id 列をマークします。
4. 名前のグループを含む新しいテーブルを保存するには、フロッピー ディスクのアイコンをクリックします。

<a id="0.11_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | int | False |
| 名前 | nvarchar(50) | False |


次に、Contacts テーブルからすべてのデータを削除する必要があります (獲得した場合は、連絡先、およびグループのテーブル間のリレーションシップを作成することはできない)。 この場合は、以下の手順に従ってください。

1. Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**です。
2. すべての行を削除します。

次に、グループのデータベース テーブルと既存の連絡先データベース テーブル間のリレーションシップを定義する必要があります。 この場合は、以下の手順に従ってください。

1. テーブル デザイナーを開き、サーバー エクスプ ローラー ウィンドウで、Contacts テーブルをダブルクリックします。
2. GroupId をという名前の連絡先のテーブルに整数型の新しい列を追加します。
3. Foreign Key Relationships ダイアログを開いてリレーションシップ ボタンをクリックして (図 3 を参照してください)。
4. [追加] ボタンをクリックします。
5. テーブルと列の指定 ボタンの横に表示される省略記号ボタンをクリックします。
6. テーブルと列 ダイアログ ボックスでは、主キー テーブルと、主キー列として Id としてグループを選択します。 外部キー テーブルと外部キー列として GroupId として連絡先を選択 (図 4 を参照してください)。 [Ok] ボタンをクリックします。
7. **INSERT および UPDATE の指定**、値を選択して**Cascade**の**ルールの削除**です。
8. Foreign Key Relationships ダイアログを閉じる閉じるボタンをクリックします。
9. 変更を保存、Contacts テーブルに保存 ボタンをクリックします。


[![データベース テーブルのリレーションシップを作成します。](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**図 03**: データベースのテーブル リレーションシップの作成 ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image6.png))


[![テーブル間のリレーションシップを指定します。](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**図 04**: テーブルのリレーションシップを指定する ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image8.png))


### <a name="updating-our-data-model"></a>このデータ モデルを更新します。

次に、新しいデータベース テーブルを表すため、データ モデルを更新する必要があります。 この場合は、以下の手順に従ってください。

1. モデル フォルダーを開くには、Entity Designer で ContactManagerModel.edmx ファイルをダブルクリックします。
2. デザイナー画面を右クリックし、メニュー オプションを選択**データベースからモデルを更新**です。
3. 更新ウィザードでは、テーブルが表示され、完了 をクリックして、グループの選択は、(図 5 を参照してください) ボタンします。
4. グループ エンティティを右クリックし、メニュー オプションを選択**の名前を変更**です。 名前を変更、*グループ*エンティティを*グループ*(単数形)。
5. Contact エンティティの下部に表示されるグループ ナビゲーション プロパティを右クリックします。 名前を変更、*グループ*へのナビゲーション プロパティ*グループ*(単数形)。


[![データベースから、Entity Framework モデルを更新します。](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**図 05**: データベースから、Entity Framework モデルを更新する ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image10.png))


次の手順を完了すると、データ モデルは、連絡先とグループの両方のテーブルを表します。 エンティティ デザイナーは、両方のエンティティを表示する必要があります (図 6 を参照してください)。


[![エンティティ デザイナーのグループおよび連絡先を表示します。](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**図 06**: グループおよび連絡先を表示するエンティティ デザイナー ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image12.png))


### <a name="creating-our-repository-classes"></a>このリポジトリ クラスを作成します。

次に、このリポジトリ クラスを実装する必要があります。 このイテレーションの過程で、いくつかの新しいメソッドに追加 IContactManagerRepository インターフェイス単体テストを満たすためにコードを書き込み中にします。 IContactManagerRepository インターフェイスの最終バージョンは、14 の一覧に含まれます。

**14 - Models\IContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

おいない t が実際に連絡先グループの操作に関連するメソッドのいずれかを実装します。 現時点では、EntityContactManagerRepository クラスでは、各 IContactManagerRepository インターフェイスで示されている連絡先グループ メソッドのスタブのメソッドがあります。 たとえば、ListGroups() メソッドは、次のように現在なります。

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

スタブ メソッドをアプリケーションをコンパイルし、単体テストに合格するようになりました。 ただし、ここには実際にはこれらのメソッドを実装するします。 EntityContactManagerRepository クラスの最終バージョンは、13 の一覧に含まれます。

**13 - Models\EntityContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>ビューを作成します。

ASP.NET MVC アプリケーションは、既定の ASP.NET ビュー エンジンを使用するとします。 そのためが表示されないは、特定の単体テストへの応答にビューを作成します。 ただし、アプリケーションがあるためビュー役に立たない、ことが t の作成と、連絡先のマネージャー アプリケーションに含まれるビューを変更せずにこのイテレーションを完了します。

次の新しいビュー連絡先グループを管理するため (図 7 を参照してください) を作成する必要があります。

- Views\Group\Index.aspx - 連絡先グループの一覧を表示
- Views\Group\Delete.aspx - 連絡先グループを削除するため確認フォームを表示


[![グループ インデックス ビュー](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**図 07**: グループのインデックス ビュー ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image14.png))


連絡先グループが含まれるように、次の既存のビューを変更する必要があります。

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

このチュートリアルに付属している Visual Studio アプリケーションを確認して、変更されたビューを表示できます。 たとえば、図 8 は、連絡先インデックス ビューを示しています。


[![連絡先のインデックス ビュー](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**図 08**:、連絡先インデックス ビュー ([フルサイズのイメージを表示するをクリックして](iteration-6-use-test-driven-development-cs/_static/image16.png))


## <a name="summary"></a>まとめ

このイテレーションが追加されました新機能、連絡先のマネージャー アプリケーションをテスト駆動開発アプリケーションの設計方法に従って。 ユーザー ストーリーのセットを作成して開始します。 ユーザー ストーリーに示される要件に対応する一連の単体テストを作成しました。 最後に、単体テストで示される要件を満たすのに十分なコードを記述します。

単体テストで示される要件を満たすために十分なコードを記述して完了すると、データベースとビューに更新されました。 データベースに新しいグループ テーブルを追加し、Entity Framework データ モデルを更新します。 おも作成し、一連のビューを変更します。

次のイテレーション--最後の反復処理--では、Ajax を活用するようにアプリケーションを書き直します。 Ajax を利用して、応答性と連絡先のマネージャー アプリケーションのパフォーマンスを改善します。

> [!div class="step-by-step"]
> [前へ](iteration-5-create-unit-tests-cs.md)
> [次へ](iteration-7-add-ajax-functionality-cs.md)
