# 供应链物流智能客服 — Agent 架构调度图

> 本文档用 Mermaid 图展示各主要物流供应链公司的 Agent 调度架构与技术路线

---

## 一、顺丰科技 Agent 架构

```mermaid
flowchart TB
    subgraph 用户触达层
        C["客户 / 快递员 / 企业用户"]
    end

    subgraph 智能体平台层
        MG["模型广场<br>日均调用10亿Token"]
        EV["测评平台"]
        OP["可观测平台"]
    end

    subgraph 基座模型层
        FY["丰语多模态大模型<br>客服/营运/国际物流"]
        FZ["丰知物流决策大模型<br>预测/优化/分析"]
    end

    subgraph Agent调度层
        SA["客服Agent<br>意图识别->对话摘要"]
        OA["营运Agent<br>快件预测->资源调度"]
        IA["国际Agent<br>多语言->关务审单"]
        CA["认知决策Agent<br>网络规划->异常处理"]
    end

    subgraph 航空调度
        A1["异常事件触发"]
        A2["意图解析 LLM理解"]
        A3["算法调用 运筹优化"]
        A4["结果解读 自动执行"]
    end

    subgraph RAG支撑层
        DR["DRAGIN实时动态检索<br>准确率96%"]
        RT["RETRO/GCR可控生成<br>一致率99.3%"]
        GR["GraphRAG图RAG<br>合同/法务场景"]
    end

    subgraph 工具与执行层
        WMS["仓储系统"]
        TMS["运输系统"]
        OMS["订单系统"]
        ACS["航空控制系统"]
        CS["客服系统"]
    end

    C --> SA
    C --> OA
    C --> IA
    C --> CA
    MG --> SA
    MG --> OA
    MG --> IA
    MG --> CA
    SA --> FY
    SA --> FZ
    OA --> FY
    OA --> FZ
    IA --> FY
    IA --> FZ
    CA --> FY
    CA --> FZ
    FY --> DR
    FY --> RT
    FY --> GR
    FZ --> DR
    FZ --> RT
    FZ --> GR
    SA --> CS
    OA --> WMS
    OA --> TMS
    OA --> OMS
    IA --> CS
    IA --> OMS
    CA --> ACS
    CA --> A1
    A1 --> A2
    A2 --> A3
    A3 --> A4
```

**调度逻辑说明：**
1. 用户请求被路由到对应领域 Agent（客服/营运/国际/决策）
2. Agent 调用「丰语/丰知」大模型进行语义理解与推理
3. 需要外部知识时通过自研 RAG 体系检索
4. 需要执行操作时调用对应业务系统 API
5. 认知决策 Agent 具备自主闭环能力：异常触发 → 意图解析 → 算法调度 → 结果执行

---

## 二、京东物流（京小智）Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        S["百万商家"]
        C["消费者"]
    end

    subgraph 接入层
        DD["咚咚商家IM"]
        WH["客服工作台"]
        PH["电话/在线"]
    end

    subgraph 京小智核心引擎
        FT["快思考模式<br>简单查询 毫秒级"]
        ST["慢思考模式<br>复杂问题 CoT推理"]
    end

    subgraph 慢思考链路
        CT["CoT思维链"]
        DC["动态上下文感知"]
        PI["潜在意图捕捉"]
    end

    subgraph 全链路Agent数字员工
        CSA["客服Agent<br>7x24秒级响应"]
        SA2["导购Agent<br>智能推荐+转化"]
        OA2["跟单Agent<br>订单追踪+异常处理"]
        AA["分析Agent<br>数据报表+趋势"]
        QA["质检Agent<br>服务质量监控"]
    end

    subgraph 后端协同
        JOY["JoyAI大模型"]
        KG["京东知识图谱"]
        LOG["京东物流履约系统"]
    end

    subgraph 5G+AI融合
        G5["5G通信能力"]
        AI_OPEN["AI-Powered Open Gateway"]
    end

    S --> DD
    C --> PH
    DD --> FT
    DD --> ST
    WH --> FT
    WH --> ST
    PH --> FT
    PH --> ST
    ST --> CT
    ST --> DC
    ST --> PI
    FT --> CSA
    FT --> SA2
    FT --> OA2
    FT --> AA
    FT --> QA
    ST --> CSA
    ST --> SA2
    ST --> OA2
    ST --> AA
    ST --> QA
    CSA --> JOY
    SA2 --> JOY
    OA2 --> JOY
    AA --> JOY
    QA --> JOY
    JOY --> KG
    JOY --> LOG
    G5 --> AI_OPEN
    AI_OPEN --> CSA
```

**调度逻辑说明：**
1. **京小智双模式调度器**判断：快思考（简单查询毫秒级）或慢思考（复杂问题 CoT 推理）
2. 分配到对应 Agent 数字员工（客服/导购/跟单/分析/质检）
3. 调用 JoyAI 大模型 + 知识图谱 + 履约系统
4. 5G+AI 融合网关实现秒级响应
5. 慢思考模式完整展示 AI 推理过程，具备可解释性

---

## 三、菜鸟网络 Agent 架构

```mermaid
flowchart TB
    subgraph 用户与场景层
        CB["消费者"]
        MB["商家"]
        IB["国际客户"]
    end

    subgraph 智能客服层
        INT["意图识别引擎<br>动态调用知识库"]
        DIAL["多语言对话<br>情绪感知 200+国家"]
        AUTO["自动执行引擎<br>低代码逻辑链"]
    end

    subgraph Multi-Agent调度
        SCA["供应链Agent<br>预测->库存->补货"]
        MCA["营销Agent<br>转化优化"]
        MA["物流匹配Agent<br>人-车-挂-货-路"]
        LA["本地生活Agent<br>骑手调度"]
        CA2["跨境Agent<br>关务/多语言"]
    end

    subgraph 双AI协同层
        DA["决策式AI<br>需求预测/库存优化/运输规划"]
        GA["生成式AI<br>自然语言理解/对话生成"]
    end

    subgraph 模型基座
        TJ["天机pi<br>预测驱动型大模型"]
        FT["微调+RLHF<br>物流领域持续预训练"]
    end

    subgraph 执行系统
        ROB["无人仓<br>1000+设备协同"]
        ROUT["路径优化<br>日10亿次规划"]
        CROSS["跨境物流<br>全球网络"]
    end

    CB --> INT
    MB --> INT
    IB --> DIAL
    INT --> SCA
    INT --> MCA
    INT --> MA
    INT --> LA
    INT --> CA2
    SCA --> DA
    MCA --> GA
    MA --> GA
    LA --> GA
    CA2 --> DA
    CA2 --> GA
    DA --> TJ
    GA --> TJ
    TJ --> FT
    SCA --> ROB
    SCA --> ROUT
    CA2 --> CROSS
    MA --> ROUT
```

**调度逻辑说明：**
1. 消费者/商家/国际客户 → 意图识别引擎 + 多语言对话入口
2. Route 到对应场景 Agent（供应链/营销/物流匹配/本地生活/跨境）
3. **双 AI 协同**：决策式 AI 负责预测与优化，生成式 AI 负责 NLU 与对话
4. 供应链 Agent 接入无人仓和路径优化系统
5. 跨境 Agent 结合决策+生成双 AI，覆盖 200+ 国家和地区

---

## 四、满帮集团 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        S["货主 / 企业"]
        D["司机 / 车队"]
    end

    subgraph 满运大模型基座
        MM["满运大模型 已备案"]
        FC["快慢双循环<br>大模型理解+快速策略执行"]
    end

    subgraph Agent网络 三方协作
        SA["货主Agent 2026<br>发布需求->比价推荐->交易决策"]
        DA["司机Agent 2026<br>智能找货->路径建议->接单决策"]
        PA["平台Agent<br>货源匹配->信任机制->风控"]
    end

    subgraph 核心能力
        MF["AI智能找车<br>空驶距离下降27%"]
        SR["货源匹配<br>精准度超过90%"]
        TR["信任机制<br>评价+风控"]
    end

    subgraph 执行层
        AU["自动驾驶<br>智加领航 节油超过10%"]
        API["开放API<br>面向中大型物流企业"]
        EC["生态治理<br>错配治理+虚假账户清理"]
    end

    subgraph 远期目标
        TPN["三方Agent协作交易网络<br>货主-平台-司机<br>自主协商-自动成交-智能履约"]
    end

    S --> SA
    D --> DA
    SA --> FC
    DA --> FC
    PA --> FC
    FC --> MM
    SA --> MF
    SA --> SR
    DA --> MF
    DA --> SR
    PA --> SR
    PA --> TR
    PA --> EC
    DA --> AU
    SA --> API
    DA --> API
    SA -.->|远期| TPN
    DA -.->|远期| TPN
    PA -.->|远期| TPN
```

**调度逻辑说明：**
1. **快慢双循环**：大模型理解用户意图（慢）→ 策略引擎快速执行（快）
2. 三方 Agent 各司其职：货主 Agent（比价推荐）、司机 Agent（找货接单）、平台 Agent（调度仲裁）
3. 当前阶段各 Agent 以辅助决策为主，建立信任
4. 远期（2027+）演进为三方自主协商 → 自动成交 → 智能履约的全自动交易网络
5. 自动驾驶（智加领航）作为干线运输的物理执行层

---

## 五、圆通速递 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        S1["寄件用户"]
        R["收件用户"]
        B["电商客户"]
    end

    subgraph 客服与仲裁层
        VS["语音客服 通义千问"]
        ARB["智能仲裁系统<br>DeepSeek驱动 精准判责+理赔"]
        TK["工单处理<br>接待->处理->闭环"]
    end

    subgraph 自研智能体
        YTO["YTO-GPT智多星<br>全网快递员AI助手"]
    end

    subgraph 多模型基座
        QW["通义千问"]
        DS["DeepSeek 逻辑推理"]
        YG["YTO-GPT 自研"]
    end

    subgraph AI执行层
        CV["机器视觉<br>边缘计算+异常识别"]
        DT["数字孪生<br>现场仿真+标准化"]
        SR2["智慧路由<br>动态调度+优化"]
        UV["无人车<br>新能源短驳"]
    end

    S1 --> VS
    R --> VS
    B --> ARB
    S1 --> TK
    R --> TK
    VS --> QW
    VS --> DS
    ARB --> QW
    ARB --> DS
    TK --> QW
    TK --> YG
    YTO --> DS
    YTO --> YG
    QW --> CV
    QW --> DT
    DS --> SR2
    YG --> UV
    ARB --> YTO
```

**调度逻辑说明：**
1. 用户通过语音/在线/工单进入客服系统
2. 简单问题 → 通义千问语音客服直接解决
3. 纠纷/理赔 → 智能仲裁系统基于 DeepSeek 精准判责
4. 快递员端 → YTO-GPT 智多星提供 AI 助手服务
5. 后端多模型协同：通义千问（通用对话）、DeepSeek（逻辑推理）、YTO-GPT（垂直场景）

---

## 六、安得智联 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        B1["品牌商 / 制造商"]
        R1["零售商"]
        C1["消费者"]
    end

    subgraph 安链通智慧供应链平台
        SCP["供应链决策大模型<br>专家级方案输出"]
        OPT["运筹优化算法<br>多仓串提+动态路径"]
        SIM["智能体仿真<br>场景推演"]
    end

    subgraph 安链云SaaS平台 四流合一
        BIZ["商流"]
        LOG["物流"]
        FIN["资金流"]
        INFO["信息流"]
    end

    subgraph AI应用层
        PRED["多模态销量预测<br>生成式AI建模"]
        REP["智能补调<br>自动补货计划"]
        DIS["智能调度<br>车辆利用率提升"]
        CS["智慧客服<br>7x24自动录单 RPA财务对账"]
        TRACE["全链路可视<br>异常预警"]
    end

    subgraph 模型基座
        DR1["DeepSeek-R1满血版"]
        ECO["行业生态协同<br>智麦联采联盟"]
    end

    B1 --> SCP
    B1 --> CS
    R1 --> SCP
    R1 --> CS
    C1 --> CS
    SCP --> OPT
    SCP --> SIM
    OPT --> PRED
    OPT --> REP
    OPT --> DIS
    SIM --> PRED
    SIM --> REP
    SIM --> DIS
    PRED --> LOG
    REP --> LOG
    DIS --> LOG
    CS --> LOG
    CS --> FIN
    CS --> INFO
    SCP --> DR1
    DR1 --> ECO
    TRACE --> LOG
```

**调度逻辑说明：**
1. 品牌商/零售商 → 安链通平台（供应链决策大模型）
2. 决策大模型输出专家级方案 → 运筹优化算法计算最优解 → 智能体仿真推演验证
3. **四流合一**：商流驱动物流、资金流、信息流协同
4. AI 应用层覆盖全链路：预测 → 补货 → 调度 → 客服 → 追踪
5. DeepSeek-R1 作为核心推理引擎，通过"智麦联采"联盟实现行业生态协同

---

## 七、德邦快递 Agent 架构

```mermaid
flowchart TB
    subgraph 用户层
        C2["客户"]
        CS2["客服坐席"]
    end

    subgraph 百度客悦智能客服平台
        NLU["NLP意图识别 准确率91%"]
        KB["物流知识库 3000+专属问答"]
        IM["全渠道IM系统 日均50万次"]
        RAG["RAG检索增强 精准匹配"]
    end

    subgraph 业务流程Agent
        QA2["查件Agent<br>物流追踪"]
        PA2["报价Agent<br>运费计算"]
        CA3["投诉Agent<br>问题升级"]
        SA3["调度Agent<br>车辆安排"]
    end

    subgraph 后端系统对接
        TMS["运输管理系统 TMS"]
        WMS["仓储系统"]
        OMS["订单系统"]
    end

    subgraph 效果指标
        M1["转人工率 35%降到10% 降幅71%"]
        M2["响应时间 120秒降到15秒 降幅87%"]
        M3["解决率 76%升到92% 升幅21%"]
        M4["单次成本 6.8元降到2.3元 降幅66%"]
        M5["年省费用 4000万元"]
    end

    subgraph 未来规划
        AR["AR虚拟顾问"]
        ML["多语言服务"]
        CAR["车机系统集成"]
    end

    C2 --> IM
    CS2 --> IM
    IM --> NLU
    NLU --> RAG
    RAG --> KB
    NLU --> QA2
    NLU --> PA2
    NLU --> CA3
    NLU --> SA3
    QA2 --> TMS
    PA2 --> WMS
    CA3 --> OMS
    SA3 --> TMS
    SA3 --> WMS
    IM --> M1
    IM --> M2
    IM --> M3
    IM --> M4
    IM --> M5
    QA2 -.->|未来| AR
    PA2 -.->|未来| ML
    CA3 -.->|未来| CAR
```

**调度逻辑说明：**
1. 客户/坐席 → 全渠道 IM 系统
2. NLP 意图识别（91% 准确率）→ 分类到对应业务 Agent
3. 各 Agent 通过 RAG 检索 3000+ 条物流知识库
4. 需要执行操作时对接 TMS/WMS/OMS 三大后端系统
5. 简单问题直接解决，复杂问题转人工（仅 10%）
6. 效果数据透明可量化：年省 4000 万

---

## 八、行业 Agent 架构总览对比

```mermaid
flowchart TB
    subgraph 模型能力层
        DS1["DeepSeek-R1"]
        QW1["通义千问"]
        JOY1["JoyAI"]
        FY1["丰语/丰知"]
        TJ1["天机pi"]
        MY1["满运"]
        YT1["YTO-GPT"]
    end

    subgraph 架构特色层
        SF1["顺丰<br>Dify+5000Agent+自研RAG"]
        JD1["京东<br>快慢双模式+5G+AI融合"]
        CN1["菜鸟<br>Multi-Agent+双AI协同"]
        MP1["满帮<br>三方Agent网络+快慢双循环"]
        YT2["圆通<br>多模型融合+智能仲裁"]
        AD1["安得<br>DeepSeek+运筹+四流合一"]
        DB1["德邦<br>百度客悦+采购成熟方案"]
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
        L1["L1人工辅助 德邦"]
        L2["L2人机协同 圆通/安得"]
        L3["L3自主执行 顺丰/京东/菜鸟"]
        L4["L4全链路自动 满帮(远期)"]
    end

    DS1 --- AD1
    QW1 --- YT2
    QW1 --- DB1
    JOY1 --- JD1
    FY1 --- SF1
    TJ1 --- CN1
    MY1 --- MP1
    YT1 --- YT2

    SF1 --> K1
    SF1 --> K2
    SF1 --> K3
    SF1 --> K4
    SF1 --> K5
    JD1 --> K1
    JD1 --> K2
    JD1 --> K3
    JD1 --> K6
    CN1 --> K1
    CN1 --> K2
    CN1 --> K3
    CN1 --> K4
    CN1 --> K5
    MP1 --> K1
    MP1 --> K2
    MP1 --> K3
    YT2 --> K1
    YT2 --> K7
    AD1 --> K1
    AD1 --> K3
    AD1 --> K4
    AD1 --> K6
    DB1 --> K1
    DB1 --> K2

    SF1 --> L3
    JD1 --> L3
    CN1 --> L3
    MP1 --> L4
    YT2 --> L2
    AD1 --> L2
    DB1 --> L1
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
2. **从串行到并行+博弈**：满帮的"货主-司机-平台"三方 Agent 是博弈协商式调度
3. **从外部采购到自研平台**：头部企业都在构建自己的 Agent 基础设施
4. **快慢分离成为共识**：京东（快慢思考）、满帮（快慢双循环）都选择了"简单问题秒级、复杂问题深度推理"的架构
5. **Agent 与物理世界闭环**：顺丰的航空调度 Agent 控制运力、菜鸟的 Agent 调度无人仓设备、满帮的 Agent 连接自动驾驶重卡

---

*本文档由 Claude Code 基于公开信息调研整理，2026-07-23*
