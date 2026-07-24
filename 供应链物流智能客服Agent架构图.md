# 供应链物流智能客服 — Agent 架构调度图

> 本文档用 Mermaid 图展示各主要物流供应链公司的 Agent 调度架构与技术路线

---

## 一、顺丰科技 Agent 架构

```mermaid
flowchart TB
    subgraph 用户触达层
        C["客户 / 快递员 / 企业用户"]
    end

    subgraph 智能体平台层 [基于 Dify 深度定制的企业级 Agent 平台]
        MG["模型广场<br>日均调用 10 亿+ Token"]
        EV["测评平台<br>多维度 Agent 评测"]
        OP["可观测平台<br>监控与追踪"]
    end

    subgraph 基座模型层
        FY["丰语多模态大模型<br>客服/营运/国际物流"]
        FZ["丰知物流决策大模型<br>预测/优化/分析"]
    end

    subgraph Agent 调度层 [5,000+ 智能体]
        SA["客服 Agent<br>意图识别→对话摘要→知识检索"]
        OA["营运 Agent<br>快件预测→资源调度→X光安检"]
        IA["国际 Agent<br>多语言→关务审单→合规"]
        CA["认知决策 Agent<br>网络规划→运力调度→异常处理"]
        subgraph 航空调度子流程
            direction TB
            A1["✈️ 异常事件触发"]
            A2["意图解析→LLM理解"]
            A3["算法调用→运筹优化"]
            A4["结果解读→自动执行"]
        end
    end

    subgraph RAG 支撑层
        DR["DRAGIN 实时动态检索<br>告警准确率 96%"]
        RT["RETRO/GCR 可控生成<br>一致率 99.3%"]
        AT["ATLAS 联合训练<br>分类准确率 95%"]
        GR["GraphRAG 图 RAG<br>合同/法务场景"]
    end

    subgraph 工具与执行层
        WMS["仓储系统"]
        TMS["运输系统"]
        OMS["订单系统"]
        ACS["航空控制系统"]
        CS["客服系统"]
    end

    C --> SA & OA & IA & CA
    SA & OA & IA & CA --> FY & FZ
    MG --> SA & OA & IA & CA
    EV --> SA & OA & IA & CA
    OP --> SA & OA & IA & CA
    FY & FZ --> DR & RT & AT & GR
    SA --> CS
    OA --> WMS & TMS & OMS
    IA --> CS & OMS
    CA --> ACS
    CA --> A1 --> A2 --> A3 --> A4
```

**调度逻辑说明：**
1. 用户请求 → 路由到对应领域 Agent（客服/营运/国际/决策）
2. Agent 调用「丰语/丰知」大模型进行语义理解与推理
3. 需要外部知识时 → 通过自研 RAG 体系（DRAGIN/RETRO/ATLAS/GraphRAG）检索
4. 需要执行操作时 → 调用对应业务系统（TMS/WMS/OMS）API
5. 返回结构化结果 → 可观测平台记录全链路 trace
6. 认知决策 Agent 具备自主闭环能力：异常触发 → 意图解析 → 算法调度 → 结果执行

---

## 二、京东物流（京小智）Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        S["百万商家"]
        C["消费者"]
    end

    subgraph 接入层
        DD["咚咚商家 IM"]
        WH["客服工作台"]
        PH["电话/在线"]
    end

    subgraph 京小智核心引擎 [Fast/Slow Thinking Dual-Mode]
        FT["⚡ 快思考模式<br>简单查询-毫秒级"]
        ST["🧠 慢思考模式<br>复杂问题-CoT推理"]
        subgraph 慢思考链路
            CT["CoT 思维链"]
            DC["动态上下文感知"]
            PI["潜在意图捕捉"]
        end
    end

    subgraph 全链路 Agent 数字员工 [覆盖客服/导购/跟单/分析/质检]
        CSA["客服 Agent<br>7×24 秒级响应"]
        SA["导购 Agent<br>智能推荐+转化"]
        OA["跟单 Agent<br>订单追踪+异常处理"]
        AA["分析 Agent<br>数据报表+趋势"]
        QA["质检 Agent<br>服务质量监控"]
    end

    subgraph 5G+AI 融合层 [携手中国移动+中兴]
        G5["5G 通信能力"]
        AI_OPEN["AI-Powered Open Gateway"]
    end

    subgraph 后端协同
        JOY["JoyAI 大模型"]
        KG["京东知识图谱"]
        LOG["京东物流履约系统"]
    end

    S --> DD
    C --> PH
    DD & WH & PH --> FT & ST
    ST --> CT & DC & PI
    FT & ST --> CSA & SA & OA & AA & QA
    CSA & SA & OA & AA & QA --> JOY
    JOY --> KG & LOG
    G5 --> AI_OPEN --> CSA
```

**调度逻辑说明：**
1. 商家/消费者 → 通过咚咚 IM / 客服工作台 / 电话进入
2. **京小智双模式调度器**判断：
   - **快思考**：简单查询（查物流、查订单）→ 毫秒级直接返回
   - **慢思考**：复杂问题（退换货纠纷、多订单合并）→ 启动 CoT 思维链 + 上下文感知 + 意图捕捉
3. 分配到对应 Agent 数字员工（客服/导购/跟单/分析/质检）
4. 调用 JoyAI 大模型 + 知识图谱 + 履约系统
5. 5G+AI 融合网关实现秒级响应
6. 全链路可解释：慢思考模式完整展示 AI 推理过程

---

## 三、菜鸟网络 Agent 架构

```mermaid
flowchart TB
    subgraph 用户与场景层
        CB["消费者"]
        MB["商家"]
        IB["国际客户"]
    end

    subgraph 智能客服层 [日均 500 万次服务]
        INT["意图识别引擎<br>动态调用知识库"]
        DIAL["多语言对话<br>情绪感知<br>200+ 国家"]
        AUTO["自动执行引擎<br>低代码逻辑链"]
    end

    subgraph Multi-Agent 调度 [全场景智能体]
        SCA["供应链 Agent<br>预测→库存→补货"]
        MCA["营销 Agent<br>转化优化"]
        MA["物流匹配 Agent<br>人-车-挂-货-路"]
        LA["本地生活 Agent<br>骑手调度"]
        CA["跨境 Agent<br>关务/多语言"]
    end

    subgraph 双 AI 协同层
        DA["决策式 AI<br>需求预测<br>库存优化<br>运输规划"]
        GA["生成式 AI<br>自然语言理解<br>对话生成<br>报告输出"]
    end

    subgraph 模型基座
        TJ["天机π<br>预测驱动型大模型"]
        FT["微调 + RLHF<br>物流领域持续预训练"]
    end

    subgraph 执行系统
        ROB["无人仓<br>1000+ 设备协同"]
        ROUT["路径优化<br>日 10 亿次规划"]
        CROSS["跨境物流<br>全球网络"]
    end

    CB & MB & IB --> INT & DIAL
    INT --> SCA & MCA & MA & LA & CA
    SCA --> DA
    MCA & MA & LA --> GA
    CA --> DA & GA
    DA & GA --> TJ --> FT
    SCA --> ROB & ROUT
    CA --> CROSS
    MA --> ROUT
```

**调度逻辑说明：**
1. 消费者/商家/国际客户 → 意图识别引擎 + 多语言对话入口
2. Route 到对应场景 Agent（供应链/营销/物流匹配/本地生活/跨境）
3. **双 AI 协同**：决策式 AI 负责预测与优化（确定性任务），生成式 AI 负责 NLU 与对话（开放性任务）
4. 供应链 Agent 接入无人仓和路径优化系统
5. 跨境 Agent 结合决策+生成双 AI，覆盖 200+ 国家和地区
6. 低代码执行引擎允许业务人员自定义服务逻辑链

---

## 四、满帮集团 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        S["货主 / 企业"]
        D["司机 / 车队"]
    end

    subgraph 满运大模型基座
        MM["满运大模型<br>已备案"]
        FC["快慢双循环<br>大模型理解 + 快速策略执行"]
    end

    subgraph Agent 网络 [三方协作]
        SA["货主 Agent<br>智能助理 2026<br>→ 发布需求<br>→ 比价推荐<br>→ 交易决策"]
        DA["司机 Agent<br>找货助手 2026<br>→ 智能找货<br>→ 路径建议<br>→ 接单决策"]
        PA["平台 Agent<br>调度与仲裁<br>→ 货源匹配<br>→ 信任机制<br>→ 风控"]
    end

    subgraph 核心能力
        MF["AI 智能找车<br>空驶距离 ↓27%"]
        SR["货源匹配<br>精准度 >90%"]
        TR["信任机制<br>评价+风控"]
    end

    subgraph 执行层
        AU["自动驾驶<br>智加领航<br>节油>10%"]
        API["开放 API<br>面向中大型物流企业"]
        EC["生态治理<br>错配治理+虚假账户清理"]
    end

    subgraph 远期目标 [2027+]
        TPN["三方 Agent 协作交易网络<br>货主Agent ↔ 平台Agent ↔ 司机Agent<br>自主协商 → 自动成交 → 智能履约"]
    end

    S --> SA
    D --> DA
    SA & DA --> FC --> MM
    PA --> FC
    SA & DA & PA --> MF & SR & TR
    PA --> EC
    DA --> AU
    SA & DA --> API
    SA -.->|远期| TPN
    DA -.->|远期| TPN
    PA -.->|远期| TPN
```

**调度逻辑说明：**
1. **快慢双循环**：大模型理解用户意图（慢）→ 策略引擎快速执行（快）
2. 三方 Agent 各司其职：
   - **货主 Agent**：代货主比价、推荐、辅助交易决策
   - **司机 Agent**：帮司机找货、规划路径、辅助接单决策
   - **平台 Agent**：居中调度、信任担保、风控仲裁
3. 当前阶段各 Agent 以**辅助决策**为主（建立信任）
4. 远期（2027+）演进为**三方自主协商 → 自动成交 → 智能履约**的全自动交易网络
5. 自动驾驶（智加领航）作为干线运输的物理执行层

---

## 五、圆通速递 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        S["寄件用户"]
        R["收件用户"]
        B["电商客户"]
    end

    subgraph 客服与仲裁层
        VS["语音客服<br>通义千问"]
        ARB["智能仲裁系统<br>DeepSeek 驱动<br>精准判责+理赔"]
        TK["工单处理<br>接待→处理→闭环"]
    end

    subgraph 自研智能体
        YTO["YTO-GPT 智多星<br>全网快递员 AI 助手"]
    end

    subgraph 多模型基座
        QW["通义千问<br>阿里云"]
        DS["DeepSeek<br>逻辑推理"]
        YG["YTO-GPT<br>自研"]
    end

    subgraph AI 执行层
        CV["机器视觉<br>边缘计算+异常识别"]
        DT["数字孪生<br>现场仿真+标准化"]
        SR["智慧路由<br>动态调度+优化"]
        UV["无人车<br>新能源短驳"]
    end

    S & R & B --> VS
    B --> ARB
    S & R --> TK
    VS & ARB & TK --> QW & DS & YG
    YTO --> DS & YG
    QW & DS & YG --> CV & DT & SR & UV
    ARB --> YTO
```

**调度逻辑说明：**
1. 用户通过语音/在线/工单进入客服系统
2. 简单问题 → 通义千问语音客服直接解决
3. 纠纷/理赔 → **智能仲裁系统**基于 DeepSeek 的逻辑推理能力精准判责
4. 快递员端 → YTO-GPT 智多星智能体提供 AI 助手服务
5. 后端多模型协同：通义千问处理通用对话，DeepSeek 处理逻辑推理，YTO-GPT 处理垂直场景
6. 视觉/数字孪生/智慧路由/无人车作为 AI 能力的物理执行出口

---

## 六、安得智联 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        B["品牌商 / 制造商"]
        R["零售商"]
        C["消费者"]
    end

    subgraph 安链通智慧供应链平台
        SCP["供应链决策大模型<br>专家级方案输出"]
        OPT["运筹优化算法<br>多仓串提+动态路径"]
        SIM["智能体仿真<br>场景推演"]
    end

    subgraph 安链云 SaaS 平台 [四流合一]
        BIZ["商流"]
        LOG["物流"]
        FIN["资金流"]
        INFO["信息流"]
    end

    subgraph AI 应用层
        PRED["多模态销量预测<br>生成式AI建模"]
        REP["智能补调<br>自动补货计划"]
        DIS["智能调度<br>车辆利用率↑"]
        CS["智慧客服<br>7×24 自动录单<br>RPA 财务对账"]
        TRACE["全链路可视<br>异常预警"]
    end

    subgraph 模型基座
        DR["DeepSeek-R1 满血版"]
        ECO["行业生态协同<br>智麦联采联盟"]
    end

    B & R & C --> SCP & CS
    SCP --> OPT & SIM
    OPT & SIM --> PRED & REP & DIS
    PRED & REP & DIS --> LOG
    CS --> LOG & FIN & INFO
    BIZ & LOG & FIN & INFO --> 安链云 SaaS 平台
    SCP --> DR
    DR --> ECO
    TRACE --> LOG
```

**调度逻辑说明：**
1. 品牌商/零售商 → 安链通平台（供应链决策大模型）
2. **决策大模型**输出专家级方案 → 运筹优化算法计算最优解 → 智能体仿真推演验证
3. **四流合一 SaaS 平台**：商流驱动物流、资金流、信息流协同
4. AI 应用层覆盖全链路：预测→补货→调度→客服→追踪
5. 客服场景：7×24 自动录单 + RPA 财务对账自动化
6. DeepSeek-R1 作为核心推理引擎，支持供应链决策
7. 通过"智麦联采"联盟实现行业生态协同

---

## 七、德邦快递 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        C["客户"]
        CS["客服坐席"]
    end

    subgraph 百度客悦智能客服平台
        NLU["NLP 意图识别<br>准确率 91%"]
        KB["物流知识库<br>3,000+ 专属问答"]
        IM["全渠道 IM 系统<br>日均 50 万次"]
        RAG["RAG 检索增强<br>精准匹配"]
    end

    subgraph 业务流程 Agent
        QA["查件 Agent<br>物流追踪"]
        PA["报价 Agent<br>运费计算"]
        CA["投诉 Agent<br>问题升级"]
        SA["调度 Agent<br>车辆安排"]
    end

    subgraph 后端系统对接
        TMS["运输管理系统 TMS"]
        WMS["仓储系统"]
        OMS["订单系统"]
    end

    subgraph 效果指标
        M1["转人工率<br>35% → 10% ↓71%"]
        M2["响应时间<br>120s → 15s ↓87%"]
        M3["解决率<br>76% → 92% ↑21%"]
        M4["单次成本<br>6.8→2.3元 ↓66%"]
        M5["年省费用<br>4,000 万元"]
    end

    C --> IM
    CS --> IM
    IM --> NLU --> RAG --> KB
    NLU --> QA & PA & CA & SA
    QA & PA & CA & SA --> TMS & WMS & OMS
    IM --> M1 & M2 & M3 & M4 & M5
    
    subgraph 未来规划
        AR["AR 虚拟顾问"]
        ML["多语言服务"]
        CAR["车机系统集成"]
    end
    QA & PA & CA & SA -.-> AR & ML & CAR
```

**调度逻辑说明：**
1. 客户/坐席 → 全渠道 IM 系统
2. NLP 意图识别（91% 准确率）→ 分类到对应业务 Agent
3. 各 Agent 通过 RAG 检索 3,000+ 条物流知识库
4. 需要执行操作时 → 对接 TMS/WMS/OMS 三大后端系统
5. 查件/报价/投诉/调度四种 Agent 各司其职
6. 简单问题直接解决，复杂问题转人工（仅 10%）
7. 效果数据透明可量化

---

## 八、行业 Agent 架构总览对比

```mermaid
flowchart LR
    subgraph 模型能力层
        DS["DeepSeek-R1"]
        QW["通义千问"]
        JOY["JoyAI"]
        FY["丰语/丰知"]
        TJ["天机π"]
        MY["满运"]
        YT["YTO-GPT"]
    end

    subgraph 架构特色层
        SF["顺丰<br>Dify+5000Agent<br>自研RAG体系"]
        JD["京东<br>快慢双模式<br>5G+AI融合"]
        CN["菜鸟<br>Multi-Agent<br>双AI协同"]
        MP["满帮<br>三方Agent网络<br>快慢双循环"]
        YT2["圆通<br>多模型融合<br>智能仲裁"]
        AD["安得<br>DeepSeek+运筹<br>四流合一"]
        DB["德邦<br>百度客悦<br>采购成熟方案"]
    end

    subgraph 核心能力层
        K1["客服问答"]
        K2["订单/物流追踪"]
        K3["智能调度/路由"]
        K4["供应链预测"]
        K5["国际/跨境"]
        K6["财务/RPA"]
        K7["仲裁/判责"]
    end

    subgraph 自动化程度
        L1["L1 人工辅助<br>德邦"]
        L2["L2 人机协同<br>圆通/中通"]
        L3["L3 自主执行<br>顺丰/京东/菜鸟"]
        L4["L4 全链路自动<br>满帮(远期)"]
    end

    DS --- AD
    QW --- YT2 & DB
    JOY --- JD
    FY --- SF
    TJ --- CN
    MY --- MP
    YT --- YT2

    SF --> K1 & K2 & K3 & K4 & K5
    JD --> K1 & K2 & K3 & K6
    CN --> K1 & K2 & K3 & K4 & K5
    MP --> K1 & K2 & K3
    YT2 --> K1 & K7
    AD --> K1 & K3 & K4 & K6
    DB --> K1 & K2

    SF --> L3
    JD --> L3
    CN --> L3
    MP --> L4
    YT2 --> L2
    AD --> L2
    DB --> L1
```

---

## 九、Agent 调度模式总结

| 企业 | 调度模式 | 核心调度器 | Agent 数量/类型 |
|------|----------|-----------|----------------|
| **顺丰科技** | 领域路由 + Dify 平台 | 自研 Agent 编排引擎 | 5,000+，4 大领域 |
| **京东物流** | 快慢双模式调度 | Fast/Slow Thinking 调度器 | 5 类数字员工 |
| **菜鸟网络** | 场景路由 + Multi-Agent | 意图识别 + 双 AI 协同 | 5 大场景 Agent |
| **满帮集团** | 三方 Agent 网络 | 快慢双循环调度 | 3 方 Agent |
| **圆通速递** | 多模型融合路由 | 问题分级调度 | 1 个 YTO-GPT |
| **安得智联** | 决策大模型 + 运筹优化 | 供应链决策引擎 | 多应用 Agent |
| **德邦快递** | 意图分类路由 | 百度客悦平台 | 4 类业务 Agent |

### 关键趋势

1. **从单 Agent 到 Multi-Agent**：顺丰（5,000+）、菜鸟（Multi-Agent）、满帮（三方网络）已率先进入多智能体协作阶段
2. **从串行到并行+博弈**：满帮的"货主-司机-平台"三方 Agent 不是简单的串行流水线，而是**博弈协商式调度**
3. **从外部采购到自研平台**：顺丰基于 Dify 定制、京东自研引擎、菜鸟自研 Multi-Agent 框架，头部企业都在构建自己的 Agent 基础设施
4. **快慢分离成为共识**：京东（快慢思考）、满帮（快慢双循环）不约而同选择了"简单问题秒级、复杂问题深度推理"的架构
5. **Agent 与物理世界闭环**：顺丰的航空调度 Agent 直接控制运力、菜鸟的 Agent 调度无人仓设备、满帮的 Agent 连接自动驾驶重卡 — Agent 正在从"信息层"走向"执行层"

---

*本文档由 Claude Code 基于公开信息调研整理，2026-07-23*
