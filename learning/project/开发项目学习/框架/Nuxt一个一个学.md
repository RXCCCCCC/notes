# 路线

![6521f6c36a5a2521e5f9d3a2a4e373bd](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/6521f6c36a5a2521e5f9d3a2a4e373bd.png)

# PS:

- 以开发模式启动您的 Nuxt 应用

	```
	npm create nuxt <project-name>
	npm run dev -- -o
	```

	```bash
	pnpm create nuxt <project-name>
	pnpm dev -o
	```

# [开始使用 Nuxt v4 --- Introduction · Get Started with Nuxt v4](https://nuxt.com/docs/4.x/getting-started/introduction)

The Nuxt server engine [Nitro](https://nitro.build/) unlocks new full-stack capabilities. Nuxt 服务器引擎 Nitro 解锁了新的全栈功能。

In development, it uses Rollup and Node.js workers for your server code and context isolation. It also generates your server API by reading files in `server/api/` and server middleware from `server/middleware/`. 在开发过程中，它使用 Rollup 和 Node.js 工作线程来处理您的服务器代码和上下文隔离。它还通过**读取 `server/api/` 中的文件来生成您的服务器 API**，并**从 `server/middleware/` 中加载服务器中间件**。

A Nuxt application can be deployed on a Node or Deno server, pre-rendered to be hosted in static environments, or deployed to serverless and edge providers. Nuxt 应用程序可以部署在 Node 或 Deno 服务器上，也可以预渲染后**托管在静态环境中**，或者部署到无服务器和边缘提供者上。

# [Installation · Get Started with Nuxt v4](https://nuxt.com/docs/4.x/getting-started/installation)

```bash
pnpm create nuxt@latest <project-name>
cd <project-name>
pnpm dev -o
```

# Configuration 配置

Nuxt is configured with sensible defaults to make you productive. Nuxt 配备了合理的默认设置，助你高效工作。

By default, Nuxt is configured to cover most use cases. The [`nuxt.config.ts`](https://nuxt.com/docs/4.x/guide/directory-structure/nuxt-config) file can override or extend this default configuration. 默认情况下，Nuxt 已配置以覆盖大多数使用场景。 `nuxt.config.ts` 文件可以覆盖或扩展此默认配置。

## Nuxt Configuration Nuxt 配置

The nuxt.config.ts file is located at the root of a Nuxt project and can override or extend the application's behavior. nuxt.config.ts 文件**位于 Nuxt 项目的根目录**，可以**覆盖或扩展**应用程序的行为。

A minimal configuration file exports the defineNuxtConfig function containing an object with your configuration. The defineNuxtConfig helper is globally available without import. 一个最小的配置文件**导出了包含你配置对象的 defineNuxtConfig 函数**。 defineNuxtConfig 辅助函数**无需导入即可全局使用。**

```ts
export default defineNuxtConfig({
  // My Nuxt config
})
```

This file will often be mentioned in the documentation, for example to add custom scripts, register modules or change rendering modes. 这个文件经常会出现在文档中，例如**添加自定义脚本、注册模块或更改渲染模式**。



You don't have to use TypeScript to build an application with Nuxt. However, it is strongly recommended to use the `.ts` extension for the `nuxt.config` file. This way you can benefit from hints in your IDE to avoid typos and mistakes while editing your configuration. 你不必使用 TypeScript 来构建 Nuxt 应用。然而，强烈建议**为 `nuxt.config` 文件使用 `.ts` 扩展**。这样你可以在编辑配置时从 IDE 中获益，避免拼写错误和错误。

### [Environment Overrides 环境覆盖](https://nuxt.com/docs/4.x/getting-started/configuration#environment-overrides)

You can configure fully typed, per-environment overrides in your nuxt.config 你可以在 nuxt.config 中配置完全类型化、**按环境区分的覆盖**。

```ts
export default defineNuxtConfig({
  $production: {
    routeRules: {
      '/**': { isr: true }
    }
  },
  $development: {
    //
  },
  $env: {
    staging: {
      // 
    }
  },
})
```

To select an environment when running a Nuxt CLI command, simply pass the name to the `--envName` flag, like so: `nuxt build --envName staging`. 要**在运行 Nuxt CLI 命令时选择环境，只需将名称传递给 `--envName` 标志，例如： `nuxt build --envName staging` 。**



If you're authoring layers, you can also use the `$meta` key to provide metadata that you or the consumers of your layer might use. 如果你在编写层，你也可以**使用 `$meta` 键来提供**你自己或你的层消费者可能使用的**元数据**。

### [Environment Variables and Private Tokens 环境变量和私有令牌](https://nuxt.com/docs/4.x/getting-started/configuration#environment-variables-and-private-tokens)

The `runtimeConfig` API exposes values like environment variables to the rest of your application. By default, these keys are only available server-side. The keys within `runtimeConfig.public` and `runtimeConfig.app` (which is used by Nuxt internally) are also available client-side. `runtimeConfig` API 将环境变量等值**暴露给应用程序的其他部分**。默认情况下，这些键**仅在服务器端可用**。 `runtimeConfig.public` 和 `runtimeConfig.app` （Nuxt 内部使用）中的键也可在客户端访问。

Those values should be defined in `nuxt.config` and can be overridden using environment variables. 这些**值应在 `nuxt.config` 中定义**，并可通过环境变量进行覆盖。

```TS
export default defineNuxtConfig({
  runtimeConfig: {
    // The private keys which are only available server-side
    apiSecret: '123',
    // Keys within public are also exposed client-side
    public: {
      apiBase: '/api'
    }
  }
})
```

These variables are exposed to the rest of your application using the useRuntimeConfig() composable. 这些变量**通过 useRuntimeConfig() composable 暴露给应用程序的其他部分。**

```VUE
<script setup lang="ts">
const runtimeConfig = useRuntimeConfig()
</script>
```

## [App Configuration 应用配置](https://nuxt.com/docs/4.x/getting-started/configuration#app-configuration)

The `app.config.ts` file, located in the source directory (by default the root of the project), is used to expose public variables that can be determined at build time. Contrary to the `runtimeConfig` option, these cannot be overridden using environment variables. **位于源目录（默认为项目根目录）的 `app.config.ts` 文件**，用于暴露可在构建时确定的公共变量。与 `runtimeConfig` 选项不同，这些变量不能通过环境变量进行覆盖。

A minimal configuration file exports the `defineAppConfig` function containing an object with your configuration. The `defineAppConfig` helper is globally available without import. 一个最小的配置文件导出了包含你配置对象的 `defineAppConfig` 函数。 `defineAppConfig` 辅助函数无需导入即可全局使用。

```TS
export default defineAppConfig({
  title: 'Hello Nuxt',
  theme: {
    dark: true,
    colors: {
      primary: '#ff0000'
    }
  }
})
```

These variables are exposed to the rest of your application using the useAppConfig composable. 这些变量通过 useAppConfig composable 暴露给应用程序的其他部分。

```VUE
<script setup lang="ts">
const appConfig = useAppConfig()
</script>
```

## [`runtimeConfig` vs. `app.config`](https://nuxt.com/docs/4.x/getting-started/configuration#runtimeconfig-vs-appconfig)

As stated above, `runtimeConfig` and `app.config` are both used to expose variables to the rest of your application. To determine whether you should use one or the other, here are some guidelines: 如上所述， `runtimeConfig` 和 `app.config` 都用于将变量暴露给应用程序的其他部分。为了确定您应该使用哪一个，这里有一些指导原则：

- `runtimeConfig`: Private or public tokens that need to be specified after build using environment variables. `runtimeConfig` : 构建后需要**使用环境变量指定的私有或公共令牌**。
- `app.config`: Public tokens that are determined at build time, website configuration such as theme variant, title and any project config that are not sensitive. `app.config` : 在构建时确定的公共令牌，例如网站配置（如主题变体、标题）以及任何非敏感的项目配置。

| Feature 特性                          | `runtimeConfig`  | `app.config`   |
| :------------------------------------ | :--------------- | :------------- |
| Client Side 客户端                    | Hydrated 已加载  | Bundled 已打包 |
| Environment Variables 环境变量        | ✅ Yes ✅ 是       | ❌ No ❌ 否      |
| Reactive 响应式                       | ✅ Yes ✅ 是       | ✅ Yes ✅ 是     |
| Types support 类型支持                | ✅ Partial ✅ 部分 | ✅ Yes ✅ 是     |
| Configuration per Request 按请求配置  | ❌ No ❌ 否        | ✅ Yes ✅ 是     |
| Hot Module Replacement 热模块替换     | ❌ No ❌ 否        | ✅ Yes ✅ 是     |
| Non primitive JS types 非原始 JS 类型 | ❌ No ❌ 否        | ✅ Yes ✅ 是     |

## [External Configuration Files 外部配置文件](https://nuxt.com/docs/4.x/getting-started/configuration#external-configuration-files)

Nuxt uses [`nuxt.config.ts`](https://nuxt.com/docs/4.x/guide/directory-structure/nuxt-config) file as the single source of truth for configurations and skips reading external configuration files. During the course of building your project, you may have a need to configure those. The following table highlights common configurations and, where applicable, how they can be configured with Nuxt. Nuxt 使用 `nuxt.config.ts` 文件**作为配置的唯一真实来源，并跳过读取外部配置文件**。在构建你的项目的过程中，你可能会需要配置这些。下表突出显示了常见的配置，以及如何在适用的情况下使用 Nuxt 进行配置。

| Name 名称                          | Config File 配置文件    | How To Configure 如何配置                                    |
| :--------------------------------- | :---------------------- | :----------------------------------------------------------- |
| [Nitro](https://nitro.build/)      | ~~`nitro.config.ts`~~   | Use [`nitro`](https://nuxt.com/docs/4.x/api/nuxt-config#nitro) key in `nuxt.config` 在 `nuxt.config` 中使用 `nitro` 键 |
| [PostCSS](https://postcss.org/)    | ~~`postcss.config.js`~~ | Use [`postcss`](https://nuxt.com/docs/4.x/api/nuxt-config#postcss) key in `nuxt.config` 使用 `postcss` 键在 `nuxt.config` |
| [Vite](https://vite.dev/)          | ~~`vite.config.ts`~~    | Use [`vite`](https://nuxt.com/docs/4.x/api/nuxt-config#vite) key in `nuxt.config` 使用 `vite` 键在 `nuxt.config` |
| [webpack](https://webpack.js.org/) | ~~`webpack.config.ts`~~ | Use [`webpack`](https://nuxt.com/docs/4.x/api/nuxt-config#webpack-1) key in `nuxt.config` 使用 `webpack` 键在 `nuxt.config` |

Here is a list of other common config files: 以下是其他常见配置文件列表：

| Name 名称                                     | Config File 配置文件  | How To Configure 如何配置                                    |
| :-------------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| [TypeScript](https://www.typescriptlang.org/) | `tsconfig.json`       | [More Info 更多信息](https://nuxt.com/docs/4.x/guide/concepts/typescript#nuxttsconfigjson) |
| [ESLint](https://eslint.org/)                 | `eslint.config.js`    | [More Info 更多信息](https://eslint.org/docs/latest/use/configure/configuration-files) |
| [Prettier](https://prettier.io/)              | `prettier.config.js`  | [More Info 更多信息](https://prettier.io/docs/en/configuration.html) |
| [Stylelint](https://stylelint.io/)            | `stylelint.config.js` | [More Info 更多信息](https://stylelint.io/user-guide/configure) |
| [TailwindCSS](https://tailwindcss.com/)       | `tailwind.config.js`  | [More Info 更多信息](https://tailwindcss.nuxtjs.org/tailwindcss/configuration) |
| [Vitest](https://vitest.dev/)                 | `vitest.config.ts`    | [More Info 更多信息](https://vitest.dev/config/)             |

Here is a list of other common config files: 以下是其他常见配置文件列表：

| Name 名称                                     | Config File 配置文件  | How To Configure 如何配置                                    |
| :-------------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| [TypeScript](https://www.typescriptlang.org/) | `tsconfig.json`       | [More Info 更多信息](https://nuxt.com/docs/4.x/guide/concepts/typescript#nuxttsconfigjson) |
| [ESLint](https://eslint.org/)                 | `eslint.config.js`    | [More Info 更多信息](https://eslint.org/docs/latest/use/configure/configuration-files) |
| [Prettier](https://prettier.io/)              | `prettier.config.js`  | [More Info 更多信息](https://prettier.io/docs/en/configuration.html) |
| [Stylelint](https://stylelint.io/)            | `stylelint.config.js` | [More Info 更多信息](https://stylelint.io/user-guide/configure) |
| [TailwindCSS](https://tailwindcss.com/)       | `tailwind.config.js`  | [More Info 更多信息](https://tailwindcss.nuxtjs.org/tailwindcss/configuration) |
| [Vitest](https://vitest.dev/)                 | `vitest.config.ts`    | [More Info 更多信息](https://vitest.dev/config/)             |

## [Vue Configuration Vue 配置](https://nuxt.com/docs/4.x/getting-started/configuration#vue-configuration)

### [With Vite 使用 Vite](https://nuxt.com/docs/4.x/getting-started/configuration#with-vite)

If you need to pass options to `@vitejs/plugin-vue` or `@vitejs/plugin-vue-jsx`, you can do this in your `nuxt.config` file. 如果你需要向 `@vitejs/plugin-vue` 或 `@vitejs/plugin-vue-jsx` 传递选项，你可以在你的 `nuxt.config` 文件中这样做。

- `vite.vue` for `@vitejs/plugin-vue`. Check [available options](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue). `vite.vue` 用于 `@vitejs/plugin-vue` 。查看可用选项。
- `vite.vueJsx` for `@vitejs/plugin-vue-jsx`. Check [available options](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue-jsx). `vite.vueJsx` 用于 `@vitejs/plugin-vue-jsx` 。查看可用选项。

```ts
export default defineNuxtConfig({
  vite: {
    vue: {
      customElement: true
    },
    vueJsx: {
      mergeProps: true
    }
  }
})
```

### [With webpack 使用 webpack](https://nuxt.com/docs/4.x/getting-started/configuration#with-webpack)

If you use webpack and need to configure `vue-loader`, you can do this using `webpack.loaders.vue` key inside your `nuxt.config` file. The available options are [defined here](https://github.com/vuejs/vue-loader/blob/main/src/index.ts#L32-L62). 如果你使用 webpack 并且需要配置 `vue-loader` ，你可以在 `nuxt.config` 文件中使用 `webpack.loaders.vue` 键来完成。可用的选项定义在此处。

```ts
export default defineNuxtConfig({
  webpack: {
    loaders: {
      vue: {
        hotReload: true,
      }
    }
  }
})
```

# Views 视图

Nuxt provides several component layers to implement the user interface of your application. Nuxt 提供了**多个组件层**来实现你的应用程序的用户界面。

## [`app.vue`](https://nuxt.com/docs/4.x/getting-started/views#appvue)

## [`app.vue`](https://nuxt.com/docs/4.x/getting-started/views#appvue)

![The app.vue file is the entry point of your application](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/app.svg+xml)

By default, Nuxt will treat this file as the **entrypoint** and render its content for every route of the application. 默认情况下，Nuxt 会将此文件视为入口点，并为应用程序的每个路由渲染其内容。

```vue
<template>
  <div>
   <h1>Welcome to the homepage</h1>
  </div>
</template>
```

If you are familiar with Vue, you might wonder where `main.js` is (the file that normally creates a Vue app). Nuxt does this behind the scene. 如果您熟悉 Vue，可能会想知道 `main.js` 在哪里（通常用于创建 Vue 应用的文件）。Nuxt **在后台处理**这些。

## [Components 组件](https://nuxt.com/docs/4.x/getting-started/views#components)

![Components are reusable pieces of UI](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/components.svg+xml)

Most components are reusable pieces of the user interface, like buttons and menus. In Nuxt, you can create these components in the [`components/`](https://nuxt.com/docs/4.x/guide/directory-structure/components) directory, and they will be automatically available across your application without having to explicitly import them. 大多数组件是用户界面的**可重用部分**，如按钮和菜单。在 Nuxt 中，您可以在 `components/` 目录中创建这些组件，它们将自动在整个应用程序中可用，而**无需显式导入**。

```vue
<template>
  <div>
    <h1>Welcome to the homepage</h1>
    <AppAlert>
      This is an auto-imported component.
    </AppAlert>
  </div>
</template>
<template>
  <span>
    <slot />
  </span>
</template>
```

## [Pages 页面](https://nuxt.com/docs/4.x/getting-started/views#pages)

![Pages are views tied to a specific route](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/pages.svg+xml)

Pages represent views for each specific route pattern. Every file in the [`pages/`](https://nuxt.com/docs/4.x/guide/directory-structure/pages) directory represents a different route displaying its content. 页面代表每个**特定路由模式的视图**。 `pages/` 目录中的**每个文件都代表一个不同的路由**，并显示其内容。

To use pages, create `pages/index.vue` file and add `<NuxtPage />` component to the [`app.vue`](https://nuxt.com/docs/4.x/guide/directory-structure/app) (or remove `app.vue` for default entry). You can now create more pages and their corresponding routes by adding new files in the [`pages/`](https://nuxt.com/docs/4.x/guide/directory-structure/pages) directory. 要使用页面，**创建 `pages/index.vue` 文件并将 `<NuxtPage />` 组件添加到 `app.vue`** （或**删除 `app.vue` 以使用默认入口**）。现在您可以通过在 `pages/` 目录中**添加新文件来创建更多页面及其对应的路由**。

```vue
<template>
  <div>
    <h1>Welcome to the homepage</h1>
    <AppAlert>
      This is an auto-imported component
    </AppAlert>
  </div>
</template>
<template>
  <section>
    <p>This page will be displayed at the /about route.</p>
  </section>
</template>
```

## [Layouts 布局](https://nuxt.com/docs/4.x/getting-started/views#layouts)

![Layouts are wrapper around pages](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/layouts.svg+xml)

Layouts are wrappers around pages that contain a common User Interface for several pages, such as header and footer displays. Layouts are Vue files using `<slot />` components to display the **page** content. The `layouts/default.vue` file will be used by default. Custom layouts can be set as part of your page metadata. 布局是围绕页面的一层包装，包含多个页面共有的用户界面，例如页眉和页脚的显示。布局是**使用 `<slot />` 组件显示页面内容的 Vue 文件**。**默认情况下将使用 `layouts/default.vue` 文件**。自定义布局**可以作为页面元数据的一部分设置**。



If you only have a single layout in your application, we recommend using app.vue with <NuxtPage /> instead. 如果你应用中只有一个布局，我们建议使用 **app.vue 与 <NuxtPage /> 代替。**

```vue
<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
<template>
  <div>
    <AppHeader />
    <slot />
    <AppFooter />
  </div>
</template>
<template>
  <div>
    <h1>Welcome to the homepage</h1>
    <AppAlert>
      This is an auto-imported component
    </AppAlert>
  </div>
</template>
<template>
  <section>
    <p>This page will be displayed at the /about route.</p>
  </section>
</template>
```

## [Advanced: Extending the HTML Template 高级：扩展 HTML 模板](https://nuxt.com/docs/4.x/getting-started/views#advanced-extending-the-html-template)

You can have full control over the HTML template by adding a Nitro plugin that registers a hook. The callback function of the render:html hook allows you to mutate the HTML before it is sent to the client. 通过**添加一个注册钩子的 Nitro 插件**，你可以完全**控制 HTML 模板**。 render:html 钩子的**回调函数允许你在 HTML 发送到客户端之前进行修改**。

```ts
export default defineNitroPlugin((nitroApp) => {
  nitroApp.hooks.hook('render:html', (html, { event }) => {
    // This will be an object representation of the html template.
    console.log(html)
    html.head.push(`<meta name="description" content="My custom description" />`)
  })
  // You can also intercept the response here.
  nitroApp.hooks.hook('render:response', (response, { event }) => { console.log(response) })
})
```

# Assets 资源

Nuxt uses two directories to handle assets like stylesheets, fonts or images. Nuxt 使用两个目录来处理**样式表、字体或图像**等资源。

- The [`public/`](https://nuxt.com/docs/4.x/guide/directory-structure/public) directory content is served at the server root as-is. `public/` 目录的内容会原样地在**服务器根目录下**提供。
- The [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory contains by convention every asset that you want the build tool (Vite or webpack) to process. `assets/` 目录**按约定包含**所有你想让构建工具（Vite 或 webpack）处理的**资源**。

## [Public Directory 公共目录](https://nuxt.com/docs/4.x/getting-started/assets#public-directory)

The [`public/`](https://nuxt.com/docs/4.x/guide/directory-structure/public) directory is used as a public server for static assets publicly available at a defined URL of your application. `public/` 目录**用作公共服务器**，用于提供在应用程序定义的 URL 上**公开可用的静态资源**。

You can get a file in the [`public/`](https://nuxt.com/docs/4.x/guide/directory-structure/public) directory from your application's code or from a browser by the root URL `/`. 您可以通过应用程序代码或**通过根 URL `/` 从 `public/` 目录中的文件获取**文件。

### [Example 示例](https://nuxt.com/docs/4.x/getting-started/assets#example)

For example, referencing an image file in the `public/img/` directory, available at the static URL `/img/nuxt.png`: 例如，引用位于 `public/img/` 目录中、可通过**静态 URL `/img/nuxt.png` 访问的图像文件**：

## [Assets Directory 资源目录](https://nuxt.com/docs/4.x/getting-started/assets#assets-directory)

Nuxt uses [Vite](https://vite.dev/guide/assets.html) (default) or [webpack](https://webpack.js.org/guides/asset-management) to build and bundle your application. The main function of these build tools is to process JavaScript files, but they can be extended through [plugins](https://vite.dev/plugins) (for Vite) or [loaders](https://webpack.js.org/loaders) (for webpack) to process other kinds of assets, like stylesheets, fonts or SVGs. This step transforms the original file, mainly for performance or caching purposes (such as stylesheet minification or browser cache invalidation). Nuxt **使用 Vite（默认）或 webpack 来构建和打包**你的应用程序。这些构建工具的主要功能是**处理 JavaScript 文件**，但可以通过插件（用于 Vite）或加载器（用于 webpack）扩展，以处理其他类型的资源，如样式表、字体或 SVG。这一步会转换原始文件，主要目的是为了**性能优化或缓存**（例如样式表压缩或浏览器缓存失效）。

By convention, Nuxt uses the [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory to store these files but there is no auto-scan functionality for this directory, and you can use any other name for it. 按照惯例，Nuxt 使用 `assets/` 目录来存储这些文件，但该目录没有自动扫描功能，你可以使用任何其他名称来命名它。

In your application's code, you can reference a file located in the [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory by using the `~/assets/` path. 在你的应用程序代码中，你可以通过**使用 `~/assets/` 路径来引用(用~就不同于静态/了)**位于 `assets/` 目录中的文件。

### [Example 示例](https://nuxt.com/docs/4.x/getting-started/assets#example-1)

For example, referencing an image file that will be processed if a build tool is configured to handle this file extension: 例如，引用一个图像文件，如果构建工具配置为处理此文件扩展名，该文件将被处理：

```vue
<template>
  <img src="~/assets/img/nuxt.png" alt="Discover Nuxt" />
</template>
```



Nuxt won't serve files in the [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory at a static URL like `/assets/my-file.png`. If you need a static URL, use the [`public/`](https://nuxt.com/docs/4.x/getting-started/assets#public-directory) directory. Nuxt **不会在静态 URL（如 `/assets/my-file.png` ）中提供 `assets/` 目录中的文件**。如果需要静态 URL，请使用 `public/` 目录。

### [Example 示例](https://nuxt.com/docs/4.x/getting-started/assets#example-1)

For example, referencing an image file that will be processed if a build tool is configured to handle this file extension: 例如，引用一个图像文件，如果构建工具配置为处理此文件扩展名，该文件将被处理：

## [Nuxt Configuration Nuxt 配置](https://nuxt.com/docs/4.x/getting-started/configuration#nuxt-configuration)

The [`nuxt.config.ts`](https://nuxt.com/docs/4.x/guide/directory-structure/nuxt-config) file is located at the root of a Nuxt project and can override or extend the application's behavior. `nuxt.config.ts` 文件位于 Nuxt 项目的根目录，可以覆盖或扩展应用程序的行为。

A minimal configuration file exports the `defineNuxtConfig` function containing an object with your configuration. The `defineNuxtConfig` helper is globally available without import. 一个最小的配置文件导出了包含你配置对象的 `defineNuxtConfig` 函数。 `defineNuxtConfig` 辅助函数无需导入即可全局使用。

# Styling 样式

## [Local Stylesheets 本地样式表](https://nuxt.com/docs/4.x/getting-started/styling#local-stylesheets)

If you're writing local stylesheets, the natural place to put them is the [`assets/` directory](https://nuxt.com/docs/4.x/guide/directory-structure/assets). 如果您正在**编写本地样式表**，那么将它们放在 `assets/` 目录是自然的选择。

### [Importing Within Components 在组件中导入](https://nuxt.com/docs/4.x/getting-started/styling#importing-within-components)

You can import stylesheets in your pages, layouts and components directly. You can use a JavaScript import, or a CSS [`@import` statement](https://developer.mozilla.org/en-US/docs/Web/CSS/@import). 您可以直接在页面、布局和组件中导入样式表。您可以使用 **JavaScript 导入，或 CSS `@import` 语句**。

```vue
<script>
// Use a static import for server-side compatibility
import '~/assets/css/first.css'

// Caution: Dynamic imports are not server-side compatible
import('~/assets/css/first.css')
</script>
<style>
@import url("~/assets/css/second.css");
</style>
```



The stylesheets will be inlined in the HTML rendered by Nuxt. 样式表将**内嵌在 Nuxt 渲染的 HTML 中**。

### Working With Fonts 处理字体

Place your local fonts files in your public/ directory, for example in public/fonts. You can then reference them in your stylesheets using url(). 

将你的本地字体文件放在 **public/ 目录中，例如在 public/fonts** 。然后你可以在样式表中**使用 url() 引用它们。**

```CSS
@font-face {
  font-family: 'FarAwayGalaxy';
  src: url('/fonts/FarAwayGalaxy.woff') format('woff');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
```

Then reference your fonts by name in your stylesheets, pages or components: 然后在你的样式表、页面或组件中通过名称引用你的字体：

```CSS
<style>
h1 {
  font-family: 'FarAwayGalaxy', sans-serif;
}
</style>
```

### [Stylesheets Distributed Through NPM 通过 NPM 分发的样式表](https://nuxt.com/docs/4.x/getting-started/styling#stylesheets-distributed-through-npm)

You can also reference stylesheets that are distributed through npm. Let's use the popular `animate.css` library as an example. 你也可以**引用通过 npm 分发的样式表**。让我们以流行的 `animate.css` 库为例。

可以在你的页面、布局和组件中直接引用它：

```VUE
<script>
import 'animate.css'
</script>

<style>
@import url("animate.css");
</style>
```

The package can also be referenced as a string in the css property of your Nuxt configuration. 该包也可以**作为字符串在 Nuxt 配置的 css 属性中引用**。

```ts
export default defineNuxtConfig({
  css: ['animate.css']
})
```

## [External Stylesheets 外部样式表](https://nuxt.com/docs/4.x/getting-started/styling#external-stylesheets)

You can include external stylesheets in your application by adding a link element in the head section of your nuxt.config file. You can achieve this result using different methods. Note that local stylesheets can also be included this way. 您可以通过**在 nuxt.config 文件的 head 部分**添加一个 link 元素来在应用程序中包含外部样式表。您可以使用不同的方法来实现这一结果。请注意，本地样式表也可以用这种方式包含。

You can manipulate the head with the [`app.head`](https://nuxt.com/docs/4.x/api/nuxt-config#head) property of your Nuxt configuration: 您可以使用 Nuxt **配置的 `app.head` 属性来操作 head**：

```ts
export default defineNuxtConfig({
  app: {
    head: {
      link: [{ rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css' }]
    }
  }
})
```

### [Dynamically Adding Stylesheets 动态添加样式表](https://nuxt.com/docs/4.x/getting-started/styling#dynamically-adding-stylesheets)

You can use the useHead composable to dynamically set a value in your head in your code. 您可以使用 **useHead composable** 在代码中**动态地在 head 中设置一个值**。

```
useHead({
  link: [{ rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css' }]
})
```

Nuxt uses `unhead` under the hood

Nuxt 在**底层**使用 `unhead`

## [Single File Components (SFC) Styling 单文件组件（SFC）样式](https://nuxt.com/docs/4.x/getting-started/styling#single-file-components-sfc-styling)

One of the best things about Vue and SFC is how great it is at naturally dealing with styling. You can directly write CSS or preprocessor code in the style block of your components file, therefore you will have fantastic developer experience without having to use something like CSS-in-JS. However if you wish to use CSS-in-JS, you can find 3rd party libraries and modules that support it, such as [pinceau](https://github.com/Tahul/pinceau). Vue 和 SFC 最棒的地方之一就是它们在处理样式方面的出色表现。你可以在组件文件的风格块中**直接编写 CSS 或预处理器代码**，因此你将获得极佳的开发体验，而**无需使用类似 CSS-in-JS 的工具**。然而，如果你希望使用 CSS-in-JS，你可以找到支持它的第三方库和模块，例如 pinceau。

### [Class And Style Bindings 类和样式绑定](https://nuxt.com/docs/4.x/getting-started/styling#class-and-style-bindings)

You can leverage Vue SFC features to style your components with class and style attributes. 你可以利用 Vue **单文件组件（SFC）**的功能，通过类和样式属性来为你的组件添加样式。



# [Key Concepts 关键概念](https://nuxt.com/docs/4.x/guide/concepts)

## Auto-imports 介绍

Nuxt auto-imports components, composables and [Vue.js APIs](https://vuejs.org/api) to use across your application without explicitly importing them. Nuxt **自动导入组件、可组合函数和 Vue.js API**，以便在应用程序中跨组件使用，无需显式导入。

```vue
<script setup lang="ts">
const count = ref(1) // ref is auto-imported
</script>
```

Thanks to its opinionated directory structure, Nuxt can auto-import your components/, composables/ and utils/.

 得益于其约定式的目录结构，Nuxt 可以**自动导入您的 components/ 、 composables/ 和 utils/** 。

Contrary to a classic global declaration, Nuxt preserves typings, IDEs completions and hints, and only includes what is used in your production code. 与传统的全局声明不同，Nuxt 保留类型定义、IDE 完成和提示，并且仅包含生产代码中实际使用的部分。



In the docs, every function that is not explicitly imported is auto-imported by Nuxt and can be used as-is in your code. You can find a reference for auto-imported components, composables and utilities in the [API section](https://nuxt.com/docs/4.x/api). 在文档中，所有未明确导入的函数都会被 Nuxt 自动导入，并且可以直接在您的代码中使用。您可以在 API 部分找到自动导入的组件、可组合函数和工具的参考。



In the [`server`](https://nuxt.com/docs/4.x/guide/directory-structure/server) directory, Nuxt auto-imports exported functions and variables from `server/utils/`. **在 `server` 目录中**，Nuxt 会自动导入从 `server/utils/` 导出的函数和变量。



You can also auto-import functions exported from custom folders or third-party packages by configuring the [`imports`](https://nuxt.com/docs/4.x/api/nuxt-config#imports) section of your `nuxt.config` file. 您还可以通过配置 `nuxt.config` 文件的 `imports` 部分来自动导入来自自定义文件夹或第三方包的函数

## [Built-in Auto-imports 内置自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#built-in-auto-imports)

Nuxt auto-imports functions and composables to perform [data fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching), get access to the [app context](https://nuxt.com/docs/4.x/api/composables/use-nuxt-app) and [runtime config](https://nuxt.com/docs/4.x/guide/going-further/runtime-config), manage [state](https://nuxt.com/docs/4.x/getting-started/state-management) or define components and plugins. Nuxt 自动导入函数和可组合函数以执行数据获取、访问应用上下文和运行时配置、管理状态或定义组件和插件。

```vue
<script setup lang="ts">
/* useFetch() is auto-imported */
const { data, refresh, status } = await useFetch('/api/hello')
</script>
```

Vue exposes Reactivity APIs like `ref` or `computed`, as well as lifecycle hooks and helpers that are auto-imported by Nuxt. Vue 暴露了如 `ref` 或 `computed` 等响应性 API，以及生命周期钩子和辅助函数，这些由 Nuxt **自动导入**。

```vue
<script setup lang="ts">
/* ref() and computed() are auto-imported */
const count = ref(1)
const double = computed(() => count.value * 2)
</script>
```

### [Vue and Nuxt Composables Vue 和 Nuxt 可组合函数](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#vue-and-nuxt-composables)

When you are using the built-in Composition API composables provided by Vue and Nuxt, be aware that many of them rely on being called in the right *context*. 当你使用 Vue 和 Nuxt 提供的内置组合式 API 可组合函数时，请注意其中许多函数依赖于在正确的上下文中调用。

During a component lifecycle, Vue tracks the temporary instance of the current component (and similarly, Nuxt tracks a temporary instance of `nuxtApp`) via a global variable, and then unsets it in the same tick. This is essential when server rendering, both to avoid cross-request state pollution (leaking a shared reference between two users) and to avoid leakage between different components. 在一个组件生命周期中，Vue 通过一个全局变量跟踪当前组件的临时实例（同样地，Nuxt 跟踪 `nuxtApp` 的临时实例），然后在同一 tick 中将其清除。这在服务器渲染时至关重要，既可以避免跨请求状态污染（在两个用户之间泄露共享引用），也可以避免不同组件之间的泄露。

That means that (with very few exceptions) you cannot use them outside a Nuxt plugin, Nuxt route middleware or Vue setup function. On top of that, you must use them synchronously - that is, you cannot use `await` before calling a composable, except within `<script setup>` blocks, within the setup function of a component declared with `defineNuxtComponent`, in `defineNuxtPlugin` or in `defineNuxtRouteMiddleware`, where we perform a transform to keep the synchronous context even after the `await`. 这意味着（除极少数例外情况），你**无法在 Nuxt 插件、Nuxt 路由中间件或 Vue setup 函数之外使用它们**。此外，你必须**同步使用**它们——也就是说，在**调用组合式 API 之前不能使用 `await`** ，**除非在 `<script setup>` 块内**、在用 `defineNuxtComponent` 声明的组件的 setup 函数中、在 `defineNuxtPlugin` 或 `defineNuxtRouteMiddleware` 中，我们在**这些地方执行转换以保持同步上下文**，即使 `await` 之后也是如此。

If you get an error message like `Nuxt instance is unavailable` then it probably means you are calling a Nuxt composable in the wrong place in the Vue or Nuxt lifecycle. 如果你得到一个错误消息，如 `Nuxt instance is unavailable` ，那么很可能意味着你在 Vue 或 Nuxt **生命周期中的错误位置调用**了 Nuxt 组合式 API。



When using a composable that requires the Nuxt context inside a non-SFC component, you need to wrap your component with `defineNuxtComponent` instead of `defineComponent` 当在**非 SFC 组件中**使用**需要 Nuxt 上下文的 composable** 时，您**需要用 `defineNuxtComponent` 包裹组件**，而不是 `defineComponent`

**Example of breaking code: 破坏代码的示例：**

```ts
// trying to access runtime config outside a composable
const config = useRuntimeConfig()
export const useMyComposable = () => {
  // accessing runtime config here
}
```

**Example of working code: 工作代码示例：**

```ts
export const useMyComposable = () => {
  // Because your composable is called in the right place in the lifecycle,
  // useRuntimeConfig will work here
  const config = useRuntimeConfig()
}
```

## [Directory-based Auto-imports 基于目录的自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#directory-based-auto-imports)

Nuxt directly auto-imports files created in defined directories: Nuxt 会直接自动导入**在定义的目录中创建的文件**：

- `components/` for [Vue components](https://nuxt.com/docs/4.x/guide/directory-structure/components).  `components/` 用于 Vue **组件**。
- `composables/` for [Vue composables](https://nuxt.com/docs/4.x/guide/directory-structure/composables).  `composables/` 用于 Vue **可组合函数**。
- `utils/` for helper functions and other utilities. `utils/` 用于**辅助函数和其他工具。**



Auto-imported ref and computed won't be unwrapped in a component .

**自动导入的 `ref` 和 `computed` 不会在组件 `<template>` 中解包。** This is due to how Vue works with refs that aren't top-level to the template. You can read more about it [in the Vue documentation](https://vuejs.org/guide/essentials/reactivity-fundamentals.html#caveat-when-unwrapping-in-templates). 这是因为 Vue 在处理模板中**非顶层引用的方式**。

## Explicit Imports 显式导入

Nuxt exposes every auto-import with the #imports alias that can be used to make the import explicit if needed:

Nuxt 通过 **#imports 别名**公开了每个自动导入，如果需要，可以使用该别名来显式导入：

```vue
<script setup lang="ts">
import { ref, computed } from '#imports'

const count = ref(1)
const double = computed(() => count.value * 2)
</script>
```

### [Partially Disabling Auto-imports 部分禁用自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#partially-disabling-auto-imports)

If you want framework-specific functions like `ref` to remain auto-imported but wish to disable auto-imports for your own code (e.g., custom composables), you can set the `imports.scan` option to `false` in your `nuxt.config.ts` file: 如果你希望框架特定的函数（如 `ref` ）保持自动导入，但希望**禁用你自己的代码（例如自定义组合式 API）的自动导入**，你可以在你的 `nuxt.config.ts` 文件中**将 `imports.scan` 选项设置为 `false`** ：

```ts
export default defineNuxtConfig({
  imports: {
    scan: false
  }
})
```

With this configuration: 使用此配置：

- Framework functions like `ref`, `computed`, or `watch` will still work without needing manual imports. 框架函数如 `ref` 、 `computed` 或 `watch` 仍然可以正常工作，无需手动导入。
- Custom code, such as composables, will need to be manually imported in your files. **自定义代码**，例如可组合函数，需要在文件中手动导入。



- If you structure your project with layers, you will need to explicitly import the composables from each layer, rather than relying on auto-imports. 如果你的项目使用**分层结构**，你需要**显式地从每一层导入可组合项**，而不是依赖自动导入。
- This breaks the layer system’s override feature. If you use `imports.scan: false`, ensure you understand this side-effect and adjust your architecture accordingly. 这会**破坏层系统的覆盖功能**。如果你使用 `imports.scan: false` ，请确保你了解这个副作用，并相应地调整你的架构。

## [Auto-import from Third-Party Packages 从第三方包自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#auto-import-from-third-party-packages)

Nuxt also allows auto-importing from third-party packages. Nuxt 还支持从**第三方包自动导入**。

If you are using the Nuxt module for that package, it is likely that the module has already configured auto-imports for that package. 如果你正在使用该包的 Nuxt 模块，那么该模块很可能已经为该包配置了自动导入。

For example, you could enable the auto-import of the `useI18n` composable from the `vue-i18n` package like this: 例如，你可以像这样启用从 `vue-i18n` 包中 `useI18n` 组合式函数的**自动导入**：

```ts
export default defineNuxtConfig({
  imports: {
    presets: [
      {
        from: 'vue-i18n',
        imports: ['useI18n']
      }
    ]
  }
})
```

# Routing 路由

Nuxt file-system routing creates a route for every file in the pages/ directory. Nuxt 的文件系统路由为 pages/目录中的每个文件创建一个路由。

One core feature of Nuxt is the file system router. Every Vue file inside the [`pages/`](https://nuxt.com/docs/4.x/guide/directory-structure/pages) directory creates a corresponding URL (or route) that displays the contents of the file. By using dynamic imports for each page, Nuxt leverages code-splitting to ship the minimum amount of JavaScript for the requested route. Nuxt 的核心特性之一是文件系统路由器。 `pages/` 目录中的**每个 Vue 文件都会创建一个对应的 URL（或路由）**，用于**显示文件内容**。通过为每个页面使用动态导入，Nuxt 利用代码分割功能，只为请求的路由发送最小量的 JavaScript。

## [Pages 页面](https://nuxt.com/docs/4.x/getting-started/routing#pages)

Nuxt routing is based on [vue-router](https://router.vuejs.org/) and generates the routes from every component created in the [`pages/` directory](https://nuxt.com/docs/4.x/guide/directory-structure/pages), based on their filename. Nuxt 的路由**基于 vue-router**，并根据每**个组件的文件名从 `pages/` 目录中生成的路由**。

This file system routing uses naming conventions to create dynamic and nested routes: 这个文件系统路由使用命名约定来创建动态和嵌套的路由：

```
-| pages/
---| about.vue
---| index.vue
---| posts/
-----| [id].vue
```

## Navigation  导航

The <NuxtLink> component links pages between them. It renders an <a> tag with the href attribute set to the route of the page. Once the application is hydrated, page transitions are performed in JavaScript by updating the browser URL. This prevents full-page refreshes and allows for animated transitions. <NuxtLink> 组件用于在**页面之间建立链接**。它会渲染一个 <a> 标签，并将 **href 属性设置为页面的路由**。应用程序**激活**后，**页面切换通过 JavaScript 完成**，同时更新浏览器的 URL。这**避免了整页刷新**，并支持动画过渡效果。

When a <NuxtLink> enters the viewport on the client side, Nuxt will automatically prefetch components and payload (generated pages) of the linked pages ahead of time, resulting in faster navigation. 当 <NuxtLink> 在客户端视口中进入时，Nuxt 会**自动预取链接页面的组件**和**有效负载（生成的页面）**，从而实现更快的导航。

```vue
<template>
  <header>
    <nav>
      <ul>
        <li><NuxtLink to="/about">关于</NuxtLink></li>
        <li><NuxtLink to="/posts/1">文章 1</NuxtLink></li>
        <li><NuxtLink to="/posts/2">文章 2</NuxtLink></li>
      </ul>
    </nav>
  </header>
</template>
```

## [路由参数](https://nuxt.com.cn/docs/4.x/getting-started/routing/#路由参数)

在 Vue 组件的 `<script setup>` 块**或 `setup()` 方法**中，可以使用 [`useRoute()`](https://nuxt.com.cn/docs/4.x/api/composables/use-route) 组合式 API 来访问当前路由的详细信息。

```vue
<script setup lang="ts">// 使用ts编写
const route = useRoute()

// 访问 /posts/1 时，route.params.id 的值为 1
console.log(route.params.id)
</script>
```

## [路由中间件](https://nuxt.com.cn/docs/4.x/getting-started/routing/#路由中间件)

Nuxt 提供了一个**可自定义的路由中间件框架**，你可以在整个应用中使用它，非常适合提取那些需要在导航到特定路由之前运行的代码。

路由中间件在 Nuxt 应用的 **Vue 部分运行**。尽管名称相似，但它们**与服务器中间件完全不同**，服务器中间件在**应用的 Nitro 服务器部分运行**。

路由中间件有三种类型：

1. **匿名（或内联）**路由中间件，直接定义在使用它们的页面中。
2. **命名**路由中间件，放置在 [`middleware/`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/middleware) 目录中，当在页面上使用时，会通过**异步导入自动加载**。（**注意**：路由中间件的名称会规范化为**短横线命名法**，因此 `someMiddleware` 会变为 `some-middleware`。）
3. **全局**路由中间件，放置在 [`middleware/`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/middleware) 目录中（**带有 `.global` 后缀**），会在**每次路由变化时自动运行**。

以下是保护 `/dashboard` 页面的 `auth` 中间件示例：

```ts
function isAuthenticated(): boolean { return false }
// ---cut---
export default defineNuxtRouteMiddleware((to, from) => {
  // isAuthenticated() 是一个示例方法，用于验证用户是否已认证
  if (isAuthenticated() === false) {
    return navigateTo('/login')
  }
})
<script setup lang="ts">
definePageMeta({
  middleware: 'auth'
})
</script>

<template>
  <h1>欢迎来到你的仪表盘</h1>
</template>
```

## [路由验证](https://nuxt.com.cn/docs/4.x/getting-started/routing/#路由验证)

Nuxt 通过每个需要验证的页面中 **[`definePageMeta()`](https://nuxt.com.cn/docs/4.x/api/utils/define-page-meta) 里的 `validate` 属性**提供**路由验证**功能。

`validate` 属性**接收 `route` 作为参数**。你可以**返回一个布尔值**来确定这**是否是一个可以用当前页面渲染的有效路由**。如果返回 `false`，将导致 404 错误。你也可以直接返回一个包含 `statusCode`/`statusMessage` 的对象来自定义返回的错误。

如果有更复杂的使用场景，你可以改用匿名路由中间件。

```vue
<script setup lang="ts">
definePageMeta({
  validate: async (route) => {
    // 检查 id 是否由数字组成
    return typeof route.params.id === 'string' && /^\d+$/.test(route.params.id)
  }
})
</script>
```

# SEO 和元数据

Nuxt 的头部标签管理由 [Unhead](https://unhead.unjs.io/) 提供支持。它提供了合理的默认值、多个强大的组合式函数以及众多配置选项，帮助你管理应用的头部和 SEO 元数据标签。

## [Nuxt 配置](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#nuxt-配置)

在 [`nuxt.config.ts`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/nuxt-config) 中提供 [`app.head`](https://nuxt.com.cn/docs/4.x/api/nuxt-config#head) 属性，可以**静态地为整个应用定制头部**。

此方法**不允许提供响应式数据**。我们建议在 `app.vue` 中使用 `useHead()`。

在这里**设置不会更改的标签是个好习惯**，例如默认站点标题、语言和 favicon。

```vue
export default defineNuxtConfig({
  app: {
    head: {
      title: 'Nuxt', // 默认备用标题
      htmlAttrs: {
        lang: 'en',
      },
      link: [
        { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
      ]
    }
  }
})
```

### [默认标签](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#默认标签)

Nuxt 默认提供了一些标签，以确保你的网站开箱即用效果良好：

- `viewport`: `width=device-width, initial-scale=1`
- `charset`: `utf-8`

虽然大多数网站无需覆盖这些默认值，但你可以使用键控快捷方式更新它们。

```ts
export default defineNuxtConfig({
  app: {
    head: {
      // 更新 Nuxt 默认值
      charset: 'utf-16',
      viewport: 'width=device-width, initial-scale=1, maximum-scale=1',
    }
  }
})
```

## [`useHead`](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#usehead)

[`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 组合式函数**支持响应式输入**，允许你**以编程方式管理头部标签。**

```vue
<script setup lang="ts">
useHead({
  title: '我的应用',
  meta: [
    { name: 'description', content: '我的精彩网站。' }
  ],
  bodyAttrs: {
    class: 'test'
  },
  script: [ { innerHTML: 'console.log(\'Hello world\')' } ]
})
</script>
```

## [`useSeoMeta`](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#useseometa)

[`useSeoMeta`](https://nuxt.com.cn/docs/4.x/api/composables/use-seo-meta) 组合式函数允许你以**对象形式定义站点的 SEO 元数据标签**，并提供完整的类型安全。

这可以帮助你避免拼写错误和常见错误，例如使用 `name` 而不是 `property`。

```vue
<script setup lang="ts">
useSeoMeta({
  title: '我的精彩网站',
  ogTitle: '我的精彩网站',
  description: '这是我的精彩网站，让我为你详细介绍。',
  ogDescription: '这是我的精彩网站，让我为你详细介绍。',
  ogImage: 'https://example.com/image.png',
  twitterCard: 'summary_large_image',
})
</script>
```

## [组件](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#组件)

虽然在所有情况下都推荐使用 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head)，但你可能更喜欢在**模板中使用组件来定义头部标签。**

Nuxt 为此提供了以下组件：`<Title>`、`<Base>`、`<NoScript>`、`<Style>`、`<Meta>`、`<Link>`、`<Body>`、`<Html>` 和 `<Head>`。注意这些组件的**首字母大写**，以确保**不使用无效的原生 HTML 标签**。

`<Head>` 和 `<Body>` 可以接受**嵌套的元数据标签**（出于美观考虑），但这**不会影响嵌套元数据标签在最终 HTML 中的渲染位置**。

```vue
<script setup lang="ts">
const title = ref('Hello World')
</script>
<template>
  <div>
    <Head>
      <Title>{{ title }}</Title>
      <Meta name="description" :content="title" />
      <Style>
      body { background-color: green; }
      </Style>
    </Head>

    <h1>{{ title }}</h1>
  </div>
</template>
```

建议将你的组件包裹在 `<Head>` 或 `<Html>` 组件中，因为这样标签去重会更直观。

## [类型](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#类型)

以下是用于 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head)、[`app.head`](https://nuxt.com.cn/docs/4.x/api/nuxt-config#head) 和组件的非响应式类型。

```vue
interface MetaObject {
  title?: string
  titleTemplate?: string | ((title?: string) => string)
  templateParams?: Record<string, string | Record<string, string>>
  base?: Base
  link?: Link[]
  meta?: Meta[]
  style?: Style[]
  script?: Script[]
  noscript?: Noscript[];
  htmlAttrs?: HtmlAttributes;
  bodyAttrs?: BodyAttributes;
}
```

## [功能](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#功能)

### [响应式](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#响应式)

所有属性都支持响应式，你可以提供计算属性、getter 或响应式对象

```vue
<script setup lang="ts">
const description = ref('我的精彩网站。')
useHead({
  meta: [
    { name: 'description', content: description }
  ],
})
</script>
```

### [标题模板](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#标题模板)

你可以使用 `titleTemplate` 选项提供动态模板来**自定义站点的标题**。例如，你可以将站点名称添加到每个页面的标题中。

`titleTemplate` 可以是一个**字符串**，其中 `%s` 会被标题替换，或者是一个函数。

如果你想使用函数（以获得完全控制），则无法在 `nuxt.config` 中设置。建议在 `app.vue` 文件中设置，它将应用于站点上的所有页面：

```vue
<script setup lang="ts">
useHead({
  titleTemplate: (titleChunk) => {
    return titleChunk ? `${titleChunk} - 站点标题` : '站点标题';
  }
})
</script>
```

现在，如果你在站点其他页面使用 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 将标题设置为 `我的页面`，浏览器标签中的标题将显示为“我的页面 - 站点标题”。你也可以传递 `null` 以默认显示“站点标题”。

### [模板参数](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#模板参数)

你可以使用 `templateParams` 在 `titleTemplate` 中提供除默认 `%s` 之外的额外占位符，从而实现更动态的标题生成。

```vue
<script setup lang="ts">
useHead({
  titleTemplate: (titleChunk) => {
    return titleChunk ? `${titleChunk} %separator %siteName` : '%siteName';
  },
  templateParams: {
    siteName: '站点标题',
    separator: '-'
  }
})
</script>
```

### [Body 标签](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#body-标签)

对于适用的标签，你可以使用 `tagPosition: 'bodyClose'` 选项将它们追加到 **`<body>` 标签的末尾。**

```vue
<script setup lang="ts">
useHead({
  script: [
    {
      src: 'https://third-party-script.com',
      // 有效选项为：'head' | 'bodyClose' | 'bodyOpen'
      tagPosition: 'bodyClose'
    }
  ]
})
</script>
```

## [示例](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#示例)

### [使用 `definePageMeta`](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#使用-definepagemeta)

在 [`pages/` 目录](https://nuxt.com.cn/docs/4.x/guide/directory-structure/pages) 中，你可以使用 `definePageMeta` 结合 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 根据当前路由设置元数据。

例如，你可以先设置当前页面标题（此标题通过宏在构建时提取，因此无法动态设置）：

```vue
<script setup lang="ts">
definePageMeta({
  title: '某个页面'
})
</script>
```

然后在你的布局文件中，你可以使用之前设置的路由元数据：

```vue
<script setup lang="ts">
const route = useRoute()
useHead({
  meta: [{ property: 'og:title', content: `应用名称 - ${route.meta.title}` }]
})
</script>
```

### [动态标题](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#动态标题)

在下面的示例中，`titleTemplate` 被设置为**带有 `%s` 占位符的字符串或一个函数**，这为 Nuxt 应用的每个路由动态设置页面标题提供了更大的灵活性：

```vue
<script setup lang="ts">
useHead({
  // 作为字符串，
  // 其中 `%s` 会被标题替换
  titleTemplate: '%s - 站点标题',
})
</script>
<script setup lang="ts">
useHead({
  // 或作为函数
  titleTemplate: (productCategory) => {
    return productCategory
      ? `${productCategory} - 站点标题`
      : '站点标题'
  }
})
</script>
```

### [外部 CSS](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#外部-css)

下面的示例展示了如何使用 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 组合式函数的 `link` 属性或使用 `<Link>` 组件启用 Google Fonts：

```vue
<script setup lang="ts">
useHead({
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com'
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto&display=swap',
      crossorigin: ''
    }
  ]
})
</script>
```

# 过渡

Nuxt 利用 Vue 的 <Transition> 组件在页面和布局之间应用过渡效果。

## [页面过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#页面过渡)

你可以启用页面过渡，为所有 [页面](https://nuxt.com.cn/docs/4.x/guide/directory-structure/pages) 自动应用过渡效果。

```ts
export default defineNuxtConfig({
  app: {
    pageTransition: { name: 'page', mode: 'out-in' }
  },
})
```

如果**你同时更改了布局和页面**，此处设置的页面过渡不会运行。相反，你应该设置 [布局过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#layout-transitions)。

要在页面之间添加过渡效果，请在 app.vue 中添加以下 CSS：

```vue
<template>
  <NuxtPage />
</template>

<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.4s;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
  filter: blur(1rem);
}
</style>
```

要为某个页面设置不同的过渡效果，可以在该页面的 [`definePageMeta`](https://nuxt.com.cn/docs/4.x/api/utils/define-page-meta) 中设置 `pageTransition` 键：

```nuxt
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'rotate'
  }
})
</script>
<template>
  <NuxtPage />
</template>

<style>
/* ... */
.rotate-enter-active,
.rotate-leave-active {
  transition: all 0.4s;
}
.rotate-enter-from,
.rotate-leave-to {
  opacity: 0;
  transform: rotate3d(1, 1, 1, 15deg);
}
</style>
```

访问“关于”页面时将添加 3D 旋转效果：

## [布局过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#布局过渡)

你可以启用布局过渡，为所有 [布局](https://nuxt.com.cn/docs/4.x/guide/directory-structure/layouts) 自动应用过渡效果。

```vue
export default defineNuxtConfig({
  app: {
    layoutTransition: { name: 'layout', mode: 'out-in' }
  },
})
```

与 `pageTransition` 类似，你可以使用 `definePageMeta` 为页面组件应用自定义 `layoutTransition`：

```vue
<script setup lang="ts">
definePageMeta({
  layout: 'orange',
  layoutTransition: {
    name: 'slide-in'
  }
})
</script>
```

## [全局设置](https://nuxt.com.cn/docs/4.x/getting-started/transitions#全局设置)

你可以使用 `nuxt.config` **全局自定义**这些默认过渡名称。

`pageTransition` 和 `layoutTransition` 键接受 [`TransitionProps`](https://vuejs.org/api/built-in-components.html#transition) 作为 JSON 可序列化的值，你可以在其中传递 `name`、`mode` 和其他有效的自定义 CSS 过渡属性。

```vue
export default defineNuxtConfig({
  app: {
    pageTransition: {
      name: 'fade',
      mode: 'out-in' // 默认值
    },
    layoutTransition: {
      name: 'slide',
      mode: 'out-in' // 默认值
    }
  }
})
```

要覆盖全局过渡属性，可以使用 `definePageMeta` 为**单个 Nuxt 页面定义页面或布局过渡**，并覆盖在 `nuxt.config` 文件中全局定义的任何页面或布局过渡。

## [禁用过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#禁用过渡)

可以为特定路由禁用 `pageTransition` 和 `layoutTransition`：

```vue
<script setup lang="ts">
definePageMeta({
  pageTransition: false,
  layoutTransition: false
})
</script>
```

或在 `nuxt.config` 中全局禁用：

```ts
export default defineNuxtConfig({
  app: {
    pageTransition: false,
    layoutTransition: false
  }
})
```

## [JavaScript 钩子](https://nuxt.com.cn/docs/4.x/getting-started/transitions#javascript-钩子)

对于高级用例，你可以使用 JavaScript 钩子为 Nuxt 页面创建**高度动态**和**自定义**的过渡效果。

这种方式非常适合使用 JavaScript 动画库，例如 [GSAP](https://gsap.com/)。

```vue
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'custom-flip',
    mode: 'out-in',
    onBeforeEnter: (el) => {
      console.log('进入之前...')
    },
    onEnter: (el, done) => {},
    onAfterEnter: (el) => {}
  }
})
</script>
```

## [动态过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#动态过渡)

要使用条件逻辑应用**动态过渡**，可以利用内联 [中间件](https://nuxt.com.cn/docs/4.x/guide/directory-structure/middleware) 为 `to.meta.pageTransition` 分配不同的过渡名称。

```vue
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'slide-right',
    mode: 'out-in'
  },
  middleware (to, from) {
    if (to.meta.pageTransition && typeof to.meta.pageTransition !== 'boolean')
      to.meta.pageTransition.name = +to.params.id! > +from.params.id! ? 'slide-left' : 'slide-right'
  }
})
</script>

<template>
  <h1>#{{ $route.params.id }}</h1>
</template>

<style>
.slide-left-enter-active,
.slide-left-leave-active,
.slide-right-enter-active,
.slide-right-leave-active {
  transition: all 0.2s;
}
.slide-left-enter-from {
  opacity: 0;
  transform: translate(50px, 0);
}
.slide-left-leave-to {
  opacity: 0;
  transform: translate(-50px, 0);
}
.slide-right-enter-from {
  opacity: 0;
  transform: translate(-50px, 0);
}
.slide-right-leave-to {
  opacity: 0;
  transform: translate(50px, 0);
}
</style>
```

## [使用 NuxtPage 的过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#使用-nuxtpage-的过渡)

当在 `app.vue` 中使用 `<NuxtPage />` 时，可以通过 `transition` 属性配置过渡效果，**以全局启用过渡**。

```vue
<template>
  <div>
    <NuxtLayout>
      <NuxtPage :transition="{
        name: 'bounce',
        mode: 'out-in'
      }" />
    </NuxtLayout>
  </div>
</template>
```

请记住，这种页面过渡**无法通过单个页面上的 `definePageMeta` 覆盖**。

# 数据获取

Nuxt 内置了两个组合式API和一个库，用于在浏览器或服务器环境中执行**数据获取**：`useFetch`、[`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 和 `$fetch`。

简而言之：

- [`$fetch`](https://nuxt.com.cn/docs/4.x/api/utils/dollarfetch) 是发起网络请求的最简单方式。
- [`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 是 `$fetch` 的封装，在[通用渲染](https://nuxt.com.cn/docs/4.x/guide/concepts/rendering#universal-rendering)中**只会获取数据一次**。
- [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 与 `useFetch` 类似，但提供更精细的控制。

`useFetch` 和 `useAsyncData` 共享一组通用选项和模式，我们将在最后几节详细介绍。

## [为什么需要 `useFetch` 和 `useAsyncData`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#为什么需要-usefetch-和-useasyncdata)

Nuxt 是一个**可以在服务器和客户端环境中**运行同构（或通用）代码的框架。如果在 Vue 组件的 setup 函数中使用 [`$fetch` 函数](https://nuxt.com.cn/docs/4.x/api/utils/dollarfetch) 进行数据获取，**可能会导致数据被获取两次：一次在服务器（用于渲染 HTML），另一次在客户端（当 HTML 被激活时）。**这可能会导致激活问题、增加交互时间并引发不可预测的行为。

[`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 和 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 组合式API通过确保如果在服务器上发起了API调用，数据会被转发到客户端的**有效载荷**中，从而解决了这个问题。

有效载荷是一个可通过 [`useNuxtApp().payload`](https://nuxt.com.cn/docs/4.x/api/composables/use-nuxt-app#payload) 访问的 JavaScript 对象。它在客户端用于避免在[激活期间](https://nuxt.com.cn/docs/4.x/guide/concepts/rendering#universal-rendering)在浏览器中重新获取相同的数据。

```vue
<script setup lang="ts">
const { data } = await useFetch('/api/data')

async function handleFormSubmit() {
  const res = await $fetch('/api/submit', {
    method: 'POST',
    body: {
      // 我的表单数据
    }
  })
}
</script>

<template>
  <div v-if="data == undefined">
    无数据
  </div>
  <div v-else>
    <form @submit="handleFormSubmit">
      <!-- 表单输入标签 -->
    </form>
  </div>
</template>
```

在上面的示例中，`useFetch` 会确保**请求在服务器上发生，并正确转发到浏览器**。`$fetch` 没有这种机制，更**适合仅从浏览器发起请求**的场景。

### [Suspense](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#suspense)

Nuxt 在底层使用 Vue 的 <Suspense> 组件，以**防止在所有异步数据可用于视图之前进行导航**。数据获取组合式API可以帮助你利用此功能，并在每次调用时使用最适合的方式。

你可以添加 <NuxtLoadingIndicator> 来在页面导航之间添加进度条。

## [`$fetch`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#fetch)

Nuxt 包含 [ofetch](https://github.com/unjs/ofetch) 库，并在整个应用中自动导入为**全局的 `$fetch` 别名。**

```vue
<script setup lang="ts">
async function addTodo() {
  const todo = await $fetch('/api/todos', {
    method: 'POST',
    body: {
      // 我的待办数据
    }
  })
}
</script>
```

注意，仅使用 `$fetch` 不会提供[网络请求去重和导航阻止](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#the-need-for-usefetch-and-useasyncdata)。 建议在客户端交互（基于事件）时使用 `$fetch`，或者在获取初始组件数据时与 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#useasyncdata) 结合使用。

### [将客户端标头传递到API](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#将客户端标头传递到api)

当在服务器上调用 `useFetch` 时，Nuxt 将**使用 [`useRequestFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-request-fetch) 来代理客户端标头和Cookie**（除了不打算转发的标头，如 `host`）。

```vue
<script setup lang="ts">
const { data } = await useFetch('/api/echo');
</script>
// /api/echo.ts
export default defineEventHandler(event => parseCookies(event))
```

或者，下面的示例展示了如何使用 [`useRequestHeaders`](https://nuxt.com.cn/docs/4.x/api/composables/use-request-headers) **从服务器端请求（源自客户端）访问Cookie并将其发送到API**。使用同构的 `$fetch` 调用，我们确保API端点可以**访问用户浏览器最初发送的相同 `cookie` 标头**。这仅在不使用 `useFetch` 时才需要。

```vue
<script setup lang="ts">
const headers = useRequestHeaders(['cookie'])

async function getCurrentUser() {
  return await $fetch('/api/me', { headers })
}
</script>
```

你也可以使用 [`useRequestFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-request-fetch) 自动将标头代理到调用中。

在将标头代理到外部API之前要非常小心，只包含你需要的标头。并非所有标头都可以安全地绕过，可能会引入不必要的行为。以下是不应代理的常见标头列表：

- `host`、`accept`
- `content-length`、`content-md5`、`content-type`
- `x-forwarded-host`、`x-forwarded-port`、`x-forwarded-proto`
- `cf-connecting-ip`、`cf-ray` ::

## [`useFetch`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#usefetch)

[`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 组合式API在底层使用 `$fetch`，用于在 setup 函数中发起SSR安全的网络请求。

```vue
<script setup lang="ts">
const { data: count } = await useFetch('/api/count')
</script>

<template>
  <p>页面访问量：{{ count }}</p>
</template>
```

这个组合式API是 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 组合式API和 `$fetch` 工具的封装。

## [`useAsyncData`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#useasyncdata)

`useAsyncData` 组合式API负责包装异步逻辑，并在解析后返回结果。

`useFetch(url)` 几乎等同于 `useAsyncData(url, () => event.$fetch(url))`。 这是最常见用例的开发体验优化。

在某些情况下，使用 [`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 组合式API并不合适，例如当CMS或第三方提供自己的查询层时。在这种情况下，你可以使用 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 来包装你的调用，同时仍然保留该组合式API提供的优势。

```vue
<script setup lang="ts">
const { data, error } = await useAsyncData('users', () => myGetFunction('users'))

// 这也是可能的：
const { data, error } = await useAsyncData(() => myGetFunction('users'))
</script>
```

[`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 的**第一个参数是一个唯一键**，**用于缓存第二个参数（查询函数）的响应**。如果直接传递查询函数，这个键**可以忽略，它将自动生成**。

由于自动生成的键仅考虑调用 `useAsyncData` 的文件和行，因此**建议始终创建自己的键**以避免不必要的行为，例如当你创建自己的自定义组合式API来包装 `useAsyncData` 时。

设置键有助于通过 [`useNuxtData`](https://nuxt.com.cn/docs/4.x/api/composables/use-nuxt-data) 在组件之间共享相同的数据，或者[刷新特定数据](https://nuxt.com.cn/docs/4.x/api/utils/refresh-nuxt-data#refresh-specific-data)。

```vue
<script setup lang="ts">
const { id } = useRoute().params

const { data, error } = await useAsyncData(`user:${id}`, () => {
  return myGetFunction('users', { id })
})
</script>
<script setup lang="ts">
const { data: discounts, status } = await useAsyncData('cart-discount', async () => {
  const [coupons, offers] = await Promise.all([
    $fetch('/cart/coupons'),
    $fetch('/cart/offers')
  ])

  return { coupons, offers }
})
// discounts.value.coupons
// discounts.value.offers
</script>
```

`useAsyncData` 用于获取和缓存数据，而不是触发副作用（如调用Pinia actions），因为这可能导致意外行为，例如使用空值重复执行。如果你需要触发副作用，请使用 [`callOnce`](https://nuxt.com.cn/docs/4.x/api/utils/call-once) 工具来实现。

```vue
<script setup lang="ts">
const offersStore = useOffersStore()
// 你不能这样做
await useAsyncData(() => offersStore.getOffer(route.params.slug))
</script>
```

## [返回值](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#返回值)

`useFetch` 和 `useAsyncData` 具有相同的返回值，如下所列。

- `data`：传入的异步函数的结果。
- `refresh`/`execute`：可用于刷新 `handler` 函数返回的数据的函数。
- `clear`：可用于将 `data` 设置为 `undefined`（或如果**提供了 `options.default()` 则设置为其值**）、将 `error` 设置为 `undefined`、将 `status` 设置为 `idle` 并将任何当前挂起的请求标记为已取消的函数。
- `error`：数据获取失败时的错误对象。
- `status`：表示数据请求状态的字符串（`"idle"`、`"pending"`、`"success"`、`"error"`）。

`data`、`error` 和 `status` 是Vue的ref，在 `<script setup>` 中可通过 `.value` 访问

默认情况下，**Nuxt 会等待 `refresh` 完成后才允许再次执行。**

如果你**没有在服务器上获取数据**（例如，设置了 `server: false`），则在**激活完成之前不会获取数据**。这意味着即使你在客户端等待 `useFetch`，在 `<script setup>` 中 `data` 仍将保持为 null。

## [实践指南](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#实践指南)

### [通过POST请求消费SSE（服务器发送事件）](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#通过post请求消费sse服务器发送事件)

通过POST请求消费SSE时，你需要手动处理连接。以下是实现方法：

```ts
// 向SSE端点发起POST请求
const response = await $fetch<ReadableStream>('/chats/ask-ai', {
  method: 'POST',
  body: {
    query: "你好AI，你好吗？",
  },
  responseType: 'stream',
})

// 使用 TextDecoderStream 从响应创建新的 ReadableStream，以文本形式获取数据
const reader = response.pipeThrough(new TextDecoderStream()).getReader()

// 在获取数据时读取数据块
while (true) {
  const { value, done } = await reader.read()

  if (done)
    break

  console.log('收到：', value)
}
```

### [并行请求](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#并行请求)

当请求不相互依赖时，你可以使用 `Promise.all()` 并行发起它们以提高性能。

```ts
const { data } = await useAsyncData(() => {
  return Promise.all([
    $fetch("/api/comments/"), 
    $fetch("/api/author/12")
  ]);
});

const comments = computed(() => data.value?.[0]);
const author = computed(() => data.value?.[1]);
```

# 状态管理

Nuxt 提供了 [`useState`](https://nuxt.com.cn/docs/4.x/api/composables/use-state) 组合式函数，用于在组件之间创建响应式且支持 SSR 的共享状态。

[`useState`](https://nuxt.com.cn/docs/4.x/api/composables/use-state) 是一个**支持 SSR 的 [`ref`](https://vuejs.org/api/reactivity-core.html#ref) 替代方案**。其值**在服务器端渲染后（客户端水合期间）会被保留**，并通过**唯一键在所有组件之间共享**。

## [最佳实践](https://nuxt.com.cn/docs/4.x/getting-started/state-management#最佳实践)

切勿在 `<script setup>` 或 `setup()` 函数外部定义 `const state = ref()`。 例如，执行 `export myState = ref({})` 会导致**状态在服务器上的请求之间共享**，并可能导致内存泄漏。

而是使用 `const useX = () => useState('x')`

## [示例](https://nuxt.com.cn/docs/4.x/getting-started/state-management#示例)

### [基本用法](https://nuxt.com.cn/docs/4.x/getting-started/state-management#基本用法)

在此示例中，我们使用组件本地的计数器状态。任何其他使用 `useState('counter')` 的组件都共享相同的响应式状态。

```vue
<script setup lang="ts">
const counter = useState('counter', () => Math.round(Math.random() * 1000))
</script>

<template>
  <div>
    计数器: {{ counter }}
    <button @click="counter++">
      +
    </button>
    <button @click="counter--">
      -
    </button>
  </div>
</template>
```

### [初始化状态](https://nuxt.com.cn/docs/4.x/getting-started/state-management#初始化状态)

大多数情况下，你可能希望使用异步解析的数据来初始化状态。你可以使用 [`app.vue`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/app) 组件和 [`callOnce`](https://nuxt.com.cn/docs/4.x/api/utils/call-once) 工具函数来实现这一点。

```vue
<script setup lang="ts">
const websiteConfig = useState('config')

await callOnce(async () => {
  websiteConfig.value = await $fetch('https://my-cms.com/api/website-config')
})
</script>
```

## [共享状态](https://nuxt.com.cn/docs/4.x/getting-started/state-management#共享状态)

通过使用 [自动导入的组合式函数](https://nuxt.com.cn/docs/4.x/guide/directory-structure/composables)，我们可以定义**全局类型安全的状态**并在整个应用中导入它们。

```ts
export const useColor = () => useState<string>('color', () => 'pink')
<script setup lang="ts">
// ---cut-start---
const useColor = () => useState<string>('color', () => 'pink')
// ---cut-end---
const color = useColor() // 与 useState('color') 相同
</script>

<template>
  <p>当前颜色: {{ color }}</p>
</template>
```

# 服务器

使用 Nuxt 的服务器框架构建全栈应用。你可以从数据库或其他服务器获取数据、创建 API，甚至生成静态的服务器端内容（如站点地图或 RSS 订阅源）—— 所有这些都可以在单一代码库中完成。

## [由 Nitro 提供支持](https://nuxt.com.cn/docs/4.x/getting-started/server#由-nitro-提供支持)

Nuxt 的服务器基于 [Nitro](https://github.com/nitrojs/nitro)。它最初是为 Nuxt 创建的，但现在是 [UnJS](https://unjs.io/) 的一部分，开放给其他框架使用——甚至可以单独使用。

使用 Nitro 为 Nuxt 带来了强大功能：

- 完全控制应用的服务器端部分
- 在任何提供商上进行通用部署（许多支持零配置）
- 混合渲染

Nitro 内部使用 [h3](https://github.com/h3js/h3)，这是一个为高性能和可移植性而构建的轻量级 H(TTP) 框架。

## [服务器端点和中间件](https://nuxt.com.cn/docs/4.x/getting-started/server#服务器端点和中间件)

你可以轻松管理 Nuxt 应用的服务器端部分，从 API 端点到中间件。

端点和中间件都可以像这样定义：

```vue
export default defineEventHandler(async (event) => {
  // ... 在这里做任何你想做的事情
})
```

你可以直接返回 `text`、`json`、`html` 甚至 `stream`。

与 Nuxt 应用的其他部分一样，它开箱即用地支持**热模块替换**和**自动导入**。

## [通用部署](https://nuxt.com.cn/docs/4.x/getting-started/server#通用部署)

Nitro 提供了将你的 Nuxt 应用部署到任何地方的能力，从裸金属服务器到边缘网络，启动时间仅需几毫秒。这速度非常快！

有超过 15 种预设可用于为不同的云提供商和服务器构建 Nuxt 应用，包括：

- [Cloudflare Workers](https://workers.cloudflare.com/)

- [Netlify Functions](https://www.netlify.com/products/functions)

- [Vercel Edge Network](https://vercel.com/docs/edge-network)

- ## [混合渲染](https://nuxt.com.cn/docs/4.x/getting-started/server#混合渲染)

	Nitro 有一个强大的功能叫做 `routeRules`，它允许你定义一组规则来自定义 Nuxt 应用每个路由的渲染方式（以及更多）。

	nuxt.config.ts

```
export default defineNuxtConfig({
  routeRules: {
    // 为 SEO 目的在构建时生成
    '/': { prerender: true },
    // 缓存 1 小时
    '/api/*': { cache: { maxAge: 60 * 60 } },
    // 重定向以避免 404
    '/old-page': {
      redirect: { to: '/new-page', statusCode: 302 }
    }
    // ...
  }
})

```

此外，还有一些路由规则（例如 `ssr`、`appMiddleware` 和 `noScripts`）是 Nuxt 特有的，用于更改将页面渲染为 HTML 时的行为。

一些路由规则（`appMiddleware`、`redirect` 和 `prerender`）也会影响客户端行为。

Nitro 用于构建服务器端渲染的应用，也用于预渲染。

# 部署

Nuxt 应用程序可以部署在 Node.js 服务器上、预渲染为静态托管，或者部署到无服务器或**边缘（CDN）环境中。**

## [Node.js 服务器](https://nuxt.com.cn/docs/4.x/getting-started/deployment#nodejs-服务器)

探索使用 Nitro 的 Node.js 服务器预设，将其部署到任何 Node 托管环境。

- 如果未指定或自动检测到输出格式，则为**默认输出格式**
- 仅加载渲染请求所需的块，以实现最佳冷启动时间
- 用于将 Nuxt 应用部署到任何 Node.js 托管环境

### [入口点](https://nuxt.com.cn/docs/4.x/getting-started/deployment#入口点)

使用 Node 服务器预设运行 `nuxt build` 时，结果将是一个可直接运行的 Node 服务器入口点。

```vue
node .output/server/index.mjs
```

这将启动您的生产 **Nuxt 服务器**，默认监听 3000 端口。

它支持以下**运行时环境变量**：

- `NITRO_PORT` 或 `PORT`（默认为 `3000`）
- `NITRO_HOST` 或 `HOST`（默认为 `'0.0.0.0'`）
- `NITRO_SSL_CERT` 和 `NITRO_SSL_KEY` - 如果两者都存在，将以 HTTPS 模式启动服务器。在绝大多数情况下，除了测试外不应使用此选项，并且 Nitro 服务器应在终止 SSL 的反向代理（如 nginx 或 Cloudflare）后面运行。

### [PM2](https://nuxt.com.cn/docs/4.x/getting-started/deployment#pm2)

[PM2](https://pm2.keymetrics.io/)（进程管理器 2）是在您的服务器或虚拟机上托管 Nuxt 应用程序的快速简便解决方案。

要使用 `pm2`，请使用 `ecosystem.config.cjs`：

```cjs
module.exports = {
  apps: [
    {
      name: 'NuxtAppName',
      port: '3000',
      exec_mode: 'cluster',
      instances: 'max',
      script: './.output/server/index.mjs'
    }
  ]
}
```

### [集群模式](https://nuxt.com.cn/docs/4.x/getting-started/deployment#集群模式)

您可以使用 `NITRO_PRESET=node_cluster` 来利用 Node.js [cluster](https://nodejs.org/dist/latest/docs/api/cluster.html) 模块实现**多进程性能。**

默认情况下，工作负载会以轮询策略分配给工作进程。

## [静态托管](https://nuxt.com.cn/docs/4.x/getting-started/deployment#静态托管)

将 Nuxt 应用程序部署到任何静态托管服务有两种方式：

- 使用 `ssr: true` 的静态站点生成 (SSG) 在构建时预渲染应用程序的路由。（这是运行 `nuxt generate` 时的默认行为。）它还将生成 `/200.html` 和 `/404.html` 单页应用回退页面，这些页面可以在客户端渲染动态路由或 404 错误（尽管您可能需要在静态主机上配置此功能）。
- 或者，您可以使用 `ssr: false` 预渲染您的站点（静态单页应用）。这将生成包含空 `<div id="__nuxt"></div>` 的 HTML 页面，您的 Vue 应用通常会在此处渲染。这样会失去预渲染站点的许多 SEO 优势，因此建议改用 [``](https://nuxt.com.cn/docs/4.x/api/components/client-only) 来包装站点中无法在服务器端渲染的部分（如果有的话）。

### [仅客户端渲染](https://nuxt.com.cn/docs/4.x/getting-started/deployment#仅客户端渲染)

如果您不想预渲染路由，另一种使用静态托管的方法是在 `nuxt.config` 文件中**将 `ssr` 属性设置为 `false`。**然后，`nuxt generate` 命令将输出一个 `.output/public/index.html` 入口点和 JavaScript 捆绑包，就像经典的客户端 Vue.js 应用程序一样。

## [CDN 代理](https://nuxt.com.cn/docs/4.x/getting-started/deployment#cdn-代理)

在大多数情况下，Nuxt 可以处理不是由 Nuxt 本身生成或创建的第三方内容。但有时此类内容可能会导致问题，尤其是 Cloudflare 的 "Minification and Security Options"。

因此，您应确保在 Cloudflare 中取消选中/禁用以下选项。否则，不必要的重新渲染或 hydration 错误可能会影响您的生产应用程序。

1. Speed > Optimization > Content Optimization > 禁用 "Rocket Loader™"
2. Speed > Optimization > Image Optimization > 禁用 "Mirage"
3. Scrape Shield > 禁用 "Email Address Obfuscation"

通过这些设置，您可以确保 Cloudflare 不会将可能导致意外副作用的脚本注入到您的 Nuxt 应用

# 示例

# Auto Imports

Example of the auto-imports feature in Nuxt with:

- Vue components **in the `components/` directory** are auto-imported and can **be used directly in your templates.**
- Vue composables **in the `composables/` directory** are auto-imported and can be used directly in your templates **and JS/TS files.**
- **JS/TS variables and functions in the `utils/` directory** are auto-imported and can be used directly in your templates and JS/TS files.

# Data Fetching

# State Management

# Meta Tags

## [Middleware · Nuxt Examples v4](https://nuxt.com.cn/docs/4.x/examples/routing/middleware)

## [Pages · Nuxt Examples v4](https://nuxt.com.cn/docs/4.x/examples/routing/pages)

## [Universal Router · Nuxt Examples v4](https://nuxt.com.cn/docs/4.x/examples/routing/universal-router)

# 路线

week1

SEO,过度,数据获取,状态管理,错误处理

week2 开始使用结束,看示例部分

# PS:

- 以开发模式启动您的 Nuxt 应用

	```
	npm create nuxt <project-name>
	npm run dev -- -o
	```

	```bash
	pnpm create nuxt <project-name>
	pnpm dev -o
	```

# [开始使用 Nuxt v4 --- Introduction · Get Started with Nuxt v4](https://nuxt.com/docs/4.x/getting-started/introduction)

The Nuxt server engine [Nitro](https://nitro.build/) unlocks new full-stack capabilities. Nuxt 服务器引擎 Nitro 解锁了新的全栈功能。

In development, it uses Rollup and Node.js workers for your server code and context isolation. It also generates your server API by reading files in `server/api/` and server middleware from `server/middleware/`. 在开发过程中，它使用 Rollup 和 Node.js 工作线程来处理您的服务器代码和上下文隔离。它还通过**读取 `server/api/` 中的文件来生成您的服务器 API**，并**从 `server/middleware/` 中加载服务器中间件**。

A Nuxt application can be deployed on a Node or Deno server, pre-rendered to be hosted in static environments, or deployed to serverless and edge providers. Nuxt 应用程序可以部署在 Node 或 Deno 服务器上，也可以预渲染后**托管在静态环境中**，或者部署到无服务器和边缘提供者上。

# [Installation · Get Started with Nuxt v4](https://nuxt.com/docs/4.x/getting-started/installation)

```bash
pnpm create nuxt@latest <project-name>
cd <project-name>
pnpm dev -o
```

# Configuration 配置

Nuxt is configured with sensible defaults to make you productive. Nuxt 配备了合理的默认设置，助你高效工作。

By default, Nuxt is configured to cover most use cases. The [`nuxt.config.ts`](https://nuxt.com/docs/4.x/guide/directory-structure/nuxt-config) file can override or extend this default configuration. 默认情况下，Nuxt 已配置以覆盖大多数使用场景。 `nuxt.config.ts` 文件可以覆盖或扩展此默认配置。

## Nuxt Configuration Nuxt 配置

The nuxt.config.ts file is located at the root of a Nuxt project and can override or extend the application's behavior. nuxt.config.ts 文件**位于 Nuxt 项目的根目录**，可以**覆盖或扩展**应用程序的行为。

A minimal configuration file exports the defineNuxtConfig function containing an object with your configuration. The defineNuxtConfig helper is globally available without import. 一个最小的配置文件**导出了包含你配置对象的 defineNuxtConfig 函数**。 defineNuxtConfig 辅助函数**无需导入即可全局使用。**

```ts
export default defineNuxtConfig({
  // My Nuxt config
})
```

This file will often be mentioned in the documentation, for example to add custom scripts, register modules or change rendering modes. 这个文件经常会出现在文档中，例如**添加自定义脚本、注册模块或更改渲染模式**。



You don't have to use TypeScript to build an application with Nuxt. However, it is strongly recommended to use the `.ts` extension for the `nuxt.config` file. This way you can benefit from hints in your IDE to avoid typos and mistakes while editing your configuration. 你不必使用 TypeScript 来构建 Nuxt 应用。然而，强烈建议**为 `nuxt.config` 文件使用 `.ts` 扩展**。这样你可以在编辑配置时从 IDE 中获益，避免拼写错误和错误。

### [Environment Overrides 环境覆盖](https://nuxt.com/docs/4.x/getting-started/configuration#environment-overrides)

You can configure fully typed, per-environment overrides in your nuxt.config 你可以在 nuxt.config 中配置完全类型化、**按环境区分的覆盖**。

```ts
export default defineNuxtConfig({
  $production: {
    routeRules: {
      '/**': { isr: true }
    }
  },
  $development: {
    //
  },
  $env: {
    staging: {
      // 
    }
  },
})
```

To select an environment when running a Nuxt CLI command, simply pass the name to the `--envName` flag, like so: `nuxt build --envName staging`. 要**在运行 Nuxt CLI 命令时选择环境，只需将名称传递给 `--envName` 标志，例如： `nuxt build --envName staging` 。**



If you're authoring layers, you can also use the `$meta` key to provide metadata that you or the consumers of your layer might use. 如果你在编写层，你也可以**使用 `$meta` 键来提供**你自己或你的层消费者可能使用的**元数据**。

### [Environment Variables and Private Tokens 环境变量和私有令牌](https://nuxt.com/docs/4.x/getting-started/configuration#environment-variables-and-private-tokens)

The `runtimeConfig` API exposes values like environment variables to the rest of your application. By default, these keys are only available server-side. The keys within `runtimeConfig.public` and `runtimeConfig.app` (which is used by Nuxt internally) are also available client-side. `runtimeConfig` API 将环境变量等值**暴露给应用程序的其他部分**。默认情况下，这些键**仅在服务器端可用**。 `runtimeConfig.public` 和 `runtimeConfig.app` （Nuxt 内部使用）中的键也可在客户端访问。

Those values should be defined in `nuxt.config` and can be overridden using environment variables. 这些**值应在 `nuxt.config` 中定义**，并可通过环境变量进行覆盖。

```TS
export default defineNuxtConfig({
  runtimeConfig: {
    // The private keys which are only available server-side
    apiSecret: '123',
    // Keys within public are also exposed client-side
    public: {
      apiBase: '/api'
    }
  }
})
```

These variables are exposed to the rest of your application using the useRuntimeConfig() composable. 这些变量**通过 useRuntimeConfig() composable 暴露给应用程序的其他部分。**

```VUE
<script setup lang="ts">
const runtimeConfig = useRuntimeConfig()
</script>
```

## [App Configuration 应用配置](https://nuxt.com/docs/4.x/getting-started/configuration#app-configuration)

The `app.config.ts` file, located in the source directory (by default the root of the project), is used to expose public variables that can be determined at build time. Contrary to the `runtimeConfig` option, these cannot be overridden using environment variables. **位于源目录（默认为项目根目录）的 `app.config.ts` 文件**，用于暴露可在构建时确定的公共变量。与 `runtimeConfig` 选项不同，这些变量不能通过环境变量进行覆盖。

A minimal configuration file exports the `defineAppConfig` function containing an object with your configuration. The `defineAppConfig` helper is globally available without import. 一个最小的配置文件导出了包含你配置对象的 `defineAppConfig` 函数。 `defineAppConfig` 辅助函数无需导入即可全局使用。

```TS
export default defineAppConfig({
  title: 'Hello Nuxt',
  theme: {
    dark: true,
    colors: {
      primary: '#ff0000'
    }
  }
})
```

These variables are exposed to the rest of your application using the useAppConfig composable. 这些变量通过 useAppConfig composable 暴露给应用程序的其他部分。

```VUE
<script setup lang="ts">
const appConfig = useAppConfig()
</script>
```

## [`runtimeConfig` vs. `app.config`](https://nuxt.com/docs/4.x/getting-started/configuration#runtimeconfig-vs-appconfig)

As stated above, `runtimeConfig` and `app.config` are both used to expose variables to the rest of your application. To determine whether you should use one or the other, here are some guidelines: 如上所述， `runtimeConfig` 和 `app.config` 都用于将变量暴露给应用程序的其他部分。为了确定您应该使用哪一个，这里有一些指导原则：

- `runtimeConfig`: Private or public tokens that need to be specified after build using environment variables. `runtimeConfig` : 构建后需要**使用环境变量指定的私有或公共令牌**。
- `app.config`: Public tokens that are determined at build time, website configuration such as theme variant, title and any project config that are not sensitive. `app.config` : 在构建时确定的公共令牌，例如网站配置（如主题变体、标题）以及任何非敏感的项目配置。

| Feature 特性                          | `runtimeConfig`  | `app.config`   |
| :------------------------------------ | :--------------- | :------------- |
| Client Side 客户端                    | Hydrated 已加载  | Bundled 已打包 |
| Environment Variables 环境变量        | ✅ Yes ✅ 是       | ❌ No ❌ 否      |
| Reactive 响应式                       | ✅ Yes ✅ 是       | ✅ Yes ✅ 是     |
| Types support 类型支持                | ✅ Partial ✅ 部分 | ✅ Yes ✅ 是     |
| Configuration per Request 按请求配置  | ❌ No ❌ 否        | ✅ Yes ✅ 是     |
| Hot Module Replacement 热模块替换     | ❌ No ❌ 否        | ✅ Yes ✅ 是     |
| Non primitive JS types 非原始 JS 类型 | ❌ No ❌ 否        | ✅ Yes ✅ 是     |

## [External Configuration Files 外部配置文件](https://nuxt.com/docs/4.x/getting-started/configuration#external-configuration-files)

Nuxt uses [`nuxt.config.ts`](https://nuxt.com/docs/4.x/guide/directory-structure/nuxt-config) file as the single source of truth for configurations and skips reading external configuration files. During the course of building your project, you may have a need to configure those. The following table highlights common configurations and, where applicable, how they can be configured with Nuxt. Nuxt 使用 `nuxt.config.ts` 文件**作为配置的唯一真实来源，并跳过读取外部配置文件**。在构建你的项目的过程中，你可能会需要配置这些。下表突出显示了常见的配置，以及如何在适用的情况下使用 Nuxt 进行配置。

| Name 名称                          | Config File 配置文件    | How To Configure 如何配置                                    |
| :--------------------------------- | :---------------------- | :----------------------------------------------------------- |
| [Nitro](https://nitro.build/)      | ~~`nitro.config.ts`~~   | Use [`nitro`](https://nuxt.com/docs/4.x/api/nuxt-config#nitro) key in `nuxt.config` 在 `nuxt.config` 中使用 `nitro` 键 |
| [PostCSS](https://postcss.org/)    | ~~`postcss.config.js`~~ | Use [`postcss`](https://nuxt.com/docs/4.x/api/nuxt-config#postcss) key in `nuxt.config` 使用 `postcss` 键在 `nuxt.config` |
| [Vite](https://vite.dev/)          | ~~`vite.config.ts`~~    | Use [`vite`](https://nuxt.com/docs/4.x/api/nuxt-config#vite) key in `nuxt.config` 使用 `vite` 键在 `nuxt.config` |
| [webpack](https://webpack.js.org/) | ~~`webpack.config.ts`~~ | Use [`webpack`](https://nuxt.com/docs/4.x/api/nuxt-config#webpack-1) key in `nuxt.config` 使用 `webpack` 键在 `nuxt.config` |

Here is a list of other common config files: 以下是其他常见配置文件列表：

| Name 名称                                     | Config File 配置文件  | How To Configure 如何配置                                    |
| :-------------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| [TypeScript](https://www.typescriptlang.org/) | `tsconfig.json`       | [More Info 更多信息](https://nuxt.com/docs/4.x/guide/concepts/typescript#nuxttsconfigjson) |
| [ESLint](https://eslint.org/)                 | `eslint.config.js`    | [More Info 更多信息](https://eslint.org/docs/latest/use/configure/configuration-files) |
| [Prettier](https://prettier.io/)              | `prettier.config.js`  | [More Info 更多信息](https://prettier.io/docs/en/configuration.html) |
| [Stylelint](https://stylelint.io/)            | `stylelint.config.js` | [More Info 更多信息](https://stylelint.io/user-guide/configure) |
| [TailwindCSS](https://tailwindcss.com/)       | `tailwind.config.js`  | [More Info 更多信息](https://tailwindcss.nuxtjs.org/tailwindcss/configuration) |
| [Vitest](https://vitest.dev/)                 | `vitest.config.ts`    | [More Info 更多信息](https://vitest.dev/config/)             |

Here is a list of other common config files: 以下是其他常见配置文件列表：

| Name 名称                                     | Config File 配置文件  | How To Configure 如何配置                                    |
| :-------------------------------------------- | :-------------------- | :----------------------------------------------------------- |
| [TypeScript](https://www.typescriptlang.org/) | `tsconfig.json`       | [More Info 更多信息](https://nuxt.com/docs/4.x/guide/concepts/typescript#nuxttsconfigjson) |
| [ESLint](https://eslint.org/)                 | `eslint.config.js`    | [More Info 更多信息](https://eslint.org/docs/latest/use/configure/configuration-files) |
| [Prettier](https://prettier.io/)              | `prettier.config.js`  | [More Info 更多信息](https://prettier.io/docs/en/configuration.html) |
| [Stylelint](https://stylelint.io/)            | `stylelint.config.js` | [More Info 更多信息](https://stylelint.io/user-guide/configure) |
| [TailwindCSS](https://tailwindcss.com/)       | `tailwind.config.js`  | [More Info 更多信息](https://tailwindcss.nuxtjs.org/tailwindcss/configuration) |
| [Vitest](https://vitest.dev/)                 | `vitest.config.ts`    | [More Info 更多信息](https://vitest.dev/config/)             |

## [Vue Configuration Vue 配置](https://nuxt.com/docs/4.x/getting-started/configuration#vue-configuration)

### [With Vite 使用 Vite](https://nuxt.com/docs/4.x/getting-started/configuration#with-vite)

If you need to pass options to `@vitejs/plugin-vue` or `@vitejs/plugin-vue-jsx`, you can do this in your `nuxt.config` file. 如果你需要向 `@vitejs/plugin-vue` 或 `@vitejs/plugin-vue-jsx` 传递选项，你可以在你的 `nuxt.config` 文件中这样做。

- `vite.vue` for `@vitejs/plugin-vue`. Check [available options](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue). `vite.vue` 用于 `@vitejs/plugin-vue` 。查看可用选项。
- `vite.vueJsx` for `@vitejs/plugin-vue-jsx`. Check [available options](https://github.com/vitejs/vite-plugin-vue/tree/main/packages/plugin-vue-jsx). `vite.vueJsx` 用于 `@vitejs/plugin-vue-jsx` 。查看可用选项。

```ts
export default defineNuxtConfig({
  vite: {
    vue: {
      customElement: true
    },
    vueJsx: {
      mergeProps: true
    }
  }
})
```

### [With webpack 使用 webpack](https://nuxt.com/docs/4.x/getting-started/configuration#with-webpack)

If you use webpack and need to configure `vue-loader`, you can do this using `webpack.loaders.vue` key inside your `nuxt.config` file. The available options are [defined here](https://github.com/vuejs/vue-loader/blob/main/src/index.ts#L32-L62). 如果你使用 webpack 并且需要配置 `vue-loader` ，你可以在 `nuxt.config` 文件中使用 `webpack.loaders.vue` 键来完成。可用的选项定义在此处。

```ts
export default defineNuxtConfig({
  webpack: {
    loaders: {
      vue: {
        hotReload: true,
      }
    }
  }
})
```

# Views 视图

Nuxt provides several component layers to implement the user interface of your application. Nuxt 提供了**多个组件层**来实现你的应用程序的用户界面。

## [`app.vue`](https://nuxt.com/docs/4.x/getting-started/views#appvue)

## [`app.vue`](https://nuxt.com/docs/4.x/getting-started/views#appvue)

![The app.vue file is the entry point of your application](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/app.svg+xml)

By default, Nuxt will treat this file as the **entrypoint** and render its content for every route of the application. 默认情况下，Nuxt 会将此文件视为入口点，并为应用程序的每个路由渲染其内容。

```vue
<template>
  <div>
   <h1>Welcome to the homepage</h1>
  </div>
</template>
```

If you are familiar with Vue, you might wonder where `main.js` is (the file that normally creates a Vue app). Nuxt does this behind the scene. 如果您熟悉 Vue，可能会想知道 `main.js` 在哪里（通常用于创建 Vue 应用的文件）。Nuxt **在后台处理**这些。

## [Components 组件](https://nuxt.com/docs/4.x/getting-started/views#components)

![Components are reusable pieces of UI](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/components.svg+xml)

Most components are reusable pieces of the user interface, like buttons and menus. In Nuxt, you can create these components in the [`components/`](https://nuxt.com/docs/4.x/guide/directory-structure/components) directory, and they will be automatically available across your application without having to explicitly import them. 大多数组件是用户界面的**可重用部分**，如按钮和菜单。在 Nuxt 中，您可以在 `components/` 目录中创建这些组件，它们将自动在整个应用程序中可用，而**无需显式导入**。

```vue
<template>
  <div>
    <h1>Welcome to the homepage</h1>
    <AppAlert>
      This is an auto-imported component.
    </AppAlert>
  </div>
</template>
<template>
  <span>
    <slot />
  </span>
</template>
```

## [Pages 页面](https://nuxt.com/docs/4.x/getting-started/views#pages)

![Pages are views tied to a specific route](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/pages.svg+xml)

Pages represent views for each specific route pattern. Every file in the [`pages/`](https://nuxt.com/docs/4.x/guide/directory-structure/pages) directory represents a different route displaying its content. 页面代表每个**特定路由模式的视图**。 `pages/` 目录中的**每个文件都代表一个不同的路由**，并显示其内容。

To use pages, create `pages/index.vue` file and add `<NuxtPage />` component to the [`app.vue`](https://nuxt.com/docs/4.x/guide/directory-structure/app) (or remove `app.vue` for default entry). You can now create more pages and their corresponding routes by adding new files in the [`pages/`](https://nuxt.com/docs/4.x/guide/directory-structure/pages) directory. 要使用页面，**创建 `pages/index.vue` 文件并将 `<NuxtPage />` 组件添加到 `app.vue`** （或**删除 `app.vue` 以使用默认入口**）。现在您可以通过在 `pages/` 目录中**添加新文件来创建更多页面及其对应的路由**。

```vue
<template>
  <div>
    <h1>Welcome to the homepage</h1>
    <AppAlert>
      This is an auto-imported component
    </AppAlert>
  </div>
</template>
<template>
  <section>
    <p>This page will be displayed at the /about route.</p>
  </section>
</template>
```

## [Layouts 布局](https://nuxt.com/docs/4.x/getting-started/views#layouts)

![Layouts are wrapper around pages](./Nuxt%E4%B8%80%E4%B8%AA%E4%B8%80%E4%B8%AA%E5%AD%A6/layouts.svg+xml)

Layouts are wrappers around pages that contain a common User Interface for several pages, such as header and footer displays. Layouts are Vue files using `<slot />` components to display the **page** content. The `layouts/default.vue` file will be used by default. Custom layouts can be set as part of your page metadata. 布局是围绕页面的一层包装，包含多个页面共有的用户界面，例如页眉和页脚的显示。布局是**使用 `<slot />` 组件显示页面内容的 Vue 文件**。**默认情况下将使用 `layouts/default.vue` 文件**。自定义布局**可以作为页面元数据的一部分设置**。



If you only have a single layout in your application, we recommend using app.vue with <NuxtPage /> instead. 如果你应用中只有一个布局，我们建议使用 **app.vue 与 <NuxtPage /> 代替。**

```vue
<template>
  <div>
    <NuxtLayout>
      <NuxtPage />
    </NuxtLayout>
  </div>
</template>
<template>
  <div>
    <AppHeader />
    <slot />
    <AppFooter />
  </div>
</template>
<template>
  <div>
    <h1>Welcome to the homepage</h1>
    <AppAlert>
      This is an auto-imported component
    </AppAlert>
  </div>
</template>
<template>
  <section>
    <p>This page will be displayed at the /about route.</p>
  </section>
</template>
```

## [Advanced: Extending the HTML Template 高级：扩展 HTML 模板](https://nuxt.com/docs/4.x/getting-started/views#advanced-extending-the-html-template)

You can have full control over the HTML template by adding a Nitro plugin that registers a hook. The callback function of the render:html hook allows you to mutate the HTML before it is sent to the client. 通过**添加一个注册钩子的 Nitro 插件**，你可以完全**控制 HTML 模板**。 render:html 钩子的**回调函数允许你在 HTML 发送到客户端之前进行修改**。

```ts
export default defineNitroPlugin((nitroApp) => {
  nitroApp.hooks.hook('render:html', (html, { event }) => {
    // This will be an object representation of the html template.
    console.log(html)
    html.head.push(`<meta name="description" content="My custom description" />`)
  })
  // You can also intercept the response here.
  nitroApp.hooks.hook('render:response', (response, { event }) => { console.log(response) })
})
```

# Assets 资源

Nuxt uses two directories to handle assets like stylesheets, fonts or images. Nuxt 使用两个目录来处理**样式表、字体或图像**等资源。

- The [`public/`](https://nuxt.com/docs/4.x/guide/directory-structure/public) directory content is served at the server root as-is. `public/` 目录的内容会原样地在**服务器根目录下**提供。
- The [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory contains by convention every asset that you want the build tool (Vite or webpack) to process. `assets/` 目录**按约定包含**所有你想让构建工具（Vite 或 webpack）处理的**资源**。

## [Public Directory 公共目录](https://nuxt.com/docs/4.x/getting-started/assets#public-directory)

The [`public/`](https://nuxt.com/docs/4.x/guide/directory-structure/public) directory is used as a public server for static assets publicly available at a defined URL of your application. `public/` 目录**用作公共服务器**，用于提供在应用程序定义的 URL 上**公开可用的静态资源**。

You can get a file in the [`public/`](https://nuxt.com/docs/4.x/guide/directory-structure/public) directory from your application's code or from a browser by the root URL `/`. 您可以通过应用程序代码或**通过根 URL `/` 从 `public/` 目录中的文件获取**文件。

### [Example 示例](https://nuxt.com/docs/4.x/getting-started/assets#example)

For example, referencing an image file in the `public/img/` directory, available at the static URL `/img/nuxt.png`: 例如，引用位于 `public/img/` 目录中、可通过**静态 URL `/img/nuxt.png` 访问的图像文件**：

## [Assets Directory 资源目录](https://nuxt.com/docs/4.x/getting-started/assets#assets-directory)

Nuxt uses [Vite](https://vite.dev/guide/assets.html) (default) or [webpack](https://webpack.js.org/guides/asset-management) to build and bundle your application. The main function of these build tools is to process JavaScript files, but they can be extended through [plugins](https://vite.dev/plugins) (for Vite) or [loaders](https://webpack.js.org/loaders) (for webpack) to process other kinds of assets, like stylesheets, fonts or SVGs. This step transforms the original file, mainly for performance or caching purposes (such as stylesheet minification or browser cache invalidation). Nuxt **使用 Vite（默认）或 webpack 来构建和打包**你的应用程序。这些构建工具的主要功能是**处理 JavaScript 文件**，但可以通过插件（用于 Vite）或加载器（用于 webpack）扩展，以处理其他类型的资源，如样式表、字体或 SVG。这一步会转换原始文件，主要目的是为了**性能优化或缓存**（例如样式表压缩或浏览器缓存失效）。

By convention, Nuxt uses the [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory to store these files but there is no auto-scan functionality for this directory, and you can use any other name for it. 按照惯例，Nuxt 使用 `assets/` 目录来存储这些文件，但该目录没有自动扫描功能，你可以使用任何其他名称来命名它。

In your application's code, you can reference a file located in the [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory by using the `~/assets/` path. 在你的应用程序代码中，你可以通过**使用 `~/assets/` 路径来引用(用~就不同于静态/了)**位于 `assets/` 目录中的文件。

### [Example 示例](https://nuxt.com/docs/4.x/getting-started/assets#example-1)

For example, referencing an image file that will be processed if a build tool is configured to handle this file extension: 例如，引用一个图像文件，如果构建工具配置为处理此文件扩展名，该文件将被处理：

```vue
<template>
  <img src="~/assets/img/nuxt.png" alt="Discover Nuxt" />
</template>
```



Nuxt won't serve files in the [`assets/`](https://nuxt.com/docs/4.x/guide/directory-structure/assets) directory at a static URL like `/assets/my-file.png`. If you need a static URL, use the [`public/`](https://nuxt.com/docs/4.x/getting-started/assets#public-directory) directory. Nuxt **不会在静态 URL（如 `/assets/my-file.png` ）中提供 `assets/` 目录中的文件**。如果需要静态 URL，请使用 `public/` 目录。

### [Example 示例](https://nuxt.com/docs/4.x/getting-started/assets#example-1)

For example, referencing an image file that will be processed if a build tool is configured to handle this file extension: 例如，引用一个图像文件，如果构建工具配置为处理此文件扩展名，该文件将被处理：

## [Nuxt Configuration Nuxt 配置](https://nuxt.com/docs/4.x/getting-started/configuration#nuxt-configuration)

The [`nuxt.config.ts`](https://nuxt.com/docs/4.x/guide/directory-structure/nuxt-config) file is located at the root of a Nuxt project and can override or extend the application's behavior. `nuxt.config.ts` 文件位于 Nuxt 项目的根目录，可以覆盖或扩展应用程序的行为。

A minimal configuration file exports the `defineNuxtConfig` function containing an object with your configuration. The `defineNuxtConfig` helper is globally available without import. 一个最小的配置文件导出了包含你配置对象的 `defineNuxtConfig` 函数。 `defineNuxtConfig` 辅助函数无需导入即可全局使用。

# Styling 样式

## [Local Stylesheets 本地样式表](https://nuxt.com/docs/4.x/getting-started/styling#local-stylesheets)

If you're writing local stylesheets, the natural place to put them is the [`assets/` directory](https://nuxt.com/docs/4.x/guide/directory-structure/assets). 如果您正在**编写本地样式表**，那么将它们放在 `assets/` 目录是自然的选择。

### [Importing Within Components 在组件中导入](https://nuxt.com/docs/4.x/getting-started/styling#importing-within-components)

You can import stylesheets in your pages, layouts and components directly. You can use a JavaScript import, or a CSS [`@import` statement](https://developer.mozilla.org/en-US/docs/Web/CSS/@import). 您可以直接在页面、布局和组件中导入样式表。您可以使用 **JavaScript 导入，或 CSS `@import` 语句**。

```vue
<script>
// Use a static import for server-side compatibility
import '~/assets/css/first.css'

// Caution: Dynamic imports are not server-side compatible
import('~/assets/css/first.css')
</script>
<style>
@import url("~/assets/css/second.css");
</style>
```



The stylesheets will be inlined in the HTML rendered by Nuxt. 样式表将**内嵌在 Nuxt 渲染的 HTML 中**。

### Working With Fonts 处理字体

Place your local fonts files in your public/ directory, for example in public/fonts. You can then reference them in your stylesheets using url(). 

将你的本地字体文件放在 **public/ 目录中，例如在 public/fonts** 。然后你可以在样式表中**使用 url() 引用它们。**

```CSS
@font-face {
  font-family: 'FarAwayGalaxy';
  src: url('/fonts/FarAwayGalaxy.woff') format('woff');
  font-weight: normal;
  font-style: normal;
  font-display: swap;
}
```

Then reference your fonts by name in your stylesheets, pages or components: 然后在你的样式表、页面或组件中通过名称引用你的字体：

```CSS
<style>
h1 {
  font-family: 'FarAwayGalaxy', sans-serif;
}
</style>
```

### [Stylesheets Distributed Through NPM 通过 NPM 分发的样式表](https://nuxt.com/docs/4.x/getting-started/styling#stylesheets-distributed-through-npm)

You can also reference stylesheets that are distributed through npm. Let's use the popular `animate.css` library as an example. 你也可以**引用通过 npm 分发的样式表**。让我们以流行的 `animate.css` 库为例。

可以在你的页面、布局和组件中直接引用它：

```VUE
<script>
import 'animate.css'
</script>

<style>
@import url("animate.css");
</style>
```

The package can also be referenced as a string in the css property of your Nuxt configuration. 该包也可以**作为字符串在 Nuxt 配置的 css 属性中引用**。

```ts
export default defineNuxtConfig({
  css: ['animate.css']
})
```

## [External Stylesheets 外部样式表](https://nuxt.com/docs/4.x/getting-started/styling#external-stylesheets)

You can include external stylesheets in your application by adding a link element in the head section of your nuxt.config file. You can achieve this result using different methods. Note that local stylesheets can also be included this way. 您可以通过**在 nuxt.config 文件的 head 部分**添加一个 link 元素来在应用程序中包含外部样式表。您可以使用不同的方法来实现这一结果。请注意，本地样式表也可以用这种方式包含。

You can manipulate the head with the [`app.head`](https://nuxt.com/docs/4.x/api/nuxt-config#head) property of your Nuxt configuration: 您可以使用 Nuxt **配置的 `app.head` 属性来操作 head**：

```ts
export default defineNuxtConfig({
  app: {
    head: {
      link: [{ rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css' }]
    }
  }
})
```

### [Dynamically Adding Stylesheets 动态添加样式表](https://nuxt.com/docs/4.x/getting-started/styling#dynamically-adding-stylesheets)

You can use the useHead composable to dynamically set a value in your head in your code. 您可以使用 **useHead composable** 在代码中**动态地在 head 中设置一个值**。

```
useHead({
  link: [{ rel: 'stylesheet', href: 'https://cdnjs.cloudflare.com/ajax/libs/animate.css/4.1.1/animate.min.css' }]
})
```

Nuxt uses `unhead` under the hood

Nuxt 在**底层**使用 `unhead`

## [Single File Components (SFC) Styling 单文件组件（SFC）样式](https://nuxt.com/docs/4.x/getting-started/styling#single-file-components-sfc-styling)

One of the best things about Vue and SFC is how great it is at naturally dealing with styling. You can directly write CSS or preprocessor code in the style block of your components file, therefore you will have fantastic developer experience without having to use something like CSS-in-JS. However if you wish to use CSS-in-JS, you can find 3rd party libraries and modules that support it, such as [pinceau](https://github.com/Tahul/pinceau). Vue 和 SFC 最棒的地方之一就是它们在处理样式方面的出色表现。你可以在组件文件的风格块中**直接编写 CSS 或预处理器代码**，因此你将获得极佳的开发体验，而**无需使用类似 CSS-in-JS 的工具**。然而，如果你希望使用 CSS-in-JS，你可以找到支持它的第三方库和模块，例如 pinceau。

### [Class And Style Bindings 类和样式绑定](https://nuxt.com/docs/4.x/getting-started/styling#class-and-style-bindings)

You can leverage Vue SFC features to style your components with class and style attributes. 你可以利用 Vue **单文件组件（SFC）**的功能，通过类和样式属性来为你的组件添加样式。



# [Key Concepts 关键概念](https://nuxt.com/docs/4.x/guide/concepts)

## Auto-imports 介绍

Nuxt auto-imports components, composables and [Vue.js APIs](https://vuejs.org/api) to use across your application without explicitly importing them. Nuxt **自动导入组件、可组合函数和 Vue.js API**，以便在应用程序中跨组件使用，无需显式导入。

```vue
<script setup lang="ts">
const count = ref(1) // ref is auto-imported
</script>
```

Thanks to its opinionated directory structure, Nuxt can auto-import your components/, composables/ and utils/.

 得益于其约定式的目录结构，Nuxt 可以**自动导入您的 components/ 、 composables/ 和 utils/** 。

Contrary to a classic global declaration, Nuxt preserves typings, IDEs completions and hints, and only includes what is used in your production code. 与传统的全局声明不同，Nuxt 保留类型定义、IDE 完成和提示，并且仅包含生产代码中实际使用的部分。



In the docs, every function that is not explicitly imported is auto-imported by Nuxt and can be used as-is in your code. You can find a reference for auto-imported components, composables and utilities in the [API section](https://nuxt.com/docs/4.x/api). 在文档中，所有未明确导入的函数都会被 Nuxt 自动导入，并且可以直接在您的代码中使用。您可以在 API 部分找到自动导入的组件、可组合函数和工具的参考。



In the [`server`](https://nuxt.com/docs/4.x/guide/directory-structure/server) directory, Nuxt auto-imports exported functions and variables from `server/utils/`. **在 `server` 目录中**，Nuxt 会自动导入从 `server/utils/` 导出的函数和变量。



You can also auto-import functions exported from custom folders or third-party packages by configuring the [`imports`](https://nuxt.com/docs/4.x/api/nuxt-config#imports) section of your `nuxt.config` file. 您还可以通过配置 `nuxt.config` 文件的 `imports` 部分来自动导入来自自定义文件夹或第三方包的函数

## [Built-in Auto-imports 内置自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#built-in-auto-imports)

Nuxt auto-imports functions and composables to perform [data fetching](https://nuxt.com/docs/4.x/getting-started/data-fetching), get access to the [app context](https://nuxt.com/docs/4.x/api/composables/use-nuxt-app) and [runtime config](https://nuxt.com/docs/4.x/guide/going-further/runtime-config), manage [state](https://nuxt.com/docs/4.x/getting-started/state-management) or define components and plugins. Nuxt 自动导入函数和可组合函数以执行数据获取、访问应用上下文和运行时配置、管理状态或定义组件和插件。

```vue
<script setup lang="ts">
/* useFetch() is auto-imported */
const { data, refresh, status } = await useFetch('/api/hello')
</script>
```

Vue exposes Reactivity APIs like `ref` or `computed`, as well as lifecycle hooks and helpers that are auto-imported by Nuxt. Vue 暴露了如 `ref` 或 `computed` 等响应性 API，以及生命周期钩子和辅助函数，这些由 Nuxt **自动导入**。

```vue
<script setup lang="ts">
/* ref() and computed() are auto-imported */
const count = ref(1)
const double = computed(() => count.value * 2)
</script>
```

### [Vue and Nuxt Composables Vue 和 Nuxt 可组合函数](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#vue-and-nuxt-composables)

When you are using the built-in Composition API composables provided by Vue and Nuxt, be aware that many of them rely on being called in the right *context*. 当你使用 Vue 和 Nuxt 提供的内置组合式 API 可组合函数时，请注意其中许多函数依赖于在正确的上下文中调用。

During a component lifecycle, Vue tracks the temporary instance of the current component (and similarly, Nuxt tracks a temporary instance of `nuxtApp`) via a global variable, and then unsets it in the same tick. This is essential when server rendering, both to avoid cross-request state pollution (leaking a shared reference between two users) and to avoid leakage between different components. 在一个组件生命周期中，Vue 通过一个全局变量跟踪当前组件的临时实例（同样地，Nuxt 跟踪 `nuxtApp` 的临时实例），然后在同一 tick 中将其清除。这在服务器渲染时至关重要，既可以避免跨请求状态污染（在两个用户之间泄露共享引用），也可以避免不同组件之间的泄露。

That means that (with very few exceptions) you cannot use them outside a Nuxt plugin, Nuxt route middleware or Vue setup function. On top of that, you must use them synchronously - that is, you cannot use `await` before calling a composable, except within `<script setup>` blocks, within the setup function of a component declared with `defineNuxtComponent`, in `defineNuxtPlugin` or in `defineNuxtRouteMiddleware`, where we perform a transform to keep the synchronous context even after the `await`. 这意味着（除极少数例外情况），你**无法在 Nuxt 插件、Nuxt 路由中间件或 Vue setup 函数之外使用它们**。此外，你必须**同步使用**它们——也就是说，在**调用组合式 API 之前不能使用 `await`** ，**除非在 `<script setup>` 块内**、在用 `defineNuxtComponent` 声明的组件的 setup 函数中、在 `defineNuxtPlugin` 或 `defineNuxtRouteMiddleware` 中，我们在**这些地方执行转换以保持同步上下文**，即使 `await` 之后也是如此。

If you get an error message like `Nuxt instance is unavailable` then it probably means you are calling a Nuxt composable in the wrong place in the Vue or Nuxt lifecycle. 如果你得到一个错误消息，如 `Nuxt instance is unavailable` ，那么很可能意味着你在 Vue 或 Nuxt **生命周期中的错误位置调用**了 Nuxt 组合式 API。



When using a composable that requires the Nuxt context inside a non-SFC component, you need to wrap your component with `defineNuxtComponent` instead of `defineComponent` 当在**非 SFC 组件中**使用**需要 Nuxt 上下文的 composable** 时，您**需要用 `defineNuxtComponent` 包裹组件**，而不是 `defineComponent`

**Example of breaking code: 破坏代码的示例：**

```ts
// trying to access runtime config outside a composable
const config = useRuntimeConfig()
export const useMyComposable = () => {
  // accessing runtime config here
}
```

**Example of working code: 工作代码示例：**

```ts
export const useMyComposable = () => {
  // Because your composable is called in the right place in the lifecycle,
  // useRuntimeConfig will work here
  const config = useRuntimeConfig()
}
```

## [Directory-based Auto-imports 基于目录的自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#directory-based-auto-imports)

Nuxt directly auto-imports files created in defined directories: Nuxt 会直接自动导入**在定义的目录中创建的文件**：

- `components/` for [Vue components](https://nuxt.com/docs/4.x/guide/directory-structure/components).  `components/` 用于 Vue **组件**。
- `composables/` for [Vue composables](https://nuxt.com/docs/4.x/guide/directory-structure/composables).  `composables/` 用于 Vue **可组合函数**。
- `utils/` for helper functions and other utilities. `utils/` 用于**辅助函数和其他工具。**



Auto-imported ref and computed won't be unwrapped in a component .

**自动导入的 `ref` 和 `computed` 不会在组件 `<template>` 中解包。** This is due to how Vue works with refs that aren't top-level to the template. You can read more about it [in the Vue documentation](https://vuejs.org/guide/essentials/reactivity-fundamentals.html#caveat-when-unwrapping-in-templates). 这是因为 Vue 在处理模板中**非顶层引用的方式**。

## Explicit Imports 显式导入

Nuxt exposes every auto-import with the #imports alias that can be used to make the import explicit if needed:

Nuxt 通过 **#imports 别名**公开了每个自动导入，如果需要，可以使用该别名来显式导入：

```vue
<script setup lang="ts">
import { ref, computed } from '#imports'

const count = ref(1)
const double = computed(() => count.value * 2)
</script>
```

### [Partially Disabling Auto-imports 部分禁用自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#partially-disabling-auto-imports)

If you want framework-specific functions like `ref` to remain auto-imported but wish to disable auto-imports for your own code (e.g., custom composables), you can set the `imports.scan` option to `false` in your `nuxt.config.ts` file: 如果你希望框架特定的函数（如 `ref` ）保持自动导入，但希望**禁用你自己的代码（例如自定义组合式 API）的自动导入**，你可以在你的 `nuxt.config.ts` 文件中**将 `imports.scan` 选项设置为 `false`** ：

```ts
export default defineNuxtConfig({
  imports: {
    scan: false
  }
})
```

With this configuration: 使用此配置：

- Framework functions like `ref`, `computed`, or `watch` will still work without needing manual imports. 框架函数如 `ref` 、 `computed` 或 `watch` 仍然可以正常工作，无需手动导入。
- Custom code, such as composables, will need to be manually imported in your files. **自定义代码**，例如可组合函数，需要在文件中手动导入。



- If you structure your project with layers, you will need to explicitly import the composables from each layer, rather than relying on auto-imports. 如果你的项目使用**分层结构**，你需要**显式地从每一层导入可组合项**，而不是依赖自动导入。
- This breaks the layer system’s override feature. If you use `imports.scan: false`, ensure you understand this side-effect and adjust your architecture accordingly. 这会**破坏层系统的覆盖功能**。如果你使用 `imports.scan: false` ，请确保你了解这个副作用，并相应地调整你的架构。

## [Auto-import from Third-Party Packages 从第三方包自动导入](https://nuxt.com/docs/4.x/guide/concepts/auto-imports#auto-import-from-third-party-packages)

Nuxt also allows auto-importing from third-party packages. Nuxt 还支持从**第三方包自动导入**。

If you are using the Nuxt module for that package, it is likely that the module has already configured auto-imports for that package. 如果你正在使用该包的 Nuxt 模块，那么该模块很可能已经为该包配置了自动导入。

For example, you could enable the auto-import of the `useI18n` composable from the `vue-i18n` package like this: 例如，你可以像这样启用从 `vue-i18n` 包中 `useI18n` 组合式函数的**自动导入**：

```ts
export default defineNuxtConfig({
  imports: {
    presets: [
      {
        from: 'vue-i18n',
        imports: ['useI18n']
      }
    ]
  }
})
```

# Routing 路由

Nuxt file-system routing creates a route for every file in the pages/ directory. Nuxt 的文件系统路由为 pages/目录中的每个文件创建一个路由。

One core feature of Nuxt is the file system router. Every Vue file inside the [`pages/`](https://nuxt.com/docs/4.x/guide/directory-structure/pages) directory creates a corresponding URL (or route) that displays the contents of the file. By using dynamic imports for each page, Nuxt leverages code-splitting to ship the minimum amount of JavaScript for the requested route. Nuxt 的核心特性之一是文件系统路由器。 `pages/` 目录中的**每个 Vue 文件都会创建一个对应的 URL（或路由）**，用于**显示文件内容**。通过为每个页面使用动态导入，Nuxt 利用代码分割功能，只为请求的路由发送最小量的 JavaScript。

## [Pages 页面](https://nuxt.com/docs/4.x/getting-started/routing#pages)

Nuxt routing is based on [vue-router](https://router.vuejs.org/) and generates the routes from every component created in the [`pages/` directory](https://nuxt.com/docs/4.x/guide/directory-structure/pages), based on their filename. Nuxt 的路由**基于 vue-router**，并根据每**个组件的文件名从 `pages/` 目录中生成的路由**。

This file system routing uses naming conventions to create dynamic and nested routes: 这个文件系统路由使用命名约定来创建动态和嵌套的路由：

```
-| pages/
---| about.vue
---| index.vue
---| posts/
-----| [id].vue
```

## Navigation  导航

The <NuxtLink> component links pages between them. It renders an <a> tag with the href attribute set to the route of the page. Once the application is hydrated, page transitions are performed in JavaScript by updating the browser URL. This prevents full-page refreshes and allows for animated transitions. <NuxtLink> 组件用于在**页面之间建立链接**。它会渲染一个 <a> 标签，并将 **href 属性设置为页面的路由**。应用程序**激活**后，**页面切换通过 JavaScript 完成**，同时更新浏览器的 URL。这**避免了整页刷新**，并支持动画过渡效果。

When a <NuxtLink> enters the viewport on the client side, Nuxt will automatically prefetch components and payload (generated pages) of the linked pages ahead of time, resulting in faster navigation. 当 <NuxtLink> 在客户端视口中进入时，Nuxt 会**自动预取链接页面的组件**和**有效负载（生成的页面）**，从而实现更快的导航。

```vue
<template>
  <header>
    <nav>
      <ul>
        <li><NuxtLink to="/about">关于</NuxtLink></li>
        <li><NuxtLink to="/posts/1">文章 1</NuxtLink></li>
        <li><NuxtLink to="/posts/2">文章 2</NuxtLink></li>
      </ul>
    </nav>
  </header>
</template>
```

## [路由参数](https://nuxt.com.cn/docs/4.x/getting-started/routing/#路由参数)

在 Vue 组件的 `<script setup>` 块**或 `setup()` 方法**中，可以使用 [`useRoute()`](https://nuxt.com.cn/docs/4.x/api/composables/use-route) 组合式 API 来访问当前路由的详细信息。

```vue
<script setup lang="ts">// 使用ts编写
const route = useRoute()

// 访问 /posts/1 时，route.params.id 的值为 1
console.log(route.params.id)
</script>
```

## [路由中间件](https://nuxt.com.cn/docs/4.x/getting-started/routing/#路由中间件)

Nuxt 提供了一个**可自定义的路由中间件框架**，你可以在整个应用中使用它，非常适合提取那些需要在导航到特定路由之前运行的代码。

路由中间件在 Nuxt 应用的 **Vue 部分运行**。尽管名称相似，但它们**与服务器中间件完全不同**，服务器中间件在**应用的 Nitro 服务器部分运行**。

路由中间件有三种类型：

1. **匿名（或内联）**路由中间件，直接定义在使用它们的页面中。
2. **命名**路由中间件，放置在 [`middleware/`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/middleware) 目录中，当在页面上使用时，会通过**异步导入自动加载**。（**注意**：路由中间件的名称会规范化为**短横线命名法**，因此 `someMiddleware` 会变为 `some-middleware`。）
3. **全局**路由中间件，放置在 [`middleware/`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/middleware) 目录中（**带有 `.global` 后缀**），会在**每次路由变化时自动运行**。

以下是保护 `/dashboard` 页面的 `auth` 中间件示例：

```ts
function isAuthenticated(): boolean { return false }
// ---cut---
export default defineNuxtRouteMiddleware((to, from) => {
  // isAuthenticated() 是一个示例方法，用于验证用户是否已认证
  if (isAuthenticated() === false) {
    return navigateTo('/login')
  }
})
<script setup lang="ts">
definePageMeta({
  middleware: 'auth'
})
</script>

<template>
  <h1>欢迎来到你的仪表盘</h1>
</template>
```

## [路由验证](https://nuxt.com.cn/docs/4.x/getting-started/routing/#路由验证)

Nuxt 通过每个需要验证的页面中 **[`definePageMeta()`](https://nuxt.com.cn/docs/4.x/api/utils/define-page-meta) 里的 `validate` 属性**提供**路由验证**功能。

`validate` 属性**接收 `route` 作为参数**。你可以**返回一个布尔值**来确定这**是否是一个可以用当前页面渲染的有效路由**。如果返回 `false`，将导致 404 错误。你也可以直接返回一个包含 `statusCode`/`statusMessage` 的对象来自定义返回的错误。

如果有更复杂的使用场景，你可以改用匿名路由中间件。

```vue
<script setup lang="ts">
definePageMeta({
  validate: async (route) => {
    // 检查 id 是否由数字组成
    return typeof route.params.id === 'string' && /^\d+$/.test(route.params.id)
  }
})
</script>
```

# SEO 和元数据

Nuxt 的头部标签管理由 [Unhead](https://unhead.unjs.io/) 提供支持。它提供了合理的默认值、多个强大的组合式函数以及众多配置选项，帮助你管理应用的头部和 SEO 元数据标签。

## [Nuxt 配置](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#nuxt-配置)

在 [`nuxt.config.ts`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/nuxt-config) 中提供 [`app.head`](https://nuxt.com.cn/docs/4.x/api/nuxt-config#head) 属性，可以**静态地为整个应用定制头部**。

此方法**不允许提供响应式数据**。我们建议在 `app.vue` 中使用 `useHead()`。

在这里**设置不会更改的标签是个好习惯**，例如默认站点标题、语言和 favicon。

```vue
export default defineNuxtConfig({
  app: {
    head: {
      title: 'Nuxt', // 默认备用标题
      htmlAttrs: {
        lang: 'en',
      },
      link: [
        { rel: 'icon', type: 'image/x-icon', href: '/favicon.ico' },
      ]
    }
  }
})
```

### [默认标签](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#默认标签)

Nuxt 默认提供了一些标签，以确保你的网站开箱即用效果良好：

- `viewport`: `width=device-width, initial-scale=1`
- `charset`: `utf-8`

虽然大多数网站无需覆盖这些默认值，但你可以使用键控快捷方式更新它们。

```ts
export default defineNuxtConfig({
  app: {
    head: {
      // 更新 Nuxt 默认值
      charset: 'utf-16',
      viewport: 'width=device-width, initial-scale=1, maximum-scale=1',
    }
  }
})
```

## [`useHead`](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#usehead)

[`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 组合式函数**支持响应式输入**，允许你**以编程方式管理头部标签。**

```vue
<script setup lang="ts">
useHead({
  title: '我的应用',
  meta: [
    { name: 'description', content: '我的精彩网站。' }
  ],
  bodyAttrs: {
    class: 'test'
  },
  script: [ { innerHTML: 'console.log(\'Hello world\')' } ]
})
</script>
```

## [`useSeoMeta`](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#useseometa)

[`useSeoMeta`](https://nuxt.com.cn/docs/4.x/api/composables/use-seo-meta) 组合式函数允许你以**对象形式定义站点的 SEO 元数据标签**，并提供完整的类型安全。

这可以帮助你避免拼写错误和常见错误，例如使用 `name` 而不是 `property`。

```vue
<script setup lang="ts">
useSeoMeta({
  title: '我的精彩网站',
  ogTitle: '我的精彩网站',
  description: '这是我的精彩网站，让我为你详细介绍。',
  ogDescription: '这是我的精彩网站，让我为你详细介绍。',
  ogImage: 'https://example.com/image.png',
  twitterCard: 'summary_large_image',
})
</script>
```

## [组件](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#组件)

虽然在所有情况下都推荐使用 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head)，但你可能更喜欢在**模板中使用组件来定义头部标签。**

Nuxt 为此提供了以下组件：`<Title>`、`<Base>`、`<NoScript>`、`<Style>`、`<Meta>`、`<Link>`、`<Body>`、`<Html>` 和 `<Head>`。注意这些组件的**首字母大写**，以确保**不使用无效的原生 HTML 标签**。

`<Head>` 和 `<Body>` 可以接受**嵌套的元数据标签**（出于美观考虑），但这**不会影响嵌套元数据标签在最终 HTML 中的渲染位置**。

```vue
<script setup lang="ts">
const title = ref('Hello World')
</script>
<template>
  <div>
    <Head>
      <Title>{{ title }}</Title>
      <Meta name="description" :content="title" />
      <Style>
      body { background-color: green; }
      </Style>
    </Head>

    <h1>{{ title }}</h1>
  </div>
</template>
```

建议将你的组件包裹在 `<Head>` 或 `<Html>` 组件中，因为这样标签去重会更直观。

## [类型](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#类型)

以下是用于 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head)、[`app.head`](https://nuxt.com.cn/docs/4.x/api/nuxt-config#head) 和组件的非响应式类型。

```vue
interface MetaObject {
  title?: string
  titleTemplate?: string | ((title?: string) => string)
  templateParams?: Record<string, string | Record<string, string>>
  base?: Base
  link?: Link[]
  meta?: Meta[]
  style?: Style[]
  script?: Script[]
  noscript?: Noscript[];
  htmlAttrs?: HtmlAttributes;
  bodyAttrs?: BodyAttributes;
}
```

## [功能](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#功能)

### [响应式](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#响应式)

所有属性都支持响应式，你可以提供计算属性、getter 或响应式对象

```vue
<script setup lang="ts">
const description = ref('我的精彩网站。')
useHead({
  meta: [
    { name: 'description', content: description }
  ],
})
</script>
```

### [标题模板](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#标题模板)

你可以使用 `titleTemplate` 选项提供动态模板来**自定义站点的标题**。例如，你可以将站点名称添加到每个页面的标题中。

`titleTemplate` 可以是一个**字符串**，其中 `%s` 会被标题替换，或者是一个函数。

如果你想使用函数（以获得完全控制），则无法在 `nuxt.config` 中设置。建议在 `app.vue` 文件中设置，它将应用于站点上的所有页面：

```vue
<script setup lang="ts">
useHead({
  titleTemplate: (titleChunk) => {
    return titleChunk ? `${titleChunk} - 站点标题` : '站点标题';
  }
})
</script>
```

现在，如果你在站点其他页面使用 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 将标题设置为 `我的页面`，浏览器标签中的标题将显示为“我的页面 - 站点标题”。你也可以传递 `null` 以默认显示“站点标题”。

### [模板参数](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#模板参数)

你可以使用 `templateParams` 在 `titleTemplate` 中提供除默认 `%s` 之外的额外占位符，从而实现更动态的标题生成。

```vue
<script setup lang="ts">
useHead({
  titleTemplate: (titleChunk) => {
    return titleChunk ? `${titleChunk} %separator %siteName` : '%siteName';
  },
  templateParams: {
    siteName: '站点标题',
    separator: '-'
  }
})
</script>
```

### [Body 标签](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#body-标签)

对于适用的标签，你可以使用 `tagPosition: 'bodyClose'` 选项将它们追加到 **`<body>` 标签的末尾。**

```vue
<script setup lang="ts">
useHead({
  script: [
    {
      src: 'https://third-party-script.com',
      // 有效选项为：'head' | 'bodyClose' | 'bodyOpen'
      tagPosition: 'bodyClose'
    }
  ]
})
</script>
```

## [示例](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#示例)

### [使用 `definePageMeta`](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#使用-definepagemeta)

在 [`pages/` 目录](https://nuxt.com.cn/docs/4.x/guide/directory-structure/pages) 中，你可以使用 `definePageMeta` 结合 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 根据当前路由设置元数据。

例如，你可以先设置当前页面标题（此标题通过宏在构建时提取，因此无法动态设置）：

```vue
<script setup lang="ts">
definePageMeta({
  title: '某个页面'
})
</script>
```

然后在你的布局文件中，你可以使用之前设置的路由元数据：

```vue
<script setup lang="ts">
const route = useRoute()
useHead({
  meta: [{ property: 'og:title', content: `应用名称 - ${route.meta.title}` }]
})
</script>
```

### [动态标题](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#动态标题)

在下面的示例中，`titleTemplate` 被设置为**带有 `%s` 占位符的字符串或一个函数**，这为 Nuxt 应用的每个路由动态设置页面标题提供了更大的灵活性：

```vue
<script setup lang="ts">
useHead({
  // 作为字符串，
  // 其中 `%s` 会被标题替换
  titleTemplate: '%s - 站点标题',
})
</script>
<script setup lang="ts">
useHead({
  // 或作为函数
  titleTemplate: (productCategory) => {
    return productCategory
      ? `${productCategory} - 站点标题`
      : '站点标题'
  }
})
</script>
```

### [外部 CSS](https://nuxt.com.cn/docs/4.x/getting-started/seo-meta#外部-css)

下面的示例展示了如何使用 [`useHead`](https://nuxt.com.cn/docs/4.x/api/composables/use-head) 组合式函数的 `link` 属性或使用 `<Link>` 组件启用 Google Fonts：

```vue
<script setup lang="ts">
useHead({
  link: [
    {
      rel: 'preconnect',
      href: 'https://fonts.googleapis.com'
    },
    {
      rel: 'stylesheet',
      href: 'https://fonts.googleapis.com/css2?family=Roboto&display=swap',
      crossorigin: ''
    }
  ]
})
</script>
```

# 过渡

Nuxt 利用 Vue 的 <Transition> 组件在页面和布局之间应用过渡效果。

## [页面过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#页面过渡)

你可以启用页面过渡，为所有 [页面](https://nuxt.com.cn/docs/4.x/guide/directory-structure/pages) 自动应用过渡效果。

```ts
export default defineNuxtConfig({
  app: {
    pageTransition: { name: 'page', mode: 'out-in' }
  },
})
```

如果**你同时更改了布局和页面**，此处设置的页面过渡不会运行。相反，你应该设置 [布局过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#layout-transitions)。

要在页面之间添加过渡效果，请在 app.vue 中添加以下 CSS：

```vue
<template>
  <NuxtPage />
</template>

<style>
.page-enter-active,
.page-leave-active {
  transition: all 0.4s;
}
.page-enter-from,
.page-leave-to {
  opacity: 0;
  filter: blur(1rem);
}
</style>
```

要为某个页面设置不同的过渡效果，可以在该页面的 [`definePageMeta`](https://nuxt.com.cn/docs/4.x/api/utils/define-page-meta) 中设置 `pageTransition` 键：

```nuxt
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'rotate'
  }
})
</script>
<template>
  <NuxtPage />
</template>

<style>
/* ... */
.rotate-enter-active,
.rotate-leave-active {
  transition: all 0.4s;
}
.rotate-enter-from,
.rotate-leave-to {
  opacity: 0;
  transform: rotate3d(1, 1, 1, 15deg);
}
</style>
```

访问“关于”页面时将添加 3D 旋转效果：

## [布局过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#布局过渡)

你可以启用布局过渡，为所有 [布局](https://nuxt.com.cn/docs/4.x/guide/directory-structure/layouts) 自动应用过渡效果。

```vue
export default defineNuxtConfig({
  app: {
    layoutTransition: { name: 'layout', mode: 'out-in' }
  },
})
```

与 `pageTransition` 类似，你可以使用 `definePageMeta` 为页面组件应用自定义 `layoutTransition`：

```vue
<script setup lang="ts">
definePageMeta({
  layout: 'orange',
  layoutTransition: {
    name: 'slide-in'
  }
})
</script>
```

## [全局设置](https://nuxt.com.cn/docs/4.x/getting-started/transitions#全局设置)

你可以使用 `nuxt.config` **全局自定义**这些默认过渡名称。

`pageTransition` 和 `layoutTransition` 键接受 [`TransitionProps`](https://vuejs.org/api/built-in-components.html#transition) 作为 JSON 可序列化的值，你可以在其中传递 `name`、`mode` 和其他有效的自定义 CSS 过渡属性。

```vue
export default defineNuxtConfig({
  app: {
    pageTransition: {
      name: 'fade',
      mode: 'out-in' // 默认值
    },
    layoutTransition: {
      name: 'slide',
      mode: 'out-in' // 默认值
    }
  }
})
```

要覆盖全局过渡属性，可以使用 `definePageMeta` 为**单个 Nuxt 页面定义页面或布局过渡**，并覆盖在 `nuxt.config` 文件中全局定义的任何页面或布局过渡。

## [禁用过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#禁用过渡)

可以为特定路由禁用 `pageTransition` 和 `layoutTransition`：

```vue
<script setup lang="ts">
definePageMeta({
  pageTransition: false,
  layoutTransition: false
})
</script>
```

或在 `nuxt.config` 中全局禁用：

```ts
export default defineNuxtConfig({
  app: {
    pageTransition: false,
    layoutTransition: false
  }
})
```

## [JavaScript 钩子](https://nuxt.com.cn/docs/4.x/getting-started/transitions#javascript-钩子)

对于高级用例，你可以使用 JavaScript 钩子为 Nuxt 页面创建**高度动态**和**自定义**的过渡效果。

这种方式非常适合使用 JavaScript 动画库，例如 [GSAP](https://gsap.com/)。

```vue
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'custom-flip',
    mode: 'out-in',
    onBeforeEnter: (el) => {
      console.log('进入之前...')
    },
    onEnter: (el, done) => {},
    onAfterEnter: (el) => {}
  }
})
</script>
```

## [动态过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#动态过渡)

要使用条件逻辑应用**动态过渡**，可以利用内联 [中间件](https://nuxt.com.cn/docs/4.x/guide/directory-structure/middleware) 为 `to.meta.pageTransition` 分配不同的过渡名称。

```vue
<script setup lang="ts">
definePageMeta({
  pageTransition: {
    name: 'slide-right',
    mode: 'out-in'
  },
  middleware (to, from) {
    if (to.meta.pageTransition && typeof to.meta.pageTransition !== 'boolean')
      to.meta.pageTransition.name = +to.params.id! > +from.params.id! ? 'slide-left' : 'slide-right'
  }
})
</script>

<template>
  <h1>#{{ $route.params.id }}</h1>
</template>

<style>
.slide-left-enter-active,
.slide-left-leave-active,
.slide-right-enter-active,
.slide-right-leave-active {
  transition: all 0.2s;
}
.slide-left-enter-from {
  opacity: 0;
  transform: translate(50px, 0);
}
.slide-left-leave-to {
  opacity: 0;
  transform: translate(-50px, 0);
}
.slide-right-enter-from {
  opacity: 0;
  transform: translate(-50px, 0);
}
.slide-right-leave-to {
  opacity: 0;
  transform: translate(50px, 0);
}
</style>
```

## [使用 NuxtPage 的过渡](https://nuxt.com.cn/docs/4.x/getting-started/transitions#使用-nuxtpage-的过渡)

当在 `app.vue` 中使用 `<NuxtPage />` 时，可以通过 `transition` 属性配置过渡效果，**以全局启用过渡**。

```vue
<template>
  <div>
    <NuxtLayout>
      <NuxtPage :transition="{
        name: 'bounce',
        mode: 'out-in'
      }" />
    </NuxtLayout>
  </div>
</template>
```

请记住，这种页面过渡**无法通过单个页面上的 `definePageMeta` 覆盖**。

# 数据获取

Nuxt 内置了两个组合式API和一个库，用于在浏览器或服务器环境中执行**数据获取**：`useFetch`、[`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 和 `$fetch`。

简而言之：

- [`$fetch`](https://nuxt.com.cn/docs/4.x/api/utils/dollarfetch) 是发起网络请求的最简单方式。
- [`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 是 `$fetch` 的封装，在[通用渲染](https://nuxt.com.cn/docs/4.x/guide/concepts/rendering#universal-rendering)中**只会获取数据一次**。
- [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 与 `useFetch` 类似，但提供更精细的控制。

`useFetch` 和 `useAsyncData` 共享一组通用选项和模式，我们将在最后几节详细介绍。

## [为什么需要 `useFetch` 和 `useAsyncData`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#为什么需要-usefetch-和-useasyncdata)

Nuxt 是一个**可以在服务器和客户端环境中**运行同构（或通用）代码的框架。如果在 Vue 组件的 setup 函数中使用 [`$fetch` 函数](https://nuxt.com.cn/docs/4.x/api/utils/dollarfetch) 进行数据获取，**可能会导致数据被获取两次：一次在服务器（用于渲染 HTML），另一次在客户端（当 HTML 被激活时）。**这可能会导致激活问题、增加交互时间并引发不可预测的行为。

[`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 和 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 组合式API通过确保如果在服务器上发起了API调用，数据会被转发到客户端的**有效载荷**中，从而解决了这个问题。

有效载荷是一个可通过 [`useNuxtApp().payload`](https://nuxt.com.cn/docs/4.x/api/composables/use-nuxt-app#payload) 访问的 JavaScript 对象。它在客户端用于避免在[激活期间](https://nuxt.com.cn/docs/4.x/guide/concepts/rendering#universal-rendering)在浏览器中重新获取相同的数据。

```vue
<script setup lang="ts">
const { data } = await useFetch('/api/data')

async function handleFormSubmit() {
  const res = await $fetch('/api/submit', {
    method: 'POST',
    body: {
      // 我的表单数据
    }
  })
}
</script>

<template>
  <div v-if="data == undefined">
    无数据
  </div>
  <div v-else>
    <form @submit="handleFormSubmit">
      <!-- 表单输入标签 -->
    </form>
  </div>
</template>
```

在上面的示例中，`useFetch` 会确保**请求在服务器上发生，并正确转发到浏览器**。`$fetch` 没有这种机制，更**适合仅从浏览器发起请求**的场景。

### [Suspense](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#suspense)

Nuxt 在底层使用 Vue 的 <Suspense> 组件，以**防止在所有异步数据可用于视图之前进行导航**。数据获取组合式API可以帮助你利用此功能，并在每次调用时使用最适合的方式。

你可以添加 <NuxtLoadingIndicator> 来在页面导航之间添加进度条。

## [`$fetch`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#fetch)

Nuxt 包含 [ofetch](https://github.com/unjs/ofetch) 库，并在整个应用中自动导入为**全局的 `$fetch` 别名。**

```vue
<script setup lang="ts">
async function addTodo() {
  const todo = await $fetch('/api/todos', {
    method: 'POST',
    body: {
      // 我的待办数据
    }
  })
}
</script>
```

注意，仅使用 `$fetch` 不会提供[网络请求去重和导航阻止](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#the-need-for-usefetch-and-useasyncdata)。 建议在客户端交互（基于事件）时使用 `$fetch`，或者在获取初始组件数据时与 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#useasyncdata) 结合使用。

### [将客户端标头传递到API](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#将客户端标头传递到api)

当在服务器上调用 `useFetch` 时，Nuxt 将**使用 [`useRequestFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-request-fetch) 来代理客户端标头和Cookie**（除了不打算转发的标头，如 `host`）。

```vue
<script setup lang="ts">
const { data } = await useFetch('/api/echo');
</script>
// /api/echo.ts
export default defineEventHandler(event => parseCookies(event))
```

或者，下面的示例展示了如何使用 [`useRequestHeaders`](https://nuxt.com.cn/docs/4.x/api/composables/use-request-headers) **从服务器端请求（源自客户端）访问Cookie并将其发送到API**。使用同构的 `$fetch` 调用，我们确保API端点可以**访问用户浏览器最初发送的相同 `cookie` 标头**。这仅在不使用 `useFetch` 时才需要。

```vue
<script setup lang="ts">
const headers = useRequestHeaders(['cookie'])

async function getCurrentUser() {
  return await $fetch('/api/me', { headers })
}
</script>
```

你也可以使用 [`useRequestFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-request-fetch) 自动将标头代理到调用中。

在将标头代理到外部API之前要非常小心，只包含你需要的标头。并非所有标头都可以安全地绕过，可能会引入不必要的行为。以下是不应代理的常见标头列表：

- `host`、`accept`
- `content-length`、`content-md5`、`content-type`
- `x-forwarded-host`、`x-forwarded-port`、`x-forwarded-proto`
- `cf-connecting-ip`、`cf-ray` ::

## [`useFetch`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#usefetch)

[`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 组合式API在底层使用 `$fetch`，用于在 setup 函数中发起SSR安全的网络请求。

```vue
<script setup lang="ts">
const { data: count } = await useFetch('/api/count')
</script>

<template>
  <p>页面访问量：{{ count }}</p>
</template>
```

这个组合式API是 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 组合式API和 `$fetch` 工具的封装。

## [`useAsyncData`](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#useasyncdata)

`useAsyncData` 组合式API负责包装异步逻辑，并在解析后返回结果。

`useFetch(url)` 几乎等同于 `useAsyncData(url, () => event.$fetch(url))`。 这是最常见用例的开发体验优化。

在某些情况下，使用 [`useFetch`](https://nuxt.com.cn/docs/4.x/api/composables/use-fetch) 组合式API并不合适，例如当CMS或第三方提供自己的查询层时。在这种情况下，你可以使用 [`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 来包装你的调用，同时仍然保留该组合式API提供的优势。

```vue
<script setup lang="ts">
const { data, error } = await useAsyncData('users', () => myGetFunction('users'))

// 这也是可能的：
const { data, error } = await useAsyncData(() => myGetFunction('users'))
</script>
```

[`useAsyncData`](https://nuxt.com.cn/docs/4.x/api/composables/use-async-data) 的**第一个参数是一个唯一键**，**用于缓存第二个参数（查询函数）的响应**。如果直接传递查询函数，这个键**可以忽略，它将自动生成**。

由于自动生成的键仅考虑调用 `useAsyncData` 的文件和行，因此**建议始终创建自己的键**以避免不必要的行为，例如当你创建自己的自定义组合式API来包装 `useAsyncData` 时。

设置键有助于通过 [`useNuxtData`](https://nuxt.com.cn/docs/4.x/api/composables/use-nuxt-data) 在组件之间共享相同的数据，或者[刷新特定数据](https://nuxt.com.cn/docs/4.x/api/utils/refresh-nuxt-data#refresh-specific-data)。

```vue
<script setup lang="ts">
const { id } = useRoute().params

const { data, error } = await useAsyncData(`user:${id}`, () => {
  return myGetFunction('users', { id })
})
</script>
<script setup lang="ts">
const { data: discounts, status } = await useAsyncData('cart-discount', async () => {
  const [coupons, offers] = await Promise.all([
    $fetch('/cart/coupons'),
    $fetch('/cart/offers')
  ])

  return { coupons, offers }
})
// discounts.value.coupons
// discounts.value.offers
</script>
```

`useAsyncData` 用于获取和缓存数据，而不是触发副作用（如调用Pinia actions），因为这可能导致意外行为，例如使用空值重复执行。如果你需要触发副作用，请使用 [`callOnce`](https://nuxt.com.cn/docs/4.x/api/utils/call-once) 工具来实现。

```vue
<script setup lang="ts">
const offersStore = useOffersStore()
// 你不能这样做
await useAsyncData(() => offersStore.getOffer(route.params.slug))
</script>
```

## [返回值](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#返回值)

`useFetch` 和 `useAsyncData` 具有相同的返回值，如下所列。

- `data`：传入的异步函数的结果。
- `refresh`/`execute`：可用于刷新 `handler` 函数返回的数据的函数。
- `clear`：可用于将 `data` 设置为 `undefined`（或如果**提供了 `options.default()` 则设置为其值**）、将 `error` 设置为 `undefined`、将 `status` 设置为 `idle` 并将任何当前挂起的请求标记为已取消的函数。
- `error`：数据获取失败时的错误对象。
- `status`：表示数据请求状态的字符串（`"idle"`、`"pending"`、`"success"`、`"error"`）。

`data`、`error` 和 `status` 是Vue的ref，在 `<script setup>` 中可通过 `.value` 访问

默认情况下，**Nuxt 会等待 `refresh` 完成后才允许再次执行。**

如果你**没有在服务器上获取数据**（例如，设置了 `server: false`），则在**激活完成之前不会获取数据**。这意味着即使你在客户端等待 `useFetch`，在 `<script setup>` 中 `data` 仍将保持为 null。

## [实践指南](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#实践指南)

### [通过POST请求消费SSE（服务器发送事件）](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#通过post请求消费sse服务器发送事件)

通过POST请求消费SSE时，你需要手动处理连接。以下是实现方法：

```ts
// 向SSE端点发起POST请求
const response = await $fetch<ReadableStream>('/chats/ask-ai', {
  method: 'POST',
  body: {
    query: "你好AI，你好吗？",
  },
  responseType: 'stream',
})

// 使用 TextDecoderStream 从响应创建新的 ReadableStream，以文本形式获取数据
const reader = response.pipeThrough(new TextDecoderStream()).getReader()

// 在获取数据时读取数据块
while (true) {
  const { value, done } = await reader.read()

  if (done)
    break

  console.log('收到：', value)
}
```

### [并行请求](https://nuxt.com.cn/docs/4.x/getting-started/data-fetching#并行请求)

当请求不相互依赖时，你可以使用 `Promise.all()` 并行发起它们以提高性能。

```ts
const { data } = await useAsyncData(() => {
  return Promise.all([
    $fetch("/api/comments/"), 
    $fetch("/api/author/12")
  ]);
});

const comments = computed(() => data.value?.[0]);
const author = computed(() => data.value?.[1]);
```

# 状态管理

Nuxt 提供了 [`useState`](https://nuxt.com.cn/docs/4.x/api/composables/use-state) 组合式函数，用于在组件之间创建响应式且支持 SSR 的共享状态。

[`useState`](https://nuxt.com.cn/docs/4.x/api/composables/use-state) 是一个**支持 SSR 的 [`ref`](https://vuejs.org/api/reactivity-core.html#ref) 替代方案**。其值**在服务器端渲染后（客户端水合期间）会被保留**，并通过**唯一键在所有组件之间共享**。

## [最佳实践](https://nuxt.com.cn/docs/4.x/getting-started/state-management#最佳实践)

切勿在 `<script setup>` 或 `setup()` 函数外部定义 `const state = ref()`。 例如，执行 `export myState = ref({})` 会导致**状态在服务器上的请求之间共享**，并可能导致内存泄漏。

而是使用 `const useX = () => useState('x')`

## [示例](https://nuxt.com.cn/docs/4.x/getting-started/state-management#示例)

### [基本用法](https://nuxt.com.cn/docs/4.x/getting-started/state-management#基本用法)

在此示例中，我们使用组件本地的计数器状态。任何其他使用 `useState('counter')` 的组件都共享相同的响应式状态。

```vue
<script setup lang="ts">
const counter = useState('counter', () => Math.round(Math.random() * 1000))
</script>

<template>
  <div>
    计数器: {{ counter }}
    <button @click="counter++">
      +
    </button>
    <button @click="counter--">
      -
    </button>
  </div>
</template>
```

### [初始化状态](https://nuxt.com.cn/docs/4.x/getting-started/state-management#初始化状态)

大多数情况下，你可能希望使用异步解析的数据来初始化状态。你可以使用 [`app.vue`](https://nuxt.com.cn/docs/4.x/guide/directory-structure/app) 组件和 [`callOnce`](https://nuxt.com.cn/docs/4.x/api/utils/call-once) 工具函数来实现这一点。

```vue
<script setup lang="ts">
const websiteConfig = useState('config')

await callOnce(async () => {
  websiteConfig.value = await $fetch('https://my-cms.com/api/website-config')
})
</script>
```

## [共享状态](https://nuxt.com.cn/docs/4.x/getting-started/state-management#共享状态)

通过使用 [自动导入的组合式函数](https://nuxt.com.cn/docs/4.x/guide/directory-structure/composables)，我们可以定义**全局类型安全的状态**并在整个应用中导入它们。

```ts
export const useColor = () => useState<string>('color', () => 'pink')
<script setup lang="ts">
// ---cut-start---
const useColor = () => useState<string>('color', () => 'pink')
// ---cut-end---
const color = useColor() // 与 useState('color') 相同
</script>

<template>
  <p>当前颜色: {{ color }}</p>
</template>
```

# 服务器

使用 Nuxt 的服务器框架构建全栈应用。你可以从数据库或其他服务器获取数据、创建 API，甚至生成静态的服务器端内容（如站点地图或 RSS 订阅源）—— 所有这些都可以在单一代码库中完成。

## [由 Nitro 提供支持](https://nuxt.com.cn/docs/4.x/getting-started/server#由-nitro-提供支持)

Nuxt 的服务器基于 [Nitro](https://github.com/nitrojs/nitro)。它最初是为 Nuxt 创建的，但现在是 [UnJS](https://unjs.io/) 的一部分，开放给其他框架使用——甚至可以单独使用。

使用 Nitro 为 Nuxt 带来了强大功能：

- 完全控制应用的服务器端部分
- 在任何提供商上进行通用部署（许多支持零配置）
- 混合渲染

Nitro 内部使用 [h3](https://github.com/h3js/h3)，这是一个为高性能和可移植性而构建的轻量级 H(TTP) 框架。

## [服务器端点和中间件](https://nuxt.com.cn/docs/4.x/getting-started/server#服务器端点和中间件)

你可以轻松管理 Nuxt 应用的服务器端部分，从 API 端点到中间件。

端点和中间件都可以像这样定义：

```vue
export default defineEventHandler(async (event) => {
  // ... 在这里做任何你想做的事情
})
```

你可以直接返回 `text`、`json`、`html` 甚至 `stream`。

与 Nuxt 应用的其他部分一样，它开箱即用地支持**热模块替换**和**自动导入**。

## [通用部署](https://nuxt.com.cn/docs/4.x/getting-started/server#通用部署)

Nitro 提供了将你的 Nuxt 应用部署到任何地方的能力，从裸金属服务器到边缘网络，启动时间仅需几毫秒。这速度非常快！

有超过 15 种预设可用于为不同的云提供商和服务器构建 Nuxt 应用，包括：

- [Cloudflare Workers](https://workers.cloudflare.com/)

- [Netlify Functions](https://www.netlify.com/products/functions)

- [Vercel Edge Network](https://vercel.com/docs/edge-network)

- ## [混合渲染](https://nuxt.com.cn/docs/4.x/getting-started/server#混合渲染)

	Nitro 有一个强大的功能叫做 `routeRules`，它允许你定义一组规则来自定义 Nuxt 应用每个路由的渲染方式（以及更多）。

	nuxt.config.ts

```
export default defineNuxtConfig({
  routeRules: {
    // 为 SEO 目的在构建时生成
    '/': { prerender: true },
    // 缓存 1 小时
    '/api/*': { cache: { maxAge: 60 * 60 } },
    // 重定向以避免 404
    '/old-page': {
      redirect: { to: '/new-page', statusCode: 302 }
    }
    // ...
  }
})

```

此外，还有一些路由规则（例如 `ssr`、`appMiddleware` 和 `noScripts`）是 Nuxt 特有的，用于更改将页面渲染为 HTML 时的行为。

一些路由规则（`appMiddleware`、`redirect` 和 `prerender`）也会影响客户端行为。

Nitro 用于构建服务器端渲染的应用，也用于预渲染。

# 部署

Nuxt 应用程序可以部署在 Node.js 服务器上、预渲染为静态托管，或者部署到无服务器或**边缘（CDN）环境中。**

## [Node.js 服务器](https://nuxt.com.cn/docs/4.x/getting-started/deployment#nodejs-服务器)

探索使用 Nitro 的 Node.js 服务器预设，将其部署到任何 Node 托管环境。

- 如果未指定或自动检测到输出格式，则为**默认输出格式**
- 仅加载渲染请求所需的块，以实现最佳冷启动时间
- 用于将 Nuxt 应用部署到任何 Node.js 托管环境

### [入口点](https://nuxt.com.cn/docs/4.x/getting-started/deployment#入口点)

使用 Node 服务器预设运行 `nuxt build` 时，结果将是一个可直接运行的 Node 服务器入口点。

```vue
node .output/server/index.mjs
```

这将启动您的生产 **Nuxt 服务器**，默认监听 3000 端口。

它支持以下**运行时环境变量**：

- `NITRO_PORT` 或 `PORT`（默认为 `3000`）
- `NITRO_HOST` 或 `HOST`（默认为 `'0.0.0.0'`）
- `NITRO_SSL_CERT` 和 `NITRO_SSL_KEY` - 如果两者都存在，将以 HTTPS 模式启动服务器。在绝大多数情况下，除了测试外不应使用此选项，并且 Nitro 服务器应在终止 SSL 的反向代理（如 nginx 或 Cloudflare）后面运行。

### [PM2](https://nuxt.com.cn/docs/4.x/getting-started/deployment#pm2)

[PM2](https://pm2.keymetrics.io/)（进程管理器 2）是在您的服务器或虚拟机上托管 Nuxt 应用程序的快速简便解决方案。

要使用 `pm2`，请使用 `ecosystem.config.cjs`：

```cjs
module.exports = {
  apps: [
    {
      name: 'NuxtAppName',
      port: '3000',
      exec_mode: 'cluster',
      instances: 'max',
      script: './.output/server/index.mjs'
    }
  ]
}
```

### [集群模式](https://nuxt.com.cn/docs/4.x/getting-started/deployment#集群模式)

您可以使用 `NITRO_PRESET=node_cluster` 来利用 Node.js [cluster](https://nodejs.org/dist/latest/docs/api/cluster.html) 模块实现**多进程性能。**

默认情况下，工作负载会以轮询策略分配给工作进程。

## [静态托管](https://nuxt.com.cn/docs/4.x/getting-started/deployment#静态托管)

将 Nuxt 应用程序部署到任何静态托管服务有两种方式：

- 使用 `ssr: true` 的静态站点生成 (SSG) 在构建时预渲染应用程序的路由。（这是运行 `nuxt generate` 时的默认行为。）它还将生成 `/200.html` 和 `/404.html` 单页应用回退页面，这些页面可以在客户端渲染动态路由或 404 错误（尽管您可能需要在静态主机上配置此功能）。
- 或者，您可以使用 `ssr: false` 预渲染您的站点（静态单页应用）。这将生成包含空 `<div id="__nuxt"></div>` 的 HTML 页面，您的 Vue 应用通常会在此处渲染。这样会失去预渲染站点的许多 SEO 优势，因此建议改用 [``](https://nuxt.com.cn/docs/4.x/api/components/client-only) 来包装站点中无法在服务器端渲染的部分（如果有的话）。

### [仅客户端渲染](https://nuxt.com.cn/docs/4.x/getting-started/deployment#仅客户端渲染)

如果您不想预渲染路由，另一种使用静态托管的方法是在 `nuxt.config` 文件中**将 `ssr` 属性设置为 `false`。**然后，`nuxt generate` 命令将输出一个 `.output/public/index.html` 入口点和 JavaScript 捆绑包，就像经典的客户端 Vue.js 应用程序一样。

## [CDN 代理](https://nuxt.com.cn/docs/4.x/getting-started/deployment#cdn-代理)

在大多数情况下，Nuxt 可以处理不是由 Nuxt 本身生成或创建的第三方内容。但有时此类内容可能会导致问题，尤其是 Cloudflare 的 "Minification and Security Options"。

因此，您应确保在 Cloudflare 中取消选中/禁用以下选项。否则，不必要的重新渲染或 hydration 错误可能会影响您的生产应用程序。

1. Speed > Optimization > Content Optimization > 禁用 "Rocket Loader™"
2. Speed > Optimization > Image Optimization > 禁用 "Mirage"
3. Scrape Shield > 禁用 "Email Address Obfuscation"

通过这些设置，您可以确保 Cloudflare 不会将可能导致意外副作用的脚本注入到您的 Nuxt 应用

# 示例

# Auto Imports

Example of the auto-imports feature in Nuxt with:

- Vue components **in the `components/` directory** are auto-imported and can **be used directly in your templates.**
- Vue composables **in the `composables/` directory** are auto-imported and can be used directly in your templates **and JS/TS files.**
- **JS/TS variables and functions in the `utils/` directory** are auto-imported and can be used directly in your templates and JS/TS files.

# Data Fetching

# State Management

# Meta Tags

## [Middleware · Nuxt Examples v4](https://nuxt.com.cn/docs/4.x/examples/routing/middleware)

## [Pages · Nuxt Examples v4](https://nuxt.com.cn/docs/4.x/examples/routing/pages)

## [Universal Router · Nuxt Examples v4](https://nuxt.com.cn/docs/4.x/examples/routing/universal-router)

# [Layers · Nuxt Examples v4](https://nuxt.com.cn/docs/4.x/examples/advanced/config-extends)

## [Teleport · Nuxt Examples v4](https://nuxt.com/docs/4.x/examples/advanced/teleport)

Vue 3 provides the <Teleport> component which **allows content to be rendered elsewhere in the DOM**, outside of the Vue application.

This example shows how to use the <Teleport> with client-side and server-side rendering.

## [useCookie · Nuxt Examples v4](https://nuxt.com/docs/4.x/examples/advanced/use-cookie)

```vue
<script setup lang="ts">
const user = useCookie<{ name: string } | null>('user')
const logins = useCookie<number>('logins')

const name = ref('')

const login = () => {
  logins.value = (logins.value || 0) + 1
  user.value = { name: name.value }
}

const logout = () => {
  user.value = null
}
</script>

<template>
  <NuxtExample
    class="h-50"
    dir="advanced/use-cookie"
  >
    <template v-if="user">
      <h1 class="text-3xl mb-3">
        Welcome, {{ user.name }}! 👋
      </h1>
      <div>
        <UAlert
          title="Logged-In"
          color="primary"
          icon="i-heroicons-light-bulb"
        >
          <template #description>
            You have logged in <b>{{ logins }} times</b>!
          </template>
        </UAlert>
      </div>
      <div class="mt-3">
        <UButton
          color="warning"
          icon="i-heroicons-arrow-left"
          @click="logout"
        >
          Log out
        </UButton>
      </div>
    </template>
    <template v-else>
      <h1 class="text-3xl mb-3">
        Login
      </h1>
      <UInput
        v-model="name"
        class="w-100 m-auto"
        placeholder="Enter your name..."
        @keypress.enter="login()"
      />
      <div class="mt-3">
        <UButton
          icon="i-heroicons-user"
          :disabled="!name"
          name="Log in"
          @click="login"
        >
          Log in
        </UButton>
      </div>
    </template>
  </NuxtExample>
</template>

```

## [Use Custom Fetch Composable · Nuxt Examples v4](https://nuxt.com/docs/4.x/examples/advanced/use-custom-fetch-composable)

This example shows **a convenient wrapper for the useFetch composable** from nuxt. It allows you to c**ustomize the fetch request with default values and user authentication token.**

# 实战学习记录

## [RXCCCCCC/cippus-web](https://github.com/RXCCCCCC/cippus-web)

## 前置小知识

### 项目结构理解

- app/：**前端页面**、布局、组件（如 pages、layouts、app.vue）
- server/：**服务端 API** 逻辑、**工具函数**（如 utils/prisma.ts）
- prisma/：Prisma schema 文件、**数据库迁移相关**
- **public/：静态资源**（如 logo、robots.txt）
- nuxt.config.ts、tsconfig.json：**项目配置文件（路由、插件、模块、环境变量等）**
- package.json、pnpm-lock.yaml：依赖与**包管理**

### 生命周期与路由机制

- 生命周期钩子（如 onMounted、onBeforeMount、useAsyncData 等）贯穿页面、组件、服务端。
- pages 目录自动生成路由，支持嵌套路由、**动态路由（[id].vue）**、**中间件（middleware）**等。
- layouts 提供页面骨架复用，支持多布局切换

### 渲染模式

- 支持**服务端渲染（SSR）**、**静态站点生成（SSG）**、**客户端渲染（CSR）**三种模式，**灵活切换**。
- useFetch、useAsyncData 等组合式 API，**简化数据获取与状态管理。**

### API 路由与后端集成

- server 目录下可**直接编写 API 路由（如 server/api/user.ts）**，支持 RESTful、GraphQL 等多种风格。
- 与 Prisma 集成：在 [prisma.ts](vscode-file://vscode-app/e:/Microsoft VS Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) **封装数据库操作**，API 路由中调用。
- 支持**中间件、认证、权限控制**等后端逻辑。

### 配置、插件与模块扩展

- [nuxt.config.ts](vscode-file://vscode-app/e:/Microsoft VS Code/resources/app/out/vs/code/electron-browser/workbench/workbench.html) 支持**自定义插件（plugins）、中间件（middleware）、模块（modules）等扩展机制**。

### UI 框架与常见技术集成

- Nuxt UI：**官方 UI 组件库，快速构建现代界面。**
- Tailwind CSS：通过模块集成，支持**原子化样式开发**。
- 状态管理：推荐使用 Pinia，支持**组合式 API**。
- Prisma：类型安全的**数据库 ORM**，适合全栈开发。

前端开发学习

1. Nuxt 4 基础：页面（pages/）、布局（layouts/）、组件（app/）
2. Nuxt UI 用法：查阅官方文档，学习常用 UI 组件集成与自定义
3. Tailwind CSS：掌握原子化样式写法，结合 Nuxt UI 实现响应式设计
4. 实践建议：尝试新增页面、修改样式、复用组件

五、后端与数据库

1. Prisma schema 建模：理解 schema.prisma 语法，尝试新增/修改模型
2. 数据库迁移：使用 pnpm prisma migrate dev 进行结构变更
3. API 开发：在 server/ 目录下编写/扩展 API 路由，调用 Prisma 进行数据操作
4. 实践建议：实现简单的 CRUD 接口，前后端联调

六、常见开发流程

1. 调试：利用 Nuxt 热更新、console.log、Prisma 日志等工具
2. 构建：pnpm build 生成生产环境代码
3. 部署：根据目标环境（如 Vercel、Netlify、自建服务器）配置部署流程
4. 代码管理：使用 Git 进行分支管理、提交、合并等操作
5. 依赖升级与安全：定期更新依赖，关注安全公告

## 知识点

### PostgreSQL pgAdmin 工具

## pgAdmin 的基本使用

### 连接到 PostgreSQL 服务器

1. 打开 pgAdmin
2. 在左侧的"浏览器"面板中，右键点击"Servers"
3. 选择"Create" > "Server..."
4. 在弹出的对话框中填写连接信息：
	- **Name**：为连接起一个名称
	- **Host**：数据库服务器地址（本地使用 localhost）
	- **Port**：PostgreSQL 端口（默认 5432）
	- **Maintenance database**：通常使用 **postgres**
	- **Username** 和 **Password**：数据库凭据

### 浏览数据库对象

成功连接后，你可以展开服务器节点查看：

- 数据库列表
- 每个数据库中的表、视图、函数等对象
- 用户和角色
- 其他服务器对象
