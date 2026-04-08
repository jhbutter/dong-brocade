# Dong Brocade Hero Demo

一个基于 React + Vite 的侗锦纹样交互 demo。

当前项目包含两个独立页面：

- `/`：选卡首页。用户在圆弧卡片布局中浏览、翻面、确认 3 张纹样卡。
- `/result`：结果页。默认显示等待界面；当首页点击“开始生成”后，结果页会实时同步更新。

## Current Status

- 首页支持拖拽旋转卡环、点击卡片放大、翻面确认、移除已选卡片。
- 首页支持中英文切换。
- 结果页和首页通过前端页面通信同步，不依赖后端。
- 结果页默认是等待首屏，不会自动带出上一次结果。
- 结果页的最终展示区域目前统一使用 `result/1.png` 作为生成后的占位图。
- “选中的 3 张卡片 -> 对应 result 文件”的匹配逻辑还没有接入。

## Tech Stack

- React 19
- TypeScript
- Vite
- Tailwind CSS v4
- Motion
- Lucide React

## Project Structure

```text
src/
  App.tsx                         # 路由入口：首页 / 结果页
  components/
    DongBrocadeHero.tsx           # 首页选卡界面
    DongBrocadeResult.tsx         # 结果页界面
  data/
    dongBrocadeCards.ts           # 卡片素材加载与数据整理
  lib/
    resultChannel.ts              # 首页与结果页之间的消息同步
result/
  1.png                           # 当前结果页占位图
```

## Development

### Prerequisites

- Node.js 18+

### Install

```bash
npm install
```

### Start Dev Server

```bash
npm run dev
```

默认开发地址：

- 首页：[http://localhost:3000/](http://localhost:3000/)
- 结果页：[http://localhost:3000/result](http://localhost:3000/result)

## How To Use

1. 启动项目后，同时打开首页 `/` 和结果页 `/result`。
2. 结果页初始会显示等待界面。
3. 在首页选择并确认 3 张纹样卡。
4. 点击“开始生成 / Start Creating”。
5. 结果页会自动同步当前这组选择，并显示对应的结果展示状态。

## Communication Between Pages

首页和结果页是两个独立页面，不通过 URL 参数同步，而是通过浏览器端消息同步：

- 优先使用 `BroadcastChannel`
- 同时写入 `localStorage` 作为状态广播补充

相关实现见：

- [src/lib/resultChannel.ts](/Users/zhanghao/Downloads/侗锦素材/remix_-dong-brocade-hero/src/lib/resultChannel.ts)

## Scripts

```bash
npm run dev      # 启动开发服务器（3000 端口）
npm run build    # 生产构建
npm run preview  # 预览构建结果
npm run lint     # TypeScript 类型检查
```

## Notes

- 当前不需要配置 `.env.local` 才能运行这个 demo。
- 仓库里保留了部分早期模板依赖和配置，但当前交互流程不依赖 Gemini API。
- 后续如果接入真实生成结果，建议新增一层“选卡组合 -> result 文件路径”映射配置，而不是直接把映射逻辑写死在组件里。
