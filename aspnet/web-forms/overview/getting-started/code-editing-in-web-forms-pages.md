---
uid: web-forms/overview/getting-started/code-editing-in-web-forms-pages
title: コードは、Visual Studio 2013 での ASP.NET Web フォームを編集 |Microsoft ドキュメント
author: Erikre
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/03/2014
ms.topic: article
ms.assetid: 5344b74e-b888-479a-92bc-601a33bd61a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/code-editing-in-web-forms-pages
msc.type: authoredcontent
ms.openlocfilehash: 79b10df04432490d6338dadb8f7ddd36192beb3e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
ms.locfileid: "30880753"
---
<a name="code-editing-aspnet-web-forms-in-visual-studio-2013"></a>Visual Studio 2013 でのコードの編集の ASP.NET Web フォームします。
====================
によって[Erik Reitan](https://github.com/Erikre)

多くの ASP.NET Web フォーム ページには、Visual Basic、C# の場合、または別の言語でコードを記述します。 Visual Studio でコード エディターには、エラーを回避することができ、コードをすばやく記述するのに役立ちます。 さらに、エディターは、行う必要がある作業量を減らすために再利用可能なコードを作成するための方法を提供します。

このチュートリアルでは、Visual Studio code エディターのさまざまな機能について説明します。

このチュートリアルでは、次の作業を行う方法について説明します。

- インラインのコーディング エラーを修正します。
- リファクタリングし、コードの名前を変更します。
- 変数およびオブジェクトの名前を変更します。
- コード スニペットを挿入します。

## <a name="prerequisites"></a>必須コンポーネント


このチュートリアルを完了するための要件は次のとおりです。

- [Microsoft Visual Studio 2013](https://www.microsoft.com/visualstudio/11/downloads#vs)または[Microsoft Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/11/downloads#express-web)です。 .NET Framework は、自動的にインストールされます。 

    > [!NOTE] 
    > 
    > Microsoft Visual Studio 2013 および Microsoft Visual Studio Express 2013 for Web が多くの場合、参照されます Visual Studio としてこのチュートリアルの系列全体です。  
    >   
    > Visual Studio を使用している場合は、このチュートリアルの前提条件が選択されている、 **Web 開発**設定のコレクションを初めて Visual Studio を起動します。 詳細については、次を参照してください。[する方法: Web 開発環境設定の選択](https://msdn.microsoft.com/library/ff521558.aspx)です。

  Visual Studio と ASP.NET の概要については、次を参照してください。 [Visual Studio 2013 での基本的な ASP.NET 4.5 Web フォーム ページの作成](creating-a-basic-web-forms-page.md)です。   
 

## <a name="creating-a-web-application-project-and-a-page"></a>Web アプリケーション プロジェクトと、ページの作成

<a id="sectionToggle0"></a>

チュートリアルのこの部分では、Web アプリケーション プロジェクトを作成し、新しいページを追加します。

### <a name="to-create-a-web-application-project"></a>Web アプリケーション プロジェクトを作成するには

1. Microsoft Visual Studio を開きます。
2. **[ファイル]** メニューの **[新しいプロジェクト]** を選択します。  
    ![[ファイル] メニュー](code-editing-in-web-forms-pages/_static/image1.png)

    **[新しいプロジェクト]** ダイアログ ボックスが表示されます。
3. 選択、**テンプレート** - &gt; **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。
4. 選択、 **ASP.NET Web アプリケーション**中央の列内のテンプレートです。
5. プロジェクトの名前を付けます***BasicWebApp***  をクリックし、 **ok**ボタンをクリックします。   
![[新しいプロジェクト] ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image2.png)
6. 次に、選択、 **Web フォーム**テンプレートをクリック、 **OK**プロジェクトを作成するボタンをクリックします。  
![新しい ASP.NET プロジェクト ダイアログ ボックス](code-editing-in-web-forms-pages/_static/image3.png)  

    Visual Studio では、Web フォーム テンプレートに基づいて作成済みの機能を含む新しいプロジェクトを作成します。


## <a name="creating-a-new-aspnet-web-forms-page"></a>新しい ASP.NET Web フォーム ページを作成します。


新しい Web フォーム アプリケーションを使用して、作成する場合、 **ASP.NET Web アプリケーション**プロジェクト テンプレートを Visual Studio が、ASP.NET (Web フォーム ページ) という名前のページを追加する*Default.aspx*とその他のいくつかのファイル、およびフォルダーです。 使用することができます、 *Default.aspx*ページ、Web アプリケーションのホーム ページとして。 ただし、このチュートリアルで作成して新しいページを使用します。

### <a name="to-add-a-page-to-the-web-application"></a>Web アプリケーション ページを追加するには


1. **ソリューション エクスプ ローラー**、Web アプリケーションの名前を右クリックし (アプリケーション名は、このチュートリアルでは**BasicWebSite**)、をクリックして**追加** - &gt;**新しい項目の**します。   
**[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。 次に、選択**Web フォーム**中央から一覧表示し、名前を付けます*名前*です。   
    ![新しい項目の追加 ダイアログ ボックスを追加します。](code-editing-in-web-forms-pages/_static/image4.png)
3. をクリックして**追加**をプロジェクトに Web フォーム ページを追加します。  
 Visual Studio では、新しいページを作成し、それを開きます。
4. 次に、この新しいページを既定のスタートアップ ページとして設定します。 **ソリューション エクスプ ローラー**、という名前の新しいページを右クリックして*名前*選択**スタート ページとして設定**です。 次に、作業の進行状況をテストするには、このアプリケーションを実行したとき、ブラウザーでこの新しいページが自動的に表示されます。


## <a name="correcting-inline-coding-errors"></a>インラインのコーディング エラーを修正します。


Visual Studio でコード エディターでは、コードを記述すると、コード エディターがエラーを修正することができます、エラーを行った場合、エラーを回避するのに役立ちます。 このチュートリアルでは、コード エディターでエラー補正機能を示す行を記述します。

### <a name="to-correct-simple-coding-errors-in-visual-studio"></a>Visual Studio での単純なコーディング エラーを修正するには


1. **デザイン**ビューで、空白のページのハンドラーを作成する をダブルクリック、**ロード**ページのイベントです。   
   一部のコードを記述するのに場所としてのみ、イベント ハンドラーを使用しています。
2. ハンドラーの内部エラーおよびキーを押してを含む次の行を入力**ENTER**:

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample1.cs)]

   押すと**ENTER**、コード エディターが緑色と赤色の下線を配置 (よくを呼び出す&quot;波&quot;行) に問題があるコードの領域の下。 緑色の下線は、警告を示します。 赤い下線では、解決する必要がありますのあるエラーを示します。 

    上にマウス ポインターを置く`myStr`警告に関する説明が表示されるツールヒントを確認します。 また、エラー メッセージを表示する赤い下線の上にマウス ポインターを保持します。

    次の図は、下線でコードを示します。

    ![デザイン ビューでの開始テキスト](code-editing-in-web-forms-pages/_static/image5.png "デザイン ビューでの開始テキスト")  
   セミコロンを追加することで、エラーを修正する必要があります`;`行の末尾にします。 警告通知するだけで、使用していないこと、`myStr`まだ変数。  

    > [!NOTE] 
    > 
    > Visual Studio での設定を書式設定を選択して、現在のコードを表示する**ツール** - &gt; **オプション** - &gt; **フォントと色**です。


## <a name="refactoring-and-renaming"></a>リファクタリングと 名前を変更します。

リファクタリングとは、ソフトウェアの手法を理解し、その機能を維持しながら、管理を容易にできるように、コードの再構成を含むです。 データベースからデータを取得するイベント ハンドラーでコードを記述するには、単純な例があります。 ページを開発するいくつかの異なるハンドラーからのデータにアクセスする必要があることを検出します。 そのため、ページのコードをリファクタリングして、ページで、データ アクセス方法を作成し、ハンドラーで、メソッドの呼び出しを挿入します。

コード エディターには、リファクタリングのさまざまなタスクを実行するのに役立つツールが含まれています。 このチュートリアルでは、2 つのリファクタリング方法を操作します。 変数の名前を変更すると、メソッドを抽出します。 その他のリファクタリング オプションには、フィールドをカプセル化する、メソッド パラメーターにローカル変数の昇格とメソッドのパラメーターの管理が含まれます。 これらのリファクタリング オプションの可用性は、コード内の場所によって異なります。

### <a name="refactoring-code"></a>コードのリファクタリング

リファクタリング一般的 (抽出) を作成するのには、メソッドなどの別のメンバー内にあるコードからメソッドです。 これにより、元のメンバーのサイズを縮小し、抽出したコードを再利用可能な。

チュートリアルのこの部分では、単純なコードを記述し、そこからメソッドを抽出します。 リファクタリングはサポートされて c# の場合は、プログラミング言語として c# を使用するページを作成するようにします。

### <a name="to-extract-a-method-in-a-c-page"></a>C# のページ内のメソッドを抽出するには

1. 切り替える**デザイン**ビュー。
2. **ツールボックス**から、**標準** タブで、ドラッグ、[ボタン](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.aspx)コントロールをページにします。
3. ダブルクリックして、**ボタン**のハンドラーを作成するコントロールをその[ をクリックして](https://msdn.microsoft.com/library/system.web.ui.webcontrols.button.click.aspx)イベント、し、次の強調表示されたコードを追加。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample2.cs?highlight=3-16)]

   このコードを作成、 **ArrayList**オブジェクト、ループを使用して値を持つロードおよび別のループを使用して、内容を表示、 **ArrayList**オブジェクト。
4. キーを押して**ctrl キーを押しながら f5 キーを押して** ページを実行し、をクリックする、**ボタン**次の出力が表示されるかどうかを確認します。   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample3.html)]
5. コード エディターに戻るし、イベント ハンドラーで、次の行を選択します。   

    [!code-html[Main](code-editing-in-web-forms-pages/samples/sample4.html)]
6. 選択範囲を右クリックし、をクリックして**リファクター**を選択し**メソッドの抽出**です。 

    **メソッドの抽出** ダイアログ ボックスが表示されます。
7. **メソッド名を新しい**ボックスに、入力**DisplayArray**、クリックして **[ok]** です。 

    コード エディターがという名前の新しいメソッドを作成`DisplayArray`で新しいメソッドの呼び出しを配置し、 **をクリックして**ループが元のハンドラー。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample5.cs?highlight=12)]
8. キーを押して**ctrl キーを押しながら f5 キーを押して**ページを再度実行し、をクリックする、**ボタン**です。

    ページは以前と同じように、同様に機能します。 `DisplayArray`メソッドが任意の場所からの呼び出しにできるようになりましたページ クラスでします。

## <a name="renaming-variables"></a>変数の名前を変更します。

オブジェクトと同様に、変数を使用する場合は、コードで既に参照されている後に名前を変更することができます。 ただし、変数およびオブジェクトの名前を変更すると、参照のいずれかの名前の変更がない場合、コードが発生することができます。 したがって、リファクタリングを使用名前の変更を実行することができます。

### <a name="to-use-refactoring-to-rename-a-variable"></a>使用してリファクタリングする変数名の変更


1. ** をクリックして**イベント ハンドラー、次の行を探します。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample6.cs)]
2. 変数名を右クリックして`alist`、選択**リファクター**を選択し**の名前を変更**です。

    **の名前を変更** ダイアログ ボックスが表示されます。
3. **新しい名前**ボックスに、入力**ArrayList1**ことを確認し、**参照の変更のプレビュー**  チェック ボックスが選択されています。 次に、 **[OK]** をクリックします。

    **変更のプレビュー**  ダイアログ ボックスが表示され、名前を変更する変数へのすべての参照を含むツリーが表示されます。
4. をクリックして**適用**を閉じる、**変更のプレビュー**  ダイアログ ボックス。

    選択したインスタンスを明示的に参照する変数の名前が変更されます。 ただしを変数`alist`次の行では、名前は変更されません。

    [!code-csharp[Main](code-editing-in-web-forms-pages/samples/sample7.cs)]

    変数`alist`この行の名前は変更されず、変数と同じ値を表していないため`alist`名前を変更しました。 変数`alist`で、`DisplayArray`宣言は、そのメソッドのローカル変数。 これは、リファクタリングを使用して、変数の名前を変更するが、エディターで検索と置換操作を実行するとは異なることは示しています。に対して操作して、変数のセマンティクスの知識を持つ変数を名前変更をリファクタリングします。


## <a name="inserting-snippets"></a>スニペットの挿入

Web フォーム開発者は頻繁に実行する必要がある多くのコーディング作業があるため、コード エディターは、スニペットの場合、または作成済みのコード ブロックのライブラリを提供します。 ページには、これらのスニペットを挿入できます。

Visual Studio で使用する各言語には、コード スニペットを挿入する方法にわずかな違いがあります。 スニペットを挿入する方法については、次を参照してください。 [Visual Basic の IntelliSense コード スニペット](https://msdn.microsoft.com/library/18yz4be4.aspx)です。 スニペットを挿入するには Visual C# の場合については、次を参照してください。 [Visual c# のコード スニペット](https://msdn.microsoft.com/library/z41h7fat.aspx)です。

## <a name="next-steps"></a>次の手順

このチュートリアルでは、コード内のエラーを修正する、コードのリファクタリング、変数の名前を変更する、コードにコード スニペットを挿入するの Visual Studio 2010 のコード エディターの基本的な機能を説明しました。 エディターで追加された機能は、アプリケーションの開発を高速で簡単になります。 たとえば、次の操作を行います。

- IntelliSense オプションの変更、コード スニペットの管理、およびコード スニペットをオンラインで検索などの IntelliSense の機能について説明します。 詳細については、「[IntelliSense の使用](https://msdn.microsoft.com/library/hcw1s69b.aspx)」を参照してください。
- 独自のコード スニペットを作成する方法を説明します。 詳細については、次を参照してください[の作成と使用の IntelliSense コード スニペット。](https://msdn.microsoft.com/library/ms165392.aspx)
- IntelliSense コード スニペットでは、スニペットをカスタマイズして、トラブルシューティングなどの Visual Basic 固有の機能について説明します。 詳細については、次を参照してください[Visual Basic の IntelliSense コード スニペット。](https://msdn.microsoft.com/library/18yz4be4.aspx)
- 詳細については、c# のリファクタリングとコード スニペットなど、IntelliSense の特定の機能です。 詳細については、次を参照してください。 [Visual c# IntelliSense](https://msdn.microsoft.com/library/43f44291.aspx)です。
