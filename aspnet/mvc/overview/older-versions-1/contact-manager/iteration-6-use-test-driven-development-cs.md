---
uid: mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
title: '繰り返し #6 – テスト駆動開発 (c#) を使用して、|Microsoft Docs'
author: microsoft
description: この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。 このイテレーションにしています.
ms.author: riande
ms.date: 02/20/2009
ms.assetid: 013c3c26-7dc3-41d1-8064-f233c86008b5
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-6-use-test-driven-development-cs
msc.type: authoredcontent
ms.openlocfilehash: 3c4358a1b979ab95d8ac25551e21ee95d75e5eae
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823827"
---
<a name="iteration-6--use-test-driven-development-c"></a>繰り返し #6 – テスト駆動開発 (c#) を使用します。
====================
によって[Microsoft](https://github.com/microsoft)

[コードをダウンロードします。](iteration-6-use-test-driven-development-cs/_static/contactmanager_6_cs1.zip)

> この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a>連絡先管理 ASP.NET MVC アプリケーション (c#) の構築
  

このチュートリアル シリーズでは、開始から終了に全体連絡先管理アプリケーションを構築します。 Contact Manager アプリケーションは、ユーザーの一覧については店舗連絡先情報の名前、電話番号、電子メール アドレスにするようにことができます。

複数のイテレーションにおける、アプリケーションを構築します。 反復処理ごとに、アプリケーション徐々 に向上します。 この複数のイテレーションのアプローチの目的は、各変更の理由を理解するためです。

- 繰り返し #1 - は、アプリケーションを作成します。 最初のイテレーションを作成、連絡先マネージャー最も簡単な方法で考えられるします。 基本的なデータベース操作のサポートを追加します: 作成、読み取り、更新、および削除 (CRUD)。

- 繰り返し #2 - は、素敵に見えるアプリケーションを作成します。 このイテレーションで、アプリケーションの見た目を向上させる、既定の ASP.NET MVC ビュー マスター ページを変更し、カスケード スタイル シート。

- 繰り返し #3 - フォーム検証を追加します。 3 番目のイテレーションでは、基本的なフォーム検証を追加します。 ユーザーは、必要なフォームのフィールドを完了しなくても、フォームを送信できないようにようにします。 また、電子メール アドレスと電話番号を検証しました。

- 繰り返し #4 - は、アプリケーションを疎結合を作成します。 この 4 番目のイテレーションで、保守し、Contact Manager アプリケーションの変更を容易にできるようにするソフトウェア設計パターンをいくつかの利点を実行します。 たとえば、Repository パターンと依存関係の注入パターンを使用するようにアプリケーションをリファクタリングします。

- 繰り返し #5 - 単体テストを作成します。 5 番目のイテレーションでアプリケーションと簡単に維持単体テストを追加して変更します。 データ モデル クラスの模擬テストを実行し、コント ローラーと検証ロジックの単体テストをビルドします。

- 繰り返し #6 - は、テスト駆動開発を使用します。 この 6 番目のイテレーションでは、アプリケーションに新しい機能を追加には、まず単体テストの記述、単体テストに対してコードを記述します。 このイテレーションは、連絡先グループを追加します。

- 繰り返し #7 - Ajax 機能を追加します。 7 番目のイテレーションで改良、応答性と、アプリケーションのパフォーマンスの Ajax のサポートを追加します。

## <a name="this-iteration"></a>このイテレーション

Contact Manager アプリケーションの前のイテレーションでは、コードの安全策を提供する単体テストを作成しました。 単体テストを作成するための意図は、コードを変更に柔軟に対応することでした。 インプレース単体テストでさいわいにも、コードを変更をすぐに既存の機能が崩れているかどうかを把握できます。

このイテレーションでは、まったく別の目的で単体テストを使用します。 このイテレーションでと呼ばれるアプリケーション設計理念の一部として単体テストを使用します*テスト駆動開発*します。 テスト駆動開発を練習するときは、まずテストを記述し、テストに対してコードを書き込みます。

正確には、テスト駆動開発を実践している場合があるコードを作成するときに完了する 3 つの手順 (赤/緑/リファクタリング)。

1. (赤) に失敗した単体テストを記述します。
2. 単体テスト (緑) に合格するコードを記述します。
3. (リファクタリング)、コードをリファクタリングします。

最初に、単体テストを記述します。 単体テストでは、動作する方法、コードが期待どおりの意図したものを表現する必要があります。 最初に、単体テストを作成するときに、単体テストは失敗します。 テストに合格するアプリケーション コードを記述していないために、テストが失敗する必要があります。

次に、単体テストが成功するためのに十分なコードを記述します。 目標は、laziest、sloppiest 可能な最速の方法でコードを記述することです。 アプリケーションのアーキテクチャについて考える時間がかからなく必要があります。 代わりに、単体テストで表現するという意図を満たすために必要なコード量が最小限の作成に集中する必要があります。

最後に、十分なコードを記述した後は、前に戻るし、アプリケーションの全体的なアーキテクチャを検討してください。 (リファクタリング) を書き直す場合、この手順でソフトウェアの設計を活用して、コード パターン - リポジトリ パターン - など、コードが保守しやすくなります。 この手順では、コードは、コードが単体テストでカバーされるため、安心書き直すことができます。

テスト駆動開発を実践に起因する多くの利点があります。 まず、テスト駆動開発を強制的に実際に書き込まれる必要のあるコードに専念すること。 常に特定のテストに合格するための十分なコード記述に注目している、ためから、雑草を歩き回っていると、膨大な量の利用しないコードを記述できません。

第 2 に、「最初のテスト」の設計方法論は、コードの使用方法の観点からコードを記述することを強制します。 つまり、テスト駆動開発を実践するには、ときに常に、テストを作成するユーザーの観点から。 そのため、テスト駆動開発が簡潔でわかりやすくし Api にされることができます。

最後に、テスト駆動開発は、通常、アプリケーションの作成のプロセスの一部として単体テストを作成することを強制します。 プロジェクトの期限が近づくとテストは通常、ウィンドウが最初に行うことです。 テスト駆動開発、実践しているときにその一方がテスト駆動開発は、アプリケーションをビルド プロセスに単体テストを中央ため単体テストの記述に関する好する可能性が高くなります。

> [!NOTE] 
> 
> Michael Feathers の書籍を読むことを勧めはテスト駆動開発の詳細については、**レガシー コードを効率的に作業**します。


このイテレーションでは、Contact Manager アプリケーションに新しい機能を追加します。 連絡先グループのサポートを追加します。 ビジネスなどのカテゴリに連絡先を整理する連絡先グループと友人のグループを使用することができます。

この新しい機能のテスト駆動開発のプロセスに従って、アプリケーションに追加します。 最初に、単体テストを記述し、すべてのこれらのテストに対して、コードを記述します。

## <a name="what-gets-tested"></a>どのようなテストを取得します

前のイテレーションで説明したように通常データ アクセス ロジックの単体テストを記述したりしないロジックを表示します。 比較的低速の操作は、データベースにアクセスするため、データ アクセス ロジックの t 書き込み単体テストはありません。 比較的低速の操作には、web サーバーを開始して、ビューにアクセスする必要があるために、ビュー ロジックの t 書き込み単体テストがありません。 T べきでは、テスト実行できる何度も高速な場合を除き、単体テストを記述します。

テスト駆動開発は、単体テストによって主導別、ので注目最初にコント ローラーとビジネス ロジックを記述します。 私たちは、データベースまたはビューを変更しないでください。 T 獲得したデータベースを変更したり、このチュートリアルの最後までビューを作成します。 まず何をテストすることができます。

## <a name="creating-user-stories"></a>ユーザー ストーリーの作成

テスト駆動開発を実践するにはとき、に、テストを記述することで常にで開始します。 これは、直後に疑問が出てきます。 最初に記述するには、どのようなテストを決定するでしょうか。 この質問のセットを書き込む必要があります[**ユーザー ストーリー**](http://en.wikipedia.org/wiki/User_stories)します。

ユーザー ストーリーは、ソフトウェア要件 (通常は 1 つの文) を簡単に説明します。 ユーザーの観点から書き込まれた要件の非技術的な説明が必要です。

ここでは、新しい連絡先グループ機能に必要な機能について説明しているユーザー ストーリーのセット。

1. ユーザーは、連絡先グループの一覧を表示できます。
2. ユーザーは、新しい連絡先グループを作成できます。
3. ユーザーは、既存の連絡先グループを削除できます。
4. ユーザーは、新しい連絡先を作成するときに、連絡先グループを選択できます。
5. ユーザーは、既存の連絡先を編集するときに、連絡先グループを選択できます。
6. 連絡先グループの一覧は、インデックス ビューに表示されます。
7. ユーザーは、連絡先グループをクリックすると、該当する連絡先の一覧が表示されます。

このユーザー ストーリーの一覧が顧客によって完全に理解できることに注意してください。 技術的な実装の詳細の記述は含まれません。

アプリケーションのビルド プロセス中に、一連のユーザー ストーリーはより洗練されたになります。 ユーザー ストーリーは、複数のストーリー (要件) に壊れることがあります。 たとえば、新しい連絡先グループを作成するが検証が含まれることをことがあります。 名前のない連絡先グループを送信すると、検証エラーを返す必要があります。

ユーザー ストーリーの一覧を作成した後は、最初の単体テストを記述する準備が完了したら。 連絡先グループの一覧を表示するための単体テストの作成から始めます。

## <a name="listing-contact-groups"></a>連絡先グループを一覧表示します。

この最初のユーザー ストーリーは、ユーザーが連絡先グループの一覧を表示できることです。 テストをこのストーリーを表現する必要があります。

ContactManager.Tests プロジェクトで、Controllers フォルダーを右クリックして新しい単体テストの作成を選択すると**追加]、[新しいテスト**を選択して、**単体テスト**テンプレート (図 1 参照)。 名前の新しい単位が GroupControllerTest.cs をテストし、をクリックして、 **OK**ボタンをクリックします。


[![GroupControllerTest 単体テストを追加します。](iteration-6-use-test-driven-development-cs/_static/image1.jpg)](iteration-6-use-test-driven-development-cs/_static/image1.png)

**図 01**: GroupControllerTest 単体テストを追加する ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image2.png))。


リスト 1 で最初の単体テストが含まれています。 このテストでは、グループのコント ローラーの Index() メソッドがグループのセットを返しますを確認します。 テストは、グループのコレクションがビュー内のデータで返されることを確認します。

**1 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample1.cs)]

Visual Studio ではリスト 1 で最初に、コードを入力すると、大量の赤の波線が表示されます。 GroupController またはグループのクラスを作成していません。

この時点では、t もビルドできる t ように、アプリケーションは、最初の単体テストを実行します。 お勧めします。 失敗したテストとしてをカウントします。 そのため、ここでアプリケーション コードの記述を開始するアクセス許可があります。 このテストを実行するための十分なコードを記述する必要があります。

リスト 2 でのグループのコント ローラー クラスには、単体テストに合格するために必要なコードの最低限が含まれています。 Index() アクションは、グループ (グループ クラスは、リスト 3 で定義されます) の静的にコード化された一覧を返します。

**2 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample2.cs)]

**3 - Models\Group.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample3.cs)]

最初の単体テストが正常に完了後に GroupController とグループのクラスをプロジェクトを追加します (図 2 参照)。 テストに合格するために必要な最低限の作業を行っています。 時間を記念することをお勧めします。


[![Success!](iteration-6-use-test-driven-development-cs/_static/image2.jpg)](iteration-6-use-test-driven-development-cs/_static/image3.png)

**図 02**: 成功! ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image4.png))。


## <a name="creating-contact-groups"></a>連絡先グループを作成します。

これで、2 つ目のユーザー ストーリー進むことができます。 新しい連絡先グループを作成できる必要があります。 テストでこの意図を表現する必要があります。

リスト 4 のテストは、新しいグループを持つメソッドは、Index() メソッドによって返されるグループの一覧に、グループを追加します。 Create() を呼び出すことを確認します。 つまり、新しいグループを作成した場合、必要のある Index() メソッドによって返されるグループの一覧から戻り、新しいグループを取得できません。

**4 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample4.cs)]

リスト 4 のテストでは、グループのコント ローラーで新しい連絡先グループ Create() メソッドを呼び出します。 次に、テストでは、データの表示で新しいグループを返すグループ コント ローラー Index() メソッドを呼び出すことを確認します。

リスト 5 で変更されたグループ コント ローラーには、必要な最小限新しいテストに合格するために必要な変更にはが含まれています。

**5 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample5.cs)]

## <a name="the-group-controller-in-listing-5-has-a-new-create-action-this-action-adds-a-group-to-a-collection-of-groups-notice-that-the-index-action-has-been-modified-to-return-the-contents-of-the-collection-of-groups"></a>リスト 5 でグループのコント ローラーでは、新しい Create() アクションを持ちます。 このアクションでは、グループのコレクションにグループを追加します。 グループのコレクションのコンテンツを返す Index() アクションが変更されたことに注意してください。

もう一度に、最低限の単体テストに合格するために必要な作業を実行しました。 グループのコント ローラーにこれらの変更を行った後、単位のすべてのパスをテストします。

## <a name="adding-validation"></a>検証の追加

この要件は、ユーザー ストーリーで明示的に報告されていません。 ただし、グループに名前があることを要求する妥当なは。 それ以外の場合、連絡先グループに編成しない非常に便利になります。

6 を一覧表示するには、この意図を表す新しいテストが含まれています。 このテストでは、モデルの状態の検証エラー メッセージに名前の結果を指定せずにグループを作成しようとすることを確認します。

**6 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample6.cs)]

このテストを満たすために、グループのクラス (7 のリストを参照) に Name プロパティを追加する必要があります。 さらに、グループ コント ローラー s Create() アクション (8 のリストを参照) をわずかな検証ロジックを追加する必要があります。

**7 - Models\Group.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample7.cs)]

**8 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample8.cs)]

今すぐ Create() のアクション グループのコント ローラーにはロジック検証とデータベースの両方にはが含まれていることを確認します。 現時点では、グループのコント ローラーによって使用されるデータベースは、nothing よりもメモリ内コレクションで構成されます。

## <a name="time-to-refactor"></a>リファクタリングする時間

赤/緑/リファクタリングの 3 番目の手順では、リファクタリング部分です。 この時点では、コードからステップのデザインを向上させるために、アプリケーションのリファクタリングを考慮する必要があります。 リファクタリング ステージは、位置真剣に考えてソフトウェア設計の原則とパターンを実装する最善の方法の段階です。

自由に、コードの設計を向上させるために選択した任意の方法でコードを変更しています。 既存の機能を損なうを妨げている単体テストのセーフティ ネットがあります。

現在のところ、グループ コント ローラーは、優れたソフトウェア設計の観点から混乱します。 グループのコント ローラーには、検証とデータ アクセス コードのすぎなく混乱が含まれています。 単一責任の原則に違反しないように、さまざまなクラスをこれらの問題を分離する必要があります。

リファクタリングされたグループのコント ローラー クラスは、9 の一覧に含まれます。 ContactManager サービス層を使用するには、コント ローラーを変更されています。 これは、同じサービス層にお問い合わせくださいコント ローラーを使用します。

10 を一覧表示するには、検証、一覧表示、およびグループの作成をサポートするために、ContactManager サービス層に追加された新しいメソッドが含まれています。 新しいメソッドを含める IContactManagerService インターフェイスが更新されました。

11 を一覧表示するには、IContactManagerRepository インターフェイスを実装する新しい FakeContactManagerRepository クラスが含まれています。 を IContactManagerRepository インターフェイスも実装する EntityContactManagerRepository クラスとは異なり、新しい FakeContactManagerRepository クラスは、データベースと通信しません。 FakeContactManagerRepository クラスは、データベースのプロキシとしてのメモリ内コレクションを使用します。 このクラス、単体テストで偽リポジトリ層として使用します。

**9 - Controllers\GroupController.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample9.cs)]

**10 - Controllers\ContactManagerService.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample10.cs)]

**11 - Controllers\FakeContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample11.cs)]

使用して EntityContactManagerRepository クラスで CreateGroup() と ListGroups() メソッドを実装するインターフェイスが必要です IContactManagerRepository を変更します。 これを行う laziest で時間のかからない方法は、次のようにスタブ メソッドを追加します。   

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample12.cs)]


最後に、これらの変更をアプリケーションの設計には、いくつかの単体テストを変更することが必要です。 これで、単体テストを実行するときに、FakeContactManagerRepository を使用する必要があります。 更新された GroupControllerTest クラスは、12 の一覧に含まれます。

**12 - Controllers\GroupControllerTest.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample13.cs)]

これらのすべての操作を行った後の変更をもう一度、すべての単体テスト成功の。 赤/緑/リファクタリングのサイクル全体が完了しました。 最初の 2 つのユーザー ストーリーを実装しました。 単体テストをサポートしているユーザー ストーリーで表される要件があるようになりました。 ユーザー ストーリーの残りの部分を実装するには、同じ赤/緑/リファクタリングのサイクルを繰り返す必要があります。

## <a name="modifying-our-database"></a>データベースの変更

残念ながら、すべての単体テストによって表される要件を満たすことも、作業は行われません。 このデータベースを変更する必要があります。

新しいグループのデータベース テーブルを作成する必要があります。 この場合は、以下の手順に従ってください。

1. サーバー エクスプ ローラー ウィンドウで テーブル フォルダーを右クリックし、メニュー オプションを選択**新しいテーブルの追加**します。
2. 次のテーブル デザイナーで説明する 2 つの列を入力します。
3. 主キーと Id 列として Id 列をマークします。
4. 名前のグループで、新しいテーブルを保存するには、フロッピー ディスクのアイコンをクリックします。

<a id="0.11_table01"></a>


| **列名** | **データ型** | **Null を許容します。** |
| --- | --- | --- |
| ID | int | False |
| name | nvarchar (50) | False |


次に、Contacts テーブルからすべてのデータを削除する必要があります (それ以外の場合、獲得した連絡先およびグループのテーブル間のリレーションシップを作成することはできません)。 この場合は、以下の手順に従ってください。

1. Contacts テーブルを右クリックし、メニュー オプションを選択**テーブル データの表示**します。
2. すべての行を削除します。

次に、グループのデータベース テーブルと既存の連絡先データベース テーブルの間のリレーションシップを定義する必要があります。 この場合は、以下の手順に従ってください。

1. テーブル デザイナーを開き、サーバー エクスプ ローラー ウィンドウで、Contacts テーブルをダブルクリックします。
2. GroupId 連絡先テーブルには、新しい整数型の列を追加します。
3. 外部キーのリレーションシップ ダイアログを開きますリレーションシップ ボタンをクリックします (図 3 を参照してください)。
4. [追加] ボタンをクリックします。
5. テーブルと列の指定 ボタンの横に表示される省略記号ボタンをクリックします。
6. テーブルと列 ダイアログで、主キー テーブルに主キー列として Id とグループを選択します。 外部キー テーブルと外部キー列として GroupId として連絡先を選択します (図 4 参照)。 [Ok] ボタンをクリックします。
7. **INSERT および UPDATE の指定**、値を選択して**Cascade**の**ルールの削除**します。
8. 外部キーのリレーションシップ ダイアログを閉じる閉じる ボタンをクリックします。
9. Contacts テーブルに対する変更を保存する [保存] ボタンをクリックします。


[![データベース テーブルのリレーションシップを作成します。](iteration-6-use-test-driven-development-cs/_static/image3.jpg)](iteration-6-use-test-driven-development-cs/_static/image5.png)

**図 03**: データベースのテーブル リレーションシップの作成 ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image6.png))。


[![テーブルのリレーションシップを指定します。](iteration-6-use-test-driven-development-cs/_static/image4.jpg)](iteration-6-use-test-driven-development-cs/_static/image7.png)

**図 04**: テーブルのリレーションシップを指定する ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image8.png))。


### <a name="updating-our-data-model"></a>このデータ モデルを更新します。

次に、新しいデータベース テーブルを表すため、データ モデルを更新する必要があります。 この場合は、以下の手順に従ってください。

1. エンティティ デザイナーを開く、Models フォルダーに ContactManagerModel.edmx ファイルをダブルクリックします。
2. デザイナー画面を右クリックし、メニュー オプションを選択**データベースからモデルを更新**します。
3. 更新ウィザードでは、選択、グループのテーブルし、完了 をクリックします。 は、(図 5 を参照してください) ボタンします。
4. グループ エンティティを右クリックし、メニュー オプションを選択**の名前を変更**します。 名前を変更、*グループ*エンティティ*グループ*(単数形)。
5. Contact エンティティの下部に表示されるグループ ナビゲーション プロパティを右クリックします。 名前を変更、*グループ*ナビゲーション プロパティを*グループ*(単数形)。


[![データベースから Entity Framework モデルを更新しています](iteration-6-use-test-driven-development-cs/_static/image5.jpg)](iteration-6-use-test-driven-development-cs/_static/image9.png)

**図 05**: データベースから Entity Framework モデルを更新しています ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image10.png))。


次の手順を完了すると、データ モデルは、連絡先とグループの両方のテーブルを表します。 エンティティ デザイナーは、両方のエンティティを表示する必要があります (図 6 参照)。


[![エンティティ デザイナーのグループと連絡先を表示します。](iteration-6-use-test-driven-development-cs/_static/image6.jpg)](iteration-6-use-test-driven-development-cs/_static/image11.png)

**図 06**: グループと連絡先を表示するエンティティ デザイナー ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image12.png))。


### <a name="creating-our-repository-classes"></a>リポジトリ クラスを作成します。

次に、リポジトリ クラスを実装する必要があります。 このイテレーションの過程で、単体テストを満たすためにコードの書き込み中に IContactManagerRepository インターフェイスにいくつかの新しいメソッドをしました。 IContactManagerRepository インターフェイスの最終バージョンは、14 の一覧に含まれます。

**14 - Models\IContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample14.cs)]

私たち haven t が実際に連絡先グループの操作に関連するメソッドのいずれかを実装します。 現時点では、EntityContactManagerRepository クラスでは、各 IContactManagerRepository インターフェイスに表示されている連絡先グループ メソッド スタブのメソッドがあります。 たとえば、現在 ListGroups() メソッドが次のようになります。

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample15.cs)]

スタブ メソッドをアプリケーションをコンパイルして、単体テストに合格するようになりました。 ただし、いよいよを実際にこれらのメソッドを実装します。 EntityContactManagerRepository クラスの最終バージョンは、13 の一覧に含まれます。

**13 - Models\EntityContactManagerRepository.cs を一覧表示します。**

[!code-csharp[Main](iteration-6-use-test-driven-development-cs/samples/sample16.cs)]

### <a name="creating-the-views"></a>ビューを作成します。

既定の ASP.NET ビュー エンジンを使用する場合の ASP.NET MVC アプリケーションです。 そのため、don t は、特定の単体テストへの応答でビューを作成します。 ただし、ため、アプリケーションが役に立たないビューである場合はなるべく t の作成と Contact Manager アプリケーションに含まれるビューを変更せずにこのイテレーションを完了します。

ビューを作成する次新しい連絡先グループを管理するため (図 7 を参照してください) 必要があります。

- Views\Group\Index.aspx - 連絡先グループの一覧を表示
- Views\Group\Delete.aspx - 連絡先グループの削除確認フォームを表示します


[![グループのインデックス ビュー](iteration-6-use-test-driven-development-cs/_static/image7.jpg)](iteration-6-use-test-driven-development-cs/_static/image13.png)

**図 07**: グループ インデックス ビュー ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image14.png))。


連絡先グループが含まれるように、次の既存のビューを変更する必要があります。

- Views\Home\Create.aspx
- Views\Home\Edit.aspx
- Views\Home\Index.aspx

このチュートリアルに付属する Visual Studio アプリケーションを調べることで変更されたビューを表示できます。 たとえば、図 8 は、連絡先のインデックス ビューを示しています。


[![連絡先のインデックス ビュー](iteration-6-use-test-driven-development-cs/_static/image8.jpg)](iteration-6-use-test-driven-development-cs/_static/image15.png)

**図 08**: 連絡先インデックス ビュー ([フルサイズの画像を表示する をクリックします](iteration-6-use-test-driven-development-cs/_static/image16.png))。


## <a name="summary"></a>まとめ

このイテレーションでテスト駆動開発アプリケーションの設計方法に従って、Contact Manager アプリケーションに新しい機能をしました。 ユーザー ストーリーのセットを作成して開始したとします。 ユーザー ストーリーによって表される要件に対応する一連の単体テストを作成しました。 最後に、単体テストによって表される要件を満たすのに十分なコードを記述します。

単体テストによって表される要件を満たすのに十分なコードを記述完了すると、データベースとビューが更新されます。 データベースに新しいグループのテーブルを追加し、Entity Framework データ モデルを更新します。 私たちも作成され、一連のビューを変更します。

次のイテレーション - 最後のイテレーション - は、Ajax を利用するようにアプリケーションを書き直します。 Ajax を利用して、応答性と Contact Manager アプリケーションのパフォーマンスを改善します。

> [!div class="step-by-step"]
> [前へ](iteration-5-create-unit-tests-cs.md)
> [次へ](iteration-7-add-ajax-functionality-cs.md)
