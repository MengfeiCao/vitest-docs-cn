# Configuring Vitest

## Configuration

`vitest` will read your root `vite.config.ts` when it present to match with the plugins and setup as your Vite app. If you want to it to have a different configuration for testing, you could either:

- Create `vitest.config.ts`, which will have the higher priority
- Pass `--config` option to CLI, e.g. `vitest --config ./path/to/vitest.config.ts`
- Use `process.env.VITEST` to conditionally apply different configuration in `vite.config.ts`

To configure `vitest` itself, add `test` property in your Vite config

```ts
// vite.config.ts
import { defineConfig } from "vite";

export default defineConfig({
  test: {
    // ...
  },
});
```

TODO: Mention [Config File Resolving](), [Config Intellisense]()

## Options

### includes

- **Type:** `string[]n`
- **Default:** `['**\/*.test.ts']`

Include globs for test files

### excludes

- **Type:** `string[]`
- **Default:** `['**\/node_modules\/**']`

Exclude globs for test files

### deps

- **Type:** `{ external?, inline? }`

Handling for dependencies inlining or externalizing
         
#### deps.external

- **Type:** `(string | RegExp)[]`
- **Default:** `['**\/node_modules\/**']`

Externalize means that Vite will bypass the package to native Node. Externalized dependencies will not be applied Vite's transformers and resolvers, so they do not support HMR on reload. Typically, packages under `node_modules` are externalized.

#### deps.inline 

- **Type:** `(string | RegExp)[]`
- **Default:** `['**\/node_modules\/**']`

Vite will process inlined modules. This could be helpful to handle packages that ship `.js` in ESM format (that Node can't handle).

### global

- **Type:** `boolean`
- **Default:** `false`

By default, `vitest` does not provide global APIs for explicitness. If you prefer to use the APIs globally like Jest, you can pass the `--global` option to CLI or add `global: true` in the config.

```ts
// vite.config.ts
import { defineConfig } from "vite";

export default defineConfig({
  test: {
    global: true,
  },
});
```

To get TypeScript working with the global APIs, add `vitest/global` to the `types` filed in your `tsconfig.json`

```jsonc
// tsconfig.json
{
  "compilerOptions": {
    "types": ["vitest/global"]
  }
}
```

If you are already using [`unplugin-auto-import`](https://github.com/antfu/unplugin-vue-components) in your project, you can also use it directly for auto importing those APIs.

```ts
// vite.config.ts
import { defineConfig } from "vite";
import AutoImport from "unplugin-auto-import/vite";

export default defineConfig({
  plugins: [
    AutoImport({
      imports: ["vitest"],
      dts: true, // generate TypeScript declaration
    }),
  ],
});
```
   
### environment

- **Type:** `'node' | 'jsdom' | 'happy-dom'`
- **Default:** `'node'`

Running environment

### update

- **Type:** `boolean`
- **Default:** `false`

Update snapshot files

### watch

- **Type:** `boolean`
- **Default:** `false`

Enable watch mode
    
### root

- **Type:** `string`

Project root

### reporters

- **Type:** `Reporter | Reporter[]`

Custom reporter for output

### threads

- **Type:** `boolean`
- **Default:** `true`

Enable multi-threading using Piscina.js

### maxThreads

- **Type:** `number`
- **Default:** _available CPUs_

Maximum number of threads

### minThreads

- **Type:** `number`
- **Default:** _available CPUs_

Minimum number of threads

### interpretDefault

- **Type:** `boolean`

### testTimeout

- **Type:** `number`
- **Default:** `5000`

Default timeout of a test in milliseconds
     
### hookTimeout

- **Type:** `number`
- **Default:** `5000`
     
Default timeout of a hook in milliseconds
     
### silent

- **Type:** `boolean`
- **Default:** `false`

Silent mode

### open

- **Type:** `boolean`
- **Default:** `false`

Open Vitest UI (WIP)

### setupFiles

- **Type:** `string | string[]`

Path to setup files

### api

- **Type:** `boolean | number`
- **Default:** `false`

Listen to port and serve API. When set to true, the default port is 55555