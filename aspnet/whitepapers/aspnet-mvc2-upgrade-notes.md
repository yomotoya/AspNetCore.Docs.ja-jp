---
uid: whitepapers/aspnet-mvc2-upgrade-notes
title: ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションのアップグレード |Microsoft ドキュメント
author: rick-anderson
description: このドキュメントでは、両方について説明し、ウィザードを使用して、手動で ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションをアップグレードする方法です。 このドキュメントも d を使用しています.
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/08/2010
ms.topic: article
ms.assetid: f1a01759-d251-4b09-8835-e112e336c6dd
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /whitepapers/aspnet-mvc2-upgrade-notes
msc.type: content
ms.openlocfilehash: d1227f0738b2d2a396fed942f32ae5e75579596e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530061"
---
<a name="upgrading-an-aspnet-mvc-10-application-to-aspnet-mvc-2"></a>ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションのアップグレード
====================
> このドキュメントでは、両方について説明し、ウィザードを使用して、手動で ASP.NET MVC 2 には、ASP.NET MVC 1.0 アプリケーションをアップグレードする方法です。 このドキュメントも[ダウンロード](https://download.microsoft.com/download/F/1/6/F16F9AF9-8EF4-4845-BC97-639791D5699C/MVC2-Upgrade-Notes.pdf)


## <a name="introduction"></a>はじめに

ASP.NET MVC 2 は、同じサーバー上の ASP.NET MVC 1.0 サイド バイ サイドでインストールできます。 これにより、アプリケーション開発者柔軟に ASP.NET MVC 2 を ASP.NET MVC 1.0 アプリケーションをアップグレードするタイミングを選択します。

Visual Studio 2010 には、ASP.NET MVC 2 を Visual Studio 2008 でビルドされた既存の ASP.NET MVC 1.0 プロジェクトをアップグレードにはウィザードが含まれています。 アップグレード ウィザードは、Visual Studio 2010 の ASP.NET MVC 1.0 プロジェクトを開くことによって開始されます。

## <a name="upgrade-wizard-for-aspnet-mvc-10-on-visual-studio-2008-sp1"></a>Visual Studio 2008 SP1 での ASP.NET MVC 1.0 用のウィザードをアップグレードします。

Visual Studio 2008 SP1 での ASP.NET MVC 2 に ASP.NET MVC 1.0 アプリケーションをアップグレードするには、(サポートされていない) MvcAppConverter アプリケーションを使用します。 このアプリケーションは、次の URL からダウンロードできます。

[https://go.microsoft.com/fwlink/?LinkID=185351](https://go.microsoft.com/fwlink/?LinkID=185351)

## <a name="manually-upgrading-an-aspnet-mvc-10-project"></a>ASP.NET MVC 1.0 プロジェクトを手動でアップグレードします。

バージョン 2 に既存の ASP.NET MVC 1.0 アプリケーションを手動でアップグレードするには、次の手順に従います。

1. 既存のプロジェクトのバックアップを作成します。
2. テキスト エディターで、プロジェクト ファイル (.csproj または .vbproj ファイル拡張子を持つファイル) を開き ProjectTypeGuid 要素を検索します。 、その要素の値として、GUID {603c0e0b-db56-11dc-be95-000d561079b0} {f85e285d-a4e0-4152-9332-ab1d724d3325} を置き換えます。 その要素の値が次のようにする必要があります。 

    `{F85E285D-A4E0-4152-9332-AB1D724D3325};{349c5851-65df-11da-9384-00065b846f21};{fae04ec0-301f-11d3-bf4b-00c04f79efbc}`
3. Web アプリケーションのルート フォルダーには、Web.config ファイルを編集します。 System.Web.Mvc、バージョンの検索 1.0.0.0 を = し、すべてのインスタンスを置き換えます System.Web.Mvc、バージョン 2.0.0.0 を = です。
4. Views フォルダーにある Web.config ファイルを前の手順を繰り返します。
5. Visual Studio を使用してプロジェクトを開くと**ソリューション エクスプ ローラー**、展開、**参照**ノード。 System.Web.Mvc (を version 1.0 のアセンブリを指す) への参照を削除します。 System.Web.Mvc (v2.0.0.0) への参照を追加します。
6. 構成セクションの下のアプリケーション ルートの Web.config ファイルに次の bindingRedirect 要素を追加します。   

    [!code-xml[Main](aspnet-mvc2-upgrade-notes/samples/sample1.xml)]
7. 新しい空の ASP.NET MVC 2 アプリケーションを作成します。 既存のアプリケーションの Scripts フォルダーに新しいアプリケーションの Scripts フォルダーからファイルをコピーします。
8. 更新の既存のアプリケーションの â €™ Site.css ファイルで CSS スタイル定義を含む s CSS ファイル。
9. アプリケーションをコンパイルして実行します。 エラーが発生した場合の重大な変更」セクションを参照してください。、 [ASP.NET MVC 2 の新](https://go.microsoft.com/fwlink/?LinkID=185038)ページ。
