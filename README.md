# MCP Server Chart

> [!NOTE]
> 本项目是 [mcp-gateway](https://github.com/ahaufox/mcp-gateway) (MCP 聚合网关) 的子项目之一，旨在提供基于 AntV 的图表生成与数据分析 MCP 服务器。

基于 [AntV](https://github.com/antvis) 的 MCP (Model Context Protocol) 图表生成服务器，支持 27+ 种图表类型，可通过 MCP 协议进行**图表生成**和**数据分析**。

## ✨ 特性

- **27+ 图表类型**: 覆盖统计图表、关系图、地图、思维导图等
- **三种传输模式**: 支持 stdio / SSE / Streamable HTTP 三种 MCP 传输协议
- **Zod 输入校验**: 所有图表工具均使用 Zod Schema 进行参数校验，提供清晰的错误提示
- **环境变量配置**: 支持自定义图表渲染服务地址、工具开关等
- **Docker 部署**: 提供多阶段构建的 Docker 镜像，支持非 root 用户运行
- **地图图表扩展**: 集成高德地图服务，支持行政区划、路径、POI 分布图

## 📋 图表列表

| 工具名 | 说明 |
|--------|------|
| `generate_area_chart` | 面积图，展示连续变量下的数据趋势 |
| `generate_bar_chart` | 条形图，水平方向比较不同类别的值 |
| `generate_boxplot_chart` | 箱线图，展示数据分布（中位数、四分位数、异常值） |
| `generate_column_chart` | 柱状图，垂直方向比较不同类别的值 |
| `generate_district_map` | 行政区划图，展示行政区域划分和数据分布 |
| `generate_dual_axes_chart` | 双轴图，展示两个不同量纲变量的关系 |
| `generate_fishbone_diagram` | 鱼骨图（因果图），识别和展示问题根因 |
| `generate_flow_diagram` | 流程图，展示流程的步骤和顺序 |
| `generate_funnel_chart` | 漏斗图，展示不同阶段的数据流失 |
| `generate_histogram_chart` | 直方图，展示数据分布（按区间统计） |
| `generate_line_chart` | 折线图，展示数据随时间或连续变量的变化趋势 |
| `generate_liquid_chart` | 水波图，以水球形式展示数据占比 |
| `generate_mind_map` | 思维导图，展示思维过程和层级信息 |
| `generate_network_graph` | 网络图，展示节点间的关系和连接 |
| `generate_organization_chart` | 组织架构图，展示组织结构和人员关系 |
| `generate_path_map` | 路径图，展示 POI 的路线规划结果 |
| `generate_pie_chart` | 饼图，展示各部分数据占比 |
| `generate_pin_map` | 标注图，展示 POI 的分布 |
| `generate_radar_chart` | 雷达图，多维度综合展示数据 |
| `generate_sankey_chart` | 桑基图，展示数据流动和流量 |
| `generate_scatter_chart` | 散点图，展示两个变量间的关系 |
| `generate_treemap_chart` | 矩形树图，展示层级数据（矩形面积代表数值） |
| `generate_venn_chart` | 韦恩图，展示集合间的关系（交集、并集、差集） |
| `generate_violin_chart` | 小提琴图，展示数据分布（箱线图 + 密度图） |
| `generate_waterfall_chart` | 瀑布图，展示顺序引入的正/负值的累积效果 |
| `generate_word_cloud_chart` | 词云图，展示文本数据中的词频 |
| `generate_spreadsheet` | 电子表格/透视表，展示表格数据或交叉汇总分析 |

> [!NOTE]
> 以上地图类图表（`district-map`, `path-map`, `pin-map`）使用[高德地图服务](https://lbs.amap.com/)，目前仅支持中国地区的地图生成。

## 🚀 快速开始

### npx 运行

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "command": "npx",
      "args": ["-y", "@antv/mcp-server-chart"]
    }
  }
}
```

Windows 系统：

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "command": "cmd",
      "args": ["/c", "npx", "-y", "@antv/mcp-server-chart"]
    }
  }
}
```

### Docker 运行

```bash
docker compose up -d
```

默认以 SSE 模式运行在 `http://localhost:1122/sse`。

## 🚰 传输模式

### SSE 模式

```bash
node build/index.js --transport sse
# 访问 http://localhost:1122/sse
```

### Streamable HTTP 模式

```bash
node build/index.js --transport streamable
# 访问 http://localhost:1122/mcp
```

### Stdio 模式（默认）

```bash
node build/index.js
```

## 🎮 CLI 参数

```
--transport, -t  指定传输协议: "stdio", "sse" 或 "streamable" (默认: "stdio")
--host, -h       指定主机地址 (默认: localhost)
--port, -p       指定端口号 (默认: 1122)
--endpoint, -e   指定端点路径:
                 - SSE 模式默认: "/sse"
                 - Streamable 模式默认: "/mcp"
--help, -H       显示帮助信息
```

## ⚙️ 环境变量

| 变量 | 说明 | 默认值 |
|------|------|--------|
| `VIS_REQUEST_SERVER` | 自定义图表生成服务地址 | `https://antv-studio.alipay.com/api/gpt-vis` |
| `SERVICE_ID` | 图表生成记录的服务标识 | - |
| `DISABLED_TOOLS` | 禁用的工具列表（逗号分隔） | - |

### 自定义图表渲染服务

默认使用 AntV 提供的免费图表生成服务。如需私有部署，可参考 [GPT-Vis-SSR](https://github.com/antvis/GPT-Vis/tree/main/bindings/gpt-vis-ssr) 部署 HTTP 服务，并通过 `VIS_REQUEST_SERVER` 指定地址：

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "command": "npx",
      "args": ["-y", "@antv/mcp-server-chart"],
      "env": {
        "VIS_REQUEST_SERVER": "<YOUR_SERVER_URL>"
      }
    }
  }
}
```

> [!NOTE]
> 私有部署目前不支持地图类图表（`generate_district_map`, `generate_path_map`, `generate_pin_map`）。

### 工具过滤

通过 `DISABLED_TOOLS` 禁用指定图表工具：

```json
{
  "mcpServers": {
    "mcp-server-chart": {
      "command": "npx",
      "args": ["-y", "@antv/mcp-server-chart"],
      "env": {
        "DISABLED_TOOLS": "generate_fishbone_diagram,generate_mind_map"
      }
    }
  }
}
```

## 🛠️ 本地开发

### 环境要求

- Node.js 16+
- npm

### 构建与运行

```bash
# 安装依赖
npm install

# 构建
npm run build

# Stdio 模式运行
npm run start

# SSE 模式运行
node build/index.js -t sse

# Streamable 模式运行
node build/index.js -t streamable
```

### 测试

```bash
npm test
```

### 代码检查

```bash
# Biome 格式化与检查
npx biome check --write src/
npx biome format --write src/
```

### Docker 构建

```bash
# 构建镜像
docker build -t mcp-server-chart .

# 运行（Streamable 模式）
docker run -p 1122:1122 mcp-server-chart

# 运行（SSE 模式）
docker run -p 1122:1122 mcp-server-chart node build/index.js -t sse -p 1122 -h 0.0.0.0
```

## 📁 项目结构

```
src/
├── charts/            # 各图表类型的工具定义（Zod Schema + Tool 描述）
│   ├── base.ts        # 基础 Schema（主题、颜色、尺寸等）
│   ├── index.ts       # 统一导出
│   ├── area.ts        # 面积图
│   ├── pie.ts         # 饼图
│   └── ...            # 其他图表
├── services/          # 传输协议服务
│   ├── sse.ts         # SSE 传输（Express 实现）
│   ├── stdio.ts       # Stdio 传输
│   └── streamable.ts  # Streamable HTTP 传输（Express + CORS）
├── utils/             # 工具函数
│   ├── callTool.ts    # 工具调用路由（图表类型映射 + 校验 + API 调用）
│   ├── env.ts         # 环境变量管理
│   ├── generate.ts    # 图表生成 API 调用
│   ├── logger.ts      # 统一日志（支持 stdio/非 stdio 模式切换）
│   ├── schema.ts      # Zod to JSON Schema 转换
│   └── validator.ts   # 数据校验（节点边唯一性校验、树结构校验）
├── index.ts           # CLI 入口
├── sdk.ts             # SDK 导出
└── server.ts          # 服务器创建（工具注册 + 多模式入口）
```

### 关键设计说明

- **输入校验**: 使用 `Zod Schema` 定义每个图表的参数结构，通过 `zod-to-json-schema` 转换为 MCP Tool 的 `inputSchema`，确保类型安全和清晰的错误提示
- **日志系统**: 自定义 `logger` 支持 stdio/非 stdio 模式自动切换（stdio 模式下日志输出到 stderr，避免干扰 MCP 协议通信）
- **数据校验**: `validator.ts` 提供节点边数据的唯一性校验（节点名唯一、边引用有效、边对唯一）和树结构数据的名称唯一性校验
- **传输协议**: SSE 使用 Express 实现，支持多 Session 管理；Streamable HTTP 使用无状态模式，每次请求创建独立的 Transport 避免请求 ID 冲突

## 📄 协议

MIT @ [AntV](https://github.com/antvis)