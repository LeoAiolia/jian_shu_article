###Xcode Workspace
```
A workspace is an Xcode document that groups projects and other documents so you can work on them together. 
A workspace can contain any number of Xcode projects, plus any other files you want to include. 
In addition to organizing all the files in each Xcode project, a workspace provides implicit and explicit relationships among the included projects and their targets.
```
一个workspace 它可以包含多个Project和其他文档文件
###Xcode Project
```
An Xcode project is a repository for all the files, resources, and information required to build one or more software products. 
A project contains all the elements used to build your products and maintains the relationships between those elements. 
It contains one or more targets, which specify how to build products. 
A project defines default build settings for all the targets in the project 
(each target can also specify its own build settings, which override the project build settings).
```
project就是一个个的仓库，里面会包含属于这个项目的所有文件，资源，以及生成一个或者多个软件产品的信息。每一个project会包含一个或者多个 targets，而每一个 target 告诉我们如何生产 products。project 会为所有 targets 定义了默认的 build settings，每一个 target 也能自定义自己的 build settings，且 target 的 build settings 会重写 project 的 build settings。

Xcode Project 文件会包含以下信息，对资源文件的引用(源码.h和.m文件，frame，资源文件plist，bundle文件等，图片文件image.xcassets还有Interface Builder(nib)，storyboard文件)、文件结构导航中用来组织源文件的组、Project-level build configurations(Debug\Release)、Targets、可执行环境，该环境用于调试或者测试程序。
###Xcode Target
```
A target specifies a product to build and contains the instructions for building the product from a set of files in a project or workspace. 
A target defines a single product; it organizes the inputs into the build system—the source files and instructions for processing those source files—required to build that product. 
Projects can contain one or more targets, each of which produces one product.
```
target 会有且唯一生成一个 product, 它将构建该 product 所需的文件和处理这些文件所需的指令集整合进 build system 中。Projects 会包含一个或者多个 targets,每一个 target 将会产出一个 product。

这里值得说明的是，每个target 中的 build setting 参数继承自 project 的 build settings, 一旦你在 target 中修改任意 settings 来重写 project settings，那么最终生效的 settings 参数以在 target 中设置的为准. Project 可以包含多个 target, 但是在同一时刻，只会有一个 target 生效，可用 Xcode 的 scheme 来指定是哪一个 target 生效。

###Build Settings
```
A build setting is a variable that contains information about how a particular aspect of a product’s build process should be performed. 
For example, the information in a build setting can specify which options Xcode passes to the compiler.
```
build setting 中包含了 product 生成过程中所需的参数信息。project的build settings会对于整个project 中的所有targets生效，而target的build settings是重写了Project的build settings，重写的配置以target为准。

一个 build configaration 指定了一套 build settings 用于生成某一 target 的 product，例如Debug和Release就属于build configaration。

###Xcode Scheme
```
An Xcode scheme defines a collection of targets to build, a configuration to use when building, and a collection of tests to execute.
```
一个Scheme就包含了一套targets(这些targets之间可能有依赖关系)，一个configuration，一套待执行的tests。

###联系
这5者的关系，举个可能不恰当的例子，
Xcode Workspace就如同工厂，Xcode Project如同车间，每个车间可以独立于工厂来生产产品(project可独立于workspace存在)，但是各个车间组合起来就需要工厂来组织(如果用了cocopods，就需要用workspace)。Xcode Target是一条条的流水线，一条流水线上面只生产一种产品。Build Settings是生产产品的秘方，如果是生产汽水，Build Settings就是其中各个原料的配方。Xcode Scheme是生产方案，包含了流水线生产，秘方，还包含生产完成之后的质检(test)。

本文摘抄自https://www.jianshu.com/p/83b6e781eb51
