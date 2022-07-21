# Prettier 

## 相关资料
- [官方文档](https://prettier.io/docs/en/index.html)
- [Prettier 与 ESLinter 配合使用](https://github.com/prettier/eslint-config-prettier)
- [结合 lint-staged 在提交代码前做点手脚](https://github.com/okonet/lint-staged#configuration)
## 什么是 Prettier

> Prettier is an opinionated code formatter

Prettier 是一个目前最流行的代码格式化工具，它能设置在项目代码中，除了 vscode 插件，还提供了命令行格式化或检查等的工具，意味着可以使用 git hook pre-commit 在代码提交前做些手脚
## Prettier vs. Linters
[官方有很好的回答](https://prettier.io/docs/en/comparison.html)

就我总结而言：
- Prettier 只关乎代码的格式问题
- Linters 除了格式问题，还会关注代码的编写风格，例如没有被使用的变量应该要被删除等。

## 结合 lint-stage, 在代码提交前的格式化检测
lint-stage 可以帮你简化了 pre-commit 的一些配置操作，只需要简单的安装并配置自己想要检测的文件以及对应的操作即可
> Run linters against staged git files and don't let 💩 slip into your code base!
lint-stage 的标语 😂

举例两种测试过能 work 的操作（省略安装步骤）：
- 自动格式化需要提交的文件
  
  package.json
  ```
  {
    "lint-staged": {
      "*": ["eslint", "prettier --write", "git add"]
    }
  }
  ```
- 没有格式化代码不允许提交
  
  package.json
  ```
  {
    "lint-staged": {
      "*": "prettier --check"
    }
  }
  ```



## 在 submodule 中使用 lint-stage
想要 lint-stage 能在 submodule 中 work ，可能就需要理解 lint-stage 的工作原理了。

可能大概清楚它会使用 git hook, 但如何使用？又是如何将此共享到仓库的？

### 如何使用 git hook
在仓库 init 的时候，git 会生成一个 .git/hooks 的文件夹，里边存储了一些对应 hook 的示例代码（一些很难看懂的 shell 命令行，可以暂时不用看懂）

可以试着把 .example 后缀去掉后，可以随意试验一个 hook

### 如何共享 git hooks 到仓库
有个问题在于, .git/hooks 文件并不会跟仓库同步，因此便需要告诉 git 把 hooks 存到另一个可以同步到仓库的目录，于是就有了这条命令行
```
git config core.hooksPath [一个 make sence 的文件名]
# 例如 git config core.hooksPath .githooks
```
于是便实现了 hooks 的共享

### 更轻松的方式：使用 [husky](https://github.com/typicode/husky)
lint-stage 其实就依赖了 husky 

安装 husky 时，它会把 git 的 hooks 目录由 .git/hooks 改成 .husky

### 回到问题
在主仓库安装 lint-stage 时，上述的操作只会在主仓库中进行，但 submodule 是一个独立的仓库，它有自己的 hooks 文件，所以 submodule 是不会被自动配置好的，所以就需我们手动去做这些配置工作
- 复制主仓库的 .husky 文件目录到子仓库
- 设置子仓库的 hooks 文件：git config core.hooksPath .husky

这样就能 work 啦~


