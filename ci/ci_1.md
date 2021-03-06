## 持续交付(一)：基础

```
持续集成是一种实践,不是一个工具，他的有效性依赖于团队的纪律。
```

### 配置管理

+ 为管理的应用程序的构建、部署、测试和发布过程做好准备。
+ 管理应用软件的配置信息。
+ 整个环境的配置管理，这包括应用程序所依赖的软件、硬件和基础设施。

#### 使用版本控制

+ **对所以内容进行版本控制**
  +  版本控制不仅仅针对源代码。所有开发相关的产物都应该放置在版本控制之下，包括
    + 测试代码
    + 数据库脚本
    + 构建和部署脚本
    + 文档
    + 库文件
    + 应用软件的配置文件


+ 目标
    + 让新加入的员工可以很容易从零开始工作。
    + 选择从开发环境至生产环境整个环节中的任意时间点，并将系统恢复到该时间点的状态。


+ **频繁提交代码到主**

  + 只有频繁提交代码,才能享受版本控制所带来的好处。(注：正确同时独立)
  + 变更提交之后，团队的所有人都可以看到变更和签出。
  + 如果使用了持续集成，还会触发一次构建。


  **长时间不提交代码的问题：**

+ 一次性提交很多功能不容易追溯问题。

+ 一次性提交太多代码，不容易合并。

    **为了确保提交代码时不破坏已有的应用程序，如下实践比较有效**

  + 提交代码前运行测试套件。


  + 增量式引入变化，完成一个小功能或者一次重构就提交代码。



+ **使用意义明显的提交注释**

  写描述性提交注释的最重要原因在于：当构建失败以后，你知道是谁破坏了构建，以及他为什么破坏构建。

#### 依赖管理

+ 外部库文件管理
  + 本地保存一份外部库的副本(如果使用Maven应该创建一个本地仓库,里面存放公司使用的全部外部库)。
  + 构建系统中应该始终指定所需外部库的确切版本。如果不这么做的话，很可能无法保证每次都能够完全再现你的构建版本。
  + 外部库文件放不放版本管理，各有利弊,这个问题到不是很重要。
    +  放版本管理可以更加方便将正确的库与库文件版本相关联，但是使源代码库的体积更大，签出时间也会更长。


+ 组件管理


#### 软件配置管理

#### 环境管理	

### 持续集成

持续集成要求每当有人提交代码时，就对整个应用进行构建，并对其执行全面的自动化测试集合。至关重要的是假如构建或测试过程失败，开发团队就要停下手中的工作，立即修复它。持续集成的目标是让开发的软件一直处于可工作状态。

#### 准备工作

+ **版本控制**

  与项目相关的所有内容都必须提交到一个版本控制中，包括产品代码、测试代码、数据库脚本、构建与部署脚本，以及所有用于创建、安装、运行和测试该应用程序的东西。


+ **自动化构建**

  人和计算机都可以通过命令行自动执行应用的构建、测试一级部署过程。

+ **团队共识**

  持续集成不是一种工具、而是一种实践。它需要开发团队能够给予一定的投入并遵循一些准则，需要每个人都以小步增量的方式频繁地将修改后的代码提交到主干，并一致认同**修改破坏应用程序的任意修改是最高优先级的任务**。

#### 持续集成的前提条件

+ **频繁提交**

  频繁提交的结果是每次修改都比较小,很少会使构建失败。当做错事情或者路线可以轻松回滚。

+ **创建全面的自动化测试套件**

  没有一系列全面的自动化测试，那么构建成功只是意味着应用程序能够编译并组装在一起。

  有三类测试会在持续集成中使用到：

  + 单元测试

    用于单独测试应用程序中某些小单元的行为，不需要启动整个应用程序，也不需要链接数据库或者文件系统或者网络。

  + 组件测试

    用于测试应用程序中的几个组件的行为，不需要启动整个应用程，但是有可能连接数据库、访问文件系统或者外面接口。

  + 验收测试

    验证程序是满意业务需求所定义的验收条件，包括应用程序提供的功能以及其他特殊需求。

+ **保持较短的构建和测试过程**

  时间太长的影响(10分钟是极限,最好是5分钟之内,90秒完成是最理想的)：

  + 大家提交前不愿意在本机进行全量构建和运行测试。
  + 大家提交会变少，因为每次提交都需要等上好久。

+ **管理开发工作区**

  当开发人员刚刚开始新任务时，应该总是从一个已知正确的状态开始。他们应该能够运行构建、执行自动化测试、以及在其可控的环境上部署其开发的应用程序。通常是在自己的开发机器上面。

  **达成以上目标的前提条件:**

  + 细心的配置管理，包括代码、测试数据、数据库脚本、构建部署脚本。
  + 第三方依赖的配置管理，即开发中使用的库文件和组件。
  + 确保自动化测试(包括冒烟测试)都能够在开开发机器上运行。


#### 必不可少的实践

+ 构建失败之后就不要再继续提交代码了。
+ 提交前再本地运行所有的提交测试、或者让持续集成服务器完成此事。
+ 等提交测试通过后再继续工作。
+ 回家之前构建必须处于成功状态。
+ 时刻准备着回滚到前一个版本。
+ 在回滚之前要规定一个修复时间。
+ 不要将失败的测试注释掉。
+ 为自己导致的问题负责。
+ 测试驱动的开发。

