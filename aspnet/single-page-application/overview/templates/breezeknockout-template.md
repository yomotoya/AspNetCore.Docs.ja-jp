---
uid: single-page-application/overview/templates/breezeknockout-template
title: Breeze/knockout テンプレート |Microsoft Docs
author: madskristensen
description: Breeze/knockout シングル ページ アプリケーション テンプレート
ms.author: riande
ms.date: 01/30/2013
ms.assetid: 3bd94827-3c59-448f-abc3-36e6df4858db
msc.legacyurl: /single-page-application/overview/templates/breezeknockout-template
msc.type: authoredcontent
ms.openlocfilehash: 006d360748674a645ceddb82017f68b0f80f041b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836805"
---
<a name="breezeknockout-template"></a>Breeze/knockout テンプレート
====================
によって[Mads Kristensen](https://github.com/madskristensen)

> Ward Bell によって Breeze/knockout の MVC テンプレートが作成されました
> 
> [Breeze/knockout MVC テンプレートをダウンロードします。](https://go.microsoft.com/fwlink/?LinkId=282649)


「シングル ページ アプリケーション」の聞いた (SPA) とは何か疑問に思います。 読み取ることについて、中には自分で発生するとではなく。 しかし、サンプルをダウンロードする時間を持つでしょうか。 Visual Studio が得られたら、SPA の使用例を必要があり、ASP.NET mvc 4 の「Breeze/knockout シングル ページ アプリケーション」テンプレート秒 60 では未満で実行されています。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

## <a name="what-is-the-breezeknockout-spa-template"></a>Breeze/knockout SPA テンプレートとは何ですか。

ほとんどのプロジェクト テンプレートは、アプリケーションのスケルトンを生成します。 コードを追加することで、ボーンに肉付けを配置し、最終的に、実用的なアプリケーションを提供します。 Breeze/knockout SPA テンプレートは異なります。 調査するためのサンプル アプリケーションを生成します。 SPA アプリケーションの設計および SPA を構築するための手法の多くを示します。

Breeze/knockout テンプレートはバリエーションの 1 つ、 [KnockoutJS SPA テンプレート](../introduction/knockoutjs-template.md)ASP.NET および Web Tools 2012.2 Update に含まれています。 Breeze SPA テンプレートには、同じユーザー エクスペリエンスとアプリケーションが生成されますが、データ管理を簡単を使用して、別の実装です。

KnockoutJS SPA テンプレートでは、単純なアプリケーションには十分生 jQuery AJAX でのサービス要求を行います。 より高度なアプリが要求の厳しいデータ管理の要件。 たとえば、ほとんどのアプリケーション。

- クエリを実行し、拡張ユーザー セッション中に、サーバーを再クエリします。
- クエリのフィルター、並べ替え、およびページングを追加します。
- 複数の画面には、同じデータを共有します。
- 多くのオブジェクトへの変更を蓄積し、それらを 1 つのトランザクションとして保存します。
- データベースに変更をコミットする前に、ユーザーが誤りを修正できますように、クライアントでは、変更を検証します。

BreezeJS ライブラリは、最も重要なアプリケーション ロジックとユーザー エクスペリエンスを開発することができますがのこれらの作業を処理します。

[**Breeze** ](http://www.breezejs.com/?utm_source=ms-spa)は JavaScript と HTML をスタンドアロンのデスクトップ アプリケーションと従来配信されたアプリの種類の豊富なデータ アプリケーションを構築するため、オープン ソース ライブラリです。

Breeze/knockout テンプレートを使用してより堅牢なデータ管理インフラストラクチャに向けた最初その重要な手順を実行するのに役立ちます。 表面上、KnockoutJS SPA テンプレートと同じであるサンプル Todo アプリケーションを生成します。 AJAX のデータ層を簡単に置き換え、近づくサイド バイ サイドでの 2 つを比較できるようにします。 もちろん、簡単なアプリケーションの可能性をほんの少し接触します。 Breeze のしくみがわかりますが、ほとんどがその遷移を必要とします。

では、始めましょう。

## <a name="create-a-breezeknockout-template-project"></a>Breeze/knockout テンプレート プロジェクトを作成します。

ダウンロードし、上記の [ダウンロード] ボタンをクリックして、テンプレートをインストールします。 テンプレートは、Visual Studio Extension (VSIX) ファイルとしてパッケージ化されます。 Visual Studio を再起動する必要があります。

**テンプレート**ペインで、**インストールされたテンプレート**を展開し、 **Visual c#** ノード。 **Visual c#**、 **Web**します。 プロジェクト テンプレートの一覧で選択**ASP.NET MVC 4 Web アプリケーション**します。 プロジェクトの名前し、クリックして**OK**します。

**新しいプロジェクト**ウィザードで、 **Breeze Knockout SPA**します。

![](http://www.breezejs.com/sites/all/images/spa-template/SelectBreezeKOSpaTemplate.png)

ビルド、デバッグを行わずにアプリケーションを実行して ctrl + f5 を押すか、f5 キーを押してデバッグを実行します。

![](http://www.breezejs.com/sites/all/images/spa-template/ZephyrRunning.png)

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

検証ロジックは、簡単で実行されるクライアント側です。 サーバー モデルのクラスに検証属性がクライアントに伝達し、クライアントは、サーバーに接続する前に自動的に実行します。

ネットワーク トラフィックを確認します。 存在しないこと、サーバー呼び出しを簡単にエラーが検出されたときに注意してください。 POST 要求に「/api/Todo/SaveChanges」に有効な各変更が発生しました。 簡単、変更をバンドルし、送信まとめて 1 つの要求として、Web API コント ローラーに`SaveChanges`メソッド。 KockoutJS SPA テンプレートは、PUT、POST、および削除の各項目の要求を個別には、異なるです。

## <a name="peek-inside"></a>内部を見る

このアプリケーションは、クライアント側とサーバー側を持ちます。 クライアント側のスタックは、少しの HTML およびアプリケーションの JavaScript モジュール ("app"フォルダー) 内の組み合わせプラス ("Scripts"フォルダーにあります) でサード パーティ製の JavaScript ライブラリで構成されます。

![](http://www.breezejs.com/sites/all/images/spa-template/ClientArchitecture.png)

KnockoutJS SPA テンプレートを調査する場合これは非常によくなります。 青い四角形に注目します。 UI のアーキテクチャは、モデル-ビュー-ビューモデル (MVVM)、ビューの HTML ウィジェットを明確にでビュー モデルのサポートのプレゼンテーション コードから区切らのです。 データ バインド システム (ここでは Knockout) では、それぞれ他の詳しい知識がない、そのジョブを実行できるように、ビューとビュー モデルを調整します。

モデルには、Todo データがカプセル化します。 モデルのエンティティは、ビューのウィジェットに直接バインドすること、Knockout の観測可能なプロパティを持つ簡単で構築されます。 ビュー モデルでは、データ コンテキストを取得し、モデルのエンティティの保存を求められます。 データ コンテキストは、簡単に作業のほとんどを委任します。

サーバー側スタックは、開発者コードと 3 つの原則の .NET ライブラリで構成されます Web API、Entity Framework、および Breeze.NET:。

![](http://www.breezejs.com/sites/all/images/spa-template/ServerArchitecture.png)

基本的なアーキテクチャでは、KockoutJS SPA テンプレートと同じです。 ただし、実装ははるかに簡単です。、Dto が削除された、およびエンティティ フレームワークの詳細のほとんどを Breeze.NET に委任されています。

## <a name="next-steps"></a>次の手順

によって実行されるコードを検証することをお勧め、[広範なディスカッション](http://www.breezejs.com/spa-template?utm_source=ms-spa)クライアントと Breeze web サイトでサーバー スタックの両方の。

Breeze クライアント側のクエリでの再生を試すことがあります。いくつかのフィルターと並べ替えを追加します。 モデルのプロパティとのエンド ツー エンドの SPA の開発を理解する複数のエンティティを追加する場合があります。 設計の確信できる場合は、Todo の機能を破棄し、独自で置き換えることすることができます。

次の大きなステップの準備完了になります。 クライアント側の画面を追加し、それらの間で移動します。 背後にあるこの SPA テンプレートのままにし、SPA より完全なスタックに有効にするよう[John papa がホット タオル](https://github.com/johnpapa/HotTowel#readme "ホット タオル")、Durandal 簡単と Knockout ミックスに追加します。
