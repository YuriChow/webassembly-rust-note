# 发布至 ***npm***
现在，我们有了一个工作、快速、小型的 `wasm-game-of-life` 包，我们可以将其发布到 ***npm***，以便其他 ***JavaScript*** 开发人员可以重用它，如果他们需要现成的 Game of Life 实现的话。

## 先决条件
首先，[确保你有一个 ***npm*** 账户](https://www.npmjs.com/signup) 。      
其次，保证你在本地登录了账户，通过运行这个命令：`wasm-pack login`。

## 发布
1. 保证 `wasm-game-of-life/pkg` 构建是最新的，通过在 *wasm-game-of-life* 目录中运行 `wasm-pack build`。
2. 然后花点时间检查一下 *pkg* 文件夹中的内容，下一步我们就要发布到 ***npm*** 上了。   
3. 当你准备好时，运行 `wasm-pack publish` 来上传包到 ***npm***。      

这就是发布到 ***npm*** 的全部内容！   
...…除了其他人也做了这个教程，因此 *wasm-game-of-life* 在 ***npm*** 上已经是一个被占用的名字了，这时最后一个命令可能没用      
打开 *wasm-game-of-life/Cargo.toml* 并将用户名添加到 `name` 的末尾，以独特的方式消除包的歧义：
```toml
[package]
name = "wasm-game-of-life-my-username"
```
然后，重新发布：
>wasm-pack build
>wasm-pack publish

这回应该有用了！


# 结束语：
本译文，仅节选至原文的 ***Reference***，并没有包括 [***Reference(参考)***，请自行查阅](https://rustwasm.github.io/docs/book/reference/index.html)