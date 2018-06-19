---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
title: カスタム エラー ページ (VB) の表示 |Microsoft ドキュメント
author: rick-anderson
description: ユーザーに表示される内容、ASP.NET web アプリケーションで、実行時エラーが発生したときですか。 方法によって異なります web サイトの&lt;customErrors&gt;構成しています.。
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 14873c5d-81a9-455b-bd71-30fb555583e7
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-vb
msc.type: authoredcontent
ms.openlocfilehash: eda7ceeac174f0d1697cb95d2eab4127f124011e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30889164"
---
<a name="displaying-a-custom-error-page-vb"></a>カスタム エラー ページ (VB) を表示します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_VB.zip)または[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_vb.pdf)

> ユーザーに表示される内容、ASP.NET web アプリケーションで、実行時エラーが発生したときですか。 方法によって異なります。 web サイトの&lt;customErrors&gt;構成します。 既定では、ユーザーのとおり、実行時エラーが発生したことを proclaiming すっきりすることの黄色い画面です。 このチュートリアルでは、サイトのルック アンド フィールに一致するカスタム エラー ページを表示、見た目心地よいこれらの設定をカスタマイズする方法を示します。


## <a name="introduction"></a>はじめに

理想の世界がないことの実行時エラー。 プログラマはコードを記述せずに、バグと信頼性の高いユーザー入力の検証と外部データベース サーバーと電子メール サーバーなどのリソースはオフラインです。 もちろん、実際にはエラー、避けられないものです。 .NET Framework のクラスは、例外をスローしてエラーを通知します。 オブジェクトの Open メソッドはたとえば、SqlConnection を呼び出して接続文字列で指定されたデータベースへの接続を確立します。 ただし、データベースがダウンした場合、または接続文字列で資格情報が有効でない場合、Open メソッドがスローされます、`SqlException`です。 使用して例外を処理することができます`Try/Catch/Finally`ブロックします。 場合内のコード、`Try`ブロックは例外をスロー、開発者が、エラーから回復を試みることができます、適切な catch ブロックに制御が移ります。 照合の catch ブロックがないか、例外が percolates search の呼び出し履歴を try ブロックに例外をスローしたコードがない場合は、`Try/Catch/Finally`ブロックします。

場合は、例外が処理されることがなく、ASP.NET ランタイムまでバブル、 [ `HttpApplication`クラス](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)の[`Error`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)が発生し、構成された*エラー ページ*が表示されます。 既定では、ASP.NET としても参照されているエラー ページを表示、[黄色死亡画面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)(YSOD)。 YSOD の 2 つのバージョン: いずれかを示します例外の詳細、スタック トレース、およびその他の情報を開発者がアプリケーションのデバッグに役立ちます (を参照してください**図 1**) です単に (参照実行時エラーがあったことを示す、他の。**図 2**)。

例外の詳細 YSOD が、アプリケーションをデバッグする開発者にとても役立ちますが、YSOD をエンドユーザーに表示趣味の悪いと不自然します。 代わりに、エンドユーザーよりユーザー フレンドリな通常の文章のような状況を記述すると、サイトのルック アンド フィールを維持するエラー ページに移動する必要があります。 良いニュースは、このようなカスタム エラー ページを作成することは非常に簡単です。 このチュートリアルは、ASP 見てを開始します。NET の別のエラー ページ。 ユーザーにエラーが発生した場合、カスタム エラー ページを表示する web アプリケーションを構成する方法を示します。

### <a name="examining-the-three-types-of-error-pages"></a>次の 3 つの種類のエラー ページを確認します。

未処理の例外は次の 3 つの種類のエラー ページのいずれかの ASP.NET アプリケーションで発生するとが表示されます。

- 例外の詳細死亡黄色画面エラー ページ
- ランタイム エラー死亡黄色画面エラー ページ、または
- カスタム エラー ページ

エラー ページの開発者は、よく知られているとは、例外の詳細 YSOD です。 既定では、このページはローカルにアクセスするユーザーに表示されるためおよび開発環境で、サイトをテストするときにエラーが発生したときに表示されるページ。 その名前が示すように、例外の詳細 YSOD は、種類、メッセージ、およびスタック トレースの例外に関する詳細を提供します。 さらに、ASP.NET ページの分離コード クラスのコードで例外が発生した場合、およびデバッグするため、アプリケーションが構成されている場合、例外の詳細 YSOD 表示されます。 もコード (およびその下に、いくつかの行を上記のコードの) の行。

**図 1**例外詳細 YSOD ページを示しています。 ブラウザーのアドレス ウィンドウで、URL に注意してください:`http://localhost:62275/Genre.aspx?ID=foo`です。 注意してください、`Genre.aspx`ページには、特定のジャンルの書評が一覧表示されます。 いる必要があります`GenreId`値 (、 `uniqueidentifier`) です。 クエリ文字列を通じて渡されるフィクション レビューを表示する適切な URL は、たとえば、`Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`です。 以外`uniqueidentifier`値が渡されます ("foo") など、クエリ文字列を通じて、例外がスローされます。

> [!NOTE]
> デモの web アプリケーションでこのエラーを再現するにはダウンロードすることができますいずれかのアクセス`Genre.aspx?ID=foo`直接の「実行時エラーを生成する」リンクをクリックしてまたは`Default.aspx`です。


例外の情報に注意してください**図 1**です。 例外メッセージ「変換に失敗した文字の文字列から uniqueidentifier に変換するときに」は、ページの上部に存在します。 例外の種類`System.Data.SqlClient.SqlException`、一覧には、同様にします。 スタック トレースもあります。

[![](displaying-a-custom-error-page-vb/_static/image2.png)](displaying-a-custom-error-page-vb/_static/image1.png)

**図 1**: YSOD 例外の詳細には、例外に関する情報が含まれています。  
 ([フルサイズのイメージを表示するをクリックして](displaying-a-custom-error-page-vb/_static/image3.png))

YSOD の他の種類に表示される、ランタイム エラー YSOD**図 2**です。 ランタイム エラー YSOD 通知実行時エラーが発生した、訪問者がスローされた例外に関する情報は含まれません。 (これは、ただし、手順は説明を変更してエラーの詳細を表示できるようにする方法の`Web.config`ファイルをそのため、このような不自然に見える YSOD)。

既定では、ランタイム エラー YSOD がリモート アクセス ユーザーが表示されます (を通じてhttp://www.yoursite.com)ことで、ブラウザーのアドレス バーに URL からわかるように、**図 2**:`http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`です。 2 つの異なる YSOD 画面存在していても開発者は、エラーの詳細を知りたい考えていますが、潜在的なセキュリティの脆弱性や他の機密情報を表示する人が公開されなどの情報を実際のサイトに表示されませんが、サイトです。

> [!NOTE]
> 以下に示します。 し、web ホストとして DiscountASP.NET を使用している場合、ランタイム エラー YSOD は実際のサイトにアクセスしたときに表示されませんがわかります可能性があります。 DiscountASP.NET に既定では例外の詳細 YSOD を表示するように構成、サーバーがあるためにです。 良いニュースは、追加することでこの既定の動作をオーバーライドすることができます、`<customErrors>`セクション、`Web.config`ファイル。 「構成がエラー ページが表示されます」セクションでは検討、`<customErrors>`詳細」セクション。


[![](displaying-a-custom-error-page-vb/_static/image5.png)](displaying-a-custom-error-page-vb/_static/image4.png)

**図 2**: 実行時エラー YSOD では、エラーの詳細が含まれません  
 ([フルサイズのイメージを表示するをクリックして](displaying-a-custom-error-page-vb/_static/image6.png))

3 番目の種類のエラー ページは、作成した web ページであるカスタム エラー ページです。 カスタム エラー ページの利点は、ページのルック アンド フィール; と共にユーザーに表示される情報を完全に制御があることです。カスタム エラー ページは、その他のページとして、同じマスター ページやスタイルを使用できます。 「を使用して、カスタム エラー ページ」セクションは、カスタム エラー ページを作成し、未処理の例外が発生した場合に表示するように構成手順について説明します。 **図 3**このカスタム エラー ページの紹介を提供します。 ご覧のように、エラー ページのルック アンド フィールははるかに本格、黄色の画面の死亡図 1 と 2 のいずれかのよりです。

[![](displaying-a-custom-error-page-vb/_static/image8.png)](displaying-a-custom-error-page-vb/_static/image7.png)

**図 3**: カスタム エラー ページがより細かく調整されたルック アンド フィールを提供しています  
 ([フルサイズのイメージを表示するをクリックして](displaying-a-custom-error-page-vb/_static/image9.png))

すぐに、ブラウザーのアドレス バーの検査を**図 3**です。 アドレス バーには、カスタム エラー ページの URL が表示されます (`/ErrorPages/Oops.aspx`)。 図 1 および 2 で死亡の黄色の画面が、同じページから、エラーが発生したことで表示 (`Genre.aspx`)。 カスタム エラー ページを使用して、エラーが発生したページの URL を渡される、 `aspxerrorpath` querystring パラメーター。

## <a name="configuring-which-error-page-is-displayed"></a>表示されるエラー ページを構成します。

表示される 3 つの可能性のあるエラー ページは、2 つの変数に基づいて決定されます。

- 構成情報、`<customErrors>`セクション、および
- かどうか、ユーザーがアクセスしたサイト ローカルまたはリモートでします。

[ `<customErrors>`セクション](https://msdn.microsoft.com/library/h0hfz6fc.aspx)で`Web.config`はどのようなエラー ページが表示に影響する 2 つの属性を持つ:`defaultRedirect`と`mode`です。 `defaultRedirect` 属性は省略できます。 指定した場合、カスタム エラー ページの URL を指定し、ランタイム エラー YSOD ではなく、カスタム エラー ページを表示するかを示します。 `mode`属性は必須であり、3 つの値のいずれかを受け入れます。 `On`、 `Off`、または`RemoteOnly`です。 これらの値には、次の動作があります。

- `On` -カスタム エラー ページまたはランタイム エラー YSOD がローカルまたはリモートされるかどうかに関係なく、すべてのユーザーに表示されていることを示します。
- `Off` -例外詳細 YSOD がローカルまたはリモートされるかどうかに関係なく、すべてのユーザーに表示されることを指定します。
- `RemoteOnly` -カスタム エラー ページまたはランタイム エラー YSOD 表示されているリモートの訪問者を例外の詳細 YSOD がローカルのユーザーに表示されるときに示します。

ASP.NET 機能モード属性を設定している場合、それ以外の場合、指定しない限り`RemoteOnly`が指定されていないと、`defaultRedirect`値。 つまり、既定の動作は、ランタイム エラー YSOD がリモートの訪問者に表示されるときに例外詳細 YSOD がローカル訪問者に表示されることにします。 追加することでこの既定の動作をオーバーライドすることができます、 `<customErrors>` web アプリケーションのセクション `Web.config file.`

## <a name="using-a-custom-error-page"></a>カスタム エラー ページを使用します。

すべての web アプリケーションは、カスタム エラー ページがある必要があります。 ランタイム エラー YSOD なよりプロフェッショナルな外観の代替が提供、簡単に作成するには、およびカスタム エラー ページを使用するアプリケーションの構成のみに時間がかかります。 最初の手順は、カスタム エラー ページを作成しています。 という名前の書評アプリケーションに新しいフォルダーを追加して`ErrorPages`され、新しい ASP.NET ページという名前に追加`Oops.aspx`です。 同じのルック アンド フィールを自動的に継承するように、サイトのページの残りの部分として同じマスター ページを使用して、ページがあります。

[![](displaying-a-custom-error-page-vb/_static/image11.png)](displaying-a-custom-error-page-vb/_static/image10.png)

**図 4**: カスタム エラー ページを作成します。

次に、数分をかけて、エラー ページのコンテンツを作成します。 単純なカスタム エラー ページ、メッセージ、ことを示す予期しないエラーが発生しました、サイトのホーム ページにリンクがバックアップを作成しました。

[![](displaying-a-custom-error-page-vb/_static/image13.png)](displaying-a-custom-error-page-vb/_static/image12.png)

**図 5**: カスタム エラー ページのデザイン  
 ([フルサイズのイメージを表示するをクリックして](displaying-a-custom-error-page-vb/_static/image14.png))

完了、エラー ページには、代わりに、ランタイム エラー YSOD カスタム エラー ページを使用する web アプリケーションを構成します。 内のエラー ページの URL を指定することによってこれを行う、`<customErrors>`セクションの`defaultRedirect`属性。 次のマークアップを追加するアプリケーションの`Web.config`ファイル。

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample1.xml)]

上記のマークアップでは、例外の詳細 YSOD をローカルにアクセスすると、これらのユーザーのリモート オフィスを訪れた Oops.aspx カスタム エラー ページを使用しているときにユーザーに表示するアプリケーションを構成します。 表示するこのアクションで、web サイトを運用環境に展開し、無効なクエリ文字列値を持つライブ サイトで Genre.aspx ページを参照してください。 カスタム エラー ページが表示されます (を参照**図 3**)。

カスタム エラー ページが、リモート ユーザーにのみに表示されていることを確認するには、次を参照してください。、`Genre.aspx`開発環境からの無効なクエリ文字列を含むページです。 例外の詳細 YSOD はそのまま表示 (を参照**図 1**)。 `RemoteOnly`設定により、ローカルで作業している開発者が、例外の詳細を参照してください。 引き続きした実稼働環境でサイトにアクセスするユーザーはカスタム エラー ページを表示します。

## <a name="notifying-developers-and-logging-error-details"></a>開発者に通知して、エラーの詳細をログ記録

彼女のコンピュータにとどまっている開発者によって、開発環境で発生したエラーが発生しました。 彼女には、例外の詳細 YSOD で例外の情報が表示され、エラーが発生したときに実行していた彼女はどのような手順を知ってです。 実稼働でエラーが発生するときに、開発者に、サイト、エンドユーザーは、エラーを報告する時間がかかりますしない限り、エラーが発生したナレッジがありません。 ユーザー例外の種類、メッセージ、およびスタック トレース、エラーの原因を診断は言うまでも修正するが困難であることができますがわからなくても、エラーが発生した、開発チームのアラートを生成する途中から外れた場合でもです。

(データベースなど) のいくつかの永続的なストアに、実稼働環境でエラーがログに記録する最優先事項であるこれらの理由により、開発者がこのエラーの通知します。 カスタム エラー ページは、このログ記録および通知を実行する適切な場所のように思えるかもしれません。 残念ながら、カスタム エラー ページはエラーの詳細にアクセスできないし、この情報をログに使用することはできません。 良いニュースされているエラーの詳細をインターセプトしに、ログを記録する方法の数が次の 3 つのチュートリアルでは、さらに詳しくは、このトピックの調査。

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>異なる HTTP エラー状態の別のカスタム エラー ページを使用します。

例外は、ASP.NET ページによってスローされ、処理されていない、ときに、例外は最大で、ASP.NET ランタイム、構成済みのエラー ページが表示されます percolates です。 ファイルのアクセス許可を無効になっている要求は ASP.NET エンジンが渡されますが、何らかの理由を処理できません - など、要求されたファイルが見つからなかったか読み取る場合は、ASP.NET エンジンを発生させます、`HttpException`です。 ASP.NET ページなどから発生した例外と同様に、この例外が原因で、適切なエラー ページを表示する、実行時までバブルします。

これにより、実稼働環境で web アプリケーションです。 ユーザーは、カスタム エラー ページが表示されますに含まれていないページを要求します。 **図 6**このような例を示しています。 要求が存在しないページのため (`NoSuchPage.aspx`)、`HttpException`スローされるカスタム エラー ページが表示されます (への参照に注意してください`NoSuchPage.aspx`で、 `aspxerrorpath` querystring パラメーター)。

[![](displaying-a-custom-error-page-vb/_static/image16.png)](displaying-a-custom-error-page-vb/_static/image15.png)**図 6**: ASP.NET ランタイムが無効な要求に対する応答で構成済みのエラー ページを表示  
 ([フルサイズのイメージを表示するをクリックして](displaying-a-custom-error-page-vb/_static/image17.png)) 

既定ですべての種類のエラーにより、同じカスタム エラー ページを表示します。 ただし、特定 HTTP ステータス コードを使用して別のカスタム エラー ページを指定できます`<error>`内の子要素、`<customErrors>`セクションです。 たとえば、ページが見つからないを HTTP ステータス コード 404 を持ち、エラーが発生した場合に表示される別のエラー ページが存在する次のように更新します。、`<customErrors>`セクションに次のマークアップを含めます。

[!code-xml[Main](displaying-a-custom-error-page-vb/samples/sample2.xml)]

この変更へのアクセスでのリモート ユーザーが存在しない、ASP.NET リソースを要求したときにそれらにリダイレクトされます、`404.aspx`の代わりにカスタム エラー ページ`Oops.aspx`です。 として**図 7**を示しています、`404.aspx`ページは、一般的なカスタム エラー ページより具体的なメッセージを含めることができます。

> [!NOTE]
> チェック アウト[404 エラー ページ、1 つ以上の時間](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)効果的な 404 エラー ページを作成する方法のガイダンスについてはします。


[![](displaying-a-custom-error-page-vb/_static/image19.png)](displaying-a-custom-error-page-vb/_static/image18.png)**図 7**: カスタム 404 エラー ページより対象を絞ったメッセージが表示されます。 `Oops.aspx`  
 ([フルサイズのイメージを表示するをクリックして](displaying-a-custom-error-page-vb/_static/image20.png)) 

わかっているため、`404.aspx`ページは、ユーザーが見つからなかったページに対する要求を行うときに、のみ、この特定の種類のエラーに対応するユーザーを支援する機能を含めるには、このカスタム エラー ページを拡張することができます。 や既知の適切な Url に無効な Url をマップするデータベース テーブルを構築する可能性がありますなどの`404.aspx`テーブルし、ページのユーザーがアクセスしようを提案するカスタム エラー ページに対してクエリを実行します。

> [!NOTE]
> カスタム エラー ページは ASP.NET エンジンによって処理され、リソースへの要求が行われたときにのみ表示されます。 説明したよう、[コア違いの間で IIS と ASP.NET 開発サーバー](core-differences-between-iis-and-the-asp-net-development-server-vb.md)チュートリアルでは、web サーバーが特定の要求を処理自体です。 既定では、IIS web HTML ファイル、画像などの静的なコンテンツのサーバー プロセスの要求を使用 ASP.NET エンジンを呼び出さずにします。 その結果、ユーザーが存在しないイメージ ファイルを要求した場合、返ってくる ASP ではなく、IIS の既定の 404 エラー メッセージ。NET の構成済みのエラー ページです。


## <a name="summary"></a>まとめ

ASP.NET アプリケーションでハンドルされない例外が発生したときに、ユーザーが表示 3 つのエラー ページのいずれかの: 例外の詳細黄色画面の死亡です。黄色死亡; の 画面の実行時エラーまたは、カスタム エラー ページを選択します。 アプリケーションのエラー ページが表示される異なります`<customErrors>`構成とユーザーがローカルまたはリモートでアクセスするかどうか。 既定の動作では、リモート ユーザーにローカルの訪問者を例外の詳細 YSOD とランタイム エラー YSOD を説明します。

ランタイム エラー YSOD には、サイトにアクセスするユーザーからの機密性の高いエラー情報が非表示に、中に、サイトのルック アンド フィールから分解し、によってにより、アプリケーション バグを探します。 適切な方法は、作成し、カスタム エラー ページのデザインでその URL を指定する、カスタム エラー ページを使用する、`<customErrors>`セクションの`defaultRedirect`属性。 さまざまな HTTP エラー状態に対する複数のカスタム エラー ページを持つこともできます。

カスタム エラー ページは、実稼働環境で web サイトの戦略を処理する包括的なエラーの最初の手順です。 エラーの開発者のアラートを生成し、その詳細をログ記録も重要な手順です。 次の 3 つのチュートリアルでは、エラーの通知とログ記録する方法について説明します。

満足プログラミング!

## <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [エラー ページ、もう一度](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [例外のデザインのガイドライン](https://msdn.microsoft.com/library/ms229014.aspx)
- [わかりやすいエラー ページ](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [処理と例外をスロー](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [ASP.NET のカスタム エラー ページを使用して正しく](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [前へ](strategies-for-database-development-and-deployment-vb.md)
> [次へ](processing-unhandled-exceptions-vb.md)
