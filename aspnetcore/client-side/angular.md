---
title: "単一ページ アプリケーション (SPAs) に AngularJS を使用します。"
author: rick-anderson
description: "AngularJS を使用して SPA スタイルの ASP.NET アプリケーションをビルドする方法をについてください。"
keywords: "ASP.NET Core、AngularJS、SPA"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4b30576b-2718-4c39-9253-a59966747893
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/angular
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 2c7929976f0c9f8284ab397b1a87d576bcdd15b0
ms.sourcegitcommit: 9cdbfd0d670d70b9c354216aabee260c52dad5ee
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/12/2017
---
# <a name="using-angularjs-for-single-page-applications-spas-with-aspnet-core"></a>AngularJS を使用して ASP.NET Core を使用する単一ページ アプリケーション (SPAs)


によって[Venkata Koppaka](https://blog.falafel.com/falafel-software-recognized-sitefinity-website-year/)と[Scott Addie](https://scottaddie.com)

この記事では、AngularJS を使用して SPA スタイルの ASP.NET アプリケーションをビルドする方法を学習します。

[サンプル コードを表示またはダウンロードする](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)

## <a name="what-is-angularjs"></a>AngularJS とは何ですか。

[AngularJS](https://angularjs.org/)シングル ページ アプリケーション (SPAs) 操作に使用される一般的な Google からの最新の JavaScript フレームワークがします。 AngularJS MIT ライセンスの下でソースに、AngularJS の開発の進行状況を後に[、GitHub リポジトリ](https://github.com/angular/angular.js)です。 ライブラリは HTML の形の角かっこを使用しているために、角速度と呼ばれます。

AngularJS は jQuery などの DOM 操作ライブラリではありませんが、jQLite と呼ばれる jQuery のサブセットを使用しています。 AngularJS は、HTML タグを追加できますされる宣言型の HTML 属性に、主に基づいています。 使用して、ブラウザーに AngularJS を試みることができます、[コード学校 web サイト](https://www.codeschool.com/courses/shaping-up-with-angularjs)または[W3Schools web サイト](https://www.w3schools.com/angular/)です。

この記事では、角の見出しで ノートのいくつかの AngularJS について説明します。

## <a name="getting-started"></a>作業の開始

AngularJS を使用してを ASP.NET アプリケーションを開始するには、プロジェクトの一部としてインストールか、コンテンツ配信ネットワーク (CDN) から参照する必要があります。

### <a name="installation"></a>インストール

アプリケーションに AngularJS を追加するいくつかの方法はあります。 Visual Studio で新しい ASP.NET Core web アプリケーションを開始している場合、は、組み込みを使用して AngularJS を追加することができます[Bower](bower.md)をサポートします。 開いている*bower.json*、エントリを追加し、`dependencies`プロパティ。

<a name=angular-bower-json></a>

[!code-json[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/bower.json?highlight=9)]

保存時に、 *bower.json*ファイル、角をプロジェクトのインストールは*wwwroot/lib*フォルダーです。 さらに、それが内にリストアップされます、`Dependencies/Bower`フォルダーです。 次のスクリーン ショットを参照してください。

![AngularJS プロジェクト ソリューション エクスプ ローラー](angular/_static/angular-solution-explorer.png)

次に、追加、`<script>`の一番下への参照、 `<body>` HTML ページのセクションまたは*_Layout.cshtml*次に示すように、ファイルします。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=4&range=48-52)]

実稼働アプリケーションで Cdn を利用して、AngularJS のような一般的なライブラリのことをお勧めします。 AngularJS は、この 1 など、いくつかの Cdn のいずれかから参照できます。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Shared/_Layout.cshtml?highlight=10&range=53-67)]

参照を作成したら、 *angular.js* web ページに AngularJS を使用して作業を開始する準備ができたら、スクリプト ファイル。

## <a name="key-components"></a>主要なコンポーネント

AngularJS などの主要なコンポーネントの数を含む*ディレクティブ*、*テンプレート*、*リピータ*、*モジュール*、 *コント ローラー*、*コンポーネント*、*コンポーネント ルーター*などです。 これらのコンポーネント連携するしくみに、web ページの動作を追加するかを調べてみましょう。

### <a name="directives"></a>ディレクティブ

AngularJS を使用して[ディレクティブ](https://docs.angularjs.org/guide/directive)のカスタム属性と要素の HTML を拡張します。 AngularJS ディレクティブが経由で定義されている`data-ng-*`または`ng-*`プレフィックス (`ng`の短縮形は、角運動)。 AngularJS ディレクティブの 2 つの種類があります。

   1. **プリミティブ ディレクティブ**: Angular チームによって事前に定義し、AngularJS フレームワークの一部であるこれらです。

   2. **カスタム ディレクティブ**: これらは、カスタム ディレクティブを定義できます。

AngularJS のすべてのアプリケーションで使用されるプリミティブ ディレクティブのいずれかが、`ng-app`ディレクティブで、AngularJS アプリケーションをブートス トラップします。 このディレクティブに適用できます、`<body>`タグまたは本文の子要素です。 アクションの例を見てみましょう。 ASP.NET プロジェクトで開いている場合、行うことができますか、追加する HTML ファイルを`wwwroot`フォルダー、または新しいコント ローラーのアクションと関連するビューを追加します。 この場合、追加した新しい`Directives`アクション メソッド`HomeController.cs`です。 関連するビューを次に示します。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Directives.cshtml?highlight=5,7)]

これらのサンプルを互いに独立するおくと、使用せずに、共有のレイアウト ファイルです。 Body タグを装飾おを参照してください、`ng-app`このページを示すディレクティブは、AngularJS アプリケーションです。 `{{2+2}}`よりについて学びますしばらくを角運動のデータ バインディング式を指定します。 このアプリケーションを実行する場合、結果を次に示します。

![単純な Angular ディレクティブ](angular/_static/simple-directive.png)

その他のプリミティブ AngularJS ディレクティブは、次のとおりです。

`ng-controller`JavaScript コント ローラーはどのビューにバインドを決定します。

`ng-model`HTML 要素のプロパティの値がバインドされているモデルを決定します。

`ng-init`現在のスコープを表す式の形式でアプリケーション データを初期化するために使用します。

`ng-if`削除するか、指定された HTML 要素に指定された式の truthiness に基づいて DOM 内で再作成します。

`ng-repeat`データのセットで指定された HTML ブロックを繰り返します。

`ng-show`指定した式に基づいて、指定された HTML 要素の表示と非表示を切り替えます。

AngularJS でサポートされるすべてのプリミティブ ディレクティブの完全な一覧を参照してください、 [AngularJS ドキュメント web サイトでのディレクティブのドキュメント セクション](https://docs.angularjs.org/api/ng/directive)です。

### <a name="data-binding"></a>データ バインディング

AngularJS 提供[データ バインディング](https://docs.angularjs.org/guide/databinding)サポートの-すぐいずれかを使用して、`ng-bind`ディレクティブまたは式の構文をなど、バインド データ`{{expression}}`です。 AngularJS には、常にテンプレートの表示との同期のモデルからのデータが保存されている双方向データ バインディングがサポートしています。 ビューへの変更は、モデルに自動的に反映されます。 同様に、モデル内のすべての変更は、ビューに反映されます。

という名前の付随するビューを HTML ファイルまたはコント ローラーのアクションのいずれかを作成する`Databinding`です。 ビューには、次を含めます。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Databinding.cshtml?highlight=8,9,10)]

ディレクティブまたはデータのバインドを使用してモデルの値を表示することに注意してください (`ng-bind`)。 結果のページは、次のようになります。

![単純データ バインディング](angular/_static/simple-databinding.png)

### <a name="templates"></a>テンプレート

[テンプレート](https://docs.angularjs.org/guide/templates)AngularJS で単の HTML ページで装飾 AngularJS ディレクティブと成果物です。 AngularJS のテンプレートは、ディレクティブ、式、フィルター、およびと HTML フォームのビューを結合するコントロールの組み合わせです。

テンプレートを示すために別のビューを追加し、次のコードを追加します。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Templates.cshtml?highlight=8,9,10)]

テンプレートに 1 つのような AngularJS ディレクティブ`ng-app`、 `ng-init`、`ng-model`とバインドするデータ バインド式の構文、`personName`プロパティです。 ビューは、ブラウザーで実行されている、ため、次のスクリーン ショットのようになります。

![テンプレートの簡単な例 1](angular/_static/simple-templates-1.png)

入力フィールドに入力して名前を変更する場合、入力フィールドの横にあるテキストを動的に表示されますアクションで角運動の双方向データ バインディングの表示、更新します。

![単純なテンプレート例 2](angular/_static/simple-templates-2.png)

### <a name="expressions"></a>式

[式](https://docs.angularjs.org/guide/expression)AngularJS 内に記述された JavaScript に似たコード スニペットは、`{{ expression }}`構文です。 これらの式からのデータと同じ方法を HTML にバインドされた`ng-bind`ディレクティブです。 AngularJS 式および JavaScript の正規表現の主な違いがその AngularJS に対して式が評価される、 `$scope` AngularJS 内のオブジェクト。

AngularJS 式バインドの下のサンプルで`personName`と、JavaScript の単純な式を計算します。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Expressions.cshtml?highlight=8,9,10)]

ブラウザーの表示での実行など、`personName`データと計算の結果。

![単純な式](angular/_static/simple-expressions.png)

### <a name="repeaters"></a>リピータ

呼ばれるプリミティブ ディレクティブを使用して行うは AngularJS の繰り返し`ng-repeat`です。 `ng-repeat`ディレクティブは、繰り返しのデータ配列の長さで、ビューで指定された HTML 要素が繰り返されます。 リピータ AngularJS では、文字列またはオブジェクトの配列を繰り返すことができます。 文字列の配列を繰り返しの使用例を次に示します。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters.cshtml?highlight=8,10,11)]

[Repeat ディレクティブ](https://docs.angularjs.org/api/ng/directive/ngRepeat)このスクリーン ショットに示すように、開発者ツールでわかるように、順序なしのリストのリスト項目の系列を出力します。

![リピータの例](angular/_static/repeater.png)

オブジェクトの配列でが繰り返さ 例を次に示します。 `ng-init`ディレクティブの確立、`names`の各要素が最初に格納するオブジェクトし、姓と名の配列。 `ng-repeat`割り当て、`name in names`配列のすべての要素の一覧項目を出力します。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters2.cshtml?highlight=8,9,10,11,13,14)]

出力では、前の例のように同じここでです。

角度は、その実行に、ループがある場合に基づく動作のために役立ついくつかの追加ディレクティブを提供します。

`$index`

使用`$index`で、`ng-repeat`では、ループを使用している、インデックス、ループの現在位置を決定します。

`$even` および `$odd`

使用して`$even`で、`ng-repeat`ループを使用して、ループ内の現在のインデックスがあってもインデックスの行であるかどうかを確認します。 同様に、使用`$odd`現在のインデックスが奇数のインデックス付き行かどうかを決定します。

`$first` および `$last`

使用して`$first`で、`ng-repeat`ループを使用して、ループ内の現在のインデックスが最初の行であるかどうかを確認します。 同様に、使用`$last`現在のインデックスが最後の行を判断します。

示すサンプルを次に示します`$index`、 `$even`、 `$odd`、 `$first`、および`$last`の動作。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Repeaters3.cshtml?highlight=14,15,16,17,18)]

結果の出力を次に示します。

![リピータ例 2](angular/_static/repeaters2.png)

### <a name="scope"></a>$scope

`$scope`JavaScript オブジェクト (テンプレート) ビューと (下記参照)、コント ローラーの間でグルーとして機能します。 AngularJS のビュー テンプレートのみが知ってに割り当てられた値、`$scope`コント ローラー内のオブジェクト。

> [!NOTE]
> MVVM 世界では、 `$scope` AngularJS 内のオブジェクトは多くの場合、ViewModel として定義されています。 AngularJS チームを指す、`$scope`データ モデルとしてオブジェクト。 [AngularJS のスコープの詳細について](https://docs.angularjs.org/guide/scope)です。

プロパティを設定する方法を示す簡単な例を次に示します`$scope`別の JavaScript ファイル内で*scope.js*:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/scope.js?highlight=2,3)]

確認、`$scope`パラメーターが 2 行目のコント ローラーに渡されます。 このオブジェクトは、ビューの知ってます。 "Mary Jane"に"name"と呼ばれるプロパティを設定すると 3 行目におはします。

新機能は、特定のプロパティはビューによって検出されなかった場合にどうなりますか。 定義を次に、ビューは、"name"および"age"プロパティを参照します。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Scope.cshtml?highlight=9,10,14)]

式の構文を使用して、"name"プロパティを表示する角度をいただきます 9 行目に注意してください。 行 10 は、"age"、存在しないプロパティを参照します。 実行中の例では、年齢の"Mary Jane"および nothing に設定した名前を示します。 不足しているプロパティは無視されます。

![スコープの例](angular/_static/scope.png)

### <a name="modules"></a>モジュール

A[モジュール](https://docs.angularjs.org/guide/module)で AngularJS のコント ローラー、サービス、ディレクティブなどのコレクション。`angular.module()`を作成し、登録、および、AngularJS モジュールを取得する関数呼び出しを使用します。 使用して、AngularJS チーム、およびサードパーティのライブラリ、出荷を含め、すべてのモジュールを登録する必要があります、`angular.module()`関数。

以下の AngularJS で新しいモジュールを作成する方法を示すコード スニペットに示します。 最初のパラメーターは、モジュールの名前です。 2 番目のパラメーターは、他のモジュールの依存関係を定義します。 この記事で後述おが表示されるにこれらの依存関係を渡す方法、`angular.module()`メソッドの呼び出しです。

```javascript
var personApp = angular.module('personApp', []);
```

使用して、 `ng-app`  ページで、AngularJS モジュールを表すためのディレクティブ。 モジュールを使用して、モジュールの名前を割り当て、`personApp`この例を`ng-app`テンプレートのディレクティブ。

```html
<body ng-app="personApp">
```

### <a name="controllers"></a>コント ローラー

[コント ローラー](https://docs.angularjs.org/guide/controller) AngularJS では、コードのエントリの最初のポイント。 `<module name>.controller()`関数呼び出しの使用を作成し、AngularJS コント ローラーに登録します。 `ng-controller`ディレクティブを使用する HTML ページで、AngularJS コント ローラーを表します。 角のコント ローラーの役割は、データ モデルの状態と動作を設定する (`$scope`)。 コント ローラーは、DOM を直接操作するのには使用できません。

新しいコント ローラーを登録するコードの例を次に示します。 `personApp` Angular モジュールで、2 行目の定義を参照して、スニペット内の変数です。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/controllers.js?highlight=2,5)]

使用して、ビュー、`ng-controller`ディレクティブには、コント ローラー名が割り当てられます。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Controllers.cshtml?highlight=8,14)]

"Mary"と"Jane"に対応するページを示しています、`firstName`と`lastName`にアタッチされているプロパティ、`$scope`オブジェクト。

![コント ローラーの例](angular/_static/controllers.png)

### <a name="components"></a>コンポーネント

[コンポーネント](https://docs.angularjs.org/guide/component)角で 1.5.x をカプセル化し個々 の HTML 要素を作成する機能を許可します。 Angular 1.4.x で .directive() メソッドを使用して、同じ機能を得ることができます。

.Component() メソッドを使用すると、開発が簡素化されますディレクティブとコント ローラーの機能を取得します。 その他の利点があります。スコープの分離のベスト プラクティスは、本質的なと角運動 2 への移行をより簡単な作業になります。 `<module name>.component()`関数呼び出しの使用を作成し、AngularJS にコンポーネントを登録します。

新しいコンポーネントを登録するコードの例を次に示します。 `personApp` Angular モジュールで、2 行目の定義を参照して、スニペット内の変数です。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/components.js?highlight=2,5,13)]

ビューでは、カスタムの HTML 要素を示されています。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/Home/Components.cshtml?highlight=8)]

コンポーネントによって使用される関連付けられているテンプレート:

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personcomponent.html?highlight=2,3)]

"Aftab"と"Ansari"に対応するページを示しています、`firstName`と`lastName`にアタッチされているプロパティ、`vm`オブジェクト。

![コンポーネントの例](angular/_static/components.png)

### <a name="services"></a>サービス

[サービス](https://docs.angularjs.org/guide/services)AngularJS で一般的に使用され角運動のアプリケーションの有効期間全体で使用できるファイルに抽象化は、共有コード。 サービスは遅延インスタンス化、つまりことがありますするには、サービスのインスタンスは、サービスに依存するコンポーネントを使用します。 ファクトリには、AngularJS アプリケーションで使用するサービスの例があります。 使用してファクトリを作成、`myApp.factory()`関数呼び出し、ここで`myApp`モジュールは、します。

以下には、AngularJS ファクトリを使用する方法を示す例を示します。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/simpleFactory.js?highlight=1)]

コント ローラーからこのファクトリを呼び出す、渡す`personFactory`へのパラメーターとして、`controller`関数。

```javascript
personApp.controller('personController', function($scope,personFactory) {
  $scope.name = personFactory.getName();
});
```

### <a name="using-services-to-talk-to-a-rest-endpoint"></a>サービスを使用して REST エンドポイントと通信

以下は、エンド ツー エンドの例サービスを使用して AngularJS ASP.NET Core Web API エンドポイントとの対話にします。 この例では、Web API からデータを取得し、ビューのテンプレートで、データが表示されます。 まず、ビューを開始しましょう。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Index.cshtml?highlight=5,8,10,17,18,19)]

このビューであると呼ばれる、Angular モジュール`PersonsApp`、コント ローラーと呼ばれます`personController`です。 使用している`ng-repeat`担当者のリストを反復処理します。 私たちは、17 ~ 19 の行に 3 つのカスタム JavaScript ファイルを参照しています。

*PersonApp.js*ファイルを登録するため、`PersonsApp`モジュールであると、構文は、前の例に似ています。 使用して、`angular.module`おりますが、モジュールの新しいインスタンスを作成する関数。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personApp.js?highlight=3)]

見てみましょう*personFactory.js*、後述します。 モジュールを呼び出している`factory`ファクトリを作成するメソッド。 12 行目は、組み込みの角を示しています。`$http`サービス、web サービスからユーザー情報を取得します。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personFactory.js?highlight=6,7,12)]

*PersonController.js*、呼び出しているモジュールの`controller`コント ローラーを作成します。 `$scope`オブジェクトの`people`プロパティには、personFactory (13 の行) から返されたデータが割り当てられます。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personController.js?highlight=6,7,13)]

Web API とその背後にあるモデルを簡単に見てをみましょう。 `Person`モデルは POCO (Plain Old CLR Object) `Id`、 `FirstName`、および`LastName`プロパティ。

[!code-csharp[Main](angular/sample/AngularJSSample/src/AngularJSSample/Models/Person.cs)]

`Person`コント ローラーは、JSON 形式の一覧を返します`Person`オブジェクト。

[!code-csharp[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Controllers/Api/PersonController.cs?highlight=9,10,19)]

アプリケーションの動作を見てみましょう。

![残りの部分を表示するコント ローラーを結果します。](angular/_static/rest-bound.png)

実行できます[GitHub で、アプリケーションの構造を表示](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)です。

> [!NOTE]
> 詳細 AngularJS アプリケーションを構成するのには、次を参照してください[John Papa Angular スタイル ガイド。](https://github.com/johnpapa/angular-styleguide)

&nbsp;

> [!NOTE]
> AngularJS モジュール、コント ローラー ファクトリを作成するディレクティブとビューのファイルを簡単にする Sayed Hashimi を確認してください[for Visual Studio SideWaffle テンプレート パック](http://sidewaffle.com/)です。 Sayed Hashimi は、Microsoft の Visual Studio Web チームのシニア プログラム マネージャー、SideWaffle テンプレートには、最高水準と見なされます。 この記事の執筆時に、SideWaffle は 2013 および 2015、Visual Studio 2012 を使用できます。

### <a name="routing-and-multiple-views"></a>ルーティングと複数のビュー

AngularJS には、SPA (Single Page Application) ベースのナビゲーションを処理する組み込みのルートのプロバイダーが存在します。 AngularJS ルーティングを使用して作業を追加する必要があります、`angular-route`ライブラリ Bower を使用します。 表示、 [bower.json](#angular-bower-json)おは既に参照していること、プロジェクトにこの記事の開始時に参照されているファイル。

パッケージをインストールした後のスクリプト参照の追加 (*角度 route.js*) の表示にします。

今すぐを構築しておナビゲーションを追加して、ユーザー アプリを見てみましょう。 アプリのコピーを作成新規作成することで最初に、`PeopleController`呼び出すアクション`Spa`と対応する`Spa.cshtml`Index.cshtml のビューをコピーして、ビュー、`People`フォルダーです。 スクリプト参照を追加`angular-route`(11 行目を参照してください)。 追加も、`div`でマークされた、`ng-view`ディレクティブ (6 行目を参照してください) でビューを配置するプレース ホルダーとして。 ここでは、いくつかの追加を*.js* 13 ~ 16 の行に参照されるファイルです。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Spa.cshtml?highlight=6,11,12,13,14,15,16)]

見てみましょう*personModule.js*ファイルをどのようにルーティング モジュールをインスタンス化おを参照します。 渡している`ngRoute`モジュールにライブラリとして。 このモジュールは、アプリケーションでのルーティングを処理します。

[!code-javascript[Main](angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personModule.js)]

*PersonRoutes.js*ファイルを次に、ルート プロバイダーに基づいてルートを定義します。 行 4 ~ 7 効果的を言うことにより、ときに使用したの URL ナビゲーションを定義する`/persons`がという名前のテンプレートを使用して、要求された`partials/personlist`を通じて`personListController`です。 8 11 行目のルート パラメーターを持つ詳細ページを示す`personId`です。 URL は、いずれのパターンと一致しません場合、角速度既定値は、`/persons`ビュー。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personRoutes.js?highlight=4,5,6,7,8,9,10,11,13)]

`personlist.html`ファイルがユーザーの一覧を表示するために必要な HTML のみを含む部分ビュー。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/partials/personlist.html?highlight=3)]

コント ローラーが、モジュールを使用して定義されている`controller`関数*personListController.js*です。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/personListController.js?highlight=1)]

このアプリケーションを実行するに移動し、 `people/spa#/persons` URL が表示されます。

![担当者のリスト ビュー](angular/_static/spa-persons.png)

たとえばに詳細ページに移動したかどうかは`people/spa#/persons/2`、部分の詳細ビューが表示されます。

![ユーザーの詳細ビュー](angular/_static/spa-persons-2.png)

完全なソースおよびにこの記事で表示されないすべてのファイルを表示できます[GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/client-side/angular/sample)です。

### <a name="event-handlers"></a>イベント ハンドラー

各入力要素の HTML DOM にイベント処理機能を追加する AngularJS ディレクティブの数があります。 以下の AngularJS に組み込まれているイベントの一覧を示します。

   * `ng-click`

   * `ng-dbl-click`

   * `ng-mousedown`

   * `ng-mouseup`

   * `ng-mouseenter`

   * `ng-mouseleave`

   * `ng-mousemove`

   * `ng-keydown`

   * `ng-keyup`

   * `ng-keypress`

   * `ng-change`

> [!NOTE]
> 使用して、独自のイベント ハンドラーを追加することができます、[カスタム ディレクティブの AngularJS 機能](https://docs.angularjs.org/guide/directive)します。

方法を見て、`ng-click`をイベントがワイヤード (有線)。 という名前の新しい JavaScript ファイルを作成する*eventHandlerController.js*、し、次のコードを追加します。

[!code-javascript[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/wwwroot/app/eventHandlerController.js?highlight=5,6,7)]

新しい`sayName`関数`eventHandlerController`上記の 5 を行にします。 すべてのメソッドの実行のようになりましたが表示されている JavaScript 通知、ようこそメッセージを持つユーザーにします。

以下のビューは、AngularJS イベントにコント ローラー関数をバインドします。 9 行目にボタンがある、 `ng-click` Angular ディレクティブが適用されています。 呼び出す、`sayName`にアタッチされている関数、`$scope`このビューに渡されるオブジェクト。

[!code-html[Main](../client-side/angular/sample/AngularJSSample/src/AngularJSSample/Views/People/Events.cshtml?highlight=9)]

実行中の例では、ことを示します、コント ローラーの`sayName`関数は、ボタンがクリックされたときに自動的に呼び出されます。

![Click イベント](angular/_static/events.png)

AngularJS 組み込みのイベント ハンドラーのディレクティブの詳細については、必ずヘッドを移動する、[ドキュメント web サイト](https://docs.angularjs.org/api/ng/directive/ngClick)AngularJS のです。

## <a name="additional-resources"></a>その他の技術情報

* [Angular Docs](https://docs.angularjs.org)

* [Angular 2 情報](https://angular.io/)
