---
uid: single-page-application/overview/templates/emberjs-template
title: EmberJS テンプレート |Microsoft ドキュメント
author: xqiu
description: EmberJS テンプレート
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/30/2013
ms.topic: article
ms.assetid: 04d5f142-5f62-494a-b5ea-4f3d068d34cb
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/emberjs-template
msc.type: authoredcontent
ms.openlocfilehash: 1fb7633aee288be648d4f9681b43c8911b7dbab9
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "26506801"
---
<a name="emberjs-template"></a>EmberJS テンプレート
====================
によって[Xinyang Qiu](https://github.com/xqiu)

> EmberJS MVC テンプレートは、Nathan Totten、Thiago Santos および Xinyang Qiu によって書き込まれます。
> 
> [EmberJS MVC テンプレートをダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=282647)


EmberJS SPA テンプレートは EmberJS を使用して対話型のクライアント側 web アプリの構築をすばやく開始するために設計されています。

「単一ページ アプリケーション」(SPA) が 1 つの HTML ページが読み込まれ、新しいページを読み込むのではなく、ページを動的に更新する web アプリケーションの一般的な用語です。 最初のページ読み込み後、SPA が AJAX 要求を使用してサーバーを説明します。

![](emberjs-template/_static/image1.png)

AJAX が現在では、JavaScript フレームワークをビルドし、大規模な高度な SPA アプリケーションを管理しやすくします。 また、html5 および css3 用がしやすく豊富な Ui を作成します。

EmberJS SPA テンプレートを使用して、 [Ember](http://emberjs.com/) AJAX 要求からのページ更新を処理する JavaScript ライブラリです。 Ember.js では、データ バインディングを使用して、ページを最新のデータと同期します。 JSON データ順を追っておよび DOM を更新するコードの記述がないようにして、 代わりに、宣言型の属性は、Ember.js にデータを表示する方法を指示する HTML に配置します。

サーバー側で EmberJS テンプレートはほぼ同じである、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)です。 ASP.NET MVC を使用して、HTML ドキュメント、および、クライアントからの AJAX 要求を処理するのに ASP.NET Web API を提供します。 テンプレートの機能についての詳細についてを参照してください、 [KnockoutJS テンプレート](../introduction/knockoutjs-template.md)ドキュメント。 このトピックでは、Knockout テンプレートと EmberJS テンプレートの違いについて説明します。

## <a name="create-an-emberjs-spa-template-project"></a>EmberJS SPA テンプレート プロジェクトを作成します。

ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。 Visual Studio を再起動する必要があります。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。 プロジェクトの名前を指定し、をクリックして**OK**です。

![](emberjs-template/_static/image2.png)

**新しいプロジェクト**ウィザードで、 **Ember.js SPA プロジェクト**です。

![](emberjs-template/_static/image4.png)

## <a name="emberjs-spa-template-overview"></a>EmberJS SPA テンプレートの概要

EmberJS テンプレートは、jQuery、Ember.js、smooth、対話型の UI を作成する Handlebars.js の組み合わせを使用します。

Ember.js は、クライアント側の MVC パターンを使用して JavaScript ライブラリです。

- A*テンプレート*ハンドル テンプレート言語で記述されたアプリケーションのユーザー インターフェイスについて説明します。 リリース モードで、[ハンドル コンパイラ](https://github.com/Myslik/csharp-ember-handlebars)をバンドルしてハンドル テンプレートをコンパイルするために使用します。
- A*モデル*(ToDo リストと ToDo 項目) のサーバーから取得するアプリケーション データを格納します。
- A*コント ローラー*アプリケーションの状態を格納します。 コント ローラーは、多くの場合に、対応するテンプレートのモデル データを表示します。
- A*ビュー*プリミティブからのイベント、アプリケーションを変換し、コント ローラーに渡されます。
- A*ルーター* Url とテンプレートの同期を保つのアプリケーションの状態を管理します。

さらに、Ember データ ライブラリ (rest ベースの API を使用してサーバーから取得された) JSON オブジェクトと、クライアントのモデルを同期するために使用できます。

EmberJS SPA テンプレート スクリプトを 8 つのレイヤーに整理して示します。

- webapi\_adapter.js、webapi\_serializer.js: ASP.NET Web API を使用する Ember データ ライブラリを拡張します。
- Scripts/helpers.js: は、新しい Ember ハンドル ヘルパーを定義します。
- Scripts/app.js: は、アプリを作成し、アダプターとシリアライザーを構成します。
- スクリプト/アプリ/モデル/\*.js: モデルを定義します。
- スクリプト/アプリケーション/ビュー/\*.js: ビューを定義します。
- コント ローラー アプリケーション/スクリプト//\*.js: コント ローラーを定義します。
- スクリプト/アプリケーション/ルート、Scripts/app/router.js: ルートを定義します。
- テンプレート/\*.hbs: ハンドル テンプレートを定義します。

これらのスクリプトで詳しく見てみましょう。

## <a name="models"></a>モデル

モデルは、スクリプト/アプリケーション/models フォルダーで定義されます。 2 つのモデル ファイルがある: todoItem.js と todoList.js です。

**todo.model.js** to do リストに表示するクライアント側 (ブラウザー) モデルを定義します。 2 つのモデル クラス: todoItem todoList とします。 Ember、モデルとは、DS のサブクラスです。モデル。 モデルには、属性を持つプロパティを持つことができます。

[!code-javascript[Main](emberjs-template/samples/sample1.js)]

モデルは、その他のモデルのリレーションシップを定義できます。

[!code-css[Main](emberjs-template/samples/sample2.css)]

モデルには、その他のプロパティにバインドするプロパティが計算されますことができます。

[!code-javascript[Main](emberjs-template/samples/sample3.js)]

モデルはオブザーバー関数は、検出されたプロパティが変更されたときに呼び出されるを持つことができます。

[!code-javascript[Main](emberjs-template/samples/sample4.js)]

## <a name="views"></a>Views

ビューは、スクリプト/アプリケーション/ビュー フォルダーで定義されます。 ビューは、アプリケーションの UI からのイベントを変換します。 イベント ハンドラーは、コント ローラーの機能をコールバックまたはデータ コンテキストを直接呼び出します。

たとえば、次のコードは views/TodoItemEditView.js です。 イベント テキスト入力フィールドの処理を定義します。

[!code-javascript[Main](emberjs-template/samples/sample5.js)]

## <a name="controller"></a>コントローラー

コント ローラーは、スクリプト/アプリケーション/controllers フォルダーで定義されます。 1 つのモデルを表現する拡張`Ember.ObjectController`:

[!code-javascript[Main](emberjs-template/samples/sample6.js)]

コント ローラーを表す場合もモデルのコレクションを拡張して`Ember.ArrayController`です。 たとえば、TodoListController がの配列を表します`todoList`オブジェクト。 コント ローラーは、todoList の id を降順で並べ替えます。

[!code-javascript[Main](emberjs-template/samples/sample7.js)]

コント ローラーという名前の関数を定義する`addTodoList`、これを新しい todoList を作成し、配列に追加します。 この関数が呼び出される方法を表示するには、テンプレート フォルダーに、todoListTemplate.html をという名前のテンプレート ファイルを開きます。 次のテンプレート コードにバインドするためのボタン、`addTodoList`関数。

[!code-html[Main](emberjs-template/samples/sample8.html)]

コント ローラーにも含まれています、`error`プロパティで、エラー メッセージを保持します。 (TodoListTemplate.html) でもエラー メッセージを表示するテンプレート コードを次に示します。

[!code-html[Main](emberjs-template/samples/sample9.html)]

## <a name="routes"></a>ルート

Router.js はルートと表示、アプリケーションの状態を設定する既定のテンプレートを定義し、ルート Url と一致します。

[!code-javascript[Main](emberjs-template/samples/sample10.js)]

TodoListRoute.js は、setupController 関数をオーバーライドすることで、TodoListRoute のデータを読み込みます。

[!code-javascript[Main](emberjs-template/samples/sample11.js)]

Ember では、名前付け規則を使用して、Url、ルート名、コント ローラー、およびテンプレートに一致します。 詳細については、次を参照してください。 [ http://emberjs.com/guides/routing/defining-your-routes/ ](http://emberjs.com/guides/routing/defining-your-routes/) EmberJS ドキュメントを参照します。

## <a name="templates"></a>テンプレート

テンプレート フォルダーには、4 つのテンプレートが含まれています。

- application.hbs: アプリケーションの起動時に表示される既定のテンプレートです。
- about.hbs:「/約」ルートのテンプレートです。
- index.hbs: ルート テンプレート「/」のルートです。
- todoList.hbs: テンプレートを"/todo"のルートです。
- \_navbar.hbs: テンプレートは、ナビゲーション メニューを定義します。

アプリケーション テンプレートは、マスター ページと同様に動作します。 ヘッダー、フッター、および「{{コンセント}}」のルートによってでは、その他のテンプレートを挿入するが含まれています。 Ember のアプリケーション テンプレートの詳細については、次を参照してください。 [ http://guides.emberjs.com/v1.10.0/templates/the-application-template//](http://guides.emberjs.com/v1.10.0/templates/the-application-template/)です。

"/TodoList"テンプレートには、2 つのループ式が含まれています。 外側のループは`{{#each controller}}`、および内部ループは`{{#each todos}}`します。 次のコードは、組み込み`Ember.Checkbox`、表示、カスタマイズされた`App.TodoItemEditView`とのリンク、`deleteTodo`アクション。

[!code-html[Main](emberjs-template/samples/sample12.html)]

`HtmlHelperExtensions` Controllers/HtmlHelperExensions.cs で定義されたクラスを定義するヘルパー関数をキャッシュし、テンプレートの挿入にファイル**デバッグ**に設定されている**true** Web.config ファイルにします。 この関数は Views/Home/App.cshtml で定義されている ASP.NET MVC ビュー ファイルから呼び出されます。

[!code-cshtml[Main](emberjs-template/samples/sample13.cshtml)]

関数は引数なしで呼び出されると、すべてのテンプレート フォルダーにテンプレート ファイルを表示します。 サブフォルダーまたは特定のテンプレート ファイルを指定することもできます。

ときに**デバッグ**は**false** Web.config で、アプリケーションにはバンドルの項目"~/bundles/templates"が含まれています。 BundleConfig.cs、ハンドル コンパイラ ライブラリを使用するのには、バンドルは、この項目が追加されます。

[!code-csharp[Main](emberjs-template/samples/sample14.cs)]
