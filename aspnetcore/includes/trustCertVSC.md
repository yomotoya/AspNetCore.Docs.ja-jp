---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472311"
---
*  次のコマンドを実行し、HTTPS 開発証明書を信頼します。

    ```console
    dotnet dev-certs https --trust
    ```

    上記のコマンドでは、次のダイアログが表示されます。

    ![セキュリティ警告のダイアログ](~/getting-started/_static/cert.png)

*    開発証明書を信頼することに同意する場合は、**[はい]** を選択します。

     詳細については、[ASP.NET Core HTTPS 開発証明書の信頼](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)に関する記事をご覧ください