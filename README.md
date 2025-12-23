# vue-ex-editor

专业的 Vue 3 富文本编辑器，基于 ProseMirror，支持 Markdown、协作编辑、插件扩展。

## ✨ 特性

- 🎨 **开箱即用** - 完整的工具栏和格式化功能
- 📝 **Markdown 支持** - 粘贴和输入自动转换
- 🔌 **插件系统** - 灵活的插件扩展机制
- 👥 **协作编辑** - 基于 Yjs 的实时协作（可选）
- 📱 **响应式** - 适配桌面和移动端
- 🎯 **TypeScript** - 完整的类型支持
- 🌙 **主题切换** - 支持亮色/暗色主题
- 🖥️ **全屏模式** - 浏览器原生全屏 API
- 🔒 **样式隔离** - 自动添加命名空间前缀，避免样式冲突

## 📦 安装

```bash
npm install vue-ex-editor
# 或
yarn add vue-ex-editor
# 或
pnpm add vue-ex-editor
```

## 🚀 快速开始

```vue
<script setup lang="ts">
import { ref } from 'vue'
import { ExEditor } from 'vue-ex-editor'
import 'vue-ex-editor/style.css'

const content = ref(null)

const handleReady = (editor) => {
  console.log('编辑器已就绪', editor)
}
</script>

<template>
  <ExEditor
    v-model:content="content"
    placeholder="开始编写内容..."
    @ready="handleReady"
  />
</template>
```

## ⚙️ Props

| 属性 | 类型 | 默认值 | 说明 |
|------|------|--------|------|
| `content` | `Object` | `undefined` | 编辑器初始内容（ProseMirror JSON） |
| `readonly` | `boolean` | `false` | 是否只读模式 |
| `showToolbar` | `boolean` | `true` | 是否显示工具栏 |
| `showStatusbar` | `boolean` | `true` | 是否显示状态栏 |
| `placeholder` | `string` | `'开始编写内容...'` | 占位符文本 |
| `theme` | `'light' \| 'dark'` | `'light'` | 主题模式 |
| `apiConfig` | `EditorAPIConfig` | `undefined` | API 配置（上传、协作等） |

## 📤 Events

| 事件 | 参数 | 说明 |
|------|------|------|
| `update:content` | `content: ProseMirrorNode` | 内容更新时触发 |
| `ready` | `editor: Editor` | 编辑器初始化完成时触发 |
| `fullscreen-change` | `isFullscreen: boolean` | 全屏状态变化时触发 |

## � 主题切换

```vue
<script setup>
import { ref } from 'vue'
import { ExEditor } from 'vue-ex-editor'

const theme = ref<'light' | 'dark'>('light')

const toggleTheme = () => {
  theme.value = theme.value === 'light' ? 'dark' : 'light'
}
</script>

<template>
  <button @click="toggleTheme">切换主题</button>
  <ExEditor :theme="theme" />
</template>
```

## 🖥️ 全屏模式

编辑器内置全屏按钮，也可以通过方法控制：

```ts
const editorRef = ref()

// 进入全屏
editorRef.value.enterFullscreen()

// 退出全屏（按 Escape 键也可退出）
editorRef.value.exitFullscreen()

// 切换全屏
editorRef.value.toggleFullscreen()
```

监听全屏变化事件：

```vue
<ExEditor @fullscreen-change="onFullscreenChange" />
```

## �🎨 导出格式

```ts
// 获取编辑器实例
const editorRef = ref()

// 导出为不同格式
const json = editorRef.value.exportContent('json')
const html = editorRef.value.exportContent('html')
const markdown = editorRef.value.exportContent('markdown')
const text = editorRef.value.exportContent('text')
```

## 🔧 高级用法

### 使用核心 Editor 类

```ts
import { Editor } from 'vue-ex-editor'

const editor = new Editor({
  container: document.getElementById('editor'),
  content: '<p>Hello World</p>',
  placeholder: '输入内容...',
})

// 执行命令
editor.executeCommand('bold')
editor.executeCommand('heading', { level: 1 })

// 获取内容
const content = editor.exportContent('markdown')
```

### API 配置

```ts
const apiConfig = {
  upload: {
    enabled: true,
    endpoint: '/api/upload',
    maxFileSize: 10 * 1024 * 1024,
    allowedTypes: ['image', 'video', 'attachment'],
  },
  collaboration: {
    enabled: true,
    websocketUrl: 'wss://collab.example.com',
    reconnectAttempts: 5,
    reconnectDelay: 3000,
  },
}
```

## 🔒 样式隔离

编辑器使用 `.vex-editor` 命名空间和 CSS 变量前缀 `--editor-`，避免与宿主项目样式冲突。

如需自定义样式，可覆盖 CSS 变量：

```css
.vex-editor {
  --editor-primary: #your-color;
  --editor-bg: #your-bg;
}
```

## 📝 License

MIT
