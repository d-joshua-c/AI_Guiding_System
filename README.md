# AI_Guiding_System

## 产品: 以AI agent为核心的, 高度客制化的导览系统
1. 形态: 网页
2. 后端: 
	1. 基于AI的推荐和路径规划算法
	2. 多agent异步架构, 解决延迟问题
	3. 微服务架构, AI和使用者共享所有的工具
3. 前端: 
	1. GUI->LUI, 实现AI驱动的动态UI
	2. 组件化页面, 高度的用户自定义
	3. 高级功能
4. 高级功能
	1. AR
	2. 空间计算

## 技术选型(demo): 
1. 前端
	1. 框架: vue + vite
	2. UI: tailwind CSS
	3. 动画库: **VueUse**
	4. 状态管理: pinia(初期先不使用)
2. 后端
	1. 架构: monolith
	2. AI agent: langchain (逐步替代)
	3. 数据库: **SQLite + ChromaDB (内存/本地模式)**
	4. 部署: **单个云服务器 + Caddy/Nginx** 或 **Vercel/Netlify/Hugging Face Spaces**
3. 文件结构
	1. 前后端分离
	2. Monorepo
