---
title: ASP.NET Core のディレクトリ構造
author: guardrex
description: 発行された ASP.NET Core アプリのディレクトリ構造について説明します。
ms.author: riande
ms.custom: mvc
ms.date: 04/09/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: 8e2693397f826d0e9a36ff52aa1d1d623b31043d
ms.sourcegitcommit: 356c8d394aaf384c834e9c90cabab43bfe36e063
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 06/26/2018
ms.locfileid: "36960828"
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core のディレクトリ構造

作成者: [Luke Latham](https://github.com/guardrex)

ASP.NET Core では、公開されるアプリケーション ディレクトリ *publish* は、アプリケーション ファイル、構成ファイル、静的資産、パッケージ、およびランタイムで構成されます ([自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd)の場合)。


| アプリの種類 | ディレクトリの構造 |
| -------- | ------------------- |
| [フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (stdout ログを受け取るために必要な場合を除きオプション)</li><li>Views&dagger; (MVC アプリ、ビューがプリコンパイルされていない場合)</li><li>Pages&dagger; (MVC または Razor ページ アプリ、ページがプリコンパイルされていない場合)</li><li>wwwroot&dagger;</li><li>*\.dll ファイル</li><li>\<アセンブリ名>.deps.json</li><li>\<アセンブリ名>.dll</li><li>\<アセンブリ名>.pdb</li><li>\<アセンブリ名>.PrecompiledViews.dll</li><li>\<アセンブリ名>.PrecompiledViews.pdb</li><li>\<アセンブリ名>.runtimeconfig.json</li><li>web.config (IIS 展開)</li></ul></li></ul> |
| [自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (stdout ログを受け取るために必要な場合を除きオプション)</li><li>refs&dagger;</li><li>Views&dagger; (MVC アプリ、ビューがプリコンパイルされていない場合)</li><li>Pages&dagger; (MVC または Razor ページ アプリ、ページがプリコンパイルされていない場合)</li><li>wwwroot&dagger;</li><li>\*.dll ファイル</li><li>\<アセンブリ名>.deps.json</li><li>\<アセンブリ名>.exe</li><li>\<アセンブリ名>.pdb</li><li>\<アセンブリ名>.PrecompiledViews.dll</li><li>\<アセンブリ名>.PrecompiledViews.pdb</li><li>\<アセンブリ名>.runtimeconfig.json</li><li>web.config (IIS 展開)</li></ul></li></ul> |

&dagger; ディレクトリを示します

*publish* ディレクトリは、展開の "*コンテンツ ルート パス*" ("*アプリケーション ベース パス*" とも呼ばれます) を表します。 サーバー上で展開されたアプリの *publish* ディレクトリにどのような名前が指定されても、その場所がホストされたアプリへのサーバーの物理パスとして機能します。

*Wwwroot* ディレクトリが存在する場合は、静的資産のみが含まれます。

stdout の *Logs* ディレクトリは、次の 2 つの方法のいずれかを使って展開用に作成できます。

* プロジェクト ファイルに次の `<Target>` 要素を追加します。

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

   `<MakeDir>` 要素は、公開される出力に空の *Logs* フォルダーを作成します。 この要素は、`PublishDir` プロパティを使って、フォルダーを作成するためのターゲットの場所を決定します。 Web 配置などの複数の展開方法は、展開の間に空のフォルダーをスキップします。 `<WriteLinesToFile>` 要素は *Logs* フォルダーにファイルを生成します。これは、サーバーへのフォルダーの展開を保証します。 ワーカー プロセスにターゲット フォルダーへの書き込みアクセス許可がない場合、フォルダーの作成が失敗する可能性があることに注意してください。

* 展開内のサーバー上に *Logs* ディレクトリを物理的に作成します。

展開ディレクトリには、読み取り/実行アクセス許可が必要です。 *Logs* ディレクトリには、読み取り/書き込みアクセス許可が必要です。 ファイルが書き込まれる追加のディレクトリには、読み取り/書き込みアクセス許可が必要です。
