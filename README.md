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

项目的文件结构:
```text
tour-guide-app/                 # 项目根目录 (Monorepo)
├── frontend/                   # Vue前端项目
│   ├── public/                 # 静态资源
│   │   └── index.html
│   ├── src/
│   │   ├── assets/             # 图片、字体等资源
│   │   ├── components/         # 可复用Vue组件
│   │   │   ├── common/         # 通用组件(按钮、卡片等)
│   │   │   ├── navigation/     # 导航相关组件
│   │   │   └── ar/            # AR相关组件
│   │   ├── composables/        # Vue组合式函数(替代Vuex/Pinia)
│   │   ├── stores/             # 状态管理(可后期添加Pinia)
│   │   ├── views/              # 页面级组件
│   │   │   ├── Home.vue
│   │   │   ├── TourView.vue
│   │   │   └── ARView.vue      # AR功能页面
│   │   ├── App.vue
│   │   ├── main.js
│   │   └── router.js           # 路由配置
│   ├── package.json
│   ├── vite.config.js          # Vite配置
│   ├── tailwind.config.js      # TailwindCSS配置
│   └── index.html
├── backend/                    # Python后端项目
│   ├── app/
│   │   ├── __init__.py
│   │   ├── main.py             # FastAPI应用入口
│   │   ├── api/                # API路由
│   │   │   ├── __init__.py
│   │   │   ├── recommendations.py  # 推荐相关端点
│   │   │   └── tours.py            # 导览相关端点
│   │   ├── core/               # 核心配置
│   │   │   ├── __init__.py
│   │   │   ├── config.py       # 应用配置
│   │   │   └── security.py     # 安全相关(可后期添加)
│   │   ├── models/             # 数据模型(SQLAlchemy)
│   │   │   ├── __init__.py
│   │   │   ├── user.py         # 用户模型
│   │   │   └── attraction.py   # 景点模型
│   │   ├── services/           # 业务逻辑层
│   │   │   ├── __init__.py
│   │   │   ├── recommender.py  # 推荐逻辑
│   │   │   └── tour_service.py # 导览逻辑
│   │   ├── agents/             # AI Agent相关
│   │   │   ├── __init__.py
│   │   │   ├── guide_agent.py  # 导览Agent
│   │   │   └── tools/          # Agent可用工具
│   │   │       ├── __init__.py
│   │   │       ├── database_tool.py    # 数据库查询工具
│   │   │       └── navigation_tool.py  # 导航工具
│   │   └── database/           # 数据库相关
│   │       ├── __init__.py
│   │       ├── session.py      # 数据库会话管理
│   │       ├── init_db.py      # 初始化数据库
│   │       └── embeddings.py   # 向量嵌入相关(ChromaDB)
│   ├── data/                   # 数据存储目录
│   │   ├── app.db             # SQLite数据库(不提交到Git)
│   │   └── chroma_db/         # ChromaDB数据目录
│   ├── requirements.txt        # Python依赖
│   ├── Dockerfile             # 容器化配置(可选)
│   └── .env                   # 环境变量(不提交到Git)
├── docs/                      # 项目文档
│   ├── ARCHITECTURE.md        # 架构说明
│   ├── API.md                 # API文档
│   └── SETUP.md               # 设置指南
├── scripts/                   # 实用脚本
│   ├── setup.sh              # 项目初始化脚本
│   ├── run_backend.sh        # 启动后端
│   └── run_frontend.sh       # 启动前端
├── .gitignore
├── docker-compose.yml         # Docker编排配置(可选)
├── package.json              # 根package.json(用于Monorepo脚本)
└── README.md
```