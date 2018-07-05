---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
title: カスタム エラー ページ (c#) の表示 |Microsoft Docs
author: rick-anderson
description: ユーザーに表示される内容を ASP.NET web アプリケーションでランタイム エラーが発生します。 方法によって異なります web サイトの&lt;customErrors&gt;構成しています.。
ms.author: aspnetcontent
ms.date: 06/09/2009
ms.assetid: cb061642-faf3-41b2-9372-69e13444d458
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/displaying-a-custom-error-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 8cd10806b3c803269d00265156ca618aef1db13a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37828684"
---
<a name="displaying-a-custom-error-page-c"></a>カスタム エラー ページ (c#) を表示します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードのダウンロード](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_11_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial11_CustomErrors_cs.pdf)

> ユーザーに表示される内容を ASP.NET web アプリケーションでランタイム エラーが発生します。 方法によって異なります。 web サイトの&lt;customErrors&gt;構成します。 既定では、ユーザーは、実行時エラーが発生したことを掲げ見苦しいの黄色い画面に表示されます。 このチュートリアルでは、サイトのルック アンド フィールに一致するカスタム エラー ページを表示、美しくて心地よいこれらの設定をカスタマイズする方法を示します。


## <a name="introduction"></a>はじめに

理想の世界ではない実行時エラー。 プログラマはコードを記述せずに、バグと、堅牢なユーザー入力の検証と外部データベース サーバーと電子メール サーバーなどのリソースはオフラインで作業しません。 もちろん、実際にはエラーのない避けします。 .NET Framework のクラスは、例外をスローしてエラーを通知します。 オブジェクトの Open メソッドはなど、SqlConnection を呼び出して接続文字列で指定されたデータベースへの接続を確立します。 ただし、データベースがダウンした場合、または接続文字列の資格情報が有効でない場合、Open メソッドがスローされます、`SqlException`します。 使用して例外を処理できる`try/catch/finally`ブロックします。 場合内のコードを`try`ブロックが例外をスロー、開発者が、エラーから回復を試みることができます、適切な catch ブロックに制御が移ります。 一致する catch ブロックがないか、例外が percolates search の呼び出し履歴を例外をスローしたコードが try ブロックにない場合は場合、`try/catch/finally`ブロックします。

場合は、例外が処理されることがなく、ASP.NET ランタイムまでバブル、 [ `HttpApplication`クラス](https://msdn.microsoft.com/library/system.web.httpapplication.aspx)の[`Error`イベント](https://msdn.microsoft.com/library/system.web.httpapplication.error.aspx)が発生し、構成された*エラー ページ*が表示されます。 既定では、ASP.NET に愛情深いと呼ばれるエラー ページが表示されます、[死の黄色い画面](http://en.wikipedia.org/wiki/Yellow_Screen_of_Death#Yellow)(YSOD)。 YSOD の 2 つのバージョンがあります 1 つを示しています、例外の詳細、スタック トレース、およびその他の情報を開発者がアプリケーションのデバッグに役立ちます (を参照してください**図 1**); 実行時エラー (参照があったことを示して、その他。**図 2**)。

例外の詳細 YSOD 開発者は、アプリケーションのデバッグに非常に便利ですが、YSOD をエンドユーザーに示す安っぽいと不自然です。 代わりに、エンドユーザーよりユーザー フレンドリな prose の状況を説明すると、サイトのルック アンド フィールを維持するエラー ページに実行する必要があります。 良い知らせは、このようなカスタム エラー ページを作成することは非常に簡単です。 このチュートリアルでは ASP を参照してください。NET の別のエラー ページ。 これは、後ユーザー エラーが発生した場合、カスタム エラー ページを表示する web アプリケーションを構成する方法を示します。

### <a name="examining-the-three-types-of-error-pages"></a>次の 3 つの種類のエラー ページを調べる

エラー ページの 3 種類のいずれかの ASP.NET アプリケーションでハンドルされない例外が発生したときに表示されます。

- 例外の詳細死亡黄色い画面のエラー ページ
- ランタイム エラー死亡黄色い画面エラー ページ、または
- カスタム エラー ページ

エラー ページの開発者は最も一般的では、例外の詳細 YSOD です。 既定では、このページはローカルにアクセスしているユーザーに表示される、そのため、開発環境で、サイトをテストするときにエラーが発生したときに表示されるページは。 その名前が示すように、例外の詳細 YSOD は、種類、メッセージ、およびスタック トレース、例外に関する詳細を提供します。 さらに、ASP.NET ページの分離コード クラス内のコードで例外が発生した場合と、アプリケーションがデバッグ用に構成されている場合、例外の詳細 YSOD も表示されますこの行のコード (および上記のコードとその下に、いくつかの行)。

**図 1**例外詳細 YSOD ページが表示されます。 ブラウザーのアドレス ウィンドウ内の URL に注意してください:`http://localhost:62275/Genre.aspx?ID=foo`します。 いることを思い出してください、`Genre.aspx`ページには、特定のジャンルの書籍レビューが一覧表示されます。 必要があります`GenreId`値 (、 `uniqueidentifier`); クエリ文字列を通じて渡される fiction レビューを表示するための適切な URL は、たとえば、`Genre.aspx?ID=7683ab5d-4589-4f03-a139-1c26044d0146`します。 以外の場合`uniqueidentifier`値は、クエリ文字列 ("foo") などを通じて、例外がスローされます。

> [!NOTE]
> デモの web アプリケーションでは、このエラーを再現するダウンロードは`Genre.aspx?ID=foo`直接「、ランタイム エラーを生成する」リンクをクリックしてまたは`Default.aspx`します。


表示される例外情報に注意してください**図 1**します。 例外メッセージでは、「変換に失敗した文字の文字列から uniqueidentifier に変換するときに」することは、ページの上部に存在します。 例外の種類`System.Data.SqlClient.SqlException`も、表示されます。 スタック トレースもあります。

[![](displaying-a-custom-error-page-cs/_static/image2.png)](displaying-a-custom-error-page-cs/_static/image1.png)

**図 1**: YSOD 例外の詳細には、例外に関する情報が含まれています。  
 ([フルサイズの画像を表示する をクリックします](displaying-a-custom-error-page-cs/_static/image3.png))。

YSOD の他の種類に、ランタイム エラー YSOD、**図 2**します。 実行時のエラー YSOD 実行時エラーが発生した、訪問者に通知がスローされた例外に関する情報は含まれません。 (変更することで、エラーの詳細を表示できるようにする方法の手順は、提供、`Web.config`ファイルは、このような不自然に見える YSOD の一部です)。

既定では、ランタイム エラー YSOD がリモート アクセス ユーザーが表示されます (を通じて http://www.yoursite.com) ことで、ブラウザーのアドレス バーに URL からわかるように、**図 2**:`http://httpruntime.web703.discountasp.net/Genre.aspx?ID=foo`です。 2 つの異なる YSOD 画面存在開発者は、エラーの詳細に関心が潜在的なセキュリティの脆弱性やその他の機密情報を訪れる人を明らかに可能性がありますが、このような情報をライブ サイトで表示しないため、サイトです。

> [!NOTE]
> 従うし、web ホストとして DiscountASP.NET を使用している場合、ライブ サイトにアクセスすると、ランタイム エラー YSOD は表示されませんお気付きです。 DiscountASP.NET があるそのサーバーが既定では、例外の詳細 YSOD を表示するように構成するためです。 良い知らせは、追加してこの既定の動作をオーバーライドすることができます、`<customErrors>`セクションを`Web.config`ファイル。 「構成エラー ページが表示される」セクションを調べ、`<customErrors>`この後説明します。


[![](displaying-a-custom-error-page-cs/_static/image5.png)](displaying-a-custom-error-page-cs/_static/image4.png)

**図 2**: ランタイム エラー YSOD では、エラーの詳細は含まれません  
 ([フルサイズの画像を表示する をクリックします](displaying-a-custom-error-page-cs/_static/image6.png))。

エラー ページの 3 番目の型は、web ページを作成するには、カスタム エラー ページです。 カスタム エラー ページの利点は、ページのルック アンド フィール; と共にユーザーに表示される情報を完全に制御があることです。カスタム エラー ページは、その他のページとして、同じマスター ページやスタイルを使用できます。 「を使用して、カスタム エラー ページ」セクションでは、カスタム エラー ページを作成し、ハンドルされない例外が発生した場合に表示するように構成する手順について説明します。 **図 3**このカスタム エラー ページのヒントのピークを提供します。 ご覧のように、エラー ページのルック アンド フィールはより洗練された外観よりも、黄色の画面の死図 1 と 2 のいずれか。

[![](displaying-a-custom-error-page-cs/_static/image8.png)](displaying-a-custom-error-page-cs/_static/image7.png)

**図 3**: カスタム エラー ページをより細かく調整されたルック アンド フィールを提供しています  
 ([フルサイズの画像を表示する をクリックします](displaying-a-custom-error-page-cs/_static/image9.png))。

ブラウザーのアドレス バーを検査する少し**図 3**します。 アドレス バーには、カスタム エラー ページの URL が表示されます (`/ErrorPages/Oops.aspx`)。 同じページから、エラーが発生したことで図 1 および 2 で死の黄色い画面が表示されます (`Genre.aspx`)。 カスタム エラー ページを使用して、エラーが発生するページの URL を渡される、`aspxerrorpath`クエリ文字列パラメーター。

## <a name="configuring-which-error-page-is-displayed"></a>これは、エラー ページの構成が表示されます。

3 つの可能性のあるエラー ページの表示は、2 つの変数に基づいています。

- 構成情報、`<customErrors>`セクションと
- かどうか、ユーザーは、ローカルまたはリモートでサイトを訪問は。

[ `<customErrors>`セクション](https://msdn.microsoft.com/library/h0hfz6fc.aspx)で`Web.config`がどのようなエラー ページが表示に影響する 2 つの属性:`defaultRedirect`と`mode`します。 `defaultRedirect` 属性は省略できます。 指定した場合、カスタム エラー ページの URL を指定し、ランタイム エラーの YSOD ではなく、カスタム エラー ページが表示することを示します。 `mode`属性が必要ですし、3 つの値のいずれかを受け入れます。 `On`、 `Off`、または`RemoteOnly`します。 これらの値には、次の動作があります。

- `On` -カスタム エラー ページまたはランタイム エラーの YSOD がローカルまたはリモートいるかどうかに関係なく、すべての訪問者に表示されていることを示します。
- `Off` -例外の詳細 YSOD がローカルまたはリモートいるかどうかに関係なく、すべての訪問者に表示されることを指定します。
- `RemoteOnly` -カスタム エラー ページまたはランタイム エラーの YSOD が表示されているリモートの訪問者に、例外の詳細 YSOD がローカルの訪問者に表示することを示します。

ASP.NET が動作しますモード属性を設定している場合、それ以外の場合、指定しない限り`RemoteOnly`が指定されていないと、`defaultRedirect`値。 つまり、既定の動作では、ランタイム エラー YSOD がリモートの訪問者に示すように、ローカルの訪問者に例外の詳細 YSOD が表示されます。 追加して、この既定の動作をオーバーライドすることができます、 `<customErrors>` web アプリケーションのセクション `Web.config file.`

## <a name="using-a-custom-error-page"></a>カスタム エラー ページを使用します。

すべての web アプリケーションでは、カスタム エラー ページが必要です。 ランタイム エラーの YSOD より洗練された外観の代替を提供します、簡単に作成するには、およびカスタム エラー ページを使用するアプリケーションを構成するだけに時間がかかります。 最初の手順は、カスタム エラー ページを作成します。 という名前の書評アプリケーションに新しいフォルダーを追加して`ErrorPages`という名前の新しい ASP.NET ページに追加して`Oops.aspx`します。 同じルック アンド フィールを自動的に継承するように、サイトのページの残りの部分として同じマスター ページを使用して、ページがあります。

[![](displaying-a-custom-error-page-cs/_static/image11.png)](displaying-a-custom-error-page-cs/_static/image10.png)

**図 4**: カスタム エラー ページを作成します。

次に、数分をかけてのエラー ページのコンテンツを作成します。 単純なカスタム エラー ページを示すメッセージが予期しないエラーが発生しました、サイトのホーム ページに戻る、リンクを作成しました。

[![](displaying-a-custom-error-page-cs/_static/image13.png)](displaying-a-custom-error-page-cs/_static/image12.png)

**図 5**: カスタム エラー ページのデザイン  
 ([フルサイズの画像を表示する をクリックします](displaying-a-custom-error-page-cs/_static/image14.png))。

完了、エラー ページには、代わりに、ランタイム エラー YSOD カスタム エラー ページを使用する web アプリケーションを構成します。 エラー ページの URL を指定することでこれは、`<customErrors>`セクションの`defaultRedirect`属性。 次のマークアップをアプリケーションに追加`Web.config`ファイル。

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample1.xml)]

上記のマークアップは、例外の詳細 YSOD をリモートでアクセスしてそれらのユーザーを Oops.aspx カスタム エラー ページを使用しながら、ローカルにアクセスするユーザーに表示するアプリケーションを構成します。 アクションの表示は、web サイトの運用環境に配置し、し、無効なクエリ文字列値を持つライブ サイトで Genre.aspx ページを参照してください。 カスタム エラー ページが表示されます (参照**図 3**)。

カスタム エラー ページは、リモート ユーザーにのみ表示を確認するを参照してください。、`Genre.aspx`開発環境から、無効なクエリ文字列を含むページ。 例外の詳細 YSOD も表示されます (参照**図 1**)。 `RemoteOnly`ローカルで作業する開発者が、例外の詳細を参照してください。 引き続き、実稼働環境で、サイトを訪問するユーザーはカスタム エラー ページをご覧ください。 設定になります。

## <a name="notifying-developers-and-logging-error-details"></a>開発者を通知して、エラーの詳細をログ記録

開発環境で発生するエラーは、自分のコンピューターに座っている開発者によって発生しました。 彼女には、例外の詳細 YSOD で例外の情報が表示され、彼女は、エラーが発生したときに実行していた、どのような手順がわかっています。 運用環境では、エラーが発生するときに、開発者にナレッジをサイトにアクセスするエンドユーザーにエラーを報告する時間がかかる場合を除き、エラーが発生したことはありません。 ユーザー例外の種類、メッセージ、およびそれを修正することはおろか、エラーの原因を診断することは難しいスタック トレースを知ることがなく、エラーが発生しました開発チームのアラートを生成する途中から外れた場合でもです。

(データベース) などのいくつかの永続的なストアを運用環境でエラーが記録する最優先事項はこれらの理由から、開発者はこのエラーの警告が表示されます。 カスタム エラー ページは、このログ記録および通知を実行する適切な場所のように思えます。 残念ながら、カスタム エラー ページが、エラーの詳細にアクセスできないと、この情報をログに使用することはできません。 良い知らせは、エラーの詳細をインターセプトして、ログに記録する方法がいくつかを次の 3 つのチュートリアルでは、このトピックで詳細を確認できます。 には。

## <a name="using-different-custom-error-pages-for-different-http-error-statuses"></a>別の HTTP エラー状態の別のカスタム エラー ページの使用

例外は、ASP.NET ページによってスローされ、処理されない、例外は percolates まで ASP.NET の実行時は、構成済みのエラー ページが表示されます。 ファイル アクセス許可が無効になっている要求は、ASP.NET エンジンしますが、何らかの理由は処理できません - など、要求されたファイルが見つからなかったか読み取る場合は、ASP.NET エンジン、`HttpException`します。 ASP.NET ページなどから発生した例外のように、この例外が原因で適切なエラー ページが表示される、実行時までバブルします。

実稼働環境で web アプリケーションのつまりされている場合、ユーザーがカスタム エラー ページが表示されますが、存在しないページを要求します。 **図 6**このような例を示しています。 要求が存在しないページのため、(`NoSuchPage.aspx`)、`HttpException`スローされるとカスタム エラー ページが表示されます (への参照に注意してください`NoSuchPage.aspx`で、`aspxerrorpath`クエリ文字列パラメーター)。

[![](displaying-a-custom-error-page-cs/_static/image16.png)](displaying-a-custom-error-page-cs/_static/image15.png)

**図 6**: ASP.NET ランタイム構成エラー ページの応答に無効な要求を表示します ([フルサイズの画像を表示する をクリックします](displaying-a-custom-error-page-cs/_static/image17.png))。

既定では、すべての種類のエラーが発生する同じカスタム エラー ページに表示されます。 ただし、特定 HTTP ステータス コードを使用して、別のカスタム エラー ページを指定できます`<error>`内の子要素、`<customErrors>`セクション。 たとえば、更新が見つからないというエラーで、HTTP 状態コード 404 は、ページが発生した場合に表示される別のエラー ページがある、`<customErrors>`セクションに次のマークアップを含めます。

[!code-xml[Main](displaying-a-custom-error-page-cs/samples/sample2.xml)]

この変更をリモートでアクセスしたユーザーが存在しない場合、ASP.NET リソースを要求したときにそれらにリダイレクトされます、`404.aspx`の代わりにカスタム エラー ページ`Oops.aspx`します。 として**図 7**に示した、`404.aspx`ページは、一般的なカスタム エラー ページよりも具体的なメッセージを含めることができます。

> [!NOTE]
> チェック アウト[404 エラー ページ、1 つ以上の時間](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)効果的な 404 エラー ページを作成する方法のガイダンスについてはします。


[![](displaying-a-custom-error-page-cs/_static/image19.png)](displaying-a-custom-error-page-cs/_static/image18.png)**図 7**: カスタム 404 エラー ページより対象が絞られたメッセージが表示されます。 `Oops.aspx`  
 ([フルサイズの画像を表示する をクリックします](displaying-a-custom-error-page-cs/_static/image20.png))。 

あるとわかっているため、`404.aspx`ページは、ユーザーが見つかりませんページを要求すると、のみと、この特定の種類のエラーに対応するユーザーを支援する機能を含めるには、このカスタム エラー ページを強化できます。 たとえば、既知の適切な Url に無効な Url をマップするデータベース テーブルを作成してがし、`404.aspx`カスタム エラー ページに対してクエリを実行すると、テーブルが表示され、ユーザーがアクセスしようページをお勧めします。

> [!NOTE]
> カスタム エラー ページは、ASP.NET エンジンによって処理されるリソースに要求が行われた場合にのみ表示されます。 説明したように、 [Core の相違点の間で IIS と ASP.NET 開発サーバー](core-differences-between-iis-and-the-asp-net-development-server-cs.md)チュートリアル, web サーバーが特定の要求を処理自体。 既定では、IIS web 画像や HTML ファイルなどの静的コンテンツのサーバー プロセスの要求を使用、ASP.NET エンジンを呼び出さずにします。 その結果、ユーザーが存在しないイメージ ファイルを要求した場合は戻る ASP ではなく、IIS の既定の 404 エラー メッセージ。NET のでは、エラー ページを構成します。


## <a name="summary"></a>まとめ

次の 3 つのエラー ページのいずれかを表示、ASP.NET アプリケーションでハンドルされない例外が発生した場合: 例外の詳細黄色い画面の終焉;実行時エラー黄色の死; の画面または、カスタム エラー ページ。 アプリケーションのエラー ページが表示される異なります`<customErrors>`構成し、ローカルまたはリモート ユーザーがアクセスしたかどうか。 既定の動作では、リモートの訪問者にローカルの訪問者に例外の詳細 YSOD とランタイム エラーの YSOD を説明します。

ランタイム エラーの YSOD には、サイトにアクセスするユーザーからの可能性がある機密性の高いエラー情報が非表示に、中にサイトのルック アンド フィールから中断し、により、アプリケーションのバグの外観します。 優れたアプローチはカスタム エラー ページで、作成、カスタム エラー ページのデザインとでは、その URL を指定する必要がありますを使用する、`<customErrors>`セクションの`defaultRedirect`属性。 さまざまな HTTP エラー状態に対する複数のカスタム エラー ページのこともできます。

カスタム エラー ページは、実稼働環境で web サイトの戦略を処理する包括的なエラーの最初の手順です。 エラーの開発者向けの警告とその詳細をログ記録は、重要な手順ではもです。 次の 3 つのチュートリアルでは、エラーの通知とログ記録のための手法について説明します。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [エラー ページ、もう 1 回](http://www.smashingmagazine.com/2009/01/29/404-error-pages-one-more-time/)
- [例外のデザインのガイドライン](https://msdn.microsoft.com/library/ms229014.aspx)
- [わかりやすいエラー ページ](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [処理と例外をスロー](https://msdn.microsoft.com/library/5b2yeyab.aspx)
- [ASP.NET でカスタム エラー ページを正しく使用](http://professionalaspnet.com/archive/2007/09/30/Properly-Using-Custom-Error-Pages-in-ASP.NET.aspx)

> [!div class="step-by-step"]
> [前へ](strategies-for-database-development-and-deployment-cs.md)
> [次へ](processing-unhandled-exceptions-cs.md)
