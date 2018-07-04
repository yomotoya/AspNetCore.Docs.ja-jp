---
uid: visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
title: 'ハンズ オン ラボ: One ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合 |Microsoft Docs'
author: rick-anderson
description: ASP.NET は、Web サイト、アプリ、および MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。 で ASP.NET h を拡張しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/16/2014
ms.topic: article
ms.assetid: 4fe2558d-67cc-4d12-a5c1-6fb9f6f16137
ms.technology: ''
msc.legacyurl: /visual-studio/overview/2013/one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api
msc.type: authoredcontent
ms.openlocfilehash: 7dec4daffa66621acaee1c76fda7b2e7550ad925
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37382953"
---
<a name="hands-on-lab-one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api"></a>ハンズ オン ラボ: One ASP.NET: ASP.NET Web フォーム、MVC、Web API の統合
====================
によって[Web キャンプ チーム](https://twitter.com/webcamps)

[Web のキャンプ トレーニング キットをダウンロードします。](http://aka.ms/webcamps-training-kit)

> ASP.NET は、Web サイト、アプリ、および MVC、Web API、およびその他のユーザーなどの特殊なテクノロジを使用してサービスを構築するためのフレームワークです。 拡張とは、ASP.NET が作成されてから確認した、明示する必要がありますこれらのテクノロジが統合された最近の取り組みに作業**One ASP.NET**します。
> 
> Visual Studio 2013 には、アプリケーションのビルドし、すべての ASP.NET テクノロジを 1 つのプロジェクトで使用できる新しい、統一されたプロジェクト システムが導入されています。 この機能は、プロジェクトとそれに伴うスティックの開始時に 1 つのテクノロジを選択する必要はありませんし、代わりに 1 つのプロジェクト内の複数の ASP.NET フレームワークの使用が推奨します。
> 
> すべてのサンプル コードとスニペットがで使用可能な Web キャンプ トレーニング キットに含まれている[ http://aka.ms/webcamps-training-kit](http://aka.ms/webcamps-training-kit)します。


<a id="Overview"></a>
## <a name="overview"></a>概要

<a id="Objectives"></a>
### <a name="objectives"></a>目的

このハンズオン ラボでは、学習する方法。

- ベースの Web サイトを作成、 **One ASP.NET**プロジェクトの種類
- 使用して異なる**ASP.NET**のようなフレームワーク**MVC**と**Web API**同じプロジェクト内で
- 主要なコンポーネントを識別、 **ASP.NET**アプリケーション
- 利用、 **ASP.NET スキャフォールディング**CRUD 操作を実行するには、コント ローラーとビューを自動的に作成するためにフレームワークが、モデル クラスに基づく
- ジョブごとに適切なツールを使用してマシンに、人間が判読できる形式で情報の同じセットを公開します。

<a id="Prerequisites"></a>
### <a name="prerequisites"></a>必須コンポーネント

このハンズオン ラボを完了する、次が必要。

- [Visual Studio Express 2013 for Web](https://www.microsoft.com/visualstudio/)以上
- [Visual Studio 2013 Update 1](https://go.microsoft.com/fwlink/?LinkId=301714)

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

1. [新しい Web フォーム プロジェクトを作成します。](#Exercise1)
2. [スキャフォールディングを使用した MVC コント ローラーを作成](#Exercise2)
3. [スキャフォールディングを使用して Web API コント ローラーを作成します。](#Exercise3)

この演習の所要時間を推定: **60 分**

> [!NOTE]
> Visual Studio を初めて起動すると、定義済みの設定のコレクションの 1 つを選択する必要があります。 定義済みの各コレクションは、特定の開発スタイルに一致するように設計されていて、ウィンドウのレイアウト、エディターの動作、IntelliSense コード スニペット、およびダイアログ ボックスのオプションを決定します。 このラボの手順を使用する場合は、Visual Studio で特定のタスクを実行するために必要な操作を記述する、**汎用開発設定**コレクション。 開発環境のさまざまな設定のコレクションを選択する場合、考慮する必要がある手順に違いがあります。


<a id="Exercise1"></a>
### <a name="exercise-1-creating-a-new-web-forms-project"></a>手順 1: 新しい Web フォーム プロジェクトを作成します。

この演習では、Visual Studio 2013 を使用して新しい Web フォーム サイトを作成する、 **One ASP.NET**同じアプリケーションで Web フォーム、MVC、Web API のコンポーネントを簡単に統合することができるプロジェクト エクスペリエンスの統合します。 生成されたソリューションを調査して、その部分を特定し、最後に、Web サイトのアクションが表示されます。

<a id="Ex1Task1"></a>
#### <a name="task-1--creating-a-new-site-using-the-one-aspnet-experience"></a>タスク 1 – 1 つの ASP.NET 機能を使用して新しいサイトを作成します。

最初にこのタスクでは Visual Studio で新しい Web サイトの作成に基づく、 **One ASP.NET**プロジェクトの種類。 **1 つの ASP.NET**すべての ASP.NET のテクノロジの統合し、を混在させるし、必要に応じてとに一致させることができます。 アプリケーション内で並行して、ライブ Web フォーム、MVC、Web API の別のコンポーネントを識別します。

1. 開いている**Visual Studio Express 2013 for Web**選択**ファイル |新しいプロジェクト.** 新しいソリューションを開始します。

    ![新規プロジェクトの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image1.png)

    *新しいプロジェクトを作成します。*
2. **新しいプロジェクト**ダイアログ ボックスで、 **ASP.NET Web アプリケーション**下、 **(Visual C#) |Web**タブをクリックし、確認 **.NET Framework 4.5**が選択されています。 プロジェクトの名前*MyHybridSite*、選択、**場所** をクリック**OK**します。

    ![新しい ASP.NET Web アプリケーション プロジェクト](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image2.png)

    *新しい ASP.NET Web アプリケーション プロジェクトを作成します。*
3. **新しい ASP.NET プロジェクト**ダイアログ ボックスで、 **Web フォーム**テンプレートと選択、 **MVC**と**Web API**オプション。 確認、**認証**にオプションが設定されている**個々 のユーザー アカウント**します。 **[OK]** をクリックして続行します。

    ![Web API と MVC のコンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image3.png)

    *Web API と MVC のコンポーネントを含め、Web フォーム テンプレートに新しいプロジェクトを作成します。*
4. 生成されたソリューションの構造を利用できるようになりました。

    ![生成されたソリューションの調査](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image4.png)

    *生成されたソリューションの調査*

    1. **アカウント:** このフォルダーには、登録にログインし、アプリケーションのユーザー アカウントを管理する Web フォーム ページが含まれています。 このフォルダーが追加されたときに、**個々 のユーザー アカウント**Web フォーム プロジェクト テンプレートの構成時に認証オプションを選択します。
    2. **モデルの**このフォルダーは、アプリケーション データを表すクラスが格納されます。
    3. **コント ローラー**と**ビュー**: これらのフォルダーに必要な**ASP.NET MVC**と**ASP.NET Web API**コンポーネント。 次の演習で MVC と Web API のテクノロジについて学びます。
    4. **Default.aspx**、 **Contact.aspx**と**About.aspx**ファイルに固有のページを構築する開始点として使用できる定義済みの Web フォーム ページは、アプリケーション。 呼ばれる別のファイルが存在するそれらのファイルのプログラミング ロジック、&quot;コード ビハインド&quot;ファイルは、 &quot;..aspx.vb ファイル&quot;または&quot;. aspx.cs&quot;拡張機能 (に応じて、使用される言語)。 分離コード ロジックは、サーバー上で実行し、動的に、ページの HTML 出力を生成します。
    5. **Site.Master**と**Site.Mobile.Master**ページは、アプリケーションで、外観とすべてのページの標準の動作を定義します。
5. ダブルクリックして、 **Default.aspx**ファイル ページの内容を調査します。

    ![Default.aspx ページの調査](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image5.png)

    *Default.aspx ページの調査*

    > [!NOTE]
    > **ページ**ファイルの上部にあるディレクティブは、Web フォーム ページの属性を定義します。 など、 **MasterPageFile**属性をマスターへのパスを指定します - この場合、ページ、 *Site.Master*ページ - と**Inherits**属性を定義します、継承するページの分離コード クラスです。 このクラスは、によって決まりますファイルにある、**分離コード**属性。
    > 
    > **Asp:content**コントロール (テキスト、マークアップとコントロール) のページの実際の内容を保持およびにマップされて、 **asp: ContentPlaceHolder**マスター ページのコントロール。 ページのコンテンツ内にレンダリングされるここで、 *MainContent*で定義されているコントロール、 *Site.Master*ページ。
6. 展開、**アプリ\_開始**フォルダーに注目してください、 **WebApiConfig.cs**ファイル。 Visual Studio には、1 つの ASP.NET テンプレートを使用して、プロジェクトを構成するときに、Web API が含まれるために、生成されたソリューションにそのファイルが含まれます。
7. 開く、 **WebApiConfig.cs**ファイル。 *WebApiConfig*マップ HTTP Web API に関連付けられている構成が表示されますクラスにルーティング**Web API コント ローラー**します。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample1.cs)]
8. 開く、 **RouteConfig.cs**ファイル。 内で、 *RegisterRoutes*メソッドへの HTTP ルートをマップする MVC と関連付けられている構成が表示されます**MVC コント ローラー**します。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample2.cs)]

<a id="Ex1Task2"></a>
#### <a name="task-2--running-the-solution"></a>タスク 2 – ソリューションの実行

生成されたソリューションを実行がこのタスクでされたら、アプリといくつかの URL の書き換えや組み込みの認証など、その機能について説明します。

1. キーを押して、ソリューションを実行する**F5**  をクリックしてまたは、**開始**ボタンがツールバーにあります。 アプリケーションのホーム ページは、ブラウザーで開く必要があります。

    ![ソリューションの実行](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image6.png)
2. Web フォーム ページが呼び出されていることを確認します。 これを行うには、次のように追加します。 **/contact.aspx**キーを押して、アドレス バーに URL を**Enter**します。

    ![フレンドリな Url](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image7.png)

    *フレンドリな Url*

    > [!NOTE]
    > ご覧のように、URL 変更**にお問い合わせください/** します。 始まる**ASP.NET 4**、URL ルーティング機能は、Web フォームに追加された、Url などの記述できるように*[ http://www.mysite.com/products/software ](http://www.mysite.com/products/software)* の代わりに *[http://www.mysite.com/products.aspx?category=software](http://www.mysite.com/products.aspx?category=software)*. 詳細についてを参照してください[URL ルーティング](../../../web-forms/overview/getting-started/getting-started-with-aspnet-45-web-forms/url-routing.md)します。
3. これで、アプリケーションに統合認証フローについて学びます。 これを行うには、次のようにクリックします。**登録**ページの右上隅にします。

    ![新しいユーザーを登録します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image8.png)

    *新しいユーザーを登録します。*
4. **登録**ページで、入力、**ユーザー名**と**パスワード**、順にクリックします**登録**します。

    ![登録 ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image9.png)

    *登録 ページ*
5. アプリケーションが、新しいアカウントを登録し、ユーザーを認証します。

    ![認証されたユーザー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image10.png)

    *認証されたユーザー*
6. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Exercise2"></a>
### <a name="exercise-2-creating-an-mvc-controller-using-scaffolding"></a>手順 2: スキャフォールディングを使用した MVC コント ローラーを作成します。

この演習では、アクションと Razor ビューを 1 行のコードを記述することがなく、CRUD 操作を実行すると、ASP.NET MVC 5 コント ローラーを作成する Visual Studio で提供される ASP.NET スキャフォールディング フレームワークの利点がかかります。 スキャフォールディングのプロセスは、SQL database にデータ コンテキストおよびデータベース スキーマを生成するのに Entity Framework Code First では使用します。

**Entity Framework の Code First**

Entity Framework (EF) は、リレーショナル ストレージ スキーマを使用して直接プログラミングではなく、概念アプリケーション モデルを使用したプログラミングによってデータ アクセス アプリケーションを作成できるようにするオブジェクト リレーショナル マッパー (ORM) です。

Entity Framework Code First のモデリング ワークフローできますこのクエリを実行するには、実行するときに、EF が依存するモデルを表す独自のドメイン クラスを使用する関数の変更の追跡と更新。 Code First の開発ワークフローを使用する必要はありませんデータベースを作成するか、スキーマを指定することによって、アプリケーションを開始します。 代わりに、アプリケーションの最も適切なドメイン モデル オブジェクトを定義する標準の .NET クラスを記述することができ、Entity Framework のデータベースが作成されます。

> [!NOTE]
> Entity Framework に関する詳細については、[ここ](../../../entity-framework.md)します。


<a id="Ex2Task1"></a>
#### <a name="task-1--creating-a-new-model"></a>タスク 1 – 新しいモデルを作成します。

ここで定義する、**人**クラスは、MVC コント ローラーとビューを作成するスキャフォールディングのプロセスによって使用されるモデルになります。 作成から始めます、 **Person**モデル クラス、およびコント ローラーでの CRUD 操作が自動的に作成スキャフォールディング機能を使用します。

1. 開いている**Visual Studio Express 2013 for Web**と**MyHybridSite.sln**ソリューション、**ソース/Ex2-MvcScaffolding/開始**フォルダー。 または、前の手順で取得したソリューションを続行できます。
2. **ソリューション エクスプ ローラー**を右クリックし、**モデル**のフォルダー、 **MyHybridSite**順に選択して**追加 |クラス.**.

    ![Person モデル クラスを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image11.png)

    *Person モデル クラスを追加します。*
3. **新しい項目の追加** ダイアログ ボックスで、ファイル名で*Person.cs*  をクリック**追加**します。

    ![Person モデル クラスを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image12.png)

    *Person モデル クラスを作成します。*
4. 内容が置換、 **Person.cs**を次のコード ファイル。 キーを押して**CTRL + S**変更を保存します。

    (コード スニペット - *BringingTogetherOneAspNet - Ex2 - PersonClass*)

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample3.cs)]
5. **ソリューション エクスプ ローラー**を右クリックし、 **MyHybridSite**順に選択して**ビルド**、またはキーを押します**CTRL + SHIFT + B**プロジェクトをビルドします。

<a id="Ex2Task2"></a>
#### <a name="task-2--creating-an-mvc-controller"></a>タスク 2 – MVC コント ローラーを作成

これで、 **Person**モデルを作成、CRUD コント ローラー アクションとビューを作成する Entity Framework と ASP.NET MVC のスキャフォールディングを使用する**人**します。

1. **ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**順に選択して**追加 |新規スキャフォールディング アイテム.**.

    ![新しいスキャフォールディングされたコント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image13.png)

    *新しいスキャフォールディングされたコント ローラーの作成*
2. **スキャフォールディングの追加**ダイアログ ボックスで、 **MVC 5 コント ローラーとビュー、Entity Framework を使用して** をクリックし、**追加します。**

    ![ビューと Entity Framework を使用した MVC 5 コント ローラーを選択](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image14.png)

    *ビューと Entity Framework を使用した MVC 5 コント ローラーを選択*
3. 設定*MvcPersonController*として、**コント ローラー名**を選択、**非同期コント ローラー アクションを使用して、** オプションし、選択**人 (MyHybridSite.Models)** として、**モデル クラス**します。

    ![スキャフォールディングある MVC コント ローラーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image15.png)

    *スキャフォールディングある MVC コント ローラーを追加します。*
4. [**データ コンテキスト クラス**、] をクリックして**新しいデータ コンテキスト.**.

    ![新しいデータ コンテキストを作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image16.png)

    *新しいデータ コンテキストを作成します。*
5. **新しいデータ コンテキスト** ダイアログ ボックスで新しいデータ コンテキスト、名前*PersonContext*クリック**追加**します。

    ![新しい PersonContext を作成します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image17.png)

    *新しい PersonContext 型を作成します。*
6. をクリックして**追加**の新しいコント ローラーを作成する**Person**スキャフォールディングとします。 Visual Studio が生成されます、コント ローラー アクション、個人のデータ コンテキストおよび Razor ビュー。

    ![MVC コント ローラーをスキャフォールディングを作成した後](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image18.png)

    *MVC コント ローラーをスキャフォールディングを作成した後*
7. 開く、 **MvcPersonController.cs**ファイル、**コント ローラー**フォルダー。 CRUD アクション メソッドが自動的に生成されたことに注意してください。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample4.cs)]

    > [!NOTE]
    > 選択して、**非同期コント ローラー アクションを使用して、** スキャフォールディングからのチェック ボックスは、前の手順でオプション、Visual Studio には、ユーザーのデータ コンテキストへのアクセスに関連するすべてのアクションの非同期アクション メソッドが生成されます。 実行時間の長い非同期アクション メソッドを使用する、CPU 以外に、要求の処理中に作業を実行してから、Web サーバーがブロックされないようにする要求がバインドされていることをお勧めします。

<a id="Ex2Task3"></a>
#### <a name="task-3--running-the-solution"></a>タスク 3 – ソリューションの実行

このタスクで実行することを確認するには、もう一度ソリューションのビュー **Person**想定どおりに動作します。 データベースに正常に保存することを確認する新しいユーザーを追加します。

1. キーを押して**F5**ソリューションを実行します。
2. 移動します **/MvcPerson**します。 ユーザーの一覧を表示するスキャフォールディングされたビューが表示されます。
3. クリックして**新規作成**新しいメンバーを追加します。

    ![スキャフォールディングの MVC ビューへの移動](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image19.png)

    *スキャフォールディングの MVC ビューへの移動*
4. **作成**ビューで、提供、**名前**と**年齢**人をクリックします**作成**です。

    ![新しいユーザーを追加します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image20.png)

    *新しいユーザーを追加します。*
5. 新しいユーザーは、一覧に追加されます。 要素の一覧でクリックして**詳細**人の詳細ビューを表示します。 次に、**詳細**ビューで、をクリックして**リストに戻る**リスト ビューに戻ります。

    ![ユーザーの詳細ビュー](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image21.png)

    *ユーザーの詳細ビュー*
6. をクリックして、**削除**ユーザーを削除するリンク。 **削除**表示で、**削除**操作を確定します。

    ![ユーザーを削除します。](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image22.png)

    *ユーザーを削除します。*
7. Visual Studio とキーを押してに戻って**shift キーを押しながら f5 キーを押して**デバッグを停止します。

<a id="Exercise3"></a>
### <a name="exercise-3-creating-a-web-api-controller-using-scaffolding"></a>手順 3: スキャフォールディングを使用して Web API コント ローラーの作成

Web API フレームワークは、ASP.NET スタックの一部であり、容易に実装する HTTP services 一般に RESTful API を使用して JSON または XML 形式のデータを送受信するように設計します。

この演習では、Web API コント ローラーを生成するのに ASP.NET スキャフォールディングをもう一度使用されます。 同じ使用する**人**と**PersonContext**クラスを前の演習を JSON 形式で同じ個人データを提供します。 同じ ASP.NET アプリケーション内でさまざまな方法で、同じリソースを公開する方法が表示されます。

<a id="Ex3Task1"></a>
#### <a name="task-1--creating-a-web-api-controller"></a>タスク 1 – Web API コント ローラーを作成します。

このタスクでは、新規に作成する**Web API コント ローラー** JSON のようにコンピューターが使用できる形式で個人データを公開します。

1. これを開いていない場合は開きます**Visual Studio Express 2013 for Web**を開くと、 **MyHybridSite.sln**ソリューション、**ソース/Ex3-WebAPI/開始**フォルダー。 または、前の手順で取得したソリューションを続行できます。

    > [!NOTE]
    > 手順 3 から Begin ソリューションを起動した場合、以下のキーを押して**CTRL + SHIFT + B**ソリューションをビルドします。
2. **ソリューション エクスプ ローラー**を右クリックし、**コント ローラー**のフォルダー、 **MyHybridSite**順に選択して**追加 |新規スキャフォールディング アイテム.**.

    ![新しいスキャフォールディングされたコント ローラーの作成](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image23.png)

    *新しいスキャフォールディングされたコント ローラーの作成*
3. **スキャフォールディングの追加**ダイアログ ボックスで、 **Web API**し、左側のウィンドウで**Web API 2 コント ローラー アクション、Entity Framework を使用して**中央のペインで順にクリックします**追加します。**

    ![アクションと Entity Framework を使用した Web API 2 コント ローラーを選択](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image24.png "を選択すると Web API 2 コント ローラー アクションと Entity Framework")

    *アクションと Entity Framework を使用した Web API 2 コント ローラーを選択*
4. 設定*ApiPersonController*として、**コント ローラー名**を選択、**非同期コント ローラー アクションを使用して、** オプションし、選択**人 (MyHybridSite.Models)** と**PersonContext (MyHybridSite.Models)** として、**モデル**と**データ コンテキスト**それぞれクラスします。 次に、 **[追加]** をクリックします。

    ![スキャフォールディングで Web API コント ローラーの追加](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image25.png "スキャフォールディングと Web API コント ローラーの追加")

    *スキャフォールディングで Web API コント ローラーの追加*
5. Visual Studio が生成し、 **ApiPersonController**データを操作する 4 つの CRUD 操作を持つクラス。

    ![Web API コント ローラーをスキャフォールディングを作成したら](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image26.png "スキャフォールディングと Web API コント ローラーを作成した後")

    *Web API コント ローラーをスキャフォールディングを作成した後*
6. 開く、 **ApiPersonController.cs**ファイルし、検査、 *GetPeople*アクション メソッド。 このメソッドはクエリの db フィールド**PersonContext**ユーザー データを取得するには型です。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample5.cs)]
7. 今すぐメソッド定義の前のコメントに注意してください。 次のタスクで使用するこのアクションを公開する URI を提供します。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample6.cs)]

    > [!NOTE]
    > 既定では、Web API は、クエリをキャッチする構成、 */api* MVC コント ローラーとの競合を避けるためのパス。 この設定を変更する必要がある場合を参照してください[ASP.NET Web API におけるルーティング](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)します。

<a id="Ex3Task2"></a>
#### <a name="task-2--running-the-solution"></a>タスク 2 – ソリューションの実行

このタスクでは、Internet Explorer を使用して**F12 開発者ツール**を Web API コント ローラーから完全な応答を検査します。 アプリケーション データの多くの洞察を取得するネットワーク トラフィックをキャプチャする方法が表示されます。

> [!NOTE]
> 確認します**Internet Explorer**でが選択されている、**開始**Visual Studio ツールバーにあるボタンをクリックします。
> 
> ![Internet Explorer オプション](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image27.png)
> 
> **F12 開発者ツール**このハンズオン ラボでカバーされていない機能の幅広いセットがあります。 詳細については、する場合を参照してください。 [F12 開発者ツールを使用して](https://msdn.microsoft.com/library/ie/bg182326(v=vs.85))します。


1. キーを押して**F5**ソリューションを実行します。

    > [!NOTE]
    > このタスクを正常に従うために、アプリケーションがデータである必要があります。 データベースが空の場合は、手順 2 での作業 3 に戻るし、MVC ビューを使用して新しいユーザーを作成する方法の手順に従ってください。
2. キーを押して、ブラウザーで**F12**を開く、 **Developer Tools**パネル。 キーを押して**CTRL** + **4**  をクリックしてまたは、**ネットワーク**緑色の矢印は、ネットワーク トラフィックのキャプチャを開始するボタン アイコンをクリックします。

    ![Web API のネットワーク キャプチャを開始する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image28.png "ネットワーク キャプチャを開始する Web API")

    *Web API のネットワーク キャプチャを開始します。*
3. 追加**api/ApiPerson**ブラウザーのアドレス バーで URL にします。 応答の詳細を調べるようになりましたが、 **ApiPersonController**します。

    ![Web API を介してユーザー データを取得する](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image29.png "Web API を介してユーザー データを取得します。")

    *Web API を介してユーザー データを取得します。*

    > [!NOTE]
    > ダウンロードが完了すると、ダウンロードしたファイルを使用して行う促されます。 ダイアログ ボックスは、開発者ツール ウィンドウからの応答のコンテンツを視聴できるように開いたままにしておきます。
4. ここでは、応答の本文を検査します。 これを行うには、をクリックして、**詳細** タブをクリックして**応答本文**します。 ダウンロードされたデータがオブジェクトとプロパティの一覧を確認する**Id**、**名前**と**年齢**に対応する、 **Person**クラス。

    ![表示する Web API の応答本文](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image30.png "表示する Web API の応答本文")

    *表示する Web API の応答本文*

<a id="Ex3Task3"></a>
#### <a name="task-3--adding-web-api-help-pages"></a>タスク 3 – Web API ヘルプ ページの追加

Web API を作成するときに、他の開発者は、API を呼び出す方法を認識できるように、ヘルプ ページを作成すると便利です。 作成し、ドキュメント ページを手動で更新しますが、自動生成すると、メンテナンス作業を実行しなくてもすむようにすることをお勧めします。 このタスクでは、ソリューションに Web API のヘルプ ページを自動的に生成するのに Nuget パッケージを使用します。

1. **ツール** メニューの選択 Visual Studio で**ライブラリ パッケージ マネージャー**、 をクリックし、**パッケージ マネージャー コンソール**します。
2. **パッケージ マネージャー コンソール**ウィンドウで、次のコマンドを実行します。

    [!code-powershell[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample7.ps1)]

    > [!NOTE]
    > **Microsoft.AspNet.WebApi.HelpPage**パッケージが必要なアセンブリをインストールし、ヘルプ ページにあるを MVC ビューを追加します、**領域/HelpPage**フォルダー。

    ![HelpPage 領域](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image31.png "HelpPage 領域")

    *HelpPage 領域*
3. 既定では、ヘルプ ページがあるドキュメントのプレース ホルダー文字列。 XML ドキュメントのコメントを使用するには、ドキュメントを作成します。 この機能を有効にする、開く、 **HelpPageConfig.cs**にあるファイル、**領域/HelpPage/アプリ\_開始**フォルダー、次の行をコメント解除します。

    [!code-javascript[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample8.js)]
4. **ソリューション エクスプ ローラー**、プロジェクトを右クリックして**MyHybridSite**を選択します**プロパティ** をクリックし、**ビルド**タブ。

    ![[ビルド] タブ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image32.png "[ビルド] セクション")

    *[ビルド] タブ*
5. **出力**、 **XML ドキュメント ファイル**します。 編集ボックスに「**アプリ\_Data/XmlDocument.xml**します。

    ![[ビルド] タブでセクションを出力](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image33.png "ビルド タブでセクションの出力")

    *[ビルド] タブで、出力セクション*
6. キーを押して**CTRL** + **S**変更を保存します。
7. 開く、 **ApiPersonController.cs**ファイルから、**コント ローラー**フォルダー。
8. 間に新しい行を入力、 *GetPeople*メソッド シグネチャと *//get api/ApiPerson*コメントの追加、し、次の 3 つのスラッシュを入力します。

    > [!NOTE]
    > Visual Studio は、メソッドのドキュメントを定義する XML 要素を自動的に挿入します。
9. 概要のテキストと、戻り値の追加、 *GetPeople*メソッド。 次のようになります。

    [!code-csharp[Main](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/samples/sample9.cs)]
10. キーを押して**F5**ソリューションを実行します。
11. 追加 **/help**ヘルプ ページを参照する、アドレス バーで URL にします。

    ![ASP.NET Web API ヘルプ ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image34.png "ASP.NET Web API ヘルプ ページ")

    *ASP.NET Web API ヘルプ ページ*

    > [!NOTE]
    > ページのメインのコンテンツは、コント ローラーでグループ化された Api のテーブルです。 テーブルのエントリも動的に生成されたを使用して、 **IApiExplorer**インターフェイス。 追加 API コント ローラーを更新するか、テーブルは自動的に更新、次回アプリケーションを構築します。
    > 
    > **API**列には、HTTP メソッドと相対 URI が表示されます。 **説明**列には、メソッドのドキュメントから抽出された情報が含まれています。
12. [説明] 列、メソッド定義の上に追加した説明が表示されることに注意してください。

    ![API メソッドの説明](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image35.png "API メソッドの説明")

    *API メソッドの説明*
13. 詳細については、サンプルの応答本文を含むページに移動する API のメソッドのいずれかをクリックします。

    ![詳細情報ページ](one-aspnet-integrating-aspnet-web-forms-mvc-and-web-api/_static/image36.png "詳細情報 ページ")

    *詳細情報 ページ*

* * *

<a id="Summary"></a>
## <a name="summary"></a>まとめ

このハンズオン ラボについて説明した方法。

- Visual Studio 2013 で ASP.NET の 1 つのエクスペリエンスを使用して新しい Web アプリケーションを作成します。
- 1 つ 1 つのプロジェクトに複数の ASP.NET テクノロジを統合します。
- ASP.NET のスキャフォールディングを使用して、モデル クラスからの MVC コント ローラーとビューを生成します。
- 非同期プログラミングと Entity Framework を介してデータ アクセスなどの機能を使用して Web API コント ローラーを生成します。
- コント ローラーの Web API のヘルプ ページを自動的に生成します。
