---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: 自動化された単体テストを有効にする |Microsoft ドキュメント
author: microsoft
description: 手順 12 では、当社 NerdDinner 機能を確認して、これが得変更を加えるという自信を持って、自動化された単体テストのスイートを開発する方法を示します.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: fede08be7e06327c6d04fa5d36f7dd818d79b380
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30875631"
---
<a name="enable-automated-unit-testing"></a>自動化された単体テストを有効にします。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF をダウンロードします。](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 12. ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク-にする方法を小規模の構築が完了すると、ASP.NET MVC 1 を使用して web アプリケーションです。
> 
> 手順 12 では、自動化された単体テスト、NerdDinner 機能を確認して、これが得アプリケーションに将来の機能強化と変更を信頼度のスイートを開発する方法を示します。
> 
> ASP.NET MVC 3 を使用している場合ことをお勧めする、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアルです。


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner 手順 12: 単体テスト

自動化された単体テスト、NerdDinner 機能を確認して、これが得アプリケーションに将来の機能強化と変更を信頼度のスイートを開発してみましょう。

### <a name="why-unit-test"></a>なぜ単体テストのですか。

作業 1 つの朝にドライブ上で作業しているアプリケーションに関する発想の突然のフラッシュがある場合。 大幅に向上アプリケーションを構成することができますを実装する変更があるがわかっています。 リファクタリング コードをクリーンアップ、新機能を追加または、バグが修正される可能性があります。

お使いのコンピューターで受信したときにする、という疑問 –「安全な方法はこの機能強化を作成するか。」 場合、変更を行った副作用および中断ものか。 変更が単純なおよびのみを実装するには数分がかかる場合は、すべてのアプリケーション シナリオを手動でテストする時間がかかる可能性がありますか。 What-if シナリオをカバーするを忘れるし、壊れたアプリケーションは、運用環境に導入しますか。 すべての工数すべき本当にこの機能強化を行っているか。

自動化された単体テストでは、継続的に、アプリケーションを強化することができます安全策を提供できで作業しているコードを恐れてを回避することができます。 機能を使用すると、コードでは、信頼: とを支援する、それ以外の場合いないを感じる快適な改善点を迅速に検証するテストを自動化することを行っています。 これらより優れたソリューションを作成し、投資収益率を大幅に高くする潜在顧客のより長い有効期間にも役立ちます。

ASP.NET MVC フレームワークを使用すると、簡単かつ自然単体テストのアプリケーション機能にします。 テスト駆動開発 (TDD) ワークフローをベースのテスト ファースト開発を有効にすることもできます。

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

このチュートリアルの先頭に NerdDinner アプリケーションを作成した際おが表示されたか、アプリケーション プロジェクトと共に移動する単体テスト プロジェクトを作成する必要かどうかを確認するダイアログ ボックス。

![](enable-automated-unit-testing/_static/image1.png)

私たちは保持「はい、単体テスト プロジェクトを作成する」ラジオ ボタンを選択: がソリューションに追加されている"NerdDinner.Tests"プロジェクトで発生したためです。

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests プロジェクトでは、NerdDinner アプリケーション プロジェクトのアセンブリを参照され、アプリケーションの機能を確認して自動テストを簡単に追加することができます。

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Dinner モデル クラスの単体テストの作成

NerdDinner.Tests プロジェクト、モデルのレイヤーを作成したときに作成した Dinner クラスを検証するには、いくつかのテストを追加してみましょう。

このモデルに関連するテストを配置おします"Models"と呼ばれる、テスト プロジェクト内に新しいフォルダーを作成することで始めます。 フォルダーを右クリックし、選択、**追加 -&gt;新しいテスト**メニュー コマンド。 これは、「新しいテストの追加」ダイアログが表示されます。

「単体テスト」を作成し、"DinnerTest.cs"という名前を選択します。

![](enable-automated-unit-testing/_static/image3.png)

"Ok"のボタンをクリックしたとき Visual Studio が追加 (され) DinnerTest.cs ファイルをプロジェクト。

![](enable-automated-unit-testing/_static/image4.png)

Visual Studio 単体テストの既定のテンプレートは、一連の内部に少し乱れたの検索をボイラー プレート コードがします。 みましょうをクリーンアップすることだけ次のコードを格納します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

DinnerTest クラス上で、[TestClass] 属性は、テスト、だけでなく省略可能なテストの初期化および終了コードを格納するクラスとして識別されます。 その中のテストに [TestMethod] 属性を持つパブリック メソッドを追加して定義できます。

次は、2 つ追加を実行するテスト、夕食クラスの最初の数値を示します。 最初のテストでは、当社 Dinner が無効である新しい Dinner が正しく設定されているすべてのプロパティを指定せずに作成された場合を確認します。 2 番目のテストでは、夕食に有効な値で設定されたすべてのプロパティがある場合、夕食が有効であることを確認します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

お気づきの上、テスト名は、明示的に指定 (とある程度詳細な) します。 数百または数千の小規模のテストの作成最終的に可能性があります (特に、テスト ランナーでのエラーの一覧を対象にしている) 場合に、それぞれの動作と目的をすばやく判断しやすくために、これを行っています。 テストの名前をテストする機能をちなんだ必要があります。 以降を使用して、"名詞\_必要があります\_動詞"の名前付けパターン。

テスト パターン –「Arrange、Act、Assert」を意味する"AAA"を使用してテストを構造化お。

- 単体テスト中にセットアップを整列します。
- Act: 単体テストを実行し、結果をキャプチャ
- アサート: 動作を確認してください。

記述するときおにならないように個々 のテストのテストが多すぎるしないでください。 代わりに各テストは、のみ、1 つの概念 (がかなり簡単にするエラーの原因を特定する) を確認してください。 アサート ステートメント テストごとに 1 つのみがあることをお勧めします。 1 つ以上のアサート ステートメントをテスト メソッドである場合は場合、は、それらすべてに使用されている、同じ概念をテストして確認します。 確かでない場合は、別のテストを作成します。

### <a name="running-tests"></a>テストを実行しています

Visual Studio 2008 Professional (および上位エディション) には、IDE 内でプロジェクトを Visual Studio 単体テストの実行に使用できる組み込みのテスト ランナーが含まれます。 選択する、**テスト -&gt;実行 -&gt;ソリューションのすべてのテスト**メニュー コマンド (または型 Ctrl R A) をすべての単体テストを実行します。 または特定のテスト クラスまたはテスト メソッド内で、カーソルの位置を使用しまたは、**テスト -&gt;実行 -&gt;の現在のコンテキストのテスト**単体テストのサブセットを実行するメニュー コマンド (または ctrl キーを押し R 型 T)。

みましょう DinnerTest クラス内でカーソルを配置して、"Ctrl R、T"を定義したテストの実行、2 つに入力します。 Visual Studio に表示されるときにこの作業を行う「テスト結果」ウィンドウと、このテストの実行結果内に表示されている後ほどお見せ。

![](enable-automated-unit-testing/_static/image5.png)

*注: VS テスト結果 ウィンドウは、既定でのクラス名列を表示していません。これは、テスト結果 ウィンドウ内で右クリックし、列の追加/削除 メニューのコマンドを使用して追加できます。*

この 2 つのテスト時間に実行してすることができます。 1 秒あたりの一部のみ渡される、どちらも参照してください。 特定のルールの検証、ことを確認できるだけでなく、次の 2 つのヘルパー メソッドの IsUserHost() および IsUserRegisterd() – Dinner クラスに追加したをカバーするテストを作成することでそれらを拡張して」に進みますおできます。 Dinner クラスのためにこれらのすべてのテストをことによりより簡単かつ安全に後で新しいビジネス ルールと検証を追加します。 夕食に、新しい規則のロジックを追加し、数秒以内、以前のロジック機能のいずれか、破損していないことを確認してください。

どのようにわかりやすいテスト名を使用しやすい各テストの確認をすばやく把握することを確認します。 使用することをお勧め、 **Tools -&gt;オプション**メニュー コマンド、開く、テスト ツール-&gt;テストの実行構成 画面で、およびチェック"障害が発生したか結果が不確定の単体テストの結果をダブルクリックするとが表示されます。テストの障害発生時点"のチェック ボックスをオンします。 こうと、テスト結果 ウィンドウでエラーをダブルクリックし、assert 障害にすぐにジャンプすることです。

### <a name="creating-dinnerscontroller-unit-tests"></a>DinnersController 単体テストの作成

当社 DinnersController 機能を確認するいくつかの単体テストを今すぐ作成してみましょう。 うまく、テスト プロジェクト内の「コント ローラー」フォルダーを右クリックして、起動し、選択し、、**追加 -&gt;新しいテスト**メニュー コマンド。 「単体テスト」を作成し、名前"DinnersControllerTest.cs"おします。

DinnersController Details() アクション メソッドを検証する 2 つのテスト メソッドを作成します。 最初は、既存の Dinner が要求されたときに、ビューが返されることを確認します。 2 つ目は、"NotFound"のビューが存在しない Dinner が要求されたときに返されることを確認します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

上記のコードがクリーンにコンパイルします。 テストを実行したときに、どちらも失敗します。

![](enable-automated-unit-testing/_static/image6.png)

エラー メッセージを見ると、テストが失敗したが、DinnersRepository クラスできなかったので、データベースに接続することをおわかります。 NerdDinner アプリケーションがローカル SQL Server Express のファイルを \App の下に存在する接続文字列を使用して\_NerdDinner アプリケーション プロジェクトのデータ ディレクトリ。 NerdDinner.Tests プロジェクトでコンパイルして実行を別のディレクトリ、アプリケーション プロジェクトであるため、接続文字列の相対パスの場所が正しくありません。

お*でした*テスト プロジェクトに、SQL Express データベース ファイルをコピーすることによってこれを修正しをテスト プロジェクトの App.config にある適切なテスト接続文字列を追加します。 これは、ブロック解除され、実行する上記のテストを取得します。

実際のデータベースを使用してコードの単体テストに含まれるチャレンジの数。 具体的には、次のように使用します。

- 単体テストの実行時間を大幅に遅くなります。 それらを頻繁に実行する可能性が低く、テストを実行する時間がかかります。 秒 – で実行してそれを行うものとして、プロジェクトをコンパイルすると、必然的にすることができるに単体テストすることをお勧めします。
- テスト内のセットアップおよび後処理用のロジックが複雑になります。 各単体テストを作成する分離 (なし、副作用または依存関係) の他の独立したにします。 実際のデータベースに対して作業を行うときにする必要がある状態に注意し、テストの状態にリセットします。

"依存関係の挿入"これらの問題を回避し、マイクロソフトによるテストで実際のデータベースを使用する必要を避けるために役立つと呼ばれるデザイン パターンを見てみましょう。

### <a name="dependency-injection"></a>依存関係の挿入

今すぐ DinnersController は「緊密」DinnerRepository クラスにします。 「結合」は、ここで、明示的に依存している別のクラス機能するために状態を表します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

DinnerRepository クラスには、データベースへのアクセスが必要であるため、密に結合された依存関係 DinnersController クラスには、DinnersController アクション メソッドをテストするためにデータベースが存在することを必要とするを DinnerRepository 両端にします。

この問題を回避"依存関係の挿入"– あるアプローチ (データ アクセスを提供するリポジトリ クラス) のような依存関係がそれらを使用するクラス内で不要になった暗黙的に作成される場所と呼ばれるデザイン パターンを採用することによって取得できます。 代わりに、依存関係に明示的に渡せるそれらを使用するクラスのコンス トラクター引数を使用します。 依存関係は、インターフェイスを使用して定義されているが場合、おし、単体テストのシナリオ用の「偽」依存関係の実装で渡す柔軟性があります。 これにより、実際にデータベースへのアクセスを必要としないテストに固有の依存関係の実装を作成することができます。

この動作を確認、DinnersController による依存性の注入を実装してみましょう。

#### <a name="extracting-an-idinnerrepository-interface"></a>IDinnerRepository インターフェイスの抽出

取得および更新ディナー、コント ローラーが必要とするリポジトリ コントラクトをカプセル化する新しい IDinnerRepository インターフェイスを作成する、最初の手順がされます。

このインターフェイス コントラクト \Models フォルダーを右クリックし を選択して手動で定義できます、**追加 -&gt;新しい項目の**メニュー コマンドと IDinnerRepository.cs をという名前の新しいインターフェイスを作成します。

または、既存の DinnerRepository クラスからのインターフェイスを作成および使用リファクタリング ツールの Visual Studio Professional に組み込ま (および上位エディション) に自動的に抽出おできます。 VS を使用してこのインターフェイスを抽出するには、単に DinnerRepository クラス テキスト エディターで、カーソルの位置しし、右クリックし、**リファクタリング -&gt;インターフェイスの抽出**メニュー コマンド。

![](enable-automated-unit-testing/_static/image7.png)

これでは「インターフェイスの抽出」ダイアログを起動して、作成するには、インターフェイスの名前の入力を求めることです。 IDinnerRepository 既定に設定され、既存の DinnerRepository クラス、インターフェイスに追加するすべてのパブリック メソッドを自動的に選択。

![](enable-automated-unit-testing/_static/image8.png)

"Ok"ボタンをクリックすると Visual Studio は、アプリケーションに新しい IDinnerRepository インターフェイスを追加します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

インターフェイスを実装できるように、既存の DinnerRepository クラスが更新されます。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>コンス トラクターの挿入をサポートするために DinnersController の更新

これで、新しいインターフェイスを使用する DinnersController クラスが更新されます。

現在 DinnersController ハードコードされてその"dinnerRepository"フィールドが DinnerRepository クラスでは常に。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

"DinnerRepository"のフィールドが型 DinnerRepository ではなく IDinnerRepository ようにして変更します。 2 つのパブリック DinnersController コンス トラクターを追加します。 コンス トラクターのいずれかにより、引数として渡される、IDinnerRepository ことができます。 もう 1 つは、既存の DinnerRepository 実装を使用する既定のコンス トラクターです。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

既定では ASP.NET MVC では、既定のコンス トラクターを使用して、コント ローラー クラスを作成するため、DinnersController 実行時に引き続き DinnerRepository クラスを使用してデータ アクセスを実行します。

渡すパラメーターのコンス トラクターを使用して、「偽」dinner リポジトリの実装で、ただし、単体テストを更新できます。 この「偽」dinner リポジトリは、実際のデータベースへのアクセスが不要し、メモリ内のサンプル データを代わりに使用します。

#### <a name="creating-the-fakedinnerrepository-class"></a>FakeDinnerRepository クラスを作成します。

FakeDinnerRepository クラスを作成してみましょう。

うまく NerdDinner.Tests プロジェクト内の"Fakes"ディレクトリを作成することで開始し、新しい FakeDinnerRepository クラスを追加、(フォルダーを右クリックし、選択**追加 -&gt;クラスの新しい**)。

![](enable-automated-unit-testing/_static/image9.png)

FakeDinnerRepository クラス IDinnerRepository インターフェイスを実装できるように、コードが更新されます。 上を右クリックし、コンテキスト メニューのコマンドを「インターフェイスの実装 IDinnerRepository」おことができますし。

![](enable-automated-unit-testing/_static/image10.png)

これにより、Visual Studio で自動的に「スタブ」の既定の実装で、FakeDinnerRepository クラスすべて IDinnerRepository インターフェイス メンバーの追加が発生します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

インメモリ リストを使用する FakeDinnerRepository 実装し、更新することが&lt;Dinner&gt;コレクションがこれをコンス トラクターの引数として渡されます。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

偽の IDinnerRepository 実装は、データベースや Dinner オブジェクトのメモリ内のリストを無効代わりに使用することを今すぐがあります。

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>使用した、FakeDinnerRepository 単体テスト

データベースを使用できなかったために、以前に失敗した DinnersController 単体テストに戻ります。 次のコードを使用して DinnersController にサンプルのインメモリ Dinner データが設定される FakeDinnerRepository を使用するテスト メソッドを更新することをがします。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

これらのテストを実行すると、両方が合格しました。

![](enable-automated-unit-testing/_static/image11.png)

何より、実行するには、2 番目の一部のみを実行し、複雑なセットアップ/クリーンアップ ロジックは必要ありません。 私たちは、実際のデータベースに接続することが必要なせずに単体テストのすべての DinnersController アクション メソッドのコード (インクルード一覧については、ページングは、詳細については、作成、更新および削除) のようになりましたことができます。

| **依存関係の挿入フレームワークの側トピックの内容** |
| --- |
| 正常に機能しますが、依存関係の数と維持が困難になりますが、(上は) 同じように、手動による依存関係の挿入を実行して、アプリケーション内のコンポーネントが向上します。 いくつかの依存関係の挿入フレームワークは、さらに多くの依存関係の管理の柔軟性を提供できる .NET に存在します。 これらのフレームワークは、「制御の反転」(IoC) コンテナーとも呼ばを (最もよく使用されるコンス トラクター インジェクション実行時にオブジェクトに依存関係を渡すことを指定するための構成サポートの追加レベルを有効にするメカニズムを提供します。). いくつかの最も一般的な OSS 依存関係の挿入/.net IOC フレームワークが含まれる: AutoFac、Ninject、Spring.NET、StructureMap、および Windsor です。 ASP.NET MVC の公開機能拡張 Api 解像度と、コント ローラーのインスタンス化に参加する開発者を有効にして、依存関係の挿入を有効にする/IoC フレームワークにおいて、このプロセス内で正常に統合します。 DI/IOC フレームワークを使用しても、当社 DinnersController は –、DinnerRepositorys との間の結合を完全に削除がから既定のコンス トラクターを削除することできます。 おして、使用しない依存関係の挿入/NerdDinner アプリケーションと IOC フレームワークです。 ですが、NerdDinner コード ベースおよび機能が大きくなった場合、将来的にすることもお何か。 |

### <a name="creating-edit-action-unit-tests"></a>編集操作の単体テストの作成

これで、DinnersController の編集機能を確認するいくつかの単体テストを作成してみましょう。 最初に、編集の操作の HTTP GET 以外のバージョンをテストします。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

有効な夕食が要求されたときに、DinnerFormViewModel オブジェクトによってバックアップされたビューが表示されることを確認するテストを作成します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

テストを実行したときに、わかります Edit メソッド プロパティにアクセスします User.Identity.Name Dinner.IsHostedBy() チェックを実行するときに、null 参照の例外がスローされたためにが失敗します。

コント ローラーの基本クラスにユーザー オブジェクトは、ログインしたユーザーに関する詳細をカプセル化しは、実行時に、コント ローラーの作成時に ASP.NET MVC によって設定されます。 ユーザー オブジェクトが設定されていないため、web サーバー環境の外部で DinnersController は、テスト、(そのため、null 参照の例外)。

### <a name="mocking-the-useridentityname-property"></a>モック User.Identity.Name プロパティ

モック フレームワークでは、偽のバージョンのマイクロソフトによるテストをサポートする依存オブジェクトを動的に作成することを有効にして簡単にテストを確認します。 たとえば、当社 DinnersController シミュレートされたユーザー名を検索に使用できるユーザー オブジェクトを動的に作成するのにモック フレームワーク、編集操作のテストで使用できます。 これにより、このテストを実行したときにスローされない null 参照が回避されます。

モック フレームワーク ASP.NET MVC で使用できる多くの .NET がある (ここでその一覧を確認できます: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/))。 使用、オープン ソースのモック フレームワーク"Moq"と呼ばれる NerdDinner アプリケーションをテストするには、これからダウンロードできます。 無料[ http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq)です。

ダウンロードされると、参照 NerdDinner.Tests プロジェクト内に追加 Moq.dll アセンブリ。

![](enable-automated-unit-testing/_static/image12.png)

"CreateDinnersControllerAs(username)"ヘルパー メソッドを追加、テスト クラスと、パラメーターとしてユーザー名を受け取り、"mocks"DinnersController インスタンスの User.Identity.Name プロパティ。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

上記のモック オブジェクト (ある ASP.NET MVC はユーザー、要求、応答、およびセッションのようなランタイム オブジェクトを公開するのにコント ローラー クラスに渡されます) ControllerContext オブジェクトの fakes を作成するのに Moq を使用します。 モックする ControllerContext HttpContext.User.Identity.Name プロパティがヘルパー メソッドに渡されたユーザー名の文字列を返すようにすることで"SetupGet"メソッドを呼び出しています。

ControllerContext プロパティとメソッドの任意の数を模擬表示おことができます。 この問題を説明するには、Request.IsAuthenticated プロパティ (– 以下のテストに実際に必要ないないが要求のプロパティを模擬表示する方法を説明するのに役立ちます) SetupGet() 呼び出しを追加しました。 作業が完了するときに ControllerContext モックのインスタンスのヘルパー メソッドを返します DinnersController に割り当てます。

このヘルパー メソッドを使用して、別のユーザーに関係する編集のシナリオをテストする単体テストを記述できますようになりました。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

テストを実行したときに渡すようになりました。

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>テストの UpdateModel() シナリオ

編集の操作の HTTP GET バージョンに対応するテストを作成しました。 編集の操作の HTTP POST のバージョンを確認するいくつかのテストが作成してみましょう。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

このアクション メソッドをサポートするうえで、興味深い新しいテスト シナリオは、コント ローラーの基本クラスで UpdateModel() ヘルパー メソッドの使用です。 このヘルパー メソッドを使用して、値をバインドするフォーム ポストを Dinner オブジェクト インスタンスにしています。

次は、どのフォーム ポストされた UpdateModel() ヘルパー メソッドを使用する値を提供可能を示す 2 つのテストを示します。 FormCollection オブジェクトを作成してこれを行うし、コント ローラーで"ValueProvider"プロパティに割り当てるおします。

最初のテストでは、正常に保存、ブラウザーがリダイレクトされるは、詳細のアクションを確認します。 2 番目のテストでは、無効な入力がポストされたときに、エラー メッセージでは、編集ビューをもう一度再アクションに表示されることを確認します。


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>テストのまとめ

単体テスト コント ローラー クラスに関連する主要な概念を説明してきました。 これらの手法を使用して、何百ものアプリケーションの動作を検証する簡単なテストを簡単に作成することできます。

このコント ローラーとモデルのテストでは、実際のデータベースは必要ありません、ため非常に高速で簡単に実行されます。 秒単位で何百もの自動テストを実行し、すぐに行った変更を超えたものかどうかについてのフィードバックになります。 これにより、意見ご感想を継続的に向上させるため、リファクタリング、およびアプリケーションを調整する、信頼度は役立ちます。

テストを説明した最後のトピックの「– としてテストが何かため、開発プロセス最後に行う必要がありますが、! 反対に、開発プロセスでできるだけ早期自動テストを記述する必要があります。 そのため実行ではきれいにすぐにフィードバックを作成したら、アプリケーションの使用シナリオ、について慎重に検討しを使用してアプリケーションを設計する方法を説明するようを重ねると、結合の点に注意することができます。

後の本の章がテスト駆動開発 (TDD)、および ASP.NET MVC で使用する方法について説明します。 TDD は、反復的なコーディングの推奨事項は、結果として得られるコードが適合するテストを記述する場所です。 TDD で、実装する機能を検証するテストを作成することで各機能を開始します。 テスト最初により、明確に理解しておいて、機能と方法としてサポートされている作業単位を作成します。 テストが記述された (およびが失敗することを確認した) 後にのみか、実際の検証機能を実装します。 既に、機能が動作すると想定される方法のユース ケースを考える時間を費やすことに、ための要件の理解を深める必要がそれらを実装する最善の方法とします。 – テストを再実行してとして即時のフィードバックを取得、実装が完了したときにするかどうか、機能が正しく機能します。 以上の 10 章 TDD を説明します。

### <a name="next-step"></a>次の手順

いくつかの最終的なコメントをラップします。

> [!div class="step-by-step"]
> [前へ](use-ajax-to-implement-mapping-scenarios.md)
> [次へ](nerddinner-wrap-up.md)
