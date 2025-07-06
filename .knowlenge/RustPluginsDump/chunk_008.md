# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 8 - Generated: 2025-07-06 20:39:06

## VoteChecker

```csharp
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Collections.ObjectModel;
using System.Linq;

Oxide.Plugins
[Info("VoteChecker", "Pho3niX90", "2.2.2", ResourceId = 1216)]
 class VoteChecker : RustPlugin
{
    static class Constants
    {
        public const string PLATFORM;
        public const string SERVICE;
        public const bool DEBUG;
    }

     int timesVoted;
    private Dictionary<ulong, DateTime> Cooldowns;
    private Collection<RewardItem> Rewards;
    public Collection<LastVote> Users;
    private Dictionary<string, string> shortnameDictionary;
    public class RewardItem
    {
        public RewardItem(string itemName, int votesRequired, int itemAmount);
        public string itemName { get; set; }
        public int votesRequired { get; set; }
        public int itemAmount { get; set; }
    }

    public class LastVote
    {
        public LastVote(ulong steamid, int lastvote);
        public ulong steamid { get; set; }
        public int lastvote { get; set; }
    }

    private Collection<RewardItem> LoadDefaultRewards();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
    private void InitializeTable();
     void Loaded();
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("addreward")]
    private void ChatCmd_AddReward(BasePlayer player, string cmd, string[] args);
    [ChatCommand("getreward")]
    private void GetMyVoteReward(BasePlayer player, string cmd);
    private readonly WebRequests webRequestsAddress;
    private readonly WebRequests webRequestsApi;
    [ChatCommand("rewardconf")]
    private void ChatCmd_Config(BasePlayer player, string command, string[] args);
    [ChatCommand("clearrewards")]
    private void ChatCmd_ClearRewards(BasePlayer player, string cmd);
    [ChatCommand("itemlist")]
    private void getItemList(BasePlayer player, string cmd);
    [ChatCommand("voterewards")]
    private void ChatCmd_Rewards(BasePlayer player, string cmd);
    private void SaveRewards();
    private void LoadRewards();
    private void SaveVotes();
    private void LoadVotes();
    private readonly WebRequests webRequests;
     void GetRewardDelayed(BasePlayer player);
     void GetRewardsForThisPlayer(BasePlayer player);
     void WebRequestCallbackAddress(int code, string response, BasePlayer player);
     void WebRequestCallbackApi(int code, string response, BasePlayer player);
     void WebRequestCallback(int code, string response, BasePlayer player);
     void giveItems(BasePlayer player, int voteCount);
    private string Capitalise(string word);
    private void Debug(int level, string msg);
    private readonly WebRequests wrAnnoucePlugin;
    private void AnnouncePlugins();
     void AnnoucePluginCallback(int code, string response, string title);
}

static class Constants
{
    public const string PLATFORM;
    public const string SERVICE;
    public const bool DEBUG;
}

public class RewardItem
{
    public RewardItem(string itemName, int votesRequired, int itemAmount);
    public string itemName { get; set; }
    public int votesRequired { get; set; }
    public int itemAmount { get; set; }
}

public class LastVote
{
    public LastVote(ulong steamid, int lastvote);
    public ulong steamid { get; set; }
    public int lastvote { get; set; }
}


```

---

## VoteDay

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("VoteDay", "S1m0n", "1.0.0")]
 class VoteDay : RustPlugin
{
    private List<ulong> votesReceived;
    static Dictionary<string, string> imageIds;
    private bool voteOpen;
    private bool isWaiting;
    private int timeRemaining;
    private int requiredVotes;
    private Timer voteTimer;
    private Timer timeMonitor;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        public static string Color(string hexColor, float alpha);
    }

    private const string Main;
    private void CreateTimeUI(BasePlayer player);
    private void RefreshAllUI();
     void Loaded();
     void OnServerInitialized();
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
    private void OpenVote();
    private void VoteTimer();
    private void MessageAll();
    private string GetFormatTime();
    private void CheckTime();
    private void TallyVotes();
    private void VoteEnd(bool success);
    private bool AlreadyVoted(BasePlayer player);
    [ChatCommand("voteday")]
    private void cmdVoteDay(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class Colors
    {
        public string MainColor { get; set; }
        public string MSGColor { get; set; }
        public string ProgressBarColor { get; set; }
        public string UIBackgroundColor { get; set; }
        public float UIBackgroundAlpha { get; set; }
    }

     class Options
    {
        public float RequiredVotePercentage { get; set; }
        public float TimeToOpen { get; set; }
        public float TimeToSet { get; set; }
        public int VoteOpenTime { get; set; }
    }

     class ConfigData
    {
        public Colors Colors { get; set; }
        public Options Options { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void Reply(BasePlayer player, string langKey);
     void Print(string langKey);
     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    public static string Color(string hexColor, float alpha);
}

 class Colors
{
    public string MainColor { get; set; }
    public string MSGColor { get; set; }
    public string ProgressBarColor { get; set; }
    public string UIBackgroundColor { get; set; }
    public float UIBackgroundAlpha { get; set; }
}

 class Options
{
    public float RequiredVotePercentage { get; set; }
    public float TimeToOpen { get; set; }
    public float TimeToSet { get; set; }
    public int VoteOpenTime { get; set; }
}

 class ConfigData
{
    public Colors Colors { get; set; }
    public Options Options { get; set; }
}


```

---

## VoteForMoney

```csharp
using System;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections;
using System.Collections.ObjectModel;
using System.Linq;
using System.Collections.Generic;

Oxide.Plugins
[Info("Vote For Money", "Frenk92", "0.5.3", ResourceId = 2086)]
 class VoteForMoney : RustPlugin
{
    [PluginReference]
     Plugin Economics;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Kits;
    const string permAdmin;
    const string site1;
    const string site2;
    const string site3;
    const string link1;
    const string link2;
    const string link3;
    public bool edit;
     string rustServersKey;
     string rustServersID;
     string topRustKey;
     string topRustID;
     string beancanKey;
     string beancanID;
     string voteType;
     int voteInterval;
     bool useRP;
     bool useEconomics;
     bool useKits;
     string prefix;
     Dictionary<string, string> kits;
     Dictionary<string, string> money;
     Dictionary<string, string> rp;
     string configVersion;
    protected override void LoadDefaultConfig();
     void LoadConfigData();
     void LoadDefaultMessages();
    public Collection<PlayerVote> Users;
    public class PlayerVote
    {
        public ulong UserId { get; set; }
        public string Name { get; set; }
        public Dictionary<string, SitesVote> Sites;
        public PlayerVote(ulong UserId, string Name);
    }

    public class SitesVote
    {
        public int Votes { get; set; }
        public string ExpDate { get; set; }
        public string Claimed { get; set; }
        public SitesVote(int Votes, string ExpDate, string Claimed);
    }

    private void LoadData();
    private void SaveData();
     void Init();
     void Loaded();
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("vote")]
    private void cmdVote(BasePlayer player, string command, string[] args);
     void NewPlayer(BasePlayer player);
     void GetRequest(BasePlayer player);
     void GetCallback(int code, string response, BasePlayer player, string site);
     void Claim(BasePlayer player, string site, PlayerVote data);
     void ClaimCheck(int code, string response, BasePlayer player, string site, PlayerVote data);
    private void CheckVote(BasePlayer player, int response, string site);
    private void GetMoney(BasePlayer player, string site);
     void SetConfig(string name, object data);
     object ReadConfig(string name, object data);
    private Dictionary<string, string> ConvertToDictionary(object obj, Dictionary<string, string> dict);
     bool HasPermission(string id, string perm);
     string Lang(string key, string id, object[] args);
     void MessageChat(BasePlayer player, string message, string args);
}

public class PlayerVote
{
    public ulong UserId { get; set; }
    public string Name { get; set; }
    public Dictionary<string, SitesVote> Sites;
    public PlayerVote(ulong UserId, string Name);
}

public class SitesVote
{
    public int Votes { get; set; }
    public string ExpDate { get; set; }
    public string Claimed { get; set; }
    public SitesVote(int Votes, string ExpDate, string Claimed);
}


```

---

## VPNBlock

```csharp
using System.Collections.Generic;
using System;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info ("VPN Block", "Calytic", "1.0.0")]
 class VPNBlock : CovalencePlugin
{
     List<string> allowedISPs;
     string IPHUB_API_KEY;
     string IPSTACK_API_KEY;
     string AA_API_KEY;
     Dictionary<string, string> headers;
     string unauthorizedMessage;
     bool debug;
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadData();
     void SaveData();
    [Command ("wisp")]
     void WhiteListISP(IPlayer player, string command, string [] args);
     void LoadMessages();
     bool IsAllowed(IPlayer player);
     bool hasAccess(IPlayer player, string permissionname);
     void OnUserConnected(IPlayer player);
     void HandleIPHubResponse(IPlayer player, string url, string ip, int code, string response);
     void FailResponse(string service, int code, string response);
     void HandleIPStackResponse(IPlayer player, string url, string ip, int code, string response);
     void HandleAAResponse(IPlayer player, string url, string ip, int code, string response);
     bool IsWhitelisted(string playerIsp);
     T GetConfig(string name, string name2, T defaultValue);
     T GetConfig(string name, T defaultValue);
     string GetMsg(string key, object userID);
}


```

---

## WakeUp

```csharp
using System;

Oxide.Plugins
[Info("WakeUp", "Virobeast", "0.0.3", ResourceId = 1487)]
[Description("Removes Your sleeping screen after hitting respawn")]
 class WakeUp : RustPlugin
{
     void OnPlayerRespawned(BasePlayer player);
}


```

---

## WantedForMurder

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using UnityEngine;
using Facepunch;
using Rust;
using Oxide.Core;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info( "Wanted For Murder", "Raptor007", "1.0.7" )]
[Description( "Players can punish unwanted PvP." )]
 class WantedForMurder : RustPlugin
{
     bool DataChanged;
     Dictionary< ulong, List<ulong> > Murderers;
     Dictionary< ulong, ulong > LastKillerOf;
     Dictionary< ulong, ulong > LastAttackerOf;
     Dictionary< string, string > EN;
    volatile bool DataLocked;
     void LockData();
     void UnlockData();
     void ChatToPlayer(BasePlayer player, string key, object[] args);
     void ChatToAll(string key, object[] args);
     bool SetUnconfiguredDefaults();
    protected override void LoadDefaultConfig();
     void LoadData();
     void SaveData();
     void OnServerSave();
     string GetPlayerNameFromID(ulong player_id);
     ulong GetPlayerIDFromName(string name);
     bool IsMurderer(ulong player_id);
     bool IsKiller(ulong player_id);
     bool PunishLastKillerOf(ulong player_id);
     void RemoveSleepingBagsFor(ulong player_id);
     void RemoveMurdererSleepingBags();
     void OnItemDeployed(Deployer deployer, BaseEntity entity);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void ShowWantedListTo(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
    [ChatCommand("wanted")]
     void WantedCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("punish")]
     void PunishCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("forgive")]
     void ForgiveCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("pardon")]
     void PardonCommand(BasePlayer player, string command, string[] args);
     void Loaded();
}


```

---

## Warps

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Warps", "Orange", "1.1.1")]
[Description("https://rustworkshop.space/resources/warps.180/")]
public class Warps : RustPlugin
{
    private static CollisionDetector[] AllWarps { get; set; }
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void cmdControlChat(BasePlayer player, string command, string[] args);
    private void CreateWarps();
    private void DestroyWarps();
    private void CreateWarp(WarpDefinition definition);
    private void OnPlayerJoined(BasePlayer player, WarpDefinition warp);
    private static void Teleport(BasePlayer player, Vector3 position);
    private static string GetReplacedString(string original, BasePlayer player, WarpDefinition warp);
    private static ConfigDefinition config;
    private class ConfigDefinition
    {
        [JsonProperty("Commands")]
        public string[] commands;
        [JsonProperty("Warps")]
        public List<WarpDefinition> warps;
    }

    private class WarpDefinition
    {
        [JsonProperty("Shortname")]
        public string shortname;
        [JsonProperty("Display Name")]
        public string displayName;
        [JsonProperty("Position")]
        public Vector3 position;
        [JsonProperty("Permission")]
        public string permission;
        [JsonProperty("Radius (meters)")]
        public float radius;
        [JsonProperty("Position to teleport")]
        public Vector3 positionToTeleport;
        [JsonProperty("Note")]
        public string note;
        [JsonProperty("Commands called to player")]
        public string[] commandsToPlayer;
        [JsonProperty("Commands called to server")]
        public string[] commandsToServer;
    }

    private partial class Message
    {
        private static Dictionary<Key, object> messages;
    }

    private void FillWarpsInformation(Dictionary<string, Vector3> warps);
    private Dictionary<string, Vector3> GetAllWarpsWithPositions();
    private string[] GetAllWarps();
    private class CollisionDetector : MonoBehaviour
    {
        private SphereCollider collider;
        public float Delay;
        public float Radius;
        public Action<GameObject> OnGameObjectEnter;
        public Action<GameObject> OnGameObjectExit;
        public Action<BasePlayer> OnPlayerEnter;
        public Action<BasePlayer> OnPlayerExit;
        public static CollisionDetector Create();
        public static CollisionDetector Create(BaseEntity entity);
        private void Start();
        private void OnDestroy();
        private void AddCollider();
        private void Enter(GameObject go);
        private void Exit(GameObject go);
        private void OnTriggerEnter(Collider component);
        private void OnCollisionEnter(Collision component);
        private void OnTriggerExit(Collider component);
        private void OnCollisionExit(Collision component);
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private partial class Message
    {
        private static RustPlugin plugin;
        private static Lang lang;
        private static ulong senderID;
        public static void ChangeSenderID(ulong newValue);
        public static void Load(Lang v1, RustPlugin v2);
        public static void Unload();
        public static void Console(string message, Type type);
        public static void Send(object receiver, string message, object[] args);
        public static void Send(object receiver, Key key, object[] args);
        public static void Broadcast(string message, object[] args);
        public static void Broadcast(Key key, object[] args);
        public static string GetMessage(Key key, string playerID, object[] args);
        public static string FormattedMessage(string message, object[] args);
        private static void SendMessage(object receiver, string message);
        private static Dictionary<string, object> OrganizeArgs(object[] args);
        private static string ReplaceArgs(string message, Dictionary<string, object> args);
    }

}

private class ConfigDefinition
{
    [JsonProperty("Commands")]
    public string[] commands;
    [JsonProperty("Warps")]
    public List<WarpDefinition> warps;
}

private class WarpDefinition
{
    [JsonProperty("Shortname")]
    public string shortname;
    [JsonProperty("Display Name")]
    public string displayName;
    [JsonProperty("Position")]
    public Vector3 position;
    [JsonProperty("Permission")]
    public string permission;
    [JsonProperty("Radius (meters)")]
    public float radius;
    [JsonProperty("Position to teleport")]
    public Vector3 positionToTeleport;
    [JsonProperty("Note")]
    public string note;
    [JsonProperty("Commands called to player")]
    public string[] commandsToPlayer;
    [JsonProperty("Commands called to server")]
    public string[] commandsToServer;
}

private partial class Message
{
    private static Dictionary<Key, object> messages;
}

private class CollisionDetector : MonoBehaviour
{
    private SphereCollider collider;
    public float Delay;
    public float Radius;
    public Action<GameObject> OnGameObjectEnter;
    public Action<GameObject> OnGameObjectExit;
    public Action<BasePlayer> OnPlayerEnter;
    public Action<BasePlayer> OnPlayerExit;
    public static CollisionDetector Create();
    public static CollisionDetector Create(BaseEntity entity);
    private void Start();
    private void OnDestroy();
    private void AddCollider();
    private void Enter(GameObject go);
    private void Exit(GameObject go);
    private void OnTriggerEnter(Collider component);
    private void OnCollisionEnter(Collision component);
    private void OnTriggerExit(Collider component);
    private void OnCollisionExit(Collision component);
}

private partial class Message
{
    private static RustPlugin plugin;
    private static Lang lang;
    private static ulong senderID;
    public static void ChangeSenderID(ulong newValue);
    public static void Load(Lang v1, RustPlugin v2);
    public static void Unload();
    public static void Console(string message, Type type);
    public static void Send(object receiver, string message, object[] args);
    public static void Send(object receiver, Key key, object[] args);
    public static void Broadcast(string message, object[] args);
    public static void Broadcast(Key key, object[] args);
    public static string GetMessage(Key key, string playerID, object[] args);
    public static string FormattedMessage(string message, object[] args);
    private static void SendMessage(object receiver, string message);
    private static Dictionary<string, object> OrganizeArgs(object[] args);
    private static string ReplaceArgs(string message, Dictionary<string, object> args);
}


```

---

## WarpSystem

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Linq;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Warp System", "PaiN", "1.9.8", ResourceId = 760)]
[Description("Create warp points for players.")]
 class WarpSystem : RustPlugin
{
    [PluginReference]
     Plugin Jail;
    private bool Changed;
    private int cooldown;
    private int warpbacktimer;
    private bool enablecooldown;
    private int backcmdauthlevel;
    private bool WarpIfRunning;
    private bool WarpIfWounded;
    private bool WarpIfSwimming;
    private bool WarpIfBuildingBlocked;
    private bool WarpIfDucking;
    private string backtolastloc;
    private string warplist;
    private string therealreadyis;
    private string warpadded;
    private string youhavetowait;
    private string cantwarpwhilerunning;
    private string cantwarpwhilewounded;
    private string cantwarpwhilebuildingblocked;
    private string cantwarpwhileducking;
    private string cantwarpwhileswimming;
    private string youhaveteleportedto;
    private string teleportingto;
    private string youhaveremoved;
     object GetConfig(string menu, string datavalue, object defaultValue);
     double GetTimeStamp();
     void LoadVariables();
    protected override void LoadDefaultConfig();
     class StoredData
    {
        public List<WarpInfo> WarpInfo;
        public Dictionary<ulong, double> cantele;
        public Dictionary<ulong, OldPosInfo> lastposition;
        public Dictionary<ulong, Dictionary<string, int>> maxuses;
    }

     class OldPosInfo
    {
        public float OldX;
        public float OldY;
        public float OldZ;
        public OldPosInfo(float x, float y, float z);
        public OldPosInfo();
    }

     class WarpInfo
    {
        public string WarpName;
        public int WarpId;
        public float WarpX;
        public float WarpY;
        public float WarpZ;
        public string WarpPermissionGroup;
        public int WarpTimer;
        public int WarpMaxUses;
        public string WarpCreatorName;
        public int RandomRange;
        public WarpInfo(string name, BasePlayer player, int timerp, string permissionp, int warpnum, int randomr, int maxusess);
        public WarpInfo();
    }

     StoredData storedData;
     void Loaded();
    [ConsoleCommand("warp.wipemaxuses")]
     void cmdWarpMaxUses(ConsoleSystem.Arg arg);
    [ConsoleCommand("warp.playerto")]
     void cmdWarpPlayerr(ConsoleSystem.Arg arg);
     int GetNewId();
     int GetRandomId(BasePlayer player);
    [ChatCommand("warp")]
     void cmdWarp(BasePlayer player, string cmdd, string[] args);
     void OnServerCommand(ConsoleSystem.Arg arg);
     void ForcePlayerPos(BasePlayer player, Vector3 xyz);
}

 class StoredData
{
    public List<WarpInfo> WarpInfo;
    public Dictionary<ulong, double> cantele;
    public Dictionary<ulong, OldPosInfo> lastposition;
    public Dictionary<ulong, Dictionary<string, int>> maxuses;
}

 class OldPosInfo
{
    public float OldX;
    public float OldY;
    public float OldZ;
    public OldPosInfo(float x, float y, float z);
    public OldPosInfo();
}

 class WarpInfo
{
    public string WarpName;
    public int WarpId;
    public float WarpX;
    public float WarpY;
    public float WarpZ;
    public string WarpPermissionGroup;
    public int WarpTimer;
    public int WarpMaxUses;
    public string WarpCreatorName;
    public int RandomRange;
    public WarpInfo(string name, BasePlayer player, int timerp, string permissionp, int warpnum, int randomr, int maxusess);
    public WarpInfo();
}


```

---

## WaterDisconnect

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;

Oxide.Plugins
[Info("WaterDisconnect", "Wulf/lukespragg", "2.2.0", ResourceId = 2122)]
[Description("Hurts or kills players that log out underwater")]
 class WaterDisconnect : CovalencePlugin
{
    const string permExclude;
     bool hurtOnLogout;
     bool hurtOverTime;
     bool killOnLogout;
     int damageAmount;
     int damageEvery;
     int waterPercent;
    protected override void LoadDefaultConfig();
     void Init();
    readonly FieldInfo modelStateInfo;
    readonly Dictionary<ulong, Timer> timers;
     void HandleDisconnect(BasePlayer player);
     void OnServerInitialized();
     void OnPlayerDisconnected(BasePlayer player);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
}


```

---

## WeaponsOnBack

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;
using Oxide.Plugins;
using System;

Oxide.Plugins
[Info("WeaponsShownOnBack", "Jake_Rich", 0.11)]
[Description("Shows player's best two weapons holstered on their back")]
public class WeaponsOnBack : RustPlugin
{
    public static WeaponsOnBack thisPlugin;
    public static int displayMode;
     void Loaded();
     void Unload();
    protected override void LoadDefaultConfig();
    public void Output(object[] text);
    public static void Write(object[] text);
    public static void Write(object text);
    public static Dictionary<BasePlayer, WeaponData> weaponData;
    public static Dictionary<BaseEntity, BasePlayer> weaponNetworking;
    public static List<BaseNetworkable> fakeEntities;
    public static List<BasePlayer> fakePlayers;
    [ChatCommand("weapondisplay")]
     void ConfigureWeaponsSettings(BasePlayer player, string command, string[] args);
     object CanNetworkTo(BaseNetworkable entity, BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     int playersPerTick;
     int lastPlayerIndex;
     DateTime lastTimeCompleted;
     void OnTick();
    public class WeaponData
    {
        public static Dictionary<string, GunConfig> gunSettings_main;
        public static Dictionary<string, GunConfig> gunSettings_holster;
        public static Dictionary<string, Vector3> shirtOffsetValues;
        public static Dictionary<string, Vector3> armourOffsetValues;
        public BasePlayer player;
        public BaseEntity mainWeapon;
        public BaseEntity sideWeapon;
        public string mainGun { get; set; }
        public string holsterGun { get; set; }
        public Item activeItem { get; set; }
        public Item oldActiveItem { get; set; }
        public BaseEntity SpawnEntity(string prefab, Vector3 pos, Quaternion rotation, BaseEntity parent, ulong skin);
        public WeaponData(BasePlayer player);
        public void Destroy();
        public void DestroyWeapons();
        public void DestroyWeapon(BaseEntity gun);
        public void NetworkUpdate();
        public void UpdateGuns();
        public void QuickUpdate();
        public void Update();
    }

    public class GunConfig
    {
        public Vector3 localPosition_slot1 { get; set; }
        public Quaternion localRotation_slot1 { get; set; }
        public Vector3 localPosition_slot2 { get; set; }
        public Quaternion localRotation_slot2 { get; set; }
        public int priority { get; set; }
        public string prefabName { get; set; }
        public GunConfig(Vector3 pos, Vector3 rotation, Vector3 slot2pos, Vector3 slot2rot, string prefab, int priority);
    }

}

public class WeaponData
{
    public static Dictionary<string, GunConfig> gunSettings_main;
    public static Dictionary<string, GunConfig> gunSettings_holster;
    public static Dictionary<string, Vector3> shirtOffsetValues;
    public static Dictionary<string, Vector3> armourOffsetValues;
    public BasePlayer player;
    public BaseEntity mainWeapon;
    public BaseEntity sideWeapon;
    public string mainGun { get; set; }
    public string holsterGun { get; set; }
    public Item activeItem { get; set; }
    public Item oldActiveItem { get; set; }
    public BaseEntity SpawnEntity(string prefab, Vector3 pos, Quaternion rotation, BaseEntity parent, ulong skin);
    public WeaponData(BasePlayer player);
    public void Destroy();
    public void DestroyWeapons();
    public void DestroyWeapon(BaseEntity gun);
    public void NetworkUpdate();
    public void UpdateGuns();
    public void QuickUpdate();
    public void Update();
}

public class GunConfig
{
    public Vector3 localPosition_slot1 { get; set; }
    public Quaternion localRotation_slot1 { get; set; }
    public Vector3 localPosition_slot2 { get; set; }
    public Quaternion localRotation_slot2 { get; set; }
    public int priority { get; set; }
    public string prefabName { get; set; }
    public GunConfig(Vector3 pos, Vector3 rotation, Vector3 slot2pos, Vector3 slot2rot, string prefab, int priority);
}


```

---

## WeatherController

```csharp
using UnityEngine;
using System;

Oxide.Plugins
[Info("Weather Controller", "Waizujin", 1.4)]
[Description("Allows you to control the weather.")]
public class WeatherController : RustPlugin
{
    private void Loaded();
    [ChatCommand("weather")]
    private void WeatherCommand(BasePlayer player, string command, string[] args);
    public void weather(BasePlayer player, string type, int status, bool quiet);
    public void mild(BasePlayer player, int status);
    public void average(BasePlayer player, int status);
    public void heavy(BasePlayer player, int status);
    public void max(BasePlayer player, int status);
     bool hasPermission(BasePlayer player, string perm);
}


```

---

## WeatherEvents

```csharp
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Weather Events", "OuTSMoKE, Rick", "1.4.0")]
[Description("Calls weather events with console commands.")]
 class WeatherEvents : RustPlugin
{
    public Timer StormTimer;
    public int time;
    public List<string> cmds;
     void Unload();
     void OnServerInitialized();
    [ConsoleCommand("we")]
     void cmdSpecial(ConsoleSystem.Arg arg);
     void NewStormTimer(string TypeOfStorm);
     void ProcessStorm(Dictionary<int, List<string>> StormData);
    public Dictionary<string, Dictionary<int, List<string>>> Storms;
}


```

---

## WelcomeHelp

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("WelcomeHelp", "Empty", "0.0.41")]
public class WelcomeHelp : RustPlugin
{
    private class Configs
    {
        public class Interface
        {
            [JsonProperty("Текст заголовка (над выделенным блоком), советую оставить пустым")]
            public string HeaderText;
            [JsonProperty("Текст в разделителе")]
            public string DelimiterText;
            [JsonProperty("Надпись в левом нижнем углу")]
            public string LeftDownText;
            [JsonProperty("Напись в правом нижнем углу")]
            public string RightDownText;
            [JsonProperty("Надпись на кнопке")]
            public string ButtonText;
        }

        [JsonProperty("Показывает при входе игрока на сервер")]
        public bool ShowOnJoin;
        [JsonProperty("Настройки дизайна")]
        public Interface InterfaceSettings;
        [JsonProperty("Список возможных страниц")]
        public List<Page> Pages;
        public static Configs GetNewConf();
    }

    private class Page
    {
        [JsonProperty("Короткое название в главном меню")]
        public string DisplayName;
        [JsonProperty("Текст в вложенном окне")]
        public string Text;
        [JsonProperty("Обязательно для чтения")]
        public bool Important;
        [JsonProperty("Время обязательного чтения (нельзя свернуть пункт)")]
        public int ReadTime;
        public void Draw(BasePlayer player);
        public IEnumerator DrawCounter(BasePlayer player);
    }

    private class PlayerInfo
    {
        [JsonProperty("Прочитанные статьи")]
        public HashSet<string> ReadArticles;
    }

    private static Hash<ulong, PlayerInfo> PlayerInfos;
    private Configs Configuration;
    [ConsoleCommand("UI_WelcomeHelpHandler")]
    private void CmdChatInfoMenu(ConsoleSystem.Arg args);
    [ChatCommand("info")]
    private void CmdChatInfo(BasePlayer player, string command, string[] args);
    private void OnServerInitialized();
    private void SaveData();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnPlayerInit(BasePlayer player);
    private void Unload();
    private const string Layer;
    private void UI_DrawInterface(BasePlayer player, bool onJoin);
}

private class Configs
{
    public class Interface
    {
        [JsonProperty("Текст заголовка (над выделенным блоком), советую оставить пустым")]
        public string HeaderText;
        [JsonProperty("Текст в разделителе")]
        public string DelimiterText;
        [JsonProperty("Надпись в левом нижнем углу")]
        public string LeftDownText;
        [JsonProperty("Напись в правом нижнем углу")]
        public string RightDownText;
        [JsonProperty("Надпись на кнопке")]
        public string ButtonText;
    }

    [JsonProperty("Показывает при входе игрока на сервер")]
    public bool ShowOnJoin;
    [JsonProperty("Настройки дизайна")]
    public Interface InterfaceSettings;
    [JsonProperty("Список возможных страниц")]
    public List<Page> Pages;
    public static Configs GetNewConf();
}

public class Interface
{
    [JsonProperty("Текст заголовка (над выделенным блоком), советую оставить пустым")]
    public string HeaderText;
    [JsonProperty("Текст в разделителе")]
    public string DelimiterText;
    [JsonProperty("Надпись в левом нижнем углу")]
    public string LeftDownText;
    [JsonProperty("Напись в правом нижнем углу")]
    public string RightDownText;
    [JsonProperty("Надпись на кнопке")]
    public string ButtonText;
}

private class Page
{
    [JsonProperty("Короткое название в главном меню")]
    public string DisplayName;
    [JsonProperty("Текст в вложенном окне")]
    public string Text;
    [JsonProperty("Обязательно для чтения")]
    public bool Important;
    [JsonProperty("Время обязательного чтения (нельзя свернуть пункт)")]
    public int ReadTime;
    public void Draw(BasePlayer player);
    public IEnumerator DrawCounter(BasePlayer player);
}

private class PlayerInfo
{
    [JsonProperty("Прочитанные статьи")]
    public HashSet<string> ReadArticles;
}


```

---

## WellFed

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("WellFed", "ColonBlow", "1.1.5", ResourceId = 1233)]
 class WellFed : RustPlugin
{
    public float LoginWellFedHealth { get; set; }
    public float SpawnWellFedHealth { get; set; }
    public float LoginWellFedHunger { get; set; }
    public float SpawnWellFedHunger { get; set; }
    public float LoginWellFedThirst { get; set; }
    public float SpawnWellFedThirst { get; set; }
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     bool CanBeFed(BasePlayer player, string perm);
}


```

---

## WheresMyCorpse

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Where's My Corpse", "LeoCurtss", 0.5)]
[Description("Points a player to their corpse when they type a command.")]
 class WheresMyCorpse : RustPlugin
{
     void Loaded();
    private string GetMessage(string name, string sid);
     Dictionary<string, string> deathInfo;
     void LoadData();
     void SaveData();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnPlayerRespawned(BasePlayer player);
    [ChatCommand("where")]
     void TestCommand(BasePlayer player, string command, string[] args);
     void drawArrow(BasePlayer player, float duration);
    public Vector3 getVector3(string rString);
    public Vector3 LerpByDistance(Vector3 A, Vector3 B, float x);
}


```

---

## WipeBlock

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("WipeBlock", "Hougan", "3.0.9")]
[Description("Блокировка предметов для вашего сервера! Куплено на DarkPlugins.RU")]
public class WipeBlock : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Duels;
    [JsonProperty("Заблокированные предметы")]
    private Dictionary<int, List<string>> blockedItems;
    [JsonProperty("Список градиентов")]
    private List<string> gradients;
    [JsonProperty("Слой с блокировкой (моментальной)")]
    private string Layer;
    [JsonProperty("%CHANGE%")]
    private string LayerBlock;
    [JsonProperty("%CHANGE%")]
    private string LayerInfoBlock;
    [JsonProperty("Красивые названия категорий")]
    private Dictionary<string, string> NiceCategories;
    private int CONF_MarginGUI;
    private string CONF_FirstString;
    private string CONF_SecondString;
    private string CONF_IgnorePermission;
    private string CONF_HeaderText;
    private int CONF_TimeMove;
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private bool? CanWearItem(PlayerInventory inventory, Item item);
    private bool? CanEquipItem(PlayerInventory inventory, Item item);
    private object OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
     object OnReloadMagazine(BasePlayer player, BaseProjectile projectile);
    private void OnPlayerInit(BasePlayer player);
    private void DrawBlockInfo(BasePlayer player);
    [ConsoleCommand("block")]
    private void cmdConsoleDrawBlock(ConsoleSystem.Arg args);
    [ConsoleCommand("blockmove")]
    private void cmdConsoleMoveblock(ConsoleSystem.Arg args);
    [ChatCommand("block")]
    private void cmdChatDrawBlock(BasePlayer player);
    private void DrawBlockGUI(BasePlayer player);
    private static string HexToRustFormat(string hex);
    private void DrawInstanceBlock(BasePlayer player, Item item);
    private string GetGradient(int t);
    private double IsBlockedCategory(int t);
    private bool IsAnyBlocked();
    private double IsBlocked(string shortname);
    private double UnBlockTime(int amount);
    private double IsBlocked(ItemDefinition itemDefinition);
    private void FillBlockedItems(Dictionary<string, Dictionary<Item, string>> fillDictionary);
    static readonly DateTime epoch;
    static double CurrentTime();
    public string Initializate();
    public static string ToShortString(TimeSpan timeSpan);
    private void GetConfig(string menu, string key, T varObject);
}


```

---

## WipeSchedule

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;

Oxide.Plugins
[Info("WipeSchedule", "k1lly0u", "2.0.2", ResourceId = 1451)]
 class WipeSchedule : RustPlugin
{
     DateTime NextWipeDate;
     DateTime NextWipeDateXP;
     Timer announceTimer;
     void Loaded();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void Unload();
    private DateTime ParseTime(string time);
    private void UpdateWipeDates();
    private void LoadWipeDates();
    private string NextWipeDays(DateTime WipeDate);
    private void BroadcastWipe();
    [ChatCommand("nextwipe")]
    private void cmdNextWipe(BasePlayer player, string command, string[] args);
    [ChatCommand("setwipe")]
    private void cmdSetWipe(BasePlayer player, string command, string[] args);
    [ConsoleCommand("setwipe")]
    private void ccmdSetWipe(ConsoleSystem.Arg arg);
    [ChatCommand("setnextwipe")]
    private void cmdSetNextWipe(BasePlayer player, string command, string[] args);
    [ConsoleCommand("setnextwipe")]
    private void ccmdSetNextWipe(ConsoleSystem.Arg arg);
    private ConfigData configData;
     class ConfigData
    {
        public string DateFormat { get; set; }
        public int DaysBetweenWipes { get; set; }
        public int DaysBetweenXPWipes { get; set; }
        public string LastWipe { get; set; }
        public string LastXPWipe { get; set; }
        public string NextWipe { get; set; }
        public string NextXPWipe { get; set; }
        public bool ShowXPWipeSchedule { get; set; }
        public bool AnnounceOnJoin { get; set; }
        public bool UseManualNextWipe { get; set; }
        public bool AnnounceOnTimer { get; set; }
        public int AnnounceTimer { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
    private string MSG(string key, string playerid);
     Dictionary<string, string> messages;
}

 class ConfigData
{
    public string DateFormat { get; set; }
    public int DaysBetweenWipes { get; set; }
    public int DaysBetweenXPWipes { get; set; }
    public string LastWipe { get; set; }
    public string LastXPWipe { get; set; }
    public string NextWipe { get; set; }
    public string NextXPWipe { get; set; }
    public bool ShowXPWipeSchedule { get; set; }
    public bool AnnounceOnJoin { get; set; }
    public bool UseManualNextWipe { get; set; }
    public bool AnnounceOnTimer { get; set; }
    public int AnnounceTimer { get; set; }
}


```

---

## WoundedLogo

```csharp
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("WoundedLogo", "Frizen", "2.0.0")]
internal class WoundedLogo : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private bool _isCH47;
    private bool _isHeli;
    private bool _isShip;
    private const string _layer;
    private static Configuration _config;
    public class Configuration
    {
        [JsonProperty("Картинки")]
        public Dictionary<string, string> Imgs;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityKill(BaseEntity entity);
     void OnEntityDeath(BaseEntity entity, HitInfo info);
     void Unload();
    private void DrawMenu(BasePlayer player);
    private void RefreshEvents(BasePlayer player, string type);
    private void OnlineUI(BasePlayer player);
    public Dictionary<ulong, bool> MenuOpened;
    [ConsoleCommand("wounded.menu")]
    private void MenuHandler(ConsoleSystem.Arg args);
    [PluginReference]
    private Plugin FreeOnline;
     float GetOnline();
    private static string HexToRustFormat(string hex);
    private void FirstCheckEvents();
    public string GetImage(string shortname, ulong skin);
}

public class Configuration
{
    [JsonProperty("Картинки")]
    public Dictionary<string, string> Imgs;
    public static Configuration DefaultConfig();
}


```

---

## XAntiCheat

```csharp
using System;
using System.Collections.Generic;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using Random = UnityEngine.Random;
using Rust;
using ProtoBuf;
using System.Collections;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("XAntiCheat", "123", "2.0.2")]
public class XAntiCheat : RustPlugin
{
    static XAntiCheat fermens;
    const bool fermensEN;
    const bool debugmode;
    const string ipinnfourl;
    const bool enablefull;
    const string ipinfosingup;
     Dictionary<string, string> messages;
    private string GetMessage(string key, string userId);
    private static bool IsBanned(ulong userid);
    private static bool IsImprisoned(ulong userid);
    private void OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
     object CanChangeCode(BasePlayer player, CodeLock codeLock, string newCode, bool isGuestCode);
    private static PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class SILENT
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Number of detections for a ban" : "Автобан (не знаешь, не трогай!)")]
        public int xdetects;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class SPIDER
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class FLY
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class SPINERBOT
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class NORECOIL
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty(fermensEN ? "Delay in seconds before the ban" : "Задержка в секундах перед баном, после детекта")]
        public float seconds;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class HITMOD
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class CODELOCK
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "Delay in seconds before the ban" : "Задержка в секундах перед баном, после детекта")]
        public float seconds;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class TEAMBAN
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
        public string reason;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "Ban if there are N bans in the team" : "Банить, если в команде N забаненных")]
        public int num;
        [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
        public int hours;
    }

     class ESPSTASH
    {
        [JsonProperty(fermensEN ? "Number of stashs" : "Количество")]
        public int amount;
        [JsonProperty("Discord Webhook")]
        public string webhook;
        [JsonProperty(fermensEN ? "Possible loot" : "Возможный лут")]
        public Dictionary<string, int> loots;
    }

     class DEBUGCAMERA
    {
        [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
        public bool enable;
        [JsonProperty("Discord Webhook")]
        public string webhook;
    }

    private class PluginConfig
    {
        [JsonProperty("-")]
        public string six;
        [JsonProperty("SteamAPI")]
        public string steampi;
        [JsonProperty(fermensEN ? "Silent Aim: setup" : "Silent Aim: настройка")]
        public SILENT sILENT;
        [JsonProperty(fermensEN ? "Spider: setup" : "Spider: настройка")]
        public SPIDER sPIDER;
        [JsonProperty(fermensEN ? "ESP SmallStash: setup" : "ESP SmallStash: настройка")]
        public ESPSTASH ESPStash;
        [JsonProperty(fermensEN ? "FLY: setup" : "FLY: настройка")]
        public FLY fLY;
        [JsonProperty(fermensEN ? "TEAMBAN: setup" : "TEAMBAN: настройка")]
        public TEAMBAN tEAMBAN;
        [JsonProperty(fermensEN ? "CODELOCK: setup" : "CODELOCK: настройка")]
        public CODELOCK cODELOCK;
        [JsonProperty(fermensEN ? "HITMOD: setup" : "HITMOD: настройка")]
        public HITMOD hITMOD;
        [JsonProperty(fermensEN ? "NORECOIL: setup" : "NORECOIL: настройка")]
        public NORECOIL nORECOIL;
        [JsonProperty(fermensEN ? "SPINERBOT: setup" : "SPINERBOT: настройка")]
        public SPINERBOT sPINERBOT;
        [JsonProperty("Debug camera")]
        public DEBUGCAMERA dEBUGCAMERA;
        [JsonProperty(fermensEN ? "Display steam account details when a player connect?" : "Отображать данные при подключении игрока?")]
        public bool show;
        [JsonProperty(fermensEN ? "Do not ban Steam players?" : "Не банить Steam игроков?")]
        public bool steamplayer;
        [JsonProperty(fermensEN ? "Send to jail if there is PrisonBitch plugin" : "Отправлять в тюрьму, если есть плагин PrisonBitch")]
        public bool prison;
        [JsonProperty(fermensEN ? "Ban not configured steam accounts?" : "Банить не настроеные аккаунты?")]
        public bool bannensatroyen;
        [JsonProperty(fermensEN ? "Ban accounts less than X days old" : "Банить аккаунты, которым меньше X дней")]
        public int banday;
        [JsonProperty(fermensEN ? "How long to ban new steam accounts (hours)" : "На сколько часов банить новые аккаунты")]
        public int bannewaccountday;
        [JsonProperty(fermensEN ? "Kick not configured steam accounts?" : "Кикать не настроенные аккаунты")]
        public bool kicknenastoyen;
        [JsonProperty(fermensEN ? "Kick private steam accounts?" : "Кикать приватные аккаунты")]
        public bool kickprivate;
        [JsonProperty(fermensEN ? "Kick players using VPN" : "Кикать игроков использующих VPN")]
        public bool kickvpn;
        [JsonProperty(fermensEN ? "Don't kick steam players for private, not configured or new account?" : "Не кикать лицухи?")]
        public bool steamkick;
        [JsonProperty(fermensEN ? "Don't ban steam players for new account?" : "Не банить лицушников за новые аккаунты?")]
        public bool steam;
        [JsonProperty(fermensEN ? "Write/save logs [0 - no | 1 - only bans | 2 - all]" : "Писать/сохранять логи [0 - нет | 1 - только баны | 2 - все]")]
        public int logspriority;
        [JsonProperty(fermensEN ? "Discord: Channel ID" : "Discord: ID канала")]
        public string discordid;
        [JsonProperty(fermensEN ? "Logs in language" : "Логи на языке")]
        public string lang { get; set; }
        [JsonProperty(fermensEN ? "Logs of hits from a firearm to the console" : " Логи попаданий с огнестрела в консоль")]
        public bool logs;
        [JsonProperty(fermensEN ? "IPINFO TOKEN" : "IPINFO ТОКЕН")]
        public string ipinfotoken;
        [JsonProperty("tt")]
        public string tt;
        [JsonProperty(fermensEN ? "Ban patterns" : "Шаблоны банов")]
        public Dictionary<string, string> pattern;
        public static PluginConfig DefaultConfig();
    }

    private static void SendDiscordMessage(string reason, string desc, string webhook);
    private class DiscordMessage
    {
        public DiscordMessage(string content, Embed[] embeds);
        [JsonProperty("content")]
        public string Content { get; set; }
        [JsonProperty("embeds")]
        public List<Embed> Embeds { get; set; }
        public string ToJson();
    }

    private class Embed
    {
        [JsonProperty("fields")]
        public List<Field> Fields { get; set; }
        public Embed AddField(string name, string value, bool inline);
    }

    private class Field
    {
        public Field(string name, string value, bool inline);
        [JsonProperty("name")]
        public string Name { get; set; }
        [JsonProperty("value")]
        public string Value { get; set; }
        [JsonProperty("inline")]
        public bool Inline { get; set; }
    }

     int nsnext;
    [ChatCommand("ns")]
    private void COMMANSTASH(BasePlayer player, string command, string[] args);
     Vector3 lastshash;
    [ChatCommand("ls")]
    private void COMMALS(BasePlayer player, string command, string[] args);
    [PluginReference]
    private Plugin PrisonBitch;
    private Plugin EnhancedBanSystem;
    private Plugin BanSystem;
    private static void BAN(string steamid, string reason, int time, string displayname, string webhook, bool checkteam);
    const int flylimit;
    const int spiderlimit;
    private Dictionary<BasePlayer, ANTICHEAT> anticheatPlayers;
     class ANTICHEAT : MonoBehaviour
    {
        private int stash;
        private int silent;
        private int spider;
        private int fly;
        private float flyheight;
         BasePlayer player;
        private DateTime lastban;
        private DateTime firsthit;
        private DateTime hitmod;
         string lasthit;
         int hits;
         float distancehit;
         string weaponhit;
        public DateTime LastFires;
        public int fires;
         string weaponfire;
         float posfire;
         float posfirel;
         float posfirer;
         int norecoil;
         Vector3 lastshot;
        private int numdetecthit;
         bool macromove;
         Vector3 startshoots;
         Vector3 direction;
        private void Awake();
        private void SILENTCLEAR();
        private List<Vector3> flyiingList;
        private int flyiing;
        public void ADDFLY(float distance);
         bool norecoilbanned;
        public void ADDFIRE(string weapon);
        private void FIREEND();
        public void ADDHIT(string hitbone, string weapon, float distance);
        private void CLEARHIT();
        public void ADDSTASH();
        public void ADDSPIDER();
        public void ADDSILENT(int amount);
        private int spiner;
        private Vector3 lastposition;
        private void TICK();
        public void DoDestroy();
        private void OnDestroy();
    }

    private void OnPlayerBanned(string name, ulong id, string address, string reason);
    const string defaultsteamapi;
     class resp
    {
        public avatar response;
    }

     class avatar
    {
        public List<Players> players;
    }

     class Players
    {
        public int? profilestate;
        public int? timecreated;
    }

     class INFO
    {
        public DateTime dateTime;
        public bool profilestate;
        public bool steam;
        public Dictionary<string, Dictionary<string, int>> hitinfo;
    }

     Dictionary<ulong, INFO> PLAYERINFO;
    private void Init();
     Dictionary<string, Vector3> Grids;
    const float calgon;
     void CreateSpawnGrid();
    private string GetNameGrid(Vector3 pos);
    private static bool prison;
    private List<StashContainer> stashContainers;
    private float sizeworldx;
    private float sizeworldz;
     List<Vector3> OntheMap;
     void foundmonuments();
    private Vector3 RANDOMPOS();
     List<string> names;
    private Vector3 FINDSPAWNPOINT(int num);
    private void CanSeeStash(BasePlayer player, StashContainer stash);
     void OnEntityKill(StashContainer stash);
     string patterban;
    private string token;
    private string namer;
    private void OnServerInitialized();
    public Dictionary<string, string> messagesEN;
    public Dictionary<string, string> messagesRU;
     IEnumerator GetCallback();
    private readonly int constructionColl;
    private readonly int buildingLayer;
    const string sspiral;
    const string sroof;
    const string sfly;
    const string prefroof;
    const string prefspiral;
    const string iceberg;
    private void OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
    private static string namefile;
    private static List<string> LOGS;
    private static List<BasePlayer> moders;
    private static void ADDLOG(string whatis, string desc, string webhook, int priority);
    private void DiscordLog(string discordWebHook, string message);
    private string GetMessage(string message, object[] args);
    private void Save();
    [ConsoleCommand("ac.logs")]
    private void cmdlastlogs(ConsoleSystem.Arg arg);
     Dictionary<ulong, List<BasePlayer.FiredProjectile>> projectiles;
    [ConsoleCommand("ac.accuracy")]
    private void cmdsaaccuracy(ConsoleSystem.Arg arg);
    [ConsoleCommand("ac.save")]
    private void cmdsavecommand(ConsoleSystem.Arg arg);
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
     class Eka
    {
        public Vector3 s;
        public Vector3 t;
    }

     Dictionary<ulong, shoots> _shoots;
     class shoots
    {
        public int number;
        public int success;
    }

    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile itemModProjectile, ProjectileShoot projectileShoot);
    private void OnEntityTakeDamage(object entity, HitInfo info);
    private bool IsNPC(BasePlayer player);
    [PluginReference]
     Plugin MultiFighting;
     Plugin Battles;
     Plugin HaxBot;
     Plugin uDiscord;
    private bool IsBattles(ulong userid);
    private bool ISSTEAM(Network.Connection connection);
     class tok
    {
        public string key;
        public uint appid;
        public string ticket;
    }

     IEnumerator GETINFO(BasePlayer player);
     class VPNINFO
    {
        public bool vpn;
        public bool proxy;
        public bool tor;
        public bool hosting;
    }

    private bool ISNASTROEN(int num);
}

 class SILENT
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Number of detections for a ban" : "Автобан (не знаешь, не трогай!)")]
    public int xdetects;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class SPIDER
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class FLY
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class SPINERBOT
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class NORECOIL
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty(fermensEN ? "Delay in seconds before the ban" : "Задержка в секундах перед баном, после детекта")]
    public float seconds;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class HITMOD
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class CODELOCK
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "Delay in seconds before the ban" : "Задержка в секундах перед баном, после детекта")]
    public float seconds;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class TEAMBAN
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty(fermensEN ? "Ban reason" : "Причина бана")]
    public string reason;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "Ban if there are N bans in the team" : "Банить, если в команде N забаненных")]
    public int num;
    [JsonProperty(fermensEN ? "How many hours to ban?" : "На сколько часов бан?")]
    public int hours;
}

 class ESPSTASH
{
    [JsonProperty(fermensEN ? "Number of stashs" : "Количество")]
    public int amount;
    [JsonProperty("Discord Webhook")]
    public string webhook;
    [JsonProperty(fermensEN ? "Possible loot" : "Возможный лут")]
    public Dictionary<string, int> loots;
}

 class DEBUGCAMERA
{
    [JsonProperty(fermensEN ? "Enable autoban?" : "Банить?")]
    public bool enable;
    [JsonProperty("Discord Webhook")]
    public string webhook;
}

private class PluginConfig
{
    [JsonProperty("-")]
    public string six;
    [JsonProperty("SteamAPI")]
    public string steampi;
    [JsonProperty(fermensEN ? "Silent Aim: setup" : "Silent Aim: настройка")]
    public SILENT sILENT;
    [JsonProperty(fermensEN ? "Spider: setup" : "Spider: настройка")]
    public SPIDER sPIDER;
    [JsonProperty(fermensEN ? "ESP SmallStash: setup" : "ESP SmallStash: настройка")]
    public ESPSTASH ESPStash;
    [JsonProperty(fermensEN ? "FLY: setup" : "FLY: настройка")]
    public FLY fLY;
    [JsonProperty(fermensEN ? "TEAMBAN: setup" : "TEAMBAN: настройка")]
    public TEAMBAN tEAMBAN;
    [JsonProperty(fermensEN ? "CODELOCK: setup" : "CODELOCK: настройка")]
    public CODELOCK cODELOCK;
    [JsonProperty(fermensEN ? "HITMOD: setup" : "HITMOD: настройка")]
    public HITMOD hITMOD;
    [JsonProperty(fermensEN ? "NORECOIL: setup" : "NORECOIL: настройка")]
    public NORECOIL nORECOIL;
    [JsonProperty(fermensEN ? "SPINERBOT: setup" : "SPINERBOT: настройка")]
    public SPINERBOT sPINERBOT;
    [JsonProperty("Debug camera")]
    public DEBUGCAMERA dEBUGCAMERA;
    [JsonProperty(fermensEN ? "Display steam account details when a player connect?" : "Отображать данные при подключении игрока?")]
    public bool show;
    [JsonProperty(fermensEN ? "Do not ban Steam players?" : "Не банить Steam игроков?")]
    public bool steamplayer;
    [JsonProperty(fermensEN ? "Send to jail if there is PrisonBitch plugin" : "Отправлять в тюрьму, если есть плагин PrisonBitch")]
    public bool prison;
    [JsonProperty(fermensEN ? "Ban not configured steam accounts?" : "Банить не настроеные аккаунты?")]
    public bool bannensatroyen;
    [JsonProperty(fermensEN ? "Ban accounts less than X days old" : "Банить аккаунты, которым меньше X дней")]
    public int banday;
    [JsonProperty(fermensEN ? "How long to ban new steam accounts (hours)" : "На сколько часов банить новые аккаунты")]
    public int bannewaccountday;
    [JsonProperty(fermensEN ? "Kick not configured steam accounts?" : "Кикать не настроенные аккаунты")]
    public bool kicknenastoyen;
    [JsonProperty(fermensEN ? "Kick private steam accounts?" : "Кикать приватные аккаунты")]
    public bool kickprivate;
    [JsonProperty(fermensEN ? "Kick players using VPN" : "Кикать игроков использующих VPN")]
    public bool kickvpn;
    [JsonProperty(fermensEN ? "Don't kick steam players for private, not configured or new account?" : "Не кикать лицухи?")]
    public bool steamkick;
    [JsonProperty(fermensEN ? "Don't ban steam players for new account?" : "Не банить лицушников за новые аккаунты?")]
    public bool steam;
    [JsonProperty(fermensEN ? "Write/save logs [0 - no | 1 - only bans | 2 - all]" : "Писать/сохранять логи [0 - нет | 1 - только баны | 2 - все]")]
    public int logspriority;
    [JsonProperty(fermensEN ? "Discord: Channel ID" : "Discord: ID канала")]
    public string discordid;
    [JsonProperty(fermensEN ? "Logs in language" : "Логи на языке")]
    public string lang { get; set; }
    [JsonProperty(fermensEN ? "Logs of hits from a firearm to the console" : " Логи попаданий с огнестрела в консоль")]
    public bool logs;
    [JsonProperty(fermensEN ? "IPINFO TOKEN" : "IPINFO ТОКЕН")]
    public string ipinfotoken;
    [JsonProperty("tt")]
    public string tt;
    [JsonProperty(fermensEN ? "Ban patterns" : "Шаблоны банов")]
    public Dictionary<string, string> pattern;
    public static PluginConfig DefaultConfig();
}

private class DiscordMessage
{
    public DiscordMessage(string content, Embed[] embeds);
    [JsonProperty("content")]
    public string Content { get; set; }
    [JsonProperty("embeds")]
    public List<Embed> Embeds { get; set; }
    public string ToJson();
}

private class Embed
{
    [JsonProperty("fields")]
    public List<Field> Fields { get; set; }
    public Embed AddField(string name, string value, bool inline);
}

private class Field
{
    public Field(string name, string value, bool inline);
    [JsonProperty("name")]
    public string Name { get; set; }
    [JsonProperty("value")]
    public string Value { get; set; }
    [JsonProperty("inline")]
    public bool Inline { get; set; }
}

 class ANTICHEAT : MonoBehaviour
{
    private int stash;
    private int silent;
    private int spider;
    private int fly;
    private float flyheight;
     BasePlayer player;
    private DateTime lastban;
    private DateTime firsthit;
    private DateTime hitmod;
     string lasthit;
     int hits;
     float distancehit;
     string weaponhit;
    public DateTime LastFires;
    public int fires;
     string weaponfire;
     float posfire;
     float posfirel;
     float posfirer;
     int norecoil;
     Vector3 lastshot;
    private int numdetecthit;
     bool macromove;
     Vector3 startshoots;
     Vector3 direction;
    private void Awake();
    private void SILENTCLEAR();
    private List<Vector3> flyiingList;
    private int flyiing;
    public void ADDFLY(float distance);
     bool norecoilbanned;
    public void ADDFIRE(string weapon);
    private void FIREEND();
    public void ADDHIT(string hitbone, string weapon, float distance);
    private void CLEARHIT();
    public void ADDSTASH();
    public void ADDSPIDER();
    public void ADDSILENT(int amount);
    private int spiner;
    private Vector3 lastposition;
    private void TICK();
    public void DoDestroy();
    private void OnDestroy();
}

 class resp
{
    public avatar response;
}

 class avatar
{
    public List<Players> players;
}

 class Players
{
    public int? profilestate;
    public int? timecreated;
}

 class INFO
{
    public DateTime dateTime;
    public bool profilestate;
    public bool steam;
    public Dictionary<string, Dictionary<string, int>> hitinfo;
}

 class Eka
{
    public Vector3 s;
    public Vector3 t;
}

 class shoots
{
    public int number;
    public int success;
}

 class tok
{
    public string key;
    public uint appid;
    public string ticket;
}

 class VPNINFO
{
    public bool vpn;
    public bool proxy;
    public bool tor;
    public bool hosting;
}


```

---

## XChatMessage

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("XChatMessage", "Sempai#3239", "1.0.0")]
 class XChatMessage : RustPlugin
{
    public int _message;
    private ChatMessageConfig config;
    private class ChatMessageConfig
    {
        internal class GeneralSetting
        {
            [JsonProperty("Интервал сообщений в чат")]
            public float TimeMessage;
            [JsonProperty("SteamID профиля для кастомной аватарки")]
            public ulong SteamID;
        }

        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Ключи сообщений для ленгов")]
        public List<string> Messages;
        public static ChatMessageConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void Broadcast();
    private void InitializeLang();
}

private class ChatMessageConfig
{
    internal class GeneralSetting
    {
        [JsonProperty("Интервал сообщений в чат")]
        public float TimeMessage;
        [JsonProperty("SteamID профиля для кастомной аватарки")]
        public ulong SteamID;
    }

    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Ключи сообщений для ленгов")]
    public List<string> Messages;
    public static ChatMessageConfig GetNewConfiguration();
}

internal class GeneralSetting
{
    [JsonProperty("Интервал сообщений в чат")]
    public float TimeMessage;
    [JsonProperty("SteamID профиля для кастомной аватарки")]
    public ulong SteamID;
}


```

---

## Xcraft

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Xcrafting", "Sparkless", "0.0.2")]
public class Xcraft : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private ConfigData Settings { get; set; }
    public class ShopItems
    {
        [JsonProperty("Shortname предмета")]
        public string ShortName;
        [JsonProperty("Кол-во")]
        public int Amount;
    }

     class ConfigData
    {
        [JsonProperty("Можно ли устанавливать переработчик на структурах?")]
        public bool Structure;
        [JsonProperty("Можно ли устанавливать переработчик на землю?")]
        public bool Ground;
        [JsonProperty("Включить/Выключить подбор переработчика")]
        public bool Available;
        [JsonProperty("Запрещать устанавливать переработчик в зоне действия чужого шкафа")]
        public bool Privelege;
        [JsonProperty("Ресурсы для крафта коптера(макс 6)")]
        public List<ShopItems> ShopItem { get; set; }
        [JsonProperty("Ресурсы для крафта переработчика(макс 6)")]
        public List<ShopItems> ShopItemrec { get; set; }
        public static ConfigData GetNewCong();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
     void OnServerInitialized();
    [ChatCommand("craft")]
     void cmdcraftopen(BasePlayer player);
    private string Layer;
     void opencraftmenu(BasePlayer player, int page, bool first);
    [ConsoleCommand("recyclercraft")]
     void cmdopenrecyclercraft(ConsoleSystem.Arg arg);
     void openmenucraftrec(BasePlayer player, int page, bool first);
    [ConsoleCommand("coptercraft")]
    private void cmdcoptercraft(ConsoleSystem.Arg arg);
    [ConsoleCommand("craftrecycler")]
    private void cmdrecyclercraft(ConsoleSystem.Arg arg);
     object CanStackItem(Item item, Item anotherItem);
     bool GiveRecycler(ItemContainer container);
     bool GiveCopter(BasePlayer player);
     bool GiveRecycler(BasePlayer player);
     bool Check(BaseEntity entity);
    private class RecyclerEntity : MonoBehaviour
    {
        private DestroyOnGroundMissing desGround;
        private GroundWatch groundWatch;
        public ulong OwnerID;
         void Awake();
    }

     void OnEntityBuilt(Planner plan, GameObject obj);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private static string HexToCuiColor(string hex);
}

public class ShopItems
{
    [JsonProperty("Shortname предмета")]
    public string ShortName;
    [JsonProperty("Кол-во")]
    public int Amount;
}

 class ConfigData
{
    [JsonProperty("Можно ли устанавливать переработчик на структурах?")]
    public bool Structure;
    [JsonProperty("Можно ли устанавливать переработчик на землю?")]
    public bool Ground;
    [JsonProperty("Включить/Выключить подбор переработчика")]
    public bool Available;
    [JsonProperty("Запрещать устанавливать переработчик в зоне действия чужого шкафа")]
    public bool Privelege;
    [JsonProperty("Ресурсы для крафта коптера(макс 6)")]
    public List<ShopItems> ShopItem { get; set; }
    [JsonProperty("Ресурсы для крафта переработчика(макс 6)")]
    public List<ShopItems> ShopItemrec { get; set; }
    public static ConfigData GetNewCong();
}

private class RecyclerEntity : MonoBehaviour
{
    private DestroyOnGroundMissing desGround;
    private GroundWatch groundWatch;
    public ulong OwnerID;
     void Awake();
}


```

---

## XDCasino

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using VLB;
using Rust;
using System.Text;

Oxide.Plugins
[Info("XDCasino", "SkuliDropek", "1.7.34")]
[Description("Casino, play for resources :3")]
public class XDCasino : RustPlugin
{
    private const string TerminalLayer;
    private const string NotiferLayer;
    private readonly ulong Id;
    private const string FileName;
     List<BaseEntity> CasinoEnt;
     BigWheelGame bigWheelGame;
     BaseEntity WoodenTrigger;
     MonumentInfo monument;
    [PluginReference]
     Plugin CopyPaste;
    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Запретить вставать игроку со стула если он учавствует в ставке | Forbid a player to get up from a chair if he participates in a bet")]
        public bool mountUse;
        [JsonProperty("Нужен ли шкаф в постройке ? | Do you need a closet in the building?")]
        public bool cupboardSpawn;
        [JsonProperty("Включить ли радио в здании ? | Should I turn on the radio in the building ?")]
        public bool useRadio;
        [JsonProperty("Включите этот параметр если у вас гниет постройка | Enable this option if your building is rotting")]
        public Boolean useDecay;
        [JsonProperty("Ссылка на радио станцию которая будет играть в доме | Link to the radio station that will play in the house")]
        public String RadioStation;
        [JsonProperty("идентификатор для подключения к камере | ID to connect to the camera")]
        public string identifier;
        [JsonProperty("Список предметов для ставок (ShortName/максимальное количество за 1 ставку) | List of items for bets (ShortName / maximum quantity for 1 bet)")]
        public Dictionary<string, int> casinoItems;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void Init();
    private void OnServerInitialized();
     void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo info);
     ItemContainer.CanAcceptResult? CanAcceptItem(ItemContainer container, Item item, int targetPos);
     object CanDismountEntity(BasePlayer player, BaseMountable entity);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
     void Unload();
    private void HelpUiNottice(BasePlayer player, string msg, string sprite);
     void GenerateBuilding();
     void PreSpawnEnt();
     void OnPasteFinished(List<BaseEntity> pastedEntities, string fileName);
    private void CheckEnt();
    private void LoadDataCopyPaste(Boolean repeat);
    public class PasteData
    {
        public Dictionary<string, object> @default;
        public ICollection<Dictionary<string, object>> entities;
        public Dictionary<string, object> protocol;
    }

    private static string HexToRustFormat(string hex);
     void Log(string msg, string file);
    private Vector3 GetResultVector();
    public static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
}

private class Configuration
{
    [JsonProperty("Запретить вставать игроку со стула если он учавствует в ставке | Forbid a player to get up from a chair if he participates in a bet")]
    public bool mountUse;
    [JsonProperty("Нужен ли шкаф в постройке ? | Do you need a closet in the building?")]
    public bool cupboardSpawn;
    [JsonProperty("Включить ли радио в здании ? | Should I turn on the radio in the building ?")]
    public bool useRadio;
    [JsonProperty("Включите этот параметр если у вас гниет постройка | Enable this option if your building is rotting")]
    public Boolean useDecay;
    [JsonProperty("Ссылка на радио станцию которая будет играть в доме | Link to the radio station that will play in the house")]
    public String RadioStation;
    [JsonProperty("идентификатор для подключения к камере | ID to connect to the camera")]
    public string identifier;
    [JsonProperty("Список предметов для ставок (ShortName/максимальное количество за 1 ставку) | List of items for bets (ShortName / maximum quantity for 1 bet)")]
    public Dictionary<string, int> casinoItems;
}

public class PasteData
{
    public Dictionary<string, object> @default;
    public ICollection<Dictionary<string, object>> entities;
    public Dictionary<string, object> protocol;
}


```

---

## XDCobaltLaboratory

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Facepunch;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using UnityEngine.SceneManagement;
using static NPCPlayerApex;
using Time = UnityEngine.Time;

Oxide.Plugins
[Info("XDCobaltLaboratory", "DezLife", "1.7.3")]
public class XDCobaltLaboratory : RustPlugin
{
    [PluginReference]
     Plugin CopyPaste;
     Plugin IQChat;
     Plugin RustMap;
    private static XDCobaltLaboratory _;
    private HashSet<Vector3> busyPoints3D;
    private List<BaseEntity> HouseCobaltLab;
    private List<NPCMonitor> nPCMonitors;
    private List<NpcZones> npcZones;
    private List<UInt64> HideUIUser;
    private string PosIvent;
    private int maxTry;
    private const int MaxRadius;
    public Timer SpawnHouseTime;
    public Timer RemoveHouseTime;
    public static DateTime TimeCreatedSave;
    public static DateTime RealTime;
    public static int SaveCreated;
    protected override void LoadDefaultMessages();
     int ApiGetTimeToStart();
    public void LoadDataCopyPaste();
    public class PasteData
    {
        public Dictionary<string, object> @default;
        public ICollection<Dictionary<string, object>> entities;
        public Dictionary<string, object> protocol;
    }

    public class LootNpcOrBox
    {
        [JsonProperty("ShortName")]
        public string Shortname;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Имя предмета")]
        public string DisplayName;
        [JsonProperty("Чертеж?")]
        public bool BluePrint;
        [JsonProperty("Минимальное количество")]
        public int MinimalAmount;
        [JsonProperty("максимальное количество")]
        public int MaximumAmount;
        [JsonProperty("Шанс выпадения предмета")]
        public int DropChance;
        [JsonProperty("Умножать этот предмета на день вайпа ?")]
        public bool wipeCheck;
    }

    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка постройки для ивента (CopyPaste)")]
        public BuildingPasteSettings pasteSettings;
        [JsonProperty("Настройка запуска и остановки ивента")]
        public IventController iventController;
        [JsonProperty("Настройка уведомлений")]
        public NotiferSettings notiferSettings;
        [JsonProperty("Настройка радиации в зоне ивента")]
        public RadiationConroller radiationConroller;
        [JsonProperty("Отображения ивента на картах")]
        public MapMarkers mapMarkers;
        [JsonProperty("Настройка NPC")]
        public NpcController npcController;
        [JsonProperty("Настройка ящика")]
        public BoxSetting boxSetting;
        internal class RadiationConroller
        {
            [JsonProperty("Включить радиацию ?")]
            public bool radUse;
            [JsonProperty("Количество радиационных частиц")]
            public int radCount;
        }

        internal class MapMarkers
        {
            [JsonProperty("Отметить ивент на карте RustMap?")]
            public bool rustMapUse;
            [JsonProperty("Иконка для карты RustMap")]
            public string rustMapIcon;
            [JsonProperty("Текст для карты RustMap")]
            public string rustMapTxt;
            [JsonProperty("Отметить ивент на карте G (Требуется https://umod.org/plugins/marker-manager)")]
            public bool MapUse;
            [JsonProperty("Текст для карты G")]
            public string MapTxt;
        }

        internal class IventController
        {
            [JsonProperty("Минимальное количество игроков для запуска ивента")]
            public int minPlayedPlayers;
            [JsonProperty("Время до начала ивента (Минимальное в секундах)")]
            public int minSpawnIvent;
            [JsonProperty("Время до начала ивента (Максимальное в секундах)")]
            public int maxSpawnIvent;
            [JsonProperty("Время до удаления ивента если никто не откроет ящик (Секунды)")]
            public int timeRemoveHouse;
            [JsonProperty("Время до удаления ивента после того как разблокируется ящик")]
            public int timeRemoveHouse2;
        }

        internal class NpcController
        {
            [JsonProperty("Спавнить NPC вокруг дома ?")]
            public bool useSpawnNPC;
            [JsonProperty("Колличевство NPC")]
            public int countSpawnNpc;
            [JsonProperty("ХП NPC")]
            public int healthNPC;
            [JsonProperty("Дистанция видимости")]
            public int DistanceRange;
            [JsonProperty("Точность оружия ученого (1 - 100)")]
            public int Accuracy;
            [JsonProperty("Спавнить ли подмогу после взлома ящика ? (НПС)")]
            public bool helpBot;
            [JsonProperty("Колличевство нпс (Подмога)")]
            public int helpCount;
            [JsonProperty("Рандомные ники нпс")]
            public List<string> nameNPC;
            [JsonProperty("Одежда для NPC")]
            public List<ItemNpc> wearNpc;
            [JsonProperty("Варианты оружия для NPC")]
            public List<ItemNpc> beltNpc;
            [JsonProperty("Использовать свой лут в нпс ?")]
            public bool useCustomLoot;
            [JsonProperty("Настройка лута в NPC (Если выключенно то будет стандартный) /cl.botitems")]
            public List<LootNpcOrBox> lootNpcs;
            internal class ItemNpc
            {
                [JsonProperty("ShortName")]
                public string Shortname;
                [JsonProperty("SkinID")]
                public ulong SkinID;
            }

        }

        internal class BuildingPasteSettings
        {
            [JsonProperty("Настройка высоты постройки (Требуется в настройке, если вы хотите ставить свою постройку)")]
            public int heightBuilding;
            [JsonProperty("файл в папке /oxide/data/copypaste с вашей постройкой(Если не указать загрузится стандартная)")]
            public string housepath;
            [JsonProperty("радиус для обнаружения построек игроков")]
            public int radiusClear;
        }

        internal class BoxSetting
        {
            [JsonProperty("Настройка лута в ящике /cl.items")]
            public List<LootNpcOrBox> lootBoxes;
            [JsonProperty("Время разблокировки ящика (Сек)")]
            public int unBlockTime;
            [JsonProperty("Макcимальное количество предметов в ящике")]
            public int maxItemCount;
            [JsonProperty("умножать количество лута на количество дней с начала вайпа (на 3й день - лута будет в 3 раза больше)")]
            public bool lootWipePlus;
            [JsonProperty("Включить сигнализацию *?")]
            public bool signaling;
        }

        internal class NotiferSettings
        {
            [JsonProperty("ВебХук дискорда (Если не нужны уведомления в дискорд, оставьте поле пустым)")]
            public string weebHook;
            [JsonProperty("Включить UI Уведомления ?")]
            public bool useUiNotifi;
            [JsonProperty("Цвет заднего фона окна UI")]
            public string colorBackground;
            [JsonProperty("Цвет Кнопки закрытия UI")]
            public string colorBtnCloseUi;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private List<Vector3>[] patternPositionsAboveWater;
    private List<Vector3>[] patternPositionsUnderWater;
    private readonly Quaternion[] directions;
    private void FillPatterns();
    [ConsoleCommand("isflat")]
    private void CmdIsFlat(ConsoleSystem.Arg arg);
    public bool IsFlat(Vector3 position);
    public bool IsDistancePoint(Vector3 point);
    private void GenerateSpawnPoints();
    private bool Is3DPointValid(Vector3 point);
    private void AcceptValue(Vector3 point);
    private object GetSpawnPoints();
     void OnEntityMounted(BaseMountable entity, BasePlayer player);
     void Unload();
     void Init();
    private void OnServerInitialized();
    private void CanHackCrate(BasePlayer player, HackableLockedCrate crate);
     void OnCrateHackEnd(HackableLockedCrate crate);
     void CanLootEntity(BasePlayer player, StorageContainer container);
     void OnCorpsePopulate(Scientist npc, NPCPlayerCorpse corpse);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void GenerateBuilding();
     void OnPasteFinished(List<BaseEntity> pastedEntities, string fileName);
    private void GenerateMapMarker(Vector3 pos);
    private void RemoveMapMarker();
    private void GenerateIvent();
    private BaseEntity CrateHackableLocked(BaseEntity box);
    private void StartIvent();
    private void StopIvent();
    private void UnscribeHook();
    private void SubscribeHook();
     Vector3 RandomCircle(Vector3 center, float radius);
    static float GetGroundPosition(Vector3 pos);
    private NPCPlayerApex InstantiateEntity(string type, Vector3 position);
     void SpawnBots(BaseEntity box, bool help);
    public HeldEntity GetFirstWeapon(BasePlayer player);
    private void Equip(BasePlayer player);
    private void ControllerInventory(BasePlayer player);
    public class NPCMonitor : FacepunchBehaviour
    {
        public NPCPlayerApex player { get; set; }
        private List<Vector3> patrolPositions;
        private Vector3 homePosition;
        private int lastPatrolIndex;
        private void Awake();
        private void checkNight();
        public void Initialize(BaseEntity box);
        private void UpdateDestination();
        private void Forget();
        private void OnDestroy();
        private void GeneratePatrolPositions();
    }

    public class NpcZones : MonoBehaviour
    {
        private Vector3 Position;
        private float Radius;
        private void Awake();
        public void Activate(BaseEntity box, float radius, bool rad);
        private void OnDestroy();
        public void Kill();
        private void UpdateCollider();
    }

     string GetGridString(Vector3 pos);
    private string NumberToString(int number);
    public class FancyMessage
    {
        public string content { get; set; }
        public bool tts { get; set; }
        public Embeds[] embeds { get; set; }
        public class Embeds
        {
            public string title { get; set; }
            public int color { get; set; }
            public List<Fields> fields { get; set; }
            public Embeds(string title, int color, List<Fields> fields);
        }

        public FancyMessage(string content, bool tts, Embeds[] embeds);
        public string toJSON();
    }

    public class Fields
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public Fields(string name, string value, bool inline);
    }

    private void Request(string url, string payload, Action<int> callback);
     void SendDiscordMsg(string msg);
    private static System.Random random;
    private int GenerateSpawnIventTime();
    public static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    public void SendChatAll(string Message, object[] args);
    public void SendChatPlayer(string Message, BasePlayer player, ConVar.Chat.ChatChannel channel);
    private static string HexToRustFormat(string hex);
    private void DestroyZone();
    [ChatCommand("cl")]
     void CLCommand(BasePlayer player, string cmd, string[] Args);
    [ConsoleCommand("HideUi")]
     void CMDHideUi(ConsoleSystem.Arg arg);
    [ChatCommand("cl.items")]
     void BoxItemCommand(BasePlayer player, string cmd, string[] Args);
    [ChatCommand("cl.botitems")]
     void NpcLootCommand(BasePlayer player, string cmd, string[] Args);
    public static class Cui
    {
        public static void CreateUIAllPlayer();
        public static void MainUI(BasePlayer player);
        public static void ButtonClose(BasePlayer player);
        public static void DestroyAllPlayer();
    }

}

public class PasteData
{
    public Dictionary<string, object> @default;
    public ICollection<Dictionary<string, object>> entities;
    public Dictionary<string, object> protocol;
}

public class LootNpcOrBox
{
    [JsonProperty("ShortName")]
    public string Shortname;
    [JsonProperty("SkinID")]
    public ulong SkinID;
    [JsonProperty("Имя предмета")]
    public string DisplayName;
    [JsonProperty("Чертеж?")]
    public bool BluePrint;
    [JsonProperty("Минимальное количество")]
    public int MinimalAmount;
    [JsonProperty("максимальное количество")]
    public int MaximumAmount;
    [JsonProperty("Шанс выпадения предмета")]
    public int DropChance;
    [JsonProperty("Умножать этот предмета на день вайпа ?")]
    public bool wipeCheck;
}

private class Configuration
{
    [JsonProperty("Настройка постройки для ивента (CopyPaste)")]
    public BuildingPasteSettings pasteSettings;
    [JsonProperty("Настройка запуска и остановки ивента")]
    public IventController iventController;
    [JsonProperty("Настройка уведомлений")]
    public NotiferSettings notiferSettings;
    [JsonProperty("Настройка радиации в зоне ивента")]
    public RadiationConroller radiationConroller;
    [JsonProperty("Отображения ивента на картах")]
    public MapMarkers mapMarkers;
    [JsonProperty("Настройка NPC")]
    public NpcController npcController;
    [JsonProperty("Настройка ящика")]
    public BoxSetting boxSetting;
    internal class RadiationConroller
    {
        [JsonProperty("Включить радиацию ?")]
        public bool radUse;
        [JsonProperty("Количество радиационных частиц")]
        public int radCount;
    }

    internal class MapMarkers
    {
        [JsonProperty("Отметить ивент на карте RustMap?")]
        public bool rustMapUse;
        [JsonProperty("Иконка для карты RustMap")]
        public string rustMapIcon;
        [JsonProperty("Текст для карты RustMap")]
        public string rustMapTxt;
        [JsonProperty("Отметить ивент на карте G (Требуется https://umod.org/plugins/marker-manager)")]
        public bool MapUse;
        [JsonProperty("Текст для карты G")]
        public string MapTxt;
    }

    internal class IventController
    {
        [JsonProperty("Минимальное количество игроков для запуска ивента")]
        public int minPlayedPlayers;
        [JsonProperty("Время до начала ивента (Минимальное в секундах)")]
        public int minSpawnIvent;
        [JsonProperty("Время до начала ивента (Максимальное в секундах)")]
        public int maxSpawnIvent;
        [JsonProperty("Время до удаления ивента если никто не откроет ящик (Секунды)")]
        public int timeRemoveHouse;
        [JsonProperty("Время до удаления ивента после того как разблокируется ящик")]
        public int timeRemoveHouse2;
    }

    internal class NpcController
    {
        [JsonProperty("Спавнить NPC вокруг дома ?")]
        public bool useSpawnNPC;
        [JsonProperty("Колличевство NPC")]
        public int countSpawnNpc;
        [JsonProperty("ХП NPC")]
        public int healthNPC;
        [JsonProperty("Дистанция видимости")]
        public int DistanceRange;
        [JsonProperty("Точность оружия ученого (1 - 100)")]
        public int Accuracy;
        [JsonProperty("Спавнить ли подмогу после взлома ящика ? (НПС)")]
        public bool helpBot;
        [JsonProperty("Колличевство нпс (Подмога)")]
        public int helpCount;
        [JsonProperty("Рандомные ники нпс")]
        public List<string> nameNPC;
        [JsonProperty("Одежда для NPC")]
        public List<ItemNpc> wearNpc;
        [JsonProperty("Варианты оружия для NPC")]
        public List<ItemNpc> beltNpc;
        [JsonProperty("Использовать свой лут в нпс ?")]
        public bool useCustomLoot;
        [JsonProperty("Настройка лута в NPC (Если выключенно то будет стандартный) /cl.botitems")]
        public List<LootNpcOrBox> lootNpcs;
        internal class ItemNpc
        {
            [JsonProperty("ShortName")]
            public string Shortname;
            [JsonProperty("SkinID")]
            public ulong SkinID;
        }

    }

    internal class BuildingPasteSettings
    {
        [JsonProperty("Настройка высоты постройки (Требуется в настройке, если вы хотите ставить свою постройку)")]
        public int heightBuilding;
        [JsonProperty("файл в папке /oxide/data/copypaste с вашей постройкой(Если не указать загрузится стандартная)")]
        public string housepath;
        [JsonProperty("радиус для обнаружения построек игроков")]
        public int radiusClear;
    }

    internal class BoxSetting
    {
        [JsonProperty("Настройка лута в ящике /cl.items")]
        public List<LootNpcOrBox> lootBoxes;
        [JsonProperty("Время разблокировки ящика (Сек)")]
        public int unBlockTime;
        [JsonProperty("Макcимальное количество предметов в ящике")]
        public int maxItemCount;
        [JsonProperty("умножать количество лута на количество дней с начала вайпа (на 3й день - лута будет в 3 раза больше)")]
        public bool lootWipePlus;
        [JsonProperty("Включить сигнализацию *?")]
        public bool signaling;
    }

    internal class NotiferSettings
    {
        [JsonProperty("ВебХук дискорда (Если не нужны уведомления в дискорд, оставьте поле пустым)")]
        public string weebHook;
        [JsonProperty("Включить UI Уведомления ?")]
        public bool useUiNotifi;
        [JsonProperty("Цвет заднего фона окна UI")]
        public string colorBackground;
        [JsonProperty("Цвет Кнопки закрытия UI")]
        public string colorBtnCloseUi;
    }

    public static Configuration GetNewConfiguration();
}

internal class RadiationConroller
{
    [JsonProperty("Включить радиацию ?")]
    public bool radUse;
    [JsonProperty("Количество радиационных частиц")]
    public int radCount;
}

internal class MapMarkers
{
    [JsonProperty("Отметить ивент на карте RustMap?")]
    public bool rustMapUse;
    [JsonProperty("Иконка для карты RustMap")]
    public string rustMapIcon;
    [JsonProperty("Текст для карты RustMap")]
    public string rustMapTxt;
    [JsonProperty("Отметить ивент на карте G (Требуется https://umod.org/plugins/marker-manager)")]
    public bool MapUse;
    [JsonProperty("Текст для карты G")]
    public string MapTxt;
}

internal class IventController
{
    [JsonProperty("Минимальное количество игроков для запуска ивента")]
    public int minPlayedPlayers;
    [JsonProperty("Время до начала ивента (Минимальное в секундах)")]
    public int minSpawnIvent;
    [JsonProperty("Время до начала ивента (Максимальное в секундах)")]
    public int maxSpawnIvent;
    [JsonProperty("Время до удаления ивента если никто не откроет ящик (Секунды)")]
    public int timeRemoveHouse;
    [JsonProperty("Время до удаления ивента после того как разблокируется ящик")]
    public int timeRemoveHouse2;
}

internal class NpcController
{
    [JsonProperty("Спавнить NPC вокруг дома ?")]
    public bool useSpawnNPC;
    [JsonProperty("Колличевство NPC")]
    public int countSpawnNpc;
    [JsonProperty("ХП NPC")]
    public int healthNPC;
    [JsonProperty("Дистанция видимости")]
    public int DistanceRange;
    [JsonProperty("Точность оружия ученого (1 - 100)")]
    public int Accuracy;
    [JsonProperty("Спавнить ли подмогу после взлома ящика ? (НПС)")]
    public bool helpBot;
    [JsonProperty("Колличевство нпс (Подмога)")]
    public int helpCount;
    [JsonProperty("Рандомные ники нпс")]
    public List<string> nameNPC;
    [JsonProperty("Одежда для NPC")]
    public List<ItemNpc> wearNpc;
    [JsonProperty("Варианты оружия для NPC")]
    public List<ItemNpc> beltNpc;
    [JsonProperty("Использовать свой лут в нпс ?")]
    public bool useCustomLoot;
    [JsonProperty("Настройка лута в NPC (Если выключенно то будет стандартный) /cl.botitems")]
    public List<LootNpcOrBox> lootNpcs;
    internal class ItemNpc
    {
        [JsonProperty("ShortName")]
        public string Shortname;
        [JsonProperty("SkinID")]
        public ulong SkinID;
    }

}

internal class ItemNpc
{
    [JsonProperty("ShortName")]
    public string Shortname;
    [JsonProperty("SkinID")]
    public ulong SkinID;
}

internal class BuildingPasteSettings
{
    [JsonProperty("Настройка высоты постройки (Требуется в настройке, если вы хотите ставить свою постройку)")]
    public int heightBuilding;
    [JsonProperty("файл в папке /oxide/data/copypaste с вашей постройкой(Если не указать загрузится стандартная)")]
    public string housepath;
    [JsonProperty("радиус для обнаружения построек игроков")]
    public int radiusClear;
}

internal class BoxSetting
{
    [JsonProperty("Настройка лута в ящике /cl.items")]
    public List<LootNpcOrBox> lootBoxes;
    [JsonProperty("Время разблокировки ящика (Сек)")]
    public int unBlockTime;
    [JsonProperty("Макcимальное количество предметов в ящике")]
    public int maxItemCount;
    [JsonProperty("умножать количество лута на количество дней с начала вайпа (на 3й день - лута будет в 3 раза больше)")]
    public bool lootWipePlus;
    [JsonProperty("Включить сигнализацию *?")]
    public bool signaling;
}

internal class NotiferSettings
{
    [JsonProperty("ВебХук дискорда (Если не нужны уведомления в дискорд, оставьте поле пустым)")]
    public string weebHook;
    [JsonProperty("Включить UI Уведомления ?")]
    public bool useUiNotifi;
    [JsonProperty("Цвет заднего фона окна UI")]
    public string colorBackground;
    [JsonProperty("Цвет Кнопки закрытия UI")]
    public string colorBtnCloseUi;
}

public class NPCMonitor : FacepunchBehaviour
{
    public NPCPlayerApex player { get; set; }
    private List<Vector3> patrolPositions;
    private Vector3 homePosition;
    private int lastPatrolIndex;
    private void Awake();
    private void checkNight();
    public void Initialize(BaseEntity box);
    private void UpdateDestination();
    private void Forget();
    private void OnDestroy();
    private void GeneratePatrolPositions();
}

public class NpcZones : MonoBehaviour
{
    private Vector3 Position;
    private float Radius;
    private void Awake();
    public void Activate(BaseEntity box, float radius, bool rad);
    private void OnDestroy();
    public void Kill();
    private void UpdateCollider();
}

public class FancyMessage
{
    public string content { get; set; }
    public bool tts { get; set; }
    public Embeds[] embeds { get; set; }
    public class Embeds
    {
        public string title { get; set; }
        public int color { get; set; }
        public List<Fields> fields { get; set; }
        public Embeds(string title, int color, List<Fields> fields);
    }

    public FancyMessage(string content, bool tts, Embeds[] embeds);
    public string toJSON();
}

public class Embeds
{
    public string title { get; set; }
    public int color { get; set; }
    public List<Fields> fields { get; set; }
    public Embeds(string title, int color, List<Fields> fields);
}

public class Fields
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public Fields(string name, string value, bool inline);
}

public static class Cui
{
    public static void CreateUIAllPlayer();
    public static void MainUI(BasePlayer player);
    public static void ButtonClose(BasePlayer player);
    public static void DestroyAllPlayer();
}


```

---

## XDGoldenskull

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using ConVar;
using System.Collections.Generic;

Oxide.Plugins
[Info("XDGoldenskull", "Sempai#3239", "1.0.9")]
public class XDGoldenskull : RustPlugin
{
    [ChatCommand("g.give")]
    private void cmdChatEmerald(BasePlayer player, string command, string[] args);
    [PluginReference]
     Plugin IQChat;
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
     object CanBeRecycled(Item item, Recycler recycler);
    private class Configuration
    {
        public void CreateItem(BasePlayer player, Vector3 position, int amount);
        [JsonProperty("Стак предмета")]
        public int StackItem;
        [JsonProperty("Отображаемое имя")]
        public string DisplayName;
        [JsonProperty("Из каких бочек будет падать и процент выпадения")]
        public Dictionary<string, int> barellList;
        [JsonProperty("Призы за переработку")]
        public List<string> itemsrec;
        [JsonProperty("Призы за потрошения")]
        public List<string> itempot;
        public int GetItemId();
        [JsonProperty("Скин ID черепа")]
        public ulong ReplaceID;
        public static Configuration GetNewConfiguration();
        [JsonProperty("Из каких ящиков будет падать и процент выпадения")]
        public Dictionary<string, int> cratelList;
        public Item Copy(int amount);
    }

    private const string ReplaceShortName;
     void OnServerInitialized();
    protected override void LoadConfig();
    private Item CreateItem();
    private static System.Random random;
    private IEnumerable<KeyValuePair<string, int>> lootContainerList;
    private void OnLootSpawn(LootContainer container);
     object OnRecycleItem(Recycler recycler, Item item);
    [ConsoleCommand("goldenskul")]
     void FishCommand(ConsoleSystem.Arg arg);
     object OnItemAction(Item item, string action, BasePlayer player);
     object CanRecycle(Recycler recycler, Item item);
    private static void ItemRemovalThink(Item item, BasePlayer player, int itemsToTake);
    protected override void LoadDefaultConfig();
    private static Configuration config;
    protected override void SaveConfig();
}

private class Configuration
{
    public void CreateItem(BasePlayer player, Vector3 position, int amount);
    [JsonProperty("Стак предмета")]
    public int StackItem;
    [JsonProperty("Отображаемое имя")]
    public string DisplayName;
    [JsonProperty("Из каких бочек будет падать и процент выпадения")]
    public Dictionary<string, int> barellList;
    [JsonProperty("Призы за переработку")]
    public List<string> itemsrec;
    [JsonProperty("Призы за потрошения")]
    public List<string> itempot;
    public int GetItemId();
    [JsonProperty("Скин ID черепа")]
    public ulong ReplaceID;
    public static Configuration GetNewConfiguration();
    [JsonProperty("Из каких ящиков будет падать и процент выпадения")]
    public Dictionary<string, int> cratelList;
    public Item Copy(int amount);
}


```

---

## XDQuest

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using Network;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using System.Collections;
using System.Text;

Oxide.Plugins
[Info("XDQuest", "DezLife", "2.0.5")]
[Description("Расширенная квест система для вашего сервера!")]
public class XDQuest : RustPlugin
{
    private const string AuthorContact;
    private const string filename;
    private static XDQuest Instance;
     MonumentInfo monument;
     HashSet<Item> ItemForce;
    private BasePlayer npc;
    private List<BaseEntity> HouseNPC;
    private Dictionary<string, string> ImageUI;
    [PluginReference]
     Plugin CopyPaste;
     Plugin ImageLibrary;
     Plugin IQChat;
     Plugin Friends;
     Plugin Clans;
     Plugin Battles;
     Plugin Duel;
    public void SendChat(BasePlayer player, string Message, ConVar.Chat.ChatChannel channel);
    public bool IsFriends(ulong userID, ulong targetID);
    public bool IsClans(ulong userID, ulong targetID);
    public bool IsDuel(ulong userID);
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public void SendImage(BasePlayer player, string imageName, ulong imageId);
    protected override void LoadDefaultMessages();
    public static Configuration config;
    public class Configuration
    {
        public class itemsNpc
        {
            [JsonProperty("ShortName")]
            public string ShortName;
            [JsonProperty("SkinId")]
            public ulong SkinId;
        }

        public class Settings
        {
            [JsonProperty("Колличевство единовременно взятых квестов")]
            public int questCount;
            [JsonProperty("Голосовое оповещение при выполнении задания")]
            public bool SoundEffect;
            [JsonProperty("Эфект")]
            public string Effect;
            [JsonProperty("Названия файла с квестами")]
            public string questListDataName;
            [JsonProperty("Названия файла с Аудио для NPC(Не менять!!!)")]
            public string audioDataPath;
            [JsonProperty("Команда для открытия квест листа с прогрессом")]
            public string questListProgress;
            [JsonProperty("Идентификатор вашей постройки")]
            public string buildid;
        }

        public class SettingsNpc
        {
            [JsonProperty("Имя нпс")]
            public string Name;
            [JsonProperty("id npc (От его ид зависит его внешность)")]
            public ulong userId;
            [JsonProperty("Одежда нпс")]
            public List<itemsNpc> Wear;
        }

        public class SettingsIQChat
        {
            [JsonProperty("Префикс в чате")]
            public string prifix;
            [JsonProperty("SteamID - Для аватарки из профиля стим")]
            public string SteamID;
        }

        [JsonProperty("Настройки NPC")]
        public SettingsNpc settingsNpc;
        [JsonProperty("Настройки")]
        public Settings settings;
        [JsonProperty("Настройки IQChat (Если есть)")]
        public SettingsIQChat settingsIQChat;
        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class PlayerQuest
    {
        public Quest parentQuest;
        public ulong UserID;
        public bool Finished;
        public int Count;
        public void AddCount(int amount);
        public int LeftAmount();
    }

    private class Quest
    {
        internal class Prize
        {
            public string nameprize;
            public PrizeType type;
            public string ShortName;
            public int Amount;
            public string Name;
            public ulong SkinID;
            public string Command;
            public string Url;
        }

        public string DisplayName;
        public string Description;
        public string Missions;
        public QuestType QuestType;
        public string Target;
        public int Amount;
        public bool UseRepeat;
        public int Cooldown;
        public List<Prize> PrizeList;
    }

    private class Building
    {
        public string name;
        public float Deg2Rad;
        public Vector3 pos;
    }

    private Dictionary<string, Building> BuildingList;
     void GenerateBuilding();
    public void InitializeNPC(Vector3 pos);
    private void ClearEnt();
     void OnPasteFinished(List<BaseEntity> pastedEntities);
     object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
     void StudySkill(BasePlayer player, string name);
     void KillHead(BasePlayer player);
     void OpenCase(BasePlayer player, string name);
     void RadOreGive(BasePlayer player, Item item);
     void LootHack(BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnItemResearch(ResearchTable table, Item targetItem, BasePlayer player);
    private void OnEntityBuilt(Planner plan, GameObject go);
     object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private void OnLootEntity(BasePlayer player, LootContainer entity);
    private void OnContainerDropItems(ItemContainer container);
     object OnItemPickup(Item item, BasePlayer player);
    private void OnCardSwipe(CardReader cardReader, Keycard card, BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo info);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     object CanAffordUpgrade(BasePlayer player, BuildingBlock block, BuildingGrade.Enum grade);
     void Init();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer d);
     void Unload();
    public class GravityItem
    {
        public string Shortname;
        public Vector3 vector;
        public Quaternion quaternion;
    }

     List<GravityItem> gravityItems;
    private void GravityItemAdd();
    public void CrategravityItems();
     System.Random rnd;
    private class ZoneTrigger : MonoBehaviour
    {
        private float ZoneRadius;
        private Vector3 Position;
        private SphereCollider sphereCollider;
        private void Awake();
        public void Activate(Vector3 pos, float radius);
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        private void OnDestroy();
        private void UpdateCollider();
    }

    public class AudioData
    {
        public String Name;
        public Single Length;
        public List<VoicePacket> VoicePacketList;
        public class VoicePacket
        {
            public Single TimeOffset;
            public Byte[] Stream;
        }

    }

    public class BotSpeakerData
    {
        public Dictionary<string, AudioData> AudioClips;
        public string[] Hey;
        public string[] Bye;
    }

    public string path;
    private BotSpeakerData _Data;
    private DynamicConfigFile DataFile;
    private void LoadDataSound();
     void WriteToData(string calback);
    public HashSet<uint> BotAlerts;
     void API_NPC_SendToAll(string clipName);
     void RunEffect(BasePlayer player, string path);
    public static class TimeHelper
    {
        public static string FormatTime(TimeSpan time, int maxSubstr, string language);
        private static string Format(int units, string form1, string form2, string form3);
    }

    private static DateTime Epoch;
    public static double GetTimeStamp();
    private IEnumerator DownloadImages();
    public void LoadDataCopyPaste();
    public static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    private static string HexToRustFormat(string hex);
    public class PasteData
    {
        public Dictionary<string, object> @default;
        public ICollection<Dictionary<string, object>> entities;
        public Dictionary<string, object> protocol;
    }

    private Vector3 GetResultVector();
    private BasePlayer FindMyBot(ulong userid);
    private void DestroyAll();
     void Log(string msg, string file);
    private const string Layers;
    private const string QuestListPanel;
    private const string QuestListLAYER;
    private const string QuestNotticePlayer;
     HashSet<ulong> openQuestPlayers;
    [ConsoleCommand("Close_UI")]
     void CloseUiPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("Close_Layer")]
     void CloseLayerPlayer(ConsoleSystem.Arg arg);
     void UI_QuestList(BasePlayer player);
    private void HelpUiNottice(BasePlayer player, string msg, string sprite, string color);
    private void UI_DrawInterface(BasePlayer player, bool upd, int page);
    public void QuestLists(BasePlayer player, int page, bool upd, bool active);
    private void OpenQuestInfo(BasePlayer player, int quest);
    public Dictionary<ulong, Coroutine> PlayersTime;
    private IEnumerator StartUpdate(BasePlayer player, int quest);
    [ConsoleCommand("UI_Handler")]
    private void CmdConsoleHandler(ConsoleSystem.Arg args);
     class PlayerData
    {
        public List<string> PlayerQuestsFinish;
        public Dictionary<int, PlayerQuest> PlayerQuestsAll;
        public Dictionary<string, double> PlayerQuestsCooldown;
    }

     class StoredData
    {
        public Dictionary<ulong, PlayerData> players;
    }

     void SaveData();
    private void SaveDataQuestList(T data);
    private void LoadDataQuestList(T data);
     void LoadDataPlayer();
     StoredData storedData;
    private DynamicConfigFile StatData;
    private Dictionary<int, Quest> QuestList;
}

public class Configuration
{
    public class itemsNpc
    {
        [JsonProperty("ShortName")]
        public string ShortName;
        [JsonProperty("SkinId")]
        public ulong SkinId;
    }

    public class Settings
    {
        [JsonProperty("Колличевство единовременно взятых квестов")]
        public int questCount;
        [JsonProperty("Голосовое оповещение при выполнении задания")]
        public bool SoundEffect;
        [JsonProperty("Эфект")]
        public string Effect;
        [JsonProperty("Названия файла с квестами")]
        public string questListDataName;
        [JsonProperty("Названия файла с Аудио для NPC(Не менять!!!)")]
        public string audioDataPath;
        [JsonProperty("Команда для открытия квест листа с прогрессом")]
        public string questListProgress;
        [JsonProperty("Идентификатор вашей постройки")]
        public string buildid;
    }

    public class SettingsNpc
    {
        [JsonProperty("Имя нпс")]
        public string Name;
        [JsonProperty("id npc (От его ид зависит его внешность)")]
        public ulong userId;
        [JsonProperty("Одежда нпс")]
        public List<itemsNpc> Wear;
    }

    public class SettingsIQChat
    {
        [JsonProperty("Префикс в чате")]
        public string prifix;
        [JsonProperty("SteamID - Для аватарки из профиля стим")]
        public string SteamID;
    }

    [JsonProperty("Настройки NPC")]
    public SettingsNpc settingsNpc;
    [JsonProperty("Настройки")]
    public Settings settings;
    [JsonProperty("Настройки IQChat (Если есть)")]
    public SettingsIQChat settingsIQChat;
    public static Configuration GetNewConfiguration();
}

public class itemsNpc
{
    [JsonProperty("ShortName")]
    public string ShortName;
    [JsonProperty("SkinId")]
    public ulong SkinId;
}

public class Settings
{
    [JsonProperty("Колличевство единовременно взятых квестов")]
    public int questCount;
    [JsonProperty("Голосовое оповещение при выполнении задания")]
    public bool SoundEffect;
    [JsonProperty("Эфект")]
    public string Effect;
    [JsonProperty("Названия файла с квестами")]
    public string questListDataName;
    [JsonProperty("Названия файла с Аудио для NPC(Не менять!!!)")]
    public string audioDataPath;
    [JsonProperty("Команда для открытия квест листа с прогрессом")]
    public string questListProgress;
    [JsonProperty("Идентификатор вашей постройки")]
    public string buildid;
}

public class SettingsNpc
{
    [JsonProperty("Имя нпс")]
    public string Name;
    [JsonProperty("id npc (От его ид зависит его внешность)")]
    public ulong userId;
    [JsonProperty("Одежда нпс")]
    public List<itemsNpc> Wear;
}

public class SettingsIQChat
{
    [JsonProperty("Префикс в чате")]
    public string prifix;
    [JsonProperty("SteamID - Для аватарки из профиля стим")]
    public string SteamID;
}

private class PlayerQuest
{
    public Quest parentQuest;
    public ulong UserID;
    public bool Finished;
    public int Count;
    public void AddCount(int amount);
    public int LeftAmount();
}

private class Quest
{
    internal class Prize
    {
        public string nameprize;
        public PrizeType type;
        public string ShortName;
        public int Amount;
        public string Name;
        public ulong SkinID;
        public string Command;
        public string Url;
    }

    public string DisplayName;
    public string Description;
    public string Missions;
    public QuestType QuestType;
    public string Target;
    public int Amount;
    public bool UseRepeat;
    public int Cooldown;
    public List<Prize> PrizeList;
}

internal class Prize
{
    public string nameprize;
    public PrizeType type;
    public string ShortName;
    public int Amount;
    public string Name;
    public ulong SkinID;
    public string Command;
    public string Url;
}

private class Building
{
    public string name;
    public float Deg2Rad;
    public Vector3 pos;
}

public class GravityItem
{
    public string Shortname;
    public Vector3 vector;
    public Quaternion quaternion;
}

private class ZoneTrigger : MonoBehaviour
{
    private float ZoneRadius;
    private Vector3 Position;
    private SphereCollider sphereCollider;
    private void Awake();
    public void Activate(Vector3 pos, float radius);
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    private void OnDestroy();
    private void UpdateCollider();
}

public class AudioData
{
    public String Name;
    public Single Length;
    public List<VoicePacket> VoicePacketList;
    public class VoicePacket
    {
        public Single TimeOffset;
        public Byte[] Stream;
    }

}

public class VoicePacket
{
    public Single TimeOffset;
    public Byte[] Stream;
}

public class BotSpeakerData
{
    public Dictionary<string, AudioData> AudioClips;
    public string[] Hey;
    public string[] Bye;
}

public static class TimeHelper
{
    public static string FormatTime(TimeSpan time, int maxSubstr, string language);
    private static string Format(int units, string form1, string form2, string form3);
}

public class PasteData
{
    public Dictionary<string, object> @default;
    public ICollection<Dictionary<string, object>> entities;
    public Dictionary<string, object> protocol;
}

 class PlayerData
{
    public List<string> PlayerQuestsFinish;
    public Dictionary<int, PlayerQuest> PlayerQuestsAll;
    public Dictionary<string, double> PlayerQuestsCooldown;
}

 class StoredData
{
    public Dictionary<ulong, PlayerData> players;
}


```

---

## XDQuest (1)

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Facepunch.Utility;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using UnityEngine.Networking;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("XDQuest", "SkuliDropek", "6.0.8")]
[Description("Расширенная квест система для вашего сервера!")]
public class XDQuest : RustPlugin
{
    [PluginReference]
     Plugin CopyPaste;
     Plugin ImageLibrary;
     Plugin IQChat;
     Plugin Friends;
     Plugin Clans;
     Plugin Battles;
     Plugin Duel;
     Plugin Notify;
    private void SendChat(BasePlayer player, string Message, ConVar.Chat.ChatChannel channel);
    private bool IsFriends(ulong userID, ulong targetID);
    private bool IsClans(string userID, string targetID);
    private bool IsDuel(ulong userID);
    private string GetImage(string shortname, ulong skin);
    private void SendImage(BasePlayer player, string imageName, ulong imageId);
    private static XDQuest Instance;
    private static readonly String Key;
    private MonumentInfo monument;
    private BasePlayer npc;
    private ComputerStation chairNpc;
    private List<BaseEntity> HouseNPC;
    private static List<uint> Light;
    private List<Quest> QuestList;
    private SafeZone safeZone;
    private ZoneTrigger zoneTrigger;
    private Dictionary<ulong, PlayerData> playersInfo;
    private class PlayerData
    {
        public List<string> PlayerQuestsFinish;
        public List<PlayerQuest> PlayerQuestsAll;
        public Dictionary<string, double> PlayerQuestsCooldown;
    }

    protected override void LoadDefaultMessages();
    private Configuration config;
    private class Configuration
    {
        public class itemsNpc
        {
            [JsonProperty("ShortName")]
            public String ShortName;
            [JsonProperty("SkinId")]
            public ulong SkinId;
        }

        public class Settings
        {
            [JsonProperty("Максимальное колличевство единовременно взятых квестов")]
            public Int32 questCount;
            [JsonProperty("Голосовое оповещение при выполнении задания")]
            public Boolean SoundEffect;
            [JsonProperty("Включите этот параметр если у вас гниет постройка")]
            public Boolean useDecay;
            [JsonProperty("Отчищать прогресс игроков при вайпе ?")]
            public Boolean useWipe;
            [JsonProperty("Эфект")]
            public String Effect;
            [JsonProperty("Names of the file with quests")]
            public String questListDataName;
            [JsonProperty("Команда для открытия квест листа с прогрессом")]
            public String questListProgress;
            [JsonProperty("Радиус безопасной зоны (Как в городе нпс)")]
            public float saveZoneRadius;
            [JsonProperty("Включить ли радио у нпс в здании ?")]
            public Boolean useRadio;
            [JsonProperty("Ссылка на радио станцию которая будет играть в доме")]
            public String RadioStation;
            [JsonProperty("Использовать метку на внутриигровой карте ? (Требуется https://skyplugins.ru/resources/428/)")]
            public Boolean mapUse;
            [JsonProperty("Имя метки на карте")]
            public String nameMarkerMap;
            [JsonProperty("Цвет маркера (без #)")]
            public String colorMarker;
            [JsonProperty("Цвет обводки (без #)")]
            public String colorOutline;
        }

        public class CustomPosition
        {
            [JsonProperty("Использовать кастомную позицию постройки ?")]
            public Boolean useCustomPos;
            [JsonProperty("Позиция постройки")]
            public Vector3 pos;
            [JsonProperty("Поворот постройки (Этим параметром вы можете повернуть постройку)")]
            public Int32 rotation;
        }

        public class SettingsNpc
        {
            [JsonProperty("Имя нпс")]
            public String Name;
            [JsonProperty("id npc (От его ид зависит его внешность)")]
            public ulong userId;
            [JsonProperty("Одежда нпс")]
            public List<itemsNpc> Wear;
        }

        public class SettingsIQChat
        {
            [JsonProperty("Префикс в чате")]
            public String prifix;
            [JsonProperty("SteamID - Для аватарки из профиля стим")]
            public String SteamID;
        }

        public class SettingsNotify
        {
            [JsonProperty("Включить уведомления (Требуется - https://codefling.com/plugins/notify)")]
            public bool useNotify;
            [JsonProperty("Тип уведомления (Требуется - https://codefling.com/plugins/notify)")]
            public int typeNotify;
        }

        public class SettingsSoundNPC
        {
            [JsonProperty("Включить возможность разговаривать NPC")]
            public bool soundUse;
            [JsonProperty("Заполнять стандартными звуками ? (Нужно добавить звуки в дату!)")]
            public bool soundAddToCfg;
            [JsonProperty("Название файлов со звуком для приветствия")]
            public List<string> heySound;
            [JsonProperty("Название файлов со звуком для прощание")]
            public List<string> byeSound;
            [JsonProperty("Название файлов со звуком для взятие задания")]
            public List<string> takeQuestSound;
            [JsonProperty("Название файлов со звуком для сдачи задания")]
            public List<string> turnQuestSound;
        }

        [JsonProperty("Настройка кастомной позиции постройки")]
        public CustomPosition customPosition;
        [JsonProperty("Настройки NPC")]
        public SettingsNpc settingsNpc;
        [JsonProperty("Настройки")]
        public Settings settings;
        [JsonProperty("Настройки звуков/разговоров NPC")]
        public SettingsSoundNPC settingsSoundNPC;
        [JsonProperty("Настройки IQChat (Если есть)")]
        public SettingsIQChat settingsIQChat;
        [JsonProperty("Настройки уведомления")]
        public SettingsNotify settingsNotify;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class PlayerQuest
    {
        public Quest parentQuest;
        public ulong UserID;
        public bool Finished;
        public int Count;
        public void AddCount(int amount);
        public int LeftAmount();
    }

    private class Quest
    {
        internal class Prize
        {
            public string nameprize;
            public PrizeType type;
            public string ShortName;
            public int Amount;
            public string Name;
            public ulong SkinID;
            public string Command;
            public string Url;
        }

        public string DisplayName;
        public string Description;
        public string Missions;
        public QuestType QuestType;
        public string Target;
        public int Amount;
        public bool UseRepeat;
        public bool Bring;
        public int Cooldown;
        public List<Prize> PrizeList;
    }

    private class Building
    {
        public string name;
        public float Deg2Rad;
        public Vector3 pos;
    }

    private Dictionary<string, Building> BuildingList;
     void GenerateBuilding();
    private void CrateBox(Vector3 pos);
    private void InitializeNPC(Vector3 pos);
    private void ClearEnt();
     void OnPasteFinished(List<BaseEntity> pastedEntities, string fileName);
    private class ZoneTrigger : FacepunchBehaviour
    {
        private float ZoneRadius;
        private Vector3 Position;
        private SphereCollider sphereCollider;
        private void Awake();
        public void Activate(Vector3 pos, float radius);
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        private void OnDestroy();
        private void UpdateCollider();
    }

    private class SafeZone : MonoBehaviour
    {
        private Vector3 Position;
        private float Radius;
        private void Awake();
        public void Activate(Vector3 pos, float radius);
        private void OnDestroy();
        private void UpdateCollider();
    }

     object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
     void StudySkill(BasePlayer player, string name);
     void KillHead(BasePlayer player);
     void OpenCase(BasePlayer player, string name);
     void RadOreGive(BasePlayer player, Item item);
     void LootHack(BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnTechTreeNodeUnlock(Workbench workbench, TechTreeData.NodeInstance node, BasePlayer player);
     void OnItemResearch(ResearchTable table, Item targetItem, BasePlayer player);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private static Dictionary<BasePlayer, List<UInt64>> LootersListCarte;
    private void OnLootEntity(BasePlayer player, LootContainer entity);
    private void OnLootEntity(BasePlayer player, NPCPlayerCorpse entity);
    private void OnContainerDropItems(ItemContainer container);
     void OnItemPickup(Item item, BasePlayer player);
    private void OnCardSwipe(CardReader cardReader, Keycard card, BasePlayer player);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnNpcGiveSoldItem(NPCVendingMachine machine, Item soldItem, BasePlayer buyer);
     void CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private Dictionary<uint, BasePlayer> recyclePlayer;
     void OnRecyclerToggle(Recycler recycler, BasePlayer player);
     void OnRecycleItem(Recycler recycler, Item item);
     void OnGrowableGathered(GrowableEntity plant, Item item, BasePlayer player);
     void OnRaidableBaseCompleted(Vector3 location, int mode, bool allowPVP, string id, float spawnTime, float despawnTime, float loadingTime, ulong ownerId, BasePlayer owner, List<BasePlayer> raiders);
     void OnFishCatch(Item fish, BaseFishingRod fishingRod, BasePlayer player);
    private void OnNewSave();
     object CanLootEntity(BasePlayer player, StorageContainer container);
     object CanAffordUpgrade(BasePlayer player, BuildingBlock block, BuildingGrade.Enum grade);
     void Init();
    private void OnServerInitialized();
     void OnEntityTakeDamage(BaseCombatEntity victim, HitInfo info);
     void OnPlayerConnected(BasePlayer player);
     void OnServerSave();
    private void OnPlayerDisconnected(BasePlayer d);
     void OnServerShutdown();
     void Unload();
    private readonly Hash<string, NpcSound> Sounds;
    private readonly Hash<string, NpcSound> cached;
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

    private void LoadDataSound();
    private NpcSound LoadDataSound(string name);
    public List<uint> BotAlerts;
    private Coroutine SoundRoutine { get; set; }
    public void SoundPlay(string clip);
    private List<byte[]> FromSaveData(byte[] bytes);
    private IEnumerator API_NPC_SendToAll(string clipName);
    private void SendSound(uint netId, byte[] data);
    private void LoadDataCopyPaste(Boolean repeat);
    private IEnumerator DownloadImages();
    private static class ImageUi
    {
        public static Dictionary<string, string> Images;
        private static Dictionary<int, string> _images;
        public static void DownloadImages();
        private static IEnumerator AddImage(string url, string name);
        public static string GetImage(String ImgKey);
        public static void Unload();
    }

    private void QuestProgress(BasePlayer player, QuestType questType, String entName, String skinId, List<Item> items, int count);
     void RunEffect(BasePlayer player, string path);
    public static class TimeHelper
    {
        public static string FormatTime(TimeSpan time, int maxSubstr, string language);
        private static string Format(int units, string form1, string form2, string form3);
    }

    private static double CurrentTime();
    public static StringBuilder sb;
    public string GetLang(string LangKey, string userID, object[] args);
    private class PasteData
    {
        public Dictionary<string, object> @default;
        public ICollection<Dictionary<string, object>> entities;
        public Dictionary<string, object> protocol;
    }

    public Vector3 GetResultVector();
    private IEnumerable<BasePlayer> FindMyBot(ulong userid);
     void Log(string msg, string file);
     List<ulong> openQuestPlayers;
    private Dictionary<ulong, Coroutine> PlayersTime;
    private const string MiniQuestList;
    private const string Layers;
    private const string LayerMainBackground;
     void MainUi(BasePlayer player);
     void QuestListUI(BasePlayer player, int page);
    private void QuestInfo(BasePlayer player, int quest);
    private IEnumerator StartUpdate(BasePlayer player, int quest);
    private void OpenMQL_CMD(BasePlayer player);
     void UIMiniQuestList(BasePlayer player, int page);
    private void UINottice(BasePlayer player, string msg, string sprite, string color);
    [ConsoleCommand("CloseMiniQuestList")]
     void CloseMiniQuestList(ConsoleSystem.Arg arg);
    [ConsoleCommand("CloseMainUI")]
     void CloseLayerPlayer(ConsoleSystem.Arg arg);
    [ChatCommand("quest.saveposition")]
     void CustomPosSave(BasePlayer player);
    [ChatCommand("quest.tphouse")]
     void TpToQuestHouse(BasePlayer player);
    [ConsoleCommand("UI_Handler")]
    private void CmdConsoleHandler(ConsoleSystem.Arg args);
    private List<Quest> LoadQuestListBuffer();
     JsonSerializerSettings settings;
    private void LoadQuestData();
    private void LoadPlayerData();
    private void SavePlayerData();
     void SaveData();
}

private class PlayerData
{
    public List<string> PlayerQuestsFinish;
    public List<PlayerQuest> PlayerQuestsAll;
    public Dictionary<string, double> PlayerQuestsCooldown;
}

private class Configuration
{
    public class itemsNpc
    {
        [JsonProperty("ShortName")]
        public String ShortName;
        [JsonProperty("SkinId")]
        public ulong SkinId;
    }

    public class Settings
    {
        [JsonProperty("Максимальное колличевство единовременно взятых квестов")]
        public Int32 questCount;
        [JsonProperty("Голосовое оповещение при выполнении задания")]
        public Boolean SoundEffect;
        [JsonProperty("Включите этот параметр если у вас гниет постройка")]
        public Boolean useDecay;
        [JsonProperty("Отчищать прогресс игроков при вайпе ?")]
        public Boolean useWipe;
        [JsonProperty("Эфект")]
        public String Effect;
        [JsonProperty("Names of the file with quests")]
        public String questListDataName;
        [JsonProperty("Команда для открытия квест листа с прогрессом")]
        public String questListProgress;
        [JsonProperty("Радиус безопасной зоны (Как в городе нпс)")]
        public float saveZoneRadius;
        [JsonProperty("Включить ли радио у нпс в здании ?")]
        public Boolean useRadio;
        [JsonProperty("Ссылка на радио станцию которая будет играть в доме")]
        public String RadioStation;
        [JsonProperty("Использовать метку на внутриигровой карте ? (Требуется https://skyplugins.ru/resources/428/)")]
        public Boolean mapUse;
        [JsonProperty("Имя метки на карте")]
        public String nameMarkerMap;
        [JsonProperty("Цвет маркера (без #)")]
        public String colorMarker;
        [JsonProperty("Цвет обводки (без #)")]
        public String colorOutline;
    }

    public class CustomPosition
    {
        [JsonProperty("Использовать кастомную позицию постройки ?")]
        public Boolean useCustomPos;
        [JsonProperty("Позиция постройки")]
        public Vector3 pos;
        [JsonProperty("Поворот постройки (Этим параметром вы можете повернуть постройку)")]
        public Int32 rotation;
    }

    public class SettingsNpc
    {
        [JsonProperty("Имя нпс")]
        public String Name;
        [JsonProperty("id npc (От его ид зависит его внешность)")]
        public ulong userId;
        [JsonProperty("Одежда нпс")]
        public List<itemsNpc> Wear;
    }

    public class SettingsIQChat
    {
        [JsonProperty("Префикс в чате")]
        public String prifix;
        [JsonProperty("SteamID - Для аватарки из профиля стим")]
        public String SteamID;
    }

    public class SettingsNotify
    {
        [JsonProperty("Включить уведомления (Требуется - https://codefling.com/plugins/notify)")]
        public bool useNotify;
        [JsonProperty("Тип уведомления (Требуется - https://codefling.com/plugins/notify)")]
        public int typeNotify;
    }

    public class SettingsSoundNPC
    {
        [JsonProperty("Включить возможность разговаривать NPC")]
        public bool soundUse;
        [JsonProperty("Заполнять стандартными звуками ? (Нужно добавить звуки в дату!)")]
        public bool soundAddToCfg;
        [JsonProperty("Название файлов со звуком для приветствия")]
        public List<string> heySound;
        [JsonProperty("Название файлов со звуком для прощание")]
        public List<string> byeSound;
        [JsonProperty("Название файлов со звуком для взятие задания")]
        public List<string> takeQuestSound;
        [JsonProperty("Название файлов со звуком для сдачи задания")]
        public List<string> turnQuestSound;
    }

    [JsonProperty("Настройка кастомной позиции постройки")]
    public CustomPosition customPosition;
    [JsonProperty("Настройки NPC")]
    public SettingsNpc settingsNpc;
    [JsonProperty("Настройки")]
    public Settings settings;
    [JsonProperty("Настройки звуков/разговоров NPC")]
    public SettingsSoundNPC settingsSoundNPC;
    [JsonProperty("Настройки IQChat (Если есть)")]
    public SettingsIQChat settingsIQChat;
    [JsonProperty("Настройки уведомления")]
    public SettingsNotify settingsNotify;
}

public class itemsNpc
{
    [JsonProperty("ShortName")]
    public String ShortName;
    [JsonProperty("SkinId")]
    public ulong SkinId;
}

public class Settings
{
    [JsonProperty("Максимальное колличевство единовременно взятых квестов")]
    public Int32 questCount;
    [JsonProperty("Голосовое оповещение при выполнении задания")]
    public Boolean SoundEffect;
    [JsonProperty("Включите этот параметр если у вас гниет постройка")]
    public Boolean useDecay;
    [JsonProperty("Отчищать прогресс игроков при вайпе ?")]
    public Boolean useWipe;
    [JsonProperty("Эфект")]
    public String Effect;
    [JsonProperty("Names of the file with quests")]
    public String questListDataName;
    [JsonProperty("Команда для открытия квест листа с прогрессом")]
    public String questListProgress;
    [JsonProperty("Радиус безопасной зоны (Как в городе нпс)")]
    public float saveZoneRadius;
    [JsonProperty("Включить ли радио у нпс в здании ?")]
    public Boolean useRadio;
    [JsonProperty("Ссылка на радио станцию которая будет играть в доме")]
    public String RadioStation;
    [JsonProperty("Использовать метку на внутриигровой карте ? (Требуется https://skyplugins.ru/resources/428/)")]
    public Boolean mapUse;
    [JsonProperty("Имя метки на карте")]
    public String nameMarkerMap;
    [JsonProperty("Цвет маркера (без #)")]
    public String colorMarker;
    [JsonProperty("Цвет обводки (без #)")]
    public String colorOutline;
}

public class CustomPosition
{
    [JsonProperty("Использовать кастомную позицию постройки ?")]
    public Boolean useCustomPos;
    [JsonProperty("Позиция постройки")]
    public Vector3 pos;
    [JsonProperty("Поворот постройки (Этим параметром вы можете повернуть постройку)")]
    public Int32 rotation;
}

public class SettingsNpc
{
    [JsonProperty("Имя нпс")]
    public String Name;
    [JsonProperty("id npc (От его ид зависит его внешность)")]
    public ulong userId;
    [JsonProperty("Одежда нпс")]
    public List<itemsNpc> Wear;
}

public class SettingsIQChat
{
    [JsonProperty("Префикс в чате")]
    public String prifix;
    [JsonProperty("SteamID - Для аватарки из профиля стим")]
    public String SteamID;
}

public class SettingsNotify
{
    [JsonProperty("Включить уведомления (Требуется - https://codefling.com/plugins/notify)")]
    public bool useNotify;
    [JsonProperty("Тип уведомления (Требуется - https://codefling.com/plugins/notify)")]
    public int typeNotify;
}

public class SettingsSoundNPC
{
    [JsonProperty("Включить возможность разговаривать NPC")]
    public bool soundUse;
    [JsonProperty("Заполнять стандартными звуками ? (Нужно добавить звуки в дату!)")]
    public bool soundAddToCfg;
    [JsonProperty("Название файлов со звуком для приветствия")]
    public List<string> heySound;
    [JsonProperty("Название файлов со звуком для прощание")]
    public List<string> byeSound;
    [JsonProperty("Название файлов со звуком для взятие задания")]
    public List<string> takeQuestSound;
    [JsonProperty("Название файлов со звуком для сдачи задания")]
    public List<string> turnQuestSound;
}

private class PlayerQuest
{
    public Quest parentQuest;
    public ulong UserID;
    public bool Finished;
    public int Count;
    public void AddCount(int amount);
    public int LeftAmount();
}

private class Quest
{
    internal class Prize
    {
        public string nameprize;
        public PrizeType type;
        public string ShortName;
        public int Amount;
        public string Name;
        public ulong SkinID;
        public string Command;
        public string Url;
    }

    public string DisplayName;
    public string Description;
    public string Missions;
    public QuestType QuestType;
    public string Target;
    public int Amount;
    public bool UseRepeat;
    public bool Bring;
    public int Cooldown;
    public List<Prize> PrizeList;
}

internal class Prize
{
    public string nameprize;
    public PrizeType type;
    public string ShortName;
    public int Amount;
    public string Name;
    public ulong SkinID;
    public string Command;
    public string Url;
}

private class Building
{
    public string name;
    public float Deg2Rad;
    public Vector3 pos;
}

private class ZoneTrigger : FacepunchBehaviour
{
    private float ZoneRadius;
    private Vector3 Position;
    private SphereCollider sphereCollider;
    private void Awake();
    public void Activate(Vector3 pos, float radius);
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    private void OnDestroy();
    private void UpdateCollider();
}

private class SafeZone : MonoBehaviour
{
    private Vector3 Position;
    private float Radius;
    private void Awake();
    public void Activate(Vector3 pos, float radius);
    private void OnDestroy();
    private void UpdateCollider();
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

private static class ImageUi
{
    public static Dictionary<string, string> Images;
    private static Dictionary<int, string> _images;
    public static void DownloadImages();
    private static IEnumerator AddImage(string url, string name);
    public static string GetImage(String ImgKey);
    public static void Unload();
}

public static class TimeHelper
{
    public static string FormatTime(TimeSpan time, int maxSubstr, string language);
    private static string Format(int units, string form1, string form2, string form3);
}

private class PasteData
{
    public Dictionary<string, object> @default;
    public ICollection<Dictionary<string, object>> entities;
    public Dictionary<string, object> protocol;
}


```

---

## XDStatistics

```csharp
using UnityEngine;
using System;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using System.Linq;
using Newtonsoft.Json.Linq;
using Rust;
using System.Text;
using System.Globalization;

Oxide.Plugins
[Info("XDStatistics", "DezLife", "1.9.33")]
[Description("Multifunctional statistics for your server!")]
 class XDStatistics : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin Friends;
     Plugin Clans;
     Plugin Battles;
     Plugin Duel;
     Plugin Economics;
     Plugin IQEconomic;
     Plugin ServerRewards;
     Plugin GameStoresRUST;
     Plugin RustStore;
    private bool IsFriends(ulong userID, ulong targetID);
    private bool IsClans(string userID, string targetID);
    private bool IsDuel(ulong userID);
    private string GetImage(string shortname, ulong skin);
    private bool AddImage(string url, string shortname, ulong skin);
    private void SendImage(BasePlayer player, string imageName, ulong imageId);
    private bool HasImage(string imageName, ulong imageId);
    private static XDStatistics _;
    private readonly string permAdmin;
    private readonly string permReset;
    private readonly string permAvailability;
    private Dictionary<string, ItemDisplayName> ItemName;
    private List<items> ItemList;
    public static StringBuilder sb;
    private Dictionary<uint, string> prefabID2Item;
    private Dictionary<string, string> prefabNameItem;
    private List<int> AlowedSeedId;
    protected override void LoadDefaultMessages();
    private Configuration config;
    private class Configuration
    {
        public class SettingsInterface
        {
            [JsonProperty("Цвет плашки заднего фона в топ 10 за 1 место | Background color in the top 10 for 1st place")]
            public string ColorTop1;
            [JsonProperty("Цвет плашки заднего фона в топ 10 за 2 место | Background color in the top 10 for 2st place")]
            public string ColorTop2;
            [JsonProperty("Цвет плашки заднего фона в топ 10 за 3 место | Background color in the top 10 for 3st place")]
            public string ColorTop3;
            [JsonProperty("Использовать свой задний фон ? (указанный снизу)| Use your own background ? (indicated at the bottom)")]
            public bool UsebackgroundImageUrl;
            [JsonProperty("Ссылка на свой задний фон (Если нужно) | Link to your background (If necessary)")]
            public string backgroundImageUrl;
        }

        public class Settings
        {
            [JsonProperty("Чат команда для открытия статистики | Chat command for opening statistics")]
            public string chatCommandOpenStat;
            [JsonProperty("Консольная команда для открытия статистики | Console command to open statistics")]
            public string consoleCommandOpenStat;
            [JsonProperty("Отправлять в чат сообщения с топ 5 игроками в разных категориях | Send chat messages with top 5 players in different categories")]
            public bool chatSendTop;
            [JsonProperty("Раз в сколько секунд будет отправлятся сообщение ? | Once in how many seconds will a message be sent ?")]
            public int chatSendTopTime;
            [JsonProperty("Включить возможность сбросить свою статистику ? (требуется XDStatistics.reset) | Enable the ability to reset your stats ? (requires XDStatistics.reset)")]
            public bool dropStatUse;
            [JsonProperty("Включить возможность скрыть свою статистику от пользователей ? (требуется XDStatistics.availability) | Enable the ability to hide your statistics from users ? (requires XDStatistics.availability)")]
            public bool availabilityUse;
            [JsonProperty("очищать данные при вайпе | Clear data when wiped")]
            public bool wipeData;
            [JsonProperty("Раз во сколько минут будут сохранятся данные | Once in a rowman, the data will be saved.")]
            public int dataSaveTime;
            [JsonProperty("Учитывать убийство npc для фаворитного оружия ? | Consider killing an NPC for a favorite weapon ?")]
            public bool npsDeathUse;
            [JsonProperty("У вас PVE сервер ?  | You have a PVE server?")]
            public bool pveServerMode;
        }

        public class SettingsScore
        {
            [JsonProperty("Очки за крафт | Points for crafting")]
            public float craftScore;
            [JsonProperty("Очки за бочки | Points for barrels")]
            public float barrelScore;
            [JsonProperty("Очки за установку строительных блоков | Points for installing building blocks")]
            public float BuildingScore;
            [JsonProperty("Очки за использования взрывчатых предметов | Points for using explosive items")]
            public Dictionary<string, float> ExplosionScore;
            [JsonProperty("Очки за добычу ресурсов | Points for resource extraction")]
            public Dictionary<string, float> GatherScore;
            [JsonProperty("Очки за найденные скрап | Points for found scraps")]
            public float ScrapScore;
            [JsonProperty("Очки за сбор урожая (с плантации) | Points for harvesting (from the plantation)")]
            public float PlantScore;
            [JsonProperty("Очки за убийство животных | Points for killing animals")]
            public float AnimalScore;
            [JsonProperty("Очки за сбитие вертолета | Points for shooting down a helicopter")]
            public float HeliScore;
            [JsonProperty("Очки за взрыв танка | Points for tank explosion")]
            public float BradleyScore;
            [JsonProperty("Очки за убийство нпс | Points for killing NPCs")]
            public float NpcScore;
            [JsonProperty("Очки за убийство игроков | Points for killing players")]
            public float PlayerScore;
            [JsonProperty("Очки за время (За кажду. минуту игры на сервере) | Points for time (for every minute of the game on the server)")]
            public float TimeScore;
            [JsonProperty("Сколько отнять очков за суицид ? | How many points to take away for suicide ?")]
            public float SuicideScore;
            [JsonProperty("Сколько отнять очков за смерть ? | How many points to take away for death ?")]
            public float DeathScore;
        }

        public class SettingsPrize
        {
            [JsonProperty("Использовать выдачу награды после вайпа ? | Use the award issue after the wipe ?")]
            public bool prizeUse;
            [JsonProperty("Награда в категории SCORE | Award in the SCORE category")]
            public List<Prize> prizeScore;
            [JsonProperty("Награда в категории Киллер | Award in the Killer category")]
            public List<Prize> prizeKiller;
            [JsonProperty("Награда в категории Фармила | Award in the gathering category")]
            public List<Prize> prizeFarm;
            [JsonProperty("Награда в категории рейдер | Reward in the Raider category")]
            public List<Prize> prizeRaid;
            [JsonProperty("Награда в категории Большой онлайн | Award in the Big Online category")]
            public List<Prize> prizeTime;
            [JsonProperty("Награда в категории Убийца НПС | Reward in the NPC Killer category")]
            public List<Prize> prizeNPCKiller;
            [JsonProperty("[RU][GameStores] ID магазина")]
            public string ShopID;
            [JsonProperty("[RU][GameStores] ID сервера")]
            public string ServerID;
            [JsonProperty("[RU][GameStores] Секретный ключ")]
            public string SecretKey;
            internal class Prize
            {
                [JsonProperty("Использовать комманды в виде приза ? | Use command as a prize ?")]
                public bool commandPrizeUse;
                [JsonProperty("За какое место давать эту награду ? (от 1 до 3) если не нужно награждать эту категорию. удалите все награды | For which place to give this award ? (from 1 to 3) if you do not need to award this category. remove all rewards")]
                public int top;
                [JsonProperty("[RU]Использовать магазин GameStore для выдачи награды")]
                public bool gamestorePrizeUse;
                [JsonProperty("[RU]Использовать магазин MoscowOVH для выдачи награды")]
                public bool ovhPrizeUse;
                [JsonProperty("Использовать [IQEconomic или Economics или ServerRewards] для выдачи награды | Use [IQEconomic or Economics or Server Rewards] to issue a reward")]
                public bool economicPrizeUse;
                [JsonProperty("Команды для приза | Command for the prize")]
                public List<string> commandPrizeList;
                [JsonProperty("[RU][GameStores] Сообщения для истории в магазине")]
                public string balancePlusMess;
                [JsonProperty("[RU][GameStores или MoscowOVH] Сколько начислять денег на баланс")]
                public int balancePlus;
                [JsonProperty("[IQEconomic или Economics или ServerRewards] Сколько начислять денег на баланс | [IQEconomic or Economics or ServerRewards] How much money to add to the balance")]
                public int balanceEconomicsPlus;
                public void GiftPrizePlayer(string player);
            }

        }

        [JsonProperty("Основные настройки плагина | Basic plugin settings")]
        public Settings settings;
        [JsonProperty("Настройка выдачи очков | Setting up the issuance of points")]
        public SettingsScore settingsScore;
        [JsonProperty("Настройка награды топ игрокам в каждой категории | Customize rewards for top 1 players in each category")]
        public SettingsPrize settingsPrize;
        [JsonProperty("Настройки интерфейса | Interface Settings")]
        public SettingsInterface settingsInterface;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private static Dictionary<ulong, PlayerInfo> Players;
    private static Dictionary<ulong, PlayerInfo> IgnoreReservedPlayer;
    private static Dictionary<ulong, List<PrizePlayer>> PrizePlayerData;
    private class PrizePlayer
    {
        public string Name;
        public CatType catType;
        public int value;
        public int top;
    }

    private class PlayerInfo
    {
        public string Name;
        public bool HidedStatistics;
        public float Score;
        public Harvesting harvesting;
        public OtherStat otherStat;
        public Explosion explosion;
        public Gather gather;
        public Weapon weapon;
        public PVP pVP;
        public PlayedTime playedTime;
        internal class PlayedTime
        {
            public string DayNumber;
            public int PlayedForWipe;
            public int PlayedToday;
        }

        internal class Harvesting
        {
            public Dictionary<string, int> HarvestingList;
            public int AllHarvesting;
        }

        internal class OtherStat
        {
            public int CrateOpen;
            public int BarrelDeath;
            public int AllCraft;
            public int BuildingCrate;
            public int AnimalsKill;
        }

        internal class Explosion
        {
            public Dictionary<string, int> ExplosionUsed;
            public int AllExplosionUsed;
        }

        internal class Gather
        {
            public Dictionary<string, int> GatheredTotal;
            public int AllGathered;
        }

        internal class Weapon
        {
            public Dictionary<string, WeaponInfo> WeaponUsed;
            internal class WeaponInfo
            {
                public int Kills;
                public int Headshots;
                public int Shots;
            }

        }

        internal class PVP
        {
            public int Kills;
            public int KillsNpc;
            public int Deaths;
            public int Suicides;
            public int Shots;
            public int Headshots;
            public int HeliKill;
            public int BradleyKill;
        }

        public static void AddPlayedTime(ulong id);
        public static void PlayerClearData(ulong id);
        public static void ClearDataWipe();
        public static PlayerInfo Find(ulong id);
    }

    private void LoadData();
    private void SaveData();
    private void LoadDataIgnoreList();
    private void SaveDataIgnoreList();
    private void LoadDataPrize();
    private void SaveDataPrize();
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private void OnEntityDeath(LootContainer entity, HitInfo info);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity, ThrownWeapon item);
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
    private void OnContainerDropItems(ItemContainer container);
     void OnItemPickup(Item item, BasePlayer player);
    private static Dictionary<BasePlayer, List<UInt64>> LootersListCarte;
    private void OnLootEntity(BasePlayer player, LootContainer entity);
     Dictionary<uint, ulong> _heliattacker;
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private object OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hitinfo);
     void OnGrowableGathered(GrowableEntity plant, Item item, BasePlayer player);
    private void Unload();
    private void Init();
    private void OnNewSave();
    private void OnPlayerConnected(BasePlayer player);
    private void OnServerInitialized();
    public const string UI_INTERFACE;
    public const string UI_MENU_BUTTON;
    public const string UI_USER_STAT;
    public const string UI_USER_STAT_INFO;
    public const string UI_CLOSE_MENU;
    public const string UI_SEARCH_USER;
    public const string UI_TOP_TEN_USER;
    private void CloseLayer(BasePlayer player);
    private void MainMenuStat(BasePlayer player);
    private void MenuButton(BasePlayer player, int page);
    private void SearchPageUser(BasePlayer player, string target);
    private void UserStat(BasePlayer player, ulong target);
    private void ButtonHideStat(BasePlayer player, PlayerInfo info);
    private void DialogConfirmationDropStat(BasePlayer player);
    private void ButtonDropStat(BasePlayer player, PlayerInfo info);
    private void OtherStatUser(BasePlayer player, ulong target, int statType);
    private void CategoryStatUser(BasePlayer player, ulong target, int cat);
    private void TopTen(BasePlayer player);
    private void SendMsgRewardWipe(BasePlayer player);
    private void ParseTopUserForPrize();
    private void PlayersTopOnChat();
    public string GetLang(string LangKey, string userID, object[] args);
    public class items
    {
        public String shortName;
        public String ENdisplayName;
        public String RUdisplayName;
    }

    private class ItemDisplayName
    {
        public String ru;
        public String en;
    }

    private void AddDisplayName();
     void SteamAvatarAdd(string userid);
    private Dictionary<string, int> GetCategory(PlayerInfo statInfo, int cat);
    private static List<KeyValuePair<ulong, PlayerInfo>> FindPlayers(string name);
    private void CheckInMinute();
    public static class TimeHelper
    {
        public static string FormatTime(TimeSpan time, int maxSubstr, string language);
        private static string Format(int units, string form1, string form2, string form3);
    }

     Int32 GetTopScore(ulong userid);
    private string GetCorrectName(string name, int length);
    private void ExplosionProgressAdd(BasePlayer player, BaseEntity entity, string shortname);
    private void WeaponProgressAdd(PlayerInfo player, HitInfo hitinfo, bool kill);
    private void ProgressAdd(BasePlayer player, Item item, bool scoreGive);
    private void ConsoleCommandOpenMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("stat.topuser")]
    private void CmdTestVkPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("stat.score")]
     void ShopMoneyGive(ConsoleSystem.Arg arg);
    [ConsoleCommand("stat.ignore")]
    private void CmdIgnorePlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_HandlerStat")]
    private void CmdConsoleHandler(ConsoleSystem.Arg args);
}

private class Configuration
{
    public class SettingsInterface
    {
        [JsonProperty("Цвет плашки заднего фона в топ 10 за 1 место | Background color in the top 10 for 1st place")]
        public string ColorTop1;
        [JsonProperty("Цвет плашки заднего фона в топ 10 за 2 место | Background color in the top 10 for 2st place")]
        public string ColorTop2;
        [JsonProperty("Цвет плашки заднего фона в топ 10 за 3 место | Background color in the top 10 for 3st place")]
        public string ColorTop3;
        [JsonProperty("Использовать свой задний фон ? (указанный снизу)| Use your own background ? (indicated at the bottom)")]
        public bool UsebackgroundImageUrl;
        [JsonProperty("Ссылка на свой задний фон (Если нужно) | Link to your background (If necessary)")]
        public string backgroundImageUrl;
    }

    public class Settings
    {
        [JsonProperty("Чат команда для открытия статистики | Chat command for opening statistics")]
        public string chatCommandOpenStat;
        [JsonProperty("Консольная команда для открытия статистики | Console command to open statistics")]
        public string consoleCommandOpenStat;
        [JsonProperty("Отправлять в чат сообщения с топ 5 игроками в разных категориях | Send chat messages with top 5 players in different categories")]
        public bool chatSendTop;
        [JsonProperty("Раз в сколько секунд будет отправлятся сообщение ? | Once in how many seconds will a message be sent ?")]
        public int chatSendTopTime;
        [JsonProperty("Включить возможность сбросить свою статистику ? (требуется XDStatistics.reset) | Enable the ability to reset your stats ? (requires XDStatistics.reset)")]
        public bool dropStatUse;
        [JsonProperty("Включить возможность скрыть свою статистику от пользователей ? (требуется XDStatistics.availability) | Enable the ability to hide your statistics from users ? (requires XDStatistics.availability)")]
        public bool availabilityUse;
        [JsonProperty("очищать данные при вайпе | Clear data when wiped")]
        public bool wipeData;
        [JsonProperty("Раз во сколько минут будут сохранятся данные | Once in a rowman, the data will be saved.")]
        public int dataSaveTime;
        [JsonProperty("Учитывать убийство npc для фаворитного оружия ? | Consider killing an NPC for a favorite weapon ?")]
        public bool npsDeathUse;
        [JsonProperty("У вас PVE сервер ?  | You have a PVE server?")]
        public bool pveServerMode;
    }

    public class SettingsScore
    {
        [JsonProperty("Очки за крафт | Points for crafting")]
        public float craftScore;
        [JsonProperty("Очки за бочки | Points for barrels")]
        public float barrelScore;
        [JsonProperty("Очки за установку строительных блоков | Points for installing building blocks")]
        public float BuildingScore;
        [JsonProperty("Очки за использования взрывчатых предметов | Points for using explosive items")]
        public Dictionary<string, float> ExplosionScore;
        [JsonProperty("Очки за добычу ресурсов | Points for resource extraction")]
        public Dictionary<string, float> GatherScore;
        [JsonProperty("Очки за найденные скрап | Points for found scraps")]
        public float ScrapScore;
        [JsonProperty("Очки за сбор урожая (с плантации) | Points for harvesting (from the plantation)")]
        public float PlantScore;
        [JsonProperty("Очки за убийство животных | Points for killing animals")]
        public float AnimalScore;
        [JsonProperty("Очки за сбитие вертолета | Points for shooting down a helicopter")]
        public float HeliScore;
        [JsonProperty("Очки за взрыв танка | Points for tank explosion")]
        public float BradleyScore;
        [JsonProperty("Очки за убийство нпс | Points for killing NPCs")]
        public float NpcScore;
        [JsonProperty("Очки за убийство игроков | Points for killing players")]
        public float PlayerScore;
        [JsonProperty("Очки за время (За кажду. минуту игры на сервере) | Points for time (for every minute of the game on the server)")]
        public float TimeScore;
        [JsonProperty("Сколько отнять очков за суицид ? | How many points to take away for suicide ?")]
        public float SuicideScore;
        [JsonProperty("Сколько отнять очков за смерть ? | How many points to take away for death ?")]
        public float DeathScore;
    }

    public class SettingsPrize
    {
        [JsonProperty("Использовать выдачу награды после вайпа ? | Use the award issue after the wipe ?")]
        public bool prizeUse;
        [JsonProperty("Награда в категории SCORE | Award in the SCORE category")]
        public List<Prize> prizeScore;
        [JsonProperty("Награда в категории Киллер | Award in the Killer category")]
        public List<Prize> prizeKiller;
        [JsonProperty("Награда в категории Фармила | Award in the gathering category")]
        public List<Prize> prizeFarm;
        [JsonProperty("Награда в категории рейдер | Reward in the Raider category")]
        public List<Prize> prizeRaid;
        [JsonProperty("Награда в категории Большой онлайн | Award in the Big Online category")]
        public List<Prize> prizeTime;
        [JsonProperty("Награда в категории Убийца НПС | Reward in the NPC Killer category")]
        public List<Prize> prizeNPCKiller;
        [JsonProperty("[RU][GameStores] ID магазина")]
        public string ShopID;
        [JsonProperty("[RU][GameStores] ID сервера")]
        public string ServerID;
        [JsonProperty("[RU][GameStores] Секретный ключ")]
        public string SecretKey;
        internal class Prize
        {
            [JsonProperty("Использовать комманды в виде приза ? | Use command as a prize ?")]
            public bool commandPrizeUse;
            [JsonProperty("За какое место давать эту награду ? (от 1 до 3) если не нужно награждать эту категорию. удалите все награды | For which place to give this award ? (from 1 to 3) if you do not need to award this category. remove all rewards")]
            public int top;
            [JsonProperty("[RU]Использовать магазин GameStore для выдачи награды")]
            public bool gamestorePrizeUse;
            [JsonProperty("[RU]Использовать магазин MoscowOVH для выдачи награды")]
            public bool ovhPrizeUse;
            [JsonProperty("Использовать [IQEconomic или Economics или ServerRewards] для выдачи награды | Use [IQEconomic or Economics or Server Rewards] to issue a reward")]
            public bool economicPrizeUse;
            [JsonProperty("Команды для приза | Command for the prize")]
            public List<string> commandPrizeList;
            [JsonProperty("[RU][GameStores] Сообщения для истории в магазине")]
            public string balancePlusMess;
            [JsonProperty("[RU][GameStores или MoscowOVH] Сколько начислять денег на баланс")]
            public int balancePlus;
            [JsonProperty("[IQEconomic или Economics или ServerRewards] Сколько начислять денег на баланс | [IQEconomic or Economics or ServerRewards] How much money to add to the balance")]
            public int balanceEconomicsPlus;
            public void GiftPrizePlayer(string player);
        }

    }

    [JsonProperty("Основные настройки плагина | Basic plugin settings")]
    public Settings settings;
    [JsonProperty("Настройка выдачи очков | Setting up the issuance of points")]
    public SettingsScore settingsScore;
    [JsonProperty("Настройка награды топ игрокам в каждой категории | Customize rewards for top 1 players in each category")]
    public SettingsPrize settingsPrize;
    [JsonProperty("Настройки интерфейса | Interface Settings")]
    public SettingsInterface settingsInterface;
}

public class SettingsInterface
{
    [JsonProperty("Цвет плашки заднего фона в топ 10 за 1 место | Background color in the top 10 for 1st place")]
    public string ColorTop1;
    [JsonProperty("Цвет плашки заднего фона в топ 10 за 2 место | Background color in the top 10 for 2st place")]
    public string ColorTop2;
    [JsonProperty("Цвет плашки заднего фона в топ 10 за 3 место | Background color in the top 10 for 3st place")]
    public string ColorTop3;
    [JsonProperty("Использовать свой задний фон ? (указанный снизу)| Use your own background ? (indicated at the bottom)")]
    public bool UsebackgroundImageUrl;
    [JsonProperty("Ссылка на свой задний фон (Если нужно) | Link to your background (If necessary)")]
    public string backgroundImageUrl;
}

public class Settings
{
    [JsonProperty("Чат команда для открытия статистики | Chat command for opening statistics")]
    public string chatCommandOpenStat;
    [JsonProperty("Консольная команда для открытия статистики | Console command to open statistics")]
    public string consoleCommandOpenStat;
    [JsonProperty("Отправлять в чат сообщения с топ 5 игроками в разных категориях | Send chat messages with top 5 players in different categories")]
    public bool chatSendTop;
    [JsonProperty("Раз в сколько секунд будет отправлятся сообщение ? | Once in how many seconds will a message be sent ?")]
    public int chatSendTopTime;
    [JsonProperty("Включить возможность сбросить свою статистику ? (требуется XDStatistics.reset) | Enable the ability to reset your stats ? (requires XDStatistics.reset)")]
    public bool dropStatUse;
    [JsonProperty("Включить возможность скрыть свою статистику от пользователей ? (требуется XDStatistics.availability) | Enable the ability to hide your statistics from users ? (requires XDStatistics.availability)")]
    public bool availabilityUse;
    [JsonProperty("очищать данные при вайпе | Clear data when wiped")]
    public bool wipeData;
    [JsonProperty("Раз во сколько минут будут сохранятся данные | Once in a rowman, the data will be saved.")]
    public int dataSaveTime;
    [JsonProperty("Учитывать убийство npc для фаворитного оружия ? | Consider killing an NPC for a favorite weapon ?")]
    public bool npsDeathUse;
    [JsonProperty("У вас PVE сервер ?  | You have a PVE server?")]
    public bool pveServerMode;
}

public class SettingsScore
{
    [JsonProperty("Очки за крафт | Points for crafting")]
    public float craftScore;
    [JsonProperty("Очки за бочки | Points for barrels")]
    public float barrelScore;
    [JsonProperty("Очки за установку строительных блоков | Points for installing building blocks")]
    public float BuildingScore;
    [JsonProperty("Очки за использования взрывчатых предметов | Points for using explosive items")]
    public Dictionary<string, float> ExplosionScore;
    [JsonProperty("Очки за добычу ресурсов | Points for resource extraction")]
    public Dictionary<string, float> GatherScore;
    [JsonProperty("Очки за найденные скрап | Points for found scraps")]
    public float ScrapScore;
    [JsonProperty("Очки за сбор урожая (с плантации) | Points for harvesting (from the plantation)")]
    public float PlantScore;
    [JsonProperty("Очки за убийство животных | Points for killing animals")]
    public float AnimalScore;
    [JsonProperty("Очки за сбитие вертолета | Points for shooting down a helicopter")]
    public float HeliScore;
    [JsonProperty("Очки за взрыв танка | Points for tank explosion")]
    public float BradleyScore;
    [JsonProperty("Очки за убийство нпс | Points for killing NPCs")]
    public float NpcScore;
    [JsonProperty("Очки за убийство игроков | Points for killing players")]
    public float PlayerScore;
    [JsonProperty("Очки за время (За кажду. минуту игры на сервере) | Points for time (for every minute of the game on the server)")]
    public float TimeScore;
    [JsonProperty("Сколько отнять очков за суицид ? | How many points to take away for suicide ?")]
    public float SuicideScore;
    [JsonProperty("Сколько отнять очков за смерть ? | How many points to take away for death ?")]
    public float DeathScore;
}

public class SettingsPrize
{
    [JsonProperty("Использовать выдачу награды после вайпа ? | Use the award issue after the wipe ?")]
    public bool prizeUse;
    [JsonProperty("Награда в категории SCORE | Award in the SCORE category")]
    public List<Prize> prizeScore;
    [JsonProperty("Награда в категории Киллер | Award in the Killer category")]
    public List<Prize> prizeKiller;
    [JsonProperty("Награда в категории Фармила | Award in the gathering category")]
    public List<Prize> prizeFarm;
    [JsonProperty("Награда в категории рейдер | Reward in the Raider category")]
    public List<Prize> prizeRaid;
    [JsonProperty("Награда в категории Большой онлайн | Award in the Big Online category")]
    public List<Prize> prizeTime;
    [JsonProperty("Награда в категории Убийца НПС | Reward in the NPC Killer category")]
    public List<Prize> prizeNPCKiller;
    [JsonProperty("[RU][GameStores] ID магазина")]
    public string ShopID;
    [JsonProperty("[RU][GameStores] ID сервера")]
    public string ServerID;
    [JsonProperty("[RU][GameStores] Секретный ключ")]
    public string SecretKey;
    internal class Prize
    {
        [JsonProperty("Использовать комманды в виде приза ? | Use command as a prize ?")]
        public bool commandPrizeUse;
        [JsonProperty("За какое место давать эту награду ? (от 1 до 3) если не нужно награждать эту категорию. удалите все награды | For which place to give this award ? (from 1 to 3) if you do not need to award this category. remove all rewards")]
        public int top;
        [JsonProperty("[RU]Использовать магазин GameStore для выдачи награды")]
        public bool gamestorePrizeUse;
        [JsonProperty("[RU]Использовать магазин MoscowOVH для выдачи награды")]
        public bool ovhPrizeUse;
        [JsonProperty("Использовать [IQEconomic или Economics или ServerRewards] для выдачи награды | Use [IQEconomic or Economics or Server Rewards] to issue a reward")]
        public bool economicPrizeUse;
        [JsonProperty("Команды для приза | Command for the prize")]
        public List<string> commandPrizeList;
        [JsonProperty("[RU][GameStores] Сообщения для истории в магазине")]
        public string balancePlusMess;
        [JsonProperty("[RU][GameStores или MoscowOVH] Сколько начислять денег на баланс")]
        public int balancePlus;
        [JsonProperty("[IQEconomic или Economics или ServerRewards] Сколько начислять денег на баланс | [IQEconomic or Economics or ServerRewards] How much money to add to the balance")]
        public int balanceEconomicsPlus;
        public void GiftPrizePlayer(string player);
    }

}

internal class Prize
{
    [JsonProperty("Использовать комманды в виде приза ? | Use command as a prize ?")]
    public bool commandPrizeUse;
    [JsonProperty("За какое место давать эту награду ? (от 1 до 3) если не нужно награждать эту категорию. удалите все награды | For which place to give this award ? (from 1 to 3) if you do not need to award this category. remove all rewards")]
    public int top;
    [JsonProperty("[RU]Использовать магазин GameStore для выдачи награды")]
    public bool gamestorePrizeUse;
    [JsonProperty("[RU]Использовать магазин MoscowOVH для выдачи награды")]
    public bool ovhPrizeUse;
    [JsonProperty("Использовать [IQEconomic или Economics или ServerRewards] для выдачи награды | Use [IQEconomic or Economics or Server Rewards] to issue a reward")]
    public bool economicPrizeUse;
    [JsonProperty("Команды для приза | Command for the prize")]
    public List<string> commandPrizeList;
    [JsonProperty("[RU][GameStores] Сообщения для истории в магазине")]
    public string balancePlusMess;
    [JsonProperty("[RU][GameStores или MoscowOVH] Сколько начислять денег на баланс")]
    public int balancePlus;
    [JsonProperty("[IQEconomic или Economics или ServerRewards] Сколько начислять денег на баланс | [IQEconomic or Economics or ServerRewards] How much money to add to the balance")]
    public int balanceEconomicsPlus;
    public void GiftPrizePlayer(string player);
}

private class PrizePlayer
{
    public string Name;
    public CatType catType;
    public int value;
    public int top;
}

private class PlayerInfo
{
    public string Name;
    public bool HidedStatistics;
    public float Score;
    public Harvesting harvesting;
    public OtherStat otherStat;
    public Explosion explosion;
    public Gather gather;
    public Weapon weapon;
    public PVP pVP;
    public PlayedTime playedTime;
    internal class PlayedTime
    {
        public string DayNumber;
        public int PlayedForWipe;
        public int PlayedToday;
    }

    internal class Harvesting
    {
        public Dictionary<string, int> HarvestingList;
        public int AllHarvesting;
    }

    internal class OtherStat
    {
        public int CrateOpen;
        public int BarrelDeath;
        public int AllCraft;
        public int BuildingCrate;
        public int AnimalsKill;
    }

    internal class Explosion
    {
        public Dictionary<string, int> ExplosionUsed;
        public int AllExplosionUsed;
    }

    internal class Gather
    {
        public Dictionary<string, int> GatheredTotal;
        public int AllGathered;
    }

    internal class Weapon
    {
        public Dictionary<string, WeaponInfo> WeaponUsed;
        internal class WeaponInfo
        {
            public int Kills;
            public int Headshots;
            public int Shots;
        }

    }

    internal class PVP
    {
        public int Kills;
        public int KillsNpc;
        public int Deaths;
        public int Suicides;
        public int Shots;
        public int Headshots;
        public int HeliKill;
        public int BradleyKill;
    }

    public static void AddPlayedTime(ulong id);
    public static void PlayerClearData(ulong id);
    public static void ClearDataWipe();
    public static PlayerInfo Find(ulong id);
}

internal class PlayedTime
{
    public string DayNumber;
    public int PlayedForWipe;
    public int PlayedToday;
}

internal class Harvesting
{
    public Dictionary<string, int> HarvestingList;
    public int AllHarvesting;
}

internal class OtherStat
{
    public int CrateOpen;
    public int BarrelDeath;
    public int AllCraft;
    public int BuildingCrate;
    public int AnimalsKill;
}

internal class Explosion
{
    public Dictionary<string, int> ExplosionUsed;
    public int AllExplosionUsed;
}

internal class Gather
{
    public Dictionary<string, int> GatheredTotal;
    public int AllGathered;
}

internal class Weapon
{
    public Dictionary<string, WeaponInfo> WeaponUsed;
    internal class WeaponInfo
    {
        public int Kills;
        public int Headshots;
        public int Shots;
    }

}

internal class WeaponInfo
{
    public int Kills;
    public int Headshots;
    public int Shots;
}

internal class PVP
{
    public int Kills;
    public int KillsNpc;
    public int Deaths;
    public int Suicides;
    public int Shots;
    public int Headshots;
    public int HeliKill;
    public int BradleyKill;
}

public class items
{
    public String shortName;
    public String ENdisplayName;
    public String RUdisplayName;
}

private class ItemDisplayName
{
    public String ru;
    public String en;
}

public static class TimeHelper
{
    public static string FormatTime(TimeSpan time, int maxSubstr, string language);
    private static string Format(int units, string form1, string form2, string form3);
}


```

---

## XDWipeReward

```csharp
using System.Collections.Generic;
using System;
using ConVar;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using Rust;

Oxide.Plugins
[Info("XDWipeReward", "TopPlugin.ru", "1.2.3")]
[Description("Награда первым N игрокам после вайпа")]
public class XDWipeReward : RustPlugin
{
    private void LoadPlayerData();
     void OnNewSave(string filename);
    public static string WipeR;
    public class Setings
    {
        [JsonProperty("Команда для выдачи приза (если не нужно то оставить поля пустым)")]
        public string CommandPrize;
        [JsonProperty("У вас магазин ОВХ?")]
        public bool OVHStore;
        [JsonProperty("API KEY Магазина(GameStore. Если OVH оставить пустым)")]
        public string Store_Key;
        [JsonProperty("Бонус в виде баланса GameStores или OVH (если не нужно оставить пустым)")]
        public string GameStoreBonus;
        [JsonProperty("Id Магазина(GameStore. Если OVH оставить пустым)")]
        public string Store_Id;
        [JsonProperty("Количество игроков")]
        public int PlayersIntConnect;
        [JsonProperty("Лог сообщения(Показывается в магазине после выдачи в истории. Если OVH оставить пустым)")]
        public string GameStoreMSG;
    }

    public Configuration config;
    private void Unload();
     bool Wipe;
    private Dictionary<ulong, bool> playersInfo;
    public void WipeRewardGui(BasePlayer p);
     List<ulong> Players;
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
     void OnPlayerConnected(BasePlayer player);
     void GiveReward(ulong ID);
    private void Init();
    [PluginReference]
     Plugin IQChat;
    protected override void LoadDefaultConfig();
    [ConsoleCommand("giveprize")]
     void GivePrize(ConsoleSystem.Arg arg);
    public class Configuration
    {
        [JsonProperty("Настройки")]
        public Setings setings;
    }

    public void LoadConfigVars();
    private void SavePlayerData();
    private void OnServerInitialized();
    private static string HexToRustFormat(string hex);
     void SaveConfig(Configuration config);
}

public class Setings
{
    [JsonProperty("Команда для выдачи приза (если не нужно то оставить поля пустым)")]
    public string CommandPrize;
    [JsonProperty("У вас магазин ОВХ?")]
    public bool OVHStore;
    [JsonProperty("API KEY Магазина(GameStore. Если OVH оставить пустым)")]
    public string Store_Key;
    [JsonProperty("Бонус в виде баланса GameStores или OVH (если не нужно оставить пустым)")]
    public string GameStoreBonus;
    [JsonProperty("Id Магазина(GameStore. Если OVH оставить пустым)")]
    public string Store_Id;
    [JsonProperty("Количество игроков")]
    public int PlayersIntConnect;
    [JsonProperty("Лог сообщения(Показывается в магазине после выдачи в истории. Если OVH оставить пустым)")]
    public string GameStoreMSG;
}

public class Configuration
{
    [JsonProperty("Настройки")]
    public Setings setings;
}


```

---

## XerCopterCraft

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("XerCopterCraft", "Mercury", "1.0.2")]
 class XerCopterCraft : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    private string prefab;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("SkinId (Иконка в инвентаре)")]
        public ulong skinID;
        [JsonProperty("Миникоптер(Эту вещь игрок будет держать в руках,когда поставит - он заменится на коптер)")]
        public string Item;
        [JsonProperty("Название вещи в инвентаре")]
        public string ItemName;
        [JsonProperty("Вещи для крафта")]
        public Dictionary<string, int> CraftItemList;
        public static Configuration GetNewConfiguration();
        internal class Interface
        {
            [JsonProperty("Title в меню")]
            public string TitleMenu;
            [JsonProperty("Title предметов в меню")]
            public string TitleItems;
            [JsonProperty("Текст в кнопке")]
            public string ButtonTitle;
            [JsonProperty("Символ показывающий,что у игрока достаточно предметов на крафт")]
            public string Sufficiently;
            [JsonProperty("Цвет символа(HEX)")]
            public string SufficientlyColor;
            [JsonProperty("Цвет показателя,сколько необходимо еще компонентов на создание")]
            public string IndispensablyColor;
            [JsonProperty("Minicopter.png(512x512)")]
            public string CopterPNG;
        }

        internal class Other
        {
            [JsonProperty("Звук при создании коптера")]
            public string EffectCreatedCopter;
            [JsonProperty("Звук когда у игрока недостаточно ресурсов")]
            public string EffectCanceled;
        }

        [JsonProperty("Настройки интерфейса")]
        public Interface InterfaceSettings;
        [JsonProperty("Дополнительные настройки")]
        public Other OtherSettings;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("copter")]
     void OpenCraftMenu(BasePlayer player);
    [ConsoleCommand("craft_copter")]
     void CraftCopter(ConsoleSystem.Arg args);
    [ConsoleCommand("give_minicopter")]
     void GiveMinicopterCommand(ConsoleSystem.Arg args);
     void OnServerInitialized();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void SpawnCopter(Vector3 position, Quaternion rotation, ulong ownerID);
    private void GiveMinicopter(BasePlayer player, bool pickup);
    private void CheckDeploy(BaseEntity entity);
    private bool CopterCheck(ulong skin);
    private Item CreateItem();
    private bool CraftCheck(BasePlayer player);
    private bool UseCraft(BasePlayer player, string Short);
    static string MainPanel;
    static string CraftItemsPanel;
    static string ItemParent;
    static string MessagePanel;
     void MessageUI(BasePlayer player, string Messages, string Color);
     void OpenMenuCraft(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

private class Configuration
{
    [JsonProperty("SkinId (Иконка в инвентаре)")]
    public ulong skinID;
    [JsonProperty("Миникоптер(Эту вещь игрок будет держать в руках,когда поставит - он заменится на коптер)")]
    public string Item;
    [JsonProperty("Название вещи в инвентаре")]
    public string ItemName;
    [JsonProperty("Вещи для крафта")]
    public Dictionary<string, int> CraftItemList;
    public static Configuration GetNewConfiguration();
    internal class Interface
    {
        [JsonProperty("Title в меню")]
        public string TitleMenu;
        [JsonProperty("Title предметов в меню")]
        public string TitleItems;
        [JsonProperty("Текст в кнопке")]
        public string ButtonTitle;
        [JsonProperty("Символ показывающий,что у игрока достаточно предметов на крафт")]
        public string Sufficiently;
        [JsonProperty("Цвет символа(HEX)")]
        public string SufficientlyColor;
        [JsonProperty("Цвет показателя,сколько необходимо еще компонентов на создание")]
        public string IndispensablyColor;
        [JsonProperty("Minicopter.png(512x512)")]
        public string CopterPNG;
    }

    internal class Other
    {
        [JsonProperty("Звук при создании коптера")]
        public string EffectCreatedCopter;
        [JsonProperty("Звук когда у игрока недостаточно ресурсов")]
        public string EffectCanceled;
    }

    [JsonProperty("Настройки интерфейса")]
    public Interface InterfaceSettings;
    [JsonProperty("Дополнительные настройки")]
    public Other OtherSettings;
}

internal class Interface
{
    [JsonProperty("Title в меню")]
    public string TitleMenu;
    [JsonProperty("Title предметов в меню")]
    public string TitleItems;
    [JsonProperty("Текст в кнопке")]
    public string ButtonTitle;
    [JsonProperty("Символ показывающий,что у игрока достаточно предметов на крафт")]
    public string Sufficiently;
    [JsonProperty("Цвет символа(HEX)")]
    public string SufficientlyColor;
    [JsonProperty("Цвет показателя,сколько необходимо еще компонентов на создание")]
    public string IndispensablyColor;
    [JsonProperty("Minicopter.png(512x512)")]
    public string CopterPNG;
}

internal class Other
{
    [JsonProperty("Звук при создании коптера")]
    public string EffectCreatedCopter;
    [JsonProperty("Звук когда у игрока недостаточно ресурсов")]
    public string EffectCanceled;
}


```

---

## XFarmRoom

```csharp
using System.Collections;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using System;
using Oxide.Game.Rust.Cui;
using System.Linq;
using System.Collections.Generic;

Oxide.Plugins
[Info("XFarmRoom", "Monster", "1.0.1")]
 class XFarmRoom : RustPlugin
{
    private bool CanPickupEntity(BasePlayer player, BaseEntity entity);
    private void ChatMessage(BasePlayer player, string message);
    protected override void LoadConfig();
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void Unload();
    internal class PrefabsRoom
    {
        public PrefabsRoom(string shortname, Vector3 pos, Vector3 rot);
        public string ShortPrefabName;
        public Vector3 Rotation;
        public Vector3 Position;
    }

    [ConsoleCommand("xfarmroom_give_ore")]
     void ccmdGiveOre(ConsoleSystem.Arg args);
    [ConsoleCommand("xfarmroom_clear_ore")]
     void ccmdClearOre(ConsoleSystem.Arg args);
    private object CanBuild(Planner planner);
    private void RemoveRoomOrEntity(Vector3 position);
    [ChatCommand("roomtp")]
     void cmdTPFarmRoom(BasePlayer player);
    private void TP(BasePlayer player, Vector3 position);
    protected override void SaveConfig();
    public List<Vector3> _spawn_room_position;
    private void OnUserPermissionRevoked(string id, string permName);
    private bool IsInvisible(BasePlayer player);
    private List<string> GetOres(ulong userID);
    private void OnUserPermissionGranted(string id, string permName);
    private void SpawnOre(ulong userID);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void CheckGroup(string id, string groupName, bool added);
    private object OnServerCommand(ConsoleSystem.Arg args);
    internal class PlayersInRoom
    {
        public Vector3 RoomPosition;
        public Vector3 LastPlayerPosition;
        public List<string> OresSpawn;
        public PlayersInRoom(Vector3 roomposition, Vector3 lastplayerposition, List<string> oresspawn);
    }

    private bool CheckPosition(Vector3 position);
    private void InitializeLang();
    private Dictionary<ulong, Data> StoredData;
    [ChatCommand("roomspawns")]
     void cmdSpawnRoomPosition(BasePlayer player);
    [ChatCommand("roomleave")]
     void cmdLeaveFarmRoom(BasePlayer player);
    private bool API_PlayerInRoom(ulong userID);
    private void OnUserGroupRemoved(string id, string groupName);
    private Coroutine _coroutine;
    [ConsoleCommand("farm_ore")]
     void ccmdFarmOre(ConsoleSystem.Arg args);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private Dictionary<ulong, int> Cooldowns;
    private object OnPlayerCommand(BasePlayer player, string command, string[] args);
    private Dictionary<ulong, PlayersInRoom> _players_in_room;
    private void OnNewSave();
    private FarmRoomConfig config;
    private void SaveData();
    private void CheckPermission(string id, string permName, bool granted);
    private void GUIFarmRoom(BasePlayer player);
    protected override void LoadDefaultConfig();
    internal class Data
    {
        [JsonProperty("Последнее обновление доступных камней")]
        public int LastUpdate;
        [JsonProperty("Доступные камни")]
        public Dictionary<string, int> Ore;
        public Data(int lastupdate, Dictionary<string, int> ore);
    }

    private bool GetOresPermission(BasePlayer player);
    public List<string> _ores_shortname;
    private void OnServerInitialized();
    private void SpawnFarmRoom(Vector3 position);
    private class FarmRoomConfig
    {
        internal class GeneralSetting
        {
            [JsonProperty("Вариант обновления доступных камней. ( только с разрешением на обновление ). [ True - раз в вайп | False - раз в N секунд ]")]
            public bool UpdateOption;
            [JsonProperty("Очищать дату после вайпа")]
            public bool DataClear;
            [JsonProperty("Ограничивать кол-во предметов которые можно взять в комнату")]
            public bool UseMaxCountItem;
            [JsonProperty("Использовать UI кнопку для выхода из комнаты")]
            public bool UseUIButton;
            [JsonProperty("Раз в сколько сек. обновлять кол-во доступных камней. ( проверяется только тогда, когда игрок пытается/попадает в комнату )")]
            public int UpdateSecond;
            [JsonProperty("Сколько максимум одновременно активных комнат может быть. ( для оптимизации )")]
            public int MaxCountRoom;
            [JsonProperty("Сколько максимум предметов можно взять в комнату")]
            public int MaxCountItem;
            [JsonProperty("Перерыв на телепортацию в комнату сек.")]
            public int CDTPRoom;
            [JsonProperty("Префикс в чате")]
            public string PrefixChat;
            [JsonProperty("Уведомлять игрока когда ему выдали или отобрали доступ к функционалу фарм комнаты")]
            public bool ChatMessages;
            [JsonProperty("SteamID профиля для кастомной аватарки")]
            public ulong SteamID;
            [JsonProperty("Список разрешенных консольных команд в фарм комнате")]
            public List<string> ConsoleCommand;
            [JsonProperty("Список разрешенных чат команд в фарм комнате")]
            public List<string> ChatCommand;
        }

        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Пермишен - кол-во камней. [ Изменять можно только значение и пермишен ]")]
        public Dictionary<string, Dictionary<string, int>> Permission;
        public static FarmRoomConfig GetNewConfiguration();
    }

    private void OnUserGroupAdded(string id, string groupName);
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Vanish;
    private IEnumerator GeneratePosition();
    private Dictionary<string, string> _ore_images;
    public List<PrefabsRoom> _prefabs_room;
    private ItemId v;
    private void LeaveFarmRoom(BasePlayer player, bool isunload);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
}

internal class PrefabsRoom
{
    public PrefabsRoom(string shortname, Vector3 pos, Vector3 rot);
    public string ShortPrefabName;
    public Vector3 Rotation;
    public Vector3 Position;
}

internal class PlayersInRoom
{
    public Vector3 RoomPosition;
    public Vector3 LastPlayerPosition;
    public List<string> OresSpawn;
    public PlayersInRoom(Vector3 roomposition, Vector3 lastplayerposition, List<string> oresspawn);
}

internal class Data
{
    [JsonProperty("Последнее обновление доступных камней")]
    public int LastUpdate;
    [JsonProperty("Доступные камни")]
    public Dictionary<string, int> Ore;
    public Data(int lastupdate, Dictionary<string, int> ore);
}

private class FarmRoomConfig
{
    internal class GeneralSetting
    {
        [JsonProperty("Вариант обновления доступных камней. ( только с разрешением на обновление ). [ True - раз в вайп | False - раз в N секунд ]")]
        public bool UpdateOption;
        [JsonProperty("Очищать дату после вайпа")]
        public bool DataClear;
        [JsonProperty("Ограничивать кол-во предметов которые можно взять в комнату")]
        public bool UseMaxCountItem;
        [JsonProperty("Использовать UI кнопку для выхода из комнаты")]
        public bool UseUIButton;
        [JsonProperty("Раз в сколько сек. обновлять кол-во доступных камней. ( проверяется только тогда, когда игрок пытается/попадает в комнату )")]
        public int UpdateSecond;
        [JsonProperty("Сколько максимум одновременно активных комнат может быть. ( для оптимизации )")]
        public int MaxCountRoom;
        [JsonProperty("Сколько максимум предметов можно взять в комнату")]
        public int MaxCountItem;
        [JsonProperty("Перерыв на телепортацию в комнату сек.")]
        public int CDTPRoom;
        [JsonProperty("Префикс в чате")]
        public string PrefixChat;
        [JsonProperty("Уведомлять игрока когда ему выдали или отобрали доступ к функционалу фарм комнаты")]
        public bool ChatMessages;
        [JsonProperty("SteamID профиля для кастомной аватарки")]
        public ulong SteamID;
        [JsonProperty("Список разрешенных консольных команд в фарм комнате")]
        public List<string> ConsoleCommand;
        [JsonProperty("Список разрешенных чат команд в фарм комнате")]
        public List<string> ChatCommand;
    }

    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Пермишен - кол-во камней. [ Изменять можно только значение и пермишен ]")]
    public Dictionary<string, Dictionary<string, int>> Permission;
    public static FarmRoomConfig GetNewConfiguration();
}

internal class GeneralSetting
{
    [JsonProperty("Вариант обновления доступных камней. ( только с разрешением на обновление ). [ True - раз в вайп | False - раз в N секунд ]")]
    public bool UpdateOption;
    [JsonProperty("Очищать дату после вайпа")]
    public bool DataClear;
    [JsonProperty("Ограничивать кол-во предметов которые можно взять в комнату")]
    public bool UseMaxCountItem;
    [JsonProperty("Использовать UI кнопку для выхода из комнаты")]
    public bool UseUIButton;
    [JsonProperty("Раз в сколько сек. обновлять кол-во доступных камней. ( проверяется только тогда, когда игрок пытается/попадает в комнату )")]
    public int UpdateSecond;
    [JsonProperty("Сколько максимум одновременно активных комнат может быть. ( для оптимизации )")]
    public int MaxCountRoom;
    [JsonProperty("Сколько максимум предметов можно взять в комнату")]
    public int MaxCountItem;
    [JsonProperty("Перерыв на телепортацию в комнату сек.")]
    public int CDTPRoom;
    [JsonProperty("Префикс в чате")]
    public string PrefixChat;
    [JsonProperty("Уведомлять игрока когда ему выдали или отобрали доступ к функционалу фарм комнаты")]
    public bool ChatMessages;
    [JsonProperty("SteamID профиля для кастомной аватарки")]
    public ulong SteamID;
    [JsonProperty("Список разрешенных консольных команд в фарм комнате")]
    public List<string> ConsoleCommand;
    [JsonProperty("Список разрешенных чат команд в фарм комнате")]
    public List<string> ChatCommand;
}


```

---

## XFireGloves

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("XFireGloves", "Monster", "1.0.11")]
 class XFireGloves : RustPlugin
{
    private GlovesConfig config;
    private class GlovesConfig
    {
        internal class SettingSetting
        {
            [JsonProperty("Использовать разрешение на шанс найти перчатки в ящиках: xfiregloves.glovesloot")]
            public bool Permission;
        }

        internal class GlovesSetting
        {
            [JsonProperty("SkinID огненных перчаток")]
            public ulong SkinIDGloves;
            [JsonProperty("Имя огненных перчаток")]
            public string NameGloves;
            [JsonProperty("Рейты добываемых ресурсов в перчатках")]
            public float GatherValue;
            [JsonProperty("Рейты подбираемых ресурсов в перчатках")]
            public float PickupValue;
            [JsonProperty("Кол-во радиации при подборе ресурсов")]
            public float RadiationPickupValue;
            [JsonProperty("Кол-во радиации при бонусной добыче")]
            public float RadiationBonusValue;
            [JsonProperty("Список ресурсов после переработки")]
            public Dictionary<string, int> ItemList;
            [JsonProperty("Включить переплавку дерева")]
            public bool SmeltWood;
            [JsonProperty("Включить переплавку добываемых ресурсов")]
            public bool SmeltGather;
            [JsonProperty("Включить переплавку подбираемых ресурсов")]
            public bool SmeltPickup;
            [JsonProperty("Включить рейты добываемых ресурсов")]
            public bool GatherGloves;
            [JsonProperty("Включить рейты подбираемых ресурсов")]
            public bool PickupGloves;
            [JsonProperty("Включить кастомные предметы после переработке огненных перчаток")]
            public bool RecyclerGloves;
            [JsonProperty("Включить выпадение перчаток из ящиков с определенным шансом")]
            public bool CrateGloves;
            [JsonProperty("Включить накопление радиации при подборе ресурсов.")]
            public bool RadiationPickup;
            [JsonProperty("Включить накопление радиации при бонусной добыче.")]
            public bool RadiationBonus;
            [JsonProperty("Настройка шанса выпадения из ящиков и бочек. Имя ящика/бочки | Шанс выпадения: 1.0 - 100%")]
            public Dictionary<string, float> Crate;
            public GlovesSetting(ulong s, string n, float gv, float pv, float rpv, float rbv, Dictionary<string, int> il, bool sw, bool sg, bool sp, bool gg, bool pg, bool rg, bool cg, bool rp, bool rb, Dictionary<string, float> c);
        }

        [JsonProperty("Общее")]
        public SettingSetting Setting;
        [JsonProperty("Список огненных перчаток")]
        public List<GlovesSetting> Gloves;
        public static GlovesConfig GetNewConfiguration();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    [ConsoleCommand("gl_give")]
     void GlovesGive(ConsoleSystem.Arg args);
     void OnServerInitialized();
    private Dictionary<ItemDefinition, ItemDefinition> Smelting;
    public List<string> SmeltingItemList;
     void Smelt();
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     object OnRecycleItem(Recycler recycler, Item item);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
}

private class GlovesConfig
{
    internal class SettingSetting
    {
        [JsonProperty("Использовать разрешение на шанс найти перчатки в ящиках: xfiregloves.glovesloot")]
        public bool Permission;
    }

    internal class GlovesSetting
    {
        [JsonProperty("SkinID огненных перчаток")]
        public ulong SkinIDGloves;
        [JsonProperty("Имя огненных перчаток")]
        public string NameGloves;
        [JsonProperty("Рейты добываемых ресурсов в перчатках")]
        public float GatherValue;
        [JsonProperty("Рейты подбираемых ресурсов в перчатках")]
        public float PickupValue;
        [JsonProperty("Кол-во радиации при подборе ресурсов")]
        public float RadiationPickupValue;
        [JsonProperty("Кол-во радиации при бонусной добыче")]
        public float RadiationBonusValue;
        [JsonProperty("Список ресурсов после переработки")]
        public Dictionary<string, int> ItemList;
        [JsonProperty("Включить переплавку дерева")]
        public bool SmeltWood;
        [JsonProperty("Включить переплавку добываемых ресурсов")]
        public bool SmeltGather;
        [JsonProperty("Включить переплавку подбираемых ресурсов")]
        public bool SmeltPickup;
        [JsonProperty("Включить рейты добываемых ресурсов")]
        public bool GatherGloves;
        [JsonProperty("Включить рейты подбираемых ресурсов")]
        public bool PickupGloves;
        [JsonProperty("Включить кастомные предметы после переработке огненных перчаток")]
        public bool RecyclerGloves;
        [JsonProperty("Включить выпадение перчаток из ящиков с определенным шансом")]
        public bool CrateGloves;
        [JsonProperty("Включить накопление радиации при подборе ресурсов.")]
        public bool RadiationPickup;
        [JsonProperty("Включить накопление радиации при бонусной добыче.")]
        public bool RadiationBonus;
        [JsonProperty("Настройка шанса выпадения из ящиков и бочек. Имя ящика/бочки | Шанс выпадения: 1.0 - 100%")]
        public Dictionary<string, float> Crate;
        public GlovesSetting(ulong s, string n, float gv, float pv, float rpv, float rbv, Dictionary<string, int> il, bool sw, bool sg, bool sp, bool gg, bool pg, bool rg, bool cg, bool rp, bool rb, Dictionary<string, float> c);
    }

    [JsonProperty("Общее")]
    public SettingSetting Setting;
    [JsonProperty("Список огненных перчаток")]
    public List<GlovesSetting> Gloves;
    public static GlovesConfig GetNewConfiguration();
}

internal class SettingSetting
{
    [JsonProperty("Использовать разрешение на шанс найти перчатки в ящиках: xfiregloves.glovesloot")]
    public bool Permission;
}

internal class GlovesSetting
{
    [JsonProperty("SkinID огненных перчаток")]
    public ulong SkinIDGloves;
    [JsonProperty("Имя огненных перчаток")]
    public string NameGloves;
    [JsonProperty("Рейты добываемых ресурсов в перчатках")]
    public float GatherValue;
    [JsonProperty("Рейты подбираемых ресурсов в перчатках")]
    public float PickupValue;
    [JsonProperty("Кол-во радиации при подборе ресурсов")]
    public float RadiationPickupValue;
    [JsonProperty("Кол-во радиации при бонусной добыче")]
    public float RadiationBonusValue;
    [JsonProperty("Список ресурсов после переработки")]
    public Dictionary<string, int> ItemList;
    [JsonProperty("Включить переплавку дерева")]
    public bool SmeltWood;
    [JsonProperty("Включить переплавку добываемых ресурсов")]
    public bool SmeltGather;
    [JsonProperty("Включить переплавку подбираемых ресурсов")]
    public bool SmeltPickup;
    [JsonProperty("Включить рейты добываемых ресурсов")]
    public bool GatherGloves;
    [JsonProperty("Включить рейты подбираемых ресурсов")]
    public bool PickupGloves;
    [JsonProperty("Включить кастомные предметы после переработке огненных перчаток")]
    public bool RecyclerGloves;
    [JsonProperty("Включить выпадение перчаток из ящиков с определенным шансом")]
    public bool CrateGloves;
    [JsonProperty("Включить накопление радиации при подборе ресурсов.")]
    public bool RadiationPickup;
    [JsonProperty("Включить накопление радиации при бонусной добыче.")]
    public bool RadiationBonus;
    [JsonProperty("Настройка шанса выпадения из ящиков и бочек. Имя ящика/бочки | Шанс выпадения: 1.0 - 100%")]
    public Dictionary<string, float> Crate;
    public GlovesSetting(ulong s, string n, float gv, float pv, float rpv, float rbv, Dictionary<string, int> il, bool sw, bool sg, bool sp, bool gg, bool pg, bool rg, bool cg, bool rp, bool rb, Dictionary<string, float> c);
}


```

---

## XHeliHealth

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("XHeliHealth", "Sempai#3239", "1.0.0")]
internal class XHeliHealth : RustPlugin
{
    private void Bradley(BradleyAPC apc);
    protected override void SaveConfig();
    private void OnEntitySpawned(BaseEntity entity);
    private class HealthConfig
    {
        [JsonProperty("Настройка кастомного ХП вертолета")]
        public HeliSetting Heli;
        public static HealthConfig GetNewConfiguration();
        internal class HeliSetting
        {
            [JsonProperty("ХП переднего винта")]
            public int BladeHealth;
            [JsonProperty("ХП заднего винта")]
            public int TailHealth;
            [JsonProperty("Включить кастомное ХП вертолета")]
            public bool HeliHealth;
            [JsonProperty("ХП корпуса вертолета")]
            public int HousingHealth;
        }

        [JsonProperty("Настройка кастомного ХП танка")]
        public BradleySetting Bradley;
        internal class BradleySetting
        {
            [JsonProperty("ХП танка")]
            public int APCHealth;
            [JsonProperty("Включить кастомное ХП танка")]
            public bool BradleyHealth;
        }

    }

    protected override void LoadConfig();
    private void Heli(BaseHelicopter heli);
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
    private HealthConfig config;
}

private class HealthConfig
{
    [JsonProperty("Настройка кастомного ХП вертолета")]
    public HeliSetting Heli;
    public static HealthConfig GetNewConfiguration();
    internal class HeliSetting
    {
        [JsonProperty("ХП переднего винта")]
        public int BladeHealth;
        [JsonProperty("ХП заднего винта")]
        public int TailHealth;
        [JsonProperty("Включить кастомное ХП вертолета")]
        public bool HeliHealth;
        [JsonProperty("ХП корпуса вертолета")]
        public int HousingHealth;
    }

    [JsonProperty("Настройка кастомного ХП танка")]
    public BradleySetting Bradley;
    internal class BradleySetting
    {
        [JsonProperty("ХП танка")]
        public int APCHealth;
        [JsonProperty("Включить кастомное ХП танка")]
        public bool BradleyHealth;
    }

}

internal class HeliSetting
{
    [JsonProperty("ХП переднего винта")]
    public int BladeHealth;
    [JsonProperty("ХП заднего винта")]
    public int TailHealth;
    [JsonProperty("Включить кастомное ХП вертолета")]
    public bool HeliHealth;
    [JsonProperty("ХП корпуса вертолета")]
    public int HousingHealth;
}

internal class BradleySetting
{
    [JsonProperty("ХП танка")]
    public int APCHealth;
    [JsonProperty("Включить кастомное ХП танка")]
    public bool BradleyHealth;
}


```

---

## XHorsePlus

```csharp
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("XHorsePlus", "SkuliDropek", "1.0.1")]
 class XHorsePlus : RustPlugin
{
    private HorseConfig config;
    private class HorseConfig
    {
        internal class HorseSetting
        {
            [JsonProperty("SkinID: 2236993578 - 1, 2236994498 - 2")]
            public ulong HorseSkinID;
            [JsonProperty("Имя")]
            public string HorseName;
        }

        internal class WorkSetting
        {
            [JsonProperty("Количество дополнительных мест. True - 1, False - 2")]
            public bool ChairValue;
            [JsonProperty("Отрисовывать стул")]
            public bool Chair;
        }

        [JsonProperty("Настройки лошади")]
        public HorseSetting Horse;
        [JsonProperty("Общее")]
        public WorkSetting Settings;
        public static HorseConfig GetNewConfiguration();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    [ConsoleCommand("h_give")]
     void cmdConsoleCommand(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private void OnEntitySpawned(RidableHorse horse);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void SpawnEntity(BaseEntity entity);
     void ChairOne(RidableHorse horse, Vector3 position, Vector3 rotation);
     void ChairTwo(RidableHorse horse, Vector3 position, Vector3 rotation);
     RidableHorse.MountPointInfo SpawnPassenger(RidableHorse horse, Vector3 position, Vector3 rotation);
}

private class HorseConfig
{
    internal class HorseSetting
    {
        [JsonProperty("SkinID: 2236993578 - 1, 2236994498 - 2")]
        public ulong HorseSkinID;
        [JsonProperty("Имя")]
        public string HorseName;
    }

    internal class WorkSetting
    {
        [JsonProperty("Количество дополнительных мест. True - 1, False - 2")]
        public bool ChairValue;
        [JsonProperty("Отрисовывать стул")]
        public bool Chair;
    }

    [JsonProperty("Настройки лошади")]
    public HorseSetting Horse;
    [JsonProperty("Общее")]
    public WorkSetting Settings;
    public static HorseConfig GetNewConfiguration();
}

internal class HorseSetting
{
    [JsonProperty("SkinID: 2236993578 - 1, 2236994498 - 2")]
    public ulong HorseSkinID;
    [JsonProperty("Имя")]
    public string HorseName;
}

internal class WorkSetting
{
    [JsonProperty("Количество дополнительных мест. True - 1, False - 2")]
    public bool ChairValue;
    [JsonProperty("Отрисовывать стул")]
    public bool Chair;
}


```

---

## XIcyJackhammer

```csharp
using Newtonsoft.Json;
using UnityEngine;
using System.Linq;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("XIcyJackhammer", "Sempai#3239", "1.0.801")]
 class XIcyJackhammer : RustPlugin
{
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void InitializeLang();
    private Item OnItemSplit(Item item, int amount);
    [PluginReference]
    private Plugin StackSizeController;
    [ConsoleCommand("j_give")]
    private void IceGive(ConsoleSystem.Arg args);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
    private class IceConfig
    {
        internal class JackhammerSetting
        {
            [JsonProperty("SkinID бура")]
            public ulong SkinIDJackhammer;
            [JsonProperty("Имя бура")]
            public string NameJackhammer;
        }

        [JsonProperty("Настройка шанса выпадения ледяного бура в ящиках")]
        public List<Crates> Crate;
        internal class Crates
        {
            [JsonProperty("Имя ящика")]
            public string NameCrate;
            [JsonProperty("Шанс выпадения")]
            public float ChanceDrop;
            public Crates(string namecrate, float chancedrop);
        }

        internal class IceRecycler
        {
            [JsonProperty("Shortname предмета")]
            public string ShortNameItem;
            [JsonProperty("Скин предмета")]
            public ulong SkinIDItem;
            [JsonProperty("Кастомное имя предмета")]
            public string ItemNameItem;
            [JsonProperty("Шанс выпасть при переработки льда. 100.0 - 100%")]
            public float ChanceItem;
            [JsonProperty("Минимальное количество предмета")]
            public int AmountMinItem;
            [JsonProperty("Максимальное количество предмета")]
            public int AmountMaxItem;
            public IceRecycler(string shortnameitem, ulong skiniditem, string itemnameitem, float chanceitem, int amountminitem, int amountmaxitem);
        }

        internal class WorkSetting
        {
            [JsonProperty("Включить выпадение ледяного бура из ящиков с определенным шансом")]
            public bool CrateJackhammer;
            [JsonProperty("Включить добычу глыб льда только в зимнем биоме")]
            public bool GatherIce;
            [JsonProperty("Использовать разрешение на шанс найти ледяной бур в ящиках: xicyjackhammer.jackhammer")]
            public bool PermissionJackhammer;
            [JsonProperty("Использовать разрешение на шанс добыть глыбу льда: xicyjackhammer.ice")]
            public bool PermissionIce;
            [JsonProperty("Ипользовать сообщение в чат при добыче глыбы льда")]
            public bool Message;
        }

        [JsonProperty("Переработка")]
        public List<IceRecycler> Recycler;
        internal class IceSetting
        {
            [JsonProperty("SkinID льда")]
            public ulong SkinIDIce;
            [JsonProperty("Имя льда")]
            public string NameIce;
        }

        [JsonProperty("Добыча льда")]
        public List<IceGather> Gather;
        internal class IceGather
        {
            [JsonProperty("Ресурс вместе с каким будет выпадать лед")]
            public string TypeGather;
            [JsonProperty("Шанс выпасть льда при бонусной добыче")]
            public float ChanceGather;
            [JsonProperty("Минимальное количество льда")]
            public int AmountMinGather;
            [JsonProperty("Максимальное количество льда")]
            public int AmountMaxGather;
            public IceGather(string typegather, float chancegather, int amountmingather, int amountmaxgather);
        }

        [JsonProperty("Ледяной бур")]
        public JackhammerSetting Jackhammer;
        [JsonProperty("Общее")]
        public WorkSetting Settings;
        [JsonProperty("Глыба льда")]
        public IceSetting Ice;
        public static IceConfig GetNewConfiguration();
    }

    private void OnServerInitialized();
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private IceConfig config;
    protected override void SaveConfig();
     object OnRecycleItem(Recycler recycler, Item item);
    private void GiveIce(BasePlayer player, int amount);
}

private class IceConfig
{
    internal class JackhammerSetting
    {
        [JsonProperty("SkinID бура")]
        public ulong SkinIDJackhammer;
        [JsonProperty("Имя бура")]
        public string NameJackhammer;
    }

    [JsonProperty("Настройка шанса выпадения ледяного бура в ящиках")]
    public List<Crates> Crate;
    internal class Crates
    {
        [JsonProperty("Имя ящика")]
        public string NameCrate;
        [JsonProperty("Шанс выпадения")]
        public float ChanceDrop;
        public Crates(string namecrate, float chancedrop);
    }

    internal class IceRecycler
    {
        [JsonProperty("Shortname предмета")]
        public string ShortNameItem;
        [JsonProperty("Скин предмета")]
        public ulong SkinIDItem;
        [JsonProperty("Кастомное имя предмета")]
        public string ItemNameItem;
        [JsonProperty("Шанс выпасть при переработки льда. 100.0 - 100%")]
        public float ChanceItem;
        [JsonProperty("Минимальное количество предмета")]
        public int AmountMinItem;
        [JsonProperty("Максимальное количество предмета")]
        public int AmountMaxItem;
        public IceRecycler(string shortnameitem, ulong skiniditem, string itemnameitem, float chanceitem, int amountminitem, int amountmaxitem);
    }

    internal class WorkSetting
    {
        [JsonProperty("Включить выпадение ледяного бура из ящиков с определенным шансом")]
        public bool CrateJackhammer;
        [JsonProperty("Включить добычу глыб льда только в зимнем биоме")]
        public bool GatherIce;
        [JsonProperty("Использовать разрешение на шанс найти ледяной бур в ящиках: xicyjackhammer.jackhammer")]
        public bool PermissionJackhammer;
        [JsonProperty("Использовать разрешение на шанс добыть глыбу льда: xicyjackhammer.ice")]
        public bool PermissionIce;
        [JsonProperty("Ипользовать сообщение в чат при добыче глыбы льда")]
        public bool Message;
    }

    [JsonProperty("Переработка")]
    public List<IceRecycler> Recycler;
    internal class IceSetting
    {
        [JsonProperty("SkinID льда")]
        public ulong SkinIDIce;
        [JsonProperty("Имя льда")]
        public string NameIce;
    }

    [JsonProperty("Добыча льда")]
    public List<IceGather> Gather;
    internal class IceGather
    {
        [JsonProperty("Ресурс вместе с каким будет выпадать лед")]
        public string TypeGather;
        [JsonProperty("Шанс выпасть льда при бонусной добыче")]
        public float ChanceGather;
        [JsonProperty("Минимальное количество льда")]
        public int AmountMinGather;
        [JsonProperty("Максимальное количество льда")]
        public int AmountMaxGather;
        public IceGather(string typegather, float chancegather, int amountmingather, int amountmaxgather);
    }

    [JsonProperty("Ледяной бур")]
    public JackhammerSetting Jackhammer;
    [JsonProperty("Общее")]
    public WorkSetting Settings;
    [JsonProperty("Глыба льда")]
    public IceSetting Ice;
    public static IceConfig GetNewConfiguration();
}

internal class JackhammerSetting
{
    [JsonProperty("SkinID бура")]
    public ulong SkinIDJackhammer;
    [JsonProperty("Имя бура")]
    public string NameJackhammer;
}

internal class Crates
{
    [JsonProperty("Имя ящика")]
    public string NameCrate;
    [JsonProperty("Шанс выпадения")]
    public float ChanceDrop;
    public Crates(string namecrate, float chancedrop);
}

internal class IceRecycler
{
    [JsonProperty("Shortname предмета")]
    public string ShortNameItem;
    [JsonProperty("Скин предмета")]
    public ulong SkinIDItem;
    [JsonProperty("Кастомное имя предмета")]
    public string ItemNameItem;
    [JsonProperty("Шанс выпасть при переработки льда. 100.0 - 100%")]
    public float ChanceItem;
    [JsonProperty("Минимальное количество предмета")]
    public int AmountMinItem;
    [JsonProperty("Максимальное количество предмета")]
    public int AmountMaxItem;
    public IceRecycler(string shortnameitem, ulong skiniditem, string itemnameitem, float chanceitem, int amountminitem, int amountmaxitem);
}

internal class WorkSetting
{
    [JsonProperty("Включить выпадение ледяного бура из ящиков с определенным шансом")]
    public bool CrateJackhammer;
    [JsonProperty("Включить добычу глыб льда только в зимнем биоме")]
    public bool GatherIce;
    [JsonProperty("Использовать разрешение на шанс найти ледяной бур в ящиках: xicyjackhammer.jackhammer")]
    public bool PermissionJackhammer;
    [JsonProperty("Использовать разрешение на шанс добыть глыбу льда: xicyjackhammer.ice")]
    public bool PermissionIce;
    [JsonProperty("Ипользовать сообщение в чат при добыче глыбы льда")]
    public bool Message;
}

internal class IceSetting
{
    [JsonProperty("SkinID льда")]
    public ulong SkinIDIce;
    [JsonProperty("Имя льда")]
    public string NameIce;
}

internal class IceGather
{
    [JsonProperty("Ресурс вместе с каким будет выпадать лед")]
    public string TypeGather;
    [JsonProperty("Шанс выпасть льда при бонусной добыче")]
    public float ChanceGather;
    [JsonProperty("Минимальное количество льда")]
    public int AmountMinGather;
    [JsonProperty("Максимальное количество льда")]
    public int AmountMaxGather;
    public IceGather(string typegather, float chancegather, int amountmingather, int amountmaxgather);
}


```

---

## XInfoMenu

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("XInfoMenu", "Monster.", "1.0.3")]
 class XInfoMenu : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private InfoMenuConfig config;
    private class InfoMenuConfig
    {
        internal class GeneralSetting
        {
            [JsonProperty("Открывать меню когда игрок зашел на сервер")]
            public bool ConnectOpen;
            [JsonProperty("Перезагружать картинки после перезагрузки плагина")]
            public bool ReloadImage;
            [JsonProperty("Отображать кнопки переключения страниц если в категории только одна страница")]
            public bool ButtonNext;
            [JsonProperty("КД действий с меню")]
            public float Cooldowns;
        }

        internal class MessageSetting
        {
            [JsonProperty("Список уникальных имен сообщений - [ Настройка текста в lang ]")]
            public List<string> ListMessage;
            [JsonProperty("Интервал сообщений в чат")]
            public float TimeMessage;
            [JsonProperty("SteamID профиля для кастомной аватарки")]
            public ulong SteamID;
            [JsonProperty("Включить сообщения в чат")]
            public bool MessageUse;
        }

        internal class PanelMessageSetting
        {
            [JsonProperty("Список уникальных имен сообщений - [ Настройка текста в lang ]")]
            public List<string> ListMessage;
            [JsonProperty("Интервал сообщений под слотами")]
            public float TimeMessage;
            [JsonProperty("Включить сообщения под слотами")]
            public bool MessageUse;
        }

        internal class GUISetting
        {
            [JsonProperty("Цвет фона_1")]
            public string ColorBackgroundO;
            [JsonProperty("Цвет фона_2")]
            public string ColorBackgroundT;
            [JsonProperty("Материал фона_1")]
            public string MaterialBackgroundO;
            [JsonProperty("Материал фона_2")]
            public string MaterialBackgroundT;
            [JsonProperty("Цвет кнопок")]
            public string ColorButton;
            [JsonProperty("Цвет активной кнопки")]
            public string ColorAButton;
        }

        internal class ElementSetting
        {
            [JsonProperty("Цвет элемента")]
            public string ColorElement;
            [JsonProperty("Материал элемента")]
            public string MaterialElement;
            [JsonProperty("OffsetMin")]
            public string OffsetMinElement;
            [JsonProperty("OffsetMax")]
            public string OffsetMaxElement;
            public ElementSetting(string color, string material, string omin, string omax);
        }

        internal class ImageSetting
        {
            [JsonProperty("Уникальное имя картинки")]
            public string NameImage;
            [JsonProperty("Ссылка на картинку")]
            public string URLImage;
            [JsonProperty("OffsetMin")]
            public string OffsetMinImage;
            [JsonProperty("OffsetMax")]
            public string OffsetMaxImage;
            public ImageSetting(string nameimage, string urlimage, string omin, string omax);
        }

        internal class TextSetting
        {
            [JsonProperty("Уникальное имя текста блока - [ Настройка текста в lang ]")]
            public string NameText;
            [JsonProperty("OffsetMin")]
            public string OffsetMinText;
            [JsonProperty("OffsetMax")]
            public string OffsetMaxText;
            [JsonProperty("TextAnchor [ Выравнивание текста ] | 0 - 8")]
            public int TextAnchor;
            [JsonProperty("Цвет текста")]
            public string TextColor;
            public TextSetting(string nametext, string omin, string omax, int ta, string tcolor);
        }

        internal class ButtonSetting
        {
            [JsonProperty("Уникальное имя кнопки - [ Настройка текста в lang ]")]
            public string NameButton;
            [JsonProperty("OffsetMin")]
            public string OffsetMinButton;
            [JsonProperty("OffsetMax")]
            public string OffsetMaxButton;
            [JsonProperty("TextAnchor [ Выравнивание текста ] | 0 - 8")]
            public int ButtonAnchor;
            [JsonProperty("Команда")]
            public string ButtonCommand;
            [JsonProperty("Цвет кнопки")]
            public string ButtonColor;
            [JsonProperty("Цвет текста")]
            public string TextColor;
            public ButtonSetting(string namebutton, string omin, string omax, int ba, string bc, string bcolor, string tcolor);
        }

        internal class IMenuSetting
        {
            [JsonProperty("Настройка элементов")]
            public List<ElementSetting> Element;
            [JsonProperty("Настройка картинки")]
            public List<ImageSetting> Image;
            [JsonProperty("Настройка блока текста")]
            public List<TextSetting> Text;
            [JsonProperty("Настройка кнопок")]
            public List<ButtonSetting> Button;
            public IMenuSetting(List<ElementSetting> element, List<ImageSetting> image, List<TextSetting> text, List<ButtonSetting> button);
        }

        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Настройка сообщений в чат")]
        public MessageSetting Message;
        [JsonProperty("Настройка сообщений под слотами")]
        public PanelMessageSetting PanelMessage;
        [JsonProperty("Настройки GUI")]
        public GUISetting GUI;
        [JsonProperty("Настройка кнопок и страниц")]
        public Dictionary<string, List<IMenuSetting>> IMenu;
        public static InfoMenuConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<BasePlayer, DateTime> Cooldowns;
    [ChatCommand("help")]
    private void cmdOpenGUI(BasePlayer player);
    [ChatCommand("info")]
    private void cmdOpenGUII(BasePlayer player);
    [ConsoleCommand("page")]
    private void ccmdPage(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private void Unload();
    private void Broadcast();
    private void PanelBroadcast();
    private void OnPlayerConnected(BasePlayer player);
    private void GUIBackground(BasePlayer player);
    private void GUIPage(BasePlayer player, int page, int Page);
    private void GUIPageNext(BasePlayer player, int page, int Page);
    private void GUIButton(BasePlayer player, int page);
    private void PanelMGUI(BasePlayer player);
     void InitializeLang();
}

private class InfoMenuConfig
{
    internal class GeneralSetting
    {
        [JsonProperty("Открывать меню когда игрок зашел на сервер")]
        public bool ConnectOpen;
        [JsonProperty("Перезагружать картинки после перезагрузки плагина")]
        public bool ReloadImage;
        [JsonProperty("Отображать кнопки переключения страниц если в категории только одна страница")]
        public bool ButtonNext;
        [JsonProperty("КД действий с меню")]
        public float Cooldowns;
    }

    internal class MessageSetting
    {
        [JsonProperty("Список уникальных имен сообщений - [ Настройка текста в lang ]")]
        public List<string> ListMessage;
        [JsonProperty("Интервал сообщений в чат")]
        public float TimeMessage;
        [JsonProperty("SteamID профиля для кастомной аватарки")]
        public ulong SteamID;
        [JsonProperty("Включить сообщения в чат")]
        public bool MessageUse;
    }

    internal class PanelMessageSetting
    {
        [JsonProperty("Список уникальных имен сообщений - [ Настройка текста в lang ]")]
        public List<string> ListMessage;
        [JsonProperty("Интервал сообщений под слотами")]
        public float TimeMessage;
        [JsonProperty("Включить сообщения под слотами")]
        public bool MessageUse;
    }

    internal class GUISetting
    {
        [JsonProperty("Цвет фона_1")]
        public string ColorBackgroundO;
        [JsonProperty("Цвет фона_2")]
        public string ColorBackgroundT;
        [JsonProperty("Материал фона_1")]
        public string MaterialBackgroundO;
        [JsonProperty("Материал фона_2")]
        public string MaterialBackgroundT;
        [JsonProperty("Цвет кнопок")]
        public string ColorButton;
        [JsonProperty("Цвет активной кнопки")]
        public string ColorAButton;
    }

    internal class ElementSetting
    {
        [JsonProperty("Цвет элемента")]
        public string ColorElement;
        [JsonProperty("Материал элемента")]
        public string MaterialElement;
        [JsonProperty("OffsetMin")]
        public string OffsetMinElement;
        [JsonProperty("OffsetMax")]
        public string OffsetMaxElement;
        public ElementSetting(string color, string material, string omin, string omax);
    }

    internal class ImageSetting
    {
        [JsonProperty("Уникальное имя картинки")]
        public string NameImage;
        [JsonProperty("Ссылка на картинку")]
        public string URLImage;
        [JsonProperty("OffsetMin")]
        public string OffsetMinImage;
        [JsonProperty("OffsetMax")]
        public string OffsetMaxImage;
        public ImageSetting(string nameimage, string urlimage, string omin, string omax);
    }

    internal class TextSetting
    {
        [JsonProperty("Уникальное имя текста блока - [ Настройка текста в lang ]")]
        public string NameText;
        [JsonProperty("OffsetMin")]
        public string OffsetMinText;
        [JsonProperty("OffsetMax")]
        public string OffsetMaxText;
        [JsonProperty("TextAnchor [ Выравнивание текста ] | 0 - 8")]
        public int TextAnchor;
        [JsonProperty("Цвет текста")]
        public string TextColor;
        public TextSetting(string nametext, string omin, string omax, int ta, string tcolor);
    }

    internal class ButtonSetting
    {
        [JsonProperty("Уникальное имя кнопки - [ Настройка текста в lang ]")]
        public string NameButton;
        [JsonProperty("OffsetMin")]
        public string OffsetMinButton;
        [JsonProperty("OffsetMax")]
        public string OffsetMaxButton;
        [JsonProperty("TextAnchor [ Выравнивание текста ] | 0 - 8")]
        public int ButtonAnchor;
        [JsonProperty("Команда")]
        public string ButtonCommand;
        [JsonProperty("Цвет кнопки")]
        public string ButtonColor;
        [JsonProperty("Цвет текста")]
        public string TextColor;
        public ButtonSetting(string namebutton, string omin, string omax, int ba, string bc, string bcolor, string tcolor);
    }

    internal class IMenuSetting
    {
        [JsonProperty("Настройка элементов")]
        public List<ElementSetting> Element;
        [JsonProperty("Настройка картинки")]
        public List<ImageSetting> Image;
        [JsonProperty("Настройка блока текста")]
        public List<TextSetting> Text;
        [JsonProperty("Настройка кнопок")]
        public List<ButtonSetting> Button;
        public IMenuSetting(List<ElementSetting> element, List<ImageSetting> image, List<TextSetting> text, List<ButtonSetting> button);
    }

    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Настройка сообщений в чат")]
    public MessageSetting Message;
    [JsonProperty("Настройка сообщений под слотами")]
    public PanelMessageSetting PanelMessage;
    [JsonProperty("Настройки GUI")]
    public GUISetting GUI;
    [JsonProperty("Настройка кнопок и страниц")]
    public Dictionary<string, List<IMenuSetting>> IMenu;
    public static InfoMenuConfig GetNewConfiguration();
}

internal class GeneralSetting
{
    [JsonProperty("Открывать меню когда игрок зашел на сервер")]
    public bool ConnectOpen;
    [JsonProperty("Перезагружать картинки после перезагрузки плагина")]
    public bool ReloadImage;
    [JsonProperty("Отображать кнопки переключения страниц если в категории только одна страница")]
    public bool ButtonNext;
    [JsonProperty("КД действий с меню")]
    public float Cooldowns;
}

internal class MessageSetting
{
    [JsonProperty("Список уникальных имен сообщений - [ Настройка текста в lang ]")]
    public List<string> ListMessage;
    [JsonProperty("Интервал сообщений в чат")]
    public float TimeMessage;
    [JsonProperty("SteamID профиля для кастомной аватарки")]
    public ulong SteamID;
    [JsonProperty("Включить сообщения в чат")]
    public bool MessageUse;
}

internal class PanelMessageSetting
{
    [JsonProperty("Список уникальных имен сообщений - [ Настройка текста в lang ]")]
    public List<string> ListMessage;
    [JsonProperty("Интервал сообщений под слотами")]
    public float TimeMessage;
    [JsonProperty("Включить сообщения под слотами")]
    public bool MessageUse;
}

internal class GUISetting
{
    [JsonProperty("Цвет фона_1")]
    public string ColorBackgroundO;
    [JsonProperty("Цвет фона_2")]
    public string ColorBackgroundT;
    [JsonProperty("Материал фона_1")]
    public string MaterialBackgroundO;
    [JsonProperty("Материал фона_2")]
    public string MaterialBackgroundT;
    [JsonProperty("Цвет кнопок")]
    public string ColorButton;
    [JsonProperty("Цвет активной кнопки")]
    public string ColorAButton;
}

internal class ElementSetting
{
    [JsonProperty("Цвет элемента")]
    public string ColorElement;
    [JsonProperty("Материал элемента")]
    public string MaterialElement;
    [JsonProperty("OffsetMin")]
    public string OffsetMinElement;
    [JsonProperty("OffsetMax")]
    public string OffsetMaxElement;
    public ElementSetting(string color, string material, string omin, string omax);
}

internal class ImageSetting
{
    [JsonProperty("Уникальное имя картинки")]
    public string NameImage;
    [JsonProperty("Ссылка на картинку")]
    public string URLImage;
    [JsonProperty("OffsetMin")]
    public string OffsetMinImage;
    [JsonProperty("OffsetMax")]
    public string OffsetMaxImage;
    public ImageSetting(string nameimage, string urlimage, string omin, string omax);
}

internal class TextSetting
{
    [JsonProperty("Уникальное имя текста блока - [ Настройка текста в lang ]")]
    public string NameText;
    [JsonProperty("OffsetMin")]
    public string OffsetMinText;
    [JsonProperty("OffsetMax")]
    public string OffsetMaxText;
    [JsonProperty("TextAnchor [ Выравнивание текста ] | 0 - 8")]
    public int TextAnchor;
    [JsonProperty("Цвет текста")]
    public string TextColor;
    public TextSetting(string nametext, string omin, string omax, int ta, string tcolor);
}

internal class ButtonSetting
{
    [JsonProperty("Уникальное имя кнопки - [ Настройка текста в lang ]")]
    public string NameButton;
    [JsonProperty("OffsetMin")]
    public string OffsetMinButton;
    [JsonProperty("OffsetMax")]
    public string OffsetMaxButton;
    [JsonProperty("TextAnchor [ Выравнивание текста ] | 0 - 8")]
    public int ButtonAnchor;
    [JsonProperty("Команда")]
    public string ButtonCommand;
    [JsonProperty("Цвет кнопки")]
    public string ButtonColor;
    [JsonProperty("Цвет текста")]
    public string TextColor;
    public ButtonSetting(string namebutton, string omin, string omax, int ba, string bc, string bcolor, string tcolor);
}

internal class IMenuSetting
{
    [JsonProperty("Настройка элементов")]
    public List<ElementSetting> Element;
    [JsonProperty("Настройка картинки")]
    public List<ImageSetting> Image;
    [JsonProperty("Настройка блока текста")]
    public List<TextSetting> Text;
    [JsonProperty("Настройка кнопок")]
    public List<ButtonSetting> Button;
    public IMenuSetting(List<ElementSetting> element, List<ImageSetting> image, List<TextSetting> text, List<ButtonSetting> button);
}


```

---

## XInfoMenuLITE

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;

Oxide.Plugins
[Info("XInfoMenuLITE", "Sempai#3239", "1.0.0")]
 class XInfoMenuLITE : RustPlugin
{
    private InfoMenuConfig config;
    private class InfoMenuConfig
    {
        [JsonProperty("Кнопки и сообщения")]
        public Dictionary<string, string> Message;
        public static InfoMenuConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("help")]
    private void cmdOpenGUI(BasePlayer player);
    [ChatCommand("info")]
    private void cmdOpenGUII(BasePlayer player);
    [ConsoleCommand("info_page")]
    private void ccmdPage(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private void GUI(BasePlayer player, int page);
}

private class InfoMenuConfig
{
    [JsonProperty("Кнопки и сообщения")]
    public Dictionary<string, string> Message;
    public static InfoMenuConfig GetNewConfiguration();
}


```

---

## XL96Plus

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("XL96Plus", "Я", "1.0.1")]
 class XL96Plus : RustPlugin
{
    private L96Config config;
    private class L96Config
    {
        internal class L96Setting
        {
            [JsonProperty("SkinID улучшенной винтовки")]
            public ulong SkinIDL96;
            [JsonProperty("Имя улучшенной винтовки")]
            public string NameL96;
        }

        internal class SettingsSetting
        {
            [JsonProperty("Убивать игрока при попадании в голову")]
            public bool KillL96;
            [JsonProperty("Оставлять 1-хп при попадании в в тело")]
            public bool HealL96;
            [JsonProperty("Включить выпадение улучшенной винтовки из ящиков с определенным шансом")]
            public bool CrateL96;
            [JsonProperty("Сбивать регенерацию")]
            public bool PendingHealth;
        }

        internal class Crates
        {
            [JsonProperty("Имя ящика")]
            public string NameCrate;
            [JsonProperty("Шанс выпадения")]
            public int ChanceDrop;
            [JsonProperty("Износ улучшенной винтовки. 60.0 - 100%")]
            public float MConditionL96;
            [JsonProperty("ХП улучшенной винтовки. 60.0 - 100%")]
            public float ConditionL96;
        }

        [JsonProperty("Настройка улучшенной винтовки")]
        public L96Setting L96;
        [JsonProperty("Общее")]
        public SettingsSetting Settings;
        [JsonProperty("Настройка шанса выпадения в ящиках")]
        public List<Crates> Crate;
        public static L96Config GetNewConfiguration();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    [ConsoleCommand("w_give")]
     void GlovesGive(ConsoleSystem.Arg args);
    private void OnServerInitialized();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnLootSpawn(LootContainer lootContainer);
}

private class L96Config
{
    internal class L96Setting
    {
        [JsonProperty("SkinID улучшенной винтовки")]
        public ulong SkinIDL96;
        [JsonProperty("Имя улучшенной винтовки")]
        public string NameL96;
    }

    internal class SettingsSetting
    {
        [JsonProperty("Убивать игрока при попадании в голову")]
        public bool KillL96;
        [JsonProperty("Оставлять 1-хп при попадании в в тело")]
        public bool HealL96;
        [JsonProperty("Включить выпадение улучшенной винтовки из ящиков с определенным шансом")]
        public bool CrateL96;
        [JsonProperty("Сбивать регенерацию")]
        public bool PendingHealth;
    }

    internal class Crates
    {
        [JsonProperty("Имя ящика")]
        public string NameCrate;
        [JsonProperty("Шанс выпадения")]
        public int ChanceDrop;
        [JsonProperty("Износ улучшенной винтовки. 60.0 - 100%")]
        public float MConditionL96;
        [JsonProperty("ХП улучшенной винтовки. 60.0 - 100%")]
        public float ConditionL96;
    }

    [JsonProperty("Настройка улучшенной винтовки")]
    public L96Setting L96;
    [JsonProperty("Общее")]
    public SettingsSetting Settings;
    [JsonProperty("Настройка шанса выпадения в ящиках")]
    public List<Crates> Crate;
    public static L96Config GetNewConfiguration();
}

internal class L96Setting
{
    [JsonProperty("SkinID улучшенной винтовки")]
    public ulong SkinIDL96;
    [JsonProperty("Имя улучшенной винтовки")]
    public string NameL96;
}

internal class SettingsSetting
{
    [JsonProperty("Убивать игрока при попадании в голову")]
    public bool KillL96;
    [JsonProperty("Оставлять 1-хп при попадании в в тело")]
    public bool HealL96;
    [JsonProperty("Включить выпадение улучшенной винтовки из ящиков с определенным шансом")]
    public bool CrateL96;
    [JsonProperty("Сбивать регенерацию")]
    public bool PendingHealth;
}

internal class Crates
{
    [JsonProperty("Имя ящика")]
    public string NameCrate;
    [JsonProperty("Шанс выпадения")]
    public int ChanceDrop;
    [JsonProperty("Износ улучшенной винтовки. 60.0 - 100%")]
    public float MConditionL96;
    [JsonProperty("ХП улучшенной винтовки. 60.0 - 100%")]
    public float ConditionL96;
}


```

---

## XLevels

```csharp
using UnityEngine;
using Oxide.Core;
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using ConVar;
using Oxide.Core.Libraries.Covalence;
using Newtonsoft.Json;
using System.Linq;
using System;

Oxide.Plugins
[Info("XLevels", "MONSTER", "1.1.12")]
 class XLevels : RustPlugin
{
    private void Message(BasePlayer player, string Messages);
    private void OnPluginUnloaded(Plugin name);
    private void OnPluginLoaded(Plugin name);
    [ConsoleCommand("level")]
    private void ccmdOpenMenu(ConsoleSystem.Arg arg);
    private void OnLootEntityEnd(BasePlayer player, NPCVendingMachine machine);
    [ConsoleCommand("x_levels")]
    private void ccmdMenuOpen(ConsoleSystem.Arg arg);
    private void RewardMenuFon(BasePlayer player);
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void Info(BasePlayer player);
    private string API_GetPlayerPrefix(BasePlayer player);
    private object OnBetterChat(Dictionary<string, object> chat);
    private class LevelConfig
    {
        [JsonProperty(LanguageEnglish ? "Coupons for XP" : "Купоны на ХР")]
        public List<Coupons> Coupon;
        [JsonProperty(LanguageEnglish ? "VIP level reward" : "Вип награда за уровни")]
        public Dictionary<int, Awards> AwardVip;
        internal class OnlineSetting
        {
            [JsonProperty(LanguageEnglish ? "Enable issuing XP to online players" : "Включить выдачу XP онлайн игрокам")]
            public bool OnlineXP;
            [JsonProperty(LanguageEnglish ? "XP issue interval (in sec.)" : "Интервал выдачи XP (в сек.)")]
            public float TimeXP;
            [JsonProperty(LanguageEnglish ? "Setting up permissions. [ Permission | XP ]" : "Настройка пермишенов. [ Пермишен | XP ]")]
            public Dictionary<string, float> Permisssion;
        }

        [JsonProperty(LanguageEnglish ? "Issuing XP for online" : "Выдача XP за онлайн")]
        public OnlineSetting Online;
        internal class GUISetting
        {
            [JsonProperty("AnchorMin")]
            public string AnchorMin;
            [JsonProperty("AnchorMax")]
            public string AnchorMax;
            [JsonProperty("OffsetMin")]
            public string OffsetMin;
            [JsonProperty("OffsetMax")]
            public string OffsetMax;
            [JsonProperty(LanguageEnglish ? "Show mini-bar" : "Отображать мини-панель")]
            public bool EnablePanel;
            [JsonProperty(LanguageEnglish ? "Color background_1" : "Цвет фона_1")]
            public string ColorBackgroundO;
            [JsonProperty(LanguageEnglish ? "Color background_2" : "Цвет фона_2")]
            public string ColorBackgroundT;
            [JsonProperty(LanguageEnglish ? "Display reward container - [ True - Only when there is a reward in the level | False - Always ]" : "Отображать контейнер наград - [ True - Только когда есть награда на уровне | False - Всегда ]")]
            public bool ContainerReward;
            [JsonProperty(LanguageEnglish ? "Display required reward level" : "Отображать требуемый уровень наград")]
            public bool RewardNumber;
            [JsonProperty(LanguageEnglish ? "Display required VIP reward level" : "Отображать требуемый уровень ВИП наград")]
            public bool RewardNumberVIP;
        }

        [JsonProperty(LanguageEnglish ? "XP multiplier" : "Множитель XP")]
        public XPRateSetting XPRate;
        [JsonProperty(LanguageEnglish ? "Vendings settings" : "Настройки магазинов")]
        public VendingSetting Vending;
        internal class XPSetting
        {
            [JsonProperty(LanguageEnglish ? "XP for the pickup of resources" : "ХР за подбор ресурсов")]
            public Dictionary<string, float> PickupXP;
            [JsonProperty(LanguageEnglish ? "XP for harvest" : "ХР за сбор урожая")]
            public Dictionary<string, float> HarvestXP;
            [JsonProperty(LanguageEnglish ? "XP for bonus resources" : "ХР за бонусную добычу")]
            public Dictionary<string, float> GatherBonusXP;
            [JsonProperty(LanguageEnglish ? "XP for kill / destroy barrel" : "ХР за убийство / разбитие бочек")]
            public Dictionary<string, float> KillXP;
            [JsonProperty(LanguageEnglish ? "XP for opening crates" : "ХР за открытие ящиков")]
            public Dictionary<string, float> CrateXP;
        }

        [JsonProperty(LanguageEnglish ? "Settings levels" : "Настройка уровней")]
        public LevelSetting Level;
        internal class VendingSetting
        {
            [JsonProperty(LanguageEnglish ? "Open the level menu. [ True - Immediately after the opening of the NPC shop | False - UI button ]" : "Открывать меню уровней. [ True - Сразу после открытия НПС магазина | False - Через UI кнопку ]")]
            public bool VendingOpen;
            [JsonProperty(LanguageEnglish ? "Access to the level menu is only through the NPC shops. [ True - NPC shop | False - Command ]" : "Доступ к меню уровней только через магазины НПС. [ True - Магазины НПС | False - Команда ]")]
            public bool VendingUse;
            [JsonProperty(LanguageEnglish ? "List of NPC shops in which you can open the level menu (shop name)" : "Список магазинов НПС в которых можно открыть меню уровней (Имя магазина)")]
            public List<string> ListNPCVending;
        }

        internal class Coupons
        {
            [JsonProperty(LanguageEnglish ? "Coupon name" : "Имя купона")]
            public string Name;
            [JsonProperty(LanguageEnglish ? "Coupon text" : "Текст купона")]
            public string Text;
            [JsonProperty(LanguageEnglish ? "Coupon skin" : "Скин купона")]
            public ulong SkinID;
            [JsonProperty(LanguageEnglish ? "XP amount" : "Кол-во ХР")]
            public int XP;
            [JsonProperty(LanguageEnglish ? "Setting the chance of falling out of crates/barrels" : "Настройка шанса выпадения из ящиков/бочек")]
            public List<Crates> Crate;
            public Coupons(string name, string text, ulong skinid, int xp, List<Crates> crates);
        }

        [JsonProperty(LanguageEnglish ? "General settings" : "Общие настройки")]
        public GeneralSetting Setting;
        internal class GeneralSetting
        {
            [JsonProperty(LanguageEnglish ? "List of commands to open the menu" : "Список команд для открытия меню")]
            public List<string> CommandList;
            [JsonProperty(LanguageEnglish ? "XP for the pickup of resources" : "Зачислять ХР за подбор ресурсов")]
            public bool PickupValide;
            [JsonProperty(LanguageEnglish ? "Display the level in prefix" : "Отображать уровень в префиксе")]
            public bool ChatPrefixLevel;
            [JsonProperty(LanguageEnglish ? "XP for opening crates" : "Зачислять ХР за открытие ящиков")]
            public bool CrateValide;
            [JsonProperty(LanguageEnglish ? "List of available ranks - [ Level - Rank ] ( If the list is empty, then the rank will not be displayed in the menu )" : "Список доступных рангов - [ Уровень - Ранг ] ( Если список пуст, то ранг не будет отображаться в меню )")]
            public Dictionary<int, string> RankList;
            [JsonProperty(LanguageEnglish ? "XP for kill" : "Зачислять ХР за убийство")]
            public bool KillValide;
            [JsonProperty(LanguageEnglish ? "Display the rank in prefix" : "Отображать ранг в префиксе")]
            public bool ChatPrefixRank;
            [JsonProperty(LanguageEnglish ? "Include messages of received rewards in chat" : "Включить сообщения полученных  наград в чат")]
            public bool ChatTakeMessages;
            [JsonProperty(LanguageEnglish ? "Enable prefix in chat - [ Set to False if the prefix should be disabled or the prefix is used by a third party chat plugin ]" : "Включить префикс в чате - [ Установите False, если префикс нужно отключить или префикс используется сторонним плагином для чата ]")]
            public bool ChatPrefix;
            [JsonProperty(LanguageEnglish ? "There is a plugin for custom loot" : "Есть плагин на кастомный лут")]
            public bool PluginCLoot;
            [JsonProperty(LanguageEnglish ? "XP for harvest" : "Зачислять ХР за сбор урожая")]
            public bool HarvestValide;
            [JsonProperty(LanguageEnglish ? "Reset the level and XP of the player after reaching the maximum level - [ Players will re-open levels and receive rewards ]" : "Обнулять уровень и ХР игрока после достижения максимального уровня - [ Игроки повторно будет открывать уровни и получать награды ]")]
            public bool ClearAll;
            [JsonProperty(LanguageEnglish ? "Profile SteamID for custom avatar" : "SteamID профиля для кастомной аватарки")]
            public ulong SteamID;
            [JsonProperty(LanguageEnglish ? "Take VIP Reward - [ True - take only with permission | False - take at any time without permission ]" : "Забрать ВИП награду - [ True - забрать только с пермишеном | False - забрать в любое время без пермишена ]")]
            public bool TakeReward;
            [JsonProperty(LanguageEnglish ? "Get VIP reward - [ True - only with permission | False - without permission ]" : "Получить ВИП награду - [ True - только с пермишеном | False - без пермишена ]")]
            public bool MoveReward;
            [JsonProperty(LanguageEnglish ? "Enable VIP rewards" : "Включить ВИП награды")]
            public bool VIPValide;
            [JsonProperty(LanguageEnglish ? "XP for bonus resources" : "Зачислять ХР за бонусные ресурсы")]
            public bool BonusValide;
            [JsonProperty(LanguageEnglish ? "Include level up messages in chat" : "Включить сообщения повышения уровня в чат")]
            public bool ChatLevelMessages;
            [JsonProperty(LanguageEnglish ? "Exchange coupons if you have already reached the maximum level - [ Suitable for top players ]" : "Обменивать купоны если уже достигнут максимальный уровень - [ Подходит для топа игроков ]")]
            public bool GiveXPCoupon;
            [JsonProperty(LanguageEnglish ? "Add XP if the maximum level is already reached - [ Suitable for top players ]" : "Засчитывать ХР если уже достигнут максимальный уровень - [ Подходит для топа игроков ]")]
            public bool GiveXP;
            [JsonProperty(LanguageEnglish ? "Enable coupons" : "Включить купоны")]
            public bool CouponsValide;
        }

        [JsonProperty(LanguageEnglish ? "XP settings | Shortname : ValueXP" : "Настройка ХР | Shortname : ValueXP")]
        public XPSetting XP;
        [JsonProperty(LanguageEnglish ? "Level reward" : "Награда за уровни")]
        public Dictionary<int, Awards> Award;
        public static LevelConfig GetNewConfiguration();
        internal class Crates
        {
            [JsonProperty(LanguageEnglish ? "Name crate/barrel" : "Имя ящика/бочки")]
            public string NameCrate;
            [JsonProperty(LanguageEnglish ? "Drop chance" : "Шанс выпадения")]
            public float ChanceDrop;
            [JsonProperty(LanguageEnglish ? "Minimum amount of coupons" : "Минимальное количество купона")]
            public int CouponMin;
            [JsonProperty(LanguageEnglish ? "Maximum number of coupons" : "Максимальное количество купона")]
            public int CouponMax;
            public Crates(string namecrate, float chancedrop, int cmin, int cmax);
        }

        internal class LevelSetting
        {
            [JsonProperty(LanguageEnglish ? "Maximum level" : "Максимальный уровень")]
            public int LevelMax;
            [JsonProperty(LanguageEnglish ? "Number of XP to upgrade one level" : "Кол-во ХР для повышения одного уровня")]
            public float LevelXP;
            [JsonProperty(LanguageEnglish ? "How much to increase the number of XP with each level" : "На сколько увеличивать кол-во ХР с каждым уровнем")]
            public float LevelXPUP;
        }

        [JsonProperty(LanguageEnglish ? "Mini-bar location / Main menu settings" : "Расположение мини-панели / Настройки главного меню")]
        public GUISetting GUI;
        internal class XPRateSetting
        {
            [JsonProperty(LanguageEnglish ? "Enable XP multiplier when exchanging coupons - [ This parameter affects only the multipliers for the exchange of coupons ]" : "Включить множитель XP при обмене купонов - [ Данный параметр влияет только на множители для обмена купонов ]")]
            public bool XPRateCoupon;
            [JsonProperty(LanguageEnglish ? "Setting up permissions for XP multipliers for the exchange of coupons and other actions. [ Permission | XP multiplier ]" : "Настройка пермишенов для умножителей ХР на обмен купонов и других действий. [ Пермишен | Множитель XP ]")]
            public Dictionary<string, float> XPRatePermisssion;
        }

        internal class Awards
        {
            [JsonProperty(LanguageEnglish ? "Item shortname / custom reward name [ Must not be empty ]" : "Шортнейм предмета / имя кастомной награды [ Не должно быть пустым ]")]
            public string Shortname;
            [JsonProperty(LanguageEnglish ? "Reward display name" : "Отображаемое имя награды")]
            public string Name;
            [JsonProperty(LanguageEnglish ? "Item quantity" : "Количество предмета")]
            public int Amount;
            [JsonProperty(LanguageEnglish ? "Item skin" : "Скин предмета")]
            public ulong SkinID;
            [JsonProperty(LanguageEnglish ? "Command" : "Команда")]
            public string Command;
            [JsonProperty(LanguageEnglish ? "Link to custom image" : "Ссылка на кастомную картинку")]
            public string URLImage;
            [JsonProperty(LanguageEnglish ? "Hide reward - [ Reward will not be displayed until the player reaches this level ]" : "Скрыть награду - [ Награда не будет отображаться пока игрок не достигнет данного уровня ]")]
            public bool Hide;
            public Awards(string shortname, string name, int amount, ulong skinid, string command, string urlimage, bool hide);
        }

    }

    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private int API_GetLevel(BasePlayer player);
    private int API_GetLevel(ulong userID);
    private void RewardMenu(BasePlayer player, int Page);
    private void SaveData(BasePlayer player);
    private void LootSpawn(LootContainer lootContainer);
    [ConsoleCommand("level_give_xp")]
    private void ccmdGiveXP(ConsoleSystem.Arg arg);
    private void InventoryRewardMenu(BasePlayer player, int Page);
    private void OnLootEntity(BasePlayer player, LootContainer container, Item item);
    private void InitializeLang();
    private const string permVip;
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void GUIOpen(BasePlayer player);
    private void Top(BasePlayer player);
    private void Unload();
    private object OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private void OnLootSpawn(LootContainer lootContainer);
    private void OnOpenVendingShop(NPCVendingMachine machine, BasePlayer player);
    private class XLevelsData
    {
        [JsonProperty(LanguageEnglish ? "Level" : "Уровень")]
        public int Level;
        [JsonProperty(LanguageEnglish ? "XP" : "ХР")]
        public float XP;
    }

    private const string permTop;
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void DataMove(BasePlayer player, int number);
    protected override void LoadConfig();
    private class InvItems
    {
        [JsonProperty(LanguageEnglish ? "Item shortname" : "Шортнейм предмета")]
        public string Shortname;
        [JsonProperty(LanguageEnglish ? "Award display name" : "Отображаемое имя награды")]
        public string Name;
        [JsonProperty(LanguageEnglish ? "Item quantity" : "Количество предмета")]
        public int Amount;
        [JsonProperty(LanguageEnglish ? "Item skin" : "Скин предмета")]
        public ulong SkinID;
        [JsonProperty(LanguageEnglish ? "Command" : "Команда")]
        public string Command;
        [JsonProperty(LanguageEnglish ? "Link to custom image" : "Своя картинка")]
        public string URLImage;
        [JsonProperty(LanguageEnglish ? "VIP reward" : "Вип награда")]
        public bool IsVIP;
        public InvItems(string shortname, string name, int amount, ulong skinid, string command, string urlimage, bool isvip);
    }

    private void API_GiveXP(BasePlayer player, float XPAmount);
    private void OGiveXP(BasePlayer player);
    private LevelConfig config;
    private Dictionary<ulong, List<InvItems>> StoredDataInv;
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin StackSizeController;
    private Plugin BetterChat;
    private void OnPlayerConnected(BasePlayer player);
    private object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
    private void ChatMessage(BasePlayer player, string message);
    private Item OnItemSplit(Item item, int amount);
    private void cmdMenuOpen(BasePlayer player, string command, string[] args);
    private void DataTake(BasePlayer player, int number, int page);
    private void CouponExchange(BasePlayer player, int x);
    private void XPRateRerm(BasePlayer player, float xp);
    protected override void LoadDefaultConfig();
    private const bool LanguageEnglish;
    private void OnGrowableGather(GrowableEntity plant, BasePlayer player);
    private void AddData(BasePlayer player, float XPData);
    private void LoadData(BasePlayer player);
    [ConsoleCommand("level_take")]
    private void ccmdTakeData(ConsoleSystem.Arg arg);
    private string GetPlayerPrefix(ulong userID);
    private void GUI(BasePlayer player);
    private Dictionary<ulong, XLevelsData> StoredData;
    private string API_GetPlayerPrefix(ulong userID);
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private int CouponInfo(BasePlayer player, int x);
    private void ExchangeInfo(BasePlayer player);
    private void Button(BasePlayer player);
    private float XPRatePermCoupon(BasePlayer player, float xp);
}

private class LevelConfig
{
    [JsonProperty(LanguageEnglish ? "Coupons for XP" : "Купоны на ХР")]
    public List<Coupons> Coupon;
    [JsonProperty(LanguageEnglish ? "VIP level reward" : "Вип награда за уровни")]
    public Dictionary<int, Awards> AwardVip;
    internal class OnlineSetting
    {
        [JsonProperty(LanguageEnglish ? "Enable issuing XP to online players" : "Включить выдачу XP онлайн игрокам")]
        public bool OnlineXP;
        [JsonProperty(LanguageEnglish ? "XP issue interval (in sec.)" : "Интервал выдачи XP (в сек.)")]
        public float TimeXP;
        [JsonProperty(LanguageEnglish ? "Setting up permissions. [ Permission | XP ]" : "Настройка пермишенов. [ Пермишен | XP ]")]
        public Dictionary<string, float> Permisssion;
    }

    [JsonProperty(LanguageEnglish ? "Issuing XP for online" : "Выдача XP за онлайн")]
    public OnlineSetting Online;
    internal class GUISetting
    {
        [JsonProperty("AnchorMin")]
        public string AnchorMin;
        [JsonProperty("AnchorMax")]
        public string AnchorMax;
        [JsonProperty("OffsetMin")]
        public string OffsetMin;
        [JsonProperty("OffsetMax")]
        public string OffsetMax;
        [JsonProperty(LanguageEnglish ? "Show mini-bar" : "Отображать мини-панель")]
        public bool EnablePanel;
        [JsonProperty(LanguageEnglish ? "Color background_1" : "Цвет фона_1")]
        public string ColorBackgroundO;
        [JsonProperty(LanguageEnglish ? "Color background_2" : "Цвет фона_2")]
        public string ColorBackgroundT;
        [JsonProperty(LanguageEnglish ? "Display reward container - [ True - Only when there is a reward in the level | False - Always ]" : "Отображать контейнер наград - [ True - Только когда есть награда на уровне | False - Всегда ]")]
        public bool ContainerReward;
        [JsonProperty(LanguageEnglish ? "Display required reward level" : "Отображать требуемый уровень наград")]
        public bool RewardNumber;
        [JsonProperty(LanguageEnglish ? "Display required VIP reward level" : "Отображать требуемый уровень ВИП наград")]
        public bool RewardNumberVIP;
    }

    [JsonProperty(LanguageEnglish ? "XP multiplier" : "Множитель XP")]
    public XPRateSetting XPRate;
    [JsonProperty(LanguageEnglish ? "Vendings settings" : "Настройки магазинов")]
    public VendingSetting Vending;
    internal class XPSetting
    {
        [JsonProperty(LanguageEnglish ? "XP for the pickup of resources" : "ХР за подбор ресурсов")]
        public Dictionary<string, float> PickupXP;
        [JsonProperty(LanguageEnglish ? "XP for harvest" : "ХР за сбор урожая")]
        public Dictionary<string, float> HarvestXP;
        [JsonProperty(LanguageEnglish ? "XP for bonus resources" : "ХР за бонусную добычу")]
        public Dictionary<string, float> GatherBonusXP;
        [JsonProperty(LanguageEnglish ? "XP for kill / destroy barrel" : "ХР за убийство / разбитие бочек")]
        public Dictionary<string, float> KillXP;
        [JsonProperty(LanguageEnglish ? "XP for opening crates" : "ХР за открытие ящиков")]
        public Dictionary<string, float> CrateXP;
    }

    [JsonProperty(LanguageEnglish ? "Settings levels" : "Настройка уровней")]
    public LevelSetting Level;
    internal class VendingSetting
    {
        [JsonProperty(LanguageEnglish ? "Open the level menu. [ True - Immediately after the opening of the NPC shop | False - UI button ]" : "Открывать меню уровней. [ True - Сразу после открытия НПС магазина | False - Через UI кнопку ]")]
        public bool VendingOpen;
        [JsonProperty(LanguageEnglish ? "Access to the level menu is only through the NPC shops. [ True - NPC shop | False - Command ]" : "Доступ к меню уровней только через магазины НПС. [ True - Магазины НПС | False - Команда ]")]
        public bool VendingUse;
        [JsonProperty(LanguageEnglish ? "List of NPC shops in which you can open the level menu (shop name)" : "Список магазинов НПС в которых можно открыть меню уровней (Имя магазина)")]
        public List<string> ListNPCVending;
    }

    internal class Coupons
    {
        [JsonProperty(LanguageEnglish ? "Coupon name" : "Имя купона")]
        public string Name;
        [JsonProperty(LanguageEnglish ? "Coupon text" : "Текст купона")]
        public string Text;
        [JsonProperty(LanguageEnglish ? "Coupon skin" : "Скин купона")]
        public ulong SkinID;
        [JsonProperty(LanguageEnglish ? "XP amount" : "Кол-во ХР")]
        public int XP;
        [JsonProperty(LanguageEnglish ? "Setting the chance of falling out of crates/barrels" : "Настройка шанса выпадения из ящиков/бочек")]
        public List<Crates> Crate;
        public Coupons(string name, string text, ulong skinid, int xp, List<Crates> crates);
    }

    [JsonProperty(LanguageEnglish ? "General settings" : "Общие настройки")]
    public GeneralSetting Setting;
    internal class GeneralSetting
    {
        [JsonProperty(LanguageEnglish ? "List of commands to open the menu" : "Список команд для открытия меню")]
        public List<string> CommandList;
        [JsonProperty(LanguageEnglish ? "XP for the pickup of resources" : "Зачислять ХР за подбор ресурсов")]
        public bool PickupValide;
        [JsonProperty(LanguageEnglish ? "Display the level in prefix" : "Отображать уровень в префиксе")]
        public bool ChatPrefixLevel;
        [JsonProperty(LanguageEnglish ? "XP for opening crates" : "Зачислять ХР за открытие ящиков")]
        public bool CrateValide;
        [JsonProperty(LanguageEnglish ? "List of available ranks - [ Level - Rank ] ( If the list is empty, then the rank will not be displayed in the menu )" : "Список доступных рангов - [ Уровень - Ранг ] ( Если список пуст, то ранг не будет отображаться в меню )")]
        public Dictionary<int, string> RankList;
        [JsonProperty(LanguageEnglish ? "XP for kill" : "Зачислять ХР за убийство")]
        public bool KillValide;
        [JsonProperty(LanguageEnglish ? "Display the rank in prefix" : "Отображать ранг в префиксе")]
        public bool ChatPrefixRank;
        [JsonProperty(LanguageEnglish ? "Include messages of received rewards in chat" : "Включить сообщения полученных  наград в чат")]
        public bool ChatTakeMessages;
        [JsonProperty(LanguageEnglish ? "Enable prefix in chat - [ Set to False if the prefix should be disabled or the prefix is used by a third party chat plugin ]" : "Включить префикс в чате - [ Установите False, если префикс нужно отключить или префикс используется сторонним плагином для чата ]")]
        public bool ChatPrefix;
        [JsonProperty(LanguageEnglish ? "There is a plugin for custom loot" : "Есть плагин на кастомный лут")]
        public bool PluginCLoot;
        [JsonProperty(LanguageEnglish ? "XP for harvest" : "Зачислять ХР за сбор урожая")]
        public bool HarvestValide;
        [JsonProperty(LanguageEnglish ? "Reset the level and XP of the player after reaching the maximum level - [ Players will re-open levels and receive rewards ]" : "Обнулять уровень и ХР игрока после достижения максимального уровня - [ Игроки повторно будет открывать уровни и получать награды ]")]
        public bool ClearAll;
        [JsonProperty(LanguageEnglish ? "Profile SteamID for custom avatar" : "SteamID профиля для кастомной аватарки")]
        public ulong SteamID;
        [JsonProperty(LanguageEnglish ? "Take VIP Reward - [ True - take only with permission | False - take at any time without permission ]" : "Забрать ВИП награду - [ True - забрать только с пермишеном | False - забрать в любое время без пермишена ]")]
        public bool TakeReward;
        [JsonProperty(LanguageEnglish ? "Get VIP reward - [ True - only with permission | False - without permission ]" : "Получить ВИП награду - [ True - только с пермишеном | False - без пермишена ]")]
        public bool MoveReward;
        [JsonProperty(LanguageEnglish ? "Enable VIP rewards" : "Включить ВИП награды")]
        public bool VIPValide;
        [JsonProperty(LanguageEnglish ? "XP for bonus resources" : "Зачислять ХР за бонусные ресурсы")]
        public bool BonusValide;
        [JsonProperty(LanguageEnglish ? "Include level up messages in chat" : "Включить сообщения повышения уровня в чат")]
        public bool ChatLevelMessages;
        [JsonProperty(LanguageEnglish ? "Exchange coupons if you have already reached the maximum level - [ Suitable for top players ]" : "Обменивать купоны если уже достигнут максимальный уровень - [ Подходит для топа игроков ]")]
        public bool GiveXPCoupon;
        [JsonProperty(LanguageEnglish ? "Add XP if the maximum level is already reached - [ Suitable for top players ]" : "Засчитывать ХР если уже достигнут максимальный уровень - [ Подходит для топа игроков ]")]
        public bool GiveXP;
        [JsonProperty(LanguageEnglish ? "Enable coupons" : "Включить купоны")]
        public bool CouponsValide;
    }

    [JsonProperty(LanguageEnglish ? "XP settings | Shortname : ValueXP" : "Настройка ХР | Shortname : ValueXP")]
    public XPSetting XP;
    [JsonProperty(LanguageEnglish ? "Level reward" : "Награда за уровни")]
    public Dictionary<int, Awards> Award;
    public static LevelConfig GetNewConfiguration();
    internal class Crates
    {
        [JsonProperty(LanguageEnglish ? "Name crate/barrel" : "Имя ящика/бочки")]
        public string NameCrate;
        [JsonProperty(LanguageEnglish ? "Drop chance" : "Шанс выпадения")]
        public float ChanceDrop;
        [JsonProperty(LanguageEnglish ? "Minimum amount of coupons" : "Минимальное количество купона")]
        public int CouponMin;
        [JsonProperty(LanguageEnglish ? "Maximum number of coupons" : "Максимальное количество купона")]
        public int CouponMax;
        public Crates(string namecrate, float chancedrop, int cmin, int cmax);
    }

    internal class LevelSetting
    {
        [JsonProperty(LanguageEnglish ? "Maximum level" : "Максимальный уровень")]
        public int LevelMax;
        [JsonProperty(LanguageEnglish ? "Number of XP to upgrade one level" : "Кол-во ХР для повышения одного уровня")]
        public float LevelXP;
        [JsonProperty(LanguageEnglish ? "How much to increase the number of XP with each level" : "На сколько увеличивать кол-во ХР с каждым уровнем")]
        public float LevelXPUP;
    }

    [JsonProperty(LanguageEnglish ? "Mini-bar location / Main menu settings" : "Расположение мини-панели / Настройки главного меню")]
    public GUISetting GUI;
    internal class XPRateSetting
    {
        [JsonProperty(LanguageEnglish ? "Enable XP multiplier when exchanging coupons - [ This parameter affects only the multipliers for the exchange of coupons ]" : "Включить множитель XP при обмене купонов - [ Данный параметр влияет только на множители для обмена купонов ]")]
        public bool XPRateCoupon;
        [JsonProperty(LanguageEnglish ? "Setting up permissions for XP multipliers for the exchange of coupons and other actions. [ Permission | XP multiplier ]" : "Настройка пермишенов для умножителей ХР на обмен купонов и других действий. [ Пермишен | Множитель XP ]")]
        public Dictionary<string, float> XPRatePermisssion;
    }

    internal class Awards
    {
        [JsonProperty(LanguageEnglish ? "Item shortname / custom reward name [ Must not be empty ]" : "Шортнейм предмета / имя кастомной награды [ Не должно быть пустым ]")]
        public string Shortname;
        [JsonProperty(LanguageEnglish ? "Reward display name" : "Отображаемое имя награды")]
        public string Name;
        [JsonProperty(LanguageEnglish ? "Item quantity" : "Количество предмета")]
        public int Amount;
        [JsonProperty(LanguageEnglish ? "Item skin" : "Скин предмета")]
        public ulong SkinID;
        [JsonProperty(LanguageEnglish ? "Command" : "Команда")]
        public string Command;
        [JsonProperty(LanguageEnglish ? "Link to custom image" : "Ссылка на кастомную картинку")]
        public string URLImage;
        [JsonProperty(LanguageEnglish ? "Hide reward - [ Reward will not be displayed until the player reaches this level ]" : "Скрыть награду - [ Награда не будет отображаться пока игрок не достигнет данного уровня ]")]
        public bool Hide;
        public Awards(string shortname, string name, int amount, ulong skinid, string command, string urlimage, bool hide);
    }

}

internal class OnlineSetting
{
    [JsonProperty(LanguageEnglish ? "Enable issuing XP to online players" : "Включить выдачу XP онлайн игрокам")]
    public bool OnlineXP;
    [JsonProperty(LanguageEnglish ? "XP issue interval (in sec.)" : "Интервал выдачи XP (в сек.)")]
    public float TimeXP;
    [JsonProperty(LanguageEnglish ? "Setting up permissions. [ Permission | XP ]" : "Настройка пермишенов. [ Пермишен | XP ]")]
    public Dictionary<string, float> Permisssion;
}

internal class GUISetting
{
    [JsonProperty("AnchorMin")]
    public string AnchorMin;
    [JsonProperty("AnchorMax")]
    public string AnchorMax;
    [JsonProperty("OffsetMin")]
    public string OffsetMin;
    [JsonProperty("OffsetMax")]
    public string OffsetMax;
    [JsonProperty(LanguageEnglish ? "Show mini-bar" : "Отображать мини-панель")]
    public bool EnablePanel;
    [JsonProperty(LanguageEnglish ? "Color background_1" : "Цвет фона_1")]
    public string ColorBackgroundO;
    [JsonProperty(LanguageEnglish ? "Color background_2" : "Цвет фона_2")]
    public string ColorBackgroundT;
    [JsonProperty(LanguageEnglish ? "Display reward container - [ True - Only when there is a reward in the level | False - Always ]" : "Отображать контейнер наград - [ True - Только когда есть награда на уровне | False - Всегда ]")]
    public bool ContainerReward;
    [JsonProperty(LanguageEnglish ? "Display required reward level" : "Отображать требуемый уровень наград")]
    public bool RewardNumber;
    [JsonProperty(LanguageEnglish ? "Display required VIP reward level" : "Отображать требуемый уровень ВИП наград")]
    public bool RewardNumberVIP;
}

internal class XPSetting
{
    [JsonProperty(LanguageEnglish ? "XP for the pickup of resources" : "ХР за подбор ресурсов")]
    public Dictionary<string, float> PickupXP;
    [JsonProperty(LanguageEnglish ? "XP for harvest" : "ХР за сбор урожая")]
    public Dictionary<string, float> HarvestXP;
    [JsonProperty(LanguageEnglish ? "XP for bonus resources" : "ХР за бонусную добычу")]
    public Dictionary<string, float> GatherBonusXP;
    [JsonProperty(LanguageEnglish ? "XP for kill / destroy barrel" : "ХР за убийство / разбитие бочек")]
    public Dictionary<string, float> KillXP;
    [JsonProperty(LanguageEnglish ? "XP for opening crates" : "ХР за открытие ящиков")]
    public Dictionary<string, float> CrateXP;
}

internal class VendingSetting
{
    [JsonProperty(LanguageEnglish ? "Open the level menu. [ True - Immediately after the opening of the NPC shop | False - UI button ]" : "Открывать меню уровней. [ True - Сразу после открытия НПС магазина | False - Через UI кнопку ]")]
    public bool VendingOpen;
    [JsonProperty(LanguageEnglish ? "Access to the level menu is only through the NPC shops. [ True - NPC shop | False - Command ]" : "Доступ к меню уровней только через магазины НПС. [ True - Магазины НПС | False - Команда ]")]
    public bool VendingUse;
    [JsonProperty(LanguageEnglish ? "List of NPC shops in which you can open the level menu (shop name)" : "Список магазинов НПС в которых можно открыть меню уровней (Имя магазина)")]
    public List<string> ListNPCVending;
}

internal class Coupons
{
    [JsonProperty(LanguageEnglish ? "Coupon name" : "Имя купона")]
    public string Name;
    [JsonProperty(LanguageEnglish ? "Coupon text" : "Текст купона")]
    public string Text;
    [JsonProperty(LanguageEnglish ? "Coupon skin" : "Скин купона")]
    public ulong SkinID;
    [JsonProperty(LanguageEnglish ? "XP amount" : "Кол-во ХР")]
    public int XP;
    [JsonProperty(LanguageEnglish ? "Setting the chance of falling out of crates/barrels" : "Настройка шанса выпадения из ящиков/бочек")]
    public List<Crates> Crate;
    public Coupons(string name, string text, ulong skinid, int xp, List<Crates> crates);
}

internal class GeneralSetting
{
    [JsonProperty(LanguageEnglish ? "List of commands to open the menu" : "Список команд для открытия меню")]
    public List<string> CommandList;
    [JsonProperty(LanguageEnglish ? "XP for the pickup of resources" : "Зачислять ХР за подбор ресурсов")]
    public bool PickupValide;
    [JsonProperty(LanguageEnglish ? "Display the level in prefix" : "Отображать уровень в префиксе")]
    public bool ChatPrefixLevel;
    [JsonProperty(LanguageEnglish ? "XP for opening crates" : "Зачислять ХР за открытие ящиков")]
    public bool CrateValide;
    [JsonProperty(LanguageEnglish ? "List of available ranks - [ Level - Rank ] ( If the list is empty, then the rank will not be displayed in the menu )" : "Список доступных рангов - [ Уровень - Ранг ] ( Если список пуст, то ранг не будет отображаться в меню )")]
    public Dictionary<int, string> RankList;
    [JsonProperty(LanguageEnglish ? "XP for kill" : "Зачислять ХР за убийство")]
    public bool KillValide;
    [JsonProperty(LanguageEnglish ? "Display the rank in prefix" : "Отображать ранг в префиксе")]
    public bool ChatPrefixRank;
    [JsonProperty(LanguageEnglish ? "Include messages of received rewards in chat" : "Включить сообщения полученных  наград в чат")]
    public bool ChatTakeMessages;
    [JsonProperty(LanguageEnglish ? "Enable prefix in chat - [ Set to False if the prefix should be disabled or the prefix is used by a third party chat plugin ]" : "Включить префикс в чате - [ Установите False, если префикс нужно отключить или префикс используется сторонним плагином для чата ]")]
    public bool ChatPrefix;
    [JsonProperty(LanguageEnglish ? "There is a plugin for custom loot" : "Есть плагин на кастомный лут")]
    public bool PluginCLoot;
    [JsonProperty(LanguageEnglish ? "XP for harvest" : "Зачислять ХР за сбор урожая")]
    public bool HarvestValide;
    [JsonProperty(LanguageEnglish ? "Reset the level and XP of the player after reaching the maximum level - [ Players will re-open levels and receive rewards ]" : "Обнулять уровень и ХР игрока после достижения максимального уровня - [ Игроки повторно будет открывать уровни и получать награды ]")]
    public bool ClearAll;
    [JsonProperty(LanguageEnglish ? "Profile SteamID for custom avatar" : "SteamID профиля для кастомной аватарки")]
    public ulong SteamID;
    [JsonProperty(LanguageEnglish ? "Take VIP Reward - [ True - take only with permission | False - take at any time without permission ]" : "Забрать ВИП награду - [ True - забрать только с пермишеном | False - забрать в любое время без пермишена ]")]
    public bool TakeReward;
    [JsonProperty(LanguageEnglish ? "Get VIP reward - [ True - only with permission | False - without permission ]" : "Получить ВИП награду - [ True - только с пермишеном | False - без пермишена ]")]
    public bool MoveReward;
    [JsonProperty(LanguageEnglish ? "Enable VIP rewards" : "Включить ВИП награды")]
    public bool VIPValide;
    [JsonProperty(LanguageEnglish ? "XP for bonus resources" : "Зачислять ХР за бонусные ресурсы")]
    public bool BonusValide;
    [JsonProperty(LanguageEnglish ? "Include level up messages in chat" : "Включить сообщения повышения уровня в чат")]
    public bool ChatLevelMessages;
    [JsonProperty(LanguageEnglish ? "Exchange coupons if you have already reached the maximum level - [ Suitable for top players ]" : "Обменивать купоны если уже достигнут максимальный уровень - [ Подходит для топа игроков ]")]
    public bool GiveXPCoupon;
    [JsonProperty(LanguageEnglish ? "Add XP if the maximum level is already reached - [ Suitable for top players ]" : "Засчитывать ХР если уже достигнут максимальный уровень - [ Подходит для топа игроков ]")]
    public bool GiveXP;
    [JsonProperty(LanguageEnglish ? "Enable coupons" : "Включить купоны")]
    public bool CouponsValide;
}

internal class Crates
{
    [JsonProperty(LanguageEnglish ? "Name crate/barrel" : "Имя ящика/бочки")]
    public string NameCrate;
    [JsonProperty(LanguageEnglish ? "Drop chance" : "Шанс выпадения")]
    public float ChanceDrop;
    [JsonProperty(LanguageEnglish ? "Minimum amount of coupons" : "Минимальное количество купона")]
    public int CouponMin;
    [JsonProperty(LanguageEnglish ? "Maximum number of coupons" : "Максимальное количество купона")]
    public int CouponMax;
    public Crates(string namecrate, float chancedrop, int cmin, int cmax);
}

internal class LevelSetting
{
    [JsonProperty(LanguageEnglish ? "Maximum level" : "Максимальный уровень")]
    public int LevelMax;
    [JsonProperty(LanguageEnglish ? "Number of XP to upgrade one level" : "Кол-во ХР для повышения одного уровня")]
    public float LevelXP;
    [JsonProperty(LanguageEnglish ? "How much to increase the number of XP with each level" : "На сколько увеличивать кол-во ХР с каждым уровнем")]
    public float LevelXPUP;
}

internal class XPRateSetting
{
    [JsonProperty(LanguageEnglish ? "Enable XP multiplier when exchanging coupons - [ This parameter affects only the multipliers for the exchange of coupons ]" : "Включить множитель XP при обмене купонов - [ Данный параметр влияет только на множители для обмена купонов ]")]
    public bool XPRateCoupon;
    [JsonProperty(LanguageEnglish ? "Setting up permissions for XP multipliers for the exchange of coupons and other actions. [ Permission | XP multiplier ]" : "Настройка пермишенов для умножителей ХР на обмен купонов и других действий. [ Пермишен | Множитель XP ]")]
    public Dictionary<string, float> XPRatePermisssion;
}

internal class Awards
{
    [JsonProperty(LanguageEnglish ? "Item shortname / custom reward name [ Must not be empty ]" : "Шортнейм предмета / имя кастомной награды [ Не должно быть пустым ]")]
    public string Shortname;
    [JsonProperty(LanguageEnglish ? "Reward display name" : "Отображаемое имя награды")]
    public string Name;
    [JsonProperty(LanguageEnglish ? "Item quantity" : "Количество предмета")]
    public int Amount;
    [JsonProperty(LanguageEnglish ? "Item skin" : "Скин предмета")]
    public ulong SkinID;
    [JsonProperty(LanguageEnglish ? "Command" : "Команда")]
    public string Command;
    [JsonProperty(LanguageEnglish ? "Link to custom image" : "Ссылка на кастомную картинку")]
    public string URLImage;
    [JsonProperty(LanguageEnglish ? "Hide reward - [ Reward will not be displayed until the player reaches this level ]" : "Скрыть награду - [ Награда не будет отображаться пока игрок не достигнет данного уровня ]")]
    public bool Hide;
    public Awards(string shortname, string name, int amount, ulong skinid, string command, string urlimage, bool hide);
}

private class XLevelsData
{
    [JsonProperty(LanguageEnglish ? "Level" : "Уровень")]
    public int Level;
    [JsonProperty(LanguageEnglish ? "XP" : "ХР")]
    public float XP;
}

private class InvItems
{
    [JsonProperty(LanguageEnglish ? "Item shortname" : "Шортнейм предмета")]
    public string Shortname;
    [JsonProperty(LanguageEnglish ? "Award display name" : "Отображаемое имя награды")]
    public string Name;
    [JsonProperty(LanguageEnglish ? "Item quantity" : "Количество предмета")]
    public int Amount;
    [JsonProperty(LanguageEnglish ? "Item skin" : "Скин предмета")]
    public ulong SkinID;
    [JsonProperty(LanguageEnglish ? "Command" : "Команда")]
    public string Command;
    [JsonProperty(LanguageEnglish ? "Link to custom image" : "Своя картинка")]
    public string URLImage;
    [JsonProperty(LanguageEnglish ? "VIP reward" : "Вип награда")]
    public bool IsVIP;
    public InvItems(string shortname, string name, int amount, ulong skinid, string command, string urlimage, bool isvip);
}


```

---

## XMasTime

```csharp
using ConVar;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("XMasEvent", "Reynostrum", "1.0.2")]
[Description("Allow admins to use Rust XMas Event.")]
 class XMasTime : RustPlugin
{
     bool status;
     int daycount;
     string RustTime;
     float spawnRange { get; set; }
     int giftsPerPlayer { get; set; }
     int NightsBetweenChristmas { get; set; }
     bool MessageOnChristmas { get; set; }
    protected override void LoadDefaultConfig();
     void Loaded();
    private void OnTick();
     void XMasFunction();
     void RustTimeCheckFunction();
    [ChatCommand("xmas")]
     void XMasCall(BasePlayer player);
     T GetConfig(string name, T defaultValue);
     string Lang(string key, string id, object[] args);
     bool HasPermission(BasePlayer player, string perm);
     Dictionary<string, string> Messages;
}


```

---

## XMenu

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("XMenu", "Monster", "1.0.804")]
 class XMenu : RustPlugin
{
    public bool eCargoShip;
    public bool eCargoPlane;
    public bool eBradleyAPC;
    public bool eBaseHelicopter;
    public bool eCH47Helicopter;
    public List<BasePlayer> players;
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin IQFakeActive;
    private Plugin FGS;
     int FakeOnline { get; set; }
     void SyncReservedFinish();
    private MenuConfig config;
    private class MenuConfig
    {
        internal class GeneralSetting
        {
            [JsonProperty("Открытое меню после подключения")]
            public bool Connect;
            [JsonProperty("Отображать информацию других плагинов")]
            public bool APluginsInfo;
            [JsonProperty("Отображать ивенты")]
            public bool AEvents;
            [JsonProperty("Обновлять меню [ Обновляется только открытое меню ]")]
            public bool Reload;
            [JsonProperty("Обновлять информацию других плагинов [ Обновляется только при открытом меню ]")]
            public bool ReloadPluginsInfo;
            [JsonProperty("Интервал обновления открытого меню")]
            public float IReload;
            [JsonProperty("Фейк онлайн от плагина - [ Default - 0 | FGS - 1 | IQFakeActive - 2]")]
            public int FakeOnline;
        }

        internal class LogoSetting
        {
            [JsonProperty("Ссылка на картинку логотипа")]
            public string LogoURL;
            [JsonProperty("Цвет логотипа")]
            public string LogoColor;
            [JsonProperty("Материал логотипа")]
            public string LogoMaterial;
            [JsonProperty("Лого - AnchorMin")]
            public string AnchorMin;
            [JsonProperty("Лого - AnchorMax")]
            public string AnchorMax;
            [JsonProperty("Лого - OffsetMin")]
            public string OffsetMin;
            [JsonProperty("Лого - OffsetMax")]
            public string OffsetMax;
            [JsonProperty("Лого в меню - AnchorMin")]
            public string MAnchorMin;
            [JsonProperty("Лого в меню - AnchorMax")]
            public string MAnchorMax;
            [JsonProperty("Лого в меню - OffsetMin")]
            public string MOffsetMin;
            [JsonProperty("Лого в меню - OffsetMax")]
            public string MOffsetMax;
        }

        internal class MenuSetting
        {
            [JsonProperty("Цвет меню")]
            public string MenuColor;
            [JsonProperty("Материал меню")]
            public string MenuMaterial;
            [JsonProperty("Цвет кнопок")]
            public string ButtonColor;
            [JsonProperty("Цвет текста кнопок")]
            public string ButtonTextColor;
            [JsonProperty("Размер текста кнопок")]
            public int ButtonSize;
            [JsonProperty("Закрывать меню после нажатия одной из кнопок")]
            public bool CloseMenu;
            [JsonProperty("Меню - AnchorMin")]
            public string MAnchorMin;
            [JsonProperty("Меню - AnchorMax")]
            public string MAnchorMax;
            [JsonProperty("Меню - OffsetMin")]
            public string MOffsetMin;
            [JsonProperty("Меню - OffsetMax")]
            public string MOffsetMax;
            [JsonProperty("Инфа плагинов - AnchorMin")]
            public string PAnchorMin;
            [JsonProperty("Инфа плагинов - AnchorMax")]
            public string PAnchorMax;
            [JsonProperty("Инфа плагинов - OffsetMin")]
            public string POffsetMin;
            [JsonProperty("Инфа плагинов - OffsetMax")]
            public string POffsetMax;
        }

        internal class EventSetting
        {
            [JsonProperty("Ссылка на картинку ивента")]
            public string EventURL;
            [JsonProperty("Цвет активного ивента")]
            public string EventAColor;
            [JsonProperty("Цвет неактивного ивента")]
            public string EventDColor;
        }

        internal class EventsSetting
        {
            [JsonProperty("Цвет меню ивентов")]
            public string EMenuColor;
            [JsonProperty("Материал меню ивентов")]
            public string EMenuMaterial;
            [JsonProperty("Цвет фона иконок ивентов")]
            public string EBackgroundColor;
            [JsonProperty("Настройка иконок ивентов")]
            public Dictionary<string, EventSetting> Events;
        }

        internal class PluginsInfoSetting
        {
            [JsonProperty("Название плагина")]
            public string PluginName;
            [JsonProperty("Название метода(API)")]
            public string HookName;
            [JsonProperty("Тип параметра хука - [ player | userID ]")]
            public string Parameter;
            public PluginsInfoSetting(string pluginname, string hookname, string parameter);
        }

        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Настройка логотипа")]
        public LogoSetting Logo;
        [JsonProperty("Настройка меню")]
        public MenuSetting Menu;
        [JsonProperty("Настройка кнопок [ Текст | Команда ]")]
        public Dictionary<string, string> Button;
        [JsonProperty("Настройка дополнительных кнопок [ Команда | Иконка ]")]
        public Dictionary<string, string> ButtonP;
        [JsonProperty("Настройка ивентов")]
        public EventsSetting Event;
        [JsonProperty("Настройка информации других плагинов. [ Хуки c типом параметра - player(BasePlayer) | userID(ulong) ]")]
        public List<PluginsInfoSetting> PluginsInfo;
        public static MenuConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnEntityKill(BaseNetworkable entity);
    [ConsoleCommand("ui_menu")]
     void cmdOpenGUI(ConsoleSystem.Arg args);
    private void GUILogo(BasePlayer player);
    private void GUIMenu(BasePlayer player);
    private void GUIMenuInfo(BasePlayer player);
    private void GUIEvent(BasePlayer player);
    private void GUIPluginsInfo(BasePlayer player);
     void InitializeLang();
}

private class MenuConfig
{
    internal class GeneralSetting
    {
        [JsonProperty("Открытое меню после подключения")]
        public bool Connect;
        [JsonProperty("Отображать информацию других плагинов")]
        public bool APluginsInfo;
        [JsonProperty("Отображать ивенты")]
        public bool AEvents;
        [JsonProperty("Обновлять меню [ Обновляется только открытое меню ]")]
        public bool Reload;
        [JsonProperty("Обновлять информацию других плагинов [ Обновляется только при открытом меню ]")]
        public bool ReloadPluginsInfo;
        [JsonProperty("Интервал обновления открытого меню")]
        public float IReload;
        [JsonProperty("Фейк онлайн от плагина - [ Default - 0 | FGS - 1 | IQFakeActive - 2]")]
        public int FakeOnline;
    }

    internal class LogoSetting
    {
        [JsonProperty("Ссылка на картинку логотипа")]
        public string LogoURL;
        [JsonProperty("Цвет логотипа")]
        public string LogoColor;
        [JsonProperty("Материал логотипа")]
        public string LogoMaterial;
        [JsonProperty("Лого - AnchorMin")]
        public string AnchorMin;
        [JsonProperty("Лого - AnchorMax")]
        public string AnchorMax;
        [JsonProperty("Лого - OffsetMin")]
        public string OffsetMin;
        [JsonProperty("Лого - OffsetMax")]
        public string OffsetMax;
        [JsonProperty("Лого в меню - AnchorMin")]
        public string MAnchorMin;
        [JsonProperty("Лого в меню - AnchorMax")]
        public string MAnchorMax;
        [JsonProperty("Лого в меню - OffsetMin")]
        public string MOffsetMin;
        [JsonProperty("Лого в меню - OffsetMax")]
        public string MOffsetMax;
    }

    internal class MenuSetting
    {
        [JsonProperty("Цвет меню")]
        public string MenuColor;
        [JsonProperty("Материал меню")]
        public string MenuMaterial;
        [JsonProperty("Цвет кнопок")]
        public string ButtonColor;
        [JsonProperty("Цвет текста кнопок")]
        public string ButtonTextColor;
        [JsonProperty("Размер текста кнопок")]
        public int ButtonSize;
        [JsonProperty("Закрывать меню после нажатия одной из кнопок")]
        public bool CloseMenu;
        [JsonProperty("Меню - AnchorMin")]
        public string MAnchorMin;
        [JsonProperty("Меню - AnchorMax")]
        public string MAnchorMax;
        [JsonProperty("Меню - OffsetMin")]
        public string MOffsetMin;
        [JsonProperty("Меню - OffsetMax")]
        public string MOffsetMax;
        [JsonProperty("Инфа плагинов - AnchorMin")]
        public string PAnchorMin;
        [JsonProperty("Инфа плагинов - AnchorMax")]
        public string PAnchorMax;
        [JsonProperty("Инфа плагинов - OffsetMin")]
        public string POffsetMin;
        [JsonProperty("Инфа плагинов - OffsetMax")]
        public string POffsetMax;
    }

    internal class EventSetting
    {
        [JsonProperty("Ссылка на картинку ивента")]
        public string EventURL;
        [JsonProperty("Цвет активного ивента")]
        public string EventAColor;
        [JsonProperty("Цвет неактивного ивента")]
        public string EventDColor;
    }

    internal class EventsSetting
    {
        [JsonProperty("Цвет меню ивентов")]
        public string EMenuColor;
        [JsonProperty("Материал меню ивентов")]
        public string EMenuMaterial;
        [JsonProperty("Цвет фона иконок ивентов")]
        public string EBackgroundColor;
        [JsonProperty("Настройка иконок ивентов")]
        public Dictionary<string, EventSetting> Events;
    }

    internal class PluginsInfoSetting
    {
        [JsonProperty("Название плагина")]
        public string PluginName;
        [JsonProperty("Название метода(API)")]
        public string HookName;
        [JsonProperty("Тип параметра хука - [ player | userID ]")]
        public string Parameter;
        public PluginsInfoSetting(string pluginname, string hookname, string parameter);
    }

    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Настройка логотипа")]
    public LogoSetting Logo;
    [JsonProperty("Настройка меню")]
    public MenuSetting Menu;
    [JsonProperty("Настройка кнопок [ Текст | Команда ]")]
    public Dictionary<string, string> Button;
    [JsonProperty("Настройка дополнительных кнопок [ Команда | Иконка ]")]
    public Dictionary<string, string> ButtonP;
    [JsonProperty("Настройка ивентов")]
    public EventsSetting Event;
    [JsonProperty("Настройка информации других плагинов. [ Хуки c типом параметра - player(BasePlayer) | userID(ulong) ]")]
    public List<PluginsInfoSetting> PluginsInfo;
    public static MenuConfig GetNewConfiguration();
}

internal class GeneralSetting
{
    [JsonProperty("Открытое меню после подключения")]
    public bool Connect;
    [JsonProperty("Отображать информацию других плагинов")]
    public bool APluginsInfo;
    [JsonProperty("Отображать ивенты")]
    public bool AEvents;
    [JsonProperty("Обновлять меню [ Обновляется только открытое меню ]")]
    public bool Reload;
    [JsonProperty("Обновлять информацию других плагинов [ Обновляется только при открытом меню ]")]
    public bool ReloadPluginsInfo;
    [JsonProperty("Интервал обновления открытого меню")]
    public float IReload;
    [JsonProperty("Фейк онлайн от плагина - [ Default - 0 | FGS - 1 | IQFakeActive - 2]")]
    public int FakeOnline;
}

internal class LogoSetting
{
    [JsonProperty("Ссылка на картинку логотипа")]
    public string LogoURL;
    [JsonProperty("Цвет логотипа")]
    public string LogoColor;
    [JsonProperty("Материал логотипа")]
    public string LogoMaterial;
    [JsonProperty("Лого - AnchorMin")]
    public string AnchorMin;
    [JsonProperty("Лого - AnchorMax")]
    public string AnchorMax;
    [JsonProperty("Лого - OffsetMin")]
    public string OffsetMin;
    [JsonProperty("Лого - OffsetMax")]
    public string OffsetMax;
    [JsonProperty("Лого в меню - AnchorMin")]
    public string MAnchorMin;
    [JsonProperty("Лого в меню - AnchorMax")]
    public string MAnchorMax;
    [JsonProperty("Лого в меню - OffsetMin")]
    public string MOffsetMin;
    [JsonProperty("Лого в меню - OffsetMax")]
    public string MOffsetMax;
}

internal class MenuSetting
{
    [JsonProperty("Цвет меню")]
    public string MenuColor;
    [JsonProperty("Материал меню")]
    public string MenuMaterial;
    [JsonProperty("Цвет кнопок")]
    public string ButtonColor;
    [JsonProperty("Цвет текста кнопок")]
    public string ButtonTextColor;
    [JsonProperty("Размер текста кнопок")]
    public int ButtonSize;
    [JsonProperty("Закрывать меню после нажатия одной из кнопок")]
    public bool CloseMenu;
    [JsonProperty("Меню - AnchorMin")]
    public string MAnchorMin;
    [JsonProperty("Меню - AnchorMax")]
    public string MAnchorMax;
    [JsonProperty("Меню - OffsetMin")]
    public string MOffsetMin;
    [JsonProperty("Меню - OffsetMax")]
    public string MOffsetMax;
    [JsonProperty("Инфа плагинов - AnchorMin")]
    public string PAnchorMin;
    [JsonProperty("Инфа плагинов - AnchorMax")]
    public string PAnchorMax;
    [JsonProperty("Инфа плагинов - OffsetMin")]
    public string POffsetMin;
    [JsonProperty("Инфа плагинов - OffsetMax")]
    public string POffsetMax;
}

internal class EventSetting
{
    [JsonProperty("Ссылка на картинку ивента")]
    public string EventURL;
    [JsonProperty("Цвет активного ивента")]
    public string EventAColor;
    [JsonProperty("Цвет неактивного ивента")]
    public string EventDColor;
}

internal class EventsSetting
{
    [JsonProperty("Цвет меню ивентов")]
    public string EMenuColor;
    [JsonProperty("Материал меню ивентов")]
    public string EMenuMaterial;
    [JsonProperty("Цвет фона иконок ивентов")]
    public string EBackgroundColor;
    [JsonProperty("Настройка иконок ивентов")]
    public Dictionary<string, EventSetting> Events;
}

internal class PluginsInfoSetting
{
    [JsonProperty("Название плагина")]
    public string PluginName;
    [JsonProperty("Название метода(API)")]
    public string HookName;
    [JsonProperty("Тип параметра хука - [ player | userID ]")]
    public string Parameter;
    public PluginsInfoSetting(string pluginname, string hookname, string parameter);
}


```

---

## XMiniCopterPlus

```csharp
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("XMiniCopterPlus", "https://topplugin.ru/ / https://discord.com/invite/5DPTsRmd3G", "1.0.31")]
 class XMiniCopterPlus : RustPlugin
{
    private CopterConfig config;
    private class CopterConfig
    {
        internal class CopterSetting
        {
            [JsonProperty("SkinID")]
            public ulong CopterSkinID;
            [JsonProperty("Имя")]
            public string CopterName;
        }

        internal class WorkSetting
        {
            [JsonProperty("Включить третье место")]
            public bool Chair;
            [JsonProperty("Включить стеш")]
            public bool Stash;
        }

        [JsonProperty("Настройки коптера")]
        public CopterSetting Copter;
        [JsonProperty("Общее")]
        public WorkSetting Settings;
        public static CopterConfig GetNewConfiguration();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    [ConsoleCommand("c_give")]
     void cmdConsoleCommand(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private void OnEntitySpawned(MiniCopter copter);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void SpawnEntity(BaseEntity entity);
    private void Chair(MiniCopter copter);
    private void Stash(MiniCopter copter);
     MiniCopter.MountPointInfo SpawnPassenger(MiniCopter copter);
}

private class CopterConfig
{
    internal class CopterSetting
    {
        [JsonProperty("SkinID")]
        public ulong CopterSkinID;
        [JsonProperty("Имя")]
        public string CopterName;
    }

    internal class WorkSetting
    {
        [JsonProperty("Включить третье место")]
        public bool Chair;
        [JsonProperty("Включить стеш")]
        public bool Stash;
    }

    [JsonProperty("Настройки коптера")]
    public CopterSetting Copter;
    [JsonProperty("Общее")]
    public WorkSetting Settings;
    public static CopterConfig GetNewConfiguration();
}

internal class CopterSetting
{
    [JsonProperty("SkinID")]
    public ulong CopterSkinID;
    [JsonProperty("Имя")]
    public string CopterName;
}

internal class WorkSetting
{
    [JsonProperty("Включить третье место")]
    public bool Chair;
    [JsonProperty("Включить стеш")]
    public bool Stash;
}


```

---

## XMoneyReward

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("XMoneyReward", "Monster", "1.0.3")]
 class XMoneyReward : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin RustStore;
    private MoneyConfig config;
    private class MoneyConfig
    {
        internal class StoreSetting
        {
            [JsonProperty("Секретный ключ: (Оставьте пустым если магазин OVH)")]
            public string StoreKey;
            [JsonProperty("ID магазина: (Оставьте пустым если магазин OVH)")]
            public string StoreID;
            [JsonProperty("0 - GameStores | 1 - OVH | 2 - плагин экономики, наград или внутриигровой магазин. К примеру XShop")]
            public int Store;
            [JsonProperty("Название плагина")]
            public string PluginName;
            [JsonProperty("Название метода(API)")]
            public string HookName;
            [JsonProperty("Тип параметра хука - [ player | userID ]")]
            public string Parameter;
        }

        internal class RewardSetting
        {
            [JsonProperty("Время онлайна для доступа к выводам")]
            public int RewardTimeTake;
            [JsonProperty("Интервал выдачи бонуса (в сек.)")]
            public int RewardTime;
            [JsonProperty("Максимальный баланс для вывода")]
            public int RewardMax;
            [JsonProperty("Вывод раз в сутки")]
            public bool TakeDay;
        }

        internal class LogoSetting
        {
            [JsonProperty("AnchorMin")]
            public string AnchorMin;
            [JsonProperty("AnchorMax")]
            public string AnchorMax;
            [JsonProperty("OffsetMin")]
            public string OffsetMin;
            [JsonProperty("OffsetMax")]
            public string OffsetMax;
            [JsonProperty("Цвет картинки")]
            public string ColorLogo;
            [JsonProperty("Цвет текста")]
            public string ColorText;
            [JsonProperty("Картинка логотипа")]
            public string ImageLogo;
        }

        internal class GUISetting
        {
            [JsonProperty("AnchorMin")]
            public string AnchorMin;
            [JsonProperty("AnchorMax")]
            public string AnchorMax;
            [JsonProperty("OffsetMin")]
            public string OffsetMin;
            [JsonProperty("OffsetMax")]
            public string OffsetMax;
            [JsonProperty("Цвет панели")]
            public string ColorPanel;
            [JsonProperty("Цвет кнопок")]
            public string ColorButton;
        }

        [JsonProperty("Настройка пермишенов. [ Размер бонуса | Пермишен ]")]
        public Dictionary<string, float> Permisssion;
        [JsonProperty("Данные магазина")]
        public StoreSetting Store;
        [JsonProperty("Деньги за онлайн")]
        public RewardSetting Reward;
        [JsonProperty("Настройка логотипа")]
        public LogoSetting LogoGUI;
        [JsonProperty("Настройка меню")]
        public GUISetting GUI;
        public static MoneyConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class XMoneyRewardData
    {
        [JsonProperty("Время игры")]
        public int TimePlay;
        [JsonProperty("Деньги")]
        public float Money;
        [JsonProperty("День последнего вывода")]
        public int MoneyTake;
    }

    private Dictionary<ulong, XMoneyRewardData> StoredData;
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void GiveMoney(BasePlayer player);
    private void TakeMoney(BasePlayer player);
    private void TakeMoneyData(BasePlayer player, double money);
    [ConsoleCommand("money")]
    private void cmdConsoleCommand(ConsoleSystem.Arg args);
    private void LogoGUI(BasePlayer player);
    private void MoneyGUI(BasePlayer player);
    private void Message(BasePlayer player, string Messages);
    private void InitializeLang();
}

private class MoneyConfig
{
    internal class StoreSetting
    {
        [JsonProperty("Секретный ключ: (Оставьте пустым если магазин OVH)")]
        public string StoreKey;
        [JsonProperty("ID магазина: (Оставьте пустым если магазин OVH)")]
        public string StoreID;
        [JsonProperty("0 - GameStores | 1 - OVH | 2 - плагин экономики, наград или внутриигровой магазин. К примеру XShop")]
        public int Store;
        [JsonProperty("Название плагина")]
        public string PluginName;
        [JsonProperty("Название метода(API)")]
        public string HookName;
        [JsonProperty("Тип параметра хука - [ player | userID ]")]
        public string Parameter;
    }

    internal class RewardSetting
    {
        [JsonProperty("Время онлайна для доступа к выводам")]
        public int RewardTimeTake;
        [JsonProperty("Интервал выдачи бонуса (в сек.)")]
        public int RewardTime;
        [JsonProperty("Максимальный баланс для вывода")]
        public int RewardMax;
        [JsonProperty("Вывод раз в сутки")]
        public bool TakeDay;
    }

    internal class LogoSetting
    {
        [JsonProperty("AnchorMin")]
        public string AnchorMin;
        [JsonProperty("AnchorMax")]
        public string AnchorMax;
        [JsonProperty("OffsetMin")]
        public string OffsetMin;
        [JsonProperty("OffsetMax")]
        public string OffsetMax;
        [JsonProperty("Цвет картинки")]
        public string ColorLogo;
        [JsonProperty("Цвет текста")]
        public string ColorText;
        [JsonProperty("Картинка логотипа")]
        public string ImageLogo;
    }

    internal class GUISetting
    {
        [JsonProperty("AnchorMin")]
        public string AnchorMin;
        [JsonProperty("AnchorMax")]
        public string AnchorMax;
        [JsonProperty("OffsetMin")]
        public string OffsetMin;
        [JsonProperty("OffsetMax")]
        public string OffsetMax;
        [JsonProperty("Цвет панели")]
        public string ColorPanel;
        [JsonProperty("Цвет кнопок")]
        public string ColorButton;
    }

    [JsonProperty("Настройка пермишенов. [ Размер бонуса | Пермишен ]")]
    public Dictionary<string, float> Permisssion;
    [JsonProperty("Данные магазина")]
    public StoreSetting Store;
    [JsonProperty("Деньги за онлайн")]
    public RewardSetting Reward;
    [JsonProperty("Настройка логотипа")]
    public LogoSetting LogoGUI;
    [JsonProperty("Настройка меню")]
    public GUISetting GUI;
    public static MoneyConfig GetNewConfiguration();
}

internal class StoreSetting
{
    [JsonProperty("Секретный ключ: (Оставьте пустым если магазин OVH)")]
    public string StoreKey;
    [JsonProperty("ID магазина: (Оставьте пустым если магазин OVH)")]
    public string StoreID;
    [JsonProperty("0 - GameStores | 1 - OVH | 2 - плагин экономики, наград или внутриигровой магазин. К примеру XShop")]
    public int Store;
    [JsonProperty("Название плагина")]
    public string PluginName;
    [JsonProperty("Название метода(API)")]
    public string HookName;
    [JsonProperty("Тип параметра хука - [ player | userID ]")]
    public string Parameter;
}

internal class RewardSetting
{
    [JsonProperty("Время онлайна для доступа к выводам")]
    public int RewardTimeTake;
    [JsonProperty("Интервал выдачи бонуса (в сек.)")]
    public int RewardTime;
    [JsonProperty("Максимальный баланс для вывода")]
    public int RewardMax;
    [JsonProperty("Вывод раз в сутки")]
    public bool TakeDay;
}

internal class LogoSetting
{
    [JsonProperty("AnchorMin")]
    public string AnchorMin;
    [JsonProperty("AnchorMax")]
    public string AnchorMax;
    [JsonProperty("OffsetMin")]
    public string OffsetMin;
    [JsonProperty("OffsetMax")]
    public string OffsetMax;
    [JsonProperty("Цвет картинки")]
    public string ColorLogo;
    [JsonProperty("Цвет текста")]
    public string ColorText;
    [JsonProperty("Картинка логотипа")]
    public string ImageLogo;
}

internal class GUISetting
{
    [JsonProperty("AnchorMin")]
    public string AnchorMin;
    [JsonProperty("AnchorMax")]
    public string AnchorMax;
    [JsonProperty("OffsetMin")]
    public string OffsetMin;
    [JsonProperty("OffsetMax")]
    public string OffsetMax;
    [JsonProperty("Цвет панели")]
    public string ColorPanel;
    [JsonProperty("Цвет кнопок")]
    public string ColorButton;
}

private class XMoneyRewardData
{
    [JsonProperty("Время игры")]
    public int TimePlay;
    [JsonProperty("Деньги")]
    public float Money;
    [JsonProperty("День последнего вывода")]
    public int MoneyTake;
}


```

---

## XNickController

```csharp
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using System;
using Oxide.Core.Plugins;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("XNickController", "Monster", "1.0.0")]
public class XNickController : RustPlugin
{
    private NickConfig config;
    private class NickConfig
    {
        internal class SymbolSetting
        {
            [JsonProperty("?_?")]
            public List<string> SymbolList;
        }

        [JsonProperty("Список символов/слов которые нужно удалять из ника")]
        public SymbolSetting Symbol;
        public static NickConfig GetNewConfiguration();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
}

private class NickConfig
{
    internal class SymbolSetting
    {
        [JsonProperty("?_?")]
        public List<string> SymbolList;
    }

    [JsonProperty("Список символов/слов которые нужно удалять из ника")]
    public SymbolSetting Symbol;
    public static NickConfig GetNewConfiguration();
}

internal class SymbolSetting
{
    [JsonProperty("?_?")]
    public List<string> SymbolList;
}


```

---

## XPBackup

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System.Linq;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("XP Backup", "PaiN", "0.2", ResourceId = 2035)]
 class XPBackup : RustPlugin
{
    [PluginReference]
     Plugin PvXselector;
     class Data
    {
        public List<XPInfo> xpinfo;
    }

    static Data data;
     class XPInfo
    {
        public float Level;
        public float XP;
        public ulong steamId;
        public bool OnConnect;
        internal static XPInfo GetInfo(ulong Id);
    }

     void Loaded();
     void OnServerSave();
     void LoadMessages();
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("xpb")]
     void cmdBackup(BasePlayer player, string cmd, string[] args);
     void ResetAndAdd(BasePlayer player);
     void SaveData();
     string LangMsg(string msg, object uid);
}

 class Data
{
    public List<XPInfo> xpinfo;
}

 class XPInfo
{
    public float Level;
    public float XP;
    public ulong steamId;
    public bool OnConnect;
    internal static XPInfo GetInfo(ulong Id);
}


```

---

## XpBooster

```csharp
using Rust.Xp;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("XpBooster", "Wulf/lukespragg", "0.6.0", ResourceId = 2001)]
[Description("Multiplies the base XP players earn per source")]
 class XpBooster : RustPlugin
{
    [PluginReference]
     Plugin AdminRadar;
    [PluginReference]
     Plugin Godmode;
     bool usePermissions;
    protected override void LoadDefaultConfig();
     void Init();
     object OnXpEarn(ulong steamId, double amount, string source);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
}


```

---

## XpControl

```csharp
using Oxide.Core.Plugins;
using Rust.Xp;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using UnityEngine;

Oxide.Plugins
[Info("XP Control", "miRror", "5.0.0", ResourceId = 2013)]
 class XpControl : RustPlugin
{
    private int maxLevel;
    private void Init();
    private void Loaded();
    private void Action(BasePlayer player, ConsoleSystem.Arg arg, string cmd, string[] args);
    private BasePlayer GetPlayer(BasePlayer player, ConsoleSystem.Arg arg, string cmd, string[] args, int needArgs, bool isServer, string userID);
    private string Lang(string key, string userID);
    private void Reply(BasePlayer player, ConsoleSystem.Arg arg, string message, bool isServer);
    private void cmdChatHandler(BasePlayer player, string cmd, string[] args);
    private void cmdConsoleHandler(ConsoleSystem.Arg arg);
    [HookMethod("XPCSetLVL")]
    public object XPCSetLVL(ulong userID, int level);
    [HookMethod("XPCAddLVL")]
    public object XPCAddLVL(ulong userID, int level);
    [HookMethod("XPCResetLVL")]
    public void XPCResetLVL(ulong userID);
    [HookMethod("XPCAddXP")]
    public void XPCAddXP(ulong userID, int xp);
}


```

---

## XPDeathReducer

```csharp
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Rust.Xp;

Oxide.Plugins
[Info("XP Death Reducer", "k1lly0u", "0.1.2", ResourceId = 2007)]
 class XPDeathReducer : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin EventManager;
     void Loaded();
     void OnServerInitialized();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void DeductPlayerXP(BasePlayer victim, BasePlayer attacker);
    private void MessagePlayer(BasePlayer player, string message);
    private bool IsClanmate(ulong playerId, ulong friendId);
    private bool IsFriend(ulong playerId, ulong friendId);
    private bool IsPlaying(BasePlayer player);
    private ConfigData configData;
     class Exempt
    {
        public bool AdminExempt;
        public bool FriendsExempt;
        public bool ClansExempt;
    }

     class Deductions
    {
        public bool UsePercentageDeduction;
        public float XPDeductionPercentage;
        public float XPDeductionStatic;
    }

     class Options
    {
        public bool GiveDeductedXPToKiller;
    }

     class Messaging
    {
        public string MSG_MainColor;
        public string MSG_Color;
    }

     class ConfigData
    {
        public Exempt Exemptions { get; set; }
        public Deductions Deductions { get; set; }
        public Options Options { get; set; }
        public Messaging Messaging { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
    private string MSG(string key, string playerid);
     Dictionary<string, string> Messages;
}

 class Exempt
{
    public bool AdminExempt;
    public bool FriendsExempt;
    public bool ClansExempt;
}

 class Deductions
{
    public bool UsePercentageDeduction;
    public float XPDeductionPercentage;
    public float XPDeductionStatic;
}

 class Options
{
    public bool GiveDeductedXPToKiller;
}

 class Messaging
{
    public string MSG_MainColor;
    public string MSG_Color;
}

 class ConfigData
{
    public Exempt Exemptions { get; set; }
    public Deductions Deductions { get; set; }
    public Options Options { get; set; }
    public Messaging Messaging { get; set; }
}


```

---

## XPEqualizer

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Rust.Xp;

Oxide.Plugins
[Info("XPEqualizer", "k1lly0u", "0.1.4", ResourceId = 2003)]
 class XPEqualizer : RustPlugin
{
     EqualizerData equalData;
    private DynamicConfigFile data;
     void Loaded();
     void Unload();
     void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
     void OnXpEarned(ulong playerid, float amount, string source);
    private float GetAverageStats();
    private object GetPlayerXP(BasePlayer player);
    private void ResetPlayerLevel(ulong userid, float amount, string message);
    private BasePlayer FindPlayer(BasePlayer player, string arg);
    [ChatCommand("xpe")]
    private void cmdXPE(BasePlayer player, string command, string[] args);
    private ConfigData configData;
     class ConfigData
    {
        public string MSG_MainColor { get; set; }
        public string MSG_Color { get; set; }
        public bool IgnoreAdmins_Reset { get; set; }
        public bool SkipAdminXP { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void SaveLoop();
     void SaveData();
     void LoadData();
     class EqualizerData
    {
        public Dictionary<ulong, float> PlayerXP;
    }

    private void SendMSG(BasePlayer player, string message, string message2);
    private string MSG(string key, string playerid);
     Dictionary<string, string> Messages;
}

 class ConfigData
{
    public string MSG_MainColor { get; set; }
    public string MSG_Color { get; set; }
    public bool IgnoreAdmins_Reset { get; set; }
    public bool SkipAdminXP { get; set; }
}

 class EqualizerData
{
    public Dictionary<ulong, float> PlayerXP;
}


```

---

## XPManager

```csharp
using System.Collections.Generic;
using System.Linq;
using System;

Oxide.Plugins
[Info("XP Manager", "LaserHydra", "1.1.0", ResourceId = 2026)]
[Description("XP Manager")]
 class XPManager : RustPlugin
{
     class Range
    {
        public int Min;
        public int Max;
        public Range(int Min, int Max);
    }

     Dictionary<string, object> PermissionLevelRanges;
     Dictionary<string, object> PermissionMultipliers;
     Dictionary<string, object> EarningMultipliers;
     bool CanAdminsGainXP;
     bool CanSleepersGainXP;
     bool InstantMaxLevel;
     float LevelLossOnDeath;
     int MinimalLevel;
     int MaximalLevel;
     void Loaded();
     void OnPlayerInit(BasePlayer player);
     void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
     object OnXpEarn(ulong id, float amount, string source);
    new void LoadConfig();
     void LoadMessages();
    protected override void LoadDefaultConfig();
     int GetMaxLevel();
     float GetLevel(BasePlayer player);
     float GetLevel(ulong id);
     void AddLevel(BasePlayer player, int level);
     void AddLevel(ulong id, int level);
     void SetLevel(BasePlayer player, int level);
     void SetLevel(ulong id, int level);
     float GetMultiplier(string source);
     float GetPermissionMultiplier(ulong id);
     int GetMinLevel(ulong id);
     int GetMaxLevel(ulong id);
     Range LevelRangeFromPermission(string perm);
     Dictionary<string, object> StandardEarningMultipliers { get; set; }
    [ConsoleCommand("xp")]
     void XPCCmd(ConsoleSystem.Arg arg);
    [ChatCommand("xp")]
     void XPCmd(BasePlayer player, string cmd, string[] args, bool console);
     bool CanBeParsedTo(string str);
     bool IsOnline(ulong id);
     bool IsAdmin(ulong id);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer player);
     string ListToString(List<T> list, int first, string seperator);
     T GetConfig(object[] args);
     void LoadData(T data, string filename);
     void SaveData(T data, string filename);
     string GetMsg(string key, object userID);
     void RegisterPerm(string[] permArray);
     bool HasPerm(object uid, string[] permArray);
     string PermissionPrefix { get; set; }
     void Reply(BasePlayer player, string message, bool console);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Range
{
    public int Min;
    public int Max;
    public Range(int Min, int Max);
}


```

---

## XPPermissions

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries;
using Oxide.Core;
using System.Linq;

Oxide.Plugins
[Info("XP Permissions", "PaiN", "0.3.1", ResourceId = 2024)]
[Description("Grants player permission on their level up.")]
 class XPPermissions : RustPlugin
{
     ConfigFile Cfg;
     class ConfigFile
    {
        public Dictionary<int, List<_Permission>> Permissions;
    }

     class _Permission
    {
        public List<string> Permission;
        public string Group;
        public _Permission(string groupname, string[] permission);
    }

     void Loaded();
     void LoadMessages();
    protected override void LoadDefaultConfig();
     void OnXpEarned(ulong id, float amount, string source);
     string LangMsg(string msg, string uid);
}

 class ConfigFile
{
    public Dictionary<int, List<_Permission>> Permissions;
}

 class _Permission
{
    public List<string> Permission;
    public string Group;
    public _Permission(string groupname, string[] permission);
}


```

---

## XPShop

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System.Globalization;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("XPShop", "Chibubrik", "1.0.0")]
 class XPShop : RustPlugin
{
    private string Layer;
    [PluginReference]
    private Plugin ImageLibrary;
    public class Xp
    {
        [JsonProperty("Ник игрока")]
        public string Name;
        [JsonProperty("Кол-во XP у игрока!")]
        public float XP;
    }

    public class ShopItems
    {
        [JsonProperty("Название предмета")]
        public string ShortName;
        [JsonProperty("Описание")]
        public string Description;
        [JsonProperty("Количество предмета при покупке")]
        public int Amount;
        [JsonProperty("Количество предмета при продаже")]
        public int Amount2;
        [JsonProperty("Цена покупки")]
        public int Price;
        [JsonProperty("Цена продажи")]
        public int Price2;
    }

    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Xp за убийства игроков")]
        public int KillPlayer;
        [JsonProperty("Xp за добычу дерева")]
        public int ExtractionWood;
        [JsonProperty("Xp за добычу камня")]
        public int ExtractionStones;
        [JsonProperty("Xp за добычу металла")]
        public int ExtractionMetall;
        [JsonProperty("Xp за добычу серы")]
        public int ExtractionSulfur;
        [JsonProperty("Xp за добычу металла высокого качества")]
        public int ExtractionMetallHQ;
        [JsonProperty("Предметы доступные для покупки")]
        public List<ShopItems> ShopItems;
        public static Configuration GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("shop")]
    private void ShopXp(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_Page")]
    private void CmdConsolePage(ConsoleSystem.Arg args);
    [ConsoleCommand("item")]
    private void CmdXpBuy(ConsoleSystem.Arg args);
    private Dictionary<ulong, Xp> XPShops;
    private void OnServerInitialized();
    private void OnPlayerInit(BasePlayer player);
     void Unload();
    private void SaveData();
     void OnEntityDeath(BasePlayer player, HitInfo info);
     void OnPlantGather(PlantEntity plant, Item item, BasePlayer player);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void ProcessItem(BasePlayer player, Item item);
    private void DrawUI(BasePlayer player, int page);
    private static string HexToUiColor(string hex);
}

public class Xp
{
    [JsonProperty("Ник игрока")]
    public string Name;
    [JsonProperty("Кол-во XP у игрока!")]
    public float XP;
}

public class ShopItems
{
    [JsonProperty("Название предмета")]
    public string ShortName;
    [JsonProperty("Описание")]
    public string Description;
    [JsonProperty("Количество предмета при покупке")]
    public int Amount;
    [JsonProperty("Количество предмета при продаже")]
    public int Amount2;
    [JsonProperty("Цена покупки")]
    public int Price;
    [JsonProperty("Цена продажи")]
    public int Price2;
}

private class Configuration
{
    [JsonProperty("Xp за убийства игроков")]
    public int KillPlayer;
    [JsonProperty("Xp за добычу дерева")]
    public int ExtractionWood;
    [JsonProperty("Xp за добычу камня")]
    public int ExtractionStones;
    [JsonProperty("Xp за добычу металла")]
    public int ExtractionMetall;
    [JsonProperty("Xp за добычу серы")]
    public int ExtractionSulfur;
    [JsonProperty("Xp за добычу металла высокого качества")]
    public int ExtractionMetallHQ;
    [JsonProperty("Предметы доступные для покупки")]
    public List<ShopItems> ShopItems;
    public static Configuration GetNewCong();
}


```

---

## XPSync

```csharp
using Oxide.Core.Database;
using Oxide.Core;
using Oxide.Ext.MySql;

Oxide.Plugins
[Info("XP Sync", "PaiN", 0.2, ResourceId = 2072)]
 class XPSync : RustPlugin
{
    public static readonly Oxide.Ext.MySql.Libraries.MySql _mySql;
    public static Connection _mySqlConnection;
     ConfigFile Cfg;
     class ConfigFile
    {
        public string Host;
        public int Port;
        public string Database;
        public string User;
        public string Password;
    }

     void Loaded();
     void Unloaded();
    protected override void LoadDefaultConfig();
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
}

 class ConfigFile
{
    public string Host;
    public int Port;
    public string Database;
    public string User;
    public string Password;
}


```

---

## XPSystem

```csharp
using System;
using System.Globalization;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("XPSystem", "http://topplugin.ru/", "0.0.8")]
[Description("Добавляет на сервер XP Систему, благодаря которой, игроки смогут получать баланс в магазине за фарм.")]
 class XPSystem : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private string Layer;
    public ulong lastDamageName;
    private readonly Dictionary<string, int> playerInfo;
    private readonly List<uint> crateInfo;
    private static Configuration Settings;
    private class Configuration
    {
        public class API
        {
            [JsonProperty("APIKey (Секретный ключ)")]
            public string SecretKey;
            [JsonProperty("ShopID (ИД магазина в сервисе)")]
            public string ShopID;
        }

        public class Balance
        {
            [JsonProperty("Сколько рублей на баланс магазина будет выдаваться")]
            public int Money;
        }

        public class Gather
        {
            [JsonProperty("Сколько игроку будет выдаваться XP за подбирание ресурсов")]
            public float CollectiblePickup;
            [JsonProperty("Сколько игроку будет выдаваться XP за подбирание плантации")]
            public float CropGather;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм бочек")]
            public float BarrelGather;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм ящиков")]
            public float CrateGather;
            [JsonProperty("Сколько игроку будет выдаваться XP за убийство животного")]
            public float AnimalGather;
        }

        public class GathersOptions
        {
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм дерева")]
            public float WoodGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм обычного камня stones")]
            public float StonesGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм металлического камня")]
            public float MetalOreGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм серного камня")]
            public float SulfurGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм бонус МВК")]
            public float HQMGathers;
        }

        public class GUI
        {
            [JsonProperty("Разрешение для использования команды /xp")]
            public string XpPermission;
            [JsonProperty("Текст в основном блоке (возле аватарки)")]
            public string MainBlockText;
            [JsonProperty("Текст во втором нижнем блоке")]
            public string SecondBlockText;
        }

        [JsonProperty("Настройки API плагина")]
        public API APISettings;
        [JsonProperty("Настройка кол-ва выдачи баланса игроку")]
        public Balance BalanceSettings;
        [JsonProperty("Настройка кол-ва выдачи XP с фарма ресурсов")]
        public Gather GatherSettings;
        [JsonProperty("Более подробная настройка кол-ва выдачи XP с фарма ресурсов и животных")]
        public GathersOptions GathersSetting;
        [JsonProperty("Настройка интерфейса")]
        public GUI GUISettings;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    public class Xp
    {
        [JsonProperty("Количество XP у игрока:")]
        public float XP { get; set; }
    }

    private Dictionary<ulong, Xp> XPS { get; set; }
     bool LogsPlayer;
     void MoneyPlus(ulong userId, int amount);
     void ApiRequestBalance(Dictionary<string, string> args);
     object OnCollectiblePickup(Item item, BasePlayer player);
     object OnCropGather(GrowableEntity plant, Item item, BasePlayer player);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void ProcessItem(BasePlayer player, Item item);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnServerInitialized();
    private void GrowableEntity(BasePlayer player);
    private void SaveData();
    [ChatCommand("xp")]
     void cmdXp(BasePlayer player, string command, string[] args);
    [ConsoleCommand("xp")]
    private void cmdXp(ConsoleSystem.Arg args);
    [ConsoleCommand("xpsell")]
    private void CmdXpSell(ConsoleSystem.Arg args);
    [ConsoleCommand("xp.give")]
    private void CmdXpgv(ConsoleSystem.Arg args);
    private void DrawGUI(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

private class Configuration
{
    public class API
    {
        [JsonProperty("APIKey (Секретный ключ)")]
        public string SecretKey;
        [JsonProperty("ShopID (ИД магазина в сервисе)")]
        public string ShopID;
    }

    public class Balance
    {
        [JsonProperty("Сколько рублей на баланс магазина будет выдаваться")]
        public int Money;
    }

    public class Gather
    {
        [JsonProperty("Сколько игроку будет выдаваться XP за подбирание ресурсов")]
        public float CollectiblePickup;
        [JsonProperty("Сколько игроку будет выдаваться XP за подбирание плантации")]
        public float CropGather;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм бочек")]
        public float BarrelGather;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм ящиков")]
        public float CrateGather;
        [JsonProperty("Сколько игроку будет выдаваться XP за убийство животного")]
        public float AnimalGather;
    }

    public class GathersOptions
    {
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм дерева")]
        public float WoodGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм обычного камня stones")]
        public float StonesGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм металлического камня")]
        public float MetalOreGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм серного камня")]
        public float SulfurGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм бонус МВК")]
        public float HQMGathers;
    }

    public class GUI
    {
        [JsonProperty("Разрешение для использования команды /xp")]
        public string XpPermission;
        [JsonProperty("Текст в основном блоке (возле аватарки)")]
        public string MainBlockText;
        [JsonProperty("Текст во втором нижнем блоке")]
        public string SecondBlockText;
    }

    [JsonProperty("Настройки API плагина")]
    public API APISettings;
    [JsonProperty("Настройка кол-ва выдачи баланса игроку")]
    public Balance BalanceSettings;
    [JsonProperty("Настройка кол-ва выдачи XP с фарма ресурсов")]
    public Gather GatherSettings;
    [JsonProperty("Более подробная настройка кол-ва выдачи XP с фарма ресурсов и животных")]
    public GathersOptions GathersSetting;
    [JsonProperty("Настройка интерфейса")]
    public GUI GUISettings;
}

public class API
{
    [JsonProperty("APIKey (Секретный ключ)")]
    public string SecretKey;
    [JsonProperty("ShopID (ИД магазина в сервисе)")]
    public string ShopID;
}

public class Balance
{
    [JsonProperty("Сколько рублей на баланс магазина будет выдаваться")]
    public int Money;
}

public class Gather
{
    [JsonProperty("Сколько игроку будет выдаваться XP за подбирание ресурсов")]
    public float CollectiblePickup;
    [JsonProperty("Сколько игроку будет выдаваться XP за подбирание плантации")]
    public float CropGather;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм бочек")]
    public float BarrelGather;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм ящиков")]
    public float CrateGather;
    [JsonProperty("Сколько игроку будет выдаваться XP за убийство животного")]
    public float AnimalGather;
}

public class GathersOptions
{
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм дерева")]
    public float WoodGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм обычного камня stones")]
    public float StonesGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм металлического камня")]
    public float MetalOreGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм серного камня")]
    public float SulfurGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм бонус МВК")]
    public float HQMGathers;
}

public class GUI
{
    [JsonProperty("Разрешение для использования команды /xp")]
    public string XpPermission;
    [JsonProperty("Текст в основном блоке (возле аватарки)")]
    public string MainBlockText;
    [JsonProperty("Текст во втором нижнем блоке")]
    public string SecondBlockText;
}

public class Xp
{
    [JsonProperty("Количество XP у игрока:")]
    public float XP { get; set; }
}


```

---

## XPSystem (1)

```csharp
using System;
using System.Globalization;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("XPSystem", "http://topplugin.ru/", "0.0.8")]
[Description("Добавляет на сервер XP Систему, благодаря которой, игроки смогут получать баланс в магазине за фарм.")]
 class XPSystem : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private string Layer;
    public ulong lastDamageName;
    private readonly Dictionary<string, int> playerInfo;
    private readonly List<uint> crateInfo;
    private static Configuration Settings;
    private class Configuration
    {
        public class API
        {
            [JsonProperty("APIKey (Секретный ключ)")]
            public string SecretKey;
            [JsonProperty("ShopID (ИД магазина в сервисе)")]
            public string ShopID;
        }

        public class Balance
        {
            [JsonProperty("Сколько рублей на баланс магазина будет выдаваться")]
            public int Money;
        }

        public class Gather
        {
            [JsonProperty("Сколько игроку будет выдаваться XP за подбирание ресурсов")]
            public float CollectiblePickup;
            [JsonProperty("Сколько игроку будет выдаваться XP за подбирание плантации")]
            public float CropGather;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм бочек")]
            public float BarrelGather;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм ящиков")]
            public float CrateGather;
            [JsonProperty("Сколько игроку будет выдаваться XP за убийство животного")]
            public float AnimalGather;
        }

        public class GathersOptions
        {
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм дерева")]
            public float WoodGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм обычного камня stones")]
            public float StonesGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм металлического камня")]
            public float MetalOreGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм серного камня")]
            public float SulfurGathers;
            [JsonProperty("Сколько игроку будет выдаваться XP за фарм бонус МВК")]
            public float HQMGathers;
        }

        public class GUI
        {
            [JsonProperty("Разрешение для использования команды /xp")]
            public string XpPermission;
            [JsonProperty("Текст в основном блоке (возле аватарки)")]
            public string MainBlockText;
            [JsonProperty("Текст во втором нижнем блоке")]
            public string SecondBlockText;
        }

        [JsonProperty("Настройки API плагина")]
        public API APISettings;
        [JsonProperty("Настройка кол-ва выдачи баланса игроку")]
        public Balance BalanceSettings;
        [JsonProperty("Настройка кол-ва выдачи XP с фарма ресурсов")]
        public Gather GatherSettings;
        [JsonProperty("Более подробная настройка кол-ва выдачи XP с фарма ресурсов и животных")]
        public GathersOptions GathersSetting;
        [JsonProperty("Настройка интерфейса")]
        public GUI GUISettings;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    public class Xp
    {
        [JsonProperty("Количество XP у игрока:")]
        public float XP { get; set; }
    }

    private Dictionary<ulong, Xp> XPS { get; set; }
     bool LogsPlayer;
     void MoneyPlus(ulong userId, int amount);
     void ApiRequestBalance(Dictionary<string, string> args);
     object OnCollectiblePickup(Item item, BasePlayer player);
     object OnCropGather(GrowableEntity plant, Item item, BasePlayer player);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void ProcessItem(BasePlayer player, Item item);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnServerInitialized();
    private void GrowableEntity(BasePlayer player);
    private void SaveData();
    [ChatCommand("xp")]
     void cmdXp(BasePlayer player, string command, string[] args);
    [ConsoleCommand("xp")]
    private void cmdXp(ConsoleSystem.Arg args);
    [ConsoleCommand("xpsell")]
    private void CmdXpSell(ConsoleSystem.Arg args);
    [ConsoleCommand("xp.give")]
    private void CmdXpgv(ConsoleSystem.Arg args);
    private void DrawGUI(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

private class Configuration
{
    public class API
    {
        [JsonProperty("APIKey (Секретный ключ)")]
        public string SecretKey;
        [JsonProperty("ShopID (ИД магазина в сервисе)")]
        public string ShopID;
    }

    public class Balance
    {
        [JsonProperty("Сколько рублей на баланс магазина будет выдаваться")]
        public int Money;
    }

    public class Gather
    {
        [JsonProperty("Сколько игроку будет выдаваться XP за подбирание ресурсов")]
        public float CollectiblePickup;
        [JsonProperty("Сколько игроку будет выдаваться XP за подбирание плантации")]
        public float CropGather;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм бочек")]
        public float BarrelGather;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм ящиков")]
        public float CrateGather;
        [JsonProperty("Сколько игроку будет выдаваться XP за убийство животного")]
        public float AnimalGather;
    }

    public class GathersOptions
    {
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм дерева")]
        public float WoodGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм обычного камня stones")]
        public float StonesGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм металлического камня")]
        public float MetalOreGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм серного камня")]
        public float SulfurGathers;
        [JsonProperty("Сколько игроку будет выдаваться XP за фарм бонус МВК")]
        public float HQMGathers;
    }

    public class GUI
    {
        [JsonProperty("Разрешение для использования команды /xp")]
        public string XpPermission;
        [JsonProperty("Текст в основном блоке (возле аватарки)")]
        public string MainBlockText;
        [JsonProperty("Текст во втором нижнем блоке")]
        public string SecondBlockText;
    }

    [JsonProperty("Настройки API плагина")]
    public API APISettings;
    [JsonProperty("Настройка кол-ва выдачи баланса игроку")]
    public Balance BalanceSettings;
    [JsonProperty("Настройка кол-ва выдачи XP с фарма ресурсов")]
    public Gather GatherSettings;
    [JsonProperty("Более подробная настройка кол-ва выдачи XP с фарма ресурсов и животных")]
    public GathersOptions GathersSetting;
    [JsonProperty("Настройка интерфейса")]
    public GUI GUISettings;
}

public class API
{
    [JsonProperty("APIKey (Секретный ключ)")]
    public string SecretKey;
    [JsonProperty("ShopID (ИД магазина в сервисе)")]
    public string ShopID;
}

public class Balance
{
    [JsonProperty("Сколько рублей на баланс магазина будет выдаваться")]
    public int Money;
}

public class Gather
{
    [JsonProperty("Сколько игроку будет выдаваться XP за подбирание ресурсов")]
    public float CollectiblePickup;
    [JsonProperty("Сколько игроку будет выдаваться XP за подбирание плантации")]
    public float CropGather;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм бочек")]
    public float BarrelGather;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм ящиков")]
    public float CrateGather;
    [JsonProperty("Сколько игроку будет выдаваться XP за убийство животного")]
    public float AnimalGather;
}

public class GathersOptions
{
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм дерева")]
    public float WoodGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм обычного камня stones")]
    public float StonesGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм металлического камня")]
    public float MetalOreGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм серного камня")]
    public float SulfurGathers;
    [JsonProperty("Сколько игроку будет выдаваться XP за фарм бонус МВК")]
    public float HQMGathers;
}

public class GUI
{
    [JsonProperty("Разрешение для использования команды /xp")]
    public string XpPermission;
    [JsonProperty("Текст в основном блоке (возле аватарки)")]
    public string MainBlockText;
    [JsonProperty("Текст во втором нижнем блоке")]
    public string SecondBlockText;
}

public class Xp
{
    [JsonProperty("Количество XP у игрока:")]
    public float XP { get; set; }
}


```

---

## XRaidProtection

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Oxide.Core.Plugins;
using System.Globalization;

Oxide.Plugins
[Info("XRaidProtection", "discord.gg/9vyTXsJyKR", "1.0.9")]
 class XRaidProtection : RustPlugin
{
    [PluginReference]
    private Plugin IQChat;
    private RaidConfig config;
    private class RaidConfig
    {
        internal class MessageSetting
        {
            [JsonProperty("Интервал между сообщениями. Мин - 10 сек")]
            public int TimeMessage;
        }

        internal class TimeSetting
        {
            [JsonProperty("Начало защиты по МСК | Часы")]
            public int HourStart;
            [JsonProperty("Начало защиты по МСК | Минуты")]
            public int MinuteStart;
            [JsonProperty("Конец защиты по МСК | Часы")]
            public int HourEnd;
            [JsonProperty("Конец защиты по МСК | Минуты")]
            public int MinuteEnd;
        }

        internal class Settings
        {
            [JsonProperty("Звуковой эффект")]
            public string Effect;
            [JsonProperty("Процент защиты для всех игроков. 1.0 - 100%")]
            public float Damage;
            [JsonProperty("Включить звуковой эффект")]
            public bool TEffect;
            [JsonProperty("Первые N дни активности защиты после вайпа")]
            public int PDays;
            [JsonProperty("Защита только в первые N дней после вайпа")]
            public bool PDay;
            [JsonProperty("Включить GUI сообщение")]
            public bool TGUIMessage;
            [JsonProperty("Включить чат сообщение")]
            public bool TMessage;
            [JsonProperty("0 - Защита только для игроков с пермишеном, 1 - Защита для всех игроков, 2 - Защита для игроков с пермишеном и для всех игроков")]
            public int TypeProtection;
        }

        internal class GUISettings
        {
            [JsonProperty("AnchorMin")]
            public string AnchorMin;
            [JsonProperty("AnchorMax")]
            public string AnchorMax;
            [JsonProperty("OffsetMin")]
            public string OffsetMin;
            [JsonProperty("OffsetMax")]
            public string OffsetMax;
            [JsonProperty("Цвет текста")]
            public string ColorText;
            [JsonProperty("Размер текста")]
            public int SizeText;
            [JsonProperty("Использовать иконки")]
            public bool Icon;
        }

        internal class PrefabSetting
        {
            [JsonProperty("Префабы")]
            public List<string> Prefabs;
        }

        internal class PermisssionSetting
        {
            [JsonProperty("Процент защиты по пермишену")]
            public float PermisssionDamage;
        }

        [JsonProperty("Сообщения в чат и GUI")]
        public MessageSetting Message;
        [JsonProperty("Настройка времени действия защиты")]
        public TimeSetting Time;
        [JsonProperty("Общее")]
        public Settings Setting;
        [JsonProperty("Настройка GUI")]
        public GUISettings GUI;
        [JsonProperty("Список префабов которые будут под защитой")]
        public PrefabSetting Prefab;
        [JsonProperty("Настройка пермишенов")]
        public Dictionary<string, PermisssionSetting> Permisssion;
        [JsonProperty("Дата вайпа")]
        public string DateWipe;
        [JsonProperty("Время по МСК")]
        public DateTime MSCTime;
        public static RaidConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void OnServerSave();
    private void MSC();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void Protection(BaseCombatEntity entity, HitInfo info, float damage);
    private Dictionary<BasePlayer, DateTime> Cooldowns;
    private bool Time();
    private void Message(BasePlayer player);
    private void InitializeLang();
    private void IQChatPuts(BasePlayer player, string Message);
}

private class RaidConfig
{
    internal class MessageSetting
    {
        [JsonProperty("Интервал между сообщениями. Мин - 10 сек")]
        public int TimeMessage;
    }

    internal class TimeSetting
    {
        [JsonProperty("Начало защиты по МСК | Часы")]
        public int HourStart;
        [JsonProperty("Начало защиты по МСК | Минуты")]
        public int MinuteStart;
        [JsonProperty("Конец защиты по МСК | Часы")]
        public int HourEnd;
        [JsonProperty("Конец защиты по МСК | Минуты")]
        public int MinuteEnd;
    }

    internal class Settings
    {
        [JsonProperty("Звуковой эффект")]
        public string Effect;
        [JsonProperty("Процент защиты для всех игроков. 1.0 - 100%")]
        public float Damage;
        [JsonProperty("Включить звуковой эффект")]
        public bool TEffect;
        [JsonProperty("Первые N дни активности защиты после вайпа")]
        public int PDays;
        [JsonProperty("Защита только в первые N дней после вайпа")]
        public bool PDay;
        [JsonProperty("Включить GUI сообщение")]
        public bool TGUIMessage;
        [JsonProperty("Включить чат сообщение")]
        public bool TMessage;
        [JsonProperty("0 - Защита только для игроков с пермишеном, 1 - Защита для всех игроков, 2 - Защита для игроков с пермишеном и для всех игроков")]
        public int TypeProtection;
    }

    internal class GUISettings
    {
        [JsonProperty("AnchorMin")]
        public string AnchorMin;
        [JsonProperty("AnchorMax")]
        public string AnchorMax;
        [JsonProperty("OffsetMin")]
        public string OffsetMin;
        [JsonProperty("OffsetMax")]
        public string OffsetMax;
        [JsonProperty("Цвет текста")]
        public string ColorText;
        [JsonProperty("Размер текста")]
        public int SizeText;
        [JsonProperty("Использовать иконки")]
        public bool Icon;
    }

    internal class PrefabSetting
    {
        [JsonProperty("Префабы")]
        public List<string> Prefabs;
    }

    internal class PermisssionSetting
    {
        [JsonProperty("Процент защиты по пермишену")]
        public float PermisssionDamage;
    }

    [JsonProperty("Сообщения в чат и GUI")]
    public MessageSetting Message;
    [JsonProperty("Настройка времени действия защиты")]
    public TimeSetting Time;
    [JsonProperty("Общее")]
    public Settings Setting;
    [JsonProperty("Настройка GUI")]
    public GUISettings GUI;
    [JsonProperty("Список префабов которые будут под защитой")]
    public PrefabSetting Prefab;
    [JsonProperty("Настройка пермишенов")]
    public Dictionary<string, PermisssionSetting> Permisssion;
    [JsonProperty("Дата вайпа")]
    public string DateWipe;
    [JsonProperty("Время по МСК")]
    public DateTime MSCTime;
    public static RaidConfig GetNewConfiguration();
}

internal class MessageSetting
{
    [JsonProperty("Интервал между сообщениями. Мин - 10 сек")]
    public int TimeMessage;
}

internal class TimeSetting
{
    [JsonProperty("Начало защиты по МСК | Часы")]
    public int HourStart;
    [JsonProperty("Начало защиты по МСК | Минуты")]
    public int MinuteStart;
    [JsonProperty("Конец защиты по МСК | Часы")]
    public int HourEnd;
    [JsonProperty("Конец защиты по МСК | Минуты")]
    public int MinuteEnd;
}

internal class Settings
{
    [JsonProperty("Звуковой эффект")]
    public string Effect;
    [JsonProperty("Процент защиты для всех игроков. 1.0 - 100%")]
    public float Damage;
    [JsonProperty("Включить звуковой эффект")]
    public bool TEffect;
    [JsonProperty("Первые N дни активности защиты после вайпа")]
    public int PDays;
    [JsonProperty("Защита только в первые N дней после вайпа")]
    public bool PDay;
    [JsonProperty("Включить GUI сообщение")]
    public bool TGUIMessage;
    [JsonProperty("Включить чат сообщение")]
    public bool TMessage;
    [JsonProperty("0 - Защита только для игроков с пермишеном, 1 - Защита для всех игроков, 2 - Защита для игроков с пермишеном и для всех игроков")]
    public int TypeProtection;
}

internal class GUISettings
{
    [JsonProperty("AnchorMin")]
    public string AnchorMin;
    [JsonProperty("AnchorMax")]
    public string AnchorMax;
    [JsonProperty("OffsetMin")]
    public string OffsetMin;
    [JsonProperty("OffsetMax")]
    public string OffsetMax;
    [JsonProperty("Цвет текста")]
    public string ColorText;
    [JsonProperty("Размер текста")]
    public int SizeText;
    [JsonProperty("Использовать иконки")]
    public bool Icon;
}

internal class PrefabSetting
{
    [JsonProperty("Префабы")]
    public List<string> Prefabs;
}

internal class PermisssionSetting
{
    [JsonProperty("Процент защиты по пермишену")]
    public float PermisssionDamage;
}


```

---

## XRaidProtection-1.2.1

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("XRaidProtection", "Monster", "1.2.1")]
 class XRaidProtection : RustPlugin
{
    private const bool LanguageEnglish;
    [PluginReference]
    private Plugin IQChat;
    private Plugin RaidableBases;
    private Plugin AbandonedBases;
    private Plugin Convoy;
    private RaidConfig config;
    private class RaidConfig
    {
        internal class MessageSetting
        {
            [JsonProperty(LanguageEnglish ? "Interval between messages. Min - 10 sec" : "Интервал между сообщениями. Мин - 10 сек")]
            public int TimeMessage;
        }

        internal class TimeSetting
        {
            [JsonProperty(LanguageEnglish ? "Start protection | Hour" : "Начало защиты | Часы")]
            public int HourStart;
            [JsonProperty(LanguageEnglish ? "Start protection | Minute" : "Начало защиты | Минуты")]
            public int MinuteStart;
            [JsonProperty(LanguageEnglish ? "End protection | Hour" : "Конец защиты | Часы")]
            public int HourEnd;
            [JsonProperty(LanguageEnglish ? "End protection | Minute" : "Конец защиты | Минуты")]
            public int MinuteEnd;
            [JsonProperty(LanguageEnglish ? "Timezone - UTC+0:00" : "Часовой пояс - UTC+0:00")]
            public int GMT;
            [JsonProperty(LanguageEnglish ? "Protection activity check timer (.sec)" : "Время таймера проверки активности защиты (.сек)")]
            public int Timer;
            [JsonProperty(LanguageEnglish ? "Use local computer/hosting time. [ Requests to an external time service and time zone will be disabled ]" : "Использовать локальное время компьютера/хостинга. [ Запросы к внешнему сервису точного времени и часовой пояс будут отключены ]")]
            public bool LocalTime;
        }

        internal class Settings
        {
            [JsonProperty(LanguageEnglish ? "Sound effect" : "Звуковой эффект")]
            public string Effect;
            [JsonProperty(LanguageEnglish ? "Protection percentage for all players. 1.0 - 100%" : "Процент защиты для всех игроков. 1.0 - 100%")]
            public float Damage;
            [JsonProperty(LanguageEnglish ? "Protection percentage for all vehicles. 1.0 - 100%" : "Процент защиты для всех транспортных средств. 1.0 - 100%")]
            public float DamageV;
            [JsonProperty(LanguageEnglish ? "Protected all vehicles" : "Защита всех транспортных средств")]
            public bool ProtectV;
            [JsonProperty(LanguageEnglish ? "Enable sound effect" : "Включить звуковой эффект")]
            public bool TEffect;
            [JsonProperty(LanguageEnglish ? "Protection activity only in the first days of the wipe" : "Активность защиты только в первые дни вайпа")]
            public bool PDay;
            [JsonProperty(LanguageEnglish ? "How protection will work in the early days of the wipe - [ True - all day | False - only in the specified time range ]" : "Как будет работать защита в первые дни вайпа - [ True - круглые сутки | False - только в указанном временном диапазоне ]")]
            public bool PDayO;
            [JsonProperty(LanguageEnglish ? "How many first days after the wipe will be active protection" : "Сколько первых дней после вайпа будет активной защита")]
            public int PDays;
            [JsonProperty(LanguageEnglish ? "Percentage of protection by day, only at ( Protection activity only in the first days of the wipe ). If the list is empty or no day is specified, the protection percentage will be taken from ( Protection percentage for all players. 1.0 - 100% )" : "Процент защиты по дням, только при ( Активность защиты только в первые дни вайпа ). Если список пуст или не указан день, процент защиты будет взят из параметра ( Процент защиты для всех игроков. 1.0 - 100% )")]
            public Dictionary<int, float> ProtectDays;
            [JsonProperty(LanguageEnglish ? "Enable GUI message" : "Включить GUI сообщение")]
            public bool TGUIMessage;
            [JsonProperty(LanguageEnglish ? "Enable chat message" : "Включить чат сообщение")]
            public bool TMessage;
            [JsonProperty(LanguageEnglish ? "Allow breaking twigs during defense" : "Разрешить ломать солому во время защиты")]
            public bool Twigs;
            [JsonProperty(LanguageEnglish ? "Enable protection for players with permission when the main protect is not active" : "Включить защиту для игроков с разрешением, когда основная защита не активна")]
            public bool PermProtect;
            [JsonProperty(LanguageEnglish ? "Enable protection against damage from helicopters, MLRS, submarines, etc. [ Excluding rot damage ]" : "Включить защиту от урона вертолета, МЛРС, подводных лодок и т.д. [ Кроме урона от гниения ]")]
            public bool MoreDamage;
            [JsonProperty(LanguageEnglish ? "0 - Protection only for players with permission, 1 - Protection for all players, 2 - Protection for players with permission and for all players" : "0 - Защита только для игроков с пермишеном, 1 - Защита для всех игроков, 2 - Защита для игроков с пермишеном и для всех игроков")]
            public int TypeProtection;
            [JsonProperty(LanguageEnglish ? "What days of the week protection can be active - [ Does not apply to protection in the early days of the wipe ] - [ Sun - 1, Mon - 2, Tue - 3, Wed - 4, Thu - 5, Fri - 6, Sat - 7 ]" : "В какие дни недели защита может быть активной - [ Не распространяется на защиту в первые дни вайпа ] - [ Вс - 1, Пн - 2, Вт - 3, Ср - 4, Чт - 5, Пт - 6, Сб - 7 ]")]
            public int[] DayOfWeekProtection;
        }

        internal class GUISettings
        {
            [JsonProperty(LanguageEnglish ? "AnchorMin [ When damage is dealt ]" : "AnchorMin [ Когда наносится урон ]")]
            public string AnchorMinD;
            [JsonProperty(LanguageEnglish ? "AnchorMax [ When damage is dealt ]" : "AnchorMax [ Когда наносится урон ]")]
            public string AnchorMaxD;
            [JsonProperty(LanguageEnglish ? "OffsetMin [ When damage is dealt ]" : "OffsetMin [ Когда наносится урон ]")]
            public string OffsetMinD;
            [JsonProperty(LanguageEnglish ? "OffsetMax [ When damage is dealt ]" : "OffsetMax [ Когда наносится урон ]")]
            public string OffsetMaxD;
            [JsonProperty(LanguageEnglish ? "Use icons [ When damage is dealt ]" : "Использовать иконки [ Когда наносится урон ]")]
            public bool IconD;
            [JsonProperty(LanguageEnglish ? "AnchorMin [ Status is always displayed ]" : "AnchorMin [ Статус всегда отображается ]")]
            public string AnchorMinA;
            [JsonProperty(LanguageEnglish ? "AnchorMax [ Status is always displayed ]" : "AnchorMax [ Статус всегда отображается ]")]
            public string AnchorMaxA;
            [JsonProperty(LanguageEnglish ? "OffsetMin [ Status is always displayed ]" : "OffsetMin [ Статус всегда отображается ]")]
            public string OffsetMinA;
            [JsonProperty(LanguageEnglish ? "OffsetMax [ Status is always displayed ]" : "OffsetMax [ Статус всегда отображается ]")]
            public string OffsetMaxA;
            [JsonProperty(LanguageEnglish ? "Use icons [ Status is always displayed ]" : "Использовать иконки [ Статус всегда отображается ]")]
            public bool IconA;
            [JsonProperty(LanguageEnglish ? "Text color" : "Цвет текста")]
            public string ColorText;
            [JsonProperty(LanguageEnglish ? "Text size" : "Размер текста")]
            public int SizeText;
            [JsonProperty(LanguageEnglish ? "Always display UI when protection is active" : "Постоянно отображать UI при активной защите")]
            public bool ActiveUI;
            [JsonProperty(LanguageEnglish ? "Always display UI when protection is inactive" : "Постоянно отображать UI при неактивной защите")]
            public bool DeactiveUI;
        }

        [JsonProperty(LanguageEnglish ? "Messages in chat and GUI" : "Сообщения в чат и GUI")]
        public MessageSetting Message;
        [JsonProperty(LanguageEnglish ? "Setting the protection duration" : "Настройка времени действия защиты")]
        public TimeSetting Time;
        [JsonProperty(LanguageEnglish ? "General settings" : "Общее")]
        public Settings Setting;
        [JsonProperty(LanguageEnglish ? "GUI settings" : "Настройка GUI")]
        public GUISettings GUI;
        [JsonProperty(LanguageEnglish ? "List of prefabs that will be protected" : "Список префабов которые будут под защитой")]
        public List<string> Prefabs;
        [JsonProperty(LanguageEnglish ? "Setting permissions - [ Permisssion | The percentage of protection by permission. 1.0 - 100% ]" : "Настройка пермишенов - [ Пермишен | Процент защиты по пермишену. 1.0 - 100% ]")]
        public Dictionary<string, float> Permisssion;
        [JsonProperty(LanguageEnglish ? "Wipe date" : "Дата вайпа")]
        public string DateWipe;
        [JsonProperty(LanguageEnglish ? "Time" : "Время")]
        public DateTime MSCTime;
        public static RaidConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("protect")]
     void cmdProtection(BasePlayer player);
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void MSC();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void Protection(BaseCombatEntity entity, HitInfo info, float damage);
    private bool EventTerritory(Vector3 entityPos);
    private Dictionary<BasePlayer, DateTime> Cooldowns;
     TimeSpan On;
     TimeSpan Off;
     bool _time;
     int _day;
    private bool Time();
    private bool IsRange();
    private void Message(BasePlayer player, string s_damage, bool destroy);
    private void InitializeLang();
    private void IQChatPuts(BasePlayer player, string Message);
}

private class RaidConfig
{
    internal class MessageSetting
    {
        [JsonProperty(LanguageEnglish ? "Interval between messages. Min - 10 sec" : "Интервал между сообщениями. Мин - 10 сек")]
        public int TimeMessage;
    }

    internal class TimeSetting
    {
        [JsonProperty(LanguageEnglish ? "Start protection | Hour" : "Начало защиты | Часы")]
        public int HourStart;
        [JsonProperty(LanguageEnglish ? "Start protection | Minute" : "Начало защиты | Минуты")]
        public int MinuteStart;
        [JsonProperty(LanguageEnglish ? "End protection | Hour" : "Конец защиты | Часы")]
        public int HourEnd;
        [JsonProperty(LanguageEnglish ? "End protection | Minute" : "Конец защиты | Минуты")]
        public int MinuteEnd;
        [JsonProperty(LanguageEnglish ? "Timezone - UTC+0:00" : "Часовой пояс - UTC+0:00")]
        public int GMT;
        [JsonProperty(LanguageEnglish ? "Protection activity check timer (.sec)" : "Время таймера проверки активности защиты (.сек)")]
        public int Timer;
        [JsonProperty(LanguageEnglish ? "Use local computer/hosting time. [ Requests to an external time service and time zone will be disabled ]" : "Использовать локальное время компьютера/хостинга. [ Запросы к внешнему сервису точного времени и часовой пояс будут отключены ]")]
        public bool LocalTime;
    }

    internal class Settings
    {
        [JsonProperty(LanguageEnglish ? "Sound effect" : "Звуковой эффект")]
        public string Effect;
        [JsonProperty(LanguageEnglish ? "Protection percentage for all players. 1.0 - 100%" : "Процент защиты для всех игроков. 1.0 - 100%")]
        public float Damage;
        [JsonProperty(LanguageEnglish ? "Protection percentage for all vehicles. 1.0 - 100%" : "Процент защиты для всех транспортных средств. 1.0 - 100%")]
        public float DamageV;
        [JsonProperty(LanguageEnglish ? "Protected all vehicles" : "Защита всех транспортных средств")]
        public bool ProtectV;
        [JsonProperty(LanguageEnglish ? "Enable sound effect" : "Включить звуковой эффект")]
        public bool TEffect;
        [JsonProperty(LanguageEnglish ? "Protection activity only in the first days of the wipe" : "Активность защиты только в первые дни вайпа")]
        public bool PDay;
        [JsonProperty(LanguageEnglish ? "How protection will work in the early days of the wipe - [ True - all day | False - only in the specified time range ]" : "Как будет работать защита в первые дни вайпа - [ True - круглые сутки | False - только в указанном временном диапазоне ]")]
        public bool PDayO;
        [JsonProperty(LanguageEnglish ? "How many first days after the wipe will be active protection" : "Сколько первых дней после вайпа будет активной защита")]
        public int PDays;
        [JsonProperty(LanguageEnglish ? "Percentage of protection by day, only at ( Protection activity only in the first days of the wipe ). If the list is empty or no day is specified, the protection percentage will be taken from ( Protection percentage for all players. 1.0 - 100% )" : "Процент защиты по дням, только при ( Активность защиты только в первые дни вайпа ). Если список пуст или не указан день, процент защиты будет взят из параметра ( Процент защиты для всех игроков. 1.0 - 100% )")]
        public Dictionary<int, float> ProtectDays;
        [JsonProperty(LanguageEnglish ? "Enable GUI message" : "Включить GUI сообщение")]
        public bool TGUIMessage;
        [JsonProperty(LanguageEnglish ? "Enable chat message" : "Включить чат сообщение")]
        public bool TMessage;
        [JsonProperty(LanguageEnglish ? "Allow breaking twigs during defense" : "Разрешить ломать солому во время защиты")]
        public bool Twigs;
        [JsonProperty(LanguageEnglish ? "Enable protection for players with permission when the main protect is not active" : "Включить защиту для игроков с разрешением, когда основная защита не активна")]
        public bool PermProtect;
        [JsonProperty(LanguageEnglish ? "Enable protection against damage from helicopters, MLRS, submarines, etc. [ Excluding rot damage ]" : "Включить защиту от урона вертолета, МЛРС, подводных лодок и т.д. [ Кроме урона от гниения ]")]
        public bool MoreDamage;
        [JsonProperty(LanguageEnglish ? "0 - Protection only for players with permission, 1 - Protection for all players, 2 - Protection for players with permission and for all players" : "0 - Защита только для игроков с пермишеном, 1 - Защита для всех игроков, 2 - Защита для игроков с пермишеном и для всех игроков")]
        public int TypeProtection;
        [JsonProperty(LanguageEnglish ? "What days of the week protection can be active - [ Does not apply to protection in the early days of the wipe ] - [ Sun - 1, Mon - 2, Tue - 3, Wed - 4, Thu - 5, Fri - 6, Sat - 7 ]" : "В какие дни недели защита может быть активной - [ Не распространяется на защиту в первые дни вайпа ] - [ Вс - 1, Пн - 2, Вт - 3, Ср - 4, Чт - 5, Пт - 6, Сб - 7 ]")]
        public int[] DayOfWeekProtection;
    }

    internal class GUISettings
    {
        [JsonProperty(LanguageEnglish ? "AnchorMin [ When damage is dealt ]" : "AnchorMin [ Когда наносится урон ]")]
        public string AnchorMinD;
        [JsonProperty(LanguageEnglish ? "AnchorMax [ When damage is dealt ]" : "AnchorMax [ Когда наносится урон ]")]
        public string AnchorMaxD;
        [JsonProperty(LanguageEnglish ? "OffsetMin [ When damage is dealt ]" : "OffsetMin [ Когда наносится урон ]")]
        public string OffsetMinD;
        [JsonProperty(LanguageEnglish ? "OffsetMax [ When damage is dealt ]" : "OffsetMax [ Когда наносится урон ]")]
        public string OffsetMaxD;
        [JsonProperty(LanguageEnglish ? "Use icons [ When damage is dealt ]" : "Использовать иконки [ Когда наносится урон ]")]
        public bool IconD;
        [JsonProperty(LanguageEnglish ? "AnchorMin [ Status is always displayed ]" : "AnchorMin [ Статус всегда отображается ]")]
        public string AnchorMinA;
        [JsonProperty(LanguageEnglish ? "AnchorMax [ Status is always displayed ]" : "AnchorMax [ Статус всегда отображается ]")]
        public string AnchorMaxA;
        [JsonProperty(LanguageEnglish ? "OffsetMin [ Status is always displayed ]" : "OffsetMin [ Статус всегда отображается ]")]
        public string OffsetMinA;
        [JsonProperty(LanguageEnglish ? "OffsetMax [ Status is always displayed ]" : "OffsetMax [ Статус всегда отображается ]")]
        public string OffsetMaxA;
        [JsonProperty(LanguageEnglish ? "Use icons [ Status is always displayed ]" : "Использовать иконки [ Статус всегда отображается ]")]
        public bool IconA;
        [JsonProperty(LanguageEnglish ? "Text color" : "Цвет текста")]
        public string ColorText;
        [JsonProperty(LanguageEnglish ? "Text size" : "Размер текста")]
        public int SizeText;
        [JsonProperty(LanguageEnglish ? "Always display UI when protection is active" : "Постоянно отображать UI при активной защите")]
        public bool ActiveUI;
        [JsonProperty(LanguageEnglish ? "Always display UI when protection is inactive" : "Постоянно отображать UI при неактивной защите")]
        public bool DeactiveUI;
    }

    [JsonProperty(LanguageEnglish ? "Messages in chat and GUI" : "Сообщения в чат и GUI")]
    public MessageSetting Message;
    [JsonProperty(LanguageEnglish ? "Setting the protection duration" : "Настройка времени действия защиты")]
    public TimeSetting Time;
    [JsonProperty(LanguageEnglish ? "General settings" : "Общее")]
    public Settings Setting;
    [JsonProperty(LanguageEnglish ? "GUI settings" : "Настройка GUI")]
    public GUISettings GUI;
    [JsonProperty(LanguageEnglish ? "List of prefabs that will be protected" : "Список префабов которые будут под защитой")]
    public List<string> Prefabs;
    [JsonProperty(LanguageEnglish ? "Setting permissions - [ Permisssion | The percentage of protection by permission. 1.0 - 100% ]" : "Настройка пермишенов - [ Пермишен | Процент защиты по пермишену. 1.0 - 100% ]")]
    public Dictionary<string, float> Permisssion;
    [JsonProperty(LanguageEnglish ? "Wipe date" : "Дата вайпа")]
    public string DateWipe;
    [JsonProperty(LanguageEnglish ? "Time" : "Время")]
    public DateTime MSCTime;
    public static RaidConfig GetNewConfiguration();
}

internal class MessageSetting
{
    [JsonProperty(LanguageEnglish ? "Interval between messages. Min - 10 sec" : "Интервал между сообщениями. Мин - 10 сек")]
    public int TimeMessage;
}

internal class TimeSetting
{
    [JsonProperty(LanguageEnglish ? "Start protection | Hour" : "Начало защиты | Часы")]
    public int HourStart;
    [JsonProperty(LanguageEnglish ? "Start protection | Minute" : "Начало защиты | Минуты")]
    public int MinuteStart;
    [JsonProperty(LanguageEnglish ? "End protection | Hour" : "Конец защиты | Часы")]
    public int HourEnd;
    [JsonProperty(LanguageEnglish ? "End protection | Minute" : "Конец защиты | Минуты")]
    public int MinuteEnd;
    [JsonProperty(LanguageEnglish ? "Timezone - UTC+0:00" : "Часовой пояс - UTC+0:00")]
    public int GMT;
    [JsonProperty(LanguageEnglish ? "Protection activity check timer (.sec)" : "Время таймера проверки активности защиты (.сек)")]
    public int Timer;
    [JsonProperty(LanguageEnglish ? "Use local computer/hosting time. [ Requests to an external time service and time zone will be disabled ]" : "Использовать локальное время компьютера/хостинга. [ Запросы к внешнему сервису точного времени и часовой пояс будут отключены ]")]
    public bool LocalTime;
}

internal class Settings
{
    [JsonProperty(LanguageEnglish ? "Sound effect" : "Звуковой эффект")]
    public string Effect;
    [JsonProperty(LanguageEnglish ? "Protection percentage for all players. 1.0 - 100%" : "Процент защиты для всех игроков. 1.0 - 100%")]
    public float Damage;
    [JsonProperty(LanguageEnglish ? "Protection percentage for all vehicles. 1.0 - 100%" : "Процент защиты для всех транспортных средств. 1.0 - 100%")]
    public float DamageV;
    [JsonProperty(LanguageEnglish ? "Protected all vehicles" : "Защита всех транспортных средств")]
    public bool ProtectV;
    [JsonProperty(LanguageEnglish ? "Enable sound effect" : "Включить звуковой эффект")]
    public bool TEffect;
    [JsonProperty(LanguageEnglish ? "Protection activity only in the first days of the wipe" : "Активность защиты только в первые дни вайпа")]
    public bool PDay;
    [JsonProperty(LanguageEnglish ? "How protection will work in the early days of the wipe - [ True - all day | False - only in the specified time range ]" : "Как будет работать защита в первые дни вайпа - [ True - круглые сутки | False - только в указанном временном диапазоне ]")]
    public bool PDayO;
    [JsonProperty(LanguageEnglish ? "How many first days after the wipe will be active protection" : "Сколько первых дней после вайпа будет активной защита")]
    public int PDays;
    [JsonProperty(LanguageEnglish ? "Percentage of protection by day, only at ( Protection activity only in the first days of the wipe ). If the list is empty or no day is specified, the protection percentage will be taken from ( Protection percentage for all players. 1.0 - 100% )" : "Процент защиты по дням, только при ( Активность защиты только в первые дни вайпа ). Если список пуст или не указан день, процент защиты будет взят из параметра ( Процент защиты для всех игроков. 1.0 - 100% )")]
    public Dictionary<int, float> ProtectDays;
    [JsonProperty(LanguageEnglish ? "Enable GUI message" : "Включить GUI сообщение")]
    public bool TGUIMessage;
    [JsonProperty(LanguageEnglish ? "Enable chat message" : "Включить чат сообщение")]
    public bool TMessage;
    [JsonProperty(LanguageEnglish ? "Allow breaking twigs during defense" : "Разрешить ломать солому во время защиты")]
    public bool Twigs;
    [JsonProperty(LanguageEnglish ? "Enable protection for players with permission when the main protect is not active" : "Включить защиту для игроков с разрешением, когда основная защита не активна")]
    public bool PermProtect;
    [JsonProperty(LanguageEnglish ? "Enable protection against damage from helicopters, MLRS, submarines, etc. [ Excluding rot damage ]" : "Включить защиту от урона вертолета, МЛРС, подводных лодок и т.д. [ Кроме урона от гниения ]")]
    public bool MoreDamage;
    [JsonProperty(LanguageEnglish ? "0 - Protection only for players with permission, 1 - Protection for all players, 2 - Protection for players with permission and for all players" : "0 - Защита только для игроков с пермишеном, 1 - Защита для всех игроков, 2 - Защита для игроков с пермишеном и для всех игроков")]
    public int TypeProtection;
    [JsonProperty(LanguageEnglish ? "What days of the week protection can be active - [ Does not apply to protection in the early days of the wipe ] - [ Sun - 1, Mon - 2, Tue - 3, Wed - 4, Thu - 5, Fri - 6, Sat - 7 ]" : "В какие дни недели защита может быть активной - [ Не распространяется на защиту в первые дни вайпа ] - [ Вс - 1, Пн - 2, Вт - 3, Ср - 4, Чт - 5, Пт - 6, Сб - 7 ]")]
    public int[] DayOfWeekProtection;
}

internal class GUISettings
{
    [JsonProperty(LanguageEnglish ? "AnchorMin [ When damage is dealt ]" : "AnchorMin [ Когда наносится урон ]")]
    public string AnchorMinD;
    [JsonProperty(LanguageEnglish ? "AnchorMax [ When damage is dealt ]" : "AnchorMax [ Когда наносится урон ]")]
    public string AnchorMaxD;
    [JsonProperty(LanguageEnglish ? "OffsetMin [ When damage is dealt ]" : "OffsetMin [ Когда наносится урон ]")]
    public string OffsetMinD;
    [JsonProperty(LanguageEnglish ? "OffsetMax [ When damage is dealt ]" : "OffsetMax [ Когда наносится урон ]")]
    public string OffsetMaxD;
    [JsonProperty(LanguageEnglish ? "Use icons [ When damage is dealt ]" : "Использовать иконки [ Когда наносится урон ]")]
    public bool IconD;
    [JsonProperty(LanguageEnglish ? "AnchorMin [ Status is always displayed ]" : "AnchorMin [ Статус всегда отображается ]")]
    public string AnchorMinA;
    [JsonProperty(LanguageEnglish ? "AnchorMax [ Status is always displayed ]" : "AnchorMax [ Статус всегда отображается ]")]
    public string AnchorMaxA;
    [JsonProperty(LanguageEnglish ? "OffsetMin [ Status is always displayed ]" : "OffsetMin [ Статус всегда отображается ]")]
    public string OffsetMinA;
    [JsonProperty(LanguageEnglish ? "OffsetMax [ Status is always displayed ]" : "OffsetMax [ Статус всегда отображается ]")]
    public string OffsetMaxA;
    [JsonProperty(LanguageEnglish ? "Use icons [ Status is always displayed ]" : "Использовать иконки [ Статус всегда отображается ]")]
    public bool IconA;
    [JsonProperty(LanguageEnglish ? "Text color" : "Цвет текста")]
    public string ColorText;
    [JsonProperty(LanguageEnglish ? "Text size" : "Размер текста")]
    public int SizeText;
    [JsonProperty(LanguageEnglish ? "Always display UI when protection is active" : "Постоянно отображать UI при активной защите")]
    public bool ActiveUI;
    [JsonProperty(LanguageEnglish ? "Always display UI when protection is inactive" : "Постоянно отображать UI при неактивной защите")]
    public bool DeactiveUI;
}


```

---

## XRate

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;
using Random = UnityEngine.Random;
using System;
using Oxide.Core;
using ConVar;
using ru = Oxide.Game.Rust;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("XRate", "Menevt", "1.0.03")]
[Description("НАСТРОЙКА РЕЙТОВ ДОБЫЧИ (ОПТИМИЗИРОВАНО)")]
public class XRate : RustPlugin
{
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class rateset
    {
        [JsonProperty("Поднимаемые ресурсы")]
        public float grab;
        [JsonProperty("Добываемые ресурсы")]
        public float gather;
        [JsonProperty("Сульфур")]
        public float sulfur;
        [JsonProperty("С карьера")]
        public float carier;
        [JsonProperty("С ящиков/бочек")]
        public float box;
        [JsonProperty("Запертый ящик")]
        public float lockbox;
        [JsonProperty("С ученых")]
        public float npc;
        [JsonProperty("Скорость переплавки")]
        public float speed;
    }

     class daynight
    {
        [JsonProperty("Включить?")]
        public bool enable;
        [JsonProperty("Длина ночи")]
        public float night;
        [JsonProperty("Длина дня")]
        public float day;
        [JsonProperty("Автопропуск ночи")]
        public bool skipnight;
        [JsonProperty("Голосование за пропуск ночи")]
        public bool vote;
        [JsonProperty("Ночное увелечение рейтов (прим. 1.0 - на 100%, 0 - выключить)")]
        public float upnight;
    }

    private class PluginConfig
    {
        [JsonProperty("Экспериментально. Не трогать!")]
        public bool exp;
        [JsonProperty("Отключить ускоренную плавку")]
        public bool speed;
        [JsonProperty("Рейты у обычных игроков")]
        public rateset rates;
        [JsonProperty("Настройка дня и ночи")]
        public daynight daynight;
        [JsonProperty("Сообщения")]
        public List<string> messages;
        [JsonProperty("Привилегии")]
        public Dictionary<string, rateset> privilige;
        [JsonProperty("На что не увеличивать рейты?")]
        public string[] blacklist;
        public static PluginConfig DefaultConfig();
    }

     Dictionary<string, rateset> cash;
     Dictionary<ulong, float> cashcariers;
    static XRate ins;
     void Init();
     bool skip;
     bool isday;
     void OnHour();
     void OnSunrise();
    const string REFRESHGUI;
    const string GUI;
    static string CONSTVOTE;
     void CLEARVOTE();
     void StartVote();
     void EndVote();
     Timer Vtimer;
     bool activevote;
    static int Vday;
    static int Vnight;
    static List<ulong> voted;
    private void REFRESHME();
    private void cmdvoteday(BasePlayer player, string command, string[] args);
    private void cmdvotenight(BasePlayer player, string command, string[] args);
     bool CHECKPOINT(BasePlayer player);
     void OnSunset();
     void nightupdate();
     float daytime;
     float nighttime;
     float upnight;
     TOD_Time comp;
     void OnServerInitialized();
     void OnGroupPermissionGranted(string name, string perm);
     void OnGroupPermissionRevoked(string name, string perm);
     void OnUserGroupRemoved(string id, string groupName);
     void OnUserGroupAdded(string id, string groupName);
     void OnUserPermissionGranted(string id, string permName);
     void OnUserPermissionRevoked(string id, string permName);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    [ChatCommand("rate")]
    private void cmdRATE(BasePlayer player, string command, string[] args);
    [PluginReference]
    private Plugin ZREWARDME;
    private Plugin FROre;
     void getuserrate(string id, float bonus);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnGrowableGather(GrowableEntity plant, Item item, BasePlayer player);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void CashCarier(ulong id);
    private object OnExcavatorGather(ExcavatorArm arm, Item item);
    private void OnQuarryToggled(BaseEntity entity, BasePlayer player);
     void OnQuarryGather(MiningQuarry quarry, Item item);
     void OnContainerDropItems(ItemContainer container);
    private void OnEntityDeath(BaseNetworkable entity, HitInfo info);
     List<uint> CHECKED;
    private void OnLootEntity(BasePlayer player, object entity);
    private void Unload();
    private void OnEntitySpawned(BaseNetworkable entity);
    private object OnOvenToggle(StorageContainer oven, BasePlayer player);
    public class FurnaceController : FacepunchBehaviour
    {
        private BaseOven _oven;
        private BaseOven Furnace { get; set; }
        private float _speedMultiplier;
        private int amountmultiplier;
        private int amount;
        private void Awake();
        public void SetSpeed(float newspeed);
        private Item FindBurnable();
        public void Cook();
        private void ConsumeFuel(Item fuel, ItemModBurnable burnable);
        private Dictionary<Item, float> cook;
         class CREAP
        {
            public string prefabname;
            public string name;
            public ulong skin;
            public int amount;
        }

        private void SmeltItems();
        public void StartCooking();
        public void StopCooking();
    }

}

 class rateset
{
    [JsonProperty("Поднимаемые ресурсы")]
    public float grab;
    [JsonProperty("Добываемые ресурсы")]
    public float gather;
    [JsonProperty("Сульфур")]
    public float sulfur;
    [JsonProperty("С карьера")]
    public float carier;
    [JsonProperty("С ящиков/бочек")]
    public float box;
    [JsonProperty("Запертый ящик")]
    public float lockbox;
    [JsonProperty("С ученых")]
    public float npc;
    [JsonProperty("Скорость переплавки")]
    public float speed;
}

 class daynight
{
    [JsonProperty("Включить?")]
    public bool enable;
    [JsonProperty("Длина ночи")]
    public float night;
    [JsonProperty("Длина дня")]
    public float day;
    [JsonProperty("Автопропуск ночи")]
    public bool skipnight;
    [JsonProperty("Голосование за пропуск ночи")]
    public bool vote;
    [JsonProperty("Ночное увелечение рейтов (прим. 1.0 - на 100%, 0 - выключить)")]
    public float upnight;
}

private class PluginConfig
{
    [JsonProperty("Экспериментально. Не трогать!")]
    public bool exp;
    [JsonProperty("Отключить ускоренную плавку")]
    public bool speed;
    [JsonProperty("Рейты у обычных игроков")]
    public rateset rates;
    [JsonProperty("Настройка дня и ночи")]
    public daynight daynight;
    [JsonProperty("Сообщения")]
    public List<string> messages;
    [JsonProperty("Привилегии")]
    public Dictionary<string, rateset> privilige;
    [JsonProperty("На что не увеличивать рейты?")]
    public string[] blacklist;
    public static PluginConfig DefaultConfig();
}

public class FurnaceController : FacepunchBehaviour
{
    private BaseOven _oven;
    private BaseOven Furnace { get; set; }
    private float _speedMultiplier;
    private int amountmultiplier;
    private int amount;
    private void Awake();
    public void SetSpeed(float newspeed);
    private Item FindBurnable();
    public void Cook();
    private void ConsumeFuel(Item fuel, ItemModBurnable burnable);
    private Dictionary<Item, float> cook;
     class CREAP
    {
        public string prefabname;
        public string name;
        public ulong skin;
        public int amount;
    }

    private void SmeltItems();
    public void StartCooking();
    public void StopCooking();
}

 class CREAP
{
    public string prefabname;
    public string name;
    public ulong skin;
    public int amount;
}


```

---

## XRestartUI

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System;
using Newtonsoft.Json;
using System.Linq;
using System.Collections;

Oxide.Plugins
[Info("XRestartUI", "Monster.", "1.0.201")]
 class XRestartUI : RustPlugin
{
    private void TimerGameTip(BasePlayer player, string message, int seconds);
    private class RestartConfig
    {
        [JsonProperty("Настройка рестартов по расписанию [ Можно запланировать любую команду в любое время ]")]
        public Dictionary<string, string> ARestart;
        public static RestartConfig GetNewConfiguration();
        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Настройка предупреждений за N минут до рестарта")]
        public List<int> Warning;
        [JsonProperty("Список уникальных имен(ключей) причин рестарта - [ Настройка текста в lang ]")]
        public List<string> ListMessage;
        internal class GeneralSetting
        {
            [JsonProperty("SteamID профиля для кастомной аватарки")]
            public ulong SteamID;
            [JsonProperty("Использовать эффект тика")]
            public bool EffectTickUse;
            [JsonProperty("Использовать UI уведомления")]
            public bool UI;
            [JsonProperty("Используемый эффект тика")]
            public string EffectTick;
            [JsonProperty("Использовать GameTip уведомления")]
            public bool GameTip;
            [JsonProperty("Используемый эффект предупреждения")]
            public string EffectWarning;
            [JsonProperty("Использовать сообщения в чате")]
            public bool Message;
            [JsonProperty("Использовать эффект предупреждения")]
            public bool EffectWarningUse;
        }

    }

    private void TimerGUI(BasePlayer player, string message, int seconds);
    private void AutoRestart();
    private IEnumerator Restart(string message, int seconds);
    private RestartConfig config;
    private void Unload();
    protected override void SaveConfig();
    private object OnServerRestart(string message, int seconds);
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private Coroutine _coroutine;
    private void OnServerInitialized();
    private void InitializeLang();
    private void WarningGameTip(BasePlayer player, TimeSpan time);
    private void WarningGUI(BasePlayer player, TimeSpan time);
}

private class RestartConfig
{
    [JsonProperty("Настройка рестартов по расписанию [ Можно запланировать любую команду в любое время ]")]
    public Dictionary<string, string> ARestart;
    public static RestartConfig GetNewConfiguration();
    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Настройка предупреждений за N минут до рестарта")]
    public List<int> Warning;
    [JsonProperty("Список уникальных имен(ключей) причин рестарта - [ Настройка текста в lang ]")]
    public List<string> ListMessage;
    internal class GeneralSetting
    {
        [JsonProperty("SteamID профиля для кастомной аватарки")]
        public ulong SteamID;
        [JsonProperty("Использовать эффект тика")]
        public bool EffectTickUse;
        [JsonProperty("Использовать UI уведомления")]
        public bool UI;
        [JsonProperty("Используемый эффект тика")]
        public string EffectTick;
        [JsonProperty("Использовать GameTip уведомления")]
        public bool GameTip;
        [JsonProperty("Используемый эффект предупреждения")]
        public string EffectWarning;
        [JsonProperty("Использовать сообщения в чате")]
        public bool Message;
        [JsonProperty("Использовать эффект предупреждения")]
        public bool EffectWarningUse;
    }

}

internal class GeneralSetting
{
    [JsonProperty("SteamID профиля для кастомной аватарки")]
    public ulong SteamID;
    [JsonProperty("Использовать эффект тика")]
    public bool EffectTickUse;
    [JsonProperty("Использовать UI уведомления")]
    public bool UI;
    [JsonProperty("Используемый эффект тика")]
    public string EffectTick;
    [JsonProperty("Использовать GameTip уведомления")]
    public bool GameTip;
    [JsonProperty("Используемый эффект предупреждения")]
    public string EffectWarning;
    [JsonProperty("Использовать сообщения в чате")]
    public bool Message;
    [JsonProperty("Использовать эффект предупреждения")]
    public bool EffectWarningUse;
}


```

---

## XRPG

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("XRPG", "Monster.", "1.0.9")]
 class XRPG : RustPlugin
{
    private RPGConfig config;
    private class RPGConfig
    {
        internal class PanelSetting
        {
            [JsonProperty("AnchorMin")]
            public string AnchorMin;
            [JsonProperty("AnchorMax")]
            public string AnchorMax;
            [JsonProperty("OffsetMin")]
            public string OffsetMin;
            [JsonProperty("OffsetMax")]
            public string OffsetMax;
            [JsonProperty("Отображение прогресса: TRUE - от минимальных до максимальных рейтов | FALSE - от 0 до максимальных рейтов ")]
            public bool Progress;
        }

        internal class SSetting
        {
            [JsonProperty("Включить замедление прокачки | Чем больше рейты, тем медленнее прокачка")]
            public bool Slow;
            [JsonProperty("Включить рпг панель")]
            public bool Panel;
            [JsonProperty("Включить рпг сообщения")]
            public bool Messages;
            [JsonProperty("Стартовый умножитель прокачки рейтов")]
            public float Boost;
        }

        internal class Permissions
        {
            [JsonProperty("Максимальные рейты лесоруба")]
            public float WoodRate;
            [JsonProperty("Максимальные рейты рудокопа")]
            public float OreRate;
            [JsonProperty("Максимальные рейты охотника")]
            public float AnimalRate;
            [JsonProperty("Множитель прокачки рейтов")]
            public float Boost;
        }

        internal class WoodSetting
        {
            [JsonProperty("Максимальные рейты лесоруба")]
            public float RateMax;
            [JsonProperty("Стартовые рейты лесоруба")]
            public float RateStart;
            [JsonProperty("Включить прокачку рейтов лесоруба добывая ресурсы")]
            public bool Bonus;
            [JsonProperty("Включить прокачку рейтов лесоруба подбирая ресурсы")]
            public bool Pickup;
            [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты лесоруба | Ресурсы на которые будут действовать рейты лесоруба")]
            public Dictionary<string, float> Item;
        }

        internal class OreSetting
        {
            [JsonProperty("Максимальные рейты рудокопа")]
            public float RateMax;
            [JsonProperty("Стартовые рейты рудокопа")]
            public float RateStart;
            [JsonProperty("Включить прокачку рейтов рудокопа добывая ресурсы")]
            public bool Bonus;
            [JsonProperty("Включить прокачку рейтов рудокопа подбирая ресурсы")]
            public bool Pickup;
            [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты рудокопа | Ресурсы на которые будут действовать рейты рудокопа")]
            public Dictionary<string, float> Item;
        }

        internal class AnimalSetting
        {
            [JsonProperty("Максимальные рейты охотника")]
            public float RateMax;
            [JsonProperty("Стартовые рейты охотника")]
            public float RateStart;
            [JsonProperty("Включить прокачку рейтов охотника добывая ресурсы")]
            public bool Bonus;
            [JsonProperty("Включить прокачку рейтов охотника подбирая ресурсы")]
            public bool Pickup;
            [JsonProperty("Включить прокачку рейтов охотника убивая животных")]
            public bool Kill;
            [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты охотника | Ресурсы на которые будут действовать рейты охотника")]
            public Dictionary<string, float> Item;
            [JsonProperty("Животные за убийство которых начислять рейты охотника")]
            public Dictionary<string, float> Animal;
        }

        [JsonProperty("Общее")]
        public SSetting Setting;
        [JsonProperty("Расположение мини-панели")]
        public PanelSetting Panel;
        [JsonProperty("Настройка пермишенов")]
        public Dictionary<string, Permissions> Permisssion;
        [JsonProperty("Настройка лесоруба")]
        public WoodSetting Wood;
        [JsonProperty("Настройка рудокопа")]
        public OreSetting Ore;
        [JsonProperty("Настройка охотника")]
        public AnimalSetting Animal;
        public static RPGConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class RPGData
    {
        [JsonProperty("Лесоруб")]
        public float Wood;
        [JsonProperty("Рудокоп")]
        public float Ore;
        [JsonProperty("Охотник")]
        public float Animal;
        [JsonProperty("Максимальные рейты лесоруба")]
        public float WoodRate;
        [JsonProperty("Максимальные рейты рудокопа")]
        public float OreRate;
        [JsonProperty("Максимальные рейты охотника")]
        public float AnimalRate;
        [JsonProperty("Множитель прокачки рейтов")]
        public float Boost;
        [JsonProperty("Активность UI")]
        public bool ActiveUI;
    }

    private Dictionary<ulong, RPGData> StoredData;
    [ChatCommand("rank")]
     void cmdTOP(BasePlayer player);
    [ConsoleCommand("ui")]
     void ccmdUI(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private void Unload();
     void OnPlayerConnected(BasePlayer player);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void TOP(BasePlayer player);
     void MaxRate(BasePlayer player);
     void RPGUI(BasePlayer player);
     void HideUI(BasePlayer player);
     void ShowUI(BasePlayer player);
     void WoodUI(BasePlayer player);
     void OreUI(BasePlayer player);
     void AnimalUI(BasePlayer player);
     void UPMessageWood(BasePlayer player, string Messages);
     void UPMessageOre(BasePlayer player, string Messages);
     void UPMessageAnimal(BasePlayer player, string Messages);
     void InitializeLang();
}

private class RPGConfig
{
    internal class PanelSetting
    {
        [JsonProperty("AnchorMin")]
        public string AnchorMin;
        [JsonProperty("AnchorMax")]
        public string AnchorMax;
        [JsonProperty("OffsetMin")]
        public string OffsetMin;
        [JsonProperty("OffsetMax")]
        public string OffsetMax;
        [JsonProperty("Отображение прогресса: TRUE - от минимальных до максимальных рейтов | FALSE - от 0 до максимальных рейтов ")]
        public bool Progress;
    }

    internal class SSetting
    {
        [JsonProperty("Включить замедление прокачки | Чем больше рейты, тем медленнее прокачка")]
        public bool Slow;
        [JsonProperty("Включить рпг панель")]
        public bool Panel;
        [JsonProperty("Включить рпг сообщения")]
        public bool Messages;
        [JsonProperty("Стартовый умножитель прокачки рейтов")]
        public float Boost;
    }

    internal class Permissions
    {
        [JsonProperty("Максимальные рейты лесоруба")]
        public float WoodRate;
        [JsonProperty("Максимальные рейты рудокопа")]
        public float OreRate;
        [JsonProperty("Максимальные рейты охотника")]
        public float AnimalRate;
        [JsonProperty("Множитель прокачки рейтов")]
        public float Boost;
    }

    internal class WoodSetting
    {
        [JsonProperty("Максимальные рейты лесоруба")]
        public float RateMax;
        [JsonProperty("Стартовые рейты лесоруба")]
        public float RateStart;
        [JsonProperty("Включить прокачку рейтов лесоруба добывая ресурсы")]
        public bool Bonus;
        [JsonProperty("Включить прокачку рейтов лесоруба подбирая ресурсы")]
        public bool Pickup;
        [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты лесоруба | Ресурсы на которые будут действовать рейты лесоруба")]
        public Dictionary<string, float> Item;
    }

    internal class OreSetting
    {
        [JsonProperty("Максимальные рейты рудокопа")]
        public float RateMax;
        [JsonProperty("Стартовые рейты рудокопа")]
        public float RateStart;
        [JsonProperty("Включить прокачку рейтов рудокопа добывая ресурсы")]
        public bool Bonus;
        [JsonProperty("Включить прокачку рейтов рудокопа подбирая ресурсы")]
        public bool Pickup;
        [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты рудокопа | Ресурсы на которые будут действовать рейты рудокопа")]
        public Dictionary<string, float> Item;
    }

    internal class AnimalSetting
    {
        [JsonProperty("Максимальные рейты охотника")]
        public float RateMax;
        [JsonProperty("Стартовые рейты охотника")]
        public float RateStart;
        [JsonProperty("Включить прокачку рейтов охотника добывая ресурсы")]
        public bool Bonus;
        [JsonProperty("Включить прокачку рейтов охотника подбирая ресурсы")]
        public bool Pickup;
        [JsonProperty("Включить прокачку рейтов охотника убивая животных")]
        public bool Kill;
        [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты охотника | Ресурсы на которые будут действовать рейты охотника")]
        public Dictionary<string, float> Item;
        [JsonProperty("Животные за убийство которых начислять рейты охотника")]
        public Dictionary<string, float> Animal;
    }

    [JsonProperty("Общее")]
    public SSetting Setting;
    [JsonProperty("Расположение мини-панели")]
    public PanelSetting Panel;
    [JsonProperty("Настройка пермишенов")]
    public Dictionary<string, Permissions> Permisssion;
    [JsonProperty("Настройка лесоруба")]
    public WoodSetting Wood;
    [JsonProperty("Настройка рудокопа")]
    public OreSetting Ore;
    [JsonProperty("Настройка охотника")]
    public AnimalSetting Animal;
    public static RPGConfig GetNewConfiguration();
}

internal class PanelSetting
{
    [JsonProperty("AnchorMin")]
    public string AnchorMin;
    [JsonProperty("AnchorMax")]
    public string AnchorMax;
    [JsonProperty("OffsetMin")]
    public string OffsetMin;
    [JsonProperty("OffsetMax")]
    public string OffsetMax;
    [JsonProperty("Отображение прогресса: TRUE - от минимальных до максимальных рейтов | FALSE - от 0 до максимальных рейтов ")]
    public bool Progress;
}

internal class SSetting
{
    [JsonProperty("Включить замедление прокачки | Чем больше рейты, тем медленнее прокачка")]
    public bool Slow;
    [JsonProperty("Включить рпг панель")]
    public bool Panel;
    [JsonProperty("Включить рпг сообщения")]
    public bool Messages;
    [JsonProperty("Стартовый умножитель прокачки рейтов")]
    public float Boost;
}

internal class Permissions
{
    [JsonProperty("Максимальные рейты лесоруба")]
    public float WoodRate;
    [JsonProperty("Максимальные рейты рудокопа")]
    public float OreRate;
    [JsonProperty("Максимальные рейты охотника")]
    public float AnimalRate;
    [JsonProperty("Множитель прокачки рейтов")]
    public float Boost;
}

internal class WoodSetting
{
    [JsonProperty("Максимальные рейты лесоруба")]
    public float RateMax;
    [JsonProperty("Стартовые рейты лесоруба")]
    public float RateStart;
    [JsonProperty("Включить прокачку рейтов лесоруба добывая ресурсы")]
    public bool Bonus;
    [JsonProperty("Включить прокачку рейтов лесоруба подбирая ресурсы")]
    public bool Pickup;
    [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты лесоруба | Ресурсы на которые будут действовать рейты лесоруба")]
    public Dictionary<string, float> Item;
}

internal class OreSetting
{
    [JsonProperty("Максимальные рейты рудокопа")]
    public float RateMax;
    [JsonProperty("Стартовые рейты рудокопа")]
    public float RateStart;
    [JsonProperty("Включить прокачку рейтов рудокопа добывая ресурсы")]
    public bool Bonus;
    [JsonProperty("Включить прокачку рейтов рудокопа подбирая ресурсы")]
    public bool Pickup;
    [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты рудокопа | Ресурсы на которые будут действовать рейты рудокопа")]
    public Dictionary<string, float> Item;
}

internal class AnimalSetting
{
    [JsonProperty("Максимальные рейты охотника")]
    public float RateMax;
    [JsonProperty("Стартовые рейты охотника")]
    public float RateStart;
    [JsonProperty("Включить прокачку рейтов охотника добывая ресурсы")]
    public bool Bonus;
    [JsonProperty("Включить прокачку рейтов охотника подбирая ресурсы")]
    public bool Pickup;
    [JsonProperty("Включить прокачку рейтов охотника убивая животных")]
    public bool Kill;
    [JsonProperty("Ресурсы за добычу/подбор которых начислять рейты охотника | Ресурсы на которые будут действовать рейты охотника")]
    public Dictionary<string, float> Item;
    [JsonProperty("Животные за убийство которых начислять рейты охотника")]
    public Dictionary<string, float> Animal;
}

private class RPGData
{
    [JsonProperty("Лесоруб")]
    public float Wood;
    [JsonProperty("Рудокоп")]
    public float Ore;
    [JsonProperty("Охотник")]
    public float Animal;
    [JsonProperty("Максимальные рейты лесоруба")]
    public float WoodRate;
    [JsonProperty("Максимальные рейты рудокопа")]
    public float OreRate;
    [JsonProperty("Максимальные рейты охотника")]
    public float AnimalRate;
    [JsonProperty("Множитель прокачки рейтов")]
    public float Boost;
    [JsonProperty("Активность UI")]
    public bool ActiveUI;
}


```

---

## XScan

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("XScan", "Я", "1.0.1")]
 class XScan : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private ScanConfig config;
    private class ScanConfig
    {
        internal class Settings
        {
            [JsonProperty("Включить GUI сообщения")]
            public bool GUIMessage;
            [JsonProperty("Включить ЧАТ сообщения")]
            public bool ChatMessage;
            [JsonProperty("Хранить логи в дате")]
            public bool SaveData;
            [JsonProperty("Автоматически очищать дату после вайпа")]
            public bool WipeData;
            [JsonProperty("Хранить логи установленных шкафов и доступ к просмотру информации о шкафе")]
            public bool CupboardData;
            [JsonProperty("Перезарядка скана в сек.")]
            public int CooldownScan;
            [JsonProperty("Перезарядка сообщений в сек.")]
            public int CooldownMessage;
            [JsonProperty("Список префабов. Отсканировав их можно узнать владельца шкафа и список авторизованных игроков")]
            public List<string> PrefabList;
        }

        internal class GUISettings
        {
            [JsonProperty("Время активности GUI")]
            public float GUIActive;
            [JsonProperty("Максимальное кол-во отображаемых игроков")]
            public int MaxCount;
            [JsonProperty("Максимальное кол-во отображаемых игроков в строке")]
            public int MaxCountString;
        }

        [JsonProperty("Общие настройки")]
        public Settings Setting;
        [JsonProperty("Настройки GUI")]
        public GUISettings GUISetting;
        public static ScanConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<BasePlayer, DateTime> CooldownsScan;
    private Dictionary<BasePlayer, DateTime> CooldownsMessage;
    [ChatCommand("scan")]
    private void cmdScan(BasePlayer player);
    [ConsoleCommand("scan_give")]
     void ccmdGiveSC(ConsoleSystem.Arg arg);
    private Dictionary<string, string> StoredData;
    private Dictionary<ulong, string> StoredDataT;
    private Dictionary<ulong, int> StoredDataS;
    private void OnServerInitialized();
    private int AuthorizedCount(int count);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    private void OnNewSave();
    private void Scan(BasePlayer player);
    private string Player_GetNameID(ulong id);
    private void ChatMessage(BasePlayer player, ulong ownerid, List<ulong> ListPlayers, string message);
    private void ChatMessageOwner(BasePlayer player, ulong ownerid);
    private void GUI(BasePlayer player, ulong ownerid, List<ulong> ListPlayers, string message);
    private void GUIOwner(BasePlayer player, ulong ownerid);
     void InitializeLang();
}

private class ScanConfig
{
    internal class Settings
    {
        [JsonProperty("Включить GUI сообщения")]
        public bool GUIMessage;
        [JsonProperty("Включить ЧАТ сообщения")]
        public bool ChatMessage;
        [JsonProperty("Хранить логи в дате")]
        public bool SaveData;
        [JsonProperty("Автоматически очищать дату после вайпа")]
        public bool WipeData;
        [JsonProperty("Хранить логи установленных шкафов и доступ к просмотру информации о шкафе")]
        public bool CupboardData;
        [JsonProperty("Перезарядка скана в сек.")]
        public int CooldownScan;
        [JsonProperty("Перезарядка сообщений в сек.")]
        public int CooldownMessage;
        [JsonProperty("Список префабов. Отсканировав их можно узнать владельца шкафа и список авторизованных игроков")]
        public List<string> PrefabList;
    }

    internal class GUISettings
    {
        [JsonProperty("Время активности GUI")]
        public float GUIActive;
        [JsonProperty("Максимальное кол-во отображаемых игроков")]
        public int MaxCount;
        [JsonProperty("Максимальное кол-во отображаемых игроков в строке")]
        public int MaxCountString;
    }

    [JsonProperty("Общие настройки")]
    public Settings Setting;
    [JsonProperty("Настройки GUI")]
    public GUISettings GUISetting;
    public static ScanConfig GetNewConfiguration();
}

internal class Settings
{
    [JsonProperty("Включить GUI сообщения")]
    public bool GUIMessage;
    [JsonProperty("Включить ЧАТ сообщения")]
    public bool ChatMessage;
    [JsonProperty("Хранить логи в дате")]
    public bool SaveData;
    [JsonProperty("Автоматически очищать дату после вайпа")]
    public bool WipeData;
    [JsonProperty("Хранить логи установленных шкафов и доступ к просмотру информации о шкафе")]
    public bool CupboardData;
    [JsonProperty("Перезарядка скана в сек.")]
    public int CooldownScan;
    [JsonProperty("Перезарядка сообщений в сек.")]
    public int CooldownMessage;
    [JsonProperty("Список префабов. Отсканировав их можно узнать владельца шкафа и список авторизованных игроков")]
    public List<string> PrefabList;
}

internal class GUISettings
{
    [JsonProperty("Время активности GUI")]
    public float GUIActive;
    [JsonProperty("Максимальное кол-во отображаемых игроков")]
    public int MaxCount;
    [JsonProperty("Максимальное кол-во отображаемых игроков в строке")]
    public int MaxCountString;
}


```

---

## XSkinMenu

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("XSkinMenu", "Я", "1.0.502")]
 class XSkinMenu : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private SkinConfig config;
    private class SkinConfig
    {
        internal class GeneralSetting
        {
            [JsonProperty("Сгенерировать/Проверять и добавлять новые скины принятые разработчиками или сделаные для твич дропсов")]
            public bool UpdateSkins;
            [JsonProperty("Сгенерировать/Проверять и добавлять новые скины добавленные разработчиками [ К примеру скин на хазмат ]")]
            public bool UpdateSkinsFacepunch;
            [JsonProperty("Отображать кнопку для удаления всех скинов")]
            public bool ButtonClear;
            [JsonProperty("Черный список скинов которые нельзя изменить. [ Например: огненные перчатки, огненный топор ]]")]
            public List<ulong> Blacklist;
        }

        internal class MenuSSetting
        {
            [JsonProperty("Иконка включенного параметра")]
            public string TButtonIcon;
            [JsonProperty("Иконка вылюченного параметра")]
            public string FButtonIcon;
            [JsonProperty("Цвет включенного параметра")]
            public string CTButton;
            [JsonProperty("Цвет вылюченного параметра")]
            public string CFButton;
        }

        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Меню настроект")]
        public MenuSSetting MenuS;
        [JsonProperty("Настройка категорий")]
        public Dictionary<string, List<string>> Category;
        public static SkinConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    internal class Data
    {
        [JsonProperty("Смена скинов в инвентаре")]
        public bool ChangeSI;
        [JsonProperty("Смена скинов на предметах")]
        public bool ChangeSE;
        [JsonProperty("Смена скинов при крафте")]
        public bool ChangeSC;
        [JsonProperty("Смена скинов в инвентаре после удаления")]
        public bool ChangeSCL;
        [JsonProperty("Смена скинов при попадании в инвентарь")]
        public bool ChangeSG;
        [JsonProperty("Скины")]
        public Dictionary<string, ulong> Skins;
    }

    private Dictionary<ulong, Data> StoredData;
    private Dictionary<ulong, bool> StoredDataFriends;
    public Dictionary<string, List<ulong>> StoredDataSkins;
    private void LoadData(BasePlayer player);
    private void SaveData(BasePlayer player);
    private void Unload();
    private Dictionary<BasePlayer, DateTime> Cooldowns;
    [ChatCommand("skin")]
    private void cmdOpenGUI(BasePlayer player);
    [ChatCommand("skinentity")]
    private void cmdSetSkinEntity(BasePlayer player);
    [ConsoleCommand("skin_c")]
    private void ccmdCategoryS(ConsoleSystem.Arg args);
    [ConsoleCommand("skin_s")]
    private void ccmdSetting(ConsoleSystem.Arg args);
    [ConsoleCommand("page.xskinmenu")]
    private void ccmdPage(ConsoleSystem.Arg args);
    [ConsoleCommand("xskin")]
    private void ccmdAdmin(ConsoleSystem.Arg args);
    private readonly Dictionary<string, string> shortnamesEntity;
    private void OnServerInitialized();
    private void GenerateItems();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    public Dictionary<ulong, string> errorskins;
    public Dictionary<ulong, string> facepunchskins;
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private void SetSkinItem(BasePlayer player, string item, ulong skin);
    private void SetSkinCraftGive(BasePlayer player, Item item);
    private void SetSkinEntity(BasePlayer player, BaseEntity entity, string shortname);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void GUI(BasePlayer player);
    private void CategoryGUI(BasePlayer player, int page);
    private void ItemGUI(BasePlayer player, string category, int Page);
    private void SkinGUI(BasePlayer player, string item, int Page);
    private void SettingGUI(BasePlayer player);
    private void SendInfo(BasePlayer player, string message);
    private void InitializeLang();
}

private class SkinConfig
{
    internal class GeneralSetting
    {
        [JsonProperty("Сгенерировать/Проверять и добавлять новые скины принятые разработчиками или сделаные для твич дропсов")]
        public bool UpdateSkins;
        [JsonProperty("Сгенерировать/Проверять и добавлять новые скины добавленные разработчиками [ К примеру скин на хазмат ]")]
        public bool UpdateSkinsFacepunch;
        [JsonProperty("Отображать кнопку для удаления всех скинов")]
        public bool ButtonClear;
        [JsonProperty("Черный список скинов которые нельзя изменить. [ Например: огненные перчатки, огненный топор ]]")]
        public List<ulong> Blacklist;
    }

    internal class MenuSSetting
    {
        [JsonProperty("Иконка включенного параметра")]
        public string TButtonIcon;
        [JsonProperty("Иконка вылюченного параметра")]
        public string FButtonIcon;
        [JsonProperty("Цвет включенного параметра")]
        public string CTButton;
        [JsonProperty("Цвет вылюченного параметра")]
        public string CFButton;
    }

    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Меню настроект")]
    public MenuSSetting MenuS;
    [JsonProperty("Настройка категорий")]
    public Dictionary<string, List<string>> Category;
    public static SkinConfig GetNewConfiguration();
}

internal class GeneralSetting
{
    [JsonProperty("Сгенерировать/Проверять и добавлять новые скины принятые разработчиками или сделаные для твич дропсов")]
    public bool UpdateSkins;
    [JsonProperty("Сгенерировать/Проверять и добавлять новые скины добавленные разработчиками [ К примеру скин на хазмат ]")]
    public bool UpdateSkinsFacepunch;
    [JsonProperty("Отображать кнопку для удаления всех скинов")]
    public bool ButtonClear;
    [JsonProperty("Черный список скинов которые нельзя изменить. [ Например: огненные перчатки, огненный топор ]]")]
    public List<ulong> Blacklist;
}

internal class MenuSSetting
{
    [JsonProperty("Иконка включенного параметра")]
    public string TButtonIcon;
    [JsonProperty("Иконка вылюченного параметра")]
    public string FButtonIcon;
    [JsonProperty("Цвет включенного параметра")]
    public string CTButton;
    [JsonProperty("Цвет вылюченного параметра")]
    public string CFButton;
}

internal class Data
{
    [JsonProperty("Смена скинов в инвентаре")]
    public bool ChangeSI;
    [JsonProperty("Смена скинов на предметах")]
    public bool ChangeSE;
    [JsonProperty("Смена скинов при крафте")]
    public bool ChangeSC;
    [JsonProperty("Смена скинов в инвентаре после удаления")]
    public bool ChangeSCL;
    [JsonProperty("Смена скинов при попадании в инвентарь")]
    public bool ChangeSG;
    [JsonProperty("Скины")]
    public Dictionary<string, ulong> Skins;
}


```

---

## XStashLogs

```csharp
using Newtonsoft.Json;
using System;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("XStashLogs", "Sempai#3239", "1.0.201")]
 class XStashLogs : RustPlugin
{
    protected override void SaveConfig();
    private void OnServerInitialized();
     object CanSeeStash(BasePlayer player, StashContainer stash);
    private class StashConfig
    {
        [JsonProperty("Настройка сообщений/логов закопанных тайников")]
        public SeeSetting See;
        public static StashConfig GetNewConfiguration();
        internal class DiscordSetting
        {
            [JsonProperty("WebHook канала для сообщений")]
            public string WebHook;
        }

        internal class SeeSetting
        {
            [JsonProperty("Включить сообщения в консоль закопанных своих тайников")]
            public bool ConsoleSeeStashOwner;
            [JsonProperty("Включить сообщения в консоль закопанных тайников друзей")]
            public bool ConsoleSeeStashFriend;
            [JsonProperty("Включить сообщения в консоль закопанных чужих тайников")]
            public bool ConsoleSeeStash;
            [JsonProperty("Включить сообщения в Discord закопанных своих тайников")]
            public bool DiscordSeeStashOwner;
            [JsonProperty("Включить сообщения в Discord закопанных тайников друзей")]
            public bool DiscordSeeStashFriend;
            [JsonProperty("Включить сообщения в Discord закопанных чужих тайников")]
            public bool DiscordSeeStash;
            [JsonProperty("Включить логирование закопанных своих тайников")]
            public bool LogSeeStashOwner;
            [JsonProperty("Включить логирование закопанных тайников друзей")]
            public bool LogSeeStashFriend;
            [JsonProperty("Включить логирование закопанных чужих тайников")]
            public bool LogSeeStash;
        }

        [JsonProperty("Настройка сообщений/логов выкопанных тайников")]
        public HideSetting Hide;
        internal class HideSetting
        {
            [JsonProperty("Включить сообщения в консоль выкопанных своих тайников")]
            public bool ConsoleHideStashOwner;
            [JsonProperty("Включить сообщения в консоль выкопанных тайников друзей")]
            public bool ConsoleHideStashFriend;
            [JsonProperty("Включить сообщения в консоль выкопанных чужих тайников")]
            public bool ConsoleHideStash;
            [JsonProperty("Включить сообщения в Discord выкопанных своих тайников")]
            public bool DiscordHideStashOwner;
            [JsonProperty("Включить сообщения в Discord выкопанных тайников друзей")]
            public bool DiscordHideStashFriend;
            [JsonProperty("Включить сообщения в Discord выкопанных чужих тайников")]
            public bool DiscordHideStash;
            [JsonProperty("Включить логирование выкопанных своих тайников")]
            public bool LogHideStashOwner;
            [JsonProperty("Включить логирование выкопанных тайников друзей")]
            public bool LogHideStashFriend;
            [JsonProperty("Включить логирование выкопанных чужих тайников")]
            public bool LogHideStash;
        }

        [JsonProperty("Настройка Discord")]
        public DiscordSetting Discord;
    }

    [PluginReference]
    private Plugin DiscordMessages;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private StashConfig config;
    public string error;
     object CanHideStash(BasePlayer player, StashContainer stash);
}

private class StashConfig
{
    [JsonProperty("Настройка сообщений/логов закопанных тайников")]
    public SeeSetting See;
    public static StashConfig GetNewConfiguration();
    internal class DiscordSetting
    {
        [JsonProperty("WebHook канала для сообщений")]
        public string WebHook;
    }

    internal class SeeSetting
    {
        [JsonProperty("Включить сообщения в консоль закопанных своих тайников")]
        public bool ConsoleSeeStashOwner;
        [JsonProperty("Включить сообщения в консоль закопанных тайников друзей")]
        public bool ConsoleSeeStashFriend;
        [JsonProperty("Включить сообщения в консоль закопанных чужих тайников")]
        public bool ConsoleSeeStash;
        [JsonProperty("Включить сообщения в Discord закопанных своих тайников")]
        public bool DiscordSeeStashOwner;
        [JsonProperty("Включить сообщения в Discord закопанных тайников друзей")]
        public bool DiscordSeeStashFriend;
        [JsonProperty("Включить сообщения в Discord закопанных чужих тайников")]
        public bool DiscordSeeStash;
        [JsonProperty("Включить логирование закопанных своих тайников")]
        public bool LogSeeStashOwner;
        [JsonProperty("Включить логирование закопанных тайников друзей")]
        public bool LogSeeStashFriend;
        [JsonProperty("Включить логирование закопанных чужих тайников")]
        public bool LogSeeStash;
    }

    [JsonProperty("Настройка сообщений/логов выкопанных тайников")]
    public HideSetting Hide;
    internal class HideSetting
    {
        [JsonProperty("Включить сообщения в консоль выкопанных своих тайников")]
        public bool ConsoleHideStashOwner;
        [JsonProperty("Включить сообщения в консоль выкопанных тайников друзей")]
        public bool ConsoleHideStashFriend;
        [JsonProperty("Включить сообщения в консоль выкопанных чужих тайников")]
        public bool ConsoleHideStash;
        [JsonProperty("Включить сообщения в Discord выкопанных своих тайников")]
        public bool DiscordHideStashOwner;
        [JsonProperty("Включить сообщения в Discord выкопанных тайников друзей")]
        public bool DiscordHideStashFriend;
        [JsonProperty("Включить сообщения в Discord выкопанных чужих тайников")]
        public bool DiscordHideStash;
        [JsonProperty("Включить логирование выкопанных своих тайников")]
        public bool LogHideStashOwner;
        [JsonProperty("Включить логирование выкопанных тайников друзей")]
        public bool LogHideStashFriend;
        [JsonProperty("Включить логирование выкопанных чужих тайников")]
        public bool LogHideStash;
    }

    [JsonProperty("Настройка Discord")]
    public DiscordSetting Discord;
}

internal class DiscordSetting
{
    [JsonProperty("WebHook канала для сообщений")]
    public string WebHook;
}

internal class SeeSetting
{
    [JsonProperty("Включить сообщения в консоль закопанных своих тайников")]
    public bool ConsoleSeeStashOwner;
    [JsonProperty("Включить сообщения в консоль закопанных тайников друзей")]
    public bool ConsoleSeeStashFriend;
    [JsonProperty("Включить сообщения в консоль закопанных чужих тайников")]
    public bool ConsoleSeeStash;
    [JsonProperty("Включить сообщения в Discord закопанных своих тайников")]
    public bool DiscordSeeStashOwner;
    [JsonProperty("Включить сообщения в Discord закопанных тайников друзей")]
    public bool DiscordSeeStashFriend;
    [JsonProperty("Включить сообщения в Discord закопанных чужих тайников")]
    public bool DiscordSeeStash;
    [JsonProperty("Включить логирование закопанных своих тайников")]
    public bool LogSeeStashOwner;
    [JsonProperty("Включить логирование закопанных тайников друзей")]
    public bool LogSeeStashFriend;
    [JsonProperty("Включить логирование закопанных чужих тайников")]
    public bool LogSeeStash;
}

internal class HideSetting
{
    [JsonProperty("Включить сообщения в консоль выкопанных своих тайников")]
    public bool ConsoleHideStashOwner;
    [JsonProperty("Включить сообщения в консоль выкопанных тайников друзей")]
    public bool ConsoleHideStashFriend;
    [JsonProperty("Включить сообщения в консоль выкопанных чужих тайников")]
    public bool ConsoleHideStash;
    [JsonProperty("Включить сообщения в Discord выкопанных своих тайников")]
    public bool DiscordHideStashOwner;
    [JsonProperty("Включить сообщения в Discord выкопанных тайников друзей")]
    public bool DiscordHideStashFriend;
    [JsonProperty("Включить сообщения в Discord выкопанных чужих тайников")]
    public bool DiscordHideStash;
    [JsonProperty("Включить логирование выкопанных своих тайников")]
    public bool LogHideStashOwner;
    [JsonProperty("Включить логирование выкопанных тайников друзей")]
    public bool LogHideStashFriend;
    [JsonProperty("Включить логирование выкопанных чужих тайников")]
    public bool LogHideStash;
}


```

---

## XWipeCalendar

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("XWipeCalendar", "SkuliDropek.", "1.0.501")]
 class XWipeCalendar : RustPlugin
{
    private CalendarConfig config;
    private class CalendarConfig
    {
        internal class GeneralSetting
        {
            [JsonProperty("Закрытие календаря нажатием в любой точке экрана")]
            public bool ButtonClose;
            [JsonProperty("Часовой пояс - UTC+0:00")]
            public int GMT;
        }

        internal class GUISetting
        {
            [JsonProperty("Цвет фона_1")]
            public string ColorBackgroundO;
            [JsonProperty("Цвет фона_2")]
            public string ColorBackgroundT;
            [JsonProperty("Цвет чисел текучего месяца")]
            public string ColorNumericM;
            [JsonProperty("Цвет чисел следующего месяца")]
            public string ColorNumericNM;
            [JsonProperty("Цвет блоков")]
            public string ColorBlock;
        }

        internal class EventsSetting
        {
            [JsonProperty("Цвет")]
            public string Color;
            [JsonProperty("Список дней события")]
            public List<int> Day;
            public EventsSetting(string color, List<int> day);
        }

        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Настройка GUI")]
        public GUISetting GUI;
        [JsonProperty("Список событий. Описание событий - oxide/lang/(ru/en)")]
        public Dictionary<int, List<EventsSetting>> Events;
        [JsonProperty("Время по МСК")]
        public DateTime MSCTime;
        public static CalendarConfig GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("wipe")]
    private void cmdOpenGUI(BasePlayer player);
    [ChatCommand("calendar")]
    private void cmdOpenGUII(BasePlayer player);
    [ConsoleCommand("wipe_page")]
    private void ccmdPage(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private void OnServerSave();
    private void MSC();
    private void GUI(BasePlayer player, int Page);
    private void InitializeLang();
}

private class CalendarConfig
{
    internal class GeneralSetting
    {
        [JsonProperty("Закрытие календаря нажатием в любой точке экрана")]
        public bool ButtonClose;
        [JsonProperty("Часовой пояс - UTC+0:00")]
        public int GMT;
    }

    internal class GUISetting
    {
        [JsonProperty("Цвет фона_1")]
        public string ColorBackgroundO;
        [JsonProperty("Цвет фона_2")]
        public string ColorBackgroundT;
        [JsonProperty("Цвет чисел текучего месяца")]
        public string ColorNumericM;
        [JsonProperty("Цвет чисел следующего месяца")]
        public string ColorNumericNM;
        [JsonProperty("Цвет блоков")]
        public string ColorBlock;
    }

    internal class EventsSetting
    {
        [JsonProperty("Цвет")]
        public string Color;
        [JsonProperty("Список дней события")]
        public List<int> Day;
        public EventsSetting(string color, List<int> day);
    }

    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Настройка GUI")]
    public GUISetting GUI;
    [JsonProperty("Список событий. Описание событий - oxide/lang/(ru/en)")]
    public Dictionary<int, List<EventsSetting>> Events;
    [JsonProperty("Время по МСК")]
    public DateTime MSCTime;
    public static CalendarConfig GetNewConfiguration();
}

internal class GeneralSetting
{
    [JsonProperty("Закрытие календаря нажатием в любой точке экрана")]
    public bool ButtonClose;
    [JsonProperty("Часовой пояс - UTC+0:00")]
    public int GMT;
}

internal class GUISetting
{
    [JsonProperty("Цвет фона_1")]
    public string ColorBackgroundO;
    [JsonProperty("Цвет фона_2")]
    public string ColorBackgroundT;
    [JsonProperty("Цвет чисел текучего месяца")]
    public string ColorNumericM;
    [JsonProperty("Цвет чисел следующего месяца")]
    public string ColorNumericNM;
    [JsonProperty("Цвет блоков")]
    public string ColorBlock;
}

internal class EventsSetting
{
    [JsonProperty("Цвет")]
    public string Color;
    [JsonProperty("Список дней события")]
    public List<int> Day;
    public EventsSetting(string color, List<int> day);
}


```

---

## ZBillBoards

```csharp
using UnityEngine;
using System.Collections.Generic;
using Oxide.Core;
using Newtonsoft.Json;
using System.Collections;
using UnityEngine.Networking;
using System.Drawing;
using System.IO;
using System.Drawing.Imaging;

Oxide.Plugins
[Info("ZBillBoards", "TopPlugin.ru", "1.3.0")]
[Description("Create huge (or small) billboards")]
public class ZBillBoards : RustPlugin
{
    private static ZBillBoards ins;
    private GameObject downloadControllerObject;
    private DownloadController downloadController;
    private GameObject pasteControllerObject;
    private PasteController pasteController;
    const string permAdmin;
    const string permConsole;
    const string permTier1;
    const string permTier2;
    const string permTier3;
    const string dataFileName;
     void Init();
     void OnServerInitialized();
     void Unload();
    private static void ProcessImage(byte[] bytes, int targetWidth, int targetHeight, DownloadRequest request);
    private static void SplitImageAndPaste(DownloadRequest request, Bitmap sourceImage, int defWidth, int defHeight);
    private static byte[] RecolorImage(MemoryStream stream, int targetWidth, int targetHeight);
    private class DownloadController : FacepunchBehaviour
    {
        private byte downloading;
        private readonly Queue<DownloadRequest> downloadQueue;
        public void QueueDownload(string url, BasePlayer player, Signage sign, int width, int height, bool adjustBrightness);
        private byte[] GetDownloadResponse(UnityWebRequest webRequest);
        private void StartNewDownload();
        private IEnumerator StartDownload(DownloadRequest request);
    }

    private class DownloadRequest
    {
        public BasePlayer player { get; set; }
        public Signage targetSign { get; set; }
        public string imageURL { get; set; }
        public int width { get; set; }
        public int height { get; set; }
        public bool adjustBrightness { get; set; }
        public DownloadRequest(string url, BasePlayer inputPlayer, Signage inputSign, int inputWidth, int inputHeight, bool inpBrightness);
    }

    private class PasteController : FacepunchBehaviour
    {
        private bool isPasting;
        private List<PasteRequest> pasteList;
        public void AddToPasteList(byte[] imgData, uint signID, int coordsX, int coordsY);
        public void PasteNextFromList();
        public void ClearPasteList();
        public int GetEmptyFrame(Signage sign);
        private void PasteImage(PasteRequest request);
    }

    private class PasteRequest
    {
        public uint signID { get; set; }
        public byte[] imgData { get; set; }
        public int signX { get; set; }
        public int signY { get; set; }
        public PasteRequest(byte[] inpImgData, uint inpSignID, int coordsX, int coordsY);
    }

    private static byte[] GetImagePart(Bitmap sourceImage, int targetWidth, int targetHeight, int splitX, int splitY);
    [ConsoleCommand("billboard.toggle")]
    private void consoleCmdZDP(ConsoleSystem.Arg arg);
    [ChatCommand("billboard")]
    private void cmdBillBoard(BasePlayer player, string command, string[] args);
    public void ValidateAllBillboards();
    public void ValidateBillboard(uint mainSignID);
    public void ToggleBillBoardPower(BasePlayer player);
    public void AddSignToBillboard(BasePlayer player, string[] args);
    public void ToggleDebug(BasePlayer player);
    private void SwitchBillBoardPower(NeonSign billboardSign, int forcePower);
    public void ChangeBillBoardSpeed(BasePlayer player, string[] args);
    public void ChangeBillBoardDimmer(BasePlayer player, string[] args);
    public void SilBillBoard(BasePlayer player, string[] args);
    public void DestroyBillBoards(BasePlayer player);
    public void DestroyBillBoard(BasePlayer player);
    public void RemoveFromUserBillboards(uint sid);
    public Signage GetBillBoard(BasePlayer player, bool checkIfBillBoard);
    public Signage GetMainSign(uint raySignID);
    public void ShowBillboardInfo(BasePlayer player);
    public void CreateBillBoard(BasePlayer player, string[] args, int maxSigns, int maxBillboards);
    public void CommandPasteNext(BasePlayer player);
    public void CommandPasteEmpty(BasePlayer player);
    public UnityEngine.Random rnd;
    private static void AddRandomPixel(Bitmap image);
     bool HasPermission(string id, string perm);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Tier 1")]
        public int maxSignsTier1;
        [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Tier 1")]
        public int maxBillboardsTier1;
        [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Tier 2")]
        public int maxSignsTier2;
        [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Tier 2")]
        public int maxBillboardsTier2;
        [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Tier 3")]
        public int maxSignsTier3;
        [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Tier 3")]
        public int maxBillboardsTier3;
        [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Admin")]
        public int maxSignsAdmin;
        [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Admin")]
        public int maxBillboardsAdmin;
        [JsonProperty(PropertyName = "Width and height of each neon sign image in pixels")]
        public int imageSize;
        [JsonProperty(PropertyName = "Seconds between pasting images")]
        public float pasteDelay;
        [JsonProperty(PropertyName = "Brightness of the images pasted on the billboards (experimental)")]
        public float brightness;
        [JsonProperty(PropertyName = "Lock signs to owner after creating billboard")]
        public bool lockSigns;
        [JsonProperty(PropertyName = "Give back a Neon Sign when a billboard is removed with the destroy command")]
        public bool refundOnDestroy;
        [JsonProperty(PropertyName = "Log extra output to console")]
        public bool debug;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
     StoredData storedData;
     class StoredData
    {
        public Dictionary<uint, BillBoardData> billBoards;
        public Dictionary<string, List<uint>> userBillboards;
    }

     class BillBoardData
    {
        public float speed;
        public int signsHorizontal;
        public int signsVertical;
        public List<uint> billBoardSigns;
    }

     void Loaded();
    public void LoadData();
     void SaveData();
}

private class DownloadController : FacepunchBehaviour
{
    private byte downloading;
    private readonly Queue<DownloadRequest> downloadQueue;
    public void QueueDownload(string url, BasePlayer player, Signage sign, int width, int height, bool adjustBrightness);
    private byte[] GetDownloadResponse(UnityWebRequest webRequest);
    private void StartNewDownload();
    private IEnumerator StartDownload(DownloadRequest request);
}

private class DownloadRequest
{
    public BasePlayer player { get; set; }
    public Signage targetSign { get; set; }
    public string imageURL { get; set; }
    public int width { get; set; }
    public int height { get; set; }
    public bool adjustBrightness { get; set; }
    public DownloadRequest(string url, BasePlayer inputPlayer, Signage inputSign, int inputWidth, int inputHeight, bool inpBrightness);
}

private class PasteController : FacepunchBehaviour
{
    private bool isPasting;
    private List<PasteRequest> pasteList;
    public void AddToPasteList(byte[] imgData, uint signID, int coordsX, int coordsY);
    public void PasteNextFromList();
    public void ClearPasteList();
    public int GetEmptyFrame(Signage sign);
    private void PasteImage(PasteRequest request);
}

private class PasteRequest
{
    public uint signID { get; set; }
    public byte[] imgData { get; set; }
    public int signX { get; set; }
    public int signY { get; set; }
    public PasteRequest(byte[] inpImgData, uint inpSignID, int coordsX, int coordsY);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Tier 1")]
    public int maxSignsTier1;
    [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Tier 1")]
    public int maxBillboardsTier1;
    [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Tier 2")]
    public int maxSignsTier2;
    [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Tier 2")]
    public int maxBillboardsTier2;
    [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Tier 3")]
    public int maxSignsTier3;
    [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Tier 3")]
    public int maxBillboardsTier3;
    [JsonProperty(PropertyName = "Maximum amount of signs in total (width x height) Admin")]
    public int maxSignsAdmin;
    [JsonProperty(PropertyName = "Maximum amount of billboards (any size, 0 = unlimited) Admin")]
    public int maxBillboardsAdmin;
    [JsonProperty(PropertyName = "Width and height of each neon sign image in pixels")]
    public int imageSize;
    [JsonProperty(PropertyName = "Seconds between pasting images")]
    public float pasteDelay;
    [JsonProperty(PropertyName = "Brightness of the images pasted on the billboards (experimental)")]
    public float brightness;
    [JsonProperty(PropertyName = "Lock signs to owner after creating billboard")]
    public bool lockSigns;
    [JsonProperty(PropertyName = "Give back a Neon Sign when a billboard is removed with the destroy command")]
    public bool refundOnDestroy;
    [JsonProperty(PropertyName = "Log extra output to console")]
    public bool debug;
}

 class StoredData
{
    public Dictionary<uint, BillBoardData> billBoards;
    public Dictionary<string, List<uint>> userBillboards;
}

 class BillBoardData
{
    public float speed;
    public int signsHorizontal;
    public int signsVertical;
    public List<uint> billBoardSigns;
}


```

---

## ZealStatistics

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Globalization;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("ZealStatistics", "Kira", "1.0.7")]
[Description("Плагин сбора статистики для сервера Rust.")]
 class ZealStatistics : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private StoredData DataBase;
    private string GetImg(string name);
    static public ConfigData config;
    public class ConfigData
    {
        [JsonProperty(PropertyName = "ZealStatistics")]
        public GUICFG ZealStatistics;
        public class GUICFG
        {
            [JsonProperty(PropertyName = "Разрешить просматривать информацию об игроках ?")]
            public bool info;
        }

    }

    public ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public ulong state1;
    public ulong state2;
    public ulong state3;
    public ulong state4;
    public ulong state5;
    private string Sharp;
    private string Blur;
    private string radial;
    private string regular;
    public ulong LastDamagePlayer;
    public class Filter
    {
        public string Name;
        public int Number;
    }

    public List<Filter> Filters;
     string Layer;
     void PlayerList(BasePlayer player);
     void MainGui(BasePlayer player);
     void InfoPlayer(ulong steamid, ulong initiator);
     void ServerTOP(BasePlayer player, int filter);
     void MathStates(BasePlayer player, int filter);
     void DrawFilters(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnServerInitialized();
    private void Unload();
    private void OnPlayerDie(BasePlayer player, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitinfo);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
    [ChatCommand("stats")]
    private void MainGuiStatistics(BasePlayer player);
    [ConsoleCommand("stats.manager")]
    private void ManagerPlayerStats(ConsoleSystem.Arg args);
    [ConsoleCommand("zstats.ban")]
    private void AddIgnorePlayer(ConsoleSystem.Arg args);
    [ConsoleCommand("servertop")]
    private void ServerGUITOP(ConsoleSystem.Arg args);
    [ConsoleCommand("filter")]
    private void FilterTOP(ConsoleSystem.Arg args);
    [ConsoleCommand("backtomain")]
    private void BackMain(ConsoleSystem.Arg args);
    public bool filter;
    [ConsoleCommand("showfilter")]
    private void FilterList(ConsoleSystem.Arg args);
    [ConsoleCommand("infopl")]
    private void infoplayer(ConsoleSystem.Arg args);
    [ConsoleCommand("close.stats")]
    private void CloseMainGuiStatistics(ConsoleSystem.Arg args);
    public class StoredData
    {
        public Dictionary<ulong, StatisticDB> StatisticDB;
        public List<ulong> IgnorePlayers;
    }

    public class StatisticDB
    {
        public string Name;
        public ulong SteamID;
        public int Kills;
        public int Death;
        public int AnimalKills;
        public int BradleyKills;
        public int HeliKills;
        public int Sulfur;
        public int Stones;
        public int MetalOre;
        public int Wood;
        public int TimePlayed;
        public StatisticDB();
    }

    [HookMethod("SaveData")]
    private void SaveData();
    private void LoadData();
    [HookMethod("GetPlayerTimePlayed")]
    public string GetPlayerTimePlayed(int time);
    [HookMethod("AddPlayer")]
     void AddPlayer(BasePlayer player);
    [HookMethod("CheckDataBase")]
     void CheckDataBase(BasePlayer player);
     void DestroyMainGUI(BasePlayer player);
     void DestroyServerGUI(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

public class ConfigData
{
    [JsonProperty(PropertyName = "ZealStatistics")]
    public GUICFG ZealStatistics;
    public class GUICFG
    {
        [JsonProperty(PropertyName = "Разрешить просматривать информацию об игроках ?")]
        public bool info;
    }

}

public class GUICFG
{
    [JsonProperty(PropertyName = "Разрешить просматривать информацию об игроках ?")]
    public bool info;
}

public class Filter
{
    public string Name;
    public int Number;
}

public class StoredData
{
    public Dictionary<ulong, StatisticDB> StatisticDB;
    public List<ulong> IgnorePlayers;
}

public class StatisticDB
{
    public string Name;
    public ulong SteamID;
    public int Kills;
    public int Death;
    public int AnimalKills;
    public int BradleyKills;
    public int HeliKills;
    public int Sulfur;
    public int Stones;
    public int MetalOre;
    public int Wood;
    public int TimePlayed;
    public StatisticDB();
}


```

---

## ZLevelsRemastered

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core;
using Oxide.Core.Database;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Zeiser Levels Remastered", "Zeiser/Visagalis", "1.6.6", ResourceId = 1453)]
[Description("Lets players level up as they harvest different resources and when crafting")]
 class ZLevelsRemastered : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
    readonly Core.MySql.Libraries.MySql _mySql;
    readonly Core.SQLite.Libraries.SQLite _sqLite;
     Connection _mySqlConnection;
     Connection _sqLiteConnection;
    public Dictionary<ulong, Dictionary<string, long>> playerList;
    readonly string sqLiteDBFile;
     void StartConnection();
     void CheckConnection();
    public void setPointsAndLevel(ulong userID, string skill, long points, long level);
    public void loadUser(BasePlayer player);
     void initPlayer(BasePlayer player, Dictionary<string, long> statsInit, List<Dictionary<string, object>> sqlData);
     void setPlayerData(ulong userID, string key, long value);
     void initPlayerData(BasePlayer player, Dictionary<string, long> playerData);
     void OnPlayerLootEnd(PlayerLoot inventory);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
    public void SaveUsers();
    static string EncodeNonAsciiCharacters(string value);
    public void saveUser(BasePlayer player);
    public Dictionary<string, long> getConnectedPlayerDetailsData(ulong userID);
    public static class Skills
    {
        public static string CRAFTING;
        public static string WOODCUTTING;
        public static string SKINNING;
        public static string MINING;
        public static string[] ALL;
    }

     List<ulong> guioff;
     Dictionary<string, string> colors;
     class CraftData
    {
        public Dictionary<string, CraftInfo> CraftList;
    }

     CraftData _craftData;
    [HookMethod("SendHelpText")]
     void SendHelpText(BasePlayer player);
    [ChatCommand("topskills")]
     void StatsTopCommand(BasePlayer player, string command, string[] args);
     void printMaxSkillDetails(BasePlayer player, string skill);
     void printMaxSkillDetails(BasePlayer player, string skill, List<Dictionary<string, object>> sqlData);
    [ConsoleCommand("zinfo")]
     void InfoCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("zlvl")]
     void ZlvlCommand(ConsoleSystem.Arg arg);
     void adminModifyPlayerStats(string skill, long level, int mode, BasePlayer p);
     void editMultiplierForPlayer(long multiplier, ulong userID);
    [ChatCommand("stats")]
     void StatsCommand(BasePlayer player, string command, string[] args);
    public static string ReadableTimeSpan(TimeSpan span);
    [ChatCommand("statinfo")]
     void StatInfoCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("statsui")]
     void StatsUICommand(BasePlayer player, string command, string[] args);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnLootEntity(BasePlayer looter, BaseEntity target);
     void OnLootPlayer(BasePlayer looter, BasePlayer beingLooter);
     void OnLootItem(BasePlayer looter, Item lootedItem);
     void FillElements(CuiElementContainer elements, string mainPanel, int rowNumber, int maxRows, long level, int percent, string skillName, string progressColor, int fontSize, string xpBarAnchorMin, string xpBarAnchorMax);
     void RenderUI(BasePlayer player);
     string getStatPrint(BasePlayer player, string skill);
     long ToEpochTime(DateTime dateTime);
     DateTime ToDateTimeFromEpoch(long intDate);
     void Loaded();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     void SetPlayerLastDeathDate(ulong userID);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void levelHandler(BasePlayer player, Item item, string skill);
     long getLevelPoints(long level);
     long getPointsLevel(long points, string skill);
     double getGathMult(long skillLevel, string skill);
     bool inPlayerList(UInt64 userID);
     void OnServerSave();
     void Unload();
     Dictionary<string, object> resourceMultipliers;
     Dictionary<string, object> levelCaps;
     Dictionary<string, object> pointsPerHit;
     Dictionary<string, object> craftingDetails;
     Dictionary<string, object> percentLostOnDeath;
     Dictionary<string, object> messages;
     Dictionary<string, object> dbConnection;
    protected override void LoadDefaultConfig();
     void Init();
     T checkCfg(string conf, T def);
     void removePoints(UInt64 userID, string skill, long points);
     long getLevel(UInt64 userID, string skill);
     long getPoints(UInt64 userID, string skill);
     void setLevel(UInt64 userID, string skill, long level);
     bool IsSkillDisabled(string skill);
     bool usingMySQL();
     int GetPenalty(BasePlayer player, string skill);
     int getPenaltyPercent(BasePlayer player, string skill);
     object OnItemCraftFinished(ItemCraftTask task, Item item);
     object OnItemCraft(ItemCraftTask task, BasePlayer crafter);
     int MaxB;
     int MinB;
     int Cooldown;
     void GenerateItems(bool reset);
     class CraftInfo
    {
        public int MaxBulkCraft;
        public int MinBulkCraft;
        public string shortName;
        public bool Enabled;
    }

     Dictionary<string, ItemDefinition> mcITEMS;
     ItemDefinition GetItem(string shortname);
     long getPointsNeededForNextLevel(long level);
     long getPercentAmount(long level, int percent);
     int getExperiencePercentInt(BasePlayer player, string skill);
     string getExperiencePercent(BasePlayer player, string skill);
}

public static class Skills
{
    public static string CRAFTING;
    public static string WOODCUTTING;
    public static string SKINNING;
    public static string MINING;
    public static string[] ALL;
}

 class CraftData
{
    public Dictionary<string, CraftInfo> CraftList;
}

 class CraftInfo
{
    public int MaxBulkCraft;
    public int MinBulkCraft;
    public string shortName;
    public bool Enabled;
}


```

---

## ZombieHorde

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Plugins;
using Rust.Ai.HTN;
using Rust.Ai.HTN.Sensors;
using Rust.Ai.HTN.Reasoning;
using Rust.Ai.HTN.Murderer;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using UnityEngine.AI;

Oxide.Plugins
[Info("ZombieHorde", "k1lly0u", "0.3.4")]
 class ZombieHorde : RustPlugin
{
    [PluginReference]
    private Plugin Kits;
    private Plugin Spawns;
    private static ZombieHorde Instance { get; set; }
    private const string SCARECROW_PREFAB;
    private const int WORLD_LAYER;
    private void OnServerInitialized();
    private void OnEntityTakeDamage(BaseCombatEntity baseCombatEntity, HitInfo hitInfo);
    private void OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private void OnEntityKill(HTNPlayer htnPlayer);
    private object CanBeTargeted(BasePlayer player, MonoBehaviour behaviour);
    private object CanEntityBeHostile(HTNPlayer htnPlayer);
    private object CanBradleyApcTarget(BradleyAPC bradleyAPC, HTNPlayer htnPlayer);
    private object OnCorpsePopulate(HTNPlayer htnPlayer, NPCPlayerCorpse npcPlayerCorpse);
    private object CanPopulateLoot(HTNPlayer htnPlayer, NPCPlayerCorpse corpse);
    private void Unload();
    private List<Vector3> _spawnPoints;
    private SpawnSystem _spawnSystem;
    private bool ValidateSpawnSystem();
    private const int SPAWN_RAYCAST_MASK;
    private const TerrainTopology.Enum SPAWN_TOPOLOGY_MASK;
    private Vector3 GetSpawnPoint();
    private void CreateMonumentHordeOrders();
    private void CreateRandomHordes();
    private ItemDefinition _blueprintBase;
    private ItemDefinition _glowEyes;
    private static MurdererDefinition _defaultDefinition;
    private static MurdererDefinition DefaultDefinition { get; set; }
    private void ValidateLoadoutProfiles();
    private void SpawnIntoContainer(LootableCorpse lootableCorpse);
    private void CreateItem(ConfigData.LootTable.RandomLoot.LootDefinition lootDefinition, ItemContainer container);
    private static void StripInventory(BasePlayer player, bool skipWear);
    private static void ClearContainer(ItemContainer container, bool skipWear);
    private static HTNPlayer InstantiateEntity(Vector3 position);
    private static NavMeshHit navHit;
    private static RaycastHit raycastHit;
    private static Collider[] _buffer;
    private static object FindPointOnNavmesh(Vector3 targetPosition, float maxDistance);
    private static bool IsInRockPrefab(Vector3 position);
    private static bool IsNearWorldCollider(Vector3 position);
    private static readonly string[] acceptedColliders;
    private static readonly string[] blockedColliders;
    private T ParseType(string type);
    private static bool ContainsTopologyAtPoint(TerrainTopology.Enum mask, Vector3 position);
    private HordeThinkManager _hordeThinkManager;
    private static SpawnState _spawnState;
    private class HordeThinkManager : MonoBehaviour
    {
        internal static void Create();
        private void Awake();
        internal void Update();
        internal void Destroy();
        private bool ShouldSpawn();
        private void CheckTimeTick();
    }

    internal class HordeMemberTickQueue : ObjectWorkQueue<HordeMember>
    {
        protected override void RunJob(HordeMember hordeMember);
        protected override bool ShouldAdd(HordeMember hordeMember);
    }

    internal class HordeTickQueue : ObjectWorkQueue<HordeManager>
    {
        protected override void RunJob(HordeManager hordeManager);
        protected override bool ShouldAdd(HordeManager hordeManager);
    }

    internal class HordeManager
    {
        internal static List<HordeManager> _allHordes;
        internal static HordeTickQueue _hordeTickQueue;
        internal List<HordeMember> members;
        internal Vector3 Destination;
        internal Vector3 AverageLocation;
        internal BaseCombatEntity PrimaryTarget;
        internal NpcPlayerInfo NpcPlayerInfo;
        internal AnimalInfo AnimalInfo;
        internal bool DebugMode { get; set; }
        private bool isRoaming;
        private bool isRegrouping;
        internal bool isDestroyed;
        private Vector3 initialSpawnPosition;
        private int initialMemberCount;
        private bool isLocalHorde;
        private float maximumRoamDistance;
        internal string hordeProfile;
        private float nextGrowthTime;
        private int maximumMemberCount;
        private float nextMergeTime;
        private float refreshRoamTime;
        private float verifyTargetTime;
        internal bool PathStateFailed { get; set; }
        private const float MERGE_COOLDOWN;
        private const float ROAM_REFRESH_RATE;
        private const float TARGET_VERIFY_RATE;
        internal static bool Create(Order order);
        internal void Destroy(bool permanent, bool killNpcs);
        internal void HordeTick();
        internal void EvaluateTarget(NpcPlayerInfo target);
        internal void EvaluateTarget(AnimalInfo target);
        internal void SetPrimaryTarget(BaseCombatEntity baseCombatEntity, NpcPlayerInfo info);
        internal void SetPrimaryTarget(BaseCombatEntity baseCombatEntity, AnimalInfo info);
        internal void SetPrimaryTarget(BaseCombatEntity baseCombatEntity);
        private void UpdateRoamTarget();
        internal Vector3 GetAverageVector();
        private const TerrainTopology.Enum DESTINATION_TOPOLOGY_MASK;
        private Vector3 GetRandomLocation(Vector3 from);
        private float GetMaximumSeperation();
        internal bool IsValidTarget(BaseCombatEntity baseCombatEntity);
        internal bool SpawnMember(Vector3 position, bool alreadyInitialized);
        internal void OnPlayerDeath(BasePlayer player, HordeMember hordeMember);
        internal void OnMemberDeath(HordeMember hordeMember, BaseCombatEntity initiator);
        private void TryGrowHorde();
        private void TryMergeHordes();
        public class Order
        {
            public Vector3 position;
            public int initialMemberCount;
            public int maximumMemberCount;
            public float maximumRoamDistance;
            public string hordeProfile;
            public Order(Vector3 position, int initialMemberCount, string hordeProfile);
            public Order(Vector3 position, int initialMemberCount, int maximumMemberCount, float maximumRoamDistance, string hordeProfile);
            private static Queue<Order> _queue;
            private static bool IsSpawning { get; set; }
            private static bool IsDespawning { get; set; }
            private static Coroutine SpawnRoutine { get; set; }
            private static Coroutine DespawnRoutine { get; set; }
            internal static void CreateOrder(Vector3 position, int initialMemberCount, int maximumMemberCount, float maximumRoamDistance, string hordeProfile);
            internal static void CreateOrder(Vector3 position, ConfigData.MonumentSpawn.MonumentSettings settings);
            internal static Coroutine BeginSpawning();
            internal static Coroutine BeginDespawning();
            internal static void StopSpawning();
            internal static void StopDespawning();
            private static IEnumerator ProcessSpawnOrders();
            private static IEnumerator ProcessDespawn();
            internal static void OnUnload();
        }

    }

    internal class HordeMember : MonoBehaviour
    {
        internal static HordeMemberTickQueue _memberTickQueue;
        internal HTNPlayer Entity { get; set; }
        internal MurdererDomain Domain { get; set; }
        internal MurdererContext Context { get; set; }
        internal MurdererMemory Memory { get; set; }
        internal HordeManager Manager { get; set; }
        internal Transform Transform { get; set; }
        internal List<AnimalInfo> animalsLineOfSight;
        internal float damageMultiplier;
        internal float lastSeenTargetTime;
        private bool lightsOn;
        private ItemContainer[] containers;
        private void Awake();
        private void OnDestroy();
        private void InitializeSensorsAndReasoners();
        private void InitializeNpc();
        private void GiveLoadout();
        private void LightCheck();
        private void LightToggle(bool on);
        private static Hash<string, float> _aimConeDefaults;
        private void UpdateProjectileAccuracy();
        private void ScheduleMemberUpdate();
        internal void OnTick();
        private void KillInSafeZone();
        private bool IsUnderWater();
        private float _lastAttackTime;
        private bool _isAttacking;
        private void BaseNpcTargetTick();
        private void TickWeapons();
        private void TickFirearm(float interval);
        private bool CanUseFirearmAtRange(float sqrRange);
        private void FireBurst(BaseProjectile proj, float time);
        private void FireFullAuto(BaseProjectile proj, float time);
        private void FireSingle(AttackEntity attackEnt, float time);
        private IEnumerator HoldTriggerLogic(BaseProjectile proj, float startTime, float triggerDownInterval);
        internal void OnTargetUpdated(NpcPlayerInfo playerInfo);
        internal void OnTargetUpdated(AnimalInfo animalInfo);
        private bool WantsToRoam();
        internal void SetRoamToDestination();
        internal void PrepareInventory();
        internal void MoveInventoryTo(LootableCorpse corpse);
    }

    public class PlayersInRangeSensor : INpcSensor
    {
        internal HordeMember HordeMember { get; set; }
        public float TickFrequency { get; set; }
        public float LastTickTime { get; set; }
        public const int MaxPlayers;
        public static BasePlayer[] PlayerQueryResults;
        public static int PlayerQueryResultCount;
        public PlayersInRangeSensor(HordeMember hordeMember);
        private Func<BasePlayer, bool> Query;
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    }

    public class AnimalsInRangeSensor : INpcSensor
    {
        internal HordeMember HordeMember { get; set; }
        public float LastTickTime { get; set; }
        public float TickFrequency { get; set; }
        public const int MaxAnimals;
        public static BaseNpc[] QueryResults;
        public static int QueryResultCount;
        public AnimalsInRangeSensor(HordeMember hordeMember);
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    }

    public class EnemyRangeReasoner : INpcReasoner
    {
        internal HordeMember HordeMember { get; set; }
        public float LastTickTime { get; set; }
        public float TickFrequency { get; set; }
        public EnemyRangeReasoner(HordeMember hordeMember);
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    }

    public class EnemyTargetReasoner : INpcReasoner
    {
        internal HordeMember HordeMember { get; set; }
        public float LastTickTime { get; set; }
        public float TickFrequency { get; set; }
        public EnemyTargetReasoner(HordeMember hordeMember);
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    }

    public class FireTacticReasoner : INpcReasoner
    {
        internal HordeMember HordeMember { get; set; }
        public float LastTickTime { get; set; }
        public float TickFrequency { get; set; }
        public FireTacticReasoner(HordeMember hordeMember);
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    }

    public class PreferredFightingRangeReasoner : INpcReasoner
    {
        internal HordeMember HordeMember { get; set; }
        public float LastTickTime { get; set; }
        public float TickFrequency { get; set; }
        public PreferredFightingRangeReasoner(HordeMember hordeMember);
        public static bool IsAtPreferredRange(MurdererContext context, float sqrDistance, AttackEntity firearm);
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    }

    public class AtLastKnownEnemyPlayerLocationReasoner : INpcReasoner
    {
        internal HordeMember HordeMember { get; set; }
        public float LastTickTime { get; set; }
        public float TickFrequency { get; set; }
        private NavMeshHit navMeshHit;
        public AtLastKnownEnemyPlayerLocationReasoner(HordeMember hordeMember);
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
        private Vector3 GetDestination();
    }

    public class EnemyPlayerLineOfSightReasoner : INpcReasoner
    {
        internal HordeMember HordeMember { get; set; }
        public float LastTickTime { get; set; }
        public float TickFrequency { get; set; }
        public EnemyPlayerLineOfSightReasoner(HordeMember hordeMember);
        public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    }

    public static Vector3 GetPreferredFightingPosition(MurdererContext context);
    [ChatCommand("horde")]
    private void cmdHorde(BasePlayer player, string command, string[] args);
    [ConsoleCommand("horde")]
    private void ccmdHorde(ConsoleSystem.Arg arg);
    private float nextCountTime;
    private string cachedString;
    private string GetInfoString();
    [ChatCommand("hordeinfo")]
    private void cmdHordeInfo(BasePlayer player, string command, string[] args);
    [ConsoleCommand("hordeinfo")]
    private void ccmdHordeInfo(ConsoleSystem.Arg arg);
    internal static ConfigData configData;
    internal class ConfigData
    {
        [JsonProperty(PropertyName = "Horde Options")]
        public HordeOptions Horde { get; set; }
        [JsonProperty(PropertyName = "Horde Member Options")]
        public MemberOptions Member { get; set; }
        [JsonProperty(PropertyName = "Loot Table")]
        public LootTable Loot { get; set; }
        [JsonProperty(PropertyName = "Monument Spawn Options")]
        public MonumentSpawn Monument { get; set; }
        [JsonProperty(PropertyName = "Timed Spawn Options")]
        public TimedSpawnOptions TimedSpawns { get; set; }
        [JsonProperty(PropertyName = "Horde Profiles (profile name, list of applicable loadouts)")]
        public Dictionary<string, List<string>> HordeProfiles { get; set; }
        public class TimedSpawnOptions
        {
            [JsonProperty(PropertyName = "Only allows spawns during the set time period")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Despawn hordes outside of the set time period")]
            public bool Despawn { get; set; }
            [JsonProperty(PropertyName = "Start time (0.0 - 24.0)")]
            public float Start { get; set; }
            [JsonProperty(PropertyName = "End time (0.0 - 24.0)")]
            public float End { get; set; }
        }

        public class HordeOptions
        {
            [JsonProperty(PropertyName = "Amount of zombies to spawn when a new horde is created")]
            public int InitialMemberCount { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of spawned zombies per horde")]
            public int MaximumMemberCount { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of hordes at any given time")]
            public int MaximumHordes { get; set; }
            [JsonProperty(PropertyName = "Amount of time from when a horde is destroyed until a new horde is created (seconds)")]
            public int RespawnTime { get; set; }
            [JsonProperty(PropertyName = "Amount of time before a horde grows in size")]
            public int GrowthRate { get; set; }
            [JsonProperty(PropertyName = "Add a zombie to the horde when a horde member kills a player")]
            public bool CreateOnDeath { get; set; }
            [JsonProperty(PropertyName = "Merge hordes together if they collide")]
            public bool MergeHordes { get; set; }
            [JsonProperty(PropertyName = "Spawn system (SpawnsDatabase, Random)")]
            public string SpawnType { get; set; }
            [JsonProperty(PropertyName = "Spawn file (only required when using SpawnsDatabase)")]
            public string SpawnFile { get; set; }
            [JsonProperty(PropertyName = "Amount of time a player needs to be outside of a zombies vision before it forgets about them")]
            public float ForgetTime { get; set; }
            [JsonProperty(PropertyName = "Force all hordes to roam locally")]
            public bool LocalRoam { get; set; }
            [JsonProperty(PropertyName = "Local roam distance")]
            public float RoamDistance { get; set; }
            [JsonProperty(PropertyName = "Use horde profiles for randomly spawned hordes")]
            public bool UseProfiles { get; set; }
        }

        public class MemberOptions
        {
            [JsonProperty(PropertyName = "Can target animals")]
            public bool TargetAnimals { get; set; }
            [JsonProperty(PropertyName = "Can be targeted by turrets")]
            public bool TargetedByTurrets { get; set; }
            [JsonProperty(PropertyName = "Can be targeted by turrets set to peacekeeper mode")]
            public bool TargetedByPeaceKeeperTurrets { get; set; }
            [JsonProperty(PropertyName = "Can be targeted by Bradley APC")]
            public bool TargetedByAPC { get; set; }
            [JsonProperty(PropertyName = "Can target other NPCs")]
            public bool TargetNPCs { get; set; }
            [JsonProperty(PropertyName = "Can target NPCs from HumanNPC")]
            public bool TargetHumanNPCs { get; set; }
            [JsonProperty(PropertyName = "Ignore sleeping players")]
            public bool IgnoreSleepers { get; set; }
            [JsonProperty(PropertyName = "Give all zombies glowing eyes")]
            public bool GiveGlowEyes { get; set; }
            [JsonProperty(PropertyName = "Headshots instantly kill zombie")]
            public bool HeadshotKills { get; set; }
            [JsonProperty(PropertyName = "Projectile weapon aimcone override (0 = disabled)")]
            public float AimconeOverride { get; set; }
            [JsonProperty(PropertyName = "Kill NPCs that are under water")]
            public bool KillUnderWater { get; set; }
            [JsonProperty(PropertyName = "Disable NPC dormant system. This will allow NPCs to move all the time, but at a cost to performance")]
            public bool DisableDormantSystem { get; set; }
            public List<Loadout> Loadouts { get; set; }
            public class Loadout
            {
                public string LoadoutID { get; set; }
                [JsonProperty(PropertyName = "Potential names for zombies using this loadout (chosen at random)")]
                public string[] Names { get; set; }
                [JsonProperty(PropertyName = "Damage multiplier")]
                public float DamageMultiplier { get; set; }
                public VitalStats Vitals { get; set; }
                public MovementStats Movement { get; set; }
                public SensoryStats Sensory { get; set; }
                public List<LootTable.InventoryItem> BeltItems { get; set; }
                public List<LootTable.InventoryItem> MainItems { get; set; }
                public List<LootTable.InventoryItem> WearItems { get; set; }
                public class VitalStats
                {
                    public float Health { get; set; }
                }

                public class MovementStats
                {
                    [JsonProperty(PropertyName = "Movement speed (running)")]
                    public float RunSpeed { get; set; }
                    [JsonProperty(PropertyName = "Movement speed (walking)")]
                    public float WalkSpeed { get; set; }
                    [JsonProperty(PropertyName = "Turn speed")]
                    public float TurnSpeed { get; set; }
                    [JsonProperty(PropertyName = "Duck speed")]
                    public float DuckSpeed { get; set; }
                    public float Acceleration { get; set; }
                }

                public class SensoryStats
                {
                    [JsonProperty(PropertyName = "Vision range")]
                    public float VisionRange { get; set; }
                    [JsonProperty(PropertyName = "Hearing range")]
                    public float HearingRange { get; set; }
                    [JsonProperty(PropertyName = "Field of view")]
                    public float FOV { get; set; }
                }

                [JsonIgnore]
                private MurdererDefinition _loadoutDefinition;
                [JsonIgnore]
                public MurdererDefinition LoadoutDefintion { get; set; }
                public Loadout();
                public Loadout(string loadoutID, MurdererDefinition definition);
            }

        }

        public class LootTable
        {
            [JsonProperty(PropertyName = "Drop inventory on death instead of random loot")]
            public bool DropInventory { get; set; }
            [JsonProperty(PropertyName = "Random loot table")]
            public RandomLoot Random { get; set; }
            public class InventoryItem
            {
                public string Shortname { get; set; }
                public ulong SkinID { get; set; }
                public int Amount { get; set; }
                [JsonProperty(PropertyName = "Attachments", NullValueHandling = NullValueHandling.Ignore)]
                public InventoryItem[] SubSpawn { get; set; }
            }

            public class RandomLoot
            {
                [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
                public int Minimum { get; set; }
                [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
                public int Maximum { get; set; }
                public List<LootDefinition> List { get; set; }
                public class LootDefinition
                {
                    public string Shortname { get; set; }
                    public int Minimum { get; set; }
                    public int Maximum { get; set; }
                    public ulong SkinID { get; set; }
                    [JsonProperty(PropertyName = "Spawn as blueprint")]
                    public bool IsBlueprint { get; set; }
                    [JsonProperty(PropertyName = "Probability (0.0 - 1.0)")]
                    public float Probability { get; set; }
                    [JsonProperty(PropertyName = "Spawn with")]
                    public LootDefinition Required { get; set; }
                    public int GetAmount();
                }

            }

        }

        public class MonumentSpawn
        {
            public MonumentSettings Airfield { get; set; }
            public MonumentSettings Dome { get; set; }
            public MonumentSettings Junkyard { get; set; }
            public MonumentSettings LargeHarbor { get; set; }
            public MonumentSettings GasStation { get; set; }
            public MonumentSettings Powerplant { get; set; }
            public MonumentSettings StoneQuarry { get; set; }
            public MonumentSettings SulfurQuarry { get; set; }
            public MonumentSettings HQMQuarry { get; set; }
            public MonumentSettings Radtown { get; set; }
            public MonumentSettings LaunchSite { get; set; }
            public MonumentSettings Satellite { get; set; }
            public MonumentSettings SmallHarbor { get; set; }
            public MonumentSettings Supermarket { get; set; }
            public MonumentSettings Trainyard { get; set; }
            public MonumentSettings Tunnels { get; set; }
            public MonumentSettings Warehouse { get; set; }
            public MonumentSettings WaterTreatment { get; set; }
            public class MonumentSettings : SpawnSettings
            {
                [JsonProperty(PropertyName = "Enable spawns at this monument")]
                public bool Enabled { get; set; }
            }

        }

        public class CustomSpawnPoints : SpawnSettings
        {
            public SerializedVector Location { get; set; }
            public class SerializedVector
            {
                public float X { get; set; }
                public float Y { get; set; }
                public float Z { get; set; }
                public SerializedVector();
                public SerializedVector(float x, float y, float z);
            }

        }

        public class SpawnSettings
        {
            [JsonProperty(PropertyName = "Distance that this horde can roam from their initial spawn point")]
            public float RoamDistance { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of members in this horde")]
            public int HordeSize { get; set; }
            [JsonProperty(PropertyName = "Horde profile")]
            public string Profile { get; set; }
        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    private List<ConfigData.MemberOptions.Loadout> BuildDefaultLoadouts();
    private ConfigData.LootTable.RandomLoot BuildDefaultLootTable();
    protected override void SaveConfig();
    private void UpdateConfigValues();
}

private class HordeThinkManager : MonoBehaviour
{
    internal static void Create();
    private void Awake();
    internal void Update();
    internal void Destroy();
    private bool ShouldSpawn();
    private void CheckTimeTick();
}

internal class HordeMemberTickQueue : ObjectWorkQueue<HordeMember>
{
    protected override void RunJob(HordeMember hordeMember);
    protected override bool ShouldAdd(HordeMember hordeMember);
}

internal class HordeTickQueue : ObjectWorkQueue<HordeManager>
{
    protected override void RunJob(HordeManager hordeManager);
    protected override bool ShouldAdd(HordeManager hordeManager);
}

internal class HordeManager
{
    internal static List<HordeManager> _allHordes;
    internal static HordeTickQueue _hordeTickQueue;
    internal List<HordeMember> members;
    internal Vector3 Destination;
    internal Vector3 AverageLocation;
    internal BaseCombatEntity PrimaryTarget;
    internal NpcPlayerInfo NpcPlayerInfo;
    internal AnimalInfo AnimalInfo;
    internal bool DebugMode { get; set; }
    private bool isRoaming;
    private bool isRegrouping;
    internal bool isDestroyed;
    private Vector3 initialSpawnPosition;
    private int initialMemberCount;
    private bool isLocalHorde;
    private float maximumRoamDistance;
    internal string hordeProfile;
    private float nextGrowthTime;
    private int maximumMemberCount;
    private float nextMergeTime;
    private float refreshRoamTime;
    private float verifyTargetTime;
    internal bool PathStateFailed { get; set; }
    private const float MERGE_COOLDOWN;
    private const float ROAM_REFRESH_RATE;
    private const float TARGET_VERIFY_RATE;
    internal static bool Create(Order order);
    internal void Destroy(bool permanent, bool killNpcs);
    internal void HordeTick();
    internal void EvaluateTarget(NpcPlayerInfo target);
    internal void EvaluateTarget(AnimalInfo target);
    internal void SetPrimaryTarget(BaseCombatEntity baseCombatEntity, NpcPlayerInfo info);
    internal void SetPrimaryTarget(BaseCombatEntity baseCombatEntity, AnimalInfo info);
    internal void SetPrimaryTarget(BaseCombatEntity baseCombatEntity);
    private void UpdateRoamTarget();
    internal Vector3 GetAverageVector();
    private const TerrainTopology.Enum DESTINATION_TOPOLOGY_MASK;
    private Vector3 GetRandomLocation(Vector3 from);
    private float GetMaximumSeperation();
    internal bool IsValidTarget(BaseCombatEntity baseCombatEntity);
    internal bool SpawnMember(Vector3 position, bool alreadyInitialized);
    internal void OnPlayerDeath(BasePlayer player, HordeMember hordeMember);
    internal void OnMemberDeath(HordeMember hordeMember, BaseCombatEntity initiator);
    private void TryGrowHorde();
    private void TryMergeHordes();
    public class Order
    {
        public Vector3 position;
        public int initialMemberCount;
        public int maximumMemberCount;
        public float maximumRoamDistance;
        public string hordeProfile;
        public Order(Vector3 position, int initialMemberCount, string hordeProfile);
        public Order(Vector3 position, int initialMemberCount, int maximumMemberCount, float maximumRoamDistance, string hordeProfile);
        private static Queue<Order> _queue;
        private static bool IsSpawning { get; set; }
        private static bool IsDespawning { get; set; }
        private static Coroutine SpawnRoutine { get; set; }
        private static Coroutine DespawnRoutine { get; set; }
        internal static void CreateOrder(Vector3 position, int initialMemberCount, int maximumMemberCount, float maximumRoamDistance, string hordeProfile);
        internal static void CreateOrder(Vector3 position, ConfigData.MonumentSpawn.MonumentSettings settings);
        internal static Coroutine BeginSpawning();
        internal static Coroutine BeginDespawning();
        internal static void StopSpawning();
        internal static void StopDespawning();
        private static IEnumerator ProcessSpawnOrders();
        private static IEnumerator ProcessDespawn();
        internal static void OnUnload();
    }

}

public class Order
{
    public Vector3 position;
    public int initialMemberCount;
    public int maximumMemberCount;
    public float maximumRoamDistance;
    public string hordeProfile;
    public Order(Vector3 position, int initialMemberCount, string hordeProfile);
    public Order(Vector3 position, int initialMemberCount, int maximumMemberCount, float maximumRoamDistance, string hordeProfile);
    private static Queue<Order> _queue;
    private static bool IsSpawning { get; set; }
    private static bool IsDespawning { get; set; }
    private static Coroutine SpawnRoutine { get; set; }
    private static Coroutine DespawnRoutine { get; set; }
    internal static void CreateOrder(Vector3 position, int initialMemberCount, int maximumMemberCount, float maximumRoamDistance, string hordeProfile);
    internal static void CreateOrder(Vector3 position, ConfigData.MonumentSpawn.MonumentSettings settings);
    internal static Coroutine BeginSpawning();
    internal static Coroutine BeginDespawning();
    internal static void StopSpawning();
    internal static void StopDespawning();
    private static IEnumerator ProcessSpawnOrders();
    private static IEnumerator ProcessDespawn();
    internal static void OnUnload();
}

internal class HordeMember : MonoBehaviour
{
    internal static HordeMemberTickQueue _memberTickQueue;
    internal HTNPlayer Entity { get; set; }
    internal MurdererDomain Domain { get; set; }
    internal MurdererContext Context { get; set; }
    internal MurdererMemory Memory { get; set; }
    internal HordeManager Manager { get; set; }
    internal Transform Transform { get; set; }
    internal List<AnimalInfo> animalsLineOfSight;
    internal float damageMultiplier;
    internal float lastSeenTargetTime;
    private bool lightsOn;
    private ItemContainer[] containers;
    private void Awake();
    private void OnDestroy();
    private void InitializeSensorsAndReasoners();
    private void InitializeNpc();
    private void GiveLoadout();
    private void LightCheck();
    private void LightToggle(bool on);
    private static Hash<string, float> _aimConeDefaults;
    private void UpdateProjectileAccuracy();
    private void ScheduleMemberUpdate();
    internal void OnTick();
    private void KillInSafeZone();
    private bool IsUnderWater();
    private float _lastAttackTime;
    private bool _isAttacking;
    private void BaseNpcTargetTick();
    private void TickWeapons();
    private void TickFirearm(float interval);
    private bool CanUseFirearmAtRange(float sqrRange);
    private void FireBurst(BaseProjectile proj, float time);
    private void FireFullAuto(BaseProjectile proj, float time);
    private void FireSingle(AttackEntity attackEnt, float time);
    private IEnumerator HoldTriggerLogic(BaseProjectile proj, float startTime, float triggerDownInterval);
    internal void OnTargetUpdated(NpcPlayerInfo playerInfo);
    internal void OnTargetUpdated(AnimalInfo animalInfo);
    private bool WantsToRoam();
    internal void SetRoamToDestination();
    internal void PrepareInventory();
    internal void MoveInventoryTo(LootableCorpse corpse);
}

public class PlayersInRangeSensor : INpcSensor
{
    internal HordeMember HordeMember { get; set; }
    public float TickFrequency { get; set; }
    public float LastTickTime { get; set; }
    public const int MaxPlayers;
    public static BasePlayer[] PlayerQueryResults;
    public static int PlayerQueryResultCount;
    public PlayersInRangeSensor(HordeMember hordeMember);
    private Func<BasePlayer, bool> Query;
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
}

public class AnimalsInRangeSensor : INpcSensor
{
    internal HordeMember HordeMember { get; set; }
    public float LastTickTime { get; set; }
    public float TickFrequency { get; set; }
    public const int MaxAnimals;
    public static BaseNpc[] QueryResults;
    public static int QueryResultCount;
    public AnimalsInRangeSensor(HordeMember hordeMember);
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
}

public class EnemyRangeReasoner : INpcReasoner
{
    internal HordeMember HordeMember { get; set; }
    public float LastTickTime { get; set; }
    public float TickFrequency { get; set; }
    public EnemyRangeReasoner(HordeMember hordeMember);
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
}

public class EnemyTargetReasoner : INpcReasoner
{
    internal HordeMember HordeMember { get; set; }
    public float LastTickTime { get; set; }
    public float TickFrequency { get; set; }
    public EnemyTargetReasoner(HordeMember hordeMember);
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
}

public class FireTacticReasoner : INpcReasoner
{
    internal HordeMember HordeMember { get; set; }
    public float LastTickTime { get; set; }
    public float TickFrequency { get; set; }
    public FireTacticReasoner(HordeMember hordeMember);
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
}

public class PreferredFightingRangeReasoner : INpcReasoner
{
    internal HordeMember HordeMember { get; set; }
    public float LastTickTime { get; set; }
    public float TickFrequency { get; set; }
    public PreferredFightingRangeReasoner(HordeMember hordeMember);
    public static bool IsAtPreferredRange(MurdererContext context, float sqrDistance, AttackEntity firearm);
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
}

public class AtLastKnownEnemyPlayerLocationReasoner : INpcReasoner
{
    internal HordeMember HordeMember { get; set; }
    public float LastTickTime { get; set; }
    public float TickFrequency { get; set; }
    private NavMeshHit navMeshHit;
    public AtLastKnownEnemyPlayerLocationReasoner(HordeMember hordeMember);
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
    private Vector3 GetDestination();
}

public class EnemyPlayerLineOfSightReasoner : INpcReasoner
{
    internal HordeMember HordeMember { get; set; }
    public float LastTickTime { get; set; }
    public float TickFrequency { get; set; }
    public EnemyPlayerLineOfSightReasoner(HordeMember hordeMember);
    public void Tick(IHTNAgent htnAgent, float deltaTime, float time);
}

internal class ConfigData
{
    [JsonProperty(PropertyName = "Horde Options")]
    public HordeOptions Horde { get; set; }
    [JsonProperty(PropertyName = "Horde Member Options")]
    public MemberOptions Member { get; set; }
    [JsonProperty(PropertyName = "Loot Table")]
    public LootTable Loot { get; set; }
    [JsonProperty(PropertyName = "Monument Spawn Options")]
    public MonumentSpawn Monument { get; set; }
    [JsonProperty(PropertyName = "Timed Spawn Options")]
    public TimedSpawnOptions TimedSpawns { get; set; }
    [JsonProperty(PropertyName = "Horde Profiles (profile name, list of applicable loadouts)")]
    public Dictionary<string, List<string>> HordeProfiles { get; set; }
    public class TimedSpawnOptions
    {
        [JsonProperty(PropertyName = "Only allows spawns during the set time period")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Despawn hordes outside of the set time period")]
        public bool Despawn { get; set; }
        [JsonProperty(PropertyName = "Start time (0.0 - 24.0)")]
        public float Start { get; set; }
        [JsonProperty(PropertyName = "End time (0.0 - 24.0)")]
        public float End { get; set; }
    }

    public class HordeOptions
    {
        [JsonProperty(PropertyName = "Amount of zombies to spawn when a new horde is created")]
        public int InitialMemberCount { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of spawned zombies per horde")]
        public int MaximumMemberCount { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of hordes at any given time")]
        public int MaximumHordes { get; set; }
        [JsonProperty(PropertyName = "Amount of time from when a horde is destroyed until a new horde is created (seconds)")]
        public int RespawnTime { get; set; }
        [JsonProperty(PropertyName = "Amount of time before a horde grows in size")]
        public int GrowthRate { get; set; }
        [JsonProperty(PropertyName = "Add a zombie to the horde when a horde member kills a player")]
        public bool CreateOnDeath { get; set; }
        [JsonProperty(PropertyName = "Merge hordes together if they collide")]
        public bool MergeHordes { get; set; }
        [JsonProperty(PropertyName = "Spawn system (SpawnsDatabase, Random)")]
        public string SpawnType { get; set; }
        [JsonProperty(PropertyName = "Spawn file (only required when using SpawnsDatabase)")]
        public string SpawnFile { get; set; }
        [JsonProperty(PropertyName = "Amount of time a player needs to be outside of a zombies vision before it forgets about them")]
        public float ForgetTime { get; set; }
        [JsonProperty(PropertyName = "Force all hordes to roam locally")]
        public bool LocalRoam { get; set; }
        [JsonProperty(PropertyName = "Local roam distance")]
        public float RoamDistance { get; set; }
        [JsonProperty(PropertyName = "Use horde profiles for randomly spawned hordes")]
        public bool UseProfiles { get; set; }
    }

    public class MemberOptions
    {
        [JsonProperty(PropertyName = "Can target animals")]
        public bool TargetAnimals { get; set; }
        [JsonProperty(PropertyName = "Can be targeted by turrets")]
        public bool TargetedByTurrets { get; set; }
        [JsonProperty(PropertyName = "Can be targeted by turrets set to peacekeeper mode")]
        public bool TargetedByPeaceKeeperTurrets { get; set; }
        [JsonProperty(PropertyName = "Can be targeted by Bradley APC")]
        public bool TargetedByAPC { get; set; }
        [JsonProperty(PropertyName = "Can target other NPCs")]
        public bool TargetNPCs { get; set; }
        [JsonProperty(PropertyName = "Can target NPCs from HumanNPC")]
        public bool TargetHumanNPCs { get; set; }
        [JsonProperty(PropertyName = "Ignore sleeping players")]
        public bool IgnoreSleepers { get; set; }
        [JsonProperty(PropertyName = "Give all zombies glowing eyes")]
        public bool GiveGlowEyes { get; set; }
        [JsonProperty(PropertyName = "Headshots instantly kill zombie")]
        public bool HeadshotKills { get; set; }
        [JsonProperty(PropertyName = "Projectile weapon aimcone override (0 = disabled)")]
        public float AimconeOverride { get; set; }
        [JsonProperty(PropertyName = "Kill NPCs that are under water")]
        public bool KillUnderWater { get; set; }
        [JsonProperty(PropertyName = "Disable NPC dormant system. This will allow NPCs to move all the time, but at a cost to performance")]
        public bool DisableDormantSystem { get; set; }
        public List<Loadout> Loadouts { get; set; }
        public class Loadout
        {
            public string LoadoutID { get; set; }
            [JsonProperty(PropertyName = "Potential names for zombies using this loadout (chosen at random)")]
            public string[] Names { get; set; }
            [JsonProperty(PropertyName = "Damage multiplier")]
            public float DamageMultiplier { get; set; }
            public VitalStats Vitals { get; set; }
            public MovementStats Movement { get; set; }
            public SensoryStats Sensory { get; set; }
            public List<LootTable.InventoryItem> BeltItems { get; set; }
            public List<LootTable.InventoryItem> MainItems { get; set; }
            public List<LootTable.InventoryItem> WearItems { get; set; }
            public class VitalStats
            {
                public float Health { get; set; }
            }

            public class MovementStats
            {
                [JsonProperty(PropertyName = "Movement speed (running)")]
                public float RunSpeed { get; set; }
                [JsonProperty(PropertyName = "Movement speed (walking)")]
                public float WalkSpeed { get; set; }
                [JsonProperty(PropertyName = "Turn speed")]
                public float TurnSpeed { get; set; }
                [JsonProperty(PropertyName = "Duck speed")]
                public float DuckSpeed { get; set; }
                public float Acceleration { get; set; }
            }

            public class SensoryStats
            {
                [JsonProperty(PropertyName = "Vision range")]
                public float VisionRange { get; set; }
                [JsonProperty(PropertyName = "Hearing range")]
                public float HearingRange { get; set; }
                [JsonProperty(PropertyName = "Field of view")]
                public float FOV { get; set; }
            }

            [JsonIgnore]
            private MurdererDefinition _loadoutDefinition;
            [JsonIgnore]
            public MurdererDefinition LoadoutDefintion { get; set; }
            public Loadout();
            public Loadout(string loadoutID, MurdererDefinition definition);
        }

    }

    public class LootTable
    {
        [JsonProperty(PropertyName = "Drop inventory on death instead of random loot")]
        public bool DropInventory { get; set; }
        [JsonProperty(PropertyName = "Random loot table")]
        public RandomLoot Random { get; set; }
        public class InventoryItem
        {
            public string Shortname { get; set; }
            public ulong SkinID { get; set; }
            public int Amount { get; set; }
            [JsonProperty(PropertyName = "Attachments", NullValueHandling = NullValueHandling.Ignore)]
            public InventoryItem[] SubSpawn { get; set; }
        }

        public class RandomLoot
        {
            [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
            public int Minimum { get; set; }
            [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
            public int Maximum { get; set; }
            public List<LootDefinition> List { get; set; }
            public class LootDefinition
            {
                public string Shortname { get; set; }
                public int Minimum { get; set; }
                public int Maximum { get; set; }
                public ulong SkinID { get; set; }
                [JsonProperty(PropertyName = "Spawn as blueprint")]
                public bool IsBlueprint { get; set; }
                [JsonProperty(PropertyName = "Probability (0.0 - 1.0)")]
                public float Probability { get; set; }
                [JsonProperty(PropertyName = "Spawn with")]
                public LootDefinition Required { get; set; }
                public int GetAmount();
            }

        }

    }

    public class MonumentSpawn
    {
        public MonumentSettings Airfield { get; set; }
        public MonumentSettings Dome { get; set; }
        public MonumentSettings Junkyard { get; set; }
        public MonumentSettings LargeHarbor { get; set; }
        public MonumentSettings GasStation { get; set; }
        public MonumentSettings Powerplant { get; set; }
        public MonumentSettings StoneQuarry { get; set; }
        public MonumentSettings SulfurQuarry { get; set; }
        public MonumentSettings HQMQuarry { get; set; }
        public MonumentSettings Radtown { get; set; }
        public MonumentSettings LaunchSite { get; set; }
        public MonumentSettings Satellite { get; set; }
        public MonumentSettings SmallHarbor { get; set; }
        public MonumentSettings Supermarket { get; set; }
        public MonumentSettings Trainyard { get; set; }
        public MonumentSettings Tunnels { get; set; }
        public MonumentSettings Warehouse { get; set; }
        public MonumentSettings WaterTreatment { get; set; }
        public class MonumentSettings : SpawnSettings
        {
            [JsonProperty(PropertyName = "Enable spawns at this monument")]
            public bool Enabled { get; set; }
        }

    }

    public class CustomSpawnPoints : SpawnSettings
    {
        public SerializedVector Location { get; set; }
        public class SerializedVector
        {
            public float X { get; set; }
            public float Y { get; set; }
            public float Z { get; set; }
            public SerializedVector();
            public SerializedVector(float x, float y, float z);
        }

    }

    public class SpawnSettings
    {
        [JsonProperty(PropertyName = "Distance that this horde can roam from their initial spawn point")]
        public float RoamDistance { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of members in this horde")]
        public int HordeSize { get; set; }
        [JsonProperty(PropertyName = "Horde profile")]
        public string Profile { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class TimedSpawnOptions
{
    [JsonProperty(PropertyName = "Only allows spawns during the set time period")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Despawn hordes outside of the set time period")]
    public bool Despawn { get; set; }
    [JsonProperty(PropertyName = "Start time (0.0 - 24.0)")]
    public float Start { get; set; }
    [JsonProperty(PropertyName = "End time (0.0 - 24.0)")]
    public float End { get; set; }
}

public class HordeOptions
{
    [JsonProperty(PropertyName = "Amount of zombies to spawn when a new horde is created")]
    public int InitialMemberCount { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of spawned zombies per horde")]
    public int MaximumMemberCount { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of hordes at any given time")]
    public int MaximumHordes { get; set; }
    [JsonProperty(PropertyName = "Amount of time from when a horde is destroyed until a new horde is created (seconds)")]
    public int RespawnTime { get; set; }
    [JsonProperty(PropertyName = "Amount of time before a horde grows in size")]
    public int GrowthRate { get; set; }
    [JsonProperty(PropertyName = "Add a zombie to the horde when a horde member kills a player")]
    public bool CreateOnDeath { get; set; }
    [JsonProperty(PropertyName = "Merge hordes together if they collide")]
    public bool MergeHordes { get; set; }
    [JsonProperty(PropertyName = "Spawn system (SpawnsDatabase, Random)")]
    public string SpawnType { get; set; }
    [JsonProperty(PropertyName = "Spawn file (only required when using SpawnsDatabase)")]
    public string SpawnFile { get; set; }
    [JsonProperty(PropertyName = "Amount of time a player needs to be outside of a zombies vision before it forgets about them")]
    public float ForgetTime { get; set; }
    [JsonProperty(PropertyName = "Force all hordes to roam locally")]
    public bool LocalRoam { get; set; }
    [JsonProperty(PropertyName = "Local roam distance")]
    public float RoamDistance { get; set; }
    [JsonProperty(PropertyName = "Use horde profiles for randomly spawned hordes")]
    public bool UseProfiles { get; set; }
}

public class MemberOptions
{
    [JsonProperty(PropertyName = "Can target animals")]
    public bool TargetAnimals { get; set; }
    [JsonProperty(PropertyName = "Can be targeted by turrets")]
    public bool TargetedByTurrets { get; set; }
    [JsonProperty(PropertyName = "Can be targeted by turrets set to peacekeeper mode")]
    public bool TargetedByPeaceKeeperTurrets { get; set; }
    [JsonProperty(PropertyName = "Can be targeted by Bradley APC")]
    public bool TargetedByAPC { get; set; }
    [JsonProperty(PropertyName = "Can target other NPCs")]
    public bool TargetNPCs { get; set; }
    [JsonProperty(PropertyName = "Can target NPCs from HumanNPC")]
    public bool TargetHumanNPCs { get; set; }
    [JsonProperty(PropertyName = "Ignore sleeping players")]
    public bool IgnoreSleepers { get; set; }
    [JsonProperty(PropertyName = "Give all zombies glowing eyes")]
    public bool GiveGlowEyes { get; set; }
    [JsonProperty(PropertyName = "Headshots instantly kill zombie")]
    public bool HeadshotKills { get; set; }
    [JsonProperty(PropertyName = "Projectile weapon aimcone override (0 = disabled)")]
    public float AimconeOverride { get; set; }
    [JsonProperty(PropertyName = "Kill NPCs that are under water")]
    public bool KillUnderWater { get; set; }
    [JsonProperty(PropertyName = "Disable NPC dormant system. This will allow NPCs to move all the time, but at a cost to performance")]
    public bool DisableDormantSystem { get; set; }
    public List<Loadout> Loadouts { get; set; }
    public class Loadout
    {
        public string LoadoutID { get; set; }
        [JsonProperty(PropertyName = "Potential names for zombies using this loadout (chosen at random)")]
        public string[] Names { get; set; }
        [JsonProperty(PropertyName = "Damage multiplier")]
        public float DamageMultiplier { get; set; }
        public VitalStats Vitals { get; set; }
        public MovementStats Movement { get; set; }
        public SensoryStats Sensory { get; set; }
        public List<LootTable.InventoryItem> BeltItems { get; set; }
        public List<LootTable.InventoryItem> MainItems { get; set; }
        public List<LootTable.InventoryItem> WearItems { get; set; }
        public class VitalStats
        {
            public float Health { get; set; }
        }

        public class MovementStats
        {
            [JsonProperty(PropertyName = "Movement speed (running)")]
            public float RunSpeed { get; set; }
            [JsonProperty(PropertyName = "Movement speed (walking)")]
            public float WalkSpeed { get; set; }
            [JsonProperty(PropertyName = "Turn speed")]
            public float TurnSpeed { get; set; }
            [JsonProperty(PropertyName = "Duck speed")]
            public float DuckSpeed { get; set; }
            public float Acceleration { get; set; }
        }

        public class SensoryStats
        {
            [JsonProperty(PropertyName = "Vision range")]
            public float VisionRange { get; set; }
            [JsonProperty(PropertyName = "Hearing range")]
            public float HearingRange { get; set; }
            [JsonProperty(PropertyName = "Field of view")]
            public float FOV { get; set; }
        }

        [JsonIgnore]
        private MurdererDefinition _loadoutDefinition;
        [JsonIgnore]
        public MurdererDefinition LoadoutDefintion { get; set; }
        public Loadout();
        public Loadout(string loadoutID, MurdererDefinition definition);
    }

}

public class Loadout
{
    public string LoadoutID { get; set; }
    [JsonProperty(PropertyName = "Potential names for zombies using this loadout (chosen at random)")]
    public string[] Names { get; set; }
    [JsonProperty(PropertyName = "Damage multiplier")]
    public float DamageMultiplier { get; set; }
    public VitalStats Vitals { get; set; }
    public MovementStats Movement { get; set; }
    public SensoryStats Sensory { get; set; }
    public List<LootTable.InventoryItem> BeltItems { get; set; }
    public List<LootTable.InventoryItem> MainItems { get; set; }
    public List<LootTable.InventoryItem> WearItems { get; set; }
    public class VitalStats
    {
        public float Health { get; set; }
    }

    public class MovementStats
    {
        [JsonProperty(PropertyName = "Movement speed (running)")]
        public float RunSpeed { get; set; }
        [JsonProperty(PropertyName = "Movement speed (walking)")]
        public float WalkSpeed { get; set; }
        [JsonProperty(PropertyName = "Turn speed")]
        public float TurnSpeed { get; set; }
        [JsonProperty(PropertyName = "Duck speed")]
        public float DuckSpeed { get; set; }
        public float Acceleration { get; set; }
    }

    public class SensoryStats
    {
        [JsonProperty(PropertyName = "Vision range")]
        public float VisionRange { get; set; }
        [JsonProperty(PropertyName = "Hearing range")]
        public float HearingRange { get; set; }
        [JsonProperty(PropertyName = "Field of view")]
        public float FOV { get; set; }
    }

    [JsonIgnore]
    private MurdererDefinition _loadoutDefinition;
    [JsonIgnore]
    public MurdererDefinition LoadoutDefintion { get; set; }
    public Loadout();
    public Loadout(string loadoutID, MurdererDefinition definition);
}

public class VitalStats
{
    public float Health { get; set; }
}

public class MovementStats
{
    [JsonProperty(PropertyName = "Movement speed (running)")]
    public float RunSpeed { get; set; }
    [JsonProperty(PropertyName = "Movement speed (walking)")]
    public float WalkSpeed { get; set; }
    [JsonProperty(PropertyName = "Turn speed")]
    public float TurnSpeed { get; set; }
    [JsonProperty(PropertyName = "Duck speed")]
    public float DuckSpeed { get; set; }
    public float Acceleration { get; set; }
}

public class SensoryStats
{
    [JsonProperty(PropertyName = "Vision range")]
    public float VisionRange { get; set; }
    [JsonProperty(PropertyName = "Hearing range")]
    public float HearingRange { get; set; }
    [JsonProperty(PropertyName = "Field of view")]
    public float FOV { get; set; }
}

public class LootTable
{
    [JsonProperty(PropertyName = "Drop inventory on death instead of random loot")]
    public bool DropInventory { get; set; }
    [JsonProperty(PropertyName = "Random loot table")]
    public RandomLoot Random { get; set; }
    public class InventoryItem
    {
        public string Shortname { get; set; }
        public ulong SkinID { get; set; }
        public int Amount { get; set; }
        [JsonProperty(PropertyName = "Attachments", NullValueHandling = NullValueHandling.Ignore)]
        public InventoryItem[] SubSpawn { get; set; }
    }

    public class RandomLoot
    {
        [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
        public int Minimum { get; set; }
        [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
        public int Maximum { get; set; }
        public List<LootDefinition> List { get; set; }
        public class LootDefinition
        {
            public string Shortname { get; set; }
            public int Minimum { get; set; }
            public int Maximum { get; set; }
            public ulong SkinID { get; set; }
            [JsonProperty(PropertyName = "Spawn as blueprint")]
            public bool IsBlueprint { get; set; }
            [JsonProperty(PropertyName = "Probability (0.0 - 1.0)")]
            public float Probability { get; set; }
            [JsonProperty(PropertyName = "Spawn with")]
            public LootDefinition Required { get; set; }
            public int GetAmount();
        }

    }

}

public class InventoryItem
{
    public string Shortname { get; set; }
    public ulong SkinID { get; set; }
    public int Amount { get; set; }
    [JsonProperty(PropertyName = "Attachments", NullValueHandling = NullValueHandling.Ignore)]
    public InventoryItem[] SubSpawn { get; set; }
}

public class RandomLoot
{
    [JsonProperty(PropertyName = "Minimum amount of items to spawn")]
    public int Minimum { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of items to spawn")]
    public int Maximum { get; set; }
    public List<LootDefinition> List { get; set; }
    public class LootDefinition
    {
        public string Shortname { get; set; }
        public int Minimum { get; set; }
        public int Maximum { get; set; }
        public ulong SkinID { get; set; }
        [JsonProperty(PropertyName = "Spawn as blueprint")]
        public bool IsBlueprint { get; set; }
        [JsonProperty(PropertyName = "Probability (0.0 - 1.0)")]
        public float Probability { get; set; }
        [JsonProperty(PropertyName = "Spawn with")]
        public LootDefinition Required { get; set; }
        public int GetAmount();
    }

}

public class LootDefinition
{
    public string Shortname { get; set; }
    public int Minimum { get; set; }
    public int Maximum { get; set; }
    public ulong SkinID { get; set; }
    [JsonProperty(PropertyName = "Spawn as blueprint")]
    public bool IsBlueprint { get; set; }
    [JsonProperty(PropertyName = "Probability (0.0 - 1.0)")]
    public float Probability { get; set; }
    [JsonProperty(PropertyName = "Spawn with")]
    public LootDefinition Required { get; set; }
    public int GetAmount();
}

public class MonumentSpawn
{
    public MonumentSettings Airfield { get; set; }
    public MonumentSettings Dome { get; set; }
    public MonumentSettings Junkyard { get; set; }
    public MonumentSettings LargeHarbor { get; set; }
    public MonumentSettings GasStation { get; set; }
    public MonumentSettings Powerplant { get; set; }
    public MonumentSettings StoneQuarry { get; set; }
    public MonumentSettings SulfurQuarry { get; set; }
    public MonumentSettings HQMQuarry { get; set; }
    public MonumentSettings Radtown { get; set; }
    public MonumentSettings LaunchSite { get; set; }
    public MonumentSettings Satellite { get; set; }
    public MonumentSettings SmallHarbor { get; set; }
    public MonumentSettings Supermarket { get; set; }
    public MonumentSettings Trainyard { get; set; }
    public MonumentSettings Tunnels { get; set; }
    public MonumentSettings Warehouse { get; set; }
    public MonumentSettings WaterTreatment { get; set; }
    public class MonumentSettings : SpawnSettings
    {
        [JsonProperty(PropertyName = "Enable spawns at this monument")]
        public bool Enabled { get; set; }
    }

}

public class MonumentSettings : SpawnSettings
{
    [JsonProperty(PropertyName = "Enable spawns at this monument")]
    public bool Enabled { get; set; }
}

public class CustomSpawnPoints : SpawnSettings
{
    public SerializedVector Location { get; set; }
    public class SerializedVector
    {
        public float X { get; set; }
        public float Y { get; set; }
        public float Z { get; set; }
        public SerializedVector();
        public SerializedVector(float x, float y, float z);
    }

}

public class SerializedVector
{
    public float X { get; set; }
    public float Y { get; set; }
    public float Z { get; set; }
    public SerializedVector();
    public SerializedVector(float x, float y, float z);
}

public class SpawnSettings
{
    [JsonProperty(PropertyName = "Distance that this horde can roam from their initial spawn point")]
    public float RoamDistance { get; set; }
    [JsonProperty(PropertyName = "Maximum amount of members in this horde")]
    public int HordeSize { get; set; }
    [JsonProperty(PropertyName = "Horde profile")]
    public string Profile { get; set; }
}


```

---

## ZoneCommand

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("ZoneCommand", "deer_SWAG", "0.1.0", ResourceId = 1254)]
[Description("Executes the commands when a player is entering a zone")]
 class ZoneCommand : RustPlugin
{
    const string PermissionName;
     class StoredData
    {
        public HashSet<Zone> Zones;
        public StoredData();
        public void Add(Zone zone);
        public void Remove(Zone zone);
    }

     class Zone
    {
        public string Id;
        public Methods Method;
        public Modes Mode;
        public int Amount;
        public string UserGroup;
        public List<string> Commands;
        public HashSet<ZonePlayer> Players;
        public Zone();
        public Zone(string Id);
        public void Add(string command);
        public void Add(ZonePlayer player);
    }

     class ZonePlayer
    {
        public ulong UserId;
        public int Count;
        public ZonePlayer();
    }

     StoredData data;
     void LoadDefaultMessages();
     void OnServerInitialized();
     void Loaded();
     void Unload();
     void OnEnterZone(string zoneID, BasePlayer player);
     void OnExitZone(string zoneID, BasePlayer player);
     void ExecuteZone(Zone zone, BasePlayer player);
    [ChatCommand("zcmd")]
     void cmdChat(BasePlayer player, string command, string[] args);
    [ConsoleCommand("zone.command")]
     void cmdConsole(ConsoleSystem.Arg arg);
     void AddCommand(QueryLanguage.Parser parser, string id, BasePlayer player);
     void RemoveCommand(QueryLanguage.Parser parser, string id, BasePlayer player);
     void ListCommand(QueryLanguage.Parser parser, BasePlayer player);
     void AddZone(BasePlayer player, string[] args, Methods method, bool onlyAmount, bool hasMethod);
     void RemoveZone(BasePlayer player, string[] args);
     void PrintZoneList(BasePlayer player);
     void SendHelpText(BasePlayer player);
     bool IsPluginExists(string name);
     string Lang(string key, BasePlayer player);
     void SaveData();
     bool IsDigitsOnly(string str);
     string ArrayToString(string[] array);
     bool IsPlayerPermitted(BasePlayer player, string permissionName);
     class QueryLanguage
    {
        public class Lexem
        {
            public LexemType Type;
            public string Value;
            public int Offset;
        }

         class LexemDefenition
        {
            public LexemType Type;
            public T Representation;
            public LexemDefenition(T representation, LexemType type);
        }

         class DynamicLexemDefenition : LexemDefenition<Regex>
        {
            public DynamicLexemDefenition(string representation, LexemType type);
        }

         class StaticLexemDefenition : LexemDefenition<string>
        {
            public StaticLexemDefenition(string representation, LexemType type);
        }

        static class LexemDefenitions
        {
            public static StaticLexemDefenition[] Static;
            public static DynamicLexemDefenition[] Dynamic;
        }

        public class Lexer
        {
            public IEnumerable<Lexem> Lexems { get; set; }
             string source;
             int offset;
            public void Parse(string src);
             Lexem ProcessStatic();
             Lexem ProcessDynamic();
             bool InBounds();
        }

        public class Parser
        {
             List<Lexem> lexems;
            public class Time
            {
                public TimeSpan? From;
                public TimeSpan? To;
                public Time(TimeSpan? from, TimeSpan? to);
            }

            public class ExecutionAndCount
            {
                public LexemType Execution1;
                public LexemType Execution2;
                public LexemType Execution3;
                public int Count;
                public ExecutionAndCount(LexemType ex1, LexemType ex2, LexemType ex3, int count);
            }

            public Parser(List<Lexem> lexems);
            public LexemType ParseCommand();
            public LexemType ParseRule();
            public string ParseId();
            public ExecutionAndCount ParseExecutionAndCount();
             int ParseCount(int position);
            public List<string> ParseCommands();
            public string ParseUserGroup();
            public Time ParseTime();
        }

    }

}

 class StoredData
{
    public HashSet<Zone> Zones;
    public StoredData();
    public void Add(Zone zone);
    public void Remove(Zone zone);
}

 class Zone
{
    public string Id;
    public Methods Method;
    public Modes Mode;
    public int Amount;
    public string UserGroup;
    public List<string> Commands;
    public HashSet<ZonePlayer> Players;
    public Zone();
    public Zone(string Id);
    public void Add(string command);
    public void Add(ZonePlayer player);
}

 class ZonePlayer
{
    public ulong UserId;
    public int Count;
    public ZonePlayer();
}

 class QueryLanguage
{
    public class Lexem
    {
        public LexemType Type;
        public string Value;
        public int Offset;
    }

     class LexemDefenition
    {
        public LexemType Type;
        public T Representation;
        public LexemDefenition(T representation, LexemType type);
    }

     class DynamicLexemDefenition : LexemDefenition<Regex>
    {
        public DynamicLexemDefenition(string representation, LexemType type);
    }

     class StaticLexemDefenition : LexemDefenition<string>
    {
        public StaticLexemDefenition(string representation, LexemType type);
    }

    static class LexemDefenitions
    {
        public static StaticLexemDefenition[] Static;
        public static DynamicLexemDefenition[] Dynamic;
    }

    public class Lexer
    {
        public IEnumerable<Lexem> Lexems { get; set; }
         string source;
         int offset;
        public void Parse(string src);
         Lexem ProcessStatic();
         Lexem ProcessDynamic();
         bool InBounds();
    }

    public class Parser
    {
         List<Lexem> lexems;
        public class Time
        {
            public TimeSpan? From;
            public TimeSpan? To;
            public Time(TimeSpan? from, TimeSpan? to);
        }

        public class ExecutionAndCount
        {
            public LexemType Execution1;
            public LexemType Execution2;
            public LexemType Execution3;
            public int Count;
            public ExecutionAndCount(LexemType ex1, LexemType ex2, LexemType ex3, int count);
        }

        public Parser(List<Lexem> lexems);
        public LexemType ParseCommand();
        public LexemType ParseRule();
        public string ParseId();
        public ExecutionAndCount ParseExecutionAndCount();
         int ParseCount(int position);
        public List<string> ParseCommands();
        public string ParseUserGroup();
        public Time ParseTime();
    }

}

public class Lexem
{
    public LexemType Type;
    public string Value;
    public int Offset;
}

 class LexemDefenition
{
    public LexemType Type;
    public T Representation;
    public LexemDefenition(T representation, LexemType type);
}

 class DynamicLexemDefenition : LexemDefenition<Regex>
{
    public DynamicLexemDefenition(string representation, LexemType type);
}

 class StaticLexemDefenition : LexemDefenition<string>
{
    public StaticLexemDefenition(string representation, LexemType type);
}

static class LexemDefenitions
{
    public static StaticLexemDefenition[] Static;
    public static DynamicLexemDefenition[] Dynamic;
}

public class Lexer
{
    public IEnumerable<Lexem> Lexems { get; set; }
     string source;
     int offset;
    public void Parse(string src);
     Lexem ProcessStatic();
     Lexem ProcessDynamic();
     bool InBounds();
}

public class Parser
{
     List<Lexem> lexems;
    public class Time
    {
        public TimeSpan? From;
        public TimeSpan? To;
        public Time(TimeSpan? from, TimeSpan? to);
    }

    public class ExecutionAndCount
    {
        public LexemType Execution1;
        public LexemType Execution2;
        public LexemType Execution3;
        public int Count;
        public ExecutionAndCount(LexemType ex1, LexemType ex2, LexemType ex3, int count);
    }

    public Parser(List<Lexem> lexems);
    public LexemType ParseCommand();
    public LexemType ParseRule();
    public string ParseId();
    public ExecutionAndCount ParseExecutionAndCount();
     int ParseCount(int position);
    public List<string> ParseCommands();
    public string ParseUserGroup();
    public Time ParseTime();
}

public class Time
{
    public TimeSpan? From;
    public TimeSpan? To;
    public Time(TimeSpan? from, TimeSpan? to);
}

public class ExecutionAndCount
{
    public LexemType Execution1;
    public LexemType Execution2;
    public LexemType Execution3;
    public int Count;
    public ExecutionAndCount(LexemType ex1, LexemType ex2, LexemType ex3, int count);
}


```

---

## ZoneInfoGUI

```csharp
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("ZoneInfoGUI", "SNAK84", "1.0.1")]
public class ZoneInfoGUI : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
    private ConfigData Conf;
    private class ConfigData
    {
        public string AnchorMax;
        public string AnchorMin;
        public int FontSize;
        public List<ZoneConfig> ZonesConfig;
    }

    private class ZoneConfig
    {
        public bool Enable;
        public string ZoneID;
        public string Text;
        public string Color;
        public string TextColor;
    }

    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void Init();
     void OnPluginLoaded(Plugin name);
     void OnServerInitialized();
     void OnPlayerSleepEnded(BasePlayer player);
     void OnEnterZone(string ZoneID, BasePlayer player);
     void OnExitZone(string ZoneID, BasePlayer player);
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     ZoneConfig GetZone(string ZoneId);
     void DestroyUI(BasePlayer player);
     void CreateUI(BasePlayer player, ZoneConfig Zone);
}

private class ConfigData
{
    public string AnchorMax;
    public string AnchorMin;
    public int FontSize;
    public List<ZoneConfig> ZonesConfig;
}

private class ZoneConfig
{
    public bool Enable;
    public string ZoneID;
    public string Text;
    public string Color;
    public string TextColor;
}


```

---

## ZoneManager

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Zone Manager", "k1lly0u", "3.0.23")]
[Description("An advanced management system for creating in-game zones")]
public class ZoneManager : RustPlugin
{
    [PluginReference]
     Plugin Backpacks;
     Plugin PopupNotifications;
     Plugin Spawns;
    private StoredData storedData;
    private DynamicConfigFile data;
    private readonly Hash<string, Zone> zones;
    private readonly Hash<ulong, EntityZones> zonedPlayers;
    private readonly Hash<uint, EntityZones> zonedEntities;
    private readonly Dictionary<ulong, string> lastPlayerZone;
    private ZoneFlags globalFlags;
    private bool zonesInitialized;
    private static ZoneManager Instance { get; set; }
    private const string PERMISSION_ZONE;
    private const string PERMISSION_IGNORE_FLAG;
    private const int PLAYER_MASK;
    private const int TARGET_LAYERS;
    private void Init();
    private void OnServerInitialized();
    private void OnTerrainInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntityKill(BaseEntity baseEntity);
    private void Unload();
    private UpdateBehaviour updateBehaviour;
    private void InitializeUpdateBehaviour();
    private void DestroyUpdateBehaviour();
    private class UpdateBehaviour : MonoBehaviour
    {
        private System.Diagnostics.Stopwatch sw;
        private Queue<BasePlayer> playerUpdateQueue;
        private const float MAX_MS;
        private void OnDestroy();
        internal void QueueUpdate(BasePlayer player);
        private void Update();
    }

    private void OnEntityBuilt(Planner planner, GameObject gObject);
    private object OnStructureUpgrade(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum grade);
    private void OnItemDeployed(Deployer deployer, BaseEntity deployedEntity);
    private void OnItemUse(Item item, int amount);
    private void OnRunPlayerMetabolism(PlayerMetabolism metabolism, BaseCombatEntity ownerEntity, float delta);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private object OnBetterChat(Oxide.Core.Libraries.Covalence.IPlayer iPlayer, string message);
    private object OnPlayerVoice(BasePlayer player, Byte[] data);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
    private void OnEntitySpawned(BaseNetworkable baseNetworkable);
    private void CanSpawn(BaseEntity baseEntity);
    private object CanBeWounded(BasePlayer player, HitInfo hitinfo);
    private object CanUpdateSign(BasePlayer player, Signage sign);
    private object OnOvenToggle(BaseOven oven, BasePlayer player);
    private object CanUseVending(BasePlayer player, VendingMachine machine);
    private object CanHideStash(BasePlayer player, StashContainer stash);
    private object CanCraft(ItemCrafter itemCrafter, ItemBlueprint bp, int amount);
    private void OnDoorOpened(Door door, BasePlayer player);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private void OnLootPlayer(BasePlayer looter, BasePlayer target);
    private object OnLootPlayerInternal(BasePlayer looter, BasePlayer target);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private object CanLootEntity(BasePlayer player, LootableCorpse corpse);
    private void OnLootCorpse(LootableCorpse corpse, BasePlayer player);
    private void OnLootContainer(DroppedItemContainer container, BasePlayer player);
    private object CanLootEntity(BasePlayer player, DroppedItemContainer container);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private object CanLootInternal(BasePlayer player, ZoneFlags flag);
    private void OnLootInternal(BasePlayer player, ZoneFlags flag);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private object CanPickupLock(BasePlayer player, BaseLock baseLock);
    private object OnItemPickup(Item item, BasePlayer player);
    private object CanPickupInternal(BasePlayer player, ZoneFlags flag);
    private object CanLootEntity(ResourceContainer container, BasePlayer player);
    private object OnCollectiblePickup(Item item, BasePlayer player);
    private object OnGrowableGather(GrowableEntity plant, Item item, BasePlayer player);
    private object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private object OnGatherInternal(BasePlayer player);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity entity);
    private object CanBradleyApcTarget(BradleyAPC apc, BaseEntity entity);
    private object CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object CanHelicopterStrafeTarget(PatrolHelicopterAI heli, BasePlayer player);
    private object OnHelicopterTarget(HelicopterTurret turret, BaseCombatEntity entity);
    private object OnNpcTarget(BaseCombatEntity entity, BasePlayer player);
    private object OnTargetPlayerInternal(BasePlayer player, ZoneFlags flag);
    private void OnPlayerSleep(BasePlayer player);
    private void OnPlayerSleepEnd(BasePlayer player);
    private void KillSleepingPlayer(BasePlayer player);
    private void UpdatePlayerZones(BasePlayer player);
    private bool IsInsideBounds(Zone zone, Vector3 worldPos);
    private void InitializeZones();
    private void CreateZone(Zone.Definition definition);
    private bool ReverseVelocity(BasePlayer player);
    private void EjectPlayer(BasePlayer player, Zone zone);
    private void AttractPlayer(BasePlayer player, Zone zone);
    private void ShowZone(BasePlayer player, string zoneId, float time);
    private Vector3 RotatePointAroundPivot(Vector3 point, Vector3 pivot, Quaternion rotation);
    public class Zone : MonoBehaviour
    {
        internal Definition definition;
        internal ZoneFlags disabledFlags;
        internal Zone parent;
        internal List<BasePlayer> players;
        internal List<BaseEntity> entities;
        internal List<ulong> keepInList;
        internal List<ulong> whitelist;
        private Rigidbody rigidbody;
        internal Collider collider;
        internal Bounds colliderBounds;
        private ChildSphereTrigger<TriggerRadiation> radiation;
        private ChildSphereTrigger<TriggerComfort> comfort;
        private ChildSphereTrigger<TriggerTemperature> temperature;
        private TriggerSafeZone safeZone;
        private bool isTogglingLights;
        private void Awake();
        private void OnDestroy();
        private void EmptyZone();
        public void InitializeZone(Definition definition);
        public void FindZoneParent();
        public void Reset();
        private void RegisterPermission();
        private void InitializeCollider();
        private void InitializeRadiation();
        private void InitializeComfort();
        private void InitializeTemperature();
        private void InitializeSafeZone();
        private void AddToTrigger(TriggerBase triggerBase, BasePlayer player);
        private void RemoveFromTrigger(TriggerBase triggerBase, BasePlayer player);
        private void AddAllPlayers();
        private void RemoveAllPlayers();
        private class ChildSphereTrigger
        {
            public GameObject Object { get; set; }
            public SphereCollider Collider { get; set; }
            public T Trigger { get; set; }
            public ChildSphereTrigger(GameObject parent, string name);
            public void Destroy();
        }

        private bool isLightsOn;
        private void InitializeAutoLights();
        private void CheckAlwaysLights();
        private void CheckLights();
        private IEnumerator ToggleLights(bool active);
        private bool ToggleLight(BaseEntity baseEntity, bool active, bool requiresFuel);
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
        public void OnPlayerEnterZone(BasePlayer player);
        public void OnPlayerExitZone(BasePlayer player);
        public void OnEntityEnterZone(BaseEntity baseEntity);
        public void OnEntityExitZone(BaseEntity baseEntity, bool resetDecay, bool isDead);
        public bool HasPermission(BasePlayer player);
        public bool CanLeaveZone(BasePlayer player);
        public bool CanEnterZone(BasePlayer player);
        private bool HasFlag(ZoneFlags flags);
        public class Definition
        {
            public string Name { get; set; }
            public float Radius { get; set; }
            public float Radiation { get; set; }
            public float Comfort { get; set; }
            public float Temperature { get; set; }
            public bool SafeZone { get; set; }
            public Vector3 Location { get; set; }
            public Vector3 Size { get; set; }
            public Vector3 Rotation { get; set; }
            public string Id { get; set; }
            public string ParentID { get; set; }
            public string EnterMessage { get; set; }
            public string LeaveMessage { get; set; }
            public string Permission { get; set; }
            public string EjectSpawns { get; set; }
            public bool Enabled { get; set; }
            public ZoneFlags Flags { get; set; }
            public Definition();
            public Definition(Vector3 position);
        }

    }

    private void OnPlayerEnterZone(BasePlayer player, Zone zone);
    private void OnPlayerExitZone(BasePlayer player, Zone zone);
    private void OnEntityEnterZone(BaseEntity baseEntity, Zone zone);
    private void OnEntityExitZone(BaseEntity baseEntity, Zone zone);
    private bool IsAdmin(BasePlayer player);
    private bool IsNpc(BasePlayer player);
    private bool HasPermission(BasePlayer player, string permname);
    private bool HasPermission(ConsoleSystem.Arg arg, string permname);
    private bool CanBypass(object player, ZoneFlags flag);
    private void SendMessage(BasePlayer player, string message, object[] args);
    private Zone GetZoneByID(string zoneId);
    private void AddToKeepinlist(Zone zone, BasePlayer player);
    private void RemoveFromKeepinlist(Zone zone, BasePlayer player);
    private void AddToWhitelist(Zone zone, BasePlayer player);
    private void RemoveFromWhitelist(Zone zone, BasePlayer player);
    private bool HasPlayerFlag(BasePlayer player, ZoneFlags flag, bool canBypass);
    private bool HasEntityFlag(BaseEntity baseEntity, ZoneFlags flag);
    private void SetZoneStatus(string zoneId, bool active);
    private Vector3 GetZoneLocation(string zoneId);
    private object GetZoneRadius(string zoneID);
    private object GetZoneSize(string zoneID);
    private object GetZoneName(string zoneID);
    private object CheckZoneID(string zoneID);
    private object GetZoneIDs();
    private bool IsPositionInZone(string zoneID, Vector3 position);
    private List<BasePlayer> GetPlayersInZone(string zoneID);
    private List<BaseEntity> GetEntitiesInZone(string zoneId);
    private bool isPlayerInZone(string zoneID, BasePlayer player);
    private bool IsPlayerInZone(string zoneID, BasePlayer player);
    private bool IsEntityInZone(string zoneID, BaseEntity entity);
    private string[] GetPlayerZoneIDs(BasePlayer player);
    private string[] GetEntityZoneIDs(BaseEntity entity);
    private bool HasFlag(string zoneId, string flagStr);
    private void AddFlag(string zoneId, string flagStr);
    private void RemoveFlag(string zoneId, string flagStr);
    private bool HasDisabledFlag(string zoneId, string flagStr);
    private void AddDisabledFlag(string zoneId, string flagStr);
    private void RemoveDisabledFlag(string zoneId, string flagStr);
    private bool CreateOrUpdateZone(string zoneId, string[] args, Vector3 position);
    private bool EraseZone(string zoneId);
    private List<string> ZoneFieldListRaw();
    private Dictionary<string, string> ZoneFieldList(string zoneId);
    private bool AddPlayerToZoneKeepinlist(string zoneId, BasePlayer player);
    private bool RemovePlayerFromZoneKeepinlist(string zoneId, BasePlayer player);
    private bool AddPlayerToZoneWhitelist(string zoneId, BasePlayer player);
    private bool RemovePlayerFromZoneWhitelist(string zoneId, BasePlayer player);
    private bool EntityHasFlag(BaseEntity baseEntity, string flagStr);
    private bool PlayerHasFlag(BasePlayer player, string flagStr);
    private object CanRedeemKit(BasePlayer player);
    private object CanTeleport(BasePlayer player);
    private object canRemove(BasePlayer player);
    private object CanRemove(BasePlayer player);
    private bool CanChat(BasePlayer player);
    private object CanTrade(BasePlayer player);
    private object canShop(BasePlayer player);
    private object CanShop(BasePlayer player);
    private void AddFlag(Zone zone, ZoneFlags flag);
    private void RemoveFlag(Zone zone, ZoneFlags flag);
    private bool HasFlag(Zone zone, ZoneFlags flag);
    private void AddDisabledFlag(Zone zone, ZoneFlags flag);
    private void RemoveDisabledFlag(Zone zone, ZoneFlags flag);
    private bool HasDisabledFlag(Zone zone, ZoneFlags flag);
    private void UpdateZoneFlags(Zone zone);
    private bool HasGlobalFlag(ZoneFlags flags);
    private void UpdateGlobalFlags();
    private bool NeedsUpdateSubscriptions();
    private void UpdateHookSubscriptions();
    private void UnsubscribeAll();
    private void UpdateZoneDefinition(Zone zone, string[] args, BasePlayer player);
    [ChatCommand("zone_add")]
    private void cmdChatZoneAdd(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_wipe")]
    private void cmdChatZoneReset(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_remove")]
    private void cmdChatZoneRemove(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_stats")]
    private void cmdChatZoneStats(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_edit")]
    private void cmdChatZoneEdit(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_list")]
    private void cmdChatZoneList(BasePlayer player, string command, string[] args);
    [ChatCommand("zone")]
    private void cmdChatZone(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_flags")]
    private void cmdChatZoneFlags(BasePlayer player, string command, string[] args);
    [ChatCommand("zone_player")]
    private void cmdChatZonePlayer(BasePlayer player, string command, string[] args);
    [ConsoleCommand("zone")]
    private void ccmdZone(ConsoleSystem.Arg arg);
    const string ZMUI;
    public static class UI
    {
        static public CuiElementContainer Container(string panel, string color, string min, string max, bool useCursor, string parent);
        static public void Panel(CuiElementContainer container, string panel, string color, string min, string max, bool cursor);
        static public void Label(CuiElementContainer container, string panel, string text, int size, string min, string max, TextAnchor align);
        static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string min, string max, string command, TextAnchor align);
        public static string Color(string hexColor, float alpha);
    }

    private void OpenFlagEditor(BasePlayer player, string zoneId);
    private float[] GetButtonPosition(int i);
    private int ColumnNumber(int max, int count);
    [ConsoleCommand("zmui.editflag")]
    private void ccmdEditFlag(ConsoleSystem.Arg arg);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Autolight Options")]
        public AutoLightOptions AutoLights { get; set; }
        [JsonProperty(PropertyName = "Notification Options")]
        public NotificationOptions Notifications { get; set; }
        [JsonProperty(PropertyName = "NPC players can deal player damage in zones with PvpGod flag")]
        public bool NPCHurtPvpGod { get; set; }
        [JsonProperty(PropertyName = "Allow decay damage in zones with Undestr flag")]
        public bool DecayDamageUndestr { get; set; }
        public class AutoLightOptions
        {
            [JsonProperty(PropertyName = "Time to turn lights on")]
            public float OnTime { get; set; }
            [JsonProperty(PropertyName = "Time to turn lights off")]
            public float OffTime { get; set; }
            [JsonProperty(PropertyName = "Lights require fuel to activate automatically")]
            public bool RequiresFuel { get; set; }
        }

        public class NotificationOptions
        {
            [JsonProperty(PropertyName = "Display notifications via PopupNotifications")]
            public bool Popups { get; set; }
            [JsonProperty(PropertyName = "Chat prefix")]
            public string Prefix { get; set; }
            [JsonProperty(PropertyName = "Chat color (hex)")]
            public string Color { get; set; }
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
    private class StoredData
    {
        public HashSet<Zone.Definition> definitions;
    }

    private class EntityZones
    {
        public ZoneFlags Flags { get; set; }
        public HashSet<Zone> Zones { get; set; }
        public EntityZones();
        public void AddFlags(ZoneFlags flags);
        public void RemoveFlags(ZoneFlags flags);
        public bool HasFlag(ZoneFlags flags);
        public void UpdateFlags();
        public bool EnterZone(Zone zone);
        public bool LeaveZone(Zone zone);
        public bool IsInZone(Zone zone);
        public bool IsInZone(string zoneId);
        public bool ShouldRemove();
        public int Count { get; set; }
    }

    private class Vector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private string Message(string key, string playerId);
    private Dictionary<string, string> Messages;
}

private class UpdateBehaviour : MonoBehaviour
{
    private System.Diagnostics.Stopwatch sw;
    private Queue<BasePlayer> playerUpdateQueue;
    private const float MAX_MS;
    private void OnDestroy();
    internal void QueueUpdate(BasePlayer player);
    private void Update();
}

public class Zone : MonoBehaviour
{
    internal Definition definition;
    internal ZoneFlags disabledFlags;
    internal Zone parent;
    internal List<BasePlayer> players;
    internal List<BaseEntity> entities;
    internal List<ulong> keepInList;
    internal List<ulong> whitelist;
    private Rigidbody rigidbody;
    internal Collider collider;
    internal Bounds colliderBounds;
    private ChildSphereTrigger<TriggerRadiation> radiation;
    private ChildSphereTrigger<TriggerComfort> comfort;
    private ChildSphereTrigger<TriggerTemperature> temperature;
    private TriggerSafeZone safeZone;
    private bool isTogglingLights;
    private void Awake();
    private void OnDestroy();
    private void EmptyZone();
    public void InitializeZone(Definition definition);
    public void FindZoneParent();
    public void Reset();
    private void RegisterPermission();
    private void InitializeCollider();
    private void InitializeRadiation();
    private void InitializeComfort();
    private void InitializeTemperature();
    private void InitializeSafeZone();
    private void AddToTrigger(TriggerBase triggerBase, BasePlayer player);
    private void RemoveFromTrigger(TriggerBase triggerBase, BasePlayer player);
    private void AddAllPlayers();
    private void RemoveAllPlayers();
    private class ChildSphereTrigger
    {
        public GameObject Object { get; set; }
        public SphereCollider Collider { get; set; }
        public T Trigger { get; set; }
        public ChildSphereTrigger(GameObject parent, string name);
        public void Destroy();
    }

    private bool isLightsOn;
    private void InitializeAutoLights();
    private void CheckAlwaysLights();
    private void CheckLights();
    private IEnumerator ToggleLights(bool active);
    private bool ToggleLight(BaseEntity baseEntity, bool active, bool requiresFuel);
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
    public void OnPlayerEnterZone(BasePlayer player);
    public void OnPlayerExitZone(BasePlayer player);
    public void OnEntityEnterZone(BaseEntity baseEntity);
    public void OnEntityExitZone(BaseEntity baseEntity, bool resetDecay, bool isDead);
    public bool HasPermission(BasePlayer player);
    public bool CanLeaveZone(BasePlayer player);
    public bool CanEnterZone(BasePlayer player);
    private bool HasFlag(ZoneFlags flags);
    public class Definition
    {
        public string Name { get; set; }
        public float Radius { get; set; }
        public float Radiation { get; set; }
        public float Comfort { get; set; }
        public float Temperature { get; set; }
        public bool SafeZone { get; set; }
        public Vector3 Location { get; set; }
        public Vector3 Size { get; set; }
        public Vector3 Rotation { get; set; }
        public string Id { get; set; }
        public string ParentID { get; set; }
        public string EnterMessage { get; set; }
        public string LeaveMessage { get; set; }
        public string Permission { get; set; }
        public string EjectSpawns { get; set; }
        public bool Enabled { get; set; }
        public ZoneFlags Flags { get; set; }
        public Definition();
        public Definition(Vector3 position);
    }

}

private class ChildSphereTrigger
{
    public GameObject Object { get; set; }
    public SphereCollider Collider { get; set; }
    public T Trigger { get; set; }
    public ChildSphereTrigger(GameObject parent, string name);
    public void Destroy();
}

public class Definition
{
    public string Name { get; set; }
    public float Radius { get; set; }
    public float Radiation { get; set; }
    public float Comfort { get; set; }
    public float Temperature { get; set; }
    public bool SafeZone { get; set; }
    public Vector3 Location { get; set; }
    public Vector3 Size { get; set; }
    public Vector3 Rotation { get; set; }
    public string Id { get; set; }
    public string ParentID { get; set; }
    public string EnterMessage { get; set; }
    public string LeaveMessage { get; set; }
    public string Permission { get; set; }
    public string EjectSpawns { get; set; }
    public bool Enabled { get; set; }
    public ZoneFlags Flags { get; set; }
    public Definition();
    public Definition(Vector3 position);
}

public static class UI
{
    static public CuiElementContainer Container(string panel, string color, string min, string max, bool useCursor, string parent);
    static public void Panel(CuiElementContainer container, string panel, string color, string min, string max, bool cursor);
    static public void Label(CuiElementContainer container, string panel, string text, int size, string min, string max, TextAnchor align);
    static public void Button(CuiElementContainer container, string panel, string color, string text, int size, string min, string max, string command, TextAnchor align);
    public static string Color(string hexColor, float alpha);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Autolight Options")]
    public AutoLightOptions AutoLights { get; set; }
    [JsonProperty(PropertyName = "Notification Options")]
    public NotificationOptions Notifications { get; set; }
    [JsonProperty(PropertyName = "NPC players can deal player damage in zones with PvpGod flag")]
    public bool NPCHurtPvpGod { get; set; }
    [JsonProperty(PropertyName = "Allow decay damage in zones with Undestr flag")]
    public bool DecayDamageUndestr { get; set; }
    public class AutoLightOptions
    {
        [JsonProperty(PropertyName = "Time to turn lights on")]
        public float OnTime { get; set; }
        [JsonProperty(PropertyName = "Time to turn lights off")]
        public float OffTime { get; set; }
        [JsonProperty(PropertyName = "Lights require fuel to activate automatically")]
        public bool RequiresFuel { get; set; }
    }

    public class NotificationOptions
    {
        [JsonProperty(PropertyName = "Display notifications via PopupNotifications")]
        public bool Popups { get; set; }
        [JsonProperty(PropertyName = "Chat prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Chat color (hex)")]
        public string Color { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class AutoLightOptions
{
    [JsonProperty(PropertyName = "Time to turn lights on")]
    public float OnTime { get; set; }
    [JsonProperty(PropertyName = "Time to turn lights off")]
    public float OffTime { get; set; }
    [JsonProperty(PropertyName = "Lights require fuel to activate automatically")]
    public bool RequiresFuel { get; set; }
}

public class NotificationOptions
{
    [JsonProperty(PropertyName = "Display notifications via PopupNotifications")]
    public bool Popups { get; set; }
    [JsonProperty(PropertyName = "Chat prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Chat color (hex)")]
    public string Color { get; set; }
}

private class StoredData
{
    public HashSet<Zone.Definition> definitions;
}

private class EntityZones
{
    public ZoneFlags Flags { get; set; }
    public HashSet<Zone> Zones { get; set; }
    public EntityZones();
    public void AddFlags(ZoneFlags flags);
    public void RemoveFlags(ZoneFlags flags);
    public bool HasFlag(ZoneFlags flags);
    public void UpdateFlags();
    public bool EnterZone(Zone zone);
    public bool LeaveZone(Zone zone);
    public bool IsInZone(Zone zone);
    public bool IsInZone(string zoneId);
    public bool ShouldRemove();
    public int Count { get; set; }
}

private class Vector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

