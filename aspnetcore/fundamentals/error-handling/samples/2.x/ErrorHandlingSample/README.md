---
ms.openlocfilehash: b1f7c306533e70f84e73a020c74756b91cae7ea7
ms.sourcegitcommit: 191d21c1e37b56f0df0187e795d9a56388bbf4c7
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 03/08/2019
ms.locfileid: "57665752"
---
# <a name="error-handling-sample-application"></a>エラー処理のサンプル アプリケーション

このサンプル アプリでは、「[ASP.NET Core のエラーを処理する](https://docs.microsoft.com/aspnet/core/fundamentals/error-handling)」のトピックで説明されているシナリオを示します。

## <a name="configure-a-custom-exception-handling-page"></a>カスタム例外処理ページを構成する

アプリを開発環境で実行していない場合は、例外処理ミドルウェア:

* 例外をキャッチします。
* 例外をログに記録します。
* 指定したパスの別のパイプラインで要求を再実行します。

## <a name="configure-custom-exception-handling-code"></a>カスタム例外処理コードを構成する

カスタム例外処理ページを使ってエラー用にエンドポイントを提供する方法の代替手段は、`UseExceptionHandler` にラムダを指定することです。 `UseExceptionHandler` と共にラムダを使うと、応答を返す前にエラーにアクセスできます。

サンプル アプリは、`Startup.Configure` のカスタム例外処理コードを示しています。 *Startup.cs* ファイル (`LambdaErrorHandler`) の上部にある手順に従います。 アプリを起動した後、Index ページの **[例外のスロー]** リンクを使って例外をトリガーします。
