# Prettier 

## 相关资料
- [官方文档](https://prettier.io/docs/en/index.html)
- [Prettier 与 ESLinter 配合使用](https://github.com/prettier/eslint-config-prettier)
- [使用 lint-staged 在提交代码前做点手脚](https://github.com/okonet/lint-staged#configuration)
## 什么是 Prettier

> Prettier is an opinionated code formatter

Prettier 是一个目前最流行的代码格式化工具，它能设置在项目代码中，提供了很多额外的功能例如格式化的配置、代码预提交的 hook等等。目前绝大部分项目都会使用它作为格式化工具。

## Prettier vs. Linters
[官方有很好的回答](https://prettier.io/docs/en/comparison.html)

就我总结而言：
- Prettier 只关乎代码的格式问题
- Linters 除了格式问题，还会关注代码的编写风格，例如没有被使用的变量应该要被删除等。

## 代码提交前的格式化检测
> Run linters against staged git files and don't let 💩 slip into your code base!

我太爱英文的表达了，生动又形象

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





