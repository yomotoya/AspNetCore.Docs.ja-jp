---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: 基本的な ASP.NET 4.5 Web フォーム ページの作成を Visual Studio 2013 で |Microsoft ドキュメント
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: 0dbd3063c9be3564637fad34e60f62c1662dcc07
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>基本的な ASP.NET 4.5 Web フォーム ページの作成では、Visual Studio 2013
====================
によって[Erik Reitan](https://github.com/Erikre)

このチュートリアルで Web 開発環境に概要を提供します。 [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)し、 [Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。 このチュートリアルを単純な ASP.NET Web フォーム ページを作成し、新しいページを作成する、コントロールの追加、およびコードの記述の基本的な方法を示しています。

このチュートリアルでは、以下のタスクを行います。

- ファイル システム Web フォーム アプリケーション プロジェクトを作成します。
- Visual Studio を理解します。
- ASP.NET ページを作成します。
- コントロールを追加します。
- イベント ハンドラーを追加します。
- 実行していると、Visual Studio からページをテストします。

## <a name="prerequisites"></a>必須コンポーネント


このチュートリアルを完了するための要件は次のとおりです。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。 .NET Framework は、自動的にインストールされます。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web が多くの場合、参照されます Visual Studio としてこのチュートリアルの系列全体です。  
    >   
    > Visual Studio を使用している場合は、このチュートリアルの前提条件が選択されている、 **Web 開発**設定のコレクションを初めて Visual Studio を起動します。 詳細については、次を参照してください。[する方法: Web 開発環境設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)です。


## <a name="creating-a-web-application-project-and-a-page"></a>Web アプリケーション プロジェクトと、ページの作成

<a id="sectionToggle0"></a>

チュートリアルのこの部分では、Web アプリケーション プロジェクトを作成し、新しいページを追加します。 また、HTML テキストを追加し、お使いのブラウザーでページを実行します。

### <a name="to-create-a-web-application-project"></a>Web アプリケーション プロジェクトを作成するには

1. Microsoft Visual Studio を開きます。
2. **[ファイル]** メニューの **[新しいプロジェクト]** を選択します。  
    ![[ファイル] メニュー](creating-a-basic-web-forms-page/_static/image1.png)

    **[新しいプロジェクト]** ダイアログ ボックスが表示されます。
3. 選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。
4. 選択、 **ASP.NET Web アプリケーション**中央の列内のテンプレートです。
5. プロジェクトの名前を付けます***BasicWebApp***  をクリックし、 **ok**ボタンをクリックします。   
![[新しいプロジェクト] ダイアログ ボックス](creating-a-basic-web-forms-page/_static/image2.png)
6. 次に、選択、 **Web フォーム**テンプレートをクリック、 **OK**プロジェクトを作成するボタンをクリックします。  
![新しい ASP.NET プロジェクト ダイアログ ボックス](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio では、Web フォーム テンプレートに基づいて作成済みの機能を含む新しいプロジェクトを作成します。 それだけでなくを提供しています、 *Home.aspx*  ページで、 *About.aspx*  ページで、 *Contact.aspx*  ページが、ユーザーを登録し、保存するメンバーシップ機能もあります自分の資格情報を web サイトにログインできるようにします。 既定では、Visual Studio が表示内のページに新しいページを作成すると、ときに**ソース**ビューでページの HTML 要素を確認できます。 次の図で表示される**ソース**という名前の新しい Web ページを作成した場合に表示*BasicWebApp.aspx*です。  
    ![ソース ビュー](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Visual Studio Web 開発環境のツアー


ページを変更して続行する前に、Visual Studio 開発環境をについて理解するおくと便利です。 次の図は、windows および Visual Studio および Visual Studio Express for Web で使用可能なツールを示しています。

> [!NOTE] 
> 
> この図では、既定の windows とウィンドウの場所を示します。 **ビュー**メニューでは、追加のウィンドウを表示および再配置し、好みに合わせてウィンドウのサイズを変更することができます。 既に加えられた変更をウィンドウの配置、表示される内容が一致しない場合の図は。


 Visual Studio 環境

![Visual Studio 環境](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Web デザイナーに慣れ親しみ

上の図を確認し、次の一覧に一致するテキスト ウィンドウやツールが、最も頻繁に記述するために使用します。 (すべての windows とここでは、ツール、のみマークした前の図にします。)

- ツールバー。 テキストや、検索テキストを書式設定するためのコマンドを提供します。 一部のツールバーで作業している場合にのみ、使用可能な**デザイン**ビュー。
- **ソリューション エクスプ ローラー**ウィンドウです。 Web アプリケーションでファイルとフォルダーを表示します。
- ドキュメント ウィンドウです。 タブ付きウィンドウ内で作業は、ドキュメントを表示します。 タブをクリックして、ドキュメントを切り替えることができます。
- **プロパティ**ウィンドウです。 使用すると、ページ、HTML 要素、コントロール、およびその他のオブジェクトの設定を変更できます。
- タブを表示します。 同じドキュメントのさまざまなビューが表示されます。 **デザイン**ビューは、WYSIWYG に近いの編集サーフェイスです。 **ソース**ビューは、ページの HTML エディターです。 **分割**ビューでは、両方が表示されます、**デザイン**ビューおよび**ソース**ドキュメントのビューです。 使用する、**デザイン**と**ソース**このチュートリアルの後半でビュー。 Web ページを開きたい場合**デザイン**ビューを**ツール** メニューのをクリックして**オプション**、select、 **HTML デザイナー**ノード、および変更、**ページで開始**オプション。
- **ツールボックス**です。 コントロールと、ページにドラッグできる HTML 要素を提供します。 **ツールボックス**要素は共通の関数でグループ化します。
- S **erver エクスプ ローラー**です。 データベース接続を表示します。 サーバー エクスプ ローラーが表示されない場合は、[表示] メニューで、サーバー エクスプ ローラーをクリックします。


### <a name="creating-a-new-aspnet-web-forms-page"></a>新しい ASP.NET Web フォーム ページを作成します。


新しい Web フォーム アプリケーションを使用して、作成する場合、 **ASP.NET Web アプリケーション**プロジェクト テンプレートを Visual Studio が、ASP.NET (Web フォーム ページ) という名前のページを追加する*Default.aspx*とその他のいくつかのファイル、およびフォルダーです。 使用することができます、 *Default.aspx*ページ、Web アプリケーションのホーム ページとして。 ただし、このチュートリアルで作成して新しいページを使用します。

### <a name="to-add-a-page-to-the-web-application"></a>Web アプリケーション ページを追加するには


1. 閉じる、 *Default.aspx*ページ。 これを行うには、ファイル名が表示されるタブをクリックし、閉じるオプションをクリックします。
2. **ソリューション エクスプ ローラー**、Web アプリケーションの名前を右クリックし (アプリケーション名は、このチュートリアルでは**BasicWebSite**)、をクリックして**追加** - &gt;**新しい項目の**します。   
**[新しい項目の追加]** ダイアログ ボックスが表示されます。
3. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。 次に、選択**Web フォーム**中央から一覧表示し、名前を付けます*名前*です。   
    ![新しい項目の追加 ダイアログ ボックスを追加します。](creating-a-basic-web-forms-page/_static/image6.png)
4. をクリックして**追加**をプロジェクトに web ページを追加します。  
Visual Studio では、新しいページを作成し、それを開きます。


### <a name="adding-html-to-the-page"></a>ページに HTML を追加します。


チュートリアルのこの部分では、ページに静的テキストを追加します。

### <a name="to-add-text-to-the-page"></a>ページにテキストを追加するには


1. ドキュメント ウィンドウの下部をクリックして、**デザイン** タブに切り替えるには**デザイン**ビュー。

    デザイン ビューでは、WYSIWYG に近い方法で、現在のページが表示されます。 この時点では、必要はありません、テキストや ページで、コントロール、ページは空白で、四角形の輪郭を示す破線です。 この四角形を表す、 **div**ページ上の要素。
2. 点線で示されている四角形の内側をクリックします。
3. 型**Visual Web Developer へようこそ**とキーを押します**ENTER** 2 回クリックします。

    次の図で入力したテキスト**デザイン**ビュー。

    ![デザイン ビューでの開始テキスト](creating-a-basic-web-forms-page/_static/image7.png "デザイン ビューでの開始テキスト")
4. 切り替える**ソース**ビュー。

    内の HTML を表示できます**ソース**で入力したときに作成したビュー**デザイン**ビュー。  
    ![静的なテキストを含む Web ページ](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>ページを実行します。


ページにコントロールを追加して続行する前に、最初に実行できます。

### <a name="to-run-the-page"></a>ページを実行するには


1. **ソリューション エクスプ ローラー**を右クリックして*名前*選択**スタート ページとして設定**です。
2. キーを押して**ctrl キーを押しながら f5 キーを押して**ページを実行します。

    ページは、ブラウザーに表示されます。 作成したページは、ファイル名拡張子が*.aspx*、現在の HTML ページのように実行されます。

    ブラウザーでページを表示するには、また内のページを右クリックしできます**ソリューション エクスプ ローラー**選択**ブラウザーで表示**です。
3. Web アプリケーションを停止するブラウザーを閉じます。


## <a name="adding-and-programming-controls"></a>追加して、コントロールのプログラミング


<a id="sectionToggle1"></a>

サーバー コントロールをページに今すぐ追加します。 ボタン、ラベル、テキスト ボックス、およびその他の使い慣れたコントロールなどのサーバー コントロールは、Web フォーム ページの一般的なフォーム処理機能を提供します。 ただし、クライアントではなく、サーバーで実行されるコードを持つコントロールをプログラミングできます。

追加、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロール、および[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロールをページおよび処理するコードを記述、 [ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベントを[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。

### <a name="to-add-controls-to-the-page"></a>コントロールをページに追加するには


1. クリックして、**デザイン** タブに切り替えるには**デザイン**ビュー。
2. 末尾にカーソルを置きます、 **Visual Web Developer へようこそ**テキストとキーを押して**ENTER**で確保するために 5 つ以上の時間、 **div**要素のボックスです。
3. **ツールボックス**、展開、**標準**が展開されていない場合にグループ化します。  
展開する必要があります、**ツールボックス**を表示する左側のウィンドウ。
4. ドラッグ、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)ページ上に制御しの途中にドロップ、 **div**を持つ要素のボックス**Visual Web Developer へようこそ**最初の行にします。
5. ドラッグ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)ページ上に制御およびの右側にドロップ、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロール。
6. ドラッグ、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)ページ上に制御し、下の個別の行の上にドロップ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。
7. 上にカーソルを配置、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)を制御し、入力**名を入力して:**です。

    この静的な HTML テキストがのキャプション、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロール。 静的な HTML と同じページ上のサーバー コントロールを混在させることができます。 次の図で 3 つのコントロールの表示方法を示しています。**デザイン**ビュー。

    ![デザイン ビューで 3 つのコントロール](creating-a-basic-web-forms-page/_static/image9.png "デザイン ビューで 3 つのコントロール")


### <a name="setting-control-properties"></a>コントロールのプロパティの設定


Visual Studio では、ページ上のコントロールのプロパティを設定するさまざまな方法を提供します。 チュートリアルのこの部分では、両方のプロパティを設定します**デザイン**ビューと**ソース**ビュー。

### <a name="to-set-control-properties"></a>コントロールのプロパティを設定するには


1. 最初に、表示、**プロパティ**windows からを選択して、**ビュー**メニュー -&gt; **その他のウィンドウ** - &gt; **プロパティ ウィンドウ**します。 別の方法として選択する可能性があります**F4**を表示する、**プロパティ**ウィンドウです。
2. 選択、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール、し、**プロパティ**ウィンドウで、設定の値**テキスト**に**表示名**です。 入力したテキストは、次の図に示すように、デザイナーで、ボタンに表示されます。

    ![ボタンのテキストを設定](creating-a-basic-web-forms-page/_static/image10.png "設定 ボタンのテキスト")
3. 切り替える**ソース**ビュー。

    **ソース**ビューには、サーバー コントロール用の Visual Studio が作成される要素を含む、ページの HTML が表示されます。 コントロールは、HTML に似た構文で宣言**asp:**属性を含めると**runat =&quot;サーバー&quot;**です。

    コントロールのプロパティは、属性として宣言されます。 たとえば、設定、[テキスト](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx)プロパティを[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)制御、ステップ 1 で、実際の設定、**テキスト**コントロールのマークアップ内の属性です。

    > [!NOTE] 
    > 
    > 内のすべてのコントロールにある、**フォーム**要素、属性を持つ**runat =&quot;サーバー&quot;**です。 **Runat =&quot;サーバー&quot;** 属性および**asp:**プレフィックスは、コントロールのタグは、によって処理される ASP.NET サーバーで、ページの実行時にされるようにコントロールをマークします。 外部コード**&lt;runat を形成 =&quot;サーバー&quot; &gt;**と**&lt;スクリプト runat =&quot;サーバー&quot; &gt;**要素は、ASP.NET コードが持つ開始タグを含む要素内にする必要がありますブラウザーに変更が送信、 **runat =&quot;サーバー&quot;** 属性。
4. 次に追加のプロパティを追加、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。 後の直後にカーソルを置く**Asp:label**で、 **&lt;Asp:label&gt;**タグ、およびキーを押します**space キー**です。

    設定できる使用可能なプロパティの一覧を表示するドロップダウン リストが表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。 この機能と呼ば**IntelliSense**で役立つ**ソース**サーバー コントロール、HTML 要素、およびその他の項目の構文を使用して、ページのビューです。 次の図は、 **IntelliSense**のドロップダウン リスト、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。

    ![IntelliSense 属性](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense 属性")
5. 選択**ForeColor**し、等号 (=) を入力します。

    IntelliSense は、色の一覧を表示します。

    > [!NOTE] 
    > 
    > 表示することができます、 **IntelliSense**キーを押して、いつでも一覧**CTRL + J**コードを表示するときにします。
6. 色を選択、 **[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)**コントロールのテキスト。 背景が白の読み取りに十分な暗い色を選択することを確認してください。

    **ForeColor**属性には、終わりの引用符を含む、選択した色が完了しました。


### <a name="programming-the-button-control"></a>ボタン コントロールのプログラミング


このチュートリアルでは、ユーザーがテキスト ボックスを入力し、内の名前を表示名を読み取っているコードを記述します、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。

### <a name="add-a-default-button-event-handler"></a>既定のボタンのイベント ハンドラーを追加します。


1. 切り替える**デザイン**ビュー。
2. ダブルクリックして、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。

    既定では、Visual Studio 分離コード ファイルに切り替わりますおよびのスケルトンのイベント ハンドラーを作成、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールの既定のイベントを[クリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント。 分離コード ファイルは、サーバー コード (c# など) から (HTML など)、UI のマークアップを分離します。   
   カーソルの位置が、このイベント ハンドラーのコードを追加します。

    > [!NOTE] 
    > 
    > 内のコントロールをダブルクリックすると**デザイン**ビューは、イベント ハンドラーを作成するいくつかの方法の 1 つです。
3. 内部、 **Button1\_ をクリックして**イベント ハンドラーで、「 **Label1**ピリオドが続く (**.**)。

    後にピリオドを入力すると、 **ID**ラベルの (**Label1**)、Visual Studio の使用可能なメンバーの一覧を表示する、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロールが、次に示す図。 メンバー プロパティでは通常、メソッド、またはイベント。

    ![コード ビューでは、IntelliSense](creating-a-basic-web-forms-page/_static/image12.png "コード ビューでの IntelliSense")
4. [完了]、**クリックして**ことを読み取り、次のコード例に示すように、ボタンのイベント ハンドラー。

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. 表示に戻り、**ソース**ビューを右クリックして HTML マークアップの*名前*で、**ソリューション エクスプ ローラー**を選択して**ビューマークアップ**です。
6. スクロールして、 **&lt;Asp:button&gt;**要素。 なお、 **&lt;Asp:button&gt;**要素が属性を持つようになりました**onclick =&quot;Button1\_ をクリックして&quot;**です。

    この属性のバインド ボタンの[クリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)前の手順でコード化されたハンドラー メソッドへのイベントです。

    イベント ハンドラー メソッドは、任意の名前を持つことができます。表示される名前は、Visual Studio によって作成される既定の名前です。 重要な点は、名前は使用する、 **OnClick** HTML の属性は分離コードで定義されたメソッドの名前と一致する必要があります。


### <a name="running-the-page"></a>ページを実行します。


ページ上のサーバー コントロールをテストできます。

### <a name="to-run-the-page"></a>ページを実行するには


1. キーを押して**ctrl キーを押しながら f5 キーを押して**ブラウザーでページを実行します。 エラーが発生する場合は、上記の手順を再確認します。
2. テキスト ボックスに名前を入力し、クリックして、**表示名**ボタンをクリックします。

    入力した名前が表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。 ボタンをクリックすると、ページは、Web サーバーにポストされたに注意してください。 ASP.NET ページを再作成、コードを実行 (この場合、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールの[ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント ハンドラーが実行される)、し、新しいページをブラウザーに送信します。 ブラウザーで、ステータス バーを監視する場合、ページは、作業を進めているラウンド トリップ Web サーバーに、ボタンをクリックするたびが確認できます。
3. ブラウザーで、表示、ページを右クリックし、選択を実行しているページのソース**ソースの表示**です。

    ページのソース コードでは、サーバー コードなし HTML を表示します。 具体的には、表示されない、 **&lt;asp:&gt;**で作業していた要素**ソース**ビュー。 ページの実行時に、ASP.NET はサーバー コントロールを処理し、コントロールを表す機能を実行するページに HTML 要素を表示します。 たとえば、 **&lt;Asp:button&gt;**コントロールは HTML としてレンダリングされます**&lt;入力の種類 =&quot;送信&quot;&gt;** 要素。
4. ブラウザーを閉じます。


## <a name="working-with-additional-controls"></a>その他のコントロールの操作

<a id="sectionToggle2"></a>

このチュートリアルでは、使用する、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)一度に 1 か月の日付を表示するコントロール。 [カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールは、ボタン、テキスト ボックス、およびラベルで作業しているよりも複雑なコントロールであり、サーバー コントロールの追加機能が一部を示しています。

このセクションでは追加、 [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールをページと書式設定します。

### <a name="to-add-a-calendar-control"></a>カレンダー コントロールを追加するには


1. Visual Studio に切り替えます**デザイン**ビュー。
2. **標準**のセクションで、**ツールボックス**、ドラッグ、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)ページ上に制御し、下にドロップして、 **div**要素をその他のコントロールが含まれています。

    カレンダーのスマート タグ パネルが表示されます。 パネルを選択したコントロールの最も一般的なタスクを実行するが簡単にするコマンドが表示されます。 次の図は、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)で表示するように制御**デザイン**ビュー。

    ![デザイン ビューでコントロールを calendar](creating-a-basic-web-forms-page/_static/image13.png "カレンダーのデザイン ビューでコントロール")
3. スマート タグ パネルで、次のように選択します。**オート フォーマット**です。

    **オート フォーマット** ダイアログ ボックスが表示されたら、カレンダーの書式指定スキームを選択することができます。 次の図は、**オート フォーマット**のダイアログ ボックス、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロール。

    ![オート フォーマット ダイアログ ボックス (Calendar コントロール)](creating-a-basic-web-forms-page/_static/image14.png "オート フォーマット ダイアログ ボックス (Calendar コントロール)")
4. **スキームを選択**一覧で、選択**単純な** をクリックし、 **OK**です。
5. 切り替える**ソース**ビュー。

    表示することができます、 **&lt;asp: カレンダー&gt;**要素。 この要素は、先ほど作成した単純なコントロールの要素よりも長い。 これもなどが含まれるサブ要素**&lt;この&gt;**、表示されているさまざまな書式設定します。 次の図は、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)で制御**ソース**ビュー。 (正確なマークアップに表示される**ソース**図からビューが多少異なる場合があります)。

    ![ソース ビュー内のコントロールを calendar](creating-a-basic-web-forms-page/_static/image15.png "カレンダーのソース ビュー内のコントロール")


### <a name="programming-the-calendar-control"></a>予定表コントロールのプログラミング


プログラムは、このセクションで、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールに現在選択されている日付を表示します。

### <a name="to-program-the-calendar-control"></a>予定表コントロールのプログラミング


1. **デザイン**ビューをダブルクリックして、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロール。

    新しいイベント ハンドラーが作成され、という名前の分離コード ファイルに表示される*FirstWebPage.aspx.cs*です。
2. [完了]、 [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx)イベント ハンドラーを次のコード。


~~~
[!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


[!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]
~~~

 上記のコードは、予定表コントロールの選択した日付をラベル コントロールのテキストを設定します。


### <a name="running-the-page"></a>ページを実行します。


予定表をテストできます。

### <a name="to-run-the-page"></a>ページを実行するには


1. キーを押して**ctrl キーを押しながら f5 キーを押して**ブラウザーでページを実行します。
2. カレンダーの日付をクリックします。

    をクリックした日付が表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。
3. ブラウザーで、ページのソース コードを表示します。

    注意してください、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)としてそのページにコントロールが表示されて、**テーブル**、としては、1 日で、 **td**要素。
4. ブラウザーを閉じます。


## <a name="next-steps"></a>次の手順


<a id="nextStepsToggle"></a>

このチュートリアルでは、Visual Studio ページ デザイナーの基本機能を説明しました。 作成し、Visual Studio で Web フォーム ページを編集する方法を理解すると、その他の機能を探索したい場合があります。 たとえば、次の操作をする可能性があります。

- ASP.NET Web フォームの詳細について次のステップ バイ ステップ チュートリアル シリーズして[ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)です。
- カスケード スタイル シート (CSS) の詳細を表示します。 詳細については、「 [CSS 概要扱う](https://msdn.microsoft.com/library/bb398931.aspx)です。
