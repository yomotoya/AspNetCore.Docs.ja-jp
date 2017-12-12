---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
title: "SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: プロジェクト プロパティの構成 - 4/12 |Microsoft ドキュメント"
author: tdykstra
description: "この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Stu を使用して、SQL Server Compact データベースが含まれています."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 8b013630-842c-4d44-a6fc-c6be43e7210f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-configuring-project-properties-4-of-12
msc.type: authoredcontent
ms.openlocfilehash: 2ba202a1a0d0ba752576e8906b739cc9e83fde2a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-configuring-project-properties---4-of-12"></a>SQL Server compact の Visual Studio または Visual Web Developer を使用して ASP.NET Web アプリケーションの配置: プロジェクト プロパティの構成 - 4/12
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> この一連のチュートリアルは、展開する方法を示します (発行)、ASP.NET web アプリケーション プロジェクトを Visual Studio 2012 RC または Visual Studio Express 2012 RC for Web を使用して SQL Server Compact データベースが含まれます。 Web 公開の更新プログラムをインストールする場合は、また Visual Studio 2010 を使用することができます。 系列の概要については、次を参照してください。[系列内の最初のチュートリアル](deployment-to-a-hosting-provider-introduction-1-of-12.md)です。
> 
> チュートリアルについては、RC のリリースの Visual Studio 2012 以降の展開機能を示しています、SQL Server Compact 以外の SQL Server のエディションを展開する方法を示していますし、Azure App Service Web アプリを展開する方法を示しています、次を参照してください[ASP.NET Web 配置。Visual Studio を使用して](../../deployment/visual-studio-web-deployment/introduction.md)です。


## <a name="overview"></a>概要

一部の展開オプションは、プロジェクト ファイルに格納されているプロジェクトのプロパティで構成されます (、 *.csproj*または*.vbproj*ファイル)。 ほとんどの場合、これらの設定の既定値は目的の動作が使用できる、**プロジェクト プロパティ**UI は、それらを変更する必要がある場合、これらの設定を使用する Visual Studio に組み込まれています。 このチュートリアルでは、展開設定を確認する**プロジェクト プロパティ**です。 空のフォルダーを展開するを原因となったプレース ホルダー ファイルを作成します。

## <a name="configuring-deployment-settings-in-the-project-properties-window"></a>プロジェクトのプロパティ ウィンドウで展開の設定を構成します。

展開の実行時の動作に影響する設定のほとんどは、次のチュートリアルで後ほど、発行プロファイルに含まれます。 内の対応する必要がありますをいくつかの設定にある、**パッケージ化/発行**のタブ、**プロジェクト プロパティ**ウィンドウです。 これらの設定は各ビルド構成の指定-デバッグ ビルドである場合よりも、リリース ビルドの別の設定を持つことができます、します。

**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトで、**プロパティ**、クリックして、**パッケージ化/発行 Web**タブです。

![Package_Publish_Web_tab](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image1.png)

ウィンドウが表示されたらのいずれかのビルド構成は、ソリューションの現在アクティブな設定が表示された既定になります。 場合、**構成**ボックスは示しません**アクティブ (リリース)****リリース**リリース ビルド構成の設定を表示するためにします。 リリース ビルド、テストと実稼働環境を展開します。

![Package_Publish_Web_tab_selecting_Release](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image2.png)

**アクティブ (リリース)**または**リリース**リリース ビルド構成を使用して展開するときに有効な値を選択すると、表示します。

- **を展開する項目**ボックスで、**アプリケーションの実行に必要なファイルのみ**が選択されています。 その他のオプションは**このプロジェクト内のすべてのファイル**または**このプロジェクト フォルダー内のすべてのファイル**です。 既定の選択内容を変更することではたとえば、ソース コード ファイルを配置しないでください。 この設定は、SQL Server Compact のバイナリ ファイルを含むフォルダーをプロジェクトに含める必要がある理由理由です。 この設定の詳細については、次を参照してください。**理由しないすべてのプロジェクト フォルダーにファイルのデプロイしますか?**で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/en-us/library/ee942158.aspx)です。
- **デバッグ シンボルの生成された除外**が選択されています。 このビルド構成を使用する場合にデバッグはありません。
- **アプリからファイルを除外する\_データ フォルダー**が選択されていません。 SQL Server Compact データベースのファイル、メンバーシップはそのフォルダー内および展開する必要があります。 データベースの変更が含まれていない更新プログラムを展開する場合は、このチェック ボックスを選択します。
- **パブリッシュする前にこのアプリケーションをプリコンパイル**が選択されていません。 ほとんどのシナリオでは、web アプリケーション プロジェクトをプリコンパイルする必要はありません。 詳細については、このオプションは、次を参照してください。[パッケージ化/発行 Web タブ、プロジェクトのプロパティ](https://msdn.microsoft.com/en-us/library/dd410108(v=vs.110).aspx)と[プリコンパイルの設定 ダイアログの高度な](https://msdn.microsoft.com/en-us/library/hh475319(v=vs.110).aspx)します。
- **SQL のパッケージ化/発行 タブで構成されているすべてのデータベースを含める**を選択すると、このオプションがありません効果今すぐ構成していないため、**パッケージ化/発行 SQL**  タブ。そのタブは SQL Server データベースを展開するための唯一のオプションを使用する従来のデータベースの展開方法です。 使用して、**パッケージ化/発行 SQL**  タブで、 [SQL Server への移行](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)チュートリアルです。
- **Web 展開パッケージの設定**1 回のクリックを使用しているために、セクションでは適用されませんこれらのチュートリアルで公開します。

変更、**構成**デバッグ ビルドの場合、既定の設定を表示するデバッグする ボックスの一覧です。 値は、同じ値を除く、**生成されたデバッグ シンボルを除外する**がクリアできるように、デバッグ ビルドを配置する場合は、デバッグを行うことができます。

## <a name="making-sure-that-the-elmah-folder-gets-deployed"></a>Elmah フォルダーが配置を取得することを確認をします。

前のチュートリアルで説明したとおり、 [Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポートの機能を提供します。 Contoso 大学アプリケーションで Elmah が構成されているという名前のフォルダーにエラーの詳細を格納する*Elmah*:

![Elmah フォルダー](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12/_static/image3.png)

一般的な要件は、展開からの特定のファイルまたはフォルダーの除外別の例には、ユーザーがファイルをアップロードできるフォルダーがあります。 ログ ファイルをしたくないか、実稼働環境に展開する開発環境で作成されたファイルをアップロードします。 あり、展開プロセスを実稼働環境に存在するファイルを削除しない更新プログラムを実稼働環境に配置する場合。 (設定に応じて、配置オプションを展開するときに、移行先サイトがソース サイトではなく、ファイルが存在する場合、Web 配置から削除、変換先です。)

このチュートリアルで既に説明したとおり、**を展開する項目**オプション、**パッケージ化/発行 Web**に設定されているタブ**のみこのアプリケーションの実行に必要なファイル**です。 その結果、開発で Elmah によって作成されるログ ファイルは配置されません、これは、発生します。 (プロジェクトに含まれる展開する必要がされ、その**ビルド アクション**プロパティに設定する必要があります**コンテンツ**です。 詳細については、次を参照してください。**理由しないすべてのプロジェクト フォルダーにファイルのデプロイしますか?**で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/en-us/library/ee942158.aspx))。 ただし、Web Deploy いないフォルダーが作成されます移行先サイトにコピーするには、少なくとも 1 つのファイルがない限り、します。 したがってに追加、 *.txt*ファイルをフォルダーがコピーできるように、プレース ホルダーとして機能するフォルダーです。

**ソリューション エクスプ ローラー**を右クリックし、 *Elmah*フォルダーを選択**新しい項目の追加**、という名前のファイルを作成および*Placeholder.txt*です。 次のテキストを挿入します"フォルダーが配置を取得することを確認するプレース ホルダー ファイルです。"。 ファイルを保存してください。 に、Visual Studio でこのファイルとには、フォルダーを配置するかどうかを確認するために必要です、**ビルド アクション**プロパティ*.txt*に設定されているファイル**コンテンツ**既定です。

展開セットアップ タスクをすべて完了しました。 チュートリアルでは、[次へ]、Contoso 大学サイトをテスト環境に展開し、テストします。

>[!div class="step-by-step"]
[前へ](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
[次へ](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)
