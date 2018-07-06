---
uid: mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
title: 自動単体テストの有効化 |Microsoft Docs
author: microsoft
description: 手順 12 では、NerdDinner 機能、ことを確認して、これにより、変更を加える信頼度に自動化された単体テストのスイートを開発する方法を示します.
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: a19ff2ce-3f7e-4358-9a51-a1403da9c63e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/enable-automated-unit-testing
msc.type: authoredcontent
ms.openlocfilehash: 2247dc2e6d22cc0d5ddba97dfe6c7d2d1b0e49be
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819411"
---
<a name="enable-automated-unit-testing"></a>自動化された単体テストを有効にします。
====================
によって[Microsoft](https://github.com/microsoft)

[PDF のダウンロード](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> これは、無料の手順 12 ["NerdDinner"アプリケーションのチュートリアル](introducing-the-nerddinner-tutorial.md)をウォーク スルーの小さなをビルドしても、ASP.NET MVC 1 を使用して web アプリケーションを実行する方法。
> 
> 手順 12 では、一連の自動化された単体テスト、NerdDinner 機能を確認して、これにより、アプリケーションを今後の機能強化と変更できるという確信を開発する方法を示します。
> 
> 次のことをお勧め ASP.NET MVC 3 を使用している場合、 [MVC 3 の開始と取得](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)または[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)チュートリアル。


## <a name="nerddinner-step-12-unit-testing"></a>NerdDinner 手順 12: 単体テスト

NerdDinner 機能、ことを確認して、これにより、アプリケーションを今後の機能強化と変更できるという確信に自動化された単体テストのスイートを開発しましょう。

### <a name="why-unit-test"></a>なぜ単体テストのでしょうか。

作業 1 つの朝にドライブ上で作業しているアプリケーションについてのヒントの突然のフラッシュがあります。 わかりますが、アプリケーションが大幅に向上する変更を実装することができます。 リファクタリング、コードのクリーンアップ、新しい機能を追加しますまたは、バグが修正される可能性があります。

到着して、コンピューターからでお買い求め問題 –「安全な方法は、この機能強化にでしょうか。」 場合、変更を行った副作用を持っているか、何かを中断しますか。 変更は、可能性がありますはシンプルなものを実装するには数分が時間のすべてのアプリケーション シナリオを手動でテストする場合のみ実行されますか。 What-if シナリオをカバーするを忘れるし、壊れたアプリケーションが運用環境にしますか。 すべての労力を費やす価値は本当にこの機能強化を行ってでしょうか。

自動化された単体テストでは、継続的に、アプリケーションを拡張することができます、セーフティ ネットを提供できで作業しているコードを恐れてを回避することができます。 機能を使用すると、コードでは、信頼度 – とを支援することをする可能性がありますそれ以外の場合いないれたように感じている使いやすい機能強化を迅速に確認するテストを自動化することを実行します。 これらは、保守しやすくなりますがソリューションを作成し、投資収益率を大幅に上回っている潜在顧客の長い有効期間にも役立ちます。

ASP.NET MVC フレームワークを使用すると、簡単かつ自然に単体テスト アプリケーションの機能です。 また、テスト駆動開発 (TDD) のワークフロー ベースのテスト ファースト開発できるようにすることもできます。

### <a name="nerddinnertests-project"></a>NerdDinner.Tests Project

このチュートリアルの先頭に、NerdDinner アプリケーションを作成したときに私たちが表示されたアプリケーション プロジェクトと共に移動する単体テスト プロジェクトを作成したいかどうかを確認するダイアログ。

![](enable-automated-unit-testing/_static/image1.png)

「はい、単体テスト プロジェクトを作成する」ラジオ ボタンを選択: では、ソリューションに追加される"NerdDinner.Tests"プロジェクトに保存しました。

![](enable-automated-unit-testing/_static/image2.png)

NerdDinner.Tests プロジェクトでは、NerdDinner アプリケーション プロジェクトのアセンブリを参照し、アプリケーションの機能を確認して自動テストを簡単に追加できます。

### <a name="creating-unit-tests-for-our-dinner-model-class"></a>Dinner モデル クラスの単体テストの作成

NerdDinner.Tests プロジェクト Dinner クラスを構築した時点で、モデルのレイヤーを作成したことを確認するには、いくつかのテストを追加してみましょう。

まず、モデルに関連するテストを配置します"Models"というテスト プロジェクト内に新しいフォルダーを作成します。 フォルダーを右クリックし、選択、**追加 -&gt;新しいテスト**メニュー コマンド。 「新しいテストの追加」ダイアログが表示されます。

「単体テスト」を作成し、"DinnerTest.cs"を名前を選択します。

![](enable-automated-unit-testing/_static/image3.png)

"Ok"ボタンをクリックすると Visual Studio は追加 (とを開く) DinnerTest.cs ファイルをプロジェクト。

![](enable-automated-unit-testing/_static/image4.png)

既定の Visual Studio 単体テスト テンプレートが、一連のボイラー プレート コード内でこれを少し乱雑な検出します。 みましょうのクリーンアップを次のコードを含めるだけです。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample1.cs)]

上記 DinnerTest クラス [TestClass] 属性は、テスト、および省略可能なテストの初期化と破棄コードを含むクラスとして識別されます。 その中のテストに [TestMethod] 属性を持つパブリック メソッドを追加して定義できます。

Dinner クラスを実行する 2 つのテストを追加の最初の数値を以下に示します。 最初のテストでは、Dinner が無効である新しい Dinner が正しく設定されているすべてのプロパティを指定せずに作成された場合を確認します。 2 番目のテストを夕食にすべてのプロパティを有効な値の設定がある場合、Dinner が有効であるかを確認します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample2.cs)]

わかります上、テスト名でが非常に明示的な (とやや面倒)。 数百または何千もの小規模なテストを作成し終了し、(特に、テスト ランナーでのエラーの一覧を検索) 場合に、それぞれの動作と意図をすばやく判断しやすくために、これを行っています。 テスト名は、テスト機能の後に名前必要があります。 使用している以上、"名詞\_する必要があります\_動詞"の名前付けパターン。

"AAA"–「Arrange、Act、Assert」のパターンのテストを使用して、テスト構造化します。

- テストする単体のセットアップを整列します。
- Act: は、単体テストを実行して、結果をキャプチャ
- 動作を検証するアサート。

記述する場合、個々 のテストを回避するテストが多すぎる行います。 代わりに各テストは、(これはそれがより簡単にエラーの原因を特定) 1 つの概念のみを確認する必要があります。 試して、アサート ステートメント テストごとに 1 つのみがあることをお勧めします。 1 つ以上のテスト メソッドでステートメントのアサートがある場合は、すべて使用されている同じ概念をテストするを確認します。 確かでない場合は、別のテストを作成します。

### <a name="running-tests"></a>テストを実行しています

Visual Studio 2008 Professional (および上位エディション)、IDE 内でプロジェクトを Visual Studio の単体テストの実行に使用できる組み込みのテスト ランナーが含まれます。 選択できる、**テスト -&gt;実行 -&gt;ソリューションのすべてのテスト**メニュー コマンド (または ctrl キーを R では、タイプ A) すべての単体テストを実行します。 または特定のテスト クラスまたはテスト メソッド内でカーソルを配置し、使用して別の方法として、**テスト -&gt;実行 -&gt;の現在のコンテキストのテスト**単体テストのサブセットを実行するメニュー コマンド (または ctrl キーを R では、T 型)。

みましょう DinnerTest クラス内でカーソルを配置し、定義したテストの実行、2 つに"Ctrl R、T"を入力します。 Visual Studio 内でを実行するとこの"テスト結果 ウィンドウが表示され、内に表示されている実行、テストの結果を説明します。

![](enable-automated-unit-testing/_static/image5.png)

*注: VS テストの結果 ウィンドウは、既定のクラス名列を表示していません。これは、テスト結果 ウィンドウ内で右クリックし、列の追加/削除 メニューのコマンドを使用して追加できます。*

2 つのテストでは、時間に実行して、できる限りの秒の端数のみ渡されるときの両方を参照してください。 移動するようになりましたを特定のルールの検証を確認し、2 つのヘルパー メソッド - IsUserHost() と Dinner クラスに追加した IsUserRegisterd() – をカバーするテストを作成してそれらを拡張できます。 Dinner クラスのためにこれらすべてのテストを持つことによりより簡単かつ安全に、今後の新しいビジネス ルールと検証を追加します。 夕食に新しいルール ロジックを追加し、数秒で、以前のロジックの機能のいずれかが破損していないことすることを確認します。

どのようにわかりやすいテスト名を使用して簡単に各テストの検証をすばやく理解に注意してください。 使用することをお勧め、 **Tools -&gt;オプション**メニュー コマンドを開き、テスト ツール-&gt;テストの実行の構成画面で、チェック"ダブルクリックすると失敗したか結果が不確定の単体テストの結果が表示されます。テストの障害発生時点"のチェック ボックスをオンします。 これは、使用すると、テストの結果ウィンドウにエラーをダブルクリックし、アサート失敗にすぐにジャンプできます。

### <a name="creating-dinnerscontroller-unit-tests"></a>DinnersController の単体テストの作成

DinnersController の機能を確認するいくつかの単体テストを今すぐ作成しましょう。 テスト プロジェクト内の「コント ローラー」フォルダーを右クリックして開始し、いますが、**追加 -&gt;新しいテスト**メニュー コマンド。 「単体テスト」を作成し、"DinnersControllerTest.cs"という名前を付けます。

DinnersController Details() アクション メソッドを検証する 2 つのテスト メソッドを作成します。 1 つ目は、既存の Dinner が要求されたときに、ビューが返されることを確認します。 2 つ目は、"NotFound"のビューが存在しない Dinner が要求されたときに返されることを確認します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample3.cs)]

上記のコードはクリーンにコンパイルします。 テストを実行すると、どちらも失敗します。

![](enable-automated-unit-testing/_static/image6.png)

エラー メッセージを見る場合、テストが失敗した理由が、DinnersRepository クラスがデータベースに接続できなかったためにのことを見ていきます。 NerdDinner アプリケーションが SQL Server Express のローカル ファイル、\App の下に存在する接続文字列を使用して\_NerdDinner アプリケーション プロジェクトのデータ ディレクトリ。 NerdDinner.Tests プロジェクトがコンパイルし、アプリケーション プロジェクトの別のディレクトリで実行されるため、接続文字列の相対パスの場所が正しくありません。

私たち*でした*テスト プロジェクトに、SQL Express のデータベース ファイルをコピーすることによってこの問題を解決し、テスト プロジェクトの app.config を適切なテスト接続文字列を追加します。 これは、ブロック解除され、実行されている、上記のテストを取得します。

実際のデータベースを使用してコードの単体テストが伴います。 さまざまな課題です。 具体的には、次のように使用します。

- 単体テストの実行時間を大幅に遅くなります。 頻繁に実行している可能性は低くなりますテストを実行する時間がかかります。 秒 – で実行されで行うものとして、プロジェクトのコンパイルと自然にすることがあることができるに単体テストをすることが理想的です。
- テスト内でセットアップおよび後処理用のロジックが複雑になります。 (なし、副作用または依存関係) での他のユーザーから独立して分離するには、各単体テストを必要とします。 実際のデータベースに対して作業を行うときに状態に注意してくださいとテストの間のリセットが必要です。

これらの問題を回避し、テストで実際のデータベースを使用する必要性を回避に役立つ「注入」と呼ばれるデザイン パターンを見てみましょう。

### <a name="dependency-injection"></a>依存関係の挿入

今すぐ DinnersController は「に密結合」ときは必ず DinnerRepository クラス。 「結合」は、クラスを明示的にクラスに依存別作業するために状態を表します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample4.cs)]

ときは必ず DinnerRepository クラスには、データベースへのアクセスが必要であるため、密に結合された依存関係 DinnersController クラスに DinnersController のアクション メソッドをテストするためにデータベースを作成するを必要とするときは必ず DinnerRepository が終了します。

この問題を回避"依存関係の挿入"– これは、アプローチ (データ アクセスを提供するためのリポジトリ クラス) などの依存関係がそれらを使用するクラス内で不要になった暗黙的に作成される場所と呼ばれるデザイン パターンを採用して取得できます。 代わりに、依存関係に明示的に渡せるそれらを使用するクラスのコンス トラクターの引数を使用します。 インターフェイスを使用して、依存関係を定義する場合し、単体テストのシナリオの「偽」依存関係の実装で渡す柔軟性があります。 これにより、データベースへのアクセスが実際に必要としないテスト固有の依存関係の実装を作成することができます。

これを実際に見る、DinnersController の依存関係の注入を実装してみましょう。

#### <a name="extracting-an-idinnerrepository-interface"></a>IDinnerRepository インターフェイスの抽出

取得および更新 Dinners、コント ローラーが必要とするリポジトリ コントラクトをカプセル化する新しい IDinnerRepository インターフェイスを作成する、最初のステップになります。

このインターフェイスのコントラクトを \Models フォルダーを右クリックし、選択し、手動で定義できます、**追加 -&gt;新しい項目の**メニュー コマンドと IDinnerRepository.cs という名前の新しいインターフェイスを作成します。

また、リファクタリング ツールの Visual Studio Professional に組み込まれて (および上位エディション) を自動的に抽出を使用して、、既存のときは必ず DinnerRepository クラスからのインターフェイスを作成できます。 VS を使用したこのインターフェイスに抽出するためだけときは必ず DinnerRepository クラスで、テキスト エディターのカーソルを配置し、右クリックして選択、**リファクター -&gt;インターフェイスの抽出**メニュー コマンド。

![](enable-automated-unit-testing/_static/image7.png)

"インターフェイスの抽出 ダイアログ ボックスを表示され、私たちを作成するインターフェイスの名前が要求されます。 IDinnerRepository を既定のされ、既存のときは必ず DinnerRepository クラス、インターフェイスに追加するすべてのパブリック メソッドを自動的に選択します。

![](enable-automated-unit-testing/_static/image8.png)

[Ok] ボタンをクリックして、ときに Visual Studio は、アプリケーションに新しい IDinnerRepository インターフェイスを追加します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample5.cs)]

既存のときは必ず DinnerRepository クラスは、インターフェイスを実装できるように更新されます。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample6.cs)]

#### <a name="updating-dinnerscontroller-to-support-constructor-injection"></a>コンス トラクターの挿入をサポートするために DinnersController の更新

DinnersController クラスの新しいインターフェイスを使用して今すぐ更新します。

現在 DinnersController はハード コーディングされた"ときは必ず dinnerRepository"フィールドがときは必ず DinnerRepository クラスでは常に。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample7.cs)]

"ときは必ず dinnerRepository"フィールドが型ときは必ず DinnerRepository の代わりに IDinnerRepository ようにに変更します。 2 つの DinnersController コンス トラクターを公開し、追加します。 コンス トラクターの 1 つには、引数として渡される、IDinnerRepository が使用できます。 もう 1 つは、既存のときは必ず DinnerRepository 実装を使用する既定のコンス トラクターです。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample8.cs)]

既定では、ASP.NET MVC では、既定のコンス トラクターを使用して、コント ローラー クラスを作成するため、実行時に、DinnersController はデータ アクセスを実行するときは必ず DinnerRepository クラスを使用して続けます。

渡すパラメーターをコンス トラクターを使用して、「偽」dinner リポジトリの実装、しかし、単体テストを更新できます。 この「偽」dinner リポジトリでは、実際のデータベースへのアクセスは必要ありませんし、代わりに、インメモリのサンプル データを使用します。

#### <a name="creating-the-fakedinnerrepository-class"></a>FakeDinnerRepository クラスを作成します。

FakeDinnerRepository クラスを作成してみましょう。

NerdDinner.Tests プロジェクト内で「フェイク」ディレクトリを作成して開始されを新しい FakeDinnerRepository クラスを追加し、(フォルダーを右クリックし、選択**追加 -&gt;クラスの新しい**)。

![](enable-automated-unit-testing/_static/image9.png)

FakeDinnerRepository クラスが IDinnerRepository インターフェイスを実装するため、コードを更新します。 右クリックし、「実装インターフェイス IDinnerRepository」コンテキスト メニュー コマンドを使用すると、私たちできます。

![](enable-automated-unit-testing/_static/image10.png)

これは、結果、Visual Studio で自動的に既定の「消去」の実装で、FakeDinnerRepository クラスすべて、IDinnerRepository インターフェイスのメンバーの追加。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample9.cs)]

メモリ内のリストを使用する FakeDinnerRepository 実装を更新し&lt;Dinner&gt;をコンス トラクター引数として渡されたコレクション。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample10.cs)]

データベースは必要ありません、Dinner オブジェクトのメモリ内のリストを使用できます代わりに IDinnerRepository 実装があるようになりました。

#### <a name="using-the-fakedinnerrepository-with-unit-tests"></a>単体テストでの FakeDinnerRepository の使用

データベースを使用できなかったために、以前に失敗した DinnersController の単体テストに戻ってみましょう。 サンプル メモリ内 Dinner データは、次のコードを使用した DinnersController を読み込む FakeDinnerRepository を使用するテスト メソッドを更新することができます。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample11.cs)]

これらのテストを実行すると、両方を渡すようになりました。

![](enable-automated-unit-testing/_static/image11.png)

何よりすばらしいは、実行するには 2 つ目の一部だけと任意の複雑なセットアップ/クリーンアップ ロジックは必要ありません。 ここでは、実際のデータベースに接続することを必要とせずに単体テストすべて、DinnersController アクション メソッド コード (インクルード一覧については、ページング、詳細は、作成、更新、削除) のようになりましたことができます。

| **依存関係挿入フレームワークの側のトピック:** |
| --- |
| 正常に機能しますが、依存関係の数として維持が困難になりますが (上記います) などの手動による依存関係の挿入を実行して、アプリケーションのコンポーネントが増加します。 さらに多くの依存関係管理の柔軟性を提供するのに役立つ .NET のいくつかの依存関係挿入フレームワークが存在します。 これらのフレームワークは、「制御の反転」(IoC) コンテナーとも呼ばを指定して、(最もよく使用コンス トラクターの挿入時のオブジェクトへの依存関係を渡すことの構成サポートの追加レベルを有効にするメカニズムを提供します。). いくつかの人気の OSS 依存関係の挿入]、[.NET の IOC フレームワークが含まれる: AutoFac、Ninject、Spring.NET、StructureMap、Windsor します。 ASP.NET MVC の公開機能拡張 Api 開発者は、解像度と、コント ローラーのインスタンス化に参加できるようにして、依存関係の挿入を有効/IoC フレームワークは、このプロセス内で正常に統合します。 DI/IOC フレームワークを使用してから、DinnersController は –、DinnerRepositorys との間の結合を完全に削除すると、既定のコンス トラクターを削除することできます。 依存関係の挿入して/NerdDinner アプリケーションで IOC フレームワーク。 NerdDinner コード ベースと機能が拡張された場合、今後のこともできます何か。 |

### <a name="creating-edit-action-unit-tests"></a>編集アクションの単体テストの作成

DinnersController の編集機能を確認するいくつかの単体テストを今すぐ作成しましょう。 まず、テスト、編集の操作の HTTP GET バージョン。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample12.cs)]

有効な dinner が要求されたときに、DinnerFormViewModel オブジェクトによってバックアップされたビューが表示されることを検証するテストを作成します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample13.cs)]

テストを実行すると、分かります Edit メソッド Dinner.IsHostedBy() チェックを実行する User.Identity.Name プロパティにアクセスするときに null 参照例外がスローされたために失敗しました。

コント ローラーの基本クラスのユーザー オブジェクトでは、ログイン ユーザーの詳細をカプセル化し、実行時に、コント ローラーを作成するときに、ASP.NET MVC によって設定されます。 ユーザー オブジェクトが設定されていないため、DinnersController の web サーバー環境の外部では、テスト、(そのため、null 参照例外)。

### <a name="mocking-the-useridentityname-property"></a>モック User.Identity.Name プロパティ

モック作成フレームワークでは、テストが簡単に有効にすると、テストをサポートする依存オブジェクトの偽のバージョンを動的に作成することを確認します。 たとえば、シミュレートされたユーザー名の参照に使用できる、DinnersController ユーザー オブジェクトを動的に作成するのに、編集操作のテストでモック作成フレームワークを使用することができます。 これにより、このテストを実行するとスローされるから null 参照が回避されます。

モック フレームワーク ASP.NET MVC で使用できる多くの .NET がある (ここでその一覧を確認できます: [ http://www.mockframeworks.com/ ](http://www.mockframeworks.com/))。 モック作成フレームワーク"Moq"と呼ばれるオープン ソース、使用、NerdDinner アプリケーションをテストするには、ダウンロードできる無料から[ http://www.mockframeworks.com/moq](http://www.mockframeworks.com/moq)します。

ダウンロードされると、Moq.dll アセンブリへの参照を NerdDinner.Tests プロジェクトに追加します。

![](enable-automated-unit-testing/_static/image12.png)

"CreateDinnersControllerAs(username)"ヘルパー メソッドと、パラメーターとしてユーザー名を受け取りし、「モック」DinnersController のインスタンスで User.Identity.Name プロパティをテスト クラスに追加します。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample14.cs)]

上記は Moq 使用 (これは ASP.NET MVC がユーザー、要求、応答、およびセッションのようなランタイム オブジェクトを公開するのにコント ローラー クラスに渡します) ControllerContext オブジェクトの fakes モック オブジェクトを作成します。 ControllerContext HttpContext.User.Identity.Name プロパティが、ヘルパー メソッドに渡されたユーザー名文字列を返すようにするモックで"SetupGet"メソッドの呼び出しいます。

ControllerContext プロパティとメソッドの任意の数をモックすることができます。 これを説明するためにも SetupGet() Request.IsAuthenticated プロパティ (– 以下のテストを実際に必要なはありませんが、要求のプロパティを模擬表示する方法を説明することができます) の呼び出しを追加しました。 完了したときに、ヘルパー メソッドを返します DinnersController ControllerContext モックのインスタンスを割り当てます。

このヘルパー メソッドを使用して、別のユーザーに関係する編集シナリオをテストする単体テストを記述できますようになりました。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample15.cs)]

テストを実行すると合格するようになりました。

![](enable-automated-unit-testing/_static/image13.png)

### <a name="testing-updatemodel-scenarios"></a>UpdateModel() シナリオのテスト

編集の操作の HTTP GET バージョンをカバーしているテストを作成しました。 編集の操作の HTTP POST のバージョンを確認するいくつかのテストを今すぐ作成しましょう。

[!code-csharp[Main](enable-automated-unit-testing/samples/sample16.cs)]

このアクション メソッドをサポートするために私たちにとって興味深い新しいテスト シナリオでは、コント ローラーの基本クラスで UpdateModel() ヘルパー メソッドの使用です。 このヘルパー メソッドは、Dinner オブジェクト インスタンスにフォーム ポスト値のバインドを使用していますが。

フォーム ポストされた UpdateModel() ヘルパー メソッドを使用する値の指定を示す 2 つのテストを以下に示します。 これは、作成して、FormCollection オブジェクトの作成がされ、コント ローラーの"ValueProvider"プロパティに割り当てます。

最初のテストを正常に保存、ブラウザーがリダイレクトされるは、詳細のアクションを確認します。 2 番目のテストは、無効な入力が投稿されたときにエラー メッセージでは、編集ビューをもう一度再アクションに表示されることを確認します。


[!code-csharp[Main](enable-automated-unit-testing/samples/sample17.cs)]

### <a name="testing-wrap-up"></a>テストのまとめ

単体テスト コント ローラー クラスに関連する主要な概念を取り上げました。 これらの手法を使用して、何百ものアプリケーションの動作を検証するための簡単なテストを簡単に作成することができます。

コント ローラーとモデルのテストでは、実際のデータベースは必要ありません、ため非常に高速で簡単に実行されます。 秒単位で何百もの自動テストを実行し、すぐに行った変更が何かを解約するかどうかに関するフィードバックを取得することが予定です。 これにより、継続的に向上させるため、リファクタリング、およびアプリケーションを調整できるという確信を提供できます。

テストについて説明しました – この章の最後のトピックとして何かがテストためではなく、開発プロセスの最後に行う必要がありますが、! 反対に、開発プロセスでできるだけ早く自動テストを記述する必要があります。 そのため、これを使用すると、きれいにすぐにフィードバックを熟考のうえ、アプリケーションのユース ケース シナリオを考えてし、使用してアプリケーションを設計する方法を紹介する、ことができますも開発するときを重ねると、結合に注意してくださいできます。

この書籍で以降の章は、テスト駆動開発 (TDD) と ASP.NET MVC で使用する方法について説明します。 TDD は、反復的なコーディング、テストされるため、結果として得られるコードを記述する場所。 TDD を実装しようとしている機能を検証するテストを作成して各機能を開始します。 明確に理解して、機能と動作するはずですが方法はまず、テスト、ユニットを作成します。 テストが記述された (および失敗したことを確認した) 後にのみを実行しを検証する実際の機能を実装します。 既に、この機能を活用する方法のユース ケースについて考える時間を費やしてきました、ため、要件の理解を深める必要が、それらを実装する最適な方法です。 – テストを再実行され、迅速なフィードバックとしてを得られる実装が済んだらかどうか、機能が正しく動作します。 TDD より 10 章で説明します。

### <a name="next-step"></a>次の手順

いくつかの最終的なコメントをラップします。

> [!div class="step-by-step"]
> [前へ](use-ajax-to-implement-mapping-scenarios.md)
> [次へ](nerddinner-wrap-up.md)
