---
uid: web-forms/overview/getting-started/creating-a-basic-web-forms-page
title: 基本的な ASP.NET 4.5 Web フォーム ページの作成では、Visual Studio 2013 |Microsoft Docs
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: a2f1c635-0817-4a9a-8c13-d5b5d29727c0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/getting-started/creating-a-basic-web-forms-page
msc.type: authoredcontent
ms.openlocfilehash: c2051b166b8800654a107c5100ad5ed94af93407
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37394454"
---
<a name="creating-a-basic-aspnet-45-web-forms-page-in-visual-studio-2013"></a>基本的な ASP.NET 4.5 Web フォーム ページの作成では、Visual Studio 2013
====================
によって[Erik Reitan](https://github.com/Erikre)

このチュートリアルの概要を Web の開発環境を提供します。 [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)し[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。 このチュートリアルでは、単純な ASP.NET Web フォーム ページを作成する方法し、の新しいページを作成するコントロールを追加、コードを記述する基本的な方法を示しています。

このチュートリアルでは、以下のタスクを行います。

- ファイル システム Web フォーム アプリケーション プロジェクトを作成します。
- Visual Studio を理解します。
- ASP.NET ページを作成します。
- コントロールを追加します。
- イベント ハンドラーを追加します。
- 実行していると、Visual Studio からページをテストします。

## <a name="prerequisites"></a>必須コンポーネント


このチュートリアルを完了するための要件は次のとおりです。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)します。 .NET Framework は、自動的にインストールされます。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web は多くの場合、呼びます Visual Studio とこのチュートリアル シリーズです。  
    >   
    > Visual Studio を使用している場合は、このチュートリアルの前提条件が選択されている、 **Web 開発**設定のコレクションを初めて Visual Studio を起動します。 詳細については、次を参照してください。[方法: Web 開発環境の設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)します。


## <a name="creating-a-web-application-project-and-a-page"></a>Web アプリケーション プロジェクトと、ページの作成

<a id="sectionToggle0"></a>

チュートリアルのこの部分では、Web アプリケーション プロジェクトを作成し、新しいページを追加します。 また、HTML テキストを追加し、お使いのブラウザーでページを実行します。

### <a name="to-create-a-web-application-project"></a>Web アプリケーション プロジェクトを作成するには

1. Microsoft Visual Studio を開きます。
2. **[ファイル]** メニューの **[新しいプロジェクト]** を選択します。  
    ![[ファイル] メニュー](creating-a-basic-web-forms-page/_static/image1.png)

    **[新しいプロジェクト]** ダイアログ ボックスが表示されます。
3. 選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレート グループ。
4. 選択、 **ASP.NET Web アプリケーション**中央の列のテンプレート。
5. プロジェクトに名前を***BasicWebApp***  をクリックし、 **OK**ボタン。   
![新しいプロジェクト ダイアログ ボックス](creating-a-basic-web-forms-page/_static/image2.png)
6. 次に、選択、 **Web フォーム**テンプレートをクリックして、 **OK**プロジェクトを作成するボタンをクリックします。  
![新しい ASP.NET プロジェクト ダイアログ ボックス](creating-a-basic-web-forms-page/_static/image3.png)  

    Visual Studio では、Web フォーム テンプレートに基づく事前構築済みの機能を含む新しいプロジェクトを作成します。 提供するだけでなく、 *Home.aspx*  ページで、 *About.aspx*  ページで、 *Contact.aspx* 、ページしますが、ユーザーを登録し、保存するメンバーシップ機能も含まれています自分の資格情報、web サイトにログインできるようにします。 既定では、Visual Studio が表示、ページで新しいページが作成されると、**ソース**ビュー、ページの HTML 要素を確認できます。 次の図で表示される**ソース**という名前の新しい Web ページを作成した場合に表示*BasicWebApp.aspx*します。  
    ![ソース ビュー](creating-a-basic-web-forms-page/_static/image4.png)


### <a name="a-tour-of-the-visual-studio-web-development-environment"></a>Visual Studio の Web 開発環境のツアー


ページの変更を続行する前に、Visual Studio 開発環境について理解するおくと便利です。 次の図は、windows と Visual Studio と Visual Studio Express for Web で利用できるツールを示しています。

> [!NOTE] 
> 
> この図では、既定の windows とウィンドウの場所を示します。 **ビュー**メニューでは、追加のウィンドウを表示および再配置し、好みに合わせて大きさを変更することができます。 既に変更されて場合、ウィンドウを整列する、表示される内容と図が一致しません。


 Visual Studio 環境

![Visual Studio 環境](creating-a-basic-web-forms-page/_static/image5.png)

### <a name="familiarize-yourself-with-the-web-designer"></a>Web デザイナーに慣れ親しみ

上の図を確認し、次の一覧に一致するテキスト ウィンドウやツールを使用する、最もよくについて説明します。 (すべての windows とここは表示されているツールだけでマーク、前の図。)

- ツールバー。 テキストや、検索テキストを書式設定するためのコマンドを提供します。 一部のツールバーで作業している場合にのみ使用可能な**デザイン**ビュー。
- **ソリューション エクスプ ローラー**ウィンドウ。 Web アプリケーションでは、ファイルとフォルダーを表示します。
- ドキュメント ウィンドウです。 タブ付きウィンドウで作業しているドキュメントを表示します。 タブをクリックしてドキュメントを切り替えることができます。
- **プロパティ**ウィンドウ。 ページ、HTML 要素、コントロール、およびその他のオブジェクトの設定を変更することができます。
- タブを表示します。 同じドキュメントのさまざまなビューが表示されます。 **デザイン**ビューは、WYSIWYG に近い編集サーフェイス。 **ソース**ビューは、ページの HTML エディター。 **分割**ビューでは、両方が表示されます、**デザイン**ビューと**ソース**ドキュメントのビュー。 使用する、**デザイン**と**ソース**このチュートリアルの後半でのビュー。 Web ページを開きたい場合**デザイン**ビュー、**ツール** メニューのをクリックして**オプション**を選択、 **HTML デザイナー**ノード、および変更、**ページで開始**オプション。
- **ツールボックス**します。 コントロールと、ページ上にドラッグできる HTML 要素を提供します。 **ツールボックス**要素が共通の関数によってグループ化されます。
- S **ーバー エクスプ ローラー**します。 データベース接続が表示されます。 サーバー エクスプ ローラーが表示されない場合は、表示 メニューで、サーバー エクスプ ローラー をクリックします。


### <a name="creating-a-new-aspnet-web-forms-page"></a>新しい ASP.NET Web フォーム ページの作成


新しい Web フォーム アプリケーションを作成する場合、 **ASP.NET Web アプリケーション**プロジェクト テンプレートを Visual Studio の ASP.NET ページ (Web フォーム ページ) という名前を追加します*Default.aspx*他のいくつかのファイルだけでなく、フォルダーを選択します。 使用することができます、 *Default.aspx*ページ、Web アプリケーションのホーム ページとして。 ただし、このチュートリアルでは作成し、新しいページを使用します。

### <a name="to-add-a-page-to-the-web-application"></a>Web アプリケーションにページを追加するには


1. 閉じる、 *Default.aspx*ページ。 これを行うには、ファイル名を表示するタブをクリックし、閉じるオプションをクリックします。
2. **ソリューション エクスプ ローラー**、Web アプリケーションの名前を右クリックし (アプリケーション名は、このチュートリアルでは**BasicWebSite**)、をクリックし、**追加** - &gt;**新しい項目の**します。   
**[新しい項目の追加]** ダイアログ ボックスが表示されます。
3. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。 次に、選択**Web フォーム**中央から一覧表示し、名前を*名前*します。   
    ![新しい項目 ダイアログ ボックスを追加します。](creating-a-basic-web-forms-page/_static/image6.png)
4. クリックして**追加**web ページ、プロジェクトを追加します。  
Visual Studio では、新しいページを作成し、それを開きます。


### <a name="adding-html-to-the-page"></a>ページに HTML を追加します。


このチュートリアルでは、ページに静的テキストを追加します。

### <a name="to-add-text-to-the-page"></a>テキスト ページを追加するには


1. ドキュメント ウィンドウの下部で、をクリックして、**デザイン** タブに切り替える**デザイン**ビュー。

    デザイン ビューでは、WYSIWYG に近い方法で、現在のページが表示されます。 この時点では、必要はありません、テキストや ページで、コントロール、ページは空白で、四角形の輪郭を示す破線。 この四角形を表す、 **div**ページ上の要素。
2. 破線で示されている四角形の内側をクリックします。
3. 型**Visual Web Developer へようこそ**キーを押します**ENTER** 2 回です。

    次の図で入力したテキスト**デザイン**ビュー。

    ![デザイン ビューでの開始テキスト](creating-a-basic-web-forms-page/_static/image7.png "デザイン ビューでの開始テキスト")
4. 切り替える**ソース**ビュー。

    HTML を表示できます**ソース**で入力したときに作成したビュー**デザイン**ビュー。  
    ![静的テキストを含む Web ページ](creating-a-basic-web-forms-page/_static/image8.png)


### <a name="running-the-page"></a>ページを実行します。


ページにコントロールを追加して続行する前に、最初に実行できます。

### <a name="to-run-the-page"></a>ページの実行


1. **ソリューション エクスプ ローラー**、右クリック*名前*選択と**スタート ページとして設定**します。
2. キーを押して**CTRL + F5**ページを実行します。

    ページがブラウザーに表示されます。 作成したページには、ファイル名拡張子が *.aspx*、現在の HTML ページのように実行されます。

    ブラウザーでページを表示するもでページを右クリックしてできます**ソリューション エクスプ ローラー**選択**ブラウザーで表示**します。
3. Web アプリケーションを停止するブラウザーを閉じます。


## <a name="adding-and-programming-controls"></a>追加して、コントロールのプログラミング


<a id="sectionToggle1"></a>

これで、ページにサーバー コントロールを追加します。 ボタン、ラベル、テキスト ボックス、およびその他の使い慣れたコントロールなどのサーバー コントロールでは、Web フォーム ページの一般的なフォーム処理機能を提供します。 ただし、クライアントではなく、サーバーで実行されるコードでコントロールをプログラミングできます。

追加、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロールと[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロールをページと、処理するコードを記述、 [ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベントを[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。

### <a name="to-add-controls-to-the-page"></a>ページにコントロールを追加するには


1. をクリックして、**デザイン** タブに切り替える**デザイン**ビュー。
2. 末尾にカーソルを置きます、 **Visual Web Developer へようこそ**テキストとキーを押して**ENTER**で確保するために 5 つ以上の時間、 **div**要素のボックスです。
3. **ツールボックス**、展開、**標準**が展開されていない場合にグループ化します。  
展開する必要がありますに注意してください、**ツールボックス**表示するための左側のウィンドウ。
4. ドラッグ、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)ページ上に制御し、ドロップの途中で、 **div**を持つ要素のボックス**Visual Web Developer へようこそ**最初の行にします。
5. ドラッグ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)ページ上に制御しの右側にドロップ、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロール。
6. ドラッグ、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)ページ上に制御し、下の個別の行にドロップ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。
7. 上にカーソルを配置、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx) 、制御し、入力**名を入力します:** します。

    この静的な HTML テキストがのキャプション、 [TextBox](https://msdn.microsoft.com/library/system.web.ui.webcontrols.textbox.aspx)コントロール。 静的な HTML と同じページ上のサーバー コントロールを混在させることができます。 次の図は、3 つのコントロールでの表示方法を示します**デザイン**ビュー。

    ![デザイン ビューで 3 つのコントロール](creating-a-basic-web-forms-page/_static/image9.png "デザイン ビューで 3 つのコントロール")


### <a name="setting-control-properties"></a>コントロールのプロパティの設定


Visual Studio では、ページ上のコントロールのプロパティを設定するさまざまな方法を提供します。 このチュートリアルでは、両方のプロパティを設定**デザイン**ビューと**ソース**ビュー。

### <a name="to-set-control-properties"></a>コントロールのプロパティを設定するには


1. 最初に、表示、**プロパティ**からを選択して、windows、**ビュー**メニュー -&gt; **その他の Windows**  - &gt; **プロパティ ウィンドウ**します。 選択または**F4**を表示する、**プロパティ**ウィンドウ。
2. 選択、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール、し、**プロパティ**ウィンドウで、設定の値**テキスト**に**表示名**します。 入力したテキストは、次の図に示すように、デザイナーで、ボタンに表示されます。

    ![ボタン設定テキスト](creating-a-basic-web-forms-page/_static/image10.png "設定ボタンのテキスト")
3. 切り替える**ソース**ビュー。

    **ソース**ビューには、Visual Studio のサーバー コントロールの作成した要素を含む、ページの HTML が表示されます。 コントロールは、HTML に似た構文で宣言**asp:** 属性を含めると**runat =&quot;server&quot;** します。

    コントロールのプロパティは、属性として宣言されます。 たとえば、設定、[テキスト](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.text.aspx)プロパティを[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールを手順 1 で、実際の設定、**テキスト**コントロールのマークアップ内の属性。

    > [!NOTE] 
    > 
    > 内のすべてのコントロールは、**フォーム**要素、属性を持つ**runat =&quot;server&quot;** します。 **Runat =&quot;server&quot;** 属性と**asp:** プレフィックスは、によって処理される ASP.NET サーバー ページが実行されるように、コントロールのタグは、コントロールをマークします。 外部コード**&lt;フォームの runat =&quot;サーバー&quot; &gt;** と**&lt;スクリプトの runat =&quot;server&quot; &gt;** 要素は、ASP.NET コードが持つ開始タグを含む要素内にある必要がありますブラウザーにそのまま送信は、 **runat =&quot;server&quot;** 属性。
4. 追加のプロパティを次に、追加は、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。 配置後に、挿入ポイントを直接**Asp:label**で、 **&lt;Asp:label&gt;** タグ、およびキーを押します**space キーを押す**します。

    設定できる使用可能なプロパティの一覧を表示するドロップダウン リストが表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。 呼ばれるこの機能により、 **IntelliSense**で役立つ**ソース**サーバー コントロール、HTML 要素、およびその他の項目の構文を使用して、ページのビュー。 次の図は、 **IntelliSense**のドロップダウン リスト、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。

    ![IntelliSense 属性](creating-a-basic-web-forms-page/_static/image11.png "IntelliSense 属性")
5. 選択**ForeColor**し、等号 (=) を入力します。

    IntelliSense では、色の一覧が表示されます。

    > [!NOTE] 
    > 
    > 表示することができます、 **IntelliSense**ドロップダウン リストを押して、いつでも**CTRL + J**コードを表示するときにします。
6. 色を選択、 **[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)** コントロールのテキスト。 白の背景を読み取る十分に暗い色を選択することを確認します。

    **ForeColor**属性には、二重引用符を含め、選択した色が完了しました。


### <a name="programming-the-button-control"></a>ボタン コントロールのプログラミング


このチュートリアルでは、名前を読み取り、ユーザーがテキスト ボックスを入力しで名前を表示するコードを記述する、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。

### <a name="add-a-default-button-event-handler"></a>既定のボタン イベント ハンドラーを追加します。


1. 切り替える**デザイン**ビュー。
2. ダブルクリックして、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロール。

    既定では、Visual Studio の分離コード ファイルに切り替わりますのスケルトン イベント ハンドラーを作成、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールの既定のイベントを[クリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント。 分離コード ファイルは、(c#) など、サーバー コードから、UI のマークアップ (HTML など) を分離します。   
   カーソルがこのイベント ハンドラーのコードを追加します。

    > [!NOTE] 
    > 
    > コントロールをダブルクリック**デザイン**ビューは、イベント ハンドラーを作成するいくつかの方法の 1 つにすぎません。
3. 内で、 **Button1\_クリックして**イベント ハンドラーで、「 **Label1**ピリオドが続く (**.**)。

    後にピリオドを入力すると、 **ID**ラベルの (**Label1**)、Visual Studio の利用可能なメンバーの一覧を表示する、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロールが、次に示すように図。 メンバー プロパティでは通常、メソッド、またはイベント。

    ![コード ビューでの IntelliSense](creating-a-basic-web-forms-page/_static/image12.png "コード ビューでの IntelliSense")
4. [完了]、**クリックして**を読み取り、次のコード例で示すように、ボタンのイベント ハンドラー。

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample1.cs?highlight=3)]

    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample2.vb?highlight=2)]
5. 表示に戻り、**ソース**ビューを右クリックし、HTML マークアップの*名前*で、**ソリューション エクスプ ローラー**を選択して**ビューマークアップ**します。
6. スクロールして、 **&lt;Asp:button&gt;** 要素。 なお、 **&lt;Asp:button&gt;** 要素が属性を持つようになりました**onclick =&quot;Button1\_クリックして&quot;** します。

    この属性のバインド ボタンの[クリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント前の手順でコード化されたハンドラー メソッドにします。

    イベント ハンドラー メソッドは、任意の名前を持つことができます。表示される名前は、Visual Studio によって作成された既定の名前です。 重要な点は、名前に使用する、 **OnClick** HTML 属性では分離コードで定義されているメソッドの名前と一致する必要があります。


### <a name="running-the-page"></a>ページを実行します。


ページ上のサーバー コントロールをテストできます。

### <a name="to-run-the-page"></a>ページの実行


1. キーを押して**CTRL + F5**ブラウザーでページを実行します。 エラーが発生する場合は、上記の手順を再確認します。
2. テキスト ボックスに名前を入力し、をクリックして、**表示名**ボタンをクリックします。

    入力した名前が表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。 ボタンをクリックするとに、ページが Web サーバーにポストされたことに注意してください。 ASP.NET ページを再作成、コードを実行 (この場合、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールの[ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント ハンドラーが実行される)、し、ブラウザーに新しいページを送信します。 ブラウザーのステータス バーを視聴する場合、ページことのラウンド トリップ Web サーバーに、ボタンをクリックするたびが確認できます。
3. ブラウザーで表示 ページの右クリックして実行しているページのソース**ソースの表示**します。

    ページのソース コードでは、サーバー コードなし HTML を表示します。 具体的には、表示されない、 **&lt;asp:&gt;** で作業していた要素**ソース**ビュー。 ページの実行時に ASP.NET がサーバー コントロールを処理し、コントロールを表す機能を実行するページに HTML 要素をレンダリングします。 たとえば、 **&lt;Asp:button&gt;** コントロールが HTML としてレンダリングされる**&lt;入力の種類 =&quot;送信&quot;&gt;** 要素。
4. ブラウザーを閉じます。


## <a name="working-with-additional-controls"></a>その他のコントロールの操作

<a id="sectionToggle2"></a>

このチュートリアルでは、機能、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)一度に 1 か月間の日付を表示するコントロール。 [カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールで作業しているボタン、テキスト ボックス、およびラベルよりもさらに複雑なコントロールし、サーバー コントロールのいくつか他の機能を示しています。

このセクションでは追加、 [System.Web.UI.WebControls.Calendar](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールをページと書式設定します。

### <a name="to-add-a-calendar-control"></a>カレンダー コントロールを追加するには


1. Visual Studio に切り替えます**デザイン**ビュー。
2. **標準**のセクション、**ツールボックス**、ドラッグ、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)ページ上に制御し、下にドロップ、 **div**要素ですその他のコントロールが含まれています。

    カレンダーのスマート タグ パネルが表示されます。 パネルには、選択したコントロールの最も一般的なタスクを実行しやすくコマンドが表示されます。 次の図は、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールで表示される**デザイン**ビュー。

    ![デザイン ビューでコントロールをカレンダー](creating-a-basic-web-forms-page/_static/image13.png "カレンダーのデザイン ビューでコントロール")
3. スマート タグ パネルで、選択**オート フォーマット**します。

    **オート フォーマット** ダイアログ ボックスが表示されたら、予定表の書式指定スキームを選択できます。 次の図は、**オート フォーマット**の ダイアログ ボックス、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロール。

    ![オート フォーマット ダイアログ ボックス (Calendar コントロール)](creating-a-basic-web-forms-page/_static/image14.png "オート フォーマット ダイアログ ボックス (Calendar コントロール)")
4. **スキームを選択**一覧で、選択**単純な**順にクリックします**OK**します。
5. 切り替える**ソース**ビュー。

    確認できます、  **&lt;asp: 予定表&gt;** 要素。 この要素は、先ほど作成した単純なコントロールの要素よりも長いです。 これも含まれています、サブ要素など**&lt;この&gt;**、さまざまな書式設定を表します。 次の図は、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)制御**ソース**ビュー。 (正確なマークアップに表示される**ソース**からの図は、ビューが若干異なる場合があります)。

    ![ソース ビュー内のコントロールをカレンダー](creating-a-basic-web-forms-page/_static/image15.png "予定表のソース ビュー内のコントロール")


### <a name="programming-the-calendar-control"></a>予定表コントロールのプログラミング


プログラムがのこのセクションで、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)現在選択されている日付を表示するコントロール。

### <a name="to-program-the-calendar-control"></a>予定表コントロールのプログラミング


1. **デザイン**ビューで、ダブルクリック、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロール。

    新しいイベント ハンドラーが作成され、という名前の分離コード ファイルに表示される*FirstWebPage.aspx.cs*します。
2. [完了]、 [SelectionChanged](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.selectionchanged.aspx)イベント ハンドラーを次のコード。

    [!code-csharp[Main](creating-a-basic-web-forms-page/samples/sample3.cs?highlight=3)]


    [!code-vb[Main](creating-a-basic-web-forms-page/samples/sample4.vb?highlight=2)]

    上記のコードは、ラベル コントロールのテキストを予定表コントロールの選択した日付に設定します。


### <a name="running-the-page"></a>ページを実行します。


予定表をテストできます。

### <a name="to-run-the-page"></a>ページの実行


1. キーを押して**CTRL + F5**ブラウザーでページを実行します。
2. カレンダーの日付をクリックします。

    をクリックした日付が表示されます、[ラベル](https://msdn.microsoft.com/library/system.web.ui.webcontrols.label.aspx)コントロール。
3. ブラウザーでページのソース コードを表示します。

    なお、[カレンダー](https://msdn.microsoft.com/library/system.web.ui.webcontrols.calendar.aspx)コントロールとしてページに表示されて、**テーブル**、として毎日を**td**要素。
4. ブラウザーを閉じます。


## <a name="next-steps"></a>次の手順


<a id="nextStepsToggle"></a>

このチュートリアルでは、Visual Studio のページ デザイナーの基本機能を説明しました。 作成し、Visual Studio で Web フォーム ページを編集する方法を理解したところでその他の機能を探索する可能性があります。 たとえば、以下を実行します。

- ASP.NET Web フォームの詳細について次のステップ バイ ステップ チュートリアル シリーズして[ASP.NET 4.5 Web フォームと Visual Studio 2013 の概要](getting-started-with-aspnet-45-web-forms/introduction-and-overview.md)します。
- カスケード スタイル シート (CSS) について説明します。 詳細については、次を参照してください。 [CSS の概要での作業](https://msdn.microsoft.com/library/bb398931.aspx)します。
