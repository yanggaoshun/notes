## pnpm

- 官方文档：https://pnpm.io/zh/

pnpm 是 npm 的替代品，它有以下优点：

- 速度快：pnpm 使用了多线程并行处理，可以加快包安装速度。
- 缓存：pnpm 使用了内容哈希算法，可以缓存重复的包，加快后续安装速度。
- 兼容性：pnpm 兼容 npm，可以与 npm 生态系统完全兼容。
- 软链接：pnpm 使用软链接，可以减少磁盘占用。
- 锁文件：pnpm 使用 lockfile，可以确保项目依赖的版本不会被意外更新。

## 常用命令

- 安装：`pnpm install`
- 全局安装：`pnpm i -g <package>`
- 升级：`pnpm update`
- 移除：`pnpm remove`
- 运行：`pnpm run <script>`
- 发布：`pnpm publish`
- 链接：`pnpm link`
    - `pnpm link <dir> 将<dir>` 链接到当前项目的 node_modules 目录下
    - `pnpm link --dir <dir>` 将当前包链接到 <dir> 目录下的 node_modules 目录下
    - `pnpm link --global` 将当前包链接到全局 node_modules 目录下
    - `pnpm unlink` 仅删除当前目录的链接
    - `pnpm uninstall --global <pkg>` 删除全局的链接
- 导入：`pnpm import`
- 打补丁：`pnpm patch <pkg>@<version>`
  - 使用完命令后会生成这个包对应的一个零时副本, 在这个副本里更改我们要修改的代码
- 提交补丁: `pnpm patch-commit <path>`
  - 使用`pnpm patch <pkg>@<version>`命令, 对包做完修改后, 执行这个命令将生成patches目录及patch文件, 同时将这次补丁注册在package.json中, 这样下次再使用pnpm install时, 会自动安装这个补丁。
  - path 一般为`pnpm patch <pkg>@<version>`生成对零时目录

## workspaces

pnpm 支持 workspaces，可以将多个项目组成一个仓库，使用 pnpm 安装时，会自动安装所有项目的依赖。

在项目根目录下，创建 `pnpm-workspace.yaml` 文件，写入以下内容：

```yaml
packages:
  - 'packages/*'
```

然后在 `packages` 目录下创建多个项目，每个项目都有自己的 `package.json` 文件。


最后，在项目根目录下运行 `pnpm install` 命令，即可安装所有项目的依赖。

### catalogs

catalog 可以指定在当前workspace中使用的同一个包的版本

pnpm 支持 catalogs，可以将多个版本的依赖包放在一起，使用时只需要指定版本号即可。

```yaml
catalogs:
  react18:
    react: ^18.2.0
    react-dom: ^18.2.0
  react17:
    react: ^17.0.2
    react-dom: ^17.0.2
```
```yaml
catalog:
  react: ^18.2.0
  react-dom: ^18.2.0
```
### 用法

```json
{
  "name": "@example/components",
  "dependencies": {
    "react": "catalog:react18",
  }
}
```

### 别名

pnpm 支持别名，可以给包设置别名，使用时可以用别名来引用包。

```shell
pnpm add lodash1@npm:lodash@1
pnpm add lodash2@npm:lodash@2
```