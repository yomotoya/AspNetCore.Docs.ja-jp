---
title: ASP.NET Core ディレクトリ構造
author: guardrex
description: 発行された ASP.NET Core アプリのディレクトリ構造について説明します。
manager: wpickett
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: host-and-deploy/directory-structure
ms.openlocfilehash: a5cc1f23d624643facddc9e2006fb246e5ae66dc
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: MT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/07/2018
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core ディレクトリ構造

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core、公開されたアプリケーション ディレクトリで*発行*、アプリケーション ファイル、構成ファイル、静的なアセット、パッケージ、およびランタイムで構成されます (の[自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd))。


| アプリの種類 | ディレクトリ構造 |
| -------- | ------------------- |
| [フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>発行&dagger;<ul><li>ログ&dagger;(標準出力ログを受信するために必要な場合を除き、省略可能)</li><li>ビュー&dagger; (MVC アプリ; ビューをプリコンパイルいない場合)</li><li>ページ&dagger;(MVC または Razor ページ アプリですページをプリコンパイルいない場合)。</li><li>wwwroot&dagger;</li><li>*\.dll ファイル</li><li>\<アセンブリ名 >. deps.json</li><li>\<アセンブリ名 > .dll</li><li>\<アセンブリ名 > .pdb</li><li>\<アセンブリ名 >。PrecompiledViews.dll</li><li>\<アセンブリ名 >。PrecompiledViews.pdb</li><li>\<アセンブリ名 >. runtimeconfig.json</li><li>web.config (IIS 展開)</li></ul></li></ul> |
| [自己完結型の配置](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>発行&dagger;<ul><li>ログ&dagger;(標準出力ログを受信するために必要な場合を除き、省略可能)</li><li>refs&dagger;</li><li>ビュー&dagger; (MVC アプリ; ビューをプリコンパイルいない場合)</li><li>ページ&dagger;(MVC または Razor ページ アプリですページをプリコンパイルいない場合)。</li><li>wwwroot&dagger;</li><li>\*.dll ファイル</li><li>\<アセンブリ名 >. deps.json</li><li>\<アセンブリ名 > .exe</li><li>\<アセンブリ名 > .pdb</li><li>\<アセンブリ名 >。PrecompiledViews.dll</li><li>\<アセンブリ名 >。PrecompiledViews.pdb</li><li>\<アセンブリ名 >. runtimeconfig.json</li><li>web.config (IIS 展開)</li></ul></li></ul> |

&dagger;ディレクトリを示します

*発行*ディレクトリを表します、*コンテンツのルート パス*もという、*アプリケーション ベース パス*デプロイのです。 任意の名前が指定された、*発行*サーバーに配置されたアプリのディレクトリの場所がホスト型アプリへのサーバーの物理パスとして機能します。

*Wwwroot*ディレクトリに存在する場合のみが含まれる静的な資産です。

Stdout*ログ*次の 2 つの方法のいずれかを使用して、展開のディレクトリを作成できます。

* 次の追加`<Target>`要素をプロジェクト ファイル。

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

* 物理的な作成、*ログ*ディレクトリ展開内のサーバーにします。

配置ディレクトリには、読み取り/実行アクセス許可が必要です。 *ログ*ディレクトリが読み取り/書き込み権限が必要です。 ファイルが書き込まれる追加のディレクトリでは、読み取り/書き込み権限が必要です。
