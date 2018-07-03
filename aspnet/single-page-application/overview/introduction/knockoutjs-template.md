---
uid: single-page-application/overview/introduction/knockoutjs-template
title: 'シングル ページ アプリケーション: KnockoutJS テンプレート |Microsoft Docs'
author: MikeWasson
description: Knockout のテンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: ''
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: 75169836d1d9a647bdd4ac3742cb1dc1f31f2a32
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377903"
---
<a name="single-page-application-knockoutjs-template"></a>シングル ページ アプリケーション: KnockoutJS テンプレート
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

> Knockout の MVC テンプレートは ASP.NET and Web Tools 2012.2 の一部
> 
> [ASP.NET と Web ツール 2012.2 をダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET and Web Tools 2012.2 の更新には、ASP.NET MVC 4 用のシングル ページ アプリケーション (SPA) のテンプレートが含まれています。 このテンプレートは、対話型のクライアント側 web アプリをすばやく構築を開始するために設計されています。

「シングル ページ アプリケーション」(SPA) は、1 つの HTML ページが読み込まれ、新しいページの読み込みではなく、ページを動的に更新する web アプリケーションの一般的な用語です。 最初のページの読み込み後に、SPA は、AJAX 要求を介してサーバーとで説明します。

![](knockoutjs-template/_static/image1.png)

AJAX は目新しいものでは、現在 JavaScript フレームワークを構築し、大規模な高度な SPA アプリケーションを管理するが容易では。 また、HTML 5、CSS3 が簡単に豊富な Ui を作成します。

開始するため、SPA テンプレートは、"To-do list"サンプル アプリケーションを作成します。 このチュートリアルでは、テンプレートのガイド付きツアーをみましょう。 まず、to do リスト アプリケーション自体を見てをし、連携できるようにするテクノロジの部分を確認します。

## <a name="create-a-new-spa-template-project"></a>新しい SPA テンプレート プロジェクトを作成します。

要件:

- Visual Studio 2012 または Visual Studio Express 2012 for Web
- ASP.NET Web Tools 2012.2 を更新します。 更新プログラムをインストールする[ここ](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)します。

Visual Studio を起動し、選択**新しいプロジェクト**スタート ページから。 またはから、**ファイル**メニューの **新規**し**プロジェクト**します。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトの名前し、クリックして**OK**します。

![](knockoutjs-template/_static/image2.png)

**新しいプロジェクト**ウィザードで、 **Single Page Application**します。

![](knockoutjs-template/_static/image3.png)

F5 キーを押してアプリケーションをビルドし、実行します。 まず、アプリケーションを実行すると、ログイン画面が表示されます。

![](knockoutjs-template/_static/image4.png)

をクリックして、&quot;サインアップ&quot;リンクし、新しいユーザーを作成します。

![](knockoutjs-template/_static/image5.png)

ログインした後、アプリケーションは、2 つの項目を含む既定の Todo リストを作成します。 "Todo リストの追加 をクリックする新しいリストを追加します。

![](knockoutjs-template/_static/image6.png)

リストの名前を変更、リストに項目を追加し、それらのチェックします。 項目を削除またはリスト全体を削除することもできます。 変更は、サーバー上のデータベースに自動的に永続化 (この時点では、LocalDB 実際には、アプリケーションをローカルで実行しているため)。

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>SPA テンプレートのアーキテクチャ

この図は、アプリケーションの主な構成要素を示しています。

![](knockoutjs-template/_static/image8.png)

サーバー側では、ASP.NET MVC は、HTML を提供しもフォーム ベース認証を処理します。

ASP.NET Web API は、ToDoLists と取得、作成、更新、削除など、ToDoItems に関連するすべての要求を処理します。 クライアントは、JSON 形式で Web API を使用したデータを交換します。

Entity Framework (EF) は、O/RM 層です。 ASP.NET のオブジェクト指向の世界と基になるデータベースの間を仲介します。 データベースが LocalDB を使用しますが、これは、Web.config ファイルで変更できます。 通常は LocalDB を使用して、ローカル開発用なり、EF code first の移行を使用して、サーバー上の SQL データベースに展開します。

クライアント側では、Knockout.js ライブラリは、AJAX 要求からのページ更新を処理します。 Knockout では、データ バインディングを使用して、ページを最新のデータと同期します。 これにより、任意の JSON データについて説明し、DOM を更新するコードを記述する必要はありません。 代わりに、Knockout、データを表示する方法を指示する HTML で宣言型属性を配置します。

このアーキテクチャの大きな利点は、アプリケーション ロジックからプレゼンテーション層を分離します。 Web ページがどのように表示されるかについて何も知らなくても、Web API の部分を作成できます。 「ビュー モデル」を作成する、クライアント側でそのデータを表し、ビュー モデルでは、Knockout を使用して、HTML にバインドします。 ビュー モデルを変更することがなく、HTML を簡単に変更できます。 (見て Knockout 後で説明します。)

## <a name="models"></a>モデル

Visual Studio プロジェクトでは、Models フォルダーには、サーバー側で使用されるモデルが含まれています。 (クライアント側でのモデルもにわかります)。

![](knockoutjs-template/_static/image9.png)

**TodoItem、TodoList**

これらは、Entity Framework Code First のデータベース モデルです。 これらのモデルが互いを指すプロパティを持つことに注意してください。 `ToDoList` ToDoItems、およびそれぞれのコレクションを含む`ToDoItem`がその親 ToDoList への参照。 これらのプロパティは、ナビゲーション プロパティと呼ばれるされ、to do リストとその to do 項目一対多リレーションシップを表します。

`ToDoItem`クラスで使用でも、 **[不変]** ことを指定する属性`ToDoListId`への外部キーは、`ToDoList`テーブル。 これにより、EF がデータベースに foreign key 制約を追加するように指示します。

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto、TodoListDto**

これらのクラスは、クライアントに送信されるデータを定義します。 "DTO"の略「のデータ転送オブジェクトです」 DTO は、JSON に、エンティティのシリアル化する方法を定義します。 一般は、Dto を使用するいくつかの理由があります。

- プロパティがシリアル化を制御します。 DTO は、ドメイン モデルからプロパティのサブセットを含めることができます。 これを行います (機密性の高いデータを非表示にする) にセキュリティ上の理由から、または単に送信するデータの量を削減します。
- などのデータの形状を変更するより複雑なデータ構造をフラット化します。
- DTO (関心の分離) からすべてのビジネス ロジックを保持します。
- 場合は、ドメイン モデルは、何らかの理由によりシリアル化することはできません。 たとえば、循環参照問題が生じるがあるオブジェクトをシリアル化すると Web API では、この問題を処理する方法 (を参照してください[循環オブジェクト参照の処理](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); 完全、問題、DTO を使用して単に回避できますが、します。

SPA テンプレートでは、Dto には、ドメイン モデルと同じデータが含まれています。 ただし、これらは有用です、ナビゲーション プロパティからの循環参照を回避して、DTO の一般的なパターンを示すためです。

**AccountModels.cs**

このファイルには、サイトのメンバーシップのモデルが含まれています。 `UserProfile`クラスは、メンバーシップ DB でのユーザー プロファイルのスキーマを定義します。 (この場合、唯一の情報は、ユーザー ID とユーザー名) です。このファイルに他のモデル クラスを使用して、ユーザー登録、ログイン フォームを作成します。

## <a name="entity-framework"></a>Entity Framework

EF Code First SPA テンプレートを使用します。 Code First の開発では、コードでは、モデルを最初に定義して、EF でモデルを使用して、データベースを作成します。 既存のデータベースで、EF を使用することもできます ([Database First](https://msdn.microsoft.com/data/jj206878.aspx))。

`TodoItemContext`モデル フォルダー内のクラスから派生**DbContext**します。 このクラスは、モデルと EF の間の「グルー」を提供します。 `TodoItemContext`を保持する`ToDoItem`コレクションと`TodoList`コレクション。 データベースを照会するには、単にこれらのコレクションに対して LINQ クエリを記述する必要があります。 たとえば、すべてのユーザー"Alice"to do リストを選択する方法を示します。

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

コレクションに新しい項目を追加、項目を更新または、コレクションから項目を削除でき、データベースへの変更を保持できます。

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web API コント ローラー

ASP.NET Web api では、コント ローラーは、HTTP 要求を処理するオブジェクトです。 SPA テンプレートで Web API を使用して、CRUD 操作を有効にする前述のように、`ToDoList`と`ToDoItem`インスタンス。 コント ローラーは、ソリューションの Controllers フォルダーに配置されます。

![](knockoutjs-template/_static/image10.png)

- `TodoController`: To do 項目用の HTTP 要求を処理します。
- `TodoListController`: To do リストの HTTP 要求を処理します。

これらの名前は重要では Web API コント ローラー名を URI パスに一致するためです。 (Web API がコント ローラーに HTTP 要求をルーティングする方法については、次を参照してください[ASP.NET Web API におけるルーティング](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。)。

見て、`ToDoListController`クラス。 1 つのデータ メンバーが含まれています。

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext`前述のように、EF との通信に使用します。 コント ローラーのメソッドは、CRUD 操作を実装します。 Web API では、コント ローラーのメソッドに、クライアントから HTTP 要求をようにマップします。

| HTTP 要求 | コント ローラー メソッド | 説明 |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | To do リストのコレクションを取得します。 |
| GET /api/todo/*id* | `GetTodoList` | ID で to do リストを取得します |
| PUT /api/todo/*id* | `PutTodoList` | To do リストを更新します。 |
| POST /api/todo | `PostTodoList` | 新しい to do リストを作成します。 |
| DELETE /api/todo/*id* | `DeleteTodoList` | TODO リストを削除します。 |

ID 値のプレース ホルダーにはいくつかの操作の Uri が含まれていることがわかります。 たとえば、リストを削除するに - 42 の ID を持つ、URI は`/api/todo/42`します。

CRUD 操作用の Web API の使用方法の詳細については、次を参照してください。[そのサポートの CRUD 操作を作成するには、Web API](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md)します。 このコント ローラーのコードはとても簡単です。 いくつかの興味深い点を次に示します。

- `GetTodoLists`メソッドでは、LINQ クエリを使用して、ログイン ユーザーの ID で結果をフィルター処理します。 これにより、ユーザーには、自分が属しているデータのみが表示されます。 また、Select ステートメントを使用に変換することに注意してください、`ToDoList`インスタンスを`TodoListDto`インスタンス。
- PUT と POST メソッドは、データベースを変更する前に、モデルの状態を確認します。 場合**ModelState.IsValid**が false の場合、これらのメソッドは、HTTP 400 正しくない要求を返します。 モデルの検証で Web API の詳細については[モデルの検証](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md)です。
- コント ローラー クラスが修飾されても、 **[Authorize]** 属性。 この属性は、HTTP 要求が認証されているかどうかを確認します。 要求が認証されていない、クライアントは HTTP 401、許可されていません。 認証の詳細について[ASP.NET Web API で認証と承認](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md)します。

`TodoController`クラスはほぼ`TodoListController`します。 最も大きな違いは、クライアントは各 to do リストと共に to do 項目を取得するための GET メソッドを定義しません。

## <a name="mvc-controllers-and-views"></a>MVC コント ローラーとビュー

MVC コント ローラーは、ソリューションの Controllers フォルダーにもあります。 `HomeController` アプリケーションのメインの HTML をレンダリングします。 Home コント ローラーのビューは、Views/Home/Index.cshtml で定義されます。 ホーム ビューには、ユーザーがログインしているかどうかに応じてさまざまなコンテンツがレンダリングされます。

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

ユーザーがログオンして、主要な UI が表示されます。 それ以外の場合、ログイン パネルが表示されます。 この条件付きのレンダリングがサーバー側で行われますことに注意してください。 クライアント側で機密性の高いコンテンツを非表示にしようとすることはありません (&)、HTTP 応答で送信する #8212anything が生の HTTP メッセージを視聴者に表示されます。

## <a name="client-side-javascript-and-knockoutjs"></a>クライアント側の JavaScript と Knockout.js

これでアプリケーションをクライアントのサーバー側からしてみましょう。 SPA テンプレートは、smooth、対話型の UI を作成するのに、jQuery と Knockout.js の組み合わせを使用します。 Knockout.js では、JavaScript ライブラリをデータに HTML をバインドするが容易にします。 Knockout.js「モデル-ビュー-ビューモデル」という名前のパターンを使用します。

- モデルは、ドメイン データ (ToDo リストと ToDo 項目) です。
- ビューは、HTML ドキュメントです。
- ビュー モデルは、モデル データを保持する JavaScript オブジェクトです。 ビュー モデルは、UI のコードの抽象化です。 HTML 形式の知識がありません。 代わりに、「ToDo 項目の一覧」など、ビューの抽象の機能を表します。

ビューはデータ バインド ビュー モデルにします。 ビュー モデルに更新プログラムは、ビューで自動的に反映されます。 バインドの他の方向も動作します。 (クリックするなど)、DOM 内のイベント データ バインド関数、ビュー モデルで AJAX 呼び出しをトリガーします。

SPA テンプレートでは、3 つの層にクライアント側の JavaScript を編成されています。

- todo.datacontext.js: AJAX 要求を送信します。
- todo.model.js: モデルを定義します。
- todo.viewmodel.js: ビュー モデルを定義します。

![](knockoutjs-template/_static/image11.png)

これらのスクリプト ファイルは、ソリューションのスクリプト/アプリ フォルダーに配置されます。

![](knockoutjs-template/_static/image12.png)

**todo.datacontext** Web API コント ローラーのすべての AJAX 呼び出しを処理します。 (でのログ記録の AJAX 呼び出しで定義されます他の場所で ajaxlogin.js。)

**todo.model.js** to do リストのクライアント側 (ブラウザー) モデルを定義します。 2 つのモデル クラスがあります: todoItem todoList とします。

モデル クラスでプロパティの多くは、型"ko.observable"です。 観測可能なオブジェクトは、Knockout がそのマジックをどのようにします。 [Knockout ドキュメント](http://knockoutjs.com/documentation/introduction.html): オブザーバブルは「を JavaScript オブジェクトの変更についてサブスクライバーに通知できます」。 可能なオブジェクトの値が変更されたときに、Knockout はそれらの観測可能なオブジェクトにバインドされている任意の HTML 要素を更新します。 たとえば、todoItem では、観測可能なオブジェクトのタイトルと先読みプロパティがあります。

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

コードの観測可能なオブジェクトにサブスクライブすることもできます。 たとえば、todoItem クラスは「先読み」と"title"プロパティの変更をサブスクライブします。

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**ビュー モデル**

ビュー モデルは、todo.viewmodel.js で定義されます。 ビュー モデルは、中心点のアプリケーションがドメインのデータを HTML ページの要素をバインドします。 SPA テンプレートでは、ビュー モデルには、todoLists の観測可能な配列が含まれています。 ビュー モデルでは、次のコードは、バインドを適用する Knockout を指示します。

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML とデータ バインド

ページのメインの HTML は Views/Home/Index.cshtml で定義されます。 データ バインディングを使用しているため、HTML は、どのような実際にレンダリングを取得テンプレートのみです。 Knockout を使用して*宣言*バインドします。 ページ要素をデータにバインドするには、要素に「データ バインド」属性を追加します。 Knockout ドキュメントから抜粋した、非常に単純な例を次に示します。

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

この例では、Knockout での内容が更新、 **&lt;span&gt;** の値を持つ要素`myItems.count()`します。 この値が変更されるたびに、Knockout は、ドキュメントを更新します。

Knockout では、各種のバインディング数を提供します。 SPA テンプレートで使用されるバインディングの一部を次に示します。

- **foreach**: ループを反復処理し、一覧内の各項目に同じマークアップを適用することができます。 これは、to do リストと to do 項目を表示するために使用されます。 内で、 **foreach**リストの要素にバインドが適用されます。
- **表示される**: 表示を切り替えるために使用します。 コレクションが空では、マークアップを非表示にまたはエラー メッセージが表示されるようにします。
- **値**: フォーム値を設定するために使用します。
- **クリックして**: クリック イベントをビュー モデルの関数にバインドします。

## <a name="anti-csrf-protection"></a>CSRF 対策保護

クロスサイト リクエスト フォージェリ (CSRF) は、悪意のあるサイトがで、ユーザーが現在ログインしている脆弱性のあるサイトに要求を送信する攻撃です。 ASP.NET MVC が使用するには CSRF 攻撃を防ぐために、*偽造防止トークン*、確認トークンの要求とも呼ばれます。 考え方は、サーバーが web ページにランダムに生成されたトークンを格納することです。 クライアントは、サーバーにデータを送信するときに、要求メッセージ内でこの値を含める必要がありますに。

偽造防止トークンは、悪意のあるページが同一オリジン ポリシーにより、ユーザーのトークンを読み取ることができませんので機能します。 (同一オリジン ポリシーは、互いのコンテンツへのアクセスを 2 つの異なるサイトでホストされているドキュメントを禁止します)。

ASP.NET MVC で偽造防止トークンの組み込みサポートを提供します、[偽造防止](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx)クラスおよび[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx)属性。 現時点では、この機能は現在の Web API には組み込まれていません。 ただし、SPA テンプレートには、Web API のカスタム実装が含まれます。 このコードで定義、`ValidateHttpAntiForgeryTokenAttribute`クラスは、ソリューションの [フィルター] フォルダーにあります。 Web API での CSRF の詳細については、次を参照してください。[防止クロスサイト リクエスト フォージェリ (CSRF) 攻撃](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md)します。

## <a name="conclusion"></a>まとめ

SPA テンプレートは、最新の対話型の web アプリケーションをすばやく記述を開始するために設計されています。 プレゼンテーション (HTML マークアップ) のデータとアプリケーション ロジックからを分離するのに、Knockout.js ライブラリを使用します。 Knockout は、SPA を作成に使用できる唯一の JavaScript ライブラリではありません。 その他のオプションを表示する場合を見て、 [SPA テンプレートのコミュニティによって作成された](../templates/index.md)します。
