---
layout: bootstrap
title: 'Gitlab CI/CD 流水线配置参考'
description: 'Gitlab CI/CD 流水线配置参考'
date: '2019-07-25 15:41:00'
author: Jast
---
## Gitlab CI/CD 流水线配置参考
.gitlab-ci.yml文件定义了流水线的结构和顺序，并确定：
- 使用GitLab Runner执行什么。
- 遇到特定条件时要做出哪些决定。例如，当进程成功或失败时。
### 不可用的jobs名称
每个作业必须具有唯一的名称，但有一些保留的关键字不能用作作业名称：
- image
- services
- stages
- types
- before_script
- after_script
- variables
- cache

### 配置参数
| 参数 | 作用描述 |
| ---- | ----  |
| script|script是job所需要的唯一必须的参数。它是一个由Runner执行的shell脚本 |
| image|用于指定用于job的Docker镜像。 |
| services|用于指定服务的Docker镜像，链接到镜像中指定的基本镜像。 |
| before_script|用于定义应在所有作业（包括部署作业）之前运行但在恢复工件之后运行的命令。这可以是数组或多行字符串。 |
| after_script|用于定义将在所有作业（包括失败的作业）之后运行的命令。这必须是数组或多行字符串。 |
| stages|用于定义可由作业使用的阶段，并在全范围内定义。 |
| stage|stage是按工作定义的，依赖于全局定义的阶段。它允许将作业分组到不同的阶段，并且同一阶段的作业并行执行（受特定条件限制）。 |
| only|定义作业将运行的分支（branches）和标记（tags）的名称。 |
| except|定义作业不会运行的分支和标记的名称。 |
| tags|用于从允许运行此项目的所有运行程序列表中选择特定的运行程序。 |
| allow_failure|允许作业失败而不会影响CI套件的其余部分。除手动作业外，默认值为false。 |
| when|用于实现在发生故障或尽管发生故障时运行的作业。取值：on_success，on_failure，always ，manual  |
| environment|用于定义作业部署到特定环境。如果指定了环境且该名称下没有环境，则将自动创建一个新环境。 |
| cache|用于指定应在作业之间缓存的文件和目录列表。您只能使用项目工作区内的路径。 |
| artifacts|用于指定文件和目录的列表，这些文件和目录应在成功，失败或始终作业时附加到作业。作业完成后，工件将被发送到GitLab，并可在GitLab UI中下载。 |
| dependencies|此功能应与工件结合使用，并允许您定义要在不同作业之间传递的工件。 |
| coverage|允许您配置从作业输出中提取代码覆盖率的方式。 |
| retry|允许您配置在发生故障时重试作业的次数。 |
| parallel|允许您配置并行运行的作业实例数。该值必须大于或等于二（2）且小于或等于50。 |
| trigger|允许您定义下游流水线触发器。当GitLab启动从触发器定义创建的作业时，将创建下游流水线 |
| include|使用include关键字，您可以允许包含外部YAML文件。 include要求外部YAML文件具有扩展名.yml或.yaml，否则将不包括外部文件。 |
| extends|定义了使用extends的作业将继承的作业名称。 |
| pages|是一项特殊工作，用于将静态内容上传到GitLab，可用于为您的网站提供服务。它具有特殊语法，因此必须满足以下两个要求：任何静态内容都必须放在public/目录下。必须定义具有public/目录路径的工件。 |
| variables|GitLab CI/CD允许您在.gitlab-ci.yml中定义变量，然后在作业环境中传递。 它们可以在全局和每个作业设置。 在作业级别使用variables关键字时，它会覆盖全局YAML变量和预定义变量。 |
