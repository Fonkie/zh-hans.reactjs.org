---
title: "为 React 预览版的未来做准备"
author: [acdlite]
---

为了与 React 生态系统的合作伙伴分享即将到来的变化，我们正在建立正式的预览通道。我们希望这个过程能帮助我们对 React 的变化更加自信，并让开发者有机会尝试实验性的功能。

> 这篇文章将与框架、库或者开发者工具的开发者相关。主要使用 React 来构建面向用户的应用程序的开发人员不需要担心我们的预览通道。

React 依赖于蓬勃发展的开源社区来提交 bug 报告，pull 请求和[提交 RFC](https://github.com/reactjs/rfcs)。为了鼓励反馈，我们有时会分享包含未发布特性的 React 的特殊版本。

由于 React 实际来源于我们的 [公共 GitHub 库](https://github.com/facebook/react)，你可以构建一个包含最新变化的 React 副本。但是，对于开发人员来说，从 npm 安装 React 非常容易，因此我们会将预览版发布到 npm 注册表。最近的例子是 16.7 alpha 版本，其中包括了早期版本的 Hook API。

我们想让开发人员更容易地测试 React 的预览版，因此我们使用了三个独立的发布通道来规范我们的流程。

## 发布通道

> 这篇文章中的内容也可以在我们的[发布通道](/docs/release-channels.html)文档页面找到。每当发布过程发生变化时，我们都会更新该文档。

React 的每个发布通道都是针对不同的用例设计的：

- [**Latest**](#latest-channel)是稳定的 React semver 版本的发布通道。这是你从 npm 安装 React 正在使用的通道。 **对于所有面向用户的 React 应用程序，请使用此通道**
- [**Next**](#next-channel) 跟踪 React 源代码库的 master 分支，下一个次要 semver 版本的候选版本，用于 React 和第三方项目之间的集成测试。
- [**Experimental**](#experimental-channel)包括实验性 API 和在稳定版本中不可用的特性。这些特性也会跟踪 master 分支，但会打开附加特性的标记。使用此通道来试用即将发布的特性。

所有的版本都会发布到 npm，但只有最新的版本使用了[语义版本](/docs/faq-versioning.html)。预览版（Next 和 Experimental 通道的版本）是根据其内容的哈希值生成的版本，例如 `0.0.0-1022ee0ec` 用于 Next 版本，`0.0.0-experimental-1022ee0ec` 用于 Experimental 版本。

**对于面向用户的应用程序，官方支持的唯一发布通道是 Latest**。Next 和 Experimental 版本仅用于测试目的，我们不能保证不同版本之间的行为不会发生变化。它们不遵循我们用于 Latest 版本的 semver 协议。

将预览版发布到与稳定版本相同的注册表，我们可以利用许多支持 npm 工作流的工具，比如：[unpkg](https://unpkg.com) 和 [CodeSandbox](https://codesandbox.io)。

### Latest 通道

Latest 用于稳定的 React 版本，它对应于 npm 上的 `latest` 标签，是所有 React 应用发布给实际用户的推荐通道。

**如果你不确定应该使用哪个通道，那就用 Latest** 如果你是 React 开发人员，那么这就是你正在使用的通道。

你可以认为 Latest 的更新是非常稳定的。版本号遵循语义化版本控制方案。在[版本号规则](/docs/faq-versioning.html)中了解更多关于我们对稳定性和增量迁移的承诺。

### Next 通道

Next 通道是一个预览通道，用于跟踪 React 库的 master 分支。我们使用 Next 通道的预览版作为 Latest 通道的候选版本。 你可以将 Next 视为 Latest 的超集，该超集的更新更频繁。

最近的 Next 版本和 Latest 版本之间的变化程度，与两个次要的 semver 版本之间的变化程度大致相同。但是，**Next 通道不符合语义版本。**在 Next 通道中，你应该预期到后续的版本中偶尔会有不兼容的改动。

**请勿在面向用户的应用程序中使用预览版。**

在 Next 中的预览版发布在 npm 上，带有 `next` 标记。版本号是根据其构建内容的哈希值生成的，例如：`0.0.0-1022ee0ec`。

#### 使用 Next 通道进行集成测试

Next 通道用于支持 React 和其他项目之间的集成测试。

React 的所有更改在发布之前都要经过大量的内部测试。然而，React 的整个生态系统使用了无数的环境和配置，我们不可能针对每一个进行测试。

如果你是第三方 React 框架、库、开发者工具或类似基础框架类型项目的作者，可以针对最近的更新，定期运行测试用例，帮助我们为你的用户和整个 React 社区保持 React 的稳定。如果你感兴趣，请按照以下步骤操作：

- 使用你喜欢的持续集成平台设置 cron 作业。cron 作业由 [CircleCI](https://circleci.com/docs/2.0/triggers/#scheduled-builds) 和 [Travis CI](https://docs.travis-ci.com/user/cron-jobs/) 支持。
- 在 cron 作业中，使用 npm 的 `next` tag，将 React 包更新到 Next 通道中最近的 React 版本。使用 npm cli：

  ```
  npm update react@next react-dom@next
  ```

  或 yarn：

  ```
  yarn upgrade react@next react-dom@next
  ```
- 针对更新的包运行你的测试用例。
- 如果一切顺利，那就太好了！你可以预期你的项目将在下一个次要的 React 版本中正常工作。
- 如果发生异常，请通过[提交 issue](https://github.com/facebook/react/issues)告知我们。

使用这个工作流的项目是 Next.js。（不开玩笑，这是真的！）你可以参考他们的 [CircleCI 配置](https://github.com/zeit/next.js/blob/c0a1c0f93966fe33edd93fb53e5fafb0dcd80a9e/.circleci/config.yml)作为示例。

### Experimental 通道

与 Next 一样，Experimental 通道也是一个预览版的通道，用于跟踪 React 库的 master 分支。 与 Next 不同的是，Experimental 版本包括尚未准备好更广泛发布的其他特性和 API。

通常，对 Next 的更新伴随着对 Experimental 相应的更新。它们基于相同的源修订，但使用一组不同的特性标志构建。

Experimental 版本可能与 Next 和 Latest 版本有很大的不同。**请勿在面向用户的应用程序中使用 Experimental 版本。**在 Experimental 通道中，你应该预期到版本之间会有不兼容的改动。

在 Experimental 中的预览版发布在 npm 上，带有 `experimental` tag。版本号是根据其构建内容的哈希值生成的，例如：`0.0.0-experimental-1022ee0ec`。

#### 什么是 Experimental 版本?

Experimental 特性是指尚未准备好向更广泛的公众发布的功能，在最终确定之前可能会发生重大变化。某些实验可能永远不会最终确定 —— 我们进行实验的原因是测试所提出的变更的可行性。

例如，如果 Experimental 通道在宣布 Hook 的时候就已经存在，我们会在 Latest 版本发布前几周就发布到 Experimental 通道。

你可能会发现在 Experimental 上运行集成测试是有价值的。这取决于你。但是，请注意 Experimental 甚至比 Next 更不稳定。**我们不保证 Experimental 版本的稳定性。**

#### 怎样才能了解更多的 Experimental 特性?

Experimental 特性可能会被记录，也可能不会被记录。通常，实验直到接近 Next 或 Stable 版本时才会被记录。

如果未记录某项特性，则可能附带 [RFC](https://github.com/reactjs/rfcs)。

当我们准备宣布新的实验时，我们将发布到 React 博客，但这并不意味着我们将公布每个实验。

你可以随时查阅我们的公共 GitHub 库的[历史](https://github.com/facebook/react/commits/master)，以获得完整的更改列表。
