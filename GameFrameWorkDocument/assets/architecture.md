# 架构图与时序图汇总

> 本文件汇总了所有重要的可视化图表，便于在不打开 HTML 的情况下集中查阅。

## 1. 三层架构

```mermaid
flowchart TB
  subgraph U[上层 - 业务/Unity 适配]
    Game[游戏业务代码]
    UGF[UnityGameFramework]
  end
  subgraph G[中间层 - GameFramework]
    Entry[GameFrameworkEntry]
    Modules[22 个 Manager]
    Base[Base 基础设施]
  end
  subgraph E[底层 - 引擎/系统]
    Unity[UnityEngine]
    Sys[System.IO/Net]
  end
  Game --> UGF --> Modules --> Base
  Modules -. 引擎能力 .-> UGF --> Unity
  Modules --> Sys
```

## 2. 模块全景

```mermaid
mindmap
  root((GameFramework))
    基础
      Base
        ReferencePool
        EventPool
        TaskPool
        Log
        LinkedList
        Variable
        Version
    控制
      Procedure
      Fsm
      Event
    资源
      FileSystem
      Resource
      Download
      WebRequest
    数据
      Config
      DataTable
      DataNode
      Localization
      Setting
    表现
      ObjectPool
      Entity
      UI
      Scene
      Sound
    工具
      Network
      Debugger
      Utility
```

## 3. 模块加载时序

```mermaid
sequenceDiagram
  participant App as Unity 主循环
  participant Entry as GameFrameworkEntry
  participant Mod as 各 Manager

  App->>Entry: GetModule<I*Manager>() (首次)
  Entry->>Mod: Activator.CreateInstance
  Entry->>Entry: 按 Priority 降序插入

  loop 每帧
    App->>Entry: Update(...)
    Entry->>Mod: foreach Update (Priority 高先)
  end

  App->>Entry: Shutdown()
  Entry->>Mod: 反向遍历 Shutdown
  Entry->>Entry: ReferencePool.ClearAll()
```

## 4. 接口 + 实现 + Helper 模式

```mermaid
classDiagram
  class IXxxManager { <<interface>> }
  class XxxManager { <<internal sealed>> }
  class GameFrameworkModule { <<abstract>> +Priority +Update +Shutdown }
  class IXxxHelper { <<interface>> }
  class UnityXxxHelper { "Unity 工程实现" }
  GameFrameworkModule <|-- XxxManager
  IXxxManager <|.. XxxManager
  XxxManager o--> IXxxHelper
  IXxxHelper <|.. UnityXxxHelper
```

## 5. 引用池

```mermaid
classDiagram
  class ReferencePool {
    +Acquire~T~()
    +Release(IReference)
    +ClearAll()
  }
  class ReferenceCollection { -Queue<IReference> m_References }
  class IReference { <<interface>> +Clear() }
  ReferencePool *-- ReferenceCollection
  ReferenceCollection ..> IReference
```

## 6. 事件池时序

```mermaid
sequenceDiagram
  participant T1 as 异步线程
  participant T2 as 主线程
  participant Pool as EventPool
  T1->>Pool: Fire(args)
  Pool->>Pool: lock { Enqueue }
  loop 每帧
    T2->>Pool: Update
    Pool->>T2: dispatch handlers
  end
```

## 7. 任务池模型

```mermaid
flowchart LR
  T1[Task A] --> Q{{优先级链表}}
  T2[Task B] --> Q
  T3[Task C] --> Q
  Q --> A1((Agent 1))
  Q --> A2((Agent 2))
  A1 --> Done1[(Done)]
  A2 --> Done2[(Done)]
```

## 8. FSM 生命周期

```mermaid
stateDiagram-v2
  [*] --> Registered: CreateFsm
  Registered --> Running: Start
  Running --> OnEnter
  OnEnter --> OnUpdate
  OnUpdate --> OnUpdate: 每帧
  OnUpdate --> OnLeave: ChangeState
  OnLeave --> OnEnter: 切换新状态
  Running --> OnDestroy: DestroyFsm
  OnDestroy --> [*]
```

## 9. 启动 Procedure 流程

```mermaid
flowchart LR
  A[Launch]-->B[Splash]-->C[CheckVersion]-->D{需要更新}
  D-- 是 -->E[UpdateRes]-->F[VerifyRes]-->G[Preload]
  D-- 否 -->G
  G-->H[Login]-->I[Main]
```

## 10. 资源加载

```mermaid
sequenceDiagram
  participant Biz
  participant RM as ResourceManager
  participant TP as TaskPool
  participant Agent as LoadResourceAgent
  participant Helper as ILoadResourceAgentHelper

  Biz->>RM: LoadAsset
  RM->>TP: AddTask
  TP->>Agent: Start
  Agent->>Helper: ReadFile / LoadAsset
  Helper-->>Agent: 完成
  Agent->>RM: Done
  RM-->>Biz: SuccessCallback
```

## 11. 资源更新

```mermaid
sequenceDiagram
  participant App
  participant VLP as VersionListProcessor
  participant Chk as ResourceChecker
  participant Up as ResourceUpdater
  participant Ver as ResourceVerifier
  participant DL as DownloadManager
  App->>VLP: 下载版本表
  VLP-->>App: 完成
  App->>Chk: 对比
  Chk-->>App: 差异列表
  App->>Up: 下载
  Up->>DL: AddDownload
  DL-->>Up: 完成
  Up-->>App: 完成
  App->>Ver: CRC32 校验
  Ver-->>App: 完成
```

## 12. UI 打开

```mermaid
sequenceDiagram
  participant Biz
  participant UIM as UIManager
  participant Pool as 对象池
  participant Res as ResourceManager
  Biz->>UIM: OpenUIForm
  UIM->>Pool: Spawn
  alt 池中有
    Pool-->>UIM: 直接复用
  else 池中无
    UIM->>Res: LoadAsset
    Res-->>UIM: prefab
    UIM->>Pool: Register
  end
  UIM->>UIM: 重排深度
  UIM-->>Biz: SuccessEvent
```

## 13. UI 分组

```mermaid
flowchart TD
  UIM[UIManager]
  UIM-->BG[Background<br/>Depth=0]
  UIM-->DEF[Default<br/>Depth=10]
  UIM-->HUD[HUD<br/>Depth=20]
  UIM-->TOP[Top<br/>Depth=100]
  DEF-->F1[MainMenu]
  HUD-->F2[HpBar]
```

## 14. 网络通信

```mermaid
flowchart LR
  App-->|Send|Ch[NetworkChannel]
  Ch-->|Serialize|SS[SendState]
  SS-->Sock[(Socket)]
  Sock-->RS[ReceiveState]
  RS-->|按 Header 解|PH[IPacketHandler]
  PH-->App
  Ch-.周期.->HB[HeartBeat]
```

## 15. FileSystem 文件格式

```
┌─────────────────────────┐
│ HeaderData              │  魔法字 + 加密 + 最大文件数 + 最大块数
├─────────────────────────┤
│ StringData[]            │  文件名字符串区
├─────────────────────────┤
│ BlockData[]             │  块表（StringIndex + Offset + Length）
├─────────────────────────┤
│ Block 数据区             │  实际文件内容（按块对齐）
└─────────────────────────┘
```
