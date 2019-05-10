---
title: ASP.NET Core の gRPC の概要
author: juntaoluo
description: Kestrel サーバーと ASP.NET Core の gRPC サービスについて説明します。
monikerRange: '>= aspnetcore-3.0'
ms.author: johluo
ms.date: 02/26/2019
uid: grpc/index
ms.openlocfilehash: dd1c42744bfda965df91ea1fcc0b71814317b969
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085559"
---
# <a name="introduction-to-grpc-on-aspnet-core"></a>ASP.NET Core の gRPC の概要

作成者: [John Luo](https://github.com/juntaoluo)

[gRPC](https://grpc.io/docs/guides/) は言語に依存しない高性能なリモート プロシージャ コール (RPC) フレームワークです。 gRPC の基礎については、[gRPC ドキュメント ページ](https://grpc.io/docs/)を参照してください。

gRPC の主な利点:
* 最新の高性能軽量 RPC フレームワーク。
* 既定でプロトコル バッファーを使用する契約優先の API 開発。言語に依存しない実装を可能にします。
* 厳密に型指定されたサーバーとクライアントを生成する目的で、さまざまな言語で利用できるツール。
* クライアント、サーバー、双方向ストリーミング呼び出しをサポートします。
* Protobuf バイナリ シリアル化でネットワークの使用率を減らします。

以上の利点から gRPC は以下に最適です。
* 効率性が重要となる軽量のマイクロサービス。
* 開発に複数の言語が必要になる多言語システム。
* ストリーミングの要求または応答を処理する必要があるポイントツーポイントのリアルタイム サービス。

公式の [gRPC ページ](https://grpc.io/docs/quickstart/csharp.html)では現在、C# 実装を利用できますが、現在の実装は C で記述されたネイティブ ライブラリに依存しています (gRPC [C-core](https://grpc.io/blog/grpc-stacks))。 Kestrel HTTP サーバーは完全管理の ASP.NET Core スタックを基盤とする新しい実装を提供するための作業が現在進行中です。 次のドキュメントは、この新しい実装で gRPC サービスを構築するための概要を提供します。

## <a name="additional-resources"></a>その他の技術情報

* <xref:grpc/basics>
* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/aspnetcore>
* <xref:grpc/migration>