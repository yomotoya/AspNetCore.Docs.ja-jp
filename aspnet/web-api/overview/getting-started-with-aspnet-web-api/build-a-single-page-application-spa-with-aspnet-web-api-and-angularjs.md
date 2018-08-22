---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'ハンズ オン ラボ: ASP.NET Web API と Angular.js で単一ページ アプリケーション (SPA) のビルド |Microsoft Docs'
author: rick-anderson
description: 従来の web アプリケーションでは、クライアント (ブラウザー) は、ページを要求することによって、サーバーとの通信を開始します。 サーバーは、要求を処理しています.
ms.author: riande
ms.date: 09/30/2015
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 24ab1c470a22b5b328d1f3bc400400978eb31600
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823922"
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>ハンズ オン ラボ: ASP.NET Web API と Angular.js で単一ページ アプリケーション (SPA) のビルドします。
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> 従来の web アプリケーションでは、クライアント (ブラウザー) は、ページを要求することによって、サーバーとの通信を開始します。 サーバーは、要求を処理し、ページの HTML をクライアントに送信します。 – ユーザーなどのリンクに移動またはフォームにデータを送信する – ページと後続のやり取りで新しい要求は、サーバーに送信され、フローをもう一度開始します。 サーバーが要求を処理し、新しいアクションの要求に応答してブラウザーに新しいページを送信します。クライアントによって ed します。
> 
> シングル ページ アプリケーション (Spa) では、ブラウザーで全体のページを読み込む最初の要求の後が、以降の Ajax 要求を通じて行われます。 つまり、ブラウザーが変更されました。 ページの部分だけを更新するにはページ全体を再読み込みする必要はありません。 SPA のアプローチには、その結果より滑らかなエクスペリエンス、ユーザーの操作に応答するアプリケーションでは時間が短縮されます。
> 
> SPA のアーキテクチャには、従来の web アプリケーションではないいくつかの課題が含まれます。 ただし、ASP.NET Web API などのテクノロジを新たな JavaScript フレームワークなどの AngularJS と CSS3 によって提供される新しいスタイル機能簡単本当に設計し、Spa を構築します。
> 
> このハンズオン ラボでは、ギーク Quiz、SPA の概念に基づくトリビアの web サイトを実装するためにこれらのテクノロジの利点がかかります。 まず、質問を取得し、回答を保存するに必要なエンドポイントを公開する ASP.NET Web API を使用したサービス層を実装します。 次に、AngularJS、および CSS3 の変換の効果を使用して、リッチで応答性の高い UI をビルドします。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。


## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- JSON データを送受信する ASP.NET Web API サービスを作成します。
- AngularJS を使用して、レスポンシブ UI を作成します。
- 変換する CSS3 UI エクスペリエンスを向上します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボの演習を実行するためには、まず環境を設定する必要があります。

1. Windows Explorer をラボの [参照] を開いて**ソース**フォルダー。
2. 右クリックして**Setup.cmd**選択と**管理者として実行**環境を構成すると、このラボの Visual Studio のコード スニペットがインストールのセットアップ プロセスを起動します。
3. ユーザー アカウント制御ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認します。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットの使用

ラボ ドキュメント全体を通じて、コード ブロックを挿入するよう指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを避けるために Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> ソリューションでは個々 の演習を伴います、**開始**を使用すると、各演習を他のユーザーとは無関係に練習のフォルダー。 演習の中に追加されるコード スニペットはこれらのスターティング ソリューションが表示されないし、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード内でも表示されます、**エンド**結果から、対応する演習の手順を実行するコードと Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプが必要な場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の演習が含まれます。

1. [Web API を作成します。](#Exercise1)
2. [SPA のインターフェイスを作成します。](#Exercise2)

この演習の所要時間を推定: **60 分**

> [!NOTE]
> Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。 このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。 開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>手順 1: Web API を作成します。

SPA の重要な部分の 1 つは、サービス層です。 UI とその呼び出しに対する応答で返されるデータによって送信される Ajax 呼び出しを処理するため、します。 取得されたデータが解析され、クライアントで使用するためにコンピューターが判読できる形式で表示されます。

Web API フレームワークは、ASP.NET スタックの一部でありは HTTP サービス、一般に RESTful API を使用して JSON または XML 形式のデータの送受信を実装するが簡単に設計されています。 この演習では、おたくのクイズ アプリケーションをホストし、公開、クイズを ASP.NET Web API を使用してデータを永続化するバックエンド サービスを実装する Web サイトを作成します。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>タスク 1 – おたくのクイズの初期のプロジェクトを作成します。

このタスクで ASP.NET Web API ベースのサポートで新しい ASP.NET MVC プロジェクトの作成を開始は、 **One ASP.NET** Visual Studio に付属する型を射影します。 **1 つの ASP.NET**すべての ASP.NET のテクノロジの統合し、を混在させるし、必要に応じてとに一致させることができます。 Entity Framework のモデル クラスと、質問を挿入するデータベース initializator を追加します。

1. 開いている**Visual Studio Express 2013 for Web**選択**ファイル |新しいプロジェクト.** 新しいソリューションを開始します。

    ![新しいプロジェクトを作成する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "新しいプロジェクトを作成します。")

    *新しいプロジェクトを作成します。*
2. **新しいプロジェクト**ダイアログ ボックスで、 **ASP.NET Web アプリケーション**下、 **(Visual C#) |Web**タブ。確認します **.NET Framework 4.5**がという名前を選択すると、 *GeekQuiz*、選択、**場所** をクリック**OK**。

    ![新しい ASP.NET Web アプリケーション プロジェクトを作成する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "新しい ASP.NET Web アプリケーション プロジェクトを作成します。")

    *新しい ASP.NET Web アプリケーション プロジェクトを作成します。*
3. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **MVC**テンプレートと選択、 **Web API**オプション。 確認、**認証**にオプションが設定されている**個々 のユーザー アカウント**します。 **[OK]** をクリックして続行します。

    ![Web API のコンポーネントを含む、MVC テンプレートを使用して新しいプロジェクトを作成します。](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Web API のコンポーネントを含む、MVC テンプレートを使用して新しいプロジェクトを作成します。*
4. **ソリューション エクスプ ローラー**を右クリックし、**モデル**のフォルダー、 **GeekQuiz**順に選択して**追加 |既存の項目.**.

    ![既存の項目を追加する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "既存の項目の追加")

    *既存の項目を追加します。*
5. **既存項目の追加** ダイアログ ボックスに移動し、**ソース/資産/モデル**フォルダーとすべてのファイルを選択します。 **[追加]** をクリックします。

    ![モデルの資産を追加する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "モデル資産を追加します。")

    *モデルの資産を追加します。*

    > [!NOTE]
    > これらのファイルを追加すると、データ モデル、Entity Framework のデータベース コンテキストおよびギーク Quiz アプリケーション データベースの初期化子に追加されます。
    > 
    > **Entity Framework (EF)** は、オブジェクト リレーショナル マッパー (ORM)、リレーショナル ストレージ スキーマを使用して直接プログラミングではなく、概念アプリケーション モデルを使用したプログラミングでデータ アクセス アプリケーションを作成することができます。 Entity Framework に関する詳細については、[ここ](../../../entity-framework.md)します。
    > 
    > 追加したクラスの説明を次に示します。
    > 
    > - **TriviaOption:** クイズの質問に関連付けられている 1 つのオプションを表します
    > - **TriviaQuestion:** クイズの質問を表し、を通じて関連付けられているオプションを公開して、**オプション**プロパティ
    > - **TriviaAnswer:** クイズの質問への応答でユーザーが選択したオプションを表します
    > - **TriviaContext:** ギーク Quiz アプリケーションの Entity Framework のデータベース コンテキストを表します。 このクラスから派生**DContext**公開**DbSet**上記で説明したエンティティのコレクションを表すプロパティ。
    > - **TriviaDatabaseInitializer:** Entity Framework の初期化子の実装、 **TriviaContext**クラスから継承される**CreateDatabaseIfNotExists**します。 指定されたエンティティを挿入するこのクラスの既定の動作が存在しない場合にのみデータベースを作成するが、**シード**メソッド。
6. 開く、 **Global.asax.cs**ファイルを開き、次を追加するステートメントを使用します。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. 先頭に次のコードを追加、**アプリケーション\_開始**を設定するメソッド、 **TriviaDatabaseInitializer**データベース初期化子として。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. 変更、**ホーム**コント ローラーへのアクセスを制限するユーザーを認証します。 これを行うには、開く、 **HomeController.cs**ファイル内で、**コント ローラー**フォルダーを追加し、 **Authorize**属性を**HomeController**クラスの定義。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Authorize**フィルターのかどうか、ユーザーは認証を確認します。 ユーザーが認証されていない場合は、アクションを呼び出すことがなく HTTP 状態コード 401 (Unauthorized) を返します。 コント ローラー レベル、または個々 のアクションのレベルでグローバルにフィルターを適用することができます。
9. これで web ページと、ブランド化のレイアウトをカスタマイズします。 これを行うには、開く、  **\_Layout.cshtml**ファイル内で、**ビュー |共有**フォルダーの内容を更新し、 **&lt;タイトル&gt;** 置き換え要素*My ASP.NET Application*で*ギーク Quiz*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. 同じファイルで削除することで、ナビゲーション バーを更新、*について*と*連絡先*リンクと名前を変更する、*ホーム*へのリンク*再生*します。 さらに、名前の変更、*アプリケーション名*へのリンク*ギーク Quiz*します。 ナビゲーション バーの HTML は、次のコードのようになります。

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. レイアウト ページのフッターを置き換えることで、更新*My ASP.NET Application*で*ギーク Quiz*します。 これを行うには、内容が置換、 **&lt;フッター&gt;** 要素を次の強調表示されたコード。

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>タスク 2 – TriviaController Web API を作成します。

前のタスクでは、ギーク Quiz の web アプリケーションの初期構造を作成します。 クイズのデータ モデルと対話し、次の操作を公開する単純な Web API サービスをビルドします。

- **Api/GET トリビア**: 認証されたユーザーが回答するクイズ リストから、次の質問を取得します。
- **POST/api/トリビア**: 認証済みユーザーが指定したクイズの答えを格納します。

Visual Studio によって提供される ASP.NET スキャフォールディング ツールを使用して、Web API コント ローラー クラスの基準を作成します。

1. 開く、 **WebApiConfig.cs**ファイル内で、**アプリ\_開始**フォルダー。 このファイルは、Web API コント ローラー アクションへのルートをマップする方法のように、Web API サービスの構成を定義します。
2. 次の追加ファイルの先頭にステートメントを使用します。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. 次の強調表示されたコードを追加、**登録**のフォーマッタを Web API アクション メソッドによって取得された JSON データをグローバルに構成する方法。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver**するプロパティの名前を自動的に変換*キャメル形式*場合は、JavaScript のプロパティ名の一般的な規則です。
4. **ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **GeekQuiz**順に選択して**追加 |新規スキャフォールディング アイテム.**.

    ![新しくスキャフォールディングした項目を作成する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "新しくスキャフォールディングした項目を作成します。")

    *新しくスキャフォールディングした項目を作成します。*
5. **スキャフォールディングの追加** ダイアログ ボックスで、ことを確認します、**共通**左側のウィンドウでノードを選択します。 次に、選択、 **Web API 2 コント ローラー - 空**中央のウィンドウにテンプレート**追加**します。

    ![Web API 2 コント ローラー空のテンプレートを選択する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Web API 2 コント ローラー空のテンプレートを選択します。")

    *Web API 2 コント ローラー空のテンプレートを選択します。*

    > [!NOTE]
    > **ASP.NET スキャフォールディング**は ASP.NET Web アプリケーションのコード生成フレームワークです。 Visual Studio 2013 には、MVC と Web API プロジェクトに対して事前にインストールされているコード ジェネレーターが含まれています。 標準的なデータ操作を開発するために必要な時間を軽減するために、データ モデルとやり取りするコードをすばやく追加する場合は、プロジェクトでスキャフォールディングを使用する必要があります。
    > 
    > スキャフォールディングのプロセスでは、必要なすべての依存関係がプロジェクトにインストールされているまた。 たとえば、空の ASP.NET プロジェクトを開始し、スキャフォールディングを使用して Web API コント ローラーを追加すると場合、必要な Web API の NuGet パッケージと参照をプロジェクトに自動的に追加されます。
6. **コント ローラーの追加**ダイアログ ボックスに「 *TriviaController*で、**コント ローラー名**テキスト ボックスをクリックします**追加**します。

    ![トリビア コント ローラーを追加する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "トリビア コント ローラーを追加します。")

    *トリビア コント ローラーを追加します。*
7. **TriviaController.cs**にファイルが追加し、**コント ローラー**のフォルダー、 **GeekQuiz**空を含むプロジェクトは、 **TriviaController**クラス。 次の追加、ファイルの先頭にステートメントを使用します。

    (コード スニペット - *AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. 先頭に次のコードを追加、 **TriviaController**クラスを定義、初期化および破棄、 **TriviaContext**コント ローラー インスタンス。

    (コード スニペット - *AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose**メソッドの**TriviaController**呼び出す、 **Dispose**のメソッド、 **TriviaContext**インスタンスで、確実にすべてコンテキスト オブジェクトで使用されるリソースがリリースされたときに、 **TriviaContext**インスタンスが破棄またはガベージ コレクトします。 これは、Entity Framework によって開かれているすべてのデータベース接続の終了が含まれます。
9. 末尾に次のヘルパー メソッドを追加、 **TriviaController**クラス。 このメソッドは、指定したユーザーが回答をデータベースから、クイズの次の質問を取得します。

    (コード スニペット - *AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. 次の追加**取得**アクション メソッドを**TriviaController**クラス。 このアクション メソッドを呼び出す、 **NextQuestionAsync**認証されたユーザーは次の質問を取得する前の手順で定義されているヘルパー メソッド。

    (コード スニペット - *AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. 末尾に次のヘルパー メソッドを追加、 **TriviaController**クラス。 このメソッドは、データベースに指定された応答を格納し、答えが正しいかどうかを示すブール値を返します。

    (コード スニペット - *AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. 次の追加**Post**アクション メソッドを**TriviaController**クラス。 このアクション メソッドは、認証されたユーザーと呼び出しに応答を関連付けます、 **StoreAsync**ヘルパー メソッド。 次に、ヘルパー メソッドによって返されるブール値を持つ応答を送信します。

    (コード スニペット - *AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. 追加することで認証されたユーザーへのアクセスを制限する Web API コント ローラーの変更、 **Authorize**属性を**TriviaController**クラスの定義。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>タスク 3 – ソリューションの実行

このタスクでは、前のタスクで作成した Web API サービスが正しく動作することを確認します。 Internet Explorer を使用する**F12 開発者ツール**をネットワーク トラフィックをキャプチャして、Web API サービスから完全な応答を検査します。

> [!NOTE]
> 確認します**Internet Explorer**でが選択されている、**開始**Visual Studio ツールバーにあるボタンをクリックします。
> 
> ![Internet Explorer オプション](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. キーを押して**F5**ソリューションを実行します。 **ログイン**ページがブラウザーに表示する必要があります。

    > [!NOTE]
    > アプリケーションの起動時に既定でマップは既定の MVC ルートがトリガーされます。、**インデックス**のアクション、 **HomeController**クラス。 **HomeController**は認証されたユーザーに制限されます (とそのクラスを装飾する注意してください、 **Authorize**演習 1 での属性) はユーザーが認証されていない、まだアプリケーションログイン ページには、元の要求をリダイレクトします。

    ![ソリューションを実行している](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "ソリューションの実行")

    *ソリューションの実行*
2. クリックして**登録**新しいユーザーを作成します。

    ![新しいユーザーを登録する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "新しいユーザーを登録します。")

    *新しいユーザーを登録します。*
3. **登録**ページで、入力、**ユーザー名**と**パスワード**、順にクリックします**登録**します。

    ![登録 ページ](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "登録ページ")

    *登録 ページ*
4. アプリケーションが、新しいアカウントを登録し、ユーザーが認証され、ホーム ページにリダイレクトします。

    ![ユーザーが認証される](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "認証されたユーザー")

    *ユーザーを認証します。*
5. キーを押して、ブラウザーで**F12**を開く、 **Developer Tools**パネル。 キーを押して**CTRL + 4**  をクリックしてまたは、**ネットワーク**緑色の矢印は、ネットワーク トラフィックのキャプチャを開始するボタン アイコンをクリックします。

    ![Web API のネットワーク キャプチャを開始する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "ネットワーク キャプチャを開始する Web API")

    *Web API のネットワーク キャプチャを開始します。*
6. 追加**api/トリビア**ブラウザーのアドレス バーで URL にします。 応答の詳細を調べるようになりましたが、**取得**内のアクション メソッド**TriviaController**します。

    ![Web API を介して次の質問のデータを取得する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Web API を介して次の質問のデータを取得します。")

    *Web API を介して次の質問のデータを取得します。*

    > [!NOTE]
    > ダウンロードが完了すると、ダウンロードしたファイルを使用して行う促されます。 ダイアログ ボックスは、開発者ツール ウィンドウからの応答のコンテンツを視聴できるように開いたままにしておきます。
7. ここでは、応答の本文を検査します。 これを行うには、をクリックして、**詳細** タブをクリックして**応答本文**します。 ダウンロードされたデータが、プロパティを持つオブジェクトをチェックする**オプション**(の一覧は**TriviaOption**オブジェクト)、 **id**と**のタイトル**に対応する、 **TriviaQuestion**クラス。

    ![Web API の応答本文を表示する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Web API の応答本文を表示します。")

    *表示する Web API の応答本文*
8. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>手順 2: SPA インターフェイスの作成

この演習ではまず作成ギーク Quiz、web フロント エンド部分を使用してシングル ページ アプリケーションの対話**AngularJS**します。 また、リッチなアニメーションを実行し、コンテキストの切り替え、次に、1 つの質問から移行する場合の視覚効果を提供する CSS3 のユーザー エクスペリエンス、強化されます。

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>タスク 1 – AngularJS を使用して SPA インターフェイスを作成します。

このタスクでは、使用**AngularJS**ギーク Quiz アプリケーションのクライアント側を実装します。 **AngularJS**でブラウザー ベースのアプリケーションを補足するオープン ソースの JavaScript フレームワークは、*モデル-ビュー-コント ローラー* (MVC) 機能、両方の開発を促進することと、テストします。

Visual Studio のパッケージ マネージャー コンソールから AngularJS をインストールすることで始めます。 次に、ギーク クイズ アプリと、クイズの質問と回答の AngularJS テンプレート エンジンを使用してレンダリングするビューの動作を提供するコント ローラーを作成します。

> [!NOTE]
> AngularJS の詳細についてを参照してください[ [ http://angularjs.org/ ](http://angularjs.org/)](http://angularjs.org/)します。


1. 開く**Visual Studio Express 2013 for Web**を開くと、 **GeekQuiz.sln**ソリューション、**ソース/Ex2-CreatingASPAInterface/開始**フォルダー。 または、前の手順で取得したソリューションを続行できます。
2. 開く、**パッケージ マネージャー コンソール**から**ツール** | **ライブラリ パッケージ マネージャー**します。 インストールするには、次のコマンドを入力、 **AngularJS.Core** NuGet パッケージ。

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. **ソリューション エクスプ ローラー**を右クリックし、**スクリプト**のフォルダー、 **GeekQuiz**順に選択して**追加 |新しいフォルダー**します。 フォルダーの名前**アプリ**キーを押します**Enter**します。
4. 右クリックし、**アプリ**フォルダーを作成して選択**追加 |JavaScript ファイル**します。

    ![新しい JavaScript ファイルを作成します。](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *新しい JavaScript ファイルを作成します。*
5. **項目の名前を指定**ダイアログ ボックスに「*クイズ コント ローラー*で、**項目名**テキスト ボックスをクリックします**OK**。

    ![新しい JavaScript ファイルの名前を付ける](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *新しい JavaScript ファイルの名前を付ける*
6. **クイズ controller.js**ファイルに追加し、次のコードを宣言して初期化 AngularJS **QuizCtrl**コント ローラー。

    (コード スニペット - *AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > コンス トラクター関数、 **QuizCtrl**コント ローラーという injectable のパラメーターが必要ですが **$scope**します。 スコープの初期状態する必要があります設定コンス トラクター関数のプロパティをアタッチすることにより、 **$scope**オブジェクト。 プロパティに含まれる、**ビュー モデル**、コント ローラーが登録されているときに、テンプレートにアクセス可能になります。
    > 
    > **QuizCtrl**コント ローラーがという名前のモジュール内で定義されている**QuizApp**します。 モジュールは、できる作業単位が、アプリケーションを個別のコンポーネントに分割されます。 モジュールを使用しての主な利点は、コードがより容易に理解し、単体テスト、再利用性と保守を容易にします。
7. ビューからトリガーされるイベントに対応するために、スコープに今すぐ動作を追加します。 末尾に次のコードを追加、 **QuizCtrl**を定義するコント ローラー、 **nextQuestion**で機能、 **$scope**オブジェクト。

    (コード スニペット - *AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > この関数から次の質問の取得、**トリビア**Web API は、前の演習で作成したし、する質問のデータをアタッチします、 **$scope**オブジェクト。
8. 末尾に次のコードを挿入、 **QuizCtrl**を定義するコント ローラー、 **sendAnswer**で機能、 **$scope**オブジェクト。

    (コード スニペット - *AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > この関数にユーザーが選択した応答の送信、**トリビア**Web API で – つまり、正解ですか -場合の結果を格納し、 **$scope**オブジェクト。
    > 
    > **NextQuestion**と**sendAnswer**上記の関数が、AngularJS を使用して **$http** XMLHttpRequest を使用して Web API との通信を抽象化するオブジェクトブラウザーから JavaScript オブジェクトです。 AngularJS より高度な RESTful Api を使用して、リソースに対する CRUD 操作を実行する抽象化は、その他のサービスをサポートします。 AngularJS **$resource**オブジェクトと対話することがなく高レベルの動作を提供するアクション メソッドがあります、 **$http**オブジェクト。 使用を検討して、 **$resource** CRUD モデルを必要とするシナリオでのオブジェクト (fore についてを参照してください、 [$resource ドキュメント](https://docs.angularjs.org/api/ngResource/service/$resource))。
9. 次の手順では、クイズのビューを定義する AngularJS テンプレートを作成します。 これを行うには、開く、 **Index.cshtml**ファイル内で、**ビュー |ホーム**フォルダーと次のコードでコンテンツを置換します。

    (コード スニペット - *AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS テンプレートは、静的なマークアップをユーザーに、ブラウザーで表示される動的なビューに変換するモデルとコント ローラーからの情報を使用して宣言型の仕様です。 AngularJS 要素とテンプレートで使用できる要素の属性の例を次に示します。
    > 
    > - **Ng アプリ**ディレクティブは AngularJS アプリケーションのルート要素を表す DOM 要素。
    > - **Ng コント ローラー**ディレクティブ、コント ローラーをディレクティブが宣言されている時点で、DOM にアタッチします。
    > - 中かっこによる表記 **{{}}** コント ローラーで定義されているスコープのプロパティへのバインドを表します。
    > - **Ng クリック**ディレクティブを使用して、ユーザーのクリックに応答内のスコープで定義された関数を呼び出します。
10. オープン、 **Site.css**内でファイル、**コンテンツ**フォルダー クイズ ビューに、ルック アンド フィールを提供するファイルの最後に、次の強調表示されているスタイルを追加します。

    (コード スニペット - *AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>タスク 2 – ソリューションの実行

実行は、このタスクでは、新しいユーザーを使用して、ソリューションのクイズの質問の一部に回答するための AngularJS でビルドするインターフェイスです。

1. キーを押して**F5**ソリューションを実行します。
2. 新しいユーザー アカウントを登録します。 これを行うには、演習 1 では、タスク 3 で説明されている登録手順に従います。

    > [!NOTE]
    > 前の演習からソリューションを使用している場合は、前に作成したユーザー アカウントを使用してログインすることができます。
3. **ホーム**ページが表示されます、クイズの最初の質問を表示します。 質問の回答のオプションのいずれかをクリックします。 これにより、 **sendAnswer**送信するオプションが選択されている、以前に定義した関数、**トリビア**Web API。

    ![質問に答え](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "質問の回答")

    *質問の回答*
4. ボタンのいずれかをクリックした後、応答が表示されます。 クリックして**次の質問**の次の質問を表示します。 これにより、 **nextQuestion**コント ローラーで定義された関数。

    ![次の質問を要求する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "次の質問を要求します。")

    *次の質問を要求します。*
5. 次の質問が表示されます。 必要な回数だけの質問に回答を続行します。 すべての質問を完了した後は、最初の質問に戻ります。

    ![もう 1 つ質問](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "別の質問")

    *次の質問*
6. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>タスク 3 – CSS3 を使用して、フリップ アニメーションを作成します。

このタスクでは、CSS3 プロパティを使用して、リッチなアニメーションを実行するには、質問の回答と、次の質問を取得するときに反転効果を追加します。

1. **ソリューション エクスプ ローラー**を右クリックし、**コンテンツ**のフォルダー、 **GeekQuiz**順に選択して**追加 |既存の項目.**.

    ![コンテンツのフォルダーへの既存の項目の追加](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "コンテンツのフォルダーへの既存の項目の追加")

    *コンテンツのフォルダーへの既存の項目の追加*
2. **既存項目の追加** ダイアログ ボックスに移動、**ソース/資産**フォルダーと選択**Flip.css**します。 **[追加]** をクリックします。

    ![アセットから Flip.css ファイルを追加する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "資産から Flip.css ファイルの追加")

    *アセットから Flip.css ファイルを追加します。*
3. 開く、 **Flip.css**先ほど追加したファイルし、その内容を確認します。
4. 検索、**変換を反転**コメント。 そのコメントの下のスタイルが CSS を使用して**パースペクティブ**と**rotateY**を生成する変換、&quot;カード フリップ&quot;効果。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. 検索、**フリップ中にウィンドウの非表示に**コメント。 設定して、ビューアー離れている場合に、ときにそのコメントの下のスタイルを顔の背面にある非表示に、**背面を表示**CSS プロパティを*隠し*します。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. 開く、 **BundleConfig.cs**ファイル内で、**アプリ\_開始**フォルダーへの参照を追加し、 **Flip.css**ファイル、 **&quot;~/Content/css&quot;** スタイル バンドル

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. キーを押して**F5**ソリューションと、資格情報でログインを実行します。
8. クリックすると、オプションのいずれかの質問に回答します。 ビューの間で遷移するときに、フリップ効果に注目してください。

    ![フリップの影響の質問に答える](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "フリップの影響の質問に答える")

    *フリップの影響の質問に答える*
9. クリックして**次の質問**の次の質問を取得します。 フリップの効果は、もう一度表示されます。

    ![フリップの影響は、次の質問を取得する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "フリップの影響は、次の質問を取得します。")

    *フリップの影響は、次の質問を取得します。*

* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボについて説明した方法。

- ASP.NET のスキャフォールディングを使用した ASP.NET Web API コント ローラーを作成します。
- クイズの次の質問を取得する Web API の Get 操作を実装します。
- クイズの解答を格納する Web API の事後アクションを実装します。
- Visual Studio パッケージ マネージャー コンソールから AngularJS をインストールします。
- 実装の AngularJS テンプレートとコント ローラー
- CSS3 遷移を使用して、アニメーション効果を実行するには
