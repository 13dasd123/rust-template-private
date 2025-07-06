# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 4 - Generated: 2025-07-06 20:25:18

## EventStatistics by k1lly0u - Stores statistic data from Event Manager

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Event Statistics", "k1lly0u", "1.0.1"), Description("Manages and provides API for statistics gathered from EventManager")]
public class EventStatistics : RustPlugin
{
    private DynamicConfigFile statisticsData;
    public static Statistics Data { get; set; }
    private void Loaded();
    private void OnServerSave();
    private void Unload();
    [HookMethod("AddStatistic")]
    public void AddStatistic(BasePlayer player, string statistic, int amount);
    [HookMethod("AddStatistic")]
    public void AddStatistic(ulong playerId, string statistic, int amount);
    [HookMethod("AddGlobalStatistic")]
    public void AddGlobalStatistic(string statistic, int amount);
    [HookMethod("OnGamePlayed")]
    public void OnGamePlayed(string eventName);
    [HookMethod("OnGamePlayed")]
    public void OnGamePlayed(BasePlayer player, string eventName);
    [HookMethod("GetStatistic")]
    public int GetStatistic(ulong playerId, string statistic);
    [HookMethod("GetRank")]
    public int GetRank(ulong playerId);
    [HookMethod("GetEventStatistic")]
    public int GetEventStatistic(ulong playerId, string eventName);
    [HookMethod("GetPlayerStatistics")]
    public void GetPlayerStatistics(List<KeyValuePair<string, int>> list, ulong playerId);
    [HookMethod("GetPlayerEvents")]
    public void GetPlayerEvents(List<KeyValuePair<string, int>> list, ulong playerId);
    [HookMethod("GetGlobalStatistic")]
    public int GetGlobalStatistic(string statistic);
    [HookMethod("GetGlobalEventStatistic")]
    public int GetGlobalEventStatistic(string eventName);
    [HookMethod("GetGlobalStatistics")]
    public void GetGlobalStatistics(List<KeyValuePair<string, int>> list);
    [HookMethod("GetGlobalEvents")]
    public void GetGlobalEvents(List<KeyValuePair<string, int>> list);
    [HookMethod("GetStatisticNames")]
    public void GetStatisticNames(List<string> list);
    private static string RemoveTags(string str);
    private static List<KeyValuePair<string, string>> _tags;
    public class Statistics
    {
        public Hash<ulong, Data> players;
        public Data global;
        [JsonIgnore]
        private Hash<Statistic, List<Data>> _cachedSortResults;
        public Data Find(ulong playerId);
        public void OnGamePlayed(string eventName);
        public void OnGamePlayed(BasePlayer player, string eventName);
        public void OnGamePlayed(ulong playerId, string eventName);
        public void AddStatistic(BasePlayer player, string statistic, int amount);
        public void AddStatistic(ulong playerId, string statistic, int amount);
        public void AddGlobalStatistic(string statistic, int amount);
        public int GetStatistic(ulong playerId, string statistic);
        public void GetPlayerStatistics(List<KeyValuePair<string, int>> list, ulong playerId);
        public void GetPlayerEvents(List<KeyValuePair<string, int>> list, ulong playerId);
        public int GetRank(ulong playerId);
        public int GetEventStatistic(ulong playerId, string eventName);
        public int GetGlobalStatistic(string statistic);
        public int GetGlobalEventStatistic(string eventName);
        public void GetGlobalStatistics(List<KeyValuePair<string, int>> list);
        public void GetGlobalEvents(List<KeyValuePair<string, int>> list);
        public void GetStatisticNames(List<string> list);
        public void ClearCachedSortResults();
        public List<Data> SortStatisticsBy(Statistic statistic);
        public void UpdateRankingScores();
        public class Data
        {
            public Hash<string, int> events;
            public Hash<string, int> statistics;
            public string DisplayName { get; set; }
            public ulong UserID { get; set; }
            [JsonIgnore]
            public float Score { get; set; }
            [JsonIgnore]
            public int Rank { get; set; }
            public Data(ulong userID, string displayName);
            public void UpdateName(string displayName);
            public void AddStatistic(string statisticName, int value);
            public void AddGamePlayed(string name);
            public int GetStatistic(string statistic);
            public void UpdateRankingScore();
        }

    }

    private void SaveData();
    private void LoadData();
}

public class Statistics
{
    public Hash<ulong, Data> players;
    public Data global;
    [JsonIgnore]
    private Hash<Statistic, List<Data>> _cachedSortResults;
    public Data Find(ulong playerId);
    public void OnGamePlayed(string eventName);
    public void OnGamePlayed(BasePlayer player, string eventName);
    public void OnGamePlayed(ulong playerId, string eventName);
    public void AddStatistic(BasePlayer player, string statistic, int amount);
    public void AddStatistic(ulong playerId, string statistic, int amount);
    public void AddGlobalStatistic(string statistic, int amount);
    public int GetStatistic(ulong playerId, string statistic);
    public void GetPlayerStatistics(List<KeyValuePair<string, int>> list, ulong playerId);
    public void GetPlayerEvents(List<KeyValuePair<string, int>> list, ulong playerId);
    public int GetRank(ulong playerId);
    public int GetEventStatistic(ulong playerId, string eventName);
    public int GetGlobalStatistic(string statistic);
    public int GetGlobalEventStatistic(string eventName);
    public void GetGlobalStatistics(List<KeyValuePair<string, int>> list);
    public void GetGlobalEvents(List<KeyValuePair<string, int>> list);
    public void GetStatisticNames(List<string> list);
    public void ClearCachedSortResults();
    public List<Data> SortStatisticsBy(Statistic statistic);
    public void UpdateRankingScores();
    public class Data
    {
        public Hash<string, int> events;
        public Hash<string, int> statistics;
        public string DisplayName { get; set; }
        public ulong UserID { get; set; }
        [JsonIgnore]
        public float Score { get; set; }
        [JsonIgnore]
        public int Rank { get; set; }
        public Data(ulong userID, string displayName);
        public void UpdateName(string displayName);
        public void AddStatistic(string statisticName, int value);
        public void AddGamePlayed(string name);
        public int GetStatistic(string statistic);
        public void UpdateRankingScore();
    }

}

public class Data
{
    public Hash<string, int> events;
    public Hash<string, int> statistics;
    public string DisplayName { get; set; }
    public ulong UserID { get; set; }
    [JsonIgnore]
    public float Score { get; set; }
    [JsonIgnore]
    public int Rank { get; set; }
    public Data(ulong userID, string displayName);
    public void UpdateName(string displayName);
    public void AddStatistic(string statisticName, int value);
    public void AddGamePlayed(string name);
    public int GetStatistic(string statistic);
    public void UpdateRankingScore();
}


```

---

## EventTipsRemover by Razor - Remove the new event tips

```csharp
using System.Collections.Generic;
using System.Collections;
using Oxide.Core.Configuration;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using Oxide.Core;
using System;
using System.Globalization;

Oxide.Plugins
[Info("Event Tips Remover", "Razor", "1.0.4")]
[Description("Remove new GameTip messages.")]
public class EventTipsRemover : RustPlugin
{
     List<PuzzleReset> puzzleReset;
     List<TriggeredEventPrefab> _triggeredEventPrefab;
    private void OnServerInitialized();
    private void Unload();
    private void OnEventTrigger(TriggeredEventPrefab eventTrig);
    private object OnExcavatorResourceSet(ExcavatorArm arm, string str, BasePlayer player);
    public void BeginMining(ExcavatorArm arm);
}


```

---

## Everlight by ohhRezyyy - Allows infinite light from configured objects by not consuming fuel

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("Everlight", "ohhRezyyy/Wulf/lukespragg/Arainrr", "3.5.1")]
[Description("Allows infinite light from configured objects by not consuming fuel")]
public class Everlight : RustPlugin
{
    private bool _enabled;
    private Dictionary<ulong, Item> _baseOvenFuelItems;
    private readonly Dictionary<ItemModBurnable, ItemDefinition> _itemModBurnables;
    private readonly Dictionary<string, EverlightEntry> _everlightItems;
    private readonly Dictionary<string, EverlightEntry> _defaultEverlightItems;
    private readonly Dictionary<string, List<string>> _hookItems;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseCombatEntity baseCombatEntity);
    private void OnLoseCondition(Item item, float amount);
    private object OnFindBurnable(BaseOven baseOven);
    private void OnEntityDistanceCheck(BaseOven baseOven, BasePlayer player, uint id, string debugName, float maximumDistance);
    private object OnItemUse(Item item, int amount);
    private bool CanEverlight(string name, string playerId);
    private void UpdateConfig();
    private ConfigData configData;
    private class EverlightEntry
    {
        public string name;
        public EverlightS everlightS;
        public EverlightEntry(string name, EverlightS everlightS);
    }

    private class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public GlobalSettings Global { get; set; }
        [JsonProperty(PropertyName = "Everlight entity list")]
        public Dictionary<string, EverlightS> EverlightList { get; set; }
    }

    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Produce Charcoal")]
        public bool ProduceCharcoal { get; set; }
    }

    private class EverlightS
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Use permission")]
        public bool UsePermission { get; set; }
        [JsonProperty(PropertyName = "Permission")]
        public string Permission { get; set; }
        public EverlightS(bool enabled, bool usePermission, string permission);
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class EverlightEntry
{
    public string name;
    public EverlightS everlightS;
    public EverlightEntry(string name, EverlightS everlightS);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public GlobalSettings Global { get; set; }
    [JsonProperty(PropertyName = "Everlight entity list")]
    public Dictionary<string, EverlightS> EverlightList { get; set; }
}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Produce Charcoal")]
    public bool ProduceCharcoal { get; set; }
}

private class EverlightS
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Use permission")]
    public bool UsePermission { get; set; }
    [JsonProperty(PropertyName = "Permission")]
    public string Permission { get; set; }
    public EverlightS(bool enabled, bool usePermission, string permission);
}


```

---

## ExcavatorLock by Lorenzo - Excavator is for PVE server, to protect looting the output piles while the excavator is running

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using System.Text;
using UnityEngine.Networking;
using Facepunch;

Oxide.Plugins
[Info("Excavator Lock", "Lorenzo", "0.4.37")]
[Description("Lock excavator to a player/team/clan when it run")]
 class ExcavatorLock : CovalencePlugin
{
    [PluginReference]
    private Plugin Clans;
    [PluginReference]
    private Plugin Notify;
    private static ExcavatorLock _instance;
    private static DiscordComponent _discord;
    private const string PermissionAdmin;
    private const ulong supplyDropSkinID;
    private const string PREFAB_ITEM_DROP;
     string TraceFile;
    const char Platform;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Use permission")]
        public bool UsePermission;
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Allow access to excavator without permission (will not lock)")]
        public bool UseExcavatorWithoutPermission;
        [JsonProperty(PropertyName = "Multiplier permission")]
        public bool MultiplierPermission;
        [JsonProperty(PropertyName = "Permission For Multiplier")]
        public string PermissionMult;
        [JsonProperty(PropertyName = "CoolDown before releasing excavator")]
        public float CoolDown;
        [JsonProperty(PropertyName = "Send Excavator available Message to All")]
        public bool MessageAll;
        [JsonProperty(PropertyName = "Enable engine loot after it is started to add diesel")]
        public bool engineloot;
        [JsonProperty(PropertyName = "Use Teams")]
        public bool UseTeams;
        [JsonProperty(PropertyName = "Use Clans plugin")]
        public bool UseClans;
        [JsonProperty(PropertyName = "Use clan table")]
        public bool UseClanTable;
        [JsonProperty(PropertyName = "CoolDown before a player or team can restart the excavator (0 is disabled)")]
        public float PlayerCoolDown;
        [JsonIgnore]
        public float _PlayerCoolDown;
        [JsonProperty(PropertyName = "Apply cooldown to all excavators")]
        public bool CoolDownGlobal;
        [JsonProperty(PropertyName = "Enable fuel modifier")]
        public bool enablefuelModifier;
        [JsonProperty(PropertyName = "Maximum stack size for diesel engine (-1 to disable function)")]
        public int DieselFuelMaxStackSize;
        [JsonProperty(PropertyName = "Enable signal Computer lock")]
        public bool enableComputeLock;
        [JsonProperty(PropertyName = "Enable signal Computer message")]
        public bool enableComputeMesssage;
        [JsonProperty(PropertyName = "Running time per fuel units (time in seconds)")]
        public float runningTimePerFuelUnit;
        [JsonProperty(PropertyName = "Sulfur production multiplier ")]
        public float SulfurMult;
        [JsonProperty(PropertyName = "HQM production multiplier")]
        public float HQMMult;
        [JsonProperty(PropertyName = "Metal production multiplier")]
        public float MetalMult;
        [JsonProperty(PropertyName = "Stone production multiplier")]
        public float StoneMult;
        [JsonProperty(PropertyName = "Excavator chat command")]
        public string excavatorquerry;
        [JsonProperty(PropertyName = "Excavator clear status")]
        public string excavatorclearstatus;
        [JsonProperty(PropertyName = "Excavator stop command")]
        public string excavatorStop;
        [JsonProperty(PropertyName = "Empty the output piles when excavator start")]
        public bool FlushOutputPiles;
        [JsonProperty(PropertyName = "Charge needed for supply drop (0 to use default of 600sec)")]
        public long chargeNeededForSupplies;
        [JsonProperty(PropertyName = "Clear excavator lock after all player from team/clan disconnect ")]
        public bool ClearExcavatorLockOnAllTeamDisconnect;
        [JsonProperty(PropertyName = "Clear excavator lock after player owner disconnect")]
        public bool ClearExcavatorLockOnPlayerOwnerDisconnect;
        [JsonProperty(PropertyName = "Time after all player disconnect before excavator clear (minutes)")]
        public long CooldownExcavatorLockOnDisconnect;
        [JsonProperty(PropertyName = "Items in report list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] ReportItems;
        [JsonProperty("Use Discord hook")]
        public bool Discordena;
        [JsonProperty("Use Discord timestamp")]
        public bool UseDiscordTimestamp;
        [JsonProperty("Discord hook url")]
        public string DiscordHookUrl;
        [JsonProperty(PropertyName = "Use Notify plugin")]
        public bool useNotify;
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
        [JsonProperty(PropertyName = "Log to file")]
        public bool LogToFile;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<ulong, ExcavatorInfo> Excavators;
    private Dictionary<ulong, ulong> Associate;
    private class ExcavatorInfo
    {
        public ulong playerid;
        public string displayName;
        [JsonIgnore]
        public double FuelTimeRemaining;
        [JsonIgnore]
        public bool state;
        public DateTime fueltime;
        public DateTime stoptime;
        public string Name;
        public Dictionary<ulong, CooldownTime> PlayerCooldown;
        [JsonIgnore]
        public DateTime PlayerDisconnectTime;
        [JsonIgnore]
        public int CountTick;
        [JsonIgnore]
        public ExcavatorArm Arm;
        [JsonIgnore]
        public Timer timerStopped;
        [JsonIgnore]
        public float[] extraMineralProduced;
        [JsonIgnore]
        public int pileIndex;
        public ExcavatorInfo();
    }

    public class CooldownTime
    {
        public bool IsOnline;
        public bool IgnoreOnline;
        public DateTime Time;
        public CooldownTime();
    }

    private void LoadData();
    private void SaveData();
    private string Lang(string key, string id, object[] args);
    private new void LoadDefaultMessages();
    private void Unload();
    private void Init();
    private void OnServerInitialized(bool initial);
     object OnDieselEngineToggle(DieselEngine engine, BasePlayer player);
     double CalcFuelTime(DieselEngine engine);
     void OnDieselEngineToggled(DieselEngine engine);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     void OnLootEntityEnd(BasePlayer player, StorageContainer container);
     void LootDieselEngineEnd(BasePlayer player, StorageContainer container);
     object LootExcavatorOutputPile(BasePlayer player, StorageContainer container);
     object LootDieselEngine(BasePlayer player, StorageContainer container);
     object LootAirdrop(BasePlayer player, StorageContainer container);
     object OnExcavatorResourceSet(ExcavatorArm arm, string resourceName, BasePlayer player);
     object OnExcavatorGather(ExcavatorArm arm, Item item);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string strReason);
     bool CheckPlayerCooldown(ulong playerID, ExcavatorInfo excavatorinfo);
     int PlayerCooldownTime(ulong playerID, ExcavatorInfo excavatorinfo);
     void AddPlayerCooldown(ulong playerID, double extra, ExcavatorInfo excavatorinfo);
     void AddSinglePlayerCooldown(ulong playerID, ExcavatorInfo excavatorinfo, CooldownTime Cooldown);
     object OnExcavatorSuppliesRequest(ExcavatorSignalComputer computer, BasePlayer player);
     void OnExcavatorSuppliesRequested(ExcavatorSignalComputer computer, BasePlayer player, CargoPlane plane);
    private void Excavator_info(IPlayer iplayer, string command, string[] args);
    private void Excavator_clearstatus(IPlayer iplayer, string command, string[] args);
    private void Excavator_stopcommand(IPlayer iplayer, string command, string[] args);
     void ClearExcavator(ExcavatorInfo excavator, bool clearcooldownlist);
     bool IsExcavatorAvailable(ExcavatorInfo excavatorinfo);
    private static void DropItemContainer(ItemContainer itemContainer, Vector3 dropPosition, Quaternion rotation);
     string ReportInventory(StorageContainer storage);
    private void PrintToLog(string message);
    private bool IsAdmin(ulong id);
    private bool IsAdmin(string id);
    private bool CanPlayerUse(ulong id);
    private bool CanPlayerUse(string id);
    private bool IsMultiplierActive(ulong id);
    private bool IsMultiplierActive(string id);
    private bool SameClan(ulong playerID, ulong friendID);
    private bool SameTeam(ulong playerID, ulong friendID);
    private void BroadcastMessage(string msg, object[] args);
    public void SendChatMessage(BasePlayer player, string msg, object[] args);
    private void PrintToDiscord(string message, double seconds);
    private string PositionToString(Vector3 position);
     bool IspluginLoaded(Plugin a);
    private class DiscordComponent : MonoBehaviour
    {
        private const float PostDelay;
        public long MsgCooldown;
        private readonly Queue<object> _queue;
        private string _url;
        private bool _busy;
        public DiscordComponent Configure(string url);
        public DiscordComponent SendTextMessage(string message, object[] args);
        private DiscordComponent AddQueue(object request);
        private IEnumerator ProcessQueue();
        private IEnumerator ProcessRequest(object request);
        private class MessageRequest
        {
            [JsonProperty("content")]
            public string Content { get; set; }
            public MessageRequest(string content);
        }

    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "Use permission")]
    public bool UsePermission;
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Allow access to excavator without permission (will not lock)")]
    public bool UseExcavatorWithoutPermission;
    [JsonProperty(PropertyName = "Multiplier permission")]
    public bool MultiplierPermission;
    [JsonProperty(PropertyName = "Permission For Multiplier")]
    public string PermissionMult;
    [JsonProperty(PropertyName = "CoolDown before releasing excavator")]
    public float CoolDown;
    [JsonProperty(PropertyName = "Send Excavator available Message to All")]
    public bool MessageAll;
    [JsonProperty(PropertyName = "Enable engine loot after it is started to add diesel")]
    public bool engineloot;
    [JsonProperty(PropertyName = "Use Teams")]
    public bool UseTeams;
    [JsonProperty(PropertyName = "Use Clans plugin")]
    public bool UseClans;
    [JsonProperty(PropertyName = "Use clan table")]
    public bool UseClanTable;
    [JsonProperty(PropertyName = "CoolDown before a player or team can restart the excavator (0 is disabled)")]
    public float PlayerCoolDown;
    [JsonIgnore]
    public float _PlayerCoolDown;
    [JsonProperty(PropertyName = "Apply cooldown to all excavators")]
    public bool CoolDownGlobal;
    [JsonProperty(PropertyName = "Enable fuel modifier")]
    public bool enablefuelModifier;
    [JsonProperty(PropertyName = "Maximum stack size for diesel engine (-1 to disable function)")]
    public int DieselFuelMaxStackSize;
    [JsonProperty(PropertyName = "Enable signal Computer lock")]
    public bool enableComputeLock;
    [JsonProperty(PropertyName = "Enable signal Computer message")]
    public bool enableComputeMesssage;
    [JsonProperty(PropertyName = "Running time per fuel units (time in seconds)")]
    public float runningTimePerFuelUnit;
    [JsonProperty(PropertyName = "Sulfur production multiplier ")]
    public float SulfurMult;
    [JsonProperty(PropertyName = "HQM production multiplier")]
    public float HQMMult;
    [JsonProperty(PropertyName = "Metal production multiplier")]
    public float MetalMult;
    [JsonProperty(PropertyName = "Stone production multiplier")]
    public float StoneMult;
    [JsonProperty(PropertyName = "Excavator chat command")]
    public string excavatorquerry;
    [JsonProperty(PropertyName = "Excavator clear status")]
    public string excavatorclearstatus;
    [JsonProperty(PropertyName = "Excavator stop command")]
    public string excavatorStop;
    [JsonProperty(PropertyName = "Empty the output piles when excavator start")]
    public bool FlushOutputPiles;
    [JsonProperty(PropertyName = "Charge needed for supply drop (0 to use default of 600sec)")]
    public long chargeNeededForSupplies;
    [JsonProperty(PropertyName = "Clear excavator lock after all player from team/clan disconnect ")]
    public bool ClearExcavatorLockOnAllTeamDisconnect;
    [JsonProperty(PropertyName = "Clear excavator lock after player owner disconnect")]
    public bool ClearExcavatorLockOnPlayerOwnerDisconnect;
    [JsonProperty(PropertyName = "Time after all player disconnect before excavator clear (minutes)")]
    public long CooldownExcavatorLockOnDisconnect;
    [JsonProperty(PropertyName = "Items in report list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] ReportItems;
    [JsonProperty("Use Discord hook")]
    public bool Discordena;
    [JsonProperty("Use Discord timestamp")]
    public bool UseDiscordTimestamp;
    [JsonProperty("Discord hook url")]
    public string DiscordHookUrl;
    [JsonProperty(PropertyName = "Use Notify plugin")]
    public bool useNotify;
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
    [JsonProperty(PropertyName = "Log to file")]
    public bool LogToFile;
}

private class ExcavatorInfo
{
    public ulong playerid;
    public string displayName;
    [JsonIgnore]
    public double FuelTimeRemaining;
    [JsonIgnore]
    public bool state;
    public DateTime fueltime;
    public DateTime stoptime;
    public string Name;
    public Dictionary<ulong, CooldownTime> PlayerCooldown;
    [JsonIgnore]
    public DateTime PlayerDisconnectTime;
    [JsonIgnore]
    public int CountTick;
    [JsonIgnore]
    public ExcavatorArm Arm;
    [JsonIgnore]
    public Timer timerStopped;
    [JsonIgnore]
    public float[] extraMineralProduced;
    [JsonIgnore]
    public int pileIndex;
    public ExcavatorInfo();
}

public class CooldownTime
{
    public bool IsOnline;
    public bool IgnoreOnline;
    public DateTime Time;
    public CooldownTime();
}

private class DiscordComponent : MonoBehaviour
{
    private const float PostDelay;
    public long MsgCooldown;
    private readonly Queue<object> _queue;
    private string _url;
    private bool _busy;
    public DiscordComponent Configure(string url);
    public DiscordComponent SendTextMessage(string message, object[] args);
    private DiscordComponent AddQueue(object request);
    private IEnumerator ProcessQueue();
    private IEnumerator ProcessRequest(object request);
    private class MessageRequest
    {
        [JsonProperty("content")]
        public string Content { get; set; }
        public MessageRequest(string content);
    }

}

private class MessageRequest
{
    [JsonProperty("content")]
    public string Content { get; set; }
    public MessageRequest(string content);
}


```

---

## ExclusiveGroups by WhiteDragon - Maintains exclusive group membership within categories of permission groups

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Exclusive Groups", "WhiteDragon", "0.2.8")]
[Description("Maintains exclusive group membership within categories.")]
internal class ExclusiveGroups : CovalencePlugin
{
    private static ExclusiveGroups _instance;
    private class Category
    {
        public class Settings : Dictionary<string, Settings.Entry>
        {
            public class Entry
            {
                public Groups Includes;
                public Groups Excludes;
                public class Groups : List<string>
                {
                    public bool Validate(string category, string type);
                }

                public bool Validate(string category);
            }

            public bool Validate();
        }

        private static void Remove(IPlayer user, string category, string type, string group);
        public static void Update();
        public static void Update(string userid, string added);
        public static bool Validate();
    }

    private static Configuration config;
    private class Configuration
    {
        public Category.Settings Categories;
        public Version.Settings Version;
        private static bool corrupt;
        private static bool dirty;
        private static bool upgraded;
        public static void Load();
        public static void Save();
        public static void SetDirty();
        public static void SetUpgrade(bool upgrade);
        public static void Unload();
        public static bool Upgraded();
        public static void Validate(T value, Func<T> initializer, Action validator);
        private static void Validate();
    }

    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Loaded();
    private void OnUserGroupAdded(string userid, string group);
    private void Unload();
    private new class Log
    {
        public static void Console(Key key, Dictionary<string, string> parameters);
    }

    private class Text
    {
        private static readonly Dictionary<Key, string> cache;
        private static Dictionary<string, Dictionary<string, string>> messages;
        public static string Get(Key key, Dictionary<string, string> parameters);
        public static void Load();
        public static void Preload();
        private static void RegisterMessages();
        private static string Replace(string message, Dictionary<string, string> parameters);
        public static void Unload();
    }

    private new class Version
    {
        public class Settings
        {
            public int Major;
            public int Minor;
            public int Patch;
            public Settings();
            public int Compare(int major, int minor, int patch);
            public void Validate();
        }

    }

}

private class Category
{
    public class Settings : Dictionary<string, Settings.Entry>
    {
        public class Entry
        {
            public Groups Includes;
            public Groups Excludes;
            public class Groups : List<string>
            {
                public bool Validate(string category, string type);
            }

            public bool Validate(string category);
        }

        public bool Validate();
    }

    private static void Remove(IPlayer user, string category, string type, string group);
    public static void Update();
    public static void Update(string userid, string added);
    public static bool Validate();
}

public class Settings : Dictionary<string, Settings.Entry>
{
    public class Entry
    {
        public Groups Includes;
        public Groups Excludes;
        public class Groups : List<string>
        {
            public bool Validate(string category, string type);
        }

        public bool Validate(string category);
    }

    public bool Validate();
}

public class Entry
{
    public Groups Includes;
    public Groups Excludes;
    public class Groups : List<string>
    {
        public bool Validate(string category, string type);
    }

    public bool Validate(string category);
}

public class Groups : List<string>
{
    public bool Validate(string category, string type);
}

private class Configuration
{
    public Category.Settings Categories;
    public Version.Settings Version;
    private static bool corrupt;
    private static bool dirty;
    private static bool upgraded;
    public static void Load();
    public static void Save();
    public static void SetDirty();
    public static void SetUpgrade(bool upgrade);
    public static void Unload();
    public static bool Upgraded();
    public static void Validate(T value, Func<T> initializer, Action validator);
    private static void Validate();
}

private new class Log
{
    public static void Console(Key key, Dictionary<string, string> parameters);
}

private class Text
{
    private static readonly Dictionary<Key, string> cache;
    private static Dictionary<string, Dictionary<string, string>> messages;
    public static string Get(Key key, Dictionary<string, string> parameters);
    public static void Load();
    public static void Preload();
    private static void RegisterMessages();
    private static string Replace(string message, Dictionary<string, string> parameters);
    public static void Unload();
}

private new class Version
{
    public class Settings
    {
        public int Major;
        public int Minor;
        public int Patch;
        public Settings();
        public int Compare(int major, int minor, int patch);
        public void Validate();
    }

}

public class Settings
{
    public int Major;
    public int Minor;
    public int Patch;
    public Settings();
    public int Compare(int major, int minor, int patch);
    public void Validate();
}


```

---

## ExclusiveLooter by redBDGR - Allows only one player to loot an entity at a time

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("ExclusiveLooter", "redBDGR", "1.0.0")]
[Description("Allow only one player to loot an entity at a time")]
 class ExclusiveLooter : RustPlugin
{
    private bool Changed;
    private const string permissionNameEXEMPT;
    private Dictionary<BaseEntity, string> containerDic;
    private List<object> entityBlacklist;
    static List<object> entityBlacklistGet();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void Init();
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
}


```

---

## ExplodingOilBarrels by Bazz3l - Exploding oil barrels with ground shaking effect for nearby players

```csharp
using System.Collections.Generic;
using Rust;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Exploding Oil Barrel", "Bazz3l", "1.1.0")]
[Description("Exploding oil barrels with explosion force, player damage and ground shake effect")]
public class ExplodingOilBarrels : RustPlugin
{
    private const string EXPLOSION_EFFECT;
    private const string FIRE_EFFECT;
    private const string SHAKE_EFFECT;
    private readonly int _playerMask;
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Screen shake effect for explosion")]
        public bool EnableShakeScreen;
        [JsonProperty(PropertyName = "Moves items in range of explosion")]
        public bool EnableExplosionForce;
        [JsonProperty(PropertyName = "Deal damage to players in distance of explosion")]
        public bool EnablePlayerDamage;
        [JsonProperty(PropertyName = "ditance to deal damage to players")]
        public float PlayerDamageDistance;
        [JsonProperty(PropertyName = "amount of damage delt to players in range")]
        public float PlayerDamage;
        [JsonProperty(PropertyName = "distance shake will effect players from explosion")]
        public float ShakeDistance;
        [JsonProperty(PropertyName = "amount of force delt to object in range")]
        public float ExplosionForce;
        [JsonProperty(PropertyName = "distance to find object near explosion")]
        public float ExplosionItemDistance;
        [JsonProperty(PropertyName = "distance to find targets near explosion")]
        public float ExplosionRange;
        public static PluginConfig DefaultConfig();
    }

    private void OnEntityDeath(LootContainer entity, HitInfo info);
    private void DoExplosion(Vector3 position);
    private void MoveItems(Vector3 position);
    private void PlayerInRange(Vector3 position);
    private void DamagePlayer(BasePlayer player);
    private static bool InDistance(Vector3 target, Vector3 position, float distance);
    private static void PlayerShake(BasePlayer player);
    private static void PlayExplosion(Vector3 position);
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Screen shake effect for explosion")]
    public bool EnableShakeScreen;
    [JsonProperty(PropertyName = "Moves items in range of explosion")]
    public bool EnableExplosionForce;
    [JsonProperty(PropertyName = "Deal damage to players in distance of explosion")]
    public bool EnablePlayerDamage;
    [JsonProperty(PropertyName = "ditance to deal damage to players")]
    public float PlayerDamageDistance;
    [JsonProperty(PropertyName = "amount of damage delt to players in range")]
    public float PlayerDamage;
    [JsonProperty(PropertyName = "distance shake will effect players from explosion")]
    public float ShakeDistance;
    [JsonProperty(PropertyName = "amount of force delt to object in range")]
    public float ExplosionForce;
    [JsonProperty(PropertyName = "distance to find object near explosion")]
    public float ExplosionItemDistance;
    [JsonProperty(PropertyName = "distance to find targets near explosion")]
    public float ExplosionRange;
    public static PluginConfig DefaultConfig();
}


```

---

## ExplosionDamageReducer by Paulsimik - Set damage value for Basic Rockets, High Velocity Rockets and HE Grenades only to players

```csharp
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Explosion Damage Reducer", "paulsimik", "1.0.0")]
[Description("Set damage value for Rockets, High Velocity Rockets and HE Grenades only to players")]
 class ExplosionDamageReducer : RustPlugin
{
    private void OnEntityTakeDamage(BasePlayer victim, HitInfo info);
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Apply reduced damage to the attacker")]
        public bool attackerReduceDamage;
        [JsonProperty(PropertyName = "Rocket")]
        public int rocket;
        [JsonProperty(PropertyName = "High Velocity Rocket")]
        public int hvRocket;
        [JsonProperty(PropertyName = "HE Grenade")]
        public int heGrenade;
        public VersionNumber version;
    }

    private Configuration GetDefaultConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Apply reduced damage to the attacker")]
    public bool attackerReduceDamage;
    [JsonProperty(PropertyName = "Rocket")]
    public int rocket;
    [JsonProperty(PropertyName = "High Velocity Rocket")]
    public int hvRocket;
    [JsonProperty(PropertyName = "HE Grenade")]
    public int heGrenade;
    public VersionNumber version;
}


```

---

## ExplosionTracker by  - Tracks and logs every explosion that happens on the server

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Explosion Tracker", "Ryan", "1.0.2")]
[Description("Tracks and logs every explosion that happens on the server")]
internal class ExplosionTracker : RustPlugin
{
    [PluginReference]
    private Plugin Slack;
    private Plugin Discord;
    private ConfigFile ConfigData;
    public class ConfigFile
    {
        [JsonProperty(PropertyName = "Enable RCON messages")]
        public bool RCON;
        [JsonProperty(PropertyName = "Enable Log messages")]
        public bool Log;
        [JsonProperty(PropertyName = "Send messages to online admins")]
        public bool AdminMsg;
        [JsonProperty(PropertyName = "Enable Discord messages")]
        public bool Discord;
        [JsonProperty(PropertyName = "Slack Settings")]
        public SlackInfo SlackInfo;
        [JsonProperty(PropertyName = "Enable rocket launch notifications")]
        public bool Launch;
        [JsonProperty(PropertyName = "Enable explosive throw notifications")]
        public bool Throw;
        [JsonProperty(PropertyName = "Enable explosive drop notifications")]
        public bool Drop;
        public static ConfigFile DefaultConfig();
    }

    public class SlackInfo
    {
        [JsonProperty(PropertyName = "Enable Slack messages")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Channel name")]
        public string Channel { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private void Log(string message, string action);
    private void SendSlack(string message, BasePlayer player);
    private void SendDiscord(string message);
    private string ThrowMsg(BasePlayer player, BaseEntity entity);
    private string LaunchMsg(BasePlayer player, BaseEntity entity);
    private string DropMsg(BasePlayer player, BaseEntity entity);
    private void SendMessages(string message, Actions action, BasePlayer player);
    private void ConstructMessage(BasePlayer player, BaseEntity entity, Actions action);
    private void Init();
    private void OnServerInitialized();
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity);
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "Enable RCON messages")]
    public bool RCON;
    [JsonProperty(PropertyName = "Enable Log messages")]
    public bool Log;
    [JsonProperty(PropertyName = "Send messages to online admins")]
    public bool AdminMsg;
    [JsonProperty(PropertyName = "Enable Discord messages")]
    public bool Discord;
    [JsonProperty(PropertyName = "Slack Settings")]
    public SlackInfo SlackInfo;
    [JsonProperty(PropertyName = "Enable rocket launch notifications")]
    public bool Launch;
    [JsonProperty(PropertyName = "Enable explosive throw notifications")]
    public bool Throw;
    [JsonProperty(PropertyName = "Enable explosive drop notifications")]
    public bool Drop;
    public static ConfigFile DefaultConfig();
}

public class SlackInfo
{
    [JsonProperty(PropertyName = "Enable Slack messages")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Channel name")]
    public string Channel { get; set; }
}


```

---

## ExplosiveBarrels by  - Allows barrels to blow up on damage or death

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Explosive Barrels", "Orange", "1.0.1")]
[Description("Allows barrels to blow up on damage or death")]
public class ExplosiveBarrels : RustPlugin
{
    private List<string> barrels;
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void CheckBarrel(BaseCombatEntity entity, bool death);
    private void Explode(BaseCombatEntity barrel, ConfigData.OBarrel config);
    private bool IsBarrel(BaseEntity entity);
    private bool IsOilBarrel(BaseEntity entity);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. NORMAL barrels settings:")]
        public OBarrel normal;
        [JsonProperty(PropertyName = "2. OIL barrels settings:")]
        public OBarrel oil;
        public class OBarrel
        {
            [JsonProperty(PropertyName = "1. Chance to explode on taking damage (set to 0 to disable)")]
            public int damage;
            [JsonProperty(PropertyName = "2. Chance to explode on death (set to 0 to disable)")]
            public int death;
            [JsonProperty(PropertyName = "3. Damage on explosion (set to 0 to disable)")]
            public int eDamage;
            [JsonProperty(PropertyName = "4. Radius of damage on explosion")]
            public int eRadius;
            [JsonProperty(PropertyName = "5. List of effects on explosion")]
            public List<string> effects;
        }

    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. NORMAL barrels settings:")]
    public OBarrel normal;
    [JsonProperty(PropertyName = "2. OIL barrels settings:")]
    public OBarrel oil;
    public class OBarrel
    {
        [JsonProperty(PropertyName = "1. Chance to explode on taking damage (set to 0 to disable)")]
        public int damage;
        [JsonProperty(PropertyName = "2. Chance to explode on death (set to 0 to disable)")]
        public int death;
        [JsonProperty(PropertyName = "3. Damage on explosion (set to 0 to disable)")]
        public int eDamage;
        [JsonProperty(PropertyName = "4. Radius of damage on explosion")]
        public int eRadius;
        [JsonProperty(PropertyName = "5. List of effects on explosion")]
        public List<string> effects;
    }

}

public class OBarrel
{
    [JsonProperty(PropertyName = "1. Chance to explode on taking damage (set to 0 to disable)")]
    public int damage;
    [JsonProperty(PropertyName = "2. Chance to explode on death (set to 0 to disable)")]
    public int death;
    [JsonProperty(PropertyName = "3. Damage on explosion (set to 0 to disable)")]
    public int eDamage;
    [JsonProperty(PropertyName = "4. Radius of damage on explosion")]
    public int eRadius;
    [JsonProperty(PropertyName = "5. List of effects on explosion")]
    public List<string> effects;
}


```

---

## ExplosiveFuseChanger by Hockeygel23 - Change explosive fuse times

```csharp
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Explosive Fuse Changer", "Hockeygel23", "0.0.3")]
[Description("Changes the fuse time of certain explosives")]
public class ExplosiveFuseChanger : RustPlugin
{
    private const string Permission;
    private const string C4PrefabName;
    private const string SatchelPrefabName;
    private const string BeanCanPrefabName;
    private const string F1NadePrefabName;
    private const string SmokeNadePrefabName;
    private const string SurveyChargePrefabName;
    private static ConfigData config;
    private void Init();
    private class ConfigData
    {
        [JsonProperty("C4 Fuse Time")]
        public float C4FuseTime;
        [JsonProperty("Satchel Fuse Time")]
        public float SatchelFuseTime;
        [JsonProperty( "BeanCan Fuse Time")]
        public float BeancanFuseTime;
        [JsonProperty("Survey Charge Fuse Time")]
        public float SurveyChargeFuseTime;
        [JsonProperty("F1 Explosive Time")]
        public float F1NadeFuseTime;
        [JsonProperty("Smoke Granade Fuse Time")]
        public float SmokeNadeFuseTime;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity, ThrownWeapon item);
    private void Unload();
}

private class ConfigData
{
    [JsonProperty("C4 Fuse Time")]
    public float C4FuseTime;
    [JsonProperty("Satchel Fuse Time")]
    public float SatchelFuseTime;
    [JsonProperty( "BeanCan Fuse Time")]
    public float BeancanFuseTime;
    [JsonProperty("Survey Charge Fuse Time")]
    public float SurveyChargeFuseTime;
    [JsonProperty("F1 Explosive Time")]
    public float F1NadeFuseTime;
    [JsonProperty("Smoke Granade Fuse Time")]
    public float SmokeNadeFuseTime;
}


```

---

## ExplosiveRounds by redBDGR - Gives explosive bullets effects on impact

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("ExplosiveRounds", "redBDGR", "1.0.1")]
[Description("Give explosive bullets impact explosions")]
internal class ExplosiveRounds : RustPlugin
{
    private const string permissionName;
    private List<object> ammotypes;
    private bool Changed;
    public string explosioneffectuse;
    private static List<object> AmmoTypes();
    private void Init();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void OnPlayerAttack(BasePlayer player, HitInfo info);
    private object GetConfig(string menu, string datavalue, object defaultValue);
}


```

---

## ExplosivesModifier by  - Allows you to modify explosive damage and radius

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;

Oxide.Plugins
[Info("Explosives Modifier", "Mughisi", "1.3.2", ResourceId = 832)]
[Description("Allows you to modify explosive damage and radius")]
 class ExplosivesModifier : RustPlugin
{
    private bool configChanged;
    private const string DefaultChatPrefix;
    private const string DefaultChatPrefixColor;
    public string ChatPrefix { get; set; }
    public string ChatPrefixColor { get; set; }
    private const float DefaultExplosiveChargeDamageModifier;
    private const float DefaultExplosiveChargeRadiusModifier;
    public float ExplosiveChargeDamageModifier { get; set; }
    public float ExplosiveChargeRadiusModifier { get; set; }
    private const float DefaultF1GrenadeDamageModifier;
    private const float DefaultF1GrenadeRadiusModifier;
    private const bool DefaultF1GrenadeSticky;
    public float F1GrenadeDamageModifier { get; set; }
    public float F1GrenadeRadiusModifier { get; set; }
    public bool F1GrenadeSticky { get; set; }
    private const float DefaultBeancanGrenadeDamageModifier;
    private const float DefaultBeancanGrenadeRadiusModifier;
    private const bool DefaultBeancanGrenadeSticky;
    public float BeancanGrenadeDamageModifier { get; set; }
    public float BeancanGrenadeRadiusModifier { get; set; }
    public bool BeancanGrenadeSticky { get; set; }
    private const float DefaultRocketDamageModifier;
    private const float DefaultRocketRadiusModifier;
    public float RocketDamageModifier { get; set; }
    public float RocketRadiusModifier { get; set; }
    private const float DefaultExplosiveAmmoDamageModifier;
    public float ExplosiveAmmoDamageModifier { get; set; }
    private const string DefaultHelpTextPlayersExplosiveCharge;
    private const string DefaultHelpTextPlayersF1Grenade;
    private const string DefaultHelpTextPlayersBeancanGrenade;
    private const string DefaultHelpTextPlayersRocket;
    private const string DefaultHelpTextPlayersExplosiveAmmo;
    private const string DefaultHelpTextAdmins;
    private const string DefaultModified;
    private const string DefaultSticky;
    private const string DefaultNotAllowed;
    private const string DefaultInvalidArguments;
    public string HelpTextPlayersExplosiveCharge { get; set; }
    public string HelpTextPlayersF1Grenade { get; set; }
    public string HelpTextPlayersBeancanGrenade { get; set; }
    public string HelpTextPlayersRocket { get; set; }
    public string HelpTextPlayersExplosiveAmmo { get; set; }
    public string HelpTextAdmins { get; set; }
    public string Modified { get; set; }
    public string Sticky { get; set; }
    public string NotAllowed { get; set; }
    public string InvalidArguments { get; set; }
    private readonly string[] weaponDamageTypes;
    private readonly string[] weaponRadiusTypes;
    protected override void LoadDefaultConfig();
    private void Init();
    private void LoadConfiguration();
    [ChatCommand("explosivedamage")]
    private void ExplosiveDamage(BasePlayer player, string command, string[] args);
    [ChatCommand("explosiveradius")]
    private void ExplosiveRadius(BasePlayer player, string command, string[] args);
    [ChatCommand("stickygrenades")]
    private void StickyGrenades(BasePlayer player, string command, string[] args);
    private void ChangeExplosive(BasePlayer player, string command, string[] args, string type);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private void OnEntityTakeDamage(BaseEntity entity, HitInfo info);
    private void SendHelpText(BasePlayer player);
    private bool IsValidType(string command, string type);
    private void SendChatMessage(BasePlayer player, string message, object[] arguments);
     bool IsAllowed(BasePlayer player);
     T GetConfigValue(string category, string setting, T defaultValue);
    private void SetConfigValue(string category, string setting, object newValue);
}


```

---

## ExtendedCrafting by Judess69er - Craft many useful prefabs/items that you couldn't otherwise craft and place

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Extended Crafting", "nivex & judess69er", "0.3.6")]
[Description("Allows players to deploy additional entities.")]
 class ExtendedCrafting : RustPlugin
{
    private static Dictionary<NetworkableId, ExtendedController> _controllers;
    private const int _layerMask;
    private List<int> _layers;
    private const string permAllow;
    private const string permFree;
    private Dictionary<ulong, ExtendedInfo> _skins;
    private static ExtendedCrafting Instance;
    private RaycastHit _hit;
    public class ExtendedController : FacepunchBehaviour
    {
        internal BaseCombatEntity entity;
        internal RaycastHit hit;
        internal ExtendedInfo ei;
        internal NetworkableId uid;
        private void Awake();
        private void CheckGround();
        public void TryPickup(BasePlayer player);
        private void OnDestroy();
    }

    private void Init();
    private void OnServerInitialized(bool isStartup);
    private void Unload();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void OnHammerHit(BasePlayer player, HitInfo hitInfo);
    private object CanBuild(Planner planner, Construction construction, Construction.Target target);
    private void CommandExtended(IPlayer p, string command, string[] args);
    private static void Message(BasePlayer player, string key, object[] args);
    private static string _(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    public class CraftItem
    {
        public string shortname;
        public int amount;
        public ulong skin;
        internal ItemDefinition _definition;
        internal int itemid { get; set; }
        internal string displayName { get; set; }
        public CraftItem(string shortname, int amount, ulong skin);
        internal ItemDefinition definition { get; set; }
    }

    public class CraftSettings
    {
        [JsonProperty("items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<CraftItem> items;
        public bool enabled;
        public CraftSettings(bool enabled);
    }

    public class DestroySettings
    {
        [JsonProperty("effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> effects;
        public bool give;
        public bool ground;
        public DestroySettings(bool give, bool ground);
    }

    public class PickupSettings
    {
        public bool enabled;
        public bool owner;
        public bool privilege;
        public PickupSettings(bool enabled, bool owner, bool privilege);
    }

    public class ExtendedInfo
    {
        public CraftSettings craft;
        public DestroySettings destroy;
        public PickupSettings pickup;
        public CraftItem item;
        public string prefab;
        public string itemname;
        public string command;
        public bool land;
        public float distance;
        public Item CreateItem();
        public static void GiveItem(IPlayer server, string command, string[] args);
        public void GiveItem(BasePlayer player, bool pickup);
        public void GiveItem(Vector3 position);
        public bool CanCraft(BasePlayer player);
        public void Spawn(BaseEntity other);
    }

    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Entities", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, ExtendedInfo> entities { get; set; }
        [JsonProperty(PropertyName = "Steam ChatID")]
        public ulong chatid { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Configuration GetDefaultConfig();
}

public class ExtendedController : FacepunchBehaviour
{
    internal BaseCombatEntity entity;
    internal RaycastHit hit;
    internal ExtendedInfo ei;
    internal NetworkableId uid;
    private void Awake();
    private void CheckGround();
    public void TryPickup(BasePlayer player);
    private void OnDestroy();
}

public class CraftItem
{
    public string shortname;
    public int amount;
    public ulong skin;
    internal ItemDefinition _definition;
    internal int itemid { get; set; }
    internal string displayName { get; set; }
    public CraftItem(string shortname, int amount, ulong skin);
    internal ItemDefinition definition { get; set; }
}

public class CraftSettings
{
    [JsonProperty("items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<CraftItem> items;
    public bool enabled;
    public CraftSettings(bool enabled);
}

public class DestroySettings
{
    [JsonProperty("effects", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> effects;
    public bool give;
    public bool ground;
    public DestroySettings(bool give, bool ground);
}

public class PickupSettings
{
    public bool enabled;
    public bool owner;
    public bool privilege;
    public PickupSettings(bool enabled, bool owner, bool privilege);
}

public class ExtendedInfo
{
    public CraftSettings craft;
    public DestroySettings destroy;
    public PickupSettings pickup;
    public CraftItem item;
    public string prefab;
    public string itemname;
    public string command;
    public bool land;
    public float distance;
    public Item CreateItem();
    public static void GiveItem(IPlayer server, string command, string[] args);
    public void GiveItem(BasePlayer player, bool pickup);
    public void GiveItem(Vector3 position);
    public bool CanCraft(BasePlayer player);
    public void Spawn(BaseEntity other);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Entities", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, ExtendedInfo> entities { get; set; }
    [JsonProperty(PropertyName = "Steam ChatID")]
    public ulong chatid { get; set; }
}


```

---

## ExtendedDisconnectInfo by Strrobez - Customize reconnect/disconnect information for wrong protocols, bans & world file mismatches.

```csharp
using Rust;
using System;
using Network;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Extended Disconnect Info", "Strrobez", "2.0.2")]
[Description("Extends the disconnect reasons with more information upon reconnecting.")]
public class ExtendedDisconnectInfo : RustPlugin
{
    private readonly Dictionary<ulong, string> CachedPlayers;
    private int CachedServerProtocol;
    private static Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Log All Disconnect Messages")]
        public bool LogAllDisconnects;
        [JsonProperty(PropertyName = "Show Ban Reason on Reconnect? (if false, shows message in language file)")]
        public bool ShowBanReason;
        [JsonProperty(PropertyName = "Cache Removal Interval")]
        public float CacheRemovalInterval;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private object CanClientLogin(Connection connection);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerBanned(ulong userID, string reason);
    protected override void LoadDefaultMessages();
}

public class Configuration
{
    [JsonProperty(PropertyName = "Log All Disconnect Messages")]
    public bool LogAllDisconnects;
    [JsonProperty(PropertyName = "Show Ban Reason on Reconnect? (if false, shows message in language file)")]
    public bool ShowBanReason;
    [JsonProperty(PropertyName = "Cache Removal Interval")]
    public float CacheRemovalInterval;
    public static Configuration DefaultConfig();
}


```

---

## ExtendedRecycler by TheFriendlyChap - Extend recyclers for personal use and more

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Extended Recycler", "beee/The Friendly Chap", "1.2.6")]
[Description("Extend recyclers for personal use with per player limits")]
public class ExtendedRecycler : RustPlugin
{
    private const ulong skinID;
    private const string prefab;
    private static ExtendedRecycler plugin;
    private const string permUse;
    private const string permUnlimited;
    private const string permVIP;
     RecyclersData recData;
    private DynamicConfigFile data;
     ProtectionProperties recyclerProtection;
     ProtectionProperties originalRecyclerProtection;
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. Pickup settings:")]
        public OPickup pickup;
        [JsonProperty(PropertyName = "2. Craft settings:")]
        public OCraft craft;
        [JsonProperty(PropertyName = "3. Destroy settings:")]
        public ODestroy destroy;
        [JsonProperty(PropertyName = "4. Damage settings:")]
        public ODamage damage;
        public class OPickup
        {
            [JsonProperty(PropertyName = "1. Enabled for personal recyclers (placed by player)")]
            public bool personal;
            [JsonProperty(PropertyName = "2. Check ability to build for pickup")]
            public bool privilege;
            [JsonProperty(PropertyName = "3. Only owner can pickup")]
            public bool onlyOwner;
        }

        public class OCraft
        {
            [JsonProperty(PropertyName = "1. Enabled")]
            public bool enabled;
            [JsonProperty(PropertyName = "2. Cost (shortname - amount):")]
            public Dictionary<string, int> cost;
            [JsonProperty(PropertyName = "3. Default Balance per Player:")]
            public int defaultBalance;
            [JsonProperty(PropertyName = "3. VIP Balance per Player:")]
            public int VIPBalance;
        }

        public class ODestroy
        {
            [JsonProperty(PropertyName = "1. Check ground for recyclers (destroy on missing)")]
            public bool checkGround;
            [JsonProperty(PropertyName = "2. Give item on destroy recycler")]
            public bool destroyItem;
            [JsonProperty(PropertyName = "3. Effects on destroy recycler")]
            public List<string> effects;
        }

        public class ODamage
        {
            [JsonProperty(PropertyName = "1. Allow damage to recycler")]
            public bool allowDamage;
            [JsonProperty(PropertyName = "2. Reduce damage to recycler by %")]
            public float reduceDamagePercent;
        }

    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<string, string> EN;
    private void message(BasePlayer player, string key, object[] args);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void OnServerInitialized();
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void Unload();
    private void OnServerSave();
    private BaseEntity SpawnRecycler(Vector3 position, Quaternion rotation, ulong ownerID);
    private void CheckRecyclers();
    private void GiveRecycler(BasePlayer player, bool pickup, bool free);
    private void GiveRecycler(Vector3 position);
    private Item CreateItem();
    private void CheckDeploy(BaseEntity entity);
    private void CheckHit(BasePlayer player, BaseEntity entity);
    [ChatCommand("recycler.craft")]
    private void Craft(BasePlayer player);
    [ChatCommand("recycler.balance")]
    private void Balance(BasePlayer player);
    private bool CanCraft(BasePlayer player);
    private string GetRecyclerName();
    private bool IsRecycler(ulong skin);
    private void CheckEntry(BasePlayer player);
    private void CheckEntry(ulong playerId);
    public int SpendFromBalance(BasePlayer player);
    private void SetBalance(BasePlayer player, int newBalance);
    private int AddBalance(BasePlayer player, int deposit);
    private int GetBalance(BasePlayer player);
    private void ShowLogo();
    [ConsoleCommand("recycler.give")]
    private void Cmd(ConsoleSystem.Arg arg);
    [ConsoleCommand("recycler.setbalance")]
    private void SetBalanceCMD(ConsoleSystem.Arg arg);
    [ConsoleCommand("rec_wipe")]
    private void ccmdPCWipe(ConsoleSystem.Arg arg);
    private void SaveData();
    private void LoadData();
    private class RecyclersData
    {
        public Dictionary<ulong, PlayerData> Players;
    }

    private class PlayerData
    {
        public string DisplayName;
        public int Balance;
    }

    private class ExtendedRecyclerComponent : MonoBehaviour
    {
        private Recycler recycler;
        private void Awake();
        private void CheckGround();
        private void GroundMissing();
        public void TryPickup(BasePlayer player);
        public void DoDestroy();
    }

}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. Pickup settings:")]
    public OPickup pickup;
    [JsonProperty(PropertyName = "2. Craft settings:")]
    public OCraft craft;
    [JsonProperty(PropertyName = "3. Destroy settings:")]
    public ODestroy destroy;
    [JsonProperty(PropertyName = "4. Damage settings:")]
    public ODamage damage;
    public class OPickup
    {
        [JsonProperty(PropertyName = "1. Enabled for personal recyclers (placed by player)")]
        public bool personal;
        [JsonProperty(PropertyName = "2. Check ability to build for pickup")]
        public bool privilege;
        [JsonProperty(PropertyName = "3. Only owner can pickup")]
        public bool onlyOwner;
    }

    public class OCraft
    {
        [JsonProperty(PropertyName = "1. Enabled")]
        public bool enabled;
        [JsonProperty(PropertyName = "2. Cost (shortname - amount):")]
        public Dictionary<string, int> cost;
        [JsonProperty(PropertyName = "3. Default Balance per Player:")]
        public int defaultBalance;
        [JsonProperty(PropertyName = "3. VIP Balance per Player:")]
        public int VIPBalance;
    }

    public class ODestroy
    {
        [JsonProperty(PropertyName = "1. Check ground for recyclers (destroy on missing)")]
        public bool checkGround;
        [JsonProperty(PropertyName = "2. Give item on destroy recycler")]
        public bool destroyItem;
        [JsonProperty(PropertyName = "3. Effects on destroy recycler")]
        public List<string> effects;
    }

    public class ODamage
    {
        [JsonProperty(PropertyName = "1. Allow damage to recycler")]
        public bool allowDamage;
        [JsonProperty(PropertyName = "2. Reduce damage to recycler by %")]
        public float reduceDamagePercent;
    }

}

public class OPickup
{
    [JsonProperty(PropertyName = "1. Enabled for personal recyclers (placed by player)")]
    public bool personal;
    [JsonProperty(PropertyName = "2. Check ability to build for pickup")]
    public bool privilege;
    [JsonProperty(PropertyName = "3. Only owner can pickup")]
    public bool onlyOwner;
}

public class OCraft
{
    [JsonProperty(PropertyName = "1. Enabled")]
    public bool enabled;
    [JsonProperty(PropertyName = "2. Cost (shortname - amount):")]
    public Dictionary<string, int> cost;
    [JsonProperty(PropertyName = "3. Default Balance per Player:")]
    public int defaultBalance;
    [JsonProperty(PropertyName = "3. VIP Balance per Player:")]
    public int VIPBalance;
}

public class ODestroy
{
    [JsonProperty(PropertyName = "1. Check ground for recyclers (destroy on missing)")]
    public bool checkGround;
    [JsonProperty(PropertyName = "2. Give item on destroy recycler")]
    public bool destroyItem;
    [JsonProperty(PropertyName = "3. Effects on destroy recycler")]
    public List<string> effects;
}

public class ODamage
{
    [JsonProperty(PropertyName = "1. Allow damage to recycler")]
    public bool allowDamage;
    [JsonProperty(PropertyName = "2. Reduce damage to recycler by %")]
    public float reduceDamagePercent;
}

private class RecyclersData
{
    public Dictionary<ulong, PlayerData> Players;
}

private class PlayerData
{
    public string DisplayName;
    public int Balance;
}

private class ExtendedRecyclerComponent : MonoBehaviour
{
    private Recycler recycler;
    private void Awake();
    private void CheckGround();
    private void GroundMissing();
    public void TryPickup(BasePlayer player);
    public void DoDestroy();
}


```

---

## ExternalWallDecayProtection by  - Adds protection from decay to walls

```csharp
using System;
using System.Linq;
using Rust;

Oxide.Plugins
[Info("External Wall Decay Protection", "Orange", "1.0.0")]
[Description("Adding protection from decay to walls")]
public class ExternalWallDecayProtection : RustPlugin
{
    private float protection1;
    private float protection2;
    protected override void LoadDefaultConfig();
     T GetConfig(string name, T value);
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void CheckEntities();
    private void AddProtection(BaseNetworkable entity);
}


```

---

## ExternalWallProtect by redBDGR - Prevents ladders from being able to be placed on external walls

```csharp
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Physics = UnityEngine.Physics;

Oxide.Plugins
[Info("External Wall Protect", "redBDGR", "1.0.3")]
[Description("Prevents ladders from being able to be placed on external walls")]
 class ExternalWallProtect : RustPlugin
{
    private const string permissionName;
    private static LayerMask collLayers;
    private void Init();
    private object CanBuild(Planner plan, Construction prefab);
    private string msg(string key, string id);
}


```

---

## ExternalWallStacker by VisEntities - Build skyscraper-like walls

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("External Wall Stacker", "Dana", "1.0.3")]
[Description("Build skyscraper-like walls.")]
public class ExternalWallStacker : RustPlugin
{
    private bool HasRunPermission(BasePlayer player, string cmdPermission);
    private HashSet<ExternalWallLink> CreateStackWall(int amount, BaseEntity entity, BasePlayer player);
     T GetConfig(string name, T value);
     class ExternalWallLink
    {
         GameObject gameObject;
        public bool isRemoving;
        public ExternalWallLink(GameObject go);
        public BaseEntity entity();
    }

     class ExternalWallController : MonoBehaviour
    {
         HashSet<ExternalWallLink> links;
         void Awake();
        public HashSet<ExternalWallLink> entityLinks();
        public void addLink(ExternalWallLink linkEntity);
    }

    [PluginReference]
     Plugin RemoverTool;
     int configStackHeight;
     bool configUsePermission;
     bool configRequireMaterials;
     string pluginPermission;
     string pluginName;
     string pluginColor;
     List<BaseEntity> removingEntity;
     HashSet<ulong> playerToggleCommand;
    protected override void LoadDefaultConfig();
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnEntityBuilt(Planner planner, GameObject gameObject);
     void OnRemovedEntity(BaseEntity entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("wstack"), Permission("externalwallstack.wstack")]
    private void cmdWStack(BasePlayer player, string command, string[] args);
}

 class ExternalWallLink
{
     GameObject gameObject;
    public bool isRemoving;
    public ExternalWallLink(GameObject go);
    public BaseEntity entity();
}

 class ExternalWallController : MonoBehaviour
{
     HashSet<ExternalWallLink> links;
     void Awake();
    public HashSet<ExternalWallLink> entityLinks();
    public void addLink(ExternalWallLink linkEntity);
}


```

---

## ExtraGatherBonuses by  - Get extra items on gathering resources

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("Extra Gather Bonuses", "Orange", "1.0.5")]
[Description("Get extra items on gathering resources")]
public class ExtraGatherBonuses : RustPlugin
{
    private Dictionary<string, GatherInfo> bonuses;
    private void Init();
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private void OnCropGather(GrowableEntity plant, Item item, BasePlayer player);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnGather(BasePlayer player, string name);
    private void CheckBonus(BasePlayer player, GatherInfo info);
    private Item CreateItem(BaseItem def);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "A. Bonus list")]
        public List<GatherInfo> list;
    }

    private class GatherInfo
    {
        [JsonProperty(PropertyName = "Item gathered to get bonus")]
        public string resource;
        [JsonProperty(PropertyName = "Permission")]
        public string perm;
        [JsonProperty(PropertyName = "Maximal items that player can get by once")]
        public int maxItems;
        [JsonProperty(PropertyName = "Bonus list")]
        public List<BaseItem> extra;
    }

    private class BaseItem
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string shortname;
        [JsonProperty(PropertyName = "Amount min")]
        public int amountMin;
        [JsonProperty(PropertyName = "Amount max")]
        public int amountMax;
        [JsonProperty(PropertyName = "Skin")]
        public ulong skinId;
        [JsonProperty(PropertyName = "Display name")]
        public string displayName;
        [JsonProperty(PropertyName = "Chance")]
        public int chance;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void Message(BasePlayer player, string messageKey, object[] args);
    private string GetMessage(string messageKey, string playerID, object[] args);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "A. Bonus list")]
    public List<GatherInfo> list;
}

private class GatherInfo
{
    [JsonProperty(PropertyName = "Item gathered to get bonus")]
    public string resource;
    [JsonProperty(PropertyName = "Permission")]
    public string perm;
    [JsonProperty(PropertyName = "Maximal items that player can get by once")]
    public int maxItems;
    [JsonProperty(PropertyName = "Bonus list")]
    public List<BaseItem> extra;
}

private class BaseItem
{
    [JsonProperty(PropertyName = "Shortname")]
    public string shortname;
    [JsonProperty(PropertyName = "Amount min")]
    public int amountMin;
    [JsonProperty(PropertyName = "Amount max")]
    public int amountMax;
    [JsonProperty(PropertyName = "Skin")]
    public ulong skinId;
    [JsonProperty(PropertyName = "Display name")]
    public string displayName;
    [JsonProperty(PropertyName = "Chance")]
    public int chance;
}


```

---

## ExtraLoot by shady14u - Add extra items (including custom) to any loot container in the game

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Extra Loot", "Shady14u", "1.0.8")]
[Description("Add extra items (including custom) to any loot container in the game")]
public partial class ExtraLoot : RustPlugin
{
    private void SpawnLoot(LootContainer container);
    private static Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "ShortName -> Items")]
        public Dictionary<string, List<BaseItem>> Containers;
        [JsonProperty(PropertyName = "Max Extra Items Per Container")]
        public int MaxItemsPerContainer;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnLootSpawn(LootContainer container);
    public class BaseItem
    {
        [JsonProperty(PropertyName = "1. ShortName")]
        public string ShortName;
        [JsonProperty(PropertyName = "2. Chance")]
        public float Chance;
        [JsonProperty(PropertyName = "3. Minimal amount")]
        public int AmountMin;
        [JsonProperty(PropertyName = "4. Maximal Amount")]
        public int AmountMax;
        [JsonProperty(PropertyName = "5. Skin Id")]
        public ulong SkinId;
        [JsonProperty(PropertyName = "6. Display Name")]
        public string DisplayName;
        [JsonProperty(PropertyName = "7. Blueprint")]
        public bool IsBlueprint;
        [JsonProperty(PropertyName = "8. Min Spawn Height")]
        public float MinHeight;
        [JsonProperty(PropertyName = "9. Max Spawn Height")]
        public float MaxHeight;
    }

}

public class Configuration
{
    [JsonProperty(PropertyName = "ShortName -> Items")]
    public Dictionary<string, List<BaseItem>> Containers;
    [JsonProperty(PropertyName = "Max Extra Items Per Container")]
    public int MaxItemsPerContainer;
    public static Configuration DefaultConfig();
}

public class BaseItem
{
    [JsonProperty(PropertyName = "1. ShortName")]
    public string ShortName;
    [JsonProperty(PropertyName = "2. Chance")]
    public float Chance;
    [JsonProperty(PropertyName = "3. Minimal amount")]
    public int AmountMin;
    [JsonProperty(PropertyName = "4. Maximal Amount")]
    public int AmountMax;
    [JsonProperty(PropertyName = "5. Skin Id")]
    public ulong SkinId;
    [JsonProperty(PropertyName = "6. Display Name")]
    public string DisplayName;
    [JsonProperty(PropertyName = "7. Blueprint")]
    public bool IsBlueprint;
    [JsonProperty(PropertyName = "8. Min Spawn Height")]
    public float MinHeight;
    [JsonProperty(PropertyName = "9. Max Spawn Height")]
    public float MaxHeight;
}


```

---

## ExtraSeating by Pho3niX90 - Allows extra seats on mini-copters and horses

```csharp
using UnityEngine;

Oxide.Plugins
[Info("Extra Seating", "Pho3niX90", "1.1.2")]
[Description("Allows extra seats on minicopters, attackcopters and horses")]
 class ExtraSeating : RustPlugin
{
    public PluginConfig config;
    static ExtraSeating _instance;
     bool debug;
     int seats;
    protected override void LoadDefaultConfig();
    public PluginConfig GetDefaultConfig();
    public class PluginConfig
    {
        public bool EnableMiniSideSeats;
        public bool EnableMiniBackSeat;
        public bool EnableExtraHorseSeat;
        public bool EnableAttackSideSeats;
    }

    private void Init();
     void LogDebug(string str);
     void OnEntitySpawned(BaseNetworkable entity);
     void AddSeat(BaseVehicle ent, Vector3 locPos, Quaternion q);
     BaseVehicle.MountPointInfo CreateMount(Vector3 vec, BaseVehicle.MountPointInfo exampleSeat, Vector3 rotation);
     class HorsePassenger : BaseRidableAnimal
    {
        override public void PlayerServerInput(InputState inputState, BasePlayer player);
    }

     class Seating : MonoBehaviour
    {
        public BaseVehicle entity;
         void Awake();
    }

}

public class PluginConfig
{
    public bool EnableMiniSideSeats;
    public bool EnableMiniBackSeat;
    public bool EnableExtraHorseSeat;
    public bool EnableAttackSideSeats;
}

 class HorsePassenger : BaseRidableAnimal
{
    override public void PlayerServerInput(InputState inputState, BasePlayer player);
}

 class Seating : MonoBehaviour
{
    public BaseVehicle entity;
     void Awake();
}


```

---

## F1ServerConsoleLog by NooBlet - Logs server console to F1 console of players with permission

```csharp
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("F1 Server Console Log", "NooBlet", "0.1.7")]
[Description("Logs Server Console to F1 Console")]
public class F1ServerConsoleLog : CovalencePlugin
{
     List<BasePlayer> playerWithPerm;
     string perm;
    private void OnServerInitialized();
    private void Loaded();
    private void Unload();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void ConsoleLog(string condition, string stackTrace, LogType type);
    private string TimeString;
    private List<object> excludestrings;
    protected override void LoadDefaultConfig();
    private void LoadConfiguration();
    private void CheckCfg(string Key, T var);
    private string GetLang(string key, string id);
     void sendtoF1(string log);
     bool exclude(string msg);
    [Command("console")]
    private void consoleCommand(IPlayer iplayer, string command, string[] args);
    private void LoadDefaultMessages();
}


```

---

## Factions by  - A Faction system with Leaders, Taxes, Trades, Ranks, Kill Rewards, and MORE!

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Text;
using System.Linq;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core;

Oxide.Plugins
[Info("Factions", "Absolut", "3.5.5", ResourceId = 1919)]
 class Factions : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Kits;
    [PluginReference]
     Plugin LustyMap;
    [PluginReference]
     Plugin ZoneManager;
    [PluginReference]
     Plugin ZoneDomes;
    [PluginReference]
     Plugin ActiveCSZone;
    private bool UseFactions;
    private bool TaxBoxFullNotification;
    private int BZPrepTime;
     ushort bZID;
     int GlobalTime;
    static FieldInfo buildingPrivlidges;
     FactionSavedPlayerData playerData;
    private DynamicConfigFile PlayerData;
     PlayerSavedInventories invData;
    private DynamicConfigFile InvData;
     FactionStatistics factionData;
    private DynamicConfigFile FactionData;
    private List<ushort> activeBoxes;
    private List<ulong> UnsureWaiting;
    private List<ulong> MenuState;
    private List<ulong> ButtonState;
    private List<ulong> ImmunityList;
    private List<ulong> OpenMemberStatus;
    private Dictionary<ulong, FactionDesigner> ActiveCreations;
    private Dictionary<ulong, FactionDesigner> ActiveEditors;
    private Dictionary<ulong, SpawnDesigner> SpawnCreation;
    private Dictionary<ushort, target> FactionInvites;
    private Dictionary<ushort, target> FactionKicks;
    private Dictionary<ushort, target> LeaderPromotes;
    private Dictionary<int, TradeProcessing> TradeAssignments;
    private Dictionary<int, TradeProcessing> TradeRemoval;
    private Dictionary<ulong, List<string>> OpenUI;
    private Dictionary<int, Monuments> MonumentLocations;
    private List<BaseEntity> bzBuildings;
    private Dictionary<ulong, PlayerCond> Condition;
    private Dictionary<ushort, string> BattleZones;
    private Dictionary<ulong, BattleZonePlayer> BZPlayers;
    private Dictionary<ushort, float> BZTimes;
    private Dictionary<ushort, Timer> BZTimers;
    private Dictionary<ulong, Timer> BZKillTimers;
    private List<ulong> SpawnTimers;
    private readonly string turretPrefab;
    private uint turretPrefabId;
     Dictionary<ulong, List<AutoTurret>> bzTurrets;
     FieldInfo bulletDamageField;
     FieldInfo healthField;
     FieldInfo maxHealthField;
    static string UIMain;
    static string FactionsUIPanel;
    static string UIEntry;
    static string BattleZoneTimer;
    static string SpawnTimerUI;
     void Loaded();
     void Unload();
     void OnServerInitialized();
     void OnEntityBuilt(Planner planner, GameObject gameobject);
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
    private void OnPlayerDisconnected(BasePlayer player);
     void DestroyPlayer(BasePlayer player);
    private object OnPlayerChat(ConsoleSystem.Arg arg);
     void OnLootEntity(BasePlayer player, object lootable);
     void OnPlantGather(PlantEntity Plant, Item item, BasePlayer player);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnDispenserGather(ResourceDispenser Dispenser, BaseEntity entity, Item item);
     object OnItemCraft(ItemCraftTask task, BasePlayer crafter);
     object OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnEntitySpawned(BaseNetworkable entity);
     object CanUseDoor(BasePlayer player, BaseLock door);
     void AssignTurretAuth(ushort faction, AutoTurret turret);
     void AuthorizePlayerOnTurrets(BasePlayer player);
     void ConfigureTurret(AutoTurret turret);
    private bool SameFactionCheck(BasePlayer self, ulong target);
    private bool FactionMemberCheck(BasePlayer player);
    private ushort GetPlayerFaction(BasePlayer player);
    private FactionType GetFactionType(BasePlayer player);
    private Trade GetPlayerTrade(BasePlayer player);
    private Rank GetPlayerRank(BasePlayer player);
    private int GetPlayerKills(BasePlayer player);
    private int GetFactionKills(ushort faction);
    private bool CheckForActiveEnemies(ushort faction);
    private int GetPlayerGathered(BasePlayer player);
    private int GetPlayerCrafted(BasePlayer player);
    private int GetPlayerLevel(BasePlayer player);
    public void Broadcast(string message, string userid);
    private void BroadcastFaction(BasePlayer source, string message, ushort faction);
    private void SendMSG(BasePlayer player, string msg, string keyword);
    private void SendPuts(string msg, string keyword);
    private void SendDeathNote(BasePlayer player, BasePlayer victim, ushort faction);
    private string CountPlayers(ushort faction);
    private object RaycastAll(Ray ray);
    static bool AuthorizedTC(BasePlayer player);
    static double GrabCurrentTime();
    private void SavePlayerFactionTime();
    private void SaveTimeData(BasePlayer player);
    private void InitPlayerTime(BasePlayer player);
    private StorageContainer GetTaxContainer(ushort faction);
     string MergeParams(string[] Params, int Start);
    static void MovePlayerPosition(BasePlayer player, Vector3 destination);
    private void SetBoxFullNotification();
     object GetTax(ushort faction);
     object GetLeaderName(ushort faction);
     object GetLeader(ushort faction);
    private bool isleader(BasePlayer player);
    private bool hasTaxBox(ushort faction);
     void Bindings(BasePlayer player);
    [ChatCommand("buildings")]
    private void cmdtest(BasePlayer player);
    private void CountTotalPlayerBuildings();
    private void CountFactionPlayerBuildings();
    private void CountFactionBuildings();
     void SpawnTimer(BasePlayer player, int time);
    private void InitializeBZPlayer(BasePlayer player, ushort BZFaction, string ZoneID);
    private IEnumerable<PlayerInv> GetItems(ItemContainer container, string containerName);
    public void SaveHealth(BasePlayer player);
    public void SaveInventory(BasePlayer player);
    public void RestoreBZPlayer(BasePlayer player);
    public void RestoreInventory(BasePlayer player);
    private void GiveBZItems(BasePlayer player, string role);
    private Item BuildItem(string shortname, int amount, ulong skin);
    public void GiveItem(BasePlayer player, Item item, string container);
     void RestorePlayerHealth(BasePlayer player);
     void ChangeGlobalTime();
     void RefreshOpenMemberStatus();
    private object GetZoneLocation(string zoneid);
    private object GetZoneRadius(string zoneid);
     void DestroyZoneEntities();
     void OnEnterZone(string zoneID, BasePlayer player);
     void OnExitZone(string zoneID, BasePlayer player);
    private void createZone(BasePlayer player, ushort ID, string type);
    private bool BuildingCheck(BasePlayer player, Vector3 pos, float radius);
    private void BZPrep(ushort ID, string zone);
    private void eraseZone(BasePlayer player, ushort ID, string type);
    private void ShadeZone(BasePlayer player, string zoneID);
    private void UnShadeZone(BasePlayer player, string zoneID);
    private object VerifyZoneID(string zoneid);
    private void AddMapMarker(float x, float z, string name, string icon);
    private void RemoveMapMarker(string name);
    private void InitializeBZTime(ushort ID, int time);
    private void BZIntermidiateCheck(ushort ID);
    private void BZAttackerCheck();
    private bool InBZ(BasePlayer player);
    private void BZTimerCountdown(ushort ID);
    private void RefreshBZTimer(BasePlayer player, ushort ID);
    private Vector3 CalculateOutsidePos(BasePlayer player, string zoneID);
     void DestroyBZTimers(ushort ID);
     void EndBZ(ushort ID, string reason);
     void ClearImmunity();
     void AnnounceBZ(ushort ID);
     void AnnounceDuringBZ();
    private void FindMonuments();
    private void QuitFactionCreation(BasePlayer player, bool isCreatingFaction);
    private void RemoveFaction(BasePlayer player, ushort ID);
    private void ReassignPlayers(ushort ID);
    private void SaveFaction(BasePlayer player, bool isCreatingFaction);
    private void ExitFactionEditor(BasePlayer player, bool isEditingFaction);
    private void DestroyUI(BasePlayer player);
    private void DestroyEntries(BasePlayer player);
    private int GetRandomNumber();
    private Faction GetFactionInfo(ushort ID);
    public static class StringTool
    {
        public static string Truncate(string source, int length);
    }

    [ConsoleCommand("CUI_SelectColor")]
    private void cmdSelectColor(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_SelectFactionType")]
    private void cmdSelectFactionType(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_SelectFactionKit")]
    private void cmdSelectFactionKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_NewFaction")]
    private void cmdNewFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_TryDeleteFaction")]
    private void cmdTryDeleteFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUI_LeaderEditing")]
    private void cmdCUI_LeaderEditing(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUIUnassignLeader")]
    private void cmdCUI_UnassignLeader(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUIAssignLeader")]
    private void cmdCUIAssignLeader(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_DeleteFaction")]
    private void cmdDeleteFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_EditFaction")]
    private void cmdEditFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_EditFactionVar")]
    private void cmdEditFactionVar(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_SaveFaction")]
    private void cmdSaveFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ExitFaction")]
    private void cmdExitFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ExitFactionEditor")]
    private void cmdExitFactionEditor(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_TryDeleteSpawn")]
    private void cmdTryDeleteSpawn(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_DeleteSpawn")]
    private void cmdDeleteSpawn(ConsoleSystem.Arg arg);
    private void CreationHelp(BasePlayer player, int page, ushort factionID);
    private void FactionInfo(BasePlayer player, ushort faction, int page);
    private void CreateFactionEditButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    private void CreateFactionDetails(CuiElementContainer container, string panelName, string text, int number);
    private void DeletionMenu(BasePlayer player, string page, string command);
    private void ConfirmFactionDeletion(BasePlayer player, ushort ID);
    private void ConfirmSpawnDeletion(BasePlayer player, ushort ID, string name, string spawntype);
    private void CreateDelEditButton(CuiElementContainer container, float xPos, string panelName, string buttonname, int number, string command);
    private void FactionEditorMenu(BasePlayer player);
    private void NumberPad(BasePlayer player, string msg, string cmd);
    private void CreateNumberPadButton(CuiElementContainer container, string panelName, float xPos, int number, string command);
    private float[] CalcNumButtonPos(int number);
    private string PanelLevelAdvanced;
    private string PanelRankAdvanced;
    private string PanelKillTicker;
    private string PanelFactionMenuBar;
    private string PanelSpawnButtons;
    private string PanelBZButton;
    private string PanelMemberStatus;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    }

    private void CreateInstructionEntry(CuiElementContainer container, string panelName, string color, string command, int num);
    private float[] InstructionPos(int number);
    private void CreateStatusEntry(CuiElementContainer container, string panelName, string name, float health, int num);
    private void CreateTickerEntry(CuiElementContainer container, string panelName, ushort faction, int num);
    private float[] TickerEntryPos(int number);
    private void CreateOptionButton(CuiElementContainer container, string panelName, string color, string name, string cmd, int num);
    private void CreateKitButton(CuiElementContainer container, string panelName, string color, string name, string cmd, int num);
    private float[] CalcKitButtonPos(int number);
    private void CreateCMDButton(CuiElementContainer container, string panelName, ushort faction, string text, string cmd, int num);
    private float[] CalcButtonPos(int number);
    private void CreatePlayerEntry(CuiElementContainer container, string panelName, string color, string info, int num);
    private float[] PlayerEntryPos(int number);
    private void CreateSBEntry(CuiElementContainer container, string panelName, string color, string name, int kills, string faction, int num);
    private float[] SBNamePos(int number);
    private void CreateFactionSelectionButton(CuiElementContainer container, string panelName, ushort faction, int num, int page);
    private void CreateFactionListButton(CuiElementContainer container, string panelName, ushort faction, int num);
    private void CreateSpawnIDButton(CuiElementContainer container, string panelName, string type, string spawnName, ushort faction, ushort SpawnID, int num);
    private void CreateSpawnButton(CuiElementContainer container, string panelName, string type, string spawnName, ushort faction, ushort SpawnID, int num);
    private float[] SpawnButtonPos(int number);
    private Dictionary<string, string> UIColors;
     void SetFaction(BasePlayer player, int page);
     void FactionMenuBar(BasePlayer player);
     void Instructions(BasePlayer player);
     void Leader(BasePlayer player);
     void BZButton(BasePlayer player, ushort ID);
    private void BZConfirmation(BasePlayer player, ushort BZID);
    private void KickPlayersMenu(BasePlayer player, ushort factionID, int page);
     void FactionInviteScreen(BasePlayer player, ushort factionID, int page);
     void FactionListsUI(BasePlayer player);
     void Player(BasePlayer player);
     void Admin(BasePlayer player);
     void SpawnManager(BasePlayer player, string user);
     void ZoneManagement(BasePlayer player, string user);
     void FactionManager(BasePlayer player);
     void PlayerManager(BasePlayer player);
     void FactionLeaders(BasePlayer player);
     void LeaderEditing(BasePlayer player, ushort faction, int page);
     void FactionEditor(BasePlayer player);
     void PlayerTradeMenu(BasePlayer player);
     void TradesManager(BasePlayer player);
     void TradeOverview(BasePlayer player, Trade trade);
    private void TradeAssignmentMenu(BasePlayer player, Trade trade);
     void Options(BasePlayer player);
    [ConsoleCommand("UI_CUIChangeOption")]
    private void cmdCUIChangeOption(ConsoleSystem.Arg arg);
     void ChangeOption(BasePlayer player, string cmd, string verbiage, string optionName, string status);
     void ScoreBoardUI(BasePlayer player, int count, int page);
     void ShowFaction(BasePlayer player, ushort faction, int count, int page);
     void FactionMemberStatus(BasePlayer player);
     void KillTicker(BasePlayer player);
     void RankAdvancement(BasePlayer player);
     void LevelAdvancement(BasePlayer player);
     void SpawnButtons(BasePlayer player, ushort faction);
    [ConsoleCommand("UI_Use_FactionChatControl")]
    private void cmdChatConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FFDisabled")]
    private void cmdFFConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BuildingProtectionEnabled")]
    private void cmdBuildingConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_Trades")]
    private void cmdTradesConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_Taxes")]
    private void cmdTaxesConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_Ranks")]
    private void cmdRanks(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AllowTradesByPlayer")]
    private void cmdAllowTradesByPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_Groups")]
    private void cmdGroups(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionsInfo")]
    private void cmdUse_FactionsInfo(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_Kits")]
    private void cmdUse_Kits(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AllowPlayerToLeaveFactions")]
    private void cmdAllowPlayerToLeaveFactions(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionBalancing")]
    private void cmdUse_FactionBalancing(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionNamesonChat")]
    private void cmdUse_FactionNamesonChat(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_ChatTitles")]
    private void cmdUse_ChatTitles(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionLeaderByRank")]
    private void cmdUse_FactionLeaderByRank(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionLeaderByTime")]
    private void cmdUse_FactionLeaderByTime(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionLeaderByAdmin")]
    private void cmdUse_FactionLeaderByAdmin(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_RevoltChallenge")]
    private void cmdUse_RevoltChallengeConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionZones")]
    private void cmdUI_Use_FactionZones(ConsoleSystem.Arg arg);
    [ConsoleCommand("Use_FactionSafeZones")]
    private void cmdUse_FactionSafeZones(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_BattleZones")]
    private void cmdUI_Use_BattleZones(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_AutoAuthorization")]
    private void cmdUI_Use_AutoAuthorization(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_RallySpawns")]
    private void cmdUse_RallySpawnsConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionSpawns")]
    private void cmdUse_FactionSpawnsConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_PersistentSpawns")]
    private void cmdUI_Use_PersistentSpawns(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionsByInvite")]
    private void cmdUI_Use_FactionsByInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_UseEconomics")]
    private void cmdUseEconomicsConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_UseRewards")]
    private void cmdUseRewardsConfig(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BroadcastDeath")]
    private void cmdBroadcastDeath(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionKillIncentives")]
    private void cmdUse_FactionKillIncentives(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUIFactionLists")]
    private void cmdCUIFactionLists(ConsoleSystem.Arg arg);
    private void CUIFactionLists(BasePlayer player);
    [ConsoleCommand("UI_CUIFactionList")]
    private void cmdCUIFactionList(ConsoleSystem.Arg arg);
    private void CUIFactionList(BasePlayer player, ushort faction, int count, int page);
    [ConsoleCommand("UI_CUIScoreBoard")]
    private void cmdCUIScoreBoard(ConsoleSystem.Arg arg);
    private void CUIScoreBoard(BasePlayer player, int count, int page);
    [ConsoleCommand("UI_CUIInstructions")]
    private void cmdCUIInstructions(ConsoleSystem.Arg arg);
    private void CUIInstructions(BasePlayer player);
    [ConsoleCommand("UI_CUILeader")]
    private void cmdCUILeader(ConsoleSystem.Arg arg);
    private void CUILeader(BasePlayer player);
    [ConsoleCommand("UI_CUIAdmin")]
    private void cmdCUIAdmin(ConsoleSystem.Arg arg);
    private void CUIAdmin(BasePlayer player);
    [ConsoleCommand("UI_CUITrades")]
    private void cmdCUITrades(ConsoleSystem.Arg arg);
    private void CUITradesdmin(BasePlayer player);
    [ConsoleCommand("UI_CUITradeAssignmentMenu")]
    private void cmdCUITradeAssignmentMenu(ConsoleSystem.Arg arg);
    private void CUITradeAssignmentMenu(BasePlayer player, Trade trade);
    [ConsoleCommand("UI_CreateZone")]
    private void cmdUI_CreateZone(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_EraseZone")]
    private void cmdUI_EraseZone(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CancelBZ")]
    private void cmdUI_CancelBZ(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUISpawnManager")]
    private void cmdCUISpawnManager(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUIZoneManager")]
    private void cmdCUIZoneManager(ConsoleSystem.Arg arg);
    private void CUISpawnManager(BasePlayer player, string user);
    private void CUIZoneManager(BasePlayer player, string user);
    [ConsoleCommand("UI_CUIFactionManager")]
    private void cmdCUIFactionManager(ConsoleSystem.Arg arg);
    private void CUIFactionManager(BasePlayer player);
    [ConsoleCommand("UI_CUI_FactionLeaders")]
    private void cmdCUI_FactionLeaders(ConsoleSystem.Arg arg);
    private void CUI_FactionLeaders(BasePlayer player);
    [ConsoleCommand("UI_CUI_FactionEditor")]
    private void cmdCUI_FactionEditor(ConsoleSystem.Arg arg);
    private void CUI_FactionEditor(BasePlayer player);
    [ConsoleCommand("UI_CUIPlayer")]
    private void cmdCUIPlayer(ConsoleSystem.Arg arg);
    private void CUIPlayer(BasePlayer player);
    [ConsoleCommand("UI_FactionSelection")]
    private void cmdUI_FactionSelection(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUIFactionInvite")]
    private void cmdCUIFactionInvite(ConsoleSystem.Arg arg);
    private void CUIFactionInvite(BasePlayer player, ushort faction, int page);
    [ConsoleCommand("UI_CUIKickPlayerMenu")]
    private void cmdUI_CUIKickPlayerMenu(ConsoleSystem.Arg arg);
    private void CUIKickPlayerMenu(BasePlayer player, ushort faction, int page);
    [ConsoleCommand("UI_CUIOptions")]
    private void cmdCUIOptions(ConsoleSystem.Arg arg);
    private void CUIOptions(BasePlayer player);
    [ConsoleCommand("UI_PlayerTradeUnassignment")]
    private void cmdPlayerTradeUnassignment(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUIUnassignTradeSkill")]
    private void cmdCUIUnassignTradeSkill(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUIAssignTradeSkill")]
    private void cmdCUIAssignTradeSkill(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_RallyMovePlayerPosition")]
    private void cmdCUIRallyMovePlayer(ConsoleSystem.Arg arg);
    private void CUIRallyMovePlayer(BasePlayer player, ushort SpawnID);
    [ConsoleCommand("UI_FactionMovePlayerPosition")]
    private void cmdCUISpawnMovePlayer(ConsoleSystem.Arg arg);
    private void CUISpawnMovePlayer(BasePlayer player, ushort SpawnID);
    [ConsoleCommand("UI_CUIRemoveRallySpawn")]
    private void cmdCUIRemoveRallySpawn(ConsoleSystem.Arg arg);
    private void CUIRemoveRallySpawn(BasePlayer player, ushort SpawnID);
    [ConsoleCommand("UI_CUISetSpawnPoint")]
    private void cmdCUISetSpawnPoint(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_TryInvitePlayerToFaction")]
    private void cmdUI_TryInvitePlayerToFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_PlayerTradeMenu")]
    private void cmdUI_PlayerTradeMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_PlayerTradeAssignment")]
    private void cmdPlayerTradeAssignment(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_TradeAssignment")]
    private void cmdUI_TradeAssignment(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUILeaveFaction")]
    private void cmdCUILeaveFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ConfirmKickPlayer")]
    private void cmdUI_ConfirmKickPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BzButton")]
    private void cmdUI_BzButton(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BzConfirmation")]
    private void cmdUI_BzConfirmation(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BZYes")]
    private void cmdUI_BZYes(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_AcceptInvite")]
    private void cmdCUI_AcceptInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_DeclineInvite")]
    private void cmdCUI_DeclineInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_SaveSpawn")]
    private void cmdSaveSpawn(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ExitSpawn")]
    private void cmdExitSpawn(ConsoleSystem.Arg arg);
    private void SaveSpawn(BasePlayer player, bool isSpawn);
    private void ExitSpawnCreation(BasePlayer player, bool isSpawn);
    [ConsoleCommand("UI_CUIRemoveFactionSpawn")]
    private void cmdCUIRemoveFactionSpawn(ConsoleSystem.Arg arg);
    private void CUIRemoveFactionSpawn(BasePlayer player, ushort SpawnID);
    [ConsoleCommand("UI_CUISetTaxBox")]
    private void cmdCUISetTaxBox(ConsoleSystem.Arg arg);
    private void CUISetTaxBox(BasePlayer player);
    [ConsoleCommand("UI_CUIRemoveTaxBox")]
    private void cmdCUIRemoveTaxBox(ConsoleSystem.Arg arg);
    private void CUIRemoveTaxBox(BasePlayer player);
    [ConsoleCommand("UI_CUIChallengeLeader")]
    private void cmdCUIChallengeLeader(ConsoleSystem.Arg arg);
    private void CUIChallengeLeader(BasePlayer player);
    [ConsoleCommand("UI_CUIResetTicker")]
    private void cmdCUIResetTicker(ConsoleSystem.Arg arg);
    private void ResetTicker();
    private void CheckForFactionSelect(BasePlayer player);
    [ConsoleCommand("UI_DestroyBZPanel")]
    private void cmdDestroyBZPanel(ConsoleSystem.Arg arg);
    private void DestroyBZ(BasePlayer player);
    [ConsoleCommand("UI_DestroyFS")]
    private void cmdDestroyFS(ConsoleSystem.Arg arg);
    private void DestroyFS(BasePlayer player);
    [ConsoleCommand("UI_DestroyTicker")]
    private void cmdDestroyTicker(ConsoleSystem.Arg arg);
    private void DestroyTicker(BasePlayer player);
    private void RefreshTicker(BasePlayer player);
    [ConsoleCommand("UI_DestroySP")]
    private void cmdDestroySP(ConsoleSystem.Arg arg);
    private void DestroyFactionsUIPanel(BasePlayer player);
    [ConsoleCommand("UI_DestroyFactionMenu")]
    private void cmdDestroyFM(ConsoleSystem.Arg arg);
    private void DestroyFactionMenu(BasePlayer player);
    [ConsoleCommand("UI_OpenFactions")]
    private void cmdOpenFactions(ConsoleSystem.Arg arg);
    private void OpenFactions(BasePlayer player);
    [ConsoleCommand("UI_CUIRequestFactionTax")]
    private void cmdCUIRequestFactionTax(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUISetFactionTax")]
    private void cmdCUISetFactionTax(ConsoleSystem.Arg arg);
    [ConsoleCommand("DestroyAll")]
    private void cmdDestroyAll(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_OpenMemberStatus")]
    private void cmdOpenMemberStatus(ConsoleSystem.Arg arg);
    [ConsoleCommand("DestroyMemberStatus")]
    private void cmdDestroyMemberStatus(ConsoleSystem.Arg arg);
    private void DestroyAll(BasePlayer player);
    [ConsoleCommand("DestroySpawnButtons")]
    private void cmdDestroySpawnButtons(ConsoleSystem.Arg arg);
    private void DestroySpawnButtons(BasePlayer player);
    [ConsoleCommand("faction.unassign")]
    private void cmdFactionUnAssign(ConsoleSystem.Arg arg);
    [ChatCommand("fc")]
    private void cmdfactionchat(BasePlayer player, string command, string[] args);
    [ChatCommand("faction")]
    private void cmdfaction(BasePlayer player, string command, string[] args);
     bool isAuth(BasePlayer player);
     bool isAuthCon(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CUI_FactionInfo")]
    private void cmdCUI_FactionInfo(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_FactionSelection")]
    private void cmdFactionSelection(ConsoleSystem.Arg arg);
    private int GetMax();
    private int GetMin();
    [ConsoleCommand("UI_CUIPlayerManager")]
    private void cmdUI_CUIPlayerManager(ConsoleSystem.Arg arg);
    private void CUIPlayerManager(BasePlayer player);
    private List<BasePlayer> FindPlayer(string arg);
    private void AssignPlayerToFaction(BasePlayer player, ushort faction);
    private void UnassignPlayerFromFaction(ulong playerID);
    private void CheckLeaderTime();
    private void CheckLeaderRank(BasePlayer player);
    private void FactionDamage(BasePlayer player);
    private int Count(ushort faction);
     string GetPlayerFactionEx(ulong playerID);
     bool CheckSameFaction(ulong player1ID, ulong player2ID);
     bool isCSPlayer(BasePlayer player);
    private object GetKits();
    public string[] GetKitNames();
    private void CloseMap(BasePlayer player);
    private void OpenMap(BasePlayer player);
    private void GiveFactionKit(BasePlayer player, ushort faction);
     class FactionSavedPlayerData
    {
        public Dictionary<ulong, FactionPlayerData> playerFactions;
    }

     class FactionPlayerData
    {
        public ushort faction;
        public string Name;
        public Rank rank;
        public int level;
        public Trade trade;
        public long LastConnection;
        public int FactionMemberTime;
        public bool ChallengeStatus;
        public int time;
        public int FactionBuildings;
        public int TotalBuildings;
        public int Kills;
        public int Gathered;
        public int Crafted;
        public List<EquipmentKits> AvailableKits { get; set; }
        public string LastKit;
        public ushort LastFaction;
        public bool SavedInventory;
        public List<AutoTurret> factionTurrets { get; set; }
    }

     class PlayerSavedInventories
    {
        public Dictionary<ulong, List<PlayerInv>> PlayerInventory;
        public Dictionary<ulong, List<PlayerInv>> PreviousInventory;
    }

     class FactionStatistics
    {
        public Dictionary<ushort, Faction> Factions;
        public Dictionary<ushort, ulong> leader;
        public Dictionary<ushort, Coords> Boxes;
        public Dictionary<ushort, Coords> FactionSpawns;
        public Dictionary<ushort, Coords> RallySpawns;
        public Dictionary<ulong, ushort> ActiveChallenges;
        public Dictionary<ushort, int> Buildings;
    }

     class Faction
    {
        public string Name;
        public string FactionKit;
        public string UIColor;
        public string ChatColor;
        public string LeaderTitle;
        public int Kills;
        public int PlayerCount;
        public double tax;
        public string group;
        public string kit;
        public FactionType type;
        public bool FactionZone;
    }

     class target
    {
        public ulong playerID;
        public ulong executerID;
        public ushort factionID;
        public bool confirm;
        public bool assign;
    }

     class EquipmentKits
    {
        public ushort KitID;
        public string KitName;
        public ushort KitLevel;
    }

     class TradeProcessing
    {
        public ushort factionID;
        public ulong playerID;
        public ulong leaderID;
        public Trade trade;
    }

     class Monuments
    {
        public Vector3 position;
        public float radius;
    }

     class Inventory
    {
         List<PlayerInv> InvItems;
    }

     class PlayerInv
    {
        public int itemid;
        public ulong skin;
        public string container;
        public int amount;
        public float condition;
        public int ammo;
        public PlayerInv[] InvContents;
    }

     class PlayerCond
    {
        public float health;
        public float calories;
        public float hydration;
    }

     class BattleZonePlayer
    {
        public bool oob;
        public ushort faction;
        public string bz;
        public bool entered;
        public bool died;
        public bool owner;
    }

     class FactionDesigner
    {
        public ushort ID;
        public Faction Entry;
        public Faction OldEntry;
        public int partNum;
    }

     class SpawnDesigner
    {
        public ushort SpawnID;
        public string type;
        public Coords Entry;
        public Coords OldEntry;
        public int partNum;
    }

     class FactionColors
    {
        public string Color;
        public string ChatColor;
        public string UIColor;
    }

     class Coords
    {
        public ushort FactionID;
        public string Name;
        public float x;
        public float y;
        public float z;
    }

     class Gear
    {
        public string shortname;
        public ulong skin;
        public int amount;
    }

    private List<FactionColors> Colors;
    private Dictionary<ushort, Faction> defaultFactions;
    private Dictionary<string, List<Gear>> BZItems;
    private void SaveLoop();
    private void InfoLoop();
     void SaveData();
     void LoadData();
     void LoadDefaultData();
    private ConfigData configData;
     class ConfigData
    {
        public int Save_Interval { get; set; }
        public bool Use_FactionsInfo { get; set; }
        public float ZoneRadius { get; set; }
        public string MSG_MainColor { get; set; }
        public string MSG_Color { get; set; }
        public bool BroadcastDeath { get; set; }
        public bool Use_FactionNamesonChat { get; set; }
        public bool Use_FactionChatControl { get; set; }
        public bool Use_ChatTitles { get; set; }
        public bool Use_Kits { get; set; }
        public bool Use_Taxes { get; set; }
        public bool Use_Groups { get; set; }
        public bool Use_PersistantSpawns { get; set; }
        public int SpawnCountLimit { get; set; }
        public int SpawnCooldown { get; set; }
        public bool Use_RallySpawns { get; set; }
        public bool Use_FactionSpawns { get; set; }
        public bool Use_BattleZones { get; set; }
        public int BattleZonesCooldown { get; set; }
        public int RequiredBZParticipants { get; set; }
        public int BZPrepTime { get; set; }
        public bool Use_FactionSafeZones { get; set; }
        public bool Use_FactionsByInvite { get; set; }
        public bool AllowPlayerToLeaveFactions { get; set; }
        public bool Use_RevoltChallenge { get; set; }
        public bool Use_FactionBalancing { get; set; }
        public int AllowedFactionDifference { get; set; }
        public bool FFDisabled { get; set; }
        public bool BuildingProtectionEnabled { get; set; }
        public float FF_DamageScale { get; set; }
        public string StarterKit { get; set; }
        public bool Use_Ranks { get; set; }
        public bool Use_Trades { get; set; }
        public bool AllowTradesByPlayer { get; set; }
        public float RankBonus { get; set; }
        public double LevelBonus { get; set; }
        public int TradeLimit { get; set; }
        public int LevelRequirement { get; set; }
        public int RankRequirement { get; set; }
        public int MaxLevel { get; set; }
        public bool Use_AutoAuthorization { get; set; }
        public bool Use_FactionKillIncentives { get; set; }
        public int KillLimit { get; set; }
        public bool Use_FactionLeaderByTime { get; set; }
        public bool Use_FactionLeaderByAdmin { get; set; }
        public bool Use_FactionLeaderByRank { get; set; }
        public bool Use_FactionZones { get; set; }
        public bool Use_EconomicsReward { get; set; }
        public int KillAmountEconomics { get; set; }
        public int FactionKillsRewardEconomics { get; set; }
        public int BattleZoneRewardEconomics { get; set; }
        public bool Use_TokensReward { get; set; }
        public int KillAmountTokens { get; set; }
        public int FactionKillsRewardTokens { get; set; }
        public int BattleZoneRewardTokens { get; set; }
        public bool Use_ServerRewardsReward { get; set; }
        public int KillAmountServerRewards { get; set; }
        public int FactionKillsRewardServerRewards { get; set; }
        public int BattleZoneRewardServerRewards { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

public static class StringTool
{
    public static string Truncate(string source, int length);
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
}

 class FactionSavedPlayerData
{
    public Dictionary<ulong, FactionPlayerData> playerFactions;
}

 class FactionPlayerData
{
    public ushort faction;
    public string Name;
    public Rank rank;
    public int level;
    public Trade trade;
    public long LastConnection;
    public int FactionMemberTime;
    public bool ChallengeStatus;
    public int time;
    public int FactionBuildings;
    public int TotalBuildings;
    public int Kills;
    public int Gathered;
    public int Crafted;
    public List<EquipmentKits> AvailableKits { get; set; }
    public string LastKit;
    public ushort LastFaction;
    public bool SavedInventory;
    public List<AutoTurret> factionTurrets { get; set; }
}

 class PlayerSavedInventories
{
    public Dictionary<ulong, List<PlayerInv>> PlayerInventory;
    public Dictionary<ulong, List<PlayerInv>> PreviousInventory;
}

 class FactionStatistics
{
    public Dictionary<ushort, Faction> Factions;
    public Dictionary<ushort, ulong> leader;
    public Dictionary<ushort, Coords> Boxes;
    public Dictionary<ushort, Coords> FactionSpawns;
    public Dictionary<ushort, Coords> RallySpawns;
    public Dictionary<ulong, ushort> ActiveChallenges;
    public Dictionary<ushort, int> Buildings;
}

 class Faction
{
    public string Name;
    public string FactionKit;
    public string UIColor;
    public string ChatColor;
    public string LeaderTitle;
    public int Kills;
    public int PlayerCount;
    public double tax;
    public string group;
    public string kit;
    public FactionType type;
    public bool FactionZone;
}

 class target
{
    public ulong playerID;
    public ulong executerID;
    public ushort factionID;
    public bool confirm;
    public bool assign;
}

 class EquipmentKits
{
    public ushort KitID;
    public string KitName;
    public ushort KitLevel;
}

 class TradeProcessing
{
    public ushort factionID;
    public ulong playerID;
    public ulong leaderID;
    public Trade trade;
}

 class Monuments
{
    public Vector3 position;
    public float radius;
}

 class Inventory
{
     List<PlayerInv> InvItems;
}

 class PlayerInv
{
    public int itemid;
    public ulong skin;
    public string container;
    public int amount;
    public float condition;
    public int ammo;
    public PlayerInv[] InvContents;
}

 class PlayerCond
{
    public float health;
    public float calories;
    public float hydration;
}

 class BattleZonePlayer
{
    public bool oob;
    public ushort faction;
    public string bz;
    public bool entered;
    public bool died;
    public bool owner;
}

 class FactionDesigner
{
    public ushort ID;
    public Faction Entry;
    public Faction OldEntry;
    public int partNum;
}

 class SpawnDesigner
{
    public ushort SpawnID;
    public string type;
    public Coords Entry;
    public Coords OldEntry;
    public int partNum;
}

 class FactionColors
{
    public string Color;
    public string ChatColor;
    public string UIColor;
}

 class Coords
{
    public ushort FactionID;
    public string Name;
    public float x;
    public float y;
    public float z;
}

 class Gear
{
    public string shortname;
    public ulong skin;
    public int amount;
}

 class ConfigData
{
    public int Save_Interval { get; set; }
    public bool Use_FactionsInfo { get; set; }
    public float ZoneRadius { get; set; }
    public string MSG_MainColor { get; set; }
    public string MSG_Color { get; set; }
    public bool BroadcastDeath { get; set; }
    public bool Use_FactionNamesonChat { get; set; }
    public bool Use_FactionChatControl { get; set; }
    public bool Use_ChatTitles { get; set; }
    public bool Use_Kits { get; set; }
    public bool Use_Taxes { get; set; }
    public bool Use_Groups { get; set; }
    public bool Use_PersistantSpawns { get; set; }
    public int SpawnCountLimit { get; set; }
    public int SpawnCooldown { get; set; }
    public bool Use_RallySpawns { get; set; }
    public bool Use_FactionSpawns { get; set; }
    public bool Use_BattleZones { get; set; }
    public int BattleZonesCooldown { get; set; }
    public int RequiredBZParticipants { get; set; }
    public int BZPrepTime { get; set; }
    public bool Use_FactionSafeZones { get; set; }
    public bool Use_FactionsByInvite { get; set; }
    public bool AllowPlayerToLeaveFactions { get; set; }
    public bool Use_RevoltChallenge { get; set; }
    public bool Use_FactionBalancing { get; set; }
    public int AllowedFactionDifference { get; set; }
    public bool FFDisabled { get; set; }
    public bool BuildingProtectionEnabled { get; set; }
    public float FF_DamageScale { get; set; }
    public string StarterKit { get; set; }
    public bool Use_Ranks { get; set; }
    public bool Use_Trades { get; set; }
    public bool AllowTradesByPlayer { get; set; }
    public float RankBonus { get; set; }
    public double LevelBonus { get; set; }
    public int TradeLimit { get; set; }
    public int LevelRequirement { get; set; }
    public int RankRequirement { get; set; }
    public int MaxLevel { get; set; }
    public bool Use_AutoAuthorization { get; set; }
    public bool Use_FactionKillIncentives { get; set; }
    public int KillLimit { get; set; }
    public bool Use_FactionLeaderByTime { get; set; }
    public bool Use_FactionLeaderByAdmin { get; set; }
    public bool Use_FactionLeaderByRank { get; set; }
    public bool Use_FactionZones { get; set; }
    public bool Use_EconomicsReward { get; set; }
    public int KillAmountEconomics { get; set; }
    public int FactionKillsRewardEconomics { get; set; }
    public int BattleZoneRewardEconomics { get; set; }
    public bool Use_TokensReward { get; set; }
    public int KillAmountTokens { get; set; }
    public int FactionKillsRewardTokens { get; set; }
    public int BattleZoneRewardTokens { get; set; }
    public bool Use_ServerRewardsReward { get; set; }
    public int KillAmountServerRewards { get; set; }
    public int FactionKillsRewardServerRewards { get; set; }
    public int BattleZoneRewardServerRewards { get; set; }
}


```

---

## FactionsCore by  - A revamped faction/team system

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System.Reflection;
using System.Globalization;

Oxide.Plugins
[Info("FactionsCore", "Absolut & misticos", "1.0.5")]
[Description("A revamped faction/team system")]
 class FactionsCore : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
     Plugin ZoneManager;
     Plugin LustyMap;
     Plugin ZoneDomes;
     Plugin Kits;
     Plugin ImageLibrary;
     Plugin Professions;
     Plugin CustomSets;
     Plugin LastFactionStanding;
     Plugin FactionsTax;
     Plugin Conquest;
    static FieldInfo buildingPrivlidges;
     FactionsData fdata;
    private DynamicConfigFile FDATA;
     string TitleColor;
     string MsgColor;
     bool initialized;
    public HashSet<FactionPlayer> FactionPlayers;
    private List<ulong> InFactionChat;
    private List<ulong> MakingFactionAnnouncement;
    private List<ulong> AdminView;
    private List<ulong> JoinCooldown;
    private List<ulong> Passthrough;
    private Dictionary<string, List<ulong>> ActiveUniforms;
    private Dictionary<ulong, ulong> AssignFaction;
    private Dictionary<ulong, Dictionary<Item, Slot>> LockedUniform;
    private List<Monuments> MonumentLocations;
     class Monuments
    {
        public Vector3 position;
        public float radius;
    }

    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, FactionDesigner> FactionDetails;
    private Dictionary<ulong, FactionDesigner> FactionEditor;
     void Loaded();
     void Unload();
    private void DestroyAll();
     void OnServerInitialized();
    [ChatCommand("testplayers")]
     void chatTP(BasePlayer player);
    private void InitializeStuff();
    public void SaveSetContents(string set);
    private void CreateLoadOrder();
     void CheckFactionStale();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    [ConsoleCommand("UI_DestroyUI")]
    private void cmdUI_DestroyUI(ConsoleSystem.Arg arg);
     void DestroyFactionPlayer(FactionPlayer player, bool unloading);
    static double GrabCurrentTime();
     object OnServerCommand(ConsoleSystem.Arg arg);
    private void FactionCreationChat(BasePlayer player, string[] Args);
     bool isAuth(BasePlayer player);
    private string TryForImage(string shortname, ulong skin);
    public string GetImage(string shortname, ulong skin, bool returnUrl);
    public bool HasImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public List<ulong> GetImageList(string shortname);
    public bool isReady();
    public object LFSLocation(ushort ID);
    public object TaxboxLocation(ushort ID);
    public List<string> GetSetContents(string set);
    private void InitializeFactionPlayer(BasePlayer player);
    private void GiveFactionGear(BasePlayer player);
     object CanWearItem(PlayerInventory inventory, Item item);
     object OnItemAction(Item item, string cmd);
     object CanAcceptItem(ItemContainer container, Item item);
    private void FindMonuments();
     void OnEntitySpawned(BaseNetworkable entity);
     object CanUseLockedEntity(BasePlayer player, BaseLock door);
     void AssignTurretAuth(Faction faction, AutoTurret turret);
     void AuthorizePlayerOnTurrets(BasePlayer player);
     void UNAuthorizePlayerOnTurrets(BasePlayer player, ushort oldFaction);
     void SaveFactionPlayer(FactionPlayer player);
    public FactionPlayer GetFactionPlayer(BasePlayer player);
     bool isOwner(BasePlayer player, ushort faction);
     bool isModerator(BasePlayer player, ushort faction);
    public void Broadcast(string message, string userid);
    public void BroadcastFaction(BasePlayer source, string message, ushort faction);
    private void BroadcastOnScreenFaction(string message, ushort faction, ushort faction2);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3);
    private string GetMSG(string message, BasePlayer player, string arg1, string arg2, string arg3);
    private void QuitFactionCreation(BasePlayer player);
    private void RemoveFaction(BasePlayer player, ushort ID, bool admin);
    private void ReassignPlayers(ushort ID);
    static bool AllowedToBuild(BasePlayer player);
    private void DestroyUI(BasePlayer player, bool all);
    private string PanelOnScreen;
    private string PanelFactions;
    private string PanelPlayer;
    private string PanelStatic;
    private string PanelFAnnouncements;
    private string PanelProfile;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public CuiElementContainer CreateOverlayContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer container, string panel, string img, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
    }

    private Dictionary<string, string> UIColors;
    private Dictionary<string, string> ChatColor;
    [ConsoleCommand("UI_FC_ToggleMenu")]
    private void cmdUI_FC_ToggleMenu(ConsoleSystem.Arg arg);
    private void ToggleFCMenu(BasePlayer player);
     void FCBackground(BasePlayer player);
    private void FactionCreation(BasePlayer player, int step);
    [ConsoleCommand("UI_ToggleFactionChat")]
    private void cmdUI_ToggleFactionChat(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ToggleFactionAnnouncement")]
    private void cmdUI_ToggleFactionAnnouncement(ConsoleSystem.Arg arg);
    private void ShadeZone(BasePlayer player, string zoneID);
    private void UnShadeZone(BasePlayer player, string zoneID);
    private object GetZoneLocation(string zoneid);
    private object VerifyZoneID(string zoneid);
     void OnScreen(BasePlayer player, string msg, string color, string arg1, string arg2, string arg3);
     void PlayerPanel(BasePlayer player);
    [ConsoleCommand("UI_SafeZone")]
    private void cmdUI_SafeZone(ConsoleSystem.Arg arg);
    private bool BuildingCheck(BasePlayer player, Vector3 pos);
    [ConsoleCommand("RunConsoleCommand")]
    private void cmdRunConsoleCommand(ConsoleSystem.Arg arg);
    private void ManageFactionMenu(BasePlayer player);
    private void ConfirmFactionDeletion(BasePlayer player, ushort ID);
     void FactionAnnouncementPanel(BasePlayer player);
    private float[] ItemListPos(int number);
    private class Games
    {
        [JsonProperty("response")]
        public Content Response;
        public class Content
        {
            [JsonProperty("game_count")]
            public int GameCount;
            [JsonProperty("games")]
            public Game[] Games;
            public class Game
            {
                [JsonProperty("appid")]
                public uint AppId;
                [JsonProperty("playtime_2weeks")]
                public int PlaytimeTwoWeeks;
                [JsonProperty("playtime_forever")]
                public int PlaytimeForever;
            }

        }

    }

    private class Summaries
    {
        [JsonProperty("response")]
        public Content Response;
        public class Content
        {
            [JsonProperty("players")]
            public Player[] Players;
            public class Player
            {
                [JsonProperty("communityvisibilitystate")]
                public int CommunityVisibilityState;
            }

        }

    }

    private int GetCommunityVisibilityState(Summaries s);
    private T Deserialise(string json);
    private void PrivateCheck(ulong ID);
    private void GetRustTime(ulong ID);
     void PlayerProfilePanel(BasePlayer player);
    static float mapSize;
    static float GetPosition(float pos);
     void FactionPanel(BasePlayer player);
    private string GetDisplayName(ulong UserID);
    private float[] LeftPanelPos(int number);
    private float[] RightPanelPos(int number);
    private float[] MiddlePanelPos(int number);
    private float[] OptionButtonPos(int number);
    private object GetBasePlayer(ulong ID);
    [ConsoleCommand("UI_FC_TogglePrivate")]
     void cmdUI_FC_TogglePrivate(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FC_SendInvite")]
    private void cmdUI_FC_SendInvite(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FC_Moderator")]
    private void cmdUI_FC_Moderator(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FC_KickPlayer")]
    private void cmdUI_FC_KickPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FC_AssignPlayer")]
    private void cmdUI_AssignPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ChangeOption")]
    private void cmdChangeOption(ConsoleSystem.Arg arg);
     void ChangeOption(BasePlayer player, string cmd, string verbiage, string optionName, string status);
    private void CreateOptionButton(CuiElementContainer container, string panelName, string button, string name, string cmd, int num);
    [ConsoleCommand("UI_DestroyOnScreenPanel")]
    private void cmdUI_DestroyOnScreenPanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AutoAuthorization")]
    private void cmdUI_AutoAuthorization(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AuthorizeLeadersOnly")]
    private void cmdUI_AuthorizeLeadersOnly(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DeleteEmptyFactions")]
    private void cmdUI_DeleteEmptyFactions(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SafeZones_Allow")]
    private void cmdUI_SafeZones_Allow(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DisableMenu")]
    private void cmdUI_DisableMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionAnnouncements")]
    private void cmdUI_Use_FactionAnnouncements(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_PrivateFactionChat")]
    private void cmdUI_Use_PrivateFactionChat(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_ShowFactionPlayersOnMap")]
    private void cmdUI_Use_ShowFactionPlayersOnMap(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_LockFactionKits_and_CustomSets")]
    private void cmdUI_LockFactionKits_and_CustomSets(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionChatControl")]
    private void cmdFactionChatControl(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_FactionTags")]
    private void cmdUI_Use_FactionTags(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_AllowPlayersToCreateFactionsTitle")]
    private void cmdUI_Use_AllowPlayersToCreateFactionsTitle(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Use_PlayerListMenu")]
    private void cmdShowOnlinePlayers(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FriendlyFire")]
    private void cmdFriendlyFire(ConsoleSystem.Arg arg);
    private int GetRandomNumber();
    [ConsoleCommand("UI_NewFaction")]
    private void cmdNewFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Description")]
    private void cmdUI_Description(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectColor")]
    private void cmdSelectColor(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectKit")]
    private void cmdUI_SelectKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ViewKit")]
    private void cmdUI_ViewKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FC_DestroyOnScreenPanel")]
    private void cmdUI_FC_DestroyOnScreenPanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectEmbleem")]
    private void cmdUI_SelectEmbleem(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SaveFaction")]
    private void cmdUI_SaveFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DeleteFaction")]
    private void cmdUI_DeleteFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ExitFactionCreation")]
    private void cmdUI_ExitFactionCreation(ConsoleSystem.Arg arg);
    private void ExitFactionCreation(BasePlayer player);
    private void SaveFaction(BasePlayer player);
    [ConsoleCommand("UI_FC_TurnPage")]
    private void cmdUI_ChangePage(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FC_ChangePanel")]
    private void cmdUI_FC_ChangePanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FactionInfo")]
    private void cmdUI_FactionInfo(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ToggleOnlineOnly")]
    private void cmdUI_ToggleOnlineOnly(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ToggleAdminView")]
    private void cmdUI_ToggleAdminView(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_FactionSelection")]
    private void cmdFactionSelection(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AssignPlayerFaction")]
    private void cmdUI_AssignPlayerFaction(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_LeaveFaction")]
    private void cmdUI_LeaveFaction(ConsoleSystem.Arg arg);
    private int GetMax();
    private int GetMin();
    private void SetJoinCooldown(BasePlayer player);
    private void AssignPlayerToFaction(BasePlayer player, ushort faction);
    private void UnassignPlayerFromFaction(ulong playerID, bool admin);
    [ChatCommand("faction")]
    private void cmdFaction(BasePlayer player, string command, string[] args);
    private float[] CalcButtonPos(int number);
    private float[] CalcEquipPos(int number);
    private float[] CalcInvItemPos(int number);
    private float[] PlayerEntryPos(int number);
    [HookMethod("BroadcastOnScreen")]
     void BroadcastOnScreen(string message, ushort Faction, ushort Faction2);
    [HookMethod("AssignToFaction")]
     bool AssignToFaction(ulong ID, ushort Faction);
    [HookMethod("ClearFactionPlayers")]
     void ClearFactionPlayers(ushort ID);
    [HookMethod("GivePlayerFactionGear")]
     bool GivePlayerFactionGear(BasePlayer player);
    [HookMethod("IsFaction")]
     bool IsFaction(ushort ID);
    [HookMethod("GetPlayerFaction")]
     object GetPlayerFaction(ulong ID);
    [HookMethod("GetFactionInfo")]
     object GetFactionInfo(ushort ID, string request);
    [HookMethod("GetFactionList")]
     List<ushort> GetFactionList();
    [HookMethod("CheckSameFaction")]
     bool CheckSameFaction(ulong player1ID, ulong player2ID);
    private void AddFactionListLM(ulong playerID, string FactionName, List<ulong> IDs);
    private void RemoveFactionListLM(ulong playerID, string FactionName);
    private void UpdateFactionListLM(ulong playerID, string FactionName, List<ulong> IDs);
    private void AddFriendLM(ulong playerID, string Name, ulong friendID);
    private void RemoveFriendLM(ulong playerID, string Name, ulong friendID);
    private void SaveLoop();
    private void InfoLoop();
    private void ReloadPlayerPanel();
    private static readonly DateTime Epoch;
    static double CurrentTotalMinutes();
    [Serializable]
    public class FactionPlayer : MonoBehaviour
    {
        public BasePlayer player;
        public ushort Faction;
        public string Panel;
        public int page;
        public bool open;
        public bool admin;
        public bool OnlineOnly;
        public ushort SelectedFaction;
        public ulong TargetPlayer;
         void Awake();
    }

    public class FactionsData
    {
        public Dictionary<ulong, PlayerData> Players;
        public Dictionary<ushort, Faction> Factions;
    }

    public class Faction
    {
        public string Name;
        public string tag;
        public ulong Leader;
        public List<ulong> Moderators;
        public Dictionary<int, string> FactionAnnouncements;
        public bool Private;
        public string UIColor;
        public string ChatColor;
        public string KitorSet;
        public string group;
        public string description;
        public string embleem;
        public List<ulong> BanList;
        public List<ulong> factionPlayers;
        public double LastMemberLoggedIn;
        public bool AutoAuthorization;
    }

    public class PlayerData
    {
        public ushort faction;
        public List<ushort> PendingInvites;
        public int RustLifeTime;
        public int RustTwoWeekTime;
    }

     class FactionDesigner
    {
        public ushort ID;
        public bool creating;
        public bool editing;
        public Faction faction;
        public int stepNum;
    }

     void SaveData();
     void LoadData();
    private Dictionary<string, Slot> ItemSlots;
    private Dictionary<int, string> DefaultEmblems;
    public static string HexTOUIColor(string hexColor);
    private List<string> DefaultColors;
    private Dictionary<ushort, Faction> DefaultFactions;
    private ConfigData configData;
     class ConfigData
    {
        public int InfoInterval { get; set; }
        public string MenuKeyBinding { get; set; }
        public bool DisableMenu { get; set; }
        public bool Use_FactionAnnouncements { get; set; }
        public bool Use_PrivateFactionChat { get; set; }
        public bool Use_FactionChatControl { get; set; }
        public bool Use_FactionTags { get; set; }
        public bool Use_PlayerListMenu { get; set; }
        public bool Use_Map { get; set; }
        public bool LockFactionKits_and_CustomSets { get; set; }
        public bool ShowFactionPlayersOnMap { get; set; }
        public int FactionLimit { get; set; }
        public int FactionPlayerLimit { get; set; }
        public int AllowedFactionDifference { get; set; }
        public bool Use_AllowPlayersToCreateFactions { get; set; }
        public bool Allow_FriendlyFire { get; set; }
        public float FriendlyFire_DamageScale { get; set; }
        public string HomePageMessage { get; set; }
        public int FactionStaleTime { get; set; }
        public bool AutoAuthorization { get; set; }
        public bool AuthorizeLeadersOnly { get; set; }
        public bool SafeZones_Allow { get; set; }
        public bool DeleteEmptyFactions { get; set; }
        public float SafeZones_Radius { get; set; }
        public string InFactionChat_ChatColor { get; set; }
        public Dictionary<int, string> FactionEmblems_URLS { get; set; }
        public List<string> Colors { get; set; }
        public List<string> Kits_and_CustomSets { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

 class Monuments
{
    public Vector3 position;
    public float radius;
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public CuiElementContainer CreateOverlayContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer container, string panel, string img, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
}

private class Games
{
    [JsonProperty("response")]
    public Content Response;
    public class Content
    {
        [JsonProperty("game_count")]
        public int GameCount;
        [JsonProperty("games")]
        public Game[] Games;
        public class Game
        {
            [JsonProperty("appid")]
            public uint AppId;
            [JsonProperty("playtime_2weeks")]
            public int PlaytimeTwoWeeks;
            [JsonProperty("playtime_forever")]
            public int PlaytimeForever;
        }

    }

}

public class Content
{
    [JsonProperty("game_count")]
    public int GameCount;
    [JsonProperty("games")]
    public Game[] Games;
    public class Game
    {
        [JsonProperty("appid")]
        public uint AppId;
        [JsonProperty("playtime_2weeks")]
        public int PlaytimeTwoWeeks;
        [JsonProperty("playtime_forever")]
        public int PlaytimeForever;
    }

}

public class Game
{
    [JsonProperty("appid")]
    public uint AppId;
    [JsonProperty("playtime_2weeks")]
    public int PlaytimeTwoWeeks;
    [JsonProperty("playtime_forever")]
    public int PlaytimeForever;
}

private class Summaries
{
    [JsonProperty("response")]
    public Content Response;
    public class Content
    {
        [JsonProperty("players")]
        public Player[] Players;
        public class Player
        {
            [JsonProperty("communityvisibilitystate")]
            public int CommunityVisibilityState;
        }

    }

}

public class Content
{
    [JsonProperty("players")]
    public Player[] Players;
    public class Player
    {
        [JsonProperty("communityvisibilitystate")]
        public int CommunityVisibilityState;
    }

}

public class Player
{
    [JsonProperty("communityvisibilitystate")]
    public int CommunityVisibilityState;
}

[Serializable]
public class FactionPlayer : MonoBehaviour
{
    public BasePlayer player;
    public ushort Faction;
    public string Panel;
    public int page;
    public bool open;
    public bool admin;
    public bool OnlineOnly;
    public ushort SelectedFaction;
    public ulong TargetPlayer;
     void Awake();
}

public class FactionsData
{
    public Dictionary<ulong, PlayerData> Players;
    public Dictionary<ushort, Faction> Factions;
}

public class Faction
{
    public string Name;
    public string tag;
    public ulong Leader;
    public List<ulong> Moderators;
    public Dictionary<int, string> FactionAnnouncements;
    public bool Private;
    public string UIColor;
    public string ChatColor;
    public string KitorSet;
    public string group;
    public string description;
    public string embleem;
    public List<ulong> BanList;
    public List<ulong> factionPlayers;
    public double LastMemberLoggedIn;
    public bool AutoAuthorization;
}

public class PlayerData
{
    public ushort faction;
    public List<ushort> PendingInvites;
    public int RustLifeTime;
    public int RustTwoWeekTime;
}

 class FactionDesigner
{
    public ushort ID;
    public bool creating;
    public bool editing;
    public Faction faction;
    public int stepNum;
}

 class ConfigData
{
    public int InfoInterval { get; set; }
    public string MenuKeyBinding { get; set; }
    public bool DisableMenu { get; set; }
    public bool Use_FactionAnnouncements { get; set; }
    public bool Use_PrivateFactionChat { get; set; }
    public bool Use_FactionChatControl { get; set; }
    public bool Use_FactionTags { get; set; }
    public bool Use_PlayerListMenu { get; set; }
    public bool Use_Map { get; set; }
    public bool LockFactionKits_and_CustomSets { get; set; }
    public bool ShowFactionPlayersOnMap { get; set; }
    public int FactionLimit { get; set; }
    public int FactionPlayerLimit { get; set; }
    public int AllowedFactionDifference { get; set; }
    public bool Use_AllowPlayersToCreateFactions { get; set; }
    public bool Allow_FriendlyFire { get; set; }
    public float FriendlyFire_DamageScale { get; set; }
    public string HomePageMessage { get; set; }
    public int FactionStaleTime { get; set; }
    public bool AutoAuthorization { get; set; }
    public bool AuthorizeLeadersOnly { get; set; }
    public bool SafeZones_Allow { get; set; }
    public bool DeleteEmptyFactions { get; set; }
    public float SafeZones_Radius { get; set; }
    public string InFactionChat_ChatColor { get; set; }
    public Dictionary<int, string> FactionEmblems_URLS { get; set; }
    public List<string> Colors { get; set; }
    public List<string> Kits_and_CustomSets { get; set; }
}


```

---

## FallDamage by alibali - Modifies or disables the fall damage for players

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Fall Damage", "Wulf", "3.0.0")]
[Description("Modifies or disables the fall damage for players")]
 class FallDamage : CovalencePlugin
{
    private Configuration config;
     class Configuration
    {
        [JsonProperty("Apply realistic fall damage")]
        public bool RealisticDamage { get; set; }
        [JsonProperty("Damage modifier for falls")]
        public float DamageModifier { get; set; }
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private const string permNone;
    private void Init();
}

 class Configuration
{
    [JsonProperty("Apply realistic fall damage")]
    public bool RealisticDamage { get; set; }
    [JsonProperty("Damage modifier for falls")]
    public float DamageModifier { get; set; }
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## FamilyShareDetect by AVOCoder - Checks players if using Family Sharing

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Plugins;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Family Share Detect", "AVOCoder", "1.2.0")]
[Description("Checks players if using Family Sharing")]
 class FamilyShareDetect : CovalencePlugin
{
    private ConfigData cfg;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Broadcast Check in Console")]
        public bool broadcastConsoleCheck { get; set; }
        [JsonProperty(PropertyName = "Kick Family Sharing Player")]
        public bool kick { get; set; }
        [JsonProperty(PropertyName = "Kick only if App Owner in Server Players list")]
        public bool kickIfOwnerIsPlayer { get; set; }
        [JsonProperty(PropertyName = "Broadcast Kick in Console")]
        public bool broadcastConsoleKick { get; set; }
        [JsonProperty(PropertyName = "Log detects")]
        public bool logDetects { get; set; }
        [JsonProperty(PropertyName = "Whitelist")]
        public List<string> whitelist { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetDefaultConfigData();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void BroadcastInConsole(string message);
    private void BroadcastKickInConsole(string message);
    private void Log(string name, string message);
    private void OnUserConnected(IPlayer player);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Broadcast Check in Console")]
    public bool broadcastConsoleCheck { get; set; }
    [JsonProperty(PropertyName = "Kick Family Sharing Player")]
    public bool kick { get; set; }
    [JsonProperty(PropertyName = "Kick only if App Owner in Server Players list")]
    public bool kickIfOwnerIsPlayer { get; set; }
    [JsonProperty(PropertyName = "Broadcast Kick in Console")]
    public bool broadcastConsoleKick { get; set; }
    [JsonProperty(PropertyName = "Log detects")]
    public bool logDetects { get; set; }
    [JsonProperty(PropertyName = "Whitelist")]
    public List<string> whitelist { get; set; }
}


```

---

## FancyDrop by FastBurst - The all-in-one airdrop toolset

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Reflection;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Plugins;
using IEnumerator = System.Collections.IEnumerator;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("FancyDrop", "FastBurst", "3.2.6")]
[Description("The Next Level of a fancy airdrop-toolset")]
 class FancyDrop : RustPlugin
{
    [PluginReference]
     Plugin AlphaLoot;
     Plugin GUIAnnouncements;
     Plugin MagicLoot;
    private static FancyDrop Instance;
     bool Changed;
     bool initialized;
     Vector3 lastDropPos;
     float lastDropRadius;
     Vector3 lastLootPos;
     int lastMinute;
     double lastHour;
     string msgConsoleDropSpawn;
    static string msgConsoleDropLanded;
     Timer _aidropTimer;
     Timer _massDropTimer;
    private const string SMOKE_EFFECT;
    private const string SIRENLIGHT_EFFECT;
    private const string SIRENALARM_EFFECT;
    private const string PLANE_PREFAB;
    private const string SUPPLY_PREFAB;
    private const string HELIBURST_PREFAB;
    private const string ROCKETSMOKE_PREFAB;
    private const string EXPLOSION_PREFAB;
    private const string HELIEXPLOSION_EFFECT;
    private const string BRADLEY_CANNON_EFFECT;
    private const string PARACHUTE_PREFAB;
    private static RaycastHit rayCastHit;
    private const int CAST_LAYERS;
     List<CargoPlane> CargoPlanes;
     List<SupplyDrop> SupplyDrops;
    private bool IsFancySupplyDrop(SupplyDrop drop);
     List<SupplyDrop> LootedDrops;
     List<BaseEntity> activeSignals;
     Dictionary<BasePlayer, Timer> timers;
    private Dictionary<string, int> itemNameToId;
    private ConfigData.StaticOptions.LootOptions lootTable;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity, ThrownWeapon thrown);
    private void SupplyThrown(BasePlayer player, BaseEntity entity);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private object getFancyDropTypes();
    private void OnTick();
    private void OnTickReal();
    private void OnTickServer();
    private bool IsFancyDrop(CargoPlane plane);
    private object IsFancyDropType(CargoPlane plane);
    private void OverrideDropTime(CargoPlane plane, float seconds);
    sealed class ColliderCheck : FacepunchBehaviour
    {
        public bool notifyEnabled;
        public bool notifyConsole;
        public Dictionary<string, object> cratesettings;
        public bool landed;
        public int hitCounter;
        public bool wasHit;
        public string dropType;
        private BaseEntity parachute;
        private void Awake();
        private void PlayerStoppedLooting(BasePlayer player);
        private void OnCollisionEnter(Collision col);
        private IEnumerator HitRemove(bool explode);
        private IEnumerator DeSpawn();
        private void OnDestroy();
        internal void OnGroundCollision();
        public ColliderCheck();
    }

     class DropTiming : FacepunchBehaviour
    {
        private int dropCount;
        private float updatedTime;
        public Vector3 startPos;
        public Vector3 endPos;
        public bool notify;
        private bool notifyConsole;
        private int cratesToDrop;
        public string dropType;
        public Dictionary<string, object> dropsettings;
        public ulong userID;
        private float gapTimeToTake;
        private float halfTimeToTake;
        private float offsetTimeToTake;
        private void Awake();
        public void GetSettings(Dictionary<string, object> drop, Vector3 start, Vector3 end, float seconds);
        public void TimeOverride(float seconds);
        private void Update();
        private void OnDestroy();
        public DropTiming();
    }

     CargoPlane createCargoPlane(Vector3 pos);
    private static void RunEffect(string name, BaseEntity entity, Vector3 position, Vector3 offset);
    private static void CreateSirenLights(BaseEntity entity);
    private static void CreateSirenAlarms(BaseEntity entity);
    private void CreateDropEffects(BaseEntity entity);
    private void createSupplyDrop(Vector3 pos, Dictionary<string, object> cratesettings, bool notify, bool notifyConsole, Vector3 start, Vector3 end, string dropType, ulong userID);
    private void createSupplyDropSpace(Vector3 pos, Dictionary<string, object> cratesettings, bool notify, bool notifyConsole, Vector3 start, Vector3 end, string dropType, ulong userID);
    private void SpawnNetworkable(BaseNetworkable ent);
    private void CreateRocket(Vector3 startPoint);
    private static Dictionary<string, object> getObjects(string name);
    public static Dictionary<string, TValue> ToDictionary(object obj);
    private void startCargoPlane(Vector3 dropToPos, bool randomDrop, CargoPlane plane, string dropType, string staticList, bool showinfo, ulong userID);
    private static string GetGridString(Vector3 position);
    private void DropNotifier(Vector3 dropToPos, string dropType, string staticList, string notificationInfo);
    private void airdropTimerNext(int custom);
    private void airdropTimerRun();
    private void airdropTimerStop();
    private void removeBuiltInAirdrop();
    private static BasePlayer FindPlayerByName(string name);
    private void ClearContainer(ItemContainer itemContainer);
    private void FillLootContainer(BaseEntity Container, string dropType);
    private void SetupContainer(StorageContainer drop, string dropType, bool CustomLoot);
    private class UIColor
    {
         string color;
        public UIColor(double red, double green, double blue, double alpha);
        public override string ToString();
    }

    private class UIObject
    {
         List<object> ui;
         List<string> objectList;
        public UIObject();
        public void Draw(BasePlayer player);
        public void Destroy(BasePlayer player);
        public string AddText(string name, double left, double top, double width, double height, string color, string text, int textsize, string parent, int alignmode, float fadeIn, float fadeOut);
    }

    private void UIMessage(BasePlayer player, string message);
    private string StripTags(string original);
    private class DropBehaviour : MonoBehaviour
    {
        private SupplyDrop sDrop;
        private Rigidbody rBody;
        private Transform transForm;
        private float heightAtPos;
        private float distToTarget;
        private bool DeployedChute;
        private BaseEntity parachute;
        private Vector3 velocityDrop;
        private void Awake();
        private void Start();
        private void Update();
        private void OnGroundCollision();
    }

    [ConsoleCommand("ad.random")]
    private void dropRandom(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.topos")]
    private void dropToPos(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.massdrop")]
    private void dropMass(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.massdropto")]
    private void dropMassTo(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.toplayer")]
    private void dropToPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.dropplayer")]
    private void dropDropOnly(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.dropspace")]
    private void dropDropSpaceOnly(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.timer")]
    private void dropReloadTimer(ConsoleSystem.Arg arg);
    [ConsoleCommand("ad.cleanup")]
    private void dropCleanUp(ConsoleSystem.Arg arg);
    private void airdropCleanUp();
    [ConsoleCommand("ad.lootreload")]
    private void dropLootReload(ConsoleSystem.Arg arg);
    [ChatCommand("droprandom")]
    private void cdropRandom(BasePlayer player, string command);
    [ChatCommand("droptopos")]
    private void cdropToPos(BasePlayer player, string command, string[] args);
    [ChatCommand("droptoplayer")]
    private void cdropToPlayer(BasePlayer player, string command, string[] args);
    [ChatCommand("dropdirect")]
    private void cdropDirect(BasePlayer player, string command, string[] args);
    [ChatCommand("dropspace")]
    private void cdropDirectSpace(BasePlayer player, string command, string[] args);
    [ChatCommand("dropmass")]
    private void cdropMass(BasePlayer player, string command, string[] args);
    [ChatCommand("droptomass")]
    private void cdropToMass(BasePlayer player, string command, string[] args);
    static Dictionary<string, object> defaultRealTimers();
    static Dictionary<string, object> defaultServerTimers();
    static Dictionary<string, object> defaultDrop();
    static Dictionary<string, object> defaultDropTypes();
    static Dictionary<string, object> defaultItemList();
     FieldInfo dropPlanestartPos;
     FieldInfo dropPlaneendPos;
     FieldInfo dropPlanesecondsToTake;
     FieldInfo dropPlanesecondsTaken;
     FieldInfo dropPlanedropped;
     FieldInfo _isSpawned;
     FieldInfo _creationFrame;
    static FieldInfo _parachute;
     CargoPlane referencePlane;
     SpawnFilter spawnFilter;
     List<Regex> regexTags;
     List<string> tags;
     Dictionary<string, object> setupDropTypes;
     Dictionary<string, object> setupDropDefault;
     Dictionary<string, object> realTimers;
     Dictionary<string, object> serverTimers;
    private static ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Debug")]
        public DebugOptions DebugSettings { get; set; }
        [JsonProperty(PropertyName = "General Settings")]
        public GenericOptions GenericSettings { get; set; }
        [JsonProperty(PropertyName = "Airdrop & General Plane Settings")]
        public AirdropOptions AirdropSettings { get; set; }
        [JsonProperty(PropertyName = "Allow Damage Airdrop Settings")]
        public DamageOptions DamageSettings { get; set; }
        [JsonProperty(PropertyName = "Notification Settings")]
        public NotificationOptions Notifications { get; set; }
        [JsonProperty(PropertyName = "SimpleUI")]
        public UIOptions UISettings { get; set; }
        [JsonProperty(PropertyName = "Timer - Random Event")]
        public TimerOptions TimerSettings { get; set; }
        [JsonProperty(PropertyName = "Timers- System Timers")]
        public TimersOptions TimersSettings { get; set; }
        [JsonProperty(PropertyName = "Drop Settings")]
        public DropOptions DropSettings { get; set; }
        [JsonProperty(PropertyName = "Custom Loot Items")]
        public StaticOptions StaticItems { get; set; }
        public class DebugOptions
        {
            [JsonProperty(PropertyName = "Enable Debugging to Console")]
            public bool useDebug { get; set; }
        }

        public class GenericOptions
        {
            [JsonProperty(PropertyName = "Admin messages color")]
            public string colorAdmMsg { get; set; }
            [JsonProperty(PropertyName = "AuthLevel needed for console commands")]
            public int neededAuthLvl { get; set; }
            [JsonProperty(PropertyName = "Broadcast messages color")]
            public string colorTextMsg { get; set; }
            [JsonProperty(PropertyName = "Chat/Message prefix")]
            public string Prefix { get; set; }
            [JsonProperty(PropertyName = "Prefix color")]
            public string Color { get; set; }
            [JsonProperty(PropertyName = "Prefix format")]
            public string Format { get; set; }
            [JsonProperty(PropertyName = "GUI Announce command")]
            public string guiCommand { get; set; }
            [JsonProperty(PropertyName = "Show message to admin after command usage")]
            public bool notifyByChatAdminCalls { get; set; }
            [JsonProperty(PropertyName = "Time for active smoke of SupplySignal (default is 210)")]
            public float supplySignalSmokeTime { get; set; }
            [JsonProperty(PropertyName = "Lock DirectDrop to be looted only by target player")]
            public bool lockDirectDrop { get; set; }
            [JsonProperty(PropertyName = "Lock SignalDrop to be looted only by target player")]
            public bool lockSignalDrop { get; set; }
            [JsonProperty(PropertyName = "Unlock crates only after player stopped looting")]
            public bool unlockDropAfterLoot { get; set; }
        }

        public class AirdropOptions
        {
            [JsonProperty(PropertyName = "Default radius for location based massdrop")]
            public float airdropMassdropRadiusDefault { get; set; }
            [JsonProperty(PropertyName = "Delay between Massdrop plane spawns")]
            public float airdropMassdropDelay { get; set; }
            [JsonProperty(PropertyName = "Massdrop default plane amount")]
            public int airdropMassdropDefault { get; set; }
            [JsonProperty(PropertyName = "Multiplier for (plane height * highest point on Map); Default 1.0")]
            public float planeOffSetYMultiply { get; set; }
            [JsonProperty(PropertyName = "Multiplier for overall flight distance; lower means faster at map")]
            public float planeOffSetXMultiply { get; set; }
            [JsonProperty(PropertyName = "Disable SupplySignal randomization")]
            public bool disableRandomSupplyPos { get; set; }
            [JsonProperty(PropertyName = "Visual & Sound Effects Settings")]
            public EffectsOptions EffectsSettings { get; set; }
            [JsonProperty(PropertyName = "Space Delivery Drop Settings")]
            public SpaceOptions SpaceSettings { get; set; }
            public class EffectsOptions
            {
                [JsonProperty(PropertyName = "Use Explosion Sound Effect when hits ground position")]
                public bool useSupplyDropEffectLanded { get; set; }
                [JsonProperty(PropertyName = "Deploy Smoke on drop as it falls")]
                public bool useSmokeEffectOnDrop { get; set; }
                [JsonProperty(PropertyName = "Deploy with Audio Alarm on drop")]
                public bool useSirenAlarmOnDrop { get; set; }
                [JsonProperty(PropertyName = "Deploy with Audio Alarms on drop only during the night")]
                public bool useSirenAlarmAtNightOnly { get; set; }
                [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop")]
                public bool useSirenLightOnDrop { get; set; }
                [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop only during the night")]
                public bool useSirenLightAtNightOnly { get; set; }
                [JsonProperty(PropertyName = "Enable Rocket Signal upon Supply Drop Landing")]
                public bool useSupplyDropRocket { get; set; }
                [JsonProperty(PropertyName = "Signal rocket speed")]
                public float signalRocketSpeed { get; set; }
                [JsonProperty(PropertyName = "Signal rocket explosion timer")]
                public float signalRocketExplosionTime { get; set; }
            }

            public class SpaceOptions
            {
                [JsonProperty(PropertyName = "Incoming Space Delivery Supply Drop velocity")]
                public float dropVelocity { get; set; }
                [JsonProperty(PropertyName = "Parachute deploy distance from ground")]
                public float dropGroundDistance { get; set; }
            }

        }

        public class DamageOptions
        {
            [JsonProperty(PropertyName = "Players can shoot down the drop")]
            public bool shootDownDrops { get; set; }
            [JsonProperty(PropertyName = "Players can shoot down the drop - needed hits")]
            public int shootDownCount { get; set; }
            [JsonProperty(PropertyName = "Set Angular Drag for drop")]
            public float dropAngularDragSetting { get; set; }
            [JsonProperty(PropertyName = "Set Drag for drop (drop resistance)")]
            public float dropDragsetting { get; set; }
            [JsonProperty(PropertyName = "Set drop allow to explode on impact")]
            public bool explodeImpact { get; set; }
            [JsonProperty(PropertyName = "Set drop chance exploding on impact (x out of 100)")]
            public int explodeChance { get; set; }
            [JsonProperty(PropertyName = "Set Mass weight for drop")]
            public float dropMassSetting { get; set; }
        }

        public class NotificationOptions
        {
            [JsonProperty(PropertyName = "Maximum distance in meters to get notified about landed Drop")]
            public float supplyDropNotifyDistance { get; set; }
            [JsonProperty(PropertyName = "Maximum distance in meters to get notified about looted Drop")]
            public float supplyLootNotifyDistance { get; set; }
            [JsonProperty(PropertyName = "Notify a player about incoming Drop to his location")]
            public bool notifyDropPlayer { get; set; }
            [JsonProperty(PropertyName = "Notify a player about spawned Drop at his location")]
            public bool notifyDropDirect { get; set; }
            [JsonProperty(PropertyName = "Notify a player about Space Delivery Drop at his location")]
            public bool notifyDropDirectSpace { get; set; }
            [JsonProperty(PropertyName = "Notify admins per chat about player who has thrown SupplySignal")]
            public bool notifyDropAdminSignal { get; set; }
            [JsonProperty(PropertyName = "Notify console at Drop by SupplySignal")]
            public bool notifyDropConsoleSignal { get; set; }
            [JsonProperty(PropertyName = "Notify console at timed-regular Drop")]
            public bool notifyDropConsoleRegular { get; set; }
            [JsonProperty(PropertyName = "Notify console when a Drop is being looted")]
            public bool notifyDropConsoleLooted { get; set; }
            [JsonProperty(PropertyName = "Notify console when Drop is landed")]
            public bool notifyDropConsoleOnLanded { get; set; }
            [JsonProperty(PropertyName = "Notify console when Drop is spawned")]
            public bool notifyDropConsoleSpawned { get; set; }
            [JsonProperty(PropertyName = "Notify console when Drop landed/spawned only at the first")]
            public bool notifyDropConsoleFirstOnly { get; set; }
            [JsonProperty(PropertyName = "Notify players at custom/event Drop")]
            public bool notifyDropServerCustom { get; set; }
            [JsonProperty(PropertyName = "Notify players at custom/event Drop including Coords")]
            public bool notifyDropServerCustomCoords { get; set; }
            [JsonProperty(PropertyName = "Notify players at custom/event Drop including Grid Area")]
            public bool notifyDropServerCustomGrid { get; set; }
            [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal")]
            public bool notifyDropServerSignal { get; set; }
            [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal including Coords")]
            public bool notifyDropServerSignalCoords { get; set; }
            [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal including Grid Area")]
            public bool notifyDropServerSignalGrid { get; set; }
            [JsonProperty(PropertyName = "Notify players at Massdrop")]
            public bool notifyDropServerMass { get; set; }
            [JsonProperty(PropertyName = "Notify players at Random/Timed Drop")]
            public bool notifyDropServerRegular { get; set; }
            [JsonProperty(PropertyName = "Notify players at Random/Timed Drop including Coords")]
            public bool notifyDropServerRegularCoords { get; set; }
            [JsonProperty(PropertyName = "Notify players at Random/Timed Drop including Grid Area")]
            public bool notifyDropServerRegularGrid { get; set; }
            [JsonProperty(PropertyName = "Notify players when a Drop is being looted")]
            public bool notifyDropServerLooted { get; set; }
            [JsonProperty(PropertyName = "Notify players when a Drop is being looted including coords")]
            public bool notifyDropServerLootedCoords { get; set; }
            [JsonProperty(PropertyName = "Notify players when a Drop is being looted including Grid Area")]
            public bool notifyDropServerLootedGrid { get; set; }
            [JsonProperty(PropertyName = "Notify players when Drop is landed about distance")]
            public bool notifyDropPlayersOnLanded { get; set; }
            [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal")]
            public bool notifyDropSignalByPlayer { get; set; }
            [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal including coords")]
            public bool notifyDropSignalByPlayerCoords { get; set; }
            [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal including Grid Area")]
            public bool notifyDropSignalByPlayerGrid { get; set; }
            [JsonProperty(PropertyName = "Use GUI Announcements for any Drop notification")]
            public bool notifyDropGUI { get; set; }
        }

        public class UIOptions
        {
            [JsonProperty(PropertyName = "SimpleUI_Enable")]
            public bool SimpleUI_Enable { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_FontSize")]
            public int SimpleUI_FontSize { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_HideTimer")]
            public float SimpleUI_HideTimer { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_NoticeColor")]
            public string SimpleUI_NoticeColor { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_ShadowColor")]
            public string SimpleUI_ShadowColor { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_Top")]
            public float SimpleUI_Top { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_Left")]
            public float SimpleUI_Left { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_MaxWidth")]
            public float SimpleUI_MaxWidth { get; set; }
            [JsonProperty(PropertyName = "SimpleUI_MaxHeight")]
            public float SimpleUI_MaxHeight { get; set; }
        }

        public class TimerOptions
        {
            [JsonProperty(PropertyName = "Use Airdrop timer")]
            public bool airdropTimerEnabled { get; set; }
            [JsonProperty(PropertyName = "Used Airdrop timer command")]
            public string airdropTimerCmd { get; set; }
            [JsonProperty(PropertyName = "Minimum players for timed Drop")]
            public int airdropTimerMinPlayers { get; set; }
            [JsonProperty(PropertyName = "Minimum minutes for random timer delay")]
            public int airdropTimerWaitMinutesMin { get; set; }
            [JsonProperty(PropertyName = "Maximum minutes for random timer delay")]
            public int airdropTimerWaitMinutesMax { get; set; }
            [JsonProperty(PropertyName = "Reset Timer after manual random drop")]
            public bool airdropTimerResetAfterRandom { get; set; }
            [JsonProperty(PropertyName = "Remove builtIn Airdrop")]
            public bool airdropRemoveInBuilt { get; set; }
            [JsonProperty(PropertyName = "Cleanup old Drops at serverstart")]
            public bool airdropCleanupAtStart { get; set; }
        }

        public class TimersOptions
        {
            [JsonProperty(PropertyName = "Log to console")]
            public bool logTimersToConsole { get; set; }
            [JsonProperty(PropertyName = "Minimum players for running Timers")]
            public int timersMinPlayers { get; set; }
            [JsonProperty(PropertyName = "Use RealTime")]
            public bool useRealtimeTimers { get; set; }
            [JsonProperty(PropertyName = "RealTime")]
            public Dictionary<string, object> realTimers { get; set; }
            [JsonProperty(PropertyName = "Use ServerTime")]
            public bool useGametimeTimers { get; set; }
            [JsonProperty(PropertyName = "ServerTime")]
            public Dictionary<string, object> serverTimers { get; set; }
        }

        public class DropOptions
        {
            [JsonProperty(PropertyName = "DropDefault")]
            public Dictionary<string, object> setupDropDefault { get; set; }
            [JsonProperty(PropertyName = "DropTypes")]
            public Dictionary<string, object> setupDropTypes { get; set; }
        }

        public class StaticOptions
        {
            [JsonProperty(PropertyName = "DropTypes")]
            public List<LootOptions> LootSettings { get; set; }
            public class LootOptions
            {
                [JsonProperty(PropertyName = "DropType Name")]
                public string DropTypeName { get; set; }
                [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
                public int MinimumItems { get; set; }
                [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
                public int MaximumItems { get; set; }
                [JsonProperty(PropertyName = "Custom Loot Contents")]
                public List<LootItem> Items { get; set; }
                public class LootItem
                {
                    [JsonProperty(PropertyName = "Shortname")]
                    public string Name { get; set; }
                    [JsonProperty(PropertyName = "Minimum amount of item")]
                    public int Minimum { get; set; }
                    [JsonProperty(PropertyName = "Maximum amount of item")]
                    public int Maximum { get; set; }
                    [JsonProperty(PropertyName = "Skin ID")]
                    public ulong Skin { get; set; }
                    [JsonProperty(PropertyName = "Display Name")]
                    public string DisplayName { get; set; }
                }

            }

        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void MessageToAllGui(string message);
    private void MessageToPlayerUI(BasePlayer player, string message);
    private void MessageToPlayer(BasePlayer player, string message);
    private string GetDirectionAngle(float angle, string UserIDString);
    private void NotifyOnDropLanded(BaseEntity drop);
    private void NotifyOnDropLooted(BaseEntity drop, BasePlayer looter);
    private static void SendChatMessage(string key, object[] args);
    private static string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

sealed class ColliderCheck : FacepunchBehaviour
{
    public bool notifyEnabled;
    public bool notifyConsole;
    public Dictionary<string, object> cratesettings;
    public bool landed;
    public int hitCounter;
    public bool wasHit;
    public string dropType;
    private BaseEntity parachute;
    private void Awake();
    private void PlayerStoppedLooting(BasePlayer player);
    private void OnCollisionEnter(Collision col);
    private IEnumerator HitRemove(bool explode);
    private IEnumerator DeSpawn();
    private void OnDestroy();
    internal void OnGroundCollision();
    public ColliderCheck();
}

 class DropTiming : FacepunchBehaviour
{
    private int dropCount;
    private float updatedTime;
    public Vector3 startPos;
    public Vector3 endPos;
    public bool notify;
    private bool notifyConsole;
    private int cratesToDrop;
    public string dropType;
    public Dictionary<string, object> dropsettings;
    public ulong userID;
    private float gapTimeToTake;
    private float halfTimeToTake;
    private float offsetTimeToTake;
    private void Awake();
    public void GetSettings(Dictionary<string, object> drop, Vector3 start, Vector3 end, float seconds);
    public void TimeOverride(float seconds);
    private void Update();
    private void OnDestroy();
    public DropTiming();
}

private class UIColor
{
     string color;
    public UIColor(double red, double green, double blue, double alpha);
    public override string ToString();
}

private class UIObject
{
     List<object> ui;
     List<string> objectList;
    public UIObject();
    public void Draw(BasePlayer player);
    public void Destroy(BasePlayer player);
    public string AddText(string name, double left, double top, double width, double height, string color, string text, int textsize, string parent, int alignmode, float fadeIn, float fadeOut);
}

private class DropBehaviour : MonoBehaviour
{
    private SupplyDrop sDrop;
    private Rigidbody rBody;
    private Transform transForm;
    private float heightAtPos;
    private float distToTarget;
    private bool DeployedChute;
    private BaseEntity parachute;
    private Vector3 velocityDrop;
    private void Awake();
    private void Start();
    private void Update();
    private void OnGroundCollision();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Debug")]
    public DebugOptions DebugSettings { get; set; }
    [JsonProperty(PropertyName = "General Settings")]
    public GenericOptions GenericSettings { get; set; }
    [JsonProperty(PropertyName = "Airdrop & General Plane Settings")]
    public AirdropOptions AirdropSettings { get; set; }
    [JsonProperty(PropertyName = "Allow Damage Airdrop Settings")]
    public DamageOptions DamageSettings { get; set; }
    [JsonProperty(PropertyName = "Notification Settings")]
    public NotificationOptions Notifications { get; set; }
    [JsonProperty(PropertyName = "SimpleUI")]
    public UIOptions UISettings { get; set; }
    [JsonProperty(PropertyName = "Timer - Random Event")]
    public TimerOptions TimerSettings { get; set; }
    [JsonProperty(PropertyName = "Timers- System Timers")]
    public TimersOptions TimersSettings { get; set; }
    [JsonProperty(PropertyName = "Drop Settings")]
    public DropOptions DropSettings { get; set; }
    [JsonProperty(PropertyName = "Custom Loot Items")]
    public StaticOptions StaticItems { get; set; }
    public class DebugOptions
    {
        [JsonProperty(PropertyName = "Enable Debugging to Console")]
        public bool useDebug { get; set; }
    }

    public class GenericOptions
    {
        [JsonProperty(PropertyName = "Admin messages color")]
        public string colorAdmMsg { get; set; }
        [JsonProperty(PropertyName = "AuthLevel needed for console commands")]
        public int neededAuthLvl { get; set; }
        [JsonProperty(PropertyName = "Broadcast messages color")]
        public string colorTextMsg { get; set; }
        [JsonProperty(PropertyName = "Chat/Message prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Prefix color")]
        public string Color { get; set; }
        [JsonProperty(PropertyName = "Prefix format")]
        public string Format { get; set; }
        [JsonProperty(PropertyName = "GUI Announce command")]
        public string guiCommand { get; set; }
        [JsonProperty(PropertyName = "Show message to admin after command usage")]
        public bool notifyByChatAdminCalls { get; set; }
        [JsonProperty(PropertyName = "Time for active smoke of SupplySignal (default is 210)")]
        public float supplySignalSmokeTime { get; set; }
        [JsonProperty(PropertyName = "Lock DirectDrop to be looted only by target player")]
        public bool lockDirectDrop { get; set; }
        [JsonProperty(PropertyName = "Lock SignalDrop to be looted only by target player")]
        public bool lockSignalDrop { get; set; }
        [JsonProperty(PropertyName = "Unlock crates only after player stopped looting")]
        public bool unlockDropAfterLoot { get; set; }
    }

    public class AirdropOptions
    {
        [JsonProperty(PropertyName = "Default radius for location based massdrop")]
        public float airdropMassdropRadiusDefault { get; set; }
        [JsonProperty(PropertyName = "Delay between Massdrop plane spawns")]
        public float airdropMassdropDelay { get; set; }
        [JsonProperty(PropertyName = "Massdrop default plane amount")]
        public int airdropMassdropDefault { get; set; }
        [JsonProperty(PropertyName = "Multiplier for (plane height * highest point on Map); Default 1.0")]
        public float planeOffSetYMultiply { get; set; }
        [JsonProperty(PropertyName = "Multiplier for overall flight distance; lower means faster at map")]
        public float planeOffSetXMultiply { get; set; }
        [JsonProperty(PropertyName = "Disable SupplySignal randomization")]
        public bool disableRandomSupplyPos { get; set; }
        [JsonProperty(PropertyName = "Visual & Sound Effects Settings")]
        public EffectsOptions EffectsSettings { get; set; }
        [JsonProperty(PropertyName = "Space Delivery Drop Settings")]
        public SpaceOptions SpaceSettings { get; set; }
        public class EffectsOptions
        {
            [JsonProperty(PropertyName = "Use Explosion Sound Effect when hits ground position")]
            public bool useSupplyDropEffectLanded { get; set; }
            [JsonProperty(PropertyName = "Deploy Smoke on drop as it falls")]
            public bool useSmokeEffectOnDrop { get; set; }
            [JsonProperty(PropertyName = "Deploy with Audio Alarm on drop")]
            public bool useSirenAlarmOnDrop { get; set; }
            [JsonProperty(PropertyName = "Deploy with Audio Alarms on drop only during the night")]
            public bool useSirenAlarmAtNightOnly { get; set; }
            [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop")]
            public bool useSirenLightOnDrop { get; set; }
            [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop only during the night")]
            public bool useSirenLightAtNightOnly { get; set; }
            [JsonProperty(PropertyName = "Enable Rocket Signal upon Supply Drop Landing")]
            public bool useSupplyDropRocket { get; set; }
            [JsonProperty(PropertyName = "Signal rocket speed")]
            public float signalRocketSpeed { get; set; }
            [JsonProperty(PropertyName = "Signal rocket explosion timer")]
            public float signalRocketExplosionTime { get; set; }
        }

        public class SpaceOptions
        {
            [JsonProperty(PropertyName = "Incoming Space Delivery Supply Drop velocity")]
            public float dropVelocity { get; set; }
            [JsonProperty(PropertyName = "Parachute deploy distance from ground")]
            public float dropGroundDistance { get; set; }
        }

    }

    public class DamageOptions
    {
        [JsonProperty(PropertyName = "Players can shoot down the drop")]
        public bool shootDownDrops { get; set; }
        [JsonProperty(PropertyName = "Players can shoot down the drop - needed hits")]
        public int shootDownCount { get; set; }
        [JsonProperty(PropertyName = "Set Angular Drag for drop")]
        public float dropAngularDragSetting { get; set; }
        [JsonProperty(PropertyName = "Set Drag for drop (drop resistance)")]
        public float dropDragsetting { get; set; }
        [JsonProperty(PropertyName = "Set drop allow to explode on impact")]
        public bool explodeImpact { get; set; }
        [JsonProperty(PropertyName = "Set drop chance exploding on impact (x out of 100)")]
        public int explodeChance { get; set; }
        [JsonProperty(PropertyName = "Set Mass weight for drop")]
        public float dropMassSetting { get; set; }
    }

    public class NotificationOptions
    {
        [JsonProperty(PropertyName = "Maximum distance in meters to get notified about landed Drop")]
        public float supplyDropNotifyDistance { get; set; }
        [JsonProperty(PropertyName = "Maximum distance in meters to get notified about looted Drop")]
        public float supplyLootNotifyDistance { get; set; }
        [JsonProperty(PropertyName = "Notify a player about incoming Drop to his location")]
        public bool notifyDropPlayer { get; set; }
        [JsonProperty(PropertyName = "Notify a player about spawned Drop at his location")]
        public bool notifyDropDirect { get; set; }
        [JsonProperty(PropertyName = "Notify a player about Space Delivery Drop at his location")]
        public bool notifyDropDirectSpace { get; set; }
        [JsonProperty(PropertyName = "Notify admins per chat about player who has thrown SupplySignal")]
        public bool notifyDropAdminSignal { get; set; }
        [JsonProperty(PropertyName = "Notify console at Drop by SupplySignal")]
        public bool notifyDropConsoleSignal { get; set; }
        [JsonProperty(PropertyName = "Notify console at timed-regular Drop")]
        public bool notifyDropConsoleRegular { get; set; }
        [JsonProperty(PropertyName = "Notify console when a Drop is being looted")]
        public bool notifyDropConsoleLooted { get; set; }
        [JsonProperty(PropertyName = "Notify console when Drop is landed")]
        public bool notifyDropConsoleOnLanded { get; set; }
        [JsonProperty(PropertyName = "Notify console when Drop is spawned")]
        public bool notifyDropConsoleSpawned { get; set; }
        [JsonProperty(PropertyName = "Notify console when Drop landed/spawned only at the first")]
        public bool notifyDropConsoleFirstOnly { get; set; }
        [JsonProperty(PropertyName = "Notify players at custom/event Drop")]
        public bool notifyDropServerCustom { get; set; }
        [JsonProperty(PropertyName = "Notify players at custom/event Drop including Coords")]
        public bool notifyDropServerCustomCoords { get; set; }
        [JsonProperty(PropertyName = "Notify players at custom/event Drop including Grid Area")]
        public bool notifyDropServerCustomGrid { get; set; }
        [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal")]
        public bool notifyDropServerSignal { get; set; }
        [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal including Coords")]
        public bool notifyDropServerSignalCoords { get; set; }
        [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal including Grid Area")]
        public bool notifyDropServerSignalGrid { get; set; }
        [JsonProperty(PropertyName = "Notify players at Massdrop")]
        public bool notifyDropServerMass { get; set; }
        [JsonProperty(PropertyName = "Notify players at Random/Timed Drop")]
        public bool notifyDropServerRegular { get; set; }
        [JsonProperty(PropertyName = "Notify players at Random/Timed Drop including Coords")]
        public bool notifyDropServerRegularCoords { get; set; }
        [JsonProperty(PropertyName = "Notify players at Random/Timed Drop including Grid Area")]
        public bool notifyDropServerRegularGrid { get; set; }
        [JsonProperty(PropertyName = "Notify players when a Drop is being looted")]
        public bool notifyDropServerLooted { get; set; }
        [JsonProperty(PropertyName = "Notify players when a Drop is being looted including coords")]
        public bool notifyDropServerLootedCoords { get; set; }
        [JsonProperty(PropertyName = "Notify players when a Drop is being looted including Grid Area")]
        public bool notifyDropServerLootedGrid { get; set; }
        [JsonProperty(PropertyName = "Notify players when Drop is landed about distance")]
        public bool notifyDropPlayersOnLanded { get; set; }
        [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal")]
        public bool notifyDropSignalByPlayer { get; set; }
        [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal including coords")]
        public bool notifyDropSignalByPlayerCoords { get; set; }
        [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal including Grid Area")]
        public bool notifyDropSignalByPlayerGrid { get; set; }
        [JsonProperty(PropertyName = "Use GUI Announcements for any Drop notification")]
        public bool notifyDropGUI { get; set; }
    }

    public class UIOptions
    {
        [JsonProperty(PropertyName = "SimpleUI_Enable")]
        public bool SimpleUI_Enable { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_FontSize")]
        public int SimpleUI_FontSize { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_HideTimer")]
        public float SimpleUI_HideTimer { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_NoticeColor")]
        public string SimpleUI_NoticeColor { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_ShadowColor")]
        public string SimpleUI_ShadowColor { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_Top")]
        public float SimpleUI_Top { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_Left")]
        public float SimpleUI_Left { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_MaxWidth")]
        public float SimpleUI_MaxWidth { get; set; }
        [JsonProperty(PropertyName = "SimpleUI_MaxHeight")]
        public float SimpleUI_MaxHeight { get; set; }
    }

    public class TimerOptions
    {
        [JsonProperty(PropertyName = "Use Airdrop timer")]
        public bool airdropTimerEnabled { get; set; }
        [JsonProperty(PropertyName = "Used Airdrop timer command")]
        public string airdropTimerCmd { get; set; }
        [JsonProperty(PropertyName = "Minimum players for timed Drop")]
        public int airdropTimerMinPlayers { get; set; }
        [JsonProperty(PropertyName = "Minimum minutes for random timer delay")]
        public int airdropTimerWaitMinutesMin { get; set; }
        [JsonProperty(PropertyName = "Maximum minutes for random timer delay")]
        public int airdropTimerWaitMinutesMax { get; set; }
        [JsonProperty(PropertyName = "Reset Timer after manual random drop")]
        public bool airdropTimerResetAfterRandom { get; set; }
        [JsonProperty(PropertyName = "Remove builtIn Airdrop")]
        public bool airdropRemoveInBuilt { get; set; }
        [JsonProperty(PropertyName = "Cleanup old Drops at serverstart")]
        public bool airdropCleanupAtStart { get; set; }
    }

    public class TimersOptions
    {
        [JsonProperty(PropertyName = "Log to console")]
        public bool logTimersToConsole { get; set; }
        [JsonProperty(PropertyName = "Minimum players for running Timers")]
        public int timersMinPlayers { get; set; }
        [JsonProperty(PropertyName = "Use RealTime")]
        public bool useRealtimeTimers { get; set; }
        [JsonProperty(PropertyName = "RealTime")]
        public Dictionary<string, object> realTimers { get; set; }
        [JsonProperty(PropertyName = "Use ServerTime")]
        public bool useGametimeTimers { get; set; }
        [JsonProperty(PropertyName = "ServerTime")]
        public Dictionary<string, object> serverTimers { get; set; }
    }

    public class DropOptions
    {
        [JsonProperty(PropertyName = "DropDefault")]
        public Dictionary<string, object> setupDropDefault { get; set; }
        [JsonProperty(PropertyName = "DropTypes")]
        public Dictionary<string, object> setupDropTypes { get; set; }
    }

    public class StaticOptions
    {
        [JsonProperty(PropertyName = "DropTypes")]
        public List<LootOptions> LootSettings { get; set; }
        public class LootOptions
        {
            [JsonProperty(PropertyName = "DropType Name")]
            public string DropTypeName { get; set; }
            [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
            public int MinimumItems { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
            public int MaximumItems { get; set; }
            [JsonProperty(PropertyName = "Custom Loot Contents")]
            public List<LootItem> Items { get; set; }
            public class LootItem
            {
                [JsonProperty(PropertyName = "Shortname")]
                public string Name { get; set; }
                [JsonProperty(PropertyName = "Minimum amount of item")]
                public int Minimum { get; set; }
                [JsonProperty(PropertyName = "Maximum amount of item")]
                public int Maximum { get; set; }
                [JsonProperty(PropertyName = "Skin ID")]
                public ulong Skin { get; set; }
                [JsonProperty(PropertyName = "Display Name")]
                public string DisplayName { get; set; }
            }

        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class DebugOptions
{
    [JsonProperty(PropertyName = "Enable Debugging to Console")]
    public bool useDebug { get; set; }
}

public class GenericOptions
{
    [JsonProperty(PropertyName = "Admin messages color")]
    public string colorAdmMsg { get; set; }
    [JsonProperty(PropertyName = "AuthLevel needed for console commands")]
    public int neededAuthLvl { get; set; }
    [JsonProperty(PropertyName = "Broadcast messages color")]
    public string colorTextMsg { get; set; }
    [JsonProperty(PropertyName = "Chat/Message prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Prefix color")]
    public string Color { get; set; }
    [JsonProperty(PropertyName = "Prefix format")]
    public string Format { get; set; }
    [JsonProperty(PropertyName = "GUI Announce command")]
    public string guiCommand { get; set; }
    [JsonProperty(PropertyName = "Show message to admin after command usage")]
    public bool notifyByChatAdminCalls { get; set; }
    [JsonProperty(PropertyName = "Time for active smoke of SupplySignal (default is 210)")]
    public float supplySignalSmokeTime { get; set; }
    [JsonProperty(PropertyName = "Lock DirectDrop to be looted only by target player")]
    public bool lockDirectDrop { get; set; }
    [JsonProperty(PropertyName = "Lock SignalDrop to be looted only by target player")]
    public bool lockSignalDrop { get; set; }
    [JsonProperty(PropertyName = "Unlock crates only after player stopped looting")]
    public bool unlockDropAfterLoot { get; set; }
}

public class AirdropOptions
{
    [JsonProperty(PropertyName = "Default radius for location based massdrop")]
    public float airdropMassdropRadiusDefault { get; set; }
    [JsonProperty(PropertyName = "Delay between Massdrop plane spawns")]
    public float airdropMassdropDelay { get; set; }
    [JsonProperty(PropertyName = "Massdrop default plane amount")]
    public int airdropMassdropDefault { get; set; }
    [JsonProperty(PropertyName = "Multiplier for (plane height * highest point on Map); Default 1.0")]
    public float planeOffSetYMultiply { get; set; }
    [JsonProperty(PropertyName = "Multiplier for overall flight distance; lower means faster at map")]
    public float planeOffSetXMultiply { get; set; }
    [JsonProperty(PropertyName = "Disable SupplySignal randomization")]
    public bool disableRandomSupplyPos { get; set; }
    [JsonProperty(PropertyName = "Visual & Sound Effects Settings")]
    public EffectsOptions EffectsSettings { get; set; }
    [JsonProperty(PropertyName = "Space Delivery Drop Settings")]
    public SpaceOptions SpaceSettings { get; set; }
    public class EffectsOptions
    {
        [JsonProperty(PropertyName = "Use Explosion Sound Effect when hits ground position")]
        public bool useSupplyDropEffectLanded { get; set; }
        [JsonProperty(PropertyName = "Deploy Smoke on drop as it falls")]
        public bool useSmokeEffectOnDrop { get; set; }
        [JsonProperty(PropertyName = "Deploy with Audio Alarm on drop")]
        public bool useSirenAlarmOnDrop { get; set; }
        [JsonProperty(PropertyName = "Deploy with Audio Alarms on drop only during the night")]
        public bool useSirenAlarmAtNightOnly { get; set; }
        [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop")]
        public bool useSirenLightOnDrop { get; set; }
        [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop only during the night")]
        public bool useSirenLightAtNightOnly { get; set; }
        [JsonProperty(PropertyName = "Enable Rocket Signal upon Supply Drop Landing")]
        public bool useSupplyDropRocket { get; set; }
        [JsonProperty(PropertyName = "Signal rocket speed")]
        public float signalRocketSpeed { get; set; }
        [JsonProperty(PropertyName = "Signal rocket explosion timer")]
        public float signalRocketExplosionTime { get; set; }
    }

    public class SpaceOptions
    {
        [JsonProperty(PropertyName = "Incoming Space Delivery Supply Drop velocity")]
        public float dropVelocity { get; set; }
        [JsonProperty(PropertyName = "Parachute deploy distance from ground")]
        public float dropGroundDistance { get; set; }
    }

}

public class EffectsOptions
{
    [JsonProperty(PropertyName = "Use Explosion Sound Effect when hits ground position")]
    public bool useSupplyDropEffectLanded { get; set; }
    [JsonProperty(PropertyName = "Deploy Smoke on drop as it falls")]
    public bool useSmokeEffectOnDrop { get; set; }
    [JsonProperty(PropertyName = "Deploy with Audio Alarm on drop")]
    public bool useSirenAlarmOnDrop { get; set; }
    [JsonProperty(PropertyName = "Deploy with Audio Alarms on drop only during the night")]
    public bool useSirenAlarmAtNightOnly { get; set; }
    [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop")]
    public bool useSirenLightOnDrop { get; set; }
    [JsonProperty(PropertyName = "Deploy with Spinning Siren Light on drop only during the night")]
    public bool useSirenLightAtNightOnly { get; set; }
    [JsonProperty(PropertyName = "Enable Rocket Signal upon Supply Drop Landing")]
    public bool useSupplyDropRocket { get; set; }
    [JsonProperty(PropertyName = "Signal rocket speed")]
    public float signalRocketSpeed { get; set; }
    [JsonProperty(PropertyName = "Signal rocket explosion timer")]
    public float signalRocketExplosionTime { get; set; }
}

public class SpaceOptions
{
    [JsonProperty(PropertyName = "Incoming Space Delivery Supply Drop velocity")]
    public float dropVelocity { get; set; }
    [JsonProperty(PropertyName = "Parachute deploy distance from ground")]
    public float dropGroundDistance { get; set; }
}

public class DamageOptions
{
    [JsonProperty(PropertyName = "Players can shoot down the drop")]
    public bool shootDownDrops { get; set; }
    [JsonProperty(PropertyName = "Players can shoot down the drop - needed hits")]
    public int shootDownCount { get; set; }
    [JsonProperty(PropertyName = "Set Angular Drag for drop")]
    public float dropAngularDragSetting { get; set; }
    [JsonProperty(PropertyName = "Set Drag for drop (drop resistance)")]
    public float dropDragsetting { get; set; }
    [JsonProperty(PropertyName = "Set drop allow to explode on impact")]
    public bool explodeImpact { get; set; }
    [JsonProperty(PropertyName = "Set drop chance exploding on impact (x out of 100)")]
    public int explodeChance { get; set; }
    [JsonProperty(PropertyName = "Set Mass weight for drop")]
    public float dropMassSetting { get; set; }
}

public class NotificationOptions
{
    [JsonProperty(PropertyName = "Maximum distance in meters to get notified about landed Drop")]
    public float supplyDropNotifyDistance { get; set; }
    [JsonProperty(PropertyName = "Maximum distance in meters to get notified about looted Drop")]
    public float supplyLootNotifyDistance { get; set; }
    [JsonProperty(PropertyName = "Notify a player about incoming Drop to his location")]
    public bool notifyDropPlayer { get; set; }
    [JsonProperty(PropertyName = "Notify a player about spawned Drop at his location")]
    public bool notifyDropDirect { get; set; }
    [JsonProperty(PropertyName = "Notify a player about Space Delivery Drop at his location")]
    public bool notifyDropDirectSpace { get; set; }
    [JsonProperty(PropertyName = "Notify admins per chat about player who has thrown SupplySignal")]
    public bool notifyDropAdminSignal { get; set; }
    [JsonProperty(PropertyName = "Notify console at Drop by SupplySignal")]
    public bool notifyDropConsoleSignal { get; set; }
    [JsonProperty(PropertyName = "Notify console at timed-regular Drop")]
    public bool notifyDropConsoleRegular { get; set; }
    [JsonProperty(PropertyName = "Notify console when a Drop is being looted")]
    public bool notifyDropConsoleLooted { get; set; }
    [JsonProperty(PropertyName = "Notify console when Drop is landed")]
    public bool notifyDropConsoleOnLanded { get; set; }
    [JsonProperty(PropertyName = "Notify console when Drop is spawned")]
    public bool notifyDropConsoleSpawned { get; set; }
    [JsonProperty(PropertyName = "Notify console when Drop landed/spawned only at the first")]
    public bool notifyDropConsoleFirstOnly { get; set; }
    [JsonProperty(PropertyName = "Notify players at custom/event Drop")]
    public bool notifyDropServerCustom { get; set; }
    [JsonProperty(PropertyName = "Notify players at custom/event Drop including Coords")]
    public bool notifyDropServerCustomCoords { get; set; }
    [JsonProperty(PropertyName = "Notify players at custom/event Drop including Grid Area")]
    public bool notifyDropServerCustomGrid { get; set; }
    [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal")]
    public bool notifyDropServerSignal { get; set; }
    [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal including Coords")]
    public bool notifyDropServerSignalCoords { get; set; }
    [JsonProperty(PropertyName = "Notify players at Drop by SupplySignal including Grid Area")]
    public bool notifyDropServerSignalGrid { get; set; }
    [JsonProperty(PropertyName = "Notify players at Massdrop")]
    public bool notifyDropServerMass { get; set; }
    [JsonProperty(PropertyName = "Notify players at Random/Timed Drop")]
    public bool notifyDropServerRegular { get; set; }
    [JsonProperty(PropertyName = "Notify players at Random/Timed Drop including Coords")]
    public bool notifyDropServerRegularCoords { get; set; }
    [JsonProperty(PropertyName = "Notify players at Random/Timed Drop including Grid Area")]
    public bool notifyDropServerRegularGrid { get; set; }
    [JsonProperty(PropertyName = "Notify players when a Drop is being looted")]
    public bool notifyDropServerLooted { get; set; }
    [JsonProperty(PropertyName = "Notify players when a Drop is being looted including coords")]
    public bool notifyDropServerLootedCoords { get; set; }
    [JsonProperty(PropertyName = "Notify players when a Drop is being looted including Grid Area")]
    public bool notifyDropServerLootedGrid { get; set; }
    [JsonProperty(PropertyName = "Notify players when Drop is landed about distance")]
    public bool notifyDropPlayersOnLanded { get; set; }
    [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal")]
    public bool notifyDropSignalByPlayer { get; set; }
    [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal including coords")]
    public bool notifyDropSignalByPlayerCoords { get; set; }
    [JsonProperty(PropertyName = "Notify Players who has thrown a SupplySignal including Grid Area")]
    public bool notifyDropSignalByPlayerGrid { get; set; }
    [JsonProperty(PropertyName = "Use GUI Announcements for any Drop notification")]
    public bool notifyDropGUI { get; set; }
}

public class UIOptions
{
    [JsonProperty(PropertyName = "SimpleUI_Enable")]
    public bool SimpleUI_Enable { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_FontSize")]
    public int SimpleUI_FontSize { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_HideTimer")]
    public float SimpleUI_HideTimer { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_NoticeColor")]
    public string SimpleUI_NoticeColor { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_ShadowColor")]
    public string SimpleUI_ShadowColor { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_Top")]
    public float SimpleUI_Top { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_Left")]
    public float SimpleUI_Left { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_MaxWidth")]
    public float SimpleUI_MaxWidth { get; set; }
    [JsonProperty(PropertyName = "SimpleUI_MaxHeight")]
    public float SimpleUI_MaxHeight { get; set; }
}

public class TimerOptions
{
    [JsonProperty(PropertyName = "Use Airdrop timer")]
    public bool airdropTimerEnabled { get; set; }
    [JsonProperty(PropertyName = "Used Airdrop timer command")]
    public string airdropTimerCmd { get; set; }
    [JsonProperty(PropertyName = "Minimum players for timed Drop")]
    public int airdropTimerMinPlayers { get; set; }
    [JsonProperty(PropertyName = "Minimum minutes for random timer delay")]
    public int airdropTimerWaitMinutesMin { get; set; }
    [JsonProperty(PropertyName = "Maximum minutes for random timer delay")]
    public int airdropTimerWaitMinutesMax { get; set; }
    [JsonProperty(PropertyName = "Reset Timer after manual random drop")]
    public bool airdropTimerResetAfterRandom { get; set; }
    [JsonProperty(PropertyName = "Remove builtIn Airdrop")]
    public bool airdropRemoveInBuilt { get; set; }
    [JsonProperty(PropertyName = "Cleanup old Drops at serverstart")]
    public bool airdropCleanupAtStart { get; set; }
}

public class TimersOptions
{
    [JsonProperty(PropertyName = "Log to console")]
    public bool logTimersToConsole { get; set; }
    [JsonProperty(PropertyName = "Minimum players for running Timers")]
    public int timersMinPlayers { get; set; }
    [JsonProperty(PropertyName = "Use RealTime")]
    public bool useRealtimeTimers { get; set; }
    [JsonProperty(PropertyName = "RealTime")]
    public Dictionary<string, object> realTimers { get; set; }
    [JsonProperty(PropertyName = "Use ServerTime")]
    public bool useGametimeTimers { get; set; }
    [JsonProperty(PropertyName = "ServerTime")]
    public Dictionary<string, object> serverTimers { get; set; }
}

public class DropOptions
{
    [JsonProperty(PropertyName = "DropDefault")]
    public Dictionary<string, object> setupDropDefault { get; set; }
    [JsonProperty(PropertyName = "DropTypes")]
    public Dictionary<string, object> setupDropTypes { get; set; }
}

public class StaticOptions
{
    [JsonProperty(PropertyName = "DropTypes")]
    public List<LootOptions> LootSettings { get; set; }
    public class LootOptions
    {
        [JsonProperty(PropertyName = "DropType Name")]
        public string DropTypeName { get; set; }
        [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
        public int MinimumItems { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
        public int MaximumItems { get; set; }
        [JsonProperty(PropertyName = "Custom Loot Contents")]
        public List<LootItem> Items { get; set; }
        public class LootItem
        {
            [JsonProperty(PropertyName = "Shortname")]
            public string Name { get; set; }
            [JsonProperty(PropertyName = "Minimum amount of item")]
            public int Minimum { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of item")]
            public int Maximum { get; set; }
            [JsonProperty(PropertyName = "Skin ID")]
            public ulong Skin { get; set; }
            [JsonProperty(PropertyName = "Display Name")]
            public string DisplayName { get; set; }
        }

    }

}

public class LootOptions
{
    [JsonProperty(PropertyName = "DropType Name")]
    public string DropTypeName { get; set; }
    [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
    public int MinimumItems { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
    public int MaximumItems { get; set; }
    [JsonProperty(PropertyName = "Custom Loot Contents")]
    public List<LootItem> Items { get; set; }
    public class LootItem
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string Name { get; set; }
        [JsonProperty(PropertyName = "Minimum amount of item")]
        public int Minimum { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of item")]
        public int Maximum { get; set; }
        [JsonProperty(PropertyName = "Skin ID")]
        public ulong Skin { get; set; }
        [JsonProperty(PropertyName = "Display Name")]
        public string DisplayName { get; set; }
    }

}

public class LootItem
{
    [JsonProperty(PropertyName = "Shortname")]
    public string Name { get; set; }
    [JsonProperty(PropertyName = "Minimum amount of item")]
    public int Minimum { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of item")]
    public int Maximum { get; set; }
    [JsonProperty(PropertyName = "Skin ID")]
    public ulong Skin { get; set; }
    [JsonProperty(PropertyName = "Display Name")]
    public string DisplayName { get; set; }
}


```

---

## FancyGrenades by PaiN - Adds different types of grenades to the game

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using System.Linq;
using System.IO;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Fancy Grenades", "PaiN", "0.1.3")]
[Description("Adds different types of grenades to the game.")]
 class FancyGrenades : RustPlugin
{
    [PluginReference]
    private Plugin Economics;
    private Plugin ServerRewards;
    const int GrenadeId;
    private Configuration config;
    private ItemDefinition item;
    protected override void LoadDefaultMessages();
    private class Configuration
    {
        [JsonProperty("General Settings")]
        public GeneralSettings settings;
        [JsonProperty("Flashbang Settings")]
        public FlashSettings flash;
        [JsonProperty("Molotov Settings")]
        public MollySettings molly;
        [JsonProperty("Pay Settings")]
        public PaySettings pay;
    }

    public class GeneralSettings
    {
        [JsonProperty("Purchase Flashbang Command")]
        public string flashCmd;
        [JsonProperty("Purchase Molotov Command")]
        public string mollyCmd;
        [JsonProperty("Message SteamID icon")]
        public ulong chatId;
    }

    public class PaySettings
    {
        [JsonProperty("Cost Item (Scrap, ServerRewards, Economics)")]
        public string costItem;
        [JsonProperty("Cost Amount (Disable Cost = 0)")]
        public int costAmount;
    }

    public class MollySettings
    {
        [JsonProperty("Molotov Grenade Name")]
        public string Name;
        [JsonProperty("Molotov SkinId")]
        public ulong SkinId;
    }

    public class FlashSettings
    {
        [JsonProperty("Flashbang Grenade Name")]
        public string Name;
        [JsonProperty("Flashbang SkinId")]
        public ulong SkinId;
        [JsonProperty("Flashbang Range")]
        public int Range;
        [JsonProperty("Flashbang Duration")]
        public float Duration;
        [JsonProperty("Disable flashing players who are not facing the grenade")]
        public bool NoFlashRule;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void Loaded();
     void OnServerInitialized();
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity, ThrownWeapon wep);
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity, ThrownWeapon item);
    [ConsoleCommand("fg.give")]
     void cmdGive(ConsoleSystem.Arg arg);
    [ConsoleCommand("fg.givefree")]
     void cmdGiveFree(ConsoleSystem.Arg arg);
    private void cmdFlash(BasePlayer player, string cmd, string[] args);
    private void cmdMolly(BasePlayer player, string cmd, string[] args);
    private void DoPayAndGive(BasePlayer player, string nadeType, bool free);
     object HasFunds(BasePlayer player, string paymentType);
    private bool GiveItem(BasePlayer player, Item i);
    private bool InNadeAngle(BasePlayer player, BaseEntity ent);
    private int GetItemAmount(BasePlayer player, string shortname);
    private List<BasePlayer> GetPlayersInRadius(Vector3 pos, int radius);
    private void FlashEffect(BasePlayer player);
    private void FireEffect(Vector3 pos);
}

private class Configuration
{
    [JsonProperty("General Settings")]
    public GeneralSettings settings;
    [JsonProperty("Flashbang Settings")]
    public FlashSettings flash;
    [JsonProperty("Molotov Settings")]
    public MollySettings molly;
    [JsonProperty("Pay Settings")]
    public PaySettings pay;
}

public class GeneralSettings
{
    [JsonProperty("Purchase Flashbang Command")]
    public string flashCmd;
    [JsonProperty("Purchase Molotov Command")]
    public string mollyCmd;
    [JsonProperty("Message SteamID icon")]
    public ulong chatId;
}

public class PaySettings
{
    [JsonProperty("Cost Item (Scrap, ServerRewards, Economics)")]
    public string costItem;
    [JsonProperty("Cost Amount (Disable Cost = 0)")]
    public int costAmount;
}

public class MollySettings
{
    [JsonProperty("Molotov Grenade Name")]
    public string Name;
    [JsonProperty("Molotov SkinId")]
    public ulong SkinId;
}

public class FlashSettings
{
    [JsonProperty("Flashbang Grenade Name")]
    public string Name;
    [JsonProperty("Flashbang SkinId")]
    public ulong SkinId;
    [JsonProperty("Flashbang Range")]
    public int Range;
    [JsonProperty("Flashbang Duration")]
    public float Duration;
    [JsonProperty("Disable flashing players who are not facing the grenade")]
    public bool NoFlashRule;
}


```

---

## FarmTools by Clearshot - Farming made easy. Take control of farming with binds.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Farm Tools", "Clearshot", "1.2.2")]
[Description("Farming made easy. Take control of farming with binds.")]
 class FarmTools : CovalencePlugin
{
    private PluginConfig _config;
    private Game.Rust.Libraries.Player _rustPlayer;
    private List<ulong> _usedCommand;
    private Dictionary<ulong, float> _cooldown;
    private int _debrisLayerMask;
    private int _deployedLayerMask;
    private const string PERM_CLONE;
    private const string PERM_CLONE_ALL;
    private const string PERM_HARVEST_ALL;
    private const string PERM_PLANT_ALL;
    private const string PERM_GENES;
    private void SendChatMsg(BasePlayer pl, string msg, string prefix);
    private void Init();
    private void ShowHelp(BasePlayer pl);
    [Command("farmtools")]
    private void FarmToolsCommand(IPlayer player, string command, string[] args);
    [Command("farmtools.clone", "clone")]
    private void CloneCommand(IPlayer player, string command, string[] args);
    [Command("farmtools.cloneall", "cloneall")]
    private void CloneAllCommand(IPlayer player, string command, string[] args);
    [Command("farmtools.harvestall", "harvestall")]
    private void HarvestAllCommand(IPlayer player, string command, string[] args);
    [Command("farmtools.plantall", "plantall")]
    private void PlantAllCommand(IPlayer player, string command, string[] args);
    [Command("farmtools.genes", "genes")]
    private void GenesCommand(IPlayer player, string command, string[] args);
    private void OnUserDisconnected(IPlayer player);
    private void CanTakeCutting(BasePlayer pl, GrowableEntity plant);
    public bool IsPlayerOnCooldown(BasePlayer pl);
    private void GiveGenesNote(BasePlayer pl, Dictionary<string, List<string>> genes, string title);
    private BaseEntity EyeTraceToEntity(BasePlayer pl, float distance, int mask);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadConfig();
    private class PluginConfig
    {
        public string chatIconID;
        public bool printGenesToConsole;
        public float geneCooldown;
    }

}

private class PluginConfig
{
    public string chatIconID;
    public bool printGenesToConsole;
    public float geneCooldown;
}


```

---

## FastLoot by DezLife - Loot crates quickly

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Fast Loot", "DezLife", "1.0.8")]
[Description("Loot crates quickly")]
 class FastLoot : RustPlugin
{
    private const string FastLootLayer;
    private const string perm;
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Color background")]
        public string colorBackground;
        [JsonProperty("Color font")]
        public string colorFont;
        [JsonProperty("Coordinates OffsetMin")]
        public string OffsetMin;
        [JsonProperty("Coordinates OffsetMax")]
        public string OffsetMax;
    }

    private Configuration GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnLootEntity(BasePlayer player, LootContainer loot);
    private void OnLootEntityEnd(BasePlayer player, LootContainer entity);
    private void Init();
     void Unload();
    protected override void LoadDefaultMessages();
    [ConsoleCommand("UI_FastLoot")]
     void FastLootCMD(ConsoleSystem.Arg arg);
    public static bool MoveAllInventoryItems(global::ItemContainer source, global::ItemContainer dest);
    private void UIFastLoot(BasePlayer player);
}

private class Configuration
{
    [JsonProperty("Color background")]
    public string colorBackground;
    [JsonProperty("Color font")]
    public string colorFont;
    [JsonProperty("Coordinates OffsetMin")]
    public string OffsetMin;
    [JsonProperty("Coordinates OffsetMax")]
    public string OffsetMax;
}


```

---

## FauxAdmin by ColonBlow - Allows Authorized players use of admin noclip and have Green chat name.

```csharp
using Network;
using Facepunch;
using System;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("FauxAdmin", "Colon Blow Modified by Dracconus", "2.0.1")]
[Description("FauxAdmin")]
public class FauxAdmin : CovalencePlugin
{
    private const string permAllowed;
    private const string permBypass;
    private const string permBlocked;
    private const string permGodMode;
    private const string permGodBlock;
    private const string permAllowTime;
    private const string permAllowKill;
    private const string permOnlyOwn;
    private const string permTerrain;
    private const string permCanLoot;
    private void Loaded();
    private void OnServerInitialized();
    private static PluginConfig config;
    private class PluginConfig
    {
        public FauxAdminSettings fauxAdminSettings { get; set; }
        public class FauxAdminSettings
        {
            [JsonProperty(PropertyName = "GodMode - Disable God Mode use for all Standard FauxAdmins (except those with fauxadmin.allowgod perms) ? (users must disconnect/reconnect to take effect) ")]
            public bool DisableGodMode { get; set; }
            [JsonProperty(PropertyName = "GodMode - Message player to disconnect/reconnect when fauxadmin.allowgod perms are granted and they are in game ? (users must disconnect/reconnect to take effect) ")]
            public bool MessageOnGodGrant { get; set; }
            [JsonProperty(PropertyName = "GodMode - Force Disconnect of player when fauxadmin.allowgod perms are revoked and they are in game ? (users must disconnect/reconnect to take effect) ")]
            public bool KickOnGodRevoke { get; set; }
            [JsonProperty(PropertyName = "Command - Env.Time - Enable Use of Env.Time Admin command to set in game time, for all FauxAdmins ? (default false) ")]
            public bool EnableEnvTime { get; set; }
            [JsonProperty(PropertyName = "Command - EntKill - Enable Simulated Use of Admin Command (ent kill) for all FauxAdmins ? (default true)  ")]
            public bool EnableEntKill { get; set; }
            [JsonProperty(PropertyName = "Command - EntKill - Only Allow FauxAdmins to entkill their OWN stuff (default true) ? ")]
            public bool EntKillOnlyOwn { get; set; }
            [JsonProperty(PropertyName = "Noclip - Disallow FauxAdmins (unless they have the 'fauxadmin.bypass' permission) from using the Noclip (flying) feature in Tool Cupboard (TC) zones where they are not authorized, which are typically owned by other players. ")]
            public bool DisableNoclipOnNoBuild { get; set; }
            [JsonProperty(PropertyName = "Noclip - Allow the FauxAdmins to use the Noclip (flying) feature exclusively within Authorized Tool Cupboard (TC) Zones, thereby preventing them from flying in all other areas. ? ")]
            public bool EnableNoclipOnBuild { get; set; }
            [JsonProperty(PropertyName = "Antihack - Terrain Kill - Allow FauxAdmin to Fly underground without Antihack killing them (if disabled, FauxAdmin must have allowterrain perm or Godmode to fly underground) ?")]
            public bool EnableUnderGround { get; set; }
            [JsonProperty(PropertyName = "Looting - Disable FauxAdmin ability to loot things (boxes, containers, corpses..etc) ?")]
            public bool DisableLooting { get; set; }
            [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Demolish OTHER players building parts ? ")]
            public bool DisableFauxAdminDemolish { get; set; }
            [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Rotate OTHER players building parts ? ")]
            public bool DisableFauxAdminRotate { get; set; }
            [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Upgrade OTHER players building parts ? ")]
            public bool DisableFauxAdminUpgrade { get; set; }
            [JsonProperty(PropertyName = "Permissions - Grant - Message Online Player when Fauxadmin.allowed perms are granted (perms only take effect when disconnect/connect) ? ")]
            public bool MessageOnFauxAdminGrant { get; set; }
            [JsonProperty(PropertyName = "Permissions - Revoke - Disconnect Online Player when Fauxadmin.allowed perms are revoked (perms only take effect when disconnect/connect) ?")]
            public bool KickOnFauxAdminRevoke { get; set; }
            [JsonProperty(PropertyName = "Player Flag - Backend - Use (isAdmin) Instead of (isDeveloper) for FauxAdmin magic (default is dev, better compatibility with other plugins) ? ")]
            public bool UseAdminFlag { get; set; }
        }

        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    [Command("fly")]
    private void cmdfly(IPlayer iplayer, string command, string[] args);
    [Command("noclip")]
    private void cmdNoClip(IPlayer iplayer, string command, string[] args);
    [Command("entkill")]
    private void cmdEntKill(IPlayer iplayer, string command, string[] args);
    private object OnClientCommand(Connection connection, string command);
    private object CanLootEntity(BasePlayer player, BaseEntity container);
    private void OnPlayerConnected(BasePlayer player);
    private object CanUpdateSign(BasePlayer player, Signage sign);
    private object OnStructureDemolish(BuildingBlock block, BasePlayer player);
    private object OnStructureRotate(BuildingBlock block, BasePlayer player);
    private object OnStructureUpgrade(BuildingBlock block, BasePlayer player);
    private void OnUserPermissionGranted(string id, string permName);
    private void OnUserPermissionRevoked(string id, string permName);
    private object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
    private void Unload();
    private void EntKillProcess(BasePlayer player);
    private IEnumerator ProcessFuaxAdminControls();
    private void ProcessPlayer(BasePlayer player);
    private static void DestroyAll();
    private class FauxAdminControl : FacepunchBehaviour
    {
        private FauxAdmin instance;
        private BasePlayer player;
        private bool hasFrozenPosition;
        public void Initialize(FauxAdmin pluginInstance);
        private void Awake();
        private void DeactivateNoClip(BasePlayer player);
        private IEnumerator DeactivateAndFreeze(BasePlayer player);
        private bool PlayerIsInOwnTCRange();
        private void FixedUpdate();
        public void OnDestroy();
    }

    private void ToggleNoclip(BasePlayer player);
}

private class PluginConfig
{
    public FauxAdminSettings fauxAdminSettings { get; set; }
    public class FauxAdminSettings
    {
        [JsonProperty(PropertyName = "GodMode - Disable God Mode use for all Standard FauxAdmins (except those with fauxadmin.allowgod perms) ? (users must disconnect/reconnect to take effect) ")]
        public bool DisableGodMode { get; set; }
        [JsonProperty(PropertyName = "GodMode - Message player to disconnect/reconnect when fauxadmin.allowgod perms are granted and they are in game ? (users must disconnect/reconnect to take effect) ")]
        public bool MessageOnGodGrant { get; set; }
        [JsonProperty(PropertyName = "GodMode - Force Disconnect of player when fauxadmin.allowgod perms are revoked and they are in game ? (users must disconnect/reconnect to take effect) ")]
        public bool KickOnGodRevoke { get; set; }
        [JsonProperty(PropertyName = "Command - Env.Time - Enable Use of Env.Time Admin command to set in game time, for all FauxAdmins ? (default false) ")]
        public bool EnableEnvTime { get; set; }
        [JsonProperty(PropertyName = "Command - EntKill - Enable Simulated Use of Admin Command (ent kill) for all FauxAdmins ? (default true)  ")]
        public bool EnableEntKill { get; set; }
        [JsonProperty(PropertyName = "Command - EntKill - Only Allow FauxAdmins to entkill their OWN stuff (default true) ? ")]
        public bool EntKillOnlyOwn { get; set; }
        [JsonProperty(PropertyName = "Noclip - Disallow FauxAdmins (unless they have the 'fauxadmin.bypass' permission) from using the Noclip (flying) feature in Tool Cupboard (TC) zones where they are not authorized, which are typically owned by other players. ")]
        public bool DisableNoclipOnNoBuild { get; set; }
        [JsonProperty(PropertyName = "Noclip - Allow the FauxAdmins to use the Noclip (flying) feature exclusively within Authorized Tool Cupboard (TC) Zones, thereby preventing them from flying in all other areas. ? ")]
        public bool EnableNoclipOnBuild { get; set; }
        [JsonProperty(PropertyName = "Antihack - Terrain Kill - Allow FauxAdmin to Fly underground without Antihack killing them (if disabled, FauxAdmin must have allowterrain perm or Godmode to fly underground) ?")]
        public bool EnableUnderGround { get; set; }
        [JsonProperty(PropertyName = "Looting - Disable FauxAdmin ability to loot things (boxes, containers, corpses..etc) ?")]
        public bool DisableLooting { get; set; }
        [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Demolish OTHER players building parts ? ")]
        public bool DisableFauxAdminDemolish { get; set; }
        [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Rotate OTHER players building parts ? ")]
        public bool DisableFauxAdminRotate { get; set; }
        [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Upgrade OTHER players building parts ? ")]
        public bool DisableFauxAdminUpgrade { get; set; }
        [JsonProperty(PropertyName = "Permissions - Grant - Message Online Player when Fauxadmin.allowed perms are granted (perms only take effect when disconnect/connect) ? ")]
        public bool MessageOnFauxAdminGrant { get; set; }
        [JsonProperty(PropertyName = "Permissions - Revoke - Disconnect Online Player when Fauxadmin.allowed perms are revoked (perms only take effect when disconnect/connect) ?")]
        public bool KickOnFauxAdminRevoke { get; set; }
        [JsonProperty(PropertyName = "Player Flag - Backend - Use (isAdmin) Instead of (isDeveloper) for FauxAdmin magic (default is dev, better compatibility with other plugins) ? ")]
        public bool UseAdminFlag { get; set; }
    }

    public static PluginConfig DefaultConfig();
}

public class FauxAdminSettings
{
    [JsonProperty(PropertyName = "GodMode - Disable God Mode use for all Standard FauxAdmins (except those with fauxadmin.allowgod perms) ? (users must disconnect/reconnect to take effect) ")]
    public bool DisableGodMode { get; set; }
    [JsonProperty(PropertyName = "GodMode - Message player to disconnect/reconnect when fauxadmin.allowgod perms are granted and they are in game ? (users must disconnect/reconnect to take effect) ")]
    public bool MessageOnGodGrant { get; set; }
    [JsonProperty(PropertyName = "GodMode - Force Disconnect of player when fauxadmin.allowgod perms are revoked and they are in game ? (users must disconnect/reconnect to take effect) ")]
    public bool KickOnGodRevoke { get; set; }
    [JsonProperty(PropertyName = "Command - Env.Time - Enable Use of Env.Time Admin command to set in game time, for all FauxAdmins ? (default false) ")]
    public bool EnableEnvTime { get; set; }
    [JsonProperty(PropertyName = "Command - EntKill - Enable Simulated Use of Admin Command (ent kill) for all FauxAdmins ? (default true)  ")]
    public bool EnableEntKill { get; set; }
    [JsonProperty(PropertyName = "Command - EntKill - Only Allow FauxAdmins to entkill their OWN stuff (default true) ? ")]
    public bool EntKillOnlyOwn { get; set; }
    [JsonProperty(PropertyName = "Noclip - Disallow FauxAdmins (unless they have the 'fauxadmin.bypass' permission) from using the Noclip (flying) feature in Tool Cupboard (TC) zones where they are not authorized, which are typically owned by other players. ")]
    public bool DisableNoclipOnNoBuild { get; set; }
    [JsonProperty(PropertyName = "Noclip - Allow the FauxAdmins to use the Noclip (flying) feature exclusively within Authorized Tool Cupboard (TC) Zones, thereby preventing them from flying in all other areas. ? ")]
    public bool EnableNoclipOnBuild { get; set; }
    [JsonProperty(PropertyName = "Antihack - Terrain Kill - Allow FauxAdmin to Fly underground without Antihack killing them (if disabled, FauxAdmin must have allowterrain perm or Godmode to fly underground) ?")]
    public bool EnableUnderGround { get; set; }
    [JsonProperty(PropertyName = "Looting - Disable FauxAdmin ability to loot things (boxes, containers, corpses..etc) ?")]
    public bool DisableLooting { get; set; }
    [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Demolish OTHER players building parts ? ")]
    public bool DisableFauxAdminDemolish { get; set; }
    [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Rotate OTHER players building parts ? ")]
    public bool DisableFauxAdminRotate { get; set; }
    [JsonProperty(PropertyName = "Construction - Disable FauxAdmin Ability to Upgrade OTHER players building parts ? ")]
    public bool DisableFauxAdminUpgrade { get; set; }
    [JsonProperty(PropertyName = "Permissions - Grant - Message Online Player when Fauxadmin.allowed perms are granted (perms only take effect when disconnect/connect) ? ")]
    public bool MessageOnFauxAdminGrant { get; set; }
    [JsonProperty(PropertyName = "Permissions - Revoke - Disconnect Online Player when Fauxadmin.allowed perms are revoked (perms only take effect when disconnect/connect) ?")]
    public bool KickOnFauxAdminRevoke { get; set; }
    [JsonProperty(PropertyName = "Player Flag - Backend - Use (isAdmin) Instead of (isDeveloper) for FauxAdmin magic (default is dev, better compatibility with other plugins) ? ")]
    public bool UseAdminFlag { get; set; }
}

private class FauxAdminControl : FacepunchBehaviour
{
    private FauxAdmin instance;
    private BasePlayer player;
    private bool hasFrozenPosition;
    public void Initialize(FauxAdmin pluginInstance);
    private void Awake();
    private void DeactivateNoClip(BasePlayer player);
    private IEnumerator DeactivateAndFreeze(BasePlayer player);
    private bool PlayerIsInOwnTCRange();
    private void FixedUpdate();
    public void OnDestroy();
}


```

---

## FauxClip by ColonBlow - NoClip Simulator for players with permission

```csharp
using System;
using System.Reflection;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("FauxClip", "Colon Blow", "1.3.10")]
 class FauxClip : RustPlugin
{
    public float GracefulLandingTime { get; set; }
    public float BaseNoClipSpeed { get; set; }
    public float SprintNoClipSpeed { get; set; }
    public float TurboNoClipSpeed { get; set; }
    public bool UseFauxClipGodMode { get; set; }
    protected override void LoadDefaultConfig();
     class PlayerData
    {
        public BasePlayer player;
        public float speed;
        public Vector3 oldPos;
        public InputState input;
    }

     class LandingData
    {
        public BasePlayer player;
    }

    private readonly Dictionary<ulong, PlayerData> _noclip;
    private readonly Dictionary<ulong, LandingData> _landing;
    private static readonly FieldInfo ServerInput;
    public static FieldInfo lastPositionValue;
     void Loaded();
     void OnFrame();
     bool NoAdmin(BasePlayer player);
     bool NoBuild(BasePlayer player);
     void DamageOn(BasePlayer player);
     void DamageOff(BasePlayer player);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void Deactivatenoclip(BasePlayer player);
     void Restrictedairspace(BasePlayer player);
     void Activatenoclip(BasePlayer player, float speed);
     void Togglenoclip(BasePlayer player, float speed);
     void LandingCycleDone(BasePlayer player);
    [ChatCommand("noclip")]
     void cmdChatnolcip(BasePlayer player, string command, string[] args);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     bool IsAllowed(BasePlayer player, string perm);
}

 class PlayerData
{
    public BasePlayer player;
    public float speed;
    public Vector3 oldPos;
    public InputState input;
}

 class LandingData
{
    public BasePlayer player;
}


```

---

## FeedFood by lorddy - Make players you're looking at consume food you drop.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Feed Food", "Lorddy", "0.0.1")]
[Description("Feeds dropped food to players you're looking at")]
public class FeedFood : RustPlugin
{
    protected override void LoadDefaultMessages();
    private const string PERMISSION_FEEDER;
    private const string PERMISSION_AFFECT;
    private void Init();
     void OnItemDropped(Item item, BaseEntity entity);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}


```

---

## Fetch by  - Downloads JSON file from URI and store in a data subfolder

```csharp
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Fetch", "CatMeat", "1.0.2")]
[Description("Download JSON file from URI and store in a DATA subfolder")]
 class Fetch : CovalencePlugin
{
    private const string FetchUse;
    private const string FetchOverwrite;
    private PluginConfig config;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string langKey, string playerId, object[] args);
    private class PluginConfig
    {
        public bool AllowOverwrite;
        public string SubFolder;
    }

    private PluginConfig GetDefaultConfig();
    [Command("fetch")]
    private void GetRequest(IPlayer player, string command, string[] args);
    private void GetCallback(int code, string response, string uri, string savefilename, IPlayer player);
}

private class PluginConfig
{
    public bool AllowOverwrite;
    public string SubFolder;
}


```

---

## FileIndexer by  - Keep track of your plugin data files

```csharp
using System;
using System.IO;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("FileIndexer", "ImagicTheCat", "1.0.0")]
public class FileIndexer : CovalencePlugin
{
    public class Index
    {
        public Dictionary<string, Index> idxs;
        public Index();
        public void IndexFile(List<string> path, int shift);
        public Index GetIndex(List<string> path, int shift);
        public bool IsFile();
        public List<string> List(Search search);
        private void _List(Search search, List<string> outpaths, string path);
        public void Cleanup(string path);
    }

    public class Indexer
    {
        public Index root;
        private string filepath;
        public Indexer(string filepath);
        public void IndexFile(string filepath, bool save);
        public List<string> List(string path, Search search);
        public void Load();
        public void Save();
        public void Cleanup(string rootpath);
        public static List<string> ParsePath(string path, bool is_file);
    }

}

public class Index
{
    public Dictionary<string, Index> idxs;
    public Index();
    public void IndexFile(List<string> path, int shift);
    public Index GetIndex(List<string> path, int shift);
    public bool IsFile();
    public List<string> List(Search search);
    private void _List(Search search, List<string> outpaths, string path);
    public void Cleanup(string path);
}

public class Indexer
{
    public Index root;
    private string filepath;
    public Indexer(string filepath);
    public void IndexFile(string filepath, bool save);
    public List<string> List(string path, Search search);
    public void Load();
    public void Save();
    public void Cleanup(string rootpath);
    public static List<string> ParsePath(string path, bool is_file);
}


```

---

## Finder by MONaH - Find players, sleepers, sleeping bags, buildings, and teleport to them

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Finder", "MON@H", "3.1.1")]
[Description("Find Players, SleepingBags, Doors, Sleepers, and teleport to them")]
public class Finder : CovalencePlugin
{
    [PluginReference]
    private Plugin PlayerDatabase;
    private Dictionary<ulong, PlayerFinder> cachedFinder;
    private const string PERMISSION_FIND;
    private const string PERMISSION_TP;
    private void Init();
    private void OnServerInitialized();
    private void UpdateConfig();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalSettings globalS;
        [JsonProperty(PropertyName = "Chat settings")]
        public ChatSettings chatS;
        public class GlobalSettings
        {
            [JsonProperty(PropertyName = "Use permissions")]
            public bool usePermission;
            [JsonProperty(PropertyName = "Allow admins to use without permission")]
            public bool adminsAllowed;
        }

        public class ChatSettings
        {
            [JsonProperty(PropertyName = "Chat command")]
            public string[] commands;
            [JsonProperty(PropertyName = "Chat prefix")]
            public string prefix;
            [JsonProperty(PropertyName = "Chat steamID icon")]
            public ulong steamIDIcon;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void CmdFind(IPlayer iplayer, string command, string[] args);
     bool hasPermission(IPlayer player, string perm);
     class FindData
    {
         string Name;
        public Vector3 Pos;
         string TypeName;
        public FindData(string TypeName, Vector3 Pos, string Name);
        public override string ToString();
    }

     class PlayerFinder
    {
         string Name;
         string Id;
         bool Online;
        public List<FindData> Data;
        public PlayerFinder(string Name, string Id, bool Online);
        public void AddFind(string TypeName, Vector3 Pos, string Name);
        public override string ToString();
    }

     PlayerFinder GetPlayerInfo(ulong userID);
    private object FindPosition(ulong userID);
    private object FindPlayerID(string arg, BasePlayer source);
    private void Print(IPlayer player, string message);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalSettings globalS;
    [JsonProperty(PropertyName = "Chat settings")]
    public ChatSettings chatS;
    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Use permissions")]
        public bool usePermission;
        [JsonProperty(PropertyName = "Allow admins to use without permission")]
        public bool adminsAllowed;
    }

    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat command")]
        public string[] commands;
        [JsonProperty(PropertyName = "Chat prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat steamID icon")]
        public ulong steamIDIcon;
    }

}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Use permissions")]
    public bool usePermission;
    [JsonProperty(PropertyName = "Allow admins to use without permission")]
    public bool adminsAllowed;
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Chat command")]
    public string[] commands;
    [JsonProperty(PropertyName = "Chat prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat steamID icon")]
    public ulong steamIDIcon;
}

 class FindData
{
     string Name;
    public Vector3 Pos;
     string TypeName;
    public FindData(string TypeName, Vector3 Pos, string Name);
    public override string ToString();
}

 class PlayerFinder
{
     string Name;
     string Id;
     bool Online;
    public List<FindData> Data;
    public PlayerFinder(string Name, string Id, bool Online);
    public void AddFind(string TypeName, Vector3 Pos, string Name);
    public override string ToString();
}


```

---

## FireArrows by ColonBlow - Players can shoot arrows with fire

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("FireArrows", "Colon Blow", "1.2.11")]
 class FireArrows : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     bool Changed;
     Dictionary<ulong, FireArrowData> FireArrowOn;
     Dictionary<ulong, FireBallData> FireBallOn;
     Dictionary<ulong, FireBombData> FireBombOn;
     Dictionary<ulong, FireArrowCooldown> Cooldown;
     Dictionary<ulong, string> GuiInfoFA;
     class FireArrowCooldown
    {
        public BasePlayer player;
    }

     class FireArrowData
    {
        public BasePlayer player;
    }

     class FireBombData
    {
        public BasePlayer player;
    }

     class FireBallData
    {
        public BasePlayer player;
    }

     void Loaded();
     void Unload();
     void LoadDefaultConfig();
    static bool ShowArrowTypeIcon;
    static bool BlockinRestrictedZone;
    static bool UseProt;
    static bool UseBasicArrowsOnly;
    static float DamageFireArrow;
    static float DamageFireBall;
    static float DamageFireBomb;
    static float DamageRadius;
    static float DurationFireArrow;
    static float DurationFireBallArrow;
    static float DurationFireBombArrow;
    static float UsageCooldown;
    static int lowGradeItemID;
    static int clothItemID;
    static int oilItemID;
    static int explosivesItemID;
    static string RestrictedZoneID;
    static int cloth;
    static int fuel;
    static int oil;
    static int explosives;
    private string IconFireArrow;
    private string IconFireBall;
    private string IconFireBomb;
     bool isRestricted;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     Dictionary<string, string> messagesFA;
     void OnPlayerAttack(BasePlayer player, HitInfo hitInfo);
     void ArrowFX(BasePlayer player, HitInfo hitInfo);
     void ArrowCooldownControl(BasePlayer player);
     void ArrowCooldownToggle(BasePlayer player);
     void FireArrowFX(BasePlayer player, HitInfo hitInfo);
     void FireBallFX(BasePlayer player, HitInfo hitInfo);
     void FireBombFX(BasePlayer player, HitInfo hitInfo);
     void applyBlastDamage(BasePlayer player, float damageamount, Rust.DamageType damagetype, HitInfo hitInfo);
     void playerBlastDamage(BasePlayer player, float damageamount, Rust.DamageType damagetype, HitInfo hitInfo);
     void OnPlayerInput(BasePlayer player, InputState input);
    [ChatCommand("firearrow")]
     void cmdChatfirearrow(BasePlayer player, string command, string[] args);
    [ConsoleCommand("firearrow")]
     void cmdConsolefirearrow(ConsoleSystem.Arg arg);
     void ArrowCui(BasePlayer player);
     void ArrowGui(BasePlayer player);
     void ToggleArrowType(BasePlayer player);
     void NormalArrowToggle(BasePlayer player);
     void FireArrowToggle(BasePlayer player);
     void FireBallToggle(BasePlayer player);
     void FireBombToggle(BasePlayer player);
     bool hasResources(BasePlayer player);
     bool notZoneRestricted(BasePlayer player);
     void tellNotGrantedArrow(BasePlayer player);
     void tellDoesNotHaveMaterials(BasePlayer player);
     void tellRestricted(BasePlayer player);
     bool IsAllowed(BasePlayer player, string perm);
     bool usingCorrectWeapon(BasePlayer player);
     void DestroyCui(BasePlayer player);
     void DestroyArrowData(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
}

 class FireArrowCooldown
{
    public BasePlayer player;
}

 class FireArrowData
{
    public BasePlayer player;
}

 class FireBombData
{
    public BasePlayer player;
}

 class FireBallData
{
    public BasePlayer player;
}


```

---

## FireWorkLogging by mrcameron999 - Creates and logs players pattern fireworks that are fired to a Discord channel

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Firework Logging", "mrcameron999", "1.0.2")]
[Description("Logs pattern fireworks to a discord bot")]
public class FireWorkLogging : CovalencePlugin
{
    private string connection;
    private float timeout;
    private string serverName;
    private Dictionary<string, string> headers;
    private void Init();
    private void LoadConfigData();
    protected override void LoadDefaultConfig();
     object OnEntityFlagsNetworkUpdate(PatternFirework entity);
    public class FireworkPoint
    {
        public float x { get; set; }
        public float y { get; set; }
        public int colora { get; set; }
        public int colorr { get; set; }
        public int colorg { get; set; }
        public int colorb { get; set; }
    }

}

public class FireworkPoint
{
    public float x { get; set; }
    public float y { get; set; }
    public int colora { get; set; }
    public int colorr { get; set; }
    public int colorg { get; set; }
    public int colorb { get; set; }
}


```

---

## FireworksMonitor by MrBlue - Sends a message to Discord with images of the firework patterns set by players

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info( "Fireworks Monitor", "hoppel & Mr. Blue", "1.0.1" )]
[Description( "Send a message to discord with an image of the firework pattern set by the user" )]
public class FireworksMonitor : RustPlugin
{
    private const string WEBHOOK_INTRO;
    private static int? _embedColor;
    private class PluginConfiguration
    {
        [JsonProperty( "Discord Webhook" )]
        public string DiscordWebhook;
        [JsonProperty( "Embed Color" )]
        public string EmbedColor;
    }

    private PluginConfiguration _config;
    private void Init();
    protected override void LoadDefaultConfig();
    private PluginConfiguration GetDefaultConfig();
    protected override void LoadDefaultMessages();
    private string FormatMessage(string key, BasePlayer player, BasePlayer fireworkOwner);
    private static readonly Texture2D DrawingTexture;
    private void OnFireworkDesignChanged(PatternFirework entity, ProtoBuf.PatternFirework.Design design, BasePlayer player);
    private void SendDiscordEmbed(byte[] image, BasePlayer player, ulong fireworkOwnerId);
    private IEnumerator HandleUpload(string url, WWWForm data);
    private static int? FromHex(string value);
}

private class PluginConfiguration
{
    [JsonProperty( "Discord Webhook" )]
    public string DiscordWebhook;
    [JsonProperty( "Embed Color" )]
    public string EmbedColor;
}


```

---

## FishingMultiplier by Malmo - Multiplies the amount of fish caught with rod or traps

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Fishing Multiplier", "Malmo", "1.0.3")]
[Description("Multiplies the amount of fish caught with rod or traps")]
 class FishingMultiplier : RustPlugin
{
    private readonly Hash<ulong, int> _trapMultipliers;
    private void Init();
    private void OnFishCatch(Item item, BaseFishingRod rod, BasePlayer player);
    private int GetMultiplier(Dictionary<string, int> dict, string shortname);
    private int GetMultiplier(string shortname, CatchTypes type);
    private ConfigData _config;
     class ConfigData
    {
        [JsonProperty("Multipliers")]
        public MultiplierConfig Multipliers { get; set; }
    }

     class MultiplierConfig
    {
        [JsonProperty("Fishing Rod Multiplier")]
        public Dictionary<string, int> RodMultipler { get; set; }
        [JsonProperty("Fish Traps Multiplier")]
        public Dictionary<string, int> FishTrapsMultipler { get; set; }
    }

    protected override void LoadDefaultConfig();
    private ConfigData GetDefaultConfig();
}

 class ConfigData
{
    [JsonProperty("Multipliers")]
    public MultiplierConfig Multipliers { get; set; }
}

 class MultiplierConfig
{
    [JsonProperty("Fishing Rod Multiplier")]
    public Dictionary<string, int> RodMultipler { get; set; }
    [JsonProperty("Fish Traps Multiplier")]
    public Dictionary<string, int> FishTrapsMultipler { get; set; }
}


```

---

## FixedHeadshotSounds by Waggy - Fixes headshots on animals and in melee PvP not making that satisfying crunch

```csharp
using UnityEngine;

Oxide.Plugins
[Info( "Fixed Headshot Sounds", "Waggy", "1.0.1" )]
[Description( "Fixes headshot sounds not playing for melee PvP and for animals" )]
 class FixedHeadshotSounds : RustPlugin
{
    public static Effect headshotEffect;
    public void CreateHeadshotEffect(BasePlayer player);
    public void SendHeadshotEffect(BasePlayer player);
    public void CheckHeadshot(HitInfo info);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}


```

---

## FixFireworks by Clearshot - Fix lit fireworks never firing

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Fix Fireworks", "Clearshot", "1.1.0")]
[Description("Fix lit fireworks never firing")]
 class FixFireworks : CovalencePlugin
{
    private PluginConfig _config;
    private HashSet<BaseFirework> _fireworkList;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseFirework firework);
    private void OnEntityKill(BaseFirework firework);
    private void FixFirework(BaseFirework firework);
    [Command("fixfireworks")]
    private void FixFireworksCommand(IPlayer player, string command, string[] args);
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadConfig();
    private class PluginConfig
    {
        public float checkFireworksInterval;
    }

}

private class PluginConfig
{
    public float checkFireworksInterval;
}


```

---

## FixShopFront by Mevent - Fixes a bug with managing other people's weapon modifications when exchanging

```csharp

Oxide.Plugins
[Info("Fix Shop Front", "Mevent", "1.0.2")]
[Description("Fixed a bug with managing other people's weapon modifications when exchanging")]
public class FixShopFront : CovalencePlugin
{
    private object OnItemAction(Item item, string action, BasePlayer player);
    private object CanMoveItem(Item item, PlayerInventory inventory, uint targetContainer, int targetSlot, int amount);
}


```

---

## Flashbang by birthdates - Throw a flashbang to temporarily blind your enemies

```csharp
using System;
using System.Collections.Generic;
using Network;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Flashbang", "birthdates", "1.0.5")]
[Description("Throw a flashbang to temporarily blind your enemies")]
public class Flashbang : RustPlugin
{
    [ConsoleCommand("flashbang")]
    private void FlashbangCommand(ConsoleSystem.Arg arg);
    private int PlayerMask { get; set; }
    private int ObstructionMask { get; set; }
    private CuiElementContainer FlashCui { get; set; }
    private CuiElementContainer SmallFlashCui { get; set; }
    private IDictionary<ulong, long> FlashExpiry { get; set; }
    private const string CuiName;
    private const string GivePermission;
    private void Init();
    private void OnServerInitialized();
    private object OnMaxStackable(Item item);
    private void OnExplosiveThrown(BasePlayer player, TimedExplosive entity, ThrownWeapon item);
    private void OnExplosiveDropped(BasePlayer player, TimedExplosive entity, ThrownWeapon item);
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void GenerateUI(CuiElementContainer container, string color);
    private void CheckForExpired();
    [HookMethod("IsFlashbang")]
    private bool IsFlashbang(Item item);
    [HookMethod("IsFlashed")]
    private bool IsFlashed(BasePlayer player);
    [HookMethod("IsFlashed")]
    private bool IsFlashed(ulong id);
    private void OnThrown(BasePlayer player, TimedExplosive entity, ThrownWeapon weapon);
    [HookMethod("Flash")]
    private void Flash(BasePlayer player, Component flash);
    private void Flash(BasePlayer player, bool small);
    [HookMethod("UnFlash")]
    private void UnFlash(BasePlayer player);
    private bool IsObstructed(BasePlayer basePlayer, BaseEntity flash);
    [HookMethod("Flash")]
    private void Flash(BaseEntity flash);
    private static void PlayPrefab(string prefab, Vector3 position, Vector3 direction, Connection source);
    private static float AngleTo(BasePlayer player, Component target);
    private ConfigFile _config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class ConfigFile
    {
        [JsonProperty("Smoke Skin ID")]
        public ulong SkinID { get; set; }
        [JsonProperty("Flashbang Deploy Time (seconds)")]
        public float DeployTime { get; set; }
        [JsonProperty("Flashbang Radius (metres)")]
        public float Radius { get; set; }
        [JsonProperty("Flashbang Velocity Multiplier")]
        public float VelocityMultiplier { get; set; }
        [JsonProperty("Flashbang Blind Time (seconds)")]
        public float BlindTime { get; set; }
        [JsonProperty("Flashbang Small Blind Time (seconds)")]
        public float SmallBlindTime { get; set; }
        [JsonProperty("Flashbang Small Angle (degrees)")]
        public float SmallAngle { get; set; }
        [JsonProperty("Ignore Angle (degrees)")]
        public float IgnoreAngle { get; set; }
        [JsonProperty("Flashbang Fadeout Time (seconds)")]
        public float FadeOut { get; set; }
        [JsonProperty("Flashbang Stack")]
        public int Stack { get; set; }
        [JsonProperty("Flashbang Deploy Prefab (leave blank for disable)")]
        public string DeployPrefab { get; set; }
        [JsonProperty("Scream Prefab (leave blank for disable)")]
        public string ScreamPrefab { get; set; }
        [JsonProperty("Smoke Prefab (leave blank for disable)")]
        public string SmokePrefab { get; set; }
        public static ConfigFile DefaultConfig();
    }

}

private class ConfigFile
{
    [JsonProperty("Smoke Skin ID")]
    public ulong SkinID { get; set; }
    [JsonProperty("Flashbang Deploy Time (seconds)")]
    public float DeployTime { get; set; }
    [JsonProperty("Flashbang Radius (metres)")]
    public float Radius { get; set; }
    [JsonProperty("Flashbang Velocity Multiplier")]
    public float VelocityMultiplier { get; set; }
    [JsonProperty("Flashbang Blind Time (seconds)")]
    public float BlindTime { get; set; }
    [JsonProperty("Flashbang Small Blind Time (seconds)")]
    public float SmallBlindTime { get; set; }
    [JsonProperty("Flashbang Small Angle (degrees)")]
    public float SmallAngle { get; set; }
    [JsonProperty("Ignore Angle (degrees)")]
    public float IgnoreAngle { get; set; }
    [JsonProperty("Flashbang Fadeout Time (seconds)")]
    public float FadeOut { get; set; }
    [JsonProperty("Flashbang Stack")]
    public int Stack { get; set; }
    [JsonProperty("Flashbang Deploy Prefab (leave blank for disable)")]
    public string DeployPrefab { get; set; }
    [JsonProperty("Scream Prefab (leave blank for disable)")]
    public string ScreamPrefab { get; set; }
    [JsonProperty("Smoke Prefab (leave blank for disable)")]
    public string SmokePrefab { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## FlippableTurrets by Razor - Allows players to place turret on roof

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Flippable Turrets", "Orange", "1.1.0")]
[Description("Allows users to place turrets how they like")]
public class FlippableTurrets : RustPlugin
{
    private const string prefab;
    private const string permUse;
    private DynamicConfigFile configFile;
    private Configuration config;
    private class Configuration
    {
        public HashSet<ulong> IgnoredSkinIDs;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private string GetMessage(string key, BasePlayer player);
    private void Init();
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void CheckInput(BasePlayer player, InputState input);
}

private class Configuration
{
    public HashSet<ulong> IgnoredSkinIDs;
}


```

---

## FloatingItems by Diametric - Allows configuring the buoyancy of dropped items

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using System;
using Oxide.Core.Libraries;
using UnityEngine;

Oxide.Plugins
[Info("FloatingItems", "Diametric", "1.0.1")]
[Description("Adds buoyancy to items dropped, causing them to float in water")]
 class FloatingItems : RustPlugin
{
     class ItemFloater : MonoBehaviour
    {
        public BaseEntity entity;
        public float buoyancyScale;
        public int waterDetectionRate;
        private void AddBuoyancyComponent();
         void FixedUpdate();
    }

    private void AddFloaterComponent(BaseEntity entity, float buoyancyScale);
     void OnItemDropped(Item item, BaseEntity entity);
    private class FloatingItemsConfig
    {
        public int waterDetectionRate;
        public float globalBuoyancy;
        public Dictionary<string, object> ItemBuoyancy;
        public Dictionary<string, object> CategoryBuoyancy;
        private FloatingItems plugin;
        public FloatingItemsConfig(FloatingItems plugin);
        private void GetConfig(T variable, string[] path);
        private void SetConfig(T variable, string[] path);
    }

    protected override void LoadDefaultConfig();
    private FloatingItemsConfig config;
    private void Init();
}

 class ItemFloater : MonoBehaviour
{
    public BaseEntity entity;
    public float buoyancyScale;
    public int waterDetectionRate;
    private void AddBuoyancyComponent();
     void FixedUpdate();
}

private class FloatingItemsConfig
{
    public int waterDetectionRate;
    public float globalBuoyancy;
    public Dictionary<string, object> ItemBuoyancy;
    public Dictionary<string, object> CategoryBuoyancy;
    private FloatingItems plugin;
    public FloatingItemsConfig(FloatingItems plugin);
    private void GetConfig(T variable, string[] path);
    private void SetConfig(T variable, string[] path);
}


```

---

## FlyingCarpet by  - Fly a custom object consisting of carpet, chair, lantern, and lock

```csharp
using Oxide.Core;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Rust;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Globalization;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Flying Carpet", "RFC1920", "1.1.7")]
[Description("Fly a custom object consisting of carpet, chair, lantern, lock, and small sign")]
 class FlyingCarpet : RustPlugin
{
    static LayerMask layerMask;
     BaseEntity newCarpet;
    static Dictionary<ulong, PlayerCarpetData> loadplayer;
    static List<ulong> pilotslist;
    [PluginReference]
     Plugin SignArtist;
    public class PlayerCarpetData
    {
        public BasePlayer player;
        public int carpetcount;
    }

     void Init();
     bool isAllowed(BasePlayer player, string perm);
    private bool HasPermission(ConsoleSystem.Arg arg, string permname);
    private static HashSet<BasePlayer> FindPlayers(string nameOrIdOrIp);
     bool UseMaxCarpetChecks;
     bool playemptysound;
    public int maxcarpets;
    public int vipmaxcarpets;
    static float MinAltitude;
    static float MinDistance;
    static ulong rugSkinID;
    static ulong chairSkinID;
    static float NormalSpeed;
    static float SprintSpeed;
    static bool requirefuel;
    static bool doublefuel;
    static bool nameOnSign;
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void LoadVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
    private string _(string msgId, BasePlayer player, object[] args);
    private void PrintMsgL(BasePlayer player, string msgId, object[] args);
    private void PrintMsg(BasePlayer player, string msg);
    [Command("fc"), Permission("flyingcarpet.use")]
     void cmdCarpetBuild(IPlayer iplayer, string command, string[] args);
    [Command("fcg"), Permission("flyingcarpet.admin")]
     void cmdCarpetGiveChat(IPlayer iplayer, string command, string[] args);
    [ConsoleCommand("fcgive")]
     void cmdCarpetGive(ConsoleSystem.Arg arg);
    [Command("fcc"), Permission("flyingcarpet.use")]
     void cmdCarpetCount(IPlayer iplayer, string command, string[] args);
    [Command("fcd"), Permission("flyingcarpet.use")]
     void cmdCarpetDestroy(IPlayer iplayer, string command, string[] args);
    [Command("fchelp"), Permission("flyingcarpet.use")]
     void cmdCarpetHelp(IPlayer iplayer, string command, string[] args);
     object OnOvenToggle(BaseOven oven, BasePlayer player);
     void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
    private object OnNightLanternToggle(BaseEntity entity, bool status);
    private void AddCarpet(BasePlayer player, Vector3 location);
    public bool PilotListContainsPlayer(BasePlayer player);
     void AddPlayerToPilotsList(BasePlayer player);
    public void RemovePlayerFromPilotsList(BasePlayer player);
     void DestroyLocalCarpet(BasePlayer player);
     void DestroyAllCarpets(BasePlayer player);
     void DestroyRemoteCarpet(BasePlayer player);
     void OnPlayerInput(BasePlayer player, InputState input);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object OnEntityGroundMissing(BaseEntity entity);
     bool CarpetLimitReached(BasePlayer player, bool vip);
     object CanDismountEntity(BasePlayer player, BaseMountable entity);
     void OnEntityMounted(BaseMountable mountable, BasePlayer player);
     void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private object CanPickupLock(BasePlayer player, BaseLock baseLock);
     void AddPlayerID(ulong ownerid);
     void RemovePlayerID(ulong ownerid);
     object OnPlayerDeath(BasePlayer player, HitInfo info);
     void RemoveCarpet(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerRespawned(BasePlayer player);
     void DestroyAll();
     void Unload();
    static List<BasePlayer> carpetantihack;
     object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
     class CarpetEntity : BaseEntity
    {
        public BaseEntity entity;
        public BasePlayer player;
        public BaseEntity sitbox;
        public BaseEntity carpet1;
        public BaseEntity lantern1;
        public BaseEntity carpetlock;
        public BaseEntity lights1;
        public BaseEntity lights2;
        public BaseEntity sign;
        public string entname;
         Quaternion entityrot;
         Vector3 entitypos;
        public bool moveforward;
        public bool movebackward;
        public bool moveup;
        public bool movedown;
        public bool rotright;
        public bool rotleft;
        public bool sprinting;
        public bool islanding;
        public bool mounted;
        public bool engineon;
        public bool hasFuel;
        public bool needfuel;
        public ulong skinid;
        public ulong ownerid;
         float minaltitude;
         FlyingCarpet instance;
        public bool throttleup;
         float sprintspeed;
         float normalspeed;
         SphereCollider sphereCollider;
         string prefabbox;
         string prefabcarpet;
         string prefablamp;
         string prefablights;
         string prefablock;
         string prefabsign;
         void Awake();
         BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, BaseEntity parent, ulong skinid);
         void SpawnRefresh(BaseEntity entity);
        public void SetFuel(int amount);
        public void SpawnCarpet();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
         BasePlayer GetPilot();
        public void CarpetInput(InputState input, BasePlayer player);
        public bool FuelCheck();
         void FixedUpdate();
        private IEnumerator RefreshTrain();
         void ResetMovement();
        public void OnDestroy();
    }

}

public class PlayerCarpetData
{
    public BasePlayer player;
    public int carpetcount;
}

 class CarpetEntity : BaseEntity
{
    public BaseEntity entity;
    public BasePlayer player;
    public BaseEntity sitbox;
    public BaseEntity carpet1;
    public BaseEntity lantern1;
    public BaseEntity carpetlock;
    public BaseEntity lights1;
    public BaseEntity lights2;
    public BaseEntity sign;
    public string entname;
     Quaternion entityrot;
     Vector3 entitypos;
    public bool moveforward;
    public bool movebackward;
    public bool moveup;
    public bool movedown;
    public bool rotright;
    public bool rotleft;
    public bool sprinting;
    public bool islanding;
    public bool mounted;
    public bool engineon;
    public bool hasFuel;
    public bool needfuel;
    public ulong skinid;
    public ulong ownerid;
     float minaltitude;
     FlyingCarpet instance;
    public bool throttleup;
     float sprintspeed;
     float normalspeed;
     SphereCollider sphereCollider;
     string prefabbox;
     string prefabcarpet;
     string prefablamp;
     string prefablights;
     string prefablock;
     string prefabsign;
     void Awake();
     BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, BaseEntity parent, ulong skinid);
     void SpawnRefresh(BaseEntity entity);
    public void SetFuel(int amount);
    public void SpawnCarpet();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
     BasePlayer GetPilot();
    public void CarpetInput(InputState input, BasePlayer player);
    public bool FuelCheck();
     void FixedUpdate();
    private IEnumerator RefreshTrain();
     void ResetMovement();
    public void OnDestroy();
}


```

---

## FlyingVehicle by misticos - Make vehicles fly on your server

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using Physics = UnityEngine.Physics;

Oxide.Plugins
[Info("Flying Vehicle", "misticos", "1.0.3")]
[Description("Make vehicles fly on your server")]
 class FlyingVehicle : CovalencePlugin
{
    private const string PermissionUse;
    private const string PrefabBoat;
    private const string PrefabRhib;
    private const string PrefabSedan;
    private const string PrefabSeat;
    private string _seatGuid;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerInput(BasePlayer player, InputState state);
    protected override void LoadDefaultMessages();
    private void CommandSpawn(IPlayer player, string command, string[] args);
    private void CommandClaim(IPlayer player, string command, string[] args);
    private object OnVehiclePush(BaseVehicle boat, BasePlayer player);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private void Setup(BaseVehicle entity);
    private class VehicleController : FacepunchBehaviour
    {
        public Rigidbody Rigidbody;
        public BaseVehicle Vehicle;
        public bool IsEnabled;
        private void Awake();
        private void OnDestroy();
        private void FixedUpdate();
        public void Toggle();
    }

    private string GetMsg(string key, string userId);
}

private class VehicleController : FacepunchBehaviour
{
    public Rigidbody Rigidbody;
    public BaseVehicle Vehicle;
    public bool IsEnabled;
    private void Awake();
    private void OnDestroy();
    private void FixedUpdate();
    public void Toggle();
}


```

---

## FogVoting by Ultra - Initializes voting to remove fog from the environment

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Fog Voting", "Ultra", "1.2.21")]
[Description("Initializes voting to remove fog from the environment")]
 class FogVoting : RustPlugin
{
     bool isVotingOpen;
     Timer fogCheckTimer;
     Timer votingTimer;
     Timer votingPanelRefreshTimer;
     DateTime votingStart;
     List<string> usersVotingNo;
    static CuiPanel votingPanel;
     string votingPanelName;
     string fogYesPanelName;
     string fogNoPanelName;
     string votingScorePanelName;
     void OnServerInitialized();
     void Unload();
    [ChatCommand("nofog")]
     void FogNo(BasePlayer player);
    [ChatCommand("setfog")]
     void SetFog(BasePlayer player, string command, string[] args);
    [ChatCommand("checkfog")]
     void CheckFog(BasePlayer player, string command, string[] args);
     void CheckCurrentFog();
     void OpenVoting();
     void CheckVoting();
     void CloseVoting(float intervalMultiplier);
     void DestroyTimer(Timer timer);
     void DestroyVotingPanel();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Fog value to start voting 0.0 - 1.0")]
        public float FogLimit;
        [JsonProperty(PropertyName = "Interval to check the current fog (seconds)")]
        public float FogCheckInterval;
        [JsonProperty(PropertyName = "Voting duration (seconds)")]
        public float VotingDuration;
        [JsonProperty(PropertyName = "Required vote percentage")]
        public float VotePercentage;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void InitCUI();
     void ShowVotingPanel();
     CuiPanel GetPanel(string anchorMin, string anchorMax, string color, bool cursorEnabled);
     CuiLabel GetLabel(string text, int size, string anchorMin, string anchorMax, TextAnchor align, string color, string font);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Fog value to start voting 0.0 - 1.0")]
    public float FogLimit;
    [JsonProperty(PropertyName = "Interval to check the current fog (seconds)")]
    public float FogCheckInterval;
    [JsonProperty(PropertyName = "Voting duration (seconds)")]
    public float VotingDuration;
    [JsonProperty(PropertyName = "Required vote percentage")]
    public float VotePercentage;
}


```

---

## FoodGrill by VisEntities - Adds the physical models of meat on the barbeque with different transforms

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using UnityEngine;

Oxide.Plugins
[Info("Food Grill", "Dana", "1.0.5")]
[Description("Displays meat models on the barbeque.")]
 class FoodGrill : RustPlugin
{
     List<FOG> FOGLIST;
    private PluginConfig _pluginConfig;
     void OnFindBurnable(BaseOven oven);
     void OnOvenToggle(BaseOven oven, BasePlayer player);
     void init();
     void Unload();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
     class FOG : MonoBehaviour
    {
         BaseEntity entity;
         BaseOven oven;
        public List<Item> ItemList;
        private IEnumerator coroutine;
         bool wait;
        private IEnumerator Wait(float waitTime);
         void Awake();
        public void AddFood(GrillConfig grillConfig);
        public void Delete();
         void Destroy();
    }

    private class PluginConfig
    {
        public GrillConfig Config { get; set; }
    }

    private class GrillConfig
    {
        [JsonProperty(PropertyName = "Plugin - Enabled")]
        public bool IsEnabled { get; set; }
        [JsonProperty(PropertyName = "Show Default Food")]
        public bool ShowDefaultFood { get; set; }
        [JsonProperty(PropertyName = "Default Raw Food")]
        public string DefaultRawFood { get; set; }
        [JsonProperty(PropertyName = "Default Cooked Food")]
        public string DefaultCookedFood { get; set; }
        [JsonProperty(PropertyName = "Default Burnt Food")]
        public string DefaultBurntFood { get; set; }
        [JsonProperty(PropertyName = "Default Spoiled Food")]
        public string DefaultSpoiledFood { get; set; }
        [JsonProperty(PropertyName = "Raw Food List")]
        public List<string> RawFoods { get; set; }
        [JsonProperty(PropertyName = "Cooked Food List")]
        public List<string> CookedFoods { get; set; }
        [JsonProperty(PropertyName = "Burnt Food List")]
        public List<string> BurntFoods { get; set; }
        [JsonProperty(PropertyName = "Spoiled Food List")]
        public List<string> SpoiledFoods { get; set; }
    }

}

 class FOG : MonoBehaviour
{
     BaseEntity entity;
     BaseOven oven;
    public List<Item> ItemList;
    private IEnumerator coroutine;
     bool wait;
    private IEnumerator Wait(float waitTime);
     void Awake();
    public void AddFood(GrillConfig grillConfig);
    public void Delete();
     void Destroy();
}

private class PluginConfig
{
    public GrillConfig Config { get; set; }
}

private class GrillConfig
{
    [JsonProperty(PropertyName = "Plugin - Enabled")]
    public bool IsEnabled { get; set; }
    [JsonProperty(PropertyName = "Show Default Food")]
    public bool ShowDefaultFood { get; set; }
    [JsonProperty(PropertyName = "Default Raw Food")]
    public string DefaultRawFood { get; set; }
    [JsonProperty(PropertyName = "Default Cooked Food")]
    public string DefaultCookedFood { get; set; }
    [JsonProperty(PropertyName = "Default Burnt Food")]
    public string DefaultBurntFood { get; set; }
    [JsonProperty(PropertyName = "Default Spoiled Food")]
    public string DefaultSpoiledFood { get; set; }
    [JsonProperty(PropertyName = "Raw Food List")]
    public List<string> RawFoods { get; set; }
    [JsonProperty(PropertyName = "Cooked Food List")]
    public List<string> CookedFoods { get; set; }
    [JsonProperty(PropertyName = "Burnt Food List")]
    public List<string> BurntFoods { get; set; }
    [JsonProperty(PropertyName = "Spoiled Food List")]
    public List<string> SpoiledFoods { get; set; }
}


```

---

## ForceJoinTeam by Akkariin - Join a player's team without invite

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Force Join Team", "Akkariin", "1.0.2")]
[Description("Force join a team")]
public class ForceJoinTeam : RustPlugin
{
    private const string Perm;
    private void Init();
    [ChatCommand("forcejoin")]
    private void ForceJoinTeamCommand(BasePlayer player, string command, string[] args);
    private BasePlayer FindPlayerByName(string keyword);
    private String FormatMessage(string message, BasePlayer player);
    protected override void LoadDefaultMessages();
}


```

---

## ForeverAlone by misticos - Limit the number of players in a team on the server

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Forever Alone", "misticos", "1.0.1")]
[Description("Limit the number of players in a team on the server")]
 class ForeverAlone : RustPlugin
{
    private static readonly int playerLayer;
    static float AroundRadius;
    static float CheckInterval;
    static float timeBeforeShock;
    static float DamagePerTime;
    static int MaxAllowedPlayers;
     void OnServerInitialized();
    protected override void LoadDefaultConfig();
     T GetConfig(string name, T defaultValue);
     void Unload();
     void OnPlayerConnected(BasePlayer player);
     void CheckComponent(BasePlayer player);
     class AroundTimer : MonoBehaviour
    {
         float ElaspedSeconds;
         BasePlayer player;
         void Awake();
         void CheckAround();
    }

     int GetLimit { get; set; }
    static void DoShock(BasePlayer player);
    static bool IsVisible(BasePlayer player, Vector3 source, Vector3 dest);
}

 class AroundTimer : MonoBehaviour
{
     float ElaspedSeconds;
     BasePlayer player;
     void Awake();
     void CheckAround();
}


```

---

## FortWars by Ryan - Custom gamemode that changes values of gathering/smelting/crafting based on a current phase

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UI = Oxide.Plugins.FortWarsUI.UIMethods;
using Rgba = Oxide.Plugins.FortWarsUI.Rgba;
using Anchor = Oxide.Plugins.FortWarsUI.Anchor;

Oxide.Plugins
[Info("Fort Wars", "Ryan", "2.0.01")]
[Description("Custom gamemode that changes values of gathering/smelting/crafting based on a current phase.")]
public class FortWars : RustPlugin
{
    private static ConfigFile CFile;
    private static DataFile DFile;
    private const string AdminPerm;
    private Timer UiTimer;
    private Timer HeliTimer;
    private Timer DropTimer;
    private Dictionary<string, float> DefaultBlueprints;
    private const string MainContainer;
    private const string CountdownContainer;
    private const string CountdownTimer;
    private const string PhaseContainer;
    private const string PhaseContent;
    private CuiElementContainer CachedContainer;
    private Dictionary<ulong, BasePlayer> UiPlayers;
    private List<uint> PluginEntities;
    private class ConfigFile
    {
        public HelicopterSettings HelicopterSettings;
        public AirdropSettings AirdropSettings;
        public CraftSettings CraftSettings;
        public GatherSettings GatherSettings;
        public TimeSettings TimeSettings;
        public UiSettings UiSettings;
        public static ConfigFile DefaultConfig();
    }

    private class SmeltSettings
    {
        public Multiplier MultiplierSettings { get; set; }
        public SmeltSettings();
    }

    private class UiSettings
    {
        public bool Enabled { get; set; }
        public Rgba PrimaryColor { get; set; }
        public Rgba SecondaryColor { get; set; }
        public ConfigAnchor CountdownAnchor { get; set; }
        public ConfigAnchor PhaseAnchor { get; set; }
        public UiSettings();
    }

    private class ConfigAnchor
    {
        public Anchor Min { get; set; }
        public Anchor Max { get; set; }
        public ConfigAnchor();
        public ConfigAnchor(Anchor min, Anchor max);
    }

    private class HelicopterSettings
    {
        public float Health;
        public Rotor RotorHealth;
        public float Speed;
        public float BulletDamage;
        public int MaxCrates;
        public HelicopterSettings();
    }

    private class AirdropSettings
    {
        public Multiplier AirdropAmounts;
        public AirdropSettings();
    }

    private class CraftSettings
    {
        public Multiplier MultiplierSettings;
        public CraftSettings();
    }

    private class GatherSettings
    {
        public Multiplier MultiplierSettings;
        public GatherSettings();
    }

    private class TimeSettings
    {
        [JsonProperty("Build phase length (minutes)")]
        public float Build;
        [JsonProperty("Fight phrase length (minutes)")]
        public float Fight;
        [JsonProperty("Helicopter frequency (seconds)")]
        public float Heli;
        [JsonProperty("Airdrop Frequency (seconds)")]
        public Multiplier Airdrop;
        public TimeSettings();
    }

    private class Multiplier
    {
        public float Fight;
        public float Build;
        public Multiplier();
        public Multiplier(float fight, float build);
    }

    private class Rotor
    {
        public float MainHealth;
        public float TailHealth;
        public Rotor();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Regenerate();
    private class DataFile
    {
        public PhaseInfo PhaseInfo;
    }

    private class PhaseInfo
    {
        public bool BuildPhase;
        public bool FightPhase;
        public DateTime StartTime;
        public bool FortWarActive { get; set; }
        public PhaseInfo();
        public bool ShouldEnd(float period);
        public double GetSeconds(float period);
    }

    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void DestroyTimers();
    private void UnsubHooks();
    private void SubHooks();
    private void InitializeBuildPhase(DateTime? startTime);
    private void EndBuildPhase();
    private void InitializeFightPhase(DateTime? startTime);
    private void EndFightPhase();
    private void CallHeli();
    private void CallDrop();
    private void CheckPhase();
    private void InitializeUi();
    private void AddUi(BasePlayer player);
    private void DestroyUi();
    private void ConstructUi();
    private void RefreshTimer();
    private CuiElementContainer DrawTimer();
    private void RefreshPhase();
    private CuiElementContainer DrawPhase();
    private string GetFormattedTime(double time);
    private float GetMultiplier(Multiplier mult);
    private void UpdateCraftTimes();
    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private void OnEntityDeath(BaseCombatEntity entity);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnect(BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnCollectiblePickup(Item item, BasePlayer player);
    [ConsoleCommand("fortwars.start")]
    private void StartCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("fortwars.end")]
    private void EndCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("fortwars.build")]
    private void BuildPhaseCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("fortwars.fight")]
    private void FightPhaseCommand(ConsoleSystem.Arg arg);
    [ChatCommand("phase")]
    private void PhaseCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("hell")]
    private void HellCommand(BasePlayer player, string command, string[] args);
}

private class ConfigFile
{
    public HelicopterSettings HelicopterSettings;
    public AirdropSettings AirdropSettings;
    public CraftSettings CraftSettings;
    public GatherSettings GatherSettings;
    public TimeSettings TimeSettings;
    public UiSettings UiSettings;
    public static ConfigFile DefaultConfig();
}

private class SmeltSettings
{
    public Multiplier MultiplierSettings { get; set; }
    public SmeltSettings();
}

private class UiSettings
{
    public bool Enabled { get; set; }
    public Rgba PrimaryColor { get; set; }
    public Rgba SecondaryColor { get; set; }
    public ConfigAnchor CountdownAnchor { get; set; }
    public ConfigAnchor PhaseAnchor { get; set; }
    public UiSettings();
}

private class ConfigAnchor
{
    public Anchor Min { get; set; }
    public Anchor Max { get; set; }
    public ConfigAnchor();
    public ConfigAnchor(Anchor min, Anchor max);
}

private class HelicopterSettings
{
    public float Health;
    public Rotor RotorHealth;
    public float Speed;
    public float BulletDamage;
    public int MaxCrates;
    public HelicopterSettings();
}

private class AirdropSettings
{
    public Multiplier AirdropAmounts;
    public AirdropSettings();
}

private class CraftSettings
{
    public Multiplier MultiplierSettings;
    public CraftSettings();
}

private class GatherSettings
{
    public Multiplier MultiplierSettings;
    public GatherSettings();
}

private class TimeSettings
{
    [JsonProperty("Build phase length (minutes)")]
    public float Build;
    [JsonProperty("Fight phrase length (minutes)")]
    public float Fight;
    [JsonProperty("Helicopter frequency (seconds)")]
    public float Heli;
    [JsonProperty("Airdrop Frequency (seconds)")]
    public Multiplier Airdrop;
    public TimeSettings();
}

private class Multiplier
{
    public float Fight;
    public float Build;
    public Multiplier();
    public Multiplier(float fight, float build);
}

private class Rotor
{
    public float MainHealth;
    public float TailHealth;
    public Rotor();
}

private class DataFile
{
    public PhaseInfo PhaseInfo;
}

private class PhaseInfo
{
    public bool BuildPhase;
    public bool FightPhase;
    public DateTime StartTime;
    public bool FortWarActive { get; set; }
    public PhaseInfo();
    public bool ShouldEnd(float period);
    public double GetSeconds(float period);
}

Oxide.Plugins.FortWarsUI
public class UIMethods
{
    public static CuiElementContainer Container(string name, string bgColor, Anchor Min, Anchor Max, string parent, float fadeOut, float fadeIn);
    public static void Panel(string name, string parent, CuiElementContainer container, string bgColor, Anchor Min, Anchor Max, bool cursor);
    public static void Label(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string text, string color, int fontSize, TextAnchor textAnchor, string font);
    public static void Button(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string command, string text, string textColor, int fontSize, string color, TextAnchor anchor, float fadeOut, float fadeIn, string font);
    public static void Text(string name, string parent, CuiElementContainer container, TextAnchor anchor, string color, int fontSize, string text, Anchor Min, Anchor Max, string font, float fadeOut, float fadeIn);
    public static void Element(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string bgColor, string material, float fadeOut, float fadeIn);
    public static void Border(Anchor posMin, Anchor posMax, CuiElementContainer container, float borderSize, string color, string parent);
}

public class Anchor
{
    public float X { get; set; }
    public float Y { get; set; }
    public Anchor();
    public Anchor(float x, float y);
}

public class Rgba
{
    public float R { get; set; }
    public float G { get; set; }
    public float B { get; set; }
    public float A { get; set; }
    public Rgba();
    public Rgba(float r, float g, float b, float a);
    public string Format();
}


```

---

## FoundationLimit by noname - Limits the number of foundations that players can place in each building

```csharp
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Foundation Limit", "noname", "1.4.2")]
[Description("Limits the number of foundations allowed")]
 class FoundationLimit : CovalencePlugin
{
    public static FoundationLimit Plugin;
    private int MaskInt;
    private const string foundationlimit_admin_Perm;
    private const string foundationlimit_bypass_Perm;
    private uint FoundationPrefabID;
    private uint TriangleFoundationPrefabID;
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private PluginConfig config;
    private void LoadConfig();
    private class PluginConfig
    {
        public int SearchRange;
        public bool UseFoundationClassification;
        public int DefaultFoundationLimit;
        public int DefaultTriangleFoundationLimit;
        public int MinFoundationsBeforeMsgShowsUp;
        public List<PermissionItem> permissionItems;
        public PluginConfig();
    }

    public class PermissionItem
    {
        public string PermissionName;
        public int FoundationLimit;
        public int TriangleFoundationLimit;
        public PermissionItem();
        public PermissionItem(string permissionName, int foundationLimit);
        public PermissionItem(string permissionName, int squarefoundationLimit, int trianglefoundationlimit);
    }

    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void RegisterPermissions();
    private void OnEntityBuilt(Planner planner, GameObject gObject);
     object OnStructureDemolish(BaseCombatEntity entity, BasePlayer player, bool immediate);
    private List<BaseEntity> FindLinkedStructures(BasePlayer player, BaseEntity entity);
    private int GetPermLimitCount(BasePlayer player);
    private FoundationSet GetPermClassLimitCount(BasePlayer player);
    [Command("fdlimit")]
     void SetFoundationLimitCommand(IPlayer player, string command, string[] args);
}

private class PluginConfig
{
    public int SearchRange;
    public bool UseFoundationClassification;
    public int DefaultFoundationLimit;
    public int DefaultTriangleFoundationLimit;
    public int MinFoundationsBeforeMsgShowsUp;
    public List<PermissionItem> permissionItems;
    public PluginConfig();
}

public class PermissionItem
{
    public string PermissionName;
    public int FoundationLimit;
    public int TriangleFoundationLimit;
    public PermissionItem();
    public PermissionItem(string permissionName, int foundationLimit);
    public PermissionItem(string permissionName, int squarefoundationLimit, int trianglefoundationlimit);
}


```

---

## FpsOverride by MJSU - Allows server owners to set their server fps greater than the 240 limit

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Fps Override", "MJSU", "0.0.1")]
[Description("Overrides the server target fps")]
internal class FpsOverride : RustPlugin
{
    private PluginConfig _pluginConfig;
    private void Init();
    protected override void LoadDefaultConfig();
    private void ConfigLoad();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Server Fps Target")]
        public int Fps { get; set; }
    }

}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Server Fps Target")]
    public int Fps { get; set; }
}


```

---

## FPSRestart by MasterSplinter - Restarts server when FPS reaches configured limit

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("FPS Restart", "RustySpoon342", "1.2.1")]
[Description("Restarts the server when FPS reaches a specific target")]
public class FPSRestart : CovalencePlugin
{
    private Timer timerAborted;
    private Timer timerFirstCheck;
    private Timer timerLastCheck;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "FPS To Trigger Restart")]
        public float FrameRate;
        [JsonProperty(PropertyName = "How Long The Restart Should Be")]
        public float RestartTime;
        [JsonProperty("Show Restart Message To Server")]
        public bool ShowMessage;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void Unload();
    protected override void LoadDefaultMessages();
    private void FramerateFirstCheck();
    private void FramerateLastCheck();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "FPS To Trigger Restart")]
    public float FrameRate;
    [JsonProperty(PropertyName = "How Long The Restart Should Be")]
    public float RestartTime;
    [JsonProperty("Show Restart Message To Server")]
    public bool ShowMessage;
}


```

---

## FrameBox by MJSU - Deploys a frame on a box allowing the player to set the image

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Drawing;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Frame Box", "MJSU", "1.0.2")]
[Description("Deploys a frame on a box allowing the player to set the image")]
internal class FrameBox : RustPlugin
{
    private StoredData _storedData;
    private PluginConfig _pluginConfig;
    private static FrameBox _ins;
    private const string UsePermission;
    private const string NoCostPermission;
    private const string AccentColor;
    private readonly Hash<NetworkableId, BoxData> _boxes;
    private readonly Hash<NetworkableId, FrameData> _frames;
    private readonly Hash<string, ContainerData> _containerPositions;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    public PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnNewSave(string filename);
    private void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private void SignBoxChatCommand(BasePlayer player, string cmd, string[] args);
    public void HandleHelp(BasePlayer player);
    public void HandleAdd(BasePlayer player, string[] args);
    public ulong AddFrame(BoxData boxData, string prefabName, PositionData framePos, string pos);
    public void HandleRemove(BasePlayer player, string[] args);
    public void HandleCost(BasePlayer player);
    public void HandleUnlock(BasePlayer player, string[] args);
    private bool GetFrameBox(BasePlayer player, BoxData boxData, FrameData frameData);
    public string[] ParseFramePositions(BasePlayer player, BoxData box, string pos);
    private object CanLootEntity(BasePlayer player, PhotoFrame frame);
    private void OnEntityKill(BoxStorage box);
    private void OnEntityKill(PhotoFrame frame);
    private void OnSignLocked(PhotoFrame frame, BasePlayer player);
    private object GetImageRotation(PhotoFrame frame);
    private Hash<string, int> GetRemoveAdditionalRefund(BoxStorage entity, Hash<string, int> refund);
    public bool CanAfford(BasePlayer player, Hash<string, int> cost, int multiplier);
    public void TakeCost(BasePlayer player, Hash<string, int> cost, int multiplier);
    public string GetShortPrefabName(string shortname);
    public string GetPrefabName(string shortname);
    public T Raycast(BasePlayer player);
    public void SaveData();
    public void Chat(BasePlayer player, string key, object[] args);
    public string Lang(string key, BasePlayer player, object[] args);
    public bool HasPermission(BasePlayer player, string perm);
    public class FrameData
    {
        public PhotoFrame Frame { get; set; }
        public BoxData BoxData { get; set; }
        public string Position { get; set; }
        public FrameData(PhotoFrame frame, BoxData boxData, string position);
        public bool Unlock(BasePlayer player);
        private void DestroyGroundWatch(BaseEntity entity);
        public void OnKilled();
        public void RemoveFrame(bool kill);
    }

    public class BoxData
    {
        public BaseCombatEntity Box { get; set; }
        private Hash<string, FrameData> BoxFrames { get; set; }
        public BoxData(BaseCombatEntity box);
        public FrameData GetFrame(string position);
        public bool AddFrame(PhotoFrame frame, string position);
        public bool RemoveFrame(string position, bool kill);
        public bool IsSlotTaken(string position);
        public string[] GetOpenPositions(bool allOnly);
        public string[] GetTakenFramePositions();
        public int GetFrameCount();
        public void OnKilled();
    }

    public class AdminBoxBehavior : FacepunchBehaviour
    {
        private BasePlayer Player { get; set; }
        private bool IsDown { get; set; }
        private void Awake();
        private void Update();
        private IEnumerator OpenBox(PhotoFrame frame, IItemContainerEntity box);
        public void DoDestroy();
    }

    public class PluginConfig
    {
        [DefaultValue("fb")]
        [JsonProperty(PropertyName = "Chat Command")]
        public string ChatCommand { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Refund on remove")]
        public bool RefundOnRemove { get; set; }
        [DefaultValue(1f)]
        [JsonProperty(PropertyName = "Refund % (0 none - 1 max)")]
        public float RefundPercentage { get; set; }
        [DefaultValue(5f)]
        [JsonProperty(PropertyName = "Max distance (Meters)")]
        public float MaxDistance { get; set; }
        [JsonProperty(PropertyName = "Cost")]
        public Hash<string, int> Cost { get; set; }
        [JsonProperty(PropertyName = "Sign Positions")]
        public Hash<string, ContainerData> SignPositions { get; set; }
    }

    public class StoredData
    {
        public Hash<ulong, Hash<string, ulong>> FrameData;
    }

    public class ContainerData
    {
        [JsonProperty(PropertyName = "Frame Item Shortname")]
        public string ItemShortname { get; set; }
        [JsonProperty(PropertyName = "Frame Positions")]
        public Hash<string, PositionData> PositionData { get; set; }
    }

    public class PositionData
    {
        [JsonConverter(typeof(Vector3Converter))]
        [JsonProperty(PropertyName = "Frame Position Offset")]
        public Vector3 Offset { get; set; }
        [JsonConverter(typeof(Vector3Converter))]
        [JsonProperty(PropertyName = "Frame Position Rotation")]
        public Vector3 Rotation { get; set; }
        [JsonProperty(PropertyName = "Include Position In All Option")]
        public bool IncludeAll { get; set; }
        [JsonIgnore]
        public string DisplayName { get; set; }
    }

    public class LangKeys
    {
        public const string Chat;
        public const string NoPermission;
        public const string HelpText;
        public const string MissingItem;
        public const string MissingMessage;
        public const string CostPre;
        public const string Cost;
        public const string AddInvalidSyntax;
        public const string InvalidAddFramePosition;
        public const string NotLookingAt;
        public const string AddSlotIsTaken;
        public const string AddSuccess;
        public const string RemoveSuccess;
        public const string UnlockBuildingBlocked;
        public const string UnlockSuccess;
        public const string NotLookingAtFrame;
        public const string NotLookingAtFrameBoxFrame;
        public const string NotLookingAtFrameBox;
        public const string NoFrameInPosition;
        public const string ContainerNotAllowed;
        public const string NoUnlockPermission;
        public const string AddSubCommand;
        public const string RemoveSubCommand;
        public const string CostSubCommand;
        public const string UnlockSubCommand;
        public const string AllSubOption;
    }

    public class Vector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class FrameData
{
    public PhotoFrame Frame { get; set; }
    public BoxData BoxData { get; set; }
    public string Position { get; set; }
    public FrameData(PhotoFrame frame, BoxData boxData, string position);
    public bool Unlock(BasePlayer player);
    private void DestroyGroundWatch(BaseEntity entity);
    public void OnKilled();
    public void RemoveFrame(bool kill);
}

public class BoxData
{
    public BaseCombatEntity Box { get; set; }
    private Hash<string, FrameData> BoxFrames { get; set; }
    public BoxData(BaseCombatEntity box);
    public FrameData GetFrame(string position);
    public bool AddFrame(PhotoFrame frame, string position);
    public bool RemoveFrame(string position, bool kill);
    public bool IsSlotTaken(string position);
    public string[] GetOpenPositions(bool allOnly);
    public string[] GetTakenFramePositions();
    public int GetFrameCount();
    public void OnKilled();
}

public class AdminBoxBehavior : FacepunchBehaviour
{
    private BasePlayer Player { get; set; }
    private bool IsDown { get; set; }
    private void Awake();
    private void Update();
    private IEnumerator OpenBox(PhotoFrame frame, IItemContainerEntity box);
    public void DoDestroy();
}

public class PluginConfig
{
    [DefaultValue("fb")]
    [JsonProperty(PropertyName = "Chat Command")]
    public string ChatCommand { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Refund on remove")]
    public bool RefundOnRemove { get; set; }
    [DefaultValue(1f)]
    [JsonProperty(PropertyName = "Refund % (0 none - 1 max)")]
    public float RefundPercentage { get; set; }
    [DefaultValue(5f)]
    [JsonProperty(PropertyName = "Max distance (Meters)")]
    public float MaxDistance { get; set; }
    [JsonProperty(PropertyName = "Cost")]
    public Hash<string, int> Cost { get; set; }
    [JsonProperty(PropertyName = "Sign Positions")]
    public Hash<string, ContainerData> SignPositions { get; set; }
}

public class StoredData
{
    public Hash<ulong, Hash<string, ulong>> FrameData;
}

public class ContainerData
{
    [JsonProperty(PropertyName = "Frame Item Shortname")]
    public string ItemShortname { get; set; }
    [JsonProperty(PropertyName = "Frame Positions")]
    public Hash<string, PositionData> PositionData { get; set; }
}

public class PositionData
{
    [JsonConverter(typeof(Vector3Converter))]
    [JsonProperty(PropertyName = "Frame Position Offset")]
    public Vector3 Offset { get; set; }
    [JsonConverter(typeof(Vector3Converter))]
    [JsonProperty(PropertyName = "Frame Position Rotation")]
    public Vector3 Rotation { get; set; }
    [JsonProperty(PropertyName = "Include Position In All Option")]
    public bool IncludeAll { get; set; }
    [JsonIgnore]
    public string DisplayName { get; set; }
}

public class LangKeys
{
    public const string Chat;
    public const string NoPermission;
    public const string HelpText;
    public const string MissingItem;
    public const string MissingMessage;
    public const string CostPre;
    public const string Cost;
    public const string AddInvalidSyntax;
    public const string InvalidAddFramePosition;
    public const string NotLookingAt;
    public const string AddSlotIsTaken;
    public const string AddSuccess;
    public const string RemoveSuccess;
    public const string UnlockBuildingBlocked;
    public const string UnlockSuccess;
    public const string NotLookingAtFrame;
    public const string NotLookingAtFrameBoxFrame;
    public const string NotLookingAtFrameBox;
    public const string NoFrameInPosition;
    public const string ContainerNotAllowed;
    public const string NoUnlockPermission;
    public const string AddSubCommand;
    public const string RemoveSubCommand;
    public const string CostSubCommand;
    public const string UnlockSubCommand;
    public const string AllSubOption;
}

public class Vector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## FreeBuild by 0x89A - Build and upgrade for free

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Free Build", "0x89A", "2.1.2")]
[Description("Allows building, upgrading and placing deployables for free")]
 class FreeBuild : CovalencePlugin
{
    private const string usePerm;
    private const int _itemStartPosition;
    private readonly HashSet<BasePlayer> _activePlayers;
    private readonly string[] _resourceItemShortnames;
    private readonly List<Item> _resourceItems;
    private void Init();
    private void Unload();
    private void OnEntitySaved(BasePlayer player, BaseNetworkable.SaveInfo saveInfo);
    private void OnInventoryNetworkUpdate(PlayerInventory inventory, ItemContainer container, ProtoBuf.UpdateItemContainer updatedItemContainer, PlayerInventory.Type inventoryType, bool sendToEveryone);
    private object CanAffordUpgrade(BasePlayer player, BuildingBlock block, BuildingGrade.Enum iGrade);
     object CanAffordToPlace(BasePlayer player, Planner planner, Construction construction);
     object OnPayForPlacement(BasePlayer player, Planner planner, Construction construction);
     object OnPayForUpgrade(BasePlayer player, BuildingBlock block, ConstructionGrade gradeTarget);
    private void Command(IPlayer iplayer, string command, string[] args);
    private bool IsAllowed(BasePlayer player);
    private bool DeployableCheck(Planner planner);
    private void AddItems(ProtoBuf.ItemContainer containerData);
    private List<Item> GetItems();
    protected override void LoadDefaultMessages();
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Chat Command")]
        public string chatCommand;
        [JsonProperty("Require Chat Command")]
        public bool requireChat;
        [JsonProperty("Free Deployables")]
        public bool freeDeployables;
        [JsonProperty("Give Player Hidden Resources")]
        public bool giveHiddenResources;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Configuration
{
    [JsonProperty("Chat Command")]
    public string chatCommand;
    [JsonProperty("Require Chat Command")]
    public bool requireChat;
    [JsonProperty("Free Deployables")]
    public bool freeDeployables;
    [JsonProperty("Give Player Hidden Resources")]
    public bool giveHiddenResources;
}


```

---

## FreeCCTV by alibali - CCTV require no power connection to run

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Free CCTV", "mspeedie", "0.1.2")]
[Description("Allows CCTVs to operate without electricity.")]
 class FreeCCTV : RustPlugin
{
     void OnServerInitialized();
     void OnEntitySpawned(CCTV_RC cctv);
     void ChangePower(int amt);
     void Unload();
}


```

---

## FreeDrivingVehicles by MONaH - Allows player to drive vehicles without fuel.

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("Free Driving Vehicles", "MON@H", "1.1.4")]
[Description("Allows player to drive vehicles without fuel.")]
public class FreeDrivingVehicles : RustPlugin
{
    private const string PermissionAll;
    private const string PermissionRHIB;
    private const string PermissionRowboat;
    private const string PermissionWorkcart;
    private const string PermissionModularCar;
    private const string PermissionMiniCopter;
    private const string PermissionSnowmobile;
    private const string PermissionScrapCopter;
    private const string PermissionSubmarineDuo;
    private const string PermissionSubmarineSolo;
    private readonly Hash<uint, string> _prefabPermissions;
    private void Init();
    private void OnServerInitialized();
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Free driving HotAirBalloons for ALL players")]
        public bool HotAirBalloonAllowed;
        [JsonProperty(PropertyName = "Only authorized players can drive vehicles")]
        public bool AuthorizedOnlyAllowed;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private object OnFuelCheck(EntityFuelSystem fuelSystem);
    private void CreateCache();
    private object HandleFreeDriving(EntityFuelSystem fuelSystem, uint prefabID, string userIDString);
    private void CacheHasFuel(EntityFuelSystem fuelSystem);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Free driving HotAirBalloons for ALL players")]
    public bool HotAirBalloonAllowed;
    [JsonProperty(PropertyName = "Only authorized players can drive vehicles")]
    public bool AuthorizedOnlyAllowed;
}


```

---

## FreeLaptop by SamGreaves - A free plugin to add functionality to laptops

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using UnityEngine;
using ProtoBuf;
using Rust;
using System.Linq;
using System.Numerics;
using System.Resources;
using Network;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Free Laptop", "Sam Greaves", "2.1.1")]
[Description("A free plugin to add functionality to laptops")]
 class FreeLaptop : RustPlugin
{
    private readonly int _layerMask;
    protected override void LoadDefaultMessages();
    [ChatCommand("laptop")]
     void ChatLaptop(BasePlayer player, string command, string[] args);
    private Item LaptopInHands(BasePlayer player);
    private BaseEntity GetEntityLookingAt(BasePlayer player);
    private void ShowSettings(BasePlayer player);
    private bool Interact(BasePlayer player, BaseEntity entity, bool showAuth);
    private bool DoorUnlocking(Door door, BasePlayer player);
    private bool TcUnlocking(BuildingPrivlidge tc, BasePlayer player);
    private bool TurretUnlocking(AutoTurret turret, BasePlayer player);
    private bool DisplayTcAuthorizedPlayers(BasePlayer player, BuildingPrivlidge tc);
    private bool DisplayDoorAuthorizedPlayers(BasePlayer player, Door door);
    private bool DisplayTurretAuthorizedPlayers(BasePlayer player, AutoTurret turret);
    private string GetName(string id);
    private string Lang(string key, string id, object[] args);
    private class PluginConfig
    {
        public float LaptopDistance;
        public bool CanOpenDoors;
        public bool CanSeeTcAuth;
        public bool CanSeeDoorAuth;
        public bool CanUnlockTc;
        public bool CanSeeTurretAuth;
        public bool CanUnlockTurret;
    }

    private PluginConfig config;
    private void Init();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
}

private class PluginConfig
{
    public float LaptopDistance;
    public bool CanOpenDoors;
    public bool CanSeeTcAuth;
    public bool CanSeeDoorAuth;
    public bool CanUnlockTc;
    public bool CanSeeTurretAuth;
    public bool CanUnlockTurret;
}


```

---

## FreeLightString by  - Allows light strings to operate without electricity

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Free Light String", "mspeedie", "0.1.2")]
[Description("Allows lightstrings to operate without electricity.")]
 class FreeLightString : RustPlugin
{
     void OnServerInitialized();
     void OnEntitySpawned(AdvancedChristmasLights lightstring);
     void ChangePower(int amt);
     void Unload();
}


```

---

## FreeResearch by Zugzwang - Makes blueprint research and workbench experiments free and instantaneous.

```csharp
using System.Linq;

Oxide.Plugins
[Info("Free Research", "Zugzwang", "1.0.11")]
[Description("Free blueprint unlocking from research tables and workbench tech trees.")]
 class FreeResearch : RustPlugin
{
    const int ScrapID;
    const int MaxResearchScrap;
     void OnEntitySpawned(ResearchTable entity);
     void OnServerInitialized();
     void Unload();
     object CanUnlockTechTreeNode(BasePlayer player, TechTreeData.NodeInstance node, TechTreeData techTree);
     object OnResearchCostDetermine();
     object CanAcceptItem(ItemContainer container, Item item, int targetPos);
     object CanMoveItem(Item item, PlayerInventory playerLoot, ItemContainerId targetContainerId, int targetSlot, int num, bool flag);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     Item OnItemSplit(Item item, int amount);
     object CanPickupEntity(BasePlayer player, ResearchTable entity);
}


```

---

## FreeRT by IIIaKa - Provides free access to differents monuments for those who have permissions

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Free RT", "IIIaKa", "0.1.8")]
[Description("A simple plugin that allows players with permissions to open card-locked doors in Rad Towns without a card.")]
 class FreeRT : RustPlugin
{
    private const string PERMISSION_ALL;
    private const string PERMISSION_GREEN;
    private const string PERMISSION_BLUE;
    private const string PERMISSION_RED;
    private const string Str_MsgNotAllowed;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Is it worth showing messages to players who don't have permissions?")]
        public bool ShowMessage;
        [JsonProperty(PropertyName = "Time in seconds(1-10) after which the door will close(hinged doors only)")]
        public float CloseTime;
        public Oxide.Core.VersionNumber Version;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private bool TryOpenDoor(CardReader cardReader, BasePlayer player, Door door);
     void OnDoorKnocked(Door door, BasePlayer player);
     void OnSwitchToggled(ElectricSwitch electricSwitch, BasePlayer player);
     void OnButtonPress(PressButton button, BasePlayer player);
     void Init();
     void OnServerInitialized(bool initial);
     void Unload();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Is it worth showing messages to players who don't have permissions?")]
    public bool ShowMessage;
    [JsonProperty(PropertyName = "Time in seconds(1-10) after which the door will close(hinged doors only)")]
    public float CloseTime;
    public Oxide.Core.VersionNumber Version;
}


```

---

## Freeze by MrBlue - Prevents players from moving, and optionally prevents chat, commands, and damage

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Freeze", "Wulf", "3.0.3")]
[Description("Prevents players from moving, and optionally prevents chat, commands, and damage")]
internal class Freeze : CovalencePlugin
{
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Block chat while frozen")]
        public bool BlockChat;
        [JsonProperty("Block commands while frozen")]
        public bool BlockCommands;
        [JsonProperty("Block damage while frozen")]
        public bool BlockDamage;
        [JsonProperty("Block movement while frozen")]
        public bool BlockMovement;
        [JsonProperty("Notify target when frozen")]
        public bool NotifyTarget;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private readonly Dictionary<string, Timer> timers;
    private const string permFrozen;
    private const string permProtect;
    private const string permUse;
    [PluginReference]
    private Plugin BetterChat;
    private void Init();
    private void CommandFreeze(IPlayer player, string command, string[] args);
    private void CommandFreezeAll(IPlayer player, string command, string[] args);
    private void CommandUnfreeze(IPlayer player, string command, string[] args);
    private void CommandUnfreezeAll(IPlayer player, string command, string[] args);
    private void FreezePlayer(IPlayer player);
    private void UnfreezePlayer(IPlayer player);
    private object OnUserCommand(IPlayer player);
    private void OnUserConnected(IPlayer player);
    private object OnUserChat(IPlayer player);
    private object OnBetterChat(Dictionary<string, object> data);
    private void OnServerInitialized();
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

private class Configuration
{
    [JsonProperty("Block chat while frozen")]
    public bool BlockChat;
    [JsonProperty("Block commands while frozen")]
    public bool BlockCommands;
    [JsonProperty("Block damage while frozen")]
    public bool BlockDamage;
    [JsonProperty("Block movement while frozen")]
    public bool BlockMovement;
    [JsonProperty("Notify target when frozen")]
    public bool NotifyTarget;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## FreezeArrows by ColonBlow - Freezes players, NPCs, and rideable horses with arrows

```csharp
using Facepunch;
using Oxide.Core;
using System;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("FreezeArrows", "Colon Blow", "1.1.2")]
 class FreezeArrows : RustPlugin
{
     List<ulong> FrozenEntityList;
     Dictionary<ulong, string> GuiInfo;
     Dictionary<ulong, ShotArrowData> loadArrow;
     class ShotArrowData
    {
        public BasePlayer player;
        public int arrows;
        public bool arrowenabled;
    }

     void Loaded();
    private static PluginConfig config;
    private class PluginConfig
    {
        public GlobalArrowSettings FreezeArrowConfig { get; set; }
        public class GlobalArrowSettings
        {
            [JsonProperty(PropertyName = "Time - How long player is frozen when hit.")]
            public float FreezeTime { get; set; }
            [JsonProperty(PropertyName = "Time - Cooldown for freezing same player again.")]
            public float ReFreezeCooldown { get; set; }
            [JsonProperty(PropertyName = "Radius - The distance from impact players are effeted.")]
            public float FreezeRadius { get; set; }
            [JsonProperty(PropertyName = "Overlay - How long frozen overlay is shown when player is frozen")]
            public float FreezeOverlayTime { get; set; }
            [JsonProperty(PropertyName = "Arrows - Number of arrows on startup per player.")]
            public int StartingArrowCount { get; set; }
            [JsonProperty(PropertyName = "Arrows - Automatically reload another freeze arrow if player has them? (false will toggle freeze arrow off after shooting one) ")]
            public bool toggleFreezeReload { get; set; }
            [JsonProperty(PropertyName = "Overlay - Show freeze overlay when player is frozen ")]
            public bool useFreezeOverlay { get; set; }
            [JsonProperty(PropertyName = "Effects - Show hit explosion effect ?")]
            public bool showHitExplosionFX { get; set; }
            [JsonProperty(PropertyName = "Targets - Arrows will freeze players ?")]
            public bool freezePlayers { get; set; }
            [JsonProperty(PropertyName = "Targets - Arrows will freeze NPCs ?")]
            public bool freezeNPCs { get; set; }
            [JsonProperty(PropertyName = "Targets - Arrows will freeze Ridable Horses ?")]
            public bool freezeRideHorses { get; set; }
        }

        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
    [ChatCommand("freezearrow")]
     void cmdChatfreezearrow(BasePlayer player, string command, string[] args, int arrows, bool arrowenabled);
    [ChatCommand("freezecount")]
     void cmdChatfreezecount(BasePlayer player, string command, string[] args, int arrows);
     void FrozenGui(BasePlayer player);
     void OnPlayerAttack(BasePlayer player, HitInfo hitInfo);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo, int arrows);
     void Unload();
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     bool HasPermission(BasePlayer player, string perm);
     bool usingCorrectWeapon(BasePlayer player);
    public void findTarget(BasePlayer player, HitInfo hitInfo);
     void StopNPCMovement(BaseNpc npc);
     void freezeposition(BasePlayer player, Vector3 newPos);
     void DestroyCui(BasePlayer player);
}

 class ShotArrowData
{
    public BasePlayer player;
    public int arrows;
    public bool arrowenabled;
}

private class PluginConfig
{
    public GlobalArrowSettings FreezeArrowConfig { get; set; }
    public class GlobalArrowSettings
    {
        [JsonProperty(PropertyName = "Time - How long player is frozen when hit.")]
        public float FreezeTime { get; set; }
        [JsonProperty(PropertyName = "Time - Cooldown for freezing same player again.")]
        public float ReFreezeCooldown { get; set; }
        [JsonProperty(PropertyName = "Radius - The distance from impact players are effeted.")]
        public float FreezeRadius { get; set; }
        [JsonProperty(PropertyName = "Overlay - How long frozen overlay is shown when player is frozen")]
        public float FreezeOverlayTime { get; set; }
        [JsonProperty(PropertyName = "Arrows - Number of arrows on startup per player.")]
        public int StartingArrowCount { get; set; }
        [JsonProperty(PropertyName = "Arrows - Automatically reload another freeze arrow if player has them? (false will toggle freeze arrow off after shooting one) ")]
        public bool toggleFreezeReload { get; set; }
        [JsonProperty(PropertyName = "Overlay - Show freeze overlay when player is frozen ")]
        public bool useFreezeOverlay { get; set; }
        [JsonProperty(PropertyName = "Effects - Show hit explosion effect ?")]
        public bool showHitExplosionFX { get; set; }
        [JsonProperty(PropertyName = "Targets - Arrows will freeze players ?")]
        public bool freezePlayers { get; set; }
        [JsonProperty(PropertyName = "Targets - Arrows will freeze NPCs ?")]
        public bool freezeNPCs { get; set; }
        [JsonProperty(PropertyName = "Targets - Arrows will freeze Ridable Horses ?")]
        public bool freezeRideHorses { get; set; }
    }

    public static PluginConfig DefaultConfig();
}

public class GlobalArrowSettings
{
    [JsonProperty(PropertyName = "Time - How long player is frozen when hit.")]
    public float FreezeTime { get; set; }
    [JsonProperty(PropertyName = "Time - Cooldown for freezing same player again.")]
    public float ReFreezeCooldown { get; set; }
    [JsonProperty(PropertyName = "Radius - The distance from impact players are effeted.")]
    public float FreezeRadius { get; set; }
    [JsonProperty(PropertyName = "Overlay - How long frozen overlay is shown when player is frozen")]
    public float FreezeOverlayTime { get; set; }
    [JsonProperty(PropertyName = "Arrows - Number of arrows on startup per player.")]
    public int StartingArrowCount { get; set; }
    [JsonProperty(PropertyName = "Arrows - Automatically reload another freeze arrow if player has them? (false will toggle freeze arrow off after shooting one) ")]
    public bool toggleFreezeReload { get; set; }
    [JsonProperty(PropertyName = "Overlay - Show freeze overlay when player is frozen ")]
    public bool useFreezeOverlay { get; set; }
    [JsonProperty(PropertyName = "Effects - Show hit explosion effect ?")]
    public bool showHitExplosionFX { get; set; }
    [JsonProperty(PropertyName = "Targets - Arrows will freeze players ?")]
    public bool freezePlayers { get; set; }
    [JsonProperty(PropertyName = "Targets - Arrows will freeze NPCs ?")]
    public bool freezeNPCs { get; set; }
    [JsonProperty(PropertyName = "Targets - Arrows will freeze Ridable Horses ?")]
    public bool freezeRideHorses { get; set; }
}


```

---

## FreshStart by yoshi2 - Removes all entities when killed by another player

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using System;
using System.Linq;
using System.Collections;

Oxide.Plugins
[Info("FreshStart", "Yoshi", "1.1.1")]
[Description("Removes all entities when killed by another player")]
 class FreshStart : RustPlugin
{
    private const string PermissionName;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "remove corpses on death (true/false)")]
        public bool NoCorpses;
        [JsonProperty(PropertyName = "kill everything belonging to the player (true/false)")]
        public bool KillAllEntitiesOnDeath;
        [JsonProperty(PropertyName = "only run if the player is killed by another player (true/false)")]
        public bool LimitToPVPDeath;
        [JsonProperty(PropertyName = "wipes all their boxes on death (true/false)")]
        public bool DespawnLoot;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     void Init();
     object OnPlayerDeath(BasePlayer player, HitInfo info);
    public IEnumerator ClearEntities(BasePlayer player);
     void OnEntitySpawned(PlayerCorpse entity);
}

public class Configuration
{
    [JsonProperty(PropertyName = "remove corpses on death (true/false)")]
    public bool NoCorpses;
    [JsonProperty(PropertyName = "kill everything belonging to the player (true/false)")]
    public bool KillAllEntitiesOnDeath;
    [JsonProperty(PropertyName = "only run if the player is killed by another player (true/false)")]
    public bool LimitToPVPDeath;
    [JsonProperty(PropertyName = "wipes all their boxes on death (true/false)")]
    public bool DespawnLoot;
}


```

---

## FridgeFood by sami37 - Prevents food from being placed in box instead of fridge

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("FridgeFood", "sami37", "1.1.3")]
[Description("Prevent food from being placed in box instead of fridge.")]
public class FridgeFood : RustPlugin
{
    [PluginReference]
     Plugin Backpacks;
    private List<object> listFood;
    private List<object> listContainer;
     List<object> defaultLists;
    private List<object> unallowedContainer;
    private List<ulong> AdminDebug;
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
    private static BasePlayer GetPlayerFromContainer(ItemContainer container, Item item);
    private static ItemContainer GetRootContainer(Item item);
     void OnItemAddedToContainer(ItemContainer container, Item item);
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    [ChatCommand("fdebug")]
     void chatCommand(BasePlayer player, string cmd, string[] args);
}


```

---

## FriendlyFire by collectvood - Gives you the ability to enable or disable friendly fire player based

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Friendly Fire", "collect_vood", "1.1.2")]
[Description("Gives you the ability to enable or disable friendly fire player based")]
 class FriendlyFire : CovalencePlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin Clans;
    protected override void LoadDefaultMessages();
    private Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Command")]
        public string Command;
        [JsonProperty(PropertyName = "Player Default Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, bool> PlayerDefaultSettings;
        [JsonProperty(PropertyName = "Change friendly fire state permission")]
        public string ChangeStatePermission;
        [JsonProperty(PropertyName = "Send friendly fire messages")]
        public bool SendMessages;
        [JsonProperty(PropertyName = "Include check if friend")]
        public bool isFriendCheck;
        [JsonProperty(PropertyName = "Include check if team member")]
        public bool isTeamMemberCheck;
        [JsonProperty(PropertyName = "Include check if clan member")]
        public bool isClanMemberCheck;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private StoredData storedData;
    private Dictionary<string, PlayerSettings> allPlayerSettings { get; set; }
    private class PlayerSettings
    {
        public bool ff;
    }

    private class StoredData
    {
        public Dictionary<string, PlayerSettings> AllPlayerSettings { get; set; }
    }

    private void SaveData();
    private void OnServerSave();
    private void Unload();
    private void CreatePlayerSettings(IPlayer player);
    private void Init();
    private void OnServerInitialized();
     object OnPlayerAttack(BasePlayer player, HitInfo info);
    private void OnUserConnected(IPlayer player);
    private void CommandFriendlyFire(IPlayer player, string command, string[] args);
     string GetMessage(string key, IPlayer player, string[] args);
     bool HasPermission(IPlayer player);
     bool IsTeamMember(BasePlayer player, BasePlayer possibleMember);
     bool IsClanMember(BasePlayer player, BasePlayer possibleMember);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Command")]
    public string Command;
    [JsonProperty(PropertyName = "Player Default Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, bool> PlayerDefaultSettings;
    [JsonProperty(PropertyName = "Change friendly fire state permission")]
    public string ChangeStatePermission;
    [JsonProperty(PropertyName = "Send friendly fire messages")]
    public bool SendMessages;
    [JsonProperty(PropertyName = "Include check if friend")]
    public bool isFriendCheck;
    [JsonProperty(PropertyName = "Include check if team member")]
    public bool isTeamMemberCheck;
    [JsonProperty(PropertyName = "Include check if clan member")]
    public bool isClanMemberCheck;
}

private class PlayerSettings
{
    public bool ff;
}

private class StoredData
{
    public Dictionary<string, PlayerSettings> AllPlayerSettings { get; set; }
}


```

---

## FriendlyNature by  - Stop animals from eating each other

```csharp

Oxide.Plugins
[Info("Friendly Nature", "Bruno Puccio", "1.0.1")]
[Description("Stop animals from eating each other")]
public class FriendlyNature : RustPlugin
{
     object OnNpcTarget(BaseNpc npc, BaseEntity target);
}


```

---

## Friends by MrBlue - Friends system and API managing friend lists

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Friends", "Wulf", "3.1.3")]
[Description("Friends system and API managing friend lists")]
internal class Friends : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Friend list cache time (0 to disable)")]
        public int CacheTime;
        [JsonProperty("Maximum number of friends (0 to disable)")]
        public int MaxFriends;
        [JsonProperty("Use permission system")]
        public bool UsePermissions;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private readonly Dictionary<string, HashSet<string>> reverseData;
    private Dictionary<string, PlayerData> friendsData;
    private static readonly DateTime Epoch;
    private class PlayerData
    {
        public string Name { get; set; }
        public HashSet<string> Friends { get; set; }
        public Dictionary<string, int> Cached { get; set; }
        public bool IsCached(string playerId);
    }

    private void SaveData();
    protected override void LoadDefaultMessages();
    private const string permUse;
    private void Init();
    private bool AddFriend(string playerId, string friendId);
    private bool AddFriend(ulong playerId, ulong friendId);
    private void AddFriendReverse(string playerId, string friendId);
    private bool RemoveFriend(string playerId, string friendId);
    private bool RemoveFriend(ulong playerId, ulong friendId);
    private bool HasFriend(string playerId, string friendId);
    private bool HasFriend(ulong playerId, ulong friendId);
    private bool HadFriend(string playerId, string friendId);
    private bool HadFriend(ulong playerId, ulong friendId);
    private bool AreFriends(string playerId, string friendId);
    private bool AreFriends(ulong playerId, ulong friendId);
    private bool WereFriends(string playerId, string friendId);
    private bool WereFriends(ulong playerId, ulong friendId);
    private bool IsFriend(string playerId, string friendId);
    private bool IsFriend(ulong playerId, ulong friendId);
    private bool WasFriend(string playerId, string friendId);
    private bool WasFriend(ulong playerId, ulong friendId);
    private int GetMaxFriends();
    private string[] GetFriends(string playerId);
    private ulong[] GetFriends(ulong playerId);
    private string[] GetFriendList(string playerId);
    private string[] GetFriendList(ulong playerId);
    private string[] IsFriendOf(string playerId);
    private ulong[] IsFriendOf(ulong playerId);
    private void CommandFriend(IPlayer player, string command, string[] args);
    private void SendHelpText(object obj);
    private string FindFriend(string nameOrId);
    private PlayerData GetPlayerData(string playerId);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

public class Configuration
{
    [JsonProperty("Friend list cache time (0 to disable)")]
    public int CacheTime;
    [JsonProperty("Maximum number of friends (0 to disable)")]
    public int MaxFriends;
    [JsonProperty("Use permission system")]
    public bool UsePermissions;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class PlayerData
{
    public string Name { get; set; }
    public HashSet<string> Friends { get; set; }
    public Dictionary<string, int> Cached { get; set; }
    public bool IsCached(string playerId);
}


```

---

## FuelAlarm by lunedi - Triggers an alarm when fuel is below a certain level

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Fuel Alarm", "Oryx", "1.0.3")]
[Description("Trigger alarm sound when fuel is below a certin level.")]
 class FuelAlarm : RustPlugin
{
    private string perm;
    private string prefabSound;
    private int threshold;
    private bool usePerm;
    private float timeInterval;
    private List<VehicleCache> cacheList;
    private List<ulong> disabledList;
     ConfigData configData;
    public class VehicleCache
    {
        public List<BasePlayer> playerlist;
        public BaseMountable entity;
        public int getFuel();
    }

    protected override void LoadDefaultConfig();
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings settings { get; set; }
        [JsonProperty(PropertyName = "Permission Settings")]
        public PermissonSettings permissionSettings { get; set; }
    }

    private class Settings
    {
        [JsonProperty(PropertyName = "Perfab Sound")]
        public string prefabsound { get; set; }
        [JsonProperty(PropertyName = "Threshold")]
        public int threshold { get; set; }
        [JsonProperty(PropertyName = "TimeInterval")]
        public float timeinterval { get; set; }
    }

    private class PermissonSettings
    {
        [JsonProperty(PropertyName = "Use Permission")]
        public bool useperm { get; set; }
    }

    private void loadVariables();
     void OnEntityMounted(BaseMountable entity, BasePlayer player);
     void OnEntityDismounted(BaseMountable entity, BasePlayer player);
     void OnServerInitialized();
    private void Init();
     void Unload();
     object OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnEntityKill(BaseNetworkable entity);
    public VehicleCache GetVehcileCache(BaseMountable entity);
    public bool IsVehicleInCache(BaseMountable entity);
    public VehicleCache IsPlayerInCache(BasePlayer player);
    public void RemovePlayer(BaseMountable entity, BasePlayer player);
    public void RemoveVehicleFromCache(BaseMountable entity);
    public void AddPlayer(BaseMountable entity, BasePlayer player);
    public void AlarmManager();
    private void CmdFuelAlarm(BasePlayer player, string command, string[] args);
    public void SaveData();
    public void ReadData();
    public void Send(BasePlayer player, string key);
    protected override void LoadDefaultMessages();
}

public class VehicleCache
{
    public List<BasePlayer> playerlist;
    public BaseMountable entity;
    public int getFuel();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings settings { get; set; }
    [JsonProperty(PropertyName = "Permission Settings")]
    public PermissonSettings permissionSettings { get; set; }
}

private class Settings
{
    [JsonProperty(PropertyName = "Perfab Sound")]
    public string prefabsound { get; set; }
    [JsonProperty(PropertyName = "Threshold")]
    public int threshold { get; set; }
    [JsonProperty(PropertyName = "TimeInterval")]
    public float timeinterval { get; set; }
}

private class PermissonSettings
{
    [JsonProperty(PropertyName = "Use Permission")]
    public bool useperm { get; set; }
}


```

---

## FuelGauge by lunedi - HUD for amount of fuel when driving a vehicle

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("Fuel Gauge", "shaigannn", "0.8.2")]
[Description("HUD for amount of fuel when riding a vehicle")]
public class FuelGauge : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private const string perm;
    private List<VehicleCache> vehicles;
     ConfigData configData;
    private bool useIcon;
    private string imageURL;
    private string dock;
    private bool onlyDriver;
    private string backgroundColor;
    private float backgroundTransparency;
    private string gaugeType;
    private readonly List<string> driverSeats;
    public class VehicleCache
    {
        public BaseMountable entity;
        public BasePlayer player;
        public int GetFuelAmount();
        public static bool HasFuelSystem(BaseMountable entity);
    }

    protected override void LoadDefaultConfig();
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings settings { get; set; }
        [JsonProperty(PropertyName = "Display")]
        public Display display { get; set; }
    }

    private class Settings
    {
        [JsonProperty(PropertyName = "Dock")]
        public string Dock { get; set; }
        [JsonProperty(PropertyName = "Only Display to Driver")]
        public bool DriverOnly { get; set; }
    }

    private class Display
    {
        [JsonProperty(PropertyName = "Use Icon")]
        public bool UseIcon { get; set; }
        [JsonProperty(PropertyName = "Image URL")]
        public string ImageURL { get; set; }
        [JsonProperty(PropertyName = "Background Color")]
        public string BackgroundColor { get; set; }
        [JsonProperty(PropertyName = "Background Transparency")]
        public float Transparency { get; set; }
    }

    private void LoadVariables();
     void Init();
     void OnServerInitialized();
    private void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnEntityMounted(BaseMountable entity, BasePlayer player);
     void OnEntityDismounted(BaseMountable entity, BasePlayer player);
     void OnEntityKill(BaseNetworkable entity);
    static class UIHelper
    {
        public static CuiElementContainer NewCuiElement(string name, string color, string aMin, string aMax);
        public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        public static void CreateLabel(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align, string color);
        public static void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        public static string HexToRGBA(string hex, float alpha);
    }

    public void CreateUI(VehicleCache vehicle);
    public void UpdateUI(VehicleCache vehicle);
    public void UIManager();
    public void DestoryUI(BasePlayer player);
    public void DestoryAllUI();
    public void RemoveVehicleByPlayer(BasePlayer player);
    public bool GetPlayer(BasePlayer player);
    private string GetImage(string fileName, ulong skin);
    public string GetMinDock();
    public string GetMaxDock();
}

public class VehicleCache
{
    public BaseMountable entity;
    public BasePlayer player;
    public int GetFuelAmount();
    public static bool HasFuelSystem(BaseMountable entity);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings settings { get; set; }
    [JsonProperty(PropertyName = "Display")]
    public Display display { get; set; }
}

private class Settings
{
    [JsonProperty(PropertyName = "Dock")]
    public string Dock { get; set; }
    [JsonProperty(PropertyName = "Only Display to Driver")]
    public bool DriverOnly { get; set; }
}

private class Display
{
    [JsonProperty(PropertyName = "Use Icon")]
    public bool UseIcon { get; set; }
    [JsonProperty(PropertyName = "Image URL")]
    public string ImageURL { get; set; }
    [JsonProperty(PropertyName = "Background Color")]
    public string BackgroundColor { get; set; }
    [JsonProperty(PropertyName = "Background Transparency")]
    public float Transparency { get; set; }
}

static class UIHelper
{
    public static CuiElementContainer NewCuiElement(string name, string color, string aMin, string aMax);
    public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    public static void CreateLabel(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align, string color);
    public static void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    public static string HexToRGBA(string hex, float alpha);
}


```

---

## FurnaceSplitter by FastBurst - Splits up ores into equal stacks when you put them into furnaces

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Furnace Splitter", "FastBurst", "2.5.2")]
[Description("Splits up resources in furnaces automatically and shows useful furnace information")]
public class FurnaceSplitter : RustPlugin
{
    [PluginReference]
    private Plugin UIScaleManager;
    private class OvenSlot
    {
        public Item Item;
        public int? Position;
        public int Index;
        public int DeltaAmount;
    }

    public class OvenInfo
    {
        public float ETA;
        public float FuelNeeded;
    }

    private class StoredData
    {
        public Dictionary<ulong, PlayerOptions> AllPlayerOptions { get; set; }
    }

    private class PlayerOptions
    {
        public bool Enabled;
        public Dictionary<string, int> TotalStacks;
    }

    private StoredData storedData;
    private Dictionary<ulong, PlayerOptions> allPlayerOptions { get; set; }
    private Dictionary<string, int> initialStackOptions;
    private PluginConfig config;
    private const string permUse;
    private readonly Dictionary<ulong, string> openUis;
    private readonly Dictionary<BaseOven, List<BasePlayer>> looters;
    private readonly Stack<BaseOven> queuedUiUpdates;
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnServerSave();
    private void SaveData();
    private void InitPlayer(BasePlayer player);
    private void OnTick();
    public OvenInfo GetOvenInfo(BaseOven oven);
    private void Unload();
    private bool GetEnabled(BasePlayer player);
    private void SetEnabled(BasePlayer player, bool enabled);
    private bool IsSlotCompatible(Item item, BaseOven oven, ItemDefinition itemDefinition);
    private void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
    private List<BasePlayer> GetLooters(BaseOven oven);
    private void AddLooter(BaseOven oven, BasePlayer player);
    private void RemoveLooter(BaseOven oven, BasePlayer player);
    private object CanMoveItem(Item item, PlayerInventory inventory, ItemContainerId targetContainerId, int targetSlotIndex, int splitAmount);
    private MoveResult MoveSplitItem(Item item, BaseOven oven, int totalSlots, int splitAmount);
    private void AutoAddFuel(PlayerInventory playerInventory, BaseOven oven);
    private int FindMatchingSlotIndex(BaseOven oven, ItemContainer container, Item existingItem, ItemDefinition itemType, List<int> indexBlacklist);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private void OnEntityKill(BaseNetworkable networkable);
    private void OnOvenToggle(BaseOven oven, BasePlayer player);
    private void CreateUiIfFurnaceOpen(BasePlayer player);
    private CuiElementContainer CreateUi(BasePlayer player, BaseOven oven, OvenInfo ovenInfo);
    private string FormatTime(float totalSeconds);
    private float GetTotalSmeltTime(BaseOven oven);
    private bool CanCook(ItemModCookable cookable, BaseOven oven);
    private float GetSmeltTime(ItemModCookable cookable, int amount);
    private int? GetTotalStacksOption(BasePlayer player, BaseOven oven);
    private void DestroyUI(BasePlayer player);
    private void DestroyOvenUI(BaseOven oven);
    private PluginConfig.OvenConfig GetOvenConfig(string ovenShortname);
    private bool IsOvenCompatible(BaseOven oven);
    [ChatCommand("fs")]
     void cmdToggle(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("furnacesplitter.enabled")]
    private void ConsoleCommand_Toggle(ConsoleSystem.Arg arg);
    [ConsoleCommand("furnacesplitter.totalstacks")]
    private void ConsoleCommand_TotalStacks(ConsoleSystem.Arg arg);
    [ConsoleCommand("furnacesplitter.trim")]
    private void ConsoleCommand_Trim(ConsoleSystem.Arg arg);
    private bool HasPermission(BasePlayer player);
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        public Vector2 UiPosition;
        public bool savePlayerData;
        public SortedDictionary<string, OvenConfig> ovens;
        public class OvenConfig
        {
            public bool enabled;
            public float fuelMultiplier;
        }

    }

    private class Vector2Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    [HookMethod("MoveSplitItem")]
    public string Hook_MoveSplitItem(Item item, BaseOven oven, int totalSlots, int splitAmount);
    [HookMethod("GetOvenInfo")]
    public JObject Hook_GetOvenInfo(BaseOven oven);
}

private class OvenSlot
{
    public Item Item;
    public int? Position;
    public int Index;
    public int DeltaAmount;
}

public class OvenInfo
{
    public float ETA;
    public float FuelNeeded;
}

private class StoredData
{
    public Dictionary<ulong, PlayerOptions> AllPlayerOptions { get; set; }
}

private class PlayerOptions
{
    public bool Enabled;
    public Dictionary<string, int> TotalStacks;
}

private class PluginConfig
{
    public Vector2 UiPosition;
    public bool savePlayerData;
    public SortedDictionary<string, OvenConfig> ovens;
    public class OvenConfig
    {
        public bool enabled;
        public float fuelMultiplier;
    }

}

public class OvenConfig
{
    public bool enabled;
    public float fuelMultiplier;
}

private class Vector2Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## FurnaceStopper by  - Stops smelting in furnaces if there are nothing to smelt

```csharp
using System.Linq;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Furnace Stopper", "Orange", "1.0.7")]
[Description("Stops smelting in furnaces if there are nothing to smelt")]
public class FurnaceStopper : RustPlugin
{
    private const float checkRate;
    private const string permUse;
    private void Init();
    private void Unload();
    private void OnOvenToggle(BaseOven entity, BasePlayer player);
    private class StopperScript : MonoBehaviour
    {
        private BaseOven oven;
        private void Awake();
        public void StateChanged();
        private void Check();
    }

}

private class StopperScript : MonoBehaviour
{
    private BaseOven oven;
    private void Awake();
    public void StateChanged();
    private void Check();
}


```

---

## GameMenu by misticos - Create your own GUI menu with buttons and title

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Game Menu", "Iv Misticos", "1.0.10")]
[Description("Create your own GUI menu with buttons and title.")]
 class GameMenu : RustPlugin
{
    private static CuiButton _backgroundButton;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Distance between buttons")]
        public float BetweenButtons;
        [JsonProperty(PropertyName = "FadeIn and FadeOut time")]
        public float FadeTime;
        [JsonProperty(PropertyName = "List of menus", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ConfigMenu> Menus;
    }

    private class ConfigMenu
    {
        [JsonIgnore]
        public CuiPanel Menu;
        [JsonIgnore]
        public CuiLabel MenuText;
        [JsonProperty(PropertyName = "Menu title")]
        public string Name;
        [JsonProperty(PropertyName = "Menu title color")]
        public string NameColor;
        [JsonProperty(PropertyName = "Menu title size")]
        public short NameSize;
        [JsonProperty(PropertyName = "Menu title height")]
        public float NameHeight;
        [JsonProperty(PropertyName = "Menu permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Chat command to open the menu")]
        public string CommmandChat;
        [JsonProperty(PropertyName = "Console command to open the menu")]
        public string CommmandConsole;
        [JsonProperty(PropertyName = "Background color")]
        public string BackgroundColor;
        [JsonProperty(PropertyName = "Menu buttons", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ConfigButton> Buttons;
    }

    private class ConfigButton
    {
        [JsonIgnore]
        public CuiButton Button;
        [JsonProperty(PropertyName = "Button color")]
        public string ButtonColor;
        [JsonProperty(PropertyName = "Text color")]
        public string TextColor;
        [JsonProperty(PropertyName = "Text size")]
        public short TextSize;
        [JsonProperty(PropertyName = "Button width")]
        public float ButtonWidth;
        [JsonProperty(PropertyName = "Button height")]
        public float ButtonHeight;
        [JsonProperty(PropertyName = "Button text")]
        public string Text;
        [JsonProperty(PropertyName = "Execute chat (true) or console (false) command")]
        public bool IsChatCommand;
        [JsonProperty(PropertyName = "Executing command")]
        public string Command;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void Init();
    private void Unload();
    protected override void LoadDefaultMessages();
    private void ShowUI(BasePlayer player, ConfigMenu menu);
    private void SendCommand(Connection conn, string[] args, bool isChat);
    private bool CanUse(BasePlayer player, string perm);
    private string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Distance between buttons")]
    public float BetweenButtons;
    [JsonProperty(PropertyName = "FadeIn and FadeOut time")]
    public float FadeTime;
    [JsonProperty(PropertyName = "List of menus", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ConfigMenu> Menus;
}

private class ConfigMenu
{
    [JsonIgnore]
    public CuiPanel Menu;
    [JsonIgnore]
    public CuiLabel MenuText;
    [JsonProperty(PropertyName = "Menu title")]
    public string Name;
    [JsonProperty(PropertyName = "Menu title color")]
    public string NameColor;
    [JsonProperty(PropertyName = "Menu title size")]
    public short NameSize;
    [JsonProperty(PropertyName = "Menu title height")]
    public float NameHeight;
    [JsonProperty(PropertyName = "Menu permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Chat command to open the menu")]
    public string CommmandChat;
    [JsonProperty(PropertyName = "Console command to open the menu")]
    public string CommmandConsole;
    [JsonProperty(PropertyName = "Background color")]
    public string BackgroundColor;
    [JsonProperty(PropertyName = "Menu buttons", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ConfigButton> Buttons;
}

private class ConfigButton
{
    [JsonIgnore]
    public CuiButton Button;
    [JsonProperty(PropertyName = "Button color")]
    public string ButtonColor;
    [JsonProperty(PropertyName = "Text color")]
    public string TextColor;
    [JsonProperty(PropertyName = "Text size")]
    public short TextSize;
    [JsonProperty(PropertyName = "Button width")]
    public float ButtonWidth;
    [JsonProperty(PropertyName = "Button height")]
    public float ButtonHeight;
    [JsonProperty(PropertyName = "Button text")]
    public string Text;
    [JsonProperty(PropertyName = "Execute chat (true) or console (false) command")]
    public bool IsChatCommand;
    [JsonProperty(PropertyName = "Executing command")]
    public string Command;
}


```

---

## GameServerTools by Disney - Game Server Tools is an admin management platform. It adds account linking, bans, reports and more!

```csharp
using ConVar;
using Facepunch.Extend;
using Facepunch.Math;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Game Server Tools", "mrcameron999", "1.0.7")]
[Description("Adds several adamin tools for use with gameservertools.com")]
public class GameServerTools : CovalencePlugin
{
    private Dictionary<string, ApprovedCachedPlayer> _approvedCachedJoins;
    private readonly Dictionary<IPlayer, CachedPlayer> _cachedJoins;
    private readonly Dictionary<string, string> _headers;
    private readonly string _connection;
    private readonly float timeout;
    private string _linkedGroupName;
    private string _nitroGroupName;
    private int _port;
    private bool _showClaimMessage;
    private bool _loggingEnabled;
    private bool _disableBanCtrl;
    private Dictionary<ulong, List<string>> messages;
    private void Init();
    private void LoadConfigData();
    private void OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private void OnPlayerReported(BasePlayer reporter, string targetName, string targetId, string subject, string message, string type);
    private void OnServerInitialized(bool initial);
    private void OnServerShutdown();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerBanned(string name, ulong id, string address, string reason);
    private void OnUserApprove(Network.Connection connection);
    private void OnUserConnected(IPlayer player);
    private void CheckForBans(Network.Connection connection, ulong ownerId);
    private void SendReport(Dictionary<string, object> parameters);
    private void ClearAllPlayerConnections();
    private void SubmitNewBan(AddBanClass newBan, IPlayer admin, Action<AddBanClass> successCallBack);
    private void UserLeftServer(Dictionary<string, object> parameters);
    private void UserConnectedToServer(IPlayer player);
    private void FetchDiscordLinkData(IPlayer player);
    private void HandleDiscordLinkerConnect(LinkModel linkData, IPlayer player);
    private ReportType GetTypeIdFromType(string type);
    private void MessageAllPlayers(string message);
    private bool TryGetBanExpiry(string arg, int n, IPlayer iplayer, long expiry, string durationSuffix);
    private long GetTimestamp(string arg, int iArg, long def);
    [Command("Near")]
    private void FindNear(IPlayer iplayer, string command, string[] args);
    [Command("checklink")]
    private void CheckLinkCommand(IPlayer iplayer, string command, string[] args);
    [Command("nitro", "linked")]
    private void NitroCheck(IPlayer iplayer, string command, string[] args);
    private void OverrideBanCommand(IPlayer iplayer, string command, string[] args);
    [Command("AddAllBans")]
    private void AddAllBans(IPlayer iplayer, string command, string[] args);
    public class LinkModel
    {
        public int LinkId { get; set; }
        public long SteamId { get; set; }
        public long DiscordId { get; set; }
        public int OrgId { get; set; }
        public DateTime LinkDate { get; set; }
        public bool InDiscord { get; set; }
        public bool ClaimedRewards { get; set; }
        public int? NitroBoostId { get; set; }
        public bool NitroBoosted { get; set; }
    }

    private class CachedPlayer
    {
        public CachedPlayer();
        public DateTime TimeOfAdd { get; set; }
    }

    private class AddBanClass
    {
        public string SteamId { get; set; }
        public string Reason { get; set; }
        public DateTime? ExpireTime { get; set; }
        public int OrgId { get; set; }
        public int? ServerId { get; set; }
        public string BannedBy { get; set; }
        public string NiceBanId { get; set; }
        public int ServerPort { get; set; }
        public bool DontSendRconKick { get; set; }
    }

    private class ApprovedCachedPlayer
    {
        public string reason { get; set; }
        public DateTime timeOfAdd { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
}

public class LinkModel
{
    public int LinkId { get; set; }
    public long SteamId { get; set; }
    public long DiscordId { get; set; }
    public int OrgId { get; set; }
    public DateTime LinkDate { get; set; }
    public bool InDiscord { get; set; }
    public bool ClaimedRewards { get; set; }
    public int? NitroBoostId { get; set; }
    public bool NitroBoosted { get; set; }
}

private class CachedPlayer
{
    public CachedPlayer();
    public DateTime TimeOfAdd { get; set; }
}

private class AddBanClass
{
    public string SteamId { get; set; }
    public string Reason { get; set; }
    public DateTime? ExpireTime { get; set; }
    public int OrgId { get; set; }
    public int? ServerId { get; set; }
    public string BannedBy { get; set; }
    public string NiceBanId { get; set; }
    public int ServerPort { get; set; }
    public bool DontSendRconKick { get; set; }
}

private class ApprovedCachedPlayer
{
    public string reason { get; set; }
    public DateTime timeOfAdd { get; set; }
}


```

---

## GameTipAnnouncements by redBDGR - Sends announcements or messages to players as game tips

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Game Tip Announcements", "redBDGR", "1.0.2")]
[Description("Send notifications to players as gametips")]
 class GameTipAnnouncements : RustPlugin
{
    private bool Changed;
    private const string permissionNameADMIN;
    private float defaultLengnth;
    private void Init();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    [ChatCommand("sendgt")]
    private void SendGameTipCMD(BasePlayer player, string command, string[] args);
    [ChatCommand("sendgtall")]
    private void SendGameTipAllCMD(BasePlayer player, string command, string[] args);
    [ConsoleCommand("sendgt")]
    private void SendGameTipAllCONSOLECMD(ConsoleSystem.Arg args);
    [ConsoleCommand("sendgtall")]
    private void SendGameTipCONSOLSECMD(ConsoleSystem.Arg args);
    private void CreateGameTip(string text, BasePlayer player, float length);
    private void CreateGameTipAll(string text, float length);
    private object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string id);
}


```

---

## GameTipAPI by S0N0FBISCUIT - API for displaying queued gametips to players

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("GameTipAPI", "S0N_0F_BISCUIT", "1.0.0", ResourceId = 2759)]
[Description("API for displaying queued gametips to players.")]
 class GameTipAPI : RustPlugin
{
     class Message
    {
        [JsonProperty(PropertyName = "Text")]
        public string text;
        [JsonProperty(PropertyName = "Duration")]
        public float duration;
    }

     class PlayerTips
    {
        public BasePlayer player;
        public Queue<Message> queue;
        public bool active;
    }

     class ScheduledTip
    {
        [JsonProperty(PropertyName = "Message")]
        public Message message;
        [JsonProperty(PropertyName = "Period")]
        public float period;
        [JsonProperty(PropertyName = "Mandatory")]
        public bool mandatory;
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled;
    }

     class ConfigData
    {
        [JsonProperty(PropertyName = "Scheduled Tips")]
        public List<ScheduledTip> ScheduledTips;
    }

     class StoredData
    {
        public List<ulong> blacklist;
    }

    private ConfigData config;
    private StoredData data;
    private List<PlayerTips> gameTips;
    private new void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadConfigData();
    private void LoadData();
    private void SaveData();
    private void Init();
     void OnServerInitialized();
     void Unload();
    [ChatCommand("hidetips")]
     void HideGameTips(BasePlayer player, string command, string[] args);
    [ChatCommand("showtips")]
     void ShowGameTips(BasePlayer player, string command, string[] args);
     void ShowGameTip(BasePlayer player, string message, float duration, bool mandatory);
     void BroadcastGameTip(string message, float duration, bool mandatory);
    private void AddScheduledTip(ScheduledTip tip);
    private void Display(PlayerTips tips);
    private string Lang(string key, string userId, object[] args);
}

 class Message
{
    [JsonProperty(PropertyName = "Text")]
    public string text;
    [JsonProperty(PropertyName = "Duration")]
    public float duration;
}

 class PlayerTips
{
    public BasePlayer player;
    public Queue<Message> queue;
    public bool active;
}

 class ScheduledTip
{
    [JsonProperty(PropertyName = "Message")]
    public Message message;
    [JsonProperty(PropertyName = "Period")]
    public float period;
    [JsonProperty(PropertyName = "Mandatory")]
    public bool mandatory;
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled;
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Scheduled Tips")]
    public List<ScheduledTip> ScheduledTips;
}

 class StoredData
{
    public List<ulong> blacklist;
}


```

---

## GatherAway by Notchu - Automatically hits weaknesses when harvesting trees and ore

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

[Info("Gather Away", "Notchu", "1.1.2")]
[Description("Automatically hits weak spot while gathering resources")]
internal class GatherAway : CovalencePlugin
{
    private class Configuration
    {
        [JsonProperty("Automatically gather X Markers")]
        public bool XMarkerGather;
        [JsonProperty("Automatically gather Ore Weak spots")]
        public bool OreWeakSpotsGather;
        [JsonProperty("BlackList - false, whitelist - true")]
        public bool ItemList;
        [JsonProperty("BlackList(false)")]
        public List<string> BlackList;
        [JsonProperty("Whitelist(true)")]
        public List<string> WhiteList;
        [JsonProperty("ignore blacklist during period")]
        public bool IgnoreBlackListDuringPeriod;
        [JsonProperty("Give players ability to turn off plugin for themselves")]
        public bool TogglePluginByPlayer;
        [JsonProperty("period start hour")]
        public int PeriodStart;
        [JsonProperty("period end hour")]
        public int PeriodEnd;
    }

    private Configuration _config;
    private HashSet<string> disabledPlayers;
    private const string DataFile;
    private void LoadData();
    private void SaveData();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void Init();
     void Unload();
     void OnServerSave();
     object OnPlayerAttack(BasePlayer player, HitInfo info);
     bool? OnTreeMarkerHit(TreeEntity tree, HitInfo info);
    private bool IsWeaponBlacklisted(HitInfo info);
    private bool IsWeaponWhitelisted(HitInfo info);
     void HitWeakSpotOnOre(HitInfo info, string playerId, OreResourceEntity ore);
     bool IsInPeriod();
     void ToggleGatherAway(IPlayer player, string command, string[] args);
}

private class Configuration
{
    [JsonProperty("Automatically gather X Markers")]
    public bool XMarkerGather;
    [JsonProperty("Automatically gather Ore Weak spots")]
    public bool OreWeakSpotsGather;
    [JsonProperty("BlackList - false, whitelist - true")]
    public bool ItemList;
    [JsonProperty("BlackList(false)")]
    public List<string> BlackList;
    [JsonProperty("Whitelist(true)")]
    public List<string> WhiteList;
    [JsonProperty("ignore blacklist during period")]
    public bool IgnoreBlackListDuringPeriod;
    [JsonProperty("Give players ability to turn off plugin for themselves")]
    public bool TogglePluginByPlayer;
    [JsonProperty("period start hour")]
    public int PeriodStart;
    [JsonProperty("period end hour")]
    public int PeriodEnd;
}


```

---

## GatherBonuses by  - Changes ore gather bonuses

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Gather bonuses", "wazzzup", "0.0.1")]
 class GatherBonuses : RustPlugin
{
    private DynamicConfigFile config;
     Dictionary<string,int> rates;
    private bool Changed;
     void LoadVariables();
    protected override void LoadDefaultConfig();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void Loaded();
    private object OnDispenserBonus(ResourceDispenser resource, BasePlayer player, Item obj1);
}


```

---

## GatherControl by CaseMan - Control gather rates by day and night with permissions

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("GatherControl", "CaseMan", "1.8.2", ResourceId = 2477)]
[Description("Control gather rates by day and night with permissions")]
 class GatherControl : RustPlugin
{
    [PluginReference]
     Plugin GUIAnnouncements;
     bool IsDay;
     bool UseZeroIndexForDefaultGroup;
     bool UseMessageBroadcast;
     bool UseGUIAnnouncements;
     bool AdminMode;
     string BannerColor;
     string TextColor;
    public Dictionary<ulong, int> Temp;
     float Sunrise;
     float Sunset;
     float DayRateMultStaticQuarry;
     float NightRateMultStaticQuarry;
     float DayRateMultExcavator;
     float NightRateMultExcavator;
     string PLPerm;
     string AdmPerm;
     string BypassPerm;
     class PermData
    {
        public Dictionary<int, PermGroups> PermissionsGroups;
        public PermData();
    }

     class PermGroups
    {
        public float DayRateMultQuarry;
        public float DayRateMultPickup;
        public float DayRateMultResource;
        public float DayRateMultResourceBonus;
        public float DayRateMultResourceHQM;
        public float DayRateMultCropGather;
        public float NightRateMultQuarry;
        public float NightRateMultPickup;
        public float NightRateMultResource;
        public float NightRateMultResourceBonus;
        public float NightRateMultResourceHQM;
        public float NightRateMultCropGather;
        public Dictionary<string, string> CustomRateMultQuarry;
        public Dictionary<string, string> CustomRateMultPickup;
        public Dictionary<string, string> CustomRateMultResource;
        public Dictionary<string, string> CustomRateMultResourceBonus;
        public Dictionary<string, string> CustomRateMultCropGather;
        public Dictionary<string, string> ToolMultiplier;
        public string PermGroup;
        public PermGroups();
    }

     PermData permData;
     void Init();
     void LoadDefaultData();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
    private void CheckDay();
    private void CheckPlayers();
    private void CheckPlayer(BasePlayer player);
    private int CheckPlayerPerm(BasePlayer player, int index);
    private void CustomList(Item item, string str);
    private void CustomList(ItemAmount item, string str);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
     void OnQuarryGather(MiningQuarry quarry, Item item);
    private void OnExcavatorGather(ExcavatorArm excavator, Item item);
     void OnGrowableGathered(GrowableEntity plant, Item item, BasePlayer player);
    private int CheckPlayerPerms(BasePlayer player);
    private void GatherMultiplier(Item item, float daymult, float nightmult);
    private void GatherMultiplier(ItemAmount item, float daymult, float nightmult);
     void OnTick();
     void OnUserPermissionGranted(string id, string permis);
     void OnUserPermissionRevoked(string id, string permis);
     void OnUserGroupAdded(string id, string name);
     void OnUserGroupRemoved(string id, string name);
     void OnGroupPermissionGranted(string name, string permis);
     void OnGroupPermissionRevoked(string name, string permis);
    private void OnChangePermsGroup(string permis);
    private void OnChangePermsUser(string id, string permis);
    private void OnChangeUserGroup(string id);
    [ChatCommand("showrate")]
     void ShowRate(BasePlayer player, string command, string[] args);
    [ConsoleCommand("showrate")]
    private void conShowRate(ConsoleSystem.Arg arg);
     T GetConfig(string name, T defaultValue);
     void MessageToAll(string key);
    private string GatherInfo(BasePlayer player, int gr);
    private void ParseFromString(string str, float day, float night);
}

 class PermData
{
    public Dictionary<int, PermGroups> PermissionsGroups;
    public PermData();
}

 class PermGroups
{
    public float DayRateMultQuarry;
    public float DayRateMultPickup;
    public float DayRateMultResource;
    public float DayRateMultResourceBonus;
    public float DayRateMultResourceHQM;
    public float DayRateMultCropGather;
    public float NightRateMultQuarry;
    public float NightRateMultPickup;
    public float NightRateMultResource;
    public float NightRateMultResourceBonus;
    public float NightRateMultResourceHQM;
    public float NightRateMultCropGather;
    public Dictionary<string, string> CustomRateMultQuarry;
    public Dictionary<string, string> CustomRateMultPickup;
    public Dictionary<string, string> CustomRateMultResource;
    public Dictionary<string, string> CustomRateMultResourceBonus;
    public Dictionary<string, string> CustomRateMultCropGather;
    public Dictionary<string, string> ToolMultiplier;
    public string PermGroup;
    public PermGroups();
}


```

---

## GatherManager by Ryan - Increases the amount of items gained from gathering resources

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Gather Manager", "Mughisi", "2.2.78")]
[Description("Increases the amount of items gained from gathering resources")]
 class GatherManager : RustPlugin
{
    private bool configChanged;
    private const string DefaultChatPrefix;
    private const string DefaultChatPrefixColor;
    public string ChatPrefix { get; set; }
    public string ChatPrefixColor { get; set; }
    private static readonly Dictionary<string, object> DefaultGatherResourceModifiers;
    private static readonly Dictionary<string, object> DefaultGatherDispenserModifiers;
    private static readonly Dictionary<string, object> DefaultQuarryResourceModifiers;
    private static readonly Dictionary<string, object> DefaultPickupResourceModifiers;
    private static readonly Dictionary<string, object> DefaultSurveyResourceModifiers;
    private const float DefaultMiningQuarryResourceTickRate;
    private const float DefaultExcavatorResourceTickRate;
    private const float DefaultExcavatorTimeForFullResources;
    private const float DefaultExcavatorBeltSpeedMax;
    public Dictionary<string, float> GatherResourceModifiers { get; set; }
    public Dictionary<string, float> GatherDispenserModifiers { get; set; }
    public Dictionary<string, float> QuarryResourceModifiers { get; set; }
    public Dictionary<string, float> ExcavatorResourceModifiers { get; set; }
    public Dictionary<string, float> PickupResourceModifiers { get; set; }
    public Dictionary<string, float> SurveyResourceModifiers { get; set; }
    public float MiningQuarryResourceTickRate { get; set; }
    public float ExcavatorResourceTickRate { get; set; }
    public float ExcavatorTimeForFullResources { get; set; }
    public float ExcavatorBeltSpeedMax { get; set; }
    private const string DefaultNotAllowed;
    private const string DefaultInvalidArgumentsGather;
    private const string DefaultInvalidArgumentsDispenser;
    private const string DefaultInvalidArgumentsSpeed;
    private const string DefaultInvalidModifier;
    private const string DefaultInvalidSpeed;
    private const string DefaultModifyResource;
    private const string DefaultModifyResourceRemove;
    private const string DefaultModifySpeed;
    private const string DefaultInvalidResource;
    private const string DefaultModifyDispenser;
    private const string DefaultInvalidDispenser;
    private const string DefaultHelpText;
    private const string DefaultHelpTextPlayer;
    private const string DefaultHelpTextAdmin;
    private const string DefaultHelpTextPlayerGains;
    private const string DefaultHelpTextPlayerMiningQuarrySpeed;
    private const string DefaultHelpTextPlayerDefault;
    private const string DefaultDispensers;
    private const string DefaultCharges;
    private const string DefaultQuarries;
    private const string DefaultExcavators;
    private const string DefaultPickups;
    public string NotAllowed { get; set; }
    public string InvalidArgumentsGather { get; set; }
    public string InvalidArgumentsDispenser { get; set; }
    public string InvalidArgumentsSpeed { get; set; }
    public string InvalidModifier { get; set; }
    public string InvalidSpeed { get; set; }
    public string ModifyResource { get; set; }
    public string ModifyResourceRemove { get; set; }
    public string ModifySpeed { get; set; }
    public string InvalidResource { get; set; }
    public string ModifyDispenser { get; set; }
    public string InvalidDispenser { get; set; }
    public string HelpText { get; set; }
    public string HelpTextPlayer { get; set; }
    public string HelpTextAdmin { get; set; }
    public string HelpTextPlayerGains { get; set; }
    public string HelpTextPlayerDefault { get; set; }
    public string HelpTextPlayerMiningQuarrySpeed { get; set; }
    public string Dispensers { get; set; }
    public string Charges { get; set; }
    public string Quarries { get; set; }
    public string Excavators { get; set; }
    public string Pickups { get; set; }
    private readonly List<string> subcommands;
    private readonly Hash<string, ItemDefinition> validResources;
    private readonly Hash<string, ResourceDispenser.GatherType> validDispensers;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    protected override void LoadDefaultConfig();
    [ChatCommand("gather")]
    private void Gather(BasePlayer player, string command, string[] args);
    private void SendHelpText(BasePlayer player);
    [ConsoleCommand("gather.rate")]
    private void GatherRate(ConsoleSystem.Arg arg);
    [ConsoleCommand("gather.resources")]
    private void GatherResources(ConsoleSystem.Arg arg);
    [ConsoleCommand("gather.dispensers")]
    private void GatherDispensers(ConsoleSystem.Arg arg);
    [ConsoleCommand("dispenser.scale")]
    private void DispenserRate(ConsoleSystem.Arg arg);
    [ConsoleCommand("quarry.tickrate")]
    private void MiningQuarryTickRate(ConsoleSystem.Arg arg);
    [ConsoleCommand("excavator.tickrate")]
    private void ExcavatorTickRate(ConsoleSystem.Arg arg);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnGrowableGathered(GrowableEntity growable, Item item, BasePlayer player);
    private void OnQuarryGather(MiningQuarry quarry, Item item);
    private void OnExcavatorGather(ExcavatorArm excavator, Item item);
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private void OnSurveyGather(SurveyCharge surveyCharge, Item item);
    private void OnMiningQuarryEnabled(MiningQuarry quarry);
    private void LoadConfigValues();
    private T GetConfigValue(string category, string setting, T defaultValue);
    private void SetConfigValue(string category, string setting, T newValue);
    private void SendMessage(BasePlayer player, string message, object[] args);
}


```

---

## GatherRewards by shady14u - Gives players money through Economics/ServerRewards for various actions (gathering, animal kills)

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Libraries;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Gather Rewards", "Shady14u", "1.7.1")]
[Description("Earn rewards through Economics/Server Rewards for killing and gathering")]
public partial class GatherRewards : RustPlugin
{
    private const string _version;
    private string _resource;
    [PluginReference]
    private Plugin Economics;
    private Plugin ServerRewards;
    private Plugin Friends;
    private Plugin Clans;
    private Plugin UINotify;
    private PluginConfig _config;
     PluginConfig DefaultConfig();
    private bool _configChanged;
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private void Merge(IDictionary<T1, T2> current, IDictionary<T1, T2> defaultDict);
    private bool CheckPermission(BasePlayer player, string perm);
    private void RegisterPermsAndCommands();
    private string Lang(string key, string userId);
    private void Language();
    private void Init();
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private float CheckPoints(string animal, float defaultAmount);
    private void GiveCredit(BasePlayer player, string type, float amount, string gathered);
    private static string UppercaseFirst(string s);
    private void cmdGatherRewards(BasePlayer player, string command, string[] args);
    private void cmdConsoleGatherRewards(ConsoleSystem.Arg arg);
    private class PluginSettings
    {
        public int UINotifyMessageType;
        public bool AddMissingRewards { get; set; }
        public bool AwardOnlyOnFullHarvest { get; set; }
        public float ChainsawModifier { get; set; }
        public string ChatEditCommand { get; set; }
        public string ConsoleEditCommand { get; set; }
        public string EditPermission { get; set; }
        public Dictionary<string, object> GroupModifiers { get; set; }
        public float JackHammerModifier { get; set; }
        public string PluginPrefix { get; set; }
        public bool ShowMessagesOnGather { get; set; }
        public bool ShowMessagesOnKill { get; set; }
        public bool UseEconomics { get; set; }
        public bool UseServerRewards { get; set; }
        public bool UseUINotify { get; set; }
        public string Version { get; set; }
    }

    private static class PluginRewards
    {
        public const string ClanMember;
        public const string Corn;
        public const string Hemp;
        public const string Mushroom;
        public const string Ore;
        public const string Player;
        public const string PlayerFriend;
        public const string Pumpkin;
        public const string Stone;
        public const string TeamMember;
        public const string Wood;
        public const string Suicide;
    }

    private class PluginConfig
    {
        public Dictionary<string, float> Rewards { get; set; }
        public PluginSettings Settings { get; set; }
    }

}

private class PluginSettings
{
    public int UINotifyMessageType;
    public bool AddMissingRewards { get; set; }
    public bool AwardOnlyOnFullHarvest { get; set; }
    public float ChainsawModifier { get; set; }
    public string ChatEditCommand { get; set; }
    public string ConsoleEditCommand { get; set; }
    public string EditPermission { get; set; }
    public Dictionary<string, object> GroupModifiers { get; set; }
    public float JackHammerModifier { get; set; }
    public string PluginPrefix { get; set; }
    public bool ShowMessagesOnGather { get; set; }
    public bool ShowMessagesOnKill { get; set; }
    public bool UseEconomics { get; set; }
    public bool UseServerRewards { get; set; }
    public bool UseUINotify { get; set; }
    public string Version { get; set; }
}

private static class PluginRewards
{
    public const string ClanMember;
    public const string Corn;
    public const string Hemp;
    public const string Mushroom;
    public const string Ore;
    public const string Player;
    public const string PlayerFriend;
    public const string Pumpkin;
    public const string Stone;
    public const string TeamMember;
    public const string Wood;
    public const string Suicide;
}

private class PluginConfig
{
    public Dictionary<string, float> Rewards { get; set; }
    public PluginSettings Settings { get; set; }
}


```

---

## GeneralItemModifier by Rick6 - Modifying global item parameters such as display name, icon.

```csharp
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("General Item Modifier", "Rick", "1.0.1")]
[Description("Modifying global item parameters such as display name, icon")]
public class GeneralItemModifier : RustPlugin
{
    private void OnServerInitialized();
    private void Unload();
    private class ItemModStatsChanger : ItemMod
    {
        public static ItemModStatsChanger New(ItemEntry _entry);
        private ItemEntry entry;
        public override void OnItemCreated(Item item);
    }

    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Values")]
        public ItemEntry[] items;
    }

    private class ItemEntry
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string shortname;
        [JsonProperty(PropertyName = "Display Name")]
        public string displayName;
        [JsonProperty(PropertyName = "New Icon (Skin)")]
        public ulong skinId;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ItemModStatsChanger : ItemMod
{
    public static ItemModStatsChanger New(ItemEntry _entry);
    private ItemEntry entry;
    public override void OnItemCreated(Item item);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Values")]
    public ItemEntry[] items;
}

private class ItemEntry
{
    [JsonProperty(PropertyName = "Shortname")]
    public string shortname;
    [JsonProperty(PropertyName = "Display Name")]
    public string displayName;
    [JsonProperty(PropertyName = "New Icon (Skin)")]
    public ulong skinId;
}


```

---

## GestureWheel by Chepzz - Convenient wheel that provides the ability to use gestures

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Globalization;
using Oxide.Core;
using UnityEngine;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("Gesture Wheel", "Tricky & Mevent", "0.1.3")]
[Description("Convenient wheel that provides the ability to use gestures")]
public class GestureWheel : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
     Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Command")]
        public string Command;
        [JsonProperty(PropertyName = "Use Permission")]
        public bool UsePermission;
        [JsonProperty(PropertyName = "Button Radius")]
        public int ButtonRadius;
        [JsonProperty(PropertyName = "Close Button Color")]
        public string CloseButtonColor;
        [JsonProperty(PropertyName = "Gesture Button Color")]
        public string GestureButtonColor;
        [JsonProperty(PropertyName = "Gesture Button Size")]
        public int GestureButtonSize;
        [JsonProperty(PropertyName = "Gestures", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Gesture> Gestures;
        public class Gesture
        {
            [JsonProperty(PropertyName = "Gesture Name")]
            public string Name;
            [JsonProperty(PropertyName = "Image")]
            public string Image;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private readonly string Layer;
    private readonly string Perm;
     List<ulong> Players;
    private void Init();
    private void OnServerInitialized();
    private void GesturesCommand(BasePlayer player, string command, string[] args);
    private void GesturesCloseCommand(ConsoleSystem.Arg arg);
    private void UI_DrawInterface(BasePlayer player);
    public void LoadImage(CuiElementContainer container, string parent, string name, string image, string aMin, string aMax, string oMin, string oMax, string color);
    private static string HexToRustFormat(string hex);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Command")]
    public string Command;
    [JsonProperty(PropertyName = "Use Permission")]
    public bool UsePermission;
    [JsonProperty(PropertyName = "Button Radius")]
    public int ButtonRadius;
    [JsonProperty(PropertyName = "Close Button Color")]
    public string CloseButtonColor;
    [JsonProperty(PropertyName = "Gesture Button Color")]
    public string GestureButtonColor;
    [JsonProperty(PropertyName = "Gesture Button Size")]
    public int GestureButtonSize;
    [JsonProperty(PropertyName = "Gestures", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Gesture> Gestures;
    public class Gesture
    {
        [JsonProperty(PropertyName = "Gesture Name")]
        public string Name;
        [JsonProperty(PropertyName = "Image")]
        public string Image;
    }

}

public class Gesture
{
    [JsonProperty(PropertyName = "Gesture Name")]
    public string Name;
    [JsonProperty(PropertyName = "Image")]
    public string Image;
}


```

---

## GetSlapped by klauz24 - Slaps players when they try to act tough in chat

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Get Slapped", "klauz24", "1.0.3"), Description("Slaps players when they try to act tough in chat.")]
internal class GetSlapped : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin Slap;
    private const string _slapBypass;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Banned word, slap force (30 = 30% of health)")]
        public Dictionary<string, float> BannedWords;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnUserChat(IPlayer player, string message);
    private void SlapPlayer(IPlayer player, float value, string message);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Banned word, slap force (30 = 30% of health)")]
    public Dictionary<string, float> BannedWords;
}


```

---

## Give by MrBlue - Allows players with permission to give items or kits

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Give", "Wulf", "3.4.1")]
[Description("Allows players with permission to give items or kits")]
public class Give : CovalencePlugin
{
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty("Item blacklist (name or item ID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> ItemBlacklist;
        [JsonProperty("Log usage to console")]
        public bool LogToConsole;
        [JsonProperty("Show chat notices")]
        public bool ShowChatNotices;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    [PluginReference]
    private Plugin Kits;
    private const string PermissionGive;
    private const string PermissionGiveAll;
    private const string PermissionGiveTo;
    private const string PermissionGiveKit;
    private const string PermissionGiveKitAll;
    private const string PermissionGiveKitTo;
    private const string PermissionBypassBlacklist;
    private void Init();
    private object GiveItem(IPlayer player, string itemNameOrId, int amount, string container, float condition, ulong skinId);
    private void CommandGive(IPlayer player, string command, string[] args);
    private void CommandGiveTo(IPlayer player, string command, string[] args);
    private void CommandGiveAll(IPlayer player, string command, string[] args);
    private bool TryGiveKit(IPlayer player, string kitName);
    private void CommandGiveKit(IPlayer player, string command, string[] args);
    private void CommandGiveKitTo(IPlayer player, string command, string[] args);
    private void CommandGiveKitAll(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

public class Configuration
{
    [JsonProperty("Item blacklist (name or item ID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> ItemBlacklist;
    [JsonProperty("Log usage to console")]
    public bool LogToConsole;
    [JsonProperty("Show chat notices")]
    public bool ShowChatNotices;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## GlobalGamble by Trey - Players can enter X amount of scrap in to a gambling pot at the chance of winning everything

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Linq;
using System;
using Oxide.Core;

Oxide.Plugins
[Info("Global Gamble", "Trey", "1.2.2")]
[Description("Gives every player currently online to enter X amount of scrap in to a gambling pot at the chance of winning everything.")]
public class GlobalGamble : RustPlugin
{
    const string depositPerm;
    const string gambleStartPerm;
    private bool gambleActive;
     Timer remindActiveGamble;
     Timer initGlobalGamble;
     int currentScrapAmount;
     WinnerData _winnerCache;
    public class WinnerData
    {
        public List<string> WinnerList;
    }

     StandingDeposits standingDeposits;
    public class StandingDeposits
    {
        public Hash<string, double> Deposits;
    }

     Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Global Gamble Options")]
        public PluginOptions POptions;
    }

    public class PluginOptions
    {
        [JsonProperty(PropertyName = "Chat Command")]
        public string Ggccmd;
        [JsonProperty(PropertyName = "Frequency of GlobalGamble(minutes)")]
        public float globalGambleTimer;
        [JsonProperty(PropertyName = "Duration of Deposit Window(minutes)")]
        public float durationOfGamble;
        [JsonProperty(PropertyName = "Remind Players of Active Gamble Every X Seconds")]
        public float remindEvery;
        [JsonProperty(PropertyName = "Minimum Online Players")]
        public int minPlayer;
        [JsonProperty(PropertyName = "Minimum Scrap Deposit")]
        public int minDeposit;
        [JsonProperty(PropertyName = "Commission Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Hash<string, double> commissionPerms;
        [JsonProperty(PropertyName = "Wipe Total Earnings List")]
        public bool wipeEarnings;
        [JsonProperty(PropertyName = "Chat Icon (Steam64ID)")]
        public ulong chatIcon;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    public class GambleSystem
    {
        public class Ticket
        {
            public string Key;
            public double Weight;
            public Ticket(string key, double weight);
        }

         List<Ticket> tickets;
         Hash<int, string> playerEntries;
        public void Add(string key, double weight);
        public string Draw();
    }

    private void OnServerInitialized();
    private void InitGambleTimer();
    private void BeginGamble(BasePlayer player);
    private void ResetFields();
    private void GetWinner();
    private void AwardWinnerFunds(BasePlayer winnerPlayer, double amt);
    private void AddDeposit(BasePlayer player, int amountDeposited);
    private double GetCurrentDeposits();
    private double GetCommissionAmt(string playerID);
    private void GlobalGambleChatCommand(BasePlayer player, string command, string[] args);
    private void PrintMsg(BasePlayer player, string message);
     string Lang(string key, string id, object[] args);
}

public class WinnerData
{
    public List<string> WinnerList;
}

public class StandingDeposits
{
    public Hash<string, double> Deposits;
}

public class Configuration
{
    [JsonProperty(PropertyName = "Global Gamble Options")]
    public PluginOptions POptions;
}

public class PluginOptions
{
    [JsonProperty(PropertyName = "Chat Command")]
    public string Ggccmd;
    [JsonProperty(PropertyName = "Frequency of GlobalGamble(minutes)")]
    public float globalGambleTimer;
    [JsonProperty(PropertyName = "Duration of Deposit Window(minutes)")]
    public float durationOfGamble;
    [JsonProperty(PropertyName = "Remind Players of Active Gamble Every X Seconds")]
    public float remindEvery;
    [JsonProperty(PropertyName = "Minimum Online Players")]
    public int minPlayer;
    [JsonProperty(PropertyName = "Minimum Scrap Deposit")]
    public int minDeposit;
    [JsonProperty(PropertyName = "Commission Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Hash<string, double> commissionPerms;
    [JsonProperty(PropertyName = "Wipe Total Earnings List")]
    public bool wipeEarnings;
    [JsonProperty(PropertyName = "Chat Icon (Steam64ID)")]
    public ulong chatIcon;
}

public class GambleSystem
{
    public class Ticket
    {
        public string Key;
        public double Weight;
        public Ticket(string key, double weight);
    }

     List<Ticket> tickets;
     Hash<int, string> playerEntries;
    public void Add(string key, double weight);
    public string Draw();
}

public class Ticket
{
    public string Key;
    public double Weight;
    public Ticket(string key, double weight);
}


```

---

## GlobalMail by Hovmodet - Spawns a mailbox next to every recycler, which allows players to mail an item

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Global Mail", "Hovmodet", "1.0.4")]
[Description("Allows you to mail a single item from monuments back to your base.")]
 class GlobalMail : RustPlugin
{
     List<BuildingPrivlidge> BuildingsWithMailBoxes;
     Dictionary<ulong, StorageContainer> GlobalMailBoxes;
     List<Vector3> SmallMonumentsPosition;
     List<Vector3> MediumMonumentsPosition;
     List<Vector3> LargeMonumentsPosition;
     Vector3 OutpostPosition;
     Vector3 BanditCampPosition;
     bool AutoPlaceMailbox;
     bool CanEmptyAtMonument;
     bool CanFillAtBase;
     bool CanPlaceMultipleMailBoxes;
     bool CanOpenMailBoxAtBuildingBlock;
     bool SpawnMailAtSmallMonuments;
     bool SpawnMailAtMediumMonuments;
     bool SpawnMailAtLargeMonuments;
     bool SpawnMailAtBanditCamp;
     bool SpawnMailAtOutpost;
     bool GivePlayersMailBoxBlueprint;
     int MaxStackSizeInMailbox;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    [ChatCommand("placemail")]
    private void cmdPlaceMail(BasePlayer player, string command, string[] args);
    [ChatCommand("mail")]
    private void cmdMail(BasePlayer player, string command, string[] args);
     object OnPlayerTick(BasePlayer player, PlayerTick msg, bool wasPlayerStalled);
     void OnPlayerInput(BasePlayer player, InputState input);
    private void Init();
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private object CanAcceptItem(ItemContainer container, Item item);
     object CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot, int amount);
     object CanStackItem(Item item, Item targetItem);
     void OnEntityKill(Mailbox mailBox);
     object OnEntityTakeDamage(Mailbox mailBox, HitInfo info);
     object CanLootEntity(BasePlayer player, Mailbox container);
     bool CanUseMailbox(BasePlayer player, Mailbox mailbox);
     void OnServerInitialized(bool initial);
     void OnPlayerConnected(BasePlayer player);
     Dictionary<BasePlayer,PlacementMailbox> previewMailBox;
    private void UpdateMailBoxPosition(BasePlayer player);
    private void learnMailBox(BasePlayer player);
    private string FindMonumentClosestToRecycler(Vector3 RecyclerPosition);
    private void FindRecyclers();
    private void FindMonuments();
    private void LoadAllMailBoxes();
    private void OpenContainer(BasePlayer player, ulong steamid);
    private void OpenPlayersMailbox(BasePlayer player, ulong steamid);
}

 class PlacementMailbox
{
    public Mailbox mailbox;
    public BasePlayer player;
    public bool placed;
}


```

---

## GlobalProgression by  - Global blueprint unlocking

```csharp
using Newtonsoft.Json;
using ProtoBuf;
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;

Oxide.Plugins
[Info("Global Progression", "ignignokt84", "0.2.1")]
[Description("Global blueprint learning")]
 class GlobalProgression : RustPlugin
{
     Timer tickTimer;
     DataWrapper data;
    const string CommandRefresh;
    const string CommandIgnore;
    const string CommandUnignore;
    const string PermAdministrate;
    const string PermIgnore;
    protected override void LoadDefaultMessages();
     void SendMessage(BasePlayer player, string key, object[] options);
     void SendMessage(ConsoleSystem.Arg arg, string key, object[] options);
     string GetMessage(string key, string userId);
     void Init();
     void OnServerInitialized();
     void Unload();
     void LoadConfiguration();
    protected override void LoadDefaultConfig();
     void SaveData();
     void CommandDelegator(ConsoleSystem.Arg arg);
     void RefreshBlueprints(ConsoleSystem.Arg arg, bool verbose);
     void CheckQueue(BasePlayer player, string command, string[] args);
     void OnPlayerInit(BasePlayer player);
     void Sync(BasePlayer player);
     object OnItemAction(Item item, string action, BasePlayer player);
     void LocalTick();
     void DestroyTimer();
     void UnlockBlueprintForAllPlayers(string shortname);
     void UnlockBlueprintforPlayer(ItemDefinition itemDef, BasePlayer player, bool notify);
     int UnlockGlobalBlueprintsForPlayer(BasePlayer player, bool notify);
    private bool HasPermission(string userIDString, string permname);
     bool IsBlueprintUnlocked(string shortname);
     bool IsBlueprintUnlocking(string shortname);
     bool ShouldUnlock(string shortname);
     bool IsBlocked(string shortname);
     int GetPendingUnlockCount();
     List<string> GetBlueprintsToUnlock();
     void Increment(string shortname);
     class DataWrapper
    {
        [JsonProperty(PropertyName = "First Load")]
        public bool firstLoad;
        [JsonProperty(PropertyName = "Blocking")]
        public bool blocking;
        [JsonProperty(PropertyName = "Global Unlock Threshold")]
        public int threshold;
        [JsonProperty(PropertyName = "Learning Delay")]
        public int delay;
        [JsonProperty(PropertyName = "Tick Rate")]
        public float tickRate;
        [JsonProperty(PropertyName = "Unlocked Blueprints")]
        public HashSet<string> unlockedBlueprints;
        [JsonProperty(PropertyName = "Pending Blueprints")]
        public Dictionary<string, Blueprint> pendingBlueprints;
    }

}

 class DataWrapper
{
    [JsonProperty(PropertyName = "First Load")]
    public bool firstLoad;
    [JsonProperty(PropertyName = "Blocking")]
    public bool blocking;
    [JsonProperty(PropertyName = "Global Unlock Threshold")]
    public int threshold;
    [JsonProperty(PropertyName = "Learning Delay")]
    public int delay;
    [JsonProperty(PropertyName = "Tick Rate")]
    public float tickRate;
    [JsonProperty(PropertyName = "Unlocked Blueprints")]
    public HashSet<string> unlockedBlueprints;
    [JsonProperty(PropertyName = "Pending Blueprints")]
    public Dictionary<string, Blueprint> pendingBlueprints;
}


```

---

## GlobalStorage by Imthenewguy - Adds a global storage system to your server, automatically spawning chests in safe-zone monuments

```csharp
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Global Storage", "imthenewguy", "1.0.5")]
[Description("Create global storage chests in safezone monuments and by placing the item.")]
 class GlobalStorage : RustPlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Storage prefab profile to use")]
        public string storage_prefab;
        [JsonProperty("Amount of slots that the players can access? Max value is the maximum number of slots for the container type.")]
        public int slot_count;
        [JsonProperty("Box skin")]
        public ulong box_skin;
        [JsonProperty("Display floating text above the containers indicating that it is a storage unit?")]
        public bool draw_text;
        [JsonProperty("If floating text is enabled, how often should it update?")]
        public float draw_update;
        [JsonProperty("Maximum distance away from the box that the player can see the text?")]
        public float draw_distance;
        [JsonProperty("Floating text color [white, black, red, blue, green, cyan, grey, magenta, yellow]")]
        public string text_col;
        [JsonProperty("Display floating text above manually deployed containers?")]
        public bool draw_text_non_monument;
        [JsonProperty("Make player deployed global storage chests invulnerable?")]
        public bool deployed_chests_invulnerable;
        [JsonProperty("Auto wipe monument and player data on new server wipe")]
        public bool auto_wipe;
        [JsonProperty("A list of item shortnames that cannot be placed into the chest")]
        public List<string> black_list;
        [JsonProperty("If populated with anything, these items will be the only items allowed in the container")]
        public List<string> white_list;
        [JsonProperty("Monument modifiers")]
        public List<Configuration.monumentInfo> monuments;
        [JsonProperty("Physical storage box prefab to spawn in the world")]
        public string spawn_prefab;
        [JsonProperty("Item that is given when using the giveglobalbox commands. Be sure to use the shortname that matches the prefab that will be spawning.")]
        public string item_shortname;
        [JsonProperty("Static box spawns on a map that can access global storage")]
        public List<CustomPosInfo> customPosBoxes;
        [JsonProperty("Anchored boxes placed in rust edit. Requires 2 anchor entities to find it.")]
        public List<AnchorInfo> RustEdit_Boxes;
        [JsonProperty("Custom item whitelist filter settings")]
        public FilterInfo filterSettings;
        public class monumentInfo
        {
            public string name;
            public bool enabled;
            public Vector3 pos;
            public Vector3 rot;
            public monumentInfo(string monument, bool enabled, Vector3 pos, Vector3 rot);
        }

        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    public class FilterInfo
    {
        [JsonProperty("Prevent other items from being added to the box besides the items whose names/shortnames are included in the list below??")]
        public bool exclude_everything_else;
        [JsonProperty("List of items that you would like to allow (item names/shortnames)?")]
        public List<string> filters;
    }

    public class AnchorInfo
    {
        [JsonProperty("Enabled?")]
        public bool enabled;
        [JsonProperty("Primary anchor entity shortname")]
        public string primary_entity;
        [JsonProperty("Secondary anchor entity shortname")]
        public string secondary_entity;
        [JsonProperty("How far away from the chest are the anchor entities")]
        public float distance;
    }

    protected override void LoadDefaultConfig();
    private List<Configuration.monumentInfo> DefaultMonuments { get; set; }
    protected override void LoadConfig();
    protected override void SaveConfig();
     PlayerEntity pcdData;
    private DynamicConfigFile PCDDATA;
    const string perms_admin;
    const string perms_chat;
    const string perms_access;
     void Init();
    public bool usingWhiteList;
     void Unload();
     void OnServerSave();
     void SaveData();
     void LoadData();
     class StorageBox
    {
        public List<StorageInfo> _storage;
    }

    public class StorageInfo
    {
        public string shortname;
        public string displayName;
        public ulong skin;
        public int amount;
        public int slot;
        public float condition;
        public float maxCondition;
        public int ammo;
        public string ammotype;
        public StorageInfo[] contents;
        public InstancedInfo instanceData;
        public Item.Flag flags;
        public class InstancedInfo
        {
            public bool ShouldPool;
            public int dataInt;
            public int blueprintTarget;
            public int blueprintAmount;
            public ulong subEntity;
        }

        public string text;
    }

     class PlayerEntity
    {
        public Dictionary<ulong, StorageBox> storage;
        public Dictionary<ulong, monumentInfo> monuments;
        public bool purgeActive;
        public Dictionary<ulong, AnchorInfo> RustEdit_storage_data;
    }

    public class monumentInfo
    {
        public string monument;
        public bool enabled;
        public Vector3 pos;
        public Vector3 rot;
        public monumentInfo(string monument, bool enabled, Vector3 pos, Vector3 rot);
    }

    public class CustomPosInfo
    {
        public Vector3 pos;
        public Vector3 rot;
        public bool enabled;
        public CustomPosInfo(bool enabled, Vector3 pos, Vector3 rot);
    }

    protected override void LoadDefaultMessages();
     List<StorageContainer> containers;
     Dictionary<ulong, float> bagCooldownTimer;
     StorageBox storageData;
    private void OpenStorage(BasePlayer player);
     void StoreContainerLoot(BasePlayer player, StorageContainer container);
     void OnLootEntityEnd(BasePlayer player, StorageContainer entity);
     StorageContainer CreateCustomBox(Vector3 pos, Vector3 rot);
     StorageContainer CreateCustomBox(Vector3 pos, Quaternion rot);
     StorageContainer CreateBox(MonumentInfo monument, Vector3 pos_settings, Vector3 rot_settings, ulong ownerID);
     Dictionary<ulong, StorageContainer> boxes;
     void OnNewSave(string filename);
    private ProtectionProperties NoDamageProtection;
     void OnServerInitialized(bool initial);
     Color GetColor(string color);
     object OnEntityTakeDamage(StorageContainer entity, HitInfo info);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     void OnEntityKill(StorageContainer entity);
     object CanPickupEntity(BasePlayer player, StorageContainer entity);
     void OnEntityBuilt(Planner plan, GameObject go);
     object CanDeployItem(BasePlayer player, Deployer deployer, ulong entityId);
     object CanMoveItem(Item item, PlayerInventory playerLoot, ItemContainerId targetContainer, int targetSlot, int amount);
     void GiveBoxItem(BasePlayer player, int quantity);
    private BasePlayer FindPlayerByName(string Playername, BasePlayer SearchingPlayer);
    private const int LAYER_TARGET;
    private BaseEntity GetTargetEntity(BasePlayer player);
    [ConsoleCommand("gspurge")]
     void PurgeCommand(ConsoleSystem.Arg arg);
    [ChatCommand("addcustomlocation")]
     void AddCustomLocation(BasePlayer player);
    [ChatCommand("removecustomlocation")]
     void RemoveCustomLocation(BasePlayer player);
    [ChatCommand("giveglobalbox")]
     void GiveBox(BasePlayer player, string command, string[] args);
    [ConsoleCommand("giveglobalbox")]
     void GiveBoxConsole(ConsoleSystem.Arg arg);
    [ChatCommand("gstorage")]
     void GlobalStorageCMD(BasePlayer player);
    [ChatCommand("addglobalstorage")]
     void AddGlobalStorageCMD(BasePlayer player, string command, string[] args);
     List<StorageContainer> map_boxes;
     bool FindAnchorBoxes(AnchorInfo anchorInfo);
}

public class Configuration
{
    [JsonProperty("Storage prefab profile to use")]
    public string storage_prefab;
    [JsonProperty("Amount of slots that the players can access? Max value is the maximum number of slots for the container type.")]
    public int slot_count;
    [JsonProperty("Box skin")]
    public ulong box_skin;
    [JsonProperty("Display floating text above the containers indicating that it is a storage unit?")]
    public bool draw_text;
    [JsonProperty("If floating text is enabled, how often should it update?")]
    public float draw_update;
    [JsonProperty("Maximum distance away from the box that the player can see the text?")]
    public float draw_distance;
    [JsonProperty("Floating text color [white, black, red, blue, green, cyan, grey, magenta, yellow]")]
    public string text_col;
    [JsonProperty("Display floating text above manually deployed containers?")]
    public bool draw_text_non_monument;
    [JsonProperty("Make player deployed global storage chests invulnerable?")]
    public bool deployed_chests_invulnerable;
    [JsonProperty("Auto wipe monument and player data on new server wipe")]
    public bool auto_wipe;
    [JsonProperty("A list of item shortnames that cannot be placed into the chest")]
    public List<string> black_list;
    [JsonProperty("If populated with anything, these items will be the only items allowed in the container")]
    public List<string> white_list;
    [JsonProperty("Monument modifiers")]
    public List<Configuration.monumentInfo> monuments;
    [JsonProperty("Physical storage box prefab to spawn in the world")]
    public string spawn_prefab;
    [JsonProperty("Item that is given when using the giveglobalbox commands. Be sure to use the shortname that matches the prefab that will be spawning.")]
    public string item_shortname;
    [JsonProperty("Static box spawns on a map that can access global storage")]
    public List<CustomPosInfo> customPosBoxes;
    [JsonProperty("Anchored boxes placed in rust edit. Requires 2 anchor entities to find it.")]
    public List<AnchorInfo> RustEdit_Boxes;
    [JsonProperty("Custom item whitelist filter settings")]
    public FilterInfo filterSettings;
    public class monumentInfo
    {
        public string name;
        public bool enabled;
        public Vector3 pos;
        public Vector3 rot;
        public monumentInfo(string monument, bool enabled, Vector3 pos, Vector3 rot);
    }

    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class monumentInfo
{
    public string name;
    public bool enabled;
    public Vector3 pos;
    public Vector3 rot;
    public monumentInfo(string monument, bool enabled, Vector3 pos, Vector3 rot);
}

public class FilterInfo
{
    [JsonProperty("Prevent other items from being added to the box besides the items whose names/shortnames are included in the list below??")]
    public bool exclude_everything_else;
    [JsonProperty("List of items that you would like to allow (item names/shortnames)?")]
    public List<string> filters;
}

public class AnchorInfo
{
    [JsonProperty("Enabled?")]
    public bool enabled;
    [JsonProperty("Primary anchor entity shortname")]
    public string primary_entity;
    [JsonProperty("Secondary anchor entity shortname")]
    public string secondary_entity;
    [JsonProperty("How far away from the chest are the anchor entities")]
    public float distance;
}

 class StorageBox
{
    public List<StorageInfo> _storage;
}

public class StorageInfo
{
    public string shortname;
    public string displayName;
    public ulong skin;
    public int amount;
    public int slot;
    public float condition;
    public float maxCondition;
    public int ammo;
    public string ammotype;
    public StorageInfo[] contents;
    public InstancedInfo instanceData;
    public Item.Flag flags;
    public class InstancedInfo
    {
        public bool ShouldPool;
        public int dataInt;
        public int blueprintTarget;
        public int blueprintAmount;
        public ulong subEntity;
    }

    public string text;
}

public class InstancedInfo
{
    public bool ShouldPool;
    public int dataInt;
    public int blueprintTarget;
    public int blueprintAmount;
    public ulong subEntity;
}

 class PlayerEntity
{
    public Dictionary<ulong, StorageBox> storage;
    public Dictionary<ulong, monumentInfo> monuments;
    public bool purgeActive;
    public Dictionary<ulong, AnchorInfo> RustEdit_storage_data;
}

public class monumentInfo
{
    public string monument;
    public bool enabled;
    public Vector3 pos;
    public Vector3 rot;
    public monumentInfo(string monument, bool enabled, Vector3 pos, Vector3 rot);
}

public class CustomPosInfo
{
    public Vector3 pos;
    public Vector3 rot;
    public bool enabled;
    public CustomPosInfo(bool enabled, Vector3 pos, Vector3 rot);
}


```

---

## Godmode by dFxPhoeniX - Allows players with permission to be invulerable and god-like

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust;
using Rust;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Godmode", "Wulf/lukespragg/Arainrr/dFxPhoeniX", "4.2.14")]
[Description("Allows players with permission to be invulnerable and god-like")]
internal class Godmode : RustPlugin
{
    private const string PermAdmin;
    private const string PermInvulnerable;
    private const string PermLootPlayers;
    private const string PermLootProtection;
    private const string PermNoAttacking;
    private const string PermToggle;
    private const string PermUntiring;
    private const string PermAutoEnable;
    private readonly object _false;
    private readonly object _true;
    private Dictionary<ulong, float> _informHistory;
    private readonly StoredMetabolism _storedMetabolism;
    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private object CanBeWounded(BasePlayer player);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private object OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private object OnRunPlayerMetabolism(PlayerMetabolism metabolism, BasePlayer player, float delta);
    private void CheckHooks();
    private void InformPlayers(BasePlayer victim, BasePlayer attacker);
    private bool? ToggleGodmode(BasePlayer target, BasePlayer player);
    private bool EnableGodmode(string playerId, bool isInit);
    private bool DisableGodmode(string playerId, bool isUnload);
    private void PlayerRename(BasePlayer player, bool isGod);
    private void Rename(BasePlayer player, string newName);
    private void ModifyMetabolism(BasePlayer player, bool isGod);
    private class StoredMetabolism
    {
        public bool FetchDefaultMetabolism();
        private Attribute calories;
        private Attribute hydration;
        private Attribute heartrate;
        private Attribute temperature;
        private Attribute poison;
        private Attribute radiation_level;
        private Attribute radiation_poison;
        private Attribute wetness;
        private Attribute dirtyness;
        private Attribute oxygen;
        private Attribute bleeding;
        public float GetMaxHydration();
        public void Store(PlayerMetabolism playerMetabolism);
        public void Unlimited(PlayerMetabolism playerMetabolism);
        public void Restore(PlayerMetabolism playerMetabolism);
    }

    private static void NullifyDamage(HitInfo info);
    private static string GetPayerOriginalName(ulong playerId);
    private bool EnableGodmode(IPlayer iPlayer);
    private bool EnableGodmode(ulong playerId);
    private bool DisableGodmode(IPlayer iPlayer);
    private bool DisableGodmode(ulong playerId);
    private bool IsGod(ulong playerId);
    private bool IsGod(BasePlayer player);
    private bool IsGod(string playerId);
    private string[] AllGods(string playerId);
    private string[] AllGods();
    private void GodCommand(IPlayer iPlayer, string command, string[] args);
    private void GodsCommand(IPlayer iPlayer, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Inform On Attack (true/false)")]
        public bool informOnAttack;
        [JsonProperty(PropertyName = "Inform Interval (Seconds)")]
        public float informInterval;
        [JsonProperty(PropertyName = "Show Name Prefix (true/false)")]
        public bool showNamePrefix;
        [JsonProperty(PropertyName = "Name Prefix (Default [God])")]
        public string namePrefix;
        [JsonProperty(PropertyName = "Time Limit (Seconds, 0 to Disable)")]
        public float timeLimit;
        [JsonProperty(PropertyName = "Disable godmode after disconnect (true/false)")]
        public bool disconnectDisable;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat Prefix color")]
        public string prefixColor;
        [JsonProperty(PropertyName = "Chat steamID icon")]
        public ulong steamIDIcon;
        [JsonProperty(PropertyName = "God commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] godCommand;
        [JsonProperty(PropertyName = "Gods commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] godsCommand;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private StoredData storedData;
    private class StoredData
    {
        public readonly HashSet<string> godPlayers;
    }

    private void LoadData();
    private void SaveData();
    private void ClearData();
    private void Print(IPlayer iPlayer, string message);
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class StoredMetabolism
{
    public bool FetchDefaultMetabolism();
    private Attribute calories;
    private Attribute hydration;
    private Attribute heartrate;
    private Attribute temperature;
    private Attribute poison;
    private Attribute radiation_level;
    private Attribute radiation_poison;
    private Attribute wetness;
    private Attribute dirtyness;
    private Attribute oxygen;
    private Attribute bleeding;
    public float GetMaxHydration();
    public void Store(PlayerMetabolism playerMetabolism);
    public void Unlimited(PlayerMetabolism playerMetabolism);
    public void Restore(PlayerMetabolism playerMetabolism);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Inform On Attack (true/false)")]
    public bool informOnAttack;
    [JsonProperty(PropertyName = "Inform Interval (Seconds)")]
    public float informInterval;
    [JsonProperty(PropertyName = "Show Name Prefix (true/false)")]
    public bool showNamePrefix;
    [JsonProperty(PropertyName = "Name Prefix (Default [God])")]
    public string namePrefix;
    [JsonProperty(PropertyName = "Time Limit (Seconds, 0 to Disable)")]
    public float timeLimit;
    [JsonProperty(PropertyName = "Disable godmode after disconnect (true/false)")]
    public bool disconnectDisable;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat Prefix color")]
    public string prefixColor;
    [JsonProperty(PropertyName = "Chat steamID icon")]
    public ulong steamIDIcon;
    [JsonProperty(PropertyName = "God commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] godCommand;
    [JsonProperty(PropertyName = "Gods commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] godsCommand;
}

private class StoredData
{
    public readonly HashSet<string> godPlayers;
}


```

---

## GodmodeIndicator by 2CHEVSKII - Displays an UI indicator for players with godmode enabled

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using static Oxide.Game.Rust.Cui.CuiHelper;

Oxide.Plugins
[Info("Godmode Indicator", "2CHEVSKII", "2.1.3")]
[Description("Displays an indicator on screen if a player is in godmode")]
 class GodmodeIndicator : CovalencePlugin
{
    const string UI_MAIN_PANEL_NAME;
    static GodmodeIndicator Instance;
    [PluginReference]
     Plugin ImageLibrary;
    [PluginReference]
     Plugin Godmode;
     string uiCached;
     Dictionary<string, GodmodeUi> idToComponent;
     PluginSettings settings;
     class PluginSettings
    {
        [JsonProperty("UI X position")]
        public float UiX { get; set; }
        [JsonProperty("UI Y position")]
        public float UiY { get; set; }
        [JsonProperty("UI URL")]
        public string UiUrl { get; set; }
        [JsonProperty("UI Color")]
        public string UiColor { get; set; }
        [DefaultValue(1.0f)]
        [JsonProperty("UI Scale", DefaultValueHandling = DefaultValueHandling.Populate)]
        public float UiScale { get; set; }
    }

     PluginSettings GetDefaultSettings();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void Init();
     void OnServerInitialized();
     void OnUserConnected(IPlayer user);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnUserDisconnected(IPlayer user);
     void Unload();
     void OnGodmodeToggled(string playerId, bool enabled);
     void BuildUi();
     class GodmodeUi : MonoBehaviour
    {
         BasePlayer player;
         bool uiVisible;
         bool isInPluginGod;
        public bool GodmodePluginStatus { get; set; }
         bool IsGod { get; set; }
        public void OnSleepEnded();
        public void OnUiBuilt();
         void Awake();
         void Start();
         void OnDestroy();
         void SetVisible(bool bVisible);
         void Tick();
    }

}

 class PluginSettings
{
    [JsonProperty("UI X position")]
    public float UiX { get; set; }
    [JsonProperty("UI Y position")]
    public float UiY { get; set; }
    [JsonProperty("UI URL")]
    public string UiUrl { get; set; }
    [JsonProperty("UI Color")]
    public string UiColor { get; set; }
    [DefaultValue(1.0f)]
    [JsonProperty("UI Scale", DefaultValueHandling = DefaultValueHandling.Populate)]
    public float UiScale { get; set; }
}

 class GodmodeUi : MonoBehaviour
{
     BasePlayer player;
     bool uiVisible;
     bool isInPluginGod;
    public bool GodmodePluginStatus { get; set; }
     bool IsGod { get; set; }
    public void OnSleepEnded();
    public void OnUiBuilt();
     void Awake();
     void Start();
     void OnDestroy();
     void SetVisible(bool bVisible);
     void Tick();
}


```

---

## GPay by  - Automatic donation and payment system

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using System.Text;

Oxide.Plugins
[Info("GPay", "Soccerjunki", "2.0.2")]
[Description("Auto donation system")]
 class GPay : CovalencePlugin
{
     class GPayWebResponse
    {
        public string error { get; set; }
        public string[] commands { get; set; }
    }

    protected override void LoadDefaultConfig();
    private new void LoadDefaultMessages();
     string Lang(string key, object userID);
    [Command("donate")]
     void DonateCommand(IPlayer player, string command, string[] args);
    [Command("setupgpay")]
     void setupGpay(IPlayer player, string command, string[] args);
    [Command("claimdonation")]
     void ClaimDonatCommand(IPlayer player, string command, string[] args);
    private void Init();
    private void RetrieveAllPendingOfflineCommands();
    private void executeOfflineCommands(int code, string response);
     void executeOnlineCommands(int code, string response, IPlayer player);
}

 class GPayWebResponse
{
    public string error { get; set; }
    public string[] commands { get; set; }
}


```

---

## GraffitiRestriction by  - Restricts drawing graffiti on buildings where players don't have building privilege

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Graffiti Restriction", "Turtle In Black", "1.1.0")]
[Description("Restricts drawing graffiti on buildings where players don't have building privilege.")]
public class GraffitiRestriction : RustPlugin
{
    private const string permBypass;
    private void Init();
    private object OnSprayCreate(SprayCan spray, Vector3 position, Quaternion rotation);
    private static class Lang
    {
        public const string NoGraffiti;
    }

    protected override void LoadDefaultMessages();
    private string GetLang(string messageKey, string playerId, object[] args);
}

private static class Lang
{
    public const string NoGraffiti;
}


```

---

## Grid by yetzt - Get the map grid cell reference for a position

```csharp
using UnityEngine;
using System.Collections.Generic;

Oxide.Plugins
[Info("Grid", "yetzt", "0.0.3")]
[Description("Get the map grid reference of a position")]
public class Grid : RustPlugin
{
     void LoadDefaultMessages();
     string getGrid(Vector3 pos);
    [ChatCommand("grid")]
     void replyGrid(BasePlayer player, string cmd, string[] args);
}


```

---

## GroupBans by zeeuss - Plugin that allows you to temporarily or permamently ban/unban players from certain oxide groups.

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System.Data.SQLite;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using UnityEngine;
using Facepunch.Models;
using ConVar;
using System.Collections.Specialized;

Oxide.Plugins
[Info("Group Bans", "Zeeuss", "0.1.4")]
[Description("Plugin that allows you to temporarily or permamently ban/unban players from certain oxide groups.")]
public class GroupBans : CovalencePlugin
{
    const string groupBanUse;
    const string groupBanProtect;
    static double GrabCurrentTime();
     void Init();
    protected override void LoadDefaultMessages();
    [Command("gban")]
     void chatBanComm(IPlayer player, string command, string[] args);
    [Command("gunban")]
     void chatUnbanComm(IPlayer player, string command, string[] args);
    private void Message(IPlayer player, string message);
    private string Lang(string key, string id);
     string GetMsg(string key, object[] args);
}


```

---

## GroupLimits by misticos - Prevent rulebreakers from breaking group limits on your server and notify your staff

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using UnityEngine;

Oxide.Plugins
[Info("Group Limits", "misticos", "3.0.4")]
[Description("Prevent rulebreakers from breaking group limits on your server and notify your staff")]
 class GroupLimits : CovalencePlugin
{
    private const string PermissionIgnore;
    private static GroupLimits _ins;
    private string _webhookBodyCached;
    private Dictionary<string, string> _cachedHeaders;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Limit> Limits;
        [JsonProperty(PropertyName = "Log Format")]
        public string LogFormat;
        public class Limit
        {
            [JsonProperty(PropertyName = "Type Name")]
            public string Name;
            [JsonProperty(PropertyName = "Max Authorized")]
            public int MaxAuthorized;
            [JsonProperty(PropertyName = "Shortnames", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> Shortnames;
            [JsonProperty(PropertyName = "Disable For Decaying Structures")]
            public bool NoDecaying;
            [JsonProperty(PropertyName = "Notify Player")]
            public bool NotifyPlayer;
            [JsonProperty(PropertyName = "Notify Owner")]
            public bool NotifyOwner;
            [JsonProperty(PropertyName = "Enforce")]
            public bool Enforce;
            [JsonProperty(PropertyName = "Deauthorize")]
            public bool Deauthorize;
            [JsonProperty(PropertyName = "Deauthorize All")]
            public bool DeauthorizeAll;
            [JsonProperty(PropertyName = "Discord")]
            public Discord Webhook;
            [JsonProperty(PropertyName = "Log To File")]
            public bool File;
            public static Limit Find(string shortname);
            public class Discord
            {
                [JsonProperty(PropertyName = "Webhook")]
                public string Webhook;
                [JsonProperty(PropertyName = "Inline")]
                public bool Inline;
                [JsonProperty(PropertyName = "Title")]
                public string Title;
                [JsonProperty(PropertyName = "Color")]
                public int Color;
                [JsonProperty(PropertyName = "Player Title")]
                public string PlayerTitle;
                [JsonProperty(PropertyName = "Player")]
                public string Player;
                [JsonProperty(PropertyName = "Authed Title")]
                public string AuthedTitle;
                [JsonProperty(PropertyName = "Authed")]
                public string Authed;
                [JsonProperty(PropertyName = "Authed Entry")]
                public string AuthedEntry;
                [JsonProperty(PropertyName = "Authed Separator")]
                public string AuthedSeparator;
                [JsonProperty(PropertyName = "Entity Title")]
                public string EntityTitle;
                [JsonProperty(PropertyName = "Entity")]
                public string Entity;
                [JsonProperty(PropertyName = "Position Title")]
                public string PositionTitle;
                [JsonProperty(PropertyName = "Position")]
                public string Position;
            }

        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void Init();
    private void Unload();
    protected override void LoadDefaultMessages();
    private object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private object OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
    private object OnTurretAuthorize(AutoTurret turret, BasePlayer player);
    private void Notify(Configuration.Limit limit, BaseEntity entity, BasePlayer basePlayer, IEnumerable<ulong> authed, bool deauth);
    private void NotifyLog(Configuration.Limit limit, BaseNetworkable entity, BasePlayer player);
    private void NotifyDiscord(Configuration.Limit limit, BaseNetworkable entity, BasePlayer player, IEnumerable<ulong> authedPlayers);
    private string FormattedCoordinates(Vector3 pos);
    private bool IsDecaying(BuildingPrivlidge privilege);
    private string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Limits", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Limit> Limits;
    [JsonProperty(PropertyName = "Log Format")]
    public string LogFormat;
    public class Limit
    {
        [JsonProperty(PropertyName = "Type Name")]
        public string Name;
        [JsonProperty(PropertyName = "Max Authorized")]
        public int MaxAuthorized;
        [JsonProperty(PropertyName = "Shortnames", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Shortnames;
        [JsonProperty(PropertyName = "Disable For Decaying Structures")]
        public bool NoDecaying;
        [JsonProperty(PropertyName = "Notify Player")]
        public bool NotifyPlayer;
        [JsonProperty(PropertyName = "Notify Owner")]
        public bool NotifyOwner;
        [JsonProperty(PropertyName = "Enforce")]
        public bool Enforce;
        [JsonProperty(PropertyName = "Deauthorize")]
        public bool Deauthorize;
        [JsonProperty(PropertyName = "Deauthorize All")]
        public bool DeauthorizeAll;
        [JsonProperty(PropertyName = "Discord")]
        public Discord Webhook;
        [JsonProperty(PropertyName = "Log To File")]
        public bool File;
        public static Limit Find(string shortname);
        public class Discord
        {
            [JsonProperty(PropertyName = "Webhook")]
            public string Webhook;
            [JsonProperty(PropertyName = "Inline")]
            public bool Inline;
            [JsonProperty(PropertyName = "Title")]
            public string Title;
            [JsonProperty(PropertyName = "Color")]
            public int Color;
            [JsonProperty(PropertyName = "Player Title")]
            public string PlayerTitle;
            [JsonProperty(PropertyName = "Player")]
            public string Player;
            [JsonProperty(PropertyName = "Authed Title")]
            public string AuthedTitle;
            [JsonProperty(PropertyName = "Authed")]
            public string Authed;
            [JsonProperty(PropertyName = "Authed Entry")]
            public string AuthedEntry;
            [JsonProperty(PropertyName = "Authed Separator")]
            public string AuthedSeparator;
            [JsonProperty(PropertyName = "Entity Title")]
            public string EntityTitle;
            [JsonProperty(PropertyName = "Entity")]
            public string Entity;
            [JsonProperty(PropertyName = "Position Title")]
            public string PositionTitle;
            [JsonProperty(PropertyName = "Position")]
            public string Position;
        }

    }

}

public class Limit
{
    [JsonProperty(PropertyName = "Type Name")]
    public string Name;
    [JsonProperty(PropertyName = "Max Authorized")]
    public int MaxAuthorized;
    [JsonProperty(PropertyName = "Shortnames", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Shortnames;
    [JsonProperty(PropertyName = "Disable For Decaying Structures")]
    public bool NoDecaying;
    [JsonProperty(PropertyName = "Notify Player")]
    public bool NotifyPlayer;
    [JsonProperty(PropertyName = "Notify Owner")]
    public bool NotifyOwner;
    [JsonProperty(PropertyName = "Enforce")]
    public bool Enforce;
    [JsonProperty(PropertyName = "Deauthorize")]
    public bool Deauthorize;
    [JsonProperty(PropertyName = "Deauthorize All")]
    public bool DeauthorizeAll;
    [JsonProperty(PropertyName = "Discord")]
    public Discord Webhook;
    [JsonProperty(PropertyName = "Log To File")]
    public bool File;
    public static Limit Find(string shortname);
    public class Discord
    {
        [JsonProperty(PropertyName = "Webhook")]
        public string Webhook;
        [JsonProperty(PropertyName = "Inline")]
        public bool Inline;
        [JsonProperty(PropertyName = "Title")]
        public string Title;
        [JsonProperty(PropertyName = "Color")]
        public int Color;
        [JsonProperty(PropertyName = "Player Title")]
        public string PlayerTitle;
        [JsonProperty(PropertyName = "Player")]
        public string Player;
        [JsonProperty(PropertyName = "Authed Title")]
        public string AuthedTitle;
        [JsonProperty(PropertyName = "Authed")]
        public string Authed;
        [JsonProperty(PropertyName = "Authed Entry")]
        public string AuthedEntry;
        [JsonProperty(PropertyName = "Authed Separator")]
        public string AuthedSeparator;
        [JsonProperty(PropertyName = "Entity Title")]
        public string EntityTitle;
        [JsonProperty(PropertyName = "Entity")]
        public string Entity;
        [JsonProperty(PropertyName = "Position Title")]
        public string PositionTitle;
        [JsonProperty(PropertyName = "Position")]
        public string Position;
    }

}

public class Discord
{
    [JsonProperty(PropertyName = "Webhook")]
    public string Webhook;
    [JsonProperty(PropertyName = "Inline")]
    public bool Inline;
    [JsonProperty(PropertyName = "Title")]
    public string Title;
    [JsonProperty(PropertyName = "Color")]
    public int Color;
    [JsonProperty(PropertyName = "Player Title")]
    public string PlayerTitle;
    [JsonProperty(PropertyName = "Player")]
    public string Player;
    [JsonProperty(PropertyName = "Authed Title")]
    public string AuthedTitle;
    [JsonProperty(PropertyName = "Authed")]
    public string Authed;
    [JsonProperty(PropertyName = "Authed Entry")]
    public string AuthedEntry;
    [JsonProperty(PropertyName = "Authed Separator")]
    public string AuthedSeparator;
    [JsonProperty(PropertyName = "Entity Title")]
    public string EntityTitle;
    [JsonProperty(PropertyName = "Entity")]
    public string Entity;
    [JsonProperty(PropertyName = "Position Title")]
    public string PositionTitle;
    [JsonProperty(PropertyName = "Position")]
    public string Position;
}


```

---

## GroupMessages by Sasuke - Send randomly configured chat messages for each player based in your groups

```csharp
using System;
using System.Linq;
using System.Collections.Generic;

Oxide.Plugins
[Info("Group Messages", "Sasuke", "1.0.1")]
[Description("Send randomly configured chat messages for each player based in your groups")]
 class GroupMessages : RustPlugin
{
    private string tag;
    private string color;
    private float interval;
    private Dictionary<string, object> messages;
    private static Dictionary<string, object> DefaultMessages();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
    private void StartAutoMessages();
    private List<string> PlayerMessages(BasePlayer player);
    private Dictionary<string, List<string>> ParseDictionary(Dictionary<string, object> dict);
    private T GetConfig(string name, T value);
}


```

---

## GroupMigrate by MrBlue - Migrate players and permissions between groups

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Group Migrate", "Wulf", "1.0.0")]
[Description("Migrate players and permissions between groups")]
 class GroupMigrate : CovalencePlugin
{
    protected override void LoadDefaultMessages();
    private const string permUse;
    private void Init();
    private void CommandMigrate(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string langKey, object[] args);
}


```

---

## GrTeleport by  - Allows teleportation using the map grid reference and death reference

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
[Info("Gr Teleport", "carny666", "1.0.3")]
[Description("Allows teleportation using the map grid reference and death reference")]
 class GrTeleport : RustPlugin
{
    const float calgon;
    private const string adminPermission;
    private const string grtPermission;
     GrTeleportData grTeleportData;
     int lastGridTested;
     List<SpawnPosition> spawnGrid;
     List<Cooldown> coolDowns;
     List<RustUser> rustUsers;
     List<DeathNotice> pendingDeathNotice;
    private bool debug;
     class GrTeleportData
    {
        public int CooldownInSeconds { get; set; }
        public bool AvoidWater { get; set; }
        public bool AllowBuildingBlocked { get; set; }
        public bool AllowOffCargoship { get; set; }
        public int LimitPerDay { get; set; }
        public string RestrictedZones { get; set; }
        public int DistanceToWarnConstructions { get; set; }
        public float AllowableWaterDepth { get; set; }
        public bool DisplayDeathLocation { get; set; }
        public int SecondsBeforeTeleport { get; set; }
        public float MinHealthForTeleport { get; set; }
        public float MinThirstForTeleport { get; set; }
        public float MinHungerForTeleport { get; set; }
        public float MinTemperatureForTeleport { get; set; }
        public float MaxTemperatureForTeleport { get; set; }
        public Dictionary<string, GroupData> groupData;
    }

     class SpawnPosition
    {
        const float aboveGoundPosition;
        public Vector3 Position;
        public Vector3 GroundPosition;
        public string GridReference;
        public SpawnPosition(Vector3 position);
        public bool isPositionAboveWater();
        public float WaterDepthAtPosition();
         Vector3 GetGroundPosition(Vector3 sourcePos);
    }

     class Cooldown
    {
        public string name;
        public int cooldownPeriodSeconds;
        public DateTime lastUse;
        public DateTime expirtyDateTime;
        public Cooldown(string PlayerName, int CoolDownInSeconds);
    }

     class RustUser
    {
        public BasePlayer Player { get; set; }
        public int TeleportsRemaining { get; set; }
        public DateTime ResetDateTime { get; set; }
        public int CoolDownInSeconds { get; set; }
        public bool isWaitingForTeleport { get; set; }
    }

     class GroupData
    {
        public int coolDownPeriodSeconds { get; set; }
        public int dailyTeleports { get; set; }
        public string groupName { get; set; }
        public int SecondsBeforeTeleport { get; set; }
        public float MinHealthForTeleport { get; set; }
        public float MinThirstForTeleport { get; set; }
        public float MinHungerForTeleport { get; set; }
        public float MinTemperatureForTeleport { get; set; }
        public float MaxTemperatureForTeleport { get; set; }
    }

     class DeathNotice
    {
        public BasePlayer Player { get; set; }
        public Vector3 DeathLocation { get; set; }
        public string DeathGridReference { get; set; }
    }

     void Init();
    protected override void LoadDefaultMessages();
     void OnEntityBuilt(Planner plan, GameObject go);
    protected override void LoadDefaultConfig();
     void OnEntityDeath(BaseEntity entity, HitInfo info);
     void OnPlayerRespawned(BasePlayer player);
    [ChatCommand("grt")]
     void chatCommandGrt(BasePlayer player, string command, string[] args);
    [ConsoleCommand("grt.testpos")]
     void ccGrtTestPos(ConsoleSystem.Arg arg);
    [ConsoleCommand("grt.nextspawn")]
     void ccGrtNextspawn(ConsoleSystem.Arg arg);
    [ConsoleCommand("grt.prevspawn")]
     void ccGrtPrevspawn(ConsoleSystem.Arg arg);
    [ChatCommand("setcooldown")]
     void chmSetCooldown(BasePlayer player, string command, string[] args);
    [ChatCommand("setwaterdepth")]
     void chmsetwaterdepth(BasePlayer player, string command, string[] args);
    [ChatCommand("toggledeathmessage")]
     void chmToggletoggledeathmessage(BasePlayer player, string command, string[] args);
    [ChatCommand("setconstruction")]
     void chmSetconstruction(BasePlayer player, string command, string[] args);
    [ChatCommand("setdailylimit")]
     void chmSetDailyLimit(BasePlayer player, string command, string[] args);
    [ChatCommand("addzone")]
     void chmAddzone(BasePlayer player, string command, string[] args);
    [ChatCommand("removezone")]
     void chmRemoveZone(BasePlayer player, string command, string[] args);
    [ChatCommand("clearzones")]
     void chmClearZones(BasePlayer player, string command, string[] args);
    [ChatCommand("togglebuildingblocked")]
     void chmtogglebuildingblocked(BasePlayer player, string command, string[] args);
    [ChatCommand("togglecargoship")]
     void chmtogglecargoship(BasePlayer player, string command, string[] args);
    [ChatCommand("toggleavoidwater")]
     void chmSetAvoidWater(BasePlayer player, string command, string[] args);
    [ChatCommand("setgroup")]
     void chmSetGroup(BasePlayer player, string command, string[] args);
    [ChatCommand("getgroups")]
     void chmGetGroups(BasePlayer player, string command, string[] args);
    [ChatCommand("clearcooldown")]
     void chmClearCooldown(BasePlayer player, string command, string[] args);
    [ChatCommand("clearlimit")]
     void chmClearLimit(BasePlayer player, string command, string[] args);
    private bool TeleportToGridReference(BasePlayer player, string gridReference, bool avoidWater);
    private bool IsGridReferenceAboveWater(string gridReference);
     string GetGridReference(Vector3 position);
     List<SpawnPosition> CreateSpawnGrid();
    private string CanPlayerTeleport(BasePlayer player);
     bool TestCanGrtTeleport(BasePlayer player, string gr);
     int GridIndexFromReference(string gridReference);
     bool TestRestrictedZone(string gridReference);
     Cooldown GetCooldown(string playerName);
     List<SpawnPosition> OrigCreateSpawnGrid();
     void StartSleeping(BasePlayer player);
     void Teleport(BasePlayer player, Vector3 position, bool startSleeping);
     bool CheckAccess(BasePlayer player, string command, string sPermission, bool onErrorDisplayMessageToUser);
     void AddToCoolDown(string userName, int seconds);
     bool AreThereCupboardsWithinDistance(Vector3 position, int distance);
     RustUser GetOrCreateUser(BasePlayer player, GroupData groupData);
     Vector3 GetActualGroundPosition(Vector3 sourcePos);
     GroupData GetUsersGroupDataOrDefault(BasePlayer user);
}

 class GrTeleportData
{
    public int CooldownInSeconds { get; set; }
    public bool AvoidWater { get; set; }
    public bool AllowBuildingBlocked { get; set; }
    public bool AllowOffCargoship { get; set; }
    public int LimitPerDay { get; set; }
    public string RestrictedZones { get; set; }
    public int DistanceToWarnConstructions { get; set; }
    public float AllowableWaterDepth { get; set; }
    public bool DisplayDeathLocation { get; set; }
    public int SecondsBeforeTeleport { get; set; }
    public float MinHealthForTeleport { get; set; }
    public float MinThirstForTeleport { get; set; }
    public float MinHungerForTeleport { get; set; }
    public float MinTemperatureForTeleport { get; set; }
    public float MaxTemperatureForTeleport { get; set; }
    public Dictionary<string, GroupData> groupData;
}

 class SpawnPosition
{
    const float aboveGoundPosition;
    public Vector3 Position;
    public Vector3 GroundPosition;
    public string GridReference;
    public SpawnPosition(Vector3 position);
    public bool isPositionAboveWater();
    public float WaterDepthAtPosition();
     Vector3 GetGroundPosition(Vector3 sourcePos);
}

 class Cooldown
{
    public string name;
    public int cooldownPeriodSeconds;
    public DateTime lastUse;
    public DateTime expirtyDateTime;
    public Cooldown(string PlayerName, int CoolDownInSeconds);
}

 class RustUser
{
    public BasePlayer Player { get; set; }
    public int TeleportsRemaining { get; set; }
    public DateTime ResetDateTime { get; set; }
    public int CoolDownInSeconds { get; set; }
    public bool isWaitingForTeleport { get; set; }
}

 class GroupData
{
    public int coolDownPeriodSeconds { get; set; }
    public int dailyTeleports { get; set; }
    public string groupName { get; set; }
    public int SecondsBeforeTeleport { get; set; }
    public float MinHealthForTeleport { get; set; }
    public float MinThirstForTeleport { get; set; }
    public float MinHungerForTeleport { get; set; }
    public float MinTemperatureForTeleport { get; set; }
    public float MaxTemperatureForTeleport { get; set; }
}

 class DeathNotice
{
    public BasePlayer Player { get; set; }
    public Vector3 DeathLocation { get; set; }
    public string DeathGridReference { get; set; }
}


```

---

## GSTBanCtrl by mrcameron999 - BanCtrl is a centralised ban list for your servers. Easily manage your bans, across all servers

```csharp
using ConVar;
using Facepunch.Extend;
using Facepunch.Math;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("GST BanCtrl", "Cameron", "1.0.4")]
[Description("Centralised ban list for multiple servers.")]
public class GSTBanCtrl : CovalencePlugin
{
    private Dictionary<string, CachedPlayer> CachedJoins;
    private readonly string apiUrl;
    private Dictionary<string, string> headers;
    private int port;
    private void Init();
    private void LoadConfigData();
    private void SubmitNewBan(AddBanClass newBan, IPlayer admin, Action<AddBanClass> successCallBack);
    private bool TryGetBanExpiry(string arg, int n, IPlayer iplayer, long expiry, string durationSuffix);
    private long GetTimestamp(string arg, int iArg, long def);
    [Command("Ban")]
    private void OverrideBanCommand(IPlayer iplayer, string command, string[] args);
    [Command("AddAllBans")]
    private void AddAllBans(IPlayer iplayer, string command, string[] args);
    private void OnPlayerBanned(string name, ulong id, string address, string reason);
    private void OnUserApprove(Network.Connection connection);
    private class AddBanClass
    {
        public string SteamId { get; set; }
        public string Reason { get; set; }
        public DateTime? ExpireTime { get; set; }
        public int OrgId { get; set; }
        public int? ServerId { get; set; }
        public string BannedBy { get; set; }
        public string NiceBanId { get; set; }
        public int ServerPort { get; set; }
        public bool DontSendRconKick { get; set; }
    }

    private class CachedPlayer
    {
        public string reason { get; set; }
        public DateTime timeOfAdd { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
}

private class AddBanClass
{
    public string SteamId { get; set; }
    public string Reason { get; set; }
    public DateTime? ExpireTime { get; set; }
    public int OrgId { get; set; }
    public int? ServerId { get; set; }
    public string BannedBy { get; set; }
    public string NiceBanId { get; set; }
    public int ServerPort { get; set; }
    public bool DontSendRconKick { get; set; }
}

private class CachedPlayer
{
    public string reason { get; set; }
    public DateTime timeOfAdd { get; set; }
}


```

---

## GSTReport by mrcameron999 - GST Reports is an extension of the in-game F7 report feature with Discord and GST integration.

```csharp
using ConVar;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("GST Report", "mrcameron999", "1.0.5")]
[Description("Extends F7 reporting with Discord Linker and gameservertools.com integration")]
public class GSTReport : CovalencePlugin
{
    private readonly string connection;
    private bool loggingEnabled;
    private readonly float timeout;
    private int port;
    private Dictionary<string, string> headers;
    private Dictionary<ulong, List<string>> messages;
    private void Init();
    private void LoadConfigData();
    private void OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private void OnPlayerReported(BasePlayer reporter, string targetName, string targetId, string subject, string message, string type);
    private ReportType GetTypeIdFromType(string type);
    protected override void LoadDefaultConfig();
}


```

---

## GuardedCrate by Bazz3l - Spawn High value loot guarded by scientists at random or specified locations.

```csharp
using System.Collections.Generic;
using System.Globalization;
using System.Collections;
using System.Text;
using System.Linq;
using System;
using Oxide.Core.Plugins;
using Oxide.Core;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json;
using GuardedCrateEx;
using UnityEngine;
using Rust;

Oxide.Plugins
[Info("Guarded Crate", "Bazz3l", "2.0.5")]
[Description("Spawn high value loot guarded by scientists at random or specified locations.")]
internal class GuardedCrate : RustPlugin
{
    [PluginReference]
    private Plugin NpcSpawn;
    private Plugin Clans;
    private Plugin ZoneManager;
    private Plugin HackableLock;
    private Plugin GUIAnnouncements;
    private const string PERM_USE;
    private const TerrainTopology.Enum BLOCKED_TOPOLOGY;
    private const string MARKER_PREFAB;
    private const string CRATE_PREFAB;
    private const string PLANE_PREFAB;
    private ConfigData _configData;
    private StoredData _storedData;
    private static GuardedCrate _instance;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class ConfigData
    {
        [JsonProperty("command name")]
        public string CommandName;
        [JsonProperty("enable auto spawning events")]
        public bool EnableAutoStart;
        [JsonProperty("auto start event after x duration")]
        public float AutoStartDuration;
        [JsonProperty("message notification settings")]
        public MessageSettings MessageSettings;
        [JsonProperty("zone manager settings")]
        public ZoneMangerSettings ZoneManagerSettings;
        public static ConfigData DefaultConfig();
    }

    private class MessageSettings
    {
        [JsonProperty("enable toast message")]
        public bool EnableToast;
        [JsonProperty("enable chat message")]
        public bool EnableChat;
        [JsonProperty("enable chat prefix")]
        public bool EnableChatPrefix;
        [JsonProperty("custom chat message icon (steam64)")]
        public ulong ChatIcon;
        [JsonProperty("enable gui announcements plugin from umod.org")]
        public bool EnableGuiAnnouncements;
        [JsonProperty("gui announcements text color")]
        public string GuiAnnouncementsTextColor;
        [JsonProperty("gui announcements background color")]
        public string GuiAnnouncementsBgColor;
    }

    private class ZoneMangerSettings
    {
        [JsonProperty("enable ignored zones")]
        public bool EnabledIgnoredZones;
        [JsonProperty("ignore these zone ids, leave empty to exclude all zones")]
        public List<string> IgnoredZones;
    }

    private void LoadDefaultData();
    private void LoadData();
    private void SaveData();
    private class StoredData
    {
        public List<EventEntry> CrateEventEntries;
        [JsonIgnore]
        private string[] _eventNames;
        [JsonIgnore]
        public string[] EventNames { get; set; }
        [JsonIgnore]
        public bool IsValid { get; set; }
        public EventEntry FindEventByName(string eventName);
    }

    private class EventEntry
    {
        [JsonProperty("event display name)")]
        public string EventName;
        [JsonProperty("event duration")]
        public float EventDuration;
        [JsonProperty("enable lock to player when completing the event")]
        public bool EnableLockToPlayer;
        [JsonProperty("enable clan tag")]
        public bool EnableClanTag;
        [JsonProperty("enable auto hacking of crate when an event is finished")]
        public bool EnableAutoHack;
        [JsonProperty("hackable locked crate")]
        public float HackSeconds;
        [JsonProperty("enable marker")]
        public bool EnableMarker;
        [JsonProperty("marker color 1")]
        public string MapMarkerColor1;
        [JsonProperty("marker color 2")]
        public string MapMarkerColor2;
        [JsonProperty("marker radius")]
        public float MapMarkerRadius;
        [JsonProperty("marker opacity")]
        public float MapMarkerOpacity;
        [JsonProperty("enable loot table")]
        public bool EnableLootTable;
        [JsonProperty("min loot items")]
        public int LootMinAmount;
        [JsonProperty("max loot items")]
        public int LootMaxAmount;
        [JsonProperty("enable eliminate all guards before looting")]
        public bool EnableEliminateGuards;
        [JsonProperty("guard spawn amount")]
        public int GuardAmount;
        [JsonProperty("guard spawn config")]
        public GuardConfig GuardConfig;
        [JsonProperty("create loot items")]
        public List<ItemEntry> LootTable;
    }

    private class ItemEntry
    {
        public string DisplayName;
        public string Shortname;
        public ulong SkinID;
        public int MinAmount;
        public int MaxAmount;
        public static List<ItemEntry> SaveItems(ItemContainer container);
        public Item CreateItem();
    }

    private class GuardConfig
    {
        public string Name;
        public List<WearEntry> WearItems;
        public List<BeltEntry> BeltItems;
        public string Kit;
        public float Health;
        public float RoamRange;
        public float ChaseRange;
        public float SenseRange;
        public float AttackRangeMultiplier;
        public bool CheckVisionCone;
        public float VisionCone;
        public float DamageScale;
        public float TurretDamageScale;
        public float AimConeScale;
        public bool HostileTargetsOnly;
        public bool DisableRadio;
        public bool CanRunAwayWater;
        public bool CanSleep;
        public float SleepDistance;
        public float Speed;
        public bool AboveOrUnderGround;
        public bool Stationary;
        public float MemoryDuration;
        [JsonIgnore]
        public JObject Parsed;
        public class BeltEntry
        {
            public string ShortName;
            public ulong SkinID;
            public int Amount;
            public string Ammo;
            public List<string> Mods;
            public static List<BeltEntry> SaveItems(ItemContainer container);
        }

        public class WearEntry
        {
            public string ShortName;
            public ulong SkinID;
            public static List<WearEntry> SaveItems(ItemContainer container);
        }

        public void CacheConfig();
    }

    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void Init();
    private void Unload();
    private void OnEntityDeath(ScientistNPC scientist, HitInfo info);
    private void OnEntityKill(ScientistNPC scientist);
    private void OnEntityKill(LootContainer container);
    private void OnEntityKill(CargoPlane plane);
    private object CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private static class SpawnManager
    {
        private static readonly LayerMask InterestedLayers;
        private static List<ZoneInfo> _ignoredZones;
        private static List<Collider> _tempColliders;
        private static List<Vector3> _spawnPoints;
        private static Coroutine _spawnRoutine;
        public static void Initialize();
        public static void OnUnload();
        public static void FindSpawnPoints();
        private static IEnumerator GenerateSpawnPoints(bool exclusionZones, int attempts);
        public static Vector3 GetSpawnPoint();
        private static void GetIgnoredZones();
        private static void AddIgnoredZone(string zoneId);
        private static bool TestSpawnPoint(Vector3 spawnPoint);
        private static bool IsBlockedTopology(Vector3 spawnPoint);
        private static bool IsIgnoredZone(Vector3 spawnPoint);
        private static bool IsSpawnPointValid(Vector3 spawnPoint);
    }

    private class ZoneInfo
    {
        public Vector3 Position;
        public float Radius;
        public ZoneInfo();
        public ZoneInfo(Vector3 position, float radius);
        public bool IsInBounds(Vector3 position);
    }

    private static class GuardedCrateManager
    {
        private static readonly List<GuardedCrateInstance> GuardedCrateInstances;
        public static void OnUnload();
        public static bool HasIntersectingEvent(Vector3 position);
        public static void CleanupInstances();
        public static void RegisterInstance(GuardedCrateInstance crateInstance);
        public static void UnregisterInstance(GuardedCrateInstance crateInstance);
    }

    private bool TryStartEvent(string eventName);
    private class GuardedCrateInstance : MonoBehaviour
    {
        public List<BaseEntity> guardSpawnInstances;
        public MapMarkerGenericRadius markerSpawnInstance;
        public HackableLockedCrate crateSpawnInstance;
        public CargoPlane planeSpawnInstance;
        public GuardConfig guardConfig;
        public List<ItemEntry> lootTable;
        private EventState eventState;
        public string eventName;
        public float eventSeconds;
        public bool enableLockToPlayer;
        public bool enableClanTag;
        public bool enableAutoHack;
        public float hackSeconds;
        public bool enableMarker;
        public Color markerColor1;
        public Color markerColor2;
        public float markerRadius;
        public float markerOpacity;
        public bool enableEliminateGuards;
        public int guardAmount;
        public bool enableLootTable;
        public int minLootAmount;
        public int maxLootAmount;
        public float thinkEvery;
        public float lastThinkTime;
        public float timePassed;
        public bool timeEnded;
        public static void CreateInstance(EventEntry eventEntry, Vector3 position);
        public static void RemoveInstance(GuardedCrateInstance crateInstance);
        private void FixedUpdate();
        private IEnumerator Initialize();
        public void Dispose();
        public void EventStartup();
        public void EventComplete(BasePlayer player);
        public void EventFailed();
        public void EventEnded(bool forced);
        private void EventWinner(BasePlayer player);
        private void UnlockCrates();
        public IEnumerator SpawnEntities();
        private IEnumerator SpawnPlane();
        private IEnumerator SpawnGuards();
        private IEnumerator SpawnGuard(Vector3 position);
        private IEnumerator SpawnMarker();
        private IEnumerator SpawnCrate();
        private void ClearGuards();
        private void ClearMarker();
        private void ClearCrate(bool completed);
        private void ClearCargoPlane();
        private void ClearEntities();
        public object CanPopulateCrate();
        private List<ItemEntry> GenerateLoot();
        private void RefillCrate();
        public object CanHackCrate(BasePlayer player);
        public void OnGuardKilled(ScientistNPC scientist, BasePlayer player);
        public void OnPlaneKilled(CargoPlane plane);
        public void OnCrateKilled(LootContainer container);
    }

    private class CargoPlaneComponent : MonoBehaviour
    {
        public GuardedCrateInstance guardedCrate;
        public CargoPlane cargoPlane;
        public bool hasDropped;
        private void Awake();
        private void Update();
    }

    private static class EntitiesCache
    {
        private static Dictionary<BaseEntity, GuardedCrateInstance> Entities;
        public static void OnUnload();
        public static GuardedCrateInstance FindCrateInstance(BaseEntity entity);
        public static void CreateEntity(BaseEntity entity, GuardedCrateInstance component);
        public static void RemoveEntity(BaseEntity entity);
    }

    private static class Notification
    {
        public static void MessagePlayer(ConsoleSystem.Arg arg, string langKey, object[] args);
        public static void MessagePlayer(BasePlayer player, string langKey, object[] args);
        public static void MessagePlayers(string langKey, object[] args);
    }

    private void GuardedCrateConsoleCommand(ConsoleSystem.Arg arg);
    private void EventCommandCommands(BasePlayer player, string command, string[] args);
    private void DisplayHelpText(BasePlayer player);
    private readonly HashSet<string> _hooks;
    private void SubscribeToHooks(int count);
    private object CanPopulateLoot(HackableLockedCrate crate);
    private object OnNpcRustEdit(ScientistNPC npc);
    private bool API_IsGuardedCrateCargoPlane(CargoPlane entity);
    private bool API_IsGuardedCrateEntity(BaseEntity entity);
}

private class ConfigData
{
    [JsonProperty("command name")]
    public string CommandName;
    [JsonProperty("enable auto spawning events")]
    public bool EnableAutoStart;
    [JsonProperty("auto start event after x duration")]
    public float AutoStartDuration;
    [JsonProperty("message notification settings")]
    public MessageSettings MessageSettings;
    [JsonProperty("zone manager settings")]
    public ZoneMangerSettings ZoneManagerSettings;
    public static ConfigData DefaultConfig();
}

private class MessageSettings
{
    [JsonProperty("enable toast message")]
    public bool EnableToast;
    [JsonProperty("enable chat message")]
    public bool EnableChat;
    [JsonProperty("enable chat prefix")]
    public bool EnableChatPrefix;
    [JsonProperty("custom chat message icon (steam64)")]
    public ulong ChatIcon;
    [JsonProperty("enable gui announcements plugin from umod.org")]
    public bool EnableGuiAnnouncements;
    [JsonProperty("gui announcements text color")]
    public string GuiAnnouncementsTextColor;
    [JsonProperty("gui announcements background color")]
    public string GuiAnnouncementsBgColor;
}

private class ZoneMangerSettings
{
    [JsonProperty("enable ignored zones")]
    public bool EnabledIgnoredZones;
    [JsonProperty("ignore these zone ids, leave empty to exclude all zones")]
    public List<string> IgnoredZones;
}

private class StoredData
{
    public List<EventEntry> CrateEventEntries;
    [JsonIgnore]
    private string[] _eventNames;
    [JsonIgnore]
    public string[] EventNames { get; set; }
    [JsonIgnore]
    public bool IsValid { get; set; }
    public EventEntry FindEventByName(string eventName);
}

private class EventEntry
{
    [JsonProperty("event display name)")]
    public string EventName;
    [JsonProperty("event duration")]
    public float EventDuration;
    [JsonProperty("enable lock to player when completing the event")]
    public bool EnableLockToPlayer;
    [JsonProperty("enable clan tag")]
    public bool EnableClanTag;
    [JsonProperty("enable auto hacking of crate when an event is finished")]
    public bool EnableAutoHack;
    [JsonProperty("hackable locked crate")]
    public float HackSeconds;
    [JsonProperty("enable marker")]
    public bool EnableMarker;
    [JsonProperty("marker color 1")]
    public string MapMarkerColor1;
    [JsonProperty("marker color 2")]
    public string MapMarkerColor2;
    [JsonProperty("marker radius")]
    public float MapMarkerRadius;
    [JsonProperty("marker opacity")]
    public float MapMarkerOpacity;
    [JsonProperty("enable loot table")]
    public bool EnableLootTable;
    [JsonProperty("min loot items")]
    public int LootMinAmount;
    [JsonProperty("max loot items")]
    public int LootMaxAmount;
    [JsonProperty("enable eliminate all guards before looting")]
    public bool EnableEliminateGuards;
    [JsonProperty("guard spawn amount")]
    public int GuardAmount;
    [JsonProperty("guard spawn config")]
    public GuardConfig GuardConfig;
    [JsonProperty("create loot items")]
    public List<ItemEntry> LootTable;
}

private class ItemEntry
{
    public string DisplayName;
    public string Shortname;
    public ulong SkinID;
    public int MinAmount;
    public int MaxAmount;
    public static List<ItemEntry> SaveItems(ItemContainer container);
    public Item CreateItem();
}

private class GuardConfig
{
    public string Name;
    public List<WearEntry> WearItems;
    public List<BeltEntry> BeltItems;
    public string Kit;
    public float Health;
    public float RoamRange;
    public float ChaseRange;
    public float SenseRange;
    public float AttackRangeMultiplier;
    public bool CheckVisionCone;
    public float VisionCone;
    public float DamageScale;
    public float TurretDamageScale;
    public float AimConeScale;
    public bool HostileTargetsOnly;
    public bool DisableRadio;
    public bool CanRunAwayWater;
    public bool CanSleep;
    public float SleepDistance;
    public float Speed;
    public bool AboveOrUnderGround;
    public bool Stationary;
    public float MemoryDuration;
    [JsonIgnore]
    public JObject Parsed;
    public class BeltEntry
    {
        public string ShortName;
        public ulong SkinID;
        public int Amount;
        public string Ammo;
        public List<string> Mods;
        public static List<BeltEntry> SaveItems(ItemContainer container);
    }

    public class WearEntry
    {
        public string ShortName;
        public ulong SkinID;
        public static List<WearEntry> SaveItems(ItemContainer container);
    }

    public void CacheConfig();
}

public class BeltEntry
{
    public string ShortName;
    public ulong SkinID;
    public int Amount;
    public string Ammo;
    public List<string> Mods;
    public static List<BeltEntry> SaveItems(ItemContainer container);
}

public class WearEntry
{
    public string ShortName;
    public ulong SkinID;
    public static List<WearEntry> SaveItems(ItemContainer container);
}

private static class SpawnManager
{
    private static readonly LayerMask InterestedLayers;
    private static List<ZoneInfo> _ignoredZones;
    private static List<Collider> _tempColliders;
    private static List<Vector3> _spawnPoints;
    private static Coroutine _spawnRoutine;
    public static void Initialize();
    public static void OnUnload();
    public static void FindSpawnPoints();
    private static IEnumerator GenerateSpawnPoints(bool exclusionZones, int attempts);
    public static Vector3 GetSpawnPoint();
    private static void GetIgnoredZones();
    private static void AddIgnoredZone(string zoneId);
    private static bool TestSpawnPoint(Vector3 spawnPoint);
    private static bool IsBlockedTopology(Vector3 spawnPoint);
    private static bool IsIgnoredZone(Vector3 spawnPoint);
    private static bool IsSpawnPointValid(Vector3 spawnPoint);
}

private class ZoneInfo
{
    public Vector3 Position;
    public float Radius;
    public ZoneInfo();
    public ZoneInfo(Vector3 position, float radius);
    public bool IsInBounds(Vector3 position);
}

private static class GuardedCrateManager
{
    private static readonly List<GuardedCrateInstance> GuardedCrateInstances;
    public static void OnUnload();
    public static bool HasIntersectingEvent(Vector3 position);
    public static void CleanupInstances();
    public static void RegisterInstance(GuardedCrateInstance crateInstance);
    public static void UnregisterInstance(GuardedCrateInstance crateInstance);
}

private class GuardedCrateInstance : MonoBehaviour
{
    public List<BaseEntity> guardSpawnInstances;
    public MapMarkerGenericRadius markerSpawnInstance;
    public HackableLockedCrate crateSpawnInstance;
    public CargoPlane planeSpawnInstance;
    public GuardConfig guardConfig;
    public List<ItemEntry> lootTable;
    private EventState eventState;
    public string eventName;
    public float eventSeconds;
    public bool enableLockToPlayer;
    public bool enableClanTag;
    public bool enableAutoHack;
    public float hackSeconds;
    public bool enableMarker;
    public Color markerColor1;
    public Color markerColor2;
    public float markerRadius;
    public float markerOpacity;
    public bool enableEliminateGuards;
    public int guardAmount;
    public bool enableLootTable;
    public int minLootAmount;
    public int maxLootAmount;
    public float thinkEvery;
    public float lastThinkTime;
    public float timePassed;
    public bool timeEnded;
    public static void CreateInstance(EventEntry eventEntry, Vector3 position);
    public static void RemoveInstance(GuardedCrateInstance crateInstance);
    private void FixedUpdate();
    private IEnumerator Initialize();
    public void Dispose();
    public void EventStartup();
    public void EventComplete(BasePlayer player);
    public void EventFailed();
    public void EventEnded(bool forced);
    private void EventWinner(BasePlayer player);
    private void UnlockCrates();
    public IEnumerator SpawnEntities();
    private IEnumerator SpawnPlane();
    private IEnumerator SpawnGuards();
    private IEnumerator SpawnGuard(Vector3 position);
    private IEnumerator SpawnMarker();
    private IEnumerator SpawnCrate();
    private void ClearGuards();
    private void ClearMarker();
    private void ClearCrate(bool completed);
    private void ClearCargoPlane();
    private void ClearEntities();
    public object CanPopulateCrate();
    private List<ItemEntry> GenerateLoot();
    private void RefillCrate();
    public object CanHackCrate(BasePlayer player);
    public void OnGuardKilled(ScientistNPC scientist, BasePlayer player);
    public void OnPlaneKilled(CargoPlane plane);
    public void OnCrateKilled(LootContainer container);
}

private class CargoPlaneComponent : MonoBehaviour
{
    public GuardedCrateInstance guardedCrate;
    public CargoPlane cargoPlane;
    public bool hasDropped;
    private void Awake();
    private void Update();
}

private static class EntitiesCache
{
    private static Dictionary<BaseEntity, GuardedCrateInstance> Entities;
    public static void OnUnload();
    public static GuardedCrateInstance FindCrateInstance(BaseEntity entity);
    public static void CreateEntity(BaseEntity entity, GuardedCrateInstance component);
    public static void RemoveEntity(BaseEntity entity);
}

private static class Notification
{
    public static void MessagePlayer(ConsoleSystem.Arg arg, string langKey, object[] args);
    public static void MessagePlayer(BasePlayer player, string langKey, object[] args);
    public static void MessagePlayers(string langKey, object[] args);
}

GuardedCrateEx
internal static class CustomUtils
{
    public static T CreateObjectWithComponent(Vector3 position, Quaternion rotation, string name);
    public static T CreateEntity(string prefab, Vector3 position, Quaternion rotation);
    public static Color GetColor(string hex);
}

internal static class ExtensionMethods
{
    public static bool IsPluginReady(Plugin plugin);
    public static string ToStringTime(float seconds);
    public static Vector3 GetPointAround(Vector3 origin, int layers, float radius, float angle);
    public static void SafeKill(BaseEntity entity);
    public static void SafeClear(ItemContainer container);
}


```

---

## Guardian by WhiteDragon - Protects the server from various annoyances, cheats, and macro attacks.

```csharp
using Facepunch;
using Facepunch.Math;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("Guardian", "WhiteDragon", "1.7.9")]
[Description("Protects the server from various annoyances, cheats, and macro attacks.")]
 class Guardian : CovalencePlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin PlaytimeTracker;
    private static Guardian _instance;
    private class ActionQueue
    {
        private Queue<Action> actions;
        public ActionQueue(float interval);
        public void Clear();
        public void Enqueue(Action callback);
        private void Scan();
    }

    private class Admin
    {
        public class Settings
        {
            public bool Broadcast;
            public bool Bypass;
            public Settings();
        }

    }

    private class AntiCheat
    {
        public class Settings
        {
            public Aim.Settings Aim;
            public FireRate.Settings FireRate;
            public Gravity.Settings Gravity;
            public MeleeRate.Settings MeleeRate;
            public Recoil.Settings Recoil;
            public Server.Settings Server;
            public Stash.Settings Stash;
            public Trajectory.Settings Trajectory;
            public WallHack.Settings WallHack;
            public Settings();
            public void Validate();
        }

        private const Key category;
        private const float epsilon;
        public static void Configure();
        public static void Unload();
        public class Aim
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public AimTrigger Trigger;
                public bool Warn;
                public Settings();
                public class AimTrigger
                {
                    public bool Animal;
                    public bool Bradley;
                    public bool Helicopter;
                    public bool NPC;
                }

                public void Validate();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            private static float aim_distance;
            private static float headshot_scale;
            private static float hit_scale;
            private static float pvp_distance;
            private static float sensitivity_lo;
            private static float sensitivity_hi;
            private static float spin_angle;
            private static float swing_angle;
            private class History
            {
                public HitArea boneArea;
                public Counter headshot;
                public Counter hit;
                public HashSet<int> hits;
                public Counter repeat;
                private static readonly Dictionary<ulong, History> histories;
                private History();
                public static void Clear();
                public static bool Contains(ulong userid);
                public static History Get(ulong userid);
            }

            public static void Configure();
            public static void Trigger(BaseEntity entity, HitInfo info);
            public static void Unload();
            public static void Update(Weapon weapon);
        }

        public class FireRate
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            private static float sensitivity;
            private class History
            {
                public int entity;
                public DateTime fired;
                public float repeat;
                private static readonly Dictionary<ulong, History> histories;
                private History();
                public static void Clear();
                public static History Get(ulong userid);
            }

            public static void Configure();
            private static uint Percent(float min, float delay, float max);
            public static void Trigger(BaseEntity entity, HitInfo info);
            public static void Unload();
        }

        public class Gravity
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            private static float sensitivity_lo;
            private static float sensitivity_md;
            private static float sensitivity_hi;
            private class History
            {
                private static readonly Dictionary<ulong, float> histories;
                public static void Clear();
                public static float Set(ulong userid, float new_value);
            }

            public static void Configure();
            private static bool Check(BasePlayer player);
            public static void Scan();
            public static void Trigger(BasePlayer player, float amount, bool scanned);
            public static void Unload();
        }

        public class MeleeRate
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            private static float sensitivity;
            private class History
            {
                private static readonly Dictionary<ulong, DateTime> histories;
                public static void Clear();
                public static float Set(ulong userid, DateTime new_value);
            }

            public static void Configure();
            private static uint Percent(float delay, float max);
            public static void Trigger(BasePlayer player, HitInfo info);
            public static void Unload();
        }

        public class Recoil
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const float latency_min;
            private const float latency_max;
            private const float recoil_max;
            private const float recoil_range;
            private const float recoil_scale;
            private const float repeat_interval;
            private const Key type;
            private static readonly Violation violation;
            private static ulong reset_limit;
            private static float sensitivity;
            private class History
            {
                public Counter count_r;
                public Counter count_x;
                public Counter count_y;
                public DateTime fired;
                public ulong repeats;
                private static readonly Dictionary<ulong, History> histories;
                private History();
                public static void Clear();
                public static History Get(BasePlayer player);
            }

            public static void Configure();
            private static float Latency(BasePlayer player, Weapon weapon);
            private static uint Percent(float value);
            public static void Trigger(BasePlayer player, Weapon weapon);
            public static void Unload();
        }

        public class Server
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private static Vector3 sensitivity;
            private const Key type;
            private static readonly Violation violation;
            public static void Configure();
            public static void Scan();
            public static void Trigger(BasePlayer player, AntiHackType ahtype, float amount);
            public static void Unload();
        }

        public class Stash
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            private class History
            {
                private static readonly Dictionary<ulong, List<Vector3>> histories;
                public static void Clear();
                public static float Set(ulong userid, Vector3 position);
            }

            private static float sensitivity;
            public static void Configure();
            public static void Subscribe();
            public static void Trigger(BasePlayer player, StashContainer stash);
            public static void Unload();
        }

        public class Trajectory
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            private static float sensitivity;
            private static float sensitivity_lo;
            private static float sensitivity_hi;
            public static void Configure();
            private static float Ratio(float a, float b);
            private static uint Percent(float value);
            public static void Trigger(BaseCombatEntity entity, HitInfo info);
            public static void Unload();
        }

        public class WallHack
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            public static void Configure();
            public static void Trigger(BasePlayer player, HitInfo info);
            public static void Unload();
        }

    }

    private class AntiFlood
    {
        public class Settings
        {
            public Chat.Settings Chat;
            public Command.Settings Command;
            public ItemDrop.Settings ItemDrop;
            public Settings();
            public void Validate();
        }

        private const Key category;
        public static void Configure();
        public static void Unload();
        public class Chat
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            public static void Configure();
            public static ulong Cooldown(ulong playerid);
            public static void Subscribe();
            public static bool Trigger(ulong playerid);
            public static void Unload();
            public static void Violation(BasePlayer player, string details);
        }

        public class Command
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            public static void Configure();
            public static ulong Cooldown(ulong playerid);
            public static void Subscribe();
            public static bool Trigger(ulong playerid);
            public static void Unload();
            public static void Violation(BasePlayer player, string details);
        }

        public class ItemDrop
        {
            public class Settings
            {
                public bool Ban;
                public ulong Cooldown;
                public bool Enabled;
                public float Sensitivity;
                public bool Warn;
                public Settings();
                public Violation.Settings Validate(ulong max);
            }

            private const Key type;
            private static readonly Violation violation;
            private static readonly Dictionary<ulong, int> history;
            public static void Configure();
            public static ulong CoolDown(ulong playerid, int itemid);
            public static void Subscribe();
            public static bool Trigger(ulong playerid, int itemid);
            public static void Unload();
            public static void Violation(BasePlayer player, string details);
        }

    }

    public class API
    {
        public class Settings
        {
            public string ApiKey;
            public bool Enabled;
            public Settings();
            public void Validate();
        }

    }

    public class Ban
    {
        public class Settings
        {
            public bool Inherit;
            public bool Teleport;
            public BanTime Time;
            public Settings();
            public class BanTime
            {
                public bool Enforce;
                public bool Multiply;
                public ulong Seconds;
            }

            public void Validate();
        }

    }

    private class Chat
    {
        public static void Admin(Key key, Dictionary<string, string> parameters);
        public static void Broadcast(Key key, Dictionary<string, string> parameters);
        public static void Console(BasePlayer player, Key key, Dictionary<string, string> parameters);
        public static void Reply(IPlayer iplayer, Key key, Dictionary<string, string> parameters);
        public static void Send(BasePlayer player, Key key, Dictionary<string, string> parameters);
    }

    private void CommandReceive(IPlayer iplayer, string command, string[] args);
    private class Command
    {
        private static readonly Dictionary<string, Info> commands;
        private static List<Info> info;
        private class Info
        {
            public Action<IPlayer, string, string[]> Action { get; set; }
            public List<string> Aliases { get; set; }
            public Key Title { get; set; }
            public Info(Action<IPlayer, string, string[]> action, Key title, string[] aliases);
        }

        public static void Load();
        public static void Receive(IPlayer iplayer, string command, string[] args);
        public static void Unload();
        private static void Config(IPlayer iplayer, string command, string[] args);
        private static string Config(string subsection);
        private static bool Config(IPlayer iplayer, string[] args, string setting, T value, Action callback);
        private static void Help(IPlayer iplayer, string command, string[] args);
        private static void Ip(IPlayer iplayer, string command, string[] args);
        private static void Ip(IPlayer iplayer, Key key, IP.NetworkInfo network, bool add);
        private static void Ip(IPlayer iplayer, Key key, List<string> list);
        private static void Log(IPlayer iplayer, string command, string[] args);
        private static void Server(IPlayer iplayer, string command, string[] args);
        private static void Teleport(IPlayer iplayer, string command, string[] args);
        private static void Users(IPlayer iplayer, string command, string[] args);
        private static void Users(IPlayer iplayer, Key key, HashSet<ulong> userids);
        private static void Vpn(IPlayer iplayer, string command, string[] args);
        private static void Vpn(IPlayer iplayer, Key key, string address);
    }

    private static Configuration config;
    private class Configuration
    {
        public Admin.Settings Admin;
        public AntiCheat.Settings AntiCheat;
        public AntiFlood.Settings AntiFlood;
        public Ban.Settings Ban;
        public Cripple.Settings Cripple;
        public Discord.Settings Discord;
        public Entity.Settings Entity;
        public IP.Settings IP;
        public Log.Settings Log;
        public Steam.Settings Steam;
        public User.Settings User;
        public Version.Settings Version;
        public Violation.Settings Violation;
        public VPN.Settings VPN;
        private static bool corrupt;
        private static bool dirty;
        private static bool upgraded;
        public static void Clamp(T value, T min, T max);
        public static void Load();
        public static void Save();
        public static void SetDirty();
        public static void SetUpgrade(bool upgrade);
        public static void Unload();
        public static bool Upgraded();
        public static void Validate(T value, Func<T> initializer, Action validator);
        private static void Validate();
    }

    private class Counter
    {
        private uint count;
        private uint delay;
        public Counter();
        public void Decrement();
        public void Increment();
        public uint Percent();
        public float Ratio(bool inverse);
        public float Ratio(float scale, bool inverse);
        public ulong Total();
    }

    public class Cripple
    {
        public class Settings
        {
            public bool Heal;
            public bool Inherit;
            public Settings();
        }

    }

    private class Data
    {
        private static DataFileSystem data;
        private static string path;
        public static void Close();
        public static bool Exists(string name);
        public static void Open();
        public static T ReadObject(string name);
        public static void WriteObject(string name, T value);
    }

    private class DataFile
    {
        private readonly Dictionary<TKey, TValue> data;
        private bool dirty;
        private string name;
        public DataFile(string name);
        public void Add(TKey key, TValue value);
        public bool Contains(TKey key);
        public void Clear();
        public bool Exists();
        public void ForEach(Action<TKey, TValue> action);
        public TValue Get(TKey key, TValue default_value);
        public bool IsDirty();
        public bool IsEmpty();
        public void Load();
        public void Load(string name);
        public void Remove(TKey key);
        public void Save();
        public void Save(string name);
        public void SetDirty(bool value);
        public void SetName(string name);
        public void Unload();
    }

    private class Discord
    {
        public class Settings
        {
            public bool Enabled;
            public string WebHook;
            public DiscordFilters Filters;
            public Settings();
            public class DiscordFilter
            {
                public bool Enabled;
                public string WebHook;
                public DiscordFilter();
                public string URL();
                public void Validate();
            }

            public class DiscordFilters
            {
                public DiscordFilter AntiCheat;
                public DiscordFilter AntiFlood;
                public DiscordFilter IP;
                public DiscordFilter Steam;
                public DiscordFilter VPN;
                public DiscordFilters();
                public void Validate();
            }

            public void Validate();
        }

        public static void Send(string category, Dictionary<string, object> message);
        private static string Select(string category);
        public static void Subscribe();
    }

    private class Entity
    {
        public class Settings
        {
            public EntityDamage Damage;
            public Settings();
            public class EntityDamage
            {
                public float Animal;
                public float Bradley;
                public float Building;
                public float Entity;
                public float Friend;
                public float Helicopter;
                public float NPC;
                public float Player;
                public float Team;
                public float Trap;
                public EntityDamage();
                public void Validate();
            }

            public void Validate();
        }

        public class Damage
        {
            private static readonly DamageTypeList cleared;
            public static void Cancel(HitInfo info);
            public static object Scale(HitInfo info, BasePlayer attacker, BaseCombatEntity victim);
            private static object Scale(HitInfo info, BasePlayer attacker, BasePlayer victim);
            private static object Scale(HitInfo info, float scale);
        }

        public static BasePlayer GetAttacker(BaseCombatEntity entity, HitInfo info);
        public static string GetName(BaseEntity entity);
        private static string GetName(BaseEntity entity, Type type);
        private static string GetPrefabName(BaseEntity entity);
        public static Type GetType(BaseEntity entity, string name);
        public static Type GetType(BaseEntity entity);
        public class Health
        {
            private static readonly Dictionary<int, float> health;
            public static bool Changed(BaseEntity entity);
            public static void Clear();
            public static void Clear(BaseEntity entity);
        }

        public static void Unload();
    }

    private class Fire
    {
        private static Dictionary<int, BasePlayer> fires;
        public static void Ignite(FireBall fire, BasePlayer initiator);
        public static BasePlayer Initiator(FireBall fire);
        public static void Spread(FireBall fire, FireBall spread);
        public static void Quench(FireBall fire);
        public static void Unload();
    }

    private class Generic
    {
        public static T Clamp(T value, T min, T max);
    }

    private new class Hooks
    {
        private static HashSet<string> subscribed;
        private static HashSet<string> unsubscribed;
        public class Base
        {
            public static void Subscribe();
        }

        public class Core
        {
            public static void Subscribe();
        }

        public class Dynamic
        {
            public static void Subscribe();
        }

        public static void Load();
        public static void Subscribe(string hook);
        public static void Unload();
        private static void Unsubscribe();
        public static void Unsubscribe(string hook);
    }

    private void Init();
    private void Loaded();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private object CanBypassQueue(Connection connection);
    private object CanCraft(ItemCrafter crafter, ItemBlueprint bp, int amount);
    private object CanLootEntity(BasePlayer looter, DroppedItemContainer target);
    private object CanLootEntity(BasePlayer looter, LootableCorpse target);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private object CanSeeStash(BasePlayer player, StashContainer stash);
    private object CanUserLogin(string name, string id, string address);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityKill(BaseNetworkable entity);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnFireBallDamage(FireBall fireball, BaseCombatEntity target, HitInfo info);
    private void OnFireBallSpread(FireBall fireball, BaseEntity entity);
    private void OnFlameExplosion(FlameExplosive explosive, BaseEntity entity);
    private void OnFlameThrowerBurn(FlameThrower flamethrower, BaseEntity entity);
    private void OnGroupPermissionGranted(string name, string perm);
    private void OnGroupPermissionRevoked(string name, string perm);
    private void OnGuardianViolation(string playerid, Dictionary<string, string> details);
    private void OnItemDropped(Item item, BaseEntity entity);
    private void OnLootEntity(BasePlayer looter, BaseEntity target);
    private void OnLootPlayer(BasePlayer looter, BasePlayer target);
    private object OnPlayerAttack(BasePlayer player, HitInfo info);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnPlayerViolation(BasePlayer player, AntiHackType ahtype, float amount);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnUserBanned(string playername, string playerid, string playerip, string reason);
    private void OnUserPermissionGranted(string id, string perm);
    private void OnUserPermissionRevoked(string id, string perm);
    private void OnUserUnbanned(string playername, string playerid, string playerip);
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile itemmod, ProtoBuf.ProjectileShoot projectiles);
    private class IP
    {
        public class Settings
        {
            public IpFilter Filter;
            public IpViolation Violation;
            public Settings();
            public class IpFilter
            {
                public ulong Cooldown;
                public IpFilter();
                public void Validate();
            }

            public class IpViolation
            {
                public bool Ban;
                public bool Enabled;
                public IpViolation();
            }

            public void Validate();
        }

        private static readonly DataFile<string, uint> allows;
        private static readonly DataFile<string, bool> blocks;
        private static readonly DataFile<string, uint> denies;
        private static readonly HashSet<ulong> empty;
        private static readonly DataFile<string, DateTime> filter;
        private static TimeSpan filter_cooldown;
        private static readonly DataFile<string, HashSet<ulong>> users;
        private static readonly Violation violation;
        public class NetworkInfo
        {
            public string Address { get; set; }
            public uint Bits { get; set; }
            public NetworkInfo(string address, uint bits);
        }

        private static uint Bits(string address);
        public static void Block(NetworkInfo network);
        public static void Block(string address, ulong userid);
        public static void Bypass(NetworkInfo network);
        public static void Bypass(string address);
        private static uint Decimal(string address);
        public static bool Cooldown(string address);
        public static void Configure();
        public static bool Filter(string address, ulong userid);
        public static HashSet<ulong> Find(string address);
        private static List<string> Get(DataFile<string, uint> data, string address);
        public static List<string> GetAllows(string address);
        public static List<string> GetDenies(string address);
        public static bool IsAllowed(string address);
        public static bool IsBlocked(string address);
        public static bool IsDenied(string address);
        private static bool IsMatched(DataFile<string, uint> data, string address);
        public static bool IsValid(string address, bool full, bool wildcard);
        public static void Load();
        private static bool Match(uint ip, uint network, uint bits);
        private static List<string> Match(NetworkInfo network);
        public static NetworkInfo Network(string address);
        public static string Parse(string address);
        public static void Save();
        private static bool Set(DataFile<string, uint> data, NetworkInfo network, bool add);
        public static bool SetAllow(NetworkInfo network, bool add);
        public static bool SetDeny(NetworkInfo network, bool add);
        public static string ToString(uint ip);
        public static void Unblock(NetworkInfo network);
        public static void Unblock(string address);
        public static void Unload();
        public static void Update(string address, ulong userid);
        private static void Violation(string address, ulong userid);
    }

    private new class Log
    {
        public class Settings
        {
            public LogAntiCheat AntiCheat;
            public LogAntiFlood AntiFlood;
            public LogIp IP;
            public LogProjectile Projectile;
            public LogUser User;
            public LogVpn VPN;
            public Settings();
            public class LogAntiCheat
            {
                public bool Aim;
                public bool Gravity;
                public bool MeleeRate;
                public bool Recoil;
                public bool Server;
                public bool Stash;
                public bool Trajectory;
            }

            public class LogAntiFlood
            {
                public bool ItemDrop;
            }

            public class LogIp
            {
                public bool Filter;
                public LogIp();
            }

            public class LogProjectile
            {
                public bool Collapse;
                public bool Verbose;
                public LogProjectile();
            }

            public class LogUser
            {
                public bool Bypass;
                public bool Connect;
                public LogUser();
            }

            public class LogVpn
            {
                public bool Check;
                public LogVpn();
            }

            public void Validate();
        }

        public static void Console(Key key, Dictionary<string, string> parameters);
        public static void Error(string message);
        public static void Warning(string message);
    }

    private class Map
    {
        public class Building
        {
            public static bool HasPrivilege(DecayEntity entity);
            public static bool InFoundation(Vector3 position);
            public static bool IsNearby(Vector3 position, bool check_privilege);
        }

        public class Cave
        {
            public static bool IsInside(Vector3 position);
        }

        public class Collider
        {
            public static string Info(Vector3 position);
        }

        public class Entities
        {
            public static bool InRange(Vector3 position, float radius);
        }

        public static string Grid(Vector3 position);
        private class Hit
        {
            private static string Collider(RaycastHit hit);
            public static bool IsBunker(RaycastHit hit);
            public static bool IsBunker(string collider);
            public static bool IsCave(RaycastHit hit);
            public static bool IsCave(string collider);
            public static bool IsCorridor(RaycastHit hit);
            public static bool IsCorridor(string collider);
            public static bool IsDuct(RaycastHit hit);
            public static bool IsDuct(string collider);
            public static bool IsFoundation(RaycastHit hit);
            public static bool IsFoundation(string collider);
            public static bool IsMine(RaycastHit hit);
            public static bool IsMine(string collider);
            public static bool IsRoad(RaycastHit hit);
            public static bool IsRoad(string collider);
            public static bool IsRock(RaycastHit hit);
            public static bool IsRock(string collider);
            public static bool IsStairwell(RaycastHit hit);
            public static bool IsStairwell(string collider);
            public static bool IsTunnel(RaycastHit hit);
            public static bool IsTunnel(string collider);
            public static bool IsUnderground(RaycastHit hit);
            public static bool IsUnderground(string collider);
        }

        public static bool InRange(Vector3 a, Vector3 b, float distance);
        public static bool InRange2D(Vector3 a, Vector3 b, float distance);
        public static void Load();
        public class Monument
        {
            private static SparseMap global;
            private static SparseMap tunnel;
            public static bool IsNearby(Vector3 position);
            public static bool IsTunnel(Vector3 position);
            public static void Load();
            private static float Range(MonumentInfo monument);
            public static void Unload();
        }

        public class Position
        {
            private static bool HasCheck(Check checks, Check check);
            public static Vector3 Random(Check checks);
            private static Vector3 Random(float min_x, float max_x, float min_z, float max_z);
        }

        public class Road
        {
            public static bool IsNearby(Vector3 position);
        }

        public class Rock
        {
            public static bool IsInside(Vector3 position, bool check_cave);
        }

        public static Vector3 Surface(Vector3 position, float offset);
        public class Terrain
        {
            public static float Height(Vector3 position);
            public static bool IsInside(Vector3 position, bool check_cave);
            public static bool IsSurface(Vector3 position);
            public static Vector3 Level(Vector3 position, float offset);
        }

        public class Underground
        {
            public static bool IsInside(Vector3 position);
        }

        public static void Unload();
        public class Water
        {
            public static float Height(Vector3 position);
            public static bool IsSurface(Vector3 position);
            public static Vector3 Level(Vector3 position, float offset);
        }

    }

    private class Permissions
    {
        private static string PERMISSION_ADMIN;
        private static string PERMISSION_ALL;
        private static string PERMISSION_IGNORE;
        private static string PERMISSION_PREFIX;
        private static readonly HashSet<ulong> all;
        private static readonly HashSet<ulong> admin;
        private static readonly HashSet<ulong> ignore;
        public static bool Admin(ulong userid, bool forced);
        public static bool All(ulong userid, bool forced);
        public class Bypass
        {
            private static string PERMISSION_PREFIX;
            private static string PERMISSION_STEAM;
            private static string PERMISSION_VPN;
            private static readonly HashSet<ulong> steam;
            private static readonly HashSet<ulong> vpn;
            public class AntiCheat
            {
                private static readonly HashSet<ulong> aim;
                private static readonly HashSet<ulong> firerate;
                private static readonly HashSet<ulong> gravity;
                private static readonly HashSet<ulong> meleerate;
                private static readonly HashSet<ulong> recoil;
                private static readonly HashSet<ulong> server;
                private static readonly HashSet<ulong> stash;
                private static readonly HashSet<ulong> trajectory;
                private static readonly HashSet<ulong> wallhack;
                private static string PERMISSION_AIM;
                private static string PERMISSION_FIRERATE;
                private static string PERMISSION_GRAVITY;
                private static string PERMISSION_MELEERATE;
                private static string PERMISSION_PREFIX;
                private static string PERMISSION_RECOIL;
                private static string PERMISSION_SERVER;
                private static string PERMISSION_STASH;
                private static string PERMISSION_TRAJECTORY;
                private static string PERMISSION_WALLHACK;
                public static void Load();
                public static bool Aim(ulong userid, bool forced);
                public static bool FireRate(ulong userid, bool forced);
                public static bool Gravity(ulong userid, bool forced);
                public static bool MeleeRate(ulong userid, bool forced);
                public static bool Recoil(ulong userid, bool forced);
                public static void Reset(ulong userid);
                public static bool Server(ulong userid, bool forced);
                public static bool Stash(ulong userid, bool forced);
                public static bool Trajectory(ulong userid, bool forced);
                public static void Unload();
                public static void Update(ulong userid);
                public static bool WallHack(ulong userid, bool forced);
            }

            public static void Load();
            public static void Reset(ulong userid);
            public static bool Steam(ulong userid, bool forced);
            public static void Unload();
            public static void Update(ulong userid);
            public static bool Vpn(ulong userid, bool forced);
        }

        public class Command
        {
            private static readonly HashSet<ulong> config;
            private static readonly HashSet<ulong> ip;
            private static readonly HashSet<ulong> server;
            private static readonly HashSet<ulong> tp;
            private static readonly HashSet<ulong> vpn;
            private static string PERMISSION_CONFIG;
            private static string PERMISSION_IP;
            private static string PERMISSION_PREFIX;
            private static string PERMISSION_SERVER;
            private static string PERMISSION_TP;
            private static string PERMISSION_VPN;
            public static void Load();
            public static bool Config(ulong userid, bool forced);
            public static bool Ip(ulong userid, bool forced);
            public static void Reset(ulong userid);
            public static bool Server(ulong userid, bool forced);
            public static bool Tp(ulong userid, bool forced);
            public static void Unload();
            public static void Update(ulong userid);
            public static bool Vpn(ulong userid, bool forced);
        }

        public static string[] Groups(ulong userid);
        private static bool HasPermission(ulong userid, string permission);
        public static bool HasPrefix(string permission);
        public static bool Ignore(ulong userid, bool forced);
        public static void Load();
        public static void Reset(ulong userid);
        public static void Unload();
        public static void Update(ulong userid);
        private static bool Update(ulong userid, string permission, HashSet<ulong> cache);
    }

    private class Projectile
    {
        public class Log
        {
            private static readonly Queue<Entry> expired;
            private static readonly Dictionary<ulong, Queue<Entry>> history;
            private static readonly Dictionary<ulong, Dictionary<int, Entry>> pending;
            private static readonly Queue<Request> request;
            private static readonly Queue<Entry> reserve;
            static readonly StringBuilder buffer;
            private class Entry
            {
                public float aim_angle;
                public float aim_pvp;
                public float aim_range;
                public ulong aim_violations;
                public string attacker;
                public ulong attacker_id;
                public ulong firerate_violations;
                public float hit_distance;
                public HitArea hit_location;
                public bool pvp;
                public float recoil_pitch;
                public ulong recoil_repeats;
                public float recoil_swing;
                public ulong recoil_violations;
                public float recoil_yaw;
                public bool ricochet;
                public float speed;
                public DateTime timestamp;
                public float trajectory;
                public ulong trajectory_violations;
                public string victim;
                public bool violations;
                public ulong wallhack_violations;
                public string weapon;
                private static readonly Queue<Entry> pool;
                private Entry(DateTime timestamp);
                private Entry Default();
                public string Format(bool collapse, bool id, bool time);
                public static Entry Get(DateTime timestamp);
                public void Release();
                private Entry Set(DateTime timestamp);
                public static void Unload();
            }

            private class Request
            {
                public IPlayer actor;
                public int lines;
                public ulong userid;
            }

            public static void Add(Weapon weapon);
            public static void Expire(Weapon weapon);
            public static void Get(IPlayer actor, ulong userid, int lines);
            public static float GetAimAngle(ulong userid, int id);
            public static bool GetAimPvp(ulong userid, int id);
            public static float GetHitDistance(ulong userid, int id);
            public static HitArea GetHitLocation(ulong userid, int id);
            public static bool GetRicochet(ulong userid, int id);
            public static string GetVictim(ulong userid, int id);
            public static void Send();
            public static void SetAim(ulong userid, int id, float aim_angle, float aim_pvp, float aim_range, bool pvp, bool ricochet);
            public static void SetAimViolations(ulong userid, int id, ulong aim_violations, bool add);
            public static void SetAttacker(ulong userid, int id, string attacker, ulong attaker_id);
            public static void SetFireRateViolations(ulong userid, int id, ulong firerate_violations);
            public static void SetHit(ulong userid, int id, float hit_distance, HitArea hit_location);
            public static void SetRecoil(ulong userid, int id, float recoil_pitch, ulong recoil_repeats, float recoil_yaw, float recoil_swing);
            public static void SetRecoilViolations(ulong userid, int id, ulong recoil_violations);
            public static void SetTrajectory(ulong userid, int id, float trajectory);
            public static void SetTrajectoryViolations(ulong userid, int id, ulong trajectory_violations);
            public static void SetVictim(ulong userid, int id, string victim);
            public static void SetWallHackViolations(ulong userid, int id, ulong wallhack_violations);
            public static void SetWeapon(ulong userid, int id, float speed, string weapon);
            private static void TrySetEntry(ulong userid, int id, Action<Entry> set);
            public static void Unload();
        }

        private static readonly List<Weapon> expired;
        private static readonly Dictionary<ulong, Dictionary<int, Weapon>> reverse;
        private static readonly Dictionary<ulong, Queue<Weapon>> weapons;
        public static void Add(Weapon weapon);
        public static void Load();
        public static void Unload();
        public static Weapon Weapon(ulong userid, int projectileid);
    }

    private class SparseMap
    {
        private Dictionary<Texel, Cell> map;
        private float map_scale;
        public SparseMap(float scale);
        public void Clear();
        public int Count();
        public void Reset(float scale);
        private void Reset(Pixel pixel);
        public void Reset(Vector3 position);
        public float Scale();
        private int Scale(float n);
        private void Set(Pixel pixel);
        public void Set(Vector3 position);
        public int Set(Vector3 origin, float range);
        private void SetScale(float scale);
        private bool Test(Pixel pixel);
        public bool Test(Vector3 position);
    }

    private class Steam
    {
        public class Settings
        {
            public API.Settings API;
            public SteamBan Ban;
            public SteamGame Game;
            public SteamProfile Profile;
            public SteamShare Share;
            public SteamViolation Violation;
            public Settings();
            public class SteamBan
            {
                public bool Active;
                public bool Community;
                public ulong Days;
                public bool Economy;
                public ulong Game;
                public ulong VAC;
            }

            public class SteamGame
            {
                public ulong Count;
                public ulong Hours;
            }

            public class SteamProfile
            {
                public bool Invalid;
                public bool Limited;
                public bool Private;
            }

            public class SteamShare
            {
                public bool Family;
            }

            public class SteamViolation
            {
                public bool Ban;
                public bool Enabled;
                public bool Warn;
                public SteamViolation();
            }

            public void Validate();
        }

        private static uint appid;
        private static ActionQueue checks;
        private static readonly Violation violation;
        public static void Check(ulong userid);
        public static void Configure();
        public static void Load();
        public static void Unload();
        private static void Violation(ulong userid, Key type, string details, bool ban);
        private class Ban
        {
            [JsonProperty("players")]
            public Player[] Players;
            public class Player
            {
                [JsonProperty("CommunityBanned")]
                public bool CommunityBanned;
                [JsonProperty("VACBanned")]
                public bool VacBanned;
                [JsonProperty("NumberOfVACBans")]
                public ulong NumberOfVacBans;
                [JsonProperty("DaysSinceLastBan")]
                public ulong DaysSinceLastBan;
                [JsonProperty("NumberOfGameBans")]
                public ulong NumberOfGameBans;
                [JsonProperty("EconomyBan")]
                public string EconomyBan;
            }

            [JsonIgnore]
            private static readonly string api;
            [JsonIgnore]
            public static bool check;
            public static void Check(ulong userid);
        }

        private class Game
        {
            [JsonProperty("response")]
            public Content Response;
            public class Content
            {
                [JsonProperty("game_count")]
                public ulong GameCount;
                [JsonProperty("games")]
                public Game[] Games;
                public class Game
                {
                    [JsonProperty("appid")]
                    public uint AppId;
                    [JsonProperty("playtime_forever")]
                    public ulong PlaytimeForever;
                }

            }

            [JsonIgnore]
            private static readonly string api;
            [JsonIgnore]
            public static bool check;
            public static void Check(ulong userid, bool private_profile, bool is_sharing);
        }

        private class Profile
        {
            private static readonly string api;
            public static bool check;
            public static void Check(ulong userid, bool private_profile);
        }

        private class Share
        {
            [JsonProperty("response")]
            public Content Response;
            public class Content
            {
                [JsonProperty("lender_steamid")]
                public ulong LenderSteamId;
            }

            [JsonIgnore]
            private static readonly string api;
            [JsonIgnore]
            public static bool check;
            public static void Check(ulong userid, bool private_profile);
        }

        private class Summaries
        {
            [JsonProperty("response")]
            public Content Response;
            public class Content
            {
                [JsonProperty("players")]
                public Player[] Players;
                public class Player
                {
                    [JsonProperty("communityvisibilitystate")]
                    public int CommunityVisibilityState;
                }

            }

            [JsonIgnore]
            private static readonly string api;
            [JsonIgnore]
            public static bool check;
            public static void Check(ulong userid);
        }

    }

    private class Text
    {
        private static readonly Dictionary<string, Dictionary<Key, string>> decorated;
        private static readonly Dictionary<string, Dictionary<Key, string>> unadorned;
        private static string server_language;
        private class RegEx
        {
            public static Regex clean1;
            public static Regex clean2;
            public static Regex clean3;
            public static Regex markup;
            public static Regex spaces;
            public static void Load();
            public static void Unload();
        }

        public static string Actor(IPlayer actor);
        public static string Actor(IPlayer actor, BasePlayer player);
        public static string Actor(IPlayer actor, IPlayer iplayer);
        public static string Actor(IPlayer actor, ulong userid);
        public static string Actor(IPlayer actor, string language);
        public static string BodyPart(HitArea area);
        public static string BodyPart(HitArea area, BasePlayer player);
        public static string BodyPart(HitArea area, IPlayer iplayer);
        public static string BodyPart(HitArea area, ulong userid);
        public static string BodyPart(HitArea area, string language);
        public class Duration
        {
            public static string Hours(TimeSpan time);
            public static string Hours(TimeSpan time, BasePlayer player);
            public static string Hours(TimeSpan time, IPlayer iplayer);
            public static string Hours(TimeSpan time, ulong userid);
            public static string Hours(TimeSpan time, string language);
            public static string Short(TimeSpan time);
            public static string Short(TimeSpan time, BasePlayer player);
            public static string Short(TimeSpan time, IPlayer iplayer);
            public static string Short(TimeSpan time, ulong userid);
            public static string Short(TimeSpan time, string language);
        }

        public static string Get(Key key, Dictionary<string, string> parameters);
        public static string Get(Key key, BasePlayer player, Dictionary<string, string> parameters);
        public static string Get(Key key, IPlayer iplayer, Dictionary<string, string> parameters);
        public static string Get(Key key, ulong userid, Dictionary<string, string> parameters);
        public static string Get(Key key, string language, Dictionary<string, string> parameters);
        private static string Get(Key key, Dictionary<Key, string> cache, Dictionary<string, string> parameters);
        public static string GetPlain(Key key, Dictionary<string, string> parameters);
        public static string GetPlain(Key key, BasePlayer player, Dictionary<string, string> parameters);
        public static string GetPlain(Key key, IPlayer iplayer, Dictionary<string, string> parameters);
        public static string GetPlain(Key key, ulong userid, Dictionary<string, string> parameters);
        public static string GetPlain(Key key, string language, Dictionary<string, string> parameters);
        public static string Language(string userid);
        public static void Load();
        private static Dictionary<Key, string> Messages(Dictionary<string, Dictionary<Key, string>> cache, string language);
        public static bool ParseTime(string message, ulong seconds);
        private static void RegisterMessages();
        private static string Replace(string message, Dictionary<string, string> parameters);
        private static string Strip(string message);
        public static string Sanitize(string message);
        private static string Trim(string message);
        public static void Unload();
    }

    private class Timers
    {
        private static readonly List<Timer> timers;
        public static void Add(float interval, Action callback);
        public static void Destroy();
    }

    private class User
    {
        public class Settings
        {
            public UserBypass Bypass;
            public UserFriend Friend;
            public UserTeam Team;
            public Settings();
            public class UserBypass
            {
                public ulong DaysSinceBan;
                public bool Enabled;
                public ulong HoursPlayed;
                public bool Multiply;
                public UserBypass();
                public void Validate();
            }

            public class UserFriend
            {
                public bool Damage;
            }

            public class UserTeam
            {
                public bool Damage;
            }

            public void Validate();
        }

        private class Info
        {
            public ulong userid;
            public string name;
            public HashSet<string> names;
            public string address;
            public HashSet<string> addresses;
            public DateTime time_connected;
            public DateTime time_disconnected;
            public TimeSpan time_played;
            public ulong ban_count;
            public string ban_reason;
            public DateTime ban_time;
            public DateTime ban_timer;
            public bool is_banned;
            public ulong cripple_count;
            public string cripple_reason;
            public DateTime cripple_time;
            public DateTime cripple_timer;
            public bool is_crippled;
            public Vector3 l_position;
            public Vector3 v_position;
            [JsonIgnore]
            public DateTime access_time;
            [JsonIgnore]
            public ulong attacked;
            [JsonIgnore]
            public ulong attacker;
            [JsonIgnore]
            public bool dirty;
            [JsonIgnore]
            public BasePlayer player;
            [JsonIgnore]
            public Stack<ulong> teleport;
            [JsonIgnore]
            public List<ulong> victims;
            public Info();
        }

        private static readonly DataFile<string, HashSet<ulong>> names;
        private static ActionQueue teleport;
        private static readonly Dictionary<ulong, Info> users;
        private static void Action(IPlayer actor, Key action, Info user);
        public static string Address(ulong userid);
        public static void AssignAttacker(BasePlayer attacker, BasePlayer victim);
        public static void AssignVictim(BasePlayer victim);
        private static void AssignVictims(Info user);
        public static void Ban(ulong userid, string reason, ulong seconds, IPlayer actor);
        public static ulong BanCount(ulong userid);
        private static void BanInherit(ulong userid, string name, string address, Info copy);
        public static void BanReset(ulong userid, IPlayer actor);
        private static void BanTeleport(Info user, ulong count);
        public static DateTime BanTime(ulong userid);
        private static void BanTimeEnforce(Info user);
        private static bool CanBypass(Info user);
        public static object CanConnect(string name, string id, string address);
        public static bool CanFly(BasePlayer player);
        private static bool CanLoot(ulong userid, ulong targetid);
        public static object CanLoot(BasePlayer player, ulong targetid);
        public static void Cripple(ulong userid, string reason, ulong seconds, IPlayer actor);
        private static void CrippleInherit(ulong userid, string name, string address, Info copy);
        public static void CrippleReset(ulong userid, IPlayer actor);
        public static bool Exists(ulong userid);
        public static HashSet<ulong> Find(string text);
        public static Vector3 GetLastSeenPosition(ulong userid);
        public static Vector3 GetViolationPosition(ulong userid);
        public static bool HasAuthLevel(BasePlayer player);
        public static bool HasAdminFlag(BasePlayer player);
        private static bool HasConnected(Info user);
        public static bool HasDeveloperFlag(BasePlayer player);
        public static bool HasParent(BasePlayer player);
        public static string InfoText(ulong userid);
        public static string InfoText(ulong userid, BasePlayer player);
        public static string InfoText(ulong userid, IPlayer iplayer);
        public static string InfoText(ulong userid, ulong playerid);
        public static string InfoText(ulong userid, string language);
        public static bool IsBanned(ulong userid);
        private static bool IsConnected(Info user);
        public static bool IsConnected(ulong userid);
        public static bool IsCrippled(ulong userid);
        public static bool IsFriend(BasePlayer a, BasePlayer b);
        public static bool IsFriend(BasePlayer player, ulong userid);
        public static bool IsInactive(BasePlayer player);
        private static bool IsStale(Info user);
        public static bool IsTeamMate(BasePlayer a, BasePlayer b);
        public static bool IsTeamMate(BasePlayer player, ulong userid);
        public static void Kick(ulong userid, string reason, IPlayer actor);
        public static void Load();
        private static Info Load(ulong userid);
        public static string Name(ulong userid);
        public static void OnBanned(ulong userid, bool banned);
        public static void OnConnected(BasePlayer player);
        public static void OnDisconnected(BasePlayer player);
        public static void OnLoot(BasePlayer player, ulong targetid);
        public static void Pardon(ulong userid, IPlayer actor);
        public static void Pardon(IPlayer actor);
        private static void Pardon(Queue<ulong> userid, IPlayer actor, int total);
        private static void Pardon(Info user, IPlayer actor, bool broadcast);
        public static void Save();
        private static void Save(Info user);
        private static void Scan();
        private static void SetLastSeenPosition(Info user, Vector3 position);
        private static void SetViolationPosition(Info user, Vector3 position);
        public static void SetViolationPosition(ulong userid);
        public static bool ShouldIgnore(BasePlayer player);
        public static string StatusText(ulong userid);
        public static string StatusText(ulong userid, BasePlayer player);
        public static string StatusText(ulong userid, IPlayer iplayer);
        public static string StatusText(ulong userid, ulong playerid);
        public static string StatusText(ulong userid, string language);
        private static string StatusText(Info user, string language);
        public static List<ulong> Team(ulong userid);
        public static TimeSpan TimeBanned(ulong userid);
        private static TimeSpan TimeBanned(Info user);
        public static TimeSpan TimeCrippled(ulong userid);
        private static TimeSpan TimeCrippled(Info user);
        private static TimeSpan TimeRemaining(DateTime time);
        public static TimeSpan TimeOffline(ulong userid);
        private static TimeSpan TimeOffline(Info user);
        public static TimeSpan TimeOnline(ulong userid);
        private static TimeSpan TimeOnline(Info user);
        private static TimeSpan TimeSpent(DateTime time);
        public static TimeSpan TimePlayed(ulong userid);
        private static TimeSpan TimePlayed(Info user);
        public static void Unban(ulong userid, bool manual, IPlayer actor);
        public static void Unban(IPlayer actor);
        private static void Unban(Info user, bool manual, IPlayer actor, bool broadcast);
        public static void Uncripple(ulong userid, bool manual, IPlayer actor);
        public static void Uncripple(IPlayer actor);
        private static void Uncripple(Info user, bool manual, IPlayer actor, bool broadcast);
        public static void Unload();
        public static void Update();
        private static void Update(Info user, string name, string address);
    }

    private new class Version
    {
        public class Settings
        {
            public int Major;
            public int Minor;
            public int Patch;
            public Settings();
            public int Compare(int major, int minor, int patch);
            public void Validate();
        }

        public static string String { get; set; }
    }

    private class Violation
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public float Sensitivity;
            public bool Warn;
            public Settings(bool ban, ulong cooldown, float sensitivity, bool warn);
            public ulong Count(ulong min, ulong max, bool squared);
            public Settings Validate(ulong max);
        }

        private bool ban;
        private Key category;
        private ulong cooldown;
        private ulong count;
        private TimeSpan rate;
        private bool warn;
        private Dictionary<ulong, History> histories;
        private static readonly Dictionary<string, Key> categories;
        private static ActionQueue triggers;
        private static readonly Violation violation;
        private class History
        {
            public DateTime cooldown;
            public ulong count;
            public DateTime time;
            public DateTime warned;
            public ulong warnings;
            public History();
        }

        public Violation(Key category);
        private void Broadcast(ulong userid, Key action, Key type, string details, Dictionary<string, string> hook_details);
        public static Key Category(string category);
        public static void Configure();
        public void Configure(Settings settings, ulong trigger_min, ulong trigger_max, ulong trigger_rate);
        public ulong Cooldown(ulong userid);
        public void Clear();
        private History Get(ulong userid);
        private bool IsFlooding(ulong userid);
        public static void Load();
        private void Reduce(ulong userid);
        private void Reset(ulong userid);
        public bool Trigger(ulong userid);
        public void Trigger(ulong userid, Key type, string details, bool kick, Dictionary<string, string> hook_details);
        public void Trigger(ulong userid, Key type, string details, ulong violations, bool kick, Dictionary<string, string> hook_details);
        private void Triggered(ulong userid, Key type, string details, ulong violations, bool kick, Dictionary<string, string> hook_details);
        public static void Unload();
        public void Warning(ulong userid, Key type, string details, Dictionary<string, string> hook_details);
        public void Zero(ulong userid);
    }

    private class VPN
    {
        public class Settings
        {
            public VpnApi API;
            public VpnCache Cache;
            public VpnCheck Check;
            public VpnViolation Violation;
            public Settings();
            public class VpnApi
            {
                public Guardian.API.Settings GetIpIntel;
                public Guardian.API.Settings IpApi;
                public Guardian.API.Settings IpHub;
                public Guardian.API.Settings IpQualityScore;
                public VpnApi();
                public void Validate();
            }

            public class VpnCache
            {
                public ulong Hours;
                public VpnCache();
            }

            public class VpnCheck
            {
                public bool Enabled;
                public bool Strict;
            }

            public class VpnViolation
            {
                public bool Ban;
                public bool Enabled;
                public bool Warn;
                public VpnViolation();
            }

            public void Validate();
        }

        private static ActionQueue checks;
        private static readonly Violation violation;
        public class API
        {
            public class GetIpIntel
            {
                [JsonProperty("message")]
                public string Message { get; set; }
                [JsonProperty("result")]
                public float Result { get; set; }
                [JsonProperty("status")]
                public string Status { get; set; }
                private const string api;
                public static void Check(string address, ulong userid);
            }

            public class IpApi
            {
                [JsonProperty("hosting")]
                public bool Hosting { get; set; }
                [JsonProperty("proxy")]
                public bool Proxy { get; set; }
                [JsonProperty("status")]
                public string Status { get; set; }
                private const string api;
                public static void Check(string address, ulong userid);
            }

            public class IpHub
            {
                [JsonProperty("block")]
                public int Block { get; set; }
                [JsonIgnore]
                private static readonly Dictionary<string, string> headers;
                private const string api;
                public static void Check(string address, ulong userid);
                public static void Configure();
                public static void Unload();
            }

            public class IpQualityScore
            {
                [JsonProperty("fraud_score")]
                public int FraudScore { get; set; }
                [JsonProperty("message")]
                public string Message { get; set; }
                [JsonProperty("proxy")]
                public bool Proxy { get; set; }
                [JsonProperty("recent_abuse")]
                public bool RecentAbuse { get; set; }
                [JsonProperty("success")]
                public bool Success { get; set; }
                [JsonProperty("vpn")]
                public bool VPN { get; set; }
                private const string api;
                public static void Check(string address, ulong userid);
            }

            public static void Configure();
            public static void Unload();
        }

        private class Cache
        {
            private static readonly DataFile<string, DateTime> blocks;
            private static readonly DataFile<string, DateTime> bypass;
            public static void Block(string address);
            public static void Bypass(string address, ulong _reserved);
            public static bool IsBlocked(string address);
            public static bool IsBypassed(string address);
            private static bool IsCached(DataFile<string, DateTime> cache, string address);
            private static bool IsExpired(DateTime timestamp, DateTime check);
            public static void Load();
            public static void Save();
            public static void Unblock(string address);
            public static void Unload();
            private static void Update();
            private static void Update(DataFile<string, DateTime> cache, DateTime current);
        }

        public static void Bypass(string address);
        public static void Check(string address, ulong userid);
        private static void Check(string address, ulong userid, float delay);
        private static void Check(float delay, string address, ulong userid, Action<string, ulong> callback);
        public static void Configure();
        public static bool IsBlocked(string address);
        public static bool IsBypassed(string address);
        public static void Load();
        public static void Save();
        public static void Unblock(string address);
        public static void Unload();
        private static void Violation(string address, ulong userid, string api);
    }

    private class Weapon
    {
        public float Accuracy { get; set; }
        public Vector3 AimAngle { get; set; }
        public float AimCone { get; set; }
        public float AimSway { get; set; }
        public string AmmoName { get; set; }
        public List<string> Attachments { get; set; }
        public bool Automatic { get; set; }
        public DateTime Fired { get; set; }
        public string Name { get; set; }
        public float Pitch { get; set; }
        public BasePlayer Player { get; set; }
        public Vector3 Position { get; set; }
        public List<int> Projectiles { get; set; }
        public float Range { get; set; }
        public float Repeat { get; set; }
        public bool Shell { get; set; }
        public string ShortName { get; set; }
        public float Speed { get; set; }
        public bool Spread { get; set; }
        public float Swing { get; set; }
        public float Velocity { get; set; }
        public float Yaw { get; set; }
        public float Zoom { get; set; }
        private static readonly Queue<Weapon> pool;
        private static readonly int ArrowBone;
        private static readonly int ArrowFire;
        private static readonly int ArrowHV;
        private static readonly int ArrowWooden;
        private static readonly int GrenadeHE;
        private static readonly int GrenadeShotgun;
        private static readonly int GrenadeSmoke;
        private static readonly int NailgunNails;
        private static readonly int PistolBullet;
        private static readonly int PistolHV;
        private static readonly int PistolIncendiary;
        private static readonly int RifleAmmo;
        private static readonly int RifleExplosive;
        private static readonly int RifleHV;
        private static readonly int RifleIncendiary;
        private static readonly int Rocket;
        private static readonly int RocketHV;
        private static readonly int RocketIncendiary;
        private static readonly int ShellBuckshot;
        private static readonly int ShellHandmade;
        private static readonly int ShellIncendiary;
        private static readonly int ShellSlug;
        private static readonly int AssaultRifle;
        private static readonly int BoltActionRifle;
        private static readonly int CompoundBow;
        private static readonly int Crossbow;
        private static readonly int CustomSMG;
        private static readonly int DoubleBarrelShotgun;
        private static readonly int EokaPistol;
        private static readonly int HuntingBow;
        private static readonly int L96Rifle;
        private static readonly int LR300AssaultRifle;
        private static readonly int M249;
        private static readonly int M39Rifle;
        private static readonly int M92Pistol;
        private static readonly int MP5A4;
        private static readonly int MultipleGrenadeLauncher;
        private static readonly int Nailgun;
        private static readonly int PumpShotgun;
        private static readonly int PythonRevolver;
        private static readonly int Revolver;
        private static readonly int RocketLauncher;
        private static readonly int SemiAutomaticPistol;
        private static readonly int SemiAutomaticRifle;
        private static readonly int Spas12Shotgun;
        private static readonly int Thompson;
        private static readonly int WaterpipeShotgun;
        private class Ammo
        {
            public float AimCone { get; set; }
            public float Range { get; set; }
            public float Velocity { get; set; }
            public Ammo(float aimcone, float range, float velocity);
        }

        private class Info
        {
            public float Accuracy { get; set; }
            public Dictionary<int, Ammo> Ammo { get; set; }
            public bool Automatic { get; set; }
            public string Name { get; set; }
            public Recoil Recoil { get; set; }
            public float Repeat { get; set; }
        }

        private class Recoil
        {
            public float Pitch { get; set; }
            public float Yaw { get; set; }
            public Recoil(float pitch, float yaw);
        }

        private static Dictionary<int, Info> weapons;
        private Weapon();
        private static Weapon Get();
        public static Weapon Get(ulong userid, int projectileid);
        public static Weapon Get(BaseProjectile weapon_fired, BasePlayer player, ProtoBuf.ProjectileShoot fired);
        public static bool IsValid(Item item);
        public static void Load();
        public void Release();
        public void SetSwing(float amount);
        public static void Unload();
    }

    private class WebHook
    {
        public static void Send(string url, string category, string message);
    }

}

private class ActionQueue
{
    private Queue<Action> actions;
    public ActionQueue(float interval);
    public void Clear();
    public void Enqueue(Action callback);
    private void Scan();
}

private class Admin
{
    public class Settings
    {
        public bool Broadcast;
        public bool Bypass;
        public Settings();
    }

}

public class Settings
{
    public bool Broadcast;
    public bool Bypass;
    public Settings();
}

private class AntiCheat
{
    public class Settings
    {
        public Aim.Settings Aim;
        public FireRate.Settings FireRate;
        public Gravity.Settings Gravity;
        public MeleeRate.Settings MeleeRate;
        public Recoil.Settings Recoil;
        public Server.Settings Server;
        public Stash.Settings Stash;
        public Trajectory.Settings Trajectory;
        public WallHack.Settings WallHack;
        public Settings();
        public void Validate();
    }

    private const Key category;
    private const float epsilon;
    public static void Configure();
    public static void Unload();
    public class Aim
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public AimTrigger Trigger;
            public bool Warn;
            public Settings();
            public class AimTrigger
            {
                public bool Animal;
                public bool Bradley;
                public bool Helicopter;
                public bool NPC;
            }

            public void Validate();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        private static float aim_distance;
        private static float headshot_scale;
        private static float hit_scale;
        private static float pvp_distance;
        private static float sensitivity_lo;
        private static float sensitivity_hi;
        private static float spin_angle;
        private static float swing_angle;
        private class History
        {
            public HitArea boneArea;
            public Counter headshot;
            public Counter hit;
            public HashSet<int> hits;
            public Counter repeat;
            private static readonly Dictionary<ulong, History> histories;
            private History();
            public static void Clear();
            public static bool Contains(ulong userid);
            public static History Get(ulong userid);
        }

        public static void Configure();
        public static void Trigger(BaseEntity entity, HitInfo info);
        public static void Unload();
        public static void Update(Weapon weapon);
    }

    public class FireRate
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        private static float sensitivity;
        private class History
        {
            public int entity;
            public DateTime fired;
            public float repeat;
            private static readonly Dictionary<ulong, History> histories;
            private History();
            public static void Clear();
            public static History Get(ulong userid);
        }

        public static void Configure();
        private static uint Percent(float min, float delay, float max);
        public static void Trigger(BaseEntity entity, HitInfo info);
        public static void Unload();
    }

    public class Gravity
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        private static float sensitivity_lo;
        private static float sensitivity_md;
        private static float sensitivity_hi;
        private class History
        {
            private static readonly Dictionary<ulong, float> histories;
            public static void Clear();
            public static float Set(ulong userid, float new_value);
        }

        public static void Configure();
        private static bool Check(BasePlayer player);
        public static void Scan();
        public static void Trigger(BasePlayer player, float amount, bool scanned);
        public static void Unload();
    }

    public class MeleeRate
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        private static float sensitivity;
        private class History
        {
            private static readonly Dictionary<ulong, DateTime> histories;
            public static void Clear();
            public static float Set(ulong userid, DateTime new_value);
        }

        public static void Configure();
        private static uint Percent(float delay, float max);
        public static void Trigger(BasePlayer player, HitInfo info);
        public static void Unload();
    }

    public class Recoil
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const float latency_min;
        private const float latency_max;
        private const float recoil_max;
        private const float recoil_range;
        private const float recoil_scale;
        private const float repeat_interval;
        private const Key type;
        private static readonly Violation violation;
        private static ulong reset_limit;
        private static float sensitivity;
        private class History
        {
            public Counter count_r;
            public Counter count_x;
            public Counter count_y;
            public DateTime fired;
            public ulong repeats;
            private static readonly Dictionary<ulong, History> histories;
            private History();
            public static void Clear();
            public static History Get(BasePlayer player);
        }

        public static void Configure();
        private static float Latency(BasePlayer player, Weapon weapon);
        private static uint Percent(float value);
        public static void Trigger(BasePlayer player, Weapon weapon);
        public static void Unload();
    }

    public class Server
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private static Vector3 sensitivity;
        private const Key type;
        private static readonly Violation violation;
        public static void Configure();
        public static void Scan();
        public static void Trigger(BasePlayer player, AntiHackType ahtype, float amount);
        public static void Unload();
    }

    public class Stash
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        private class History
        {
            private static readonly Dictionary<ulong, List<Vector3>> histories;
            public static void Clear();
            public static float Set(ulong userid, Vector3 position);
        }

        private static float sensitivity;
        public static void Configure();
        public static void Subscribe();
        public static void Trigger(BasePlayer player, StashContainer stash);
        public static void Unload();
    }

    public class Trajectory
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        private static float sensitivity;
        private static float sensitivity_lo;
        private static float sensitivity_hi;
        public static void Configure();
        private static float Ratio(float a, float b);
        private static uint Percent(float value);
        public static void Trigger(BaseCombatEntity entity, HitInfo info);
        public static void Unload();
    }

    public class WallHack
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        public static void Configure();
        public static void Trigger(BasePlayer player, HitInfo info);
        public static void Unload();
    }

}

public class Settings
{
    public Aim.Settings Aim;
    public FireRate.Settings FireRate;
    public Gravity.Settings Gravity;
    public MeleeRate.Settings MeleeRate;
    public Recoil.Settings Recoil;
    public Server.Settings Server;
    public Stash.Settings Stash;
    public Trajectory.Settings Trajectory;
    public WallHack.Settings WallHack;
    public Settings();
    public void Validate();
}

public class Aim
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public AimTrigger Trigger;
        public bool Warn;
        public Settings();
        public class AimTrigger
        {
            public bool Animal;
            public bool Bradley;
            public bool Helicopter;
            public bool NPC;
        }

        public void Validate();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    private static float aim_distance;
    private static float headshot_scale;
    private static float hit_scale;
    private static float pvp_distance;
    private static float sensitivity_lo;
    private static float sensitivity_hi;
    private static float spin_angle;
    private static float swing_angle;
    private class History
    {
        public HitArea boneArea;
        public Counter headshot;
        public Counter hit;
        public HashSet<int> hits;
        public Counter repeat;
        private static readonly Dictionary<ulong, History> histories;
        private History();
        public static void Clear();
        public static bool Contains(ulong userid);
        public static History Get(ulong userid);
    }

    public static void Configure();
    public static void Trigger(BaseEntity entity, HitInfo info);
    public static void Unload();
    public static void Update(Weapon weapon);
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public AimTrigger Trigger;
    public bool Warn;
    public Settings();
    public class AimTrigger
    {
        public bool Animal;
        public bool Bradley;
        public bool Helicopter;
        public bool NPC;
    }

    public void Validate();
    public Violation.Settings Validate(ulong max);
}

public class AimTrigger
{
    public bool Animal;
    public bool Bradley;
    public bool Helicopter;
    public bool NPC;
}

private class History
{
    public HitArea boneArea;
    public Counter headshot;
    public Counter hit;
    public HashSet<int> hits;
    public Counter repeat;
    private static readonly Dictionary<ulong, History> histories;
    private History();
    public static void Clear();
    public static bool Contains(ulong userid);
    public static History Get(ulong userid);
}

public class FireRate
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    private static float sensitivity;
    private class History
    {
        public int entity;
        public DateTime fired;
        public float repeat;
        private static readonly Dictionary<ulong, History> histories;
        private History();
        public static void Clear();
        public static History Get(ulong userid);
    }

    public static void Configure();
    private static uint Percent(float min, float delay, float max);
    public static void Trigger(BaseEntity entity, HitInfo info);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

private class History
{
    public int entity;
    public DateTime fired;
    public float repeat;
    private static readonly Dictionary<ulong, History> histories;
    private History();
    public static void Clear();
    public static History Get(ulong userid);
}

public class Gravity
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    private static float sensitivity_lo;
    private static float sensitivity_md;
    private static float sensitivity_hi;
    private class History
    {
        private static readonly Dictionary<ulong, float> histories;
        public static void Clear();
        public static float Set(ulong userid, float new_value);
    }

    public static void Configure();
    private static bool Check(BasePlayer player);
    public static void Scan();
    public static void Trigger(BasePlayer player, float amount, bool scanned);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

private class History
{
    private static readonly Dictionary<ulong, float> histories;
    public static void Clear();
    public static float Set(ulong userid, float new_value);
}

public class MeleeRate
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    private static float sensitivity;
    private class History
    {
        private static readonly Dictionary<ulong, DateTime> histories;
        public static void Clear();
        public static float Set(ulong userid, DateTime new_value);
    }

    public static void Configure();
    private static uint Percent(float delay, float max);
    public static void Trigger(BasePlayer player, HitInfo info);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

private class History
{
    private static readonly Dictionary<ulong, DateTime> histories;
    public static void Clear();
    public static float Set(ulong userid, DateTime new_value);
}

public class Recoil
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const float latency_min;
    private const float latency_max;
    private const float recoil_max;
    private const float recoil_range;
    private const float recoil_scale;
    private const float repeat_interval;
    private const Key type;
    private static readonly Violation violation;
    private static ulong reset_limit;
    private static float sensitivity;
    private class History
    {
        public Counter count_r;
        public Counter count_x;
        public Counter count_y;
        public DateTime fired;
        public ulong repeats;
        private static readonly Dictionary<ulong, History> histories;
        private History();
        public static void Clear();
        public static History Get(BasePlayer player);
    }

    public static void Configure();
    private static float Latency(BasePlayer player, Weapon weapon);
    private static uint Percent(float value);
    public static void Trigger(BasePlayer player, Weapon weapon);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

private class History
{
    public Counter count_r;
    public Counter count_x;
    public Counter count_y;
    public DateTime fired;
    public ulong repeats;
    private static readonly Dictionary<ulong, History> histories;
    private History();
    public static void Clear();
    public static History Get(BasePlayer player);
}

public class Server
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private static Vector3 sensitivity;
    private const Key type;
    private static readonly Violation violation;
    public static void Configure();
    public static void Scan();
    public static void Trigger(BasePlayer player, AntiHackType ahtype, float amount);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

public class Stash
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    private class History
    {
        private static readonly Dictionary<ulong, List<Vector3>> histories;
        public static void Clear();
        public static float Set(ulong userid, Vector3 position);
    }

    private static float sensitivity;
    public static void Configure();
    public static void Subscribe();
    public static void Trigger(BasePlayer player, StashContainer stash);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

private class History
{
    private static readonly Dictionary<ulong, List<Vector3>> histories;
    public static void Clear();
    public static float Set(ulong userid, Vector3 position);
}

public class Trajectory
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    private static float sensitivity;
    private static float sensitivity_lo;
    private static float sensitivity_hi;
    public static void Configure();
    private static float Ratio(float a, float b);
    private static uint Percent(float value);
    public static void Trigger(BaseCombatEntity entity, HitInfo info);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

public class WallHack
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    public static void Configure();
    public static void Trigger(BasePlayer player, HitInfo info);
    public static void Unload();
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

private class AntiFlood
{
    public class Settings
    {
        public Chat.Settings Chat;
        public Command.Settings Command;
        public ItemDrop.Settings ItemDrop;
        public Settings();
        public void Validate();
    }

    private const Key category;
    public static void Configure();
    public static void Unload();
    public class Chat
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        public static void Configure();
        public static ulong Cooldown(ulong playerid);
        public static void Subscribe();
        public static bool Trigger(ulong playerid);
        public static void Unload();
        public static void Violation(BasePlayer player, string details);
    }

    public class Command
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        public static void Configure();
        public static ulong Cooldown(ulong playerid);
        public static void Subscribe();
        public static bool Trigger(ulong playerid);
        public static void Unload();
        public static void Violation(BasePlayer player, string details);
    }

    public class ItemDrop
    {
        public class Settings
        {
            public bool Ban;
            public ulong Cooldown;
            public bool Enabled;
            public float Sensitivity;
            public bool Warn;
            public Settings();
            public Violation.Settings Validate(ulong max);
        }

        private const Key type;
        private static readonly Violation violation;
        private static readonly Dictionary<ulong, int> history;
        public static void Configure();
        public static ulong CoolDown(ulong playerid, int itemid);
        public static void Subscribe();
        public static bool Trigger(ulong playerid, int itemid);
        public static void Unload();
        public static void Violation(BasePlayer player, string details);
    }

}

public class Settings
{
    public Chat.Settings Chat;
    public Command.Settings Command;
    public ItemDrop.Settings ItemDrop;
    public Settings();
    public void Validate();
}

public class Chat
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    public static void Configure();
    public static ulong Cooldown(ulong playerid);
    public static void Subscribe();
    public static bool Trigger(ulong playerid);
    public static void Unload();
    public static void Violation(BasePlayer player, string details);
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

public class Command
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    public static void Configure();
    public static ulong Cooldown(ulong playerid);
    public static void Subscribe();
    public static bool Trigger(ulong playerid);
    public static void Unload();
    public static void Violation(BasePlayer player, string details);
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

public class ItemDrop
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public bool Enabled;
        public float Sensitivity;
        public bool Warn;
        public Settings();
        public Violation.Settings Validate(ulong max);
    }

    private const Key type;
    private static readonly Violation violation;
    private static readonly Dictionary<ulong, int> history;
    public static void Configure();
    public static ulong CoolDown(ulong playerid, int itemid);
    public static void Subscribe();
    public static bool Trigger(ulong playerid, int itemid);
    public static void Unload();
    public static void Violation(BasePlayer player, string details);
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public bool Enabled;
    public float Sensitivity;
    public bool Warn;
    public Settings();
    public Violation.Settings Validate(ulong max);
}

public class API
{
    public class Settings
    {
        public string ApiKey;
        public bool Enabled;
        public Settings();
        public void Validate();
    }

}

public class Settings
{
    public string ApiKey;
    public bool Enabled;
    public Settings();
    public void Validate();
}

public class Ban
{
    public class Settings
    {
        public bool Inherit;
        public bool Teleport;
        public BanTime Time;
        public Settings();
        public class BanTime
        {
            public bool Enforce;
            public bool Multiply;
            public ulong Seconds;
        }

        public void Validate();
    }

}

public class Settings
{
    public bool Inherit;
    public bool Teleport;
    public BanTime Time;
    public Settings();
    public class BanTime
    {
        public bool Enforce;
        public bool Multiply;
        public ulong Seconds;
    }

    public void Validate();
}

public class BanTime
{
    public bool Enforce;
    public bool Multiply;
    public ulong Seconds;
}

private class Chat
{
    public static void Admin(Key key, Dictionary<string, string> parameters);
    public static void Broadcast(Key key, Dictionary<string, string> parameters);
    public static void Console(BasePlayer player, Key key, Dictionary<string, string> parameters);
    public static void Reply(IPlayer iplayer, Key key, Dictionary<string, string> parameters);
    public static void Send(BasePlayer player, Key key, Dictionary<string, string> parameters);
}

private class Command
{
    private static readonly Dictionary<string, Info> commands;
    private static List<Info> info;
    private class Info
    {
        public Action<IPlayer, string, string[]> Action { get; set; }
        public List<string> Aliases { get; set; }
        public Key Title { get; set; }
        public Info(Action<IPlayer, string, string[]> action, Key title, string[] aliases);
    }

    public static void Load();
    public static void Receive(IPlayer iplayer, string command, string[] args);
    public static void Unload();
    private static void Config(IPlayer iplayer, string command, string[] args);
    private static string Config(string subsection);
    private static bool Config(IPlayer iplayer, string[] args, string setting, T value, Action callback);
    private static void Help(IPlayer iplayer, string command, string[] args);
    private static void Ip(IPlayer iplayer, string command, string[] args);
    private static void Ip(IPlayer iplayer, Key key, IP.NetworkInfo network, bool add);
    private static void Ip(IPlayer iplayer, Key key, List<string> list);
    private static void Log(IPlayer iplayer, string command, string[] args);
    private static void Server(IPlayer iplayer, string command, string[] args);
    private static void Teleport(IPlayer iplayer, string command, string[] args);
    private static void Users(IPlayer iplayer, string command, string[] args);
    private static void Users(IPlayer iplayer, Key key, HashSet<ulong> userids);
    private static void Vpn(IPlayer iplayer, string command, string[] args);
    private static void Vpn(IPlayer iplayer, Key key, string address);
}

private class Info
{
    public Action<IPlayer, string, string[]> Action { get; set; }
    public List<string> Aliases { get; set; }
    public Key Title { get; set; }
    public Info(Action<IPlayer, string, string[]> action, Key title, string[] aliases);
}

private class Configuration
{
    public Admin.Settings Admin;
    public AntiCheat.Settings AntiCheat;
    public AntiFlood.Settings AntiFlood;
    public Ban.Settings Ban;
    public Cripple.Settings Cripple;
    public Discord.Settings Discord;
    public Entity.Settings Entity;
    public IP.Settings IP;
    public Log.Settings Log;
    public Steam.Settings Steam;
    public User.Settings User;
    public Version.Settings Version;
    public Violation.Settings Violation;
    public VPN.Settings VPN;
    private static bool corrupt;
    private static bool dirty;
    private static bool upgraded;
    public static void Clamp(T value, T min, T max);
    public static void Load();
    public static void Save();
    public static void SetDirty();
    public static void SetUpgrade(bool upgrade);
    public static void Unload();
    public static bool Upgraded();
    public static void Validate(T value, Func<T> initializer, Action validator);
    private static void Validate();
}

private class Counter
{
    private uint count;
    private uint delay;
    public Counter();
    public void Decrement();
    public void Increment();
    public uint Percent();
    public float Ratio(bool inverse);
    public float Ratio(float scale, bool inverse);
    public ulong Total();
}

public class Cripple
{
    public class Settings
    {
        public bool Heal;
        public bool Inherit;
        public Settings();
    }

}

public class Settings
{
    public bool Heal;
    public bool Inherit;
    public Settings();
}

private class Data
{
    private static DataFileSystem data;
    private static string path;
    public static void Close();
    public static bool Exists(string name);
    public static void Open();
    public static T ReadObject(string name);
    public static void WriteObject(string name, T value);
}

private class DataFile
{
    private readonly Dictionary<TKey, TValue> data;
    private bool dirty;
    private string name;
    public DataFile(string name);
    public void Add(TKey key, TValue value);
    public bool Contains(TKey key);
    public void Clear();
    public bool Exists();
    public void ForEach(Action<TKey, TValue> action);
    public TValue Get(TKey key, TValue default_value);
    public bool IsDirty();
    public bool IsEmpty();
    public void Load();
    public void Load(string name);
    public void Remove(TKey key);
    public void Save();
    public void Save(string name);
    public void SetDirty(bool value);
    public void SetName(string name);
    public void Unload();
}

private class Discord
{
    public class Settings
    {
        public bool Enabled;
        public string WebHook;
        public DiscordFilters Filters;
        public Settings();
        public class DiscordFilter
        {
            public bool Enabled;
            public string WebHook;
            public DiscordFilter();
            public string URL();
            public void Validate();
        }

        public class DiscordFilters
        {
            public DiscordFilter AntiCheat;
            public DiscordFilter AntiFlood;
            public DiscordFilter IP;
            public DiscordFilter Steam;
            public DiscordFilter VPN;
            public DiscordFilters();
            public void Validate();
        }

        public void Validate();
    }

    public static void Send(string category, Dictionary<string, object> message);
    private static string Select(string category);
    public static void Subscribe();
}

public class Settings
{
    public bool Enabled;
    public string WebHook;
    public DiscordFilters Filters;
    public Settings();
    public class DiscordFilter
    {
        public bool Enabled;
        public string WebHook;
        public DiscordFilter();
        public string URL();
        public void Validate();
    }

    public class DiscordFilters
    {
        public DiscordFilter AntiCheat;
        public DiscordFilter AntiFlood;
        public DiscordFilter IP;
        public DiscordFilter Steam;
        public DiscordFilter VPN;
        public DiscordFilters();
        public void Validate();
    }

    public void Validate();
}

public class DiscordFilter
{
    public bool Enabled;
    public string WebHook;
    public DiscordFilter();
    public string URL();
    public void Validate();
}

public class DiscordFilters
{
    public DiscordFilter AntiCheat;
    public DiscordFilter AntiFlood;
    public DiscordFilter IP;
    public DiscordFilter Steam;
    public DiscordFilter VPN;
    public DiscordFilters();
    public void Validate();
}

private class Entity
{
    public class Settings
    {
        public EntityDamage Damage;
        public Settings();
        public class EntityDamage
        {
            public float Animal;
            public float Bradley;
            public float Building;
            public float Entity;
            public float Friend;
            public float Helicopter;
            public float NPC;
            public float Player;
            public float Team;
            public float Trap;
            public EntityDamage();
            public void Validate();
        }

        public void Validate();
    }

    public class Damage
    {
        private static readonly DamageTypeList cleared;
        public static void Cancel(HitInfo info);
        public static object Scale(HitInfo info, BasePlayer attacker, BaseCombatEntity victim);
        private static object Scale(HitInfo info, BasePlayer attacker, BasePlayer victim);
        private static object Scale(HitInfo info, float scale);
    }

    public static BasePlayer GetAttacker(BaseCombatEntity entity, HitInfo info);
    public static string GetName(BaseEntity entity);
    private static string GetName(BaseEntity entity, Type type);
    private static string GetPrefabName(BaseEntity entity);
    public static Type GetType(BaseEntity entity, string name);
    public static Type GetType(BaseEntity entity);
    public class Health
    {
        private static readonly Dictionary<int, float> health;
        public static bool Changed(BaseEntity entity);
        public static void Clear();
        public static void Clear(BaseEntity entity);
    }

    public static void Unload();
}

public class Settings
{
    public EntityDamage Damage;
    public Settings();
    public class EntityDamage
    {
        public float Animal;
        public float Bradley;
        public float Building;
        public float Entity;
        public float Friend;
        public float Helicopter;
        public float NPC;
        public float Player;
        public float Team;
        public float Trap;
        public EntityDamage();
        public void Validate();
    }

    public void Validate();
}

public class EntityDamage
{
    public float Animal;
    public float Bradley;
    public float Building;
    public float Entity;
    public float Friend;
    public float Helicopter;
    public float NPC;
    public float Player;
    public float Team;
    public float Trap;
    public EntityDamage();
    public void Validate();
}

public class Damage
{
    private static readonly DamageTypeList cleared;
    public static void Cancel(HitInfo info);
    public static object Scale(HitInfo info, BasePlayer attacker, BaseCombatEntity victim);
    private static object Scale(HitInfo info, BasePlayer attacker, BasePlayer victim);
    private static object Scale(HitInfo info, float scale);
}

public class Health
{
    private static readonly Dictionary<int, float> health;
    public static bool Changed(BaseEntity entity);
    public static void Clear();
    public static void Clear(BaseEntity entity);
}

private class Fire
{
    private static Dictionary<int, BasePlayer> fires;
    public static void Ignite(FireBall fire, BasePlayer initiator);
    public static BasePlayer Initiator(FireBall fire);
    public static void Spread(FireBall fire, FireBall spread);
    public static void Quench(FireBall fire);
    public static void Unload();
}

private class Generic
{
    public static T Clamp(T value, T min, T max);
}

private new class Hooks
{
    private static HashSet<string> subscribed;
    private static HashSet<string> unsubscribed;
    public class Base
    {
        public static void Subscribe();
    }

    public class Core
    {
        public static void Subscribe();
    }

    public class Dynamic
    {
        public static void Subscribe();
    }

    public static void Load();
    public static void Subscribe(string hook);
    public static void Unload();
    private static void Unsubscribe();
    public static void Unsubscribe(string hook);
}

public class Base
{
    public static void Subscribe();
}

public class Core
{
    public static void Subscribe();
}

public class Dynamic
{
    public static void Subscribe();
}

private class IP
{
    public class Settings
    {
        public IpFilter Filter;
        public IpViolation Violation;
        public Settings();
        public class IpFilter
        {
            public ulong Cooldown;
            public IpFilter();
            public void Validate();
        }

        public class IpViolation
        {
            public bool Ban;
            public bool Enabled;
            public IpViolation();
        }

        public void Validate();
    }

    private static readonly DataFile<string, uint> allows;
    private static readonly DataFile<string, bool> blocks;
    private static readonly DataFile<string, uint> denies;
    private static readonly HashSet<ulong> empty;
    private static readonly DataFile<string, DateTime> filter;
    private static TimeSpan filter_cooldown;
    private static readonly DataFile<string, HashSet<ulong>> users;
    private static readonly Violation violation;
    public class NetworkInfo
    {
        public string Address { get; set; }
        public uint Bits { get; set; }
        public NetworkInfo(string address, uint bits);
    }

    private static uint Bits(string address);
    public static void Block(NetworkInfo network);
    public static void Block(string address, ulong userid);
    public static void Bypass(NetworkInfo network);
    public static void Bypass(string address);
    private static uint Decimal(string address);
    public static bool Cooldown(string address);
    public static void Configure();
    public static bool Filter(string address, ulong userid);
    public static HashSet<ulong> Find(string address);
    private static List<string> Get(DataFile<string, uint> data, string address);
    public static List<string> GetAllows(string address);
    public static List<string> GetDenies(string address);
    public static bool IsAllowed(string address);
    public static bool IsBlocked(string address);
    public static bool IsDenied(string address);
    private static bool IsMatched(DataFile<string, uint> data, string address);
    public static bool IsValid(string address, bool full, bool wildcard);
    public static void Load();
    private static bool Match(uint ip, uint network, uint bits);
    private static List<string> Match(NetworkInfo network);
    public static NetworkInfo Network(string address);
    public static string Parse(string address);
    public static void Save();
    private static bool Set(DataFile<string, uint> data, NetworkInfo network, bool add);
    public static bool SetAllow(NetworkInfo network, bool add);
    public static bool SetDeny(NetworkInfo network, bool add);
    public static string ToString(uint ip);
    public static void Unblock(NetworkInfo network);
    public static void Unblock(string address);
    public static void Unload();
    public static void Update(string address, ulong userid);
    private static void Violation(string address, ulong userid);
}

public class Settings
{
    public IpFilter Filter;
    public IpViolation Violation;
    public Settings();
    public class IpFilter
    {
        public ulong Cooldown;
        public IpFilter();
        public void Validate();
    }

    public class IpViolation
    {
        public bool Ban;
        public bool Enabled;
        public IpViolation();
    }

    public void Validate();
}

public class IpFilter
{
    public ulong Cooldown;
    public IpFilter();
    public void Validate();
}

public class IpViolation
{
    public bool Ban;
    public bool Enabled;
    public IpViolation();
}

public class NetworkInfo
{
    public string Address { get; set; }
    public uint Bits { get; set; }
    public NetworkInfo(string address, uint bits);
}

private new class Log
{
    public class Settings
    {
        public LogAntiCheat AntiCheat;
        public LogAntiFlood AntiFlood;
        public LogIp IP;
        public LogProjectile Projectile;
        public LogUser User;
        public LogVpn VPN;
        public Settings();
        public class LogAntiCheat
        {
            public bool Aim;
            public bool Gravity;
            public bool MeleeRate;
            public bool Recoil;
            public bool Server;
            public bool Stash;
            public bool Trajectory;
        }

        public class LogAntiFlood
        {
            public bool ItemDrop;
        }

        public class LogIp
        {
            public bool Filter;
            public LogIp();
        }

        public class LogProjectile
        {
            public bool Collapse;
            public bool Verbose;
            public LogProjectile();
        }

        public class LogUser
        {
            public bool Bypass;
            public bool Connect;
            public LogUser();
        }

        public class LogVpn
        {
            public bool Check;
            public LogVpn();
        }

        public void Validate();
    }

    public static void Console(Key key, Dictionary<string, string> parameters);
    public static void Error(string message);
    public static void Warning(string message);
}

public class Settings
{
    public LogAntiCheat AntiCheat;
    public LogAntiFlood AntiFlood;
    public LogIp IP;
    public LogProjectile Projectile;
    public LogUser User;
    public LogVpn VPN;
    public Settings();
    public class LogAntiCheat
    {
        public bool Aim;
        public bool Gravity;
        public bool MeleeRate;
        public bool Recoil;
        public bool Server;
        public bool Stash;
        public bool Trajectory;
    }

    public class LogAntiFlood
    {
        public bool ItemDrop;
    }

    public class LogIp
    {
        public bool Filter;
        public LogIp();
    }

    public class LogProjectile
    {
        public bool Collapse;
        public bool Verbose;
        public LogProjectile();
    }

    public class LogUser
    {
        public bool Bypass;
        public bool Connect;
        public LogUser();
    }

    public class LogVpn
    {
        public bool Check;
        public LogVpn();
    }

    public void Validate();
}

public class LogAntiCheat
{
    public bool Aim;
    public bool Gravity;
    public bool MeleeRate;
    public bool Recoil;
    public bool Server;
    public bool Stash;
    public bool Trajectory;
}

public class LogAntiFlood
{
    public bool ItemDrop;
}

public class LogIp
{
    public bool Filter;
    public LogIp();
}

public class LogProjectile
{
    public bool Collapse;
    public bool Verbose;
    public LogProjectile();
}

public class LogUser
{
    public bool Bypass;
    public bool Connect;
    public LogUser();
}

public class LogVpn
{
    public bool Check;
    public LogVpn();
}

private class Map
{
    public class Building
    {
        public static bool HasPrivilege(DecayEntity entity);
        public static bool InFoundation(Vector3 position);
        public static bool IsNearby(Vector3 position, bool check_privilege);
    }

    public class Cave
    {
        public static bool IsInside(Vector3 position);
    }

    public class Collider
    {
        public static string Info(Vector3 position);
    }

    public class Entities
    {
        public static bool InRange(Vector3 position, float radius);
    }

    public static string Grid(Vector3 position);
    private class Hit
    {
        private static string Collider(RaycastHit hit);
        public static bool IsBunker(RaycastHit hit);
        public static bool IsBunker(string collider);
        public static bool IsCave(RaycastHit hit);
        public static bool IsCave(string collider);
        public static bool IsCorridor(RaycastHit hit);
        public static bool IsCorridor(string collider);
        public static bool IsDuct(RaycastHit hit);
        public static bool IsDuct(string collider);
        public static bool IsFoundation(RaycastHit hit);
        public static bool IsFoundation(string collider);
        public static bool IsMine(RaycastHit hit);
        public static bool IsMine(string collider);
        public static bool IsRoad(RaycastHit hit);
        public static bool IsRoad(string collider);
        public static bool IsRock(RaycastHit hit);
        public static bool IsRock(string collider);
        public static bool IsStairwell(RaycastHit hit);
        public static bool IsStairwell(string collider);
        public static bool IsTunnel(RaycastHit hit);
        public static bool IsTunnel(string collider);
        public static bool IsUnderground(RaycastHit hit);
        public static bool IsUnderground(string collider);
    }

    public static bool InRange(Vector3 a, Vector3 b, float distance);
    public static bool InRange2D(Vector3 a, Vector3 b, float distance);
    public static void Load();
    public class Monument
    {
        private static SparseMap global;
        private static SparseMap tunnel;
        public static bool IsNearby(Vector3 position);
        public static bool IsTunnel(Vector3 position);
        public static void Load();
        private static float Range(MonumentInfo monument);
        public static void Unload();
    }

    public class Position
    {
        private static bool HasCheck(Check checks, Check check);
        public static Vector3 Random(Check checks);
        private static Vector3 Random(float min_x, float max_x, float min_z, float max_z);
    }

    public class Road
    {
        public static bool IsNearby(Vector3 position);
    }

    public class Rock
    {
        public static bool IsInside(Vector3 position, bool check_cave);
    }

    public static Vector3 Surface(Vector3 position, float offset);
    public class Terrain
    {
        public static float Height(Vector3 position);
        public static bool IsInside(Vector3 position, bool check_cave);
        public static bool IsSurface(Vector3 position);
        public static Vector3 Level(Vector3 position, float offset);
    }

    public class Underground
    {
        public static bool IsInside(Vector3 position);
    }

    public static void Unload();
    public class Water
    {
        public static float Height(Vector3 position);
        public static bool IsSurface(Vector3 position);
        public static Vector3 Level(Vector3 position, float offset);
    }

}

public class Building
{
    public static bool HasPrivilege(DecayEntity entity);
    public static bool InFoundation(Vector3 position);
    public static bool IsNearby(Vector3 position, bool check_privilege);
}

public class Cave
{
    public static bool IsInside(Vector3 position);
}

public class Collider
{
    public static string Info(Vector3 position);
}

public class Entities
{
    public static bool InRange(Vector3 position, float radius);
}

private class Hit
{
    private static string Collider(RaycastHit hit);
    public static bool IsBunker(RaycastHit hit);
    public static bool IsBunker(string collider);
    public static bool IsCave(RaycastHit hit);
    public static bool IsCave(string collider);
    public static bool IsCorridor(RaycastHit hit);
    public static bool IsCorridor(string collider);
    public static bool IsDuct(RaycastHit hit);
    public static bool IsDuct(string collider);
    public static bool IsFoundation(RaycastHit hit);
    public static bool IsFoundation(string collider);
    public static bool IsMine(RaycastHit hit);
    public static bool IsMine(string collider);
    public static bool IsRoad(RaycastHit hit);
    public static bool IsRoad(string collider);
    public static bool IsRock(RaycastHit hit);
    public static bool IsRock(string collider);
    public static bool IsStairwell(RaycastHit hit);
    public static bool IsStairwell(string collider);
    public static bool IsTunnel(RaycastHit hit);
    public static bool IsTunnel(string collider);
    public static bool IsUnderground(RaycastHit hit);
    public static bool IsUnderground(string collider);
}

public class Monument
{
    private static SparseMap global;
    private static SparseMap tunnel;
    public static bool IsNearby(Vector3 position);
    public static bool IsTunnel(Vector3 position);
    public static void Load();
    private static float Range(MonumentInfo monument);
    public static void Unload();
}

public class Position
{
    private static bool HasCheck(Check checks, Check check);
    public static Vector3 Random(Check checks);
    private static Vector3 Random(float min_x, float max_x, float min_z, float max_z);
}

public class Road
{
    public static bool IsNearby(Vector3 position);
}

public class Rock
{
    public static bool IsInside(Vector3 position, bool check_cave);
}

public class Terrain
{
    public static float Height(Vector3 position);
    public static bool IsInside(Vector3 position, bool check_cave);
    public static bool IsSurface(Vector3 position);
    public static Vector3 Level(Vector3 position, float offset);
}

public class Underground
{
    public static bool IsInside(Vector3 position);
}

public class Water
{
    public static float Height(Vector3 position);
    public static bool IsSurface(Vector3 position);
    public static Vector3 Level(Vector3 position, float offset);
}

private class Permissions
{
    private static string PERMISSION_ADMIN;
    private static string PERMISSION_ALL;
    private static string PERMISSION_IGNORE;
    private static string PERMISSION_PREFIX;
    private static readonly HashSet<ulong> all;
    private static readonly HashSet<ulong> admin;
    private static readonly HashSet<ulong> ignore;
    public static bool Admin(ulong userid, bool forced);
    public static bool All(ulong userid, bool forced);
    public class Bypass
    {
        private static string PERMISSION_PREFIX;
        private static string PERMISSION_STEAM;
        private static string PERMISSION_VPN;
        private static readonly HashSet<ulong> steam;
        private static readonly HashSet<ulong> vpn;
        public class AntiCheat
        {
            private static readonly HashSet<ulong> aim;
            private static readonly HashSet<ulong> firerate;
            private static readonly HashSet<ulong> gravity;
            private static readonly HashSet<ulong> meleerate;
            private static readonly HashSet<ulong> recoil;
            private static readonly HashSet<ulong> server;
            private static readonly HashSet<ulong> stash;
            private static readonly HashSet<ulong> trajectory;
            private static readonly HashSet<ulong> wallhack;
            private static string PERMISSION_AIM;
            private static string PERMISSION_FIRERATE;
            private static string PERMISSION_GRAVITY;
            private static string PERMISSION_MELEERATE;
            private static string PERMISSION_PREFIX;
            private static string PERMISSION_RECOIL;
            private static string PERMISSION_SERVER;
            private static string PERMISSION_STASH;
            private static string PERMISSION_TRAJECTORY;
            private static string PERMISSION_WALLHACK;
            public static void Load();
            public static bool Aim(ulong userid, bool forced);
            public static bool FireRate(ulong userid, bool forced);
            public static bool Gravity(ulong userid, bool forced);
            public static bool MeleeRate(ulong userid, bool forced);
            public static bool Recoil(ulong userid, bool forced);
            public static void Reset(ulong userid);
            public static bool Server(ulong userid, bool forced);
            public static bool Stash(ulong userid, bool forced);
            public static bool Trajectory(ulong userid, bool forced);
            public static void Unload();
            public static void Update(ulong userid);
            public static bool WallHack(ulong userid, bool forced);
        }

        public static void Load();
        public static void Reset(ulong userid);
        public static bool Steam(ulong userid, bool forced);
        public static void Unload();
        public static void Update(ulong userid);
        public static bool Vpn(ulong userid, bool forced);
    }

    public class Command
    {
        private static readonly HashSet<ulong> config;
        private static readonly HashSet<ulong> ip;
        private static readonly HashSet<ulong> server;
        private static readonly HashSet<ulong> tp;
        private static readonly HashSet<ulong> vpn;
        private static string PERMISSION_CONFIG;
        private static string PERMISSION_IP;
        private static string PERMISSION_PREFIX;
        private static string PERMISSION_SERVER;
        private static string PERMISSION_TP;
        private static string PERMISSION_VPN;
        public static void Load();
        public static bool Config(ulong userid, bool forced);
        public static bool Ip(ulong userid, bool forced);
        public static void Reset(ulong userid);
        public static bool Server(ulong userid, bool forced);
        public static bool Tp(ulong userid, bool forced);
        public static void Unload();
        public static void Update(ulong userid);
        public static bool Vpn(ulong userid, bool forced);
    }

    public static string[] Groups(ulong userid);
    private static bool HasPermission(ulong userid, string permission);
    public static bool HasPrefix(string permission);
    public static bool Ignore(ulong userid, bool forced);
    public static void Load();
    public static void Reset(ulong userid);
    public static void Unload();
    public static void Update(ulong userid);
    private static bool Update(ulong userid, string permission, HashSet<ulong> cache);
}

public class Bypass
{
    private static string PERMISSION_PREFIX;
    private static string PERMISSION_STEAM;
    private static string PERMISSION_VPN;
    private static readonly HashSet<ulong> steam;
    private static readonly HashSet<ulong> vpn;
    public class AntiCheat
    {
        private static readonly HashSet<ulong> aim;
        private static readonly HashSet<ulong> firerate;
        private static readonly HashSet<ulong> gravity;
        private static readonly HashSet<ulong> meleerate;
        private static readonly HashSet<ulong> recoil;
        private static readonly HashSet<ulong> server;
        private static readonly HashSet<ulong> stash;
        private static readonly HashSet<ulong> trajectory;
        private static readonly HashSet<ulong> wallhack;
        private static string PERMISSION_AIM;
        private static string PERMISSION_FIRERATE;
        private static string PERMISSION_GRAVITY;
        private static string PERMISSION_MELEERATE;
        private static string PERMISSION_PREFIX;
        private static string PERMISSION_RECOIL;
        private static string PERMISSION_SERVER;
        private static string PERMISSION_STASH;
        private static string PERMISSION_TRAJECTORY;
        private static string PERMISSION_WALLHACK;
        public static void Load();
        public static bool Aim(ulong userid, bool forced);
        public static bool FireRate(ulong userid, bool forced);
        public static bool Gravity(ulong userid, bool forced);
        public static bool MeleeRate(ulong userid, bool forced);
        public static bool Recoil(ulong userid, bool forced);
        public static void Reset(ulong userid);
        public static bool Server(ulong userid, bool forced);
        public static bool Stash(ulong userid, bool forced);
        public static bool Trajectory(ulong userid, bool forced);
        public static void Unload();
        public static void Update(ulong userid);
        public static bool WallHack(ulong userid, bool forced);
    }

    public static void Load();
    public static void Reset(ulong userid);
    public static bool Steam(ulong userid, bool forced);
    public static void Unload();
    public static void Update(ulong userid);
    public static bool Vpn(ulong userid, bool forced);
}

public class AntiCheat
{
    private static readonly HashSet<ulong> aim;
    private static readonly HashSet<ulong> firerate;
    private static readonly HashSet<ulong> gravity;
    private static readonly HashSet<ulong> meleerate;
    private static readonly HashSet<ulong> recoil;
    private static readonly HashSet<ulong> server;
    private static readonly HashSet<ulong> stash;
    private static readonly HashSet<ulong> trajectory;
    private static readonly HashSet<ulong> wallhack;
    private static string PERMISSION_AIM;
    private static string PERMISSION_FIRERATE;
    private static string PERMISSION_GRAVITY;
    private static string PERMISSION_MELEERATE;
    private static string PERMISSION_PREFIX;
    private static string PERMISSION_RECOIL;
    private static string PERMISSION_SERVER;
    private static string PERMISSION_STASH;
    private static string PERMISSION_TRAJECTORY;
    private static string PERMISSION_WALLHACK;
    public static void Load();
    public static bool Aim(ulong userid, bool forced);
    public static bool FireRate(ulong userid, bool forced);
    public static bool Gravity(ulong userid, bool forced);
    public static bool MeleeRate(ulong userid, bool forced);
    public static bool Recoil(ulong userid, bool forced);
    public static void Reset(ulong userid);
    public static bool Server(ulong userid, bool forced);
    public static bool Stash(ulong userid, bool forced);
    public static bool Trajectory(ulong userid, bool forced);
    public static void Unload();
    public static void Update(ulong userid);
    public static bool WallHack(ulong userid, bool forced);
}

public class Command
{
    private static readonly HashSet<ulong> config;
    private static readonly HashSet<ulong> ip;
    private static readonly HashSet<ulong> server;
    private static readonly HashSet<ulong> tp;
    private static readonly HashSet<ulong> vpn;
    private static string PERMISSION_CONFIG;
    private static string PERMISSION_IP;
    private static string PERMISSION_PREFIX;
    private static string PERMISSION_SERVER;
    private static string PERMISSION_TP;
    private static string PERMISSION_VPN;
    public static void Load();
    public static bool Config(ulong userid, bool forced);
    public static bool Ip(ulong userid, bool forced);
    public static void Reset(ulong userid);
    public static bool Server(ulong userid, bool forced);
    public static bool Tp(ulong userid, bool forced);
    public static void Unload();
    public static void Update(ulong userid);
    public static bool Vpn(ulong userid, bool forced);
}

private class Projectile
{
    public class Log
    {
        private static readonly Queue<Entry> expired;
        private static readonly Dictionary<ulong, Queue<Entry>> history;
        private static readonly Dictionary<ulong, Dictionary<int, Entry>> pending;
        private static readonly Queue<Request> request;
        private static readonly Queue<Entry> reserve;
        static readonly StringBuilder buffer;
        private class Entry
        {
            public float aim_angle;
            public float aim_pvp;
            public float aim_range;
            public ulong aim_violations;
            public string attacker;
            public ulong attacker_id;
            public ulong firerate_violations;
            public float hit_distance;
            public HitArea hit_location;
            public bool pvp;
            public float recoil_pitch;
            public ulong recoil_repeats;
            public float recoil_swing;
            public ulong recoil_violations;
            public float recoil_yaw;
            public bool ricochet;
            public float speed;
            public DateTime timestamp;
            public float trajectory;
            public ulong trajectory_violations;
            public string victim;
            public bool violations;
            public ulong wallhack_violations;
            public string weapon;
            private static readonly Queue<Entry> pool;
            private Entry(DateTime timestamp);
            private Entry Default();
            public string Format(bool collapse, bool id, bool time);
            public static Entry Get(DateTime timestamp);
            public void Release();
            private Entry Set(DateTime timestamp);
            public static void Unload();
        }

        private class Request
        {
            public IPlayer actor;
            public int lines;
            public ulong userid;
        }

        public static void Add(Weapon weapon);
        public static void Expire(Weapon weapon);
        public static void Get(IPlayer actor, ulong userid, int lines);
        public static float GetAimAngle(ulong userid, int id);
        public static bool GetAimPvp(ulong userid, int id);
        public static float GetHitDistance(ulong userid, int id);
        public static HitArea GetHitLocation(ulong userid, int id);
        public static bool GetRicochet(ulong userid, int id);
        public static string GetVictim(ulong userid, int id);
        public static void Send();
        public static void SetAim(ulong userid, int id, float aim_angle, float aim_pvp, float aim_range, bool pvp, bool ricochet);
        public static void SetAimViolations(ulong userid, int id, ulong aim_violations, bool add);
        public static void SetAttacker(ulong userid, int id, string attacker, ulong attaker_id);
        public static void SetFireRateViolations(ulong userid, int id, ulong firerate_violations);
        public static void SetHit(ulong userid, int id, float hit_distance, HitArea hit_location);
        public static void SetRecoil(ulong userid, int id, float recoil_pitch, ulong recoil_repeats, float recoil_yaw, float recoil_swing);
        public static void SetRecoilViolations(ulong userid, int id, ulong recoil_violations);
        public static void SetTrajectory(ulong userid, int id, float trajectory);
        public static void SetTrajectoryViolations(ulong userid, int id, ulong trajectory_violations);
        public static void SetVictim(ulong userid, int id, string victim);
        public static void SetWallHackViolations(ulong userid, int id, ulong wallhack_violations);
        public static void SetWeapon(ulong userid, int id, float speed, string weapon);
        private static void TrySetEntry(ulong userid, int id, Action<Entry> set);
        public static void Unload();
    }

    private static readonly List<Weapon> expired;
    private static readonly Dictionary<ulong, Dictionary<int, Weapon>> reverse;
    private static readonly Dictionary<ulong, Queue<Weapon>> weapons;
    public static void Add(Weapon weapon);
    public static void Load();
    public static void Unload();
    public static Weapon Weapon(ulong userid, int projectileid);
}

public class Log
{
    private static readonly Queue<Entry> expired;
    private static readonly Dictionary<ulong, Queue<Entry>> history;
    private static readonly Dictionary<ulong, Dictionary<int, Entry>> pending;
    private static readonly Queue<Request> request;
    private static readonly Queue<Entry> reserve;
    static readonly StringBuilder buffer;
    private class Entry
    {
        public float aim_angle;
        public float aim_pvp;
        public float aim_range;
        public ulong aim_violations;
        public string attacker;
        public ulong attacker_id;
        public ulong firerate_violations;
        public float hit_distance;
        public HitArea hit_location;
        public bool pvp;
        public float recoil_pitch;
        public ulong recoil_repeats;
        public float recoil_swing;
        public ulong recoil_violations;
        public float recoil_yaw;
        public bool ricochet;
        public float speed;
        public DateTime timestamp;
        public float trajectory;
        public ulong trajectory_violations;
        public string victim;
        public bool violations;
        public ulong wallhack_violations;
        public string weapon;
        private static readonly Queue<Entry> pool;
        private Entry(DateTime timestamp);
        private Entry Default();
        public string Format(bool collapse, bool id, bool time);
        public static Entry Get(DateTime timestamp);
        public void Release();
        private Entry Set(DateTime timestamp);
        public static void Unload();
    }

    private class Request
    {
        public IPlayer actor;
        public int lines;
        public ulong userid;
    }

    public static void Add(Weapon weapon);
    public static void Expire(Weapon weapon);
    public static void Get(IPlayer actor, ulong userid, int lines);
    public static float GetAimAngle(ulong userid, int id);
    public static bool GetAimPvp(ulong userid, int id);
    public static float GetHitDistance(ulong userid, int id);
    public static HitArea GetHitLocation(ulong userid, int id);
    public static bool GetRicochet(ulong userid, int id);
    public static string GetVictim(ulong userid, int id);
    public static void Send();
    public static void SetAim(ulong userid, int id, float aim_angle, float aim_pvp, float aim_range, bool pvp, bool ricochet);
    public static void SetAimViolations(ulong userid, int id, ulong aim_violations, bool add);
    public static void SetAttacker(ulong userid, int id, string attacker, ulong attaker_id);
    public static void SetFireRateViolations(ulong userid, int id, ulong firerate_violations);
    public static void SetHit(ulong userid, int id, float hit_distance, HitArea hit_location);
    public static void SetRecoil(ulong userid, int id, float recoil_pitch, ulong recoil_repeats, float recoil_yaw, float recoil_swing);
    public static void SetRecoilViolations(ulong userid, int id, ulong recoil_violations);
    public static void SetTrajectory(ulong userid, int id, float trajectory);
    public static void SetTrajectoryViolations(ulong userid, int id, ulong trajectory_violations);
    public static void SetVictim(ulong userid, int id, string victim);
    public static void SetWallHackViolations(ulong userid, int id, ulong wallhack_violations);
    public static void SetWeapon(ulong userid, int id, float speed, string weapon);
    private static void TrySetEntry(ulong userid, int id, Action<Entry> set);
    public static void Unload();
}

private class Entry
{
    public float aim_angle;
    public float aim_pvp;
    public float aim_range;
    public ulong aim_violations;
    public string attacker;
    public ulong attacker_id;
    public ulong firerate_violations;
    public float hit_distance;
    public HitArea hit_location;
    public bool pvp;
    public float recoil_pitch;
    public ulong recoil_repeats;
    public float recoil_swing;
    public ulong recoil_violations;
    public float recoil_yaw;
    public bool ricochet;
    public float speed;
    public DateTime timestamp;
    public float trajectory;
    public ulong trajectory_violations;
    public string victim;
    public bool violations;
    public ulong wallhack_violations;
    public string weapon;
    private static readonly Queue<Entry> pool;
    private Entry(DateTime timestamp);
    private Entry Default();
    public string Format(bool collapse, bool id, bool time);
    public static Entry Get(DateTime timestamp);
    public void Release();
    private Entry Set(DateTime timestamp);
    public static void Unload();
}

private class Request
{
    public IPlayer actor;
    public int lines;
    public ulong userid;
}

private class SparseMap
{
    private Dictionary<Texel, Cell> map;
    private float map_scale;
    public SparseMap(float scale);
    public void Clear();
    public int Count();
    public void Reset(float scale);
    private void Reset(Pixel pixel);
    public void Reset(Vector3 position);
    public float Scale();
    private int Scale(float n);
    private void Set(Pixel pixel);
    public void Set(Vector3 position);
    public int Set(Vector3 origin, float range);
    private void SetScale(float scale);
    private bool Test(Pixel pixel);
    public bool Test(Vector3 position);
}

private class Steam
{
    public class Settings
    {
        public API.Settings API;
        public SteamBan Ban;
        public SteamGame Game;
        public SteamProfile Profile;
        public SteamShare Share;
        public SteamViolation Violation;
        public Settings();
        public class SteamBan
        {
            public bool Active;
            public bool Community;
            public ulong Days;
            public bool Economy;
            public ulong Game;
            public ulong VAC;
        }

        public class SteamGame
        {
            public ulong Count;
            public ulong Hours;
        }

        public class SteamProfile
        {
            public bool Invalid;
            public bool Limited;
            public bool Private;
        }

        public class SteamShare
        {
            public bool Family;
        }

        public class SteamViolation
        {
            public bool Ban;
            public bool Enabled;
            public bool Warn;
            public SteamViolation();
        }

        public void Validate();
    }

    private static uint appid;
    private static ActionQueue checks;
    private static readonly Violation violation;
    public static void Check(ulong userid);
    public static void Configure();
    public static void Load();
    public static void Unload();
    private static void Violation(ulong userid, Key type, string details, bool ban);
    private class Ban
    {
        [JsonProperty("players")]
        public Player[] Players;
        public class Player
        {
            [JsonProperty("CommunityBanned")]
            public bool CommunityBanned;
            [JsonProperty("VACBanned")]
            public bool VacBanned;
            [JsonProperty("NumberOfVACBans")]
            public ulong NumberOfVacBans;
            [JsonProperty("DaysSinceLastBan")]
            public ulong DaysSinceLastBan;
            [JsonProperty("NumberOfGameBans")]
            public ulong NumberOfGameBans;
            [JsonProperty("EconomyBan")]
            public string EconomyBan;
        }

        [JsonIgnore]
        private static readonly string api;
        [JsonIgnore]
        public static bool check;
        public static void Check(ulong userid);
    }

    private class Game
    {
        [JsonProperty("response")]
        public Content Response;
        public class Content
        {
            [JsonProperty("game_count")]
            public ulong GameCount;
            [JsonProperty("games")]
            public Game[] Games;
            public class Game
            {
                [JsonProperty("appid")]
                public uint AppId;
                [JsonProperty("playtime_forever")]
                public ulong PlaytimeForever;
            }

        }

        [JsonIgnore]
        private static readonly string api;
        [JsonIgnore]
        public static bool check;
        public static void Check(ulong userid, bool private_profile, bool is_sharing);
    }

    private class Profile
    {
        private static readonly string api;
        public static bool check;
        public static void Check(ulong userid, bool private_profile);
    }

    private class Share
    {
        [JsonProperty("response")]
        public Content Response;
        public class Content
        {
            [JsonProperty("lender_steamid")]
            public ulong LenderSteamId;
        }

        [JsonIgnore]
        private static readonly string api;
        [JsonIgnore]
        public static bool check;
        public static void Check(ulong userid, bool private_profile);
    }

    private class Summaries
    {
        [JsonProperty("response")]
        public Content Response;
        public class Content
        {
            [JsonProperty("players")]
            public Player[] Players;
            public class Player
            {
                [JsonProperty("communityvisibilitystate")]
                public int CommunityVisibilityState;
            }

        }

        [JsonIgnore]
        private static readonly string api;
        [JsonIgnore]
        public static bool check;
        public static void Check(ulong userid);
    }

}

public class Settings
{
    public API.Settings API;
    public SteamBan Ban;
    public SteamGame Game;
    public SteamProfile Profile;
    public SteamShare Share;
    public SteamViolation Violation;
    public Settings();
    public class SteamBan
    {
        public bool Active;
        public bool Community;
        public ulong Days;
        public bool Economy;
        public ulong Game;
        public ulong VAC;
    }

    public class SteamGame
    {
        public ulong Count;
        public ulong Hours;
    }

    public class SteamProfile
    {
        public bool Invalid;
        public bool Limited;
        public bool Private;
    }

    public class SteamShare
    {
        public bool Family;
    }

    public class SteamViolation
    {
        public bool Ban;
        public bool Enabled;
        public bool Warn;
        public SteamViolation();
    }

    public void Validate();
}

public class SteamBan
{
    public bool Active;
    public bool Community;
    public ulong Days;
    public bool Economy;
    public ulong Game;
    public ulong VAC;
}

public class SteamGame
{
    public ulong Count;
    public ulong Hours;
}

public class SteamProfile
{
    public bool Invalid;
    public bool Limited;
    public bool Private;
}

public class SteamShare
{
    public bool Family;
}

public class SteamViolation
{
    public bool Ban;
    public bool Enabled;
    public bool Warn;
    public SteamViolation();
}

private class Ban
{
    [JsonProperty("players")]
    public Player[] Players;
    public class Player
    {
        [JsonProperty("CommunityBanned")]
        public bool CommunityBanned;
        [JsonProperty("VACBanned")]
        public bool VacBanned;
        [JsonProperty("NumberOfVACBans")]
        public ulong NumberOfVacBans;
        [JsonProperty("DaysSinceLastBan")]
        public ulong DaysSinceLastBan;
        [JsonProperty("NumberOfGameBans")]
        public ulong NumberOfGameBans;
        [JsonProperty("EconomyBan")]
        public string EconomyBan;
    }

    [JsonIgnore]
    private static readonly string api;
    [JsonIgnore]
    public static bool check;
    public static void Check(ulong userid);
}

public class Player
{
    [JsonProperty("CommunityBanned")]
    public bool CommunityBanned;
    [JsonProperty("VACBanned")]
    public bool VacBanned;
    [JsonProperty("NumberOfVACBans")]
    public ulong NumberOfVacBans;
    [JsonProperty("DaysSinceLastBan")]
    public ulong DaysSinceLastBan;
    [JsonProperty("NumberOfGameBans")]
    public ulong NumberOfGameBans;
    [JsonProperty("EconomyBan")]
    public string EconomyBan;
}

private class Game
{
    [JsonProperty("response")]
    public Content Response;
    public class Content
    {
        [JsonProperty("game_count")]
        public ulong GameCount;
        [JsonProperty("games")]
        public Game[] Games;
        public class Game
        {
            [JsonProperty("appid")]
            public uint AppId;
            [JsonProperty("playtime_forever")]
            public ulong PlaytimeForever;
        }

    }

    [JsonIgnore]
    private static readonly string api;
    [JsonIgnore]
    public static bool check;
    public static void Check(ulong userid, bool private_profile, bool is_sharing);
}

public class Content
{
    [JsonProperty("game_count")]
    public ulong GameCount;
    [JsonProperty("games")]
    public Game[] Games;
    public class Game
    {
        [JsonProperty("appid")]
        public uint AppId;
        [JsonProperty("playtime_forever")]
        public ulong PlaytimeForever;
    }

}

public class Game
{
    [JsonProperty("appid")]
    public uint AppId;
    [JsonProperty("playtime_forever")]
    public ulong PlaytimeForever;
}

private class Profile
{
    private static readonly string api;
    public static bool check;
    public static void Check(ulong userid, bool private_profile);
}

private class Share
{
    [JsonProperty("response")]
    public Content Response;
    public class Content
    {
        [JsonProperty("lender_steamid")]
        public ulong LenderSteamId;
    }

    [JsonIgnore]
    private static readonly string api;
    [JsonIgnore]
    public static bool check;
    public static void Check(ulong userid, bool private_profile);
}

public class Content
{
    [JsonProperty("lender_steamid")]
    public ulong LenderSteamId;
}

private class Summaries
{
    [JsonProperty("response")]
    public Content Response;
    public class Content
    {
        [JsonProperty("players")]
        public Player[] Players;
        public class Player
        {
            [JsonProperty("communityvisibilitystate")]
            public int CommunityVisibilityState;
        }

    }

    [JsonIgnore]
    private static readonly string api;
    [JsonIgnore]
    public static bool check;
    public static void Check(ulong userid);
}

public class Content
{
    [JsonProperty("players")]
    public Player[] Players;
    public class Player
    {
        [JsonProperty("communityvisibilitystate")]
        public int CommunityVisibilityState;
    }

}

public class Player
{
    [JsonProperty("communityvisibilitystate")]
    public int CommunityVisibilityState;
}

private class Text
{
    private static readonly Dictionary<string, Dictionary<Key, string>> decorated;
    private static readonly Dictionary<string, Dictionary<Key, string>> unadorned;
    private static string server_language;
    private class RegEx
    {
        public static Regex clean1;
        public static Regex clean2;
        public static Regex clean3;
        public static Regex markup;
        public static Regex spaces;
        public static void Load();
        public static void Unload();
    }

    public static string Actor(IPlayer actor);
    public static string Actor(IPlayer actor, BasePlayer player);
    public static string Actor(IPlayer actor, IPlayer iplayer);
    public static string Actor(IPlayer actor, ulong userid);
    public static string Actor(IPlayer actor, string language);
    public static string BodyPart(HitArea area);
    public static string BodyPart(HitArea area, BasePlayer player);
    public static string BodyPart(HitArea area, IPlayer iplayer);
    public static string BodyPart(HitArea area, ulong userid);
    public static string BodyPart(HitArea area, string language);
    public class Duration
    {
        public static string Hours(TimeSpan time);
        public static string Hours(TimeSpan time, BasePlayer player);
        public static string Hours(TimeSpan time, IPlayer iplayer);
        public static string Hours(TimeSpan time, ulong userid);
        public static string Hours(TimeSpan time, string language);
        public static string Short(TimeSpan time);
        public static string Short(TimeSpan time, BasePlayer player);
        public static string Short(TimeSpan time, IPlayer iplayer);
        public static string Short(TimeSpan time, ulong userid);
        public static string Short(TimeSpan time, string language);
    }

    public static string Get(Key key, Dictionary<string, string> parameters);
    public static string Get(Key key, BasePlayer player, Dictionary<string, string> parameters);
    public static string Get(Key key, IPlayer iplayer, Dictionary<string, string> parameters);
    public static string Get(Key key, ulong userid, Dictionary<string, string> parameters);
    public static string Get(Key key, string language, Dictionary<string, string> parameters);
    private static string Get(Key key, Dictionary<Key, string> cache, Dictionary<string, string> parameters);
    public static string GetPlain(Key key, Dictionary<string, string> parameters);
    public static string GetPlain(Key key, BasePlayer player, Dictionary<string, string> parameters);
    public static string GetPlain(Key key, IPlayer iplayer, Dictionary<string, string> parameters);
    public static string GetPlain(Key key, ulong userid, Dictionary<string, string> parameters);
    public static string GetPlain(Key key, string language, Dictionary<string, string> parameters);
    public static string Language(string userid);
    public static void Load();
    private static Dictionary<Key, string> Messages(Dictionary<string, Dictionary<Key, string>> cache, string language);
    public static bool ParseTime(string message, ulong seconds);
    private static void RegisterMessages();
    private static string Replace(string message, Dictionary<string, string> parameters);
    private static string Strip(string message);
    public static string Sanitize(string message);
    private static string Trim(string message);
    public static void Unload();
}

private class RegEx
{
    public static Regex clean1;
    public static Regex clean2;
    public static Regex clean3;
    public static Regex markup;
    public static Regex spaces;
    public static void Load();
    public static void Unload();
}

public class Duration
{
    public static string Hours(TimeSpan time);
    public static string Hours(TimeSpan time, BasePlayer player);
    public static string Hours(TimeSpan time, IPlayer iplayer);
    public static string Hours(TimeSpan time, ulong userid);
    public static string Hours(TimeSpan time, string language);
    public static string Short(TimeSpan time);
    public static string Short(TimeSpan time, BasePlayer player);
    public static string Short(TimeSpan time, IPlayer iplayer);
    public static string Short(TimeSpan time, ulong userid);
    public static string Short(TimeSpan time, string language);
}

private class Timers
{
    private static readonly List<Timer> timers;
    public static void Add(float interval, Action callback);
    public static void Destroy();
}

private class User
{
    public class Settings
    {
        public UserBypass Bypass;
        public UserFriend Friend;
        public UserTeam Team;
        public Settings();
        public class UserBypass
        {
            public ulong DaysSinceBan;
            public bool Enabled;
            public ulong HoursPlayed;
            public bool Multiply;
            public UserBypass();
            public void Validate();
        }

        public class UserFriend
        {
            public bool Damage;
        }

        public class UserTeam
        {
            public bool Damage;
        }

        public void Validate();
    }

    private class Info
    {
        public ulong userid;
        public string name;
        public HashSet<string> names;
        public string address;
        public HashSet<string> addresses;
        public DateTime time_connected;
        public DateTime time_disconnected;
        public TimeSpan time_played;
        public ulong ban_count;
        public string ban_reason;
        public DateTime ban_time;
        public DateTime ban_timer;
        public bool is_banned;
        public ulong cripple_count;
        public string cripple_reason;
        public DateTime cripple_time;
        public DateTime cripple_timer;
        public bool is_crippled;
        public Vector3 l_position;
        public Vector3 v_position;
        [JsonIgnore]
        public DateTime access_time;
        [JsonIgnore]
        public ulong attacked;
        [JsonIgnore]
        public ulong attacker;
        [JsonIgnore]
        public bool dirty;
        [JsonIgnore]
        public BasePlayer player;
        [JsonIgnore]
        public Stack<ulong> teleport;
        [JsonIgnore]
        public List<ulong> victims;
        public Info();
    }

    private static readonly DataFile<string, HashSet<ulong>> names;
    private static ActionQueue teleport;
    private static readonly Dictionary<ulong, Info> users;
    private static void Action(IPlayer actor, Key action, Info user);
    public static string Address(ulong userid);
    public static void AssignAttacker(BasePlayer attacker, BasePlayer victim);
    public static void AssignVictim(BasePlayer victim);
    private static void AssignVictims(Info user);
    public static void Ban(ulong userid, string reason, ulong seconds, IPlayer actor);
    public static ulong BanCount(ulong userid);
    private static void BanInherit(ulong userid, string name, string address, Info copy);
    public static void BanReset(ulong userid, IPlayer actor);
    private static void BanTeleport(Info user, ulong count);
    public static DateTime BanTime(ulong userid);
    private static void BanTimeEnforce(Info user);
    private static bool CanBypass(Info user);
    public static object CanConnect(string name, string id, string address);
    public static bool CanFly(BasePlayer player);
    private static bool CanLoot(ulong userid, ulong targetid);
    public static object CanLoot(BasePlayer player, ulong targetid);
    public static void Cripple(ulong userid, string reason, ulong seconds, IPlayer actor);
    private static void CrippleInherit(ulong userid, string name, string address, Info copy);
    public static void CrippleReset(ulong userid, IPlayer actor);
    public static bool Exists(ulong userid);
    public static HashSet<ulong> Find(string text);
    public static Vector3 GetLastSeenPosition(ulong userid);
    public static Vector3 GetViolationPosition(ulong userid);
    public static bool HasAuthLevel(BasePlayer player);
    public static bool HasAdminFlag(BasePlayer player);
    private static bool HasConnected(Info user);
    public static bool HasDeveloperFlag(BasePlayer player);
    public static bool HasParent(BasePlayer player);
    public static string InfoText(ulong userid);
    public static string InfoText(ulong userid, BasePlayer player);
    public static string InfoText(ulong userid, IPlayer iplayer);
    public static string InfoText(ulong userid, ulong playerid);
    public static string InfoText(ulong userid, string language);
    public static bool IsBanned(ulong userid);
    private static bool IsConnected(Info user);
    public static bool IsConnected(ulong userid);
    public static bool IsCrippled(ulong userid);
    public static bool IsFriend(BasePlayer a, BasePlayer b);
    public static bool IsFriend(BasePlayer player, ulong userid);
    public static bool IsInactive(BasePlayer player);
    private static bool IsStale(Info user);
    public static bool IsTeamMate(BasePlayer a, BasePlayer b);
    public static bool IsTeamMate(BasePlayer player, ulong userid);
    public static void Kick(ulong userid, string reason, IPlayer actor);
    public static void Load();
    private static Info Load(ulong userid);
    public static string Name(ulong userid);
    public static void OnBanned(ulong userid, bool banned);
    public static void OnConnected(BasePlayer player);
    public static void OnDisconnected(BasePlayer player);
    public static void OnLoot(BasePlayer player, ulong targetid);
    public static void Pardon(ulong userid, IPlayer actor);
    public static void Pardon(IPlayer actor);
    private static void Pardon(Queue<ulong> userid, IPlayer actor, int total);
    private static void Pardon(Info user, IPlayer actor, bool broadcast);
    public static void Save();
    private static void Save(Info user);
    private static void Scan();
    private static void SetLastSeenPosition(Info user, Vector3 position);
    private static void SetViolationPosition(Info user, Vector3 position);
    public static void SetViolationPosition(ulong userid);
    public static bool ShouldIgnore(BasePlayer player);
    public static string StatusText(ulong userid);
    public static string StatusText(ulong userid, BasePlayer player);
    public static string StatusText(ulong userid, IPlayer iplayer);
    public static string StatusText(ulong userid, ulong playerid);
    public static string StatusText(ulong userid, string language);
    private static string StatusText(Info user, string language);
    public static List<ulong> Team(ulong userid);
    public static TimeSpan TimeBanned(ulong userid);
    private static TimeSpan TimeBanned(Info user);
    public static TimeSpan TimeCrippled(ulong userid);
    private static TimeSpan TimeCrippled(Info user);
    private static TimeSpan TimeRemaining(DateTime time);
    public static TimeSpan TimeOffline(ulong userid);
    private static TimeSpan TimeOffline(Info user);
    public static TimeSpan TimeOnline(ulong userid);
    private static TimeSpan TimeOnline(Info user);
    private static TimeSpan TimeSpent(DateTime time);
    public static TimeSpan TimePlayed(ulong userid);
    private static TimeSpan TimePlayed(Info user);
    public static void Unban(ulong userid, bool manual, IPlayer actor);
    public static void Unban(IPlayer actor);
    private static void Unban(Info user, bool manual, IPlayer actor, bool broadcast);
    public static void Uncripple(ulong userid, bool manual, IPlayer actor);
    public static void Uncripple(IPlayer actor);
    private static void Uncripple(Info user, bool manual, IPlayer actor, bool broadcast);
    public static void Unload();
    public static void Update();
    private static void Update(Info user, string name, string address);
}

public class Settings
{
    public UserBypass Bypass;
    public UserFriend Friend;
    public UserTeam Team;
    public Settings();
    public class UserBypass
    {
        public ulong DaysSinceBan;
        public bool Enabled;
        public ulong HoursPlayed;
        public bool Multiply;
        public UserBypass();
        public void Validate();
    }

    public class UserFriend
    {
        public bool Damage;
    }

    public class UserTeam
    {
        public bool Damage;
    }

    public void Validate();
}

public class UserBypass
{
    public ulong DaysSinceBan;
    public bool Enabled;
    public ulong HoursPlayed;
    public bool Multiply;
    public UserBypass();
    public void Validate();
}

public class UserFriend
{
    public bool Damage;
}

public class UserTeam
{
    public bool Damage;
}

private class Info
{
    public ulong userid;
    public string name;
    public HashSet<string> names;
    public string address;
    public HashSet<string> addresses;
    public DateTime time_connected;
    public DateTime time_disconnected;
    public TimeSpan time_played;
    public ulong ban_count;
    public string ban_reason;
    public DateTime ban_time;
    public DateTime ban_timer;
    public bool is_banned;
    public ulong cripple_count;
    public string cripple_reason;
    public DateTime cripple_time;
    public DateTime cripple_timer;
    public bool is_crippled;
    public Vector3 l_position;
    public Vector3 v_position;
    [JsonIgnore]
    public DateTime access_time;
    [JsonIgnore]
    public ulong attacked;
    [JsonIgnore]
    public ulong attacker;
    [JsonIgnore]
    public bool dirty;
    [JsonIgnore]
    public BasePlayer player;
    [JsonIgnore]
    public Stack<ulong> teleport;
    [JsonIgnore]
    public List<ulong> victims;
    public Info();
}

private new class Version
{
    public class Settings
    {
        public int Major;
        public int Minor;
        public int Patch;
        public Settings();
        public int Compare(int major, int minor, int patch);
        public void Validate();
    }

    public static string String { get; set; }
}

public class Settings
{
    public int Major;
    public int Minor;
    public int Patch;
    public Settings();
    public int Compare(int major, int minor, int patch);
    public void Validate();
}

private class Violation
{
    public class Settings
    {
        public bool Ban;
        public ulong Cooldown;
        public float Sensitivity;
        public bool Warn;
        public Settings(bool ban, ulong cooldown, float sensitivity, bool warn);
        public ulong Count(ulong min, ulong max, bool squared);
        public Settings Validate(ulong max);
    }

    private bool ban;
    private Key category;
    private ulong cooldown;
    private ulong count;
    private TimeSpan rate;
    private bool warn;
    private Dictionary<ulong, History> histories;
    private static readonly Dictionary<string, Key> categories;
    private static ActionQueue triggers;
    private static readonly Violation violation;
    private class History
    {
        public DateTime cooldown;
        public ulong count;
        public DateTime time;
        public DateTime warned;
        public ulong warnings;
        public History();
    }

    public Violation(Key category);
    private void Broadcast(ulong userid, Key action, Key type, string details, Dictionary<string, string> hook_details);
    public static Key Category(string category);
    public static void Configure();
    public void Configure(Settings settings, ulong trigger_min, ulong trigger_max, ulong trigger_rate);
    public ulong Cooldown(ulong userid);
    public void Clear();
    private History Get(ulong userid);
    private bool IsFlooding(ulong userid);
    public static void Load();
    private void Reduce(ulong userid);
    private void Reset(ulong userid);
    public bool Trigger(ulong userid);
    public void Trigger(ulong userid, Key type, string details, bool kick, Dictionary<string, string> hook_details);
    public void Trigger(ulong userid, Key type, string details, ulong violations, bool kick, Dictionary<string, string> hook_details);
    private void Triggered(ulong userid, Key type, string details, ulong violations, bool kick, Dictionary<string, string> hook_details);
    public static void Unload();
    public void Warning(ulong userid, Key type, string details, Dictionary<string, string> hook_details);
    public void Zero(ulong userid);
}

public class Settings
{
    public bool Ban;
    public ulong Cooldown;
    public float Sensitivity;
    public bool Warn;
    public Settings(bool ban, ulong cooldown, float sensitivity, bool warn);
    public ulong Count(ulong min, ulong max, bool squared);
    public Settings Validate(ulong max);
}

private class History
{
    public DateTime cooldown;
    public ulong count;
    public DateTime time;
    public DateTime warned;
    public ulong warnings;
    public History();
}

private class VPN
{
    public class Settings
    {
        public VpnApi API;
        public VpnCache Cache;
        public VpnCheck Check;
        public VpnViolation Violation;
        public Settings();
        public class VpnApi
        {
            public Guardian.API.Settings GetIpIntel;
            public Guardian.API.Settings IpApi;
            public Guardian.API.Settings IpHub;
            public Guardian.API.Settings IpQualityScore;
            public VpnApi();
            public void Validate();
        }

        public class VpnCache
        {
            public ulong Hours;
            public VpnCache();
        }

        public class VpnCheck
        {
            public bool Enabled;
            public bool Strict;
        }

        public class VpnViolation
        {
            public bool Ban;
            public bool Enabled;
            public bool Warn;
            public VpnViolation();
        }

        public void Validate();
    }

    private static ActionQueue checks;
    private static readonly Violation violation;
    public class API
    {
        public class GetIpIntel
        {
            [JsonProperty("message")]
            public string Message { get; set; }
            [JsonProperty("result")]
            public float Result { get; set; }
            [JsonProperty("status")]
            public string Status { get; set; }
            private const string api;
            public static void Check(string address, ulong userid);
        }

        public class IpApi
        {
            [JsonProperty("hosting")]
            public bool Hosting { get; set; }
            [JsonProperty("proxy")]
            public bool Proxy { get; set; }
            [JsonProperty("status")]
            public string Status { get; set; }
            private const string api;
            public static void Check(string address, ulong userid);
        }

        public class IpHub
        {
            [JsonProperty("block")]
            public int Block { get; set; }
            [JsonIgnore]
            private static readonly Dictionary<string, string> headers;
            private const string api;
            public static void Check(string address, ulong userid);
            public static void Configure();
            public static void Unload();
        }

        public class IpQualityScore
        {
            [JsonProperty("fraud_score")]
            public int FraudScore { get; set; }
            [JsonProperty("message")]
            public string Message { get; set; }
            [JsonProperty("proxy")]
            public bool Proxy { get; set; }
            [JsonProperty("recent_abuse")]
            public bool RecentAbuse { get; set; }
            [JsonProperty("success")]
            public bool Success { get; set; }
            [JsonProperty("vpn")]
            public bool VPN { get; set; }
            private const string api;
            public static void Check(string address, ulong userid);
        }

        public static void Configure();
        public static void Unload();
    }

    private class Cache
    {
        private static readonly DataFile<string, DateTime> blocks;
        private static readonly DataFile<string, DateTime> bypass;
        public static void Block(string address);
        public static void Bypass(string address, ulong _reserved);
        public static bool IsBlocked(string address);
        public static bool IsBypassed(string address);
        private static bool IsCached(DataFile<string, DateTime> cache, string address);
        private static bool IsExpired(DateTime timestamp, DateTime check);
        public static void Load();
        public static void Save();
        public static void Unblock(string address);
        public static void Unload();
        private static void Update();
        private static void Update(DataFile<string, DateTime> cache, DateTime current);
    }

    public static void Bypass(string address);
    public static void Check(string address, ulong userid);
    private static void Check(string address, ulong userid, float delay);
    private static void Check(float delay, string address, ulong userid, Action<string, ulong> callback);
    public static void Configure();
    public static bool IsBlocked(string address);
    public static bool IsBypassed(string address);
    public static void Load();
    public static void Save();
    public static void Unblock(string address);
    public static void Unload();
    private static void Violation(string address, ulong userid, string api);
}

public class Settings
{
    public VpnApi API;
    public VpnCache Cache;
    public VpnCheck Check;
    public VpnViolation Violation;
    public Settings();
    public class VpnApi
    {
        public Guardian.API.Settings GetIpIntel;
        public Guardian.API.Settings IpApi;
        public Guardian.API.Settings IpHub;
        public Guardian.API.Settings IpQualityScore;
        public VpnApi();
        public void Validate();
    }

    public class VpnCache
    {
        public ulong Hours;
        public VpnCache();
    }

    public class VpnCheck
    {
        public bool Enabled;
        public bool Strict;
    }

    public class VpnViolation
    {
        public bool Ban;
        public bool Enabled;
        public bool Warn;
        public VpnViolation();
    }

    public void Validate();
}

public class VpnApi
{
    public Guardian.API.Settings GetIpIntel;
    public Guardian.API.Settings IpApi;
    public Guardian.API.Settings IpHub;
    public Guardian.API.Settings IpQualityScore;
    public VpnApi();
    public void Validate();
}

public class VpnCache
{
    public ulong Hours;
    public VpnCache();
}

public class VpnCheck
{
    public bool Enabled;
    public bool Strict;
}

public class VpnViolation
{
    public bool Ban;
    public bool Enabled;
    public bool Warn;
    public VpnViolation();
}

public class API
{
    public class GetIpIntel
    {
        [JsonProperty("message")]
        public string Message { get; set; }
        [JsonProperty("result")]
        public float Result { get; set; }
        [JsonProperty("status")]
        public string Status { get; set; }
        private const string api;
        public static void Check(string address, ulong userid);
    }

    public class IpApi
    {
        [JsonProperty("hosting")]
        public bool Hosting { get; set; }
        [JsonProperty("proxy")]
        public bool Proxy { get; set; }
        [JsonProperty("status")]
        public string Status { get; set; }
        private const string api;
        public static void Check(string address, ulong userid);
    }

    public class IpHub
    {
        [JsonProperty("block")]
        public int Block { get; set; }
        [JsonIgnore]
        private static readonly Dictionary<string, string> headers;
        private const string api;
        public static void Check(string address, ulong userid);
        public static void Configure();
        public static void Unload();
    }

    public class IpQualityScore
    {
        [JsonProperty("fraud_score")]
        public int FraudScore { get; set; }
        [JsonProperty("message")]
        public string Message { get; set; }
        [JsonProperty("proxy")]
        public bool Proxy { get; set; }
        [JsonProperty("recent_abuse")]
        public bool RecentAbuse { get; set; }
        [JsonProperty("success")]
        public bool Success { get; set; }
        [JsonProperty("vpn")]
        public bool VPN { get; set; }
        private const string api;
        public static void Check(string address, ulong userid);
    }

    public static void Configure();
    public static void Unload();
}

public class GetIpIntel
{
    [JsonProperty("message")]
    public string Message { get; set; }
    [JsonProperty("result")]
    public float Result { get; set; }
    [JsonProperty("status")]
    public string Status { get; set; }
    private const string api;
    public static void Check(string address, ulong userid);
}

public class IpApi
{
    [JsonProperty("hosting")]
    public bool Hosting { get; set; }
    [JsonProperty("proxy")]
    public bool Proxy { get; set; }
    [JsonProperty("status")]
    public string Status { get; set; }
    private const string api;
    public static void Check(string address, ulong userid);
}

public class IpHub
{
    [JsonProperty("block")]
    public int Block { get; set; }
    [JsonIgnore]
    private static readonly Dictionary<string, string> headers;
    private const string api;
    public static void Check(string address, ulong userid);
    public static void Configure();
    public static void Unload();
}

public class IpQualityScore
{
    [JsonProperty("fraud_score")]
    public int FraudScore { get; set; }
    [JsonProperty("message")]
    public string Message { get; set; }
    [JsonProperty("proxy")]
    public bool Proxy { get; set; }
    [JsonProperty("recent_abuse")]
    public bool RecentAbuse { get; set; }
    [JsonProperty("success")]
    public bool Success { get; set; }
    [JsonProperty("vpn")]
    public bool VPN { get; set; }
    private const string api;
    public static void Check(string address, ulong userid);
}

private class Cache
{
    private static readonly DataFile<string, DateTime> blocks;
    private static readonly DataFile<string, DateTime> bypass;
    public static void Block(string address);
    public static void Bypass(string address, ulong _reserved);
    public static bool IsBlocked(string address);
    public static bool IsBypassed(string address);
    private static bool IsCached(DataFile<string, DateTime> cache, string address);
    private static bool IsExpired(DateTime timestamp, DateTime check);
    public static void Load();
    public static void Save();
    public static void Unblock(string address);
    public static void Unload();
    private static void Update();
    private static void Update(DataFile<string, DateTime> cache, DateTime current);
}

private class Weapon
{
    public float Accuracy { get; set; }
    public Vector3 AimAngle { get; set; }
    public float AimCone { get; set; }
    public float AimSway { get; set; }
    public string AmmoName { get; set; }
    public List<string> Attachments { get; set; }
    public bool Automatic { get; set; }
    public DateTime Fired { get; set; }
    public string Name { get; set; }
    public float Pitch { get; set; }
    public BasePlayer Player { get; set; }
    public Vector3 Position { get; set; }
    public List<int> Projectiles { get; set; }
    public float Range { get; set; }
    public float Repeat { get; set; }
    public bool Shell { get; set; }
    public string ShortName { get; set; }
    public float Speed { get; set; }
    public bool Spread { get; set; }
    public float Swing { get; set; }
    public float Velocity { get; set; }
    public float Yaw { get; set; }
    public float Zoom { get; set; }
    private static readonly Queue<Weapon> pool;
    private static readonly int ArrowBone;
    private static readonly int ArrowFire;
    private static readonly int ArrowHV;
    private static readonly int ArrowWooden;
    private static readonly int GrenadeHE;
    private static readonly int GrenadeShotgun;
    private static readonly int GrenadeSmoke;
    private static readonly int NailgunNails;
    private static readonly int PistolBullet;
    private static readonly int PistolHV;
    private static readonly int PistolIncendiary;
    private static readonly int RifleAmmo;
    private static readonly int RifleExplosive;
    private static readonly int RifleHV;
    private static readonly int RifleIncendiary;
    private static readonly int Rocket;
    private static readonly int RocketHV;
    private static readonly int RocketIncendiary;
    private static readonly int ShellBuckshot;
    private static readonly int ShellHandmade;
    private static readonly int ShellIncendiary;
    private static readonly int ShellSlug;
    private static readonly int AssaultRifle;
    private static readonly int BoltActionRifle;
    private static readonly int CompoundBow;
    private static readonly int Crossbow;
    private static readonly int CustomSMG;
    private static readonly int DoubleBarrelShotgun;
    private static readonly int EokaPistol;
    private static readonly int HuntingBow;
    private static readonly int L96Rifle;
    private static readonly int LR300AssaultRifle;
    private static readonly int M249;
    private static readonly int M39Rifle;
    private static readonly int M92Pistol;
    private static readonly int MP5A4;
    private static readonly int MultipleGrenadeLauncher;
    private static readonly int Nailgun;
    private static readonly int PumpShotgun;
    private static readonly int PythonRevolver;
    private static readonly int Revolver;
    private static readonly int RocketLauncher;
    private static readonly int SemiAutomaticPistol;
    private static readonly int SemiAutomaticRifle;
    private static readonly int Spas12Shotgun;
    private static readonly int Thompson;
    private static readonly int WaterpipeShotgun;
    private class Ammo
    {
        public float AimCone { get; set; }
        public float Range { get; set; }
        public float Velocity { get; set; }
        public Ammo(float aimcone, float range, float velocity);
    }

    private class Info
    {
        public float Accuracy { get; set; }
        public Dictionary<int, Ammo> Ammo { get; set; }
        public bool Automatic { get; set; }
        public string Name { get; set; }
        public Recoil Recoil { get; set; }
        public float Repeat { get; set; }
    }

    private class Recoil
    {
        public float Pitch { get; set; }
        public float Yaw { get; set; }
        public Recoil(float pitch, float yaw);
    }

    private static Dictionary<int, Info> weapons;
    private Weapon();
    private static Weapon Get();
    public static Weapon Get(ulong userid, int projectileid);
    public static Weapon Get(BaseProjectile weapon_fired, BasePlayer player, ProtoBuf.ProjectileShoot fired);
    public static bool IsValid(Item item);
    public static void Load();
    public void Release();
    public void SetSwing(float amount);
    public static void Unload();
}

private class Ammo
{
    public float AimCone { get; set; }
    public float Range { get; set; }
    public float Velocity { get; set; }
    public Ammo(float aimcone, float range, float velocity);
}

private class Info
{
    public float Accuracy { get; set; }
    public Dictionary<int, Ammo> Ammo { get; set; }
    public bool Automatic { get; set; }
    public string Name { get; set; }
    public Recoil Recoil { get; set; }
    public float Repeat { get; set; }
}

private class Recoil
{
    public float Pitch { get; set; }
    public float Yaw { get; set; }
    public Recoil(float pitch, float yaw);
}

private class WebHook
{
    public static void Send(string url, string category, string message);
}


```

---

## GuardianADR by Rockioux - Starts demo recordings on Guardian Violations.

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Guardian ADR", "Rockioux", "1.0.2")]
[Description("Starts demo recordings on Guardian Violations.")]
 class GuardianADR : CovalencePlugin
{
    private class GuardianADRConfig
    {
        [JsonProperty(PropertyName = "Gravity Violation Demo Config")]
        public HookConfig GravityHookConfig { get; set; }
        [JsonProperty(PropertyName = "Melee Rate Violation Demo Config")]
        public HookConfig MeleeRateConfig { get; set; }
        [JsonProperty(PropertyName = "NoClip Violation Demo Config")]
        public HookConfig NoClipConfig { get; set; }
        [JsonProperty(PropertyName = "InsideTerrain Violation Demo Config")]
        public HookConfig InsideTerrainConfig { get; set; }
    }

    private class HookConfig
    {
        [JsonProperty(PropertyName = "Enable Demo Recordings for this hook")]
        public bool HookEnabled { get; set; }
        [JsonProperty(PropertyName = "Demo Duration in minutes")]
        public int DemoDuration { get; set; }
        [JsonProperty(PropertyName = "Discord Webhook")]
        public string DiscordWebhook { get; set; }
        public HookConfig();
        public HookConfig(bool enabled);
    }

    [PluginReference]
    private Plugin AutoDemoRecord;
    private GuardianADRConfig _config;
    private void Init();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnGuardianAntiCheatGravity(string playerID, Dictionary<string, string> details);
    private void OnGuardianAntiCheatMeleeRate(string playerID, Dictionary<string, string> details);
    private void OnGuardianServer(string playerID, Dictionary<string, string> details);
    private bool GetBasePlayer(string playerID, BasePlayer player);
}

private class GuardianADRConfig
{
    [JsonProperty(PropertyName = "Gravity Violation Demo Config")]
    public HookConfig GravityHookConfig { get; set; }
    [JsonProperty(PropertyName = "Melee Rate Violation Demo Config")]
    public HookConfig MeleeRateConfig { get; set; }
    [JsonProperty(PropertyName = "NoClip Violation Demo Config")]
    public HookConfig NoClipConfig { get; set; }
    [JsonProperty(PropertyName = "InsideTerrain Violation Demo Config")]
    public HookConfig InsideTerrainConfig { get; set; }
}

private class HookConfig
{
    [JsonProperty(PropertyName = "Enable Demo Recordings for this hook")]
    public bool HookEnabled { get; set; }
    [JsonProperty(PropertyName = "Demo Duration in minutes")]
    public int DemoDuration { get; set; }
    [JsonProperty(PropertyName = "Discord Webhook")]
    public string DiscordWebhook { get; set; }
    public HookConfig();
    public HookConfig(bool enabled);
}


```

---

## GuessTheNumber by Mabel - An event that requires player to guess the correct number

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System.Text;
using UnityEngine;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core;

Oxide.Plugins
[Info("Guess The Number", "Mabel", "2.2.3")]
[Description("An event that requires player to guess the correct number")]
 class GuessTheNumber : RustPlugin
{
    [PluginReference]
    private readonly Plugin Battlepass;
    private readonly Plugin ServerRewards;
    private readonly Plugin Economics;
    private readonly Plugin SkillTree;
    private readonly Plugin XPerience;
    private Data data;
    public Dictionary<ulong, int> playerInfo;
     bool useEconomics;
     bool useEconomicsloss;
     bool useServerRewards;
     bool useServerRewardsloss;
     bool useBattlepass1;
     bool useBattlepass2;
     bool useBattlepassloss;
     bool useSkillTree;
     bool useSkillTreeloss;
     bool useXPerience;
     bool useXPerienceloss;
     bool useItem;
     bool useCommand;
     bool autoEventsEnabled;
     bool showAttempts;
     bool showAttemptsTips;
     bool useChat;
     bool useGameTip;
     float gameTipDuration;
     float autoEventTime;
     float eventLength;
     int minDefault;
     int maxDefault;
     int maxTries;
     int MinPlayer;
     int economicsWinReward;
     int economicsLossReward;
     int serverRewardsWinReward;
     int serverRewardsLossReward;
     int battlepassWinReward1;
     int battlepassWinReward2;
     int battlepassLossReward1;
     int battlepassLossReward2;
     int skillTreeWinReward;
     int skillTreeLossReward;
     int xPerienceWinReward;
     int xPerienceLossReward;
     int itemWinReward;
     string itemName;
     ulong itemSkin;
     string itemCustomName;
     string commandName;
     string commandToExecute;
     string chatCommand;
     string Prefix;
     ulong SteamIDIcon;
    private bool AddSecondCurrency;
    private bool AddFirstCurrency;
    const string permissionNameADMIN;
    const string permissionNameENTER;
     bool Changed;
     bool eventActive;
     Timer eventTimer;
     Timer autoRepeatTimer;
     int minNumber;
     int maxNumber;
     bool hasEconomics;
     bool hasServerRewards;
     bool hasBattlepass;
     bool hasSkillTree;
     bool hasXPerince;
     int number;
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void Init();
     void Unload();
    private void OnServerInitialized();
     void Loaded();
    [ConsoleCommand("gtn")]
     void GTNCONSOLECMD(ConsoleSystem.Arg args);
    [ChatCommand("gtn")]
    private void startGuessNumberEvent(BasePlayer player, string cmd, string[] args);
    [ChatCommand("guess")]
    private void numberReply(BasePlayer player, string cmd, string[] args);
     void StartEvent();
     string DoHelpMenu();
     bool IsNumber(string str);
     object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string id);
     void SendToastToActivePlayers(string messageKey);
     void SendMessageToActivePlayers(string messageKey);
    [ConsoleCommand("gtn_wipe")]
    private void WipeData(ConsoleSystem.Arg arg);
    public class Data
    {
        public Dictionary<string, int> Winners { get; set; }
    }

    private void SaveData();
    private void LoadData();
    private void ResetData();
     void OnNewSave();
    private void LogWinner(string playerName);
    private Dictionary<string, int> GetWinners();
    private List<KeyValuePair<string, int>> GetTopWinners();
    private void TopCommand(BasePlayer player, string cmd);
    private HashSet<BasePlayer> UiPlayers;
    [ConsoleCommand("guessnumber.close")]
    private void CloseGuessTheNumberUI(ConsoleSystem.Arg arg);
    private void killUI();
    private void GuessTheNumberUI(BasePlayer player);
}

public class Data
{
    public Dictionary<string, int> Winners { get; set; }
}


```

---

## GuessTheWord by Bazz3l - Guess the scrambled word and receive a reward

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Guess The Word", "Bazz3l", "1.0.7")]
[Description("Guess the scrambled word and receive a reward.")]
 class GuessTheWord : CovalencePlugin
{
    [PluginReference]
     Plugin ServerRewards;
     Plugin Economics;
     List<string> wordList;
     bool eventActive;
     Timer eventRepeatTimer;
     Timer eventTimer;
     string currentScramble;
     string currentWord;
     PluginConfig config;
     PluginConfig GetDefaultConfig();
     class PluginConfig
    {
        public string APIEndpoint;
        public bool UseServerRewards;
        public bool UseEconomics;
        public int ServerRewardPoints;
        public double EconomicsPoints;
        public int MinWordLength;
        public int MaxWordLength;
        public int MaxWords;
        public float eventTime;
        public float eventLength;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void OnServerInitialized();
     void Init();
     void StartEvent();
     void EventEnded();
     void ResetEvent();
     void FetchWordList();
     string ScrambleWord();
     bool CheckGuess(string currentGuess);
     void RewardPlayer(IPlayer player);
     void MessageAll(string key, object[] args);
    [Command("word")]
     void WordCommand(IPlayer player, string command, string[] args);
     string Lang(string key, string id, object[] args);
}

 class PluginConfig
{
    public string APIEndpoint;
    public bool UseServerRewards;
    public bool UseEconomics;
    public int ServerRewardPoints;
    public double EconomicsPoints;
    public int MinWordLength;
    public int MaxWordLength;
    public int MaxWords;
    public float eventTime;
    public float eventLength;
}


```

---

## GUIAnnouncements by JoeSheep - Creates announcements with custom messages across the top of player's screens

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Globalization;
using System.Collections;
using UnityEngine;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("GUIAnnouncements", "JoeSheep", "2.0.6", ResourceId = 1222)]
[Description("Creates announcements with custom messages across the top of player's screens.")]
 class GUIAnnouncements : RustPlugin
{
    const string PermAnnounce;
    const string PermAnnounceToggle;
    const string PermAnnounceGetNextRestart;
    const string PermAnnounceJoinLeave;
    private Dictionary<ulong, string> Exclusions;
    private HashSet<ulong> JustJoined;
    private HashSet<ulong> GlobalTimerPlayerList;
    private Dictionary<BasePlayer, Timer> PrivateTimers;
    private Dictionary<BasePlayer, Timer> NewPlayerPrivateTimers;
    private Dictionary<BasePlayer, Timer> PlayerRespawnedTimers;
    private Timer PlayerTimer;
    private Timer GlobalTimer;
    private Timer NewPlayerTimer;
    private Timer PlayerRespawnedTimer;
    private Timer RealTimeTimer;
    private bool RealTimeTimerStarted;
    private Timer SixtySecondsTimer;
    private Timer AutomaticAnnouncementsTimer;
    private Timer GetNextRestartTimer;
    private string HeliLastHitPlayer;
    private string CH47LastHitPlayer;
    private string APCLastHitPlayer;
    private HashSet<ulong> HeliNetIDs;
    private HashSet<ulong> CH47NetIDs;
    private HashSet<ulong> KilledCH47NetIDs;
    private bool ConfigUpdated;
    private static readonly int WorldSize;
    private List<DateTime> RestartTimes;
    private Dictionary<DateTime, TimeSpan> CalcNextRestartDict;
    private DateTime NextRestart;
     List<TimeSpan> RestartAnnouncementsTimeSpans;
    private int LastHour;
    private int LastMinute;
    private bool RestartCountdown;
    private bool RestartJustScheduled;
    private bool RestartScheduled;
    private string RestartReason;
    private List<string> RestartAnnouncementsWhenStrings;
    private DateTime ScheduledRestart;
    private TimeSpan AutomaticTimedAnnouncementsRepeatTimeSpan;
    private bool RestartSuspended;
    private bool DontCheckNextRestart;
    private bool MutingBans;
    private bool AnnouncingGameTime;
    private string LastGameTime;
    private int GameTimeCurrentCount;
    private bool AnnouncingRealTime;
    private string LastRealTime;
    private int RealTimeCurrentCount;
    private List<string> GameTimes;
    private List<string> RealTimes;
    private IEnumerator<List<string>> ATALEnum;
    private List<List<string>> AutomaticTimedAnnouncementsStringList;
     string BannerTintGrey;
     string BannerTintRed;
     string BannerTintOrange;
     string BannerTintYellow;
     string BannerTintGreen;
     string BannerTintCyan;
     string BannerTintBlue;
     string BannerTintPurple;
     string TextRed;
     string TextOrange;
     string TextYellow;
     string TextGreen;
     string TextCyan;
     string TextBlue;
     string TextPurple;
     string TextWhite;
     string BannerAnchorMaxX;
     string BannerAnchorMaxYDefault;
     string BannerAnchorMaxY;
     string BannerAnchorMinX;
     string BannerAnchorMinYDefault;
     string BannerAnchorMinY;
     string TextAnchorMaxX;
     string TextAnchorMaxYDefault;
     string TextAnchorMaxY;
     string TextAnchorMinX;
     string TextAnchorMinYDefault;
     string TextAnchorMinY;
     string TABannerAnchorMaxY;
     string TABannerAnchorMinY;
     string TATextAnchorMaxY;
     string TATextAnchorMinY;
    public string BannerColorList { get; set; }
    public string TextColorList { get; set; }
    public bool AirdropAnnouncementsEnabled { get; set; }
    public bool AirdropAnnouncementsLocation { get; set; }
    public string AirdropAnnouncementsText { get; set; }
    public string AirdropAnnouncementsTextWithGrid { get; set; }
    public string AirdropAnnouncementsBannerColor { get; set; }
    public string AirdropAnnouncementsTextColor { get; set; }
    public bool AutomaticGameTimeAnnouncementsEnabled { get; set; }
    public Dictionary<string, List<object>> AutomaticGameTimeAnnouncementsList { get; set; }
    public string AutomaticGameTimeAnnouncementsBannerColor { get; set; }
    public string AutomaticGameTimeAnnouncementsTextColor { get; set; }
    public bool AutomaticTimedAnnouncementsEnabled { get; set; }
    public static List<object> AutomaticTimedAnnouncementsList { get; set; }
    public string AutomaticTimedAnnouncementsRepeat { get; set; }
    public string AutomaticTimedAnnouncementsBannerColor { get; set; }
    public string AutomaticTimedAnnouncementsTextColor { get; set; }
    public bool AutomaticRealtimeAnnouncementsEnabled { get; set; }
    public Dictionary<string, List<object>> AutomaticRealTimeAnnouncementsList { get; set; }
    public string AutomaticRealTimeAnnouncementsBannerColor { get; set; }
    public string AutomaticRealTimeAnnouncementsTextColor { get; set; }
    public bool StockingRefillAnnouncementsEnabled { get; set; }
    public string StockingRefillAnnouncementText { get; set; }
    public string StockingRefillAnnouncementBannerColor { get; set; }
    public string StockingRefillAnnouncementTextColor { get; set; }
    public float AnnouncementDuration { get; set; }
    public int FontSize { get; set; }
    public float FadeOutTime { get; set; }
    public float FadeInTime { get; set; }
    public static float AdjustVPosition { get; set; }
    public bool GlobalLeaveAnnouncementsEnabled { get; set; }
    public bool GlobalJoinAnnouncementsEnabled { get; set; }
    public bool GlobalJoinLeavePermissionOnly { get; set; }
    public string GlobalLeaveText { get; set; }
    public string GlobalJoinText { get; set; }
    public string GlobalLeaveAnnouncementBannerColor { get; set; }
    public string GlobalLeaveAnnouncementTextColor { get; set; }
    public string GlobalJoinAnnouncementBannerColor { get; set; }
    public string GlobalJoinAnnouncementTextColor { get; set; }
    public bool HelicopterSpawnAnnouncementEnabled { get; set; }
    public bool HelicopterDespawnAnnouncementEnabled { get; set; }
    public bool HelicopterDestroyedAnnouncementEnabled { get; set; }
    public bool HelicopterDestroyedAnnouncementWithDestroyer { get; set; }
    public string HelicopterSpawnAnnouncementText { get; set; }
    public string HelicopterDespawnAnnouncementText { get; set; }
    public string HelicopterDestroyedAnnouncementText { get; set; }
    public string HelicopterDestroyedAnnouncementWithDestroyerText { get; set; }
    public string HelicopterSpawnAnnouncementBannerColor { get; set; }
    public string HelicopterSpawnAnnouncementTextColor { get; set; }
    public string HelicopterDestroyedAnnouncementBannerColor { get; set; }
    public string HelicopterDestroyedAnnouncementTextColor { get; set; }
    public string HelicopterDespawnAnnouncementBannerColor { get; set; }
    public string HelicopterDespawnAnnouncementTextColor { get; set; }
    public bool CH47SpawnAnnouncementsEnabled { get; set; }
    public bool CH47DespawnAnnouncementsEnabled { get; set; }
    public bool CH47DestroyedAnnouncementsEnabled { get; set; }
    public bool CH47DestroyedAnnouncementsWithDestroyer { get; set; }
    public bool CH47CrateDroppedAnnouncementsEnabled { get; set; }
    public bool CH47CrateDroppedAnnouncementsWithLocation { get; set; }
    public string CH47SpawnAnnouncementText { get; set; }
    public string CH47DespawnAnnouncementText { get; set; }
    public string CH47DestroyedAnnouncementText { get; set; }
    public string CH47DestroyedAnnouncementWithDestroyerText { get; set; }
    public string CH47CrateDroppedAnnouncementText { get; set; }
    public string CH47CrateDroppedAnnouncementTextWithGrid { get; set; }
    public string CH47SpawnAnnouncementBannerColor { get; set; }
    public string CH47SpawnAnnouncementTextColor { get; set; }
    public string CH47DestroyedAnnouncementBannerColor { get; set; }
    public string CH47DestroyedAnnouncementTextColor { get; set; }
    public string CH47DespawnAnnouncementBannerColor { get; set; }
    public string CH47DespawnAnnouncementTextColor { get; set; }
    public string CH47CrateDroppedAnnouncementBannerColor { get; set; }
    public string CH47CrateDroppedAnnouncementTextColor { get; set; }
    public bool APCSpawnAnnouncementsEnabled { get; set; }
    public bool APCDestroyedAnnouncementsEnabled { get; set; }
    public bool APCDestroyedAnnouncementsWithDestroyer { get; set; }
    public string APCSpawnAnnouncementText { get; set; }
    public string APCDestroyedAnnouncementText { get; set; }
    public string APCDestroyedAnnouncementWithDestroyerText { get; set; }
    public string APCSpawnAnnouncementBannerColor { get; set; }
    public string APCSpawnAnnouncementTextColor { get; set; }
    public string APCDestroyedAnnouncementBannerColor { get; set; }
    public string APCDestroyedAnnouncementTextColor { get; set; }
    public bool CargoshipSpawnAnnouncementsEnabled { get; set; }
    public bool CargoshipEgressAnnouncementsEnabled { get; set; }
    public string CargoshipSpawnAnnouncementText { get; set; }
    public string CargoshipEgressAnnouncementText { get; set; }
    public string CargoshipSpawnAnnouncementBannerColor { get; set; }
    public string CargoshipSpawnAnnouncementTextColor { get; set; }
    public string CargoshipEgressAnnouncementBannerColor { get; set; }
    public string CargoshipEgressAnnouncementTextColor { get; set; }
    public bool CrateHackAnnouncementsEnabled { get; set; }
    public bool CrateHackSpecifyOilRig { get; set; }
    public string CrateHackAnnouncementText { get; set; }
    public string CrateHackAnnouncementSmallOilRigText { get; set; }
    public string CrateHackAnnouncementLargeOilRigText { get; set; }
    public string CrateHackAnnouncementBannerColor { get; set; }
    public string CrateHackAnnouncementTextColor { get; set; }
    public bool NewPlayerAnnouncementsEnabled { get; set; }
    public string NewPlayerAnnouncementsBannerColor { get; set; }
    public string NewPlayerAnnouncementsTextColor { get; set; }
    public Dictionary<int, List<object>> NewPlayerAnnouncementsList { get; set; }
    public bool PlayerBannedAnnouncementsEnabled { get; set; }
    public string PlayerBannedAnnouncmentText { get; set; }
    public string PlayerBannedAnnouncementBannerColor { get; set; }
    public string PlayerBannedAnnouncementTextColor { get; set; }
    public bool RespawnAnnouncementsEnabled { get; set; }
    public string RespawnAnnouncementsBannerColor { get; set; }
    public string RespawnAnnouncementsTextColor { get; set; }
    public List<object> RespawnAnnouncementsList { get; set; }
    public bool RestartAnnouncementsEnabled { get; set; }
    public string RestartAnnouncementsFormat { get; set; }
    public string RestartAnnouncementsBannerColor { get; set; }
    public string RestartAnnouncementsTextColor { get; set; }
    public List<object> RestartTimesList { get; set; }
    public List<object> RestartAnnouncementsTimes { get; set; }
    public bool RestartServer { get; set; }
    public string RestartSuspendedAnnouncement { get; set; }
    public string RestartCancelledAnnouncement { get; set; }
    public float TestAnnouncementDuration { get; set; }
    public int TestAnnouncementFontSize { get; set; }
    public float TestAnnouncementFadeOutTime { get; set; }
    public float TestAnnouncementFadeInTime { get; set; }
    public static float TestAnnouncementAdjustVPosition { get; set; }
    public string TestAnnouncementBannerColor { get; set; }
    public string TestAnnouncementsTextColor { get; set; }
    public bool DoNotOverlayLustyMap { get; set; }
    public string LustyMapPosition { get; set; }
    public bool WelcomeAnnouncementsEnabled { get; set; }
    public string WelcomeAnnouncementText { get; set; }
    public string WelcomeBackAnnouncementText { get; set; }
    public string WelcomeAnnouncementBannerColor { get; set; }
    public string WelcomeAnnouncementTextColor { get; set; }
    public float WelcomeAnnouncementDuration { get; set; }
    public float WelcomeAnnouncementDelay { get; set; }
    public bool WelcomeBackAnnouncement { get; set; }
    private void LoadGUIAnnouncementsConfig();
    protected override void LoadDefaultConfig();
    private T GetConfig(string category, string setting, T defaultValue);
    private List<string> ConvertObjectListToString(object value);
     void SaveData();
     void LoadSavedData();
     class StoredData
    {
        public Dictionary<ulong, PlayerData> PlayerData;
        public StoredData();
    }

     class PlayerData
    {
        public string Name;
        public string UserID;
        public int TimesJoined;
        public bool Dead;
        public PlayerData();
    }

     void CreatePlayerData(BasePlayer player);
     StoredData storedData;
     void OnServerSave();
    private Dictionary<ulong, AnnouncementInfo> AnnouncementsData;
     class AnnouncementInfo
    {
        public string BannerTintColor;
        public string TextColor;
        public bool IsTestAnnouncement;
        public AnnouncementInfo();
    }

     void StoreAnnouncementData(BasePlayer player, string BannerTintColor, string TextColor, bool IsTestAnnouncement);
     void LoadDefaultMessages();
     void OnServerInitialized();
    [HookMethod("CreateAnnouncement")]
     void CreateAnnouncement(string Msg, string bannerTintColor, string textColor, BasePlayer player, float APIAdjustVPosition, bool isWelcomeAnnouncement, bool isRestartAnnouncement, string group, bool isTestAnnouncement);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void DestroyAllGUI();
     void DestroyGlobalGUI();
     void DestroyPrivateGUI(BasePlayer player);
     void DestroyAllTimers(BasePlayer player);
     void DestroyPrivateTimer(BasePlayer player);
     void Unload();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     string ConvertBannerColor(string BColor);
     string ConvertTextColor(string TColor);
     void CheckBannerColor(string BColor, string Category, string Setting, string Default);
     void CheckTextColor(string TColor, string Category, string Setting, string Default);
    private static BasePlayer FindPlayer(string IDName);
    private bool HasPermission(BasePlayer player, string perm);
     void GameTimeAnnouncementsStart();
     void AutomaticRealTimeAnnouncementsStart();
     void AutomaticTimedAnnouncementsStart();
     void RestartAnnouncementsStart();
     void GetNextRestart(List<DateTime> DateTimes);
    private string GetGrid(Vector3 position, BasePlayer player);
    private void SendReply(BasePlayer Commander, string format, bool isConsole, object[] args);
     string Lang(string key, string userId);
     void RestartAnnouncements();
     void GameTimeAnnouncements();
     void RealTimeAnnouncements();
     void OnUserBanned(string id, string name, string IP, string reason);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityDeath(BaseCombatEntity entity);
     void OnCargoShipEgress(CargoShip cargoship);
     void OnEntityKill(BaseNetworkable entity);
     void OnAirdrop(CargoPlane plane, Vector3 location);
     void OnHelicopterDropCrate(CH47HelicopterAIController heli);
     void OnCrateHack(HackableLockedCrate crate);
     void WelcomeAnnouncement(BasePlayer player);
     void NewPlayerAnnouncements(BasePlayer player);
     void RespawnedAnnouncements(BasePlayer player);
     void AutomaticTimedAnnouncements();
     void ConsoleCMDInput(ConsoleSystem.Arg inputter);
     void CMDHandler(BasePlayer commander, string cmd, string[] args, bool isConsole, bool isAdmin, string userID);
     void Announce(BasePlayer player, string[] args);
     void AnnounceToPlayer(BasePlayer player, string[] args, string userID, bool isConsole);
     void AnnounceToGroup(BasePlayer player, string[] args, bool isConsole);
     void AnnounceTest(BasePlayer player, string userID, bool isConsole);
     void DestroyAnnouncement(BasePlayer player);
     void MuteBans(BasePlayer player, string userID, bool isConsole);
     void ToggleAnnouncements(BasePlayer player, string[] args, string userID, bool isConsole);
     void ScheduleRestart(BasePlayer player, string[] args, string userID, bool isConsole);
     void SuspendRestart(BasePlayer player, string userID, bool isConsole);
     void ResumeRestart(BasePlayer player, string userID, bool isConsole);
     void GetNextRestart(BasePlayer player, string userID, bool isConsole);
     void CancelScheduledRestart(BasePlayer player, string userID, bool isConsole);
}

 class StoredData
{
    public Dictionary<ulong, PlayerData> PlayerData;
    public StoredData();
}

 class PlayerData
{
    public string Name;
    public string UserID;
    public int TimesJoined;
    public bool Dead;
    public PlayerData();
}

 class AnnouncementInfo
{
    public string BannerTintColor;
    public string TextColor;
    public bool IsTestAnnouncement;
    public AnnouncementInfo();
}


```

---

## GUIShop by OfficeEgg - GUI Shop based on Economics, supports NPCs

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System.Globalization;
using System.Text;
using System.Text.RegularExpressions;
using Rust;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Steamworks;
using WebSocketSharp;
using Physics = UnityEngine.Physics;

Oxide.Plugins
[Info("GUIShop", "Khan", "2.4.48")]
[Description("GUI Shop Supports all known Currency, with NPC support - Re-Write Edition 2")]
public class GUIShop : RustPlugin
{
    [PluginReference]
     Plugin Economics;
     Plugin Kits;
     Plugin ImageLibrary;
     Plugin ServerRewards;
     Plugin LangAPI;
    private bool _isRestart;
    private bool _isShopReady;
    private bool _isLangAPIReady;
    private bool _isEconomicsLimits;
    private bool _isEconomicsDebt;
    private KeyValuePair<int, int> _balanceLimits;
    private Dictionary<string, string> _imageListGUIShop;
    private List<KeyValuePair<string, ulong>> _guishopItemIcons;
    private const string GUIShopOverlayName;
    private const string GUIShopContentName;
    private const string GUIShopDescOverlay;
    private const string GUIShopColorPicker;
    private const string BlockAllow;
    private const string Use;
    private const string Admin;
    private const string Color;
    private const string Button;
    private int _imageLibraryCheck;
    private Hash<ulong, int> _shopPage;
    private Dictionary<ulong, Dictionary<string, double>> _sellCoolDownData;
    private Dictionary<ulong, Dictionary<string, double>> _buyCooldownData;
    private Dictionary<ulong, Dictionary<string, double>> _buyLimitResetCoolDownData;
    private Dictionary<ulong, Dictionary<string, double>> _sellLimitResetCoolDownData;
    private Dictionary<string, ulong> _boughtData;
    private Dictionary<string, ulong> _soldData;
    private Dictionary<ulong, ItemLimit> _limitsData;
    readonly Dictionary<string, string> _headers;
    private List<MonumentInfo> _monuments { get; set; }
    private bool _configChanged;
    private int playersMask;
    private const string GUIShopWelcomeImage;
    private const string GUIShopBackgroundImage;
    private const string GUIShopAmount1Image;
    private const string GUIShopAmount2Image;
    private const string GUIShopBuyImage;
    private const string GUIShopSellImage;
    private const string GUIShopBackArrowImage;
    private const string GUIShopForwardArrowImage;
    private const string GUIShopCloseImage;
    private const string GUIShopShopButton;
    private HashSet<string> playerGUIShopUIOpen;
    private Dictionary<string, PlayerUISetting> _playerUIData;
    private string _uiSettingChange;
    private bool _imageChanger;
    private double Transparency;
    private PluginConfig _config;
    private static GUIShop _instance;
    private HashSet<string> _playerDisabledButtonData;
    private readonly Dictionary<string, string> _corrections;
    private readonly HashSet<string> _exclude;
    internal class PluginConfig : SerializableConfiguration
    {
        [JsonProperty("Carefully Edit This")]
        public CarefullyEdit Time;
        [JsonProperty("Wipe GUIShop Data files on Map Changes / Server Wipes & if server save file is deleted")]
        public bool AutoWipe;
        [JsonProperty("Sets the ImageLibrary Counter Check ( Set higher if needed for server restarts )")]
        public int ImageLibraryCounter;
        [JsonProperty("Enable Discord Buy Transaction Logging")]
        public bool EnableDiscordLogging;
        [JsonProperty("Enable Discord Sell Transaction Logging")]
        public bool EnableDiscordSellLogging;
        [JsonProperty("Discord Webhook URL")]
        public string DiscordWebHookURL;
        [JsonProperty("Discord Embed Color")]
        public string DiscordColor;
        [JsonProperty("Discord Author Image")]
        public string DiscordAuthorImage;
        [JsonProperty("Discord Embed Icon")]
        public string DiscordAuthorName;
        [JsonProperty("Set Default Global Shop to open")]
        public string DefaultShop;
        [JsonProperty("Sets shop command")]
        public string shopcommand;
        [JsonProperty("Sets Vehicle Spawn Distance")]
        public float SpawnDistance;
        [JsonProperty("Switches to Economics as default curency")]
        public bool Economics;
        [JsonProperty("Switches to ServerRewards as default curency")]
        public bool ServerRewards;
        [JsonProperty("Switches to Custom as default curency")]
        public bool CustomCurrency;
        [JsonProperty("Allow Custom Currency Sell Of Used Items")]
        public bool CustomCurrencyAllowSellOfUsedItems;
        [JsonProperty("Custom Currency Item ID")]
        public int CustomCurrencyID;
        [JsonProperty("Custom Currency Skin ID")]
        public ulong CustomCurrencySkinID;
        [JsonProperty("Custom Currency Name")]
        public string CustomCurrencyName;
        [JsonProperty("Allows you to specify which containers you can sell items from")]
        public InventoryTypes AllowedSellContainers;
        [JsonProperty("Enable Shop Buy All Button")]
        public bool BuyAllButton;
        [JsonProperty("Enable Shop Sell All Button")]
        public bool SellAllButton;
        [JsonProperty("Sets the buy/Sell button amounts + how many buttons")]
        public int[] steps;
        [JsonProperty("Player UI display")]
        public bool PersonalUI;
        [JsonProperty("Block Monuments")]
        public bool BlockMonuments;
        [JsonProperty("If true = Images, If False = Text Labels")]
        public bool UIImageOption;
        [JsonProperty("NPC Distance Check")]
        public float NPCDistanceCheck;
        [JsonProperty("Enable NPC Auto Open")]
        public bool NPCAutoOpen;
        [JsonProperty("Enable GUIShop NPC Msg's")]
        public bool NPCLeaveResponse;
        [JsonProperty("GUI Shop - Welcome MSG")]
        public string WelcomeMsg;
        [JsonProperty("Shop - Buy Price Label")]
        public string BuyLabel;
        [JsonProperty("Shop - Amount1 Label1")]
        public string AmountLabel;
        [JsonProperty("Shop - Sell $ Label")]
        public string SellLabel;
        [JsonProperty("Shop - Amount2 Label2")]
        public string AmountLabel2;
        [JsonProperty("Shop - Back Button Text")]
        public string BackButtonText;
        [JsonProperty("Shop - Forward Button Text")]
        public string ForwardButtonText;
        [JsonProperty("Shop - Close Label")]
        public string CloseButtonlabel;
        [JsonProperty("Shop - GUIShop Welcome Url")]
        public string GuiShopWelcomeUrl;
        [JsonProperty("Shop - GUIShop Enable Background Image")]
        public bool BackgroundImage;
        [JsonProperty("Shop - GUIShop Background Image Url")]
        public string BackgroundUrl;
        [JsonProperty("Shop - Sets any shop items to this image if image link does not exist.")]
        public string IconUrl;
        [JsonProperty("Shop - Shop Buy Icon Url")]
        public string BuyIconUrl;
        [JsonProperty("Shop - Shop Amount Left Url")]
        public string AmountUrl;
        [JsonProperty("Shop - Shop Amount Right Url")]
        public string AmountUrl2;
        [JsonProperty("Shop - Shop Sell Icon Url")]
        public string SellIconUrl;
        [JsonProperty("Shop - Shop Back Arrow Url")]
        public string BackButtonUrl;
        [JsonProperty("Shop - Shop Forward Arrow Url")]
        public string ForwardButtonUrl;
        [JsonProperty("Shop - Close Image Url")]
        public string CloseButton;
        [JsonProperty("Shop GUI Button")]
        public GUIButton GUI;
        [JsonProperty("GUIShop Configurable UI colors (First 8 Colors!)")]
        public HashSet<string> ColorsUI;
        [JsonProperty("Set Default Shop Buy Color")]
        public string BuyColor;
        [JsonProperty("Set Default Shop Sell Color")]
        public string SellColor;
        [JsonProperty("Set Default Shop Text Color")]
        public string TextColor;
        [JsonProperty("Was Saved Don't Touch!")]
        public bool WasSaved;
        [JsonProperty("Shop - Shop Categories")]
        public Dictionary<string, ShopCategory> ShopCategories;
        [JsonProperty("Shop - Shop List")]
        public Dictionary<string, ShopItem> ShopItems;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    public class ShopItem
    {
        public string DisplayName;
        public bool CraftAsDisplayName;
        public string Shortname;
        public int ItemId;
        public bool MakeBlueprint;
        public bool AllowSellOfUsedItems;
        public float Condition;
        public bool EnableBuy;
        public bool EnableSell;
        public string Image;
        public double SellPrice;
        public double BuyPrice;
        public int BuyCooldown;
        public int SellCooldown;
        public int[] BuyQuantity;
        public int[] SellQuantity;
        public int BuyLimit;
        public int BuyLimitResetCoolDown;
        public bool SwapLimitToQuantityBuyLimit;
        public int SellLimit;
        public int SellLimitResetCoolDown;
        public bool SwapLimitToQuantitySoldLimit;
        public string KitName;
        public List<string> Command;
        public bool RunCommandAndCustomShopItem;
        public List<char> GeneTypes;
        public ulong SkinId;
        public double GetSellPrice(string playerId);
        public double GetBuyPrice(string playerId);
    }

    public class ShopCategory
    {
        public string DisplayName;
        public string DisplayNameColor;
        public string Description;
        public string DescriptionColor;
        public string Permission;
        [JsonIgnore]
        public string PrefixPermission { get; set; }
        public string Currency;
        public bool CustomCurrencyAllowSellOfUsedItems;
        public string CustomCurrencyNames;
        public int CustomCurrencyIDs;
        public ulong CustomCurrencySkinIDs;
        public bool EnabledCategory;
        public bool EnableNPC;
        public string NPCId;
        public List<string> NpcIds;
        public HashSet<string> Items;
        [JsonIgnore]
        public HashSet<ShopItem> ShopItems;
    }

    public class ItemLimit
    {
        public Dictionary<string, int> BLimit;
        public Dictionary<string, int> SLimit;
        public bool CheckSellLimit(string item, int amount);
        public void IncrementSell(string item, int amount, bool toggle);
        public bool CheckBuyLimit(string item, int amount);
        public void IncrementBuy(string item, int amount, bool toggle);
    }

    private class PlayerUISetting
    {
        public double Transparency;
        public string SellBoxColors;
        public string BuyBoxColors;
        public string UITextColor;
        public double RangeValue;
        public bool ImageOrText;
        public string ShopKey;
    }

    public class GUIButton
    {
        [JsonProperty(PropertyName = "Image")]
        public string Image;
        [JsonProperty(PropertyName = "Background color (RGBA format)")]
        public string Color;
        [JsonProperty(PropertyName = "GUI Button Position")]
        public Position GUIButtonPosition;
        public class Position
        {
            [JsonProperty(PropertyName = "Anchors Min")]
            public string AnchorsMin;
            [JsonProperty(PropertyName = "Anchors Max")]
            public string AnchorsMax;
            [JsonProperty(PropertyName = "Offsets Min")]
            public string OffsetsMin;
            [JsonProperty(PropertyName = "Offsets Max")]
            public string OffsetsMax;
        }

    }

    public class CarefullyEdit
    {
        public string WipeTime;
        public string LastWipe;
        [JsonProperty("Sets time before shops can be used after the server wipes")]
        public float CanShopIn;
    }

    private void CheckConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    internal class SerializableConfiguration
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
    private void LoadData();
    private void SaveData();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id);
    private string Lang(string key, string id, object[] args);
     RequestQueue _requestQueue;
    private void Enqueue(Action action);
    private class RequestQueue
    {
        readonly Queue<Action> _actions;
        readonly Timer _timer;
        public RequestQueue(float interval);
        public void Enqueue(Action action);
        public void Destroy();
        public void Check();
    }

    private class Request
    {
        public Dictionary<string, string> Headers;
        public Message Message;
        public Plugin Plugin;
        public string Webhook;
        public int Retries;
        public Request();
        public Request(string webhook, Message message, Plugin plugin, Dictionary<string, string> headers);
        public void SendRequest();
        public void RequestCallback(int code, string message);
    }

    private class Response
    {
        public int Code;
        public string Message;
        public bool IsLimited { get; set; }
        public bool IsOk { get; set; }
    }

    private class Message
    {
        [JsonProperty("content")]
        public string Content;
        [JsonProperty("embeds")]
        public List<Embed> Embeds;
        public Message();
        public Message(string content);
        public Message AddContent(string content);
        public Message AddEmbed(Embed embed);
        public string ToJson();
    }

    private class Author
    {
        [JsonProperty("icon_url")]
        public string IconUrl;
        [JsonProperty("name")]
        public string Name;
        [JsonProperty("url")]
        public string Url;
    }

    private class Embed
    {
        [JsonProperty("author")]
        public Author Author;
        [JsonProperty("title")]
        public string Title;
        [JsonProperty("description")]
        public string Description;
        [JsonProperty("color")]
        public int Color;
        [JsonProperty("fields")]
        public List<Field> Fields;
        public Embed AddTitle(string title);
        public Embed AddDescription(string description);
        public Embed AddColor(string color);
        public Embed AddField(string name, string value, bool inline);
        public Embed AddAuthor(string name, string url, string iconUrl);
    }

    private class Field
    {
        public Field(string name, string value, bool inline);
        [JsonProperty("name")]
        public string Name;
        [JsonProperty("value")]
        public string Value;
        [JsonProperty("inline")]
        public bool Inline;
    }

    private void SendToDiscord(List<Embed> embeds);
    private Embed BuildMessage(BasePlayer player);
    private bool _wipeReady;
    private void OnNewSave(string filename);
    readonly Dictionary<ulong, string> _customSpawnables;
    private void OnUserConnected(IPlayer player);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void SpawnReplacementItem(BaseEntity entity, string prefabPath);
    private void OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private string Currency;
    private Dictionary<int, string[]> _categoryRects;
    private Dictionary<int, float[]> _itemRects;
    private void CacheRects();
    private void OnServerInitialized();
    private DynamicConfigFile _buyCoolDowns;
    private DynamicConfigFile _sellCoolDowns;
    private DynamicConfigFile _buyLimitResetCoolDowns;
    private DynamicConfigFile _sellLimitResetCoolDowns;
    private DynamicConfigFile _bought;
    private DynamicConfigFile _sold;
    private DynamicConfigFile _limits;
    private DynamicConfigFile _playerData;
    private DynamicConfigFile _buttonData;
    private void Loaded();
    private void Unload();
    private void OnGroupPermissionGranted(string group, string perm);
    private void OnGroupPermissionRevoked(string group, string perm);
    private void OnUserPermissionGranted(string id, string permName);
    private void OnUserPermissionRevoked(string userId, string perm);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPluginLoaded(Plugin name);
    private void OnServerSave();
    private void LibraryCheck();
    private void LoadImages();
    private void ShopReady();
    private void OnLangAPIFinished();
    private CuiElementContainer CreateGUIShopOverlay(BasePlayer player, bool toggle);
    private readonly CuiLabel _guishopDescription;
    private CuiElementContainer CreateGUIShopItemEntry(ShopItem shopItem, float ymax, float ymin, string shopKey, string color, bool pricebuttons, bool cooldown, BasePlayer player);
    private void CreateGUIShopItemIcon(CuiElementContainer container, string item, float ymax, float ymin, ShopItem data, BasePlayer player);
    private void CreateGUIShopColorChanger(CuiElementContainer container, string shopKey, BasePlayer player, bool toggle);
    private void CreateGUIShopChangePage(CuiElementContainer container, string shopKey, int minus, int plus, BasePlayer player, bool toggle);
    private void CreateShopButton(CuiElementContainer container, ShopCategory shopCategory, int minus, int rowPos, BasePlayer player);
    private void DestroyUi(BasePlayer player, bool full);
    private void CreateGUIShopButton(BasePlayer player);
    private void DestroyGUIShopButton(BasePlayer player);
    private int GetAmount(PlayerInventory inventory, InventoryTypes type, int itemid, string display, bool used, ulong skinId);
    private int GetAmount(ItemContainer container, int itemid, string display, bool used, ulong skinId);
    private double GetCurrency(ShopCategory shop, BasePlayer player);
    private int Take(PlayerInventory inventory, InventoryTypes type, List<Item> collect, int itemid, int amount, bool used, string display, ulong skinId);
    private int Take(ItemContainer container, List<Item> collect, int itemid, int iAmount, bool used, string display, ulong skinId);
    private int GetAmountPrice(double amount);
    private bool TakeCurrency(ShopCategory shop, BasePlayer player, double amount);
    private void AddCurrency(ShopCategory shop, BasePlayer player, double amount, string item);
    private void ShowGUIShops(BasePlayer player, string shopKey, int from, bool fullPaint, bool refreshFunds);
    private void GetCategoryItems(ShopCategory category);
    private string CheckAction(BasePlayer player, string shopKey, string item, string ttype);
    private string IsShop(BasePlayer player, string shopKey);
    private int newamounts;
    private string TryShopBuy(BasePlayer player, string shopKey, string item, int amount, bool isAll);
    private string CanBuy(BasePlayer player, ShopCategory shopCategory, string item, int amount, bool isAll);
    private int GetAmountBuy(BasePlayer player, ShopCategory shopCategory, string item);
    private string TryGive(BasePlayer player, ShopCategory shopCategory, string item, int amount, double purchase);
    private string GiveItem(BasePlayer player, ShopItem data, int amount, ShopCategory shopCategory);
    public GrowableGenes Genes;
     GrowableGenetics.GeneType CharToGeneType(char h);
    public void EncodeGenesToItem(GrowableGenes sourceGrowable, Item targetItem);
    private static Item CreateByName(string strName, int iAmount, string name, float con, ulong skin);
    private static Item CreateByItemID(int itemID, int iAmount, string name, float con, ulong skin);
    private static Item Create(ItemDefinition template, int iAmount, string name, float con, ulong skin);
    private static void TrySkinChangeItem(ItemDefinition template, ulong skinId);
    private static bool GiveItem(BasePlayer player, Item item, ItemContainer container);
    protected static void GetIdealPickupContainer(BasePlayer player, Item item, ItemContainer container, int position);
    private int FreeSlots(BasePlayer player);
    private List<int> GetStacks(ItemDefinition items, int amount);
    private string TryShopSell(BasePlayer player, string shopKey, string item, int amount);
    private string TrySell(BasePlayer player, string shopKey, string item, int amount);
    private int GetAmountSell(BasePlayer player, string item);
    private string TakeItem(BasePlayer player, ShopItem data, ShopCategory shopCategory, int amount);
    private string CanSell(BasePlayer player, string item, int amount);
    public string StripTags(string input);
    private void CmdGUIShop(BasePlayer player, string command, string[] args);
    [ChatCommand("cleardata")]
    private void CmdGUIShopClearData(BasePlayer player, string command, string[] args);
    [ChatCommand("shopgui")]
    private void CmdGUIShopToggleBackpackGUI(BasePlayer player, string cmd, string[] args);
    [ChatCommand("update")]
    private void CmdGUIShopUpdate(BasePlayer player, string cmd, string[] args);
    [ChatCommand("updateold")]
    private void Update(BasePlayer player, string cmd, string[] args);
    [ChatCommand("resetconditions")]
    private void CmdGUIShopResetConditions(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("limits.clear")]
    private void ConsoleGUIShopClearLimit(ConsoleSystem.Arg arg);
    [ConsoleCommand("transactions.clear")]
    private void ConsoleGUIShopClearTransactions(ConsoleSystem.Arg arg);
    [ConsoleCommand("shop.button")]
    private void ConsoleGUIShopButton(ConsoleSystem.Arg arg);
    [ConsoleCommand("shop.show")]
    private void ConsoleGUIShopShow(ConsoleSystem.Arg arg);
    [ConsoleCommand("shop.buy")]
    private void ConsoleGUIShopBuy(ConsoleSystem.Arg arg, ShopCategory shopCategory);
    private string GetItemDisplayName(string shorname, string displayName, string userID);
    [ConsoleCommand("shop.sell")]
    private void ConsoleGUIShopSell(ConsoleSystem.Arg arg);
    [ConsoleCommand("shop.transparency")]
    private void ConsoleGUIShopTransparency(ConsoleSystem.Arg arg);
    [ConsoleCommand("shop.uicolor")]
    private void ConsoleGUIShopUIColor(ConsoleSystem.Arg arg);
    [ConsoleCommand("shop.colorsetting")]
    private void ConsoleGUIShopUIColorSetting(ConsoleSystem.Arg arg);
    [ConsoleCommand("shop.imageortext")]
    private void ConsoleGUIShopUIImageOrText(ConsoleSystem.Arg arg);
    [ConsoleCommand(("guishop.reset"))]
    private void ConsoleGUIShopReset(ConsoleSystem.Arg arg);
    private int CurrentTime();
    private bool HasBuyCooldown(ulong userID, string item, double itemCooldown);
    private bool HasSellCooldown(ulong userID, string item, double itemCooldown);
    private bool HasBuyLimitCoolDown(ulong userID, string item, double itemCooldown);
    private bool HasSellLimitCoolDown(ulong userID, string item, double itemCooldown);
    private string FormatTime(long seconds);
    private string GetSettingTypeToChange(string type);
    private void SetImageOrText(BasePlayer player);
    private bool GetImageOrText(BasePlayer player);
    private string GetText(string text, string type, BasePlayer player);
    private double AnchorBarMath(BasePlayer uiPlayer);
    private PlayerUISetting GetPlayerData(BasePlayer player);
    private void PlayerTransparencyChange(BasePlayer uiPlayer, string action);
    private double GetUITransparency(BasePlayer uiPlayer);
    private void PlayerColorTextChange(BasePlayer uiPlayer, string textColorRed, string textColorGreen, string textColorBlue, string uiSettingToChange);
    private string GetUITextColor(BasePlayer uiPlayer, string defaultHex);
    private string GetUISellBoxColor(BasePlayer uiPlayer);
    private string GetUIBuyBoxColor(BasePlayer uiPlayer);
    private string HexToColor(string hexString);
    private ItemLimit GetPlayerLimit(BasePlayer player);
    private bool NearNpc(string npcId, BasePlayer player);
    private BasePlayer GetNPC(string npcId);
     bool GetNearestVendor(BasePlayer player, ShopCategory shopCategory);
     bool IsPlayer(ulong userID);
    private void OnUseNPC(BasePlayer npc, BasePlayer player);
    private void OnEnterNPC(BasePlayer npc, BasePlayer player);
    private void OnLeaveNPC(BasePlayer npc, BasePlayer player);
    private Vector3 GetLookPoint(BasePlayer player);
    private bool IsNearMonument(BasePlayer player);
    private double GetCurrency(BasePlayer player, string shopKey);
    private void OpenShop(BasePlayer player, string shopKey, string npcID);
    private void CloseShop(BasePlayer player);
}

internal class PluginConfig : SerializableConfiguration
{
    [JsonProperty("Carefully Edit This")]
    public CarefullyEdit Time;
    [JsonProperty("Wipe GUIShop Data files on Map Changes / Server Wipes & if server save file is deleted")]
    public bool AutoWipe;
    [JsonProperty("Sets the ImageLibrary Counter Check ( Set higher if needed for server restarts )")]
    public int ImageLibraryCounter;
    [JsonProperty("Enable Discord Buy Transaction Logging")]
    public bool EnableDiscordLogging;
    [JsonProperty("Enable Discord Sell Transaction Logging")]
    public bool EnableDiscordSellLogging;
    [JsonProperty("Discord Webhook URL")]
    public string DiscordWebHookURL;
    [JsonProperty("Discord Embed Color")]
    public string DiscordColor;
    [JsonProperty("Discord Author Image")]
    public string DiscordAuthorImage;
    [JsonProperty("Discord Embed Icon")]
    public string DiscordAuthorName;
    [JsonProperty("Set Default Global Shop to open")]
    public string DefaultShop;
    [JsonProperty("Sets shop command")]
    public string shopcommand;
    [JsonProperty("Sets Vehicle Spawn Distance")]
    public float SpawnDistance;
    [JsonProperty("Switches to Economics as default curency")]
    public bool Economics;
    [JsonProperty("Switches to ServerRewards as default curency")]
    public bool ServerRewards;
    [JsonProperty("Switches to Custom as default curency")]
    public bool CustomCurrency;
    [JsonProperty("Allow Custom Currency Sell Of Used Items")]
    public bool CustomCurrencyAllowSellOfUsedItems;
    [JsonProperty("Custom Currency Item ID")]
    public int CustomCurrencyID;
    [JsonProperty("Custom Currency Skin ID")]
    public ulong CustomCurrencySkinID;
    [JsonProperty("Custom Currency Name")]
    public string CustomCurrencyName;
    [JsonProperty("Allows you to specify which containers you can sell items from")]
    public InventoryTypes AllowedSellContainers;
    [JsonProperty("Enable Shop Buy All Button")]
    public bool BuyAllButton;
    [JsonProperty("Enable Shop Sell All Button")]
    public bool SellAllButton;
    [JsonProperty("Sets the buy/Sell button amounts + how many buttons")]
    public int[] steps;
    [JsonProperty("Player UI display")]
    public bool PersonalUI;
    [JsonProperty("Block Monuments")]
    public bool BlockMonuments;
    [JsonProperty("If true = Images, If False = Text Labels")]
    public bool UIImageOption;
    [JsonProperty("NPC Distance Check")]
    public float NPCDistanceCheck;
    [JsonProperty("Enable NPC Auto Open")]
    public bool NPCAutoOpen;
    [JsonProperty("Enable GUIShop NPC Msg's")]
    public bool NPCLeaveResponse;
    [JsonProperty("GUI Shop - Welcome MSG")]
    public string WelcomeMsg;
    [JsonProperty("Shop - Buy Price Label")]
    public string BuyLabel;
    [JsonProperty("Shop - Amount1 Label1")]
    public string AmountLabel;
    [JsonProperty("Shop - Sell $ Label")]
    public string SellLabel;
    [JsonProperty("Shop - Amount2 Label2")]
    public string AmountLabel2;
    [JsonProperty("Shop - Back Button Text")]
    public string BackButtonText;
    [JsonProperty("Shop - Forward Button Text")]
    public string ForwardButtonText;
    [JsonProperty("Shop - Close Label")]
    public string CloseButtonlabel;
    [JsonProperty("Shop - GUIShop Welcome Url")]
    public string GuiShopWelcomeUrl;
    [JsonProperty("Shop - GUIShop Enable Background Image")]
    public bool BackgroundImage;
    [JsonProperty("Shop - GUIShop Background Image Url")]
    public string BackgroundUrl;
    [JsonProperty("Shop - Sets any shop items to this image if image link does not exist.")]
    public string IconUrl;
    [JsonProperty("Shop - Shop Buy Icon Url")]
    public string BuyIconUrl;
    [JsonProperty("Shop - Shop Amount Left Url")]
    public string AmountUrl;
    [JsonProperty("Shop - Shop Amount Right Url")]
    public string AmountUrl2;
    [JsonProperty("Shop - Shop Sell Icon Url")]
    public string SellIconUrl;
    [JsonProperty("Shop - Shop Back Arrow Url")]
    public string BackButtonUrl;
    [JsonProperty("Shop - Shop Forward Arrow Url")]
    public string ForwardButtonUrl;
    [JsonProperty("Shop - Close Image Url")]
    public string CloseButton;
    [JsonProperty("Shop GUI Button")]
    public GUIButton GUI;
    [JsonProperty("GUIShop Configurable UI colors (First 8 Colors!)")]
    public HashSet<string> ColorsUI;
    [JsonProperty("Set Default Shop Buy Color")]
    public string BuyColor;
    [JsonProperty("Set Default Shop Sell Color")]
    public string SellColor;
    [JsonProperty("Set Default Shop Text Color")]
    public string TextColor;
    [JsonProperty("Was Saved Don't Touch!")]
    public bool WasSaved;
    [JsonProperty("Shop - Shop Categories")]
    public Dictionary<string, ShopCategory> ShopCategories;
    [JsonProperty("Shop - Shop List")]
    public Dictionary<string, ShopItem> ShopItems;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class ShopItem
{
    public string DisplayName;
    public bool CraftAsDisplayName;
    public string Shortname;
    public int ItemId;
    public bool MakeBlueprint;
    public bool AllowSellOfUsedItems;
    public float Condition;
    public bool EnableBuy;
    public bool EnableSell;
    public string Image;
    public double SellPrice;
    public double BuyPrice;
    public int BuyCooldown;
    public int SellCooldown;
    public int[] BuyQuantity;
    public int[] SellQuantity;
    public int BuyLimit;
    public int BuyLimitResetCoolDown;
    public bool SwapLimitToQuantityBuyLimit;
    public int SellLimit;
    public int SellLimitResetCoolDown;
    public bool SwapLimitToQuantitySoldLimit;
    public string KitName;
    public List<string> Command;
    public bool RunCommandAndCustomShopItem;
    public List<char> GeneTypes;
    public ulong SkinId;
    public double GetSellPrice(string playerId);
    public double GetBuyPrice(string playerId);
}

public class ShopCategory
{
    public string DisplayName;
    public string DisplayNameColor;
    public string Description;
    public string DescriptionColor;
    public string Permission;
    [JsonIgnore]
    public string PrefixPermission { get; set; }
    public string Currency;
    public bool CustomCurrencyAllowSellOfUsedItems;
    public string CustomCurrencyNames;
    public int CustomCurrencyIDs;
    public ulong CustomCurrencySkinIDs;
    public bool EnabledCategory;
    public bool EnableNPC;
    public string NPCId;
    public List<string> NpcIds;
    public HashSet<string> Items;
    [JsonIgnore]
    public HashSet<ShopItem> ShopItems;
}

public class ItemLimit
{
    public Dictionary<string, int> BLimit;
    public Dictionary<string, int> SLimit;
    public bool CheckSellLimit(string item, int amount);
    public void IncrementSell(string item, int amount, bool toggle);
    public bool CheckBuyLimit(string item, int amount);
    public void IncrementBuy(string item, int amount, bool toggle);
}

private class PlayerUISetting
{
    public double Transparency;
    public string SellBoxColors;
    public string BuyBoxColors;
    public string UITextColor;
    public double RangeValue;
    public bool ImageOrText;
    public string ShopKey;
}

public class GUIButton
{
    [JsonProperty(PropertyName = "Image")]
    public string Image;
    [JsonProperty(PropertyName = "Background color (RGBA format)")]
    public string Color;
    [JsonProperty(PropertyName = "GUI Button Position")]
    public Position GUIButtonPosition;
    public class Position
    {
        [JsonProperty(PropertyName = "Anchors Min")]
        public string AnchorsMin;
        [JsonProperty(PropertyName = "Anchors Max")]
        public string AnchorsMax;
        [JsonProperty(PropertyName = "Offsets Min")]
        public string OffsetsMin;
        [JsonProperty(PropertyName = "Offsets Max")]
        public string OffsetsMax;
    }

}

public class Position
{
    [JsonProperty(PropertyName = "Anchors Min")]
    public string AnchorsMin;
    [JsonProperty(PropertyName = "Anchors Max")]
    public string AnchorsMax;
    [JsonProperty(PropertyName = "Offsets Min")]
    public string OffsetsMin;
    [JsonProperty(PropertyName = "Offsets Max")]
    public string OffsetsMax;
}

public class CarefullyEdit
{
    public string WipeTime;
    public string LastWipe;
    [JsonProperty("Sets time before shops can be used after the server wipes")]
    public float CanShopIn;
}

internal class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private class RequestQueue
{
    readonly Queue<Action> _actions;
    readonly Timer _timer;
    public RequestQueue(float interval);
    public void Enqueue(Action action);
    public void Destroy();
    public void Check();
}

private class Request
{
    public Dictionary<string, string> Headers;
    public Message Message;
    public Plugin Plugin;
    public string Webhook;
    public int Retries;
    public Request();
    public Request(string webhook, Message message, Plugin plugin, Dictionary<string, string> headers);
    public void SendRequest();
    public void RequestCallback(int code, string message);
}

private class Response
{
    public int Code;
    public string Message;
    public bool IsLimited { get; set; }
    public bool IsOk { get; set; }
}

private class Message
{
    [JsonProperty("content")]
    public string Content;
    [JsonProperty("embeds")]
    public List<Embed> Embeds;
    public Message();
    public Message(string content);
    public Message AddContent(string content);
    public Message AddEmbed(Embed embed);
    public string ToJson();
}

private class Author
{
    [JsonProperty("icon_url")]
    public string IconUrl;
    [JsonProperty("name")]
    public string Name;
    [JsonProperty("url")]
    public string Url;
}

private class Embed
{
    [JsonProperty("author")]
    public Author Author;
    [JsonProperty("title")]
    public string Title;
    [JsonProperty("description")]
    public string Description;
    [JsonProperty("color")]
    public int Color;
    [JsonProperty("fields")]
    public List<Field> Fields;
    public Embed AddTitle(string title);
    public Embed AddDescription(string description);
    public Embed AddColor(string color);
    public Embed AddField(string name, string value, bool inline);
    public Embed AddAuthor(string name, string url, string iconUrl);
}

private class Field
{
    public Field(string name, string value, bool inline);
    [JsonProperty("name")]
    public string Name;
    [JsonProperty("value")]
    public string Value;
    [JsonProperty("inline")]
    public bool Inline;
}


```

---

## GunGame by k1lly0u - GunGame event for Event Manager

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Plugins.EventManagerEx;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("GunGame", "k1lly0u", "0.5.1"), Description("GunGame event mode for EventManager")]
 class GunGame : RustPlugin, IEventPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    private string[] _validWeapons;
    private static Func<string, StoredData.WeaponSet> GetWeaponSet;
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void Unload();
    private void FindValidWeapons();
    private string[] GetGunGameWeapons();
    private string[] GetGunGameWeaponSets();
    public bool InitializeEvent(EventManager.EventConfig config);
    public bool CanUseClassSelector { get; set; }
    public bool RequireTimeLimit { get; set; }
    public bool RequireScoreLimit { get; set; }
    public bool UseScoreLimit { get; set; }
    public bool UseTimeLimit { get; set; }
    public bool IsTeamEvent { get; set; }
    public void FormatScoreEntry(EventManager.ScoreEntry scoreEntry, ulong langUserId, string score1, string score2);
    public List<EventManager.EventParameter> AdditionalParameters { get; set; }
    public string ParameterIsValid(string fieldName, object value);
    public class GunGameEvent : EventManager.BaseEventGame
    {
        private StoredData.WeaponSet weaponSet;
        private ItemDefinition downgradeWeapon;
        public EventManager.BaseEventPlayer winner;
        internal override void InitializeEvent(IEventPlugin plugin, EventManager.EventConfig config);
        internal override void PrestartEvent();
        protected override EventManager.BaseEventPlayer AddPlayerComponent(BasePlayer player);
        protected override void OnKitGiven(EventManager.BaseEventPlayer eventPlayer);
        internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo hitInfo);
        protected override bool CanDropBackpack();
        private string GetWeaponShortname(HitInfo hitInfo);
        private bool KilledByRankedWeapon(GunGamePlayer attacker, string weapon);
        private bool KilledByDowngradeWeapon(string weapon);
        protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
        protected override void BuildScoreboard();
        protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override void SortScores(List<EventManager.ScoreEntry> list);
        internal override void GetAdditionalEventDetails(List<KeyValuePair<string, object>> list, ulong playerId);
    }

    private class GunGamePlayer : EventManager.BaseEventPlayer
    {
        public int Rank { get; set; }
        public Item RankWeapon { get; set; }
        public void RemoveRankWeapon();
        public void GiveRankWeapon(Item item);
    }

    private Hash<ulong, StoredData.WeaponSet> setCreator;
    [ChatCommand("ggset")]
    private void cmdGunGameSet(BasePlayer player, string command, string[] args);
    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Respawn time (seconds)")]
        public int RespawnTime { get; set; }
        [JsonProperty(PropertyName = "Reset heath when killing an enemy")]
        public bool ResetHealthOnKill { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private void SaveData();
    private void LoadData();
    private class StoredData
    {
        public Hash<string, WeaponSet> _weaponSets;
        public bool TryFindWeaponSet(string name, WeaponSet weaponSet);
        public WeaponSet GetWeaponSet(string name);
        public class WeaponSet
        {
            public List<EventManager.ItemData> weapons;
            public int Count { get; set; }
            public Item CreateItem(int rank);
            public int AddItem(Item item, int index);
        }

    }

    public string Message(string key, ulong playerId);
    private static Func<string, ulong, string> GetMessage;
    private readonly Dictionary<string, string> Messages;
}

public class GunGameEvent : EventManager.BaseEventGame
{
    private StoredData.WeaponSet weaponSet;
    private ItemDefinition downgradeWeapon;
    public EventManager.BaseEventPlayer winner;
    internal override void InitializeEvent(IEventPlugin plugin, EventManager.EventConfig config);
    internal override void PrestartEvent();
    protected override EventManager.BaseEventPlayer AddPlayerComponent(BasePlayer player);
    protected override void OnKitGiven(EventManager.BaseEventPlayer eventPlayer);
    internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo hitInfo);
    protected override bool CanDropBackpack();
    private string GetWeaponShortname(HitInfo hitInfo);
    private bool KilledByRankedWeapon(GunGamePlayer attacker, string weapon);
    private bool KilledByDowngradeWeapon(string weapon);
    protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
    protected override void BuildScoreboard();
    protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override void SortScores(List<EventManager.ScoreEntry> list);
    internal override void GetAdditionalEventDetails(List<KeyValuePair<string, object>> list, ulong playerId);
}

private class GunGamePlayer : EventManager.BaseEventPlayer
{
    public int Rank { get; set; }
    public Item RankWeapon { get; set; }
    public void RemoveRankWeapon();
    public void GiveRankWeapon(Item item);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Respawn time (seconds)")]
    public int RespawnTime { get; set; }
    [JsonProperty(PropertyName = "Reset heath when killing an enemy")]
    public bool ResetHealthOnKill { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}

private class StoredData
{
    public Hash<string, WeaponSet> _weaponSets;
    public bool TryFindWeaponSet(string name, WeaponSet weaponSet);
    public WeaponSet GetWeaponSet(string name);
    public class WeaponSet
    {
        public List<EventManager.ItemData> weapons;
        public int Count { get; set; }
        public Item CreateItem(int rank);
        public int AddItem(Item item, int index);
    }

}

public class WeaponSet
{
    public List<EventManager.ItemData> weapons;
    public int Count { get; set; }
    public Item CreateItem(int rank);
    public int AddItem(Item item, int index);
}


```

---

## GunStats by VisEntities - Tracks your weapon use history, including kills and hits, and displays it on the weapon name

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Gun Stats", "VisEntities", "2.2.0")]
[Description("Tracks your weapon use history, including kills and hits, and displays it on the weapon name.")]
public class GunStats : RustPlugin
{
    private static GunStats _plugin;
    private static Configuration _config;
    private Dictionary<ulong, WeaponStats> _weaponStats;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Weapon Name Format")]
        public string WeaponNameFormat { get; set; }
        [JsonProperty("Include NPC In Stats")]
        public bool IncludeNPCInStats { get; set; }
        [JsonProperty("Excluded Item Short Names")]
        public List<string> ExcludedItemShortNames { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private class WeaponStats
    {
        public int Kills { get; set; }
        public int ShotsFired { get; set; }
        public int ShotsHit { get; set; }
    }

    private void Init();
    private void Unload();
    private void OnPlayerDeath(BasePlayer victim, HitInfo hitInfo);
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player);
    private void OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
    private void UpdateWeaponStats(Item weapon, int addKills, int addShotsHit, int addShotsFired);
    private void UpdateWeaponName(Item weapon, WeaponStats stats);
    private static class PermissionUtil
    {
        public const string USE;
        public static void RegisterPermissions();
        public static bool VerifyHasPermission(BasePlayer player, string permissionName);
    }

}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Weapon Name Format")]
    public string WeaponNameFormat { get; set; }
    [JsonProperty("Include NPC In Stats")]
    public bool IncludeNPCInStats { get; set; }
    [JsonProperty("Excluded Item Short Names")]
    public List<string> ExcludedItemShortNames { get; set; }
}

private class WeaponStats
{
    public int Kills { get; set; }
    public int ShotsFired { get; set; }
    public int ShotsHit { get; set; }
}

private static class PermissionUtil
{
    public const string USE;
    public static void RegisterPermissions();
    public static bool VerifyHasPermission(BasePlayer player, string permissionName);
}


```

---

## Gyrocopter by ColonBlow - Allows players to fly there very own scrap build gyrocopter

```csharp
using Rust;
using System;
using System.Linq;
using System.Collections;
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Gyrocopter", "ColonBlow", "1.2.11")]
[Description("Allows players to fly there very own scrap build gyrocopter")]
 class Gyrocopter : RustPlugin
{
    static LayerMask layerMask;
    private BaseEntity newCopter;
    static Dictionary<ulong, PlayerCopterData> loadplayer;
    static List<ulong> pilotslist;
    public class PlayerCopterData
    {
        public BasePlayer player;
        public int coptercount;
    }

    private void Init();
    private bool isAllowed(BasePlayer player, string perm);
    private bool UseMaxCopterChecks;
    public int maxcopters;
    static float MinAltitude;
    static float RechargeRange;
    static int NormalCost;
    static int SprintCost;
    static int BonusRechargeRate;
    static int BaseRechargeRate;
    static float NormalSpeed;
    static float SprintSpeed;
    static float bombdamageradius;
    static float bombdamage;
    static bool enablebombs;
    static bool enablestoragebox;
    static bool enablepassstoragebox;
    private bool OwnerLockPaint;
    private bool Changed;
    private void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void LoadVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private Dictionary<string, string> messages;
    [ChatCommand("copterhelp")]
    private void chatCopterHelp(BasePlayer player, string command, string[] args);
    [ChatCommand("copterbuild")]
    private void chatBuildCopter(BasePlayer player, string command, string[] args);
    [ConsoleCommand("copterbuild")]
    private void cmdConsoleBuildCopter(ConsoleSystem.Arg arg);
    [ChatCommand("copterdropnet")]
    private void chatDropNetCopter(BasePlayer player, string command, string[] args);
    [ChatCommand("copterlockpaint")]
    private void chatCopterLockPaint(BasePlayer player, string command, string[] args);
    [ChatCommand("copterunlockpaint")]
    private void chatCopterUnLockPaint(BasePlayer player, string command, string[] args);
    private void AddCopter(BasePlayer player, Vector3 location);
    [ChatCommand("copterswag")]
    private void chatGetCopterSwag(BasePlayer player, string command, string[] args);
    [ChatCommand("coptercount")]
    private void cmdChatCopterCount(BasePlayer player, string command, string[] args);
    [ChatCommand("copterdestroy")]
    private void cmdChatCopterDestroy(BasePlayer player, string command, string[] args);
    public bool PilotListContainsPlayer(BasePlayer player);
    private void AddPlayerToPilotsList(BasePlayer player);
    public void RemovePlayerFromPilotsList(BasePlayer player);
    private void DestroyLocalCopter(BasePlayer player);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private object OnEntityGroundMissing(BaseEntity entity);
    private void OnSpinWheel(BasePlayer player, SpinnerWheel wheel);
    private bool CopterLimitReached(BasePlayer player);
    private object CanDismountEntity(BasePlayer player, BaseMountable entity);
    private void OnEntityMounted(BaseMountable mountable, BasePlayer player);
    private void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private void AddPlayerID(ulong ownerid);
    private void RemovePlayerID(ulong ownerid);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void RemoveCopter(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerRespawned(BasePlayer player);
    private void DestroyAll();
    private void Unload();
    private static List<BasePlayer> copterantihack;
    private object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
    private class GyroCopter : BaseEntity
    {
        public BaseEntity entity;
        public BasePlayer player;
        public BaseEntity wheel;
        public BaseEntity deck1;
        public BaseEntity deck2;
        public BaseEntity barrel;
        public BaseEntity barcenter;
        public BaseEntity rotor1;
        public BaseEntity rotor2;
        public BaseEntity rotor3;
        public BaseEntity rotor4;
        public BaseEntity skid1;
        public BaseEntity skid2;
        public BaseEntity skid3;
        public BaseEntity skid4;
        public BaseEntity skidsupr;
        public BaseEntity skidsupl;
        public BaseEntity tailrotor1;
        public BaseEntity tailrotor2;
        public BaseEntity floor;
        public BaseEntity nosesign;
        public BaseEntity tail1;
        public BaseEntity tail2;
        public BaseEntity passengerchair1;
        public BaseEntity passengerchair2;
        public BaseEntity copterlock;
        public BaseEntity panel;
        public BaseEntity dabomb;
        private BaseEntity fseat;
        private BaseEntity fseatb;
        private BaseEntity fseatl;
        private BaseEntity fseatr;
        private BaseEntity rseat;
        private BaseEntity rseatb;
        private BaseEntity rseatl;
        private BaseEntity rseatr;
        private BaseEntity lseat;
        private BaseEntity lseatb;
        private BaseEntity lseatl;
        private BaseEntity lseatr;
        public BaseEntity pilotstorage;
        public BaseEntity passstorage1;
        public BaseEntity passstorage2;
        private Quaternion entityrot;
        private Vector3 entitypos;
        public bool moveforward;
        public bool movebackward;
        public bool moveup;
        public bool movedown;
        public bool rotright;
        public bool rotleft;
        public bool sprinting;
        public bool islanding;
        public bool hasbonuscharge;
        public bool paintingsarelocked;
        public ulong ownerid;
        private int count;
        public bool engineon;
        private float minaltitude;
        private Gyrocopter instance;
        public bool throttleup;
        private int sprintcost;
        private int normalcost;
        private float sprintspeed;
        private float normalspeed;
        public int currentfuel;
        private int baserechargerate;
        private int bonusrechargerate;
        private bool isenabled;
        private bool hasdabomb;
        private SphereCollider sphereCollider;
        private string prefabdeck;
        private string prefabbar;
        private string prefabrotor;
        private string prefabbarrel;
        private string prefabbomb;
        private string prefabfloor;
        private string prefabnosesign;
        private string prefabpanel;
        private string prefabskid;
        private string wheelprefab;
        private string copterlockprefab;
        private string prefabchair;
        private string prefabnetting;
        private string prefabdabomb;
         void Awake();
        public void SetBaseAltitude();
        private BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, BaseEntity parent, ulong skinid);
        private void SpawnRefresh(BaseEntity entity);
        public void SpawnCopter();
        private void SpawnFuelBox();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        public void DropNet();
        public void LockPaintings();
        public void UnLockPaintings();
        private BasePlayer GetPilot();
        private void FuelCheck();
        public void CopterInput(InputState input, BasePlayer player);
        public void UseDaBombs(BasePlayer player);
        public void ReloadDaBombs(BasePlayer player);
        public void FindMoreDaBombs(BasePlayer player);
        private void FixedUpdate();
        private IEnumerator RefreshCopter();
        private void ResetMovement();
        public void OnDestroy();
    }

    private class DaBomb : MonoBehaviour
    {
        private BaseEntity entity;
        private GyroCopter copter;
        private Vector3 entitypos;
        private Quaternion entityrot;
        private BaseEntity dabomb;
        private bool onGround;
        private float damageradius;
        private float damageamount;
        private void Awake();
        private void ImpactDamage(Vector3 hitpos);
        private void ImpactFX(Vector3 pos);
        private void SpawnFireEffects();
        private void FixedUpdate();
        private void OnDestroy();
    }

    private class CopterNet : MonoBehaviour
    {
        public BaseEntity netting1;
        public BaseEntity netting2;
        public BaseEntity netting3;
        private BaseEntity entity;
        private GyroCopter copter;
        private Vector3 entitypos;
        private Quaternion entityrot;
        private void Awake();
        private void RefreshNetting();
        private void FixedUpdate();
        private void OnDestroy();
    }

    private class FuelControl : MonoBehaviour
    {
        private BasePlayer player;
        private GyroCopter copter;
        public string anchormaxstr;
        public string colorstr;
        private Vector3 playerpos;
        private Gyrocopter instance;
        private bool ischarging;
        private int count;
        private float rechargerange;
        private int rechargerate;
        private void Awake();
        private void Recharge();
        private void ChargingFX();
        private void FixedUpdate();
        public void RechargeIndicator(BasePlayer player);
        public void fuelIndicator(BasePlayer player, int fuel);
        private void DestroyChargeCui(BasePlayer player);
        private void DestroyCui(BasePlayer player);
        public void OnDestroy();
    }

}

public class PlayerCopterData
{
    public BasePlayer player;
    public int coptercount;
}

private class GyroCopter : BaseEntity
{
    public BaseEntity entity;
    public BasePlayer player;
    public BaseEntity wheel;
    public BaseEntity deck1;
    public BaseEntity deck2;
    public BaseEntity barrel;
    public BaseEntity barcenter;
    public BaseEntity rotor1;
    public BaseEntity rotor2;
    public BaseEntity rotor3;
    public BaseEntity rotor4;
    public BaseEntity skid1;
    public BaseEntity skid2;
    public BaseEntity skid3;
    public BaseEntity skid4;
    public BaseEntity skidsupr;
    public BaseEntity skidsupl;
    public BaseEntity tailrotor1;
    public BaseEntity tailrotor2;
    public BaseEntity floor;
    public BaseEntity nosesign;
    public BaseEntity tail1;
    public BaseEntity tail2;
    public BaseEntity passengerchair1;
    public BaseEntity passengerchair2;
    public BaseEntity copterlock;
    public BaseEntity panel;
    public BaseEntity dabomb;
    private BaseEntity fseat;
    private BaseEntity fseatb;
    private BaseEntity fseatl;
    private BaseEntity fseatr;
    private BaseEntity rseat;
    private BaseEntity rseatb;
    private BaseEntity rseatl;
    private BaseEntity rseatr;
    private BaseEntity lseat;
    private BaseEntity lseatb;
    private BaseEntity lseatl;
    private BaseEntity lseatr;
    public BaseEntity pilotstorage;
    public BaseEntity passstorage1;
    public BaseEntity passstorage2;
    private Quaternion entityrot;
    private Vector3 entitypos;
    public bool moveforward;
    public bool movebackward;
    public bool moveup;
    public bool movedown;
    public bool rotright;
    public bool rotleft;
    public bool sprinting;
    public bool islanding;
    public bool hasbonuscharge;
    public bool paintingsarelocked;
    public ulong ownerid;
    private int count;
    public bool engineon;
    private float minaltitude;
    private Gyrocopter instance;
    public bool throttleup;
    private int sprintcost;
    private int normalcost;
    private float sprintspeed;
    private float normalspeed;
    public int currentfuel;
    private int baserechargerate;
    private int bonusrechargerate;
    private bool isenabled;
    private bool hasdabomb;
    private SphereCollider sphereCollider;
    private string prefabdeck;
    private string prefabbar;
    private string prefabrotor;
    private string prefabbarrel;
    private string prefabbomb;
    private string prefabfloor;
    private string prefabnosesign;
    private string prefabpanel;
    private string prefabskid;
    private string wheelprefab;
    private string copterlockprefab;
    private string prefabchair;
    private string prefabnetting;
    private string prefabdabomb;
     void Awake();
    public void SetBaseAltitude();
    private BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, BaseEntity parent, ulong skinid);
    private void SpawnRefresh(BaseEntity entity);
    public void SpawnCopter();
    private void SpawnFuelBox();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    public void DropNet();
    public void LockPaintings();
    public void UnLockPaintings();
    private BasePlayer GetPilot();
    private void FuelCheck();
    public void CopterInput(InputState input, BasePlayer player);
    public void UseDaBombs(BasePlayer player);
    public void ReloadDaBombs(BasePlayer player);
    public void FindMoreDaBombs(BasePlayer player);
    private void FixedUpdate();
    private IEnumerator RefreshCopter();
    private void ResetMovement();
    public void OnDestroy();
}

private class DaBomb : MonoBehaviour
{
    private BaseEntity entity;
    private GyroCopter copter;
    private Vector3 entitypos;
    private Quaternion entityrot;
    private BaseEntity dabomb;
    private bool onGround;
    private float damageradius;
    private float damageamount;
    private void Awake();
    private void ImpactDamage(Vector3 hitpos);
    private void ImpactFX(Vector3 pos);
    private void SpawnFireEffects();
    private void FixedUpdate();
    private void OnDestroy();
}

private class CopterNet : MonoBehaviour
{
    public BaseEntity netting1;
    public BaseEntity netting2;
    public BaseEntity netting3;
    private BaseEntity entity;
    private GyroCopter copter;
    private Vector3 entitypos;
    private Quaternion entityrot;
    private void Awake();
    private void RefreshNetting();
    private void FixedUpdate();
    private void OnDestroy();
}

private class FuelControl : MonoBehaviour
{
    private BasePlayer player;
    private GyroCopter copter;
    public string anchormaxstr;
    public string colorstr;
    private Vector3 playerpos;
    private Gyrocopter instance;
    private bool ischarging;
    private int count;
    private float rechargerange;
    private int rechargerate;
    private void Awake();
    private void Recharge();
    private void ChargingFX();
    private void FixedUpdate();
    public void RechargeIndicator(BasePlayer player);
    public void fuelIndicator(BasePlayer player, int fuel);
    private void DestroyChargeCui(BasePlayer player);
    private void DestroyCui(BasePlayer player);
    public void OnDestroy();
}


```

---

## HackableLock by Ryz0r - Locks looting a hackable crate to the person who started the hack, if enabled with configuration

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Hackable Lock", "Ryz0r", "2.3.0")]
[Description("Locks Hackable Crate to person who started hack process")]
public class HackableLock : CovalencePlugin
{
    [PluginReference]
    private Plugin DiscordEvents;
    private Plugin Clans;
    private Plugin Friends;
    private const string PermissionUse;
    private void Init();
    private void OnServerSave();
    private void Unload();
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalConfiguration GlobalSettings;
        [JsonProperty(PropertyName = "Chat settings")]
        public ChatConfiguration ChatSettings;
        [JsonProperty(PropertyName = "Discord settings")]
        public DiscordConfiguration DiscordSettings;
        public class GlobalConfiguration
        {
            [JsonProperty(PropertyName = "Use permissions")]
            public bool UsePermission;
            [JsonProperty(PropertyName = "Allow admins to use without permission")]
            public bool AdminsAllowed;
            [JsonProperty(PropertyName = "Enabled?")]
            public bool Enabled;
            [JsonProperty(PropertyName = "Lock time (seconds)")]
            public float LockTime;
            [JsonProperty(PropertyName = "Use Clans")]
            public bool EnableClans;
            [JsonProperty(PropertyName = "Use Teams")]
            public bool EnableTeams;
            [JsonProperty(PropertyName = "Use Friends")]
            public bool EnableFriends;
        }

        public class ChatConfiguration
        {
            [JsonProperty(PropertyName = "Chat steamID icon")]
            public ulong SteamIDIcon;
            [JsonProperty(PropertyName = "Notifications Enabled?")]
            public bool Notifications;
            [JsonProperty(PropertyName = "Player Console Notifications only")]
            public bool SendConsoleOnly;
        }

        public class DiscordConfiguration
        {
            [JsonProperty(PropertyName = "WebhookURL")]
            public string WebhookURL;
            [JsonProperty(PropertyName = "Enabled?")]
            public bool Enabled;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private StoredData _storedData;
    private class StoredData
    {
        public Dictionary<ulong, LockInfo> _lockedCrates;
    }

    private class LockInfo
    {
        public ulong PlayerID;
        public string PlayerName;
        public float UnlockTime;
    }

    private void LoadData();
    private void SaveData();
    private void ClearData();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    private void CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private void OnEntityKill(HackableLockedCrate crate);
    private object CanLootEntity(BasePlayer player, HackableLockedCrate crate);
    private void LockCrateToPlayer(BasePlayer player, HackableLockedCrate crate);
    private float GetCrateLockTime(ulong crateID);
    private string GetGridPosition(Vector3 pos);
    private void PrintDiscord(string message);
    private void Print(IPlayer player, string message);
    private bool IsOwner(ulong userID, ulong owner);
    private bool SameTeam(ulong playerID, ulong friendID);
    private bool AreFriends(ulong playerID, ulong friendID);
    private bool SameClan(ulong playerID, ulong friendID);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalConfiguration GlobalSettings;
    [JsonProperty(PropertyName = "Chat settings")]
    public ChatConfiguration ChatSettings;
    [JsonProperty(PropertyName = "Discord settings")]
    public DiscordConfiguration DiscordSettings;
    public class GlobalConfiguration
    {
        [JsonProperty(PropertyName = "Use permissions")]
        public bool UsePermission;
        [JsonProperty(PropertyName = "Allow admins to use without permission")]
        public bool AdminsAllowed;
        [JsonProperty(PropertyName = "Enabled?")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Lock time (seconds)")]
        public float LockTime;
        [JsonProperty(PropertyName = "Use Clans")]
        public bool EnableClans;
        [JsonProperty(PropertyName = "Use Teams")]
        public bool EnableTeams;
        [JsonProperty(PropertyName = "Use Friends")]
        public bool EnableFriends;
    }

    public class ChatConfiguration
    {
        [JsonProperty(PropertyName = "Chat steamID icon")]
        public ulong SteamIDIcon;
        [JsonProperty(PropertyName = "Notifications Enabled?")]
        public bool Notifications;
        [JsonProperty(PropertyName = "Player Console Notifications only")]
        public bool SendConsoleOnly;
    }

    public class DiscordConfiguration
    {
        [JsonProperty(PropertyName = "WebhookURL")]
        public string WebhookURL;
        [JsonProperty(PropertyName = "Enabled?")]
        public bool Enabled;
    }

}

public class GlobalConfiguration
{
    [JsonProperty(PropertyName = "Use permissions")]
    public bool UsePermission;
    [JsonProperty(PropertyName = "Allow admins to use without permission")]
    public bool AdminsAllowed;
    [JsonProperty(PropertyName = "Enabled?")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Lock time (seconds)")]
    public float LockTime;
    [JsonProperty(PropertyName = "Use Clans")]
    public bool EnableClans;
    [JsonProperty(PropertyName = "Use Teams")]
    public bool EnableTeams;
    [JsonProperty(PropertyName = "Use Friends")]
    public bool EnableFriends;
}

public class ChatConfiguration
{
    [JsonProperty(PropertyName = "Chat steamID icon")]
    public ulong SteamIDIcon;
    [JsonProperty(PropertyName = "Notifications Enabled?")]
    public bool Notifications;
    [JsonProperty(PropertyName = "Player Console Notifications only")]
    public bool SendConsoleOnly;
}

public class DiscordConfiguration
{
    [JsonProperty(PropertyName = "WebhookURL")]
    public string WebhookURL;
    [JsonProperty(PropertyName = "Enabled?")]
    public bool Enabled;
}

private class StoredData
{
    public Dictionary<ulong, LockInfo> _lockedCrates;
}

private class LockInfo
{
    public ulong PlayerID;
    public string PlayerName;
    public float UnlockTime;
}


```

---

## HammerTime by Shady - Tweak "Demolish" and "Rotate" time when using the hammer

```csharp
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Hammer Time", "Shady", "1.0.20", ResourceId = 1711)]
[Description("Tweak settings for building blocks like demolish time, and rotate time.")]
internal class HammerTime : RustPlugin
{
    [PluginReference]
    private readonly Plugin Friends;
    [PluginReference]
    private readonly Plugin Clans;
    private float DemolishTime;
    private float RotateTime;
    private float RepairCooldown;
    private float pluginInitTime;
    private bool DemolishAfterRestart;
    private bool RotateAfterRestart;
    private bool MustOwnDemolish;
    private bool MustOwnRotate;
    private bool FriendsCanDemolish;
    private bool FriendsCanRotate;
    private bool ClanCanDemolish;
    private bool ClanCanRotate;
    protected override void LoadDefaultConfig();
    private void Init();
    private void OnServerInitialized();
    [ConsoleCommand("hammertime.updateall")]
    private void consoleUpdateAll(ConsoleSystem.Arg arg);
    private void DoInvokes(BuildingBlock block, bool demo, bool rotate);
    private void OnEntityBuilt(Planner plan, GameObject objectBlock);
    private void OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
    private object OnStructureRepair(BaseCombatEntity block, BasePlayer player);
    private object OnHammerHit(BasePlayer player, HitInfo hitInfo);
    private object OnStructureDemolish(BuildingBlock block, BasePlayer player);
    private object OnStructureRotate(BuildingBlock block, BasePlayer player);
    protected override void LoadDefaultMessages();
    private string GetMessage(string key, string steamId);
    private T GetConfig(string name, T defaultValue);
    private bool HasPerms(string userID, string perm);
    private bool HasPerms(ulong userID, string perm);
}


```

---

## HandyMan by rostov114 - Provides AOE repair functionality to players with permission

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Facepunch;
using System;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Handy Man", "nivex", "1.3.4")]
[Description("Provides AOE repair functionality to the player. Repair is only possible where you can build.")]
public class HandyMan : RustPlugin
{
    [PluginReference]
     Plugin NoEscape;
    readonly DynamicConfigFile dataFile;
     Dictionary<ulong, bool> playerData;
     Dictionary<BuildingPrivlidge, BaseCombatEntity> entities;
     bool _allowHandyManFixMessage;
     bool _allowAOERepair;
     PluginTimers RepairMessageTimer;
    static int constructionMask;
    static int allMask;
    static float lastAttackLimit;
    static float privDistance;
     bool IsRaidBlocked(string targetId);
     bool HasPerm(BasePlayer player);
     bool HasResources(BasePlayer player);
    private void Loaded();
     void OnHammerHit(BasePlayer player, HitInfo info);
     void Repair(BaseCombatEntity entity, BasePlayer player);
    private void RepairAOE(BaseCombatEntity entity, BasePlayer player);
     object CanRepair(BaseCombatEntity entity, BasePlayer player, List<BaseCombatEntity> entities);
    public bool DoRepair(BaseCombatEntity entity, BasePlayer player);
    [ChatCommand("handyman")]
    private void ChatCommand_HandyMan(BasePlayer player, string command, string[] args);
    [ConsoleCommand("healthcheck")]
    private void ConsoleCommand_HealthCheck();
    private bool Changed;
    private bool UseRaidBlocker;
    private bool DefaultHandyManOn;
    private int RepairRange;
    private int HandyManChatInterval;
    private float repairMulti;
    private bool repairDeployables;
    private int maxRepairEnts;
    private float markRepairedTime;
    protected override void LoadDefaultMessages();
     void LoadVariables();
    protected override void LoadDefaultConfig();
     object GetConfig(string menu, string dataValue, object defaultValue);
    public string msg(string key, string id, object[] args);
    public string RemoveFormatting(string source);
    private void SendChatMessage(BasePlayer player, string msg);
}


```

---

## HappyHour by  - Gives players items at specific times

```csharp
using UnityEngine;
using Convert = System.Convert;
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Core;
using System.Linq;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Happy Hour Plugin", "BuzZ", "2.0.2")]
public class HappyHour : RustPlugin
{
     bool debug;
    private string page;
    private string MainHappyPanel;
    private CuiElementContainer HoursCuiElement;
    private string InsideHappyPanel;
    private string hourbutton;
    private string hourstatus;
    private string houritem;
    private string itembutton;
    private string whathour;
     float rate;
    const string admin;
    private Dictionary<int, int> temporary;
    public class StoredData
    {
        public Dictionary<int, Pack> hourpack;
        public StoredData();
    }

    private StoredData storedData;
    public class Pack
    {
        public Dictionary<int, int> intquantity;
        public bool status;
        public List<ulong> received;
    }

     void Init();
     void Unload();
     void OnServerInitialized();
     void insidetimer();
     void CleanZeroz();
     void GiveDaHourGifts(int hour);
    [ChatCommand("happy")]
    private void OpenMainHappyPanel(BasePlayer player, string command, string[] args, int editing);
     void DisplayHoursButtons(BasePlayer player, int editing);
    [ConsoleCommand("hour_toggle")]
    private void ToggleDaHourStatus(ConsoleSystem.Arg arg);
    [ConsoleCommand("hour_display")]
    private void ItemCatalogForDaHour(ConsoleSystem.Arg arg);
    private void ItemCatalogForDaHour(BasePlayer player, int hour);
    [ConsoleCommand("hour_item")]
    private void AddItemOnHourPack(ConsoleSystem.Arg arg);
    public Dictionary<int, string> items;
}

public class StoredData
{
    public Dictionary<int, Pack> hourpack;
    public StoredData();
}

public class Pack
{
    public Dictionary<int, int> intquantity;
    public bool status;
    public List<ulong> received;
}


```

---

## HardcoreWorkbench by Marat - Removes the tech tree from workbenches, replacing it with the research table menu

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Hardcore Workbench", "Marat", "1.0.4")]
[Description("Removes tech tree from workbenches")]
public class HardcoreWorkbench : RustPlugin
{
    private const string WorkbenchLayer;
    private const string permissionName;
    private readonly Dictionary<BasePlayer, WorkbenchBehavior> benchOpen;
    private readonly List<BaseEntity> vendingMachine;
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void CanLootEntity(BasePlayer player, Workbench container);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private void OnEntityLeave(TriggerWorkbench trigger, BasePlayer player);
    private object CanUnlockTechTreeNode(BasePlayer player);
    private object OnEntityVisibilityCheck(ResearchTable table, BasePlayer player);
    private void OnOpenBox(BasePlayer player);
    private void OnCloseBox(BasePlayer player);
    private class PluginConfig
    {
        [JsonProperty("Use workbench menu")]
        public bool useMenuWorkbench;
        [JsonProperty("Remove need for workbench")]
        public bool disableCraftMode;
        [JsonProperty("Time to research item")]
        public float itemResearchTime;
        [JsonProperty("Add vehicles parts vending machine")]
        public bool addVehiclesParts;
        [JsonProperty("Use permission to open workbench")]
        public bool usePermission;
    }

    protected override void LoadDefaultConfig();
    private static PluginConfig config;
    protected override void SaveConfig();
    protected override void LoadConfig();
    [ConsoleCommand("UI_Workbench")]
    private void CmdWorkbenchUI(ConsoleSystem.Arg arg);
    private void OpenWorkbench(BasePlayer player);
    public class WorkbenchBehavior : FacepunchBehaviour
    {
        private ResearchTable container;
        public void Awake();
        public void Open(BasePlayer player);
        public void Close(BasePlayer player);
        public void Destroy();
        public static ResearchTable CreateTable(BasePlayer player);
        public void StartLoot(BasePlayer player);
    }

    protected override void LoadDefaultMessages();
    private string GetMessage(string key, BasePlayer player);
    private StoredData storedData;
    private class StoredData
    {
        public Dictionary<string, int> cachedLevel;
    }

    private void SaveData();
    private void LoadData();
}

private class PluginConfig
{
    [JsonProperty("Use workbench menu")]
    public bool useMenuWorkbench;
    [JsonProperty("Remove need for workbench")]
    public bool disableCraftMode;
    [JsonProperty("Time to research item")]
    public float itemResearchTime;
    [JsonProperty("Add vehicles parts vending machine")]
    public bool addVehiclesParts;
    [JsonProperty("Use permission to open workbench")]
    public bool usePermission;
}

public class WorkbenchBehavior : FacepunchBehaviour
{
    private ResearchTable container;
    public void Awake();
    public void Open(BasePlayer player);
    public void Close(BasePlayer player);
    public void Destroy();
    public static ResearchTable CreateTable(BasePlayer player);
    public void StartLoot(BasePlayer player);
}

private class StoredData
{
    public Dictionary<string, int> cachedLevel;
}


```

---

## HazmatDiving by  - Protects players wearing any Hazmat suit while swimming (Supports the new Spacesuit skin)

```csharp
using Oxide.Core;
using System.Collections.Generic;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using System;

Oxide.Plugins
[Info("Hazmat Diving", "Krungh Crow", "2.0.2")]
[Description("This will protect you from drowning and cold damage while swimming.")]
public class HazmatDiving : RustPlugin
{
    private bool Changed;
     bool debug;
     bool loaded;
    private bool applydamageArmour;
    private float armourDamageAmount;
    private float dmgdrowning1;
    private float dmgcold1;
    private float dmgdrowning2;
    private float dmgcold2;
    private float dmgdrowning3;
    private float dmgcold3;
    private float dmgdrowning4;
    private float dmgcold4;
    private float dmgdrowning5;
    private float dmgcold5;
     string Prefix;
     string PrefixColor;
     string ChatColor;
     ulong SteamIDIcon;
    const string HazmatUse;
     void Init();
    protected override void LoadDefaultConfig();
     void Loaded();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void LoadVariables();
    protected override void LoadDefaultMessages();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
    private float getScaledDamage(float current, float deduction);
    private float getDamageDeduction(BasePlayer player, Rust.DamageType damageType);
     void CanWearItem(PlayerInventory inventory, Item item, int targetPos);
}


```

---

## HazmatSkinChanger by MasterSplinter - Allows players to craft/skin any Hazmat suit instead of the regular Hazmat suit

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Hazmat Skin Changer", "MasterSplinter", "1.0.3")]
[Description("Craft/Skin any Hazmat instead of the stock Hazmat for players with permission.")]
public class HazmatSkinChanger : RustPlugin
{
     string Prefix;
     string PrefixHelp;
     string PrefixColor;
     ulong SteamIDIcon;
    private bool ConfigChanged;
     string stocksuit;
     string suitblue;
     string suitgreen;
     string suitheavy;
     string suitarctic;
     string arcticsuit;
     string suitspace;
     string suitnomad;
     bool loaded;
     bool debug;
    const string HTSS_craftS;
    const string HTSS_skinS;
    const string HTSS_wearS;
    const string HTSS_craftGS;
    const string HTSS_skinGS;
    const string HTSS_wearGS;
    const string HTSS_craftH;
    const string HTSS_skinH;
    const string HTSS_wearH;
    const string HTSS_craftASS;
    const string HTSS_skinASS;
    const string HTSS_wearASS;
    const string HTSS_craftAS;
    const string HTSS_skinAS;
    const string HTSS_wearAS;
    const string HTSS_craftSS;
    const string HTSS_skinSS;
    const string HTSS_wearSS;
    const string HTSS_craftNS;
    const string HTSS_skinNS;
    const string HTSS_wearNS;
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    protected override void LoadDefaultMessages();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void Init();
     void OnItemCraftFinished(ItemCraftTask task, Item item);
     void CanWearItem(PlayerInventory inventory, Item item, int targetPos);
     void OnItemPickup(Item item, BasePlayer player, int targetPos);
    [ChatCommand("skinhazmat")]
    private void skinhazmatCmd(BasePlayer player, string command, string[] args, Item item);
}


```

---

## Headquarters by digital0iced0 - Allows players to have one protected headquarter base (protection % varies with storage usage).

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Headquarters", "digital0iced0", "1.2.0")]
[Description("Allows players to have one protected headquarter base.")]
public class Headquarters : RustPlugin
{
    private static PluginData _data;
    protected static ConfigFile _config;
    private bool _freeForAllActive;
    private bool _hooksSubscribed;
    private Timer _utilityTimer;
    private CuiElementContainer _cachedUI;
    private const string AdminPermissionName;
    private static readonly string[] StorageTypesPenalizeModules;
    private static readonly string[] StorageTypes;
    protected class ConfigFile
    {
        public HeadquartersConfig HeadquartersConfig;
        public static ConfigFile DefaultConfig();
    }

    protected class HeadquartersConfig
    {
        public float Radius { get; set; }
        public bool MapMarkersEnabled { get; set; }
        public bool TeleportEnabled { get; set; }
        public float QuitPenaltyHours { get; set; }
        public float DistanceToTC { get; set; }
        public bool InvulnerableTC { get; set; }
        public bool ConquerModeEnabled { get; set; }
        public bool FreeForAllEnabled { get; set; }
        public float FreeForAllHoursAfterWipe { get; set; }
        public string MarkerPrefab { get; set; }
        public float ProtectionPercent { get; set; }
        public float ProtectionPercentMinimum { get; set; }
        public float ProtectionSlotsWithoutPenalty { get; set; }
        public float ProtectionPenaltyPercentPerSlot { get; set; }
        public int ProtectionConstantSecondsAfterDamage { get; set; }
        public bool MessagePlayersHeadquarterAttacked { get; set; }
        public bool MessagePlayersHeadquarterDestroyed { get; set; }
        public bool UIEnabled { get; set; }
        public int UIRefreshRateSeconds { get; set; }
        public Anchor UIAnchorMin { get; set; }
        public Anchor UIAnchorMax { get; set; }
        public string[] AdditionalProtectedEntities { get; set; }
    }

    protected class Anchor
    {
        public float X { get; set; }
        public float Y { get; set; }
        public Anchor();
        public Anchor(float x, float y);
    }

    protected static HeadquartersConfig getConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginData
    {
        [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, Headquarter> AvailableHeadquarters;
        [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, HeadquarterMember> MemberPlayers;
        [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, HeadquarterQuitter> QuitterPlayers;
    }

    private class HeadquarterQuitter
    {
        public string UserId { get; set; }
        public DateTime QuitStartedAt { get; set; }
        public HeadquarterQuitter(string user);
    }

    private class HeadquarterMember
    {
        public string UserId { get; set; }
        public string LeaderId { get; set; }
        public HeadquarterMember(string user, string leader);
    }

    private class Headquarter
    {
        public string LeaderId { get; set; }
        public string Name { get; set; }
        public int StorageSlots { get; set; }
        public float PositionX { get; set; }
        public float PositionY { get; set; }
        public float PositionZ { get; set; }
        public bool IsActive { get; set; }
        public List<string> MemberIds { get; set; }
        [JsonIgnore]
        public MapMarkerGenericRadius marker;
        public DateTime DismantleStartedAt { get; set; }
        public float LastKnownProtectionPercent { get; set; }
        public bool MapMarkerEnabled { get; set; }
        public DateTime LastMarkerRefresh { get; set; }
        public DateTime LastDamaged { get; set; }
        public DateTime LastUIUpdate { get; set; }
        [JsonIgnore]
        private Headquarters Instance;
        public Headquarter(string user, string name, float positionX, float positionY, float positionZ, int storageSlots, bool mapMarkerEnabled);
        public void SetInstance(Headquarters instance);
        public bool HasMember(string user);
        public Vector3 getPosition();
        public void MarkDamaged();
        public void MarkUIUpdated();
        public bool ShouldUpdateUI();
        public void CreateMapMarker(bool freeForAllActive);
        public void RemoveMapMarker();
        public void UpdateMapMarker();
        public void RefreshMapMarker(bool freeForAllActive, bool forceUpdate);
        private Color getProtectionColor();
        public void RecalculateProtectionScale();
        public void RefreshUI(bool forceRefresh);
        public void RemoveUI();
        public bool IsBeingRaided();
        public bool IsDismantling();
        public float GetCurrentProtectionPercent();
    }

    private class Rgba
    {
        public float R { get; set; }
        public float G { get; set; }
        public float B { get; set; }
        public float A { get; set; }
        public Rgba();
        public Rgba(float r, float g, float b, float a);
        public static string Format(Rgba rgba);
    }

    private class UI
    {
        public const string MainContainer;
        public const string ProtectionContainer;
        public static Rgba PrimaryColor;
        public static Rgba LightColor;
        public static Rgba TextColor;
        public static CuiElementContainer Container(string name, string bgColor, Anchor Min, Anchor Max, string parent, float fadeOut, float fadeIn);
        public static void Text(string name, string parent, CuiElementContainer container, TextAnchor anchor, string color, int fontSize, string text, Anchor Min, Anchor Max, string font, float fadeOut, float fadeIn);
        public static void Element(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string bgColor, float fadeOut, float fadeIn);
        public static void Image(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string img, string color);
        public static CuiElementContainer GetCachedUI(HeadquartersConfig c);
        public static CuiElementContainer GetProtectionUI(HeadquartersConfig c, Headquarter hq, BasePlayer player, Headquarters instance);
    }

    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
     string GetGrid(Vector3 pos);
    private Headquarter GetPlayerHeadquarter(BasePlayer player);
    private BuildingPrivlidge GetHeadquarterTC(Headquarter hq);
    private void DismantleLeaderHQ(BasePlayer player);
    private void ConqueredHQ(Headquarter hq);
    private void DismantleHQ(Headquarter hq);
    private void AddQuitter(BasePlayer player);
    private void AnnounceConquered(Headquarter conqueringHQ, Headquarter fallenHQ);
    private bool IsLeader(BasePlayer player);
    private bool IsMember(BasePlayer player);
    private bool IsQuitter(BasePlayer player);
    private void SaveData();
    private void LoadData();
    private void Init();
    private void OnServerInitialized(bool initial);
    private void OnServerSave();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
     void OnEntityBuilt(Planner plan, GameObject go);
     object CanMoveItem(Item item, PlayerInventory playerLoot, ItemContainerId targetContainerID, int targetSlot, int amount);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private object OnEntityTakeDamage(BaseVehicleModule entity, HitInfo info);
    private void OnEntityDeath(BuildingPrivlidge entity, HitInfo info);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private object HandleToolCupboardDamage(BuildingPrivlidge entity, HitInfo info);
    private bool shouldHandleBuildingDamage(BaseCombatEntity entity);
    private object HandleBuildingDamage(BaseCombatEntity entity, HitInfo info);
    private Headquarter GetHeadquarterAtPosition(Vector3 position, float radius);
     object OnCupboardClearList(BuildingPrivlidge privilege, BasePlayer player);
     object OnCupboardDeauthorize(BuildingPrivlidge privilege, BasePlayer player);
     object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private void OutputFFAStatus(BasePlayer player);
    private void LoadMapMarkers();
    private void RefreshMapMarkers();
    private void RemoveMapMarkers();
    private void RefreshStorageCounts();
    private void RefreshHeadquarterStorageCount(Headquarter headquarter);
    private void RefreshUIForAllPlayers();
    private void AttemptRefreshUIForPlayer(BasePlayer p);
    private void RefreshUIForPlayer(BasePlayer p, Headquarter hq);
    private void RemoveUIForAllPlayers();
    private void RemoveUIForPlayer(BasePlayer p);
    private void CheckFreeForAll();
    private void RemoveDismantled();
    private void RemoveQuitters();
    private void DeauthPlayerFromTC(BasePlayer player, BuildingPrivlidge privilege);
    private void MessageAllPlayersHeadquarterBeingAttacked(Headquarter hq, BasePlayer attacker);
    private void OutputPlayerStatus(BasePlayer player);
    [ChatCommand("hq.help")]
    private void cmdChatHeadquarterListAll(BasePlayer player, string command);
    [ChatCommand("hq.start")]
    private void cmdChatHeadquarterStart(BasePlayer player, string command, string[] args);
    [ChatCommand("hq.disband")]
    private void cmdChatHeadquarterDisband(BasePlayer player, string command);
    [ChatCommand("hq.quit")]
    private void cmdChatHeadquarterQuit(BasePlayer player, string command);
    [ChatCommand("hq.list")]
    private void cmdChatHeadquarterList(BasePlayer player, string command);
    [ChatCommand("hq.teleport")]
    private void cmdChatHeadquarterTeleport(BasePlayer player, string command);
    [ChatCommand("hq.ffa")]
    private void cmdChatHeadquarterFFA(BasePlayer player, string command);
    [ChatCommand("hq.status")]
    private void cmdChatHeadquarterCheck(BasePlayer player, string command);
    [ChatCommand("grid")]
    private void cmdChatHeadquarterGrid(BasePlayer player, string command);
    [ConsoleCommand("hq.hide-markers")]
    private void cmdConsoleHideMarkers(ConsoleSystem.Arg arg);
    [ConsoleCommand("hq.show-markers")]
    private void cmdConsoleShowMarkers(ConsoleSystem.Arg arg);
    [ConsoleCommand("hq.clear-all")]
    private void cmdConsoleHeadquarterClearAll(ConsoleSystem.Arg arg);
    [ConsoleCommand("hq.start-ffa")]
    private void cmdConsoleStartFFA(ConsoleSystem.Arg arg);
    [ConsoleCommand("hq.stop-ffa")]
    private void cmdConsoleStopFFA(ConsoleSystem.Arg arg);
    [ConsoleCommand("hq.remove")]
    private void cmdConsoleRemoveHeadquarter(ConsoleSystem.Arg arg);
    [ConsoleCommand("hq.remove-quitter")]
    private void cmdConsoleRemoveQuitter(ConsoleSystem.Arg arg);
    [ConsoleCommand("hq.clear-quitters")]
    private void cmdConsoleClearQuitters(ConsoleSystem.Arg arg);
}

protected class ConfigFile
{
    public HeadquartersConfig HeadquartersConfig;
    public static ConfigFile DefaultConfig();
}

protected class HeadquartersConfig
{
    public float Radius { get; set; }
    public bool MapMarkersEnabled { get; set; }
    public bool TeleportEnabled { get; set; }
    public float QuitPenaltyHours { get; set; }
    public float DistanceToTC { get; set; }
    public bool InvulnerableTC { get; set; }
    public bool ConquerModeEnabled { get; set; }
    public bool FreeForAllEnabled { get; set; }
    public float FreeForAllHoursAfterWipe { get; set; }
    public string MarkerPrefab { get; set; }
    public float ProtectionPercent { get; set; }
    public float ProtectionPercentMinimum { get; set; }
    public float ProtectionSlotsWithoutPenalty { get; set; }
    public float ProtectionPenaltyPercentPerSlot { get; set; }
    public int ProtectionConstantSecondsAfterDamage { get; set; }
    public bool MessagePlayersHeadquarterAttacked { get; set; }
    public bool MessagePlayersHeadquarterDestroyed { get; set; }
    public bool UIEnabled { get; set; }
    public int UIRefreshRateSeconds { get; set; }
    public Anchor UIAnchorMin { get; set; }
    public Anchor UIAnchorMax { get; set; }
    public string[] AdditionalProtectedEntities { get; set; }
}

protected class Anchor
{
    public float X { get; set; }
    public float Y { get; set; }
    public Anchor();
    public Anchor(float x, float y);
}

private class PluginData
{
    [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, Headquarter> AvailableHeadquarters;
    [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, HeadquarterMember> MemberPlayers;
    [JsonProperty(ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, HeadquarterQuitter> QuitterPlayers;
}

private class HeadquarterQuitter
{
    public string UserId { get; set; }
    public DateTime QuitStartedAt { get; set; }
    public HeadquarterQuitter(string user);
}

private class HeadquarterMember
{
    public string UserId { get; set; }
    public string LeaderId { get; set; }
    public HeadquarterMember(string user, string leader);
}

private class Headquarter
{
    public string LeaderId { get; set; }
    public string Name { get; set; }
    public int StorageSlots { get; set; }
    public float PositionX { get; set; }
    public float PositionY { get; set; }
    public float PositionZ { get; set; }
    public bool IsActive { get; set; }
    public List<string> MemberIds { get; set; }
    [JsonIgnore]
    public MapMarkerGenericRadius marker;
    public DateTime DismantleStartedAt { get; set; }
    public float LastKnownProtectionPercent { get; set; }
    public bool MapMarkerEnabled { get; set; }
    public DateTime LastMarkerRefresh { get; set; }
    public DateTime LastDamaged { get; set; }
    public DateTime LastUIUpdate { get; set; }
    [JsonIgnore]
    private Headquarters Instance;
    public Headquarter(string user, string name, float positionX, float positionY, float positionZ, int storageSlots, bool mapMarkerEnabled);
    public void SetInstance(Headquarters instance);
    public bool HasMember(string user);
    public Vector3 getPosition();
    public void MarkDamaged();
    public void MarkUIUpdated();
    public bool ShouldUpdateUI();
    public void CreateMapMarker(bool freeForAllActive);
    public void RemoveMapMarker();
    public void UpdateMapMarker();
    public void RefreshMapMarker(bool freeForAllActive, bool forceUpdate);
    private Color getProtectionColor();
    public void RecalculateProtectionScale();
    public void RefreshUI(bool forceRefresh);
    public void RemoveUI();
    public bool IsBeingRaided();
    public bool IsDismantling();
    public float GetCurrentProtectionPercent();
}

private class Rgba
{
    public float R { get; set; }
    public float G { get; set; }
    public float B { get; set; }
    public float A { get; set; }
    public Rgba();
    public Rgba(float r, float g, float b, float a);
    public static string Format(Rgba rgba);
}

private class UI
{
    public const string MainContainer;
    public const string ProtectionContainer;
    public static Rgba PrimaryColor;
    public static Rgba LightColor;
    public static Rgba TextColor;
    public static CuiElementContainer Container(string name, string bgColor, Anchor Min, Anchor Max, string parent, float fadeOut, float fadeIn);
    public static void Text(string name, string parent, CuiElementContainer container, TextAnchor anchor, string color, int fontSize, string text, Anchor Min, Anchor Max, string font, float fadeOut, float fadeIn);
    public static void Element(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string bgColor, float fadeOut, float fadeIn);
    public static void Image(string name, string parent, CuiElementContainer container, Anchor Min, Anchor Max, string img, string color);
    public static CuiElementContainer GetCachedUI(HeadquartersConfig c);
    public static CuiElementContainer GetProtectionUI(HeadquartersConfig c, Headquarter hq, BasePlayer player, Headquarters instance);
}


```

---

## Heal by MrBlue - Allows players with permission to heal themselves or others

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Heal", "Wulf", "3.1.1")]
[Description("Allows players with permission to heal themselves or others")]
public class Heal : CovalencePlugin
{
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Command cooldown in seconds (0 to disable)")]
        public int CommandCooldown;
        [JsonProperty("Maximum heal amount")]
        public int MaxHealAmount;
        [JsonProperty("Notify target when healed")]
        public bool NotifyTarget;
        [JsonProperty("Use permission system")]
        public bool UsePermissions;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private readonly Hash<string, float> cooldowns;
    private const string permSelf;
    private const string permAll;
    private const string permPlayer;
    private const string permNoCooldown;
    private void Init();
    private bool HasCooldown(IPlayer player, string command);
    private object HealPlayer(IPlayer target, float amount);
    private void CommandHeal(IPlayer player, string command, string[] args);
    private void CommandHealPlayer(IPlayer player, string command, string[] args);
    private void CommandHealAll(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
    private void MigratePermission(string oldPerm, string newPerm);
}

private class Configuration
{
    [JsonProperty("Command cooldown in seconds (0 to disable)")]
    public int CommandCooldown;
    [JsonProperty("Maximum heal amount")]
    public int MaxHealAmount;
    [JsonProperty("Notify target when healed")]
    public bool NotifyTarget;
    [JsonProperty("Use permission system")]
    public bool UsePermissions;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## HealGun by Wolfleader101 - A customizable heal gun

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Heal Gun", "Wolfleader101", "1.3.7")]
[Description("A customizable heal gun")]
 class HealGun : RustPlugin
{
    private PluginConfig config;
    public const string HealgunPerms;
    private void Init();
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
     IEnumerator WoundTimer(BasePlayer player);
    private class PluginConfig
    {
        [JsonProperty("Healgun")]
        public string Healgun { get; set; }
        [JsonProperty("Heal Amount")]
        public float HealAmount { get; set; }
        [JsonProperty("Pending Health Amount")]
        public float PendingHealAmount { get; set; }
        [JsonProperty("Can Revive")]
        public bool CanRevive { get; set; }
        [JsonProperty("Revive Time")]
        public float ReviveTime { get; set; }
    }

    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
}

private class PluginConfig
{
    [JsonProperty("Healgun")]
    public string Healgun { get; set; }
    [JsonProperty("Heal Amount")]
    public float HealAmount { get; set; }
    [JsonProperty("Pending Health Amount")]
    public float PendingHealAmount { get; set; }
    [JsonProperty("Can Revive")]
    public bool CanRevive { get; set; }
    [JsonProperty("Revive Time")]
    public float ReviveTime { get; set; }
}


```

---

## HealthVehicles by SwenenzY - Changes health of minicopter, scrap copter, and balloon

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("Health Vehicles", "SwenenzY", "1.0.1")]
[Description("Raises vehicles health.")]
public class HealthVehicles : RustPlugin
{
    private ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty(PropertyName = "Mini start health ( ex : 500 )")]
        public float MiniStartHealth;
        [JsonProperty(PropertyName = "Mini max health ( ex : 1000 )")]
        public float MiniMaxHealth;
        [JsonProperty(PropertyName = "Scrap start health ( ex : 500 )")]
        public float ScrapStartHealth;
        [JsonProperty(PropertyName = "Scrap max health ( ex : 2000 )")]
        public float ScrapMaxHealth;
        [JsonProperty(PropertyName = "Balloon start health ( ex : 500 )")]
        public float BalloonStartHealth;
        [JsonProperty(PropertyName = "Balloon max health ( ex : 2000 )")]
        public float BalloonMaxHealth;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void OnEntitySpawned(MiniCopter mini);
     void OnEntitySpawned(ScrapTransportHelicopter scrap);
     void OnEntitySpawned(HotAirBalloon balloon);
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "Mini start health ( ex : 500 )")]
    public float MiniStartHealth;
    [JsonProperty(PropertyName = "Mini max health ( ex : 1000 )")]
    public float MiniMaxHealth;
    [JsonProperty(PropertyName = "Scrap start health ( ex : 500 )")]
    public float ScrapStartHealth;
    [JsonProperty(PropertyName = "Scrap max health ( ex : 2000 )")]
    public float ScrapMaxHealth;
    [JsonProperty(PropertyName = "Balloon start health ( ex : 500 )")]
    public float BalloonStartHealth;
    [JsonProperty(PropertyName = "Balloon max health ( ex : 2000 )")]
    public float BalloonMaxHealth;
    public static ConfigFile DefaultConfig();
}


```

---

## HealthyGuns by VisEntities - Restores full condition to weapons spawned in loot crates and barrels

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Healthy Guns", "VisEntities", "4.1.0")]
[Description("Restores full condition to weapons spawned in loot crates and barrels.")]
public class HealthyGuns : RustPlugin
{
    private static HealthyGuns _plugin;
    private void Init();
    private void Unload();
    private void OnServerInitialized(bool isStartup);
    private void OnLootSpawn(LootContainer container);
    private IEnumerator RepairAllContainersCoroutine();
    private void RepairContainerContents(LootContainer container);
    private bool ItemOfCategory(Item item, ItemCategory category);
    private static class CoroutineUtil
    {
        private static readonly Dictionary<string, Coroutine> _activeCoroutines;
        public static void StartCoroutine(string coroutineName, IEnumerator coroutineFunction);
        public static void StopCoroutine(string coroutineName);
        public static void StopAllCoroutines();
    }

}

private static class CoroutineUtil
{
    private static readonly Dictionary<string, Coroutine> _activeCoroutines;
    public static void StartCoroutine(string coroutineName, IEnumerator coroutineFunction);
    public static void StopCoroutine(string coroutineName);
    public static void StopAllCoroutines();
}


```

---

## HeliControl by Shady - Manage CH47 and helicopter health, player damage, configure turrets/rockets, and more

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System.Linq;

Oxide.Plugins
[Info("HeliControl", "Shady", "1.4.5", ResourceId = 1348)]
[Description("Tweak various settings of helicopters.")]
internal class HeliControl : RustPlugin
{
    private const uint ROCKET_PREFAB_ID;
    private const uint AIRBURST_ROCKET_PREFAB_ID;
    private const uint NAPALM_ROCKET_PREFAB_ID;
    private const uint HELI_CRATE_PREFAB_ID;
    private const uint NAPALM_FIREBALL_PREFAB_ID;
    private const uint OIL_FIREBALL_PREFAB_ID;
    private const uint CHINOOK_EVENT_PREFAB_ID;
    private const uint HELI_EVENT_PREFAB_ID;
    private const uint CHINOOK_SCIENTISTS_PREFAB_ID;
    private const string HELI_PREFAB;
    private const string CHINOOK_PREFAB;
    [PluginReference]
    private readonly Plugin Vanish;
    private PatrolHelicopterAI HeliInstance { get; set; }
    private CH47HelicopterAIController CH47Instance { get; set; }
    private float _lastSpawnTimer;
    private float _lastSpawnTimerCH47;
    private DateTime _lastTimerStart;
    private DateTime _lastTimerStartCH47;
    private Timer _callTimer;
    private Timer _callTimerCH47;
    private Timer CallTimer { get; set; }
    private Timer CallTimerCH47 { get; set; }
    private TriggeredEventPrefab[] _eventPrefabs;
    public TriggeredEventPrefab[] EventPrefabs { get; set; }
    private NetworkableId _timerHeliId;
    private CH47HelicopterAIController _timerCH47;
    private bool _configChanged;
    private bool _terrainHookCalled;
    private bool _useNapalm;
    private bool _init;
    private StoredData _lootData;
    private StoredData2 _weaponsData;
    private StoredData3 _cooldownData;
    private StoredData4 _spawnsData;
    private float _boundary;
    private readonly HashSet<PatrolHelicopter> _PatrolHelicopters;
    private readonly HashSet<CH47HelicopterAIController> _chinooks;
    private readonly HashSet<HelicopterDebris> _gibs;
    private readonly HashSet<FireBall> _fireBalls;
    private readonly HashSet<PatrolHelicopter> _forceCalled;
    private readonly HashSet<CH47HelicopterAIController> _forceCalledCh;
    private readonly HashSet<LockedByEntCrate> _lockedCrates;
    private readonly HashSet<HackableLockedCrate> _hackLockedCrates;
    private readonly Dictionary<PatrolHelicopter, int> _strafeCount;
    private static readonly System.Random _rng;
    private readonly int _groundLayer;
    private bool DisableHeli;
    private bool DisableDefaultHeliSpawns;
    private bool DisableDefaultChinookSpawns;
    private bool UseCustomLoot;
    private bool DisableGibs;
    private bool DisableNapalm;
    private bool AutoCallIfExists;
    private bool AutoCallIfExistsCH47;
    private bool DisableCratesDeath;
    private bool HelicopterCanShootWhileDying;
    private bool UseCustomHeliSpawns;
    private bool UseOldSpawning;
    private bool UseOldSpawningCH47;
    private bool SpawnHeliOnRestart;
    private bool SpawnChinookOnRestart;
    private bool SpawnHeliOnTarget;
    private float GlobalDamageMultiplier;
    private float HeliBulletDamageAmount;
    private float MainRotorHealth;
    private float TailRotorHealth;
    private float BaseHealth;
    private float BaseChinookHealth;
    private float HeliSpeed;
    private float HeliStartSpeed;
    private float HeliStartLength;
    private float HeliAccuracy;
    private float TimeBeforeUnlocking;
    private float TimeBeforeUnlockingHack;
    private float TurretFireRate;
    private float TurretBurstLength;
    private float TurretTimeBetweenBursts;
    private float TurretMaxRange;
    private float GibsTooHotLength;
    private float GibsHealth;
    private float TimeBetweenRockets;
    private float MinSpawnTime;
    private float MinSpawnTimeCH47;
    private float MaxSpawnTime;
    private float MaxSpawnTimeCH47;
    private float RocketDamageBlunt;
    private float RocketDamageExplosion;
    private float RocketExplosionRadius;
    private int MaxLootCrates;
    private int MaxHeliRockets;
    private int BulletSpeed;
    private int LifeTimeMinutes;
    private int LifeTimeMinutesCH47;
    private int MaxActiveHelicopters;
    private int HelicoptersToSpawn;
    private int ChinooksToSpawn;
    private Dictionary<string, float> Cds;
    private Dictionary<string, int> Limits;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private string GetMessage(string key, string steamId);
    private void Init();
    private void OnTerrainInitialized();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(BaseNetworkable entity);
    private object CanBeTargeted(BaseCombatEntity entity, MonoBehaviour monoTurret);
    private object OnHelicopterTarget(HelicopterTurret turret, BaseCombatEntity entity);
    private object CanHelicopterStrafeTarget(PatrolHelicopterAI entity, BasePlayer target);
    private object CanHelicopterStrafe(PatrolHelicopterAI entity);
    private object CanHelicopterTarget(PatrolHelicopterAI entity, BasePlayer player);
    private object CanHelicopterUseNapalm(PatrolHelicopterAI entity);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
    private void UpdateHeli(PatrolHelicopter heli, bool justCreated);
    private void UpdateChinook(CH47HelicopterAIController chinook, bool justCreated);
    private void FireRocket(PatrolHelicopterAI heliAI);
    private PatrolHelicopter callHeli(Vector3 coordinates, bool forced, bool setPositionAfterSpawn);
    private CH47HelicopterAIController callChinook(Vector3 coordinates, bool forced);
    private List<PatrolHelicopter> callHelis(int amount, Vector3 coordinates, bool forced, bool setPositionAfterSpawn);
    private List<CH47HelicopterAIController> callChinooks(int amount, Vector3 coordinates, bool forced);
    private PatrolHelicopter callCoordinates(Vector3 coordinates);
    private void UpdateTurrets(PatrolHelicopterAI helicopter);
    private int KillAllHelis(bool isForced);
    private int KillAllChinooks(bool isForced);
    [ChatCommand("helispawn")]
    private void cmdHeliSpawns(BasePlayer player, string command, string[] args);
    private void cmdUnlockCrates(IPlayer player, string command, string[] args);
    private void cmdDebug(IPlayer player, string command, string[] args);
    private void cmdNextHeli(IPlayer player, string command, string[] args);
    private void cmdDropCH47Crate(IPlayer player, string command, string[] args);
    private void cmdTeleportHeli(IPlayer player, string command, string[] args);
    private object CanPlayerCallHeli(BasePlayer player, bool ch47);
    [ChatCommand("callheli")]
    private void cmdCallToPlayer(BasePlayer player, string command, string[] args);
    [ChatCommand("callch47")]
    private void cmdCallCH47(BasePlayer player, string command, string[] args);
    private void cmdKillHeli(IPlayer player, string command, string[] args);
    private void cmdKillCH47(IPlayer player, string command, string[] args);
    private void cmdUpdateHelicopters(IPlayer player, string command, string[] args);
    private void cmdStrafeHeli(IPlayer player, string command, string[] args);
    private void cmdDestChangeHeli(IPlayer player, string command, string[] args);
    private void cmdKillFB(IPlayer player, string command, string[] args);
    private void cmdKillGibs(IPlayer player, string command, string[] args);
    [ConsoleCommand("callheli")]
    private void consoleCallHeli(ConsoleSystem.Arg arg);
    [ConsoleCommand("callch47")]
    private void consoleCallCH47(ConsoleSystem.Arg arg);
    private TimeSpan GetNextHeliTime();
    private TimeSpan GetNextCH47Time();
    private Timer GetHeliSpawnTimer(float heliSpawnTime);
    private Timer GetChinookSpawnTimer(float chinookSpawnTime);
    private float GetAdjustedSecondsToSpawn(float timeSecs);
    private string ReadableTimeSpan(TimeSpan span, string stringFormat);
    private bool TeleportPlayer(BasePlayer player, Vector3 dest, bool distChecks, bool doSleep);
    private float GetRandomSpawnTime(bool ch47);
    private void StartStrafe(PatrolHelicopterAI heli, Vector3 target, bool useNapalm);
    private void SetDestination(PatrolHelicopterAI heli, Vector3 target);
    private void SetDestination(CH47HelicopterAIController heli, Vector3 target);
    private void ToggleCH47Event(bool value);
    private void ToggleHeliEvent(bool value);
    private Action _checkHelicopter;
    private Action CheckHelicopter { get; set; }
    private void UnlockCrate(LockedByEntCrate crate);
    private void UnlockCrate(HackableLockedCrate crate);
    private int HeliCount { get; set; }
    private int CH47Count { get; set; }
    private CooldownInfo GetCooldownInfo(ulong userId);
    private void SendNoPerms(IPlayer player);
    private void SendNoPerms(BasePlayer player);
    private string GetNoPerms(string userID);
    private Vector3 GetGround(Vector3 sourcePos);
    public static Vector3 GetVector3FromString(string vectorStr);
    private int KillAllFireballs();
    private int KillAllGibs();
    private T GetConfig(string name, T defaultValue);
    private void SetConfig(string name, T value);
    private bool HasPerms(string userId, string perm);
    private int Fitness(string individual, string target);
    private int ExactMatch(string comp1, string comp2, StringComparison options);
    public const string VALID_CHARACTERS;
    public string[] GetValidCharactersString();
    private string CleanPlayerName(string str);
    private BasePlayer FindPlayerByPartialName(string name, bool sleepers);
    private BasePlayer FindPlayerByPartialName(string name, bool sleepers, BasePlayer[] ignore);
    private BasePlayer FindPlayerByID(ulong userID);
    private void RemoveFromWorld(Item item);
    private bool CheckBoundaries(float x, float y, float z);
    private float GetLowestCooldown(BasePlayer player, bool ch47);
    private int GetHighestLimit(BasePlayer player, bool ch47);
    private bool IgnoreCooldown(BasePlayer player);
    private bool IgnoreLimits(BasePlayer player);
    private class StoredData
    {
        public List<BoxInventory> HeliInventoryLists;
        public StoredData();
    }

    private class StoredData2
    {
        public Dictionary<string, float> WeaponList;
        public StoredData2();
    }

    private class StoredData3
    {
        public List<CooldownInfo> cooldownList;
        public StoredData3();
    }

    public class HelicopterSpawn
    {
        public HeliType Type;
        [JsonRequired]
        private string _pos;
        [JsonIgnore]
        public Vector3 Position { get; set; }
        public string Name { get; set; }
        public HelicopterSpawn();
        public HelicopterSpawn(Vector3 position, HeliType type);
    }

    private class StoredData4
    {
        public List<HelicopterSpawn> HelicopterSpawns;
        public StoredData4();
    }

    public HelicopterSpawn FindSpawn(string name, StringComparison comparison);
    private class CooldownInfo
    {
        public string LastCallDay { get; set; }
        public string CooldownTime { get; set; }
        public int TimesCalled { get; set; }
        public string LastCallDayCH47 { get; set; }
        public string CooldownTimeCH47 { get; set; }
        public int TimesCalledCH47 { get; set; }
        public ulong UserID { get; set; }
        public CooldownInfo();
        public CooldownInfo(BasePlayer newPlayer);
        public CooldownInfo(string userID);
        public CooldownInfo(ulong userID);
    }

    private class BoxInventory
    {
        public List<ItemDef> lootBoxContents;
        public BoxInventory();
        public BoxInventory(List<ItemDef> list);
        public BoxInventory(List<Item> list);
        public BoxInventory(string name, int amount, int amountMin, int amountMax, ulong skinID);
    }

    private class ItemDef
    {
        public string name;
        public int amountMin;
        public int amountMax;
        public int amount;
        public ulong skinID;
        public ItemDef();
        public ItemDef(string name, int amount, ulong skinID);
    }

    private void LoadLootData();
    private void LoadWeaponData();
    private void LoadHeliSpawns();
    private void SaveLootData();
    private void SaveWeaponData();
    private void SaveCooldownData();
    private void SaveSpawnData();
}

private class StoredData
{
    public List<BoxInventory> HeliInventoryLists;
    public StoredData();
}

private class StoredData2
{
    public Dictionary<string, float> WeaponList;
    public StoredData2();
}

private class StoredData3
{
    public List<CooldownInfo> cooldownList;
    public StoredData3();
}

public class HelicopterSpawn
{
    public HeliType Type;
    [JsonRequired]
    private string _pos;
    [JsonIgnore]
    public Vector3 Position { get; set; }
    public string Name { get; set; }
    public HelicopterSpawn();
    public HelicopterSpawn(Vector3 position, HeliType type);
}

private class StoredData4
{
    public List<HelicopterSpawn> HelicopterSpawns;
    public StoredData4();
}

private class CooldownInfo
{
    public string LastCallDay { get; set; }
    public string CooldownTime { get; set; }
    public int TimesCalled { get; set; }
    public string LastCallDayCH47 { get; set; }
    public string CooldownTimeCH47 { get; set; }
    public int TimesCalledCH47 { get; set; }
    public ulong UserID { get; set; }
    public CooldownInfo();
    public CooldownInfo(BasePlayer newPlayer);
    public CooldownInfo(string userID);
    public CooldownInfo(ulong userID);
}

private class BoxInventory
{
    public List<ItemDef> lootBoxContents;
    public BoxInventory();
    public BoxInventory(List<ItemDef> list);
    public BoxInventory(List<Item> list);
    public BoxInventory(string name, int amount, int amountMin, int amountMax, ulong skinID);
}

private class ItemDef
{
    public string name;
    public int amountMin;
    public int amountMax;
    public int amount;
    public ulong skinID;
    public ItemDef();
    public ItemDef(string name, int amount, ulong skinID);
}


```

---

## HelicopterHover by 0x89A - Enables minicopter, scrap transport helicopter and chinook to hover without pilot

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Helicopter Hover", "0x89A", "2.0.8")]
[Description("Allows minicopters to hover without driver on command")]
 class HelicopterHover : RustPlugin
{
    private static HelicopterHover _plugin;
    private static Configuration _config;
    private Dictionary<int, HoveringComponent> _helicopters;
    private const string CanHover;
     void Init();
     void Unload();
     void OnServerInitialized();
    [ConsoleCommand("helicopterhover.hover")]
     void ConsoleHover(ConsoleSystem.Arg args);
    [ChatCommand("hover")]
     void Hover(BasePlayer player);
     void OnEntitySpawned(BaseHelicopter helicopter);
     void OnEntityMounted(BaseMountable mount, BasePlayer player);
     void OnEntityDismounted(BaseMountable mount, BasePlayer player);
     void OnServerCommand(ConsoleSystem.Arg args);
    private class HoveringComponent : MonoBehaviour
    {
        private BaseHelicopter _helicopter;
        private PlayerHelicopter _playerHelicopter;
         Rigidbody _rb;
         Timer _timedHoverTimer;
         Timer _fuelUseTimer;
         Coroutine _hoverCoroutine;
         VehicleEngineController<PlayerHelicopter> _engineController;
        public bool IsHovering { get; set; }
         void Awake();
        public void ToggleHover();
        public void StartHover();
        public void StopHover();
         IEnumerator HoveringCoroutine();
         void OnDestroy();
    }

     class Configuration
    {
        [JsonProperty(PropertyName = "Broadcast message on mounted")]
        public bool EnterBroadcast;
        [JsonProperty(PropertyName = "Permissions")]
        public PermissionClass Permission;
        [JsonProperty(PropertyName = "Hovering")]
        public HoveringClass Hovering;
        public class PermissionClass
        {
            [JsonProperty(PropertyName = "Minicopter can hover")]
            public bool MiniCanHover;
            [JsonProperty(PropertyName = "Scrap Transport Helicopter can hover")]
            public bool ScrapheliCanHover;
            [JsonProperty(PropertyName = "Chinook can hover")]
            public bool ChinookCanHover;
            [JsonProperty(PropertyName = "Enable hover with two occupants")]
            public bool EnableHoverWithTwoOccupants;
            [JsonProperty(PropertyName = "Passenger can toggle hover")]
            public bool PassengerToggle;
        }

        public class HoveringClass
        {
            [JsonProperty(PropertyName = "Disable hover on dismount")]
            public bool DisableHoverOnDismount;
            [JsonProperty(PropertyName = "Use fuel while hovering")]
            public bool UseFuelOnHover;
            [JsonProperty(PropertyName = "Keep engine on when hovering")]
            public bool KeepEngineOnHover;
            [JsonProperty(PropertyName = "Enable helicopter rotation on hover")]
            public bool EnableRotationOnHover;
            [JsonProperty(PropertyName = "Disable hover on change seats")]
            public bool DisableHoverOnSeat;
            [JsonProperty(PropertyName = "Hover on seat change")]
            public bool HoverOnSeatSwitch;
            [JsonProperty(PropertyName = "Timed hover")]
            public bool TimedHover;
            [JsonProperty(PropertyName = "Timed hover duration")]
            public float HoverDuration;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
}

private class HoveringComponent : MonoBehaviour
{
    private BaseHelicopter _helicopter;
    private PlayerHelicopter _playerHelicopter;
     Rigidbody _rb;
     Timer _timedHoverTimer;
     Timer _fuelUseTimer;
     Coroutine _hoverCoroutine;
     VehicleEngineController<PlayerHelicopter> _engineController;
    public bool IsHovering { get; set; }
     void Awake();
    public void ToggleHover();
    public void StartHover();
    public void StopHover();
     IEnumerator HoveringCoroutine();
     void OnDestroy();
}

 class Configuration
{
    [JsonProperty(PropertyName = "Broadcast message on mounted")]
    public bool EnterBroadcast;
    [JsonProperty(PropertyName = "Permissions")]
    public PermissionClass Permission;
    [JsonProperty(PropertyName = "Hovering")]
    public HoveringClass Hovering;
    public class PermissionClass
    {
        [JsonProperty(PropertyName = "Minicopter can hover")]
        public bool MiniCanHover;
        [JsonProperty(PropertyName = "Scrap Transport Helicopter can hover")]
        public bool ScrapheliCanHover;
        [JsonProperty(PropertyName = "Chinook can hover")]
        public bool ChinookCanHover;
        [JsonProperty(PropertyName = "Enable hover with two occupants")]
        public bool EnableHoverWithTwoOccupants;
        [JsonProperty(PropertyName = "Passenger can toggle hover")]
        public bool PassengerToggle;
    }

    public class HoveringClass
    {
        [JsonProperty(PropertyName = "Disable hover on dismount")]
        public bool DisableHoverOnDismount;
        [JsonProperty(PropertyName = "Use fuel while hovering")]
        public bool UseFuelOnHover;
        [JsonProperty(PropertyName = "Keep engine on when hovering")]
        public bool KeepEngineOnHover;
        [JsonProperty(PropertyName = "Enable helicopter rotation on hover")]
        public bool EnableRotationOnHover;
        [JsonProperty(PropertyName = "Disable hover on change seats")]
        public bool DisableHoverOnSeat;
        [JsonProperty(PropertyName = "Hover on seat change")]
        public bool HoverOnSeatSwitch;
        [JsonProperty(PropertyName = "Timed hover")]
        public bool TimedHover;
        [JsonProperty(PropertyName = "Timed hover duration")]
        public float HoverDuration;
    }

}

public class PermissionClass
{
    [JsonProperty(PropertyName = "Minicopter can hover")]
    public bool MiniCanHover;
    [JsonProperty(PropertyName = "Scrap Transport Helicopter can hover")]
    public bool ScrapheliCanHover;
    [JsonProperty(PropertyName = "Chinook can hover")]
    public bool ChinookCanHover;
    [JsonProperty(PropertyName = "Enable hover with two occupants")]
    public bool EnableHoverWithTwoOccupants;
    [JsonProperty(PropertyName = "Passenger can toggle hover")]
    public bool PassengerToggle;
}

public class HoveringClass
{
    [JsonProperty(PropertyName = "Disable hover on dismount")]
    public bool DisableHoverOnDismount;
    [JsonProperty(PropertyName = "Use fuel while hovering")]
    public bool UseFuelOnHover;
    [JsonProperty(PropertyName = "Keep engine on when hovering")]
    public bool KeepEngineOnHover;
    [JsonProperty(PropertyName = "Enable helicopter rotation on hover")]
    public bool EnableRotationOnHover;
    [JsonProperty(PropertyName = "Disable hover on change seats")]
    public bool DisableHoverOnSeat;
    [JsonProperty(PropertyName = "Hover on seat change")]
    public bool HoverOnSeatSwitch;
    [JsonProperty(PropertyName = "Timed hover")]
    public bool TimedHover;
    [JsonProperty(PropertyName = "Timed hover duration")]
    public float HoverDuration;
}


```

---

## HelicopterProtection by Nobu - Protects you from the helicopter and vice versa

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("Helicopter Protection", "Nobu", "1.0.2")]
[Description("Protects you from the helicopter and vice versa")]
 class HelicopterProtection : RustPlugin
{
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Time on server before heli targets them")]
        public double contime;
        [JsonProperty(PropertyName = "Protect players from heli")]
        public bool protectplayer;
        [JsonProperty(PropertyName = "Protect heli from players")]
        public bool protectheli;
    }

    private bool LoadConfigVariables();
     void Init();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
     bool? CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Time on server before heli targets them")]
    public double contime;
    [JsonProperty(PropertyName = "Protect players from heli")]
    public bool protectplayer;
    [JsonProperty(PropertyName = "Protect heli from players")]
    public bool protectheli;
}


```

---

## HeliEditor by Mabel - Edits how scrap helis, minicopters and player attack helis behave and work

```csharp
using Oxide.Core;
using System;
using UnityEngine;

Oxide.Plugins
[Info("Heli Editor", "Mabel", "1.0.0")]
[Description("Modify several characteristics of aircrafts")]
 class HeliEditor : RustPlugin
{
    private PluginConfig _config;
    public string permissionTakeoff;
    private class MinicopterSettings
    {
        public float maxHealth { get; set; }
        public bool invincible { get; set; }
        public bool blockExplosions { get; set; }
        public bool instantTakeoff { get; set; }
        public bool hydrophobic { get; set; }
    }

    private class ScrapheliSettings
    {
        public float maxHealth { get; set; }
        public bool invincible { get; set; }
        public bool blockExplosions { get; set; }
        public bool instantTakeoff { get; set; }
        public bool hydrophobic { get; set; }
    }

    private class AttackHelicopterSettings
    {
        public float maxHealth { get; set; }
        public bool invincible { get; set; }
        public bool blockExplosions { get; set; }
        public bool instantTakeoff { get; set; }
        public bool hydrophobic { get; set; }
    }

    private class PluginConfig
    {
        public MinicopterSettings minicopter { get; set; }
        public ScrapheliSettings scrapheli { get; set; }
        public AttackHelicopterSettings attackHelicopter { get; set; }
        public VersionNumber version { get; set; }
        public static PluginConfig DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private void Init();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseHelicopter entity);
     void OnEngineStarted(BaseVehicle vehicle, BasePlayer driver);
}

private class MinicopterSettings
{
    public float maxHealth { get; set; }
    public bool invincible { get; set; }
    public bool blockExplosions { get; set; }
    public bool instantTakeoff { get; set; }
    public bool hydrophobic { get; set; }
}

private class ScrapheliSettings
{
    public float maxHealth { get; set; }
    public bool invincible { get; set; }
    public bool blockExplosions { get; set; }
    public bool instantTakeoff { get; set; }
    public bool hydrophobic { get; set; }
}

private class AttackHelicopterSettings
{
    public float maxHealth { get; set; }
    public bool invincible { get; set; }
    public bool blockExplosions { get; set; }
    public bool instantTakeoff { get; set; }
    public bool hydrophobic { get; set; }
}

private class PluginConfig
{
    public MinicopterSettings minicopter { get; set; }
    public ScrapheliSettings scrapheli { get; set; }
    public AttackHelicopterSettings attackHelicopter { get; set; }
    public VersionNumber version { get; set; }
    public static PluginConfig DefaultConfig();
}


```

---

## HeliRide by ColonBlow - Allows players with permission to fly the Patrol Helicopter

```csharp
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Heli Ride", "ColonBlow", "1.1.17")]
[Description("Allows players to fly the Patrol Helicopter")]
public class HeliRide : RustPlugin
{
    [PluginReference]
    private Plugin Chute;
    private Plugin Vanish;
    public bool CockpitOverlay { get; set; }
    public bool CrossHair { get; set; }
    private static Dictionary<ulong, HeliData> HeliFlying;
    private static Dictionary<ulong, HeliDamage> DamagedHeli;
    private static Dictionary<ulong, HasParachute> AddParachute;
    public class HeliData
    {
        public BasePlayer player;
    }

    public class HeliDamage
    {
        public BasePlayer player;
    }

    public class HasParachute
    {
        public BasePlayer player;
    }

    private void Loaded();
    private bool Changed;
    private static bool ShowCockpitOverlay;
    private static bool ShowCrosshair;
    private static bool UseParachutes;
    private static bool SpawnCrates;
    private static bool UseAutoVanish;
    private static double RocketDelay;
    private static float RocketMax;
    private static float NapalmMax;
    private static double RocketNapalmReloadTime;
    private static float BulletDamage;
    private static string RocketPrefab;
    private static string NapalmPrefab;
    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private new void LoadDefaultMessages();
    [ChatCommand("flyheli")]
    private void chatFlyHeli(BasePlayer player, string command, string[] args);
    [ConsoleCommand("flyheli")]
    private void cmdConsoleFlyHeli(ConsoleSystem.Arg arg);
    [ConsoleCommand("showcockpit")]
    private void cmdConsoleShowCockpit(ConsoleSystem.Arg arg);
    [ConsoleCommand("hidecockpit")]
    private void cmdConsoleHideCockpit(ConsoleSystem.Arg arg);
    private void OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
    private void OnPlayerTick(BasePlayer player);
    private void AddHeli(BasePlayer player);
    private void Unload();
    private static void DestroyAll();
    private void RemoveHeliComponents(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private bool isAllowed(BasePlayer player, string perm);
    private class FlyHelicopter : MonoBehaviour
    {
        public BasePlayer player;
        public BaseEntity helicopterBase;
        private BaseEntity rockets;
        public PatrolHelicopterAI heliAI;
        public PatrolHelicopter heli;
        public HelicopterTurret heliturret;
        public InputState input;
        private RaycastHit hitInfo;
        public Vector3 PlayerPOS;
        public Vector3 target;
        public Vector3 CurrentPOS;
        private Vector3 direction;
        private float bulletDamage;
        private float rocketMax;
        private bool hasRockets;
        private float napalmMax;
        private bool hasNapalm;
        private double rocketcycletimer;
        private double reloadtimer;
        private double rocketDelay;
        private bool rocketcycle;
        private bool leftTubeFiredLast;
        private bool isReloading;
        private double rocketNapalmReploadTime;
        private void Awake();
        public void CockpitOverlay(BasePlayer player);
        public void CrosshairOverlay(BasePlayer player);
        public void DamageOverlay(BasePlayer player);
        public void HealthIndicator(BasePlayer player, float health);
        private void FixedUpdate();
        private void FireGuns(Vector3 target);
        private void FireRocket(bool leftTubeFiredLast, Vector3 direction, Vector3 PlayerPOS, bool isrocket);
        private Vector3 FindTarget(Vector3 target);
        public void DestroyCui(BasePlayer player);
        private void addplayerchute();
        public void OnDestroy();
    }

}

public class HeliData
{
    public BasePlayer player;
}

public class HeliDamage
{
    public BasePlayer player;
}

public class HasParachute
{
    public BasePlayer player;
}

private class FlyHelicopter : MonoBehaviour
{
    public BasePlayer player;
    public BaseEntity helicopterBase;
    private BaseEntity rockets;
    public PatrolHelicopterAI heliAI;
    public PatrolHelicopter heli;
    public HelicopterTurret heliturret;
    public InputState input;
    private RaycastHit hitInfo;
    public Vector3 PlayerPOS;
    public Vector3 target;
    public Vector3 CurrentPOS;
    private Vector3 direction;
    private float bulletDamage;
    private float rocketMax;
    private bool hasRockets;
    private float napalmMax;
    private bool hasNapalm;
    private double rocketcycletimer;
    private double reloadtimer;
    private double rocketDelay;
    private bool rocketcycle;
    private bool leftTubeFiredLast;
    private bool isReloading;
    private double rocketNapalmReploadTime;
    private void Awake();
    public void CockpitOverlay(BasePlayer player);
    public void CrosshairOverlay(BasePlayer player);
    public void DamageOverlay(BasePlayer player);
    public void HealthIndicator(BasePlayer player, float health);
    private void FixedUpdate();
    private void FireGuns(Vector3 target);
    private void FireRocket(bool leftTubeFiredLast, Vector3 direction, Vector3 PlayerPOS, bool isrocket);
    private Vector3 FindTarget(Vector3 target);
    public void DestroyCui(BasePlayer player);
    private void addplayerchute();
    public void OnDestroy();
}


```

---

## HeliSams by Whispers88 - Allows SAM Sites to target Patrol Helicopters and Chinooks

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using VLB;
using static SamSite;
using static BaseVehicle;

Oxide.Plugins
[Info("Heli Sams", "WhiteThunder & Whispers88", "2.1.3")]
[Description("Allows Sam Sites to target CH47 and Patrol Helicopters")]
internal class HeliSams : CovalencePlugin
{
    private const float DebugDrawDistance;
    private const string PermissionCh47Npc;
    private const string PermissionCh47Player;
    private const string PermissionPatrolHeli;
    private const string CH47NpcPrefab;
    private readonly object False;
    private static Configuration _config;
    private static uint _ch47NpcPrefabId;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(CH47Helicopter entity);
    private void OnEntitySpawned(PatrolHelicopter entity);
    private void OnEntityKill(CH47Helicopter entity);
    private void OnEntityKill(PatrolHelicopter entity);
    private object OnSamSiteTarget(SamSite samSite, SAMTargetComponent targetComponent);
    private void OnEntityTakeDamage(CH47Helicopter ch47, HitInfo info);
    private void OnEntityTakeDamage(PatrolHelicopter patrolHeli, HitInfo info);
    private static bool IsNpcCH47(CH47Helicopter ch47);
    private static bool IsOccupied(BaseCombatEntity entity, List<MountPointInfo> mountPoints);
    private static bool IsAuthed(BuildingPrivlidge cupboard, ulong userId);
    private static BuildingPrivlidge GetSamSiteToolCupboard(SamSite samSite);
    private bool ShouldTargetNpcCH47(SamSite samSite, CH47Helicopter ch47);
    private bool ShouldTargetPlayerCH47(SamSite samSite, CH47Helicopter ch47);
    private bool ShouldTargetPatrolHelicopter(SamSite samSite, PatrolHelicopter patrolHeli);
    private bool SamSiteHasPermission(SamSite samSite, string perm);
    private Vector3 PredictedPos(BaseEntity target, SamSite samSite, Vector3 targetVelocity, float projectileSpeedMultiplier);
    private void ShowRocketPath(Vector3 samSitePositon, Vector3 position);
    private void ShowRocketDamage(Vector3 position, float amount);
    private class SAMTargetComponent : FacepunchBehaviour, ISamSiteTarget
    {
        public static HashSet<SAMTargetComponent> SAMTargetComponents;
        public static void AddToEntity(BaseCombatEntity entity);
        public static void RemoveFromEntity(BaseCombatEntity entity);
        public BaseEntity Entity;
        public float TargetRangeSquared;
        private GameObject _child;
        private Transform _transform;
        private SamTargetType _targetType;
        private void Awake();
        private void OnDestroy();
        public Vector3 Position { get; set; }
        public SamTargetType SAMTargetType { get; set; }
        public bool isClient { get; set; }
        public bool IsValidSAMTarget(bool isStaticSamSite);
        public Vector3 CenterPoint();
        public Vector3 GetWorldVelocity();
        public bool IsVisible(Vector3 position, float distance);
    }

    private void OnSamSiteTargetScan(SamSite samSite, List<ISamSiteTarget> targetList);
    private void CanSamSiteShoot(SamSite samSite);
    [JsonObject(MemberSerialization.OptIn)]
    private class HeliSettings
    {
        [JsonProperty("Can be targeted by static SAM Sites")]
        public bool DeprecatedCanBeTargetedByStaticSamSites { get; set; }
        [JsonProperty("Can be targeted by static Sam Sites")]
        public bool CanBeTargetedByStaticSamSites;
        [JsonProperty("Targeting range")]
        public float TargetRange;
        [JsonProperty("Rocket speed multiplier")]
        public float RocketSpeedMultiplier;
        [JsonProperty("Rocket damage multiplier")]
        public float RocketDamageMultiplier;
        [JsonProperty("Seconds between rocket bursts")]
        public float SecondsBetweenBursts;
        private SamTargetType _targetType;
        public SamTargetType TargetType { get; set; }
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class PatrolHeliSettings : HeliSettings
    {
        [JsonProperty("Require cupboard auth for owned helicopters", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool RequireCupboardAuth;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class PlayerCH47Settings : HeliSettings
    {
        [JsonProperty("Check cupboard auth")]
        public bool CheckCupboardAuth;
    }

    private class Configuration : BaseConfiguration
    {
        [JsonProperty("NPC CH47 Helicopter")]
        public HeliSettings CH47Npc;
        [JsonProperty("Player CH47 Helicopter")]
        public PlayerCH47Settings CH47Player;
        [JsonProperty("Patrol Helicopter")]
        public PatrolHeliSettings PatrolHeli;
        [JsonProperty("Debug rocket prediction")]
        public bool DebugRocketPrediction;
        [JsonProperty("Debug rocket damage")]
        public bool DebugRocketDamage;
    }

    private Configuration GetDefaultConfig();
    private class BaseConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(BaseConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
}

private class SAMTargetComponent : FacepunchBehaviour, ISamSiteTarget
{
    public static HashSet<SAMTargetComponent> SAMTargetComponents;
    public static void AddToEntity(BaseCombatEntity entity);
    public static void RemoveFromEntity(BaseCombatEntity entity);
    public BaseEntity Entity;
    public float TargetRangeSquared;
    private GameObject _child;
    private Transform _transform;
    private SamTargetType _targetType;
    private void Awake();
    private void OnDestroy();
    public Vector3 Position { get; set; }
    public SamTargetType SAMTargetType { get; set; }
    public bool isClient { get; set; }
    public bool IsValidSAMTarget(bool isStaticSamSite);
    public Vector3 CenterPoint();
    public Vector3 GetWorldVelocity();
    public bool IsVisible(Vector3 position, float distance);
}

[JsonObject(MemberSerialization.OptIn)]
private class HeliSettings
{
    [JsonProperty("Can be targeted by static SAM Sites")]
    public bool DeprecatedCanBeTargetedByStaticSamSites { get; set; }
    [JsonProperty("Can be targeted by static Sam Sites")]
    public bool CanBeTargetedByStaticSamSites;
    [JsonProperty("Targeting range")]
    public float TargetRange;
    [JsonProperty("Rocket speed multiplier")]
    public float RocketSpeedMultiplier;
    [JsonProperty("Rocket damage multiplier")]
    public float RocketDamageMultiplier;
    [JsonProperty("Seconds between rocket bursts")]
    public float SecondsBetweenBursts;
    private SamTargetType _targetType;
    public SamTargetType TargetType { get; set; }
}

[JsonObject(MemberSerialization.OptIn)]
private class PatrolHeliSettings : HeliSettings
{
    [JsonProperty("Require cupboard auth for owned helicopters", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool RequireCupboardAuth;
}

[JsonObject(MemberSerialization.OptIn)]
private class PlayerCH47Settings : HeliSettings
{
    [JsonProperty("Check cupboard auth")]
    public bool CheckCupboardAuth;
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("NPC CH47 Helicopter")]
    public HeliSettings CH47Npc;
    [JsonProperty("Player CH47 Helicopter")]
    public PlayerCH47Settings CH47Player;
    [JsonProperty("Patrol Helicopter")]
    public PatrolHeliSettings PatrolHeli;
    [JsonProperty("Debug rocket prediction")]
    public bool DebugRocketPrediction;
    [JsonProperty("Debug rocket damage")]
    public bool DebugRocketDamage;
}

private class BaseConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}


```

---

## HeliScrap by Camoec - Call patrol helicopters with scrap

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using System;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Heli Scrap", "Camoec", "1.5.1")]
[Description("Call heli with scrap")]
public class HeliScrap : RustPlugin
{
    private PluginConfig _config;
    [PluginReference]
    private readonly Plugin Economics;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "ChatPrefix")]
        public string ChatPrefix;
        [JsonProperty(PropertyName = "Command")]
        public string Command;
        [JsonProperty(PropertyName = "Scrap Amount (If Economics is enabled, use RP)")]
        public int ScrapAmount;
        [JsonProperty(PropertyName = "UseEconomics")]
        public bool UseEconomics;
        [JsonProperty(PropertyName = "UsePermission")]
        public bool UsePermission;
        [JsonProperty(PropertyName = "MaxSpawnedHelis")]
        public int MaxSpawnedHelis;
        [JsonProperty(PropertyName = "Player Cooldown (in seconds)")]
        public int Cooldown;
        [JsonProperty(PropertyName = "Global Cooldown (in seconds)")]
        public int GCooldown;
    }

    private const string UsePerm;
    private const string HELI_PREFAB;
    private HashSet<PatrolHelicopter> activeHelis;
    private Dictionary<BasePlayer, DateTime> playerCooldown;
    private DateTime lastCall;
    private bool RemoveCost(BasePlayer player);
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
     void Loaded();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string userid);
    private void Init();
    private void CallCommand(BasePlayer player);
    private void OnServerInitialized();
     void OnEntitySpawned(PatrolHelicopter entity);
    private void CheckHelis();
    private bool CanRemoveItem(BasePlayer player, int itemid, int amount);
    public void RemoveItemsFromInventory(BasePlayer player, int itemid, int amount);
    private PatrolHelicopter callHeli(Vector3 coordinates, bool setPositionAfterSpawn);
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "ChatPrefix")]
    public string ChatPrefix;
    [JsonProperty(PropertyName = "Command")]
    public string Command;
    [JsonProperty(PropertyName = "Scrap Amount (If Economics is enabled, use RP)")]
    public int ScrapAmount;
    [JsonProperty(PropertyName = "UseEconomics")]
    public bool UseEconomics;
    [JsonProperty(PropertyName = "UsePermission")]
    public bool UsePermission;
    [JsonProperty(PropertyName = "MaxSpawnedHelis")]
    public int MaxSpawnedHelis;
    [JsonProperty(PropertyName = "Player Cooldown (in seconds)")]
    public int Cooldown;
    [JsonProperty(PropertyName = "Global Cooldown (in seconds)")]
    public int GCooldown;
}


```

---

## HeliSupport by FastBurst - Get heli support in battle

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Heli Support", "Vault Boy", "1.0.0")]
[Description("Call a support from heli forces")]
public class HeliSupport : RustPlugin
{
    private const string nocdperm;
    private const string heliprefab;
    private void OnServerInitialized();
    private bool CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
    private bool CanHelicopterUseNapalm(PatrolHelicopterAI heli);
    private Dictionary<uint, ulong> helis;
    [ChatCommand("heli")]
    private void CmdCall(BasePlayer player);
    private void CallHeli(BasePlayer player);
}


```

---

## HeliVote by k1lly0u - Allows players to open a vote to call helicopters

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;

Oxide.Plugins
[Info("HeliVote", "k1lly0u", "0.1.32", ResourceId = 1665)]
 class HeliVote : RustPlugin
{
     bool Changed;
    private List<ulong> receivedYes;
    private List<ulong> receivedNo;
    private List<BaseEntity> currentHelis;
    private bool voteOpen;
    private bool helisActive;
    private bool timeBetween;
    private BasePlayer initiator;
     void Loaded();
     void OnServerInitialized();
     void LoadDefaultConfig();
     void Unload();
     void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
    private bool alreadyVoted(BasePlayer player);
    private bool TallyVotes();
    private void voteEnd(int amount);
    private void clearData();
    private void CallHeli(int amount);
    private void VoteTimer(int amount);
    private void msgAll(string left);
    private bool CheckIfStillExist();
    [ChatCommand("helivote")]
    private void cmdHeiVote(BasePlayer player, string command, string[] args);
    [ConsoleCommand("helivote")]
    private void ccmdVote(ConsoleSystem.Arg arg);
    private bool canVote(BasePlayer player);
     bool isAuth(BasePlayer player);
     bool isAuthCon(ConsoleSystem.Arg arg);
    static float requiredVotesPercentage;
    static bool useMajorityRules;
    static int voteOpenTimer;
    static bool displayProgress;
    static int auth;
    static int minBetween;
    static int maxAmount;
    static bool heliToInit;
    static bool usePerms;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     Dictionary<string, string> messages;
}


```

---

## HelpText by Calytic - Allows players to use /help to get custom help and help info from plugins

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("HelpText", "Calytic", "2.0.51")]
 class HelpText : CovalencePlugin
{
    private bool UseCustomHelpText;
    private bool AllowHelpTextFromOtherPlugins;
    private int BreakAfter;
    private Dictionary<string, Dictionary<string, object>> Pages;
    private void Loaded();
    protected override void LoadDefaultConfig();
    protected Dictionary<string, object> GetDefaultPages();
    protected Dictionary<string, object> GetEmptyPages();
    [Command("help")]
     void cmdHelp(IPlayer player, string command, string[] args);
     void CheckConfig();
    protected void ReloadConfig();
    private T GetConfig(string name, T defaultValue);
    private T GetConfig(string name, string name2, T defaultValue);
}


```

---

## HempDaddy by TacoSauce - Modifies gather rate of hemp plants when planted in planters

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Hemp Daddy", "TacoSauce", "1.0.1")]
[Description("Modifies gather rate of hemp plants when planted in planters.")]
 class HempDaddy : RustPlugin
{
    private void OnGrowableGathered(GrowableEntity growable, Item item, BasePlayer player);
    private confData config;
    protected override void LoadDefaultConfig();
    private void Init();
    private new void SaveConfig();
    public class confData
    {
        [JsonProperty("Cloth Mulitplier")]
        public int modifier;
    }

}

public class confData
{
    [JsonProperty("Cloth Mulitplier")]
    public int modifier;
}


```

---

## HideAndSeek by VisEntities - The classic game mode of Hide and Seek. Hide from the imposter until only one crewmate remains

```csharp
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Hide and Seek", "Wulf/lukespragg", "0.1.3", ResourceId = 1421)]
[Description("The classic game(mode) of hide and seek, as props")]
public class HideAndSeek : CovalencePlugin
{
    protected override void LoadDefaultMessages();
    private static System.Random random;
    private static readonly string[] animalTaunts;
    private static readonly string[] buildingTaunts;
    private static readonly string[] otherTaunts;
    private static readonly string[] weaponTaunts;
    private Dictionary<IPlayer, BaseEntity> props;
    private const string permAllow;
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void SetPropFlags(BasePlayer player);
    private void UnsetPropFlags(BasePlayer player);
    private void HidePlayer(IPlayer player);
    private void UnhidePlayer(IPlayer player);
    [Command("hide")]
    private void HideCommand(IPlayer player, string command, string[] args);
    [Command("unhide")]
    private void UnhideCommand(IPlayer player, string command, string[] args);
    [Command("taunt")]
    private void TauntCommand(IPlayer player, string command, string[] args);
    private object OnEntityTakeDamage(BaseEntity entity, HitInfo info);
    private void OnEntityDeath(BaseEntity entity);
    private void OnEntitySpawned(BaseNetworkable entity);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object OnPlayerInput(BasePlayer player, InputState input);
     string tauntPanel;
    private void TauntButton(BasePlayer player, string text);
    private void Unload();
    private static BaseEntity FindObject(Ray ray, float distance);
    private string Lang(string key, string id, object[] args);
}


```

---

## HighlightPlayer by Freakyy - Marks players on the map after using a command

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Highlight Player", "Freakyy", "1.2.1")]
[Description("Marks players on the map after using a command. So other players can find the target for pvp action.")]
 class HighlightPlayer : RustPlugin
{
    public List<MapMarkerGenericRadius> Markers;
    public List<BasePlayer> Players_To_Highlight;
    private string permission_can_highlight_other_players;
    private string permission_cant_be_highlighted_by_other_players;
    protected override void LoadDefaultMessages();
     void OnServerInitialized();
     void Init();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
    [ChatCommand("hme")]
     void highlight(BasePlayer player);
    [ChatCommand("uhme")]
     void unhighlight(BasePlayer player);
    [ChatCommand("h")]
    private void highlightOtherPlayer(BasePlayer player, string command, string[] args);
    private void unhighlight_player(BasePlayer player);
    private void highlight_player(BasePlayer player);
    private bool has_user_permission(IPlayer player, string permission_name);
     void clearplayermarker(BasePlayer player);
     void clearallmarkers();
     void update_player_position_on_map();
    private new void LoadConfig();
    private void GetConfig(T variable, string[] path);
    protected override void LoadDefaultConfig();
}


```

---

## HighWallBarricades by 2CHEVSKII - Configurable decay speed for certain barricades

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Rust;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("High Wall Barricades", "2CHEVSKII", "2.3.1")]
[Description("Makes hgih external walls decay")]
public class HighWallBarricades : RustPlugin
{
    private class BarricadeDecay : FacepunchBehaviour
    {
        private float defaultProtection;
        private float lastTickTime;
        private DecayEntity Barricade { get; set; }
         void Start();
        private void CheckDecay();
        public void RemoveComponent();
        private void UpdateTickTime();
        private void TickDecay();
        private void OnDestroy();
    }

    private Configuration Settings { get; set; }
    private static HighWallBarricades Singleton { get; set; }
    private HashSet<BarricadeDecay> Barricades { get; set; }
    private class Configuration
    {
        [JsonProperty(PropertyName = "Time between decay ticks")]
        internal int DecayTime { get; set; }
        [JsonProperty(PropertyName = "Decay tick damage")]
        internal float DecayDamage { get; set; }
        [JsonProperty(PropertyName = "Barricade initial health")]
        internal float InitialHealth { get; set; }
        [JsonProperty(PropertyName = "Should barricades decay while inside the cupboard range")]
        internal bool DecayInCupRange { get; set; }
        [JsonProperty(PropertyName = "Check if owner of barricade is authorized")]
        internal bool CheckBarricadeOwner { get; set; }
        [JsonProperty(PropertyName = "Disable standart decay for barricades")]
        internal bool DisableStandartDecay { get; set; }
        [JsonProperty(PropertyName = "Enabled types of barricades")]
        internal Dictionary<string, bool> EnabledEntities { get; set; }
        [JsonProperty(PropertyName = "Configuration version (Needed for auto-update, don't modify)")]
        internal VersionNumber ConfigVersion { get; set; }
    }

    private Configuration GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnServerInitialized();
    private IEnumerator FindAllBarricades();
    private bool NeedToDecay(GameObject obj);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private void Unload();
}

private class BarricadeDecay : FacepunchBehaviour
{
    private float defaultProtection;
    private float lastTickTime;
    private DecayEntity Barricade { get; set; }
     void Start();
    private void CheckDecay();
    public void RemoveComponent();
    private void UpdateTickTime();
    private void TickDecay();
    private void OnDestroy();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Time between decay ticks")]
    internal int DecayTime { get; set; }
    [JsonProperty(PropertyName = "Decay tick damage")]
    internal float DecayDamage { get; set; }
    [JsonProperty(PropertyName = "Barricade initial health")]
    internal float InitialHealth { get; set; }
    [JsonProperty(PropertyName = "Should barricades decay while inside the cupboard range")]
    internal bool DecayInCupRange { get; set; }
    [JsonProperty(PropertyName = "Check if owner of barricade is authorized")]
    internal bool CheckBarricadeOwner { get; set; }
    [JsonProperty(PropertyName = "Disable standart decay for barricades")]
    internal bool DisableStandartDecay { get; set; }
    [JsonProperty(PropertyName = "Enabled types of barricades")]
    internal Dictionary<string, bool> EnabledEntities { get; set; }
    [JsonProperty(PropertyName = "Configuration version (Needed for auto-update, don't modify)")]
    internal VersionNumber ConfigVersion { get; set; }
}


```

---

## HitIcon by FastBurst - Configurable UI icon and text of damage amount when you hit player|friend|clan member|headshot

```csharp
using UnityEngine;
using System;
using Oxide.Game.Rust.Cui;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.IO;
using Newtonsoft.Json;

Oxide.Plugins
[Info("HitIcon", "FastBurst", "2.0.2")]
[Description("Configurable precached icon when you hit player|friend|clan member|headshot")]
 class HitIcon : RustPlugin
{
    [PluginReference]
     Plugin Friends;
     Plugin Clans;
    private static HitIcon ins { get; set; }
    private ImageCache _imageAssets;
    private GameObject _hitObject;
    private StoredData _storedData;
    private Dictionary<ulong, UIHandler> _playersUIHandler;
    private void OnServerInitialized();
    private void Loaded();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hitinfo);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void InitializeAPI();
    private bool AreFriends(string playerId, string friendId);
    private bool AreClanMates(ulong playerID, ulong victimID);
    private void CacheImage();
     class ImageCache : MonoBehaviour
    {
        public Dictionary<string, string> imageFiles;
        private List<Queue> queued;
         class Queue
        {
            public string Url { get; set; }
            public string Name { get; set; }
        }

        public void OnDestroy();
        public void GetImage(string name, string url);
         IEnumerator WaitForRequest(Queue queue);
        public void Process();
    }

    private string FetchImage(string name);
    private void Download();
    private void Png(BasePlayer player, string name, string image, string start, string end, string color);
    private void Dmg(BasePlayer player, string name, string text, string start, string end, string color, int size);
     class UIHandler : MonoBehaviour
    {
        public BasePlayer player;
        public bool isDestroyed;
        private void Awake();
        public void DestroyUI();
        public void Destroy();
    }

    private void SendHit(BasePlayer attacker, HitInfo info);
    private void SendDeath(BaseCombatEntity entity, HitInfo info);
    private void GuiDisplay(BasePlayer player, string color, HitInfo hitinfo, bool isKill, string whatIsIt, bool isHead);
    private UIHandler GetUIHandler(BasePlayer player);
    [ChatCommand("hit")]
    private void ToggleHit(BasePlayer player);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Color")]
        public ColorOptions ColorSettings { get; set; }
        [JsonProperty(PropertyName = "Configuration")]
        public ConfigOptions ConfigSettings { get; set; }
        public class ColorOptions
        {
            [JsonProperty(PropertyName = "Hit clan member color")]
            public string colorClan { get; set; }
            [JsonProperty(PropertyName = "Hit friend color")]
            public string colorFriend { get; set; }
            [JsonProperty(PropertyName = "Hit head color")]
            public string colorHead { get; set; }
            [JsonProperty(PropertyName = "Hit body color")]
            public string colorBody { get; set; }
            [JsonProperty(PropertyName = "Hit NPC body color")]
            public string colorNpc { get; set; }
            [JsonProperty(PropertyName = "Hit Death body color")]
            public string colorDeath { get; set; }
            [JsonProperty(PropertyName = "Text damage color")]
            public string colorDamage { get; set; }
            [JsonProperty(PropertyName = "Text head damage color")]
            public string colorHeadDamage { get; set; }
        }

        public class ConfigOptions
        {
            [JsonProperty(PropertyName = "Damage text size")]
            public int dmgTextSize { get; set; }
            [JsonProperty(PropertyName = "Show clan member damage")]
            public bool showClanDamage { get; set; }
            [JsonProperty(PropertyName = "Show damage")]
            public bool showDamage { get; set; }
            [JsonProperty(PropertyName = "Show death kill")]
            public bool showDeathSkull { get; set; }
            [JsonProperty(PropertyName = "Show friend damage")]
            public bool showFriendDamage { get; set; }
            [JsonProperty(PropertyName = "Show hit icon")]
            public bool showHit { get; set; }
            [JsonProperty(PropertyName = "Show hits/deaths on NPC (Bears, wolfs, etc.)")]
            public bool showNpc { get; set; }
            [JsonProperty(PropertyName = "Text Font")]
            public string dmgFont { get; set; }
            [JsonProperty(PropertyName = "Text Outline Color")]
            public string dmgOutlineColor { get; set; }
            [JsonProperty(PropertyName = "Text Outline Distance")]
            public string dmgOutlineDistance { get; set; }
            [JsonProperty(PropertyName = "Time to destroy")]
            public float timeToDestroy { get; set; }
            [JsonProperty(PropertyName = "Use Clans")]
            public bool useClans { get; set; }
            [JsonProperty(PropertyName = "Use Friends")]
            public bool useFriends { get; set; }
            [JsonProperty(PropertyName = "Use sound when clan/friends get attacked")]
            public bool useSound { get; set; }
            [JsonProperty(PropertyName = "When clan/friends get attacked sound fx")]
            public string mateSound { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
     T GetConfig(string name, T defaultValue);
    private class StoredData
    {
        public List<ulong> DisabledUsers;
    }

    private void SaveData();
    private void LoadData();
    private void InitLanguage();
}

 class ImageCache : MonoBehaviour
{
    public Dictionary<string, string> imageFiles;
    private List<Queue> queued;
     class Queue
    {
        public string Url { get; set; }
        public string Name { get; set; }
    }

    public void OnDestroy();
    public void GetImage(string name, string url);
     IEnumerator WaitForRequest(Queue queue);
    public void Process();
}

 class Queue
{
    public string Url { get; set; }
    public string Name { get; set; }
}

 class UIHandler : MonoBehaviour
{
    public BasePlayer player;
    public bool isDestroyed;
    private void Awake();
    public void DestroyUI();
    public void Destroy();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Color")]
    public ColorOptions ColorSettings { get; set; }
    [JsonProperty(PropertyName = "Configuration")]
    public ConfigOptions ConfigSettings { get; set; }
    public class ColorOptions
    {
        [JsonProperty(PropertyName = "Hit clan member color")]
        public string colorClan { get; set; }
        [JsonProperty(PropertyName = "Hit friend color")]
        public string colorFriend { get; set; }
        [JsonProperty(PropertyName = "Hit head color")]
        public string colorHead { get; set; }
        [JsonProperty(PropertyName = "Hit body color")]
        public string colorBody { get; set; }
        [JsonProperty(PropertyName = "Hit NPC body color")]
        public string colorNpc { get; set; }
        [JsonProperty(PropertyName = "Hit Death body color")]
        public string colorDeath { get; set; }
        [JsonProperty(PropertyName = "Text damage color")]
        public string colorDamage { get; set; }
        [JsonProperty(PropertyName = "Text head damage color")]
        public string colorHeadDamage { get; set; }
    }

    public class ConfigOptions
    {
        [JsonProperty(PropertyName = "Damage text size")]
        public int dmgTextSize { get; set; }
        [JsonProperty(PropertyName = "Show clan member damage")]
        public bool showClanDamage { get; set; }
        [JsonProperty(PropertyName = "Show damage")]
        public bool showDamage { get; set; }
        [JsonProperty(PropertyName = "Show death kill")]
        public bool showDeathSkull { get; set; }
        [JsonProperty(PropertyName = "Show friend damage")]
        public bool showFriendDamage { get; set; }
        [JsonProperty(PropertyName = "Show hit icon")]
        public bool showHit { get; set; }
        [JsonProperty(PropertyName = "Show hits/deaths on NPC (Bears, wolfs, etc.)")]
        public bool showNpc { get; set; }
        [JsonProperty(PropertyName = "Text Font")]
        public string dmgFont { get; set; }
        [JsonProperty(PropertyName = "Text Outline Color")]
        public string dmgOutlineColor { get; set; }
        [JsonProperty(PropertyName = "Text Outline Distance")]
        public string dmgOutlineDistance { get; set; }
        [JsonProperty(PropertyName = "Time to destroy")]
        public float timeToDestroy { get; set; }
        [JsonProperty(PropertyName = "Use Clans")]
        public bool useClans { get; set; }
        [JsonProperty(PropertyName = "Use Friends")]
        public bool useFriends { get; set; }
        [JsonProperty(PropertyName = "Use sound when clan/friends get attacked")]
        public bool useSound { get; set; }
        [JsonProperty(PropertyName = "When clan/friends get attacked sound fx")]
        public string mateSound { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class ColorOptions
{
    [JsonProperty(PropertyName = "Hit clan member color")]
    public string colorClan { get; set; }
    [JsonProperty(PropertyName = "Hit friend color")]
    public string colorFriend { get; set; }
    [JsonProperty(PropertyName = "Hit head color")]
    public string colorHead { get; set; }
    [JsonProperty(PropertyName = "Hit body color")]
    public string colorBody { get; set; }
    [JsonProperty(PropertyName = "Hit NPC body color")]
    public string colorNpc { get; set; }
    [JsonProperty(PropertyName = "Hit Death body color")]
    public string colorDeath { get; set; }
    [JsonProperty(PropertyName = "Text damage color")]
    public string colorDamage { get; set; }
    [JsonProperty(PropertyName = "Text head damage color")]
    public string colorHeadDamage { get; set; }
}

public class ConfigOptions
{
    [JsonProperty(PropertyName = "Damage text size")]
    public int dmgTextSize { get; set; }
    [JsonProperty(PropertyName = "Show clan member damage")]
    public bool showClanDamage { get; set; }
    [JsonProperty(PropertyName = "Show damage")]
    public bool showDamage { get; set; }
    [JsonProperty(PropertyName = "Show death kill")]
    public bool showDeathSkull { get; set; }
    [JsonProperty(PropertyName = "Show friend damage")]
    public bool showFriendDamage { get; set; }
    [JsonProperty(PropertyName = "Show hit icon")]
    public bool showHit { get; set; }
    [JsonProperty(PropertyName = "Show hits/deaths on NPC (Bears, wolfs, etc.)")]
    public bool showNpc { get; set; }
    [JsonProperty(PropertyName = "Text Font")]
    public string dmgFont { get; set; }
    [JsonProperty(PropertyName = "Text Outline Color")]
    public string dmgOutlineColor { get; set; }
    [JsonProperty(PropertyName = "Text Outline Distance")]
    public string dmgOutlineDistance { get; set; }
    [JsonProperty(PropertyName = "Time to destroy")]
    public float timeToDestroy { get; set; }
    [JsonProperty(PropertyName = "Use Clans")]
    public bool useClans { get; set; }
    [JsonProperty(PropertyName = "Use Friends")]
    public bool useFriends { get; set; }
    [JsonProperty(PropertyName = "Use sound when clan/friends get attacked")]
    public bool useSound { get; set; }
    [JsonProperty(PropertyName = "When clan/friends get attacked sound fx")]
    public string mateSound { get; set; }
}

private class StoredData
{
    public List<ulong> DisabledUsers;
}


```

---

## HolidayLoot by FastBurst - Modify the loot tables for the Christmas Presents, Halloween Bags and Easter Eggs

```csharp
using System;
using Oxide.Core;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("HolidayLoot", "FastBurst", "1.1.7")]
[Description("Modify the loot tables for the Christmas Presents, Halloween Bags and Easter Eggs")]
 class HolidayLoot : RustPlugin
{
    private bool Changed;
    private DynamicConfigFile PresentData;
    private DynamicConfigFile PresentData2;
    private DynamicConfigFile PresentData3;
    private DynamicConfigFile HalloweenData2;
    private DynamicConfigFile HalloweenData3;
    private DynamicConfigFile EasterData2;
    private DynamicConfigFile EasterData3;
    private DynamicConfigFile SantaData;
    private StoredData storedData;
    private StoredData storedData2;
    private StoredData storedData3;
    private StoredData HalloweenStoredData2;
    private StoredData HalloweenStoredData3;
    private StoredData EasterStoredData2;
    private StoredData EasterStoredData3;
    private StoredData SantaStoredData;
    private class StoredData
    {
        public List<ItemInfo> lootTable;
    }

    private void SaveData();
    private void LoadData();
    private class ItemInfo
    {
        public string itemName;
        public int minItemAmount;
        public int maxItemAmount;
        public Dictionary<string, float> attachments;
        public float chance;
        public int skinID;
    }

    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private List<ItemInfo> smallList;
    private List<ItemInfo> mediumList;
    private List<ItemInfo> largeList;
    private List<ItemInfo> mediumListHalloween;
    private List<ItemInfo> largeListHalloween;
    private List<ItemInfo> mediumListEaster;
    private List<ItemInfo> largeListEaster;
    private List<ItemInfo> santaList;
    private int smallPresentsToUse;
    private int mediumPresentsToUse;
    private int largePresentsToUse;
    private int mediumBagsToUse;
    private int largeBagsToUse;
    private int mediumEggsToUse;
    private int largeEggsToUse;
    private int SantaDropToUse;
    private Dictionary<string, int> amountNeededDic;
    private int smallMinItems;
    private int smallMaxItems;
    private int mediumMinItems;
    private int mediumMaxItems;
    private int largeMinItems;
    private int largeMaxItems;
    private int mediumMinEggsItems;
    private int mediumMaxEggsItems;
    private int largeMinEggsItems;
    private int largeMaxEggsItems;
    private int mediumMinBagsItems;
    private int mediumMaxBagsItems;
    private int largeMinBagsItems;
    private int largeMaxBagsItems;
    private int santaMinItems;
    private int santaMaxItems;
    private bool weaponsSpawnWithAmmo;
    private bool sureItemsWillAlwaysSpawn;
    private void Init();
    private void AddDefaultItems();
    private object OnItemAction(Item item, string action);
    private bool CheckAmountNeeded(string itemName, BasePlayer player);
    private void GiveThink(BasePlayer player, string presentName);
    private List<Item> CreateItems(string presentname);
    private Item TryMakeItem(List<ItemInfo> list);
    private List<Item> CheckFor100Items(List<ItemInfo> list);
    private static void ItemRemovalThink(Item item, BasePlayer player, int itemsToTake);
    private string msg(string key, string id);
}

private class StoredData
{
    public List<ItemInfo> lootTable;
}

private class ItemInfo
{
    public string itemName;
    public int minItemAmount;
    public int maxItemAmount;
    public Dictionary<string, float> attachments;
    public float chance;
    public int skinID;
}


```

---

## Holograms by birthdates - Provide floating text with information to players

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text.RegularExpressions;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Holograms", "birthdates", "1.4.3")]
[Description("Provide floating text with information to players")]
public class Holograms : RustPlugin
{
    private int PlayerMask { get; set; }
    private LayerMask ObstructionMask { get; set; }
    private static Holograms Instance { get; set; }
    private readonly IDictionary<string, Placeholder> _placeholders;
    private readonly Stopwatch _stopwatch;
    private readonly Regex _placeholderRegex;
    private const string UsePermission;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void UpdatePlaceholders();
    private string GetPlaceholderValue(string id);
    [HookMethod("RegisterPlaceholder")]
    private void RegisterPlaceholder(string id, float updateTime, Func<string> callback);
    private void RegisterPlaceholder(string id, Func<string> callback);
    private void TickHolograms();
    [HookMethod("CreateHologram")]
    private Hologram CreateHologram(string id, Vector3 spawnPos);
    [HookMethod("FindHologram")]
    private Hologram FindHologram(string id);
    [HookMethod("DeleteHologram")]
    private bool DeleteHologram(string id);
    [ChatCommand("hologram")]
    private void HologramPlayerCommand(BasePlayer player, string label, string[] args);
    [ConsoleCommand("hologram")]
    private void HologramConsoleCommand(ConsoleSystem.Arg arg);
    private void HologramCommand(IExecutor executor, string label, string[] args);
    private void SendHelp(IExecutor executor, string label);
    private class Placeholder
    {
        private string Id { get; set; }
        private DateTime _nextUpdate;
        private float UpdateTime { get; set; }
        private Func<string> Callback { get; set; }
        public string Value { get; set; }
        public Placeholder(string id, float updateTime, Func<string> callback);
        public void TryUpdate();
    }

    private class Line
    {
        public string Text { get; set; }
        [JsonIgnore]
        public string PlaceholderText { get; set; }
    }

    private class Hologram
    {
        private bool _hasPlaceholders;
        private bool _hasChecked;
        private DateTime _nextUpdate;
        public string Id { get; set; }
        public IList<Line> Lines { get; set; }
        public Vector3 Position { get; set; }
        public float LineSpacing { get; set; }
        public float UpdateInterval { get; set; }
        public float ViewRadius { get; set; }
        public string Permission { get; set; }
        public void AddLine(string text);
        public void SetLine(int index, string text);
        public void RemoveLine(int index);
        private void CheckForPlaceholders();
        private void DoesHavePlaceholders(string text);
        public void Tick();
        private void Update();
        private void UpdateForNearby();
    }

    private Data _data;
    private ConfigFile _config;
    protected override void LoadDefaultMessages();
    public class ConfigFile
    {
        [JsonProperty("Default Line Spacing")]
        public float DefaultLineSpacing { get; set; }
        [JsonProperty("Hologram Tick Interval (seconds)")]
        public float TickInterval { get; set; }
        [JsonProperty("Placeholder Tick Interval (seconds)")]
        public float PlaceholderInterval { get; set; }
        [JsonProperty("Obstruction Mask")]
        public string[] ObstructionMask { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class Data
    {
        public IDictionary<string, Hologram> Holograms { get; set; }
    }

    private void SaveData();
}

private class Placeholder
{
    private string Id { get; set; }
    private DateTime _nextUpdate;
    private float UpdateTime { get; set; }
    private Func<string> Callback { get; set; }
    public string Value { get; set; }
    public Placeholder(string id, float updateTime, Func<string> callback);
    public void TryUpdate();
}

private class Line
{
    public string Text { get; set; }
    [JsonIgnore]
    public string PlaceholderText { get; set; }
}

private class Hologram
{
    private bool _hasPlaceholders;
    private bool _hasChecked;
    private DateTime _nextUpdate;
    public string Id { get; set; }
    public IList<Line> Lines { get; set; }
    public Vector3 Position { get; set; }
    public float LineSpacing { get; set; }
    public float UpdateInterval { get; set; }
    public float ViewRadius { get; set; }
    public string Permission { get; set; }
    public void AddLine(string text);
    public void SetLine(int index, string text);
    public void RemoveLine(int index);
    private void CheckForPlaceholders();
    private void DoesHavePlaceholders(string text);
    public void Tick();
    private void Update();
    private void UpdateForNearby();
}

public class ConfigFile
{
    [JsonProperty("Default Line Spacing")]
    public float DefaultLineSpacing { get; set; }
    [JsonProperty("Hologram Tick Interval (seconds)")]
    public float TickInterval { get; set; }
    [JsonProperty("Placeholder Tick Interval (seconds)")]
    public float PlaceholderInterval { get; set; }
    [JsonProperty("Obstruction Mask")]
    public string[] ObstructionMask { get; set; }
    public static ConfigFile DefaultConfig();
}

private class Data
{
    public IDictionary<string, Hologram> Holograms { get; set; }
}


```

---

## HomeProtection by VisEntities - Protects players and their homes from intruders

```csharp

Oxide.Plugins
[Info("Home Protection", "Wulf/lukespragg", "1.0.0")]
[Description("Protects you and your home from intruders.")]
 class HomeProtection : RustPlugin
{
    private void OnEntityTakeDamage(BasePlayer victim, HitInfo hitInfo);
    private void OnEntityTakeDamage(BuildingBlock buildingBlock, HitInfo hitInfo);
}


```

---

## HornDoors by Imthenewguy - Open and close garage doors and gates with the honk of your horn.

```csharp
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Text;
using UnityEngine;
using Rust;

Oxide.Plugins
[Info("Horn Doors", "imthenewguy", "1.0.1")]
[Description("Allows players to use the horn from their vehicle to open garage doors.")]
 class HornDoors : RustPlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Maximum distance that the vehicle can be from the door before we attempt to open it?")]
        public float distance;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    const string perm_use;
    const string perm_off;
     void Init();
     Dictionary<BasePlayer, float> HornLastPressed;
     void OnVehicleHornPressed(VehicleModuleSeating seat, BasePlayer player);
    [ConsoleCommand("togglehorn")]
     void ToggleConsole(ConsoleSystem.Arg arg);
    [ChatCommand("togglehorn")]
     void ToggleChat(BasePlayer player);
     void Toggle(BasePlayer player);
    public void HandleDoors(BasePlayer player, ModularCar car);
    private bool CanSeeDoor(Door door, ModularCar car);
     bool IsVehicleDoor(string prefab);
    private static List<T> FindEntitiesOfType(Vector3 a, float n, int m);
    private static bool InRange(Vector3 a, Vector3 b, float distance);
}

public class Configuration
{
    [JsonProperty("Maximum distance that the vehicle can be from the door before we attempt to open it?")]
    public float distance;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## HorseSeat by Chokitu - Adds an extra seat to horses

```csharp
using UnityEngine;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Horse Seat", "Chokitu", "1.0.3")]
[Description("Gives horses 2 seats")]
public class HorseSeat : RustPlugin
{
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private class PluginConfig
    {
        public bool Enable2Seats;
    }

    private void Init();
    private void OnEntitySpawned(RidableHorse entity);
    public class AddSeats : MonoBehaviour
    {
        public RidableHorse entity;
         void Awake();
    }

}

private class PluginConfig
{
    public bool Enable2Seats;
}

public class AddSeats : MonoBehaviour
{
    public RidableHorse entity;
     void Awake();
}


```

---

## HorseSettings by  - Allows adjusting ridable horses for players with certain permissions

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Horse Settings", "hoppel", "1.0.0")]
[Description("Allows you to adjust ridable horses for players with certain permissions")]
public class HorseSettings : RustPlugin
{
    private Configuration _config;
    private HorseConfiguration _default;
    private void Init();
    private void OnEntityMounted(BaseMountable entity, BasePlayer player);
    private void ApplyHorseSettings(RidableHorse horse, HorseConfiguration config);
    public class HorseConfiguration
    {
        public SpeedSettings SpeedSetting;
        public MiscSetttings MiscSetting;
        public MetabolismSettings MetabolismSetting;
        public class SpeedSettings
        {
            public float WalkSpeed;
            public float TrotSpeed;
            public float RunSpeed;
            public float RoadBonusSpeed;
            public float TurnSpeed;
        }

        public class MiscSetttings
        {
            [JsonProperty("How deep can the horse go into water")]
            public float MaxWaterDepth;
            [JsonProperty("Max Stamina (seconds)")]
            public float MaxStaminaSeconds;
        }

        public class MetabolismSettings
        {
            [JsonProperty("How much Stamina the horse is using")]
            public float StaminaCoreLossRatio;
            [JsonProperty("Stamina core speed bonus")]
            public float StaminaCoreSpeedBonus;
            [JsonProperty("Stamina recovery ratio while moving")]
            public float StaminaRecoveryRatioMoving;
            [JsonProperty("Stamina recovery ratio while standing")]
            public float StaminaRecoveryRatioStanding;
            [JsonProperty("Calories used to recover stamina")]
            public float CaloriesToStaminaRatio;
            [JsonProperty("Water used to recover stamina")]
            public float WaterToStaminaRatio;
        }

    }

    public class Configuration
    {
        [JsonProperty(PropertyName = "Settings list")]
        public Dictionary<string, HorseConfiguration> HorseSettingsList;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class HorseConfiguration
{
    public SpeedSettings SpeedSetting;
    public MiscSetttings MiscSetting;
    public MetabolismSettings MetabolismSetting;
    public class SpeedSettings
    {
        public float WalkSpeed;
        public float TrotSpeed;
        public float RunSpeed;
        public float RoadBonusSpeed;
        public float TurnSpeed;
    }

    public class MiscSetttings
    {
        [JsonProperty("How deep can the horse go into water")]
        public float MaxWaterDepth;
        [JsonProperty("Max Stamina (seconds)")]
        public float MaxStaminaSeconds;
    }

    public class MetabolismSettings
    {
        [JsonProperty("How much Stamina the horse is using")]
        public float StaminaCoreLossRatio;
        [JsonProperty("Stamina core speed bonus")]
        public float StaminaCoreSpeedBonus;
        [JsonProperty("Stamina recovery ratio while moving")]
        public float StaminaRecoveryRatioMoving;
        [JsonProperty("Stamina recovery ratio while standing")]
        public float StaminaRecoveryRatioStanding;
        [JsonProperty("Calories used to recover stamina")]
        public float CaloriesToStaminaRatio;
        [JsonProperty("Water used to recover stamina")]
        public float WaterToStaminaRatio;
    }

}

public class SpeedSettings
{
    public float WalkSpeed;
    public float TrotSpeed;
    public float RunSpeed;
    public float RoadBonusSpeed;
    public float TurnSpeed;
}

public class MiscSetttings
{
    [JsonProperty("How deep can the horse go into water")]
    public float MaxWaterDepth;
    [JsonProperty("Max Stamina (seconds)")]
    public float MaxStaminaSeconds;
}

public class MetabolismSettings
{
    [JsonProperty("How much Stamina the horse is using")]
    public float StaminaCoreLossRatio;
    [JsonProperty("Stamina core speed bonus")]
    public float StaminaCoreSpeedBonus;
    [JsonProperty("Stamina recovery ratio while moving")]
    public float StaminaRecoveryRatioMoving;
    [JsonProperty("Stamina recovery ratio while standing")]
    public float StaminaRecoveryRatioStanding;
    [JsonProperty("Calories used to recover stamina")]
    public float CaloriesToStaminaRatio;
    [JsonProperty("Water used to recover stamina")]
    public float WaterToStaminaRatio;
}

public class Configuration
{
    [JsonProperty(PropertyName = "Settings list")]
    public Dictionary<string, HorseConfiguration> HorseSettingsList;
    public static Configuration DefaultConfig();
}


```

---

## HorseStorage by Bazz3l - Gives horses the ability to carry items

```csharp
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Horse Storage", "Bazz3l", "1.0.5")]
[Description("Gives horses the ability to carry items")]
public class HorseStorage : RustPlugin
{
    private const string STASH_PREFAB;
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class PluginConfig
    {
        public bool EnableStorage;
        public static PluginConfig DefaultConfig();
    }

     void OnEntitySpawned(RidableHorse entity);
     void OnEntityDeath(RidableHorse entity, HitInfo info);
     class AddStorageBox : MonoBehaviour
    {
        public RidableHorse entity;
        public StashContainer stash1;
        public StashContainer stash2;
         void Awake();
        public void OnDeath();
         StashContainer CreateStorageContainer(Vector3 localPosition, Vector3 rotation);
         void RemoveStorageContainer(StashContainer stash);
    }

}

 class PluginConfig
{
    public bool EnableStorage;
    public static PluginConfig DefaultConfig();
}

 class AddStorageBox : MonoBehaviour
{
    public RidableHorse entity;
    public StashContainer stash1;
    public StashContainer stash2;
     void Awake();
    public void OnDeath();
     StashContainer CreateStorageContainer(Vector3 localPosition, Vector3 rotation);
     void RemoveStorageContainer(StashContainer stash);
}


```

---

## HostileTime by rostov114 - Changes value of hostile duration

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("Hostile Time", "Orange", "1.0.2")]
[Description("Changes value of hostile duration")]
public class HostileTime : RustPlugin
{
    private void OnEntityMarkHostile(BasePlayer player, float duration);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Hostile duration (seconds)")]
        public float newDuration;
    }

    protected override void LoadConfig();
    private static void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Hostile duration (seconds)")]
    public float newDuration;
}


```

---

## Hotel by shady14u - Complete Hotel System for Rust

```csharp
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Hotel", "Shady14u", "2.0.26")]
[Description("Complete Hotel System for Rust.")]
public class Hotel : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     Plugin Economics;
     Plugin ServerRewards;
     Plugin Backpacks;
    private Timer hotelGuiTimer;
    private Timer hotelRoomCheckoutTimer;
    private readonly Hash<BasePlayer, Timer> playerGuiTimers;
    private readonly Hash<BasePlayer, Timer> playerBlackListGuiTimers;
    private readonly Hash<ulong, HotelData> hotelGuests;
    private StoredData storedData;
    static readonly int ConstructionColl;
    static readonly int DeployableColl;
    static readonly DateTime Epoch;
    public static Quaternion DefaultQuaternion;
    private static Dictionary<string, HotelData> EditHotel;
    private static Dictionary<string, HotelMarker> HotelMarkers;
    private static readonly Vector3 Vector3Up;
    private static readonly Vector3 Vector3Up2;
     void Init();
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "adminGuiJson")]
        public string AdminGuiJson;
        [JsonProperty(PropertyName = "authLevel")]
        public int AuthLevel;
        [JsonProperty(PropertyName = "enterZoneShowPlayerGUI")]
        public bool EnterZoneShowPlayerGui;
        [JsonProperty(PropertyName = "enterZoneShowRoom")]
        public bool EnterZoneShowRoom;
        [JsonProperty(PropertyName = "KickHobos")]
        public bool KickHobos;
        [JsonProperty(PropertyName = "mapMarker")]
        public string MapMarker;
        [JsonProperty(PropertyName = "mapMarkerColor")]
        public string MapMarkerColor;
        [JsonProperty(PropertyName = "mapMarkerColorBorder")]
        public string MapMarkerColorBorder;
        [JsonProperty(PropertyName = "mapMarkerRadius")]
        public float MapMarkerRadius;
        [JsonProperty(PropertyName = "openDoorPlayerGUI")]
        public bool OpenDoorPlayerGui;
        [JsonProperty(PropertyName = "openDoorShowRoom")]
        public bool OpenDoorShowRoom;
        [JsonProperty(PropertyName = "panelTimeOut")]
        public int PanelTimeOut;
        [JsonProperty(PropertyName = "panelXMax")]
        public string PanelXMax;
        [JsonProperty(PropertyName = "panelXMin")]
        public string PanelXMin;
        [JsonProperty(PropertyName = "panelYMax")]
        public string PanelYMax;
        [JsonProperty(PropertyName = "panelYMin")]
        public string PanelYMin;
        [JsonProperty(PropertyName = "playerGuiJson")]
        public string PlayerGuiJson;
        [JsonProperty(PropertyName = "useNPCShowPlayerGUI")]
        public bool UseNpcShowPlayerGui;
        [JsonProperty(PropertyName = "useNPCShowRoom")]
        public bool UseNpcShowRoom;
        [JsonProperty(PropertyName = "xMax")]
        public string XMax;
        [JsonProperty(PropertyName = "xMin")]
        public string XMin;
        [JsonProperty(PropertyName = "yMax")]
        public string YMax;
        [JsonProperty(PropertyName = "yMin")]
        public string YMin;
        [JsonProperty(PropertyName = "blackListGuiJson")]
        public string BlackListGuiJson;
        [JsonProperty(PropertyName = "blackList")]
        public string[] BlackList;
        [JsonProperty(PropertyName = "defaultZoneFlags")]
        public string[] DefaultZoneFlags;
        [JsonProperty(PropertyName = "showRoomCounterUi")]
        public bool ShowRoomCounterUi { get; set; }
        [JsonProperty(PropertyName = "counterUiAnchorMin")]
        public string CounterUiAnchorMin { get; set; }
        [JsonProperty(PropertyName = "counterUiAnchorMax")]
        public string CounterUiAnchorMax { get; set; }
        [JsonProperty(PropertyName = "counterUiTextColor")]
        public string CounterUiTextColor { get; set; }
        [JsonProperty(PropertyName = "counterUiTextSize")]
        public int CounterUiTextSize { get; set; }
        [JsonProperty(PropertyName = "hideUiForNonRenters")]
        public bool HideUiForNonRenters { get; set; }
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private static class PluginMessages
    {
        public const string MessageHotelDisableHelp;
        public const string MessagePlayerIsRenter;
        public const string MessagePlayerNotRenter;
        public const string MessageHotelEnableHelp;
        public const string MessageErrorPlayerNotFound;
        public const string MessageAlreadyEditing;
        public const string MessageNoHotelHelp;
        public const string MessageHotelNewHelp;
        public const string MessageHotelEditHelp;
        public const string MessageHotelEditEditing;
        public const string MessageErrorAlreadyExist;
        public const string MessageErrorNotAllowed;
        public const string MessageErrorEditDoesNotExist;
        public const string MessageMaintenance;
        public const string MessageErrorUnavailableRoom;
        public const string MessageHotelNewCreated;
        public const string MessageErrorNotAllowedToEnter;
        public const string MessageHotelExtendHelp;
        public const string MessageErrorAlreadyGotRoom;
        public const string MessageErrorPermissionsNeeded;
        public const string MessageCouldNotFindToExtend;
        public const string MessageRentUnlimited;
        public const string MessageRentTimeLeft;
        public const string MessagePayedRent;
        public const string MessageErrorNotEnoughCoins;
        public const string MessageErrorNotEnoughRp;
        public const string MessageErrorNotEnoughItems;
        public const string GuiBoardAdmin;
        public const string GuiBoardBlackList;
        public const string GuiBoardPlayer;
        public const string GuiBoardPlayerRoom;
        public const string GuiBoardPlayerMaintenance;
        public const string MessageMustLookAtDoor;
        public const string Menu1;
        public const string Menu2;
        public const string Menu3;
        public const string Menu4;
        public const string Menu5;
        public const string Menu6;
        public const string Menu7;
        public const string Menu8;
        public const string Menu9;
        public const string Menu10;
        public const string Menu11;
        public const string Menu12;
        public const string Menu13;
        public const string ExtendButtonText;
    }

    private string GetMsg(string key, object userId);
    protected override void LoadDefaultMessages();
     object CanPickupLock(BasePlayer player, BaseLock baseLock);
     object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
    private void LoadData();
     void OnServerSave();
     void OnEnterZone(string zoneId, BasePlayer player);
     void OnExitZone(string ZoneID, BasePlayer player);
     object OnItemPickup(Item item, BasePlayer player);
     void OnPlayerLootEnd(PlayerLoot inventory);
     void OnPlayerDisconnected(BasePlayer player);
     void OnServerInitialized(bool initial);
     void OnUseNPC(BasePlayer npc, BasePlayer player);
    private bool CanRentRoom(BasePlayer player, HotelData hotel, bool isExtending);
     void CheckTimeOutRooms();
    private void CleanUpMarkers();
    private static void CloseDoor(Door door);
    private void EconomicsWithdraw(BasePlayer player, int amount);
    private static void EmptyDeployablesRoom(BaseEntity door, float radius);
    private static Dictionary<string, Room> FindAllRooms(Vector3 position, float radius, float roomradius);
    private static BuildingBlock FindBlockFromRay(Vector3 pos, Vector3 aim);
    private static CodeLock FindCodeLockByPos(Vector3 pos);
    private static CodeLock FindCodeLockByRoomId(string roomId);
    private static List<Door> FindDoorsFromPosition(Vector3 position, float radius);
    private bool FindHotelAndRoomByPos(Vector3 position, HotelData hotelData, Room roomData);
    private IPlayer FindPlayer(string nameOrIdOrIp);
    private bool FindRoomById(string roomId, HotelData targetHotel, Room targetRoom);
    private static Room FindRoomByDoorAndHotel(HotelData hotel, BaseEntity door);
    private RoomTimeMessage GetRoomTimeLeft(HotelData hotel, string userIdString);
    private bool HasBlackListedItems(BasePlayer player, List<string> blackList);
    private bool HasAccess(BasePlayer player, string accessRole);
    private void LoadPermissions();
    private static void LockLock(CodeLock codeLock);
    static double LogTime();
    private void NewRoomOwner(CodeLock codeLock, BasePlayer player, HotelData hotel, Room room);
    private static void OpenDoor(Door door);
    static object RaycastAll(Ray ray);
    private Vector3 RayForDoor(BasePlayer player);
    private void ResetRoom(Room room);
    private static void ResetRoom(CodeLock codeLock, Room room);
    private void SaveData();
    private void ServerRewardsWithdraw(BasePlayer player, int amount);
    private static void SpawnDeployable(string prefabName, Vector3 pos, Quaternion rot, BasePlayer player, ulong skinId);
    private bool TryParseHtmlString(string value, Color color);
     void Unload();
    private void CleanUpUi();
    private static void UnlockLock(CodeLock codeLock);
    private void UpdateHotelCounter();
    private void ShowHotelCounterUi(BasePlayer player, RoomTimeMessage roomTimeMessage);
    private void RefreshAdminHotelGui(BasePlayer player);
    private void RefreshBlackListGui(BasePlayer player, HotelData hotel, List<string> blackList);
    private string CreateBlackListGuiMsg(BasePlayer player, HotelData hotel, List<string> blackList);
    private void RemoveBlackListGui(BasePlayer player);
    private void RefreshPlayerHotelGui(BasePlayer player, HotelData hotel);
    private static string ConvertSecondsToBetter(string seconds);
    private static string ConvertSecondsToBetter(double seconds);
    private static string ConvertSecondsToDate(string seconds);
    private static string ConvertSecondsToDate(double seconds);
    private string CreatePlayerGuiMsg(BasePlayer player, HotelData hotel, string guiMsg);
    private string CreateAdminGuiMsg(BasePlayer player);
    private void RemoveAdminHotelGui(BasePlayer player);
    private void RemovePlayerHotelGui(BasePlayer player);
    private void ShowHotelGrid(BasePlayer player);
    private static void ShowPlayerRoom(BasePlayer player, HotelData hotel);
    public void CreateMapMarker(HotelData hotel);
    private Color GetMarkerColor(string id);
    [ChatCommand("hotel_save")]
     void CmdChatHotelSave(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_close")]
     void CmdChatHotelClose(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel")]
     void CmdChatHotel(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_list")]
     void CmdChatHotelList(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_disable")]
     void CmdChatHotelDisable(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_edit")]
     void CmdChatHotelEdit(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_enable")]
     void CmdChatHotelEnable(BasePlayer player, string command, string[] args);
    private void SetPlayerAsRenter(string playerName, bool isRenter);
    [Command("hotelextend")]
     void HotelExtend(IPlayer iplayer, string command, string[] args);
    [ChatCommand("hotel_extend")]
     void CmdChatHotelExtend(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_reset")]
     void CmdChatHotelReset(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_remove")]
     void CmdChatHotelRemove(BasePlayer player, string command, string[] args);
    [ChatCommand("rooms")]
     void CmdChatRooms(BasePlayer player, string command, string[] args);
    [ChatCommand("room")]
     void CmdChatRoom(BasePlayer player, string command, string[] args);
    [ChatCommand("hotel_new")]
     void CmdChatHotelNew(BasePlayer player, string command, string[] args);
    public class RoomTimeMessage
    {
        public HotelData HotelData { get; set; }
        public string TimeMessage { get; set; }
        public double TimeRemaining { get; set; }
    }

    public class HotelData
    {
         Vector3 _pos;
        public string currency;
        public string e;
        public bool enabled;
        public string hotelName;
        public bool kickHobos;
        public string markerColor;
        public string npc;
        public string p;
        public int price;
        public string r;
        public string rd;
        public Dictionary<string, Room> rooms;
        public string rr;
        public bool showMarker;
        public string x;
        public string y;
        public string z;
        public HotelData();
        public HotelData(string hotelName);
        public void Activate();
        public void AddRoom(Room newRoom);
        public void Deactivate();
        public Vector3 Pos();
        public int Price();
        public void RefreshRooms();
    }

    public class Room
    {
        public string checkingTime;
        public string checkoutTime;
        public List<DeployableItem> defaultDeployables;
        public double intCheckoutTime;
        public string lastRenter;
        public Vector3 pos;
        public string renter;
        public string roomId;
        public string x;
        public string y;
        public string z;
        public Room(Vector3 position);
        public double CheckOutTime();
        public void ExtendDuration(double duration);
        public Vector3 Pos();
        public void Reset();
    }

    public class DeployableItem
    {
         Vector3 _pos;
         Quaternion _rot;
        public string prefabName;
        public string rw;
        public string rx;
        public string ry;
        public string rz;
        public ulong skinId;
        public string x;
        public string y;
        public string z;
        public DeployableItem();
        public DeployableItem(BaseEntity deployable);
        public Vector3 Pos();
        public Quaternion Rot();
    }

    public class StoredData
    {
        public StoredData();
        public HashSet<HotelData> Hotels { get; set; }
    }

    public class HotelPanelImage
    {
        public string AnchorX { get; set; }
        public string AnchorY { get; set; }
        public bool Available { get; set; }
        public string BackgroundColor { get; set; }
        public string Dock { get; set; }
        public double Height { get; set; }
        public string Margin { get; set; }
        public int Order { get; set; }
        public string Url { get; set; }
        public double Width { get; set; }
    }

    public class HotelPanelText
    {
        public string Align { get; set; }
        public string AnchorX { get; set; }
        public string AnchorY { get; set; }
        public bool Available { get; set; }
        public string BackgroundColor { get; set; }
        public string Content { get; set; }
        public string Dock { get; set; }
        public string FontColor { get; set; }
        public int FontSize { get; set; }
        public double Height { get; set; }
        public string Margin { get; set; }
        public int Order { get; set; }
        public double Width { get; set; }
    }

    public class HotelMarker
    {
        public MapMarkerGenericRadius GenericMapMarker { get; set; }
        public VendingMachineMapMarker VendingMachineMapMarker { get; set; }
    }

}

public class Configuration
{
    [JsonProperty(PropertyName = "adminGuiJson")]
    public string AdminGuiJson;
    [JsonProperty(PropertyName = "authLevel")]
    public int AuthLevel;
    [JsonProperty(PropertyName = "enterZoneShowPlayerGUI")]
    public bool EnterZoneShowPlayerGui;
    [JsonProperty(PropertyName = "enterZoneShowRoom")]
    public bool EnterZoneShowRoom;
    [JsonProperty(PropertyName = "KickHobos")]
    public bool KickHobos;
    [JsonProperty(PropertyName = "mapMarker")]
    public string MapMarker;
    [JsonProperty(PropertyName = "mapMarkerColor")]
    public string MapMarkerColor;
    [JsonProperty(PropertyName = "mapMarkerColorBorder")]
    public string MapMarkerColorBorder;
    [JsonProperty(PropertyName = "mapMarkerRadius")]
    public float MapMarkerRadius;
    [JsonProperty(PropertyName = "openDoorPlayerGUI")]
    public bool OpenDoorPlayerGui;
    [JsonProperty(PropertyName = "openDoorShowRoom")]
    public bool OpenDoorShowRoom;
    [JsonProperty(PropertyName = "panelTimeOut")]
    public int PanelTimeOut;
    [JsonProperty(PropertyName = "panelXMax")]
    public string PanelXMax;
    [JsonProperty(PropertyName = "panelXMin")]
    public string PanelXMin;
    [JsonProperty(PropertyName = "panelYMax")]
    public string PanelYMax;
    [JsonProperty(PropertyName = "panelYMin")]
    public string PanelYMin;
    [JsonProperty(PropertyName = "playerGuiJson")]
    public string PlayerGuiJson;
    [JsonProperty(PropertyName = "useNPCShowPlayerGUI")]
    public bool UseNpcShowPlayerGui;
    [JsonProperty(PropertyName = "useNPCShowRoom")]
    public bool UseNpcShowRoom;
    [JsonProperty(PropertyName = "xMax")]
    public string XMax;
    [JsonProperty(PropertyName = "xMin")]
    public string XMin;
    [JsonProperty(PropertyName = "yMax")]
    public string YMax;
    [JsonProperty(PropertyName = "yMin")]
    public string YMin;
    [JsonProperty(PropertyName = "blackListGuiJson")]
    public string BlackListGuiJson;
    [JsonProperty(PropertyName = "blackList")]
    public string[] BlackList;
    [JsonProperty(PropertyName = "defaultZoneFlags")]
    public string[] DefaultZoneFlags;
    [JsonProperty(PropertyName = "showRoomCounterUi")]
    public bool ShowRoomCounterUi { get; set; }
    [JsonProperty(PropertyName = "counterUiAnchorMin")]
    public string CounterUiAnchorMin { get; set; }
    [JsonProperty(PropertyName = "counterUiAnchorMax")]
    public string CounterUiAnchorMax { get; set; }
    [JsonProperty(PropertyName = "counterUiTextColor")]
    public string CounterUiTextColor { get; set; }
    [JsonProperty(PropertyName = "counterUiTextSize")]
    public int CounterUiTextSize { get; set; }
    [JsonProperty(PropertyName = "hideUiForNonRenters")]
    public bool HideUiForNonRenters { get; set; }
    public static Configuration DefaultConfig();
}

private static class PluginMessages
{
    public const string MessageHotelDisableHelp;
    public const string MessagePlayerIsRenter;
    public const string MessagePlayerNotRenter;
    public const string MessageHotelEnableHelp;
    public const string MessageErrorPlayerNotFound;
    public const string MessageAlreadyEditing;
    public const string MessageNoHotelHelp;
    public const string MessageHotelNewHelp;
    public const string MessageHotelEditHelp;
    public const string MessageHotelEditEditing;
    public const string MessageErrorAlreadyExist;
    public const string MessageErrorNotAllowed;
    public const string MessageErrorEditDoesNotExist;
    public const string MessageMaintenance;
    public const string MessageErrorUnavailableRoom;
    public const string MessageHotelNewCreated;
    public const string MessageErrorNotAllowedToEnter;
    public const string MessageHotelExtendHelp;
    public const string MessageErrorAlreadyGotRoom;
    public const string MessageErrorPermissionsNeeded;
    public const string MessageCouldNotFindToExtend;
    public const string MessageRentUnlimited;
    public const string MessageRentTimeLeft;
    public const string MessagePayedRent;
    public const string MessageErrorNotEnoughCoins;
    public const string MessageErrorNotEnoughRp;
    public const string MessageErrorNotEnoughItems;
    public const string GuiBoardAdmin;
    public const string GuiBoardBlackList;
    public const string GuiBoardPlayer;
    public const string GuiBoardPlayerRoom;
    public const string GuiBoardPlayerMaintenance;
    public const string MessageMustLookAtDoor;
    public const string Menu1;
    public const string Menu2;
    public const string Menu3;
    public const string Menu4;
    public const string Menu5;
    public const string Menu6;
    public const string Menu7;
    public const string Menu8;
    public const string Menu9;
    public const string Menu10;
    public const string Menu11;
    public const string Menu12;
    public const string Menu13;
    public const string ExtendButtonText;
}

public class RoomTimeMessage
{
    public HotelData HotelData { get; set; }
    public string TimeMessage { get; set; }
    public double TimeRemaining { get; set; }
}

public class HotelData
{
     Vector3 _pos;
    public string currency;
    public string e;
    public bool enabled;
    public string hotelName;
    public bool kickHobos;
    public string markerColor;
    public string npc;
    public string p;
    public int price;
    public string r;
    public string rd;
    public Dictionary<string, Room> rooms;
    public string rr;
    public bool showMarker;
    public string x;
    public string y;
    public string z;
    public HotelData();
    public HotelData(string hotelName);
    public void Activate();
    public void AddRoom(Room newRoom);
    public void Deactivate();
    public Vector3 Pos();
    public int Price();
    public void RefreshRooms();
}

public class Room
{
    public string checkingTime;
    public string checkoutTime;
    public List<DeployableItem> defaultDeployables;
    public double intCheckoutTime;
    public string lastRenter;
    public Vector3 pos;
    public string renter;
    public string roomId;
    public string x;
    public string y;
    public string z;
    public Room(Vector3 position);
    public double CheckOutTime();
    public void ExtendDuration(double duration);
    public Vector3 Pos();
    public void Reset();
}

public class DeployableItem
{
     Vector3 _pos;
     Quaternion _rot;
    public string prefabName;
    public string rw;
    public string rx;
    public string ry;
    public string rz;
    public ulong skinId;
    public string x;
    public string y;
    public string z;
    public DeployableItem();
    public DeployableItem(BaseEntity deployable);
    public Vector3 Pos();
    public Quaternion Rot();
}

public class StoredData
{
    public StoredData();
    public HashSet<HotelData> Hotels { get; set; }
}

public class HotelPanelImage
{
    public string AnchorX { get; set; }
    public string AnchorY { get; set; }
    public bool Available { get; set; }
    public string BackgroundColor { get; set; }
    public string Dock { get; set; }
    public double Height { get; set; }
    public string Margin { get; set; }
    public int Order { get; set; }
    public string Url { get; set; }
    public double Width { get; set; }
}

public class HotelPanelText
{
    public string Align { get; set; }
    public string AnchorX { get; set; }
    public string AnchorY { get; set; }
    public bool Available { get; set; }
    public string BackgroundColor { get; set; }
    public string Content { get; set; }
    public string Dock { get; set; }
    public string FontColor { get; set; }
    public int FontSize { get; set; }
    public double Height { get; set; }
    public string Margin { get; set; }
    public int Order { get; set; }
    public double Width { get; set; }
}

public class HotelMarker
{
    public MapMarkerGenericRadius GenericMapMarker { get; set; }
    public VendingMachineMapMarker VendingMachineMapMarker { get; set; }
}


```

---

## HumanNPC by Razor - Adds interactive human NPCs which can be modded by other plugins

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Rust;
using Network;
using Facepunch;
using Facepunch.Utility;
using System;
using System.Collections.Generic;
using System.Collections;
using System.Globalization;
using System.Linq;
using UnityEngine;
using Convert = System.Convert;

Oxide.Plugins
[Info("Human NPC", "Razor", "0.5.4")]
[Description("Adds interactive human NPCs which can be modded by other plugins")]
public class HumanNPC : RustPlugin
{
    private static Collider[] colBuffer;
    private int playerLayer;
    private static int targetLayer;
    private static Vector3 Vector3Down;
    private static int groundLayer;
    private static HumanNPC ins;
    private Coroutine QueuedSpawn { get; set; }
    private bool NewTeam;
    private int TeamCount;
    private BasePlayer TeamPlayer;
    private Hash<ulong, RelationshipManager.PlayerTeam> PlayersTeams;
    private Hash<ulong, HumanNPCInfo> humannpcs;
    static int playerMask;
    static int obstructionMask;
    static int constructionMask;
    static int terrainMask;
    private readonly Hash<ulong, RecordingData> Recording;
    private readonly Hash<string, NpcSound> Sounds;
    private readonly Hash<string, NpcSound> cached;
    public List<ulong> NpcTalking;
    private bool save;
    private StoredData storedData;
    private DynamicConfigFile data;
    private Vector3 eyesPosition;
    private string chat;
    private bool TeamsNPC;
    private float RadiusNPC;
    [PluginReference]
    private Plugin Kits;
    private Plugin Waypoints;
    private Plugin Vanish;
    private static PathFinding PathFinding;
    private class StoredData
    {
        public HashSet<HumanNPCInfo> HumanNPCs;
    }

    public class WaypointInfo
    {
        public float Speed;
        public Vector3 Position;
        public WaypointInfo(Vector3 position, float speed);
    }

    public static bool IsLayerBlocked(Vector3 position, float radius, int mask);
    [ChatCommand("npc_sound")]
    private void ChatCommandNpctalk(BasePlayer player, string cmd, string[] args);
    private void Add(BasePlayer player, string[] args);
    private void Save(BasePlayer player);
    private void Reset(BasePlayer player);
    public class NpcSound
    {
        [JsonConverter(typeof(SoundFileConverter))]
        public List<byte[]> Data;
    }

    private class SoundFileConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private byte[] ToSaveData(List<byte[]> data);
    private NpcSound LoadDataSound(string name);
    private List<byte[]> FromSaveData(byte[] bytes);
    private NpcSound LoadData(string name);
    private class RecordingData : NpcSound
    {
        public string Name { get; set; }
    }

    private bool FileExists(string name);
    private void SaveSoundData(string name, NpcSound data);
    private void OnPlayerVoice(BasePlayer player, byte[] data);
    public class SpawnInfo
    {
        public Vector3 position;
        public Quaternion rotation;
        public SpawnInfo(Vector3 position, Quaternion rotation);
        public string String();
        public string ShortString();
    }

    public class HumanTrigger : MonoBehaviour
    {
        private HumanPlayer npc;
        private readonly HashSet<BasePlayer> triggerPlayers;
        public float collisionRadius;
        private void Awake();
        private void OnDestroy();
        private void UpdateTriggerArea();
        private void OnEnterCollision(BasePlayer player);
        private void OnLeaveCollision(BasePlayer player);
    }

    public class HumanLocomotion : MonoBehaviour
    {
        private HumanPlayer npc;
        public Vector3 StartPos;
        public Vector3 EndPos;
        public Vector3 LastPos;
        public Dictionary<Vector3, float> NoMoveInfo;
        private Vector3 nextPos;
        private float waypointDone;
        public float secondsTaken;
        private float secondsToTake;
        public List<WaypointInfo> cachedWaypoints;
        private int currentWaypoint;
        public float followDistance;
        private float lastHit;
        public int noPath;
        public bool shouldMove;
        private float startedReload;
        private bool reloading;
        public bool returning;
        public bool sitting;
        public static List<BaseEntity> allChairs;
        public static List<Vector3> chairPosition;
        public BaseCombatEntity attackEntity;
        public BaseEntity followEntity;
        public Vector3 targetPosition;
        public List<Vector3> pathFinding;
        private HeldEntity firstWeapon;
        public void Awake();
        public void UpdateWaypoints();
        private void FixedUpdate();
        public void TryToMove();
        private void Execute_Move();
        public void Evade();
        public bool IsSwimming();
        public static bool checkChairPosition(Vector3 positionChair);
        private bool CanSit();
        public void Sit();
        public void Stand();
        private float GetSpeed(float speed);
        private void GetNextPath();
        public void SetMovementPoint(Vector3 startpos, Vector3 endpos, float s);
        private bool HitChance(float chance);
        private void Move(Vector3 position, float speed);
        private void ProcessAttack(BaseCombatEntity entity);
        public void ProcessFollow(Vector3 target);
        public void PathFinding();
        public void PathFinding(Vector3 targetPos);
        public void GetBackToLastPos();
        public void Enable();
        public void Disable();
        public float GetMoveY(Vector3 position);
        public float GetGroundY(Vector3 position);
        public void CreateProjectileEffect(BaseCombatEntity target, BaseProjectile baseProjectile, float dmg, bool miss);
        private void ApplyDamage(BaseCombatEntity entity, Vector3 point, Vector3 normal, BaseCombatEntity target);
        public void AttemptAttack(BaseCombatEntity entity);
        public void DoAttack(BaseCombatEntity target, bool miss);
    }

    public class HumanPlayer : MonoBehaviour
    {
        public HumanNPCInfo info;
        public HumanLocomotion locomotion;
        public HumanTrigger trigger;
        public ProtectionProperties protection;
        private HeldEntity heldEntity;
        private readonly List<NpcSound> _queuedSounds;
        private Coroutine QueuedRoutine { get; set; }
        public BasePlayer player;
        public float lastMessage;
        private void Awake();
        public void SetInfo(HumanNPCInfo info, bool update);
        public void QueueSound(NpcSound data);
        private IEnumerator RunTalker();
        private void SendSound(NetworkableId netId, byte[] data);
        public void PlayTune();
        public void PlayNote();
        public void UpdateHealth(HumanNPCInfo info);
        public void Evade();
        public void AllowMove();
        public void DisableMove();
        public void TemporaryDisableMove(float thetime);
        public void EndAttackingEntity(bool trigger);
        public void EndFollowingEntity(bool trigger);
        public void EndGo(bool trigger);
        public void StartAttackingEntity(BaseCombatEntity entity);
        public void StartFollowingEntity(BaseEntity entity, string pname);
        public void StartGo(Vector3 position);
        public HeldEntity GetCurrentWeapon();
        public Item GetFirstWeaponItem();
        public HeldEntity GetFirstWeapon();
        public HeldEntity GetFirstTool();
        public HeldEntity GetFirstMisc();
        public HeldEntity GetFirstInstrument();
        public List<Item> GetAmmo(Item item);
        public bool HasAmmo(Item item);
        public void UnequipAll();
        public HeldEntity EquipFirstWeapon();
        public HeldEntity EquipFirstTool();
        public HeldEntity EquipFirstMisc();
        public HeldEntity EquipFirstInstrument();
        public void SetActive(ItemId id);
        private void OnDestroy();
        public void LookTowards(Vector3 pos);
        public void ForceSignalGesture();
        public void ForceSignalAttack();
        public void SetViewAngle(Quaternion viewAngles);
    }

    public class HumanNPCInfo
    {
        public ulong userid;
        public string displayName;
        public bool invulnerability;
        public float health;
        public bool respawn;
        public float respawnSeconds;
        public SpawnInfo spawnInfo;
        public string waypoint;
        public float collisionRadius;
        public string spawnkit;
        public float damageAmount;
        public float damageDistance;
        public float damageInterval;
        public float attackDistance;
        public float maxDistance;
        public bool hostile;
        public float speed;
        public bool stopandtalk;
        public float stopandtalkSeconds;
        public bool enable;
        public bool lootable;
        public float hitchance;
        public float reloadDuration;
        public bool needsAmmo;
        public bool defend;
        public bool evade;
        public bool follow;
        public float evdist;
        public bool allowsit;
        public string musician;
        public bool playTune;
        public bool SoundOnEnter;
        public bool SoundOnUse;
        public float band;
        public string permission;
        public string Sound;
        public List<string> message_hello;
        public List<string> message_bye;
        public List<string> message_use;
        public List<string> message_hurt;
        public List<string> message_kill;
        public Dictionary<DamageType, float> protections;
        public HumanNPCInfo(ulong userid, Vector3 position, Quaternion rotation);
        public HumanNPCInfo Clone(ulong userid);
    }

    private class NPCEditor : MonoBehaviour
    {
        public BasePlayer player;
        public HumanPlayer targetNPC;
        private void Awake();
    }

    public static Dictionary<string, AmmoTypes> ammoTypes;
    private static Dictionary<string, BaseProjectile> weaponProjectile;
    private void Init();
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings settings { get; set; }
        public class Settings
        {
            public string Chat { get; set; }
            public bool NpcInTeams { get; set; }
            public float NpcUseRadius { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private static bool GetBoolValue(string value);
    private void Unload();
    private void SaveData();
    private void LoadData();
    private void OnServerInitialized();
    private void RespawnAllNpc();
    private IEnumerator respawnAllNPC();
    private void OnServerSave();
    private void OnServerShutdown();
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
     object CanDropActiveItem(BasePlayer player);
    private void OnEntityDeath(BaseEntity entity, HitInfo hitinfo);
    private void CreatNpcTeam(BasePlayer player);
    private void AddNpcTeam(BasePlayer player, BasePlayer teamPlayer);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private void OnLootPlayer(BasePlayer looter, BasePlayer target);
    private void OnLootEntity(BasePlayer looter, BaseEntity entity);
    private Dictionary<ulong, HumanPlayer> cache;
    public HumanPlayer FindHumanPlayerByID(ulong userid);
    public HumanPlayer FindHumanPlayer(string nameOrId);
    private BasePlayer FindPlayerByID(ulong userid);
    private void RefreshAllNPC();
    private void SpawnOrRefresh(ulong userid);
    private void SpawnNPC(ulong userid, bool isediting);
    private void UpdateInventory(HumanPlayer humanPlayer);
    private void KillNpc(BasePlayer player);
    public void RefreshNPC(BasePlayer player, bool isediting);
    public void UpdateNPC(BasePlayer player, bool isediting);
    private object CreateNPCHook(Vector3 position, Quaternion currentRot, string name, ulong clone, bool saved);
    public HumanPlayer CreateNPC(Vector3 position, Quaternion currentRot, string name, ulong clone, bool saved);
    public void RemoveNPC(ulong npcid);
    private bool hasAccess(BasePlayer player);
    private bool TryGetPlayerView(BasePlayer player, Quaternion viewAngle);
    private bool TryGetClosestRayPoint(Vector3 sourcePos, Quaternion sourceDir, object closestEnt, Vector3 closestHitpoint);
    private static bool CanSee(HumanPlayer npc, BaseEntity target1);
    private static string GetRandomMessage(List<string> messagelist);
    private static int GetRandom(int min, int max);
    private List<string> ListFromArgs(string[] args, int from);
    [ChatCommand("npc_add")]
    private void cmdChatNPCAdd(BasePlayer player, string command, string[] args);
    [ChatCommand("npc_way")]
    private void cmdChatNPCWay(BasePlayer player, string command, string[] args);
    [ChatCommand("npc_edit")]
    private void cmdChatNPCEdit(BasePlayer player, string command, string[] args);
    [ChatCommand("npc_list")]
    private void cmdChatNPCList(BasePlayer player, string command, string[] args);
    [ChatCommand("npc")]
    private void cmdChatNPC(BasePlayer player, string command, string[] args);
    [ChatCommand("npc_end")]
    private void cmdChatNPCEnd(BasePlayer player, string command, string[] args);
    [ChatCommand("npc_pathtest")]
    private void cmdChatNPCPathTest(BasePlayer player, string command, string[] args);
    [ChatCommand("npc_remove")]
    private void cmdChatNPCRemove(BasePlayer player, string command, string[] args);
    [ChatCommand("npc_reset")]
    private void cmdChatNPCReset(BasePlayer player, string command, string[] args);
    private void SendMessage(HumanPlayer npc, BasePlayer target, string message);
    private void OnEnterNPC(BasePlayer npc, BasePlayer player);
    private void OnLeaveNPC(BasePlayer npc, BasePlayer player);
    private class UnityQuaternionConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private class SpawnInfoConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanWrite { get; set; }
        public override bool CanConvert(Type objectType);
    }

}

private class StoredData
{
    public HashSet<HumanNPCInfo> HumanNPCs;
}

public class WaypointInfo
{
    public float Speed;
    public Vector3 Position;
    public WaypointInfo(Vector3 position, float speed);
}

public class NpcSound
{
    [JsonConverter(typeof(SoundFileConverter))]
    public List<byte[]> Data;
}

private class SoundFileConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

private class RecordingData : NpcSound
{
    public string Name { get; set; }
}

public class SpawnInfo
{
    public Vector3 position;
    public Quaternion rotation;
    public SpawnInfo(Vector3 position, Quaternion rotation);
    public string String();
    public string ShortString();
}

public class HumanTrigger : MonoBehaviour
{
    private HumanPlayer npc;
    private readonly HashSet<BasePlayer> triggerPlayers;
    public float collisionRadius;
    private void Awake();
    private void OnDestroy();
    private void UpdateTriggerArea();
    private void OnEnterCollision(BasePlayer player);
    private void OnLeaveCollision(BasePlayer player);
}

public class HumanLocomotion : MonoBehaviour
{
    private HumanPlayer npc;
    public Vector3 StartPos;
    public Vector3 EndPos;
    public Vector3 LastPos;
    public Dictionary<Vector3, float> NoMoveInfo;
    private Vector3 nextPos;
    private float waypointDone;
    public float secondsTaken;
    private float secondsToTake;
    public List<WaypointInfo> cachedWaypoints;
    private int currentWaypoint;
    public float followDistance;
    private float lastHit;
    public int noPath;
    public bool shouldMove;
    private float startedReload;
    private bool reloading;
    public bool returning;
    public bool sitting;
    public static List<BaseEntity> allChairs;
    public static List<Vector3> chairPosition;
    public BaseCombatEntity attackEntity;
    public BaseEntity followEntity;
    public Vector3 targetPosition;
    public List<Vector3> pathFinding;
    private HeldEntity firstWeapon;
    public void Awake();
    public void UpdateWaypoints();
    private void FixedUpdate();
    public void TryToMove();
    private void Execute_Move();
    public void Evade();
    public bool IsSwimming();
    public static bool checkChairPosition(Vector3 positionChair);
    private bool CanSit();
    public void Sit();
    public void Stand();
    private float GetSpeed(float speed);
    private void GetNextPath();
    public void SetMovementPoint(Vector3 startpos, Vector3 endpos, float s);
    private bool HitChance(float chance);
    private void Move(Vector3 position, float speed);
    private void ProcessAttack(BaseCombatEntity entity);
    public void ProcessFollow(Vector3 target);
    public void PathFinding();
    public void PathFinding(Vector3 targetPos);
    public void GetBackToLastPos();
    public void Enable();
    public void Disable();
    public float GetMoveY(Vector3 position);
    public float GetGroundY(Vector3 position);
    public void CreateProjectileEffect(BaseCombatEntity target, BaseProjectile baseProjectile, float dmg, bool miss);
    private void ApplyDamage(BaseCombatEntity entity, Vector3 point, Vector3 normal, BaseCombatEntity target);
    public void AttemptAttack(BaseCombatEntity entity);
    public void DoAttack(BaseCombatEntity target, bool miss);
}

public class HumanPlayer : MonoBehaviour
{
    public HumanNPCInfo info;
    public HumanLocomotion locomotion;
    public HumanTrigger trigger;
    public ProtectionProperties protection;
    private HeldEntity heldEntity;
    private readonly List<NpcSound> _queuedSounds;
    private Coroutine QueuedRoutine { get; set; }
    public BasePlayer player;
    public float lastMessage;
    private void Awake();
    public void SetInfo(HumanNPCInfo info, bool update);
    public void QueueSound(NpcSound data);
    private IEnumerator RunTalker();
    private void SendSound(NetworkableId netId, byte[] data);
    public void PlayTune();
    public void PlayNote();
    public void UpdateHealth(HumanNPCInfo info);
    public void Evade();
    public void AllowMove();
    public void DisableMove();
    public void TemporaryDisableMove(float thetime);
    public void EndAttackingEntity(bool trigger);
    public void EndFollowingEntity(bool trigger);
    public void EndGo(bool trigger);
    public void StartAttackingEntity(BaseCombatEntity entity);
    public void StartFollowingEntity(BaseEntity entity, string pname);
    public void StartGo(Vector3 position);
    public HeldEntity GetCurrentWeapon();
    public Item GetFirstWeaponItem();
    public HeldEntity GetFirstWeapon();
    public HeldEntity GetFirstTool();
    public HeldEntity GetFirstMisc();
    public HeldEntity GetFirstInstrument();
    public List<Item> GetAmmo(Item item);
    public bool HasAmmo(Item item);
    public void UnequipAll();
    public HeldEntity EquipFirstWeapon();
    public HeldEntity EquipFirstTool();
    public HeldEntity EquipFirstMisc();
    public HeldEntity EquipFirstInstrument();
    public void SetActive(ItemId id);
    private void OnDestroy();
    public void LookTowards(Vector3 pos);
    public void ForceSignalGesture();
    public void ForceSignalAttack();
    public void SetViewAngle(Quaternion viewAngles);
}

public class HumanNPCInfo
{
    public ulong userid;
    public string displayName;
    public bool invulnerability;
    public float health;
    public bool respawn;
    public float respawnSeconds;
    public SpawnInfo spawnInfo;
    public string waypoint;
    public float collisionRadius;
    public string spawnkit;
    public float damageAmount;
    public float damageDistance;
    public float damageInterval;
    public float attackDistance;
    public float maxDistance;
    public bool hostile;
    public float speed;
    public bool stopandtalk;
    public float stopandtalkSeconds;
    public bool enable;
    public bool lootable;
    public float hitchance;
    public float reloadDuration;
    public bool needsAmmo;
    public bool defend;
    public bool evade;
    public bool follow;
    public float evdist;
    public bool allowsit;
    public string musician;
    public bool playTune;
    public bool SoundOnEnter;
    public bool SoundOnUse;
    public float band;
    public string permission;
    public string Sound;
    public List<string> message_hello;
    public List<string> message_bye;
    public List<string> message_use;
    public List<string> message_hurt;
    public List<string> message_kill;
    public Dictionary<DamageType, float> protections;
    public HumanNPCInfo(ulong userid, Vector3 position, Quaternion rotation);
    public HumanNPCInfo Clone(ulong userid);
}

private class NPCEditor : MonoBehaviour
{
    public BasePlayer player;
    public HumanPlayer targetNPC;
    private void Awake();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings settings { get; set; }
    public class Settings
    {
        public string Chat { get; set; }
        public bool NpcInTeams { get; set; }
        public float NpcUseRadius { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class Settings
{
    public string Chat { get; set; }
    public bool NpcInTeams { get; set; }
    public float NpcUseRadius { get; set; }
}

private class UnityQuaternionConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

private class SpawnInfoConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanWrite { get; set; }
    public override bool CanConvert(Type objectType);
}


```

---

## HuntRPG by  - An RPG system with level, stats, skills, and specializations

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Serialization;
using UnityEngine;
using Oxide.Plugins.HuntRPGExt;
using Oxide.Plugins.HuntRPGExt.Keys;
using Random = UnityEngine.Random;
using Time = UnityEngine.Time;

Oxide.Plugins
[Info("Hunt RPG", "Default", "1.8.3")]
[Description("An RPG system with level, stats, skills, and specializations")]
public class HuntRPG : RustPlugin
{
    [PluginReference]
    private Plugin Pets;
    private Plugin EventManager;
    private Plugin PopupNotifications;
    private Plugin Economics;
    private Plugin MagazinBoost;
    private Plugin InstantCraft;
    private bool initialized;
    private bool updatePlayerData;
    private readonly DynamicConfigFile huntDataFile;
    private VersionNumber DataVersion;
    private HuntData Data;
    private string ChatPrefix;
    private ulong[] Trainer;
    private Dictionary<HRK, Skill> SkillTable;
    private Dictionary<ResourceDispenser.GatherType, float> ExpRateTable;
    private Dictionary<int, string> TameTable;
    private Dictionary<string, ItemInfo> ItemTable;
    private Dictionary<string, int> ResearchTable;
    private Dictionary<BuildingGrade.Enum, float> UpgradeBuildingTable;
    private string[] AllowedEntites;
    private bool AdminReset;
    private bool ShowHud;
    private bool ShowProfile;
    private uint DefaultHud;
    private float NightXP;
    private float DeleteProfileAfter;
    private float DeathReducer;
    private Dictionary<string, string> itemShortname;
    private readonly Dictionary<ulong, float> PlayerLastPercentChange;
    private readonly Dictionary<ulong, Dictionary<HRK, float>> SkillsCooldowns;
    private readonly Dictionary<ulong, GUIInfo> GUIInfo;
    private readonly int playersMask;
    private readonly int triggerMask;
    private double EcoBoost;
    private int MaximumLevel;
    private Vector3 position;
    public HuntRPG();
    private void OnServerInitialized();
     void Unload();
     void OnTerrainInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo, BaseCombatEntity entity);
    private object OnPlayerAttack(BasePlayer player, HitInfo hitInfo);
    private static Vector3 blinkArrow(HitInfo hitInfo);
     void OnEntityKill(BaseNetworkable networkable);
    private void OnEntityDeath(BasePlayer player, HitInfo hitInfo);
     object OnItemCraft(ItemCraftTask task, BasePlayer crafter);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnCollectiblePickup(Item item, BasePlayer player);
    private void OnItemDeployed(Deployer deployer, BaseEntity baseEntity);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnPlayerHealthChange(BasePlayer player);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private object OnOvenToggle(BaseOven oven, BasePlayer player);
    private void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
     object OnStructureUpgrade(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum grade);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnUseNPC(BasePlayer npc, BasePlayer player);
    private void OnServerSave();
    private void OnServerShutdown();
     bool GiveEXP(BasePlayer player, int xp);
    private bool OnAttackedInternal(BasePlayer player, HitInfo hitInfo);
    private void OnEntityDeployedInternal(BasePlayer player, BaseEntity entity, IDictionary<string, ulong> data);
    [ChatCommand("h")]
     void cmdHuntShortcut(BasePlayer player, string command, string[] args);
    [ChatCommand("hunt")]
     void cmdHunt(BasePlayer player, string command, string[] args);
    [ChatCommand("hg")]
     void cmdHuntGui(BasePlayer player, string command, string[] args);
    [ConsoleCommand("hunt.cmd")]
    private void cmdCmd(ConsoleSystem.Arg arg);
    [ConsoleCommand("hunt.saverpg")]
    private void cmdSaveRPG(ConsoleSystem.Arg arg);
    [ConsoleCommand("hunt.resetrpg")]
    private void cmdResetRPG(ConsoleSystem.Arg arg);
    [ConsoleCommand("hunt.lvlup")]
    private void cmdLevelUp(ConsoleSystem.Arg arg);
    [ConsoleCommand("hunt.lvlreset")]
    private void cmdLvlReset(ConsoleSystem.Arg arg);
    [ConsoleCommand("hunt.statreset")]
    private void cmdStatReset(ConsoleSystem.Arg arg);
    [ConsoleCommand("hunt.skillreset")]
    private void cmdSkillReset(ConsoleSystem.Arg arg);
    [ConsoleCommand("hunt.genxptable")]
    private void cmdGenerateXPTable(ConsoleSystem.Arg arg);
    private void HandleChatCommand(BasePlayer player, string[] args, bool npc);
    private void ListSkills(BasePlayer player);
    private void SkillInfo(BasePlayer player, StringBuilder sb, Skill skill, int cut);
    private void ShowTop(BasePlayer player);
    private void ToggleAutoBlinkArrow(BasePlayer player, RPGInfo rpgInfo);
    private void ToggleBlinkArrow(BasePlayer player, RPGInfo rpgInfo);
    private void ToggleCraftMessage(BasePlayer player, RPGInfo rpgInfo);
    private void ToggleShowProfile(BasePlayer player, RPGInfo rpgInfo);
    private void ToggleShowHud(BasePlayer player, RPGInfo rpgInfo);
    private void DisplaySkillCommand(BasePlayer player, string[] args);
    private void SetSkillsCommand(BasePlayer player, string[] args, RPGInfo rpgInfo, bool npc);
    private void SetStatsCommand(BasePlayer player, string[] args, RPGInfo rpgInfo, bool npc);
    private void LevelUpChatHandler(BasePlayer player, string[] args, RPGInfo rpgInfo);
    private void ChangePlayerXPMessagePreference(BasePlayer player, string[] args, RPGInfo rpgInfo);
    private bool IsAdmin(ConsoleSystem.Arg arg);
    protected override void LoadDefaultConfig();
    private void DefaultConfig();
    private void DefaultItems();
    private void UpdateLang();
    private void UpdateData();
    private void LoadRpg(VersionNumber dataVersion);
    private T ReadFromConfig(string configKey);
    private object GetConfig(string key, object defaultValue);
    private void SaveRpg(bool showMsgs);
    private void GuiInit(BasePlayer player);
    private static CuiLabel CreateLabel(string text, int i, float rowHeight, TextAnchor align, int fontSize, string xMin, string xMax, string color);
    private static CuiButton CreateButton(string command, int i, float rowHeight, int fontSize, string content, string xMin, string xMax);
    private static CuiPanel CreatePanel(string anchorMin, string anchorMax, string color);
    private void NpcGui(BasePlayer player, bool repaint);
    private void ProfileGui(BasePlayer player, bool repaint);
    private void UpdateHud(BasePlayer player, bool repaint, bool points);
    private void ExpGain(RPGInfo rpgInfo, int experience, BasePlayer player);
    private void NotifyLevelUp(BasePlayer player, RPGInfo rpgInfo);
    private string Profile(RPGInfo rpgInfo, BasePlayer player);
    private void UpdateGatherPlayer(BasePlayer player, RPGInfo rpgInfo);
    private void UpdateMagazinPlayer(BasePlayer player, RPGInfo rpgInfo);
    private void UpdateEffectsPlayer(BasePlayer player, RPGInfo rpgInfo);
    private void UpdateGather(Item item, RPGInfo rpgInfo);
    private void UpdateGatherPropertyEntry(ResourceDispenser.GatherPropertyEntry entry, ResourceDispenser.GatherPropertyEntry defaultEntry, RPGInfo rpgInfo, HRK skillType);
    private void UpdateMagazin(Item item, RPGInfo rpgInfo);
    private void SetCooldown(int skillPoints, float time, Dictionary<HRK, float> playerCooldowns, HRK skillKey);
    private Dictionary<HRK, float> PlayerCooldowns(ulong steamId);
    private float GatherModifier(int skillpoints, HRK skillType);
    private int GatherModifierInt(int skillpoints, HRK skillType, int value);
    private float CooldownModifier(int skillpoints, HRK skillKey, float currenttime);
    private string _(HMK key, object[] args);
    private string _(HMK key, BasePlayer player, object[] args);
    private string _(HMK key, string userid, object[] args);
    private void ChatMessage(BasePlayer player, string message);
    private void ChatMessage(BasePlayer player, HMK key, object[] args);
    private RPGInfo FindRpgInfo(BasePlayer player);
    private RPGInfo FindRpgInfo(ulong userId);
    private bool IsBuildingAllowed(Vector3 position, BasePlayer player);
    private string XPProgression(BasePlayer player, RPGInfo rpgInfo);
    private float CurrentPercent(RPGInfo rpgInfo);
    private bool IsNPCInRange(Vector3 pos);
    private static void TeleportPlayerTo(BasePlayer player, Vector3 position);
    private static Vector3 GetGround(Vector3 position);
    private static bool IsNight();
    private static bool IsSkillReady(Dictionary<HRK, float> playerCooldowns, float availableAt, float time, HRK skillKey);
    private static BasePlayer FindPlayer(string nameOrIdOrIp);
    private static BasePlayer FindPlayer(ulong userId);
    private static string TimeLeft(float availableAt, float time);
    private static string EntityId(BaseEntity entity);
    private static string EntityName(BaseEntity entity);
    private void DestroyUi(BasePlayer player, string name);
    public class IgnoreJsonPropertyResolver : DefaultContractResolver
    {
        private Dictionary<string, string> PropertyMappings { get; set; }
        public IgnoreJsonPropertyResolver();
        protected string ResolvePropertyName(string propertyName);
    }

}

public class IgnoreJsonPropertyResolver : DefaultContractResolver
{
    private Dictionary<string, string> PropertyMappings { get; set; }
    public IgnoreJsonPropertyResolver();
    protected string ResolvePropertyName(string propertyName);
}

Oxide.Plugins.HuntRPGExt
public static class HuntTablesGenerator
{
    public static Dictionary<int, long> GenerateXPTable(int maxLevel, int baseExp, float levelMultiplier, int levelModule, float moduleReducer);
    public static Dictionary<HRK, Skill> GenerateSkillTable();
    public static Dictionary<string, ItemInfo> GenerateItemTable(Dictionary<string, ItemInfo> itemDict);
    public static Dictionary<string, int> GenerateResearchTable();
    public static Dictionary<int, string> GenerateTameTable();
    public static Dictionary<BuildingGrade.Enum, float> GenerateUpgradeBuildingTable();
    public static Dictionary<string, object> GenerateDefaults();
    public static Dictionary<HRK, Modifier> GenerateMaxStatsTable();
    public static Dictionary<ResourceDispenser.GatherType, float> GenerateExpRateTable();
    public static List<string> GenerateAllowedEntites();
}

public class ProfilePreferences
{
    [JsonProperty("sxpmp")]
    public float ShowXPMessagePercent;
    [JsonProperty("sp")]
    public bool ShowProfile;
    [JsonProperty("scm")]
    public bool ShowCraftMessage;
    [JsonProperty("sh")]
    public uint ShowHud;
    [JsonProperty("uba")]
    public bool UseBlinkArrow;
    [JsonProperty("atba")]
    public bool AutoToggleBlinkArrow;
    public ProfilePreferences(uint defaultHud, bool showProfile);
}

public class GUIInfo
{
    public string LastMain;
    public string LastContent;
    public string LastInfo;
    public string LastStats;
    public string LastSkills;
    public string LastHud;
    public string LastHudFirst;
    public string LastHudSecond;
    public float LastHudTime;
    public Timer LastHudTimer;
}

public class RPGInfo
{
    private float evasionCache;
    private int agility;
    [JsonProperty("agi")]
    public int Agility { get; set; }
    private float blockCache;
    private int strength;
    [JsonProperty("str")]
    public int Strength { get; set; }
    private float craftCache;
    private int intelligence;
    [JsonProperty("int")]
    public int Intelligence { get; set; }
    [JsonProperty("sn")]
    public string SteamName;
    [JsonProperty("l")]
    public int Level;
    [JsonProperty("xp")]
    public long Experience;
    [JsonProperty("statp")]
    public int StatsPoints;
    [JsonProperty("skillp")]
    public int SkillPoints;
    [JsonProperty("s")]
    public Dictionary<HRK, int> Skills;
    [JsonProperty("p")]
    public ProfilePreferences Preferences;
    [JsonProperty("ls")]
    public long LastSeen;
    private ulong userId;
    public static long[] XPTable;
    public static Dictionary<HRK, Modifier> MaxStatsTable;
    public static Dictionary<int, string> TameTable;
    public static float SkillPointsGain;
    public static float StatPointsGain;
    public static int SkillPointsPerLevel;
    public static int StatPointsPerLevel;
    public static int MaxLevel;
    public static Permission Perm;
    private static int[] skillCostCache;
    private static int[] statCostCache;
    public RPGInfo(string steamName, uint defaultHud, bool showProfile);
    public void SetUserId(ulong userId);
    public bool AddExperience(long xp);
    public void LevelUp(int desiredLevel);
    public long RequiredExperience();
    public void Died(float percent);
    private void LevelUp();
    public bool AddStat(HRK stat, int points);
    public void Reset();
    public void ResetStats();
    public void ResetSkills();
    public int AddSkill(Skill skill, int level, HMK reason, Plugin pets);
    public float GetEvasion();
    public float GetBlock();
    public float GetCraftingReducer();
    public int GetSkillPointsCostNext(Skill skill, int levelsToAdd);
    private static int GetSkillPointsCost(Skill skill, int level, int add);
    public int GetStatPointsCost(HRK stat, int add);
    public static void OnUnload();
}

public class Skill
{
    public string Name;
    public HRK Type;
    public bool Enabled;
    public HMK Description;
    public HMK Usage;
    public int RequiredLevel;
    public int MaxLevel;
    public List<Dictionary<HRK, int>> RequiredSkills;
    public Dictionary<HRK, Modifier> Modifiers;
    public List<Dictionary<string, int>> RequiredStats;
    public int SkillPointsPerLevel;
    public Skill(HRK type, HMK description, int requiredLevel, int maxLevel);
    public void AddRequiredStat(string stat, int points, int level);
    public void AddRequiredSkill(HRK skillName, int pointsNeeded, int level);
    public void AddModifier(HRK modifier, Modifier handler);
}

public class HuntData
{
    [JsonProperty(HK.Furnaces)]
    public Dictionary<string, ulong> Furnaces { get; set; }
    [JsonProperty(HK.Quarries)]
    public Dictionary<string, ulong> Quarries { get; set; }
    [JsonProperty(HK.Profile)]
    public Dictionary<ulong, RPGInfo> Profiles { get; set; }
}

public class HuntDefaults
{
    public float SkillPointsGain;
    public float StatPointsGain;
    public int SkillPointsPerLevel;
    public int StatPointsPerLevel;
    public int MaximumLevel;
    public HuntDefaults();
}

Oxide.Plugins.HuntRPGExt.Keys
static class HK
{
    public const string Defaults;
    public const string SkillPointsGain;
    public const string StatPointsGain;
    public const string DeleteProfileAfterOfflineDays;
    public const string ShowHud;
    public const string ShowProfile;
    public const string DefaultHud;
    public const string ConfigVersion;
    public const string DataVersion;
    public const string DataFileName;
    public const string Profile;
    public const string Furnaces;
    public const string Quarries;
    public const string ChatPrefix;
    public const string XPTable;
    public const string ExpRateTable;
    public const string MaxStatsTable;
    public const string SkillTable;
    public const string ItemTable;
    public const string ResearchSkillTable;
    public const string TameTable;
    public const string UpgradeBuildTable;
    public const string AllowedEntities;
    public const string AdminReset;
    public const string NightXP;
    public const string EcoBoost;
    public const string Trainer;
    public const string DeathReducerK;
    public const string SkillPointsPerLevel;
    public const string StatPointsPerLevel;
    public const string MaximumLevel;
}

static class HKD
{
    public const int MaximumLevel;
    public const float SkillPointsGain;
    public const float StatPointsGain;
    public const int SkillPointsPerLevel;
    public const int StatPointsPerLevel;
    public const int BaseXp;
    public const float LevelMultiplier;
    public const int LevelModule;
    public const float ModuleReducer;
    public const float DeathReducer;
}

public static class HPK
{
    public const string CanTame;
    public const string CanTameChicken;
    public const string CanTameBoar;
    public const string CanTameStag;
    public const string CanTameWolf;
    public const string CanTameBear;
    public const string CanTameHorse;
}


```

---

## HWCustomMapName by klauz24 - Allows you to set custom map name for your server at the server list

```csharp
using System;
using Steamworks;
using System.Linq;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("HW Custom Map Name", "klauz24", "1.5.3"), Description("Changes map name at the server list.")]
internal class HWCustomMapName : CovalencePlugin
{
    private DateTime _nextWipe;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Map name")]
        public string MapName;
        [JsonProperty("Random map name")]
        public RandomMapName RMN;
        [JsonProperty("Wipe countdown")]
        public WipeCountdown WPC;
        public class RandomMapName
        {
            [JsonProperty("Enable random map name")]
            public bool EnableRandomMapName;
            [JsonProperty(PropertyName = "Random map names")]
            public List<string> RandomMapNames { get; set; }
        }

        public class WipeCountdown
        {
            [JsonProperty("Enable wipe countdown")]
            public bool EnableWipeCountdown;
            [JsonProperty("Wipe countdown format")]
            public string WipeCountdownFormat;
            [JsonProperty("Next wipe year")]
            public int NextWipeYear;
            [JsonProperty("Next wipe month")]
            public int NextWipeMonth;
            [JsonProperty("Next wipe day")]
            public int NextWipeDay;
            [JsonProperty("Next wipe hour")]
            public int NextWipeHours;
            [JsonProperty("Next wipe minutes")]
            public int NextWipeMinutes;
        }

        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private string GetMapName();
    private string FormatTime(double time);
    private string Lang(string key);
    private void UpdateMapName(string str);
}

private class Configuration
{
    [JsonProperty("Map name")]
    public string MapName;
    [JsonProperty("Random map name")]
    public RandomMapName RMN;
    [JsonProperty("Wipe countdown")]
    public WipeCountdown WPC;
    public class RandomMapName
    {
        [JsonProperty("Enable random map name")]
        public bool EnableRandomMapName;
        [JsonProperty(PropertyName = "Random map names")]
        public List<string> RandomMapNames { get; set; }
    }

    public class WipeCountdown
    {
        [JsonProperty("Enable wipe countdown")]
        public bool EnableWipeCountdown;
        [JsonProperty("Wipe countdown format")]
        public string WipeCountdownFormat;
        [JsonProperty("Next wipe year")]
        public int NextWipeYear;
        [JsonProperty("Next wipe month")]
        public int NextWipeMonth;
        [JsonProperty("Next wipe day")]
        public int NextWipeDay;
        [JsonProperty("Next wipe hour")]
        public int NextWipeHours;
        [JsonProperty("Next wipe minutes")]
        public int NextWipeMinutes;
    }

    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class RandomMapName
{
    [JsonProperty("Enable random map name")]
    public bool EnableRandomMapName;
    [JsonProperty(PropertyName = "Random map names")]
    public List<string> RandomMapNames { get; set; }
}

public class WipeCountdown
{
    [JsonProperty("Enable wipe countdown")]
    public bool EnableWipeCountdown;
    [JsonProperty("Wipe countdown format")]
    public string WipeCountdownFormat;
    [JsonProperty("Next wipe year")]
    public int NextWipeYear;
    [JsonProperty("Next wipe month")]
    public int NextWipeMonth;
    [JsonProperty("Next wipe day")]
    public int NextWipeDay;
    [JsonProperty("Next wipe hour")]
    public int NextWipeHours;
    [JsonProperty("Next wipe minutes")]
    public int NextWipeMinutes;
}


```

---

## Identifier by MrBlue - Gets identification information for one or all connected players

```csharp
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Identifier", "Wulf", "2.0.3")]
[Description("Gets identification information for one or all connected players")]
public class Identifier : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Use permission system")]
        public bool UsePermissions;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private const string permId;
    private const string permIpAddress;
    private void Init();
    private void CommandId(IPlayer player, string command, string[] args);
    private void CommandIdAll(IPlayer player, string command, string[] args);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

public class Configuration
{
    [JsonProperty("Use permission system")]
    public bool UsePermissions;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

