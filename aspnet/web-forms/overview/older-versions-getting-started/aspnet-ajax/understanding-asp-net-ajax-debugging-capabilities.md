---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: ASP.NET AJAX デバッグ機能を理解する |Microsoft Docs
author: scottcate
description: コードをデバッグする機能は、すべての開発者で使用しているテクノロジに関係なく、コレクションに加えることが必要なスキルです。 多くの開発者には.
ms.author: aspnetcontent
ms.date: 03/28/2008
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: 95c2487f26109cbdd8c76dc6f269f37264f5e34b
ms.sourcegitcommit: 028ad28c546de706ace98066c76774de33e4ad20
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/08/2018
ms.locfileid: "39655447"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>ASP.NET AJAX デバッグ機能を理解します。
====================
によって[Scott Cate](https://github.com/scottcate)

[PDF のダウンロード](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> コードをデバッグする機能は、すべての開発者で使用しているテクノロジに関係なく、コレクションに加えることが必要なスキルです。 多くの開発者が Visual Studio .NET または Web Developer Express を使用して、VB.NET や c# のコードを使用する ASP.NET アプリケーションをデバッグすることに慣れて、JavaScript などのクライアント側コードのデバッグに非常に便利ですがもことに注意して存在しません。 同じ種類の .NET アプリケーションをデバッグするために使用する手法は、AJAX 対応アプリケーションとより具体的には ASP.NET AJAX アプリケーションにも適用できます。


## <a name="debugging-aspnet-ajax-applications"></a>ASP.NET AJAX アプリケーションのデバッグ

Dan Wahlin

コードをデバッグする機能は、すべての開発者で使用しているテクノロジに関係なく、コレクションに加えることが必要なスキルです。 利用できるさまざまなデバッグ オプションについて保存できます膨大な時間、プロジェクト、およびおそらくいくつかの問題で言うまでもなくなります。 多くの開発者が Visual Studio .NET または Web Developer Express を使用して、VB.NET や c# のコードを使用する ASP.NET アプリケーションをデバッグすることに慣れて、JavaScript などのクライアント側コードのデバッグに非常に便利ですがもことに注意して存在しません。 同じ種類の .NET アプリケーションをデバッグするために使用する手法は、AJAX 対応アプリケーションとより具体的には ASP.NET AJAX アプリケーションにも適用できます。

この記事では、Visual Studio 2008 とその他のいくつかのツールを使用して、バグおよびその他の問題をすばやく検索する ASP.NET AJAX アプリケーションをデバッグする方法が表示されます。 この説明は、Internet Explorer 6 以降のデバッグ、Visual Studio 2008 と、スクリプト エクスプ ローラーを使用してコードをステップ実行すると、Web Development Helper などの他の無償ツールを使用して有効にする方法の情報が含まれます。 Firefox では、拡張機能が Firebug を他の任意のツールがない場合、ブラウザーで直接 JavaScript コードをステップ実行できるという名前を使用して ASP.NET AJAX アプリケーションをデバッグする方法も学習します。 最後に、トレースとアサーションのコード ステートメントなどのさまざまなデバッグ タスクに役立つ、ASP.NET AJAX Library のクラスを導入します。

Internet Explorer で表示するページをデバッグしようとする前に、いくつかの基本的な手順をデバッグできるようにするために実行する必要があります。 開始するために実行する必要があるいくつかの基本的なセットアップ要件を見ていきましょう。

## <a name="configuring-internet-explorer-for-debugging"></a>デバッグ用の Internet Explorer の構成

ほとんどの人は、Internet Explorer で表示する web サイトで発生した JavaScript の問題の確認に関心がないです。 実際には、平均的なユーザーもご存知エラー メッセージが表示されていた場合の対処方法。 結果として、デバッグ オプションは、既定では、ブラウザーでオフにされます。 ただし、非常に簡単にデバッグを有効にして、新しい AJAX アプリケーションを開発するときに使用するように配置されます。

デバッグ機能を有効にするのには、Internet Explorer メニューの [インターネット オプションのツールに移動し、詳細設定] タブを選択します。参照セクション内で、次の項目がチェックされていないことを確認します。

- スクリプトのデバッグ (Internet Explorer) を無効にします。
- スクリプトのデバッグ (その他) を無効にします。

必須ではない場合を一見してわかるすぐページに JavaScript エラーがあるとよいでしょうアプリケーションをデバッグしようとしています。 「すべてのスクリプト エラー通知を表示する」のチェック ボックスをオンのメッセージ ボックスに表示されるすべてのエラーを強制することができます。 これは、アプリケーションの開発中は有効にする優れたオプションですが、面倒なは、JavaScript のエラーが発生する可能性はかなり高いためだけ他の web サイトを参照している場合は、すぐにできます。

図 1 に示しますどのような Internet Explorer のダイアログの詳細は、後デバッグを正しく構成されているはずです。


[![デバッグの Internet Explorer を設定します。](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**図 1**: デバッグの Internet Explorer を構成します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))。


デバッグが有効になっていると、スクリプト デバッガーをという名前の表示 メニューに表示されます、新しいメニュー項目を確認します。 次のステートメントでオープンおよび break の移動を含む使用可能な 2 つのオプションがあります。 オープンを選択すると Visual Studio 2008 (Visual Web Developer Express 使用できることもデバッグするために注意してください) でページをデバッグする求め。 Visual Studio .NET は現在実行されている場合は、そのインスタンスを使用する新しいインスタンスを作成するかを選択できます。 次のステートメントで中断が選択されているときに JavaScript コードを実行すると、ページをデバッグする求め。 ページの onLoad イベントで JavaScript コードを実行する場合は、デバッグ セッションをトリガーするページを更新することができます。 ボタンがクリックされた後の JavaScript コードが実行される場合は、このボタンをクリックした直後に、デバッガーは実行されます。

> [!NOTE]
> Windows Vista ユーザー アクセス制御 (UAC) が有効になっているでを実行して、Visual Studio 2008 が、管理者として実行する設定がある場合、アタッチするメッセージが表示されたら、プロセスにアタッチする Visual Studio は失敗します。 この問題を回避するには、最初に、Visual Studio を起動し、そのインスタンスを使用してデバッグします。

次のセクションでは、Visual Studio 2008 内から直接 ASP.NET AJAX ページをデバッグする方法について説明、Internet Explorer スクリプト デバッガー オプションを使用してページがまだ開いているより詳細に検査したいときに便利です。

## <a name="debugging-with-visual-studio-2008"></a>Visual Studio 2008 でのデバッグ

Visual Studio 2008 では、世界中の開発者が .NET アプリケーションをデバッグする日常的に依存するデバッグの機能を提供します。 組み込みのデバッガー ビューのオブジェクト データを特定の変数のウォッチ、コール スタックとその他の監視コードのステップ スルーすることができます。 に加えて、VB.NET や c# のコードをデバッグするには、デバッガーも ASP.NET AJAX アプリケーションのデバッグに便利ですし、JavaScript コード 1 行ずつステップ実行できます。 Visual Studio 2008 を使用してアプリケーションをデバッグするプロセス全体での discourse を提供するのではなく、クライアント側スクリプト ファイルのデバッグに使用できる手法にフォーカスを次の詳細。

いくつかの方法では、Visual Studio 2008 でのページをデバッグするプロセスを開始できます。 最初に、前のセクションで説明したように Internet Explorer スクリプト デバッガー オプションを使用することができます。 これは、ページがブラウザーで既に読み込まれて、デバッグを開始したい場合に適しています。 ソリューション エクスプ ローラーで .aspx ページの右クリックし、メニューからスタート ページとして設定を選択することができます。 ASP.NET ページをデバッグするのに慣れている場合、おそらくこれが完了したらする前にします。 F5 キーを押すと、ページをデバッグできます。 ただし、任意の場所、ブレークポイントを設定できる通常 VB.NET や c# のコードが常に JavaScript の場合と次のようにするとします。

*外部スクリプトと埋め込み*

Visual Studio 2008 デバッガーでは、外部の JavaScript ファイルとは異なるページに埋め込まれた JavaScript を扱います。 外部のスクリプト ファイルは、ファイルを開くし、選択した任意の行にブレークポイントを設定します。 コード エディター ウィンドウの左側の灰色のトレイ領域内をクリックして、ブレークポイントを設定できます。 JavaScript を使用してページに直接埋め込まれたとき、`<script>`タグ、灰色のトレイ をクリックしてブレークポイントを設定することができません。 埋め込みスクリプトの行にブレークポイントを設定しようとすると、「これは、ブレークポイントの有効な場所」を示す警告が発生します。

この問題を回避の src 属性を使用して参照を外部 .js ファイルにコードを移動して、&lt;スクリプト&gt;タグ。


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

場合、コードを外部ファイルに移動することができませんまたはよりも多くの作業が必要ですが価値でしょうか。 エディターを使用してブレークポイントを設定することはできません、中には、デバッグを開始するコードに直接デバッガー ステートメントを追加することができます。 強制的にデバッグを開始するのに ASP.NET AJAX ライブラリで Sys.Debug クラスを使用することもできます。 この記事の後半で Sys.Debug クラスについて説明します。

使用例、`debugger`キーワードはリスト 1 で表示します。 この例では、デバッガーが、更新関数の呼び出しが行われる前に、適切な中断を強制します。

**1 を一覧表示します。デバッガーのキーワードを使用して、強制的に Visual Studio .NET デバッガーが中断します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

デバッガー ステートメントに達すると、Visual Studio .NET を使用してページをデバッグするように求められ、コードをステップ実行を開始することができます。 それでは ページで使用する ASP.NET AJAX ライブラリ スクリプト ファイルへのアクセスに関する問題が発生する可能性がありますこれを行うときに Visual Studio を使用して見てください。NET のスクリプト エクスプ ローラー。

## <a name="using-visual-studio-net-windows-to-debug"></a>Visual Studio .NET の Windows を使用してデバッグするには

デバッグ セッションが開始される、既定 F11 キーを使用してコードのウォークを開始してに示すように、エラー ダイアログが表示される可能性があると、ページで使用されるすべてのスクリプト ファイルが開いて、デバッグに使用できる場合を除き、図 2 を参照してください。


[![エラー ダイアログがソース コードが使用できない場合のデバッグを表示します。](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**図 2**: エラー ダイアログのソース コードが使用できない場合のデバッグを表示します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))。


Visual Studio .NET がないためことを確認して、ページによって参照されるスクリプトの一部のソース コードを取得する方法は、このダイアログ ボックスが表示されます。 これが非常にイライラすることができます、最初は、単純な解決策です。 デバッグ セッションを開始し、ブレークポイントにヒットしたら後、は、Visual Studio 2008 のメニューの [デバッグの Windows スクリプト エクスプ ローラー] ウィンドウに移動または Ctrl + Alt + N ホットキーを使用します。

> [!NOTE]
> 表示されているスクリプト エクスプ ローラーのメニューを表示されない場合に移動**ツール** > **カスタマイズ** > **コマンド**Visual Studio .NET メニュー。 検索、**デバッグ**カテゴリ内のエントリ セクションし、使用可能なメニュー エントリがすべて表示する をクリックします。 コマンドの一覧では、スクリプト エクスプ ローラーまで下へスクロールし、メニューには前に説明したデバッグの Windows ドラッグします。 これが、スクリプト エクスプ ローラー メニュー エントリ利用できるように Visual Studio .NET を実行するたびにします。

スクリプト エクスプ ローラーは、ページで使用されるすべてのスクリプトを表示し、コード エディターで開くことができます。 コード エディター ウィンドウで開く現在デバッグ中の .aspx ページで、スクリプト エクスプ ローラーが開いたらをダブルクリックします。 スクリプト エクスプ ローラーに示すように、他のスクリプトのすべての同じアクションを実行します。 できますが、コード ウィンドウで開いているすべてのスクリプトと F11 キーを押して (とその他のデバッグ ホット キーを使用して) コードをステップ実行します。 図 3 は、スクリプト エクスプ ローラーの例を示します。 2 つのカスタム スクリプトと ASP.NET AJAX scriptmanager コントロールによって、ページに動的に挿入する 2 つのスクリプト デバッグされている現在のファイル (Demo.aspx) が一覧表示します。


[![スクリプト エクスプ ローラーでは、ページで使用されるスクリプトに簡単にアクセスを提供します。](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**図 3**します。 スクリプト エクスプ ローラーでは、ページで使用されるスクリプトに簡単にアクセスを提供します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))。


その他いくつかの windows がページでコードをステップ実行すると、有益な情報を提供することもできます。 たとえば、[ローカル] ウィンドウを使用すると、ページでは、特定の変数または条件を評価し、出力を表示するのに、イミディ エイト ウィンドウで使用される別の変数の値を参照してください。 (これはこの記事の後半で説明) Sys.Debug.trace 関数または Internet Explorer の Debug.writeln 関数を使用して書き込むトレース ステートメントを表示するのに出力ウィンドウを使用することもできます。

デバッガーを使用してコードをステップ実行とが割り当てられている値を表示するコードの変数にマウスを置くことができます。 ただし、スクリプト デバッガー場合によっては表示されません何も指定された JavaScript 変数にマウスを置くと。 値を表示するには、ステートメントまたはコード エディター ウィンドウに表示し、マウス ポインターをしようとしている変数を選択します。 すべての状況では、この手法は機能しませんが何度もができる [ローカル] ウィンドウなどのさまざまなデバッグ ウィンドウで検索することがなく、値を参照してください。

いくつかの記事で取り上げる機能のデモ ビデオ チュートリアルを表示できます[ http://www.xmlforasp.net](http://www.xmlforasp.net)します。

## <a name="debugging-with-web-development-helper"></a>Web Development Helper を使用したデバッグ

Visual Studio 2008 (と Visual Web Developer Express 2008) は、非常に有能なデバッグ ツールは追加のオプションも使用できるより軽量であることがあります。 リリースされる最新のツールの 1 つは、Web Development Helper です。 Microsoft の Nikhil kothari 氏 (マイクロソフトのキーの ASP.NET AJAX の設計者の 1 つ) は、HTTP 要求と応答メッセージを表示する単純なデバッグからさまざまなタスクを実行できるこの優れたツールを作成しました。 ダウンロードできる web Development Helper [ http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx)します。

Web Development helper を使用すると便利になります Internet Explorer の内部で直接使用できます。 Internet Explorer のメニューから Web Development Helper のツールを選択して開始します。 HTTP 要求と応答メッセージのログ記録などのいくつかのタスクを実行するブラウザーのままにする必要はありませんので便利ですが、ブラウザーの下部で、ツールが開きます。 図 4 は、アクションで Web Development Helper がどのようにを示します。


[![Web Development Helper](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**図 4**: Web Development Helper ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))。


Web Development helper を使用してツールのない 1 行ずつとして Visual Studio 2008 でのコードをステップ実行します。 ただし、トレース出力を表示、簡単にスクリプトで変数を評価または探索を使用できますが、JSON オブジェクト内のデータが。 ASP.NET AJAX ページとサーバーとの間に渡されるデータを表示するために非常に便利なもできます。

Web Development Helper は Internet Explorer で開くが、スクリプトを有効にするスクリプトのデバッグ メニューから選択して、Web 開発ヘルパー図 4 で前に示したスクリプトのデバッグを有効する必要があります。 これにより、ページの実行時に発生するエラーをインターセプトするツールです。 ページに出力されるトレース メッセージを簡単にアクセスできます。 トレース情報を表示またはページ内の別の関数をテストするスクリプト コマンドの実行、Web Development Helper メニューからスクリプトを表示するスクリプトのコンソールを選択します。 これは、コマンド ウィンドウと単純なイミディ エイト ウィンドウへのアクセスを提供します。

*トレース メッセージと JSON オブジェクトのデータを表示*

スクリプト コマンドを実行またはでも読み込みまたはページのさまざまな機能をテストに使用されるスクリプトを保存するのには、イミディ エイト ウィンドウを使用できます。 コマンド ウィンドウには、表示されているページによって書き出されたトレースまたはデバッグ メッセージが表示されます。 リスト 2 は、Internet Explorer の Debug.writeln 関数を使用して、トレース メッセージを書き込む方法を示します。

**2 を一覧表示します。Debug クラスを使用してクライアント側のトレース メッセージを書き込んでいます。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

Web Development Helper メッセージが表示されます、LastName プロパティの値が Doe の場合は、"ユーザー名: Doe"(デバッグが有効であると仮定)、スクリプトのコンソールのコマンド ウィンドウでします。 Web Development Helper には、トレース情報を書き出しましたまたは JSON オブジェクトのコンテンツを表示するために使用できるページに、最上位 debugService オブジェクトも追加します。 リスト 3. debugService クラスのトレース関数の使用例を示します。

**3 を一覧表示します。Web Development Helper debugService クラスを使用して、トレース メッセージを作成します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

DebugService クラスの優れた機能は、デバッグが簡単に Web Development Helper を実行するときに、トレース データを常にアクセスする Internet Explorer で有効になっている場合でも動作することです。 ツールは、ページのデバッグに使用されていない、ときにトレース ステートメントは無視されますので window.debugService への呼び出しは false を返します。

DebugService クラスは、Web Development Helper のインスペクター ウィンドウを使用して表示する JSON オブジェクトのデータにもできます。 リスト 4 は、個人データを含む単純な JSON オブジェクトを作成します。 呼び出しが行われたオブジェクトが作成されると、クラスの debugService 視覚的に検査する JSON オブジェクトを許可する関数を調査します。

**4 を一覧表示します。DebugService.inspect 関数を使用して、JSON オブジェクトのデータを表示します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

イミディ エイト ウィンドウなど、ページには、GetPerson() 関数を呼び出すと、図 5 に示すように表示されるオブジェクトのインスペクター ダイアログ ウィンドウが発生します。 強調表示し、オブジェクト内のプロパティが動的に変更する値のテキスト ボックスに表示される値を変更して、[更新] リンクをクリックします。 オブジェクト インスペクターを使用すると、JSON オブジェクトのデータを表示し、別の値をプロパティに適用することで実験を簡単になります。

*エラーのデバッグ*

トレースのデータと JSON オブジェクトを表示できるように、だけでなく、ページにエラーのデバッグに Web Development helper も役立ちます。 エラーが発生した場合は促されます次のコード行に進むか、スクリプトをデバッグします (図 6 参照)。 スクリプト内での問題が簡単に識別できるように、完全な呼び出しが行番号とスタック スクリプト エラー ダイアログ ウィンドウの表示します。


[![オブジェクトのインスペクター ウィンドウを使用して、JSON オブジェクトを表示します。](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**図 5**: JSON オブジェクトを表示するオブジェクトのインスペクター ウィンドウを使用します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))。


デバッグ オプションを選択すると、変数の値を表示、その他の JSON オブジェクトを記述する Web Development Helper イミディ エイト ウィンドウで直接スクリプト ステートメントを実行できます。 エラーをトリガーしたのと同じ操作をもう一度実行して、Visual Studio 2008 がコンピューター上で使用、デバッグ セッションを開始し、前のセクションで説明されている、1 行ずつコードをステップスルーように促されます。


[![Web Development Helper のスクリプトのエラー ダイアログ](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**図 6**: Web Development Helper のスクリプトのエラー ダイアログ ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))。


*要求と応答メッセージの検査*

ASP.NET AJAX ページのデバッグ中にページとサーバー間で送信される要求と応答のメッセージを表示すると便利です。 メッセージ内のコンテンツの表示を使用すると、メッセージのサイズと適切なデータが渡されるかどうかを参照してください。 Web Development Helper を未加工のテキストとして、またはより読みやすい形式でデータを表示するが容易にする優れた HTTP メッセージ ロガー機能を提供します。

ASP.NET AJAX 要求と応答メッセージを表示するには、Web Development Helper メニューから HTTP のログを有効に HTTP を選択して、HTTP のロガーを有効にする必要があります。 有効にすると、HTTP: HTTP ログの表示を選択してアクセスできる HTTP ログ ビューアーの現在のページから送信されたすべてのメッセージを表示できます。

各要求/応答メッセージで送信された未加工のテキストを表示することは確かに便利ですが、Web Development Helper でオプションを) より多くのグラフィック形式でメッセージ データを表示しやすいです。 HTTP ログ記録を有効になっているし、メッセージが記録された、HTTP ログ ビューアーでメッセージをダブルクリックして、メッセージ データを表示できます。 これを行うと、コンテンツ使用すると、実際のメッセージと同様に、メッセージに関連付けられているすべてのヘッダーを表示できます。 図 7 では、要求メッセージと応答メッセージの HTTP ログ ビューアー ウィンドウに表示の例を示します。


[![HTTP ログ ビューアーを使用して、要求と応答のメッセージ データを表示します。](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**図 7**: HTTP ログ ビューアーを使用して、要求と応答のメッセージ データを表示します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))。


HTTP ログの表示は自動的に JSON オブジェクトを解析し、迅速かつ簡単に、オブジェクトのプロパティのデータを表示することがツリー ビューを使用してそれらを表示します。 UpdatePanel は、ASP.NET AJAX ページで使用されている、ときに、ビューアーは、図 8 に示すように個々 のパーツにメッセージの各部分を中断します。 これは、生のメッセージ データの表示と比較して、メッセージの新機能を理解しやすくする優れた機能です。


[![HTTP ログ ビューアーを使用して表示 UpdatePanel 応答メッセージ。](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**図 8**: HTTP ログ ビューアーを使用して、UpdatePanel の応答メッセージの表示。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))。


Web Development Helper だけでなく、要求と応答のメッセージを表示するために使用できるその他のいくつかのツールがあります。 別の適切なオプションは、Fiddler を無料で利用可能な[ http://www.fiddlertool.com](http://www.fiddlertool.com)します。 Fiddler をここでは触れていませんが、また、適したオプションではメッセージ ヘッダーおよびデータを徹底的に検査する必要がある場合。

## <a name="debugging-with-firefox-and-firebug"></a>Firefox と Firebug を使用したデバッグ

Internet Explorer は、最も広く使用されているブラウザーでは引き続き、Firefox などの他のブラウザーは非常に一般的になったしより多く使用されています。 その結果、確認と Firefox と Internet Explorer アプリケーションが正しく動作できるようにする、ASP.NET AJAX ページをデバッグします。 Firefox は、デバッグ用 Visual Studio 2008 に直接関連付けることはできません、拡張機能ページのデバッグに使用できる Firebug と呼ばれることがあります。 Firebug に移動して無料でダウンロードできる[ http://www.getfirebug.com](http://www.getfirebug.com)します。

Firebug は、1 行ずつコードをステップ実行、ページ内で使用されるすべてのスクリプトへのアクセス、DOM の構造を表示、CSS スタイルおよび発生する追跡イベントをページに表示に使用できるフル機能のデバッグ環境を提供します。 インストールされると、Firebug は Firefox メニューからツール Firebug オープン Firebug を選択してアクセスできます。 Firebug は、Web Development Helper のように、スタンドアロン アプリケーションとして使用することもできますが、ブラウザーで直接使用されます。

Firebug を実行すると、かどうか、スクリプトがページに埋め込まれているかどうかのブレークポイントは任意の JavaScript ファイルの行設定できます。 ブレークポイントを設定するには、最初に Firefox でデバッグするには、適切なページを読み込みます。 ページが読み込まれると、Firebug のスクリプトのドロップダウン リストからデバッグするスクリプトを選択します。 ページで使用されるすべてのスクリプトが表示されます。 ブレークポイントを設定するには、ブレークポイントが Visual Studio 2008 で行うようにする必要がありますを移動する必要があります、行の Firebug の灰色のトレイ 領域でクリックします。

Firebug でブレークポイントが設定されると、ボタンをクリックするか、onLoad イベントをトリガーする、ブラウザーの更新などをデバッグする必要があるスクリプトを実行するために必要なアクションを実行できます。 実行は、ブレークポイントを含む行に自動的に停止されます。 図 9 は、Firebug で起動されているブレークポイントの例を示します。


[![Firebug 内のブレークポイントを処理します。](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**図 9**: Firebug 内のブレークポイントを処理します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))。


ブレークポイントがヒットするにステップ インにステップ オーバーまたはコードは、矢印ボタンを使用してステップ アウトします。 コードをステップ実行するスクリプト変数の値を表示し、オブジェクトへのドリル ダウンすることができます、デバッガーの右側の部分に表示されます。 Firebug では、デバッグ中の現在の行原因となったスクリプトの実行手順を表示する呼び出し履歴ドロップダウン リストも含まれています。

Firebug では、別のスクリプト ステートメントをテストし、変数を評価し、トレース出力を表示するために使用するコンソール ウィンドウも含まれています。 これは、Firebug ウィンドウの上部にある [コンソール] タブをクリックしてアクセスします。 デバッグ中のページも「検査できる」を 検査 タブをクリックして、DOM の構造と内容を参照してください。簡単に ページで、要素を使用する場所を表示、ページの適切な部分のインスペクター ウィンドウに表示されるさまざまな DOM 要素上にマウスがハイライトされます。 指定した要素に関連付けられた属性値を変更するを試してみたり、異なる幅やスタイルなどの要素に適用するには、「ライブ」です。 これは、常に簡単な方法の変更に影響がページを表示するには、ソース コード エディターと Firefox ブラウザー間で切り替える手間が 優れた機能です。

図 10 では、テキスト ボックス ページで txtCountry という名前を検索する DOM インスペクターを使用する例を示します。 Firebug インスペクターは、マウスの動き、ボタンのクリック、その他の追跡などに発生するイベントと同様に、ページで使用される CSS スタイルを表示するのにも使用できます。


[![Firebug の DOM の検査を使用します。](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**図 10**: を使用して Firebug の DOM の検査します。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))。


Firebug は、すぐに Firefox と、ページ内のさまざまな要素を検査するための優れたツールで直接ページをデバッグする軽量な方法を提供します。

## <a name="debugging-support-in-aspnet-ajax"></a>ASP.NET AJAX でのデバッグのサポート

ASP.NET AJAX ライブラリには、web ページに AJAX 機能を追加するプロセスを簡略化に使用できるさまざまなクラスが含まれています。 これらのクラスを使用して、ページ内の要素を検索し、操作、新しいコントロールを追加、Web サービスの呼び出しおよびでもイベントを処理することができます。 ASP.NET AJAX ライブラリには、ページのデバッグのプロセスを強化するために使用できるクラスも含まれています。 このセクションでは、Sys.Debug クラスを紹介し、アプリケーションでの使用方法を参照してください。

*Sys.Debug クラスを使用します。*

トレース出力の書き込み、コードのアサーションを実行する、強制コードをデバッグできるように失敗など、さまざまな機能を実行する Sys.Debug クラス (Sys 名前空間にある JavaScript クラス) を使用できます。 これは広く使用 (既定で C:\Program files \microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 にインストールされている)、ASP.NET AJAX ライブラリのデバッグ ファイルに条件付きテスト (を実行するにはアサーションと呼ばれます) 関数、オブジェクトに必要なデータが含まれているとトレース ステートメントを記述する、パラメーターは正しくやり取りすることを確認します。

Sys.Debug クラスは、トレース、コードのアサーションまたは表 1 に示すように、エラーを処理するために使用できるさまざまな機能を公開します。

**表 1。Sys.Debug クラスの関数。**

| **関数名** | **説明** |
| --- | --- |
| assert (条件、メッセージ、displayCaller) | 条件のパラメーターが true であることをアサートします。 テスト対象の条件が false の場合は、メッセージ ボックスを使用してメッセージ パラメーターの値を表示します。 DisplayCaller パラメーターが true の場合、メソッドには、呼び出し元に関する情報も表示されます。 |
| clearTrace() | 操作のトレースからのステートメントの出力を消去します。 |
| fail(message) | プログラムを実行を停止し、デバッガーを中断させます。 失敗の理由を指定するメッセージのパラメーターを使用できます。 |
| trace(message) | メッセージ パラメーターは、トレース出力に書き込みます。 |
| traceDump(object, name) | 読みやすい形式でオブジェクトのデータを出力します。 トレース ダンプのラベルを指定する name パラメーターを使用できます。 ダンプされるオブジェクト内のすべてのサブ オブジェクトは、既定で書き込まれます。 |

ASP.NET で使用できるトレース機能と同様に、クライアント側のトレースを使用できます。 これにより、さまざまなメッセージをアプリケーションのフローを中断することがなく簡単に見ることができます。 リスト 5. Sys.Debug.trace 関数を使用してトレース ログに書き込むの例を示します。 この関数は、単にパラメーターとして書き込む必要があるメッセージを受け取ります。

**5 を一覧表示します。Sys.Debug.trace 関数を使用します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

リスト 5 に示すようにコードを実行する場合は、ページのトレース出力が表示されなくなります。 表示する唯一の方法では、Visual Studio .NET、Web Development Helper または Firebug で使用可能なコンソール ウィンドウを使用します。 トレース出力をページに表示する場合は、TextArea タグを追加し、次に示すように TraceConsole の id に付与する必要があります。


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

ページで Sys.Debug.trace ステートメントは TraceConsole テキスト領域に書き込まれます。

JSON オブジェクト内のデータを表示する場合は、Sys.Debug クラスの traceDump 関数を使用できます。 この関数は、トレースにダンプする必要がありますオブジェクトおよびトレース出力内のオブジェクトを識別するために使用できる名前を含む 2 つのパラメーターを受け取ります。 リスト 6. traceDump 関数の使用例を示します。

**6 を一覧表示します。Sys.Debug.traceDump 関数を使用します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

図 11 では、Sys.Debug.traceDump 関数の呼び出しからの出力を示します。 Person オブジェクトのデータを書き出すだけでなくも書き込まれるアドレス サブ-オブジェクトのデータに注目してください。

トレース、に加えて Sys.Debug クラスをコードのアサーションを実行する使用もできます。 アサーションは、アプリケーションの実行中に、特定の条件を満たしていることをテストに使用されます。 ASP.NET AJAX ライブラリ スクリプトのデバッグ バージョンでは、いくつか含まれて、さまざまな条件をテストするステートメントをアサートします。

リスト 7. Sys.Debug.assert 関数を使用して、条件をテストする例を示します。 コードでは、Person オブジェクトを更新する前に、アドレス オブジェクトが null かどうかをテストします。


[![Sys.Debug.traceDump 関数の出力。](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**図 11**: Sys.Debug.traceDump 関数の出力。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))。


**7 を一覧表示します。Debug.assert 関数を使用します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

アサーションが false で、呼び出し元に関する情報を表示するかどうかを返す場合に表示するメッセージを評価する条件を含む 3 つのパラメーターが渡されます。 アサーションが失敗した場合、メッセージが表示されます呼び出し元情報と 3 番目のパラメーターが true だった場合。 リスト 7 に示すように、アサーションが失敗した場合に表示されるエラー ダイアログの例を図 12 に示します。

対応する最終的な関数では、Sys.Debug.fail です。 スクリプトで、特定の行で失敗するコードを強制するときは、JavaScript アプリケーションで通常使用されるデバッガー ステートメントではなく Sys.Debug.fail 呼び出しを追加できます。 Sys.Debug.fail 関数は、次に示すように、失敗の理由を表す 1 つの文字列パラメーターを受け取ります。


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Sys.Debug.assert のエラー メッセージ。](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**図 12**: A Sys.Debug.assert エラー メッセージ。  ([フルサイズの画像を表示する をクリックします](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))。


Sys.Debug.fail ステートメントがスクリプトの実行中に発生したし、メッセージ パラメーターの値は、Visual Studio 2008 などのデバッグ アプリケーションのコンソールに表示されますが、アプリケーションをデバッグすることを求められます。 場合この非常に便利ですが、1 つのケースでは、インライン スクリプトに Visual Studio 2008 でブレークポイントを設定することはできませんが、変数の値を確認できるように、特定の行で停止するコードを希望する際にです。

*ScriptManager コントロールの ScriptMode プロパティについてください。*

ASP.NET AJAX ライブラリに含まれるデバッグし、既定で C:\Program files \microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 にインストールされているスクリプトのバージョンをリリースします。 デバッグ スクリプトは、適切に書式設定された、読みやすいとがいくつかの Sys.Debug.assert 呼び出し内にあちこちにリリース スクリプトが除去空白があるし、全体的なサイズを最小限に抑える Sys.Debug クラスを多用します。

ASP.NET AJAX ページに追加された ScriptManager コントロールは、ライブラリ、スクリプトを読み込むのバージョンを判断する web.config の compilation 要素の debug 属性を読み取ります。 ただし場合を制御できますデバッグまたはリリース スクリプトが読み込まれた (ライブラリのスクリプトまたは独自のカスタム スクリプト) ScriptMode プロパティを変更します。 ScriptMode では、メンバーが、自動、デバッグ、リリース、および継承 ScriptMode 列挙体を受け入れます。

ScriptMode 既定値は auto は、scriptmanager コントロールでは、web.config の debug 属性を確認します。デバッグが false の場合、scriptmanager コントロールは ASP.NET AJAX ライブラリ スクリプトのリリース バージョンを読み込みます。 デバッグが true の場合は、スクリプトのデバッグ バージョンは読み込まれます。 リリースまたはデバッグ ScriptMode プロパティを変更すると、web.config の debug 属性がどのような値に関係なく、適切なスクリプトを読み込むため、scriptmanager コントロールが実行されます。リスト 8. の ScriptManager コントロールを使用して、ASP.NET AJAX ライブラリからデバッグ スクリプトを読み込む例を示します。

**8 を一覧表示します。Scriptmanager コントロールを使用してデバッグ スクリプトを読み込んで**します。


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

リスト 9 に示すように、ScriptReference コンポーネントと共に、ScriptManager のスクリプト プロパティを使用して、独自のカスタム スクリプトの別のバージョン (デバッグまたはリリース) を読み込むこともできます。

**9 を一覧表示します。Scriptmanager コントロールを使用してカスタム スクリプトを読み込んでいます。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> ScriptReference のコンポーネントを使用してカスタム スクリプトを読み込んでいる場合は、スクリプトには、スクリプトの下部にある次のコードを追加することで読み込みが完了したら、scriptmanager コントロールに通知する必要があります。


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

リスト 9 に示すコードでは、Person.debug.js Person.js ではなく、検索は自動的にようになって、Person のスクリプトのデバッグ バージョンを探すように、scriptmanager コントロールに指示します。 場合は、Person.debug.js ファイルでは、エラーが発生しますが見つかりません。

デバッグまたは ScriptManager コントロールに設定する ScriptMode プロパティの値に基づいて、リリース バージョンのカスタムのスクリプトを読み込む場合は、継承に ScriptReference コントロールの ScriptMode プロパティを設定できます。 これにより読み込まれるカスタム スクリプトの適切なバージョンを一覧表示する 10 に示すように、ScriptManager の ScriptMode プロパティに基づいています。 ScriptManager コントロールの ScriptMode プロパティは、デバッグに設定されているため、Person.debug.js スクリプトが読み込まれ、ページで使用されます。

**10 を一覧表示します。カスタム スクリプトのため、ScriptManager の ScriptMode を継承しています。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

ScriptMode プロパティを適切を使用してより簡単にアプリケーションをデバッグし、全体的なプロセスを簡略化できます。 ASP.NET AJAX ライブラリのリリースのスクリプトでは、ステップ実行し、コードの書式設定が削除されたデバッグ スクリプトが具体的には、デバッグ用に書式設定中にするための読み取りではなく困難です。

## <a name="conclusion"></a>まとめ

マイクロソフトの ASP.NET AJAX テクノロジは、エンドユーザーの全体的なエクスペリエンスを強化するため、AJAX 対応のアプリケーションを構築するための強固な基盤を提供します。 ただし、として、プログラミング技術とバグおよびその他のアプリケーションの問題が確実に生じます。 さまざまなデバッグ オプションについて知っておくより安定した製品で、多くの時間と結果が保存できます。

この記事では、Visual Studio 2008、Web Development Helper firebug を使用して Internet Explorer を含め、ASP.NET AJAX ページをデバッグするためのいくつかの異なる手法が導入されたしました。 これらのツールは、変数のデータへのアクセス、コード行を順を追っておよびトレース ステートメントを表示するため、全体的なデバッグ プロセスを簡略化できます。 説明したさまざまなデバッグ ツール、だけでなくもし説明しました、ASP.NET AJAX ライブラリの Sys.Debug クラスを使用して、アプリケーションでどのように ScriptManager クラスを使用して読み込む方法をデバッグまたはリリース バージョンのスクリプト。

## <a name="bio"></a>自己紹介

Dan Wahlin (Microsoft Most Valuable Professional ASP.NET と XML Web サービス) は、技術トレーニングをインターフェイスで .NET の開発インストラクターとアーキテクチャ コンサルタント ([www.interfacett.com)](http://www.interfacett.com)します。 Dan は ASP.NET 開発者 Web サイトの XML を設立 ([www.XMLforASP.NET](http://www.XMLforASP.NET))、INETA の講演者の局に、いくつかのカンファレンスで講演しています。 Dan は、Professional Windows DNA (Wrox)、ASP.NET を共同執筆: ヒント、チュートリアルとコード (Sam)、ASP.NET 1.1 Insider ソリューション、Professional ASP.NET 2.0 AJAX (Wrox)、ASP.NET 2.0 の MVP のハッキングおよび ASP.NET 開発者向け (Sam) の XML を作成します。 コード、記事や書籍を書き込んで彼がいない、ときに書き込み、音楽の録音およびゴルフと彼の妻と子供のバスケット ボールの再生 Dan を楽しんでいます。

1997 年からマイクロソフトの Web テクノロジで働いてあり myKB.com プレジデント、Scott Cate ([www.myKB.com](http://www.myKB.com)) ベースのナレッジ ベースのソフトウェア ソリューションに重点を置いてアプリケーションを ASP.NET の記述を専門としています。 Scott は時に電子メールが接続可能[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)またはで彼のブログ[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-web-services.md)
