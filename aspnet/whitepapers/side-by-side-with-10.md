---
uid: whitepapers/side-by-side-with-10
title: .NET Framework 1.0 および 1.1 の ASP.NET サイド バイ サイド実行 |Microsoft ドキュメント
author: rick-anderson
description: このホワイト ペーパーでは、ASP.NET Web アプリケーションを fram のいずれかのバージョンで実行することにより、コンピューターに .NET 1.0 と .NET 1.1 の両方をインストールする方法について説明しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2010
ms.topic: article
ms.assetid: bdea2003-e964-4db5-9092-d56cc7560616
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/side-by-side-with-10
msc.type: content
ms.openlocfilehash: 939b04d440955b184bf5f4c40a2ef8175641edfa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530181"
---
<a name="aspnet-side-by-side-execution-of-net-framework-10-and-11"></a>.NET Framework 1.0 および 1.1 の ASP.NET サイド バイ サイド実行
====================
> このホワイト ペーパーでは、ASP.NET Web アプリケーションをフレームワークのいずれかのバージョンで実行することにより、コンピューターに .NET 1.0 と .NET 1.1 の両方をインストールする方法について説明します。
> 
> ASP.NET 1.0 および 1.1 を ASP.NET に適用されます。


ASP.NET では、アプリケーションは、同じコンピューターにインストールされてものの、異なるバージョンの .NET Framework を使用している場合でサイド バイ サイドで実行されていると言います。 次のトピックでは、サイド バイ サイド実行用の ASP.NET アプリケーションを構成する方法について説明し、詳細な手順を提供します。

- [インストール中に .NET framework version 1.0、Web アプリケーションのマッピングを管理します。](#1)
- [マップを .NET Framework の特定のバージョンの Web アプリケーション](#2)
- [Web サイトを使用している .NET Framework のバージョンを検索します。](#3)

従来、コンポーネントまたはアプリケーションがコンピューターに更新されると、古いバージョンは削除され、新しいバージョンに置き換えられます。 新しいバージョンが、以前のバージョンと互換性ない場合は、通常、コンポーネントやアプリケーションを使用するその他のアプリケーションが破損します。 .NET Framework は、複数のバージョン、アセンブリまたはアプリケーションを同時に同じコンピューターにインストールできるは、サイド バイ サイド実行のためのサポートを提供します。 複数のバージョンを同時にインストールできるため、マネージ アプリケーションは、別のバージョンを使用するアプリケーションに影響を与えずに使用するバージョンを選択できます。

既定では、.NET Framework version 1.1 のインストール中に既存のすべての ASP.NET アプリケーションが自動的に再構成、.NET Framework の最新バージョンを使用します。 既定で .NET Framework 1.1 に、ASP.NET アプリケーションしたくない場合はクリックして[ここ](#1)のインストール時にこれを回避する方法を学習します。

.NET Framework 1.1 に、Web サーバーを更新し、1 つまたは複数の Web アプリケーションを .NET Framework 1.0 を実行すると場合、は、インターネット インフォメーション サービス (IIS) のスクリプト マップを更新する必要があります。 スクリプトのマッピングは、.NET Framework のバージョンを特定の Web アプリケーションの .aspx ファイル拡張子をマップするメカニズムです。 をクリックして[ここ](#2)を .NET Framework の特定のバージョンの Web アプリケーションにマップする方法を学習します。

インターネット情報マネージャーまたは ASP.NET IIS 登録ツールを使用することができます (Aspnet\_regiis.exe) を特定の Web アプリケーションを実行しているどの .NET Framework のバージョンを検索します。 をクリックして[ここ](#3)を Web サイトを使用している .NET Framework のバージョンを検索する方法を参照してください。

1 つのインポート際の考慮事項、.NET Framework の各バージョンが、独自の Machine.config ファイルを使用する .NET Framework 1.1 への移行です。 その結果、Web 管理者が、Machine.config ファイルに変更を加えた場合それらの変更を .NET Framework 1.1 の Machine.config ファイルに移行する必要があります。

<a id="1"></a>

## <a name="maintaining-your-web-applications-mapping-to-net-framework-10-during-installation"></a>インストール中に .NET Framework 1.0 に、Web アプリケーションのマッピングを維持します。

既定では、既存のすべての ASP.NET アプリケーションは、.NET Framework の新しいバージョンを使用するインストール時に自動的に再構成されます。 .NET Framework の新しいバージョンを使用して、アプリケーション フル活用できますの強化機能と、新しいリリースに含まれる新しい機能です。 同時に、Web 管理者は、どのアプリケーションをきめ細かく制御する可能性がありますが更新、自動再マップのすべての既存の ASP.NET アプリケーションの .NET Framework のインストール中に防ぐことができます。

.NET Framework の新しいバージョンに ASP.NET アプリケーション全体の自動再マッピングを防ぐためには、Web 管理者は、Dotnetfx.exe セットアップ プログラムで/noaspupgrade コマンド ライン オプションを使用できます。

**ASP.NET アプリケーションを新しいバージョンの合計の再割り当てを回避するには**

1. 移動して**開始**です。
2. をクリックして**実行**です。
3. 「**cmd**」と入力します。
4. **[OK]** をクリックします。  
  
    ![](side-by-side-with-10/_static/image1.gif)
5. コマンド プロンプトから、.NET Framework のインストールを開始する次の行を入力: **Dotnetfx.exe 展開/c"/noaspupgrade をインストールしますか?** です。  
  
    ![](side-by-side-with-10/_static/image2.gif)
6. をクリックして**はい**Microsoft .NET Framework 1.1 のセットアップにします。 これにより、.NET Framework 1.1 のセットアップ処理が開始されます。  
  
    ![](side-by-side-with-10/_static/image3.gif)

<a id="2"></a>

## <a name="map-a-web-application-to-a-specific-version-of-the-net-framework"></a>マップを .NET Framework の特定のバージョンの Web アプリケーション

.NET Framework のバージョンごとに、ASP.NET IIS 登録ツールのバージョンが含まれています (Aspnet\_regiis.exe)。 このツールは、Web アプリケーションが .NET Framework の特定のバージョンで実行することを指定できます。 これは、.NET Framework のバージョンに Web アプリケーションのマッピングと呼ばれます。 管理者は Aspnet を選択する必要があります\_regiis.exe Web アプリケーションに関連付けられる .NET Framework のバージョンに対応します。 たとえば、Web サイトが .NET Framework 1.1 を使用するように指定したい管理者は Aspnet を使用する必要があります\_regiis.exe .NET Framework 1.1 に付属しています。

Aspnet\_のバージョンが 1.0 regiis.exe は。

- C:\WINDOWS\Microsoft.NET\Framework\**v1.0.3705** \aspnet\_iis 登録ツール

Aspnet\_regiis.exe 1, 1 のバージョンについては。

- C:\WINDOWS\Microsoft.NET\Framework\**v1.1.4322** \aspnet\_iis 登録ツール

Aspnet\_regiis.exe には、スクリプト、Web アプリケーションのマッピングの 2 つのオプションが用意されています。

- **-s**設定スクリプト マップの追加、およびその子パスでディレクトリ。
- **-sn**のみのパスでスクリプト マップを設定します。

パスは、W3SVC/ルートの形式で定義された Web アプリケーション IIS メタデータ パスを定義/{WebSiteNumber}/{アプリケーション\_名}。 たとえば、既定の Web サイトの下にあるポータルと呼ばれる Web アプリケーション、メタベース パスは W3SVC/1/ルート/ポータルです。

![](side-by-side-with-10/_static/image4.gif)

メタベース パスを取得するメタベース エディターと呼ばれるツールを使用することもできますに注意してください。 このツールをダウンロードするには、Microsoft サポート サイトから[https://support.microsoft.com/default.aspx?scid=kb;en-us;232068 です。](https://support.microsoft.com/default.aspx?scid=kb;en-us;232068)

- 実行 Aspnet\_マップとその subapplication regiis.exe-s W3SVC/1/ルート/ポータルを IIS のポータルを更新するスクリプトを作成します。  
  
    ![](side-by-side-with-10/_static/image5.gif)

- Aspnet の実行\_regiis.exe -sn W3SVC/1/ルート/ポータル ポータルの IIS スクリプトを更新するマップのポータルでアプリケーションに影響を与えずに? s サブディレクトリです。  
  
    ![](side-by-side-with-10/_static/image6.gif)

<a id="3"></a>

## <a name="find-the-net-framework-version-that-a-web-application-is-using"></a>Web アプリケーションを使用している .NET Framework のバージョンを検索します。

管理者は、インターネット サービス マネージャーを使用して、Web サイトを実行する .NET Framework のバージョンを検索します。 別のオペレーティング システムのバージョンでは、異なる方法で、インターネット サービス マネージャーを起動します。 サービス マネージャーを起動するには、次の手順に従います。

**インターネット サービス マネージャを起動するには**

1. 移動して**開始**です。
2. をクリックして**実行**です。
3. 型**inetmgr**です。  
  
    ![](side-by-side-with-10/_static/image7.gif)
4. インターネット サービス マネージャーからでは、Web アプリケーションを確認する、.NET Framework のバージョンを選択します。  
  
    ![](side-by-side-with-10/_static/image8.gif)
5. Web アプリケーションを右クリックし、をクリックして**プロパティです。**  
  
    ![](side-by-side-with-10/_static/image9.gif)
6. [プロパティ] ウィンドウで、選択**構成します。**  
  
    ![](side-by-side-with-10/_static/image10.gif)
7. このアプリケーションのマッピング テーブルから選択 **.aspx**、 をクリック**編集**です。  
  
    ![](side-by-side-with-10/_static/image11.gif)
8. **実行可能ファイル**テキスト ボックス、スクロールしてバージョンのディレクトリを確認します。 バージョンのディレクトリが v.1.1.4322 の場合は、アプリケーションが .NET Framework 1.1 にマップされます。 逆に、バージョンのディレクトリが v1.0.3705 の場合は、アプリケーションが .NET Framework 1.0 にマップされます。  
  
    ![](side-by-side-with-10/_static/image12.gif)
