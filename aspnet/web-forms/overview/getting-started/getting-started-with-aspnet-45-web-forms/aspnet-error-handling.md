---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET エラー処理 |Microsoft Docs
author: Erikre
description: このチュートリアル シリーズでは、私たちの ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
ms.date: 09/08/2014
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 8d6c19be7c77079b870261d1c4cf0ea62e0e2fd6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807591"
---
<a name="aspnet-error-handling"></a>ASP.NET エラー処理
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) をダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアル シリーズでは、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)このチュートリアル シリーズをと共に使用できます。


このチュートリアルでは、エラー処理およびエラーのログ記録を含む Wingtip Toys のサンプル アプリケーションを変更します。 エラー処理により、アプリケーションを適切にエラーを処理し、それに従ってエラー メッセージを表示します。 エラーのログ記録は検出と発生したエラーを修正できます。 このチュートリアルでは、「URL ルーティング」前のチュートリアルに基づいており、Wingtip Toys のチュートリアル シリーズのパートです。

## <a name="what-youll-learn"></a>学習内容。

- グローバル エラー処理アプリケーションの構成に追加する方法。
- エラー処理アプリケーション、ページ、およびコードのレベルを追加する方法。
- 後で確認エラーを記録する方法。
- セキュリティが低下しないエラー メッセージを表示する方法。
- エラー ログのモジュールとハンドラー (ELMAH) エラーのログ記録を実装する方法。

## <a name="overview"></a>概要

ASP.NET アプリケーションでは、一貫した方法で実行中に発生するエラーを処理できる必要があります。 ASP.NET では、統一された方法でアプリケーションのエラーを通知するための手段を提供する共通言語ランタイム (CLR) を使用します。 エラーが発生する例外がスローされます。 すべてのエラー、条件、またはアプリケーションが発生した予期しない動作は例外です。

.NET Framework では、例外は `System.Exception` クラスから継承されるオブジェクトです。 例外は問題が発生したコード領域からスローされます。 例外は、アプリケーション例外を処理するコードを提供している場所にコール スタックの上に渡されます。 アプリケーションが例外を処理していない場合は、ブラウザーを強制的にエラーの詳細を表示します。

ベスト プラクティスとしては、コード レベルでのエラーを処理`Try` / `Catch` / `Finally`コード内でブロックします。 ユーザーが発生したコンテキストでの問題を修正できるように、これらのブロックを配置しようとしてください。 エラー処理のブロックが、エラーが発生したから離れすぎている場合に、問題の解決に必要な情報をユーザーに提供することが困難になります。

### <a name="exception-class"></a>例外クラス

例外クラスは、例外の継承元となる基本クラスです。 ほとんどの例外オブジェクトは、例外クラスの派生クラスのインスタンスなど、`SystemException`クラス、`IndexOutOfRangeException`クラス、または`ArgumentNullException`クラス。 例外クラスなどのプロパティがあります、`StackTrace`プロパティ、`InnerException`プロパティ、および`Message`プロパティは、発生したエラーに関する特定の情報を提供します。

### <a name="exception-inheritance-hierarchy"></a>例外の継承階層

ランタイムに起因する例外の基本セットを`SystemException`ランタイムは、例外が発生した場合にスローするクラス。 など、例外クラスから継承するクラスのほとんどの`IndexOutOfRangeException`クラスおよび`ArgumentNullException`クラス、その他のメンバーを実装していません。 そのため、例外の最も重要な情報は、例外、例外の名前と、例外に含まれる情報の階層で見つかんだことができます。

### <a name="exception-handling-hierarchy"></a>例外処理の階層

ASP.NET Web フォーム アプリケーションでは、特定の処理の階層に基づく例外を処理することができます。 次のレベルでは、例外を処理できます。

- アプリケーション レベル
- ページ レベル
- コード レベル

アプリケーションでは、例外を処理するときに例外クラスから継承される例外に関する追加情報多くの場合、取得でき、ユーザーに表示されます。 に加えて、アプリケーション、ページ、およびコード レベルでは、HTTP モジュール レベルでは、IIS のカスタム ハンドラーを使用して例外を処理することもできます。

### <a name="application-level-error-handling"></a>アプリケーション レベルのエラー処理

アプリケーション レベルで既定のエラーを処理するには、アプリケーションの構成を変更または追加することで、`Application_Error`ハンドラーで、 *Global.asax*アプリケーションのファイル。

追加することで、既定のエラーと HTTP エラーを処理することができます、`customErrors`セクションを*Web.config*ファイル。 `customErrors`セクションでは、ユーザーは、エラーが発生したときにリダイレクトされる既定のページを指定することができます。 また、個々 の特定のステータス コードのエラー ページを指定することもできます。

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

残念ながら、別のページにユーザーをリダイレクトする、構成を使用する場合は、発生したエラーの詳細を必要は。

コードを追加して、アプリケーションで任意の場所で発生したエラーをトラップするただし、`Application_Error`ハンドラーで、 *Global.asax*ファイル。

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>ページ レベルのエラー イベントの処理

ページ レベルのハンドラーでは、エラーが発生したため、コントロールのインスタンスは保持されませんが、ページにユーザーを返しますには不要になったものページ。 アプリケーションのユーザーに、エラーの詳細を提供するには、具体的にはページにエラーの詳細を記述する必要があります。

通常、ハンドルされないエラーをログに、または、ユーザーに役立つ情報を表示できるページに移動するは、ページ レベルのエラー ハンドラーを使用します。

このコード例では、ASP.NET Web ページでエラー イベントのハンドラーを示します。 このハンドラー内で既に処理されていないすべての例外をキャッチする`try` / `catch`  ページをブロックします。

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

エラーを処理した後に呼び出してクリアする必要があります、`ClearError`サーバー オブジェクトのメソッド (`HttpServerUtility`クラス)、それ以外の場合、以前発生したエラーが表示されます。

### <a name="code-level-error-handling"></a>コードのレベルのエラー処理

Try-catch ステートメントから成る 1 が続く try ブロックまたは以上の catch 句で、さまざまな例外ハンドラーを指定します。 例外がスローされたときに、共通言語ランタイム (CLR) は、この例外を処理する catch ステートメントを検索します。 現在実行中のメソッドに catch ブロックが含まれていない場合、CLR は、現在のメソッドというように、コール スタックの上に呼び出されるメソッドで検索します。 Catch ブロックが見つからない場合、CLR は、ユーザーにハンドルされない例外メッセージを表示し、プログラムの実行を停止します。

次のコード例は、一般的な使用方法を示しています。 `try` / `catch` / `finally`エラーを処理します。

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

上記のコードでは、try ブロックには、可能性のある例外から保護する必要があるコードが含まれています。 例外がスローされたか、ブロックが正常に完了するまで、ブロックが実行されます。 どちらの場合、`FileNotFoundException`例外または`IOException`例外が発生した、実行が別のページに転送します。 含まれるコードし、ブロックが最後に実行、エラーが発生したかどうかどうか。

## <a name="adding-error-logging-support"></a>エラー ログ記録のサポートを追加します。

Wingtip Toys のサンプル アプリケーションにエラー処理を追加する前に追加してエラーのログ記録のサポートを追加するが、`ExceptionUtility`クラスを*ロジック*フォルダー。 これにより、アプリケーションがエラーを処理するたびに、エラー ログ ファイルにエラーの詳細が追加されます。

1. 右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **コード**左側のテンプレート グループ。 次に、選択**クラス**中央から一覧表示し、名前を**ExceptionUtility.cs**します。
3. **[追加]** をクリックします。 新しいクラス ファイルが表示されます。
4. 既存のコードを次のコードに置き換えます。  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

例外を呼び出すことによって例外のログ ファイルに書き込むことが例外が発生したときに、`LogException`メソッド。 このメソッドは、2 つのパラメーター、例外オブジェクトと、例外の原因に関する詳細を含む文字列を受け取ります。 例外のログを書き込む、 *ErrorLog.txt*ファイル、*アプリ\_データ*フォルダー。

### <a name="adding-an-error-page"></a>エラー ページを追加します。

Wingtip Toys のサンプル アプリケーションでエラーを表示する 1 つのページが使用されます。 エラー ページは、サイトのユーザーにセキュリティで保護されたエラー メッセージを表示する設計されています。 ただし、ユーザーが、コードが存在するコンピューターのローカルで提供される HTTP 要求を行う開発者の場合は、エラー ページに追加のエラーの詳細が表示されます。

1. プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**追加** - &gt; **新しい項目の**.   
   **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレート グループ。 真ん中の一覧から選択**マスター ページを使用した Web フォーム**、名前を付けます**ErrorPage.aspx**します。
3. **[追加]** をクリックします。
4. 選択、 *Site.Master*ファイルをマスター ページとして選び、 **OK**。
5. 次のように既存のマークアップに置き換えます。   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. 分離コードの既存のコードに置き換えます (*ErrorPage.aspx.cs*) 次のように表示されるようにします。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

エラー ページが表示されたら、`Page_Load`イベント ハンドラーが実行されます。 `Page_Load`ハンドラーでエラーが処理された最初の場所が決定されます。 呼び出しによって発生した最後のエラーを確認し、`GetLastError`サーバー オブジェクトのメソッド。 例外が存在しない場合は、汎用的な例外が作成されます。 次に、HTTP 要求がローカルで行われたすべてのエラーの詳細が表示されます。 この場合、これらのエラーの詳細、web アプリケーションを実行するローカル コンピューターのみが表示されます。 エラー情報を表示した後、エラーがログ ファイルに追加し、サーバーからエラーをクリアします。

### <a name="displaying-unhandled-error-messages-for-the-application"></a>アプリケーションの未処理のエラー メッセージを表示します。

追加することで、`customErrors`セクションを*Web.config*ファイル、アプリケーション全体で発生する単純なエラー迅速に処理できます。 404 - ファイルが見つかりません。 など、状態コード値に基づいてエラーを処理する方法を指定することもできます。

#### <a name="update-the-configuration"></a>構成を更新します

追加することで、構成の更新を`customErrors`セクションを*Web.config*ファイル。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Web.config* Wingtip Toys のサンプル アプリケーションのルートにあるファイル。
2. 追加、`customErrors`セクションを*Web.config*ファイル内で、`<system.web>`ノードとして次のとおりです。   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. 保存、 *Web.config*ファイル。

`customErrors`セクションが"On"に設定されていると、モードを指定します。 指定します、 `defaultRedirect`、アプリケーション エラーが発生したときに移動するには、どのページを指定します。 さらに、ページが見つからない場合に、404 エラーを処理する方法を指定する特定のエラー要素を追加しました。 このチュートリアルの後半では、追加のエラー処理は、アプリケーション レベルのエラーの詳細をキャプチャを追加します。

#### <a name="running-the-application"></a>アプリケーションの実行

更新されたルートを表示するようになりましたアプリケーションを実行することができます。

1. キーを押して**F5** Wingtip Toys のサンプル アプリケーションを実行します。  
 ブラウザーが開きを示しています、 *Default.aspx*ページ。
2. ブラウザーに次の URL を入力してください (を使用してください、 **ポ** ート番号)。  
    `https://localhost:44300/NoPage.aspx`
3. レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラー処理 - ページが見つからないというエラー](aspnet-error-handling/_static/image1.png)

要求したときに、 *NoPage.aspx*  ページで、存在していない、エラー ページに表示されます、単純なエラー メッセージと詳細なエラー情報の詳細は、使用可能な場合。 ただし、ユーザーには、リモートの場所からの存在しないページ機能が要求された場合、エラー ページはのみ、エラー メッセージ赤で表示されます。

### <a name="including-an-exception-for-testing-purposes"></a>など、例外の目的でのテスト

発生したときにエラーに、アプリケーションがどのように機能することを確認する、ASP.NET でエラー条件を意図的に作成できます。 Wingtip Toys のサンプル アプリケーションでは、何が起こるかを確認する既定のページの読み込み時にテスト例外がスローされます。

1. 分離コードを開いて、 *Default.aspx* Visual Studio でのページ。   
   *Default.aspx.cs*分離コード ページが表示されます。
2. `Page_Load`ハンドラー、ハンドラーは次のように表示されるようにコードを追加します。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

さまざまな種類の例外を作成することになります。 上記のコードで作成する、`InvalidOperationException`ときに、 *Default.aspx*ページが読み込まれます。

#### <a name="running-the-application"></a>アプリケーションの実行

アプリケーションが例外を処理する方法を表示するアプリケーションを実行することができます。

1. キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。  
 アプリケーションでは、InvalidOperationException がスローされます。 

    > [!NOTE] 
    > 
    > キーを押す必要があります**CTRL + F5** Visual Studio で、エラーの原因を表示するコードを損なうことがなくページを表示します。
2. レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラーの処理 - エラー ページ](aspnet-error-handling/_static/image2.png)

によって、例外がトラップされたエラーの詳細をご覧のとおり、`customError`セクション、 *Web.config*ファイル。

### <a name="adding-application-level-error-handling"></a>アプリケーション レベルのエラー処理を追加します。

使用して例外をトラップではなく、`customErrors`セクション、 *Web.config*ファイルで、ほとんどの例外情報を取得すると、場所は、アプリケーション レベルでエラーをトラップしてエラーの詳細を取得します。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Global.asax.cs*ファイル。
2. 追加、**アプリケーション\_エラー**ハンドラーことが次のように表示されるようにします。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

アプリケーションでは、エラーが発生したときに、`Application_Error`ハンドラーが呼び出されます。 このハンドラーでは、最後の例外が取得され、レビューします。 例外はハンドルされませんでした。 例外には、内部例外の詳細が含まれている場合 (つまり、`InnerException`が null でない)、アプリケーションは、例外の詳細を表示する場所のエラー ページに実行を転送します。

#### <a name="running-the-application"></a>アプリケーションの実行

アプリケーション レベルで例外を処理することによって提供される追加のエラーの詳細を表示するアプリケーションを実行することができます。

1. キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。  
 アプリケーションによってスロー、`InvalidOperationException`します。
2. レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラー処理のアプリケーション レベルのエラー](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>ページ レベルのエラー処理を追加します。

追加できますページ レベルのエラー処理ページを追加することを使用するか、`ErrorPage`属性を`@Page`または追加することで、ページのディレクティブを`Page_Error`ページの分離コードにイベント ハンドラー。 このセクションでは追加、`Page_Error`転送を実行するイベント ハンドラー、 *ErrorPage.aspx*ページ。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Default.aspx.cs*ファイル。
2. 追加、`Page_Error`ハンドラーと分離コードが表示されるように従います。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

ページで、エラーが発生したときに、`Page_Error`イベント ハンドラーが呼び出されます。 このハンドラーでは、最後の例外が取得され、レビューします。 場合、`InvalidOperationException`が発生した、`Page_Error`イベント ハンドラーは例外の詳細を表示する場所のエラー ページに実行を転送します。

#### <a name="running-the-application"></a>アプリケーションの実行

更新されたルートを表示するようになりましたアプリケーションを実行することができます。

1. キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。  
 アプリケーションによってスロー、`InvalidOperationException`します。
2. レビュー、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラーの処理 - ページ レベルのエラー](aspnet-error-handling/_static/image4.png)
3. ブラウザー ウィンドウを閉じます。

### <a name="removing-the-exception-used-for-testing"></a>テストに使用される例外を削除します。

このチュートリアルで先ほど追加した例外をスローせずに機能する Wingtip Toys のサンプル アプリケーションを許可するのには、例外を削除します。

1. 分離コードを開いて、 *Default.aspx*ページ。
2. `Page_Load`ハンドラー、ハンドラーは次のように表示されるように、例外をスローするコードを削除します。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>コード レベルのエラー ログ機能の追加

このチュートリアルの前半で説明したように、コードのセクションを実行し、最初に発生するエラーを処理しようとする try/catch ステートメントを追加できます。 この例ではのみ記述するエラーの詳細エラー ログ ファイルにエラーは、後で確認できるように。

1. **ソリューション エクスプ ローラー**の*ロジック*フォルダー、検索、オープン、 *PayPalFunctions.cs*ファイル。
2. 更新プログラム、`HttpCall`メソッドとして、コードが表示されるように従います。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

上記のコードの呼び出し、`LogException`メソッドに含まれている、`ExceptionUtility`クラス。 追加した、 *ExceptionUtility.cs*クラス ファイルを*ロジック*このチュートリアルで前にフォルダー。 `LogException` は、次の 2 つのパラメーターを受け取ります。 最初のパラメーターは、例外オブジェクトです。 2 番目のパラメーターは、エラーの原因の認識に使用される文字列です。

### <a name="inspecting-the-error-logging-information"></a>エラー ログの情報を調べる

前述のように、どのエラー、アプリケーションでは、最初に修正する必要がありますを決定するのに、エラー ログを使用できます。 もちろん、トラップされ、エラー ログに記録されているエラーのみが記録されます。

1. **ソリューション エクスプ ローラー**を検索して開く、 *ErrorLog.txt*ファイル、*アプリ\_データ*フォルダー。   
 選択する必要があります、"**すべてのファイル**"オプション、または"**更新**"オプションの先頭から**ソリューション エクスプ ローラー**を参照してください、 *ErrorLog.txt*ファイル。
2. Visual Studio に表示されるエラー ログを確認します。 

    ![ASP.NET エラーの処理 - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>安全なエラー メッセージ

**に注意して**こと、アプリケーションでは、エラー メッセージが表示されたら、それを提供しないように離れた悪意のあるユーザーがアプリケーションを攻撃するに役立つ情報が。 たとえば、アプリケーションは、失敗したデータベースへ書き込みを試みるを使用しているユーザー名を含む、エラー メッセージが表示されません。 このため、ユーザーに赤で一般的なエラー メッセージが表示されます。 すべての追加のエラーの詳細は、ローカル コンピューター上の開発者にのみ表示されます。

## <a name="using-elmah"></a>ELMAH を使用します。

ELMAH (Error Logging Modules and ハンドラー) は、NuGet パッケージとして、ASP.NET アプリケーションに接続するエラー ログ機能です。 ELMAH は、次の機能を提供します。

- 未処理の例外のログを記録します。
- 記録された未処理の例外の全体のログを表示する web ページ。
- それぞれの完全な詳細を表示する web ページには、例外が記録されます。
- 電子メールは、各エラーの発生時に通知します。
- 最後の 15 のエラー ログからの RSS フィードです。

ELMAH を使用することができます、前にインストールする必要があります。 これは簡単なを使用して、 *NuGet*パッケージ インストーラー。 このチュートリアル シリーズで前に述べた、NuGet は、Visual Studio 拡張機能をインストールし、オープン ソース ライブラリと Visual Studio のツールを更新するが容易にします。

1. Visual Studio 内から、**ツール**メニューの **ライブラリ パッケージ マネージャー**  - &gt; **ソリューションの NuGet パッケージの管理**します。 

    ![ASP.NET エラー処理 - ソリューションの NuGet パッケージの管理](aspnet-error-handling/_static/image6.png)
2. **NuGet パッケージの管理**Visual Studio 内でダイアログ ボックスが表示されます。
3. **NuGet パッケージの管理**] ダイアログ ボックスで、展開**オンライン**左側で、し、[ **nuget.org**します。次に、検索し、インストール、 **ELMAH**利用可能なパッケージをオンラインの一覧からパッケージ。 

    ![ASP.NET エラーの処理 - ELMA NuGet パッケージ](aspnet-error-handling/_static/image7.png)
4. パッケージをダウンロードするインターネットに接続する必要があります。
5. **プロジェクトの選択** ダイアログ ボックスで、確認、 **WingtipToys**選択範囲が選択されているし、順にクリックします**ok**します。 

    ![ASP.NET エラー処理 - プロジェクト ダイアログ ボックスを選択します。](aspnet-error-handling/_static/image8.png)
6. クリックして**閉じる**で**NuGet パッケージの管理** ダイアログ ボックスが必要な場合。
7. Visual Studio では、開いているファイルを再読み込みすることを要求する場合は、選択"**すべてはい**"。
8. ELMAH パッケージ自体でのエントリを追加する、 *Web.config*プロジェクトのルートにあるファイル。 Visual Studio メッセージに表示されます、変更を再読み込みする*Web.config*ファイルで、をクリックして**はい**。

ELMAH は、発生する未処理のエラーを格納する準備ができました。

### <a name="viewing-the-elmah-log"></a>ELMAH ログの表示

ELMAH ログの表示は簡単ですが最初に、ELMAH は、ログに記録した未処理の例外を作成します。

1. キーを押して**CTRL + F5** Wingtip Toys のサンプル アプリケーションを実行します。
2. 未処理の例外をログに書き込む、ELMAH、(実際のポート番号を使用して) 次の URL をブラウザーに移動します。  
    `https://localhost:44300/NoPage.aspx` エラー ページが表示されます。
3. ELMAH のログを表示するには、(実際のポート番号を使用して) 次の URL をブラウザーで移動します。  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET エラーの処理 - ELMAH エラー ログ](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、アプリケーション レベル、ページ レベル、およびコード レベルでのエラーの処理について学習しました。 後で確認の処理済みおよび未処理のエラー ログに記録する方法についても説明しました。 例外のログ記録と NuGet を使用して、アプリケーションに通知を提供する、ELMAH ユーティリティを追加しました。 さらに、安全なエラー メッセージの重要性について説明しました。

## <a name="tutorial-series-conclusion"></a>チュートリアル シリーズのまとめ

*次に沿ったいただきありがとうございます。この一連のチュートリアルでは、ASP.NET Web フォームを使い慣れてきたら役立つと思います。詳細については、Web フォームの機能 ASP.NET 4.5 と Visual Studio 2013 で使用可能な場合を参照してください* [*ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート*](../../../../visual-studio/overview/2013/release-notes.md)*.また、必ずで説明したチュートリアルを見て、* ***次の手順****セクションと defintely お試し、* [*無料試用版の Azure*](https://azure.microsoft.com/pricing/free-trial/)*.*

![ありがとうございます - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>次の手順

Microsoft Azure に web アプリケーションのデプロイについてを参照してください[をセキュリティで保護された ASP.NET Web フォーム アプリのメンバーシップ、OAuth、SQL Database と Azure Web サイトへのデプロイ](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)します。

## <a name="free-trial"></a>無料試用版

[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)  
 Microsoft Azure に web サイトを発行節約メンテナンスの時間とコスト。 Web アプリを Azure にデプロイする簡単なプロセスです。 維持し、web アプリを監視する必要がある場合、Azure は、さまざまなツールとサービスを提供します。 データ、トラフィック、id、メッセージング、メディア、およびパフォーマンスを Azure でのバックアップを管理します。 また、非常にコスト効率に優れたアプローチですべて提供されます。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET 状態監視によるエラーの詳細をログ記録](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>謝辞

このチュートリアル シリーズのコンテンツに多大な貢献を行った以下の方々 に感謝したいと思います。

- [Alberto Poblacion、MVP &amp; MCT、スペイン](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen (オランダ)](http://blog.alexthissen.nl/) (twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier 州 (米国)](http://andret503.wordpress.com/)
- Apurva Joshi、Microsoft
- [Bojan Vrhovnik, スロベニア](http://twitter.com/bvrhovnik)
- [Bruno Sonnino (ブラジル)](http://msmvps.com/blogs/bsonnino) (twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos、ブラジル](http://www.carloscds.net/)
- [Dave Campbell、USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway, Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael シャープ、USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike 教皇
- [Mitchel 販売者、USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba、Microsoft](http://linqto.me/Links/pcociuba)
- [Paulo Morgado (ポルトガル)](http://paulomorgado.net/)
- [Pranav Rastogi、Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann、Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra、Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>コミュニティへの投稿

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
  Visual Studio 2012 に関連する msdn コード サンプル: [Wingtip Toys のナビゲーション](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
  Visual Studio 2012 に関連する msdn コード サンプル: [Visual Basic で ASP.NET 4.5 Web フォームのチュートリアル シリーズ](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - Microsoft テクニカル対象ユーザーの共同作成者 (twitter: @driazevedo)  
  Visual Studio 2012 の翻訳: [Iniciando com Visão Geral ASP.NET Web フォームの 4.5 - Parte 1 - Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

> [!div class="step-by-step"]
> [前へ](url-routing.md)
