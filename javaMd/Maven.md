# Maven

- Maven是一个Java项目管理和构建工具，它可以定义项目结构、项目依赖，并使用统一的方式进行自动化构建，是Java项目不可缺少的工具。
- Maven解决了依赖管理的问题



---

## 依赖关系

- Maven定义了几种依赖关系，分别是`Complie`、`test`、`runtime`、`provided`

| scope    | 说明                                          | 示例            |
| :------- | :-------------------------------------------- | :-------------- |
| compile  | 编译时需要用到该jar包（默认）                 | commons-logging |
| test     | 编译Test时需要用到该jar包                     | junit           |
| runtime  | 编译时不需要，但运行时需要用到                | mysql           |
| provided | 编译时需要用到，但运行时由JDK或某个服务器提供 | servlet-api     |



---

## 唯一ID

对于某个依赖，Maven只需要3个变量即可唯一确定某个jar包：

- groupId：属于组织的名称，类似Java的包名；
- artifactId：该jar包自身的名称，类似Java的类名；
- version：该jar包的版本。

通过上述3个变量，即可唯一确定某个jar包。Maven通过对jar包进行PGP签名确保任何一个jar包一经发布就无法修改。修改已发布jar包的唯一方法是发布一个新版本。

因此，某个jar包一旦被Maven下载过，即可永久地安全缓存在本地。

---

## 构建流程

### 生命周期（lifecycle）

- 由一系列phase构成

### 阶段（phase）

- 由一系列goal构成
- 使用`mvn`这个命令时，后面的参数是phase，Maven自动根据生命周期运行到指定的phase。
- 经常用到的phase其实只有几个：
  - clean：清理
  - compile：编译
  - test：运行测试
  - package：打包

## Goal

- goal的命名总是`abc:xyz`这种形式。



***三者关系***：

- lifecycle相当于Java的package，它包含一个或多个phase；
- phase相当于Java的class，它包含一个或多个goal；
- goal相当于class的method，它其实才是真正干活的。

---

