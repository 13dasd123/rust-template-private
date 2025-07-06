# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 2 - Generated: 2025-07-06 20:39:06

## BuriedTreasure (1)

```csharp
using System;
using Rust;
using Oxide.Core;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using System.Runtime.CompilerServices;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("BuriedTreasure", "Colon Blow", "1.0.10")]
[Description("Куплено на Oxide Russia")]
 class BuriedTreasure : RustPlugin
{
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Economics;
     void Loaded();
    private static PluginConfig config;
    private class PluginConfig
    {
        public GlobalSettings globalSettings { get; set; }
        public class GlobalSettings
        {
            [JsonProperty(PropertyName = "Gold - Enable gold to be sold for Server Reward Points ? ")]
            public bool UseServerRewards { get; set; }
            [JsonProperty(PropertyName = "Gold - Enable gold to be sold for Economics Bucks ? ")]
            public bool UseEconomics { get; set; }
            [JsonProperty(PropertyName = "Gold - Player will get this many Server Reward Points when selling 1 gold : ")]
            public int ServerRewardsGoldExhcange { get; set; }
            [JsonProperty(PropertyName = "Gold - Player will get this many Economics Bucks when selling 1 gold : ")]
            public int EconomicsGoldExchange { get; set; }
            [JsonProperty(PropertyName = "AutoLoot - Automatically turn in gold coins for rewards when looting ? ")]
            public bool EnableAutoGoldRewardOnLoot { get; set; }
            [JsonProperty(PropertyName = "AutoLoot - Automatically mark treasure maps when they are looted ? ")]
            public bool EnableAutoReadMapOnLoot { get; set; }
            [JsonProperty(PropertyName = "Standard Loot - Enable chance for random treasure map in standard loot crates ? ")]
            public bool EnableMapsInStandardLoot { get; set; }
            [JsonProperty(PropertyName = "Standard Loot - Enable chance for gold to spawn in standard loot crates ? ")]
            public bool EnableGoldInStandardLoot { get; set; }
            [JsonProperty(PropertyName = "Standard Loot - Random Treasure Map chance (if enabled) : ")]
            public int StandardLootAddMapChance { get; set; }
            [JsonProperty(PropertyName = "Standard Loot - Gold spawn chance (if enabled) : ")]
            public int StandardLootAddGoldChance { get; set; }
            [JsonProperty(PropertyName = "Treasure - Spawn - Only spawn Treasure up to this far from players current postion : ")]
            public float LocalTreasureMaxDistance { get; set; }
            [JsonProperty(PropertyName = "Treasure - Spawn - Use whole map (instead of distance from player) to get random spawn point ? ")]
            public bool UseWholeMapSpawn { get; set; }
            [JsonProperty(PropertyName = "Treasure - Spawn - When whole map size is used, reduce spawn area by this much offset (closer to land) : ")]
            public float WholeMapOffset { get; set; }
            [JsonProperty(PropertyName = "Treasure - Despawn - Approx Seconds the Treasure Marker and Location will despawn if not found : ")]
            public float DespawnTime { get; set; }
            [JsonProperty(PropertyName = "Treasure - Despawn - Approx Seconds the Spawned Chest will despawn if not looted : ")]
            public float TreasureDespawnTime { get; set; }
            [JsonProperty(PropertyName = "Treasure - Location - When player gets within this distance, treasure will spawn nearby : ")]
            public float LootDetectionRadius { get; set; }
            [JsonProperty(PropertyName = "Treasure - Chance - to add a Random Map to Treasure Chest : ")]
            public int AddMapChance { get; set; }
            [JsonProperty(PropertyName = "Treasure - Chance - to add a Gold to Treasure Chest : ")]
            public int AddGoldChance { get; set; }
            [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Basic Map: ")]
            public int BasicMapChance { get; set; }
            [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a UnCommon Map: ")]
            public int UnCommonMapChance { get; set; }
            [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Rare Map: ")]
            public int RareMapChance { get; set; }
            [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Elite Map: ")]
            public int EliteMapChance { get; set; }
            [JsonProperty(PropertyName = "Map Marker - Prefab - Treasure Chest Map marker prefab (default explosion marker) : ")]
            public string MapMarkerPrefab { get; set; }
            [JsonProperty(PropertyName = "Treasure - Prefab - Basic Treasure Chest prefab : ")]
            public string BasicTreasurePrefab { get; set; }
            [JsonProperty(PropertyName = "Treasure - Prefab - UnCommon Treasure Chest prefab : ")]
            public string UnCommonTreasurePrefab { get; set; }
            [JsonProperty(PropertyName = "Treasure - Prefab - Rare Treasure Chest prefab : ")]
            public string RareTreasurePrefab { get; set; }
            [JsonProperty(PropertyName = "Treasure - Prefab - Elite Treasure Chest prefab : ")]
            public string EliteTreasurePrefab { get; set; }
            [JsonProperty(PropertyName = "Text - Basic Map name when inspecting map in inventory")]
            public string BasicMapTitle { get; set; }
            [JsonProperty(PropertyName = "Text - Uncommon Map name when inspecting map in inventory")]
            public string UncommonMapTitle { get; set; }
            [JsonProperty(PropertyName = "Text - Rare Map name when inspecting map in inventory")]
            public string RareMapTitle { get; set; }
            [JsonProperty(PropertyName = "Text - Elite Map name when inspecting map in inventory")]
            public string EliteMapTitle { get; set; }
            [JsonProperty(PropertyName = "Text - Notes to player when inspecting map in inventory")]
            public string MapInfomation { get; set; }
            [JsonProperty(PropertyName = "Loot Table - Only Use Loot Table Items ? ")]
            public bool UseOnlyLootTable { get; set; }
            [JsonProperty(PropertyName = "Loot Table - Basic Treasure Chest")]
            public Dictionary<int, int> BasicLootTable { get; set; }
            [JsonProperty(PropertyName = "Loot Table - UnCommon Treasure Chest")]
            public Dictionary<int, int> UnCommonLootTable { get; set; }
            [JsonProperty(PropertyName = "Loot Table - Rare Treasure Chest")]
            public Dictionary<int, int> RareLootTable { get; set; }
            [JsonProperty(PropertyName = "Loot Table - Elite Treasure Chest")]
            public Dictionary<int, int> EliteLootTable { get; set; }
        }

        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    [ConsoleCommand("buymap")]
     void cmdConsoleBuyMap(ConsoleSystem.Arg arg);
    [ConsoleCommand("buyuncommonmap")]
     void cmdConsoleBuyUnCommonMap(ConsoleSystem.Arg arg);
    [ConsoleCommand("buyraremap")]
     void cmdConsoleBuyRareMap(ConsoleSystem.Arg arg);
    [ConsoleCommand("givegold")]
     void cmdConsoleGiveGold(ConsoleSystem.Arg arg);
    [ConsoleCommand("buyelitemap")]
     void cmdConsoleBuyEliteMap(ConsoleSystem.Arg arg);
    [ConsoleCommand("buyrandommap")]
     void cmdConsoleBuyRandomMap(ConsoleSystem.Arg arg);
    [ChatCommand("markmap")]
     void cmdMarkMap(BasePlayer player, string command, string[] args);
    [ChatCommand("treasurehelp")]
     void cmdTreasureHelp(BasePlayer player, string command, string[] args);
    [ChatCommand("sellgold")]
     void cmdSellGold(BasePlayer player, string command, string[] args);
     object CanStackItem(Item item, Item targetItem);
     void OnPlayerInput(BasePlayer player, InputState input);
     void CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot);
     bool HoldingMap(BasePlayer player, Item item);
     void GiveTreasureMap(BasePlayer player);
     void GiveUnCommonTreasureMap(BasePlayer player);
     void GiveRareTreasureMap(BasePlayer player);
     void GiveEliteTreasureMap(BasePlayer player);
     void GiveRandomTreasureMap(BasePlayer player);
     void GiveContainerRandomTreasureMap(LootContainer container);
     void GiveGold(BasePlayer player);
     void GiveContainerGold(LootContainer container);
     void SellGold(BasePlayer player, Item item);
    static float GetGroundPosition(Vector3 pos);
     Vector3 GetSpawnLocation(BasePlayer player);
     Vector3 FindGlobalSpawnPoint();
     void BuryTheTreasure(BasePlayer player, int maprarity);
     void OnLootSpawn(LootContainer container);
     string GetGridLocation(Vector3 position);
     void Unload();
    static void DestroyAll();
     object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
     class TreasureMarker : BaseEntity
    {
         BaseEntity lootbox;
         BaseEntity treasurechest;
         MapMarker mapmarker;
         SphereCollider sphereCollider;
        public ulong playerid;
         BuriedTreasure instance;
        public int rarity;
         string prefabtreasure;
         Dictionary<int, int> loottable;
         bool isvisible;
         bool didspawnchest;
         float despawncounter;
         float detectionradius;
         void Awake();
        private void OnTriggerEnter(Collider col);
         void SpawnTreasureChest();
         void AddLootTableItems(BaseEntity treasurebox);
         void CheckSpawnVisibility(BaseEntity entitybox);
         void CheckForExtras(BaseEntity entitybox);
         void AddRandomGold(BaseEntity entitybox);
         void AddRandomMap(BaseEntity entitybox);
         void FixedUpdate();
         void OnDestroy();
    }

     class TreasureDespawner : BaseEntity
    {
         BaseEntity treasure;
         BuriedTreasure instance;
         float despawncounter;
         void Awake();
         void FixedUpdate();
         void OnDestroy();
    }

}

private class PluginConfig
{
    public GlobalSettings globalSettings { get; set; }
    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Gold - Enable gold to be sold for Server Reward Points ? ")]
        public bool UseServerRewards { get; set; }
        [JsonProperty(PropertyName = "Gold - Enable gold to be sold for Economics Bucks ? ")]
        public bool UseEconomics { get; set; }
        [JsonProperty(PropertyName = "Gold - Player will get this many Server Reward Points when selling 1 gold : ")]
        public int ServerRewardsGoldExhcange { get; set; }
        [JsonProperty(PropertyName = "Gold - Player will get this many Economics Bucks when selling 1 gold : ")]
        public int EconomicsGoldExchange { get; set; }
        [JsonProperty(PropertyName = "AutoLoot - Automatically turn in gold coins for rewards when looting ? ")]
        public bool EnableAutoGoldRewardOnLoot { get; set; }
        [JsonProperty(PropertyName = "AutoLoot - Automatically mark treasure maps when they are looted ? ")]
        public bool EnableAutoReadMapOnLoot { get; set; }
        [JsonProperty(PropertyName = "Standard Loot - Enable chance for random treasure map in standard loot crates ? ")]
        public bool EnableMapsInStandardLoot { get; set; }
        [JsonProperty(PropertyName = "Standard Loot - Enable chance for gold to spawn in standard loot crates ? ")]
        public bool EnableGoldInStandardLoot { get; set; }
        [JsonProperty(PropertyName = "Standard Loot - Random Treasure Map chance (if enabled) : ")]
        public int StandardLootAddMapChance { get; set; }
        [JsonProperty(PropertyName = "Standard Loot - Gold spawn chance (if enabled) : ")]
        public int StandardLootAddGoldChance { get; set; }
        [JsonProperty(PropertyName = "Treasure - Spawn - Only spawn Treasure up to this far from players current postion : ")]
        public float LocalTreasureMaxDistance { get; set; }
        [JsonProperty(PropertyName = "Treasure - Spawn - Use whole map (instead of distance from player) to get random spawn point ? ")]
        public bool UseWholeMapSpawn { get; set; }
        [JsonProperty(PropertyName = "Treasure - Spawn - When whole map size is used, reduce spawn area by this much offset (closer to land) : ")]
        public float WholeMapOffset { get; set; }
        [JsonProperty(PropertyName = "Treasure - Despawn - Approx Seconds the Treasure Marker and Location will despawn if not found : ")]
        public float DespawnTime { get; set; }
        [JsonProperty(PropertyName = "Treasure - Despawn - Approx Seconds the Spawned Chest will despawn if not looted : ")]
        public float TreasureDespawnTime { get; set; }
        [JsonProperty(PropertyName = "Treasure - Location - When player gets within this distance, treasure will spawn nearby : ")]
        public float LootDetectionRadius { get; set; }
        [JsonProperty(PropertyName = "Treasure - Chance - to add a Random Map to Treasure Chest : ")]
        public int AddMapChance { get; set; }
        [JsonProperty(PropertyName = "Treasure - Chance - to add a Gold to Treasure Chest : ")]
        public int AddGoldChance { get; set; }
        [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Basic Map: ")]
        public int BasicMapChance { get; set; }
        [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a UnCommon Map: ")]
        public int UnCommonMapChance { get; set; }
        [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Rare Map: ")]
        public int RareMapChance { get; set; }
        [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Elite Map: ")]
        public int EliteMapChance { get; set; }
        [JsonProperty(PropertyName = "Map Marker - Prefab - Treasure Chest Map marker prefab (default explosion marker) : ")]
        public string MapMarkerPrefab { get; set; }
        [JsonProperty(PropertyName = "Treasure - Prefab - Basic Treasure Chest prefab : ")]
        public string BasicTreasurePrefab { get; set; }
        [JsonProperty(PropertyName = "Treasure - Prefab - UnCommon Treasure Chest prefab : ")]
        public string UnCommonTreasurePrefab { get; set; }
        [JsonProperty(PropertyName = "Treasure - Prefab - Rare Treasure Chest prefab : ")]
        public string RareTreasurePrefab { get; set; }
        [JsonProperty(PropertyName = "Treasure - Prefab - Elite Treasure Chest prefab : ")]
        public string EliteTreasurePrefab { get; set; }
        [JsonProperty(PropertyName = "Text - Basic Map name when inspecting map in inventory")]
        public string BasicMapTitle { get; set; }
        [JsonProperty(PropertyName = "Text - Uncommon Map name when inspecting map in inventory")]
        public string UncommonMapTitle { get; set; }
        [JsonProperty(PropertyName = "Text - Rare Map name when inspecting map in inventory")]
        public string RareMapTitle { get; set; }
        [JsonProperty(PropertyName = "Text - Elite Map name when inspecting map in inventory")]
        public string EliteMapTitle { get; set; }
        [JsonProperty(PropertyName = "Text - Notes to player when inspecting map in inventory")]
        public string MapInfomation { get; set; }
        [JsonProperty(PropertyName = "Loot Table - Only Use Loot Table Items ? ")]
        public bool UseOnlyLootTable { get; set; }
        [JsonProperty(PropertyName = "Loot Table - Basic Treasure Chest")]
        public Dictionary<int, int> BasicLootTable { get; set; }
        [JsonProperty(PropertyName = "Loot Table - UnCommon Treasure Chest")]
        public Dictionary<int, int> UnCommonLootTable { get; set; }
        [JsonProperty(PropertyName = "Loot Table - Rare Treasure Chest")]
        public Dictionary<int, int> RareLootTable { get; set; }
        [JsonProperty(PropertyName = "Loot Table - Elite Treasure Chest")]
        public Dictionary<int, int> EliteLootTable { get; set; }
    }

    public static PluginConfig DefaultConfig();
}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Gold - Enable gold to be sold for Server Reward Points ? ")]
    public bool UseServerRewards { get; set; }
    [JsonProperty(PropertyName = "Gold - Enable gold to be sold for Economics Bucks ? ")]
    public bool UseEconomics { get; set; }
    [JsonProperty(PropertyName = "Gold - Player will get this many Server Reward Points when selling 1 gold : ")]
    public int ServerRewardsGoldExhcange { get; set; }
    [JsonProperty(PropertyName = "Gold - Player will get this many Economics Bucks when selling 1 gold : ")]
    public int EconomicsGoldExchange { get; set; }
    [JsonProperty(PropertyName = "AutoLoot - Automatically turn in gold coins for rewards when looting ? ")]
    public bool EnableAutoGoldRewardOnLoot { get; set; }
    [JsonProperty(PropertyName = "AutoLoot - Automatically mark treasure maps when they are looted ? ")]
    public bool EnableAutoReadMapOnLoot { get; set; }
    [JsonProperty(PropertyName = "Standard Loot - Enable chance for random treasure map in standard loot crates ? ")]
    public bool EnableMapsInStandardLoot { get; set; }
    [JsonProperty(PropertyName = "Standard Loot - Enable chance for gold to spawn in standard loot crates ? ")]
    public bool EnableGoldInStandardLoot { get; set; }
    [JsonProperty(PropertyName = "Standard Loot - Random Treasure Map chance (if enabled) : ")]
    public int StandardLootAddMapChance { get; set; }
    [JsonProperty(PropertyName = "Standard Loot - Gold spawn chance (if enabled) : ")]
    public int StandardLootAddGoldChance { get; set; }
    [JsonProperty(PropertyName = "Treasure - Spawn - Only spawn Treasure up to this far from players current postion : ")]
    public float LocalTreasureMaxDistance { get; set; }
    [JsonProperty(PropertyName = "Treasure - Spawn - Use whole map (instead of distance from player) to get random spawn point ? ")]
    public bool UseWholeMapSpawn { get; set; }
    [JsonProperty(PropertyName = "Treasure - Spawn - When whole map size is used, reduce spawn area by this much offset (closer to land) : ")]
    public float WholeMapOffset { get; set; }
    [JsonProperty(PropertyName = "Treasure - Despawn - Approx Seconds the Treasure Marker and Location will despawn if not found : ")]
    public float DespawnTime { get; set; }
    [JsonProperty(PropertyName = "Treasure - Despawn - Approx Seconds the Spawned Chest will despawn if not looted : ")]
    public float TreasureDespawnTime { get; set; }
    [JsonProperty(PropertyName = "Treasure - Location - When player gets within this distance, treasure will spawn nearby : ")]
    public float LootDetectionRadius { get; set; }
    [JsonProperty(PropertyName = "Treasure - Chance - to add a Random Map to Treasure Chest : ")]
    public int AddMapChance { get; set; }
    [JsonProperty(PropertyName = "Treasure - Chance - to add a Gold to Treasure Chest : ")]
    public int AddGoldChance { get; set; }
    [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Basic Map: ")]
    public int BasicMapChance { get; set; }
    [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a UnCommon Map: ")]
    public int UnCommonMapChance { get; set; }
    [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Rare Map: ")]
    public int RareMapChance { get; set; }
    [JsonProperty(PropertyName = "Treasure - Chance - When a random map is added to chest or spawned, chance it will be a Elite Map: ")]
    public int EliteMapChance { get; set; }
    [JsonProperty(PropertyName = "Map Marker - Prefab - Treasure Chest Map marker prefab (default explosion marker) : ")]
    public string MapMarkerPrefab { get; set; }
    [JsonProperty(PropertyName = "Treasure - Prefab - Basic Treasure Chest prefab : ")]
    public string BasicTreasurePrefab { get; set; }
    [JsonProperty(PropertyName = "Treasure - Prefab - UnCommon Treasure Chest prefab : ")]
    public string UnCommonTreasurePrefab { get; set; }
    [JsonProperty(PropertyName = "Treasure - Prefab - Rare Treasure Chest prefab : ")]
    public string RareTreasurePrefab { get; set; }
    [JsonProperty(PropertyName = "Treasure - Prefab - Elite Treasure Chest prefab : ")]
    public string EliteTreasurePrefab { get; set; }
    [JsonProperty(PropertyName = "Text - Basic Map name when inspecting map in inventory")]
    public string BasicMapTitle { get; set; }
    [JsonProperty(PropertyName = "Text - Uncommon Map name when inspecting map in inventory")]
    public string UncommonMapTitle { get; set; }
    [JsonProperty(PropertyName = "Text - Rare Map name when inspecting map in inventory")]
    public string RareMapTitle { get; set; }
    [JsonProperty(PropertyName = "Text - Elite Map name when inspecting map in inventory")]
    public string EliteMapTitle { get; set; }
    [JsonProperty(PropertyName = "Text - Notes to player when inspecting map in inventory")]
    public string MapInfomation { get; set; }
    [JsonProperty(PropertyName = "Loot Table - Only Use Loot Table Items ? ")]
    public bool UseOnlyLootTable { get; set; }
    [JsonProperty(PropertyName = "Loot Table - Basic Treasure Chest")]
    public Dictionary<int, int> BasicLootTable { get; set; }
    [JsonProperty(PropertyName = "Loot Table - UnCommon Treasure Chest")]
    public Dictionary<int, int> UnCommonLootTable { get; set; }
    [JsonProperty(PropertyName = "Loot Table - Rare Treasure Chest")]
    public Dictionary<int, int> RareLootTable { get; set; }
    [JsonProperty(PropertyName = "Loot Table - Elite Treasure Chest")]
    public Dictionary<int, int> EliteLootTable { get; set; }
}

 class TreasureMarker : BaseEntity
{
     BaseEntity lootbox;
     BaseEntity treasurechest;
     MapMarker mapmarker;
     SphereCollider sphereCollider;
    public ulong playerid;
     BuriedTreasure instance;
    public int rarity;
     string prefabtreasure;
     Dictionary<int, int> loottable;
     bool isvisible;
     bool didspawnchest;
     float despawncounter;
     float detectionradius;
     void Awake();
    private void OnTriggerEnter(Collider col);
     void SpawnTreasureChest();
     void AddLootTableItems(BaseEntity treasurebox);
     void CheckSpawnVisibility(BaseEntity entitybox);
     void CheckForExtras(BaseEntity entitybox);
     void AddRandomGold(BaseEntity entitybox);
     void AddRandomMap(BaseEntity entitybox);
     void FixedUpdate();
     void OnDestroy();
}

 class TreasureDespawner : BaseEntity
{
     BaseEntity treasure;
     BuriedTreasure instance;
     float despawncounter;
     void Awake();
     void FixedUpdate();
     void OnDestroy();
}


```

---

## BypassQueue

```csharp
using Network;

Oxide.Plugins
[Info("BypassQueue", "Nogrod", "1.0.1", ResourceId = 1855)]
 class BypassQueue : RustPlugin
{
    private const string Perm;
     void OnServerInitialized();
     object CanBypassQueue(Connection connection);
}


```

---

## CamSpeed

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core;
using Network;
using UnityEngine;
using Oxide.Core.Libraries;
using System.Linq;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("CamSpeed", "noname:Deversive", "0.0.1")]
public class CamSpeed : RustPlugin
{
     Dictionary<BasePlayer, Timer> timerslist;
     string cc;
     void Init();
    [HookMethod("OnPlayerInit")]
     void OnPlayerInit(BasePlayer player);
     void d();
    [HookMethod("OnPlayerDisconnected")]
     void OnPlayerDisconnected(BasePlayer player, string reason);
}


```

---

## CapsNoCaps

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("CapsNoCaps", "PsychoTea", "1")]
[Description("Turns all uppercase chat into lowercase")]
public sealed class CapsNoCaps : RustPlugin
{
     bool OnPlayerChat(ConsoleSystem.Arg arg, string player);
}


```

---

## CapsuleSystem

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("CapsuleSystem", "https://topplugin.ru/", "1.0.0")]
public class CapsuleSystem : RustPlugin
{
    public string Layer;
    public HashSet<uint> openCrates;
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     object OnCollectiblePickup(Item item, BasePlayer player);
     object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void GiveExperiencePlayer(BasePlayer player, double amount);
     object OnItemAction(Item item, string action, BasePlayer player);
     void Unload();
     void Loaded();
     void OnServerInitialized();
    [ChatCommand("adddropcapsule")]
     void adddrop(BasePlayer player, string command, string[] args);
    [ChatCommand("capsule")]
     void chatCmdCapsule(BasePlayer player, string command, string[] args);
    [ConsoleCommand("capsule_givebalance")]
     void consoleGivebalance(ConsoleSystem.Arg arg);
    [ConsoleCommand("getdailymoney")]
     void consoleGetDailyMoney(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Capsule")]
     void consoleCmdCapsule(ConsoleSystem.Arg arg);
     class StoredData
    {
        public Dictionary<ulong, PlayerInfo> players;
    }

     class PlayerInfo
    {
        [JsonProperty("Баланс")]
        public int balance;
        [JsonProperty("Опыта")]
        public double XP;
        [JsonProperty("d")]
        public int lastday;
    }

     void SaveData();
     void LoadData();
     StoredData storedData;
    private DynamicConfigFile CapsuleData;
    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    public Configuration _config;
    public class Configuration
    {
        [JsonProperty("Стартовый баланс")]
        public int startBalance;
        [JsonProperty("Получение XP")]
        public Dictionary<string, double> gainingExperience;
        [JsonProperty("Капсулы на продажу")]
        public List<CapsuleInfo> items;
        [JsonProperty("Рейт для капсул")]
        public Dictionary<string, float> permission;
    }

    public class CapsuleInfo
    {
        [JsonProperty("Название предмета")]
        public string name;
        [JsonProperty("Цена за 1шт")]
        public int price;
        [JsonProperty("Шортнейм")]
        public string shortname;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("СкинИД")]
        public ulong skinID;
        [JsonProperty("Дроп с капсулы")]
        public List<DropInfo> drop;
        [JsonProperty("Изображение")]
        public string png;
    }

    public class DropInfo
    {
        [JsonProperty("Это команда?")]
        public bool isCommand;
        [JsonProperty("Айди")]
        public int id;
        [JsonProperty("Название")]
        public string name;
        [JsonProperty("Редкость")]
        public string rare;
        [JsonProperty("Шортнейм")]
        public string shortname;
        [JsonProperty("Команда")]
        public string command;
        [JsonProperty("Количество")]
        public int amount;
        [JsonProperty("СкинИД")]
        public ulong skinID;
        [JsonProperty("Ссылка на изображение")]
        public string image;
        [JsonProperty("Минимальный шанс")]
        public int minChance;
        [JsonProperty("Максимальный шанс")]
        public int maxChance;
        [JsonProperty("Сообщение в чат")]
        public string chatText;
    }

    private PlayerInfo GetPlayerInfo(ulong userid);
    private static string HexToRGB(string hex);
    public CuiRawImageComponent GetAvatarImageComponent(ulong user_id, string color);
    public CuiRawImageComponent GetImageComponent(string url, string shortName, string color);
    public CuiRawImageComponent GetItemImageComponent(string shortName);
    public bool AddImage(string url, string shortName);
    public static string Translit(string str);
}

 class StoredData
{
    public Dictionary<ulong, PlayerInfo> players;
}

 class PlayerInfo
{
    [JsonProperty("Баланс")]
    public int balance;
    [JsonProperty("Опыта")]
    public double XP;
    [JsonProperty("d")]
    public int lastday;
}

public class Configuration
{
    [JsonProperty("Стартовый баланс")]
    public int startBalance;
    [JsonProperty("Получение XP")]
    public Dictionary<string, double> gainingExperience;
    [JsonProperty("Капсулы на продажу")]
    public List<CapsuleInfo> items;
    [JsonProperty("Рейт для капсул")]
    public Dictionary<string, float> permission;
}

public class CapsuleInfo
{
    [JsonProperty("Название предмета")]
    public string name;
    [JsonProperty("Цена за 1шт")]
    public int price;
    [JsonProperty("Шортнейм")]
    public string shortname;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("СкинИД")]
    public ulong skinID;
    [JsonProperty("Дроп с капсулы")]
    public List<DropInfo> drop;
    [JsonProperty("Изображение")]
    public string png;
}

public class DropInfo
{
    [JsonProperty("Это команда?")]
    public bool isCommand;
    [JsonProperty("Айди")]
    public int id;
    [JsonProperty("Название")]
    public string name;
    [JsonProperty("Редкость")]
    public string rare;
    [JsonProperty("Шортнейм")]
    public string shortname;
    [JsonProperty("Команда")]
    public string command;
    [JsonProperty("Количество")]
    public int amount;
    [JsonProperty("СкинИД")]
    public ulong skinID;
    [JsonProperty("Ссылка на изображение")]
    public string image;
    [JsonProperty("Минимальный шанс")]
    public int minChance;
    [JsonProperty("Максимальный шанс")]
    public int maxChance;
    [JsonProperty("Сообщение в чат")]
    public string chatText;
}


```

---

## CarCommander

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Rust;
using System.Linq;
using System.Globalization;

Oxide.Plugins
[Info("CarCommander", "k1lly0u", "0.2.61")]
[Description("A custom car controller with many options including persistence")]
 class CarCommander : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin Spawns;
     Plugin RandomSpawns;
     RestoreData storedData;
    private Dictionary<ulong, double> userCooldowns;
    private DynamicConfigFile data;
    private DynamicConfigFile cooldowns;
    private static CarCommander ins;
    private Dictionary<string, string> itemNames;
    private Dictionary<ulong, HotwireManager> isHotwiring;
    private Dictionary<CommandType, BUTTON> controlButtons;
    private List<CarController> temporaryCars;
    private List<CarController> saveableCars;
    private bool initialized;
    private bool wipeData;
    private int fuelType;
    private int repairType;
    private string fuelTypeName;
    private string repairTypeName;
    const string carPrefab;
    const string boxPrefab;
    const string explosionPrefab;
    const string uiHealth;
    const string uiFuel;
    private void Loaded();
    private void OnServerInitialized();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void OnItemDeployed(Deployer deployer, BaseEntity entity);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void OnEntityKill(BaseNetworkable networkable);
    private void OnNewSave(string filename);
    private void OnServerSave();
    private void Unload();
    private object CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityMounted(BaseMountable mountable, BasePlayer player);
    private void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    private object CanMountEntity(BasePlayer player, BaseMountable mountable);
    private object CanDismountEntity(BasePlayer player, BaseMountable mountable);
    private T ParseType(string type);
    private bool HasPermission(BasePlayer player, string perm);
    private void ConvertControlButtons();
    private void NullifyDamage(HitInfo info);
    private void OpenInventory(BasePlayer player, CarController controller, ItemContainer container);
    private void RestoreVehicleInventories();
    private void CheckForSpawns();
    private double GrabCurrentTime();
    private string FormatTime(double time);
    private BaseEntity SpawnAtLocation(Vector3 position, Quaternion rotation, bool enableSaving, bool isExternallyManaged, bool repairEnabled, bool disableFuel, bool disableSecurity, bool disableCollision);
    private void ToggleController(BaseCar baseCar, bool enabled);
    private void MountPlayerTo(BasePlayer player, BaseCar baseCar);
    private void EjectAllPlayers(BaseCar baseCar);
    private ItemContainer GetVehicleInventory(BaseCar baseCar);
    private ItemContainer GetVehicleFuelTank(BaseCar baseCar);
    public class CarController : MonoBehaviour
    {
        public BaseCar entity;
        public StorageContainer container;
        public ItemContainer fuelTank;
        private Rigidbody rb;
        private Dictionary<CommandType, BUTTON> cb;
        private ConfigData.SecurityOptions security;
        private ConfigData.CollisionOptions collision;
        private ConfigData.DecaySettings decay;
        public List<BasePlayer> occupants;
         WheelCollider[] allWheels;
         WheelFrictionCurve sidewaysFriction;
         WheelFrictionCurve sidewaysFrictionHB;
        public ulong ownerId;
        private float engineTorque;
        private float brakeTorque;
        private float reverseTorque;
        private float speed;
        private float maxSpeed;
        private bool useDefaultHandling;
        private float maxSteeringAngleSpeed;
        private float maxSteeringAngle;
        private bool applyCounterSteer;
        private float driftAngle;
        private bool isDrifting;
        public float antiRollFrontHorizontal;
        public float antiRollRearHorizontal;
        public float antiRollVertical;
        private float accelInput;
        private float brakeInput;
        private float steerInput;
        private bool repairEnabled;
        private bool fuelEnabled;
        private float consumptionRate;
        private int fuelId;
        private float pendingFuel;
        private float nextFuelCheckTime;
        private bool hasFuel;
        private bool isFlipped;
        private float upsideDownTime;
        private int ignitionCode;
        private bool hasBeenHotwired;
        private bool ignitionOn;
        private bool eBrake;
        private bool driftFriction;
        public bool externallyManaged;
        public bool isDieing;
        public bool lightsOn;
        private Vector3[] dismountLocals;
        public BasePlayer Commander { get; set; }
        public bool HasBeenHotwired { get; set; }
        public int IgnitionCode { get; set; }
        public bool IsFlipped { get; set; }
        private void Awake();
        private void SetWheelColliders();
        private void InitializeSettings();
        private void InitializeFuel();
        private void InitializeInventory();
        private void InitializeDecay();
        private void FixedUpdate();
        private void ApplyForceAtWheels();
        private void DoSteering();
        private void ApplyAcceleration();
        private void AdjustSteering();
        private void AntiRollBars();
        private void CheckForDrift();
        private void CheckUpsideDown();
        private void OnCarFlipped();
        public void ResetCar();
        private void OnCollisionEnter(Collision col);
        private void StartDecayTimer();
        private void DealDecayDamage();
        private void ToggleLights();
        private int GetFuelAmount();
        private bool HasFuel(bool forceCheck);
        private void UseFuel(float seconds);
        public bool HasCommander();
        public void OnDriverMounted(BasePlayer player);
        public void OnPassengerMounted(BasePlayer player);
        private void CheckIgnitionSystems();
        public void CreateVehicleKey(BasePlayer player);
        public bool HasVehicleKey(BasePlayer keyHolder);
        private bool IsCorrectKey(Item key);
        public void OnVehicleHotwired();
        private void OnDestroy();
        public void EjectAllPlayers();
        public void ManageDamage(HitInfo info);
        public void StopToDie();
        private void OnDeath();
        private void RadiusDamage(float amount, float radius, Vector3 position);
        public void SetExternallyManaged();
        public void SetFeatures(bool repairEnabled, bool disableFuel, bool disableSecurity, bool disableCollision);
        public void DisableFuelConsumption();
        public void DisableSecuritySettings();
        public void DisableCollisionSettings();
        public void OnEntityMounted(BasePlayer player, bool isDriver);
        public void DismountPlayer(BasePlayer player, BaseMountable mountable);
        private Vector3 GetDismountPosition(BaseMountable mountable);
        public void OnEntityDismounted(BasePlayer player, BaseMountable mountable);
    }

     class HotwireManager
    {
        public BasePlayer player;
        public CarController controller;
        public Timer hwTimer;
        public HotwireManager();
        public HotwireManager(BasePlayer player, CarController controller);
        private void BeginHotwire();
        public void CancelHotwire();
    }

    public static class UI
    {
        static public CuiElementContainer ElementContainer(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
        static public void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
        static public void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        public static string Color(string hexColor, float alpha);
    }

    public class UI4
    {
        public float xMin;
        public float yMin;
        public float xMax;
        public float yMax;
        public UI4(float xMin, float yMin, float xMax, float yMax);
        public string GetMin();
        public string GetMax();
    }

    private void CreateHealthUI(BasePlayer player, CarController controller);
    private void CreateFuelUI(BasePlayer player, CarController controller);
    private void DestroyUI(BasePlayer player, string panel);
    private void DestroyAllUI(BasePlayer player);
    [ChatCommand("copykey")]
     void cmdCopyKey(BasePlayer player, string command, string[] args);
    [ChatCommand("hotwire")]
     void cmdHotwire(BasePlayer player, string command, string[] args);
    [ChatCommand("spawncar")]
     void cmdSpawnCar(BasePlayer player, string command, string[] args);
    [ChatCommand("admincar")]
     void cmdAdminCar(BasePlayer player, string command, string[] args);
    [ChatCommand("clearcars")]
     void cmdClearCars(BasePlayer player, string command, string[] args);
    [ChatCommand("flipcar")]
     void cmdFlipCar(BasePlayer player, string command, string[] args);
    [ChatCommand("buildcar")]
    private void cmdBuildHeli(BasePlayer player, string command, string[] args);
    [ConsoleCommand("clearcars")]
     void ccmdClearCars(ConsoleSystem.Arg arg);
    [ConsoleCommand("spawncar")]
     void ccmdSpawnCar(ConsoleSystem.Arg arg);
    private bool AreFriends(ulong playerId, ulong friendId);
    private bool IsClanmate(ulong playerId, ulong friendId);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Movement Settings")]
        public MovementSettings Movement { get; set; }
        [JsonProperty(PropertyName = "Button Configuration")]
        public ButtonConfiguration Buttons { get; set; }
        [JsonProperty(PropertyName = "Passenger Options")]
        public PassengerOptions Passengers { get; set; }
        [JsonProperty(PropertyName = "Inventory Options")]
        public InventoryOptions Inventory { get; set; }
        [JsonProperty(PropertyName = "Spawnable Options")]
        public SpawnableOptions Spawnable { get; set; }
        [JsonProperty(PropertyName = "Fuel Options")]
        public FuelOptions Fuel { get; set; }
        [JsonProperty(PropertyName = "Repair Options")]
        public RepairSettings Repair { get; set; }
        [JsonProperty(PropertyName = "Death Options")]
        public DeathOptions Death { get; set; }
        [JsonProperty(PropertyName = "Active Item Options")]
        public ActiveItemOptions ActiveItems { get; set; }
        [JsonProperty(PropertyName = "Security Options")]
        public SecurityOptions Security { get; set; }
        [JsonProperty(PropertyName = "Collision Options")]
        public CollisionOptions Collision { get; set; }
        [JsonProperty(PropertyName = "Decay Options")]
        public DecaySettings Decay { get; set; }
        [JsonProperty(PropertyName = "UI Options")]
        public UIOptions UI { get; set; }
        [JsonProperty(PropertyName = "Build Options")]
        public BuildOptions Build { get; set; }
        public class BuildOptions
        {
            [JsonProperty(PropertyName = "Allow users to build a car")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Use cooldown timers")]
            public bool Cooldown { get; set; }
            [JsonProperty(PropertyName = "Build Costs")]
            public List<BuildOption> Costs { get; set; }
            public class BuildOption
            {
                [JsonProperty(PropertyName = "Item Shortname")]
                public string Shortname { get; set; }
                [JsonProperty(PropertyName = "Amount")]
                public int Amount { get; set; }
            }

        }

        public class ButtonConfiguration
        {
            [JsonProperty(PropertyName = "Open inventory")]
            public string Inventory { get; set; }
            public string Accelerate { get; set; }
            [JsonProperty(PropertyName = "Brake / Reverse")]
            public string Brake { get; set; }
            [JsonProperty(PropertyName = "Turn Left")]
            public string Left { get; set; }
            [JsonProperty(PropertyName = "Turn Right")]
            public string Right { get; set; }
            [JsonProperty(PropertyName = "Hand Brake")]
            public string HBrake { get; set; }
            [JsonProperty(PropertyName = "Open fuel tank")]
            public string FuelTank { get; set; }
            [JsonProperty(PropertyName = "Toggle lights")]
            public string Lights { get; set; }
        }

        public class DecaySettings
        {
            [JsonProperty(PropertyName = "Enable decay system")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Amount of decay per decay tick (percentage of maximum health)")]
            public float Amount { get; set; }
            [JsonProperty(PropertyName = "Time between decay ticks (seconds)")]
            public int Time { get; set; }
        }

        public class RepairSettings
        {
            [JsonProperty(PropertyName = "Repair system enabled")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Shortname of item required to repair")]
            public string Shortname { get; set; }
            [JsonProperty(PropertyName = "Amount of item required to repair")]
            public int Amount { get; set; }
            [JsonProperty(PropertyName = "Amount of damage repaired per hit")]
            public int Damage { get; set; }
            [JsonProperty(PropertyName = "Allow players to flip cars that are on their roof")]
            public bool CanFlip { get; set; }
        }

        public class MovementSettings
        {
            [JsonProperty(PropertyName = "Use custom handling options")]
            public bool CustomHandling { get; set; }
            [JsonProperty(PropertyName = "Engine - Acceleration torque")]
            public float Acceleration { get; set; }
            [JsonProperty(PropertyName = "Engine - Brake  torque")]
            public float Brakes { get; set; }
            [JsonProperty(PropertyName = "Engine - Reverse  torque")]
            public float Reverse { get; set; }
            [JsonProperty(PropertyName = "Engine - Maximum speed")]
            public float Speed { get; set; }
            [JsonProperty(PropertyName = "Steering - Max angle")]
            public float Steer { get; set; }
            [JsonProperty(PropertyName = "Steering - Max angle at speed")]
            public float SteerSpeed { get; set; }
            [JsonProperty(PropertyName = "Steering - Automatically counter steer")]
            public bool CounterSteer { get; set; }
            [JsonProperty(PropertyName = "Suspension - Force")]
            public float Spring { get; set; }
            [JsonProperty(PropertyName = "Suspension - Damper")]
            public float Damper { get; set; }
            [JsonProperty(PropertyName = "Suspension - Target position (min 0, max 1)")]
            public float Target { get; set; }
            [JsonProperty(PropertyName = "Suspension - Distance")]
            public float Distance { get; set; }
            [JsonProperty(PropertyName = "Anti Roll - Front horizontal force")]
            public float AntiRollFH { get; set; }
            [JsonProperty(PropertyName = "Anti Roll - Rear horizontal force")]
            public float AntiRollRH { get; set; }
            [JsonProperty(PropertyName = "Anti Roll - Vertical force")]
            public float AntiRollV { get; set; }
        }

        public class PassengerOptions
        {
            [JsonProperty(PropertyName = "Allow passengers")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Require passenger to be a friend (FriendsAPI)")]
            public bool UseFriends { get; set; }
            [JsonProperty(PropertyName = "Require passenger to be a clan mate (Clans)")]
            public bool UseClans { get; set; }
        }

        public class CollisionOptions
        {
            [JsonProperty(PropertyName = "Enable collision damage system")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Collision damage multiplier")]
            public float Multiplier { get; set; }
        }

        public class DeathOptions
        {
            [JsonProperty(PropertyName = "Enable explosion damage on death")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Damage radius")]
            public float Radius { get; set; }
            [JsonProperty(PropertyName = "Damage Amount")]
            public float Amount { get; set; }
        }

        public class SecurityOptions
        {
            [JsonProperty(PropertyName = "Enable ignition systems")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Set first player with a key as vehicle owner (doesn't require a key to drive)")]
            public bool Owners { get; set; }
            [JsonProperty(PropertyName = "Ignition options")]
            public IgnitionOptions Ignition { get; set; }
            [JsonProperty(PropertyName = "Hotwire options")]
            public HotwireOptions Hotwire { get; set; }
            [JsonProperty(PropertyName = "Trunk lock options")]
            public TrunkOptions Trunk { get; set; }
            public class TrunkOptions
            {
                [JsonProperty(PropertyName = "Allow locks to be placed on the trunk")]
                public bool CanLock { get; set; }
                [JsonProperty(PropertyName = "Only allow locks to be placed if the player has the ignition key")]
                public bool RequireKey { get; set; }
            }

            public class IgnitionOptions
            {
                [JsonProperty(PropertyName = "Allow players to copy a ignition key")]
                public bool CanCopy { get; set; }
                [JsonProperty(PropertyName = "Give the first player to enter the vehicle a key")]
                public bool KeyOnEnter { get; set; }
                [JsonProperty(PropertyName = "Chance of getting a key on entrance (1 in X)")]
                public int KeyChance { get; set; }
            }

            public class HotwireOptions
            {
                [JsonProperty(PropertyName = "Allow players to hotwire vehicles")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Deal shock damage on failed hotwire attempts")]
                public bool DealDamage { get; set; }
                [JsonProperty(PropertyName = "Amount of time it takes per hotwire attempt (seconds)")]
                public int Time { get; set; }
                [JsonProperty(PropertyName = "Chance of successfully hotwiring a vehicle (1 in X chance)")]
                public int Chance { get; set; }
            }

        }

        public class InventoryOptions
        {
            [JsonProperty(PropertyName = "Enable inventory system")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Drop inventory on death")]
            public bool DropInv { get; set; }
            [JsonProperty(PropertyName = "Inventory size (max 36)")]
            public int Size { get; set; }
        }

        public class SpawnableOptions
        {
            [JsonProperty(PropertyName = "Enable automatic vehicle spawning")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Use RandomSpawns for spawn locations")]
            public bool RandomSpawns { get; set; }
            [JsonProperty(PropertyName = "Spawnfile name")]
            public string Spawnfile { get; set; }
            [JsonProperty(PropertyName = "Maximum spawned vehicles at any time")]
            public int Max { get; set; }
            [JsonProperty(PropertyName = "Time between autospawns (seconds)")]
            public int Time { get; set; }
            [JsonProperty(PropertyName = "Cooldown time for player spawned vehicles via chat command (seconds)")]
            public int Cooldown { get; set; }
        }

        public class FuelOptions
        {
            [JsonProperty(PropertyName = "Requires fuel")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Fuel type (item shortname)")]
            public string FuelType { get; set; }
            [JsonProperty(PropertyName = "Fuel consumption rate (litres per second)")]
            public float Consumption { get; set; }
            [JsonProperty(PropertyName = "Spawn vehicles with fuel")]
            public bool GiveFuel { get; set; }
            [JsonProperty(PropertyName = "Amount of fuel to give spawned vehicles (minimum)")]
            public int FuelAmountMin { get; set; }
            [JsonProperty(PropertyName = "Amount of fuel to give spawned vehicles (maximum)")]
            public int FuelAmountMax { get; set; }
        }

        public class ActiveItemOptions
        {
            [JsonProperty(PropertyName = "Driver - Disable all held items")]
            public bool DisableDriver { get; set; }
            [JsonProperty(PropertyName = "Passenger - Disable all held items")]
            public bool DisablePassengers { get; set; }
        }

        public class UIOptions
        {
            [JsonProperty(PropertyName = "Health settings")]
            public UICounter Health { get; set; }
            [JsonProperty(PropertyName = "Fuel settings")]
            public UICounter Fuel { get; set; }
            public class UICounter
            {
                [JsonProperty(PropertyName = "Display to player")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Position - X minimum")]
                public float Xmin { get; set; }
                [JsonProperty(PropertyName = "Position - X maximum")]
                public float XMax { get; set; }
                [JsonProperty(PropertyName = "Position - Y minimum")]
                public float YMin { get; set; }
                [JsonProperty(PropertyName = "Position - Y maximum")]
                public float YMax { get; set; }
                [JsonProperty(PropertyName = "Background color (hex)")]
                public string Color1 { get; set; }
                [JsonProperty(PropertyName = "Background alpha")]
                public float Color1A { get; set; }
                [JsonProperty(PropertyName = "Status color (hex)")]
                public string Color2 { get; set; }
                [JsonProperty(PropertyName = "Status alpha")]
                public float Color2A { get; set; }
            }

        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private ConfigData GetBaseConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    public class RestoreData
    {
        public Hash<uint, InventoryData> restoreData;
        public Hash<uint, SecurityData> securityData;
        public void AddData(CarController controller);
        public void RemoveData(uint netId);
        public bool HasRestoreData(uint netId);
        public bool HasSecurityData(uint netId);
        public void RestoreVehicle(CarController controller);
        private void RestoreAllItems(CarController controller, InventoryData inventoryData);
        private bool RestoreItems(CarController controller, ItemData[] itemData, bool isInventory);
        private Item CreateItem(ItemData itemData);
        public class InventoryData
        {
            public ItemData[] vehicleContainer;
            public ItemData[] fuelContainer;
            public InventoryData();
            public InventoryData(CarController controller);
            private IEnumerable<ItemData> GetItems(ItemContainer container);
        }

        public class ItemData
        {
            public int itemid;
            public ulong skin;
            public int amount;
            public float condition;
            public int ammo;
            public string ammotype;
            public int position;
            public int blueprintTarget;
            public InstanceData instanceData;
            public ItemData[] contents;
            public class InstanceData
            {
                public int dataInt;
                public int blueprintTarget;
                public int blueprintAmount;
                public InstanceData();
                public InstanceData(Item item);
                public void Restore(Item item);
            }

        }

        public class SecurityData
        {
            public int ignitionCode;
            public float health;
            public bool hasBeenHotwired;
            public bool hasCodeLock;
            public bool hasKeyLock;
            public bool isLocked;
            public string lockCode;
            public string guestCode;
            public ulong ownerId;
            public List<ulong> whiteListPlayers;
            public List<ulong> guestPlayers;
            public SecurityData();
            public SecurityData(CarController controller);
            public void RestoreVehicleSecurity(CarController controller);
        }

    }

     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

public class CarController : MonoBehaviour
{
    public BaseCar entity;
    public StorageContainer container;
    public ItemContainer fuelTank;
    private Rigidbody rb;
    private Dictionary<CommandType, BUTTON> cb;
    private ConfigData.SecurityOptions security;
    private ConfigData.CollisionOptions collision;
    private ConfigData.DecaySettings decay;
    public List<BasePlayer> occupants;
     WheelCollider[] allWheels;
     WheelFrictionCurve sidewaysFriction;
     WheelFrictionCurve sidewaysFrictionHB;
    public ulong ownerId;
    private float engineTorque;
    private float brakeTorque;
    private float reverseTorque;
    private float speed;
    private float maxSpeed;
    private bool useDefaultHandling;
    private float maxSteeringAngleSpeed;
    private float maxSteeringAngle;
    private bool applyCounterSteer;
    private float driftAngle;
    private bool isDrifting;
    public float antiRollFrontHorizontal;
    public float antiRollRearHorizontal;
    public float antiRollVertical;
    private float accelInput;
    private float brakeInput;
    private float steerInput;
    private bool repairEnabled;
    private bool fuelEnabled;
    private float consumptionRate;
    private int fuelId;
    private float pendingFuel;
    private float nextFuelCheckTime;
    private bool hasFuel;
    private bool isFlipped;
    private float upsideDownTime;
    private int ignitionCode;
    private bool hasBeenHotwired;
    private bool ignitionOn;
    private bool eBrake;
    private bool driftFriction;
    public bool externallyManaged;
    public bool isDieing;
    public bool lightsOn;
    private Vector3[] dismountLocals;
    public BasePlayer Commander { get; set; }
    public bool HasBeenHotwired { get; set; }
    public int IgnitionCode { get; set; }
    public bool IsFlipped { get; set; }
    private void Awake();
    private void SetWheelColliders();
    private void InitializeSettings();
    private void InitializeFuel();
    private void InitializeInventory();
    private void InitializeDecay();
    private void FixedUpdate();
    private void ApplyForceAtWheels();
    private void DoSteering();
    private void ApplyAcceleration();
    private void AdjustSteering();
    private void AntiRollBars();
    private void CheckForDrift();
    private void CheckUpsideDown();
    private void OnCarFlipped();
    public void ResetCar();
    private void OnCollisionEnter(Collision col);
    private void StartDecayTimer();
    private void DealDecayDamage();
    private void ToggleLights();
    private int GetFuelAmount();
    private bool HasFuel(bool forceCheck);
    private void UseFuel(float seconds);
    public bool HasCommander();
    public void OnDriverMounted(BasePlayer player);
    public void OnPassengerMounted(BasePlayer player);
    private void CheckIgnitionSystems();
    public void CreateVehicleKey(BasePlayer player);
    public bool HasVehicleKey(BasePlayer keyHolder);
    private bool IsCorrectKey(Item key);
    public void OnVehicleHotwired();
    private void OnDestroy();
    public void EjectAllPlayers();
    public void ManageDamage(HitInfo info);
    public void StopToDie();
    private void OnDeath();
    private void RadiusDamage(float amount, float radius, Vector3 position);
    public void SetExternallyManaged();
    public void SetFeatures(bool repairEnabled, bool disableFuel, bool disableSecurity, bool disableCollision);
    public void DisableFuelConsumption();
    public void DisableSecuritySettings();
    public void DisableCollisionSettings();
    public void OnEntityMounted(BasePlayer player, bool isDriver);
    public void DismountPlayer(BasePlayer player, BaseMountable mountable);
    private Vector3 GetDismountPosition(BaseMountable mountable);
    public void OnEntityDismounted(BasePlayer player, BaseMountable mountable);
}

 class HotwireManager
{
    public BasePlayer player;
    public CarController controller;
    public Timer hwTimer;
    public HotwireManager();
    public HotwireManager(BasePlayer player, CarController controller);
    private void BeginHotwire();
    public void CancelHotwire();
}

public static class UI
{
    static public CuiElementContainer ElementContainer(string panelName, string color, UI4 dimensions, bool useCursor, string parent);
    static public void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
    static public void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    public static string Color(string hexColor, float alpha);
}

public class UI4
{
    public float xMin;
    public float yMin;
    public float xMax;
    public float yMax;
    public UI4(float xMin, float yMin, float xMax, float yMax);
    public string GetMin();
    public string GetMax();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Movement Settings")]
    public MovementSettings Movement { get; set; }
    [JsonProperty(PropertyName = "Button Configuration")]
    public ButtonConfiguration Buttons { get; set; }
    [JsonProperty(PropertyName = "Passenger Options")]
    public PassengerOptions Passengers { get; set; }
    [JsonProperty(PropertyName = "Inventory Options")]
    public InventoryOptions Inventory { get; set; }
    [JsonProperty(PropertyName = "Spawnable Options")]
    public SpawnableOptions Spawnable { get; set; }
    [JsonProperty(PropertyName = "Fuel Options")]
    public FuelOptions Fuel { get; set; }
    [JsonProperty(PropertyName = "Repair Options")]
    public RepairSettings Repair { get; set; }
    [JsonProperty(PropertyName = "Death Options")]
    public DeathOptions Death { get; set; }
    [JsonProperty(PropertyName = "Active Item Options")]
    public ActiveItemOptions ActiveItems { get; set; }
    [JsonProperty(PropertyName = "Security Options")]
    public SecurityOptions Security { get; set; }
    [JsonProperty(PropertyName = "Collision Options")]
    public CollisionOptions Collision { get; set; }
    [JsonProperty(PropertyName = "Decay Options")]
    public DecaySettings Decay { get; set; }
    [JsonProperty(PropertyName = "UI Options")]
    public UIOptions UI { get; set; }
    [JsonProperty(PropertyName = "Build Options")]
    public BuildOptions Build { get; set; }
    public class BuildOptions
    {
        [JsonProperty(PropertyName = "Allow users to build a car")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Use cooldown timers")]
        public bool Cooldown { get; set; }
        [JsonProperty(PropertyName = "Build Costs")]
        public List<BuildOption> Costs { get; set; }
        public class BuildOption
        {
            [JsonProperty(PropertyName = "Item Shortname")]
            public string Shortname { get; set; }
            [JsonProperty(PropertyName = "Amount")]
            public int Amount { get; set; }
        }

    }

    public class ButtonConfiguration
    {
        [JsonProperty(PropertyName = "Open inventory")]
        public string Inventory { get; set; }
        public string Accelerate { get; set; }
        [JsonProperty(PropertyName = "Brake / Reverse")]
        public string Brake { get; set; }
        [JsonProperty(PropertyName = "Turn Left")]
        public string Left { get; set; }
        [JsonProperty(PropertyName = "Turn Right")]
        public string Right { get; set; }
        [JsonProperty(PropertyName = "Hand Brake")]
        public string HBrake { get; set; }
        [JsonProperty(PropertyName = "Open fuel tank")]
        public string FuelTank { get; set; }
        [JsonProperty(PropertyName = "Toggle lights")]
        public string Lights { get; set; }
    }

    public class DecaySettings
    {
        [JsonProperty(PropertyName = "Enable decay system")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Amount of decay per decay tick (percentage of maximum health)")]
        public float Amount { get; set; }
        [JsonProperty(PropertyName = "Time between decay ticks (seconds)")]
        public int Time { get; set; }
    }

    public class RepairSettings
    {
        [JsonProperty(PropertyName = "Repair system enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Shortname of item required to repair")]
        public string Shortname { get; set; }
        [JsonProperty(PropertyName = "Amount of item required to repair")]
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Amount of damage repaired per hit")]
        public int Damage { get; set; }
        [JsonProperty(PropertyName = "Allow players to flip cars that are on their roof")]
        public bool CanFlip { get; set; }
    }

    public class MovementSettings
    {
        [JsonProperty(PropertyName = "Use custom handling options")]
        public bool CustomHandling { get; set; }
        [JsonProperty(PropertyName = "Engine - Acceleration torque")]
        public float Acceleration { get; set; }
        [JsonProperty(PropertyName = "Engine - Brake  torque")]
        public float Brakes { get; set; }
        [JsonProperty(PropertyName = "Engine - Reverse  torque")]
        public float Reverse { get; set; }
        [JsonProperty(PropertyName = "Engine - Maximum speed")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Steering - Max angle")]
        public float Steer { get; set; }
        [JsonProperty(PropertyName = "Steering - Max angle at speed")]
        public float SteerSpeed { get; set; }
        [JsonProperty(PropertyName = "Steering - Automatically counter steer")]
        public bool CounterSteer { get; set; }
        [JsonProperty(PropertyName = "Suspension - Force")]
        public float Spring { get; set; }
        [JsonProperty(PropertyName = "Suspension - Damper")]
        public float Damper { get; set; }
        [JsonProperty(PropertyName = "Suspension - Target position (min 0, max 1)")]
        public float Target { get; set; }
        [JsonProperty(PropertyName = "Suspension - Distance")]
        public float Distance { get; set; }
        [JsonProperty(PropertyName = "Anti Roll - Front horizontal force")]
        public float AntiRollFH { get; set; }
        [JsonProperty(PropertyName = "Anti Roll - Rear horizontal force")]
        public float AntiRollRH { get; set; }
        [JsonProperty(PropertyName = "Anti Roll - Vertical force")]
        public float AntiRollV { get; set; }
    }

    public class PassengerOptions
    {
        [JsonProperty(PropertyName = "Allow passengers")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Require passenger to be a friend (FriendsAPI)")]
        public bool UseFriends { get; set; }
        [JsonProperty(PropertyName = "Require passenger to be a clan mate (Clans)")]
        public bool UseClans { get; set; }
    }

    public class CollisionOptions
    {
        [JsonProperty(PropertyName = "Enable collision damage system")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Collision damage multiplier")]
        public float Multiplier { get; set; }
    }

    public class DeathOptions
    {
        [JsonProperty(PropertyName = "Enable explosion damage on death")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Damage radius")]
        public float Radius { get; set; }
        [JsonProperty(PropertyName = "Damage Amount")]
        public float Amount { get; set; }
    }

    public class SecurityOptions
    {
        [JsonProperty(PropertyName = "Enable ignition systems")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Set first player with a key as vehicle owner (doesn't require a key to drive)")]
        public bool Owners { get; set; }
        [JsonProperty(PropertyName = "Ignition options")]
        public IgnitionOptions Ignition { get; set; }
        [JsonProperty(PropertyName = "Hotwire options")]
        public HotwireOptions Hotwire { get; set; }
        [JsonProperty(PropertyName = "Trunk lock options")]
        public TrunkOptions Trunk { get; set; }
        public class TrunkOptions
        {
            [JsonProperty(PropertyName = "Allow locks to be placed on the trunk")]
            public bool CanLock { get; set; }
            [JsonProperty(PropertyName = "Only allow locks to be placed if the player has the ignition key")]
            public bool RequireKey { get; set; }
        }

        public class IgnitionOptions
        {
            [JsonProperty(PropertyName = "Allow players to copy a ignition key")]
            public bool CanCopy { get; set; }
            [JsonProperty(PropertyName = "Give the first player to enter the vehicle a key")]
            public bool KeyOnEnter { get; set; }
            [JsonProperty(PropertyName = "Chance of getting a key on entrance (1 in X)")]
            public int KeyChance { get; set; }
        }

        public class HotwireOptions
        {
            [JsonProperty(PropertyName = "Allow players to hotwire vehicles")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Deal shock damage on failed hotwire attempts")]
            public bool DealDamage { get; set; }
            [JsonProperty(PropertyName = "Amount of time it takes per hotwire attempt (seconds)")]
            public int Time { get; set; }
            [JsonProperty(PropertyName = "Chance of successfully hotwiring a vehicle (1 in X chance)")]
            public int Chance { get; set; }
        }

    }

    public class InventoryOptions
    {
        [JsonProperty(PropertyName = "Enable inventory system")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Drop inventory on death")]
        public bool DropInv { get; set; }
        [JsonProperty(PropertyName = "Inventory size (max 36)")]
        public int Size { get; set; }
    }

    public class SpawnableOptions
    {
        [JsonProperty(PropertyName = "Enable automatic vehicle spawning")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Use RandomSpawns for spawn locations")]
        public bool RandomSpawns { get; set; }
        [JsonProperty(PropertyName = "Spawnfile name")]
        public string Spawnfile { get; set; }
        [JsonProperty(PropertyName = "Maximum spawned vehicles at any time")]
        public int Max { get; set; }
        [JsonProperty(PropertyName = "Time between autospawns (seconds)")]
        public int Time { get; set; }
        [JsonProperty(PropertyName = "Cooldown time for player spawned vehicles via chat command (seconds)")]
        public int Cooldown { get; set; }
    }

    public class FuelOptions
    {
        [JsonProperty(PropertyName = "Requires fuel")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Fuel type (item shortname)")]
        public string FuelType { get; set; }
        [JsonProperty(PropertyName = "Fuel consumption rate (litres per second)")]
        public float Consumption { get; set; }
        [JsonProperty(PropertyName = "Spawn vehicles with fuel")]
        public bool GiveFuel { get; set; }
        [JsonProperty(PropertyName = "Amount of fuel to give spawned vehicles (minimum)")]
        public int FuelAmountMin { get; set; }
        [JsonProperty(PropertyName = "Amount of fuel to give spawned vehicles (maximum)")]
        public int FuelAmountMax { get; set; }
    }

    public class ActiveItemOptions
    {
        [JsonProperty(PropertyName = "Driver - Disable all held items")]
        public bool DisableDriver { get; set; }
        [JsonProperty(PropertyName = "Passenger - Disable all held items")]
        public bool DisablePassengers { get; set; }
    }

    public class UIOptions
    {
        [JsonProperty(PropertyName = "Health settings")]
        public UICounter Health { get; set; }
        [JsonProperty(PropertyName = "Fuel settings")]
        public UICounter Fuel { get; set; }
        public class UICounter
        {
            [JsonProperty(PropertyName = "Display to player")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Position - X minimum")]
            public float Xmin { get; set; }
            [JsonProperty(PropertyName = "Position - X maximum")]
            public float XMax { get; set; }
            [JsonProperty(PropertyName = "Position - Y minimum")]
            public float YMin { get; set; }
            [JsonProperty(PropertyName = "Position - Y maximum")]
            public float YMax { get; set; }
            [JsonProperty(PropertyName = "Background color (hex)")]
            public string Color1 { get; set; }
            [JsonProperty(PropertyName = "Background alpha")]
            public float Color1A { get; set; }
            [JsonProperty(PropertyName = "Status color (hex)")]
            public string Color2 { get; set; }
            [JsonProperty(PropertyName = "Status alpha")]
            public float Color2A { get; set; }
        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class BuildOptions
{
    [JsonProperty(PropertyName = "Allow users to build a car")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Use cooldown timers")]
    public bool Cooldown { get; set; }
    [JsonProperty(PropertyName = "Build Costs")]
    public List<BuildOption> Costs { get; set; }
    public class BuildOption
    {
        [JsonProperty(PropertyName = "Item Shortname")]
        public string Shortname { get; set; }
        [JsonProperty(PropertyName = "Amount")]
        public int Amount { get; set; }
    }

}

public class BuildOption
{
    [JsonProperty(PropertyName = "Item Shortname")]
    public string Shortname { get; set; }
    [JsonProperty(PropertyName = "Amount")]
    public int Amount { get; set; }
}

public class ButtonConfiguration
{
    [JsonProperty(PropertyName = "Open inventory")]
    public string Inventory { get; set; }
    public string Accelerate { get; set; }
    [JsonProperty(PropertyName = "Brake / Reverse")]
    public string Brake { get; set; }
    [JsonProperty(PropertyName = "Turn Left")]
    public string Left { get; set; }
    [JsonProperty(PropertyName = "Turn Right")]
    public string Right { get; set; }
    [JsonProperty(PropertyName = "Hand Brake")]
    public string HBrake { get; set; }
    [JsonProperty(PropertyName = "Open fuel tank")]
    public string FuelTank { get; set; }
    [JsonProperty(PropertyName = "Toggle lights")]
    public string Lights { get; set; }
}

public class DecaySettings
{
    [JsonProperty(PropertyName = "Enable decay system")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Amount of decay per decay tick (percentage of maximum health)")]
    public float Amount { get; set; }
    [JsonProperty(PropertyName = "Time between decay ticks (seconds)")]
    public int Time { get; set; }
}

public class RepairSettings
{
    [JsonProperty(PropertyName = "Repair system enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Shortname of item required to repair")]
    public string Shortname { get; set; }
    [JsonProperty(PropertyName = "Amount of item required to repair")]
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Amount of damage repaired per hit")]
    public int Damage { get; set; }
    [JsonProperty(PropertyName = "Allow players to flip cars that are on their roof")]
    public bool CanFlip { get; set; }
}

public class MovementSettings
{
    [JsonProperty(PropertyName = "Use custom handling options")]
    public bool CustomHandling { get; set; }
    [JsonProperty(PropertyName = "Engine - Acceleration torque")]
    public float Acceleration { get; set; }
    [JsonProperty(PropertyName = "Engine - Brake  torque")]
    public float Brakes { get; set; }
    [JsonProperty(PropertyName = "Engine - Reverse  torque")]
    public float Reverse { get; set; }
    [JsonProperty(PropertyName = "Engine - Maximum speed")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Steering - Max angle")]
    public float Steer { get; set; }
    [JsonProperty(PropertyName = "Steering - Max angle at speed")]
    public float SteerSpeed { get; set; }
    [JsonProperty(PropertyName = "Steering - Automatically counter steer")]
    public bool CounterSteer { get; set; }
    [JsonProperty(PropertyName = "Suspension - Force")]
    public float Spring { get; set; }
    [JsonProperty(PropertyName = "Suspension - Damper")]
    public float Damper { get; set; }
    [JsonProperty(PropertyName = "Suspension - Target position (min 0, max 1)")]
    public float Target { get; set; }
    [JsonProperty(PropertyName = "Suspension - Distance")]
    public float Distance { get; set; }
    [JsonProperty(PropertyName = "Anti Roll - Front horizontal force")]
    public float AntiRollFH { get; set; }
    [JsonProperty(PropertyName = "Anti Roll - Rear horizontal force")]
    public float AntiRollRH { get; set; }
    [JsonProperty(PropertyName = "Anti Roll - Vertical force")]
    public float AntiRollV { get; set; }
}

public class PassengerOptions
{
    [JsonProperty(PropertyName = "Allow passengers")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Require passenger to be a friend (FriendsAPI)")]
    public bool UseFriends { get; set; }
    [JsonProperty(PropertyName = "Require passenger to be a clan mate (Clans)")]
    public bool UseClans { get; set; }
}

public class CollisionOptions
{
    [JsonProperty(PropertyName = "Enable collision damage system")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Collision damage multiplier")]
    public float Multiplier { get; set; }
}

public class DeathOptions
{
    [JsonProperty(PropertyName = "Enable explosion damage on death")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Damage radius")]
    public float Radius { get; set; }
    [JsonProperty(PropertyName = "Damage Amount")]
    public float Amount { get; set; }
}

public class SecurityOptions
{
    [JsonProperty(PropertyName = "Enable ignition systems")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Set first player with a key as vehicle owner (doesn't require a key to drive)")]
    public bool Owners { get; set; }
    [JsonProperty(PropertyName = "Ignition options")]
    public IgnitionOptions Ignition { get; set; }
    [JsonProperty(PropertyName = "Hotwire options")]
    public HotwireOptions Hotwire { get; set; }
    [JsonProperty(PropertyName = "Trunk lock options")]
    public TrunkOptions Trunk { get; set; }
    public class TrunkOptions
    {
        [JsonProperty(PropertyName = "Allow locks to be placed on the trunk")]
        public bool CanLock { get; set; }
        [JsonProperty(PropertyName = "Only allow locks to be placed if the player has the ignition key")]
        public bool RequireKey { get; set; }
    }

    public class IgnitionOptions
    {
        [JsonProperty(PropertyName = "Allow players to copy a ignition key")]
        public bool CanCopy { get; set; }
        [JsonProperty(PropertyName = "Give the first player to enter the vehicle a key")]
        public bool KeyOnEnter { get; set; }
        [JsonProperty(PropertyName = "Chance of getting a key on entrance (1 in X)")]
        public int KeyChance { get; set; }
    }

    public class HotwireOptions
    {
        [JsonProperty(PropertyName = "Allow players to hotwire vehicles")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Deal shock damage on failed hotwire attempts")]
        public bool DealDamage { get; set; }
        [JsonProperty(PropertyName = "Amount of time it takes per hotwire attempt (seconds)")]
        public int Time { get; set; }
        [JsonProperty(PropertyName = "Chance of successfully hotwiring a vehicle (1 in X chance)")]
        public int Chance { get; set; }
    }

}

public class TrunkOptions
{
    [JsonProperty(PropertyName = "Allow locks to be placed on the trunk")]
    public bool CanLock { get; set; }
    [JsonProperty(PropertyName = "Only allow locks to be placed if the player has the ignition key")]
    public bool RequireKey { get; set; }
}

public class IgnitionOptions
{
    [JsonProperty(PropertyName = "Allow players to copy a ignition key")]
    public bool CanCopy { get; set; }
    [JsonProperty(PropertyName = "Give the first player to enter the vehicle a key")]
    public bool KeyOnEnter { get; set; }
    [JsonProperty(PropertyName = "Chance of getting a key on entrance (1 in X)")]
    public int KeyChance { get; set; }
}

public class HotwireOptions
{
    [JsonProperty(PropertyName = "Allow players to hotwire vehicles")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Deal shock damage on failed hotwire attempts")]
    public bool DealDamage { get; set; }
    [JsonProperty(PropertyName = "Amount of time it takes per hotwire attempt (seconds)")]
    public int Time { get; set; }
    [JsonProperty(PropertyName = "Chance of successfully hotwiring a vehicle (1 in X chance)")]
    public int Chance { get; set; }
}

public class InventoryOptions
{
    [JsonProperty(PropertyName = "Enable inventory system")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Drop inventory on death")]
    public bool DropInv { get; set; }
    [JsonProperty(PropertyName = "Inventory size (max 36)")]
    public int Size { get; set; }
}

public class SpawnableOptions
{
    [JsonProperty(PropertyName = "Enable automatic vehicle spawning")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Use RandomSpawns for spawn locations")]
    public bool RandomSpawns { get; set; }
    [JsonProperty(PropertyName = "Spawnfile name")]
    public string Spawnfile { get; set; }
    [JsonProperty(PropertyName = "Maximum spawned vehicles at any time")]
    public int Max { get; set; }
    [JsonProperty(PropertyName = "Time between autospawns (seconds)")]
    public int Time { get; set; }
    [JsonProperty(PropertyName = "Cooldown time for player spawned vehicles via chat command (seconds)")]
    public int Cooldown { get; set; }
}

public class FuelOptions
{
    [JsonProperty(PropertyName = "Requires fuel")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Fuel type (item shortname)")]
    public string FuelType { get; set; }
    [JsonProperty(PropertyName = "Fuel consumption rate (litres per second)")]
    public float Consumption { get; set; }
    [JsonProperty(PropertyName = "Spawn vehicles with fuel")]
    public bool GiveFuel { get; set; }
    [JsonProperty(PropertyName = "Amount of fuel to give spawned vehicles (minimum)")]
    public int FuelAmountMin { get; set; }
    [JsonProperty(PropertyName = "Amount of fuel to give spawned vehicles (maximum)")]
    public int FuelAmountMax { get; set; }
}

public class ActiveItemOptions
{
    [JsonProperty(PropertyName = "Driver - Disable all held items")]
    public bool DisableDriver { get; set; }
    [JsonProperty(PropertyName = "Passenger - Disable all held items")]
    public bool DisablePassengers { get; set; }
}

public class UIOptions
{
    [JsonProperty(PropertyName = "Health settings")]
    public UICounter Health { get; set; }
    [JsonProperty(PropertyName = "Fuel settings")]
    public UICounter Fuel { get; set; }
    public class UICounter
    {
        [JsonProperty(PropertyName = "Display to player")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Position - X minimum")]
        public float Xmin { get; set; }
        [JsonProperty(PropertyName = "Position - X maximum")]
        public float XMax { get; set; }
        [JsonProperty(PropertyName = "Position - Y minimum")]
        public float YMin { get; set; }
        [JsonProperty(PropertyName = "Position - Y maximum")]
        public float YMax { get; set; }
        [JsonProperty(PropertyName = "Background color (hex)")]
        public string Color1 { get; set; }
        [JsonProperty(PropertyName = "Background alpha")]
        public float Color1A { get; set; }
        [JsonProperty(PropertyName = "Status color (hex)")]
        public string Color2 { get; set; }
        [JsonProperty(PropertyName = "Status alpha")]
        public float Color2A { get; set; }
    }

}

public class UICounter
{
    [JsonProperty(PropertyName = "Display to player")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Position - X minimum")]
    public float Xmin { get; set; }
    [JsonProperty(PropertyName = "Position - X maximum")]
    public float XMax { get; set; }
    [JsonProperty(PropertyName = "Position - Y minimum")]
    public float YMin { get; set; }
    [JsonProperty(PropertyName = "Position - Y maximum")]
    public float YMax { get; set; }
    [JsonProperty(PropertyName = "Background color (hex)")]
    public string Color1 { get; set; }
    [JsonProperty(PropertyName = "Background alpha")]
    public float Color1A { get; set; }
    [JsonProperty(PropertyName = "Status color (hex)")]
    public string Color2 { get; set; }
    [JsonProperty(PropertyName = "Status alpha")]
    public float Color2A { get; set; }
}

public class RestoreData
{
    public Hash<uint, InventoryData> restoreData;
    public Hash<uint, SecurityData> securityData;
    public void AddData(CarController controller);
    public void RemoveData(uint netId);
    public bool HasRestoreData(uint netId);
    public bool HasSecurityData(uint netId);
    public void RestoreVehicle(CarController controller);
    private void RestoreAllItems(CarController controller, InventoryData inventoryData);
    private bool RestoreItems(CarController controller, ItemData[] itemData, bool isInventory);
    private Item CreateItem(ItemData itemData);
    public class InventoryData
    {
        public ItemData[] vehicleContainer;
        public ItemData[] fuelContainer;
        public InventoryData();
        public InventoryData(CarController controller);
        private IEnumerable<ItemData> GetItems(ItemContainer container);
    }

    public class ItemData
    {
        public int itemid;
        public ulong skin;
        public int amount;
        public float condition;
        public int ammo;
        public string ammotype;
        public int position;
        public int blueprintTarget;
        public InstanceData instanceData;
        public ItemData[] contents;
        public class InstanceData
        {
            public int dataInt;
            public int blueprintTarget;
            public int blueprintAmount;
            public InstanceData();
            public InstanceData(Item item);
            public void Restore(Item item);
        }

    }

    public class SecurityData
    {
        public int ignitionCode;
        public float health;
        public bool hasBeenHotwired;
        public bool hasCodeLock;
        public bool hasKeyLock;
        public bool isLocked;
        public string lockCode;
        public string guestCode;
        public ulong ownerId;
        public List<ulong> whiteListPlayers;
        public List<ulong> guestPlayers;
        public SecurityData();
        public SecurityData(CarController controller);
        public void RestoreVehicleSecurity(CarController controller);
    }

}

public class InventoryData
{
    public ItemData[] vehicleContainer;
    public ItemData[] fuelContainer;
    public InventoryData();
    public InventoryData(CarController controller);
    private IEnumerable<ItemData> GetItems(ItemContainer container);
}

public class ItemData
{
    public int itemid;
    public ulong skin;
    public int amount;
    public float condition;
    public int ammo;
    public string ammotype;
    public int position;
    public int blueprintTarget;
    public InstanceData instanceData;
    public ItemData[] contents;
    public class InstanceData
    {
        public int dataInt;
        public int blueprintTarget;
        public int blueprintAmount;
        public InstanceData();
        public InstanceData(Item item);
        public void Restore(Item item);
    }

}

public class InstanceData
{
    public int dataInt;
    public int blueprintTarget;
    public int blueprintAmount;
    public InstanceData();
    public InstanceData(Item item);
    public void Restore(Item item);
}

public class SecurityData
{
    public int ignitionCode;
    public float health;
    public bool hasBeenHotwired;
    public bool hasCodeLock;
    public bool hasKeyLock;
    public bool isLocked;
    public string lockCode;
    public string guestCode;
    public ulong ownerId;
    public List<ulong> whiteListPlayers;
    public List<ulong> guestPlayers;
    public SecurityData();
    public SecurityData(CarController controller);
    public void RestoreVehicleSecurity(CarController controller);
}


```

---

## Cases

```csharp
using System.Linq;
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Cases", "Drop Dead / Redesign and fix by Deversive", "1.0.2")]
public class Cases : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private static Cases _ins;
     bool addday;
     string Layer;
     string Case231;
    private string Case341;
    private string Case56161;
    private string Case612354;
    public Dictionary<ulong, int> time;
    public Dictionary<ulong, List<Inventory>> inventory;
    public List<int> day;
    public Dictionary<ulong, int> taked;
    public List<ulong> openedui;
     string MainIMG;
     string InventoryIMG;
    private string sera1;
    private string case1s;
    private string case2s;
    private string case3s;
    private string case4s;
    private string button;
    public class Inventory
    {
        public bool command;
        public string strcommand;
        public string shortname;
        public int amount;
    }

    public class random
    {
        [JsonProperty("Шанс выпадения")]
        public int chance;
        [JsonProperty("Минимальное количество")]
        public int min;
        [JsonProperty("Максимальное количество")]
        public int max;
    }

    public class chance
    {
        [JsonProperty("Шанс выпадения")]
        public int chances;
        [JsonProperty("Картинка")]
        public string image;
    }

    public class Case
    {
        [JsonProperty("Сколько времени должен отыграть игрок для открытия кейса (в секундах)")]
        public int time;
        [JsonProperty("Использовать выпадение предметов?")]
        public bool items;
        [JsonProperty("Использовать выдачу команды?")]
        public bool command;
        [JsonProperty("Команды для выполнения (%steamid% заменяется на айди игрока)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, chance> strcommands;
        [JsonProperty("Предметы которые могут выпасть при открытии", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, random> itemsdrop;
    }

    private PluginConfig cfg;
    public class PluginConfig
    {
        [JsonProperty("Настройки кейсов")]
        public MainSettings settings;
        public class MainSettings
        {
            [JsonProperty("День кейса, её настройки", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public Dictionary<string, Case> cases;
        }

    }

    private void Init();
    protected override void LoadDefaultConfig();
    private void SaveData();
    private void LoadData();
     void OnServerInitialized();
     void OnServerSave();
     void Unload();
     void OnPlayerConnected(BasePlayer player);
     void UpdateUI();
     void UpdateTime();
     int GetWipeDay();
     bool HasCooldown(BasePlayer player, int Day);
     int GetCooldown(BasePlayer player, int Day);
     string CanTake(BasePlayer player, int Day);
    [ChatCommand("case")]
    private void CaseCommand(BasePlayer player);
    [ConsoleCommand("CloseUI1248712389")]
    private void CloseUI(ConsoleSystem.Arg args);
    private bool ShouldItemDrop(int chance);
    private int GetRandomAmount(int min, int max);
    private bool HasItemInInventory(Dictionary<ulong, List<Inventory>> inventory, ulong playerId, string shortname);
    private void AddItemToInventory(Dictionary<ulong, List<Inventory>> inventory, ulong playerId, string shortname, int amount);
    [ConsoleCommand("inventoryuiopenz")]
     void inventoryui(ConsoleSystem.Arg args);
    [ConsoleCommand("TakeCase")]
    private void TakeCase(ConsoleSystem.Arg args);
    [ConsoleCommand("casepage")]
    private void ChangePage(ConsoleSystem.Arg args);
    [ConsoleCommand("TakeItem")]
    private void TakeItem(ConsoleSystem.Arg args);
    [ConsoleCommand("TakePerm")]
    private void TakePerm(ConsoleSystem.Arg args);
     void DrawMainUI(BasePlayer player);
     void DrawInventoryUI(BasePlayer player, int page);
    public string TimeToString(double time);
    private static string HexToRustFormat(string hex);
     string GetImage(string name);
    public static class IMGLibrary
    {
        public static bool AddImage(string url, string imageName, ulong imageId, Action callback);
        public static bool AddImageData(string imageName, byte[] array, ulong imageId, Action callback);
        public static string GetImageURL(string imageName, ulong imageId);
        public static string GetImage(string imageName, ulong imageId, bool returnUrl);
        public static List<ulong> GetImageList(string name);
        public static Dictionary<string, object> GetSkinInfo(string name, ulong id);
        public static bool HasImage(string imageName, ulong imageId);
        public static bool IsInStorage(uint crc);
        public static bool IsReady();
        public static void ImportImageList(string title, Dictionary<string, string> imageList, ulong imageId, bool replace, Action callback);
        public static void ImportItemList(string title, Dictionary<string, Dictionary<ulong, string>> itemList, bool replace, Action callback);
        public static void ImportImageData(string title, Dictionary<string, byte[]> imageList, ulong imageId, bool replace, Action callback);
        public static void LoadImageList(string title, List<KeyValuePair<string, ulong>> imageList, Action callback);
        public static void RemoveImage(string imageName, ulong imageId);
    }

}

public class Inventory
{
    public bool command;
    public string strcommand;
    public string shortname;
    public int amount;
}

public class random
{
    [JsonProperty("Шанс выпадения")]
    public int chance;
    [JsonProperty("Минимальное количество")]
    public int min;
    [JsonProperty("Максимальное количество")]
    public int max;
}

public class chance
{
    [JsonProperty("Шанс выпадения")]
    public int chances;
    [JsonProperty("Картинка")]
    public string image;
}

public class Case
{
    [JsonProperty("Сколько времени должен отыграть игрок для открытия кейса (в секундах)")]
    public int time;
    [JsonProperty("Использовать выпадение предметов?")]
    public bool items;
    [JsonProperty("Использовать выдачу команды?")]
    public bool command;
    [JsonProperty("Команды для выполнения (%steamid% заменяется на айди игрока)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, chance> strcommands;
    [JsonProperty("Предметы которые могут выпасть при открытии", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, random> itemsdrop;
}

public class PluginConfig
{
    [JsonProperty("Настройки кейсов")]
    public MainSettings settings;
    public class MainSettings
    {
        [JsonProperty("День кейса, её настройки", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, Case> cases;
    }

}

public class MainSettings
{
    [JsonProperty("День кейса, её настройки", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, Case> cases;
}

public static class IMGLibrary
{
    public static bool AddImage(string url, string imageName, ulong imageId, Action callback);
    public static bool AddImageData(string imageName, byte[] array, ulong imageId, Action callback);
    public static string GetImageURL(string imageName, ulong imageId);
    public static string GetImage(string imageName, ulong imageId, bool returnUrl);
    public static List<ulong> GetImageList(string name);
    public static Dictionary<string, object> GetSkinInfo(string name, ulong id);
    public static bool HasImage(string imageName, ulong imageId);
    public static bool IsInStorage(uint crc);
    public static bool IsReady();
    public static void ImportImageList(string title, Dictionary<string, string> imageList, ulong imageId, bool replace, Action callback);
    public static void ImportItemList(string title, Dictionary<string, Dictionary<ulong, string>> itemList, bool replace, Action callback);
    public static void ImportImageData(string title, Dictionary<string, byte[]> imageList, ulong imageId, bool replace, Action callback);
    public static void LoadImageList(string title, List<KeyValuePair<string, ulong>> imageList, Action callback);
    public static void RemoveImage(string imageName, ulong imageId);
}


```

---

## CashSystem

```csharp
using System.Collections.Generic;
using System;

Oxide.Plugins
[Info("CashSystem", "igor1150", 1.0)]
 class CashSystem : RustPlugin
{
    private string menuPreco;
    private string menuQuantia;
    private string menuNome;
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("buy")]
     void chatCompra(BasePlayer player, string command, string[] args);
    [ChatCommand("cash")]
     void chatCash(BasePlayer player, string command, string[] args);
    [ChatCommand("addcash")]
     void chataddcash(BasePlayer player, string command, string[] args);
    [ChatCommand("help")]
     void chatajuda(BasePlayer player, string command, string[] args);
    [ChatCommand("generatepricelist")]
     void chatgerarlistaprecos(BasePlayer player, string command, string[] args);
    [ChatCommand("getprice")]
     void chatobterpreco(BasePlayer player, string command, string[] args);
    [ChatCommand("setprice")]
     void chatdefinirpreco(BasePlayer player, string command, string[] args);
    [ChatCommand("delitem")]
     void chatremoveritem(BasePlayer player, string command, string[] args);
    [ChatCommand("generateamountlist")]
     void chatgerarlistaquantia(BasePlayer player, string command, string[] args);
    [ChatCommand("getamount")]
     void chatobterquantia(BasePlayer player, string command, string[] args);
    [ChatCommand("setamount")]
     void chatdefinirquantia(BasePlayer player, string command, string[] args);
    [ChatCommand("generatenamelist")]
     void chatgerarlistanome(BasePlayer player, string command, string[] args);
     bool VenderItem(BasePlayer player, string nome, int quantia);
     int ObterCash(BasePlayer player);
     bool RemoveCash(BasePlayer player);
     bool AddCash(BasePlayer player, string nick, int cash);
     bool Administrador(BasePlayer player);
     bool GerarListaPrecos();
     int ObterPreco(string nomeItem);
     void DefinirPreco(string nomeItem, int Preco);
     void RemoverItem(string nomeItem);
     bool GerarListaQuantia();
     int ObterQuantia(string nomeItem);
     void DefinirQuantia(string nomeItem, int quantia);
     bool GerarListaNome();
}


```

---

## Casino

```csharp
using Facepunch;
using Newtonsoft.Json;
using System;
using System.Linq;
using System.Globalization;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Casino", "k1lly0u", "0.1.3")]
[Description("Core card game and table management system")]
 class Casino : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    [PluginReference]
    private Plugin ServerRewards;
    private Plugin Economics;
    public static Casino Instance { get; set; }
    private Hash<GameType, Action<BaseEntity, StoredData.GameData>> registeredGames;
    private List<CardGame> cardGames;
    private KeyValuePair<Vector3, Vector3>[] chairPositions;
    private bool wipeData;
    private const string CHAIR_PREFAB;
    private const string TABLE_PREFAB;
    private void Loaded();
    private void OnServerInitialized();
    private void OnNewSave(string filename);
    private void OnEntityMounted(BaseMountable mountable, BasePlayer player);
    private object CanMountEntity(BasePlayer player, BaseMountable mountable);
    private object CanDismountEntity(BasePlayer player, BaseMountable mountable);
    private void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity baseCombatEntity, HitInfo info);
    private void OnEntityKill(BaseNetworkable baseNetworkable);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    public void RegisterGame(GameType gameType, Action<BaseEntity, StoredData.GameData> callback);
    public void UnregisterGame(GameType gameType);
    private BaseMountable CreateSeat(BaseEntity entity, int number, CardGame cardgame);
    private void LoadTables();
    private void CreateCardGame(BaseEntity baseEntity, StoredData.GameData gameData);
    private CardGame IsGameChair(BaseMountable mountable);
    public CardGame IsGamePlayer(BasePlayer player);
    private BaseEntity FindEntityFromRay(BasePlayer player);
    private static T ParseType(string type);
    public class CardGame : MonoBehaviour
    {
        internal StoredData.GameData gameData;
        public GameType gameType;
        internal GameState gameState;
        internal TimerElement timer;
        private TableInformer informer;
        public CardPlayer[] cardPlayers;
        private BaseMountable[] availableSeats;
        public DecorDeployable table;
        private int bettingItemID;
        private string bettingItemName;
        private BettingType bettingType;
        public void Awake();
        public virtual void OnDestroy();
        public virtual void OnPlayerEnter(BasePlayer player, int position);
        public virtual void OnPlayerExit(BasePlayer player);
        public virtual bool CanDismountStandard();
        public int GetChairPosition(BaseMountable baseMountable);
        public int CurrentPlayerCount { get; set; }
        public int CurrentPlayingCount { get; set; }
        public void InitializeGame(StoredData.GameData gameData);
        public void CreateSeat(int number);
        public virtual void OnGameInitialized();
        public void AdjustBet(CardPlayer cardPlayer, int amount);
        public void ResetBet(CardPlayer cardPlayer);
        private void SetBettingType();
        public int GetUserAmount(BasePlayer player);
        public void TakeAmount(BasePlayer player, int amount);
        public void GiveAmount(BasePlayer player, int amount);
        public string FormatBetString(BasePlayer player);
    }

    internal class GameChair : MonoBehaviour
    {
        private BaseMountable mountable;
        public CardGame CardGame { get; set; }
        public int Number { get; set; }
        private void Awake();
        internal void SetCardGame(CardGame cardGame, int number);
    }

    internal class TableInformer : MonoBehaviour
    {
        private DecorDeployable table;
        private CardGame cardGame;
        private OBB worldSpaceBounds;
        private Vector3 drawPosition;
        private string informationStr;
        private const float REFRESH_RATE;
        private void Awake();
        public void OnGameInitialized();
        private void InformationTick();
    }

    internal class TimerElement : MonoBehaviour
    {
        public Casino.CardGame CardGame { get; set; }
        private int timeRemaining;
        private Action callback;
        private const string TIMER_OVERLAY;
        private UI4 TIMER_POSITION;
        private void Awake();
        public void StartTimer(int time, Action callback);
        public void StopTimer();
        private void TimerTick();
        private void UpdateTimerUI();
    }

    public class CardPlayer : MonoBehaviour
    {
        internal List<Card> hand;
        internal List<string> uiPanels;
        public BasePlayer Player { get; set; }
        public Casino.CardGame CardGame { get; set; }
        internal bool IsLeaving { get; set; }
        internal int UID { get; set; }
        internal int BetAmount { get; set; }
        internal bool BetLocked { get; set; }
        internal bool IsPlaying { get; set; }
        internal int Position { get; set; }
        public int BankBalance { get; set; }
        public int BalanceAndBet { get; set; }
        internal virtual void Awake();
        internal virtual void OnDestroy();
        internal void GenerateUID();
        internal virtual void ResetHand();
        internal void SetBet();
        internal void IssueWin(int amount);
        internal void AddUI(string str, CuiElementContainer container);
        internal void DestroyUI(string str);
        internal void DestroyUI();
    }

    public class Deck
    {
        private Queue<Card> deckOfCards;
        private List<int> cardsAsInt;
        private System.Random generator;
        public Deck();
        private void GenerateCardsAsInt();
        public void Shuffle();
        private void FillDeck();
        public Card DealCard();
    }

    public static class UI
    {
        public static CuiElementContainer ElementContainer(string panel, string color, UI4 dimensions, bool useCursor, string parent);
        public static CuiElementContainer ElementContainer(string panel, UI4 dimensions, bool useCursor, string parent);
        public static CuiElementContainer Popup(string panelName, string text, int size, UI4 dimensions, TextAnchor align, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
        public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        public static void OutlineLabel(CuiElementContainer container, string panel, string color, string text, int size, string distance, string aMin, string aMax, TextAnchor align, string parent);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, UI4 dimensions, string command);
        public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions);
        public static string Color(string hexColor, float alpha);
    }

    public class UI4
    {
        public float xMin;
        public float yMin;
        public float xMax;
        public float yMax;
        public UI4(float xMin, float yMin, float xMax, float yMax);
        public string GetMin();
        public string GetMax();
        private static UI4 _fullScreen;
        public static UI4 FullScreen { get; set; }
    }

    [ChatCommand("casino")]
    private void cmdCasino(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Betting Item (shortname)")]
        public string BettingItem { get; set; }
        [JsonProperty(PropertyName = "Betting Item Type (Item/ServerRewards/Economics)")]
        public string BettingType { get; set; }
        [JsonProperty(PropertyName = "Wipe data on map wipe")]
        public bool WipeOnNewSave { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    public class StoredData
    {
        public Hash<uint, GameData> tables;
        public bool IsRegisteredTable(uint netId);
        public void RegisterTable(uint netId, GameData gameData);
        public void RemoveTable(uint netId);
        public void SetBetType(uint netId, string bettingType, string shortname);
        public class GameData
        {
            public GameType gameType;
            public int maxPlayers;
            public int minimumBet;
            public int maximumBet;
            public string bettingItemOverride;
            public string bettingTypeOverride;
            public GameData();
            public GameData(GameType gameType, int maxPlayers, int minimumBet, int maximumBet);
        }

    }

}

public class CardGame : MonoBehaviour
{
    internal StoredData.GameData gameData;
    public GameType gameType;
    internal GameState gameState;
    internal TimerElement timer;
    private TableInformer informer;
    public CardPlayer[] cardPlayers;
    private BaseMountable[] availableSeats;
    public DecorDeployable table;
    private int bettingItemID;
    private string bettingItemName;
    private BettingType bettingType;
    public void Awake();
    public virtual void OnDestroy();
    public virtual void OnPlayerEnter(BasePlayer player, int position);
    public virtual void OnPlayerExit(BasePlayer player);
    public virtual bool CanDismountStandard();
    public int GetChairPosition(BaseMountable baseMountable);
    public int CurrentPlayerCount { get; set; }
    public int CurrentPlayingCount { get; set; }
    public void InitializeGame(StoredData.GameData gameData);
    public void CreateSeat(int number);
    public virtual void OnGameInitialized();
    public void AdjustBet(CardPlayer cardPlayer, int amount);
    public void ResetBet(CardPlayer cardPlayer);
    private void SetBettingType();
    public int GetUserAmount(BasePlayer player);
    public void TakeAmount(BasePlayer player, int amount);
    public void GiveAmount(BasePlayer player, int amount);
    public string FormatBetString(BasePlayer player);
}

internal class GameChair : MonoBehaviour
{
    private BaseMountable mountable;
    public CardGame CardGame { get; set; }
    public int Number { get; set; }
    private void Awake();
    internal void SetCardGame(CardGame cardGame, int number);
}

internal class TableInformer : MonoBehaviour
{
    private DecorDeployable table;
    private CardGame cardGame;
    private OBB worldSpaceBounds;
    private Vector3 drawPosition;
    private string informationStr;
    private const float REFRESH_RATE;
    private void Awake();
    public void OnGameInitialized();
    private void InformationTick();
}

internal class TimerElement : MonoBehaviour
{
    public Casino.CardGame CardGame { get; set; }
    private int timeRemaining;
    private Action callback;
    private const string TIMER_OVERLAY;
    private UI4 TIMER_POSITION;
    private void Awake();
    public void StartTimer(int time, Action callback);
    public void StopTimer();
    private void TimerTick();
    private void UpdateTimerUI();
}

public class CardPlayer : MonoBehaviour
{
    internal List<Card> hand;
    internal List<string> uiPanels;
    public BasePlayer Player { get; set; }
    public Casino.CardGame CardGame { get; set; }
    internal bool IsLeaving { get; set; }
    internal int UID { get; set; }
    internal int BetAmount { get; set; }
    internal bool BetLocked { get; set; }
    internal bool IsPlaying { get; set; }
    internal int Position { get; set; }
    public int BankBalance { get; set; }
    public int BalanceAndBet { get; set; }
    internal virtual void Awake();
    internal virtual void OnDestroy();
    internal void GenerateUID();
    internal virtual void ResetHand();
    internal void SetBet();
    internal void IssueWin(int amount);
    internal void AddUI(string str, CuiElementContainer container);
    internal void DestroyUI(string str);
    internal void DestroyUI();
}

public class Deck
{
    private Queue<Card> deckOfCards;
    private List<int> cardsAsInt;
    private System.Random generator;
    public Deck();
    private void GenerateCardsAsInt();
    public void Shuffle();
    private void FillDeck();
    public Card DealCard();
}

public static class UI
{
    public static CuiElementContainer ElementContainer(string panel, string color, UI4 dimensions, bool useCursor, string parent);
    public static CuiElementContainer ElementContainer(string panel, UI4 dimensions, bool useCursor, string parent);
    public static CuiElementContainer Popup(string panelName, string text, int size, UI4 dimensions, TextAnchor align, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions, bool cursor);
    public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    public static void OutlineLabel(CuiElementContainer container, string panel, string color, string text, int size, string distance, string aMin, string aMax, TextAnchor align, string parent);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, UI4 dimensions, string command);
    public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static void Input(CuiElementContainer container, string panel, string text, int size, string command, UI4 dimensions);
    public static string Color(string hexColor, float alpha);
}

public class UI4
{
    public float xMin;
    public float yMin;
    public float xMax;
    public float yMax;
    public UI4(float xMin, float yMin, float xMax, float yMax);
    public string GetMin();
    public string GetMax();
    private static UI4 _fullScreen;
    public static UI4 FullScreen { get; set; }
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Betting Item (shortname)")]
    public string BettingItem { get; set; }
    [JsonProperty(PropertyName = "Betting Item Type (Item/ServerRewards/Economics)")]
    public string BettingType { get; set; }
    [JsonProperty(PropertyName = "Wipe data on map wipe")]
    public bool WipeOnNewSave { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

public class StoredData
{
    public Hash<uint, GameData> tables;
    public bool IsRegisteredTable(uint netId);
    public void RegisterTable(uint netId, GameData gameData);
    public void RemoveTable(uint netId);
    public void SetBetType(uint netId, string bettingType, string shortname);
    public class GameData
    {
        public GameType gameType;
        public int maxPlayers;
        public int minimumBet;
        public int maximumBet;
        public string bettingItemOverride;
        public string bettingTypeOverride;
        public GameData();
        public GameData(GameType gameType, int maxPlayers, int minimumBet, int maximumBet);
    }

}

public class GameData
{
    public GameType gameType;
    public int maxPlayers;
    public int minimumBet;
    public int maximumBet;
    public string bettingItemOverride;
    public string bettingTypeOverride;
    public GameData();
    public GameData(GameType gameType, int maxPlayers, int minimumBet, int maximumBet);
}


```

---

## ChatGuard

```csharp
using System.Collections.Generic;
using System.Linq;
using System;

Oxide.Plugins
[Info("Chat Guard", "LaserHydra", "2.1.0", ResourceId = 1486)]
[Description("Allows you to censor unwanted words and symbols in the chat.")]
 class ChatGuard : RustPlugin
{
     void Loaded();
     void LoadConfig();
    protected override void LoadDefaultConfig();
     object OnPlayerChat(ConsoleSystem.Arg arg);
     string FilterText(string original);
     string Replace(string original);
     string TranslateLeet(string original);
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## ChatHead

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using System.Diagnostics;
using Oxide.Core;
using UnityEngine;
using Rust;
using System.Linq;

Oxide.Plugins
[Info("ChatHead", "LeoCurtss", 0.3)]
[Description("Displays chat messages above player")]
 class ChatHead : RustPlugin
{
     Dictionary<string, string> lastChatMessage;
     void OnPlayerChat(ConsoleSystem.Arg arg);
     void DrawChatMessage(BasePlayer onlinePlayer, BasePlayer chatPlayer);
}


```

---

## ChatMinus

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("ChatMinus", "Хомячок", "2.1")]
[Description("Отправка и просмотр ЛС, тикетов админам и модераторам")]
public class ChatMinus : RustPlugin
{
    private List<BasePlayer> _activeGui;
    private List<BasePlayer> _activeFind;
    private Dictionary<BasePlayer, BasePlayer> _activeGuiModer;
    private Dictionary<BasePlayer, BasePlayer> _activeGuiAdmin;
    private Dictionary<BasePlayer, BasePlayer> _activeGuiPrivate;
    private Dictionary<BasePlayer, BasePlayer> _activeMess;
    private Dictionary<BasePlayer, string> _string;
    private const string PermModer;
    private const string PermAdmin;
    private const string UiElement;
    private const string UiInput;
    private const string UiInputFind;
    private const string UiMess;
    private const string UiPlayers;
    private const string UiNotiсe;
    private const string Sound;
    [ChatCommand("cm")]
    private void CmdChatGui(BasePlayer player, string command, string[] args);
    [ConsoleCommand("cm.change")]
    private void CmdChatMinusChange(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.opendopmenu")]
    private void CmdChatMinusMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.openinputmenu")]
    private void CmdAnswer(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.filtercharsadmininput")]
    private void CmdFilterCharsAdmin(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.filtercharsadmincharf")]
    private void CmdFilterCharsAdminCh(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.filtercharsmoderinput")]
    private void CmdFilterCharsModer(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.filtercharsmodercharf")]
    private void CmdFilterCharsModerCh(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.filtercharsplayersinput")]
    private void CmdFilterCharsPlayers(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.filtercharsplayerscharf")]
    private void CmdFilterCharsPlayersCh(ConsoleSystem.Arg arg);
    [ConsoleCommand("cm.sendchatminus")]
    private void CmdChatMinusSendMessage(ConsoleSystem.Arg arg);
    private static void CreateGui(BasePlayer player, BasePlayer target);
    private void InputGui(BasePlayer player, ulong targetUlong, int num);
    private static void FilterCharsInputMenu(BasePlayer player, int num);
    private void MessageList(BasePlayer player, BasePlayer target, int num);
    private static void ShowMessage(BasePlayer player, IEnumerable<string> messageList);
    private void ShowPlayers(BasePlayer player, Dictionary<string, ulong> list, string chars, int num, int numList, string charF);
    private static void DestroyGui(BasePlayer player, string container);
    private static void DestroyGuiNoOne(BasePlayer player);
    private static void DestroyGuiAll(BasePlayer player);
    private static bool IsNpc(BasePlayer player);
    private bool AllowModer(BasePlayer player);
    private bool AllowAdmin(BasePlayer player);
    private void Msg(BasePlayer player, string msg, object[] args);
    private string GetMsg(string key);
    private string GetMsg(string key, BasePlayer player);
    private static BasePlayer GetPlayer(ulong userId);
    private void CheckData(ulong playerId, ulong targetId);
    private static bool CheckMessage(string message);
    private void SendUpdateMessages(BasePlayer player, BasePlayer target, int num);
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerInit(BasePlayer player);
    private void Unload();
    [HookMethod("SendPrivateMessage")]
    private void SendPrivateMessage(ulong playerId, ulong targetId, string msg);
    private PlayerMessage _data;
    private void SaveData();
    private void LoadData();
    private class PlayerMessage
    {
        public Dictionary<ulong, ChatMess> PlayerMess;
        public Dictionary<string, ulong> Players;
    }

    private class ChatMess
    {
        public int CountAdminMess;
        public int CountModerMess;
        public Dictionary<DateTime, string> ModerMess;
        public Dictionary<DateTime, string> AdminMess;
        public Dictionary<ulong, Dictionary<DateTime, string>> PrivateMess;
    }

    private class Ui
    {
        public static CuiElementContainer Container(string panelName, string color, string aMin, string aMax, float fadein, bool useCursor, string parent);
        public static void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, float fadein, bool cursor);
        public static void Label(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, float fadein, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, string color1, int size, string aMin, string aMax, string command, TextAnchor align);
        public static void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
        public static string Color(string hexColor, float alpha);
    }

    private PluginConfig _config;
    private class PluginConfig
    {
        [JsonProperty("Префикс уведомлений")]
        public string Prefix { get; set; }
        [JsonProperty("Включить звуковое оповещение")]
        public bool EnableSound { get; set; }
        [JsonProperty("Звук оповещения")]
        public string Sound { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
}

private class PlayerMessage
{
    public Dictionary<ulong, ChatMess> PlayerMess;
    public Dictionary<string, ulong> Players;
}

private class ChatMess
{
    public int CountAdminMess;
    public int CountModerMess;
    public Dictionary<DateTime, string> ModerMess;
    public Dictionary<DateTime, string> AdminMess;
    public Dictionary<ulong, Dictionary<DateTime, string>> PrivateMess;
}

private class Ui
{
    public static CuiElementContainer Container(string panelName, string color, string aMin, string aMax, float fadein, bool useCursor, string parent);
    public static void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, float fadein, bool cursor);
    public static void Label(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, float fadein, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, string color1, int size, string aMin, string aMax, string command, TextAnchor align);
    public static void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
    public static string Color(string hexColor, float alpha);
}

private class PluginConfig
{
    [JsonProperty("Префикс уведомлений")]
    public string Prefix { get; set; }
    [JsonProperty("Включить звуковое оповещение")]
    public bool EnableSound { get; set; }
    [JsonProperty("Звук оповещения")]
    public string Sound { get; set; }
}


```

---

## ChatPlus

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using System.Text.RegularExpressions;
using ConVar;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("ChatPlus", "RustPlugin.ru - Developed by Vlad-00003", "1.4.0", ResourceId = 82)]
public class ChatPlus : CovalencePlugin
{
    private ChatConfig config;
    private static ChatPlus m_Instance;
    private static Dictionary<string, MuteData> mutes;
    private Dictionary<string, PlayerData> PlayersData;
    private PlayerData defaultData;
    private bool GlobalMute;
    private bool HasExpired;
    private IPlayer ConsoleIPly;
     DynamicConfigFile mutes_File;
     DynamicConfigFile players_File;
    public static Dictionary<Plugin, Func<IPlayer, string>> ThirdPartyTitles;
     void OnServerInitialized();
     void Unload();
     void OnServerSave();
    private void SaveMutes();
     void LoadData();
     object OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private string RemoveTags(string message);
     string RemoveCaps(string message);
     bool? MuteCheck(IPlayer sender);
     bool? SpamCheck(IPlayer sender, string message);
    public string CensorBadWords(string input, bool found);
    private void Log(LogType type, string msg);
     void SendChat(BasePlayer players, string name, Chat.ChatChannel channel, string message, string censorMessage, string userId);
     void BroadcastChat(string langKey, object[] args);
    private void Reply(IPlayer player, string langkey, object[] args);
     void Mute(string userID, string name, TimeSpan? time, string reason, string sender);
    private Dictionary<string, string> GetPlayers(string NameOrID);
    private void SendChatHelp(IPlayer player);
     bool IsModerator(IPlayer player);
     bool IsAdmin(IPlayer player);
     bool CanMute(IPlayer player);
     bool CanMuteAll(IPlayer player);
     bool CanAssign(IPlayer player);
     bool CanUnMute(IPlayer player);
    [Command("chatplus.prefix")]
    private void cmdConsolePrefix(IPlayer player, string cmd, string[] Args);
    [Command("chatplus.name")]
    private void cmdConsoleName(IPlayer player, string cmd, string[] Args);
    [Command("chatplus.message")]
    private void cmdConsoleMessage(IPlayer player, string cmd, string[] Args);
    [Command("muteall")]
    private void cmdChatMuteAll(IPlayer player, string cmd, string[] Args);
    [Command("mutelist")]
     void cmdChatMuteList(IPlayer player, string cmd, string[] Args);
    [Command("mute")]
     void cmdChatMute(IPlayer player, string cmd, string[] args);
    [Command("unmute")]
     void cmdChatUnMute(IPlayer player, string cmd, string[] args);
    [Command("chat")]
     void cmdChat(IPlayer player, string cmd, string[] args);
     Dictionary<string, string> pmHistory;
    [Command("pm")]
     void cmdChatPM(IPlayer player, string cmd, string[] args);
    [Command("r")]
     void cmdChatR(IPlayer player, string cmd, string[] args);
    private class MuteData
    {
        public readonly string Initiator;
        public readonly string Reason;
        public readonly DateTime ExpireDate;
        [JsonIgnore]
        private bool Timed { get; set; }
        [JsonIgnore]
        public bool Expired { get; set; }
        [JsonIgnore]
        public string Remain { get; set; }
        public static bool IsMuted(string userID);
        public MuteData(string Initiator, string Reason, TimeSpan? Until);
        [JsonConstructor]
        public MuteData(string Initiator, string Reason, DateTime ExpireDate);
        public static string TimeToString(TimeSpan elapsedTime);
        public static bool StringToTime(string source, TimeSpan time);
    }

    public class PlayerData
    {
        public string Prefix;
        public string NameColor;
        public string MessageColor;
        public bool Censor;
        public bool PMSound;
        public string Name;
        public bool IsAdmin;
        public bool IsModer;
        public List<string> BlackList;
    }

     PlayerData GetPlayerData(IPlayer player);
     PlayerData GetPlayerData(BasePlayer player);
     PlayerData GetPlayerData(string userId);
     string GetMsg(string key, object ID, object[] args);
    private void LoadMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    public class ChatPrivilege
    {
        [JsonProperty("Привилегия")]
        public string Perm;
        [JsonProperty("Аргумент")]
        public string Arg;
        [JsonProperty("Формат")]
        public string Format;
    }

    public class AdminPrivilages
    {
        [JsonProperty("Привилегия для включения режима администратора")]
        public string AdminPermiss;
        [JsonProperty("Привилегия для включения режима модератора")]
        public string ModerPermiss;
        [JsonProperty("Привилегия для использования команды /mute")]
        public string MutePermiss;
        [JsonProperty("Привилегия для использования команды /unmute")]
        public string UnMutePermiss;
        [JsonProperty("Формат чата режима администратор")]
        public string AdminFormat;
        [JsonProperty("Формат чата режима модератор")]
        public string ModerFormat;
        [JsonProperty("Привилегия для полного отключения чата")]
        public string MuteAllPermiss;
        [JsonProperty("Привилегия для использования консольных команд на присваивание префикса и цветов")]
        public string AssignPermiss;
        [JsonProperty("Скрывать имена администраторов при блокировке чата")]
        public bool? HideAdmins;
        [JsonProperty("Замена имени администратора при блокировке чата(если включено)")]
        public string AdminReplace;
    }

    public class ChatConfig
    {
        [JsonProperty("A. Автоматически блокировать чат за нецензурную лексику")]
        public bool BadWordsBlock;
        [JsonProperty("B. Длительность блокировки чата за нецензурную лексику(в секундах)")]
        public int MuteBadWordsDefault;
        [JsonProperty("C. Причина мута при автоматической блокировке чата за нецензурную лексику")]
        public string MuteReasonAutoMute;
        [JsonProperty("D. Стандартная причина мута")]
        public string MuteReason;
        [JsonProperty("E. Воспроизводить звук при получении личного сообщения")]
        public bool PrivateSoundMessage;
        [JsonProperty("F. Полный путь к звуковому файлу")]
        public string PrivateSoundMessagePath;
        [JsonProperty("G. Выключить заглавные буквы в чате")]
        public bool CapsBlock;
        [JsonProperty("H. Настройки привелегий администраторов")]
        public AdminPrivilages adminPrivilages;
        [JsonProperty("I. Цвет имен")]
        public List<ChatPrivilege> names;
        [JsonProperty("J. Префиксы")]
        public List<ChatPrivilege> prefixes;
        [JsonProperty("K. Цвет сообщений")]
        public List<ChatPrivilege> messages;
        [JsonProperty("L. Список начальных букв нецензурных слов или слова целиком | список исключений")]
        public Dictionary<string, List<string>> badWords;
        [JsonProperty("M. Имя консоли при отправке личных сообщений и отключении чата из консоли")]
        public string ConsoleName;
        [JsonProperty("N. Формат отправки сообщений из консоли командой say")]
        public string ConsoleFormat;
        [JsonProperty("O. Отображать ли аватарки игроков при получении ЛС(только RUST)")]
        public bool? PMAva;
        public void RegisterPerms();
        public ChatPrivilege Get(List<ChatPrivilege> list, string perm);
    }

     bool IsPlayerMuted(object id);
     void OnPluginUnloaded(Plugin plugin);
    private void API_RegisterThirdPartyTitle(Plugin plugin, Func<IPlayer, string> titleGetter);
    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(string uid, string permissionName);
        public static void RegisterPermissions(List<string> permissions);
        public static bool UserIDVaild(string userID);
    }

}

private class MuteData
{
    public readonly string Initiator;
    public readonly string Reason;
    public readonly DateTime ExpireDate;
    [JsonIgnore]
    private bool Timed { get; set; }
    [JsonIgnore]
    public bool Expired { get; set; }
    [JsonIgnore]
    public string Remain { get; set; }
    public static bool IsMuted(string userID);
    public MuteData(string Initiator, string Reason, TimeSpan? Until);
    [JsonConstructor]
    public MuteData(string Initiator, string Reason, DateTime ExpireDate);
    public static string TimeToString(TimeSpan elapsedTime);
    public static bool StringToTime(string source, TimeSpan time);
}

public class PlayerData
{
    public string Prefix;
    public string NameColor;
    public string MessageColor;
    public bool Censor;
    public bool PMSound;
    public string Name;
    public bool IsAdmin;
    public bool IsModer;
    public List<string> BlackList;
}

public class ChatPrivilege
{
    [JsonProperty("Привилегия")]
    public string Perm;
    [JsonProperty("Аргумент")]
    public string Arg;
    [JsonProperty("Формат")]
    public string Format;
}

public class AdminPrivilages
{
    [JsonProperty("Привилегия для включения режима администратора")]
    public string AdminPermiss;
    [JsonProperty("Привилегия для включения режима модератора")]
    public string ModerPermiss;
    [JsonProperty("Привилегия для использования команды /mute")]
    public string MutePermiss;
    [JsonProperty("Привилегия для использования команды /unmute")]
    public string UnMutePermiss;
    [JsonProperty("Формат чата режима администратор")]
    public string AdminFormat;
    [JsonProperty("Формат чата режима модератор")]
    public string ModerFormat;
    [JsonProperty("Привилегия для полного отключения чата")]
    public string MuteAllPermiss;
    [JsonProperty("Привилегия для использования консольных команд на присваивание префикса и цветов")]
    public string AssignPermiss;
    [JsonProperty("Скрывать имена администраторов при блокировке чата")]
    public bool? HideAdmins;
    [JsonProperty("Замена имени администратора при блокировке чата(если включено)")]
    public string AdminReplace;
}

public class ChatConfig
{
    [JsonProperty("A. Автоматически блокировать чат за нецензурную лексику")]
    public bool BadWordsBlock;
    [JsonProperty("B. Длительность блокировки чата за нецензурную лексику(в секундах)")]
    public int MuteBadWordsDefault;
    [JsonProperty("C. Причина мута при автоматической блокировке чата за нецензурную лексику")]
    public string MuteReasonAutoMute;
    [JsonProperty("D. Стандартная причина мута")]
    public string MuteReason;
    [JsonProperty("E. Воспроизводить звук при получении личного сообщения")]
    public bool PrivateSoundMessage;
    [JsonProperty("F. Полный путь к звуковому файлу")]
    public string PrivateSoundMessagePath;
    [JsonProperty("G. Выключить заглавные буквы в чате")]
    public bool CapsBlock;
    [JsonProperty("H. Настройки привелегий администраторов")]
    public AdminPrivilages adminPrivilages;
    [JsonProperty("I. Цвет имен")]
    public List<ChatPrivilege> names;
    [JsonProperty("J. Префиксы")]
    public List<ChatPrivilege> prefixes;
    [JsonProperty("K. Цвет сообщений")]
    public List<ChatPrivilege> messages;
    [JsonProperty("L. Список начальных букв нецензурных слов или слова целиком | список исключений")]
    public Dictionary<string, List<string>> badWords;
    [JsonProperty("M. Имя консоли при отправке личных сообщений и отключении чата из консоли")]
    public string ConsoleName;
    [JsonProperty("N. Формат отправки сообщений из консоли командой say")]
    public string ConsoleFormat;
    [JsonProperty("O. Отображать ли аватарки игроков при получении ЛС(только RUST)")]
    public bool? PMAva;
    public void RegisterPerms();
    public ChatPrivilege Get(List<ChatPrivilege> list, string perm);
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(string uid, string permissionName);
    public static void RegisterPermissions(List<string> permissions);
    public static bool UserIDVaild(string userID);
}


```

---

## ChatSystem

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Globalization;
using System.Linq;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("ChatSystem", "https://topplugin.ru/", "2.1.0")]
[Description("Улучшенная чат-система с интерфейсом! Куплено на Topplugin.ru")]
public class ChatSystem : RustPlugin
{
    private class PluginSettings
    {
        [JsonProperty("Стандартные причины мута")]
        public Dictionary<string, string> Reasons;
        [JsonProperty("Стандартные теги игроков")]
        public Dictionary<string, string> Tags;
        [JsonProperty("Скрытые теги")]
        public List<string> HiddenTags;
        [JsonProperty("Цвета доступные для выбора")]
        public Dictionary<string, string> Colors;
        [JsonProperty("Случайные сообщения")]
        public List<string> RandomMessages;
    }

    private class Chatter
    {
        [JsonProperty("Текущий тег игрока")]
        public string CurrentTag;
        [JsonProperty("Текущий цвет игрока")]
        public string CurrentColor;
        [JsonProperty("Скрывать сообщения игроков?")]
        public bool HideMessages;
        [JsonProperty("Скрывать подсказки")]
        public bool HideHelpers;
        [JsonProperty("Снятие блокировки чата")]
        public double MuteTime;
        public double IsMuted();
    }

    [JsonProperty("Настройки каждого игрока")]
    private Dictionary<ulong, Chatter> playerSettings;
    [JsonProperty("Настройки плагина")]
    private PluginSettings Settings;
    [JsonProperty("Стандартный цвет ника")]
    private string NameColor;
    [JsonProperty("Разрешение на блокировку чата игрокам")]
    private string ModerPermission;
    [JsonProperty("Системный слой")]
    private string Layer;
     List<string> BadWords;
     Dictionary<ulong, int> floods;
    private void BroadMessage(string message, string header);
    private void HandleMessage(Chat.ChatChannel channel, BasePlayer player, string message);
     void SendMessage(Chat.ChatChannel channel, BasePlayer player, string format, string message);
    private void MutePlayer(BasePlayer admin, BasePlayer target, string reason, string time);
    private void UnMutePlayer(BasePlayer admin, ulong userid);
     void OnUserPermissionRevoked(string id, string perm);
    private bool OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    [ChatCommand("mute")]
    private void cmdMuteChat(BasePlayer player);
    [ChatCommand("chat")]
    private void cmdChat(BasePlayer player);
     Dictionary<ulong, ulong> pmHistory;
     Dictionary<ulong, List<ulong>> ignoreList;
    [ChatCommand("ignore")]
    private void cmdIgnorePM(BasePlayer player, string command, string[] args);
    [ChatCommand("pm")]
    private void cmdChatPM(BasePlayer player, string command, string[] args);
    [ChatCommand("r")]
     void cmdChatR(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_MuteHandler")]
    private void consoleMuteHandler(ConsoleSystem.Arg args);
    [ConsoleCommand("UI_ChatSystem")]
    private void consoleUIHandler(ConsoleSystem.Arg args);
    private void ChatSettUp(BasePlayer player, string change);
    private void DrawGUI(BasePlayer player, string target, string reason, string time);
    public static string FormatTime(TimeSpan time, int maxSubstr, string language);
    public static long TimeToSeconds(string time);
    private static string Format(int units, string form1, string form2, string form3);
    static DateTime epoch;
    static double CurrentTime();
    private BasePlayer FindPlayer(string nameOrId);
    private static string HexToRustFormat(string hex);
}

private class PluginSettings
{
    [JsonProperty("Стандартные причины мута")]
    public Dictionary<string, string> Reasons;
    [JsonProperty("Стандартные теги игроков")]
    public Dictionary<string, string> Tags;
    [JsonProperty("Скрытые теги")]
    public List<string> HiddenTags;
    [JsonProperty("Цвета доступные для выбора")]
    public Dictionary<string, string> Colors;
    [JsonProperty("Случайные сообщения")]
    public List<string> RandomMessages;
}

private class Chatter
{
    [JsonProperty("Текущий тег игрока")]
    public string CurrentTag;
    [JsonProperty("Текущий цвет игрока")]
    public string CurrentColor;
    [JsonProperty("Скрывать сообщения игроков?")]
    public bool HideMessages;
    [JsonProperty("Скрывать подсказки")]
    public bool HideHelpers;
    [JsonProperty("Снятие блокировки чата")]
    public double MuteTime;
    public double IsMuted();
}


```

---

## ChatToConsole

```csharp
using System;
using System.Collections.Generic;
using ConVar;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Chat To Console", "Purples", "1.1.1")]
[Description("Copies the in-game chat to the console.")]
internal class ChatToConsole : RustPlugin
{
    private void CmdToggle(IPlayer player, string command, string[] args);
    private class ChatFormat
    {
        private readonly Chat.ChatChannel? _channel;
        private readonly string _formattedMessage;
        private readonly string _formattedPrefixes;
        private readonly string _formattedUsername;
        private readonly BasePlayer _sender;
        private ChatFormat(BasePlayer sender, string formattedUsername, string formattedPrefixes, string formattedMessage, Chat.ChatChannel? channel);
        public string ChatLang { get; set; }
        private static string GetFormattedText(string text, string color);
        public static ChatFormat FromRust(BasePlayer player, string message, Chat.ChatChannel channel);
        public static ChatFormat FromBetterChat(Dictionary<string, object> data);
        public static ChatFormat FromChatPlus(Dictionary<string, object> data);
        public bool ShouldSee(ulong playerid);
        public string GetFormatted();
    }

    private string _permission;
    private string _permissionAdmin;
    private static string _command;
    private static string _format;
    private static string _pmFormat;
    private static bool _enabledByDefault;
    [PluginReference]
    private Plugin ChatPlus;
    private Plugin BetterChat;
    private List<string> _listedUsers;
    private void LoadData();
    private void SaveData();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void Init();
    private string GetMsg(string key);
    private string GetMsg(string key, string playerId);
    protected override void LoadDefaultMessages();
    private void OnPlayerChat(BasePlayer sender, string message, Chat.ChatChannel channel);
    private void OnBetterChat(Dictionary<string, object> data);
    private void OnChatPlusMessage(Dictionary<string, object> data);
    [HookMethod("OnPMProcessed")]
    private void OnPrivateMessage(IPlayer sender, IPlayer receiver, string message);
    private void SendChatToConsole(ChatFormat message);
    private void SendPmToConsole(string message, string[] participants);
    private bool CheckEnabled(string id);
    private bool CanUse(string id);
    private bool CanAdmin(string id);
    private bool GetConfig(string key, T var);
}

private class ChatFormat
{
    private readonly Chat.ChatChannel? _channel;
    private readonly string _formattedMessage;
    private readonly string _formattedPrefixes;
    private readonly string _formattedUsername;
    private readonly BasePlayer _sender;
    private ChatFormat(BasePlayer sender, string formattedUsername, string formattedPrefixes, string formattedMessage, Chat.ChatChannel? channel);
    public string ChatLang { get; set; }
    private static string GetFormattedText(string text, string color);
    public static ChatFormat FromRust(BasePlayer player, string message, Chat.ChatChannel channel);
    public static ChatFormat FromBetterChat(Dictionary<string, object> data);
    public static ChatFormat FromChatPlus(Dictionary<string, object> data);
    public bool ShouldSee(ulong playerid);
    public string GetFormatted();
}


```

---

## CheckPlayers

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("CheckPlayers", "SkiTles", "0.2")]
 class CheckPlayers : RustPlugin
{
    [PluginReference]
     Plugin VKBot;
    private Dictionary<BasePlayer, BasePlayer> PlayersCheckList;
    private BasePlayer CheckCMDPlayer;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Основные настройки")]
        public MainSettings mainSet { get; set; }
        public class MainSettings
        {
            [JsonProperty(PropertyName = "Привилегия для команд вызова на проверку/завершения проверки")]
            [DefaultValue("vkbot.checkplayers")]
            public string PlCheckPerm { get; set; }
            [JsonProperty(PropertyName = "Текст уведомления")]
            [DefaultValue("<color=#990404>Модератор вызвал вас на проверку.</color> \nНапишите свой скайп с помощью команды <color=#990404>/skype <НИК в СКАЙПЕ>.</color>\nЕсли вы покините сервер, Вы будете забанены на нашем проекте серверов.")]
            public string PlCheckText { get; set; }
            [JsonProperty(PropertyName = "Бан игрока при выходе с сервера во время проверки")]
            [DefaultValue(false)]
            public bool AutoBan { get; set; }
            [JsonProperty(PropertyName = "Кастомная команда для автобана (оставить none если не нужно). Пример: banid {steamid} {reason} 4d")]
            [DefaultValue("none")]
            public string BanCmd { get; set; }
            [JsonProperty(PropertyName = "Позиция GUI AnchorMin (дефолт 0 0.826)")]
            [DefaultValue("0 0.826")]
            public string GUIAnchorMin { get; set; }
            [JsonProperty(PropertyName = "Позиция GUI AnchorMax (дефолт 1 0.965)")]
            [DefaultValue("1 0.965")]
            public string GUIAnchorMax { get; set; }
            [JsonProperty(PropertyName = "Команда вызова игрока на проверку")]
            [DefaultValue("alert")]
            public string CMDalert { get; set; }
            [JsonProperty(PropertyName = "Команда завершения проверки игрока")]
            [DefaultValue("unalert")]
            public string CMDunalert { get; set; }
            [JsonProperty(PropertyName = "Команда отправки скайпа модератору")]
            [DefaultValue("skype")]
            public string CMDskype { get; set; }
            [JsonProperty(PropertyName = "Отправка скайпа админу в ВК (при вызове на проверку консольной командой)")]
            [DefaultValue(false)]
            public bool admMsg { get; set; }
            [JsonProperty(PropertyName = "Отправка скайпа в беседу ВК (при вызове на проверку консольной командой)")]
            [DefaultValue(false)]
            public bool admChat { get; set; }
        }

    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void Loaded();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void CheckPlayerChat(BasePlayer player, string cmd, string[] args);
    private void UnCheckPlayerChat(BasePlayer player, string cmd, string[] args);
    private void SendSkype(BasePlayer player, string cmd, string[] args);
    private void CheckPlayerCMD(ConsoleSystem.Arg arg);
    private void StopCheckPlayerCMD(ConsoleSystem.Arg arg);
    [ConsoleCommand("checkplayers.gui")]
    private void PListCMD(ConsoleSystem.Arg arg);
    private CuiElement Panel(string name, string color, string anMin, string anMax, string parent, bool cursor);
    private CuiElement Text(string parent, string color, string text, TextAnchor pos, int fsize, string anMin, string anMax, string fname);
    private CuiElement Button(string name, string parent, string command, string color, string anMin, string anMax);
    private void UnloadAllGUI();
    private void StartGUI(BasePlayer player);
    private void StopGui(BasePlayer player);
    private void PListUI(BasePlayer player);
     int CalculatePages(int value);
     class GUIManager
    {
        public static Dictionary<BasePlayer, GUIManager> Players;
        public int Page;
        public static GUIManager Get(BasePlayer player);
    }

    private void StartCheck(BasePlayer moder, BasePlayer target);
    private string UserName(string name);
    private static List<BasePlayer> FindPlayersOnline(string nameOrIdOrIp);
    private void LoadMessages();
     string GetMsg(string key, BasePlayer player);
     string GetMsg(string key, object userID);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Основные настройки")]
    public MainSettings mainSet { get; set; }
    public class MainSettings
    {
        [JsonProperty(PropertyName = "Привилегия для команд вызова на проверку/завершения проверки")]
        [DefaultValue("vkbot.checkplayers")]
        public string PlCheckPerm { get; set; }
        [JsonProperty(PropertyName = "Текст уведомления")]
        [DefaultValue("<color=#990404>Модератор вызвал вас на проверку.</color> \nНапишите свой скайп с помощью команды <color=#990404>/skype <НИК в СКАЙПЕ>.</color>\nЕсли вы покините сервер, Вы будете забанены на нашем проекте серверов.")]
        public string PlCheckText { get; set; }
        [JsonProperty(PropertyName = "Бан игрока при выходе с сервера во время проверки")]
        [DefaultValue(false)]
        public bool AutoBan { get; set; }
        [JsonProperty(PropertyName = "Кастомная команда для автобана (оставить none если не нужно). Пример: banid {steamid} {reason} 4d")]
        [DefaultValue("none")]
        public string BanCmd { get; set; }
        [JsonProperty(PropertyName = "Позиция GUI AnchorMin (дефолт 0 0.826)")]
        [DefaultValue("0 0.826")]
        public string GUIAnchorMin { get; set; }
        [JsonProperty(PropertyName = "Позиция GUI AnchorMax (дефолт 1 0.965)")]
        [DefaultValue("1 0.965")]
        public string GUIAnchorMax { get; set; }
        [JsonProperty(PropertyName = "Команда вызова игрока на проверку")]
        [DefaultValue("alert")]
        public string CMDalert { get; set; }
        [JsonProperty(PropertyName = "Команда завершения проверки игрока")]
        [DefaultValue("unalert")]
        public string CMDunalert { get; set; }
        [JsonProperty(PropertyName = "Команда отправки скайпа модератору")]
        [DefaultValue("skype")]
        public string CMDskype { get; set; }
        [JsonProperty(PropertyName = "Отправка скайпа админу в ВК (при вызове на проверку консольной командой)")]
        [DefaultValue(false)]
        public bool admMsg { get; set; }
        [JsonProperty(PropertyName = "Отправка скайпа в беседу ВК (при вызове на проверку консольной командой)")]
        [DefaultValue(false)]
        public bool admChat { get; set; }
    }

}

public class MainSettings
{
    [JsonProperty(PropertyName = "Привилегия для команд вызова на проверку/завершения проверки")]
    [DefaultValue("vkbot.checkplayers")]
    public string PlCheckPerm { get; set; }
    [JsonProperty(PropertyName = "Текст уведомления")]
    [DefaultValue("<color=#990404>Модератор вызвал вас на проверку.</color> \nНапишите свой скайп с помощью команды <color=#990404>/skype <НИК в СКАЙПЕ>.</color>\nЕсли вы покините сервер, Вы будете забанены на нашем проекте серверов.")]
    public string PlCheckText { get; set; }
    [JsonProperty(PropertyName = "Бан игрока при выходе с сервера во время проверки")]
    [DefaultValue(false)]
    public bool AutoBan { get; set; }
    [JsonProperty(PropertyName = "Кастомная команда для автобана (оставить none если не нужно). Пример: banid {steamid} {reason} 4d")]
    [DefaultValue("none")]
    public string BanCmd { get; set; }
    [JsonProperty(PropertyName = "Позиция GUI AnchorMin (дефолт 0 0.826)")]
    [DefaultValue("0 0.826")]
    public string GUIAnchorMin { get; set; }
    [JsonProperty(PropertyName = "Позиция GUI AnchorMax (дефолт 1 0.965)")]
    [DefaultValue("1 0.965")]
    public string GUIAnchorMax { get; set; }
    [JsonProperty(PropertyName = "Команда вызова игрока на проверку")]
    [DefaultValue("alert")]
    public string CMDalert { get; set; }
    [JsonProperty(PropertyName = "Команда завершения проверки игрока")]
    [DefaultValue("unalert")]
    public string CMDunalert { get; set; }
    [JsonProperty(PropertyName = "Команда отправки скайпа модератору")]
    [DefaultValue("skype")]
    public string CMDskype { get; set; }
    [JsonProperty(PropertyName = "Отправка скайпа админу в ВК (при вызове на проверку консольной командой)")]
    [DefaultValue(false)]
    public bool admMsg { get; set; }
    [JsonProperty(PropertyName = "Отправка скайпа в беседу ВК (при вызове на проверку консольной командой)")]
    [DefaultValue(false)]
    public bool admChat { get; set; }
}

 class GUIManager
{
    public static Dictionary<BasePlayer, GUIManager> Players;
    public int Page;
    public static GUIManager Get(BasePlayer player);
}


```

---

## ChopperSurvival

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Linq;
using Rust;

Oxide.Plugins
[Info("ChopperSurvival", "k1lly0u", "0.2.42", ResourceId = 1590)]
 class ChopperSurvival : RustPlugin
{
    [PluginReference]
     EventManager EventManager;
    [PluginReference]
     Plugin Spawns;
    private bool isCurrent;
    private bool Active;
    private static ChopperSurvival chopperSurvival;
    private static ConfigData config;
    private FieldInfo _spawnTime;
    private float adjHeliHealth;
    private float adjHeliBulletDamage;
    private float adjMainRotorHealth;
    private float adjEngineHealth;
    private float adjTailRotorHealth;
    private float adjHeliAccuracy;
    private int WaveNumber;
    private string EventSpawnFile;
    private string MainColor;
    private string MSGColor;
    private List<CS_Player> CSPlayers;
    private List<Timer> GameTimers;
    private List<CS_Helicopter> CSHelicopters;
    public string CurrentKit;
     class CS_Player : MonoBehaviour
    {
        public BasePlayer player;
        public int deaths;
        public int points;
         void Awake();
    }

     class CS_Helicopter : MonoBehaviour
    {
        public BaseHelicopter Helicopter;
        public PatrolHelicopterAI AI;
        public Vector3 Destination;
        public float MainHealth;
        public float MaxMain;
        public float EngineHealth;
        public float MaxEngine;
        public float TailHealth;
        public float MaxTail;
        public float BodyHealth;
        public float MaxBody;
        public int UIHealth;
         void Awake();
        public int DealDamage(HitInfo info);
        public void SetDestination(Vector3 destination);
        public void SetStats(int health, float main, float engine, float tail, float damage);
        private void CheckDistance(BaseEntity entity);
    }

     class LeaderBoard
    {
        public string Name;
        public int Kills;
    }

    private List<CS_Player> SortScores();
    private string PlayerMsg(int key, CS_Player player);
    private CuiElementContainer CreateScoreboard(BasePlayer player);
    private CuiElementContainer CreateHealthIndicator(CS_Helicopter heli);
    private void CreateHealthElement(CuiElementContainer element, string panelName, string name, float maxHealth, float currentHealth, float minY);
    private void DestroyHealthUI(int number);
    private void DestroyAllHealthUI(BasePlayer player);
    private void RefreshUI();
    private void RefreshHealthUI(CS_Helicopter heli);
    private void RefreshPlayerHealthUI(BasePlayer player);
    private void AddUI(BasePlayer player);
    private void DestroyUI(BasePlayer player);
    private float[] CalcHealthPos(int number);
     void OnServerInitialized();
     void Unload();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void OnEntitySpawned(BaseNetworkable entity);
     object CanBeTargeted(BaseCombatEntity target, MonoBehaviour turret);
     void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
    private void StartRounds();
    private void NextRound();
    private void SetPlayers();
    private void SpawnWave();
    private void SpawnHelicopter(int num);
    private void InitStatModifiers();
    private void SetStatModifiers();
    private void ShowHeliStats();
    private void SetHelicopterStats();
    private void MoveToArena(BaseEntity entity);
    private Vector3 GetDestination();
    private Vector3 FindSpawnPosition(Vector3 arenaPos);
    private void DestroyEvent();
    private void DestroyHelicopters();
    private void DestroyFires(Vector3 pos);
     void KillEntity(BaseEntity entity);
    private void DestroyTimers();
    private void DestroyPlayers();
    static Vector3 FindGround(Vector3 sourcePos);
     void RegisterGame();
     void OnSelectEventGamePost(string name);
     void OnEventPlayerSpawn(BasePlayer player);
     object OnEventOpenPost();
     object OnEventCancel();
     object OnEventEndPre();
     object OnEventEndPost();
     object OnEventStartPre();
     object OnSelectKit(string kitname);
     object OnEventJoinPost(BasePlayer player);
     object OnEventLeavePost(BasePlayer player);
     void OnEventPlayerDeath(BasePlayer victim, HitInfo hitinfo);
     object OnRequestZoneName();
     object OnSelectSpawnFile(string name);
     object OnEventStartPost();
    private string MSG(string msg);
    private void MessageAll(string msg, string keyword, bool title);
    private void MessageAllPlayers(string msg, string keyword, bool title);
    private void MessagePlayer(BasePlayer player, string msg, string keyword, bool title);
    private void RegisterMessages();
    private ConfigData configData;
     class EventSettings
    {
        public string DefaultKit { get; set; }
        public string DefaultSpawnfile { get; set; }
        public string DefaultZoneID { get; set; }
        public int MaximumWaves { get; set; }
        public int MaximumHelicopters { get; set; }
        public bool ShowStatsInConsole { get; set; }
        public bool ShowHeliHealthUI { get; set; }
    }

     class PlayerSettings
    {
        public float StartHealth { get; set; }
        public float StartHydration { get; set; }
        public float StartCalories { get; set; }
        public int DeathLimit { get; set; }
        public float FFDamageScale { get; set; }
    }

     class HeliSettings
    {
        public float HeliBulletDamage { get; set; }
        public float HeliHealth { get; set; }
        public float MainRotorHealth { get; set; }
        public float TailRotorHealth { get; set; }
        public float EngineHealth { get; set; }
        public float HeliSpeed { get; set; }
        public float HeliAccuracy { get; set; }
        public float HeliModifier { get; set; }
        public float SpawnDistance { get; set; }
        public float CheckDistanceTimer { get; set; }
        public float DestinationHeightAdjust { get; set; }
        public float SpawnWaveTimer { get; set; }
        public float SpawnBeginTimer { get; set; }
        public bool UseRockets { get; set; }
    }

     class Messaging
    {
        public string MainColor { get; set; }
        public string MSGColor { get; set; }
    }

     class ConfigData
    {
        public EventSettings EventSettings { get; set; }
        public HeliSettings HelicopterSettings { get; set; }
        public PlayerSettings PlayerSettings { get; set; }
        public Messaging Messaging { get; set; }
        public Scoring Scoring { get; set; }
    }

     class Scoring
    {
        public int RotorHitPoints { get; set; }
        public int HeliHitPoints { get; set; }
        public int SurvivalTokens { get; set; }
        public int WinnerTokens { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void AddPoints();
     void FindWinner();
     void Winner(BasePlayer player);
}

 class CS_Player : MonoBehaviour
{
    public BasePlayer player;
    public int deaths;
    public int points;
     void Awake();
}

 class CS_Helicopter : MonoBehaviour
{
    public BaseHelicopter Helicopter;
    public PatrolHelicopterAI AI;
    public Vector3 Destination;
    public float MainHealth;
    public float MaxMain;
    public float EngineHealth;
    public float MaxEngine;
    public float TailHealth;
    public float MaxTail;
    public float BodyHealth;
    public float MaxBody;
    public int UIHealth;
     void Awake();
    public int DealDamage(HitInfo info);
    public void SetDestination(Vector3 destination);
    public void SetStats(int health, float main, float engine, float tail, float damage);
    private void CheckDistance(BaseEntity entity);
}

 class LeaderBoard
{
    public string Name;
    public int Kills;
}

 class EventSettings
{
    public string DefaultKit { get; set; }
    public string DefaultSpawnfile { get; set; }
    public string DefaultZoneID { get; set; }
    public int MaximumWaves { get; set; }
    public int MaximumHelicopters { get; set; }
    public bool ShowStatsInConsole { get; set; }
    public bool ShowHeliHealthUI { get; set; }
}

 class PlayerSettings
{
    public float StartHealth { get; set; }
    public float StartHydration { get; set; }
    public float StartCalories { get; set; }
    public int DeathLimit { get; set; }
    public float FFDamageScale { get; set; }
}

 class HeliSettings
{
    public float HeliBulletDamage { get; set; }
    public float HeliHealth { get; set; }
    public float MainRotorHealth { get; set; }
    public float TailRotorHealth { get; set; }
    public float EngineHealth { get; set; }
    public float HeliSpeed { get; set; }
    public float HeliAccuracy { get; set; }
    public float HeliModifier { get; set; }
    public float SpawnDistance { get; set; }
    public float CheckDistanceTimer { get; set; }
    public float DestinationHeightAdjust { get; set; }
    public float SpawnWaveTimer { get; set; }
    public float SpawnBeginTimer { get; set; }
    public bool UseRockets { get; set; }
}

 class Messaging
{
    public string MainColor { get; set; }
    public string MSGColor { get; set; }
}

 class ConfigData
{
    public EventSettings EventSettings { get; set; }
    public HeliSettings HelicopterSettings { get; set; }
    public PlayerSettings PlayerSettings { get; set; }
    public Messaging Messaging { get; set; }
    public Scoring Scoring { get; set; }
}

 class Scoring
{
    public int RotorHitPoints { get; set; }
    public int HeliHitPoints { get; set; }
    public int SurvivalTokens { get; set; }
    public int WinnerTokens { get; set; }
}


```

---

## Christmas

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Christmas", "", "1.0.0")]
[Description("")]
public class Christmas : RustPlugin
{
    private const string PLAYER_PERM;
    private static Christmas ins { get; set; }
    private void Init();
    private void Unload();
    public void RefillPresents();
    [ConsoleCommand("giftwqdwqwqdwqdwq")]
    private void GiftsConsole(ConsoleSystem.Arg arg);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Event Automation Settings")]
        public AutomationOptions Automation { get; set; }
        public class AutomationOptions
        {
            [JsonProperty(PropertyName = "Time in-between presents and stocking refills (minutes)")]
            public int refillTime { get; set; }
            [JsonProperty(PropertyName = "Distance a player in which to spawn")]
            public int playerDistance { get; set; }
            [JsonProperty(PropertyName = "Gifts per player")]
            public int giftsPerPlayer { get; set; }
            [JsonProperty(PropertyName = "Broadcast Message enabled to players when gifts sent (true/false)")]
            public bool messagesEnabled { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
     T GetConfig(string name, T defaultValue);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Event Automation Settings")]
    public AutomationOptions Automation { get; set; }
    public class AutomationOptions
    {
        [JsonProperty(PropertyName = "Time in-between presents and stocking refills (minutes)")]
        public int refillTime { get; set; }
        [JsonProperty(PropertyName = "Distance a player in which to spawn")]
        public int playerDistance { get; set; }
        [JsonProperty(PropertyName = "Gifts per player")]
        public int giftsPerPlayer { get; set; }
        [JsonProperty(PropertyName = "Broadcast Message enabled to players when gifts sent (true/false)")]
        public bool messagesEnabled { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class AutomationOptions
{
    [JsonProperty(PropertyName = "Time in-between presents and stocking refills (minutes)")]
    public int refillTime { get; set; }
    [JsonProperty(PropertyName = "Distance a player in which to spawn")]
    public int playerDistance { get; set; }
    [JsonProperty(PropertyName = "Gifts per player")]
    public int giftsPerPlayer { get; set; }
    [JsonProperty(PropertyName = "Broadcast Message enabled to players when gifts sent (true/false)")]
    public bool messagesEnabled { get; set; }
}


```

---

## Clans

```csharp
using Facepunch.Extend;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Clans", "C cайта на букву T", "1.2.2", ResourceId = 14)]
public class Clans : RustPlugin
{
     bool Changed;
     bool Initialized;
    internal static Clans cc;
     bool newSaveDetected;
     List<ulong> manuallyEnabledBy;
     HashSet<ulong> bypass;
     Dictionary<string, DateTime> notificationTimes;
    static readonly DateTime UnixEpoch;
    static readonly double MaxUnixSeconds;
     Library lib;
    public Dictionary<string, Clan> clans;
    public Dictionary<string, string> clansSearch;
     List<string> purgedClans;
     Dictionary<string, string> originalNames;
     Dictionary<string, List<string>> pendingPlayerInvites;
     Regex tagReExt;
     Dictionary<string, Clan> clanCache;
     List<object> filterDefaults();
    public int limitMembers;
     int limitModerators;
    public int limitAlliances;
     int tagLengthMin;
     int tagLengthMax;
     int inviteValidDays;
     int friendlyFireNotifyTimeout;
     string allowedSpecialChars;
    public bool enableFFOPtion;
     bool enableAllyFFOPtion;
     bool enableWordFilter;
     bool enableClanTagging;
    public bool enableClanAllies;
     bool forceAllyFFNoDeactivate;
     bool forceClanFFNoDeactivate;
     bool enableWhoIsOnlineMsg;
     bool enableComesOnlineMsg;
     int authLevelRename;
     int authLevelDelete;
     int authLevelInvite;
     int authLevelKick;
     int authLevelPromoteDemote;
     int authLevelClanInfo;
     bool purgeOldClans;
     int notUpdatedSinceDays;
     bool listPurgedClans;
     bool wipeClansOnNewSave;
     bool useProtostorageClandata;
     string consoleName;
     string broadcastPrefix;
     string broadcastPrefixAlly;
     string broadcastPrefixColor;
     string broadcastPrefixFormat;
     string broadcastMessageColor;
     string colorCmdUsage;
     string colorTextMsg;
     string colorClanNamesOverview;
     string colorClanFFOff;
     string colorClanFFOn;
     string pluginPrefix;
     string pluginPrefixColor;
     string pluginPrefixREBORNColor;
     bool pluginPrefixREBORNShow;
     string pluginPrefixFormat;
     string clanServerColor;
     string clanOwnerColor;
     string clanCouncilColor;
     string clanModeratorColor;
     string clanMemberColor;
     bool setHomeOwner;
     bool setHomeModerator;
     bool setHomeMember;
     string chatCommandClan;
     string chatCommandFF;
     string chatCommandAllyChat;
     string chatCommandClanChat;
     string chatCommandClanInfo;
     string subCommandClanHelp;
     string subCommandClanAlly;
     bool usePermGroups;
     string permGroupPrefix;
     bool usePermToCreateClan;
     string permissionToCreateClan;
     bool usePermToJoinClan;
     string permissionToJoinClan;
     bool addClanMembersAsIOFriends;
     string clanTagColorBetterChat;
     int clanTagSizeBetterChat;
     string clanTagOpening;
     string clanTagClosing;
     bool clanChatDenyOnMuted;
     List<string> activeRadarUsers;
     Dictionary<string, List<BasePlayer>> clanRadarMemberobjects;
    static Vector3 sleeperHeight;
    static Vector3 playerHeight;
     bool enableClanRadar;
     string colorClanRadarOff;
     string colorClanRadarOn;
     float refreshTime;
     string nameColor;
     string sleeperNameColor;
     string distanceColor;
    static float minDistance;
    static float maxNamedistance;
    static float maxSleeperDistance;
     bool showSleepers;
     bool extendOnAllyMembers;
     bool enableAtLogin;
     int radarTextSize;
     string permissionClanRadarUse;
     bool usePermissionClanRadar;
     string chatCommandRadar;
     List<object> wordFilter;
     object GetConfig(string menu, string datavalue, object defaultValue);
     int PointsOfDeath;
     int PointsOfKilled;
     int PointsOfKilledHeli;
     int PointsOfSuicide;
     int PointsOfGatherSulfur;
     int PointsOfGatherMetalOre;
     int PointsOfGatherStone;
     int PointsOfGatherWood;
     int PointsOfGatherHQM;
     int PointsOfBarrel;
     void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void Init();
     void OnPluginLoaded(Plugin plugin);
     string getFormattedClanTag(IPlayer player);
    [PluginReference]
    private Plugin ImageLibrary;
    public string GetImageSkin(string shortname, ulong skin);
    public List<ulong> GetImageSkins(string shortname);
    static Clans ins;
     void OnServerInitialized();
     void OnServerSave();
     void OnNewSave();
     void Unload();
     void LoadData();
     void InitializeClans(bool newFileFound);
     void SaveData(bool force);
    public Clan findClan(string tag);
    public Clan findClanByUser(string userId);
     void setupPlayer(BasePlayer player, string cName, string cId);
     void setupPlayers(List<string> playerIds);
     void OnPlayerConnected(BasePlayer player);
     IEnumerator WaitForReady(BasePlayer player, Clan clan);
     void ComingOnlineInfo(BasePlayer player, Clan clan);
     void OnPlayerDisconnected(BasePlayer player);
     void OnDie(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerAttack(BasePlayer attacker, HitInfo hit);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hit);
     object OnAttackShared(BasePlayer attacker, BasePlayer victim, HitInfo hit);
     void AllyRemovalCheck();
     void cmdChatClan(BasePlayer player, string command, string[] args);
     void cmdClanOverview(BasePlayer player);
     void cmdClanCreate(BasePlayer player, string[] args);
    public void InvitePlayer(BasePlayer player, string targetId);
     void cmdClanInvite(BasePlayer player, string[] args);
     string ButtonListed;
    [ConsoleCommand("clanui.page")]
     void cmdConsoleClanUI(ConsoleSystem.Arg args);
    private string GetImageUrl(string shortname, ulong skinid);
    private void AddLoadOrder(IDictionary<string, string> imageList, bool replace);
    private string GetImage(string shortname, ulong skinid);
     void CLanUIInfo(BasePlayer player, string command, string[] args);
     bool? CanWearItem(PlayerInventory inventory, Item item, int targetPos);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
    [ConsoleCommand("clans_getskinIds")]
     void cmdClansGetSkinList(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_setChange")]
     void cmdSetChangeOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_Change")]
     void cmdSetNewChangeOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_setAvatar")]
     void cmdSetAvatarOfClan(ConsoleSystem.Arg args);
     class Position
    {
        public float Xmin;
        public float Xmax;
        public float Ymin;
        public float Ymax;
        public string AnchorMin { get; set; }
        public string AnchorMax { get; set; }
        public override string ToString();
    }

    [SuppressMessage("ReSharper", "CompareOfFloatsByEqualityOperator")]
    private static List<Position> GetPositions(int colums, int rows, float colPadding, float rowPadding, bool columsFirst);
    [ConsoleCommand("clan_changeskin")]
     void cmdChatSkinOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_kickplayer")]
     void cmdKickOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_changeAvatar")]
     void cmdChangeAvatarOfClan(ConsoleSystem.Arg args);
    private Dictionary<uint, ulong> LastHeliHit;
    private BasePlayer GetLastHeliAttacker(uint heliNetId);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private bool IsNPC(BasePlayer player);
    [PluginReference]
    private Plugin ItemNameLocalizator;
    private bool IsInlReady { get; set; }
    public bool IsReady();
    private string GetItemName(string shortname, object player);
     int GetPercent(int need, int current);
     double GetPercentFUll(double need, double current);
     string ButtonListedClan;
     string TOP;
     string InfoTOP;
     string InfoTOPButton;
    [ConsoleCommand("clanstop_info")]
     void cmdClansTopKey(ConsoleSystem.Arg arg);
    [ConsoleCommand("clanstop_main")]
     void cmdClansTopMain(ConsoleSystem.Arg arg);
    [ChatCommand("ctop")]
     void cmdClanTOP(BasePlayer player, string command, string[] args);
    public int GetClanIndex(string key);
     string GetPlayerStatus(string player, Clan clan);
     void ClanTOPInfo(BasePlayer player, Clan clan, int page);
     void ClanTOP(BasePlayer player, int page);
     void ClanUI(BasePlayer player, int page, bool changeSkin, bool changeResource);
     long GetFullPercent(string id);
     long GetFullClanPercent(string tag);
    public static string FormatShortTime(TimeSpan time);
    public void WithdrawPlayer(BasePlayer player, string targetId);
     void cmdClanWithdraw(BasePlayer player, string[] args);
     void cmdClanJoin(BasePlayer player, string[] args);
    [ConsoleCommand("clanui_promote")]
     void cmdClanPromoteMember(ConsoleSystem.Arg args);
    public void PromotePlayer(BasePlayer player, string targetId);
     void cmdClanPromote(BasePlayer player, string[] args);
    public void DemotePlayer(BasePlayer player, string targetId);
     void cmdClanDemote(BasePlayer player, string[] args);
    public void LeaveClan(BasePlayer player);
     void cmdClanLeave(BasePlayer player, string[] args);
    public void KickPlayer(BasePlayer player, string targetId);
     void cmdClanKick(BasePlayer player, string[] args);
    public void DisbandClan(BasePlayer player);
     void cmdClanDisband(BasePlayer player, string[] args);
    public void Alliance(BasePlayer player, string targetClan, string type);
     void cmdChatClanAlly(BasePlayer player, string command, string[] args);
     void cmdChatClanHelp(BasePlayer player, string command, string[] args);
     void cmdChatClanInfo(BasePlayer player, string command, string[] args);
     void cmdChatClanchat(BasePlayer player, string command, string[] args);
     void cmdChatAllychat(BasePlayer player, string command, string[] args);
     void cmdChatClanFF(BasePlayer player, string command, string[] args);
    public bool HasFFEnabled(ulong playerId);
    public void ToggleFF(ulong playerId);
     void cmdChatClanRadar(BasePlayer player, string command, string[] args);
    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
     class StoredData
    {
        public Dictionary<string, Clan> clans;
        public Int32 saveStamp;
        public string lastStorage;
        public StoredData();
    }

     StoredData clanSaves;
    public class ChangeListed
    {
        public uint Need { get; set; }
        public uint Complete { get; set; }
    }

    public class MembersChangeList
    {
        public int Complete { get; set; }
    }

    public class PlayerStats
    {
        public int PlayerPoints;
        public int Killed;
        public int Death;
        public int Suicide;
        public int KilledHeli;
        public int GatherStone;
        public int GatherSulfur;
        public int GatherMetal;
        public int GatherHQM;
        public int GatherWood;
        public int KilledBarrel;
        public Dictionary<string, int> GatherInfo;
    }

    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
    public class Clan
    {
        public int ClanPoints;
        public string tag;
        public string description;
        public string owner;
        public string ownerName;
        public string ClanAvatar;
        public string council;
        public int created;
        public int updated;
        [JsonIgnore, ProtoIgnore]
        public int online;
        [JsonIgnore, ProtoIgnore]
        public int total;
        [JsonIgnore, ProtoIgnore]
        public int mods;
        public List<string> moderators;
        public Dictionary<string, PlayerStats> members;
        public Dictionary<string, int> invites;
        public List<string> clanAlliances;
        public List<string> invitedAllies;
        public List<string> pendingInvites;
        public Dictionary<string, ulong> SkinList;
        public Dictionary<string, ChangeListed> Change;
        public static Clan Create(string tag, string description, string ownerId, string owName, string URL);
        public bool IsOwner(string userId);
        public bool IsCouncil(string userId);
        public bool IsModerator(string userId);
        public bool IsMember(string userId);
        public bool IsInvited(string userId);
        public void BroadcastChat(string message);
        public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
        public void AllyBroadcastChat(string message);
        public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
        public string ColNam(string Id, string Name);
        public string PlayerLevel(string userID);
        public string PlayerColor(string userID);
        public IPlayer GetIPlayer(string partialName);
        public IPlayer GetIMember(string partialName);
        public List<IPlayer> GetIMembers();
        public List<IPlayer> GetInvites();
        internal JObject ToJObject();
        internal void onCreate();
        internal void onUpdate();
        internal void onDestroy();
    }

    sealed class ClanRadar : FacepunchBehaviour
    {
         BasePlayer player;
         Clan clan;
         bool noAdmin;
         void Awake();
        public void DoStart();
         void SetPlayerFlag(BasePlayer.PlayerFlags f, bool b);
         void DoRadar();
         void DoDestroy();
         void OnDestroy();
    }

     void RemoveRadar(BasePlayer player, string tag, bool leftOrKicked);
     void RemoveRadarGroup(List<string> playerIds, string tag, bool isDisband);
    [HookMethod("GetClan")]
    private JObject GetClan(string tag);
    [HookMethod("GetAllClans")]
    private JArray GetAllClans();
    [HookMethod("GetClanOf")]
    private string GetClanOf(ulong player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(string player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(BasePlayer player);
    [HookMethod("GetClanMembers")]
    private List<string> GetClanMembers(ulong PlayerID);
    [HookMethod("HasFriend")]
    private object HasFriend(ulong entOwnerID, ulong PlayerUserID);
    private bool IsClanMember(string playerId, string otherId);
    private bool IsClanMember(ulong playerId, ulong otherId);
    private bool IsMemberOrAlly(string playerId, string otherId);
    private bool IsAllyPlayer(string playerId, string otherId);
    [HookMethod("IsModerator")]
    private object IsModerator(ulong PlayerUserID);
    private Int32 UnixTimeStampUTC();
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
     string msg(string key, string id);
     void PrintChat(BasePlayer player, string message);
    [ConsoleCommand("clans")]
     void cclans(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.cmds")]
     void cclansCommands(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.list")]
     void cclansList(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.showduplicates")]
     void cclansDuplicates(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.listex")]
     void cclansListEx(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.show")]
     void cclansShow(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.msg")]
     void cclansBroadcast(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.rename")]
     void cclansRename(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerinvite")]
     void cclansPlayerInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerjoin")]
     void cclansPlayerJoin(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerkick")]
     void cclansPlayerKick(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.changeowner")]
     void cclansChangeOwner(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerpromote")]
     void cclansPlayerPromote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerdemote")]
     void cclansPlayerDemote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.delete")]
     void cclansDelete(ConsoleSystem.Arg arg);
     bool FilterText(string tag);
     string TranslateLeet(string original);
     bool TryGetClan(string input, Clan clan);
     void RemoveClan(string tag);
    [HookMethod("EnableBypass")]
     void EnableBypass(object userId);
    [HookMethod("DisableBypass")]
     void DisableBypass(object userId);
}

 class Position
{
    public float Xmin;
    public float Xmax;
    public float Ymin;
    public float Ymax;
    public string AnchorMin { get; set; }
    public string AnchorMax { get; set; }
    public override string ToString();
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
 class StoredData
{
    public Dictionary<string, Clan> clans;
    public Int32 saveStamp;
    public string lastStorage;
    public StoredData();
}

public class ChangeListed
{
    public uint Need { get; set; }
    public uint Complete { get; set; }
}

public class MembersChangeList
{
    public int Complete { get; set; }
}

public class PlayerStats
{
    public int PlayerPoints;
    public int Killed;
    public int Death;
    public int Suicide;
    public int KilledHeli;
    public int GatherStone;
    public int GatherSulfur;
    public int GatherMetal;
    public int GatherHQM;
    public int GatherWood;
    public int KilledBarrel;
    public Dictionary<string, int> GatherInfo;
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
public class Clan
{
    public int ClanPoints;
    public string tag;
    public string description;
    public string owner;
    public string ownerName;
    public string ClanAvatar;
    public string council;
    public int created;
    public int updated;
    [JsonIgnore, ProtoIgnore]
    public int online;
    [JsonIgnore, ProtoIgnore]
    public int total;
    [JsonIgnore, ProtoIgnore]
    public int mods;
    public List<string> moderators;
    public Dictionary<string, PlayerStats> members;
    public Dictionary<string, int> invites;
    public List<string> clanAlliances;
    public List<string> invitedAllies;
    public List<string> pendingInvites;
    public Dictionary<string, ulong> SkinList;
    public Dictionary<string, ChangeListed> Change;
    public static Clan Create(string tag, string description, string ownerId, string owName, string URL);
    public bool IsOwner(string userId);
    public bool IsCouncil(string userId);
    public bool IsModerator(string userId);
    public bool IsMember(string userId);
    public bool IsInvited(string userId);
    public void BroadcastChat(string message);
    public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
    public void AllyBroadcastChat(string message);
    public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
    public string ColNam(string Id, string Name);
    public string PlayerLevel(string userID);
    public string PlayerColor(string userID);
    public IPlayer GetIPlayer(string partialName);
    public IPlayer GetIMember(string partialName);
    public List<IPlayer> GetIMembers();
    public List<IPlayer> GetInvites();
    internal JObject ToJObject();
    internal void onCreate();
    internal void onUpdate();
    internal void onDestroy();
}

sealed class ClanRadar : FacepunchBehaviour
{
     BasePlayer player;
     Clan clan;
     bool noAdmin;
     void Awake();
    public void DoStart();
     void SetPlayerFlag(BasePlayer.PlayerFlags f, bool b);
     void DoRadar();
     void DoDestroy();
     void OnDestroy();
}


```

---

## Clans (1)

```csharp
using CompanionServer;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Plugins;
using ProtoBuf;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Clans", "k1lly0u", "3.0.24")]
 class Clans : RustPlugin
{
    [PluginReference]
    private Plugin DiscordClans;
    internal StoredData storedData;
    private bool wipeData;
    private bool isInitialized;
    private Coroutine initClansRoutine;
    private Regex tagFilter;
    private Regex hexFilter;
    private int[] customTagMinValue;
    private int[] customTagMaxValue;
    private HashSet<ulong> friendlyFireDisabled;
    public static Clans Instance { get; set; }
    private static DateTime Epoch;
    private static double MaxUnixSeconds;
    private const string COLORED_LABEL;
    private void Loaded();
    private void OnServerInitialized();
    private void OnNewSave(string str);
    private void OnServerSave();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private object OnTeamCreate(BasePlayer player);
    private object OnTeamKick(RelationshipManager.PlayerTeam playerTeam, BasePlayer player, ulong targetId);
    private object OnTeamInvite(BasePlayer player, BasePlayer other);
    private object OnTeamAcceptInvite(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private object OnTeamRejectInvite(BasePlayer player, RelationshipManager.PlayerTeam playerTeam);
    private object OnTeamLeave(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private object OnTeamPromote(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private object OnTeamDisband(RelationshipManager.PlayerTeam playerTeam);
    private object OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private void Unload();
    private IEnumerator InitializeClans();
    private void SetMinMaxColorRange();
    private bool TagColorIsBlocked(string color);
    private bool TagColorWithinRange(string color);
    public class TagEqualityComparer : IEqualityComparer<string>
    {
        private static TagEqualityComparer _instance;
        public static TagEqualityComparer Instance { get; set; }
        public bool Equals(string a, string b);
        public int GetHashCode(string obj);
    }

    private string BetterChat_FormattedClanTag(Oxide.Core.Libraries.Covalence.IPlayer player);
    private static int UnixTimeStampUTC();
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private bool ContainsBlockedWord(string tag);
    private string TranslateLeet(string original);
    private bool ClanTagExists(string tag);
    private string FormatTime(double time);
    private BasePlayer FindPlayer(string partialNameOrID);
    private static void RemoveFromTeam(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private static void RemoveFromTeam(RelationshipManager.PlayerTeam playerTeam, ulong playerId);
    private static void ClearTeam(RelationshipManager.PlayerTeam playerTeam);
    private static string RemoveTags(string str);
    private static List<KeyValuePair<string, string>> _tags;
    internal void CreateClan(BasePlayer player, string tag, string description);
    internal bool InvitePlayer(BasePlayer inviter, ulong targetId);
    internal bool InvitePlayer(BasePlayer inviter, BasePlayer invitee);
    internal bool WithdrawInvite(BasePlayer player, string partialNameOrID);
    internal bool RejectInvite(BasePlayer player, string tag);
    internal bool JoinClan(BasePlayer player, string tag);
    internal bool LeaveClan(BasePlayer player);
    internal bool KickPlayer(BasePlayer player, ulong playerId);
    internal bool PromotePlayer(BasePlayer promoter, ulong targetId);
    internal bool DemotePlayer(BasePlayer demoter, ulong targetId);
    internal bool DisbandClan(BasePlayer player);
    internal bool OfferAlliance(BasePlayer player, string tag);
    internal bool WithdrawAlliance(BasePlayer player, string tag);
    internal bool AcceptAlliance(BasePlayer player, string tag);
    internal bool RejectAlliance(BasePlayer player, string tag);
    internal bool RevokeAlliance(BasePlayer player, string tag);
    private void ClanChat(BasePlayer player, string message);
    private void AllianceChat(BasePlayer player, string message);
    private void cmdAllianceChat(BasePlayer player, string command, string[] args);
    private void cmdClanChat(BasePlayer player, string command, string[] args);
    private void cmdClanFF(BasePlayer player, string command, string[] args);
    private void cmdAllyFF(BasePlayer player, string command, string[] args);
    private void cmdChatClanInfo(BasePlayer player, string command, string[] args);
    private void cmdChatClanHelp(BasePlayer player, string command, string[] args);
    private void cmdChatClanAlly(BasePlayer player, string command, string[] args);
    private void cmdChatClan(BasePlayer player, string command, string[] args);
    private void DisableBypass(ulong userId);
    private void EnableBypass(ulong userId);
    private JObject GetClan(string tag);
    private JArray GetAllClans();
    private string GetClanOf(ulong playerId);
    private string GetClanOf(BasePlayer player);
    private string GetClanOf(string playerId);
    private List<string> GetClanMembers(ulong playerId);
    private List<string> GetClanMembers(string playerId);
    private object HasFriend(ulong ownerId, ulong playerId);
    private object HasFriend(string ownerId, string playerId);
    private bool IsClanMember(ulong playerId, ulong otherId);
    private bool IsClanMember(string playerId, string otherId);
    private bool IsMemberOrAlly(ulong playerId, ulong otherId);
    private bool IsMemberOrAlly(string playerId, string otherId);
    private bool IsAllyPlayer(ulong playerId, ulong otherId);
    private bool IsAllyPlayer(string playerId, string otherId);
    private List<string> GetClanAlliances(ulong playerId);
    private List<string> GetClanAlliances(string playerId);
    [HookMethod("ToggleFF")]
    public void ToggleFF(ulong playerId);
    [HookMethod("HasFFEnabled")]
    public bool HasFFEnabled(ulong playerId);
    [Serializable, ProtoContract]
    public class Clan
    {
        [ProtoMember(1)]
        public string Tag { get; set; }
        [ProtoMember(2)]
        public string Description { get; set; }
        [ProtoMember(3)]
        public ulong OwnerID { get; set; }
        [ProtoMember(4)]
        public double CreationTime { get; set; }
        [ProtoMember(5)]
        public double LastOnlineTime { get; set; }
        [ProtoMember(6)]
        public Hash<ulong, Member> ClanMembers { get; set; }
        [ProtoMember(7)]
        public Hash<ulong, MemberInvite> MemberInvites { get; set; }
        [ProtoMember(8)]
        public HashSet<string> Alliances { get; set; }
        [ProtoMember(9)]
        public Hash<string, double> AllianceInvites { get; set; }
        [ProtoMember(10)]
        public HashSet<string> IncomingAlliances { get; set; }
        [ProtoMember(11)]
        public string TagColor { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int OnlineCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal ulong CouncilID { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int ModeratorCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int MemberCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int MemberInviteCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int AllianceCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int AllianceInviteCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        private RelationshipManager.PlayerTeam _playerTeam;
        [JsonIgnore, ProtoIgnore]
        internal RelationshipManager.PlayerTeam PlayerTeam { get; set; }
        private static ulong FindRandomTeamID { get; set; }
        public Clan();
        public Clan(BasePlayer player, string tag, string description);
        internal void OnPlayerConnected(BasePlayer player);
        internal void OnPlayerDisconnected(BasePlayer player);
        internal bool InvitePlayer(BasePlayer inviter, BasePlayer invitee);
        internal bool JoinClan(BasePlayer player);
        internal bool LeaveClan(BasePlayer player);
        internal bool KickMember(BasePlayer player, ulong targetId);
        internal bool PromotePlayer(BasePlayer promoter, ulong targetId);
        internal bool DemotePlayer(BasePlayer demoter, ulong targetId);
        internal void DisbandClan();
        internal void OnClanDisbanded(string tag);
        internal void OnUnload();
        internal bool IsAlliedClan(string otherClan);
        internal void MarkDirty();
        internal void Broadcast(string message);
        internal void Broadcast(string key, object[] args);
        [JsonIgnore, ProtoIgnore]
        private string cachedClanInfo;
        [JsonIgnore, ProtoIgnore]
        private string membersOnline;
        internal void PrintClanInfo(BasePlayer player);
        internal string GetMembersOnline();
        internal bool IsOwner(ulong playerId);
        internal bool IsCouncil(ulong playerId);
        internal bool IsModerator(ulong playerId);
        internal bool IsMember(ulong playerId);
        internal Member GetOwner();
        internal bool HasCouncil();
        internal string GetRoleColor(ulong userID);
        internal string GetRoleColor(Member.MemberRole role);
        [Serializable, ProtoContract]
        public class Member
        {
            [JsonIgnore, ProtoIgnore]
            public BasePlayer Player { get; set; }
            [ProtoMember(1)]
            public string DisplayName { get; set; }
            [ProtoMember(2)]
            public MemberRole Role { get; set; }
            [ProtoMember(3)]
            public bool MemberFFEnabled { get; set; }
            [ProtoMember(4)]
            public bool AllyFFEnabled { get; set; }
            [JsonIgnore, ProtoIgnore]
            internal bool IsConnected { get; set; }
            [JsonIgnore, ProtoIgnore]
            internal float lastFFAttackTime;
            [JsonIgnore, ProtoIgnore]
            internal float lastAFFAttackTime;
            public Member();
            public Member(MemberRole role, Clan clan);
            public Member(MemberRole role, bool memberFFEnabled, bool allyFFEnabled);
            public void OnClanMemberHit(string victimName);
            public void OnAllyMemberHit(string victimName);
        }

        [Serializable, ProtoContract]
        public class MemberInvite
        {
            [ProtoMember(1)]
            public string DisplayName { get; set; }
            [ProtoMember(2)]
            public double ExpiryTime { get; set; }
            public MemberInvite();
            public MemberInvite(BasePlayer player);
        }

        [JsonIgnore, ProtoIgnore]
        private JObject serializedClanObject;
        internal JObject ToJObject();
        internal ulong FindPlayer(string partialNameOrID);
    }

    [ConsoleCommand("clans")]
    private void ccmdClans(ConsoleSystem.Arg arg);
    internal static ConfigData configData;
    internal class ConfigData
    {
        [JsonProperty(PropertyName = "Clan Options")]
        public ClanOptions Clans { get; set; }
        [JsonProperty(PropertyName = "Command Options")]
        public CommandOptions Commands { get; set; }
        [JsonProperty(PropertyName = "Role Colors")]
        public ColorOptions Colors { get; set; }
        [JsonProperty(PropertyName = "Clan Tag Options")]
        public TagOptions Tags { get; set; }
        [JsonProperty(PropertyName = "Permission Options")]
        public PermissionOptions Permissions { get; set; }
        [JsonProperty(PropertyName = "Purge Options")]
        public PurgeOptions Purge { get; set; }
        [JsonProperty(PropertyName = "Settings")]
        public OtherOptions Options { get; set; }
        public class ClanOptions
        {
            [JsonProperty(PropertyName = "Member limit")]
            public int MemberLimit { get; set; }
            [JsonProperty(PropertyName = "Moderator limit")]
            public int ModeratorLimit { get; set; }
            [JsonProperty(PropertyName = "Allow friendly fire toggle (clan members)")]
            public bool MemberFF { get; set; }
            [JsonProperty(PropertyName = "Enable friendly fire by default (clan members)")]
            public bool DefaultEnableFF { get; set; }
            [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (clan members)")]
            public bool OwnerFF { get; set; }
            [JsonProperty(PropertyName = "Alliance Options")]
            public AllianceOptions Alliance { get; set; }
            [JsonProperty(PropertyName = "Invite Options")]
            public InviteOptions Invites { get; set; }
            [JsonProperty(PropertyName = "Rust Team Options")]
            public TeamOptions Teams { get; set; }
            public class AllianceOptions
            {
                [JsonProperty(PropertyName = "Enable clan alliances")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Alliance limit")]
                public int AllianceLimit { get; set; }
                [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
                public bool AllyFF { get; set; }
                [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
                public bool DefaultEnableFF { get; set; }
                [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
                public bool OwnerFF { get; set; }
            }

            public class InviteOptions
            {
                [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
                public int MemberInviteLimit { get; set; }
                [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
                public int MemberInviteExpireTime { get; set; }
                [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
                public int AllianceInviteLimit { get; set; }
                [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
                public int AllianceInviteExpireTime { get; set; }
            }

            public class TeamOptions
            {
                [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
                public bool AllowLeave { get; set; }
                [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
                public bool AllowKick { get; set; }
                [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
                public bool AllowInvite { get; set; }
                [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
                public bool AllowPromote { get; set; }
            }

        }

        public class ColorOptions
        {
            [JsonProperty(PropertyName = "Clan owner color (hex)")]
            public string Owner { get; set; }
            [JsonProperty(PropertyName = "Clan council color (hex)")]
            public string Council { get; set; }
            [JsonProperty(PropertyName = "Clan moderator color (hex)")]
            public string Moderator { get; set; }
            [JsonProperty(PropertyName = "Clan member color (hex)")]
            public string Member { get; set; }
            [JsonProperty(PropertyName = "General text color (hex)")]
            public string TextColor { get; set; }
        }

        public class TagOptions
        {
            [JsonProperty(PropertyName = "Enable clan tags (Display Name)")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Enable clan tags (BetterChat)")]
            public bool EnabledBC { get; set; }
            [JsonProperty(PropertyName = "Tag opening character")]
            public string TagOpen { get; set; }
            [JsonProperty(PropertyName = "Tag closing character")]
            public string TagClose { get; set; }
            [JsonProperty(PropertyName = "Tag color (hex) (BetterChat)")]
            public string TagColor { get; set; }
            [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
            public bool CustomColors { get; set; }
            [JsonProperty(PropertyName = "Custom tag color minimum value (hex)")]
            public string CustomTagColorMin { get; set; }
            [JsonProperty(PropertyName = "Custom tag color maximum value (hex)")]
            public string CustomTagColorMax { get; set; }
            [JsonProperty(PropertyName = "Blacklisted tag colors (hex without the # at the start)")]
            public string[] BlockedTagColors { get; set; }
            [JsonProperty(PropertyName = "Tag size (BetterChat)")]
            public int TagSize { get; set; }
            [JsonProperty(PropertyName = "Tag character limits")]
            public Range TagLength { get; set; }
            [JsonProperty(PropertyName = "Special characters allowed in tags")]
            public string AllowedCharacters { get; set; }
            [JsonProperty(PropertyName = "Words/characters not allowed in tags")]
            public string[] BlockedWords { get; set; }
        }

        public class CommandOptions
        {
            [JsonProperty(PropertyName = "Ally chat command")]
            public string AllyChatCommand { get; set; }
            [JsonProperty(PropertyName = "Clan chat command")]
            public string ClanChatCommand { get; set; }
            [JsonProperty(PropertyName = "Clan command")]
            public string ClanCommand { get; set; }
            [JsonProperty(PropertyName = "Clan info command")]
            public string ClanInfoCommand { get; set; }
            [JsonProperty(PropertyName = "Ally friendly fire command")]
            public string AFFCommand { get; set; }
            [JsonProperty(PropertyName = "Friendly fire command")]
            public string FFCommand { get; set; }
            [JsonProperty(PropertyName = "Clan ally command")]
            public string ClanAllyCommand { get; set; }
            [JsonProperty(PropertyName = "Clan help command")]
            public string ClanHelpCommand { get; set; }
            [JsonProperty(PropertyName = "Required auth-levels to use admin console command")]
            public AdminAuth Auth { get; set; }
            [JsonProperty(PropertyName = "Clan info options")]
            public ClanInfo ClanInfoOptions { get; set; }
            public class ClanInfo
            {
                [JsonProperty(PropertyName = "Show online/offline players")]
                public bool Players { get; set; }
                [JsonProperty(PropertyName = "Show clan alliances")]
                public bool Alliances { get; set; }
                [JsonProperty(PropertyName = "Show last online time")]
                public bool LastOnline { get; set; }
                [JsonProperty(PropertyName = "Show member count")]
                public bool MemberCount { get; set; }
                [JsonProperty(PropertyName = "Show alliance count")]
                public bool AllianceCount { get; set; }
            }

            public class AdminAuth
            {
                [JsonProperty(PropertyName = "Create clan")]
                public int Create { get; set; }
                [JsonProperty(PropertyName = "Rename clan")]
                public int Rename { get; set; }
                [JsonProperty(PropertyName = "Disband clan")]
                public int Disband { get; set; }
                [JsonProperty(PropertyName = "Invite member to clan")]
                public int Invite { get; set; }
                [JsonProperty(PropertyName = "Kick member from clan")]
                public int Kick { get; set; }
                [JsonProperty(PropertyName = "Promote/Demote member in clan")]
                public int Promote { get; set; }
            }

        }

        public class PermissionOptions
        {
            [JsonProperty(PropertyName = "Minimum auth level required to view clan info (0 = player, 1 = moderator, 2 = owner)")]
            public int ClanInfoAuthLevel { get; set; }
            [JsonProperty(PropertyName = "Use permission groups")]
            public bool PermissionGroups { get; set; }
            [JsonProperty(PropertyName = "Permission group prefix")]
            public string PermissionGroupPrefix { get; set; }
            [JsonProperty(PropertyName = "Use permission to create a clan")]
            public bool UsePermissionCreate { get; set; }
            [JsonProperty(PropertyName = "Clan creation permission")]
            public string PermissionCreate { get; set; }
            [JsonProperty(PropertyName = "Use permission to join a clan")]
            public bool UsePermissionJoin { get; set; }
            [JsonProperty(PropertyName = "Clan join permission")]
            public string PermissionJoin { get; set; }
            [JsonProperty(PropertyName = "Use permission to leave a clan")]
            public bool UsePermissionLeave { get; set; }
            [JsonProperty(PropertyName = "Clan leave permission")]
            public string PermissionLeave { get; set; }
            [JsonProperty(PropertyName = "Use permission to disband a clan")]
            public bool UsePermissionDisband { get; set; }
            [JsonProperty(PropertyName = "Clan disband permission")]
            public string PermissionDisband { get; set; }
            [JsonProperty(PropertyName = "Use permission to kick a clan member")]
            public bool UsePermissionKick { get; set; }
            [JsonProperty(PropertyName = "Clan kick permission")]
            public string PermissionKick { get; set; }
        }

        public class PurgeOptions
        {
            [JsonProperty(PropertyName = "Enable clan purging")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
            public int OlderThanDays { get; set; }
            [JsonProperty(PropertyName = "List purged clans in console when purging")]
            public bool ListPurgedClans { get; set; }
            [JsonProperty(PropertyName = "Wipe clans on new map save")]
            public bool WipeOnNewSave { get; set; }
        }

        public class OtherOptions
        {
            [JsonProperty(PropertyName = "Block clan/ally chat when muted")]
            public bool DenyOnMuted { get; set; }
            [JsonProperty(PropertyName = "Log clan and member changes")]
            public bool LogChanges { get; set; }
            [JsonProperty(PropertyName = "Use ProtoBuf data storage")]
            public bool UseProtoStorage { get; set; }
        }

        public class Range
        {
            public int Minimum { get; set; }
            public int Maximum { get; set; }
            public Range();
            public Range(int minimum, int maximum);
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    [ConsoleCommand("clans.convertdata")]
    private void ConvertFromOldData(ConsoleSystem.Arg arg);
    private void ConvertFromOldData(ConsoleSystem.Arg arg, OldDataStructure oldData);
    [Serializable, ProtoContract]
    internal class StoredData
    {
        [ProtoMember(1)]
        public Hash<string, Clan> clans;
        [ProtoMember(2)]
        public int timeSaved;
        [ProtoMember(3)]
        public Hash<ulong, List<string>> playerInvites;
        [JsonIgnore,ProtoIgnore]
        private Hash<ulong, string> playerLookup;
        [JsonIgnore, ProtoIgnore]
        private Hash<string, string> clanLookup;
        internal Clan FindClan(string tag);
        internal Clan FindClanByID(ulong playerId);
        internal Clan FindClanByID(string playerId);
        internal Clan.Member FindMemberByID(ulong playerId);
        internal void RegisterPlayer(ulong playerId, string tag);
        internal void UnregisterPlayer(ulong playerId);
        internal void AddPlayerInvite(ulong target, string tag);
        internal void RevokePlayerInvite(ulong target, string tag);
        internal void OnInviteAccepted(ulong target, string tag);
        internal void OnInviteRejected(ulong target, string tag);
    }

    private class OldDataStructure
    {
        public Dictionary<string, OldClan> clans;
        public int saveStamp;
        public string lastStorage;
    }

    public class OldClan
    {
        public string tag;
        public string description;
        public string owner;
        public string council;
        public int created;
        public int updated;
        public List<string> moderators;
        public List<string> members;
        public Dictionary<string, int> invites;
        public List<string> clanAlliances;
        public List<string> invitedAllies;
        public List<string> pendingInvites;
    }

    private static string msg(string key, string playerId);
     Dictionary<string, string> Messages;
    private Dictionary<string, string> leetTable;
}

public class TagEqualityComparer : IEqualityComparer<string>
{
    private static TagEqualityComparer _instance;
    public static TagEqualityComparer Instance { get; set; }
    public bool Equals(string a, string b);
    public int GetHashCode(string obj);
}

[Serializable, ProtoContract]
public class Clan
{
    [ProtoMember(1)]
    public string Tag { get; set; }
    [ProtoMember(2)]
    public string Description { get; set; }
    [ProtoMember(3)]
    public ulong OwnerID { get; set; }
    [ProtoMember(4)]
    public double CreationTime { get; set; }
    [ProtoMember(5)]
    public double LastOnlineTime { get; set; }
    [ProtoMember(6)]
    public Hash<ulong, Member> ClanMembers { get; set; }
    [ProtoMember(7)]
    public Hash<ulong, MemberInvite> MemberInvites { get; set; }
    [ProtoMember(8)]
    public HashSet<string> Alliances { get; set; }
    [ProtoMember(9)]
    public Hash<string, double> AllianceInvites { get; set; }
    [ProtoMember(10)]
    public HashSet<string> IncomingAlliances { get; set; }
    [ProtoMember(11)]
    public string TagColor { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int OnlineCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal ulong CouncilID { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int ModeratorCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int MemberCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int MemberInviteCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int AllianceCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int AllianceInviteCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    private RelationshipManager.PlayerTeam _playerTeam;
    [JsonIgnore, ProtoIgnore]
    internal RelationshipManager.PlayerTeam PlayerTeam { get; set; }
    private static ulong FindRandomTeamID { get; set; }
    public Clan();
    public Clan(BasePlayer player, string tag, string description);
    internal void OnPlayerConnected(BasePlayer player);
    internal void OnPlayerDisconnected(BasePlayer player);
    internal bool InvitePlayer(BasePlayer inviter, BasePlayer invitee);
    internal bool JoinClan(BasePlayer player);
    internal bool LeaveClan(BasePlayer player);
    internal bool KickMember(BasePlayer player, ulong targetId);
    internal bool PromotePlayer(BasePlayer promoter, ulong targetId);
    internal bool DemotePlayer(BasePlayer demoter, ulong targetId);
    internal void DisbandClan();
    internal void OnClanDisbanded(string tag);
    internal void OnUnload();
    internal bool IsAlliedClan(string otherClan);
    internal void MarkDirty();
    internal void Broadcast(string message);
    internal void Broadcast(string key, object[] args);
    [JsonIgnore, ProtoIgnore]
    private string cachedClanInfo;
    [JsonIgnore, ProtoIgnore]
    private string membersOnline;
    internal void PrintClanInfo(BasePlayer player);
    internal string GetMembersOnline();
    internal bool IsOwner(ulong playerId);
    internal bool IsCouncil(ulong playerId);
    internal bool IsModerator(ulong playerId);
    internal bool IsMember(ulong playerId);
    internal Member GetOwner();
    internal bool HasCouncil();
    internal string GetRoleColor(ulong userID);
    internal string GetRoleColor(Member.MemberRole role);
    [Serializable, ProtoContract]
    public class Member
    {
        [JsonIgnore, ProtoIgnore]
        public BasePlayer Player { get; set; }
        [ProtoMember(1)]
        public string DisplayName { get; set; }
        [ProtoMember(2)]
        public MemberRole Role { get; set; }
        [ProtoMember(3)]
        public bool MemberFFEnabled { get; set; }
        [ProtoMember(4)]
        public bool AllyFFEnabled { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal bool IsConnected { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal float lastFFAttackTime;
        [JsonIgnore, ProtoIgnore]
        internal float lastAFFAttackTime;
        public Member();
        public Member(MemberRole role, Clan clan);
        public Member(MemberRole role, bool memberFFEnabled, bool allyFFEnabled);
        public void OnClanMemberHit(string victimName);
        public void OnAllyMemberHit(string victimName);
    }

    [Serializable, ProtoContract]
    public class MemberInvite
    {
        [ProtoMember(1)]
        public string DisplayName { get; set; }
        [ProtoMember(2)]
        public double ExpiryTime { get; set; }
        public MemberInvite();
        public MemberInvite(BasePlayer player);
    }

    [JsonIgnore, ProtoIgnore]
    private JObject serializedClanObject;
    internal JObject ToJObject();
    internal ulong FindPlayer(string partialNameOrID);
}

[Serializable, ProtoContract]
public class Member
{
    [JsonIgnore, ProtoIgnore]
    public BasePlayer Player { get; set; }
    [ProtoMember(1)]
    public string DisplayName { get; set; }
    [ProtoMember(2)]
    public MemberRole Role { get; set; }
    [ProtoMember(3)]
    public bool MemberFFEnabled { get; set; }
    [ProtoMember(4)]
    public bool AllyFFEnabled { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal bool IsConnected { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal float lastFFAttackTime;
    [JsonIgnore, ProtoIgnore]
    internal float lastAFFAttackTime;
    public Member();
    public Member(MemberRole role, Clan clan);
    public Member(MemberRole role, bool memberFFEnabled, bool allyFFEnabled);
    public void OnClanMemberHit(string victimName);
    public void OnAllyMemberHit(string victimName);
}

[Serializable, ProtoContract]
public class MemberInvite
{
    [ProtoMember(1)]
    public string DisplayName { get; set; }
    [ProtoMember(2)]
    public double ExpiryTime { get; set; }
    public MemberInvite();
    public MemberInvite(BasePlayer player);
}

internal class ConfigData
{
    [JsonProperty(PropertyName = "Clan Options")]
    public ClanOptions Clans { get; set; }
    [JsonProperty(PropertyName = "Command Options")]
    public CommandOptions Commands { get; set; }
    [JsonProperty(PropertyName = "Role Colors")]
    public ColorOptions Colors { get; set; }
    [JsonProperty(PropertyName = "Clan Tag Options")]
    public TagOptions Tags { get; set; }
    [JsonProperty(PropertyName = "Permission Options")]
    public PermissionOptions Permissions { get; set; }
    [JsonProperty(PropertyName = "Purge Options")]
    public PurgeOptions Purge { get; set; }
    [JsonProperty(PropertyName = "Settings")]
    public OtherOptions Options { get; set; }
    public class ClanOptions
    {
        [JsonProperty(PropertyName = "Member limit")]
        public int MemberLimit { get; set; }
        [JsonProperty(PropertyName = "Moderator limit")]
        public int ModeratorLimit { get; set; }
        [JsonProperty(PropertyName = "Allow friendly fire toggle (clan members)")]
        public bool MemberFF { get; set; }
        [JsonProperty(PropertyName = "Enable friendly fire by default (clan members)")]
        public bool DefaultEnableFF { get; set; }
        [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (clan members)")]
        public bool OwnerFF { get; set; }
        [JsonProperty(PropertyName = "Alliance Options")]
        public AllianceOptions Alliance { get; set; }
        [JsonProperty(PropertyName = "Invite Options")]
        public InviteOptions Invites { get; set; }
        [JsonProperty(PropertyName = "Rust Team Options")]
        public TeamOptions Teams { get; set; }
        public class AllianceOptions
        {
            [JsonProperty(PropertyName = "Enable clan alliances")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Alliance limit")]
            public int AllianceLimit { get; set; }
            [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
            public bool AllyFF { get; set; }
            [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
            public bool DefaultEnableFF { get; set; }
            [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
            public bool OwnerFF { get; set; }
        }

        public class InviteOptions
        {
            [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
            public int MemberInviteLimit { get; set; }
            [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
            public int MemberInviteExpireTime { get; set; }
            [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
            public int AllianceInviteLimit { get; set; }
            [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
            public int AllianceInviteExpireTime { get; set; }
        }

        public class TeamOptions
        {
            [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
            public bool AllowLeave { get; set; }
            [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
            public bool AllowKick { get; set; }
            [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
            public bool AllowInvite { get; set; }
            [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
            public bool AllowPromote { get; set; }
        }

    }

    public class ColorOptions
    {
        [JsonProperty(PropertyName = "Clan owner color (hex)")]
        public string Owner { get; set; }
        [JsonProperty(PropertyName = "Clan council color (hex)")]
        public string Council { get; set; }
        [JsonProperty(PropertyName = "Clan moderator color (hex)")]
        public string Moderator { get; set; }
        [JsonProperty(PropertyName = "Clan member color (hex)")]
        public string Member { get; set; }
        [JsonProperty(PropertyName = "General text color (hex)")]
        public string TextColor { get; set; }
    }

    public class TagOptions
    {
        [JsonProperty(PropertyName = "Enable clan tags (Display Name)")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Enable clan tags (BetterChat)")]
        public bool EnabledBC { get; set; }
        [JsonProperty(PropertyName = "Tag opening character")]
        public string TagOpen { get; set; }
        [JsonProperty(PropertyName = "Tag closing character")]
        public string TagClose { get; set; }
        [JsonProperty(PropertyName = "Tag color (hex) (BetterChat)")]
        public string TagColor { get; set; }
        [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
        public bool CustomColors { get; set; }
        [JsonProperty(PropertyName = "Custom tag color minimum value (hex)")]
        public string CustomTagColorMin { get; set; }
        [JsonProperty(PropertyName = "Custom tag color maximum value (hex)")]
        public string CustomTagColorMax { get; set; }
        [JsonProperty(PropertyName = "Blacklisted tag colors (hex without the # at the start)")]
        public string[] BlockedTagColors { get; set; }
        [JsonProperty(PropertyName = "Tag size (BetterChat)")]
        public int TagSize { get; set; }
        [JsonProperty(PropertyName = "Tag character limits")]
        public Range TagLength { get; set; }
        [JsonProperty(PropertyName = "Special characters allowed in tags")]
        public string AllowedCharacters { get; set; }
        [JsonProperty(PropertyName = "Words/characters not allowed in tags")]
        public string[] BlockedWords { get; set; }
    }

    public class CommandOptions
    {
        [JsonProperty(PropertyName = "Ally chat command")]
        public string AllyChatCommand { get; set; }
        [JsonProperty(PropertyName = "Clan chat command")]
        public string ClanChatCommand { get; set; }
        [JsonProperty(PropertyName = "Clan command")]
        public string ClanCommand { get; set; }
        [JsonProperty(PropertyName = "Clan info command")]
        public string ClanInfoCommand { get; set; }
        [JsonProperty(PropertyName = "Ally friendly fire command")]
        public string AFFCommand { get; set; }
        [JsonProperty(PropertyName = "Friendly fire command")]
        public string FFCommand { get; set; }
        [JsonProperty(PropertyName = "Clan ally command")]
        public string ClanAllyCommand { get; set; }
        [JsonProperty(PropertyName = "Clan help command")]
        public string ClanHelpCommand { get; set; }
        [JsonProperty(PropertyName = "Required auth-levels to use admin console command")]
        public AdminAuth Auth { get; set; }
        [JsonProperty(PropertyName = "Clan info options")]
        public ClanInfo ClanInfoOptions { get; set; }
        public class ClanInfo
        {
            [JsonProperty(PropertyName = "Show online/offline players")]
            public bool Players { get; set; }
            [JsonProperty(PropertyName = "Show clan alliances")]
            public bool Alliances { get; set; }
            [JsonProperty(PropertyName = "Show last online time")]
            public bool LastOnline { get; set; }
            [JsonProperty(PropertyName = "Show member count")]
            public bool MemberCount { get; set; }
            [JsonProperty(PropertyName = "Show alliance count")]
            public bool AllianceCount { get; set; }
        }

        public class AdminAuth
        {
            [JsonProperty(PropertyName = "Create clan")]
            public int Create { get; set; }
            [JsonProperty(PropertyName = "Rename clan")]
            public int Rename { get; set; }
            [JsonProperty(PropertyName = "Disband clan")]
            public int Disband { get; set; }
            [JsonProperty(PropertyName = "Invite member to clan")]
            public int Invite { get; set; }
            [JsonProperty(PropertyName = "Kick member from clan")]
            public int Kick { get; set; }
            [JsonProperty(PropertyName = "Promote/Demote member in clan")]
            public int Promote { get; set; }
        }

    }

    public class PermissionOptions
    {
        [JsonProperty(PropertyName = "Minimum auth level required to view clan info (0 = player, 1 = moderator, 2 = owner)")]
        public int ClanInfoAuthLevel { get; set; }
        [JsonProperty(PropertyName = "Use permission groups")]
        public bool PermissionGroups { get; set; }
        [JsonProperty(PropertyName = "Permission group prefix")]
        public string PermissionGroupPrefix { get; set; }
        [JsonProperty(PropertyName = "Use permission to create a clan")]
        public bool UsePermissionCreate { get; set; }
        [JsonProperty(PropertyName = "Clan creation permission")]
        public string PermissionCreate { get; set; }
        [JsonProperty(PropertyName = "Use permission to join a clan")]
        public bool UsePermissionJoin { get; set; }
        [JsonProperty(PropertyName = "Clan join permission")]
        public string PermissionJoin { get; set; }
        [JsonProperty(PropertyName = "Use permission to leave a clan")]
        public bool UsePermissionLeave { get; set; }
        [JsonProperty(PropertyName = "Clan leave permission")]
        public string PermissionLeave { get; set; }
        [JsonProperty(PropertyName = "Use permission to disband a clan")]
        public bool UsePermissionDisband { get; set; }
        [JsonProperty(PropertyName = "Clan disband permission")]
        public string PermissionDisband { get; set; }
        [JsonProperty(PropertyName = "Use permission to kick a clan member")]
        public bool UsePermissionKick { get; set; }
        [JsonProperty(PropertyName = "Clan kick permission")]
        public string PermissionKick { get; set; }
    }

    public class PurgeOptions
    {
        [JsonProperty(PropertyName = "Enable clan purging")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
        public int OlderThanDays { get; set; }
        [JsonProperty(PropertyName = "List purged clans in console when purging")]
        public bool ListPurgedClans { get; set; }
        [JsonProperty(PropertyName = "Wipe clans on new map save")]
        public bool WipeOnNewSave { get; set; }
    }

    public class OtherOptions
    {
        [JsonProperty(PropertyName = "Block clan/ally chat when muted")]
        public bool DenyOnMuted { get; set; }
        [JsonProperty(PropertyName = "Log clan and member changes")]
        public bool LogChanges { get; set; }
        [JsonProperty(PropertyName = "Use ProtoBuf data storage")]
        public bool UseProtoStorage { get; set; }
    }

    public class Range
    {
        public int Minimum { get; set; }
        public int Maximum { get; set; }
        public Range();
        public Range(int minimum, int maximum);
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class ClanOptions
{
    [JsonProperty(PropertyName = "Member limit")]
    public int MemberLimit { get; set; }
    [JsonProperty(PropertyName = "Moderator limit")]
    public int ModeratorLimit { get; set; }
    [JsonProperty(PropertyName = "Allow friendly fire toggle (clan members)")]
    public bool MemberFF { get; set; }
    [JsonProperty(PropertyName = "Enable friendly fire by default (clan members)")]
    public bool DefaultEnableFF { get; set; }
    [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (clan members)")]
    public bool OwnerFF { get; set; }
    [JsonProperty(PropertyName = "Alliance Options")]
    public AllianceOptions Alliance { get; set; }
    [JsonProperty(PropertyName = "Invite Options")]
    public InviteOptions Invites { get; set; }
    [JsonProperty(PropertyName = "Rust Team Options")]
    public TeamOptions Teams { get; set; }
    public class AllianceOptions
    {
        [JsonProperty(PropertyName = "Enable clan alliances")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Alliance limit")]
        public int AllianceLimit { get; set; }
        [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
        public bool AllyFF { get; set; }
        [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
        public bool DefaultEnableFF { get; set; }
        [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
        public bool OwnerFF { get; set; }
    }

    public class InviteOptions
    {
        [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
        public int MemberInviteLimit { get; set; }
        [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
        public int MemberInviteExpireTime { get; set; }
        [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
        public int AllianceInviteLimit { get; set; }
        [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
        public int AllianceInviteExpireTime { get; set; }
    }

    public class TeamOptions
    {
        [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
        public bool AllowLeave { get; set; }
        [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
        public bool AllowKick { get; set; }
        [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
        public bool AllowInvite { get; set; }
        [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
        public bool AllowPromote { get; set; }
    }

}

public class AllianceOptions
{
    [JsonProperty(PropertyName = "Enable clan alliances")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Alliance limit")]
    public int AllianceLimit { get; set; }
    [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
    public bool AllyFF { get; set; }
    [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
    public bool DefaultEnableFF { get; set; }
    [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
    public bool OwnerFF { get; set; }
}

public class InviteOptions
{
    [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
    public int MemberInviteLimit { get; set; }
    [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
    public int MemberInviteExpireTime { get; set; }
    [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
    public int AllianceInviteLimit { get; set; }
    [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
    public int AllianceInviteExpireTime { get; set; }
}

public class TeamOptions
{
    [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
    public bool AllowLeave { get; set; }
    [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
    public bool AllowKick { get; set; }
    [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
    public bool AllowInvite { get; set; }
    [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
    public bool AllowPromote { get; set; }
}

public class ColorOptions
{
    [JsonProperty(PropertyName = "Clan owner color (hex)")]
    public string Owner { get; set; }
    [JsonProperty(PropertyName = "Clan council color (hex)")]
    public string Council { get; set; }
    [JsonProperty(PropertyName = "Clan moderator color (hex)")]
    public string Moderator { get; set; }
    [JsonProperty(PropertyName = "Clan member color (hex)")]
    public string Member { get; set; }
    [JsonProperty(PropertyName = "General text color (hex)")]
    public string TextColor { get; set; }
}

public class TagOptions
{
    [JsonProperty(PropertyName = "Enable clan tags (Display Name)")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Enable clan tags (BetterChat)")]
    public bool EnabledBC { get; set; }
    [JsonProperty(PropertyName = "Tag opening character")]
    public string TagOpen { get; set; }
    [JsonProperty(PropertyName = "Tag closing character")]
    public string TagClose { get; set; }
    [JsonProperty(PropertyName = "Tag color (hex) (BetterChat)")]
    public string TagColor { get; set; }
    [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
    public bool CustomColors { get; set; }
    [JsonProperty(PropertyName = "Custom tag color minimum value (hex)")]
    public string CustomTagColorMin { get; set; }
    [JsonProperty(PropertyName = "Custom tag color maximum value (hex)")]
    public string CustomTagColorMax { get; set; }
    [JsonProperty(PropertyName = "Blacklisted tag colors (hex without the # at the start)")]
    public string[] BlockedTagColors { get; set; }
    [JsonProperty(PropertyName = "Tag size (BetterChat)")]
    public int TagSize { get; set; }
    [JsonProperty(PropertyName = "Tag character limits")]
    public Range TagLength { get; set; }
    [JsonProperty(PropertyName = "Special characters allowed in tags")]
    public string AllowedCharacters { get; set; }
    [JsonProperty(PropertyName = "Words/characters not allowed in tags")]
    public string[] BlockedWords { get; set; }
}

public class CommandOptions
{
    [JsonProperty(PropertyName = "Ally chat command")]
    public string AllyChatCommand { get; set; }
    [JsonProperty(PropertyName = "Clan chat command")]
    public string ClanChatCommand { get; set; }
    [JsonProperty(PropertyName = "Clan command")]
    public string ClanCommand { get; set; }
    [JsonProperty(PropertyName = "Clan info command")]
    public string ClanInfoCommand { get; set; }
    [JsonProperty(PropertyName = "Ally friendly fire command")]
    public string AFFCommand { get; set; }
    [JsonProperty(PropertyName = "Friendly fire command")]
    public string FFCommand { get; set; }
    [JsonProperty(PropertyName = "Clan ally command")]
    public string ClanAllyCommand { get; set; }
    [JsonProperty(PropertyName = "Clan help command")]
    public string ClanHelpCommand { get; set; }
    [JsonProperty(PropertyName = "Required auth-levels to use admin console command")]
    public AdminAuth Auth { get; set; }
    [JsonProperty(PropertyName = "Clan info options")]
    public ClanInfo ClanInfoOptions { get; set; }
    public class ClanInfo
    {
        [JsonProperty(PropertyName = "Show online/offline players")]
        public bool Players { get; set; }
        [JsonProperty(PropertyName = "Show clan alliances")]
        public bool Alliances { get; set; }
        [JsonProperty(PropertyName = "Show last online time")]
        public bool LastOnline { get; set; }
        [JsonProperty(PropertyName = "Show member count")]
        public bool MemberCount { get; set; }
        [JsonProperty(PropertyName = "Show alliance count")]
        public bool AllianceCount { get; set; }
    }

    public class AdminAuth
    {
        [JsonProperty(PropertyName = "Create clan")]
        public int Create { get; set; }
        [JsonProperty(PropertyName = "Rename clan")]
        public int Rename { get; set; }
        [JsonProperty(PropertyName = "Disband clan")]
        public int Disband { get; set; }
        [JsonProperty(PropertyName = "Invite member to clan")]
        public int Invite { get; set; }
        [JsonProperty(PropertyName = "Kick member from clan")]
        public int Kick { get; set; }
        [JsonProperty(PropertyName = "Promote/Demote member in clan")]
        public int Promote { get; set; }
    }

}

public class ClanInfo
{
    [JsonProperty(PropertyName = "Show online/offline players")]
    public bool Players { get; set; }
    [JsonProperty(PropertyName = "Show clan alliances")]
    public bool Alliances { get; set; }
    [JsonProperty(PropertyName = "Show last online time")]
    public bool LastOnline { get; set; }
    [JsonProperty(PropertyName = "Show member count")]
    public bool MemberCount { get; set; }
    [JsonProperty(PropertyName = "Show alliance count")]
    public bool AllianceCount { get; set; }
}

public class AdminAuth
{
    [JsonProperty(PropertyName = "Create clan")]
    public int Create { get; set; }
    [JsonProperty(PropertyName = "Rename clan")]
    public int Rename { get; set; }
    [JsonProperty(PropertyName = "Disband clan")]
    public int Disband { get; set; }
    [JsonProperty(PropertyName = "Invite member to clan")]
    public int Invite { get; set; }
    [JsonProperty(PropertyName = "Kick member from clan")]
    public int Kick { get; set; }
    [JsonProperty(PropertyName = "Promote/Demote member in clan")]
    public int Promote { get; set; }
}

public class PermissionOptions
{
    [JsonProperty(PropertyName = "Minimum auth level required to view clan info (0 = player, 1 = moderator, 2 = owner)")]
    public int ClanInfoAuthLevel { get; set; }
    [JsonProperty(PropertyName = "Use permission groups")]
    public bool PermissionGroups { get; set; }
    [JsonProperty(PropertyName = "Permission group prefix")]
    public string PermissionGroupPrefix { get; set; }
    [JsonProperty(PropertyName = "Use permission to create a clan")]
    public bool UsePermissionCreate { get; set; }
    [JsonProperty(PropertyName = "Clan creation permission")]
    public string PermissionCreate { get; set; }
    [JsonProperty(PropertyName = "Use permission to join a clan")]
    public bool UsePermissionJoin { get; set; }
    [JsonProperty(PropertyName = "Clan join permission")]
    public string PermissionJoin { get; set; }
    [JsonProperty(PropertyName = "Use permission to leave a clan")]
    public bool UsePermissionLeave { get; set; }
    [JsonProperty(PropertyName = "Clan leave permission")]
    public string PermissionLeave { get; set; }
    [JsonProperty(PropertyName = "Use permission to disband a clan")]
    public bool UsePermissionDisband { get; set; }
    [JsonProperty(PropertyName = "Clan disband permission")]
    public string PermissionDisband { get; set; }
    [JsonProperty(PropertyName = "Use permission to kick a clan member")]
    public bool UsePermissionKick { get; set; }
    [JsonProperty(PropertyName = "Clan kick permission")]
    public string PermissionKick { get; set; }
}

public class PurgeOptions
{
    [JsonProperty(PropertyName = "Enable clan purging")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
    public int OlderThanDays { get; set; }
    [JsonProperty(PropertyName = "List purged clans in console when purging")]
    public bool ListPurgedClans { get; set; }
    [JsonProperty(PropertyName = "Wipe clans on new map save")]
    public bool WipeOnNewSave { get; set; }
}

public class OtherOptions
{
    [JsonProperty(PropertyName = "Block clan/ally chat when muted")]
    public bool DenyOnMuted { get; set; }
    [JsonProperty(PropertyName = "Log clan and member changes")]
    public bool LogChanges { get; set; }
    [JsonProperty(PropertyName = "Use ProtoBuf data storage")]
    public bool UseProtoStorage { get; set; }
}

public class Range
{
    public int Minimum { get; set; }
    public int Maximum { get; set; }
    public Range();
    public Range(int minimum, int maximum);
}

[Serializable, ProtoContract]
internal class StoredData
{
    [ProtoMember(1)]
    public Hash<string, Clan> clans;
    [ProtoMember(2)]
    public int timeSaved;
    [ProtoMember(3)]
    public Hash<ulong, List<string>> playerInvites;
    [JsonIgnore,ProtoIgnore]
    private Hash<ulong, string> playerLookup;
    [JsonIgnore, ProtoIgnore]
    private Hash<string, string> clanLookup;
    internal Clan FindClan(string tag);
    internal Clan FindClanByID(ulong playerId);
    internal Clan FindClanByID(string playerId);
    internal Clan.Member FindMemberByID(ulong playerId);
    internal void RegisterPlayer(ulong playerId, string tag);
    internal void UnregisterPlayer(ulong playerId);
    internal void AddPlayerInvite(ulong target, string tag);
    internal void RevokePlayerInvite(ulong target, string tag);
    internal void OnInviteAccepted(ulong target, string tag);
    internal void OnInviteRejected(ulong target, string tag);
}

private class OldDataStructure
{
    public Dictionary<string, OldClan> clans;
    public int saveStamp;
    public string lastStorage;
}

public class OldClan
{
    public string tag;
    public string description;
    public string owner;
    public string council;
    public int created;
    public int updated;
    public List<string> moderators;
    public List<string> members;
    public Dictionary<string, int> invites;
    public List<string> clanAlliances;
    public List<string> invitedAllies;
    public List<string> pendingInvites;
}


```

---

## Clans (10)

```csharp
using Facepunch.Extend;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Clans", "OxideBro - RustPlugin.ru", "1.0.3", ResourceId = 14)]
public class Clans : RustPlugin
{
     bool Changed;
     bool Initialized;
    internal static Clans cc;
     bool newSaveDetected;
     List<ulong> manuallyEnabledBy;
     HashSet<ulong> bypass;
     Dictionary<string, DateTime> notificationTimes;
    static readonly DateTime UnixEpoch;
    static readonly double MaxUnixSeconds;
     Library lib;
    public Dictionary<string, Clan> clans;
    public Dictionary<string, string> clansSearch;
     List<string> purgedClans;
     Dictionary<string, string> originalNames;
     Dictionary<string, List<string>> pendingPlayerInvites;
     Regex tagReExt;
     Dictionary<string, Clan> clanCache;
     List<object> filterDefaults();
    private void SaveConf();
    private static string r(string i);
     Dictionary<string, object> rewardsDefaults();
     Dictionary<string, object> rewardsTranslateDefault();
    public int limitMembers;
     int limitModerators;
    public int limitAlliances;
     int tagLengthMin;
     int tagLengthMax;
     int inviteValidDays;
     int friendlyFireNotifyTimeout;
     string allowedSpecialChars;
    public bool enableFFOPtion;
     bool enableAllyFFOPtion;
     bool enableWordFilter;
     bool enableClanTagging;
    public bool enableClanAllies;
     bool forceAllyFFNoDeactivate;
     bool forceClanFFNoDeactivate;
     bool enableWhoIsOnlineMsg;
     bool enableComesOnlineMsg;
     int authLevelRename;
     int authLevelDelete;
     int authLevelInvite;
     int authLevelKick;
     int authLevelPromoteDemote;
     int authLevelClanInfo;
     bool purgeOldClans;
     int notUpdatedSinceDays;
     bool listPurgedClans;
     bool wipeClansOnNewSave;
     bool useProtostorageClandata;
     string consoleName;
     string broadcastPrefix;
     string broadcastPrefixAlly;
     string broadcastPrefixColor;
     string broadcastPrefixFormat;
     string broadcastMessageColor;
     string colorCmdUsage;
     string colorTextMsg;
     string colorClanNamesOverview;
     string colorClanFFOff;
     string colorClanFFOn;
     string pluginPrefix;
     string pluginPrefixColor;
     string pluginPrefixREBORNColor;
     bool pluginPrefixREBORNShow;
     string pluginPrefixFormat;
     string clanServerColor;
     string clanOwnerColor;
     string clanCouncilColor;
     string clanModeratorColor;
     string clanMemberColor;
     bool setHomeOwner;
     bool setHomeModerator;
     bool setHomeMember;
     string chatCommandClan;
     string chatCommandFF;
     string chatCommandAllyChat;
     string chatCommandClanChat;
     string chatCommandClanInfo;
     string subCommandClanHelp;
     string subCommandClanAlly;
     bool usePermGroups;
     string permGroupPrefix;
     bool usePermToCreateClan;
     string permissionToCreateClan;
     bool usePermToJoinClan;
     string permissionToJoinClan;
     bool addClanMembersAsIOFriends;
     string clanTagColorBetterChat;
     int clanTagSizeBetterChat;
     string clanTagOpening;
     string clanTagClosing;
     bool clanChatDenyOnMuted;
     List<string> activeRadarUsers;
     Dictionary<string, List<BasePlayer>> clanRadarMemberobjects;
    static Vector3 sleeperHeight;
    static Vector3 playerHeight;
     bool enableClanRadar;
     string colorClanRadarOff;
     string colorClanRadarOn;
     float refreshTime;
     string nameColor;
     string sleeperNameColor;
     string distanceColor;
    static float minDistance;
    static float maxNamedistance;
    static float maxSleeperDistance;
     bool showSleepers;
     bool extendOnAllyMembers;
     bool enableAtLogin;
     int radarTextSize;
     string permissionClanRadarUse;
     bool usePermissionClanRadar;
     string chatCommandRadar;
    private bool forceNametagsOnTagging;
    public static bool useRelationshipManager;
    private bool teamUiWasDisabled;
    private bool useRankColorsPanel;
    private bool disableManageFunctions;
    private bool allowButtonLeave;
    private bool allowButtonKick;
    private bool allowDirectInvite;
    private bool allowPromoteLeader;
     List<object> wordFilter;
     Dictionary<string, object> RewardGather;
     Dictionary<string, object> RewardTranslate;
     object GetConfig(string menu, string datavalue, object defaultValue);
     int PointsOfDeath;
     int PointsOfKilled;
     int PointsOfKilledHeli;
     int PointsOfKilledTank;
     int PointsOfSuicide;
     int PointsOfBarrel;
     int PointsOfRocket;
     int PointsOfGatherSulfur;
     int PointsOfGatherMetalOre;
     int PointsOfGatherStone;
     int PointsOfGatherWood;
     int PointsOfGatherHQM;
     void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void Init();
     void OnPluginLoaded(Plugin plugin);
     string getFormattedClanTag(IPlayer player);
    [PluginReference]
    private Plugin ImageLibrary;
    public string GetImageSkin(string shortname, ulong skin);
    public List<ulong> GetImageSkins(string shortname);
    static Clans ins;
     void OnServerInitialized();
    private IEnumerator ServerInitialized(object obj);
    private object OnServerCommand(ConsoleSystem.Arg arg);
     void OnServerSave();
     void OnNewSave();
     void Unload();
    private void DoCleanUp(BasePlayer player);
    private object LoadData();
     void InitializeClans(bool newFileFound);
     void SaveData(bool force);
    public Clan findClan(string tag);
    public Clan findClanByUser(string userId);
    private Clan SetupPlayer(BasePlayer player, IPlayer current, bool hasLeft, Clan clan, bool teamForced, string oldTag);
    private bool NullClanTeam(BasePlayer player);
     void setupPlayers(List<string> playerIds, bool remove, string tag);
    private void OnPlayerConnected(BasePlayer player);
     IEnumerator WaitForReady(BasePlayer player, Clan clan);
     void ComingOnlineInfo(BasePlayer player, Clan clan);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerAttack(BasePlayer attacker, HitInfo hit);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hit);
     object OnAttackShared(BasePlayer attacker, BasePlayer victim, HitInfo hit);
     void AllyRemovalCheck();
     void cmdChatClan(BasePlayer player, string command, string[] args);
    public string MainLayer;
     void NewClanUI(BasePlayer player, bool leadermode);
    [ConsoleCommand("UI_CLAN")]
     void cmdUIClan(ConsoleSystem.Arg args);
     void CreatePlayerInfo(BasePlayer player, Clan clan, PlayerStats stats, string userID);
    [ConsoleCommand("clans_authplayers")]
     void cmdAuthPlayerInClan(ConsoleSystem.Arg args);
     void cmdClanOverview(BasePlayer player);
     void cmdClanCreate(BasePlayer player, string[] args);
    public void InvitePlayer(BasePlayer player, string targetId);
     void cmdClanInvite(BasePlayer player, string[] args);
     string ButtonListed;
    private string GetImageUrl(string shortname, ulong skinid);
    private void AddLoadOrder(IDictionary<string, string> imageList, bool replace);
    private string GetImage(string shortname, ulong skinid);
     bool? CanWearItem(PlayerInventory inventory, Item item, int targetPos);
    private bool? CanEquipItem(PlayerInventory inventory, Item item, int targetPos);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item item);
    static readonly DateTime epoch;
    static double CurrentTime();
    private double IsBlocked();
    private void SmeltOre(BasePlayer player, Item item, bool bonus);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
    [ConsoleCommand("clans_getskinIds")]
     void cmdClansGetSkinList(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_setChange")]
     void cmdSetChangeOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_Change")]
     void cmdSetNewChangeOfClan(ConsoleSystem.Arg args);
     class Position
    {
        public float Xmin;
        public float Xmax;
        public float Ymin;
        public float Ymax;
        public string AnchorMin { get; set; }
        public string AnchorMax { get; set; }
        public override string ToString();
    }

    [SuppressMessage("ReSharper", "CompareOfFloatsByEqualityOperator")]
    private static List<Position> GetPositions(int colums, int rows, float colPadding, float rowPadding, bool columsFirst);
    [ConsoleCommand("clan_changeskin")]
     void cmdChatSkinOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_kickplayer")]
     void cmdKickOfClan(ConsoleSystem.Arg args);
    public List<uint> IgnoreList;
     void OnItemDropped(Item item, BaseEntity entity);
    private void CopySeenPlayers(Item @from, Item to);
     void CanStackItem(Item stack, Item item);
    private void SetSeenPlayer(ulong ownerId, Item item);
    private Dictionary<uint, HashSet<ulong>> _looters;
    private bool HasSeenPlayer(ulong ownerId, Item item);
    [ConsoleCommand("clan_setAvatar")]
     void cmdSetAvatarOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("clan_changeAvatar")]
     void cmdChangeAvatarOfClan(ConsoleSystem.Arg args);
    private Dictionary<uint, ulong> LastHeliHit;
    private BasePlayer GetLastHeliAttacker(uint heliNetId);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private bool IsNPC(BasePlayer player);
     int GetPercent(int need, int current);
     double GetPercentFUll(double need, double current);
     string ButtonListedClan;
     string TOP;
     string InfoTOP;
     string InfoTOPButton;
    [ConsoleCommand("clanstop_info")]
     void cmdClansTopKey(ConsoleSystem.Arg arg);
    [ConsoleCommand("clanstop_main")]
     void cmdClansTopMain(ConsoleSystem.Arg arg);
    [ChatCommand("ctop")]
     void cmdClanTOP(BasePlayer player, string command, string[] args);
    public int GetClanIndex(string key);
     string GetPlayerStatus(string player, Clan clan);
     void ClanTOPInfo(BasePlayer player, Clan clan, int page);
    [PluginReference]
    private Plugin Tournament;
     void ClanTOP(BasePlayer player, int page);
     long GetFullPercent(string id);
     long GetFullClanPercent(string tag);
    public static string FormatShortTime(TimeSpan time);
    public void WithdrawPlayer(BasePlayer player, string targetId);
     void cmdClanWithdraw(BasePlayer player, string[] args);
     void cmdClanJoin(BasePlayer player, string[] args);
    [ConsoleCommand("clanui_promote")]
     void cmdClanPromoteMember(ConsoleSystem.Arg args);
    public void PromotePlayer(BasePlayer player, string targetId);
     void cmdClanPromote(BasePlayer player, string[] args);
    public void DemotePlayer(BasePlayer player, string targetId);
     void cmdClanDemote(BasePlayer player, string[] args);
    public void LeaveClan(BasePlayer player);
     void cmdClanLeave(BasePlayer player, string[] args);
    public void KickPlayer(BasePlayer player, string targetId);
     void cmdClanKick(BasePlayer player, string[] args);
    public void DisbandClan(BasePlayer player);
     void cmdClanDisband(BasePlayer player, string[] args);
    public void Alliance(BasePlayer player, string targetClan, string type);
     void cmdChatClanAlly(BasePlayer player, string command, string[] args);
     void cmdChatClanHelp(BasePlayer player, string command, string[] args);
     void cmdChatClanInfo(BasePlayer player, string command, string[] args);
     void cmdChatClanchat(BasePlayer player, string command, string[] args);
     void cmdChatAllychat(BasePlayer player, string command, string[] args);
     void cmdChatClanFF(BasePlayer player, string command, string[] args);
    public bool HasFFEnabled(ulong playerid);
    public void ToggleFF(ulong playerId);
     void cmdChatClanRadar(BasePlayer player, string command, string[] args);
    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
     class StoredData
    {
        public Dictionary<string, Clan> clans;
        public Int32 saveStamp;
        public string lastStorage;
        public StoredData();
    }

     StoredData clanSaves;
    public class ChangeListed
    {
        public int Need { get; set; }
        public int Complete { get; set; }
    }

    public class MembersChangeList
    {
        public int Complete { get; set; }
    }

    public class PlayerStats
    {
        public int PlayerPoints;
        public int Killed;
        public int Death;
        public int Suicide;
        public int KilledHeli;
        public int KilledTank;
        public bool CupAuth;
        public bool CodeAuth;
        public bool TurretAuth;
        public Dictionary<string, int> GatherInfo;
    }

    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
    public class Clan
    {
        public int ClanPoints;
        public string tag;
        public string description;
        public string owner;
        public string ownerName;
        public string ClanAvatar;
        public string council;
        public int created;
        public int updated;
        [JsonIgnore, ProtoIgnore]
        public int online;
        [JsonIgnore, ProtoIgnore]
        public int total;
        [JsonIgnore, ProtoIgnore]
        public int mods;
        public List<string> moderators;
        public Dictionary<string, PlayerStats> members;
        public Dictionary<string, int> invites;
        public List<string> clanAlliances;
        public List<string> invitedAllies;
        public List<string> pendingInvites;
        [JsonIgnore]
        [ProtoIgnore]
        private string currentTeamLeader { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        private bool wasDisbanded;
        [JsonIgnore]
        [ProtoIgnore]
        private RelationshipManager.PlayerTeam _playerTeam;
        [JsonIgnore]
        [ProtoIgnore]
        public RelationshipManager.PlayerTeam PlayerTeam { get; set; }
        public void RemoveModerator(object obj);
        private string GetObjectId(object obj);
        internal void OnCreate();
        public static int UnixTimeStampUTC();
        internal void OnUpdate(bool hasChanges);
        private static DateTime Epoch;
        public void DisbandTeam();
        internal void OnDestroy();
        internal void OnUnload();
        internal void UpdateTeam();
         void UpdateOnline();
        private void DestroyPlayerTeam();
        public string GetColoredName(string Id, string Name);
        public IPlayer FindClanMember(string nameOrId);
        [JsonIgnore]
        [ProtoIgnore]
        public List<BasePlayer> membersBasePlayer;
        public void AddBasePlayer(BasePlayer basePlayer, bool flag);
        public void RemoveBasePlayer(BasePlayer basePlayer, bool flag);
        public BasePlayer GetBasePlayer(string Id);
        public void SetModerator(object obj);
        public Dictionary<string, Dictionary<string, ulong>> SkinList;
        public Dictionary<string, ChangeListed> Change;
        public PlayerStats GetPlayerStats(string playerid);
        public static Clan Create(string tag, string description, string ownerId, string owName, string URL);
        public bool IsOwner(string userId);
        public bool IsCouncil(string userId);
        public bool IsModerator(string userId);
        public bool IsMember(string userId);
        public bool IsInvited(string userId);
        public void BroadcastChat(string message);
        public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
        public void AllyBroadcastChat(string message);
        public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
        public string ColNam(string Id, string Name);
        public string PlayerLevel(string userID);
        public string PlayerColor(string userID);
        public IPlayer GetIPlayer(string partialName);
        public IPlayer GetIMember(string partialName);
        public List<IPlayer> GetIMembers();
        public List<IPlayer> GetInvites();
        internal JObject ToJObject();
    }

    sealed class ClanRadar : FacepunchBehaviour
    {
         BasePlayer player;
         Clan clan;
         bool noAdmin;
         void Awake();
        public void DoStart();
         void SetPlayerFlag(BasePlayer.PlayerFlags f, bool b);
         void DoRadar();
         void DoDestroy();
         void OnDestroy();
    }

     void RemoveRadar(BasePlayer player, string tag, bool leftOrKicked);
     void RemoveRadarGroup(List<string> playerIds, string tag, bool isDisband);
    [HookMethod("GetClan")]
    private JObject GetClan(string tag);
    [HookMethod("GetAllClans")]
    private JArray GetAllClans();
    [HookMethod("GetClanOf")]
    private string GetClanOf(ulong player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(string player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(BasePlayer player);
    [HookMethod("GetClanMembers")]
    private List<string> GetClanMembers(ulong PlayerID);
     object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
     void OnEntityBuilt(Planner plan, GameObject go);
    private void AutOnBuildingPrivilage(BasePlayer player, Clan clan, Action<T, string> callback);
    private static List<T> GetPlayerEnitityByType(BasePlayer player);
     object OnTurretTarget(AutoTurret turret, BasePlayer player);
    private bool CheckClans(ulong TurretID, ulong TargetID);
     string ClanAlready(ulong ownerid);
     void ScoreRemove(ulong targetclan, ulong acceptclan);
     object ClanCount(ulong owner);
    [HookMethod("HasFriend")]
    private object HasFriend(ulong entOwnerID, ulong PlayerUserID);
    [HookMethod("IsModerator")]
    private object IsModerator(ulong PlayerUserID);
    private Int32 UnixTimeStampUTC();
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
     string msg(string key, string id);
     void PrintChat(BasePlayer player, string message);
    [ConsoleCommand("clans")]
     void cclans(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.cmds")]
     void cclansCommands(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.list")]
     void cclansList(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.showduplicates")]
     void cclansDuplicates(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.listex")]
     void cclansListEx(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.show")]
     void cclansShow(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.msg")]
     void cclansBroadcast(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.rename")]
     void cclansRename(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerinvite")]
     void cclansPlayerInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerjoin")]
     void cclansPlayerJoin(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerkick")]
     void cclansPlayerKick(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.changeowner")]
     void cclansChangeOwner(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerpromote")]
     void cclansPlayerPromote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.playerdemote")]
     void cclansPlayerDemote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.delete")]
     void cclansDelete(ConsoleSystem.Arg arg);
     bool FilterText(string tag);
     string TranslateLeet(string original);
     bool TryGetClan(string input, Clan clan);
     void RemoveClan(string tag);
    [HookMethod("EnableBypass")]
     void EnableBypass(object userId);
    [HookMethod("DisableBypass")]
     void DisableBypass(object userId);
}

 class Position
{
    public float Xmin;
    public float Xmax;
    public float Ymin;
    public float Ymax;
    public string AnchorMin { get; set; }
    public string AnchorMax { get; set; }
    public override string ToString();
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
 class StoredData
{
    public Dictionary<string, Clan> clans;
    public Int32 saveStamp;
    public string lastStorage;
    public StoredData();
}

public class ChangeListed
{
    public int Need { get; set; }
    public int Complete { get; set; }
}

public class MembersChangeList
{
    public int Complete { get; set; }
}

public class PlayerStats
{
    public int PlayerPoints;
    public int Killed;
    public int Death;
    public int Suicide;
    public int KilledHeli;
    public int KilledTank;
    public bool CupAuth;
    public bool CodeAuth;
    public bool TurretAuth;
    public Dictionary<string, int> GatherInfo;
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
public class Clan
{
    public int ClanPoints;
    public string tag;
    public string description;
    public string owner;
    public string ownerName;
    public string ClanAvatar;
    public string council;
    public int created;
    public int updated;
    [JsonIgnore, ProtoIgnore]
    public int online;
    [JsonIgnore, ProtoIgnore]
    public int total;
    [JsonIgnore, ProtoIgnore]
    public int mods;
    public List<string> moderators;
    public Dictionary<string, PlayerStats> members;
    public Dictionary<string, int> invites;
    public List<string> clanAlliances;
    public List<string> invitedAllies;
    public List<string> pendingInvites;
    [JsonIgnore]
    [ProtoIgnore]
    private string currentTeamLeader { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    private bool wasDisbanded;
    [JsonIgnore]
    [ProtoIgnore]
    private RelationshipManager.PlayerTeam _playerTeam;
    [JsonIgnore]
    [ProtoIgnore]
    public RelationshipManager.PlayerTeam PlayerTeam { get; set; }
    public void RemoveModerator(object obj);
    private string GetObjectId(object obj);
    internal void OnCreate();
    public static int UnixTimeStampUTC();
    internal void OnUpdate(bool hasChanges);
    private static DateTime Epoch;
    public void DisbandTeam();
    internal void OnDestroy();
    internal void OnUnload();
    internal void UpdateTeam();
     void UpdateOnline();
    private void DestroyPlayerTeam();
    public string GetColoredName(string Id, string Name);
    public IPlayer FindClanMember(string nameOrId);
    [JsonIgnore]
    [ProtoIgnore]
    public List<BasePlayer> membersBasePlayer;
    public void AddBasePlayer(BasePlayer basePlayer, bool flag);
    public void RemoveBasePlayer(BasePlayer basePlayer, bool flag);
    public BasePlayer GetBasePlayer(string Id);
    public void SetModerator(object obj);
    public Dictionary<string, Dictionary<string, ulong>> SkinList;
    public Dictionary<string, ChangeListed> Change;
    public PlayerStats GetPlayerStats(string playerid);
    public static Clan Create(string tag, string description, string ownerId, string owName, string URL);
    public bool IsOwner(string userId);
    public bool IsCouncil(string userId);
    public bool IsModerator(string userId);
    public bool IsMember(string userId);
    public bool IsInvited(string userId);
    public void BroadcastChat(string message);
    public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
    public void AllyBroadcastChat(string message);
    public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
    public string ColNam(string Id, string Name);
    public string PlayerLevel(string userID);
    public string PlayerColor(string userID);
    public IPlayer GetIPlayer(string partialName);
    public IPlayer GetIMember(string partialName);
    public List<IPlayer> GetIMembers();
    public List<IPlayer> GetInvites();
    internal JObject ToJObject();
}

sealed class ClanRadar : FacepunchBehaviour
{
     BasePlayer player;
     Clan clan;
     bool noAdmin;
     void Awake();
    public void DoStart();
     void SetPlayerFlag(BasePlayer.PlayerFlags f, bool b);
     void DoRadar();
     void DoDestroy();
     void OnDestroy();
}


```

---

## Clans (2)

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text;
using System.Linq;
using System.Text.RegularExpressions;
using UnityEngine;
using Rust;
using ProtoBuf;
using Facepunch.Extend;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Libraries.Covalence;

Oxide.Plugins
[Info("Clans", "FuJiCuRa", "2.14.4", ResourceId = 14)]
[Description("Clans plugin with Allies, inbuilt FriendlyFire and much more...")]
public class Clans : RustPlugin
{
    private bool Changed;
    private bool Initialized;
    private static Clans cc;
    private bool newSaveDetected;
    private List<ulong> manuallyEnabledBy;
    private HashSet<ulong> bypass;
    private Dictionary<string, DateTime> notificationTimes;
    private List<int> creationTimes;
    private static DateTime Epoch;
    private static double MaxUnixSeconds;
    public Dictionary<string, Clan> clans;
    public Dictionary<string, string> clansSearch;
    private List<string> purgedClans;
    private Dictionary<string, List<string>> pendingPlayerInvites;
    private Regex tagReExt;
    private Dictionary<string, Clan> clanCache;
    private List<object> filterDefaults();
    public int limitMembers;
    private int limitModerators;
    public int limitAlliances;
    private int tagLengthMin;
    private int tagLengthMax;
    private int inviteValidDays;
    private int friendlyFireNotifyTimeout;
    private string allowedSpecialChars;
    public bool enableFFOPtion;
    private bool enableAllyFFOPtion;
    private bool enableWordFilter;
    private bool enableClanTagging;
    public bool enableClanAllies;
    private bool forceAllyFFNoDeactivate;
    private bool forceClanFFNoDeactivate;
    private bool enableWhoIsOnlineMsg;
    private bool enableComesOnlineMsg;
    private bool forceNametagsOnTagging;
    private int authLevelRename;
    private int authLevelDisband;
    private int authLevelInvite;
    private int authLevelKick;
    private int authLevelCreate;
    private int authLevelPromoteDemote;
    private int authLevelClanInfo;
    private bool purgeOldClans;
    private int notUpdatedSinceDays;
    private bool listPurgedClans;
    private bool wipeClansOnNewSave;
    private bool useProtostorageClandata;
    private string consoleName;
    private string broadcastPrefix;
    private string broadcastPrefixAlly;
    private string broadcastPrefixColor;
    private string broadcastPrefixFormat;
    private string broadcastMessageColor;
    private string colorCmdUsage;
    private string colorTextMsg;
    private string colorClanNamesOverview;
    private string colorClanFFOff;
    private string colorClanFFOn;
    private string pluginPrefix;
    private string pluginPrefixColor;
    private string pluginPrefixREBORNColor;
    private bool pluginPrefixREBORNShow;
    private string pluginPrefixFormat;
    private string clanServerColor;
    private string clanOwnerColor;
    private string clanCouncilColor;
    private string clanModeratorColor;
    private string clanMemberColor;
    private bool setHomeOwner;
    private bool setHomeModerator;
    private bool setHomeMember;
    private string chatCommandClan;
    private string chatCommandFF;
    private string chatCommandAllyChat;
    private string chatCommandClanChat;
    private string chatCommandClanInfo;
    private string subCommandClanHelp;
    private string subCommandClanAlly;
    private bool usePermGroups;
    private string permGroupPrefix;
    private bool usePermToCreateClan;
    private string permissionToCreateClan;
    private bool usePermToJoinClan;
    private string permissionToJoinClan;
    private string clanTagColorBetterChat;
    private int clanTagSizeBetterChat;
    private string clanTagOpening;
    private string clanTagClosing;
    private bool clanChatDenyOnMuted;
    public static bool useRelationshipManager;
    private bool teamUiWasDisabled;
    private bool useRankColorsPanel;
    private bool disableManageFunctions;
    private bool allowButtonLeave;
    private bool allowButtonKick;
    private bool allowDirectInvite;
    private bool allowPromoteLeader;
    private bool logClanChanges;
    private List<object> wordFilter;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void Loaded();
    private void OnServerSave();
    private void OnNewSave();
    private void Unload();
    private void OnServerInitialized();
    private IEnumerator ServerInitialized(object obj);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void SaveConf();
    private static string r(string i);
    private object LoadData();
    private void InitializeClans(bool newFileFound);
    public static int TakeCreatedTime(int stamp);
    private void SaveData(bool force);
    public Clan findClan(string tag);
    public Clan findClanByUser(string userId);
    private Clan SetupPlayer(BasePlayer player, IPlayer current, bool hasLeft, Clan clan, bool teamForced, string oldTag);
    private bool NullClanTeam(BasePlayer player);
    private void DoCleanUp(BasePlayer player);
    private void setupPlayers(List<string> playerIds, bool isDisband, Clan oldClan, string tag);
    private void OnPlayerInit(BasePlayer player);
    private IEnumerator WaitForReady(BasePlayer player, Clan clan);
    private void ComingOnlineInfo(BasePlayer player, Clan clan);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hit);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hit);
    private object OnAttackShared(BasePlayer attacker, BasePlayer victim, HitInfo hit);
    private void AllyRemovalCheck();
    private void ccmdChatClan(ConsoleSystem.Arg arg);
    private void cmdChatClan(BasePlayer player, string command, string[] args);
    private void cmdClanOverview(BasePlayer player);
    private void cmdClanCreate(BasePlayer player, string[] args);
    public void InvitePlayer(BasePlayer player, string targetId);
    private void cmdClanInvite(BasePlayer player, string[] args);
    public void WithdrawPlayer(BasePlayer player, string targetId);
    private void cmdClanWithdraw(BasePlayer player, string[] args);
    private void cmdClanJoin(BasePlayer player, string[] args);
    public void PromotePlayer(BasePlayer player, string targetId);
    private void cmdClanPromote(BasePlayer player, string[] args);
    public void DemotePlayer(BasePlayer player, string targetId);
    private void cmdClanDemote(BasePlayer player, string[] args);
    public void LeaveClan(BasePlayer player);
    private void cmdClanLeave(BasePlayer player, string[] args);
    public void KickPlayer(BasePlayer player, string targetId);
    private void cmdClanKick(BasePlayer player, string[] args);
    public void DisbandClan(BasePlayer player);
    private void cmdClanDisband(BasePlayer player, string[] args);
    public void Alliance(BasePlayer player, string targetClan, string type);
    private void cmdChatClanAlly(BasePlayer player, string command, string[] args);
    private void cmdChatClanHelp(BasePlayer player, string command, string[] args);
    private void cmdChatClanInfo(BasePlayer player, string command, string[] args);
    private void cmdChatClanchat(BasePlayer player, string command, string[] args);
    private void cmdChatAllychat(BasePlayer player, string command, string[] args);
    private void cmdChatClanFF(BasePlayer player, string command, string[] args);
    public bool HasFFEnabled(ulong playerId);
    public void ToggleFF(ulong playerId);
    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
    private class StoredData
    {
        public Dictionary<string, Clan> clans;
        public int saveStamp;
        public string lastStorage;
        public StoredData();
    }

    private StoredData clanSaves;
    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
    public class Clan
    {
        public string tag;
        public string description;
        public string owner;
        public string council;
        public int created;
        public int updated;
        public List<string> moderators;
        public List<string> members;
        [JsonIgnore]
        [ProtoIgnore]
        public List<BasePlayer> membersBasePlayer;
        [JsonIgnore]
        [ProtoIgnore]
        public List<IPlayer> membersIPlayer;
        public Dictionary<string, int> invites;
        public List<string> clanAlliances;
        public List<string> invitedAllies;
        public List<string> pendingInvites;
        [JsonIgnore]
        [ProtoIgnore]
        private RelationshipManager.PlayerTeam _playerTeam;
        [JsonIgnore]
        [ProtoIgnore]
        public RelationshipManager.PlayerTeam PlayerTeam { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        public int Total { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        public int Mods { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        private string currentTeamLeader { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        private bool wasDisbanded;
        [JsonIgnore]
        [ProtoIgnore]
        public int Online { get; set; }
        public bool IsOwner(string userId);
        public bool IsCouncil(string userId);
        public bool IsModerator(string userId);
        public bool IsMember(string userId);
        public bool IsInvited(string userId);
        public bool HasAnyRole(string userId);
        public static Clan Create(string tag, string description, string ownerId);
        public void SetOwner(object obj);
        public void SetCouncil(object obj);
        private string GetObjectId(object obj);
        public void AddMember(object obj);
        public void RemoveMember(object obj);
        public void SetModerator(object obj);
        public void RemoveModerator(object obj);
        public void AddInvite(object obj);
        public void RemoveInvite(object obj);
        public void AddBasePlayer(BasePlayer basePlayer, bool flag);
        public void RemoveBasePlayer(BasePlayer basePlayer, bool flag);
        public BasePlayer GetBasePlayer(string Id);
        public void AddIPlayer(IPlayer iPlayer);
        public IPlayer GetIPlayer(string Id);
        public bool ValidateOwner();
        public IPlayer FindClanMember(string nameOrId);
        public IPlayer FindServerIPlayer(string partialName);
        public IPlayer FindInvitedIPlayer(string partialName);
        public void BroadcastChat(string message);
        public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
        public void AllyBroadcastChat(string message);
        public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
        public string GetColoredName(string Id, string Name);
        public string GetRoleString(string userID);
        public string GetRoleColor(string userID);
        internal JObject ToJObject();
        public Clan();
        internal void OnCreate();
        internal void OnUnload();
        internal void OnDestroy();
        public void DisbandTeam();
        private void DestroyPlayerTeam();
        internal void OnUpdate(bool hasChanges);
        internal void UpdateTeam();
    }

    private void OnPlayerDie(BasePlayer player, HitInfo info);
    private void OnPlayerRespawned(BasePlayer player);
    [HookMethod("GetClan")]
    private JObject GetClan(string tag);
    [HookMethod("GetAllClans")]
    private JArray GetAllClans();
    [HookMethod("GetClanOf")]
    private string GetClanOf(ulong player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(string player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(BasePlayer player);
    [HookMethod("GetClanMembers")]
    private List<string> GetClanMembers(ulong PlayerID);
    [HookMethod("GetClanMembers")]
    private List<string> GetClanMembers(string PlayerID);
    [HookMethod("HasFriend")]
    private object HasFriend(ulong entOwnerID, ulong PlayerUserID);
    [HookMethod("IsModerator")]
    private object IsModerator(ulong PlayerUserID);
    public static int UnixTimeStampUTC();
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private string msg(string key, string id);
    private void PrintChat(BasePlayer player, string message);
    [ConsoleCommand("clans")]
    private void cclans(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.cmds")]
    private void cclansCommands(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.list")]
    private void cclansList(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.showduplicates")]
    private void cclansDuplicates(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.listex")]
    private void cclansListEx(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.show")]
    private void cclansShow(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.msg")]
    private void cclansBroadcast(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.create")]
    private void cclansClanCreate(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.rename")]
    private void cclansClanRename(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.invite")]
    private void cclansPlayerInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.join")]
    private void cclansPlayerJoin(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.kick")]
    private void cclansPlayerKick(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.owner")]
    private void cclansClanOwner(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.promote")]
    private void cclansPlayerPromote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.demote")]
    private void cclansPlayerDemote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.disband")]
    private void cclansClanDisband(ConsoleSystem.Arg arg);
    private bool FilterText(string tag);
    private string TranslateLeet(string original);
    private bool TryGetClan(string input, Clan clan);
    private void RemoveClan(string tag);
    [HookMethod("EnableBypass")]
    private void EnableBypass(object userId);
    [HookMethod("DisableBypass")]
    private void DisableBypass(object userId);
    private void OnPluginLoaded(Plugin plugin);
    private string getFormattedClanTag(IPlayer player);
    private List<ulong> usedConsoleInput;
    private void ChatSwitch(BasePlayer player, string message, bool keepConsole);
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
private class StoredData
{
    public Dictionary<string, Clan> clans;
    public int saveStamp;
    public string lastStorage;
    public StoredData();
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
public class Clan
{
    public string tag;
    public string description;
    public string owner;
    public string council;
    public int created;
    public int updated;
    public List<string> moderators;
    public List<string> members;
    [JsonIgnore]
    [ProtoIgnore]
    public List<BasePlayer> membersBasePlayer;
    [JsonIgnore]
    [ProtoIgnore]
    public List<IPlayer> membersIPlayer;
    public Dictionary<string, int> invites;
    public List<string> clanAlliances;
    public List<string> invitedAllies;
    public List<string> pendingInvites;
    [JsonIgnore]
    [ProtoIgnore]
    private RelationshipManager.PlayerTeam _playerTeam;
    [JsonIgnore]
    [ProtoIgnore]
    public RelationshipManager.PlayerTeam PlayerTeam { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    public int Total { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    public int Mods { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    private string currentTeamLeader { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    private bool wasDisbanded;
    [JsonIgnore]
    [ProtoIgnore]
    public int Online { get; set; }
    public bool IsOwner(string userId);
    public bool IsCouncil(string userId);
    public bool IsModerator(string userId);
    public bool IsMember(string userId);
    public bool IsInvited(string userId);
    public bool HasAnyRole(string userId);
    public static Clan Create(string tag, string description, string ownerId);
    public void SetOwner(object obj);
    public void SetCouncil(object obj);
    private string GetObjectId(object obj);
    public void AddMember(object obj);
    public void RemoveMember(object obj);
    public void SetModerator(object obj);
    public void RemoveModerator(object obj);
    public void AddInvite(object obj);
    public void RemoveInvite(object obj);
    public void AddBasePlayer(BasePlayer basePlayer, bool flag);
    public void RemoveBasePlayer(BasePlayer basePlayer, bool flag);
    public BasePlayer GetBasePlayer(string Id);
    public void AddIPlayer(IPlayer iPlayer);
    public IPlayer GetIPlayer(string Id);
    public bool ValidateOwner();
    public IPlayer FindClanMember(string nameOrId);
    public IPlayer FindServerIPlayer(string partialName);
    public IPlayer FindInvitedIPlayer(string partialName);
    public void BroadcastChat(string message);
    public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
    public void AllyBroadcastChat(string message);
    public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
    public string GetColoredName(string Id, string Name);
    public string GetRoleString(string userID);
    public string GetRoleColor(string userID);
    internal JObject ToJObject();
    public Clan();
    internal void OnCreate();
    internal void OnUnload();
    internal void OnDestroy();
    public void DisbandTeam();
    private void DestroyPlayerTeam();
    internal void OnUpdate(bool hasChanges);
    internal void UpdateTeam();
}


```

---

## Clans (3)

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text;
using System.Linq;
using System.Text.RegularExpressions;
using UnityEngine;
using Rust;
using ProtoBuf;
using Facepunch.Extend;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Libraries.Covalence;

Oxide.Plugins
[Info("Clans", "FuJiCuRa", "2.14.4", ResourceId = 14)]
[Description("Clans plugin with Allies, inbuilt FriendlyFire and much more...")]
public class Clans : RustPlugin
{
    private bool Changed;
    private bool Initialized;
    private static Clans cc;
    private bool newSaveDetected;
    private List<ulong> manuallyEnabledBy;
    private HashSet<ulong> bypass;
    private Dictionary<string, DateTime> notificationTimes;
    private List<int> creationTimes;
    private static DateTime Epoch;
    private static double MaxUnixSeconds;
    public Dictionary<string, Clan> clans;
    public Dictionary<string, string> clansSearch;
    private List<string> purgedClans;
    private Dictionary<string, List<string>> pendingPlayerInvites;
    private Regex tagReExt;
    private Dictionary<string, Clan> clanCache;
    private List<object> filterDefaults();
    public int limitMembers;
    private int limitModerators;
    public int limitAlliances;
    private int tagLengthMin;
    private int tagLengthMax;
    private int inviteValidDays;
    private int friendlyFireNotifyTimeout;
    private string allowedSpecialChars;
    public bool enableFFOPtion;
    private bool enableAllyFFOPtion;
    private bool enableWordFilter;
    private bool enableClanTagging;
    public bool enableClanAllies;
    private bool forceAllyFFNoDeactivate;
    private bool forceClanFFNoDeactivate;
    private bool enableWhoIsOnlineMsg;
    private bool enableComesOnlineMsg;
    private bool forceNametagsOnTagging;
    private int authLevelRename;
    private int authLevelDisband;
    private int authLevelInvite;
    private int authLevelKick;
    private int authLevelCreate;
    private int authLevelPromoteDemote;
    private int authLevelClanInfo;
    private bool purgeOldClans;
    private int notUpdatedSinceDays;
    private bool listPurgedClans;
    private bool wipeClansOnNewSave;
    private bool useProtostorageClandata;
    private string consoleName;
    private string broadcastPrefix;
    private string broadcastPrefixAlly;
    private string broadcastPrefixColor;
    private string broadcastPrefixFormat;
    private string broadcastMessageColor;
    private string colorCmdUsage;
    private string colorTextMsg;
    private string colorClanNamesOverview;
    private string colorClanFFOff;
    private string colorClanFFOn;
    private string pluginPrefix;
    private string pluginPrefixColor;
    private string pluginPrefixREBORNColor;
    private bool pluginPrefixREBORNShow;
    private string pluginPrefixFormat;
    private string clanServerColor;
    private string clanOwnerColor;
    private string clanCouncilColor;
    private string clanModeratorColor;
    private string clanMemberColor;
    private bool setHomeOwner;
    private bool setHomeModerator;
    private bool setHomeMember;
    private string chatCommandClan;
    private string chatCommandFF;
    private string chatCommandAllyChat;
    private string chatCommandClanChat;
    private string chatCommandClanInfo;
    private string subCommandClanHelp;
    private string subCommandClanAlly;
    private bool usePermGroups;
    private string permGroupPrefix;
    private bool usePermToCreateClan;
    private string permissionToCreateClan;
    private bool usePermToJoinClan;
    private string permissionToJoinClan;
    private string clanTagColorBetterChat;
    private int clanTagSizeBetterChat;
    private string clanTagOpening;
    private string clanTagClosing;
    private bool clanChatDenyOnMuted;
    public static bool useRelationshipManager;
    private bool teamUiWasDisabled;
    private bool useRankColorsPanel;
    private bool disableManageFunctions;
    private bool allowButtonLeave;
    private bool allowButtonKick;
    private bool allowDirectInvite;
    private bool allowPromoteLeader;
    private bool logClanChanges;
    private List<object> wordFilter;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void Loaded();
    private void OnServerSave();
    private void OnNewSave();
    private void Unload();
    private void OnServerInitialized();
    private IEnumerator ServerInitialized(object obj);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void SaveConf();
    private static string r(string i);
    private object LoadData();
    private void InitializeClans(bool newFileFound);
    public static int TakeCreatedTime(int stamp);
    private void SaveData(bool force);
    public Clan findClan(string tag);
    public Clan findClanByUser(string userId);
    private Clan SetupPlayer(BasePlayer player, IPlayer current, bool hasLeft, Clan clan, bool teamForced, string oldTag);
    private bool NullClanTeam(BasePlayer player);
    private void DoCleanUp(BasePlayer player);
    private void setupPlayers(List<string> playerIds, bool isDisband, Clan oldClan, string tag);
    private void OnPlayerInit(BasePlayer player);
    private IEnumerator WaitForReady(BasePlayer player, Clan clan);
    private void ComingOnlineInfo(BasePlayer player, Clan clan);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hit);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hit);
    private object OnAttackShared(BasePlayer attacker, BasePlayer victim, HitInfo hit);
    private void AllyRemovalCheck();
    private void ccmdChatClan(ConsoleSystem.Arg arg);
    private void cmdChatClan(BasePlayer player, string command, string[] args);
    private void cmdClanOverview(BasePlayer player);
    private void cmdClanCreate(BasePlayer player, string[] args);
    public void InvitePlayer(BasePlayer player, string targetId);
    private void cmdClanInvite(BasePlayer player, string[] args);
    public void WithdrawPlayer(BasePlayer player, string targetId);
    private void cmdClanWithdraw(BasePlayer player, string[] args);
    private void cmdClanJoin(BasePlayer player, string[] args);
    public void PromotePlayer(BasePlayer player, string targetId);
    private void cmdClanPromote(BasePlayer player, string[] args);
    public void DemotePlayer(BasePlayer player, string targetId);
    private void cmdClanDemote(BasePlayer player, string[] args);
    public void LeaveClan(BasePlayer player);
    private void cmdClanLeave(BasePlayer player, string[] args);
    public void KickPlayer(BasePlayer player, string targetId);
    private void cmdClanKick(BasePlayer player, string[] args);
    public void DisbandClan(BasePlayer player);
    private void cmdClanDisband(BasePlayer player, string[] args);
    public void Alliance(BasePlayer player, string targetClan, string type);
    private void cmdChatClanAlly(BasePlayer player, string command, string[] args);
    private void cmdChatClanHelp(BasePlayer player, string command, string[] args);
    private void cmdChatClanInfo(BasePlayer player, string command, string[] args);
    private void cmdChatClanchat(BasePlayer player, string command, string[] args);
    private void cmdChatAllychat(BasePlayer player, string command, string[] args);
    private void cmdChatClanFF(BasePlayer player, string command, string[] args);
    public bool HasFFEnabled(ulong playerId);
    public void ToggleFF(ulong playerId);
    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
    private class StoredData
    {
        public Dictionary<string, Clan> clans;
        public int saveStamp;
        public string lastStorage;
        public StoredData();
    }

    private StoredData clanSaves;
    [ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
    public class Clan
    {
        public string tag;
        public string description;
        public string owner;
        public string council;
        public int created;
        public int updated;
        public List<string> moderators;
        public List<string> members;
        [JsonIgnore]
        [ProtoIgnore]
        public List<BasePlayer> membersBasePlayer;
        [JsonIgnore]
        [ProtoIgnore]
        public List<IPlayer> membersIPlayer;
        public Dictionary<string, int> invites;
        public List<string> clanAlliances;
        public List<string> invitedAllies;
        public List<string> pendingInvites;
        [JsonIgnore]
        [ProtoIgnore]
        private RelationshipManager.PlayerTeam _playerTeam;
        [JsonIgnore]
        [ProtoIgnore]
        public RelationshipManager.PlayerTeam PlayerTeam { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        public int Total { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        public int Mods { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        private string currentTeamLeader { get; set; }
        [JsonIgnore]
        [ProtoIgnore]
        private bool wasDisbanded;
        [JsonIgnore]
        [ProtoIgnore]
        public int Online { get; set; }
        public bool IsOwner(string userId);
        public bool IsCouncil(string userId);
        public bool IsModerator(string userId);
        public bool IsMember(string userId);
        public bool IsInvited(string userId);
        public bool HasAnyRole(string userId);
        public static Clan Create(string tag, string description, string ownerId);
        public void SetOwner(object obj);
        public void SetCouncil(object obj);
        private string GetObjectId(object obj);
        public void AddMember(object obj);
        public void RemoveMember(object obj);
        public void SetModerator(object obj);
        public void RemoveModerator(object obj);
        public void AddInvite(object obj);
        public void RemoveInvite(object obj);
        public void AddBasePlayer(BasePlayer basePlayer, bool flag);
        public void RemoveBasePlayer(BasePlayer basePlayer, bool flag);
        public BasePlayer GetBasePlayer(string Id);
        public void AddIPlayer(IPlayer iPlayer);
        public IPlayer GetIPlayer(string Id);
        public bool ValidateOwner();
        public IPlayer FindClanMember(string nameOrId);
        public IPlayer FindServerIPlayer(string partialName);
        public IPlayer FindInvitedIPlayer(string partialName);
        public void BroadcastChat(string message);
        public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
        public void AllyBroadcastChat(string message);
        public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
        public string GetColoredName(string Id, string Name);
        public string GetRoleString(string userID);
        public string GetRoleColor(string userID);
        internal JObject ToJObject();
        public Clan();
        internal void OnCreate();
        internal void OnUnload();
        internal void OnDestroy();
        public void DisbandTeam();
        private void DestroyPlayerTeam();
        internal void OnUpdate(bool hasChanges);
        internal void UpdateTeam();
    }

    private void OnPlayerDie(BasePlayer player, HitInfo info);
    private void OnPlayerRespawned(BasePlayer player);
    [HookMethod("GetClan")]
    private JObject GetClan(string tag);
    [HookMethod("GetAllClans")]
    private JArray GetAllClans();
    [HookMethod("GetClanOf")]
    private string GetClanOf(ulong player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(string player);
    [HookMethod("GetClanOf")]
    private string GetClanOf(BasePlayer player);
    [HookMethod("GetClanMembers")]
    private List<string> GetClanMembers(ulong PlayerID);
    [HookMethod("GetClanMembers")]
    private List<string> GetClanMembers(string PlayerID);
    [HookMethod("HasFriend")]
    private object HasFriend(ulong entOwnerID, ulong PlayerUserID);
    [HookMethod("IsModerator")]
    private object IsModerator(ulong PlayerUserID);
    public static int UnixTimeStampUTC();
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private string msg(string key, string id);
    private void PrintChat(BasePlayer player, string message);
    [ConsoleCommand("clans")]
    private void cclans(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.cmds")]
    private void cclansCommands(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.list")]
    private void cclansList(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.showduplicates")]
    private void cclansDuplicates(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.listex")]
    private void cclansListEx(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.show")]
    private void cclansShow(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.msg")]
    private void cclansBroadcast(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.create")]
    private void cclansClanCreate(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.rename")]
    private void cclansClanRename(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.invite")]
    private void cclansPlayerInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.join")]
    private void cclansPlayerJoin(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.kick")]
    private void cclansPlayerKick(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.owner")]
    private void cclansClanOwner(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.promote")]
    private void cclansPlayerPromote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.demote")]
    private void cclansPlayerDemote(ConsoleSystem.Arg arg);
    [ConsoleCommand("clans.disband")]
    private void cclansClanDisband(ConsoleSystem.Arg arg);
    private bool FilterText(string tag);
    private string TranslateLeet(string original);
    private bool TryGetClan(string input, Clan clan);
    private void RemoveClan(string tag);
    [HookMethod("EnableBypass")]
    private void EnableBypass(object userId);
    [HookMethod("DisableBypass")]
    private void DisableBypass(object userId);
    private void OnPluginLoaded(Plugin plugin);
    private string getFormattedClanTag(IPlayer player);
    private List<ulong> usedConsoleInput;
    private void ChatSwitch(BasePlayer player, string message, bool keepConsole);
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
private class StoredData
{
    public Dictionary<string, Clan> clans;
    public int saveStamp;
    public string lastStorage;
    public StoredData();
}

[ProtoContract(ImplicitFields = ImplicitFields.AllFields)]
public class Clan
{
    public string tag;
    public string description;
    public string owner;
    public string council;
    public int created;
    public int updated;
    public List<string> moderators;
    public List<string> members;
    [JsonIgnore]
    [ProtoIgnore]
    public List<BasePlayer> membersBasePlayer;
    [JsonIgnore]
    [ProtoIgnore]
    public List<IPlayer> membersIPlayer;
    public Dictionary<string, int> invites;
    public List<string> clanAlliances;
    public List<string> invitedAllies;
    public List<string> pendingInvites;
    [JsonIgnore]
    [ProtoIgnore]
    private RelationshipManager.PlayerTeam _playerTeam;
    [JsonIgnore]
    [ProtoIgnore]
    public RelationshipManager.PlayerTeam PlayerTeam { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    public int Total { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    public int Mods { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    private string currentTeamLeader { get; set; }
    [JsonIgnore]
    [ProtoIgnore]
    private bool wasDisbanded;
    [JsonIgnore]
    [ProtoIgnore]
    public int Online { get; set; }
    public bool IsOwner(string userId);
    public bool IsCouncil(string userId);
    public bool IsModerator(string userId);
    public bool IsMember(string userId);
    public bool IsInvited(string userId);
    public bool HasAnyRole(string userId);
    public static Clan Create(string tag, string description, string ownerId);
    public void SetOwner(object obj);
    public void SetCouncil(object obj);
    private string GetObjectId(object obj);
    public void AddMember(object obj);
    public void RemoveMember(object obj);
    public void SetModerator(object obj);
    public void RemoveModerator(object obj);
    public void AddInvite(object obj);
    public void RemoveInvite(object obj);
    public void AddBasePlayer(BasePlayer basePlayer, bool flag);
    public void RemoveBasePlayer(BasePlayer basePlayer, bool flag);
    public BasePlayer GetBasePlayer(string Id);
    public void AddIPlayer(IPlayer iPlayer);
    public IPlayer GetIPlayer(string Id);
    public bool ValidateOwner();
    public IPlayer FindClanMember(string nameOrId);
    public IPlayer FindServerIPlayer(string partialName);
    public IPlayer FindInvitedIPlayer(string partialName);
    public void BroadcastChat(string message);
    public void BroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4, string current);
    public void AllyBroadcastChat(string message);
    public void AllyBroadcastLoc(string messagetype, string arg1, string arg2, string arg3, string arg4);
    public string GetColoredName(string Id, string Name);
    public string GetRoleString(string userID);
    public string GetRoleColor(string userID);
    internal JObject ToJObject();
    public Clan();
    internal void OnCreate();
    internal void OnUnload();
    internal void OnDestroy();
    public void DisbandTeam();
    private void DestroyPlayerTeam();
    internal void OnUpdate(bool hasChanges);
    internal void UpdateTeam();
}


```

---

## Clans-0.2.8

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using ClansEx;

Oxide.Plugins
[Info("Clans", "k1lly0u", "0.2.8")]
public class Clans : CovalencePlugin
{
    private bool isInitialized;
    private Regex hexFilter;
    public static Clans Instance { get; set; }
    private static readonly DateTime Epoch;
    private static readonly double MaxUnixSeconds;
    private const string COLORED_LABEL;
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnPluginLoaded(Plugin plugin);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void Unload();
    private void InitializeClans();
    private bool ClanTagExists(string tag);
    private string FormatTime(double time);
    private string BetterChat_FormattedClanTag(IPlayer player);
    private static int UnixTimeStampUTC();
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    [HookMethod("CreateClan")]
    public void CreateClan(IPlayer player, string tag, string description);
    [HookMethod("InvitePlayer")]
    public bool InvitePlayer(IPlayer inviter, string targetId);
    [HookMethod("InvitePlayer")]
    public bool InvitePlayer(IPlayer inviter, IPlayer invitee);
    [HookMethod("WithdrawInvite")]
    public bool WithdrawInvite(IPlayer player, string partialNameOrID);
    [HookMethod("RejectInvite")]
    public bool RejectInvite(IPlayer player, string tag);
    [HookMethod("JoinClan")]
    public bool JoinClan(IPlayer player, string tag);
    [HookMethod("LeaveClan")]
    public bool LeaveClan(IPlayer player);
    [HookMethod("KickPlayer")]
    public bool KickPlayer(IPlayer player, string playerId);
    [HookMethod("PromotePlayer")]
    public bool PromotePlayer(IPlayer promoter, string targetId);
    [HookMethod("DemotePlayer")]
    public bool DemotePlayer(IPlayer demoter, string targetId);
    [HookMethod("DisbandClan")]
    public bool DisbandClan(IPlayer player);
    [HookMethod("OfferAlliance")]
    public bool OfferAlliance(IPlayer player, string tag);
    [HookMethod("WithdrawAlliance")]
    public bool WithdrawAlliance(IPlayer player, string tag);
    [HookMethod("AcceptAlliance")]
    public bool AcceptAlliance(IPlayer player, string tag);
    [HookMethod("RejectAlliance")]
    public bool RejectAlliance(IPlayer player, string tag);
    [HookMethod("RevokeAlliance")]
    public bool RevokeAlliance(IPlayer player, string tag);
    private void ClanChat(IPlayer player, string message);
    private void AllianceChat(IPlayer player, string message);
    [Command("a")]
    private void cmdAllianceChat(IPlayer player, string command, string[] args);
    [Command("c")]
    private void cmdClanChat(IPlayer player, string command, string[] args);
    [Command("cinfo")]
    private void cmdChatClanInfo(IPlayer player, string command, string[] args);
    [Command("clanhelp")]
    private void cmdChatClanHelp(IPlayer player, string command, string[] args);
    [Command("ally")]
    private void cmdChatClanAlly(IPlayer player, string command, string[] args);
    [Command("clan")]
    private void cmdChatClan(IPlayer player, string command, string[] args);
    private JObject GetClan(string tag);
    private JArray GetAllClans();
    private string GetClanOf(string playerId);
    private string GetClanOf(ulong playerId);
    private string GetClanOf(IPlayer player);
    private List<string> GetClanMembers(ulong playerId);
    private List<string> GetClanMembers(string playerId);
    private object HasFriend(ulong ownerId, ulong playerId);
    private object HasFriend(string ownerId, string playerId);
    private bool IsClanMember(ulong playerId, ulong otherId);
    private bool IsClanMember(string playerId, string otherId);
    private bool IsMemberOrAlly(ulong playerId, ulong otherId);
    private bool IsMemberOrAlly(string playerId, string otherId);
    private bool IsAllyPlayer(ulong playerId, ulong otherId);
    private bool IsAllyPlayer(string playerId, string otherId);
    private List<string> GetClanAlliances(ulong playerId);
    private List<string> GetClanAlliances(string playerId);
    [Serializable]
    public class Clan
    {
        public string Tag { get; set; }
        public string Description { get; set; }
        public string OwnerID { get; set; }
        public double CreationTime { get; set; }
        public double LastOnlineTime { get; set; }
        public Hash<string, Member> ClanMembers { get; set; }
        public Hash<string, MemberInvite> MemberInvites { get; set; }
        public HashSet<string> Alliances { get; set; }
        public Hash<string, double> AllianceInvites { get; set; }
        public HashSet<string> IncomingAlliances { get; set; }
        public string TagColor { get; set; }
        [JsonIgnore]
        public int OnlineCount { get; set; }
        [JsonIgnore]
        public int ModeratorCount { get; set; }
        [JsonIgnore]
        public int MemberCount { get; set; }
        [JsonIgnore]
        public int MemberInviteCount { get; set; }
        [JsonIgnore]
        public int AllianceCount { get; set; }
        [JsonIgnore]
        public int AllianceInviteCount { get; set; }
        public Clan();
        public Clan(IPlayer player, string tag, string description);
        internal void OnPlayerConnected(IPlayer player);
        internal void OnPlayerDisconnected(IPlayer player);
        internal bool InvitePlayer(IPlayer inviter, IPlayer invitee);
        internal bool JoinClan(IPlayer player);
        internal bool LeaveClan(IPlayer player);
        internal bool KickMember(IPlayer player, string targetId);
        internal bool PromotePlayer(IPlayer promoter, string targetId);
        internal bool DemotePlayer(IPlayer demoter, string targetId);
        internal void DisbandClan();
        internal void OnClanDisbanded(string tag);
        internal void OnUnload();
        internal bool IsAlliedClan(string otherClan);
        internal void MarkDirty();
        internal void Broadcast(string message);
        internal void Broadcast(string key, object[] args);
        [JsonIgnore]
        private string cachedClanInfo;
        [JsonIgnore]
        private string membersOnline;
        internal void PrintClanInfo(IPlayer player);
        internal string GetMembersOnline();
        public bool IsOwner(ulong playerId);
        public bool IsOwner(string playerId);
        public bool IsModerator(ulong playerId);
        public bool IsModerator(string playerId);
        public bool IsCouncil(ulong playerId);
        public bool IsMember(ulong playerId);
        public bool IsMember(string playerId);
        public Member GetOwner();
        public string GetRoleColor(string Id);
        public string GetRoleColor(Member.MemberRole role);
        [Serializable]
        public class Member
        {
            [JsonIgnore]
            public IPlayer Player { get; set; }
            [JsonProperty("Name")]
            public string DisplayName { get; set; }
            public MemberRole Role { get; set; }
            [JsonIgnore]
            public bool IsConnected { get; set; }
            [JsonIgnore]
            public bool MemberFFEnabled { get; set; }
            [JsonIgnore]
            public bool AllyFFEnabled { get; set; }
            public Member();
            public Member(MemberRole role, string name);
        }

        [Serializable]
        public class MemberInvite
        {
            [JsonProperty("Name")]
            public string DisplayName { get; set; }
            public double ExpiryTime { get; set; }
            public MemberInvite();
            public MemberInvite(IPlayer player);
            public MemberInvite(string name);
        }

        [JsonIgnore]
        private JObject serializedClanObject;
        internal JObject ToJObject();
        internal string FindPlayer(string partialNameOrID);
    }

    public static ConfigData configData;
    public class ConfigData
    {
        [JsonProperty(PropertyName = "Clan Options")]
        public ClanOptions Clans { get; set; }
        [JsonProperty(PropertyName = "Role Colors")]
        public ColorOptions Colors { get; set; }
        [JsonProperty(PropertyName = "Clan Tag Options")]
        public TagOptions Tags { get; set; }
        [JsonProperty(PropertyName = "Purge Options")]
        public PurgeOptions Purge { get; set; }
        [JsonProperty(PropertyName = "Settings")]
        public OtherOptions Options { get; set; }
        public class ClanOptions
        {
            [JsonProperty(PropertyName = "Member limit")]
            public int MemberLimit { get; set; }
            [JsonProperty(PropertyName = "Moderator limit")]
            public int ModeratorLimit { get; set; }
            [JsonProperty(PropertyName = "Alliance Options")]
            public AllianceOptions Alliance { get; set; }
            [JsonProperty(PropertyName = "Invite Options")]
            public InviteOptions Invites { get; set; }
            [JsonIgnore]
            public bool MemberFF { get; set; }
            [JsonIgnore]
            public bool OwnerFF { get; set; }
            public class AllianceOptions
            {
                [JsonProperty(PropertyName = "Enable clan alliances")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Alliance limit")]
                public int AllianceLimit { get; set; }
                [JsonIgnore]
                public bool AllyFF { get; set; }
                [JsonIgnore]
                public bool OwnerFF { get; set; }
            }

            public class InviteOptions
            {
                [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
                public int MemberInviteLimit { get; set; }
                [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
                public int MemberInviteExpireTime { get; set; }
                [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
                public int AllianceInviteLimit { get; set; }
                [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
                public int AllianceInviteExpireTime { get; set; }
            }

        }

        public class ColorOptions
        {
            [JsonProperty(PropertyName = "Clan owner color (hex)")]
            public string Owner { get; set; }
            [JsonProperty(PropertyName = "Clan moderator color (hex)")]
            public string Moderator { get; set; }
            [JsonProperty(PropertyName = "Clan member color (hex)")]
            public string Member { get; set; }
            [JsonProperty(PropertyName = "General text color (hex)")]
            public string TextColor { get; set; }
        }

        public class TagOptions
        {
            [JsonProperty(PropertyName = "Enable clan tags (requires BetterChat)")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Tag opening character")]
            public string TagOpen { get; set; }
            [JsonProperty(PropertyName = "Tag closing character")]
            public string TagClose { get; set; }
            [JsonProperty(PropertyName = "Tag color (hex)")]
            public string TagColor { get; set; }
            [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
            public bool CustomColors { get; set; }
            [JsonProperty(PropertyName = "Tag size")]
            public int TagSize { get; set; }
            [JsonProperty(PropertyName = "Tag character limits")]
            public Range TagLength { get; set; }
        }

        public class PurgeOptions
        {
            [JsonProperty(PropertyName = "Enable clan purging")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
            public int OlderThanDays { get; set; }
            [JsonProperty(PropertyName = "List purged clans in console when purging")]
            public bool ListPurgedClans { get; set; }
        }

        public class OtherOptions
        {
            [JsonProperty(PropertyName = "Log clan and member changes")]
            public bool LogChanges { get; set; }
            [JsonProperty(PropertyName = "Data save interval (seconds)")]
            public int SaveInterval { get; set; }
        }

        public class Range
        {
            public int Minimum { get; set; }
            public int Maximum { get; set; }
            public Range();
            public Range(int minimum, int maximum);
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    public StoredData storedData;
    private DynamicConfigFile data;
    private void TimedSaveData();
    private void SaveData();
    private void LoadData();
    private void RestoreClanData(Dictionary<string, OldClan> clanData);
    [Serializable]
    public class StoredData
    {
        public Hash<string, Clan> clans;
        public Hash<string, List<string>> playerInvites;
        [JsonIgnore]
        private Hash<string, string> playerLookup;
        public Clan FindClan(string tag);
        public Clan FindClanByID(ulong playerId);
        public Clan FindClanByID(string playerId);
        public Clan.Member FindMemberByID(ulong playerId);
        public Clan.Member FindMemberByID(string playerId);
        internal void RegisterPlayer(string playerId, string tag);
        internal void UnregisterPlayer(string playerId);
        internal void AddPlayerInvite(string target, string tag);
        internal void RevokePlayerInvite(string target, string tag);
        internal void OnInviteAccepted(string target, string tag);
        internal void OnInviteRejected(string target, string tag);
    }

    public class OldClan
    {
        public string clanTag;
        public string ownerID;
        public List<string> moderators;
        public Dictionary<string, string> members;
        public List<string> clanAlliances;
        public Dictionary<string, string> invitedPlayers;
        public List<string> invitedAllies;
        public List<string> pendingInvites;
    }

    private static string Message(string key, string playerId);
    private readonly Dictionary<string, string> Messages;
}

[Serializable]
public class Clan
{
    public string Tag { get; set; }
    public string Description { get; set; }
    public string OwnerID { get; set; }
    public double CreationTime { get; set; }
    public double LastOnlineTime { get; set; }
    public Hash<string, Member> ClanMembers { get; set; }
    public Hash<string, MemberInvite> MemberInvites { get; set; }
    public HashSet<string> Alliances { get; set; }
    public Hash<string, double> AllianceInvites { get; set; }
    public HashSet<string> IncomingAlliances { get; set; }
    public string TagColor { get; set; }
    [JsonIgnore]
    public int OnlineCount { get; set; }
    [JsonIgnore]
    public int ModeratorCount { get; set; }
    [JsonIgnore]
    public int MemberCount { get; set; }
    [JsonIgnore]
    public int MemberInviteCount { get; set; }
    [JsonIgnore]
    public int AllianceCount { get; set; }
    [JsonIgnore]
    public int AllianceInviteCount { get; set; }
    public Clan();
    public Clan(IPlayer player, string tag, string description);
    internal void OnPlayerConnected(IPlayer player);
    internal void OnPlayerDisconnected(IPlayer player);
    internal bool InvitePlayer(IPlayer inviter, IPlayer invitee);
    internal bool JoinClan(IPlayer player);
    internal bool LeaveClan(IPlayer player);
    internal bool KickMember(IPlayer player, string targetId);
    internal bool PromotePlayer(IPlayer promoter, string targetId);
    internal bool DemotePlayer(IPlayer demoter, string targetId);
    internal void DisbandClan();
    internal void OnClanDisbanded(string tag);
    internal void OnUnload();
    internal bool IsAlliedClan(string otherClan);
    internal void MarkDirty();
    internal void Broadcast(string message);
    internal void Broadcast(string key, object[] args);
    [JsonIgnore]
    private string cachedClanInfo;
    [JsonIgnore]
    private string membersOnline;
    internal void PrintClanInfo(IPlayer player);
    internal string GetMembersOnline();
    public bool IsOwner(ulong playerId);
    public bool IsOwner(string playerId);
    public bool IsModerator(ulong playerId);
    public bool IsModerator(string playerId);
    public bool IsCouncil(ulong playerId);
    public bool IsMember(ulong playerId);
    public bool IsMember(string playerId);
    public Member GetOwner();
    public string GetRoleColor(string Id);
    public string GetRoleColor(Member.MemberRole role);
    [Serializable]
    public class Member
    {
        [JsonIgnore]
        public IPlayer Player { get; set; }
        [JsonProperty("Name")]
        public string DisplayName { get; set; }
        public MemberRole Role { get; set; }
        [JsonIgnore]
        public bool IsConnected { get; set; }
        [JsonIgnore]
        public bool MemberFFEnabled { get; set; }
        [JsonIgnore]
        public bool AllyFFEnabled { get; set; }
        public Member();
        public Member(MemberRole role, string name);
    }

    [Serializable]
    public class MemberInvite
    {
        [JsonProperty("Name")]
        public string DisplayName { get; set; }
        public double ExpiryTime { get; set; }
        public MemberInvite();
        public MemberInvite(IPlayer player);
        public MemberInvite(string name);
    }

    [JsonIgnore]
    private JObject serializedClanObject;
    internal JObject ToJObject();
    internal string FindPlayer(string partialNameOrID);
}

[Serializable]
public class Member
{
    [JsonIgnore]
    public IPlayer Player { get; set; }
    [JsonProperty("Name")]
    public string DisplayName { get; set; }
    public MemberRole Role { get; set; }
    [JsonIgnore]
    public bool IsConnected { get; set; }
    [JsonIgnore]
    public bool MemberFFEnabled { get; set; }
    [JsonIgnore]
    public bool AllyFFEnabled { get; set; }
    public Member();
    public Member(MemberRole role, string name);
}

[Serializable]
public class MemberInvite
{
    [JsonProperty("Name")]
    public string DisplayName { get; set; }
    public double ExpiryTime { get; set; }
    public MemberInvite();
    public MemberInvite(IPlayer player);
    public MemberInvite(string name);
}

public class ConfigData
{
    [JsonProperty(PropertyName = "Clan Options")]
    public ClanOptions Clans { get; set; }
    [JsonProperty(PropertyName = "Role Colors")]
    public ColorOptions Colors { get; set; }
    [JsonProperty(PropertyName = "Clan Tag Options")]
    public TagOptions Tags { get; set; }
    [JsonProperty(PropertyName = "Purge Options")]
    public PurgeOptions Purge { get; set; }
    [JsonProperty(PropertyName = "Settings")]
    public OtherOptions Options { get; set; }
    public class ClanOptions
    {
        [JsonProperty(PropertyName = "Member limit")]
        public int MemberLimit { get; set; }
        [JsonProperty(PropertyName = "Moderator limit")]
        public int ModeratorLimit { get; set; }
        [JsonProperty(PropertyName = "Alliance Options")]
        public AllianceOptions Alliance { get; set; }
        [JsonProperty(PropertyName = "Invite Options")]
        public InviteOptions Invites { get; set; }
        [JsonIgnore]
        public bool MemberFF { get; set; }
        [JsonIgnore]
        public bool OwnerFF { get; set; }
        public class AllianceOptions
        {
            [JsonProperty(PropertyName = "Enable clan alliances")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Alliance limit")]
            public int AllianceLimit { get; set; }
            [JsonIgnore]
            public bool AllyFF { get; set; }
            [JsonIgnore]
            public bool OwnerFF { get; set; }
        }

        public class InviteOptions
        {
            [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
            public int MemberInviteLimit { get; set; }
            [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
            public int MemberInviteExpireTime { get; set; }
            [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
            public int AllianceInviteLimit { get; set; }
            [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
            public int AllianceInviteExpireTime { get; set; }
        }

    }

    public class ColorOptions
    {
        [JsonProperty(PropertyName = "Clan owner color (hex)")]
        public string Owner { get; set; }
        [JsonProperty(PropertyName = "Clan moderator color (hex)")]
        public string Moderator { get; set; }
        [JsonProperty(PropertyName = "Clan member color (hex)")]
        public string Member { get; set; }
        [JsonProperty(PropertyName = "General text color (hex)")]
        public string TextColor { get; set; }
    }

    public class TagOptions
    {
        [JsonProperty(PropertyName = "Enable clan tags (requires BetterChat)")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Tag opening character")]
        public string TagOpen { get; set; }
        [JsonProperty(PropertyName = "Tag closing character")]
        public string TagClose { get; set; }
        [JsonProperty(PropertyName = "Tag color (hex)")]
        public string TagColor { get; set; }
        [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
        public bool CustomColors { get; set; }
        [JsonProperty(PropertyName = "Tag size")]
        public int TagSize { get; set; }
        [JsonProperty(PropertyName = "Tag character limits")]
        public Range TagLength { get; set; }
    }

    public class PurgeOptions
    {
        [JsonProperty(PropertyName = "Enable clan purging")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
        public int OlderThanDays { get; set; }
        [JsonProperty(PropertyName = "List purged clans in console when purging")]
        public bool ListPurgedClans { get; set; }
    }

    public class OtherOptions
    {
        [JsonProperty(PropertyName = "Log clan and member changes")]
        public bool LogChanges { get; set; }
        [JsonProperty(PropertyName = "Data save interval (seconds)")]
        public int SaveInterval { get; set; }
    }

    public class Range
    {
        public int Minimum { get; set; }
        public int Maximum { get; set; }
        public Range();
        public Range(int minimum, int maximum);
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class ClanOptions
{
    [JsonProperty(PropertyName = "Member limit")]
    public int MemberLimit { get; set; }
    [JsonProperty(PropertyName = "Moderator limit")]
    public int ModeratorLimit { get; set; }
    [JsonProperty(PropertyName = "Alliance Options")]
    public AllianceOptions Alliance { get; set; }
    [JsonProperty(PropertyName = "Invite Options")]
    public InviteOptions Invites { get; set; }
    [JsonIgnore]
    public bool MemberFF { get; set; }
    [JsonIgnore]
    public bool OwnerFF { get; set; }
    public class AllianceOptions
    {
        [JsonProperty(PropertyName = "Enable clan alliances")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Alliance limit")]
        public int AllianceLimit { get; set; }
        [JsonIgnore]
        public bool AllyFF { get; set; }
        [JsonIgnore]
        public bool OwnerFF { get; set; }
    }

    public class InviteOptions
    {
        [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
        public int MemberInviteLimit { get; set; }
        [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
        public int MemberInviteExpireTime { get; set; }
        [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
        public int AllianceInviteLimit { get; set; }
        [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
        public int AllianceInviteExpireTime { get; set; }
    }

}

public class AllianceOptions
{
    [JsonProperty(PropertyName = "Enable clan alliances")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Alliance limit")]
    public int AllianceLimit { get; set; }
    [JsonIgnore]
    public bool AllyFF { get; set; }
    [JsonIgnore]
    public bool OwnerFF { get; set; }
}

public class InviteOptions
{
    [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
    public int MemberInviteLimit { get; set; }
    [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
    public int MemberInviteExpireTime { get; set; }
    [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
    public int AllianceInviteLimit { get; set; }
    [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
    public int AllianceInviteExpireTime { get; set; }
}

public class ColorOptions
{
    [JsonProperty(PropertyName = "Clan owner color (hex)")]
    public string Owner { get; set; }
    [JsonProperty(PropertyName = "Clan moderator color (hex)")]
    public string Moderator { get; set; }
    [JsonProperty(PropertyName = "Clan member color (hex)")]
    public string Member { get; set; }
    [JsonProperty(PropertyName = "General text color (hex)")]
    public string TextColor { get; set; }
}

public class TagOptions
{
    [JsonProperty(PropertyName = "Enable clan tags (requires BetterChat)")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Tag opening character")]
    public string TagOpen { get; set; }
    [JsonProperty(PropertyName = "Tag closing character")]
    public string TagClose { get; set; }
    [JsonProperty(PropertyName = "Tag color (hex)")]
    public string TagColor { get; set; }
    [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
    public bool CustomColors { get; set; }
    [JsonProperty(PropertyName = "Tag size")]
    public int TagSize { get; set; }
    [JsonProperty(PropertyName = "Tag character limits")]
    public Range TagLength { get; set; }
}

public class PurgeOptions
{
    [JsonProperty(PropertyName = "Enable clan purging")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
    public int OlderThanDays { get; set; }
    [JsonProperty(PropertyName = "List purged clans in console when purging")]
    public bool ListPurgedClans { get; set; }
}

public class OtherOptions
{
    [JsonProperty(PropertyName = "Log clan and member changes")]
    public bool LogChanges { get; set; }
    [JsonProperty(PropertyName = "Data save interval (seconds)")]
    public int SaveInterval { get; set; }
}

public class Range
{
    public int Minimum { get; set; }
    public int Maximum { get; set; }
    public Range();
    public Range(int minimum, int maximum);
}

[Serializable]
public class StoredData
{
    public Hash<string, Clan> clans;
    public Hash<string, List<string>> playerInvites;
    [JsonIgnore]
    private Hash<string, string> playerLookup;
    public Clan FindClan(string tag);
    public Clan FindClanByID(ulong playerId);
    public Clan FindClanByID(string playerId);
    public Clan.Member FindMemberByID(ulong playerId);
    public Clan.Member FindMemberByID(string playerId);
    internal void RegisterPlayer(string playerId, string tag);
    internal void UnregisterPlayer(string playerId);
    internal void AddPlayerInvite(string target, string tag);
    internal void RevokePlayerInvite(string target, string tag);
    internal void OnInviteAccepted(string target, string tag);
    internal void OnInviteRejected(string target, string tag);
}

public class OldClan
{
    public string clanTag;
    public string ownerID;
    public List<string> moderators;
    public Dictionary<string, string> members;
    public List<string> clanAlliances;
    public Dictionary<string, string> invitedPlayers;
    public List<string> invitedAllies;
    public List<string> pendingInvites;
}

public static class StringExtensions
{
    public static bool Contains(string haystack, string needle, CompareOptions options);
}

public static class ListPool
{
    public static Dictionary<Type, object> directory;
    public static void CreateCollection(int capacity);
    public static ListCollection<T> FindCollection();
    public static List<T> Get();
    public static List<T> Get(int capacity);
    private static T GetList();
    public static void Free(List<T> list);
    private static void FreeList(T t);
    public static void ClearPool();
    public class ListCollection
    {
        public Stack<T> stack;
        private readonly int maximumSize;
        public bool HasSpace { get; set; }
        public ListCollection(int maximumSize);
    }

}

public class ListCollection
{
    public Stack<T> stack;
    private readonly int maximumSize;
    public bool HasSpace { get; set; }
    public ListCollection(int maximumSize);
}

ClansEx
public static class StringExtensions
{
    public static bool Contains(string haystack, string needle, CompareOptions options);
}

public static class ListPool
{
    public static Dictionary<Type, object> directory;
    public static void CreateCollection(int capacity);
    public static ListCollection<T> FindCollection();
    public static List<T> Get();
    public static List<T> Get(int capacity);
    private static T GetList();
    public static void Free(List<T> list);
    private static void FreeList(T t);
    public static void ClearPool();
    public class ListCollection
    {
        public Stack<T> stack;
        private readonly int maximumSize;
        public bool HasSpace { get; set; }
        public ListCollection(int maximumSize);
    }

}

public class ListCollection
{
    public Stack<T> stack;
    private readonly int maximumSize;
    public bool HasSpace { get; set; }
    public ListCollection(int maximumSize);
}


```

---

## Clans-3.0.40

```csharp
using CompanionServer;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using ProtoBuf;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;
using VersionNumber = Oxide.Core.VersionNumber;

Oxide.Plugins
[Info("Clans", "k1lly0u", "3.0.40")]
 class Clans : RustPlugin
{
    [PluginReference]
    private Plugin DiscordClans;
    internal StoredData storedData;
    private bool wipeData;
    private bool isInitialized;
    private Coroutine initClansRoutine;
    private Regex tagFilter;
    private Regex hexFilter;
    private int[] customTagMinValue;
    private int[] customTagMaxValue;
    private HashSet<ulong> friendlyFireDisabled;
    public static Clans Instance { get; set; }
    private static DateTime Epoch;
    private static double MaxUnixSeconds;
    private const string COLORED_LABEL;
    private void Loaded();
    private void OnServerInitialized();
    private void OnNewSave(string str);
    private void OnServerSave();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private object OnTeamCreate(BasePlayer player);
    private object OnTeamKick(RelationshipManager.PlayerTeam playerTeam, BasePlayer player, ulong targetId);
    private object OnTeamInvite(BasePlayer player, BasePlayer other);
    private object OnTeamAcceptInvite(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private object OnTeamRejectInvite(BasePlayer player, RelationshipManager.PlayerTeam playerTeam);
    private object OnTeamLeave(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private object OnTeamPromote(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private object OnTeamDisband(RelationshipManager.PlayerTeam playerTeam);
    private object OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private void Unload();
    private IEnumerator InitializeClans();
    private void SetMinMaxColorRange();
    private bool TagColorIsBlocked(string color);
    private bool TagColorWithinRange(string color);
    private string BetterChat_FormattedClanTag(Oxide.Core.Libraries.Covalence.IPlayer player);
    private static int UnixTimeStampUTC();
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private bool ContainsBlockedWord(string tag);
    private string TranslateLeet(string original);
    private bool ClanTagExists(string tag);
    private static string FormatTime(double time);
    private BasePlayer FindPlayer(string partialNameOrID);
    private static void RemoveFromTeam(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private static void RemoveFromTeam(RelationshipManager.PlayerTeam playerTeam, ulong playerId);
    private static void ClearTeam(RelationshipManager.PlayerTeam playerTeam);
    private static string RemoveTags(string str);
    private static List<KeyValuePair<string, string>> _tags;
    public static string StripHTMLTags(string source);
    internal void CreateClan(BasePlayer player, string tag, string description);
    internal bool InvitePlayer(BasePlayer inviter, ulong targetId);
    internal bool InvitePlayer(BasePlayer inviter, BasePlayer invitee);
    internal bool WithdrawInvite(BasePlayer player, string partialNameOrID);
    internal bool RejectInvite(BasePlayer player, string tag);
    internal bool JoinClan(BasePlayer player, string tag);
    internal bool LeaveClan(BasePlayer player);
    internal bool KickPlayer(BasePlayer player, ulong playerId);
    internal bool PromotePlayer(BasePlayer promoter, ulong targetId);
    internal bool DemotePlayer(BasePlayer demoter, ulong targetId);
    internal bool DisbandClan(BasePlayer player);
    internal bool OfferAlliance(BasePlayer player, string tag);
    internal bool WithdrawAlliance(BasePlayer player, string tag);
    internal bool AcceptAlliance(BasePlayer player, string tag);
    internal bool RejectAlliance(BasePlayer player, string tag);
    internal bool RevokeAlliance(BasePlayer player, string tag);
    private void ClanChat(BasePlayer player, string message);
    private void AllianceChat(BasePlayer player, string message);
    private void cmdAllianceChat(BasePlayer player, string command, string[] args);
    private void cmdClanChat(BasePlayer player, string command, string[] args);
    private void cmdClanFF(BasePlayer player, string command, string[] args);
    private void cmdAllyFF(BasePlayer player, string command, string[] args);
    private void cmdChatClanInfo(BasePlayer player, string command, string[] args);
    private void cmdChatClanHelp(BasePlayer player, string command, string[] args);
    private void cmdChatClanAlly(BasePlayer player, string command, string[] args);
    private void cmdChatClan(BasePlayer player, string command, string[] args);
    private void DisableBypass(ulong userId);
    private void EnableBypass(ulong userId);
    private JObject GetClan(string tag);
    private JArray GetAllClans();
    private string GetClanOf(ulong playerId);
    private string GetClanOf(BasePlayer player);
    private string GetClanOf(string playerId);
    private List<string> GetClanMembers(ulong playerId);
    private List<string> GetClanMembers(string playerId);
    private object HasFriend(ulong ownerId, ulong playerId);
    private object HasFriend(string ownerId, string playerId);
    private bool IsClanMember(ulong playerId, ulong otherId);
    private bool IsClanMember(string playerId, string otherId);
    private bool IsMemberOrAlly(ulong playerId, ulong otherId);
    private bool IsMemberOrAlly(string playerId, string otherId);
    private bool IsAllyPlayer(ulong playerId, ulong otherId);
    private bool IsAllyPlayer(string playerId, string otherId);
    private List<string> GetClanAlliances(ulong playerId);
    private List<string> GetClanAlliances(string playerId);
    [HookMethod("ToggleFF")]
    public void ToggleFF(ulong playerId);
    [HookMethod("HasFFEnabled")]
    public bool HasFFEnabled(ulong playerId);
    [Serializable, ProtoContract]
    public class Clan
    {
        [ProtoMember(1), JsonProperty]
        public string Tag { get; set; }
        [ProtoMember(2), JsonProperty]
        public string Description { get; set; }
        [ProtoMember(3), JsonProperty]
        public ulong OwnerID { get; set; }
        [ProtoMember(4), JsonProperty]
        public double CreationTime { get; set; }
        [ProtoMember(5), JsonProperty]
        public double LastOnlineTime { get; set; }
        [ProtoMember(6), JsonProperty]
        public Hash<ulong, Member> ClanMembers { get; set; }
        [ProtoMember(7), JsonProperty]
        public Hash<ulong, MemberInvite> MemberInvites { get; set; }
        [ProtoMember(8), JsonProperty]
        public HashSet<string> Alliances { get; set; }
        [ProtoMember(9), JsonProperty]
        public Hash<string, double> AllianceInvites { get; set; }
        [ProtoMember(10), JsonProperty]
        public HashSet<string> IncomingAlliances { get; set; }
        [ProtoMember(11), JsonProperty]
        public string TagColor { get; set; }
        [ProtoMember(12), JsonProperty]
        public int MemberInviteCooldownTime { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int OnlineCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal ulong CouncilID { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int ModeratorCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int MemberCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int MemberInviteCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int AllianceCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal int AllianceInviteCount { get; set; }
        [JsonIgnore, ProtoIgnore]
        private RelationshipManager.PlayerTeam _playerTeam;
        [JsonIgnore, ProtoIgnore]
        internal RelationshipManager.PlayerTeam PlayerTeam { get; set; }
        public int CountMembersAndAlliances();
        private static ulong FindRandomTeamID { get; set; }
        public Clan();
        public Clan(BasePlayer player, string tag, string description);
        internal void OnPlayerConnected(BasePlayer player);
        internal void OnPlayerDisconnected(BasePlayer player);
        internal bool InvitePlayer(BasePlayer inviter, BasePlayer invitee);
        internal bool JoinClan(BasePlayer player);
        internal bool LeaveClan(BasePlayer player);
        internal bool KickMember(BasePlayer player, ulong targetId);
        internal bool PromotePlayer(BasePlayer promoter, ulong targetId);
        internal bool DemotePlayer(BasePlayer demoter, ulong targetId);
        internal void DisbandClan();
        internal void OnClanDisbanded(string tag);
        internal void OnUnload();
        internal bool IsAlliedClan(string otherClan);
        internal void MarkDirty();
        internal void Broadcast(string message);
        internal void LocalizedBroadcast(string key, object[] args);
        [JsonIgnore, ProtoIgnore]
        private string cachedClanInfo;
        [JsonIgnore, ProtoIgnore]
        private string membersOnline;
        internal void PrintClanInfo(BasePlayer player);
        internal string GetMembersOnline();
        internal bool IsOwner(ulong playerId);
        internal bool IsCouncil(ulong playerId);
        internal bool IsModerator(ulong playerId);
        internal bool IsMember(ulong playerId);
        internal Member GetOwner();
        internal bool HasCouncil();
        internal string GetRoleColor(ulong userID);
        internal string GetRoleColor(Member.MemberRole role);
        [Serializable, ProtoContract]
        public class Member
        {
            [JsonIgnore, ProtoIgnore]
            public BasePlayer Player { get; set; }
            [ProtoMember(1)]
            public string DisplayName { get; set; }
            [ProtoMember(2)]
            public MemberRole Role { get; set; }
            [ProtoMember(3)]
            public bool MemberFFEnabled { get; set; }
            [ProtoMember(4)]
            public bool AllyFFEnabled { get; set; }
            [JsonIgnore, ProtoIgnore]
            internal bool IsConnected { get; set; }
            [JsonIgnore, ProtoIgnore]
            internal float lastFFAttackTime;
            [JsonIgnore, ProtoIgnore]
            internal float lastAFFAttackTime;
            public Member();
            public Member(MemberRole role, Clan clan);
            public Member(MemberRole role, bool memberFFEnabled, bool allyFFEnabled);
            public void OnClanMemberHit(string victimName);
            public void OnAllyMemberHit(string victimName);
        }

        [Serializable, ProtoContract]
        public class MemberInvite
        {
            [ProtoMember(1)]
            public string DisplayName { get; set; }
            [ProtoMember(2)]
            public double ExpiryTime { get; set; }
            public MemberInvite();
            public MemberInvite(BasePlayer player);
        }

        [JsonIgnore, ProtoIgnore]
        private JObject serializedClanObject;
        internal JObject ToJObject();
        internal ulong FindPlayer(string partialNameOrID);
    }

    [ConsoleCommand("clans")]
    private void ccmdClans(ConsoleSystem.Arg arg);
    internal static ConfigData configData;
    internal class ConfigData
    {
        [JsonProperty(PropertyName = "Clan Options")]
        public ClanOptions Clans { get; set; }
        [JsonProperty(PropertyName = "Command Options")]
        public CommandOptions Commands { get; set; }
        [JsonProperty(PropertyName = "Role Colors")]
        public ColorOptions Colors { get; set; }
        [JsonProperty(PropertyName = "Clan Tag Options")]
        public TagOptions Tags { get; set; }
        [JsonProperty(PropertyName = "Permission Options")]
        public PermissionOptions Permissions { get; set; }
        [JsonProperty(PropertyName = "Purge Options")]
        public PurgeOptions Purge { get; set; }
        [JsonProperty(PropertyName = "Settings")]
        public OtherOptions Options { get; set; }
        public class ClanOptions
        {
            [JsonProperty(PropertyName = "Member limit")]
            public int MemberLimit { get; set; }
            [JsonProperty(PropertyName = "Moderator limit")]
            public int ModeratorLimit { get; set; }
            [JsonProperty(PropertyName = "Allow friendly fire toggle (clan members)")]
            public bool MemberFF { get; set; }
            [JsonProperty(PropertyName = "Enable friendly fire by default (clan members)")]
            public bool DefaultEnableFF { get; set; }
            [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (clan members)")]
            public bool OwnerFF { get; set; }
            [JsonProperty(PropertyName = "Alliance Options")]
            public AllianceOptions Alliance { get; set; }
            [JsonProperty(PropertyName = "Invite Options")]
            public InviteOptions Invites { get; set; }
            [JsonProperty(PropertyName = "Rust Team Options")]
            public TeamOptions Teams { get; set; }
            public class AllianceOptions
            {
                [JsonProperty(PropertyName = "Enable clan alliances")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Alliance limit")]
                public int AllianceLimit { get; set; }
                [JsonProperty(PropertyName = "Count alliance members as clan members")]
                public bool CountAllianceMembers { get; set; }
                [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
                public bool AllyFF { get; set; }
                [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
                public bool DefaultEnableFF { get; set; }
                [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
                public bool OwnerFF { get; set; }
            }

            public class InviteOptions
            {
                [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
                public int MemberInviteLimit { get; set; }
                [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
                public int MemberInviteExpireTime { get; set; }
                [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
                public int AllianceInviteLimit { get; set; }
                [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
                public int AllianceInviteExpireTime { get; set; }
                [JsonProperty(PropertyName = "Member invite cooldown time after a member has left the clan (seconds)")]
                public int InviteCooldownAfterMemberLeave { get; set; }
            }

            public class TeamOptions
            {
                [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
                public bool AllowLeave { get; set; }
                [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
                public bool AllowKick { get; set; }
                [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
                public bool AllowInvite { get; set; }
                [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
                public bool AllowPromote { get; set; }
            }

        }

        public class ColorOptions
        {
            [JsonProperty(PropertyName = "Clan owner color (hex)")]
            public string Owner { get; set; }
            [JsonProperty(PropertyName = "Clan council color (hex)")]
            public string Council { get; set; }
            [JsonProperty(PropertyName = "Clan moderator color (hex)")]
            public string Moderator { get; set; }
            [JsonProperty(PropertyName = "Clan member color (hex)")]
            public string Member { get; set; }
            [JsonProperty(PropertyName = "General text color (hex)")]
            public string TextColor { get; set; }
        }

        public class TagOptions
        {
            [JsonProperty(PropertyName = "Enable clan tags (Display Name)")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Enable clan tags (BetterChat)")]
            public bool EnabledBC { get; set; }
            [JsonProperty(PropertyName = "Tag opening character")]
            public string TagOpen { get; set; }
            [JsonProperty(PropertyName = "Tag closing character")]
            public string TagClose { get; set; }
            [JsonProperty(PropertyName = "Tag color (hex) (BetterChat)")]
            public string TagColor { get; set; }
            [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
            public bool CustomColors { get; set; }
            [JsonProperty(PropertyName = "Custom tag color minimum value (hex)")]
            public string CustomTagColorMin { get; set; }
            [JsonProperty(PropertyName = "Custom tag color maximum value (hex)")]
            public string CustomTagColorMax { get; set; }
            [JsonProperty(PropertyName = "Blacklisted tag colors (hex without the # at the start)")]
            public string[] BlockedTagColors { get; set; }
            [JsonProperty(PropertyName = "Tag size (BetterChat)")]
            public int TagSize { get; set; }
            [JsonProperty(PropertyName = "Tag character limits")]
            public Range TagLength { get; set; }
            [JsonProperty(PropertyName = "Special characters allowed in tags")]
            public string AllowedCharacters { get; set; }
            [JsonProperty(PropertyName = "Words/characters not allowed in tags")]
            public string[] BlockedWords { get; set; }
            [JsonProperty(PropertyName = "Enable Oxide group clan tag colors (BetterChat)")]
            public bool EnabledGroupColors { get; set; }
            [JsonProperty(PropertyName = "Default tag colors per Oxide group (BetterChat)")]
            public Hash<string, string> GroupTagColors { get; set; }
            [JsonProperty(PropertyName = "Reserved clan tags (Tag, List of SteamIDs)")]
            public Hash<string, List<ulong>> ReservedClanTags { get; set; }
        }

        public class CommandOptions
        {
            [JsonProperty(PropertyName = "Ally chat command")]
            public string AllyChatCommand { get; set; }
            [JsonProperty(PropertyName = "Clan chat command")]
            public string ClanChatCommand { get; set; }
            [JsonProperty(PropertyName = "Clan command")]
            public string ClanCommand { get; set; }
            [JsonProperty(PropertyName = "Clan info command")]
            public string ClanInfoCommand { get; set; }
            [JsonProperty(PropertyName = "Ally friendly fire command")]
            public string AFFCommand { get; set; }
            [JsonProperty(PropertyName = "Friendly fire command")]
            public string FFCommand { get; set; }
            [JsonProperty(PropertyName = "Clan ally command")]
            public string ClanAllyCommand { get; set; }
            [JsonProperty(PropertyName = "Clan help command")]
            public string ClanHelpCommand { get; set; }
            [JsonProperty(PropertyName = "Required auth-levels to use admin console command")]
            public AdminAuth Auth { get; set; }
            [JsonProperty(PropertyName = "Clan info options")]
            public ClanInfo ClanInfoOptions { get; set; }
            public class ClanInfo
            {
                [JsonProperty(PropertyName = "Show online/offline players")]
                public bool Players { get; set; }
                [JsonProperty(PropertyName = "Show clan alliances")]
                public bool Alliances { get; set; }
                [JsonProperty(PropertyName = "Show last online time")]
                public bool LastOnline { get; set; }
                [JsonProperty(PropertyName = "Show member count")]
                public bool MemberCount { get; set; }
                [JsonProperty(PropertyName = "Show alliance count")]
                public bool AllianceCount { get; set; }
            }

            public class AdminAuth
            {
                [JsonProperty(PropertyName = "Create clan")]
                public int Create { get; set; }
                [JsonProperty(PropertyName = "Rename clan")]
                public int Rename { get; set; }
                [JsonProperty(PropertyName = "Disband clan")]
                public int Disband { get; set; }
                [JsonProperty(PropertyName = "Invite member to clan")]
                public int Invite { get; set; }
                [JsonProperty(PropertyName = "Kick member from clan")]
                public int Kick { get; set; }
                [JsonProperty(PropertyName = "Promote/Demote member in clan")]
                public int Promote { get; set; }
                [JsonProperty(PropertyName = "Reserve clan tag for member or group")]
                public int Reserve { get; set; }
            }

        }

        public class PermissionOptions
        {
            [JsonProperty(PropertyName = "Use permission for clan info command")]
            public bool UsePermissionClanInfo { get; set; }
            [JsonProperty(PropertyName = "Clan info command permission")]
            public string ClanInfoPermission { get; set; }
            [JsonProperty(PropertyName = "Use permission groups")]
            public bool PermissionGroups { get; set; }
            [JsonProperty(PropertyName = "Permission group prefix")]
            public string PermissionGroupPrefix { get; set; }
            [JsonProperty(PropertyName = "Use permission to create a clan")]
            public bool UsePermissionCreate { get; set; }
            [JsonProperty(PropertyName = "Clan creation permission")]
            public string PermissionCreate { get; set; }
            [JsonProperty(PropertyName = "Use permission to join a clan")]
            public bool UsePermissionJoin { get; set; }
            [JsonProperty(PropertyName = "Clan join permission")]
            public string PermissionJoin { get; set; }
            [JsonProperty(PropertyName = "Use permission to leave a clan")]
            public bool UsePermissionLeave { get; set; }
            [JsonProperty(PropertyName = "Clan leave permission")]
            public string PermissionLeave { get; set; }
            [JsonProperty(PropertyName = "Use permission to disband a clan")]
            public bool UsePermissionDisband { get; set; }
            [JsonProperty(PropertyName = "Clan disband permission")]
            public string PermissionDisband { get; set; }
            [JsonProperty(PropertyName = "Use permission to kick a clan member")]
            public bool UsePermissionKick { get; set; }
            [JsonProperty(PropertyName = "Clan kick permission")]
            public string PermissionKick { get; set; }
        }

        public class PurgeOptions
        {
            [JsonProperty(PropertyName = "Enable clan purging")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
            public int OlderThanDays { get; set; }
            [JsonProperty(PropertyName = "List purged clans in console when purging")]
            public bool ListPurgedClans { get; set; }
            [JsonProperty(PropertyName = "Wipe clans on new map save")]
            public bool WipeOnNewSave { get; set; }
        }

        public class OtherOptions
        {
            [JsonProperty(PropertyName = "Block clan/ally chat when muted")]
            public bool DenyOnMuted { get; set; }
            [JsonProperty(PropertyName = "Log clan and member changes")]
            public bool LogChanges { get; set; }
            [JsonProperty(PropertyName = "Use ProtoBuf data storage")]
            public bool UseProtoStorage { get; set; }
        }

        public class Range
        {
            public int Minimum { get; set; }
            public int Maximum { get; set; }
            public Range();
            public Range(int minimum, int maximum);
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    [ConsoleCommand("clans.convertdata")]
    private void ConvertFromOldData(ConsoleSystem.Arg arg);
    private void ConvertFromOldData(ConsoleSystem.Arg arg, OldDataStructure oldData);
    [Serializable, ProtoContract]
    internal class StoredData
    {
        [ProtoMember(1)]
        public Hash<string, Clan> clans;
        [ProtoMember(2)]
        public int timeSaved;
        [ProtoMember(3)]
        public Hash<ulong, List<string>> playerInvites;
        [JsonIgnore, ProtoIgnore]
        private Hash<ulong, string> playerLookup;
        [JsonIgnore, ProtoIgnore]
        private Hash<string, string> clanLookup;
        internal Clan FindClan(string tag);
        internal Clan FindClanByID(ulong playerId);
        internal Clan FindClanByID(string playerId);
        internal Clan.Member FindMemberByID(ulong playerId);
        internal void RegisterPlayer(ulong playerId, string tag);
        internal void UnregisterPlayer(ulong playerId);
        internal void AddPlayerInvite(ulong target, string tag);
        internal void RevokePlayerInvite(ulong target, string tag);
        internal void OnInviteAccepted(ulong target, string tag);
        internal void OnInviteRejected(ulong target, string tag);
        internal void OnClanRenamed(string oldTag, string newTag);
    }

    private class OldDataStructure
    {
        public Dictionary<string, OldClan> clans;
        public int saveStamp;
        public string lastStorage;
    }

    public class OldClan
    {
        public string tag;
        public string description;
        public string owner;
        public string council;
        public int created;
        public int updated;
        public List<string> moderators;
        public List<string> members;
        public Dictionary<string, int> invites;
        public List<string> clanAlliances;
        public List<string> invitedAllies;
        public List<string> pendingInvites;
    }

    private static string msg(string key, string playerId);
     Dictionary<string, string> Messages;
    private Dictionary<string, string> leetTable;
}

[Serializable, ProtoContract]
public class Clan
{
    [ProtoMember(1), JsonProperty]
    public string Tag { get; set; }
    [ProtoMember(2), JsonProperty]
    public string Description { get; set; }
    [ProtoMember(3), JsonProperty]
    public ulong OwnerID { get; set; }
    [ProtoMember(4), JsonProperty]
    public double CreationTime { get; set; }
    [ProtoMember(5), JsonProperty]
    public double LastOnlineTime { get; set; }
    [ProtoMember(6), JsonProperty]
    public Hash<ulong, Member> ClanMembers { get; set; }
    [ProtoMember(7), JsonProperty]
    public Hash<ulong, MemberInvite> MemberInvites { get; set; }
    [ProtoMember(8), JsonProperty]
    public HashSet<string> Alliances { get; set; }
    [ProtoMember(9), JsonProperty]
    public Hash<string, double> AllianceInvites { get; set; }
    [ProtoMember(10), JsonProperty]
    public HashSet<string> IncomingAlliances { get; set; }
    [ProtoMember(11), JsonProperty]
    public string TagColor { get; set; }
    [ProtoMember(12), JsonProperty]
    public int MemberInviteCooldownTime { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int OnlineCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal ulong CouncilID { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int ModeratorCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int MemberCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int MemberInviteCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int AllianceCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal int AllianceInviteCount { get; set; }
    [JsonIgnore, ProtoIgnore]
    private RelationshipManager.PlayerTeam _playerTeam;
    [JsonIgnore, ProtoIgnore]
    internal RelationshipManager.PlayerTeam PlayerTeam { get; set; }
    public int CountMembersAndAlliances();
    private static ulong FindRandomTeamID { get; set; }
    public Clan();
    public Clan(BasePlayer player, string tag, string description);
    internal void OnPlayerConnected(BasePlayer player);
    internal void OnPlayerDisconnected(BasePlayer player);
    internal bool InvitePlayer(BasePlayer inviter, BasePlayer invitee);
    internal bool JoinClan(BasePlayer player);
    internal bool LeaveClan(BasePlayer player);
    internal bool KickMember(BasePlayer player, ulong targetId);
    internal bool PromotePlayer(BasePlayer promoter, ulong targetId);
    internal bool DemotePlayer(BasePlayer demoter, ulong targetId);
    internal void DisbandClan();
    internal void OnClanDisbanded(string tag);
    internal void OnUnload();
    internal bool IsAlliedClan(string otherClan);
    internal void MarkDirty();
    internal void Broadcast(string message);
    internal void LocalizedBroadcast(string key, object[] args);
    [JsonIgnore, ProtoIgnore]
    private string cachedClanInfo;
    [JsonIgnore, ProtoIgnore]
    private string membersOnline;
    internal void PrintClanInfo(BasePlayer player);
    internal string GetMembersOnline();
    internal bool IsOwner(ulong playerId);
    internal bool IsCouncil(ulong playerId);
    internal bool IsModerator(ulong playerId);
    internal bool IsMember(ulong playerId);
    internal Member GetOwner();
    internal bool HasCouncil();
    internal string GetRoleColor(ulong userID);
    internal string GetRoleColor(Member.MemberRole role);
    [Serializable, ProtoContract]
    public class Member
    {
        [JsonIgnore, ProtoIgnore]
        public BasePlayer Player { get; set; }
        [ProtoMember(1)]
        public string DisplayName { get; set; }
        [ProtoMember(2)]
        public MemberRole Role { get; set; }
        [ProtoMember(3)]
        public bool MemberFFEnabled { get; set; }
        [ProtoMember(4)]
        public bool AllyFFEnabled { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal bool IsConnected { get; set; }
        [JsonIgnore, ProtoIgnore]
        internal float lastFFAttackTime;
        [JsonIgnore, ProtoIgnore]
        internal float lastAFFAttackTime;
        public Member();
        public Member(MemberRole role, Clan clan);
        public Member(MemberRole role, bool memberFFEnabled, bool allyFFEnabled);
        public void OnClanMemberHit(string victimName);
        public void OnAllyMemberHit(string victimName);
    }

    [Serializable, ProtoContract]
    public class MemberInvite
    {
        [ProtoMember(1)]
        public string DisplayName { get; set; }
        [ProtoMember(2)]
        public double ExpiryTime { get; set; }
        public MemberInvite();
        public MemberInvite(BasePlayer player);
    }

    [JsonIgnore, ProtoIgnore]
    private JObject serializedClanObject;
    internal JObject ToJObject();
    internal ulong FindPlayer(string partialNameOrID);
}

[Serializable, ProtoContract]
public class Member
{
    [JsonIgnore, ProtoIgnore]
    public BasePlayer Player { get; set; }
    [ProtoMember(1)]
    public string DisplayName { get; set; }
    [ProtoMember(2)]
    public MemberRole Role { get; set; }
    [ProtoMember(3)]
    public bool MemberFFEnabled { get; set; }
    [ProtoMember(4)]
    public bool AllyFFEnabled { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal bool IsConnected { get; set; }
    [JsonIgnore, ProtoIgnore]
    internal float lastFFAttackTime;
    [JsonIgnore, ProtoIgnore]
    internal float lastAFFAttackTime;
    public Member();
    public Member(MemberRole role, Clan clan);
    public Member(MemberRole role, bool memberFFEnabled, bool allyFFEnabled);
    public void OnClanMemberHit(string victimName);
    public void OnAllyMemberHit(string victimName);
}

[Serializable, ProtoContract]
public class MemberInvite
{
    [ProtoMember(1)]
    public string DisplayName { get; set; }
    [ProtoMember(2)]
    public double ExpiryTime { get; set; }
    public MemberInvite();
    public MemberInvite(BasePlayer player);
}

internal class ConfigData
{
    [JsonProperty(PropertyName = "Clan Options")]
    public ClanOptions Clans { get; set; }
    [JsonProperty(PropertyName = "Command Options")]
    public CommandOptions Commands { get; set; }
    [JsonProperty(PropertyName = "Role Colors")]
    public ColorOptions Colors { get; set; }
    [JsonProperty(PropertyName = "Clan Tag Options")]
    public TagOptions Tags { get; set; }
    [JsonProperty(PropertyName = "Permission Options")]
    public PermissionOptions Permissions { get; set; }
    [JsonProperty(PropertyName = "Purge Options")]
    public PurgeOptions Purge { get; set; }
    [JsonProperty(PropertyName = "Settings")]
    public OtherOptions Options { get; set; }
    public class ClanOptions
    {
        [JsonProperty(PropertyName = "Member limit")]
        public int MemberLimit { get; set; }
        [JsonProperty(PropertyName = "Moderator limit")]
        public int ModeratorLimit { get; set; }
        [JsonProperty(PropertyName = "Allow friendly fire toggle (clan members)")]
        public bool MemberFF { get; set; }
        [JsonProperty(PropertyName = "Enable friendly fire by default (clan members)")]
        public bool DefaultEnableFF { get; set; }
        [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (clan members)")]
        public bool OwnerFF { get; set; }
        [JsonProperty(PropertyName = "Alliance Options")]
        public AllianceOptions Alliance { get; set; }
        [JsonProperty(PropertyName = "Invite Options")]
        public InviteOptions Invites { get; set; }
        [JsonProperty(PropertyName = "Rust Team Options")]
        public TeamOptions Teams { get; set; }
        public class AllianceOptions
        {
            [JsonProperty(PropertyName = "Enable clan alliances")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Alliance limit")]
            public int AllianceLimit { get; set; }
            [JsonProperty(PropertyName = "Count alliance members as clan members")]
            public bool CountAllianceMembers { get; set; }
            [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
            public bool AllyFF { get; set; }
            [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
            public bool DefaultEnableFF { get; set; }
            [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
            public bool OwnerFF { get; set; }
        }

        public class InviteOptions
        {
            [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
            public int MemberInviteLimit { get; set; }
            [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
            public int MemberInviteExpireTime { get; set; }
            [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
            public int AllianceInviteLimit { get; set; }
            [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
            public int AllianceInviteExpireTime { get; set; }
            [JsonProperty(PropertyName = "Member invite cooldown time after a member has left the clan (seconds)")]
            public int InviteCooldownAfterMemberLeave { get; set; }
        }

        public class TeamOptions
        {
            [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
            public bool AllowLeave { get; set; }
            [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
            public bool AllowKick { get; set; }
            [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
            public bool AllowInvite { get; set; }
            [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
            public bool AllowPromote { get; set; }
        }

    }

    public class ColorOptions
    {
        [JsonProperty(PropertyName = "Clan owner color (hex)")]
        public string Owner { get; set; }
        [JsonProperty(PropertyName = "Clan council color (hex)")]
        public string Council { get; set; }
        [JsonProperty(PropertyName = "Clan moderator color (hex)")]
        public string Moderator { get; set; }
        [JsonProperty(PropertyName = "Clan member color (hex)")]
        public string Member { get; set; }
        [JsonProperty(PropertyName = "General text color (hex)")]
        public string TextColor { get; set; }
    }

    public class TagOptions
    {
        [JsonProperty(PropertyName = "Enable clan tags (Display Name)")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Enable clan tags (BetterChat)")]
        public bool EnabledBC { get; set; }
        [JsonProperty(PropertyName = "Tag opening character")]
        public string TagOpen { get; set; }
        [JsonProperty(PropertyName = "Tag closing character")]
        public string TagClose { get; set; }
        [JsonProperty(PropertyName = "Tag color (hex) (BetterChat)")]
        public string TagColor { get; set; }
        [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
        public bool CustomColors { get; set; }
        [JsonProperty(PropertyName = "Custom tag color minimum value (hex)")]
        public string CustomTagColorMin { get; set; }
        [JsonProperty(PropertyName = "Custom tag color maximum value (hex)")]
        public string CustomTagColorMax { get; set; }
        [JsonProperty(PropertyName = "Blacklisted tag colors (hex without the # at the start)")]
        public string[] BlockedTagColors { get; set; }
        [JsonProperty(PropertyName = "Tag size (BetterChat)")]
        public int TagSize { get; set; }
        [JsonProperty(PropertyName = "Tag character limits")]
        public Range TagLength { get; set; }
        [JsonProperty(PropertyName = "Special characters allowed in tags")]
        public string AllowedCharacters { get; set; }
        [JsonProperty(PropertyName = "Words/characters not allowed in tags")]
        public string[] BlockedWords { get; set; }
        [JsonProperty(PropertyName = "Enable Oxide group clan tag colors (BetterChat)")]
        public bool EnabledGroupColors { get; set; }
        [JsonProperty(PropertyName = "Default tag colors per Oxide group (BetterChat)")]
        public Hash<string, string> GroupTagColors { get; set; }
        [JsonProperty(PropertyName = "Reserved clan tags (Tag, List of SteamIDs)")]
        public Hash<string, List<ulong>> ReservedClanTags { get; set; }
    }

    public class CommandOptions
    {
        [JsonProperty(PropertyName = "Ally chat command")]
        public string AllyChatCommand { get; set; }
        [JsonProperty(PropertyName = "Clan chat command")]
        public string ClanChatCommand { get; set; }
        [JsonProperty(PropertyName = "Clan command")]
        public string ClanCommand { get; set; }
        [JsonProperty(PropertyName = "Clan info command")]
        public string ClanInfoCommand { get; set; }
        [JsonProperty(PropertyName = "Ally friendly fire command")]
        public string AFFCommand { get; set; }
        [JsonProperty(PropertyName = "Friendly fire command")]
        public string FFCommand { get; set; }
        [JsonProperty(PropertyName = "Clan ally command")]
        public string ClanAllyCommand { get; set; }
        [JsonProperty(PropertyName = "Clan help command")]
        public string ClanHelpCommand { get; set; }
        [JsonProperty(PropertyName = "Required auth-levels to use admin console command")]
        public AdminAuth Auth { get; set; }
        [JsonProperty(PropertyName = "Clan info options")]
        public ClanInfo ClanInfoOptions { get; set; }
        public class ClanInfo
        {
            [JsonProperty(PropertyName = "Show online/offline players")]
            public bool Players { get; set; }
            [JsonProperty(PropertyName = "Show clan alliances")]
            public bool Alliances { get; set; }
            [JsonProperty(PropertyName = "Show last online time")]
            public bool LastOnline { get; set; }
            [JsonProperty(PropertyName = "Show member count")]
            public bool MemberCount { get; set; }
            [JsonProperty(PropertyName = "Show alliance count")]
            public bool AllianceCount { get; set; }
        }

        public class AdminAuth
        {
            [JsonProperty(PropertyName = "Create clan")]
            public int Create { get; set; }
            [JsonProperty(PropertyName = "Rename clan")]
            public int Rename { get; set; }
            [JsonProperty(PropertyName = "Disband clan")]
            public int Disband { get; set; }
            [JsonProperty(PropertyName = "Invite member to clan")]
            public int Invite { get; set; }
            [JsonProperty(PropertyName = "Kick member from clan")]
            public int Kick { get; set; }
            [JsonProperty(PropertyName = "Promote/Demote member in clan")]
            public int Promote { get; set; }
            [JsonProperty(PropertyName = "Reserve clan tag for member or group")]
            public int Reserve { get; set; }
        }

    }

    public class PermissionOptions
    {
        [JsonProperty(PropertyName = "Use permission for clan info command")]
        public bool UsePermissionClanInfo { get; set; }
        [JsonProperty(PropertyName = "Clan info command permission")]
        public string ClanInfoPermission { get; set; }
        [JsonProperty(PropertyName = "Use permission groups")]
        public bool PermissionGroups { get; set; }
        [JsonProperty(PropertyName = "Permission group prefix")]
        public string PermissionGroupPrefix { get; set; }
        [JsonProperty(PropertyName = "Use permission to create a clan")]
        public bool UsePermissionCreate { get; set; }
        [JsonProperty(PropertyName = "Clan creation permission")]
        public string PermissionCreate { get; set; }
        [JsonProperty(PropertyName = "Use permission to join a clan")]
        public bool UsePermissionJoin { get; set; }
        [JsonProperty(PropertyName = "Clan join permission")]
        public string PermissionJoin { get; set; }
        [JsonProperty(PropertyName = "Use permission to leave a clan")]
        public bool UsePermissionLeave { get; set; }
        [JsonProperty(PropertyName = "Clan leave permission")]
        public string PermissionLeave { get; set; }
        [JsonProperty(PropertyName = "Use permission to disband a clan")]
        public bool UsePermissionDisband { get; set; }
        [JsonProperty(PropertyName = "Clan disband permission")]
        public string PermissionDisband { get; set; }
        [JsonProperty(PropertyName = "Use permission to kick a clan member")]
        public bool UsePermissionKick { get; set; }
        [JsonProperty(PropertyName = "Clan kick permission")]
        public string PermissionKick { get; set; }
    }

    public class PurgeOptions
    {
        [JsonProperty(PropertyName = "Enable clan purging")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
        public int OlderThanDays { get; set; }
        [JsonProperty(PropertyName = "List purged clans in console when purging")]
        public bool ListPurgedClans { get; set; }
        [JsonProperty(PropertyName = "Wipe clans on new map save")]
        public bool WipeOnNewSave { get; set; }
    }

    public class OtherOptions
    {
        [JsonProperty(PropertyName = "Block clan/ally chat when muted")]
        public bool DenyOnMuted { get; set; }
        [JsonProperty(PropertyName = "Log clan and member changes")]
        public bool LogChanges { get; set; }
        [JsonProperty(PropertyName = "Use ProtoBuf data storage")]
        public bool UseProtoStorage { get; set; }
    }

    public class Range
    {
        public int Minimum { get; set; }
        public int Maximum { get; set; }
        public Range();
        public Range(int minimum, int maximum);
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class ClanOptions
{
    [JsonProperty(PropertyName = "Member limit")]
    public int MemberLimit { get; set; }
    [JsonProperty(PropertyName = "Moderator limit")]
    public int ModeratorLimit { get; set; }
    [JsonProperty(PropertyName = "Allow friendly fire toggle (clan members)")]
    public bool MemberFF { get; set; }
    [JsonProperty(PropertyName = "Enable friendly fire by default (clan members)")]
    public bool DefaultEnableFF { get; set; }
    [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (clan members)")]
    public bool OwnerFF { get; set; }
    [JsonProperty(PropertyName = "Alliance Options")]
    public AllianceOptions Alliance { get; set; }
    [JsonProperty(PropertyName = "Invite Options")]
    public InviteOptions Invites { get; set; }
    [JsonProperty(PropertyName = "Rust Team Options")]
    public TeamOptions Teams { get; set; }
    public class AllianceOptions
    {
        [JsonProperty(PropertyName = "Enable clan alliances")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Alliance limit")]
        public int AllianceLimit { get; set; }
        [JsonProperty(PropertyName = "Count alliance members as clan members")]
        public bool CountAllianceMembers { get; set; }
        [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
        public bool AllyFF { get; set; }
        [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
        public bool DefaultEnableFF { get; set; }
        [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
        public bool OwnerFF { get; set; }
    }

    public class InviteOptions
    {
        [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
        public int MemberInviteLimit { get; set; }
        [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
        public int MemberInviteExpireTime { get; set; }
        [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
        public int AllianceInviteLimit { get; set; }
        [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
        public int AllianceInviteExpireTime { get; set; }
        [JsonProperty(PropertyName = "Member invite cooldown time after a member has left the clan (seconds)")]
        public int InviteCooldownAfterMemberLeave { get; set; }
    }

    public class TeamOptions
    {
        [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
        public bool AllowLeave { get; set; }
        [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
        public bool AllowKick { get; set; }
        [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
        public bool AllowInvite { get; set; }
        [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
        public bool AllowPromote { get; set; }
    }

}

public class AllianceOptions
{
    [JsonProperty(PropertyName = "Enable clan alliances")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Alliance limit")]
    public int AllianceLimit { get; set; }
    [JsonProperty(PropertyName = "Count alliance members as clan members")]
    public bool CountAllianceMembers { get; set; }
    [JsonProperty(PropertyName = "Allow friendly fire toggle (allied clans)")]
    public bool AllyFF { get; set; }
    [JsonProperty(PropertyName = "Enable friendly fire by default (allied clans)")]
    public bool DefaultEnableFF { get; set; }
    [JsonProperty(PropertyName = "Only allow clan owner and council to toggle friendly fire (allied clans)")]
    public bool OwnerFF { get; set; }
}

public class InviteOptions
{
    [JsonProperty(PropertyName = "Maximum allowed member invites at any given time")]
    public int MemberInviteLimit { get; set; }
    [JsonProperty(PropertyName = "Member invite expiry time (seconds)")]
    public int MemberInviteExpireTime { get; set; }
    [JsonProperty(PropertyName = "Maximum allowed alliance invites at any given time")]
    public int AllianceInviteLimit { get; set; }
    [JsonProperty(PropertyName = "Alliance invite expiry time (seconds)")]
    public int AllianceInviteExpireTime { get; set; }
    [JsonProperty(PropertyName = "Member invite cooldown time after a member has left the clan (seconds)")]
    public int InviteCooldownAfterMemberLeave { get; set; }
}

public class TeamOptions
{
    [JsonProperty(PropertyName = "Automatically create and manage Rust team's for each clan")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Allow players to leave their clan by using Rust's leave team button")]
    public bool AllowLeave { get; set; }
    [JsonProperty(PropertyName = "Allow players to kick members from their clan using Rust's kick member button")]
    public bool AllowKick { get; set; }
    [JsonProperty(PropertyName = "Allow players to invite other players to their clan via Rust's team invite system")]
    public bool AllowInvite { get; set; }
    [JsonProperty(PropertyName = "Allow players to promote other clan members via Rust's team promote button")]
    public bool AllowPromote { get; set; }
}

public class ColorOptions
{
    [JsonProperty(PropertyName = "Clan owner color (hex)")]
    public string Owner { get; set; }
    [JsonProperty(PropertyName = "Clan council color (hex)")]
    public string Council { get; set; }
    [JsonProperty(PropertyName = "Clan moderator color (hex)")]
    public string Moderator { get; set; }
    [JsonProperty(PropertyName = "Clan member color (hex)")]
    public string Member { get; set; }
    [JsonProperty(PropertyName = "General text color (hex)")]
    public string TextColor { get; set; }
}

public class TagOptions
{
    [JsonProperty(PropertyName = "Enable clan tags (Display Name)")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Enable clan tags (BetterChat)")]
    public bool EnabledBC { get; set; }
    [JsonProperty(PropertyName = "Tag opening character")]
    public string TagOpen { get; set; }
    [JsonProperty(PropertyName = "Tag closing character")]
    public string TagClose { get; set; }
    [JsonProperty(PropertyName = "Tag color (hex) (BetterChat)")]
    public string TagColor { get; set; }
    [JsonProperty(PropertyName = "Allow clan leaders to set custom tag colors (BetterChat only)")]
    public bool CustomColors { get; set; }
    [JsonProperty(PropertyName = "Custom tag color minimum value (hex)")]
    public string CustomTagColorMin { get; set; }
    [JsonProperty(PropertyName = "Custom tag color maximum value (hex)")]
    public string CustomTagColorMax { get; set; }
    [JsonProperty(PropertyName = "Blacklisted tag colors (hex without the # at the start)")]
    public string[] BlockedTagColors { get; set; }
    [JsonProperty(PropertyName = "Tag size (BetterChat)")]
    public int TagSize { get; set; }
    [JsonProperty(PropertyName = "Tag character limits")]
    public Range TagLength { get; set; }
    [JsonProperty(PropertyName = "Special characters allowed in tags")]
    public string AllowedCharacters { get; set; }
    [JsonProperty(PropertyName = "Words/characters not allowed in tags")]
    public string[] BlockedWords { get; set; }
    [JsonProperty(PropertyName = "Enable Oxide group clan tag colors (BetterChat)")]
    public bool EnabledGroupColors { get; set; }
    [JsonProperty(PropertyName = "Default tag colors per Oxide group (BetterChat)")]
    public Hash<string, string> GroupTagColors { get; set; }
    [JsonProperty(PropertyName = "Reserved clan tags (Tag, List of SteamIDs)")]
    public Hash<string, List<ulong>> ReservedClanTags { get; set; }
}

public class CommandOptions
{
    [JsonProperty(PropertyName = "Ally chat command")]
    public string AllyChatCommand { get; set; }
    [JsonProperty(PropertyName = "Clan chat command")]
    public string ClanChatCommand { get; set; }
    [JsonProperty(PropertyName = "Clan command")]
    public string ClanCommand { get; set; }
    [JsonProperty(PropertyName = "Clan info command")]
    public string ClanInfoCommand { get; set; }
    [JsonProperty(PropertyName = "Ally friendly fire command")]
    public string AFFCommand { get; set; }
    [JsonProperty(PropertyName = "Friendly fire command")]
    public string FFCommand { get; set; }
    [JsonProperty(PropertyName = "Clan ally command")]
    public string ClanAllyCommand { get; set; }
    [JsonProperty(PropertyName = "Clan help command")]
    public string ClanHelpCommand { get; set; }
    [JsonProperty(PropertyName = "Required auth-levels to use admin console command")]
    public AdminAuth Auth { get; set; }
    [JsonProperty(PropertyName = "Clan info options")]
    public ClanInfo ClanInfoOptions { get; set; }
    public class ClanInfo
    {
        [JsonProperty(PropertyName = "Show online/offline players")]
        public bool Players { get; set; }
        [JsonProperty(PropertyName = "Show clan alliances")]
        public bool Alliances { get; set; }
        [JsonProperty(PropertyName = "Show last online time")]
        public bool LastOnline { get; set; }
        [JsonProperty(PropertyName = "Show member count")]
        public bool MemberCount { get; set; }
        [JsonProperty(PropertyName = "Show alliance count")]
        public bool AllianceCount { get; set; }
    }

    public class AdminAuth
    {
        [JsonProperty(PropertyName = "Create clan")]
        public int Create { get; set; }
        [JsonProperty(PropertyName = "Rename clan")]
        public int Rename { get; set; }
        [JsonProperty(PropertyName = "Disband clan")]
        public int Disband { get; set; }
        [JsonProperty(PropertyName = "Invite member to clan")]
        public int Invite { get; set; }
        [JsonProperty(PropertyName = "Kick member from clan")]
        public int Kick { get; set; }
        [JsonProperty(PropertyName = "Promote/Demote member in clan")]
        public int Promote { get; set; }
        [JsonProperty(PropertyName = "Reserve clan tag for member or group")]
        public int Reserve { get; set; }
    }

}

public class ClanInfo
{
    [JsonProperty(PropertyName = "Show online/offline players")]
    public bool Players { get; set; }
    [JsonProperty(PropertyName = "Show clan alliances")]
    public bool Alliances { get; set; }
    [JsonProperty(PropertyName = "Show last online time")]
    public bool LastOnline { get; set; }
    [JsonProperty(PropertyName = "Show member count")]
    public bool MemberCount { get; set; }
    [JsonProperty(PropertyName = "Show alliance count")]
    public bool AllianceCount { get; set; }
}

public class AdminAuth
{
    [JsonProperty(PropertyName = "Create clan")]
    public int Create { get; set; }
    [JsonProperty(PropertyName = "Rename clan")]
    public int Rename { get; set; }
    [JsonProperty(PropertyName = "Disband clan")]
    public int Disband { get; set; }
    [JsonProperty(PropertyName = "Invite member to clan")]
    public int Invite { get; set; }
    [JsonProperty(PropertyName = "Kick member from clan")]
    public int Kick { get; set; }
    [JsonProperty(PropertyName = "Promote/Demote member in clan")]
    public int Promote { get; set; }
    [JsonProperty(PropertyName = "Reserve clan tag for member or group")]
    public int Reserve { get; set; }
}

public class PermissionOptions
{
    [JsonProperty(PropertyName = "Use permission for clan info command")]
    public bool UsePermissionClanInfo { get; set; }
    [JsonProperty(PropertyName = "Clan info command permission")]
    public string ClanInfoPermission { get; set; }
    [JsonProperty(PropertyName = "Use permission groups")]
    public bool PermissionGroups { get; set; }
    [JsonProperty(PropertyName = "Permission group prefix")]
    public string PermissionGroupPrefix { get; set; }
    [JsonProperty(PropertyName = "Use permission to create a clan")]
    public bool UsePermissionCreate { get; set; }
    [JsonProperty(PropertyName = "Clan creation permission")]
    public string PermissionCreate { get; set; }
    [JsonProperty(PropertyName = "Use permission to join a clan")]
    public bool UsePermissionJoin { get; set; }
    [JsonProperty(PropertyName = "Clan join permission")]
    public string PermissionJoin { get; set; }
    [JsonProperty(PropertyName = "Use permission to leave a clan")]
    public bool UsePermissionLeave { get; set; }
    [JsonProperty(PropertyName = "Clan leave permission")]
    public string PermissionLeave { get; set; }
    [JsonProperty(PropertyName = "Use permission to disband a clan")]
    public bool UsePermissionDisband { get; set; }
    [JsonProperty(PropertyName = "Clan disband permission")]
    public string PermissionDisband { get; set; }
    [JsonProperty(PropertyName = "Use permission to kick a clan member")]
    public bool UsePermissionKick { get; set; }
    [JsonProperty(PropertyName = "Clan kick permission")]
    public string PermissionKick { get; set; }
}

public class PurgeOptions
{
    [JsonProperty(PropertyName = "Enable clan purging")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Purge clans that havent been online for x amount of day")]
    public int OlderThanDays { get; set; }
    [JsonProperty(PropertyName = "List purged clans in console when purging")]
    public bool ListPurgedClans { get; set; }
    [JsonProperty(PropertyName = "Wipe clans on new map save")]
    public bool WipeOnNewSave { get; set; }
}

public class OtherOptions
{
    [JsonProperty(PropertyName = "Block clan/ally chat when muted")]
    public bool DenyOnMuted { get; set; }
    [JsonProperty(PropertyName = "Log clan and member changes")]
    public bool LogChanges { get; set; }
    [JsonProperty(PropertyName = "Use ProtoBuf data storage")]
    public bool UseProtoStorage { get; set; }
}

public class Range
{
    public int Minimum { get; set; }
    public int Maximum { get; set; }
    public Range();
    public Range(int minimum, int maximum);
}

[Serializable, ProtoContract]
internal class StoredData
{
    [ProtoMember(1)]
    public Hash<string, Clan> clans;
    [ProtoMember(2)]
    public int timeSaved;
    [ProtoMember(3)]
    public Hash<ulong, List<string>> playerInvites;
    [JsonIgnore, ProtoIgnore]
    private Hash<ulong, string> playerLookup;
    [JsonIgnore, ProtoIgnore]
    private Hash<string, string> clanLookup;
    internal Clan FindClan(string tag);
    internal Clan FindClanByID(ulong playerId);
    internal Clan FindClanByID(string playerId);
    internal Clan.Member FindMemberByID(ulong playerId);
    internal void RegisterPlayer(ulong playerId, string tag);
    internal void UnregisterPlayer(ulong playerId);
    internal void AddPlayerInvite(ulong target, string tag);
    internal void RevokePlayerInvite(ulong target, string tag);
    internal void OnInviteAccepted(ulong target, string tag);
    internal void OnInviteRejected(ulong target, string tag);
    internal void OnClanRenamed(string oldTag, string newTag);
}

private class OldDataStructure
{
    public Dictionary<string, OldClan> clans;
    public int saveStamp;
    public string lastStorage;
}

public class OldClan
{
    public string tag;
    public string description;
    public string owner;
    public string council;
    public int created;
    public int updated;
    public List<string> moderators;
    public List<string> members;
    public Dictionary<string, int> invites;
    public List<string> clanAlliances;
    public List<string> invitedAllies;
    public List<string> pendingInvites;
}


```

---

## ClansTop

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Clans Top", "Mevent", "1.0.1")]
public class ClansTop : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private static ClansTop _instance;
    private const string Layer;
    private string _topJson;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Interface Settings")]
        public InterfaceSettings UI;
    }

    private class InterfaceSettings
    {
        [JsonProperty(PropertyName = "Display type (Overlay/Hud)")]
        public string DisplayType;
        [JsonProperty(PropertyName = "Max clans on string")]
        public int MaxClansOnString;
        [JsonProperty(PropertyName = "Colors", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<IColor> Colors;
        [JsonProperty(PropertyName = "Background Color")]
        public IColor BackgroundColor;
        [JsonProperty(PropertyName = "Bottom Indent")]
        public float BottomIndent;
        [JsonProperty(PropertyName = "Side Indent")]
        public float SideIndent;
        [JsonProperty(PropertyName = "Width")]
        public float Width;
        [JsonProperty(PropertyName = "Height")]
        public float Height;
        [JsonProperty(PropertyName = "Margin")]
        public float Margin;
        [JsonProperty(PropertyName = "Number Text Size")]
        public int NumberSize;
        [JsonProperty(PropertyName = "Text Size")]
        public int TextSize;
        [JsonProperty(PropertyName = "TextAlign")]
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor TextAlign;
        [JsonProperty(PropertyName = "Show Score")]
        public bool ShowScore;
        [JsonProperty(PropertyName = "Score Format")]
        public string ScoreFormat;
        [JsonProperty(PropertyName = "Use value abbreviation?")]
        public bool ValueAbbreviation;
        public string GetScore(string clanTag);
    }

    private class IColor
    {
        [JsonProperty(PropertyName = "HEX")]
        public string Hex;
        [JsonProperty(PropertyName = "Opacity (0 - 100)")]
        public readonly float Alpha;
        [JsonIgnore]
        private string _color;
        [JsonIgnore]
        public string Get { get; set; }
        private string GetColor();
        public IColor();
        public IColor(string hex, float alpha);
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnClanTopUpdated();
    private void UpdateUI(string json);
    private void UpdateUI(BasePlayer player, string json);
    private string GenerateTopUI(IReadOnlyDictionary<int, string> clans);
    private string GetValue(float value);
    private Dictionary<int, string> CLANS_GetTopClans();
    private float CLANS_GetClanScore(string clanTag);
    private void StartUpdate();
    private void StartUpdate(BasePlayer player);
    private void UpdateTopJson();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Interface Settings")]
    public InterfaceSettings UI;
}

private class InterfaceSettings
{
    [JsonProperty(PropertyName = "Display type (Overlay/Hud)")]
    public string DisplayType;
    [JsonProperty(PropertyName = "Max clans on string")]
    public int MaxClansOnString;
    [JsonProperty(PropertyName = "Colors", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<IColor> Colors;
    [JsonProperty(PropertyName = "Background Color")]
    public IColor BackgroundColor;
    [JsonProperty(PropertyName = "Bottom Indent")]
    public float BottomIndent;
    [JsonProperty(PropertyName = "Side Indent")]
    public float SideIndent;
    [JsonProperty(PropertyName = "Width")]
    public float Width;
    [JsonProperty(PropertyName = "Height")]
    public float Height;
    [JsonProperty(PropertyName = "Margin")]
    public float Margin;
    [JsonProperty(PropertyName = "Number Text Size")]
    public int NumberSize;
    [JsonProperty(PropertyName = "Text Size")]
    public int TextSize;
    [JsonProperty(PropertyName = "TextAlign")]
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor TextAlign;
    [JsonProperty(PropertyName = "Show Score")]
    public bool ShowScore;
    [JsonProperty(PropertyName = "Score Format")]
    public string ScoreFormat;
    [JsonProperty(PropertyName = "Use value abbreviation?")]
    public bool ValueAbbreviation;
    public string GetScore(string clanTag);
}

private class IColor
{
    [JsonProperty(PropertyName = "HEX")]
    public string Hex;
    [JsonProperty(PropertyName = "Opacity (0 - 100)")]
    public readonly float Alpha;
    [JsonIgnore]
    private string _color;
    [JsonIgnore]
    public string Get { get; set; }
    private string GetColor();
    public IColor();
    public IColor(string hex, float alpha);
}


```

---

## ClansUI

```csharp
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("ClansUI", "BGRM", "0.1.43", ResourceId = 0)]
 class ClansUI : RustPlugin
{
    [PluginReference]
     Clans Clans;
    const string ClanUI;
    const string ClanBG;
    private int maxMembers;
    private int maxAllies;
    private bool canToggleFF;
    private bool alliesEnabled;
     SortedList<string, string> playerList;
    private List<ulong> openMenu;
     void Loaded();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
    private string RemoveTag(string str);
    private string TrimToSize(string str, int size);
     class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public string Color(string hexColor, float alpha);
    }

     class PlayerInfo
    {
        public string playerId;
        public string playerName;
    }

     void LoadClanMenu(BasePlayer player, Clans.Clan clan, int page);
     void MembersMenu(BasePlayer player, Clans.Clan clan, int page);
     void AddClanMember(CuiElementContainer container, string playerId, string memberId, int number, int page, string status, bool isOwner, bool isPlayer);
     void MemberSelection(BasePlayer player, Clans.Clan clan, int page, bool isRemoving);
     void MemberButton(CuiElementContainer container, string name, string playerId, int number, bool isRemoving);
     void AllianceMenu(BasePlayer player, Clans.Clan clan, AllianceType type, int page);
     void AddAlliance(CuiElementContainer container, AllianceType type, string playerId, string clanTag, int number, int page);
     void AllySelection(BasePlayer player, Clans.Clan clan, int page);
     void ClanAllyButton(CuiElementContainer container, string clanTag, int number);
     void ConfirmDisband(BasePlayer player, Clans.Clan clan);
    private float[] CalculateEntryPos(int number);
    [ConsoleCommand("ClansUIToggle")]
     void ccmdClansUIToggle(ConsoleSystem.Arg arg);
    [ConsoleCommand("ClansUI")]
     void ccmdClansUI(ConsoleSystem.Arg arg);
     void cmdClanUI(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class MenuActivation
    {
        public string CommandToOpen { get; set; }
    }

     class UIColor
    {
        public string HexColor { get; set; }
        public float Opacity { get; set; }
    }

     class UIColors
    {
        public UIColor Background { get; set; }
        public UIColor TitlePanel { get; set; }
        public UIColor TextPanel { get; set; }
        public UIColor CloseButton { get; set; }
        public UIColor ButtonColor { get; set; }
    }

     class UISize
    {
        public float X_Position { get; set; }
        public float X_Dimension { get; set; }
        public float Y_Position { get; set; }
        public float Y_Dimension { get; set; }
    }

     class CommandButton
    {
        public string Name { get; set; }
        public string Command { get; set; }
        public string Arg { get; set; }
    }

     class ConfigData
    {
        public List<CommandButton> Commands { get; set; }
        public UIColors UIColors { get; set; }
        public UISize UISize { get; set; }
        public MenuActivation MenuActivation { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

 class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public string Color(string hexColor, float alpha);
}

 class PlayerInfo
{
    public string playerId;
    public string playerName;
}

 class MenuActivation
{
    public string CommandToOpen { get; set; }
}

 class UIColor
{
    public string HexColor { get; set; }
    public float Opacity { get; set; }
}

 class UIColors
{
    public UIColor Background { get; set; }
    public UIColor TitlePanel { get; set; }
    public UIColor TextPanel { get; set; }
    public UIColor CloseButton { get; set; }
    public UIColor ButtonColor { get; set; }
}

 class UISize
{
    public float X_Position { get; set; }
    public float X_Dimension { get; set; }
    public float Y_Position { get; set; }
    public float Y_Dimension { get; set; }
}

 class CommandButton
{
    public string Name { get; set; }
    public string Command { get; set; }
    public string Arg { get; set; }
}

 class ConfigData
{
    public List<CommandButton> Commands { get; set; }
    public UIColors UIColors { get; set; }
    public UISize UISize { get; set; }
    public MenuActivation MenuActivation { get; set; }
}


```

---

## ClansUI (1)

```csharp
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("ClansUI", "BGRM", "0.1.43", ResourceId = 0)]
 class ClansUI : RustPlugin
{
    [PluginReference]
     Clans Clans;
    const string ClanUI;
    const string ClanBG;
    private int maxMembers;
    private int maxAllies;
    private bool canToggleFF;
    private bool alliesEnabled;
     SortedList<string, string> playerList;
    private List<ulong> openMenu;
     void Loaded();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
    private string RemoveTag(string str);
    private string TrimToSize(string str, int size);
     class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public string Color(string hexColor, float alpha);
    }

     class PlayerInfo
    {
        public string playerId;
        public string playerName;
    }

     void LoadClanMenu(BasePlayer player, Clans.Clan clan, int page);
     void MembersMenu(BasePlayer player, Clans.Clan clan, int page);
     void AddClanMember(CuiElementContainer container, string playerId, string memberId, int number, int page, string status, bool isOwner, bool isPlayer);
     void MemberSelection(BasePlayer player, Clans.Clan clan, int page, bool isRemoving);
     void MemberButton(CuiElementContainer container, string name, string playerId, int number, bool isRemoving);
     void AllianceMenu(BasePlayer player, Clans.Clan clan, AllianceType type, int page);
     void AddAlliance(CuiElementContainer container, AllianceType type, string playerId, string clanTag, int number, int page);
     void AllySelection(BasePlayer player, Clans.Clan clan, int page);
     void ClanAllyButton(CuiElementContainer container, string clanTag, int number);
     void ConfirmDisband(BasePlayer player, Clans.Clan clan);
    private float[] CalculateEntryPos(int number);
    [ConsoleCommand("ClansUIToggle")]
     void ccmdClansUIToggle(ConsoleSystem.Arg arg);
    [ConsoleCommand("ClansUI")]
     void ccmdClansUI(ConsoleSystem.Arg arg);
     void cmdClanUI(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class MenuActivation
    {
        public string CommandToOpen { get; set; }
    }

     class UIColor
    {
        public string HexColor { get; set; }
        public float Opacity { get; set; }
    }

     class UIColors
    {
        public UIColor Background { get; set; }
        public UIColor TitlePanel { get; set; }
        public UIColor TextPanel { get; set; }
        public UIColor CloseButton { get; set; }
        public UIColor ButtonColor { get; set; }
    }

     class UISize
    {
        public float X_Position { get; set; }
        public float X_Dimension { get; set; }
        public float Y_Position { get; set; }
        public float Y_Dimension { get; set; }
    }

     class CommandButton
    {
        public string Name { get; set; }
        public string Command { get; set; }
        public string Arg { get; set; }
    }

     class ConfigData
    {
        public List<CommandButton> Commands { get; set; }
        public UIColors UIColors { get; set; }
        public UISize UISize { get; set; }
        public MenuActivation MenuActivation { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

 class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor, string parent);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public string Color(string hexColor, float alpha);
}

 class PlayerInfo
{
    public string playerId;
    public string playerName;
}

 class MenuActivation
{
    public string CommandToOpen { get; set; }
}

 class UIColor
{
    public string HexColor { get; set; }
    public float Opacity { get; set; }
}

 class UIColors
{
    public UIColor Background { get; set; }
    public UIColor TitlePanel { get; set; }
    public UIColor TextPanel { get; set; }
    public UIColor CloseButton { get; set; }
    public UIColor ButtonColor { get; set; }
}

 class UISize
{
    public float X_Position { get; set; }
    public float X_Dimension { get; set; }
    public float Y_Position { get; set; }
    public float Y_Dimension { get; set; }
}

 class CommandButton
{
    public string Name { get; set; }
    public string Command { get; set; }
    public string Arg { get; set; }
}

 class ConfigData
{
    public List<CommandButton> Commands { get; set; }
    public UIColors UIColors { get; set; }
    public UISize UISize { get; set; }
    public MenuActivation MenuActivation { get; set; }
}


```

---

## Clean

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Obj = UnityEngine.Object;

Oxide.Plugins
[Info("Clean", "Frizen", "1.0.0")]
public class Clean : CovalencePlugin
{
     void OnServerInitialized();
     void Unload();
     void DoClean();
}


```

---

## CleanUp

```csharp
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("CleanUp", "Reneb & SPooCK", "2.0.2")]
public class CleanUp : RustPlugin
{
    private int constructionColl;
     void Loaded();
     void OnServerInitialized();
     Dictionary<string, string> displaynameToShortname;
    private void InitializeTable();
     bool shouldRemove(Deployable deployable, bool forceRemove, float eraseRadius);
     bool hasAccess(BasePlayer player);
    [ConsoleCommand("cc.clean")]
     void cmdConsoleClean(ConsoleSystem.Arg arg);
    [ConsoleCommand("cc.count")]
     void cmdConsoleCount(ConsoleSystem.Arg arg);
    [ChatCommand("clean")]
     void cmdChatClean(BasePlayer player, string command, string[] args);
    [ChatCommand("count")]
     void cmdChatCount(BasePlayer player, string command, string[] args);
}


```

---

## ClearDoors

```csharp
using System;
using UnityEngine;
using System.Globalization;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Удаление дверей с РТ", "poof.", "1.0.0")]
 class ClearDoors : RustPlugin
{
    private void clearallchest();
     void OnServerInitialized();
    private static string HexToCuiColor(string hex);
    private void GetConfig(string menu, string Key, T var);
}


```

---

## ClearNight

```csharp
using Facepunch;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using ProtoBuf;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Clear Night", "Clearshot", "2.3.5")]
[Description("Always bright nights")]
 class ClearNight : CovalencePlugin
{
    private PluginConfig _config;
    private EnvSync _envSync;
    private List<DateTime> _fullMoonDates;
    private Dictionary<string, string> _weatherSync;
    private DateTime _date;
    private Climate _climate;
    private int _current;
    private bool _playSound;
    private bool IsDay;
    private bool IsNight;
    [PluginReference("NightVision")]
     Plugin NightVisionRef;
     VersionNumber NightVisionMinVersion;
     void OnServerInitialized();
     void Unload();
     void OnDay();
     void OnSunset();
    private void UpdatePlayerDateTime(Connection connection, DateTime date);
    private void UpdatePlayerWeather(Dictionary<string, string> weatherVars);
    private void UpdateCelestials();
    private void SyncWeather();
    [Command("clearnight.debug")]
    private void DebugCommand(Core.Libraries.Covalence.IPlayer player, string command, string[] args);
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadConfig();
    private class PluginConfig
    {
        public string[] fullMoonDates;
        public Dictionary<string, string> weatherAtNight;
        public bool syncWeather;
        public bool randomizeDates;
        public bool freezeMoon;
        public bool playSoundAtSunset;
        public string sound;
        public float syncInterval;
    }

}

private class PluginConfig
{
    public string[] fullMoonDates;
    public Dictionary<string, string> weatherAtNight;
    public bool syncWeather;
    public bool randomizeDates;
    public bool freezeMoon;
    public bool playSoundAtSunset;
    public string sound;
    public float syncInterval;
}


```

---

## CleverEvent

```csharp
using System;
using System.Collections;
using System.Globalization;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using System.Runtime.CompilerServices;
using System.Runtime.InteropServices;
using Apex;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("CleverEvent", "Hougan, Ryamkk", "1.0.4")]
public class CleverEvent : RustPlugin
{
    [PluginReference]
    private Plugin StoreHandler;
    [PluginReference]
    private Plugin ImageLibrary;
    public ulong senderID;
    public string header;
    public string prefixcolor;
    public string prefixsize;
    public int QuestionAmount;
    public int TimeToAnswer;
    public int AnnounceTime;
    public int FinishPrize;
    public int MinimalPlayers;
    private void LoadDefaultConfig();
    private class Player
    {
        [JsonProperty("Текущий баланс игрока")]
        public int Balance;
    }

    [JsonProperty("Список игроков и их баланса")]
    private Dictionary<ulong, Player> playerBalance;
    [JsonProperty("Список открытых клеверов")]
    private List<ulong> OpenList;
    [JsonProperty("Текущая игра")]
    private Game currentGame;
    private class Game
    {
        [JsonProperty("Время начала игры")]
        public double StartTimeStamp;
        [JsonProperty("Текущий номер вопроса")]
        public int CurrentQuestionIndex;
        [JsonProperty("Текущий призовой фонд")]
        public int FinishPrize;
        [JsonProperty("Время на ответ")]
        public int TimeToAnswer;
        [JsonProperty("Текущий таймер")]
        public int NextTimerAmount;
        [JsonProperty("Сфомированный список вопросов")]
        public List<Question> Questions;
        [JsonProperty("Правильные ответы на вопросы")]
        public Dictionary<int, int> PercentsAnswers;
        [JsonProperty("Ответы игроков на каждый вопрос")]
        public List<Dictionary<BasePlayer, bool>> PlayerAnswers;
        public Game(double startTime, int finishPrize, int timeToAnswer, List<Question> questionList);
        public Question CurrentQuestion();
        public bool IsLoose(BasePlayer player);
        public double LeftTime();
        public string LeftTimeString();
        public string LeftTimeShortString();
    }

    private static DateTime Epoch;
    public static long GetTimeStamp();
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
    private class Question
    {
        [JsonProperty("Текст вопроса")]
        public string Text;
        [JsonProperty("Варианты ответов")]
        public Dictionary<string, bool> Answers;
    }

    [JsonProperty("Список вопросов составляющих игру")]
    private List<Question> questionList;
    private string MenuLayer;
    private string AcceptMenu;
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
    private string PrepareGame(int questionAmount, int finishPrize);
    private void StartGame();
    private void SwitchQuestion();
    private void CountResults();
    private void AnnounceClever();
    private void ShowResult(BasePlayer player);
    private void DrawQuestion(BasePlayer player);
    private void UpdatetimerNext(BasePlayer player);
    private void UpdateTimerStart(BasePlayer player);
    private void DrawMenu(BasePlayer player);
    private void CreateQuestions(List<Question> questions, int amount);
    [ChatCommand("clever")]
    private void cmdChatClever(BasePlayer player);
    [ConsoleCommand("UI_Clever")]
    private void cmdConsoleHandler(ConsoleSystem.Arg args);
    [ConsoleCommand("clever")]
    private void cmdControlClever(ConsoleSystem.Arg args);
    private static string HexToCuiColor(string hex);
    public void ReplyWithHelper(BasePlayer player, string message, string[] args);
    private void GetConfig(string menu, string Key, T var);
}

private class Player
{
    [JsonProperty("Текущий баланс игрока")]
    public int Balance;
}

private class Game
{
    [JsonProperty("Время начала игры")]
    public double StartTimeStamp;
    [JsonProperty("Текущий номер вопроса")]
    public int CurrentQuestionIndex;
    [JsonProperty("Текущий призовой фонд")]
    public int FinishPrize;
    [JsonProperty("Время на ответ")]
    public int TimeToAnswer;
    [JsonProperty("Текущий таймер")]
    public int NextTimerAmount;
    [JsonProperty("Сфомированный список вопросов")]
    public List<Question> Questions;
    [JsonProperty("Правильные ответы на вопросы")]
    public Dictionary<int, int> PercentsAnswers;
    [JsonProperty("Ответы игроков на каждый вопрос")]
    public List<Dictionary<BasePlayer, bool>> PlayerAnswers;
    public Game(double startTime, int finishPrize, int timeToAnswer, List<Question> questionList);
    public Question CurrentQuestion();
    public bool IsLoose(BasePlayer player);
    public double LeftTime();
    public string LeftTimeString();
    public string LeftTimeShortString();
}

private class Question
{
    [JsonProperty("Текст вопроса")]
    public string Text;
    [JsonProperty("Варианты ответов")]
    public Dictionary<string, bool> Answers;
}


```

---

## CobaltLaboratory-1.0.0

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("N1KTO COMPANY - Cobalt Laboratory", "RustPlugin", "1.0.0")]
[Description("Автоматическое событие с ботами и кастомным зданием от N1KTO COMPANY")]
public class CobaltLaboratory : RustPlugin
{
    private Configuration config;
    private const string PermissionUse;
    private const string PermissionAdmin;
    private Timer eventTimer;
    private Timer eventDurationTimer;
    private Timer radiationTimer;
    private Timer autoStartCheckTimer;
    private Vector3 eventPosition;
    private bool isEventActive;
    private List<BasePlayer> activeNPCs;
    private List<BaseEntity> spawnedEntities;
    private StorageContainer lootBox;
    private BaseEntity radiationEntity;
    private float eventEndTime;
    private string mapMarkerID;
     class Configuration
    {
        [JsonProperty("Setting up and stopping an event")]
        public EventSettings Event { get; set; }
        [JsonProperty("Configuring notifications")]
        public NotificationSettings Notifications { get; set; }
        [JsonProperty("Setting up radiation in the event area")]
        public RadiationSettings Radiation { get; set; }
        [JsonProperty("Event display on maps")]
        public MapSettings Map { get; set; }
        [JsonProperty("Bot settings")]
        public BotSettings Bots { get; set; }
        [JsonProperty("UI settings")]
        public UISettings UI { get; set; }
        [JsonProperty("Loot settings")]
        public LootSettings Loot { get; set; }
        [JsonProperty("Command settings")]
        public CommandSettings Commands { get; set; }
        public Configuration();
    }

     class EventSettings
    {
        [JsonProperty("The minimum number of players to start an event")]
        public int MinPlayers { get; set; }
        [JsonProperty("Time before the start of the event (Minimum in seconds)")]
        public int MinStartTime { get; set; }
        [JsonProperty("Time before the start of the event (Maximum in seconds)")]
        public int MaxStartTime { get; set; }
        [JsonProperty("Enable auto-start at specific time")]
        public bool EnableAutoStart { get; set; }
        [JsonProperty("Auto-start time in seconds")]
        public int AutoStartTime { get; set; }
    }

     class NotificationSettings
    {
        [JsonProperty("Discord WebHook")]
        public string DiscordWebHook { get; set; }
        [JsonProperty("Enable UI Notifications?")]
        public bool EnableUINotifications { get; set; }
        [JsonProperty("Auto hide UI notifications?")]
        public bool AutoHideUI { get; set; }
        [JsonProperty("How long after the show will it hide? (sec)")]
        public float HideDelay { get; set; }
    }

     class RadiationSettings
    {
        [JsonProperty("Turn on radiation?")]
        public bool EnableRadiation { get; set; }
        [JsonProperty("Number of radiation particles")]
        public int RadiationParticles { get; set; }
    }

     class MapSettings
    {
        [JsonProperty("Mark the event on the G card")]
        public bool ShowOnMap { get; set; }
        [JsonProperty("Text for map G")]
        public string MapText { get; set; }
    }

     class BotSettings
    {
        [JsonProperty("Bot types")]
        public List<BotType> Types { get; set; }
        [JsonProperty("Enable night vision")]
        public bool EnableNightVision { get; set; }
        [JsonProperty("Enable flashlights at night")]
        public bool EnableFlashlights { get; set; }
        [JsonProperty("Bot behavior settings")]
        public BotBehavior Behavior { get; set; }
        public BotSettings();
    }

     class BotType
    {
        [JsonProperty("Bot name")]
        public string Name { get; set; }
        [JsonProperty("Health")]
        public float Health { get; set; }
        [JsonProperty("Accuracy (0.0-1.0)")]
        public float Accuracy { get; set; }
        [JsonProperty("Roam range")]
        public float RoamRange { get; set; }
        [JsonProperty("Chase range")]
        public float ChaseRange { get; set; }
        [JsonProperty("Equipment")]
        public Equipment Equipment { get; set; }
    }

     class Equipment
    {
        [JsonProperty("Weapons")]
        public List<string> Weapons { get; set; }
        [JsonProperty("Armor")]
        public List<string> Armor { get; set; }
        [JsonProperty("Items")]
        public List<string> Items { get; set; }
    }

     class BotBehavior
    {
        [JsonProperty("Use cover")]
        public bool UseCover { get; set; }
        [JsonProperty("Help wounded allies")]
        public bool HelpAllies { get; set; }
        [JsonProperty("Retreat when low health")]
        public bool RetreatWhenLowHealth { get; set; }
        [JsonProperty("Low health threshold")]
        public float LowHealthThreshold { get; set; }
    }

     class UISettings
    {
        [JsonProperty("Event timer color")]
        public string TimerColor { get; set; }
        [JsonProperty("Event notification color")]
        public string NotificationColor { get; set; }
        [JsonProperty("Show event timer")]
        public bool ShowEventTimer { get; set; }
        [JsonProperty("Show kill feed")]
        public bool ShowKillFeed { get; set; }
        [JsonProperty("Show minimap")]
        public bool ShowMinimap { get; set; }
    }

     class LootSettings
    {
        [JsonProperty("Настройки ящиков")]
        public BoxSettings BoxSettings { get; set; }
        [JsonProperty("Категории лута")]
        public List<LootCategory> Categories { get; set; }
        [JsonProperty("Минимум предметов в ящике")]
        public int MinItemsPerBox { get; set; }
        [JsonProperty("Максимум предметов в ящике")]
        public int MaxItemsPerBox { get; set; }
        public LootSettings();
    }

     class BoxSettings
    {
        [JsonProperty("Тип ящика (large.wooden/elite/military/...)")]
        public string BoxType { get; set; }
        [JsonProperty("Высота спавна ящика над землей")]
        public float BoxHeight { get; set; }
        [JsonProperty("Можно ли подбирать ящик")]
        public bool IsPickupable { get; set; }
        [JsonProperty("Время жизни ящика (в минутах, 0 = бесконечно)")]
        public float BoxLifetime { get; set; }
        [JsonProperty("Защита ящика (0-1000)")]
        public float BoxHealth { get; set; }
        [JsonProperty("Создавать несколько ящиков")]
        public bool EnableMultipleBoxes { get; set; }
        [JsonProperty("Минимум ящиков")]
        public int MinBoxes { get; set; }
        [JsonProperty("Максимум ящиков")]
        public int MaxBoxes { get; set; }
        [JsonProperty("Радиус спавна ящиков")]
        public float BoxSpawnRadius { get; set; }
    }

     class LootCategory
    {
        [JsonProperty("Category name")]
        public string Name { get; set; }
        [JsonProperty("Category weight (higher = more common)")]
        public int Weight { get; set; }
        [JsonProperty("Items in category")]
        public List<LootItem> Items { get; set; }
    }

     class LootItem
    {
        [JsonProperty("Item shortname")]
        public string ShortName { get; set; }
        [JsonProperty("Minimum amount")]
        public int MinAmount { get; set; }
        [JsonProperty("Maximum amount")]
        public int MaxAmount { get; set; }
        [JsonProperty("Drop chance (0-100)")]
        public float Chance { get; set; }
        [JsonProperty("Is blueprint")]
        public bool Blueprint { get; set; }
        [JsonProperty("Custom skin ID")]
        public ulong SkinID { get; set; }
    }

     class CommandSettings
    {
        [JsonProperty("Основная команда")]
        public string MainCommand { get; set; }
        [JsonProperty("Команда старта")]
        public string StartCommand { get; set; }
        [JsonProperty("Команда остановки")]
        public string StopCommand { get; set; }
        [JsonProperty("Команда статуса")]
        public string StatusCommand { get; set; }
        [JsonProperty("Команда настройки времени")]
        public string TimeCommand { get; set; }
        [JsonProperty("Команда настройки вебхука")]
        public string WebhookCommand { get; set; }
        [JsonProperty("Команда автостарта")]
        public string AutoStartCommand { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized(bool initial);
     void Unload();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    [Command("cobaltlab")]
    private void CmdCobaltLab(BasePlayer player, string command, string[] args);
    private void ScheduleNextEvent();
    private void StartEvent();
    private void StopEvent();
    private void CleanupEvent();
    private void FindEventPosition();
    private void SpawnBuilding();
    private void SpawnNPCs();
    private void CreateLootBox();
    private void PopulateLootBox(StorageContainer box);
    private LootCategory SelectRandomCategory();
    private LootItem SelectRandomItem(LootCategory category);
    private void CreateRadiation();
    private void CreateMapMarker();
    private void RemoveMapMarker();
    private void BroadcastEventStart();
    private void ShowEventStatus(BasePlayer player);
    private void SendMessage(BasePlayer player, string message);
    private void CreateUI(BasePlayer player, string message);
    private string GetDirectionToEvent(Vector3 playerPos);
    private void BroadcastKill(BasePlayer killer, string victimName);
    private void SendDiscordMessage(string message);
    private string GetNearestMonument();
    private void CheckAutoStart();
}

 class Configuration
{
    [JsonProperty("Setting up and stopping an event")]
    public EventSettings Event { get; set; }
    [JsonProperty("Configuring notifications")]
    public NotificationSettings Notifications { get; set; }
    [JsonProperty("Setting up radiation in the event area")]
    public RadiationSettings Radiation { get; set; }
    [JsonProperty("Event display on maps")]
    public MapSettings Map { get; set; }
    [JsonProperty("Bot settings")]
    public BotSettings Bots { get; set; }
    [JsonProperty("UI settings")]
    public UISettings UI { get; set; }
    [JsonProperty("Loot settings")]
    public LootSettings Loot { get; set; }
    [JsonProperty("Command settings")]
    public CommandSettings Commands { get; set; }
    public Configuration();
}

 class EventSettings
{
    [JsonProperty("The minimum number of players to start an event")]
    public int MinPlayers { get; set; }
    [JsonProperty("Time before the start of the event (Minimum in seconds)")]
    public int MinStartTime { get; set; }
    [JsonProperty("Time before the start of the event (Maximum in seconds)")]
    public int MaxStartTime { get; set; }
    [JsonProperty("Enable auto-start at specific time")]
    public bool EnableAutoStart { get; set; }
    [JsonProperty("Auto-start time in seconds")]
    public int AutoStartTime { get; set; }
}

 class NotificationSettings
{
    [JsonProperty("Discord WebHook")]
    public string DiscordWebHook { get; set; }
    [JsonProperty("Enable UI Notifications?")]
    public bool EnableUINotifications { get; set; }
    [JsonProperty("Auto hide UI notifications?")]
    public bool AutoHideUI { get; set; }
    [JsonProperty("How long after the show will it hide? (sec)")]
    public float HideDelay { get; set; }
}

 class RadiationSettings
{
    [JsonProperty("Turn on radiation?")]
    public bool EnableRadiation { get; set; }
    [JsonProperty("Number of radiation particles")]
    public int RadiationParticles { get; set; }
}

 class MapSettings
{
    [JsonProperty("Mark the event on the G card")]
    public bool ShowOnMap { get; set; }
    [JsonProperty("Text for map G")]
    public string MapText { get; set; }
}

 class BotSettings
{
    [JsonProperty("Bot types")]
    public List<BotType> Types { get; set; }
    [JsonProperty("Enable night vision")]
    public bool EnableNightVision { get; set; }
    [JsonProperty("Enable flashlights at night")]
    public bool EnableFlashlights { get; set; }
    [JsonProperty("Bot behavior settings")]
    public BotBehavior Behavior { get; set; }
    public BotSettings();
}

 class BotType
{
    [JsonProperty("Bot name")]
    public string Name { get; set; }
    [JsonProperty("Health")]
    public float Health { get; set; }
    [JsonProperty("Accuracy (0.0-1.0)")]
    public float Accuracy { get; set; }
    [JsonProperty("Roam range")]
    public float RoamRange { get; set; }
    [JsonProperty("Chase range")]
    public float ChaseRange { get; set; }
    [JsonProperty("Equipment")]
    public Equipment Equipment { get; set; }
}

 class Equipment
{
    [JsonProperty("Weapons")]
    public List<string> Weapons { get; set; }
    [JsonProperty("Armor")]
    public List<string> Armor { get; set; }
    [JsonProperty("Items")]
    public List<string> Items { get; set; }
}

 class BotBehavior
{
    [JsonProperty("Use cover")]
    public bool UseCover { get; set; }
    [JsonProperty("Help wounded allies")]
    public bool HelpAllies { get; set; }
    [JsonProperty("Retreat when low health")]
    public bool RetreatWhenLowHealth { get; set; }
    [JsonProperty("Low health threshold")]
    public float LowHealthThreshold { get; set; }
}

 class UISettings
{
    [JsonProperty("Event timer color")]
    public string TimerColor { get; set; }
    [JsonProperty("Event notification color")]
    public string NotificationColor { get; set; }
    [JsonProperty("Show event timer")]
    public bool ShowEventTimer { get; set; }
    [JsonProperty("Show kill feed")]
    public bool ShowKillFeed { get; set; }
    [JsonProperty("Show minimap")]
    public bool ShowMinimap { get; set; }
}

 class LootSettings
{
    [JsonProperty("Настройки ящиков")]
    public BoxSettings BoxSettings { get; set; }
    [JsonProperty("Категории лута")]
    public List<LootCategory> Categories { get; set; }
    [JsonProperty("Минимум предметов в ящике")]
    public int MinItemsPerBox { get; set; }
    [JsonProperty("Максимум предметов в ящике")]
    public int MaxItemsPerBox { get; set; }
    public LootSettings();
}

 class BoxSettings
{
    [JsonProperty("Тип ящика (large.wooden/elite/military/...)")]
    public string BoxType { get; set; }
    [JsonProperty("Высота спавна ящика над землей")]
    public float BoxHeight { get; set; }
    [JsonProperty("Можно ли подбирать ящик")]
    public bool IsPickupable { get; set; }
    [JsonProperty("Время жизни ящика (в минутах, 0 = бесконечно)")]
    public float BoxLifetime { get; set; }
    [JsonProperty("Защита ящика (0-1000)")]
    public float BoxHealth { get; set; }
    [JsonProperty("Создавать несколько ящиков")]
    public bool EnableMultipleBoxes { get; set; }
    [JsonProperty("Минимум ящиков")]
    public int MinBoxes { get; set; }
    [JsonProperty("Максимум ящиков")]
    public int MaxBoxes { get; set; }
    [JsonProperty("Радиус спавна ящиков")]
    public float BoxSpawnRadius { get; set; }
}

 class LootCategory
{
    [JsonProperty("Category name")]
    public string Name { get; set; }
    [JsonProperty("Category weight (higher = more common)")]
    public int Weight { get; set; }
    [JsonProperty("Items in category")]
    public List<LootItem> Items { get; set; }
}

 class LootItem
{
    [JsonProperty("Item shortname")]
    public string ShortName { get; set; }
    [JsonProperty("Minimum amount")]
    public int MinAmount { get; set; }
    [JsonProperty("Maximum amount")]
    public int MaxAmount { get; set; }
    [JsonProperty("Drop chance (0-100)")]
    public float Chance { get; set; }
    [JsonProperty("Is blueprint")]
    public bool Blueprint { get; set; }
    [JsonProperty("Custom skin ID")]
    public ulong SkinID { get; set; }
}

 class CommandSettings
{
    [JsonProperty("Основная команда")]
    public string MainCommand { get; set; }
    [JsonProperty("Команда старта")]
    public string StartCommand { get; set; }
    [JsonProperty("Команда остановки")]
    public string StopCommand { get; set; }
    [JsonProperty("Команда статуса")]
    public string StatusCommand { get; set; }
    [JsonProperty("Команда настройки времени")]
    public string TimeCommand { get; set; }
    [JsonProperty("Команда настройки вебхука")]
    public string WebhookCommand { get; set; }
    [JsonProperty("Команда автостарта")]
    public string AutoStartCommand { get; set; }
}


```

---

## CodelockNerf

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;

Oxide.Plugins
[Info("CodeLockNerf", "Kyrah Abattoir", "0.1", ResourceId = 1873)]
[Description("If you die, your character can forget your door codes, better keep that notepad ready.")]
 class CodelockNerf : RustPlugin
{
     int _cfgMaxUniqueCodes;
     int _cfgCodeForgetRate;
     int _cfgMaxUniqueLocks;
     int _cfgLockForgetRate;
     bool _cfgOutputstats;
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     bool _isLoaded;
     List<CodeLock> _codelocks;
    readonly FieldInfo _whitelistField;
    readonly FieldInfo _codeField;
     void OnServerInitialized();
     void Loaded();
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
     T GetConfig(string name, T defaultValue);
     string GetMessage(string key, string userId);
}


```

---

## ColliderCount

```csharp
using System;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Collider Count", "Cheeze www.ukwasteland.co.uk", "0.0.3", ResourceId = 1306)]
 class ColliderCount : RustPlugin
{
    private const string ChatPrefix;
    private const string ChatPrefixColor;
    private readonly DateTime epoch;
    private DateTime wipeDate;
     IFormatProvider culture;
    protected override void LoadDefaultConfig();
     void Loaded();
    [ChatCommand("wipeinfo")]
    private void WipeInfoChat(BasePlayer player, string command, string[] args);
    [ConsoleCommand("wipeinfo")]
    private void WipeInfoConsole(ConsoleSystem.Arg arg);
    private int GetColliderCount();
    private int GetMaxColliders();
    private string GetColor1();
    private string GetColor2();
    private string GetTimeToWipe();
    private static void SendMessage(BasePlayer player, string message, object[] args);
    private long GetTimestamp(DateTime date);
}


```

---

## ColouredNames

```csharp
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("ColouredNames", "PsychoTea", "1.0.0")]
internal class ColouredNames : RustPlugin
{
     class StoredData
    {
        public Dictionary<ulong, string> colour;
    }

     StoredData storedData;
     void Init();
     void Loaded();
     object OnPlayerChat(ConsoleSystem.Arg arg);
    [ChatCommand("colour")]
    private void ColourCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("colours")]
    private void ColoursCmd(BasePlayer player, string command, string[] args);
}

 class StoredData
{
    public Dictionary<ulong, string> colour;
}


```

---

## CombatBB

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using ru = Oxide.Game.Rust;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json.Linq;
using UnityEngine;
using Facepunch;
using Rust;

Oxide.Plugins
[Info("CombatBB", "King", "2.1.35")]
[Description("Prevent commands/actions while raid and/or combat is occuring")]
 class CombatBB : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public static CombatBB ins;
    private void CmdMenuOpen1y(IPlayer user, string cmd, string[] args);
    private void CmdMenuClose1y(IPlayer user, string cmd, string[] args);
    public void consoleOpenf(BasePlayer player);
    public Dictionary<BasePlayer, bool> openPanels;
    public void MainGUIf(BasePlayer player);
    public void InfoMenuf(BasePlayer player);
    public void timerUIf(BasePlayer player, double time);
    public void linesf(BasePlayer player, double time);
    private static string HexToRustFormat(string hex);
    [ChatCommand("5789")]
     void pop(BasePlayer player);
     List<string> blockTypes;
     bool combatBlock;
    static float combatDuration;
     bool combatOnHitPlayer;
     float combatOnHitPlayerMinCondition;
     float combatOnHitPlayerMinDamage;
     bool combatOnTakeDamage;
     float combatOnTakeDamageMinCondition;
     float combatOnTakeDamageMinDamage;
     bool combatOnHitNPC;
     bool combatOnTakeDamageNPC;
     bool raidBlock;
    static float raidDuration;
     float raidDistance;
     bool blockOnDamage;
     float blockOnDamageMinCondition;
     bool blockOnDestroy;
     bool ownerCheck;
     bool blockUnowned;
     bool blockAll;
     bool ownerBlock;
     bool cupboardShare;
     bool friendShare;
     bool clanShare;
     bool clanCheck;
     bool friendCheck;
     bool raiderBlock;
     List<string> raidDamageTypes;
     List<string> raidDeathTypes;
     List<string> combatDamageTypes;
     bool raidUnblockOnDeath;
     bool raidUnblockOnWakeup;
     bool raidUnblockOnRespawn;
     bool combatUnblockOnDeath;
     bool combatUnblockOnWakeup;
     bool combatUnblockOnRespawn;
     float cacheTimer;
     bool raidBlockNotify;
     bool combatBlockNotify;
     bool useZoneManager;
     bool zoneEnter;
     bool zoneLeave;
     bool useRaidableBases;
     bool raidableZoneEnter;
     bool raidableZoneLeave;
     bool sendUINotification;
     bool sendChatNotification;
     bool sendGUIAnnouncementsNotification;
     bool sendLustyMapNotification;
     string GUIAnnouncementTintColor;
     string GUIAnnouncementTextColor;
     string LustyMapIcon;
     float LustyMapDuration;
     Dictionary<string, RaidZone> zones;
     Dictionary<string, List<string>> memberCache;
     Dictionary<string, string> clanCache;
     Dictionary<string, List<string>> friendCache;
     Dictionary<string, DateTime> lastClanCheck;
     Dictionary<string, DateTime> lastCheck;
     Dictionary<string, DateTime> lastFriendCheck;
     Dictionary<string, bool> prefabBlockCache;
    internal Dictionary<ulong, BlockBehavior> blockBehaviors;
    public static CombatBB plugin;
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin ZoneManager;
     Plugin GUIAnnouncements;
     Plugin LustyMap;
    readonly int cupboardMask;
    readonly int blockLayer;
     Dictionary<string, bool> _cachedExcludedWeapons;
     List<string> blockedPrefabs;
     List<string> exceptionPrefabs;
     List<string> exceptionWeapons;
    private List<string> GetDefaultRaidDamageTypes();
    private List<string> GetDefaultCombatDamageTypes();
     Dictionary<string, object> blockWhenRaidDamageDefault;
     Dictionary<string, object> blockWhenCombatDamageDefault;
    static Regex _htmlRegex;
    protected override void LoadDefaultConfig();
     void Loaded();
     void Unload();
     void LoadMessages();
     void CheckConfig();
    protected void ReloadConfig();
     void OnServerInitialized();
    const string ss;
     void UnsubscribeHooks();
     object canRedeemKit(BasePlayer player);
    public class RaidZone
    {
        public string zoneid;
        public Vector3 position;
        public Timer timer;
        public RaidZone(string zoneid, Vector3 position);
        public float Distance(RaidZone zone);
        public float Distance(Vector3 pos);
        public RaidZone ResetTimer();
    }

    public abstract class BlockBehavior : MonoBehaviour
    {
        protected BasePlayer player;
        public DateTime lastBlock;
        public DateTime lastNotification;
        internal DateTime lastUINotification;
        internal Timer timer;
        internal Timer timer2;
        internal Action notifyCallback;
        internal string iconUID;
        internal bool moved;
         double pp;
        public void CopyFrom(BlockBehavior behavior);
        internal abstract float Duration { get; set; }
        internal abstract CuiRectTransformComponent NotificationWindow { get; set; }
        internal abstract string notifyMessage { get; set; }
        internal string BlockName { get; set; }
        public bool Active { get; set; }
         void Awake();
         void Destroy();
        public void Destroys();
         void sss();
        public void Stop();
         TimeSpan ts;
        public void Notify(Action callback);
         void SendGUI();
        public double GetTimeBlock();
    }

    public class CombatBlock : BlockBehavior
    {
        internal override float Duration { get; set; }
        internal override string notifyMessage { get; set; }
         CuiRectTransformComponent _notificationWindow;
        internal override CuiRectTransformComponent NotificationWindow { get; set; }
    }

    public class RaidBlock : BlockBehavior
    {
        internal override float Duration { get; set; }
        internal override string notifyMessage { get; set; }
        private CuiRectTransformComponent _notificationWindow;
        internal override CuiRectTransformComponent NotificationWindow { get; set; }
    }

     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     void StructureAttack(BaseEntity targetEntity, BaseEntity sourceEntity, string weapon, Vector3 hitPosition);
     float GetHealthPercent(BaseEntity entity, float damage);
     void BlockAll(BasePlayer source, BaseEntity targetEntity, List<string> sourceMembers);
     void OwnerBlock(BasePlayer source, BaseEntity sourceEntity, ulong target, Vector3 position, List<string> sourceMembers);
     List<string> CupboardShare(string owner, Vector3 position, BaseEntity sourceEntity, List<string> sourceMembers);
     void RaiderBlock(BasePlayer source, ulong target, Vector3 position, List<string> sourceMembers);
     bool IsBlocked(string target);
     bool IsBlocked(BasePlayer target);
    public bool IsBlocked(BasePlayer target);
     bool IsRaidBlocked(BasePlayer target);
     bool IsCombatBlocked(BasePlayer target);
     bool IsEscapeBlocked(string target);
     bool IsRaidBlocked(string target);
     bool IsCombatBlocked(string target);
     bool ShouldBlockEscape(ulong target, ulong source, List<string> sourceMembers);
     void StartRaidBlocking(BasePlayer target, bool createZone);
     void StartRaidBlocking(BasePlayer target, Vector3 position, bool createZone);
     void StartCombatBlocking(BasePlayer target);
     void StartBlockingBB(ConsoleSystem.Arg ar);
     void StopBlocking(BasePlayer target);
    public void StopBlocking(BasePlayer target);
     void ClearRaidBlockingS(string target);
     void StopRaidBlocking(BasePlayer player);
     void StopRaidBlocking(string target);
     void StopCombatBlocking(BasePlayer player);
     void StopCombatBlocking(string target);
     void ClearCombatBlocking(string target);
     void EraseZone(string zoneid);
     void ResetZoneTimer(RaidZone zone);
     void CreateRaidZone(Vector3 position);
    [HookMethod("OnEnterZone")]
     void OnEnterZone(string zoneid, BasePlayer player);
    [HookMethod("OnExitZone")]
     void OnExitZone(string zoneid, BasePlayer player);
    [HookMethod("OnPlayerEnteredRaidableBase")]
     void OnPlayerEnteredRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP);
    [HookMethod("OnPlayerExitedRaidableBase")]
     void OnPlayerExitedRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP);
    public List<string> getFriends(string player);
    public List<string> getFriendList(string player);
    public List<string> getClanMembers(string player);
     JObject GetClan(string tag);
     List<string> CacheClan(JObject clan);
    [HookMethod("OnClanCreate")]
     void OnClanCreate(string tag);
    [HookMethod("OnClanUpdate")]
     void OnClanUpdate(string tag);
    [HookMethod("OnClanDestroy")]
     void OnClanDestroy(string tag);
     bool HasPerm(string userid, string perm);
     bool CanRaidCommand(BasePlayer player, string command);
     bool CanRaidCommand(string playerID, string command);
     bool CanCombatCommand(BasePlayer player, string command);
     bool CanCombatCommand(string playerID, string command);
     object CanDo(string command, BasePlayer player);
     object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
     object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
     object CanRedeemKit(BasePlayer player);
     object CanUseMailbox(BasePlayer player, Mailbox mailbox);
     object CanUseVending(VendingMachine machine, BasePlayer player);
     object CanBuild(Planner plan, Construction prefab);
     object CanAssignBed(SleepingBag bag, BasePlayer player, ulong targetPlayerId);
     object CanOpenBackpack(BasePlayer player, ulong backpackOwnerID);
     object CanBank(BasePlayer player);
     object CanTrade(BasePlayer player);
     object canRemove(BasePlayer player);
     object canShop(BasePlayer player);
     object CanShop(BasePlayer player);
     object CanTeleport(BasePlayer player);
     object canTeleport(BasePlayer player);
     object CanGridTeleport(BasePlayer player);
     object CanCraft(ItemCrafter itemCrafter, ItemBlueprint bp, int amount);
     object CanRecycleCommand(BasePlayer player);
     object CanBGrade(BasePlayer player, int grade, BuildingBlock buildingBlock, Planner planner);
     void SendBlockMessage(BasePlayer target, BlockBehavior blocker, string langMessage, string completeMessage);
     string GetCooldownTime(float f, string userID);
    public string GetMessage(BasePlayer player);
    public string GetPrefix(string player);
    public string GetMessage(BasePlayer player, string blockMsg, float duration);
     bool TryGetBlocker(BasePlayer player, T blocker);
    public bool isEntityException(string prefabName);
    public bool IsEntityBlocked(BaseCombatEntity entity);
     bool IsRaidDamage(DamageType dt);
     bool IsDeathDamage(DamageType dt);
     bool IsRaidDamage(DamageTypeList dtList);
     bool IsDeathDamage(DamageTypeList dtList);
     bool IsExcludedWeapon(string name);
     bool IsCombatDamage(DamageType dt);
     bool IsCombatDamage(DamageTypeList dtList);
     T GetConfig(string name, string name2, string name3, T defaultValue);
     T GetConfig(string name, string name2, T defaultValue);
     T GetConfig(string name, T defaultValue);
     T ParseValue(object val, T defaultValue);
    static string GetMsg(string key, object user);
    public static class UI
    {
        public static void AddImage(CuiElementContainer container, string parrent, string name, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
        public static void AddRawImage(CuiElementContainer container, string parrent, string name, string png, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax);
        public static void AddText(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
        public static void AddButton(CuiElementContainer container, string parrent, string name, string cmd, string close, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
    }

    private static string FormatTime(TimeSpan time);
    private static string FormatMinutes(int minutes);
    private static string FormatSeconds(int seconds);
    private static string FormatUnits2(int units);
    private static string FormatUnits(int units);
}

public class RaidZone
{
    public string zoneid;
    public Vector3 position;
    public Timer timer;
    public RaidZone(string zoneid, Vector3 position);
    public float Distance(RaidZone zone);
    public float Distance(Vector3 pos);
    public RaidZone ResetTimer();
}

public abstract class BlockBehavior : MonoBehaviour
{
    protected BasePlayer player;
    public DateTime lastBlock;
    public DateTime lastNotification;
    internal DateTime lastUINotification;
    internal Timer timer;
    internal Timer timer2;
    internal Action notifyCallback;
    internal string iconUID;
    internal bool moved;
     double pp;
    public void CopyFrom(BlockBehavior behavior);
    internal abstract float Duration { get; set; }
    internal abstract CuiRectTransformComponent NotificationWindow { get; set; }
    internal abstract string notifyMessage { get; set; }
    internal string BlockName { get; set; }
    public bool Active { get; set; }
     void Awake();
     void Destroy();
    public void Destroys();
     void sss();
    public void Stop();
     TimeSpan ts;
    public void Notify(Action callback);
     void SendGUI();
    public double GetTimeBlock();
}

public class CombatBlock : BlockBehavior
{
    internal override float Duration { get; set; }
    internal override string notifyMessage { get; set; }
     CuiRectTransformComponent _notificationWindow;
    internal override CuiRectTransformComponent NotificationWindow { get; set; }
}

public class RaidBlock : BlockBehavior
{
    internal override float Duration { get; set; }
    internal override string notifyMessage { get; set; }
    private CuiRectTransformComponent _notificationWindow;
    internal override CuiRectTransformComponent NotificationWindow { get; set; }
}

public static class UI
{
    public static void AddImage(CuiElementContainer container, string parrent, string name, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
    public static void AddRawImage(CuiElementContainer container, string parrent, string name, string png, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax);
    public static void AddText(CuiElementContainer container, string parrent, string name, string color, string text, TextAnchor align, int size, string aMin, string aMax, string oMin, string oMax, string outColor, string font, string dist, float FadeIN, float FadeOut);
    public static void AddButton(CuiElementContainer container, string parrent, string name, string cmd, string close, string color, string sprite, string mat, string aMin, string aMax, string oMin, string oMax, string outline, string dist);
}


```

---

## CombatBlock

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("CombatBlock", "King", "1.0.0")]
public class CombatBlock : RustPlugin
{
    private const string Layer;
    private static CombatBlock plugin;
    private readonly Dictionary<BasePlayer, CombatManager> _components;
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty("Настройки плагина")]
        public Settings _Settings;
        [JsonProperty("Версия конфигурации")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

    public class Settings
    {
        [JsonProperty("Длительность комбат-блока")]
        public float timeCombatBlock;
        [JsonProperty("Черный список команд какие запрещены при комбат блоке")]
        public List<string> BlackListCommands;
        [JsonProperty("Блокировать при попадании по игроку ?")]
        public bool blockFirePlayer;
        [JsonProperty("Блокировать при получении урона от игрока ?")]
        public bool blockHitPlayer;
    }

    private void Init();
    private void Unload();
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private object OnUserCommand(IPlayer ipl, string command, string[] args);
    private class CombatManager : FacepunchBehaviour
    {
        private BasePlayer _player;
        private float _startTime;
        private bool _started;
        private float _cooldown;
        private void Awake();
        public void Init();
        public void MainUi();
        private void UpdateUi();
        private void FixedUpdate();
        private int GetLeftTime();
        public void DestroyComp();
        private void OnDestroy();
        public void Kill();
    }

    private static bool IsNPC(BasePlayer player);
    private CombatManager AddOrGetBuild(BasePlayer player);
    private CombatManager IsCombatBlocked(BasePlayer player);
    private void StartingCombatBlock(BasePlayer player);
    private IEnumerator StartUpdate(BasePlayer player);
}

private class PluginConfig
{
    [JsonProperty("Настройки плагина")]
    public Settings _Settings;
    [JsonProperty("Версия конфигурации")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}

public class Settings
{
    [JsonProperty("Длительность комбат-блока")]
    public float timeCombatBlock;
    [JsonProperty("Черный список команд какие запрещены при комбат блоке")]
    public List<string> BlackListCommands;
    [JsonProperty("Блокировать при попадании по игроку ?")]
    public bool blockFirePlayer;
    [JsonProperty("Блокировать при получении урона от игрока ?")]
    public bool blockHitPlayer;
}

private class CombatManager : FacepunchBehaviour
{
    private BasePlayer _player;
    private float _startTime;
    private bool _started;
    private float _cooldown;
    private void Awake();
    public void Init();
    public void MainUi();
    private void UpdateUi();
    private void FixedUpdate();
    private int GetLeftTime();
    public void DestroyComp();
    private void OnDestroy();
    public void Kill();
}


```

---

## CombatBlock-1.1.1

```csharp
using Oxide.Core;
using ConVar;
using System.Text;
using UnityEngine.Networking;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using System;
using Oxide.Plugins.CombatBlockExt;
using Oxide.Core.Plugins;
using System.Collections;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Globalization;
using Time = UnityEngine.Time;

Oxide.Plugins.CombatBlockExt
public static class ExtensionMethods
{
    private static readonly Lang Lang;
    public static string GetAdaptedMessage(string langKey, string userID, object[] args);
    public static string ToTimeFormat(float source);
}

Oxide.Plugins
[Info("CombatBlock", "Mercury", "1.1.1")]
public class CombatBlock : RustPlugin
{
    private static void RunEffect(BasePlayer player, string prefab);
    private List<BasePlayer> _playerInCache;
    protected override void LoadConfig();
    private void OnRaidBlock(BasePlayer player, Vector3 position);
    private void OnPlayerConnected(BasePlayer player);
    private Boolean IsCombatBlock(BasePlayer player);
    private void DrawUI_CB_Main(BasePlayer player, float timeLeft);
    private bool IsDuel(ulong userID);
    private Dictionary<BasePlayer ,CombatPlayer> _combatPlayers;
    private Boolean IsCombatBlocked(BasePlayer player);
    private static ImageUI _imageUI;
    private object canTeleport(BasePlayer player);
    private class CombatPlayer : FacepunchBehaviour
    {
        public BasePlayer player;
        private float _timeToUnblock;
        private int _combatBlockDuration;
        public float UnblockTimeLeft { get; set; }
        private void Awake();
        private void OnDestroy();
        public void UpdateBlockTime();
        public void CreateUI();
        public void KillComponent(bool isRaidBlocked);
        private void RefreshUI();
    }

    private Boolean IsCombatBlocked(String playerID);
    private object CanRedeemKit(BasePlayer player);
    private object CanTrade(BasePlayer player);
    private void Unload();
    private class ImageUI
    {
        private readonly string _paths;
        private readonly string _printPath;
        private readonly Dictionary<string, ImageData> _images;
        public ImageUI();
        private class ImageData
        {
            public ImageStatus Status;
            public string Id { get; set; }
        }

        public string GetImage(string name);
        public void DownloadImage();
        public void UnloadImages();
        private IEnumerator ProcessDownloadImage(KeyValuePair<string, ImageData> image);
    }

    private void OnServerInitialized();
    private void Init();
    private void StartCombatBlocked(BasePlayer player);
    protected override void LoadDefaultConfig();
    private void DrawUI_CB_Updated(BasePlayer player, float timeLeft, bool upd);
    private object canTrade(BasePlayer player);
    private bool IsClans(string userID, string targetID);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
    private class InterfaceBuilder
    {
        private static InterfaceBuilder _instance;
        public const string CB_MAIN;
        public const string CB_PROGRESS_BAR;
        public const string CB_PROGRESS;
        public const string CB_PROGRESS_TIMER;
        public static int TypeUi;
        public static double Factor;
        private Configuration.CombatBlockUi.CombatBlockUiSettings _uiSettings;
        private static float _fade;
        private Dictionary<string, string> _interfaces;
        public InterfaceBuilder();
        private static void AddInterface(string name, string json);
        public static string GetInterface(string name);
        public static void DestroyAll();
        private void BuildingCombatBlockMain();
        private void BuildingCombatBlockUpdated();
        private void BuildingCombatBlockMainV2();
        private void BuildingCombatBlockUpdatedV2();
        private void BuildingCombatBlockMainV3();
        private void BuildingCombatBlockUpdatedV3();
    }

    private void OnRaidBlockStarted(BasePlayer player);
    private bool IsRaidBlocked(BasePlayer player, bool chat);
    [PluginReference]
     Plugin RaidBlock;
     Plugin NoEscape;
     Plugin IQChat;
     Plugin Friends;
     Plugin Clans;
     Plugin Duelist;
     Plugin Duel;
     Plugin EventHelper;
     Plugin Battles;
     Plugin ArenaTournament;
    private Configuration config;
    protected override void SaveConfig();
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
    private const string _permIgnoreCombat;
    private Boolean IsCombatBlocked(UInt64 playerID);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void SendChat(string message, BasePlayer player, Single timeout, Chat.ChatChannel channel);
    private const bool LanguageEn;
    protected override void LoadDefaultMessages();
    public static CombatBlock Instance;
    private class Configuration
    {
        [JsonProperty(LanguageEn ? "Primary combat settings" : "Основные настройки комбат-блока")]
        public CombatBlock CombatBlockMain;
        public class CombatBlockActionsBlocked
        {
            [JsonProperty(LanguageEn ? "Disable teleportation capability (true - yes/false - no)" : "Блокировать возможность телепортироваться (true - да/false - нет)")]
            public bool CanTeleport;
            [JsonProperty(LanguageEn ? "Disable the use of kits (true - yes/false - no)" : "Блокировать возможность использования китов (true - да/false - нет)")]
            public bool CanUseKit;
            [JsonProperty(LanguageEn ? "Disable trade functionality (true - yes/false - no)" : "Блокировать возможность обмена (Trade) (true - да/false - нет)")]
            public bool CanTrade;
            [JsonProperty(LanguageEn ? "List of prohibited commands during active lockdown [Specify them without a slash (/)]" : "Список запрещенных команд при активной блокировки [указывайте их без слэша (/)]", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> BlockedCommands;
        }

        public class CombatBlock
        {
            [JsonProperty(LanguageEn ? "Disable the combat block when receiving a raid block (true - yes/false - no)" : "Отключить комбат блок при получении рейд блока (true - да/false - нет)")]
            public bool CombatBlockOnRaidBlock;
            [JsonProperty(LanguageEn ? "Lockout time (seconds)" : "Время блокировки (секунды)")]
            public int CombatBlockDuration;
        }

        [JsonProperty(LanguageEn ? "Interface settings" : "Настройки интерфейса")]
        public CombatBlockUi CombatBlockInterface;
        public class IQChat
        {
            [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
            public String CustomPrefix;
            [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
            public String CustomAvatar;
            [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
            public Boolean UIAlertUse;
        }

        [JsonProperty(LanguageEn ? "Trigger settings" : "Настройка триггеров")]
        public CombatBlockDetect BlockDetect;
        public class CombatBlockUi
        {
            [JsonProperty(LanguageEn ? "Interface variant (0, 1, 2) - example: " : "Вариант интерфейса (0, 1, 2)")]
            public int UiType;
            [JsonProperty(LanguageEn ? "Interface layer: Overlay - will overlay other UI, Hud - will be overlaid by other interfaces" : "Слой интерфейса : Overlay - будет перекрывать другие UI, Hud - будет перекрываться другим интерфейсом")]
            public string Layers;
            [JsonProperty(LanguageEn ? "Vertical padding" : "Вертикальный отступ")]
            public int OffsetY;
            [JsonProperty(LanguageEn ? "Horizontal padding" : "Горизонтальный отступ")]
            public int OffsetX;
            [JsonProperty(LanguageEn ? "Interface settings for variant 0" : "Настройки интерфейса для варианта 0")]
            public CombatBlockUiSettings InterfaceSettingsVariant0;
            [JsonProperty(LanguageEn ? "Interface settings for variant 1" : "Настройки интерфейса для варианта 1")]
            public CombatBlockUiSettings InterfaceSettingsVariant1;
            [JsonProperty(LanguageEn ? "Interface settings for variant 2" : "Настройки интерфейса для варианта 2")]
            public CombatBlockUiSettings InterfaceSettingsVariant2;
            public class CombatBlockUiSettings
            {
                [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
                public string BackgroundColor;
                [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
                public string IconColor;
                [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
                public string AdditionalElementsColor;
                [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
                public string MainTextColor;
                [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
                public string SecondaryTextColor;
                [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
                public string ProgressBarMainColor;
                [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
                public string ProgressBarBackgroundColor;
                [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
                public float SmoothTransition;
            }

        }

        [JsonProperty(LanguageEn ? "Setting IQChat" : "Настройка IQChat")]
        public IQChat IQChatSetting;
        public class CombatBlockDetect
        {
            [JsonProperty(LanguageEn ? "Activate combat mode upon NPC attack (true - yes/false - no)" : "Активировать комбат-блок при атаке NPC (true - да/false - нет)")]
            public bool ActivateOnNpcAttack;
            [JsonProperty(LanguageEn ? "Activate combat mode upon receiving damage from NPCs (true - yes/false - no)" : "Активировать комбат-блок при получении урона от NPC (true - да/false - нет)")]
            public bool ActivateOnNpcDamageReceived;
            [JsonProperty(LanguageEn ? "Activate combat block when dealing damage to a sleeping player (true - yes/false - no)" : "Активировать комбат-блок при нанесении урона спящему игроку (true - да/false - нет)")]
            public bool ActivateOnSleeperAttack;
            [JsonProperty(LanguageEn ? "Deactivate combat mode after death (true - yes/false - no)" : "Деактивировать комбат-блок после смерти (true - да/false - нет)")]
            public bool DeactivateOnPlayerDeath;
        }

        [JsonProperty(LanguageEn ? "Combat mode restrictions settings" : "Настройка ограничений во время комбат-блока")]
        public CombatBlockActionsBlocked ActionsBlocked;
    }

    private object OnPlayerCommand(BasePlayer player, String command, String[] args);
    private void ValidateConfig();
    private static class ObjectCache
    {
        private static readonly object True;
        private static readonly object False;
        private static class StaticObjectCache
        {
            private static readonly Dictionary<T, object> CacheByValue;
            public static object Get(T value);
        }

        public static object Get(T value);
        public static object Get(bool value);
    }

    private object CanTeleport(BasePlayer player);
    private bool IsFriends(ulong userID, ulong targetID);
    private object CanActions(BasePlayer player, bool returnMessage);
    private static InterfaceBuilder _interface;
    private void OnEntityTakeDamage(BasePlayer target, HitInfo hitInfo);
}

private class CombatPlayer : FacepunchBehaviour
{
    public BasePlayer player;
    private float _timeToUnblock;
    private int _combatBlockDuration;
    public float UnblockTimeLeft { get; set; }
    private void Awake();
    private void OnDestroy();
    public void UpdateBlockTime();
    public void CreateUI();
    public void KillComponent(bool isRaidBlocked);
    private void RefreshUI();
}

private class ImageUI
{
    private readonly string _paths;
    private readonly string _printPath;
    private readonly Dictionary<string, ImageData> _images;
    public ImageUI();
    private class ImageData
    {
        public ImageStatus Status;
        public string Id { get; set; }
    }

    public string GetImage(string name);
    public void DownloadImage();
    public void UnloadImages();
    private IEnumerator ProcessDownloadImage(KeyValuePair<string, ImageData> image);
}

private class ImageData
{
    public ImageStatus Status;
    public string Id { get; set; }
}

private class InterfaceBuilder
{
    private static InterfaceBuilder _instance;
    public const string CB_MAIN;
    public const string CB_PROGRESS_BAR;
    public const string CB_PROGRESS;
    public const string CB_PROGRESS_TIMER;
    public static int TypeUi;
    public static double Factor;
    private Configuration.CombatBlockUi.CombatBlockUiSettings _uiSettings;
    private static float _fade;
    private Dictionary<string, string> _interfaces;
    public InterfaceBuilder();
    private static void AddInterface(string name, string json);
    public static string GetInterface(string name);
    public static void DestroyAll();
    private void BuildingCombatBlockMain();
    private void BuildingCombatBlockUpdated();
    private void BuildingCombatBlockMainV2();
    private void BuildingCombatBlockUpdatedV2();
    private void BuildingCombatBlockMainV3();
    private void BuildingCombatBlockUpdatedV3();
}

private class Configuration
{
    [JsonProperty(LanguageEn ? "Primary combat settings" : "Основные настройки комбат-блока")]
    public CombatBlock CombatBlockMain;
    public class CombatBlockActionsBlocked
    {
        [JsonProperty(LanguageEn ? "Disable teleportation capability (true - yes/false - no)" : "Блокировать возможность телепортироваться (true - да/false - нет)")]
        public bool CanTeleport;
        [JsonProperty(LanguageEn ? "Disable the use of kits (true - yes/false - no)" : "Блокировать возможность использования китов (true - да/false - нет)")]
        public bool CanUseKit;
        [JsonProperty(LanguageEn ? "Disable trade functionality (true - yes/false - no)" : "Блокировать возможность обмена (Trade) (true - да/false - нет)")]
        public bool CanTrade;
        [JsonProperty(LanguageEn ? "List of prohibited commands during active lockdown [Specify them without a slash (/)]" : "Список запрещенных команд при активной блокировки [указывайте их без слэша (/)]", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> BlockedCommands;
    }

    public class CombatBlock
    {
        [JsonProperty(LanguageEn ? "Disable the combat block when receiving a raid block (true - yes/false - no)" : "Отключить комбат блок при получении рейд блока (true - да/false - нет)")]
        public bool CombatBlockOnRaidBlock;
        [JsonProperty(LanguageEn ? "Lockout time (seconds)" : "Время блокировки (секунды)")]
        public int CombatBlockDuration;
    }

    [JsonProperty(LanguageEn ? "Interface settings" : "Настройки интерфейса")]
    public CombatBlockUi CombatBlockInterface;
    public class IQChat
    {
        [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
        public String CustomPrefix;
        [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
        public String CustomAvatar;
        [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
        public Boolean UIAlertUse;
    }

    [JsonProperty(LanguageEn ? "Trigger settings" : "Настройка триггеров")]
    public CombatBlockDetect BlockDetect;
    public class CombatBlockUi
    {
        [JsonProperty(LanguageEn ? "Interface variant (0, 1, 2) - example: " : "Вариант интерфейса (0, 1, 2)")]
        public int UiType;
        [JsonProperty(LanguageEn ? "Interface layer: Overlay - will overlay other UI, Hud - will be overlaid by other interfaces" : "Слой интерфейса : Overlay - будет перекрывать другие UI, Hud - будет перекрываться другим интерфейсом")]
        public string Layers;
        [JsonProperty(LanguageEn ? "Vertical padding" : "Вертикальный отступ")]
        public int OffsetY;
        [JsonProperty(LanguageEn ? "Horizontal padding" : "Горизонтальный отступ")]
        public int OffsetX;
        [JsonProperty(LanguageEn ? "Interface settings for variant 0" : "Настройки интерфейса для варианта 0")]
        public CombatBlockUiSettings InterfaceSettingsVariant0;
        [JsonProperty(LanguageEn ? "Interface settings for variant 1" : "Настройки интерфейса для варианта 1")]
        public CombatBlockUiSettings InterfaceSettingsVariant1;
        [JsonProperty(LanguageEn ? "Interface settings for variant 2" : "Настройки интерфейса для варианта 2")]
        public CombatBlockUiSettings InterfaceSettingsVariant2;
        public class CombatBlockUiSettings
        {
            [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
            public string BackgroundColor;
            [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
            public string IconColor;
            [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
            public string AdditionalElementsColor;
            [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
            public string MainTextColor;
            [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
            public string SecondaryTextColor;
            [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
            public string ProgressBarMainColor;
            [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
            public string ProgressBarBackgroundColor;
            [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
            public float SmoothTransition;
        }

    }

    [JsonProperty(LanguageEn ? "Setting IQChat" : "Настройка IQChat")]
    public IQChat IQChatSetting;
    public class CombatBlockDetect
    {
        [JsonProperty(LanguageEn ? "Activate combat mode upon NPC attack (true - yes/false - no)" : "Активировать комбат-блок при атаке NPC (true - да/false - нет)")]
        public bool ActivateOnNpcAttack;
        [JsonProperty(LanguageEn ? "Activate combat mode upon receiving damage from NPCs (true - yes/false - no)" : "Активировать комбат-блок при получении урона от NPC (true - да/false - нет)")]
        public bool ActivateOnNpcDamageReceived;
        [JsonProperty(LanguageEn ? "Activate combat block when dealing damage to a sleeping player (true - yes/false - no)" : "Активировать комбат-блок при нанесении урона спящему игроку (true - да/false - нет)")]
        public bool ActivateOnSleeperAttack;
        [JsonProperty(LanguageEn ? "Deactivate combat mode after death (true - yes/false - no)" : "Деактивировать комбат-блок после смерти (true - да/false - нет)")]
        public bool DeactivateOnPlayerDeath;
    }

    [JsonProperty(LanguageEn ? "Combat mode restrictions settings" : "Настройка ограничений во время комбат-блока")]
    public CombatBlockActionsBlocked ActionsBlocked;
}

public class CombatBlockActionsBlocked
{
    [JsonProperty(LanguageEn ? "Disable teleportation capability (true - yes/false - no)" : "Блокировать возможность телепортироваться (true - да/false - нет)")]
    public bool CanTeleport;
    [JsonProperty(LanguageEn ? "Disable the use of kits (true - yes/false - no)" : "Блокировать возможность использования китов (true - да/false - нет)")]
    public bool CanUseKit;
    [JsonProperty(LanguageEn ? "Disable trade functionality (true - yes/false - no)" : "Блокировать возможность обмена (Trade) (true - да/false - нет)")]
    public bool CanTrade;
    [JsonProperty(LanguageEn ? "List of prohibited commands during active lockdown [Specify them without a slash (/)]" : "Список запрещенных команд при активной блокировки [указывайте их без слэша (/)]", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> BlockedCommands;
}

public class CombatBlock
{
    [JsonProperty(LanguageEn ? "Disable the combat block when receiving a raid block (true - yes/false - no)" : "Отключить комбат блок при получении рейд блока (true - да/false - нет)")]
    public bool CombatBlockOnRaidBlock;
    [JsonProperty(LanguageEn ? "Lockout time (seconds)" : "Время блокировки (секунды)")]
    public int CombatBlockDuration;
}

public class IQChat
{
    [JsonProperty(LanguageEn ? "IQChat : Custom prefix in the chat" : "IQChat : Кастомный префикс в чате")]
    public String CustomPrefix;
    [JsonProperty(LanguageEn ? "IQChat : Custom avatar in the chat (If required)" : "IQChat : Кастомный аватар в чате(Если требуется)")]
    public String CustomAvatar;
    [JsonProperty(LanguageEn ? "IQChat : Use UI notifications" : "IQChat : Использовать UI уведомления")]
    public Boolean UIAlertUse;
}

public class CombatBlockUi
{
    [JsonProperty(LanguageEn ? "Interface variant (0, 1, 2) - example: " : "Вариант интерфейса (0, 1, 2)")]
    public int UiType;
    [JsonProperty(LanguageEn ? "Interface layer: Overlay - will overlay other UI, Hud - will be overlaid by other interfaces" : "Слой интерфейса : Overlay - будет перекрывать другие UI, Hud - будет перекрываться другим интерфейсом")]
    public string Layers;
    [JsonProperty(LanguageEn ? "Vertical padding" : "Вертикальный отступ")]
    public int OffsetY;
    [JsonProperty(LanguageEn ? "Horizontal padding" : "Горизонтальный отступ")]
    public int OffsetX;
    [JsonProperty(LanguageEn ? "Interface settings for variant 0" : "Настройки интерфейса для варианта 0")]
    public CombatBlockUiSettings InterfaceSettingsVariant0;
    [JsonProperty(LanguageEn ? "Interface settings for variant 1" : "Настройки интерфейса для варианта 1")]
    public CombatBlockUiSettings InterfaceSettingsVariant1;
    [JsonProperty(LanguageEn ? "Interface settings for variant 2" : "Настройки интерфейса для варианта 2")]
    public CombatBlockUiSettings InterfaceSettingsVariant2;
    public class CombatBlockUiSettings
    {
        [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
        public string BackgroundColor;
        [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
        public string IconColor;
        [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
        public string AdditionalElementsColor;
        [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
        public string MainTextColor;
        [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
        public string SecondaryTextColor;
        [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
        public string ProgressBarMainColor;
        [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
        public string ProgressBarBackgroundColor;
        [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
        public float SmoothTransition;
    }

}

public class CombatBlockUiSettings
{
    [JsonProperty(LanguageEn ? "Background color (RGBA)" : "Цвет фона (RGBA)")]
    public string BackgroundColor;
    [JsonProperty(LanguageEn ? "Icon color (RGBA)" : "Цвет иконки (RGBA)")]
    public string IconColor;
    [JsonProperty(LanguageEn ? "Color of additional elements (RGBA)" : "Цвет дополнительных элементов (RGBA)")]
    public string AdditionalElementsColor;
    [JsonProperty(LanguageEn ? "Main text color (RGBA)" : "Цвет основного текста (RGBA)")]
    public string MainTextColor;
    [JsonProperty(LanguageEn ? "Secondary text Color (RGBA)" : "Цвет второстепенного текста (RGBA)")]
    public string SecondaryTextColor;
    [JsonProperty(LanguageEn ? "Main color of the progress-bar (RGBA)" : "Основной цвет прогресс-бара (RGBA)")]
    public string ProgressBarMainColor;
    [JsonProperty(LanguageEn ? "Background Color of the Progress Bar (RGBA)" : "Цвет фона прогресс-бара (RGBA)")]
    public string ProgressBarBackgroundColor;
    [JsonProperty(LanguageEn ? "Delay before the UI appears and disappears (for smooth transitions)" : "Задержка перед появлением и исчезновением UI (для плавности)")]
    public float SmoothTransition;
}

public class CombatBlockDetect
{
    [JsonProperty(LanguageEn ? "Activate combat mode upon NPC attack (true - yes/false - no)" : "Активировать комбат-блок при атаке NPC (true - да/false - нет)")]
    public bool ActivateOnNpcAttack;
    [JsonProperty(LanguageEn ? "Activate combat mode upon receiving damage from NPCs (true - yes/false - no)" : "Активировать комбат-блок при получении урона от NPC (true - да/false - нет)")]
    public bool ActivateOnNpcDamageReceived;
    [JsonProperty(LanguageEn ? "Activate combat block when dealing damage to a sleeping player (true - yes/false - no)" : "Активировать комбат-блок при нанесении урона спящему игроку (true - да/false - нет)")]
    public bool ActivateOnSleeperAttack;
    [JsonProperty(LanguageEn ? "Deactivate combat mode after death (true - yes/false - no)" : "Деактивировать комбат-блок после смерти (true - да/false - нет)")]
    public bool DeactivateOnPlayerDeath;
}

private static class ObjectCache
{
    private static readonly object True;
    private static readonly object False;
    private static class StaticObjectCache
    {
        private static readonly Dictionary<T, object> CacheByValue;
        public static object Get(T value);
    }

    public static object Get(T value);
    public static object Get(bool value);
}

private static class StaticObjectCache
{
    private static readonly Dictionary<T, object> CacheByValue;
    public static object Get(T value);
}


```

---

## ComfortBed

```csharp
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("ComfortBed", "Mercury", "0.0.2")]
[Description("Самый лучший пионер Mercury")]
 class ComfortBed : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
     string ShortnameBed;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("SkinID для кровати")]
        public ulong SkinID;
        [JsonProperty("DisplayName для кровати")]
        public string DisplayName;
        [JsonProperty("Ломать кровать при возрождении(Сделать ее одноразовой)")]
        public bool KillBed;
        [JsonProperty("Настройки пользователя при возрождении")]
        public MetabolismUser metabolismUser;
        internal class MetabolismUser
        {
            [JsonProperty("Кол-во ХП при возраждении на кровати")]
            public int Health;
            [JsonProperty("Кол-во ЖАЖДЫ при возраждении на кровати")]
            public int Water;
            [JsonProperty("Кол-во СЫТНОСТИ при возраждении на кровати")]
            public int Hungry;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void CreateItem(BasePlayer player);
    private object OnPlayerRespawn(BasePlayer player, SleepingBag bag);
     void OnEntitySpawned(BaseNetworkable entity);
    [ConsoleCommand("cb")]
     void ComfortBedCommand(ConsoleSystem.Arg arg);
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    private new void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("SkinID для кровати")]
    public ulong SkinID;
    [JsonProperty("DisplayName для кровати")]
    public string DisplayName;
    [JsonProperty("Ломать кровать при возрождении(Сделать ее одноразовой)")]
    public bool KillBed;
    [JsonProperty("Настройки пользователя при возрождении")]
    public MetabolismUser metabolismUser;
    internal class MetabolismUser
    {
        [JsonProperty("Кол-во ХП при возраждении на кровати")]
        public int Health;
        [JsonProperty("Кол-во ЖАЖДЫ при возраждении на кровати")]
        public int Water;
        [JsonProperty("Кол-во СЫТНОСТИ при возраждении на кровати")]
        public int Hungry;
    }

    public static Configuration GetNewConfiguration();
}

internal class MetabolismUser
{
    [JsonProperty("Кол-во ХП при возраждении на кровати")]
    public int Health;
    [JsonProperty("Кол-во ЖАЖДЫ при возраждении на кровати")]
    public int Water;
    [JsonProperty("Кол-во СЫТНОСТИ при возраждении на кровати")]
    public int Hungry;
}


```

---

## ComfortBed (1)

```csharp
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("ComfortBed", "Sempai#3239", "0.0.2")]
[Description("Самый лучший пионер Sempai#3239")]
 class ComfortBed : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
     string ShortnameBed;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("SkinID для кровати")]
        public ulong SkinID;
        [JsonProperty("DisplayName для кровати")]
        public string DisplayName;
        [JsonProperty("Ломать кровать при возрождении(Сделать ее одноразовой)")]
        public bool KillBed;
        [JsonProperty("Настройки пользователя при возрождении")]
        public MetabolismUser metabolismUser;
        internal class MetabolismUser
        {
            [JsonProperty("Кол-во ХП при возраждении на кровати")]
            public int Health;
            [JsonProperty("Кол-во ЖАЖДЫ при возраждении на кровати")]
            public int Water;
            [JsonProperty("Кол-во СЫТНОСТИ при возраждении на кровати")]
            public int Hungry;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void CreateItem(BasePlayer player);
    private object OnPlayerRespawn(BasePlayer player, SleepingBag bag);
     void OnEntitySpawned(BaseNetworkable entity);
    [ConsoleCommand("cb")]
     void ComfortBedCommand(ConsoleSystem.Arg arg);
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    private new void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("SkinID для кровати")]
    public ulong SkinID;
    [JsonProperty("DisplayName для кровати")]
    public string DisplayName;
    [JsonProperty("Ломать кровать при возрождении(Сделать ее одноразовой)")]
    public bool KillBed;
    [JsonProperty("Настройки пользователя при возрождении")]
    public MetabolismUser metabolismUser;
    internal class MetabolismUser
    {
        [JsonProperty("Кол-во ХП при возраждении на кровати")]
        public int Health;
        [JsonProperty("Кол-во ЖАЖДЫ при возраждении на кровати")]
        public int Water;
        [JsonProperty("Кол-во СЫТНОСТИ при возраждении на кровати")]
        public int Hungry;
    }

    public static Configuration GetNewConfiguration();
}

internal class MetabolismUser
{
    [JsonProperty("Кол-во ХП при возраждении на кровати")]
    public int Health;
    [JsonProperty("Кол-во ЖАЖДЫ при возраждении на кровати")]
    public int Water;
    [JsonProperty("Кол-во СЫТНОСТИ при возраждении на кровати")]
    public int Hungry;
}


```

---

## CommandRateLimiter

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using UnityEngine;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("CommandRateLimiter", "Calytic", "0.0.9", ResourceId = 1812)]
public class CommandRateLimiter : CovalencePlugin
{
    private int KickAfter;
    private int CooldownMS;
    private int ClearRateCountSeconds;
    private bool LogExcessiveUsage;
    private bool SendPlayerMessage;
     Dictionary<string, DateTime> lastRun;
     Dictionary<string, Timer> rateTimer;
     Dictionary<string, int> rateCount;
     List<object> commandWhitelist;
    private Dictionary<string, Dictionary<string, int>> spamLog;
     void OnServerInitialized();
     void LoadMessages();
     void LoadDefaultConfig();
    private T GetConfig(string name, T defaultValue);
     object OnServerCommand(ConsoleSystem.Arg arg);
     void OnUserDisconnected(IPlayer player);
     string GetMsg(string key, object userID);
}


```

---

## CommercialNick

```csharp
using Oxide.Core.Plugins;
using System.Collections.Generic;
using ConVar;
using System;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("CommercialNick+", "Sempai#3239", "2.6.0")]
[Description("Плагин позволяющий давать награду за приставку в нике , например название вашего сервера")]
internal class CommercialNick : RustPlugin
{
    private void SaveConfig(Configuration config);
    private void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private Configuration config;
    public class setings
    {
        [JsonProperty("Использовать выдачу баланса ?")]
        public bool GameStore;
        [JsonProperty("Бонус в виде баланса GameStores или OVH (если не нужно оставить пустым)")]
        public string GameStoreBonus;
        [JsonProperty("У вас магазин ОВХ?")]
        public bool OVHStore;
        [JsonProperty("[GameStores] ID магазина")]
        public string ShopID;
        [JsonProperty("[GameStores] ID сервера")]
        public string ServerID;
        [JsonProperty("[GameStores] Секретный ключ")]
        public string SecretKey;
        [JsonProperty("Лог сообщения(Показывается в магазине после выдачи в истории. Если OVH оставить пустым)")]
        public string GameStoreMSG;
        [JsonProperty("Использовать выдачу привилегии")]
        public bool commands;
        [JsonProperty("Команда для выдачи")]
        public string commandsgo;
        [JsonProperty("Названия того что он получит от команды")]
        public string commandprize;
        [JsonProperty("Время которое нужно отыграть игроку с приставкой в нике что бы получить награду (секунды)")]
        public int timeplay;
        [JsonProperty("Разршить после вайпа получить приз заново")]
        public bool wipeclear;
    }

    private void OnNewSave(string filename);
    public void LoadConfigVars();
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    [PluginReference]
    private readonly Plugin IQChat;
    public class Configuration
    {
        [JsonProperty("Настройки")]
        public setings seting;
        [JsonProperty("Наградить за что то в нике:")]
        public List<string> CONF_BlockedParts;
    }

    private Dictionary<ulong, bool> ConnectedPlayers;
    protected override void LoadDefaultConfig();
    private void OnServerSave();
    public void PrizeGive(ulong id);
    private bool ContainsAny(string input, List<string> check);
    private void OnServerInitialized();
    private void NickName(BasePlayer player);
}

public class setings
{
    [JsonProperty("Использовать выдачу баланса ?")]
    public bool GameStore;
    [JsonProperty("Бонус в виде баланса GameStores или OVH (если не нужно оставить пустым)")]
    public string GameStoreBonus;
    [JsonProperty("У вас магазин ОВХ?")]
    public bool OVHStore;
    [JsonProperty("[GameStores] ID магазина")]
    public string ShopID;
    [JsonProperty("[GameStores] ID сервера")]
    public string ServerID;
    [JsonProperty("[GameStores] Секретный ключ")]
    public string SecretKey;
    [JsonProperty("Лог сообщения(Показывается в магазине после выдачи в истории. Если OVH оставить пустым)")]
    public string GameStoreMSG;
    [JsonProperty("Использовать выдачу привилегии")]
    public bool commands;
    [JsonProperty("Команда для выдачи")]
    public string commandsgo;
    [JsonProperty("Названия того что он получит от команды")]
    public string commandprize;
    [JsonProperty("Время которое нужно отыграть игроку с приставкой в нике что бы получить награду (секунды)")]
    public int timeplay;
    [JsonProperty("Разршить после вайпа получить приз заново")]
    public bool wipeclear;
}

public class Configuration
{
    [JsonProperty("Настройки")]
    public setings seting;
    [JsonProperty("Наградить за что то в нике:")]
    public List<string> CONF_BlockedParts;
}


```

---

## ComponentBlocker

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("ComponentBlocker", "Calytic", "0.1.2", ResourceId = 1382)]
 class ComponentBlocker : RustPlugin
{
     List<string> blockList;
     List<string> blockCache;
     bool enabled;
     bool craftRefund;
    private bool sendMessages;
    private Dictionary<string, string> messages;
    private List<string> texts;
     void OnServerInitialized();
     void LoadData();
     void LoadDefaultConfig();
    protected void ReloadConfig();
    private void SendHelpText(BasePlayer player);
    [ChatCommand("listinv")]
    private void cmdListInv(BasePlayer player, string command, string[] args);
    [ChatCommand("clearblocklist")]
    private void cmdClearBlockList(BasePlayer player, string command, string[] args);
    [ChatCommand("blocklist")]
    private void cmdBlockList(BasePlayer player, string command, string[] args);
    [ChatCommand("blocker")]
    private void cmdBlock(BasePlayer player, string command, string[] args);
    [ConsoleCommand("blocker")]
     void ccBlock(ConsoleSystem.Arg arg);
     void AddBlock(string name);
     void RemoveBlock(string name);
     bool IsBlocking(string name);
     T GetConfig(string key, T defaultValue);
     void OnPlayerInit(BasePlayer player);
     object OnItemCraft(ItemCraftTask task);
    private void RefundIngredients(ItemBlueprint bp, BasePlayer player, int amount);
     void OnEntitySpawned(BaseNetworkable networkable);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnItemDeployed(Deployer deployer, BaseEntity entity);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnPlantGather(PlantEntity plant, Item item, BasePlayer player);
     void OnQuarryGather(MiningQuarry quarry, Item item);
    private bool CheckItem(Item item);
    private bool CheckNetworkable(BaseNetworkable networkable);
     object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
    private bool isBlocked(string[] names);
}


```

---

## ComponentBox

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("ComponentBox", "TopPlugin.ru", "2.0.2"), Description("Allows players to store components in a secondary container carried in their inventory")]
 class ComponentBox : RustPlugin
{
    private const int PRESENT_ITEM_ID;
    private const ulong PRESENT_SKIN_ID;
    private const string PERMISSION_USE;
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private object OnItemPickup(Item item, BasePlayer player);
    private object CanAcceptItem(ItemContainer container, Item componentBox, int target);
    private object CanMoveItem(Item movedItem, PlayerInventory inventory, uint targetContainerID, int targetSlot, int amount);
    private void GiveComponentBox(BasePlayer player);
    private void SetupComponentBoxes(IEnumerable<Item> items);
    private void SetupComponentBox(Item componentBox);
    private IEnumerable<Item> FindComponentBoxes(BasePlayer player);
    private void BeginLootingComponentBox(BasePlayer player, Item item, bool isInInventory);
    private void DropComponentBoxItems(BasePlayer player);
    private void MoveContentsToPlayer(Item componentBox, BasePlayer player, IEnumerable<Item> componentBoxes);
    [ConsoleCommand("toolbox")]
    private void CraftToolboxConsoleCommand(ConsoleSystem.Arg arg);
    [ChatCommand("toolbox")]
    private void CraftToolboxCommand(BasePlayer player, string command, string[] args);
    private ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Leave component box on corpse when player is killed")]
        public bool LeaveOnCorpse { get; set; }
        [JsonProperty(PropertyName = "Automatically place allowed items in the component box when picked up")]
        public bool DepositAutomatically { get; set; }
        [JsonProperty(PropertyName = "Number of slots in the component box (1 - 36)")]
        public int Slots { get; set; }
        [JsonProperty(PropertyName = "Allow players to drop the component box")]
        public bool IsDroppable { get; set; }
        [JsonProperty(PropertyName = "Allow players to store the component box in other containers")]
        public bool IsStorable { get; set; }
        [JsonProperty(PropertyName = "Only allow players to carry 1 component box at a time")]
        public bool LimitCarryAmount { get; set; }
        [JsonProperty(PropertyName = "Crafting Settings")]
        public CraftingSettings Crafting { get; set; }
        [JsonProperty(PropertyName = "Item Settings")]
        public ItemSettings Items { get; set; }
        public class ItemSettings
        {
            [JsonProperty(PropertyName = "Allowed item categories")]
            public Hash<ItemCategory, bool> Categories { get; set; }
            [JsonProperty(PropertyName = "Blocked item shortnames")]
            public string[] BlockedItems { get; set; }
            [JsonProperty(PropertyName = "Allowed item shortnames")]
            public string[] AllowedItems { get; set; }
            public bool CanAcceptItem(Item item);
        }

        public class CraftingSettings
        {
            [JsonProperty(PropertyName = "Require component box to be crafted")]
            public bool RequiresCrafting { get; set; }
            [JsonProperty(PropertyName = "Refund crafting cost when destroying the component box")]
            public bool RefundOnDestroy { get; set; }
            [JsonProperty(PropertyName = "Refund cost fraction (0.0 - 1.0)")]
            public float RefundPercentage { get; set; }
            [JsonProperty(PropertyName = "Cost to craft a component box")]
            public List<CraftItem> Cost { get; set; }
            [JsonIgnore]
            private IEnumerable<string> craftingRequirements;
            [JsonIgnore]
            public IEnumerable<string> Requirements { get; set; }
            public bool CanAffordToCraft(BasePlayer player);
            public void PayCraftCost(BasePlayer player);
            public void RefundCraftCost(BasePlayer player);
            public class CraftItem
            {
                public string Shortname { get; set; }
                public int Amount { get; set; }
                [JsonIgnore]
                public int ItemID { get; set; }
                [JsonIgnore]
                public string CostString { get; set; }
                [JsonIgnore]
                public bool IsValid { get; set; }
                public void Validate();
            }

        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void Message(BasePlayer player, string key, string[] args);
    private readonly Dictionary<string, string> Messages;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Leave component box on corpse when player is killed")]
    public bool LeaveOnCorpse { get; set; }
    [JsonProperty(PropertyName = "Automatically place allowed items in the component box when picked up")]
    public bool DepositAutomatically { get; set; }
    [JsonProperty(PropertyName = "Number of slots in the component box (1 - 36)")]
    public int Slots { get; set; }
    [JsonProperty(PropertyName = "Allow players to drop the component box")]
    public bool IsDroppable { get; set; }
    [JsonProperty(PropertyName = "Allow players to store the component box in other containers")]
    public bool IsStorable { get; set; }
    [JsonProperty(PropertyName = "Only allow players to carry 1 component box at a time")]
    public bool LimitCarryAmount { get; set; }
    [JsonProperty(PropertyName = "Crafting Settings")]
    public CraftingSettings Crafting { get; set; }
    [JsonProperty(PropertyName = "Item Settings")]
    public ItemSettings Items { get; set; }
    public class ItemSettings
    {
        [JsonProperty(PropertyName = "Allowed item categories")]
        public Hash<ItemCategory, bool> Categories { get; set; }
        [JsonProperty(PropertyName = "Blocked item shortnames")]
        public string[] BlockedItems { get; set; }
        [JsonProperty(PropertyName = "Allowed item shortnames")]
        public string[] AllowedItems { get; set; }
        public bool CanAcceptItem(Item item);
    }

    public class CraftingSettings
    {
        [JsonProperty(PropertyName = "Require component box to be crafted")]
        public bool RequiresCrafting { get; set; }
        [JsonProperty(PropertyName = "Refund crafting cost when destroying the component box")]
        public bool RefundOnDestroy { get; set; }
        [JsonProperty(PropertyName = "Refund cost fraction (0.0 - 1.0)")]
        public float RefundPercentage { get; set; }
        [JsonProperty(PropertyName = "Cost to craft a component box")]
        public List<CraftItem> Cost { get; set; }
        [JsonIgnore]
        private IEnumerable<string> craftingRequirements;
        [JsonIgnore]
        public IEnumerable<string> Requirements { get; set; }
        public bool CanAffordToCraft(BasePlayer player);
        public void PayCraftCost(BasePlayer player);
        public void RefundCraftCost(BasePlayer player);
        public class CraftItem
        {
            public string Shortname { get; set; }
            public int Amount { get; set; }
            [JsonIgnore]
            public int ItemID { get; set; }
            [JsonIgnore]
            public string CostString { get; set; }
            [JsonIgnore]
            public bool IsValid { get; set; }
            public void Validate();
        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class ItemSettings
{
    [JsonProperty(PropertyName = "Allowed item categories")]
    public Hash<ItemCategory, bool> Categories { get; set; }
    [JsonProperty(PropertyName = "Blocked item shortnames")]
    public string[] BlockedItems { get; set; }
    [JsonProperty(PropertyName = "Allowed item shortnames")]
    public string[] AllowedItems { get; set; }
    public bool CanAcceptItem(Item item);
}

public class CraftingSettings
{
    [JsonProperty(PropertyName = "Require component box to be crafted")]
    public bool RequiresCrafting { get; set; }
    [JsonProperty(PropertyName = "Refund crafting cost when destroying the component box")]
    public bool RefundOnDestroy { get; set; }
    [JsonProperty(PropertyName = "Refund cost fraction (0.0 - 1.0)")]
    public float RefundPercentage { get; set; }
    [JsonProperty(PropertyName = "Cost to craft a component box")]
    public List<CraftItem> Cost { get; set; }
    [JsonIgnore]
    private IEnumerable<string> craftingRequirements;
    [JsonIgnore]
    public IEnumerable<string> Requirements { get; set; }
    public bool CanAffordToCraft(BasePlayer player);
    public void PayCraftCost(BasePlayer player);
    public void RefundCraftCost(BasePlayer player);
    public class CraftItem
    {
        public string Shortname { get; set; }
        public int Amount { get; set; }
        [JsonIgnore]
        public int ItemID { get; set; }
        [JsonIgnore]
        public string CostString { get; set; }
        [JsonIgnore]
        public bool IsValid { get; set; }
        public void Validate();
    }

}

public class CraftItem
{
    public string Shortname { get; set; }
    public int Amount { get; set; }
    [JsonIgnore]
    public int ItemID { get; set; }
    [JsonIgnore]
    public string CostString { get; set; }
    [JsonIgnore]
    public bool IsValid { get; set; }
    public void Validate();
}


```

---

## ComponentPlus

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Newtonsoft.Json;

Oxide.Plugins
[Info("ComponentPlus", "Nimant", "1.0.1")]
 class ComponentPlus : RustPlugin
{
    private static System.Random ieFBMxuqzNswDjfuYMBTQFeygbiLKt;
    private static HashSet<uint> ygEgUlTFsRi;
    private void Init();
    private void OnServerInitialized();
    private void OnNewSave();
    private void OnServerSave();
    private void Unload();
    private void OnLootEntity(BasePlayer MuVRSRPMeAsbljnHT, BaseEntity MHfWIIkPbh);
    private void OnEntityDeath(BaseCombatEntity MHfWIIkPbh, HitInfo FQcIjcRMwFIBBDibJtYtkdbS);
    private void RTPvQzhdQpnltCdunEPH(BasePlayer MuVRSRPMeAsbljnHT, ItemContainer ccsoVoyWjQmtwAtGSG);
    private float lZHPMCdlqQmfVUHc(BasePlayer MuVRSRPMeAsbljnHT);
    [HookMethod("ExcludeContainer")]
    public void ExcludeContainer(uint entityID);
    [HookMethod("ChangeContLoot")]
    public void ChangeContLoot(LootContainer ccsoVoyWjQmtwAtGSG);
    private static FxpbuOJygnsWd cKSPjBbzvHdaOeMaMOs;
    private class VgHHUoIjtOoQZEFQ
    {
        [JsonProperty(PropertyName = "Использовать дефолтные значения минимума и максимума")]
        public bool WuLBqqWLrUheHJSeXmpiNYz;
        [JsonProperty(PropertyName = "Минимум")]
        public int gNVNmcNlIx;
        [JsonProperty(PropertyName = "Максимум (если 0 - компонент будет удалён)")]
        public int EnmPKSvwtwutqMfBacziYBjsppABru;
        [JsonProperty(PropertyName = "Индивидуальный рейт относительно общего (если 0 - компонент будет удалён)")]
        public float nvQoOpzcKhevflyYqkgTdazB;
    }

    private class FxpbuOJygnsWd
    {
        [JsonProperty(PropertyName = "Общий множитель компонентов")]
        public float MAagJwexxjGxKBooqoUoCpjjVyurta;
        [JsonProperty(PropertyName = "Изменение общего множителя компонентов для игроков с привилегиями")]
        public Dictionary<string, float> axOuBSVXrbiijLEnccrV;
        [JsonProperty(PropertyName = "Изменение количества выпадаемых компонентов")]
        public Dictionary<string, VgHHUoIjtOoQZEFQ> ZOoNYwHQLaBZnNUZCITuYFQumgUMu;
    }

    private void ardgnTYlTTGYktPDEDtMkYLWjoiac();
    protected override void LoadDefaultConfig();
    private void zIAEzlElrCMpJSwZK(FxpbuOJygnsWd ByDhRZemcahCmKCyITa);
    private void PsHkXHyBfMSuBYAnQVyv();
    private void jQoJOXYMeMuQtZxewIempL();
}

private class VgHHUoIjtOoQZEFQ
{
    [JsonProperty(PropertyName = "Использовать дефолтные значения минимума и максимума")]
    public bool WuLBqqWLrUheHJSeXmpiNYz;
    [JsonProperty(PropertyName = "Минимум")]
    public int gNVNmcNlIx;
    [JsonProperty(PropertyName = "Максимум (если 0 - компонент будет удалён)")]
    public int EnmPKSvwtwutqMfBacziYBjsppABru;
    [JsonProperty(PropertyName = "Индивидуальный рейт относительно общего (если 0 - компонент будет удалён)")]
    public float nvQoOpzcKhevflyYqkgTdazB;
}

private class FxpbuOJygnsWd
{
    [JsonProperty(PropertyName = "Общий множитель компонентов")]
    public float MAagJwexxjGxKBooqoUoCpjjVyurta;
    [JsonProperty(PropertyName = "Изменение общего множителя компонентов для игроков с привилегиями")]
    public Dictionary<string, float> axOuBSVXrbiijLEnccrV;
    [JsonProperty(PropertyName = "Изменение количества выпадаемых компонентов")]
    public Dictionary<string, VgHHUoIjtOoQZEFQ> ZOoNYwHQLaBZnNUZCITuYFQumgUMu;
}


```

---

## ConnectionDB

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("ConnectionDB", "Norn", 0.2, ResourceId = 1459)]
[Description("Connection database for devs.")]
public class ConnectionDB : RustPlugin
{
     class StoredData
    {
        public Dictionary<ulong, PlayerInfo> PlayerInfo;
        public StoredData();
    }

     class PlayerInfo
    {
        public ulong uUserID;
        public string tFirstName;
        public string tLastName;
        public int iInitTimestamp;
        public int iLastSeen;
        public string tInitIP;
        public string tLastIP;
        public int iSecondsPlayed;
        public int iConnections;
        public string tReason;
        public bool bAlive;
        public PlayerInfo();
    }

     StoredData DB_Connection;
     void Unload();
     void SaveData();
    private bool InitPlayer(BasePlayer player);
    private void SyncAlive(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
    private bool SaveConnectionDataFromID(ulong steamid, string reason);
    private void SaveConnectionData(BasePlayer player, string reason);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private Int32 UnixTimeStampUTC();
    static readonly DateTime UnixEpoch;
    static readonly double MaxUnixSeconds;
    private static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private DateTime FirstSeenFromID(ulong steamid);
    private int Connections(BasePlayer player);
    private int ConnectionsFromID(ulong steamid);
    private int SecondsPlayed(BasePlayer player);
    private int SecondsPlayedFromID(ulong steamid);
    private string FirstIP(BasePlayer player);
    private string FirstIPFromID(ulong steamid);
    private string LastIP(BasePlayer player);
    private string LastIPFromID(ulong steamid);
    private string LastName(BasePlayer player);
    private string LastNameFromID(ulong steamid);
    private string FirstName(BasePlayer player);
    private string FirstNameFromID(ulong steamid);
    private string DisconnectReasonFromID(ulong steamid);
    private string DisconnectReason(BasePlayer player);
    private bool IsPlayerAliveFromID(ulong steamid);
    private DateTime FirstSeen(BasePlayer player);
    private DateTime LastSeen(BasePlayer player);
    private DateTime ConfigInitTimestamp();
    private DateTime LastSeenFromID(ulong steamid);
    protected override void LoadDefaultConfig();
     System.Random rnd;
    protected int GetRandomInt(int min, int max);
     void Loaded();
    private bool ConnectionDataExistsFromID(ulong steamid);
    private bool ConnectionDataExists(BasePlayer player);
    private int UniqueConnections();
    private int ConnectionPlayerCount();
    private void UpdateSeconds();
     Timer SecondsCount;
     void OnServerInitialized();
}

 class StoredData
{
    public Dictionary<ulong, PlayerInfo> PlayerInfo;
    public StoredData();
}

 class PlayerInfo
{
    public ulong uUserID;
    public string tFirstName;
    public string tLastName;
    public int iInitTimestamp;
    public int iLastSeen;
    public string tInitIP;
    public string tLastIP;
    public int iSecondsPlayed;
    public int iConnections;
    public string tReason;
    public bool bAlive;
    public PlayerInfo();
}


```

---

## ConnectionFix

```csharp
using System.Linq;
using Network;

Oxide.Plugins
[Info("ConnectionFix", "Zirper", "1.0.0")]
 class ConnectionFix : RustPlugin
{
     void OnClientAuth(Connection connection);
}


```

---

## ConnectionInfo

```csharp
using System;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("ConnectionInfo", "Norn", 0.1, ResourceId = 1460)]
[Description("Basic player information.")]
public class ConnectionInfo : RustPlugin
{
    [PluginReference]
     Plugin ConnectionDB;
    [ChatCommand("pinfo")]
    private void PlayerCommand(BasePlayer player, string command, string[] args);
     void Loaded();
}


```

---

## ConsoleMessage

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("ConsoleMessages", "Skrallex", "1.0.0")]
[Description("Send messages to players with a console command")]
 class ConsoleMessage : RustPlugin
{
     void Loaded();
     void LoadDefaultMessages();
    [ConsoleCommand("cm.say")]
     void CMSay(ConsoleSystem.Arg args);
     List<BasePlayer> GetPlayersByName(string playerName);
     string Lang(string key);
}


```

---

## ConsoleMessages

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("ConsoleMessages", "Skrallex", "1.1.1", ResourceId = 2093)]
[Description("Send messages to players with a console command")]
 class ConsoleMessages : RustPlugin
{
     bool UsePermissionsOnly;
    const string adminPerm;
    const string sayPerm;
    const string sayAllPerm;
    const string replyPerm;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadConfig();
     void LoadDefaultMessages();
    [ConsoleCommand("cm.say")]
     void ConsoleCmdCMSay(ConsoleSystem.Arg args);
    [ConsoleCommand("cm.saynp")]
     void ConsoleCmdCMSayNP(ConsoleSystem.Arg args);
    [ConsoleCommand("cm.sayall")]
     void ConsoleCmdCMSayAll(ConsoleSystem.Arg args);
    [ConsoleCommand("cm.sayallnp")]
     void ConsoleCmdCMSayAllNP(ConsoleSystem.Arg args);
     void CMSay(ConsoleSystem.Arg args, bool usePrefix);
     void CMSayAll(ConsoleSystem.Arg args, bool usePrefix);
    [ChatCommand("cm_reply")]
     void ChatCmdCMReply(BasePlayer player, string cmd, string[] args);
     List<BasePlayer> GetPlayersByName(string playerName);
     void ReplyPlayer(BasePlayer player, string langKey, bool usePrefix);
     void ReplyFormatted(BasePlayer player, string msg, bool usePrefix);
     void ReplyConsole(ConsoleSystem.Arg args, string langKey, bool usePrefix);
     void ReplyConsoleFormatted(ConsoleSystem.Arg args, string msg, bool usePrefix);
     bool IsAllowed(BasePlayer player, string perm);
     string Lang(string key);
}


```

---

## CopterSpawns

```csharp
using Facepunch;
using Rust;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System;

Oxide.Plugins
[Info("CopterSpawns", "OxideBro", "0.0.1")]
public class CopterSpawns : RustPlugin
{
     string copter;
    private void OnServerInitialized();
     void OnEntityKill(BaseNetworkable entity);
     void StartSpawn();
     SpawnFilter filter;
     List<Vector3> monuments;
    static float GetGroundPosition(Vector3 pos);
    public Vector3 RandomDropPosition();
     List<int> BlockedLayers;
    static int blockedMask;
    public Vector3 GetSafeDropPosition(Vector3 position);
    public Vector3 GetEventPosition();
}


```

---

## CoptorTracker

```csharp
using System;
using UnityEngine;

Oxide.Plugins
[Info("Chopper Tracker", "Smoosher", "1.6.5")]
[Description("Tracking Of The Coptor")]
 class CoptorTracker : RustPlugin
{
     DateTime TimerStart;
     int ChopperSpawnTime;
     float ChopperLifeTimeOriginal;
     float ChopperLifeTimeCurrent;
     DateTime TimerSpawn;
     DateTime ChopperSpawned;
     bool SpawnedHeli;
    protected override void LoadDefaultConfig();
    private void SetConfig();
    private bool TrueorFalse(string input);
     void Loaded();
    [ChatCommand("Nextheli")]
    private void NextCoptor(BasePlayer player, string command, string[] args);
    [ChatCommand("KillAllHelis")]
    private void KillHelis(BasePlayer player, string command, string[] args);
    [ChatCommand("SpawnHeli")]
    private void SpawnHeli(BasePlayer player, string command, string[] args);
    private void StartChopperSpawnFreq();
    private void SetChopperLifetimeMins();
    private void SpawnChopper();
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void KillCoptor(BaseNetworkable entity);
    private void KillCoptor();
}


```

---

## CopyPaste

```csharp
using Facepunch;
using Oxide.Core;
using Oxide.Core.Libraries;
using Newtonsoft.Json;
using ProtoBuf;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("Copy Paste", "FIX BY HUZAKI| Reneb ", "3.5.4", ResourceId = 716)]
[Description("Copy and paste your buildings to save them or move them")]
 class CopyPaste : RustPlugin
{
    private int copyLayer;
    private int groundLayer;
    private int rayCopy;
    private int rayPaste;
    private string copyPermission;
    private string pastePermission;
    private string undoPermission;
    private string serverID;
    private string subDirectory;
    private Dictionary<string, Stack<List<BaseEntity>>> lastPastes;
    private List<BaseEntity.Slot> checkSlots;
    private FieldInfo _equippingActive;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Copy Options")]
        public CopyOptions Copy { get; set; }
        [JsonProperty(PropertyName = "Paste Options")]
        public PasteOptions Paste { get; set; }
        public class CopyOptions
        {
            [JsonProperty(PropertyName = "Buildings (true/false)")]
            [DefaultValue(true)]
            public bool Buildings { get; set; }
            [JsonProperty(PropertyName = "Deployables (true/false)")]
            [DefaultValue(true)]
            public bool Deployables { get; set; }
            [JsonProperty(PropertyName = "Check radius from each entity (true/false)")]
            [DefaultValue(true)]
            public bool EachToEach { get; set; }
            [JsonProperty(PropertyName = "Inventories (true/false)")]
            [DefaultValue(true)]
            public bool Inventories { get; set; }
            [JsonProperty(PropertyName = "Share (true/false)")]
            [DefaultValue(false)]
            public bool Share { get; set; }
            [JsonProperty(PropertyName = "Tree (true/false)")]
            [DefaultValue(false)]
            public bool Tree { get; set; }
        }

        public class PasteOptions
        {
            [JsonProperty(PropertyName = "Auth (true/false)")]
            [DefaultValue(false)]
            public bool Auth { get; set; }
            [JsonProperty(PropertyName = "Deployables (true/false)")]
            [DefaultValue(true)]
            public bool Deployables { get; set; }
            [JsonProperty(PropertyName = "Inventories (true/false)")]
            [DefaultValue(true)]
            public bool Inventories { get; set; }
            [JsonProperty(PropertyName = "Vending Machines (true/false)")]
            [DefaultValue(true)]
            public bool VendingMachines { get; set; }
            [JsonProperty(PropertyName = "Stability (true/false)")]
            [DefaultValue(true)]
            public bool Stability { get; set; }
        }

    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void Init();
    private void OnServerInitialized();
     object TryCopyFromSteamID(ulong userID, string filename, string[] args);
     object TryPasteFromSteamID(ulong userID, string filename, string[] args);
     object TryPasteFromVector3(Vector3 pos, float rotationCorrection, string filename, string[] args);
    private object CheckCollision(List<Dictionary<string,object>> entities, Vector3 startPos, float radius);
    private bool CheckPlaced(string prefabname, Vector3 pos, Quaternion rot);
    private object cmdPasteBack(BasePlayer player, string[] args);
    private object cmdUndo(string userIDString, string[] args);
    private object Copy(Vector3 sourcePos, Vector3 sourceRot, string filename, float RotationCorrection, CopyMechanics copyMechanics, float range, bool saveBuildings, bool saveDeployables, bool saveInventories, bool saveTree, bool saveShare, bool eachToEach);
    private object CopyProcess(Vector3 sourcePos, Vector3 sourceRot, float RotationCorrection, float range, bool saveBuildings, bool saveDeployables, bool saveInventories, bool saveTree, bool saveShare, CopyMechanics copyMechanics, bool eachToEach);
    private Dictionary<string, object> EntityData(BaseEntity entity, Vector3 sourcePos, Vector3 sourceRot, Vector3 entPos, Vector3 entRot, float diffRot, bool saveInventories, bool saveShare);
    private object FindBestHeight(List<Dictionary<string,object>> entities, Vector3 startPos);
    private bool FindRayEntity(Vector3 sourcePos, Vector3 sourceDir, Vector3 point, BaseEntity entity, int rayLayer);
    private object GetGround(Vector3 pos);
    private bool HasAccess(BasePlayer player, string permName);
    private bool IsValid(BaseEntity entity);
    private string Lang(string key, string userID, object[] args);
    private Vector3 NormalizePosition(Vector3 InitialPos, Vector3 CurrentPos, float diffRot);
    private List<BaseEntity> Paste(List<Dictionary<string,object>> entities, Vector3 startPos, BasePlayer player, bool stability);
    private List<Dictionary<string, object>> PreLoadData(List<object> entities, Vector3 startPos, float RotationCorrection, bool deployables, bool inventories, bool auth, bool vending);
    private object TryCopy(Vector3 sourcePos, Vector3 sourceRot, string filename, float RotationCorrection, string[] args);
    private void TryCopySlots(BaseEntity ent, IDictionary<string, object> housedata, bool saveShare);
    private Dictionary<string, object> TryCopyFlags(BaseEntity entity);
    private object TryPaste(Vector3 startPos, string filename, BasePlayer player, float RotationCorrection, string[] args, bool autoHeight);
    private List<BaseEntity> TryPasteSlots(BaseEntity ent, Dictionary<string, object> structure);
    private object TryPasteBack(string filename, BasePlayer player, string[] args);
    [ChatCommand("copy")]
    private void cmdChatCopy(BasePlayer player, string command, string[] args);
    [ChatCommand("paste")]
    private void cmdChatPaste(BasePlayer player, string command, string[] args);
    [ChatCommand("pasteback")]
    private void cmdChatPasteBack(BasePlayer player, string command, string[] args);
    [ChatCommand("undo")]
    private void cmdChatUndo(BasePlayer player, string command, string[] args);
    [ConsoleCommand("pasteback")]
    private void cmdConsolePasteBack(ConsoleSystem.Arg arg);
    [ConsoleCommand("undo")]
    private void cmdConsoleUndo(ConsoleSystem.Arg arg);
    private readonly Dictionary<string, Dictionary<string, string>> messages;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Copy Options")]
    public CopyOptions Copy { get; set; }
    [JsonProperty(PropertyName = "Paste Options")]
    public PasteOptions Paste { get; set; }
    public class CopyOptions
    {
        [JsonProperty(PropertyName = "Buildings (true/false)")]
        [DefaultValue(true)]
        public bool Buildings { get; set; }
        [JsonProperty(PropertyName = "Deployables (true/false)")]
        [DefaultValue(true)]
        public bool Deployables { get; set; }
        [JsonProperty(PropertyName = "Check radius from each entity (true/false)")]
        [DefaultValue(true)]
        public bool EachToEach { get; set; }
        [JsonProperty(PropertyName = "Inventories (true/false)")]
        [DefaultValue(true)]
        public bool Inventories { get; set; }
        [JsonProperty(PropertyName = "Share (true/false)")]
        [DefaultValue(false)]
        public bool Share { get; set; }
        [JsonProperty(PropertyName = "Tree (true/false)")]
        [DefaultValue(false)]
        public bool Tree { get; set; }
    }

    public class PasteOptions
    {
        [JsonProperty(PropertyName = "Auth (true/false)")]
        [DefaultValue(false)]
        public bool Auth { get; set; }
        [JsonProperty(PropertyName = "Deployables (true/false)")]
        [DefaultValue(true)]
        public bool Deployables { get; set; }
        [JsonProperty(PropertyName = "Inventories (true/false)")]
        [DefaultValue(true)]
        public bool Inventories { get; set; }
        [JsonProperty(PropertyName = "Vending Machines (true/false)")]
        [DefaultValue(true)]
        public bool VendingMachines { get; set; }
        [JsonProperty(PropertyName = "Stability (true/false)")]
        [DefaultValue(true)]
        public bool Stability { get; set; }
    }

}

public class CopyOptions
{
    [JsonProperty(PropertyName = "Buildings (true/false)")]
    [DefaultValue(true)]
    public bool Buildings { get; set; }
    [JsonProperty(PropertyName = "Deployables (true/false)")]
    [DefaultValue(true)]
    public bool Deployables { get; set; }
    [JsonProperty(PropertyName = "Check radius from each entity (true/false)")]
    [DefaultValue(true)]
    public bool EachToEach { get; set; }
    [JsonProperty(PropertyName = "Inventories (true/false)")]
    [DefaultValue(true)]
    public bool Inventories { get; set; }
    [JsonProperty(PropertyName = "Share (true/false)")]
    [DefaultValue(false)]
    public bool Share { get; set; }
    [JsonProperty(PropertyName = "Tree (true/false)")]
    [DefaultValue(false)]
    public bool Tree { get; set; }
}

public class PasteOptions
{
    [JsonProperty(PropertyName = "Auth (true/false)")]
    [DefaultValue(false)]
    public bool Auth { get; set; }
    [JsonProperty(PropertyName = "Deployables (true/false)")]
    [DefaultValue(true)]
    public bool Deployables { get; set; }
    [JsonProperty(PropertyName = "Inventories (true/false)")]
    [DefaultValue(true)]
    public bool Inventories { get; set; }
    [JsonProperty(PropertyName = "Vending Machines (true/false)")]
    [DefaultValue(true)]
    public bool VendingMachines { get; set; }
    [JsonProperty(PropertyName = "Stability (true/false)")]
    [DefaultValue(true)]
    public bool Stability { get; set; }
}


```

---

## Cornucopia

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using Random = UnityEngine.Random;
using Oxide.Core.Plugins;
using Oxide.Core;

Oxide.Plugins
[Info("Cornucopia", "Deicide666ra", "1.1.4", ResourceId = 1264)]
 class Cornucopia : RustPlugin
{
     class CornuConfig
    {
        public CornuConfig();
        public int RefreshMinutes;
        public bool ApplyLootFix;
        public bool RefreshOnStart;
        public List<CornuConfigItem> Animals;
        public List<CornuConfigItem> Ores;
        public List<CornuConfigItem> Loots;
    }

     class CornuConfigItem
    {
        public string Prefab;
        public int Min;
        public int Max;
        public bool IgnoreIrridiated;
        public bool DeleteEmtpy;
    }

    private CornuConfig g_config;
    private Timer g_refreshTimer;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
     void OnServerInitialized();
     void OnTimer();
     void Unloaded();
     Dictionary<string, int> GetCollectibles();
     Dictionary<string, IGrouping<string, BaseEntity>> GetOreNodes();
     Dictionary<string, IGrouping<string, BaseEntity>> GetLootContainers();
     Dictionary<string, IGrouping<string, BaseEntity>> GetAnimals();
     void DumpSpawns(Dictionary<string, int> entities);
     void DumpSpawns(Dictionary<string, IGrouping<string, BaseEntity>> entities);
    [ConsoleCommand("cornu.dump")]
    private void DumpCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("cornu.spawn")]
    private void SpawnCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("cornu.fixloot")]
    private void FixLootCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("cornu.purge")]
    private void PurgeCommand(ConsoleSystem.Arg arg);
     void DumpEntities();
     Vector2 GetBoxPos(LootContainer box);
    [ChatCommand("cpurge")]
     void cmdPurge(BasePlayer player, string cmd, string[] args);
    [ChatCommand("cfixloot")]
     void cmdFixLoot(BasePlayer player, string cmd, string[] args);
    [ChatCommand("cdump")]
     void cmdDump(BasePlayer player, string cmd, string[] args);
    [ChatCommand("cspawn")]
     void cmdSpawn(BasePlayer player, string cmd, string[] args);
    [ChatCommand("ctest")]
     void cmdTest(BasePlayer player, string cmd, string[] args);
     void SubCycle(IEnumerable<BaseEntity> entities, IEnumerable<CornuConfigItem> limits, List<CollectibleEntity> collectibles, bool aborted);
     void MainSpawnCycle();
     void PopulationControl(IEnumerable<BaseEntity> matches, int cap);
     void BatchSpawn(int current, int wanted, string prefab, List<CollectibleEntity> collectibles, bool aborted);
    private void ReplaceCollectibleWithSomething(string prefabName, List<CollectibleEntity> collectibles);
     void FixLoot(BasePlayer player);
    private void Purge();
}

 class CornuConfig
{
    public CornuConfig();
    public int RefreshMinutes;
    public bool ApplyLootFix;
    public bool RefreshOnStart;
    public List<CornuConfigItem> Animals;
    public List<CornuConfigItem> Ores;
    public List<CornuConfigItem> Loots;
}

 class CornuConfigItem
{
    public string Prefab;
    public int Min;
    public int Max;
    public bool IgnoreIrridiated;
    public bool DeleteEmtpy;
}


```

---

## Corpsedel

```csharp
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Corpsedel", "Toshik", "1.0.1")]
public class Corpsedel : RustPlugin
{
    private void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## Corpses

```csharp
using UnityEngine;

Oxide.Plugins
[Info("Corpses", "Steven", "1.0.1", ResourceId = 8913)]
 class Corpses : RustPlugin
{
    [ChatCommand("deadclean")]
     void DeadCleanCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("deadcount")]
     void DeadCountCmd(BasePlayer player, string command, string[] args);
}


```

---

## CoverShop

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("CoverShop", "Fipp", "0.0.1")]
public class CoverShop : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private ConfigData _config;
     class ItemsCover
    {
        [JsonProperty("Shortname предмета")]
        public string ShortName;
        [JsonProperty("Цена(В Изоленте)")]
        public int AmountForBuy;
        [JsonProperty("Кол-во")]
        public int Amount;
    }

     class ConfigData
    {
        [JsonProperty("Шанс выпадение Изоленты из бочек и ящиков(0-100%")]
        public int Chance;
        [JsonProperty("Сколько минимум будет падать Изоленты")]
        public int AmountMin;
        [JsonProperty("Сколько максимум будет падать Изоленты")]
        public int AmountMax;
        [JsonProperty("Товары в магазине")]
        public List<ItemsCover> ListItems { get; set; }
        public static ConfigData GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
    public string Layer;
    [ChatCommand("shop")]
     void Shoping(BasePlayer player, string command, string[] args);
     void ResourceFind(BasePlayer player, int page);
    [ConsoleCommand("CommandCover")]
     void CoverConsole(ConsoleSystem.Arg args);
    private List<StorageContainer> handledContainers;
     object CanLootEntity(BasePlayer player, StorageContainer container);
    private Item OnItemSplit(Item item, int amount);
    private static string HexToCuiColor(string hex);
    [ConsoleCommand("givecover")]
     void GiveCommands(ConsoleSystem.Arg args);
}

 class ItemsCover
{
    [JsonProperty("Shortname предмета")]
    public string ShortName;
    [JsonProperty("Цена(В Изоленте)")]
    public int AmountForBuy;
    [JsonProperty("Кол-во")]
    public int Amount;
}

 class ConfigData
{
    [JsonProperty("Шанс выпадение Изоленты из бочек и ящиков(0-100%")]
    public int Chance;
    [JsonProperty("Сколько минимум будет падать Изоленты")]
    public int AmountMin;
    [JsonProperty("Сколько максимум будет падать Изоленты")]
    public int AmountMax;
    [JsonProperty("Товары в магазине")]
    public List<ItemsCover> ListItems { get; set; }
    public static ConfigData GetNewCong();
}


```

---

## CovertAdmin

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = UnityEngine.Random;
using System.Text;

Oxide.Plugins
[Info("CovertAdmin", "redBDGR", "1.0.9")]
[Description("Go fully undercover and disguise yourself as another player")]
internal class CovertAdmin : RustPlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private bool Changed;
    private Dictionary<string, __CovertInfo> covertPlayerList;
    private class __CovertInfo
    {
        public ulong __covertID;
        public string __covertName;
        public BasePlayer __player;
        public string __restoreName;
        public ulong __restoreID;
    }

    private string covertNameColor;
    private const string __permissionName;
    private Dictionary<string, object> covertNameList;
    private void Init();
    private string covertTags;
    private string covertTextSize;
    private bool disableOnLogoff;
    private bool disableAdminAbilities;
    private static Dictionary<string, object> __RandomPlayerNames();
    protected override void LoadDefaultConfig();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void LoadVariables();
    protected override void LoadDefaultMessages();
    [ChatCommand("covertstatus")]
    private void CovertStatusCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("covert")]
    private void CovertCMD(BasePlayer player, string command, string[] args);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private void OnPlayerRespawned(BasePlayer player);
    private void __RenamePlayer(BasePlayer player, string __name, bool __startingCovert);
    private object OnBetterChat(Dictionary<string, object> __data);
    private object OnPlayerChat(ConsoleSystem.Arg arg);
    private object OnUserChat(IPlayer __player, string __message);
    private void Unload();
    private string msg(string key, string id);
}

private class __CovertInfo
{
    public ulong __covertID;
    public string __covertName;
    public BasePlayer __player;
    public string __restoreName;
    public ulong __restoreID;
}


```

---

## CovertAdmin (1)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = UnityEngine.Random;
using System.Text;

Oxide.Plugins
[Info("CovertAdmin", "redBDGR", "1.0.6")]
[Description("Go fully undercover and disguise yourself as another player")]
internal class CovertAdmin : RustPlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private bool Changed;
    private Dictionary<string, CovertInfo> covertDic;
    private string nameColour;
    private const string permissionName;
    private Dictionary<string, object> playerNames;
    private string tags;
    private string textSize;
    private bool disableOnLogout;
    private static Dictionary<string, object> RandomPlayerNames();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void Init();
    private void Unload();
    [ChatCommand("covertstatus")]
    private void CovertStatusCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("covert")]
    private void CovertCMD(BasePlayer player, string command, string[] args);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerInit(BasePlayer player);
    private object OnPlayerDie(BasePlayer player, HitInfo info);
    private void OnPlayerRespawned(BasePlayer player);
    private void RenamePlayer(BasePlayer player, string name);
    private object OnBetterChat(Dictionary<string, object> data);
    private object OnPlayerChat(ConsoleSystem.Arg arg);
    private object OnUserChat(IPlayer player, string message);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
    private class CovertInfo
    {
        public ulong covertID;
        public string covertName;
        public BasePlayer player;
        public string restoreName;
        public ulong restoreID;
    }

}

private class CovertInfo
{
    public ulong covertID;
    public string covertName;
    public BasePlayer player;
    public string restoreName;
    public ulong restoreID;
}


```

---

## Crafter

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Rust;
using System;
using UnityEngine;

Oxide.Plugins
[Info("Crafter", "Night_Tiger", "0.1.2")]
[Description("Позволяет крафтить оружия: lr300, m249, m92, spas12")]
 class Crafter : RustPlugin
{
    static Crafter ins;
    private bool initialized;
    private void Loaded();
    private void OnServerInitialized();
    private bool HasPermission(BasePlayer player, string perm);
    [ChatCommand("craft")]
     void cmdCraftUser(BasePlayer player, string command, string[] args);
    [ChatCommand("admcraft")]
     void cmdCraftAdmin(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Компоненты необходимые для крафта")]
        public CrafterComponents CC { get; set; }
        public class CrafterComponents
        {
            [JsonProperty(PropertyName = "Для винтовки LR-300")]
            public CClr300 lr300 { get; set; }
            [JsonProperty(PropertyName = "Для пулемета M249")]
            public CCm249 m249 { get; set; }
            [JsonProperty(PropertyName = "Для пистолета M92")]
            public CCm92 m92 { get; set; }
            [JsonProperty(PropertyName = "Для дробовика Spas12")]
            public CCspas spas { get; set; }
            public class CClr300
            {
                [JsonProperty(PropertyName = "Металл высокого качества")]
                public int comp1 { get; set; }
                [JsonProperty(PropertyName = "Фрагменты металла")]
                public int comp2 { get; set; }
                [JsonProperty(PropertyName = "Корпус винтовки")]
                public int comp3 { get; set; }
                [JsonProperty(PropertyName = "Пружины")]
                public int comp4 { get; set; }
            }

            public class CCm249
            {
                [JsonProperty(PropertyName = "Металл высокого качества")]
                public int comp1 { get; set; }
                [JsonProperty(PropertyName = "Фрагменты металла")]
                public int comp2 { get; set; }
                [JsonProperty(PropertyName = "Корпус винтовки")]
                public int comp3 { get; set; }
                [JsonProperty(PropertyName = "Пружины")]
                public int comp4 { get; set; }
            }

            public class CCm92
            {
                [JsonProperty(PropertyName = "Металл высокого качества")]
                public int comp1 { get; set; }
                [JsonProperty(PropertyName = "Трубы")]
                public int comp2 { get; set; }
                [JsonProperty(PropertyName = "Корпус полуавтомата")]
                public int comp3 { get; set; }
                [JsonProperty(PropertyName = "Пружины")]
                public int comp4 { get; set; }
            }

            public class CCspas
            {
                [JsonProperty(PropertyName = "Металл высокого качества")]
                public int comp1 { get; set; }
                [JsonProperty(PropertyName = "Трубы")]
                public int comp2 { get; set; }
                [JsonProperty(PropertyName = "Фрагменты металла")]
                public int comp3 { get; set; }
                [JsonProperty(PropertyName = "Пружины")]
                public int comp4 { get; set; }
            }

        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private ConfigData GetBaseConfig();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Компоненты необходимые для крафта")]
    public CrafterComponents CC { get; set; }
    public class CrafterComponents
    {
        [JsonProperty(PropertyName = "Для винтовки LR-300")]
        public CClr300 lr300 { get; set; }
        [JsonProperty(PropertyName = "Для пулемета M249")]
        public CCm249 m249 { get; set; }
        [JsonProperty(PropertyName = "Для пистолета M92")]
        public CCm92 m92 { get; set; }
        [JsonProperty(PropertyName = "Для дробовика Spas12")]
        public CCspas spas { get; set; }
        public class CClr300
        {
            [JsonProperty(PropertyName = "Металл высокого качества")]
            public int comp1 { get; set; }
            [JsonProperty(PropertyName = "Фрагменты металла")]
            public int comp2 { get; set; }
            [JsonProperty(PropertyName = "Корпус винтовки")]
            public int comp3 { get; set; }
            [JsonProperty(PropertyName = "Пружины")]
            public int comp4 { get; set; }
        }

        public class CCm249
        {
            [JsonProperty(PropertyName = "Металл высокого качества")]
            public int comp1 { get; set; }
            [JsonProperty(PropertyName = "Фрагменты металла")]
            public int comp2 { get; set; }
            [JsonProperty(PropertyName = "Корпус винтовки")]
            public int comp3 { get; set; }
            [JsonProperty(PropertyName = "Пружины")]
            public int comp4 { get; set; }
        }

        public class CCm92
        {
            [JsonProperty(PropertyName = "Металл высокого качества")]
            public int comp1 { get; set; }
            [JsonProperty(PropertyName = "Трубы")]
            public int comp2 { get; set; }
            [JsonProperty(PropertyName = "Корпус полуавтомата")]
            public int comp3 { get; set; }
            [JsonProperty(PropertyName = "Пружины")]
            public int comp4 { get; set; }
        }

        public class CCspas
        {
            [JsonProperty(PropertyName = "Металл высокого качества")]
            public int comp1 { get; set; }
            [JsonProperty(PropertyName = "Трубы")]
            public int comp2 { get; set; }
            [JsonProperty(PropertyName = "Фрагменты металла")]
            public int comp3 { get; set; }
            [JsonProperty(PropertyName = "Пружины")]
            public int comp4 { get; set; }
        }

    }

}

public class CrafterComponents
{
    [JsonProperty(PropertyName = "Для винтовки LR-300")]
    public CClr300 lr300 { get; set; }
    [JsonProperty(PropertyName = "Для пулемета M249")]
    public CCm249 m249 { get; set; }
    [JsonProperty(PropertyName = "Для пистолета M92")]
    public CCm92 m92 { get; set; }
    [JsonProperty(PropertyName = "Для дробовика Spas12")]
    public CCspas spas { get; set; }
    public class CClr300
    {
        [JsonProperty(PropertyName = "Металл высокого качества")]
        public int comp1 { get; set; }
        [JsonProperty(PropertyName = "Фрагменты металла")]
        public int comp2 { get; set; }
        [JsonProperty(PropertyName = "Корпус винтовки")]
        public int comp3 { get; set; }
        [JsonProperty(PropertyName = "Пружины")]
        public int comp4 { get; set; }
    }

    public class CCm249
    {
        [JsonProperty(PropertyName = "Металл высокого качества")]
        public int comp1 { get; set; }
        [JsonProperty(PropertyName = "Фрагменты металла")]
        public int comp2 { get; set; }
        [JsonProperty(PropertyName = "Корпус винтовки")]
        public int comp3 { get; set; }
        [JsonProperty(PropertyName = "Пружины")]
        public int comp4 { get; set; }
    }

    public class CCm92
    {
        [JsonProperty(PropertyName = "Металл высокого качества")]
        public int comp1 { get; set; }
        [JsonProperty(PropertyName = "Трубы")]
        public int comp2 { get; set; }
        [JsonProperty(PropertyName = "Корпус полуавтомата")]
        public int comp3 { get; set; }
        [JsonProperty(PropertyName = "Пружины")]
        public int comp4 { get; set; }
    }

    public class CCspas
    {
        [JsonProperty(PropertyName = "Металл высокого качества")]
        public int comp1 { get; set; }
        [JsonProperty(PropertyName = "Трубы")]
        public int comp2 { get; set; }
        [JsonProperty(PropertyName = "Фрагменты металла")]
        public int comp3 { get; set; }
        [JsonProperty(PropertyName = "Пружины")]
        public int comp4 { get; set; }
    }

}

public class CClr300
{
    [JsonProperty(PropertyName = "Металл высокого качества")]
    public int comp1 { get; set; }
    [JsonProperty(PropertyName = "Фрагменты металла")]
    public int comp2 { get; set; }
    [JsonProperty(PropertyName = "Корпус винтовки")]
    public int comp3 { get; set; }
    [JsonProperty(PropertyName = "Пружины")]
    public int comp4 { get; set; }
}

public class CCm249
{
    [JsonProperty(PropertyName = "Металл высокого качества")]
    public int comp1 { get; set; }
    [JsonProperty(PropertyName = "Фрагменты металла")]
    public int comp2 { get; set; }
    [JsonProperty(PropertyName = "Корпус винтовки")]
    public int comp3 { get; set; }
    [JsonProperty(PropertyName = "Пружины")]
    public int comp4 { get; set; }
}

public class CCm92
{
    [JsonProperty(PropertyName = "Металл высокого качества")]
    public int comp1 { get; set; }
    [JsonProperty(PropertyName = "Трубы")]
    public int comp2 { get; set; }
    [JsonProperty(PropertyName = "Корпус полуавтомата")]
    public int comp3 { get; set; }
    [JsonProperty(PropertyName = "Пружины")]
    public int comp4 { get; set; }
}

public class CCspas
{
    [JsonProperty(PropertyName = "Металл высокого качества")]
    public int comp1 { get; set; }
    [JsonProperty(PropertyName = "Трубы")]
    public int comp2 { get; set; }
    [JsonProperty(PropertyName = "Фрагменты металла")]
    public int comp3 { get; set; }
    [JsonProperty(PropertyName = "Пружины")]
    public int comp4 { get; set; }
}


```

---

## CraftingController

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;

Oxide.Plugins
[Info("Crafting Controller", "Whispers88", "3.2.9")]
[Description("Allows you to modify the time spend crafting and which items can be crafted")]
public class CraftingController : RustPlugin
{
    private Configuration config;
    private static Dictionary<string, float> defaultsetup;
    public class CraftingData
    {
        public bool canCraft;
        public bool canResearch;
        public bool useCrafteRateMultiplier;
        public float craftTime;
        public int workbenchLevel;
        public ulong defaultskinid;
        public CraftingData();
    }

    public class Configuration
    {
        [JsonProperty("Default crafting rate percentage")]
        public float CraftingRate;
        [JsonProperty("Save commands to config (save config changes via command to the configuration)")]
        public bool SaveCommands;
        [JsonProperty("Simple Mode (disables: instant bulk craft, skin options and full inventory checks for better performance)")]
        public bool SimpleMode;
        [JsonProperty("Allow crafting when inventory is full")]
        public bool FullInventory;
        [JsonProperty("Complete crafting on server shut down")]
        public bool CompleteCrafting;
        [JsonProperty("Craft items with random skins if not already skinned")]
        public bool RandomSkins;
        [JsonProperty("Show Crafting Notes")]
        public bool ShowCraftNotes;
        [JsonProperty("Crafting rate bonus mulitplier (apply oxide perms for additional mulitpliers")]
        public Dictionary<string, float> BonusMultiplier;
        [JsonProperty("Advanced Crafting Options")]
        public Dictionary<string, CraftingData> CraftingOptions;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private const string perminstantbulkcraft;
    private const string permblockitems;
    private const string permitemrate;
    private const string permcraftingrate;
    private const string permsetbenchlvl;
    private const string permsetskins;
    private List<string> permissions;
    private List<string> permissionsBonusMultiplier;
    private List<string> commands;
    private void OnServerInitialized();
    private void Unload();
    protected override void LoadDefaultMessages();
    private void CommandCraftingRate(IPlayer iplayer, string command, string[] args);
    private void CommandCraftTime(IPlayer iplayer, string command, string[] args);
    private void CommandBlockItem(IPlayer iplayer, string command, string[] args);
    private void CommandUnblockItem(IPlayer iplayer, string command, string[] args);
    private void CommandWorkbenchLVL(IPlayer iplayer, string command, string[] args);
    private void CommandSetDefaultSkin(IPlayer iplayer, string command, string[] args);
    private void UpdateCraftingRate();
    private void InstantBulkCraft(BasePlayer player, ItemCraftTask task, ItemDefinition item, List<int> stacks, int craftSkin, ulong skin);
    private static void CompleteCrafting(BasePlayer player);
    private static void CancelAllCrafting(BasePlayer player);
    private Dictionary<ItemCraftTask, ulong> skinupdate;
    private object OnItemCraft(ItemCraftTask task, BasePlayer player);
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnItemCraftCancelled(ItemCraftTask task);
    private void OnServerQuit();
    private void ReturnCraft(ItemCraftTask task, BasePlayer crafter);
    private ItemDefinition FindItem(string itemNameOrId);
    private int FreeSpace(BasePlayer player, ItemDefinition item);
    private int FreeSlots(BasePlayer player);
    private int Stackroom(List<Item> items, string item);
    private List<int> GetStacks(ItemDefinition item, int amount);
    private readonly Dictionary<string, List<ulong>> skinsCache;
    private List<ulong> GetSkins(ItemDefinition def);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string langKey, object[] args);
    private bool HasPerm(string id, string perm);
    private void AddLocalizedCommand(string command);
}

public class CraftingData
{
    public bool canCraft;
    public bool canResearch;
    public bool useCrafteRateMultiplier;
    public float craftTime;
    public int workbenchLevel;
    public ulong defaultskinid;
    public CraftingData();
}

public class Configuration
{
    [JsonProperty("Default crafting rate percentage")]
    public float CraftingRate;
    [JsonProperty("Save commands to config (save config changes via command to the configuration)")]
    public bool SaveCommands;
    [JsonProperty("Simple Mode (disables: instant bulk craft, skin options and full inventory checks for better performance)")]
    public bool SimpleMode;
    [JsonProperty("Allow crafting when inventory is full")]
    public bool FullInventory;
    [JsonProperty("Complete crafting on server shut down")]
    public bool CompleteCrafting;
    [JsonProperty("Craft items with random skins if not already skinned")]
    public bool RandomSkins;
    [JsonProperty("Show Crafting Notes")]
    public bool ShowCraftNotes;
    [JsonProperty("Crafting rate bonus mulitplier (apply oxide perms for additional mulitpliers")]
    public Dictionary<string, float> BonusMultiplier;
    [JsonProperty("Advanced Crafting Options")]
    public Dictionary<string, CraftingData> CraftingOptions;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## CraftMenu

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using Facepunch;
using Network;
using VLB;

Oxide.Plugins
[Info("CraftMenu", "David", "1.0.91")]
[Description("Simple craft menu for uncraftable items.")]
public class CraftMenu : RustPlugin
{
     int _bpsTotal;
     int _bpsDefault;
     string click;
     string pageChange;
     string research;
     string craft;
     string[] ca;
     string[] ia;
    private string[] ba;
    private void OnServerInitialized();
     void Unload();
     void OnNewSave();
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void OnEntityDeath(BasePlayer player, HitInfo info);
    private void OnLootEntity(BasePlayer player, Workbench bench);
    private void OnLootEntityEnd(BasePlayer player, Workbench bench);
    private void RegisterPerms();
    [ConsoleCommand("craftmenu_admin")]
    private void craftmenu_admin(ConsoleSystem.Arg arg);
    [ConsoleCommand("craftmenu_cmd")]
    private void craftmenu_cmd(ConsoleSystem.Arg arg);
    private List<string> CreateBpOrder(BasePlayer player, int tier, string category);
    private bool CheckInv(BasePlayer player, string shortName, int amount);
    private bool _CanCraft(BasePlayer player, string shortName);
    private void CraftItem(BasePlayer player, int tier, string shortName, int index);
    private void ResearchItem(BasePlayer player, int tier, string shortName, int index);
    private void RefundItem(BasePlayer player, string shortName);
    private void CreateItemMono(BasePlayer player, string shortName);
    [ConsoleCommand("craftmenu_cancel")]
    private void craftmenu_cancel(ConsoleSystem.Arg arg);
    [ConsoleCommand("craftmenu_addtoque")]
    private void craftmenu_addtoque(ConsoleSystem.Arg arg);
    [ConsoleCommand("craftmenu_openquepanel")]
    private void craftmenu_openquepanel(ConsoleSystem.Arg arg);
    private void PlayFx(BasePlayer player, string fx);
    static CraftMenu plugin;
    private void Init();
    private void AddMonoComponent();
    private void DestroyMonoComponent();
    private class CraftingQue : MonoBehaviour
    {
         BasePlayer player;
         List<string> craftOrder;
         bool craftPanelOpen;
         int progress;
         void Awake();
        public void CancelAll(BasePlayer player);
        public void OpenQuePanel(BasePlayer player);
        public void CancelCraft(BasePlayer player, int index);
         void CraftProgress();
        public void AddToQue(BasePlayer player, string itemName);
    }

    private void CreateBaseCui(BasePlayer player, int tier);
    private void DestroyCui(BasePlayer player);
    private void CreatePageBtns(BasePlayer player, string category, int tier, int currentPage);
    private void ShowBps(BasePlayer player, int tier, string category, int page);
    private void CreateBp(BasePlayer player, int tier, string shortName, int index, bool selected);
    private void DestroyBps(BasePlayer player);
     string[] anchors;
    private void CreateCraftQueLayout(BasePlayer player, string itemName);
    private void CreateTimer(BasePlayer player, string itemName, int seconds);
    private void CreateQueButton(BasePlayer player, int count);
    private void CreateQuePanel(BasePlayer player, List<string> craftingQue);
    public class CUIClass
    {
        public static CuiElementContainer CreateOverlay(string _name, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fade, string _mat);
        public static void CreatePanel(CuiElementContainer _container, string _name, string _parent, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fadeIn, float _fadeOut, string _mat2, string _OffsetMin, string _OffsetMax);
        public static void CreateImage(CuiElementContainer _container, string _name, string _parent, string _image, string _anchorMin, string _anchorMax, float _fadeIn, float _fadeOut, string _OffsetMin, string _OffsetMax);
        public static void PullFromAssets(CuiElementContainer _container, string _name, string _parent, string _color, string _sprite, float _fadeIn, float _fadeOut, string _anchorMin, string _anchorMax, string _material);
        public static void CreateInput(CuiElementContainer _container, string _name, string _parent, string _color, int _size, string _anchorMin, string _anchorMax, string _font, string _command, TextAnchor _align);
        public static void CreateText(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, TextAnchor _align, string _font, float _fadeIn, float _fadeOut, string _outlineColor, string _outlineScale);
        public static void CreateButton(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, string _command, string _close, string _textColor, float _fade, TextAnchor _align, string _font, string _material);
    }

    private void SaveData();
    private Dictionary<string, Bps> bps;
    private class Bps
    {
        public string Name;
        public string Image;
        public ulong SkinID;
        public string Category;
        public int Tier;
        public int ResearchCost;
        public Dictionary<string, int> Resources;
    }

    private void LoadData();
    private void CreateExamples();
    private void SavePlayerData();
    private Dictionary<ulong, PlayerBps> playerBps;
    private class PlayerBps
    {
        public List<string> bp;
    }

    private void LoadPlayerData();
    private void CreatePlayerExamples();
    private void SaveNamesData();
    private Dictionary<string, string> nameReplace;
    private void LoadNamesData();
    private void CreateNameReplacements();
    [PluginReference]
     Plugin ImageLibrary;
    private List<string> imgList;
    private void DownloadImages();
    private void ImageQueCheck();
    private string Img(string url);
    private Configuration config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     class Configuration
    {
        [JsonProperty(PropertyName = "Main Settings")]
        public MainSet main { get; set; }
        public class MainSet
        {
            [JsonProperty("Wipe Blueprints at Map Wipe")]
            public bool wipe { get; set; }
            [JsonProperty("Sound Effects")]
            public bool fx { get; set; }
            [JsonProperty("Categories (Max 6)")]
            public Dictionary<string, string> cat { get; set; }
            [JsonProperty("Permissions required for each category")]
            public bool perms { get; set; }
        }

        [JsonProperty(PropertyName = "Crafting Time")]
        public CT ct { get; set; }
        public class CT
        {
            [JsonProperty("Enabled")]
            public bool craftQue { get; set; }
            [JsonProperty("Default Craft Time for all items (in seconds)")]
            public int defaultTime { get; set; }
            [JsonProperty("Specific Craft Time (in seconds)")]
            public Dictionary<string, int> excp { get; set; }
        }

        public static Configuration CreateConfig();
    }

}

private class CraftingQue : MonoBehaviour
{
     BasePlayer player;
     List<string> craftOrder;
     bool craftPanelOpen;
     int progress;
     void Awake();
    public void CancelAll(BasePlayer player);
    public void OpenQuePanel(BasePlayer player);
    public void CancelCraft(BasePlayer player, int index);
     void CraftProgress();
    public void AddToQue(BasePlayer player, string itemName);
}

public class CUIClass
{
    public static CuiElementContainer CreateOverlay(string _name, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fade, string _mat);
    public static void CreatePanel(CuiElementContainer _container, string _name, string _parent, string _color, string _anchorMin, string _anchorMax, bool _cursorOn, float _fadeIn, float _fadeOut, string _mat2, string _OffsetMin, string _OffsetMax);
    public static void CreateImage(CuiElementContainer _container, string _name, string _parent, string _image, string _anchorMin, string _anchorMax, float _fadeIn, float _fadeOut, string _OffsetMin, string _OffsetMax);
    public static void PullFromAssets(CuiElementContainer _container, string _name, string _parent, string _color, string _sprite, float _fadeIn, float _fadeOut, string _anchorMin, string _anchorMax, string _material);
    public static void CreateInput(CuiElementContainer _container, string _name, string _parent, string _color, int _size, string _anchorMin, string _anchorMax, string _font, string _command, TextAnchor _align);
    public static void CreateText(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, TextAnchor _align, string _font, float _fadeIn, float _fadeOut, string _outlineColor, string _outlineScale);
    public static void CreateButton(CuiElementContainer _container, string _name, string _parent, string _color, string _text, int _size, string _anchorMin, string _anchorMax, string _command, string _close, string _textColor, float _fade, TextAnchor _align, string _font, string _material);
}

private class Bps
{
    public string Name;
    public string Image;
    public ulong SkinID;
    public string Category;
    public int Tier;
    public int ResearchCost;
    public Dictionary<string, int> Resources;
}

private class PlayerBps
{
    public List<string> bp;
}

 class Configuration
{
    [JsonProperty(PropertyName = "Main Settings")]
    public MainSet main { get; set; }
    public class MainSet
    {
        [JsonProperty("Wipe Blueprints at Map Wipe")]
        public bool wipe { get; set; }
        [JsonProperty("Sound Effects")]
        public bool fx { get; set; }
        [JsonProperty("Categories (Max 6)")]
        public Dictionary<string, string> cat { get; set; }
        [JsonProperty("Permissions required for each category")]
        public bool perms { get; set; }
    }

    [JsonProperty(PropertyName = "Crafting Time")]
    public CT ct { get; set; }
    public class CT
    {
        [JsonProperty("Enabled")]
        public bool craftQue { get; set; }
        [JsonProperty("Default Craft Time for all items (in seconds)")]
        public int defaultTime { get; set; }
        [JsonProperty("Specific Craft Time (in seconds)")]
        public Dictionary<string, int> excp { get; set; }
    }

    public static Configuration CreateConfig();
}

public class MainSet
{
    [JsonProperty("Wipe Blueprints at Map Wipe")]
    public bool wipe { get; set; }
    [JsonProperty("Sound Effects")]
    public bool fx { get; set; }
    [JsonProperty("Categories (Max 6)")]
    public Dictionary<string, string> cat { get; set; }
    [JsonProperty("Permissions required for each category")]
    public bool perms { get; set; }
}

public class CT
{
    [JsonProperty("Enabled")]
    public bool craftQue { get; set; }
    [JsonProperty("Default Craft Time for all items (in seconds)")]
    public int defaultTime { get; set; }
    [JsonProperty("Specific Craft Time (in seconds)")]
    public Dictionary<string, int> excp { get; set; }
}


```

---

## CraftPanel

```csharp
using System;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("CraftPanel", "TopPlugin.ru", "1.3.1")]
[Description("Simple Custom Crafting Interface")]
public class CraftPanel : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private const string elemq0;
    private const string elemq1;
    private const string elemq2;
    private const string elemq7;
    private const string elemq6;
    private const string elemqitemvip;
    private bool _isCraftReady;
    private List<CraftInfo> craftList;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    [ChatCommand("craft")]
     void chatopenvip(BasePlayer player);
    private void cmdCloseConsole(ConsoleSystem.Arg arg);
    private void cmdClose2Console(ConsoleSystem.Arg arg);
    private void cmdInfoVipConsole(ConsoleSystem.Arg arg);
    private void cmdInfoVip(BasePlayer player, string command, string[] args);
    private void cmdCraft(ConsoleSystem.Arg arg);
    private void cmdCrafting(BasePlayer player, string command, string[] args);
    private void cmdItemsPage(ConsoleSystem.Arg arg);
    private void Permissions();
    private bool HasPermission(string userID, string perm);
    private void LoadImages();
    private void CraftReady();
    private string GetImageLibrary(string name, ulong skinid);
     void Efecto(BasePlayer player, String prefab);
    private List<CraftInfo> GetCraftItemsForPlayer(BasePlayer player);
    private void OpenPanelCraft(BasePlayer player);
    private void OpenPanelItem(BasePlayer player, string item, int page);
    private void OpenPanelError(BasePlayer player, string textshow);
    private void ClosePanel(BasePlayer player, bool sonido);
    private void ClosePanelNoSound(BasePlayer player, bool sonido);
    private void GetPanelItem(BasePlayer player, string i, int page);
    private CuiElementContainer GetPanel2(BasePlayer player, string textshow);
    public class UI
    {
        static public void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor, string material);
        static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, string color, TextAnchor align, bool font);
        static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
        static public void Image(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    }

    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Custom")]
        public CraftCustom Custom;
        [JsonProperty(PropertyName = "Craft")]
        public List<CraftInfo> Craft;
    }

    private class CraftInfo
    {
        [JsonProperty(PropertyName = "Short Name")]
        public string name;
        [JsonProperty(PropertyName = "Full Name")]
        public string namecomplete;
        [JsonProperty(PropertyName = "Img Full")]
        public string img;
        [JsonProperty(PropertyName = "Img Icon")]
        public string imgicon;
        [JsonProperty(PropertyName = "Description")]
        public string description;
        [JsonProperty(PropertyName = "Craft Result")]
        public CraftResult result;
        [JsonProperty(PropertyName = "Permission Use")]
        public string permission;
        [JsonProperty(PropertyName = "Permission VIP")]
        public string permissionVIP;
        [JsonProperty(PropertyName = "Permission No Cost")]
        public string permissionNoCost;
        [JsonProperty(PropertyName = "VIP discount: 10 = 10%")]
        public int VIPdiscount;
        [JsonProperty(PropertyName = "Require Workbench? 0 = NOT, 1 = Level 1,...")]
        public int workbench;
        [JsonProperty(PropertyName = "Items")]
        public List<CraftInfoItem> items;
    }

    private class CraftInfoItem
    {
        [JsonProperty(PropertyName = "Item")]
        public string item;
        [JsonProperty(PropertyName = "Amount")]
        public int amount;
        [JsonProperty(PropertyName = "Skin ID")]
        public ulong skinID;
    }

    private class CraftResult
    {
        [JsonProperty(PropertyName = "Command (keep empty to create item)")]
        public string command;
        [JsonProperty(PropertyName = "Shortname")]
        public string shortname;
        [JsonProperty(PropertyName = "Amount")]
        public int amount;
        [JsonProperty(PropertyName = "Skin ID")]
        public ulong skinID;
    }

    private class CraftCustom
    {
        [JsonProperty(PropertyName = "Title")]
        public string title;
        [JsonProperty(PropertyName = "Show even if you don't have permissions (you won't be able to craft)")]
        public bool showWithoutPerm;
        [JsonProperty(PropertyName = "Sound Effects")]
        public bool soundeffects;
        [JsonProperty(PropertyName = "Sound Prefab 1")]
        public string sound1;
        [JsonProperty(PropertyName = "Sound Prefab 2")]
        public string sound2;
        [JsonProperty(PropertyName = "Sound Prefab 3")]
        public string sound3;
        [JsonProperty(PropertyName = "Permission Use /craft")]
        public string permissionuse;
        [JsonProperty(PropertyName = "Color Title")]
        public string colortitle;
        [JsonProperty(PropertyName = "Color Button List")]
        public string colorbtnlist;
        [JsonProperty(PropertyName = "Color Button Close")]
        public string colorbtnclose;
        [JsonProperty(PropertyName = "Color Button Craft")]
        public string colorbtncraft;
        [JsonProperty(PropertyName = "Color Button Back")]
        public string colorbtnback;
        [JsonProperty(PropertyName = "Color Button Next")]
        public string colorbtnnext;
        [JsonProperty(PropertyName = "Color Background Panel")]
        public string colorbgpanel;
        [JsonProperty(PropertyName = "Color Text VIP")]
        public string colortxtvip;
        [JsonProperty(PropertyName = "Color Text Amount")]
        public string coloramount;
        [JsonProperty(PropertyName = "Color Text Amount VIP")]
        public string coloramountvip;
        [JsonProperty(PropertyName = "Img Block Item")]
        public string imgblock;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class IngredientData
    {
        public int amount;
        public List<Item> items;
        public ulong skin;
    }

    private bool CanCraftItem(BasePlayer player, int craftitem);
    private Dictionary<string, IngredientData> GetExistingIngredients(Item[] items);
    private bool HasIngredients(Dictionary<string, IngredientData> existing, List<CraftInfoItem> cost, bool vip, int discount);
    private void TakeIngredients(Dictionary<string, IngredientData> existing, List<CraftInfoItem> cost, bool vip, int discount);
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void PrintToChat(BasePlayer player, string message);
}

public class UI
{
    static public void Panel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor, string material);
    static public void Label(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, string color, TextAnchor align, bool font);
    static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void Input(CuiElementContainer container, string panel, string color, string text, int size, string command, string aMin, string aMax);
    static public void Image(CuiElementContainer container, string panel, string png, string aMin, string aMax);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Custom")]
    public CraftCustom Custom;
    [JsonProperty(PropertyName = "Craft")]
    public List<CraftInfo> Craft;
}

private class CraftInfo
{
    [JsonProperty(PropertyName = "Short Name")]
    public string name;
    [JsonProperty(PropertyName = "Full Name")]
    public string namecomplete;
    [JsonProperty(PropertyName = "Img Full")]
    public string img;
    [JsonProperty(PropertyName = "Img Icon")]
    public string imgicon;
    [JsonProperty(PropertyName = "Description")]
    public string description;
    [JsonProperty(PropertyName = "Craft Result")]
    public CraftResult result;
    [JsonProperty(PropertyName = "Permission Use")]
    public string permission;
    [JsonProperty(PropertyName = "Permission VIP")]
    public string permissionVIP;
    [JsonProperty(PropertyName = "Permission No Cost")]
    public string permissionNoCost;
    [JsonProperty(PropertyName = "VIP discount: 10 = 10%")]
    public int VIPdiscount;
    [JsonProperty(PropertyName = "Require Workbench? 0 = NOT, 1 = Level 1,...")]
    public int workbench;
    [JsonProperty(PropertyName = "Items")]
    public List<CraftInfoItem> items;
}

private class CraftInfoItem
{
    [JsonProperty(PropertyName = "Item")]
    public string item;
    [JsonProperty(PropertyName = "Amount")]
    public int amount;
    [JsonProperty(PropertyName = "Skin ID")]
    public ulong skinID;
}

private class CraftResult
{
    [JsonProperty(PropertyName = "Command (keep empty to create item)")]
    public string command;
    [JsonProperty(PropertyName = "Shortname")]
    public string shortname;
    [JsonProperty(PropertyName = "Amount")]
    public int amount;
    [JsonProperty(PropertyName = "Skin ID")]
    public ulong skinID;
}

private class CraftCustom
{
    [JsonProperty(PropertyName = "Title")]
    public string title;
    [JsonProperty(PropertyName = "Show even if you don't have permissions (you won't be able to craft)")]
    public bool showWithoutPerm;
    [JsonProperty(PropertyName = "Sound Effects")]
    public bool soundeffects;
    [JsonProperty(PropertyName = "Sound Prefab 1")]
    public string sound1;
    [JsonProperty(PropertyName = "Sound Prefab 2")]
    public string sound2;
    [JsonProperty(PropertyName = "Sound Prefab 3")]
    public string sound3;
    [JsonProperty(PropertyName = "Permission Use /craft")]
    public string permissionuse;
    [JsonProperty(PropertyName = "Color Title")]
    public string colortitle;
    [JsonProperty(PropertyName = "Color Button List")]
    public string colorbtnlist;
    [JsonProperty(PropertyName = "Color Button Close")]
    public string colorbtnclose;
    [JsonProperty(PropertyName = "Color Button Craft")]
    public string colorbtncraft;
    [JsonProperty(PropertyName = "Color Button Back")]
    public string colorbtnback;
    [JsonProperty(PropertyName = "Color Button Next")]
    public string colorbtnnext;
    [JsonProperty(PropertyName = "Color Background Panel")]
    public string colorbgpanel;
    [JsonProperty(PropertyName = "Color Text VIP")]
    public string colortxtvip;
    [JsonProperty(PropertyName = "Color Text Amount")]
    public string coloramount;
    [JsonProperty(PropertyName = "Color Text Amount VIP")]
    public string coloramountvip;
    [JsonProperty(PropertyName = "Img Block Item")]
    public string imgblock;
}

private class IngredientData
{
    public int amount;
    public List<Item> items;
    public ulong skin;
}


```

---

## Crafts

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using VLB;
using WebSocketSharp;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Crafts", "Mevent", "1.11.0⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠")]
public class Crafts : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private const string Layer;
    private static Crafts _instance;
    private readonly List<int> _blockedLayers;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Цвет кнопки когда все предметы присутствуют")]
        public string GreenColor;
        [JsonProperty(PropertyName = "Комманда меню крафта")]
        public string Command;
        [JsonProperty(PropertyName = "Включить дебаг?")]
        public bool useDebug;
        [JsonProperty(PropertyName = "Настройка цветов верстаков",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<WorkbenchLevel, WorkbenchConfig> Workbenchs;
        [JsonProperty(PropertyName = "Настройка крафтов", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<CraftConfig> CraftsList;
        [JsonProperty(PropertyName = "Настройка переработчика")]
        public RecyclerConfig Recycler;
        [JsonProperty(PropertyName = "Настройка машины")]
        public CarConfig Car;
    }

    private class CarConfig
    {
        [JsonProperty(PropertyName = "Активные предметы (которые в руки)")]
        public ActiveItemOptions ActiveItems;
        [JsonProperty(PropertyName = "Радиус в котором будет показан текст на машине")]
        public float Radius;
        [JsonProperty(PropertyName = "Текст на машине")]
        public string Text;
        [JsonProperty(PropertyName = "Цвет текста на машине")]
        public string Color;
        [JsonProperty(PropertyName = "Время показа текста на машине (сек)")]
        public float Delay;
    }

    public class ActiveItemOptions
    {
        [JsonProperty(PropertyName = "Запретить держать все предметы")]
        public bool Disable;
        [JsonProperty(PropertyName = "Список запрещённых к держанию предметов (shortname)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] BlackList;
    }

    private class RecyclerConfig
    {
        [JsonProperty(PropertyName = "Скорость переработки")]
        public float Speed;
        [JsonProperty(PropertyName = "Радиус в котором будет показан текст на переработчике")]
        public float Radius;
        [JsonProperty(PropertyName = "Показывать дамаг на переработчике")]
        public bool DDraw;
        [JsonProperty(PropertyName = "Текст на переработчике")]
        public string Text;
        [JsonProperty(PropertyName = "Цвет текста на переработчике")]
        public string Color;
        [JsonProperty(PropertyName = "Время показа текста на переработчике (сек)")]
        public float Delay;
        [JsonProperty(PropertyName = "Можно ли подбирать переработчик")]
        public bool Available;
        [JsonProperty(PropertyName = "Подбор только владельцем?")]
        public bool Owner;
        [JsonProperty(PropertyName = "Право на постройку для подбора")]
        public bool Building;
        [JsonProperty(PropertyName = "Настройка BaseProtection",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public float[] Amounts;
        [JsonProperty(PropertyName = "Множитель урона по переработчику")]
        public float Scale;
    }

    private class WorkbenchConfig
    {
        [JsonProperty(PropertyName = "Цвет")]
        public string Color;
        [JsonProperty(PropertyName = "Надпись")]
        public string Title;
        public WorkbenchConfig(string color, string title);
    }

    private class CraftConfig
    {
        [JsonProperty(PropertyName = "Включить крафт?")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Картинка")]
        public string ImageURL;
        [JsonProperty(PropertyName = "Описание")]
        public List<string> Description;
        [JsonProperty(PropertyName = "Команда для получения")]
        public string Command;
        [JsonProperty(PropertyName = "Право на крафт")]
        public string Permission;
        [JsonProperty(PropertyName = "Отображаемое имя заменяемого предмета")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Shortname заменяемого предмета")]
        public string ShortName;
        [JsonProperty(PropertyName = "Скин заменяемого предмета")]
        public ulong SkinID;
        [JsonProperty(PropertyName = "Тип предмета (Предмет/Команда/Транспорт)")]
        [JsonConverter(typeof(StringEnumConverter))]
        public CraftType Type;
        [JsonProperty(PropertyName = "Префаб (для транспорта)")]
        public string Prefab;
        [JsonProperty(PropertyName = "Команда при получении")]
        public string GiveCommand;
        [JsonProperty(PropertyName = "Уровень верстака")]
        public WorkbenchLevel Level;
        [JsonProperty(PropertyName = "Включить проверку на дистанцию?")]
        public bool UseDistance;
        [JsonProperty(PropertyName = "Дистанция")]
        public float Distance;
        [JsonProperty(PropertyName = "Установка на землю")]
        public bool Ground;
        [JsonProperty(PropertyName = "Установка на строения")]
        public bool Structure;
        [JsonProperty(PropertyName = "Настройка предметов для крафта",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ItemForCraft> Items;
        public Item ToItem();
        public void Give(BasePlayer player);
    }

    private class ItemForCraft
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string ShortName;
        [JsonProperty(PropertyName = "Количество")]
        public int Amount;
        [JsonProperty(PropertyName = "Скин")]
        public ulong SkinID;
        public ItemForCraft(string shortname, int amount, ulong skin);
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntityBuilt(Planner held, GameObject go);
    private object CanResearchItem(BasePlayer player, Item item);
    private void OnEntitySpawned(BaseEntity entity);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private object OnRecyclerToggle(Recycler recycler, BasePlayer player);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    [ConsoleCommand("UI_Crafts")]
    private void CmdConsoleCraft(ConsoleSystem.Arg arg);
    private void CmdGiveItem(IPlayer iPlayer, string cmd, string[] args);
    private void CmdChatOpenUI(BasePlayer player, string cmd, string[] args);
    private void DrawUI(BasePlayer player, int page, bool isFirst);
    private void GiveCraft(BasePlayer player, CraftConfig cfg);
    private void CraftItem(BasePlayer player, CraftConfig item);
    private static bool HasWorkbench(BasePlayer player, WorkbenchLevel level);
    private static bool HasAllItems(IReadOnlyList<Item> items, CraftConfig craftConfig);
    private static int ItemCount(IReadOnlyList<Item> items, string shortname, ulong skin);
    private void Take(IEnumerable<Item> itemList, string shortname, ulong skinId, int iAmount);
    private void SpawnVehicle(string prefab, ulong owner, ulong skin, Vector3 position, Quaternion rotation);
    private static string HexToCuiColor(string hex);
    private List<CraftConfig> GetPlayerCrafts(BasePlayer player, int page, int count);
    private static Color HexToUnityColor(string hex);
    private static void SetPlayerFlag(BasePlayer player, BasePlayer.PlayerFlags f, bool b);
    private class RecyclerComponent : FacepunchBehaviour
    {
        private Recycler recycler;
        private GroundWatch groundWatch;
        private DestroyOnGroundMissing groundMissing;
        [NonSerialized]
        private readonly BaseEntity[] SensesResults;
        private void Awake();
        public void DDraw();
        public void StartRecycling();
        public void StopRecycling();
        public void RecycleThink();
        public bool HasRecyclable();
        public void TryPickup(BasePlayer player);
        private void OnDestroy();
        public void Kill();
    }

    public class CarController : FacepunchBehaviour
    {
        public BasicCar entity;
        public BasePlayer player;
        public bool isDieing;
        private bool allowHeldItems;
        private string[] disallowedItems;
        [NonSerialized]
        private readonly BaseEntity[] SensesResults;
        private void Awake();
        private void Update();
        public void ManageDamage(HitInfo info);
        public void DDraw();
        private void NullifyDamage(HitInfo info);
        public void UpdateHeldItems();
        public void CheckWaterLevel();
        public void StopToDie(bool death);
        private void OnDeath();
        public void Kill();
    }

    private const string NOTRESOURCES;
    private const string PLAYERNOTFOUND;
    private const string COMMANDNOTFOUND;
    private const string GIVECRAFT;
    private const string GIVEDEBUG;
    private const string NOTWORKBENCH;
    private const string NOTCRAFTS;
    private const string CREATE;
    private const string Title;
    private const string OnGround;
    private const string BuildDistance;
    private const string OnStruct;
    private const string NotTake;
    private const string ItemNotAllowed;
    protected override void LoadDefaultMessages();
    private void Reply(BasePlayer player, string key, object[] obj);
    private void Reply(IPlayer player, string key, object[] obj);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Цвет кнопки когда все предметы присутствуют")]
    public string GreenColor;
    [JsonProperty(PropertyName = "Комманда меню крафта")]
    public string Command;
    [JsonProperty(PropertyName = "Включить дебаг?")]
    public bool useDebug;
    [JsonProperty(PropertyName = "Настройка цветов верстаков",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<WorkbenchLevel, WorkbenchConfig> Workbenchs;
    [JsonProperty(PropertyName = "Настройка крафтов", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<CraftConfig> CraftsList;
    [JsonProperty(PropertyName = "Настройка переработчика")]
    public RecyclerConfig Recycler;
    [JsonProperty(PropertyName = "Настройка машины")]
    public CarConfig Car;
}

private class CarConfig
{
    [JsonProperty(PropertyName = "Активные предметы (которые в руки)")]
    public ActiveItemOptions ActiveItems;
    [JsonProperty(PropertyName = "Радиус в котором будет показан текст на машине")]
    public float Radius;
    [JsonProperty(PropertyName = "Текст на машине")]
    public string Text;
    [JsonProperty(PropertyName = "Цвет текста на машине")]
    public string Color;
    [JsonProperty(PropertyName = "Время показа текста на машине (сек)")]
    public float Delay;
}

public class ActiveItemOptions
{
    [JsonProperty(PropertyName = "Запретить держать все предметы")]
    public bool Disable;
    [JsonProperty(PropertyName = "Список запрещённых к держанию предметов (shortname)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] BlackList;
}

private class RecyclerConfig
{
    [JsonProperty(PropertyName = "Скорость переработки")]
    public float Speed;
    [JsonProperty(PropertyName = "Радиус в котором будет показан текст на переработчике")]
    public float Radius;
    [JsonProperty(PropertyName = "Показывать дамаг на переработчике")]
    public bool DDraw;
    [JsonProperty(PropertyName = "Текст на переработчике")]
    public string Text;
    [JsonProperty(PropertyName = "Цвет текста на переработчике")]
    public string Color;
    [JsonProperty(PropertyName = "Время показа текста на переработчике (сек)")]
    public float Delay;
    [JsonProperty(PropertyName = "Можно ли подбирать переработчик")]
    public bool Available;
    [JsonProperty(PropertyName = "Подбор только владельцем?")]
    public bool Owner;
    [JsonProperty(PropertyName = "Право на постройку для подбора")]
    public bool Building;
    [JsonProperty(PropertyName = "Настройка BaseProtection",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public float[] Amounts;
    [JsonProperty(PropertyName = "Множитель урона по переработчику")]
    public float Scale;
}

private class WorkbenchConfig
{
    [JsonProperty(PropertyName = "Цвет")]
    public string Color;
    [JsonProperty(PropertyName = "Надпись")]
    public string Title;
    public WorkbenchConfig(string color, string title);
}

private class CraftConfig
{
    [JsonProperty(PropertyName = "Включить крафт?")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Картинка")]
    public string ImageURL;
    [JsonProperty(PropertyName = "Описание")]
    public List<string> Description;
    [JsonProperty(PropertyName = "Команда для получения")]
    public string Command;
    [JsonProperty(PropertyName = "Право на крафт")]
    public string Permission;
    [JsonProperty(PropertyName = "Отображаемое имя заменяемого предмета")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Shortname заменяемого предмета")]
    public string ShortName;
    [JsonProperty(PropertyName = "Скин заменяемого предмета")]
    public ulong SkinID;
    [JsonProperty(PropertyName = "Тип предмета (Предмет/Команда/Транспорт)")]
    [JsonConverter(typeof(StringEnumConverter))]
    public CraftType Type;
    [JsonProperty(PropertyName = "Префаб (для транспорта)")]
    public string Prefab;
    [JsonProperty(PropertyName = "Команда при получении")]
    public string GiveCommand;
    [JsonProperty(PropertyName = "Уровень верстака")]
    public WorkbenchLevel Level;
    [JsonProperty(PropertyName = "Включить проверку на дистанцию?")]
    public bool UseDistance;
    [JsonProperty(PropertyName = "Дистанция")]
    public float Distance;
    [JsonProperty(PropertyName = "Установка на землю")]
    public bool Ground;
    [JsonProperty(PropertyName = "Установка на строения")]
    public bool Structure;
    [JsonProperty(PropertyName = "Настройка предметов для крафта",
                ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ItemForCraft> Items;
    public Item ToItem();
    public void Give(BasePlayer player);
}

private class ItemForCraft
{
    [JsonProperty(PropertyName = "Shortname")]
    public string ShortName;
    [JsonProperty(PropertyName = "Количество")]
    public int Amount;
    [JsonProperty(PropertyName = "Скин")]
    public ulong SkinID;
    public ItemForCraft(string shortname, int amount, ulong skin);
}

private class RecyclerComponent : FacepunchBehaviour
{
    private Recycler recycler;
    private GroundWatch groundWatch;
    private DestroyOnGroundMissing groundMissing;
    [NonSerialized]
    private readonly BaseEntity[] SensesResults;
    private void Awake();
    public void DDraw();
    public void StartRecycling();
    public void StopRecycling();
    public void RecycleThink();
    public bool HasRecyclable();
    public void TryPickup(BasePlayer player);
    private void OnDestroy();
    public void Kill();
}

public class CarController : FacepunchBehaviour
{
    public BasicCar entity;
    public BasePlayer player;
    public bool isDieing;
    private bool allowHeldItems;
    private string[] disallowedItems;
    [NonSerialized]
    private readonly BaseEntity[] SensesResults;
    private void Awake();
    private void Update();
    public void ManageDamage(HitInfo info);
    public void DDraw();
    private void NullifyDamage(HitInfo info);
    public void UpdateHeldItems();
    public void CheckWaterLevel();
    public void StopToDie(bool death);
    private void OnDeath();
    public void Kill();
}


```

---

## CraftSpamBlocker

```csharp
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Craft Spam Blocker", "LeoCurtss", 0.2)]
[Description("Prevents items from being crafted if the player's inventory is full.")]
 class CraftSpamBlocker : RustPlugin
{
     void Loaded();
     void OnItemCraftFinished(ItemCraftTask task, Item item);
    private string GetMessage(string name, string sid);
}


```

---

## CraftSystem

```csharp
using System;
using System.Linq;
using System.Globalization;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("CraftSystem", "Chibubrik", "1.2.0")]
 class CraftSystem : RustPlugin
{
     string Layer;
     string LayerCraftInfo;
    [PluginReference]
     Plugin ImageLibrary;
    public class CraftSettings
    {
        [JsonProperty("Включить крафт этого предмета?")]
        public bool Enable;
        [JsonProperty("Название")]
        public string Name;
        [JsonProperty("Ссылка на префаб")]
        public string Prefab;
        [JsonProperty("Короткое название предмета")]
        public string ShortName;
        [JsonProperty("Описание предмета")]
        public string Info;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Кол-во предмета")]
        public int Amount;
        [JsonProperty("Какой верстак нужен для крафта (Если 0 то не нужен)")]
        public int LevelWorkBench;
        [JsonProperty("Ссылка на изображение")]
        public string Url;
        [JsonProperty("Список предметов для крафта")]
        public Dictionary<string, int> ItemsList;
    }

    public Configuration config;
    public class Configuration
    {
        [JsonProperty("Список предметов")]
        public List<CraftSettings> craftSettings;
        public static Configuration GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void Unload();
     class BasesEntity : MonoBehaviour
    {
        private DestroyOnGroundMissing desGround;
        private GroundWatch groundWatch;
        public ulong OwnerID;
         void Awake();
    }

     void OnEntityBuilt(Planner plan, GameObject go);
    [ChatCommand("craft")]
     void cmdCraft(BasePlayer player, string command, string[] args);
    [ConsoleCommand("craft")]
     void ConsoleCraft(ConsoleSystem.Arg args);
    [ConsoleCommand("closs")]
     void ConsoleCloss(ConsoleSystem.Arg args);
    private void CraftUI(BasePlayer player, int page);
    private void CraftInfoUI(BasePlayer player, string craft);
    private static string HexToUiColor(string hex);
}

public class CraftSettings
{
    [JsonProperty("Включить крафт этого предмета?")]
    public bool Enable;
    [JsonProperty("Название")]
    public string Name;
    [JsonProperty("Ссылка на префаб")]
    public string Prefab;
    [JsonProperty("Короткое название предмета")]
    public string ShortName;
    [JsonProperty("Описание предмета")]
    public string Info;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Кол-во предмета")]
    public int Amount;
    [JsonProperty("Какой верстак нужен для крафта (Если 0 то не нужен)")]
    public int LevelWorkBench;
    [JsonProperty("Ссылка на изображение")]
    public string Url;
    [JsonProperty("Список предметов для крафта")]
    public Dictionary<string, int> ItemsList;
}

public class Configuration
{
    [JsonProperty("Список предметов")]
    public List<CraftSettings> craftSettings;
    public static Configuration GetNewCong();
}

 class BasesEntity : MonoBehaviour
{
    private DestroyOnGroundMissing desGround;
    private GroundWatch groundWatch;
    public ulong OwnerID;
     void Awake();
}


```

---

## CupboardForFriends

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Cupboard for Friends", "LaserHydra", "1.2.4", ResourceId = 1578)]
[Description("Only allow friends of already authorized people to authorize themselves.")]
 class CupboardForFriends : RustPlugin
{
     bool debug;
     bool blockDamage;
     RustPlugin FriendsAPI;
     RustPlugin Clans;
     void Loaded();
     void OnPluginLoaded(object plugin);
     void LoadMessages();
    new void LoadConfig();
    protected override void LoadDefaultConfig();
     void OnEntityTakeDamage(BaseCombatEntity vic, HitInfo info);
     object OnCupboardClearList(BuildingPrivlidge priviledge, BasePlayer player);
     object OnCupboardAuthorize(BuildingPrivlidge priviledge, BasePlayer player);
     object TestForFriends(BuildingPrivlidge priviledge, BasePlayer player, bool isDamage);
     bool IsFriend(BasePlayer player, string friendID);
     bool IsClanMember(BasePlayer player, string targetID);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(object[] args);
     string GetMsg(string key, string userID);
     void DevMsg(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}


```

---

## CupboardLogs

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Cupboard Logging", "DylanSMR", "1.0.3", ResourceId = 1904)]
[Description("Creates a log when a player places a cupboard.")]
 class CupboardLogs : RustPlugin
{
     void Loaded();
    public Dictionary<string, string> messages;
     void WriteLogs();
     class LogData
    {
        public List<string> logs;
    }

     LogData logData;
     void OnEntitySpawned(BaseEntity entity, UnityEngine.GameObject gameObject);
}

 class LogData
{
    public List<string> logs;
}


```

---

## CupboardProtection

```csharp

Oxide.Plugins
[Info("CupboardProtection", "Wulf/lukespragg", 0.1, ResourceId = 1390)]
[Description("Makes cupboards invulnerable, unable to be destroyed.")]
 class CupboardProtection : RustPlugin
{
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}


```

---

## CupboardRadius

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("CupboardRadius", "playrust.io / dcode", "1.2.0", ResourceId = 1316)]
public class CupboardRadius : RustPlugin
{
    protected override void LoadDefaultConfig();
    private int radius;
    private bool ignoreInfluence;
    private CupboardRadiusPersistence pst;
    private bool initialized;
    [HookMethod("OnServerInitialized")]
    private void onServerInitialized();
    [HookMethod("OnEntitySpawned")]
    private void onEntitySpawned(BaseNetworkable ent);
    private bool updateTrigger(BuildPrivilegeTrigger bpt);
    private void updateInfluence(uint privlidgePrefabID);
    private class CupboardRadiusPersistence : MonoBehaviour
    {
        public bool influenceIgnored;
        public SocketMod[] influenceBackup;
    }

}

private class CupboardRadiusPersistence : MonoBehaviour
{
    public bool influenceIgnored;
    public SocketMod[] influenceBackup;
}


```

---

## CupboardRestrictions

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("CupboardRestrictions", "DylanSMR", "1.0.4", ResourceId = 2020)]
[Description("Confirms cupboards are only placed on foundations or floors.")]
public class CupboardRestrictions : RustPlugin
{
     void Loaded();
     Dictionary<string, string> messages;
     void OnEntitySpawned(BaseEntity entity, UnityEngine.GameObject gameObject);
}


```

---

## CupboardSettings

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Cupboard Settings", "Sempai#3239", "1.0.21")]
 class CupboardSettings : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public Dictionary<ulong, PlayerSetting> PlayerListed;
    public class PlayerSetting
    {
        public Dictionary<uint, CupboardSetting> CupboardsList;
    }

     void Unload();
    public class CupboardSetting
    {
        public int MaxLimit;
        public bool Announcement;
        public bool AuthOther;
        public string CupboardName;
    }

    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void SaveData();
     void LoadData();
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string shortname, string name);
    private static PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Стандартный лимит авторизаций в шкафу")]
        public int StandartAuthLimit;
        [JsonProperty(PropertyName = "Лимит авторизаций в шкафу с привилегией [Привилегия:Максимальный лимит]")]
        public Dictionary<string, int> PrivilageList;
        [JsonProperty(PropertyName = "Общая привилегия на использование UI настроек шкафа")]
        public string DefaultPermission;
        public static PluginConfig DefaultConfig();
    }

     void Loaded();
     void OnLootEntity(BasePlayer player, BaseEntity entity);
    [ConsoleCommand("CupboardSettings_main")]
     void cmdCupboardSettings(ConsoleSystem.Arg args);
    public Dictionary<string, ulong> PlayerAuth(BaseEntity BuildingID);
    public int GetPrivilage(string steamid);
     void DrawSettingMenu(BasePlayer player, uint cupboardId);
     void CuiAddButtonOther(BasePlayer player, uint netID, ulong playerid);
    [ConsoleCommand("cupboardSetting_setname")]
     void cmdSetAvatarOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("cupboardsetting_playersList")]
     void cmdShowAllPlayers(ConsoleSystem.Arg args);
    [ConsoleCommand("cupboardsetting_changeauth")]
     void cmdChangeAuthOther(ConsoleSystem.Arg args);
    [ConsoleCommand("cupboardsetting_authuser")]
     void cmdAuthNewUSer(ConsoleSystem.Arg args);
    [ConsoleCommand("cupboardsetting_changeName")]
     void cmdChangeAvatarOfClan(ConsoleSystem.Arg args);
    [ConsoleCommand("cupsetting.deauth")]
     void cmdDeAutorization(ConsoleSystem.Arg arg);
    [ConsoleCommand("cupsetting.changelimit")]
     void cmdChangeCupLimit(ConsoleSystem.Arg arg);
    [ConsoleCommand("cupsetting.enabledanno")]
     void cmdEnabledAnnouncement(ConsoleSystem.Arg arg);
     void LimitSetting(BasePlayer player, uint cup, ulong ownerID);
     class Position
    {
        public float Xmin;
        public float Xmax;
        public float Ymin;
        public float Ymax;
        public string AnchorMin { get; set; }
        public string AnchorMax { get; set; }
        public override string ToString();
    }

    [SuppressMessage("ReSharper", "CompareOfFloatsByEqualityOperator")]
    private static List<Position> GetPositions(int colums, int rows, float colPadding, float rowPadding, bool columsFirst);
     Dictionary<string, string> Messages;
     object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
}

public class PlayerSetting
{
    public Dictionary<uint, CupboardSetting> CupboardsList;
}

public class CupboardSetting
{
    public int MaxLimit;
    public bool Announcement;
    public bool AuthOther;
    public string CupboardName;
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Стандартный лимит авторизаций в шкафу")]
    public int StandartAuthLimit;
    [JsonProperty(PropertyName = "Лимит авторизаций в шкафу с привилегией [Привилегия:Максимальный лимит]")]
    public Dictionary<string, int> PrivilageList;
    [JsonProperty(PropertyName = "Общая привилегия на использование UI настроек шкафа")]
    public string DefaultPermission;
    public static PluginConfig DefaultConfig();
}

 class Position
{
    public float Xmin;
    public float Xmax;
    public float Ymin;
    public float Ymax;
    public string AnchorMin { get; set; }
    public string AnchorMax { get; set; }
    public override string ToString();
}


```

---

## CustomBackpacks-2.1.4

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;
using Array = System.Array;

[Info("CustomBackpacks", "misty.dev (Cobalt Studios)", "2.1.4")]
[Description("Allows you to create custom backpacks and add them to the loot tables.")]
public class CustomBackpacks : RustPlugin
{
    private Configuration _config;
    private Dictionary<ulong, Item> _backpacks;
    private static VersionNumber _configVersion;
    private const GameTip.Styles Error;
    private void Init();
    private object OnBackpackDrop(Item backpack, PlayerInventory inv);
    private void OnPlayerRespawn(BasePlayer player, BasePlayer.SpawnPoint _);
    private void OnLootSpawn(LootContainer container);
    private void ConsoleGivePlayerBackpack(ConsoleSystem.Arg arg);
    private void GivePlayerBackpack(BasePlayer player, string command, string[] args);
    private void RemoveBackpacks(LootContainer container);
    private Item GetBackpack(string shortname, int capacity, string name);
    private void GiveBackpack(BasePlayer player, string? name);
    private bool CanAccept(Item item, int _);
    protected override void LoadDefaultMessages();
    private void SendMessage(BasePlayer? player, string key, GameTip.Styles style, object[] args);
    private class Configuration
    {
        [JsonProperty("RemoveDefaultBackpacks", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public bool RemoveDefaultBackpacks;
        [JsonProperty("Backpacks", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, BackpackInfo> Backpacks;
        [JsonProperty("Command Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, string> Commands;
        [JsonProperty("LootSpawns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, Dictionary<string, float>> LootSpawns;
        [JsonProperty("Version")]
        public VersionNumber VersionNumber;
    }

    private class BackpackInfo
    {
        [JsonProperty("Shortname")]
        public string Shortname;
        [JsonProperty("SaveContentsOnDeath")]
        public bool SaveContentsOnDeath;
        [JsonProperty("Capacity")]
        public int Capacity;
        [JsonProperty("ItemBlackList")]
        public List<string> ItemBlackList;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
}

private class Configuration
{
    [JsonProperty("RemoveDefaultBackpacks", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public bool RemoveDefaultBackpacks;
    [JsonProperty("Backpacks", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, BackpackInfo> Backpacks;
    [JsonProperty("Command Names", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, string> Commands;
    [JsonProperty("LootSpawns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, Dictionary<string, float>> LootSpawns;
    [JsonProperty("Version")]
    public VersionNumber VersionNumber;
}

private class BackpackInfo
{
    [JsonProperty("Shortname")]
    public string Shortname;
    [JsonProperty("SaveContentsOnDeath")]
    public bool SaveContentsOnDeath;
    [JsonProperty("Capacity")]
    public int Capacity;
    [JsonProperty("ItemBlackList")]
    public List<string> ItemBlackList;
}


```

---

## CustomCommands

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections;
using System.IO;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("CustomCommands", "Absolut", "1.0.2", ResourceId = 2158)]
 class CustomCommands : RustPlugin
{
    [PluginReference]
     Plugin BetterChat;
    static GameObject webObject;
    static CCImages ccImage;
    static buttonImage bImage;
     CustomCommandData ccData;
    private DynamicConfigFile CCData;
     ButtonImages bimages;
    private DynamicConfigFile BImages;
     class ButtonImages
    {
        public List<string> AdminImages;
    }

     string TitleColor;
     string MsgColor;
     bool ForceBar;
    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, CommandCreation> cmdCreation;
    private List<ulong> UIOpen;
    private List<ulong> Mouse;
     void Loaded();
     void Unload();
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerInit(BasePlayer player);
    private void InitializePlayer(BasePlayer player);
    private void DestroyPlayer(BasePlayer player);
     void OnServerInitialized();
    private object OnPlayerChat(ConsoleSystem.Arg arg);
     object OnBetterChat(IPlayer iplayer, string message);
    private void CommandCreationChat(BasePlayer player, string[] Args);
    public void DestroyCCPanel(BasePlayer player);
    public void DestroyCreationPanel(BasePlayer player);
    private string GetLang(string msg);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3, string arg4);
    private string GetMSG(string message, string arg1, string arg2, string arg3, string arg4);
     bool isAuth(BasePlayer player);
    private string PanelCC;
    private string PanelCreation;
    private string PanelMouse;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer element, string panel, string png, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    }

    private Dictionary<string, string> UIColors;
    private Dictionary<string, string> TextColors;
     void CCPanel(BasePlayer player, string mode);
    private void CreateCommand(BasePlayer player, int step);
    private void CreateButtonSelection(CuiElementContainer container, string panelName, uint img, int num);
    private void CreateCommandSelection(CuiElementContainer container, string panelName, string cmd, string type, uint img, int num);
     void FreeMouse(BasePlayer player);
    private float[] CmdButtonPos(int number);
    private float[] CalcButtonPos(int number);
    [ConsoleCommand("MouseFreeLookUI")]
    private void cmdMouseUI(ConsoleSystem.Arg arg);
    private void MousePanel(BasePlayer player);
    [ConsoleCommand("UI_DestroyMouse")]
    private void cmdUI_DestroyMouse(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DestroyCC")]
    private void cmdUI_DestroyCC(ConsoleSystem.Arg arg);
    [ChatCommand("cc")]
    private void cmdcc(BasePlayer player, string command, string[] args);
    [ConsoleCommand("any")]
    private void cmdchat(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CCPanel")]
    private void cmdUI_CCPanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_RemoveCommand")]
    private void cmdUI_RemoveCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SavePanel")]
    private void cmdUI_SavePanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ForcePanel")]
    private void cmdUI_ForcePanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CreateCommand")]
    private void cmdUI_CreateCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AddCommand")]
    private void cmdUI_AddCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AddPlayerCommand")]
    private void cmdUI_AddPlayerCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SetType")]
    private void cmdUI_SetType(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SetImg")]
    private void cmdUI_SetImg(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SaveCommand")]
    private void cmdUI_SaveCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SavePlayerCommand")]
    private void cmdUI_SavePlayerCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CancelCommand")]
    private void cmdUI_CancelCommand(ConsoleSystem.Arg arg);
    private void SaveLoop();
    private void InfoLoop();
    private void SetBoxFullNotification(string ID);
     class CustomCommandData
    {
        public Dictionary<string, uint> SavedImages;
        public List<uint> ButtonImages;
        public List<Command> ForceCommands;
        public List<Command> AdminCommands;
        public Dictionary<ulong, List<Command>> PlayerCommands;
    }

     class Command
    {
        public string cmd;
        public string type;
        public uint img;
    }

     class CommandCreation
    {
        public int step;
        public Command cmd;
    }

     class QueueImage
    {
        public string url;
        public string name;
        public QueueImage(string st, string ur);
    }

     class CCImages : MonoBehaviour
    {
         CustomCommands filehandler;
        const int MaxActiveLoads;
        static readonly List<QueueImage> QueueList;
        static byte activeLoads;
        private MemoryStream stream;
        public void SetDataDir(CustomCommands cc);
        public void Add(string name, string url);
         void Next();
        private void ClearStream();
         IEnumerator WaitForRequest(WWW www, QueueImage info);
    }

    [ConsoleCommand("getUIimages")]
    private void cmdgetimages(ConsoleSystem.Arg arg);
    private void Getimages();
    [ConsoleCommand("checkUIimages")]
    private void cmdrefreshimages(ConsoleSystem.Arg arg);
    private void Refreshimages();
     class QueueAdminImage
    {
        public string url;
        public QueueAdminImage(string ur);
    }

     class buttonImage : MonoBehaviour
    {
         CustomCommands filehandler;
        const int MaxActiveLoads;
        static readonly List<QueueAdminImage> QueueList;
        static byte activeLoads;
        private MemoryStream stream;
        public void SetDataDir(CustomCommands cc);
        public void Add(string url);
         void Next();
        private void ClearStream();
         IEnumerator WaitForRequest(WWW www, QueueAdminImage info);
    }

    [ConsoleCommand("getserverimages")]
    private void cmdgetserverimages(ConsoleSystem.Arg arg);
    private void GetServerImages();
    private void SaveServerImages();
    private Dictionary<string, string> urls;
    private List<string> defaultButtonImages;
     void SaveData();
     void LoadData();
    private ConfigData configData;
     class ConfigData
    {
        public int InfoInterval { get; set; }
        public string OptionsKeyBinding { get; set; }
        public bool AdminOnlyButtons { get; set; }
        public bool AdminCreatesButtons { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

 class ButtonImages
{
    public List<string> AdminImages;
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer element, string panel, string png, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
}

 class CustomCommandData
{
    public Dictionary<string, uint> SavedImages;
    public List<uint> ButtonImages;
    public List<Command> ForceCommands;
    public List<Command> AdminCommands;
    public Dictionary<ulong, List<Command>> PlayerCommands;
}

 class Command
{
    public string cmd;
    public string type;
    public uint img;
}

 class CommandCreation
{
    public int step;
    public Command cmd;
}

 class QueueImage
{
    public string url;
    public string name;
    public QueueImage(string st, string ur);
}

 class CCImages : MonoBehaviour
{
     CustomCommands filehandler;
    const int MaxActiveLoads;
    static readonly List<QueueImage> QueueList;
    static byte activeLoads;
    private MemoryStream stream;
    public void SetDataDir(CustomCommands cc);
    public void Add(string name, string url);
     void Next();
    private void ClearStream();
     IEnumerator WaitForRequest(WWW www, QueueImage info);
}

 class QueueAdminImage
{
    public string url;
    public QueueAdminImage(string ur);
}

 class buttonImage : MonoBehaviour
{
     CustomCommands filehandler;
    const int MaxActiveLoads;
    static readonly List<QueueAdminImage> QueueList;
    static byte activeLoads;
    private MemoryStream stream;
    public void SetDataDir(CustomCommands cc);
    public void Add(string url);
     void Next();
    private void ClearStream();
     IEnumerator WaitForRequest(WWW www, QueueAdminImage info);
}

 class ConfigData
{
    public int InfoInterval { get; set; }
    public string OptionsKeyBinding { get; set; }
    public bool AdminOnlyButtons { get; set; }
    public bool AdminCreatesButtons { get; set; }
}


```

---

## CustomCraft

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Reflection;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("CustomCraft", "CASHR", "1.0.0")]
public class CustomCraft : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    public string Layer;
    public string LayerBlur;
    public Dictionary<ulong, int> playerModifity;
    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    public Configuration _config;
    public class Configuration
    {
        [JsonProperty("Новые предметы")]
        public List<ItemInfo> items;
    }

    public class ItemInfo
    {
        [JsonProperty("Название предмета")]
        public string name;
        [JsonProperty("Текст кнопки")]
        public string textBtn;
        [JsonProperty("Шортнейм предмета")]
        public string shortname;
        [JsonProperty("Айди предмета")]
        public int idItem;
        [JsonProperty("Верстак для крафта")]
        public int WorkBench;
        [JsonProperty("Время крафта( в секундках) ")]
        public float timeCraft;
        [JsonProperty("Крафтить шт")]
        public int countCraft;
        [JsonProperty("СкинИД предмета[Не менять]")]
        public ulong skinID;
        [JsonProperty("Ссылка на иконку")]
        public string png;
        [JsonProperty("Отображать крафт предмета")]
        public bool isCrafted;
        [JsonProperty("Описание для крафта")]
        public string descriptionCraft;
        [JsonProperty("Предметы для крафта")]
        public List<CraftInfo> craftItems;
    }

    public class CraftInfo
    {
        [JsonProperty("Шортнейм предмета")]
        public string shortname;
        [JsonProperty("Иконка предмета")]
        public string icons;
        [JsonProperty("Количество предмета")]
        public int amount;
        [JsonProperty("СкинИД предмета")]
        public ulong skinID;
        [JsonProperty("АйтемИД предмета")]
        public int itemID;
    }

    [ChatCommand("craft")]
     void chatCmdCraft(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_CustomCraft")]
     void consoleCmdCustomCraft(ConsoleSystem.Arg arg);
    private void OnServerInitialized();
    public void ReplyWithHelper(BasePlayer player, string message, string[] args);
    private static string HexToRGB(string hex);
    public Item FindItem(BasePlayer player, int itemID, ulong skinID, int amount);
    public bool HaveItem(BasePlayer player, int itemID, ulong skinID, int amount);
    private string GetImage(string fileName, ulong skin);
    public void UpdateItemsInfo(BasePlayer player, string name);
    private void CraftGUI(BasePlayer player);
}

public class Configuration
{
    [JsonProperty("Новые предметы")]
    public List<ItemInfo> items;
}

public class ItemInfo
{
    [JsonProperty("Название предмета")]
    public string name;
    [JsonProperty("Текст кнопки")]
    public string textBtn;
    [JsonProperty("Шортнейм предмета")]
    public string shortname;
    [JsonProperty("Айди предмета")]
    public int idItem;
    [JsonProperty("Верстак для крафта")]
    public int WorkBench;
    [JsonProperty("Время крафта( в секундках) ")]
    public float timeCraft;
    [JsonProperty("Крафтить шт")]
    public int countCraft;
    [JsonProperty("СкинИД предмета[Не менять]")]
    public ulong skinID;
    [JsonProperty("Ссылка на иконку")]
    public string png;
    [JsonProperty("Отображать крафт предмета")]
    public bool isCrafted;
    [JsonProperty("Описание для крафта")]
    public string descriptionCraft;
    [JsonProperty("Предметы для крафта")]
    public List<CraftInfo> craftItems;
}

public class CraftInfo
{
    [JsonProperty("Шортнейм предмета")]
    public string shortname;
    [JsonProperty("Иконка предмета")]
    public string icons;
    [JsonProperty("Количество предмета")]
    public int amount;
    [JsonProperty("СкинИД предмета")]
    public ulong skinID;
    [JsonProperty("АйтемИД предмета")]
    public int itemID;
}


```

---

## CustomCraftTimes

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Custom Craft Times", "Camoec", "1.1.1")]
[Description("Allows you to change the crafting times")]
public class CustomCraftTimes : RustPlugin
{
    private const string UsePerm;
     Dictionary<int, BPItem> _restore;
    private PluginConfig _config;
    private class BPItem
    {
        public string shortname;
        public float time;
    }

    private class PluginConfig
    {
        public Dictionary<int,BPItem> itemdefinitions;
    }

    private void Init();
    protected override void SaveConfig();
    private void _LoadDefaultConfig();
    private void _LoadConfig();
    [ConsoleCommand("cct")]
     void GlobalSetup(ConsoleSystem.Arg arg);
     void OnServerInitialized(bool initial);
     void Unload();
}

private class BPItem
{
    public string shortname;
    public float time;
}

private class PluginConfig
{
    public Dictionary<int,BPItem> itemdefinitions;
}


```

---

## CustomDropChanger

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("CustomDropChanger", "Server-Rust.ru", "1.1.4")]
[Description("Позволяет добавить лут во все ящики, скачано с Server-Rust.ru")]
 class CustomDropChanger : RustPlugin
{
    private List<string> containerNames;
    private Dictionary<uint, int> conditionPendingList;
    private Dictionary<ItemContainer, List<Item>> spawnedLoot;
    private List<LootContainer> affectedContainers;
    private bool isReady;
    private NewDropItemConfig config;
    private class ItemDropConfig
    {
        [JsonProperty("Название предмета (ShortName)")]
        public string ItemShortName;
        [JsonProperty("Минимальное количество предмета")]
        public int MinCount;
        [JsonProperty("Максимальное количество предмета")]
        public int MaxCount;
        [JsonProperty("Шанс выпадения предмета (0 - отключить)")]
        public int Chance;
        [JsonProperty("Это чертеж?")]
        public bool IsBluePrint;
        [JsonProperty("Имя предмета (оставьте пустым для стандартного)")]
        public string Name;
        [JsonProperty("Описание предмета (оставьте пустым для стандартного)")]
        public string Description;
        [JsonProperty("ID Скина (0 - стандартный)")]
        public ulong Skin;
        [JsonProperty("Целостность предмета в % от 1 до 100 (0 - стандартная)")]
        public int Condition;
    }

    private class NewDropItemConfig
    {
        [JsonProperty("Добавляем лут в ученых?")]
        public bool EnableScientistLoot { get; set; }
        [JsonProperty("Оставить стандартный лут в контейнерах и ученых?")]
        public bool EnableStandartLoot { get; set; }
        [JsonProperty("Список лута ученых:")]
        public List<ItemDropConfig> ScientistLootSettings { get; set; }
        [JsonProperty("Настройка контейнеров:")]
        public Dictionary<string, List<ItemDropConfig>> ChestSettings { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Loaded();
     void Unload();
    private List<ItemDropConfig> GetItemListByChest(string chestname);
    private void AddToContainer(ItemDropConfig item, ItemContainer container);
    private void ProcessScientist(ItemContainer container);
    private void ProcessContainer(LootContainer container);
    private void OnLootSpawn(LootContainer lootContainer);
    private void OnEntitySpawned(BaseNetworkable entity);
     void OnItemAddedToContainer(ItemContainer container, Item item);
}

private class ItemDropConfig
{
    [JsonProperty("Название предмета (ShortName)")]
    public string ItemShortName;
    [JsonProperty("Минимальное количество предмета")]
    public int MinCount;
    [JsonProperty("Максимальное количество предмета")]
    public int MaxCount;
    [JsonProperty("Шанс выпадения предмета (0 - отключить)")]
    public int Chance;
    [JsonProperty("Это чертеж?")]
    public bool IsBluePrint;
    [JsonProperty("Имя предмета (оставьте пустым для стандартного)")]
    public string Name;
    [JsonProperty("Описание предмета (оставьте пустым для стандартного)")]
    public string Description;
    [JsonProperty("ID Скина (0 - стандартный)")]
    public ulong Skin;
    [JsonProperty("Целостность предмета в % от 1 до 100 (0 - стандартная)")]
    public int Condition;
}

private class NewDropItemConfig
{
    [JsonProperty("Добавляем лут в ученых?")]
    public bool EnableScientistLoot { get; set; }
    [JsonProperty("Оставить стандартный лут в контейнерах и ученых?")]
    public bool EnableStandartLoot { get; set; }
    [JsonProperty("Список лута ученых:")]
    public List<ItemDropConfig> ScientistLootSettings { get; set; }
    [JsonProperty("Настройка контейнеров:")]
    public Dictionary<string, List<ItemDropConfig>> ChestSettings { get; set; }
}


```

---

## CustomIcon

```csharp
using System.Collections.Generic;
using Network;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Custom Icon", "collect_vood", "1.0.4")]
[Description("Set a customizable icon for all non user messages")]
 class CustomIcon : CovalencePlugin
{
    private Configuration _configuration;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Steam Avatar User ID")]
        public ulong SteamAvatarUserID;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void OnBroadcastCommand(string command, object[] args);
    private void OnSendCommand(Connection cn, string command, object[] args);
    private void OnSendCommand(List<Connection> cn, string command, object[] args);
    private void TryApplySteamAvatarUserID(string command, object[] args);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Steam Avatar User ID")]
    public ulong SteamAvatarUserID;
}


```

---

## CustomItemDefinitions-1.3.0

```csharp
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Reflection;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System;
using SilentOrbit.ProtocolBuffers;
using System.Linq;
using System.Diagnostics;
using System.IO;
using Network;
using HarmonyLib;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Plugins.CustomItemDefinitionExtensions;
using UnityEngine.SceneManagement;
using Oxide.Core;
using System.Collections;

Oxide.Plugins
[Info("CustomItemDefinitions", "0xF [dsc.gg/0xf-plugins]", "1.3.0")]
[Description("Library of the Future. Allows you to create your own full-fledged custom items with own item definition.")]
public class CustomItemDefinitions : RustPlugin
{
    public const ItemDefinition.Flag CUSTOM_DEFINITION_FLAG;
    public static readonly ItemDefinition FallbackItemDefinition;
    public static readonly ItemDefinition FakeBlueprintItemDefinition;
    public class CustomItemDefinition
    {
        public int parentItemId;
        public string shortname;
        public int? itemId;
        public string defaultName;
        public string defaultDescription;
        public ulong defaultSkinId;
        public int? maxStackSize;
        public ItemCategory? category;
        public ItemDefinition.Flag flags;
        public ItemMod[] itemMods;
        public bool repairable;
        public bool craftable;
        public List<ItemAmount> blueprintIngredients;
        public int workbenchLevelRequired;
        public static CustomItemDefinition FromObject(object @object);
    }

    private static CustomItemDefinitions PluginInstance;
    private static Dictionary<int, ItemDefinition> AlreadyCreatedItemDefinitions;
    private static Dictionary<Plugin, HashSet<CustomItemDefinition>> CustomDefinitions;
     void Init();
    private void Loaded();
    private void Unload();
    private static void OnItemDefinitionBroken(Item item, ProtoBuf.Item protoItem);
     void OnEntitySaved(BaseNetworkable entity, BaseNetworkable.SaveInfo saveInfo);
     void OnLootNetworkUpdate(PlayerLoot loot);
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
     object OnItemCraft(IndustrialCrafter crafter, ItemBlueprint blueprint);
    [ConsoleCommand("itemid")]
    private void itemid(ConsoleSystem.Arg arg);
    public static class FieldsToMutate
    {
        public static List<FieldInfo> validFields;
        public static void Initialize();
        private static List<FieldInfo> CollectFields(Type type);
        public static bool IsValidField(FieldInfo field);
    }

    public static class Mutate
    {
        public static readonly Dictionary<Type, Action<object>> TypeToMethodMap;
        public static void ToClientSide(ProtoBuf.Entity protoEntity);
        public static void ToClientSide(object obj);
        public static void ToClientSide(ProtoBuf.VendingMachine.SellOrderContainer sellOrderContainer);
        public static void ToClientSide(List<ProtoBuf.VendingMachine.SellOrder> sellOrders);
        public static void ToClientSide(ProtoBuf.VendingMachine.SellOrder sellOrder);
        public static void ToClientSide(List<ProtoBuf.WeaponRackItem> weaponRackItems);
        public static void ToClientSide(ProtoBuf.WeaponRackItem rackItem);
        public static void ToClientSide(List<ProtoBuf.IndustrialConveyor.ItemFilter> filters);
        public static void ToClientSide(ProtoBuf.IndustrialConveyor.ItemFilter filter);
        public static void ToClientSide(List<ProtoBuf.IndustrialConveyorTransfer.ItemTransfer> transfers);
        public static void ToClientSide(List<ProtoBuf.ItemCrafter.Task> tasks);
        public static void ToClientSide(ProtoBuf.ItemCrafter.Task task);
        public static void ToClientSide(ProtoBuf.ItemAmountList itemAmountList);
        public static void ToClientSide(List<ProtoBuf.AppEntityPayload.Item> items);
        public static void ToClientSide(ProtoBuf.AppEntityPayload.Item item);
        public static void ToClientSide(List<ProtoBuf.AppMarker.SellOrder> sellOrders);
        public static void ToClientSide(ProtoBuf.AppMarker.SellOrder sellOrder);
        public static void ToClientSide(ProtoBuf.FrankensteinTable frankensteinTable);
        public static void ToClientSide(List<ProtoBuf.ItemContainer> containers);
        public static void ToClientSide(ProtoBuf.ItemContainer container);
        public static void ToClientSide(List<ProtoBuf.Item> items);
        public static void ToClientSide(ProtoBuf.Item protoItem);
    }

    private object Register(object @object, Plugin plugin);
    private static void CacheAlreadyCreated();
    public static HashSet<ItemDefinition> RegisterPluginItemDefinitions(IEnumerable<CustomItemDefinition> definitions, Plugin plugin);
    public static ItemDefinition RegisterPluginItemDefinition(CustomItemDefinition definition, Plugin plugin);
    public static void UnloadPluginItemDefinitions(Plugin plugin);
    public static void UnloadPluginItemDefinition(Plugin plugin, CustomItemDefinition definition);
    private static void UnloadCustomItemDefinition(CustomItemDefinition definition);
    private static ItemDefinition CloneItemDefinition(ItemDefinition itemDefinition);
    private static ItemDefinition CloneItemDefinition(string shortname);
    private static ItemDefinition CloneItemDefinition(int itemId);
    private static bool IsValidCustomItemDefinition(ItemDefinition itemDefinition);
    private static void SendSnapshotWithUnlockedBlueprint(BasePlayer player, int itemId);
    private static void SendFakeUnlockedBlueprint(BasePlayer player, int itemId);
    private static string FormatNameWithDescription(string name, string description);
    private class DefaultProperties : ItemMod
    {
        public string name;
        public string description;
        public string finalName;
        public ulong skinId;
    }

    public class ItemAmountConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanRead { get; set; }
    }

    internal static class Patches
    {
        internal static void UnloadPlugin(string name);
        internal static void Item_Load1(Item __instance, ProtoBuf.Item __0);
        internal static void Item_Load2(Item __instance, ProtoBuf.Item __0);
        internal static void VendingMachineMapMarker_GetAppMarkerData(ProtoBuf.AppMarker __result);
        internal static void StorageMonitor_FillEntityPayload(ProtoBuf.AppEntityPayload payload);
        internal static bool PlayerUpdateLoot_WriteToStream(ProtoBuf.PlayerUpdateLoot __instance);
        internal static bool UpdateItem_WriteToStream(ProtoBuf.UpdateItem __instance);
        internal static bool UpdateItemContainer_WriteToStream(ProtoBuf.UpdateItemContainer __instance);
        internal static bool SellOrderContainer_WriteToStream(ProtoBuf.VendingMachine.SellOrderContainer __instance);
        internal static bool ItemAmountList_WriteToStream(ProtoBuf.ItemAmountList __instance);
        internal static bool ItemFilterList_WriteToStream(ProtoBuf.IndustrialConveyor.ItemFilterList __instance);
        internal static bool IndustrialConveyorTransfer_WriteToStream(ProtoBuf.IndustrialConveyorTransfer __instance);
        internal static void CuiItemIconPatch(CuiImageComponent __instance, int __result);
    }

}

public class CustomItemDefinition
{
    public int parentItemId;
    public string shortname;
    public int? itemId;
    public string defaultName;
    public string defaultDescription;
    public ulong defaultSkinId;
    public int? maxStackSize;
    public ItemCategory? category;
    public ItemDefinition.Flag flags;
    public ItemMod[] itemMods;
    public bool repairable;
    public bool craftable;
    public List<ItemAmount> blueprintIngredients;
    public int workbenchLevelRequired;
    public static CustomItemDefinition FromObject(object @object);
}

public static class FieldsToMutate
{
    public static List<FieldInfo> validFields;
    public static void Initialize();
    private static List<FieldInfo> CollectFields(Type type);
    public static bool IsValidField(FieldInfo field);
}

public static class Mutate
{
    public static readonly Dictionary<Type, Action<object>> TypeToMethodMap;
    public static void ToClientSide(ProtoBuf.Entity protoEntity);
    public static void ToClientSide(object obj);
    public static void ToClientSide(ProtoBuf.VendingMachine.SellOrderContainer sellOrderContainer);
    public static void ToClientSide(List<ProtoBuf.VendingMachine.SellOrder> sellOrders);
    public static void ToClientSide(ProtoBuf.VendingMachine.SellOrder sellOrder);
    public static void ToClientSide(List<ProtoBuf.WeaponRackItem> weaponRackItems);
    public static void ToClientSide(ProtoBuf.WeaponRackItem rackItem);
    public static void ToClientSide(List<ProtoBuf.IndustrialConveyor.ItemFilter> filters);
    public static void ToClientSide(ProtoBuf.IndustrialConveyor.ItemFilter filter);
    public static void ToClientSide(List<ProtoBuf.IndustrialConveyorTransfer.ItemTransfer> transfers);
    public static void ToClientSide(List<ProtoBuf.ItemCrafter.Task> tasks);
    public static void ToClientSide(ProtoBuf.ItemCrafter.Task task);
    public static void ToClientSide(ProtoBuf.ItemAmountList itemAmountList);
    public static void ToClientSide(List<ProtoBuf.AppEntityPayload.Item> items);
    public static void ToClientSide(ProtoBuf.AppEntityPayload.Item item);
    public static void ToClientSide(List<ProtoBuf.AppMarker.SellOrder> sellOrders);
    public static void ToClientSide(ProtoBuf.AppMarker.SellOrder sellOrder);
    public static void ToClientSide(ProtoBuf.FrankensteinTable frankensteinTable);
    public static void ToClientSide(List<ProtoBuf.ItemContainer> containers);
    public static void ToClientSide(ProtoBuf.ItemContainer container);
    public static void ToClientSide(List<ProtoBuf.Item> items);
    public static void ToClientSide(ProtoBuf.Item protoItem);
}

private class DefaultProperties : ItemMod
{
    public string name;
    public string description;
    public string finalName;
    public ulong skinId;
}

public class ItemAmountConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanRead { get; set; }
}

internal static class Patches
{
    internal static void UnloadPlugin(string name);
    internal static void Item_Load1(Item __instance, ProtoBuf.Item __0);
    internal static void Item_Load2(Item __instance, ProtoBuf.Item __0);
    internal static void VendingMachineMapMarker_GetAppMarkerData(ProtoBuf.AppMarker __result);
    internal static void StorageMonitor_FillEntityPayload(ProtoBuf.AppEntityPayload payload);
    internal static bool PlayerUpdateLoot_WriteToStream(ProtoBuf.PlayerUpdateLoot __instance);
    internal static bool UpdateItem_WriteToStream(ProtoBuf.UpdateItem __instance);
    internal static bool UpdateItemContainer_WriteToStream(ProtoBuf.UpdateItemContainer __instance);
    internal static bool SellOrderContainer_WriteToStream(ProtoBuf.VendingMachine.SellOrderContainer __instance);
    internal static bool ItemAmountList_WriteToStream(ProtoBuf.ItemAmountList __instance);
    internal static bool ItemFilterList_WriteToStream(ProtoBuf.IndustrialConveyor.ItemFilterList __instance);
    internal static bool IndustrialConveyorTransfer_WriteToStream(ProtoBuf.IndustrialConveyorTransfer __instance);
    internal static void CuiItemIconPatch(CuiImageComponent __instance, int __result);
}

Oxide.Plugins.CustomItemDefinitionExtensions
public static class ItemModExtensions
{
    public static void CopyFields(ItemMod from, ItemMod to);
}


```

---

## CustomItems

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System;

Oxide.Plugins
[Info("CustomItems", "rdapplehappy", "1.3")]
 class CustomItems : RustPlugin
{
     List<Ore> Ores;
    private string MessageTake;
    private bool MessageEnabled;
    private bool ItemRecycle;
    private List<ulong> Skins;
     class Ore
    {
        public string name;
        public ulong id;
        public string Description;
        public bool Recycled;
        public int MinCount;
        public int Maxcount;
        public Dictionary<string, int> itemsdrop;
        public int ItemId;
        public int ChanceStone;
        public int ChanceSulfur;
        public int ChanceMetall;
        public int ChanceBarrel;
        public int ChanceCrate_underwater;
        public int StandartCrateChance;
        public int GreenCrateChance;
        public int EliteCrateChance;
    }

    private void LoadDefaultConfig();
     void OnServerInitialized();
     Item OnItemSplit(Item item, int amount);
     object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
     void OnDispenserGather(ResourceDispenser dis, BaseEntity ent, Item it);
     void OnLootSpawn(LootContainer container);
     void CanRecycle(Recycler recycler, Item item);
     object OnRecycleItem(Recycler recycler, Item item);
    private Ore FindOre(string shortname, string itemName, ulong skin);
    private void CreateItem(string shortName, string itemName, ulong skin);
    protected void LoadData();
    protected void SaveData();
    protected void GenerateData();
    protected bool CheckData();
     bool isGenerated(string shortName, string itemName, ulong skin);
     void itemGenerate(string shortName, string itemName, ulong skin);
    private bool OreRandom(Ore ore, OresName name);
    private OresName GetOreName(string name);
    private OresName GetCrateName(Crates crates);
    private void GetSkinsId();
    private void GetConfig(string menu, string Key, T var);
    private Item AddItem(BasePlayer p, string name, ulong skin, string shortname, int count);
    private void GiveItems(Ore ore, BasePlayer player);
    private void GivetoBox(Ore ore, LootContainer cont);
    private void MessageSend(string message, BasePlayer player);
    private Ore RandomGather(OresName name);
    private void BoxDrop(LootContainer cont, Crates crate);
}

 class Ore
{
    public string name;
    public ulong id;
    public string Description;
    public bool Recycled;
    public int MinCount;
    public int Maxcount;
    public Dictionary<string, int> itemsdrop;
    public int ItemId;
    public int ChanceStone;
    public int ChanceSulfur;
    public int ChanceMetall;
    public int ChanceBarrel;
    public int ChanceCrate_underwater;
    public int StandartCrateChance;
    public int GreenCrateChance;
    public int EliteCrateChance;
}


```

---

## CustomNPC

```csharp
using Facepunch;
using Newtonsoft.Json.Linq;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using UnityEngine;
using UnityEngine.AI;

Oxide.Plugins
[Info("CustomNPC", "k1lly0u", "1.2.2")]
 class CustomNPC : RustPlugin
{
    [PluginReference]
    private Plugin Kits;
    private Hash<Plugin, List<HumanAI>> _humanAIRef;
    private static CustomNPC Instance;
    private void Loaded();
    private void OnEntityKill(ScientistNPC scientistNpc);
    private object OnNpcTarget(ScientistNPC scientistNpc, BasePlayer target);
    private void OnPluginUnloaded(Plugin plugin);
    private void Unload();
    private HumanAI CreateHumanAI(Vector3 position, Settings settings);
    private HumanAI ConvertHumanAI(ScientistNPC scientistNPC, Settings settings);
    private static ScientistNPC InstantiateEntity(string type, Vector3 position);
    private static bool IsInSafeZone(Vector3 position);
    private static void StripInventory(BasePlayer player, bool skipWear);
    private ScientistNPC SpawnNPC(Plugin plugin, Vector3 position, JObject settingsJson);
    private bool ConvertNPC(Plugin plugin, ScientistNPC scientistNPC, JObject settingsJson);
    private bool AddCustomAIState(ScientistNPC scientistNPC, BaseAIBrain<global::HumanNPC>.BasicAIState state);
    private void SetRoamHomePosition(ScientistNPC scientistNPC, Vector3 position);
    private bool EnableNavAgent(ScientistNPC scientistNPC);
    private void EquipWeapon(ScientistNPC scientistNPC);
    private void HolsterWeapon(ScientistNPC scientistNPC);
    private void SetDestination(ScientistNPC scientistNPC, Vector3 destination, global::HumanNPC.SpeedType speed, Action onReachedDestination);
    internal class Settings
    {
        public List<string> States;
        public string NPCType;
        public string DisplayName;
        public float Health;
        public float SightRange;
        public float RoamRange;
        public float ChaseRange;
        public string Kit;
        public bool StripCorpseLoot;
        public bool KillInSafeZone;
        public float DespawnTime;
        public bool StartDead;
        public bool StartWounded;
        public float WoundedDuration;
        public float WoundedRecoveryChance;
        public bool EnableNavMesh;
        public bool EquipWeapon;
        public bool CanUseWeaponMounted;
    }

    private static NavMeshHit navmeshHit;
    private static RaycastHit raycastHit;
    private static Collider[] _buffer;
    private const int WORLD_LAYER;
    internal static object FindPointOnNavmesh(Vector3 targetPosition, float maxDistance);
    private static bool IsInRockPrefab(Vector3 position);
    private static bool IsNearWorldCollider(Vector3 position);
    private static string[] acceptedColliders;
    private static string[] blockedColliders;
    public class HumanAI : MonoBehaviour
    {
        internal ScientistNPC Entity { get; set; }
        internal Transform Transform { get; set; }
        internal CustomAIBrain Brain { get; set; }
        internal AttackEntity _attackEntity;
        internal Vector3 _roamBasePosition;
        internal Settings _settings;
        internal bool HasHomePosition { get; set; }
        internal float DistanceFromBase { get; set; }
        internal bool _NavMeshEnabled { get; set; }
        internal bool NavMeshEnabled { get; set; }
        private const int AREA_MASK;
        private const int AGENT_TYPE_ID;
        private void Awake();
        private void Start();
        internal void Setup(Settings settings);
        internal void AddStates();
        private void UpdateGear();
        internal void EnableNavAgent();
        internal void DisableNavAgent();
        internal void SetDestination(Vector3 destination);
        internal void SetDestination(Vector3 destination, global::HumanNPC.SpeedType speed, Action onReachedDestination);
        internal void ForgetEntity(BaseEntity baseEntity);
        private void UpdateTick();
        internal void EquipWeapon();
        internal void HolsterWeapon();
        internal void Die();
        internal void Die(HitInfo hitInfo);
        internal void Despawn();
        private void OnDestroy();
        private readonly static GameObjectRef[] ZombieDeathEffects;
        private readonly static GameObjectRef[] ZombieChatterEffects;
        private float nextSenseUpdateTime;
        private float nextFriendlyUpdateTime;
        private const float FRIENDLY_UPDATE_INTERVAL;
        private const float SENSE_UPDATE_INTERVAL;
        private const float SENSE_RANGE;
        private const float VISION_CONE;
        private static readonly BaseEntity[] QueryResults;
        private static readonly BasePlayer[] PlayerQueryResults;
        private List<HumanAI> friendlies;
        internal void UpdateSenses();
        internal void UpdateNearbyFriendlies();
        private void SenseBrains();
        private void SensePlayers();
        private bool IsFriendlyPlayer(BasePlayer player);
        private bool CaresAbout(BaseEntity entity);
        private bool CanSenseType(BaseEntity ent);
        private bool CanSeeTarget(BaseEntity entity);
        private bool IsTargetInVision(BaseEntity target);
    }

    public class CustomAIBrain : BaseAIBrain<globalHumanNPC>
    {
        private HumanAI humanAI;
        private void Awake();
        public override void AddStates();
        public override void InitializeAI();
        public override void Think(float delta);
        internal void ClearStates();
        internal BasicAIState GetState();
        public class IdleState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            public IdleState(HumanAI humanAI);
            public override float GetWeight();
            public override void StateEnter();
        }

        public class WoundedState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            private readonly float woundedDuration;
            private readonly float woundedRecoveryChance;
            private bool isIncapacitated;
            private Vector3 destination;
            public WoundedState(HumanAI humanAI, float woundedDuration, float woundedRecoveryChance);
            public override float GetWeight();
            public override void StateEnter();
            public override void StateLeave();
            public override StateStatus StateThink(float delta);
        }

        public class ChaseState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            private readonly float chaseRange;
            public ChaseState(HumanAI humanAI, float chaseRange);
            public override float GetWeight();
            public override void StateEnter();
            public override StateStatus StateThink(float delta);
        }

        public class CombatState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            private readonly float chaseRange;
            private float nextStrafeTime;
            public CombatState(HumanAI humanAI, float chaseRange);
            public override float GetWeight();
            public override void StateEnter();
            public override void StateLeave();
            public override StateStatus StateThink(float delta);
            private void DoMeleeAttack();
            private void DoMeleeDamage(BaseMelee baseMelee);
        }

        public class RoamState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            private readonly float roamRange;
            private float nextSetDestinationTime;
            private float currentDestinationTime;
            private bool isAtDestination;
            private Vector3 roamDestination;
            public RoamState(HumanAI humanAI, float roamRange);
            public override float GetWeight();
            public override void StateEnter();
            public override void StateLeave();
            public override StateStatus StateThink(float delta);
        }

        public class MountedState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            private readonly bool canUseWeapon;
            public MountedState(HumanAI humanAI, bool canUseWeapon);
            public override float GetWeight();
            public override void StateEnter();
            public override void StateLeave();
        }

        public class MoveDestinationState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            private Vector3 destination;
            private global::HumanNPC.SpeedType speed;
            private Action onReachedDestination;
            private bool hasDestination;
            public MoveDestinationState(HumanAI humanAI);
            public void SetDestination(Vector3 destination, global::HumanNPC.SpeedType speed, Action onReachedDestination);
            public override float GetWeight();
            public override void StateEnter();
            public override void StateLeave();
            public override StateStatus StateThink(float delta);
        }

        public class FallingState : BaseAIBrain<globalHumanNPC>.BasicAIState
        {
            private readonly HumanAI humanAI;
            public FallingState(HumanAI humanAI);
            public override float GetWeight();
            public override void StateEnter();
            public override void StateLeave();
        }

    }

    [ChatCommand("spawnnpc")]
    private void cmdSpawnNPC(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
}

internal class Settings
{
    public List<string> States;
    public string NPCType;
    public string DisplayName;
    public float Health;
    public float SightRange;
    public float RoamRange;
    public float ChaseRange;
    public string Kit;
    public bool StripCorpseLoot;
    public bool KillInSafeZone;
    public float DespawnTime;
    public bool StartDead;
    public bool StartWounded;
    public float WoundedDuration;
    public float WoundedRecoveryChance;
    public bool EnableNavMesh;
    public bool EquipWeapon;
    public bool CanUseWeaponMounted;
}

public class HumanAI : MonoBehaviour
{
    internal ScientistNPC Entity { get; set; }
    internal Transform Transform { get; set; }
    internal CustomAIBrain Brain { get; set; }
    internal AttackEntity _attackEntity;
    internal Vector3 _roamBasePosition;
    internal Settings _settings;
    internal bool HasHomePosition { get; set; }
    internal float DistanceFromBase { get; set; }
    internal bool _NavMeshEnabled { get; set; }
    internal bool NavMeshEnabled { get; set; }
    private const int AREA_MASK;
    private const int AGENT_TYPE_ID;
    private void Awake();
    private void Start();
    internal void Setup(Settings settings);
    internal void AddStates();
    private void UpdateGear();
    internal void EnableNavAgent();
    internal void DisableNavAgent();
    internal void SetDestination(Vector3 destination);
    internal void SetDestination(Vector3 destination, global::HumanNPC.SpeedType speed, Action onReachedDestination);
    internal void ForgetEntity(BaseEntity baseEntity);
    private void UpdateTick();
    internal void EquipWeapon();
    internal void HolsterWeapon();
    internal void Die();
    internal void Die(HitInfo hitInfo);
    internal void Despawn();
    private void OnDestroy();
    private readonly static GameObjectRef[] ZombieDeathEffects;
    private readonly static GameObjectRef[] ZombieChatterEffects;
    private float nextSenseUpdateTime;
    private float nextFriendlyUpdateTime;
    private const float FRIENDLY_UPDATE_INTERVAL;
    private const float SENSE_UPDATE_INTERVAL;
    private const float SENSE_RANGE;
    private const float VISION_CONE;
    private static readonly BaseEntity[] QueryResults;
    private static readonly BasePlayer[] PlayerQueryResults;
    private List<HumanAI> friendlies;
    internal void UpdateSenses();
    internal void UpdateNearbyFriendlies();
    private void SenseBrains();
    private void SensePlayers();
    private bool IsFriendlyPlayer(BasePlayer player);
    private bool CaresAbout(BaseEntity entity);
    private bool CanSenseType(BaseEntity ent);
    private bool CanSeeTarget(BaseEntity entity);
    private bool IsTargetInVision(BaseEntity target);
}

public class CustomAIBrain : BaseAIBrain<globalHumanNPC>
{
    private HumanAI humanAI;
    private void Awake();
    public override void AddStates();
    public override void InitializeAI();
    public override void Think(float delta);
    internal void ClearStates();
    internal BasicAIState GetState();
    public class IdleState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        public IdleState(HumanAI humanAI);
        public override float GetWeight();
        public override void StateEnter();
    }

    public class WoundedState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        private readonly float woundedDuration;
        private readonly float woundedRecoveryChance;
        private bool isIncapacitated;
        private Vector3 destination;
        public WoundedState(HumanAI humanAI, float woundedDuration, float woundedRecoveryChance);
        public override float GetWeight();
        public override void StateEnter();
        public override void StateLeave();
        public override StateStatus StateThink(float delta);
    }

    public class ChaseState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        private readonly float chaseRange;
        public ChaseState(HumanAI humanAI, float chaseRange);
        public override float GetWeight();
        public override void StateEnter();
        public override StateStatus StateThink(float delta);
    }

    public class CombatState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        private readonly float chaseRange;
        private float nextStrafeTime;
        public CombatState(HumanAI humanAI, float chaseRange);
        public override float GetWeight();
        public override void StateEnter();
        public override void StateLeave();
        public override StateStatus StateThink(float delta);
        private void DoMeleeAttack();
        private void DoMeleeDamage(BaseMelee baseMelee);
    }

    public class RoamState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        private readonly float roamRange;
        private float nextSetDestinationTime;
        private float currentDestinationTime;
        private bool isAtDestination;
        private Vector3 roamDestination;
        public RoamState(HumanAI humanAI, float roamRange);
        public override float GetWeight();
        public override void StateEnter();
        public override void StateLeave();
        public override StateStatus StateThink(float delta);
    }

    public class MountedState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        private readonly bool canUseWeapon;
        public MountedState(HumanAI humanAI, bool canUseWeapon);
        public override float GetWeight();
        public override void StateEnter();
        public override void StateLeave();
    }

    public class MoveDestinationState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        private Vector3 destination;
        private global::HumanNPC.SpeedType speed;
        private Action onReachedDestination;
        private bool hasDestination;
        public MoveDestinationState(HumanAI humanAI);
        public void SetDestination(Vector3 destination, global::HumanNPC.SpeedType speed, Action onReachedDestination);
        public override float GetWeight();
        public override void StateEnter();
        public override void StateLeave();
        public override StateStatus StateThink(float delta);
    }

    public class FallingState : BaseAIBrain<globalHumanNPC>.BasicAIState
    {
        private readonly HumanAI humanAI;
        public FallingState(HumanAI humanAI);
        public override float GetWeight();
        public override void StateEnter();
        public override void StateLeave();
    }

}

public class IdleState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    public IdleState(HumanAI humanAI);
    public override float GetWeight();
    public override void StateEnter();
}

public class WoundedState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    private readonly float woundedDuration;
    private readonly float woundedRecoveryChance;
    private bool isIncapacitated;
    private Vector3 destination;
    public WoundedState(HumanAI humanAI, float woundedDuration, float woundedRecoveryChance);
    public override float GetWeight();
    public override void StateEnter();
    public override void StateLeave();
    public override StateStatus StateThink(float delta);
}

public class ChaseState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    private readonly float chaseRange;
    public ChaseState(HumanAI humanAI, float chaseRange);
    public override float GetWeight();
    public override void StateEnter();
    public override StateStatus StateThink(float delta);
}

public class CombatState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    private readonly float chaseRange;
    private float nextStrafeTime;
    public CombatState(HumanAI humanAI, float chaseRange);
    public override float GetWeight();
    public override void StateEnter();
    public override void StateLeave();
    public override StateStatus StateThink(float delta);
    private void DoMeleeAttack();
    private void DoMeleeDamage(BaseMelee baseMelee);
}

public class RoamState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    private readonly float roamRange;
    private float nextSetDestinationTime;
    private float currentDestinationTime;
    private bool isAtDestination;
    private Vector3 roamDestination;
    public RoamState(HumanAI humanAI, float roamRange);
    public override float GetWeight();
    public override void StateEnter();
    public override void StateLeave();
    public override StateStatus StateThink(float delta);
}

public class MountedState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    private readonly bool canUseWeapon;
    public MountedState(HumanAI humanAI, bool canUseWeapon);
    public override float GetWeight();
    public override void StateEnter();
    public override void StateLeave();
}

public class MoveDestinationState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    private Vector3 destination;
    private global::HumanNPC.SpeedType speed;
    private Action onReachedDestination;
    private bool hasDestination;
    public MoveDestinationState(HumanAI humanAI);
    public void SetDestination(Vector3 destination, global::HumanNPC.SpeedType speed, Action onReachedDestination);
    public override float GetWeight();
    public override void StateEnter();
    public override void StateLeave();
    public override StateStatus StateThink(float delta);
}

public class FallingState : BaseAIBrain<globalHumanNPC>.BasicAIState
{
    private readonly HumanAI humanAI;
    public FallingState(HumanAI humanAI);
    public override float GetWeight();
    public override void StateEnter();
    public override void StateLeave();
}

private class ConfigData
{
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## CustomRespawn

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
private class PlayerPerk : MonoBehaviour
{
    [JsonProperty("Отображаемое имя перка")]
    public string DisplayName;
    [JsonProperty("Ссылка на изображение")]
    public string PictureURL;
    [JsonProperty("Описание")]
    public string Description;
    [JsonProperty("Цена данного перка")]
    public int Price;
}

private class HealPerk : PlayerPerk
{
    private void Awake();
}

private class KillPerk : PlayerPerk
{
    private void Awake();
}

private class CustomItem
{
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("Короткое имя предмета")]
    public string ShortName;
    [JsonProperty("Номер скина (ID)")]
    public ulong SkinID;
    [JsonProperty("Количество предмета")]
    public int Amount;
    [JsonProperty("Доп. изображение предмета")]
    public string PictureURL;
    [JsonProperty("Разрешить только по пермишену")]
    public string Permission;
    [JsonProperty("Дополнительные предметы")]
    public Dictionary<string, int> AdditionalItems;
    [JsonProperty("Цена предмета")]
    public int Price;
    public void CreateItem(BasePlayer player);
}

private class PlayerPack
{
    [JsonProperty("Список предметов игрока")]
    public Dictionary<ItemType, CustomItem> CustomItems;
    [JsonProperty("Улучшения которые можно будет докупать")]
    public List<PlayerPerk> PerkList;
    public bool AddPerk(PlayerPerk addPerk);
    public int BuyPrice();
    public bool IsDefault();
}

private class Configuration
{
    internal class Design
    {
        [JsonProperty("Ссылка на изображение заднего плана (1920х1080)")]
        public string CONF_BackgroundImage;
        [JsonProperty("Ссылка на изображение человека (270х686)")]
        public string CONF_ManImage;
    }

    internal class Balance
    {
        [JsonProperty("Использовать баланс от Server Rewards")]
        public bool CONF_UseSR;
        [JsonProperty("Использовать баланс от Economics")]
        public bool CONF_UseEconomics;
        [JsonProperty("Использовать баланс от RustShop")]
        public bool CONF_UseRustShop;
        [JsonProperty("Использовать баланс от NShop")]
        public bool CONF_UseNShop;
        [JsonProperty("Использовать внутренний баланс (сихронизация с донат-магазинами)")]
        public bool CONF_UseCustom;
        [JsonProperty("Стартовый баланс при использовании внутреннего баланса")]
        public int CONF_CustomStartBalance;
    }

    internal class Plugin
    {
        [JsonProperty("Показывать меню выбора снаряжения для игроков с привилегией")]
        public string CONF_ShowPermission;
        [JsonProperty("Выдавать помимо закупленных предметов - автоКит")]
        public bool CONF_GiveAutoKit;
        [JsonProperty("Настройки отображения информации об убийце")]
        public KillInfo CONF_KillInfo;
    }

    internal class KillInfo
    {
        internal class Button
        {
            [JsonProperty("Отображаемое имя")]
            public string DisplayName;
            [JsonProperty("Команда")]
            public string Command;
            [JsonProperty("Цвет кнопки")]
            public string Color;
        }

        [JsonProperty("Показывать информацию об убийце")]
        public bool CONF_ShowKillerInfo;
        [JsonProperty("Кнопки под информацией")]
        public List<Button> CONF_KillerInfoButton;
    }

    [JsonProperty("Настройки дизайна плагина", Order = 0)]
    public Design DesignSetting;
    [JsonProperty("Настройки баланса игроков", Order = 1)]
    public Balance BalanceSettings;
    [JsonProperty("Настройки плагина", Order = 2)]
    public Plugin PluginSettings;
    [JsonProperty("Предметы доступные для покупки", Order = 3)]
    public Dictionary<ItemType, Dictionary<string, CustomItem>> ListOfItems;
    public PlayerPack GetDefaultPack();
    public static Configuration GetNewCong();
}

internal class Design
{
    [JsonProperty("Ссылка на изображение заднего плана (1920х1080)")]
    public string CONF_BackgroundImage;
    [JsonProperty("Ссылка на изображение человека (270х686)")]
    public string CONF_ManImage;
}

internal class Balance
{
    [JsonProperty("Использовать баланс от Server Rewards")]
    public bool CONF_UseSR;
    [JsonProperty("Использовать баланс от Economics")]
    public bool CONF_UseEconomics;
    [JsonProperty("Использовать баланс от RustShop")]
    public bool CONF_UseRustShop;
    [JsonProperty("Использовать баланс от NShop")]
    public bool CONF_UseNShop;
    [JsonProperty("Использовать внутренний баланс (сихронизация с донат-магазинами)")]
    public bool CONF_UseCustom;
    [JsonProperty("Стартовый баланс при использовании внутреннего баланса")]
    public int CONF_CustomStartBalance;
}

internal class Plugin
{
    [JsonProperty("Показывать меню выбора снаряжения для игроков с привилегией")]
    public string CONF_ShowPermission;
    [JsonProperty("Выдавать помимо закупленных предметов - автоКит")]
    public bool CONF_GiveAutoKit;
    [JsonProperty("Настройки отображения информации об убийце")]
    public KillInfo CONF_KillInfo;
}

internal class KillInfo
{
    internal class Button
    {
        [JsonProperty("Отображаемое имя")]
        public string DisplayName;
        [JsonProperty("Команда")]
        public string Command;
        [JsonProperty("Цвет кнопки")]
        public string Color;
    }

    [JsonProperty("Показывать информацию об убийце")]
    public bool CONF_ShowKillerInfo;
    [JsonProperty("Кнопки под информацией")]
    public List<Button> CONF_KillerInfoButton;
}

internal class Button
{
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("Команда")]
    public string Command;
    [JsonProperty("Цвет кнопки")]
    public string Color;
}


```

---

## CustomSpawnPoints

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("CustomSpawnPoints", "Reneb", "1.0.4", ResourceId = 1076)]
public class CustomSpawnPoints : RustPlugin
{
    [PluginReference]
     Plugin Spawns;
     bool activated;
     BasePlayer.SpawnPoint OnFindSpawnPoint();
    private static string spawnsname;
    static string MessagesPermissionsNotAllowed;
    static string CheckUp;
    static string CheckDown;
     Vector3 vectorUp;
     float checkDown;
     void LoadDefaultConfig();
    private void CheckCfg(string Key, T var);
     void Init();
     void OnServerInitialized();
     void LoadSpawns();
     bool hasAccess(ConsoleSystem.Arg arg);
    [ConsoleCommand("spawns.config")]
     void ccmdSpawnFile(ConsoleSystem.Arg arg);
}


```

---

## CustomTurret

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("CustomTurret", "BadMandarin", "1.0.0")]
[Description("CustomTurret")]
 class CustomTurret : RustPlugin
{
    private class PluginConfig
    {
        [JsonProperty("Сколько берёт патронов за спрей")]
        public int TakeAmmo;
        [JsonProperty("Дистанция наводки")]
        public float Distance;
        [JsonProperty("Здоровье турели (стандарт 1000)")]
        public float Health;
        [JsonProperty("Прицеливание (стандарт 4 обычная турель) чем меньше тем лучше")]
        public float AimCone;
        [JsonProperty("Множитель урона (стандарт 1f)")]
        public float DamageScale;
        [JsonProperty("Ресурсы чтобы скрафтить")]
        public Dictionary<string, int> ResourcesToCraft;
    }

     ulong skinid;
     int ammoid;
     string turretPrefab;
    private PluginConfig config;
    private static CustomTurret plugin;
     string permSet;
     string permCraft;
    [PluginReference]
     Plugin ImageLibrary;
     string sentryimage;
    private List<uint> customTurrets;
    private void Init();
    private void OnServerInitialized();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     object OnHammerHit(BasePlayer player, HitInfo info);
    private void TurretRepair(BasePlayer player, NPCAutoTurret turret);
     void OnEntityBuilt(Planner planner, GameObject gameobject);
    private void SetupProtection(BaseCombatEntity turret);
     object OnTurretTarget(AutoTurret turret, BaseCombatEntity entity);
     bool CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     object CanStackItem(Item item, Item targetItem);
     object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
     string UI_Layer;
    private void DrawUi(BasePlayer player);
    [ChatCommand("sentry")]
     void Chat_ShowTop(BasePlayer player, string command, string[] args);
    [ConsoleCommand("sentry.craft")]
    private void SentryCraft(ConsoleSystem.Arg arg);
    [ConsoleCommand("sentry.add")]
    private void AddSentry(ConsoleSystem.Arg arg);
    private string GetNameByURL(string url);
    private static string GetColor(string hex, float alpha);
    private void GiveSentry(BasePlayer player);
    private void GiveSentry(Vector3 position);
    private void CheckSentry();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private class ExtendedSentryComponent : MonoBehaviour
    {
        private NPCAutoTurret sentry;
        private void Awake();
        private void CheckGround();
        private void GroundMissing();
        public void DoDestroy();
    }

}

private class PluginConfig
{
    [JsonProperty("Сколько берёт патронов за спрей")]
    public int TakeAmmo;
    [JsonProperty("Дистанция наводки")]
    public float Distance;
    [JsonProperty("Здоровье турели (стандарт 1000)")]
    public float Health;
    [JsonProperty("Прицеливание (стандарт 4 обычная турель) чем меньше тем лучше")]
    public float AimCone;
    [JsonProperty("Множитель урона (стандарт 1f)")]
    public float DamageScale;
    [JsonProperty("Ресурсы чтобы скрафтить")]
    public Dictionary<string, int> ResourcesToCraft;
}

private class ExtendedSentryComponent : MonoBehaviour
{
    private NPCAutoTurret sentry;
    private void Awake();
    private void CheckGround();
    private void GroundMissing();
    public void DoDestroy();
}


```

---

## CustomVendingSetup

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Runtime.Serialization;
using Facepunch;
using UnityEngine;
using VLB;
using static ProtoBuf.VendingMachine;
using static VendingMachine;
using CustomGetDataCallback = System.Func<Newtonsoft.Json.Linq.JObject>;
using CustomSaveDataCallback = System.Action<Newtonsoft.Json.Linq.JObject>;

Oxide.Plugins
[Info("Custom Vending Setup", "WhiteThunder", "2.10.3")]
[Description("Allows editing orders at NPC vending machines.")]
internal class CustomVendingSetup : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin BagOfHolding;
    private readonly Plugin Economics;
    private readonly Plugin ItemRetriever;
    private readonly Plugin MonumentFinder;
    private readonly Plugin ServerRewards;
    private SavedData _pluginData;
    private Configuration _config;
    private const string PermissionUse;
    private const string StoragePrefab;
    private const int ItemsPerRow;
    private const int InventorySize;
    private const int MaxVendingOffers;
    private const int ShopNameNoteSlot;
    private const int ContainerCapacity;
    private const int MaxItemRows;
    private const int BlueprintItemId;
    private const float MinCurrencyCondition;
    private readonly object True;
    private readonly object False;
    private ItemRetrieverAdapter _itemRetrieverAdapter;
    private DataProviderRegistry _dataProviderRegistry;
    private ComponentTracker<NPCVendingMachine, VendingMachineComponent> _componentTracker;
    private ComponentFactory<NPCVendingMachine, VendingMachineComponent> _componentFactory;
    private MonumentFinderAdapter _monumentFinderAdapter;
    private VendingMachineManager _vendingMachineManager;
    private BagOfHoldingLimitManager _bagOfHoldingLimitManager;
    private DynamicHookSubscriber<BaseVendingController> _inaccessibleVendingMachines;
    private DynamicHookSubscriber<BasePlayer> _playersNeedingFakeInventory;
    private PaymentProviderResolver _paymentProviderResolver;
    private ItemDefinition _noteItemDefinition;
    private bool _isServerInitialized;
    private bool _performingInstantRestock;
    private VendingItem _itemBeingSold;
    private Dictionary<string, object> _itemRetrieverQuery;
    private List<Item> _reusableItemList;
    private object[] _objectArray1;
    private object[] _objectArray2;
    public CustomVendingSetup();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnEntitySpawned(NPCVendingMachine vendingMachine);
    private void OnEntityKill(NPCVendingMachine vendingMachine);
    private void OnVendingShopOpened(NPCVendingMachine vendingMachine, BasePlayer player);
    private object OnVendingTransaction(NPCVendingMachine vendingMachine, BasePlayer player, int sellOrderIndex, int numberOfTransactions, ItemContainer targetContainer);
    private void OnBuyVendingItem(NPCVendingMachine vendingMachine, BasePlayer player, int sellOrderID, int amount);
    private object OnNpcGiveSoldItem(NPCVendingMachine vendingMachine, Item item, BasePlayer player);
    private object CanVendingStockRefill(NPCVendingMachine vendingMachine, Item soldItem, BasePlayer player);
    private object CanAccessVendingMachine(DeliveryDroneConfig deliveryDroneConfig, NPCVendingMachine vendingMachine);
    private void OnEntitySaved(BasePlayer player, BaseNetworkable.SaveInfo saveInfo);
    private void OnInventoryNetworkUpdate(PlayerInventory inventory, ItemContainer container, ProtoBuf.UpdateItemContainer updatedItemContainer, PlayerInventory.Type inventoryType);
    private bool API_IsCustomized(NPCVendingMachine vendingMachine);
    private void API_RefreshDataProvider(NPCVendingMachine vendingMachine);
    private JObject API_MigrateVendingProfile(NPCVendingMachine vendingMachine);
    private class MonumentAdapter
    {
        public string PrefabName { get; set; }
        public string Alias { get; set; }
        public Vector3 Position { get; set; }
        private Dictionary<string, object> _monumentInfo;
        public MonumentAdapter(Dictionary<string, object> monumentInfo);
        public Vector3 InverseTransformPoint(Vector3 worldPosition);
        public bool IsInBounds(Vector3 position);
    }

    private class MonumentFinderAdapter
    {
        private CustomVendingSetup _plugin;
        private Plugin _monumentFinder { get; set; }
        public MonumentFinderAdapter(CustomVendingSetup plugin);
        public MonumentAdapter GetMonumentAdapter(Vector3 position);
        public MonumentAdapter GetMonumentAdapter(BaseEntity entity);
    }

    private class BagOfHoldingLimitManager
    {
        private class CustomLimitProfile
        {
            [JsonProperty("Max total bags")]
            public int MaxTotalBags;
        }

        private CustomVendingSetup _plugin;
        private object _limitProfile;
        public BagOfHoldingLimitManager(CustomVendingSetup plugin);
        public void OnServerInitialized();
        public void HandleBagOfHoldingLoadedChanged();
        public void SetLimitProfile(ItemContainer container);
        public void RemoveLimitProfile(ItemContainer container);
    }

    private class ItemRetrieverApi
    {
        public Func<BasePlayer, Dictionary<string, object>, int> SumPlayerItems { get; set; }
        public Func<BasePlayer, Dictionary<string, object>, int, List<Item>, int> TakePlayerItems { get; set; }
        public ItemRetrieverApi(Dictionary<string, object> apiDict);
    }

    private class ItemRetrieverAdapter
    {
        public ItemRetrieverApi Api { get; set; }
        private CustomVendingSetup _plugin;
        private Plugin ItemRetriever { get; set; }
        public ItemRetrieverAdapter(CustomVendingSetup plugin);
        public void HandleItemRetrieverLoaded();
        public void HandleItemRetrieverUnloaded();
    }

    private static class ExposedHooks
    {
        public static object OnCustomVendingSetup(NPCVendingMachine vendingMachine);
        public static object CanPurchaseItem(BasePlayer player, Item item, Action<BasePlayer, Item> onItemPurchased, NPCVendingMachine vendingMachine, ItemContainer targetContainer);
        public static Dictionary<string, object> OnCustomVendingSetupDataProvider(NPCVendingMachine vendingMachine);
        public static void OnCustomVendingSetupOfferSettingsParse(CaseInsensitiveDictionary<string> localizedSettings, CaseInsensitiveDictionary<object> customSettings);
        public static void OnCustomVendingSetupOfferSettingsDisplay(CaseInsensitiveDictionary<object> customSettings, CaseInsensitiveDictionary<string> localizedSettings);
        public static void OnCustomVendingSetupTransactionWithCustomSettings(NPCVendingMachine vendingMachine, CaseInsensitiveDictionary<object> customSettings);
    }

    private static class UICommands
    {
        public const string Edit;
        public const string Reset;
        public const string Save;
        public const string Cancel;
        public const string ToggleBroadcast;
        public const string ToggleDroneAccessible;
    }

    [Command("customvendingsetup.ui")]
    private void CommandUI(IPlayer player, string cmd, string[] args);
    public static void LogInfo(string message);
    public static void LogError(string message);
    public static void LogWarning(string message);
    private static bool IsLootingVendingMachine(BasePlayer player, NPCVendingMachine vendingMachine);
    private static bool AreVectorsClose(Vector3 a, Vector3 b, float xZTolerance, float yTolerance);
    private static bool HasCondition(ItemDefinition itemDefinition);
    private static void OpenVendingMachine(BasePlayer player, NPCVendingMachine vendingMachine);
    private static VendingOffer[] GetOffersFromVendingMachine(NPCVendingMachine vendingMachine);
    private static VendingOffer[] GetOffersFromContainer(CustomVendingSetup plugin, BasePlayer player, ItemContainer container);
    private static StorageContainer CreateContainerEntity(string prefabPath);
    private static int OrderIndexToSlot(int orderIndex);
    private static string CreateNoteContents(Dictionary<string, string> settingsMap);
    private static StorageContainer CreateOrdersContainer(CustomVendingSetup plugin, BasePlayer player, VendingOffer[] vendingOffers, string shopName);
    private static void MaybeGiveWeaponAmmo(Item item, BasePlayer player);
    private static void GiveSoldItem(Item item, BasePlayer player, TransactionContext transaction);
    private static int GetHighestUsedSlot(ProtoBuf.ItemContainer containerData);
    private static void AddItemForNetwork(ProtoBuf.ItemContainer containerData, int slot, int itemId, int amount, ItemId uid);
    private object CallPlugin(Plugin plugin, string methodName, T1 arg1);
    private object CallPlugin(Plugin plugin, string methodName, T1 arg1, T2 arg2);
    private void ScheduleRemoveUI(NPCVendingMachine vendingMachine, BasePlayer player, VendingMachineComponent component);
    private void AddCurrencyToContainerSnapshot(BasePlayer player, ProtoBuf.ItemContainer containerData);
    private Dictionary<string, object> SetupItemRetrieverQuery(ItemQuery itemQuery);
    private int SumPlayerItems(BasePlayer player, ItemQuery itemQuery);
    private int TakePlayerItems(BasePlayer player, ItemQuery itemQuery, int amount, List<Item> collect);
    private bool PassesUICommandChecks(IPlayer player, string[] args, NPCVendingMachine vendingMachine, BaseVendingController controller);
    private void OpenVendingMachineDelayed(BasePlayer player, NPCVendingMachine vendingMachine, float delay);
    private bool IsCustomized(NPCVendingMachine vendingMachine);
    private static class UIConstants
    {
        public const string EditButtonColor;
        public const string EditButtonTextColor;
        public const string ResetButtonColor;
        public const string ResetButtonTextColor;
        public const string SaveButtonColor;
        public const string SaveButtonTextColor;
        public const string CancelButtonColor;
        public const string CancelButtonTextColor;
        public const float PanelWidth;
        public const float HeaderHeight;
        public const float ItemSpacing;
        public const float ItemBoxSize;
        public const int ButtonHorizontalSpacing;
        public const int ButtonHeight;
        public const int ButtonWidth;
        public const string TexturedBackgroundSprite;
        public const string BroadcastIcon;
        public const string DroneIcon;
        public const string IconMaterial;
        public const string GreyOutMaterial;
        public const string AnchorMin;
        public const string AnchorMax;
    }

    private class EditFormState
    {
        public static EditFormState FromVendingMachine(BaseVendingController vendingController, NPCVendingMachine vendingMachine);
        public bool Broadcast;
        public bool DroneAccessible;
    }

    private static class ContainerUIRenderer
    {
        public const string UIName;
        public const string TipUIName;
        public const string BroadcastUIName;
        public const string DroneUIName;
        public static string RenderContainerUI(CustomVendingSetup plugin, BasePlayer player, NPCVendingMachine vendingMachine, EditFormState uiState);
        private static void AddHeaderLabel(CuiElementContainer cuiElements, int index, string text);
        private static void AddBroadcastButton(CuiElementContainer cuiElements, NPCVendingMachine vendingMachine, EditFormState uiState);
        private static void AddDroneButton(CuiElementContainer cuiElements, NPCVendingMachine vendingMachine, EditFormState uiState);
        public static string RenderBroadcastUI(NPCVendingMachine vendingMachine, EditFormState uiState);
        private static void AddButton(CuiElementContainer cuiElements, ulong vendingMachineId, string text, string subCommand, float xMax, string color, string textColor);
    }

    private static class AdminUIRenderer
    {
        public const string UIName;
        public static string RenderAdminUI(CustomVendingSetup plugin, BasePlayer player, NPCVendingMachine vendingMachine, VendingProfile profile);
        private static float GetButtonOffset(int reverseButtonIndex);
        private static void AddVendingButton(CuiElementContainer cuiElements, ulong vendingMachineId, string text, string subCommand, int reverseButtonIndex, string color, string textColor);
    }

    private static class ShopUIRenderer
    {
        public const string UIName;
        private const float OffsetXItem;
        private const float OffsetXCurrency;
        private const float OverlaySize;
        private const float IconSize;
        private const float PaddingLeft;
        private const float PaddingBottom;
        public static string RenderShopUI(VendingProfile vendingProfile);
        private static void AddItemOverlay(CuiElementContainer cuiElements, int indexFromBottom, VendingOffer offer, bool isCurrency);
    }

    private static class StringUtils
    {
        public static bool Equals(string a, string b);
        public static bool Contains(string haystack, string needle);
    }

    private static class ObjectCache
    {
        private static class StaticObjectCache
        {
            private static readonly Dictionary<T, object> _cacheByValue;
            public static object Get(T value);
            public static void Clear();
        }

        public static object Get(T value);
        public static void Clear();
    }

    private static bool LocationsMatch(IMonumentRelativePosition a, IMonumentRelativePosition b);
    private class MonumentRelativePosition : IMonumentRelativePosition
    {
        public static MonumentRelativePosition FromVendingMachine(MonumentFinderAdapter monumentFinderAdapter, NPCVendingMachine vendingMachine);
        private MonumentAdapter _monument;
        private Vector3 _position;
        private Vector3 _legacyPosition;
        public string GetMonumentPrefabName();
        public string GetMonumentAlias();
        public Vector3 GetPosition();
        public Vector3 GetLegacyPosition();
    }

    private class ItemsPaymentProvider : IPaymentProvider
    {
        public VendingItem VendingItem;
        private CustomVendingSetup _plugin;
        public ItemsPaymentProvider(CustomVendingSetup plugin);
        public int GetBalance(BasePlayer player);
        public bool AddBalance(BasePlayer player, int amount, TransactionContext transaction);
        public bool TakeBalance(BasePlayer player, int amount, List<Item> collect);
    }

    private class EconomicsPaymentProvider : IPaymentProvider
    {
        private CustomVendingSetup _plugin;
        private Plugin _ownerPlugin { get; set; }
        public EconomicsPaymentProvider(CustomVendingSetup plugin);
        public bool IsAvailable { get; set; }
        public int GetBalance(BasePlayer player);
        public bool AddBalance(BasePlayer player, int amount, TransactionContext transaction);
        public bool TakeBalance(BasePlayer player, int amount, List<Item> collect);
    }

    private class ServerRewardsPaymentProvider : IPaymentProvider
    {
        private CustomVendingSetup _plugin;
        private Plugin _ownerPlugin { get; set; }
        public ServerRewardsPaymentProvider(CustomVendingSetup plugin);
        public bool IsAvailable { get; set; }
        public int GetBalance(BasePlayer player);
        public bool AddBalance(BasePlayer player, int amount, TransactionContext transaction);
        public bool TakeBalance(BasePlayer player, int amount, List<Item> collect);
    }

    private class PaymentProviderResolver
    {
        public readonly EconomicsPaymentProvider EconomicsPaymentProvider;
        public readonly ServerRewardsPaymentProvider ServerRewardsPaymentProvider;
        private readonly CustomVendingSetup _plugin;
        private readonly ItemsPaymentProvider _itemsPaymentProvider;
        private Configuration _config { get; set; }
        public PaymentProviderResolver(CustomVendingSetup plugin);
        public IPaymentProvider Resolve(VendingItem vendingItem);
    }

    private static class ItemUtils
    {
        public static Item FindFirstContainerItem(ItemContainer container, ItemQuery itemQuery);
        public static int SumContainerItems(ItemContainer container, ItemQuery itemQuery);
        public static int SumPlayerItems(BasePlayer player, ItemQuery itemQuery);
        public static int TakeContainerItems(ItemContainer container, ItemQuery itemQuery, int totalAmountToTake, List<Item> collect);
        public static int TakePlayerItems(BasePlayer player, ItemQuery itemQuery, int amountToTake, List<Item> collect);
    }

    private class DynamicHookSubscriber
    {
        private CustomVendingSetup _plugin;
        private HashSet<T> _list;
        private string[] _hookNames;
        public DynamicHookSubscriber(CustomVendingSetup plugin, string[] hookNames);
        public bool Contains(T item);
        public void Add(T item);
        public void Remove(T item);
        public void SubscribeAll();
        public void UnsubscribeAll();
    }

    private class DataProvider
    {
        public static DataProvider FromDictionary(Dictionary<string, object> spec);
        public Dictionary<string, object> Spec { get; set; }
        public CustomGetDataCallback GetDataCallback;
        public CustomSaveDataCallback SaveDataCallback;
        private VendingProfile _vendingProfile;
        private JObject _serializedData;
        public VendingProfile GetData(Configuration config);
        public void SaveData(VendingProfile vendingProfile);
    }

    private class DataProviderRegistry
    {
        private Dictionary<Dictionary<string, object>, DataProvider> _dataProviderCache;
        public DataProvider Register(Dictionary<string, object> dataProviderSpec);
        public void Unregister(DataProvider dataProvider);
    }

    private class VendingMachineManager
    {
        private CustomVendingSetup _plugin;
        private ComponentFactory<NPCVendingMachine, VendingMachineComponent> _componentFactory;
        private DataProviderRegistry _dataProviderRegistry;
        private HashSet<BaseVendingController> _uniqueControllers;
        private Dictionary<NetworkableId, BaseVendingController> _controllersByVendingMachine;
        private Dictionary<DataProvider, CustomVendingController> _controllersByDataProvider;
        public VendingMachineManager(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory, DataProviderRegistry dataProviderRegistry);
        public void HandleVendingMachineSpawned(NPCVendingMachine vendingMachine);
        public void HandleVendingMachineKilled(NPCVendingMachine vendingMachine);
        public BaseVendingController GetController(NPCVendingMachine vendingMachine);
        public void SetupAll();
        public void ResetAll();
        private MonumentVendingController GetControllerByLocation(MonumentRelativePosition location);
        private MonumentVendingController EnsureMonumentController(MonumentRelativePosition location);
        private CustomVendingController GetControllerByDataProvider(DataProvider dataProvider);
        private CustomVendingController EnsureCustomController(DataProvider dataProvider);
    }

    private class EditContainerComponent : FacepunchBehaviour
    {
        public static void AddToContainer(CustomVendingSetup plugin, StorageContainer container, EditController editController);
        private CustomVendingSetup _plugin;
        private EditController _editController;
        private void PlayerStoppedLooting(BasePlayer player);
    }

    private class EditController
    {
        private static void OpenEditPanel(BasePlayer player, StorageContainer containerEntity);
        public BasePlayer EditorPlayer { get; set; }
        private CustomVendingSetup _plugin;
        private BaseVendingController _vendingController;
        private NPCVendingMachine _vendingMachine;
        private StorageContainer _container;
        private EditFormState _formState;
        public EditController(CustomVendingSetup plugin, BaseVendingController vendingController, NPCVendingMachine vendingMachine, BasePlayer editorPlayer);
        public void ToggleBroadcast();
        public void ToggleDroneAccessible();
        public void ApplyStateTo(VendingProfile profile);
        public void HandlePlayerLootEnd(BasePlayer player);
        public void Kill();
        private void DestroyUI();
        private void KillContainer();
    }

    private abstract class BaseVendingController
    {
        public VendingProfile Profile { get; set; }
        public EditController EditController { get; set; }
        public bool HasVendingMachines { get; set; }
        protected CustomVendingSetup _plugin;
        private HashSet<NPCVendingMachine> _vendingMachineList;
        private ComponentFactory<NPCVendingMachine, VendingMachineComponent> _componentFactory;
        private string _cachedShopUI;
        protected BaseVendingController(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory);
        protected abstract void SaveProfile(VendingProfile vendingProfile);
        protected abstract void DeleteProfile(VendingProfile vendingProfile);
        public void StartEditing(BasePlayer player, NPCVendingMachine vendingMachine);
        public void HandleReset();
        public void Destroy();
        public void HandleSave(NPCVendingMachine vendingMachine);
        public void AddVendingMachine(NPCVendingMachine vendingMachine);
        public void RemoveVendingMachine(NPCVendingMachine vendingMachine);
        public void OnEditControllerKilled();
        public string GetShopUI();
        public void UpdateDroneAccessibility();
        protected virtual void CreateOrUpdateProfile(NPCVendingMachine vendingMachine);
        private void SetupVendingMachines();
        private void ResetVendingMachines();
    }

    private class CustomVendingController : BaseVendingController
    {
        public DataProvider DataProvider { get; set; }
        public CustomVendingController(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory, DataProvider dataProvider);
        protected override void SaveProfile(VendingProfile vendingProfile);
        protected override void DeleteProfile(VendingProfile vendingProfile);
    }

    private class MonumentVendingController : BaseVendingController
    {
        public MonumentRelativePosition Location { get; set; }
        private SavedData _pluginData { get; set; }
        public MonumentVendingController(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory, MonumentRelativePosition location);
        protected override void SaveProfile(VendingProfile vendingProfile);
        protected override void DeleteProfile(VendingProfile vendingProfile);
        protected override void CreateOrUpdateProfile(NPCVendingMachine vendingMachine);
    }

    private class ComponentTracker
    {
        private readonly Dictionary<THost, TGuest> _hostToGuest;
        public void RegisterComponent(THost host, TGuest guest);
        public TGuest GetComponent(THost host);
        public void UnregisterComponent(THost source);
    }

    private class TrackedComponent : FacepunchBehaviour
    {
        public CustomVendingSetup Plugin;
        public ComponentTracker<THost, TGuest> ComponentTracker;
        public THost Host;
        public virtual void OnCreated();
        protected virtual void OnDestroy();
    }

    private class ComponentFactory
    {
        private CustomVendingSetup _plugin;
        private ComponentTracker<THost, TGuest> _componentTracker;
        public ComponentFactory(CustomVendingSetup plugin, ComponentTracker<THost, TGuest> componentTracker);
        public TGuest GetOrAddTo(THost host);
    }

    private class VendingMachineComponent : TrackedComponent<NPCVendingMachine, VendingMachineComponent>
    {
        public static void RemoveFromVendingMachine(NPCVendingMachine vendingMachine);
        public VendingProfile Profile { get; set; }
        private readonly List<BasePlayer> _adminUIViewers;
        private readonly List<BasePlayer> _shopUIViewers;
        private BaseVendingController _vendingController;
        private NPCVendingMachine _vendingMachine;
        private float[] _refillTimes;
        private string _originalShopName;
        private bool? _originalBroadcast;
        public override void OnCreated();
        public bool HasUI(BasePlayer player);
        public void ShowAdminUI(BasePlayer player);
        public void ShowShopUI(BasePlayer player);
        public void RemoveUI(BasePlayer player);
        protected override void OnDestroy();
        private void PlayerStoppedLooting(BasePlayer player);
        public void SetController(BaseVendingController vendingController);
        public void SetProfile(VendingProfile profile);
        private void ScheduleRefill(int offerIndex, VendingOffer offer, int min);
        private void ScheduleDelayedRefill(int offerIndex, VendingOffer offer);
        private void StopRefilling(int offerIndex);
        private void CustomRefill(bool maxRefill);
        private void TimedRefill();
        private void DestroyAdminUI(BasePlayer player);
        private void DestroyShopUI(BasePlayer player);
        private void DestroyUIs();
        private void DisableVanillaBehavior();
        private void ResetToVanilla();
    }

    private class LegacyVendingItem
    {
        public string Shortname;
        public string DisplayName;
        public int Amount;
        public ulong Skin;
        public bool IsBlueprint;
    }

    private class LegacyVendingOffer
    {
        public LegacyVendingItem Currency;
        public LegacyVendingItem SellItem;
    }

    private class LegacyVendingProfile
    {
        public string Id;
        public List<LegacyVendingOffer> Offers;
        public string Shortname;
        public Vector3 WorldPosition;
        public Vector3 RelativePosition;
        public string RelativeMonument;
        public bool DetectByShortname;
    }

    private class CaseInsensitiveDictionary : Dictionary<string, TValue>
    {
        public CaseInsensitiveDictionary();
        public CaseInsensitiveDictionary(Dictionary<string, TValue> dict);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class VendingItem
    {
        public static VendingItem FromItem(Item item);
        private static List<VendingItem> SerializeContents(List<Item> itemList);
        private static int GetAmmoAmountAndType(Item item, ItemDefinition ammoType);
        [JsonProperty("ShortName")]
        public string ShortName;
        [JsonProperty("DisplayName", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string DisplayName;
        [JsonProperty("Amount")]
        public int Amount;
        [JsonProperty("Skin", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong SkinId;
        [JsonProperty("IsBlueprint", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool IsBlueprint;
        [JsonProperty("DataInt", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int DataInt;
        [JsonProperty("Position", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int Position;
        [JsonProperty("Ammo", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(-1)]
        public int AmmoAmount;
        [JsonProperty("AmmoType", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string AmmoType;
        [JsonProperty("Capacity", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int Capacity;
        [JsonProperty("Contents", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public List<VendingItem> Contents;
        private ItemDefinition _itemDefinition;
        public ItemDefinition ItemDefinition { get; set; }
        private ItemDefinition _ammoTypeDefinition;
        public ItemDefinition AmmoTypeDefinition { get; set; }
        public bool IsValid { get; set; }
        public int ItemId { get; set; }
        public Item Create(int amount);
        public Item Create();
        public VendingItem Copy();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class VendingOffer
    {
        public const int DefaultRefillMax;
        public const int DefaultRefillDelay;
        public const int DefaultRefillAmount;
        public static VendingOffer FromVanillaSellOrder(SellOrder sellOrder, NPCVendingOrder.Entry manifestEntry);
        public static VendingOffer FromItems(CustomVendingSetup plugin, BasePlayer player, Item sellItem, Item currencyItem, Item settingsItem);
        private static CaseInsensitiveDictionary<string> ParseSettingsItem(Item settingsItem);
        private static bool TryParseIntKey(Dictionary<string, string> dict, string key, int result);
        [JsonProperty("SellItem")]
        public VendingItem SellItem;
        [JsonProperty("CurrencyItem")]
        public VendingItem CurrencyItem;
        [JsonProperty("RefillMax", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(DefaultRefillMax)]
        public int RefillMax;
        [JsonProperty("RefillDelay", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(DefaultRefillDelay)]
        public int RefillDelay;
        [JsonProperty("RefillAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(DefaultRefillAmount)]
        public int RefillAmount;
        [JsonProperty("CustomSettings", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public CaseInsensitiveDictionary<object> CustomSettings;
        public bool IsValid { get; set; }
        public VendingOffer Copy();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class VendingProfile : IMonumentRelativePosition
    {
        public static VendingProfile FromVendingMachine(NPCVendingMachine vendingMachine, MonumentRelativePosition location);
        [JsonProperty("ShopName", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string ShopName;
        [JsonProperty("Broadcast", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(true)]
        public bool Broadcast;
        [JsonProperty("DroneAccessible", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(true)]
        public bool DroneAccessible;
        [JsonProperty("Monument", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Monument;
        [JsonProperty("MonumentAlias", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string MonumentAlias;
        [JsonProperty("Position", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Vector3 Position;
        [JsonProperty("LegacyPosition", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Vector3 LegacyPosition;
        [JsonProperty("Offers")]
        public VendingOffer[] Offers;
        public VendingOffer GetOfferForSellOrderIndex(int index);
        public bool HasPaymentProviderCurrency(PaymentProviderConfig paymentProviderConfig);
        public string GetMonumentPrefabName();
        public string GetMonumentAlias();
        public Vector3 GetPosition();
        public Vector3 GetLegacyPosition();
        [OnDeserialized]
        private void OnDeserialized(StreamingContext context);
        private void UpdateOldSaddleOffers();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class SavedData
    {
        [JsonProperty("Vendings", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public List<LegacyVendingProfile> Vendings;
        [JsonProperty("VendingProfiles")]
        public List<VendingProfile> VendingProfiles;
        public static SavedData Load();
        public void Save();
        public VendingProfile FindProfile(IMonumentRelativePosition location);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class ShopUISettings
    {
        [JsonProperty("Enable skin overlays")]
        public bool EnableSkinOverlays;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class PaymentProviderConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Item short name")]
        public string ItemShortName;
        [JsonProperty("Item skin ID")]
        public ulong ItemSkinId;
        public ItemDefinition ItemDefinition { get; set; }
        public bool EnabledAndValid { get; set; }
        public void Init();
        public bool MatchesItem(VendingItem vendingItem);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Shop UI settings")]
        public ShopUISettings ShopUISettings;
        [JsonProperty("Economics integration")]
        public PaymentProviderConfig Economics;
        [JsonProperty("Server Rewards integration")]
        public PaymentProviderConfig ServerRewards;
        [JsonProperty("Override item max stack sizes (shortname: amount)")]
        public Dictionary<string, int> ItemStackSizeOverrides;
        public void Init();
        public int GetItemMaxStackSize(Item item);
    }

    private Configuration GetDefaultConfig();
    private class SerializableConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private string GetMessage(string playerId, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(BasePlayer player, string messageName, object[] args);
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private static class Lang
    {
        public const string ButtonEdit;
        public const string ButtonReset;
        public const string InfoForSale;
        public const string ButtonSave;
        public const string ButtonCancel;
        public const string InfoCost;
        public const string InfoSettings;
        public const string SettingsRefillMax;
        public const string SettingsRefillDelay;
        public const string SettingsRefillAmount;
        public const string ErrorCurrentlyBeingEdited;
    }

    protected override void LoadDefaultMessages();
}

private class MonumentAdapter
{
    public string PrefabName { get; set; }
    public string Alias { get; set; }
    public Vector3 Position { get; set; }
    private Dictionary<string, object> _monumentInfo;
    public MonumentAdapter(Dictionary<string, object> monumentInfo);
    public Vector3 InverseTransformPoint(Vector3 worldPosition);
    public bool IsInBounds(Vector3 position);
}

private class MonumentFinderAdapter
{
    private CustomVendingSetup _plugin;
    private Plugin _monumentFinder { get; set; }
    public MonumentFinderAdapter(CustomVendingSetup plugin);
    public MonumentAdapter GetMonumentAdapter(Vector3 position);
    public MonumentAdapter GetMonumentAdapter(BaseEntity entity);
}

private class BagOfHoldingLimitManager
{
    private class CustomLimitProfile
    {
        [JsonProperty("Max total bags")]
        public int MaxTotalBags;
    }

    private CustomVendingSetup _plugin;
    private object _limitProfile;
    public BagOfHoldingLimitManager(CustomVendingSetup plugin);
    public void OnServerInitialized();
    public void HandleBagOfHoldingLoadedChanged();
    public void SetLimitProfile(ItemContainer container);
    public void RemoveLimitProfile(ItemContainer container);
}

private class CustomLimitProfile
{
    [JsonProperty("Max total bags")]
    public int MaxTotalBags;
}

private class ItemRetrieverApi
{
    public Func<BasePlayer, Dictionary<string, object>, int> SumPlayerItems { get; set; }
    public Func<BasePlayer, Dictionary<string, object>, int, List<Item>, int> TakePlayerItems { get; set; }
    public ItemRetrieverApi(Dictionary<string, object> apiDict);
}

private class ItemRetrieverAdapter
{
    public ItemRetrieverApi Api { get; set; }
    private CustomVendingSetup _plugin;
    private Plugin ItemRetriever { get; set; }
    public ItemRetrieverAdapter(CustomVendingSetup plugin);
    public void HandleItemRetrieverLoaded();
    public void HandleItemRetrieverUnloaded();
}

private static class ExposedHooks
{
    public static object OnCustomVendingSetup(NPCVendingMachine vendingMachine);
    public static object CanPurchaseItem(BasePlayer player, Item item, Action<BasePlayer, Item> onItemPurchased, NPCVendingMachine vendingMachine, ItemContainer targetContainer);
    public static Dictionary<string, object> OnCustomVendingSetupDataProvider(NPCVendingMachine vendingMachine);
    public static void OnCustomVendingSetupOfferSettingsParse(CaseInsensitiveDictionary<string> localizedSettings, CaseInsensitiveDictionary<object> customSettings);
    public static void OnCustomVendingSetupOfferSettingsDisplay(CaseInsensitiveDictionary<object> customSettings, CaseInsensitiveDictionary<string> localizedSettings);
    public static void OnCustomVendingSetupTransactionWithCustomSettings(NPCVendingMachine vendingMachine, CaseInsensitiveDictionary<object> customSettings);
}

private static class UICommands
{
    public const string Edit;
    public const string Reset;
    public const string Save;
    public const string Cancel;
    public const string ToggleBroadcast;
    public const string ToggleDroneAccessible;
}

private static class UIConstants
{
    public const string EditButtonColor;
    public const string EditButtonTextColor;
    public const string ResetButtonColor;
    public const string ResetButtonTextColor;
    public const string SaveButtonColor;
    public const string SaveButtonTextColor;
    public const string CancelButtonColor;
    public const string CancelButtonTextColor;
    public const float PanelWidth;
    public const float HeaderHeight;
    public const float ItemSpacing;
    public const float ItemBoxSize;
    public const int ButtonHorizontalSpacing;
    public const int ButtonHeight;
    public const int ButtonWidth;
    public const string TexturedBackgroundSprite;
    public const string BroadcastIcon;
    public const string DroneIcon;
    public const string IconMaterial;
    public const string GreyOutMaterial;
    public const string AnchorMin;
    public const string AnchorMax;
}

private class EditFormState
{
    public static EditFormState FromVendingMachine(BaseVendingController vendingController, NPCVendingMachine vendingMachine);
    public bool Broadcast;
    public bool DroneAccessible;
}

private static class ContainerUIRenderer
{
    public const string UIName;
    public const string TipUIName;
    public const string BroadcastUIName;
    public const string DroneUIName;
    public static string RenderContainerUI(CustomVendingSetup plugin, BasePlayer player, NPCVendingMachine vendingMachine, EditFormState uiState);
    private static void AddHeaderLabel(CuiElementContainer cuiElements, int index, string text);
    private static void AddBroadcastButton(CuiElementContainer cuiElements, NPCVendingMachine vendingMachine, EditFormState uiState);
    private static void AddDroneButton(CuiElementContainer cuiElements, NPCVendingMachine vendingMachine, EditFormState uiState);
    public static string RenderBroadcastUI(NPCVendingMachine vendingMachine, EditFormState uiState);
    private static void AddButton(CuiElementContainer cuiElements, ulong vendingMachineId, string text, string subCommand, float xMax, string color, string textColor);
}

private static class AdminUIRenderer
{
    public const string UIName;
    public static string RenderAdminUI(CustomVendingSetup plugin, BasePlayer player, NPCVendingMachine vendingMachine, VendingProfile profile);
    private static float GetButtonOffset(int reverseButtonIndex);
    private static void AddVendingButton(CuiElementContainer cuiElements, ulong vendingMachineId, string text, string subCommand, int reverseButtonIndex, string color, string textColor);
}

private static class ShopUIRenderer
{
    public const string UIName;
    private const float OffsetXItem;
    private const float OffsetXCurrency;
    private const float OverlaySize;
    private const float IconSize;
    private const float PaddingLeft;
    private const float PaddingBottom;
    public static string RenderShopUI(VendingProfile vendingProfile);
    private static void AddItemOverlay(CuiElementContainer cuiElements, int indexFromBottom, VendingOffer offer, bool isCurrency);
}

private static class StringUtils
{
    public static bool Equals(string a, string b);
    public static bool Contains(string haystack, string needle);
}

private static class ObjectCache
{
    private static class StaticObjectCache
    {
        private static readonly Dictionary<T, object> _cacheByValue;
        public static object Get(T value);
        public static void Clear();
    }

    public static object Get(T value);
    public static void Clear();
}

private static class StaticObjectCache
{
    private static readonly Dictionary<T, object> _cacheByValue;
    public static object Get(T value);
    public static void Clear();
}

private class MonumentRelativePosition : IMonumentRelativePosition
{
    public static MonumentRelativePosition FromVendingMachine(MonumentFinderAdapter monumentFinderAdapter, NPCVendingMachine vendingMachine);
    private MonumentAdapter _monument;
    private Vector3 _position;
    private Vector3 _legacyPosition;
    public string GetMonumentPrefabName();
    public string GetMonumentAlias();
    public Vector3 GetPosition();
    public Vector3 GetLegacyPosition();
}

private class ItemsPaymentProvider : IPaymentProvider
{
    public VendingItem VendingItem;
    private CustomVendingSetup _plugin;
    public ItemsPaymentProvider(CustomVendingSetup plugin);
    public int GetBalance(BasePlayer player);
    public bool AddBalance(BasePlayer player, int amount, TransactionContext transaction);
    public bool TakeBalance(BasePlayer player, int amount, List<Item> collect);
}

private class EconomicsPaymentProvider : IPaymentProvider
{
    private CustomVendingSetup _plugin;
    private Plugin _ownerPlugin { get; set; }
    public EconomicsPaymentProvider(CustomVendingSetup plugin);
    public bool IsAvailable { get; set; }
    public int GetBalance(BasePlayer player);
    public bool AddBalance(BasePlayer player, int amount, TransactionContext transaction);
    public bool TakeBalance(BasePlayer player, int amount, List<Item> collect);
}

private class ServerRewardsPaymentProvider : IPaymentProvider
{
    private CustomVendingSetup _plugin;
    private Plugin _ownerPlugin { get; set; }
    public ServerRewardsPaymentProvider(CustomVendingSetup plugin);
    public bool IsAvailable { get; set; }
    public int GetBalance(BasePlayer player);
    public bool AddBalance(BasePlayer player, int amount, TransactionContext transaction);
    public bool TakeBalance(BasePlayer player, int amount, List<Item> collect);
}

private class PaymentProviderResolver
{
    public readonly EconomicsPaymentProvider EconomicsPaymentProvider;
    public readonly ServerRewardsPaymentProvider ServerRewardsPaymentProvider;
    private readonly CustomVendingSetup _plugin;
    private readonly ItemsPaymentProvider _itemsPaymentProvider;
    private Configuration _config { get; set; }
    public PaymentProviderResolver(CustomVendingSetup plugin);
    public IPaymentProvider Resolve(VendingItem vendingItem);
}

private static class ItemUtils
{
    public static Item FindFirstContainerItem(ItemContainer container, ItemQuery itemQuery);
    public static int SumContainerItems(ItemContainer container, ItemQuery itemQuery);
    public static int SumPlayerItems(BasePlayer player, ItemQuery itemQuery);
    public static int TakeContainerItems(ItemContainer container, ItemQuery itemQuery, int totalAmountToTake, List<Item> collect);
    public static int TakePlayerItems(BasePlayer player, ItemQuery itemQuery, int amountToTake, List<Item> collect);
}

private class DynamicHookSubscriber
{
    private CustomVendingSetup _plugin;
    private HashSet<T> _list;
    private string[] _hookNames;
    public DynamicHookSubscriber(CustomVendingSetup plugin, string[] hookNames);
    public bool Contains(T item);
    public void Add(T item);
    public void Remove(T item);
    public void SubscribeAll();
    public void UnsubscribeAll();
}

private class DataProvider
{
    public static DataProvider FromDictionary(Dictionary<string, object> spec);
    public Dictionary<string, object> Spec { get; set; }
    public CustomGetDataCallback GetDataCallback;
    public CustomSaveDataCallback SaveDataCallback;
    private VendingProfile _vendingProfile;
    private JObject _serializedData;
    public VendingProfile GetData(Configuration config);
    public void SaveData(VendingProfile vendingProfile);
}

private class DataProviderRegistry
{
    private Dictionary<Dictionary<string, object>, DataProvider> _dataProviderCache;
    public DataProvider Register(Dictionary<string, object> dataProviderSpec);
    public void Unregister(DataProvider dataProvider);
}

private class VendingMachineManager
{
    private CustomVendingSetup _plugin;
    private ComponentFactory<NPCVendingMachine, VendingMachineComponent> _componentFactory;
    private DataProviderRegistry _dataProviderRegistry;
    private HashSet<BaseVendingController> _uniqueControllers;
    private Dictionary<NetworkableId, BaseVendingController> _controllersByVendingMachine;
    private Dictionary<DataProvider, CustomVendingController> _controllersByDataProvider;
    public VendingMachineManager(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory, DataProviderRegistry dataProviderRegistry);
    public void HandleVendingMachineSpawned(NPCVendingMachine vendingMachine);
    public void HandleVendingMachineKilled(NPCVendingMachine vendingMachine);
    public BaseVendingController GetController(NPCVendingMachine vendingMachine);
    public void SetupAll();
    public void ResetAll();
    private MonumentVendingController GetControllerByLocation(MonumentRelativePosition location);
    private MonumentVendingController EnsureMonumentController(MonumentRelativePosition location);
    private CustomVendingController GetControllerByDataProvider(DataProvider dataProvider);
    private CustomVendingController EnsureCustomController(DataProvider dataProvider);
}

private class EditContainerComponent : FacepunchBehaviour
{
    public static void AddToContainer(CustomVendingSetup plugin, StorageContainer container, EditController editController);
    private CustomVendingSetup _plugin;
    private EditController _editController;
    private void PlayerStoppedLooting(BasePlayer player);
}

private class EditController
{
    private static void OpenEditPanel(BasePlayer player, StorageContainer containerEntity);
    public BasePlayer EditorPlayer { get; set; }
    private CustomVendingSetup _plugin;
    private BaseVendingController _vendingController;
    private NPCVendingMachine _vendingMachine;
    private StorageContainer _container;
    private EditFormState _formState;
    public EditController(CustomVendingSetup plugin, BaseVendingController vendingController, NPCVendingMachine vendingMachine, BasePlayer editorPlayer);
    public void ToggleBroadcast();
    public void ToggleDroneAccessible();
    public void ApplyStateTo(VendingProfile profile);
    public void HandlePlayerLootEnd(BasePlayer player);
    public void Kill();
    private void DestroyUI();
    private void KillContainer();
}

private abstract class BaseVendingController
{
    public VendingProfile Profile { get; set; }
    public EditController EditController { get; set; }
    public bool HasVendingMachines { get; set; }
    protected CustomVendingSetup _plugin;
    private HashSet<NPCVendingMachine> _vendingMachineList;
    private ComponentFactory<NPCVendingMachine, VendingMachineComponent> _componentFactory;
    private string _cachedShopUI;
    protected BaseVendingController(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory);
    protected abstract void SaveProfile(VendingProfile vendingProfile);
    protected abstract void DeleteProfile(VendingProfile vendingProfile);
    public void StartEditing(BasePlayer player, NPCVendingMachine vendingMachine);
    public void HandleReset();
    public void Destroy();
    public void HandleSave(NPCVendingMachine vendingMachine);
    public void AddVendingMachine(NPCVendingMachine vendingMachine);
    public void RemoveVendingMachine(NPCVendingMachine vendingMachine);
    public void OnEditControllerKilled();
    public string GetShopUI();
    public void UpdateDroneAccessibility();
    protected virtual void CreateOrUpdateProfile(NPCVendingMachine vendingMachine);
    private void SetupVendingMachines();
    private void ResetVendingMachines();
}

private class CustomVendingController : BaseVendingController
{
    public DataProvider DataProvider { get; set; }
    public CustomVendingController(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory, DataProvider dataProvider);
    protected override void SaveProfile(VendingProfile vendingProfile);
    protected override void DeleteProfile(VendingProfile vendingProfile);
}

private class MonumentVendingController : BaseVendingController
{
    public MonumentRelativePosition Location { get; set; }
    private SavedData _pluginData { get; set; }
    public MonumentVendingController(CustomVendingSetup plugin, ComponentFactory<NPCVendingMachine, VendingMachineComponent> componentFactory, MonumentRelativePosition location);
    protected override void SaveProfile(VendingProfile vendingProfile);
    protected override void DeleteProfile(VendingProfile vendingProfile);
    protected override void CreateOrUpdateProfile(NPCVendingMachine vendingMachine);
}

private class ComponentTracker
{
    private readonly Dictionary<THost, TGuest> _hostToGuest;
    public void RegisterComponent(THost host, TGuest guest);
    public TGuest GetComponent(THost host);
    public void UnregisterComponent(THost source);
}

private class TrackedComponent : FacepunchBehaviour
{
    public CustomVendingSetup Plugin;
    public ComponentTracker<THost, TGuest> ComponentTracker;
    public THost Host;
    public virtual void OnCreated();
    protected virtual void OnDestroy();
}

private class ComponentFactory
{
    private CustomVendingSetup _plugin;
    private ComponentTracker<THost, TGuest> _componentTracker;
    public ComponentFactory(CustomVendingSetup plugin, ComponentTracker<THost, TGuest> componentTracker);
    public TGuest GetOrAddTo(THost host);
}

private class VendingMachineComponent : TrackedComponent<NPCVendingMachine, VendingMachineComponent>
{
    public static void RemoveFromVendingMachine(NPCVendingMachine vendingMachine);
    public VendingProfile Profile { get; set; }
    private readonly List<BasePlayer> _adminUIViewers;
    private readonly List<BasePlayer> _shopUIViewers;
    private BaseVendingController _vendingController;
    private NPCVendingMachine _vendingMachine;
    private float[] _refillTimes;
    private string _originalShopName;
    private bool? _originalBroadcast;
    public override void OnCreated();
    public bool HasUI(BasePlayer player);
    public void ShowAdminUI(BasePlayer player);
    public void ShowShopUI(BasePlayer player);
    public void RemoveUI(BasePlayer player);
    protected override void OnDestroy();
    private void PlayerStoppedLooting(BasePlayer player);
    public void SetController(BaseVendingController vendingController);
    public void SetProfile(VendingProfile profile);
    private void ScheduleRefill(int offerIndex, VendingOffer offer, int min);
    private void ScheduleDelayedRefill(int offerIndex, VendingOffer offer);
    private void StopRefilling(int offerIndex);
    private void CustomRefill(bool maxRefill);
    private void TimedRefill();
    private void DestroyAdminUI(BasePlayer player);
    private void DestroyShopUI(BasePlayer player);
    private void DestroyUIs();
    private void DisableVanillaBehavior();
    private void ResetToVanilla();
}

private class LegacyVendingItem
{
    public string Shortname;
    public string DisplayName;
    public int Amount;
    public ulong Skin;
    public bool IsBlueprint;
}

private class LegacyVendingOffer
{
    public LegacyVendingItem Currency;
    public LegacyVendingItem SellItem;
}

private class LegacyVendingProfile
{
    public string Id;
    public List<LegacyVendingOffer> Offers;
    public string Shortname;
    public Vector3 WorldPosition;
    public Vector3 RelativePosition;
    public string RelativeMonument;
    public bool DetectByShortname;
}

private class CaseInsensitiveDictionary : Dictionary<string, TValue>
{
    public CaseInsensitiveDictionary();
    public CaseInsensitiveDictionary(Dictionary<string, TValue> dict);
}

[JsonObject(MemberSerialization.OptIn)]
private class VendingItem
{
    public static VendingItem FromItem(Item item);
    private static List<VendingItem> SerializeContents(List<Item> itemList);
    private static int GetAmmoAmountAndType(Item item, ItemDefinition ammoType);
    [JsonProperty("ShortName")]
    public string ShortName;
    [JsonProperty("DisplayName", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string DisplayName;
    [JsonProperty("Amount")]
    public int Amount;
    [JsonProperty("Skin", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong SkinId;
    [JsonProperty("IsBlueprint", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool IsBlueprint;
    [JsonProperty("DataInt", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int DataInt;
    [JsonProperty("Position", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int Position;
    [JsonProperty("Ammo", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(-1)]
    public int AmmoAmount;
    [JsonProperty("AmmoType", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string AmmoType;
    [JsonProperty("Capacity", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int Capacity;
    [JsonProperty("Contents", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public List<VendingItem> Contents;
    private ItemDefinition _itemDefinition;
    public ItemDefinition ItemDefinition { get; set; }
    private ItemDefinition _ammoTypeDefinition;
    public ItemDefinition AmmoTypeDefinition { get; set; }
    public bool IsValid { get; set; }
    public int ItemId { get; set; }
    public Item Create(int amount);
    public Item Create();
    public VendingItem Copy();
}

[JsonObject(MemberSerialization.OptIn)]
private class VendingOffer
{
    public const int DefaultRefillMax;
    public const int DefaultRefillDelay;
    public const int DefaultRefillAmount;
    public static VendingOffer FromVanillaSellOrder(SellOrder sellOrder, NPCVendingOrder.Entry manifestEntry);
    public static VendingOffer FromItems(CustomVendingSetup plugin, BasePlayer player, Item sellItem, Item currencyItem, Item settingsItem);
    private static CaseInsensitiveDictionary<string> ParseSettingsItem(Item settingsItem);
    private static bool TryParseIntKey(Dictionary<string, string> dict, string key, int result);
    [JsonProperty("SellItem")]
    public VendingItem SellItem;
    [JsonProperty("CurrencyItem")]
    public VendingItem CurrencyItem;
    [JsonProperty("RefillMax", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(DefaultRefillMax)]
    public int RefillMax;
    [JsonProperty("RefillDelay", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(DefaultRefillDelay)]
    public int RefillDelay;
    [JsonProperty("RefillAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(DefaultRefillAmount)]
    public int RefillAmount;
    [JsonProperty("CustomSettings", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public CaseInsensitiveDictionary<object> CustomSettings;
    public bool IsValid { get; set; }
    public VendingOffer Copy();
}

[JsonObject(MemberSerialization.OptIn)]
private class VendingProfile : IMonumentRelativePosition
{
    public static VendingProfile FromVendingMachine(NPCVendingMachine vendingMachine, MonumentRelativePosition location);
    [JsonProperty("ShopName", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string ShopName;
    [JsonProperty("Broadcast", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(true)]
    public bool Broadcast;
    [JsonProperty("DroneAccessible", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(true)]
    public bool DroneAccessible;
    [JsonProperty("Monument", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Monument;
    [JsonProperty("MonumentAlias", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string MonumentAlias;
    [JsonProperty("Position", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Vector3 Position;
    [JsonProperty("LegacyPosition", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Vector3 LegacyPosition;
    [JsonProperty("Offers")]
    public VendingOffer[] Offers;
    public VendingOffer GetOfferForSellOrderIndex(int index);
    public bool HasPaymentProviderCurrency(PaymentProviderConfig paymentProviderConfig);
    public string GetMonumentPrefabName();
    public string GetMonumentAlias();
    public Vector3 GetPosition();
    public Vector3 GetLegacyPosition();
    [OnDeserialized]
    private void OnDeserialized(StreamingContext context);
    private void UpdateOldSaddleOffers();
}

[JsonObject(MemberSerialization.OptIn)]
private class SavedData
{
    [JsonProperty("Vendings", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public List<LegacyVendingProfile> Vendings;
    [JsonProperty("VendingProfiles")]
    public List<VendingProfile> VendingProfiles;
    public static SavedData Load();
    public void Save();
    public VendingProfile FindProfile(IMonumentRelativePosition location);
}

[JsonObject(MemberSerialization.OptIn)]
private class ShopUISettings
{
    [JsonProperty("Enable skin overlays")]
    public bool EnableSkinOverlays;
}

[JsonObject(MemberSerialization.OptIn)]
private class PaymentProviderConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Item short name")]
    public string ItemShortName;
    [JsonProperty("Item skin ID")]
    public ulong ItemSkinId;
    public ItemDefinition ItemDefinition { get; set; }
    public bool EnabledAndValid { get; set; }
    public void Init();
    public bool MatchesItem(VendingItem vendingItem);
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : SerializableConfiguration
{
    [JsonProperty("Shop UI settings")]
    public ShopUISettings ShopUISettings;
    [JsonProperty("Economics integration")]
    public PaymentProviderConfig Economics;
    [JsonProperty("Server Rewards integration")]
    public PaymentProviderConfig ServerRewards;
    [JsonProperty("Override item max stack sizes (shortname: amount)")]
    public Dictionary<string, int> ItemStackSizeOverrides;
    public void Init();
    public int GetItemMaxStackSize(Item item);
}

private class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private static class Lang
{
    public const string ButtonEdit;
    public const string ButtonReset;
    public const string InfoForSale;
    public const string ButtonSave;
    public const string ButtonCancel;
    public const string InfoCost;
    public const string InfoSettings;
    public const string SettingsRefillMax;
    public const string SettingsRefillDelay;
    public const string SettingsRefillAmount;
    public const string ErrorCurrentlyBeingEdited;
}


```

---

## CustomWound

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("CustomWound", "CustomWound", "1.0.9")]
[Description("Custom Recovery")]
public class CustomWound : RustPlugin
{
    private class PlayerRecover : MonoBehaviour
    {
        private BasePlayer player;
        private float lastUpdate;
        private float lastScream;
        public int endChance;
        private int resultChance;
        private int different;
        public bool timeDrawed;
        private void Awake();
        private void Update();
        public void DisableObject(bool withDie);
        private void MakeScream();
    }

    [JsonProperty("Отключить состояние ранения? (Автоматическая смерть)")]
    private static bool CONF_DisableWound;
    [JsonProperty("Отключить возможность встать после ранения?")]
    private static bool CONF_DisableWoundEnd;
    [JsonProperty("Новое время состояния ранения (Должно быть меньше 40)")]
    private static int CONF_WoundTime;
    [JsonProperty("Шанс встать после состояния ранения")]
    private static int CONF_WoundEndChance;
    [JsonProperty("Показывать время до окончания ранения?")]
    private static bool CONF_ShowEndWoundTime;
    [JsonProperty("Разрешать поднимать игроков уколом шприца")]
    private static bool CONF_CanEndWoundBySyringe;
    [JsonProperty("Дополнительный шанс встать, при метаболизме > 250")]
    private static int CONF_StopWoundFromMetabolism;
    [JsonProperty("Включить тряску экрана после подъёма предметом")]
    private static bool CONF_ShakeAfterWoundEnd;
    [JsonProperty("Специальная картинка для предмета")]
    private static string CONF_PictureURL;
    [JsonProperty("Отображать шанс встать после падения")]
    private static bool CONF_ShowChances;
    [JsonProperty("Стандартное количество дефибрилляторов")]
    private static int CONF_DefaultDef;
    [JsonProperty("Разрешить крафтить предмет специальный предмет")]
    private static bool CONF_CanCraftSpecialItem;
    [JsonProperty("Предметы необходимые для крафта")]
    public Dictionary<string, int> CONF_SpecialItemReceipt;
    [JsonProperty("Включить крик игрока в состоянии ранения")]
    private static bool CONF_EnableScream;
    [JsonProperty("Дополнительные шансы встать для игроков с привилегиями")]
    private static Dictionary<string, int> CONF_CustomChances;
    [PluginReference]
    private Plugin ImageLibrary;
    private static CustomWound instance;
    private static string UI_LayerTime;
    private static string UI_LayerChance;
    private static string UI_LayerItem;
    private static List<string> ShakeEffects;
    [JsonProperty("Количество предметов для подъёма у игроков")]
    private static Dictionary<ulong, int> PlayerRecovery;
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void OnPlayerDie(BasePlayer player, HitInfo info);
    private void OnPlayerRespawn(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerRecover(BasePlayer player);
    private void OnHealingItemUse(MedicalTool tool, BasePlayer player);
    private void StartShake(BasePlayer player, float amount);
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerWound(BasePlayer player);
    private void Unload();
    private void SaveData();
    private int GetWakeChance(BasePlayer player);
    private static void UI_DrawWakeChances(BasePlayer player);
    private static void UI_DrawLeftTime(BasePlayer player);
    private static void UI_DrawItemRecovery(BasePlayer player);
    [ChatCommand("recover")]
    private void cmdChatRecover(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_CW_Handler")]
    private void consoleHandler(ConsoleSystem.Arg args);
    [ConsoleCommand("cw")]
    private void cmdAdminCommnand(ConsoleSystem.Arg args);
    private static string HexToRustFormat(string hex);
    private void GetConfig(string menu, string key, T varObject);
}

private class PlayerRecover : MonoBehaviour
{
    private BasePlayer player;
    private float lastUpdate;
    private float lastScream;
    public int endChance;
    private int resultChance;
    private int different;
    public bool timeDrawed;
    private void Awake();
    private void Update();
    public void DisableObject(bool withDie);
    private void MakeScream();
}


```

---

## DailyReward

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Oxide.Core.Database;

Oxide.Plugins
[Info("DailyReward", "Hougan", "1.0.0")]
[Description("Награды за ежедневный вход на сервер")]
public class DailyReward : RustPlugin
{
     Core.MySql.Libraries.MySql Sql;
     Connection Sql_conn;
    private class DailyPlayer
    {
        [JsonProperty("Отображаемое имя игрока")]
        public string DisplayName;
        [JsonProperty("Отображаемый ID игрока")]
        public ulong UserID;
        [JsonProperty("Дни в которых игрок получил награду")]
        public Dictionary<string, bool> Joins;
    }

    private Dictionary<ulong, DailyPlayer> dailyPlayers;
    [JsonProperty("Название слоя с ГУИ")]
    private string Layer;
    [JsonProperty("Начальный бонус в рублях")]
    private int DefaultMoney;
    [JsonProperty("Максимальное пополнение за раз")]
    private int MaxDeposit;
    [JsonProperty("Ключ магазина")]
    private string APIKey;
    [JsonProperty("ID Сервера")]
    private string ServerID;
    [JsonProperty("Использовать MySQL?")]
    private bool MySQL_Use;
    [JsonProperty("MySQL. IP БД")]
    private string MySQL_IP;
    [JsonProperty("MySQL. Port")]
    private int MySQL_Port;
    [JsonProperty("MySQL. Название БД")]
    private string MySQL_DBName;
    [JsonProperty("MySQL. Название таблицы")]
    private string MySQL_TBName;
    [JsonProperty("MySQL. Имя пользователя")]
    private string MySQL_Name;
    [JsonProperty("MySQL. Пароль пользователя")]
    private string MySQL_Password;
    [ConsoleCommand("db.get")]
    private void cmdConsoleGet(ConsoleSystem.Arg args);
    private void TodayPlayer(BasePlayer player);
    private void AddPrize(BasePlayer player, bool newPlayer);
    [PluginReference]
    private Plugin RustStore;
    private bool Moscow;
    private void AddMoney(ulong userId, int amount);
     void ExecuteApiRequest(Dictionary<string, string> args, bool Moscow);
    private void MySQL_Initialize();
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
    private void Unload();
    private void OnPlayerInit(BasePlayer player);
    private static string HexToRustFormat(string hex);
    [ChatCommand("daily")]
    private void DailyGUI(BasePlayer player);
     T GetConfig(string name, T value);
}

private class DailyPlayer
{
    [JsonProperty("Отображаемое имя игрока")]
    public string DisplayName;
    [JsonProperty("Отображаемый ID игрока")]
    public ulong UserID;
    [JsonProperty("Дни в которых игрок получил награду")]
    public Dictionary<string, bool> Joins;
}


```

---

## DamageDisplay

```csharp
using Rust;
using System;
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("DamageDisplayGUI", "cogu", "1.6.1")]
[Description("Displays the given damage to a player in a GUI")]
 class DamageDisplay : RustPlugin
{
     HashSet<ulong> users;
     System.Collections.Generic.List<ulong> DisabledFor;
    public float DisplayAttackerNameRange { get; set; }
    public float DisplayVictimNameRange { get; set; }
    public bool DisplayDistance { get; set; }
    public bool DisplayBodyPart { get; set; }
    public bool DamageForAttacker { get; set; }
    public bool DamageForVictim { get; set; }
    public float X_MinVictim { get; set; }
    public float X_MaxVictim { get; set; }
    public float Y_MinVictim { get; set; }
    public float Y_MaxVictim { get; set; }
    public float X_MinAttacker { get; set; }
    public float X_MaxAttacker { get; set; }
    public float Y_MinAttacker { get; set; }
    public float Y_MaxAttacker { get; set; }
    public float DisplayTime { get; set; }
     void Unload();
     void OnServerSave();
    protected override void LoadDefaultConfig();
    private void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo hitInfo);
     void Loaded();
     void OnPlayerInit();
     void SaveData();
     void LoadSavedData();
    private void UseUI(BasePlayer player, string dmg, string dst, string name, string bpart, float xmin, float xmax, float ymin, float ymax);
    private void DestroyNotification(BasePlayer player);
     string GetBoneName(BaseCombatEntity entity, uint boneId);
     string FirstUpper(string original);
     string ListToString(List<string> list, int first, string seperator);
     string GetDistance(BaseCombatEntity entity, HitInfo info);
}


```

---

## DamageModifier

```csharp
using System.Collections.Generic;
using System;
using Rust;

Oxide.Plugins
[Info("DamageModifier", "ColonBlow", "1.2.1")]
internal class DamageModifier : RustPlugin
{
    private const int DamageTypeMax;
    private readonly float[] _modifiers;
    private bool _didConfigChange;
    private void Loaded();
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private object GetConfigValue(string category, string setting, object defaultValue);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
}


```

---

## DeadPlayersList

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Dead Players List", "Mughisi", "2.0.5", ResourceId = 696)]
 class DeadPlayersList : RustPlugin
{
     class LocationInfo
    {
        public string x;
        public string y;
        public string z;
         Vector3 position;
        public LocationInfo(Vector3 position);
        public Vector3 GetPosition();
    }

     class DeadPlayerInfo
    {
        public string UserId;
        public string Name;
        public string Reason;
        public LocationInfo Position;
        public DeadPlayerInfo();
        public DeadPlayerInfo(BasePlayer victim, HitInfo info);
        public ulong GetUserId();
    }

     class StoredData
    {
        public HashSet<DeadPlayerInfo> DeadPlayers;
        public StoredData();
    }

     StoredData storedData;
     Hash<ulong, DeadPlayerInfo> deadPlayers;
     bool dataChanged;
     void Loaded();
     void Unloaded();
     void LoadData();
     void SaveData();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnEntityDeath(BaseEntity entity, HitInfo info);
     void OnServerSave();
     Dictionary<string, string> GetPlayerList();
     string GetPlayerName(object userID);
     string GetPlayerDeathReason(object userID);
     Vector3 GetPlayerDeathPosition(object userID);
}

 class LocationInfo
{
    public string x;
    public string y;
    public string z;
     Vector3 position;
    public LocationInfo(Vector3 position);
    public Vector3 GetPosition();
}

 class DeadPlayerInfo
{
    public string UserId;
    public string Name;
    public string Reason;
    public LocationInfo Position;
    public DeadPlayerInfo();
    public DeadPlayerInfo(BasePlayer victim, HitInfo info);
    public ulong GetUserId();
}

 class StoredData
{
    public HashSet<DeadPlayerInfo> DeadPlayers;
    public StoredData();
}


```

---

## DeathKick

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("DeathKick", "k1lly0u", "0.1.2", ResourceId = 1779)]
public class DeathKick : RustPlugin
{
    private Dictionary<ulong, double> deadPlayers;
    private List<Timer> Timers;
    private Dictionary<ulong, int> deathCounts;
     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
    private void ProcessDeath(BasePlayer player, HitInfo info, bool isBounty);
    public bool GetDeathType(BasePlayer player, HitInfo info);
     object CanClientLogin(Network.Connection connection);
     void ClearData();
    static double GrabCurrentTime();
    static int cooldownTime;
    static int deathLimit;
    static bool usePlayers;
    static bool useHeli;
    static bool useAnimals;
    static bool useBeartrap;
    static bool useLandmine;
    static bool useFloorspikes;
    static bool useBarricades;
    static bool useAutoturret;
    static bool useSuicide;
    static bool useFall;
    static bool useBounty;
    private bool changed;
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     Dictionary<string, string> messages;
}


```

---

## DeathMarker

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("DeathMarker", "redBDGR", "1.0.11")]
[Description("Show your death location on your map.")]
 class DeathMarker : RustPlugin
{
    private const string permissionName;
    private bool Changed;
    private float radiusSize;
    private float markerAlpha;
    private float markerLenght;
    private string colour;
    private float messageDelay;
    private bool sendNotificationMessage;
    private bool use3DMarker;
    private bool use3DRadius;
    private bool arrowEnabled;
    private bool ingameTextEnabled;
    private float arrowVerticalOffset;
    private float textVerticalOffset;
    private float ingameRadiusSize;
    private bool ingameRadiusRandomized;
    private float ingameVisualsLength;
    private string visualsColour;
    private bool useDeathConsoleDebug;
    private class MarkerInfo
    {
        public MapMarkerGenericRadius radiusMarker;
        public VendingMachineMapMarker vendingMarker;
    }

    private Dictionary<string, MarkerInfo> playerDic;
    private List<MapMarker> mapMarkers;
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void Init();
    private void Unload();
    private object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
    private object OnPlayerDie(BasePlayer player, HitInfo info);
    private void OnPlayerRespawned(BasePlayer player);
    private static Color ConvertColourString(string colourString);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
}

private class MarkerInfo
{
    public MapMarkerGenericRadius radiusMarker;
    public VendingMachineMapMarker vendingMarker;
}


```

---

## DeathMessage-1.1.2

```csharp
using System.Collections.Generic;
using System.Collections.Specialized;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;
using Rust;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;
using System;
using System.Globalization;
using System.Linq;
using System.Net;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("DeathMessage", "Damo/beee / M&B-Studios", "1.1.2", ResourceId = 0)]
[Description("Players can freely switch between the two modes of death tips")]
 class DeathMessage : RustPlugin
{
    private ConfigData configData;
    private int _TempText;
    private const string Permission_registration;
    private static DeathMessage Instance;
    private HashSet<ulong> _DeathMessageType;
    public TextAnchor _textAnchor;
    private ulong Last_attacker;
    private class ConfigData
    {
        public Oxide.Core.VersionNumber Version;
        [JsonProperty("➊ Global Messages settings")]
        public EnableSettings Enable_settings;
        [JsonProperty("➊ Discord settings")]
        public DiscordEnableSettings Discord_settings;
        [JsonProperty("➋ Display name modification and activation")]
        public AboutName About_name;
        [JsonProperty("➌ Other settings")]
        public OtherSettings Other_settings;
        [JsonProperty("➍ Lang settings")]
        public Dictionary<string, string> Lang_settings;
    }

    private class EnableSettings
    {
        [JsonProperty("Enable About Animal")]
        public bool Enable_Animal;
        [JsonProperty("Enable About Entitys")]
        public bool Enable_Entity;
        [JsonProperty("Enable About NPC")]
        public bool Enable_NPC;
        [JsonProperty("Enable Player Deaths")]
        public bool Enable_Player;
        [JsonProperty("Enable About Suicide")]
        public bool Enable_About_Suicide;
    }

    private class DiscordEnableSettings
    {
        [JsonProperty("Webhook URL")]
        public string DiscordWebHookUrl;
        [JsonProperty("Bot Name")]
        public string DiscordBotName;
        [JsonProperty("Bot Avatar Link")]
        public string DiscordAvatarLink;
        [JsonProperty("Enable Animal Deaths")]
        public bool Enable_Animal;
        [JsonProperty("Enable Entities Deaths")]
        public bool Enable_Entity;
        [JsonProperty("Enable NPC Deaths")]
        public bool Enable_NPC;
        [JsonProperty("Enable Player Deaths")]
        public bool Enable_Player;
    }

    private class OtherSettings
    {
        [JsonProperty("Default command")]
        public string Default_command;
        [JsonProperty("Chat Icon Id")]
        public string Chat_Icon;
        [JsonProperty("Default display(true = FloatUI , false = Chat box)")]
        public bool Defaultdisplay;
        [JsonProperty("FloatUI message closing time second")]
        public int Ui_time;
        [JsonProperty("Click on FloatUI switch to the chat box in seconds")]
        public int switching_time;
    }

    private class AboutName
    {
        [JsonProperty("➀ Animal name")]
        public Dictionary<string, EnableData> Animal_Name;
        [JsonProperty("➁ NPC name")]
        public Dictionary<string, EnableData> NPC_name;
        [JsonProperty("➂ Entity name")]
        public Dictionary<string, EnableData> Entity_Name;
        [JsonProperty("➃ Weapon name")]
        public Dictionary<string, string> Default_weapon;
        [JsonProperty("➄ Body part name")]
        public Dictionary<string, string> Body_part_name;
    }

    private class EnableData
    {
        [JsonProperty("Enable")]
        public bool Enable;
        [JsonProperty("Display name")]
        public string Display_name;
    }

    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void LoadVariables();
    protected override void LoadDefaultConfig();
     DamoData damoData;
     class DamoData
    {
        public float[] pos;
    }

    private void LoadData();
    private void OnServerSave();
    private void SaveData();
    private void ClearData();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
    private void Get_textAnchor();
     void Send_Kill_Player_Note(String langKey, String killerName, String shortnameWeapon, String victomPlayer, String distanceKiller, String bodyName);
    private void UPGP(BroadcastTo broadcastTo, string Key, string A, string B, string C, string D, string E);
    private BasePlayer FindPlayer(string nameOrIdOrIp);
    private BasePlayer FindPlayer(ulong userId);
    private string GetBodyName(HitInfo hitInfo);
    private string HitBodyPos(HitInfo hitInfo);
    private bool IS_BotSpawn_Name(string displayName);
    private string Weapon_Name(BasePlayer player, HitInfo hit);
    private string Attack_distance(HitInfo info);
    private CuiElement DanTextC(string parent, string Nam, string text, string anchorMin, string anchorMax, int DX, TextAnchor align);
    private CuiElement DanText(string parent, string Nam, string text, string anchorMin, string anchorMax, int DX, TextAnchor align);
    private CuiElement CuiText(string parent, string text, string anchorMin, string anchorMax, string yanst, int DX, TextAnchor align);
    private CuiElement YxText(string parent, string text, string anchorMin, string anchorMax, int DX, TextAnchor align);
    private CuiPanel CreatePanel(string anchorMin, string anchorMax, string color, bool SB);
    private CuiButton GButton(string ml);
    private CuiButton DMButton(string dz, string ml, string Zx, string zd, string wb, string ys, int dx, TextAnchor ddd);
    private void Remove_UI(BasePlayer player, int r_num);
    private void Destroy_All(BasePlayer player);
    private Dictionary<ulong, UIHandler> _playersUIHandler;
    private UIHandler GetUiInfo(BasePlayer player);
    private class UIHandler
    {
        public List<int> _UI_List;
        public HashSet<int> _Remove;
        public int num;
    }

    private string _mianbanyou_A;
    private string _mianbanyou_B;
    public void ControlMenuUi(BasePlayer player);
    private void UpUIMessage(BasePlayer player, string Stext);
    private List<string> TempText;
    private void UpUIMessageText(BasePlayer player);
    private void UpUIMessageRE(BasePlayer player);
    private void DeathMessageCommand(IPlayer iplayer, string command, string[] args);
    [ConsoleCommand("deathmessage.ui")]
    private void cmdDeathMessageui(ConsoleSystem.Arg arg);
    [ConsoleCommand("deathmessage.ove")]
    private void cmdDeathOver(ConsoleSystem.Arg arg);
    [ConsoleCommand("deathmessage.cmd")]
    private void cmdDeathMessage(ConsoleSystem.Arg arg);
    private BroadcastTo GetNPCEnableState();
    private BroadcastTo GetAnimalEnableState();
    private BroadcastTo GetEntityEnableState();
    private void BroadcastToDiscord(string message);
    private readonly Dictionary<string, string> _headers;
    private class DiscordMessage
    {
        [JsonProperty("content")]
        private string Content { get; set; }
        [JsonProperty("username")]
        private string Username { get; set; }
        [JsonProperty("avatar_url")]
        private string AvatarURL { get; set; }
        public DiscordMessage(string content, string username, string avatarurl);
        public DiscordMessage AddContent(string content);
        public string GetContent();
        public string ToJson();
    }

    private void DiscordSendMessageCallback(int code, string message);
    public static string ClearFormatting(string msg);
}

private class ConfigData
{
    public Oxide.Core.VersionNumber Version;
    [JsonProperty("➊ Global Messages settings")]
    public EnableSettings Enable_settings;
    [JsonProperty("➊ Discord settings")]
    public DiscordEnableSettings Discord_settings;
    [JsonProperty("➋ Display name modification and activation")]
    public AboutName About_name;
    [JsonProperty("➌ Other settings")]
    public OtherSettings Other_settings;
    [JsonProperty("➍ Lang settings")]
    public Dictionary<string, string> Lang_settings;
}

private class EnableSettings
{
    [JsonProperty("Enable About Animal")]
    public bool Enable_Animal;
    [JsonProperty("Enable About Entitys")]
    public bool Enable_Entity;
    [JsonProperty("Enable About NPC")]
    public bool Enable_NPC;
    [JsonProperty("Enable Player Deaths")]
    public bool Enable_Player;
    [JsonProperty("Enable About Suicide")]
    public bool Enable_About_Suicide;
}

private class DiscordEnableSettings
{
    [JsonProperty("Webhook URL")]
    public string DiscordWebHookUrl;
    [JsonProperty("Bot Name")]
    public string DiscordBotName;
    [JsonProperty("Bot Avatar Link")]
    public string DiscordAvatarLink;
    [JsonProperty("Enable Animal Deaths")]
    public bool Enable_Animal;
    [JsonProperty("Enable Entities Deaths")]
    public bool Enable_Entity;
    [JsonProperty("Enable NPC Deaths")]
    public bool Enable_NPC;
    [JsonProperty("Enable Player Deaths")]
    public bool Enable_Player;
}

private class OtherSettings
{
    [JsonProperty("Default command")]
    public string Default_command;
    [JsonProperty("Chat Icon Id")]
    public string Chat_Icon;
    [JsonProperty("Default display(true = FloatUI , false = Chat box)")]
    public bool Defaultdisplay;
    [JsonProperty("FloatUI message closing time second")]
    public int Ui_time;
    [JsonProperty("Click on FloatUI switch to the chat box in seconds")]
    public int switching_time;
}

private class AboutName
{
    [JsonProperty("➀ Animal name")]
    public Dictionary<string, EnableData> Animal_Name;
    [JsonProperty("➁ NPC name")]
    public Dictionary<string, EnableData> NPC_name;
    [JsonProperty("➂ Entity name")]
    public Dictionary<string, EnableData> Entity_Name;
    [JsonProperty("➃ Weapon name")]
    public Dictionary<string, string> Default_weapon;
    [JsonProperty("➄ Body part name")]
    public Dictionary<string, string> Body_part_name;
}

private class EnableData
{
    [JsonProperty("Enable")]
    public bool Enable;
    [JsonProperty("Display name")]
    public string Display_name;
}

 class DamoData
{
    public float[] pos;
}

private class UIHandler
{
    public List<int> _UI_List;
    public HashSet<int> _Remove;
    public int num;
}

private class DiscordMessage
{
    [JsonProperty("content")]
    private string Content { get; set; }
    [JsonProperty("username")]
    private string Username { get; set; }
    [JsonProperty("avatar_url")]
    private string AvatarURL { get; set; }
    public DiscordMessage(string content, string username, string avatarurl);
    public DiscordMessage AddContent(string content);
    public string GetContent();
    public string ToJson();
}


```

---

## DeathMessages

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Death Messages", "Skrip|Tal", "2.1.55")]
 class DeathMessages : RustPlugin
{
    private static PluginConfig _config;
    private string version;
    private List<DeathMessage> _notes;
    private Dictionary<ulong, HitInfo> _lastHits;
     class PluginConfig
    {
        [JsonProperty("A. Время показа сообщения (сек)")]
        public int Cooldown { get; set; }
        [JsonProperty("B. Размер текста")]
        public int FontSize { get; set; }
        [JsonProperty("C. Показывать убиства животных")]
        public bool ShowDeathAnimals { get; set; }
        [JsonProperty("D. Показывать убийства спящих")]
        public bool ShowDeathSleepers { get; set; }
        [JsonProperty("E. Хранение логов")]
        public bool Log { get; set; }
        [JsonProperty("F. Цвет атакующего")]
        public string ColorAttacker { get; set; }
        [JsonProperty("G. Цвет убитого")]
        public string ColorVictim { get; set; }
        [JsonProperty("H. Цвет оружия")]
        public string ColorWeapon { get; set; }
        [JsonProperty("I. Цвет дистанции")]
        public string ColorDistance { get; set; }
        [JsonProperty("J. Цвет части тела")]
        public string ColorBodyPart { get; set; }
        [JsonProperty("K. Дистанция")]
        public double Distance { get; set; }
        [JsonProperty("L. Название вертолета")]
        public string HelicopterName { get; set; }
        [JsonProperty("M. Название Bradlay (Танк)")]
        public string BradleyAPCName { get; set; }
        [JsonProperty("N. Имя NPC")]
        public string NPCName { get; set; }
        [JsonProperty("O. Имя Zombie")]
        public string ZombieName { get; set; }
        [JsonProperty("Оружие")]
        public Dictionary<string, string> Weapons { get; set; }
        [JsonProperty("Конструкции")]
        public Dictionary<string, string> Structures { get; set; }
        [JsonProperty("Ловушки")]
        public Dictionary<string, string> Traps { get; set; }
        [JsonProperty("Турели")]
        public Dictionary<string, string> Turrets { get; set; }
        [JsonProperty("Животные")]
        public Dictionary<string, string> Animals { get; set; }
        [JsonProperty("Сообщения")]
        public Dictionary<string, string> Messages { get; set; }
        [JsonProperty("Части тела")]
        public Dictionary<string, string> BodyParts { get; set; }
    }

     class Attacker
    {
        public Attacker(BaseEntity entity);
        public BaseEntity Entity { get; set; }
        public string Name { get; set; }
        public AttackerType Type { get; set; }
        private AttackerType InitializeType();
        private string InitializeName();
    }

     class Victim
    {
        public Victim(BaseCombatEntity entity);
        public BaseCombatEntity Entity { get; set; }
        public string Name { get; set; }
        public VictimType Type { get; set; }
        private VictimType InitializeType();
        private string InitializeName();
    }

     class DeathMessage
    {
        public DeathMessage(Attacker attacker, Victim victim, string weapon, string damageType, string bodyPart, double distance);
        public List<BasePlayer> Players { get; set; }
        public Attacker Attacker { get; set; }
        public Victim Victim { get; set; }
        public string Weapon { get; set; }
        public string BodyPart { get; set; }
        public string DamageType { get; set; }
        public double Distance { get; set; }
        public DeathReason Reason { get; set; }
        public string Message { get; set; }
        private DeathReason InitializeReason();
        private DeathReason GetDeathReason(string damage);
        private string InitializeDeathMessage();
    }

    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private Dictionary<uint, BasePlayer> LastHeli;
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
    private void Unload();
    private void AddNote(DeathMessage note);
    private void RefreshUI(DeathMessage note);
    private void DestroyUI(BasePlayer player);
    private void InitilizeUI(BasePlayer player);
    private string InitilizeLabel(CuiElementContainer container, string text, string anchorMin, string anchorMax);
    private static string FirstUpper(string str);
    private static string FormatName(string prefab);
    private static string GetMessage(string name, Dictionary<string, string> source);
}

 class PluginConfig
{
    [JsonProperty("A. Время показа сообщения (сек)")]
    public int Cooldown { get; set; }
    [JsonProperty("B. Размер текста")]
    public int FontSize { get; set; }
    [JsonProperty("C. Показывать убиства животных")]
    public bool ShowDeathAnimals { get; set; }
    [JsonProperty("D. Показывать убийства спящих")]
    public bool ShowDeathSleepers { get; set; }
    [JsonProperty("E. Хранение логов")]
    public bool Log { get; set; }
    [JsonProperty("F. Цвет атакующего")]
    public string ColorAttacker { get; set; }
    [JsonProperty("G. Цвет убитого")]
    public string ColorVictim { get; set; }
    [JsonProperty("H. Цвет оружия")]
    public string ColorWeapon { get; set; }
    [JsonProperty("I. Цвет дистанции")]
    public string ColorDistance { get; set; }
    [JsonProperty("J. Цвет части тела")]
    public string ColorBodyPart { get; set; }
    [JsonProperty("K. Дистанция")]
    public double Distance { get; set; }
    [JsonProperty("L. Название вертолета")]
    public string HelicopterName { get; set; }
    [JsonProperty("M. Название Bradlay (Танк)")]
    public string BradleyAPCName { get; set; }
    [JsonProperty("N. Имя NPC")]
    public string NPCName { get; set; }
    [JsonProperty("O. Имя Zombie")]
    public string ZombieName { get; set; }
    [JsonProperty("Оружие")]
    public Dictionary<string, string> Weapons { get; set; }
    [JsonProperty("Конструкции")]
    public Dictionary<string, string> Structures { get; set; }
    [JsonProperty("Ловушки")]
    public Dictionary<string, string> Traps { get; set; }
    [JsonProperty("Турели")]
    public Dictionary<string, string> Turrets { get; set; }
    [JsonProperty("Животные")]
    public Dictionary<string, string> Animals { get; set; }
    [JsonProperty("Сообщения")]
    public Dictionary<string, string> Messages { get; set; }
    [JsonProperty("Части тела")]
    public Dictionary<string, string> BodyParts { get; set; }
}

 class Attacker
{
    public Attacker(BaseEntity entity);
    public BaseEntity Entity { get; set; }
    public string Name { get; set; }
    public AttackerType Type { get; set; }
    private AttackerType InitializeType();
    private string InitializeName();
}

 class Victim
{
    public Victim(BaseCombatEntity entity);
    public BaseCombatEntity Entity { get; set; }
    public string Name { get; set; }
    public VictimType Type { get; set; }
    private VictimType InitializeType();
    private string InitializeName();
}

 class DeathMessage
{
    public DeathMessage(Attacker attacker, Victim victim, string weapon, string damageType, string bodyPart, double distance);
    public List<BasePlayer> Players { get; set; }
    public Attacker Attacker { get; set; }
    public Victim Victim { get; set; }
    public string Weapon { get; set; }
    public string BodyPart { get; set; }
    public string DamageType { get; set; }
    public double Distance { get; set; }
    public DeathReason Reason { get; set; }
    public string Message { get; set; }
    private DeathReason InitializeReason();
    private DeathReason GetDeathReason(string damage);
    private string InitializeDeathMessage();
}


```

---

## DeathNotices

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using Newtonsoft.Json.Linq;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using System.Linq;
using Oxide.Core;
using System;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("DeathNotices", "Stifler", "1.0.0")]
[Description("Broadcast deaths with many details")]
 class DeathNotices : RustPlugin
{
     bool debug;
     Dictionary<ulong, HitInfo> LastWounded;
     Dictionary<string, string> reproduceableKills;
     Dictionary<BasePlayer, Timer> timers;
     Dictionary<ulong, PlayerSettings> playerSettings;
    static DeathNotices dn;
     float AnchorMaxY;
     List<string> killsCache;
     List<Timer> killsTimer;
     UIColor deathNoticeShadowColor;
     UIColor deathNoticeColor;
     List<string> selfInflictedDeaths;
     List<DeathReason> SleepingDeaths;
     List<Regex> regexTags;
     List<string> tags;
     bool MessageRadiusEnabled;
     float MessageRadius;
     bool LogToFile;
     bool WriteToConsole;
     bool UseSimpleUI;
     string AttachmentSplit;
     string AttachmentFormatting;
     string ChatTitle;
     string ChatFormatting;
     string ConsoleFormatting;
     string TitleColor;
     string VictimColor;
     string AttackerColor;
     string WeaponColor;
     string AttachmentColor;
     string DistanceColor;
     string BodypartColor;
     string MessageColor;
     Dictionary<string, object> Names;
     Dictionary<string, object> Bodyparts;
     Dictionary<string, object> Weapons;
     Dictionary<string, object> Attachments;
     Dictionary<string, List<string>> Messages;
     bool SimpleUI_StripColors;
     int SimpleUI_FontSize;
     float SimpleUI_Top;
     float SimpleUI_Left;
     float SimpleUI_MaxWidth;
     float SimpleUI_MaxHeight;
     float SimpleUI_HideTimer;
     class UIColor
    {
         string color;
        public UIColor(double red, double green, double blue, double alpha);
        public override string ToString();
    }

     class UIObject
    {
         List<object> ui;
         List<string> objectList;
        public UIObject();
         string RandomString();
        public void Draw(BasePlayer player);
        public void Destroy(BasePlayer player);
        public string AddText(string name, double left, double top, double width, double height, UIColor color, string text, int textsize, string parent, int alignmode, float fadeIn, float fadeOut);
    }

     class PlayerSettings
    {
        public bool ui;
        public PlayerSettings();
        internal PlayerSettings(DeathNotices deathnotices);
    }

     class Attacker
    {
        public string name;
        [JsonIgnore]
        public BaseCombatEntity entity;
        public AttackerType type;
        public string TryGetName();
        public AttackerType TryGetType();
    }

     class Victim
    {
        public string name;
        [JsonIgnore]
        public BaseCombatEntity entity;
        public VictimType type;
        public string TryGetName();
        public VictimType TryGetType();
    }

     class DeathData
    {
        public Victim victim;
        public Attacker attacker;
        public DeathReason reason;
        public string damageType;
        public string weapon;
        public List<string> attachments;
        public string bodypart;
        internal float _distance;
        public float distance { get; set; }
        public DeathReason TryGetReason();
        public DeathReason GetDeathReason(string damage);
        [JsonIgnore]
        internal string JSON { get; set; }
        internal static DeathData Get(object obj);
    }

     List<string> playerSettingFields { get; set; }
     List<string> GetSettingValues(BasePlayer player);
     void SetSettingField(BasePlayer player, string field, T value);
     void Loaded();
    protected override void LoadDefaultConfig();
     void OnPlayerInit(BasePlayer player);
     void LoadData();
     void SaveData();
     void LoadConfig();
     void LoadMessages();
    [ChatCommand("deaths")]
     void cmdDeaths(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("reproducekill")]
     void ccmdReproduceKill(ConsoleSystem.Arg arg);
     void AddDeath(string text);
     void ShowDeathHud();
     void removeLine();
     HitInfo TryGetLastWounded(ulong uid, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo info);
     void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
     void NoticeDeath(DeathData data, bool reproduced);
     string FormatThrownWeapon(string unformatted);
     string StripTags(string original);
     string FirstUpper(string original);
     List<string> GetMessages(string reason);
     List<string> GetAttachments(HitInfo info);
     string GetBoneName(BaseCombatEntity entity, uint boneId);
     bool InRadius(BasePlayer player, BaseCombatEntity attacker);
     string GetDeathMessage(DeathData data, bool console);
     DeathData UpdateData(DeathData data);
     bool CanSee(BasePlayer player, string type);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     string GetMsg(string key, object userID);
     void RegisterPerm(string[] permArray);
     bool HasPerm(object uid, string[] permArray);
     string PermissionPrefix { get; set; }
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg, object uid);
     void UIMessage(BasePlayer player, string message);
}

 class UIColor
{
     string color;
    public UIColor(double red, double green, double blue, double alpha);
    public override string ToString();
}

 class UIObject
{
     List<object> ui;
     List<string> objectList;
    public UIObject();
     string RandomString();
    public void Draw(BasePlayer player);
    public void Destroy(BasePlayer player);
    public string AddText(string name, double left, double top, double width, double height, UIColor color, string text, int textsize, string parent, int alignmode, float fadeIn, float fadeOut);
}

 class PlayerSettings
{
    public bool ui;
    public PlayerSettings();
    internal PlayerSettings(DeathNotices deathnotices);
}

 class Attacker
{
    public string name;
    [JsonIgnore]
    public BaseCombatEntity entity;
    public AttackerType type;
    public string TryGetName();
    public AttackerType TryGetType();
}

 class Victim
{
    public string name;
    [JsonIgnore]
    public BaseCombatEntity entity;
    public VictimType type;
    public string TryGetName();
    public VictimType TryGetType();
}

 class DeathData
{
    public Victim victim;
    public Attacker attacker;
    public DeathReason reason;
    public string damageType;
    public string weapon;
    public List<string> attachments;
    public string bodypart;
    internal float _distance;
    public float distance { get; set; }
    public DeathReason TryGetReason();
    public DeathReason GetDeathReason(string damage);
    [JsonIgnore]
    internal string JSON { get; set; }
    internal static DeathData Get(object obj);
}


```

---

## DeathStats

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("DeathStats", "SkiTles", "0.3")]
[Description("Отображение статистики на экране смерти")]
 class DeathStats : RustPlugin
{
    private List<string> openUI;
    private bool NewWipe;
     void OnServerInitialized();
     void OnNewSave(string filename);
     void OnServerSave();
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerRespawned(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProtoBuf.ProjectileShoot projectiles);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hitinfo);
    private CuiElement MainPanel(string name, string color, string anMin, string anMax);
    private CuiElement Panel(string name, string parent, string color, string anMin, string anMax);
    private CuiElement Text(string parent, string color, string text, TextAnchor pos, string fname, int fsize, string anMin, string anMax);
    private CuiElement Button(string name, string parent, string command, string color, string anMin, string anMax);
    private void DestroyGUI(BasePlayer player);
    private void DrawGUI(BasePlayer player);
    [ConsoleCommand("ds.clear")]
    private void Clear(ConsoleSystem.Arg arg);
    [ConsoleCommand("ds.close")]
    private void Close(ConsoleSystem.Arg arg);
     class PlayerInfo
    {
        public int shoots;
        public int hits;
        public int hs;
        public int kills;
        public int deaths;
        public int damage;
    }

     class DataStorageStats
    {
        public Dictionary<ulong, PlayerInfo> PlayersStats;
        public DataStorageStats();
    }

     DataStorageStats data;
    private DynamicConfigFile DSdata;
     void LoadData();
     void SaveData();
    private Dictionary<ulong, TempPlayerInfo> TempStats;
     class TempPlayerInfo
    {
        public int shoots;
        public int hits;
        public int hs;
        public int dmg;
        public int kills;
    }

    private string GetKD(int kills, int deaths);
    private string GetAccuracy(int hits, int shoots);
    private string GetAVG(int dmg, int deaths);
    private void AddPlayer(BasePlayer player);
    private void AddPlayerT(BasePlayer player);
    private void ClearStatsT(BasePlayer player);
    private void ClearStats(ulong userid);
    private bool IsNPC(BasePlayer player);
}

 class PlayerInfo
{
    public int shoots;
    public int hits;
    public int hs;
    public int kills;
    public int deaths;
    public int damage;
}

 class DataStorageStats
{
    public Dictionary<ulong, PlayerInfo> PlayersStats;
    public DataStorageStats();
}

 class TempPlayerInfo
{
    public int shoots;
    public int hits;
    public int hs;
    public int dmg;
    public int kills;
}


```

---

## Decay

```csharp


```

---

## Deekay

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Plugins;
using UnityEngine;
using Facepunch;
using System;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Deekay", "k1lly0u", "0.2.27")]
 class Deekay : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
    private static Deekay ins;
    private Hash<uint, DecayManager> dkEntities;
    private bool isInitialized;
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnDoorOpened(Door door, BasePlayer player);
    private void OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
    private void OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
    private void OnEntityKill(BaseNetworkable entity);
    private void Unload();
    private void InitializeConfigData();
    private T[] GetAllPrefabs();
    private void FindAllEntities();
    private void DecayTouch(Vector3 position);
    private class DecayManager
    {
        public BaseCombatEntity Entity { get; set; }
        private ConfigData.DecayData decayData;
        private bool isInPrivs;
        private bool isConstruction;
        private float decayRate;
        public DecayManager();
        public DecayManager(BaseCombatEntity entity, ConfigData.DecayData decayData);
        public void BeginDecay();
        private void SetInitialTimer();
        private void RunDecay();
        public bool DealDamage();
        public bool IsInPrivilege(bool reset);
        public void ResetDecayOnDestroy();
        public void ResetDecay(bool addRandom);
        public void CancelInvokes();
        public void UpdateDecayRates(ConfigData.DecayData decayData);
    }

    [ConsoleCommand("dk.reset")]
    private void ccmdDKReset(ConsoleSystem.Arg arg);
    [ConsoleCommand("dk.rundecay")]
    private void ccmdDKRun(ConsoleSystem.Arg arg);
    [ConsoleCommand("dk.setall")]
    private void ccmdDKSet(ConsoleSystem.Arg arg);
    private object ParseType(string type);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Activity - Player activity will reset nearby decay timers")]
        public bool MonitorActivity { get; set; }
        [JsonProperty(PropertyName = "Activity - Radius of effect")]
        public float ActivityRadius { get; set; }
        [JsonProperty(PropertyName = "Decay - 1. Default decay settings for all entities when plugin first loads")]
        public DecayData DefaultDecay { get; set; }
        [JsonProperty(PropertyName = "Decay - 2. Decay settings for building blocks")]
        public Dictionary<string, Dictionary<BuildingGrade.Enum, DecayData>> Buildings { get; set; }
        [JsonProperty(PropertyName = "Decay - 3. Decay settings for all other entities")]
        public Dictionary<string, DecayData> Entities { get; set; }
        [JsonProperty(PropertyName = "ZoneManager - Ignore decay-able entities inside of 'nodecay' zones")]
        public bool ZoneManager { get; set; }
        public class DecayData
        {
            [JsonProperty(PropertyName = "Inside of privilege")]
            public InternalRates InsidePrivilege { get; set; }
            [JsonProperty(PropertyName = "Outside of privilege")]
            public Rates OutsidePrivilege { get; set; }
            [JsonProperty(PropertyName = "Decay is enabled for this entity")]
            public bool IsEnabled { get; set; }
            public class Rates
            {
                [JsonProperty(PropertyName = "Damage per decay tick (% of max health)")]
                public float DamageRate { get; set; }
                [JsonProperty(PropertyName = "Time between decay passes")]
                public float DecayRate { get; set; }
            }

            public class InternalRates : Rates
            {
                [JsonProperty(PropertyName = "Use upkeep to handle decay when the TC has resources")]
                public bool UseUpkeep { get; set; }
            }

        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
}

private class DecayManager
{
    public BaseCombatEntity Entity { get; set; }
    private ConfigData.DecayData decayData;
    private bool isInPrivs;
    private bool isConstruction;
    private float decayRate;
    public DecayManager();
    public DecayManager(BaseCombatEntity entity, ConfigData.DecayData decayData);
    public void BeginDecay();
    private void SetInitialTimer();
    private void RunDecay();
    public bool DealDamage();
    public bool IsInPrivilege(bool reset);
    public void ResetDecayOnDestroy();
    public void ResetDecay(bool addRandom);
    public void CancelInvokes();
    public void UpdateDecayRates(ConfigData.DecayData decayData);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Activity - Player activity will reset nearby decay timers")]
    public bool MonitorActivity { get; set; }
    [JsonProperty(PropertyName = "Activity - Radius of effect")]
    public float ActivityRadius { get; set; }
    [JsonProperty(PropertyName = "Decay - 1. Default decay settings for all entities when plugin first loads")]
    public DecayData DefaultDecay { get; set; }
    [JsonProperty(PropertyName = "Decay - 2. Decay settings for building blocks")]
    public Dictionary<string, Dictionary<BuildingGrade.Enum, DecayData>> Buildings { get; set; }
    [JsonProperty(PropertyName = "Decay - 3. Decay settings for all other entities")]
    public Dictionary<string, DecayData> Entities { get; set; }
    [JsonProperty(PropertyName = "ZoneManager - Ignore decay-able entities inside of 'nodecay' zones")]
    public bool ZoneManager { get; set; }
    public class DecayData
    {
        [JsonProperty(PropertyName = "Inside of privilege")]
        public InternalRates InsidePrivilege { get; set; }
        [JsonProperty(PropertyName = "Outside of privilege")]
        public Rates OutsidePrivilege { get; set; }
        [JsonProperty(PropertyName = "Decay is enabled for this entity")]
        public bool IsEnabled { get; set; }
        public class Rates
        {
            [JsonProperty(PropertyName = "Damage per decay tick (% of max health)")]
            public float DamageRate { get; set; }
            [JsonProperty(PropertyName = "Time between decay passes")]
            public float DecayRate { get; set; }
        }

        public class InternalRates : Rates
        {
            [JsonProperty(PropertyName = "Use upkeep to handle decay when the TC has resources")]
            public bool UseUpkeep { get; set; }
        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class DecayData
{
    [JsonProperty(PropertyName = "Inside of privilege")]
    public InternalRates InsidePrivilege { get; set; }
    [JsonProperty(PropertyName = "Outside of privilege")]
    public Rates OutsidePrivilege { get; set; }
    [JsonProperty(PropertyName = "Decay is enabled for this entity")]
    public bool IsEnabled { get; set; }
    public class Rates
    {
        [JsonProperty(PropertyName = "Damage per decay tick (% of max health)")]
        public float DamageRate { get; set; }
        [JsonProperty(PropertyName = "Time between decay passes")]
        public float DecayRate { get; set; }
    }

    public class InternalRates : Rates
    {
        [JsonProperty(PropertyName = "Use upkeep to handle decay when the TC has resources")]
        public bool UseUpkeep { get; set; }
    }

}

public class Rates
{
    [JsonProperty(PropertyName = "Damage per decay tick (% of max health)")]
    public float DamageRate { get; set; }
    [JsonProperty(PropertyName = "Time between decay passes")]
    public float DecayRate { get; set; }
}

public class InternalRates : Rates
{
    [JsonProperty(PropertyName = "Use upkeep to handle decay when the TC has resources")]
    public bool UseUpkeep { get; set; }
}


```

---

## DeepWater

```csharp
using System;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Rust;
using Network;
using System.Runtime.CompilerServices;
using System.Collections.Generic;
using System.Resources;
using Facepunch;

Oxide.Plugins
[Info("DeepWater", "Colon Blow", "1.0.22")]
 class DeepWater : RustPlugin
{
    private bool initialized;
     void Loaded();
    private void OnServerInitialized();
     void LoadDefaultConfig();
    private void RestoreSavedRigs();
    private string gridLocation;
    static bool SpawnRandomOnLoad;
    static bool EnableAutoZoneOnRandom;
    static bool EnableAutoZoneOnLocal;
    static bool EnableAutoZoneOnCompound;
    static bool EnableAutoZoneOnRigEvent;
    static bool useStaticRandomID;
    static bool useStaticLocalID;
    static bool useStaticComoundID;
    static bool useStaticEventID;
    static string randomRigZoneID;
    static string localRigZoneID;
    static string compoundRigZoneID;
    static string eventRigZoneID;
    static string randomzoneargs;
    static string localzoneargs;
    static string compoundzoneargs;
    static string eventzoneargs;
    static int PumpStartingFuel;
    static uint HeliPadRugLogo1;
    static uint HeliPadRugLogo2;
    static bool UseSentryOnCompound;
     bool CanDamageRig;
     bool DestroyRandomOnReload;
     bool DestroyLocalOnReload;
     float RigSpawnHeight;
    static bool UseDespawnOnRandom;
    static bool UseDespawnOnLocal;
    static bool AutoMoveLiftBucket;
    static bool StandardRigHasLiftBucket;
    static bool StandardRigHasBarrels;
    static bool StandardRigHasLadders;
    static bool StandardRigHasLootSpawn;
    static float DespawnTime;
    static float LootRespawnTime;
    static int EventMinRestartTime;
    static int EventMaxRestartTime;
    static int MinPlayersForEventSpawn;
    static bool EventRigHasLiftBucket;
    static bool EventRigHasBarrels;
    static bool EventRigHasLadders;
    static bool EventRigHasHackCrate;
    static bool EventRigHasLootSpawn;
    static bool AddFullDivingKit;
    static int Stage1Duration;
    static int Stage2Duration;
    static float Stage2HackDuration;
    static int Stage3Duration;
    static int Stage3TimeBetweenBarrels;
    static float Stage3BarrelDamage;
    static int Stage4Duration;
    static float Stage4HackDefaultReset;
    static int MaxLootCratesToSpawn;
    static string LootPrefab1;
    static string LootPrefab2;
    static string LootPrefab3;
    static string LootPrefab4;
    static string LootPrefab5;
    static bool HasOilDeposit;
    static bool HasHQMetalDeposit;
    static bool HasSulfurDeposit;
    static bool HasMetalDeposit;
    static int OilDepositTickRate;
    static int HQMetalTickRate;
    static int SulfurOreTickRate;
    static int MetalOreTickRate;
    static bool BroadcastSpawn;
    static bool BroadcastEventSpawn;
    static bool BroadcastEventEnd;
    static bool BroadCastEventStages;
     int eventcounter;
     int eventrespawntime;
     bool Changed;
    static bool DoRandomRespawn;
     bool EventCountDown;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     string msg(string key, string playerId);
     Dictionary<string, string> messages;
    [ChatCommand("spawnrandomrig")]
     void cmdSpawnRandomRig(BasePlayer player, string command, string[] args);
    [ConsoleCommand("spawnrandomrig")]
     void cmdConsoleSpawnRandomRig(ConsoleSystem.Arg arg);
    [ChatCommand("spawnrandomrigevent")]
     void cmdSpawnRandomRigEvent(BasePlayer player, string command, string[] args);
    [ConsoleCommand("spawnrandomrigevent")]
     void cmdConsoleSpawnRandomRigEvent(ConsoleSystem.Arg arg);
    [ChatCommand("spawnrig")]
     void cmdSpawnRig(BasePlayer player, string command, string[] args);
    [ChatCommand("spawnrigevent")]
     void cmdSpawnRigEvent(BasePlayer player, string command, string[] args);
    [ChatCommand("destroyrig")]
     void cmdDestroyRig(BasePlayer player, string command, string[] args);
    [ChatCommand("clearrigdatabase")]
     void cmdChatClearRigDataBase(BasePlayer player, string command, string[] args);
    static List<BasePlayer> rigantihack;
     object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
    static Dictionary<ulong, PlayerRigCount> loadplayer;
    public class PlayerRigCount
    {
        public BasePlayer player;
        public int rigcount;
    }

    static StoredData storedData;
     DynamicConfigFile dataFile;
    public class StoredData
    {
        public Dictionary<uint, StoredRigData> saveRigData;
        public StoredData();
    }

    public class StoredRigData
    {
        public ulong ownerid;
        public string pos;
        public string eangles;
        public string rot;
        public int iscompound;
    }

     void LoadDataFile();
     void AddData(uint entnetid, ulong sownerid, string spos, string seangles, string srot, int compound);
     void RemoveData(uint entnetid);
     void SaveData();
    public static Vector3 StringToVector3(string sVector);
    public static Quaternion StringToQuaternion(string sVector);
    public void CreateZone(BaseEntity entity, string zoneidstr, string[] zoneargs);
    public void EraseZone(string zoneid);
    private int GetRandomTime();
     void OnTick();
     void SetAutoSpawnEnabled();
     void SpawnRandom();
     bool IsAllowed(BasePlayer player, string perm);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void OnEntityDeath(BaseCombatEntity target, HitInfo info);
    private object OnEntityGroundMissing(BaseEntity entity);
    private object CanBuild(Planner plan, Construction prefab, object obj);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
     Vector3 FindSpawnPoint();
     string GetGridLocation(Vector3 position);
    public void BuildRandomDeepWater(bool eventstarted, bool autospawned);
    public void BuildLocalDeepWater(BasePlayer player, bool eventstarted, int compound);
     void SpawnRig(ulong ownerid, Vector3 pos, Quaternion rot, Vector3 angle, int compound);
     void DestroyAll();
     void Unload();
    public class DeepWaterEntity : BaseEntity
    {
         DeepWater deepwater;
        public MiningQuarry pumpjackentity;
         BaseEntity entity;
         Vector3 entitypos;
         Quaternion entityrot;
         BaseEntity spawnEntity;
         BaseEntity refinerysmall;
         BaseEntity furnacelarge;
         BaseEntity windmill;
         BaseEntity lootspawn1;
         BaseEntity lootspawn2;
         BaseEntity lootspawn3;
         BaseEntity lootspawn4;
         BaseEntity lootspawn5;
         BaseEntity lootspawn6;
         BaseEntity lootspawn7;
         BaseEntity lootspawn8;
         BaseEntity leg1floorbase;
         BaseEntity leg1floor5wall1;
         BaseEntity leg1floor5wall2;
         BaseEntity leg1floor5wall3;
         BaseEntity leg1floor5wall4;
         BaseEntity leg1floor4wall1;
         BaseEntity leg1floor4wall2;
         BaseEntity leg1floor4wall3;
         BaseEntity leg1floor4wall4;
         BaseEntity leg1floor1wall1;
         BaseEntity leg1floor1wall2;
         BaseEntity leg1floor1wall3;
         BaseEntity leg1floor1wall4;
         BaseEntity leg1floor2wall1;
         BaseEntity leg1floor2wall2;
         BaseEntity leg1floor2wall3;
         BaseEntity leg1floor2wall4;
         BaseEntity leg1floor3wall1;
         BaseEntity leg1floor3wall2;
         BaseEntity leg1floor3wall3;
         BaseEntity leg1floor3wall4;
         BaseEntity leg2floorbase;
         BaseEntity leg2floor5wall1;
         BaseEntity leg2floor5wall2;
         BaseEntity leg2floor5wall3;
         BaseEntity leg2floor5wall4;
         BaseEntity leg2floor4wall1;
         BaseEntity leg2floor4wall2;
         BaseEntity leg2floor4wall3;
         BaseEntity leg2floor4wall4;
         BaseEntity leg2floor1wall1;
         BaseEntity leg2floor1wall2;
         BaseEntity leg2floor1wall3;
         BaseEntity leg2floor1wall4;
         BaseEntity leg2floor2wall1;
         BaseEntity leg2floor2wall2;
         BaseEntity leg2floor2wall3;
         BaseEntity leg2floor2wall4;
         BaseEntity leg2floor3wall1;
         BaseEntity leg2floor3wall2;
         BaseEntity leg2floor3wall3;
         BaseEntity leg2floor3wall4;
         BaseEntity leg3floorbase;
         BaseEntity leg3floor5wall1;
         BaseEntity leg3floor5wall2;
         BaseEntity leg3floor5wall3;
         BaseEntity leg3floor5wall4;
         BaseEntity leg3floor4wall1;
         BaseEntity leg3floor4wall2;
         BaseEntity leg3floor4wall3;
         BaseEntity leg3floor4wall4;
         BaseEntity leg3floor1wall1;
         BaseEntity leg3floor1wall2;
         BaseEntity leg3floor1wall3;
         BaseEntity leg3floor1wall4;
         BaseEntity leg3floor2wall1;
         BaseEntity leg3floor2wall2;
         BaseEntity leg3floor2wall3;
         BaseEntity leg3floor2wall4;
         BaseEntity leg3floor3wall1;
         BaseEntity leg3floor3wall2;
         BaseEntity leg3floor3wall3;
         BaseEntity leg3floor3wall4;
         BaseEntity leg4floorbase;
         BaseEntity leg4floor5wall1;
         BaseEntity leg4floor5wall2;
         BaseEntity leg4floor5wall3;
         BaseEntity leg4floor5wall4;
         BaseEntity leg4floor4wall1;
         BaseEntity leg4floor4wall2;
         BaseEntity leg4floor4wall3;
         BaseEntity leg4floor4wall4;
         BaseEntity leg4floor1wall1;
         BaseEntity leg4floor1wall2;
         BaseEntity leg4floor1wall3;
         BaseEntity leg4floor1wall4;
         BaseEntity leg4floor2wall1;
         BaseEntity leg4floor2wall2;
         BaseEntity leg4floor2wall3;
         BaseEntity leg4floor2wall4;
         BaseEntity leg4floor3wall1;
         BaseEntity leg4floor3wall2;
         BaseEntity leg4floor3wall3;
         BaseEntity leg4floor3wall4;
         BaseEntity deck1row1floor1;
         BaseEntity deck1row1floor2;
         BaseEntity deck1row1floor3;
         BaseEntity deck1row1floor4;
         BaseEntity deck1row1floor5;
         BaseEntity deck1row1floor6;
         BaseEntity deck1row1floor7;
         BaseEntity deck1row2floor1;
         BaseEntity deck1row2floor2;
         BaseEntity deck1row2floor3;
         BaseEntity deck1row2floor4;
         BaseEntity deck1row2floor5;
         BaseEntity deck1row2floor6;
         BaseEntity deck1row2floor7;
         BaseEntity deck1row3floor1;
         BaseEntity deck1row3floor2;
         BaseEntity deck1row3floor3;
         BaseEntity deck1row3floor4;
         BaseEntity deck1row3floor5;
         BaseEntity deck1row4floor1;
         BaseEntity deck1row4floor2;
         BaseEntity deck1row4floor3;
         BaseEntity deck1row4floor4;
         BaseEntity deck1row4floor5;
         BaseEntity helipadsupport1;
         BaseEntity helipadsupport2;
         BaseEntity helipadsupport3;
         BaseEntity helipadsupport4;
         BaseEntity helipadsupport5;
         BaseEntity deck1row2lowwall1;
         BaseEntity deck1row2lowwall2;
         BaseEntity deck1row2lowwall3;
         BaseEntity deck1row2lowwall4;
         BaseEntity deck1row2lowwall5;
         BaseEntity deck1row3lowwall1;
         BaseEntity deck1row3lowwall2;
         BaseEntity deck1row3lowwall3;
         BaseEntity deck1row3lowwall4;
         BaseEntity deck1row3lowwall5;
         BaseEntity deck1row4lowwall1;
         BaseEntity deck1row4lowwall2;
         BaseEntity deck1row4lowwall3;
         BaseEntity deck1row4lowwall4;
         BaseEntity deck1row4lowwall5;
         BaseEntity deck1row1wallframe1;
         BaseEntity deck1row1wallframe2;
         BaseEntity deck1row1wallframe3;
         BaseEntity deck1row1wallframe4;
         BaseEntity deck1row1wallframe5;
         BaseEntity deck1row2wallframe1;
         BaseEntity deck1row2wallframe2;
         BaseEntity deck1row2wallframe3;
         BaseEntity deck1row2wallframe4;
         BaseEntity deck1row2wallframe5;
         BaseEntity deck1row3wallframe1;
         BaseEntity deck1row3wallframe2;
         BaseEntity deck1row3wallframe3;
         BaseEntity deck1row3wallframe4;
         BaseEntity deck1row3wallframe5;
         BaseEntity deck1row4wallframe1;
         BaseEntity deck1row4wallframe2;
         BaseEntity deck1row4wallframe3;
         BaseEntity deck1row4wallframe4;
         BaseEntity deck1row4wallframe5;
         BaseEntity deck1row1floorframe1;
         BaseEntity deck1row1floorframe2;
         BaseEntity deck1row1floorframe3;
         BaseEntity deck1row1floorframe4;
         BaseEntity deck1row1floorframe5;
         BaseEntity deck1row1floorgrill1;
         BaseEntity deck1row1floorgrill2;
         BaseEntity deck1row1floorgrill3;
         BaseEntity deck1row1floorgrill4;
         BaseEntity deck1row1floorgrill5;
         BaseEntity deck1row2floorframe1;
         BaseEntity deck1row2floorframe2;
         BaseEntity deck1row2floorframe3;
         BaseEntity deck1row2floorframe4;
         BaseEntity deck1row2floorframe5;
         BaseEntity deck1row2floorgrill1;
         BaseEntity deck1row2floorgrill2;
         BaseEntity BucketLift;
         BaseEntity deck1row2floorgrill4;
         BaseEntity deck1row2floorgrill5;
         BaseEntity deck1row3floorframe1;
         BaseEntity deck1row3floorframe2;
         BaseEntity deck1row3floorframe3;
         BaseEntity deck1row3floorgrill1;
         BaseEntity deck1row3floorgrill2;
         BaseEntity deck1row3floorgrill3;
         BaseEntity deck1row4floorframe1;
         BaseEntity deck1row4floorframe2;
         BaseEntity deck1row4floorframe3;
         BaseEntity deck1row4floorgrill1;
         BaseEntity deck1row4floorgrill2;
         BaseEntity deck1row4floorgrill3;
         BaseEntity deck2frontrowfloor1;
         BaseEntity deck2frontrowfloor2;
         BaseEntity deck2frontrowfloor3;
         BaseEntity deck2frontrowfloor4;
         BaseEntity deck2frontrowfloor5;
         BaseEntity deck2row1floor1;
         BaseEntity deck2row1floor2;
         BaseEntity deck2row1floor3;
         BaseEntity deck2row1floor4;
         BaseEntity deck2row1floor5;
         BaseEntity deck2row2floor1;
         BaseEntity deck2row2floor2;
         BaseEntity deck2row2floor3;
         BaseEntity deck2row2floor4;
         BaseEntity deck2row2floor5;
         BaseEntity deck2row3floor1;
         BaseEntity deck2row3floor2;
         BaseEntity deck2row3floor3;
         BaseEntity deck2row4floor1;
         BaseEntity deck2row4floor2;
         BaseEntity deck2row4floor3;
         BaseEntity deck2center1;
         BaseEntity deck2center2;
         BaseEntity deck2center3;
         BaseEntity deck2center4;
         BaseEntity deck2center5;
         BaseEntity deck2center6;
         BaseEntity deck2center7;
         BaseEntity deck2center8;
         BaseEntity deck2center9;
         BaseEntity deck2backrowfloor1;
         BaseEntity deck2backrowfloor2;
         BaseEntity deck2backrowfloor3;
         BaseEntity deck2backrowfloor4;
         BaseEntity deck2backrowfloor5;
         BaseEntity helipadsupportframe1;
         BaseEntity helipadsupportframe2;
         BaseEntity helipadsupportframe3;
         BaseEntity helipadsupportframe4;
         BaseEntity helipadsupportframe5;
         BaseEntity deck2row2lowwall1;
         BaseEntity deck2row2lowwall2;
         BaseEntity deck2row2lowwall3;
         BaseEntity deck2row2lowwall4;
         BaseEntity deck2row2lowwall5;
         BaseEntity deck2row3lowwall1;
         BaseEntity deck2row3lowwall2;
         BaseEntity deck2row3lowwall3;
         BaseEntity deck2row3lowwall3U;
         BaseEntity deck2row3lowwall4;
         BaseEntity deck2row3lowwall5;
         BaseEntity deck2row4lowwall1;
         BaseEntity deck2row4lowwall2;
         BaseEntity deck2row4lowwall3;
         BaseEntity deck2row4lowwall3U;
         BaseEntity deck2row4lowwall4;
         BaseEntity deck2row4lowwall5;
         BaseEntity ladder1leg1;
         BaseEntity ladder2leg1;
         BaseEntity ladder3leg1;
         BaseEntity ladder4leg1;
         BaseEntity ladder5leg1;
         BaseEntity ladder6leg1;
         BaseEntity ladder1leg2;
         BaseEntity ladder2leg2;
         BaseEntity ladder3leg2;
         BaseEntity ladder4leg2;
         BaseEntity ladder5leg2;
         BaseEntity ladder6leg2;
         BaseEntity ladder1leg3;
         BaseEntity ladder2leg3;
         BaseEntity ladder3leg3;
         BaseEntity ladder4leg3;
         BaseEntity ladder5leg3;
         BaseEntity ladder6leg3;
         BaseEntity ladder1leg4;
         BaseEntity ladder2leg4;
         BaseEntity ladder3leg4;
         BaseEntity ladder4leg4;
         BaseEntity ladder5leg4;
         BaseEntity ladder6leg4;
         BaseEntity lowerlight1;
         BaseEntity lowerlight2;
         BaseEntity lowerlight3;
         BaseEntity lowerlight4;
         BaseEntity lowerlight5;
         BaseEntity lowerlight6;
         BaseEntity lowerlight7;
         BaseEntity lowerlight8;
         BaseEntity upperlight1;
         BaseEntity upperlight2;
         BaseEntity upperlight3;
         BaseEntity upperlight4;
         BaseEntity upperlight5;
         BaseEntity upperlight6;
         BaseEntity upperlight7;
         BaseEntity helipadfloor1;
         BaseEntity helipadfloor2;
         BaseEntity helipadfloor3;
         BaseEntity helipadfloor4;
         BaseEntity helipadfloor5;
         BaseEntity helipadfloor6;
         BaseEntity helipadfloor7;
         BaseEntity helipadfloor8;
         BaseEntity helipadfloor9;
         BaseEntity helipadfloor10;
         BaseEntity helipadfence1;
         BaseEntity helipadfence2;
         BaseEntity helipadfence3;
         BaseEntity helipadfence4;
         BaseEntity helipadfence5;
         BaseEntity helilogo1;
         BaseEntity helilogo2;
         BaseEntity helicopter;
         BaseEntity helicrate;
         BaseEntity explosivecrate1;
         BaseEntity explosivecrate2;
         BaseEntity explosivecrate3;
         BaseEntity explosivecrate4;
         BaseEntity explosivecrate5;
         BaseEntity explosivecrate6;
         BaseEntity explosivecrate7;
         BaseEntity explosivecrate8;
         BaseEntity explosivecrate9;
         BaseEntity explosivecrate10;
         BaseEntity vendattire;
         BaseEntity vendbuilding;
         BaseEntity vendcomponents;
         BaseEntity vendresources;
         BaseEntity vendtools;
         BaseEntity vendweapons;
         BaseEntity peacekeeper1;
         BaseEntity peacekeeper2;
         BaseEntity peacekeeper3;
         BaseEntity peacekeeper4;
         BaseEntity peacekeeper5;
         BaseEntity recycler1;
         BaseEntity recycler2;
         BaseEntity workbench;
         BaseEntity researchtable;
         BaseEntity waterwell;
         BoxCollider boxcollider;
         bool didspawnvariables;
         bool setactive;
         bool isrepairing;
        public bool iscompound;
        public bool israndom;
        public bool isevent;
        public bool isautospawn;
        public bool stage1;
        public int stage1time;
         int stage1counter;
        public bool stage2;
        public int stage2time;
         int stage2counter;
        public bool stage3;
         bool stage3startfire;
        public int stage3time;
        public int stage3barreltime;
         int stage3barrelcounter;
         int stage3counter;
        public bool stage4;
        public int stage4time;
         int stage4counter;
         float F1HeightOffset;
         float F2HeightOffset;
         float F3HeightOffset;
         float F4HeightOffset;
         float F5HeightOffset;
         float ShiftOffset;
         float UDHeightOffset;
         float LDHeightOffSet;
         int count;
         int despawncount;
         int liftcount;
         int maxlootcrates;
        public int currentlootcrates;
         BaseEntity mapmarker;
         uint entitynetid;
         string zoneid;
         bool dorigdestroy;
         string prefabfoundation;
         string prefabfoundationsteps;
         string prefabfloor;
         string prefabfloorframe;
         string prefabfloorgrill;
         string prefabwall;
         string prefabwallframe;
         string prefabwallhalf;
         string prefablowwall;
         string prefabstairsl;
         string prefabdoorway;
         string prefabpumpjack;
         string prefabladder;
         string prefabrefinerysmall;
         string prefabfurnacelarge;
         string prefabwindmill;
         string prefabnpc;
         string prefabrowboat;
         string prefabceilinglight;
         string prefabrug;
         string prefabfence;
         string prefabheli;
         string prefabexplosivecrate;
         string prefabfireball;
         string prefabattirevending;
         string prefabbuildingvending;
         string prefabcomponentsvending;
         string prefabresourcesvending;
         string prefabtoolsvending;
         string prefabweaponsvending;
         string prefabpeacekeeper;
         string prefabrecycler;
         string prefabworkbench;
         string prefabresearchtable;
         string prefabwaterwell;
         void Awake();
         string GetZoneID();
         void AddZone();
         void DespawnRig();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
         void CheckVariables();
         void AddMarker();
         void AddOilDeposit();
        private BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, uint skinid);
         void SpawnRefresh(BaseNetworkable entity1);
        public void RefreshAll();
         void SpawnPumpJack();
         void SpawnRefinery();
         void SpawnLeg1();
         void SpawnLeg2();
         void SpawnLeg3();
         void SpawnLeg4();
         void SpawnLowerDeck();
         void SpawnLowerDeckGrid();
         void SpawnUpperDeck();
         void SpawnHelipad();
         void SpawnLowerLights();
         void SpawnUpperLights();
         void SpawnLift();
         void SpawnLadders();
         void SpawnCompound();
         void DeepWaterStage1();
         void DeepWaterStage2();
         void AddWetSuitKit(BaseEntity entity);
         void SpawnRadBarrels();
         bool HasBarrels();
         void DeepWaterStage3();
         void DoBarrelExplosion(BaseEntity barrel);
         void BarrelExplosionDamage(Vector3 location);
         void DeepWaterStage4();
         void SpawnRigLoot();
         int DoLootCrateCount();
         void DoLootSpawn();
         BaseEntity SpawnLootBox(BaseEntity treasurebox, Vector3 spawnloc);
        public void MoveBucket();
         void BroadcastMessage(string str);
         void FixedUpdate();
         void DestroyLootBoxs();
         void SendDespawnMessage();
        public void OnDestroy();
    }

}

public class PlayerRigCount
{
    public BasePlayer player;
    public int rigcount;
}

public class StoredData
{
    public Dictionary<uint, StoredRigData> saveRigData;
    public StoredData();
}

public class StoredRigData
{
    public ulong ownerid;
    public string pos;
    public string eangles;
    public string rot;
    public int iscompound;
}

public class DeepWaterEntity : BaseEntity
{
     DeepWater deepwater;
    public MiningQuarry pumpjackentity;
     BaseEntity entity;
     Vector3 entitypos;
     Quaternion entityrot;
     BaseEntity spawnEntity;
     BaseEntity refinerysmall;
     BaseEntity furnacelarge;
     BaseEntity windmill;
     BaseEntity lootspawn1;
     BaseEntity lootspawn2;
     BaseEntity lootspawn3;
     BaseEntity lootspawn4;
     BaseEntity lootspawn5;
     BaseEntity lootspawn6;
     BaseEntity lootspawn7;
     BaseEntity lootspawn8;
     BaseEntity leg1floorbase;
     BaseEntity leg1floor5wall1;
     BaseEntity leg1floor5wall2;
     BaseEntity leg1floor5wall3;
     BaseEntity leg1floor5wall4;
     BaseEntity leg1floor4wall1;
     BaseEntity leg1floor4wall2;
     BaseEntity leg1floor4wall3;
     BaseEntity leg1floor4wall4;
     BaseEntity leg1floor1wall1;
     BaseEntity leg1floor1wall2;
     BaseEntity leg1floor1wall3;
     BaseEntity leg1floor1wall4;
     BaseEntity leg1floor2wall1;
     BaseEntity leg1floor2wall2;
     BaseEntity leg1floor2wall3;
     BaseEntity leg1floor2wall4;
     BaseEntity leg1floor3wall1;
     BaseEntity leg1floor3wall2;
     BaseEntity leg1floor3wall3;
     BaseEntity leg1floor3wall4;
     BaseEntity leg2floorbase;
     BaseEntity leg2floor5wall1;
     BaseEntity leg2floor5wall2;
     BaseEntity leg2floor5wall3;
     BaseEntity leg2floor5wall4;
     BaseEntity leg2floor4wall1;
     BaseEntity leg2floor4wall2;
     BaseEntity leg2floor4wall3;
     BaseEntity leg2floor4wall4;
     BaseEntity leg2floor1wall1;
     BaseEntity leg2floor1wall2;
     BaseEntity leg2floor1wall3;
     BaseEntity leg2floor1wall4;
     BaseEntity leg2floor2wall1;
     BaseEntity leg2floor2wall2;
     BaseEntity leg2floor2wall3;
     BaseEntity leg2floor2wall4;
     BaseEntity leg2floor3wall1;
     BaseEntity leg2floor3wall2;
     BaseEntity leg2floor3wall3;
     BaseEntity leg2floor3wall4;
     BaseEntity leg3floorbase;
     BaseEntity leg3floor5wall1;
     BaseEntity leg3floor5wall2;
     BaseEntity leg3floor5wall3;
     BaseEntity leg3floor5wall4;
     BaseEntity leg3floor4wall1;
     BaseEntity leg3floor4wall2;
     BaseEntity leg3floor4wall3;
     BaseEntity leg3floor4wall4;
     BaseEntity leg3floor1wall1;
     BaseEntity leg3floor1wall2;
     BaseEntity leg3floor1wall3;
     BaseEntity leg3floor1wall4;
     BaseEntity leg3floor2wall1;
     BaseEntity leg3floor2wall2;
     BaseEntity leg3floor2wall3;
     BaseEntity leg3floor2wall4;
     BaseEntity leg3floor3wall1;
     BaseEntity leg3floor3wall2;
     BaseEntity leg3floor3wall3;
     BaseEntity leg3floor3wall4;
     BaseEntity leg4floorbase;
     BaseEntity leg4floor5wall1;
     BaseEntity leg4floor5wall2;
     BaseEntity leg4floor5wall3;
     BaseEntity leg4floor5wall4;
     BaseEntity leg4floor4wall1;
     BaseEntity leg4floor4wall2;
     BaseEntity leg4floor4wall3;
     BaseEntity leg4floor4wall4;
     BaseEntity leg4floor1wall1;
     BaseEntity leg4floor1wall2;
     BaseEntity leg4floor1wall3;
     BaseEntity leg4floor1wall4;
     BaseEntity leg4floor2wall1;
     BaseEntity leg4floor2wall2;
     BaseEntity leg4floor2wall3;
     BaseEntity leg4floor2wall4;
     BaseEntity leg4floor3wall1;
     BaseEntity leg4floor3wall2;
     BaseEntity leg4floor3wall3;
     BaseEntity leg4floor3wall4;
     BaseEntity deck1row1floor1;
     BaseEntity deck1row1floor2;
     BaseEntity deck1row1floor3;
     BaseEntity deck1row1floor4;
     BaseEntity deck1row1floor5;
     BaseEntity deck1row1floor6;
     BaseEntity deck1row1floor7;
     BaseEntity deck1row2floor1;
     BaseEntity deck1row2floor2;
     BaseEntity deck1row2floor3;
     BaseEntity deck1row2floor4;
     BaseEntity deck1row2floor5;
     BaseEntity deck1row2floor6;
     BaseEntity deck1row2floor7;
     BaseEntity deck1row3floor1;
     BaseEntity deck1row3floor2;
     BaseEntity deck1row3floor3;
     BaseEntity deck1row3floor4;
     BaseEntity deck1row3floor5;
     BaseEntity deck1row4floor1;
     BaseEntity deck1row4floor2;
     BaseEntity deck1row4floor3;
     BaseEntity deck1row4floor4;
     BaseEntity deck1row4floor5;
     BaseEntity helipadsupport1;
     BaseEntity helipadsupport2;
     BaseEntity helipadsupport3;
     BaseEntity helipadsupport4;
     BaseEntity helipadsupport5;
     BaseEntity deck1row2lowwall1;
     BaseEntity deck1row2lowwall2;
     BaseEntity deck1row2lowwall3;
     BaseEntity deck1row2lowwall4;
     BaseEntity deck1row2lowwall5;
     BaseEntity deck1row3lowwall1;
     BaseEntity deck1row3lowwall2;
     BaseEntity deck1row3lowwall3;
     BaseEntity deck1row3lowwall4;
     BaseEntity deck1row3lowwall5;
     BaseEntity deck1row4lowwall1;
     BaseEntity deck1row4lowwall2;
     BaseEntity deck1row4lowwall3;
     BaseEntity deck1row4lowwall4;
     BaseEntity deck1row4lowwall5;
     BaseEntity deck1row1wallframe1;
     BaseEntity deck1row1wallframe2;
     BaseEntity deck1row1wallframe3;
     BaseEntity deck1row1wallframe4;
     BaseEntity deck1row1wallframe5;
     BaseEntity deck1row2wallframe1;
     BaseEntity deck1row2wallframe2;
     BaseEntity deck1row2wallframe3;
     BaseEntity deck1row2wallframe4;
     BaseEntity deck1row2wallframe5;
     BaseEntity deck1row3wallframe1;
     BaseEntity deck1row3wallframe2;
     BaseEntity deck1row3wallframe3;
     BaseEntity deck1row3wallframe4;
     BaseEntity deck1row3wallframe5;
     BaseEntity deck1row4wallframe1;
     BaseEntity deck1row4wallframe2;
     BaseEntity deck1row4wallframe3;
     BaseEntity deck1row4wallframe4;
     BaseEntity deck1row4wallframe5;
     BaseEntity deck1row1floorframe1;
     BaseEntity deck1row1floorframe2;
     BaseEntity deck1row1floorframe3;
     BaseEntity deck1row1floorframe4;
     BaseEntity deck1row1floorframe5;
     BaseEntity deck1row1floorgrill1;
     BaseEntity deck1row1floorgrill2;
     BaseEntity deck1row1floorgrill3;
     BaseEntity deck1row1floorgrill4;
     BaseEntity deck1row1floorgrill5;
     BaseEntity deck1row2floorframe1;
     BaseEntity deck1row2floorframe2;
     BaseEntity deck1row2floorframe3;
     BaseEntity deck1row2floorframe4;
     BaseEntity deck1row2floorframe5;
     BaseEntity deck1row2floorgrill1;
     BaseEntity deck1row2floorgrill2;
     BaseEntity BucketLift;
     BaseEntity deck1row2floorgrill4;
     BaseEntity deck1row2floorgrill5;
     BaseEntity deck1row3floorframe1;
     BaseEntity deck1row3floorframe2;
     BaseEntity deck1row3floorframe3;
     BaseEntity deck1row3floorgrill1;
     BaseEntity deck1row3floorgrill2;
     BaseEntity deck1row3floorgrill3;
     BaseEntity deck1row4floorframe1;
     BaseEntity deck1row4floorframe2;
     BaseEntity deck1row4floorframe3;
     BaseEntity deck1row4floorgrill1;
     BaseEntity deck1row4floorgrill2;
     BaseEntity deck1row4floorgrill3;
     BaseEntity deck2frontrowfloor1;
     BaseEntity deck2frontrowfloor2;
     BaseEntity deck2frontrowfloor3;
     BaseEntity deck2frontrowfloor4;
     BaseEntity deck2frontrowfloor5;
     BaseEntity deck2row1floor1;
     BaseEntity deck2row1floor2;
     BaseEntity deck2row1floor3;
     BaseEntity deck2row1floor4;
     BaseEntity deck2row1floor5;
     BaseEntity deck2row2floor1;
     BaseEntity deck2row2floor2;
     BaseEntity deck2row2floor3;
     BaseEntity deck2row2floor4;
     BaseEntity deck2row2floor5;
     BaseEntity deck2row3floor1;
     BaseEntity deck2row3floor2;
     BaseEntity deck2row3floor3;
     BaseEntity deck2row4floor1;
     BaseEntity deck2row4floor2;
     BaseEntity deck2row4floor3;
     BaseEntity deck2center1;
     BaseEntity deck2center2;
     BaseEntity deck2center3;
     BaseEntity deck2center4;
     BaseEntity deck2center5;
     BaseEntity deck2center6;
     BaseEntity deck2center7;
     BaseEntity deck2center8;
     BaseEntity deck2center9;
     BaseEntity deck2backrowfloor1;
     BaseEntity deck2backrowfloor2;
     BaseEntity deck2backrowfloor3;
     BaseEntity deck2backrowfloor4;
     BaseEntity deck2backrowfloor5;
     BaseEntity helipadsupportframe1;
     BaseEntity helipadsupportframe2;
     BaseEntity helipadsupportframe3;
     BaseEntity helipadsupportframe4;
     BaseEntity helipadsupportframe5;
     BaseEntity deck2row2lowwall1;
     BaseEntity deck2row2lowwall2;
     BaseEntity deck2row2lowwall3;
     BaseEntity deck2row2lowwall4;
     BaseEntity deck2row2lowwall5;
     BaseEntity deck2row3lowwall1;
     BaseEntity deck2row3lowwall2;
     BaseEntity deck2row3lowwall3;
     BaseEntity deck2row3lowwall3U;
     BaseEntity deck2row3lowwall4;
     BaseEntity deck2row3lowwall5;
     BaseEntity deck2row4lowwall1;
     BaseEntity deck2row4lowwall2;
     BaseEntity deck2row4lowwall3;
     BaseEntity deck2row4lowwall3U;
     BaseEntity deck2row4lowwall4;
     BaseEntity deck2row4lowwall5;
     BaseEntity ladder1leg1;
     BaseEntity ladder2leg1;
     BaseEntity ladder3leg1;
     BaseEntity ladder4leg1;
     BaseEntity ladder5leg1;
     BaseEntity ladder6leg1;
     BaseEntity ladder1leg2;
     BaseEntity ladder2leg2;
     BaseEntity ladder3leg2;
     BaseEntity ladder4leg2;
     BaseEntity ladder5leg2;
     BaseEntity ladder6leg2;
     BaseEntity ladder1leg3;
     BaseEntity ladder2leg3;
     BaseEntity ladder3leg3;
     BaseEntity ladder4leg3;
     BaseEntity ladder5leg3;
     BaseEntity ladder6leg3;
     BaseEntity ladder1leg4;
     BaseEntity ladder2leg4;
     BaseEntity ladder3leg4;
     BaseEntity ladder4leg4;
     BaseEntity ladder5leg4;
     BaseEntity ladder6leg4;
     BaseEntity lowerlight1;
     BaseEntity lowerlight2;
     BaseEntity lowerlight3;
     BaseEntity lowerlight4;
     BaseEntity lowerlight5;
     BaseEntity lowerlight6;
     BaseEntity lowerlight7;
     BaseEntity lowerlight8;
     BaseEntity upperlight1;
     BaseEntity upperlight2;
     BaseEntity upperlight3;
     BaseEntity upperlight4;
     BaseEntity upperlight5;
     BaseEntity upperlight6;
     BaseEntity upperlight7;
     BaseEntity helipadfloor1;
     BaseEntity helipadfloor2;
     BaseEntity helipadfloor3;
     BaseEntity helipadfloor4;
     BaseEntity helipadfloor5;
     BaseEntity helipadfloor6;
     BaseEntity helipadfloor7;
     BaseEntity helipadfloor8;
     BaseEntity helipadfloor9;
     BaseEntity helipadfloor10;
     BaseEntity helipadfence1;
     BaseEntity helipadfence2;
     BaseEntity helipadfence3;
     BaseEntity helipadfence4;
     BaseEntity helipadfence5;
     BaseEntity helilogo1;
     BaseEntity helilogo2;
     BaseEntity helicopter;
     BaseEntity helicrate;
     BaseEntity explosivecrate1;
     BaseEntity explosivecrate2;
     BaseEntity explosivecrate3;
     BaseEntity explosivecrate4;
     BaseEntity explosivecrate5;
     BaseEntity explosivecrate6;
     BaseEntity explosivecrate7;
     BaseEntity explosivecrate8;
     BaseEntity explosivecrate9;
     BaseEntity explosivecrate10;
     BaseEntity vendattire;
     BaseEntity vendbuilding;
     BaseEntity vendcomponents;
     BaseEntity vendresources;
     BaseEntity vendtools;
     BaseEntity vendweapons;
     BaseEntity peacekeeper1;
     BaseEntity peacekeeper2;
     BaseEntity peacekeeper3;
     BaseEntity peacekeeper4;
     BaseEntity peacekeeper5;
     BaseEntity recycler1;
     BaseEntity recycler2;
     BaseEntity workbench;
     BaseEntity researchtable;
     BaseEntity waterwell;
     BoxCollider boxcollider;
     bool didspawnvariables;
     bool setactive;
     bool isrepairing;
    public bool iscompound;
    public bool israndom;
    public bool isevent;
    public bool isautospawn;
    public bool stage1;
    public int stage1time;
     int stage1counter;
    public bool stage2;
    public int stage2time;
     int stage2counter;
    public bool stage3;
     bool stage3startfire;
    public int stage3time;
    public int stage3barreltime;
     int stage3barrelcounter;
     int stage3counter;
    public bool stage4;
    public int stage4time;
     int stage4counter;
     float F1HeightOffset;
     float F2HeightOffset;
     float F3HeightOffset;
     float F4HeightOffset;
     float F5HeightOffset;
     float ShiftOffset;
     float UDHeightOffset;
     float LDHeightOffSet;
     int count;
     int despawncount;
     int liftcount;
     int maxlootcrates;
    public int currentlootcrates;
     BaseEntity mapmarker;
     uint entitynetid;
     string zoneid;
     bool dorigdestroy;
     string prefabfoundation;
     string prefabfoundationsteps;
     string prefabfloor;
     string prefabfloorframe;
     string prefabfloorgrill;
     string prefabwall;
     string prefabwallframe;
     string prefabwallhalf;
     string prefablowwall;
     string prefabstairsl;
     string prefabdoorway;
     string prefabpumpjack;
     string prefabladder;
     string prefabrefinerysmall;
     string prefabfurnacelarge;
     string prefabwindmill;
     string prefabnpc;
     string prefabrowboat;
     string prefabceilinglight;
     string prefabrug;
     string prefabfence;
     string prefabheli;
     string prefabexplosivecrate;
     string prefabfireball;
     string prefabattirevending;
     string prefabbuildingvending;
     string prefabcomponentsvending;
     string prefabresourcesvending;
     string prefabtoolsvending;
     string prefabweaponsvending;
     string prefabpeacekeeper;
     string prefabrecycler;
     string prefabworkbench;
     string prefabresearchtable;
     string prefabwaterwell;
     void Awake();
     string GetZoneID();
     void AddZone();
     void DespawnRig();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
     void CheckVariables();
     void AddMarker();
     void AddOilDeposit();
    private BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, uint skinid);
     void SpawnRefresh(BaseNetworkable entity1);
    public void RefreshAll();
     void SpawnPumpJack();
     void SpawnRefinery();
     void SpawnLeg1();
     void SpawnLeg2();
     void SpawnLeg3();
     void SpawnLeg4();
     void SpawnLowerDeck();
     void SpawnLowerDeckGrid();
     void SpawnUpperDeck();
     void SpawnHelipad();
     void SpawnLowerLights();
     void SpawnUpperLights();
     void SpawnLift();
     void SpawnLadders();
     void SpawnCompound();
     void DeepWaterStage1();
     void DeepWaterStage2();
     void AddWetSuitKit(BaseEntity entity);
     void SpawnRadBarrels();
     bool HasBarrels();
     void DeepWaterStage3();
     void DoBarrelExplosion(BaseEntity barrel);
     void BarrelExplosionDamage(Vector3 location);
     void DeepWaterStage4();
     void SpawnRigLoot();
     int DoLootCrateCount();
     void DoLootSpawn();
     BaseEntity SpawnLootBox(BaseEntity treasurebox, Vector3 spawnloc);
    public void MoveBucket();
     void BroadcastMessage(string str);
     void FixedUpdate();
     void DestroyLootBoxs();
     void SendDespawnMessage();
    public void OnDestroy();
}


```

---

## DemolishLimiter

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Demolish Limiter", "Mughisi", "1.0.1")]
 class DemolishLimiter : RustPlugin
{
    private bool configChanged;
     string defaultChatPrefix;
     string defaultChatPrefixColor;
     string chatPrefix;
     string chatPrefixColor;
     bool defaultAdminCanDemolish;
     bool defaultModeratorCanDemolish;
     bool defaultLogDemolishToConsole;
     bool adminCanDemolish;
     bool moderatorCanDemolish;
     bool logToConsole;
     string defaultNotAllowed;
     string notAllowed;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadConfigValues();
     object OnBuildingBlockDemolish(BuildingBlock block, BasePlayer player);
     void Log(string message);
     void SendChatMessage(BasePlayer player, string message, string arguments);
     object GetConfigValue(string category, string setting, object defaultValue);
     void SetConfigValue(string category, string setting, object newValue);
}


```

---

## DevilsIsland

```csharp
using DevilsIsland;
using JetStream;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Plugins;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Devil's Island", "Nick Holmes", 0.7, ResourceId = 1372)]
[Description("Devil's Island Game Mode")]
public class DevilsIsland : GameModePlugin<DevilsIslandConfig, DevilsIslandState>
{
    private ILocator liveLocator;
    private ILocator locator;
    protected override void Initialize();
     void AdviseBossPosition();
    public void AdviseRules();
    public void TryForceBoss();
    [ChatCommand("rules")]
     void RulesCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("status")]
     void StatusCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("claim")]
     void ClaimCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("loot")]
     void LootCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("helo")]
     void HeloCommmand(BasePlayer player, string command, string[] args);
    [ChatCommand("decoy")]
     void DecoyCommmand(BasePlayer player, string command, string[] args);
    [ChatCommand("tax")]
     void SetTaxCommmand(BasePlayer player, string command, string[] args);
    [ChatCommand("where")]
     void WhereCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("rebel")]
     void RebelCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("recruit")]
     void RecruitCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("accept")]
     void AcceptCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("devilsisland.diagnostic")]
    private void DiagnosticCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("devilsisland.reset")]
    private void ResetCommand(ConsoleSystem.Arg arg);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnQuarryGather(MiningQuarry quarry, Item item);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    public BasePlayer Boss { get; set; }
}

DevilsIsland
public class DevilsIslandState : IGameState
{
    public DevilsIslandState();
    public void AttachConfig(IGameConfig configFile);
    private DevilsIslandConfig config;
    private BasePlayer currentBoss;
    internal DateTime noBossSince;
    private float taxRate;
    private Dictionary<ItemDefinition, float> coffers;
    public bool NoBoss();
    public bool BossExists();
    public bool IsBoss(BasePlayer player);
    public bool IsNotBoss(BasePlayer player);
    public BasePlayer Boss { get; set; }
    public string BossName { get; set; }
    public void SetBoss(BasePlayer newBoss);
    public bool TryForceNewBoss();
    public float TaxRate { get; set; }
    public StorageContainer LootContainer { get; set; }
    public IEnumerable<ItemDefinition> CofferItems { get; set; }
    public float CofferAmount(ItemDefinition key);
    public void CollectTaxFrom(Item item);
    public bool CanNotAffordHeloStrike(BasePlayer player);
    public void OrderHeloStrike(string targetPartialName);
    public Outlaws Outlaws { get; set; }
    public bool CanAffordDecoy(BasePlayer player);
    public void Decoy(BasePlayer player, string targetPartialName);
    public Henchmen Henchmen { get; set; }
    public List<BasePlayer> PendingRequest { get; set; }
    public BasePlayer TryRecruit(string playerPartialName);
    public bool TryPromote(BasePlayer player);
    public bool PlayerGoodMatch(string playerPartialName);
}

public class Outlaws
{
     List<Outlaw> outlaws;
    public void Clear();
    public void Add(BasePlayer newOutlaw);
    public void Remove(BasePlayer oldOutlaw);
    public bool Contains(BasePlayer player);
    public bool Any();
    public IEnumerable<Outlaw> All();
    public bool TryResovleByPlayer(BasePlayer player, Outlaw matchingOutlaw);
    public bool HasMatchByPartialName(string partialName);
    public bool TryResolveByPartialName(string partialName, Outlaw matchingOutlaw);
}

public class Outlaw
{
    public Outlaw(BasePlayer player);
    public BasePlayer Player { get; set; }
    private BasePlayer decoyTarget;
    private DateTime decoyUntil;
    public void SetDecoy(BasePlayer player, DateTime until);
    public BasePlayer GetEffectiveTarget();
}

public class Henchmen
{
     List<Henchman> henchmen;
    public void Clear();
    public void Add(BasePlayer newHenchman);
    public void Remove(BasePlayer oldHenchman);
    public bool Contains(BasePlayer player);
    public bool Any();
    public IEnumerable<Henchman> All();
    public bool TryResovleByPlayer(BasePlayer player, Henchman matchingHenchman);
    public bool HasMatchByPartialName(string partialName);
    public bool TryResolveByPartialName(string partialName, Henchman matchingHenchman);
}

public class Henchman
{
    public Henchman(BasePlayer player);
    public BasePlayer Player { get; set; }
}

public class DevilsIslandConfig : IGameConfig
{
    public void AttachConfigFile(DynamicConfigFile configFile);
    private DynamicConfigFile configFile;
    public void UpdateConfigFile();
    private bool DefaultValue(string key, object defaultValue);
    public bool IsHelpNotiferEnabled { get; set; }
    public int HelpNotifierInverval { get; set; }
    public bool IsBossPositionNotifierEnabled { get; set; }
    public int BossPositionNotifierInterval { get; set; }
    public int AutoBossPromoteDelay { get; set; }
    public int AutoBossPromoteMinPlayers { get; set; }
    private ItemDefinition heloStrikePriceItemDef;
    public ItemDefinition HeloStrikePrice_ItemDef { get; set; }
    public int HeloStrikePrice_Quantity { get; set; }
    public int MaxHelos { get; set; }
    private ItemDefinition decoyPriceItemDef;
    public ItemDefinition DecoyPrice_ItemId { get; set; }
    public int DecoyPrice_Quantity { get; set; }
    private ItemDefinition evadePriceItemDef;
    public ItemDefinition EvadePrice_ItemId { get; set; }
    public int EvadePrice_Quantity { get; set; }
    public int FallbackWorldSize { get; set; }
    private static ItemDefinition FindOrDefault(string itemName, string fallbackName);
    public bool AllowBossDisconnect { get; set; }
}

public class RustIOLocator : ILocator
{
    public RustIOLocator(int worldSize);
    private readonly float translate;
    private readonly float scale;
    public string GridReference(Component component, bool moved);
}

public class LocatorWithDelay : ILocator
{
    public LocatorWithDelay(ILocator liveLocator, int updateInterval);
    private readonly ILocator liveLocator;
    private readonly int updateInterval;
    private readonly Dictionary<Component, ExpiringCoordinates> locations;
    public string GridReference(Component component, bool moved);
     class ExpiringCoordinates
    {
        public string Location { get; set; }
        public bool GridChanged { get; set; }
        public DateTime Expires { get; set; }
    }

}

 class ExpiringCoordinates
{
    public string Location { get; set; }
    public bool GridChanged { get; set; }
    public DateTime Expires { get; set; }
}

public static class Text
{
    public const string NoBoss;
    public const string YouAreTheBoss;
    public const string YouAreAnOutlaw;
    public const string CurrentBoss;
    public const string CurrentTaxBox;
    public const string TaxRate;
    public const string CofferItem;
    public const string YourLocation;
    public const string Synopsis;
    public const string PlayerCommandSection;
    public const string StatusCommandHint;
    public const string ClaimCommandHint;
    public const string RebelCommandHint;
    public const string WhereCommandHint;
    public const string BossCommandSection;
    public const string TaxCommandHint;
    public const string LootCommandHint;
    public const string HeloStrikeCommandHint;
    public const string RecruitCommandHint;
    public const string Broadcast_ClaimAvailable;
    public const string Broadcast_HelpAdvice;
    public const string Error_YourAreBoss;
    public const string Error_OtherIsBoss;
    public const string Success_WelcomeNewBoss;
    public const string Broadcast_FirstBoss;
    public const string Error_Loot_NoBoss;
    public const string Error_Loot_NotBoss;
    public const string Success_Looting;
    public const string Error_TaxChange_NoBoss;
    public const string Error_TaxChange_NotBoss;
    public const string Error_TaxChange_BadArgs;
    public const string Broadcast_TaxIncrease;
    public const string Broadcast_TaxDecrease;
    public const string Error_Rebel_IsTheBoss;
    public const string Error_Rebel_IsAlreadyOutlaw;
    public const string Broadcast_NewOutlaw;
    public const string Broadcast_BossSuicide;
    public const string Broadcast_NewBoss;
    public const string Broadcast_BossLocation_Moved;
    public const string Broadcast_BossLocation_Static;
}

JetStream
public class GameModePlugin : RustPlugin
{
    private Dictionary<string, Timer> timers;
    protected Dictionary<string, Timer> Timers { get; set; }
    protected TConfig GameConfig { get; set; }
    protected TState State { get; set; }
    protected override void LoadDefaultConfig();
    [HookMethod("OnServerInitialized")]
     void base_OnServerInitialized();
    [HookMethod("Unload")]
     void base_Unload();
    protected virtual void Initialize();
    protected bool GuardAgainst(Func<bool> condition, BasePlayer player, string errorMsgFormat, object[] args);
}


```

---

## DiscordClans

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Messages.Embeds;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Logging;
using System;
using System.Linq;

Oxide.Plugins
[Info("DiscordClans", "k1lly0u", "0.1.2"), Description("Log ClansReborn events to Discord")]
 class DiscordClans : RustPlugin
{
    [DiscordClient]
    private DiscordClient Client;
    private DiscordGuild Guild;
    private bool ConnectionExists { get; set; }
    private void Unload();
    private void OnDiscordClientCreated();
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void LogMessage(string message, int messageType);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Discord Settings")]
        public DiscordSettings Discord { get; set; }
        [JsonProperty(PropertyName = "Log Settings")]
        public Hash<MessageType, LogSettings> Log { get; set; }
        public class DiscordSettings
        {
            [JsonProperty(PropertyName = "Bot Token")]
            public string APIKey { get; set; }
            [JsonProperty(PropertyName = "Bot Client ID")]
            public string BotID { get; set; }
            [JsonConverter(typeof(StringEnumConverter))]
            [JsonProperty(PropertyName = "Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
            public DiscordLogLevel LogLevel { get; set; }
        }

        public class LogSettings
        {
            [JsonProperty(PropertyName = "Logs enabled for this message type")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Log Channel Name")]
            public string Channel { get; set; }
            [JsonProperty(PropertyName = "Embed Color (hex)")]
            public string Color { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Discord Settings")]
    public DiscordSettings Discord { get; set; }
    [JsonProperty(PropertyName = "Log Settings")]
    public Hash<MessageType, LogSettings> Log { get; set; }
    public class DiscordSettings
    {
        [JsonProperty(PropertyName = "Bot Token")]
        public string APIKey { get; set; }
        [JsonProperty(PropertyName = "Bot Client ID")]
        public string BotID { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel LogLevel { get; set; }
    }

    public class LogSettings
    {
        [JsonProperty(PropertyName = "Logs enabled for this message type")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Log Channel Name")]
        public string Channel { get; set; }
        [JsonProperty(PropertyName = "Embed Color (hex)")]
        public string Color { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class DiscordSettings
{
    [JsonProperty(PropertyName = "Bot Token")]
    public string APIKey { get; set; }
    [JsonProperty(PropertyName = "Bot Client ID")]
    public string BotID { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel LogLevel { get; set; }
}

public class LogSettings
{
    [JsonProperty(PropertyName = "Logs enabled for this message type")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Log Channel Name")]
    public string Channel { get; set; }
    [JsonProperty(PropertyName = "Embed Color (hex)")]
    public string Color { get; set; }
}


```

---

## DiscordCore

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Builders.MessageComponents;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Entities.Applications;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Emojis;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Interactions;
using Oxide.Ext.Discord.Entities.Interactions.ApplicationCommands;
using Oxide.Ext.Discord.Entities.Interactions.MessageComponents;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Users;
using Oxide.Ext.Discord.Extensions;
using Oxide.Ext.Discord.Helpers;
using Oxide.Ext.Discord.Libraries.Command;
using Oxide.Ext.Discord.Libraries.Linking;
using Oxide.Ext.Discord.Logging;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("Discord Core", "MJSU", "2.1.3")]
[Description("Creates a link between a player and discord")]
internal class DiscordCore : CovalencePlugin, IDiscordLinkPlugin
{
    [DiscordClient]
    private DiscordClient _client;
    private StoredData _storedData;
    private PluginConfig _pluginConfig;
    private const string AccentColor;
    private const string UsePermission;
    private char[] _linkChars;
    private DiscordUser _bot;
    private DiscordGuild _guild;
    private bool _initialized;
    private readonly DiscordSettings _discordSettings;
    private readonly Hash<string, BanInfo> _banList;
    private readonly List<LinkActivation> _activations;
    private readonly List<Snowflake> _allowedCommandChannels;
    private string _allowedChannelNames;
    private char _cmdPrefix;
    private readonly DiscordLink _link;
    private readonly DiscordCommand _dcCommands;
    private const string AcceptEmoji;
    private const string DeclineEmoji;
    private const string LinkAccountsButtonId;
    private const string MessageLinkAccountsButtonId;
    private const string AcceptLinkButtonId;
    private const string DeclineLinkButtonId;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    public PluginConfig AdditionalConfig(PluginConfig config);
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnPluginLoaded(Plugin plugin);
    private void Unload();
    public void DiscordCoreReady(Plugin plugin);
    private void DiscordCoreChatCommand(IPlayer player, string cmd, string[] args);
    public void DisplayHelp(IPlayer player);
    public void HandleChatJoin(IPlayer player, string[] args);
    public void HandleChatJoinWithCode(IPlayer player);
    private void HandleChatJoinWithUserName(IPlayer player, string search);
    private void HandleChatJoinUserResults(IPlayer player, List<GuildMember> members, string userName, string discriminator);
    private void HandleChatJoinUser(IPlayer player, DiscordUser user);
    public void HandleChatCompleteLink(IPlayer player, string[] args);
    private void HandleDebugWelcome(IPlayer player, string[] args);
    private void DiscordCoreMessageCommand(DiscordMessage message, string cmd, string[] args);
    public void DisplayDiscordHelp(DiscordMessage message, IPlayer player);
    public void HandleDiscordJoin(DiscordMessage message, IPlayer player);
    public void HandleDiscordCompleteLink(DiscordMessage message, string[] args);
    [HookMethod(DiscordExtHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMembersLoaded)]
    private void OnDiscordGuildMembersLoaded(DiscordGuild guild);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberAdded)]
    private void OnDiscordGuildMemberAdded(GuildMember member);
    private void SendWelcomeMessage(Snowflake discordId);
    [HookMethod(DiscordExtHooks.OnDiscordGuildMemberRemoved)]
    private void OnDiscordGuildMemberRemoved(GuildMember member);
    [HookMethod(DiscordExtHooks.OnDiscordInteractionCreated)]
    private void OnDiscordInteractionCreated(DiscordInteraction interaction);
    public void HandleLinkAccountsButton(DiscordInteraction interaction, bool ephemeral);
    private void HandleAcceptLinkButton(DiscordInteraction interaction, DiscordUser user);
    private void HandleDeclineLinkButton(DiscordInteraction interaction, DiscordUser user);
    public void SetupGuildLinkMessage();
    public MessageCreate CreateGuildLinkMessage(string content);
    public List<ActionRowComponent> CreateGuildLinkActions();
    public void SaveGuildLinkMessageInfo(DiscordMessage message);
    public void CompletedLink(LinkActivation activation);
    public void HandleLeave(IPlayer player, DiscordUser user, bool backup, bool message);
    public bool HandleRejoin(DiscordUser user);
    public void HandleLeaveRejoin();
    public IDictionary<string, Snowflake> GetSteamToDiscordIds();
    [HookMethod(nameof(GetDiscordServerName))]
    public string GetDiscordServerName();
    public string GetDiscordFormattedMessage(string key, IPlayer player, object[] args);
    private bool IsDiscordCoreOnline();
    public string GenerateCode();
    public BanInfo GetPlayerBanInfo(string id);
    public void SaveData();
    public void Chat(IPlayer player, string key, object[] args);
    public string Lang(string key, IPlayer player, object[] args);
    public void RegisterChatLangCommand(string command, string langKey);
    public void RegisterDiscordLangCommand(string command, string langKey, bool direct, bool guild, List<Snowflake> allowedChannels);
    public class PluginConfig
    {
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string ApiKey { get; set; }
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Server Name Override")]
        public string ServerNameOverride { get; set; }
        [DefaultValue("")]
        [JsonProperty(PropertyName = "Discord Server Join Code")]
        public string JoinCode { get; set; }
        [JsonProperty(PropertyName = "Welcome Message Settings")]
        public WelcomeMessageSettings WelcomeMessageSettings { get; set; }
        [JsonProperty(PropertyName = "Link Settings")]
        public DiscordLinkingSettings LinkSettings { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class DiscordLinkingSettings
    {
        [JsonProperty(PropertyName = "Link Code Generator Characters")]
        public string LinkCodeCharacters { get; set; }
        [JsonProperty(PropertyName = "Link Code Length")]
        public int LinkCodeLength { get; set; }
        [JsonProperty(PropertyName = "Automatically Relink A Player If They Leave And Rejoin The Discord Server")]
        public bool AutoRelinkPlayer { get; set; }
        [JsonProperty(PropertyName = "Allow Commands To Be Used In Guild Channels")]
        public bool AllowCommandsInGuild { get; set; }
        [JsonProperty(PropertyName = "Allow Guild Commands Only In The Following Guild Channel Or Category (Channel ID Or Category ID)")]
        public List<Snowflake> AllowCommandInChannels { get; set; }
        [JsonProperty(PropertyName = "Link / Unlink Announcement Channel Id")]
        public Snowflake AnnouncementChannel { get; set; }
        [JsonProperty(PropertyName = "Guild Link Message Settings")]
        public LinkMessageSettings LinkMessageSettings { get; set; }
        [JsonProperty(PropertyName = "Link Ban Settings")]
        public LinkBanSettings LinkBanSettings { get; set; }
        public DiscordLinkingSettings(DiscordLinkingSettings settings);
    }

    public class WelcomeMessageSettings
    {
        [JsonProperty(PropertyName = "Enable Discord Server Welcome DM Message")]
        public bool EnableJoinMessage { get; set; }
        [JsonProperty(PropertyName = "Add Link Accounts Button In Welcome Message")]
        public bool EnableLinkButton { get; set; }
        public WelcomeMessageSettings(WelcomeMessageSettings settings);
    }

    public class LinkMessageSettings
    {
        [JsonProperty(PropertyName = "Enable Guild Link Message")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Message Channel ID")]
        public Snowflake ChannelId { get; set; }
        public LinkMessageSettings(LinkMessageSettings settings);
    }

    public class LinkBanSettings
    {
        [JsonProperty(PropertyName = "Enable Link Ban")]
        public bool EnableLinkBanning { get; set; }
        [JsonProperty(PropertyName = "Ban Link After X Join Declines")]
        public int BanDeclineAmount { get; set; }
        [JsonProperty(PropertyName = "Ban Duration (Hours)")]
        public int BanDuration { get; set; }
        public LinkBanSettings(LinkBanSettings settings);
    }

    public class StoredData
    {
        public Hash<string, DiscordInfo> PlayerDiscordInfo;
        public Hash<Snowflake, DiscordInfo> LeftPlayerInfo;
        public LinkMessageData MessageData;
    }

    public class DiscordInfo
    {
        public Snowflake DiscordId { get; set; }
        public string PlayerId { get; set; }
    }

    public class LinkActivation
    {
        public IPlayer Player { get; set; }
        public DiscordUser Discord { get; set; }
        public string Code { get; set; }
        public Snowflake Channel { get; set; }
    }

    public class LinkMessageData
    {
        public Snowflake ChannelId { get; set; }
        public Snowflake MessageId { get; set; }
    }

    public class BanInfo
    {
        public int Times { get; set; }
        public DateTime BannedUntil { get; set; }
        public void AddDeclined(int timesBeforeBanned, float banDuration);
        public bool IsBanned();
        public TimeSpan GetRemainingBan();
    }

    public static class LangKeys
    {
        public const string NoPermission;
        public const string ChatFormat;
        public const string DiscordFormat;
        public const string DiscordCoreOffline;
        public const string GenericError;
        public const string ConsolePlayerNotSupported;
        public static class Commands
        {
            private const string Base;
            public const string Unknown;
            public const string ChatHelpText;
            public const string DiscordHelpText;
            public static class Leave
            {
                private const string Base;
                public static class Errors
                {
                    private const string Base;
                    public const string NotLinked;
                }

            }

            public static class Join
            {
                private const string Base;
                public const string Modes;
                public static class Messages
                {
                    private const string Base;
                    public static class Discord
                    {
                        private const string Base;
                        public const string Username;
                        public const string CompletedInGame;
                        public const string CompleteInGameResponse;
                        public const string Declined;
                        public const string Accept;
                        public const string Decline;
                        public const string LinkAccounts;
                    }

                    public static class Chat
                    {
                        private const string Base;
                        public const string UsernameDmSent;
                        public const string Declined;
                    }

                }

                public static class Complete
                {
                    private const string Base;
                    public const string Info;
                    public const string InfoServer;
                    public const string InfoGuildAny;
                    public const string InfoGuildChannel;
                    public const string InfoAlsoDm;
                    public const string InfoDmOnly;
                }

                public static class Errors
                {
                    private const string Base;
                    public const string AlreadySignedUp;
                    public const string UnableToFindUser;
                    public const string FoundMultipleUsers;
                    public const string UsernameSearchError;
                    public const string InvalidSyntax;
                    public const string NoPendingActivations;
                    public const string MustBeUsedServer;
                    public const string MustBeUsedDiscord;
                    public const string Banned;
                }

            }

        }

        public static class Linking
        {
            private const string Base;
            public static class Chat
            {
                private const string Base;
                public const string Linked;
                public const string Unlinked;
            }

            public static class Discord
            {
                private const string Base;
                public const string Linked;
                public const string Unlinked;
            }

        }

        public static class Guild
        {
            private const string Base;
            public const string WelcomeMessage;
            public const string WelcomeLinkMessage;
            public const string LinkMessage;
        }

        public static class Notifications
        {
            private const string Base;
            public const string Link;
            public const string Rejoin;
            public const string Unlink;
        }

        public static class Emoji
        {
            private const string Base;
            public const string Accept;
            public const string Decline;
        }

    }

    public static class CommandKeys
    {
        public const string ChatCommand;
        public const string ChatJoinCommand;
        public const string ChatJoinCodeCommand;
        public const string ChatLeaveCommand;
        public const string DiscordCommand;
        public const string DiscordJoinCommand;
        public const string DiscordLeaveCommand;
    }

}

public class PluginConfig
{
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string ApiKey { get; set; }
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Server Name Override")]
    public string ServerNameOverride { get; set; }
    [DefaultValue("")]
    [JsonProperty(PropertyName = "Discord Server Join Code")]
    public string JoinCode { get; set; }
    [JsonProperty(PropertyName = "Welcome Message Settings")]
    public WelcomeMessageSettings WelcomeMessageSettings { get; set; }
    [JsonProperty(PropertyName = "Link Settings")]
    public DiscordLinkingSettings LinkSettings { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class DiscordLinkingSettings
{
    [JsonProperty(PropertyName = "Link Code Generator Characters")]
    public string LinkCodeCharacters { get; set; }
    [JsonProperty(PropertyName = "Link Code Length")]
    public int LinkCodeLength { get; set; }
    [JsonProperty(PropertyName = "Automatically Relink A Player If They Leave And Rejoin The Discord Server")]
    public bool AutoRelinkPlayer { get; set; }
    [JsonProperty(PropertyName = "Allow Commands To Be Used In Guild Channels")]
    public bool AllowCommandsInGuild { get; set; }
    [JsonProperty(PropertyName = "Allow Guild Commands Only In The Following Guild Channel Or Category (Channel ID Or Category ID)")]
    public List<Snowflake> AllowCommandInChannels { get; set; }
    [JsonProperty(PropertyName = "Link / Unlink Announcement Channel Id")]
    public Snowflake AnnouncementChannel { get; set; }
    [JsonProperty(PropertyName = "Guild Link Message Settings")]
    public LinkMessageSettings LinkMessageSettings { get; set; }
    [JsonProperty(PropertyName = "Link Ban Settings")]
    public LinkBanSettings LinkBanSettings { get; set; }
    public DiscordLinkingSettings(DiscordLinkingSettings settings);
}

public class WelcomeMessageSettings
{
    [JsonProperty(PropertyName = "Enable Discord Server Welcome DM Message")]
    public bool EnableJoinMessage { get; set; }
    [JsonProperty(PropertyName = "Add Link Accounts Button In Welcome Message")]
    public bool EnableLinkButton { get; set; }
    public WelcomeMessageSettings(WelcomeMessageSettings settings);
}

public class LinkMessageSettings
{
    [JsonProperty(PropertyName = "Enable Guild Link Message")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Message Channel ID")]
    public Snowflake ChannelId { get; set; }
    public LinkMessageSettings(LinkMessageSettings settings);
}

public class LinkBanSettings
{
    [JsonProperty(PropertyName = "Enable Link Ban")]
    public bool EnableLinkBanning { get; set; }
    [JsonProperty(PropertyName = "Ban Link After X Join Declines")]
    public int BanDeclineAmount { get; set; }
    [JsonProperty(PropertyName = "Ban Duration (Hours)")]
    public int BanDuration { get; set; }
    public LinkBanSettings(LinkBanSettings settings);
}

public class StoredData
{
    public Hash<string, DiscordInfo> PlayerDiscordInfo;
    public Hash<Snowflake, DiscordInfo> LeftPlayerInfo;
    public LinkMessageData MessageData;
}

public class DiscordInfo
{
    public Snowflake DiscordId { get; set; }
    public string PlayerId { get; set; }
}

public class LinkActivation
{
    public IPlayer Player { get; set; }
    public DiscordUser Discord { get; set; }
    public string Code { get; set; }
    public Snowflake Channel { get; set; }
}

public class LinkMessageData
{
    public Snowflake ChannelId { get; set; }
    public Snowflake MessageId { get; set; }
}

public class BanInfo
{
    public int Times { get; set; }
    public DateTime BannedUntil { get; set; }
    public void AddDeclined(int timesBeforeBanned, float banDuration);
    public bool IsBanned();
    public TimeSpan GetRemainingBan();
}

public static class LangKeys
{
    public const string NoPermission;
    public const string ChatFormat;
    public const string DiscordFormat;
    public const string DiscordCoreOffline;
    public const string GenericError;
    public const string ConsolePlayerNotSupported;
    public static class Commands
    {
        private const string Base;
        public const string Unknown;
        public const string ChatHelpText;
        public const string DiscordHelpText;
        public static class Leave
        {
            private const string Base;
            public static class Errors
            {
                private const string Base;
                public const string NotLinked;
            }

        }

        public static class Join
        {
            private const string Base;
            public const string Modes;
            public static class Messages
            {
                private const string Base;
                public static class Discord
                {
                    private const string Base;
                    public const string Username;
                    public const string CompletedInGame;
                    public const string CompleteInGameResponse;
                    public const string Declined;
                    public const string Accept;
                    public const string Decline;
                    public const string LinkAccounts;
                }

                public static class Chat
                {
                    private const string Base;
                    public const string UsernameDmSent;
                    public const string Declined;
                }

            }

            public static class Complete
            {
                private const string Base;
                public const string Info;
                public const string InfoServer;
                public const string InfoGuildAny;
                public const string InfoGuildChannel;
                public const string InfoAlsoDm;
                public const string InfoDmOnly;
            }

            public static class Errors
            {
                private const string Base;
                public const string AlreadySignedUp;
                public const string UnableToFindUser;
                public const string FoundMultipleUsers;
                public const string UsernameSearchError;
                public const string InvalidSyntax;
                public const string NoPendingActivations;
                public const string MustBeUsedServer;
                public const string MustBeUsedDiscord;
                public const string Banned;
            }

        }

    }

    public static class Linking
    {
        private const string Base;
        public static class Chat
        {
            private const string Base;
            public const string Linked;
            public const string Unlinked;
        }

        public static class Discord
        {
            private const string Base;
            public const string Linked;
            public const string Unlinked;
        }

    }

    public static class Guild
    {
        private const string Base;
        public const string WelcomeMessage;
        public const string WelcomeLinkMessage;
        public const string LinkMessage;
    }

    public static class Notifications
    {
        private const string Base;
        public const string Link;
        public const string Rejoin;
        public const string Unlink;
    }

    public static class Emoji
    {
        private const string Base;
        public const string Accept;
        public const string Decline;
    }

}

public static class Commands
{
    private const string Base;
    public const string Unknown;
    public const string ChatHelpText;
    public const string DiscordHelpText;
    public static class Leave
    {
        private const string Base;
        public static class Errors
        {
            private const string Base;
            public const string NotLinked;
        }

    }

    public static class Join
    {
        private const string Base;
        public const string Modes;
        public static class Messages
        {
            private const string Base;
            public static class Discord
            {
                private const string Base;
                public const string Username;
                public const string CompletedInGame;
                public const string CompleteInGameResponse;
                public const string Declined;
                public const string Accept;
                public const string Decline;
                public const string LinkAccounts;
            }

            public static class Chat
            {
                private const string Base;
                public const string UsernameDmSent;
                public const string Declined;
            }

        }

        public static class Complete
        {
            private const string Base;
            public const string Info;
            public const string InfoServer;
            public const string InfoGuildAny;
            public const string InfoGuildChannel;
            public const string InfoAlsoDm;
            public const string InfoDmOnly;
        }

        public static class Errors
        {
            private const string Base;
            public const string AlreadySignedUp;
            public const string UnableToFindUser;
            public const string FoundMultipleUsers;
            public const string UsernameSearchError;
            public const string InvalidSyntax;
            public const string NoPendingActivations;
            public const string MustBeUsedServer;
            public const string MustBeUsedDiscord;
            public const string Banned;
        }

    }

}

public static class Leave
{
    private const string Base;
    public static class Errors
    {
        private const string Base;
        public const string NotLinked;
    }

}

public static class Errors
{
    private const string Base;
    public const string NotLinked;
}

public static class Join
{
    private const string Base;
    public const string Modes;
    public static class Messages
    {
        private const string Base;
        public static class Discord
        {
            private const string Base;
            public const string Username;
            public const string CompletedInGame;
            public const string CompleteInGameResponse;
            public const string Declined;
            public const string Accept;
            public const string Decline;
            public const string LinkAccounts;
        }

        public static class Chat
        {
            private const string Base;
            public const string UsernameDmSent;
            public const string Declined;
        }

    }

    public static class Complete
    {
        private const string Base;
        public const string Info;
        public const string InfoServer;
        public const string InfoGuildAny;
        public const string InfoGuildChannel;
        public const string InfoAlsoDm;
        public const string InfoDmOnly;
    }

    public static class Errors
    {
        private const string Base;
        public const string AlreadySignedUp;
        public const string UnableToFindUser;
        public const string FoundMultipleUsers;
        public const string UsernameSearchError;
        public const string InvalidSyntax;
        public const string NoPendingActivations;
        public const string MustBeUsedServer;
        public const string MustBeUsedDiscord;
        public const string Banned;
    }

}

public static class Messages
{
    private const string Base;
    public static class Discord
    {
        private const string Base;
        public const string Username;
        public const string CompletedInGame;
        public const string CompleteInGameResponse;
        public const string Declined;
        public const string Accept;
        public const string Decline;
        public const string LinkAccounts;
    }

    public static class Chat
    {
        private const string Base;
        public const string UsernameDmSent;
        public const string Declined;
    }

}

public static class Discord
{
    private const string Base;
    public const string Username;
    public const string CompletedInGame;
    public const string CompleteInGameResponse;
    public const string Declined;
    public const string Accept;
    public const string Decline;
    public const string LinkAccounts;
}

public static class Chat
{
    private const string Base;
    public const string UsernameDmSent;
    public const string Declined;
}

public static class Complete
{
    private const string Base;
    public const string Info;
    public const string InfoServer;
    public const string InfoGuildAny;
    public const string InfoGuildChannel;
    public const string InfoAlsoDm;
    public const string InfoDmOnly;
}

public static class Errors
{
    private const string Base;
    public const string AlreadySignedUp;
    public const string UnableToFindUser;
    public const string FoundMultipleUsers;
    public const string UsernameSearchError;
    public const string InvalidSyntax;
    public const string NoPendingActivations;
    public const string MustBeUsedServer;
    public const string MustBeUsedDiscord;
    public const string Banned;
}

public static class Linking
{
    private const string Base;
    public static class Chat
    {
        private const string Base;
        public const string Linked;
        public const string Unlinked;
    }

    public static class Discord
    {
        private const string Base;
        public const string Linked;
        public const string Unlinked;
    }

}

public static class Chat
{
    private const string Base;
    public const string Linked;
    public const string Unlinked;
}

public static class Discord
{
    private const string Base;
    public const string Linked;
    public const string Unlinked;
}

public static class Guild
{
    private const string Base;
    public const string WelcomeMessage;
    public const string WelcomeLinkMessage;
    public const string LinkMessage;
}

public static class Notifications
{
    private const string Base;
    public const string Link;
    public const string Rejoin;
    public const string Unlink;
}

public static class Emoji
{
    private const string Base;
    public const string Accept;
    public const string Decline;
}

public static class CommandKeys
{
    public const string ChatCommand;
    public const string ChatJoinCommand;
    public const string ChatJoinCodeCommand;
    public const string ChatLeaveCommand;
    public const string DiscordCommand;
    public const string DiscordJoinCommand;
    public const string DiscordLeaveCommand;
}


```

---

## DiscordMessages

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info( "DiscordMessages", "Slut", "2.1.8" )]
[SuppressMessage( "ReSharper", "UnusedMember.Local" )]
internal class DiscordMessages : CovalencePlugin
{
    private void OnUserChat(IPlayer player, string message);
    private void MessageCommand(IPlayer player, string command, string[] args);
    private class Data
    {
        public readonly Dictionary<string, PlayerData> Players;
    }

    private class PlayerData
    {
        public int Reports { get; set; }
        public DateTime? ReportCooldown { get; set; }
        public DateTime? MessageCooldown { get; set; }
        public bool ReportDisabled { get; set; }
    }

    public class FancyMessage
    {
        [JsonProperty( "content" )]
        private string Content { get; set; }
        [JsonProperty( "tts" )]
        private bool TextToSpeech { get; set; }
        [JsonProperty( "embeds" )]
        private EmbedBuilder[] Embeds { get; set; }
        public FancyMessage WithContent(string value);
        public FancyMessage AsTTS(bool value);
        public FancyMessage SetEmbed(EmbedBuilder value);
        public string GetContent();
        public bool IsTTS();
        public EmbedBuilder GetEmbed();
        public string ToJson();
    }

    public class EmbedBuilder
    {
        public EmbedBuilder();
        [JsonProperty( "title" )]
        private string Title { get; set; }
        [JsonProperty( "color" )]
        private int Color { get; set; }
        [JsonProperty( "fields" )]
        private List<Field> Fields { get; set; }
        [JsonProperty( "description" )]
        private string Description { get; set; }
        public EmbedBuilder WithTitle(string title);
        public EmbedBuilder WithDescription(string description);
        public EmbedBuilder SetColor(int color);
        public EmbedBuilder SetColor(string color);
        public EmbedBuilder AddInlineField(string name, object value);
        public EmbedBuilder AddField(string name, object value);
        public EmbedBuilder AddField(Field field);
        public EmbedBuilder AddFields(Field[] fields);
        public int GetColor();
        public string GetTitle();
        public Field[] GetFields();
        private int ParseColor(string input);
        public class Field
        {
            [JsonProperty( "inline" )]
            public bool Inline;
            [JsonProperty( "name" )]
            public string Name;
            [JsonProperty( "value" )]
            public object Value;
            public Field(string name, object value, bool inline);
            public Field();
        }

    }

    private abstract class Response
    {
        public int Code { get; set; }
        public string Message { get; set; }
    }

    private class BaseResponse : Response
    {
        public bool IsRatelimit { get; set; }
        public bool IsOk { get; set; }
        public bool IsBad { get; set; }
        public RateLimitResponse GetRateLimit();
    }

    private class Request
    {
        private static bool _rateLimited;
        private static bool _busy;
        private static Queue<Request> _requestQueue;
        private readonly string _payload;
        private readonly Plugin _plugin;
        private readonly Action<BaseResponse> _response;
        private readonly string _url;
        public static void Init();
        private Request(string url, FancyMessage message, Action<BaseResponse> response, Plugin plugin);
        private Request(string url, FancyMessage message, Plugin plugin);
        private static void SendNextRequest();
        private static void EnqueueRequest(Request request);
        private void Send();
        private static void OnRateLimit(int retryAfter);
        private static void OnRateLimitEnd();
        public static void Send(string url, FancyMessage message, Plugin plugin);
        public static void Send(string url, FancyMessage message, Action<BaseResponse> callback, Plugin plugin);
        public static void Dispose();
    }

    private class RateLimitResponse : BaseResponse
    {
        [JsonProperty( "retry_after" )]
        public int RetryAfter { get; set; }
    }

    private Configuration _config;
    private class Configuration
    {
        public General GeneralSettings { get; set; }
        public Ban BanSettings { get; set; }
        public Report ReportSettings { get; set; }
        public Message MessageSettings { get; set; }
        public Chat ChatSettings { get; set; }
        public Mute MuteSettings { get; set; }
        [JsonIgnore]
        public Dictionary<FeatureType, WebhookObject> FeatureTypes { get; set; }
        public static Configuration Defaults();
        public class General
        {
            public bool Announce { get; set; }
        }

        public class Ban : EmbedObject
        {
        }

        public class Message : EmbedObject
        {
            public bool LogToConsole { get; set; }
            public bool SuggestAlias { get; set; }
            public string Alert { get; set; }
            public int Cooldown { get; set; }
        }

        public class Report : EmbedObject
        {
            public bool LogToConsole { get; set; }
            public string Alert { get; set; }
            public int Cooldown { get; set; }
        }

        public class Chat : WebhookObject
        {
            public bool TextToSpeech { get; set; }
        }

        public class Mute : EmbedObject
        {
        }

        public class EmbedObject : WebhookObject
        {
            public string Color { get; set; }
        }

        public class WebhookObject
        {
            public bool Enabled { get; set; }
            public string WebhookUrl { get; set; }
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private T GetFeatureConfig(FeatureType type);
    private Data _data;
    [PluginReference]
    private readonly Plugin BetterChatMute;
    private readonly Plugin AdminChat;
    private static DiscordMessages _instance;
    private readonly JsonSerializerSettings _jsonSettings;
    private readonly Dictionary<string, string> _headers;
    private void Loaded();
    private void CheckHooks();
    private void Unload();
    private void OnServerSave();
    private void RegisterCommands();
    protected override void LoadDefaultMessages();
    private const string BanPermission;
    private const string ReportPermission;
    private const string MessagePermission;
    private const string AdminPermission;
    private void RegisterPermissions();
    private void API_SendFancyMessage(string webhookUrl, string content, string embedJsonString, Plugin plugin);
    private void API_SendFancyMessage(string webhookUrl, string embedName, int embedColor, string json, string content, Plugin plugin);
    private void API_SendFancyMessage(string webhookUrl, string embedName, string json, string content, int embedColor, Plugin plugin);
    private void API_SendTextMessage(string webhookUrl, string content, bool tts, Plugin plugin);
    private void ReportAdminCommand(IPlayer player, string command, string[] args);
    private void ReportCommand(IPlayer player, string command, string[] args);
    private static string FormatTime(TimeSpan time);
    private void OnBetterChatTimeMuted(IPlayer target, IPlayer player, TimeSpan expireDate, string reason);
    private void OnBetterChatMuted(IPlayer target, IPlayer player, string reason);
    private void SendMute(IPlayer target, IPlayer player, TimeSpan expireDate, bool timed, string reason);
    private bool _banFromCommand;
    private void BanCommand(IPlayer player, string command, string[] args);
    private void ExecuteBan(IPlayer target, IPlayer player, string reason);
    private void OnUserBanned(string name, string bannedId, string address, string reason, long expiry, IPlayer source);
    private string GetLang(string key, string id, object[] args);
    private void SendMessage(IPlayer player, string message);
    private bool OnCooldown(IPlayer player, CooldownType type, int secondsRemaining);
    private void SaveData();
    private void LoadData();
    private IPlayer GetPlayer(string nameOrId, IPlayer player, bool sendError);
}

private class Data
{
    public readonly Dictionary<string, PlayerData> Players;
}

private class PlayerData
{
    public int Reports { get; set; }
    public DateTime? ReportCooldown { get; set; }
    public DateTime? MessageCooldown { get; set; }
    public bool ReportDisabled { get; set; }
}

public class FancyMessage
{
    [JsonProperty( "content" )]
    private string Content { get; set; }
    [JsonProperty( "tts" )]
    private bool TextToSpeech { get; set; }
    [JsonProperty( "embeds" )]
    private EmbedBuilder[] Embeds { get; set; }
    public FancyMessage WithContent(string value);
    public FancyMessage AsTTS(bool value);
    public FancyMessage SetEmbed(EmbedBuilder value);
    public string GetContent();
    public bool IsTTS();
    public EmbedBuilder GetEmbed();
    public string ToJson();
}

public class EmbedBuilder
{
    public EmbedBuilder();
    [JsonProperty( "title" )]
    private string Title { get; set; }
    [JsonProperty( "color" )]
    private int Color { get; set; }
    [JsonProperty( "fields" )]
    private List<Field> Fields { get; set; }
    [JsonProperty( "description" )]
    private string Description { get; set; }
    public EmbedBuilder WithTitle(string title);
    public EmbedBuilder WithDescription(string description);
    public EmbedBuilder SetColor(int color);
    public EmbedBuilder SetColor(string color);
    public EmbedBuilder AddInlineField(string name, object value);
    public EmbedBuilder AddField(string name, object value);
    public EmbedBuilder AddField(Field field);
    public EmbedBuilder AddFields(Field[] fields);
    public int GetColor();
    public string GetTitle();
    public Field[] GetFields();
    private int ParseColor(string input);
    public class Field
    {
        [JsonProperty( "inline" )]
        public bool Inline;
        [JsonProperty( "name" )]
        public string Name;
        [JsonProperty( "value" )]
        public object Value;
        public Field(string name, object value, bool inline);
        public Field();
    }

}

public class Field
{
    [JsonProperty( "inline" )]
    public bool Inline;
    [JsonProperty( "name" )]
    public string Name;
    [JsonProperty( "value" )]
    public object Value;
    public Field(string name, object value, bool inline);
    public Field();
}

private abstract class Response
{
    public int Code { get; set; }
    public string Message { get; set; }
}

private class BaseResponse : Response
{
    public bool IsRatelimit { get; set; }
    public bool IsOk { get; set; }
    public bool IsBad { get; set; }
    public RateLimitResponse GetRateLimit();
}

private class Request
{
    private static bool _rateLimited;
    private static bool _busy;
    private static Queue<Request> _requestQueue;
    private readonly string _payload;
    private readonly Plugin _plugin;
    private readonly Action<BaseResponse> _response;
    private readonly string _url;
    public static void Init();
    private Request(string url, FancyMessage message, Action<BaseResponse> response, Plugin plugin);
    private Request(string url, FancyMessage message, Plugin plugin);
    private static void SendNextRequest();
    private static void EnqueueRequest(Request request);
    private void Send();
    private static void OnRateLimit(int retryAfter);
    private static void OnRateLimitEnd();
    public static void Send(string url, FancyMessage message, Plugin plugin);
    public static void Send(string url, FancyMessage message, Action<BaseResponse> callback, Plugin plugin);
    public static void Dispose();
}

private class RateLimitResponse : BaseResponse
{
    [JsonProperty( "retry_after" )]
    public int RetryAfter { get; set; }
}

private class Configuration
{
    public General GeneralSettings { get; set; }
    public Ban BanSettings { get; set; }
    public Report ReportSettings { get; set; }
    public Message MessageSettings { get; set; }
    public Chat ChatSettings { get; set; }
    public Mute MuteSettings { get; set; }
    [JsonIgnore]
    public Dictionary<FeatureType, WebhookObject> FeatureTypes { get; set; }
    public static Configuration Defaults();
    public class General
    {
        public bool Announce { get; set; }
    }

    public class Ban : EmbedObject
    {
    }

    public class Message : EmbedObject
    {
        public bool LogToConsole { get; set; }
        public bool SuggestAlias { get; set; }
        public string Alert { get; set; }
        public int Cooldown { get; set; }
    }

    public class Report : EmbedObject
    {
        public bool LogToConsole { get; set; }
        public string Alert { get; set; }
        public int Cooldown { get; set; }
    }

    public class Chat : WebhookObject
    {
        public bool TextToSpeech { get; set; }
    }

    public class Mute : EmbedObject
    {
    }

    public class EmbedObject : WebhookObject
    {
        public string Color { get; set; }
    }

    public class WebhookObject
    {
        public bool Enabled { get; set; }
        public string WebhookUrl { get; set; }
    }

}

public class General
{
    public bool Announce { get; set; }
}

public class Ban : EmbedObject
{
}

public class Message : EmbedObject
{
    public bool LogToConsole { get; set; }
    public bool SuggestAlias { get; set; }
    public string Alert { get; set; }
    public int Cooldown { get; set; }
}

public class Report : EmbedObject
{
    public bool LogToConsole { get; set; }
    public string Alert { get; set; }
    public int Cooldown { get; set; }
}

public class Chat : WebhookObject
{
    public bool TextToSpeech { get; set; }
}

public class Mute : EmbedObject
{
}

public class EmbedObject : WebhookObject
{
    public string Color { get; set; }
}

public class WebhookObject
{
    public bool Enabled { get; set; }
    public string WebhookUrl { get; set; }
}


```

---

## DiscordPlayerJoin-1.0.1

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("DiscordPlayerJoin", "AKACHATGPTPASTEROK", "1.0.1")]
[Description("Sends a message to Discord when a player joins the server.")]
public class DiscordPlayerJoin : RustPlugin
{
    [PluginReference]
     Plugin Discord;
    private string webhookUrl;
    private const string TimeFormat;
     void Init();
     void OnPlayerConnected(BasePlayer player);
     void SendDiscordMessage(string message);
}


```

---

## DiscordRewards

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Entities.Activities;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Commands;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Entities.Users;
using Oxide.Ext.Discord.Logging;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("DiscordRewards", "k1lly0u", "0.2.2")]
[Description("Reward players with items, kits and commands for being a member of your Discord")]
 class DiscordRewards : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Kits;
    [DiscordClient]
    private DiscordClient Client;
    private DiscordGuild Guild;
    private DiscordRole NitroRole;
    private DiscordChannel ValidationChannel;
    public static DiscordRewards Instance { get; set; }
    private bool isInitialized;
    private bool needsWipe;
    private int statusIndex;
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnNewSave(string filename);
    private void Unload();
    private void OnDiscordClientCreated();
    private void OnDiscordGuildCreated(DiscordGuild guild);
    private void OnDiscordGuildMemberRemoved(GuildMember member, DiscordGuild guild);
    private void OnDiscordDirectMessageCreated(DiscordMessage message, DiscordChannel channel);
    private void OnDiscordGuildMessageCreated(DiscordMessage message, DiscordChannel channel, DiscordGuild guild);
    private bool AttemptTokenValidation(DiscordUser discordUser, int code);
    private BasePlayer FindPlayer(ulong userId);
    private void UpdateStatus();
    private GuildMember FindMember(Snowflake id);
    public DiscordRole GetRoleByID(string id);
    private int GenerateToken();
    private static double CurrentTime();
    private T ParseType(string type);
    private string FormatTime(double time);
    private void AddToGroup(string userId, string groupId);
    private void RemoveFromGroup(string userId, string groupId);
    private bool GroupExists(string groupId);
    private bool HasGroup(string userId, string groupId);
    private void GrantPermission(string userId, string perm);
    private void RevokePermission(string userId, string perm);
    private bool HasPermission(string userId, string perm);
    private bool PermissionExists(string userId);
    private void ValidateUser(BasePlayer player);
    private void IssueAlternativeRewards(BasePlayer player);
    private void ApplyUserRoles(UserData.User user, IEnumerable<string> roles, bool storeChanges);
    private void RevokeUserRoles(UserData.User user, IEnumerable<string> roles, bool storeChanges);
    private void IssueNitroRewards(BasePlayer player, UserData.User user);
    private void RevokeRewards(ulong playerId, UserData.User user);
    private void RevokeNitroRewards(ulong playerId, UserData.User user);
    private const string UI_MENU;
    private class UI
    {
        public static CuiElementContainer Container(string panel, UI4 dimensions, string color);
        public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
        public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
        public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
        public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
        public static string Color(string hexColor, float alpha);
    }

    private void OpenStore(BasePlayer player);
    private void LoadStoreUI(BasePlayer player, RewardType rewardType, int page);
    private void AddCategoryButtons(CuiElementContainer container, RewardType selected, ulong playerId);
    private void PopulateItems(List<T> list, CuiElementContainer container, int index, ulong playerId, bool isNitroBooster);
    private void AddPagination(CuiElementContainer container, RewardType rewardType, int page);
    private readonly HoriztonalAlignment RewardAlignment;
    private readonly HoriztonalAlignment TypeAlignment;
    private class HoriztonalAlignment
    {
        private int Columns { get; set; }
        private float XBorder { get; set; }
        private float XSpacing { get; set; }
        private float YOffset { get; set; }
        private float Height { get; set; }
        private float YSpacing { get; set; }
        private float ReservedSpace { get; set; }
        private float Width { get; set; }
        internal HoriztonalAlignment(int columns, float xBorder, float xSpacing, float yOffset, float height, float ySpacing);
        internal UI4 Get(int index);
    }

    [ConsoleCommand("drui.changepage")]
    private void ccmdChangePage(ConsoleSystem.Arg arg);
    [ConsoleCommand("drui.exit")]
    private void ccmdExit(ConsoleSystem.Arg arg);
    [ConsoleCommand("drui.claim")]
    private void ccmdClaim(ConsoleSystem.Arg arg);
    [ChatCommand("discord")]
    private void cmdDiscord(BasePlayer player, string command, string[] args);
    [ConsoleCommand("discord.admin")]
    private void ccmdDiscordAdmin(ConsoleSystem.Arg arg);
    [ConsoleCommand("discord.rewards")]
    private void ccmdDiscordRewards(ConsoleSystem.Arg arg);
    private void LoadImages();
    private string GetImage(string fileName, ulong skin);
    private string SteamToDiscordID(ulong playerId);
    private string DiscordToSteamID(string discordId);
    private ConfigData Configuration;
    private class ConfigData
    {
        public DiscordSettings Settings { get; set; }
        [JsonProperty(PropertyName = "Alternative Rewards")]
        public AlternativeRewards Rewards { get; set; }
        [JsonProperty(PropertyName = "Validation Tokens")]
        public Validation Token { get; set; }
        [JsonProperty(PropertyName = "Global Cooldown")]
        public GlobalCooldown Cooldown { get; set; }
        [JsonProperty(PropertyName = "UI Options")]
        public UIOptions UISettings { get; set; }
        public class DiscordSettings
        {
            [JsonProperty(PropertyName = "Bot Token")]
            public string APIKey { get; set; }
            [JsonProperty(PropertyName = "Bot Client ID")]
            public string BotID { get; set; }
            [JsonProperty(PropertyName = "Bot Status Messages")]
            public string[] StatusMessages { get; set; }
            [JsonProperty(PropertyName = "Bot Status Cycle Time (seconds)")]
            public int StatusCycle { get; set; }
            [JsonConverter(typeof(StringEnumConverter))]
            [JsonProperty(PropertyName = "Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
            public DiscordLogLevel LogLevel { get; set; }
        }

        public class UIOptions
        {
            [JsonProperty(PropertyName = "Enable Reward Menu")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Selected Button Color")]
            public UIColor Selected { get; set; }
            [JsonProperty(PropertyName = "Deselected Button Color")]
            public UIColor Deselected { get; set; }
            [JsonProperty(PropertyName = "Close Button Color")]
            public UIColor Close { get; set; }
            [JsonProperty(PropertyName = "Claim Button Color")]
            public UIColor Claim { get; set; }
            [JsonProperty(PropertyName = "Nitro Color")]
            public UIColor Nitro { get; set; }
            [JsonProperty(PropertyName = "Background Color")]
            public UIColor Background { get; set; }
            [JsonProperty(PropertyName = "Panel Color")]
            public UIColor Panel { get; set; }
            public class UIColor
            {
                public string Hex { get; set; }
                public float Alpha { get; set; }
                [JsonIgnore]
                private string _color;
                [JsonIgnore]
                public string Color { get; set; }
            }

        }

        public class GlobalCooldown
        {
            [JsonProperty(PropertyName = "Use Global Cooldown")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Global Cooldown Time (seconds)")]
            public int Time { get; set; }
        }

        public class Validation
        {
            [JsonProperty(PropertyName = "Token Lifetime (seconds)")]
            public int TokenLife { get; set; }
            [JsonProperty(PropertyName = "Require Re-validation")]
            public bool RequireRevalidation { get; set; }
            [JsonProperty(PropertyName = "Automatically try and re-validate users when their token has expired")]
            public bool AutoRevalidation { get; set; }
            [JsonProperty(PropertyName = "Revalidation Interval (seconds)")]
            public int RevalidationInterval { get; set; }
            [JsonProperty(PropertyName = "Revoke rewards and wipe token data on map wipe")]
            public bool WipeReset { get; set; }
            [JsonProperty(PropertyName = "Reset reward cooldowns on map wipe")]
            public bool WipeResetRewards { get; set; }
            [JsonProperty(PropertyName = "Validation channel")]
            public string ValidationChannel { get; set; }
        }

        public class AlternativeRewards
        {
            [JsonProperty(PropertyName = "Add user to user groups")]
            public string[] Groups { get; set; }
            [JsonProperty(PropertyName = "Commands to run on successful validation")]
            public string[] Commands { get; set; }
            [JsonProperty(PropertyName = "Permissions to grant on successful validation")]
            public string[] Permissions { get; set; }
            [JsonProperty(PropertyName = "Discord roles to grant on successful validation")]
            public string[] Roles { get; set; }
            [JsonProperty(PropertyName = "Discord roles to revoke on successful validation")]
            public string[] RevokeRoles { get; set; }
            [JsonProperty(PropertyName = "[Nitro Boosters] Add user to user groups")]
            public string[] NitroGroups { get; set; }
            [JsonProperty(PropertyName = "[Nitro Boosters] Commands to run on successful validation")]
            public string[] NitroCommands { get; set; }
            [JsonProperty(PropertyName = "[Nitro Boosters] Permissions to grant on successful validation")]
            public string[] NitroPermissions { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void VerifyConfigContents();
    private UserData userData;
    private RewardData rewardData;
    private DynamicConfigFile userdata;
    private DynamicConfigFile rewarddata;
    private void SaveData();
    private void SaveRewards();
    private void WipeData();
    private void WipeRewardCooldowns();
    private void LoadData();
    private class UserData
    {
        public Dictionary<ulong, User> users;
        public Hash<int, DiscordToken> tokenToUser;
        public User AddNewUser(ulong playerId, string discordId);
        public User GetUser(ulong userId);
        public bool FindByID(string discordId, ulong userId);
        public void AddToken(int code, ulong playerId, int duration);
        public bool HasPendingToken(ulong playerId, int code);
        public bool IsValidToken(int code, DiscordToken token);
        public void InvalidateToken(int token);
        public class DiscordToken
        {
            public ulong playerId;
            public double expireTime;
            public DiscordToken(ulong playerId, int duration);
        }

        public class User
        {
            public string discordId;
            public double expireTime;
            public double globalTime;
            public bool isNitroBooster;
            public HashSet<string> groups;
            public HashSet<string> permissions;
            public HashSet<string> roles;
            public HashSet<string> nitroGroups;
            public HashSet<string> nitroPermissions;
            [JsonIgnore]
            private Snowflake _id;
            [JsonIgnore]
            public Snowflake Id { get; set; }
            public User(string discordId);
            public void SetExpiryDate(int duration);
            public Dictionary<RewardType, Dictionary<int, double>> items;
            public void AddCooldown(RewardType type, int id, int time);
            public void AddCooldown(int time);
            public bool HasCooldown(RewardType type, int id, double remaining);
            public bool HasCooldown(double remaining);
            public void WipeCooldowns();
        }

    }

    private class RewardData
    {
        public List<RewardItem> items;
        public List<RewardKit> kits;
        public List<RewardCommand> commands;
        public class RewardItem : BaseReward
        {
            public string Shortname { get; set; }
            public int Amount { get; set; }
            public ulong SkinID { get; set; }
            public bool IsBP { get; set; }
            internal override string RewardType { get; set; }
            public override void GiveReward(BasePlayer player);
            internal override void CreateIconImage(CuiElementContainer container, UI4 position);
            internal override void CreateNameLabel(CuiElementContainer container, UI4 position);
            internal override void CreateDescriptionLabel(CuiElementContainer container, UI4 position);
        }

        public class RewardCommand : BaseReward
        {
            public List<string> Commands { get; set; }
            internal override string RewardType { get; set; }
            public override void GiveReward(BasePlayer player);
        }

        public class RewardKit : BaseReward
        {
            public string Kit { get; set; }
            internal override string RewardType { get; set; }
            public override void GiveReward(BasePlayer player);
        }

        public class BaseReward
        {
            public string Name { get; set; }
            public string Description { get; set; }
            public int Cooldown { get; set; }
            public string Icon { get; set; }
            public bool Nitro { get; set; }
            [JsonIgnore]
            internal virtual string RewardType { get; set; }
            public virtual void GiveReward(BasePlayer player);
            internal virtual void AddUIEntry(CuiElementContainer container, UI4 position, int listIndex, ulong playerId, bool isNitroBooster);
            internal virtual void CreateIconImage(CuiElementContainer container, UI4 position);
            internal virtual void CreateNameLabel(CuiElementContainer container, UI4 position);
            internal virtual void CreateDescriptionLabel(CuiElementContainer container, UI4 position);
        }

    }

    private string Message(string key, ulong playerId);
    private Dictionary<string, string> Messages;
}

private class UI
{
    public static CuiElementContainer Container(string panel, UI4 dimensions, string color);
    public static void Panel(CuiElementContainer container, string panel, string color, UI4 dimensions);
    public static void Label(CuiElementContainer container, string panel, string text, int size, UI4 dimensions, TextAnchor align);
    public static void Button(CuiElementContainer container, string panel, string color, string text, int size, UI4 dimensions, string command, TextAnchor align);
    public static void Image(CuiElementContainer container, string panel, string png, UI4 dimensions);
    public static string Color(string hexColor, float alpha);
}

private class HoriztonalAlignment
{
    private int Columns { get; set; }
    private float XBorder { get; set; }
    private float XSpacing { get; set; }
    private float YOffset { get; set; }
    private float Height { get; set; }
    private float YSpacing { get; set; }
    private float ReservedSpace { get; set; }
    private float Width { get; set; }
    internal HoriztonalAlignment(int columns, float xBorder, float xSpacing, float yOffset, float height, float ySpacing);
    internal UI4 Get(int index);
}

private class ConfigData
{
    public DiscordSettings Settings { get; set; }
    [JsonProperty(PropertyName = "Alternative Rewards")]
    public AlternativeRewards Rewards { get; set; }
    [JsonProperty(PropertyName = "Validation Tokens")]
    public Validation Token { get; set; }
    [JsonProperty(PropertyName = "Global Cooldown")]
    public GlobalCooldown Cooldown { get; set; }
    [JsonProperty(PropertyName = "UI Options")]
    public UIOptions UISettings { get; set; }
    public class DiscordSettings
    {
        [JsonProperty(PropertyName = "Bot Token")]
        public string APIKey { get; set; }
        [JsonProperty(PropertyName = "Bot Client ID")]
        public string BotID { get; set; }
        [JsonProperty(PropertyName = "Bot Status Messages")]
        public string[] StatusMessages { get; set; }
        [JsonProperty(PropertyName = "Bot Status Cycle Time (seconds)")]
        public int StatusCycle { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel LogLevel { get; set; }
    }

    public class UIOptions
    {
        [JsonProperty(PropertyName = "Enable Reward Menu")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Selected Button Color")]
        public UIColor Selected { get; set; }
        [JsonProperty(PropertyName = "Deselected Button Color")]
        public UIColor Deselected { get; set; }
        [JsonProperty(PropertyName = "Close Button Color")]
        public UIColor Close { get; set; }
        [JsonProperty(PropertyName = "Claim Button Color")]
        public UIColor Claim { get; set; }
        [JsonProperty(PropertyName = "Nitro Color")]
        public UIColor Nitro { get; set; }
        [JsonProperty(PropertyName = "Background Color")]
        public UIColor Background { get; set; }
        [JsonProperty(PropertyName = "Panel Color")]
        public UIColor Panel { get; set; }
        public class UIColor
        {
            public string Hex { get; set; }
            public float Alpha { get; set; }
            [JsonIgnore]
            private string _color;
            [JsonIgnore]
            public string Color { get; set; }
        }

    }

    public class GlobalCooldown
    {
        [JsonProperty(PropertyName = "Use Global Cooldown")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Global Cooldown Time (seconds)")]
        public int Time { get; set; }
    }

    public class Validation
    {
        [JsonProperty(PropertyName = "Token Lifetime (seconds)")]
        public int TokenLife { get; set; }
        [JsonProperty(PropertyName = "Require Re-validation")]
        public bool RequireRevalidation { get; set; }
        [JsonProperty(PropertyName = "Automatically try and re-validate users when their token has expired")]
        public bool AutoRevalidation { get; set; }
        [JsonProperty(PropertyName = "Revalidation Interval (seconds)")]
        public int RevalidationInterval { get; set; }
        [JsonProperty(PropertyName = "Revoke rewards and wipe token data on map wipe")]
        public bool WipeReset { get; set; }
        [JsonProperty(PropertyName = "Reset reward cooldowns on map wipe")]
        public bool WipeResetRewards { get; set; }
        [JsonProperty(PropertyName = "Validation channel")]
        public string ValidationChannel { get; set; }
    }

    public class AlternativeRewards
    {
        [JsonProperty(PropertyName = "Add user to user groups")]
        public string[] Groups { get; set; }
        [JsonProperty(PropertyName = "Commands to run on successful validation")]
        public string[] Commands { get; set; }
        [JsonProperty(PropertyName = "Permissions to grant on successful validation")]
        public string[] Permissions { get; set; }
        [JsonProperty(PropertyName = "Discord roles to grant on successful validation")]
        public string[] Roles { get; set; }
        [JsonProperty(PropertyName = "Discord roles to revoke on successful validation")]
        public string[] RevokeRoles { get; set; }
        [JsonProperty(PropertyName = "[Nitro Boosters] Add user to user groups")]
        public string[] NitroGroups { get; set; }
        [JsonProperty(PropertyName = "[Nitro Boosters] Commands to run on successful validation")]
        public string[] NitroCommands { get; set; }
        [JsonProperty(PropertyName = "[Nitro Boosters] Permissions to grant on successful validation")]
        public string[] NitroPermissions { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class DiscordSettings
{
    [JsonProperty(PropertyName = "Bot Token")]
    public string APIKey { get; set; }
    [JsonProperty(PropertyName = "Bot Client ID")]
    public string BotID { get; set; }
    [JsonProperty(PropertyName = "Bot Status Messages")]
    public string[] StatusMessages { get; set; }
    [JsonProperty(PropertyName = "Bot Status Cycle Time (seconds)")]
    public int StatusCycle { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel LogLevel { get; set; }
}

public class UIOptions
{
    [JsonProperty(PropertyName = "Enable Reward Menu")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Selected Button Color")]
    public UIColor Selected { get; set; }
    [JsonProperty(PropertyName = "Deselected Button Color")]
    public UIColor Deselected { get; set; }
    [JsonProperty(PropertyName = "Close Button Color")]
    public UIColor Close { get; set; }
    [JsonProperty(PropertyName = "Claim Button Color")]
    public UIColor Claim { get; set; }
    [JsonProperty(PropertyName = "Nitro Color")]
    public UIColor Nitro { get; set; }
    [JsonProperty(PropertyName = "Background Color")]
    public UIColor Background { get; set; }
    [JsonProperty(PropertyName = "Panel Color")]
    public UIColor Panel { get; set; }
    public class UIColor
    {
        public string Hex { get; set; }
        public float Alpha { get; set; }
        [JsonIgnore]
        private string _color;
        [JsonIgnore]
        public string Color { get; set; }
    }

}

public class UIColor
{
    public string Hex { get; set; }
    public float Alpha { get; set; }
    [JsonIgnore]
    private string _color;
    [JsonIgnore]
    public string Color { get; set; }
}

public class GlobalCooldown
{
    [JsonProperty(PropertyName = "Use Global Cooldown")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Global Cooldown Time (seconds)")]
    public int Time { get; set; }
}

public class Validation
{
    [JsonProperty(PropertyName = "Token Lifetime (seconds)")]
    public int TokenLife { get; set; }
    [JsonProperty(PropertyName = "Require Re-validation")]
    public bool RequireRevalidation { get; set; }
    [JsonProperty(PropertyName = "Automatically try and re-validate users when their token has expired")]
    public bool AutoRevalidation { get; set; }
    [JsonProperty(PropertyName = "Revalidation Interval (seconds)")]
    public int RevalidationInterval { get; set; }
    [JsonProperty(PropertyName = "Revoke rewards and wipe token data on map wipe")]
    public bool WipeReset { get; set; }
    [JsonProperty(PropertyName = "Reset reward cooldowns on map wipe")]
    public bool WipeResetRewards { get; set; }
    [JsonProperty(PropertyName = "Validation channel")]
    public string ValidationChannel { get; set; }
}

public class AlternativeRewards
{
    [JsonProperty(PropertyName = "Add user to user groups")]
    public string[] Groups { get; set; }
    [JsonProperty(PropertyName = "Commands to run on successful validation")]
    public string[] Commands { get; set; }
    [JsonProperty(PropertyName = "Permissions to grant on successful validation")]
    public string[] Permissions { get; set; }
    [JsonProperty(PropertyName = "Discord roles to grant on successful validation")]
    public string[] Roles { get; set; }
    [JsonProperty(PropertyName = "Discord roles to revoke on successful validation")]
    public string[] RevokeRoles { get; set; }
    [JsonProperty(PropertyName = "[Nitro Boosters] Add user to user groups")]
    public string[] NitroGroups { get; set; }
    [JsonProperty(PropertyName = "[Nitro Boosters] Commands to run on successful validation")]
    public string[] NitroCommands { get; set; }
    [JsonProperty(PropertyName = "[Nitro Boosters] Permissions to grant on successful validation")]
    public string[] NitroPermissions { get; set; }
}

private class UserData
{
    public Dictionary<ulong, User> users;
    public Hash<int, DiscordToken> tokenToUser;
    public User AddNewUser(ulong playerId, string discordId);
    public User GetUser(ulong userId);
    public bool FindByID(string discordId, ulong userId);
    public void AddToken(int code, ulong playerId, int duration);
    public bool HasPendingToken(ulong playerId, int code);
    public bool IsValidToken(int code, DiscordToken token);
    public void InvalidateToken(int token);
    public class DiscordToken
    {
        public ulong playerId;
        public double expireTime;
        public DiscordToken(ulong playerId, int duration);
    }

    public class User
    {
        public string discordId;
        public double expireTime;
        public double globalTime;
        public bool isNitroBooster;
        public HashSet<string> groups;
        public HashSet<string> permissions;
        public HashSet<string> roles;
        public HashSet<string> nitroGroups;
        public HashSet<string> nitroPermissions;
        [JsonIgnore]
        private Snowflake _id;
        [JsonIgnore]
        public Snowflake Id { get; set; }
        public User(string discordId);
        public void SetExpiryDate(int duration);
        public Dictionary<RewardType, Dictionary<int, double>> items;
        public void AddCooldown(RewardType type, int id, int time);
        public void AddCooldown(int time);
        public bool HasCooldown(RewardType type, int id, double remaining);
        public bool HasCooldown(double remaining);
        public void WipeCooldowns();
    }

}

public class DiscordToken
{
    public ulong playerId;
    public double expireTime;
    public DiscordToken(ulong playerId, int duration);
}

public class User
{
    public string discordId;
    public double expireTime;
    public double globalTime;
    public bool isNitroBooster;
    public HashSet<string> groups;
    public HashSet<string> permissions;
    public HashSet<string> roles;
    public HashSet<string> nitroGroups;
    public HashSet<string> nitroPermissions;
    [JsonIgnore]
    private Snowflake _id;
    [JsonIgnore]
    public Snowflake Id { get; set; }
    public User(string discordId);
    public void SetExpiryDate(int duration);
    public Dictionary<RewardType, Dictionary<int, double>> items;
    public void AddCooldown(RewardType type, int id, int time);
    public void AddCooldown(int time);
    public bool HasCooldown(RewardType type, int id, double remaining);
    public bool HasCooldown(double remaining);
    public void WipeCooldowns();
}

private class RewardData
{
    public List<RewardItem> items;
    public List<RewardKit> kits;
    public List<RewardCommand> commands;
    public class RewardItem : BaseReward
    {
        public string Shortname { get; set; }
        public int Amount { get; set; }
        public ulong SkinID { get; set; }
        public bool IsBP { get; set; }
        internal override string RewardType { get; set; }
        public override void GiveReward(BasePlayer player);
        internal override void CreateIconImage(CuiElementContainer container, UI4 position);
        internal override void CreateNameLabel(CuiElementContainer container, UI4 position);
        internal override void CreateDescriptionLabel(CuiElementContainer container, UI4 position);
    }

    public class RewardCommand : BaseReward
    {
        public List<string> Commands { get; set; }
        internal override string RewardType { get; set; }
        public override void GiveReward(BasePlayer player);
    }

    public class RewardKit : BaseReward
    {
        public string Kit { get; set; }
        internal override string RewardType { get; set; }
        public override void GiveReward(BasePlayer player);
    }

    public class BaseReward
    {
        public string Name { get; set; }
        public string Description { get; set; }
        public int Cooldown { get; set; }
        public string Icon { get; set; }
        public bool Nitro { get; set; }
        [JsonIgnore]
        internal virtual string RewardType { get; set; }
        public virtual void GiveReward(BasePlayer player);
        internal virtual void AddUIEntry(CuiElementContainer container, UI4 position, int listIndex, ulong playerId, bool isNitroBooster);
        internal virtual void CreateIconImage(CuiElementContainer container, UI4 position);
        internal virtual void CreateNameLabel(CuiElementContainer container, UI4 position);
        internal virtual void CreateDescriptionLabel(CuiElementContainer container, UI4 position);
    }

}

public class RewardItem : BaseReward
{
    public string Shortname { get; set; }
    public int Amount { get; set; }
    public ulong SkinID { get; set; }
    public bool IsBP { get; set; }
    internal override string RewardType { get; set; }
    public override void GiveReward(BasePlayer player);
    internal override void CreateIconImage(CuiElementContainer container, UI4 position);
    internal override void CreateNameLabel(CuiElementContainer container, UI4 position);
    internal override void CreateDescriptionLabel(CuiElementContainer container, UI4 position);
}

public class RewardCommand : BaseReward
{
    public List<string> Commands { get; set; }
    internal override string RewardType { get; set; }
    public override void GiveReward(BasePlayer player);
}

public class RewardKit : BaseReward
{
    public string Kit { get; set; }
    internal override string RewardType { get; set; }
    public override void GiveReward(BasePlayer player);
}

public class BaseReward
{
    public string Name { get; set; }
    public string Description { get; set; }
    public int Cooldown { get; set; }
    public string Icon { get; set; }
    public bool Nitro { get; set; }
    [JsonIgnore]
    internal virtual string RewardType { get; set; }
    public virtual void GiveReward(BasePlayer player);
    internal virtual void AddUIEntry(CuiElementContainer container, UI4 position, int listIndex, ulong playerId, bool isNitroBooster);
    internal virtual void CreateIconImage(CuiElementContainer container, UI4 position);
    internal virtual void CreateNameLabel(CuiElementContainer container, UI4 position);
    internal virtual void CreateDescriptionLabel(CuiElementContainer container, UI4 position);
}


```

---

## DiscordStatus

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Data;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Entities.Activities;
using Oxide.Ext.Discord.Entities.Applications;
using Oxide.Ext.Discord.Entities.Channels;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Commands;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Messages.Embeds;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Libraries.Linking;
using Oxide.Ext.Discord.Logging;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("Discord Status", "Gonzi", "4.0.1")]
[Description("Shows server information as a discord bot status")]
public class DiscordStatus : CovalencePlugin
{
    private string seperatorText;
    private bool enableChatSeparators;
    [DiscordClient]
    private DiscordClient Client;
    private readonly DiscordSettings _settings;
    private DiscordGuild _guild;
    private readonly DiscordLink _link;
     Configuration config;
    private int statusIndex;
    private string[] StatusTypes;
     class Configuration
    {
        [JsonProperty(PropertyName = "Discord Bot Token")]
        public string BotToken;
        [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty(PropertyName = "Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Discord Group Id needed for Commands (null to disable)")]
        public Snowflake? GroupId;
        [JsonProperty(PropertyName = "Update Interval (Seconds)")]
        public int UpdateInterval;
        [JsonProperty(PropertyName = "Randomize Status")]
        public bool Randomize;
        [JsonProperty(PropertyName = "Status Type (Game/Stream/Listen/Watch)")]
        public string StatusType;
        [JsonProperty(PropertyName = "Status", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Status;
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, object[] args);
    public DiscordEmbed ServerStats(string content);
    [HookMethod(DiscordHooks.OnDiscordGuildMessageCreated)]
     void OnDiscordGuildMessageCreated(DiscordMessage message);
    private void DiscordCMD(string command, DiscordMessage message);
    private void OnServerInitialized();
    [HookMethod(DiscordHooks.OnDiscordGatewayReady)]
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
    private void UpdateStatus();
    private int GetStatusIndex();
    private ActivityType GetStatusType();
    private string Format(string message);
    private int GetAuthCount();
}

 class Configuration
{
    [JsonProperty(PropertyName = "Discord Bot Token")]
    public string BotToken;
    [JsonProperty(PropertyName = "Discord Server ID (Optional if bot only in 1 guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty(PropertyName = "Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Discord Group Id needed for Commands (null to disable)")]
    public Snowflake? GroupId;
    [JsonProperty(PropertyName = "Update Interval (Seconds)")]
    public int UpdateInterval;
    [JsonProperty(PropertyName = "Randomize Status")]
    public bool Randomize;
    [JsonProperty(PropertyName = "Status Type (Game/Stream/Listen/Watch)")]
    public string StatusType;
    [JsonProperty(PropertyName = "Status", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Status;
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose, Debug, Info, Warning, Error, Exception, Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}


```

---

## DiscordWin

```csharp
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("DiscordWin", "Visagalis", "1.1.0")]
[Description("Starts event to win some stuff via Discord.")]
 class DiscordWin : RustPlugin
{
    [PluginReference]
     Plugin DiscordMessages;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Economics;
     string secretCode;
     int reward;
     List<ulong> claimedReward;
    public static class DWConfig
    {
        public static bool UseEconomics;
        public static bool UseServerRewards;
        public static string WebHookUrl;
        public static string DiscordChannel;
        public static int RewardBase;
        public static int RewardMinPlusVariance;
        public static int RewardMaxPlusVariance;
        public static float RewardMultiplyBaseMinVariance;
        public static float RewardMultiplyBaseMaxVariance;
        public static int EventLengthMinVariance;
        public static int EventLengthMaxVariance;
        public static int EventOccurenceMinVariance;
        public static int EventOccurenceMaxVariance;
        public static int EventAnnounceMinVariance;
        public static int EventAnnounceMaxVariance;
        public static int MaximumWinners;
    }

     void LoadDefaultConfig();
     void InitConfig();
     void LoadConfig();
     void SetConfig(object[] args);
     void OnServerInitialized();
     void Unloaded();
    [ChatCommand("get")]
     void cmdTest(BasePlayer player, string command, string[] args);
     void Tell(BasePlayer player, string message);
     void Broadcast(string message);
     void StartEvent();
     void ContinueslyBroadcast();
     void EndEvent(string reason);
     void SendMessageToDiscord(string message, string icon);
}

public static class DWConfig
{
    public static bool UseEconomics;
    public static bool UseServerRewards;
    public static string WebHookUrl;
    public static string DiscordChannel;
    public static int RewardBase;
    public static int RewardMinPlusVariance;
    public static int RewardMaxPlusVariance;
    public static float RewardMultiplyBaseMinVariance;
    public static float RewardMultiplyBaseMaxVariance;
    public static int EventLengthMinVariance;
    public static int EventLengthMaxVariance;
    public static int EventOccurenceMinVariance;
    public static int EventOccurenceMaxVariance;
    public static int EventAnnounceMinVariance;
    public static int EventAnnounceMaxVariance;
    public static int MaximumWinners;
}


```

---

## Distance

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using System.Text.RegularExpressions;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("Distance", "ignignokt84", "0.1.1", ResourceId = 1780)]
 class Distance : RustPlugin
{
     void LoadDefaultMessages();
    private FieldInfo serverinput;
    private Dictionary<BasePlayer,Vector3> origin;
    private Dictionary<BasePlayer,KeyValuePair<Vector3[],float>[]> history;
    private Vector3 eyesAdjust;
    public const int histSize;
    public KeyValuePair<string,Color>[] histColors;
    public string usageString;
     void Loaded();
     string GetMessage(string key, string userId);
     void cmdChatDelegator(BasePlayer player, string command, string[] args);
     void notifyInvalidParameter(BasePlayer player, string message);
     void notifyDistance(BasePlayer player, string message);
     void notifyHistory(BasePlayer player, string message);
     void showUsage(BasePlayer player);
     void setOrigin(BasePlayer player);
     void measure(BasePlayer player);
     void rangefinder(BasePlayer player);
     string getHitObjectName(Collider collider);
     string fixName(string str);
     string prettify(string str);
     string properCase(string str);
     string clearOrigin(BasePlayer player);
     string clearHistory(BasePlayer player);
    private void addToHistory(BasePlayer player, Vector3 p1, Vector3 p2, float distance);
     void showHistory(BasePlayer player);
     void sendMessage(BasePlayer player, string message);
    static string wrapSize(int size, string input);
    static string wrapColor(string color, string input);
    static void draw(BasePlayer player, Vector3 from, Vector3 to, Color color, float duration, string text);
    static void drawText(BasePlayer player, Vector3 position, Color color, float duration, string text);
     object OnReject(Network.Connection conn, string reason);
}


```

---

## DMBuildingBlocks

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("DMBuildingBlocks", "ColonBlow", "1.0.4")]
 class DMBuildingBlocks : RustPlugin
{
     void Loaded();
     bool HasPermission(BasePlayer player, string perm);
    public bool ProtectFoundation { get; set; }
    public bool ProtectFoundationSteps { get; set; }
    public bool ProtectFoundationTriangle { get; set; }
    public bool ProtectWindowWall { get; set; }
    public bool ProtectDoorway { get; set; }
    public bool ProtectFloor { get; set; }
    public bool ProtectFloorTriangle { get; set; }
    public bool ProtectPillar { get; set; }
    public bool ProtectStairsLShaped { get; set; }
    public bool ProtectStairsUShaped { get; set; }
    public bool ProtectRoof { get; set; }
    public bool ProtectLowWall { get; set; }
    public bool ProtectWall { get; set; }
    public bool ProtectWallFrame { get; set; }
    public bool ProtectFloorFrame { get; set; }
    protected override void LoadDefaultConfig();
     Dictionary<string, string> messages;
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    [ChatCommand("ProtectFoundation")]
     void chatCommand_ProtectFoundation(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFoundationSteps")]
     void chatCommand_ProtectFoundationSteps(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFoundationTriangle")]
     void chatCommand_ProtectFoundationTriangle(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectWindowWall")]
     void chatCommand_ProtectWindowWall(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectDoorway")]
     void chatCommand_ProtectDoorway(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFloor")]
     void chatCommand_ProtectFloor(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFloorTriangle")]
     void chatCommand_ProtectFloorTriangle(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectPillar")]
     void chatCommand_ProtectPillar(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectStairsLShaped")]
     void chatCommand_ProtectStairsLShaped(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectStairsUShaped")]
     void chatCommand_ProtectStairsUShaped(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectRoof")]
     void chatCommand_ProtectRoof(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectLowWall")]
     void chatCommand_ProtectLowWall(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectWall")]
     void chatCommand_ProtectWall(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectWallFrame")]
     void chatCommand_ProtectWallFrame(BasePlayer player, string command, string[] args);
    [ChatCommand("ProtectFloorFrame")]
     void chatCommand_ProtectFloorFrame(BasePlayer player, string command, string[] args);
}


```

---

## DMBuildings

```csharp
using System.Collections.Generic;
using System;
using Rust;

Oxide.Plugins
[Info("DMBuildings", "ColonBlow", "1.2.5", ResourceId = 1239)]
internal class DMBuildings : RustPlugin
{
    private const int DamageTypeMax;
    private readonly float[] _modifiers;
    private bool _didConfigChange;
    private void Loaded();
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private object GetConfigValue(string category, string setting, object defaultValue);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
}


```

---

## DMDeployables

```csharp

Oxide.Plugins
[Info("DMDeployables", "ColonBlow", "1.1.5", ResourceId = 1240)]
 class DMDeployables : RustPlugin
{
    public bool ProtectToolCupboard { get; set; }
    public bool ProtectSignage { get; set; }
    public bool ProtectBaseOven { get; set; }
    public bool ProtectMiningQuarry { get; set; }
    public bool ProtectWaterCatcher { get; set; }
    public bool ProtectResearchTable { get; set; }
    public bool ProtectSleepingBag { get; set; }
    public bool ProtectRepairBench { get; set; }
    public bool ProtectAutoTurret { get; set; }
    public bool ProtectBoxes { get; set; }
    public bool ProtectBarricade { get; set; }
    public bool ProtectSingleDoors { get; set; }
    public bool ProtectDoubleDoors { get; set; }
    public bool ProtectWindows { get; set; }
    public bool ProtectShutters { get; set; }
    public bool ProtectShelves { get; set; }
    public bool ProtectChainLink { get; set; }
    public bool ProtectPrisonCell { get; set; }
    public bool ProtectShopFront { get; set; }
    public bool ProtectExternalWalls { get; set; }
    public bool ProtectExternalGates { get; set; }
    public bool ProtectFloorGrill { get; set; }
    public bool ProtectFloorLadder { get; set; }
    protected override void LoadDefaultConfig();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
}


```

---

## DMPlayers

```csharp
using System.Collections.Generic;
using System;
using Rust;

Oxide.Plugins
[Info("DMPlayers", "ColonBlow", "1.2.3", ResourceId = 1238)]
internal class DMPlayers : RustPlugin
{
    private const int DamageTypeMax;
    private readonly float[] _modifiers;
    private bool _didConfigChange;
    private void Loaded();
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private object GetConfigValue(string category, string setting, object defaultValue);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
}


```

---

## DonateTransport-0.0.2

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("DonateTransport", "MaltrzD", "0.0.2")]
 class DonateTransport : RustPlugin
{
    private ConfigData _config;
    private void Loaded();
    private void OnEntityBuilt(Planner planner, GameObject go);
    [ConsoleCommand("dt.give")]
    private void GiveTransport_ConsoleCommand(ConsoleSystem.Arg arg);
    private void GiveTransport(BasePlayer player, Transport transport);
    private Transport GetTransportByName(string name);
    private Transport GetTransportBySkinId(ulong skinId);
     class ConfigData
    {
        [JsonProperty("Настройка транспорта")]
        public List<Transport> Transports;
    }

    protected override void LoadDefaultConfig();
     void SaveConfig(object config);
     void ReadConfig();
    public class Transport
    {
        [JsonProperty("Шортнейм предмета который ставить")]
        public string BuildItemShortName;
        [JsonProperty("корд плюс")]
        public float CordPlus;
        [JsonProperty("Название транспорта (для выдачи)")]
        public string TransportName;
        [JsonProperty("Отображаемое имя айтема")]
        public string DisplayName;
        [JsonProperty("Путь до префаба транспорта")]
        public string TransportPrefab;
        [JsonProperty("Ставится только на конструкцию?")]
        public bool CanPlaceOnlyCosntruction;
        [JsonProperty("Скин ид")]
        public ulong SkinId;
    }

}

 class ConfigData
{
    [JsonProperty("Настройка транспорта")]
    public List<Transport> Transports;
}

public class Transport
{
    [JsonProperty("Шортнейм предмета который ставить")]
    public string BuildItemShortName;
    [JsonProperty("корд плюс")]
    public float CordPlus;
    [JsonProperty("Название транспорта (для выдачи)")]
    public string TransportName;
    [JsonProperty("Отображаемое имя айтема")]
    public string DisplayName;
    [JsonProperty("Путь до префаба транспорта")]
    public string TransportPrefab;
    [JsonProperty("Ставится только на конструкцию?")]
    public bool CanPlaceOnlyCosntruction;
    [JsonProperty("Скин ид")]
    public ulong SkinId;
}


```

---

## DonationClaim

```csharp
using System.Text.RegularExpressions;
using Oxide.Ext.MySql;
using System.Text;
using Oxide.Core;
using Oxide.Game.Rust.Libraries;
using Oxide.Core.Plugins;
using Oxide.Core;
using System;
using System.IO;
using System.Linq;
using System.Net;
using System.Runtime.Serialization.Formatters.Binary;
using System.Collections.Generic;

Oxide.Plugins
[Info("Donation Claim", "LeoCurtss", "0.5")]
[Description("Player can claim rewards from PayPal donations.")]
 class DonationClaim : RustPlugin
{
    private readonly Ext.MySql.Libraries.MySql _mySql;
    private Ext.MySql.Connection _mySqlConnection;
     class DCConfig
    {
        public DCConfig();
        public string MySQLIP;
        public int MySQLPort;
        public string MySQLDatabase;
        public string MySQLusername;
        public string MySQLpassword;
         List<string> ExampleCommands;
        public Dictionary<string, List<string>> Packages;
    }

    protected override void LoadDefaultConfig();
     void Loaded();
    private DCConfig dc_config;
    private string GetMessage(string name, string sid);
    [ChatCommand("claimreward")]
     void ClaimRewardCommand(BasePlayer player, string command, string[] args);
    private string getPackageKey(string packageName, Dictionary<string, List<string>> packages);
     void RunConsoleCommands(List<string> CommandsList, string playerName);
}

 class DCConfig
{
    public DCConfig();
    public string MySQLIP;
    public int MySQLPort;
    public string MySQLDatabase;
    public string MySQLusername;
    public string MySQLpassword;
     List<string> ExampleCommands;
    public Dictionary<string, List<string>> Packages;
}


```

---

## DoorsControl

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using System;
using System.Reflection;
using System.Text;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("DoorsControl", "RusskiIvan", "1.0.4")]
[Description("DoorsControl")]
public class DoorsControl : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private StoredData _data;
    private ConfigData _config;
    private readonly FieldInfo _serverInput;
    private readonly Vector3 _eyesAdjust;
    private bool _dataLoaded;
    private readonly FieldInfo _hasCode;
    private class ConfigData
    {
        [JsonProperty("Команда для замков")]
        public string codelockCommand { get; set; }
        [JsonProperty("Команда для дверей ")]
        public string doorsCommand { get; set; }
        [JsonProperty("Привилегии")]
        public Permissions permissions { get; set; }
        [JsonProperty("Настройки")]
        public Settings settings { get; set; }
    }

    private class Permissions
    {
        [JsonProperty("Привилегия для дверей")]
        public string permissionDeployDoor { get; set; }
        [JsonProperty("Привилегия для ящиков")]
        public string permissionDeployBox { get; set; }
        [JsonProperty("Привилегия для шкафов с одеждой")]
        public string permissionDeployLocker { get; set; }
        [JsonProperty("Привилегия для шкафа")]
        public string permissionDeployCupboard { get; set; }
        [JsonProperty("Привилегия для автозакрытия замка")]
        public string permissionAutoLock { get; set; }
        [JsonProperty("Привилегия для установки замка без замка :)")]
        public string permissionNoLockNeed { get; set; }
        [JsonProperty("Привилегия для автозакрывания двери")]
        public string permissionAutoCloseDoor { get; set; }
        [JsonProperty("Привилегия для умного дома")]
        public string permissionSmartHome { get; set; }
    }

    private class Settings
    {
        [JsonProperty("Автозакрытие замка")]
        public bool AutoLock { get; set; }
        [JsonProperty("Авто установка на двери")]
        public bool DeployDoor { get; set; }
        [JsonProperty("Авто установка на ящики")]
        public bool DeployBox { get; set; }
        [JsonProperty("Авто установка на шкафы с одеждой")]
        public bool DeployLocker { get; set; }
        [JsonProperty("Авто установка на шкаф")]
        public bool DeployCupboard { get; set; }
        [JsonProperty("Задержка закрытия двери")]
        public float defaultDelay { get; set; }
        [JsonProperty("Автозакрытие дверей")]
        public bool autoDoor { get; set; }
    }

    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnDoorOpened(Door door, BasePlayer player);
    private void OnItemDeployed(Deployer deployer, BaseEntity entity);
    private void OnEntityBuilt(Planner planner, GameObject obj);
    private void OnNewSave();
    private void OnServerSave();
    private void Unload();
     bool IsClanMember(ulong playerid, ulong targetID);
    private void CloseDoor(BaseEntity door);
    private static BaseEntity DoRay(Vector3 pos, Vector3 aim);
    private void AutoDoorCommand(BasePlayer player, string command, string[] args);
    private void SetDoor(BasePlayer player);
    private void SwitchDoor(BasePlayer player, BaseNetworkable door);
    private void RegisterPermissions();
    private void AddNewPlayer(BasePlayer player);
    private void SetPlayerData(BasePlayer player, int Code, bool AutoLock, bool DeployDoor, bool DeployBox, bool DeployLocker, bool DeployCupboard, bool autoDoor, float defaultDelay);
    private PlayerInfo GetPlayerData(BasePlayer player);
    private void LockPlacing(BasePlayer player, BaseEntity entity);
    private void CodeLockCommand(BasePlayer player, string command, string[] args);
    private void SaveData();
    private void LoadData();
     class StoredData
    {
        public Dictionary<ulong, PlayerInfo> PlayerInfo;
        public List<uint> DoorsList;
    }

     class PlayerInfo
    {
        public bool AutoLock;
        public bool DeployDoor;
        public bool DeployBox;
        public bool DeployLocker;
        public bool DeployCupboard;
        public int Password;
        public bool AutoDoor;
        public float DefaultDelay;
    }

    protected override void LoadDefaultMessages();
    private string Msg(string key, BasePlayer player);
}

private class ConfigData
{
    [JsonProperty("Команда для замков")]
    public string codelockCommand { get; set; }
    [JsonProperty("Команда для дверей ")]
    public string doorsCommand { get; set; }
    [JsonProperty("Привилегии")]
    public Permissions permissions { get; set; }
    [JsonProperty("Настройки")]
    public Settings settings { get; set; }
}

private class Permissions
{
    [JsonProperty("Привилегия для дверей")]
    public string permissionDeployDoor { get; set; }
    [JsonProperty("Привилегия для ящиков")]
    public string permissionDeployBox { get; set; }
    [JsonProperty("Привилегия для шкафов с одеждой")]
    public string permissionDeployLocker { get; set; }
    [JsonProperty("Привилегия для шкафа")]
    public string permissionDeployCupboard { get; set; }
    [JsonProperty("Привилегия для автозакрытия замка")]
    public string permissionAutoLock { get; set; }
    [JsonProperty("Привилегия для установки замка без замка :)")]
    public string permissionNoLockNeed { get; set; }
    [JsonProperty("Привилегия для автозакрывания двери")]
    public string permissionAutoCloseDoor { get; set; }
    [JsonProperty("Привилегия для умного дома")]
    public string permissionSmartHome { get; set; }
}

private class Settings
{
    [JsonProperty("Автозакрытие замка")]
    public bool AutoLock { get; set; }
    [JsonProperty("Авто установка на двери")]
    public bool DeployDoor { get; set; }
    [JsonProperty("Авто установка на ящики")]
    public bool DeployBox { get; set; }
    [JsonProperty("Авто установка на шкафы с одеждой")]
    public bool DeployLocker { get; set; }
    [JsonProperty("Авто установка на шкаф")]
    public bool DeployCupboard { get; set; }
    [JsonProperty("Задержка закрытия двери")]
    public float defaultDelay { get; set; }
    [JsonProperty("Автозакрытие дверей")]
    public bool autoDoor { get; set; }
}

 class StoredData
{
    public Dictionary<ulong, PlayerInfo> PlayerInfo;
    public List<uint> DoorsList;
}

 class PlayerInfo
{
    public bool AutoLock;
    public bool DeployDoor;
    public bool DeployBox;
    public bool DeployLocker;
    public bool DeployCupboard;
    public int Password;
    public bool AutoDoor;
    public float DefaultDelay;
}


```

---

