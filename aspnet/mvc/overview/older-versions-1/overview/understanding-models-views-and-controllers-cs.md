---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
title: 理解のモデル、ビュー、およびコント ローラー (c#) |Microsoft Docs
author: StephenWalther
description: モデル、ビュー、およびコント ローラーと混同するでしょうか。 このチュートリアルで Stephen Walther がわかる ASP.NET MVC アプリケーションのさまざまな部分。
ms.author: riande
ms.date: 08/19/2008
ms.assetid: 87313792-0a96-4caf-89fc-1457d54e5c1e
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e9186a6c261266de8f1a1509a49b84b359bd920
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827153"
---
<a name="understanding-models-views-and-controllers-c"></a>理解のモデル、ビュー、およびコント ローラー (c#)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> モデル、ビュー、およびコント ローラーと混同するでしょうか。 このチュートリアルで Stephen Walther がわかる ASP.NET MVC アプリケーションのさまざまな部分。


このチュートリアルでは ASP.NET MVC の概要をモデル、ビュー、およびコント ローラー。 つまり、M を説明 '、V'、および C' ASP.NET MVC でします。

このチュートリアルを読むには、ASP.NET MVC アプリケーションのさまざまな部分がどのように連携を理解する必要があります。 ASP.NET Web フォーム アプリケーションまたは Active Server Pages アプリケーションの相違点、ASP.NET MVC アプリケーションのアーキテクチャを理解する必要があります。

## <a name="the-sample-aspnet-mvc-application"></a>サンプルの ASP.NET MVC アプリケーション

ASP.NET MVC Web アプリケーションを作成するための既定の Visual Studio テンプレートには、ASP.NET MVC アプリケーションのさまざまな部分を理解するのに使用できる非常に簡単なサンプル アプリケーションが含まれています。 私たちは、このチュートリアルではこのシンプルなアプリケーションの利用します。

Visual Studio 2008 を起動することで、MVC テンプレートを使用して、新しい ASP.NET MVC アプリケーションを作成して、新規プロジェクト ファイル メニュー オプションを選択すると、(図 1 参照)。 新しいプロジェクト] ダイアログで [プロジェクトの種類 (Visual Basic または c#)、好みのプログラミング言語を選択し、[ **ASP.NET MVC Web アプリケーション**テンプレート] の下。 [Ok] ボタンをクリックします。


[![新しいプロジェクト ダイアログ ボックス](understanding-models-views-and-controllers-cs/_static/image1.jpg)](understanding-models-views-and-controllers-cs/_static/image1.png)

**図 01**: 新しいプロジェクト ダイアログ ボックス ([フルサイズの画像を表示する をクリックします](understanding-models-views-and-controllers-cs/_static/image2.png))。


新しい ASP.NET MVC アプリケーションを作成するときに、**単体テスト プロジェクトの作成**ダイアログでは、(図 2 参照) が表示されます。 このダイアログ ボックスでは、ASP.NET MVC アプリケーションのテスト用のソリューションで別のプロジェクトを作成することができます。 オプションを選択**単体テスト プロジェクトを作成できません** をクリックし、 **OK**ボタン。


[![単体テスト ダイアログ ボックスを作成します。](understanding-models-views-and-controllers-cs/_static/image2.jpg)](understanding-models-views-and-controllers-cs/_static/image3.png)

**図 02**: 単体テストの作成 ダイアログ ボックス ([フルサイズの画像を表示する をクリックします](understanding-models-views-and-controllers-cs/_static/image4.png))。


新しい ASP.NET MVC アプリケーションが作成されます。 複数のフォルダーと、ソリューション エクスプ ローラー ウィンドウ内のファイルが表示されます。 具体的には、モデル、ビュー、およびコント ローラーという 3 つのフォルダーが表示されます。 フォルダー名から推察のとおり、これらのフォルダーには、モデル、ビュー、およびコント ローラーを実装するためのファイルが含まれます。

Controllers フォルダーを展開する場合、AccountController.cs という名前のファイルと HomeController.cs をという名前のファイルが表示されます。 Views フォルダーを展開する場合は、アカウント ホーム共有という 3 つのサブフォルダーが表示されます。 ホーム フォルダーを展開する場合は、About.aspx と Index.aspx (図 3 参照) という 2 つの追加ファイルが表示されます。 これらのファイルは、既定の ASP.NET MVC テンプレートに含まれているサンプル アプリケーションを構成します。


[![ソリューション エクスプ ローラー ウィンドウ](understanding-models-views-and-controllers-cs/_static/image3.jpg)](understanding-models-views-and-controllers-cs/_static/image5.png)

**図 03**: [ソリューション エクスプ ローラー] ウィンドウ ([フルサイズの画像を表示する をクリックします](understanding-models-views-and-controllers-cs/_static/image6.png))。


サンプル アプリケーションを実行するには、メニュー オプションを選択して**デバッグ、デバッグの開始**します。 または、F5 キーを押してことができます。

ASP.NET アプリケーションを初めて実行すると、デバッグ モードを有効にすることをお勧めの図 4 ダイアログが表示されます。 [Ok] ボタンをクリックし、アプリケーションを実行します。


[![デバッグの有効になっていません ダイアログ](understanding-models-views-and-controllers-cs/_static/image4.jpg)](understanding-models-views-and-controllers-cs/_static/image7.png)

**図 04**: ダイアログのデバッグが無効 ([フルサイズの画像を表示する をクリックします](understanding-models-views-and-controllers-cs/_static/image8.png))。


ASP.NET MVC アプリケーションを実行すると、Visual Studio は、web ブラウザーでアプリケーションを起動します。 サンプル アプリケーションは、2 つだけのページで構成されます。 インデックス ページと [About] ページ。 まず、アプリケーションの起動時に、インデックス ページ (図 5 参照) が表示されます。 上部のメニューのリンクをクリックして、About ページに移動することができます、アプリケーションの右。


[![インデックス ページ](understanding-models-views-and-controllers-cs/_static/image10.png)](understanding-models-views-and-controllers-cs/_static/image9.png)

**図 05**:、インデックス ページ ([フルサイズの画像を表示する をクリックします](understanding-models-views-and-controllers-cs/_static/image11.png))。


お使いのブラウザーのアドレス バーに Url に注意してください。 たとえば、バージョン情報 メニューのリンクをクリックするとブラウザーのアドレス バーで URL に変更 **/ホーム/について**します。

ブラウザー ウィンドウを閉じるし、Visual Studio に戻り場合、パス ホーム/約ファイルを検索することはできません。 ファイルは存在しません。 設定できないのでしょうか。

## <a name="a-url-does-not-equal-a-page"></a>URL がページと一致しません

従来の ASP.NET Web フォーム アプリケーションまたは Active Server Pages アプリケーションをビルドするときに、URL とページ間の一対一の対応。 サーバーから SomePage.aspx をという名前のページを要求する場合がよりページにあります SomePage.aspx という名前のディスク。 SomePage.aspx ファイルが存在しない場合、これらが表示**404 - ページが見つかりません**エラー。

ASP.NET MVC アプリケーションを構築するときにこれに対しはありません、ブラウザーのアドレス バーに入力した URL と、アプリケーションで表示されているファイルの間の通信。 ASP.NET MVC アプリケーションでは、URL は、ディスク上のページではなく、コント ローラー アクションに対応します。

従来の ASP.NET または ASP アプリケーションでは、ブラウザーの要求がページにマップされます。 これに対し、ASP.NET MVC アプリケーションではブラウザーの要求がコント ローラー アクションへマッピングされています。 ASP.NET Web フォーム アプリケーションは、コンテンツを中心としました。 ASP.NET MVC アプリケーションは、中心のアプリケーション ロジックをこれに対しです。

## <a name="understanding-aspnet-routing"></a>ASP.NET ルーティングを理解します。

ブラウザーの要求と呼ばれる ASP.NET フレームワークの機能を介してコント ローラー アクションにマップ*ASP.NET ルーティング*します。 ASP.NET ルーティングがする ASP.NET MVC フレームワークによって使用される*ルート*コント ローラー アクションへの着信要求。

ASP.NET ルーティングでは、ルート テーブルを使用して、着信要求を処理します。 Web アプリケーションの起動時にこのルート テーブルが作成されます。 ルート テーブルは、Global.asax ファイルで設定します。 既定の MVC の Global.asax ファイルは、リスト 1 に含まれます。

**1 - Global.asax を一覧表示します。**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample1.cs)]

ASP.NET アプリケーションを最初の起動時、アプリケーション\_Start() メソッドが呼び出されます。 リストの 1 では、このメソッドは、RegisterRoutes() メソッドを呼び出し、RegisterRoutes() メソッドが既定のルーティング テーブルを作成します。

既定のルーティング テーブルは、1 つのルートで構成されます。 この既定のルートでは、すべての着信要求を (URL セグメントは、スラッシュの間を) 次の 3 つのセグメントに分割します。 最初のセグメントは、コント ローラー名にマップされて、2 番目のセグメントは、アクション名にマップされて、および最終セグメントがという名前の id。 アクションに渡されるパラメーターにマップされます。

たとえば、次のような URL があるとします。

製品/詳細/3

このような 3 つのパラメーターには、この URL が解析されます。

コント ローラー製品を =

アクションの詳細を =

id = 3

Global.asax ファイルで定義されている既定のルートには、次の 3 つすべてのパラメーターの既定値が含まれています。 既定のコント ローラーはホーム、既定のアクションは、インデックス、および既定の Id が空の文字列。 これらの既定値に注意してくださいには、次の URL を解析する方法を検討してください。

あたり従業員

このような 3 つのパラメーターには、この URL が解析されます。

コント ローラーの従業員を =

アクション インデックスを =

Id =

最後に、任意の URL を指定せずに、ASP.NET MVC アプリケーションを開くかどうか (たとえば、 `http://localhost`) このような URL が解析結果。

コント ローラー = ホーム

アクション インデックスを =

Id =

要求は、HomeController クラスに Index() アクションにルーティングされます。

## <a name="understanding-controllers"></a>コント ローラーの説明

コント ローラーは、MVC アプリケーションでユーザーと対話する方法を制御します。 コント ローラーには、ASP.NET MVC アプリケーションのフロー制御ロジックが含まれています。 コント ローラーは、ユーザーがブラウザーの要求を行うと、ユーザーに送信するには、どのような応答を決定します。

コント ローラーは、クラス (たとえば、Visual Basic または c# クラス) だけです。 サンプル ASP.NET MVC アプリケーションには、コント ローラーがコント ローラーのフォルダーにある HomeController.cs をという名前が含まれています。 HomeController.cs ファイルの内容は、リスト 2 に継承されます。

**2 - HomeController.cs を一覧表示します。**

[!code-csharp[Main](understanding-models-views-and-controllers-cs/samples/sample2.cs)]

HomeController が Index() と About() という 2 つのメソッドを持つことに注意してください。 これら 2 つのメソッドは、コント ローラーによって公開されている 2 つのアクションに対応します。 URL/Home/インデックス HomeController.Index() メソッドを呼び出すし、URL/Home/について HomeController.About() メソッドを呼び出します。

コント ローラーのすべてのパブリック メソッドは、コント ローラーのアクションとして公開されます。 これについて注意する必要があります。 これは、ブラウザーに適切な URL を入力して、インターネットへのアクセスを持つユーザーで、コント ローラーに含まれているすべてのパブリック メソッドを起動できることを意味します。

## <a name="understanding-views"></a>ビューについて

Index() と About()、HomeController クラスによって公開される 2 つのコント ローラー アクション ビューはどちらも返します。 ビューには、HTML マークアップと、ブラウザーに送信されるコンテンツが含まれています。 ビューとは、ASP.NET MVC アプリケーションを使用する場合、ページに相当です。

適切な場所で、ビューを作成する必要があります。 HomeController.Index() アクションには、次のパスにあるビューが返されます。

\Views\Home\Index.aspx

HomeController.About() アクションには、次のパスにあるビューが返されます。

\Views\Home\About.aspx

一般に、コント ローラー アクションのビューを返す場合は、しする必要があります、コント ローラーと同じ名前で、Views フォルダーにサブフォルダーを作成します。 サブフォルダー内には、コント ローラー アクションと同じ名前の .aspx ファイルを作成する必要があります。

リスト 3 のファイルには、About.aspx ビューが含まれています。

**3 - About.aspx を一覧表示します。**

[!code-aspx[Main](understanding-models-views-and-controllers-cs/samples/sample3.aspx)]

リスト 3 の最初の行を無視する場合、ビューの残りの部分のほとんどは標準の HTML で構成されます。 ここでする任意の HTML を入力して、ビューの内容を変更できます。

ビューは、Active Server Pages または ASP.NET Web フォーム内のページによく似ています。 ビューは、HTML コンテンツとスクリプトに含めることができます。 プログラミング言語 (たとえば、c# または Visual Basic .NET)、使い慣れた .NET で、スクリプトを記述することができます。 データベースのデータなどの動的なコンテンツを表示するのにには、スクリプトを使用します。

## <a name="understanding-models"></a>理解のモデル

コント ローラーについて説明し、ビューについて説明します。 説明が必要な最後のトピックでは、モデルです。 MVC モデルとは何ですか。

MVC モデルには、ビューまたはコント ローラーに含まれていないこと、アプリケーション ロジックのすべてが含まれます。 モデルには、すべてのアプリケーションのビジネス ロジック、検証ロジックでは、データベース アクセス ロジックを含める必要があります。 たとえば、データベースにアクセスする Microsoft Entity Framework を使用する場合は、Models フォルダーに、Entity Framework クラス (.edmx ファイル) を作成は。

ビューには、ユーザー インターフェイスの生成に関連するロジックのみを含める必要があります。 コント ローラーには、右側のビューを返すか、別のアクション (フロー制御) にユーザーをリダイレクトするために必要なロジックの最低限ののみを含める必要があります。 他のすべては、モデルに含める必要があります。

一般に、fat モデルおよびスキニー コント ローラーよう努力する必要があります。 コント ローラーのメソッドは、数行のコードのみを含める必要があります。 コント ローラーのアクションが fat すぎる場合は、ロジックを行き来する、Models フォルダーに新しいクラスを検討してください。

## <a name="summary"></a>まとめ

このチュートリアルは、web アプリケーションで、ASP.NET MVC のさまざまな部分の高レベルな概要を提供します。 特定のコント ローラー アクションへ受信ブラウザー要求をマップ ASP.NET ルーティング方法を学習しました。 コント ローラーがブラウザーにビューを返す方法を調整する方法を学習しました。 最後に、モデルがアプリケーションの業務、検証、およびデータベース アクセス ロジックを含めることが方法を学習しました。
