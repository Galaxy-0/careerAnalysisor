# careerAnalysisor 开发任务清单

以下文档基于 README 中定义的 「careerAnalysisor 智能职业发展分析工具」 需求，按照 DevOps 流程，将整个项目拆解为 7 个阶段的节点性方案，每个阶段包含目标、产物、任务和验收标准。

---

## 阶段 0：项目初始化与基础设施搭建（预估 3 天）

**目标**
- 搭建本地开发环境
- 建立 GitHub 仓库与分支规范
- 配置依赖管理、代码格式化、Lint 与初步测试框架

**Deliverables**
- GitHub 仓库与分支规范文档
- `requirements.txt`、`.gitignore`、`CHANGELOG.md`
- pre-commit 钩子配置
- 示例单元测试通过

**任务**
1. 新建 GitHub 项目，设置分支：`main`, `develop`, `feature/*`
2. 本地 Python 虚拟环境（`venv`）并配置清华源
3. 初始化项目文件：`requirements.txt`, `.gitignore`, `CHANGELOG.md`
4. 安装并配置 Black、isort、flake8、pre-commit
5. 编写示例测试：`tests/test_dummy.py`，并运行 `pytest` 验证

**验收标准**
- `pip install -r requirements.txt` 无错误
- pre-commit 钩子生效
- `pytest` 绿灯
- 初始版本推送至 `develop`，并更新 `CHANGELOG.md`

---

## 阶段 1：需求澄清与 PRD 编写（预估 2 天）

**目标**
- 梳理产品功能边界
- 输出正式 PRD 文档

**Deliverables**
- `docs/PRD.md`
- `docs/api_spec.yaml`（OpenAPI 3.0）

**任务**
1. 在 `docs/PRD.md` 列出各模块功能：简历解析、行业趋势预测、个性化建议、智能匹配、报告导出
2. 定义输入/输出/性能指标
3. 商定 API 接口规范草案：`docs/api_spec.yaml`
4. 更新 `CHANGELOG.md`

**验收标准**
- PRD 文档评审通过
- API 规范初稿确认

---

## 阶段 2：技术选型与项目骨架（预估 4 天）

**目标**
- 确定技术栈
- 搭建 FastAPI 服务骨架

**Deliverables**
- 项目目录结构
- `app/main.py`、路由模板
- Demo API 示例返回

**任务**
1. 技术选型：spaCy、Scikit-learn/TensorFlow、Plotly/WeasyPrint、PostgreSQL+SQLAlchemy
2. 在 `app/` 下创建目录：`api/`, `core/`, `models/`, `services/`, `utils/`
3. 编写 `app/main.py`，配置 CORS、日志和版本路由
4. 在 `routers/` 中新增五个示例接口
5. 更新 `requirements.txt`、添加 `Dockerfile`、`.dockerignore`
6. 更新 `CHANGELOG.md`

**验收标准**
- `uvicorn app.main:app --reload` 可访问 `/docs`
- 路由返回 200 示例数据
- 自动生成 API 文档

---

## 阶段 3：核心模块开发与测试（预估 6 周）

**整体原则**：每个模块遵循 「设计 → 实现 → 单元测试 → 文档」 流程。

### 3.1 简历与经验解析（2 周）
- **实现**：`services/resume_parser.py`
- **测试**：`tests/test_resume_parser.py`（覆盖率 ≥ 90%）
- **文档**：`docs/resume_parser.md`

### 3.2 行业趋势预测（1.5 周）
- **实现**：`services/trend_predictor.py` + 模型文件
- **测试**：`tests/test_trend_predictor.py`
- **文档**：`docs/trend_predictor.md`

### 3.3 个性化建议与职位匹配（1.5 周）
- **实现**：`services/recommender.py`, `services/job_matcher.py`
- **测试**：`tests/test_recommender.py`, `tests/test_job_matcher.py`
- **文档**：`docs/recommender.md`, `docs/job_matcher.md`

### 3.4 可视化报告（1 周）
- **实现**：`services/report_generator.py`
- **测试**：`tests/test_report_generator.py`
- **文档**：`docs/report_generator.md`

**阶段验收**
- 所有模块单元测试通过
- API Mock 调用输出符合预期
- 文档齐全

---

## 阶段 4：CI/CD + 容器化 + 编排（预估 1 周）

**目标**
- 配置 GitHub Actions 实现 Lint → Test → Build & Push
- Docker 及 Kubernetes 基本配置

**Tasks**
1. 创建 `.github/workflows/ci.yml`
2. 编写 `Dockerfile` 与 `docker-compose.yml`
3. 在 `k8s/` 下草拟部署文件
4. 更新 `CHANGELOG.md`

**验收标准**
- GitHub Actions Lint/Test 绿色
- Docker 容器可启动并访问
- `docker-compose up` 正常

---

## 阶段 5：集成验收与性能优化（预估 1 周）

**目标**
- 完整 E2E 验证
- 压测与优化

**Tasks**
1. 编写 Playwright/Locust 脚本
2. 完成全流程测试
3. 优化异步、缓存、索引
4. 更新 `CHANGELOG.md`

**验收标准**
- End-to-End 请求成功率 ≥ 99%
- P95 响应 < 500ms

---

## 阶段 6：部署上线与运维监控（预估 3 天）

**目标**
- Kubernetes 集群部署
- Prometheus + Grafana 监控
- EFK/ELK 日志收集

**Tasks**
1. 部署至生产 K8s 集群
2. 配置 HPA
3. 搭建监控与日志平台
4. 更新 `CHANGELOG.md`

**验收标准**
- 服务稳定运行 24 小时
- 弹性伸缩正常
- 监控与日志检索就绪

---

> 完整节点性开发方案已在此文档中罗列，请基于此在 `develop` 分支上按阶段推进，确保每次变更及时更新 `CHANGELOG.md`。 