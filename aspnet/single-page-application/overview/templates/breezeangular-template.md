---
uid: single-page-application/overview/templates/breezeangular-template
title: Breeze/angular テンプレート |Microsoft Docs
author: madskristensen
description: Breeze/angular シングル ページ アプリケーション テンプレート
ms.author: riande
ms.date: 03/08/2013
ms.assetid: db31e909-563a-4516-aadd-62aa210ac7e4
msc.legacyurl: /single-page-application/overview/templates/breezeangular-template
msc.type: authoredcontent
ms.openlocfilehash: a3e8b42cdadf99df6971a278834b1429e129ce72
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833616"
---
<a name="breezeangular-template"></a>Breeze/angular テンプレート
====================
によって[Mads Kristensen](https://github.com/madskristensen)

> Ward Bell によって Breeze/angular の MVC テンプレートが作成されました
> 
> [Breeze/Angular の MVC テンプレートをダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=286437)


[AngularJS](http://angularjs.org) Google からオープン ソース ライブラリをシングル ページ アプリケーション (Spa) を構築するためです。 データ バインド、依存関係の挿入、および画面の管理を提供します。 組み合わせる[BreezeJS](http://www.breezejs.com/?utm_source=ms-spa)、データ モデリングとデータ管理、他のオープン ソース ライブラリがある優れた Html/javascript クライアント アプリの基本的な構成要素。

Breeze/angular SPA テンプレートはバリエーションの 1 つ、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)ASP.NET および Web Tools 2012.2 Update に含まれています。 ある場合は Visual Studio が得られたら、SPA の例を稼働して 60 秒以内にします。

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningTodoPage.png)

表面上、アプリケーションは、非常によう KnockoutJS SPA テンプレートになります。 内部的にはまったく異なります。 KnockoutJS テンプレートは、データ バインディングとデータ アクセス用の生の AJAX Knockout を使用します。 Breeze/angular テンプレートは、データ アクセスのデータ バインディングと Breeze Angular を使用します。 これらのライブラリは、ページ ナビゲーションと履歴を含む、追加の機能を有効にします。

アプリケーションのに関するページを次に示します。

![](http://www.breezejs.com/sites/all/images/spa-template/NgRunningAboutPage.png)

このページは、現在のユーザー セッション中にイベントの実行ログを表示を含みます。

- ページングします。 #2 および 7 で Todo コント ローラーの作成に注意してください。
- リモート クエリ (3) とローカル キャッシュのクエリ (7)。
- 新しく保存する (5, 6) と (4) エンティティを変更します。
- データベースに変更をコミットする前に、ユーザーが誤りを修正できるようにを (#9)、クライアントで検証を変更します。

さらに、このテンプレートでの検索含みます。

- HTML ビュー テンプレートの動的読み込み。
- Angular「ディレクティブ」を使用してカスタム データ バインド
- モジュール性と依存関係の注入します。
- クエリ フィルター、並べ替え、ページング、予測、および関連エンティティを含めること。
- 複数の画面でのデータを共有します。
- 単一のトランザクションとしては、複数の変更を保存しています。
- JavaScript クライアントに、検証規則が、サーバーからは自動的に反映します。

では、始めましょう。

## <a name="create-a-breezeangular-template-project"></a>Breeze/Angular プロジェクト テンプレートを作成します。

ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。 テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージ化されます。 Visual Studio を再起動する必要があります。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトの名前し、クリックして**OK**します。

**新しいプロジェクト**ウィザードで、 **Breeze Angular SPA**します。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeNgSpaTemplate.png)

ビルド、デバッグを行わずにアプリケーションを実行して ctrl + f5 を押すか、f5 キーを押してデバッグを実行します。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrLogin.png)

まず、アプリケーションを実行すると、ログイン画面が表示されます。 「サインアップ」リンクをクリックし、新しいページ滑空をビューにユーザー名とパスワードを入力できます。 (ログインと登録ページは構築される ASP.NET MVC を使用します。)登録フォームを送信するときに、サーバーには、アカウントの 2 つの項目を含む、TodoList が生成されます。 それらに表示を黄色に注意してください。

![](http://www.breezejs.com/sites/all/images/spa-template/TodoList.png)

SPA の land するようになりました。 すべてのものが表示され、todo 項目の操作の表示し、Knockout と簡単に利用してクライアントで管理中に発生します。 ユーザーとしてアプリの詳細. 開発者の目にします。 お使いのブラウザーで開発者ツールを使用して、ネットワーク トラフィックをキャプチャします。 (Internet explorer: f12 キーを押して選択、**ネットワーク**タブをクリックし、をクリックして**のキャプチャを開始**)。これで、次の点をお試しください。

- 新しい Todo 項目を追加します。
- ラベルをクリックし、Todo 項目のタイトルの編集
- 項目の完了をマークする チェック ボックスを確認します。 タイトルが編集可能なテキスト ボックスが無効になっていることに注目してください。
- ラベルの右側に 'x' をクリックします。 項目が表示されなくなり、データベースから削除されます。
- 別の項目を選択し、そのタイトルをオフにします。 タイトルが必要である検証エラーが表示されます。 少し間を置いて、以前のタイトルが復元されます。
- 驚くほど長いタイトルを入力します。 タイトルが長すぎるという異なる検証エラーが表示されます。
- Todo List"追加"ボタンをクリックします。 上記の一覧の左側に新しいリストが表示されます。
- TodoList のタイトル、トリガーの要求に応じて、および長さの検証を再生します。
- エラー メッセージを消去するタイトル テキスト ボックスをクリックします。
- TodoList と、todo 項目を削除する右上隅にある円の中で [x] をクリックします。
- これらのアクティビティのログを表示する右上にある"About"リンクをクリックします。

検証ロジックは、簡単で実行されるクライアント側です。 サーバー モデルのクラスに検証属性がクライアントに伝達し、クライアントは、サーバーに接続する前に自動的に実行します。

ネットワーク トラフィックを確認します。 存在しないこと、サーバー呼び出しを簡単にエラーが検出されたときに注意してください。 POST 要求に「/api/Todo/SaveChanges」に有効な各変更が発生しました。 簡単、変更をバンドルし、送信まとめて 1 つの要求として、Web API コント ローラーに`SaveChanges`メソッド。 KockoutJS SPA テンプレートは、PUT、POST、および削除の各項目の要求を個別には、異なるです。

また、およびページについて、TodoList 間を切り替えると、ネットワーク トラフィックはありません。 Breeze のローカル キャッシュにクエリが制約されているためにです。

## <a name="peek-inside"></a>内部を見る

このアプリケーションは、クライアント側とサーバー側を持ちます。 クライアント側のスタックは、少しの HTML およびアプリケーションの JavaScript モジュール ("app"フォルダー) 内の組み合わせプラス ("Scripts"フォルダーにあります) でサード パーティ製の JavaScript ライブラリで構成されます。

![](http://www.breezejs.com/sites/all/images/spa-template/NgClientArchitecture2.png)

UI のアーキテクチャでは、コント ローラーでのサポートのプレゼンテーション コードとビューの HTML ウィジェットを分離します。 Angular のデータ バインド システムは、それぞれ他の詳しい知識がない、そのジョブを実行できるように、ビューとコント ローラーを調整します。

コント ローラーは、取得、およびモデルのエンティティを保存するデータ コンテキストを確認します。 データ コンテキストは、ほとんどの簡単で、JSON クエリの結果から、自己追跡のモデル オブジェクトを構築する作業を委任します。

サーバー側スタックは、開発者コードと 3 つの原則の .NET ライブラリで構成されます Web API、Entity Framework、および Breeze.NET:。

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

基本的なアーキテクチャでは、KockoutJS SPA テンプレートと同じです。 ただし、実装ははるかに簡単です。、Dto が削除された、およびエンティティ フレームワークの詳細のほとんどを Breeze.NET に委任されています。

## <a name="next-steps"></a>次の手順

によって実行されるコードを検証することをお勧め、[広範なディスカッション](http://www.breezejs.com/ng-spa-template?utm_source=ms-spa)クライアントと Breeze web サイトでサーバー スタックの両方の。

Breeze クライアント側のクエリでの再生を試すことがあります。いくつかのフィルターと並べ替えを追加します。 モデルのプロパティとのエンド ツー エンドの SPA の開発を理解する複数のエンティティを追加する場合があります。 設計の確信できる場合は、Todo の機能を破棄し、独自で置き換えることすることができます。

コーディングを楽しんでください。
