---
uid: web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
title: "マスター ページ (c#) を使用して、サイト全体のレイアウトを作成 |Microsoft ドキュメント"
author: rick-anderson
description: "このチュートリアルでは、マスター ページの基本を説明します。 つまり、マスター ページとはどのようにいずれか 1 つの cr をどのようにコンテンツ プレース ホルダーとは、マスター ページを作成しています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/21/2008
ms.topic: article
ms.assetid: 78f8d194-03b9-44a5-8255-90e7cd1c2ee1
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/master-pages/creating-a-site-wide-layout-using-master-pages-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e3507acda4958fa7caa4a22fec7603deaec73c2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="creating-a-site-wide-layout-using-master-pages-c"></a>マスター ページ (c#) を使用して、サイト全体のレイアウトを作成します。
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[コードをダウンロードする](http://download.microsoft.com/download/e/e/f/eef369f5-743a-4a52-908f-b6532c4ce0a4/ASPNET_MasterPages_Tutorial_01_CS.zip)または[PDF のダウンロード](http://download.microsoft.com/download/8/f/6/8f6349e4-6554-405a-bcd7-9b094ba5089a/ASPNET_MasterPages_Tutorial_01_CS.pdf)

> このチュートリアルでは、マスター ページの基本を説明します。 つまり、マスター ページはいずれかの作成方法、マスター ページ コンテンツ プレース ホルダーとは何かが 1 つ作成方法変更する方法、マスター ページが自動的に反映されます、関連するコンテンツ ページで、マスター ページを使用する ASP.NET ページです。


## <a name="introduction"></a>はじめに

適切に設計された web サイトの 1 つの属性は、一貫性のあるサイト全体のページ レイアウトです。 たとえば、www.asp.net の web サイトを考えてみましょう。 この記事の執筆時は、すべてのページは、上部とページの下部に同じコンテンツがします。 図 1 に示す各ページの最上位には、Microsoft のコミュニティの一覧が灰色のバーが表示されます。 つまり、サイトのロゴ、言語に翻訳したサイト、およびコア セクションでは、の一覧の下にある: ホーム、概要、詳細を表示、ダウンロード、およびなどです。 同様に、ページの下部には、www.asp.net、著作権、およびプライバシーに関する声明へのリンクで、広告に関する情報が含まれます。


[![Www.asp.net の web サイトは、すべてのページにわたって一貫したルック アンド フィールを採用します。](creating-a-site-wide-layout-using-master-pages-cs/_static/image2.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image1.png)

**図 01**: www.asp.net の web サイトを使用して、一貫した参照と思われる間でのすべてのページ ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image3.png))


適切に設計されたサイトの別の属性は、簡単に使用するサイトの外観を変更できます。 図 1 は 2008 年 3 月、時点で www.asp.net のホーム ページを示していますが、ルック アンド フィールをここで、このチュートリアルの文書間変更可能性があります。 おそらく、上部にメニュー項目には、MVC フレームワークの新しいセクションが含まれますが展開されます。 またはおそらくさまざまな色、フォント、およびレイアウトを大幅に新たに設計されません出現します。 サイト全体にこのような変更を適用すると、サイトを構成する web ページの数千の変更が不要な高速で単純な処理をする必要があります。

使用すると考えられる ASP.NET でサイト全体のページ テンプレートの作成は*マスター ページ*です。 簡単に言うと、マスター ページでは、特殊な種類のすべての間で共通するマークアップを定義する ASP.NET ページ*コンテンツ ページ*はコンテンツのページ コンテンツ ページごとにカスタマイズ可能な領域とします。 (コンテンツ ページとは、マスター ページにバインドされている ASP.NET ページのことです)。マスター ページのレイアウトや書式設定が変更されたときにそのコンテンツ ページの出力のすべてが同様に直ちに更新される、されるため、更新し、1 つのファイル (つまり、マスター ページ) の配置と同じくらい簡単サイト全体の外観の変更を適用します。

これは、一連のマスター ページの使用方法について説明するチュートリアルの最初のチュートリアルです。 このチュートリアル シリーズの過程では。

- マスター ページと、対応するコンテンツ ページの作成を調べる
- さまざまなヒント、テクニック、およびトラップの説明します。
- マスター ページよくある落とし穴を特定し、回避策を調べる
- マスター ページ コンテンツ ページと逆からへのアクセス方法を参照してください。
- 実行時に、コンテンツ ページのマスター ページを指定する方法について説明し、
- マスター ページのトピックは他の詳細。

これらのチュートリアルは、簡潔なは、スクリーン ショットが多いプロセスを視覚的に順番の詳細な手順についての提供に特化しています。 各チュートリアルは、c# および Visual Basic バージョンで利用可能な使用される完全なコードのダウンロードが含まれています。

このスタートとなったチュートリアルは、マスター ページの基本を確認を開始します。 マスター ページと Visual Web Developer を使用して、関連するコンテンツ ページの作成について見て、およびマスター ページへの変更がそのコンテンツ ページですぐに反映される方法を参照してください。 おマスター ページの動作について説明します。 開始しましょう!

## <a name="understanding-how-master-pages-work"></a>マスター ページの動作を理解します。

一貫性のあるサイト全体のページ レイアウトと web サイトを構築するには、各 web ページがそのカスタム コンテンツに加えて共通の書式設定マークアップを生成することが必要です。 たとえば、www.asp.net の各チュートリアルやフォーラムの投稿では、独自の一意のコンテンツがありますが、これらのページの各もレンダリング一連の共通`<div>`の最上位のセクションのリンクを表示する要素: Home、開始、学習、およびなどです。

さまざまな一貫したルック アンド フィールで web ページを作成するための方法があります。 単純な手法は、単にコピーして、すべての web ページに共通のレイアウトのマークアップを貼り付けますが、このアプローチにはマイナス面の数。 まず、新しいページが作成されるたびに、コピーして共有のコンテンツをページに貼り付けますことを忘れないでください。 このようなコピーと貼り付け操作はエラー、誤っての新しいページに共有マークアップのサブセットのみをコピーする場合があります。 入れたら、このアプローチにより、既存のサイト全体の外観を実際のペインの新しいルック アンド フィールを使用するために、サイト内の各単一ページを編集する必要があるため、新しいものに置き換えます。

ASP.NET version 2.0 では前のページで一般的なマークアップを配置することがよく開発者[ユーザー コントロール](https://msdn.microsoft.com/en-us/library/y6wb1a0e.aspx)され、各ページにこれらのユーザー コントロールを追加します。 この方法では、ページの開発者が手動ですべての新しいページにユーザー コントロールを追加するが、変更する一般的なマークアップを更新するときにユーザー コントロールだけが必要なために、サイト全体の変更を簡単に許可が必要です。 残念ながら、Visual Studio .NET 2002 および 2003 のバージョンの Visual Studio は、1.x の ASP.NET アプリケーションのユーザー コントロールをデザイン ビューで灰色のボックスとしてレンダリングの作成に使用します。 そのため、このアプローチを使用しているページの開発者は、いない WYSIWYG デザイン時環境をご利用いただけますでした。

ユーザー コントロールを使用する場合の欠点は、ASP.NET version 2.0 および Visual Studio 2005 での導入で対処された*マスター ページ*です。 マスター ページは、特殊な種類の両方のサイト全体のマークアップを定義する ASP.NET ページと*領域*関連付けられている、*ページのコンテンツ*カスタムのマークアップを定義します。 手順 1. でよう、これらの領域は、プレース ホルダー コントロールによって定義されます。 ContentPlaceHolder コントロールは、単にカスタム コンテンツを挿入して、コンテンツ ページで、マスター ページのコントロールの階層内の位置を表します。

> [!NOTE]
> ASP.NET version 2.0 以降には、主要な概念とマスター ページの機能が変更されていません。 ただし、Visual Studio 2008 では、Visual Studio 2005 で欠けている機能、入れ子になったマスター ページのデザイン時サポートを提供しています。 入れ子になったマスター ページを使用して、今後のチュートリアルで見ていきます。


図 2 は、www.asp.net のマスター ページの外観を示しています。 マスター ページでは、中央左に、web ページごとに固有のコンテンツが配置されている ContentPlaceHolder だけでなくサイト全体の一般的なレイアウトの上部、下部、およびすべてのページの右側にマークアップ - が定義に注意してください。


![マスター ページ コンテンツ ページ コンテンツ ページごとに、サイト全体のレイアウトや編集可能な領域を定義します。](creating-a-site-wide-layout-using-master-pages-cs/_static/image4.png)

**図 02**: マスター ページ コンテンツ ページ コンテンツ ページごとに、サイト全体のレイアウトや編集可能な領域を定義します。


マスター ページを定義した後は、チェック ボックスのチェック マークを新規の ASP.NET ページにバインドすることができます。 コンテンツ ページと呼ばれる - これらの ASP.NET ページには、マスター ページの ContentPlaceHolder のコントロールの各コンテンツ コントロールが含まれます。 コンテンツ ページがブラウザーからアクセスしたときに、ASP.NET エンジンは、マスター ページのコントロールの階層構造を作成し、コンテンツ ページのコントロールの階層では、該当する場所に挿入します。 この結合されたコントロールの階層が表示され、結果の HTML がエンドユーザーのブラウザーに返されます。 したがって、コンテンツ ページでは、一般的なマークアップをマスター ページ プレース ホルダー コントロールの外部で定義されていると、独自のコンテンツ コントロール内で定義されたページ固有のマークアップの両方を出力します。 図 3 は、この概念を示します。


[![マスター ページに、要求されたページのマークアップを定着します。](creating-a-site-wide-layout-using-master-pages-cs/_static/image6.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image5.png)

**図 03**: マスター ページに、要求されたページのマークアップを定着 ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image7.png))


マスター ページの動作に説明したが、これで、マスター ページと Visual Web Developer を使用して、関連するコンテンツ ページの作成を見てをみましょう。

> [!NOTE]
> 最も幅の広い可能な対象ユーザーを達成するために構築するこのチュートリアルの系列全体での ASP.NET web サイトが作成されますの Visual Studio 2008 では、Microsoft の無料版で ASP.NET 3.5 を使用して[Visual Web Developer 2008](https://www.microsoft.com/express/vwd/)です。 されませんがまだ心配しないで ASP.NET 3.5 にアップグレードの場合は、これらのチュートリアルの作業で説明する概念と同様 ASP.NET 2.0 および Visual Studio 2005。 ただし、デモ アプリケーションによっては、.NET Framework version 3.5; に新機能を使用できます。3.5 に固有の機能を使用している場合は、バージョン 2.0 では同様の機能を実装する方法について説明する注記を含めます。 その結果、.NET Framework version 3.5 では、チュートリアルの各ターゲットからの使用可能なデモ アプリケーションをダウンロードすることに留意してくださいは、 `Web.config` 3.5 に固有の構成要素との 3.5 に固有の名前空間への参照を含むファイル、`using` ASP.NET ページの分離コード クラス内のステートメント。 最初から 3.5 固有のマークアップを削除しないで手短、.NET 3.5 をインストール、ダウンロード可能な web アプリケーションでは、コンピューターにまだがある場合は機能しません`Web.config`です。 参照してください[分解 ASP.NET Version 3.5 の`Web.config`ファイル](http://www.4guysfromrolla.com/articles/121207-1.aspx)詳細については、このトピックの内容。 削除する必要があります、 `using` 3.5 に固有の名前空間を参照するステートメント。


## <a name="step-1-creating-a-master-page"></a>手順 1: マスター ページの作成

作成およびマスター ページとコンテンツ ページを使用して調べることができます、前に、ASP.NET web サイトが最初に必要です。 まず、新しいファイル システムに基づく ASP.NET web サイトを作成します。 これを実現する、Visual Web Developer を起動して、ファイル メニューに移動し、新しい Web サイト ダイアログを表示する新しい Web サイトを選択ボックス (図 4 を参照してください)。 ASP.NET Web サイト テンプレートの選択、ファイル システムに場所ドロップダウン リストを設定、web サイトを配置するフォルダーを選択および言語は C# の場合に設定します。 これで新しい web サイトが作成されます、 `Default.aspx` ASP.NET ページ、`App_Data`フォルダー、および`Web.config`ファイル。

> [!NOTE]
> Visual Studio には、プロジェクト管理の 2 つのモードがサポートされています。 Web サイト プロジェクトと Web アプリケーション プロジェクト。 Web サイト プロジェクトが Web アプリケーション プロジェクトを模倣プロジェクト アーキテクチャ Visual Studio .NET 2002年/2003 - プロジェクト ファイルが含まれていて、プロジェクトのソース コードに配置されている単一のアセンブリにコンパイルされ、プロジェクト ファイルの不足している、 `/bin`フォルダーです。 Visual Studio 2005 最初にのみサポートされている Web サイトのプロジェクトが、 [Web アプリケーション プロジェクト モデル](https://msdn.microsoft.com/en-us/library/aa730880(vs.80).aspx)Service Pack 1; 再導入されましたVisual Studio 2008 には、両方のプロジェクト モデルが用意されています。 Visual Web Developer 2005 および 2008 のエディションはサポート Web サイト プロジェクト。 このチュートリアル シリーズのマイ デモ Web サイト プロジェクトのモデルを使用します。 非 Express edition を使用して、Web アプリケーション プロジェクト モデルの代わりに使用する、気軽にこれがあることをいくつかの相違点、画面と手順を示すスクリーン ショットと instructio との比較を行う必要がありますに表示されるものの間に注意してください。これらのチュートリアルで提供される ns です。


[![新しいファイル システムに基づいた Web サイトを作成します。](creating-a-site-wide-layout-using-master-pages-cs/_static/image9.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image8.png)

**図 04**: New File System-Based Web サイトの作成 ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image10.png))


次に、プロジェクト名を右クリックし、新しい項目の追加 を選択して、マスター ページのテンプレートを選択すると、ルート ディレクトリ内のサイトにマスター ページを追加します。 マスター ページが、拡張子で終わることに注意してください`.master`です。 この新しいマスター ページの名前`Site.master`追加 をクリックします。


[![マスター ページを追加するという名前の web サイトに Site.master](creating-a-site-wide-layout-using-master-pages-cs/_static/image12.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image11.png)

**図 05**: マスター ページの名前付きの追加`Site.master`、web サイトに ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image13.png))


Visual Web Developer で新しいマスター ページ ファイルを追加すると、次の宣言型マークアップ マスター ページが作成されます。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample1.aspx)]

宣言型マークアップの最初の行は、 [ `@Master`ディレクティブ](https://msdn.microsoft.com/en-us/library/ms228176.aspx)です。 `@Master`ディレクティブがに似ていますが、 [ `@Page`ディレクティブ](https://msdn.microsoft.com/en-us/library/ydy4x04a.aspx)ASP.NET ページに表示されます。 これは、サーバー側の言語 (c#) と、マスター ページの分離コード クラスの継承と場所に関する情報を定義します。

`DOCTYPE`ページの宣言型マークアップが下に表示し、`@Master`ディレクティブです。 このページには、静的な HTML と共に次の 4 つのサーバー側コントロールが含まれます。

- **Web フォーム (、 `<form runat="server">`)** - すべての ASP.NET ページは、通常 Web フォームであるし、マスター ページは、Web コントロールを Web フォームに表示することを含めることができますので、マスター ページ (ではなく電子メールへの Web フォームの追加を Web フォームを追加することを確認するためコンテンツ ページ)。
- **という名前のプレース ホルダー コントロール`ContentPlaceHolder1`**  -この ContentPlaceHolder コントロールは、Web フォーム内で表示され、コンテンツ ページのユーザー インターフェイスの領域として機能します。
- **サーバー側`<head>`要素**-`<head>`要素には、`runat="server"`属性に、サーバー側コードからアクセスできるようにします。 `<head>`要素はこの方法で実装ができるように、ページのタイトルおよびその他の`<head>`-関連するマークアップを追加またはプログラムで調整することがあります。 たとえば、ASP.NET ページのプロパティに設定`Title`プロパティが変更された、`<title>`によってレンダリングされる要素、`<head>`サーバー コントロールです。
- **という名前のプレース ホルダー コントロール`head`**  -内でこのプレース ホルダー コントロールが表示されます、`<head>`サーバーを制御し、宣言的にコンテンツを追加するために使用する、`<head>`要素。

この既定のマスター ページの宣言型マークアップは、マスター ページをデザインするための開始点として機能します。 自由に HTML を編集するか、マスター ページに追加の Web コントロールまたは contentplaceholders を追加できます。

> [!NOTE]
> マスター ページをデザインすることを確認してし、マスター ページには、Web フォームが含まれています。 この Web フォーム内でプレース ホルダーを制御するには、少なくとも 1 つが表示されます。


### <a name="creating-a-simple-site-layout"></a>単純なサイトのレイアウトを作成します。

拡張しましょう`Site.master`のすべてのページを共有しているサイトのレイアウトを作成する既定の宣言型マークアップ: 共通ヘッダー; ナビゲーション、ニュースとその他のサイト全体のコンテンツです。"Microsoft ASP.NET での電源"アイコンを表示するページ フッターと左の列です。 図 6 は、ブラウザーを使って、コンテンツ ページのいずれかを表示するときに、マスター ページの最終結果を示します。 図 6 に赤い丸領域にアクセスしたページに固有です (`Default.aspx`) です。 他のコンテンツはマスター ページで定義され、したがって一貫性のあるすべてのコンテンツ ページで、します。


[![マスター ページ上、左、および下の部分を表すマークアップを定義します。](creating-a-site-wide-layout-using-master-pages-cs/_static/image15.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image14.png)

**図 06**: マスター ページ上、左、および下の部分を表すマークアップを定義する ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image16.png))


図 6 に示すようにサイトのレイアウトを実現するために更新することで開始、`Site.master`マスター ページを次の宣言型マークアップが含まれるようにします。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample2.aspx)]

マスター ページのレイアウトが、一連ので定義されている`<div>`HTML 要素です。 `topContent` `<div>` 、各ページの上部に表示されるマークアップを格納中に、 `mainContent`、 `leftContent`、および`footerContent``<div>`ページのコンテンツ、左の列と、"電源がオンで Microsoft を表示するために使用されますASP.NET"アイコンでは、それぞれします。 これらの追加に加えて`<div>`要素も変更したところ、`ID`からプライマリ ContentPlaceHolder コントロールのプロパティ`ContentPlaceHolder1`に`MainContent`です。

これらのさまざまな書式設定とレイアウト規則`<div>`に要素が記述された、[カスケード スタイル シート (CSS)](http://en.wikipedia.org/wiki/Cascading_Style_Sheets)ファイル`Styles.css`、経由で指定されている、&lt;リンク&gt;内の要素、マスター ページの&lt;ヘッド&gt;要素。 これらのさまざまなルール定義のそれぞれのルック アンド フィール`<div>`要素上で述べたです。 たとえば、 `topContent` `<div>`要素は、「マスター ページ チュートリアル」のテキストとリンクが表示されたらがで指定された書式指定規則`Styles.css`次のようにします。

[!code-css[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample3.css)]

実行する場合、コンピューターは、このチュートリアルの付属のコードをダウンロードし、追加する必要があります、`Styles.css`ファイルをプロジェクト。 同様に、イメージをという名前のフォルダーを作成し、プロジェクトをダウンロードしたデモ web サイトから、"Microsoft ASP.NET での電源"アイコンをコピーする必要があります。

> [!NOTE]
> CSS、および web ページの書式の詳細については、この記事の範囲を超えてはします。 詳細については、CSS、チェック アウト、 [CSS チュートリアル](http://www.w3schools.com/css/default.asp)で[W3Schools.com](http://www.w3schools.com/)です。ぜひ、このチュートリアルの付属のコードをダウンロードし、CSS の設定で再生`Styles.css`さまざまな書式指定規則の効果を確認します。


### <a name="creating-a-master-page-using-an-existing-design-template"></a>既存のデザインのテンプレートを使用してマスター ページを作成します。

長年にわたって多数の小規模から中規模企業の ASP.NET web アプリケーションを構築しました。 を使用したい既存のサイトのレイアウトをいた一部のクライアント他のユーザーには、資格のあるグラフィックス デザイナーとして採用されました。 いくつかでは、web サイトのレイアウトをデザインするから委託されします。 図 6 で分かるように、web サイトのレイアウトのデザインにプログラマのマルチタスクをお勧め通常として、経理担当医師は税金 open-heart surgery 実行があるとします。

幸いにも、無料の HTML デザイン テンプレート innumerous の web サイトがある - Google 検索用語「無料の web サイト テンプレート」の 600万より多くの結果が返されます 自分の好みの 1 つは[OpenDesigns.org](http://opendesigns.org/)です。たい web サイト テンプレートが見つかったらは、CSS ファイルとイメージを web サイト プロジェクトに追加し、マスター ページに、テンプレートの HTML を統合します。

> [!NOTE]
> Microsoft にも多数の[デザイン スタート キットのテンプレートの ASP.NET を空き](https://msdn.microsoft.com/en-us/asp.net/aa336613.aspx)Visual Studio で新しい Web サイト ダイアログ ボックスに統合します。


## <a name="step-2-creating-associated-content-pages"></a>手順 2: コンテンツのページは関連付けを作成します。

作成されたマスター ページ、マスター ページにバインドされている ASP.NET ページの作成を開始する準備が整いました。 このようなページをいいます*コンテンツ ページ*です。

プロジェクトに新しい ASP.NET ページを追加してにバインドしてみましょう、`Site.master`マスター ページ。 ソリューション エクスプ ローラーでプロジェクト名を右クリックし、[新しい項目の追加] オプションを選択します。 Web フォーム テンプレートを選択し、名前を入力します`About.aspx`、し、図 7 に示すように「マスター ページを選択」チェック ボックスを確認します。 これは選択マスター ページ ダイアログ ボックスを表示 (図 8 を参照してください) をボックスから使用するマスター ページを選択できます。

> [!NOTE]
> Web サイト プロジェクトのモデルではなく、Web アプリケーション プロジェクト モデルを使用して、ASP.NET web サイトを作成する場合は図 7 に示すように新しい項目の追加 ダイアログ ボックスで"Select マスター ページ"チェック ボックスは表示されません。 コンテンツを作成するには、ページ、Web アプリケーション プロジェクトを使用してモデル化する場合は、Web フォーム テンプレートの代わりに、Web コンテンツのフォーム テンプレートを選択してください。 Web コンテンツのフォーム テンプレートを選択すると、[追加] をクリックして、図 8 に示すダイアログ ボックスが表示されますマスター ページの選択と同じです。


[![新しいコンテンツ ページを追加します。](creating-a-site-wide-layout-using-master-pages-cs/_static/image18.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image17.png)

**図 07**: 新しいコンテンツ ページを追加 ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image19.png))


[![Site.master マスター ページを選択します。](creating-a-site-wide-layout-using-master-pages-cs/_static/image21.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image20.png)

**図 08**: 選択、`Site.master`マスター ページ ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image22.png))


次の宣言型マークアップを示しています、新しいコンテンツ ページが含まれています、`@Page`ことを指してそのマスタ ページおよびコンテンツ コントロール、マスター ページの ContentPlaceHolder のコントロールの各ディレクティブです。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample4.aspx)]

> [!NOTE]
> 変更したところ、「を作成する、単純なサイト レイアウト」セクションで手順 1. で`ContentPlaceHolder1`に`MainContent`です。 このプレース ホルダー コントロールの名前を変更するがない場合`ID`、同じ方法で、コンテンツ ページの宣言型マークアップが少し異なりますの前に示したマークアップ。 つまり、2 番目コンテンツ コントロールの`ContentPlaceHolderID`が反映されます、`ID`の対応する ContentPlaceHolder のマスター ページで制御します。


ASP.NET エンジンが、ページを組み込む必要がありますコンテンツ ページを表示するときにコントロールをマスター ページ プレース ホルダー コントロールのコンテンツします。 ASP.NET エンジンからのコンテンツ ページのマスター ページを決定する、`@Page`ディレクティブの`MasterPageFile`属性。 このコンテンツのページがバインドされているように、上記のマークアップを示しています、`~/Site.master`です。

マスター ページが 2 つのプレース ホルダー コントロール -`head`と`MainContent`-Visual Web Developer は、次の 2 つのコンテンツ コントロールを生成します。 各コンテンツ コントロールの参照を使用して特定 ContentPlaceHolder その`ContentPlaceHolderID`プロパティです。

デザイン時サポートはサイト全体のテンプレートの以前の技術上のマスター ページの際立つ場所です。 図 9 を示しています、 `About.aspx` Visual Web Developer のデザイン ビューで表示したときに、コンテンツ ページ。 マスター ページ コンテンツが表示されることに注目、これは淡色表示して変更できません。 マスター ページの contentplaceholders に対応するコンテンツ コントロールは、ただし、編集します。 同様に他の ASP.NET ページと、ソース ビューまたはデザイン ビューを通じて、Web コントロールを追加することで、コンテンツ ページのインターフェイスを作成できます。


[![コンテンツ ページの [デザイン] ビューには、両方のページ固有およびマスター ページ内容が表示されます。](creating-a-site-wide-layout-using-master-pages-cs/_static/image24.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image23.png)

**図 09**:、コンテンツ ページのデザイン ビューが表示されます両方ページ固有とマスター ページの内容 ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image25.png))


### <a name="adding-markup-and-web-controls-to-the-content-page"></a>コンテンツ ページにマークアップと Web コントロールの追加

一部のコンテンツを作成する、`About.aspx`ページ。 図 10 では、"について、Author"見出しと、いくつかの段落のテキストを入力したを参照してください。 が自由にも、Web コントロールを追加することができます。 このインターフェイスを作成すると、次を参照してください。、`About.aspx`ページがブラウザーを使用します。


[![ブラウザーを使って About.aspx ページを参照してください。](creating-a-site-wide-layout-using-master-pages-cs/_static/image27.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image26.png)

**図 10**: を参照してください、`About.aspx`ブラウザーを介してページ ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image28.png))


これは、要求されたコンテンツ ページと関連付けられているマスター ページ: fused は web サーバーで完全に全体として表示されることを理解しておく必要です。 エンドユーザーのブラウザーでは、結果として得られる、余裕が生まれます HTML は送信されます。 これを確認するには、ブラウザーで表示 メニューの ソース を選択して受信した HTML を表示します。 フレームなしまたは 1 つのウィンドウに次の 2 つの異なる web ページを表示するため、その他の特殊な手法ことに注意してください。

### <a name="binding-a-master-page-to-an-existing-aspnet-page"></a>既存の ASP.NET ページへのマスター ページのバインド

この手順で説明したとおりは、「マスター ページの選択」チェック ボックスをオンし、マスター ページの選択と同じくらい簡単では、ASP.NET web アプリケーションを新しいコンテンツ ページを追加することです。 残念ながら、既存の ASP.NET ページをマスター ページに変換するは簡単ではありません。

マスター ページを既存の ASP.NET ページにバインドするには、次の手順を実行する必要があります。

1. 追加、`MasterPageFile`属性を ASP.NET ページの`@Page`ディレクティブについては、適切なマスター ページをポイントします。
2. マスター ページで contentplaceholders の各コンテンツ コントロールを追加します。
3. 選択的を切り取って、適切なコンテンツ コントロールに、ASP.NET ページの既存の内容を貼り付けます。 ここで言う「選択的に」次に、ASP.NET ページの可能性があります含まれているため既にによって表されます、マスター ページなどのマークアップ、 `DOCTYPE`、`<html>`要素、および Web フォームです。

スクリーン ショットと共にこのプロセスの手順については、チェック アウト[Scott Guthrie](https://weblogs.asp.net/scottgu/)の[を使用してマスター ページとサイト ナビゲーション](http://webproject.scottgu.com/CSharp/MasterPages/MasterPages.aspx)チュートリアルです。 "更新`Default.aspx`と`DataSample.aspx`マスター ページを使用する」セクションには、これらの手順が詳しく説明します。

コンテンツ ページに既存の ASP.NET ページを変換するよりも新しいコンテンツ ページを作成するはるかに簡単なのでを作成するたびに新しい ASP.NET web サイト マスター ページを追加サイトをお勧めします。 このマスター ページには、すべての新しい ASP.NET ページをバインドします。 最初のマスター ページが非常に単純なまたは標準以外の場合、心配しないでください。後で、マスター ページを更新することができます。

> [!NOTE]
> Visual Web Developer が追加された新しい ASP.NET アプリケーションを作成するときに、`Default.aspx`マスター ページにバインドされていないページ。 使用してくださいプラクティス コンテンツ ページに既存の ASP.NET ページを変換する場合は、`Default.aspx`です。 削除する代わりに、`Default.aspx`してから、今度は「マスター ページを選択」チェック ボックスを再び追加します。


## <a name="step-3-updating-the-master-pages-markup"></a>手順 3: マスター ページのマークアップを更新します。

マスター ページの主な利点の 1 つは、サイトで多数のページの全体的なレイアウトを定義する、単一のマスター ページを使用することがあることです。 そのため、サイトのルック アンド フィールの更新には、マスター ページの 1 つのファイルの更新が必要です。

この動作を示すためには、更新、マスター ページで、左側の列の上部にある現在の日付がみましょう。 という名前のラベルを追加`DateDisplay`を`leftContent``<div>`です。

[!code-aspx[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample5.aspx)]

次に、作成、`Page_Load`マスターのイベント ハンドラー ページし、次のコードを追加します。

[!code-csharp[Main](creating-a-site-wide-layout-using-master-pages-cs/samples/sample6.cs)]

上記のコードを設定、ラベルの`Text`を現在の日付と時刻のプロパティは、曜日、月、および 2 桁の日の名前として書式設定 (図 11 を参照してください)。 この変更により、コンテンツ、ページの 1 つを再表示します。 図 11 では、結果として得られるマークアップはマスター ページへの変更を含めるすぐに更新されます。


[![マスター ページへの変更が反映されるときに表示する、コンテンツ ページ](creating-a-site-wide-layout-using-master-pages-cs/_static/image30.png)](creating-a-site-wide-layout-using-master-pages-cs/_static/image29.png)

**図 11**: マスター ページへの変更が反映されるときに表示する、コンテンツ ページ ([フルサイズのイメージを表示するをクリックして](creating-a-site-wide-layout-using-master-pages-cs/_static/image31.png))


> [!NOTE]
> この例のように、マスター ページはサーバー側の Web コントロール、コード、およびイベント ハンドラーを含めることができます。


## <a name="summary"></a>概要

マスター ページでは、ASP.NET 開発者は簡単に更新を一貫性のあるサイト全体のレイアウトをデザインを有効にします。 Visual Web Developer が豊富なデザイン時サポートを提供できると、マスター ページと、対応するコンテンツ ページの作成は標準の ASP.NET ページを作成するように単純です。

このチュートリアルで作成したマスター ページの例では 2 つのプレース ホルダー コントロール`head`と`MainContent`です。 マークアップを指定するだけ、`MainContent`コンテンツのページでは、ただし ContentPlaceHolder を制御します。 複数のコンテンツを使用して次のチュートリアルで見てコンテンツ ページのコントロールです。 表示されています。 既定値を定義する方法のコンテンツのマークアップ内で制御マスター ページで、も既定値の使用を切り替えるマークアップで定義されているマスター ページと、コンテンツ ページからカスタムのマークアップを提供する方法です。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [デザイナーの ASP.NET: Web 標準を使用して ASP.NET web サイトの構築にデザイン テンプレートおよびガイダンスを解放します。](https://msdn.microsoft.com/en-us/asp.net/aa336602.aspx)
- [ASP.NET マスター ページの概要](https://msdn.microsoft.com/en-us/library/wtxbf3hh.aspx)
- [カスケード スタイル シート (CSS) のチュートリアル](http://www.w3schools.com/css/default.asp)
- [ページのタイトルの動的設定](http://aspnet.4guysfromrolla.com/articles/051006-1.aspx)
- [ASP.NET マスター ページ](http://www.odetocode.com/articles/419.aspx)
- [マスター ページのクイック スタート チュートリアル](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/masterpages/default.aspx)

### <a name="about-the-author"></a>作成者について

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)、作成者複数受け取ります書籍や 4GuysFromRolla.com の創設者を操作した Microsoft Web テクノロジ 1998 年です。 Scott は、コンサルタント、トレーナー、ライターとして機能します。 最新の著書[ *Sam 学べる自分で ASP.NET 3.5 24 時間以内に*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)です。 Scott に到達できる[ mitchell@4GuysFromRolla.com ](mailto:mitchell@4GuysFromRolla.com)または彼のブログでを介して[http://ScottOnWriting.NET](http://scottonwriting.net/)です。

### <a name="special-thanks-to"></a>感謝の特別な

今後、MSDN の記事を確認することに関心のあるですか。 場合は、ドロップ me 一度に 1 行ずつ[ mitchell@4GuysFromRolla.com](mailto:mitchell@4GuysFromRolla.com)です。

>[!div class="step-by-step"]
[次へ](multiple-contentplaceholders-and-default-content-cs.md)
