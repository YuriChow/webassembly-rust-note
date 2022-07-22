# 缩小 `.wasm` 尺寸
对于我们通过网络发送给客户端的 `.wasm` 二进制文件，例如我们的 Game of Life Web应用程序，我们希望关注代码大小。我们的 `.wasm` 尺寸越小，我们的页面加载速度越快，我们的用户就越高兴。

## 通过构建配置，我们的 Game of Life `.wasm` 能有多小？
[花点时间回顾一下构建配置选项，我们可以调整这些选项 获得更小的 `.wasm` 代码](https://rustwasm.github.io/docs/book/reference/code-size.html#optimizing-builds-for-code-size)。      
通过默认发布构建配置选项(没有 debug 符号)，我们的 WebAssembly 二进制是 29,410 字节(译者：我的是 26,759，有点不同很正常，毕竟不是 100% 相同的代码编写)：
>wc -c pkg/wasm_game_of_life_bg.wasm

在开启 LTO 之后，设定 `opt-level = "z"`，然后运行 `wasm-opt -Oz`，将会使 `.wasm` 文件缩小到只有 17,317 字节。      
然后我们使用 `gzip` 压缩(几乎每个 HTTP 服务器都会这样做)，我们将其大小再次缩小到 9,045 字节：
>gzip -9 < pkg/wasm_game_of_life_bg.wasm | wc -c