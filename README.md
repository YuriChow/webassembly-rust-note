# webassembly-rust-note
My note of webassembly-rust. Basically, for now, this is a ***partial*** translation(CN) of ["Rust 🦀 and WebAssembly 🕸"](https://rustwasm.github.io/docs/book/).

## description
本文是阅读时顺便翻译的，用于记录与分享。几乎全部内容是 [Rust 🦀 and WebAssembly 🕸](https://rustwasm.github.io/docs/book/) 的中文译文。但是只是部分翻译，仅对实现部分进行了完整的翻译，测试部分暂时仅翻译了一部分，参考部分没有翻译。请阅读原文获取完整内容。      
If you do not read Chinese, reading [Rust 🦀 and WebAssembly 🕸](https://rustwasm.github.io/docs/book/) would be best, or finding other translations.

章节安排、行文顺序均和原书基本一致。并且，同本笔记配套的还有示例代码，里面也有一点笔记，还掺杂了一些我的测试/修改代码，因此格式有点乱，供参考。

由于水平有限，如果有翻译错误/知识点错误的部分，烦请知会，我会重新审核/更正，学习进步。十分感谢。

## you can
对于笔记文档
- 您可以阅读/下载此笔记
- 您可以引用此笔记内容，但需要注明引用的：原文链接 - [Rust 🦀 and WebAssembly 🕸](https://rustwasm.github.io/docs/book/)，和 [译文链接](https://github.com/YuriChow/webassembly-rust-note)
- 您可以转载此笔记，但需要注明：原文链接 - [Rust 🦀 and WebAssembly 🕸](https://rustwasm.github.io/docs/book/)，和 [译文链接](https://github.com/YuriChow/webassembly-rust-note)，并且明示从此两个链接可以免费获取全文。

## code
从 [模板代码：**rustwasm**/wasm-pack-template](https://github.com/rustwasm/wasm-pack-template) 构建出来的代码。仅用作参考，推荐自己手打。
1. 根目录下运行 `wasm-pack build`；
2. *./www* 下运行 `npm install`；
3. *./www* 下运行 `npm run start`；

## others
### 笔记
此笔记使用 ***Obsidian*** 记录，虽然没有使用几个特性功能 - 99%内容均为基础的 ***Markdown*** 语法，但是为了最好的阅读体验，推荐使用 ***Obsidian*** 打开此文档库。
### 代码
仅用作笔记参考和个人归档，推荐您跟随教程自己练习。      
(文件过多，因此上传压缩包。同时，两个 [模板代码：**rustwasm**/wasm-pack-template](https://github.com/rustwasm/wasm-pack-template) 中的 LICENSE 文件复制暴露在外面了一份。) 
