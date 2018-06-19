---
uid: mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
title: Understanding モデル、ビュー、およびコント ローラー (VB) |Microsoft ドキュメント
author: StephenWalther
description: モデル、ビュー、およびコント ローラーに関する混乱しますか。 このチュートリアルでは Stephen Walther について説明しています、ASP.NET MVC アプリケーションのさまざまな部分です。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: a106374a-5e74-4fd0-9ac0-1a32280e5d0d
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/overview/understanding-models-views-and-controllers-vb
msc.type: authoredcontent
ms.openlocfilehash: 36f70503f7e96210e5fb8a2d361da6759c18c85b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26500991"
---
<a name="understanding-models-views-and-controllers-vb"></a>Understanding モデル、ビュー、およびコント ローラー (VB)
====================
によって[Stephen Walther](https://github.com/StephenWalther)

> モデル、ビュー、およびコント ローラーに関する混乱しますか。 このチュートリアルでは Stephen Walther について説明しています、ASP.NET MVC アプリケーションのさまざまな部分です。


このチュートリアルは、ASP.NET MVC の概要を示しますモデル、ビュー、およびコント ローラーです。 つまり、M について説明します '、V'、および C' ASP.NET MVC でします。

このチュートリアルを読むと、後に、ASP.NET MVC アプリケーションのさまざまな部分がどのように連携を理解する必要があります。 ASP.NET MVC アプリケーションのアーキテクチャの違い、ASP.NET Web フォーム アプリケーションまたは Active Server Pages アプリケーションも理解する必要があります。

## <a name="the-sample-aspnet-mvc-application"></a>サンプルの ASP.NET MVC アプリケーション

ASP.NET MVC Web アプリケーションを作成するための既定の Visual Studio テンプレートには、ASP.NET MVC アプリケーションのさまざまな部分を理解するために使用する非常に簡単なサンプル アプリケーションが含まれています。 このチュートリアルではこの単純なアプリケーションのことを利用します。

MVC テンプレートを使用して Visual Studio 2008 を起動して、新しい ASP.NET MVC アプリケーションを作成して、新規プロジェクト ファイル メニュー オプションを選択すると、(図 1 を参照してください)。 新しいプロジェクト] ダイアログで [プロジェクトの種類 (Visual Basic または C# の場合)、好みのプログラミング言語を選択し、[ **ASP.NET MVC Web アプリケーション**テンプレート] の下。 [Ok] ボタンをクリックします。


[![新しいプロジェクト ダイアログ ボックス](understanding-models-views-and-controllers-vb/_static/image1.jpg)](understanding-models-views-and-controllers-vb/_static/image1.png)

**図 01**: 新しいプロジェクト ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-vb/_static/image2.png))


新しい ASP.NET MVC アプリケーションを作成するときに、**単体テスト プロジェクトの作成**ダイアログが表示されます (図 2)。 このダイアログ ボックスでは、ASP.NET MVC アプリケーションのテスト用のソリューションで別のプロジェクトを作成することができます。 オプションを選択**単体テスト プロジェクトを作成できません** をクリックし、 **OK**ボタンをクリックします。


[![単体テスト ダイアログ ボックスを作成します。](understanding-models-views-and-controllers-vb/_static/image2.jpg)](understanding-models-views-and-controllers-vb/_static/image3.png)

**図 02**: 単体テストの作成 ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-vb/_static/image4.png))


新しい ASP.NET MVC アプリケーションが作成されます。 いくつかのフォルダーとファイル、ソリューション エクスプ ローラー ウィンドウが表示されます。 具体的には、モデル、ビュー、およびコント ローラーという 3 つのフォルダーが表示されます。 フォルダー名から推測することがあります、これらのフォルダーは、モデル、ビュー、およびコント ローラーを実装するためのファイルを格納します。

Controllers フォルダーを展開する場合は、AccountController.vb をという名前のファイルおよび HomeController.vb をという名前のファイルが表示されます。 Views フォルダーを展開する場合は、アカウント、ホームおよび Shared という名前の 3 つのサブフォルダーが表示されます。 ホーム フォルダーを展開する場合は、About.aspx および Index.aspx (図 3 を参照してください) という 2 つの追加のファイルが表示されます。 これらのファイルは、既定の ASP.NET MVC テンプレートに含まれているサンプル アプリケーションを構成します。


[![ソリューション エクスプ ローラー ウィンドウ](understanding-models-views-and-controllers-vb/_static/image3.jpg)](understanding-models-views-and-controllers-vb/_static/image5.png)

**図 03**:、ソリューション エクスプ ローラー ウィンドウ ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-vb/_static/image6.png))


メニュー オプションを選択して、サンプル アプリケーションを実行することができます**デバッグ、デバッグの開始**です。 または、F5 キーを押してことができます。

最初に、ASP.NET アプリケーションを実行すると、デバッグ モードを有効にすることをお勧めの図 4 では、ダイアログが表示されます。 [Ok] ボタンをクリックし、そのアプリケーションを実行します。


[![デバッグの有効になっていません ダイアログ](understanding-models-views-and-controllers-vb/_static/image4.jpg)](understanding-models-views-and-controllers-vb/_static/image7.png)

**図 04**: ダイアログのデバッグが無効 ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-vb/_static/image8.png))


ASP.NET MVC アプリケーションを実行すると、Visual Studio は、web ブラウザーでアプリケーションを起動します。 サンプル アプリケーションは、2 つのページで構成されます: インデックス ページと [バージョン情報] ページ。 アプリケーションの起動時、(図 5 を参照してください)、インデックス ページが表示されます。 上部のメニューのリンクをクリックして、[バージョン情報] ページに移動することができます、アプリケーションの権限です。


[![[インデックス] ページ](understanding-models-views-and-controllers-vb/_static/image5.jpg)](understanding-models-views-and-controllers-vb/_static/image9.png)

**図 05**:、インデックス ページ ([フルサイズのイメージを表示するをクリックして](understanding-models-views-and-controllers-vb/_static/image10.png))


お使いのブラウザーのアドレス バーに Url に注意してください。 たとえば、バージョン情報] メニューの [リンクをクリックするとブラウザーのアドレス バーに URL を変更**ホーム//について**です。

ブラウザー ウィンドウを閉じるし、Visual Studio に戻り、パス ホーム約/ファイルを検索することはできません。 ファイルが存在しません。 次のとおりですか。

## <a name="a-url-does-not-equal-a-page"></a>URL がページに等しくないです。

従来の ASP.NET Web フォーム アプリケーションまたは Active Server Pages アプリケーションをビルドするときに、URL とページ間の一対一の対応。 サーバーから SomePage.aspx をという名前のページを要求する場合がよりありますページ SomePage.aspx をという名前のディスク上。 見た目を取得する SomePage.aspx ファイルが存在しない場合**ページが見つかりません - 404**エラーです。

ASP.NET MVC アプリケーションを構築するときにこれに対しはありません、ブラウザーのアドレス バーに入力した URL と、アプリケーション内にあるファイルの対応。 ASP.NET MVC アプリケーションでは、URL は、ディスク上のページではなくコント ローラーのアクションに対応します。

従来の ASP.NET または ASP アプリケーションでは、ブラウザーの要求がページにマップされます。 ASP.NET MVC アプリケーションでこれに対し、ブラウザーの要求にマップされますコント ローラーのアクション。 ASP.NET Web フォーム アプリケーションは、コンテンツを中心としました。 これに対し、ASP.NET MVC アプリケーションは、中心のアプリケーション ロジックです。

## <a name="understanding-aspnet-routing"></a>ASP.NET ルーティング

ブラウザー要求を取得という ASP.NET フレームワークの機能を通じて、コント ローラー アクションにマップされている*ASP.NET ルーティング*です。 ASP.NET ルーティングが、ASP.NET MVC フレームワークによって使用される*ルート*コント ローラー アクションへの着信要求します。

ASP.NET ルーティングでは、ルート テーブルを使用して、受信要求を処理します。 Web アプリケーションが初めて起動したときに、このルート テーブルが作成されます。 ルート テーブルは、Global.asax ファイルで設定します。 既定の MVC Global.asax ファイルが 1 のリストに含まれます。

**1 - Global.asax を一覧表示します。**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample1.vb)]

ASP.NET アプリケーション最初の開始時、アプリケーション\_Start() メソッドが呼び出されます。 リストの 1 では、このメソッドは、RegisterRoutes() し RegisterRoutes() メソッドは、既定のルート テーブルを作成します。

既定のルート テーブルは、1 つのルートで構成されます。 この既定のルートでは、すべての着信要求を 3 つのセグメント (URL セグメントとは何もスラッシュの間) に分割します。 最初のセグメントは、コント ローラー名にマップされて、2 番目のセグメントは、アクション名にマップ、および最後のセグメントが id。 をという名前のアクションに渡されるパラメーターにマップされています。

たとえば、次の URL を考慮してください。

/製品/詳細/3

次のように 3 つのパラメーターには、この URL が解析されます。

コント ローラー製品を =

アクションの詳細を =

Id = 3

Global.asax ファイルで定義されている既定のルートには、次の 3 つのすべてのパラメーターの既定値が含まれています。 既定値コント ローラーはホーム、既定のアクションでは、インデックス、および既定の Id は空の文字列。 これらの既定値を念頭には、次の URL を解析する方法について考えてみます。

/従業員

次のように 3 つのパラメーターには、この URL が解析されます。

コント ローラーの従業員を =

アクション インデックスを =

Id =

最後に、任意の URL を指定せずに ASP.NET MVC アプリケーションを開くかどうか (たとえば、 `http://localhost`) 次のように URL が解析結果。

コント ローラー ホーム =

アクション インデックスを =

Id =

HomeController クラスに対する Index() アクションに、要求がルーティングされます。

## <a name="understanding-controllers"></a>コント ローラーの説明

コント ローラーは、ユーザーが、MVC アプリケーションと対話する方法を制御します。 コント ローラーには、ASP.NET MVC アプリケーションのフロー制御ロジックが含まれています。 コント ローラーでは、ユーザーがブラウザーの要求を行うときに、ユーザーに返送するには、どのような応答を決定します。

コント ローラーは、クラス (たとえば、Visual Basic または c# クラス) だけです。 ASP.NET MVC アプリケーションの例には、コント ローラーのフォルダーにある HomeController.vb をという名前のコント ローラーが含まれています。 リスト 2 で HomeController.vb ファイルの内容を再現します。

**2 - HomeController.cs を一覧表示します。**

[!code-vb[Main](understanding-models-views-and-controllers-vb/samples/sample2.vb)]

HomeController が Index() および About() という名前の 2 つのメソッドを持つことに注意してください。 これら 2 つのメソッドは、コント ローラーによって公開されている 2 つのアクションに対応します。 URL/Home/インデックス、HomeController.Index() メソッドを呼び出しに関する URL/Home/HomeController.About() メソッドを呼び出します。

コント ローラー内のすべてのパブリック メソッドは、コント ローラーのアクションとして公開されます。 これについて注意する必要があります。 これは、ブラウザーに正しい URL を入力して、インターネットにアクセス権を持つすべてのユーザーが、コント ローラーに含まれているすべてのパブリック メソッドを起動できることを意味します。

## <a name="understanding-views"></a>ビューについて

HomeController クラス、Index() と About()、によって公開されている 2 つのコント ローラーのアクション、ビューはどちらも返します。 ビューには、HTML マークアップと、ブラウザーに送信されるコンテンツが含まれています。 ビューは、ASP.NET MVC アプリケーションを使用する場合、ページの相当します。

適切な場所で、ビューを作成する必要があります。 HomeController.Index() アクションには、次のパスにあるビューが返されます。

\Views\Home\Index.aspx

HomeController.About() アクションには、次のパスにあるビューが返されます。

\Views\Home\About.aspx

一般に、コント ローラーのアクションのビューを取得するには、場合、コント ローラーと同じ名前のビュー フォルダーにサブフォルダーを作成します。 サブフォルダー内には、コント ローラーのアクションと同じ名前の .aspx ファイルを作成する必要があります。

3 の一覧で、ファイルには、About.aspx ビューが含まれています。

**3 - About.aspx を一覧表示します。**

[!code-aspx[Main](understanding-models-views-and-controllers-vb/samples/sample3.aspx)]

リスト 3 の最初の行を無視する場合、ビューの残りの部分のほとんどは標準の HTML で構成されます。 ここにする任意の HTML を入力して、ビューの内容を変更できます。

ビューは、Active Server Pages または ASP.NET Web フォームでのページによく似ています。 ビューには、HTML コンテンツおよびスクリプトを含めることができます。 スクリプトは、プログラミング言語 (たとえば、c# または Visual Basic .NET)、使い慣れた .NET で記述できます。 スクリプトを使用して、データベースのデータなどの動的なコンテンツを表示します。

## <a name="understanding-models"></a>Understanding モデル

コント ローラーを説明したおよびビューについて説明してきました。 最後のトピックで説明する必要がありますが、モデルです。 MVC モデルとは何ですか。

MVC モデルには、すべてのビューまたはコント ローラーに含まれていないこと、アプリケーション ロジックが含まれます。 モデルには、すべてのアプリケーションのビジネス ロジック、検証ロジック、およびデータベース アクセス ロジックを含める必要があります。 たとえば、データベースにアクセスする、Microsoft の Entity Framework を使用している場合、Models フォルダーに、Entity Framework のクラス (.edmx ファイル) を作成です。

ビューには、ユーザー インターフェイスの生成に関連するロジックのみを含める必要があります。 コント ローラーには、右側のビューを取得または別のアクション (フロー制御) にユーザーをリダイレクトするために必要なロジックの最低限ののみを含める必要があります。 他のすべては、モデルに含める必要があります。

一般に、fat モデルおよびスキニー コント ローラーに限り必要があります。 コント ローラーのメソッドは、数行のコードのみを含める必要があります。 コント ローラーのアクションが fat すぎるを取得する場合、Models フォルダーに新しいクラスをロジックの移動を検討してください。

## <a name="summary"></a>概要

このチュートリアルは、web アプリケーション、ASP.NET MVC のさまざまな部分の高レベルな概要を提供します。 特定のコント ローラーのアクションをブラウザーの入力方向の要求がどのように ASP.NET ルーティング マップについて学習しました。 コント ローラーがブラウザーにビューを返す方法を調整する方法を学習しました。 最後に、モデルがアプリケーションの業務、検証、およびデータベース アクセス ロジックを含める方法を学習しました。
