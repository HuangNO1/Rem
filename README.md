[Rem Blog](huangno1.github.io)
===

我用 Hugo 與 Github Pages 做的個人 Blog 靜態網頁，這個 Repository 是沒有用 Hugo 編譯前的源碼。

編輯時有幾個注意點：

- `content/posts/` 裡面放寫的文章，每篇文章裡面有個 `index.md` 作為文章內容，文章資料夾裡 `index/` 裡面放文章會用到的圖片備份，裡面分別有兩個資料夾，`uncompressed/` 放還沒壓縮過得圖片，`compressed/` 放已經壓縮過得圖片，我會將 `compressed/` 的圖片都上傳圖床給文章中引用。
- 每篇文章的 Feature Image 會另外放到 `static/featuredImage/`，一樣分有壓縮跟沒壓縮的圖片，引用有壓縮過的圖片。