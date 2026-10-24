# ProjectDrama Flow fork

- 上游：`https://github.com/MothCocoon/FlowGraph`
- 基线：`v2.3-5.8` / `8211b25999068407cb7b40b8c97e18b52d4832ca`
- 分支：`codex/projectdrama-ue5.8`，用于 UE 5.8 Angelscript 工程。
- 主工程通过 `Plugins/Flow` submodule 的精确 commit 使用此 fork；日常同步不执行 `submodule update --remote`。

定制保持通用，不包含 ProjectDrama 头文件：

1. `UFlowAsset::ReplaceNodesForEditorImport`：在已验证的源节点安装完成前不访问尚未重建的 EdGraph；逐个 RegisterNode 的即时 Harvest 不适合源导入。
2. 图节点的 `OnBuildAssetSourceMenu` 委托：主工程追加整份资产的源 JSON 菜单，不反向依赖项目的导出实现。
3. `NotifyGraphChanged` 尊重 LockUpdates，解锁后统一 Harvest；供导入/粘贴重建完整图和 AddOn 树。
4. Gather 时刷新已加载的非 Blueprint 类，过滤被热重载替换的旧类；项目适配器在 AS 编译后触发 Gather，Flow 无需依赖 Angelscript。

JSON 格式、反射序列化、游戏节点、AS 桥和 ECS 服务均在主工程。升级时在独立分支同步明确的 upstream 标签，验证 Editor、源 JSON（含连接/嵌套 AddOn）、AS 和可玩 Demo，再先 push fork commit、后更新主工程 gitlink。
