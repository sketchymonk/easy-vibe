# 现代 AI 生图 SaaS 开发实战

## Descripcion general

Este proyecto practico te requiere trabajar con un PRD real（产品需求文档），completar desde cero un参考 Midjourney 体验的 AI 生图 SaaS 产品。你将完整经历Analisis de requisitos、项目拆解、Desarrollo iterativo、Integracion y despliegue的全过程。

Esta es la seccion de practica integral de la Etapa 2。在前面几章中，你已经分别学习了前端页面设计、后端接口开发、数据库操作、Integracion de pagos等单项技能——这个项目要求你把它们全部串起来，交付一个可运行的产品原型。

## Conocimientos previos

Antes de comenzar este proyecto, ya deberias dominar lo siguiente:

- Diseno de paginas frontend y uso de bibliotecas de componentes（[UI 设计](../../frontend/ui-design/)、[现代组件库](../../frontend/modern-component-library/)）
- Diseno y desarrollo de interfaces backend（[接口代码编写](../../backend/ai-interface-code/)）
- Fundamentos de bases de datos y Supabase（[从数据库到 Supabase](../../backend/database-supabase/)）
- Integracion de pagos（[Stripe 收费系统](../../backend/stripe-payment/)）
- Flujo de trabajo de Git y despliegue（[Git 和 GitHub](../../backend/git-workflow/)、[Despliegue Web 应用](../../backend/zeabur-deployment/)）

## Objetivos de aprendizaje

Despues de completar esta practica, podras:

1. Leer y comprender un PRD real, extrayendo una lista de tareas de desarrollo
2. Dividir modulos basandose en el PRD, formulando un plan de avance paso a paso
3. Usar IA para asistir en la construccion del esqueleto frontend y el desarrollo de interfaces backend
4. Verificar y optimizar iterativamente cada modulo
5. Completar la integracion de extremo a extremo, llevando el proyecto de "funcional" a "entregable"

## Introduccion del proyecto

El producto que vas a construir es一个现代 AI 生图 SaaS 平台，包含三个Subsistema：

| Subsistema | Responsabilidad |
|--------|------|
| **Sitio web publico** | Introduccion del producto, precios, FAQ, conversion de registro |
| **Espacio de trabajo del usuario** | Prompt 输入、图片生成、图库、积分、套餐、社区互动 |
| **Panel de administracion** | Gestion de usuarios, gestion de tareas, gestion de pagos, moderacion de contenido, metricas SaaS, monitoreo del sistema |

后端necesita soportar las siguientes capacidades centrales：用户鉴权、图片生成任务、OSS 对象存储、积分与套餐支付、图片社交互动、运营数据监控。

::: tip PRD 入口
El documento de requisitos de este proyecto esta en GitHub： [Ver PRD](https://github.com/datawhalechina/easy-vibe/blob/main/docs/es-es/stage-2/assignments/modern-landing-page/PRD.md)
:::

<div style="margin: 32px 0;">
  <ClientOnly>
    <StepBar :active="0" :items="[
      { title: 'Analisis de requisitos', description: 'Leer el PRD，提取页面、模块、数据模型和边界' },
      { title: 'Construccion del esqueleto', description: '用 AI 生成三套前端骨架（www / app / admin）' },
      { title: 'Desarrollo iterativo', description: '逐模块补充接口、权限、支付、监控' },
      { title: 'Integracion y despliegue', description: 'Verificar de extremo a extremo，Desplegar y preparar la demostracion' }
    ]" />
  </ClientOnly>
</div>

## Primera parte：Analisis de requisitos

### 1.1 Leer el PRD

打开 PRD 文档，重点回答以下问题：

- 系统有几个入口？各自覆盖哪些页面？
- 每个页面的核心功能是什么？
- 后端包含哪些模块和数据库表？
- MVP 范围是什么？第一版哪些做，哪些不做？

::: warning
Si no tienes respuestas claras a las preguntas anteriores, no comiences a escribir codigo. La comprension inadecuada de los requisitos es la causa mas comun de retrabajo.
:::

### 1.2 Confirmar la arquitectura del sistema

根据 PRD 中的描述，梳理出系统的整体架构：

```mermaid
flowchart TD
  prd["PRD"] --> web["Sitio web publico"]
  prd --> app["Espacio de trabajo del usuario"]
  prd --> admin["Panel de administracion"]
  app --> auth["鉴权"]
  app --> gen["图片生成任务"]
  gen --> oss["OSS 对象存储"]
  gen --> db["数据库"]
  billing["支付与套餐"] --> db
  social["分享 / 点赞 / 评论 / 转发"] --> db
  admin --> analytics["SaaS 指标看板"]
  admin --> observability["API / DB / Provider 监控"]
```

建议你用自己的话把架构图画一遍，确认你对系统的理解是完整的。

## Segunda parte：搭建项目骨架

### 2.1 Generar paginas frontend

使用 AI 先生成所有页面的基本结构和假数据。这一步的目标是搭出信息架构和路由，不需要接真实接口。

Referencia de prompts：

```text
请基于当前 PRD，帮我生成一个现代 AI 生图 SaaS 的前端骨架。

要求：
1. 分成三个入口：www、app、admin
2. 官网包括：首页、定价、FAQ
3. app 包括：登录、注册、生成工作台、图库、套餐、积分、社区、作品详情、个人中心
4. admin 包括：后台首页、用户管理、任务管理、内容管理、套餐管理、支付订单、运营配置、SaaS 指标、系统监控
5. 先只生成页面结构和假数据，不接真实接口
6. 风格参考 Midjourney，简洁、现代、带产品感
```

### 2.2 Verificar la estructura de paginas

骨架生成后，Verificar item por item:

- [ ] 三个入口的路由是否独立（`/`、`/app`、`/admin`）
- [ ] 页面数量是否与 PRD 一致
- [ ] 每个页面是否可以正常访问和导航
- [ ] 假数据是否展示了基本的 UI 状态（列表、空状态、表单等）

## Tercera parte：Desarrollo iterativo

### 3.1 Avanzar por modulos

在骨架的基础上，按以下顺序逐模块补充功能：

1. **鉴权**：注册、登录、角色区分
2. **数据库**：数据表创建、读写接口
3. **核心业务**：图片生成任务、结果存储
4. **OSS 存储**：图片上传与访问
5. **支付**：套餐、积分、Stripe 集成
6. **社交互动**：分享、点赞、评论
7. **后台管理**：用户管理、任务管理、内容审核
8. **数据监控**：SaaS 指标看板、系统监控

每完成一个模块，使用下表进行自检：

| Item de verificacion | Metodo de verificacion |
|--------|----------|
| 页面一致性 | 页面数量、入口、功能是否符合 PRD |
| 接口正确性 | 请求参数、返回结构、状态处理是否合理 |
| Aislamiento de permisos | 普通用户和管理员是否互相隔离 |
| Consistencia de datos | 数据库、OSS、支付、积分是否对得上 |
| Demostrabilidad | 是否能给别人完整演示一条业务链路 |

::: tip
Si encuentras que el contenido generado por IA se desvia del PRD, no vuelvas a hacer toda la pagina, simplemente pidele que modifique los modulos especificos.
:::

### 3.2 角色与分工

在迭代过程中，你需要同时扮演三个角色：

- **产品经理**：确认每个模块的功能是否符合 PRD
- **技术负责人**：确认实现方案是否合理
- **测试工程师**：确认功能是否跑得通

## Cuarta parte：联调与上线

### 4.1 Pruebas de extremo a extremo

最后阶段的重点不是补新页面，而是把完整业务链路跑通。Verificar al menos los siguientes escenarios:

- 注册 → 购买积分 → 生成图片 → 查看历史 → 分享互动
- 管理员登录 → 查看用户数据 → 查看任务统计 → 查看系统监控

### 4.2 Despliegue

将项目Despliegue到公网环境，确保：

- 环境变量配置完整
- 登录回调地址正确
- 支付回调地址正确
- 页面无缺失的 loading、空状态、错误提示

Despliegue教程参考：[Git 和 GitHub 工作流](../../backend/git-workflow/)、[如何Despliegue Web 应用](../../backend/zeabur-deployment/)。

## Entregables

Despues de completar este proyecto, necesitas enviar lo siguiente:

- [ ] Enlace de demostracion en linea accesible
- [ ] Enlace al repositorio de codigo fuente (incluyendo README)
- [ ] PRD 文档
- [ ] Capturas de pantalla de paginas clave（官网首页、生图工作台、图库、套餐页、后台首页）
- [ ] 60 segundos de video de demostracion（覆盖注册 → 生成 → 查看 → 后台管理）

README 至少包含：Introduccion del proyecto、核心页面说明、技术栈、本地启动步骤、环境变量清单。

## Criterios de evaluacion

| 维度 | Requisitos basicos | Requisitos avanzados |
|------|---------|---------|
| Alineacion con PRD | 页面、功能、数据结构基本符合 PRD | 能清晰说明每个设计决策与 PRD 的对应关系 |
| Ciclo completo del producto | 注册 → 购买积分 → 生成图片 → 查看历史 → 分享互动可跑通 | 支付状态、积分余额、生成次数数据一致 |
| Capacidades del backend | 用户、任务、支付、内容管理可查看 | SaaS 指标看板和系统监控页完整可用 |
| Completitud de ingenieria | 前端、后端、数据库、OSS、支付链路已接通 | 有错误处理、空状态、loading 状态 |
| Calidad de entrega | 可Despliegue、可运行 | README 清楚、演示视频结构完整 |

## Referencias

- [UI 设计](../../frontend/ui-design/)
- [参考 UI 设计规范设计页面和按钮](../../frontend/multi-product-ui/)
- [用 LLM 和 Skills 让界面变好看](../../frontend/llm-skills-beautiful/)
- [从设计原型到项目代码](../../frontend/design-to-code/)
- [使用现代组件库更新你的界面](../../frontend/modern-component-library/)
- [从数据库到 Supabase](../../backend/database-supabase/)
- [大模型辅助编写接口代码与接口文档](../../backend/ai-interface-code/)
- [Git 和 GitHub 工作流](../../backend/git-workflow/)
- [如何Despliegue Web 应用](../../backend/zeabur-deployment/)
- [如何集成 Stripe 等收费系统](../../backend/stripe-payment/)
