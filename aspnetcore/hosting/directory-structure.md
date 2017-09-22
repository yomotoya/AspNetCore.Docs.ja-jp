---
title: "ASP.NET Core ディレクトリ構造"
author: guardrex
description: "発行済みの ASP.NET Core アプリケーションのディレクトリ構造です。"
keywords: "ASP.NET Core、ディレクトリ構造"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 9ec635b596520e3d19f4040758dd59c1cbca97f4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 09/22/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>発行された ASP.NET Core アプリケーションのディレクトリ構造

によって[Luke Latham](https://github.com/GuardRex)

ASP.NET Core、アプリケーションのディレクトリで*発行*、アプリケーション ファイル、構成ファイル、静的なアセット、パッケージ、および (自己完結型のアプリ) のランタイムで構成されます。 これは、以前のバージョンの ASP.NET、web のルート ディレクトリ内全体のアプリケーションが存在する場所と同じディレクトリ構造です。

| アプリの種類 | ディレクトリ構造 |
| --- | --- |
| フレームワークに依存する展開 | <ul><li>発行\*<ul><li>ログ\*(publishOptions に含まれる) の場合</li><li>refs\*</li><li>ランタイム\*</li><li>ビュー\* (publishOptions に含まれる) の場合</li><li>wwwroot\* (publishOptions に含まれる) の場合</li><li>.dll ファイル</li><li>myapp.deps.json</li><li>myapp.dll</li><li>myapp.pdb</li><li>myapp です。PrecompiledViews.dll (Razor ビューをプリコンパイルする) 場合</li><li>myapp です。PrecompiledViews.pdb (Razor ビューをプリコンパイルする) 場合</li><li>myapp.runtimeconfig.json</li><li>(publishOptions に含まれる) 場合、web.config</li></ul></li></ul> |
| 自己完結型の配置 | <ul><li>発行\*<ul><li>ログ\*(publishOptions に含まれる) の場合</li><li>refs\*</li><li>ビュー\* (publishOptions に含まれる) の場合</li><li>wwwroot\* (publishOptions に含まれる) の場合</li><li>.dll ファイル</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>myapp です。PrecompiledViews.dll (Razor ビューをプリコンパイルする) 場合</li><li>myapp です。PrecompiledViews.pdb (Razor ビューをプリコンパイルする) 場合</li><li>myapp.runtimeconfig.json</li><li>(publishOptions に含まれる) 場合、web.config</li></ul></li></ul> |
\*ディレクトリを示します

内容、*発行*ディレクトリを表します、*コンテンツのルート パス*もという、*アプリケーション ベース パス*デプロイのです。 任意の名前が指定された、*発行*展開内のディレクトリ、その場所が、ホストされるアプリケーションへのサーバーの物理パスとして機能します。 *Wwwroot*ディレクトリに存在する場合のみが含まれる静的な資産です。 *ログ*ディレクトリは、プロジェクトの作成と追加することによって、展開に含めることは、`<Target>`要素を次に示す、 *.csproj*ファイルまたはディレクトリを物理的に上に作成して、サーバー。

```xml
<Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
  <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
  <MakeDir Directories="$(PublishUrl)Logs" Condition="!Exists('$(PublishUrl)Logs')" />
</Target>
```

最初の`<MakeDir>`要素を使用して、 `PublishDir` .NET Core CLI を使用してパブリッシュ操作のターゲットの場所を決定するプロパティが使用されます。 2 番目`<MakeDir>`要素を使用して、`PublishUrl`プロパティがターゲットの場所を特定する Visual Studio で使用します。 Visual Studio を使用して、`PublishUrl`以外の .NET Core プロジェクトと互換性のためのプロパティです。

配置ディレクトリには、読み取り/実行許可が必要です。 中に、*ログ*ディレクトリが読み取り/書き込み権限が必要です。 資産の書き込み先の追加のディレクトリでは、読み取り/書き込み権限が必要です。
