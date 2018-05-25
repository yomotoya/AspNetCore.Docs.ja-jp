---
uid: web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
title: 'ハンズ オン ラボ: ビルドと ASP.NET Web API および Angular.js シングル ページ アプリケーション (SPA) |Microsoft ドキュメント'
author: rick-anderson
description: 従来の web アプリケーションでは、クライアント (ブラウザー) は、ページを要求することによって、サーバーとの通信を開始します。 サーバーは、要求を処理しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 09/30/2015
ms.topic: article
ms.assetid: 719727b7-bef3-45ad-bfe9-ba5bcdb2305f
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs
msc.type: authoredcontent
ms.openlocfilehash: 9a748628d53878be380869ac5327de0111d2284d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="hands-on-lab-build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs"></a>ハンズ オン ラボ: ビルドと ASP.NET Web API および Angular.js シングル ページ アプリケーション (SPA)
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web キャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> 従来の web アプリケーションでは、クライアント (ブラウザー) は、ページを要求することによって、サーバーとの通信を開始します。 サーバーは要求を処理し、ページの HTML クライアントに送信します。 – ユーザーなどのリンクに移動またはフォームにデータを送信する – ページと後続のやり取りで新しい要求が、サーバーに送信され、フローをもう一度開始しますサーバーが要求を処理し、新しいページが新しいアクションの要求に応答してブラウザーに送信。クライアントによって ed です。
> 
> 単一ページ アプリケーション (SPAs) では、ブラウザーでページ全体を読み込む後、最初の要求が Ajax 要求を介して以降の操作が行わ。 つまり、ブラウザーが変更されたページの部分のみを更新するにはページ全体を再読み込みする必要はありません。 SPA アプローチより滑らかなエクスペリエンスに結果として得られる、ユーザー アクションに応答するアプリケーションでは時間が短縮されます。
> 
> SPA のアーキテクチャでは、いくつかの課題が従来の web アプリケーションではありません。 ただし、AngularJS などの JavaScript フレームワークの ASP.NET Web API などのテクノロジに出現する、し CSS3 によって提供される新しいスタイル機能簡単本当に設計および SPAs を構築します。
> 
> この手の形でラボでは、マニア クイズ、SPA 概念に基づいてトリビア web サイトを実装するこれらのテクノロジの利点がかかります。 まず、質問を取得し、回答を保存するに必要なエンドポイントを公開する ASP.NET Web API とサービス層を実装します。 次に、AngularJS および css3 用の変換の効果を使用してや応答性の機能豊富な UI をビルドします。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)です。


## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは学習する方法。

- JSON データを送受信する ASP.NET Web API サービスを作成します。
- AngularJS を使用して、応答性の高い UI を作成します。
- CSS3 変換で UI エクスペリエンスを向上します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上

<a id="Setup"></a>
### <a name="setup"></a>セットアップ

このハンズオン ラボでは、演習を実行するのには、最初に、環境を設定する必要があります。

1. 開いている Windows エクスプ ローラーおよびテスト環境の参照**ソース**フォルダーです。
2. 右クリックして**Setup.cmd**選択と**管理者として実行**を環境を構成して、このラボ用の Visual Studio のコード スニペットをインストールするセットアップ プロセスを起動します。
3. ユーザー アカウント制御 ダイアログ ボックスが表示されている場合は、続行するアクションを確認します。

> [!NOTE]
> セットアップを実行する前に、このラボのすべての依存関係をチェックしたことを確認してください。


<a id="CodeSnippets"></a>
### <a name="using-the-code-snippets"></a>コード スニペットを使用します。

ラボ ドキュメント全体にわたって、コード ブロックを挿入するように指示されます。 便宜上、このコードのほとんどは、Visual Studio コード スニペットを手動で追加することを回避する Visual Studio 2013 内からアクセスできるとして提供されます。

> [!NOTE]
> 各演習にある開始ソリューションが組み込まれた、**開始**個別に各手順をたどることによってこの作業のフォルダーです。 実習では、追加のコード スニペットがこれらのソリューションの開始から不足しており、演習を完了するまで動作しない可能性がありますに注意してください。 演習では、ソース コード、内部も表示されます、**終了**を対応する手順の手順を実行した結果のコードの Visual Studio ソリューションを含むフォルダー。 このハンズオン ラボを使用すると、追加のヘルプを必要がある場合は、これらのソリューションをガイドとして使用できます。


* * *

<a id="Exercises"></a>
## <a name="exercises"></a>演習

このハンズオン ラボには、次の実習が含まれます。

1. [Web API を作成します。](#Exercise1)
2. [SPA インターフェイスを作成します。](#Exercise2)

この演習を完了する時間を推定: **60 分**

> [!NOTE]
> Visual Studio を初めて起動するときに定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルと一致するものではまた、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペットでは、およびダイアログ ボックスのオプションを判断します。 このラボの手順を使用する場合は、Visual Studio での特定のタスクを実行に必要な操作を記述する、**全般的な開発設定**コレクション。 開発環境に合わせてさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-web-api"></a>手順 1: Web API を作成します。

サービス層は、SPA の主要部分のいずれかです。 UI とその呼び出しに対する応答で返されるデータによって送信される Ajax 呼び出しの処理を行います。 解析され、クライアントで利用するためにコンピューターが判読できる形式で取得されたデータを表示する必要があります。

Web API フレームワークでは、ASP.NET スタックの一部であるし、HTTP サービス、通常、RESTful API を通じて JSON または XML 形式のデータの送受信を実装するが簡単にいます。 この演習では、マニア クイズ アプリケーションをホストし、バックエンド サービスを公開して、クイズを ASP.NET Web API を使用してデータを永続化を実装するには、Web サイトを作成します。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-the-initial-project-for-geek-quiz"></a>作業 1: マニア クイズの初期のプロジェクトを作成します。

このタスクでは作成を開始する新しい ASP.NET MVC プロジェクトのサポートの ASP.NET Web API に基づいて、 **1 つの ASP.NET** Visual Studio に付属している型を射影します。 **1 つの ASP.NET**にすべての ASP.NET テクノロジをまとめを混在させるし、必要に応じて、それらが一致するオプションを選択できます。 Entity Framework のモデルのクラスとクイズの質問を挿入するデータベース initializator を追加します。

1. 開いている**Visual Studio Express 2013 for Web**選択**ファイル |新しいプロジェクト.** を新しいソリューションを開始します。

    ![新しいプロジェクトを作成する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image1.png "新規プロジェクトの作成")

    *新しいプロジェクトを作成します。*
2. **新しいプロジェクト**ダイアログ ボックスで、 **ASP.NET Web アプリケーション**下にある、 **Visual c# |Web**タブです。確認してください **.NET Framework 4.5**がという名前を選択すると、 *GeekQuiz*を選択、**場所** をクリック**OK**です。

    ![新しい ASP.NET Web アプリケーション プロジェクトを作成する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image2.png "新しい ASP.NET Web アプリケーション プロジェクトを作成します。")

    *新しい ASP.NET Web アプリケーション プロジェクトを作成します。*
3. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **MVC**テンプレートを選択、 **Web API**オプション。 また、ことを確認、**認証**にオプションが設定されている**個々 のユーザー アカウント**です。 **[OK]** をクリックして続行します。

    ![Web API のコンポーネントを含め、MVC テンプレートに新しいプロジェクトの作成](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image3.png)

    *Web API のコンポーネントを含め、MVC テンプレートに新しいプロジェクトの作成*
4. **ソリューション エクスプ ローラー**を右クリックし、**モデル**のフォルダー、 **GeekQuiz**プロジェクトし、選択**追加 |既存アイテム.**.

    ![既存の項目を追加する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image4.png "既存の項目を追加します。")

    *既存の項目を追加します。*
5. **既存項目の追加** ダイアログ ボックスに移動、**ソース/資産/モデル**フォルダーとすべてのファイルを選択します。 **[追加]** をクリックします。

    ![モデルのアセットの追加](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image5.png "モデル アセットの追加")

    *モデルのアセットの追加*

    > [!NOTE]
    > これらのファイルを追加すると、データ モデル、エンティティ フレームワークのデータベースのコンテキストとマニア Quiz アプリケーションのデータベース初期化子を追加します。
    > 
    > **Entity Framework (EF)** は、オブジェクト リレーショナル マッパー (ORM) 直接リレーショナル ストレージ スキーマを使用したプログラミングではなく、概念アプリケーション モデルを使用したプログラミング、データ アクセス アプリケーションを作成することができます。 Entity Framework に関する詳細については、[ここ](../../../entity-framework.md)です。
    > 
    > 追加したばかりのクラスの説明を次に示します。
    > 
    > - **TriviaOption:** クイズの質問に関連付けられている 1 つのオプションを表します
    > - **TriviaQuestion:** クイズの質問を表し、を通じて関連付けられているオプションを公開して、**オプション**プロパティ
    > - **TriviaAnswer:** クイズの質問への応答でユーザーが選択されているオプションを表します
    > - **TriviaContext:** マニア Quiz アプリケーションの Entity Framework のデータベース コンテキストを表します。 このクラスから派生**DContext**と公開**DbSet**を上記で説明したエンティティのコレクションを表すプロパティ。
    > - **TriviaDatabaseInitializer:** Entity Framework の初期化子の実装、 **TriviaContext**クラスから継承される**CreateDatabaseIfNotExists**です。 指定されたエンティティを挿入するこのクラスの既定の動作は、存在しない場合にのみ、データベースを作成するのには、**シード**メソッドです。
6. 開く、 **Global.asax.cs**ファイルし、次を追加するステートメントを使用します。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample1.cs)]
7. 先頭に次のコードを追加、**アプリケーション\_開始**を設定するメソッド、 **TriviaDatabaseInitializer**データベース初期化子として。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample2.cs)]
8. 変更、**ホーム**へのアクセスを制限するコント ローラーがユーザーを認証します。 これを行うには、開く、 **HomeController.cs**内のファイル、**コント ローラー**フォルダーを追加し、 **Authorize**属性を**HomeController**クラス定義です。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample3.cs)]

    > [!NOTE]
    > **Authorize**フィルター処理、ユーザーが認証されるかどうかかどうかを確認します。 ユーザーが認証されていない場合は、アクションを呼び出すことがなく HTTP ステータス コード 401 (Unauthorized) を返します。 コント ローラー レベル、または個々 のアクション レベルでグローバルに、フィルターを適用することができます。
9. これで、web ページと、ブランド化のレイアウトをカスタマイズします。 これを行うには、開く、  **\_Layout.cshtml**内のファイル、**ビュー |共有**フォルダーの内容を更新し、 **&lt;タイトル&gt;** に置き換えることで要素*マイ ASP.NET アプリケーション*で*マニア クイズ*.

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample4.cshtml)]
10. 同じファイル内には、削除することで、ナビゲーション バーを更新、*に関する*と*連絡先*リンクと名前を変更する、*ホーム*へのリンク*再生*です。 さらに、名前の変更、*アプリケーション名*へのリンク*マニア Quiz*です。 ナビゲーション バーの HTML は、次のコードのようになります。

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample5.cshtml)]
11. レイアウト ページのフッターを置き換えることで更新*マイ ASP.NET アプリケーション*で*マニア Quiz*です。 これを行うには、コンテンツを置き換え、 **&lt;フッター&gt;** 要素を次の強調表示されたコード。

    [!code-html[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample6.html)]

<a id="Ex1Task2"></a>
#### <a name="task-2--creating-the-triviacontroller-web-api"></a>タスク 2: TriviaController Web API を作成します。

前のタスクでは、マニア クイズの web アプリケーションの初期の構造を作成します。 クイズ データ モデルと対話し、次の操作を公開する単純な Web API サービスが作成されます。

- **GET api/トリビア**: 認証済みユーザーが回答するクイズ リストから次の質問を取得します。
- **POST api/トリビア**: 認証されたユーザーを指定してクイズの答えを格納します。

Visual Studio によって提供される ASP.NET スキャフォールディング ツールを使用する Web API コント ローラー クラスの基準を作成します。

1. 開く、 **WebApiConfig.cs**内のファイル、**アプリ\_開始**フォルダーです。 このファイルは、Web API コント ローラーのアクションをルートをマップする方法と同様に、Web API サービスの構成を定義します。
2. 次の追加、ファイルの先頭にステートメントを使用します。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample7.cs)]
3. 次の強調表示されたコードを追加、**登録**Web API のアクション メソッドによって取得された JSON データのフォーマッタをグローバルに構成する方法です。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample8.cs)]

    > [!NOTE]
    > **CamelCasePropertyNamesContractResolver**にプロパティ名を自動的に変換*camel 形式*場合は、JavaScript のプロパティ名の一般的な規則です。
4. **ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **GeekQuiz**プロジェクトし、選択**追加 |新しい項目のスキャフォールディングしています.**.

    ![スキャフォールディングされた新しい項目を作成する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image6.png "新しいスキャフォールディングされた項目を作成します。")

    *スキャフォールディングされた新しい項目を作成します。*
5. **追加 Scaffold**  ダイアログ ボックスで、ことを確認して、**共通**左側のウィンドウでノードを選択します。 次に、選択、 **Web API 2 コント ローラー - 空**中央のペインにテンプレート**追加**です。

    ![Web API 2 コント ローラー空のテンプレートを選択する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image7.png "Web API 2 コント ローラー空のテンプレートを選択します。")

    *Web API 2 コント ローラー空のテンプレートを選択します。*

    > [!NOTE]
    > **ASP.NET スキャフォールディング**は、ASP.NET Web アプリケーションのコード生成のフレームワークです。 Visual Studio 2013 には、MVC、Web API プロジェクトに対して事前インストールされているコード ジェネレーターが含まれています。 標準的なデータ操作を開発するために必要な時間の量を削減するためにデータ モデルと対話するコードを簡単に追加するときに、プロジェクトのスキャフォールディングを使用する必要があります。
    > 
    > スキャフォールディング プロセスは、プロジェクトに必要なすべての依存関係がインストールされているできるようになります。 たとえば、空の ASP.NET プロジェクトを開始して、スキャフォールディングを使用して Web API コント ローラーを追加した場合、必要な Web API NuGet パッケージと参照をプロジェクトに自動的に追加されます。
6. **コント ローラーの追加** ダイアログ ボックスで、「 *TriviaController*で、**コント ローラー名**テキスト ボックスをクリック**追加**です。

    ![トリビア コント ローラーを追加する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image8.png "トリビア コント ローラーを追加します。")

    *トリビア コント ローラーを追加します。*
7. **TriviaController.cs**にファイルが追加し、**コント ローラー**のフォルダー、 **GeekQuiz** 、空を含むプロジェクトは、 **TriviaController**クラスです。 次の追加、ファイルの先頭にステートメントを使用します。

    (コード スニペットの*AspNetWebApiSpa - Ex1 - TriviaControllerUsings*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample9.cs)]
8. 先頭に次のコードを追加、 **TriviaController**クラスを定義、初期化および破棄、 **TriviaContext**コント ローラーのインスタンス。

    (コード スニペットの*AspNetWebApiSpa - Ex1 - TriviaControllerContext*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample10.cs)]

    > [!NOTE]
    > **Dispose**メソッドの**TriviaController**呼び出します、 **Dispose**のメソッド、 **TriviaContext**インスタンスで、確実にすべてコンテキスト オブジェクトで使用されているリソースがリリースされたときに、 **TriviaContext**インスタンスを破棄またはガベージ コレクションします。 これには、Entity Framework によって開かれたすべてのデータベース接続の終了が含まれます。
9. 末尾に次のヘルパー メソッドを追加、 **TriviaController**クラスです。 このメソッドは、応答するように、指定したユーザー データベースからクイズの次の質問を取得します。

    (コード スニペットの*AspNetWebApiSpa - Ex1 - TriviaControllerNextQuestion*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample11.cs)]
10. 次の追加**取得**アクション メソッド、 **TriviaController**クラスです。 このアクション メソッドを呼び出す、 **NextQuestionAsync**認証されたユーザーの次の質問を取得する前の手順で定義されているヘルパー メソッドです。

    (コード スニペットの*AspNetWebApiSpa - Ex1 - TriviaControllerGetAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample12.cs)]
11. 末尾に次のヘルパー メソッドを追加、 **TriviaController**クラスです。 このメソッドは、データベースに指定された応答を格納し、答えが正しいかどうかを示すブール値を返します。

    (コード スニペットの*AspNetWebApiSpa - Ex1 - TriviaControllerStoreAsync*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample13.cs)]
12. 次の追加**Post**アクション メソッド、 **TriviaController**クラスです。 このアクション メソッドが、認証されたユーザーとの呼び出しに応答を関連付ける、 **StoreAsync**ヘルパー メソッドです。 その後、ヘルパー メソッドによって返されるブール値を持つ応答を送信します。

    (コード スニペットの*AspNetWebApiSpa - Ex1 - TriviaControllerPostAction*)

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample14.cs)]
13. 追加することで認証されたユーザーへのアクセスを制限する Web API コント ローラーの変更、 **Authorize**属性を**TriviaController**クラス定義です。

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample15.cs)]

<a id="Ex1Task3"></a>
#### <a name="task-3--running-the-solution"></a>タスク 3: ソリューションの実行

このタスクでは、前のタスクでビルドした Web API サービスが正しく動作することを確認します。 Internet Explorer を使用する、 **F12 開発者ツール**ネットワーク トラフィックをキャプチャし、Web API サービスから応答の全体を検査します。

> [!NOTE]
> 確認して**Internet Explorer**でが選択されている、**開始**Visual Studio ツールバー上にあるボタンをクリックします。
> 
> ![Internet Explorer オプション](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image9.png)


1. キーを押して**f5 キーを押して**ソリューションを実行します。 **ログイン**ページがブラウザーに表示する必要があります。

    > [!NOTE]
    > アプリケーションの起動時に既定の MVC ルートがトリガーされる、既定でにマップされて、**インデックス**のアクション、 **HomeController**クラスです。 **HomeController**認証されたユーザーに制限されます (そのクラスを装飾することに注意してください、 **Authorize**演習 1 の属性) し、ユーザーが認証されていない、まだアプリケーションがあります。元の要求をログイン ページにリダイレクトします。

    ![ソリューションを実行している](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image10.png "ソリューションの実行")

    *ソリューションの実行*
2. をクリックして**登録**新しいユーザーを作成します。

    ![新しいユーザーを登録する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image11.png "新しいユーザーを登録します。")

    *新しいユーザーを登録します。*
3. **登録** ページで、入力、**ユーザー名**と**パスワード**、クリックして**登録**です。

    ![登録 ページ](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image12.png "登録ページ")

    *登録 ページ*
4. アプリケーションが、新しいアカウントを登録し、ユーザーが認証され、ホーム ページにリダイレクトします。

    ![ユーザーが認証される](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image13.png "認証されたユーザー")

    *ユーザーが認証されます。*
5. ブラウザーで、キーを押して**F12**を開くには、 **Developer Tools**パネルです。 キーを押して**CTRL + 4**  をクリックして、**ネットワーク**アイコンをクリックしてネットワーク トラフィックのキャプチャを開始する緑色の矢印ボタンをクリックします。

    ![Web API のネットワーク キャプチャを開始する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image14.png "ネットワーク キャプチャを開始する Web API")

    *Web API のネットワーク キャプチャを開始しています*
6. 追加**api/トリビア**ブラウザーのアドレス バーの URL にします。 応答の詳細を調査して、ここでは、**取得**でアクション メソッド**TriviaController**です。

    ![Web API を介して次質問データを取得する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image15.png "Web API を介して次質問データを取得します。")

    *Web API を介して次質問データを取得します。*

    > [!NOTE]
    > ダウンロードが終了した、ダウンロードしたファイルの操作を行うが求められます。 ダイアログ ボックスは、開発者ツール ウィンドウから応答のコンテンツを監視するために開いたままにしておきます。
7. ようになりましたが、応答の本文を検査します。 これを行うをクリックして、**詳細** タブをクリックして**応答本文**です。 ダウンロードしたデータが、プロパティを持つオブジェクトをチェックする**オプション**(の一覧は**TriviaOption**オブジェクト)、 **id**と**タイトル**に対応する、 **TriviaQuestion**クラスです。

    ![Web API の応答本文を表示する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image16.png "Web API の応答本文を表示します。")

    *表示する Web API の応答本文*
8. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-the-spa-interface"></a>手順 2: SPA インターフェイスの作成

この演習では最初をビルドする、web フロント エンドの部分マニア クイズ、単一ページ アプリケーションの相互作用を使用して、焦点を当てた**AngularJS**です。 機能豊富なアニメーションを実行し、コンテキストの切り替えを 1 つの質問へ移行する場合の視覚効果を提供する css3 用のユーザー エクスペリエンスを拡張します。

<a id="Ex2Task1"></a>
#### <a name="task-1--creating-the-spa-interface-using-angularjs"></a>作業 1: AngularJS を使用して SPA インターフェイスを作成します。

このタスクでは、使用**AngularJS**マニア Quiz アプリケーションのクライアント側を実装します。 **AngularJS**でブラウザー ベースのアプリケーションを補足するオープン ソースの JavaScript フレームワークは*モデル-ビュー-コント ローラー* (MVC) の機能、両方の開発を容易にすることと、テストします。

Visual Studio のパッケージ マネージャー コンソールから AngularJS をインストールすることで始めます。 その後、マニア クイズ アプリと、クイズの質問と回答 AngularJS テンプレート エンジンを使用して表示するビューの動作を提供するコント ローラーを作成します。

> [!NOTE]
> AngularJS の詳細についてを参照してください[ [http://angularjs.org/](http://angularjs.org/)](http://angularjs.org/)です。


1. 開く**Visual Studio Express 2013 for Web**を開くと、 **GeekQuiz.sln**にソリューションがある、**ソース/Ex2-CreatingASPAInterface/開始**フォルダーです。 代わりに、前の手順で取得した、ソリューションを続行できます。
2. 開く、 **Package Manager Console**から**ツール** | **ライブラリ パッケージ マネージャー**です。 インストールする次のコマンドを入力、 **AngularJS.Core** NuGet パッケージです。

    [!code-powershell[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample16.ps1)]
3. **ソリューション エクスプ ローラー**を右クリックし、**スクリプト**のフォルダー、 **GeekQuiz**プロジェクトし、選択**追加 |新しいフォルダー**です。 名前のフォルダー**アプリ**とキーを押します**Enter**です。
4. 右クリックし、**アプリ**フォルダーだけを作成し、選択**追加 |JavaScript ファイル**です。

    ![新しい JavaScript ファイルを作成します。](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image17.png)

    *新しい JavaScript ファイルを作成します。*
5. **項目の名前を指定** ダイアログ ボックスで、「*クイズ コント ローラー*で、**項目の名前**テキスト ボックスをクリック**OK**です。

    ![新しい JavaScript ファイルの名前を付ける](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image18.png)

    *新しい JavaScript ファイルの名前を付ける*
6. **クイズ controller.js**ファイルに追加し、次のコードを宣言および初期化する AngularJS **QuizCtrl**コント ローラー。

    (コード スニペットの*AspNetWebApiSpa - Ex2 - AngularQuizController*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample17.js)]

    > [!NOTE]
    > コンス トラクター関数、 **QuizCtrl**コント ローラーという injectable のパラメーターが必要ですが **$scope**です。 スコープの初期状態必要がありますでセットアップされているコンス トラクター関数のプロパティをアタッチすることによって、 **$scope**オブジェクト。 プロパティに含まれる、**ビュー モデル**、コント ローラーを登録するときに、テンプレートにアクセスできるとします。
    > 
    > **QuizCtrl**コント ローラーがという名前のモジュール内に定義されている**QuizApp**です。 モジュールは、個別のコンポーネントにすることのできる作業単位が、アプリケーションを中断します。 モジュールを使用する主な利点は、コードは理解しやすいされ、単体テスト、再利用性と保守が容易になります。
7. ビューからトリガーされたイベントに反応するために、スコープを今すぐ動作を追加します。 末尾に次のコードを追加、 **QuizCtrl**を定義するコント ローラー、 **nextQuestion**で機能、 **$scope**オブジェクト。

    (コード スニペットの*AspNetWebApiSpa - Ex2 - AngularQuizControllerNextQuestion*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample18.js)]

    > [!NOTE]
    > この関数から次の質問を取得する、**トリビア**Web API は、前述の手順で作成し、質問にデータを **$scope**オブジェクト。
8. 最後に、次のコードを挿入、 **QuizCtrl**を定義するコント ローラー、 **sendAnswer**で機能、 **$scope**オブジェクト。

    (コード スニペットの*AspNetWebApiSpa - Ex2 - AngularQuizControllerSendAnswer*)

    [!code-javascript[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample19.js)]

    > [!NOTE]
    > この関数は、ユーザーが選択されている応答を送信、**トリビア**Web API で – つまり、応答が正しいかどう – 場合の結果を格納し、 **$scope**オブジェクト。
    > 
    > **NextQuestion**と**sendAnswer**上記の関数は、AngularJS を使用して **$http** XMLHttpRequest 経由で Web API との通信を抽象化するオブジェクトJavaScript オブジェクト ブラウザーからです。 AngularJS には、高い RESTful Api を介してリソースに対して CRUD 操作を実行する抽象化レベルを表示する別のサービスがサポートしています。 AngularJS **$resource**オブジェクトと対話する必要がない高度な動作を提供するアクション メソッドがあります、 **$http**オブジェクト。 使用を検討して、 **$resource** CRUD モデルを必要とするシナリオでのオブジェクト (fore についてを参照してください、 [$resource ドキュメント](https://docs.angularjs.org/api/ngResource/service/$resource))。
9. 次の手順では、クイズのビューを定義する AngularJS テンプレートを作成します。 これを行うには、開く、 **Index.cshtml**内のファイル、**ビュー |ホーム**フォルダーと次のコードにコンテンツを置換します。

    (コード スニペットの*AspNetWebApiSpa - Ex2 - GeekQuizView*)

    [!code-cshtml[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample20.cshtml)]

    > [!NOTE]
    > AngularJS テンプレートは、static のマークアップをブラウザーでユーザーに表示する動的ビューに変換するモデルとコント ローラーからの情報を使用して宣言型の仕様です。 AngularJS 要素とテンプレートで使用できる要素の属性の例を次に示します。
    > 
    > - **Ng アプリ**ディレクティブは AngularJS アプリケーションのルート要素を表す DOM 要素。
    > - **Ng コント ローラー**ディレクティブでは、コント ローラーをディレクティブが宣言されている時点で、DOM にアタッチします。
    > - 中かっこによる表記 **{{}}** コント ローラーで定義されているスコープのプロパティへのバインドを表します。
    > - **Ng **ディレクティブを使用して、ユーザーのクリックに応答では、スコープで定義された関数を呼び出します。
10. 開く、 **Site.css**内のファイル、**コンテンツ**フォルダー クイズのビューのルック アンド フィールを提供するファイルの末尾に次の強調表示されているスタイルを追加します。

    (コード スニペットの*AspNetWebApiSpa - Ex2 - GeekQuizStyles*)

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample21.css)]

<a id="Ex2Task2"></a>
#### <a name="task-2--running-the-solution"></a>タスク 2 – ソリューションの実行

このタスクを実行します、新しいユーザーを使用してソリューションをインターフェイスに答えるクイズの質問のために AngularJS でビルドします。

1. キーを押して**f5 キーを押して**ソリューションを実行します。
2. 新しいユーザー アカウントを登録します。 これを行うには、手順 1 では、タスク 3 で説明されている登録の手順に従います。

    > [!NOTE]
    > 前の手順からソリューションを使用している場合は、以前に作成したユーザー アカウントを使用してログインすることができます。
3. **ホーム**ページが表示されます、クイズの最初の質問を表示します。 オプションのいずれかをクリックして、質問に答えます。 これにより、 **sendAnswer**が選択したオプションを送信する前に、定義されている関数、**トリビア**Web API です。

    ![質問の回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image19.png "質問に答えること")

    *質問に答えること*
4. 一方のボタンをクリックすると、回答が表示されます。 をクリックして**次の質問**を次の質問を表示します。 これにより、 **nextQuestion**コント ローラーで定義された関数。

    ![次の質問を要求する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image20.png "次の質問を要求します。")

    *次の質問を要求します。*
5. 次の質問が表示されます。 必要な回数だけの質問に回答を続行します。 すべての質問を完了した後は、最初の質問を返す必要があります。

    ![別の質問](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image21.png "別の問題")

    *次の質問*
6. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Ex2Task3"></a>
#### <a name="task-3--creating-a-flip-animation-using-css3"></a>タスク 3 – CSS3 を使用して反転アニメーションを作成します。

このタスクでは、CSS3 プロパティを使用して、豊富なアニメーションを実行するには、質問に答えると、次の質問を取得するときに反転効果を追加します。

1. **ソリューション エクスプ ローラー**を右クリックし、**コンテンツ**のフォルダー、 **GeekQuiz**プロジェクトし、選択**追加 |既存アイテム.**.

    ![コンテンツのフォルダーへの既存の項目の追加](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image22.png "コンテンツのフォルダーへの既存の項目の追加")

    *コンテンツのフォルダーへの既存の項目の追加*
2. **既存項目の追加** ダイアログ ボックスに移動、**ソース/資産**フォルダーと選択**Flip.css**です。 **[追加]** をクリックします。

    ![アセットから Flip.css ファイルを追加する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image23.png "アセットから Flip.css ファイルを追加します。")

    *アセットから Flip.css ファイルを追加します。*
3. 開く、 **Flip.css**ファイルだけを追加して、その内容を確認します。
4. 検索、**変換を反転**コメントです。 そのコメントの下のスタイルが CSS を使用して**パースペクティブ**と**rotateY**を生成する変換、&quot;カードのフリップ&quot;効果。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample22.css)]
5. 検索、**フリップ中にウィンドウの非表示に**コメントです。 直面しているビューアーから離れた場所で設定するときにそのコメントの下のスタイルを面の背面にある非表示に、**背面可視性**CSS プロパティを*隠し*です。

    [!code-css[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample23.css)]
6. 開く、 **BundleConfig.cs**内のファイル、**アプリ\_開始**フォルダーへの参照を追加し、 **Flip.css**ファイルで、 **&quot;~/Content/css&quot;** スタイル バンドル

    [!code-csharp[Main](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/samples/sample24.cs)]
7. キーを押して**f5 キーを押して**ソリューションと、資格情報でログインを実行します。
8. クリックすると、オプションのいずれかの質問に回答します。 ビュー間を遷移するときに、反転効果に注目してください。

    ![フリップ効果のある質問の回答](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image24.png "フリップ効果のある質問の回答")

    *フリップ効果のある質問の回答*
9. をクリックして**次の質問**を次の質問を取得します。 フリップ効果が再度表示されます。

    ![次の質問、フリップの効果を取得する](build-a-single-page-application-spa-with-aspnet-web-api-and-angularjs/_static/image25.png "フリップ効果の次の質問を取得します。")

    *次の質問、フリップの効果を取得します。*

* * *

<a id="Summary"></a>
## <a name="summary"></a>概要

このハンズオン ラボを完了して学習した方法。

- ASP.NET のスキャフォールディングを使用した ASP.NET Web API コント ローラーを作成します。
- クイズの次の質問を取得する Web API 取得操作を実装します。
- 質問に対する回答を格納する Web API の後のアクションを実装します。
- Visual Studio パッケージ マネージャー コンソールから AngularJS をインストールします。
- 実装する AngularJS テンプレートとコント ローラー
- CSS3 切り替え効果を使用して、アニメーション効果を実行するには
