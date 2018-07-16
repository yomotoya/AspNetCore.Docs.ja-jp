# <a name="update-the-generated-pages"></a><span data-ttu-id="431a3-101">生成されたページの更新</span><span class="sxs-lookup"><span data-stu-id="431a3-101">Update the generated pages</span></span>

<span data-ttu-id="431a3-102">作成者: [Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="431a3-102">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="431a3-103">ムービー アプリは上々の滑り出しでしたが、表示が理想的ではありません。</span><span class="sxs-lookup"><span data-stu-id="431a3-103">We have a good start to the movie app, but the presentation isn't ideal.</span></span> <span data-ttu-id="431a3-104">時刻の表示が好ましくなく (下の画像の 12:00:00 AM)、**ReleaseDate** は **Release Date** (2 つの単語) にするべきです。</span><span class="sxs-lookup"><span data-stu-id="431a3-104">We don't want to see the time (12:00:00 AM in the image below) and **ReleaseDate** should be **Release Date** (two words).</span></span>

![ムービー データが表示された、Chrome で開かれているムービー アプリケーション](../../tutorials/razor-pages/sql/_static/m55.png)

## <a name="update-the-generated-code"></a><span data-ttu-id="431a3-106">生成されたコードの更新</span><span class="sxs-lookup"><span data-stu-id="431a3-106">Update the generated code</span></span>

<span data-ttu-id="431a3-107">*Models/Movie.cs* ファイルを開き、下のコードで強調表示されている行を追加します。</span><span class="sxs-lookup"><span data-stu-id="431a3-107">Open the *Models/Movie.cs* file and add the highlighted lines shown in the following code:</span></span>

[!code-csharp[](code/Models/Movie.cs?highlight=2,11-12)]
