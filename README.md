# unnamed

## Run Setup

```bash
yarn && yarn run dev

# 1. Set your monerod file directory
# 2. Run 'Start Node' from System Tray
```
## Directory

```tree
├
├── configs
├   ├── vite-main.config.ts          Main-process config file, for -> src/main
├   ├── vite-preload.config.ts       Preload-script config file, for -> src/preload
├   ├── vite-renderer.config.ts      Renderer-script config file, for -> src/renderer
├
├── scripts
├   ├── build.mjs                    Build script, for -> npm run build
├   ├── electron-builder.config.mjs
├   ├── watch.mjs                    Develop script, for -> npm run dev
├
├── src
├   ├── main                         Main-process source code
├   ├── preload                      Preload-script source code
├   ├── renderer                     Renderer-process source code
├
```

#### `dist` and `src`

- Once `npm run dev` or `npm run build` is executed. Will be generated `dist`, it is the same as the `src` structure.

- This ensures the accuracy of path calculation.

```tree
├── dist
├   ├── main
├   ├── preload
├   ├── renderer
├── src
├   ├── main
├   ├── preload
├   ├── renderer
├
```

## Communication

**All NodeJs、Electron API invoke passed `Preload-script`**

* **src/preload/index.ts**

  ```typescript
  // --------- Expose some API to Renderer process. ---------
  contextBridge.exposeInMainWorld('fs', fs)
  contextBridge.exposeInMainWorld('ipcRenderer', ipcRenderer)
  ```

* **src/renderer/src/main.ts**

  ```typescript
  console.log('fs', window.fs)
  console.log('ipcRenderer', window.ipcRenderer)
  ```