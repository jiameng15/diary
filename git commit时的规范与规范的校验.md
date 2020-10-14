近期小小的参与了开课吧接受的element3开源项目,并用vue3.0的composition API 重构了部分代码,成为贡献者并学到了很多平时不在表面的知识,感谢开课吧提供的这次机会,感谢为element贡献力量的小伙伴们,感谢开源世界让我们为梦想不再负重前行.

暂用次文档记录开发中遇到的问题,和学到的知识,如果很多,会再深化细分.

不管是在开源世界中穿梭行走,还是在中小企业中挣扎求存,git都是我们必须的宝具,pull,commit,push早已炉火纯青,无需多言,今次记录的是在提交代码之前,学到的知识——Git Hook.

## Git Hook

package.json

```json
// 
{
  "gitHooks": {
    "pre-commit": "lint-staged",
    "commit-msg": "node build/verifyCommit.js",
    "pre-push": "npm run test:unit"
  },
  "lint-staged": {
    "*.{js,vue}": "eslint"
  }
}
```

在package.json文件中,我们使用 ` gitHooks ` 字段配置了 [ Git Hook ](https://cli.vuejs.org/zh/guide/cli-service.html#git-hook)  的使用,依赖于 [yorkie](https://github.com/yyx990803/yorkie)

[^1]: yorkie由husky改编而来

### commit-msg规范

代码编写的校验规则,我们使用了eslint,而使用git提交代码的规则,我们使用了commi-msg,具体规范写在下文的 verifyCommit.js中.

verifyCommit.js

```js
// Invoked on the commit-msg git hook by yorkie.

const chalk = require('chalk')
const msgPath = process.env.GIT_PARAMS
const msg = require('fs').readFileSync(msgPath, 'utf-8').trim()

const releaseRE = /^v\d/
const commitRE = /^(revert: )?(migrate|feat|fix|docs|dx|refactor|perf|test|workflow|build|ci|chore|types|wip|release): .{1,50}/
if (!msg.startsWith('Merge branch')) {
  if (!releaseRE.test(msg) && !commitRE.test(msg)) {
    console.log('message is:', msg)
    console.error(
      `  ${chalk.bgRed.white(' ERROR ')} ${chalk.red(
        'invalid commit message format.'
      )}\n\n` +
        ` rule is ${chalk.green(
          '/^(revert: )?(migrate|feat|fix|docs|dx|refactor|perf|test|workflow|build|ci|chore|types|wip|release): .{1,50}/'
        )} \n` +
        chalk.red(
          '  Proper commit message format is required for automated changelog generation. Examples:\n\n'
        ) +
        `    ${chalk.green("feat: add 'comments' option")}\n` +
        `    ${chalk.green('fix: handle events on blur (close #28)')}\n\n` +
        chalk.red('  See .github/commit-convention.md for more details.\n')
    )
    process.exit(1)
  }
}

```

commit-msg规范旨在填写commit message时对message所做的检查,具体规范与约束,其中给出了如下说明:

#### commit message 的规范

`<type>: <subject>`

翻译一下就是

<提交的类型>:我是一个空格<提交的信息>

规范中规定,`type`有7种类型可以使用,而在上述配置的`commit-msg`规则中,又增加了几种

#### type 的类型

* 7中常用type

  * `feat` 新功能(feature)
  * `fix` 修复bug
  * `refactor` 重构(既不是feat,也不是fix)
  * `docs` 文档(documentation)
  * `style` 格式(不影响代码运行的变动)
  * `test` 测试
  * `chore` 构建过程或辅助工具的变动

* 其他type

  * `revert` 

  * `migrate` 迁移
  * dx
  * perf
  * workflow
  * build
  * ci
  * types
  * wip
  * release

