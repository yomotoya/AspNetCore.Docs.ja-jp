---
uid: single-page-application/overview/templates/breezeangular-template
title: "簡単/角テンプレート |Microsoft ドキュメント"
author: madskristensen
description: "簡単/角の単一ページ アプリケーション テンプレート"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/08/2013
ms.topic: article
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: faf28a510a83b7fa07585904344176601c2e1f34
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="breezeangular-template"></a>簡単/角テンプレート
====================
によって[Mads Kristensen](https://github.com/madskristensen)

> ワード ベルによって簡単/角の MVC テンプレートが書き込まれました
> 
> [簡単/角運動の MVC テンプレートをダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org)単一ページ アプリケーション (SPAs) の構築には、Google からオープン ソース ライブラリです。 データ バインディング、依存関係の挿入、および画面の管理を提供します。 組み合わせて[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)、データ モデリングとデータ管理用の別のオープン ソース ライブラリがある、優れた HTML または JavaScript クライアント アプリケーションの基本的な構成要素。

簡単/角 SPA テンプレートは、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)ASP.NET および Web ツール 2012.2 更新プログラムに含まれます。 Visual Studio を取得したらがあります例 SPA 立ち上がっており実行中 60 秒未満にします。

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

外見上、アプリケーション、よく似て KnockoutJS SPA テンプレート。 ですが、内部で大きく異なります。 KnockoutJS テンプレートは、データ バインディング、およびデータ アクセス用の生の AJAX の Knockout を使用します。 簡単/角テンプレートは、データ アクセス用のデータのバインドともとても簡単角を使用します。 これらのライブラリには、ページ ナビゲーションと履歴を含む、追加の機能が有効にします。

アプリケーションのに関するページを次に示します。

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

このページでは、現在のユーザー セッション中にイベントの実行ログを表示を含みます。

- ページングします。 #2 と 7 に、Todo コント ローラーの作成に注意してください。
- リモート クエリ (3) とローカル キャッシュ クエリ (7)。
- 新規の保存 (5, 6)、(4) エンティティを変更します。
- 上で検証クライアント (9)、ユーザーが変更をコミットする前に誤りを修正できるように、データベースに変更します。

さらに、このテンプレートで探索を含みます。

- HTML ビュー テンプレートの動的な読み込みします。
- 角「ディレクティブ」を使用してカスタムのデータ バインド
- モジュール性と依存関係の挿入します。
- クエリ フィルター、並べ替え、ページング、射影、および関連エンティティを追加します。
- 複数の画面でデータを共有します。
- 単一のトランザクションとして複数の変更を保存しています。
- JavaScript クライアントに、検証規則が、サーバーからは自動的に反映します。

では、始めましょう。

## <a name="create-a-breezeangular-template-project"></a>簡単/Angular テンプレート プロジェクトを作成します。

ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。 テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージされています。 Visual Studio を再起動する必要があります。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#**ノード。 **Visual c#** **Web**です。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web Application**です。 プロジェクトの名前を指定し、をクリックして**OK**です。

**新しいプロジェクト**ウィザードで、**簡単 Angular SPA**です。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

Ctrl キーを押し、f5 キーを押してビルドおよびデバッグせずにアプリケーションを実行するか、f5 キーを押してデバッグを実行します。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

アプリケーションを最初に実行すると、ログイン画面が表示されます。 「登録」リンクをクリックし、新しいページ滑空をビューには、ユーザー名とパスワードを入力できます。 (ログインと登録ページは、ビルド ASP.NET MVC を使用して)。登録フォームを送信するときに、サーバーは、アカウントの 2 つの項目を含む TodoList を生成します。 それらに表示する黄色に注意してください。

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

土地の SPA するようになりました。 すべてのものが表示され、todo などの操作は表示され、Knockout と簡単を利用して、クライアントで管理中に発生します。 ユーザーとして、アプリを調査してください. 開発者の目にします。 お使いのブラウザーで developer tools を使用して、ネットワーク トラフィックをキャプチャします。 (Internet Explorer で: キーを押して F12、select、**ネットワーク**タブをクリックし、をクリックして**のキャプチャを開始**)。これで、次を再試行してください。

- 新しい Todo 項目を追加します。
- ラベルをクリックし、Todo 項目のタイトルの編集
- 項目の終了をマークする チェック ボックスを確認します。 タイトルが編集可能なテキスト ボックスが無効になっていることを確認します。
- ラベルの右側にある 'x' をクリックします。 アイテムは表示されなくなりますされ、データベースから削除されます。
- 別の項目を選択し、そのタイトルをオフにします。 タイトルが必要である検証エラーが表示されます。 少し間を置いて、以前のタイトルが復元されます。
- 長い途方もなく、タイトルを入力します。 タイトルが長すぎるという別の検証エラーが表示されます。
- 「Todo リストの追加」ボタンをクリックします。 上記の一覧の左側に、新しいリストが表示されます。
- TodoList タイトル、その必要なトリガーと長さ検証を再生します。
- エラー メッセージを消去するタイトル テキスト ボックス内をクリックします。
- TodoList およびその todo などを削除する右上隅の円の"x"をクリックします。
- これらのアクティビティ ログを表示する右上に、"About"リンクをクリックします。

検証ロジックは、とても簡単で実行されるクライアント側です。 サーバー モデルのクラスに検証属性は、クライアントに伝達し、クライアントがサーバーに接続する前に自動的に実行します。

ネットワーク トラフィックを確認します。 存在しないサーバーへの呼び出しもとても簡単にエラーが検出されたときに注意してください。 「/Api/Todo/SaveChanges」には、POST 要求に有効な変更が発生しました。 とても簡単です、変更内容をバンドルし、送信一緒に 1 つの要求として、Web API コント ローラーに`SaveChanges`メソッドです。 KockoutJS SPA テンプレートは、PUT、POST、および DELETE の各項目に対して要求を個別には、異なっています。

また、TodoList 間およびページの概要を切り替えたとき、ネットワーク トラフィックがないことを確認します。 クエリがとても簡単のローカル キャッシュに制約されているためにです。

## <a name="peek-inside"></a>内部のピークします。

このアプリケーションは、クライアント側およびサーバー側を持ちます。 クライアント側のスタックは、少しの HTML およびアプリケーションの JavaScript モジュール (フォルダー内の"app") の組み合わせ ("Scripts"フォルダー) にサード パーティ製の JavaScript ライブラリとで構成されます。

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

UI のアーキテクチャは、コント ローラーのサポートのプレゼンテーション コードとビューの HTML ウィジェットを分離します。 角度のデータ バインド システムでは、各熟知し、その他のジョブの実行できるようにビューとコント ローラーを調整します。

コント ローラーでは、データ コンテキストを取得し、モデルのエンティティの保存を求められます。 データ コンテキストは、ほとんどの JSON クエリ結果から自律追跡のモデル オブジェクトの構築もとても簡単に作業を委任します。

一部の開発者のコードと 3 つの原則 .NET ライブラリのサーバー側のスタックで構成されます Web API、Entity Framework および Breeze.NET:。

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

基本的なアーキテクチャでは、KockoutJS SPA テンプレートと同じです。 ただし、実装はずっと簡単:「dtos の使用が削除され、エンティティ フレームワークの詳細のほとんどが Breeze.NET を委任されています。

## <a name="next-steps"></a>次の手順

によって実行されるコードを検証することをお勧め、[広範なディスカッション](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)クライアントとサーバー スタック、とても簡単 web サイトの両方をします。

クライアント側のクエリをとても簡単です; での再生しようとする可能性があります。一部のフィルターや並べ替えを追加します。 複数のモデルのプロパティと、エンド ツー エンド SPA 開発の理解を取得する複数のエンティティを追加する場合があります。 設計の確信できる場合は、Todo 機能を破棄し、自分と置き換えています。

コーディングを楽しんでください。
