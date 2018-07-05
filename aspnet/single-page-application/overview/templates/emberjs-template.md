---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS テンプレート |Microsoft Docs
author: xqiu
description: EmberJS テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1f5b005180fed15c51b36417cdcd6acc98123c9a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374167"
---
<a name="emberjs-template"></a>EmberJS テンプレート
====================
によって[Xinyang Qiu](https://github.com/xqiu)

> EmberJS の MVC テンプレートは、Nathan Totten、Thiago Santos、および Xinyang Qiu によって書き込まれます。
> 
> [EmberJS MVC テンプレートをダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=282647)


EmberJS SPA テンプレートは EmberJS を使用して対話型のクライアント側 web アプリをすばやく構築を開始するために設計されています。

「シングル ページ アプリケーション」(SPA) は、1 つの HTML ページが読み込まれ、新しいページの読み込みではなく、ページを動的に更新する web アプリケーションの一般的な用語です。 最初のページの読み込み後に、SPA は、AJAX 要求を介してサーバーとで説明します。

![](emberjs-template/_static/image1.png)

AJAX は目新しいものでは、現在 JavaScript フレームワークを構築し、大規模な高度な SPA アプリケーションを管理するが容易では。 また、HTML 5、CSS3 が簡単に豊富な Ui を作成します。

EmberJS SPA テンプレートを使用して、 [Ember](http://emberjs.com/) AJAX 要求からのページ更新を処理する JavaScript ライブラリです。 Ember.js では、データ バインディングを使用して、ページを最新のデータと同期します。 これにより、任意の JSON データについて説明し、DOM を更新するコードを記述する必要はありません。 代わりに、Ember.js、データを表示する方法を指示する HTML で宣言型属性を配置します。

EmberJS テンプレートのとほぼ同じですが、サーバー側で、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)します。 HTML ドキュメント、およびクライアントからの AJAX 要求を処理するために ASP.NET Web API を処理するために、ASP.NET MVC を使用します。 テンプレートの機能についての詳細についてを参照してください、 [KnockoutJS テンプレート](../introduction/knockoutjs-template.md)ドキュメント。 このトピックでは、Knockout テンプレートと EmberJS テンプレートの違いについて説明します。

## <a name="create-an-emberjs-spa-template-project"></a>EmberJS SPA テンプレート プロジェクトを作成します。

ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。 Visual Studio を再起動する必要があります。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトの名前し、クリックして**OK**します。

![](emberjs-template/_static/image2.png)

**新しいプロジェクト**ウィザードで、 **Ember.js SPA プロジェクト**します。

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA テンプレートの概要

EmberJS テンプレートは、jQuery、Ember.js、smooth、対話型の UI を作成する Handlebars.js の組み合わせを使用します。

Ember.js は、クライアント側の MVC パターンを使用して JavaScript ライブラリです。

- A*テンプレート*、Handlebars テンプレート作成言語で記述されたアプリケーションのユーザー インターフェイスについて説明します。 リリース モードで、 [Handlebars コンパイラ](https://github.com/Myslik/csharp-ember-handlebars)バンドルし、handlebars テンプレートのコンパイルに使用されます。
- A*モデル*(ToDo リストと ToDo 項目)、サーバーから取得したアプリケーション データを格納します。
- A*コント ローラー*アプリケーションの状態を格納します。 コント ローラーは、多くの場合、対応するテンプレート モデル データを提示します。
- A*ビュー*アプリケーションのプリミティブのイベントを変換し、コント ローラーに渡されます。
- A*ルーター* Url とテンプレートの同期を保つ、アプリケーションの状態を管理します。

さらに、Ember データ ライブラリを使用して、(RESTful API を使用してサーバーから取得された) JSON オブジェクトとクライアントのモデルを同期することができます。

EmberJS SPA テンプレートには、8 つの層に、スクリプトが編成されています。

- webapi\_adapter.js、webapi\_serializer.js: ASP.NET Web API を使用する Ember データ ライブラリを拡張します。
- Scripts/helpers.js: は、新しい Ember Handlebars ヘルパーを定義します。
- Scripts/app.js: は、アプリを作成し、アダプターとシリアライザーを構成します。
- スクリプト/アプリ/モデル/\*.js: モデルを定義します。
- スクリプト/アプリ/ビュー/\*.js: ビューを定義します。
- コント ローラー アプリケーション/スクリプト//\*.js: コント ローラーを定義します。
- スクリプト/アプリ/ルート、Scripts/app/router.js: ルートを定義します。
- テンプレート/\*.hbs: handlebars テンプレートを定義します。

詳細でこれらのスクリプトのいくつかを見てみましょう。

## <a name="models"></a>モデル

モデルは、スクリプト/アプリ/models フォルダーで定義されます。 2 つのモデル ファイルがある: todoItem.js と todoList.js します。

**todo.model.js** to do リストのクライアント側 (ブラウザー) モデルを定義します。 2 つのモデル クラスがあります: todoItem todoList とします。 Ember、モデルとは、DS のサブクラスです。モデル。 モデル プロパティの属性を持つことができます。

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

モデルは、その他のモデルのリレーションシップを定義できます。

[!code-css[Main](emberjs-template/samples/sample2.css)]

モデルには、その他のプロパティにバインドするプロパティが計算ことができます。

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

モデルには、オブザーバー関数は、監視対象のプロパティが変更されたときに呼び出されることがあります。

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Views

ビューは、スクリプト/アプリ/ビュー フォルダーで定義されます。 ビューは、アプリケーションの UI からのイベントを変換します。 イベント ハンドラーは、コント ローラーの関数にコールバックまたはデータ コンテキストを直接呼び出します。

たとえば、次のコードは views/TodoItemEditView.js します。 イベント処理、テキスト入力フィールドを定義します。

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>コントローラー

コント ローラーは、スクリプト/アプリ/controllers フォルダーで定義されます。 1 つのモデルを表現する拡張`Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

コント ローラーを拡張することによってモデルのコレクションを表すことができますも`Ember.ArrayController`します。 たとえば、TodoListController がの配列を表します`todoList`オブジェクト。 コント ローラーを todoList id、降順で並べ替えます。

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

コント ローラーという名前の関数を定義する`addTodoList`、新しい todoList を作成し、配列に追加します。 この関数が呼び出される方法を表示するには、テンプレート フォルダー内の todoListTemplate.html をという名前のテンプレート ファイルを開きます。 次のテンプレート コードにバインドするためのボタン、`addTodoList`関数。

[!code-html[Main](emberjs-template/samples/sample8.html)]

コント ローラーにも含まれています、`error`プロパティで、エラー メッセージを保持します。 (TodoListTemplate.html) でもエラー メッセージを表示するテンプレート コードを次に示します。

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>ルート

Router.js では、ルートとアプリケーションの状態のセットを表示します。 既定のテンプレートを定義し、ルート Url と一致します。

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js は、setupController 関数をオーバーライドすることで、TodoListRoute のデータを読み込みます。

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember は、Url、ルート名、コント ローラー、およびテンプレートに一致するように、名前付け規則を使用します。 詳細については、次を参照してください。 [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS ドキュメント。

## <a name="templates"></a>テンプレート

テンプレート フォルダーには、4 つのテンプレートが含まれています。

- application.hbs: アプリケーションの起動時に表示される既定のテンプレート。
- about.hbs:「/約」ルート テンプレート。
- index.hbs: ルートのテンプレート「/」のルート。
- todoList.hbs: テンプレートを"/todo"ルート。
- \_navbar.hbs: テンプレートは、ナビゲーション メニューを定義します。

アプリケーション テンプレートは、マスター ページと同様に機能します。 ヘッダー、フッター、および「{{アウトレット}}」をルートによってでは、その他のテンプレートを挿入するが含まれています。 Ember のアプリケーション テンプレートの詳細については、次を参照してください。 [ http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)します。

"/TodoList"テンプレートには、2 つのループ式が含まれています。 外側のループが`{{#each controller}}`、および内部ループは`{{#each todos}}`します。 次のコードは、組み込み`Ember.Checkbox`表示、カスタマイズされた`App.TodoItemEditView`とのリンクを`deleteTodo`アクション。

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs で定義されているクラス定義のヘルパー関数をキャッシュし、テンプレートの挿入時にファイル**デバッグ**に設定されている**true** Web.config ファイルで。 この関数は Views/Home/App.cshtml で定義されている ASP.NET MVC ビュー ファイルから呼び出されます。

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

関数を引数なしで呼び出されると、すべてのテンプレート ファイル テンプレート フォルダーをレンダリングします。 サブフォルダーまたは特定のテンプレート ファイルを指定することもできます。

ときに**デバッグ**は**false** 、web.config ファイルで、アプリケーションにはバンドルの項目"~/bundles/templates"が含まれています。 BundleConfig.cs、Handlebars コンパイラのライブラリを使用するのには、バンドルは、この項目が追加されます。

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
