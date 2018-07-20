---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Visual Studio を使用して ASP.NET Web 展開: 余分なファイルの展開 |Microsoft Docs'
author: tdykstra
description: このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを使用して、.
ms.author: aspnetcontent
ms.date: 03/23/2015
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 609c0be968e8f38d7be6e5f5c96a569a9a35c2eb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 07/05/2018
ms.locfileid: "37820851"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Visual Studio を使用して ASP.NET Web 展開: Extra Files のデプロイ
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> このチュートリアル シリーズは、展開する方法を示します (発行) ASP.NET web アプリケーションを Azure App Service Web Apps、またはサード パーティのホスティング プロバイダーを Visual Studio 2012 または Visual Studio 2010 を使用しています。 系列の詳細については、次を参照してください。[シリーズの最初のチュートリアル](introduction.md)します。


## <a name="overview"></a>概要

このチュートリアルでは、web の発行、Visual Studio の展開中に、追加のタスクを実行するパイプラインを拡張する方法を示します。 タスクでは、web サイトにプロジェクト フォルダーに含まれていない余分なファイルをコピーします。

このチュートリアルでは、1 つの余分なファイルをコピーします: *robots.txt*します。 運用環境ではなく、ステージング環境には、このファイルを展開するには。 [を運用環境に展開する](deploying-to-production.md)チュートリアル プロジェクトにこのファイルを追加して、運用環境を構成する発行プロファイルを除外します。 このチュートリアルでは、この場合は、すべてのファイルを展開するが、プロジェクトに含めるしたくないで役に立つ 1 つを処理するために別の方法を確認します。

## <a name="move-the-robotstxt-file"></a>Robots.txt ファイルを移動します。

処理のさまざまなメソッドを準備する*robots.txt*、チュートリアルのこのセクションでは、プロジェクトには含まれていないフォルダーにファイルを移動して、削除する*robots.txt*ステージング環境から環境。 ファイルをその環境に展開する新しいメソッドが正しく動作していることを確認できるようにをステージングからファイルを削除する必要があります。

1. **ソリューション エクスプ ローラー**を右クリックし、 *robots.txt*ファイルし、クリックして**プロジェクトから除外**します。
2. Windows エクスプ ローラーを使用して、ソリューション フォルダーに新しいフォルダーを作成し、名前*ExtraFiles*します。
3. 移動、 *robots.txt*ファイルから、 *ContosoUniversity*プロジェクト フォルダーに、 *ExtraFiles*フォルダー。

    ![ExtraFiles フォルダー](deploying-extra-files/_static/image1.png)
4. 削除、FTP ツールを使用して、 *robots.txt*ステージング web サイトからのファイル。

    代わりに、選択することができます**転送先に追加のファイルを削除****ファイル発行オプション**上、**設定**ステージング環境の発行プロファイルのタブとステージング環境に再発行します。

## <a name="update-the-publish-profile-file"></a>発行プロファイル ファイルを更新します。

必要なだけ*robots.txt*のため、デプロイするために更新する必要がある唯一の発行プロファイルのステージング、ステージングします。

1. Visual Studio で開く*Staging.pubxml*します。
2. 終了する前に、ファイルの最後に`</Project>`タグは、次のマークアップを追加します。

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    このコードを作成する新しい*ターゲット*を配置する追加のファイルが収集されます。 ターゲットは 1 つまたは指定した条件に基づいて、MSBuild を実行するタスク。

    `Include`属性を指定、ファイルを検索するためのフォルダーがある*ExtraFiles*プロジェクト フォルダーと同じレベルにあります。 MSBuild では、(二重のアスタリスクでは、再帰サブフォルダーを指定します) のすべてのサブフォルダーから再帰的にそのフォルダーからすべてのファイルを収集します。 このコード内に配置複数のファイル、およびファイル内のサブフォルダー、 *ExtraFiles*フォルダー、およびそのすべてが配置されます。

    `DestinationRelativePath`要素は、フォルダーとファイルがで見つかった場合は、先の web サイトで同じファイルおよびフォルダー構造のルート フォルダーにコピーする必要がありますを指定します、 *ExtraFiles*フォルダー。 コピーする場合は、 *ExtraFiles*自体には、フォルダー、`DestinationRelativePath`値になります。 *ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)* します。
3. 終了する前に、ファイルの最後に`</Project>`タグは、新しいターゲットを実行するかを指定する次のマークアップを追加します。

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    このコードにより、新しい`CustomCollectFiles`先フォルダーにファイルをコピーするターゲットが実行されるたびに実行されるターゲット。 別のターゲットが展開パッケージの作成と発行し、発行ではなく、展開パッケージを使用してデプロイする場合、両方のターゲットで新しいターゲットが挿入されます。

    *.Pubxml*ファイルは次の例のようになります。

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. 保存して閉じます、 *Staging.pubxml*ファイル。

## <a name="publish-to-staging"></a>ステージングへの公開します。

1 回のクリックを使用して公開またはコマンド ラインでステージング環境のプロファイルを使用して、アプリケーションを発行します。

1 回のクリックを使用する場合は、次の発行に確認できる、**プレビュー**ウィンドウを*robots.txt*がコピーされます。 それ以外の場合、FTP ツールを使用することを確認します、 *robots.txt*ファイルがデプロイ後に、web サイトのルート フォルダーにします。

## <a name="summary"></a>まとめ

これは、この一連のサード パーティのホスティング プロバイダーへの ASP.NET web アプリケーションを展開する方法のチュートリアルを完了します。 これらのチュートリアルで取り上げるトピックのいずれかの詳細については、次を参照してください。、 [ASP.NET 配置コンテンツ マップ](https://go.microsoft.com/fwlink/p/?LinkId=282413)します。

## <a name="more-information"></a>詳細情報

MSBuild ファイルを操作する方法がわかっている場合コードを記述して他の多くの展開タスクを自動化できます *.pubxml* (プロファイルの特定のタスク) 用のファイルまたはプロジェクト *. wpp.targets*ファイル (のタスク適用するすべてのプロファイル)。 詳細については *.pubxml*と *. wpp.targets*ファイルを参照してください[方法: 発行プロファイル (.pubxml) ファイルでの展開設定の編集、および wpp.targets Visual Studio Web 内のファイル。プロジェクト](https://msdn.microsoft.com/library/ff398069)します。 MSBuild のコードに基本的な概要については、次を参照してください。**プロジェクト ファイルの構造、** で[エンタープライズ展開シリーズ: プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)します。 独自のシナリオのタスクを実行する MSBuild ファイルで作業する方法については、この書籍を参照してください。:[内で、Microsoft Build Engine: Using MSBuild and Team Foundation ビルド](http://msbuildbook.com)Sayed Ibraham Hashimi、William Bartholomew します。

## <a name="acknowledgements"></a>謝辞

このチュートリアル シリーズのコンテンツに多大な貢献を行った以下の方々 に感謝したいと思います。

- [Alberto Poblacion、MVP &amp; MCT、スペイン](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson、データ プラットフォームの開発の MVP、United States
- Harsh Mittal、Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson、Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 教皇、Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava、Microsoft
- [Raffaele Rialdi (イタリア)](http://www.iamraf.net/)
- [Rick Anderson、Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott Hunter、Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [前へ](command-line-deployment.md)
> [次へ](troubleshooting.md)
