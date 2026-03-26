# CyberYuan Base

> **CyberYuan Base** 是 CyberYuan 产品家族的基础框架层，提供引擎抽象、碰撞检测、数据持久化、录制回放和数据库管理等核心能力。

## 依赖说明

| 仓库 | 是否需要 Base |
|------|:------------:|
| **CyberYuan-Base** | -- (本仓库) |
| [RobotMatrix_Release](https://github.com/Yuan5520/RobotMatrix_Release) | 需要 |
| [RealTimeDrive_Release](https://github.com/Yuan5520/RealTimeDrive_Release) | 需要 |

**Base 是所有 CyberYuan 产品的必备依赖。** 请在导入其他产品仓库之前，先将本仓库导入到 Unity 项目中。

---

## 安装

1. 将本仓库克隆或下载到 Unity 项目的 `Assets/` 目录下
2. 确保以下目录结构正确放置：
   - `Assets/RobotMatrix/Math/`
   - `Assets/IEngine/`
   - `Assets/Collision/`
   - `Assets/Persistence/`
   - `Assets/Recorder/`
   - `Assets/DBManager/`
   - `Assets/Plugins/MySQLPlugins/`
3. Unity 将自动识别 Assembly Definition 并完成编译

## 系统要求

- Unity 2021.3 LTS 或更高版本
- .NET Standard 2.1 / .NET Framework 4.x

---

## 模块概览

### RobotMatrix.Math -- 数学基础库

提供与 Unity 引擎解耦的数学类型，用于跨平台运动学计算。

- `RMVector3` -- 三维向量，支持叉积、点积、归一化等操作
- `RMQuaternion` -- 四元数，支持球面插值 (Slerp)、欧拉角互转
- `RMMatrix4x4` -- 4x4 齐次变换矩阵，支持 DH 参数构建
- `RMMatrixMN` -- M x N 通用矩阵，支持转置、伪逆运算（用于雅可比计算）
- `RMMathUtils` -- 角度弧度转换、范围限制等工具方法
- `RMGeometryUtils` -- 几何辅助函数

### IEngine -- 引擎抽象层

将物理引擎、渲染引擎、输入系统抽象为接口，使上层业务逻辑不直接依赖 Unity API。

**核心接口：**
- `IEngineHandle` -- 引擎对象句柄（位置、旋转、层级）
- `IEngineObject` -- 对象创建与查找
- `IEnginePhysics` -- 物理查询（射线检测、碰撞体管理）
- `IEngineInput` -- 输入设备抽象
- `IEngineVisualizer` -- 可视化绘制（线段、球体、标签）
- `ICollisionListener` -- 碰撞事件监听

**Unity 适配实现：**
- `UnityAdapterFactory` -- 工厂类，一键创建 Unity 适配器
- `UnityCollisionBridge` -- 将 Unity OnCollision 事件桥接到 IEngine 体系
- `TypeConverter` -- RMVector3/RMQuaternion 与 Unity Vector3/Quaternion 互转

### Collision -- 碰撞检测系统

基于关节分段的机器人碰撞检测与评分系统。

- `CollisionDetector` -- 实时碰撞检测引擎
- `ColliderManager` -- 碰撞体批量管理
- `CollisionScorer` -- 碰撞严重度评分
- `CollisionSuiteController` -- 碰撞检测套件主控制器
- `JointSegmenter` -- 关节链自动分段
- `StaticInterferenceCalibrator` -- 静态干涉校准（过滤安装态固有碰撞）

### Persistence -- 数据持久化

高性能异步数据写入系统，用于实验数据的文件存储。

- `AsyncBatchWriter` -- 异步批量写入器，减少 I/O 阻塞
- `PersistenceManager` -- 持久化会话管理
- `FilePathResolver` -- 文件路径解析与构建
- `IPersistenceSession` -- 会话接口

### Recorder -- 录制回放

关节运动数据的采样、平滑和会话化录制。

- `JointDataSampler` -- 多关节同步采样器
- `JointDataSmoother` -- 数据平滑滤波器
- `RecordingSession` -- 录制会话管理
- `RecorderSuiteController` -- 录制套件主控制器

### DBManager -- 数据库管理

MySQL 数据库连接池管理、异步操作与表结构校验。

- `MySqlConnectionPool` -- 连接池实现，支持自动重连
- `MySQLAsyncHandler` -- 异步数据库操作封装
- `DatabaseInitializer` -- 数据库/表自动初始化
- `TableSchemaValidator` -- 运行时表结构校验
- `DatabaseManager` -- 统一管理入口
- `DatabaseConfig` -- 可视化数据库配置 (ScriptableObject-like 组件)
- `MainThreadDispatcher` -- 异步结果回调到主线程
- `SqlExporter` -- SQL 导出工具

---

## 产品功能

- **引擎无关架构**：通过 IEngine 抽象层，业务代码不直接依赖 Unity，方便未来迁移至其他引擎
- **高精度数学库**：自研数学类型，避免 Unity float 精度限制，支持双精度运算
- **实时碰撞检测**：基于关节分段的碰撞检测，支持严重度评分和静态干涉过滤
- **异步数据管道**：Persistence + Recorder 提供高吞吐量的实验数据录制能力
- **MySQL 连接池**：生产级数据库管理，支持连接池、自动重连、表结构校验

## 产品亮点

1. **模块化设计** -- 每个模块独立 Assembly Definition，按需引用，最小化编译依赖
2. **零侵入集成** -- 通过 asmdef 引用机制集成，不修改现有项目结构
3. **生产级数据库** -- 内置连接池、异步操作、表结构校验，开箱即用
4. **跨平台数学** -- 纯 C# 数学库，不依赖 UnityEngine，可用于 Editor 工具和 CI 测试

---

## 技术支持

如有问题请联系 CyberYuan 技术支持团队。
