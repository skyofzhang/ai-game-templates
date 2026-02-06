# 游戏开发的程序知识库（Unity） v2.1.1

> **面向**：程序 AI / Unity 开发者  
> **依据**：《游戏生成的需求知识库 v2.2》《游戏设计的策划知识库 v2.2》《游戏开发美术知识库 v2.2》《00_项目基础规范》《验收与口径》（0 章与 UI 清单以 00 为准，术语与首版以验收与口径为准）。  
> **使用场景**：Unity 工程开发、代码审查与重构、技术架构与自动化测试设计。

---

## 更新记录 (Changelog)

- **v2.1.1 (2026-01-30 晚上)**:
  - **表格修复**: 更新StatType枚举，新增Gold/LifeSteal/DamageReduction等属性，使用显式枚举值（0, 1, 2...）和分组注释，与《数值策划案_v3.1》完全对齐。
  - **表格修复**: 扩展脚本职责边界表，新增ExperienceManager/DropPickup/BoundaryWalls等核心脚本定义。
  - **吸收Kimi AI的合理修改**：吸收了Kimi AI v2.2版本中关于StatType枚举和脚本职责边界的优化，但保留了Manus v2.1的完整Cursor实战反馈代码实现（第10章）。
- **v2.1 (2026-01-30)**:
  - 新增“**游戏基础规范**”章节（第0章），明确定义游戏类型、屏幕方向、分辨率、Unity版本、平台等基础规范。
  - 在“工程落地补充”中整合Cursor实战反馈，补充存档系统、掉落拾取、技能配置化、事件监听时序、场景边界、多AudioListener等实战经验。
  - 在“编码规范与反模式”中补充Cursor实战中发现的常见问题和解决方案。
  - 更新所有对需求知识库、策划知识库、美术知识库的引用版本为v2.2。
  - **v2.1.1 (2026-01-30 下午)**: 根据Kimi AI的质疑进行第三轮深度自检，修复了以下问题：
    - 补充StatType枚举的缺失属性：MaxHP、Level、CurrentExp、NextLevelExp
    - 补充核心脚本职责边界定义：SkillManager、SaveSystem、InventoryManager、EquipmentManager
  - **历史性提升记录**: 本次更新基于2026-01-29的Cursor实战反馈，补充了大量“隐性知识”（人类开发者知道但AI不知道的知识），确保AI能够生成可运行的、符合规范的Unity工程。
- **v1.7 (2026-01-29)**:
  - 新增「工程落地补充」：存档/掉落拾取/技能配置化/事件监听时序/场景边界与多 AudioListener 的处理要点，作为 v1.6 的工程实操增量。
- **v1.6 (2026-01-29)**:
  - 在章节9中新增"配置数据验证机制"（章节9.3），包括格式验证、完整性验证、数值范围验证、引用完整性验证和资源路径验证。
  - 新增配置数据验证器基类和具体验证器实现示例。
  - 更新ConfigManager实现，集成配置数据验证功能。
  - 更新所有对策划知识库的引用版本为v2.2。
- **v1.5 (2026-01-29)**:
  - **级联更新**：同步《策划知识库 v2.2》的新增内容。
  - 新增章节6：游戏状态机实现规范（对应策划知识库的"游戏流程与状态机"）。
  - 新增章节7：UI系统实现规范（对应策划知识库的"UI系统设计规范"和"UI反馈系统设计规范"）。
  - 新增章节8：事件系统实现规范（对应策划知识库的"系统间协作与数据流转"）。
  - 新增章节9：配置数据加载与解析（对应策划知识库的"配置数据结构定义"）。
- **v1.4 (2026-01-29)**:
- 新增依赖：程序知识库现在同时依赖《游戏开发美术知识库 v2.2》，明确了程序如何加载和使用美术资产。
  - 同步版本号至v1.4，与其他知识库保持一致。
- **v1.3 (2026-01-28)**:
  - 在"自检、测试与自动修复"章节中新增"**自动化验收测试**"要求。
  - 明确规定程序AI**必须**解析GDD中的《游戏功能验收测试SOP》，并生成对应的Unity PlayMode Test自动化测试脚本。
- **v1.2 (2026-01-28)**: 
  - 同步更新版本号，与策划及需求知识库对齐。
  - 在目录结构和资源管理器中增加了对美术AI和音效AI产出资产的引用说明。
- **v1.1 (2026-01-28)**: 新增自动修复流程和脚本依赖关系约束。
- **v1.0 (2026-01-28)**: 初始版本。

---

## 0. 游戏基础规范（引用）

> **唯一口径**：游戏类型、屏幕、Unity 版本、平台、性能及优化约束以 **《00_项目基础规范》** 为准。路径：`知识库与策划案/00_项目基础规范.md`。所有程序AI生成的代码和配置必须严格遵循该文件。

---

## 1. Unity 工程结构与场景约定

### 1.1 目录结构（统一约定）

所有自动生成的工程应严格遵守以下目录结构。美术和音效资源将由对应的AI生成并放置在指定位置。

-   **Assets/Art/**
    -   `Prefabs/`: 由美术AI生成并符合策划规范的预制体。
        -   `Characters/`: 玩家与怪物预制体。
        -   `Environments/`: 场景模块预制体。
        -   `UI/`: UI 预制体。
    -   `Models/`: 角色、场景等模型文件。
    -   `Textures/`: 贴图文件。
    -   `VFX/`: 特效资源（粒子系统、序列帧）。
-   **Assets/Audio/**
    -   `Music/`: 背景音乐 (BGM)。
    -   `SFX/`: 音效 (Sound Effects)。
-   **Assets/Scripts/**
    -   `Managers/`: GameManager, UIManager, AudioManager 等。
    -   `Player/`: PlayerController, PlayerStats 等。
    -   `Monsters/`: MonsterController, MonsterStats, MonsterSpawner 等。
    -   `Combat/`: CombatSystem, ProjectileController 等。
    -   `Systems/`: StageSystem, LootManager, EconomySystem, EventManager 等。
    -   `UI/`: UI 逻辑脚本。
    -   `Data/`: 配置数据类（对应JSON配置文件的C#类）。
-   **Assets/Scenes/**: Boot, MainMenu, Gameplay 等场景文件。
-   **Assets/Resources/Config/**: JSON / ScriptableObject 等配置数据。
-   **Assets/Tests/PlayMode/**: 自动化验收测试脚本。

### 1.2 必需场景及职责

-   **Boot 场景**: 启动场景，负责初始化核心服务并加载 MainMenu 场景。
-   **MainMenu 场景**: 主界面，包含开始游戏、设置等入口。
-   **Gameplay 场景**: 核心战斗场景。

> **规则**: 场景切换统一通过 GameManager 或 SceneController 实现，禁止在任何普通脚本中直接调用 `SceneManager.LoadScene`。

---

## 2. 核心脚本与职责边界

> **说明**: 定义每类核心脚本的"管什么 / 不管什么"。程序 AI 生成脚本时必须遵守，避免"上帝类"和职责混乱。

-   **GameManager**: 负责游戏状态机（Init, MainMenu, Loading, InGame, Paused, Settlement）、关卡流程控制、状态转换。
-   **UIManager**: 负责管理UI面板的显示与隐藏、UI界面层级管理、UI堆栈管理，监听GameManager状态变化。
-   **AudioManager**: 
    -   **负责**: 背景音乐与音效播放。从 `Assets/Audio/` 目录中加载由音效AI生成的音频片段。
    -   **不负责**: 游戏逻辑判断。
-   **EventManager**: 负责全局事件的派发和监听，实现系统间的解耦通信。
-   **PlayerController**: 负责接收输入、控制玩家移动和朝向、触发攻击/技能。
-   **PlayerStats**: 负责存储和维护玩家所有属性，提供统一接口供其他系统查询和修改。
-   **MonsterController**: 负责怪物的AI行为（寻路、追踪、攻击）。
-   **MonsterStats**: 负责存储怪物的属性，处理伤害和死亡事件。
-   **MonsterSpawner**: 负责根据关卡配置在指定位置和时间生成怪物。
-   **CombatSystem**: 负责执行一次攻击的数值计算流程，复用策划知识库中定义的伤害公式。
-   **LootManager**: 负责监听怪物死亡事件，并根据掉落表计算实际掉落。
-   **ConfigManager**: 负责加载和解析所有JSON配置文件（关卡、怪物、装备、掉落表等）。
-   **SkillManager**: 
    -   **负责**: 技能系统的管理，包括技能配置加载、技能冷却管理、技能释放逻辑。
    -   **不负责**: 技能的具体效果实现（由CombatSystem执行）。
-   **SaveSystem**: 
    -   **负责**: 游戏存档的保存和加载，包括玩家属性、背包、装备、设置等数据的持久化。
    -   **不负责**: 游戏逻辑判断，仅提供数据存储和读取接口。
-   **InventoryManager**: 
    -   **负责**: 背包系统的管理，包括物品添加、删除、查询、排序等功能。
    -   **不负责**: UI显示（由UIManager负责）、装备穿戴逻辑（由EquipmentManager负责）。
-   **EquipmentManager**: 
    -   **负责**: 装备系统的管理，包括装备穿戴、卸下、属性计算等功能。
    -   **不负责**: 背包管理（由InventoryManager负责）、UI显示（由UIManager负责）。
-   **ExperienceManager**: 
    -   **负责**: 经验系统的管理，包括经验值计算、等级升级逻辑、升级奖励发放等功能。
    -   **不负责**: 经验值存储（由PlayerStats负责）、UI显示（由UIManager负责）。
-   **DropPickup**: 
    -   **负责**: 掉落物实体的管理，包括掉落物生成、拾取检测、拾取逻辑等功能。
    -   **不负责**: 掉落表计算（由LootManager负责）、背包管理（由InventoryManager负责）。
-   **BoundaryWalls**: 
    -   **负责**: 场景边界的管理，包括边界墙体生成、边界检测等功能。
    -   **不负责**: 玩家移动逻辑（由PlayerController负责）、场景加载（由GameManager负责）。

---

## 3. 统一数据结构与接口

### 3.1 属性枚举 (StatType)

所有属性访问应使用此枚举，与策划知识库保持一致。

```csharp
public enum StatType {
    // ========== 基础属性 ==========
    /// <summary>当前生命值</summary>
    HP = 0,

    /// <summary>最大生命值</summary>
    MaxHP = 1,

    /// <summary>攻击力</summary>
    Attack = 2,

    /// <summary>防御力</summary>
    Defense = 3,

    /// <summary>移动速度（米/秒）</summary>
    MoveSpeed = 4,

    /// <summary>攻击速度（攻击间隔秒数，越小越快）</summary>
    AttackSpeed = 5,

    // ========== 暴击属性 ==========
    /// <summary>暴击率（0-1小数，如0.15表示15%）</summary>
    CritChance = 10,

    /// <summary>暴击伤害系数（如1.5表示150%伤害）</summary>
    CritDamage = 11,

    // ========== 成长属性 ==========
    /// <summary>当前等级</summary>
    Level = 20,

    /// <summary>当前经验值</summary>
    CurrentExp = 21,

    /// <summary>升级所需经验</summary>
    NextLevelExp = 22,

    // ========== 资源属性 ==========
    /// <summary>金币数量</summary>
    Gold = 30,

    // ========== 特殊属性（预留） ==========
    /// <summary>生命偷取（0-1小数）</summary>
    LifeSteal = 40,

    /// <summary>伤害减免（0-1小数）</summary>
    DamageReduction = 41,
}
```

> **重要说明**：
> - **MaxHP**：最大生命值，用于计算血条百分比（HP / MaxHP）。必须与HP分开，因为HP是当前值，MaxHP是上限。
> - **Level**：等级，用于角色成长系统。
> - **CurrentExp**：当前经验值，用于计算经验条百分比（CurrentExp / NextLevelExp）。
> - **NextLevelExp**：升级所需经验值，用于判断是否可以升级。
> - **CritDamage**：暴击伤害系数，为浮点数，如1.5表示150%伤害，2.0表示200%伤害。

### 3.2 IStatsProvider 接口

PlayerStats 和 MonsterStats 都必须实现此接口，为战斗系统提供统一的属性访问方式。

```csharp
public interface IStatsProvider {
    float GetStat(StatType type);
    void ModifyStat(StatType type, float delta);
}
```

---

## 4. 自检、测试与自动修复

> **原则**: 程序 AI 必须在"宣称可体验"之前，自动生成并执行以下自检和测试逻辑，确保交付质量。

### 4.1 SanityCheck 组件

在Gameplay场景中必须存在一个SanityCheck组件，在启动时检查：
1.  **核心对象存在性**: 玩家、怪物刷怪点、核心UI Canvas是否存在。
2.  **核心预制体链接**: 检查 `MonsterSpawner` 等脚本是否正确链接了由美术AI生成的怪物预制体。
3.  **数值护栏检查**: 检查玩家移动速度等关键数值是否在策划定义的"护栏"范围内。

若有缺失或异常，应打印清晰的错误日志。

### 4.2 自动修复流程 (Auto-Fix Workflow)

当SanityCheck检测到可自动修复的问题时，应触发修复流程。例如，若玩家对象缺失，则从 `Assets/Art/Prefabs/Characters/` 目录下实例化一个默认玩家预制体。

| 问题类型 | 检测条件 | 自动修复方案 |
|---|---|---|
| 玩家对象缺失 | 场景中无Player对象 | 从`Prefabs/Characters/Player.prefab`实例化到(0,0,0) |
| 怪物刷怪点缺失 | 场景中无MonsterSpawner | 在(0,0,5)位置创建MonsterSpawner对象并挂载脚本 |
| HUD Canvas缺失 | 场景中无HUD Canvas | 从`Prefabs/UI/HUD.prefab`实例化 |

> **失败处理**: 如果连续修复3次后仍然失败，记录详细日志并标记为"需要人类介入"。

### 4.3 自动化验收测试

> **原则**: 这是保证游戏核心流程符合策划意图的最后一道防线。程序AI必须将此作为交付前的强制步骤。

#### 4.3.1 测试脚本生成流程

1.  **输入**: GDD附录中的《游戏功能验收测试SOP》。
2.  **解析SOP**: 程序AI**必须**解析SOP中的每一个测试用例（Test Case），提取以下信息：
    -   测试用例ID
    -   前置条件
    -   操作步骤（按顺序）
    -   预期结果（与操作步骤一一对应）
    -   自动化可行性（仅处理标记为"高"的用例）
3.  **生成测试脚本**: 对于"自动化可行性"为"高"的用例，生成对应的Unity PlayMode Test测试脚本。
4.  **执行测试**: 在Unity Test Runner中执行所有测试，并记录测试结果。
5.  **交付标准**: **所有自动化测试用例必须全部通过（Pass）**，才可认为满足了需求知识库中定义的"测试闭环"成功标准。

#### 4.3.2 测试脚本生成规范

**文件命名规范**:
-   测试文件命名格式：`[TestCaseID]Test.cs`
-   例如：`TC_GAMEPLAY_001Test.cs`
-   存放路径：`Assets/Tests/PlayMode/`

**测试类结构规范**:
```csharp
using UnityEngine;
using UnityEngine.TestTools;
using NUnit.Framework;
using System.Collections;

public class TC_GAMEPLAY_001Test
{
    [UnityTest]
    public IEnumerator TestCoreGameplayLoop()
    {
        // 测试逻辑
        yield return null;
    }
}
```

**操作步骤模拟规范**:

| SOP操作步骤 | Unity测试实现方法 | 示例代码 |
|---|---|---|
| 加载场景 | `SceneManager.LoadScene(sceneName)` | `SceneManager.LoadScene("MainMenu");` |
| 点击按钮 | `Button.onClick.Invoke()` 或 `EventSystem`模拟点击 | `startButton.onClick.Invoke();` |
| 模拟玩家移动 | 直接设置`PlayerController`的位置或调用移动方法 | `playerController.Move(direction);` |
| 模拟攻击 | 调用`CombatSystem`的攻击方法 | `combatSystem.Attack(target);` |
| 检查对象存在性 | `GameObject.Find()` 或 `FindObjectOfType()` | `Assert.IsNotNull(GameObject.Find("Player"));` |
| 检查数值 | `Assert.AreEqual()` 或 `Assert.IsTrue()` | `Assert.AreEqual(expectedHP, playerStats.GetHP());` |
| 等待动画/效果 | `yield return new WaitForSeconds(time)` | `yield return new WaitForSeconds(1.0f);` |
| 检查事件触发 | 监听事件并设置标志位 | `bool eventTriggered = false; EventManager.Subscribe("EVENT_NAME", () => eventTriggered = true);` |

**预期结果断言规范**:

| SOP预期结果类型 | Unity断言方法 | 示例代码 |
|---|---|---|
| 对象存在 | `Assert.IsNotNull()` | `Assert.IsNotNull(GameObject.Find("Player"));` |
| 对象不存在 | `Assert.IsNull()` | `Assert.IsNull(GameObject.Find("DeletedObject"));` |
| 数值相等 | `Assert.AreEqual()` | `Assert.AreEqual(100, playerStats.GetHP());` |
| 数值范围 | `Assert.Greater()` / `Assert.Less()` | `Assert.Greater(playerStats.GetHP(), 0);` |
| 布尔值 | `Assert.IsTrue()` / `Assert.IsFalse()` | `Assert.IsTrue(playerStats.IsAlive());` |
| 字符串匹配 | `Assert.AreEqual()` | `Assert.AreEqual("MainMenu", SceneManager.GetActiveScene().name);` |
| 场景切换 | `Assert.AreEqual()` | `Assert.AreEqual("Gameplay", SceneManager.GetActiveScene().name);` |

#### 4.3.3 测试脚本生成示例

**示例1：核心战斗循环测试 (TC-GAMEPLAY-001)**

```csharp
using UnityEngine;
using UnityEngine.TestTools;
using NUnit.Framework;
using System.Collections;
using UnityEngine.SceneManagement;

public class TC_GAMEPLAY_001Test
{
    private GameObject player;
    private PlayerController playerController;
    private PlayerStats playerStats;
    
    [SetUp]
    public void SetUp()
    {
        // 每个测试方法执行前的初始化
    }
    
    [TearDown]
    public void TearDown()
    {
        // 每个测试方法执行后的清理
    }
    
    [UnityTest]
    public IEnumerator TestCoreGameplayLoop()
    {
        // 步骤1：加载MainMenu场景
        SceneManager.LoadScene("MainMenu");
        yield return new WaitForSeconds(0.5f); // 等待场景加载
        Assert.AreEqual("MainMenu", SceneManager.GetActiveScene().name, "主界面场景加载失败");
        
        // 步骤2：点击"开始游戏"按钮
        GameObject startButton = GameObject.Find("_Button_Start");
        Assert.IsNotNull(startButton, "开始游戏按钮不存在");
        startButton.GetComponent<UnityEngine.UI.Button>().onClick.Invoke();
        yield return new WaitForSeconds(0.5f);
        
        // 步骤3：点击"第一关"按钮（假设在关卡选择界面）
        GameObject level1Button = GameObject.Find("_Button_Level1");
        Assert.IsNotNull(level1Button, "第一关按钮不存在");
        level1Button.GetComponent<UnityEngine.UI.Button>().onClick.Invoke();
        yield return new WaitForSeconds(1.0f); // 等待场景切换
        
        // 步骤4：验证Gameplay场景加载成功
        Assert.AreEqual("Gameplay", SceneManager.GetActiveScene().name, "游戏场景加载失败");
        
        // 步骤5：验证玩家对象存在
        player = GameObject.FindWithTag("Player");
        Assert.IsNotNull(player, "玩家对象不存在");
        
        // 步骤6：验证怪物生成
        yield return new WaitForSeconds(2.0f); // 等待怪物生成
        GameObject[] monsters = GameObject.FindGameObjectsWithTag("Monster");
        Assert.Greater(monsters.Length, 0, "怪物未生成");
        
        // 步骤7：模拟玩家攻击
        playerController = player.GetComponent<PlayerController>();
        playerController.Attack();
        yield return new WaitForSeconds(0.5f);
        
        // 步骤8：验证怪物受到伤害
        MonsterStats monsterStats = monsters[0].GetComponent<MonsterStats>();
        Assert.Less(monsterStats.GetStat(StatType.HP), monsterStats.GetMaxHP(), "怪物未受到伤害");
    }
}
```

#### 4.3.4 测试执行与报告

- 测试执行后，必须记录以下信息：
  - 总测试用例数
  - 通过的测试用例数
  - 失败的测试用例数
  - 每个失败用例的详细错误信息
- 如果存在失败的测试用例，必须分析失败原因并修复代码或测试脚本
- **交付标准**: 所有自动化测试用例必须全部通过（Pass），才可认为满足了需求知识库中定义的"测试闭环"成功标准

### 4.4 脚本依赖关系约束

脚本生成必须遵循以下依赖顺序，确保编译正确：
1.  **第一层（无依赖）**: `StatType` 枚举, `IStatsProvider` 接口, 配置数据类（如`LevelConfig`, `MonsterConfig`）。
2.  **第二层（依赖第一层）**: `PlayerStats`, `MonsterStats`, `EventManager`, `ConfigManager`。
3.  **第三层（依赖第二层）**: `PlayerController`, `MonsterController`。
4.  **第四层（依赖前序层）**: `CombatSystem`, `LootManager`, `MonsterSpawner`。
5.  **第五层（依赖所有）**: `GameManager`, `UIManager`, `AudioManager`。

---

## 5. 编码规范与反模式

-   **推荐规范**: 类名与文件名一致、避免魔法数、使用事件解耦。
-   **禁止的反模式**: 
    -   在运行时频繁使用 `FindObjectOfType` 查找依赖。
    -   UI脚本直接修改 `PlayerStats` 的内部字段。
    -   `GameManager` 变成包含所有逻辑的"上帝类"。
    -   在没有通过SanityCheck和**自动化验收测试**的前提下宣称"功能已完成"。

### 5.1 Cursor实战中发现的常见问题与解决方案

基于2026-01-29的Cursor实战反馈，以下是程序AI在生成代码时经常遇到的问题和解决方案：

#### 5.1.1 事件监听时序问题

**问题**: UI/Audio等在`Awake/Start`很早注册监听，但`EventManager.Instance`还没初始化，导致监听丢失。

**解决方案**: EventManager提供**pending listeners缓存**：Instance未创建时先缓存在静态容器，Instance创建后回放。

```csharp
public class EventManager : MonoBehaviour {
    private static EventManager instance;
    private static List<PendingListener> pendingListeners = new List<PendingListener>();
    
    void Awake() {
        if (instance == null) {
            instance = this;
            DontDestroyOnLoad(gameObject);
            
            // 回放pending listeners
            foreach (var pending in pendingListeners) {
                Subscribe(pending.eventName, pending.listener);
            }
            pendingListeners.Clear();
        } else {
            Destroy(gameObject);
        }
    }
    
    public static void Subscribe(string eventName, Action<object> listener) {
        if (instance == null) {
            // 缓存到pending列表
            pendingListeners.Add(new PendingListener { eventName = eventName, listener = listener });
        } else {
            // 正常注册
            if (!instance.events.ContainsKey(eventName)) {
                instance.events[eventName] = null;
            }
            instance.events[eventName] += listener;
        }
    }
    
    private struct PendingListener {
        public string eventName;
        public Action<object> listener;
    }
}
```

#### 5.1.2 场景边界问题

**问题**: 玩家可以移动到地图边界之外，导致视觉错误。

**解决方案**: 优先使用物理墙（BoxCollider），Clamping仅作为兜底。

```csharp
// 在Gameplay场景中生成4面物理墙
public class MapBoundary : MonoBehaviour {
    public float mapSize = 80f; // 80m × 80m
    
    void Start() {
        CreateBoundaryWalls();
    }
    
    void CreateBoundaryWalls() {
        float halfSize = mapSize / 2f;
        
        // 上墙
        CreateWall(new Vector3(0, 0, halfSize), new Vector3(mapSize, 10, 1));
        // 下墙
        CreateWall(new Vector3(0, 0, -halfSize), new Vector3(mapSize, 10, 1));
        // 左墙
        CreateWall(new Vector3(-halfSize, 0, 0), new Vector3(1, 10, mapSize));
        // 右墙
        CreateWall(new Vector3(halfSize, 0, 0), new Vector3(1, 10, mapSize));
    }
    
    void CreateWall(Vector3 position, Vector3 size) {
        GameObject wall = new GameObject("BoundaryWall");
        wall.transform.position = position;
        BoxCollider collider = wall.AddComponent<BoxCollider>();
        collider.size = size;
    }
}
```

#### 5.1.3 多AudioListener问题

**问题**: Managers `DontDestroyOnLoad` 携带 `Camera/AudioListener`，进入 Gameplay 后与场景相机产生冲突。

**解决方案**: 在`SceneLoaded`回调中对Managers上的`Camera/AudioListener`做按场景切换的启用/禁用。

```csharp
public class GameManager : MonoBehaviour {
    void Awake() {
        SceneManager.sceneLoaded += OnSceneLoaded;
    }
    
    void OnSceneLoaded(Scene scene, LoadSceneMode mode) {
        // 在Gameplay场景中禁用Managers上的Camera/AudioListener
        if (scene.name == "Gameplay") {
            Camera managerCamera = GetComponentInChildren<Camera>();
            if (managerCamera != null) {
                managerCamera.enabled = false;
            }
            
            AudioListener managerListener = GetComponentInChildren<AudioListener>();
            if (managerListener != null) {
                managerListener.enabled = false;
            }
        }
    }
}
```

#### 5.1.4 Resources.Load路径问题

**问题**: 配置中的`prefab_path/icon_path`无法被`Resources.Load`命中，导致运行时错误。

**解决方案**: 配置中的路径必须是`Resources`相对路径（不含扩展名），并在配置加载验证时尝试`Resources.Load`。

```csharp
public class MonsterConfigValidator : ConfigValidator<MonsterConfig> {
    public override ValidationResult Validate(MonsterConfig config) {
        // 资源路径验证
        GameObject prefab = Resources.Load<GameObject>(config.prefab_path);
        if (prefab == null) {
            return CreateError($"预制体资源不存在: monster_id={config.monster_id}, path={config.prefab_path}");
        }
        
        return CreateSuccess();
    }
}
```

---

## 6. 游戏状态机实现规范

本章节对应《策划知识库 v2.2》中的"游戏流程与状态机"章节，定义了程序AI如何实现游戏的主流程控制。

### 6.1 状态枚举定义

```csharp
public enum GameState {
    Init,        // 初始化
    MainMenu,    // 主界面
    Loading,     // 加载中
    InGame,      // 游戏内
    Paused,      // 暂停
    Settlement   // 结算
}
```

### 6.2 GameManager状态机实现

GameManager必须实现一个状态机来管理游戏的主流程。推荐使用简单的switch-case或状态模式。

**核心方法**：

```csharp
public class GameManager : MonoBehaviour {
    public GameState CurrentState { get; private set; }
    
    public void ChangeState(GameState newState) {
        // 退出当前状态
        OnStateExit(CurrentState);
        
        // 切换状态
        CurrentState = newState;
        
        // 进入新状态
        OnStateEnter(CurrentState);
        
        // 派发状态变化事件
        EventManager.Dispatch("GAME_STATE_CHANGED", CurrentState);
    }
    
    private void OnStateEnter(GameState state) {
        switch (state) {
            case GameState.Init:
                // 初始化核心服务
                break;
            case GameState.MainMenu:
                // 加载主界面场景
                SceneManager.LoadScene("MainMenu");
                break;
            case GameState.Loading:
                // 显示加载界面
                break;
            case GameState.InGame:
                // 加载游戏场景
                SceneManager.LoadScene("Gameplay");
                break;
            case GameState.Paused:
                // 暂停游戏时间
                Time.timeScale = 0;
                break;
            case GameState.Settlement:
                // 显示结算界面
                break;
        }
    }
    
    private void OnStateExit(GameState state) {
        switch (state) {
            case GameState.Paused:
                // 恢复游戏时间
                Time.timeScale = 1;
                break;
        }
    }
}
```

### 6.3 状态转换触发

状态转换必须严格按照《策划知识库 v2.2》中定义的"游戏状态转换规则"进行。不允许出现未定义的状态跳转。

**示例：从主界面进入游戏**

```csharp
// 在MainMenu场景的StartButton点击事件中
public void OnStartButtonClicked() {
    GameManager.Instance.ChangeState(GameState.Loading);
}
```

---

## 7. UI系统实现规范

本章节对应《策划知识库 v2.2》中的"UI系统设计规范"和"UI反馈系统设计规范"章节。

### 7.1 UI界面层级管理

UIManager必须实现UI界面的层级管理，确保不同层级的UI能够正确显示和交互。

**层级定义**：

```csharp
public enum UILayer {
    Background = 0,  // 背景层
    Main = 100,      // 主界面层
    Popup = 200,     // 弹窗层
    Tip = 300        // 提示层
}
```

**UIManager核心方法**：

```csharp
public class UIManager : MonoBehaviour {
    private Dictionary<string, GameObject> uiPanels = new Dictionary<string, GameObject>();
    private Stack<GameObject> popupStack = new Stack<GameObject>();
    
    public void ShowPanel(string panelName, UILayer layer = UILayer.Main) {
        // 加载并显示UI面板
        GameObject panel = LoadPanel(panelName);
        panel.GetComponent<Canvas>().sortingOrder = (int)layer;
        
        if (layer == UILayer.Popup) {
            popupStack.Push(panel);
        }
        
        uiPanels[panelName] = panel;
    }
    
    public void HidePanel(string panelName) {
        if (uiPanels.ContainsKey(panelName)) {
            GameObject panel = uiPanels[panelName];
            
            if (popupStack.Count > 0 && popupStack.Peek() == panel) {
                popupStack.Pop();
            }
            
            Destroy(panel);
            uiPanels.Remove(panelName);
        }
    }
    
    public void OnBackButtonPressed() {
        // 处理返回键：关闭最上层的弹窗
        if (popupStack.Count > 0) {
            GameObject topPanel = popupStack.Pop();
            Destroy(topPanel);
        }
    }
}
```

### 7.2 UI状态刷新实现

UI脚本必须通过监听事件来刷新界面，而不是轮询或直接访问数据源。

**示例：血条刷新**

```csharp
public class HealthBar : MonoBehaviour {
    private Slider healthSlider;
    
    void Start() {
        healthSlider = GetComponent<Slider>();
        
        // 监听血量变化事件
        EventManager.Subscribe("HEALTH_CHANGED", OnHealthChanged);
    }
    
    void OnHealthChanged(object data) {
        var healthData = (HealthData)data;
        healthSlider.value = healthData.currentHP / healthData.maxHP;
    }
    
    void OnDestroy() {
        EventManager.Unsubscribe("HEALTH_CHANGED", OnHealthChanged);
    }
}
```

### 7.3 UI反馈实现

所有可交互控件必须实现明确的视觉和音效反馈。具体实现参数请参考《策划知识库 v2.2》章节9.4"UI反馈的具体实现规范"。

**示例：按钮反馈**

```csharp
public class FeedbackButton : MonoBehaviour, IPointerDownHandler, IPointerUpHandler {
    private Vector3 originalScale;
    
    void Start() {
        originalScale = transform.localScale;
    }
    
    public void OnPointerDown(PointerEventData eventData) {
        // 视觉反馈：缩小至95%（参考策划知识库v2.1章节9.4.1）
        transform.localScale = originalScale * 0.95f;
        
        // 音效反馈
        AudioManager.Instance.PlaySFX("ButtonClick");
    }
    
    public void OnPointerUp(PointerEventData eventData) {
        // 恢复原始大小
        transform.localScale = originalScale;
    }
}
```

---

## 8. 事件系统实现规范

本章节对应《策划知识库 v2.2》中的"系统间协作与数据流转"章节。

### 8.1 EventManager实现

```csharp
public class EventManager : MonoBehaviour {
    private static EventManager instance;
    public static EventManager Instance => instance;
    
    private Dictionary<string, Action<object>> events = new Dictionary<string, Action<object>>();
    
    void Awake() {
        if (instance == null) {
            instance = this;
            DontDestroyOnLoad(gameObject);
        } else {
            Destroy(gameObject);
        }
    }
    
    public static void Subscribe(string eventName, Action<object> listener) {
        if (!Instance.events.ContainsKey(eventName)) {
            Instance.events[eventName] = null;
        }
        Instance.events[eventName] += listener;
    }
    
    public static void Unsubscribe(string eventName, Action<object> listener) {
        if (Instance.events.ContainsKey(eventName)) {
            Instance.events[eventName] -= listener;
        }
    }
    
    public static void Dispatch(string eventName, object data = null) {
        if (Instance.events.ContainsKey(eventName) && Instance.events[eventName] != null) {
            Instance.events[eventName].Invoke(data);
        }
    }
}
```

### 8.2 完整事件定义

程序AI应根据《策划知识库 v2.2》章节10.2中定义的"完整事件定义表"，实现所有必需的事件派发和监听。事件定义表包含30+个系统间通信事件的详细定义，包括事件名称、派发者、监听者、事件参数、触发时机和用途说明。

**示例：怪物死亡事件**

```csharp
// 在MonsterStats中派发事件
public void TakeDamage(float damage) {
    currentHP -= damage;
    
    if (currentHP <= 0) {
        // 派发怪物死亡事件（参考策划知识库v2.1章节10.2完整事件定义表）
        EventManager.Dispatch("MONSTER_KILLED", new MonsterKilledData {
            monsterId = this.monsterId,
            position = transform.position
        });
        
        Destroy(gameObject);
    }
}

// 在LootManager中监听事件
void Start() {
    EventManager.Subscribe("MONSTER_KILLED", OnMonsterKilled);
}

void OnMonsterKilled(object data) {
    var killedData = (MonsterKilledData)data;
    // 根据怪物ID查询掉落表，生成掉落物
    GenerateLoot(killedData.monsterId, killedData.position);
}
```

---

## 9. 配置数据加载与解析

本章节对应《策划知识库 v2.2》中的"配置数据结构定义"章节。

### 9.1 配置数据类定义

程序AI必须根据《策划知识库 v2.2》中定义的JSON配置结构，生成对应的C#数据类。

**示例：关卡配置类**

```csharp
[System.Serializable]
public class LevelConfig {
    public int level_id;
    public string level_name;
    public string scene_name;
    public WaveConfig[] waves;
    public BossConfig boss;
}

[System.Serializable]
public class WaveConfig {
    public int wave_id;
    public MonsterSpawnInfo[] monsters;
}

[System.Serializable]
public class MonsterSpawnInfo {
    public int monster_id;
    public int count;
    public string[] spawn_points;
}

[System.Serializable]
public class BossConfig {
    public int monster_id;
    public string spawn_point;
}
```

### 9.2 ConfigManager实现

```csharp
public class ConfigManager : MonoBehaviour {
    private static ConfigManager instance;
    public static ConfigManager Instance => instance;
    
    private Dictionary<int, LevelConfig> levelConfigs = new Dictionary<int, LevelConfig>();
    private Dictionary<int, MonsterConfig> monsterConfigs = new Dictionary<int, MonsterConfig>();
    private Dictionary<int, EquipmentConfig> equipmentConfigs = new Dictionary<int, EquipmentConfig>();
    
    void Awake() {
        if (instance == null) {
            instance = this;
            DontDestroyOnLoad(gameObject);
            LoadAllConfigs();
        } else {
            Destroy(gameObject);
        }
    }
    
    private void LoadAllConfigs() {
        // 加载关卡配置
        TextAsset levelJson = Resources.Load<TextAsset>("Config/level");
        LevelConfig[] levels = JsonHelper.FromJson<LevelConfig>(levelJson.text);
        foreach (var level in levels) {
            levelConfigs[level.level_id] = level;
        }
        
        // 加载怪物配置
        TextAsset monsterJson = Resources.Load<TextAsset>("Config/monster");
        MonsterConfig[] monsters = JsonHelper.FromJson<MonsterConfig>(monsterJson.text);
        foreach (var monster in monsters) {
            monsterConfigs[monster.monster_id] = monster;
        }
        
        // 加载装备配置
        TextAsset equipmentJson = Resources.Load<TextAsset>("Config/equipment");
        EquipmentConfig[] equipments = JsonHelper.FromJson<EquipmentConfig>(equipmentJson.text);
        foreach (var equipment in equipments) {
            equipmentConfigs[equipment.equipment_id] = equipment;
        }
    }
    
    public LevelConfig GetLevelConfig(int levelId) {
        return levelConfigs.ContainsKey(levelId) ? levelConfigs[levelId] : null;
    }
    
    public MonsterConfig GetMonsterConfig(int monsterId) {
        return monsterConfigs.ContainsKey(monsterId) ? monsterConfigs[monsterId] : null;
    }
    
    public EquipmentConfig GetEquipmentConfig(int equipmentId) {
        return equipmentConfigs.ContainsKey(equipmentId) ? equipmentConfigs[equipmentId] : null;
    }
}

// JSON数组解析辅助类
public static class JsonHelper {
    public static T[] FromJson<T>(string json) {
        string newJson = "{\"array\":" + json + "}";
        Wrapper<T> wrapper = JsonUtility.FromJson<Wrapper<T>>(newJson);
        return wrapper.array;
    }
    
    [System.Serializable]
    private class Wrapper<T> {
        public T[] array;
    }
}
```

### 9.3 配置数据验证机制

> **原则**: 配置数据是游戏运行的基础，必须确保所有配置数据的正确性和完整性。程序AI必须在加载配置数据时进行验证，防止因配置错误导致运行时问题。

#### 9.3.1 配置数据验证规则

**验证时机**: 配置数据应在`ConfigManager.LoadAllConfigs()`方法中加载后立即进行验证。

**验证类型**:
1. **格式验证**: 检查JSON格式是否正确，能否成功解析
2. **完整性验证**: 检查必需字段是否存在
3. **数值范围验证**: 检查数值是否在合理范围内（参考策划知识库中的"数值护栏"）
4. **引用完整性验证**: 检查引用的其他配置是否存在（如关卡配置中引用的怪物ID、掉落表ID等）
5. **资源路径验证**: 检查引用的资源路径是否存在

#### 9.3.2 配置数据验证实现

**验证基类**:
```csharp
public abstract class ConfigValidator<T> {
    public abstract ValidationResult Validate(T config);
    
    protected ValidationResult CreateSuccess() {
        return new ValidationResult { IsValid = true };
    }
    
    protected ValidationResult CreateError(string message) {
        return new ValidationResult { 
            IsValid = false, 
            ErrorMessage = message 
        };
    }
}

public class ValidationResult {
    public bool IsValid;
    public string ErrorMessage;
}
```

**关卡配置验证器示例**:
```csharp
public class LevelConfigValidator : ConfigValidator<LevelConfig> {
    public override ValidationResult Validate(LevelConfig config) {
        // 1. 完整性验证
        if (config.level_id <= 0) {
            return CreateError($"关卡ID无效: {config.level_id}");
        }
        
        if (string.IsNullOrEmpty(config.level_name)) {
            return CreateError($"关卡名称不能为空: level_id={config.level_id}");
        }
        
        if (string.IsNullOrEmpty(config.scene_name)) {
            return CreateError($"场景名称不能为空: level_id={config.level_id}");
        }
        
        // 2. 数值范围验证
        if (config.waves == null || config.waves.Length == 0) {
            return CreateError($"关卡必须至少包含一波怪物: level_id={config.level_id}");
        }
        
        // 3. 引用完整性验证
        foreach (var wave in config.waves) {
            if (wave.monsters == null || wave.monsters.Length == 0) {
                return CreateError($"波次必须至少包含一个怪物: level_id={config.level_id}, wave_id={wave.wave_id}");
            }
            
            foreach (var monster in wave.monsters) {
                // 验证怪物配置是否存在
                MonsterConfig monsterConfig = ConfigManager.Instance.GetMonsterConfig(monster.monster_id);
                if (monsterConfig == null) {
                    return CreateError($"怪物配置不存在: level_id={config.level_id}, monster_id={monster.monster_id}");
                }
                
                // 验证数量范围
                if (monster.count <= 0) {
                    return CreateError($"怪物数量必须大于0: level_id={config.level_id}, monster_id={monster.monster_id}");
                }
            }
        }
        
        // 4. BOSS配置验证
        if (config.boss != null) {
            MonsterConfig bossConfig = ConfigManager.Instance.GetMonsterConfig(config.boss.monster_id);
            if (bossConfig == null) {
                return CreateError($"BOSS配置不存在: level_id={config.level_id}, boss_id={config.boss.monster_id}");
            }
        }
        
        return CreateSuccess();
    }
}
```

**怪物配置验证器示例**:
```csharp
public class MonsterConfigValidator : ConfigValidator<MonsterConfig> {
    public override ValidationResult Validate(MonsterConfig config) {
        // 1. 完整性验证
        if (config.monster_id <= 0) {
            return CreateError($"怪物ID无效: {config.monster_id}");
        }
        
        if (string.IsNullOrEmpty(config.name)) {
            return CreateError($"怪物名称不能为空: monster_id={config.monster_id}");
        }
        
        if (string.IsNullOrEmpty(config.prefab_path)) {
            return CreateError($"预制体路径不能为空: monster_id={config.monster_id}");
        }
        
        // 2. 数值范围验证（参考策划知识库的数值护栏）
        if (config.hp <= 0) {
            return CreateError($"怪物HP必须大于0: monster_id={config.monster_id}");
        }
        
        if (config.attack < 0) {
            return CreateError($"怪物攻击力不能为负数: monster_id={config.monster_id}");
        }
        
        if (config.defense < 0) {
            return CreateError($"怪物防御力不能为负数: monster_id={config.monster_id}");
        }
        
        if (config.move_speed <= 0) {
            return CreateError($"怪物移动速度必须大于0: monster_id={config.monster_id}");
        }
        
        // 3. 资源路径验证
        GameObject prefab = Resources.Load<GameObject>(config.prefab_path);
        if (prefab == null) {
            return CreateError($"预制体资源不存在: monster_id={config.monster_id}, path={config.prefab_path}");
        }
        
        // 4. 引用完整性验证
        if (config.drop_table_id > 0) {
            DropTableConfig dropTable = ConfigManager.Instance.GetDropTableConfig(config.drop_table_id);
            if (dropTable == null) {
                return CreateError($"掉落表配置不存在: monster_id={config.monster_id}, drop_table_id={config.drop_table_id}");
            }
        }
        
        return CreateSuccess();
    }
}
```

**ConfigManager增强版（包含验证）**:
```csharp
public class ConfigManager : MonoBehaviour {
    // ... 原有代码 ...
    
    private void LoadAllConfigs() {
        // 加载关卡配置
        TextAsset levelJson = Resources.Load<TextAsset>("Config/level");
        if (levelJson == null) {
            Debug.LogError("关卡配置文件不存在: Config/level");
            return;
        }
        
        LevelConfig[] levels = JsonHelper.FromJson<LevelConfig>(levelJson.text);
        if (levels == null) {
            Debug.LogError("关卡配置JSON解析失败");
            return;
        }
        
        LevelConfigValidator levelValidator = new LevelConfigValidator();
        foreach (var level in levels) {
            // 先加载到字典中（验证器可能需要查询其他配置）
            levelConfigs[level.level_id] = level;
        }
        
        // 验证所有关卡配置
        foreach (var level in levels) {
            ValidationResult result = levelValidator.Validate(level);
            if (!result.IsValid) {
                Debug.LogError($"关卡配置验证失败: {result.ErrorMessage}");
                // 可以选择移除无效配置或阻止游戏启动
                levelConfigs.Remove(level.level_id);
            }
        }
        
        // 加载怪物配置
        TextAsset monsterJson = Resources.Load<TextAsset>("Config/monster");
        if (monsterJson == null) {
            Debug.LogError("怪物配置文件不存在: Config/monster");
            return;
        }
        
        MonsterConfig[] monsters = JsonHelper.FromJson<MonsterConfig>(monsterJson.text);
        if (monsters == null) {
            Debug.LogError("怪物配置JSON解析失败");
            return;
        }
        
        MonsterConfigValidator monsterValidator = new MonsterConfigValidator();
        foreach (var monster in monsters) {
            monsterConfigs[monster.monster_id] = monster;
        }
        
        // 验证所有怪物配置
        foreach (var monster in monsters) {
            ValidationResult result = monsterValidator.Validate(monster);
            if (!result.IsValid) {
                Debug.LogError($"怪物配置验证失败: {result.ErrorMessage}");
                monsterConfigs.Remove(monster.monster_id);
            }
        }
        
        // ... 其他配置的加载和验证 ...
    }
}
```

#### 9.3.3 配置数据验证检查清单

程序AI在实现配置数据加载时，必须确保：

- [ ] 所有配置数据都有对应的验证器
- [ ] 验证器检查了所有必需字段的完整性
- [ ] 验证器检查了数值范围（参考策划知识库的数值护栏）
- [ ] 验证器检查了引用完整性（如ID引用、资源路径等）
- [ ] 验证失败时，有清晰的错误日志
- [ ] 验证失败时，有适当的处理策略（移除无效配置或阻止游戏启动）

### 9.4 配置数据使用示例

```csharp
// 在MonsterSpawner中使用配置数据
public class MonsterSpawner : MonoBehaviour {
    public void SpawnMonster(int monsterId, Vector3 position) {
        // 从ConfigManager获取怪物配置
        MonsterConfig config = ConfigManager.Instance.GetMonsterConfig(monsterId);
        
        if (config == null) {
            Debug.LogError($"Monster config not found: {monsterId}");
            return;
        }
        
        // 加载怪物预制体
        GameObject monsterPrefab = Resources.Load<GameObject>(config.prefab_path);
        GameObject monster = Instantiate(monsterPrefab, position, Quaternion.identity);
        
        // 初始化怪物属性
        MonsterStats stats = monster.GetComponent<MonsterStats>();
        stats.Initialize(config);
    }
}
```

---

## 10. 工程落地补充（Cursor实战经验）

> **用途**：当"知识库规范"与"工程真实落地实现"存在缺口时，用本节记录**最小闭环实现**与**关键护栏**，避免后续重复踩坑。

### 10.1 事件系统：监听时序护栏（避免 Instance 未就绪导致丢监听）
- **问题特征**：UI/Audio 等在 `Awake/Start` 很早注册监听，但 `EventManager.Instance` 还没初始化，导致监听丢失。
- **推荐策略**：
  - EventManager 提供 **pending listeners 缓存**：Instance 未创建时先缓存在静态容器，Instance 创建后回放。
  - 事件名称与参数仍以《游戏设计的策划知识库 v2.2》第10.2节"完整事件定义表"为准；缓存仅解决"时序"，不改变"事件契约"。
  - **重要**：程序实现时必须与策划知识库事件表对齐，否则测试用例（如TC-EVENT-001）将失败。

#### 10.1.1 EventManager的pendingListeners机制实现

```csharp
// Assets/Scripts/Core/EventManager.cs
using System;
using System.Collections.Generic;
using UnityEngine;

public class EventManager : MonoBehaviour
{
    public static EventManager Instance { get; private set; }
    
    private Dictionary<string, Action<object>> eventDictionary = new Dictionary<string, Action<object>>();
    private List<Action> pendingListeners = new List<Action>();
    
    private void Awake()
    {
        if (Instance != null && Instance != this)
        {
            Destroy(gameObject);
            return;
        }
        Instance = this;
        DontDestroyOnLoad(gameObject);
        
        // 回放所有pending listeners
        foreach (var action in pendingListeners)
        {
            action.Invoke();
        }
        pendingListeners.Clear();
    }
    
    public void AddListener(string eventName, Action<object> listener)
    {
        // 如果Instance还未创建，缓存到pendingListeners
        if (Instance == null)
        {
            pendingListeners.Add(() => AddListener(eventName, listener));
            return;
        }
        
        // 正常注册逻辑
        if (eventDictionary.ContainsKey(eventName))
        {
            eventDictionary[eventName] += listener;
        }
        else
        {
            Debug.LogWarning($"事件未注册: {eventName}");
        }
    }
}
```

### 10.2 存档（SaveSystem）：首版强约束
- **目标**：首版至少持久化 **金币/经验/等级/背包/穿戴/设置（bgm/sfx）**，避免"回主界面/重启丢进度"。
- **推荐策略**：
  - 使用 `PlayerPrefs + JsonUtility` 保存一个 `GameSaveData`（含 version 字段）。
  - 两条关键路径：
    - **EnsureLoaded → ApplyToRuntime**：进场景/初始化时把存档应用到运行态对象（Audio/PlayerStats/EquipmentManager）。
    - **CaptureFromRuntime → SaveNow**：切场景/结算/暂停/退出时抓取运行态并落盘。

#### 10.2.1 SaveSystem的实现细节

```csharp
// Assets/Scripts/Core/SaveSystem.cs
using UnityEngine;
using System;

[Serializable]
public class GameSaveData
{
    public int version = 1;
    public int gold;
    public int experience;
    public int level;
    public InventoryData inventory;
    public EquipmentData equipment;
    public AudioSettings audioSettings;
}

[Serializable]
public class InventoryData
{
    public int[] itemIds;
    public int[] itemCounts;
}

[Serializable]
public class EquipmentData
{
    public int weaponId;
    public int armorId;
    public int accessoryId;
}

[Serializable]
public class AudioSettings
{
    public float bgmVolume = 0.7f;
    public float sfxVolume = 1.0f;
}

public class SaveSystem
{
    private const string SAVE_KEY = "GameSaveData";
    
    public static void SavePlayerData(GameSaveData data)
    {
        // 简单数据使用PlayerPrefs
        PlayerPrefs.SetInt("Gold", data.gold);
        PlayerPrefs.SetInt("Experience", data.experience);
        PlayerPrefs.SetInt("Level", data.level);
        
        // 复杂数据使用JSON
        PlayerPrefs.SetString("Inventory", JsonUtility.ToJson(data.inventory));
        PlayerPrefs.SetString("Equipment", JsonUtility.ToJson(data.equipment));
        PlayerPrefs.SetString("AudioSettings", JsonUtility.ToJson(data.audioSettings));
        
        PlayerPrefs.Save();
        Debug.Log("游戏数据已保存");
    }
    
    public static GameSaveData LoadPlayerData()
    {
        GameSaveData data = new GameSaveData();
        
        // 加载简单数据（提供默认值）
        data.gold = PlayerPrefs.GetInt("Gold", 1000);
        data.experience = PlayerPrefs.GetInt("Experience", 0);
        data.level = PlayerPrefs.GetInt("Level", 1);
        
        // 加载复杂数据
        string inventoryJson = PlayerPrefs.GetString("Inventory", "{}");
        data.inventory = JsonUtility.FromJson<InventoryData>(inventoryJson) ?? new InventoryData();
        
        string equipmentJson = PlayerPrefs.GetString("Equipment", "{}");
        data.equipment = JsonUtility.FromJson<EquipmentData>(equipmentJson) ?? new EquipmentData();
        
        string audioJson = PlayerPrefs.GetString("AudioSettings", "{}");
        data.audioSettings = JsonUtility.FromJson<AudioSettings>(audioJson) ?? new AudioSettings();
        
        Debug.Log($"游戏数据已加载：金币={data.gold}, 等级={data.level}");
        return data;
    }
    
    public static bool HasSaveData()
    {
        return PlayerPrefs.HasKey("Gold");
    }
    
    public static void DeleteSaveData()
    {
        PlayerPrefs.DeleteAll();
        Debug.Log("游戏数据已删除");
    }
}
```

### 10.3 掉落拾取：从"直接入账"升级为"可见掉落 + 拾取"
- **目标**：击杀反馈要闭环：怪物死亡 → 掉落出现在场景 → 玩家靠近拾取 → 数值/背包变化 → UI 刷新。
- **推荐实现**：
  - `LootManager` 监听 `MONSTER_KILLED`，根据掉落表生成掉落物实体，并派发 `ITEM_DROPPED`。
  - 掉落物使用 `Collider.isTrigger = true`，玩家进入触发范围后（可加 `pickupDelay`）执行拾取逻辑，派发 `ITEM_PICKED_UP`。
  - **事件参数建议**（与策划知识库 v2.2 同步）：`item_type/item_id/count/position`。

#### 10.3.1 DropPickup的实现细节

```csharp
// Assets/Scripts/Gameplay/DropPickup.cs
using UnityEngine;
using System.Collections;

public enum DropType
{
    Gold,
    Equipment,
    Potion
}

public class DropPickup : MonoBehaviour
{
    [Header("掉落物配置")]
    public DropType dropType;
    public int dropId;
    public int dropAmount = 1;
    
    [Header("拾取设置")]
    public float pickupDelay = 0.5f; // 掉落后延迟多久可拾取
    public float magnetRange = 2f;   // 磁力吸引范围
    public float magnetSpeed = 5f;   // 磁力吸引速度
    
    private bool canPickup = false;
    private Transform player;
    private SphereCollider triggerCollider;
    
    private void Start()
    {
        // 添加触发器
        triggerCollider = gameObject.AddComponent<SphereCollider>();
        triggerCollider.isTrigger = true;
        triggerCollider.radius = magnetRange;
        
        // 延迟后才能拾取
        StartCoroutine(EnablePickupAfterDelay());
        
        // 查找玩家
        player = GameObject.FindGameObjectWithTag("Player")?.transform;
    }
    
    private IEnumerator EnablePickupAfterDelay()
    {
        yield return new WaitForSeconds(pickupDelay);
        canPickup = true;
    }
    
    private void Update()
    {
        // 磁力吸引效果
        if (canPickup && player != null)
        {
            float distance = Vector3.Distance(transform.position, player.position);
            if (distance < magnetRange)
            {
                transform.position = Vector3.MoveTowards(
                    transform.position, 
                    player.position, 
                    magnetSpeed * Time.deltaTime
                );
            }
        }
    }
    
    private void OnTriggerEnter(Collider other)
    {
        if (!canPickup) return;
        
        if (other.CompareTag("Player"))
        {
            // 执行拾取逻辑
            PickupItem();
        }
    }
    
    private void PickupItem()
    {
        // 根据类型执行不同的拾取逻辑
        switch (dropType)
        {
            case DropType.Gold:
                // 金币入账
                PlayerStats.Instance.AddGold(dropAmount);
                break;
            case DropType.Equipment:
                // 装备入包
                InventoryManager.Instance.AddItem(dropId, dropAmount);
                break;
            case DropType.Potion:
                // 药水入包
                InventoryManager.Instance.AddItem(dropId, dropAmount);
                break;
        }
        
        // 派发拾取事件
        EventManager.TriggerEvent("ITEM_PICKED_UP", new {
            dropType = dropType,
            dropId = dropId,
            dropAmount = dropAmount
        });
        
        // 播放拾取音效
        AudioManager.Instance?.PlaySFX("SFX_Pickup");
        
        // 销毁掉落物
        Destroy(gameObject);
    }
}
```

### 10.4 技能配置化：最小字段先跑通闭环
- **目标**：技能效果与冷却不硬编码，至少支持 `SK001` 多重箭的扇形 AOE。
- **推荐策略**：
  - 在 `Assets/Resources/Config/SkillConfigs.json` 中维护最小字段：
    - `skill_id, cooldown, damage_multiplier, arrow_count, aoe_shape, aoe_range, aoe_angle`
  - `ConfigManager` 加载后提供 `SkillConfigs` 字典；`PlayerController` 使用技能时按 `skill_id` 应用配置。

#### 10.4.1 SkillConfigs.json的实现细节

**文件路径**：`Assets/Resources/Config/SkillConfigs.json`

**数据结构**：

```json
{
  "skills": [
    {
      "skillId": "SK001",
      "skillName": "多重箭",
      "cooldown": 5.0,
      "range": 10.0,
      "angle": 60.0,
      "arrowCount": 3,
      "damageMultiplier": 1.5,
      "aoeShape": "Sector"
    },
    {
      "skillId": "SK002",
      "skillName": "穿透箭",
      "cooldown": 10.0,
      "range": 12.0,
      "angle": 0.0,
      "arrowCount": 0,
      "damageMultiplier": 1.2,
      "aoeShape": "Line"
    }
  ]
}
```

**ConfigManager加载代码**：

```csharp
// Assets/Scripts/Core/ConfigManager.cs
using System.Collections.Generic;
using UnityEngine;

[System.Serializable]
public class SkillConfig
{
    public string skillId;
    public string skillName;
    public float cooldown;
    public float range;
    public float angle;
    public int arrowCount;
    public float damageMultiplier;
    public string aoeShape;
    public int healAmount;
}

[System.Serializable]
public class SkillConfigList
{
    public List<SkillConfig> skills;
}

public class ConfigManager : MonoBehaviour
{
    public Dictionary<string, SkillConfig> SkillConfigs { get; private set; }
    
    private void LoadSkillConfigs()
    {
        SkillConfigs = new Dictionary<string, SkillConfig>();
        TextAsset jsonFile = Resources.Load<TextAsset>("Config/SkillConfigs");
        
        if (jsonFile != null)
        {
            SkillConfigList configList = JsonUtility.FromJson<SkillConfigList>(jsonFile.text);
            foreach (var config in configList.skills)
            {
                SkillConfigs[config.skillId] = config;
            }
            Debug.Log($"加载技能配置: {SkillConfigs.Count}个");
        }
        else
        {
            Debug.LogError("技能配置文件未找到: Config/SkillConfigs");
        }
    }
}
```

### 10.5 Prefab 路径护栏：配置中的 prefab_path 必须可被 Resources.Load 命中
- **约束**：若怪物/装备等配置中包含 `prefab_path/icon_path`：
  - 必须是 `Resources` 相对路径（不含扩展名），例如 `Prefabs/Monsters/Murloc`。
  - 配置加载验证应尝试 `Resources.Load`（或在 SanityCheck 中验证），失败要给出明确日志。

#### 10.5.1 Resources路径约束规范

**路径约束**：

1. **怪物Prefab路径**：`Prefabs/Monsters/{MonsterName}`
   - 示例：`Prefabs/Monsters/Murloc`、`Prefabs/Monsters/Kobold`、`Prefabs/Monsters/OrcWarrior`

2. **角色Prefab路径**：`Prefabs/Characters/{CharacterName}`
   - 示例：`Prefabs/Characters/Player_Hunter`

3. **配置文件路径**：`Config/{ConfigName}`
   - 示例：`Config/SkillConfigs`、`Config/MonsterConfigs`、`Config/LevelConfigs`

4. **UI资源路径**：`Art/UI/{UIName}`
   - 示例：`Art/UI/UI_MainMenu_Background`、`Art/UI/Icons/UI_Icon_Monster_Murloc`

5. **音频资源路径**：`Audio/{AudioType}/{AudioName}`
   - 示例：`Audio/BGM/BGM_MainMenu`、`Audio/SFX/SFX_ButtonClick`

**验证代码**：

```csharp
// Assets/Scripts/Core/GameManager.cs
private void ValidateResourcePaths()
{
    Debug.Log("=== 验证Resources路径 ===");
    
    // 验证怪物Prefab
    foreach (var config in ConfigManager.Instance.MonsterConfigs.Values)
    {
        GameObject prefab = Resources.Load<GameObject>(config.prefabPath);
        if (prefab == null)
        {
            Debug.LogError($"怪物Prefab加载失败: {config.prefabPath}");
        }
    }
    
    // 验证技能配置
    TextAsset skillConfig = Resources.Load<TextAsset>("Config/SkillConfigs");
    if (skillConfig == null)
    {
        Debug.LogError("技能配置文件加载失败: Config/SkillConfigs");
    }
    
    Debug.Log("=== Resources路径验证完成 ===");
}
```

### 10.6 场景边界：优先物理墙，Clamping 仅作为兜底
- **推荐**：在 Gameplay 生成 4 面 `BoxCollider` 墙体限制移动；Clamping 作为可开关兜底（默认关闭，避免与物理墙冲突）。

#### 10.6.1 BoundaryWalls的实现细节

```csharp
// Assets/Scripts/Gameplay/MonsterSpawner.cs
using UnityEngine;

public class MonsterSpawner : MonoBehaviour
{
    [Header("场景边界设置")]
    public bool createBoundaryWalls = true;
    public float mapSize = 80f;        // 地图尺寸（正方形）
    public float wallHeight = 5f;      // 墙体高度
    public float wallThickness = 1f;   // 墙体厚度
    
    private void Start()
    {
        if (createBoundaryWalls)
        {
            CreateBoundaryWalls();
        }
    }
    
    private void CreateBoundaryWalls()
    {
        Debug.Log("创建场景边界墙体");
        
        // 创建父对象
        GameObject wallsParent = new GameObject("BoundaryWalls");
        wallsParent.transform.SetParent(transform);
        
        // 创建4面墙
        CreateWall("North", wallsParent.transform, 
            new Vector3(0, wallHeight/2, mapSize/2), 
            new Vector3(mapSize, wallHeight, wallThickness));
            
        CreateWall("South", wallsParent.transform, 
            new Vector3(0, wallHeight/2, -mapSize/2), 
            new Vector3(mapSize, wallHeight, wallThickness));
            
        CreateWall("East", wallsParent.transform, 
            new Vector3(mapSize/2, wallHeight/2, 0), 
            new Vector3(wallThickness, wallHeight, mapSize));
            
        CreateWall("West", wallsParent.transform, 
            new Vector3(-mapSize/2, wallHeight/2, 0), 
            new Vector3(wallThickness, wallHeight, mapSize));
    }
    
    private void CreateWall(string name, Transform parent, Vector3 position, Vector3 size)
    {
        GameObject wall = GameObject.CreatePrimitive(PrimitiveType.Cube);
        wall.name = $"BoundaryWall_{name}";
        wall.transform.SetParent(parent);
        wall.transform.position = position;
        wall.transform.localScale = size;
        
        // 设置Layer为Ground，确保与玩家碰撞
        wall.layer = LayerMask.NameToLayer("Ground");
        
        // 移除MeshRenderer（不可见）
        Destroy(wall.GetComponent<MeshRenderer>());
        
        // 确保BoxCollider存在且不是触发器
        BoxCollider collider = wall.GetComponent<BoxCollider>();
        collider.isTrigger = false;
        
        Debug.Log($"创建边界墙: {name} at {position}");
    }
}
```

**PlayerController中的Clamping兜底**：

```csharp
// Assets/Scripts/Gameplay/PlayerController.cs
public class PlayerController : MonoBehaviour
{
    [Header("移动边界")]
    public bool enableClamping = false; // 默认关闭，避免与物理墙冲突
    public float boundarySize = 40f;
    
    private void LateUpdate()
    {
        if (enableClamping)
        {
            // Clamping兜底（仅在物理墙失效时使用）
            Vector3 pos = transform.position;
            pos.x = Mathf.Clamp(pos.x, -boundarySize, boundarySize);
            pos.z = Mathf.Clamp(pos.z, -boundarySize, boundarySize);
            transform.position = pos;
        }
    }
}
```

### 10.7 多 AudioListener：跨场景持久化对象要能"按场景禁用/启用"
- **问题特征**：Managers `DontDestroyOnLoad` 携带 `Camera/AudioListener`，进入 Gameplay 后与场景相机产生冲突。
- **推荐**：在 `SceneLoaded` 回调中对 Managers 上的 `Camera/AudioListener` 做按场景切换的启用/禁用。

#### 10.7.1 Camera/AudioListener的场景切换管理

```csharp
// Assets/Scripts/Core/GameManager.cs
using UnityEngine;
using UnityEngine.SceneManagement;

public class GameManager : MonoBehaviour
{
    private Camera managersCamera;
    private AudioListener managersAudioListener;
    
    private void Awake()
    {
        // ... Instance 创建逻辑
        
        // 获取Managers上的Camera和AudioListener
        managersCamera = GetComponentInChildren<Camera>();
        managersAudioListener = GetComponentInChildren<AudioListener>();
        
        // 注册场景加载回调
        SceneManager.sceneLoaded += OnSceneLoaded;
    }
    
    private void OnDestroy()
    {
        SceneManager.sceneLoaded -= OnSceneLoaded;
    }
    
    private void OnSceneLoaded(Scene scene, LoadSceneMode mode)
    {
        Debug.Log($"场景加载完成: {scene.name}");
        
        // 根据场景类型启用/禁用Managers上的Camera和AudioListener
        if (scene.name == "Gameplay")
        {
            // 进入Gameplay场景，禁用Managers上的Camera和AudioListener
            if (managersCamera != null) managersCamera.enabled = false;
            if (managersAudioListener != null) managersAudioListener.enabled = false;
            Debug.Log("禁用Managers上的Camera和AudioListener");
        }
        else if (scene.name == "MainMenu")
        {
            // 回到MainMenu场景，启用Managers上的Camera和AudioListener
            if (managersCamera != null) managersCamera.enabled = true;
            if (managersAudioListener != null) managersAudioListener.enabled = true;
            Debug.Log("启用Managers上的Camera和AudioListener");
        }
        
        // 验证场景中只有一个AudioListener
        AudioListener[] listeners = FindObjectsOfType<AudioListener>();
        if (listeners.Length > 1)
        {
            Debug.LogWarning($"场景中存在{listeners.Length}个AudioListener，可能导致音频问题");
        }
    }
}
```

---

## 11. 迭代与沉淀

本程序知识库本身也应被视为一个可迭代的"活文档"。如果在未来的Cursor实战中发现此框架存在缺陷或不足，应主动向用户提出修订建议，以持续优化工作流程。
