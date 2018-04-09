---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
title: 'Visual Studio を使用した ASP.NET Web 展開: 余分なファイルの展開 |Microsoft ドキュメント'
author: tdykstra
description: この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにしています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 1cd91055-84bc-42c6-9d80-646f41429d4d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-extra-files
msc.type: authoredcontent
ms.openlocfilehash: 5cefcedde7715844a7d7a9db1455193564ef9805
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-extra-files"></a>Visual Studio を使用した ASP.NET Web 展開: 余分なファイルの展開
====================
によって[Tom Dykstra](https://github.com/tdykstra)

[スタート プロジェクトをダウンロードします。](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> この一連のチュートリアルについては、展開する方法を示します (ASP.NET の発行) Visual Studio 2012 または Visual Studio 2010 を使用して web アプリケーションを Azure App Service Web Apps またはサード パーティのホスティング プロバイダーにします。 系列の詳細については、次を参照してください。[系列内の最初のチュートリアル](introduction.md)です。


## <a name="overview"></a>概要

このチュートリアルでは、web の発行、Visual Studio の展開時に、追加のタスクを実行するためのパイプラインを拡張する方法を示します。 タスクでは、プロジェクト フォルダーに移動先の web サイトに含まれていない余分なファイルをコピーします。

このチュートリアルを 1 つの余分なファイルをコピーします: *robots.txt*です。 実稼働環境ではなく、ステージングには、このファイルを展開するには。 [実稼働環境に展開する](deploying-to-production.md)チュートリアル プロジェクトにこのファイルを追加し、実稼働環境を構成する発行プロファイルを除外します。 このチュートリアルでは、この状況は、いずれかのファイルを配置するプロジェクトに含めるようにすることが役に立つに対処する代替方法が表示されます。

## <a name="move-the-robotstxt-file"></a>Robots.txt ファイルを移動します。

別の方法で処理を準備する*robots.txt*、チュートリアルのこのセクションで、プロジェクトに含まれていないフォルダーにファイルを移動して、削除する*robots.txt*ステージングから環境。 ファイルをその環境に展開する新しいメソッドが正常に機能していることを検証できるようにステージングからファイルを削除する必要があります。

1. **ソリューション エクスプ ローラー**を右クリックし、 *robots.txt*ファイルし、をクリックして**プロジェクトから除外**です。
2. Windows エクスプ ローラーを使用して、ソリューション フォルダーに新しいフォルダーを作成し、名前*ExtraFiles*です。
3. 移動、 *robots.txt*ファイルから、 *ContosoUniversity*プロジェクト フォルダーに、 *ExtraFiles*フォルダーです。

    ![ExtraFiles フォルダー](deploying-extra-files/_static/image1.png)
4. 削除、FTP ツールを使用して、 *robots.txt*ステージング web サイトからのファイルです。

    代わりを選択できます**先で追加ファイルを削除する****ファイルの発行オプション**上、**設定**ステージング発行プロファイルのタブとステージングに再発行します。

## <a name="update-the-publish-profile-file"></a>発行プロファイル ファイルを更新します。

必要なだけ*robots.txt* 、ステージング環境のため、それを展開するために更新する必要が唯一の発行プロファイルがステージングされます。

1. Visual Studio で開く*Staging.pubxml*です。
2. 終了する前に、ファイルの最後に`</Project>`タグは、次のマークアップを追加します。

    [!code-xml[Main](deploying-extra-files/samples/sample1.xml)]

    このコードでは、新しい*ターゲット*を配置する追加のファイルが収集されます。 ターゲットは 1 つまたは指定した条件に基づいて、MSBuild を実行するタスクの詳細。

    `Include`属性を指定、ファイルを検索するためのフォルダーは、ある*ExtraFiles*プロジェクト フォルダーと同じレベルに配置されている。 MSBuild (2 つのアスタリスクを指定するサブフォルダーを再帰的な) すべてのサブフォルダーからを再帰的にそのフォルダーからすべてのファイルが収集されます。 このコードを持つでした配置する複数のファイル、およびファイル内のサブフォルダーに、 *ExtraFiles*フォルダー、およびそのすべてが展開されます。

    `DestinationRelativePath`要素は、ファイルとフォルダーがで見つかった場合は、移動先 web サイトの同じファイルおよびフォルダー構造内のルート フォルダーにコピーする必要がありますを指定します、 *ExtraFiles*フォルダーです。 コピーする場合は、 *ExtraFiles*フォルダー自体、`DestinationRelativePath`値になります*ExtraFiles\%(RecursiveDir)%(Filename)%(Extension)*です。
3. 終了する前に、ファイルの最後に`</Project>`タグは、新しいターゲットを実行するかを指定する次のマークアップを追加します。

    [!code-xml[Main](deploying-extra-files/samples/sample2.xml)]

    このコードにより、新しい`CustomCollectFiles`先フォルダーにファイルをコピーするターゲットが実行されるたびに実行するターゲット。 別のターゲットが展開パッケージの作成と発行し、発行ではなく、展開パッケージを使用して、展開を決定する場合に、新しいターゲットが両方のターゲットに挿入されます。

    *.Pubxml*ファイルは次の例のようになります。

    [!code-xml[Main](deploying-extra-files/samples/sample3.xml?highlight=53-71)]
4. 保存して閉じます、 *Staging.pubxml*ファイル。

## <a name="publish-to-staging"></a>ステージングへの公開します。

1 回のクリックを使用して発行またはコマンド ラインで、ステージングのプロファイルを使用して、アプリケーションを発行します。

1 回のクリックを使用する場合は、次の発行のことを確認することができます、**プレビュー**ウィンドウを*robots.txt*コピーされます。 それ以外の場合、FTP ツールを使用していることを確認、 *robots.txt*ファイルが、web サイトのルート フォルダーに配置します。

## <a name="summary"></a>まとめ

これは、この一連のサード パーティのホスティング プロバイダーへの ASP.NET web アプリケーションの配置に関するチュートリアルを完了します。 これらのチュートリアルで説明されているトピックのいずれかの詳細については、次を参照してください。、 [ASP.NET 展開のコンテンツ マップ](https://go.microsoft.com/fwlink/p/?LinkId=282413)です。

## <a name="more-information"></a>詳細情報

MSBuild ファイルを操作する方法がわかっている場合でコードを記述して他の展開タスクの多くを自動化できます*.pubxml* (プロファイルに固有のタスク) のファイルまたはプロジェクト*. wpp.targets*ファイル (のタスクプロファイルに適用されるすべて)。 詳細については*.pubxml*と*. wpp.targets*ファイルを参照してください[する方法: 発行プロファイル (.pubxml) ファイルでの展開設定の編集、および wpp.targets Visual Studio Web 内のファイル。プロジェクト](https://msdn.microsoft.com/library/ff398069)です。 MSBuild のコードに基本的な概要については、次を参照してください。**プロジェクト ファイルの構造**で[エンタープライズ展開シリーズ: プロジェクト ファイルを理解する](../web-deployment-in-the-enterprise/understanding-the-project-file.md)です。 タスク、独自のシナリオを実行する MSBuild ファイルを操作する方法については、このブックを参照してください。:[内の Microsoft Build Engine: MSBuild を使用して、Team Foundation ビルド](http://msbuildbook.com)Sayed Ibraham Hashimi して William Bartholomew です。

## <a name="acknowledgements"></a>謝辞

このチュートリアル シリーズのコンテンツに多大な協力者次の方々 に感謝したいと思います。

- [Alberto Poblacion、MVP &amp; MCT、スペイン](https://mvp.microsoft.com/mvp/Alberto%20Poblacion%20Bolano-36772)
- Jarod Ferguson、データ プラットフォームの開発 MVP、United States
- 過酷 Mittal、Microsoft
- [Jon Galloway](https://weblogs.asp.net/jgalloway) (twitter: [ @jongalloway ](http://twitter.com/jongalloway))
- [Kristina Olson、Microsoft](https://blogs.iis.net/krolson/default.aspx)
- [Mike 教皇、Microsoft](http://www.mikepope.com/blog/DisplayBlog.aspx)
- Mohit Srivastava、Microsoft
- [Raffaele Rialdi (イタリア)](http://www.iamraf.net/)
- [Rick Anderson、Microsoft](https://blogs.msdn.com/b/rickandy/)
- [Sayed Hashimi、Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))
- [Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))
- [Scott ハンター、Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))
- [Srđan Božović, Serbia](http://msforge.net/blogs/zmajcek/)
- [Vishal Joshi、Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))

> [!div class="step-by-step"]
> [前へ](command-line-deployment.md)
> [次へ](troubleshooting.md)
