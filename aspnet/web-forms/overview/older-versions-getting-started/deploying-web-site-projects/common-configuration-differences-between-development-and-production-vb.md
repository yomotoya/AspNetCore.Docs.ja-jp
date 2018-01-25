---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
title: "一般的な構成の違い開発および運用 (VB) |Microsoft ドキュメント"
author: rick-anderson
description: "前のチュートリアルは展開当社の web サイト開発環境から実稼働環境へのすべての関連するファイルのコピーで。 ただし、しました."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/01/2009
ms.topic: article
ms.assetid: 548e75f6-4d6c-4cb4-8da8-417915eb8393
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/common-configuration-differences-between-development-and-production-vb
msc.type: authoredcontent
ms.openlocfilehash: 8de1acada8713abf5f92c1f13fa82a5d4ccc18be
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 01/24/2018
---
<a name="common-configuration-differences-between-development-and-production-vb"></a>開発および運用 (VB) の間で共通の構成の相違点
====================
によって[Scott Mitchell](https://twitter.com/ScottOnWriting)

[PDF をダウンロードします。](http://download.microsoft.com/download/E/8/9/E8920AE6-D441-41A7-8A77-9EF8FF970D8B/aspnet_tutorial05_ConfigDifferences_vb.pdf)

> 前のチュートリアルは展開当社の web サイト開発環境から実稼働環境へのすべての関連するファイルのコピーで。 ただし、珍しくありません環境は、一意の Web.config ファイルの各環境にいる必要のある構成の違いが場合もあります。 このチュートリアルでは、一般的な構成の相違点を調べを別の構成情報を保持するための戦略を見ます。


## <a name="introduction"></a>はじめに


最後の 2 つのチュートリアルでは、単純な web アプリケーションを配置するをとおし、します。 [ *FTP クライアントを使用して、サイトを展開する*](deploying-your-site-using-an-ftp-client-vb.md)チュートリアルでは、スタンドアロンの FTP クライアントを使用して、実稼働まで開発環境から、必要なファイルをコピーする方法を示しました。 前のチュートリアルでは、 [*を展開する、サイトを使用して Visual Studio*](deploying-your-site-using-visual-studio-vb.md)、Visual Studio の Web サイトのコピー ツールと発行 オプションを使用して展開で参照します。 両方のチュートリアルでは、運用環境のすべてのファイルは、開発環境でファイルのコピーをでした。 ただし、開発環境で異なるを実稼働環境での構成ファイルは珍しいはありません。 Web アプリケーションの構成に保存、`Web.config`ファイルし、通常、データベース、web、および電子メール サーバーなどの外部リソースに関する情報が含まれます。 ハンドルされない例外が発生したときに実行するアクションなどの特定の状況で、アプリケーションの動作が詳しくもします。

Web アプリケーションを配置するときは、適切な構成情報終わるを実稼働環境で重要です。 ほとんどの場合、`Web.config`として、運用環境に開発環境でファイルをコピーすることはできません-はします。 代わりに、カスタマイズされたバージョンの`Web.config`実稼働環境にアップロードする必要があります。 このチュートリアルを簡単にレビューいくつかの一般的な構成の相違点です。環境間でさまざまな構成情報を維持するためのいくつかの方法の概要も示します。

## <a name="typical-configuration-differences-between-the-development-and-production-environments"></a>一般的な構成、開発と実稼働環境の相違点

`Web.config`ファイルには、多種多様な ASP.NET アプリケーションの構成情報が含まれています。 この構成情報の一部は、環境に関係なく同じです。 たとえば、URL 承認規則と認証設定に記述された、`Web.config`ファイルの`<authentication>`と`<authorization>`要素は、通常は、環境に関係なく同じです。 外部リソースに関する情報などの他の構成情報は、通常、環境によって異なります。

データベース接続文字列は、環境によっては異なる構成情報の主要な例です。 Web アプリケーションが通信する場合、データベース サーバーに接続を確立する必要があります最初とは、によって達成される、[接続文字列](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)です。 配置することをお勧め web ページや、データベースに接続するコードで直接、データベース接続文字列をハード コーディングすることはできますが、`Web.config`の[`<connectionStrings>`要素](https://msdn.microsoft.com/library/bf7sd233.aspx)ように接続文字列情報は、1 つ、一元化された場所には。 別のデータベースは開発時に使用される; 実稼働環境で使用するよりも多くの場合その結果、接続文字列情報は、環境ごとに一意でなければなりません。

> [!NOTE]
> 今後のチュートリアルでは、この時点で、構成ファイルでデータベース接続文字列を格納する方法の詳細に進むが、データ ドリブンのアプリケーションの配置について説明します。


開発および運用環境の目的の動作は大幅に異なります。 開発環境で web アプリケーションがによって作成される、テスト、およびデバッグ開発者の小規模なグループです。 実稼働環境で同じアプリケーションが多数の同時ユーザーによって参照されていること。 ASP.NET には、いくつかのテストと、アプリケーションのデバッグでは開発者を支援する機能が含まれていますが、パフォーマンスおよびセキュリティ上の理由から、実稼働環境でのこれらの機能を無効にする必要があります。 このようないくつかの構成設定を見てみましょう。

### <a name="configuration-settings-that-impact-performance"></a>パフォーマンスに影響する構成設定

ASP.NET ページにアクセスするときに、初めて (または変更した後に初めて) の宣言型マークアップをクラスに変換する必要があり、このクラスをコンパイルする必要があります。 Web アプリケーションは、自動コンパイルを使用している場合は、ページの分離コード クラスをすぎて、コンパイルする必要があります。 多種多様なコンパイル オプションを構成することができます、`Web.config`ファイルの[`<compilation>`要素](https://msdn.microsoft.com/library/s10awwz0.aspx)です。

Debug 属性は、最も重要な属性での 1 つ、`<compilation>`要素。 場合、`debug`属性がコンパイルされたアセンブリが Visual Studio でアプリケーションをデバッグするときに必要なは、デバッグ シンボルを含めるし、"true"に設定します。 デバッグ シンボルはアセンブリのサイズが大きくなり、コードを実行するときに、追加のメモリ要件を強制します。 さらに、ときに、`debug`属性がによって返されるすべてのコンテンツを"true"に設定されている`WebResource.axd`がキャッシュされていない、つまり、たびに、ユーザー ページにアクセスを再度によって返される静的なコンテンツをダウンロードする必要があります`WebResource.axd`です。

> [!NOTE]
> `WebResource.axd`組み込みの HTTP ハンドラーで導入された ASP.NET 2.0 サーバー コントロールは、スクリプト ファイル、画像、CSS ファイル、およびその他のコンテンツなどの埋め込みリソースを取得するために使用します。 方法の詳細については`WebResource.axd`および動作を使用して、カスタム サーバー コントロールから埋め込みリソースにアクセスする方法を参照してください。[にアクセスする埋め込みリソースで、URL を使用して`WebResource.axd`](http://aspnet.4guysfromrolla.com/articles/080906-1.aspx)です。


`<compilation>`要素の`debug`属性が通常に設定されている開発環境では"true"です。 実際には、この属性は、web アプリケーションをデバッグするのには、"true"に設定する必要があります。Visual Studio から ASP.NET アプリケーションをデバッグしようとするかどうか、`debug`属性が"false"に設定されている、Visual Studio は、メッセージを説明するまで、アプリケーションをデバッグすることはできませんを表示、`debug`属性が"true"とに設定このように変更するを提供します。

行う必要があります**決して**が、`debug`属性パフォーマンスへの影響のため、運用環境では、"true"に設定します。 このトピックの詳細についてを参照してください[Scott Guthrie](https://weblogs.asp.net/scottgu/)のブログ投稿「[しない実行運用 ASP.NET アプリケーションで`debug="true"`有効](https://weblogs.asp.net/scottgu/442448)です。

### <a name="custom-errors-and-tracing"></a>カスタム エラーおよびトレース

ASP.NET アプリケーションでハンドルされない例外が発生したときに、次の 3 つのいずれかが発生する時点で実行時までバブルします。

- 一般的なランタイム エラー メッセージが表示されます。 このページでは、ランタイム エラーが発生しました、エラーの詳細は提供されませんをユーザーに通知します。
- 例外の詳細メッセージが表示されます、だけスローされた例外に関する情報が含まれます。
- ASP.NET ページを作成することが望ましくない任意のメッセージが表示されますが、カスタム エラー ページが表示されます。

未処理の例外が発生した場合の動作によって異なります、`Web.config`ファイルの[`<customErrors>`セクション](https://msdn.microsoft.com/library/h0hfz6fc.aspx)です。

開発およびアプリケーションのテストには、ブラウザーですべての例外の詳細を表示するのに役立ちます。 ただし、運用上のアプリケーションの例外の詳細を表示は、潜在的なセキュリティ リスクです。 さらに、そのいない flattering と不自然に見える web サイトです。 理想的には、未処理の例外が発生した場合、web アプリケーション開発環境で詳細が表示されます、例外の実稼働環境で同じアプリケーションがカスタム エラー ページを表示するときにします。

> [!NOTE]
> 既定値`<customErrors>`セクションの設定は、ページが localhost、を通じて参照されると、それ以外の場合、一般的なランタイム エラー ページを表示している場合にのみメッセージの例外の詳細を示しています。 これは、理想的ではありませんが、ある既定の動作を表示せずにローカルでない訪問者を例外の詳細を知ることを確保することがします。 今後のチュートリアルを調べ、`<customErrors>`さらに詳しくセクション実稼働環境でエラーが発生したときに表示されるカスタム エラー ページが存在する方法を示しています。


開発時に便利ですがもう 1 つの ASP.NET 機能をトレースしています。 トレース、有効な場合、各入力方向の要求に関する情報を記録および特別な web ページでは、用意されています`Trace.axd`、最新の要求の詳細を表示するためです。 有効にして使用してトレースを構成する、 [ `<trace>`要素](https://msdn.microsoft.com/library/6915t83k.aspx)で`Web.config`です。

有効にした場合なトレースの作成を確認して実稼働環境で無効になっていること。 トレース情報には、クッキー、セッション データ、およびその他の機密情報が含まれているために、実稼働環境でトレースを無効にする必要があります。 良いニュースは、既定では、トレースが無効であると、`Trace.axd`ファイルは、localhost を使用してアクセスのみです。 開発の既定の設定を変更する場合をオフにする戻る実稼働環境でを確認します。

## <a name="techniques-for-maintaining-separate-configuration-information"></a>別の構成情報を保持するための手法

開発と実稼働環境で異なる構成設定を持つ、配置プロセスが複雑になります。 前の 2 つのチュートリアルで、展開プロセス関与を実稼働での開発からのすべての必要なファイルをコピーが、その方法は、構成情報は、両方の環境で同じ場合にのみ機能します。 さまざまなさまざまな構成情報を持つアプリケーションを展開するための方法があります。 ホストされる web アプリケーションにこれらのオプションの一部をカタログみましょう。

### <a name="manually-deploying-the-production-environment-configuration-file"></a>実稼働環境の構成ファイルを手動で展開します。

最も簡単な方法は 2 つのバージョンを維持するためには、`Web.config`ファイル: 開発環境と運用環境のいずれかのいずれか。 実稼働環境にサイトを展開する場合は、開発環境で実稼働サーバーへのすべてのファイルのコピーする必要があります*を除く*の`Web.config`ファイル。 代わりに、実稼働環境に固有`Web.config`ファイルを実稼働環境にコピーができます。

この方法は非常に高度ではありませんが、簡単に構成情報の変更頻度が低いために実装します。 小規模な開発チームと 1 つの web サーバーでホストされているの構成情報が変更される頻度の低いアプリケーションに最適動作します。 スタンドアロンの FTP クライアントを使用してアプリケーション ファイルを手動で展開する場合、最も tenable です。 Visual Studio の Web サイトのコピー ツールまたは発行 オプションを使用する必要があります最初をスワップ アウト展開固有`Web.config`、展開する前に、実稼働環境に固有でファイルし、し、展開が完了した後にスワップします。

### <a name="change-the-configuration-during-the-build-or-deployment-process"></a>ビルドまたは配置処理中に構成を変更します。

これまで、ディスカッションは、アドホック ビルドおよび配置プロセスを想定しました。 多くの大規模なソフトウェア プロジェクト オープン ソース、自社製の使用、サード パーティのツールを構成するプロセスはより定式化します。 このようなプロジェクトの可能性がありますを実稼働環境にこれがプッシュされる前に適切に構成情報を変更するビルドまたは配置プロセスをカスタマイズできます。 使用して web アプリケーションをビルドする場合[MSBuild](http://en.wikipedia.org/wiki/MSBuild)、 [NAnt](http://nant.sourceforge.net/)、いくつかのビルド ツールを追加することもできます可能性がありますを変更するビルド ステップ、`Web.config`を実稼働環境に固有の設定を含めるファイルです。 展開ワークフローでしたプログラムでソース管理サーバーに接続し、適切なを取得または`Web.config`ファイル。

実稼働環境に適切な構成情報を取得するための実際のアプローチは、ツールとワークフローに応じて大きく変わります。 そのため、ありませんを扱ってさらにこのトピックの内容。 MSBuild または NAnt などの人気のあるビルド ツールを使用している場合が表示のデプロイに関する記事やチュートリアルこれらのツールは web 検索を実行する特定されます。

### <a name="managing-configuration-differences-via-the-web-deployment-project-add-in"></a>アドインのプロジェクトの Web 配置を使用して管理する構成の違い

2006 年にリリース Web 開発プロジェクト アドインで Visual Studio 2005 用。 Visual Studio 2008 では、2008 年にリリースされました。 このアドインでは、ASP.NET 開発者は独立した Web デプロイメント プロジェクトを作成、web アプリケーション プロジェクトと共に、ビルド時に、明示的に web アプリケーションをコンパイルおよびローカル出力ディレクトリに配置するファイルをコピーします。 Web アプリケーション プロジェクトでは、バック グラウンドで MSBuild を使用します。

既定では、開発環境の`Web.config`ファイルは出力ディレクトリにコピーしますが、Web デプロイ プロジェクトをカスタマイズするをセットアップすることができます、

構成の詳細については次の方法でこのディレクトリにコピーを取得します。

- 使用して`Web.config`ファイル セクションの交換、置換するセクションを指定して、置換するテキストを含んだ XML ファイルです。
- 外部構成ソース ファイルへのパスを提供します。 このオプションを選択して、Web デプロイ プロジェクトにコピー特定`Web.config`出力ディレクトリにファイル (ではなく、`Web.config`開発環境で使用されるファイル)。
- Web デプロイメント プロジェクトによって使用される MSBuild ファイルをカスタム規則を追加します。

Web アプリケーションのビルド Web デプロイメント プロジェクトを展開し、プロジェクトの出力フォルダーからファイルを実稼働環境にコピーします。

チェック アウト Web デプロイメント プロジェクトの使用に関する詳細については、[この Web プロジェクトのデプロイ記事](https://msdn.microsoft.com/magazine/cc163448.aspx)の 2007 年 4 月問題から[MSDN マガジン](https://msdn.microsoft.com/magazine/default.aspx)で関連項目セクションにあるリンクを参照してくださいまたは、。このチュートリアルの最後。

> [!NOTE]
> Web デプロイメント プロジェクトは、Visual Studio アドインとして実装されており、Visual Studio Express Edition (Visual Web Developer を含む) は、アドインをサポートしていませんので、Visual Web Developer で Web デプロイ プロジェクトを使用できません。


## <a name="summary"></a>まとめ

開発で web アプリケーションの動作と外部リソースは、同じアプリケーションが運用環境の場合よりも通常異なります。 たとえば、データベース接続文字列、コンパイル オプション、および未処理の例外は一般的に発生したときの動作は、環境間で異なります。 展開プロセスは、これらの違いに対応する必要があります。 このチュートリアルで説明したよう、簡単な方法は、運用環境に代替の構成ファイルを手動でコピーします。 洗練されたソリューションは、可能な Web 展開プロジェクト アドインを使用する場合、またはこのようなカスタマイズを格納できるより正式なビルドまたは配置プロセスとします。

満足プログラミング!

### <a name="further-reading"></a>関連項目

このチュートリアルで説明したトピックの詳細については、次の情報を参照してください。

- [接続文字列の説明](http://www.connectionstrings.com/Articles/Show/what-is-a-connection-string)
- [データベース接続文字列を @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [使用して実稼働 ASP.NET アプリケーションを実行しない`debug="true"`有効になっています。](https://weblogs.asp.net/scottgu/Don_1920_t-run-production-ASP.NET-Applications-with-debug_3D001D20_true_1D20_-enabled)
- [わかりやすいエラー ページの表示 - 未処理の例外に応答して適切に](http://aspnet.4guysfromrolla.com/articles/090606-1.aspx)
- [操作方法: を使用して、Visual Studio 2008 の Web 配置プロジェクトしますか。](../../../videos/how-do-i/how-do-i-use-a-visual-studio-2008-web-deployment-project.md)
- [データベースを展開するときに、キーの構成設定](http://aspnet.4guysfromrolla.com/articles/121008-1.aspx)
- [Visual Studio 2008 Web 展開プロジェクト ダウンロード](https://www.microsoft.com/downloads/details.aspx?FamilyId=0AA30AE8-C73B-4BDD-BB1B-FE697256C459&amp;displaylang=en) | [Visual Studio 2005 Web 展開プロジェクト ダウンロード](https://download.microsoft.com/download/9/4/9/9496adc4-574e-4043-bb70-bc841e27f13c/WebDeploymentSetup.msi)
- [VS 2008 Web デプロイメント プロジェクト](https://weblogs.asp.net/scottgu/archive/2005/11/06/429723.aspx) | [VS 2008 の Web 配置でプロジェクトのサポートがリリース](https://weblogs.asp.net/scottgu/archive/2008/01/28/vs-2008-web-deployment-project-support-released.aspx)
- [Web 配置プロジェクト](https://msdn.microsoft.com/magazine/cc163448.aspx)

>[!div class="step-by-step"]
[前へ](deploying-your-site-using-visual-studio-vb.md)
[次へ](core-differences-between-iis-and-the-asp-net-development-server-vb.md)
