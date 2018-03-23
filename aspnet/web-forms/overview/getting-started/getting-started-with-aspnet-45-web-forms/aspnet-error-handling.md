---
uid: web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
title: ASP.NET エラー処理 |Microsoft ドキュメント
author: Erikre
description: このチュートリアルの系列では、お用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/08/2014
ms.topic: article
ms.assetid: 423498f7-1a4b-44a1-b342-5f39d0bcf94f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/aspnet-error-handling
msc.type: authoredcontent
ms.openlocfilehash: 3f732ae6f1b7845bcae88912b4a4fe26574c10de
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="aspnet-error-handling"></a>ASP.NET エラー処理
====================
によって[Erik Reitan](https://github.com/Erikre)

[Wingtip Toys のサンプル プロジェクト (c#) をダウンロード](http://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)または[電子書籍 (PDF) のダウンロード](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20ASP.NET%204.5%20Web%20Forms%20and%20Visual%20Studio%202013.pdf)

> このチュートリアルの系列では、Web 用 ASP.NET 4.5 と Microsoft Visual Studio Express 2013 を使用して ASP.NET Web フォーム アプリケーションの構築の基礎を説明します。 Visual Studio 2013[プロジェクトと c# ソース コード](https://go.microsoft.com/fwlink/?LinkID=389434&clcid=0x409)はこのチュートリアルのシリーズに付随する使用できます。


このチュートリアルでは、エラー処理およびエラーのログ記録を含めて、Wingtip Toys サンプル アプリケーションを変更します。 エラー処理により、アプリケーションを正常にエラーを処理し、それに従ってエラー メッセージを表示できます。 エラーのログ記録を使用すると、検出し、発生したエラーを修正できます。 このチュートリアルでは、「URL ルーティング」前のチュートリアルを上に構築され、Wingtip Toys チュートリアル シリーズの一部です。

## <a name="what-youll-learn"></a>学習する内容。

- グローバルにエラー処理アプリケーションの構成を追加する方法。
- エラー処理アプリケーション、ページ、およびコードのレベルを追加する方法。
- 後で確認できるエラーを記録する方法。
- セキュリティが脅かされないエラー メッセージを表示する方法。
- エラー ログ モジュールとハンドラー (ELMAH) エラーのログ記録を実装する方法。

## <a name="overview"></a>概要

ASP.NET アプリケーションでは、一貫した方法で実行中に発生したエラーを処理する必要があります。 ASP.NET では、一貫した方法で、エラーのアプリケーションに通知する方法を提供する共通言語ランタイム (CLR) を使用します。 エラーが発生するときに例外がスローされます。 例外は、任意のエラー、条件、またはアプリケーションが予期しない動作です。

.NET Framework では、例外は `System.Exception` クラスから継承されるオブジェクトです。 例外は問題が発生したコード領域からスローされます。 例外は、アプリケーションが例外を処理するコードを提供する場所の場所へ呼び出しスタックに渡されます。 アプリケーションで例外が処理されないエラーの詳細を表示する、ブラウザーが強制されます。

ベスト プラクティスとして、レベルのコードでエラーを処理`Try` / `Catch` / `Finally`コード内にブロックします。 ユーザーが行われるコンテキストの問題を修正できるように、これらのブロックを配置しようとしてください。 エラー処理のブロックが、エラーが発生したから離れすぎている場合に、問題の解決に必要な情報をユーザーに提供することが困難になります。

### <a name="exception-class"></a>例外クラス

例外クラスは、例外の継承元となる基本クラスです。 ほとんどの例外オブジェクトは、例外クラスの派生クラスのインスタンスなど、`SystemException`クラス、`IndexOutOfRangeException`クラス、または`ArgumentNullException`クラスです。 例外クラスなどのプロパティがあります、 `StackTrace` 、プロパティ、`InnerException`プロパティ、および`Message`が発生したエラーに関する情報を提供するプロパティです。

### <a name="exception-inheritance-hierarchy"></a>例外の継承階層

ランタイムはから派生した例外の基本セットを`SystemException`クラス、例外が発生したときに、ランタイムをスローします。 などの例外クラスから継承するクラスのほとんどの`IndexOutOfRangeException`クラスおよび`ArgumentNullException`クラス、追加のメンバーを実装していません。 そのため、例外の最も重要な情報は含まれて、階層の例外、例外の名前と、例外に含まれる情報です。

### <a name="exception-handling-hierarchy"></a>例外処理の階層構造

ASP.NET Web フォーム アプリケーションでは、特定の処理階層に基づいて例外を処理することができます。 例外は、次のレベルで処理できます。

- アプリケーション レベル
- ページ レベル
- コードのレベル

アプリケーションでは、例外を処理するときに例外クラスから継承される例外に関する追加情報多くの場合、取得して、ユーザーに表示されます。 アプリケーション、ページ、およびコード レベル、だけでなく、HTTP モジュール レベルでと IIS のカスタム ハンドラーを使用して、例外も処理できます。

### <a name="application-level-error-handling"></a>アプリケーション レベルのエラー処理

アプリケーション レベルで既定のエラーを処理するには、アプリケーションの構成を変更することによって、または追加することによって、`Application_Error`ハンドラー、 *Global.asax*アプリケーションのファイルです。

追加することで、既定のエラーと HTTP エラーを処理することができます、`customErrors`セクション、 *Web.config*ファイル。 `customErrors`セクションでは、エラーが発生したときにそのユーザーにリダイレクトされる既定のページを指定することができます。 また、個々 のページの特定の状態コード エラーを指定することもできます。

[!code-xml[Main](aspnet-error-handling/samples/sample1.xml?highlight=3-5)]

残念ながら、構成を使用して別のページにユーザーをリダイレクトするときにありませんが発生したエラーの詳細。

コードを追加して、アプリケーションで任意の場所で発生したエラーをトラップするただし、`Application_Error`ハンドラー、 *Global.asax*ファイル。

[!code-csharp[Main](aspnet-error-handling/samples/sample2.cs)]

### <a name="page-level-error-event-handling"></a>ページ レベルのエラー イベントの処理

ページ レベルのハンドラーで返される、ユーザーのページに、エラーが発生した場所が、コントロールのインスタンスは変更されないため、存在しなくなるもの ページでします。 アプリケーションのユーザーに、エラーの詳細を提供するには、具体的には、ページにエラーの詳細を記述する必要があります。

通常、有用な情報を表示できるページにユーザーを移動または未処理のエラーをログには、ページ レベルのエラー ハンドラーを使用します。

このコード例は、ASP.NET Web ページで、エラー イベントのハンドラーを示します。 このハンドラーは内で既に処理されていないすべての例外をキャッチ`try` / `catch`ページ内のブロックです。

[!code-csharp[Main](aspnet-error-handling/samples/sample3.cs)]

エラーを処理した後に呼び出すことによってクリアする必要があります、`ClearError`サーバー オブジェクトのメソッド (`HttpServerUtility`クラス)、それ以外の場合、以前に発生したエラーが表示されます。

### <a name="code-level-error-handling"></a>コードのレベルのエラー処理

Try-catch ステートメント、try ブロックの後に 1 つから成るまたは以上の catch 句で、さまざまな例外のハンドラーを指定します。 例外がスローされると、共通言語ランタイム (CLR) は、この例外を処理する catch ステートメントを検索します。 現在実行中のメソッドに catch ブロックが含まれていない場合、CLR は、現在のメソッドと呼び出し履歴の上に、呼び出されるメソッドを検索します。 Catch ブロックが見つからない、CLR をユーザーにハンドルされない例外メッセージを表示して、プログラムの実行を停止します。

次のコード例を使用する一般的な方法を示しています。 `try` / `catch` / `finally`エラーを処理します。

[!code-csharp[Main](aspnet-error-handling/samples/sample4.cs)]

上記のコードでは、try ブロックには、可能性のある例外に対して保護されている必要があるコードが含まれています。 例外がスローされるか、ブロックが正常に完了するまで、ブロックが実行されます。 どちらの場合、`FileNotFoundException`例外または`IOException`例外が発生する、実行が別のページに転送します。 含まれるコード、finally ブロックが実行される、エラーが発生したかどうかどうか。

## <a name="adding-error-logging-support"></a>エラー ログのサポートを追加します。

Wingtip Toys のサンプル アプリケーションにエラー処理を追加する前に、追加することでエラーのログ記録のサポートを追加するが、`ExceptionUtility`クラスを*ロジック*フォルダーです。 これにより、アプリケーションがエラーを処理するたびに、エラーの詳細をエラー ログ ファイルに追加されます。

1. 右クリックし、*ロジック*クリックしてフォルダー**追加** - &gt; **新しい項目の**します。   
 **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **コード**左側のテンプレートのグループです。 次に、選択**クラス**中央から一覧表示し、名前を付けます**ExceptionUtility.cs**です。
3. **[追加]** をクリックします。 新しいクラス ファイルが表示されます。
4. 既存のコードを次のコードに置き換えます。  

    [!code-csharp[Main](aspnet-error-handling/samples/sample5.cs)]

例外が発生するときに例外を書き込む例外ログ ファイルを呼び出すことによって、`LogException`メソッドです。 このメソッドは、2 つのパラメーター、例外オブジェクトおよび例外の原因に関する詳細を含む文字列を受け取ります。 例外のログを書き込む、 *ErrorLog.txt*ファイルで、*アプリ\_データ*フォルダーです。

### <a name="adding-an-error-page"></a>エラー ページを追加します。

Wingtip Toys サンプル アプリケーションでは、エラーを表示する 1 つのページが使用されます。 エラー ページはサイトのユーザーにセキュリティで保護されたエラー メッセージを表示するよう設計されています。 ただし、ユーザーが、コードが存在するコンピューターにローカルで提供される HTTP 要求を行う開発者の場合は、エラー ページに追加のエラーの詳細が表示されます。

1. プロジェクト名を右クリックして (**Wingtip Toys**) で**ソリューション エクスプ ローラー**選択**追加** - &gt; **新しい項目の**.   
 **[新しい項目の追加]** ダイアログ ボックスが表示されます。
2. 選択、 **Visual c#**  - &gt; **Web**左側のテンプレートのグループです。 中央のリストから選択**マスター ページを含む Web フォーム**、名前を付けます**ErrorPage.aspx**です。
3. **[追加]**をクリックします。
4. 選択、 *Site.Master*クリックしてファイルをマスター ページとして**OK**です。
5. 既存のマークアップを次のように置き換えます。   

    [!code-aspx[Main](aspnet-error-handling/samples/sample6.aspx)]
6. 分離コードの既存のコードに置き換えます (*ErrorPage.aspx.cs*) 次のように表示されるようにします。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample7.cs)]

エラー ページが表示されたら、`Page_Load`イベント ハンドラーが実行されます。 `Page_Load`ハンドラーでエラーが処理された最初の場所を特定します。 次に、最後のエラーが発生したは、呼び出しによって決定されます、`GetLastError`サーバー オブジェクトのメソッドです。 例外が存在しない場合は、汎用的な例外が作成されます。 次に、HTTP 要求がローカルで行われたすべてのエラーの詳細が表示されます。 この場合、web アプリケーションを実行して、ローカル コンピューターだけでは、これらのエラーの詳細が表示されます。 エラー情報が表示された後に、エラーがログ ファイルに追加し、サーバーから、エラーをクリアします。

### <a name="displaying-unhandled-error-messages-for-the-application"></a>アプリケーションの未処理のエラー メッセージを表示します。

追加することによって、`customErrors`セクション、 *Web.config*ファイル、迅速にエラーを処理する単純なアプリケーション全体で発生します。 404 - ファイルが見つかりません。 など、状態コード値に基づいてエラーを処理する方法を指定することもできます。

#### <a name="update-the-configuration"></a>構成を更新します。

追加することで、構成を更新、`customErrors`セクション、 *Web.config*ファイル。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Web.config* Wingtip Toys のサンプル アプリケーションのルートにあるファイルです。
2. 追加、`customErrors`セクション、 *Web.config*ファイルの場所、`<system.web>`次のようにノード。   

    [!code-xml[Main](aspnet-error-handling/samples/sample8.xml?highlight=3-5)]
3. 保存、 *Web.config*ファイル。

`customErrors`セクションが「オン」に設定されているモードを指定します。 また、指定、`defaultRedirect`エラーが発生したときに移動するページ、アプリケーションに通知します。 さらに、ページが見つからない場合に、404 エラーを処理する方法を指定する特定のエラー要素を追加しました。 このチュートリアルの後半では、追加のエラーを処理では、アプリケーション レベルでのエラーの詳細情報をキャプチャを追加します。

#### <a name="running-the-application"></a>アプリケーションの実行

今すぐ更新済みのルートを表示するアプリケーションを実行することができます。

1. キーを押して**f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。  
 ブラウザーが開き、表示、 *Default.aspx*ページ。
2. ブラウザーに次の URL を入力してください (を使用してください**、**ポート番号)。  
    `https://localhost:44300/NoPage.aspx`
3. 確認、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラー処理のページには見つからないエラー](aspnet-error-handling/_static/image1.png)

要求すると、 *NoPage.aspx*  ページで、存在しない、エラー ページに表示されます、単純なエラー メッセージと詳細なエラー情報の詳細は、使用可能な場合です。 ただし場合は、ユーザーは、リモートの場所から、存在しないページを要求した、エラー ページがのみ、エラー メッセージ赤で表示します。

### <a name="including-an-exception-for-testing-purposes"></a>テスト目的での例外を含む

発生したときにエラーに、アプリケーションがどのように機能することを確認する、ASP.NET のエラー条件を意図的に作成できます。 Wingtip Toys サンプル アプリケーションでは、動作を表示する既定のページを読み込むときにテスト例外をスローします。

1. 分離コードを開き、 *Default.aspx* Visual Studio でのページです。   
 *Default.aspx.cs*分離コード ページが表示されます。
2. `Page_Load`ハンドラー、ハンドラーが次のように表示されるようにコードを追加します。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample9.cs?highlight=3-4)]

さまざまな種類の例外を作成することができます。 上記のコードで作成する、`InvalidOperationException`ときに、 *Default.aspx*ページが読み込まれます。

#### <a name="running-the-application"></a>アプリケーションの実行

アプリケーションが例外を処理する方法を表示するアプリケーションを実行することができます。

1. キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。  
 アプリケーションが、InvalidOperationException をスローします。 

    > [!NOTE] 
    > 
    > キーを押す必要があります**ctrl キーを押しながら f5 キーを押して**を Visual Studio で、エラーの発生元を表示するコードを分断することがなく、ページを表示します。
2. 確認、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラーの処理 - エラー ページ](aspnet-error-handling/_static/image2.png)

例外がによってトラップされたエラーの詳細に示すように、 `customError` 」の「、 *Web.config*ファイル。

### <a name="adding-application-level-error-handling"></a>アプリケーション レベルのエラー処理を追加します。

使用して例外トラップではなく、 `customErrors` 」の「、 *Web.config*ファイル、例外に関する情報を過不足なくを取得する場所は、アプリケーション レベル エラーをトラップして、エラーの詳細を取得することができます。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Global.asax.cs*ファイル。
2. 追加、**アプリケーション\_エラー**ハンドラー it が次のように表示されるようにします。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample10.cs)]

アプリケーションでは、エラーが発生したときに、`Application_Error`ハンドラーが呼び出されます。 このハンドラーでは、最後の例外が取得され、確認します。 例外はハンドルされませんでした。 例外には、内部例外の詳細が含まれている場合 (つまり、`InnerException`が null でない)、アプリケーションは例外の詳細を表示する場所のエラー ページに実行を転送します。

#### <a name="running-the-application"></a>アプリケーションの実行

アプリケーション レベルで例外を処理することによって提供される追加のエラー詳細を表示するアプリケーションを実行することができます。

1. キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。  
 アプリケーションをスロー、`InvalidOperationException`です。
2. 確認、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラー処理のアプリケーション レベルのエラー](aspnet-error-handling/_static/image3.png)

### <a name="adding-page-level-error-handling"></a>ページ レベルのエラー処理を追加します。

追加できますページ レベルのエラー処理、ページの追加を使用するか、`ErrorPage`属性を`@Page`ディレクティブのページ、または追加することによって、`Page_Error`分離コード ページのイベント ハンドラー。 このセクションでは追加、`Page_Error`に実行を転送するイベント ハンドラー、 *ErrorPage.aspx*ページ。

1. **ソリューション エクスプ ローラー**を検索して開く、 *Default.aspx.cs*ファイル。
2. 追加、`Page_Error`ハンドラーとして、分離コードが表示されるように依存します。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample11.cs?highlight=18-30)]

ページで、エラーが発生したときに、`Page_Error`イベント ハンドラーが呼び出されます。 このハンドラーでは、最後の例外が取得され、確認します。 場合、`InvalidOperationException`が発生した、`Page_Error`イベント ハンドラーは例外の詳細を表示する場所のエラー ページに実行を転送します。

#### <a name="running-the-application"></a>アプリケーションの実行

今すぐ更新済みのルートを表示するアプリケーションを実行することができます。

1. キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。  
 アプリケーションをスロー、`InvalidOperationException`です。
2. 確認、 *ErrorPage.aspx*ブラウザーに表示されます。 

    ![ASP.NET エラー処理でページ レベルのエラー](aspnet-error-handling/_static/image4.png)
3. ブラウザー ウィンドウを閉じます。

### <a name="removing-the-exception-used-for-testing"></a>テストに使用される例外を削除します。

このチュートリアルで先ほど追加した例外をスローせずに機能する Wingtip Toys サンプル アプリケーションを許可するのには、例外を削除します。

1. 分離コードを開き、 *Default.aspx*ページ。
2. `Page_Load`ハンドラー、ハンドラーが次のように表示されるように、例外をスローするコードを削除します。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample12.cs)]

### <a name="adding-code-level-error-logging"></a>コード レベルのエラー ログ機能の追加

このチュートリアルで既に説明したように、コードのセクションを実行し、最初に発生するエラーを処理しようとする try ブロックと catch ステートメントに追加できます。 この例ではのみ記述するエラーの詳細エラー ログ ファイルにエラーは後で確認できるようにします。

1. **ソリューション エクスプ ローラー**で、*ロジック*フォルダー、検索して開く、 *PayPalFunctions.cs*ファイル。
2. 更新プログラム、`HttpCall`メソッドとして、コードが表示されるように依存します。   

    [!code-csharp[Main](aspnet-error-handling/samples/sample13.cs?highlight=20,22-23)]

上記のコードの呼び出し、`LogException`メソッドに含まれている、`ExceptionUtility`クラスです。 追加する、 *ExceptionUtility.cs*クラス ファイルを*ロジック*このチュートリアルで先ほどフォルダーです。 `LogException` は、次の 2 つのパラメーターを受け取ります。 最初のパラメーターは、例外オブジェクトです。 2 番目のパラメーターは、エラーの原因を認識するように使用される文字列です。

### <a name="inspecting-the-error-logging-information"></a>エラー ログの情報を検査

前述のように、まず、アプリケーションでエラーを修正してくださいを決定するのに、エラー ログを使用できます。 もちろん、トラップされ、エラー ログに記録されているエラーのみが記録されます。

1. **ソリューション エクスプ ローラー**を検索して開く、 *ErrorLog.txt*ファイルで、*アプリ\_データ*フォルダーです。   
 選択する必要があります、"**すべてのファイル**"オプションまたは"**更新**"オプションの最上位から**ソリューション エクスプ ローラー**を表示する、 *ErrorLog.txt*ファイル。
2. Visual Studio に表示されるエラー ログを確認します。 

    ![ASP.NET エラーの処理 - ErrorLog.txt](aspnet-error-handling/_static/image5.png)

### <a name="safe-error-messages"></a>安全なエラー メッセージ

**に注意して**こと、アプリケーションには、エラー メッセージが表示されたら、その必要がありますいない寄付悪意のあるユーザーは、アプリケーションを攻撃するに役に立つ情報。 たとえば、アプリケーションが、データベースへ書き込みに失敗したしようとすると、使用されているユーザー名を含むエラー メッセージいない表示されます。 このため、赤で一般的なエラー メッセージがユーザーに表示されます。 すべての追加のエラーの詳細は、ローカル コンピューター上の開発者にのみ表示されます。

## <a name="using-elmah"></a>ELMAH を使用します。

ELMAH (エラー Logging Modules and Handlers) は、NuGet パッケージとして、ASP.NET アプリケーションに接続エラーのログ記録機能です。 ELMAH では、次の機能を提供します。

- 未処理の例外のログを記録します。
- 記録された未処理の例外の全体のログを表示する web ページです。
- それぞれの完全な詳細情報を表示する web ページには、例外が記録されます。
- 電子メールは、各エラーの発生時に通知します。
- 最後の 15 のエラー ログからの RSS フィード。

ELMAH を使用することができます、前にインストールする必要があります。 これは簡単なを使用して、 *NuGet*インストーラーをパッケージ化します。 既に述べたようこのチュートリアルのシリーズ、NuGet は Visual Studio の拡張のオープン ソース ライブラリおよび Visual Studio のツールをインストールおよび更新を簡単にします。

1. Visual Studio 内から、**ツール**メニューの **ライブラリ パッケージ マネージャー**  - &gt; **Manage NuGet Packages for Solution**です。 

    ![ASP.NET エラーの処理 - ソリューションの NuGet パッケージの管理](aspnet-error-handling/_static/image6.png)
2. **NuGet パッケージの管理** ダイアログ ボックスが Visual Studio 内で表示されます。
3. **NuGet パッケージの管理**] ダイアログ ボックスで、展開**オンライン**、左側にし、[ **nuget.org**です。次に、検索してインストール、 **ELMAH**使用可能なパッケージをオンラインの一覧からパッケージです。 

    ![ASP.NET エラーの処理 - ELMA NuGet パッケージ](aspnet-error-handling/_static/image7.png)
4. パッケージのダウンロードをインターネットに接続する必要があります。
5. **プロジェクトの選択** ダイアログ ボックスで確認、 **WingtipToys**選択範囲を選択して、をクリックして**OK**です。 

    ![ASP.NET エラーの処理 - プロジェクト ダイアログ ボックスを選択](aspnet-error-handling/_static/image8.png)
6. をクリックして**閉じる**で**NuGet パッケージの管理**ダイアログ ボックスで必要な場合です。
7. Visual Studio では、開いているファイルを再読み込みすることを要求する場合は、選択"**すべてはい**"です。
8. ELMAH パッケージ自体でのエントリを追加する、 *Web.config*プロジェクトのルートにあるファイルです。 かどうかは Visual Studio が表示されたら、変更の再読み込みする*Web.config*ファイルをクリックして**はい**です。

ELMAH は、発生する未処理のエラーを格納する準備ができました。

### <a name="viewing-the-elmah-log"></a>ELMAH ログを表示します。

ELMAH ログの表示は簡単ですが最初に ELMAH ログに記録する未処理の例外を作成します。

1. キーを押して**ctrl キーを押しながら f5 キーを押して**Wingtip Toys サンプル アプリケーションを実行します。
2. 未処理の例外をログに書き込む、ELMAH、(使用するポート番号を使用して) 次の URL をブラウザーに移動します。  
    `https://localhost:44300/NoPage.aspx` エラー ページが表示されます。
3. ELMAH のログを表示するには、(使用するポート番号を使用して) 次の URL をブラウザーに移動します。  
    `https://localhost:44300/elmah.axd`

    ![ASP.NET エラーの処理 - ELMAH エラー ログ](aspnet-error-handling/_static/image9.png)

## <a name="summary"></a>まとめ

このチュートリアルでは、アプリケーション レベル、ページ レベル、およびコード レベルでのエラーの処理について学習しました。 後で確認の処理および未処理のエラー ログに記録する方法も学習しました。 例外ログに記録して NuGet を使用して、アプリケーションに通知を提供する ELMAH ユーティリティが追加されます。 さらに、安全なエラー メッセージの重要性について学習しました。

## <a name="tutorial-series-conclusion"></a>最後に一連のチュートリアル

*次に沿ったいただきありがとうございます。このチュートリアルのセットを使用して、ASP.NET Web フォームに慣れることを望みます。詳細については、Web フォームの機能 ASP.NET 4.5 と Visual Studio 2013 で使用可能な場合を参照してください* [*ASP.NET および Web Tools for Visual Studio 2013 のリリース ノート*](../../../../visual-studio/overview/2013/release-notes.md)*.また、必ずで説明したチュートリアルを見て、* ***次の手順****セクションと defintely お試し、* [*無料試用版の Azure*](https://azure.microsoft.com/pricing/free-trial/)*.*

![ありがとうございます - Erik](aspnet-error-handling/_static/image10.png)  

## <a name="next-steps"></a>次の手順

Microsoft Azure に web アプリケーションの配置の詳細については、「 [Azure Web サイトをセキュリティで保護された ASP.NET Web フォームを使用したアプリのメンバーシップ OAuth、および SQL データベースを展開](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-webforms-app-membership-oauth-sql-database/)です。

## <a name="free-trial"></a>無料試用版

[Microsoft Azure の無料試用版](https://azure.microsoft.com/pricing/free-trial/)  
 Microsoft Azure への web サイトの発行は保存すると、時間、メンテナンスとコストします。 これは、Azure に web アプリを配置するクイック プロセスです。 維持し、web アプリを監視する必要がある場合、Azure はさまざまなツールやサービスを提供します。 データ、トラフィック、identity、メッセージング、Azure でのパフォーマンスとメディアのバックアップを管理します。 このすべては非常にコスト効果の高い方法で提供します。

## <a name="additional-resources"></a>その他のリソース

[ASP.NET の状態監視によるエラーの詳細をログ記録](../../older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-asp-net-health-monitoring-cs.md)   
[ELMAH](https://code.google.com/p/elmah/)

## <a name="acknowledgements"></a>謝辞

このチュートリアル シリーズのコンテンツに多大な協力者次の方々 に感謝したいと思います。

- [Alberto Poblacion、MVP &amp; MCT、スペイン](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- [Alex Thissen、オランダ](http://blog.alexthissen.nl/)(twitter: [ @alexthissen ](http://twitter.com/alexthissen))
- [Andre Tournier (米国)](http://andret503.wordpress.com/)
- Apurva Joshi、Microsoft
- [Bojan Vrhovnik、スロベニア](http://twitter.com/bvrhovnik)
- [Bruno Sonnino、ブラジル](http://msmvps.com/blogs/bsonnino)(twitter: [ @bsonnino ](http://twitter.com/bsonnino))
- [Carlos dos Santos、ブラジル](http://www.carloscds.net/)
- [Dave Campbell、USA](http://www.wynapse.com/) (twitter: [ @windowsdevnews ](http://twitter.com/windowsdevnews))
- [Jon Galloway、Microsoft](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Michael シャープ、USA](http://www.930solutions.com/) (twitter: [ @mrsharps ](http://twitter.com/mrsharps))
- Mike 教皇
- [Mitchel Sellers、USA](http://www.mitchelsellers.com/) (twitter: [ @MitchelSellers ](http://twitter.com/MitchelSellers))
- [Paul Cociuba、Microsoft](http://linqto.me/Links/pcociuba)
- [サンパウロ Morgado、ポルトガル](http://paulomorgado.net/)
- [Pranav Rastogi、Microsoft](https://blogs.msdn.com/b/pranav_rastogi)
- [Tim Ammann、Microsoft](https://blogs.iis.net/timamm/default.aspx)
- [Tom Dykstra、Microsoft](https://blogs.msdn.com/aspnetue)

## <a name="community-contributions"></a>コミュニティからの投稿

- Graham Mendick ([@grahammendick](http://twitter.com/grahammendick))  
 Visual Studio 2012 に関連する msdn コード サンプル:[ナビゲーション Wingtip Toys](https://code.msdn.microsoft.com/Navigation-Wingtip-Toys-5f0daba2)
- James Chaney ([jchaney@agvance.net](mailto:jchaney@agvance.net))  
 Visual Studio 2012 に関連する msdn コード サンプル: [Visual Basic では ASP.NET 4.5 Web フォーム チュートリアル シリーズ](https://code.msdn.microsoft.com/ASPNET-45-Web-Forms-f37f0f63)
- Andrielle Azevedo - Microsoft Technical Audience 共同作成者 (twitter: @driazevedo)  
 Visual Studio 2012 翻訳: [Iniciando com Visão Geral ASP.NET Web フォーム 4.5 - Parte 1 - Introdução e](https://andrielleazevedo.wordpress.com/2013/01/24/iniciando-com-asp-net-web-forms-4-5-introducao-e-visao-geral/)

>[!div class="step-by-step"]
[前へ](url-routing.md)
