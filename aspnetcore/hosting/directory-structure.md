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
ms.openlocfilehash: 60797bff85a44dd10caad4aabc109ee12dedfe61
ms.sourcegitcommit: 7efdc4b6025ad70c15c26bf7451c3c0411123794
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/02/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>発行された ASP.NET Core アプリケーションのディレクトリ構造

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core、アプリケーションのディレクトリで*発行*、アプリケーション ファイル、構成ファイル、静的なアセット、パッケージ、および (自己完結型のアプリ) のランタイムで構成されます。 これは、以前のバージョンの ASP.NET、web のルート ディレクトリ内全体のアプリケーションが存在する場所と同じディレクトリ構造です。

| アプリの種類 | ディレクトリ構造 |
| --- | --- |
| フレームワークに依存する展開 | <ul><li>発行\*<ul><li>ログ\*(publishOptions に含まれる) の場合</li><li>refs\*</li><li>ランタイム\*</li><li>ビュー\* (publishOptions に含まれる) の場合</li><li>wwwroot\* (publishOptions に含まれる) の場合</li><li>.dll ファイル</li><li>myapp.deps.json</li><li>myapp.dll</li><li>myapp.pdb</li><li>myapp です。PrecompiledViews.dll (Razor ビューをプリコンパイルする) 場合</li><li>myapp です。PrecompiledViews.pdb (Razor ビューをプリコンパイルする) 場合</li><li>myapp.runtimeconfig.json</li><li>(publishOptions に含まれる) 場合、web.config</li></ul></li></ul> |
| 自己完結型の配置 | <ul><li>発行\*<ul><li>ログ\*(publishOptions に含まれる) の場合</li><li>refs\*</li><li>ビュー\* (publishOptions に含まれる) の場合</li><li>wwwroot\* (publishOptions に含まれる) の場合</li><li>.dll ファイル</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>myapp です。PrecompiledViews.dll (Razor ビューをプリコンパイルする) 場合</li><li>myapp です。PrecompiledViews.pdb (Razor ビューをプリコンパイルする) 場合</li><li>myapp.runtimeconfig.json</li><li>(publishOptions に含まれる) 場合、web.config</li></ul></li></ul> |
\*ディレクトリを示します

内容、*発行*ディレクトリを表します、*コンテンツのルート パス*もという、*アプリケーション ベース パス*デプロイのです。 任意の名前が指定された、*発行*展開内のディレクトリ、その場所が、ホストされるアプリケーションへのサーバーの物理パスとして機能します。 *Wwwroot*ディレクトリに存在する場合のみが含まれる静的な資産です。 *ログ*ディレクトリは、プロジェクトの作成と追加することによって、展開に含めることは、`<Target>`要素を次に示す、 *.csproj*ファイルまたはディレクトリを物理的に上に作成して、サーバー。

```xml
<Target Name="CreateLogsFolder" AfterTargets="Publish">
  <MakeDir Directories="$(PublishDir)Logs" 
           Condition="!Exists('$(PublishDir)Logs')" />
  <WriteLinesToFile File="$(PublishDir)Logs\.log" 
                    Lines="Generated file" 
                    Overwrite="True" 
                    Condition="!Exists('$(PublishDir)Logs\.log')" />
</Target>
```

`<MakeDir>`要素は、空を作成*ログ*公開済みの出力内のフォルダーです。 要素を使用して、`PublishDir`フォルダーを作成するため、ターゲットの場所を決定するプロパティです。 Web Deploy などのいくつかの展開方法は、配置時に空のフォルダーをスキップします。 `<WriteLinesToFile>`要素内のファイルを生成する、*ログ*フォルダーで、サーバー フォルダーの展開を保証します。 ワーカー プロセスは、ターゲット フォルダーに書き込みアクセス権を持っていない場合、まだフォルダーの作成が失敗することに注意してください。

配置ディレクトリには、読み取り/実行許可が必要です。 中に、*ログ*ディレクトリが読み取り/書き込み権限が必要です。 資産の書き込み先の追加のディレクトリでは、読み取り/書き込み権限が必要です。
