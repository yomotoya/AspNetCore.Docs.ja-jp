---
ms.openlocfilehash: a9bdff481b1a72a9ee19f4e51fada177530c0cbb
ms.sourcegitcommit: 948e533e02c2a7cb6175ada20b2c9cabb7786d0b
ms.translationtype: HT
ms.contentlocale: ja-JP
ms.lasthandoff: 04/10/2019
ms.locfileid: "59472311"
---
*  <span data-ttu-id="eb009-101">次のコマンドを実行し、HTTPS 開発証明書を信頼します。</span><span class="sxs-lookup"><span data-stu-id="eb009-101">Trust the HTTPS development certificate by running the following command:</span></span>

    ```console
    dotnet dev-certs https --trust
    ```

    <span data-ttu-id="eb009-102">上記のコマンドでは、次のダイアログが表示されます。</span><span class="sxs-lookup"><span data-stu-id="eb009-102">The preceding command displays the following dialog:</span></span>

    ![セキュリティ警告のダイアログ](~/getting-started/_static/cert.png)

*    <span data-ttu-id="eb009-104">開発証明書を信頼することに同意する場合は、**[はい]** を選択します。</span><span class="sxs-lookup"><span data-stu-id="eb009-104">Select **Yes** if you agree to trust the development certificate.</span></span>

     <span data-ttu-id="eb009-105">詳細については、[ASP.NET Core HTTPS 開発証明書の信頼](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos)に関する記事をご覧ください</span><span class="sxs-lookup"><span data-stu-id="eb009-105">See [Trust the ASP.NET Core HTTPS development certificate](xref:security/enforcing-ssl#trust-the-aspnet-core-https-development-certificate-on-windows-and-macos) for more information.</span></span>