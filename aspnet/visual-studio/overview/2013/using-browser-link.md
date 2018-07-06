---
uid: visual-studio/overview/2013/using-browser-link
title: Visual Studio 2013 でブラウザー リンクの使用 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 10/04/2013
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: 9da93c279bfa2af614733e3234ba62429abf688a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822196"
---
<a name="using-browser-link-in-visual-studio-2013"></a>Visual Studio 2013 でブラウザー リンクの使用
====================
作成者[Mike Wasson](https://github.com/MikeWasson)

ブラウザー リンクとは、開発環境と 1 つまたは複数の web ブラウザーの間の通信チャネルを作成する Visual Studio 2013 の新機能です。 クロス ブラウザー テスト用に便利です、いくつかのブラウザーで web のアプリケーションは、一度にを更新するブラウザー リンクを使用できます。

- [ブラウザーの更新](#browser-refresh)
- [ブラウザー リンク ダッシュ ボードを表示します。](#dashboard)
- [静的な HTML ファイルのブラウザー リンクの有効化](#static-html)
- [ブラウザー リンクを無効にします。](#disabling)
- [作業のでしょうか。](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>ブラウザーの更新

ブラウザーを更新するには、ブラウザー リンクを介して Visual Studio に接続されている複数のブラウザーを更新できます。

ブラウザーの更新を使用するには、最初のプロジェクト テンプレートのいずれかを使用して、ASP.NET アプリケーションを作成します。 F5 キーを押すか、ツールバーの矢印アイコンをクリックしてアプリケーションをデバッグします。

![](using-browser-link/_static/image1.png)

デバッグ用の特定のブラウザーを選択するのにドロップダウン リストを使用することもできます。

![](using-browser-link/_static/image2.png)

複数のブラウザーをデバッグするには、選択**ブラウザー**します。 **ブラウザー**ダイアログ ボックスで、キーを押し、CTRL キーを 1 つ以上のブラウザーを選択します。 クリックして**参照**で選択されているブラウザーをデバッグします。 ブラウザー リンクは、Visual Studio の外部からブラウザーを起動し、アプリケーションの URL に移動する場合にも機能します。

![](using-browser-link/_static/image3.png)

ブラウザー リンク コントロールは、円形の矢印アイコンを含むドロップダウンに配置されます。 矢印アイコンが、**更新**ボタンをクリックします。

![](using-browser-link/_static/image4.png)

どのブラウザーが接続されているを表示するには、マウス ポインターを置き、**更新**デバッグ中にボタンをクリックします。 ツールヒント ウィンドウは、接続されているブラウザーに表示されます。

![](using-browser-link/_static/image5.png)

接続されているブラウザーを更新する をクリックして、**更新**ボタンまたは CTRL + ALT + ENTER キーを押します。 たとえば、次のスクリーン ショットでは、MVC 5 プロジェクト テンプレートを使用して作成した ASP.NET プロジェクトを示しています。 上部にある 2 つのブラウザーで実行されているアプリケーションを確認できます。 下部で、プロジェクトは Visual Studio で開いています。

![](using-browser-link/_static/image6.png)

Visual Studio で、変更、 &lt;h1&gt;のホーム ページの見出し。

![](using-browser-link/_static/image7.png)

クリックしたときに、**更新** ボタン、両方のブラウザー ウィンドウに表示されていた変更。

![](using-browser-link/_static/image8.png)

**ノート**

- Browser Link を有効にするには設定`debug=true`で、 [&lt;コンパイル&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx)プロジェクトの Web.config ファイル内の要素。
- アプリケーションは、localhost 上で実行する必要があります。
- アプリケーションでは、.NET 4.0 以降をターゲットする必要があります。

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>ブラウザー リンク ダッシュ ボードを表示します。

ブラウザー リンク ダッシュ ボードには、ブラウザー リンク接続に関する情報が表示されます。 ダッシュ ボードを表示するには、ブラウザー リンクのドロップダウン メニューを選択します。 (横にある小さい矢印、**更新**ボタン)。 クリックして**ブラウザー リンク ダッシュ ボード**します。

![](using-browser-link/_static/image9.png)

ダッシュ ボードには、接続されているブラウザーと各ブラウザーが移動先 URL が一覧表示します。

![](using-browser-link/_static/image10.png)

**の前提条件**でそのプロジェクトに対してブラウザー リンクを有効にするために必要なすべての手順を説明します。 たとえば、次のスクリーン ショット、プロジェクトで Web.config ファイルで false に設定が"debug"。

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>静的な HTML ファイルのブラウザー リンクの有効化

静的な HTML ファイルで Browser Link を有効にするには、Web.config ファイルに、次を追加します。

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

パフォーマンス向上のためには、プロジェクトを発行するときに、この設定を削除します。

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>ブラウザー リンクを無効にします。

ブラウザー リンクは、既定で有効です。 無効にするいくつかの方法はあります。

- ブラウザー リンクのドロップダウン メニューで、オフに**Browser Link を有効にする**します。 

    ![](using-browser-link/_static/image12.png)
- Web.config ファイルでは、"vs: EnableBrowserLink"値"false"に appSettings セクションでという名前のキーを追加します。 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Web.config ファイルでデバッグを false に設定します。 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>作業のでしょうか。

ブラウザー リンクを使用して[SignalR](../../../signalr/index.md) Visual Studio とブラウザー間の通信チャネルを作成します。 Browser Link を有効にすると、Visual Studio は、複数のクライアント (ブラウザー) が接続できる SignalR サーバーとして機能します。 また、ブラウザー リンクは、ASP.NET を使用した HTTP モジュールを登録します。 このモジュールは特殊な挿入&lt;スクリプト&gt;ページ要求ごとに、サーバーからの参照。 ブラウザーでソースの表示 を選択して、スクリプト参照を確認できます。

![](using-browser-link/_static/image13.png)

ソース ファイルは変更されません。 HTTP モジュールは、スクリプト参照を動的に挿入します。

ブラウザー側のコードは、すべての JavaScript であるために、すべてのブラウザーで動作する[SignalR をサポートしています](../../../signalr/overview/getting-started/supported-platforms.md)、任意のブラウザー プラグインを必要とせずします。
