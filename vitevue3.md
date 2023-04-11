### 1 初始化醒目

```
vue create @vitejs/app vitedemo3
```
发现报错如下
```
vue-cli · Failed to download repo @vitejs/app: Response code 404 (Not Found)

```
更新后仍报错
```
$ vue init vite@latest vitedemo3
vue-cli · Failed to download repo vuejs-templates/vite@latest: Response code 404 (Not Found)
```
发现上面的都过期了 使用
```
vue create vite vitedemo3
```

### 2 @ 需要进行别名的配置

```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
const path = require('path')

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': path.resolve(__dirname, './src')
    }
  }
})
```
### 3 配置@时引入path报错
Error: Dynamic require of "path" is not supported
将require 改成 import的形势
```
import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'
import { resolve } from 'path'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    alias: {
      '@': resolve(__dirname, './src')
    }
  }
})

```


