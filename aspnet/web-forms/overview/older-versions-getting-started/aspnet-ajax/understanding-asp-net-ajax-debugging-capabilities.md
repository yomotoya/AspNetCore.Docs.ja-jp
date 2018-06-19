---
uid: web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
title: ASP.NET AJAX のデバッグ機能を理解する |Microsoft ドキュメント
author: scottcate
description: コードをデバッグする機能は、スキルを使用しているテクノロジに関係なく、備品においてすべての開発者に設定することです。 多くの開発者がいるときにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/28/2008
ms.topic: article
ms.assetid: 7f9380c6-19f7-4c82-a019-916ec6dffc9c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/aspnet-ajax/understanding-asp-net-ajax-debugging-capabilities
msc.type: authoredcontent
ms.openlocfilehash: f082e2206f5e691579670e42634f30b57e3b3593
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890932"
---
<a name="understanding-aspnet-ajax-debugging-capabilities"></a>ASP.NET AJAX のデバッグ機能を理解します。
====================
によって[Scott カテゴリ](https://github.com/scottcate)

[PDF をダウンロードします。](http://download.microsoft.com/download/C/1/9/C19A3451-1D14-477C-B703-54EF22E197EE/AJAX_tutorial06_Debugging_MS_Ajax_Applications_cs.pdf)

> コードをデバッグする機能は、スキルを使用しているテクノロジに関係なく、備品においてすべての開発者に設定することです。 多くの開発者は使い慣れた Visual Studio .NET または Web Developer Express VB.NET や c# のコードを使用する ASP.NET アプリケーションをデバッグするが、対応することは、JavaScript などのクライアント側のコードをデバッグで非常に役立ちますはも存在しません。 同じ種類の .NET アプリケーションのデバッグに使用される手法は、AJAX 対応のアプリケーションおよびより具体的には ASP.NET AJAX アプリケーションにも適用できます。


## <a name="debugging-aspnet-ajax-applications"></a>ASP.NET AJAX アプリケーションのデバッグ

Dan Wahlin

コードをデバッグする機能は、スキルを使用しているテクノロジに関係なく、備品においてすべての開発者に設定することです。 使用可能なさまざまなデバッグ オプションを理解することに保存できること、膨大な時間プロジェクトやでも通常数分頭痛の原因で言うまでもなくなります。 多くの開発者は使い慣れた Visual Studio .NET または Web Developer Express VB.NET や c# のコードを使用する ASP.NET アプリケーションをデバッグするが、対応することは、JavaScript などのクライアント側のコードをデバッグで非常に役立ちますはも存在しません。 同じ種類の .NET アプリケーションのデバッグに使用される手法は、AJAX 対応のアプリケーションおよびより具体的には ASP.NET AJAX アプリケーションにも適用できます。

この記事では、Visual Studio 2008 とその他のいくつかのツールを使用して、バグ、およびその他の問題をすばやく検索する ASP.NET AJAX アプリケーションをデバッグする方法が表示されます。 Internet Explorer 6 以降のデバッグ、Visual Studio 2008 とスクリプト エクスプ ローラーを使用してコードをステップ実行すると、Web 開発ヘルパーなどのツールを使用して有効にする方法については、この説明が含まれます。 Firefox では、拡張機能が Firebug を直接その他の任意のツールがないブラウザーで JavaScript コードをステップ実行することができますという名前を使用して ASP.NET AJAX アプリケーションをデバッグする方法も学習します。 最後に、紹介トレースとアサーションのコード ステートメントなどのさまざまなデバッグ タスクに役立つ、ASP.NET AJAX ライブラリ内のクラスにします。

Internet Explorer で表示したページをデバッグしようとする前に、デバッグに対して有効にするために実行する必要があります、いくつかの基本的な手順があります。 作業を開始するために実行する必要があるいくつかの基本的なセットアップ要件を見てをみましょう。

## <a name="configuring-internet-explorer-for-debugging"></a>デバッグ用の Internet Explorer の構成

ほとんどの人は Internet Explorer で表示する web サイトで発生した JavaScript の問題が表示される必要のないです。 実際には、平均的なユーザーそれに気づくエラー メッセージが表示されていた場合の対処方法。 結果として、デバッグ オプションはブラウザーで既定でオフにします。 ただし、デバッグを有効にし、新しい AJAX アプリケーションを開発する際に使用するように非常に簡単ですが。

デバッグ機能を有効にするには、Internet Explorer のメニューの インターネット オプションをツールに移動し、詳細設定 タブを選択します。参照セクション内で、次の項目がチェックされていないことを確認します。

- スクリプトのデバッグ (Internet Explorer) を無効にします。
- スクリプトのデバッグを (その他) を無効にします。

必須ではない場合は、ページを一見してすぐには、JavaScript エラーとよいでしょうアプリケーションをデバッグしようとしています。 「スクリプト エラーごとに、通知を表示する」チェック ボックスをオンにメッセージ ボックスに表示されるすべてのエラーを強制することができます。 これは、アプリケーションを開発しているときにオンにする便利なオプションですが、JavaScript のエラーが発生する可能性は非常に適しているため他の web サイトをだけ適ったしている場合は、面倒な場合すぐにことができます。

図 1 はどのような Internet Explorer の詳細 ダイアログは、適切に構成されているデバッグ後になります。


[![デバッグの Internet Explorer を構成します。](understanding-asp-net-ajax-debugging-capabilities/_static/image2.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image1.png)

**図 1**: デバッグの Internet Explorer を構成します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image3.png))


デバッグがオンにする、スクリプト デバッガーをという名前の表示 メニューに表示される新しいメニュー項目が表示されます。 次のステートメントで開くと中断を含む使用可能な 2 つのオプションがあります。 開くが選択されているときに Visual Studio 2008 (Visual Web Developer Express 使用できることもデバッグするために注意してください) でページをデバッグする求められます。 Visual Studio .NET は現在実行されている場合は、そのインスタンスを使用する新しいインスタンスを作成するかを選択できます。 次のステートメントで中断が選択されている場合に JavaScript コードを実行すると、ページのデバッグを求められます。 ページのオンロード イベント内の JavaScript コードが実行する場合は、デバッグ セッションをトリガーするページを更新することができます。 ボタンをクリックした後、JavaScript コードが実行される場合は、ボタンがクリックされた後にすぐに、デバッガーは実行されます。

> *> [!NOTE] Windows Vista ユーザー アクセス制御 (UAC) が有効になっているを実行している Visual Studio 2008 が設定を管理者として実行する必要がある場合は、アタッチするメッセージが表示されたら、プロセスにアタッチする Visual Studio は失敗します。この問題を回避するには、最初に、Visual Studio を起動し、そのインスタンスを使用してデバッグします。*


次のセクションでは、Visual Studio 2008 内から直接 ASP.NET AJAX ページをデバッグする方法をデモンストレーションします、Internet Explorer スクリプト デバッガー オプションを使用してページが既に開いているより詳細に調査したいときが役立ちます。

## <a name="debugging-with-visual-studio-2008"></a>Visual Studio 2008 でのデバッグ

Visual Studio 2008 では、世界中の開発者は、.NET アプリケーションをデバッグする日常的に依存しているデバッグ機能を提供します。 組み込みのデバッガーでは、コード、オブジェクト データを特定の変数のウォッチ コール スタックと多くの監視ビューのステップ実行することができます。 VB.NET や c# のコードをデバッグするには、だけでなく、デバッガー ASP.NET AJAX アプリケーションのデバッグに役立つとも JavaScript コード行をステップ実行することがあります。 Visual Studio 2008 を使用してアプリケーションをデバッグするプロセス全体で discourse を提供するのではなく、クライアント側スクリプト ファイルをデバッグするために使用される技法にフォーカスを次の詳細。

Visual Studio 2008 でのページをデバッグするプロセスは、いくつかの方法で起動できます。 最初に、前のセクションに記載されている Internet Explorer スクリプト デバッガー オプションを使用することができます。 これは、ブラウザーにページがまだ読み込まれていると、デバッグを開始するには機能します。 .Aspx ページで、ソリューション エクスプ ローラーで右クリックし、スタート ページとして設定メニューから選択することができます。 ASP.NET ページをデバッグするのに慣れている場合、おそらくこれが完了したらする前にします。 F5 キーを押すと、ページをデバッグできます。 ただし、一般にブレークポイントを設定する、任意の場所間たい VB.NET や c# のコードでは常に JavaScript の場合と、次が表示されます。

*外部スクリプトと埋め込み*

Visual Studio 2008 デバッガーには、外部の JavaScript ファイルとは異なるページに埋め込まれた JavaScript が扱われます。 外部スクリプト ファイル、ファイルを開くでき、選択した任意の行にブレークポイントを設定できます。 コード エディター ウィンドウの左側の灰色のトレイ領域内をクリックして、ブレークポイントを設定できます。 JavaScript がページを使用して、直接に埋め込まれたとき、`<script>`タグ、灰色のトレイ をクリックしてブレークポイントの設定オプションはされていません。 埋め込まれたスクリプトの行にブレークポイントを設定しようとすると、「これは、ブレークポイントの有効な場所」を示す警告が発生します。

この問題を回避するには、コード、外部の .js ファイルを移動しの src 属性を使用して参照することによって、&lt;スクリプト&gt;タグ。


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample1.html)]

場合、外部ファイルに、コードを移動することができませんまたはよりも多くの作業が必要ですがすべきですか。 エディターを使用してブレークポイントを設定することはできません、中には、デバッグを開始するコードに直接デバッガー ステートメントを追加することができます。 ASP.NET AJAX ライブラリに使用可能な Sys.Debug クラスを使用すると、強制的に起動するのに、デバッグをできますも。 この記事で後述 Sys.Debug クラスについて学習します。

使用例、 `debugger` 1 の一覧にキーワードを示します。 この例では、デバッガーが update 関数の呼び出しが行われる前に、右中断は強制的します。

**1 を一覧表示します。デバッガーのキーワードを使用して、強制的に、Visual Studio .NET デバッガーが中断します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample2.js)]

Debugger ステートメントに達すると、Visual Studio .NET を使用して、ページをデバッグするように求められます、コードのステップ実行を開始できます。 これを行うと、では] ページで使用する ASP.NET AJAX ライブラリ スクリプト ファイルへのアクセスに問題が発生する可能性があります中に見て Visual Studio を使用します。NET の [スクリプト エクスプ ローラー。

## <a name="using-visual-studio-net-windows-to-debug"></a>Visual Studio .NET の Windows を使用してデバッグするには

デバッグ セッションが開始される、既定 F11 キーを使用してコードを用いてを開始してに示すように、エラー ダイアログが表示される可能性があると ページで使用されるすべてのスクリプト ファイルが開かれていて、デバッグに使用できる場合を除き、図 2 を参照してください。


[![エラー ダイアログがソース コードが使用できない場合のデバッグを表示します。](understanding-asp-net-ajax-debugging-capabilities/_static/image5.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image4.png)

**図 2**: エラー ダイアログのソース コードが使用できない場合のデバッグを表示します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image6.png))


Visual Studio .NET に存在しないためことを確認して、ページによって参照されるスクリプトの一部のソース コードを取得する方法は、このダイアログ ボックスが表示されます。 これと手間が非常になるには、最初は、単純な解決策です。 デバッグ セッションを開始し、ブレークポイントにヒットした、デバッグ Windows スクリプト エクスプ ローラー ウィンドウ、Visual Studio 2008 メニューに移動しか、Ctrl + Alt + N ホット キーを使用します。

> *> [!NOTE] 表示されているスクリプト エクスプ ローラーのメニューができない場合は、ツールに進みます**カスタマイズ* *Visual Studio .NET メニューのコマンド。[カテゴリ] デバッグ エントリを見つけて利用可能なメニューのすべてのエントリを表示する] をクリックします。コマンドの一覧で [スクリプト エクスプ ローラーまで下にスクロールし、デバッグ ドラッグ* *Windows メニューで既に説明しました。これを行うスクリプト エクスプ ローラーのメニュー項目を使用できるように Visual Studio .NET を実行するたびにします。*


ページで使用されるすべてのスクリプトを表示し、コード エディターで開くことは、スクリプト エクスプ ローラーを使用できます。 スクリプト エクスプ ローラーが開いていると、現在デバッグ中のコード エディター ウィンドウで開く、.aspx ページをダブルクリックします。 スクリプト エクスプ ローラーで示すように、他のスクリプトのすべての同じアクションを実行します。 後のすることができます、コード ウィンドウで開いているすべてのスクリプトは F11 キーを押して (およびその他のデバッグ ホット キーを使用して) コードをステップ実行します。 図 3 は、スクリプト エクスプ ローラーの例を示します。 2 つのカスタム スクリプトと ASP.NET AJAX ScriptManager がページに動的に挿入する 2 つのスクリプトだけでなく、現在デバッグ対象のファイル (Demo.aspx) が一覧表示します。


[![スクリプト エクスプ ローラーでは、ページで使用されるスクリプトに簡単にアクセスを提供します。](understanding-asp-net-ajax-debugging-capabilities/_static/image8.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image7.png)

**図 3**です。 スクリプト エクスプ ローラーでは、ページで使用されるスクリプトに簡単にアクセスを提供します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image9.png))


その他いくつかの windows は、ページ内にコードをステップ実行するように、有用な情報を提供するも使用できます。 たとえば、[ローカル] ウィンドウを使用すると、ページでは、特定の変数または条件を評価し、出力を表示するのに、イミディ エイト ウィンドウで使用される別の変数の値を参照してください。 また、Sys.Debug.trace 関数に (この記事の後半で説明されます) または Internet Explorer の Debug.writeln 関数を使用して書き込むトレース ステートメントを表示するのには、出力ウィンドウを使用することができます。

デバッガーを使用してコードをステップ実行するように割り当てられている値を表示するコード内の変数にマウスを置くことができます。 ただし、スクリプト デバッガー場合によっては表示されません何も特定の JavaScript 変数にマウスを置くとします。 値を表示するには、ステートメントまたはコード エディター ウィンドウに表示し、マウス ポインターをしようとしている変数を選択します。 この手法は、すべての状況で動作しないが何度もことができます、[ローカル] ウィンドウなど、さまざまなデバッグ ウィンドウで検索することがなく、値を表示します。

ここで説明した機能の一部を示すビデオ チュートリアルで表示できる[ http://www.xmlforasp.net](http://www.xmlforasp.net)です。

## <a name="debugging-with-web-development-helper"></a>Web 開発ヘルパーとデバッグ

Visual Studio 2008 (と Visual Web Developer Express 2008) は非常に対応デバッグ ツールが、追加のオプションも使用できるより軽量であるがあります。 解放される最新のツールの 1 つは、Web 開発ヘルパーです。 Microsoft の Nikhil Kothari (Microsoft のキーの ASP.NET AJAX 設計者の 1 つ) は、HTTP 要求と応答メッセージを表示する簡単なデバッグからさまざまなタスクを実行できる優れたこのツールを書き込みました。 Web 開発ヘルパーをダウンロードできます[ http://projects.nikhilk.net/Projects/WebDevHelper.aspx](http://projects.nikhilk.net/Projects/WebDevHelper.aspx)です。

Web 開発ヘルパーを使用するが容易にする Internet Explorer の内部で直接使用できます。 Internet Explorer のメニューから Web 開発ヘルパーのツールを選択して開始します。 これは、HTTP の要求と応答メッセージのログ記録などのいくつかのタスクを実行するブラウザーのままにする必要はありませんので nice ブラウザーの下の部分で、ツールが開きます。 図 4 は、アクション内で Web 開発ヘルパーがどのようにを示します。


[![Web 開発のヘルパー](understanding-asp-net-ajax-debugging-capabilities/_static/image11.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image10.png)

**図 4**: Web 開発のヘルパー ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image12.png))


Web 開発ヘルパーを使用して、ツールのない行として Visual Studio 2008 でのコードをステップ実行します。 ただし、トレース出力を表示、簡単にスクリプトで変数を評価または探索を使用する JSON オブジェクトの内部では、データ。 ASP.NET AJAX ページおよびサーバーの間に渡されるデータを表示するため便利です。

Web 開発ヘルパーが Internet Explorer で開かれて、スクリプトを有効にするスクリプトのデバッグ メニューから選択して、Web 開発ヘルパー図 4 で前述したとおりスクリプトのデバッグを有効する必要があります。 これにより、インターセプト発生したエラー ページが実行されるとするツールです。 ページで出力されるトレース メッセージを簡単にアクセスできます。 トレース情報を表示またはページ内の異なる関数をテストするスクリプト コマンドを実行する、するには、Web 開発ヘルパー メニューからスクリプトを表示するスクリプトのコンソールを選択します。 これは、コマンド ウィンドウと簡単なイミディ エイト ウィンドウへのアクセスを提供します。

*トレース メッセージと JSON オブジェクトのデータを表示します。*

スクリプト コマンドを実行またはでも読み込みまたはページのさまざまな機能をテストに使用されるスクリプトを保存するのには、イミディ エイト ウィンドウを使用できます。 コマンド ウィンドウには、表示されているページ書き込まれたトレースまたはデバッグのメッセージが表示されます。 2 を一覧表示する Internet Explorer の Debug.writeln 関数を使用して、トレース メッセージを記述する方法を示しています。

**2 を一覧表示します。Debug クラスを使用してクライアント側のトレース メッセージを書き込んでいます。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample3.js)]

LastName プロパティ Doe の値が含まれている場合、Web 開発ヘルパーにメッセージが表示"人名: Doe"スクリプト本体のコマンド ウィンドウ (デバッグが有効になっていると仮定)。 Web 開発ヘルパーでは、トレース情報を書き込むまたは JSON オブジェクトのコンテンツの表示に使用できるページに、最上位 debugService オブジェクトも追加されます。 3 を一覧表示する、debugService クラスのトレース関数を使用する例を示します。

**3 を一覧表示します。Web 開発ヘルパーの debugService クラスを使用して、トレース メッセージを作成します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample4.js)]

DebugService クラスの優れた機能は、Internet Explorer Web 開発ヘルパーが実行されている場合にトレース データを常にアクセスするが容易では、デバッグが有効にされていない場合でも動作することです。 ツールは、ページのデバッグに使用されていない、ときに、window.debugService への呼び出しは false を返すためにトレース ステートメントが無視されます。

DebugService クラスでは、JSON オブジェクトのデータを Web 開発ヘルパーのインスペクター ウィンドウを使用して表示することもできます。 4 を一覧表示するには、ユーザー データを含む単純な JSON オブジェクトを作成します。 呼び出しが行われた、オブジェクトが作成されると、クラスの debugService 視覚的に検査する JSON オブジェクトを許可する関数を調査します。

**4 を一覧表示します。DebugService.inspect 関数を使用して、JSON オブジェクトのデータを表示します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample5.js)]

ページまたはイミディ エイト ウィンドウから GetPerson() 関数を呼び出すと、図 5 に示すように表示されるオブジェクトのインスペクター ダイアログ ウィンドウが発生します。 強調表示され、オブジェクト内のプロパティを動的に変更することができます値テキスト ボックスに表示される値を変更し、リンクをクリックし、更新します。 オブジェクト インスペクターを使用すると、JSON オブジェクトのデータを表示し、プロパティに異なる値を適用することで実験を簡単になります。

*エラーのデバッグ*

トレース データおよび表示する JSON オブジェクトを許可するだけでなく、ページにエラーのデバッグに Web 開発ヘルパーも役立ちます。 次のコード行に進むか、スクリプトをデバッグ求められますエラーが発生した場合 (図 6 を参照してください)。 スクリプト内に問題があるを簡単に識別できるように行番号だけでなく、完全な呼び出しがスタック スクリプト エラー ダイアログ ウィンドウの表示します。


[![オブジェクトのインスペクター ウィンドウを使用して、JSON オブジェクトを表示します。](understanding-asp-net-ajax-debugging-capabilities/_static/image14.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image13.png)

**図 5**: オブジェクトのインスペクター ウィンドウを使用して、JSON オブジェクトを表示します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image15.png))


デバッグ オプションを選択するには、イミディ エイト ウィンドウを表示する変数の値を JSON オブジェクトとその詳細を書き込む Web 開発ヘルパーで直接スクリプト ステートメントを実行することができます。 エラーを発生させた同じアクションが再実行すると、Visual Studio 2008 がコンピューター上で使用、デバッグ セッションの開始行前のセクションで説明したようにコードをステップスルーするように促されます。


[![Web 開発ヘルパーのスクリプトのエラー ダイアログ ボックス](understanding-asp-net-ajax-debugging-capabilities/_static/image17.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image16.png)

**図 6**: Web 開発ヘルパーのスクリプトのエラー ダイアログ ボックス ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image18.png))


*要求および応答メッセージの検査*

ASP.NET AJAX ページのデバッグ中にページとサーバー間で送信される要求と応答のメッセージを表示すると便利です。 メッセージ内でコンテンツを表示するには、メッセージのサイズと適切なデータが渡されるかどうかを参照してくださいすることができます。 Web 開発ヘルパーは、未加工のテキストとして、またはより読みやすい形式では、データを表示しやすく素晴らしい HTTP メッセージ ロガー機能を提供します。

ASP.NET AJAX 要求および応答メッセージを表示するには、Web 開発ヘルパー メニューから HTTP を有効にする HTTP ログ記録 を選択して HTTP ロガーを有効にする必要があります。 有効にすると、現在のページから送信されたすべてのメッセージが HTTP HTTP ログの表示 を選択してアクセス可能な HTTP ログ ビューアーで表示できます。

各要求/応答メッセージで送信された生のテキストの表示には確かに便利ですが、Web 開発ヘルパーでオプションを)、簡単に複数のグラフィカルな形式でメッセージ データを表示は多くの場合です。 HTTP ログ記録を有効になっているし、メッセージが記録された、HTTP ログ ビューアー内のメッセージをダブルクリックしてメッセージ データを表示できます。 これを行うようにメッセージだけでなく、実際のメッセージに関連付けられているすべてのヘッダーを表示するコンテンツ。 図 7 は、要求メッセージと応答メッセージの HTTP ログ ビューアー ウィンドウに表示の例を示します。


[![HTTP ログ ビューアーを使用して、要求と応答のメッセージ データを表示します。](understanding-asp-net-ajax-debugging-capabilities/_static/image20.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image19.png)

**図 7**: HTTP ログ ビューアーを使用して、要求と応答のメッセージ データを表示します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image21.png))


HTTP ログの表示は自動的に JSON オブジェクトを解析し、クイックかつ簡単に、オブジェクトのプロパティのデータを表示することをツリー ビューを使用してそれらを表示します。 UpdatePanel を ASP.NET AJAX ページで使用されている場合、ビューアーは、図 8 に示すように個々 の部分へのメッセージの各部分を中断します。 これは、優れた機能は未処理のメッセージ データの表示と比較してメッセージには、新機能を把握するより簡単です。


[![HTTP ログ ビューアーを使用して表示 UpdatePanel 応答メッセージ。](understanding-asp-net-ajax-debugging-capabilities/_static/image23.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image22.png)

**図 8**: HTTP ログ ビューアーを使用して、UpdatePanel 応答メッセージを表示します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image24.png))


Web 開発ヘルパーだけでなく、要求と応答のメッセージを表示するために使用できるその他のいくつかのツールがあります。 別の適切なオプションは、無料で利用できる Fiddler [ http://www.fiddlertool.com](http://www.fiddlertool.com)です。Fiddler はここでは説明されませんがも適して場合にメッセージ ヘッダーおよびデータを徹底的に検査する必要があります。

## <a name="debugging-with-firefox-and-firebug"></a>Firefox および Firebug でのデバッグ

Internet Explorer は、最も広く使用されているブラウザーでは引き続き、Firefox などの他のブラウザーが非常に一般的になりより多く使用されています。 その結果、表示をデバッグする ASP.NET AJAX ページ Firefox だけでなく Internet Explorer アプリケーションが正しく動作することを確認します。 Firefox は、デバッグ用 Visual Studio 2008 に直接関連付けることはできません、ですが、ページのデバッグに使用できる、Firebug と呼ばれる、拡張子が付きます。 移動して、firebug を無料でダウンロードできる[ http://www.getfirebug.com](http://www.getfirebug.com)です。

Firebug は、コード 1 行ずつステップ実行により、ページ内で使用されるすべてのスクリプトへのアクセス、DOM の構造を表示、CSS スタイルおよび発生した追跡イベントをページに表示するために使用するフル機能のデバッグ環境を提供します。 インストールされると、ツール、Firebug 開く Firebug メニューをクリックして、Firefox Firebug をアクセスできます。 Web 開発ヘルパーに渡しと同様に、Firebug はスタンドアロン アプリケーションとして使用することもできますが、ブラウザーで直接使用されます。

か、スクリプトがページに埋め込まれているかどうか、Firebug が実行されていると、JavaScript ファイルの任意の行にブレークポイントを設定することができます。 ブレークポイントを設定するには、まず Firefox でデバッグするには、適切なページにロードします。 ページが読み込まれると、Firebug のスクリプトのドロップダウン リストからデバッグするスクリプトを選択します。 ページで使用されるすべてのスクリプトが表示されます。 ブレークポイントを設定するには、ブレークポイントが Visual Studio 2008 の場合と同じようにする必要がありますを移動する必要があります、線上の Firebug の灰色のトレイ 領域でクリックします。

Firebug でブレークポイントが設定されると、デバッグを行うなど、ボタンをクリックするか、オンロード イベントをトリガーするブラウザーを更新する必要があるスクリプトを実行するために必要なアクションを実行できます。 実行はブレークポイントを含む行に自動的に停止します。 図 9 では、Firebug でトリガーされたブレークポイントの使用例を示します。


[![また、Firebug 内のブレークポイントを処理しています。](understanding-asp-net-ajax-debugging-capabilities/_static/image26.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image25.png)

**図 9**: Firebug 内のブレークポイントを処理します。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image27.png))


ブレークポイントに達するとにステップ イン、ステップ オーバーしたり、コードの矢印ボタンを使用してステップ アウトできます。 コードをステップ実行するスクリプト変数を値を表示し、オブジェクトへのドリル ダウンできるように、デバッガーの右側の部分に表示されます。 Firebug には、デバッグされている現在の行原因となったスクリプトの実行手順を表示する呼び出しスタックのドロップダウン リストも含まれています。

Firebug には、コンソール ウィンドウを別のスクリプト ステートメントのテスト、変数を評価し、トレース出力を表示するために使用することも含まれます。 Firebug ウィンドウの上部にある [コンソール] タブをクリックしてアクセスします。 デバッグ中のページも「検査できる」検査 タブをクリックして、DOM の構造と内容を表示します。したのと、ページの適切な部分のインスペクター ウィンドウに表示されているさまざまな DOM 要素でマウスを動かすは強調表示が容易に ページで、要素が使用されているを参照してください。 指定された要素に関連付けられた属性値を変更するには、異なる幅、スタイルなどを要素に適用することをテストするには、「ライブ」です。 これは、常に簡単な方法の変更の影響、ページを表示するには、ソース コード エディターと Firefox ブラウザーの間で切り替えるにはしなくて済むため便利な機能です。

図 10 では、DOM の検査を検索 ページで txtCountry をという名前のテキスト ボックスを使用する例を示します。 Firebug インスペクターは、ページだけでなく plus マウスの動きをボタンのクリックの追跡などに発生するイベントで使用される CSS スタイルを表示するのにも使用できます。


[![Firebug の DOM の検査を使用します。](understanding-asp-net-ajax-debugging-capabilities/_static/image29.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image28.png)

**図 10**: を使用して、Firebug の DOM の検査。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image30.png))


Firebug は、直接 Firefox だけでなく、ページ内の別の要素を検査するための優れたツールのページをすばやくデバッグ コンパクトな方法を提供します。

## <a name="debugging-support-in-aspnet-ajax"></a>ASP.NET AJAX でのデバッグのサポート

ASP.NET AJAX ライブラリには、web ページへの AJAX 機能の追加のプロセスを簡単に使用できる多くのさまざまなクラスが含まれています。 これらのクラスを使用して、ページ内の要素を検索し、それらの操作、新しいコントロールを追加、Web サービスの呼び出しおよびでもイベントを処理することができます。 ASP.NET AJAX ライブラリには、デバッグのページのプロセスを強化するために使用できるクラスも含まれています。 ここでは、Sys.Debug クラスを紹介し、アプリケーションでの使用方法を参照してください。

*Sys.Debug クラスを使用します。*

Sys.Debug クラス (Sys の名前空間にある JavaScript クラス) は、トレース出力の書き込み、アサーションのコードの実行、および強制の失敗はデバッグできるようにするためのコードを含むいくつかの異なる機能を実行するために使用します。 これによく使用されます (既定で C:\Program files \microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 にインストールされている)、ASP.NET AJAX ライブラリのデバッグ ファイルに条件付きのテスト (を実行するにはアサーションと呼ばれる) パラメーターは正しく関数、オブジェクトに、必要なデータが含まれていると、トレース ステートメントを記述することを確認してください。

Sys.Debug クラスでは、トレース、コードのアサーションや表 1 に示すように、エラーの処理に使用できるいくつかの異なる関数を公開します。

**表 1 です。Sys.Debug クラスの関数。**

| **関数名** | **説明** |
| --- | --- |
| assert (条件、メッセージ、displayCaller) | 条件のパラメーターが true であることをアサートします。 テストされた条件が false の場合は、メッセージ ボックスを使用してメッセージ パラメーターの値を表示します。 DisplayCaller パラメーターが true の場合、メソッドには、呼び出し元に関する情報も表示されます。 |
| clearTrace() | トレース操作からのステートメントの出力を消去します。 |
| fail(message) | プログラムを実行を停止し、デバッガーで中断させます。 失敗の理由を指定するメッセージのパラメーターを使用できます。 |
| trace(message) | メッセージ パラメーターをトレース出力に書き込みます。 |
| traceDump(object, name) | 読み取り可能な形式でオブジェクトのデータを出力します。 トレースのダンプのラベルを指定する name パラメーターを使用できます。 ダンプされるオブジェクト内のすべてのサブ オブジェクトは、既定でに書き出されます。 |

クライアント側のトレースは、ASP.NET で使用できるトレース機能とほぼ同じ方法で使用できます。 により、さまざまなメッセージをアプリケーションのフローを中断することがなく簡単に見ることができます。 5 を一覧表示する Sys.Debug.trace 関数を使用して、トレース ログに書き込むの例に示します。 この関数は、単にパラメーターとして出力するか、メッセージを受け取ります。

**5 を一覧表示します。Sys.Debug.trace 関数を使用します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample6.js)]

リスト 5 に示すようにコードを実行する場合、ページのトレース出力は表示されません。 Visual Studio .NET で Web 開発ヘルパーまたは Firebug で利用可能なコンソール ウィンドウを使用するには表示するしかありません。 ページのトレース出力を表示するようにする場合、TextArea タグを追加し、id TraceConsole の次に示すように必要があります。


[!code-html[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample7.html)]

ページで Sys.Debug.trace ステートメントは TraceConsole TextArea に書き込まれます。

JSON オブジェクト内に含まれるデータを表示する場合は、Sys.Debug クラスの traceDump 関数を使用できます。 この関数は、トレースをダンプする必要が、オブジェクトおよびトレースに出力オブジェクトの識別に使用できる名前を含む 2 つのパラメーターを受け取ります。 リスト 6 traceDump 関数を使用する例を示します。

**6 を一覧表示します。Sys.Debug.traceDump 関数を使用します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample8.js)]

図 11 は、Sys.Debug.traceDump 関数の呼び出しからの出力を示します。 に加えて、ユーザー オブジェクトのデータの書き込みもに書き込まれますアドレス sub-オブジェクトのデータに注目してください。

トレース、に加えて Sys.Debug クラスをコードのアサーションを実行する使用もできます。 アサーションは、アプリケーションの実行中に、特定の条件が満たされているテストに使用されます。 ASP.NET AJAX ライブラリ スクリプトのデバッグ バージョンでは、いくつか含まれて assert ステートメントをいくつかの条件をテストします。

7 を一覧表示する条件をテストする Sys.Debug.assert 関数を使用する例を示します。 コードでは、ユーザー オブジェクトを更新する前に、アドレス オブジェクトが null かどうかをテストします。


[![Sys.Debug.traceDump 関数の出力です。](understanding-asp-net-ajax-debugging-capabilities/_static/image32.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image31.png)

**図 11**: Sys.Debug.traceDump 関数の出力。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image33.png))


**7 を一覧表示します。Debug.assert 関数を使用します。**


[!code-javascript[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample9.js)]

アサーションが false で、呼び出し元に関する情報を表示するかどうかを返す場合に表示するメッセージを評価する条件を含む 3 つのパラメーターが渡されます。 アサーションが失敗した場合、メッセージが表示されます呼び出し元情報と 3 番目のパラメーターが true の場合。 図 12 を一覧表示する 7 に示すのアサーションが失敗した場合に表示されるエラー ダイアログの例に示します。

対応する最終の関数は、Sys.Debug.fail です。 スクリプト内で特定の行に失敗するコードを強制するときは、JavaScript アプリケーションで通常使用されるデバッガー ステートメントではなく Sys.Debug.fail 呼び出しを追加できます。 Sys.Debug.fail 関数は、次に示すように、失敗の理由を表す 1 つの文字列パラメーターを受け取ります。


[!code-css[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample10.css)]


[![Sys.Debug.assert エラー メッセージ。](understanding-asp-net-ajax-debugging-capabilities/_static/image35.png)](understanding-asp-net-ajax-debugging-capabilities/_static/image34.png)

**図 12**: A Sys.Debug.assert エラー メッセージ。  ([フルサイズのイメージを表示するをクリックして](understanding-asp-net-ajax-debugging-capabilities/_static/image36.png))


Sys.Debug.fail ステートメントをスクリプトが実行中に発生するとメッセージ パラメーターの値は、Visual Studio 2008 などのデバッグ アプリケーションのコンソールに表示されますが、アプリケーションをデバッグするよう求められます。 場合などがこれを非常に役には、インライン スクリプトに Visual Studio 2008 にブレークポイントを設定することはできません、変数の値を確認できるように、特定の行で停止するコードを希望する場合です。

*ScriptManager コントロールの ScriptMode プロパティについてください。*

ASP.NET AJAX ライブラリに含まれるデバッグ ビルドと既定で C:\Program files \microsoft ASP.NET\ASP.NET 2.0 AJAX Extensions\v1.0.61025\MicrosoftAjaxLibrary\System.Web.Extensions\1.0.61025.0 にインストールされているスクリプトのバージョンをリリースします。 デバッグ スクリプトは、適切に書式設定された、読みやすいとがいくつかの Sys.Debug.assert 呼び出しに散在してリリース スクリプト除去空白があるし、全体的なサイズを最小限に抑える Sys.Debug クラスを多用中にします。

ASP.NET AJAX ページに追加した ScriptManager コントロールは、web.config を読み込むためのスクリプトをライブラリのバージョンを特定で compilation 要素のデバッグの属性を読み取ります。 ただし場合を制御できますデバッグやリリース スクリプトが読み込まれた (ライブラリ スクリプトまたは独自のカスタム スクリプト) ScriptMode プロパティを変更することによりします。 ScriptMode は、メンバーが、Auto、デバッグ、リリース、および継承 ScriptMode 列挙を受け入れます。

ScriptMode ScriptManager が web.config で debug 属性を確認することを意味する自動の値に既定値です。デバッグが false の場合、ScriptManager は ASP.NET AJAX ライブラリ スクリプトのリリース バージョンを読み込みます。 デバッグが true の場合は、スクリプトのデバッグ バージョンが読み込まれます。 リリースまたはデバッグ ScriptMode プロパティを変更すると、デバッグ属性が web.config にどのような値に関係なく適切なスクリプトを読み込むためには、ScriptManager が実行されます。8 を一覧表示を ASP.NET AJAX ライブラリからデバッグ スクリプトを読み込むには、ScriptManager コントロールを使用する例を示します。

**8 を一覧表示します。ScriptManager を使用してデバッグ スクリプトを読み込んで**です。


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample11.aspx)]

リスト 9 に示すように、うにコンポーネントと共に ScriptManager のスクリプトのプロパティを使用して、独自のカスタム スクリプトの別のバージョン (デバッグまたはリリース) を読み込むこともできます。

**9 を一覧表示します。ScriptManager を使用してカスタム スクリプトを読み込んでいます。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample12.aspx)]

> [!NOTE]
> うにコンポーネントを使用してカスタム スクリプトを読み込んでいる場合は、スクリプトには、スクリプトの下部にある次のコードを追加することで読み込みが完了したときに、ScriptManager を通知する必要があります。


[!code-csharp[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample13.cs)]

リスト 9 に示すコードでは、Person.js ではなく Person.debug.js が自動的に検索されますので、ユーザーのスクリプトのデバッグ バージョンを探すには、ScriptManager を指示します。 Person.debug.js ファイルが見つからない場合、エラーが発生します。

デバッグまたは ScriptManager コントロールに設定 ScriptMode プロパティの値に基づいて、読み込まれるカスタム スクリプトのリリース バージョンの場合、継承をうにコントロールの ScriptMode プロパティを設定できます。 これにより読み込まれるカスタム スクリプトの適切なバージョンを一覧表示する 10 に示すように、ScriptManager の ScriptMode プロパティに基づいてされます。 デバッグするには、ScriptManager コントロールの ScriptMode プロパティが設定されているため、Person.debug.js スクリプトが読み込まれ、ページで使用されます。

**10 を一覧表示します。カスタム スクリプトのスクリプト マネージャーから、ScriptMode を継承します。**


[!code-aspx[Main](understanding-asp-net-ajax-debugging-capabilities/samples/sample14.aspx)]

ScriptMode プロパティを適切を使用してより簡単にアプリケーションをデバッグし、全体的なプロセスを簡略化できます。 ASP.NET AJAX ライブラリのリリースのスクリプトではなくステップ実行したり、コードの書式設定が削除されたデバッグ スクリプトはデバッグ目的専用の書式設定中に後に読み取られたするが困難。

## <a name="conclusion"></a>まとめ

Microsoft の ASP.NET AJAX テクノロジは、エンドユーザーの全体的なエクスペリエンスを強化するため、AJAX 対応のアプリケーションを構築するための強固な基盤を提供します。 ただし、として他のプログラミング テクノロジ バグおよびその他のアプリケーションの問題が確実に生じます。 使用可能なさまざまなデバッグ オプションの詳細を知ることと、多くの時間と結果がより安定した製品に保存できます。

この記事の内容には、いくつかの方法、Visual Studio 2008 や Web 開発ヘルパー Firebug インターネット エクスプ ローラーを含む ASP.NET AJAX ページをデバッグするために導入されてしました。 これらのツールは、変数のデータにアクセス、コード行を順を追っておよびトレース ステートメントを表示できるため、全体的なデバッグ プロセスを簡略化できます。 さまざまなデバッグ ツールについて説明だけでなくも表示アプリケーションで ASP.NET AJAX ライブラリの Sys.Debug クラスを使用する方法とは、ScriptManager をクラスを使用してを読み込む方法をデバッグまたはリリース バージョンのスクリプトのです。

## <a name="bio"></a>略歴

Dan Wahlin (Microsoft 最も有益な Professional ASP.NET と XML Web サービス) は、インターフェイスの技術的なトレーニングで .NET 開発インストラクターとアーキテクチャ コンサルタント ([www.interfacett.com)](http://www.interfacett.com)です。 Dan は ASP.NET 開発者 Web サイトの XML を設立 ([www.XMLforASP.NET](http://www.XMLforASP.NET))、INETA スピーカーの事務所では、いくつかの会議で読みます。 Dan 共同 Professional Windows DNA (Wrox)、ASP.NET で作成: ヒント、チュートリアルとコード (Sam)、ASP.NET 1.1 Insider ソリューション、Professional ASP.NET 2.0 AJAX (Wrox)、ASP.NET 2.0 MVP ハックおよび ASP.NET 開発者向け (Sam) の XML を作成します。 コード、文書またはブックが作成されません、ときに、Dan は書き込みと音楽を記録し、再生するゴルフと自分の子供と妻のバスケット楽しんでいます。

Scott カテゴリは、1997 年以降の Microsoft の Web テクノロジの使用されているがあり、myKB.com の代表者 ([www.myKB.com](http://www.myKB.com))、専門分野は、ASP.NET の書き込みの際にベースのアプリケーションのナレッジ ベースのソフトウェア ソリューションに重点を置きます。 Scott が接続時に電子メール[ scott.cate@myKB.com ](mailto:scott.cate@myKB.com)または彼のブログで[ScottCate.com](http://ScottCate.com)

> [!div class="step-by-step"]
> [前へ](understanding-asp-net-ajax-web-services.md)
