---
uid: visual-studio/overview/2013/using-browser-link
title: "Visual Studio 2013 で Browser Link を使用して |Microsoft ドキュメント"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 10/04/2013
ms.topic: article
ms.assetid: 46cbfe20-b4dc-449b-9016-80657dd44fbe
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/using-browser-link
msc.type: authoredcontent
ms.openlocfilehash: e5a13405a303580ec8c1d4cdacafc26c6f8ff34a
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="using-browser-link-in-visual-studio-2013"></a>Visual Studio 2013 で Browser Link を使用します。
====================
によって[Mike Wasson](https://github.com/MikeWasson)

Browser Link とは、開発環境と 1 つまたは複数の web ブラウザーの間の通信チャネルを作成する Visual Studio 2013 での新機能です。 これはクロス ブラウザー テスト用に便利にいくつかのブラウザーで web のアプリケーションは、一度に更新 Browser Link を使用できます。

- [ブラウザーの更新](#browser-refresh)
- [ブラウザー リンク ダッシュ ボードを表示します。](#dashboard)
- [静的な HTML ファイルの Browser Link を有効にします。](#static-html)
- [Browser Link を無効にします。](#disabling)
- [作業のですか。](#how-it-works)

<a id="browser-refresh"></a>
## <a name="browser-refresh"></a>ブラウザーの更新

ブラウザーを更新するには、ブラウザー リンクを介して Visual Studio に接続されている複数のブラウザーを更新できます。

ブラウザーの更新を使用するには、最初のプロジェクト テンプレートのいずれかを使用して、ASP.NET アプリケーションを作成します。 F5 キーを押すか、ツールバーにある矢印アイコンをクリックして、アプリケーションをデバッグします。

![](using-browser-link/_static/image1.png)

ドロップダウンを使用して、デバッグ用の特定のブラウザーを選択することができますも。

![](using-browser-link/_static/image2.png)

複数のブラウザーをデバッグするには、選択**Browse With**です。 **Browse With**ダイアログ ボックスで、キーを押し、CTRL キーを複数のブラウザーを選択します。 をクリックして**参照**選択されているブラウザーを使用してデバッグします。 Browser Link では、Visual Studio の外部からブラウザーを起動し、アプリケーションの URL に移動する場合でも動作します。

![](using-browser-link/_static/image3.png)

ブラウザー リンク コントロールは、円形の矢印のアイコンで、ドロップダウン リストにあります。 矢印アイコンは、**更新**ボタンをクリックします。

![](using-browser-link/_static/image4.png)

どのブラウザーが接続を表示するには、マウス ポインター、**更新**デバッグ中にボタンをクリックします。 接続されているブラウザーは、ツールヒント ウィンドウに表示されます。

![](using-browser-link/_static/image5.png)

接続されているブラウザーを更新する をクリックして、**更新**クリックするか、CTRL + ALT + ENTER キーを押します。 たとえば、次のスクリーン ショットでは、MVC 5 のプロジェクト テンプレートを使用して作成した ASP.NET プロジェクトが表示されます。 上部にある 2 つのブラウザーで実行されているアプリケーションを表示できます。 下部で、プロジェクトは Visual Studio で開いています。

![](using-browser-link/_static/image6.png)

Visual Studio で、変更、 &lt;h1&gt;のホーム ページの見出し。

![](using-browser-link/_static/image7.png)

クリックすると、**更新** ボタン、両方のブラウザー ウィンドウで、変更されています。

![](using-browser-link/_static/image8.png)

**ノート**

- Browser Link を有効にするには設定`debug=true`で、 [&lt;コンパイル&gt;](https://msdn.microsoft.com/library/s10awwz0(v=vs.85).aspx)プロジェクトの Web.config ファイル内の要素。
- アプリケーションは、ローカル ホストで実行されている必要があります。
- アプリケーションでは、.NET 4.0 以降をターゲットする必要があります。

<a id="dashboard"></a>
## <a name="viewing-the-browser-link-dashboard"></a>ブラウザー リンク ダッシュ ボードを表示します。

ブラウザー リンク ダッシュ ボードには、ブラウザー リンク接続に関する情報が表示されます。 ダッシュ ボードを表示するには、ブラウザー リンク ドロップダウン メニューを選択 (横にある小さい矢印、**更新**ボタン)。 をクリックして**ブラウザー リンク ダッシュ ボード**です。

![](using-browser-link/_static/image9.png)

ダッシュ ボードには、接続されているブラウザーと各ブラウザーが移動先の URL が一覧表示します。

![](using-browser-link/_static/image10.png)

**の前提条件**でそのプロジェクトを Browser Link を有効にするために必要なすべての手順を説明します。 たとえば、次のスクリーン ショット、プロジェクト"debug"が設定されているを false に Web.config ファイルにします。

![](using-browser-link/_static/image11.png)

<a id="static-html"></a>
## <a name="enabling-browser-link-for-static-html-files"></a>静的な HTML ファイルの Browser Link を有効にします。

静的な HTML ファイルで Browser Link を有効にするのには、Web.config ファイルに、次を追加します。

[!code-xml[Main](using-browser-link/samples/sample1.xml)]

パフォーマンス向上のためには、プロジェクトを発行するときに、この設定を削除します。

<a id="disabling"></a>
## <a name="disabling-browser-link"></a>Browser Link を無効にします。

Browser Link は既定で有効にします。 いくつかの方法で無効にするにがあります。

- Browser Link のドロップダウン メニューで、ボックスをオフに**Browser Link を有効にする**です。 

    ![](using-browser-link/_static/image12.png)
- Web.config ファイルでは、"vs: EnableBrowserLink"値"false"の appSettings セクションをという名前のキーを追加します。 

    [!code-xml[Main](using-browser-link/samples/sample2.xml)]
- Web.config ファイルでデバッグを false に設定します。 

    [!code-xml[Main](using-browser-link/samples/sample3.xml)]

<a id="how-it-works"></a>
## <a name="how-does-it-work"></a>作業のですか。

Browser Link を使用して[SignalR](../../../signalr/index.md)を Visual Studio とブラウザー間の通信チャネルを作成します。 Browser Link を有効にすると、Visual Studio は、複数のクライアント (ブラウザー) に接続できる SignalR サーバーとして機能します。 また、ブラウザー リンクは、ASP.NET を使用した HTTP モジュールを登録します。 このモジュールは特殊な挿入&lt;スクリプト&gt;ページ要求ごとに、サーバーからの参照。 ブラウザーでソースの表示 を選択して、スクリプト参照を確認できます。

![](using-browser-link/_static/image13.png)

ソース ファイルは変更されません。 HTTP モジュールは、スクリプト参照を動的に挿入します。

ブラウザー側コードでは、すべての JavaScript は、ために、すべてのブラウザーでを[SignalR をサポートしている](../../../signalr/overview/getting-started/supported-platforms.md)、任意のブラウザー プラグインを必要とせずします。
