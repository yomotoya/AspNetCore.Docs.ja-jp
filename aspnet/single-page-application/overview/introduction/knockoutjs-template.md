---
uid: single-page-application/overview/introduction/knockoutjs-template
title: "Single Page Application: KnockoutJS テンプレート |Microsoft ドキュメント"
author: MikeWasson
description: "Knockout テンプレート"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: f9c07af0-4b20-4b08-af8f-47fc3df169a2
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/introduction/knockoutjs-template
msc.type: authoredcontent
ms.openlocfilehash: e6c0c45bed098a8a1160ff11e4f77244bf55ffd3
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/15/2018
---
<a name="single-page-application-knockoutjs-template"></a>Single Page Application: KnockoutJS テンプレート
====================
作成者 [Mike Wasson](https://github.com/MikeWasson)

> ASP.NET および Web ツール 2012.2 の一部である Knockout MVC テンプレート
> 
> [ASP.NET および Web ツール 2012.2 をダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=282650)


ASP.NET および Web ツール 2012.2 更新には、ASP.NET MVC 4 用の単一ページ アプリケーション (SPA) テンプレートが含まれています。 このテンプレートは、対話型のクライアント側 web アプリケーションの構築をすばやく開始するために設計されています。

「単一ページ アプリケーション」(SPA) が 1 つの HTML ページが読み込まれ、新しいページを読み込むのではなく、ページを動的に更新する web アプリケーションの一般的な用語です。 最初のページ読み込み後、SPA が AJAX 要求を使用してサーバーを説明します。

![](knockoutjs-template/_static/image1.png)

AJAX が現在では、JavaScript フレームワークをビルドし、大規模な高度な SPA アプリケーションを管理しやすくします。 また、html5 および css3 用がしやすく豊富な Ui を作成します。

開始に役立つは、SPA テンプレートは、サンプルの「to do リスト」アプリケーションを作成します。 このチュートリアルでは、テンプレートのガイド付きツアーをみましょう。 まず、to do リスト、アプリケーション自体を確認し、動作を構成するテクノロジの部分を調べるおします。

## <a name="create-a-new-spa-template-project"></a>新しい SPA テンプレート プロジェクトを作成します。

要件:

- Visual Studio 2012 または Visual Studio Express 2012 for Web
- ASP.NET Web ツール 2012.2 を更新します。 更新プログラムをインストールする[ここ](https://www.microsoft.com/web/handlers/webpi.ashx?command=getinstallerredirect&appid=ASPDOTNETandWebTools2012_2)です。

Visual Studio を起動し、選択**新しいプロジェクト**スタート ページからです。 またはから、**ファイル**メニューの **新規**し**プロジェクト**です。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。 プロジェクトの名前を指定し、をクリックして**OK**です。

![](knockoutjs-template/_static/image2.png)

**新しいプロジェクト**ウィザードで、 **Single Page Application**です。

![](knockoutjs-template/_static/image3.png)

F5 キーを押してアプリケーションをビルドし、実行します。 アプリケーションを最初に実行すると、ログイン画面が表示されます。

![](knockoutjs-template/_static/image4.png)

クリックして、&quot;サインアップ&quot;リンクし、新しいユーザーを作成します。

![](knockoutjs-template/_static/image5.png)

ログインした後、アプリケーションは、次の 2 つの項目を含む既定の Todo リストを作成します。 "Todo リストの追加 をクリックすることができます、新しいリストを追加します。

![](knockoutjs-template/_static/image6.png)

リストの名前を変更の一覧に項目を追加し、それらのチェックします。 項目を削除したり、リスト全体を削除できます。 変更は、サーバー上のデータベースに自動的に保存されます (この時点では、LocalDB 実際には、アプリケーションをローカルで実行しているため)。

![](knockoutjs-template/_static/image7.png)

## <a name="architecture-of-the-spa-template"></a>SPA テンプレートのアーキテクチャ

この図では、アプリケーションのメインのビルド ブロックを示します。

![](knockoutjs-template/_static/image8.png)

サーバー側では、ASP.NET MVC は、HTML を提供しもフォーム ベース認証を処理します。

ASP.NET Web API は、ToDoItems、取得、作成、更新、および削除を含む、ToDoLists に関連するすべての要求を処理します。 クライアントは、JSON 形式で Web API でのデータを交換します。

Entity Framework (EF) は、O/RM 層です。 ASP.NET のオブジェクト指向 world と基になるデータベースの間を仲介します。 データベースが LocalDB を使用しますが、これは、Web.config ファイルで変更できます。 通常を使用して LocalDB ローカル開発用 EF コード優先の移行を使用して、サーバー上の SQL データベースに展開します。

クライアント側では、Knockout.js ライブラリは、AJAX 要求からのページ更新を処理します。 Knockout では、データ バインディングを使用して、ページを最新のデータと同期します。 JSON データ順を追っておよび DOM を更新するコードの記述がないようにして、 代わりに、宣言型の属性は、Knockout にデータを表示する方法を指示する HTML に配置します。

このアーキテクチャの大きな利点は、アプリケーション ロジックからプレゼンテーション層が分離されることです。 Web API の部分は、web ページがどのように表示されるかについての知識がなくても作成できます。 「ビュー モデル」を作成する、クライアント側でそのデータを表すビュー モデルを使用して Knockout を HTML にバインドします。 ビュー モデルを変更することがなく、HTML を簡単に変更することができます。 (見ていきます Knockout、少し後でします。)

## <a name="models"></a>モデル

Visual Studio プロジェクトには、Models フォルダーには、サーバー側で使用されるモデルが含まれています。 (クライアント側でのモデルはまた、ものが表示されます)。

![](knockoutjs-template/_static/image9.png)

**TodoItem、TodoList**

これらは、Entity Framework Code First のデータベース モデルです。 これらのモデルが互いを指すプロパティを持つことに注意してください。 `ToDoList` ToDoItems、およびそれぞれのコレクションを含む`ToDoItem`がその親 ToDoList への参照。 これらのプロパティと呼ばれるナビゲーションされ、to do リストとそのタスク項目一対多リレーションシップを表します。

`ToDoItem`クラスも使用して、 **[ForeignKey]**ことを指定する属性`ToDoListId`への外部キーは、`ToDoList`テーブル。 これは、データベースに foreign key 制約を追加する EF を示しています。

[!code-csharp[Main](knockoutjs-template/samples/sample1.cs)]

**TodoItemDto、TodoListDto**

これらのクラスは、クライアントに送信されるデータを定義します。 データの転送オブジェクト"の"は"DTO"を表します DTO では、エンティティを JSON にシリアル化する方法を定義します。 一般は dtos の使用を使用するいくつかの理由があります。

- 制御するプロパティはシリアル化します。 DTO には、ドメイン モデルからプロパティのサブセットを含めることができます。 これを行います (機密データを非表示) をセキュリティ上の理由から、または単に送信されるデータの量を削減します。
- たとえば、データの形状を変更するより複雑なデータ構造を平坦化します。
- DTO (関心の分離) 外の任意のビジネス ロジックを保持します。
- 場合は、ドメイン モデルは、何らかの理由によりシリアル化できません。 たとえば、循環参照問題が発生があるオブジェクトをシリアル化するときに Web API では、この問題を処理する方法 (を参照してください[循環オブジェクト参照の処理](../../../web-api/overview/formats-and-model-binding/json-and-xml-serialization.md#handling_circular_object_references)); DTO を使用して単に問題を回避して、完全がします。

SPA テンプレートで、dtos の使用には、ドメイン モデルと同じデータが含まれています。 ただし、これらはでも役立ちますしないのは、循環参照ナビゲーション プロパティの一般的なパターンで DTO 例も示します。

**AccountModels.cs**

このファイルには、サイトのメンバーシップのモデルが含まれています。 `UserProfile`クラスは、メンバーシップ DB でのユーザー プロファイルのスキーマを定義します。 (この場合、唯一の情報は、ユーザー ID とユーザー名) です。このファイルに他のモデル クラスを使用して、ユーザーの登録とログイン フォームを作成します。

## <a name="entity-framework"></a>Entity Framework

SPA テンプレート EF Code First を使用します。 Code First の開発でコードでは、モデルを最初に定義するし、EF モデルを使用してデータベースを作成します。 既存のデータベースで EF を使用することもできます ([Database First](https://msdn.microsoft.com/data/jj206878.aspx))。

`TodoItemContext`モデル フォルダー内のクラスから派生**DbContext**です。 このクラスは、モデルと EF 間「グルー」を提供します。 `TodoItemContext`を保持する`ToDoItem`コレクションおよび`TodoList`コレクション。 データベースを照会するには、単にこれらのコレクションに対して LINQ クエリを記述する必要があります。 たとえば、ユーザー"Alice"のタスク一覧のすべてを選択する方法を示します。

[!code-csharp[Main](knockoutjs-template/samples/sample2.cs)]

新しい項目をコレクションに追加する、項目を更新またはコレクションから項目を削除できデータベースへの変更を保持できます。

## <a name="aspnet-web-api-controllers"></a>ASP.NET Web API コント ローラー

ASP.NET Web API では、コント ローラーは、HTTP 要求を処理するオブジェクトです。 SPA テンプレートで Web API を使用してでの CRUD 操作を有効にするように、`ToDoList`と`ToDoItem`インスタンス。 コント ローラーは、ソリューションのコント ローラーのフォルダーにあります。

![](knockoutjs-template/_static/image10.png)

- `TodoController`: 作業項目の HTTP 要求を処理します。
- `TodoListController`: タスク一覧の HTTP 要求を処理します。

これらの名前は、Web API コント ローラー名を URI パスに一致するために、考慮されます。 (Web API がコント ローラーに HTTP 要求をルーティングする方法については、次を参照してください[ASP.NET Web API でルーティング](../../../web-api/overview/web-api-routing-and-actions/routing-in-aspnet-web-api.md)。)。

見てみましょう、`ToDoListController`クラスです。 1 つのデータ メンバーが含まれています。

[!code-csharp[Main](knockoutjs-template/samples/sample3.cs)]

`TodoItemContext`前述のように、EF との通信に使用します。 コント ローラーのメソッドは、CRUD 操作を実装します。 Web API では、コント ローラーのメソッドに、クライアントからの HTTP 要求をようにマップします。

| HTTP 要求 | コント ローラー メソッド | 説明 |
| --- | --- | --- |
| GET /api/todo | `GetTodoLists` | タスク一覧のコレクションを取得します。 |
| GET /api/todo/*id* | `GetTodoList` | ID でタスク一覧を取得します。 |
| PUT /api/todo/*id* | `PutTodoList` | To do リストを更新します。 |
| POST /api/todo | `PostTodoList` | 新しいタスク一覧を作成します。 |
| DELETE /api/todo/*id* | `DeleteTodoList` | TODO リストを削除します。 |

一部の操作の Uri に ID 値のプレース ホルダーが含まれていることがわかります。 たとえば、リストを削除するに - 42 の ID を持つ、URI は`/api/todo/42`します。

CRUD 操作の Web API の使用に関する詳細については、次を参照してください。 [CRUD 操作をサポートする Web API を作成する](../../../web-api/overview/older-versions/creating-a-web-api-that-supports-crud-operations.md)です。 このコント ローラーのコードはきわめて簡単です。 いくつかの興味深い点を次に示します。

- `GetTodoLists`メソッドでは、LINQ クエリを使用して、ログイン ユーザーの ID で結果をフィルター処理します。 このように、ユーザーには、その人に属するデータのみが表示されます。 また、変換に、Select ステートメントを使用することを確認、`ToDoList`インスタンスを`TodoListDto`インスタンス。
- PUT や POST メソッドでは、データベースを変更する前にモデルの状態を確認します。 場合**ModelState.IsValid**が false の場合、これらのメソッドは、HTTP 400 Bad Request を返します。 詳しくは、Web API でのモデル検証[モデルの検証](../../../web-api/overview/formats-and-model-binding/model-validation-in-aspnet-web-api.md)です。
- コント ローラーのクラスがで装飾されても、 **[Authorize]**属性。 この属性は、HTTP 要求が認証されるかどうかを確認します。 クライアントが HTTP 401 を受け取る場合は、要求が認証されていない権限がありません。 認証の詳細を参照[ASP.NET Web API で認証と承認](../../../web-api/overview/security/authentication-and-authorization-in-aspnet-web-api.md)です。

`TodoController`クラスは類似`TodoListController`です。 最も大きな違いは、クライアントは、各タスクの一覧と共に作業アイテムが取得されるため、GET メソッドを定義しません。

## <a name="mvc-controllers-and-views"></a>MVC コント ローラーとビュー

MVC コント ローラーは、ソリューションのコント ローラーのフォルダーにあります。 `HomeController` アプリケーションのメインの HTML をレンダリングします。 Home コント ローラーのビューは、Views/Home/Index.cshtml で定義されます。 ホーム ビューは、ユーザーがログインしているかどうかに応じて異なるコンテンツを表示します。

[!code-cshtml[Main](knockoutjs-template/samples/sample4.cshtml)]

ユーザーがログインしている、主要な UI が表示されます。 それ以外の場合、ログイン パネルが表示されます。 サーバー側でこの条件付き表示が行われることに注意してください。 クライアント側で機密性の高いコンテンツを非表示を試行しません (&) を HTTP 応答で送信する #8212anything は生の HTTP メッセージを監視しているユーザーに表示します。

## <a name="client-side-javascript-and-knockoutjs"></a>クライアント側の JavaScript および Knockout.js

今すぐアプリケーションをクライアントのサーバー側の有効みましょう。 SPA テンプレートでは、jQuery、および Knockout.js の組み合わせを使用して、smooth、対話型の UI を作成します。 Knockout.js は、HTML をデータにバインドしやすく JavaScript ライブラリです。 Knockout.js"モデル-ビュー-ViewModel です"と呼ばれるパターンを使用します。

- モデルは、ドメインのデータ (ToDo リストと ToDo 項目) です。
- ビューは、HTML ドキュメントです。
- ビュー モデルは、モデル データを保持する JavaScript オブジェクトです。 ビュー モデルは、UI のコードの抽象化です。 HTML 形式の知識がないです。 代わりに、「ToDo 項目の一覧」など、ビューの抽象機能を表します。

ビュー データにバインドされてビュー モデルです。 ビュー モデルへの更新は、ビューに自動的に反映されます。 バインディングは、逆方向にも動作します。 (クリックするなど)、DOM 内のイベント データ バインド関数をビュー モデルに AJAX 呼び出しをトリガーします。

SPA テンプレートには、3 つの層に、クライアント側 JavaScript が編成されています。

- todo.datacontext.js: AJAX 要求を送信します。
- todo.model.js: モデルを定義します。
- todo.viewmodel.js: ビュー モデルを定義します。

![](knockoutjs-template/_static/image11.png)

これらのスクリプト ファイルは、ソリューションのスクリプト/アプリケーション フォルダーにあります。

![](knockoutjs-template/_static/image12.png)

**todo.datacontext** Web API コント ローラーのすべての AJAX 呼び出しを処理します。 (でのログ記録の AJAX 呼び出しがで定義されている、ajaxlogin.js。)

**todo.model.js** to do リストに表示するクライアント側 (ブラウザー) モデルを定義します。 2 つのモデル クラス: todoItem todoList とします。

モデルのクラスのプロパティの多くは、型"ko.observable"です。 観測可能なオブジェクトは、そのマジック Knockout のしくみです。 [Knockout ドキュメント](http://knockoutjs.com/documentation/introduction.html): 監視対象は、「を JavaScript オブジェクトの変更はサブスクライバーに通知できます」。 監視対象の値が変更されたときに、Knockout はそれらの観測可能なオブジェクトにバインドされている HTML 要素を更新します。 たとえば、todoItem では、観測可能なオブジェクトのタイトルと先読みプロパティがあります。

[!code-javascript[Main](knockoutjs-template/samples/sample5.js)]

コードで観測可能なオブジェクトをサブスクライブすることもできます。 たとえば、todoItem クラスは、「先読み」および"title"プロパティの変更をサブスクライブしています。

[!code-javascript[Main](knockoutjs-template/samples/sample6.js)]

**ビュー モデル**

ビュー モデルは、todo.viewmodel.js で定義されます。 ビュー モデルは、中心点のアプリケーションがドメインのデータを HTML ページの要素をバインドする場所です。 SPA テンプレートでは、ビュー モデルには、todoLists の監視可能な配列が含まれています。 ビュー モデルには、次のコードは、バインドを適用する Knockout を指示します。

[!code-javascript[Main](knockoutjs-template/samples/sample7.js)]

## <a name="html-and-data-binding"></a>HTML およびデータ バインディング

メイン ページの HTML は Views/Home/Index.cshtml で定義されます。 データ バインディングを使用しているため、HTML、どのような実際に表示される取得のテンプレートだけです。 Knockout を使用して*宣言*バインドします。 ページの要素をデータにバインドするには、要素に「データ バインド」属性を追加します。 Knockout ドキュメントからの抜粋、非常に単純な例を次に示します。

[!code-html[Main](knockoutjs-template/samples/sample8.html)]

Knockout 更新の内容をこの例では、  **&lt;span&gt;** の値を持つ要素`myItems.count()`です。 この値が変更されるたびに、Knockout はドキュメントを更新します。

Knockout は異なるバインディングの種類の数を提供します。 SPA テンプレートで使用されるバインディングの一部を次に示します。

- **foreach**: ループを反復処理し、一覧内の各項目に同じマークアップを適用することができます。 これは、to do リストとタスク アイテムを表示するために使用されます。 内で、 **foreach**バインドがリストの要素に適用されます。
- **表示されている**: 表示を切り替えるために使用します。 非表示にするコレクションが空、またはエラー メッセージが表示されるようにします。
- **値**: フォームの値を設定するために使用します。
- **をクリックして**: クリック イベントをビュー モデルの関数をバインドします。

## <a name="anti-csrf-protection"></a>アンチ CSRF 保護

クロスサイト リクエスト フォージェリ (CSRF) は、攻撃が悪意のあるサイトは、ユーザーが現在記録される場所で脆弱なサイトに要求を送信します。 ASP.NET MVC を使用して CSRF 攻撃を防ぐため、*偽造防止トークン*、確認トークンの要求とも呼ばれます。 つまり、サーバーが、web ページにランダムに生成されたトークンを格納します。 クライアントは、サーバーにデータを送信するときに、この値、要求メッセージにいる必要があります。

偽造防止トークンは動作ため、悪意のあるページが同じオリジン ポリシーにより、ユーザーのトークンを読み取ることができません。 (同じオリジンのポリシーは、互いのコンテンツへのアクセスから 2 つの異なるサイトでホストされているドキュメントを避けるため)。

ASP.NET MVC は、経由した偽造防止トークンの組み込みサポートを提供、 [AntiForgery](https://msdn.microsoft.com/library/system.web.helpers.antiforgery.aspx)クラスおよび[[ValidateAntiForgeryToken]](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute.aspx)属性。 現時点では、この機能では、Web API に組み込まれていません。 ただし、SPA テンプレートには、Web API のカスタム実装が含まれています。 このコードがで定義されている、`ValidateHttpAntiForgeryTokenAttribute`ソリューションの [フィルター] フォルダーにあるクラスです。 Web API でのアンチ CSRF の詳細については、次を参照してください。[防止クロスサイト リクエスト フォージェリ (CSRF) 攻撃](../../../web-api/overview/security/preventing-cross-site-request-forgery-csrf-attacks.md)です。

## <a name="conclusion"></a>まとめ

SPA テンプレートは、すばやくを対話形式で最新の web アプリケーションの作成を開始するために設計されています。 Knockout.js ライブラリを使用してデータとアプリケーションのロジックからプレゼンテーション (HTML マークアップ) を区切ります。 Knockout は、SPA を作成に使用できる唯一の JavaScript ライブラリではありません。 その他のいくつかのオプションを探索する場合を見て、 [SPA テンプレートのコミュニティによって作成された](../templates/index.md)です。
