# TailwindCSS Usage reports

`Tailwind CSS` 是流行的原子化 `css` 框架。

原子化 css 是这样的写法：

```html
<div class="flex justify-center p-1 border"></div>

<!-- Equivalent to this -->
<style>
  .flex {
    display: flex
  }
  .justify-center {
    justify-content: center;
  }
  .p-1 {
    padding: 4px;
  }
  .border {
    border-width: 1px;
  }
</style>
```

## CSS VS Tailwind CSS

```html
// CSS
<div class="container">
  <div class="video-list-wrapper">
    <div class="first-video-wrapper">main</div>
    {#each counts as i}
      <div class="other-video-wrapper">{i}</div>
    {/each}
  </div>
</div>

<style>
  * {
    box-sizing: border-box;
  }
  div.container {
    width: 1440rem;
    padding: 0 64rem;
    font-size: 12rem;
  }
  div.video-list-wrapper {
    display: grid;
    grid-template-columns: repeat(5, 1fr);
    grid-template-rows: repeat(auto-fill, 1fr);
    grid-auto-rows: 228rem;
    grid-gap: 16rem;
  }
  div.first-video-wrapper {
    background-color: antiquewhite;
    grid-row: 1 / 3;
    grid-column: 1 / 3;
    border-radius: 16rem;
    display: flex;
    align-items: center;
    justify-content: center;
    width: 100%;
  }
  div.other-video-wrapper {
    background-color: brown;
    border-radius: 16rem;
    display: flex;
    align-items: center;
    justify-content: center;
  }
  @media screen and (max-width: 720px) {
    div.container {
      width: 100%;
      padding: 0 16rem;
      font-size: 12rem;
    }
    div.video-list-wrapper {
      grid-template-columns: repeat(2, 1fr);
      grid-gap: 14rem;
      grid-auto-rows: 200rem;
    }
  }
</style>
```

```html
// Tailwind CSS
<div
  class="w-[1440rem] px-[64rem] text-[12rem] max-[720px]:w-full max-[720px]:px-[16rem] max-[720px]:text-[12rem]"
>
  <div
    class="grid grid-cols-5 grid-rows-[repeat(auto-fill, 1fr)] auto-rows-[228rem] gap-[16rem] video-list-wrapper"
  >
    <div
      class="bg-[antiquewhite] row-[1/3] col-[1/3] rounded-[16rem] flex justify-center items-center w-full"
    >
      main
    </div>
    {#each counts as i}
      <div class="bg-[brown] rounded-[16rem] flex justify-center items-center">{i}</div>
    {/each}
  </div>
</div>

<style lang="postcss">
  * {
    box-sizing: border-box;
  }
  @media screen and (max-width: 720px) {
    div.video-list-wrapper {
      @apply grid-cols-[repeat(2,1fr)] gap-56 auto-rows-[200rem];
    }
  }
</style>
```

## 优缺点

### 优点

1. 减少编写样式的代码行数。
2. 快速开发，能够借助其丰富的类库，快速制作原型并迭代设计，非常适合初创的小项目。
3. 使用原子类可以**大大**减少需要起名的场景。
4. 轻松实现响应式设计。

### 缺点

1. 若组件样式过多，则 `class` 代码会非常冗长 `<div className="text-base mg-1 pt-1 pr-1 md:pt-2 md:pr-2 rounded border-solid border-2 border-light-blue-500 bg-blue-500 text-white-500"></div>`。
2. 文件大小增加，由于 `Tailwind CSS` 提供了大量的实用类库，与使用预构建组件的框架相比，虽然采用的是 `JIT` 的编译策略，还是可能导致较大的 `CSS` 文件大小。
3. 初学者可能需要花更多的时间熟悉文档中的类命名。
4. 维护和管理成本较高，随着项目的迭代，管理和维护自定义配置可能会变得复杂，尤其是对于大型且自定义配置较多的项目。

## 代码检查和约束

1. `eslint` 插件使用 [`eslint-plugin-tailwindcss`](https://github.com/francoismassart/eslint-plugin-tailwindcss)，根据文档安装即可。
2. `Prettier` 插件使用官方有提供的 [`prettier-plugin-tailwindcss`](https://github.com/tailwindlabs/prettier-plugin-tailwindcss)，根据文档安装即可。需要注意的是从 v0.5.x 开始，此插件现在需要 Prettier v3，文档有详细说明。

## 总结

`Tailwind CSS` 本质上就是个 `postcss` 插件。`postcss` 通过 `AST` 拿到 `@tailwind`、`@layer`、`@apply` 这些它扩展的指令，分别作相应的处理。
所以说，`Tailwind CSS` 就是基于 `postcss` 的 `AST` 实现的 `css` 代码生成工具。
