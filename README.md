# DTPerfWatch - 免费开源性能监控SDK

[![Version](https://img.shields.io/badge/version-1.0.0-blue.svg)](https://github.com/your-repo/yideng)
[![License](https://img.shields.io/badge/license-ISC-green.svg)](LICENSE)
[![TypeScript](https://img.shields.io/badge/TypeScript-4.x-blue.svg)](https://www.typescriptlang.org/)

一款轻量级、功能完整的Web性能监控SDK，专注于收集核心性能指标和错误监控，为Web应用提供全面的性能分析数据。

## ✨ 特性

- 🚀 **轻量级** - 体积小巧，性能开销低
- 📊 **全面监控** - 支持Web Vitals核心指标
- 🐛 **错误追踪** - 完整的JavaScript错误监控
- 🌐 **网络监控** - 网络连接信息和资源加载性能
- 🔧 **高度可配置** - 丰富的配置选项
- 📱 **兼容性好** - 支持多种浏览器和上报方式
- 🆓 **完全免费** - 开源免费使用

## 📦 安装

```bash
npm install yideng
```

或使用CDN：

```html
<script src="https://unpkg.com/yideng@1.0.0/dist/yideng.umd.js"></script>
```

## 🚀 快速开始

### 基础用法

```javascript
import Yideng from 'yideng';

// 初始化监控
const monitor = new Yideng({
  logUrl: 'https://your-api.com/performance', // 必填：数据上报地址
});

// 开启错误监控
const monitorWithError = new Yideng({
  logUrl: 'https://your-api.com/performance',
  captureError: true, // 开启错误监控
});
```

### 完整配置

```javascript
const monitor = new Yideng({
  // 必填配置
  logUrl: 'https://your-api.com/performance',
  
  // 可选配置
  captureError: true,           // 开启错误监控
  resourceTiming: true,         // 开启资源性能监控
  elementTiming: true,          // 开启元素性能监控
  maxMeasureTime: 15000,        // 最大测量时间(ms)
  
  // 自定义数据处理
  analyticsTracker: (data) => {
    console.log('性能数据:', data);
    // 自定义处理逻辑
  }
});
```

## 📊 监控指标

### 核心性能指标 (Web Vitals)

| 指标 | 描述 | 阈值 |
|------|------|------|
| **FCP** | 首次内容绘制时间 | < 1.8s |
| **LCP** | 最大内容绘制时间 | < 2.5s |
| **FID** | 首次输入延迟 | < 100ms |
| **CLS** | 累积布局偏移 | < 0.1 |
| **TBT** | 总阻塞时间 | < 200ms |

### 导航时间指标

- **DNS查询时间** - 域名解析耗时
- **TCP连接时间** - 建立连接耗时
- **白屏时间** - 页面开始渲染时间
- **DOM解析时间** - DOM构建耗时
- **资源加载时间** - 资源下载耗时
- **首字节时间(TTFB)** - 服务器响应时间

### 网络信息

- **连接类型** - 2G/3G/4G/5G
- **下载速度** - 网络带宽
- **RTT延迟** - 往返时间
- **数据节省模式** - 是否开启省流量

### 错误监控

- **JavaScript错误** - 运行时错误
- **Promise异常** - 未处理的Promise错误
- **资源加载失败** - 404、网络错误等
- **iframe错误** - 跨域iframe错误

## 🔧 API 参考

### 构造函数选项

```typescript
interface IYidengOptions {
  // 必填
  logUrl?: string;                    // 数据上报地址
  
  // 可选
  captureError?: boolean;             // 是否开启错误监控
  resourceTiming?: boolean;           // 是否开启资源性能监控
  elementTiming?: boolean;            // 是否开启元素性能监控
  maxMeasureTime?: number;            // 最大测量时间(ms)
  analyticsTracker?: (options: IAnalyticsTrackerOptions) => void; // 自定义数据处理
}
```

### 数据上报优先级

```typescript
enum AskPriority {
  URGENT = 1,  // 紧急：立即上报
  IDLE = 2,    // 空闲：延迟上报
}
```

## 📁 项目结构

```
src/
├── config/              # 配置管理
│   └── index.ts
├── data/               # 数据收集和上报
│   ├── analyticsTracker.ts
│   ├── constants.ts
│   ├── log.ts
│   ├── metrics.ts
│   ├── ReportData.ts
│   ├── reportPerf.ts
│   └── storageEstimate.ts
├── error/              # 错误监控
│   └── Index.ts
├── helpers/            # 工具函数
│   ├── getNavigatorInfo.ts
│   ├── getNetworkInformation.ts
│   ├── isLowEnd.ts
│   ├── isSupported.ts
│   ├── onVisibilityChange.ts
│   ├── utils.ts
│   └── vitalsScore.ts
├── performance/        # 性能指标收集
│   ├── cumulativeLayoutShift.ts
│   ├── firstInput.ts
│   ├── getNavigationTiming.ts
│   ├── observe.ts
│   ├── observeInstances.ts
│   ├── paint.ts
│   ├── performanceObserver.ts
│   ├── resourceTiming.ts
│   └── totalBlockingTime.ts
├── tools/              # 支持性检测
│   └── isSupported.ts
├── typings/            # 类型定义
│   └── types.ts
└── yideng.ts          # 主入口文件
```

## 🛠️ 开发

### 环境要求

- Node.js >= 12
- TypeScript >= 4.0

### 安装依赖

```bash
npm install
```

### 开发模式

```bash
npm run dev
```

### 构建

```bash
npm run build
```

### 运行示例

```bash
npm run example:run
```

### 生成文档

```bash
npm run api:doc
```

## 📝 示例

### 基础监控示例

```html
<!DOCTYPE html>
<html>
<head>
    <title>性能监控示例</title>
</head>
<body>
    <div id="app">Hello World</div>
    
    <script src="https://unpkg.com/yideng@1.0.0/dist/yideng.umd.js"></script>
    <script>
        // 初始化监控
        const monitor = new Yideng({
            logUrl: '/api/performance',
            captureError: true,
            resourceTiming: true
        });
    </script>
</body>
</html>
```

### 错误监控示例

```javascript
// 开启错误监控后，会自动捕获以下错误：

// 1. JavaScript运行时错误
throw new Error('测试错误');

// 2. Promise未处理异常
Promise.reject('Promise错误');

// 3. 资源加载失败
const img = new Image();
img.src = 'non-existent-image.png';
```

## 🔍 浏览器支持

- Chrome >= 60
- Firefox >= 55
- Safari >= 12
- Edge >= 79

## 📄 许可证

ISC License

## 🤝 贡献

欢迎提交 Issue 和 Pull Request！

## 📞 联系方式

如有问题，请通过以下方式联系：

- 提交 [Issue](https://github.com/your-repo/yideng/issues)
- 发送邮件至：wangyaotang0228@gmail.com


