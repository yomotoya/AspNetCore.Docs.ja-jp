---
uid: web-forms/overview/deployment/visual-studio-web-deployment/project-properties
title: 'Visual Studio を使用した ASP.NET Web 展開: プロジェクト プロパティ |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ec1cec4c-a75f-47af-a2ba-b1e2f971d24b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/project-properties
msc.type: authoredcontent
ms.openlocfilehash: fba3f089bf1693eec873b08b4bc50e3accba06ee
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/08/2018
ms.locfileid: "30879882"
---
<a name="aspnet-web-deployment-using-visual-studio-project-properties"></a>Visual Studio を使用した ASP.NET Web 展開: プロジェクトのプロパティ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

一部の展開オプションは、プロジェクト ファイルに格納されているプロジェクトのプロパティで構成されます (、 *.csproj*または *.vbproj*ファイル)。 ほとんどの場合、これらの設定の既定値は目的の動作が使用できる、**プロジェクト プロパティ**UI は、それらを変更する必要がある場合、これらの設定を使用する Visual Studio に組み込まれています。 このチュートリアルでは、展開設定を確認する**プロジェクト プロパティ**です。 空のフォルダーを展開するを原因となったプレース ホルダー ファイルを作成します。

## <a name="configure-deployment-settings-in-the-project-properties-window"></a>プロジェクトのプロパティ ウィンドウで展開の設定を構成します。

展開の実行時の動作に影響する設定のほとんどは、次のチュートリアルで後ほど、発行プロファイルに含まれます。 内の対応する必要がありますをいくつかの設定にある、**パッケージ化/発行**のタブ、**プロジェクト プロパティ**ウィンドウです。 これらの設定は各ビルド構成の指定-デバッグ ビルドである場合よりも、リリース ビルドの別の設定を持つことができます、します。

**ソリューション エクスプ ローラー**を右クリックし、 **ContosoUniversity**プロジェクトで、**プロパティ**、クリックして、**パッケージ化/発行 Web**タブです。

![[パッケージ/Web の発行] タブ](project-properties/_static/image1.png)

ウィンドウが表示されたらのいずれかのビルド構成は、ソリューションの現在アクティブな設定が表示された既定になります。 場合、**構成**ボックスは示しません**アクティブ (リリース)****リリース**リリース ビルド構成の設定を表示するためにします。 リリース ビルド、テストと実稼働環境を展開します。

![リリース ビルドの構成の選択](project-properties/_static/image2.png)

**アクティブ (リリース)** または**リリース**リリース ビルド構成を使用して展開するときに有効な値を選択すると、表示します。

- **を展開する項目**ボックスで、**アプリケーションの実行に必要なファイルのみ**が選択されています。 その他のオプションは**このプロジェクト内のすべてのファイル**または**このプロジェクト フォルダー内のすべてのファイル**です。 既定の選択内容を変更することではたとえば、ソース コード ファイルを配置しないでください。 この設定は、SQL Server Compact のバイナリ ファイルを含むフォルダーをプロジェクトに含める必要がある理由理由です。 この設定の詳細については、次を参照してください。**理由しないすべてのプロジェクト フォルダーにファイルのデプロイしますか?** で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/library/ee942158.aspx)です。
- **デバッグ シンボルの生成された除外**が選択されています。 このビルド構成を使用する場合にデバッグはありません。
- **SQL のパッケージ化/発行 タブで構成されているすべてのデータベースを含める**が選択されています。 Visual Studio でのデータベースだけでなく、ファイルを展開するかどうかを指定します。 チェック ボックス ラベルだけ記載ですが、**パッケージ化/発行 SQL**  タブで、このチェック ボックスをオフにするとも無効になりますが、発行プロファイルで構成されているデータベースの配置。 実行する予定を後で、ため、チェック ボックスが選択されたまま必要があります。 **パッケージ化/発行 SQL**  タブはこれらのチュートリアルを使用しないメソッドを公開する従来のデータベースを使用します。
- **Web 展開パッケージの設定**1 回のクリックを使用しているために、セクションでは適用されませんこれらのチュートリアルで公開します。

変更、**構成**デバッグ ビルドの場合、既定の設定を表示するデバッグする ボックスの一覧です。 値は、同じ値を除く、**生成されたデバッグ シンボルを除外する**がクリアできるように、デバッグ ビルドを配置する場合は、デバッグを行うことができます。

## <a name="make-sure-that-the-elmah-folder-gets-deployed"></a>Elmah フォルダーを取得展開されていることを確認してください。

前のチュートリアルで説明したとおり、 [Elmah NuGet パッケージ](http://www.hanselman.com/blog/NuGetPackageOfTheWeek7ELMAHErrorLoggingModulesAndHandlersWithSQLServerCompact.aspx)エラー ログとレポートの機能を提供します。 Contoso 大学アプリケーションで Elmah が構成されているという名前のフォルダーにエラーの詳細を格納する*Elmah*:

![Elmah フォルダー](project-properties/_static/image3.png)

一般的な要件は、展開からの特定のファイルまたはフォルダーの除外別の例には、ユーザーがファイルをアップロードできるフォルダーがあります。 ログ ファイルをしたくないか、実稼働環境に展開する開発環境で作成されたファイルをアップロードします。 あり、展開プロセスを実稼働環境に存在するファイルを削除しない更新プログラムを実稼働環境に配置する場合。 (設定に応じて、配置オプションを展開するときに、移行先サイトがソース サイトではなく、ファイルが存在する場合、Web 配置から削除、変換先です。)

このチュートリアルで既に説明したとおり、**を展開する項目**オプション、**パッケージ化/発行 Web**に設定されているタブ**のみこのアプリケーションの実行に必要なファイル**です。 その結果、開発で Elmah によって作成されるログ ファイルは配置されません、これは、発生します。 (プロジェクトに含まれる展開する必要がされ、その**ビルド アクション**プロパティに設定する必要があります**コンテンツ**です。 詳細については、次を参照してください。**理由しないすべてのプロジェクト フォルダーにファイルのデプロイしますか?** で[ASP.NET Web アプリケーション プロジェクトの展開に関する FAQ](https://msdn.microsoft.com/library/ee942158.aspx))。 ただし、Web Deploy いないフォルダーが作成されます移行先サイトにコピーするには、少なくとも 1 つのファイルがない限り、します。 したがってに追加、 *.txt*ファイルをフォルダーがコピーできるように、プレース ホルダーとして機能するフォルダーです。

**ソリューション エクスプ ローラー**を右クリックし、 *Elmah*フォルダーを選択**新しい項目の追加**、という名前のテキスト ファイルを作成および*Placeholder.txt*です。 次のテキストを挿入します"フォルダーが配置を取得することを確認するプレース ホルダー ファイルです。"。 ファイルを保存してください。 に、Visual Studio でこのファイルとには、フォルダーを配置するかどうかを確認するために必要です、**ビルド アクション**プロパティ *.txt*に設定されているファイル**コンテンツ**既定です。

## <a name="summary"></a>まとめ

展開セットアップ タスクをすべて完了しました。 チュートリアルでは、[次へ]、Contoso 大学サイトをテスト環境に展開し、テストします。

> [!div class="step-by-step"]
> [前へ](web-config-transformations.md)
> [次へ](deploying-to-iis.md)
