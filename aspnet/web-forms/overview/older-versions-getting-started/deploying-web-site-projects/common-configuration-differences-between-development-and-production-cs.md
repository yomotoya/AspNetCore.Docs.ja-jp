---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
title: 一般的な構成の違いの開発と運用 (c#) |Microsoft Docs
author: rick-anderson
description: 前のチュートリアルでは、開発環境から運用環境にすべての関連ファイルをコピーすることによって、web サイトをデプロイしました。 ただし、しました.
ms.author: aspnetcontent
ms.date: 04/01/2009
ms.assetid: 721a5c37-7e21-48e0-832e-535c6351dcae
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-cs
msc.type: authoredcontent
ms.openlocfilehash: 5b3476cdb25a0fc87ce233d7e26de094a0b7b17c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808909"
---
<a name="common-configuration-differences-between-development-and-production-c"></a>開発と運用 (C++) の一般的な構成の違い
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF のダウンロード](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_cs.pdf)

> 前のチュートリアルでは、開発環境から運用環境にすべての関連ファイルをコピーすることによって、web サイトをデプロイしました。 ただし、環境ごとに一意の Web.config ファイルがあることが必要になります環境間構成の違いがあることは珍しくないです。 このチュートリアルは、一般的な構成の違いを調べ、個別の構成情報を保持するための戦略を調べる。


## <a name="introduction"></a>はじめに


最後の 2 つのチュートリアルは、単純な web アプリケーションを配置する方法を説明しました。 [ *FTP クライアントを使用してサイトを展開する*](deploying-your-site-using-an-ftp-client-cs.md)チュートリアルではスタンドアロン FTP クライアントを使用して、運用環境まで、開発環境から、必要なファイルをコピーする方法を示しました。 このチュートリアルでは、 [*を展開するサイトを使用して Visual Studio*](deploying-your-site-using-visual-studio-cs.md)、Visual Studio の Web サイトのコピー ツールと発行 オプションを使用してデプロイ時に参照します。 両方のチュートリアルでは、運用環境のすべてのファイルは、開発環境でのファイルのコピーをでした。 ただし、構成ファイルで、開発環境と異なるものを運用環境では珍しいはありません。 Web アプリケーションの構成に保存する、`Web.config`ファイルし、通常、データベース、web、および電子メール サーバーなどの外部リソースに関する情報が含まれます。 ハンドルされない例外が発生したときに実行するアクションのコースなど、特定の状況でアプリケーションの動作が綴ったもします。

Web アプリケーションをデプロイするときに、実稼働環境で適切な構成情報が終了は重要です。 ほとんどの場合、`Web.config`として運用環境には、開発環境でのファイルをコピーできません-です。 代わりに、カスタマイズされたバージョンの`Web.config`を運用環境にアップロードする必要があります。 このチュートリアルを簡単にレビューいくつかの一般的な構成の違いです。また、環境間でさまざまな構成情報を維持するためのいくつかの手法をまとめたものです。

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>開発と運用環境の間の一般的な構成の違い

`Web.config`ファイルには、さまざまな ASP.NET アプリケーションの構成情報が含まれています。 この構成情報の一部は、環境に関係なく同じです。 たとえば、認証設定と URL 承認規則に記述された、`Web.config`ファイルの`<authentication>`と`<authorization>`要素は、通常は、環境に関係なく同じです。 外部のリソースに関する情報などの他の構成情報は、通常、環境によって異なります。

データベース接続文字列は、環境に基づいてとは異なる構成情報の典型的な例です。 接続を確立する必要があります最初に、によって実現されるデータベース サーバーと web アプリケーションが通信するときに、[接続文字列](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)します。 それを配置することをお勧め、web ページや、データベースに接続するコードで直接、データベース接続文字列をハード コーディングすることはできますが、`Web.config`の[`<connectionStrings>`要素](https://msdn.microsoft.com/library/bf7sd233.aspx)ように接続文字列情報は 1 つで一元的な場所にあります。 運用環境以外で使用したものに開発中で別のデータベースが使用される多くの場合その結果、接続文字列情報は、環境ごとに一意である必要があります。

> [!NOTE]
> 今後のチュートリアルでは、この時点で、構成ファイルでデータベース接続文字列を格納する方法の詳細をについて説明、データ ドリブンのアプリケーションの配置を確認できます。


開発および運用環境の動作目的が大幅に異なります。 開発環境で web アプリケーションがによって作成される、テスト、およびデバッグ対象の開発者の小規模なグループ。 運用環境でその同じアプリケーションが多数の同時ユーザーによって参照されています。 ASP.NET には、多く開発者がテストと、アプリケーションのデバッグに役立つ機能にはが含まれていますが、パフォーマンスおよびセキュリティ上の理由から運用環境でのこれらの機能を無効にする必要があります。 このようないくつかの構成設定を見てみましょう。

### <a name="configuration-settings-that-impact-performance"></a>パフォーマンスに影響する構成設定

ASP.NET ページがアクセスしたときに初めて (または変更した後に初めて) の宣言型マークアップをクラスに変換する必要があり、このクラスをコンパイルする必要があります。 Web アプリケーションが自動コンパイルを使用している場合は、ページの分離コード クラスをすぎて、コンパイルする必要があります。 さまざまなコンパイル オプションを使用してを構成することができます、`Web.config`ファイルの[`<compilation>`要素](https://msdn.microsoft.com/library/s10awwz0.aspx)します。

Debug 属性で最も重要な属性の 1 つ、`<compilation>`要素。 場合、`debug`属性がコンパイル済みアセンブリは、Visual Studio でアプリケーションをデバッグするときに必要なデバッグ シンボルを含めるし、"true"に設定します。 デバッグ シンボルを選択し、アセンブリのサイズを増やし、コードを実行するときに、追加のメモリ要件を強制します。 さらに、ときに、`debug`属性によって返される任意のコンテンツを"true"に設定されて`WebResource.axd`はキャッシュされません、つまり、毎回ユーザーがページにアクセスを再度によって返される静的コンテンツをダウンロードする必要があります`WebResource.axd`します。

> [!NOTE]
> `WebResource.axd` 組み込み HTTP ハンドラー 2.0 で導入された ASP.NET サーバー コントロールの使用など、スクリプト ファイル、イメージ、CSS ファイル、およびその他のコンテンツの埋め込みリソースを取得します。 方法の詳細についての`WebResource.axd`しくみと、カスタム サーバー コントロールから埋め込みリソースにアクセスの使用方法を参照してください。[にアクセスする埋め込みリソースで、URL を使用して`WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx)します。


`<compilation>`要素の`debug`属性に設定は、通常、開発環境では"true"です。 実際には、この属性は、web アプリケーションをデバッグするためには、"true"に設定する必要があります。Visual Studio から ASP.NET アプリケーションをデバッグしようとするかどうか、`debug`属性が"false"に設定、Visual Studio はまで、アプリケーションをデバッグすることはできませんを説明するメッセージを表示、`debug`属性が"true"とに設定この変更を行えるに提供します。

必要があります**決して**が、`debug`属性がパフォーマンスに影響するそのため、運用環境では、"true"に設定します。 このトピックの詳細についてを参照してください[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ投稿「[しない実行実稼働 ASP.NET アプリケーションで`debug="true"`有効](https://weblogs.asp.net/scottgu/442448)します。

### <a name="custom-errors-and-tracing"></a>カスタム エラーおよびトレース

ASP.NET アプリケーションでハンドルされない例外が発生した場合は、次の 3 つのいずれかが発生する時点で、実行時までバブルします。

- 一般的なランタイム エラー メッセージが表示されます。 このページには、ランタイム エラーが発生しました、エラーの詳細は提供されませんが、ユーザーが表示されます。
- だけスローされた例外の情報を含む例外の詳細メッセージが表示されます。
- 必要に応じて任意のメッセージを表示するを作成する ASP.NET ページは、カスタム エラー ページが表示されます。

ハンドルされない例外が発生した場合の動作によって異なります、`Web.config`ファイルの[`<customErrors>`セクション](https://msdn.microsoft.com/library/h0hfz6fc.aspx)します。

開発や、ブラウザーで任意の例外の詳細を見るのに役立つアプリケーションをテストします。 ただし、潜在的なセキュリティ リスクは、運用上のアプリケーションの例外の詳細を表示します。 さらに、flattering でないし、不自然に見える場合、web サイトを使用します。 理想的には、ハンドルされない例外が発生した場合、開発環境で web アプリケーションの詳細が表示されます、例外の実稼働環境で同じアプリケーションのカスタム エラー ページは表示します。

> [!NOTE]
> 既定の`<customErrors>`セクションの設定は、ページが localhost、を通じて参照されていると、それ以外の場合、一般的なランタイム エラー ページを表示している場合にのみ、例外の詳細がメッセージを示しています。 これは理想的ではありませんが、既定の動作が非ローカルの訪問者に例外の詳細を表示しないことを把握することを確保することは。 今後のチュートリアルを調べ、`<customErrors>`詳細セクションと実稼働環境でエラーが発生したときに表示されるカスタム エラー ページが存在する方法について説明します。


開発時に役に立つもう 1 つの ASP.NET 機能をトレースしています。 トレース、有効な場合、各受信要求に関する情報を記録および特別な web ページでは、提供`Trace.axd`、最新の要求の詳細を表示するためです。 有効にして使用してトレースを構成することができます、 [ `<trace>`要素](https://msdn.microsoft.com/library/6915t83k.aspx)で`Web.config`します。

有効にした場合なトレースの作成を確認運用環境で無効にします。 トレース情報には、cookie、セッション データ、およびその他の機密情報が含まれているためには、実稼働環境でトレースを無効にする必要があります。 良い知らせは、既定では、トレースが無効であると、`Trace.axd`ファイルはローカル ホストからアクセスできるのみです。 開発の既定の設定を変更する場合をオフにする戻る実稼働環境でを確認します。

## <a name="techniques-for-maintaining-separate-configuration-information"></a>個別の構成情報を保持するための手法

開発および運用環境で異なる構成設定が発生すると、展開プロセスが複雑になります。 前の 2 つのチュートリアルで、展開プロセス関係開発から運用環境にコピーするすべての必要なファイルのアプローチは、構成情報は、両方の環境で同じ場合にのみ機能するが、します。 さまざまなさまざまな構成情報を持つアプリケーションをデプロイするための手法があります。 ホステッド web アプリケーションにこれらのオプションの一部をカタログみましょう。

### <a name="manually-deploying-the-production-environment-configuration-file"></a>運用環境の構成ファイルを手動で展開します。

最も簡単な方法は 2 つのバージョンを維持するためには、`Web.config`ファイル: 開発環境と運用環境のいずれかのいずれか。 サイトを運用環境にデプロイする場合は、開発環境で実稼働サーバーへのすべてのファイルをコピーする必要があります*を除く*の`Web.config`ファイル。 代わりに、実稼働環境に固有`Web.config`ファイルを運用環境にコピーします。

この方法は非常に高度ではありませんが、簡単に構成情報の変更頻度が低いために実装できます。 1 つの web サーバーでホストされていると、その構成情報が変更頻度の低い小規模な開発チームとアプリケーションに最適に動作します。 スタンドアロン FTP クライアントを使用してアプリケーション ファイルを手動で展開する場合、最も tenable です。 Visual Studio の Web サイトのコピー ツールまたは発行 オプションを使用する場合は、まず、展開に固有の入れ替えする必要があります`Web.config`ファイルを展開する前に運用環境に固有のものとし、デプロイが完了後に交換します。

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>ビルドまたは展開プロセス中に、構成を変更します。

ディスカッションこれまでと想定しています、アドホックのビルドおよび配置プロセスです。 多くの大規模なソフトウェア プロジェクト使用のオープン ソースの自社製またはサードパーティ製のツールを構成するプロセスはより正式な。 このようなプロジェクトのビルドまたは配置プロセスを運用環境レバーが前に、構成情報を適切に変更する可能性がありますをカスタマイズできます。 使用して web アプリケーションをビルドする場合[MSBuild](http://en.wikipedia.org/wiki/MSBuild)、 [NAnt](http://nant.sourceforge.net/)、またはその他のいくつかのビルド ツール、ビルド ステップを変更する可能性があります追加できます、`Web.config`ファイルを運用環境に固有の設定が含まれます。 デプロイのワークフローのソース管理サーバーに接続し、適切な取得でしたプログラムでまたは`Web.config`ファイル。

運用環境に適切な構成情報を取得するための実際のアプローチは、ツールとワークフローに応じて大きく変わります。 そのため、さらに、このトピックにこれについてはありません。 MSBuild または NAnt などの人気のあるビルド ツールを使用している見つかりますデプロイに関する記事とチュートリアル web 検索を通じてこれらのツールに固有です。

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>アドイン プロジェクトの Web 配置を使用して管理する構成の違い

Microsoft は、2006 年に、Visual Studio 2005 の Web 開発プロジェクト アドイン リリース。 Visual Studio 2008 用のアドインでは、2008 年にリリースされました。 このアドインでにより、ビルド時に、web アプリケーション プロジェクトと共に別の Web 展開プロジェクトを作成する ASP.NET 開発者向けの明示的に web アプリケーションをコンパイルし、ローカルの出力ディレクトリに配置するファイルをコピーします。 Web アプリケーション プロジェクトでは、バック グラウンドで MSBuild を使用します。

既定では、開発環境の`Web.config`ファイルは、出力ディレクトリにコピーされますが、カスタマイズする Web 展開プロジェクトをセットアップすることができます、

次の方法でこのディレクトリにコピーを取得する構成情報:

- 使用して`Web.config`ファイルのセクションの置換、置換するセクションを指定して、置換テキストを含む XML ファイル。
- 外部構成ソース ファイルへのパスを提供します。 このオプションを選択すると、Web 展開プロジェクトにコピー特定`Web.config`出力ディレクトリにファイル (なく`Web.config`開発環境で使用されるファイル)。
- Web 展開プロジェクトで使用される MSBuild ファイルには、カスタム ルールを追加します。

Web アプリケーションのビルド Web 展開プロジェクトを展開し、プロジェクトの出力フォルダーからファイルを運用環境にコピーします。

チェック アウト Web 展開プロジェクトの使用方法の詳細については、 [Web Deployment Projects 今回](https://msdn.microsoft.com/magazine/cc163448.aspx)2007 年 4 月号の[MSDN Magazine](https://msdn.microsoft.com/magazine/default.aspx)にある「関連項目」セクション リンクを参照してください。 または、このチュートリアルの最後。

> [!NOTE]
> Web 展開プロジェクトは、Visual Studio アドインとして実装され、Visual Studio Express Editions (Visual Web Developer を含む) は、アドインをサポートしていませんので、Visual Web Developer で Web 展開プロジェクトを使用することはできません。


## <a name="summary"></a>まとめ

開発での web アプリケーションの動作と外部リソースは通常、同じアプリケーションの場合は、実稼働環境で異なるです。 たとえば、データベース接続文字列、コンパイル オプション、および未処理の例外は一般的に発生したときの動作は、環境間で異なります。 展開プロセスは、これらの違いに対応する必要があります。 このチュートリアルで説明したように、最も簡単な方法は、運用環境に、代替の構成ファイルを手動でコピーするは。 洗練されたソリューションは、可能な Web 展開プロジェクト アドインを使用する場合、またはこのようなカスタマイズに対応できるより正式なビルドまたは配置のプロセス。

満足のプログラミングです。

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [接続文字列の説明](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [データベース接続文字列の @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [使用して実稼働 ASP.NET アプリケーションを実行しない`debug="true"`有効になっています。](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [わかりやすいエラー ページの表示 - 未処理の例外を適切に応答](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [How Do i: の利用で、Visual Studio 2008 Web 展開プロジェクトですか。](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [データベースをデプロイするときに、主要な構成設定](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web 配置プロジェクト ダウンロード](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Web 配置プロジェクト ダウンロード](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web Deployment Projects](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 Web 配置プロジェクト サポート リリース](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Web 配置プロジェクト](https://msdn.microsoft.com/magazine/cc163448.aspx)

> [!div class="step-by-step"]
> [前へ](deploying-your-site-using-visual-studio-cs.md)
> [次へ](core-differences-between-iis-and-the-asp-net-development-server-cs.md)
