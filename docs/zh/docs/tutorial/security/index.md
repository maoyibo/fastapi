# 安全介绍

有许多方法可以处理安全性、身份验证和授权。

这通常是一个复杂而 "困难" 的话题。

在许多框架和系统中，仅仅处理安全性和身份验证就需要大量的工作和代码(在许多情况下，它可能占所有代码的50%或更多)。

**FastAPI** 提供了几种工具，可以帮助您轻松、快速地以标准的方式处理 **安全性**，而无需研究和学习所有的安全性规范。

但首先，让我们检查一些小概念。

## 赶时间吗?

如果您不关心这些术语，您*现在*只需要添加基于用户名和密码的身份验证安全性，请跳到下一章。

## OAuth2

OAuth2 是一个规范，定义了几种处理身份验证和授权的方法。

它是一个相当广泛的规范，涵盖了几个复杂的用例。

它包括使用 "第三方" 进行身份验证的方法。

这是所有 "登录 Facebook，谷歌，Twitter, GitHub" 的系统内部使用的。

### OAuth 1

还有一个 OAuth 1，它与 OAuth2 非常不同，而且更复杂，因为它包含了关于如何加密通信的直接规范。

它现在不是很流行或使用。

OAuth2 没有指定如何加密通信，它希望您的应用程序使用 HTTPS。

!!! tip
    在 **部署** 一节中，您将看到如何使用 Traefik 和 Let's Encrypt 免费设置 HTTPS 。


## OpenID Connect

OpenID Connect 是另一个基于 **OAuth2** 的规范。

它只是扩展了 OAuth2，指定了一些在 OAuth2 中相对模糊的东西，以尝试使其更具互操作性。

例如，谷歌登录使用 OpenID Connect (它底层使用 OAuth2 )。

但是Facebook登录不支持 OpenID 连接。它有自己独特的 OAuth2 风格。

### OpenID (不是 "OpenID Connect")

还有一个 "OpenID" 规范。它试图解决与 **OpenID Connect** 相同的问题，但不是基于OAuth2。

所以，这是一个完整的附加系统。

它现在不是很流行或使用。

## OpenAPI

OpenAPI (以前称为Swagger)是用于构建 APIs 的开放规范(现在是Linux基金会的一部分)。

**FastAPI** 是基于 **OpenAPI** 的。

这就使得有多个自动交互式文档接口、代码生成等成为可能。

OpenAPI有一种定义多种安全 "方案" 的方法。

通过使用它们，您可以利用所有这些基于标准的工具，包括这些交互式文档系统。

OpenAPI定义了以下安全方案:

* `apiKey`: 一个应用程序特定的键，可以来自:
    * 查询参数。
    * 消息头。
    * cookie。
* `http`: 标准http认证系统，包括:
    * `bearer`: 消息头 `Authorization` 具有一个值 `Bearer ` 加上令牌。这继承自OAuth2。
    * HTTP 基本身份验证。
    * HTTP 摘要，等等。
* `oauth2`: 所有处理安全性的 OAuth2 方法(称为 "流" )。
    * 其中几个流适用于构建 OAuth 2.0 认证提供商(如谷歌，Facebook, Twitter, GitHub 等):
        * `implicit`
        * `clientCredentials`
        * `authorizationCode`
    * 但有一个特定的 "流" 可以直接地在同一个应用程序中完美地处理身份验证:
        * `password`: 下一章将会介绍一些例子。
* `openIdConnect`: 有一种方法定义如何自动发现 OAuth2 身份验证数据。
    * 这个自动发现是在 OpenID 连接规范中定义的。


!!! tip
    集成其他认证/授权提供商，如谷歌，Facebook, Twitter, GitHub等 也是可能的，而且相对容易。

    最复杂的问题是构建这样的身份验证/授权提供者，但是 **FastAPI** 为您提供的工具为您完成了繁重的工作，可以轻松完成此任务。

## **FastAPI** 工具

FastAPI 在 `fastapi.security` 模块中为每种安全方案提供了几种工具，简化了使用这些安全机制。

在下一章中，您将看到如何使用 **FastAPI** 提供的工具为您的 API 添加安全性。

您还将看到它如何自动集成到交互式文档系统中。
