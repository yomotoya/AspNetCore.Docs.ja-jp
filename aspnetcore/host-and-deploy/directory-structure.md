---
title: ASP.NET Core のディレクトリ構造
author: guardrex
description: 発行された ASP.NET Core アプリのディレクトリ構造について説明します。
monikerRange: '>= aspnetcore-2.2'
ms.author: riande
ms.custom: mvc
ms.date: 12/11/2018
uid: host-and-deploy/directory-structure
ms.openlocfilehash: ee0bebb8b5c688f8471d6420d1641b87ac271f6c
ms.sourcegitcommit: b34b25da2ab68e6495b2460ff570468f16a9bf0d
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 12/12/2018
ms.locfileid: "53284566"
---
# <a name="aspnet-core-directory-structure"></a>ASP.NET Core のディレクトリ構造

作成者: [Luke Latham](https://github.com/guardrex)

*publish* ディレクトリには、[dotnet publish](/dotnet/core/tools/dotnet-publish) コマンドによって生成された、アプリの展開可能な資産が含まれています。 ディレクトリには次のものが含まれます。

* アプリケーション ファイル
* 構成ファイル
* 静的な資産
* パッケージ
* ランタイム ([自己完結型展開](/dotnet/core/deploying/#self-contained-deployments-scd)のみ)

| アプリの種類 | ディレクトリの構造 |
| -------- | ------------------- |
| [フレームワークに依存する展開](/dotnet/core/deploying/#framework-dependent-deployments-fdd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (stdout ログを受け取るために必要な場合を除きオプション)</li><li>Views&dagger; (MVC アプリ、ビューがプリコンパイルされていない場合)</li><li>Pages&dagger; (MVC または Razor ページ アプリ、ページがプリコンパイルされていない場合)</li><li>wwwroot&dagger;</li><li>*\.dll ファイル</li><li>{ASSEMBLY NAME}.deps.json</li><li>{ASSEMBLY NAME}.dll</li><li>{ASSEMBLY NAME}.pdb</li><li>{ASSEMBLY NAME}.Views.dll</li><li>{ASSEMBLY NAME}.Views.pdb</li><li>{ASSEMBLY NAME}.runtimeconfig.json</li><li>web.config (IIS 展開)</li></ul></li></ul> |
| [自己完結型の展開](/dotnet/core/deploying/#self-contained-deployments-scd) | <ul><li>publish&dagger;<ul><li>Logs&dagger; (stdout ログを受け取るために必要な場合を除きオプション)</li><li>Views&dagger; (MVC アプリ、ビューがプリコンパイルされていない場合)</li><li>Pages&dagger; (MVC または Razor ページ アプリ、ページがプリコンパイルされていない場合)</li><li>wwwroot&dagger;</li><li>\*.dll ファイル</li><li>{ASSEMBLY NAME}.deps.json</li><li>{ASSEMBLY NAME}.dll</li><li>{ASSEMBLY NAME}.exe</li><li>{ASSEMBLY NAME}.pdb</li><li>{ASSEMBLY NAME}.Views.dll</li><li>{ASSEMBLY NAME}.Views.pdb</li><li>{ASSEMBLY NAME}.runtimeconfig.json</li><li>web.config (IIS 展開)</li></ul></li></ul> |

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

   `<MakeDir>` 要素は、公開される出力に空の *Logs* フォルダーを作成します。 この要素は、`PublishDir` プロパティを使って、フォルダーを作成するためのターゲットの場所を決定します。 Web 配置などの複数の展開方法は、展開の間に空のフォルダーをスキップします。 `<WriteLinesToFile>` 要素は *Logs* フォルダーにファイルを生成します。これは、サーバーへのフォルダーの展開を保証します。 ワーカー プロセスにターゲット フォルダーへの書き込みアクセス許可がない場合、このアプローチを使用したフォルダーの作成は失敗します。

* 展開内のサーバー上に *Logs* ディレクトリを物理的に作成します。

展開ディレクトリには、読み取り/実行アクセス許可が必要です。 *Logs* ディレクトリには、読み取り/書き込みアクセス許可が必要です。 ファイルが書き込まれる追加のディレクトリには、読み取り/書き込みアクセス許可が必要です。

## <a name="additional-resources"></a>その他の技術情報

* [dotnet publish](/dotnet/core/tools/dotnet-publish)
* [.NET Core アプリケーションの展開](/dotnet/core/deploying/)
* [ターゲット フレームワーク](/dotnet/standard/frameworks)
* [.NET Core の RID カタログ](/dotnet/core/rid-catalog)
