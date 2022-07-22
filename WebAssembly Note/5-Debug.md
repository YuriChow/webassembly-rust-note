# Debugging
在我们编写更多的代码之前，我们希望拥有一些调试工具，以备出错时使用。花点时间查看 [参考页，其中列出了可用于调试 ***Rust*** 生成的 WebAssembly 的工具和方法](https://rustwasm.github.io/docs/book/reference/debugging.html)。

## 启用记录 Panics 的日志
[如果我们的代码 panics，我们希望在开发人员控制台中显示信息性的错误消息](https://rustwasm.github.io/docs/book/reference/debugging.html#logging-panics)。      
我们的 `wasm-pack-template` 附带了一个可选的、默认情况下启用的依赖 [the `console_error_panic_hook` crate](https://github.com/rustwasm/console_error_panic_hook)，此 crate 在 *wasm-game-of-life/src/utils.rs* 中配置。我们所需要做的就是在初始化函数或公共代码路径中安装钩子。我们可以在 *wasm-game-of-life/src/lib.rs* 中的 `Universe::new()` 构造函数中调用它：
```rust
pub fn new() -> Universe {
    utils::set_panic_hook();

    // ...
}
```

## 增加日志到我们的 Game of Life
让我们 [使用 `console.log` 函数 通过 `web-sys` 箱 来增加一些日志](https://rustwasm.github.io/docs/book/reference/debugging.html#logging-with-the-console-apis)，关于 `Universe::tick` 函数中的每个细胞。      
首先，在 *wasm-game-of-life/Cargo.toml* 中，增加 `web-sys` 作为一个依赖，并且启动它的 `"console"` 特性。
```toml
[dependencies]

# ...

[dependencies.web-sys]
version = "0.3"
features = [
  "console",
]
```
为了工效学，我们将 `console.log` 函数包裹进一个 `println!` 风格的宏中：
```rust
extern crate web_sys;

// 一个为了 `console.log` 记录日志提供了 `println!(..)`-风格语法 的宏.
macro_rules! log {
    ( $( $t:tt )* ) => {
        web_sys::console::log_1(&format!( $( $t )* ).into());
    }
}
```
现在，我们可以 通过在 ***Rust*** 代码中插入 `log` 的调用 来开始将消息日志记录到控制台。例如，为了记录每个细胞的状态、活得邻居数和下一个状态，我们可以修改 `wasm-game-of-life/src/lib.rs`，如下：
```rust
diff --git a/src/lib.rs b/src/lib.rs
index f757641..a30e107 100755
--- a/src/lib.rs
+++ b/src/lib.rs
@@ -123,6 +122,14 @@ impl Universe {
                 let cell = self.cells[idx];
                 let live_neighbors = self.live_neighbor_count(row, col);

+                log!(
+                    "cell[{}, {}] is initially {:?} and has {} live neighbors",
+                    row,
+                    col,
+                    cell,
+                    live_neighbors
+                );
+
                 let next_cell = match (cell, live_neighbors) {
                     // Rule 1: Any live cell with fewer than two live neighbours
                     // dies, as if caused by underpopulation.
@@ -140,6 +147,8 @@ impl Universe {
                     (otherwise, _) => otherwise,
                 };

+                log!("    it becomes {:?}", next_cell);
+
                 next[idx] = next_cell;
             }
         }
```
注意：需要将 *wasm-game-of-life/Cargo.toml* 中 `[features]` 节中的 `default` 注释掉，应该是有冲突。然后才能编译成功。(注意，如果开的东西太多了，这个网页可能会很卡，因为默认以 1ms 的间隔 tick，控制台输出非常多)
```toml
[features]
# default = ["console_error_panic_hook"]
```

## 使用一个 Debugger 来在每个 Tick 间暂停
调试器对于检查生成的WebAssembly与之交互的JavaScript非常有用。
[浏览器的步进 debuggers 对于检查我们的 ***Rust*** 生成的 WebAssembly 与之交互的 ***JavaScript*** 非常有用](https://rustwasm.github.io/docs/book/reference/debugging.html#using-a-debugger)。      
例如，我们可以使用调试器在 `renderLoop` 函数的每次迭代中暂停，方法是在 `universe.tick()` 前放置  [一个 ***JavaScript*** `debugger;` 语句](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/debugger)。
```javascript
const renderLoop = () => {
  debugger;
  universe.tick();

  drawGrid();
  drawCells();

  requestAnimationFrame(renderLoop);
};
```
这为我们提供了一个方便的检查点，用于检查日志记录的内容，并将当前渲染的帧与前一帧进行比较。

## 练习 
- 将日志记录添加到 `tick` 函数中，该记录每个发生变化的单元格的行和列，和它发生的变化。
- 在 `Universe::new` 中 触发 `panic!()` 。在 Web 浏览器的 ***JavaScript*** 调试器中检查 panic 的回溯。禁用调试符号，在没有 `console_error_panic_hook` 可选依赖项的情况下重建，并再次检查堆栈跟踪。不是很有用，是吧？