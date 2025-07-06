# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 7 - Generated: 2025-07-06 20:39:06

## Skinner-2.1.5

```csharp
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.Pool;
using System.Text;
using Facepunch;

Oxide.Plugins
[Info("Skinner", "Whispers88", "2.1.5")]
[Description("Brings automation and ease to skinning items")]
public class Skinner : CovalencePlugin
{
    static Skinner skinner;
    private const string permdefault;
    private const string permitems;
    private const string permcraft;
    private const string permskininv;
    private const string permskinteam;
    private const string permskinteamblock;
    private const string permskincon;
    private const string permbypassauth;
    private const string permimport;
    private const string permskinbase;
    private const string permskinall;
    private const string permskinauto;
    private const string permskinautotoggle;
    private const string permskinrequest;
    private const string permskintry;
    private List<string> permissions;
    private void OnServerInitialized();
    private void Loaded();
    private void Unload();
    public static Dictionary<ulong, string>? _skinNames;
    public static Dictionary<int, List<ulong>>? _cachedSkins;
    private static ulong maskID;
    private ulong SetMask(ulong num);
    public static ulong GetMask(ulong skinID, int itemID, bool redirectSkin);
    public static bool HasMask(ulong uID);
    public static ulong UnsetMask(ulong num);
    private void AddSkin(ulong skinID, int itemID, string displayName, int redirectID);
    private int totskins;
    private void GetSkins();
    public static List<int>? _cachedSkinKeys;
    private void UpdateImportedSkins();
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Skin Commands (skin items in you inventory")]
        public string[] cmdsskin;
        [JsonProperty("Skin Items Commands (skin items you have already placed")]
        public string[] cmdsskinitems;
        [JsonProperty("Set default items to be skinned")]
        public string[] cmdsskincraft;
        [JsonProperty("Automatically set all items in you inventory to your default skins")]
        public string[] cmdsskininv;
        [JsonProperty("Automatically set all items a container to your default skins")]
        public string[] cmdsskincon;
        [JsonProperty("Automatically skin all deployables in your base")]
        public string[] cmdskinbase;
        [JsonProperty("Automatically skin all items in your base")]
        public string[] cmdskinallitems;
        [JsonProperty("Automatically skin all items that are moved into you inventory")]
        public string[] cmdtoggleautoskin;
        [JsonProperty("Skin your teams inventories with your skin set")]
        public string[] cmdskinteam;
        [JsonProperty("Request workshop skins via workshop ID")]
        public string[] cmdskinrequest;
        [JsonProperty("Approve workshop skin requests")]
        public string[] cmdskinrequests;
        [JsonProperty("Set your selected skin set")]
        public string[] cmdskinset;
        [JsonProperty("Import Custom Skins")]
        public string[] cmdskinimport;
        [JsonProperty("Import Workshop Collection Command")]
        public string[] cmdcollectionimport;
        [JsonProperty("Skin Request Notification Discord Webhook")]
        public string DiscordWebhook;
        [JsonProperty("Custom Page Change UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
        public string[] uiposition;
        [JsonProperty("Custom Searchbar UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
        public string[] uisearchposition;
        [JsonProperty("Custom Set Selection UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
        public string[] uisetsposition;
        [JsonProperty("Auto import approved skins")]
        public bool autoImportApproved;
        [JsonProperty("Remove player data after inactivity (days)")]
        public int removedataTime;
        [JsonProperty("Apply names of skins to skinned items")]
        public bool applySkinNames;
        [JsonProperty("Add Search Bar UI")]
        public bool searchbar;
        [JsonProperty("Use on itemcraft hook (skin items after crafting - not required when using skinauto)")]
        public bool useOnItemCraft;
        [JsonProperty("Override spraycan behaviour")]
        public bool sprayCanOveride;
        [JsonProperty("Use spraycan effect when holding spraycan and skinning deployables")]
        public bool sprayCanEffect;
        [JsonProperty("Blacklisted Skins (skinID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> blacklistedskins;
        [JsonProperty("Blacklisted Items (itemID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<int> blacklisteditems;
        [JsonProperty("Import Skin collections (steam workshop ID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> skinCollectionIDs;
        [JsonProperty("Command based cooldowns ('permission' : 'command' seconds")]
        public Dictionary<string, CoolDowns> Cooldowns;
        [JsonProperty("Imported Skins List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<ulong, ImportedItem> ImportedSkinList;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private DynamicConfigFile _defaultSkins;
    private DynamicConfigFile _playerData;
    private DynamicConfigFile _skinRequestsData;
    private Dictionary<ulong, CoolDowns> _playercooldowns;
    private Dictionary<ulong, int> _playerSelectedSet;
    private void LoadData();
    private void SaveData();
    private Dictionary<ulong, Dictionary<int, ulong>[]> _playerDefaultSkins;
    private Dictionary<ulong, PlayerData> _playerUsageData;
    private List<RequestItem> _requestsData;
    public class PlayerDataDec
    {
        public Dictionary<int, List<decimal>> skinusage;
        public long lastonline { get; set; }
    }

    public class PlayerData
    {
        public Dictionary<int, List<ulong>> skinusage;
        public long lastonline { get; set; }
        public void AddSkinUsage(ulong skinID, int itemID, int rItemID);
        public List<ulong>? GetSkinUsage(int itemID);
        public void UpdateLastOnline();
    }

    public class CoolDowns
    {
        public float skin;
        public float skinitem;
        public float skincraft;
        public float skincon;
        public float skininv;
        public float skinteam;
        public float skinbase;
        public float skinall;
    }

    protected override void LoadDefaultMessages();
    private void SkinAutoCMD(IPlayer iplayer, string command, string[] args);
    private void SkinImportCMD(IPlayer iplayer, string command, string[] args);
    private void SkinImportCollection(IPlayer iplayer, string command, string[] args);
    private void SkinCMD(IPlayer iplayer, string command, string[] args);
    private void DefaultSkinsCMD(IPlayer iplayer, string command, string[] args);
    private void SkinRequestCMD(IPlayer iplayer, string command, string[] args);
    private void SkinRequestsCMD(IPlayer iplayer, string command, string[] args);
    private static WaitForSeconds _WaitForSecondsMore;
    private IEnumerator CheckforRequests(BasePlayer player, ItemContainer itemContainer, BoxController boxController);
    private static int Layermask;
    private void SkinItemCMD(IPlayer iplayer, string command, string[] args);
    private void SkinInvCMD(IPlayer iplayer, string command, string[] args);
    private void SkinTeamCMD(IPlayer iplayer, string command, string[] args);
    private void SkinConCMD(IPlayer iplayer, string command, string[] args);
    private void SkinBaseCMD(IPlayer iplayer, string command, string[] args);
    private Dictionary<int, ulong> GetCachedSkins(BasePlayer player, int set);
    private void SkinAllItemsCMD(IPlayer iplayer, string command, string[] args);
    public Dictionary<BasePlayer, BoxController> _boxcontrollers;
    private void SBNextPageCMD(IPlayer iplayer, string command, string[] args);
    private void SBBackPageCMD(IPlayer iplayer, string command, string[] args);
    private void SearchCMD(IPlayer iplayer, string command, string[] args);
    private void SetSelectCMD(IPlayer iplayer, string command, string[] args);
    private void RequestSelectCMD(IPlayer iplayer, string command, string[] args);
    private Dictionary<ulong, BoxController> _viewingcon;
    private void OnPlayerLootEnd(PlayerLoot instance);
     void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string strReason);
    private void OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter itemCrafter);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private static string _coffinPrefab;
    private BaseEntity _limitedEntity;
    private ItemContainer CreateContainer();
    private void StartLooting(BasePlayer player, ItemContainer itemContainer);
    private static readonly uint RPCOpenLootstr;
    private static readonly byte[] RPCOpenLootbytes;
    public static void RPCStartLooting(BasePlayer player);
    public void SkinItem(Item item, ulong uID);
    public BaseCombatEntity? SkinDeployable(BaseCombatEntity baseCombatEntity, ulong uID);
    private bool GetEntityPrefabPath(ItemDefinition def, string resourcePath);
     void SaveEntityStorage(BaseEntity baseEntity, Dictionary<ContainerSet, List<Item>> dictionary, int index);
     void RestoreEntityStorage(BaseEntity baseEntity, int index, Dictionary<ContainerSet, List<Item>> copy);
    private class InventoryWatcher : FacepunchBehaviour
    {
        private BasePlayer player;
         bool _enabled;
         Dictionary<int, ulong> cachedSkins;
        private void Awake();
        private void subContainerWatch();
        private void skinWatch(Item item, bool f);
        public void refreshSkins();
        private void OnDestroy();
    }

    private static int spraycanid;
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private class SpraycanController : FacepunchBehaviour
    {
        private BasePlayer player;
        private SprayCan sprayCan;
        private string skinitem;
         BUTTON _fire2;
         BaseEntity.Flags fbusy;
         float lasttime;
        private void Awake();
        private void FixedUpdate();
    }

    public Queue<Item> itemPool;
    public class BoxController : FacepunchBehaviour
    {
        public ItemContainer inventory;
        private BasePlayer player;
        private Item? mainitem;
        private Item? returnitem;
        private Item? returnitemplayer;
        private Vector3 ogpos;
        public bool _fillingbox;
        public bool _clearingbox;
        private ItemDefinition? itemselected;
        public BaseCombatEntity? maindeployable;
        public string searchtxt;
        public int setSelect;
        public string requestselected;
        public Dictionary<int, ulong> setSkins;
         bool rebuildsearchUI;
         bool rebuildPageUI;
         bool fsearchUI;
         bool fpageUI;
         bool fskinSetsUI;
         bool fskinRequestsUI;
        public string boxtype;
        private int page;
        private int scpage;
        public void StartAwake();
        private void Preremove(Item item);
        public void SkinDeplyoables(BaseCombatEntity entity, ItemDefinition itemDefinition);
        private void CheckforItemDply(Item item, bool b);
        private void GetDeployableSkins(bool skipchecks);
        private void FillSkins(ItemDefinition itemdef, bool bbreak);
        private int olditempos;
        private ItemContainer oldcon;
        private Item? posWatchItem;
        private Item? backpack;
        public void StartItemSkin();
        private Item SplitItem(Item item2, int split_Amount);
        private void OnItemAddedToStack(Item item, int amount);
        private void PosWatch(Item item);
        private void CheckforItem(Item item, bool b);
        private List<ulong>? cachedskins;
        private List<ulong>? usageskins;
        private void GetSkins(bool skipchecks);
        private void ResetCon();
        private bool searchtextwas;
        private bool PrepareSkins(ItemDefinition itemdef);
        public void QuickRemove(Item item);
        private void ItemRemoveCheck(Item item);
        public void UpdateActiveItem(BasePlayer player, Item item);
        public void SkinCraft();
        private void GetDefaultSkins();
        private void CheckforItemSelect(Item item, bool b);
        private void CheckforSkinSelect(Item item, bool b);
        public void SkinRequests();
        private void GetRequestSkins();
        private void CheckforRequestSelect(Item item, bool b);
        public void NextPage();
        public void BackPage();
        public void SetUpdate();
        public void SearchUpdate();
         bool IsMainItemBlacklisted(Item mainitem);
         bool IsMainDeployableBlacklisted(BaseCombatEntity maindeployable);
        private Item GiveItem(Item item, bool drop);
        private void Remove(Item item, bool nextTick);
        private void DoRemove(Item item);
        private void ClearCon(bool nexttick);
        private Item GetItemFromPool(ulong cachedSkin, ItemDefinition itemDef, int pos);
        private void InsertItem(ulong cachedSkin, ItemDefinition? itemDef, int amount, int pos);
        private List<Item>? GetPlayerItems(int itemid);
        public void OnDestroy();
    }

    public string searchUIstring;
    public string pageUIstring;
    public string setsUIstring;
    public string requestsUIstring;
    public class UI
    {
        public static readonly uint AddUIstr;
        public static bool AddUI(BasePlayer player, byte[] elements);
        public static readonly uint DestroyUIstr;
        public static bool DestroyUI(BasePlayer player, byte[] bytes);
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, string oMin, string oMax, bool useCursor, string parent);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string sprite, string material, string anchorMin, string anchorMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string text, int size, string anchorMin, string anchorMax, string colour, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string bgcolor, string textcolour, string text, int size, string anchorMin, string anchorMax, string command, TextAnchor align);
        static public void AddInputField(CuiElementContainer container, string panel, string bgcolor, string txtcolor, string text, int size, string command, string anchorMin, string anchorMax);
    }

    public const string SkinPageUI;
    public const string SkinSearchUI;
    public const string SkinSetsSelectUI;
    public const string SkinRequestsUI;
    public static byte[] BSkinPageUI;
    public static byte[] BSkinSearchUI;
    public static byte[] BSkinSetsSelectUI;
    public static byte[] BSkinRequestsUI;
    private void InitUI();
    private static Dictionary<ValueTuple<int, int>, byte[]> cacheduiPageDictionary;
    public byte[] AddPageUI(int minpage, int maxpage);
    private byte[] cachedsearchUIDefault;
    public byte[] AddSearchUI(string searchtxt);
    private static Dictionary<int, byte[]> cacheduiSetsDictionary;
    public byte[] AddSetsUI(int selectedset);
    private static Dictionary<string, byte[]> cacheduiRequestsDictionary;
    public byte[] AddRequestsUI(string requesttype);
    private string InitPageUI();
    public string InitSearchUI();
    public static string EscapeBrackets(string input);
    public string InitSetsUI();
    public string InitRequestsUI();
    private Message.Type RPCMessage;
    private uint refreshSkin;
    private void SendNetworkUpdate(BaseEntity ent, SendInfo sendInfo);
    private void DoRemove(Item item);
    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
    private IEnumerator getCollectionscouroutine;
    private List<string> _WorkshopSkinIDCollectionList;
    private IEnumerator GetCollectionSkinIDS();
    private IEnumerator getSteamWorkshopSkinData;
    private IEnumerator GetSteamWorkshopSkinData();
    private ValueTuple<string, string>? htmllines2shortname(string[] htmlslines, string workshopid);
    private IEnumerator getSteamWorkshopRequestData;
    private IEnumerator GetSteamWorkshopSkinRequests();
    private List<DiscordData> _discordData;
    private IEnumerator notifyDiscordCoroutine;
    private IEnumerator NotifyDiscord();
    public class Root
    {
        public string content { get; set; }
        public bool tts { get; set; }
        public List<Embed> embeds { get; set; }
        public string avatar_url { get; set; }
        public string username { get; set; }
    }

    public class Embed
    {
        public int id { get; set; }
        public List<Field> fields { get; set; }
        public Author author { get; set; }
        public string title { get; set; }
        public Image image { get; set; }
        public DateTime timestamp { get; set; }
    }

    public class Field
    {
        public int id { get; set; }
        public string name { get; set; }
        public string value { get; set; }
    }

    public class Author
    {
        public string name { get; set; }
        public string icon_url { get; set; }
    }

    public class Image
    {
        public string url { get; set; }
    }

    private Dictionary<string, string> WorkshopSkinNameConversion;
    public Dictionary<int, List<ulong>> GetAllCachedSkins();
    public bool IsRedirectID(ulong uID);
    public int RedirectIDtoItemID(ulong uID);
    public List<ulong>? GetSkinsItemList(int itemid);
}

public class Configuration
{
    [JsonProperty("Skin Commands (skin items in you inventory")]
    public string[] cmdsskin;
    [JsonProperty("Skin Items Commands (skin items you have already placed")]
    public string[] cmdsskinitems;
    [JsonProperty("Set default items to be skinned")]
    public string[] cmdsskincraft;
    [JsonProperty("Automatically set all items in you inventory to your default skins")]
    public string[] cmdsskininv;
    [JsonProperty("Automatically set all items a container to your default skins")]
    public string[] cmdsskincon;
    [JsonProperty("Automatically skin all deployables in your base")]
    public string[] cmdskinbase;
    [JsonProperty("Automatically skin all items in your base")]
    public string[] cmdskinallitems;
    [JsonProperty("Automatically skin all items that are moved into you inventory")]
    public string[] cmdtoggleautoskin;
    [JsonProperty("Skin your teams inventories with your skin set")]
    public string[] cmdskinteam;
    [JsonProperty("Request workshop skins via workshop ID")]
    public string[] cmdskinrequest;
    [JsonProperty("Approve workshop skin requests")]
    public string[] cmdskinrequests;
    [JsonProperty("Set your selected skin set")]
    public string[] cmdskinset;
    [JsonProperty("Import Custom Skins")]
    public string[] cmdskinimport;
    [JsonProperty("Import Workshop Collection Command")]
    public string[] cmdcollectionimport;
    [JsonProperty("Skin Request Notification Discord Webhook")]
    public string DiscordWebhook;
    [JsonProperty("Custom Page Change UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
    public string[] uiposition;
    [JsonProperty("Custom Searchbar UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
    public string[] uisearchposition;
    [JsonProperty("Custom Set Selection UI Positon anchor/offset 'min x, min y', 'max x', max y'")]
    public string[] uisetsposition;
    [JsonProperty("Auto import approved skins")]
    public bool autoImportApproved;
    [JsonProperty("Remove player data after inactivity (days)")]
    public int removedataTime;
    [JsonProperty("Apply names of skins to skinned items")]
    public bool applySkinNames;
    [JsonProperty("Add Search Bar UI")]
    public bool searchbar;
    [JsonProperty("Use on itemcraft hook (skin items after crafting - not required when using skinauto)")]
    public bool useOnItemCraft;
    [JsonProperty("Override spraycan behaviour")]
    public bool sprayCanOveride;
    [JsonProperty("Use spraycan effect when holding spraycan and skinning deployables")]
    public bool sprayCanEffect;
    [JsonProperty("Blacklisted Skins (skinID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> blacklistedskins;
    [JsonProperty("Blacklisted Items (itemID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<int> blacklisteditems;
    [JsonProperty("Import Skin collections (steam workshop ID)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> skinCollectionIDs;
    [JsonProperty("Command based cooldowns ('permission' : 'command' seconds")]
    public Dictionary<string, CoolDowns> Cooldowns;
    [JsonProperty("Imported Skins List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<ulong, ImportedItem> ImportedSkinList;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class PlayerDataDec
{
    public Dictionary<int, List<decimal>> skinusage;
    public long lastonline { get; set; }
}

public class PlayerData
{
    public Dictionary<int, List<ulong>> skinusage;
    public long lastonline { get; set; }
    public void AddSkinUsage(ulong skinID, int itemID, int rItemID);
    public List<ulong>? GetSkinUsage(int itemID);
    public void UpdateLastOnline();
}

public class CoolDowns
{
    public float skin;
    public float skinitem;
    public float skincraft;
    public float skincon;
    public float skininv;
    public float skinteam;
    public float skinbase;
    public float skinall;
}

private class InventoryWatcher : FacepunchBehaviour
{
    private BasePlayer player;
     bool _enabled;
     Dictionary<int, ulong> cachedSkins;
    private void Awake();
    private void subContainerWatch();
    private void skinWatch(Item item, bool f);
    public void refreshSkins();
    private void OnDestroy();
}

private class SpraycanController : FacepunchBehaviour
{
    private BasePlayer player;
    private SprayCan sprayCan;
    private string skinitem;
     BUTTON _fire2;
     BaseEntity.Flags fbusy;
     float lasttime;
    private void Awake();
    private void FixedUpdate();
}

public class BoxController : FacepunchBehaviour
{
    public ItemContainer inventory;
    private BasePlayer player;
    private Item? mainitem;
    private Item? returnitem;
    private Item? returnitemplayer;
    private Vector3 ogpos;
    public bool _fillingbox;
    public bool _clearingbox;
    private ItemDefinition? itemselected;
    public BaseCombatEntity? maindeployable;
    public string searchtxt;
    public int setSelect;
    public string requestselected;
    public Dictionary<int, ulong> setSkins;
     bool rebuildsearchUI;
     bool rebuildPageUI;
     bool fsearchUI;
     bool fpageUI;
     bool fskinSetsUI;
     bool fskinRequestsUI;
    public string boxtype;
    private int page;
    private int scpage;
    public void StartAwake();
    private void Preremove(Item item);
    public void SkinDeplyoables(BaseCombatEntity entity, ItemDefinition itemDefinition);
    private void CheckforItemDply(Item item, bool b);
    private void GetDeployableSkins(bool skipchecks);
    private void FillSkins(ItemDefinition itemdef, bool bbreak);
    private int olditempos;
    private ItemContainer oldcon;
    private Item? posWatchItem;
    private Item? backpack;
    public void StartItemSkin();
    private Item SplitItem(Item item2, int split_Amount);
    private void OnItemAddedToStack(Item item, int amount);
    private void PosWatch(Item item);
    private void CheckforItem(Item item, bool b);
    private List<ulong>? cachedskins;
    private List<ulong>? usageskins;
    private void GetSkins(bool skipchecks);
    private void ResetCon();
    private bool searchtextwas;
    private bool PrepareSkins(ItemDefinition itemdef);
    public void QuickRemove(Item item);
    private void ItemRemoveCheck(Item item);
    public void UpdateActiveItem(BasePlayer player, Item item);
    public void SkinCraft();
    private void GetDefaultSkins();
    private void CheckforItemSelect(Item item, bool b);
    private void CheckforSkinSelect(Item item, bool b);
    public void SkinRequests();
    private void GetRequestSkins();
    private void CheckforRequestSelect(Item item, bool b);
    public void NextPage();
    public void BackPage();
    public void SetUpdate();
    public void SearchUpdate();
     bool IsMainItemBlacklisted(Item mainitem);
     bool IsMainDeployableBlacklisted(BaseCombatEntity maindeployable);
    private Item GiveItem(Item item, bool drop);
    private void Remove(Item item, bool nextTick);
    private void DoRemove(Item item);
    private void ClearCon(bool nexttick);
    private Item GetItemFromPool(ulong cachedSkin, ItemDefinition itemDef, int pos);
    private void InsertItem(ulong cachedSkin, ItemDefinition? itemDef, int amount, int pos);
    private List<Item>? GetPlayerItems(int itemid);
    public void OnDestroy();
}

public class UI
{
    public static readonly uint AddUIstr;
    public static bool AddUI(BasePlayer player, byte[] elements);
    public static readonly uint DestroyUIstr;
    public static bool DestroyUI(BasePlayer player, byte[] bytes);
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, string oMin, string oMax, bool useCursor, string parent);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string sprite, string material, string anchorMin, string anchorMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string text, int size, string anchorMin, string anchorMax, string colour, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string bgcolor, string textcolour, string text, int size, string anchorMin, string anchorMax, string command, TextAnchor align);
    static public void AddInputField(CuiElementContainer container, string panel, string bgcolor, string txtcolor, string text, int size, string command, string anchorMin, string anchorMax);
}

public class Root
{
    public string content { get; set; }
    public bool tts { get; set; }
    public List<Embed> embeds { get; set; }
    public string avatar_url { get; set; }
    public string username { get; set; }
}

public class Embed
{
    public int id { get; set; }
    public List<Field> fields { get; set; }
    public Author author { get; set; }
    public string title { get; set; }
    public Image image { get; set; }
    public DateTime timestamp { get; set; }
}

public class Field
{
    public int id { get; set; }
    public string name { get; set; }
    public string value { get; set; }
}

public class Author
{
    public string name { get; set; }
    public string icon_url { get; set; }
}

public class Image
{
    public string url { get; set; }
}


```

---

## SkinsOptimization

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("SkinsOptimization", "bazuka5801", "1.0.0")]
 class SkinsOptimization : RustPlugin
{
    private List<ulong> PlayersActivated;
     void OnServerInitialized();
     void Unload();
     void OnPlayerConnected(Network.Message packet);
     void OnPlayerInit(BasePlayer player);
    [ChatCommand("skinon")]
     void cmdChatSkinOn(BasePlayer player, string command, string[] args);
    [ChatCommand("skinoff")]
     void cmdChatSkinOff(BasePlayer player, string command, string[] args);
     void ItemSkinsCommand(BasePlayer player, bool value);
    private readonly DynamicConfigFile PlayersActivatedFile;
     void OnServerSave();
     void LoadData();
     void SaveData();
     Dictionary<string, string> Messages;
}


```

---

## SkipNight

```csharp
using Oxide.Core;
using UnityEngine;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("SkipNight", "walkinrey", "1.0.4")]
 class SkipNight : RustPlugin
{
     class Configuration
    {
        [JsonProperty(PropertyName = "Сколько секунд будет длиться голосование (по умолчанию 60)")]
        public float VoteTime;
        [JsonProperty(PropertyName = "Сообщение при пропуске ночи")]
        public string SkipNightString;
        [JsonProperty(PropertyName = "Сообщение против пропуска ночи")]
        public string DisskipNightString;
        [JsonProperty(PropertyName = "Какое время устанавливать при пропуске ночи (по умолчанию 12)")]
        public float TimeSet;
        [JsonProperty(PropertyName = "Раз в сколько секунд проверять текущее время на сервере? (влияет на производительность сервера)")]
        public float CheckTime;
        [JsonProperty(PropertyName = "Во сколько начинать голосование по игровому времени? (по умолчанию 18)")]
        public float VoteGameTime;
        [JsonProperty(PropertyName = "Какие дни будут пропускаться")]
        public int[] daysDisable;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private Configuration config;
    private int AgreeVoted;
    private int DisagreeVoted;
    private bool VoteActive;
    private bool DisagreeActive;
    private List<BasePlayer> PlayersVoted;
    [PluginReference]
    private Plugin ImageLibrary;
    private void OnServerInitialized(bool initial);
    [ChatCommand("disagreevote")]
    private void DisagreeVote(BasePlayer player);
    [ChatCommand("agreevote")]
    private void AgreeVote(BasePlayer player);
    private void VoteTimer();
    private void SetDay();
    private float GetCurrentTime();
    private bool IsDayCheck();
    private void StartTimer();
    private void Loaded();
    private void Unload();
    private void DestroyGUI(BasePlayer player);
    private void DestroyGUIAll();
    private void CreateVoteGUI();
    private CuiElementContainer CreateObjects();
}

 class Configuration
{
    [JsonProperty(PropertyName = "Сколько секунд будет длиться голосование (по умолчанию 60)")]
    public float VoteTime;
    [JsonProperty(PropertyName = "Сообщение при пропуске ночи")]
    public string SkipNightString;
    [JsonProperty(PropertyName = "Сообщение против пропуска ночи")]
    public string DisskipNightString;
    [JsonProperty(PropertyName = "Какое время устанавливать при пропуске ночи (по умолчанию 12)")]
    public float TimeSet;
    [JsonProperty(PropertyName = "Раз в сколько секунд проверять текущее время на сервере? (влияет на производительность сервера)")]
    public float CheckTime;
    [JsonProperty(PropertyName = "Во сколько начинать голосование по игровому времени? (по умолчанию 18)")]
    public float VoteGameTime;
    [JsonProperty(PropertyName = "Какие дни будут пропускаться")]
    public int[] daysDisable;
}


```

---

## SkipNightVote

```csharp
using System.Collections.Generic;
using System;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SkipNightVote", "Mordenak", "1.1.0", ResourceId = 1014)]
 class SkipNightVote : RustPlugin
{
    public class TimePoll : RustPlugin
    {
         Dictionary<string, bool> votesReceived;
         float votesRequired;
        public TimePoll(float votes);
         bool checkVote(string playerId);
        public bool voteDay(BasePlayer player);
        public int tallyVotes();
        public bool wasVoteSuccessful();
    }

    public float requiredVotesPercentage;
    public float pollRetryTime;
    public float pollTimer;
    public int sunsetHour;
    public int sunriseHour;
    public bool displayVoteProgress;
     bool readyToCheck;
    public TimePoll votePoll;
     float lastPoll;
     string noPollOpen;
     string alreadyVoted;
     string voteProgress;
     string voteOpenTime;
     string voteSuccessful;
     string voteFailed;
     string voteReOpenTime;
     string voteNow;
    [ChatCommand("voteday")]
     void cmdVoteTime(BasePlayer player, string command, string[] args);
     void openVote();
     void closeVote();
     void checkVotes();
    [HookMethod("OnTick")]
    private void OnTick();
    private void MessageAllPlayers(string message);
     void PopulateConfig();
     void Loaded();
}

public class TimePoll : RustPlugin
{
     Dictionary<string, bool> votesReceived;
     float votesRequired;
    public TimePoll(float votes);
     bool checkVote(string playerId);
    public bool voteDay(BasePlayer player);
    public int tallyVotes();
    public bool wasVoteSuccessful();
}


```

---

## SkyCraftSystem

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SkyCraftSystem", "fhff444", "1.0.{DarkPluginsID}")]
 class SkyCraftSystem : RustPlugin
{
     Plugin ImageLibrary { get; set; }
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Настройка системы крафта | SettingsPluign")]
        public List<CraftItemsConfiguration> SettingsPlugins;
        [JsonProperty("Настройка интерфейса | Interface Settings")]
        public CraftInterface SettingsInterface;
        internal class CraftItemsConfiguration
        {
            [JsonProperty("Включить крафт этого предмета | On-Off craft this item")]
            public bool OnOffCraft;
            [JsonProperty("Права для крафта | Permissions for craft")]
            public string Permissions;
            [JsonProperty("Ссылка на префаб | Prefab [ЕСЛИ ВЫ НЕ ЗНАЕТЕ ЧТО ЭТО - НЕ ТРОГАЙТЕ.ПРОСТО ИСПОЛЬЗУЙТЕ BOOL(TRUE / FALSE)]")]
            public string Prefab;
            [JsonProperty("Shortname заменяемого предмета | Shortname reply items for prefabs")]
            public string ShortnameReplyPrefab;
            [JsonProperty("Название предмета | DisplayName items")]
            public string DisplayName;
            [JsonProperty("Уровень верстака для крафта | WorkBench Level for Craft")]
            public int WorkBenchLevel;
            [JsonProperty("SkinID предмету(С фоткой)")]
            public ulong SkinID;
            [JsonProperty("Ссылка на картинку(512x512) | LINKPNG.png(512x512)")]
            public string PNGLink;
            [JsonProperty("Предметы для крафта | Crafting Items")]
            public Dictionary<string, int> CraftingItems;
        }

        internal class CraftInterface
        {
            [JsonProperty("[Главная/Main]Цвет заднего фона | Background color")]
            public string BackgrounColorMain;
            [JsonProperty("[Главная/Main]Материал заднего фона | Metrial Background")]
            public string BackgrounMaterialMain;
            [JsonProperty("[Главная/Main]Спрайт заднего фона | Sprite Background")]
            public string BackgrounSpriteMain;
            [JsonProperty("[Панель предлметов/Panel Items]Цвет заднего фона | Background color")]
            public string PItemsBackgroundColor;
            [JsonProperty("[Доп.Панель предлметов/Additional Panel Items]Цвет заднего фона | Background color")]
            public string AdditionalPItemsBackgroundColor;
            [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 0 уровня | WorkBench level - 0")]
            public string WorkBenchLevelColorNull;
            [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 1 уровня | WorkBench level - 1")]
            public string WorkBenchLevelColorOne;
            [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 2 уровня | WorkBench level - 2")]
            public string WorkBenchLevelColorTwo;
            [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 3 уровня | WorkBench level - 3")]
            public string WorkBenchLevelColorThree;
            [JsonProperty("Цвет кнопки | Color Button")]
            public string ColorButtonCreate;
            [JsonProperty("Цвет панели с ресурсом,если конкретный ресурс собран | The color of the resource panel, if a specific resource is compiled")]
            public string ColorPanelResourceComplete;
            [JsonProperty("Цвет панели с ресурсом,если конкретный ресурс не собран | Color of the panel with the resource, if a specific resource is not assembled")]
            public string ColorPanelResourceNoComplete;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private Item OnItemSplit(Item item, int amount);
     object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
     object CanStackItem(Item item, Item targetItem);
    [ConsoleCommand("craftssystem")]
     void ConsoleCraftSystem(ConsoleSystem.Arg arg);
    [ChatCommand("craft")]
     void OpenCraftMenuChat(BasePlayer player);
    [ConsoleCommand("craft")]
     void OpenCraftMenuConsole(ConsoleSystem.Arg arg);
     void RegisteredPlugin();
    private void TakeItemForCraft(BasePlayer player, int index);
    private void GiveItems(BasePlayer player, int index);
    private Item CreateItem(int index);
    private void CheckDeploy(BaseEntity entity);
    private bool ItemCheck(ulong skin);
    private void SpawnItem(Vector3 position, ulong SkinID, Quaternion rotation, ulong ownerID);
    public bool PermissionCheck(string ID, string permissions);
    private bool CheckResourceCraft(BasePlayer player, string Key, int Value);
    private new void LoadDefaultMessages();
    public static string PARENT_MAINMENU;
    public static string PARENT_MENUACTION;
    public void UI_MainMenu(BasePlayer player);
    public void UI_CraftMenuAction(BasePlayer player, int index);
    private static string HexToRustFormat(string hex);
     void MessageUI(BasePlayer player, string Messages);
}

private class Configuration
{
    [JsonProperty("Настройка системы крафта | SettingsPluign")]
    public List<CraftItemsConfiguration> SettingsPlugins;
    [JsonProperty("Настройка интерфейса | Interface Settings")]
    public CraftInterface SettingsInterface;
    internal class CraftItemsConfiguration
    {
        [JsonProperty("Включить крафт этого предмета | On-Off craft this item")]
        public bool OnOffCraft;
        [JsonProperty("Права для крафта | Permissions for craft")]
        public string Permissions;
        [JsonProperty("Ссылка на префаб | Prefab [ЕСЛИ ВЫ НЕ ЗНАЕТЕ ЧТО ЭТО - НЕ ТРОГАЙТЕ.ПРОСТО ИСПОЛЬЗУЙТЕ BOOL(TRUE / FALSE)]")]
        public string Prefab;
        [JsonProperty("Shortname заменяемого предмета | Shortname reply items for prefabs")]
        public string ShortnameReplyPrefab;
        [JsonProperty("Название предмета | DisplayName items")]
        public string DisplayName;
        [JsonProperty("Уровень верстака для крафта | WorkBench Level for Craft")]
        public int WorkBenchLevel;
        [JsonProperty("SkinID предмету(С фоткой)")]
        public ulong SkinID;
        [JsonProperty("Ссылка на картинку(512x512) | LINKPNG.png(512x512)")]
        public string PNGLink;
        [JsonProperty("Предметы для крафта | Crafting Items")]
        public Dictionary<string, int> CraftingItems;
    }

    internal class CraftInterface
    {
        [JsonProperty("[Главная/Main]Цвет заднего фона | Background color")]
        public string BackgrounColorMain;
        [JsonProperty("[Главная/Main]Материал заднего фона | Metrial Background")]
        public string BackgrounMaterialMain;
        [JsonProperty("[Главная/Main]Спрайт заднего фона | Sprite Background")]
        public string BackgrounSpriteMain;
        [JsonProperty("[Панель предлметов/Panel Items]Цвет заднего фона | Background color")]
        public string PItemsBackgroundColor;
        [JsonProperty("[Доп.Панель предлметов/Additional Panel Items]Цвет заднего фона | Background color")]
        public string AdditionalPItemsBackgroundColor;
        [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 0 уровня | WorkBench level - 0")]
        public string WorkBenchLevelColorNull;
        [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 1 уровня | WorkBench level - 1")]
        public string WorkBenchLevelColorOne;
        [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 2 уровня | WorkBench level - 2")]
        public string WorkBenchLevelColorTwo;
        [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 3 уровня | WorkBench level - 3")]
        public string WorkBenchLevelColorThree;
        [JsonProperty("Цвет кнопки | Color Button")]
        public string ColorButtonCreate;
        [JsonProperty("Цвет панели с ресурсом,если конкретный ресурс собран | The color of the resource panel, if a specific resource is compiled")]
        public string ColorPanelResourceComplete;
        [JsonProperty("Цвет панели с ресурсом,если конкретный ресурс не собран | Color of the panel with the resource, if a specific resource is not assembled")]
        public string ColorPanelResourceNoComplete;
    }

    public static Configuration GetNewConfiguration();
}

internal class CraftItemsConfiguration
{
    [JsonProperty("Включить крафт этого предмета | On-Off craft this item")]
    public bool OnOffCraft;
    [JsonProperty("Права для крафта | Permissions for craft")]
    public string Permissions;
    [JsonProperty("Ссылка на префаб | Prefab [ЕСЛИ ВЫ НЕ ЗНАЕТЕ ЧТО ЭТО - НЕ ТРОГАЙТЕ.ПРОСТО ИСПОЛЬЗУЙТЕ BOOL(TRUE / FALSE)]")]
    public string Prefab;
    [JsonProperty("Shortname заменяемого предмета | Shortname reply items for prefabs")]
    public string ShortnameReplyPrefab;
    [JsonProperty("Название предмета | DisplayName items")]
    public string DisplayName;
    [JsonProperty("Уровень верстака для крафта | WorkBench Level for Craft")]
    public int WorkBenchLevel;
    [JsonProperty("SkinID предмету(С фоткой)")]
    public ulong SkinID;
    [JsonProperty("Ссылка на картинку(512x512) | LINKPNG.png(512x512)")]
    public string PNGLink;
    [JsonProperty("Предметы для крафта | Crafting Items")]
    public Dictionary<string, int> CraftingItems;
}

internal class CraftInterface
{
    [JsonProperty("[Главная/Main]Цвет заднего фона | Background color")]
    public string BackgrounColorMain;
    [JsonProperty("[Главная/Main]Материал заднего фона | Metrial Background")]
    public string BackgrounMaterialMain;
    [JsonProperty("[Главная/Main]Спрайт заднего фона | Sprite Background")]
    public string BackgrounSpriteMain;
    [JsonProperty("[Панель предлметов/Panel Items]Цвет заднего фона | Background color")]
    public string PItemsBackgroundColor;
    [JsonProperty("[Доп.Панель предлметов/Additional Panel Items]Цвет заднего фона | Background color")]
    public string AdditionalPItemsBackgroundColor;
    [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 0 уровня | WorkBench level - 0")]
    public string WorkBenchLevelColorNull;
    [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 1 уровня | WorkBench level - 1")]
    public string WorkBenchLevelColorOne;
    [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 2 уровня | WorkBench level - 2")]
    public string WorkBenchLevelColorTwo;
    [JsonProperty("[Цвет верстака/WorkBenchColor]Цвет верстака 3 уровня | WorkBench level - 3")]
    public string WorkBenchLevelColorThree;
    [JsonProperty("Цвет кнопки | Color Button")]
    public string ColorButtonCreate;
    [JsonProperty("Цвет панели с ресурсом,если конкретный ресурс собран | The color of the resource panel, if a specific resource is compiled")]
    public string ColorPanelResourceComplete;
    [JsonProperty("Цвет панели с ресурсом,если конкретный ресурс не собран | Color of the panel with the resource, if a specific resource is not assembled")]
    public string ColorPanelResourceNoComplete;
}


```

---

## Skyfall

```csharp
using System;
using UnityEngine;
using Rust;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using GameTips;
using System.Collections.Generic;

Oxide.Plugins
[Info("Skyfall", "Colon Blow", "1.0.11")]
 class Skyfall : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    static Skyfall _instance;
     BaseEntity skyfallPlane;
    static LayerMask layerMask;
    static List<ulong> skyfallplayerlist;
    static List<ulong> isParachuting;
    static List<ulong> cooldownlist;
     void Loaded();
    private bool enableLocalRespawn;
    private static float localRespawnDistance;
    private static float parchuteFwdSpeed;
    private static float parachuteDownSpeed;
    private float ChaosDropCountdown;
    private static float FlightDeck;
    private static float GlobalMapOffset;
    private bool UseCooldown;
    static float SkyFallCoolDown;
    private bool AllowFreeFall;
    private static bool ForceDismountFromPlane;
    private static float SkyFallPlaneDespawn;
    private bool DoSkyfallOnFirstTime;
    private static float ForceJumpTime;
    private static bool EnableRespawnButton;
    private static bool UseRandomRespawn;
    private static ulong wallskinid;
    private bool Changed;
     void LoadVariables();
     void LoadDefaultConfig();
     void LoadConfigVariables();
     void CheckCfg(string Key, T var);
     void CheckCfgFloat(string Key, float var);
     void CheckCfgUlong(string Key, ulong var);
    private void LoadMessages();
    [ChatCommand("skyfall")]
     void chatSkyfall(BasePlayer player, string command, string[] args);
    [ConsoleCommand("givechute")]
     void cmdConsoleGiveChute(ConsoleSystem.Arg arg);
    [ConsoleCommand("dorespawn")]
     void cmdConsoleDoRespawn(ConsoleSystem.Arg arg);
    [ConsoleCommand("chaosdrop")]
     void cmdConsoleChaosDrop(ConsoleSystem.Arg arg);
     void ActivateChaosDrop();
     void DoChaosDrop();
    public bool ListConstainsPlayerID(BasePlayer player);
     void AddPlayerID(BasePlayer player);
    public void RemovePlayerID(BasePlayer player);
     bool CooldownListConstainsPlayerID(BasePlayer player);
     void CooldownAddPlayerID(BasePlayer player);
     Vector3 FindSpawnPoint();
     Vector3 FindPlayerSKyfallPoint();
     Vector3 GetLocalSkyfallPoint(BasePlayer player);
     SkyfallPlane FindJumpPlane();
     void ActivateJumpPlane(BasePlayer player);
     void ActivateSkyfall(BasePlayer player, SkyfallPlane plane);
     void GiveChutePack(BasePlayer player);
    private void CanWearItem(PlayerInventory inventory, Item item, int targetPos);
    public void DoRespawnAt(BasePlayer player, Vector3 position, Quaternion rotation, bool isrespawn);
     void RespawnAtPlane(BasePlayer player, bool isrespawn);
     void RespawnAtRandom(BasePlayer player, bool isrespawn);
     void PreparePlayerTPSkyfall(BasePlayer player);
     void DoTPSkyfallRandom(BasePlayer player);
    public void AttachChute(BasePlayer player);
     object OnPlayerLand(BasePlayer player, float num);
     void OnPlayerInput(BasePlayer player, InputState input);
     object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
     void SendInfoMessage(BasePlayer player, string message, float time);
     object CanDismountEntity(BasePlayer player, BaseMountable entity);
    private void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    public void DisMountPlayer(BasePlayer player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    static void DestroyAll();
     void OnPlayerDie(BasePlayer player, HitInfo info);
     void AddPlayerButton(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void Unload();
     class SkyfallRespawnButton : MonoBehaviour
    {
         BasePlayer player;
         void Awake();
        public void RespawnButton(BasePlayer player);
         void DestroyCui(BasePlayer player);
         void OnDestroy();
    }

     class SkyfallPlane : BaseEntity
    {
         BaseEntity cpentity;
         BaseEntity plane;
         Vector3 cpentitypos;
        public float counter;
        public BaseEntity jumpchair1;
        public BaseEntity jumpchair2;
        public BaseEntity jumpchair3;
        public BaseEntity jumpchair4;
        public BaseEntity jumpchair5;
        public BaseEntity jumpchair6;
        public BaseEntity jumpchair7;
        public BaseEntity jumpchair8;
        public BaseEntity jumpchair9;
        public BaseEntity jumpchair10;
        public BaseEntity jumpchair11;
        public BaseEntity jumpchair12;
         BaseEntity wallsign;
         Vector3 movetopoint;
         void Awake();
         void AddWallSign();
        public bool FindMountableChair(BasePlayer player);
         bool ChairIsAvailable(BasePlayer player, BaseEntity entity);
        public bool PlayersAreMounted();
         void SpawnRefresh(BaseNetworkable entity1);
         Vector3 FindCoords();
         void FixedUpdate();
         void OnDestroy();
    }

     class PlayerParachute : BaseEntity
    {
         BaseEntity mount;
         Vector3 direction;
         Vector3 position;
        public bool moveforward;
        public bool rotright;
        public bool rotleft;
         void Awake();
         bool PlayerIsMounted();
        public void ChuteInput(InputState input, BasePlayer player);
         void FixedUpdate();
        public void OnDestroy();
    }

     class PlayerJumpController : MonoBehaviour
    {
         BasePlayer player;
        public bool usedchute;
         Skyfall _instance;
         float dismountcounter;
         void Awake();
         void FixedUpdate();
        public void OnDestroy();
    }

}

 class SkyfallRespawnButton : MonoBehaviour
{
     BasePlayer player;
     void Awake();
    public void RespawnButton(BasePlayer player);
     void DestroyCui(BasePlayer player);
     void OnDestroy();
}

 class SkyfallPlane : BaseEntity
{
     BaseEntity cpentity;
     BaseEntity plane;
     Vector3 cpentitypos;
    public float counter;
    public BaseEntity jumpchair1;
    public BaseEntity jumpchair2;
    public BaseEntity jumpchair3;
    public BaseEntity jumpchair4;
    public BaseEntity jumpchair5;
    public BaseEntity jumpchair6;
    public BaseEntity jumpchair7;
    public BaseEntity jumpchair8;
    public BaseEntity jumpchair9;
    public BaseEntity jumpchair10;
    public BaseEntity jumpchair11;
    public BaseEntity jumpchair12;
     BaseEntity wallsign;
     Vector3 movetopoint;
     void Awake();
     void AddWallSign();
    public bool FindMountableChair(BasePlayer player);
     bool ChairIsAvailable(BasePlayer player, BaseEntity entity);
    public bool PlayersAreMounted();
     void SpawnRefresh(BaseNetworkable entity1);
     Vector3 FindCoords();
     void FixedUpdate();
     void OnDestroy();
}

 class PlayerParachute : BaseEntity
{
     BaseEntity mount;
     Vector3 direction;
     Vector3 position;
    public bool moveforward;
    public bool rotright;
    public bool rotleft;
     void Awake();
     bool PlayerIsMounted();
    public void ChuteInput(InputState input, BasePlayer player);
     void FixedUpdate();
    public void OnDestroy();
}

 class PlayerJumpController : MonoBehaviour
{
     BasePlayer player;
    public bool usedchute;
     Skyfall _instance;
     float dismountcounter;
     void Awake();
     void FixedUpdate();
    public void OnDestroy();
}


```

---

## SkyReportSystem

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries;
using ConVar;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("SkyReportSystem", "DezLife", "3.2.0")]
[Description("Report system for rust")]
public class SkyReportSystem : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
     Plugin ImageLibrary;
    public string GetImage(string shortname, ulong skin);
     Dictionary<ulong, ulong> CheckPlayerModeration;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "VK:", Order = 0)]
        public VKontakte vKontakte;
        [JsonProperty(PropertyName = "Discord:", Order = 1)]
        public Discord discord;
        [JsonProperty(PropertyName = "Setting:", Order = 2)]
        public Setting setting;
        [JsonProperty(PropertyName = "Проверка на AFK:", Order = 3)]
        public IsAfk isafk;
        [JsonProperty(PropertyName = "Причины бана:", Order = 4)]
        public List<BanReasons> banReasons;
        [JsonProperty("Настройки плагина", Order = 5)]
        public List<string> Reasonsforcomplaint;
    }

    public class BanReasons
    {
        [JsonProperty(PropertyName = "Причина бана:", Order = 0)]
        public string BanReason;
        [JsonProperty(PropertyName = "Команда для бана:", Order = 1)]
        public string BanReasonCommand;
    }

    public class IsAfk
    {
        [JsonProperty(PropertyName = "Включить проверку на AFK ?", Order = 0)]
        public bool usecheckafk;
        [JsonProperty(PropertyName = "Время между проверками на AFK", Order = 2)]
        public float timecheckisafk;
    }

    public class Setting
    {
        [JsonProperty(PropertyName = "Аватар для сообщений(Для работы с IQChat )", Order = 0)]
        public ulong avatarid;
        [JsonProperty(PropertyName = "Префикс(Для работы с IQChat )", Order = 1)]
        public string prefix;
        [JsonProperty(PropertyName = "Колличевство репортов для вызова на проверку", Order = 3)]
        public int maxreportcall;
        [JsonProperty(PropertyName = "Отправлять сообщения модераторам в чат о том что игрок превысил количевство репортов (Требуется разрешения SkyReportSystem.moderator)", Order = 4)]
        public bool usemodercall;
        [JsonProperty(PropertyName = "Включить логирование ?", Order = 5)]
        public bool uselog;
        [JsonProperty(PropertyName = "Кд на отправку репортов", Order = 6)]
        public int Cooldown;
        [JsonProperty(PropertyName = "Названия сервера", Order = 7)]
        public string servername;
        [JsonProperty(PropertyName = "Команда для открытия репорт меню", Order = 8)]
        public string comandplayer;
        [JsonProperty(PropertyName = "Команда для открытия модер меню", Order = 9)]
        public string comandmoder;
        [JsonProperty(PropertyName = "Сообщения в титле", Order = 10)]
        public string teatletxt;
    }

    public class VKontakte
    {
        [JsonProperty(PropertyName = "Используем Вконтакте:", Order = 0)]
        public bool UseVK;
        [JsonProperty(PropertyName = "ID Беседы ВК для бота:", Order = 1)]
        public string VK_ChatID;
        [JsonProperty(PropertyName = "Token Группы ВК:", Order = 2)]
        public string VK_Token;
    }

    public class Discord
    {
        [JsonProperty(PropertyName = "Используем Discord:", Order = 0)]
        public bool UseDiscord;
        [JsonProperty(PropertyName = "Webhook Discrod:", Order = 1)]
        public string Discord_webHook;
    }

    protected override void LoadDefaultConfig();
     void SaveConfig(Configuration config);
    public void LoadConfigVars();
    public Dictionary<ulong, ReportUser> ReportData;
    public class ReportUser
    {
        public int ReportCount;
        public int CheckCount;
        public List<string> complaint;
    }

    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
    private void ReportActivity(BasePlayer reportplayer, BasePlayer target, string reason);
    public static string mainskymenu;
    public static string ModerMenuSky;
    public static string PlayerAlert;
    public static string ModerAlert;
    private void ReportMenu(BasePlayer player);
    [ConsoleCommand("reportplayer")]
    private void cmdreportplayer(ConsoleSystem.Arg args);
    [ConsoleCommand("players")]
    private void cmdreportplayerConsole(ConsoleSystem.Arg args);
    private void modermenu(BasePlayer player);
    [ConsoleCommand("moderreport")]
    private void cmdreportmenu(ConsoleSystem.Arg args);
    [ConsoleCommand("closeui")]
     void closeuimain(ConsoleSystem.Arg args);
    [ChatCommand("discord")]
     void discordaccess(BasePlayer player, string cmd, string[] Args);
    [ConsoleCommand("openinfoplayer")]
     void consolego(ConsoleSystem.Arg args);
    [ConsoleCommand("Banplayers")]
     void ReportSystemBanReason(ConsoleSystem.Arg arg);
    public List<ulong> BanPlayer;
    public Dictionary<ulong, int> CooldownPC;
    static DateTime epoch;
    static double CurrentTime();
     void Metods_GiveCooldown(ulong ID, int cooldown);
     bool Metods_GetCooldown(ulong ID);
    [ConsoleCommand("SkyReportGo")]
    private void PlayerReportSystem(ConsoleSystem.Arg args);
    private void PlayerBanIPlayerModeration(BasePlayer player, BasePlayer targetinfoban);
    private void ModerAlertCheck(BasePlayer player, BasePlayer target);
    private void AlertPlayerCheck(BasePlayer player, BasePlayer moderinformation);
    private void SkyModerMenu(BasePlayer player);
    private void playerinfomenu(BasePlayer player, string target, BasePlayer targetinfocheck);
    private void SkyReportPlayers(BasePlayer player, string target, string reason);
    private BasePlayer FindPlayer(string nameOrId);
    readonly Hash<ulong, Vector3> lastPosition;
    public bool IsPlayerAfk(BasePlayer player);
    private static string HexToCuiColor(string hex);
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
    private void SendChatMessage(string msg, object[] args);
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
            public Footer footer { get; set; }
            public Authors author { get; set; }
            public Embeds(string title, int color, List<Fields> fields, Authors author, Footer footer);
        }

        public FancyMessage(string content, bool tts, Embeds[] embeds);
        public string toJSON();
    }

    public class Footer
    {
        public string text { get; set; }
        public string icon_url { get; set; }
        public string proxy_icon_url { get; set; }
        public Footer(string text, string icon_url, string proxy_icon_url);
    }

    public class Authors
    {
        public string name { get; set; }
        public string url { get; set; }
        public string icon_url { get; set; }
        public string proxy_icon_url { get; set; }
        public Authors(string name, string url, string icon_url, string proxy_icon_url);
    }

    public class Fields
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public Fields(string name, string value, bool inline);
    }

    private void Request(string url, string payload, Action<int> callback);
     void SendDiscordMsg(string key, object[] args);
    private void LoadData(string name, T data, bool enableSaving);
    private void SaveData(string name, T data);
}

public class Configuration
{
    [JsonProperty(PropertyName = "VK:", Order = 0)]
    public VKontakte vKontakte;
    [JsonProperty(PropertyName = "Discord:", Order = 1)]
    public Discord discord;
    [JsonProperty(PropertyName = "Setting:", Order = 2)]
    public Setting setting;
    [JsonProperty(PropertyName = "Проверка на AFK:", Order = 3)]
    public IsAfk isafk;
    [JsonProperty(PropertyName = "Причины бана:", Order = 4)]
    public List<BanReasons> banReasons;
    [JsonProperty("Настройки плагина", Order = 5)]
    public List<string> Reasonsforcomplaint;
}

public class BanReasons
{
    [JsonProperty(PropertyName = "Причина бана:", Order = 0)]
    public string BanReason;
    [JsonProperty(PropertyName = "Команда для бана:", Order = 1)]
    public string BanReasonCommand;
}

public class IsAfk
{
    [JsonProperty(PropertyName = "Включить проверку на AFK ?", Order = 0)]
    public bool usecheckafk;
    [JsonProperty(PropertyName = "Время между проверками на AFK", Order = 2)]
    public float timecheckisafk;
}

public class Setting
{
    [JsonProperty(PropertyName = "Аватар для сообщений(Для работы с IQChat )", Order = 0)]
    public ulong avatarid;
    [JsonProperty(PropertyName = "Префикс(Для работы с IQChat )", Order = 1)]
    public string prefix;
    [JsonProperty(PropertyName = "Колличевство репортов для вызова на проверку", Order = 3)]
    public int maxreportcall;
    [JsonProperty(PropertyName = "Отправлять сообщения модераторам в чат о том что игрок превысил количевство репортов (Требуется разрешения SkyReportSystem.moderator)", Order = 4)]
    public bool usemodercall;
    [JsonProperty(PropertyName = "Включить логирование ?", Order = 5)]
    public bool uselog;
    [JsonProperty(PropertyName = "Кд на отправку репортов", Order = 6)]
    public int Cooldown;
    [JsonProperty(PropertyName = "Названия сервера", Order = 7)]
    public string servername;
    [JsonProperty(PropertyName = "Команда для открытия репорт меню", Order = 8)]
    public string comandplayer;
    [JsonProperty(PropertyName = "Команда для открытия модер меню", Order = 9)]
    public string comandmoder;
    [JsonProperty(PropertyName = "Сообщения в титле", Order = 10)]
    public string teatletxt;
}

public class VKontakte
{
    [JsonProperty(PropertyName = "Используем Вконтакте:", Order = 0)]
    public bool UseVK;
    [JsonProperty(PropertyName = "ID Беседы ВК для бота:", Order = 1)]
    public string VK_ChatID;
    [JsonProperty(PropertyName = "Token Группы ВК:", Order = 2)]
    public string VK_Token;
}

public class Discord
{
    [JsonProperty(PropertyName = "Используем Discord:", Order = 0)]
    public bool UseDiscord;
    [JsonProperty(PropertyName = "Webhook Discrod:", Order = 1)]
    public string Discord_webHook;
}

public class ReportUser
{
    public int ReportCount;
    public int CheckCount;
    public List<string> complaint;
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
        public Footer footer { get; set; }
        public Authors author { get; set; }
        public Embeds(string title, int color, List<Fields> fields, Authors author, Footer footer);
    }

    public FancyMessage(string content, bool tts, Embeds[] embeds);
    public string toJSON();
}

public class Embeds
{
    public string title { get; set; }
    public int color { get; set; }
    public List<Fields> fields { get; set; }
    public Footer footer { get; set; }
    public Authors author { get; set; }
    public Embeds(string title, int color, List<Fields> fields, Authors author, Footer footer);
}

public class Footer
{
    public string text { get; set; }
    public string icon_url { get; set; }
    public string proxy_icon_url { get; set; }
    public Footer(string text, string icon_url, string proxy_icon_url);
}

public class Authors
{
    public string name { get; set; }
    public string url { get; set; }
    public string icon_url { get; set; }
    public string proxy_icon_url { get; set; }
    public Authors(string name, string url, string icon_url, string proxy_icon_url);
}

public class Fields
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public Fields(string name, string value, bool inline);
}


```

---

## SkyReportSystem (1)

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries;
using ConVar;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("SkyReportSystem", "https://topplugin.ru/", "3.2.0")]
[Description("Report system for rust")]
public class SkyReportSystem : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
     Plugin ImageLibrary;
    public string GetImage(string shortname, ulong skin);
     Dictionary<ulong, ulong> CheckPlayerModeration;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "VK:", Order = 0)]
        public VKontakte vKontakte;
        [JsonProperty(PropertyName = "Discord:", Order = 1)]
        public Discord discord;
        [JsonProperty(PropertyName = "Setting:", Order = 2)]
        public Setting setting;
        [JsonProperty(PropertyName = "Проверка на AFK:", Order = 3)]
        public IsAfk isafk;
        [JsonProperty(PropertyName = "Причины бана:", Order = 4)]
        public List<BanReasons> banReasons;
        [JsonProperty("Настройки плагина", Order = 5)]
        public List<string> Reasonsforcomplaint;
    }

    public class BanReasons
    {
        [JsonProperty(PropertyName = "Причина бана:", Order = 0)]
        public string BanReason;
        [JsonProperty(PropertyName = "Команда для бана:", Order = 1)]
        public string BanReasonCommand;
    }

    public class IsAfk
    {
        [JsonProperty(PropertyName = "Включить проверку на AFK ?", Order = 0)]
        public bool usecheckafk;
        [JsonProperty(PropertyName = "Время между проверками на AFK", Order = 2)]
        public float timecheckisafk;
    }

    public class Setting
    {
        [JsonProperty(PropertyName = "Аватар для сообщений(Для работы с IQChat )", Order = 0)]
        public ulong avatarid;
        [JsonProperty(PropertyName = "Префикс(Для работы с IQChat )", Order = 1)]
        public string prefix;
        [JsonProperty(PropertyName = "Колличевство репортов для вызова на проверку", Order = 3)]
        public int maxreportcall;
        [JsonProperty(PropertyName = "Отправлять сообщения модераторам в чат о том что игрок превысил количевство репортов (Требуется разрешения SkyReportSystem.moderator)", Order = 4)]
        public bool usemodercall;
        [JsonProperty(PropertyName = "Включить логирование ?", Order = 5)]
        public bool uselog;
        [JsonProperty(PropertyName = "Кд на отправку репортов", Order = 6)]
        public int Cooldown;
        [JsonProperty(PropertyName = "Названия сервера", Order = 7)]
        public string servername;
        [JsonProperty(PropertyName = "Команда для открытия репорт меню", Order = 8)]
        public string comandplayer;
        [JsonProperty(PropertyName = "Команда для открытия модер меню", Order = 9)]
        public string comandmoder;
        [JsonProperty(PropertyName = "Сообщения в титле", Order = 10)]
        public string teatletxt;
    }

    public class VKontakte
    {
        [JsonProperty(PropertyName = "Используем Вконтакте:", Order = 0)]
        public bool UseVK;
        [JsonProperty(PropertyName = "ID Беседы ВК для бота:", Order = 1)]
        public string VK_ChatID;
        [JsonProperty(PropertyName = "Token Группы ВК:", Order = 2)]
        public string VK_Token;
    }

    public class Discord
    {
        [JsonProperty(PropertyName = "Используем Discord:", Order = 0)]
        public bool UseDiscord;
        [JsonProperty(PropertyName = "Webhook Discrod:", Order = 1)]
        public string Discord_webHook;
    }

    protected override void LoadDefaultConfig();
     void SaveConfig(Configuration config);
    public void LoadConfigVars();
    public Dictionary<ulong, ReportUser> ReportData;
    public class ReportUser
    {
        public int ReportCount;
        public int CheckCount;
        public List<string> complaint;
    }

    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
    private void ReportActivity(BasePlayer reportplayer, BasePlayer target, string reason);
    public static string mainskymenu;
    public static string ModerMenuSky;
    public static string PlayerAlert;
    public static string ModerAlert;
    private void ReportMenu(BasePlayer player);
    [ConsoleCommand("reportplayer")]
    private void cmdreportplayer(ConsoleSystem.Arg args);
    [ConsoleCommand("players")]
    private void cmdreportplayerConsole(ConsoleSystem.Arg args);
    private void modermenu(BasePlayer player);
    [ConsoleCommand("moderreport")]
    private void cmdreportmenu(ConsoleSystem.Arg args);
    [ConsoleCommand("closeui")]
     void closeuimain(ConsoleSystem.Arg args);
    [ChatCommand("discord")]
     void discordaccess(BasePlayer player, string cmd, string[] Args);
    [ConsoleCommand("openinfoplayer")]
     void consolego(ConsoleSystem.Arg args);
    [ConsoleCommand("Banplayers")]
     void ReportSystemBanReason(ConsoleSystem.Arg arg);
    public List<ulong> BanPlayer;
    public Dictionary<ulong, int> CooldownPC;
    static DateTime epoch;
    static double CurrentTime();
     void Metods_GiveCooldown(ulong ID, int cooldown);
     bool Metods_GetCooldown(ulong ID);
    [ConsoleCommand("SkyReportGo")]
    private void PlayerReportSystem(ConsoleSystem.Arg args);
    private void PlayerBanIPlayerModeration(BasePlayer player, BasePlayer targetinfoban);
    private void ModerAlertCheck(BasePlayer player, BasePlayer target);
    private void AlertPlayerCheck(BasePlayer player, BasePlayer moderinformation);
    private void SkyModerMenu(BasePlayer player);
    private void playerinfomenu(BasePlayer player, string target, BasePlayer targetinfocheck);
    private void SkyReportPlayers(BasePlayer player, string target, string reason);
    private BasePlayer FindPlayer(string nameOrId);
    readonly Hash<ulong, Vector3> lastPosition;
    public bool IsPlayerAfk(BasePlayer player);
    private static string HexToCuiColor(string hex);
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
    private void SendChatMessage(string msg, object[] args);
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
            public Footer footer { get; set; }
            public Authors author { get; set; }
            public Embeds(string title, int color, List<Fields> fields, Authors author, Footer footer);
        }

        public FancyMessage(string content, bool tts, Embeds[] embeds);
        public string toJSON();
    }

    public class Footer
    {
        public string text { get; set; }
        public string icon_url { get; set; }
        public string proxy_icon_url { get; set; }
        public Footer(string text, string icon_url, string proxy_icon_url);
    }

    public class Authors
    {
        public string name { get; set; }
        public string url { get; set; }
        public string icon_url { get; set; }
        public string proxy_icon_url { get; set; }
        public Authors(string name, string url, string icon_url, string proxy_icon_url);
    }

    public class Fields
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public Fields(string name, string value, bool inline);
    }

    private void Request(string url, string payload, Action<int> callback);
     void SendDiscordMsg(string key, object[] args);
    private void LoadData(string name, T data, bool enableSaving);
    private void SaveData(string name, T data);
}

public class Configuration
{
    [JsonProperty(PropertyName = "VK:", Order = 0)]
    public VKontakte vKontakte;
    [JsonProperty(PropertyName = "Discord:", Order = 1)]
    public Discord discord;
    [JsonProperty(PropertyName = "Setting:", Order = 2)]
    public Setting setting;
    [JsonProperty(PropertyName = "Проверка на AFK:", Order = 3)]
    public IsAfk isafk;
    [JsonProperty(PropertyName = "Причины бана:", Order = 4)]
    public List<BanReasons> banReasons;
    [JsonProperty("Настройки плагина", Order = 5)]
    public List<string> Reasonsforcomplaint;
}

public class BanReasons
{
    [JsonProperty(PropertyName = "Причина бана:", Order = 0)]
    public string BanReason;
    [JsonProperty(PropertyName = "Команда для бана:", Order = 1)]
    public string BanReasonCommand;
}

public class IsAfk
{
    [JsonProperty(PropertyName = "Включить проверку на AFK ?", Order = 0)]
    public bool usecheckafk;
    [JsonProperty(PropertyName = "Время между проверками на AFK", Order = 2)]
    public float timecheckisafk;
}

public class Setting
{
    [JsonProperty(PropertyName = "Аватар для сообщений(Для работы с IQChat )", Order = 0)]
    public ulong avatarid;
    [JsonProperty(PropertyName = "Префикс(Для работы с IQChat )", Order = 1)]
    public string prefix;
    [JsonProperty(PropertyName = "Колличевство репортов для вызова на проверку", Order = 3)]
    public int maxreportcall;
    [JsonProperty(PropertyName = "Отправлять сообщения модераторам в чат о том что игрок превысил количевство репортов (Требуется разрешения SkyReportSystem.moderator)", Order = 4)]
    public bool usemodercall;
    [JsonProperty(PropertyName = "Включить логирование ?", Order = 5)]
    public bool uselog;
    [JsonProperty(PropertyName = "Кд на отправку репортов", Order = 6)]
    public int Cooldown;
    [JsonProperty(PropertyName = "Названия сервера", Order = 7)]
    public string servername;
    [JsonProperty(PropertyName = "Команда для открытия репорт меню", Order = 8)]
    public string comandplayer;
    [JsonProperty(PropertyName = "Команда для открытия модер меню", Order = 9)]
    public string comandmoder;
    [JsonProperty(PropertyName = "Сообщения в титле", Order = 10)]
    public string teatletxt;
}

public class VKontakte
{
    [JsonProperty(PropertyName = "Используем Вконтакте:", Order = 0)]
    public bool UseVK;
    [JsonProperty(PropertyName = "ID Беседы ВК для бота:", Order = 1)]
    public string VK_ChatID;
    [JsonProperty(PropertyName = "Token Группы ВК:", Order = 2)]
    public string VK_Token;
}

public class Discord
{
    [JsonProperty(PropertyName = "Используем Discord:", Order = 0)]
    public bool UseDiscord;
    [JsonProperty(PropertyName = "Webhook Discrod:", Order = 1)]
    public string Discord_webHook;
}

public class ReportUser
{
    public int ReportCount;
    public int CheckCount;
    public List<string> complaint;
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
        public Footer footer { get; set; }
        public Authors author { get; set; }
        public Embeds(string title, int color, List<Fields> fields, Authors author, Footer footer);
    }

    public FancyMessage(string content, bool tts, Embeds[] embeds);
    public string toJSON();
}

public class Embeds
{
    public string title { get; set; }
    public int color { get; set; }
    public List<Fields> fields { get; set; }
    public Footer footer { get; set; }
    public Authors author { get; set; }
    public Embeds(string title, int color, List<Fields> fields, Authors author, Footer footer);
}

public class Footer
{
    public string text { get; set; }
    public string icon_url { get; set; }
    public string proxy_icon_url { get; set; }
    public Footer(string text, string icon_url, string proxy_icon_url);
}

public class Authors
{
    public string name { get; set; }
    public string url { get; set; }
    public string icon_url { get; set; }
    public string proxy_icon_url { get; set; }
    public Authors(string name, string url, string icon_url, string proxy_icon_url);
}

public class Fields
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public Fields(string name, string value, bool inline);
}


```

---

## Slasher

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("Slasher", "k1lly0u", "0.1.32", ResourceId = 1662)]
 class Slasher : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin ZoneManager;
    [PluginReference]
     Plugin Spawns;
    private bool useSlasher;
    private bool SlasherStarted;
    private bool autoOpen;
    private bool gameOpen;
    private bool Changed;
    private bool failed;
    private bool timerStarted;
    private List<SlasherPlayer> SlasherPlayers;
    public List<Timer> SlasherTimers;
    private List<ulong> DeadPlayers;
    private List<BasePlayer> Slashers;
    private List<BasePlayer> Players;
    private Dictionary<ulong, Team> Teams;
    private Dictionary<string, string> displaynameToShortname;
    static string GameName;
    static int RoundNumber;
     class SlasherPlayer : MonoBehaviour
    {
        public BasePlayer player;
        public int kills;
        public bool isSlasher;
         void Awake();
    }

     void Loaded();
     void OnServerInitialized();
     void RegisterGame();
     void LoadDefaultConfig();
     void Unload();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
     void OnSelectEventGamePost(string name);
     void OnEventPlayerSpawn(BasePlayer player);
     object OnSelectSpawnFile(string name);
     void OnSelectEventZone(MonoBehaviour monoplayer, string radius);
     void OnPostZoneCreate(string name);
     object CanEventOpen();
     object CanEventStart();
     object OnEventOpenPost();
     object OnEventClosePost();
     object OnEventEndPre();
     object OnEventEndPost();
     object OnEventStartPre();
     object OnEventStartPost();
     object CanEventJoin(BasePlayer player);
     object OnSelectKit(string kitname);
     object OnEventJoinPost(BasePlayer player);
     object OnEventLeavePost(BasePlayer player);
     void OnEventPlayerAttack(BasePlayer attacker, HitInfo hitinfo);
     void OnEventPlayerDeath(BasePlayer victim, HitInfo hitinfo);
     object EventChooseSpawn(BasePlayer player, Vector3 destination);
     object OnRequestZoneName();
    private void NextRound();
    private void BuildTimer(int type);
    private void timerMsg(string left);
    private void GiveWeapons(BasePlayer player);
    private void GiveSlasherGear(BasePlayer player);
    private void GivePlayerGear(BasePlayer player);
    private void GivePlayerWeapons();
    private void CheckPlayers();
    private void TimeLoop();
    private void CheckTime();
    private void ForceOpenEvent();
    private void OpenReminder();
     void DestroyTimers();
    private void InitializeTable();
    public object GiveItem(BasePlayer player, string itemname, int amount, ItemContainer pref);
    private void Give(BasePlayer player, Item item);
    private Item BuildItem(string shortname, int amount, object skin);
    private Item BuildSlasherWeapon(string shortname);
    private bool CheckForTeam(BasePlayer player);
    private void TeamAssign(BasePlayer player);
    private void ChooseRandomSlasher();
     void AddKill(BasePlayer player, BasePlayer victim, int amount);
     void CheckScores();
     void Winner(BasePlayer player);
    [ConsoleCommand("slasher.spawnfile")]
     void ccmdSpawns(ConsoleSystem.Arg arg);
    [ConsoleCommand("slasher.deadspawnfile")]
     void ccmdDeadSpawns(ConsoleSystem.Arg arg);
    [ConsoleCommand("slasher.toggle")]
     void ccmdToggle(ConsoleSystem.Arg arg);
     bool isAuth(ConsoleSystem.Arg arg);
    static Dictionary<string, object> EventZoneConfig;
    public float openHour;
    public float startHour;
    public float endHour;
    static string EventZoneName;
    static string PlayerSpawns;
    static string DeadPlayerSpawns;
    static string SlasherWeapon;
    static string SlasherAmmo;
    static float EventStartHealth;
    static float ffDamageMod;
    static float torchDamageMod;
    static int slasherTime;
    static int playTimer;
    static bool useAutoStart;
    static int KillTokens;
    static int WinTokens;
    static int KillSlasherTokens;
    static int AmmoAmount;
    static int maxRounds;
    static int sBoots;
    static int sPants;
    static int sShirt;
    static int sMask;
    static int pBoots;
    static int pPants;
    static int pShirt;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     object GetEventConfig(string configname);
     Dictionary<string, string> messages;
}

 class SlasherPlayer : MonoBehaviour
{
    public BasePlayer player;
    public int kills;
    public bool isSlasher;
     void Awake();
}


```

---

## SleeperAnimalProtection

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Facepunch;

Oxide.Plugins
[Info("SleeperAnimalProtection", "Fujikura", "0.2.1")]
[Description("Protects sleeping players from being killed by animals")]
 class SleeperAnimalProtection : RustPlugin
{
    private bool Changed;
    private bool usePermission;
    private string permissionName;
    private bool checkForFoundation;
    private readonly int buildingLayer;
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void Loaded();
    private List<BuildingBlock> GetFoundation(Vector3 positionCoordinates);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
}


```

---

## SleepingSystem

```csharp
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SleepingSystem", "Hougan", "0.0.1")]
public class SleepingSystem : RustPlugin
{
    private class Configuration
    {
        [JsonProperty("Количество восстанавливаемого здоровья за одну секунду")]
        public float HealPerSecond;
        [JsonProperty("Количество отнимаемого 'голода' за одну секунду")]
        public float CaloriedPerSecond;
        [JsonProperty("Количество отнимаемой 'воды' за одну секунду")]
        public float HydraPerSecond;
        [JsonProperty("Разрешать спать только на спальниках")]
        public bool SleepOnlyOnBag;
        [JsonProperty("Запрещать спать в зоне чужого шкафа")]
        public bool BlockBuildSleep;
    }

    private class Sleeper : MonoBehaviour
    {
        private const string UpdateLayer;
        private const string HydraLayer;
        private const string FoodLayer;
        private const string Layer;
        private BasePlayer Player;
        private float Interval;
        private float HealAmount;
        private float HydraAmount;
        private float CaloriesAmount;
        public void Awake();
        public void DrawInterface();
        public void ControlUpdate();
        public void UpdateInterface();
        public void OnDestroy();
    }

    private static Configuration Settings;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnPlayerInput(BasePlayer player, InputState state);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void Unload();
}

private class Configuration
{
    [JsonProperty("Количество восстанавливаемого здоровья за одну секунду")]
    public float HealPerSecond;
    [JsonProperty("Количество отнимаемого 'голода' за одну секунду")]
    public float CaloriedPerSecond;
    [JsonProperty("Количество отнимаемой 'воды' за одну секунду")]
    public float HydraPerSecond;
    [JsonProperty("Разрешать спать только на спальниках")]
    public bool SleepOnlyOnBag;
    [JsonProperty("Запрещать спать в зоне чужого шкафа")]
    public bool BlockBuildSleep;
}

private class Sleeper : MonoBehaviour
{
    private const string UpdateLayer;
    private const string HydraLayer;
    private const string FoodLayer;
    private const string Layer;
    private BasePlayer Player;
    private float Interval;
    private float HealAmount;
    private float HydraAmount;
    private float CaloriesAmount;
    public void Awake();
    public void DrawInterface();
    public void ControlUpdate();
    public void UpdateInterface();
    public void OnDestroy();
}


```

---

## SlidingDoors

```csharp
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("SlidingDoors", "k1lly0u", "2.0.2")]
 class SlidingDoors : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    private static SlidingDoors Instance { get; set; }
    private bool wipeData;
    private const string PERMISSION_WOOD;
    private const string PERMISSION_METAL;
    private const string PERMISSION_ARMOUR;
    private const string PERMISSION_USE;
    private const string PERMISSION_ALL;
    private const string PERMISSION_AUTO_DOOR;
    private const string PERMISSION_AUTO_DEPLOY;
    private const string DOOR_WOOD_SPN;
    private const string DOOR_METAL_SPN;
    private const string DOOR_ARMOUR_SPN;
    private const string EFFECT_WOOD;
    private const string EFFECT_METAL;
    private FieldInfo OwnerItemUID;
    private void Loaded();
    private void OnNewSave();
    private void OnServerSave();
    private void OnServerInitialized();
    private void OnDoorOpened(Door door, BasePlayer player);
    private void OnItemDeployed(Deployer deployer, BaseEntity entity);
    private int GetOwnerItemID(Deployer deployer);
    private void OnEntitySpawned(BaseNetworkable baseNetworkable);
    private void OnEntityKill(BaseNetworkable baseNetworkable);
    private void Unload();
    private IEnumerator LoadSlidingDoors();
    private IEnumerator RestoreOnSaveFinished();
    private static T FindEntityFromRay(BasePlayer player);
    private static bool HasPermission(ulong ownerId, string perm);
    private static SlidingDoor.DoorType ToDoorType(string shortPrefabName);
    private class SlidingDoor : MonoBehaviour
    {
        public static List<SlidingDoor> _allDoors;
        protected Door Entity { get; set; }
        private List<DoorEntity> Children;
        private Transform tr;
        private Vector3 originalPosition;
        private Vector3 openPosition;
        private bool isOpening;
        private bool isPaused;
        private DoorType doorType;
        private float timeTaken;
        private float timeToTake;
        public bool IsOpen { get; set; }
        private void Awake();
        private void Update();
        private void OnDestroy();
        private bool onSave_enabled;
        private float onSave_timeTaken;
        private Vector3 onSave_currentPosition;
        internal void CloseForGameSave();
        internal void RestoreAfterGameSave();
        internal void ResetDoorState();
        internal void OnDoorToggled();
        private void UpdateCurrentChildren();
        private void SendNetworkUpdate();
    }

    [ChatCommand("sdoor")]
    private void cmdSDoor(BasePlayer player, string command, string[] args);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Auto-Close Settings")]
        public AutoDoorSettings AutoDoor { get; set; }
        [JsonProperty(PropertyName = "Door Settings")]
        public DoorSettings Door { get; set; }
        public class AutoDoorSettings
        {
            [JsonProperty(PropertyName = "Amount of time before auto-closing a door (seconds)")]
            public float OpenTime { get; set; }
            [JsonProperty(PropertyName = "Enable automatic door closing")]
            public bool Enabled { get; set; }
        }

        public class DoorSettings
        {
            [JsonProperty(PropertyName = "Door Speed (Wood)")]
            public float WoodSpeed { get; set; }
            [JsonProperty(PropertyName = "Door Speed (Metal)")]
            public float MetalSpeed { get; set; }
            [JsonProperty(PropertyName = "Door Speed (Armour)")]
            public float ArmourSpeed { get; set; }
            [JsonProperty(PropertyName = "Require code lock authentication")]
            public bool CodeLockAuth { get; set; }
            [JsonProperty(PropertyName = "Require building privilege")]
            public bool BuildingAuth { get; set; }
            [JsonProperty(PropertyName = "Automatically apply to all doors")]
            public bool ApplyToAll { get; set; }
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
        public List<uint> ids;
    }

    private string msg(string key, ulong playerId);
    private Dictionary<string, string> Messages;
}

private class SlidingDoor : MonoBehaviour
{
    public static List<SlidingDoor> _allDoors;
    protected Door Entity { get; set; }
    private List<DoorEntity> Children;
    private Transform tr;
    private Vector3 originalPosition;
    private Vector3 openPosition;
    private bool isOpening;
    private bool isPaused;
    private DoorType doorType;
    private float timeTaken;
    private float timeToTake;
    public bool IsOpen { get; set; }
    private void Awake();
    private void Update();
    private void OnDestroy();
    private bool onSave_enabled;
    private float onSave_timeTaken;
    private Vector3 onSave_currentPosition;
    internal void CloseForGameSave();
    internal void RestoreAfterGameSave();
    internal void ResetDoorState();
    internal void OnDoorToggled();
    private void UpdateCurrentChildren();
    private void SendNetworkUpdate();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Auto-Close Settings")]
    public AutoDoorSettings AutoDoor { get; set; }
    [JsonProperty(PropertyName = "Door Settings")]
    public DoorSettings Door { get; set; }
    public class AutoDoorSettings
    {
        [JsonProperty(PropertyName = "Amount of time before auto-closing a door (seconds)")]
        public float OpenTime { get; set; }
        [JsonProperty(PropertyName = "Enable automatic door closing")]
        public bool Enabled { get; set; }
    }

    public class DoorSettings
    {
        [JsonProperty(PropertyName = "Door Speed (Wood)")]
        public float WoodSpeed { get; set; }
        [JsonProperty(PropertyName = "Door Speed (Metal)")]
        public float MetalSpeed { get; set; }
        [JsonProperty(PropertyName = "Door Speed (Armour)")]
        public float ArmourSpeed { get; set; }
        [JsonProperty(PropertyName = "Require code lock authentication")]
        public bool CodeLockAuth { get; set; }
        [JsonProperty(PropertyName = "Require building privilege")]
        public bool BuildingAuth { get; set; }
        [JsonProperty(PropertyName = "Automatically apply to all doors")]
        public bool ApplyToAll { get; set; }
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class AutoDoorSettings
{
    [JsonProperty(PropertyName = "Amount of time before auto-closing a door (seconds)")]
    public float OpenTime { get; set; }
    [JsonProperty(PropertyName = "Enable automatic door closing")]
    public bool Enabled { get; set; }
}

public class DoorSettings
{
    [JsonProperty(PropertyName = "Door Speed (Wood)")]
    public float WoodSpeed { get; set; }
    [JsonProperty(PropertyName = "Door Speed (Metal)")]
    public float MetalSpeed { get; set; }
    [JsonProperty(PropertyName = "Door Speed (Armour)")]
    public float ArmourSpeed { get; set; }
    [JsonProperty(PropertyName = "Require code lock authentication")]
    public bool CodeLockAuth { get; set; }
    [JsonProperty(PropertyName = "Require building privilege")]
    public bool BuildingAuth { get; set; }
    [JsonProperty(PropertyName = "Automatically apply to all doors")]
    public bool ApplyToAll { get; set; }
}

private class StoredData
{
    public List<uint> ids;
}


```

---

## SlotPanel

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System;
using System.Linq;
using GameTips;
using UnityEngine.UI;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SlotPanel", "Morecelo", "0.2.1")]
[Description("Плагин добавляет ГУИ панель под слоты игрока, которая отображает основные события на сервере")]
 class SlotPanel : RustPlugin
{
    private bool P_ShowKill;
    private bool P_ShowMessages;
    private bool P_SM_DoubleToChat;
    private int P_SM_Interval;
    private bool P_ShowConnections;
    private bool P_ShowDisconnections;
    private string P_ShadowSize;
    private string P_FontName;
    private int P_FontSize;
     void OnServerInitialized();
    protected override void LoadDefaultConfig();
    private void Unload();
     void StartBroadcast();
     string GetMessage();
     string ReplaceMessage(BasePlayer player, string currentText);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPlayerDie(BasePlayer player, HitInfo info);
     void PanelGUI(BasePlayer player, string message);
     void Reply(BasePlayer player, string message, object[] args);
     T GetConfig(string name, T value);
}


```

---

## SmallDecay

```csharp
using System.Collections.Generic;
using System.Linq;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SmallDecay", "Sempai#3239", "0.0.1")]
[Description("Гниение Mercury")]
 class SmallDecay : RustPlugin
{
    [PluginReference]
     Plugin IQChat;
    public void SendChat(BasePlayer player, string Message, Chat.ChatChannel channel);
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty("Через сколько времени снимать ХП смоллтешу(секунды)")]
        public int DecayTime;
        [JsonProperty("Сколько ХП снимать соллтешу(У тайника - 150 ХП)")]
        public int DecayDamage;
        [JsonProperty("Сообщение игроку")]
        public string MessagePlayer;
        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public List<StashContainer> SmollData;
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityKill(BaseNetworkable entity);
    private void OnServerInitialized();
     void DecayStart();
     void DecayWrite(StashContainer stash);
}

private class Configuration
{
    [JsonProperty("Через сколько времени снимать ХП смоллтешу(секунды)")]
    public int DecayTime;
    [JsonProperty("Сколько ХП снимать соллтешу(У тайника - 150 ХП)")]
    public int DecayDamage;
    [JsonProperty("Сообщение игроку")]
    public string MessagePlayer;
    public static Configuration GetNewConfiguration();
}


```

---

## SmartHomes

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using System.Reflection;
using Facepunch;
using System;

Oxide.Plugins
[Info("SmartHomes", "k1lly0u & DylanSMR", "0.1.3", ResourceId = 2051)]
 class SmartHomes : RustPlugin
{
    static MethodInfo updatelayer;
     List<ulong> setupUI;
     List<ulong> barUI;
     List<ulong> isEditing;
     Dictionary<ulong, newData> newD;
     class newData
    {
        public uint entKey;
        public string entName;
        public bool entStatus;
        public string entType;
        public Vector3 entLocation;
        public newData();
    }

     void Loaded();
     void Unload();
     void OnServerSave();
     void OnPlayerInit(BasePlayer player);
     void SaveData();
     void OnServerInitialized();
    static HomeData homeData;
     class HomeData
    {
        public Dictionary<ulong, Homes> homeD;
    }

     class Homes
    {
        public float locx;
        public float locy;
        public float locz;
        public ulong playerID;
        public Dictionary<string, TurretData> tData;
        public Dictionary<string, LightData> lData;
        public Dictionary<string, DoorData> dData;
        public List<float> objectX;
        public Homes();
        internal static Homes Find(BasePlayer player);
    }

    public class TurretData
    {
        public float locx;
        public float locy;
        public float locz;
        public uint key;
        public bool status;
        public string name;
        public TurretData();
    }

    public class LightData
    {
        public float locx;
        public float locy;
        public float locz;
        public uint key;
        public bool status;
        public string name;
        public LightData();
    }

    public class DoorData
    {
        public float locx;
        public float locy;
        public float locz;
        public uint key;
        public bool status;
        public string name;
        public DoorData();
    }

    private ConfigData configData;
     class Options
    {
        public float ActivationDistance { get; set; }
    }

     class ConfigData
    {
        public Options Options { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
    [ConsoleCommand("CloseCUIMain")]
    private void destroyUI(ConsoleSystem.Arg arg);
     void destroyUIN(ConsoleSystem.Arg arg);
    [ChatCommand("rem")]
     void openUI(BasePlayer player);
    [ConsoleCommand("CUI_ControlMenu")]
     void ControlMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ControlOpen")]
     void ControlOpen(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ChangeElement")]
    private void ChangeElement(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_OpenTurret")]
     void ControlTurret(ConsoleSystem.Arg arg, int page);
    private void CreateTurretButton(CuiElementContainer container, string panelName, TurretData data, BasePlayer player, int num);
    [ConsoleCommand("CUI_ToggleTurret")]
     void toggle(ConsoleSystem.Arg arg);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    [ConsoleCommand("CUI_OpenLight")]
     void ControlLight(ConsoleSystem.Arg arg, int page);
    private void CreateLightButton(CuiElementContainer container, string panelName, LightData light, BasePlayer player, int num);
    [ConsoleCommand("CUI_ToggleLight")]
     void toggleLight(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_OpenDoor")]
     void ControlDoor(ConsoleSystem.Arg arg, int page);
    private void CreateDoorButton(CuiElementContainer container, string panelName, DoorData door, BasePlayer player, int num);
    [ConsoleCommand("CUI_ToggleDoor")]
     void toggleDoor(ConsoleSystem.Arg arg);
    private float[] CalcButtonPos(int number);
    [ConsoleCommand("CUI_Homes")]
     void SetUP(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_HomesNew")]
     void SetUPB(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_HomeNew2")]
     void SetUP2B(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_Homesetup2")]
     void SetUP2(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_HomeNew3")]
     void SetUP3B(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_Homesetup3")]
     void SetUP3(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ObjectSetup")]
     void ObjectSetup(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ObjectRemove1")]
     void ObjectRemove(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ObjectRemove2")]
     void ObjectRemove2(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ObjectSetup2")]
     void ObjectSetup2(ConsoleSystem.Arg arg);
     List<T> FindEntities(Vector3 position, float distance);
    [ConsoleCommand("CUI_ObjectSetup3")]
     void ObjectSetup3(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_ObjectSetup4")]
     void ObjectSetup4(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_SaveNewObject")]
     void ObjectSetup5(ConsoleSystem.Arg arg);
     void OnPlayerChat(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUI_RemoteBar")]
     void RemoteBar(BasePlayer player);
    static string PublicSetupName;
    static string PublicSideBar;
    static string PublicObjectSetup;
    static string PublicControlSetup;
    static string PublicControlSetupTurret;
    static string PublicControlSetupLight;
    static string PublicControlSetupDoor;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    }

    private Dictionary<string, string> UIColors;
     Dictionary<string, string> messages;
}

 class newData
{
    public uint entKey;
    public string entName;
    public bool entStatus;
    public string entType;
    public Vector3 entLocation;
    public newData();
}

 class HomeData
{
    public Dictionary<ulong, Homes> homeD;
}

 class Homes
{
    public float locx;
    public float locy;
    public float locz;
    public ulong playerID;
    public Dictionary<string, TurretData> tData;
    public Dictionary<string, LightData> lData;
    public Dictionary<string, DoorData> dData;
    public List<float> objectX;
    public Homes();
    internal static Homes Find(BasePlayer player);
}

public class TurretData
{
    public float locx;
    public float locy;
    public float locz;
    public uint key;
    public bool status;
    public string name;
    public TurretData();
}

public class LightData
{
    public float locx;
    public float locy;
    public float locz;
    public uint key;
    public bool status;
    public string name;
    public LightData();
}

public class DoorData
{
    public float locx;
    public float locy;
    public float locz;
    public uint key;
    public bool status;
    public string name;
    public DoorData();
}

 class Options
{
    public float ActivationDistance { get; set; }
}

 class ConfigData
{
    public Options Options { get; set; }
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
}


```

---

## SoFriends

```csharp
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System;
using Facepunch.Extend;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;
using Pool = Facepunch.Pool;

Oxide.Plugins
[Info("SoFriends", "https://topplugin.ru/", "1.1.2")]
public class SoFriends : RustPlugin
{
    private Dictionary<ulong, FriendData> friendData;
    private Dictionary<ulong, ulong> playerAccept;
    private static Configs cfg { get; set; }
    private class FriendData
    {
        [JsonProperty("Ник")]
        public string Name;
        [JsonProperty("Список друзей")]
        public Dictionary<ulong, FriendAcces> friendList;
        public class FriendAcces
        {
            [JsonProperty("Ник")]
            public string name;
            [JsonProperty("Урон по человеку")]
            public bool Damage;
            [JsonProperty("Авторизациия в турелях")]
            public bool Turret;
            [JsonProperty("Авторизациия в дверях")]
            public bool Door;
            [JsonProperty("Авторизациия в пво")]
            public bool Sam;
            [JsonProperty("Авторизациия в шкафу")]
            public bool bp;
        }

    }

    private class Configs
    {
        [JsonProperty("Включить настройку авто авторизации турелей?")]
        public bool Turret;
        [JsonProperty("Включить настройку урона по своим?")]
        public bool Damage;
        [JsonProperty("Включить настройку авто авторизации в дверях?")]
        public bool Door;
        [JsonProperty("Включить настройку авто авторизации в пво?")]
        public bool Sam;
        [JsonProperty("Включить настройку авто авторизации в шкафу?")]
        public bool build;
        [JsonProperty("Сколько максимум людей может быть в друзьях?")]
        public int MaxFriends;
        [JsonProperty("Урон по человеку(По стандрату у игрока включена?)")]
        public bool SDamage;
        [JsonProperty("Авторизациия в турелях(По стандрату у игрока включена?)")]
        public bool STurret;
        [JsonProperty("Авторизациия в дверях(По стандрату у игрока включена?)")]
        public bool SDoor;
        [JsonProperty("Авторизациия в пво(По стандрату у игрока включена?)")]
        public bool SSam;
        [JsonProperty("Авторизациия в шкафу(По стандрату у игрока включена?)")]
        public bool bp;
        [JsonProperty("Время ожидания  ответа на запроса в секнудах")]
        public int otvet;
        [JsonProperty("Вообще включать пво настройку?")]
        public bool SSamOn;
        public static Configs GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    private string PlugName;
    [ChatCommand("f")]
    private void FriendCmd(BasePlayer player, string command, string[] arg);
    [ConsoleCommand("friendui2")]
    private void FriendConsole(ConsoleSystem.Arg arg);
    [ChatCommand("friend")]
    private void FriendCmd2(BasePlayer player, string command, string[] arg);
    private void OnEntitySpawned(BuildingPrivlidge entity);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity entity);
    private object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
    private object OnSamSiteTarget(SamSite entity, BaseCombatEntity target);
    private void OnPlayerConnected(BasePlayer player);
    private void OnServerInitialized();
    private void Unload();
    private static string Layer;
    private string LayerInvite;
    private string Hud;
    private string Overlay;
    private CuiPanel Fon;
    private CuiPanel MainFon;
    private CuiPanel MainPanel;
    private CuiElement ButtonList;
    private CuiElement ButtonListText;
    private CuiButton SettingText;
    private CuiElement AddFriendsText;
    private CuiElement AddFriends;
    private CuiButton CloseKrest;
    private CuiButton ClosePanel;
    private CuiPanel Invite;
    private CuiButton DenyButton;
    private CuiButton AcceptButton;
    private CuiPanel StaticSetPanel;
     CuiElement page0;
     CuiElement page1;
     CuiElement page2;
     CuiElement page3;
    private void InivteStart(BasePlayer inviter, BasePlayer target);
    [ChatCommand("fmenu")]
    private void StartUi(BasePlayer player);
    [ConsoleCommand("friendui")]
    private void FriendUI(ConsoleSystem.Arg arg);
    private void AddFriendList(BasePlayer player, int page);
    private void SettingInit(BasePlayer player, ulong steamdIdTarget);
    private void AuthBuild(BasePlayer player, ulong friendId);
    private void FriendsInit(BasePlayer player, int page);
    private static string HexToRustFormat(string hex);
    private bool HasFriend(ulong playerId, ulong friendId);
    private bool AreFriends(ulong playerId, ulong friendId);
    private bool AddFriend(ulong playerId, ulong friendId);
    private bool RemoveFriend(ulong playerId, ulong friendId);
    private bool IsFriend(ulong playerId, ulong friendId);
    private int GetMaxFriends();
    private ulong[] GetFriends(ulong playerId);
    private ulong[] GetFriendList(string playerId);
    private ulong[] GetFriendList(ulong playerId);
    private ulong[] IsFriendOf(ulong playerId);
}

private class FriendData
{
    [JsonProperty("Ник")]
    public string Name;
    [JsonProperty("Список друзей")]
    public Dictionary<ulong, FriendAcces> friendList;
    public class FriendAcces
    {
        [JsonProperty("Ник")]
        public string name;
        [JsonProperty("Урон по человеку")]
        public bool Damage;
        [JsonProperty("Авторизациия в турелях")]
        public bool Turret;
        [JsonProperty("Авторизациия в дверях")]
        public bool Door;
        [JsonProperty("Авторизациия в пво")]
        public bool Sam;
        [JsonProperty("Авторизациия в шкафу")]
        public bool bp;
    }

}

public class FriendAcces
{
    [JsonProperty("Ник")]
    public string name;
    [JsonProperty("Урон по человеку")]
    public bool Damage;
    [JsonProperty("Авторизациия в турелях")]
    public bool Turret;
    [JsonProperty("Авторизациия в дверях")]
    public bool Door;
    [JsonProperty("Авторизациия в пво")]
    public bool Sam;
    [JsonProperty("Авторизациия в шкафу")]
    public bool bp;
}

private class Configs
{
    [JsonProperty("Включить настройку авто авторизации турелей?")]
    public bool Turret;
    [JsonProperty("Включить настройку урона по своим?")]
    public bool Damage;
    [JsonProperty("Включить настройку авто авторизации в дверях?")]
    public bool Door;
    [JsonProperty("Включить настройку авто авторизации в пво?")]
    public bool Sam;
    [JsonProperty("Включить настройку авто авторизации в шкафу?")]
    public bool build;
    [JsonProperty("Сколько максимум людей может быть в друзьях?")]
    public int MaxFriends;
    [JsonProperty("Урон по человеку(По стандрату у игрока включена?)")]
    public bool SDamage;
    [JsonProperty("Авторизациия в турелях(По стандрату у игрока включена?)")]
    public bool STurret;
    [JsonProperty("Авторизациия в дверях(По стандрату у игрока включена?)")]
    public bool SDoor;
    [JsonProperty("Авторизациия в пво(По стандрату у игрока включена?)")]
    public bool SSam;
    [JsonProperty("Авторизациия в шкафу(По стандрату у игрока включена?)")]
    public bool bp;
    [JsonProperty("Время ожидания  ответа на запроса в секнудах")]
    public int otvet;
    [JsonProperty("Вообще включать пво настройку?")]
    public bool SSamOn;
    public static Configs GetNewConf();
}


```

---

## SoInfo

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SoInfo", "Sempai#3239", "1.0.2")]
public class SoInfo : RustPlugin
{
    private ConfigData cfg { get; set; }
    private class ConfigData
    {
        [JsonProperty("Стартовая страница")]
        public string Name;
        [JsonProperty("Цвет линий")]
        public string Color;
        [JsonProperty("Цвет линий(Открытой вкладки)")]
        public string ColorOpen;
        [JsonProperty("Включить открытие при заходе?")]
        public bool coonect;
        [JsonProperty("Список кнопок")]
        public Dictionary<string, TextInfo> _itemList;
        public static ConfigData GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
     class TextInfo
    {
        public string Lable;
        public string UnderLable;
        public List<string> Text;
    }

    private static string Layer;
    private static string LayerMain;
    private string Hud;
    private string Overlay;
    private string regular;
    private static string Sharp;
    private static string Blur;
    public static string radial;
    private CuiPanel _fon;
    private CuiPanel _mainFon;
    private CuiPanel _mainFon2;
    [ChatCommand("info")]
    private void StartUi(BasePlayer player);
    [ConsoleCommand("uisoinfo")]
    private void LoadButtonInfo(ConsoleSystem.Arg arg);
    private void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private static string HexToRustFormat(string hex);
}

private class ConfigData
{
    [JsonProperty("Стартовая страница")]
    public string Name;
    [JsonProperty("Цвет линий")]
    public string Color;
    [JsonProperty("Цвет линий(Открытой вкладки)")]
    public string ColorOpen;
    [JsonProperty("Включить открытие при заходе?")]
    public bool coonect;
    [JsonProperty("Список кнопок")]
    public Dictionary<string, TextInfo> _itemList;
    public static ConfigData GetNewConf();
}

 class TextInfo
{
    public string Lable;
    public string UnderLable;
    public List<string> Text;
}


```

---

## SoPass

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Facepunch.Extend;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SoPass", "Xoloproshka", "1.2.0")]
public class SoPass : RustPlugin
{
    private Dictionary<ulong, PlayerData> _playerData;
    private ConfigData cfg { get; set; }
    internal class Reward
    {
        [JsonProperty("Шортнейм(Шортнейм предмета или название команды или название набора)")]
        public string ShortName;
        [JsonProperty("Кол-во")]
        public int Amount;
        [JsonProperty("Скинайди")]
        public ulong SkinId;
        [JsonProperty("Команда(Если надо)")]
        public string command;
        [JsonProperty("Использовать набор?")]
        public bool nabor;
        [JsonProperty("Картинка(Если команда или набор)")]
        public string URL;
        [JsonProperty(
                "Набор: Список предметов и команд(Если используете набор все параметры кроме \"Картинка\" и \"Шортнейм\" оставить пустыми и поставить использовать набор на true)")]
        public List<Items> itemList;
        internal class Items
        {
            [JsonProperty("Шортнейм")]
            public string ShortName;
            [JsonProperty("Кол-во")]
            public int Amount;
            [JsonProperty("Скинайди")]
            public ulong SkinId;
            [JsonProperty("Команда(Если надо)")]
            public string command;
        }

    }

    private class ConfigData
    {
        [JsonProperty("Список задач для классов(\"Название класса\":{ Список задач)}")]
        public Dictionary<string, List<Quest>> _listQuest;
        [JsonProperty("Список классов")]
        public List<ClassPlayer> _classList;
        internal class ClassPlayer
        {
            [JsonProperty("Название")]
            public string Name;
            [JsonProperty("Картинка")]
            public string URL;
            [JsonProperty("Пермищен")]
            public string Perm;
            [JsonProperty("Описание")]
            public string Text;
        }

        public static ConfigData GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
     class Zadachi
    {
        [JsonProperty("Текст задачи")]
        public string DisplayName;
        [JsonProperty(
                "Тип задачи(1-Добыть, 2-Убить, 3-Скрафтить, 4-Изучить,5-Залутать, 6-Поставить,7-Починить,8-Собрать с земли)")]
        public int type;
        [JsonProperty("Задача закончен(Оставлять false)")]
        public bool IsFinished;
        [JsonProperty("Объект задачи(Тип Калаш-rifle.ak, Игрок-player")]
        public string need;
        [JsonProperty("Кол-во")]
        public int amount;
        [JsonProperty("Оружие или инструмент(Например задача убить с калаша, тогда сюда rifle.ak)")]
        public string Weapon;
    }

     class Quest
    {
        [JsonProperty("Название уровня")]
        public string DisplayName;
        [JsonProperty("Какой лвл")]
        public int Lvl;
        [JsonProperty(
                "Список наград(Если используете набор все параметры кроме \"Картинка\" и \"Шортнейм\" оставить пустыми и поставить использовать набор на true))")]
        public List<Reward> _listReward;
        [JsonProperty("Список задач")]
        public List<Zadachi> _listZadach;
    }

     class PlayerData
    {
        [JsonProperty("НикНейм")]
        public string NickName;
        [JsonProperty("Класс")]
        public string Klass;
        [JsonProperty("Лвл")]
        public int Lvl;
        [JsonProperty("Список активных заданий")]
        public List<Zadachi> listZadachi;
        [JsonProperty("Список ревардов")]
        public List<Reward> ListRewards;
    }

    private static string Layer;
    private static string LayerMain;
    private string Hud;
    private string Overlay;
    private string regular;
    private static string Sharp;
    private static string Blur;
    private string radial;
    private CuiPanel _fon;
    private CuiPanel _mainFon;
    [ChatCommand("pass")]
    private void Start(BasePlayer player);
     void StartUI(BasePlayer player, int num);
    private void LoadZadach(BasePlayer player, int page);
    [ConsoleCommand("uiopeninv")]
     void OpenInv(ConsoleSystem.Arg arg);
    private void LoadPanelNagrads(BasePlayer player, int page, string klass);
    private void LoadNagrads(BasePlayer player, int page, string klass);
    private void LoadInv(BasePlayer player, int page);
    [ConsoleCommand("UISoPass")]
     void SoPassCommand(ConsoleSystem.Arg arg);
    private object OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
    private object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private void OnItemResearch(ResearchTable table, Item item, BasePlayer player);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnEntitySpawned(BaseEntity entity);
    private void OnItemRepair(BasePlayer player, Item item);
    private object OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
    private Dictionary<int, string> _gradeList;
    private object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
    private object OnCardSwipe(CardReader cardReader, Keycard card, BasePlayer player);
    private object OnBuyVendingItem(VendingMachine machine, BasePlayer player, int sellOrderId, int numberOfTransactions);
    private void OnServerInitialized();
    private void Unload();
     void Check(BasePlayer player, Zadachi findZadah, int amount, PlayerData data, string weapon);
    private bool IsFriends(ulong owner, ulong player);
    [PluginReference]
    private Plugin ImageLibrary;
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    private static string HexToRustFormat(string hex);
    private void ReplySend(BasePlayer player, string message);
    [PluginReference]
    private Plugin SoFriends;
    private Plugin Friends;
}

internal class Reward
{
    [JsonProperty("Шортнейм(Шортнейм предмета или название команды или название набора)")]
    public string ShortName;
    [JsonProperty("Кол-во")]
    public int Amount;
    [JsonProperty("Скинайди")]
    public ulong SkinId;
    [JsonProperty("Команда(Если надо)")]
    public string command;
    [JsonProperty("Использовать набор?")]
    public bool nabor;
    [JsonProperty("Картинка(Если команда или набор)")]
    public string URL;
    [JsonProperty(
                "Набор: Список предметов и команд(Если используете набор все параметры кроме \"Картинка\" и \"Шортнейм\" оставить пустыми и поставить использовать набор на true)")]
    public List<Items> itemList;
    internal class Items
    {
        [JsonProperty("Шортнейм")]
        public string ShortName;
        [JsonProperty("Кол-во")]
        public int Amount;
        [JsonProperty("Скинайди")]
        public ulong SkinId;
        [JsonProperty("Команда(Если надо)")]
        public string command;
    }

}

internal class Items
{
    [JsonProperty("Шортнейм")]
    public string ShortName;
    [JsonProperty("Кол-во")]
    public int Amount;
    [JsonProperty("Скинайди")]
    public ulong SkinId;
    [JsonProperty("Команда(Если надо)")]
    public string command;
}

private class ConfigData
{
    [JsonProperty("Список задач для классов(\"Название класса\":{ Список задач)}")]
    public Dictionary<string, List<Quest>> _listQuest;
    [JsonProperty("Список классов")]
    public List<ClassPlayer> _classList;
    internal class ClassPlayer
    {
        [JsonProperty("Название")]
        public string Name;
        [JsonProperty("Картинка")]
        public string URL;
        [JsonProperty("Пермищен")]
        public string Perm;
        [JsonProperty("Описание")]
        public string Text;
    }

    public static ConfigData GetNewConf();
}

internal class ClassPlayer
{
    [JsonProperty("Название")]
    public string Name;
    [JsonProperty("Картинка")]
    public string URL;
    [JsonProperty("Пермищен")]
    public string Perm;
    [JsonProperty("Описание")]
    public string Text;
}

 class Zadachi
{
    [JsonProperty("Текст задачи")]
    public string DisplayName;
    [JsonProperty(
                "Тип задачи(1-Добыть, 2-Убить, 3-Скрафтить, 4-Изучить,5-Залутать, 6-Поставить,7-Починить,8-Собрать с земли)")]
    public int type;
    [JsonProperty("Задача закончен(Оставлять false)")]
    public bool IsFinished;
    [JsonProperty("Объект задачи(Тип Калаш-rifle.ak, Игрок-player")]
    public string need;
    [JsonProperty("Кол-во")]
    public int amount;
    [JsonProperty("Оружие или инструмент(Например задача убить с калаша, тогда сюда rifle.ak)")]
    public string Weapon;
}

 class Quest
{
    [JsonProperty("Название уровня")]
    public string DisplayName;
    [JsonProperty("Какой лвл")]
    public int Lvl;
    [JsonProperty(
                "Список наград(Если используете набор все параметры кроме \"Картинка\" и \"Шортнейм\" оставить пустыми и поставить использовать набор на true))")]
    public List<Reward> _listReward;
    [JsonProperty("Список задач")]
    public List<Zadachi> _listZadach;
}

 class PlayerData
{
    [JsonProperty("НикНейм")]
    public string NickName;
    [JsonProperty("Класс")]
    public string Klass;
    [JsonProperty("Лвл")]
    public int Lvl;
    [JsonProperty("Список активных заданий")]
    public List<Zadachi> listZadachi;
    [JsonProperty("Список ревардов")]
    public List<Reward> ListRewards;
}


```

---

## SoReport

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Facepunch.Extend;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SoReport", "https://topplugin.ru/", "1.0.1")]
public class SoReport : RustPlugin
{
    private ConfigData cfg { get; set; }
    private class ConfigData
    {
        [JsonProperty("Название сервера")]
        public string Lable;
        [JsonProperty("Включить дискорд?")]
        public bool discord;
        [JsonProperty("Dircord Report WebHook")]
        public string discordhook;
        [JsonProperty("Dircord PlayerSayDiscord WebHook")]
        public string discordhook2;
        [JsonProperty("Отправлять ли уведомления в телеграмм ?")]
        public bool TelegramUse;
        [JsonProperty("Название чата(Пригласить своего бота в чат)")]
        public string chatid;
        [JsonProperty("Создать своего бота через BotFather и скопировать сюда токен")]
        public string botToken;
        [JsonProperty("Отправлять ли уведомления в вконтакте ?")]
        public bool Vkontakte;
        [JsonProperty("Вк админа")]
        public string vkadmin;
        [JsonProperty("VK Token группы")]
        public string vkAcces;
        [JsonProperty("Cooldown")]
        public int cooldown;
        [JsonProperty("Причина 1")]
        public string res1;
        [JsonProperty("Причина 2")]
        public string res2;
        [JsonProperty("Причина 3")]
        public string res3;
        [JsonProperty("Причина 4")]
        public string res4;
        [JsonProperty("Кол-во репортов для появление в панели модератора")]
        public int kolreport;
        [JsonProperty("Кол-во репортов для подсветки красный в панели модератора")]
        public int redcolor;
        [JsonProperty("Кол-во проверки для подсветки зеленым в панели модератора")]
        public int greencolor;
        public static ConfigData GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
     Dictionary<ulong, PlayerData> _playerData;
     class PlayerData
    {
        public string UserName;
        public int ReportCount;
        public int AlertCount;
        public double ReportCD;
        public double IsCooldown { get; set; }
    }

    private static string Layer;
    private string Hud;
    private string Overlay;
    private string regular;
    private static string Sharp;
    private static string Blur;
    private string radial;
    private CuiPanel _fon;
    private CuiPanel _mainFon;
    private CuiPanel _redPanel;
    private CuiPanel _modPanel;
    private CuiPanel _mod2Panel;
    private CuiPanel _playersPanel;
    private CuiButton _close;
    [ChatCommand("report")]
    private void StartUi(BasePlayer player);
    private void Alert(ulong targetId);
    private void ModeratorInterface(BasePlayer player, int page);
    private void LoadProfile(BasePlayer player, ulong targetId);
    private void ReportFon(BasePlayer player);
    private void PlayerListLoad(BasePlayer player, int page, string find);
    [ChatCommand("discord")]
    private void SayDiscord(BasePlayer player, string c, string[] a);
    [ConsoleCommand("UISoReport")]
    private void SoCommands(ConsoleSystem.Arg arg);
    [ChatCommand("alert")]
    private void AlertCommand(BasePlayer player, string c, string[] a);
    private readonly JsonSerializerSettings _jsonSettings;
    private static SoReport _instance;
    private readonly Dictionary<string, string> _headers;
    private class FancyMessage
    {
        [JsonProperty("content")]
        private string Content { get; set; }
        [JsonProperty("tts")]
        private bool TextToSpeech { get; set; }
        [JsonProperty("embeds")]
        private EmbedBuilder[] Embeds { get; set; }
        public FancyMessage WithContent(string value);
        public FancyMessage SetEmbed(EmbedBuilder value);
        public string ToJson();
    }

    public class EmbedBuilder
    {
        public EmbedBuilder();
        [JsonProperty("title")]
        private string Title { get; set; }
        [JsonProperty("color")]
        private int Color { get; set; }
        [JsonProperty("fields")]
        private List<Field> Fields { get; set; }
        public EmbedBuilder WithTitle(string title);
        public EmbedBuilder SetColor(int color);
        public EmbedBuilder AddField(Field field);
        public class Field
        {
            public Field(string name, object value);
            [JsonProperty("name")]
            public string Name { get; set; }
            [JsonProperty("value")]
            public object Value { get; set; }
        }

    }

    private class Request
    {
        private readonly string _payload;
        private readonly Plugin _plugin;
        private readonly string _url;
        public void Send();
        public static void Send(string url, FancyMessage message, Plugin plugin);
        private Request(string url, FancyMessage message, Plugin plugin);
    }

    private void SendDiscord(string type, string reason, string hook);
    private Dictionary<ulong, ulong> _alertList;
    private Dictionary<ulong, List<ulong>> _reportList;
    private void OnPlayerConnected(BasePlayer player);
    private void OnServerInitialized();
    private void Unload();
    private static string Format(int units, string form1, string form2, string form3);
    public static string FormatTime(TimeSpan time, string language, int maxSubstr);
    [PluginReference]
    private Plugin ImageLibrary;
    public string GetImage(string shortname, ulong skin);
    private static string HexToRustFormat(string hex);
    private void ReplySend(BasePlayer player, string message);
    private static DateTime epoch;
    private static double CurrentTime();
}

private class ConfigData
{
    [JsonProperty("Название сервера")]
    public string Lable;
    [JsonProperty("Включить дискорд?")]
    public bool discord;
    [JsonProperty("Dircord Report WebHook")]
    public string discordhook;
    [JsonProperty("Dircord PlayerSayDiscord WebHook")]
    public string discordhook2;
    [JsonProperty("Отправлять ли уведомления в телеграмм ?")]
    public bool TelegramUse;
    [JsonProperty("Название чата(Пригласить своего бота в чат)")]
    public string chatid;
    [JsonProperty("Создать своего бота через BotFather и скопировать сюда токен")]
    public string botToken;
    [JsonProperty("Отправлять ли уведомления в вконтакте ?")]
    public bool Vkontakte;
    [JsonProperty("Вк админа")]
    public string vkadmin;
    [JsonProperty("VK Token группы")]
    public string vkAcces;
    [JsonProperty("Cooldown")]
    public int cooldown;
    [JsonProperty("Причина 1")]
    public string res1;
    [JsonProperty("Причина 2")]
    public string res2;
    [JsonProperty("Причина 3")]
    public string res3;
    [JsonProperty("Причина 4")]
    public string res4;
    [JsonProperty("Кол-во репортов для появление в панели модератора")]
    public int kolreport;
    [JsonProperty("Кол-во репортов для подсветки красный в панели модератора")]
    public int redcolor;
    [JsonProperty("Кол-во проверки для подсветки зеленым в панели модератора")]
    public int greencolor;
    public static ConfigData GetNewConf();
}

 class PlayerData
{
    public string UserName;
    public int ReportCount;
    public int AlertCount;
    public double ReportCD;
    public double IsCooldown { get; set; }
}

private class FancyMessage
{
    [JsonProperty("content")]
    private string Content { get; set; }
    [JsonProperty("tts")]
    private bool TextToSpeech { get; set; }
    [JsonProperty("embeds")]
    private EmbedBuilder[] Embeds { get; set; }
    public FancyMessage WithContent(string value);
    public FancyMessage SetEmbed(EmbedBuilder value);
    public string ToJson();
}

public class EmbedBuilder
{
    public EmbedBuilder();
    [JsonProperty("title")]
    private string Title { get; set; }
    [JsonProperty("color")]
    private int Color { get; set; }
    [JsonProperty("fields")]
    private List<Field> Fields { get; set; }
    public EmbedBuilder WithTitle(string title);
    public EmbedBuilder SetColor(int color);
    public EmbedBuilder AddField(Field field);
    public class Field
    {
        public Field(string name, object value);
        [JsonProperty("name")]
        public string Name { get; set; }
        [JsonProperty("value")]
        public object Value { get; set; }
    }

}

public class Field
{
    public Field(string name, object value);
    [JsonProperty("name")]
    public string Name { get; set; }
    [JsonProperty("value")]
    public object Value { get; set; }
}

private class Request
{
    private readonly string _payload;
    private readonly Plugin _plugin;
    private readonly string _url;
    public void Send();
    public static void Send(string url, FancyMessage message, Plugin plugin);
    private Request(string url, FancyMessage message, Plugin plugin);
}


```

---

## SortItems

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SortItems", "Own3r", "1.0.1")]
 class SortItems : RustPlugin
{
    const string permAccess;
     List<string> toggleCaptions;
     List<ulong> putPlayers;
    [ConsoleCommand("sortitems.toggle")]
     void cmdToggle(ConsoleSystem.Arg arg);
    [ConsoleCommand("sortitems.sort")]
     void cmdSort(ConsoleSystem.Arg arg);
    [PluginReference]
     Plugin Duel;
     bool InDuel(BasePlayer player);
     void Unload();
     void OnPlayerSleepEnded(PlayerLoot inventory, BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     void OnServerInitialized();
     void OnLootEntity(BasePlayer player, BaseEntity entry);
     void OnPlayerLootEnd(PlayerLoot inventory);
     List<Item> GetItemsByCategory(ItemContainer container, int category);
     bool HasAccess(BasePlayer player);
     string HandleArgs(string json, object[] args);
     string GUI;
     void DrawUI(BasePlayer player);
     void DestroyUI(BasePlayer player);
}


```

---

## SpawnControl

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using System.Reflection;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Configuration;
using Rust;

Oxide.Plugins
[Info("SpawnControl", "FuJiCuRa", "1.6.5", ResourceId = 18)]
[Description("Provides complete control over the population of animals, ore's, tree's, junkpiles and all others")]
internal class SpawnControl : RustPlugin
{
    private bool Changed;
    private bool _loaded;
    private bool isAtStartup;
    private bool onTerrainCalled;
    private bool heartbeatOn;
    private double lastMinute;
    private bool newSave;
    private int versionMajor;
    private int versionMinor;
    private bool _newConfig;
    private Dictionary<string, object> spawnPopulations;
    private Dictionary<string, object> spawnGroups;
    private Dictionary<string, object> spawnDefaults;
    private Dictionary<string, object> spawnPrefabDefaults;
    private List<string> PopulationNames;
    private Dictionary<string, ConvarControlledSpawnPopulation> convarCommands;
    private Dictionary<string, string> populationConvars;
    private Dictionary<string, int> fillPopulations;
    private Dictionary<string, int> fillJobs;
    private DynamicConfigFile defaultSpawnPops;
    private DynamicConfigFile getFile(string file);
    private bool chkFile(string file);
    private FieldInfo _numToSpawn;
    private Coroutine _groupKill;
    private Coroutine _groupFill;
    private Coroutine _spawnKill;
    private Coroutine _spawnFill;
    private Coroutine _enforceLimits;
    private Coroutine _spawnLowerDensity;
    private Coroutine _spawnRaiseDensity;
    private bool normalizeDefaultVariables;
    private bool reloadWithIncludedFill;
    private bool logJobsToConsole;
    private bool fixBarricadeStacking;
    private bool fillAtEveryStartup;
    private float tickInterval;
    private int minSpawnsPerTick;
    private int maxSpawnsPerTick;
    private SpawnPopulation[] AllSpawnPopulations;
    private SpawnDistribution[] SpawnDistributions;
    private List<string> killAllProtected;
    private bool currentTickStatus;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void Init();
    private void OnNewSave(string strFilename);
    private void Unload();
    private void UnloadCoRoutines();
    private void OnTerrainInitialized();
    private void OnServerInitialized();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private void OnTick();
    private void PauseTick();
    private void ResumeTick();
    private void ReloadAnimals();
    private void SetAnimal(string name, float value);
    private void SaveDefaultsToFile();
    private void LoadDefaultsFromFile();
    private void GetPopulationNames();
    private void GetSpawnPopulationDefaults();
    private object GetSpawnFilter(SpawnFilter filter, SpawnPopulation population);
    private SpawnFilter SetSpawnFilter(Dictionary<string, object> values, SpawnPopulation population);
    private List<string> GetTerrainTopologies(int value);
    [ConsoleCommand("sc.topologyget")]
    private void GetTopology(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.topologylist")]
    private void ListTopology(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.topologycreate")]
    private void SetTopology(ConsoleSystem.Arg arg);
    private void LoadSpawnPopulations();
    private void GetSpawnGroupDefaults();
    private void LoadSpawnGroups();
    [ConsoleCommand("sc.cleardata")]
    private void ccmdClearData(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.tickinterval")]
    private void ccmdTickInterval(ConsoleSystem.Arg arg);
    private void SetTickConvars(bool status);
    [ConsoleCommand("sc.spawnticktoggle")]
    private void ccmdToggleSpawnTick(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.fillpopulation")]
    private void ccmdSpawnFill(ConsoleSystem.Arg arg);
    private IEnumerator SpawnFill(string populationname, bool single, ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.raisedensity")]
    private void ccmdSpawnRaiseDensity(ConsoleSystem.Arg arg);
    private IEnumerator SpawnRaiseDensity(float percentage, string populationname, bool raiseAll, ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.lowerdensity")]
    private void ccmdSpawnLowerDensity(ConsoleSystem.Arg arg);
    private IEnumerator SpawnLowerDensity(float percentage, string populationname, bool lowerAll, ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.killpopulation")]
    private void ccmdSpawnKill(ConsoleSystem.Arg arg);
    private IEnumerator SpawnKill(string populationname, bool killAll, ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.killgroups")]
    private void ccmdGroupKill(ConsoleSystem.Arg arg);
    private IEnumerator GroupKill(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.fillgroups")]
    private void ccmdGroupFill(ConsoleSystem.Arg arg);
    private IEnumerator GroupFill(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.enforcelimits")]
    private void ccmdEnforceLimits(ConsoleSystem.Arg arg);
    private IEnumerator EnforceLimits(bool forceAll, ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.reloadconfig")]
    private void ccmdSpawnReload(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.populationreport")]
    private void ccmdGetPopulationReport(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.populationsettings")]
    private void ccmdGetPopulations(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.restorepopulationdensities")]
    private void ccmdRestoreDensities(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.restorepopulationweights")]
    private void ccmdRestoreWeights(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.groupsettings")]
    private void ccmdGetGroups(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.dumpresources")]
    private void ccmdDumpResources(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.dumploot")]
    private void ccmdDumpLoot(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.cmds")]
    private void ccmdComandsList(ConsoleSystem.Arg arg);
    private Dictionary<string, int> GetCollectibles();
    private Dictionary<string, IGrouping<string, BaseEntity>> GetOreNodes();
    private Dictionary<string, IGrouping<string, BaseEntity>> GetLootContainers();
    private Dictionary<string, IGrouping<string, BaseEntity>> GetAnimals();
    private string PrefabCut(string name);
    private void FixBarricades();
}


```

---

## SpawnHeli

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Facepunch;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Spawn Heli", "SpooksAU", "3.0.0")]
[Description("Allows players to spawn helicopters")]
internal class SpawnHeli : CovalencePlugin
{
    private const string LegacyPluginName;
    private const string LegacyPermissionPrefix;
    private const string PermissionMinicopter;
    private const string PermissionScrapHeli;
    private const string PermissionAttackHeli;
    private const int SpawnPointLayerMask;
    private const int SpaceCheckLayerMask;
    private const float VerticalSpawnOffset;
    private SaveData _data;
    private Configuration _config;
    private readonly VehicleInfoManager _vehicleInfoManager;
    private readonly object True;
    private readonly object False;
    public SpawnHeli();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnNewSave();
    private void OnEntityKill(PlayerHelicopter heli);
    private object OnEntityTakeDamage(PlayerHelicopter heli, HitInfo info);
    private object CanMountEntity(BasePlayer player, BaseVehicleMountPoint mountPoint);
    private void OnPlayerDisconnected(BasePlayer player);
    private void ScheduleDespawnVehicleIfUnmounted(PlayerHelicopter heli);
    private void OnEntityDismounted(BaseVehicleSeat seat);
    private void CanLootEntity(BasePlayer player, StorageContainer container);
    private void CommandSpawnMinicopter(IPlayer player, string cmd, string[] args);
    private void CommandSpawnScrapTransportHelicopter(IPlayer player, string cmd, string[] args);
    private void CommandSpawnAttackHelicopter(IPlayer player, string cmd, string[] args);
    private void SpawnCommandInternal(VehicleInfo vehicleInfo, IPlayer player, string command, string[] args);
    private void CommandFetchMinicopter(IPlayer player, string cmd, string[] args);
    private void CommandFetchScrapTransportHelicopter(IPlayer player, string cmd, string[] args);
    private void CommandFetchAttackHelicopter(IPlayer player, string cmd, string[] args);
    private void FetchCommandInternal(VehicleInfo vehicleInfo, IPlayer player, string command, string[] args);
    private void CommandDespawnMinicopter(IPlayer player, string cmd, string[] args);
    private void CommandDespawnScrapTransportHelicopter(IPlayer player, string cmd, string[] args);
    private void CommandDespawnAttackHelicopter(IPlayer player, string cmd, string[] args);
    private void DespawnCommandInternal(VehicleInfo vehicleInfo, IPlayer player, string command, string[] args);
    [Command("spawnmini.give")]
    private void CommandGiveMinicopter(IPlayer player, string cmd, string[] args);
    private void CommandGiveScrapTransportHelicopter(IPlayer player, string cmd, string[] args);
    private void CommandGiveAttackHelicopter(IPlayer player, string cmd, string[] args);
    private void GiveCommandInternal(VehicleInfo vehicleInfo, IPlayer player, string cmd, string[] args);
    private static class StringUtils
    {
        public static string StripPrefix(string subject, string prefix);
        public static string StripPrefixes(string subject, string[] prefixes);
    }

    public static void LogWarning(string message);
    public static void LogError(string message);
    private static bool VerifyPlayer(IPlayer player, BasePlayer basePlayer);
    private bool VerifyPermission(IPlayer player, string perm);
    private bool VerifyVehicleExists(IPlayer player, BasePlayer basePlayer, VehicleInfo vehicleInfo, PlayerHelicopter heli);
    private bool VerifyVehicleWithinDistance(IPlayer player, BasePlayer basePlayer, PlayerHelicopter heli, float maxDistance);
    private bool VerifyValidSpawnOrFetchPosition(VehicleInfo vehicleInfo, BasePlayer player, Vector3 position, Quaternion rotation);
    private bool VerifyOffCooldown(VehicleInfo vehicleInfo, BasePlayer player, CooldownConfig cooldownConfig, Dictionary<string, DateTime> cooldownMap);
    private bool VerifyNotBuildingBlocked(IPlayer player, BasePlayer basePlayer);
    private static bool SpawnWasBlocked(VehicleInfo vehicleInfo, BasePlayer player);
    private static bool FetchWasBlocked(VehicleInfo vehicleInfo, BasePlayer player, PlayerHelicopter heli);
    private static bool DespawnWasBlocked(VehicleInfo vehicleInfo, BasePlayer player, PlayerHelicopter heli);
    private static TimeSpan CeilingTimeSpan(TimeSpan timeSpan);
    private static Vector3 GetFixedPositionForPlayer(VehicleInfo vehicleInfo, BasePlayer player);
    private static Quaternion GetFixedRotationForPlayer(VehicleInfo vehicleInfo, BasePlayer player);
    private static void EnableUnlimitedFuel(PlayerHelicopter heli);
    private static bool AnyParentedPlayers(PlayerHelicopter heli);
    private static bool IsHeliOccupied(PlayerHelicopter heli);
    private static void UnparentPlayers(PlayerHelicopter heli);
    private bool IsPlayerVehicle(PlayerHelicopter heli, VehicleInfo vehicleInfo);
    private PlayerHelicopter FindPlayerVehicle(VehicleInfo vehicleInfo, BasePlayer player);
    private void FetchVehicle(VehicleInfo vehicleInfo, IPlayer player, BasePlayer basePlayer, PlayerHelicopter heli);
    private PlayerHelicopter SpawnVehicle(VehicleInfo vehicleInfo, BasePlayer player, Vector3 position, Quaternion rotation);
    private void GiveVehicle(VehicleInfo vehicleInfo, BasePlayer player, Vector3? customPosition);
    private bool TryDespawnHeli(VehicleInfo vehicleInfo, PlayerHelicopter heli, BasePlayer basePlayer);
    private float GetPlayerCooldownSeconds(CooldownConfig cooldownConfig, BasePlayer player);
    private int GetPlayerAllowedFuel(VehicleInfo vehicleInfo, BasePlayer player);
    private void AddInitialFuel(VehicleInfo vehicleInfo, PlayerHelicopter heli, BasePlayer player);
    private class VehicleInfo
    {
        public class PermissionSet
        {
            public string Spawn;
            public string Fetch;
            public string Despawn;
            public string UnlimitedFuel;
            public string NoDecay;
            public string NoCooldown;
        }

        public class HookSet
        {
            public string Spawn;
            public string Fetch;
            public string Despawn;
        }

        public class MessageSet
        {
            public LangEntry Destroyed;
            public LangEntry AlreadySpawned;
            public LangEntry NotFound;
        }

        public string VehicleName { get; set; }
        public string PrefabPath;
        public Bounds Bounds;
        public string GiveCommand { get; set; }
        public VehicleConfig Config;
        public VehicleData Data;
        public uint PrefabId { get; set; }
        public PermissionSet Permissions;
        public HookSet Hooks;
        public MessageSet Messages;
        public void Init(SpawnHeli plugin);
        public void OnServerInitialized();
    }

    private class VehicleInfoManager
    {
        public VehicleInfo Minicopter { get; set; }
        public VehicleInfo ScrapTransportHelicopter { get; set; }
        public VehicleInfo AttackHelicopter { get; set; }
        public VehicleInfo[] AllVehicles { get; set; }
        private readonly SpawnHeli _plugin;
        private readonly Dictionary<uint, VehicleInfo> _prefabIdToVehicleInfo;
        public bool AnyOwnerOnly { get; set; }
        public bool AnyDespawnOnDisconnect { get; set; }
        private Configuration _config { get; set; }
        private SaveData _data { get; set; }
        public VehicleInfoManager(SpawnHeli plugin);
        public void Init();
        public void OnServerInitialized();
        public VehicleInfo GetVehicleInfo(BaseEntity entity);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class LegacySaveData
    {
        private const string Filename;
        public static LegacySaveData LoadIfExists();
        [JsonProperty("playerMini")]
        public Dictionary<string, ulong> playerMini;
        [JsonProperty("spawnCooldowns")]
        public Dictionary<string, DateTime> spawnCooldowns;
        [JsonProperty("cooldown")]
        private Dictionary<string, DateTime> deprecatedCooldown { get; set; }
        [JsonProperty("fetchCooldowns")]
        public Dictionary<string, DateTime> fetchCooldowns;
        public void Delete();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class VehicleData
    {
        [JsonProperty("Vehicles")]
        public Dictionary<string, ulong> Vehicles;
        [JsonProperty("SpawnCooldowns")]
        public Dictionary<string, DateTime> SpawnCooldowns;
        [JsonProperty("FetchCooldowns")]
        public Dictionary<string, DateTime> FetchCooldowns;
        public PlayerHelicopter GetVehicle(string playerId);
        public bool HasVehicle(BaseEntity vehicle);
        public bool RegisterVehicle(string playerId, ulong netId);
        public bool UnregisterVehicle(string playerId);
        public void SetSpawnCooldown(string playerId, DateTime dateTime);
        public void SetFetchCooldown(string playerId, DateTime dateTime);
        public bool Clean();
        public bool Reset();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class SaveData
    {
        private const string Filename;
        public static SaveData Load();
        private bool _dirty;
        [JsonProperty("Minicopter")]
        public VehicleData Minicopter;
        [JsonProperty("ScrapTransportHelicopter")]
        public VehicleData ScrapTransportHelicopter;
        [JsonProperty("AttackHelicopter")]
        public VehicleData AttackHelicopter;
        public void Clean();
        public void Reset();
        public void SaveIfChanged();
        public void StartSpawnCooldown(VehicleInfo vehicleInfo, BasePlayer player);
        public void StartFetchCooldown(VehicleInfo vehicleInfo, BasePlayer player);
        public void RegisterVehicle(VehicleInfo vehicleInfo, string playerId, PlayerHelicopter heli);
        public void UnregisterVehicle(VehicleInfo vehicleInfo, string playerId);
        public void RemoveCooldown(Dictionary<string, DateTime> cooldownMap, BasePlayer player);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class BasePermissionAmount
    {
        [JsonProperty("Permission suffix")]
        protected string PermissionSuffix;
        [JsonIgnore]
        public string Permission { get; set; }
        public void Init(string permissionInfix);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class FuelProfile : BasePermissionAmount
    {
        [JsonProperty("Fuel amount")]
        public int FuelAmount;
        public FuelProfile();
        public FuelProfile(string permissionSuffix, int fuelAmount);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class FuelConfig
    {
        [JsonProperty("Default fuel amount")]
        public int DefaultFuelAmount;
        [JsonProperty("Fuel profiles requiring permission")]
        public FuelProfile[] FuelProfiles;
        public void Init(string vehicleName);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class CooldownProfile : BasePermissionAmount
    {
        [JsonProperty("Cooldown (seconds)")]
        public float CooldownSeconds;
        public CooldownProfile();
        public CooldownProfile(string permissionSuffix, float cooldownSeconds);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class CooldownConfig
    {
        [JsonProperty("Default cooldown (seconds)")]
        public float DefaultCooldown;
        [JsonProperty("Cooldown profiles requiring permission")]
        public CooldownProfile[] CooldownProfiles;
        public void Init(string vehicleName, string cooldownType);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class FixedSpawnDistanceConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Distance from player")]
        public float Distance;
        [JsonProperty("Helicopter rotation angle")]
        public float RotationAngle;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class VehicleConfig
    {
        [JsonProperty("Spawn commands")]
        public string[] SpawnCommands;
        [JsonProperty("Fetch commands")]
        public string[] FetchCommands;
        [JsonProperty("Despawn commands")]
        public string[] DespawnCommands;
        [JsonProperty("Can despawn while occupied")]
        public bool CanDespawnWhileOccupied;
        [JsonProperty("Can fetch while occupied")]
        public bool CanFetchWhileOccupied;
        [JsonProperty("Can spawn while building blocked")]
        public bool CanSpawnBuildingBlocked;
        [JsonProperty("Can fetch while building blocked")]
        public bool CanFetchBuildingBlocked;
        [JsonProperty("Auto fetch")]
        public bool AutoFetch;
        [JsonProperty("Repair on fetch")]
        public bool RepairOnFetch;
        [JsonProperty("Max spawn distance")]
        public float MaxSpawnDistance;
        [JsonProperty("Max fetch distance")]
        public float MaxFetchDistance;
        [JsonProperty("Max despawn distance")]
        public float MaxDespawnDistance;
        [JsonProperty("Fixed spawn distance")]
        public FixedSpawnDistanceConfig FixedSpawnDistanceConfig;
        [JsonProperty("Only owner and team can mount")]
        public bool OnlyOwnerAndTeamCanMount;
        [JsonProperty("Spawn health")]
        public float SpawnHealth;
        [JsonProperty("Destroy on disconnect")]
        public bool DespawnOnDisconnect;
        [JsonProperty("Fuel")]
        public FuelConfig FuelConfig;
        [JsonProperty("Spawn cooldowns")]
        public CooldownConfig SpawnCooldowns;
        [JsonProperty("Fetch cooldowns")]
        public CooldownConfig FetchCooldowns;
        public void Init(string vehicleName);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Limit players to one helicopter type at a time")]
        public bool LimitPlayersToOneHelicopterType;
        [JsonProperty("Try to auto despawn other helicopter types")]
        public bool AutoDespawnOtherHelicopterTypes;
        [JsonProperty("Minicopter")]
        public VehicleConfig Minicopter;
        [JsonProperty("ScrapTransportHelicopter")]
        public VehicleConfig ScrapTransportHelicopter;
        [JsonProperty("AttackHelicopter")]
        public VehicleConfig AttackHelicopter;
        public bool Migrate();
        [JsonProperty("CanDespawnWhileOccupied", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedCanDespawnWhileOccupied;
        [JsonProperty("CanFetchWhileOccupied", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedCanFetchWhileOccupied;
        [JsonProperty("CanSpawnBuildingBlocked", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedCanSpawnBuildingBlocked;
        [JsonProperty("CanFetchBuildingBlocked", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(true)]
        private bool DeprecatedCanFetchBuildingBlocked;
        [JsonProperty("AutoFetch", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedAutoFetch;
        [JsonProperty("RepairOnFetch", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedRepairOnFetch;
        [JsonProperty("FuelAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private int DeprecatedFuelAmount;
        [JsonProperty("FuelAmountsRequiringPermission", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private int[] DeprecatedFuelAmountsRequiringPermission;
        [JsonProperty("MaxNoMiniDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float DeprecatedNoMiniDistance;
        [JsonProperty("MaxSpawnDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float DeprecatedMaxSpawnDistance;
        [JsonProperty("UseFixedSpawnDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(true)]
        private bool DeprecatedUseFixedSpawnDistance;
        [JsonProperty("FixedSpawnDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float DeprecatedFixedSpawnDistance;
        [JsonProperty("FixedSpawnRotationAngle", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float DeprecatedFixedSpawnRotationAngle;
        [JsonProperty("OwnerAndTeamCanMount", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedOwnerOnly;
        [JsonProperty("DefaultSpawnCooldown", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float DeprecatedDefaultSpawnCooldown;
        [JsonProperty("PermissionSpawnCooldowns", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private Dictionary<string, float> DeprecatedSpawnPermissionCooldowns;
        [JsonProperty("DefaultFetchCooldown", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float DeprecatedDefaultFetchCooldown;
        [JsonProperty("PermissionFetchCooldowns", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private Dictionary<string, float> DeprecatedFetchPermissionCooldowns;
        [JsonProperty("SpawnHealth", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float DeprecatedSpawnHealth;
        [JsonProperty("DestroyOnDisconnect", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedDespawnOnDisconnect;
        public void Init();
    }

    private Configuration GetDefaultConfig();
    internal class SerializableConfiguration
    {
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    internal static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class LangEntry
    {
        public static readonly List<LangEntry> AllLangEntries;
        public static readonly LangEntry ErrorNoPermission;
        public static readonly LangEntry ErrorBuildingBlocked;
        public static readonly LangEntry ErrorOnCooldown;
        public static readonly LangEntry InsufficientSpace;
        public static readonly LangEntry ErrorConflictingHeli;
        public static readonly LangEntry ErrorSpawnDistance;
        public static readonly LangEntry ErrorNoSpawnLocationFound;
        public static readonly LangEntry ErrorHeliOccupied;
        public static readonly LangEntry ErrorHeliDistance;
        public static readonly LangEntry ErrorCannotMount;
        public static readonly LangEntry ErrorUnlimitedFuel;
        public static readonly LangEntry MiniDestroyed;
        public static readonly LangEntry ScrapHeliDestroyed;
        public static readonly LangEntry AttackHeliDestroyed;
        public static readonly LangEntry ErrorMiniExists;
        public static readonly LangEntry ErrorScrapHeliExist;
        public static readonly LangEntry ErrorAttackHeliExists;
        public static readonly LangEntry ErrorMiniNotFound;
        public static readonly LangEntry ErrorScrapHeliNotFound;
        public static readonly LangEntry ErrorAttackHeliNotFound;
        public readonly string Name;
        public readonly Dictionary<Lang, string> PhrasesByLanguage;
        private LangEntry(string name, Dictionary<Lang, string> phrasesByLanguage);
    }

    private string GetMessage(string playerId, LangEntry langEntry);
    private string GetMessage(string playerId, LangEntry langEntry, object arg1);
    protected override void LoadDefaultMessages();
}

private static class StringUtils
{
    public static string StripPrefix(string subject, string prefix);
    public static string StripPrefixes(string subject, string[] prefixes);
}

private class VehicleInfo
{
    public class PermissionSet
    {
        public string Spawn;
        public string Fetch;
        public string Despawn;
        public string UnlimitedFuel;
        public string NoDecay;
        public string NoCooldown;
    }

    public class HookSet
    {
        public string Spawn;
        public string Fetch;
        public string Despawn;
    }

    public class MessageSet
    {
        public LangEntry Destroyed;
        public LangEntry AlreadySpawned;
        public LangEntry NotFound;
    }

    public string VehicleName { get; set; }
    public string PrefabPath;
    public Bounds Bounds;
    public string GiveCommand { get; set; }
    public VehicleConfig Config;
    public VehicleData Data;
    public uint PrefabId { get; set; }
    public PermissionSet Permissions;
    public HookSet Hooks;
    public MessageSet Messages;
    public void Init(SpawnHeli plugin);
    public void OnServerInitialized();
}

public class PermissionSet
{
    public string Spawn;
    public string Fetch;
    public string Despawn;
    public string UnlimitedFuel;
    public string NoDecay;
    public string NoCooldown;
}

public class HookSet
{
    public string Spawn;
    public string Fetch;
    public string Despawn;
}

public class MessageSet
{
    public LangEntry Destroyed;
    public LangEntry AlreadySpawned;
    public LangEntry NotFound;
}

private class VehicleInfoManager
{
    public VehicleInfo Minicopter { get; set; }
    public VehicleInfo ScrapTransportHelicopter { get; set; }
    public VehicleInfo AttackHelicopter { get; set; }
    public VehicleInfo[] AllVehicles { get; set; }
    private readonly SpawnHeli _plugin;
    private readonly Dictionary<uint, VehicleInfo> _prefabIdToVehicleInfo;
    public bool AnyOwnerOnly { get; set; }
    public bool AnyDespawnOnDisconnect { get; set; }
    private Configuration _config { get; set; }
    private SaveData _data { get; set; }
    public VehicleInfoManager(SpawnHeli plugin);
    public void Init();
    public void OnServerInitialized();
    public VehicleInfo GetVehicleInfo(BaseEntity entity);
}

[JsonObject(MemberSerialization.OptIn)]
private class LegacySaveData
{
    private const string Filename;
    public static LegacySaveData LoadIfExists();
    [JsonProperty("playerMini")]
    public Dictionary<string, ulong> playerMini;
    [JsonProperty("spawnCooldowns")]
    public Dictionary<string, DateTime> spawnCooldowns;
    [JsonProperty("cooldown")]
    private Dictionary<string, DateTime> deprecatedCooldown { get; set; }
    [JsonProperty("fetchCooldowns")]
    public Dictionary<string, DateTime> fetchCooldowns;
    public void Delete();
}

[JsonObject(MemberSerialization.OptIn)]
private class VehicleData
{
    [JsonProperty("Vehicles")]
    public Dictionary<string, ulong> Vehicles;
    [JsonProperty("SpawnCooldowns")]
    public Dictionary<string, DateTime> SpawnCooldowns;
    [JsonProperty("FetchCooldowns")]
    public Dictionary<string, DateTime> FetchCooldowns;
    public PlayerHelicopter GetVehicle(string playerId);
    public bool HasVehicle(BaseEntity vehicle);
    public bool RegisterVehicle(string playerId, ulong netId);
    public bool UnregisterVehicle(string playerId);
    public void SetSpawnCooldown(string playerId, DateTime dateTime);
    public void SetFetchCooldown(string playerId, DateTime dateTime);
    public bool Clean();
    public bool Reset();
}

[JsonObject(MemberSerialization.OptIn)]
private class SaveData
{
    private const string Filename;
    public static SaveData Load();
    private bool _dirty;
    [JsonProperty("Minicopter")]
    public VehicleData Minicopter;
    [JsonProperty("ScrapTransportHelicopter")]
    public VehicleData ScrapTransportHelicopter;
    [JsonProperty("AttackHelicopter")]
    public VehicleData AttackHelicopter;
    public void Clean();
    public void Reset();
    public void SaveIfChanged();
    public void StartSpawnCooldown(VehicleInfo vehicleInfo, BasePlayer player);
    public void StartFetchCooldown(VehicleInfo vehicleInfo, BasePlayer player);
    public void RegisterVehicle(VehicleInfo vehicleInfo, string playerId, PlayerHelicopter heli);
    public void UnregisterVehicle(VehicleInfo vehicleInfo, string playerId);
    public void RemoveCooldown(Dictionary<string, DateTime> cooldownMap, BasePlayer player);
}

[JsonObject(MemberSerialization.OptIn)]
private class BasePermissionAmount
{
    [JsonProperty("Permission suffix")]
    protected string PermissionSuffix;
    [JsonIgnore]
    public string Permission { get; set; }
    public void Init(string permissionInfix);
}

[JsonObject(MemberSerialization.OptIn)]
private class FuelProfile : BasePermissionAmount
{
    [JsonProperty("Fuel amount")]
    public int FuelAmount;
    public FuelProfile();
    public FuelProfile(string permissionSuffix, int fuelAmount);
}

[JsonObject(MemberSerialization.OptIn)]
private class FuelConfig
{
    [JsonProperty("Default fuel amount")]
    public int DefaultFuelAmount;
    [JsonProperty("Fuel profiles requiring permission")]
    public FuelProfile[] FuelProfiles;
    public void Init(string vehicleName);
}

[JsonObject(MemberSerialization.OptIn)]
private class CooldownProfile : BasePermissionAmount
{
    [JsonProperty("Cooldown (seconds)")]
    public float CooldownSeconds;
    public CooldownProfile();
    public CooldownProfile(string permissionSuffix, float cooldownSeconds);
}

[JsonObject(MemberSerialization.OptIn)]
private class CooldownConfig
{
    [JsonProperty("Default cooldown (seconds)")]
    public float DefaultCooldown;
    [JsonProperty("Cooldown profiles requiring permission")]
    public CooldownProfile[] CooldownProfiles;
    public void Init(string vehicleName, string cooldownType);
}

[JsonObject(MemberSerialization.OptIn)]
private class FixedSpawnDistanceConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Distance from player")]
    public float Distance;
    [JsonProperty("Helicopter rotation angle")]
    public float RotationAngle;
}

[JsonObject(MemberSerialization.OptIn)]
private class VehicleConfig
{
    [JsonProperty("Spawn commands")]
    public string[] SpawnCommands;
    [JsonProperty("Fetch commands")]
    public string[] FetchCommands;
    [JsonProperty("Despawn commands")]
    public string[] DespawnCommands;
    [JsonProperty("Can despawn while occupied")]
    public bool CanDespawnWhileOccupied;
    [JsonProperty("Can fetch while occupied")]
    public bool CanFetchWhileOccupied;
    [JsonProperty("Can spawn while building blocked")]
    public bool CanSpawnBuildingBlocked;
    [JsonProperty("Can fetch while building blocked")]
    public bool CanFetchBuildingBlocked;
    [JsonProperty("Auto fetch")]
    public bool AutoFetch;
    [JsonProperty("Repair on fetch")]
    public bool RepairOnFetch;
    [JsonProperty("Max spawn distance")]
    public float MaxSpawnDistance;
    [JsonProperty("Max fetch distance")]
    public float MaxFetchDistance;
    [JsonProperty("Max despawn distance")]
    public float MaxDespawnDistance;
    [JsonProperty("Fixed spawn distance")]
    public FixedSpawnDistanceConfig FixedSpawnDistanceConfig;
    [JsonProperty("Only owner and team can mount")]
    public bool OnlyOwnerAndTeamCanMount;
    [JsonProperty("Spawn health")]
    public float SpawnHealth;
    [JsonProperty("Destroy on disconnect")]
    public bool DespawnOnDisconnect;
    [JsonProperty("Fuel")]
    public FuelConfig FuelConfig;
    [JsonProperty("Spawn cooldowns")]
    public CooldownConfig SpawnCooldowns;
    [JsonProperty("Fetch cooldowns")]
    public CooldownConfig FetchCooldowns;
    public void Init(string vehicleName);
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : SerializableConfiguration
{
    [JsonProperty("Limit players to one helicopter type at a time")]
    public bool LimitPlayersToOneHelicopterType;
    [JsonProperty("Try to auto despawn other helicopter types")]
    public bool AutoDespawnOtherHelicopterTypes;
    [JsonProperty("Minicopter")]
    public VehicleConfig Minicopter;
    [JsonProperty("ScrapTransportHelicopter")]
    public VehicleConfig ScrapTransportHelicopter;
    [JsonProperty("AttackHelicopter")]
    public VehicleConfig AttackHelicopter;
    public bool Migrate();
    [JsonProperty("CanDespawnWhileOccupied", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedCanDespawnWhileOccupied;
    [JsonProperty("CanFetchWhileOccupied", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedCanFetchWhileOccupied;
    [JsonProperty("CanSpawnBuildingBlocked", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedCanSpawnBuildingBlocked;
    [JsonProperty("CanFetchBuildingBlocked", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(true)]
    private bool DeprecatedCanFetchBuildingBlocked;
    [JsonProperty("AutoFetch", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedAutoFetch;
    [JsonProperty("RepairOnFetch", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedRepairOnFetch;
    [JsonProperty("FuelAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private int DeprecatedFuelAmount;
    [JsonProperty("FuelAmountsRequiringPermission", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private int[] DeprecatedFuelAmountsRequiringPermission;
    [JsonProperty("MaxNoMiniDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float DeprecatedNoMiniDistance;
    [JsonProperty("MaxSpawnDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float DeprecatedMaxSpawnDistance;
    [JsonProperty("UseFixedSpawnDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(true)]
    private bool DeprecatedUseFixedSpawnDistance;
    [JsonProperty("FixedSpawnDistance", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float DeprecatedFixedSpawnDistance;
    [JsonProperty("FixedSpawnRotationAngle", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float DeprecatedFixedSpawnRotationAngle;
    [JsonProperty("OwnerAndTeamCanMount", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedOwnerOnly;
    [JsonProperty("DefaultSpawnCooldown", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float DeprecatedDefaultSpawnCooldown;
    [JsonProperty("PermissionSpawnCooldowns", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private Dictionary<string, float> DeprecatedSpawnPermissionCooldowns;
    [JsonProperty("DefaultFetchCooldown", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float DeprecatedDefaultFetchCooldown;
    [JsonProperty("PermissionFetchCooldowns", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private Dictionary<string, float> DeprecatedFetchPermissionCooldowns;
    [JsonProperty("SpawnHealth", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float DeprecatedSpawnHealth;
    [JsonProperty("DestroyOnDisconnect", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedDespawnOnDisconnect;
    public void Init();
}

internal class SerializableConfiguration
{
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

internal static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private class LangEntry
{
    public static readonly List<LangEntry> AllLangEntries;
    public static readonly LangEntry ErrorNoPermission;
    public static readonly LangEntry ErrorBuildingBlocked;
    public static readonly LangEntry ErrorOnCooldown;
    public static readonly LangEntry InsufficientSpace;
    public static readonly LangEntry ErrorConflictingHeli;
    public static readonly LangEntry ErrorSpawnDistance;
    public static readonly LangEntry ErrorNoSpawnLocationFound;
    public static readonly LangEntry ErrorHeliOccupied;
    public static readonly LangEntry ErrorHeliDistance;
    public static readonly LangEntry ErrorCannotMount;
    public static readonly LangEntry ErrorUnlimitedFuel;
    public static readonly LangEntry MiniDestroyed;
    public static readonly LangEntry ScrapHeliDestroyed;
    public static readonly LangEntry AttackHeliDestroyed;
    public static readonly LangEntry ErrorMiniExists;
    public static readonly LangEntry ErrorScrapHeliExist;
    public static readonly LangEntry ErrorAttackHeliExists;
    public static readonly LangEntry ErrorMiniNotFound;
    public static readonly LangEntry ErrorScrapHeliNotFound;
    public static readonly LangEntry ErrorAttackHeliNotFound;
    public readonly string Name;
    public readonly Dictionary<Lang, string> PhrasesByLanguage;
    private LangEntry(string name, Dictionary<Lang, string> phrasesByLanguage);
}


```

---

## SpectatorPlus

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("SpectatorPlus", "https://shoprust.ru/", "0.0.4")]
 class SpectatorPlus : RustPlugin
{
    [PluginReference]
     Plugin MultiFighting;
    private static Configuration config;
    public class Configuration
    {
        [JsonProperty("Положение панели")]
        public Settings Setting;
        [JsonProperty("Настройка кнопок")]
        public List<Buttons> ButtonsSet;
        [JsonProperty("Причины бана")]
        public List<BanReason> ReasonBan;
        [JsonProperty("Другие настройки")]
        public AnotherSettings Another;
        [JsonProperty(
                "Положения кнопок с дествиями(нужно для размещения кнопок в заданном порядке) не рекомендую менять это, а то кнопки могут быть отрисованы неправильно")]
        public List<ButtonPos> Positions;
        internal class Settings
        {
            [JsonProperty("AnchorMin")]
            public string MainAnchorMin;
            [JsonProperty("AnchorMax")]
            public string MainAnchorMax;
            [JsonProperty("OffsetMin")]
            public string MainOffsetMin;
            [JsonProperty("OffsetMax")]
            public string MainOffsetMax;
        }

        internal class Buttons
        {
            [JsonProperty("Надпись на кнопке")]
            public string Title;
            [JsonProperty("Спец-Функция(если нужна просто команда оставьте поле пустым)")]
            public string Funk;
            [JsonProperty("Команда ([ID] заменяется на ID наблюдюдаемого)")]
            public string Command;
            [JsonProperty("Пермишн на отображение")]
            public string Perm;
        }

        internal class BanReason
        {
            [JsonProperty("Название")]
            public string ReasonName;
            [JsonProperty("Команда")]
            public string BanCommand;
        }

        internal class AnotherSettings
        {
            [JsonProperty("На сколько хп хилять/ранить наблюдаемого по кнопке Heal/Hurt")]
            public float HP;
        }

        internal class ButtonPos
        {
            [JsonProperty("OffsetMin")]
            public string oMin;
            [JsonProperty("OffsetMax")]
            public string oMax;
        }

        public static Configuration GetNewConfiguration();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void LoadDefaultMessages();
     void OnServerInitialized();
     string spectateLayer;
     string spectateLayerBan;
    private string spectateHPControl;
    private void Unload();
    [ChatCommand("spec")]
     void StartSpectate(BasePlayer admin, string command, string[] args);
    [ChatCommand("specstop")]
    private void SpectatingEnd(BasePlayer player);
     object OnPlayerSpectateEnd(BasePlayer player);
    private object CanSpectateTarget(BasePlayer player, string filter);
    private void DrawBlockInfo(BasePlayer player, string filter);
     void DrawButtons(BasePlayer player, CuiElementContainer container, BasePlayer currentSuspect, int i);
    [ConsoleCommand("spectatorbutton")]
     void Button(ConsoleSystem.Arg arg);
    private static string HexToRustFormat(string hex);
     string IsSteam(string suspectid);
}

public class Configuration
{
    [JsonProperty("Положение панели")]
    public Settings Setting;
    [JsonProperty("Настройка кнопок")]
    public List<Buttons> ButtonsSet;
    [JsonProperty("Причины бана")]
    public List<BanReason> ReasonBan;
    [JsonProperty("Другие настройки")]
    public AnotherSettings Another;
    [JsonProperty(
                "Положения кнопок с дествиями(нужно для размещения кнопок в заданном порядке) не рекомендую менять это, а то кнопки могут быть отрисованы неправильно")]
    public List<ButtonPos> Positions;
    internal class Settings
    {
        [JsonProperty("AnchorMin")]
        public string MainAnchorMin;
        [JsonProperty("AnchorMax")]
        public string MainAnchorMax;
        [JsonProperty("OffsetMin")]
        public string MainOffsetMin;
        [JsonProperty("OffsetMax")]
        public string MainOffsetMax;
    }

    internal class Buttons
    {
        [JsonProperty("Надпись на кнопке")]
        public string Title;
        [JsonProperty("Спец-Функция(если нужна просто команда оставьте поле пустым)")]
        public string Funk;
        [JsonProperty("Команда ([ID] заменяется на ID наблюдюдаемого)")]
        public string Command;
        [JsonProperty("Пермишн на отображение")]
        public string Perm;
    }

    internal class BanReason
    {
        [JsonProperty("Название")]
        public string ReasonName;
        [JsonProperty("Команда")]
        public string BanCommand;
    }

    internal class AnotherSettings
    {
        [JsonProperty("На сколько хп хилять/ранить наблюдаемого по кнопке Heal/Hurt")]
        public float HP;
    }

    internal class ButtonPos
    {
        [JsonProperty("OffsetMin")]
        public string oMin;
        [JsonProperty("OffsetMax")]
        public string oMax;
    }

    public static Configuration GetNewConfiguration();
}

internal class Settings
{
    [JsonProperty("AnchorMin")]
    public string MainAnchorMin;
    [JsonProperty("AnchorMax")]
    public string MainAnchorMax;
    [JsonProperty("OffsetMin")]
    public string MainOffsetMin;
    [JsonProperty("OffsetMax")]
    public string MainOffsetMax;
}

internal class Buttons
{
    [JsonProperty("Надпись на кнопке")]
    public string Title;
    [JsonProperty("Спец-Функция(если нужна просто команда оставьте поле пустым)")]
    public string Funk;
    [JsonProperty("Команда ([ID] заменяется на ID наблюдюдаемого)")]
    public string Command;
    [JsonProperty("Пермишн на отображение")]
    public string Perm;
}

internal class BanReason
{
    [JsonProperty("Название")]
    public string ReasonName;
    [JsonProperty("Команда")]
    public string BanCommand;
}

internal class AnotherSettings
{
    [JsonProperty("На сколько хп хилять/ранить наблюдаемого по кнопке Heal/Hurt")]
    public float HP;
}

internal class ButtonPos
{
    [JsonProperty("OffsetMin")]
    public string oMin;
    [JsonProperty("OffsetMax")]
    public string oMax;
}


```

---

## SQLStatistics

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System;
using System.Linq;
using Oxide.Core.Database;
using Connection = Oxide.Core.Database.Connection;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("SQLStatistics", "Visagalis", "1.0.0")]
[Description("Announces various statistics gathered by SQLStats to in-game chat.")]
public class SQLStatistics : RustPlugin
{
    private static readonly Core.MySql.Libraries.MySql _mySql;
    private static Connection _mySqlConnection;
    private Dictionary<string, object> dbConnection;
    private readonly List<Timer> listOfTimers;
    private readonly List<StatisticData> listOfStatistics;
    protected override void LoadDefaultConfig();
    internal static SQLStatistics ss;
    private int currentStatistic;
    private int getStatisticToDisplay();
    private T checkCfg(string conf, T def);
     void Unload();
     void OnServerInitialized();
     void Loaded();
     void ProcessRandomStatistic();
    private void StartConnection();
    public class StatisticData
    {
        public string query;
        public string announcementText;
        public string announcementInfo;
        public string[] fieldsToDisplay;
    }

    [ChatCommand("last")]
     void cmdChatGetLastStatistic(BasePlayer player, string command, string[] args);
    [ChatCommand("rstat")]
     void cmdChatDoNextStatistic(BasePlayer player, string command, string[] args);
    public void Announce(StatisticData data, BasePlayer player);
    public static string Beautify(object numberStr);
}

public class StatisticData
{
    public string query;
    public string announcementText;
    public string announcementInfo;
    public string[] fieldsToDisplay;
}


```

---

## SQLStats

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System;
using System.Net;
using System.Text;
using UnityEngine;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SQLStats", "Visagalis", "1.0.2")]
[Description("Logs various statistics about ingame stuff to MySQL.")]
public class SQLStats : RustPlugin
{
    private Dictionary<BasePlayer, Int32> loginTime;
    private readonly Ext.MySql.Libraries.MySql _mySql;
    private Ext.MySql.Connection _mySqlConnection;
    private Dictionary<string, object> dbConnection;
    protected override void LoadDefaultConfig();
    private T checkCfg(string conf, T def);
     void Unloaded();
     void Loaded();
    private void StartConnection();
    public void executeQuery(string query, object[] data);
    private string getDate();
    private string getDateTime();
     string UppercaseFirst(string s);
    static string EncodeNonAsciiCharacters(string value);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile itemProjectile, ProtoBuf.ProjectileShoot projectiles);
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     string GetDistance(BaseCombatEntity victim, BaseEntity attacker);
     string GetFormattedAnimal(string animal);
     string formatBodyPartName(HitInfo hitInfo);
     void OnEntityBuilt(Planner planner, GameObject component);
}


```

---

## SSkin

```csharp
using CompanionServer;
using Facepunch.Extend;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Plugins.Extension;
using Oxide.Plugins.Helper;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;
using VLB;
using Time = UnityEngine.Time;
using Player;
using Skin;

Oxide.Plugins
[Info("SSkin", "https://discord.gg/dNGbxafuJn", "1.1.3")]
internal class SSkin : RustPlugin
{
    private readonly Dictionary<ulong, BaseEntity> _startUseMenu;
    private class Configuration : Sigleton<Configuration>
    {
        public bool AddApproved;
        public bool AddHazmat;
        public string DefaultSkinsPermission;
        public Grades GradesSettings;
        public Hammer HammerSettings;
        public Command SkinCommand;
        public Command SkinEntityCommand;
        public Dictionary<string, List<ulong>> PermissionSkins;
        public class Grades
        {
            public List<BuildingGrade> Array;
            public string Permission;
        }

        public class BuildingGrade
        {
            public string Name;
            public List<SkinUrl> Skins;
        }

        public class SkinUrl : Skin.Skin
        {
            public string Url;
        }

        public class Hammer
        {
            public string OpenSkinEntityPermission;
            public BUTTON OpenSkinEntityButton;
            public BUTTON OpenSkinBuildingButton;
            public string ChangeSkinPressLeftButtonPermission;
        }

        public class Command
        {
            public List<string> Commands;
            public string Permission;
        }

        public List<ulong> GetBlockPermissionSkins(BasePlayer player);
        public List<ulong> GetPermissionSkins(BasePlayer player);
        public void SetDefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    private void CanWearItem(PlayerInventory inventory, Item item, int targetSlot);
    private void CanEquipItem(PlayerInventory inventory, Item item, int targetPos);
    private void OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
    private void OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter owner);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private object CanLootPlayer(BasePlayer looted, BasePlayer looter);
    private void OnPlayerLootEnd(PlayerLoot inventory);
    private void SkinCommand(BasePlayer player, string command, string[] args);
    private void SkinEntityCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("uiskinmenu")]
    private void ConsoleCommand(ConsoleSystem.Arg arg);
    private class MainContainer : IContainer
    {
        public string OffsetMin { get; set; }
        public string OffsetMax { get; set; }
        public PlayerInventory.Type Type { get; set; }
        public Tuple<string, string> GetOffsets(int i);
    }

    private class WearContainer : IContainer
    {
        public string OffsetMin { get; set; }
        public string OffsetMax { get; set; }
        public PlayerInventory.Type Type { get; set; }
        public Tuple<string, string> GetOffsets(int i);
    }

    private class BeltContainer : IContainer
    {
        public string OffsetMin { get; set; }
        public string OffsetMax { get; set; }
        public PlayerInventory.Type Type { get; set; }
        public Tuple<string, string> GetOffsets(int i);
    }

    private class DefaultInterface
    {
        private string _json;
        public DefaultInterface();
        public string GetElement(string lableMenuText, string infoClickSkin, string favSkinable);
    }

    private class ContainerInterface
    {
        private string _json;
        public ContainerInterface();
        public string GetElement(string offsetMin, string offsetMax, string layer);
    }

    private class ContainerItemInterface
    {
        private string _json;
        public ContainerItemInterface();
        public string GetElement(string offsetMin, string offsetMax, string layer, string shortName, ulong skinId);
    }

    private class BlockItemInterface
    {
        private string _json;
        public BlockItemInterface();
        public string GetElement(string offsetMin, string offsetMax, string layer);
    }

    private class ItemSkinInterface
    {
        private string _jsonOwnerElement;
        private string _jsonSelectebleElement;
        private string _jsonDefaultElement;
        public ItemSkinInterface();
        public string GetElement(string offsetMin, string offsetMax, string shortName, Skin.Skin skin, ulong selectItemId, string ownLayer, string layerName, int page);
        public string GetElementSelectable(string shortName, ulong selectItemId, ulong skinId, int page, string layerName, string ownLayer, bool isSelected);
        public string GetElementDefault(string shortName, ulong selectItemId, ulong skinId, int page, string layerName, string ownLayer, bool isDefault);
    }

    private class SearchSkinsInterface
    {
         string _json;
        public SearchSkinsInterface();
        public string GetElement(string offsetMax, string commandRightButton, string colorRightButton, string commandLeftButton, string colorLeftButton);
    }

    private class InterfaceBuilder : Sigleton<InterfaceBuilder>
    {
        public const string Layer;
        public const string Regular;
        private readonly List<IContainer> _wears;
        private DefaultInterface _defaultInterface;
        private ContainerInterface _containerInterface;
        private ContainerItemInterface _containerItemInterface;
        private BlockItemInterface _blockItemInterface;
        private ItemSkinInterface _itemSkinInterface;
        private SearchSkinsInterface _searchSkinsInterface;
        public void Draw(BasePlayer player, bool drawWears);
        private void LoadMainUI(BasePlayer player, IContainer iContainer);
        public void DrawSkins(BasePlayer player, string shortName, ulong selectedItemId, int page);
        private string GetItem(string offsetMin, string offsetMax, string layer, string layerParrent, string shortName, Skin.Skin skin, ulong selectedItemId, int page, bool useDefault, bool useSelectable, Player.Player player);
        public void DrawSelectSkins(BasePlayer player, string shortname, ulong selectedItemId, int page);
        public void UpdateItem(BasePlayer player, string shortName, ulong selectedItemId, ulong skinId, int page);
        public void DestroyAllUI(BasePlayer player);
    }

    private class OpenMenuButton : MonoBehaviour
    {
        private BasePlayer _player;
        private float _lastCheck;
        private void Awake();
        private void FixedUpdate();
        public void Destroy();
    }

    private void RegisterPermission(string perm);
    private class Helper : Sigleton<Helper>
    {
        private readonly Dictionary<string, string> _shortNamesEntity;
        public readonly Dictionary<string, string> WorkshopsNames;
        public Helper();
        private void UpdateWorkshopShortName();
        private void LoadShortNamesEntity();
        public string GetShortNamePrefab(string shortPrefabName);
    }

    private class ImageLibrary : PluginSigleton<ImageLibrary>
    {
        public ImageLibrary(Plugin plugin);
        public string GetImage(string shortname, ulong skin);
        public bool AddImage(string url, string shortname, ulong skin);
        public bool HasImage(string imageName, ulong imageId);
    }

    private class SSkinWeb : PluginSigleton<SSkinWeb>
    {
        private readonly WebRequests _webrequest;
        public SSkinWeb(Plugin plugin);
        public void DownloadSkin(ulong skinId);
        public class PublishedFileQueryResponse
        {
            public FileResponse response { get; set; }
        }

        public class FileResponse
        {
            public int result { get; set; }
            public int resultcount { get; set; }
            public PublishedFileQueryDetail[] publishedfiledetails { get; set; }
        }

        public class PublishedFileQueryDetail
        {
            public string publishedfileid { get; set; }
            public int result { get; set; }
            public string creator { get; set; }
            public int creator_app_id { get; set; }
            public int consumer_app_id { get; set; }
            public string filename { get; set; }
            public int file_size { get; set; }
            public string preview_url { get; set; }
            public string hcontent_preview { get; set; }
            public string title { get; set; }
            public string description { get; set; }
            public int time_created { get; set; }
            public int time_updated { get; set; }
            public int visibility { get; set; }
            public int banned { get; set; }
            public string ban_reason { get; set; }
            public int subscriptions { get; set; }
            public int favorited { get; set; }
            public int lifetime_subscriptions { get; set; }
            public int lifetime_favorited { get; set; }
            public int views { get; set; }
            public Tag[] tags { get; set; }
            public class Tag
            {
                public string tag { get; set; }
            }

        }

        public class CollectionQueryResponse
        {
            public CollectionResponse response { get; set; }
        }

        public class CollectionResponse
        {
            public int result { get; set; }
            public int resultcount { get; set; }
            public CollectionDetails[] collectiondetails { get; set; }
        }

        public class CollectionDetails
        {
            public string publishedfileid { get; set; }
            public int result { get; set; }
            public CollectionChild[] children { get; set; }
        }

        public class CollectionChild
        {
            public string publishedfileid { get; set; }
            public int sortorder { get; set; }
            public int filetype { get; set; }
        }

    }

    private class SSkinLang : PluginSigleton<SSkinLang>
    {
        protected Lang lang;
        public SSkinLang(Plugin plugin);
        public string Get(string message, string userId);
    }

}

private class Configuration : Sigleton<Configuration>
{
    public bool AddApproved;
    public bool AddHazmat;
    public string DefaultSkinsPermission;
    public Grades GradesSettings;
    public Hammer HammerSettings;
    public Command SkinCommand;
    public Command SkinEntityCommand;
    public Dictionary<string, List<ulong>> PermissionSkins;
    public class Grades
    {
        public List<BuildingGrade> Array;
        public string Permission;
    }

    public class BuildingGrade
    {
        public string Name;
        public List<SkinUrl> Skins;
    }

    public class SkinUrl : Skin.Skin
    {
        public string Url;
    }

    public class Hammer
    {
        public string OpenSkinEntityPermission;
        public BUTTON OpenSkinEntityButton;
        public BUTTON OpenSkinBuildingButton;
        public string ChangeSkinPressLeftButtonPermission;
    }

    public class Command
    {
        public List<string> Commands;
        public string Permission;
    }

    public List<ulong> GetBlockPermissionSkins(BasePlayer player);
    public List<ulong> GetPermissionSkins(BasePlayer player);
    public void SetDefaultConfig();
}

public class Grades
{
    public List<BuildingGrade> Array;
    public string Permission;
}

public class BuildingGrade
{
    public string Name;
    public List<SkinUrl> Skins;
}

public class SkinUrl : Skin.Skin
{
    public string Url;
}

public class Hammer
{
    public string OpenSkinEntityPermission;
    public BUTTON OpenSkinEntityButton;
    public BUTTON OpenSkinBuildingButton;
    public string ChangeSkinPressLeftButtonPermission;
}

public class Command
{
    public List<string> Commands;
    public string Permission;
}

private class MainContainer : IContainer
{
    public string OffsetMin { get; set; }
    public string OffsetMax { get; set; }
    public PlayerInventory.Type Type { get; set; }
    public Tuple<string, string> GetOffsets(int i);
}

private class WearContainer : IContainer
{
    public string OffsetMin { get; set; }
    public string OffsetMax { get; set; }
    public PlayerInventory.Type Type { get; set; }
    public Tuple<string, string> GetOffsets(int i);
}

private class BeltContainer : IContainer
{
    public string OffsetMin { get; set; }
    public string OffsetMax { get; set; }
    public PlayerInventory.Type Type { get; set; }
    public Tuple<string, string> GetOffsets(int i);
}

private class DefaultInterface
{
    private string _json;
    public DefaultInterface();
    public string GetElement(string lableMenuText, string infoClickSkin, string favSkinable);
}

private class ContainerInterface
{
    private string _json;
    public ContainerInterface();
    public string GetElement(string offsetMin, string offsetMax, string layer);
}

private class ContainerItemInterface
{
    private string _json;
    public ContainerItemInterface();
    public string GetElement(string offsetMin, string offsetMax, string layer, string shortName, ulong skinId);
}

private class BlockItemInterface
{
    private string _json;
    public BlockItemInterface();
    public string GetElement(string offsetMin, string offsetMax, string layer);
}

private class ItemSkinInterface
{
    private string _jsonOwnerElement;
    private string _jsonSelectebleElement;
    private string _jsonDefaultElement;
    public ItemSkinInterface();
    public string GetElement(string offsetMin, string offsetMax, string shortName, Skin.Skin skin, ulong selectItemId, string ownLayer, string layerName, int page);
    public string GetElementSelectable(string shortName, ulong selectItemId, ulong skinId, int page, string layerName, string ownLayer, bool isSelected);
    public string GetElementDefault(string shortName, ulong selectItemId, ulong skinId, int page, string layerName, string ownLayer, bool isDefault);
}

private class SearchSkinsInterface
{
     string _json;
    public SearchSkinsInterface();
    public string GetElement(string offsetMax, string commandRightButton, string colorRightButton, string commandLeftButton, string colorLeftButton);
}

private class InterfaceBuilder : Sigleton<InterfaceBuilder>
{
    public const string Layer;
    public const string Regular;
    private readonly List<IContainer> _wears;
    private DefaultInterface _defaultInterface;
    private ContainerInterface _containerInterface;
    private ContainerItemInterface _containerItemInterface;
    private BlockItemInterface _blockItemInterface;
    private ItemSkinInterface _itemSkinInterface;
    private SearchSkinsInterface _searchSkinsInterface;
    public void Draw(BasePlayer player, bool drawWears);
    private void LoadMainUI(BasePlayer player, IContainer iContainer);
    public void DrawSkins(BasePlayer player, string shortName, ulong selectedItemId, int page);
    private string GetItem(string offsetMin, string offsetMax, string layer, string layerParrent, string shortName, Skin.Skin skin, ulong selectedItemId, int page, bool useDefault, bool useSelectable, Player.Player player);
    public void DrawSelectSkins(BasePlayer player, string shortname, ulong selectedItemId, int page);
    public void UpdateItem(BasePlayer player, string shortName, ulong selectedItemId, ulong skinId, int page);
    public void DestroyAllUI(BasePlayer player);
}

private class OpenMenuButton : MonoBehaviour
{
    private BasePlayer _player;
    private float _lastCheck;
    private void Awake();
    private void FixedUpdate();
    public void Destroy();
}

private class Helper : Sigleton<Helper>
{
    private readonly Dictionary<string, string> _shortNamesEntity;
    public readonly Dictionary<string, string> WorkshopsNames;
    public Helper();
    private void UpdateWorkshopShortName();
    private void LoadShortNamesEntity();
    public string GetShortNamePrefab(string shortPrefabName);
}

private class ImageLibrary : PluginSigleton<ImageLibrary>
{
    public ImageLibrary(Plugin plugin);
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public bool HasImage(string imageName, ulong imageId);
}

private class SSkinWeb : PluginSigleton<SSkinWeb>
{
    private readonly WebRequests _webrequest;
    public SSkinWeb(Plugin plugin);
    public void DownloadSkin(ulong skinId);
    public class PublishedFileQueryResponse
    {
        public FileResponse response { get; set; }
    }

    public class FileResponse
    {
        public int result { get; set; }
        public int resultcount { get; set; }
        public PublishedFileQueryDetail[] publishedfiledetails { get; set; }
    }

    public class PublishedFileQueryDetail
    {
        public string publishedfileid { get; set; }
        public int result { get; set; }
        public string creator { get; set; }
        public int creator_app_id { get; set; }
        public int consumer_app_id { get; set; }
        public string filename { get; set; }
        public int file_size { get; set; }
        public string preview_url { get; set; }
        public string hcontent_preview { get; set; }
        public string title { get; set; }
        public string description { get; set; }
        public int time_created { get; set; }
        public int time_updated { get; set; }
        public int visibility { get; set; }
        public int banned { get; set; }
        public string ban_reason { get; set; }
        public int subscriptions { get; set; }
        public int favorited { get; set; }
        public int lifetime_subscriptions { get; set; }
        public int lifetime_favorited { get; set; }
        public int views { get; set; }
        public Tag[] tags { get; set; }
        public class Tag
        {
            public string tag { get; set; }
        }

    }

    public class CollectionQueryResponse
    {
        public CollectionResponse response { get; set; }
    }

    public class CollectionResponse
    {
        public int result { get; set; }
        public int resultcount { get; set; }
        public CollectionDetails[] collectiondetails { get; set; }
    }

    public class CollectionDetails
    {
        public string publishedfileid { get; set; }
        public int result { get; set; }
        public CollectionChild[] children { get; set; }
    }

    public class CollectionChild
    {
        public string publishedfileid { get; set; }
        public int sortorder { get; set; }
        public int filetype { get; set; }
    }

}

public class PublishedFileQueryResponse
{
    public FileResponse response { get; set; }
}

public class FileResponse
{
    public int result { get; set; }
    public int resultcount { get; set; }
    public PublishedFileQueryDetail[] publishedfiledetails { get; set; }
}

public class PublishedFileQueryDetail
{
    public string publishedfileid { get; set; }
    public int result { get; set; }
    public string creator { get; set; }
    public int creator_app_id { get; set; }
    public int consumer_app_id { get; set; }
    public string filename { get; set; }
    public int file_size { get; set; }
    public string preview_url { get; set; }
    public string hcontent_preview { get; set; }
    public string title { get; set; }
    public string description { get; set; }
    public int time_created { get; set; }
    public int time_updated { get; set; }
    public int visibility { get; set; }
    public int banned { get; set; }
    public string ban_reason { get; set; }
    public int subscriptions { get; set; }
    public int favorited { get; set; }
    public int lifetime_subscriptions { get; set; }
    public int lifetime_favorited { get; set; }
    public int views { get; set; }
    public Tag[] tags { get; set; }
    public class Tag
    {
        public string tag { get; set; }
    }

}

public class Tag
{
    public string tag { get; set; }
}

public class CollectionQueryResponse
{
    public CollectionResponse response { get; set; }
}

public class CollectionResponse
{
    public int result { get; set; }
    public int resultcount { get; set; }
    public CollectionDetails[] collectiondetails { get; set; }
}

public class CollectionDetails
{
    public string publishedfileid { get; set; }
    public int result { get; set; }
    public CollectionChild[] children { get; set; }
}

public class CollectionChild
{
    public string publishedfileid { get; set; }
    public int sortorder { get; set; }
    public int filetype { get; set; }
}

private class SSkinLang : PluginSigleton<SSkinLang>
{
    protected Lang lang;
    public SSkinLang(Plugin plugin);
    public string Get(string message, string userId);
}

public class Items : SigletonData<Items, List<Item>>
{
    public override string Name { get; set; }
    public Item Add(string name);
    public Item Get(string name);
    public Skin GetSkin(string name, ulong id);
    public void LoadSkinHazmats();
    public void LoadSkinDefines();
    public void LoadApprovedSkin();
}

public class Players : SigletonData<Players, List<Player>>
{
    public override string Name { get; set; }
    public Player Get(ulong steamId);
}

public abstract class PluginSigleton : Sigleton<T>
{
    public Plugin Plugin { get; set; }
    public PluginSigleton(Plugin plugin);
}

public abstract class Sigleton
{
    public static T Instance;
    public Sigleton();
}

public abstract class SigletonData : Sigleton<B>
{
    public T Value { get; set; }
    public abstract string Name { get; set; }
    public virtual void Load();
    public virtual void Unload();
}

public class Skin
{
    public string Name { get; set; }
    public ulong Id { get; set; }
}

public class Item
{
    public string Name { get; set; }
    public List<Skin> Skins { get; set; }
    public Skin GetSkin(ulong id);
}

public class Player
{
    public ulong SteamId { get; set; }
    public Dictionary<string, List<ulong>> SelectedSkins { get; set; }
    public Dictionary<string, ulong> DefaultSkins { get; set; }
    public void AddOrRemoveSelectedSkin(string name, ulong skinId);
}

public static class HexExtension
{
    public static string ToRustFormat(string hex);
}

public static class PlayerExtension
{
    public static void SetDefaultSkin(Player.Player player, Item item);
    public static void SetDefaultSkin(Player.Player player, BaseEntity entity);
    public static void SetDefaultSkin(Player.Player player, BuildingBlock block, BuildingGrade.Enum grade);
    public static BaseEntity FindConstructioninDirectionLook(BasePlayer player);
    public static void SetSkin(BaseEntity entity, ulong skinId);
    public static string GetShortName(BaseEntity entity);
    public static bool HasPermission(BasePlayer player, string[] perms);
    public static bool HasPermission(BasePlayer player, string perm);
    public static void LootContainer(BasePlayer player, ItemContainer container);
}

public static class ItemExtension
{
    private static readonly Dictionary<string, int> _itemIds;
    private static readonly Dictionary<ulong, string> _hazmats;
    public static IEnumerable<KeyValuePair<ulong, string>> GetHazmats();
    public static void SetItemSkin(Item item, ulong skinId);
    public static int GetItemId(string name, ulong skinId);
    public static int GetItemId(string name);
}

Helper
public class Items : SigletonData<Items, List<Item>>
{
    public override string Name { get; set; }
    public Item Add(string name);
    public Item Get(string name);
    public Skin GetSkin(string name, ulong id);
    public void LoadSkinHazmats();
    public void LoadSkinDefines();
    public void LoadApprovedSkin();
}

public class Players : SigletonData<Players, List<Player>>
{
    public override string Name { get; set; }
    public Player Get(ulong steamId);
}

public abstract class PluginSigleton : Sigleton<T>
{
    public Plugin Plugin { get; set; }
    public PluginSigleton(Plugin plugin);
}

public abstract class Sigleton
{
    public static T Instance;
    public Sigleton();
}

public abstract class SigletonData : Sigleton<B>
{
    public T Value { get; set; }
    public abstract string Name { get; set; }
    public virtual void Load();
    public virtual void Unload();
}

Skin
public class Skin
{
    public string Name { get; set; }
    public ulong Id { get; set; }
}

public class Item
{
    public string Name { get; set; }
    public List<Skin> Skins { get; set; }
    public Skin GetSkin(ulong id);
}

Player
public class Player
{
    public ulong SteamId { get; set; }
    public Dictionary<string, List<ulong>> SelectedSkins { get; set; }
    public Dictionary<string, ulong> DefaultSkins { get; set; }
    public void AddOrRemoveSelectedSkin(string name, ulong skinId);
}

Extension
public static class HexExtension
{
    public static string ToRustFormat(string hex);
}

public static class PlayerExtension
{
    public static void SetDefaultSkin(Player.Player player, Item item);
    public static void SetDefaultSkin(Player.Player player, BaseEntity entity);
    public static void SetDefaultSkin(Player.Player player, BuildingBlock block, BuildingGrade.Enum grade);
    public static BaseEntity FindConstructioninDirectionLook(BasePlayer player);
    public static void SetSkin(BaseEntity entity, ulong skinId);
    public static string GetShortName(BaseEntity entity);
    public static bool HasPermission(BasePlayer player, string[] perms);
    public static bool HasPermission(BasePlayer player, string perm);
    public static void LootContainer(BasePlayer player, ItemContainer container);
}

public static class ItemExtension
{
    private static readonly Dictionary<string, int> _itemIds;
    private static readonly Dictionary<ulong, string> _hazmats;
    public static IEnumerable<KeyValuePair<ulong, string>> GetHazmats();
    public static void SetItemSkin(Item item, ulong skinId);
    public static int GetItemId(string name, ulong skinId);
    public static int GetItemId(string name);
}


```

---

## SSNNotifier

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using System.Security.Cryptography;
using System.Text;

Oxide.Plugins
[Info("SSNNotifier", "Umlaut", "0.0.5")]
 class SSNNotifier : RustPlugin
{
     class BanItem
    {
        public string timestamp;
        public string reason;
        public BanItem();
    }

     class MuteItem
    {
        public string timestamp;
        public string reason;
        public TimeRange level;
        public DateTime untilDatetime();
         TimeSpan timeSpan();
    }

     class ConfigData
    {
        public bool print_errors;
        public string server_name;
        public string server_password;
        public Dictionary<string, string> Messages;
        public Dictionary<ulong, BanItem> BannedPlayers;
        public Dictionary<ulong, MuteItem> MutedPlayers;
    }

     ConfigData m_configData;
     WebRequests m_webRequests;
    public string m_host;
    public string m_port;
     Dictionary<ulong, string> m_playersNames;
     Dictionary<ulong, List<ulong>> m_contextPlayers;
     void LoadConfig();
     void SaveConfig();
     void LoadDynamic();
     void SaveDynamic();
    public void InsertDefaultMessage(string key, string message);
     void InsertDefaultMessages();
     void Loaded();
     void checkPermission(string _permission);
    private void Unload();
     void LoadDefaultConfig();
     object CanClientLogin(Network.Connection connection);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     object OnPlayerChat(ConsoleSystem.Arg arg);
    [ChatCommand("players")]
     void cmdChatPlayers(BasePlayer player, string command, string[] args);
    [ChatCommand("ban")]
     void cmdBan(BasePlayer player, string command, string[] args);
    [ChatCommand("unban")]
     void cmdUnban(BasePlayer player, string command, string[] args);
    [ChatCommand("bans")]
     void cmdChatBans(BasePlayer player, string command, string[] args);
    [ChatCommand("mute")]
     void cmdChatMute(BasePlayer player, string command, string[] args);
    [ChatCommand("unmute")]
     void cmdChatUnnute(BasePlayer player, string command, string[] args);
    [ChatCommand("mutes")]
     void cmdChatMutes(BasePlayer player, string command, string[] args);
    private ulong UserIdByAlias(ulong contextId, string alias);
    private void SetContextPlayers(ulong context, List<ulong> players);
    private string PlayerName(ulong userID);
    private void SendWebRequest(string subUrl, List<string> values);
    private void ReceiveWebResponse(int code, string response);
    private void NotifyMurder(ulong victimSteamId, string victimDisplayName, ulong killerSteamId, string killerDisplayName, int weaponRustItemId, double distance, bool isHeadshot, bool isSleeping);
    private void NotifyPlayerConnected(ulong steamid, string displayName, string ipAddress);
    private void NotifyPlayerDisconnected(ulong steamid, string displayName);
    private void NotifyPlayerChatMessage(ulong steamid, string displayName, string messageText);
    private void NotifyPlayerBan(ulong steamid, string displayName, string reason);
    private void NotifyPlayerMute(ulong steamid, string displayName, string reason);
    private void NotifyServerOn();
    private void NotifyServerOff();
}

 class BanItem
{
    public string timestamp;
    public string reason;
    public BanItem();
}

 class MuteItem
{
    public string timestamp;
    public string reason;
    public TimeRange level;
    public DateTime untilDatetime();
     TimeSpan timeSpan();
}

 class ConfigData
{
    public bool print_errors;
    public string server_name;
    public string server_password;
    public Dictionary<string, string> Messages;
    public Dictionary<ulong, BanItem> BannedPlayers;
    public Dictionary<ulong, MuteItem> MutedPlayers;
}


```

---

## Stacks

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Stacks", "Mevent", "1.5.11")]
public class Stacks : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Notify;
    private const string Layer;
    private const string AdminPerm;
    private readonly Dictionary<Item, float> _multiplierByItem;
    private readonly List<CategoryInfo> _categories;
    private class CategoryInfo
    {
        public string Title;
        public List<ItemInfo> Items;
    }

    private readonly Dictionary<string, int> _defaultItemStackSize;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] Commands;
        [JsonProperty(PropertyName = "Work with Notify?")]
        public bool UseNotify;
        [JsonProperty(PropertyName = "Changing multiplies in containers using a hammer")]
        public bool UserHammer;
        [JsonProperty(PropertyName = "Default Multiplier for new containers")]
        public float DefaultContainerMultiplier;
        [JsonProperty(PropertyName = "Blocked List")]
        public BlockList BlockList;
    }

    private class BlockList
    {
        [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Items;
        [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> Skins;
        public bool Exists(Item item);
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private List<ItemInfo> _items;
    private Dictionary<string, ContainerData> _containers;
    private void SaveData();
    private void LoadData();
    private class ContainerData
    {
        [JsonProperty(PropertyName = "Image")]
        public string Image;
        [JsonProperty(PropertyName = "Multiplier")]
        [DefaultValue(1f)]
        public float Multiplier;
        [JsonIgnore]
        public int ID;
    }

    private class ItemInfo
    {
        [JsonProperty(PropertyName = "ShortName")]
        public string ShortName;
        [JsonProperty(PropertyName = "Name")]
        public string Name;
        [JsonProperty(PropertyName = "Default Stack Size")]
        public int DefaultStackSize;
        [JsonProperty(PropertyName = "Custom Stack Size")]
        public int CustomStackSize;
        [JsonIgnore]
        public int ID;
    }

    private readonly Dictionary<string, string> _constContainers;
    private void Init();
    private void OnServerInitialized(bool init);
    private void Unload();
    private object CanMoveItem(Item movedItem, PlayerInventory playerInventory, uint targetContainerID, int targetSlot, int amount);
    private int? OnMaxStackable(Item item);
    private void OnItemDropped(Item item, BaseEntity entity);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void CmdOpenStacks(IPlayer cov, string command, string[] args);
    [ConsoleCommand("UI_Stacks")]
    private void CmdConsoleStacks(ConsoleSystem.Arg arg);
    private void MainUi(BasePlayer player, int type, int category, int page, string search, bool first);
    private void SettingsUi(BasePlayer player, int type, int category, int page, int id, int enterValue, bool first);
    private static string HexToCuiColor(string hex, float alpha);
    private void RegisterPermissions();
    private void LoadCategories();
    private void LoadContainers(bool init);
    private int GetContainerId();
    private void LoadItems();
    private void UpdateItemStack(ItemInfo info);
    private int GetItemId();
    private void LoadImages();
    private class ItemHelper
    {
        public static bool SplitMoveItem(Item item, int amount, ItemContainer targetContainer, int targetSlot);
        public static bool SplitMoveItem(Item item, int amount, BasePlayer player);
        public static bool SplitMoveItem(Item item, int amount, PlayerInventory inventory);
        public static void SwapItems(Item item1, Item item2);
    }

    private const string SetStack;
    private const string NoPermission;
    private const string AcceptTitle;
    private const string EnterMultiplier;
    private const string CurrentMultiplier;
    private const string EnterStack;
    private const string CurrentStack;
    private const string CustomMultiplier;
    private const string DefaultMultiplier;
    private const string SettingsTitle;
    private const string StackFormat;
    private const string CustomStack;
    private const string DefaultStack;
    private const string AllTitle;
    private const string SearchTitle;
    private const string BtnBack;
    private const string BtnNext;
    private const string ItemTitle;
    private const string ContainerTitle;
    private const string CloseButton;
    private const string TitleMenu;
    protected override void LoadDefaultMessages();
    private string Msg(string key, string userid, object[] obj);
    private string Msg(BasePlayer player, string key, object[] obj);
    private void Reply(BasePlayer player, string key, object[] obj);
    private void SendNotify(BasePlayer player, string key, int type, object[] obj);
}

private class CategoryInfo
{
    public string Title;
    public List<ItemInfo> Items;
}

private class Configuration
{
    [JsonProperty(PropertyName = "Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] Commands;
    [JsonProperty(PropertyName = "Work with Notify?")]
    public bool UseNotify;
    [JsonProperty(PropertyName = "Changing multiplies in containers using a hammer")]
    public bool UserHammer;
    [JsonProperty(PropertyName = "Default Multiplier for new containers")]
    public float DefaultContainerMultiplier;
    [JsonProperty(PropertyName = "Blocked List")]
    public BlockList BlockList;
}

private class BlockList
{
    [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Items;
    [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> Skins;
    public bool Exists(Item item);
}

private class ContainerData
{
    [JsonProperty(PropertyName = "Image")]
    public string Image;
    [JsonProperty(PropertyName = "Multiplier")]
    [DefaultValue(1f)]
    public float Multiplier;
    [JsonIgnore]
    public int ID;
}

private class ItemInfo
{
    [JsonProperty(PropertyName = "ShortName")]
    public string ShortName;
    [JsonProperty(PropertyName = "Name")]
    public string Name;
    [JsonProperty(PropertyName = "Default Stack Size")]
    public int DefaultStackSize;
    [JsonProperty(PropertyName = "Custom Stack Size")]
    public int CustomStackSize;
    [JsonIgnore]
    public int ID;
}

private class ItemHelper
{
    public static bool SplitMoveItem(Item item, int amount, ItemContainer targetContainer, int targetSlot);
    public static bool SplitMoveItem(Item item, int amount, BasePlayer player);
    public static bool SplitMoveItem(Item item, int amount, PlayerInventory inventory);
    public static void SwapItems(Item item1, Item item2);
}


```

---

## StacksExtended

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("StacksExtended", "Fujikura", "1.0.2", ResourceId = 35)]
 class StacksExtended : RustPlugin
{
     bool Changed;
     bool _loaded;
     List<string> itemCategories;
     List<object> itemStackExcludes;
     Dictionary<string, List<string>> registeredPermissions;
     Dictionary<string,object> containerStacks;
     Dictionary<string,object> containerVIP;
     bool clearOnReboot;
     int commandsAuthLevel;
     bool limitPlayerInventory;
     int playerInventoryStacklimit;
    static List<object> defaultItemExcludes();
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void Init();
     void OnTerrainInitialized();
     void OnServerInitialized();
     void OnEntityBuilt(Planner planner, GameObject obj);
     void OnQuarryBuilt(Planner planner, GameObject obj);
     void UpdatePlayer(BasePlayer player);
     void OnPlayerRespawned(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     void CreateContainerVIP(List<BaseEntity> storages);
     void CreateContainerStacks(List<BaseEntity> storages);
     void CreatePermissions();
     Dictionary<string, int> UpdateContainerStacks();
     Dictionary<string, int> UpdateQuarryVIP();
     void StackDefaults();
     void StackLoad();
    [ConsoleCommand("se.reload")]
     void ccmdReload(ConsoleSystem.Arg arg);
    [ConsoleCommand("se.clearreload")]
    private void ccmdStackReload(ConsoleSystem.Arg arg);
    [ConsoleCommand("se.stackcategory")]
     void ccmdStackCategory(ConsoleSystem.Arg arg);
    [ConsoleCommand("se.stackitem")]
     void ccmdStackItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("se.listcategory")]
     void ccmdListCategory(ConsoleSystem.Arg arg);
    [ConsoleCommand("se.permissions")]
     void ccmdListPerms(ConsoleSystem.Arg arg);
}


```

---

## StackSizeController

```csharp
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;

Oxide.Plugins
[Info("Stack Size Controller", "Waizujin", 1.9, ResourceId = 1185)]
[Description("Allows you to set the max stack size of every item.")]
public class StackSizeController : RustPlugin
{
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
    [ChatCommand("stack")]
    private void StackCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("stackall")]
    private void StackAllCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("stack")]
    private void StackConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("stackall")]
    private void StackAllConsoleCommand(ConsoleSystem.Arg arg);
     bool hasPermission(BasePlayer player, string perm);
}


```

---

## StalkerLite

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Random = System.Random;

Oxide.Plugins
[Info("Stalker Emission", "Unknown", "1.0.17")]
[Description("На всей карте появляется радиация, что сопровождается ужудшением погоды, звуками и визуальными эфектами")]
public class StalkerLite : RustPlugin
{
    private bool active;
    private double last;
    private const int shockNum;
    private const int explosionsNum;
    private const int fireNum;
    private const int shockMin;
    private const int shockMax;
    private const int explosionMin;
    private const int explosionMax;
    private const int fireMin;
    private const int fireMax;
    private float cooldown;
    private float duration;
    private float delay;
    private float radDelay;
    private float shockDelay;
    private float explosionDelay;
    private float fireDelay;
    private float radAmount;
    private const string firePrefab;
    private const string electricPrefab;
    private const string explosionPrefab;
    private const string preMessage;
    private const string emission;
    private const string endemission;
    private const string safe;
    private const string nonsafe;
    private const string permRad;
    private const string prefix;
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private void GetConfig(string Key, T var);
    [ChatCommand("rad")]
    private void CmdRad(BasePlayer player);
    [ChatCommand("stalker")]
    private void ChatCmd(BasePlayer player);
    private void Loaded();
    private void OnServerInitialized();
    private void Unload();
    private List<ulong> Protected;
    public Timer Radiation;
    public Timer Explosion;
    public Timer Fire;
    public Timer Shock;
    public Timer CheckProtection;
    private void DestroyTimers();
    private Random rand;
    private void StartEmission();
    private void StopEmission();
    private void Emission();
    private bool bProtected(BasePlayer player);
    private static double Now();
}


```

---

## StartProtection

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("StartProtection", "Norn", 1.9, ResourceId = 1342)]
[Description("Give people some leeway when they first join the game.")]
public class StartProtection : RustPlugin
{
     class StoredData
    {
        public Dictionary<ulong, ProtectionInfo> Players;
        public StoredData();
    }

     class ProtectionInfo
    {
        public ulong UserId;
        public int TimeLeft;
        public bool Multiple;
        public int InitTimestamp;
        public ProtectionInfo();
    }

     StoredData storedData;
     StoredData storedDataEx;
    private void Loaded();
    public Int32 UnixTimeStampUTC();
    static readonly DateTime UnixEpoch;
    static readonly double MaxUnixSeconds;
    public static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private void RemoveOldUsers();
     void OnPlayerFirstInit(ulong steamid);
     void OnUserApprove(Network.Connection connection);
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     string GetMessage(string key, string steamId);
    private void PunishPlayer(BasePlayer player, int new_time, bool message);
     Dictionary<Type, Action> EntityTypes;
    private HitInfo OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    [ChatCommand("sp")]
    private void SPCommand(BasePlayer player, string command, string[] args);
    private void UpdateProtectedListEx(BasePlayer player);
    private void UpdateProtectedList();
     void Unload();
     void SaveData();
    private void OnPlayerSleepEnded(BasePlayer player);
    private void PrintToChatEx(BasePlayer player, string result, string tcolour);
     Timer ProtectionTimer;
    private void OnServerInitialized();
}

 class StoredData
{
    public Dictionary<ulong, ProtectionInfo> Players;
    public StoredData();
}

 class ProtectionInfo
{
    public ulong UserId;
    public int TimeLeft;
    public bool Multiple;
    public int InitTimestamp;
    public ProtectionInfo();
}


```

---

## StashBlocker

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Stash Blocker", "Orange", "1.0.0")]
[Description("Controls stashes placement")]
public class StashBlocker : RustPlugin
{
    private void Init();
    private object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. Block placing stashes (globally)")]
        public bool global;
        [JsonProperty(PropertyName = "2. Radius of allowed distance between stashes and any entities (set to 0 to disable)")]
        public float entities;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<string, string> EN;
    private void message(BasePlayer player, string key, object[] args);
    private object CheckBuild(Planner planner, Construction prefab, Construction.Target target);
    private bool IsStash(string name);
    private bool HasStashesNearby(Vector3 position);
    private bool HasBuildingsNearby(Vector3 position);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. Block placing stashes (globally)")]
    public bool global;
    [JsonProperty(PropertyName = "2. Radius of allowed distance between stashes and any entities (set to 0 to disable)")]
    public float entities;
}


```

---

## StashControl

```csharp
using UnityEngine;
using System;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("StashControl", "Piarb", "1.0.1", ResourceId = 1950)]
[Description("Manage small stashes")]
public class StashControl : RustPlugin
{
    public bool PerpetuumStashes;
    public bool ConstDecayStashes;
    public bool Debug;
    public int StashControlTick;
    public int StashLifeTime;
    public bool RepairOnLoot;
    public bool HiddenDecayOnly;
    private bool ConfigChanged;
    private float hurt_amount;
    protected override void LoadDefaultConfig();
     void LoadConfig();
     object GetConfigValue(string category, string setting, object defaultValue);
     void Loaded();
    private void NoDecayStashes();
    private void HurtStahes();
     void OnLootEntity(BasePlayer player, BaseCombatEntity entity);
}


```

---

## Statistics

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Facepunch.Math;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Statistics", "oxide-russia.ru", "0.1.5")]
[Description("Statistics")]
public class Statistics : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private StoredData DataBase;
    public ulong lastDamageName;
    public string GetImage(string shortname, ulong skin);
    private static ConfigFile config;
    public class ConfigFile
    {
        [JsonProperty(PropertyName = "StatsCmd")]
        public string StatsCmd { get; set; }
        [JsonProperty(PropertyName = "ServerNAME")]
        public string ServerName { get; set; }
        [JsonProperty(PropertyName = "MainHeaderColor")]
        public string MainHeaderColor { get; set; }
        [JsonProperty(PropertyName = "MainHeaderButtonColor")]
        public string MainHeaderButtonColor { get; set; }
        [JsonProperty(PropertyName = "MainHeaderButtonEdgeColor")]
        public string MainHeaderButtonEdgeColor { get; set; }
        [JsonProperty(PropertyName = "MainHeaderButtonCloseColor")]
        public string MainHeaderButtonCloseColor { get; set; }
        [JsonProperty(PropertyName = "PlayerStatsHeaderColor1")]
        public string PlayerStatsHeaderColor1 { get; set; }
        [JsonProperty(PropertyName = "PlayerStatsHeaderColor2")]
        public string PlayerStatsHeaderColor2 { get; set; }
        [JsonProperty(PropertyName = "PlayerStatsBlocColor1")]
        public string PlayerStatsBlocColor1 { get; set; }
        [JsonProperty(PropertyName = "PlayerStatsBlocColor2")]
        public string PlayerStatsBlocColor2 { get; set; }
        [JsonProperty(PropertyName = "PlayerStatsProfileLineColor")]
        public string PlayerStatsProfileLineColor { get; set; }
        [JsonProperty(PropertyName = "PlayerStatsTextColor")]
        public string PlayerStatsTextColor { get; set; }
        [JsonProperty(PropertyName = "ServerStatsButtonsUpLine")]
        public string ServerStatsButtonsUpLine { get; set; }
        [JsonProperty(PropertyName = "ServerStatsButtonsDownLine")]
        public string ServerStatsButtonsDownLine { get; set; }
        [JsonProperty(PropertyName = "ServerStatsButtonsColor")]
        public string ServerStatsButtonsColor { get; set; }
        [JsonProperty(PropertyName = "ServerStatsButtonsEdgeColor")]
        public string ServerStatsButtonsEdge { get; set; }
        [JsonProperty(PropertyName = "ServerStatsTopColor1")]
        public string ServerStatsTopColor1 { get; set; }
        [JsonProperty(PropertyName = "ServerStatsTopColor2")]
        public string ServerStatsTopColor2 { get; set; }
        [JsonProperty(PropertyName = "ServerStatsTopEdgeColor")]
        public string ServerStatsTopEdgeColor { get; set; }
        [JsonProperty(PropertyName = "ServerStatsTextColor")]
        public string ServerStatsTextColor { get; set; }
        [JsonProperty(PropertyName = "ServerStatsTopTextColor1")]
        public string ServerStatsTopTextColor1 { get; set; }
        [JsonProperty(PropertyName = "ServerStatsTopTextColor2")]
        public string ServerStatsTopTextColor2 { get; set; }
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    private void Regenerate();
    protected override void LoadDefaultConfig();
    private new void LoadDefaultMessages();
    public class StoredData
    {
        public Dictionary<ulong, PlayerInfo> PlayerInfo;
    }

    public class PlayerInfo
    {
        public string Name;
        public int kills;
        public int deaths;
        public int animalkills;
        public int bradleykills;
        public int helikills;
        public int wood;
        public int stones;
        public int irons;
        public int sulfur;
        public int time;
        public int Bear;
        public int Boar;
        public int Chicken;
        public int Wolf;
        public int Horse;
        public int Stag;
        public PlayerInfo();
    }

    private void SaveData();
    private void LoadData();
     void AddPlayer(BasePlayer player);
    public void PlayerStatGuiCreate(BasePlayer player);
    public void ServerStatGuiCreate(BasePlayer player);
    private string TopGui;
     void CmdStats(IPlayer player, string command, string[] args);
    [ConsoleCommand("serverstats_gui")]
     void CmdServerStatsGui(ConsoleSystem.Arg args);
    [ConsoleCommand("PlayerStats_gui")]
     void CmdPlayerStatsGui(ConsoleSystem.Arg args);
    [ConsoleCommand("topkills")]
     void CmdTopKills(ConsoleSystem.Arg args);
    [ConsoleCommand("topDeaths")]
     void CmdTopDeaths(ConsoleSystem.Arg args);
    [ConsoleCommand("topTanks")]
     void CmdTopTanks(ConsoleSystem.Arg args);
    [ConsoleCommand("topHeli")]
     void CmdTopHeli(ConsoleSystem.Arg args);
    [ConsoleCommand("topAnimalkills")]
     void CmdTopAnimalKills(ConsoleSystem.Arg args);
    [ConsoleCommand("topServerTime")]
     void CmdTopServerTime(ConsoleSystem.Arg args);
    [ConsoleCommand("topWood")]
     void CmdTopWood(ConsoleSystem.Arg args);
    [ConsoleCommand("topStones")]
     void CmdTopStones(ConsoleSystem.Arg args);
    [ConsoleCommand("topMetal")]
     void CmdTopMetal(ConsoleSystem.Arg args);
    [ConsoleCommand("topSulfur")]
     void CmdTopSulfur(ConsoleSystem.Arg args);
     Dictionary<ulong, int> timelist;
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
     void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerDie(BasePlayer player, HitInfo info);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitinfo);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void CheckDb(BasePlayer player);
    private string GetPlaytimeClock(double time);
    private static string HexToRustFormat(string hex);
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "StatsCmd")]
    public string StatsCmd { get; set; }
    [JsonProperty(PropertyName = "ServerNAME")]
    public string ServerName { get; set; }
    [JsonProperty(PropertyName = "MainHeaderColor")]
    public string MainHeaderColor { get; set; }
    [JsonProperty(PropertyName = "MainHeaderButtonColor")]
    public string MainHeaderButtonColor { get; set; }
    [JsonProperty(PropertyName = "MainHeaderButtonEdgeColor")]
    public string MainHeaderButtonEdgeColor { get; set; }
    [JsonProperty(PropertyName = "MainHeaderButtonCloseColor")]
    public string MainHeaderButtonCloseColor { get; set; }
    [JsonProperty(PropertyName = "PlayerStatsHeaderColor1")]
    public string PlayerStatsHeaderColor1 { get; set; }
    [JsonProperty(PropertyName = "PlayerStatsHeaderColor2")]
    public string PlayerStatsHeaderColor2 { get; set; }
    [JsonProperty(PropertyName = "PlayerStatsBlocColor1")]
    public string PlayerStatsBlocColor1 { get; set; }
    [JsonProperty(PropertyName = "PlayerStatsBlocColor2")]
    public string PlayerStatsBlocColor2 { get; set; }
    [JsonProperty(PropertyName = "PlayerStatsProfileLineColor")]
    public string PlayerStatsProfileLineColor { get; set; }
    [JsonProperty(PropertyName = "PlayerStatsTextColor")]
    public string PlayerStatsTextColor { get; set; }
    [JsonProperty(PropertyName = "ServerStatsButtonsUpLine")]
    public string ServerStatsButtonsUpLine { get; set; }
    [JsonProperty(PropertyName = "ServerStatsButtonsDownLine")]
    public string ServerStatsButtonsDownLine { get; set; }
    [JsonProperty(PropertyName = "ServerStatsButtonsColor")]
    public string ServerStatsButtonsColor { get; set; }
    [JsonProperty(PropertyName = "ServerStatsButtonsEdgeColor")]
    public string ServerStatsButtonsEdge { get; set; }
    [JsonProperty(PropertyName = "ServerStatsTopColor1")]
    public string ServerStatsTopColor1 { get; set; }
    [JsonProperty(PropertyName = "ServerStatsTopColor2")]
    public string ServerStatsTopColor2 { get; set; }
    [JsonProperty(PropertyName = "ServerStatsTopEdgeColor")]
    public string ServerStatsTopEdgeColor { get; set; }
    [JsonProperty(PropertyName = "ServerStatsTextColor")]
    public string ServerStatsTextColor { get; set; }
    [JsonProperty(PropertyName = "ServerStatsTopTextColor1")]
    public string ServerStatsTopTextColor1 { get; set; }
    [JsonProperty(PropertyName = "ServerStatsTopTextColor2")]
    public string ServerStatsTopTextColor2 { get; set; }
}

public class StoredData
{
    public Dictionary<ulong, PlayerInfo> PlayerInfo;
}

public class PlayerInfo
{
    public string Name;
    public int kills;
    public int deaths;
    public int animalkills;
    public int bradleykills;
    public int helikills;
    public int wood;
    public int stones;
    public int irons;
    public int sulfur;
    public int time;
    public int Bear;
    public int Boar;
    public int Chicken;
    public int Wolf;
    public int Horse;
    public int Stag;
    public PlayerInfo();
}


```

---

## Stats

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Stats", "Я и Я", "1.0.0")]
public class Stats : RustPlugin
{
    private string Layer;
    private const int Count;
    private readonly string[] _statsName;
    private Dictionary<string, string> tops;
    private void chatCmdStats(BasePlayer player, string command, string[] args);
    private void consoleCmdStats(ConsoleSystem.Arg arg);
     void OnServerInitialized();
     object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     object OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     object OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProtoBuf.ProjectileShoot projectiles);
     void OnPlayerConnected(BasePlayer player);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void Unload();
    [HookMethod("ApiGetName")]
    public string ApiGetName(ulong steamID);
    private void ShowStatistic(BasePlayer player, ulong userID);
    private static string HexToRGB(string hex);
     void SaveData();
     void LoadData();
     Dictionary<ulong, Dictionary<StatsType, int>> storedData;
    private DynamicConfigFile StatData;
    public CuiRawImageComponent GetAvatarImageComponent(ulong user_id, string color);
    public CuiRawImageComponent GetImageComponent(string url, string shortName, string color);
    public CuiRawImageComponent GetItemImageComponent(string shortName);
    public bool AddImage(string url, string shortName);
}


```

---

## StatsSystem

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("StatsSystem", "Dezz", "1.0.6")]
 class StatsSystem : RustPlugin
{
     string Layer;
    [PluginReference]
     Plugin ImageLibrary;
     Plugin RustStore;
     Dictionary<ulong, DBSettings> DB;
    public string GetImage(string shortname, ulong skin);
    public class DBSettings
    {
        public string DisplayName;
        public int Points;
        public bool IsConnected;
        public int Balance;
        public Dictionary<string, int> Settings;
        public Dictionary<string, int> Res;
    }

     Configuration config;
     class Configuration
    {
        [JsonProperty("ID магазина")]
        public string ShopID;
        [JsonProperty("Secret ключ магазина")]
        public string Secret;
        [JsonProperty("Настройки бонусов")]
        public List<string> Bonus;
        public static Configuration GetNewConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void PlayTime();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void Unload();
     void SaveDataBase();
     void OnDispenserGather(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
    public ulong lastDamageName;
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnNewSave();
     void ApiChangeGameStoresBalance(ulong userId, int amount);
     void APIChangeUserBalance(ulong steam, int balanceChange);
     void ExecuteApiRequest(Dictionary<string, string> args);
     List<string> ResImage;
    [ChatCommand("stat")]
     void ChatTop(BasePlayer player);
    [ConsoleCommand("stats")]
     void ConsoleSkip(ConsoleSystem.Arg args);
     void StatsUI(BasePlayer player, int page);
    [ChatCommand("stats")]
     void cmdProfileUis(BasePlayer player);
     void ProfileUI(BasePlayer player, ulong SteamID, int z);
     void SteamAvatarAdd(string userid);
}

public class DBSettings
{
    public string DisplayName;
    public int Points;
    public bool IsConnected;
    public int Balance;
    public Dictionary<string, int> Settings;
    public Dictionary<string, int> Res;
}

 class Configuration
{
    [JsonProperty("ID магазина")]
    public string ShopID;
    [JsonProperty("Secret ключ магазина")]
    public string Secret;
    [JsonProperty("Настройки бонусов")]
    public List<string> Bonus;
    public static Configuration GetNewConfig();
}


```

---

## StickyChat

```csharp
using System;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;

Oxide.Plugins
[Info("StickyChat", "Visagalis", "0.0.3")]
public class StickyChat : RustPlugin
{
    public class StickyInfo
    {
        public string details;
        public ChatType type;
    }

     Dictionary<BasePlayer, StickyInfo> stickies;
    [HookMethod("OnPlayerDisconnected")]
     void OnPlayerDisconnected(BasePlayer player);
    [PluginReference("Clans")]
     Plugin clanLib;
    [PluginReference("PM")]
     Plugin pmLib;
    [ChatCommand("ct")]
    private void clanChat(BasePlayer player, string command, string[] args);
    [ChatCommand("gt")]
    private void generalChat(BasePlayer player, string command, string[] args);
    [ChatCommand("rt")]
    private void replyChat(BasePlayer player, string command, string[] args);
    [ChatCommand("pt")]
    private void privateChat(BasePlayer player, string command, string[] args);
     object OnPlayerChat(ConsoleSystem.Arg arg);
    private void SendHelpText(BasePlayer player);
    private int PlayerStickyState(BasePlayer player);
}

public class StickyInfo
{
    public string details;
    public ChatType type;
}


```

---

## StopDamageMan

```csharp
using ConVar;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("StopDamageMan", "Sempai#3239", "0.0.2")]
 class StopDamageMan : RustPlugin
{
    public List<ulong> PlayerStopDamage;
     void OnServerInitialized();
     void Unload();
     void OnServerSave();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    [ChatCommand("sdm_add")]
     void AddPlayerNoDamageList(BasePlayer player, string command, string[] args);
    [ChatCommand("sdm_remove")]
     void RemovePlayerNoDamageList(BasePlayer player, string command, string[] args);
    private BasePlayer FindPlayerByPartialName(string name);
    [PluginReference]
     Plugin IQChat;
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    private new void LoadDefaultMessages();
}


```

---

## StoreBonus

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;

Oxide.Plugins
[Info("StoreBonus", "OxideBro", "1.0.1")]
 class StoreBonus : RustPlugin
{
     List<Counts> logs;
    public int timercallbackdelay;
     class Counts
    {
        [JsonProperty("time")]
        public string time;
        public Counts(BasePlayer player);
    }

     Dictionary<BasePlayer, int> timers;
     List<ulong> activePlayers;
     string HandleArgs(string json, object[] args);
    private double GetCurrentTime();
    [ConsoleCommand("getbonus")]
     void CmdGetBonus(ConsoleSystem.Arg arg);
    public BasePlayer FindBasePlayer(string nameOrUserId);
     void GradeTimerHandler();
     void UpdateTimer(BasePlayer player);
     void DeactivateTimer(BasePlayer player);
     void ActivateTimer(ulong userId);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
    private string GetFormatTime(int seconds);
    public Timer mytimer;
    public Timer mytimer2;
     string secret;
    private string ChatSms;
     string shopId;
     int amount1;
     bool EnableMsg;
     bool LogsPlayer;
    public int GameActive;
     bool MoscowStore;
     bool EnableGUIPlayer;
     string EnableGUIPlayerMin;
     string EnableGUIPlayerMax;
     string GUIEnabledColor;
     string GUIEnabledText;
    public string GetShop;
    private void LoadDefaultConfig();
    private void GetConfig(string menu, string Key, T var);
    [ChatCommand("bonus")]
     void cmdChatBonus(BasePlayer player, string command, string[] args);
    [ConsoleCommand("bonus.plus")]
     void cmdStoreBonusAdd(ConsoleSystem.Arg arg);
    [ConsoleCommand("bonus.minus")]
     void cmdStoreBonusRemove(ConsoleSystem.Arg arg);
    [ConsoleCommand("money.plus")]
     void cmdStoreMoneyAdd(ConsoleSystem.Arg arg);
    [ConsoleCommand("money.minus")]
     void cmdStoreMoneyRemove(ConsoleSystem.Arg arg);
    [ConsoleCommand("bonusclose")]
     void CmdDestroyGui(ConsoleSystem.Arg arg);
    [ConsoleCommand("drawui")]
     void DrawUIPlayer(ConsoleSystem.Arg arg);
    [ConsoleCommand("acceptclose")]
     void CmdDestroyGuiAccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("bonus.accept")]
     void CmdBonusGetAllAccept(ConsoleSystem.Arg arg);
    [ConsoleCommand("bonus.getall")]
     void CmdBonusGetAll(ConsoleSystem.Arg arg);
     void DrawUIBalance(BasePlayer player, int seconds);
     void DrawUIGetBonus(BasePlayer player);
     void DrawUIPlayer(BasePlayer player);
     void DrawUIAccept(BasePlayer player);
     void DrawUI(BasePlayer player);
     void DestroyUI(BasePlayer player);
     void DestroyUIPlayer(BasePlayer player);
     void DestroyUIAccept(BasePlayer player);
     void Unload();
     void OnPlayerDisconnected(BasePlayer player);
     void OnServerInitialized();
     void LoadData();
     void OnPlayerSleepEnded(BasePlayer player);
     void SaveData();
     void OnPlayerInit(BasePlayer player);
     void APIChangeUserBalance(ulong steam, int balanceChange, Action<string> callback);
     void MoneyPlus(ulong userId, int amount);
     void MoneyMinus(ulong userId, int amount);
     void ExecuteApiRequest(Dictionary<string, string> args);
     class DataStorage
    {
        public Dictionary<ulong, STOREDATA> StoreData;
        public DataStorage();
    }

     class STOREDATA
    {
        public string Name;
        public int EnabledBonus;
        public int Bonus;
        public int GameTime;
    }

     DataStorage data;
    private DynamicConfigFile StoreData;
     DynamicConfigFile logsFile;
}

 class Counts
{
    [JsonProperty("time")]
    public string time;
    public Counts(BasePlayer player);
}

 class DataStorage
{
    public Dictionary<ulong, STOREDATA> StoreData;
    public DataStorage();
}

 class STOREDATA
{
    public string Name;
    public int EnabledBonus;
    public int Bonus;
    public int GameTime;
}


```

---

## StrikeSystem

```csharp
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System.Linq;
using System;

Oxide.Plugins
[Info("StrikeSystem", "LaserHydra", "2.0.0", ResourceId = 1276)]
[Description("Strike players & time-ban players with a specific amount of strikes")]
 class StrikeSystem : RustPlugin
{
    static List<Player> players;
    [PluginReference("EnhancedBanSystem")]
     Plugin EBS;
     class Player
    {
        public ulong steamId;
        public string name;
        public string lastStrike;
        public int strikes;
        public int activeStrikes;
        public Player();
        internal Player(BasePlayer player);
        internal void Update(BasePlayer player);
        internal static Player Get(BasePlayer player);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }

     void Loaded();
     void LoadConfig();
     void LoadMessages();
    protected override void LoadDefaultConfig();
    [ChatCommand("strike")]
     void cmdStrike(BasePlayer player, string cmd, string[] args);
     void StrikePlayer(BasePlayer player, string reason);
     bool ReachedMaxStrikes(BasePlayer player);
     void BanPlayer(BasePlayer player, string reason);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer player);
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void LoadData(T data, string filename);
     void SaveData(T data, string filename);
     string GetMsg(string key, object userID);
     void RegisterPerm(string[] permArray);
     bool HasPerm(object uid, string[] permArray);
     string PermissionPrefix { get; set; }
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Player
{
    public ulong steamId;
    public string name;
    public string lastStrike;
    public int strikes;
    public int activeStrikes;
    public Player();
    internal Player(BasePlayer player);
    internal void Update(BasePlayer player);
    internal static Player Get(BasePlayer player);
    public override bool Equals(object obj);
    public override int GetHashCode();
}


```

---

## StructureRefund

```csharp
using System;

Oxide.Plugins
[Info("StructureRefund", "Wulf/lukespragg", "1.2.0", ResourceId = 1692)]
[Description("Refunds previous build materials when demolishing and/or upgrading")]
 class StructureRefund : CovalencePlugin
{
    const string permDemolish;
    const string permUpgrade;
     bool demolishRefunds;
     bool upgradeRefunds;
    protected override void LoadDefaultConfig();
     void Init();
     void RefundMaterials(BuildingBlock block, BasePlayer player);
     void OnStructureDemolish(BuildingBlock block, BasePlayer player);
     void OnStructureUpgrade(BuildingBlock block, BasePlayer player);
     T GetConfig(string name, T value);
}


```

---

## StructureWhitelist

```csharp
using System.Collections.Generic;
using System;

Oxide.Plugins
[Info("StructureWhitelist", "Norn", 0.2, ResourceId = 1511)]
[Description("Choose which grades players can build.")]
public class StructureWhitelist : RustPlugin
{
     void OnServerInitialized();
     void Loaded();
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     string GetMessage(string key, string steamId);
     object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum grade);
}


```

---

## SuicideBomber

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("SuicideBomber", "Calytic @ cyclone.network", "0.0.2", ResourceId = 1425)]
 class SuicideBomber : RustPlugin
{
    private float damage;
    private float radius;
    private int explosives;
    private int c4;
    private bool flare;
    private bool scream;
    private Dictionary<string, string> messages;
    private List<string> texts;
     void OnServerInitialized();
     void LoadData();
    protected void ReloadConfig();
    protected override void LoadDefaultConfig();
    private T GetConfig(string name, T defaultValue);
     void OnPlayerInput(BasePlayer player, InputState input);
}


```

---

## SuicideNerf

```csharp
using System;
using UnityEngine;
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("SuicideNerf", "Kyrah Abattoir", "0.1", ResourceId = 1873)]
[Description("Forces you to bleedout when using 'kill' and adds a cooldown.")]
 class SuicideNerf : RustPlugin
{
    private int cfgMinTimeBetweenSuicideAttempts;
    private bool cfgDoBleedout;
    private Dictionary<ulong, float> nextSuicideTime;
    [HookMethod("OnRunCommand")]
    private object OnRunCommand(ConsoleSystem.Arg arg);
    private object DoSuicide(BasePlayer ply);
}


```

---

## SuperGame

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("SuperGame", "Chibubrik", "1.3.0")]
 class SuperGame : RustPlugin
{
    private string Layer;
    [PluginReference]
    private Plugin ImageLibrary;
    public Dictionary<ulong, Gather> gather;
    public class Settings
    {
        [JsonProperty("Название панельки")]
        public string Name;
        [JsonProperty("Сбрасывать ли прогресс при достижении 200 баллов?")]
        public bool Progress;
        [JsonProperty("Информация")]
        public string Info;
        [JsonProperty("Баллы")]
        public string Ball;
    }

    public class GameSettings
    {
        [JsonProperty("Название руды")]
        public string Name;
        [JsonProperty("ShortName руды")]
        public string ShortName;
        [JsonProperty("Сколько нужно набрать очков")]
        public int Count;
    }

    public class Gather
    {
        [JsonProperty("Общее кол - во баллов")]
        public int Amount;
        [JsonProperty("Список добываемых ресурсов")]
        public Dictionary<string, PlayerGather> Res;
    }

    public class PlayerGather
    {
        [JsonProperty("Количество очков у игрока")]
        public int Count;
    }

    public Configuration config;
    public class Configuration
    {
        [JsonProperty("Настройки")]
        public Settings settings;
        [JsonProperty("Ресурсы")]
        public List<GameSettings> game;
        [JsonProperty("Список наград (выбирается случайно)")]
        public Dictionary<string, string> RewardList;
        public static Configuration GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("game")]
    private void cmdGame(BasePlayer player, string command, string[] args);
    [ConsoleCommand("game.priz")]
    private void cmdConsoleGame(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private PlayerGather GatherData(ulong userID, string name);
    private void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void SaveData();
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void GameUI(BasePlayer player);
}

public class Settings
{
    [JsonProperty("Название панельки")]
    public string Name;
    [JsonProperty("Сбрасывать ли прогресс при достижении 200 баллов?")]
    public bool Progress;
    [JsonProperty("Информация")]
    public string Info;
    [JsonProperty("Баллы")]
    public string Ball;
}

public class GameSettings
{
    [JsonProperty("Название руды")]
    public string Name;
    [JsonProperty("ShortName руды")]
    public string ShortName;
    [JsonProperty("Сколько нужно набрать очков")]
    public int Count;
}

public class Gather
{
    [JsonProperty("Общее кол - во баллов")]
    public int Amount;
    [JsonProperty("Список добываемых ресурсов")]
    public Dictionary<string, PlayerGather> Res;
}

public class PlayerGather
{
    [JsonProperty("Количество очков у игрока")]
    public int Count;
}

public class Configuration
{
    [JsonProperty("Настройки")]
    public Settings settings;
    [JsonProperty("Ресурсы")]
    public List<GameSettings> game;
    [JsonProperty("Список наград (выбирается случайно)")]
    public Dictionary<string, string> RewardList;
    public static Configuration GetNewCong();
}


```

---

## SuperMedkit-1.0.0

```csharp
using System;
using System.Linq;
using UnityEngine;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;

Oxide.Plugins
[Info("SuperMedkit", "chhhh", "1.0.0")]
 class SuperMedkit : RustPlugin
{
    private const string BloodItemName;
    private const string MedkitItemName;
    private static HashSet<ResourceDispenser> GivenAnimals;
    private static Dictionary<ulong, double> Cooldown;
    private void Init();
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private object OnItemSplit(Item item, int split_Amount);
    private object CanStackItem(Item item, Item anotherItem);
    private object CanCombineDroppedItem(DroppedItem drItem, DroppedItem anotherDrItem);
    private bool? CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot, int amount);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private bool ChangeItem(Item item);
    private void GiveBlood(BasePlayer player, int amount);
    private double GrabCurrentTime();
    [ChatCommand("sm_give")]
    private void CommandGive(BasePlayer player, string command, string[] args);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "При добыче мяса животных выпадает кровь")]
        public Dictionary<string, int> BloodRates;
        [JsonProperty(PropertyName = "Название на пакете с кровью")]
        public string BloodName;
        [JsonProperty(PropertyName = "Скин супер аптечки")]
        public ulong SuperMedkitSkin;
        [JsonProperty(PropertyName = "Название супер аптечки")]
        public string SuperMedkitName;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void SaveConfig(ConfigData config);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "При добыче мяса животных выпадает кровь")]
    public Dictionary<string, int> BloodRates;
    [JsonProperty(PropertyName = "Название на пакете с кровью")]
    public string BloodName;
    [JsonProperty(PropertyName = "Скин супер аптечки")]
    public ulong SuperMedkitSkin;
    [JsonProperty(PropertyName = "Название супер аптечки")]
    public string SuperMedkitName;
}


```

---

## SupplyDropDestroyer

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;
using System.Linq;
using Rust;

Oxide.Plugins
[Info("Supply Drop Destroyer", "PaiN", 0.2, ResourceId = 1281)]
[Description("This plugin destroys the supply drop container after x seconds.")]
 class SupplyDropDestroyer : RustPlugin
{
    private bool Changed;
    private int destroyafter;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void Loaded();
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## SupplySignalAlert

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using System.Reflection;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Rust;

Oxide.Plugins
[Info("Supply Signal Alerter", "Lederp", "1.0.0")]
 class SupplySignalAlert : RustPlugin
{
     void OnWeaponThrown(BasePlayer player, BaseEntity entity);
}


```

---

## SupplySpeed

```csharp
using UnityEngine;
using Newtonsoft.Json;
using System.Collections;

Oxide.Plugins
[Info("Supply Speed", "rustmods.ru.", "1.0.1")]
public class SupplySpeed : RustPlugin
{
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty("Во сколько раз ускорять аирдроп?")]
        public float speed;
        [JsonProperty("Через сколько секунд будет удаляться шашка с дымом? (обычно 210 сек.)")]
        public float grenadeDespawn;
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private void OnEntitySpawned(SupplyDrop supplyDrop);
    private void OnEntitySpawned(SupplySignal signal);
    private class SignalSettings : MonoBehaviour
    {
        public void ActivateDestroyer(float duration);
        private IEnumerator KillCoroutine(float duration);
    }

    private class DropSettings : MonoBehaviour
    {
        private SupplyDrop supplyDrop;
        private BaseEntity chute;
        private Vector3 windDir;
        private Vector3 newDir;
        public float windSpeed;
        private void Awake();
        private Vector3 GetDirection();
        private void FixedUpdate();
    }

}

public class Configuration
{
    [JsonProperty("Во сколько раз ускорять аирдроп?")]
    public float speed;
    [JsonProperty("Через сколько секунд будет удаляться шашка с дымом? (обычно 210 сек.)")]
    public float grenadeDespawn;
}

private class SignalSettings : MonoBehaviour
{
    public void ActivateDestroyer(float duration);
    private IEnumerator KillCoroutine(float duration);
}

private class DropSettings : MonoBehaviour
{
    private SupplyDrop supplyDrop;
    private BaseEntity chute;
    private Vector3 windDir;
    private Vector3 newDir;
    public float windSpeed;
    private void Awake();
    private Vector3 GetDirection();
    private void FixedUpdate();
}


```

---

## SwapCoins

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("SwapCoins", "Aliluya/Sparkless", "1.1.0")]
 class SwapCoins : RustPlugin
{
    private string NameSilver;
    private string NameGold;
    private List<string> listContainersSilver;
    private List<string> ListContainersGold;
    [PluginReference]
     Plugin ImageLibrary;
     void Unload();
     void OnServerInitialized();
    private PluginConfig config;
    public class CustomItem
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string target;
        [JsonProperty(PropertyName = "Название предмета")]
        public string name;
        [JsonProperty(PropertyName = "Скин")]
        public ulong skinid;
    }

     class PluginConfig
    {
        [JsonProperty("Номер магазина gamestores")]
        public string ShopID;
        [JsonProperty("Секретный ключ")]
        public string APIKey;
        [JsonProperty("Ссылка на магазин")]
        public string servername;
        [JsonProperty("Мин рублей для обмена")]
        public int ObmenMoneyNeed;
        [JsonProperty("Кол-во золотых монет к рублю")]
        public int gold;
        [JsonProperty("Кол-во серебряных монет к рублю")]
        public int silver;
        [JsonProperty("Кол-во золотых монет к серебряным")]
        public int obmen;
        [JsonProperty("Какой магазин использовать (true = gamestores, false = moscow.ovh)")]
        public bool gamestores;
        [JsonProperty("Шанс выпадение серебра из ящиков")]
        public int ChanceSilver;
        [JsonProperty("Мин.выпадение серебра(шт)")]
        public int MinSilver;
        [JsonProperty("Макс.выпадение серебра(шт)")]
        public int MaxSilver;
        [JsonProperty("Шанс выпадение золота из ящика")]
        public int ChanceGold;
        [JsonProperty("Мин.выпадение золота(шт)")]
        public int MinGold;
        [JsonProperty("Макс.выпадение золота(шт)")]
        public int MaxGold;
        [JsonProperty(PropertyName = "Список предметов для замены скинов и имени")]
        public List<CustomItem> items;
    }

    private PluginConfig PanelConfig();
    [JsonProperty("Изображения плагина")]
    private Dictionary<string, string> PluginImages;
    private void Init();
    protected override void LoadDefaultConfig();
     void OnLootEntity(BasePlayer player, BaseEntity entity, Item item);
    private List<LootContainer> handledContainers;
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info, Item item);
    [ConsoleCommand("UI_OBMENGOLDSILVER")]
    private void ObmenForGold(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_OBMENRUBSILVER")]
    private void ObmenForRubSilver(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_OBMENRUBGOLD")]
    private void ObmenForRubGold(ConsoleSystem.Arg arg);
    const string Layer;
    [ChatCommand("swap")]
    private void DrawGui(BasePlayer player);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void APIChangeUserBalance(ulong steam, int balanceChange, Action<string> callback);
     void MoneyPlus(ulong userId, int amount);
     void ExecuteApiRequest(Dictionary<string, string> args);
    private static string HexToCuiColor(string hex);
}

public class CustomItem
{
    [JsonProperty(PropertyName = "Shortname")]
    public string target;
    [JsonProperty(PropertyName = "Название предмета")]
    public string name;
    [JsonProperty(PropertyName = "Скин")]
    public ulong skinid;
}

 class PluginConfig
{
    [JsonProperty("Номер магазина gamestores")]
    public string ShopID;
    [JsonProperty("Секретный ключ")]
    public string APIKey;
    [JsonProperty("Ссылка на магазин")]
    public string servername;
    [JsonProperty("Мин рублей для обмена")]
    public int ObmenMoneyNeed;
    [JsonProperty("Кол-во золотых монет к рублю")]
    public int gold;
    [JsonProperty("Кол-во серебряных монет к рублю")]
    public int silver;
    [JsonProperty("Кол-во золотых монет к серебряным")]
    public int obmen;
    [JsonProperty("Какой магазин использовать (true = gamestores, false = moscow.ovh)")]
    public bool gamestores;
    [JsonProperty("Шанс выпадение серебра из ящиков")]
    public int ChanceSilver;
    [JsonProperty("Мин.выпадение серебра(шт)")]
    public int MinSilver;
    [JsonProperty("Макс.выпадение серебра(шт)")]
    public int MaxSilver;
    [JsonProperty("Шанс выпадение золота из ящика")]
    public int ChanceGold;
    [JsonProperty("Мин.выпадение золота(шт)")]
    public int MinGold;
    [JsonProperty("Макс.выпадение золота(шт)")]
    public int MaxGold;
    [JsonProperty(PropertyName = "Список предметов для замены скинов и имени")]
    public List<CustomItem> items;
}


```

---

## SyringeDenerf

```csharp
using System;

Oxide.Plugins
[Info("SyringeDenerf", "ignignokt84", "0.1.3", ResourceId = 1809)]
 class SyringeDenerf : RustPlugin
{
    private bool hasConfigChanged;
     float healAmount;
     float hotAmount;
     float hotTime;
     object OnHealingItemUse(HeldEntity item, BasePlayer target);
     void Loaded();
    protected override void LoadDefaultConfig();
    private void LoadConfig();
    private object GetConfig(string str, object defaultValue);
}


```

---

## TankCommander

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using System.Globalization;
using Rust;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Tank Commander", "k1lly0u", "0.2.3")]
[Description("Drive tanks, shoot stuff")]
 class TankCommander : RustPlugin
{
    [PluginReference]
    private Plugin Friends;
    private Plugin Clans;
    private Plugin Godmode;
    private static TankCommander ins;
    private List<APCController> controllers;
    private Dictionary<CommandType, BUTTON> controlButtons;
    private int rocketId;
    private int mgId;
    private const string APC_PREFAB;
    private const string CHAIR_PREFAB;
    private const string UI_HEALTH;
    private const string UI_AMMO_MG;
    private const string UI_AMMO_ROCKET;
    private const int TARGET_LAYERS;
    private void Loaded();
    private void OnServerInitialized();
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private object CanDismountEntity(BasePlayer player, BaseMountable mountable);
    private void OnPlayerDisconnected(BasePlayer player);
    private object OnRunPlayerMetabolism(PlayerMetabolism metabolism, BaseCombatEntity entity);
    private void Unload();
    private bool IsOnboardAPC(BasePlayer player);
    private bool IsOnboardAPC(BasePlayer player, APCController controller);
    private T ParseType(string type);
    private bool HasPermission(BasePlayer player, string perm);
    private void ConvertControlButtons();
    private void OpenTankInventory(BasePlayer player, APCController controller);
    private class APCController : MonoBehaviour
    {
        public BradleyAPC entity;
        private Rigidbody rigidBody;
        private BaseMountable[] mountables;
        private WheelCollider[] leftWheels;
        private WheelCollider[] rightWheels;
        public ItemContainer inventory;
        private float accelTimeTaken;
        private float accelTimeToTake;
        private float forwardTorque;
        private float maxBrakeTorque;
        private float turnTorque;
        private float lastFireCannon;
        private float lastFireMG;
        private float lastFireCoax;
        private bool isDying;
        private RaycastHit eyeRay;
        private Vector3 mouseInput;
        private Vector3 aimVector;
        private Vector3 aimVectorTop;
        private Dictionary<CommandType, BUTTON> controlButtons;
        private ConfigData.WeaponOptions.WeaponSystem cannon;
        private ConfigData.WeaponOptions.WeaponSystem mg;
        private ConfigData.WeaponOptions.WeaponSystem coax;
        private ConfigData.CrushableTypes crushables;
        public bool HasCommander { get; set; }
        public BasePlayer Commander { get; set; }
        private void Awake();
        private void OnDestroy();
        private void Update();
        private void LateUpdate();
        private void OnCollisionEnter(Collision collision);
        private float CalculateImpactForce(Collision col);
        private bool IsPassenger(BasePlayer player);
        private List<KeyValuePair<Vector3, Vector3>> mountOffsets;
        private void CreateMountPoints();
        private void CreateMountPoint(int index);
        public bool CanMountPlayer();
        public void MountPlayer(BasePlayer player);
        public void DismountPlayer(BasePlayer player);
        private void DismountAll();
        private void OnEntityMounted(BasePlayer player, bool isOperator);
        private void SetInitialAimDirection();
        private void DoWeaponControls();
        private void AdjustAiming();
        private const float DDRAW_UPDATE_TIME;
        private void DrawTargeting();
        private void FireCannon();
        private void FireCoax();
        private void FireMG();
        private void FireSideGuns();
        private void ApplyDamage(BaseCombatEntity hitEntity, float damage, Vector3 point, Vector3 normal);
        private void DoMovementControls();
        private void SetThrottleSpeed(float acceleration, float steering, bool boost);
        private void ApplyBrakes(float amount);
        private void ApplyBrakeTorque(float amount, bool rightSide);
        private void ApplyMotorTorque(float torque, bool rightSide);
        private void ToggleLights();
        public void ManageDamage(HitInfo info);
        private void OnDeath();
    }

    [ChatCommand("spawntank")]
     void cmdTank(BasePlayer player, string command, string[] args);
    [ConsoleCommand("spawntank")]
     void ccmdSpawnTank(ConsoleSystem.Arg arg);
    private bool AreFriends(ulong playerId, ulong friendId);
    private bool IsClanmate(ulong playerId, ulong friendId);
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

    private static void CreateHealthUI(BasePlayer player, APCController controller);
    private static void CreateMGAmmoUI(BasePlayer player, APCController controller);
    private static void CreateRocketAmmoUI(BasePlayer player, APCController controller);
    private static void DestroyUI(BasePlayer player, string panel);
    private static void DestroyAllUI(BasePlayer player);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Movement Settings")]
        public MovementSettings Movement { get; set; }
        [JsonProperty(PropertyName = "Button Configuration")]
        public ButtonConfiguration Buttons { get; set; }
        [JsonProperty(PropertyName = "Crushable Types")]
        public CrushableTypes Crushables { get; set; }
        [JsonProperty(PropertyName = "Passenger Options")]
        public PassengerOptions Passengers { get; set; }
        [JsonProperty(PropertyName = "Inventory Options")]
        public InventoryOptions Inventory { get; set; }
        [JsonProperty(PropertyName = "Weapon Options")]
        public WeaponOptions Weapons { get; set; }
        public class CrushableTypes
        {
            [JsonProperty(PropertyName = "Can crush buildings")]
            public bool Buildings { get; set; }
            [JsonProperty(PropertyName = "Can crush resources")]
            public bool Resources { get; set; }
            [JsonProperty(PropertyName = "Can crush loot containers")]
            public bool Loot { get; set; }
            [JsonProperty(PropertyName = "Can crush animals")]
            public bool Animals { get; set; }
            [JsonProperty(PropertyName = "Can crush players")]
            public bool Players { get; set; }
            [JsonProperty(PropertyName = "Amount of force required to crush various building grades")]
            public Dictionary<string, float> GradeForce { get; set; }
            [JsonProperty(PropertyName = "Amount of force required to crush external walls")]
            public float WallForce { get; set; }
            [JsonProperty(PropertyName = "Amount of force required to crush resources")]
            public float ResourceForce { get; set; }
        }

        public class ButtonConfiguration
        {
            [JsonProperty(PropertyName = "Enter/Exit vehicle")]
            public string Enter { get; set; }
            [JsonProperty(PropertyName = "Toggle light")]
            public string Lights { get; set; }
            [JsonProperty(PropertyName = "Open inventory")]
            public string Inventory { get; set; }
            [JsonProperty(PropertyName = "Speed boost")]
            public string Boost { get; set; }
            [JsonProperty(PropertyName = "Fire Cannon")]
            public string Cannon { get; set; }
            [JsonProperty(PropertyName = "Fire Coaxial Gun")]
            public string Coax { get; set; }
            [JsonProperty(PropertyName = "Fire MG")]
            public string MG { get; set; }
        }

        public class MovementSettings
        {
            [JsonProperty(PropertyName = "Forward torque (nm)")]
            public float ForwardTorque { get; set; }
            [JsonProperty(PropertyName = "Rotation torque (nm)")]
            public float TurnTorque { get; set; }
            [JsonProperty(PropertyName = "Brake torque (nm)")]
            public float BrakeTorque { get; set; }
            [JsonProperty(PropertyName = "Time to reach maximum acceleration (seconds)")]
            public float Acceleration { get; set; }
            [JsonProperty(PropertyName = "Boost torque (nm)")]
            public float BoostTorque { get; set; }
        }

        public class PassengerOptions
        {
            [JsonProperty(PropertyName = "Allow passengers")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Number of allowed passengers (Max 4)")]
            public int Max { get; set; }
            [JsonProperty(PropertyName = "Require passenger to be a friend (FriendsAPI)")]
            public bool UseFriends { get; set; }
            [JsonProperty(PropertyName = "Require passenger to be a clan mate (Clans)")]
            public bool UseClans { get; set; }
        }

        public class InventoryOptions
        {
            [JsonProperty(PropertyName = "Enable inventory system")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Drop inventory on death")]
            public bool DropInv { get; set; }
            [JsonProperty(PropertyName = "Drop loot on death")]
            public bool DropLoot { get; set; }
            [JsonProperty(PropertyName = "Inventory size (max 36)")]
            public int Size { get; set; }
        }

        public class WeaponOptions
        {
            [JsonProperty(PropertyName = "Cannon")]
            public WeaponSystem Cannon { get; set; }
            [JsonProperty(PropertyName = "Coaxial")]
            public WeaponSystem Coax { get; set; }
            [JsonProperty(PropertyName = "Machine Gun")]
            public WeaponSystem MG { get; set; }
            [JsonProperty(PropertyName = "Enable Crosshair")]
            public bool EnableCrosshair { get; set; }
            [JsonProperty(PropertyName = "Crosshair Color")]
            public SerializedColor CrosshairColor { get; set; }
            [JsonProperty(PropertyName = "Crosshair Size")]
            public int CrosshairSize { get; set; }
            public class SerializedColor
            {
                public float R { get; set; }
                public float G { get; set; }
                public float B { get; set; }
                public float A { get; set; }
                private Color _color;
                private bool _isInit;
                public SerializedColor(float r, float g, float b, float a);
                [JsonIgnore]
                public Color Color { get; set; }
            }

            public class WeaponSystem
            {
                [JsonProperty(PropertyName = "Enable weapon system")]
                public bool Enabled { get; set; }
                [JsonProperty(PropertyName = "Require ammunition in inventory")]
                public bool RequireAmmo { get; set; }
                [JsonProperty(PropertyName = "Ammunition type (item shortname)")]
                public string Type { get; set; }
                [JsonProperty(PropertyName = "Fire rate (seconds)")]
                public float Interval { get; set; }
                [JsonProperty(PropertyName = "Aim cone (smaller number is more accurate)")]
                public float Accuracy { get; set; }
                [JsonProperty(PropertyName = "Damage")]
                public float Damage { get; set; }
            }

        }

        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private static string msg(string key, string playerId);
    private Dictionary<string, string> Messages;
}

private class APCController : MonoBehaviour
{
    public BradleyAPC entity;
    private Rigidbody rigidBody;
    private BaseMountable[] mountables;
    private WheelCollider[] leftWheels;
    private WheelCollider[] rightWheels;
    public ItemContainer inventory;
    private float accelTimeTaken;
    private float accelTimeToTake;
    private float forwardTorque;
    private float maxBrakeTorque;
    private float turnTorque;
    private float lastFireCannon;
    private float lastFireMG;
    private float lastFireCoax;
    private bool isDying;
    private RaycastHit eyeRay;
    private Vector3 mouseInput;
    private Vector3 aimVector;
    private Vector3 aimVectorTop;
    private Dictionary<CommandType, BUTTON> controlButtons;
    private ConfigData.WeaponOptions.WeaponSystem cannon;
    private ConfigData.WeaponOptions.WeaponSystem mg;
    private ConfigData.WeaponOptions.WeaponSystem coax;
    private ConfigData.CrushableTypes crushables;
    public bool HasCommander { get; set; }
    public BasePlayer Commander { get; set; }
    private void Awake();
    private void OnDestroy();
    private void Update();
    private void LateUpdate();
    private void OnCollisionEnter(Collision collision);
    private float CalculateImpactForce(Collision col);
    private bool IsPassenger(BasePlayer player);
    private List<KeyValuePair<Vector3, Vector3>> mountOffsets;
    private void CreateMountPoints();
    private void CreateMountPoint(int index);
    public bool CanMountPlayer();
    public void MountPlayer(BasePlayer player);
    public void DismountPlayer(BasePlayer player);
    private void DismountAll();
    private void OnEntityMounted(BasePlayer player, bool isOperator);
    private void SetInitialAimDirection();
    private void DoWeaponControls();
    private void AdjustAiming();
    private const float DDRAW_UPDATE_TIME;
    private void DrawTargeting();
    private void FireCannon();
    private void FireCoax();
    private void FireMG();
    private void FireSideGuns();
    private void ApplyDamage(BaseCombatEntity hitEntity, float damage, Vector3 point, Vector3 normal);
    private void DoMovementControls();
    private void SetThrottleSpeed(float acceleration, float steering, bool boost);
    private void ApplyBrakes(float amount);
    private void ApplyBrakeTorque(float amount, bool rightSide);
    private void ApplyMotorTorque(float torque, bool rightSide);
    private void ToggleLights();
    public void ManageDamage(HitInfo info);
    private void OnDeath();
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

private class ConfigData
{
    [JsonProperty(PropertyName = "Movement Settings")]
    public MovementSettings Movement { get; set; }
    [JsonProperty(PropertyName = "Button Configuration")]
    public ButtonConfiguration Buttons { get; set; }
    [JsonProperty(PropertyName = "Crushable Types")]
    public CrushableTypes Crushables { get; set; }
    [JsonProperty(PropertyName = "Passenger Options")]
    public PassengerOptions Passengers { get; set; }
    [JsonProperty(PropertyName = "Inventory Options")]
    public InventoryOptions Inventory { get; set; }
    [JsonProperty(PropertyName = "Weapon Options")]
    public WeaponOptions Weapons { get; set; }
    public class CrushableTypes
    {
        [JsonProperty(PropertyName = "Can crush buildings")]
        public bool Buildings { get; set; }
        [JsonProperty(PropertyName = "Can crush resources")]
        public bool Resources { get; set; }
        [JsonProperty(PropertyName = "Can crush loot containers")]
        public bool Loot { get; set; }
        [JsonProperty(PropertyName = "Can crush animals")]
        public bool Animals { get; set; }
        [JsonProperty(PropertyName = "Can crush players")]
        public bool Players { get; set; }
        [JsonProperty(PropertyName = "Amount of force required to crush various building grades")]
        public Dictionary<string, float> GradeForce { get; set; }
        [JsonProperty(PropertyName = "Amount of force required to crush external walls")]
        public float WallForce { get; set; }
        [JsonProperty(PropertyName = "Amount of force required to crush resources")]
        public float ResourceForce { get; set; }
    }

    public class ButtonConfiguration
    {
        [JsonProperty(PropertyName = "Enter/Exit vehicle")]
        public string Enter { get; set; }
        [JsonProperty(PropertyName = "Toggle light")]
        public string Lights { get; set; }
        [JsonProperty(PropertyName = "Open inventory")]
        public string Inventory { get; set; }
        [JsonProperty(PropertyName = "Speed boost")]
        public string Boost { get; set; }
        [JsonProperty(PropertyName = "Fire Cannon")]
        public string Cannon { get; set; }
        [JsonProperty(PropertyName = "Fire Coaxial Gun")]
        public string Coax { get; set; }
        [JsonProperty(PropertyName = "Fire MG")]
        public string MG { get; set; }
    }

    public class MovementSettings
    {
        [JsonProperty(PropertyName = "Forward torque (nm)")]
        public float ForwardTorque { get; set; }
        [JsonProperty(PropertyName = "Rotation torque (nm)")]
        public float TurnTorque { get; set; }
        [JsonProperty(PropertyName = "Brake torque (nm)")]
        public float BrakeTorque { get; set; }
        [JsonProperty(PropertyName = "Time to reach maximum acceleration (seconds)")]
        public float Acceleration { get; set; }
        [JsonProperty(PropertyName = "Boost torque (nm)")]
        public float BoostTorque { get; set; }
    }

    public class PassengerOptions
    {
        [JsonProperty(PropertyName = "Allow passengers")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Number of allowed passengers (Max 4)")]
        public int Max { get; set; }
        [JsonProperty(PropertyName = "Require passenger to be a friend (FriendsAPI)")]
        public bool UseFriends { get; set; }
        [JsonProperty(PropertyName = "Require passenger to be a clan mate (Clans)")]
        public bool UseClans { get; set; }
    }

    public class InventoryOptions
    {
        [JsonProperty(PropertyName = "Enable inventory system")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Drop inventory on death")]
        public bool DropInv { get; set; }
        [JsonProperty(PropertyName = "Drop loot on death")]
        public bool DropLoot { get; set; }
        [JsonProperty(PropertyName = "Inventory size (max 36)")]
        public int Size { get; set; }
    }

    public class WeaponOptions
    {
        [JsonProperty(PropertyName = "Cannon")]
        public WeaponSystem Cannon { get; set; }
        [JsonProperty(PropertyName = "Coaxial")]
        public WeaponSystem Coax { get; set; }
        [JsonProperty(PropertyName = "Machine Gun")]
        public WeaponSystem MG { get; set; }
        [JsonProperty(PropertyName = "Enable Crosshair")]
        public bool EnableCrosshair { get; set; }
        [JsonProperty(PropertyName = "Crosshair Color")]
        public SerializedColor CrosshairColor { get; set; }
        [JsonProperty(PropertyName = "Crosshair Size")]
        public int CrosshairSize { get; set; }
        public class SerializedColor
        {
            public float R { get; set; }
            public float G { get; set; }
            public float B { get; set; }
            public float A { get; set; }
            private Color _color;
            private bool _isInit;
            public SerializedColor(float r, float g, float b, float a);
            [JsonIgnore]
            public Color Color { get; set; }
        }

        public class WeaponSystem
        {
            [JsonProperty(PropertyName = "Enable weapon system")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Require ammunition in inventory")]
            public bool RequireAmmo { get; set; }
            [JsonProperty(PropertyName = "Ammunition type (item shortname)")]
            public string Type { get; set; }
            [JsonProperty(PropertyName = "Fire rate (seconds)")]
            public float Interval { get; set; }
            [JsonProperty(PropertyName = "Aim cone (smaller number is more accurate)")]
            public float Accuracy { get; set; }
            [JsonProperty(PropertyName = "Damage")]
            public float Damage { get; set; }
        }

    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class CrushableTypes
{
    [JsonProperty(PropertyName = "Can crush buildings")]
    public bool Buildings { get; set; }
    [JsonProperty(PropertyName = "Can crush resources")]
    public bool Resources { get; set; }
    [JsonProperty(PropertyName = "Can crush loot containers")]
    public bool Loot { get; set; }
    [JsonProperty(PropertyName = "Can crush animals")]
    public bool Animals { get; set; }
    [JsonProperty(PropertyName = "Can crush players")]
    public bool Players { get; set; }
    [JsonProperty(PropertyName = "Amount of force required to crush various building grades")]
    public Dictionary<string, float> GradeForce { get; set; }
    [JsonProperty(PropertyName = "Amount of force required to crush external walls")]
    public float WallForce { get; set; }
    [JsonProperty(PropertyName = "Amount of force required to crush resources")]
    public float ResourceForce { get; set; }
}

public class ButtonConfiguration
{
    [JsonProperty(PropertyName = "Enter/Exit vehicle")]
    public string Enter { get; set; }
    [JsonProperty(PropertyName = "Toggle light")]
    public string Lights { get; set; }
    [JsonProperty(PropertyName = "Open inventory")]
    public string Inventory { get; set; }
    [JsonProperty(PropertyName = "Speed boost")]
    public string Boost { get; set; }
    [JsonProperty(PropertyName = "Fire Cannon")]
    public string Cannon { get; set; }
    [JsonProperty(PropertyName = "Fire Coaxial Gun")]
    public string Coax { get; set; }
    [JsonProperty(PropertyName = "Fire MG")]
    public string MG { get; set; }
}

public class MovementSettings
{
    [JsonProperty(PropertyName = "Forward torque (nm)")]
    public float ForwardTorque { get; set; }
    [JsonProperty(PropertyName = "Rotation torque (nm)")]
    public float TurnTorque { get; set; }
    [JsonProperty(PropertyName = "Brake torque (nm)")]
    public float BrakeTorque { get; set; }
    [JsonProperty(PropertyName = "Time to reach maximum acceleration (seconds)")]
    public float Acceleration { get; set; }
    [JsonProperty(PropertyName = "Boost torque (nm)")]
    public float BoostTorque { get; set; }
}

public class PassengerOptions
{
    [JsonProperty(PropertyName = "Allow passengers")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Number of allowed passengers (Max 4)")]
    public int Max { get; set; }
    [JsonProperty(PropertyName = "Require passenger to be a friend (FriendsAPI)")]
    public bool UseFriends { get; set; }
    [JsonProperty(PropertyName = "Require passenger to be a clan mate (Clans)")]
    public bool UseClans { get; set; }
}

public class InventoryOptions
{
    [JsonProperty(PropertyName = "Enable inventory system")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Drop inventory on death")]
    public bool DropInv { get; set; }
    [JsonProperty(PropertyName = "Drop loot on death")]
    public bool DropLoot { get; set; }
    [JsonProperty(PropertyName = "Inventory size (max 36)")]
    public int Size { get; set; }
}

public class WeaponOptions
{
    [JsonProperty(PropertyName = "Cannon")]
    public WeaponSystem Cannon { get; set; }
    [JsonProperty(PropertyName = "Coaxial")]
    public WeaponSystem Coax { get; set; }
    [JsonProperty(PropertyName = "Machine Gun")]
    public WeaponSystem MG { get; set; }
    [JsonProperty(PropertyName = "Enable Crosshair")]
    public bool EnableCrosshair { get; set; }
    [JsonProperty(PropertyName = "Crosshair Color")]
    public SerializedColor CrosshairColor { get; set; }
    [JsonProperty(PropertyName = "Crosshair Size")]
    public int CrosshairSize { get; set; }
    public class SerializedColor
    {
        public float R { get; set; }
        public float G { get; set; }
        public float B { get; set; }
        public float A { get; set; }
        private Color _color;
        private bool _isInit;
        public SerializedColor(float r, float g, float b, float a);
        [JsonIgnore]
        public Color Color { get; set; }
    }

    public class WeaponSystem
    {
        [JsonProperty(PropertyName = "Enable weapon system")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Require ammunition in inventory")]
        public bool RequireAmmo { get; set; }
        [JsonProperty(PropertyName = "Ammunition type (item shortname)")]
        public string Type { get; set; }
        [JsonProperty(PropertyName = "Fire rate (seconds)")]
        public float Interval { get; set; }
        [JsonProperty(PropertyName = "Aim cone (smaller number is more accurate)")]
        public float Accuracy { get; set; }
        [JsonProperty(PropertyName = "Damage")]
        public float Damage { get; set; }
    }

}

public class SerializedColor
{
    public float R { get; set; }
    public float G { get; set; }
    public float B { get; set; }
    public float A { get; set; }
    private Color _color;
    private bool _isInit;
    public SerializedColor(float r, float g, float b, float a);
    [JsonIgnore]
    public Color Color { get; set; }
}

public class WeaponSystem
{
    [JsonProperty(PropertyName = "Enable weapon system")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Require ammunition in inventory")]
    public bool RequireAmmo { get; set; }
    [JsonProperty(PropertyName = "Ammunition type (item shortname)")]
    public string Type { get; set; }
    [JsonProperty(PropertyName = "Fire rate (seconds)")]
    public float Interval { get; set; }
    [JsonProperty(PropertyName = "Aim cone (smaller number is more accurate)")]
    public float Accuracy { get; set; }
    [JsonProperty(PropertyName = "Damage")]
    public float Damage { get; set; }
}


```

---

## TargetPractice

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Core;
using System.Linq;
using Oxide.Game.Rust.Cui;
using System.Reflection;

Oxide.Plugins
[Info("TargetPractice", "k1lly0u", "0.1.55", ResourceId = 1731)]
 class TargetPractice : RustPlugin
{
     TargetData shotData;
    private DynamicConfigFile ShotData;
    private static Vector2 position;
    private static Vector2 dimension;
    private Dictionary<ulong, PlayerMSG> currentHits;
     FieldInfo knockdownMaxValue;
     void Loaded();
     void OnServerInitialized();
    protected override void LoadDefaultConfig();
     void Unload();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
    private string FormatWeapon(Item weapon);
     void OnEntitySpawned(BaseNetworkable entity);
    private void ModifyTargetStats(BaseNetworkable entity);
    private void OverwriteDuplicate(BasePlayer player);
    private void GetMessage(BasePlayer player);
    private void CheckPlayerData(BasePlayer player);
    protected void LoadTargets();
    private float GetPlayerDistance(Vector3 targetPos, Vector3 attackerPos);
    private void BroadcastToAll(string name, string range, string weapon);
     class TPUI : MonoBehaviour
    {
        public List<int> slots;
         int i;
        private BasePlayer player;
         void Awake();
        public static TPUI GetPlayer(BasePlayer player);
        private int FindSlot();
        public void UseUI(string msg);
    }

    private void DestroyNotification(BasePlayer player, string msgNum, int slot);
    private void DestroyHitMsg(BasePlayer player, string msgNum, int slot, float duration);
    [ChatCommand("target")]
     void cmdTarget(BasePlayer player, string command, string[] args);
     bool isAuth(BasePlayer player);
     class TargetData
    {
        public Dictionary<ulong, TargetInfo> longShot;
        public BestHit bestHit;
        public TargetData();
    }

     class BestHit
    {
        public string Name;
        public float Range;
        public string Weapon;
        public bool isBullseye;
    }

     class TargetInfo
    {
        public string Name;
        public float Range;
        public float Bullseye;
        public string Weapon;
        public bool UseUI;
        public int PopupTime;
    }

     class PlayerMSG
    {
        public string msg;
        public float distance;
        public string weapon;
        public bool time;
    }

     void SaveLoop();
     void SaveData();
     void LoadData();
    private bool changed;
    private static int popupTime;
    private static int msgDuration;
    private static int fontSize;
    private static int saveTimer;
    private static int maxUIMsg;
    private static bool broadcastNewScore;
    private static bool disableFlamer;
    private static string fontColor1;
    private static string fontColor2;
    private static float maxKnockdown;
    private static float UIspacing;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     Dictionary<string, string> messages;
}

 class TPUI : MonoBehaviour
{
    public List<int> slots;
     int i;
    private BasePlayer player;
     void Awake();
    public static TPUI GetPlayer(BasePlayer player);
    private int FindSlot();
    public void UseUI(string msg);
}

 class TargetData
{
    public Dictionary<ulong, TargetInfo> longShot;
    public BestHit bestHit;
    public TargetData();
}

 class BestHit
{
    public string Name;
    public float Range;
    public string Weapon;
    public bool isBullseye;
}

 class TargetInfo
{
    public string Name;
    public float Range;
    public float Bullseye;
    public string Weapon;
    public bool UseUI;
    public int PopupTime;
}

 class PlayerMSG
{
    public string msg;
    public float distance;
    public string weapon;
    public bool time;
}


```

---

## TeamBattlefield

```csharp
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Reflection;
using System;

Oxide.Plugins
[Info("TeamBattlefield", "BodyweightEnergy / k1lly0u", "2.1.2", ResourceId = 1330)]
 class TeamBattlefield : RustPlugin
{
    [PluginReference]
     Plugin Spawns;
    readonly MethodInfo entitySnapshot;
    private List<TBPlayer> TBPlayers;
    private Dictionary<ulong, PlayerData> DCPlayers;
    private Dictionary<ulong, Timer> DCTimers;
    private bool UseTB;
    private int TeamA_Score;
    private int TeamB_Score;
    private const string UIMain;
    private const string UIScoreboard;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    }

    private void OpenTeamSelection(BasePlayer player);
    public void Scoreboard(BasePlayer player);
     void OnServerInitialized();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
    private void RefreshScoreboard();
    private void OnPlayerInit(BasePlayer player);
    private void InitPlayer(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void DestroyPlayer(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private object OnPlayerChat(ConsoleSystem.Arg arg);
     void Unload();
    private bool CheckDependencies();
    private bool CheckSpawnfiles();
    static void MovePlayerPosition(BasePlayer player, Vector3 destination);
    private void StartSpectating(BasePlayer player, BasePlayer target);
    private void EndSpectating(BasePlayer player);
    private void AddPoints(BasePlayer player, BasePlayer victim);
    private void GivePlayerWeapons(BasePlayer player);
    private void GivePlayerGear(BasePlayer player, Team team);
    private Item BuildItem(string shortname, int amount, ulong skin);
    private Item BuildWeapon(Weapon newWeapon);
    public void GiveItem(BasePlayer player, Item item, string container);
    [ConsoleCommand("tbf.list")]
    private void cmdList(ConsoleSystem.Arg arg);
    [ConsoleCommand("tbf.clearscore")]
    private void cmdClearscore(ConsoleSystem.Arg arg);
    [ConsoleCommand("tbf.assign")]
    private void cmdAssign(ConsoleSystem.Arg arg);
    [ConsoleCommand("tbf.version")]
    private void cmdVersion(ConsoleSystem.Arg arg);
    [ConsoleCommand("tbf.help")]
    private void cmdHelp(ConsoleSystem.Arg arg);
    [ConsoleCommand("tbf.purge")]
    private void cmdPurge(ConsoleSystem.Arg arg);
    [ChatCommand("switchteam")]
    private void cmdChangeTeam(BasePlayer player, string command, string[] args);
     bool isAuth(ConsoleSystem.Arg arg);
    [ChatCommand("t")]
    private void cmdTeamChat(BasePlayer player, string command, string[] args);
    [ConsoleCommand("TBUI_TeamSelect")]
    private void cmdTeamSelectA(ConsoleSystem.Arg arg);
    private Team ConvertStringToTeam(string team);
    private List<BasePlayer> FindPlayer(string arg);
    private int CountPlayers(Team team);
    private void AssignPlayerToTeam(BasePlayer player, Team team);
    private BasePlayer GetRandomTeammate(BasePlayer player);
     string GetPlayerTeam(ulong playerID);
     Dictionary<ulong, string> GetTeams();
    private ConfigData configData;
     class TeamOptions
    {
        public string Spawnfile { get; set; }
        public string Chat_Prefix { get; set; }
        public string Chat_Color { get; set; }
        public List<Gear> Gear { get; set; }
    }

     class Options
    {
        public int MaximumTeamCountDifference { get; set; }
        public int RemoveSleeper_Timer { get; set; }
        public float FF_DamageScale { get; set; }
        public bool UsePluginChatControl { get; set; }
        public bool BroadcastDeath { get; set; }
    }

     class GUI
    {
        public float XPosition { get; set; }
        public float YPosition { get; set; }
        public float XDimension { get; set; }
        public float YDimension { get; set; }
    }

     class ConfigGear
    {
        public List<Gear> CommonGear { get; set; }
        public List<Weapon> StartingWeapons { get; set; }
    }

     class Spectators
    {
        public bool EnableSpectators { get; set; }
        public string Chat_Color { get; set; }
        public string Chat_Prefix { get; set; }
    }

     class ConfigData
    {
        public TeamOptions TeamA { get; set; }
        public TeamOptions TeamB { get; set; }
        public TeamOptions Admin { get; set; }
        public ConfigGear Gear { get; set; }
        public Options Options { get; set; }
        public Spectators Spectators { get; set; }
        public GUI ScoreboardUI { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     class TBPlayer : MonoBehaviour
    {
        public BasePlayer player;
        public int kills;
        public Team team;
         void Awake();
    }

     class PlayerData
    {
        public int kills;
        public Team team;
    }

     class Gear
    {
        public string name;
        public string shortname;
        public ulong skin;
        public int amount;
        public string container;
    }

     class Weapon
    {
        public string name;
        public string shortname;
        public ulong skin;
        public string container;
        public int amount;
        public int ammo;
        public string ammoType;
        public string[] contents;
    }

}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
}

 class TeamOptions
{
    public string Spawnfile { get; set; }
    public string Chat_Prefix { get; set; }
    public string Chat_Color { get; set; }
    public List<Gear> Gear { get; set; }
}

 class Options
{
    public int MaximumTeamCountDifference { get; set; }
    public int RemoveSleeper_Timer { get; set; }
    public float FF_DamageScale { get; set; }
    public bool UsePluginChatControl { get; set; }
    public bool BroadcastDeath { get; set; }
}

 class GUI
{
    public float XPosition { get; set; }
    public float YPosition { get; set; }
    public float XDimension { get; set; }
    public float YDimension { get; set; }
}

 class ConfigGear
{
    public List<Gear> CommonGear { get; set; }
    public List<Weapon> StartingWeapons { get; set; }
}

 class Spectators
{
    public bool EnableSpectators { get; set; }
    public string Chat_Color { get; set; }
    public string Chat_Prefix { get; set; }
}

 class ConfigData
{
    public TeamOptions TeamA { get; set; }
    public TeamOptions TeamB { get; set; }
    public TeamOptions Admin { get; set; }
    public ConfigGear Gear { get; set; }
    public Options Options { get; set; }
    public Spectators Spectators { get; set; }
    public GUI ScoreboardUI { get; set; }
}

 class TBPlayer : MonoBehaviour
{
    public BasePlayer player;
    public int kills;
    public Team team;
     void Awake();
}

 class PlayerData
{
    public int kills;
    public Team team;
}

 class Gear
{
    public string name;
    public string shortname;
    public ulong skin;
    public int amount;
    public string container;
}

 class Weapon
{
    public string name;
    public string shortname;
    public ulong skin;
    public string container;
    public int amount;
    public int ammo;
    public string ammoType;
    public string[] contents;
}


```

---

## TeamDeathmatch

```csharp
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;

Oxide.Plugins
[Info("Team Deathmatch", "k1lly0u", "0.2.21", ResourceId = 1484)]
 class TeamDeathmatch : RustPlugin
{
    [PluginReference]
     EventManager EventManager;
    [PluginReference]
     Plugin Spawns;
    private bool UseTDM;
    private bool Started;
    private bool Changed;
    public string Kit;
    public int TeamAKills;
    public int TeamBKills;
    private List<TDMPlayer> TDMPlayers;
    private ConfigData configData;
     void Loaded();
     void OnServerInitialized();
     void RegisterGame();
     void Unload();
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
     void OnSelectEventGamePost(string name);
     void OnEventPlayerSpawn(BasePlayer player);
    private void GiveTeamGear(BasePlayer player);
    private void GiveTeamShirts(BasePlayer player);
     object OnSelectSpawnFile(string name);
    private object CanEventOpen();
    private bool CheckSpawnfiles();
     object CanEventStart();
     object OnEventOpenPost();
     object OnEventClosePost();
     object OnEventEndPre();
     void OnPlayerSelectClass(BasePlayer player);
     object OnEventCancel();
     object OnEventEndPost();
     object OnEventStartPre();
     object OnEventStartPost();
     object CanEventJoin(BasePlayer player);
     object OnSelectKit(string kitname);
     object OnEventJoinPost(BasePlayer player);
     object OnEventLeavePost(BasePlayer player);
     void OnEventPlayerAttack(BasePlayer attacker, HitInfo hitinfo);
     void OnEventPlayerDeath(BasePlayer vic, HitInfo hitinfo);
    private void RefreshSB();
     object EventChooseSpawn(BasePlayer player, Vector3 destination);
     object OnRequestZoneName();
    private bool CheckForTeam(BasePlayer player);
    private void TeamAssign(BasePlayer player);
    private string GetTeamName(Team team, BasePlayer player);
    private Team CountForBalance();
    private int Count(Team team);
    private void CreateScoreboard(BasePlayer player);
    private void DestroyUI(BasePlayer player);
    private void MessagePlayers(string msg);
     List<TDMPlayer> FindPlayer(string arg);
    [ConsoleCommand("tdm.spawns.a")]
     void ccmdSpawnsA(ConsoleSystem.Arg arg);
    [ConsoleCommand("tdm.spawns.b")]
     void ccmdSpawnsB(ConsoleSystem.Arg arg);
    [ConsoleCommand("tdm.kills")]
     void ccmdKills(ConsoleSystem.Arg arg);
    [ConsoleCommand("tdm.team")]
    private void cmdTeam(ConsoleSystem.Arg arg);
     bool isAuth(ConsoleSystem.Arg arg);
     void AddKill(BasePlayer player, BasePlayer victim);
     void CheckScores(bool timelimitreached);
     void Winner(Team winner);
     class TDMPlayer : MonoBehaviour
    {
        public BasePlayer player;
        public Team team;
         void Awake();
    }

     class ConfigData
    {
        public string DefaultKit { get; set; }
        public string EventName { get; set; }
        public string TeamA_Spawnfile { get; set; }
        public string TeamB_Spawnfile { get; set; }
        public string TeamA_Color { get; set; }
        public string TeamB_Color { get; set; }
        public string TeamA_Shirt { get; set; }
        public string TeamB_Shirt { get; set; }
        public int TeamA_SkinID { get; set; }
        public int TeamB_SkinID { get; set; }
        public float GUIPosX { get; set; }
        public float GUIPosY { get; set; }
        public float GUIDimX { get; set; }
        public float GUIDimY { get; set; }
        public int GUI_TextSize { get; set; }
        public float FF_Damage_Modifier { get; set; }
        public float StartingHealth { get; set; }
        public int KillLimit { get; set; }
        public int Tokens_Kill { get; set; }
        public int Tokens_Win { get; set; }
        public string ZoneName { get; set; }
    }

    private void LoadVariables();
    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
     object GetEventConfig(string configname);
    private string TitleM();
     Dictionary<string, string> messages;
}

 class TDMPlayer : MonoBehaviour
{
    public BasePlayer player;
    public Team team;
     void Awake();
}

 class ConfigData
{
    public string DefaultKit { get; set; }
    public string EventName { get; set; }
    public string TeamA_Spawnfile { get; set; }
    public string TeamB_Spawnfile { get; set; }
    public string TeamA_Color { get; set; }
    public string TeamB_Color { get; set; }
    public string TeamA_Shirt { get; set; }
    public string TeamB_Shirt { get; set; }
    public int TeamA_SkinID { get; set; }
    public int TeamB_SkinID { get; set; }
    public float GUIPosX { get; set; }
    public float GUIPosY { get; set; }
    public float GUIDimX { get; set; }
    public float GUIDimY { get; set; }
    public int GUI_TextSize { get; set; }
    public float FF_Damage_Modifier { get; set; }
    public float StartingHealth { get; set; }
    public int KillLimit { get; set; }
    public int Tokens_Kill { get; set; }
    public int Tokens_Win { get; set; }
    public string ZoneName { get; set; }
}


```

---

## TeamGuard

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.Serialization;
using Oxide.Core;
using UnityEngine;
using Formatter = Oxide.Core.Libraries.Covalence.Formatter;

Oxide.Plugins
[Info("TeamGuard", "RustShop.ru -Vlad-00003", "2.1.4")]
[Description("Plugin allow admins to easier control player groups size.")]
internal class TeamGuard : RustPlugin
{
    private class TGChecker : TriggerBase
    {
        private readonly HashSet<BasePlayer> _players;
        private SphereCollider _collider;
        private Info _info;
        private bool _mainCuiCreated;
        private BasePlayer _player;
        private ChatMessage Message { get; set; }
        private void Awake();
        private TGChecker SetPlayer(BasePlayer player);
        private void InvokeRepeating(Action action, float time);
        private bool ShouldCount(BasePlayer player);
        private IEnumerator DoShock();
        public void Kill();
        private void FixedUpdate();
        private void Update();
        private void CuiSequence();
        private void ChatSequence();
        private void DamageSequence();
        public static TGChecker Create(BasePlayer player);
        private void ShowCui(string text);
        private void DestroyCui();
        public override GameObject InterestedInObject(GameObject obj);
        public override void OnEntityEnter(BaseEntity ent);
        public override void OnEntityLeave(BaseEntity ent);
    }

    private static PluginConfig _config;
    private static TeamGuard _inst;
    [PluginReference]
     Plugin Duel;
     Plugin EventManager;
     Plugin ZoneManager;
    private readonly Dictionary<BasePlayer, uint> _lastUsed;
    private readonly Dictionary<BasePlayer, TGChecker> _checkers;
    private class ChatMessage
    {
        public static readonly ChatMessage CodeLockName;
        public static readonly ChatMessage CupboardName;
        public static readonly ChatMessage TurretName;
        public static readonly ChatMessage ClearMode;
        public static readonly ChatMessage ReplaceMode;
        private readonly object[] _args;
        private readonly string _langKey;
        public ChatMessage(string langKey);
        public ChatMessage(string langKey, object[] args);
        public string Read(string userId);
        public void SendToChat(BasePlayer player);
        public void SendToConsole(BasePlayer player);
    }

    private static string GetMsg(string langKey, string userid);
    protected override void LoadDefaultMessages();
    private class RadiusCheckConfig
    {
        [JsonProperty("Частота сообщений в чате")]
        public float ChatUpdateFrequency;
        [JsonProperty("Частота обновления UI")]
        public float CuiUpdateFrequency;
        [JsonProperty("Частота нанесения урона")]
        public float DamageFrequency;
        [JsonProperty("Наносимый урон за раз")]
        public float DamagePerTime;
        [JsonProperty("Разрешённое время нахождения рядом")]
        public float DelayBeforeShock;
        [JsonProperty("Отключить проверку по радиусу в безопасных зонах")]
        public bool DisableInSaveZone;
        [JsonProperty("Список эффектов, запускающихся при получении урона")]
        public List<string> Effects;
        [JsonProperty("Максимальное количество игроков в радиусе")]
        public int MaxPlayers;
        [JsonProperty("Радиус зоны проверки")]
        public float Radius;
        [JsonProperty("Использовать ли проверку по радиусу")]
        public bool Use;
        [JsonProperty("Выводить ли сообщения о нанесении урона в чат?")]
        public bool UseChat;
        [JsonProperty("Использовать ли графическую панель?")]
        public bool UseCui;
        [JsonIgnore]
        public string DamageFrequencyString;
        public static RadiusCheckConfig DefaultConfig { get; set; }
        [OnDeserialized]
        internal void OnDeserializedMethod(StreamingContext context);
    }

    private class GenericConfig
    {
        [JsonProperty("Формат сообщений в чате")]
        private string _chatFormat;
        [JsonIgnore]
        public string ChatFormat;
        [JsonProperty("Привилегия для просмотра логов")]
        public string LogPermission;
        [JsonProperty("Выводить лог в чат (false - в консоль)")]
        public bool LogToChat;
        public static GenericConfig DefaultConfig { get; set; }
        [OnDeserialized]
        internal void OnDeserializedMethod(StreamingContext context);
    }

    private class CuiConfig
    {
        [JsonProperty("Цвет фона")]
        private string _color;
        [JsonProperty("Размер шрифта")]
        private int _fontSize;
        [JsonProperty("Максимальный отступ")]
        private string _max;
        [JsonProperty("Минимальный отступ")]
        private string _min;
        [JsonIgnore]
        private readonly string _textPanelName;
        [JsonIgnore]
        private readonly string _mainPanelName;
        public static CuiConfig DefaultConfig { get; set; }
        private static readonly CuiElementContainer ReusableContainer;
        [JsonIgnore]
        private CuiElement MainPanel { get; set; }
        private CuiElement GetTextPanel(string format, object[] args);
        public void AddMain(BasePlayer player);
        public void UpdateText(BasePlayer player, string format, object[] args);
        public void Destroy(BasePlayer player);
    }

    private class AdminsConfig
    {
        [JsonProperty("Необходимый уровень AuthLevel для игнорирования")]
        public int AuthLevel;
        [JsonProperty("Привилегия для игнорирования при проверке")]
        public string IgnorePermission;
        [JsonProperty("Игнорировать администраторов при проверке?")]
        public bool IgnoreAdmins;
        public static AdminsConfig DefaultConfig { get; set; }
    }

    private class EntityCheckConfig
    {
        [JsonProperty("Очищать список при превышении лимита")]
        public bool ClearAll;
        [JsonProperty("Максимум авторизаций")]
        public int PlayersLimit;
        [JsonProperty("Проверять авторизации")]
        public bool Use;
        public static EntityCheckConfig DefaultConfig { get; set; }
        public ChatMessage GetLimitMessage(ChatMessage entityName);
    }

    private class PluginConfig
    {
        [JsonProperty("Общие Настройки")]
        public GenericConfig Generic;
        [JsonProperty("Проверка администраторов")]
        public AdminsConfig Admins;
        [JsonProperty("Проверка игроков по радиусу")]
        public RadiusCheckConfig RadiusCheck;
        [JsonProperty("Настройки GUI")]
        public CuiConfig Cui;
        [JsonProperty("Авторизации в кодовых замках")]
        public EntityCheckConfig CodeLock;
        [JsonProperty("Авторизации в шкафах")]
        public EntityCheckConfig Cupboard;
        [JsonProperty("Авторизации в турелях")]
        public EntityCheckConfig Turret;
        [JsonProperty("Список зон ZoneManager, в которых не нужно вести проверку")]
        private List<string> _zonesList;
        public static PluginConfig DefaultConfig { get; set; }
        public bool InZone(Plugin zoneManager, BasePlayer player);
        public void Register(TeamGuard plugin);
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private bool ShouldUpdateConfig();
    private bool ConfigExists(string[] path);
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private object OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
    private object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private object OnTurretAuthorize(AutoTurret turret, BasePlayer player);
    private static bool InSafeZone(BasePlayer player, ulong playerid);
    private static bool IsNpc(BasePlayer player);
    private static bool HasAuth(BasePlayer player);
    private bool HasAuth(ulong playerid);
    private static bool IsDuelPlayer(BasePlayer player);
    private static bool InWhitelistedZone(BasePlayer player);
    private static bool IsEventPlayer(BasePlayer player);
    private static bool CanSee(BasePlayer player, BasePlayer target);
    private string GetName(ulong id);
    private void Log(string langKey, object[] args);
    private static string ToRustColor(string input);
}

private class TGChecker : TriggerBase
{
    private readonly HashSet<BasePlayer> _players;
    private SphereCollider _collider;
    private Info _info;
    private bool _mainCuiCreated;
    private BasePlayer _player;
    private ChatMessage Message { get; set; }
    private void Awake();
    private TGChecker SetPlayer(BasePlayer player);
    private void InvokeRepeating(Action action, float time);
    private bool ShouldCount(BasePlayer player);
    private IEnumerator DoShock();
    public void Kill();
    private void FixedUpdate();
    private void Update();
    private void CuiSequence();
    private void ChatSequence();
    private void DamageSequence();
    public static TGChecker Create(BasePlayer player);
    private void ShowCui(string text);
    private void DestroyCui();
    public override GameObject InterestedInObject(GameObject obj);
    public override void OnEntityEnter(BaseEntity ent);
    public override void OnEntityLeave(BaseEntity ent);
}

private class ChatMessage
{
    public static readonly ChatMessage CodeLockName;
    public static readonly ChatMessage CupboardName;
    public static readonly ChatMessage TurretName;
    public static readonly ChatMessage ClearMode;
    public static readonly ChatMessage ReplaceMode;
    private readonly object[] _args;
    private readonly string _langKey;
    public ChatMessage(string langKey);
    public ChatMessage(string langKey, object[] args);
    public string Read(string userId);
    public void SendToChat(BasePlayer player);
    public void SendToConsole(BasePlayer player);
}

private class RadiusCheckConfig
{
    [JsonProperty("Частота сообщений в чате")]
    public float ChatUpdateFrequency;
    [JsonProperty("Частота обновления UI")]
    public float CuiUpdateFrequency;
    [JsonProperty("Частота нанесения урона")]
    public float DamageFrequency;
    [JsonProperty("Наносимый урон за раз")]
    public float DamagePerTime;
    [JsonProperty("Разрешённое время нахождения рядом")]
    public float DelayBeforeShock;
    [JsonProperty("Отключить проверку по радиусу в безопасных зонах")]
    public bool DisableInSaveZone;
    [JsonProperty("Список эффектов, запускающихся при получении урона")]
    public List<string> Effects;
    [JsonProperty("Максимальное количество игроков в радиусе")]
    public int MaxPlayers;
    [JsonProperty("Радиус зоны проверки")]
    public float Radius;
    [JsonProperty("Использовать ли проверку по радиусу")]
    public bool Use;
    [JsonProperty("Выводить ли сообщения о нанесении урона в чат?")]
    public bool UseChat;
    [JsonProperty("Использовать ли графическую панель?")]
    public bool UseCui;
    [JsonIgnore]
    public string DamageFrequencyString;
    public static RadiusCheckConfig DefaultConfig { get; set; }
    [OnDeserialized]
    internal void OnDeserializedMethod(StreamingContext context);
}

private class GenericConfig
{
    [JsonProperty("Формат сообщений в чате")]
    private string _chatFormat;
    [JsonIgnore]
    public string ChatFormat;
    [JsonProperty("Привилегия для просмотра логов")]
    public string LogPermission;
    [JsonProperty("Выводить лог в чат (false - в консоль)")]
    public bool LogToChat;
    public static GenericConfig DefaultConfig { get; set; }
    [OnDeserialized]
    internal void OnDeserializedMethod(StreamingContext context);
}

private class CuiConfig
{
    [JsonProperty("Цвет фона")]
    private string _color;
    [JsonProperty("Размер шрифта")]
    private int _fontSize;
    [JsonProperty("Максимальный отступ")]
    private string _max;
    [JsonProperty("Минимальный отступ")]
    private string _min;
    [JsonIgnore]
    private readonly string _textPanelName;
    [JsonIgnore]
    private readonly string _mainPanelName;
    public static CuiConfig DefaultConfig { get; set; }
    private static readonly CuiElementContainer ReusableContainer;
    [JsonIgnore]
    private CuiElement MainPanel { get; set; }
    private CuiElement GetTextPanel(string format, object[] args);
    public void AddMain(BasePlayer player);
    public void UpdateText(BasePlayer player, string format, object[] args);
    public void Destroy(BasePlayer player);
}

private class AdminsConfig
{
    [JsonProperty("Необходимый уровень AuthLevel для игнорирования")]
    public int AuthLevel;
    [JsonProperty("Привилегия для игнорирования при проверке")]
    public string IgnorePermission;
    [JsonProperty("Игнорировать администраторов при проверке?")]
    public bool IgnoreAdmins;
    public static AdminsConfig DefaultConfig { get; set; }
}

private class EntityCheckConfig
{
    [JsonProperty("Очищать список при превышении лимита")]
    public bool ClearAll;
    [JsonProperty("Максимум авторизаций")]
    public int PlayersLimit;
    [JsonProperty("Проверять авторизации")]
    public bool Use;
    public static EntityCheckConfig DefaultConfig { get; set; }
    public ChatMessage GetLimitMessage(ChatMessage entityName);
}

private class PluginConfig
{
    [JsonProperty("Общие Настройки")]
    public GenericConfig Generic;
    [JsonProperty("Проверка администраторов")]
    public AdminsConfig Admins;
    [JsonProperty("Проверка игроков по радиусу")]
    public RadiusCheckConfig RadiusCheck;
    [JsonProperty("Настройки GUI")]
    public CuiConfig Cui;
    [JsonProperty("Авторизации в кодовых замках")]
    public EntityCheckConfig CodeLock;
    [JsonProperty("Авторизации в шкафах")]
    public EntityCheckConfig Cupboard;
    [JsonProperty("Авторизации в турелях")]
    public EntityCheckConfig Turret;
    [JsonProperty("Список зон ZoneManager, в которых не нужно вести проверку")]
    private List<string> _zonesList;
    public static PluginConfig DefaultConfig { get; set; }
    public bool InZone(Plugin zoneManager, BasePlayer player);
    public void Register(TeamGuard plugin);
}


```

---

## TeamJoin

```csharp
using CompanionServer.Handlers;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;
using System.Collections.Generic;
using System.Linq;
using System;

Oxide.Plugins
[Info("TeamJoin", "Frizen", "1.0.0")]
public class TeamJoin : RustPlugin
{
    private Locker _locker;
    [JsonProperty(PropertyName = "Def players", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> _defside;
    [JsonProperty(PropertyName = "Admin Position", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Vector3> positionAdmin;
    [JsonProperty(PropertyName = "Players Position", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Vector3> positionplayers;
    [JsonProperty(PropertyName = "Lockers Position", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Vector3> lockerspos;
    [JsonProperty(PropertyName = "Lockers List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public HashSet<BaseEntity> lockersList;
    private RelationshipManager.PlayerTeam TeamPl;
    private RelationshipManager.PlayerTeam TeamAd;
     void OnServerInitialized();
     void SpawnLocker();
    public void Teleport(BasePlayer player, BasePlayer target);
    public void Teleport(BasePlayer player, float x, float y, float z);
    public void Teleport(BasePlayer player, Vector3 position);
    [ChatCommand("event")]
     void cmdCommandMain(BasePlayer player, string command, string[] args);
    public BasePlayer FindBasePlayer(string nameOrUserId);
     object OnPlayerRespawn(BasePlayer player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hit);
    private Vector3 sphereposition;
    [ChatCommand("r")]
    private void SpherePos(BasePlayer p);
     object OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void DestroyLocker();
     void RefreshTeam();
     void Unload();
}


```

---

## TeamMarker

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("TeamMarker", "Seires", "1.0.2")]
[Description("Displays marker for you and your teammates.")]
 class TeamMarker : RustPlugin
{
    private class PluginConfig
    {
        [JsonProperty("Activation button (Check list for valid buttons below)")]
        public string ActivateButton;
        [JsonProperty("Marker symbol (From ASCII table: https://www.ascii-code.com)")]
        public char MarkerSymbol;
        [JsonProperty("The time how long marker is visible")]
        public int MarkerTime;
        [JsonProperty("Cooldown time")]
        public int MarkerDelay;
        [JsonProperty("Max distance to locate the marker")]
        public int MarkerDist;
        [JsonProperty("Marker color")]
        public string MarkerColor;
        [JsonProperty("Font size of marker info")]
        public int InfoFontSize;
        [JsonProperty("Color of text")]
        public string InfoTextColor;
        [JsonProperty("Effect prefab")]
        public string EffectPrefab;
        [JsonIgnore]
        public BUTTON ActButton;
        [JsonProperty("Buttons list (Read only)")]
        public string[] Buttons;
    }

    private static TeamMarker _plugin;
    private static PluginConfig _config;
    private const string PermissionUse;
    private Effect _effect;
    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
     void OnServerInitialized();
     void Unload();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void PlayFx(BasePlayer player);
    public void RegPermission(string name);
    public bool HasPermission(BasePlayer player, string name);
    private class InputController : MonoBehaviour
    {
        public static List<InputController> InputControllers;
        private BasePlayer _player;
        private RealTimeSince _markerCooldown;
        private RealTimeSince _markerLifeTime;
        private Vector3 _lastMarkerPosition;
        public static void Init(BasePlayer player);
        public static void DestroyAll();
        private void Awake();
        private void Update();
        private void MarkerCheck();
        private void DrawMarker();
        private void MarkerUpdate();
    }

}

private class PluginConfig
{
    [JsonProperty("Activation button (Check list for valid buttons below)")]
    public string ActivateButton;
    [JsonProperty("Marker symbol (From ASCII table: https://www.ascii-code.com)")]
    public char MarkerSymbol;
    [JsonProperty("The time how long marker is visible")]
    public int MarkerTime;
    [JsonProperty("Cooldown time")]
    public int MarkerDelay;
    [JsonProperty("Max distance to locate the marker")]
    public int MarkerDist;
    [JsonProperty("Marker color")]
    public string MarkerColor;
    [JsonProperty("Font size of marker info")]
    public int InfoFontSize;
    [JsonProperty("Color of text")]
    public string InfoTextColor;
    [JsonProperty("Effect prefab")]
    public string EffectPrefab;
    [JsonIgnore]
    public BUTTON ActButton;
    [JsonProperty("Buttons list (Read only)")]
    public string[] Buttons;
}

private class InputController : MonoBehaviour
{
    public static List<InputController> InputControllers;
    private BasePlayer _player;
    private RealTimeSince _markerCooldown;
    private RealTimeSince _markerLifeTime;
    private Vector3 _lastMarkerPosition;
    public static void Init(BasePlayer player);
    public static void DestroyAll();
    private void Awake();
    private void Update();
    private void MarkerCheck();
    private void DrawMarker();
    private void MarkerUpdate();
}


```

---

## TearUpBP

```csharp
using Oxide.Core;
using System.Collections.Generic;

Oxide.Plugins
[Info("Tear Up BluePrints", "DaBludger", 1.91, ResourceId = 1300)]
[Description("This will allow players to 'Tear up' Blueprints for BP Fragments")]
public class TearUpBP : RustPlugin
{
    private static Dictionary<int, ItemDefinition> ALLITEMS;
    private static Dictionary<string, object> CUSTOME_ITME_NAMES;
    private static double MIN_FRAGS_MULTIPLYER;
    private static double MAX_FRAGS_MULTIPLYER;
    private static bool CAN_PLAYER_USE;
    private static bool USE_PERMISSIONS;
    private static readonly Dictionary<string, int> bprarety2frags;
    private static readonly string UNKNOWN_ITEM;
    private static readonly string HELP_TEXT;
    private static readonly string NO_PERMISSION;
     void Init();
     void OnServerInitialized();
    private void CheckCfg(string Key, T var);
     void LoadDefaultConfig();
    private void setMinFrags(double min);
    private void setMaxFrag(double max);
    [ChatCommand("tearup")]
    private void TearupCommand(BasePlayer player, string command, string[] args);
    private void sendChant2Player(BasePlayer player, string msg);
    private void givePlayerFragments(BasePlayer player, int frags2give);
    private bool isNameOfBP(string bpname);
    private string getRealBPItemName(string cName);
    private ItemBlueprint getBPItem(string bpname);
    private bool removeBP(BasePlayer player, int bpid);
    private int getItemId(string name);
    private string prityPrintItemNames(ItemBlueprint iBP);
    private string prityPrintItemNames(string iBPname);
}


```

---

## Teleport

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System.Globalization;

Oxide.Plugins
[Info("Teleport", "unknown", "1.1.40")]
 class Teleport : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    [PluginReference]
     Plugin NoEscape;
    [PluginReference]
     Plugin Duel;
    private TeleportConfig m_Config;
    private Dictionary<int, TeleportObject> m_TeleportToPlayerWaiters;
    private Dictionary<string, PlayerPluginData> m_ActivePlayers;
    private Dictionary<int, HomeTeleportObject> m_TeleportToHomeWaiters;
    private class TeleportConfig
    {
        [JsonProperty("Префикс плагина для отображения в чате")]
        public string PluginPrefix { get; set; }
        [JsonProperty("Включить префикс плагина ?")]
        public bool EnablePluginPrefix { get; set; }
        [JsonProperty("Включить вывод информации о привилегиях в консоль ?")]
        public bool EnablePrintPermissionsInfo { get; set; }
        [JsonProperty("Включить GUI в плагине ?")]
        public bool EnableGUI { get; set; }
        [JsonProperty("Включить логирование использования команд администраторами ?")]
        public bool EnableAdminCommandsLogging { get; set; }
        [JsonProperty("Файл логирования команд администраторами")]
        public string AdminCommandsLoggingFile { get; set; }
        [JsonProperty("Включить логирование использования команд игроками ?")]
        public bool EnableUsersCommandsLogging { get; set; }
        [JsonProperty("Файл логирования команд игроками")]
        public string UserCommandsLoggingFile { get; set; }
        [JsonProperty("Включить систему домов ?")]
        public bool AllowHomeSystem { get; set; }
        [JsonProperty("Включить систему телепортации ?")]
        public bool AllowTeleportSystem { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта во время блока NoEscape ?")]
        public bool AllowTeleportInEscapeBlock { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта во время рейда ?")]
        public bool AllowTeleportInRaidBlock { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта во время боя ?")]
        public bool AllowTeleportInCombatBlock { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта в воде ?")]
        public bool AllowTeleportInWater { get; set; }
        [JsonProperty("Уровень воды для отмены телепорта")]
        public double CancelTeleportWaterFactor { get; set; }
        [JsonProperty("Разрешить телепортироваться в ванише ?")]
        public bool AllowTeleportInVanish { get; set; }
        [JsonProperty("Разрешить телепорт из РТ ?")]
        public bool AllowTeleportInRT { get; set; }
        [JsonProperty("Дальность от РТ для проверки")]
        public float ToRTDistance { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта во время крафта ?")]
        public bool AllowTeleportInCraft { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта при ваунде ?")]
        public bool AllowTeleportInWounded { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта после смерти ?")]
        public bool AllowTeleportAfterDeath { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта во время действия радиации ?")]
        public bool AllowTeleportInRadiation { get; set; }
        [JsonProperty("Уровень радиации для отмены телепорта")]
        public int CancelTeleportRadiationFactor { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта во время действия кровотечения ?")]
        public bool AllowTeleportInBleeding { get; set; }
        [JsonProperty("Уровень кровотечения для отмены телепорта")]
        public int CancelTeleportBleedingFactor { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта при получении урона ?")]
        public bool AllowTeleportAfterDamage { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта во время охлаждения ?")]
        public bool AllowTeleportInFreezing { get; set; }
        [JsonProperty("Уровень холода для отмены телепорта")]
        public int CancelTeleportFreezeFactor { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта в зоне действия чужого шкафа ?")]
        public bool AllowTeleportInBBZone { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта только друзьям ?")]
        public bool AllowTeleportOnlyFriends { get; set; }
        [JsonProperty("Разрешить использовать систему телепорта только соклановцам ?")]
        public bool AllowTeleportOnlyClan { get; set; }
        [JsonProperty("Разрешить телепорт при получении урона ?")]
        public bool AllowTeleportAfterRCVDamage { get; set; }
        [JsonProperty("Разрешить сохранять дом только на фундаменте ?")]
        public bool AllowSaveHomeOnlyFundament { get; set; }
        [JsonProperty("Создавать спальный мешок после сохранения дома ?")]
        public bool CreateBagAfterHomeSave { get; set; }
        [JsonProperty("Разрешить сохранять дом в зоне действия чужого шкафа ?")]
        public bool AllowSaveHomeOnBB { get; set; }
        [JsonProperty("Разрешить сохранять дом только на фундаменте соклановцев ?")]
        public bool AllowSaveHomeOnClanFundament { get; set; }
        [JsonProperty("Разрешить сохранять дом только на фундаменте друзей ?")]
        public bool AllowSaveHomeOnFriendFundament { get; set; }
        [JsonProperty("Время ожидания телепорта по умолчанию")]
        public int DefaultTimeToTeleport { get; set; }
        [JsonProperty("Время ожидания телепорта домой по умолчанию")]
        public int DefaultTimeToTeleportHome { get; set; }
        [JsonProperty("Время восстановление телепорта по умолчанию")]
        public int DefaultRemainingAfterTeleport { get; set; }
        [JsonProperty("Время восстановления телепорта домой по умолчанию")]
        public int DefaultRemainingAfterHomeTeleport { get; set; }
        [JsonProperty("Количество сохраняемых домов по умолчанию")]
        public int DefaultSaveHomeLimit { get; set; }
        [JsonProperty("Количество телепортов к игроку в сутки ?")]
        public int DefaultTTPOneDay { get; set; }
        [JsonProperty("Количество телепортов домой в сутки ?")]
        public int DefaultTTHOneDay { get; set; }
        [JsonProperty("Время, через которое запрос будет отменен автоматически, если игрок на него не ответил")]
        public int DefaultRequestCancelledTime { get; set; }
        [JsonProperty("Время, через которое лимиты будут обновлены автоматически (По умолчанию = 24 часа)")]
        public int DefaultLimitsUpdated { get; set; }
        [JsonProperty("Название уведомления о телепортации. (По умолчанию: ЗАПРОС:)")]
        public string DefaultHeaderTeleportNotify { get; set; }
        [JsonProperty("Сообщение о запросе на телепорт. (По умолчанию {0} хочет телепортироваться к Вам)")]
        public string DefaultFromTeleportNotify { get; set; }
        [JsonProperty("Включить звуковое уведомление")]
        public bool EnableSoundEffects { get; set; }
        [JsonProperty("Кастомные пути к звуковому файлу, для воспроизведения. Если парааметр: 'Включить звуковое уведомление' включен")]
        public Dictionary<string, string> SoundsFotNotify { get; set; }
        [JsonProperty("Динамическая система привилегий. Указывайте настройки для каждой привилегии. Пример указан.")]
        public Dictionary<string, Dictionary<string, Dictionary<string, int>>> PermissionSettings { get; set; }
        public static TeleportConfig Prototype();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private class PlayerPluginData
    {
        public string ID { get; set; }
        public string Name { get; set; }
        public int TTPLimit { get; set; }
        public int TTHLimit { get; set; }
        public bool GuiEnabled { get; set; }
        public bool FriendsEnabled { get; set; }
        public bool ClanEnabled { get; set; }
        public bool AllEnabled { get; set; }
        public Dictionary<string, HomeObject> Homes { get; set; }
        public TimerBase NextTeleportRemaining { get; set; }
        public TimerBase NextHomeTeleportRemaining { get; set; }
        public TimerBase NextTTPLimitRemaining { get; set; }
        public TimerBase NextTTHLimitRemaining { get; set; }
        public PlayerPluginData();
        public PlayerPluginData(string id, string name, int ttpl, int tthl, Dictionary<string, HomeObject> homes, TimerBase nextTR, TimerBase nextHTR, TimerBase nttp, TimerBase ntth);
        public void CreateRemaining(Remainings type, PluginTimers timer, int seconds, Action callback);
        public void CreateHome(int id, string name, BasePlayer player, int limit, bool createBag);
        public void RestoreHome(string name, SleepingBag bag);
        public void RemoveHome(string name);
        public void PrepareToSave();
    }

    private class TeleportObject
    {
        public int ID { get; set; }
        public int Seconds { get; set; }
        public bool Accepted { get; set; }
        public PlayerPluginData Initiator { get; set; }
        public PlayerPluginData Target { get; set; }
        public TimerBase Remaining { get; set; }
        public TeleportObject(PlayerPluginData init, PlayerPluginData target, int secs);
        public void Instantiate(PluginTimers timer, Action callback, Action onTick);
    }

    private class HomeObject
    {
        public int ID { get; set; }
        public string Name { get; set; }
        public Vector3 Position { get; set; }
        public SleepingBag Bag { get; set; }
        public HomeObject();
        public HomeObject(int id, string name, Vector3 pos, BasePlayer player, bool createBag);
        private SleepingBag CreateSleepingBag(BasePlayer player);
    }

    private class HomeTeleportObject
    {
        public int ID { get; set; }
        public int Seconds { get; set; }
        public TimerBase Remaining { get; set; }
        public PlayerPluginData Owner { get; set; }
        public Vector3 Position { get; set; }
        public HomeTeleportObject(PlayerPluginData owner, int secs, Vector3 pos);
        public void Instantiate(PluginTimers timer, Action callback, Action onTick);
    }

    private class TimerBase
    {
        public Timer Object { get; set; }
        public int Remaining { get; set; }
        public bool IsEnabled { get; set; }
        public TimerBase();
        public void Instantiate(PluginTimers @object, int secs, Action callBackAction, Action onTick);
        public void Destroy();
    }

    public Teleport();
    private void RegisterPermissions();
     void Loaded();
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     object OnPlayerDie(BasePlayer player, HitInfo info);
     void OnEntityKill(BaseNetworkable entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     object CanRenameBed(BasePlayer player, SleepingBag bed, string bedName);
     bool CanBeWounded(BasePlayer player, HitInfo info);
     bool CanPickupEntity(BasePlayer basePlayer, BaseCombatEntity entity);
     object CanAssignBed(BasePlayer assigner, SleepingBag bagEntity, ulong targetPlayerId);
    [ChatCommand("tpr")]
     void CommandChatTPR(BasePlayer player, string command, string[] args);
    [ChatCommand("tpa")]
     void CommandChatTPA(BasePlayer player, string command, string[] args);
    [ChatCommand("tpc")]
     void CommandChatTPC(BasePlayer player, string command, string[] args);
    [ChatCommand("home")]
     void CommandChatHomeObjective(BasePlayer player, string command, string[] args);
     void SetHome(BasePlayer player, string homename);
     void RemoveHome(BasePlayer player, string homename);
     void ShowHomelist(BasePlayer player);
     void TPToHome(BasePlayer player, string homename);
    [ChatCommand("homes")]
     void CommandChatAdminHomelist(BasePlayer player, string command, string[] args);
    [ChatCommand("tp")]
     void CommandChatTP_Admin(BasePlayer player, string command, string[] args);
    [ChatCommand("tpswitch")]
     void CommandChatGuiSwitch(BasePlayer player, string command, string[] args);
    [ConsoleCommand("tp.request")]
    private void CmdConsoleTPRequest(ConsoleSystem.Arg arg);
    [ConsoleCommand("tp.cancel")]
    private void CmdConsoleTPCancel(ConsoleSystem.Arg arg);
    [ConsoleCommand("tp.accept")]
    private void CmdConsoleTPAccept(ConsoleSystem.Arg arg);
    private void LoadAllPlayers();
    private void SaveAllPlayers();
    private void LoadPlayer(BasePlayer player);
    private void SavePlayer(BasePlayer player, bool all);
    private void ShowHomes(BasePlayer target, bool isAdminCheck);
    private void CreateTTHRequest(PlayerPluginData initiator, string name);
    private void FinishedTTHRequest(HomeTeleportObject obj);
    private void CancelTTHRequest(string id);
    private HomeTeleportObject GetHomeObjectFromWaiters(string id);
    private bool IsWaitTeleportToHome(BasePlayer player);
    private static void ClearTeleport(BasePlayer player, Vector3 position);
    private string CheckPlayer(BasePlayer player, bool isInitiator, bool target, bool accept, bool finished);
    private bool IsDuelPlayer(BasePlayer target);
    private bool IsRT(BasePlayer player);
    private bool IsBleeding(BasePlayer player);
    private bool IsFreezing(BasePlayer player);
    private bool IsWounded(BasePlayer player);
    private bool InWater(BasePlayer player);
    private bool IsRadiation(BasePlayer player);
    private bool IsCanTeleported(BasePlayer player);
    private bool IsPlayerInBB(BasePlayer player);
    private bool IsPlayerInOtherFoundation(BasePlayer player);
    private bool IsRemaining(BasePlayer player, bool home);
    private bool IsUnlimited(BasePlayer player, bool home);
    private bool IsClanMember(ulong playerID, ulong targetID);
    private bool IsFriends(ulong playerID, ulong friendId);
    private bool IsRaidBlockedRP(BasePlayer player);
    private bool IsRaidBlockedOM(BasePlayer player);
    private bool IsEscapeBlocked(BasePlayer player);
    private bool IsCombatBlocked(BasePlayer player);
    private bool IsAlreadyWait(BasePlayer player, bool isInitiator);
    private bool IsBagInBB(SleepingBag bag, BasePlayer owner);
    private BasePlayer FindPlayer(string nameOrUserId);
    private string GetMessage(string key, Plugin caller);
    private string GetReadeableSeconds(int seconds, bool isTTH);
    private BaseEntity GetFoundation(Vector3 pos);
    private void CreateTTPRequest(PlayerPluginData initiator, PlayerPluginData target);
    private void CancelTTPRequest(string id);
    private void AppendTTPRequest(TeleportObject obj);
    private void FinishedTTPRequest(TeleportObject obj);
    private void PlaySound(BasePlayer player, string key);
    public class UI
    {
        public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
        public static void LoadImage(CuiElementContainer container, string panel, string url, string aMin, string aMax);
        public static void CreateInput(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, bool password, int charLimit, TextAnchor align);
        public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        public static void CreateText(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        public static string Color(string hexColor, float alpha);
    }

    private static string TeleportRequestHud;
    private static string TeleportTimerHud;
    public void ShowTeleportRequest(BasePlayer player, string from);
    public void DestroyTeleportRequest(BasePlayer player);
    public void RefreshTeleportTimer(BasePlayer player, int secs, string to);
    public void DestroyTeleportTimer(BasePlayer player);
    private PlayerPluginData GetPlayerDataFromWaiters(string id, bool isInitiator);
    private PlayerPluginData GetPlayerFromActive(string id);
    private TeleportObject GetTeleportData(int id);
    private TeleportObject GetTeleportData(PlayerPluginData obj, bool isInitiator);
    private TeleportObject GetTeleportData(string id);
    private int GenerateID();
    public int GetPermissionProperty(string permission, Settings setting);
    public int GetWaitTTPSeconds(string id);
    public int GetWaitTTHSeconds(string id);
    public int GetTTPRemaining(string id);
    public int GetTTHRemaining(string id);
    public int GetSHLimit(string id);
    public int GetTTHLimit(string id);
    public int GetTTPLimit(string id);
    static UnityVector3Converter Converter;
    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

private class TeleportConfig
{
    [JsonProperty("Префикс плагина для отображения в чате")]
    public string PluginPrefix { get; set; }
    [JsonProperty("Включить префикс плагина ?")]
    public bool EnablePluginPrefix { get; set; }
    [JsonProperty("Включить вывод информации о привилегиях в консоль ?")]
    public bool EnablePrintPermissionsInfo { get; set; }
    [JsonProperty("Включить GUI в плагине ?")]
    public bool EnableGUI { get; set; }
    [JsonProperty("Включить логирование использования команд администраторами ?")]
    public bool EnableAdminCommandsLogging { get; set; }
    [JsonProperty("Файл логирования команд администраторами")]
    public string AdminCommandsLoggingFile { get; set; }
    [JsonProperty("Включить логирование использования команд игроками ?")]
    public bool EnableUsersCommandsLogging { get; set; }
    [JsonProperty("Файл логирования команд игроками")]
    public string UserCommandsLoggingFile { get; set; }
    [JsonProperty("Включить систему домов ?")]
    public bool AllowHomeSystem { get; set; }
    [JsonProperty("Включить систему телепортации ?")]
    public bool AllowTeleportSystem { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта во время блока NoEscape ?")]
    public bool AllowTeleportInEscapeBlock { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта во время рейда ?")]
    public bool AllowTeleportInRaidBlock { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта во время боя ?")]
    public bool AllowTeleportInCombatBlock { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта в воде ?")]
    public bool AllowTeleportInWater { get; set; }
    [JsonProperty("Уровень воды для отмены телепорта")]
    public double CancelTeleportWaterFactor { get; set; }
    [JsonProperty("Разрешить телепортироваться в ванише ?")]
    public bool AllowTeleportInVanish { get; set; }
    [JsonProperty("Разрешить телепорт из РТ ?")]
    public bool AllowTeleportInRT { get; set; }
    [JsonProperty("Дальность от РТ для проверки")]
    public float ToRTDistance { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта во время крафта ?")]
    public bool AllowTeleportInCraft { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта при ваунде ?")]
    public bool AllowTeleportInWounded { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта после смерти ?")]
    public bool AllowTeleportAfterDeath { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта во время действия радиации ?")]
    public bool AllowTeleportInRadiation { get; set; }
    [JsonProperty("Уровень радиации для отмены телепорта")]
    public int CancelTeleportRadiationFactor { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта во время действия кровотечения ?")]
    public bool AllowTeleportInBleeding { get; set; }
    [JsonProperty("Уровень кровотечения для отмены телепорта")]
    public int CancelTeleportBleedingFactor { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта при получении урона ?")]
    public bool AllowTeleportAfterDamage { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта во время охлаждения ?")]
    public bool AllowTeleportInFreezing { get; set; }
    [JsonProperty("Уровень холода для отмены телепорта")]
    public int CancelTeleportFreezeFactor { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта в зоне действия чужого шкафа ?")]
    public bool AllowTeleportInBBZone { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта только друзьям ?")]
    public bool AllowTeleportOnlyFriends { get; set; }
    [JsonProperty("Разрешить использовать систему телепорта только соклановцам ?")]
    public bool AllowTeleportOnlyClan { get; set; }
    [JsonProperty("Разрешить телепорт при получении урона ?")]
    public bool AllowTeleportAfterRCVDamage { get; set; }
    [JsonProperty("Разрешить сохранять дом только на фундаменте ?")]
    public bool AllowSaveHomeOnlyFundament { get; set; }
    [JsonProperty("Создавать спальный мешок после сохранения дома ?")]
    public bool CreateBagAfterHomeSave { get; set; }
    [JsonProperty("Разрешить сохранять дом в зоне действия чужого шкафа ?")]
    public bool AllowSaveHomeOnBB { get; set; }
    [JsonProperty("Разрешить сохранять дом только на фундаменте соклановцев ?")]
    public bool AllowSaveHomeOnClanFundament { get; set; }
    [JsonProperty("Разрешить сохранять дом только на фундаменте друзей ?")]
    public bool AllowSaveHomeOnFriendFundament { get; set; }
    [JsonProperty("Время ожидания телепорта по умолчанию")]
    public int DefaultTimeToTeleport { get; set; }
    [JsonProperty("Время ожидания телепорта домой по умолчанию")]
    public int DefaultTimeToTeleportHome { get; set; }
    [JsonProperty("Время восстановление телепорта по умолчанию")]
    public int DefaultRemainingAfterTeleport { get; set; }
    [JsonProperty("Время восстановления телепорта домой по умолчанию")]
    public int DefaultRemainingAfterHomeTeleport { get; set; }
    [JsonProperty("Количество сохраняемых домов по умолчанию")]
    public int DefaultSaveHomeLimit { get; set; }
    [JsonProperty("Количество телепортов к игроку в сутки ?")]
    public int DefaultTTPOneDay { get; set; }
    [JsonProperty("Количество телепортов домой в сутки ?")]
    public int DefaultTTHOneDay { get; set; }
    [JsonProperty("Время, через которое запрос будет отменен автоматически, если игрок на него не ответил")]
    public int DefaultRequestCancelledTime { get; set; }
    [JsonProperty("Время, через которое лимиты будут обновлены автоматически (По умолчанию = 24 часа)")]
    public int DefaultLimitsUpdated { get; set; }
    [JsonProperty("Название уведомления о телепортации. (По умолчанию: ЗАПРОС:)")]
    public string DefaultHeaderTeleportNotify { get; set; }
    [JsonProperty("Сообщение о запросе на телепорт. (По умолчанию {0} хочет телепортироваться к Вам)")]
    public string DefaultFromTeleportNotify { get; set; }
    [JsonProperty("Включить звуковое уведомление")]
    public bool EnableSoundEffects { get; set; }
    [JsonProperty("Кастомные пути к звуковому файлу, для воспроизведения. Если парааметр: 'Включить звуковое уведомление' включен")]
    public Dictionary<string, string> SoundsFotNotify { get; set; }
    [JsonProperty("Динамическая система привилегий. Указывайте настройки для каждой привилегии. Пример указан.")]
    public Dictionary<string, Dictionary<string, Dictionary<string, int>>> PermissionSettings { get; set; }
    public static TeleportConfig Prototype();
}

private class PlayerPluginData
{
    public string ID { get; set; }
    public string Name { get; set; }
    public int TTPLimit { get; set; }
    public int TTHLimit { get; set; }
    public bool GuiEnabled { get; set; }
    public bool FriendsEnabled { get; set; }
    public bool ClanEnabled { get; set; }
    public bool AllEnabled { get; set; }
    public Dictionary<string, HomeObject> Homes { get; set; }
    public TimerBase NextTeleportRemaining { get; set; }
    public TimerBase NextHomeTeleportRemaining { get; set; }
    public TimerBase NextTTPLimitRemaining { get; set; }
    public TimerBase NextTTHLimitRemaining { get; set; }
    public PlayerPluginData();
    public PlayerPluginData(string id, string name, int ttpl, int tthl, Dictionary<string, HomeObject> homes, TimerBase nextTR, TimerBase nextHTR, TimerBase nttp, TimerBase ntth);
    public void CreateRemaining(Remainings type, PluginTimers timer, int seconds, Action callback);
    public void CreateHome(int id, string name, BasePlayer player, int limit, bool createBag);
    public void RestoreHome(string name, SleepingBag bag);
    public void RemoveHome(string name);
    public void PrepareToSave();
}

private class TeleportObject
{
    public int ID { get; set; }
    public int Seconds { get; set; }
    public bool Accepted { get; set; }
    public PlayerPluginData Initiator { get; set; }
    public PlayerPluginData Target { get; set; }
    public TimerBase Remaining { get; set; }
    public TeleportObject(PlayerPluginData init, PlayerPluginData target, int secs);
    public void Instantiate(PluginTimers timer, Action callback, Action onTick);
}

private class HomeObject
{
    public int ID { get; set; }
    public string Name { get; set; }
    public Vector3 Position { get; set; }
    public SleepingBag Bag { get; set; }
    public HomeObject();
    public HomeObject(int id, string name, Vector3 pos, BasePlayer player, bool createBag);
    private SleepingBag CreateSleepingBag(BasePlayer player);
}

private class HomeTeleportObject
{
    public int ID { get; set; }
    public int Seconds { get; set; }
    public TimerBase Remaining { get; set; }
    public PlayerPluginData Owner { get; set; }
    public Vector3 Position { get; set; }
    public HomeTeleportObject(PlayerPluginData owner, int secs, Vector3 pos);
    public void Instantiate(PluginTimers timer, Action callback, Action onTick);
}

private class TimerBase
{
    public Timer Object { get; set; }
    public int Remaining { get; set; }
    public bool IsEnabled { get; set; }
    public TimerBase();
    public void Instantiate(PluginTimers @object, int secs, Action callBackAction, Action onTick);
    public void Destroy();
}

public class UI
{
    public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor, string parent);
    public static void LoadImage(CuiElementContainer container, string panel, string url, string aMin, string aMax);
    public static void CreateInput(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, bool password, int charLimit, TextAnchor align);
    public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    public static void CreateText(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    public static string Color(string hexColor, float alpha);
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## Teleportation

```csharp
using Oxide.Core;
using System.Collections.Generic;
using System.Linq;
using System;
using System.Reflection;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Core.Libraries;
using System.IO;

Oxide.Plugins
[Info("Teleportation", "OxideBro/Ryamkk", "1.2.2")]
 class Teleportation : RustPlugin
{
    [PluginReference]
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
    readonly MethodInfo entitySnapshot;
     Dictionary<ulong, Vector3> lastPositions;
     Dictionary<BasePlayer, int> spectatingPlayers;
     bool IsClanMember(ulong playerID, ulong targetID);
     bool IsFriends(ulong playerID, ulong friendId);
     class TP
    {
        public BasePlayer Player;
        public BasePlayer Player2;
        public Vector3 pos;
        public int seconds;
        public TP(BasePlayer player, Vector3 pos, int seconds, BasePlayer player2);
    }

    const string TPADMIN;
     Dictionary<string, int> homelimitPerms;
     Dictionary<string, int> teleportSecsPerms;
     Dictionary<string, int> tpkdPerms;
     Dictionary<string, int> tpkdhomePerms;
     int RadiationBlockTP;
     int ColdBlockTP;
     int BleedingBlockTP;
     int CraftBlockTP;
     int homelimitDefault;
     int tpkdDefault;
     int tpkdhomeDefault;
     int teleportSecsDefault;
     int resetPendingTime;
     int CraftBlockHome;
     int RadiationBlockHome;
     int BleedingBlockHome;
     int ColdBlockHome;
     bool CanSwimmingTP;
     bool CanCraftTP;
     bool CanAliveTP;
     bool CanRadiationTP;
     bool CanBleedingTP;
     bool CanWoundedTP;
     bool CanColdTP;
     bool restrictCupboard;
     bool enabledTP;
     bool enabledHome;
     bool homecupboard;
     bool adminsLogs;
     bool foundationOwner;
     bool foundationOwnerFC;
     bool restrictTPRCupboard;
     bool foundationEx;
     bool wipedData;
     bool createSleepingBug;
     bool CanWoundedHome;
     bool CanColdHome;
     bool CanSwimmingHome;
     bool CanCraftHome;
     bool CanAliveHome;
     bool CanRadiationHome;
     bool CanBleedingHome;
     string EffectPrefab1;
     string EffectPrefab;
     string SleepingBagName;
    static DynamicConfigFile config;
     void WipeData();
    protected override void LoadDefaultConfig();
     T GetConfig(string name, T defaultValue);
    public static void GetVariable(DynamicConfigFile config, string name, string Key, T value, T defaultValue);
    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(ulong uid, string permissionName);
        public static void RegisterPermissions(Plugin owner, List<string> permissions);
    }

    public BasePlayer FindBasePlayer(string nameOrUserId);
    private FieldInfo SleepingBagUnlockTimeField;
    private readonly int groundLayer;
    private readonly int buildingMask;
     Dictionary<ulong, Dictionary<string, Vector3>> homes;
     Dictionary<ulong, Dictionary<string, Vector3>> tpsave;
     Dictionary<ulong, int> cooldownsTP;
     Dictionary<ulong, int> cooldownsHOME;
     List<TP> tpQueue;
     List<TP> pendings;
     List<ulong> sethomeBlock;
    [ChatCommand("sethome")]
     void cmdChatSetHome(BasePlayer player, string command, string[] args);
    [ChatCommand("removehome")]
     void cmdChatRemoveHome(BasePlayer player, string command, string[] args);
    [ConsoleCommand("home")]
     void cmdHome(ConsoleSystem.Arg arg);
    [ChatCommand("homelist")]
    private void cmdHomeList(BasePlayer player, string command, string[] args);
    [ChatCommand("home")]
     void cmdChatHome(BasePlayer player, string command, string[] args);
    [ChatCommand("tpr")]
     void cmdChatTpr(BasePlayer player, string command, string[] args);
    [ChatCommand("tpa")]
     void cmdChatTpa(BasePlayer player, string command, string[] args);
    [ChatCommand("tpc")]
     void cmdChatTpc(BasePlayer player, string command, string[] args);
     void SpectateFinish(BasePlayer player);
    [ChatCommand("tpl")]
     void cmdChattpGo(BasePlayer player, string command, string[] args);
    [ChatCommand("tpspec")]
     void cmdTPSpec(BasePlayer player, string command, string[] args);
    [ChatCommand("tp")]
     void cmdTP(BasePlayer player, string command, string[] args);
    [ConsoleCommand("home.wipe")]
    private void CmdTest(ConsoleSystem.Arg arg);
    public string TimeToString(double time);
     void OnPlayerDisconnected(BasePlayer player);
     void OnNewSave();
     void OnServerInitialized();
     void Unload();
     void OnEntityBuilt(Planner planner, GameObject gameobject);
     void TeleportationTimerHandle();
     void SetTpSave(BasePlayer player, string name);
     void SetHome(BasePlayer player, string name);
     int GetKDHome(ulong uid);
     int GetKD(ulong uid);
     int GetHomeLimit(ulong uid);
     int GetTeleportTime(ulong uid);
     BaseEntity GetBuldings(Vector3 pos);
     BaseEntity GetFoundation(Vector3 pos);
     SleepingBag GetSleepingBag(string name, Vector3 pos);
     void CreateSleepingBag(BasePlayer player, Vector3 pos, string name);
     Dictionary<string, Vector3> GetHomes(ulong uid);
    public void Teleport(BasePlayer player, BasePlayer target);
    public void Teleport(BasePlayer player, float x, float y, float z);
    public void Teleport(BasePlayer player, Vector3 position);
     DynamicConfigFile cooldownsFile;
    private class Cooldown
    {
        public ulong UserId;
        public double Expired;
        [JsonIgnore]
        public Action OnExpired;
    }

    private static Dictionary<string, List<Cooldown>> cooldowns;
    internal static void Service();
    public static void SetCooldown(BasePlayer player, string key, int seconds, Action onExpired);
    public static int GetCooldown(BasePlayer player, string key);
    static double GrabCurrentTime();
     DynamicConfigFile homesFile;
     DynamicConfigFile tpsaveFile;
     void OnServerSave();
     void LoadData();
     void SaveData();
     Dictionary<string, string> Messages;
    static UnityVector3Converter converter;
    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

 class TP
{
    public BasePlayer Player;
    public BasePlayer Player2;
    public Vector3 pos;
    public int seconds;
    public TP(BasePlayer player, Vector3 pos, int seconds, BasePlayer player2);
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(ulong uid, string permissionName);
    public static void RegisterPermissions(Plugin owner, List<string> permissions);
}

private class Cooldown
{
    public ulong UserId;
    public double Expired;
    [JsonIgnore]
    public Action OnExpired;
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## TeleportGUI

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("TeleportGUI", "PsychoTea", "1.6.2")]
 class TeleportGUI : RustPlugin
{
    private const string permUse;
    private const string permCancel;
    private const string permBack;
    private const string permHere;
    private const string permSleepers;
    private static TeleportGUI Instance;
    private Dictionary<BasePlayer, bool> GUIOpen;
    private Dictionary<BasePlayer, Vector3> LastTeleport;
    private List<GameObject> GameObjects;
    private bool DebuggingMode;
    [PluginReference]
    private Plugin Economics;
    private Plugin ServerRewards;
    private Plugin NoEscape;
    private Plugin ZoneManager;
     class GUIManager
    {
        public static Dictionary<BasePlayer, GUIManager> Players;
        public int Page;
        public bool TPHere;
        public bool Sleepers;
        public static GUIManager Get(BasePlayer player);
    }

     class TeleportRequest : MonoBehaviour
    {
        private BasePlayer _from;
        private BasePlayer _to;
        private bool _tpHere;
        private double _time;
        private bool _playerIsPaying;
        public static void Create(BasePlayer from, BasePlayer to, double timeoutTime, bool tpHere, bool playerIsPaying);
         void Start();
         void TimerTick();
        public void RequestAccepted();
        public void RequestDeclined();
        public void RequestCancelled();
         void RequestTimeOut();
         void CancelRequest();
         void OnDestroy();
    }

     class PendingRequest : MonoBehaviour
    {
        public BasePlayer From;
        public TeleportRequest TeleportRequest;
    }

     class Teleporter : MonoBehaviour
    {
        private GameObject _gameObject;
        private BasePlayer _from;
        private BasePlayer _to;
        private int _timeUtilTeleport;
        public void Create(BasePlayer from, BasePlayer to, int timeUntilTeleport);
         void Start();
         void TimerTick();
         void Teleport();
        public void CancelTeleport();
         void StartSleeping(BasePlayer player);
         void OnDestory();
    }

     class CooldownManager : MonoBehaviour
    {
         GameObject GameObject;
        public static void Create();
         void Start();
         void TimerTick();
         void OnDestroy();
    }

     class StoredData
    {
        public Dictionary<ulong, double> Cooldowns;
        public Dictionary<ulong, int> UsesToday;
    }

     StoredData storedData;
     void Init();
     void OnServerInitialized();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void Unload();
     ConfigData ConfigFile;
     class ConfigData
    {
        public bool PrefixEnabled;
        public string PrefixText;
        public int DefaultTimeUntilTeleport;
        public Dictionary<string, int> TimeUntilTeleport;
        public List<string> TPCommandAliases;
        public double RequestTimeoutTime;
        public double DefaultCooldown;
        public Dictionary<string, double> Cooldowns;
        public int DefaultDailyLimit;
        public Dictionary<string, int> DailyLimit;
        public bool AdminTPSilent;
        public bool AdminTPEnabled;
        public bool AllowTeleportWhilstBleeding;
        public bool AllowTeleportToBuildBlockedPlayer;
        public bool AllowTeleportFromBuildBlock;
        public bool UseEconomicsPlugin;
        public double EconomicsPrice;
        public bool UseServerRewardsPlugin;
        public int ServerRewardsPrice;
        public bool PayAfterUsingDailyLimits;
        public bool BlockTPCrafting;
        public bool AllowSpecialCharacters;
        public bool CancelTeleportOnDamage;
        public bool CanTeleportIntoZone;
        public bool CanTeleportFromZone;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ChatCommand("tp")]
     void TPCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("tpr")]
     void TPRCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("tpa")]
     void TPACommand(BasePlayer player, string command, string[] args);
    [ChatCommand("tpd")]
     void TPDCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("tpc")]
     void TPCCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("tpb")]
     void TPBCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("tpahere")]
     void TPAHereCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("tpgui")]
     void TPGuiCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("tpgui.resetdatafile")]
     void ResetAllCommand(ConsoleSystem.Arg arg);
     void ShowTeleportUI(BasePlayer player);
     void UIChooser(BasePlayer player);
     void CloseUI(BasePlayer player);
     void SmallUI(BasePlayer player);
     void BigUI(BasePlayer player);
     void TPR(BasePlayer player, BasePlayer targetPlayer, bool tpHere);
     void TPC(BasePlayer player);
     void TPB(BasePlayer player);
     void TPHere(BasePlayer player, ulong targetID);
     void Teleport(BasePlayer player, BasePlayer target);
     void Teleport(BasePlayer player, Vector3 position);
     void StartSleeping(BasePlayer player);
     void RecordLastTP(BasePlayer player, Vector3 oldPos);
     bool HasPendingTeleport(BasePlayer player);
     bool HasReachedDailyLimit(BasePlayer player);
     int IncrementUses(BasePlayer player);
     void ResetDailyUses();
     bool IsCrafting(BasePlayer player);
     int CalculatePages(int value);
     BasePlayer FindByName(string name, bool sleepers);
     List<BasePlayer> FindByNameMulti(string name, bool sleepers);
     bool EconomicsInstalled();
     bool ServerRewardsInstalled();
     bool PayEconomics(BasePlayer player);
     bool PayServerRewards(BasePlayer player);
     void RefundPlayerEconomics(BasePlayer player);
     void RefundServerRewards(BasePlayer player);
     bool HasPerm(BasePlayer player);
     bool HasPerm(BasePlayer player, string perm);
     string CleanText(string text);
     int TimeUntilMidnight();
     string GetMessage(string key);
     bool HasComponent(GameObject go);
     bool HasComponent(BasePlayer player);
     void SaveData();
     void ReadData();
     List<BasePlayer> SpareNames();
    static string names;
     string[] nameList;
}

 class GUIManager
{
    public static Dictionary<BasePlayer, GUIManager> Players;
    public int Page;
    public bool TPHere;
    public bool Sleepers;
    public static GUIManager Get(BasePlayer player);
}

 class TeleportRequest : MonoBehaviour
{
    private BasePlayer _from;
    private BasePlayer _to;
    private bool _tpHere;
    private double _time;
    private bool _playerIsPaying;
    public static void Create(BasePlayer from, BasePlayer to, double timeoutTime, bool tpHere, bool playerIsPaying);
     void Start();
     void TimerTick();
    public void RequestAccepted();
    public void RequestDeclined();
    public void RequestCancelled();
     void RequestTimeOut();
     void CancelRequest();
     void OnDestroy();
}

 class PendingRequest : MonoBehaviour
{
    public BasePlayer From;
    public TeleportRequest TeleportRequest;
}

 class Teleporter : MonoBehaviour
{
    private GameObject _gameObject;
    private BasePlayer _from;
    private BasePlayer _to;
    private int _timeUtilTeleport;
    public void Create(BasePlayer from, BasePlayer to, int timeUntilTeleport);
     void Start();
     void TimerTick();
     void Teleport();
    public void CancelTeleport();
     void StartSleeping(BasePlayer player);
     void OnDestory();
}

 class CooldownManager : MonoBehaviour
{
     GameObject GameObject;
    public static void Create();
     void Start();
     void TimerTick();
     void OnDestroy();
}

 class StoredData
{
    public Dictionary<ulong, double> Cooldowns;
    public Dictionary<ulong, int> UsesToday;
}

 class ConfigData
{
    public bool PrefixEnabled;
    public string PrefixText;
    public int DefaultTimeUntilTeleport;
    public Dictionary<string, int> TimeUntilTeleport;
    public List<string> TPCommandAliases;
    public double RequestTimeoutTime;
    public double DefaultCooldown;
    public Dictionary<string, double> Cooldowns;
    public int DefaultDailyLimit;
    public Dictionary<string, int> DailyLimit;
    public bool AdminTPSilent;
    public bool AdminTPEnabled;
    public bool AllowTeleportWhilstBleeding;
    public bool AllowTeleportToBuildBlockedPlayer;
    public bool AllowTeleportFromBuildBlock;
    public bool UseEconomicsPlugin;
    public double EconomicsPrice;
    public bool UseServerRewardsPlugin;
    public int ServerRewardsPrice;
    public bool PayAfterUsingDailyLimits;
    public bool BlockTPCrafting;
    public bool AllowSpecialCharacters;
    public bool CancelTeleportOnDamage;
    public bool CanTeleportIntoZone;
    public bool CanTeleportFromZone;
}


```

---

## TeleportMarker

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("Teleport Marker", "Talha", "1.0.5")]
[Description("Authorized players can teleport to map markers with that plugin.")]
public class TeleportMarker : RustPlugin
{
    private const string permUse;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Admins can teleport without permission")]
        public bool admins;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Message(BasePlayer player, string key, object[] args);
    private void Unload();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private void TP(BasePlayer player, MapNote note);
    private void OnMapMarkerAdded(BasePlayer player, MapNote note);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Admins can teleport without permission")]
    public bool admins;
}


```

---

## Template

```csharp
using Oxide.Core.Plugins;
using System.Collections.Generic;

Oxide.Plugins
[Info("Plugin name", "Author name", "1.0.0", ResourceId = 123)]
public class Template : RustPlugin
{
    [PluginReference]
     Plugin RustIO;
     bool IO();
     bool HasFriend(string playerId, string friendId);
     bool AddFriend(string playerId, string friendId);
     bool DeleteFriend(string playerId, string friendId);
     List<string> GetFriends(string playerId);
    [HookMethod("OnServerInitialized")]
     void OnServerInitialized();
}


```

---

## TextPromo

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Numerics;
using System.Security.Cryptography.X509Certificates;
using ConVar;
using Facepunch.Extend;
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("TextPromo", "TopPlugin.ru", "1.0.0")]
public class TextPromo : RustPlugin
{
    public class ConfigData
    {
        [JsonProperty("Адресс магазина")]
        public string nameserver;
        [JsonProperty("Первый промо")]
        public string promo1;
        [JsonProperty("Второй промо")]
        public string promo2;
        [JsonProperty("Сколько игроков получит промо")]
        public int count;
        public static ConfigData GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
    private ConfigData cfg { get; set; }
    public List<TextPromos> TextPromo2;
    [ChatCommand("givepromonote")]
     void cmdgive(BasePlayer player);
    public class TextPromos
    {
        [JsonProperty("Ник")]
        public string Name { get; set; }
        [JsonProperty("СтимАйди")]
        public ulong SteamID { get; set; }
        [JsonProperty("Выдал")]
        public bool Vidal { get; set; }
    }

     void OnPlayerInit(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void GivePromo(BasePlayer player);
     void SaveData();
     void OnServerInitialized();
}

public class ConfigData
{
    [JsonProperty("Адресс магазина")]
    public string nameserver;
    [JsonProperty("Первый промо")]
    public string promo1;
    [JsonProperty("Второй промо")]
    public string promo2;
    [JsonProperty("Сколько игроков получит промо")]
    public int count;
    public static ConfigData GetNewConf();
}

public class TextPromos
{
    [JsonProperty("Ник")]
    public string Name { get; set; }
    [JsonProperty("СтимАйди")]
    public ulong SteamID { get; set; }
    [JsonProperty("Выдал")]
    public bool Vidal { get; set; }
}


```

---

## TheGoldenEgg-1.9.24

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Facepunch;

Oxide.Plugins
[Info("The Golden Egg", "crunch", "1.9.24")]
[Description("Once found, the Golden Egg can bring you great riches, but it doesn't want to remain hidden...")]
 class TheGoldenEgg : CovalencePlugin
{
    [PluginReference]
    private Plugin TimedPermissions;
    private Plugin RaidableBases;
    private Configuration _pluginConfig;
    private static TheGoldenEgg ins;
    private const string permissionUse;
    private StoredData storedData;
    private DynamicConfigFile data;
    public float chance;
    public ulong eggUID;
    public ulong chinookID;
    private List<ulong> crateID;
    private Timer markerTimer;
    private Timer rewardTimer;
    private Timer delayTimer;
    private Timer saveTimer;
    private Timer blockTimer;
    private Timer moveTimer;
    private Timer crateEventTimer;
    private Timer crateMarkTimer;
    private Timer safeTimer;
    private Timer buildTimer;
    private Timer raidBaseTimer;
    private int resource;
    private int amount;
    private int spawnTime;
    private string cleanName;
    private string itemName;
    private bool blockEggOpen;
    private bool safezone;
    private bool inPriv;
    private bool pop;
    private bool priv;
    private bool tod;
    private bool blocked;
    private bool isBlocked;
    private bool crateDrop;
    private bool isRunning;
    public static string currentOwner;
    private DateTime startTime;
    private static PluginTimers Timer;
    public List<MapMarkerGenericRadius> eggRadMarker;
    public List<VendingMachineMapMarker> eggVendMarker;
    public MapMarkerGenericRadius _marker;
    public Dictionary<string, Item> _spawnedItems;
    public Dictionary<ulong, HackableLockedCrate> _spawnedCrates;
    readonly int blockLayer;
    private List<ulong> allowDraw;
    private List<uint> buildID;
    public Vector3 pos;
    public Vector3 lastPosition;
    public Vector3 cratePos;
     void Init();
     void OnServerInitialized(bool initial);
    private void runEvent();
    private void UpdateLoop();
     void Unload();
     void OnNewSave(string filename);
     object CanLootEntity(BasePlayer player, StorageContainer container);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void HandleAdds(ItemContainer container, Item item);
     object OnItemAction(Item item, string action, BasePlayer player);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
    public static bool IsBetween(TimeSpan start, TimeSpan end);
    private void SetModifiers(BasePlayer player);
     object OnItemPickup(Item item, BasePlayer player);
     void OnItemDropped(Item item, BaseEntity entity);
     object OnEntityKill(HackableLockedCrate crate);
     object OnItemRemove(Item item);
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object CanEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object OnProcessPlayerEntity(BaseEntity entity, HitInfo info);
     bool CheckDamage(BaseCombatEntity entity, HitInfo hitInfo);
     bool CheckOwner(BaseEntity target, BasePlayer source, BuildingPrivlidge priv);
    private List<StoredData.UserData> leaderList;
    private void CmdRunCommand(IPlayer player, string cmd, string[] args);
    private void AddToGroupTimed(string userId, string groupName, string duration);
     object OnHelicopterDropCrate(CH47HelicopterAIController heli);
     void OnCrateDropped(HackableLockedCrate crate);
    private static Color GetColor(string hex);
    public void SpawnMarker();
     object CanHackCrate(BasePlayer player, HackableLockedCrate crate);
    private static string FormatTime(double time);
    private void AddCommands();
     void EggText(BasePlayer player, Vector3 eggPos, string text);
    private string GetMsg(string key, string userId, object[] args);
    private int GetSpawnTime(string shortname);
    private int GetResourceAmount(string shortname);
     void SpawnRewards(BaseEntity contOwner, int resource, int amount);
    [HookMethod("OnPlayerEnteredRaidableBase")]
     void OnPlayerEnteredRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP);
    [HookMethod("OnPlayerExitedRaidableBase")]
     void OnPlayerExitedRaidableBase(BasePlayer player, Vector3 raidPos, bool allowPVP);
     void CreateMarkers(BasePlayer player);
     void MarkerDelete();
    private void LoadData();
    private void SaveDataTimer();
    private void SaveData();
     object OnPlayerCommand(BasePlayer player, string command, string[] args);
    private class StoredData
    {
        [JsonProperty]
        internal Hash<ulong, UserData> _eggPossession;
        [JsonProperty]
        internal string _eggCreated;
        public void DestroyTimers();
        public void OnEggMove(IPlayer user);
        public void GetTotals();
        public void GetEggLeaders(List<UserData> list);
        public class UserData
        {
            public double eggtime;
            public string displayName;
            public List<ulong> Team;
            public double TeamTotal;
            [JsonIgnore]
            public Timer _timer;
            private const float timer_Refresh;
            public void OnEggMove(IPlayer user);
            private void StartTimer();
        }

    }

    private CuiElementContainer CreateEggUI();
     Dictionary<ulong, Timer> spamCheck;
     void AddSpamBlock(ulong playerID);
     void RemoveSpamBlock(ulong playerID);
     Timer spamTimer(ulong playerID);
    protected override void LoadDefaultMessages();
     class DiscordWebhook
    {
        public string content { get; set; }
        public List<DiscordEmbed> embeds { get; set; }
        public DiscordWebhook SetContent(string Content);
        public DiscordEmbed AddEmbed();
        public void Send(TheGoldenEgg plugin, string Webhook);
    }

     class DiscordEmbed
    {
        private DiscordWebhook _webhook;
        public string description { get; set; }
        public string title { get; set; }
        public DateTime timestamp { get; set; }
        public int color { get; set; }
        public List<DiscordEmbedField> fields;
        public DiscordEmbedThumbnail thumbnail { get; set; }
        public DiscordEmbed SetDescription(string Description);
        public DiscordEmbed SetTitle(string Title);
        public DiscordEmbed SetTimestamp(DateTime Timestamp);
        public DiscordEmbed SetColor(int Color);
        public DiscordEmbed AddThumbnail(string Url);
        public DiscordEmbed AddField(string Name, string Value, bool Inline);
        public DiscordEmbed AddFields(IEnumerable<DiscordEmbedField> Fields);
        public DiscordEmbed AddEmbed();
        public DiscordWebhook EndEmbed();
        public DiscordEmbed();
        public DiscordEmbed(DiscordWebhook WebHook);
    }

     class DiscordEmbedThumbnail
    {
        public string url { get; set; }
    }

     class DiscordEmbedField
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public DiscordEmbedField();
        public DiscordEmbedField(string Name, string Value, bool Inline);
    }

    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("Egg spawn chance for Locked Crates (0 to 100)")]
        public float LockedCrateEggChance;
        [JsonProperty("Egg spawn chance for Elite Crates")]
        public float EliteCrateEggChance;
        [JsonProperty("Egg spawn chance for Military Crates")]
        public float MilitaryCrateEggChance;
        [JsonProperty("Egg spawn chance for Normal Crates")]
        public float NormalCrateEggChance;
        [JsonProperty(PropertyName = "Marker Settings")]
        public MarkerSettings markerS;
        [JsonProperty(PropertyName = "Resource Settings")]
        public ResourceSettings resourceS;
        [JsonProperty(PropertyName = "Item Customisation")]
        public ItemCustomisation customItemS;
        [JsonProperty(PropertyName = "Roam Settings")]
        public RoamSettings roamS;
        [JsonProperty(PropertyName = "Event Settings")]
        public CrateEvent crateS;
        [JsonProperty("Let players with permission pinpoint the egg on screen (use /egg find)")]
        public bool PinpointEggCommand;
        [JsonProperty("Send a webhook when the egg is found/destroyed")]
        public string sendWebhook;
        [JsonProperty("Don't add time while the player is in a safe zone")]
        public bool BlockSafeZone;
        [JsonProperty("Don't add time while the egg is in a stash")]
        public bool BlockStashTime;
        [JsonProperty("Don't add time while the egg is in a building")]
        public bool NoPrivTime;
        [JsonProperty("Don't add time while server pop is below (leave at 0 to disable)")]
        public int ServerPop;
        [JsonProperty("Don't add time between certain hours")]
        public bool TimeOfDay;
        [JsonProperty("Start of time period")]
        public string StartTime;
        [JsonProperty("End of time period")]
        public string EndTime;
        [JsonProperty("Destroy the egg if in a safe zone for longer than (seconds, leave at 0 to disable)")]
        public float safeDestroy;
        [JsonProperty("Destroy the egg if in a building blocked zone for longer than (seconds, leave at 0 to disable)")]
        public float blockedDestroy;
        [JsonProperty("Destroy the egg if in a Raidable Base zone for longer than (seconds, leave at 0 to disable)")]
        public float raidBaseDestroy;
        [JsonProperty("Name of permission group to grant with /egg winner (requires Timed Permissions plugin)")]
        public string GroupName;
        [JsonProperty("Duration to grant access to group (requires Timed Permissions plugin). Format: 1d12h30m")]
        public string GroupDuration;
        [JsonProperty("Destroy the egg after x minutes (leave at 0 to disable)")]
        public int KillTime;
        [JsonProperty("Block player from cracking open the egg while being raided")]
        public bool BlockEggCrack;
        [JsonProperty("Raid block timer")]
        public int BlockInterval;
        [JsonProperty("Data save interval")]
        public int SaveInterval;
        [JsonProperty("Clear data on map wipe")]
        public bool ClearData;
        [JsonProperty("Use custom item list when cracking open the egg")]
        public bool UseCustomList;
        [JsonProperty("Custom item list (use item shortname, eg rifle.m39, explosive.timed, etc")]
        public List<string> EggRewards;
        [JsonProperty("Blacklist commands whilst holding the egg")]
        public bool UseBlacklist;
        [JsonProperty("Blacklisted commands")]
        public List<string> BlacklistedCommands;
        [JsonProperty(PropertyName = "TruePVE Only")]
        public TruePVE tpveS;
    }

    public class TruePVE
    {
        [JsonProperty("Enable damage to players and bases if they have the egg")]
        public bool DisableTruePVE;
        [JsonProperty("Max distance between players for damage to register")]
        public float DamageDistance;
    }

    public class CrateEvent
    {
        [JsonProperty("Run the chinook event")]
        public bool crateEvent;
        [JsonProperty("Number of crates to drop")]
        public int numCrates;
        [JsonProperty("Crate unlock time")]
        public int crateTimer;
        [JsonProperty("Maximum additional items to add to the crate(s)")]
        public int addedItems;
        [JsonProperty("Run the event once, between a certain time")]
        public bool crateTOD;
        [JsonProperty("Start of time period")]
        public string eventStartTime;
        [JsonProperty("End of time period")]
        public string eventEndTime;
        [JsonProperty("Run the event on repeat")]
        public bool crateRepeat;
        [JsonProperty("Minimum time between events (seconds)")]
        public float minStartTime;
        [JsonProperty("Maximum time between events (seconds)")]
        public float maxStartTime;
        [JsonProperty("Don't run the event if server pop is below (leave at 0 to disable)")]
        public int cratePop;
        [JsonProperty("Show map marker")]
        public bool crateMarker;
        [JsonProperty("Marker Radius")]
        public float crateMarkerRadius;
        [JsonProperty("Marker Transparency")]
        public float crateMarkerAlpha;
        [JsonProperty("Marker Color (hex)")]
        public string crateColor1;
        [JsonProperty("Marker Border Color (hex)")]
        public string crateColor2;
    }

    public class ItemCustomisation
    {
        [JsonProperty("Item Name")]
        public string ItemName;
        [JsonProperty("Item Skin ID")]
        public ulong ItemSkin;
        [JsonProperty("Item Found Image in Game (use an image 1000x400)")]
        public string ItemFoundImage;
        [JsonProperty(PropertyName = "Chat command", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string Command;
        [JsonProperty("Item Found Image for the Discord Webhook")]
        public string ItemFoundDiscord;
        [JsonProperty("Item Lost Image for the Discord Webhook")]
        public string ItemLostDiscord;
    }

    public class MarkerSettings
    {
        [JsonProperty("Show map marker when the egg is in a box")]
        public bool boxMarker;
        [JsonProperty("Show map marker when the egg is on a player")]
        public bool playerMarker;
        [JsonProperty("Player marker refresh time (seconds)")]
        public int PlayerMarkerRefreshTime;
        [JsonProperty("Box marker refresh time (seconds)")]
        public int BoxMarkerRefreshTime;
        [JsonProperty("Initial marker delay when the egg is found")]
        public int InitialMarkerDelay;
        [JsonProperty("Marker Radius")]
        public float MarkerRadius;
        [JsonProperty("Marker Transparency")]
        public float MarkerAlpha;
        [JsonProperty("Marker Color (hex)")]
        public string color1;
        [JsonProperty("Marker Border Color (hex)")]
        public string color2;
        [JsonProperty("Add a Vending marker")]
        public bool AddVendingMarker;
        [JsonProperty("Vending Marker Name")]
        public string VendingMarkerName;
    }

    public class RoamSettings
    {
        [JsonProperty("Increase health whilst holding the egg")]
        public bool IncreaseHealth;
        [JsonProperty("Total health")]
        public int TotalHealth;
        [JsonProperty("Increase ore/wood gather rate whilst holding the egg")]
        public bool IncreaseGather;
        [JsonProperty("Gather multipler")]
        public float GatherMultiplier;
        [JsonProperty("Increase pickup amount whilst holding the egg (hemp/food etc)")]
        public bool IncreasePickup;
        [JsonProperty("Pickup multipler")]
        public float PickupMultiplier;
        [JsonProperty("Don't allow roam bonus while server pop is below (leave at 0 to disable)")]
        public int RoamPop;
        [JsonProperty("Don't allow roam bonus between certain hours")]
        public bool RoamTod;
        [JsonProperty("Start of time period")]
        public string RoamStart;
        [JsonProperty("End of time period")]
        public string RoamEnd;
        [JsonProperty("Broadcast a chat message when someone starts roaming")]
        public bool RoamMessage;
    }

    public class ResourceSettings
    {
        [JsonProperty("Resource Spawn Time (seconds)")]
        public int ResourceSpawnTime;
        [JsonProperty("Scrap Spawn Amount (0 to disable)")]
        public int ScrapSpawnAmount;
        [JsonProperty("HQM Spawn Amount")]
        public int HQMSpawnAmount;
        [JsonProperty("Low Grade Spawn Amount")]
        public int LowGradeSpawnAmount;
        [JsonProperty("Metal Frags Spawn Amount")]
        public int FragsSpawnAmount;
        [JsonProperty("Allow Sulfur Ore")]
        public bool AllowSulfurOre;
        [JsonProperty("Sulfur Ore Spawn Amount")]
        public int SulfurOreAmount;
        [JsonProperty("Allow Cooked Sulfur")]
        public bool AllowSulfurCooked;
        [JsonProperty("Cooked Sulfur Spawn Amount")]
        public int SulfurCookedAmount;
        [JsonProperty("Custom Item 1 (use item shortname, eg ammo.rifle, gears, green.berry)")]
        public string CustomItem1;
        [JsonProperty("Custom Item 1 Amount")]
        public int CustomAmount1;
        [JsonProperty("Custom Item 1 Spawn Time (seconds)")]
        public int CustomTime1;
        [JsonProperty("Custom Item 2")]
        public string CustomItem2;
        [JsonProperty("Custom Item 2 Amount")]
        public int CustomAmount2;
        [JsonProperty("Custom Item 2 Spawn Time (seconds)")]
        public int CustomTime2;
        [JsonProperty("Custom Item 3")]
        public string CustomItem3;
        [JsonProperty("Custom Item 3 Amount")]
        public int CustomAmount3;
        [JsonProperty("Custom Item 3 Spawn Time (seconds)")]
        public int CustomTime3;
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

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    private static class Logger
    {
        public static void Info(string message);
        public static void Error(string message);
        public static void Warning(string message);
    }

}

private class StoredData
{
    [JsonProperty]
    internal Hash<ulong, UserData> _eggPossession;
    [JsonProperty]
    internal string _eggCreated;
    public void DestroyTimers();
    public void OnEggMove(IPlayer user);
    public void GetTotals();
    public void GetEggLeaders(List<UserData> list);
    public class UserData
    {
        public double eggtime;
        public string displayName;
        public List<ulong> Team;
        public double TeamTotal;
        [JsonIgnore]
        public Timer _timer;
        private const float timer_Refresh;
        public void OnEggMove(IPlayer user);
        private void StartTimer();
    }

}

public class UserData
{
    public double eggtime;
    public string displayName;
    public List<ulong> Team;
    public double TeamTotal;
    [JsonIgnore]
    public Timer _timer;
    private const float timer_Refresh;
    public void OnEggMove(IPlayer user);
    private void StartTimer();
}

 class DiscordWebhook
{
    public string content { get; set; }
    public List<DiscordEmbed> embeds { get; set; }
    public DiscordWebhook SetContent(string Content);
    public DiscordEmbed AddEmbed();
    public void Send(TheGoldenEgg plugin, string Webhook);
}

 class DiscordEmbed
{
    private DiscordWebhook _webhook;
    public string description { get; set; }
    public string title { get; set; }
    public DateTime timestamp { get; set; }
    public int color { get; set; }
    public List<DiscordEmbedField> fields;
    public DiscordEmbedThumbnail thumbnail { get; set; }
    public DiscordEmbed SetDescription(string Description);
    public DiscordEmbed SetTitle(string Title);
    public DiscordEmbed SetTimestamp(DateTime Timestamp);
    public DiscordEmbed SetColor(int Color);
    public DiscordEmbed AddThumbnail(string Url);
    public DiscordEmbed AddField(string Name, string Value, bool Inline);
    public DiscordEmbed AddFields(IEnumerable<DiscordEmbedField> Fields);
    public DiscordEmbed AddEmbed();
    public DiscordWebhook EndEmbed();
    public DiscordEmbed();
    public DiscordEmbed(DiscordWebhook WebHook);
}

 class DiscordEmbedThumbnail
{
    public string url { get; set; }
}

 class DiscordEmbedField
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public DiscordEmbedField();
    public DiscordEmbedField(string Name, string Value, bool Inline);
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("Egg spawn chance for Locked Crates (0 to 100)")]
    public float LockedCrateEggChance;
    [JsonProperty("Egg spawn chance for Elite Crates")]
    public float EliteCrateEggChance;
    [JsonProperty("Egg spawn chance for Military Crates")]
    public float MilitaryCrateEggChance;
    [JsonProperty("Egg spawn chance for Normal Crates")]
    public float NormalCrateEggChance;
    [JsonProperty(PropertyName = "Marker Settings")]
    public MarkerSettings markerS;
    [JsonProperty(PropertyName = "Resource Settings")]
    public ResourceSettings resourceS;
    [JsonProperty(PropertyName = "Item Customisation")]
    public ItemCustomisation customItemS;
    [JsonProperty(PropertyName = "Roam Settings")]
    public RoamSettings roamS;
    [JsonProperty(PropertyName = "Event Settings")]
    public CrateEvent crateS;
    [JsonProperty("Let players with permission pinpoint the egg on screen (use /egg find)")]
    public bool PinpointEggCommand;
    [JsonProperty("Send a webhook when the egg is found/destroyed")]
    public string sendWebhook;
    [JsonProperty("Don't add time while the player is in a safe zone")]
    public bool BlockSafeZone;
    [JsonProperty("Don't add time while the egg is in a stash")]
    public bool BlockStashTime;
    [JsonProperty("Don't add time while the egg is in a building")]
    public bool NoPrivTime;
    [JsonProperty("Don't add time while server pop is below (leave at 0 to disable)")]
    public int ServerPop;
    [JsonProperty("Don't add time between certain hours")]
    public bool TimeOfDay;
    [JsonProperty("Start of time period")]
    public string StartTime;
    [JsonProperty("End of time period")]
    public string EndTime;
    [JsonProperty("Destroy the egg if in a safe zone for longer than (seconds, leave at 0 to disable)")]
    public float safeDestroy;
    [JsonProperty("Destroy the egg if in a building blocked zone for longer than (seconds, leave at 0 to disable)")]
    public float blockedDestroy;
    [JsonProperty("Destroy the egg if in a Raidable Base zone for longer than (seconds, leave at 0 to disable)")]
    public float raidBaseDestroy;
    [JsonProperty("Name of permission group to grant with /egg winner (requires Timed Permissions plugin)")]
    public string GroupName;
    [JsonProperty("Duration to grant access to group (requires Timed Permissions plugin). Format: 1d12h30m")]
    public string GroupDuration;
    [JsonProperty("Destroy the egg after x minutes (leave at 0 to disable)")]
    public int KillTime;
    [JsonProperty("Block player from cracking open the egg while being raided")]
    public bool BlockEggCrack;
    [JsonProperty("Raid block timer")]
    public int BlockInterval;
    [JsonProperty("Data save interval")]
    public int SaveInterval;
    [JsonProperty("Clear data on map wipe")]
    public bool ClearData;
    [JsonProperty("Use custom item list when cracking open the egg")]
    public bool UseCustomList;
    [JsonProperty("Custom item list (use item shortname, eg rifle.m39, explosive.timed, etc")]
    public List<string> EggRewards;
    [JsonProperty("Blacklist commands whilst holding the egg")]
    public bool UseBlacklist;
    [JsonProperty("Blacklisted commands")]
    public List<string> BlacklistedCommands;
    [JsonProperty(PropertyName = "TruePVE Only")]
    public TruePVE tpveS;
}

public class TruePVE
{
    [JsonProperty("Enable damage to players and bases if they have the egg")]
    public bool DisableTruePVE;
    [JsonProperty("Max distance between players for damage to register")]
    public float DamageDistance;
}

public class CrateEvent
{
    [JsonProperty("Run the chinook event")]
    public bool crateEvent;
    [JsonProperty("Number of crates to drop")]
    public int numCrates;
    [JsonProperty("Crate unlock time")]
    public int crateTimer;
    [JsonProperty("Maximum additional items to add to the crate(s)")]
    public int addedItems;
    [JsonProperty("Run the event once, between a certain time")]
    public bool crateTOD;
    [JsonProperty("Start of time period")]
    public string eventStartTime;
    [JsonProperty("End of time period")]
    public string eventEndTime;
    [JsonProperty("Run the event on repeat")]
    public bool crateRepeat;
    [JsonProperty("Minimum time between events (seconds)")]
    public float minStartTime;
    [JsonProperty("Maximum time between events (seconds)")]
    public float maxStartTime;
    [JsonProperty("Don't run the event if server pop is below (leave at 0 to disable)")]
    public int cratePop;
    [JsonProperty("Show map marker")]
    public bool crateMarker;
    [JsonProperty("Marker Radius")]
    public float crateMarkerRadius;
    [JsonProperty("Marker Transparency")]
    public float crateMarkerAlpha;
    [JsonProperty("Marker Color (hex)")]
    public string crateColor1;
    [JsonProperty("Marker Border Color (hex)")]
    public string crateColor2;
}

public class ItemCustomisation
{
    [JsonProperty("Item Name")]
    public string ItemName;
    [JsonProperty("Item Skin ID")]
    public ulong ItemSkin;
    [JsonProperty("Item Found Image in Game (use an image 1000x400)")]
    public string ItemFoundImage;
    [JsonProperty(PropertyName = "Chat command", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string Command;
    [JsonProperty("Item Found Image for the Discord Webhook")]
    public string ItemFoundDiscord;
    [JsonProperty("Item Lost Image for the Discord Webhook")]
    public string ItemLostDiscord;
}

public class MarkerSettings
{
    [JsonProperty("Show map marker when the egg is in a box")]
    public bool boxMarker;
    [JsonProperty("Show map marker when the egg is on a player")]
    public bool playerMarker;
    [JsonProperty("Player marker refresh time (seconds)")]
    public int PlayerMarkerRefreshTime;
    [JsonProperty("Box marker refresh time (seconds)")]
    public int BoxMarkerRefreshTime;
    [JsonProperty("Initial marker delay when the egg is found")]
    public int InitialMarkerDelay;
    [JsonProperty("Marker Radius")]
    public float MarkerRadius;
    [JsonProperty("Marker Transparency")]
    public float MarkerAlpha;
    [JsonProperty("Marker Color (hex)")]
    public string color1;
    [JsonProperty("Marker Border Color (hex)")]
    public string color2;
    [JsonProperty("Add a Vending marker")]
    public bool AddVendingMarker;
    [JsonProperty("Vending Marker Name")]
    public string VendingMarkerName;
}

public class RoamSettings
{
    [JsonProperty("Increase health whilst holding the egg")]
    public bool IncreaseHealth;
    [JsonProperty("Total health")]
    public int TotalHealth;
    [JsonProperty("Increase ore/wood gather rate whilst holding the egg")]
    public bool IncreaseGather;
    [JsonProperty("Gather multipler")]
    public float GatherMultiplier;
    [JsonProperty("Increase pickup amount whilst holding the egg (hemp/food etc)")]
    public bool IncreasePickup;
    [JsonProperty("Pickup multipler")]
    public float PickupMultiplier;
    [JsonProperty("Don't allow roam bonus while server pop is below (leave at 0 to disable)")]
    public int RoamPop;
    [JsonProperty("Don't allow roam bonus between certain hours")]
    public bool RoamTod;
    [JsonProperty("Start of time period")]
    public string RoamStart;
    [JsonProperty("End of time period")]
    public string RoamEnd;
    [JsonProperty("Broadcast a chat message when someone starts roaming")]
    public bool RoamMessage;
}

public class ResourceSettings
{
    [JsonProperty("Resource Spawn Time (seconds)")]
    public int ResourceSpawnTime;
    [JsonProperty("Scrap Spawn Amount (0 to disable)")]
    public int ScrapSpawnAmount;
    [JsonProperty("HQM Spawn Amount")]
    public int HQMSpawnAmount;
    [JsonProperty("Low Grade Spawn Amount")]
    public int LowGradeSpawnAmount;
    [JsonProperty("Metal Frags Spawn Amount")]
    public int FragsSpawnAmount;
    [JsonProperty("Allow Sulfur Ore")]
    public bool AllowSulfurOre;
    [JsonProperty("Sulfur Ore Spawn Amount")]
    public int SulfurOreAmount;
    [JsonProperty("Allow Cooked Sulfur")]
    public bool AllowSulfurCooked;
    [JsonProperty("Cooked Sulfur Spawn Amount")]
    public int SulfurCookedAmount;
    [JsonProperty("Custom Item 1 (use item shortname, eg ammo.rifle, gears, green.berry)")]
    public string CustomItem1;
    [JsonProperty("Custom Item 1 Amount")]
    public int CustomAmount1;
    [JsonProperty("Custom Item 1 Spawn Time (seconds)")]
    public int CustomTime1;
    [JsonProperty("Custom Item 2")]
    public string CustomItem2;
    [JsonProperty("Custom Item 2 Amount")]
    public int CustomAmount2;
    [JsonProperty("Custom Item 2 Spawn Time (seconds)")]
    public int CustomTime2;
    [JsonProperty("Custom Item 3")]
    public string CustomItem3;
    [JsonProperty("Custom Item 3 Amount")]
    public int CustomAmount3;
    [JsonProperty("Custom Item 3 Spawn Time (seconds)")]
    public int CustomTime3;
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

private static class Logger
{
    public static void Info(string message);
    public static void Error(string message);
    public static void Warning(string message);
}


```

---

## ThiefAPI

```csharp
using System;
using System.Reflection;
using System.Data;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Thief", "Alphawar", "0.5.0", ResourceId = 1503)]
[Description("Mod for handling Thief class.")]
 class ThiefAPI : RustPlugin
{
    private int Cooldown;
    private int Pass;
    private int LPchance;
    private int RandomNumber;
    private int DamageTicks;
    private int pickCostFM;
    private int pickCostSE;
    private float DamageAmount;
    private string NotAllowed;
    private string Allowed;
    private string Failed;
    private string Success;
    private string MaxedAttempts;
    private string ChatPrefixColor;
    private string ChatPrefix;
    private string PickEnabledMessage;
    private string PickDisabledMessage;
    private string CooldownMessage;
    private bool PickEnabled;
     object GetConfig(string menu, string dataValue, object defaultValue);
    protected override void LoadDefaultConfig();
     void LoadVariables();
     Hash<ulong, PlayerInfo> Thiefs;
     class PlayerInfo
    {
        public string UserId;
        public string Name;
        public PlayerInfo();
        public PlayerInfo(BasePlayer player);
        public ulong GetUserId();
    }

     class StoredData
    {
        public Dictionary<ulong, double> canpick;
        public HashSet<PlayerInfo> Thiefs;
        public StoredData();
    }

     StoredData storedData;
     System.Reflection.FieldInfo whitelistPlayers;
    private void Loaded();
    [ChatCommand("lp")]
     void picklock(BasePlayer player, string cmd, string[] args);
    [ChatCommand("lpwipe")]
     void picklockwipe(BasePlayer player, string cmd, string[] args);
     object CanUseDoor(BasePlayer player, BaseLock codeLock);
    private bool lockPickCost(BasePlayer player);
     bool LPRoll();
     bool IsAllowed(BasePlayer player, string perm, string reason);
     double GetTimeStamp();
     void DisableThiefMode(BasePlayer player);
     void EnableThiefMode(BasePlayer player);
     void SendChatMessage(BasePlayer player, string message, string args);
}

 class PlayerInfo
{
    public string UserId;
    public string Name;
    public PlayerInfo();
    public PlayerInfo(BasePlayer player);
    public ulong GetUserId();
}

 class StoredData
{
    public Dictionary<ulong, double> canpick;
    public HashSet<PlayerInfo> Thiefs;
    public StoredData();
}


```

---

## Tickets

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Tickets", "LaserHydra", "2.0.0", ResourceId = 1065)]
[Description("Gives players the opportunity to send Tickets to admins.")]
 class Tickets : RustPlugin
{
     class Ticket
    {
        public int ticketID;
        public string steamID;
        public string player;
        public string profile;
        public string position;
        public string message;
        public string reply;
        public float x;
        public float y;
        public float z;
        public string timestamp;
        public Ticket(int id, BasePlayer Player, string msg);
        public Ticket();
    }

     class Data
    {
        public List<Ticket> tickets;
        public Data();
    }

     Data data;
     void Loaded();
     Data LoadData();
     void SaveData();
     void LoadConfig();
     void LoadDefaultConfig();
     void OnPlayerInit(BasePlayer player);
     void SendToAPI(Ticket ticket);
     void CheckRepliedTickets(BasePlayer player);
     bool CheckUnrepliedTickets();
     int GetNewID();
     Ticket GetTicketByID(int id);
     bool IsAdmin(BasePlayer player, bool reply);
     void ShowSyntax(BasePlayer player);
    [ChatCommand("clear")]
     void Clear(BasePlayer player);
    [ChatCommand("ticket")]
     void cmdTicket(BasePlayer player, string cmd, string[] args);
     void TicketFunction(string function, string arg, BasePlayer player);
     void SendTicketReply(string arg);
     void Teleport(BasePlayer player, Vector3 destination);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ListToString(List<string> list, int first, string seperator);
     void SetConfig(string Arg1, object Arg2, object Arg3, object Arg4);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Ticket
{
    public int ticketID;
    public string steamID;
    public string player;
    public string profile;
    public string position;
    public string message;
    public string reply;
    public float x;
    public float y;
    public float z;
    public string timestamp;
    public Ticket(int id, BasePlayer Player, string msg);
    public Ticket();
}

 class Data
{
    public List<Ticket> tickets;
    public Data();
}


```

---

## TimedEvents

```csharp
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Timed Events", "Orange", "1.1.5")]
[Description("Triggers various types of events like Airdrops, Helicopters and same")]
public class TimedEvents : RustPlugin
{
    private const string prefabCH47;
    private const string prefabPlane;
    private const string prefabShip;
    private const string prefabPatrol;
    private void OnServerInitialized();
    private object OnEventTrigger(TriggeredEventPrefab info);
    private void SpawnTank(bool skipSpawn);
    private void SpawnShip(bool skipSpawn);
    private void SpawnPatrol(bool skipSpawn);
    private void SpawnPlane(bool skipSpawn);
    private void SpawnCH47(bool skipSpawn);
    private int Online();
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. Cargo plane settings:")]
        public EventSettings plane;
        [JsonProperty(PropertyName = "2. Patrol Helicopter settings:")]
        public EventSettings patrol;
        [JsonProperty(PropertyName = "3. Bradley APC settings:")]
        public EventSettings tank;
        [JsonProperty(PropertyName = "4. CH47 settings:")]
        public EventSettings ch47;
        [JsonProperty(PropertyName = "5. Cargo ship settings:")]
        public EventSettings ship;
    }

    private class EventSettings
    {
        [JsonProperty(PropertyName = "1. Disable default spawns")]
        public bool disableDefault;
        [JsonProperty(PropertyName = "2. Minimal respawn time (in seconds)")]
        public int timerMin;
        [JsonProperty(PropertyName = "3. Maximal respawn time (in seconds)")]
        public int timerMax;
        [JsonProperty(PropertyName = "4. Minimal amount that spawned by once")]
        public int spawnMin;
        [JsonProperty(PropertyName = "5. Maximal amount that spawned by once")]
        public int spawnMax;
        [JsonProperty(PropertyName = "6. Minimal players to start event")]
        public int playersMin;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. Cargo plane settings:")]
    public EventSettings plane;
    [JsonProperty(PropertyName = "2. Patrol Helicopter settings:")]
    public EventSettings patrol;
    [JsonProperty(PropertyName = "3. Bradley APC settings:")]
    public EventSettings tank;
    [JsonProperty(PropertyName = "4. CH47 settings:")]
    public EventSettings ch47;
    [JsonProperty(PropertyName = "5. Cargo ship settings:")]
    public EventSettings ship;
}

private class EventSettings
{
    [JsonProperty(PropertyName = "1. Disable default spawns")]
    public bool disableDefault;
    [JsonProperty(PropertyName = "2. Minimal respawn time (in seconds)")]
    public int timerMin;
    [JsonProperty(PropertyName = "3. Maximal respawn time (in seconds)")]
    public int timerMax;
    [JsonProperty(PropertyName = "4. Minimal amount that spawned by once")]
    public int spawnMin;
    [JsonProperty(PropertyName = "5. Maximal amount that spawned by once")]
    public int spawnMax;
    [JsonProperty(PropertyName = "6. Minimal players to start event")]
    public int playersMin;
}


```

---

## TimedExecute

```csharp
using System.Collections.Generic;
using System.Reflection;
using System;
using System.Linq;
using System.Data;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Timed Execute", "PaiN", 2.6, ResourceId = 919)]
[Description("Execute commands every (x) seconds.")]
 class TimedExecute : RustPlugin
{
    private Timer repeater;
    private Timer chaintimer;
    private Timer checkreal;
     void Loaded();
     void RunRepeater();
     void RealTime();
     void RunOnce();
     void Unloaded();
     Dictionary<string, object> repeatcmds;
     Dictionary<string, object> chaincmds;
     Dictionary<string, object> realtimecmds;
    protected override void LoadDefaultConfig();
    [ConsoleCommand("reset.oncetimer")]
     void cmdResOnceTimer(ConsoleSystem.Arg arg);
}


```

---

## TimedHeliCall

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("TimedHeliCall", "Troubled", 0.1)]
 class TimedHeliCall : RustPlugin
{
    private int _heliInterval;
    private int _x;
    private int _z;
    protected override void LoadDefaultConfig();
     void Init();
    private int GetIntConfig(string configkey);
    private void CallHeli();
}


```

---

## TimedItemsBlocker

```csharp
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("TimedItemsBlocker", "Vlad-00003", "1.0.0")]
[Description("Prevents some items from being used for a limited period of time.")]
 class TimedItemsBlocker : RustPlugin
{
    private string PanelName;
    private Dictionary<string, int> BlockedItems;
    private Dictionary<string, int> BlockedClothes;
    private DateTime DateOfWipe;
    private bool UseChat;
    private bool Wipe;
    private string Prefix;
    private string PrefixColor;
    private string BypassPermission;
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
    private void LoadMessages();
     void Loaded();
     object CanEquipItem(PlayerInventory inventory, Item item);
     object CanWearItem(PlayerInventory inventory, Item item);
    private void BlockerUi(BasePlayer player, string inputText);
    private bool InBlock(int EndTime);
     TimeSpan TimeLeft(int hours);
     string GetMsg(string key, object userID);
     string TimeToString(DateTime time);
    private void SendToChat(BasePlayer Player, string Message);
    private void SendToChat(string Message);
    private void GetConfig(string Key, T var);
}


```

---

## TimedNotifications

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Game.Rust.Libraries;

Oxide.Plugins
[Info("Timed Notifications", "Hirsty", "0.0.4", ResourceId = 1277)]
[Description("Depending on Preset Times, this plugin will display a popup with a notification.")]
 class TimedNotifications : RustPlugin
{
    public static string version;
    public bool PopUpNotifier;
    public string[] days;
    public string TZ;
    public int hour;
    public DateTime setDate;
    public int min;
     char sep;
     char sep2;
    [PluginReference]
     Plugin PopupNotifications;
     class StoredData
    {
        public HashSet<EventData> Events;
        public StoredData();
    }

     class EventData
    {
        public DateTime EventDate;
        public string EventInfo;
        public string CommandLine;
        public bool broadcast;
        public string Schedule;
        public EventData();
        public EventData(DateTime Date, string info, string EventType);
    }

     StoredData storedData;
    protected override void LoadDefaultConfig();
    private void Loaded();
     bool hasPermission(BasePlayer player, string permname);
    private void LoadData();
    [ChatCommand("notification")]
    private void notification(BasePlayer player, string command, string[] args);
     void EventCheck();
     void SendHelp(BasePlayer player);
     void SendNotification(string message, int delay, BasePlayer player);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class StoredData
{
    public HashSet<EventData> Events;
    public StoredData();
}

 class EventData
{
    public DateTime EventDate;
    public string EventInfo;
    public string CommandLine;
    public bool broadcast;
    public string Schedule;
    public EventData();
    public EventData(DateTime Date, string info, string EventType);
}


```

---

## TimedPermissions

```csharp
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Timed Permissions", "LaserHydra", "1.2.1", ResourceId = 1926)]
[Description("Allows you to grant permissions or groups for a specific time")]
 class TimedPermissions : CovalencePlugin
{
    static TimedPermissions Instance;
    static List<Player> _players;
     class Player
    {
         PluginTimers timer;
        public List<TimedPermission> permissions;
        public List<TimedGroup> groups;
        public string name;
        public string steamID;
        public Player();
        internal Player(IPlayer player);
        internal Player(string steamID);
        internal static Player Get(IPlayer player);
        internal static Player Get(string steamID);
        internal static Player GetOrCreate(IPlayer player);
        internal static Player GetOrCreate(string steamID);
        internal void AddPermission(string permission, DateTime expireDate);
        internal void RemovePermission(string permission);
        internal void AddGroup(string group, DateTime expireDate);
        internal void RemoveGroup(string group);
        internal void UpdatePlayer(IPlayer player);
        internal void Update();
         List<T> CopyList(List<T> list);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }

     class TimedPermission
    {
        public string permission;
        public string _expireDate;
        internal DateTime expireDate { get; set; }
        internal bool Expired { get; set; }
        public TimedPermission();
        internal TimedPermission(string permission, DateTime expireDate);
        internal static TimedPermission Get(string permission, Player player);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }

     class TimedGroup
    {
        public string group;
        public string _expireDate;
        internal DateTime expireDate { get; set; }
        internal bool Expired { get; set; }
        public TimedGroup();
        internal TimedGroup(string group, DateTime expireDate);
        internal static TimedGroup Get(string group, Player player);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }

     void Loaded();
     void LoadMessages();
    [Command("grantperm", "global.grantperm"), Permission("timedpermissions.use")]
     void cmdGrantPerm(IPlayer player, string cmd, string[] args);
    [Command("addgroup", "global.addgroup"), Permission("timedpermissions.use")]
     void cmdAddGroup(IPlayer player, string cmd, string[] args);
    [Command("pinfo", "global.pinfo"), Permission("timedpermissions.use")]
     void cmdPlayerInfo(IPlayer player, string cmd, string[] args);
     bool TryGetDateTime(string source, DateTime date);
     IPlayer GetPlayer(string searchedPlayer, IPlayer player);
     string ListToString(List<T> list, int first, string seperator);
     bool TryConvert(S source, C converted);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void LoadData(T data, string filename);
    static void SaveData(T data, string filename);
     string GetMsg(string key, object userID);
     void RegisterPerm(string[] permArray);
     bool HasPerm(object uid, string[] permArray);
     string PermissionPrefix { get; set; }
    static void Print(string message);
}

 class Player
{
     PluginTimers timer;
    public List<TimedPermission> permissions;
    public List<TimedGroup> groups;
    public string name;
    public string steamID;
    public Player();
    internal Player(IPlayer player);
    internal Player(string steamID);
    internal static Player Get(IPlayer player);
    internal static Player Get(string steamID);
    internal static Player GetOrCreate(IPlayer player);
    internal static Player GetOrCreate(string steamID);
    internal void AddPermission(string permission, DateTime expireDate);
    internal void RemovePermission(string permission);
    internal void AddGroup(string group, DateTime expireDate);
    internal void RemoveGroup(string group);
    internal void UpdatePlayer(IPlayer player);
    internal void Update();
     List<T> CopyList(List<T> list);
    public override bool Equals(object obj);
    public override int GetHashCode();
}

 class TimedPermission
{
    public string permission;
    public string _expireDate;
    internal DateTime expireDate { get; set; }
    internal bool Expired { get; set; }
    public TimedPermission();
    internal TimedPermission(string permission, DateTime expireDate);
    internal static TimedPermission Get(string permission, Player player);
    public override bool Equals(object obj);
    public override int GetHashCode();
}

 class TimedGroup
{
    public string group;
    public string _expireDate;
    internal DateTime expireDate { get; set; }
    internal bool Expired { get; set; }
    public TimedGroup();
    internal TimedGroup(string group, DateTime expireDate);
    internal static TimedGroup Get(string group, Player player);
    public override bool Equals(object obj);
    public override int GetHashCode();
}


```

---

## TimeOfDay

```csharp
using Oxide.Core.Plugins;
using System;
using System.Text;

Oxide.Plugins
[Info("TimeOfDay", "Sin", "1.0.1", ResourceId = 1355)]
[Description("Provides tools for managing time. It can also alter day and night duration.")]
public class TimeOfDay : RustPlugin
{
    public float SunriseHour { get; set; }
    public float SunsetHour { get; set; }
    public uint Daylength { get; set; }
    public uint Nightlength { get; set; }
    public bool IsDay { get; set; }
    public bool IsNight { get; set; }
    private uint componentSearchAttempts;
    private TOD_Time timeComponent;
    private void Loaded();
    private void LoadDefaultConfig();
    private void GetTimeComponent();
    private void UpdateTimeOnSunrise();
    private void UpdateTimeOnSunset();
    [ChatCommand("tod")]
    private void TodCommand(BasePlayer Player, string Command, string[] Args);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer Player);
    private void PrintPluginMessageToChat(BasePlayer Player, string Message);
    private void PrintPluginMessageToChat(string Message);
    private string FormNeutralMessage(string Message);
    private string FormWarningMessage(string Message);
    private string FormErrorMessage(string Message);
    private bool CheckForPermission(BasePlayer Player);
}


```

---

## TimeSet

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Collections;
using System.Text.RegularExpressions;
using UnityEngine;
using Rust;
using Network;
using System.Reflection;
using Facepunch;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Time", "Tsunderella", "1.2.0")]
[Description("Adds Time Command.")]
 class TimeSet : RustPlugin
{
     bool readyToCheck;
    private float toff;
    private float number;
    private float secondnumber;
    private bool cooldown;
    private string meridiem;
    private string Timeofday;
    private string arguments;
    private bool TimeStopped;
    private float TimeStoppedAt;
     HashSet<ulong> users;
     void Init();
     void Unloaded();
    [ChatCommand("time")]
     void cmdtTime(BasePlayer player, string cmd, string[] args);
    [ChatCommand("day")]
     void cmdtDay(BasePlayer player);
    [ChatCommand("night")]
     void cmdtNight(BasePlayer player);
    [HookMethod("OnTick")]
    private void OnTick();
    [HookMethod("whatisthetime")]
    private string whatisthetime();
     void Loaded();
     string GetMessage(string name, string sid);
     bool pPerm(BasePlayer player, string perm);
}


```

---

## TimeSystem

```csharp
using Oxide.Core.Plugins;
using Oxide.Core;
using System;
using System.Text;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Time System", "Nimant", "1.0.1")]
public class TimeSystem : RustPlugin
{
    private bool Initialized;
    private int componentSearchAttempts;
    private TOD_Time timeComponent;
    private bool activatedDay;
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private void SetTimeComponent();
    private void OnHour();
    private void OnSunrise();
    private void OnSunset();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Час когда начинается день (восход)")]
        public int DayStart;
        [JsonProperty(PropertyName = "Час когда начинается ночь (закат)")]
        public int NightStart;
        [JsonProperty(PropertyName = "Длительность дня (в минутах)")]
        public int DayLength;
        [JsonProperty(PropertyName = "Длительность ночи (в минутах)")]
        public int NightLength;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void SaveConfig(ConfigData config);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Час когда начинается день (восход)")]
    public int DayStart;
    [JsonProperty(PropertyName = "Час когда начинается ночь (закат)")]
    public int NightStart;
    [JsonProperty(PropertyName = "Длительность дня (в минутах)")]
    public int DayLength;
    [JsonProperty(PropertyName = "Длительность ночи (в минутах)")]
    public int NightLength;
}


```

---

## Top

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Top", "RustPlugin.ru", "1.1.0")]
 class Top : RustPlugin
{
    private int MessageNum;
    protected override void LoadDefaultConfig();
    [ChatCommand("rank")]
     void TurboRankCommand(BasePlayer player, string command);
    [ChatCommand("top")]
     void TurboTopCommand(BasePlayer player, string command, string[] args);
    [ConsoleCommand("top.show")]
    private void TopShowOpenCmd2(ConsoleSystem.Arg arg);
     CuiElementContainer CreatePanel(string number);
    [ConsoleCommand("top.exit")]
    private void MagazineOpenCmd2(ConsoleSystem.Arg arg);
    private Dictionary<uint, string> LastHeliHit;
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void OnEntityDeath(BaseCombatEntity victim, HitInfo info);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProtoBuf.ProjectileShoot projectiles);
     void OnItemCraftFinished(ItemCraftTask task, Item item);
     void OnServerSave();
     void Unload();
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void DoGather(BasePlayer player, Item item);
     void CreateInfo(BasePlayer player);
     void OnPlayerInit(BasePlayer player);
     void Loaded();
     void Saved();
    private bool IsNPC(BasePlayer player);
    public List<TopData> Tops;
    public class TopData
    {
        public TopData(string Ник, string UID);
        public void Reset();
        public string Ник { get; set; }
        public string UID { get; set; }
        public int РакетВыпущено { get; set; }
        public int УбийствPVP { get; set; }
        public int ВзрывчатокИспользовано { get; set; }
        public int УбийствЖивотных { get; set; }
        public int ПульВыпущено { get; set; }
        public int СтрелВыпущено { get; set; }
        public int Смертей { get; set; }
        public int ПредметовСкрафчено { get; set; }
        public int РесурсовСобрано { get; set; }
        public int ВертолётовУничтожено { get; set; }
        public int ТанковУничтожено { get; set; }
        public int NPCУбито { get; set; }
    }

}

public class TopData
{
    public TopData(string Ник, string UID);
    public void Reset();
    public string Ник { get; set; }
    public string UID { get; set; }
    public int РакетВыпущено { get; set; }
    public int УбийствPVP { get; set; }
    public int ВзрывчатокИспользовано { get; set; }
    public int УбийствЖивотных { get; set; }
    public int ПульВыпущено { get; set; }
    public int СтрелВыпущено { get; set; }
    public int Смертей { get; set; }
    public int ПредметовСкрафчено { get; set; }
    public int РесурсовСобрано { get; set; }
    public int ВертолётовУничтожено { get; set; }
    public int ТанковУничтожено { get; set; }
    public int NPCУбито { get; set; }
}


```

---

## ToukDurability

```csharp
using System;

Oxide.Plugins
[Info("ToukDurability", "Touk", "1.0.0")]
[Description("Customize durability")]
public class ToukDurability : RustPlugin
{
     float durabilityRatio;
    protected override void LoadDefaultConfig();
     void OnLoseCondition(Item item, float amount);
     T GetConfig(string name, T defaultValue);
}


```

---

## Tournament

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
[Info("Tournament", "Xavier", "1.0.0")]
public class Tournament : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    public class CupSettings
    {
        public ulong ownerclan;
        public bool isremove;
        public uint build;
        public VendingMachineMapMarker Marker;
    }

    public List<CupSettings> ClanData;
     void AddMarker(CupSettings settings);
     void RemoveMarker(CupSettings settings);
     void OnServerInitialized();
     void OnServerSave();
    private Configur config;
    private class Configur
    {
        [JsonProperty("Изначальное время в секундах")]
        public int times;
        public static Configur GetNewConfiguration();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void ReadData();
     void WriteData();
     TimeSpan time;
    public static string FormatShortTime(TimeSpan time);
    [ConsoleCommand("tournament")]
     void testcm(ConsoleSystem.Arg arg);
     void Unload();
     string CanRemove(BasePlayer player, BaseEntity entity);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     bool CheckTournament(ulong owner);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    public string CupLayer;
    [ConsoleCommand("registration.clanssss")]
     void RegClan(ConsoleSystem.Arg args);
    private static string HexToCuiColor(string hex);
     void ui(BasePlayer player);
    private readonly Dictionary<ulong, Timer> timers;
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
     object OnEntityGroundMissing(BaseEntity entity);
}

public class CupSettings
{
    public ulong ownerclan;
    public bool isremove;
    public uint build;
    public VendingMachineMapMarker Marker;
}

private class Configur
{
    [JsonProperty("Изначальное время в секундах")]
    public int times;
    public static Configur GetNewConfiguration();
}


```

---

## TownWars

```csharp
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Rust;
using System.Linq;
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using Oxide.Core;
using System.Globalization;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("TownWars", "Qbis", "1.0.4")]
public class TownWars : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin Clans;
    private List<GameObject> townObjects;
    private Dictionary<string, List<inventoryData>> inventorysCache;
    private static TownWars plugin;
    public class townsData
    {
        public string OwnerID;
        public string OwnerName;
        public int lastReward;
        public int lastCapture;
    }

    public class inventoryData
    {
        public string shortName;
        public ulong skinID;
        public int amount;
        public string customName;
        public int itemID;
        public string command;
    }

    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty("Общие настройки")]
        public Settings settings;
        [JsonProperty("Настройки маркеров")]
        public MarkersSettings marker;
        [JsonProperty("Настройки РТ")]
        public Dictionary<string, TownSettings> towns;
        [JsonProperty("Настройки UI")]
        public UiSettings ui;
        [JsonProperty("Версия конфига")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

    public class Settings
    {
        [JsonProperty("Тип работы плагина Team - обычные команды(зеленка), Solo - для соло захвата, Clans - поддержка кланов ClanReborn(Chaos), ClansUI(RP), Clans(Umod)")]
        public string typeTeam;
        [JsonProperty("Переодичность выдачи наград в секундах")]
        public int addRewardSettings;
        [JsonProperty("Предметы из инвентаря может забрать только глава клана")]
        public bool allCanTakeReward;
        [JsonProperty("Начать захват может только лидер клана или группы")]
        public bool allCanStartCapt;
        [JsonProperty("Удалять ли инвентарь предметов после вайпа")]
        public bool clearInventoryAfterWipe;
        [JsonProperty("Откат до следующего захвата в секундах")]
        public int delayCapture;
        [JsonProperty("Сколько секунд длится захват")]
        public int durCapture;
        [JsonProperty("Добавлять ли видимые сферы, для обозначения границ захвата")]
        public bool addSphere;
        [JsonProperty("Минимум игроков на сервере для начала захвата (0 - выкл)")]
        public int minPlayers;
    }

    public class MarkersSettings
    {
        [JsonProperty("Радиус маркера")]
        public float markerRadius;
        [JsonProperty("Прозрачность маркера")]
        public float markerAlpha;
        [JsonProperty("Цвет маркера когда РТ можно захватить")]
        public string markerColorCanCapture;
        [JsonProperty("Цвет маркера когда РТ захватывают")]
        public string markerColorCapture;
        [JsonProperty("Цвет маркера когда РТ нельзя захваить")]
        public string markerColorCantCapture;
        [JsonProperty("Добавлять ли название на карту маркер с именем, кто последний захватил РТ")]
        public bool addMarkerWithLast;
    }

    public class TownSettings
    {
        [JsonProperty("Название РТ")]
        public string name;
        [JsonProperty("На каком расстояние от центра РТ начислять очки захвата")]
        public int capDist;
        [JsonProperty("Список наград")]
        public List<Reward> rewards;
    }

    public class UiSettings
    {
        [JsonProperty("Цвет фона")]
        public string colorBG;
        [JsonProperty("Цвет обводки")]
        public string colorLines;
        [JsonProperty("Цвет кнопки 'Начать захват'")]
        public string colorButtonCapture;
        [JsonProperty("Цвет кнопки 'Инвентарь'")]
        public string colorButtonInventory;
    }

    public class Reward
    {
        [JsonProperty("Shortname предмета")]
        public string shortName;
        [JsonProperty("Количество предмета")]
        public int amount;
        [JsonProperty("Скин айди предмета")]
        public ulong skinID;
        [JsonProperty("Имя предмета (если кастом)")]
        public string customName;
        [JsonProperty("Ссылка на картинку (если кастом)")]
        public string imageUrl;
        [JsonProperty("Команда для выполнения %STEAMID% (для загрузки картинки придумайте любой номер SkinID и ShortName)")]
        public string command;
    }

    protected override void LoadDefaultMessages();
     string GetMsg(string key, BasePlayer player);
     string GetMsg(string key);
    private void OnServerInitialized();
    private void Init();
    private void Unload();
    private void OnNewSave();
    private void OnPlayerConnected(BasePlayer player);
    private object OnPlayerDeath(BasePlayer player, HitInfo info);
    private void CheckPlayerInZone(BasePlayer player);
    private void BroadCastToStartCapture(string rt);
    private void BroadCastToStopCapture(string owner, string rt);
    private string GetPlayerClan(BasePlayer player);
    private string GetPlayerClanName(BasePlayer player);
    private bool isPlayerOwner(BasePlayer player);
    private void AddReward(string id, string town);
    private string isPlayerInZoneAndCapt(BasePlayer player);
    private void CreateTownWarsMainMenu(BasePlayer player, int page);
    private void CreateTownWarsInventory(BasePlayer player, int page);
    private void CreateTownItems(BasePlayer player, int id, int page);
    [ConsoleCommand("UI_CLOSE_CAPT")]
    private void cmd_UI_CLOSE_CAPT(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_RETURN_MAIN")]
    private void cmd_UI_RETURN_MAIN(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SHOW_CAPTURE_ITEMS")]
    private void cmd_UI_SHOW_CAPTURE_ITEMS(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_START_CAPT")]
    private void cmd_UI_START_CAPT(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CHANGE_MAIN_PAGE")]
    private void cmd_UI_CHANGE_MAIN_PAGE(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CHANGE_INV_PAGE")]
    private void cmd_UI_CHANGE_INV_PAGE(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_INV_PAGE_GET_ITEM")]
    private void cmd_UI_INV_PAGE_GET_ITEM(ConsoleSystem.Arg arg);
    public class UI
    {
        public static void CreateOutLines(CuiElementContainer container, string parent, string color);
        public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, string name, float FadeIn);
        public static void CreatePanel(CuiElementContainer container, string name, string parent, string color, string aMin, string aMax, float Fadeout, float Fadein);
        public static void CreatePanelBlur(CuiElementContainer container, string name, string parent, string color, string aMin, string aMax, float Fadeout, float Fadein);
        public static void CreateText(CuiElementContainer container, string parent, string text, string color, string aMin, string aMax, TextAnchor align, int size, string name, float Fadein);
        public static void CreateTextOutLine(CuiElementContainer container, string parent, string text, string color, string aMin, string aMax, TextAnchor align, int size, string name, float Fadein);
    }

    public void CreateImage(CuiElementContainer container, string name, string panel, string color, string image, string aMin, string aMax, float Fadeout, float Fadein, ulong skin);
    private void SaveTownsData();
    private void LoadTownsData();
    private void clearTownsData(bool load);
    private void AddInventoryToCache(string id);
    private void SaveInventoryItem(string id);
    private void SaveInventorys();
    private void clearInventory(string id);
    private void wipeInventorys();
    public class Capture : MonoBehaviour
    {
        public string monumentName;
        public string Name;
        public float distance;
        public string OwnerId;
        public string OwnerName;
        public int lastReward;
        public int lastCapture;
        public bool isCapture;
        public int id;
        private int timerSec;
        private List<BasePlayer> players;
        private Dictionary<string, teamInfo> capTeams;
        private SphereCollider sphereCollider;
        private BaseEntity[] spheres;
        private MapMarkerGenericRadius mapMarker;
        private VendingMachineMapMarker vendingMarker;
        public void DestroyComp();
        private void OnDestroy();
        private void Awake();
        public void Initialize(string monName, string townName, int capDist, string oId, string oName, int reward, int capt);
        private void OnTriggerEnter(Collider other);
        private void OnTriggerExit(Collider other);
        private void Timer();
        private void DrawInfo(BasePlayer player);
        public void StartCapture(BasePlayer player);
        private void StopCapture();
        private void UpdateMarker();
        private void RemoveMarker();
        private void CreateMarker(string color);
        private void CreateCaptureSphere();
        private void RemoveCaptureSphere();
        private void SetPlayerFlag(BasePlayer player, BasePlayer.PlayerFlags f, bool b);
        private Color ConvertToColor(string color);
        public bool isPlayerInZone(BasePlayer player);
        public bool removePlayerFromZone(BasePlayer player);
        public string GetStatus();
        public class teamInfo
        {
            public string id;
            public string name;
            public int points;
        }

    }

    [ChatCommand("tw")]
    private void command_TownWars(BasePlayer player, string c, string[] a);
}

public class townsData
{
    public string OwnerID;
    public string OwnerName;
    public int lastReward;
    public int lastCapture;
}

public class inventoryData
{
    public string shortName;
    public ulong skinID;
    public int amount;
    public string customName;
    public int itemID;
    public string command;
}

private class PluginConfig
{
    [JsonProperty("Общие настройки")]
    public Settings settings;
    [JsonProperty("Настройки маркеров")]
    public MarkersSettings marker;
    [JsonProperty("Настройки РТ")]
    public Dictionary<string, TownSettings> towns;
    [JsonProperty("Настройки UI")]
    public UiSettings ui;
    [JsonProperty("Версия конфига")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}

public class Settings
{
    [JsonProperty("Тип работы плагина Team - обычные команды(зеленка), Solo - для соло захвата, Clans - поддержка кланов ClanReborn(Chaos), ClansUI(RP), Clans(Umod)")]
    public string typeTeam;
    [JsonProperty("Переодичность выдачи наград в секундах")]
    public int addRewardSettings;
    [JsonProperty("Предметы из инвентаря может забрать только глава клана")]
    public bool allCanTakeReward;
    [JsonProperty("Начать захват может только лидер клана или группы")]
    public bool allCanStartCapt;
    [JsonProperty("Удалять ли инвентарь предметов после вайпа")]
    public bool clearInventoryAfterWipe;
    [JsonProperty("Откат до следующего захвата в секундах")]
    public int delayCapture;
    [JsonProperty("Сколько секунд длится захват")]
    public int durCapture;
    [JsonProperty("Добавлять ли видимые сферы, для обозначения границ захвата")]
    public bool addSphere;
    [JsonProperty("Минимум игроков на сервере для начала захвата (0 - выкл)")]
    public int minPlayers;
}

public class MarkersSettings
{
    [JsonProperty("Радиус маркера")]
    public float markerRadius;
    [JsonProperty("Прозрачность маркера")]
    public float markerAlpha;
    [JsonProperty("Цвет маркера когда РТ можно захватить")]
    public string markerColorCanCapture;
    [JsonProperty("Цвет маркера когда РТ захватывают")]
    public string markerColorCapture;
    [JsonProperty("Цвет маркера когда РТ нельзя захваить")]
    public string markerColorCantCapture;
    [JsonProperty("Добавлять ли название на карту маркер с именем, кто последний захватил РТ")]
    public bool addMarkerWithLast;
}

public class TownSettings
{
    [JsonProperty("Название РТ")]
    public string name;
    [JsonProperty("На каком расстояние от центра РТ начислять очки захвата")]
    public int capDist;
    [JsonProperty("Список наград")]
    public List<Reward> rewards;
}

public class UiSettings
{
    [JsonProperty("Цвет фона")]
    public string colorBG;
    [JsonProperty("Цвет обводки")]
    public string colorLines;
    [JsonProperty("Цвет кнопки 'Начать захват'")]
    public string colorButtonCapture;
    [JsonProperty("Цвет кнопки 'Инвентарь'")]
    public string colorButtonInventory;
}

public class Reward
{
    [JsonProperty("Shortname предмета")]
    public string shortName;
    [JsonProperty("Количество предмета")]
    public int amount;
    [JsonProperty("Скин айди предмета")]
    public ulong skinID;
    [JsonProperty("Имя предмета (если кастом)")]
    public string customName;
    [JsonProperty("Ссылка на картинку (если кастом)")]
    public string imageUrl;
    [JsonProperty("Команда для выполнения %STEAMID% (для загрузки картинки придумайте любой номер SkinID и ShortName)")]
    public string command;
}

public class UI
{
    public static void CreateOutLines(CuiElementContainer container, string parent, string color);
    public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, string name, float FadeIn);
    public static void CreatePanel(CuiElementContainer container, string name, string parent, string color, string aMin, string aMax, float Fadeout, float Fadein);
    public static void CreatePanelBlur(CuiElementContainer container, string name, string parent, string color, string aMin, string aMax, float Fadeout, float Fadein);
    public static void CreateText(CuiElementContainer container, string parent, string text, string color, string aMin, string aMax, TextAnchor align, int size, string name, float Fadein);
    public static void CreateTextOutLine(CuiElementContainer container, string parent, string text, string color, string aMin, string aMax, TextAnchor align, int size, string name, float Fadein);
}

public class Capture : MonoBehaviour
{
    public string monumentName;
    public string Name;
    public float distance;
    public string OwnerId;
    public string OwnerName;
    public int lastReward;
    public int lastCapture;
    public bool isCapture;
    public int id;
    private int timerSec;
    private List<BasePlayer> players;
    private Dictionary<string, teamInfo> capTeams;
    private SphereCollider sphereCollider;
    private BaseEntity[] spheres;
    private MapMarkerGenericRadius mapMarker;
    private VendingMachineMapMarker vendingMarker;
    public void DestroyComp();
    private void OnDestroy();
    private void Awake();
    public void Initialize(string monName, string townName, int capDist, string oId, string oName, int reward, int capt);
    private void OnTriggerEnter(Collider other);
    private void OnTriggerExit(Collider other);
    private void Timer();
    private void DrawInfo(BasePlayer player);
    public void StartCapture(BasePlayer player);
    private void StopCapture();
    private void UpdateMarker();
    private void RemoveMarker();
    private void CreateMarker(string color);
    private void CreateCaptureSphere();
    private void RemoveCaptureSphere();
    private void SetPlayerFlag(BasePlayer player, BasePlayer.PlayerFlags f, bool b);
    private Color ConvertToColor(string color);
    public bool isPlayerInZone(BasePlayer player);
    public bool removePlayerFromZone(BasePlayer player);
    public string GetStatus();
    public class teamInfo
    {
        public string id;
        public string name;
        public int points;
    }

}

public class teamInfo
{
    public string id;
    public string name;
    public int points;
}


```

---

## TPApi

```csharp
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using UnityEngine;

Oxide.Plugins
[Info("TPApi", "Sempai#3239", "2.0.0")]
 class TPApi : RustPlugin
{
    [ChatCommand("gtatest")]
    private void Test(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("tpapi.showforall")]
    private void ShowGameTipForAll_ConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("tpapi.showforplayer")]
    private void ShowGameTipForPlayer_ConsoleCommand(ConsoleSystem.Arg arg);
    private void ShowGameTip(BasePlayer target, string message, int type);
    private void ShowGameTip(List<BasePlayer> targets, string message, int type);
    private string FormatString(string text);
    [HookMethod("ShowGameTipForPlayer")]
    public void ShowGameTipForPlayer_Hook(BasePlayer player, int type, string message);
    [HookMethod("ShowGameTipForAll")]
    public void ShowGameTipForPlayer_Hook(int type, string message);
}


```

---

## TPBaraxolka

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics.CodeAnalysis;
using System.Globalization;
using System.Linq;
using UnityEngine;
using UnityEngine.Networking;
using Network;

Oxide.Plugins
[Info("TPBaraxolka", "Sempai#3239", "1.0.3")]
internal class TPBaraxolka : RustPlugin
{
    public List<BaseEntity> BaseEntityList;
    [PluginReference]
     Plugin ImageLibrary;
     Plugin TPEconomic;
     Plugin TPMenuSystem;
     void Loaded();
     void OnServerInitialized();
    public string External;
    public string Internal;
    public string Layer95;
    private void DrawNPCUI(BasePlayer player, string img, int a);
     void CreateInfoPlayer(BasePlayer player, bool opened);
     string GetMoodTranslate(FatherComponent.Mood mood);
     string GetMoodInfoRmation(FatherComponent.Mood mood);
     string InitialLayer;
    private void InitializeLayers(BasePlayer player, string SelectMenu);
     void CreateInfoJson(BasePlayer player, int page);
     void CreateInfo(BasePlayer player);
    [ConsoleCommand("playerVideoBatia")]
     void paltdsag(ConsoleSystem.Arg arg);
     void CreateBottleExchange(BasePlayer player, int page);
     ItemContainer.CanAcceptResult? CanAcceptItem(ItemContainer container, Item item, int pos);
    [ChatCommand("baraxolka")]
     void asf(BasePlayer player);
    public void DrawMenuPoints(BasePlayer player, MenuPoints choosed);
     void updateBalance(BasePlayer player);
    [ConsoleCommand("UI_TPBaraxolka")]
     void cmdMenuTPBaraxolka(ConsoleSystem.Arg args);
    public ulong ContainerID;
     object CanLootEntity(BasePlayer player, BaseEntity container);
     void FindPositions();
    private void FixSignage(Signage sign);
    private static byte[] ImageBytes;
    private IEnumerator DownloadImage(string url);
    public static TPBaraxolka ins;
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    public class MenuPoints
    {
        [JsonProperty("Название пункта меню в UI")]
        public string DisplayName;
        [JsonProperty("Выполняемая команда")]
        public string DrawMethod;
        [JsonProperty("Титл страницы")]
        public string Title;
        [JsonProperty("Диалог при нажатии (Пустое ничего не будет)")]
        public string Sound;
    }

     class PluginConfig
    {
        [JsonProperty("Видео обзор")]
        public string urlvido;
        [JsonProperty("Настройка продажи предметов")]
        public BottleSetting bottleSetting;
        [JsonProperty("Настройка покупки")]
        public ExpeditionSettings expeditionSettings;
        [JsonProperty("Configuration Version")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

    public class CustomShopItems
    {
        [JsonProperty("Название предмета в UI")]
        public string Title;
        [JsonProperty("Выполняемая команда (Если это предмет оставь ПУСТЫМ! %STEAMID% - индификатор игрока)")]
        public List<string> Command;
        [JsonProperty("Кастомное изображение предмета (Если у игровой предмет можно не указывать)")]
        public string ImageURL;
        [JsonProperty("Нужное количество золота на покупку данного предмета")]
        public int NeedGold;
        [JsonProperty("Настройка предмета (Если не привилегия трогать не нужно)")]
        public DefaultItem defaultItem;
    }

    public class DefaultItem
    {
        [JsonProperty("Shortname предмета")]
        public string ShortName;
        [JsonProperty("Количество")]
        public int MinAmount;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Имя предмета при создании (Оставте поле постым чтобы использовать стандартное название итема)")]
        public string Name;
        [JsonProperty("Это чертеж")]
        public bool IsBlueprnt;
    }

    internal class ItemsAddSetting
    {
        [JsonProperty("Количество очков за предмет")]
        public float amount;
        [JsonProperty("Максимальное количество предмета")]
        public int maxCount;
    }

    internal class ArtefactsItems
    {
        [JsonProperty("ShortName предмета")]
        public string ShortName;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Ссылка на изображение")]
        public string ImageURL;
    }

    public class BottleSetting
    {
        [JsonProperty("Список предметов на обмен")]
        public List<CustomShopItems> CustomItemsShop;
    }

    public class ExpeditionSettings
    {
        [JsonProperty("Список предметов, которые игрок может положить")]
        public Dictionary<string, ItemsAddSetting> ItemsAdded;
    }

    [ChatCommand("say")]
     void cmdBotSay(BasePlayer player, string com, string[] args);
    private void GiveItem(BasePlayer player, Item item, BaseEntity.GiveItemReason reason);
    private bool GiveItem(PlayerInventory inv, Item item, ItemContainer container);
    private static bool MoveToContainer(Item itemBase, ItemContainer newcontainer, int iTargetPos, bool allowStack);
    private void GetIdealPickupContainer(PlayerInventory inv, Item item, ItemContainer container, int position);
    public BasePlayer newPlayer;
     FatherComponent com;
    [PluginReference]
     Plugin IQKits;
     Plugin RustStore;
    private void SpawnNPC(Vector3 positon);
     void Unload();
     string GetImageInMood();
     void RegisteredDataUser(UInt64 player);
     void OnPlayerConnected(BasePlayer player);
    public List<ulong> ArtefactsList;
    private void OnServerShutdown();
    private void Init();
    [JsonProperty("Список обмена")]
    public Dictionary<ulong, Dictionary<string, int>> DataObmen;
     void ReadData();
     void WriteData();
     void sendmoney(BasePlayer player, float money);
     class FatherComponent : FacepunchBehaviour
    {
        public BasePlayer player;
         SphereCollider sphereCollider;
        public BasePlayer target;
        public Dictionary<ulong, ExpeditionPlayer> CurrentExpeditions;
        public Dictionary<ulong, float> EndExpeditions;
        public List<BasePlayer> OpenInterface;
        public void SaveData();
        public void LoadData();
        public Mood mood;
        public class ExpeditionPlayer
        {
            public double EndTime;
            public float Points;
            public bool Ending;
        }

         void SendMessages(ulong Player);
         void ExpeditionHandler();
         void CreateInfoPlayer(BasePlayer player, double time);
        public static string FormatShortTime(TimeSpan time);
         void Awake();
         void ChangeInMood();
        private void OnTriggerEnter(Collider other);
        private void OnTriggerExit(Collider other);
         void Puts(string messages);
        public void Destroy();
    }

    public Dictionary<string, SoundData> Sounds;
    public class SoundData
    {
        public List<List<byte[]>> Sounds;
        public List<byte[]> RandomSound();
    }

     Dictionary<ulong, bool> status;
    public List<byte[]> timed;
    private void GetSoundToPlayer(BasePlayer player, ulong netid, string name);
    public void SendToPlayer(BasePlayer player, ulong netid, byte[] data);
     void LoadSounds();
    private void SaveSoundData();
     object OnPlayerVoice(BasePlayer player, byte[] data);
    [ChatCommand("sound")]
     void startcmd(BasePlayer player, string command, string[] args);
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
    private static string _(string i);
    public class ExpeditionExceptionBox : MonoBehaviour
    {
        public StorageContainer storage;
        public BasePlayer player;
        public string UIPanel;
        public void Init(StorageContainer storage, BasePlayer owner);
        public static ExpeditionExceptionBox Spawn(BasePlayer player);
        public static StorageContainer SpawnContainer(BasePlayer player);
        private void PlayerStoppedLooting(BasePlayer player);
        public void Close();
        public void StartLoot();
         bool disabledButton;
         void CreateUI();
        public void ReturnPlayerItems();
         void UpdatePanels();
        public float PoitsInvectoryCount();
         void UpdateInfo();
        public void SendItems();
        public List<Item> GetItems { get; set; }
         void OnDestroy();
    }

     object CanMoveItem(Item item, PlayerInventory playerLoot, ItemContainerId targetContainer, int targetSlot, int amount);
     object OnEntityTakeDamage(BasePlayer entity, HitInfo info);
}

public class MenuPoints
{
    [JsonProperty("Название пункта меню в UI")]
    public string DisplayName;
    [JsonProperty("Выполняемая команда")]
    public string DrawMethod;
    [JsonProperty("Титл страницы")]
    public string Title;
    [JsonProperty("Диалог при нажатии (Пустое ничего не будет)")]
    public string Sound;
}

 class PluginConfig
{
    [JsonProperty("Видео обзор")]
    public string urlvido;
    [JsonProperty("Настройка продажи предметов")]
    public BottleSetting bottleSetting;
    [JsonProperty("Настройка покупки")]
    public ExpeditionSettings expeditionSettings;
    [JsonProperty("Configuration Version")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}

public class CustomShopItems
{
    [JsonProperty("Название предмета в UI")]
    public string Title;
    [JsonProperty("Выполняемая команда (Если это предмет оставь ПУСТЫМ! %STEAMID% - индификатор игрока)")]
    public List<string> Command;
    [JsonProperty("Кастомное изображение предмета (Если у игровой предмет можно не указывать)")]
    public string ImageURL;
    [JsonProperty("Нужное количество золота на покупку данного предмета")]
    public int NeedGold;
    [JsonProperty("Настройка предмета (Если не привилегия трогать не нужно)")]
    public DefaultItem defaultItem;
}

public class DefaultItem
{
    [JsonProperty("Shortname предмета")]
    public string ShortName;
    [JsonProperty("Количество")]
    public int MinAmount;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Имя предмета при создании (Оставте поле постым чтобы использовать стандартное название итема)")]
    public string Name;
    [JsonProperty("Это чертеж")]
    public bool IsBlueprnt;
}

internal class ItemsAddSetting
{
    [JsonProperty("Количество очков за предмет")]
    public float amount;
    [JsonProperty("Максимальное количество предмета")]
    public int maxCount;
}

internal class ArtefactsItems
{
    [JsonProperty("ShortName предмета")]
    public string ShortName;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Ссылка на изображение")]
    public string ImageURL;
}

public class BottleSetting
{
    [JsonProperty("Список предметов на обмен")]
    public List<CustomShopItems> CustomItemsShop;
}

public class ExpeditionSettings
{
    [JsonProperty("Список предметов, которые игрок может положить")]
    public Dictionary<string, ItemsAddSetting> ItemsAdded;
}

 class FatherComponent : FacepunchBehaviour
{
    public BasePlayer player;
     SphereCollider sphereCollider;
    public BasePlayer target;
    public Dictionary<ulong, ExpeditionPlayer> CurrentExpeditions;
    public Dictionary<ulong, float> EndExpeditions;
    public List<BasePlayer> OpenInterface;
    public void SaveData();
    public void LoadData();
    public Mood mood;
    public class ExpeditionPlayer
    {
        public double EndTime;
        public float Points;
        public bool Ending;
    }

     void SendMessages(ulong Player);
     void ExpeditionHandler();
     void CreateInfoPlayer(BasePlayer player, double time);
    public static string FormatShortTime(TimeSpan time);
     void Awake();
     void ChangeInMood();
    private void OnTriggerEnter(Collider other);
    private void OnTriggerExit(Collider other);
     void Puts(string messages);
    public void Destroy();
}

public class ExpeditionPlayer
{
    public double EndTime;
    public float Points;
    public bool Ending;
}

public class SoundData
{
    public List<List<byte[]>> Sounds;
    public List<byte[]> RandomSound();
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

public class ExpeditionExceptionBox : MonoBehaviour
{
    public StorageContainer storage;
    public BasePlayer player;
    public string UIPanel;
    public void Init(StorageContainer storage, BasePlayer owner);
    public static ExpeditionExceptionBox Spawn(BasePlayer player);
    public static StorageContainer SpawnContainer(BasePlayer player);
    private void PlayerStoppedLooting(BasePlayer player);
    public void Close();
    public void StartLoot();
     bool disabledButton;
     void CreateUI();
    public void ReturnPlayerItems();
     void UpdatePanels();
    public float PoitsInvectoryCount();
     void UpdateInfo();
    public void SendItems();
    public List<Item> GetItems { get; set; }
     void OnDestroy();
}


```

---

## TPBattlePass

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Oxide.Plugins.TPExtensionMethods;

Oxide.Plugins
[Info("TPBattle Pass", "Sempai#3239", "2.0.2")]
internal class TPBattlePass : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin TPApi;
    private Configuration _config;
    private const string Layer;
    private const string perm;
    private const string perm1;
    private class Configuration
    {
        [JsonProperty("Настройка очков")]
        public readonly PointSettings Point;
        [JsonProperty("Настройка уровней")]
        public readonly LevelSettings LevelDefault;
        [JsonProperty("Настройка для донатеров")]
        public readonly LevelSettings LevelDonate;
        internal class LevelSettings
        {
            [JsonProperty(PropertyName = "Список уровней", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public readonly List<Settings> LevelList;
            internal class Settings
            {
                [JsonProperty("Level Number")]
                public int Level;
                [JsonProperty("Number Of Exp To Get This Level")]
                public int Exp;
                [JsonProperty("DisplayName")]
                public string DisplayName;
                [JsonProperty("Award Display Image")]
                public string Image;
                [JsonProperty("Level Award")]
                public RewardSettings Reward;
                internal class RewardSettings
                {
                    [JsonProperty("ShortName")]
                    public string ShortName;
                    [JsonProperty("Amount")]
                    public int Amount;
                    [JsonProperty("SkinID")]
                    public ulong SkinID;
                    [JsonProperty("Commands To Be Executed")]
                    public List<string> command;
                }

            }

        }

        internal class PointSettings
        {
            [JsonProperty("Donator Point Multiplier")]
            public readonly int DonateAmount;
            [JsonProperty("Количество Очков За Убийство Игрока")]
            public readonly int killPlayer;
            [JsonProperty("Количество Очков За Убийство Животных")]
            public readonly int killHuman;
            [JsonProperty("Количество Очков За Уничтожение вертолета")]
            public readonly int killHeli;
            [JsonProperty("Количество Очков за Убийство NPC")]
            public readonly int killNPC;
            [JsonProperty("Количество Очков за Уничтожение Танка")]
            public readonly int killBredly;
            [JsonProperty("Количество Очков, Вычитаемых За Смерть")]
            public readonly int deathPlayer;
            [JsonProperty("Настройки фарма")]
            public readonly GatherSettings Gather;
        }

        internal class GatherSettings
        {
            [JsonProperty(PropertyName = "Настройка фарма(Краткое название/Количество Очков)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public readonly Dictionary<string, int> GatherList;
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
    private void Unload();
    private object OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private readonly Dictionary<NetworkableId, ulong> LastHeliHit;
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(PatrolHelicopter entity, HitInfo info);
    private void OnEntityDeath(BaseEntity entity, HitInfo info);
    private Dictionary<ulong, Data> _data;
    private class Data
    {
        public int Level;
        public int Score;
        public readonly List<int> DefaultRewardID;
        public readonly List<int> DonateRewardID;
    }

    private void LoadData();
    private void OnServerSave();
    private void SaveData();
    [ConsoleCommand("UI_BATTLEPASS_GETREWARDDEFULT")]
    private void cmdChatUI_BATTLEPASS_GETREWARD(ConsoleSystem.Arg arg);
    [ChatCommand("pass")]
    private void cmdChatpass(BasePlayer player, string command, string[] args);
    [ChatCommand("lvl")]
    private void cmdChatpa22ss(BasePlayer player, string command, string[] args);
    [ChatCommand("level")]
    private void cmdChatpas2s(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_BATTLEPASS_GETREWARDDONATE")]
    private void cmdChatUI_BATTLEPASS_DDONATE(ConsoleSystem.Arg arg);
    [ConsoleCommand("givepoint")]
    private void cmdChatgivepoint(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BATTLEPASS_CHANGEPAGE")]
    private void cmdChatUI_BATTLEPASS_CHANGEPAGE(ConsoleSystem.Arg arg);
    private void GivePoint(ulong userid, int amount);
     void ShowUIMain(BasePlayer player, int page);
}

private class Configuration
{
    [JsonProperty("Настройка очков")]
    public readonly PointSettings Point;
    [JsonProperty("Настройка уровней")]
    public readonly LevelSettings LevelDefault;
    [JsonProperty("Настройка для донатеров")]
    public readonly LevelSettings LevelDonate;
    internal class LevelSettings
    {
        [JsonProperty(PropertyName = "Список уровней", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly List<Settings> LevelList;
        internal class Settings
        {
            [JsonProperty("Level Number")]
            public int Level;
            [JsonProperty("Number Of Exp To Get This Level")]
            public int Exp;
            [JsonProperty("DisplayName")]
            public string DisplayName;
            [JsonProperty("Award Display Image")]
            public string Image;
            [JsonProperty("Level Award")]
            public RewardSettings Reward;
            internal class RewardSettings
            {
                [JsonProperty("ShortName")]
                public string ShortName;
                [JsonProperty("Amount")]
                public int Amount;
                [JsonProperty("SkinID")]
                public ulong SkinID;
                [JsonProperty("Commands To Be Executed")]
                public List<string> command;
            }

        }

    }

    internal class PointSettings
    {
        [JsonProperty("Donator Point Multiplier")]
        public readonly int DonateAmount;
        [JsonProperty("Количество Очков За Убийство Игрока")]
        public readonly int killPlayer;
        [JsonProperty("Количество Очков За Убийство Животных")]
        public readonly int killHuman;
        [JsonProperty("Количество Очков За Уничтожение вертолета")]
        public readonly int killHeli;
        [JsonProperty("Количество Очков за Убийство NPC")]
        public readonly int killNPC;
        [JsonProperty("Количество Очков за Уничтожение Танка")]
        public readonly int killBredly;
        [JsonProperty("Количество Очков, Вычитаемых За Смерть")]
        public readonly int deathPlayer;
        [JsonProperty("Настройки фарма")]
        public readonly GatherSettings Gather;
    }

    internal class GatherSettings
    {
        [JsonProperty(PropertyName = "Настройка фарма(Краткое название/Количество Очков)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly Dictionary<string, int> GatherList;
    }

}

internal class LevelSettings
{
    [JsonProperty(PropertyName = "Список уровней", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly List<Settings> LevelList;
    internal class Settings
    {
        [JsonProperty("Level Number")]
        public int Level;
        [JsonProperty("Number Of Exp To Get This Level")]
        public int Exp;
        [JsonProperty("DisplayName")]
        public string DisplayName;
        [JsonProperty("Award Display Image")]
        public string Image;
        [JsonProperty("Level Award")]
        public RewardSettings Reward;
        internal class RewardSettings
        {
            [JsonProperty("ShortName")]
            public string ShortName;
            [JsonProperty("Amount")]
            public int Amount;
            [JsonProperty("SkinID")]
            public ulong SkinID;
            [JsonProperty("Commands To Be Executed")]
            public List<string> command;
        }

    }

}

internal class Settings
{
    [JsonProperty("Level Number")]
    public int Level;
    [JsonProperty("Number Of Exp To Get This Level")]
    public int Exp;
    [JsonProperty("DisplayName")]
    public string DisplayName;
    [JsonProperty("Award Display Image")]
    public string Image;
    [JsonProperty("Level Award")]
    public RewardSettings Reward;
    internal class RewardSettings
    {
        [JsonProperty("ShortName")]
        public string ShortName;
        [JsonProperty("Amount")]
        public int Amount;
        [JsonProperty("SkinID")]
        public ulong SkinID;
        [JsonProperty("Commands To Be Executed")]
        public List<string> command;
    }

}

internal class RewardSettings
{
    [JsonProperty("ShortName")]
    public string ShortName;
    [JsonProperty("Amount")]
    public int Amount;
    [JsonProperty("SkinID")]
    public ulong SkinID;
    [JsonProperty("Commands To Be Executed")]
    public List<string> command;
}

internal class PointSettings
{
    [JsonProperty("Donator Point Multiplier")]
    public readonly int DonateAmount;
    [JsonProperty("Количество Очков За Убийство Игрока")]
    public readonly int killPlayer;
    [JsonProperty("Количество Очков За Убийство Животных")]
    public readonly int killHuman;
    [JsonProperty("Количество Очков За Уничтожение вертолета")]
    public readonly int killHeli;
    [JsonProperty("Количество Очков за Убийство NPC")]
    public readonly int killNPC;
    [JsonProperty("Количество Очков за Уничтожение Танка")]
    public readonly int killBredly;
    [JsonProperty("Количество Очков, Вычитаемых За Смерть")]
    public readonly int deathPlayer;
    [JsonProperty("Настройки фарма")]
    public readonly GatherSettings Gather;
}

internal class GatherSettings
{
    [JsonProperty(PropertyName = "Настройка фарма(Краткое название/Количество Очков)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly Dictionary<string, int> GatherList;
}

private class Data
{
    public int Level;
    public int Score;
    public readonly List<int> DefaultRewardID;
    public readonly List<int> DonateRewardID;
}

Oxide.Plugins.TPExtensionMethods
public static class ExtensionMethods
{
    public static TSource TPLast(IList<TSource> source);
}


```

---

## TPBotbonus

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("TPBotbonus", "Sempai#3239", "4.0.1")]
 class TPBotbonus : RustPlugin
{
    [PluginReference]
     Plugin Duel;
     Plugin ImageLibrary;
    static string apiver;
    private bool OxideUpdateSended;
    private System.Random random;
    private bool NewWipe;
     JsonSerializerSettings jsonsettings;
    private List<string> allowedentity;
     List<string> ExplosiveList;
    private List<ulong> BDayPlayers;
     class GiftItem
    {
        public string shortname;
        public ulong skinid;
        public int count;
    }

     class ServerInfo
    {
        public string name;
        public string online;
        public string slots;
        public string sleepers;
        public string map;
    }

    private Dictionary<BasePlayer, DateTime> GiftsList;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Ключи VK API, ID группы")]
        public VKAPITokens VKAPIT;
        [JsonProperty(PropertyName = "Награда за вступление в группу")]
        public GroupGifts GrGifts;
        [JsonProperty(PropertyName = "Оповещения при вайпе")]
        public WipeSettings WipeStg;
        public class VKAPITokens
        {
            [JsonProperty(PropertyName = "VK Token группы (для сообщений)")]
            public string VKToken;
            [JsonProperty(PropertyName = "VK Token приложения (для записей на стене и статуса)")]
            public string VKTokenApp;
            [JsonProperty(PropertyName = "VKID группы")]
            public string GroupID;
        }

        public class GroupGifts
        {
            [JsonProperty(PropertyName = "Выдавать подарок игроку за вступление в группу ВК?")]
            public bool VKGroupGifts;
            [JsonProperty(PropertyName = "Подарок за вступление в группу (команда, если стоит none выдаются предметы из файла data/TPBotbonus.json). Пример: grantperm {steamid} vkraidalert.allow 7d")]
            public string VKGroupGiftCMD;
            [JsonProperty(PropertyName = "Описание команды")]
            public string GiftCMDdesc;
            [JsonProperty(PropertyName = "Ссылка на группу ВК")]
            public string VKGroupUrl;
            [JsonProperty(PropertyName = "Оповещения в общий чат о получении награды")]
            public bool GiftsBool;
            [JsonProperty(PropertyName = "Включить оповещения для игроков не получивших награду за вступление в группу?")]
            public bool VKGGNotify;
            [JsonProperty(PropertyName = "Интервал оповещений для игроков не получивших награду за вступление в группу (в минутах)")]
            public int VKGGTimer;
            [JsonProperty(PropertyName = "Выдавать награду каждый вайп?")]
            public bool GiftsWipe;
        }

        public class WipeSettings
        {
            [JsonProperty(PropertyName = "Отправлять пост в группу после вайпа?")]
            public bool WPostB;
            [JsonProperty(PropertyName = "Текст поста о вайпе")]
            public string WPostMsg;
            [JsonProperty(PropertyName = "Добавить изображение к посту о вайпе?")]
            public bool WPostAttB;
            [JsonProperty(PropertyName = "Отправлять сообщение администратору о вайпе?")]
            public bool WPostMsgAdmin;
            [JsonProperty(PropertyName = "Ссылка на изображение к посту о вайпе вида 'photo-1_265827614' (изображение должно быть в альбоме группы)")]
            public string WPostAtt;
            [JsonProperty(PropertyName = "Отправлять игрокам сообщение о вайпе автоматически?")]
            public bool WMsgPlayers;
            [JsonProperty(PropertyName = "Текст сообщения игрокам о вайпе (сообщение отправляется только тем кто подтвердил профиль)")]
            public string WMsgText;
            [JsonProperty(PropertyName = "Игнорировать тех кто подтвердил профиль? (если включено, сообщение о вайпе будет отправляться всем)")]
            public bool WCMDIgnore;
            [JsonProperty(PropertyName = "Смена названия группы после вайпа")]
            public bool GrNameChange;
            [JsonProperty(PropertyName = "Название группы (переменная {wipedate} отображает дату последнего вайпа)")]
            public string GrName;
        }

    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
     class DataStorageStats
    {
        public int WoodGath;
        public int SulfureGath;
        public int Rockets;
        public int Blueprints;
        public int Explosive;
        public int Reports;
        public List<GiftItem> Gifts;
        public DataStorageStats();
    }

     class DataStorageUsers
    {
        public Dictionary<ulong, VKUDATA> VKUsersData;
        public DataStorageUsers();
    }

     class VKUDATA
    {
        public ulong UserID;
        public string Name;
        public string VkID;
        public string VkOwnerID;
        public int ConfirmCode;
        public bool Confirmed;
        public bool GiftRecived;
        public string LastRaidNotice;
        public bool WipeMsg;
        public string Bdate;
        public int Raids;
        public int Kills;
        public int Farm;
        public string LastSeen;
    }

     class DataStorageReports
    {
        public Dictionary<int, REPORT> VKReportsData;
        public DataStorageReports();
    }

     class REPORT
    {
        public ulong UserID;
        public string Name;
        public string Text;
    }

     DataStorageStats statdata;
     DataStorageUsers usersdata;
     DataStorageReports reportsdata;
    private DynamicConfigFile VKBData;
    private DynamicConfigFile StatData;
    private DynamicConfigFile ReportsData;
     void LoadData();
    private void OnServerInitialized();
    private void Init();
    private void Loaded();
    private void Unload();
    private void OnNewSave(string filename);
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void UpdateUsersData(ConsoleSystem.Arg arg);
    private void CheckVkUser(BasePlayer player, string url);
    private void AddVKUser(BasePlayer player, string Userid, string bdate);
    private void VKcommand(BasePlayer player, string cmd, string[] args);
    private void WAlert(BasePlayer player);
    private void VKGift(BasePlayer player);
    private void GetGift(int code, string Result, BasePlayer player);
    private void GiftNotifier();
     void WipeFunctions();
    private void WipeAlertsSend();
    private void SendConfCode(string reciverID, string msg, BasePlayer player);
    private void SendVkMessage(string reciverID, string msg);
    private void SendVkWall(string msg);
    private void SendVkStatus(string msg);
    private void AddComentToBoard(string topicid, string msg);
    private string RandomId();
     string GetUserVKId(ulong userid);
     string GetUserLastNotice(ulong userid);
    private void VKAPISaveLastNotice(ulong userid, string lasttime);
    private void VKAPIWall(string text, string attachments, bool atimg);
    private void VKAPIMsg(string text, string attachments, string reciverID, bool atimg);
    private void VKAPIStatus(string msg);
     void Log(string filename, string text);
     void GetCallback(int code, string response, string type, BasePlayer player);
    private string EmojiCounters(string counter);
    private string WipeDate();
    private string URLEncode(string input);
    private void StatusCheck(string msg);
    private bool IsNPC(BasePlayer player);
    private static string GetColor(string hex);
    private void DeleteOldUsers(string days);
    private void CheckOxideUpdate();
    private string BannedUsers;
    private CuiElement BPanel(string name, string color, string anMin, string anMax, string parent, bool cursor, float fade);
    private CuiElement Panel(string name, string color, string anMin, string anMax, string parent, bool cursor, float fade);
    private CuiElement Text(string parent, string color, string text, TextAnchor pos, int fsize, string anMin, string anMax, string fname, float fade);
    private CuiElement Button(string name, string parent, string command, string color, string anMin, string anMax, float fade);
    private CuiElement Image(string parent, string url, string anMin, string anMax, float fade, string color);
    private CuiElement Input(string name, string parent, int fsize, string command, string anMin, string anMax, TextAnchor pos, int chlimit, bool psvd, float fade);
    private void UnloadAllGUI();
    private string UserName(string name);
    private void StartTPBotbonusAddVKGUI(BasePlayer player);
    public string GetImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public bool HasImage(string imageName);
    public void SendImage(BasePlayer player, string imageName, ulong imageId);
     void OnPlayerConnected(BasePlayer player);
     void SteamAvatarAdd(string userid);
    public string VkICO;
    public string GiftICO;
    public string AlertICO;
    public string VkConnect;
    public string VkHelp;
    public string VkWait;
    public string MainLayer;
    public string VkReward;
    public string VkAlert;
    public string AlertPermission;
    [ChatCommand("vk")]
     void StartTPBotbonusMainGUI(BasePlayer player, ulong target);
    private void StartTPBotbonusHelpVKGUI(BasePlayer player);
    private void StartCodeSendedGUI(BasePlayer player);
    private void RewardTPBotbonusGUI(BasePlayer player);
    private void CompleteRewardTPBotbonusGUI(BasePlayer player);
    private void AlertTPBotbonusGUI(BasePlayer player);
    [ConsoleCommand("vk.menugui46570981")]
    private void CmdChoose(ConsoleSystem.Arg arg);
    [ConsoleCommand("vk.refresh")]
    private void CmdRefresh(ConsoleSystem.Arg arg);
    private void FixedGifts(BasePlayer player);
    private void LoadMessages();
     string GetMsg(string key);
}

 class GiftItem
{
    public string shortname;
    public ulong skinid;
    public int count;
}

 class ServerInfo
{
    public string name;
    public string online;
    public string slots;
    public string sleepers;
    public string map;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Ключи VK API, ID группы")]
    public VKAPITokens VKAPIT;
    [JsonProperty(PropertyName = "Награда за вступление в группу")]
    public GroupGifts GrGifts;
    [JsonProperty(PropertyName = "Оповещения при вайпе")]
    public WipeSettings WipeStg;
    public class VKAPITokens
    {
        [JsonProperty(PropertyName = "VK Token группы (для сообщений)")]
        public string VKToken;
        [JsonProperty(PropertyName = "VK Token приложения (для записей на стене и статуса)")]
        public string VKTokenApp;
        [JsonProperty(PropertyName = "VKID группы")]
        public string GroupID;
    }

    public class GroupGifts
    {
        [JsonProperty(PropertyName = "Выдавать подарок игроку за вступление в группу ВК?")]
        public bool VKGroupGifts;
        [JsonProperty(PropertyName = "Подарок за вступление в группу (команда, если стоит none выдаются предметы из файла data/TPBotbonus.json). Пример: grantperm {steamid} vkraidalert.allow 7d")]
        public string VKGroupGiftCMD;
        [JsonProperty(PropertyName = "Описание команды")]
        public string GiftCMDdesc;
        [JsonProperty(PropertyName = "Ссылка на группу ВК")]
        public string VKGroupUrl;
        [JsonProperty(PropertyName = "Оповещения в общий чат о получении награды")]
        public bool GiftsBool;
        [JsonProperty(PropertyName = "Включить оповещения для игроков не получивших награду за вступление в группу?")]
        public bool VKGGNotify;
        [JsonProperty(PropertyName = "Интервал оповещений для игроков не получивших награду за вступление в группу (в минутах)")]
        public int VKGGTimer;
        [JsonProperty(PropertyName = "Выдавать награду каждый вайп?")]
        public bool GiftsWipe;
    }

    public class WipeSettings
    {
        [JsonProperty(PropertyName = "Отправлять пост в группу после вайпа?")]
        public bool WPostB;
        [JsonProperty(PropertyName = "Текст поста о вайпе")]
        public string WPostMsg;
        [JsonProperty(PropertyName = "Добавить изображение к посту о вайпе?")]
        public bool WPostAttB;
        [JsonProperty(PropertyName = "Отправлять сообщение администратору о вайпе?")]
        public bool WPostMsgAdmin;
        [JsonProperty(PropertyName = "Ссылка на изображение к посту о вайпе вида 'photo-1_265827614' (изображение должно быть в альбоме группы)")]
        public string WPostAtt;
        [JsonProperty(PropertyName = "Отправлять игрокам сообщение о вайпе автоматически?")]
        public bool WMsgPlayers;
        [JsonProperty(PropertyName = "Текст сообщения игрокам о вайпе (сообщение отправляется только тем кто подтвердил профиль)")]
        public string WMsgText;
        [JsonProperty(PropertyName = "Игнорировать тех кто подтвердил профиль? (если включено, сообщение о вайпе будет отправляться всем)")]
        public bool WCMDIgnore;
        [JsonProperty(PropertyName = "Смена названия группы после вайпа")]
        public bool GrNameChange;
        [JsonProperty(PropertyName = "Название группы (переменная {wipedate} отображает дату последнего вайпа)")]
        public string GrName;
    }

}

public class VKAPITokens
{
    [JsonProperty(PropertyName = "VK Token группы (для сообщений)")]
    public string VKToken;
    [JsonProperty(PropertyName = "VK Token приложения (для записей на стене и статуса)")]
    public string VKTokenApp;
    [JsonProperty(PropertyName = "VKID группы")]
    public string GroupID;
}

public class GroupGifts
{
    [JsonProperty(PropertyName = "Выдавать подарок игроку за вступление в группу ВК?")]
    public bool VKGroupGifts;
    [JsonProperty(PropertyName = "Подарок за вступление в группу (команда, если стоит none выдаются предметы из файла data/TPBotbonus.json). Пример: grantperm {steamid} vkraidalert.allow 7d")]
    public string VKGroupGiftCMD;
    [JsonProperty(PropertyName = "Описание команды")]
    public string GiftCMDdesc;
    [JsonProperty(PropertyName = "Ссылка на группу ВК")]
    public string VKGroupUrl;
    [JsonProperty(PropertyName = "Оповещения в общий чат о получении награды")]
    public bool GiftsBool;
    [JsonProperty(PropertyName = "Включить оповещения для игроков не получивших награду за вступление в группу?")]
    public bool VKGGNotify;
    [JsonProperty(PropertyName = "Интервал оповещений для игроков не получивших награду за вступление в группу (в минутах)")]
    public int VKGGTimer;
    [JsonProperty(PropertyName = "Выдавать награду каждый вайп?")]
    public bool GiftsWipe;
}

public class WipeSettings
{
    [JsonProperty(PropertyName = "Отправлять пост в группу после вайпа?")]
    public bool WPostB;
    [JsonProperty(PropertyName = "Текст поста о вайпе")]
    public string WPostMsg;
    [JsonProperty(PropertyName = "Добавить изображение к посту о вайпе?")]
    public bool WPostAttB;
    [JsonProperty(PropertyName = "Отправлять сообщение администратору о вайпе?")]
    public bool WPostMsgAdmin;
    [JsonProperty(PropertyName = "Ссылка на изображение к посту о вайпе вида 'photo-1_265827614' (изображение должно быть в альбоме группы)")]
    public string WPostAtt;
    [JsonProperty(PropertyName = "Отправлять игрокам сообщение о вайпе автоматически?")]
    public bool WMsgPlayers;
    [JsonProperty(PropertyName = "Текст сообщения игрокам о вайпе (сообщение отправляется только тем кто подтвердил профиль)")]
    public string WMsgText;
    [JsonProperty(PropertyName = "Игнорировать тех кто подтвердил профиль? (если включено, сообщение о вайпе будет отправляться всем)")]
    public bool WCMDIgnore;
    [JsonProperty(PropertyName = "Смена названия группы после вайпа")]
    public bool GrNameChange;
    [JsonProperty(PropertyName = "Название группы (переменная {wipedate} отображает дату последнего вайпа)")]
    public string GrName;
}

 class DataStorageStats
{
    public int WoodGath;
    public int SulfureGath;
    public int Rockets;
    public int Blueprints;
    public int Explosive;
    public int Reports;
    public List<GiftItem> Gifts;
    public DataStorageStats();
}

 class DataStorageUsers
{
    public Dictionary<ulong, VKUDATA> VKUsersData;
    public DataStorageUsers();
}

 class VKUDATA
{
    public ulong UserID;
    public string Name;
    public string VkID;
    public string VkOwnerID;
    public int ConfirmCode;
    public bool Confirmed;
    public bool GiftRecived;
    public string LastRaidNotice;
    public bool WipeMsg;
    public string Bdate;
    public int Raids;
    public int Kills;
    public int Farm;
    public string LastSeen;
}

 class DataStorageReports
{
    public Dictionary<int, REPORT> VKReportsData;
    public DataStorageReports();
}

 class REPORT
{
    public ulong UserID;
    public string Name;
    public string Text;
}


```

---

## TPCases

```csharp
using Internal;
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
[Info("TPCases", "Sempai#3239", "5.0.0")]
public class TPCases : RustPlugin
{
    static DateTime epoch;
    static double CurrentTime();
    private string LStorage;
    [JsonProperty("Изображения плагина")]
    private Dictionary<string, string> PluginImages;
    [PluginReference]
    private Plugin ImageLibrary;
    public class ItemsData
    {
        public List<ItemData> CheckItemData;
        public class ItemData
        {
            public string ShortName { get; set; }
            public string Url { get; set; }
            public string Command { get; set; }
            public ulong SkinID { get; set; }
            public int AmountDrop { get; set; }
            public ItemData(string shortname, string Url, string Command, ulong SkinID, int amountdrop);
        }

    }

     void Data();
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void Unload();
     void OnServerSave();
    public Dictionary<ulong, ItemsData> CheckFileDrop;
    public Dictionary<ulong, bool> EnableUI;
    private ConfigData Settings { get; set; }
    public class Itemss
    {
        [JsonProperty("Предмет из игры(shortname)")]
        public string ShortName;
        [JsonProperty("Изображение")]
        public string Url;
        [JsonProperty("Команда")]
        public string Command;
        [JsonProperty("Скин")]
        public ulong SkinID;
        [JsonProperty("мин кол-во предмета")]
        public int MinDrop;
        [JsonProperty("макс кол-во предмета")]
        public int MaxDrop;
    }

     class ConfigData
    {
        [JsonProperty("Включить поддержку плагина скилл")]
        public bool Enable;
        [JsonProperty("Очищать ли склад предметов у игрока после вайпа?")]
        public bool ClearSklad;
        [JsonProperty("Описание плагина кейсов")]
        public string Desk;
        [JsonProperty("Очищать ли кулдавн игрока после вайпа?")]
        public bool ClearTime;
        [JsonProperty("Сколько выдавать очков xp за добычу ресурсов")]
        public double XpBonusGather;
        [JsonProperty("Сколько выдавать очков xp за убийство животных")]
        public double XpKillAnimal;
        [JsonProperty("Сколько выдавать очков xp за убийство игрока")]
        public double XpKillPlayer;
        [JsonProperty("Сколько выдавать очков xp за убийство игрока в голову")]
        public double XpKillPlayerHead;
        [JsonProperty("Сколько выдавать очков xp за сбитие вертолета")]
        public double XpKillHeli;
        [JsonProperty("Сколько выдавать очков xp за разбитие бочек")]
        public double XpKillBarrel;
        [JsonProperty("Сколько выдавать очков xp за уничтожении танка")]
        public double XpKillBradley;
        [JsonProperty("Сколько давать кулдауна после открытия первого кейса?(в секундах)")]
        public double TimeOne;
        [JsonProperty("Сколько давать кулдауна после открытия второго кейса?(в секундах)")]
        public double TimeTo;
        [JsonProperty("Сколько давать кулдауна после открытия третьего кейса?(в секундах)")]
        public double TimeFree;
        [JsonProperty("Сколько давать кулдауна после открытия четвертого кейса?(В секундах)")]
        public double TimeFoo;
        [JsonProperty("Описание для первого кейса!")]
        public string DescOne;
        [JsonProperty("Описание для второй кейса!")]
        public string DescTo;
        [JsonProperty("Описание для третьего кейса!")]
        public string DescFree;
        [JsonProperty("Описание для четвертого кейса!")]
        public string DescFoo;
        [JsonProperty("Настройка предметов для первого кейса")]
        public List<Itemss> ListDrop { get; set; }
        [JsonProperty("Настройка предметов для второго кейса")]
        public List<Itemss> ListDropTo { get; set; }
        [JsonProperty("Настройка предметов для третьего кейса")]
        public List<Itemss> ListDropFree { get; set; }
        [JsonProperty("Настройка предметов для четвертого кейса")]
        public List<Itemss> ListDropFoo { get; set; }
        public static ConfigData GetNewCong();
    }

    protected override void LoadConfig();
     void OnNewSave();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     class DataList
    {
        [JsonProperty("Кулдаун первого кейса")]
        public double onecase { get; set; }
        [JsonProperty("Кулдаун второго кейса!")]
        public double tocase { get; set; }
        [JsonProperty("Кулдаун третьего кейса!")]
        public double freecase { get; set; }
        [JsonProperty("Кулдаун четвертого кейса!")]
        public double foocase { get; set; }
        [JsonProperty("Уровень")]
        public double level { get; set; }
        [JsonProperty("XP игрока")]
        public double xp { get; set; }
    }

    private Dictionary<ulong, DataList> PlayerTimeOut;
    [ConsoleCommand("openDesc")]
    private void cmdOPenDesc(ConsoleSystem.Arg Args);
    [ConsoleCommand("opencase")]
     void giveopencaseitems(ConsoleSystem.Arg args);
    const string Layer;
    [ConsoleCommand("returnCase")]
    private void cmdReturnCase(ConsoleSystem.Arg args);
    [ConsoleCommand("openInventory")]
    private void cmdOpenInventory(ConsoleSystem.Arg args);
    private void DrawGuiStorage(BasePlayer player);
    private void DrawGui(BasePlayer player);
    [ConsoleCommand("desc")]
     void DescUI(ConsoleSystem.Arg args);
    private void DescriptionUi(BasePlayer player, int element);
    [ConsoleCommand("enablecase")]
     void Console_EnableCase(ConsoleSystem.Arg args);
     void InterfaceXP(BasePlayer player);
    [ConsoleCommand("takeitem")]
    private void skladgive(ConsoleSystem.Arg args);
     void ConvertXP(BasePlayer player, double xp);
     void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    public ulong lastDamageName;
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [Oxide.Core.Plugins.HookMethod("OnEntityDeath")]
     void OnDeadEntity(BaseCombatEntity entity, HitInfo info);
     void EntityDeathFunc(BaseCombatEntity entity, HitInfo info);
}

public class ItemsData
{
    public List<ItemData> CheckItemData;
    public class ItemData
    {
        public string ShortName { get; set; }
        public string Url { get; set; }
        public string Command { get; set; }
        public ulong SkinID { get; set; }
        public int AmountDrop { get; set; }
        public ItemData(string shortname, string Url, string Command, ulong SkinID, int amountdrop);
    }

}

public class ItemData
{
    public string ShortName { get; set; }
    public string Url { get; set; }
    public string Command { get; set; }
    public ulong SkinID { get; set; }
    public int AmountDrop { get; set; }
    public ItemData(string shortname, string Url, string Command, ulong SkinID, int amountdrop);
}

public class Itemss
{
    [JsonProperty("Предмет из игры(shortname)")]
    public string ShortName;
    [JsonProperty("Изображение")]
    public string Url;
    [JsonProperty("Команда")]
    public string Command;
    [JsonProperty("Скин")]
    public ulong SkinID;
    [JsonProperty("мин кол-во предмета")]
    public int MinDrop;
    [JsonProperty("макс кол-во предмета")]
    public int MaxDrop;
}

 class ConfigData
{
    [JsonProperty("Включить поддержку плагина скилл")]
    public bool Enable;
    [JsonProperty("Очищать ли склад предметов у игрока после вайпа?")]
    public bool ClearSklad;
    [JsonProperty("Описание плагина кейсов")]
    public string Desk;
    [JsonProperty("Очищать ли кулдавн игрока после вайпа?")]
    public bool ClearTime;
    [JsonProperty("Сколько выдавать очков xp за добычу ресурсов")]
    public double XpBonusGather;
    [JsonProperty("Сколько выдавать очков xp за убийство животных")]
    public double XpKillAnimal;
    [JsonProperty("Сколько выдавать очков xp за убийство игрока")]
    public double XpKillPlayer;
    [JsonProperty("Сколько выдавать очков xp за убийство игрока в голову")]
    public double XpKillPlayerHead;
    [JsonProperty("Сколько выдавать очков xp за сбитие вертолета")]
    public double XpKillHeli;
    [JsonProperty("Сколько выдавать очков xp за разбитие бочек")]
    public double XpKillBarrel;
    [JsonProperty("Сколько выдавать очков xp за уничтожении танка")]
    public double XpKillBradley;
    [JsonProperty("Сколько давать кулдауна после открытия первого кейса?(в секундах)")]
    public double TimeOne;
    [JsonProperty("Сколько давать кулдауна после открытия второго кейса?(в секундах)")]
    public double TimeTo;
    [JsonProperty("Сколько давать кулдауна после открытия третьего кейса?(в секундах)")]
    public double TimeFree;
    [JsonProperty("Сколько давать кулдауна после открытия четвертого кейса?(В секундах)")]
    public double TimeFoo;
    [JsonProperty("Описание для первого кейса!")]
    public string DescOne;
    [JsonProperty("Описание для второй кейса!")]
    public string DescTo;
    [JsonProperty("Описание для третьего кейса!")]
    public string DescFree;
    [JsonProperty("Описание для четвертого кейса!")]
    public string DescFoo;
    [JsonProperty("Настройка предметов для первого кейса")]
    public List<Itemss> ListDrop { get; set; }
    [JsonProperty("Настройка предметов для второго кейса")]
    public List<Itemss> ListDropTo { get; set; }
    [JsonProperty("Настройка предметов для третьего кейса")]
    public List<Itemss> ListDropFree { get; set; }
    [JsonProperty("Настройка предметов для четвертого кейса")]
    public List<Itemss> ListDropFoo { get; set; }
    public static ConfigData GetNewCong();
}

 class DataList
{
    [JsonProperty("Кулдаун первого кейса")]
    public double onecase { get; set; }
    [JsonProperty("Кулдаун второго кейса!")]
    public double tocase { get; set; }
    [JsonProperty("Кулдаун третьего кейса!")]
    public double freecase { get; set; }
    [JsonProperty("Кулдаун четвертого кейса!")]
    public double foocase { get; set; }
    [JsonProperty("Уровень")]
    public double level { get; set; }
    [JsonProperty("XP игрока")]
    public double xp { get; set; }
}


```

---

## TPChat

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("TPChat", "Sempai#3239", "5.0.0")]
public class TPChat : RustPlugin
{
    private class Prefix
    {
        [JsonProperty("Наименование префикса")]
        public string Name;
        [JsonProperty("Цвет префикса")]
        public string Color;
        [JsonProperty("Размер префикса")]
        public string Size;
        [JsonProperty("Скобки префикса")]
        public string Hooks;
    }

    private class Chatter
    {
        [JsonProperty("Подсказки")]
        public bool Tips;
        [JsonProperty("Звуки личных сообщений")]
        public bool Sound;
        [JsonProperty("Звук сообщений в чате")]
        public bool Censor;
        [JsonProperty("Сообщения в чат")]
        public bool Chat;
        [JsonProperty("Сообщения в ПМ")]
        public bool PM;
    }

    private class Name
    {
        public string Color;
    }

    private class Settings
    {
        [JsonProperty("Настройки текущего префикса")]
        public Prefix Prefixes;
        [JsonProperty("Настройки чата")]
        public Chatter Chatters;
        [JsonProperty("Настройки имени")]
        public Name Names;
        [JsonProperty("Список игнорируемых игроков")]
        public Dictionary<ulong, string> IgnoreList;
        [JsonProperty("Последнее личное сообщение для"), JsonIgnore]
        public BasePlayer ReplyTarget;
        public double UMT;
        public Settings();
        public static Settings Generate();
    }

    private class DataBase
    {
        public Dictionary<ulong, Settings> Settingses;
        public static DataBase LoadData();
        public void SaveData();
    }

    private class Configuration
    {
        [JsonProperty("Список доступных префиксов")]
        public Dictionary<string, string> Prefixes;
        [JsonProperty("Список доступных цветов")]
        public Dictionary<string, string> Colors;
        [JsonProperty("Список доступных размеров")]
        public Dictionary<string, string> Sizes;
        [JsonProperty("Список доступных типов префикса")]
        public Dictionary<string, string> Types;
        [JsonProperty("Список цензуры и исключений")]
        public Dictionary<string, List<string>> Censures;
        [JsonProperty("Список доступных сообщений")]
        public List<string> Broadcaster;
        [JsonProperty("Интервал отправки сообщений")]
        public int BroadcastInterval;
        [JsonProperty("Оповещать о подключении с префиксом")]
        public bool WelcomePrefix;
        [JsonProperty("SteamID отправителя в чат")]
        public ulong ImageID;
        public static Configuration LoadDefaultConfiguration();
    }

    private static TPChat _;
    private static DataBase Handler;
    private static Configuration Settingses;
    private static List<ulong> AntiSpamFilter;
    private void Unload();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    [ConsoleCommand("UI_Chat")]
    private void CmdConsoleHandler(ConsoleSystem.Arg arg);
    private class Response
    {
        [JsonProperty("country")]
        public string Country { get; set; }
    }

     void OnCorpse(BasePlayer player, BaseCorpse corpse);
    private void FetchStatus(BasePlayer player);
    private void OnPlayerInit(BasePlayer player);
    private static HashSet<Message> MessagesLogs;
    private void Broadcast(string text);
    [ChatCommand("none")]
     void ChatMute(BasePlayer player, string command, string[] args);
    [ChatCommand("none")]
     void ChatUnMute(BasePlayer player, string command, string[] args);
    private void MutePlayer(BasePlayer player, string initiatorName, int time, string reason);
     BasePlayer FindBasePlayer(string nameOrUserId);
    private void SendPrivateMessage(BasePlayer initiator, BasePlayer target, string message);
    private void SendPrivateMessage(ulong initiatorReadyId, ulong targetReadyId, string message);
    public class Message
    {
        [JsonProperty("id")]
        public string Id;
        [JsonProperty("displayName")]
        public string DisplayName;
        [JsonProperty("userId")]
        public string UserID;
        [JsonProperty("targetDisplayName")]
        public string TargetDisplayName;
        [JsonProperty("targetUserId")]
        public string TargetUserId;
        [JsonProperty("text")]
        public string Text;
        [JsonProperty("isTeam")]
        public bool IsTeam;
        [JsonProperty("time")]
        public string Time;
    }

    private void AddMessage(BasePlayer player, string message, bool team);
    private void AddMessage(string displayName, ulong userId, string message, string targetName, ulong targetId, bool team);
    private static void LogInfo(string text);
    private static void UnloadPlugin();
    private static double Time();
    private static void SafeMessage(BasePlayer player, string text, ulong avatarId);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    [ChatCommand("chat")]
    private void CmdChatCommandSecret(BasePlayer player, string command, string[] args);
    [ChatCommand("ignore")]
    private void CmdChatPersonalIgnore(BasePlayer player, string command, string[] args);
    [ChatCommand("r")]
    private void CmdChatPersonalReply(BasePlayer player, string command, string[] args);
    [ChatCommand("pm")]
    private void CmdChatPersonalMessage(BasePlayer player, string command, string[] args);
    private string SettingsLayer;
    private void InitializeInterface(BasePlayer player, bool reopen);
    private void LogChat(string message);
    private void LogPM(string message);
    private string PrepareNickForConnect(BasePlayer player);
    private string PrepareNick(BasePlayer player);
    private string Censure(string message);
    private string PrepareMessage(BasePlayer player, string message, bool team);
}

private class Prefix
{
    [JsonProperty("Наименование префикса")]
    public string Name;
    [JsonProperty("Цвет префикса")]
    public string Color;
    [JsonProperty("Размер префикса")]
    public string Size;
    [JsonProperty("Скобки префикса")]
    public string Hooks;
}

private class Chatter
{
    [JsonProperty("Подсказки")]
    public bool Tips;
    [JsonProperty("Звуки личных сообщений")]
    public bool Sound;
    [JsonProperty("Звук сообщений в чате")]
    public bool Censor;
    [JsonProperty("Сообщения в чат")]
    public bool Chat;
    [JsonProperty("Сообщения в ПМ")]
    public bool PM;
}

private class Name
{
    public string Color;
}

private class Settings
{
    [JsonProperty("Настройки текущего префикса")]
    public Prefix Prefixes;
    [JsonProperty("Настройки чата")]
    public Chatter Chatters;
    [JsonProperty("Настройки имени")]
    public Name Names;
    [JsonProperty("Список игнорируемых игроков")]
    public Dictionary<ulong, string> IgnoreList;
    [JsonProperty("Последнее личное сообщение для"), JsonIgnore]
    public BasePlayer ReplyTarget;
    public double UMT;
    public Settings();
    public static Settings Generate();
}

private class DataBase
{
    public Dictionary<ulong, Settings> Settingses;
    public static DataBase LoadData();
    public void SaveData();
}

private class Configuration
{
    [JsonProperty("Список доступных префиксов")]
    public Dictionary<string, string> Prefixes;
    [JsonProperty("Список доступных цветов")]
    public Dictionary<string, string> Colors;
    [JsonProperty("Список доступных размеров")]
    public Dictionary<string, string> Sizes;
    [JsonProperty("Список доступных типов префикса")]
    public Dictionary<string, string> Types;
    [JsonProperty("Список цензуры и исключений")]
    public Dictionary<string, List<string>> Censures;
    [JsonProperty("Список доступных сообщений")]
    public List<string> Broadcaster;
    [JsonProperty("Интервал отправки сообщений")]
    public int BroadcastInterval;
    [JsonProperty("Оповещать о подключении с префиксом")]
    public bool WelcomePrefix;
    [JsonProperty("SteamID отправителя в чат")]
    public ulong ImageID;
    public static Configuration LoadDefaultConfiguration();
}

private class Response
{
    [JsonProperty("country")]
    public string Country { get; set; }
}

public class Message
{
    [JsonProperty("id")]
    public string Id;
    [JsonProperty("displayName")]
    public string DisplayName;
    [JsonProperty("userId")]
    public string UserID;
    [JsonProperty("targetDisplayName")]
    public string TargetDisplayName;
    [JsonProperty("targetUserId")]
    public string TargetUserId;
    [JsonProperty("text")]
    public string Text;
    [JsonProperty("isTeam")]
    public bool IsTeam;
    [JsonProperty("time")]
    public string Time;
}


```

---

## TPEconomic

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("TPEconomic", "Sempai#3239", "1.0.3")]
 class TPEconomic : RustPlugin
{
    [ChatCommand("f")]
     void fd(BasePlayer player);
    private float API_GET_BALANCE(UInt64 player);
    private void API_PUT_BALANCE_PLUS(ulong player, float money);
    private void API_PUT_BALANCE_MINUS(ulong player, float money);
    private BasePlayer FindPlayer(string nameOrId);
    private void AnoncePlayer(ulong player, string txt);
     void OnPlayerConnected(BasePlayer player);
     void RegisteredDataUser(UInt64 player);
     void OnServerInitialized();
     void Unload();
    private void OnServerShutdown();
    private void Init();
    [JsonProperty("Система экономики")]
    public Dictionary<ulong, float> DataEconomics;
     void ReadData();
     void WriteData();
}


```

---

## TPKits

```csharp
using System.ComponentModel;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("TPKits", "Sempai#3239", "5.0.0")]
 class TPKits : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    static TPKits ins;
    private PluginConfig config;
    private List<Kit> kitsList;
    private Dictionary<ulong, Dictionary<string, KitData>> PlayersData;
    private Dictionary<BasePlayer, List<Kit>> OpenGUI;
    public List<BasePlayer> AdminSetting;
    private class RarityColor
    {
        [JsonProperty("Шанс выпадения предмета данной редкости")]
        public int Chance;
        [JsonProperty("Цвет этой редкости в интерфейсе")]
        public string Color;
        public RarityColor(int chance, string color);
    }

     class PluginConfig
    {
        [JsonProperty("Кастомные автокиты по привилегии (Привилегию устанавливаете в настройке кита) | Custom autokit, install privilege in the configuration of the kit")]
        public List<string> CustomAutoKits;
        [JsonProperty("Префикс чата | Chat Prefix")]
        public string DefaultPrefix { get; set; }
        [JsonProperty("Информация плагина")]
        public string Info;
        [JsonProperty("Настройка цвета предмета по шансу")]
        public List<RarityColor> RaritiesColor;
        [JsonProperty("Версия конфигурации | Configuration Version")]
        public VersionNumber PluginVersion;
        public static PluginConfig CreateDefault();
    }

    public class Kit
    {
        public string Name { get; set; }
        public string DisplayName { get; set; }
        public string CustomImage { get; set; }
        public int Amount { get; set; }
        public double Cooldown { get; set; }
        public bool Hide { get; set; }
        public string Permission { get; set; }
        public string Color { get; set; }
        public List<KitItem> Items { get; set; }
    }

    public class KitItem
    {
        public string ShortName { get; set; }
        public int Amount { get; set; }
        public int Blueprint { get; set; }
        public ulong SkinID { get; set; }
        public string Container { get; set; }
        public float Condition { get; set; }
        public int Change { get; set; }
        public bool EnableCommand { get; set; }
        [JsonProperty("Command (Player identifier %STEAMID%)")]
        public string Command { get; set; }
        public string CustomImage { get; set; }
        public Weapon Weapon { get; set; }
        public List<ItemContent> Content { get; set; }
    }

    public class Weapon
    {
        public string ammoType { get; set; }
        public int ammoAmount { get; set; }
    }

    public class ItemContent
    {
        public string ShortName { get; set; }
        public float Condition { get; set; }
        public int Amount { get; set; }
    }

    public class KitData
    {
        public int Amount { get; set; }
        public double Cooldown { get; set; }
    }

    public class Position
    {
        public string AnchorMin { get; set; }
        public string AnchorMax { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
     void OnPlayerRespawned(BasePlayer player);
    private void SaveKits();
    private void SaveData();
     void OnServerSave();
    private void LoadMessages();
    private void Loaded();
     void LoadData();
    private void Unload();
    public bool AddImage(string url, string name, ulong skin);
    public string GetImage(string shortname, ulong skin);
    private void OnServerInitialized();
     void CheckKits();
    private void OnPlayerDisconnected(BasePlayer player);
    [ConsoleCommand("kit")]
    private void CommandConsoleKit(ConsoleSystem.Arg arg);
    [ChatCommand("kit")]
    private void CommandChatKit(BasePlayer player, string command, string[] args);
    private bool GiveKit(BasePlayer player, string kitname, int page, bool admin);
    private void KitCommandAdd(BasePlayer player, string kitname);
    private void KitCommandClone(BasePlayer player, string kitname);
    private void KitCommandRemove(BasePlayer player, string kitname);
    private void KitCommandList(BasePlayer player);
    private void KitCommandReset(BasePlayer player);
    private void KitCommandGive(BasePlayer player, BasePlayer foundPlayer, string kitname);
    private void GiveItems(BasePlayer player, Kit kit);
    private void GiveItem(BasePlayer player, Item item, int percent, ItemContainer cont);
    private Item BuildItem(string ShortName, int Amount, ulong SkinID, float Condition, int blueprintTarget, Weapon weapon, List<ItemContent> Content);
    [ConsoleCommand("kits.page")]
     void cmdKitsPage(ConsoleSystem.Arg args);
    private void InitilizeUI(BasePlayer player);
    private void InitilizeKitsUI(BasePlayer player, int page);
    [ConsoleCommand("kitdesc")]
     void DescUI(ConsoleSystem.Arg args);
    private string FormatTime(double time);
    [ConsoleCommand("kit.drawkitinfo")]
     void cmdDrawKitInfo(ConsoleSystem.Arg args);
    private void DrawKitInfo(BasePlayer player, Kit kit);
    private int? ChangeSelect(int x);
    private void SendEffectToPlayer2(BasePlayer player, string effectPrefab);
    private KitData GetPlayerData(string name, ulong playerid);
    private List<KitItem> GetPlayerItems(BasePlayer player);
     string GetMsg(string key, BasePlayer player);
    private KitItem ItemToKit(Item item, string container);
    private List<Kit> GetKitsForPlayer(BasePlayer player);
    private BasePlayer FindPlayer(BasePlayer player, string nameOrID);
    private double GetCurrentTime();
    private static string HexToRustFormat(string hex);
    private static class TimeExtensions
    {
        public static string FormatShortTime(TimeSpan time);
        public static string FormatTime(TimeSpan time);
        private static string Format(int units, string form1, string form2, string form3);
    }

    [HookMethod("isKit")]
    public bool isKit(string kitName);
    [HookMethod("GetAllKits")]
    public string[] GetAllKits();
}

private class RarityColor
{
    [JsonProperty("Шанс выпадения предмета данной редкости")]
    public int Chance;
    [JsonProperty("Цвет этой редкости в интерфейсе")]
    public string Color;
    public RarityColor(int chance, string color);
}

 class PluginConfig
{
    [JsonProperty("Кастомные автокиты по привилегии (Привилегию устанавливаете в настройке кита) | Custom autokit, install privilege in the configuration of the kit")]
    public List<string> CustomAutoKits;
    [JsonProperty("Префикс чата | Chat Prefix")]
    public string DefaultPrefix { get; set; }
    [JsonProperty("Информация плагина")]
    public string Info;
    [JsonProperty("Настройка цвета предмета по шансу")]
    public List<RarityColor> RaritiesColor;
    [JsonProperty("Версия конфигурации | Configuration Version")]
    public VersionNumber PluginVersion;
    public static PluginConfig CreateDefault();
}

public class Kit
{
    public string Name { get; set; }
    public string DisplayName { get; set; }
    public string CustomImage { get; set; }
    public int Amount { get; set; }
    public double Cooldown { get; set; }
    public bool Hide { get; set; }
    public string Permission { get; set; }
    public string Color { get; set; }
    public List<KitItem> Items { get; set; }
}

public class KitItem
{
    public string ShortName { get; set; }
    public int Amount { get; set; }
    public int Blueprint { get; set; }
    public ulong SkinID { get; set; }
    public string Container { get; set; }
    public float Condition { get; set; }
    public int Change { get; set; }
    public bool EnableCommand { get; set; }
    [JsonProperty("Command (Player identifier %STEAMID%)")]
    public string Command { get; set; }
    public string CustomImage { get; set; }
    public Weapon Weapon { get; set; }
    public List<ItemContent> Content { get; set; }
}

public class Weapon
{
    public string ammoType { get; set; }
    public int ammoAmount { get; set; }
}

public class ItemContent
{
    public string ShortName { get; set; }
    public float Condition { get; set; }
    public int Amount { get; set; }
}

public class KitData
{
    public int Amount { get; set; }
    public double Cooldown { get; set; }
}

public class Position
{
    public string AnchorMin { get; set; }
    public string AnchorMax { get; set; }
}

private static class TimeExtensions
{
    public static string FormatShortTime(TimeSpan time);
    public static string FormatTime(TimeSpan time);
    private static string Format(int units, string form1, string form2, string form3);
}


```

---

## TPLotterySystem

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("TPLotterySystem", "Sempai#3239", "5.0.0")]
 class TPLotterySystem : RustPlugin
{
    private string Layer;
    private string Inventory;
    [PluginReference]
     Plugin ImageLibrary;
    private Hash<ulong, PlayersSettings> Settings;
    public class LotterySettings
    {
        [JsonProperty("ID предмета")]
        public string ID;
        [JsonProperty("Название предмета")]
        public string DisplayName;
        [JsonProperty("Короткое название предмета")]
        public string ShortName;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Дополнительная команда")]
        public string Command;
        [JsonProperty("Сколько нужно одинаковых предметов, чтобы забрать из инвентаря?")]
        public int Amount;
        [JsonProperty("Количество")]
        public int Count;
        [JsonProperty("Изображение")]
        public string Url;
    }

    public class PlayersSettings
    {
        [JsonProperty("Сколько игрок открыл ячеек")]
        public int Count;
        [JsonProperty("Откат")]
        public double Time;
        [JsonProperty("Список предметов")]
        public Dictionary<string, InventorySettings> Inventory;
    }

    public class InventorySettings
    {
        [JsonProperty("ID предмета")]
        public string ID;
        [JsonProperty("Название предмета")]
        public string DisplayName;
        [JsonProperty("Короткое название предмета")]
        public string ShortName;
        [JsonProperty("SkinID предмета")]
        public ulong SkinID;
        [JsonProperty("Дополнительная команда")]
        public string Command;
        [JsonProperty("Собранно одинаковых предметов")]
        public int Amount;
        [JsonProperty("Количество")]
        public int Count;
        [JsonProperty("Изображение")]
        public string Url;
    }

    public Configuration config;
    public class Configuration
    {
        [JsonProperty("Название")]
        public string Name;
        [JsonProperty("Откат на открытие ячеек(в секундах)")]
        public int Time;
        [JsonProperty("Список призов")]
        public List<LotterySettings> Settings;
        public static Configuration GetNewConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void SaveData(BasePlayer player);
    private void SaveData(ulong userID);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private InventorySettings GetItem(ulong userID, string name);
    private void AddItem(BasePlayer player, LotterySettings settings);
    private void Unload();
    [ConsoleCommand("lottery")]
    private void ConsoleLottery(ConsoleSystem.Arg args);
    private void LotteryUI(BasePlayer player, int page);
    private void PrizUI(BasePlayer player, string z, LotterySettings settings);
    private void InventoryUI(BasePlayer player);
    static double CurrentTime();
    public static string FormatShortTime(TimeSpan time);
}

public class LotterySettings
{
    [JsonProperty("ID предмета")]
    public string ID;
    [JsonProperty("Название предмета")]
    public string DisplayName;
    [JsonProperty("Короткое название предмета")]
    public string ShortName;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Дополнительная команда")]
    public string Command;
    [JsonProperty("Сколько нужно одинаковых предметов, чтобы забрать из инвентаря?")]
    public int Amount;
    [JsonProperty("Количество")]
    public int Count;
    [JsonProperty("Изображение")]
    public string Url;
}

public class PlayersSettings
{
    [JsonProperty("Сколько игрок открыл ячеек")]
    public int Count;
    [JsonProperty("Откат")]
    public double Time;
    [JsonProperty("Список предметов")]
    public Dictionary<string, InventorySettings> Inventory;
}

public class InventorySettings
{
    [JsonProperty("ID предмета")]
    public string ID;
    [JsonProperty("Название предмета")]
    public string DisplayName;
    [JsonProperty("Короткое название предмета")]
    public string ShortName;
    [JsonProperty("SkinID предмета")]
    public ulong SkinID;
    [JsonProperty("Дополнительная команда")]
    public string Command;
    [JsonProperty("Собранно одинаковых предметов")]
    public int Amount;
    [JsonProperty("Количество")]
    public int Count;
    [JsonProperty("Изображение")]
    public string Url;
}

public class Configuration
{
    [JsonProperty("Название")]
    public string Name;
    [JsonProperty("Откат на открытие ячеек(в секундах)")]
    public int Time;
    [JsonProperty("Список призов")]
    public List<LotterySettings> Settings;
    public static Configuration GetNewConfig();
}


```

---

## TPMenuSystem

```csharp
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;

Oxide.Plugins
[Info("TPMenuSystem", "Sempai#3239", "5.0.0")]
 class TPMenuSystem : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin TPRulesSystem;
     Plugin TPSkillSystem;
     Plugin TPWipeBlock;
     Plugin TPChat;
     Plugin TPKits;
     Plugin TPCases;
     Plugin TPStatsSystem;
     Plugin TPTeleportation;
     Plugin TPReportSystem;
     Plugin TPSkinMenu;
     Plugin TPBattlePass;
     Plugin GameStoresRUST;
     Plugin TPLotterySystem;
    public string Layer;
     Dictionary<ulong, bool> hidden;
     Dictionary<ulong, string> activeButton;
    public class Settings
    {
        [JsonProperty("Название отображаемое в меню")]
        public string DisplayName;
        [JsonProperty("Выполняемая команда в меню")]
        public string Command;
        [JsonProperty("Изображение которое будет отображаться на кнопке")]
        public string Url;
    }

     Configuration config;
     class Configuration
    {
        [JsonProperty("Изображение беннера в окне с информацией")]
        public string BannerURL;
        [JsonProperty("Первый заголовок в окне с информацией")]
        public string Title1;
        [JsonProperty("Второй заголовок в окне с информацией")]
        public string Title2;
        [JsonProperty("Первый текст с информацией")]
        public string Text1;
        [JsonProperty("Второй текст с информацией")]
        public string Text2;
        [JsonProperty("Заголовок донат магазина в окне с информацией")]
        public string ShopTitle;
        [JsonProperty("Ссылка донат магазина в окне с информацией")]
        public string ShopText;
        [JsonProperty("QRCode изображение донат магазина в окне с информацией")]
        public string ShopQR;
        [JsonProperty("Заголовок дискорда в окне с информацией")]
        public string DSTitle;
        [JsonProperty("Ссылка на группу в дискорде в окне с информацией")]
        public string DSText;
        [JsonProperty("QRCode изображение дискорда в окне с информацией")]
        public string DSQR;
        [JsonProperty("Настройки навигации меню")]
        public List<Settings> settings;
        public static Configuration GetNewConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     Dictionary<string, string> imageMenu;
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
    [ChatCommand("menu")]
     void ChatMenu(BasePlayer player);
    [ChatCommand("info")]
     void ChatInfo(BasePlayer player);
    [ChatCommand("skill")]
     void ChatSkill(BasePlayer player);
    [ChatCommand("block")]
     void ChatBlock(BasePlayer player);
    [ChatCommand("kits")]
     void ChatKit(BasePlayer player);
    [ChatCommand("case")]
     void ChatCase(BasePlayer player);
    [ChatCommand("stat")]
     void ChatStat(BasePlayer player);
    [ChatCommand("tpmenu")]
     void ChatTeleport(BasePlayer player);
    [ChatCommand("report")]
     void ChatReport(BasePlayer player);
    [ChatCommand("Chat")]
     void ChatChat(BasePlayer player);
    [ChatCommand("skin")]
     void ChatSkin(BasePlayer player);
    [ChatCommand("lot")]
     void ChatLot(BasePlayer player);
    [ConsoleCommand("hidden_menu")]
     void Hidden(ConsoleSystem.Arg args);
    [ConsoleCommand("menu")]
     void ConsoleMenu(ConsoleSystem.Arg args);
     void MenuUI(BasePlayer player, string name);
     void UI(BasePlayer player, string name);
     void ButtonUI(BasePlayer player);
     void InfoUI(BasePlayer player);
}

public class Settings
{
    [JsonProperty("Название отображаемое в меню")]
    public string DisplayName;
    [JsonProperty("Выполняемая команда в меню")]
    public string Command;
    [JsonProperty("Изображение которое будет отображаться на кнопке")]
    public string Url;
}

 class Configuration
{
    [JsonProperty("Изображение беннера в окне с информацией")]
    public string BannerURL;
    [JsonProperty("Первый заголовок в окне с информацией")]
    public string Title1;
    [JsonProperty("Второй заголовок в окне с информацией")]
    public string Title2;
    [JsonProperty("Первый текст с информацией")]
    public string Text1;
    [JsonProperty("Второй текст с информацией")]
    public string Text2;
    [JsonProperty("Заголовок донат магазина в окне с информацией")]
    public string ShopTitle;
    [JsonProperty("Ссылка донат магазина в окне с информацией")]
    public string ShopText;
    [JsonProperty("QRCode изображение донат магазина в окне с информацией")]
    public string ShopQR;
    [JsonProperty("Заголовок дискорда в окне с информацией")]
    public string DSTitle;
    [JsonProperty("Ссылка на группу в дискорде в окне с информацией")]
    public string DSText;
    [JsonProperty("QRCode изображение дискорда в окне с информацией")]
    public string DSQR;
    [JsonProperty("Настройки навигации меню")]
    public List<Settings> settings;
    public static Configuration GetNewConfig();
}


```

---

## TPOnMap

```csharp
using System.Collections.Generic;
using Mono.Cecil;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("TPOnMap", "CASHR", "1.0.1")]
public class TPOnMap : RustPlugin
{
    private List<ulong> TPList;
    [ChatCommand("tmap")]
    private void cmdChatAdminTP(BasePlayer player, string command, string[] arg);
    private void OnMapMarkerAdd(BasePlayer player, MapNote note);
    static float GetGroundPosition(Vector3 pos);
}


```

---

## TPPanelSystem

```csharp
using System;
using System.Globalization;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using UnityEngine;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("TPPanelSystem", "Sempai#3239/https://topplugin.ru/", "1.0.1")]
public class TPPanelSystem : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Dictionary<ulong, bool> PanelVisibility;
    private List<string> EventNames;
    private readonly string defaultSprite;
    private Dictionary<string, string> EventAnMinX;
    private Dictionary<string, string> EventAnMaxX;
    private readonly string Layer;
    private readonly string Layer1;
    protected CuiElement Panel(string name, string anMin, string anMax, string color, string parent, float fadeout, string png, bool cursor, string offsetmin, string offsetmax);
    protected CuiElement Text(string name, string parent, string color, string text, TextAnchor pos, int fsize, string anMin, string anMax, string fname);
    protected CuiElement Button(string name, string parent, string sprite, string command, string color, string anMin, string anMax);
    private PluginConfig cfg;
    public class PluginConfig
    {
        [JsonProperty("Основные настройки")]
        public Settings MainSettings;
        [JsonProperty("Сообщения")]
        public MessageSettings SettingsMessages;
        public class Settings
        {
            [JsonProperty("Включить показ кнопки магазина?")]
            public bool EnableStore;
            [JsonProperty("Фейк онлайн++")]
            public int FakeOnline;
        }

        public class MessageSettings
        {
            [JsonProperty("Время обновления сообщений")]
            public float RefreshTimer;
            [JsonProperty("Размер текста для автосообщений")]
            public int TextSize;
            [JsonProperty("Список сообщений", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<string> Messages;
        }

    }

    protected override void LoadDefaultConfig();
     Dictionary<string, string> Messages;
     void InitializeLang();
     void Loaded();
     void OnServerInitialized();
     void Unload();
     void OnEntityKill(BaseNetworkable entity);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnEntitySpawned(BaseNetworkable entity);
     bool HasEntity(string name);
    [ChatCommand("panel")]
     void PanelCommand(BasePlayer player, string command, string[] args);
     void refreshPlayers();
     void DrawMessage();
     void DrawEvents(BasePlayer player, string name);
    private void DrawStoreMenu(BasePlayer player);
    private void DrawMenu(BasePlayer player);
    private static string HexToRustFormat(string hex);
}

public class PluginConfig
{
    [JsonProperty("Основные настройки")]
    public Settings MainSettings;
    [JsonProperty("Сообщения")]
    public MessageSettings SettingsMessages;
    public class Settings
    {
        [JsonProperty("Включить показ кнопки магазина?")]
        public bool EnableStore;
        [JsonProperty("Фейк онлайн++")]
        public int FakeOnline;
    }

    public class MessageSettings
    {
        [JsonProperty("Время обновления сообщений")]
        public float RefreshTimer;
        [JsonProperty("Размер текста для автосообщений")]
        public int TextSize;
        [JsonProperty("Список сообщений", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Messages;
    }

}

public class Settings
{
    [JsonProperty("Включить показ кнопки магазина?")]
    public bool EnableStore;
    [JsonProperty("Фейк онлайн++")]
    public int FakeOnline;
}

public class MessageSettings
{
    [JsonProperty("Время обновления сообщений")]
    public float RefreshTimer;
    [JsonProperty("Размер текста для автосообщений")]
    public int TextSize;
    [JsonProperty("Список сообщений", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Messages;
}


```

---

## TPReportSystem

```csharp
using System.Net;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Core;
using System.Linq;
using System;
using Network;
using Newtonsoft.Json.Linq;
using ConVar;

Oxide.Plugins
[Info("TPReportSystem", "Sempai#3239", "5.0.0")]
 class TPReportSystem : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    public string Layer;
     Dictionary<ulong, string> name;
     Dictionary<ulong, DataBase> DB;
    public class DataBase
    {
        [JsonProperty("Ник игрока")]
        public string DisplayName;
        [JsonProperty("SteamID игрока")]
        public ulong SteamID;
        [JsonProperty("Кол-во проверок у игрока")]
        public int Count;
        [JsonProperty("Дискорд игрока")]
        public string DS;
        [JsonProperty("SteamID проверяющего")]
        public ulong SteamID2;
        [JsonProperty("Вызван ли игрок на проверку")]
        public bool Enable;
        [JsonProperty("Список с жалобами и их кол-вом")]
        public Dictionary<string, int> Res;
    }

    public Configuration config;
    public class Configuration
    {
        [JsonProperty("Пермишен для модераторов")]
        public string Perm;
        [JsonProperty("Информация плагина")]
        public string Info;
        [JsonProperty("Оповещание")]
        public string Title;
        [JsonProperty("Webhook")]
        public String WebhookNotify;
        [JsonProperty("Цвет сообщения в Discord (Можно найти на сайте - https://old.message.style/dashboard в разделе JSON)")]
        public Int32 Color;
        [JsonProperty("Заголовок сообщения")]
        public String AuthorName;
        [JsonProperty("Ссылка на иконку для аватарки сообщения")]
        public String IconURL;
        [JsonProperty("Список жалоб")]
        public Dictionary<string, string> Reasons;
        [JsonProperty("Список причин бана")]
        public List<string> Ban;
        public static Configuration GetNewCong();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     Dictionary<ulong, int> gg;
     void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
     void SaveDataBase();
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void Unload();
    [ChatCommand("contact")]
     void ChatContact(BasePlayer player, string command, string[] args);
    [ConsoleCommand("report")]
     void ConsoleReport(ConsoleSystem.Arg args);
     void ReportUI(BasePlayer player);
    [ConsoleCommand("reportdesc")]
     void DescUI(ConsoleSystem.Arg args);
     void UI(BasePlayer player, string name, int page);
     void ModerUI(BasePlayer player, string name, int page);
     void InfoUI(BasePlayer player, ulong id);
     string LayerCheck;
     void TargetUI(BasePlayer player, ulong id);
     void CheckUI(BasePlayer player);
     List<Fields> DT_PlayerSendReport(BasePlayer Sender, UInt64 TargetID, String Reason);
     List<Fields> DT_PlayerSendContact(BasePlayer Sender, String Contact);
     List<Fields> DT_PlayerCheck(BasePlayer Sender, UInt64 TargetID);
     List<Fields> DT_PlayerCheckRemove(BasePlayer Sender, UInt64 TargetID);
     void SendDiscord(String Webhook, List<Fields> fields, Authors Authors, Int32 Color);
     void Request(String url, String payload, Action<Int32> callback);
    public class Fields
    {
        public String name { get; set; }
        public String value { get; set; }
        public bool inline { get; set; }
        public Fields(String name, String value, bool inline);
    }

    public class Authors
    {
        public String name { get; set; }
        public String url { get; set; }
        public String icon_url { get; set; }
        public String proxy_icon_url { get; set; }
        public Authors(String name, String url, String icon_url, String proxy_icon_url);
    }

    public class FancyMessage
    {
        public String content { get; set; }
        public Boolean tts { get; set; }
        public Embeds[] embeds { get; set; }
        public class Embeds
        {
            public String title { get; set; }
            public Int32 color { get; set; }
            public List<Fields> fields { get; set; }
            public Authors author { get; set; }
            public Embeds(String title, Int32 color, List<Fields> fields, Authors author);
        }

        public FancyMessage(String content, bool tts, Embeds[] embeds);
        public String toJSON();
    }

}

public class DataBase
{
    [JsonProperty("Ник игрока")]
    public string DisplayName;
    [JsonProperty("SteamID игрока")]
    public ulong SteamID;
    [JsonProperty("Кол-во проверок у игрока")]
    public int Count;
    [JsonProperty("Дискорд игрока")]
    public string DS;
    [JsonProperty("SteamID проверяющего")]
    public ulong SteamID2;
    [JsonProperty("Вызван ли игрок на проверку")]
    public bool Enable;
    [JsonProperty("Список с жалобами и их кол-вом")]
    public Dictionary<string, int> Res;
}

public class Configuration
{
    [JsonProperty("Пермишен для модераторов")]
    public string Perm;
    [JsonProperty("Информация плагина")]
    public string Info;
    [JsonProperty("Оповещание")]
    public string Title;
    [JsonProperty("Webhook")]
    public String WebhookNotify;
    [JsonProperty("Цвет сообщения в Discord (Можно найти на сайте - https://old.message.style/dashboard в разделе JSON)")]
    public Int32 Color;
    [JsonProperty("Заголовок сообщения")]
    public String AuthorName;
    [JsonProperty("Ссылка на иконку для аватарки сообщения")]
    public String IconURL;
    [JsonProperty("Список жалоб")]
    public Dictionary<string, string> Reasons;
    [JsonProperty("Список причин бана")]
    public List<string> Ban;
    public static Configuration GetNewCong();
}

public class Fields
{
    public String name { get; set; }
    public String value { get; set; }
    public bool inline { get; set; }
    public Fields(String name, String value, bool inline);
}

public class Authors
{
    public String name { get; set; }
    public String url { get; set; }
    public String icon_url { get; set; }
    public String proxy_icon_url { get; set; }
    public Authors(String name, String url, String icon_url, String proxy_icon_url);
}

public class FancyMessage
{
    public String content { get; set; }
    public Boolean tts { get; set; }
    public Embeds[] embeds { get; set; }
    public class Embeds
    {
        public String title { get; set; }
        public Int32 color { get; set; }
        public List<Fields> fields { get; set; }
        public Authors author { get; set; }
        public Embeds(String title, Int32 color, List<Fields> fields, Authors author);
    }

    public FancyMessage(String content, bool tts, Embeds[] embeds);
    public String toJSON();
}

public class Embeds
{
    public String title { get; set; }
    public Int32 color { get; set; }
    public List<Fields> fields { get; set; }
    public Authors author { get; set; }
    public Embeds(String title, Int32 color, List<Fields> fields, Authors author);
}


```

---

## TPSkillSystem

```csharp
using System;
using System.Reflection;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using System.Globalization;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("TPSkillSystem", "Sempai#3239", "5.0.0")]
public class TPSkillSystem : RustPlugin
{
    public class classSkillParameters
    {
        [JsonProperty("Количество XP(%)")]
        public double countXP;
        [JsonProperty("Количество DNK")]
        public float countDNK;
    }

    public class PlayerSkillData
    {
        [JsonProperty("Название")]
        public string Name;
        [JsonProperty("Описание")]
        public string Description;
        [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла")]
        public List<string> ShortnameList;
        [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)")]
        public int Rate;
        [JsonProperty("Сколько % прибавлять за уровень")]
        public int PercentIncrement;
        [JsonProperty("Сколько количество жизней будет увеличиваться на XRate")]
        public double HPIncrementInRate;
        [JsonProperty("Сколько максимум хп игрок получит от регенерации")]
        public int MaxRegen;
        [JsonProperty("Максимальная защита от способности")]
        public int MaxProtection;
        [JsonProperty("Максимальный урон от способности")]
        public int MaxAtack;
        [JsonProperty("Уровень способности")]
        public int Level;
        [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)")]
        public int ReturnPercent;
    }

    public static string CUILayerName;
    public static string DescLayerName;
    public static string MessageUILayerName;
    public static string UIName;
    private Dictionary<NetworkableId, BasePlayer> BradleyAtackerList;
    private Dictionary<NetworkableId, BasePlayer> HeliAtackerList;
    public Dictionary<ulong, classSkillParameters> PlayersSkillParameters;
    public Dictionary<ulong, List<PlayerSkillData>> PlayersSkillDataMassiv;
    private static DefaultSettings MainGUISettings;
     List<PlayerSkillData> DefaultPlayersSkillData();
    private class DefaultSettings
    {
        public class classSkill
        {
            [JsonProperty("Название", Order = 0)]
            public string Name;
            [JsonProperty("Описание", Order = 1)]
            public string Description;
            [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла", Order = 2)]
            public List<string> ShortnameList;
            [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)", Order = 3)]
            public int Rate;
            [JsonProperty("Сколько % прибавлять за уровень", Order = 4)]
            public int PercentIncrement;
            [JsonProperty("Сколько количество жизней будет увеличиваться на XRate", Order = 5)]
            public double HPIncrementInRate;
            [JsonProperty("Сколько максимум хп игрок получит от регенерации", Order = 6)]
            public int MaxRegen;
            [JsonProperty("Максимальная защита от способности", Order = 7)]
            public int MaxProtection;
            [JsonProperty("Максимальный урон от способности", Order = 8)]
            public int MaxAtack;
            [JsonProperty("Максимальный уровень", Order = 9)]
            public int MaxLevel;
            [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)", Order = 10)]
            public int ReturnPercent;
        }

        [JsonProperty("Влючить поддержку кейсов (для отображения прогресс бара выше кейсов)", Order = 1)]
        public bool Enabled;
        [JsonProperty("Описание плагина скилов", Order = 1)]
        public string Desc;
        [JsonProperty("Очищать ли очки игроков после вайпа", Order = 1)]
        public bool ClearPoint;
        [JsonProperty("Очищать ли скилы игроков после вайпа", Order = 1)]
        public bool ClearSkill;
        [JsonProperty("Предметы-исключения(если игрок добаывает этим предметом , опыт не зачисляется)", Order = 3)]
        public List<string> ItemExceptionList;
        [JsonProperty("Настройка способностей", Order = 2)]
        public List<classSkill> configSkillSettings;
        internal class classSkillSettings
        {
            [JsonProperty("Сколько опыта давать за убийство животных", Order = 5)]
            public double xpForKillAnimal;
            [JsonProperty("Сколько опыта давать за убийство NPC", Order = 2)]
            public double xpForKillNPC;
            [JsonProperty("Сколько опыта давать за убийство игрока в голову", Order = 4)]
            public double xpForKillHeadshot;
            [JsonProperty("Сколько опыта давать за убийство игрока", Order = 3)]
            public double xpForKillPlayer;
            [JsonProperty("Сколько опыта давать за добычу ресурсов", Order = 0)]
            public double xpForResourceExtraction;
            [JsonProperty("Сколько опыта давать за сбитие чинука", Order = 8)]
            public double xpForKillChinuk;
            [JsonProperty("Сколько опыта давать за разбитие бочек", Order = 1)]
            public double xpForBreakBarrels;
            [JsonProperty("Сколько опыта давать за взрыв танка", Order = 7)]
            public double xpForKillBredley;
            [JsonProperty("Сколько опыта давать за сбитие вертолета", Order = 6)]
            public double xpForKillHely;
        }

        [JsonProperty("Настройка опыта за действия", Order = 0)]
        public classSkillSettings xpSkillSettingsConfig;
        [JsonProperty("Сколько DNK давать за новый уровень", Order = 0)]
        public int countDNKForNewLevel;
        public static DefaultSettings SetDefaultSettings();
        [JsonProperty("Настройка UI", Order = 1)]
        public classGUISettings GUISettings;
        internal class classGUISettings
        {
            [JsonProperty("Цвет заднего фона в меню способностей", Order = 4)]
            public string BackgroundColor;
            [JsonProperty("Цвет заднего фона способности", Order = 6)]
            public string SkillBackgroundColor;
        }

        [JsonProperty("Сообщение при 100% опыта", Order = 2)]
        public string LevelupText;
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    [Oxide.Core.Plugins.HookMethod("OnDispenserGather")]
     void HookOnDisoenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void DispenserGatherFunc(ResourceDispenser dispenser, BaseEntity entity, Item item);
    [Oxide.Core.Plugins.HookMethod("OnDispenserBonus")]
     object MyHookDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
     object DispenserBonusFunc(ResourceDispenser dispenser, BasePlayer player, Item item);
     void OnCollectiblePickup(Item item, BasePlayer player, CollectibleEntity entity);
     List<string> Prefabs;
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void Protection(BaseCombatEntity entity, HitInfo info, float damage);
    [Oxide.Core.Plugins.HookMethod("OnPlayerRespawned")]
     void myHookOnPlayerRespawn(BasePlayer player);
    private void AddHealth(BasePlayer player);
     object OnPlayerHealthChange(BasePlayer player, float oldValue, float newValue);
    [Oxide.Core.Plugins.HookMethod("OnEntityDeath")]
     void OnDeadEntity(BaseCombatEntity entity, HitInfo info);
     void EntityDeathFunc(BaseCombatEntity entity, HitInfo info);
    [Oxide.Core.Plugins.HookMethod("OnEntityTakeDamage")]
     void MyHookTakeDamage(BaseCombatEntity entity, HitInfo info);
     void TakeDamageFunc(BaseCombatEntity entity, HitInfo info);
    [Oxide.Core.Plugins.HookMethod("OnServerInitialized")]
     void MyHookServerInit();
    private void InitSkillSystem();
    [Oxide.Core.Plugins.HookMethod("Unload")]
     void MyHookUnload();
     void OnNewSave();
     void UnloadFunc();
    [Oxide.Core.Plugins.HookMethod("OnServerSave")]
     void MyHookOnServerSave();
     void ServerSaveFunc();
    [Oxide.Core.Plugins.HookMethod("OnPlayerConnected")]
     void MyHookPlayerInit(BasePlayer player);
    public void PlayerInitFunc(BasePlayer player);
     void ShowMainUI(BasePlayer player);
    [ConsoleCommand("descs")]
     void DescUI(ConsoleSystem.Arg args);
    private void DescriptionUi(BasePlayer player, float element);
    public void GetXP(BasePlayer player, double param);
    private IEnumerator UnloadUI(BasePlayer player, CuiElementContainer container, int element);
    [ConsoleCommand("enableskill")]
     void Console_EnableCase(ConsoleSystem.Arg args);
     Dictionary<ulong, bool> EnableUI;
     void InterfaceXP(BasePlayer player);
     Plugin ImageLibrary { get; set; }
    public bool AddImage(string url, string shortname, ulong skin);
    public string GetImage(string Name, ulong l);
    [PluginReference]
    private Plugin TPCases;
    private static string getHEXColor(string colorValue);
    [ConsoleCommand("plussbtn")]
     void cmdPlusBtnPress(ConsoleSystem.Arg Argum);
    public void cmdPlusLevel(BasePlayer player, int element);
    [ConsoleCommand("skillpoint")]
     void GiveSkillPointConsoleCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("minusbtn")]
     void cmdMinusBtnPress(ConsoleSystem.Arg Argum);
    public void cmdMinusLevel(BasePlayer player, int element);
    [ConsoleCommand("opendescription")]
     void cmdOpenDescription(ConsoleSystem.Arg Args);
    [ConsoleCommand("open_dnk_system")]
     void cmdOpenDNKPanel(ConsoleSystem.Arg Argum);
     void Log(string filename, string text);
}

public class classSkillParameters
{
    [JsonProperty("Количество XP(%)")]
    public double countXP;
    [JsonProperty("Количество DNK")]
    public float countDNK;
}

public class PlayerSkillData
{
    [JsonProperty("Название")]
    public string Name;
    [JsonProperty("Описание")]
    public string Description;
    [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла")]
    public List<string> ShortnameList;
    [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)")]
    public int Rate;
    [JsonProperty("Сколько % прибавлять за уровень")]
    public int PercentIncrement;
    [JsonProperty("Сколько количество жизней будет увеличиваться на XRate")]
    public double HPIncrementInRate;
    [JsonProperty("Сколько максимум хп игрок получит от регенерации")]
    public int MaxRegen;
    [JsonProperty("Максимальная защита от способности")]
    public int MaxProtection;
    [JsonProperty("Максимальный урон от способности")]
    public int MaxAtack;
    [JsonProperty("Уровень способности")]
    public int Level;
    [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)")]
    public int ReturnPercent;
}

private class DefaultSettings
{
    public class classSkill
    {
        [JsonProperty("Название", Order = 0)]
        public string Name;
        [JsonProperty("Описание", Order = 1)]
        public string Description;
        [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла", Order = 2)]
        public List<string> ShortnameList;
        [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)", Order = 3)]
        public int Rate;
        [JsonProperty("Сколько % прибавлять за уровень", Order = 4)]
        public int PercentIncrement;
        [JsonProperty("Сколько количество жизней будет увеличиваться на XRate", Order = 5)]
        public double HPIncrementInRate;
        [JsonProperty("Сколько максимум хп игрок получит от регенерации", Order = 6)]
        public int MaxRegen;
        [JsonProperty("Максимальная защита от способности", Order = 7)]
        public int MaxProtection;
        [JsonProperty("Максимальный урон от способности", Order = 8)]
        public int MaxAtack;
        [JsonProperty("Максимальный уровень", Order = 9)]
        public int MaxLevel;
        [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)", Order = 10)]
        public int ReturnPercent;
    }

    [JsonProperty("Влючить поддержку кейсов (для отображения прогресс бара выше кейсов)", Order = 1)]
    public bool Enabled;
    [JsonProperty("Описание плагина скилов", Order = 1)]
    public string Desc;
    [JsonProperty("Очищать ли очки игроков после вайпа", Order = 1)]
    public bool ClearPoint;
    [JsonProperty("Очищать ли скилы игроков после вайпа", Order = 1)]
    public bool ClearSkill;
    [JsonProperty("Предметы-исключения(если игрок добаывает этим предметом , опыт не зачисляется)", Order = 3)]
    public List<string> ItemExceptionList;
    [JsonProperty("Настройка способностей", Order = 2)]
    public List<classSkill> configSkillSettings;
    internal class classSkillSettings
    {
        [JsonProperty("Сколько опыта давать за убийство животных", Order = 5)]
        public double xpForKillAnimal;
        [JsonProperty("Сколько опыта давать за убийство NPC", Order = 2)]
        public double xpForKillNPC;
        [JsonProperty("Сколько опыта давать за убийство игрока в голову", Order = 4)]
        public double xpForKillHeadshot;
        [JsonProperty("Сколько опыта давать за убийство игрока", Order = 3)]
        public double xpForKillPlayer;
        [JsonProperty("Сколько опыта давать за добычу ресурсов", Order = 0)]
        public double xpForResourceExtraction;
        [JsonProperty("Сколько опыта давать за сбитие чинука", Order = 8)]
        public double xpForKillChinuk;
        [JsonProperty("Сколько опыта давать за разбитие бочек", Order = 1)]
        public double xpForBreakBarrels;
        [JsonProperty("Сколько опыта давать за взрыв танка", Order = 7)]
        public double xpForKillBredley;
        [JsonProperty("Сколько опыта давать за сбитие вертолета", Order = 6)]
        public double xpForKillHely;
    }

    [JsonProperty("Настройка опыта за действия", Order = 0)]
    public classSkillSettings xpSkillSettingsConfig;
    [JsonProperty("Сколько DNK давать за новый уровень", Order = 0)]
    public int countDNKForNewLevel;
    public static DefaultSettings SetDefaultSettings();
    [JsonProperty("Настройка UI", Order = 1)]
    public classGUISettings GUISettings;
    internal class classGUISettings
    {
        [JsonProperty("Цвет заднего фона в меню способностей", Order = 4)]
        public string BackgroundColor;
        [JsonProperty("Цвет заднего фона способности", Order = 6)]
        public string SkillBackgroundColor;
    }

    [JsonProperty("Сообщение при 100% опыта", Order = 2)]
    public string LevelupText;
}

public class classSkill
{
    [JsonProperty("Название", Order = 0)]
    public string Name;
    [JsonProperty("Описание", Order = 1)]
    public string Description;
    [JsonProperty("Shortname на который будет действовать увеличенный рейтинг скилла", Order = 2)]
    public List<string> ShortnameList;
    [JsonProperty("Рейты игрока ( ПРИБАВЛЯЮТСЯ % ОТ ДОБЫТОГО ИМ РЕСУРСА)", Order = 3)]
    public int Rate;
    [JsonProperty("Сколько % прибавлять за уровень", Order = 4)]
    public int PercentIncrement;
    [JsonProperty("Сколько количество жизней будет увеличиваться на XRate", Order = 5)]
    public double HPIncrementInRate;
    [JsonProperty("Сколько максимум хп игрок получит от регенерации", Order = 6)]
    public int MaxRegen;
    [JsonProperty("Максимальная защита от способности", Order = 7)]
    public int MaxProtection;
    [JsonProperty("Максимальный урон от способности", Order = 8)]
    public int MaxAtack;
    [JsonProperty("Максимальный уровень", Order = 9)]
    public int MaxLevel;
    [JsonProperty("Сколько возвращать DNK при откате способности(0-100%)", Order = 10)]
    public int ReturnPercent;
}

internal class classSkillSettings
{
    [JsonProperty("Сколько опыта давать за убийство животных", Order = 5)]
    public double xpForKillAnimal;
    [JsonProperty("Сколько опыта давать за убийство NPC", Order = 2)]
    public double xpForKillNPC;
    [JsonProperty("Сколько опыта давать за убийство игрока в голову", Order = 4)]
    public double xpForKillHeadshot;
    [JsonProperty("Сколько опыта давать за убийство игрока", Order = 3)]
    public double xpForKillPlayer;
    [JsonProperty("Сколько опыта давать за добычу ресурсов", Order = 0)]
    public double xpForResourceExtraction;
    [JsonProperty("Сколько опыта давать за сбитие чинука", Order = 8)]
    public double xpForKillChinuk;
    [JsonProperty("Сколько опыта давать за разбитие бочек", Order = 1)]
    public double xpForBreakBarrels;
    [JsonProperty("Сколько опыта давать за взрыв танка", Order = 7)]
    public double xpForKillBredley;
    [JsonProperty("Сколько опыта давать за сбитие вертолета", Order = 6)]
    public double xpForKillHely;
}

internal class classGUISettings
{
    [JsonProperty("Цвет заднего фона в меню способностей", Order = 4)]
    public string BackgroundColor;
    [JsonProperty("Цвет заднего фона способности", Order = 6)]
    public string SkillBackgroundColor;
}


```

---

## TPSkinMenu

```csharp
using System;
using Newtonsoft.Json;
using UnityEngine;
using System.Linq;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using System.Collections.Generic;
using System.Collections;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("TPSkinMenu", "Sempai#3239", "5.0.0")]
 class TPSkinMenu : RustPlugin
{
    private void SetSkinTransport(BasePlayer player, BaseVehicle vehicle, string shortname);
    protected override void LoadConfig();
    private void SaveData(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    [ConsoleCommand("skin_c")]
    private void ccmdCategoryS(ConsoleSystem.Arg args);
    [ConsoleCommand("skinimage_reload")]
    private void ccmdReloadIMG(ConsoleSystem.Arg args);
    private void CategoryGUI(BasePlayer player, int page);
    [ConsoleCommand("xskin")]
    private void ccmdAdmin(ConsoleSystem.Arg args);
    private void OnServerInitialized();
    private readonly Dictionary<string, string> _shortnamesEntity;
    private void SetSkinItem(BasePlayer player, string item, ulong skin);
    private IEnumerator ReloadImage();
    private void SkinGUI(BasePlayer player, string item, int Page);
    private void OnPlayerConnected(BasePlayer player);
    private void ItemGUI(BasePlayer player, string category, int Page);
    [ConsoleCommand("page.xskinmenu")]
    private void ccmdPage(ConsoleSystem.Arg args);
    private IEnumerator LoadImage();
    public Dictionary<string, string> errorshortnames;
    private void OnNewSave();
    private void SettingGUI(BasePlayer player);
    private void Unload();
    public Dictionary<ulong, string> errorskins;
    [PluginReference]
    private Plugin ImageLibrary;
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnPlayerRespawned(BasePlayer player);
    private void InitializeLang();
    private Dictionary<ulong, bool> StoredDataFriends;
    [ChatCommand("skinentity")]
    private void cmdSetSkinEntity(BasePlayer player);
    protected override void LoadDefaultConfig();
    [ChatCommand("skin")]
    private void cmdOpenGUI(BasePlayer player);
    public Dictionary<string, ulong> _items;
    [ConsoleCommand("skin_s")]
    private void ccmdSetting(ConsoleSystem.Arg args);
    private Dictionary<BasePlayer, DateTime> Cooldowns;
    private Dictionary<ulong, Data> StoredData;
    private void OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter ItemCrafter);
    private void SendInfo(BasePlayer player, string message);
    private Coroutine _coroutine;
    private SkinConfig config;
    private void SetSkinCraftGive(BasePlayer player, Item item);
    public Dictionary<string, List<ulong>> StoredDataSkins;
    private void LoadData(BasePlayer player);
    public Dictionary<string, int> _itemsId;
    private object OnItemSkinChange(int skinID, Item item, StorageContainer container, BasePlayer player);
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
            [JsonProperty("Распространять черный список скинов на ремонтный верстак")]
            public bool RepairBench;
            [JsonProperty("Включить кнопку для удаления скинов через UI")]
            public bool DeleteButton;
            [JsonProperty("Запретить менять скин предмета которого нет в конфиге")]
            public bool ReskinConfig;
            [JsonProperty("Изменять скины на предметы после респавна игрока")]
            public bool ReskinRespawn;
            [JsonProperty("Логи в консоль загрузки изображений")]
            public bool LogLoadIMG;
            [JsonProperty("Логи в консоль перезагрузки изображений")]
            public bool LogReloadIMG;
            [JsonProperty("Черный список скинов которые нельзя изменить. [ Например: огненные перчатки, огненный топор ]]")]
            public List<ulong> Blacklist;
        }

        [JsonProperty("Меню настроект")]
        public MenuSSetting MenuS;
        [JsonProperty("Настройка категорий - [ Шортнейм предмета | Дефолтный скин предмета ]")]
        public Dictionary<string, Dictionary<string, ulong>> Category;
        [JsonProperty("Настройки GUI")]
        public GUISetting GUI;
        internal class PlayerSetting
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
            [JsonProperty("Разрешить друзьям изменять скины")]
            public bool ChangeF;
        }

        [JsonProperty("Общие настройки")]
        public GeneralSetting Setting;
        [JsonProperty("Настройки игрока по умолчанию")]
        public PlayerSetting PSetting;
        internal class GUISetting
        {
            [JsonProperty("Информация плагина")]
            public string Info;
            [JsonProperty("Обновлять UI страницу после выбора скина")]
            public bool SkinUP;
            [JsonProperty("Обновлять UI страницу после удаления скина")]
            public bool DelSkinUP;
            [JsonProperty("Отображать выбранные скины на главной")]
            public bool MainSkin;
            [JsonProperty("Цвет активной категории")]
            public string ActiveColor;
            [JsonProperty("Цвет неактивной категории")]
            public string InactiveColor;
            [JsonProperty("Цвет блока выбранного скина")]
            public string ActiveBlockColor;
        }

        internal class MenuSSetting
        {
            [JsonProperty("Иконка включенного параметра")]
            public string TButtonIcon;
            [JsonProperty("Иконка вылюченного параметра")]
            public string FButtonIcon;
        }

        [JsonProperty("Настройки API/изображений")]
        public APISetting API;
        internal class APISetting
        {
            [JsonProperty("Какое API использовать для загрузки изображений - [ True - обычные изображения из Steam Workshop (практически все существующие скины) | False - красивые изображения (все принятые скины разработчиками, а также половина из Steam Workshop) ]")]
            public bool APIOption;
            [JsonProperty("Отключить проверку на наличие изображения в дате ImageLibrary (изображение будет повторно загружаться после каждой загрузки/перезагрузки плагина) - Если вы не знаете/понимаете как работают изображения в ImageLibrary или у вас проблемы с изображениями, то полезно отключить проверку (true)")]
            public bool HasImage;
            [JsonProperty("Отображать изображения предметов и скинов методами игры. ( Установите false если хотите использовать API и плагин ImageLibrary )")]
            public bool GameIMG;
        }

        public static SkinConfig GetNewConfiguration();
    }

    [ConsoleCommand("skinimage_stop")]
    private void ccmdIMGStop(ConsoleSystem.Arg args);
    protected override void SaveConfig();
    private Data DATA();
    private void GUI(BasePlayer player);
    [ConsoleCommand("skindesc")]
     void DescUI(ConsoleSystem.Arg args);
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

    private void GenerateItems();
    private void SetSkinEntity(BasePlayer player, BaseEntity entity, string shortname);
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
        [JsonProperty("Распространять черный список скинов на ремонтный верстак")]
        public bool RepairBench;
        [JsonProperty("Включить кнопку для удаления скинов через UI")]
        public bool DeleteButton;
        [JsonProperty("Запретить менять скин предмета которого нет в конфиге")]
        public bool ReskinConfig;
        [JsonProperty("Изменять скины на предметы после респавна игрока")]
        public bool ReskinRespawn;
        [JsonProperty("Логи в консоль загрузки изображений")]
        public bool LogLoadIMG;
        [JsonProperty("Логи в консоль перезагрузки изображений")]
        public bool LogReloadIMG;
        [JsonProperty("Черный список скинов которые нельзя изменить. [ Например: огненные перчатки, огненный топор ]]")]
        public List<ulong> Blacklist;
    }

    [JsonProperty("Меню настроект")]
    public MenuSSetting MenuS;
    [JsonProperty("Настройка категорий - [ Шортнейм предмета | Дефолтный скин предмета ]")]
    public Dictionary<string, Dictionary<string, ulong>> Category;
    [JsonProperty("Настройки GUI")]
    public GUISetting GUI;
    internal class PlayerSetting
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
        [JsonProperty("Разрешить друзьям изменять скины")]
        public bool ChangeF;
    }

    [JsonProperty("Общие настройки")]
    public GeneralSetting Setting;
    [JsonProperty("Настройки игрока по умолчанию")]
    public PlayerSetting PSetting;
    internal class GUISetting
    {
        [JsonProperty("Информация плагина")]
        public string Info;
        [JsonProperty("Обновлять UI страницу после выбора скина")]
        public bool SkinUP;
        [JsonProperty("Обновлять UI страницу после удаления скина")]
        public bool DelSkinUP;
        [JsonProperty("Отображать выбранные скины на главной")]
        public bool MainSkin;
        [JsonProperty("Цвет активной категории")]
        public string ActiveColor;
        [JsonProperty("Цвет неактивной категории")]
        public string InactiveColor;
        [JsonProperty("Цвет блока выбранного скина")]
        public string ActiveBlockColor;
    }

    internal class MenuSSetting
    {
        [JsonProperty("Иконка включенного параметра")]
        public string TButtonIcon;
        [JsonProperty("Иконка вылюченного параметра")]
        public string FButtonIcon;
    }

    [JsonProperty("Настройки API/изображений")]
    public APISetting API;
    internal class APISetting
    {
        [JsonProperty("Какое API использовать для загрузки изображений - [ True - обычные изображения из Steam Workshop (практически все существующие скины) | False - красивые изображения (все принятые скины разработчиками, а также половина из Steam Workshop) ]")]
        public bool APIOption;
        [JsonProperty("Отключить проверку на наличие изображения в дате ImageLibrary (изображение будет повторно загружаться после каждой загрузки/перезагрузки плагина) - Если вы не знаете/понимаете как работают изображения в ImageLibrary или у вас проблемы с изображениями, то полезно отключить проверку (true)")]
        public bool HasImage;
        [JsonProperty("Отображать изображения предметов и скинов методами игры. ( Установите false если хотите использовать API и плагин ImageLibrary )")]
        public bool GameIMG;
    }

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
    [JsonProperty("Распространять черный список скинов на ремонтный верстак")]
    public bool RepairBench;
    [JsonProperty("Включить кнопку для удаления скинов через UI")]
    public bool DeleteButton;
    [JsonProperty("Запретить менять скин предмета которого нет в конфиге")]
    public bool ReskinConfig;
    [JsonProperty("Изменять скины на предметы после респавна игрока")]
    public bool ReskinRespawn;
    [JsonProperty("Логи в консоль загрузки изображений")]
    public bool LogLoadIMG;
    [JsonProperty("Логи в консоль перезагрузки изображений")]
    public bool LogReloadIMG;
    [JsonProperty("Черный список скинов которые нельзя изменить. [ Например: огненные перчатки, огненный топор ]]")]
    public List<ulong> Blacklist;
}

internal class PlayerSetting
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
    [JsonProperty("Разрешить друзьям изменять скины")]
    public bool ChangeF;
}

internal class GUISetting
{
    [JsonProperty("Информация плагина")]
    public string Info;
    [JsonProperty("Обновлять UI страницу после выбора скина")]
    public bool SkinUP;
    [JsonProperty("Обновлять UI страницу после удаления скина")]
    public bool DelSkinUP;
    [JsonProperty("Отображать выбранные скины на главной")]
    public bool MainSkin;
    [JsonProperty("Цвет активной категории")]
    public string ActiveColor;
    [JsonProperty("Цвет неактивной категории")]
    public string InactiveColor;
    [JsonProperty("Цвет блока выбранного скина")]
    public string ActiveBlockColor;
}

internal class MenuSSetting
{
    [JsonProperty("Иконка включенного параметра")]
    public string TButtonIcon;
    [JsonProperty("Иконка вылюченного параметра")]
    public string FButtonIcon;
}

internal class APISetting
{
    [JsonProperty("Какое API использовать для загрузки изображений - [ True - обычные изображения из Steam Workshop (практически все существующие скины) | False - красивые изображения (все принятые скины разработчиками, а также половина из Steam Workshop) ]")]
    public bool APIOption;
    [JsonProperty("Отключить проверку на наличие изображения в дате ImageLibrary (изображение будет повторно загружаться после каждой загрузки/перезагрузки плагина) - Если вы не знаете/понимаете как работают изображения в ImageLibrary или у вас проблемы с изображениями, то полезно отключить проверку (true)")]
    public bool HasImage;
    [JsonProperty("Отображать изображения предметов и скинов методами игры. ( Установите false если хотите использовать API и плагин ImageLibrary )")]
    public bool GameIMG;
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

## TPStatsSystem

```csharp
using System.IO;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("TPStatsSystem", "Sempai#3239", "5.0.0")]
public class TPStatsSystem : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private string[] _gatherHooks;
    private static TPStatsSystem plugin;
    private const string Layer;
    private readonly Dictionary<ulong, BasePlayer> _lastHeli;
    private Dictionary<string, int> _itemIds;
    private List<ulong> _lootEntity;
    private bool HasImage(string imageName, ulong imageId);
    private bool AddImage(string url, string shortname, ulong skin);
    private string GetImage(string shortname, ulong skin);
     Dictionary<ulong, playerData> _playerList;
    public class playerData
    {
        public string Name;
        public int Point;
        public int PlayTimeInServer;
        public int Kill;
        public int Death;
        public Dictionary<string, int> Gather;
        public int TotalFarm();
    }

    private playerData GetPlayerData(ulong member);
    private void SavePlayer();
    private void LoadPlayer();
    private void OnPluginLoaded(Plugin plugin);
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave(string filename);
    private void WipeEnded();
    private void PlayerTop(BasePlayer player, int page);
    [ConsoleCommand("statdesc")]
     void DescUI(ConsoleSystem.Arg args);
    private void TopPlayerList(BasePlayer player, int page);
    private void PlayerTopInfo(BasePlayer player, ulong playerID);
    private void OnPlayerConnected(BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnCollectiblePickup(CollectibleEntity collectible, BasePlayer player);
    private void OnEntityTakeDamage(PatrolHelicopter entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnLootEntity(BasePlayer player, LootContainer entity);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void cmdOpenStats(BasePlayer player);
    [ConsoleCommand("UI_BSTATS")]
    private void StatsUIHandler(ConsoleSystem.Arg args);
    private readonly Regex Regex;
    private void GetAvatar(string userId, Action<string> callback);
    private void GetRandomTopPlayer();
    private void ServerBroadcast(BasePlayer player, string message, ulong AvatarID);
    public static string FormatShortTime(TimeSpan time);
    private bool IsTeammates(ulong player, ulong friend);
    private void TimeHandle();
    private int GetTopScore(ulong userid);
    private int FindItemID(string shortName);
    private void AddResourse(BasePlayer player, string shortname, int amount, bool GivePoint);
    private PluginConfig config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void UpdateConfigValues();
    protected override void SaveConfig();
    public class PointsSettings
    {
        [JsonProperty("Сколько давать очков за дерево")]
        public int pWood;
        [JsonProperty("Сколько давать очков за каменный камень")]
        public int pStone;
        [JsonProperty("Сколько давать очков за металический камень")]
        public int pMetal;
        [JsonProperty("Сколько давать очков за серный камень")]
        public int pSulfur;
        [JsonProperty("Сколько давать очков за уничтожение бочки | Лутание обычного ящика у дороги")]
        public int pBarrel;
    }

    public class PointsDestroy
    {
        [JsonProperty("Сколько давать очков за уничтожение вертолета")]
        public int dHeli;
        [JsonProperty("Сколько давать очков за уничтожение танка")]
        public int dBradley;
    }

    public class PointsKillDeath
    {
        [JsonProperty("Сколько давать очков за убийство игрока")]
        public int pKill;
        [JsonProperty("Сколько отнимать очков за смерть")]
        public int pDeath;
        [JsonProperty("Сколько отнимать очков за суицид")]
        public int pSuicide;
    }

    public class GameStoreSettings
    {
        [JsonProperty("Включить авто выдачу призов при вайпе сервера?")]
        public bool GivePrize;
        [JsonProperty("ИД магазина в сервисе")]
        public string ShopID;
        [JsonProperty("Секретный ключ (не распростраяйте его)")]
        public string SecretKey;
        [JsonProperty("Место в топе и выдаваемый баланс игроку")]
        public Dictionary<int, float> RewardSettings;
    }

    public class NotifyChatRandom
    {
        [JsonProperty("Отправлять в чат сообщения с топ 5 игроками ?")]
        public bool chatSendTop;
        [JsonProperty("Раз в сколько секунд будет отправлятся сообщение ?")]
        public int chatSendTopTime;
    }

    private class PluginConfig
    {
        [JsonProperty("Команда для открытия топа")]
        public string openMenuTop;
        [JsonProperty("Информация")]
        public string Info;
        [JsonProperty("Сколько выдавать очков за лутание мегаящика")]
        public int CountPoint;
        [JsonProperty("Сколько выдавать очков за захват карьера")]
        public int CountPointQuarry;
        [JsonProperty("Настройка начисления очков за добычу")]
        public PointsSettings _PointsSettings;
        [JsonProperty("Настройка начисления очков за уничтожение")]
        public PointsDestroy _PointsDestroy;
        [JsonProperty("Настройка начисления и отнимания очков за убийства и смерти")]
        public PointsKillDeath _PointsKillDeath;
        [JsonProperty("Настройка призов")]
        public GameStoreSettings _GameStoreSettings;
        [JsonProperty("Настройка оповещений в чате")]
        public NotifyChatRandom _NotifyChatRandom;
        [JsonProperty("Config version")]
        public VersionNumber PluginVersion;
        public static PluginConfig DefaultConfig();
    }

     int AddPoint(BasePlayer player);
     int AddPointQuarry(BasePlayer player);
     int PointQuarry();
}

public class playerData
{
    public string Name;
    public int Point;
    public int PlayTimeInServer;
    public int Kill;
    public int Death;
    public Dictionary<string, int> Gather;
    public int TotalFarm();
}

public class PointsSettings
{
    [JsonProperty("Сколько давать очков за дерево")]
    public int pWood;
    [JsonProperty("Сколько давать очков за каменный камень")]
    public int pStone;
    [JsonProperty("Сколько давать очков за металический камень")]
    public int pMetal;
    [JsonProperty("Сколько давать очков за серный камень")]
    public int pSulfur;
    [JsonProperty("Сколько давать очков за уничтожение бочки | Лутание обычного ящика у дороги")]
    public int pBarrel;
}

public class PointsDestroy
{
    [JsonProperty("Сколько давать очков за уничтожение вертолета")]
    public int dHeli;
    [JsonProperty("Сколько давать очков за уничтожение танка")]
    public int dBradley;
}

public class PointsKillDeath
{
    [JsonProperty("Сколько давать очков за убийство игрока")]
    public int pKill;
    [JsonProperty("Сколько отнимать очков за смерть")]
    public int pDeath;
    [JsonProperty("Сколько отнимать очков за суицид")]
    public int pSuicide;
}

public class GameStoreSettings
{
    [JsonProperty("Включить авто выдачу призов при вайпе сервера?")]
    public bool GivePrize;
    [JsonProperty("ИД магазина в сервисе")]
    public string ShopID;
    [JsonProperty("Секретный ключ (не распростраяйте его)")]
    public string SecretKey;
    [JsonProperty("Место в топе и выдаваемый баланс игроку")]
    public Dictionary<int, float> RewardSettings;
}

public class NotifyChatRandom
{
    [JsonProperty("Отправлять в чат сообщения с топ 5 игроками ?")]
    public bool chatSendTop;
    [JsonProperty("Раз в сколько секунд будет отправлятся сообщение ?")]
    public int chatSendTopTime;
}

private class PluginConfig
{
    [JsonProperty("Команда для открытия топа")]
    public string openMenuTop;
    [JsonProperty("Информация")]
    public string Info;
    [JsonProperty("Сколько выдавать очков за лутание мегаящика")]
    public int CountPoint;
    [JsonProperty("Сколько выдавать очков за захват карьера")]
    public int CountPointQuarry;
    [JsonProperty("Настройка начисления очков за добычу")]
    public PointsSettings _PointsSettings;
    [JsonProperty("Настройка начисления очков за уничтожение")]
    public PointsDestroy _PointsDestroy;
    [JsonProperty("Настройка начисления и отнимания очков за убийства и смерти")]
    public PointsKillDeath _PointsKillDeath;
    [JsonProperty("Настройка призов")]
    public GameStoreSettings _GameStoreSettings;
    [JsonProperty("Настройка оповещений в чате")]
    public NotifyChatRandom _NotifyChatRandom;
    [JsonProperty("Config version")]
    public VersionNumber PluginVersion;
    public static PluginConfig DefaultConfig();
}


```

---

## TPTeleportation

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("TPTeleportation", "Sempai#3239", "5.0.0")]
 class TPTeleportation : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin Clans;
    [PluginReference]
     Plugin Friends;
     Dictionary<ulong, Vector3> lastPositions;
     Dictionary<BasePlayer, int> spectatingPlayers;
    private const string Layer;
    private class ButtonEntry
    {
        public string Name;
        public string Sprite;
        public string Command;
        public string Color;
        public bool Close;
    }

     bool IsTeamate(BasePlayer player, ulong targetID);
     class TP
    {
        public BasePlayer Player;
        public BasePlayer Player2;
        public Vector3 pos;
        public bool EnabledShip;
        public int seconds;
        public bool TPL;
        public bool TownTp;
        public TP(BasePlayer player, Vector3 Pos, int Seconds, bool EnabledShip1, bool tpl, BasePlayer player2, bool townTp);
    }

     int homelimitDefault;
     bool homecupboardblock;
     Dictionary<string, int> homelimitPerms;
     int tpkdDefault;
     Dictionary<string, int> tpkdPerms;
     int tpkdhomeDefault;
     Dictionary<string, int> tpkdhomePerms;
     int teleportSecsDefault;
     int resetPendingTime;
     bool restrictCupboard;
     bool enabledTPR;
     bool homecupboard;
     bool adminsLogs;
     bool foundationOwner;
     bool foundationOwnerFC;
     bool restrictTPRCupboard;
     bool foundationEx;
     bool OutpostEnable;
     bool BanditEnable;
     string Info;
     bool wipedData;
     bool createSleepingBug;
     string EffectPrefab1;
     string EffectPrefab;
     bool EnabledShipTP;
     bool EnabledBallonTP;
     bool CancelTPMetabolism;
     bool CancelTPCold;
     bool CancelTPRadiation;
     bool FriendsEnabled;
     bool CancelTPWounded;
     bool EnabledTPLForPlayers;
     int TPLCooldown;
     int TplPedingTime;
     bool TPLAdmin;
     int DefaultTpToTown;
     int DefaultCooldownTpToTown;
     Dictionary<string, int> TpTownsCooldowns;
     Dictionary<string, int> TeleportSecsTownsPerms;
    static DynamicConfigFile config;
     Dictionary<string, int> teleportSecsPerms;
     void OnNewSave();
     void WipeData();
    protected override void LoadDefaultConfig();
     T GetConfig(string name, T defaultValue);
    public static void GetVariable(DynamicConfigFile config, string name, T value, T defaultValue);
    public static class PermissionService
    {
        public static Permission permission;
        public static bool HasPermission(ulong uid, string permissionName);
        public static void RegisterPermissions(Plugin owner, List<string> permissions);
    }

    public BasePlayer FindBasePlayer(string nameOrUserId);
     List<Vector3> OutpostSpawns;
     List<Vector3> BanditSpawns;
     void FindTowns();
    private readonly int groundLayer;
    private readonly int buildingMask;
     Dictionary<ulong, Dictionary<string, Vector3>> homes;
     List<TPList> tpsave;
     class TPList
    {
        public string Name;
        public Vector3 pos;
    }

     Dictionary<ulong, int> cooldownsTP;
     Dictionary<ulong, int> cooldownsHOME;
     Dictionary<ulong, int> cooldownsTowns;
     List<TP> tpQueue;
     List<TP> pendings;
     List<ulong> sethomeBlock;
    public List<BasePlayer> OpenTeleportMenu;
    private void DDrawMenu(BasePlayer player);
    [ConsoleCommand("tpdesc")]
     void DescUI(ConsoleSystem.Arg args);
    [ConsoleCommand("tp.cmd")]
     void PlayerCMD(ConsoleSystem.Arg args);
     Dictionary<string, Vector3> CheckGetHomes(BasePlayer player);
    public List<ulong> GetFriends(ulong playerid);
    private string GetGrid(Vector3 position, bool addVector);
    private string NumberToLetter(int num);
    private static string HexToCuiColor(string hex);
     void AddPlayerAutoTP(BasePlayer player);
    [ChatCommand("atp")]
     void cmdAutoTPA(BasePlayer player, string com, string[] args);
    [ConsoleCommand("sethome")]
     void cmdChatSetHome(ConsoleSystem.Arg arg);
    [ChatCommand("sethome")]
     void cmdChatSetHome(BasePlayer player, string command, string[] args);
    [ChatCommand("removehome")]
     void cmdChatRemoveHome(BasePlayer player, string command, string[] args);
    [ConsoleCommand("home")]
     void cmdHome(ConsoleSystem.Arg arg);
    [ConsoleCommand("tpa")]
     void cmdTpa(ConsoleSystem.Arg arg);
    [ChatCommand("homelist")]
    private void cmdHomeList(BasePlayer player, string command, string[] args);
    [ChatCommand("home")]
     void cmdChatHome(BasePlayer player, string command, string[] args);
    [ChatCommand("outpost")]
     void ChatOutpost(BasePlayer player);
     int GetKDToTpTown(ulong uid);
     int GetKDSecsToTpTown(ulong uid);
    [ChatCommand("banditcamp")]
     void ChatBanditcamp(BasePlayer player);
    [ChatCommand("tpr")]
     void cmdChatTpr(BasePlayer player, string command, string[] args);
    [ChatCommand("tpa")]
     void cmdChatTpa(BasePlayer player, string command, string[] args);
    [ChatCommand("tpc")]
     void cmdChatTpc(BasePlayer player, string command, string[] args);
     void SpectateFinish(BasePlayer player);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void ResetSpectate(IPlayer player);
     object OnPlayerSpectateEnd(BasePlayer player, string spectateFilter);
    [ChatCommand("tpl")]
     void cmdChattpGo(BasePlayer player, string command, string[] args);
    [ChatCommand("tpspec")]
     void cmdTPSpec(BasePlayer player, string command, string[] args);
    [ChatCommand("tp")]
    [Obsolete]
     void cmdTP(BasePlayer player, string command, string[] args);
    [ConsoleCommand("home.wipe")]
    private void CmdTest(ConsoleSystem.Arg arg);
    public string TimeToString(double time);
     void OnPlayerDisconnected(BasePlayer player);
     void OnServerInitialized();
     void Unload();
     void OnEntityBuilt(Planner planner, GameObject gameobject);
    [PluginReference]
     Plugin Duel;
     bool InDuel(BasePlayer player);
     void TeleportationTimerHandle();
     void SetTpSave(BasePlayer player, string name);
     void SetHome(BasePlayer player, string name);
    private bool CheckInsideInFoundation(Vector3 position);
     int GetKDHome(ulong uid);
     int GetKD(ulong uid);
     int GetHomeLimit(ulong uid);
     int GetTeleportTime(ulong uid);
     BaseEntity GetBuldings(Vector3 pos);
    private BaseEntity GetFoundation(Vector3 pos);
     SleepingBag GetSleepingBag(string name, Vector3 pos);
     void CreateSleepingBag(BasePlayer player, Vector3 pos, string name);
     Dictionary<string, Vector3> GetHomes(ulong uid);
    public void Teleport(BasePlayer player, BasePlayer target);
    public void Teleport(BasePlayer player, float x, float y, float z);
    public void Teleport(BasePlayer player, Vector3 position);
     DynamicConfigFile homesFile;
     DynamicConfigFile tpsaveFile;
    public Dictionary<ulong, AutoTPASettings> AutoTPA;
    public class AutoTPASettings
    {
        public bool Enabled;
        public Dictionary<string, ulong> PlayersList;
    }

     void LoadData();
     void SaveData();
    private string URLEncode(string input);
    [Obsolete]
     void SendVKLogs(string Message);
    private const string GUI_TPACCEPT;
     Dictionary<string, string> Messages;
}

private class ButtonEntry
{
    public string Name;
    public string Sprite;
    public string Command;
    public string Color;
    public bool Close;
}

 class TP
{
    public BasePlayer Player;
    public BasePlayer Player2;
    public Vector3 pos;
    public bool EnabledShip;
    public int seconds;
    public bool TPL;
    public bool TownTp;
    public TP(BasePlayer player, Vector3 Pos, int Seconds, bool EnabledShip1, bool tpl, BasePlayer player2, bool townTp);
}

public static class PermissionService
{
    public static Permission permission;
    public static bool HasPermission(ulong uid, string permissionName);
    public static void RegisterPermissions(Plugin owner, List<string> permissions);
}

 class TPList
{
    public string Name;
    public Vector3 pos;
}

public class AutoTPASettings
{
    public bool Enabled;
    public Dictionary<string, ulong> PlayersList;
}


```

---

## TPWipeBlock

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
[Info("TPWipeBlock", "Sempai#3239", "5.0.0")]
 class TPWipeBlock : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Duel;
    private Plugin ArenaTournament;
    private static ConfigData config;
    private string CONF_IgnorePermission;
    private class ConfigData
    {
        [JsonProperty("Настройки от мазепы")]
        public List<WeaponData> weaponDatas;
        [JsonProperty(PropertyName = "Начало отрисовки столбцов(требуется для центрирования)")]
        public float StartUI;
        [JsonProperty(PropertyName = "Информация плагина")]
        public string Info;
        [JsonProperty(PropertyName = "Блокировка предметов")]
        public Dictionary<int, List<string>> items;
    }

    public class WeaponData
    {
        [JsonProperty("Префаб оружия")]
        public string Prefab;
        [JsonProperty("Длительность перезарядки оружия")]
        public float ReloadInSeconds;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private object CanWearItem(PlayerInventory inventory, Item item);
    private object CanEquipItem(PlayerInventory inventory, Item item);
    private object OnWeaponReload(BaseProjectile weapon, BasePlayer player);
     object OnMagazineReload(BaseProjectile projectile, int desiredAmount, BasePlayer player);
    private WeaponData TryGetWeaponData(string prefab);
    private bool playerOnDuel(BasePlayer player);
    private void OnServerInitialized();
    private void MessBlockUi(BasePlayer player, string shortname);
    private const string Layer;
     void BlockUi(BasePlayer player);
    [ConsoleCommand("blockdesc")]
     void DescUI(ConsoleSystem.Arg args);
    private double IsBlocked(string shortName);
    private bool BlockTimeGui(string shortName);
    private double UnBlockTime(int amount);
    private double IsBlocked(ItemDefinition itemDefinition);
    static readonly DateTime epoch;
    static double CurrentTime();
    public static string FormatShortTime(TimeSpan time);
    private static string HexToRustFormat(string hex);
}

private class ConfigData
{
    [JsonProperty("Настройки от мазепы")]
    public List<WeaponData> weaponDatas;
    [JsonProperty(PropertyName = "Начало отрисовки столбцов(требуется для центрирования)")]
    public float StartUI;
    [JsonProperty(PropertyName = "Информация плагина")]
    public string Info;
    [JsonProperty(PropertyName = "Блокировка предметов")]
    public Dictionary<int, List<string>> items;
}

public class WeaponData
{
    [JsonProperty("Префаб оружия")]
    public string Prefab;
    [JsonProperty("Длительность перезарядки оружия")]
    public float ReloadInSeconds;
}


```

---

## TPWipeSchedule

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("TPWipeSchedule", "Sempai#3239", "3.0.0⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠⁠")]
public class TPWipeSchedule : RustPlugin
{
    private string Layer;
    [PluginReference]
     Plugin ImageLibrary;
    private Dictionary<int, string> DaysOfWeek;
    public List<DayClass> DaysList;
    public class DayClass
    {
        public int day;
        public string color;
        public Types types;
        public string description;
    }

    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty("Раз во сколько секунд обновлять календарь?")]
        public int Delay;
        [JsonProperty("Цвет дней в нынешнем месяце (изображение)")]
        public string ActiveImage;
        [JsonProperty("Настройка")]
        public Dictionary<int, WipeClass> wipe;
    }

    private class WipeClass
    {
        [JsonProperty("Тип")]
        public Types type;
        [JsonProperty("Изображение с цветом кнопки")]
        public string image;
        [JsonProperty("Описание")]
        public string description;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void CmdConsoleSchedule(ConsoleSystem.Arg args);
    public const string MenuLayer;
    public const string MenuItemsLayer;
    public const string MenuSubItemsLayer;
    public const string MenuContent;
     void BuildUI(BasePlayer player);
    private void CalculateTable();
    public string FirstUpper(string str);
}

public class DayClass
{
    public int day;
    public string color;
    public Types types;
    public string description;
}

private class ConfigData
{
    [JsonProperty("Раз во сколько секунд обновлять календарь?")]
    public int Delay;
    [JsonProperty("Цвет дней в нынешнем месяце (изображение)")]
    public string ActiveImage;
    [JsonProperty("Настройка")]
    public Dictionary<int, WipeClass> wipe;
}

private class WipeClass
{
    [JsonProperty("Тип")]
    public Types type;
    [JsonProperty("Изображение с цветом кнопки")]
    public string image;
    [JsonProperty("Описание")]
    public string description;
}


```

---

## Tracker

```csharp
using UnityEngine;
using System;
using System.Reflection;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("Tracker", "Wolfs Darker", "1.0.1", ResourceId = 1278)]
 class Tracker : RustPlugin
{
    static String invalid_name;
    static String multiple_players;
    public class TrackedItem
    {
        public String container_name;
        public String location;
        public int count;
    }

     List<TrackedItem> findPlayerItems(String item, int min_amount);
     List<TrackedItem> findChestItems(String item, int min_amount);
    [ConsoleCommand("trackitemcount")]
     void cmdTrackItemCount(ConsoleSystem.Arg arg);
    [ConsoleCommand("trackitem")]
     void consoleTrackItem(ConsoleSystem.Arg arg);
     bool itemExist(String item);
}

public class TrackedItem
{
    public String container_name;
    public String location;
    public int count;
}


```

---

## Trade

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Facepunch;
using Network;
using Network.Visibility;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("TradeBox", "OxideBro", "1.1.31")]
public class Trade : RustPlugin
{
    [PluginReference]
     Plugin Duel;
    public static readonly List<int> sizes;
    public Timer mytimer;
    private Dictionary<BasePlayer, DateTime> Cooldowns;
    private static Trade m_Instance;
    private Dictionary<string, TradeController> m_Boxes;
    private static Dictionary<string, ShopFront> boxes;
    private static Dictionary<string, List<BasePlayer>> players;
    private List<ulong> tradingPlayers;
    private Dictionary<BasePlayer, BasePlayer> pendings;
     bool getCupAuth;
     bool getCupSend;
     bool getFly;
     bool getSwim;
     bool getWound;
     int getTime;
    private double CooldownTrade;
    public int getInt;
    private void LoadDefaultConfig();
    private void GetConfig(string menu, string Key, T var);
    private void Loaded();
     void Unload();
    private void OnShopCompleteTrade(ShopFront shop);
    [ConsoleCommand("trade")]
     void cmdTrade(ConsoleSystem.Arg arg);
    public BasePlayer FindOnline(string nameOrUserId);
    [ChatCommand("trade")]
     void CmdChatTrade(BasePlayer player, string command, string[] args);
    private void RemovePending(BasePlayer player);
    private bool IsDuelPlayer(BasePlayer player);
     void OnPlayerLootEnd(PlayerLoot inventory);
    private static void DrawUI(BasePlayer a);
    private static void DestroyUI(BasePlayer a);
     void DrawUIPlayer(BasePlayer player);
     void DestroyUIPlayer(BasePlayer player);
    private static void cmdTradeButton(ConsoleSystem.Arg arg);
    [ConsoleCommand("tradebox.button")]
     void cmdTradeButton1(ConsoleSystem.Arg a);
    public static PluginTimers GetTimer();
    public static string Create(BasePlayer player1, BasePlayer player2, int getInt);
    static void SendEntity(BasePlayer a, BaseEntity b);
    public static T AddComponent(string a);
    public static void Destroy(string a);
    private void DropTrade(TradeController trade);
    private void OpenBox(BasePlayer player1, BasePlayer player2);
    private void OnTradeCanceled(string guid);
     object CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer);
    public static void StartLooting(string guid, BasePlayer player);
     void Reply(BasePlayer player, string langKey, object[] args);
     bool CanPlayerTrade(BasePlayer player);
     class TradeController : MonoBehaviour
    {
        public string guid;
        public ShopFront shop;
        public BasePlayer player1;
        public BasePlayer player2;
        public void Init(string guid, BasePlayer player1, BasePlayer player2);
        private void Awake();
    }

     Dictionary<string, string> Messages;
}

 class TradeController : MonoBehaviour
{
    public string guid;
    public ShopFront shop;
    public BasePlayer player1;
    public BasePlayer player2;
    public void Init(string guid, BasePlayer player1, BasePlayer player2);
    private void Awake();
}


```

---

## Trains

```csharp
using Rust;
using System;
using System.Linq;
using System.Text;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Facepunch;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Trains", "Colon Blow", "1.0.5")]
 class Trains : CovalencePlugin
{
    [PluginReference]
     Plugin TrainsE1;
    const string permAdmin;
    const string permConductor;
     List<ulong> playersInTrackMode;
     List<BaseEntity> trackEditMarkers;
    private void OnServerInitialized();
    private static PluginConfig config;
    private class PluginConfig
    {
        public TrainSettings trainSettings { get; set; }
        public class TrainSettings
        {
            [JsonProperty(PropertyName = "Data File Setting : Use Data File named : ")]
            public string saveInfoName { get; set; }
            [JsonProperty(PropertyName = "Global Setting - Enable Event Train ? ")]
            public bool enableEventTrain { get; set; }
            [JsonProperty(PropertyName = "Global Setting - Destroy things in trains way ?")]
            public bool damageOnCollision { get; set; }
            [JsonProperty(PropertyName = "Global Setting - Eject Sleepers from train ?")]
            public bool ejectSleepers { get; set; }
            [JsonProperty(PropertyName = "Global Setting - Trains speeds up and down depending on angle ? ")]
            public bool useAngleSpeed { get; set; }
            [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks waypoint number above waypoint when toggled (1k or more waypoints recommend 5 or less) ")]
            public float timeShowText { get; set; }
            [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks lines betweet waypoints (1k or more waypoints recommend 5 or less) ")]
            public float timeShowLines { get; set; }
            [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks stop number above stop when toggled ")]
            public float timeShowStops { get; set; }
            [JsonProperty(PropertyName = "Custom Prefab - Prefab string for Train Type 5 (default is rowboat) ")]
            public string customPrefabStr { get; set; }
        }

        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    static StoredData storedData;
     DynamicConfigFile dataFile;
    public class StoredData
    {
        public Dictionary<string, TrainTrackData> trainTrackData;
        public class TrainTrackData
        {
            public int trainType;
            public bool looping;
            public bool autospawn;
            public bool autostart;
            public float maxSpeed;
            public float stopWaitTime;
            public List<Vector3> trackMarkers;
            public List<Vector3> trainStops;
            public TrainTrackData();
        }

        public StoredData();
    }

    private void LoadDataFile();
    private void SaveData();
    private void OnServerSave();
    [Command("train")]
    private void cmdTrainHelp(IPlayer player, string command);
    [Command("trackmode")]
    private void cmdTrackEnable(IPlayer player, string command, string[] args);
    [Command("track.create")]
    private void cmdTrackCreate(IPlayer player, string command, string[] args);
    [Command("track.remove")]
    private void cmdTrackRemove(IPlayer player, string command, string[] args);
    [Command("track.edit")]
    private void cmdTrackEdit(IPlayer player, string command, string[] args);
    [Command("track.traintype")]
    private void cmdTrackTrainType(IPlayer player, string command, string[] args);
    [Command("track.trainloop")]
    private void cmdTrackTrainLoop(IPlayer player, string command, string[] args);
    [Command("track.autospawn")]
    private void cmdTrackAutoSpawn(IPlayer player, string command, string[] args);
    [Command("track.autostart")]
    private void cmdTrackAutoStart(IPlayer player, string command, string[] args);
    [Command("track.maxspeed")]
    private void cmdTrackMaxSpeed(IPlayer player, string command, string[] args);
    [Command("track.waittime")]
    private void cmdTrackWaitTime(IPlayer player, string command, string[] args);
    [Command("track.showlines")]
    private void cmdTrackShowLines(IPlayer player, string command, string[] args);
    [Command("track.showtext")]
    private void cmdTrackShowText(IPlayer player, string command, string[] args);
    [Command("track.showstops")]
    private void cmdTrackShowStops(IPlayer player, string command, string[] args);
    [Command("track.enablemiddle")]
    private void cmdTrackEnableMiddle(IPlayer player, string command, string[] args);
    [Command("track.mark")]
    private void cmdTrackMarkWaypoint(IPlayer player, string command, string[] args);
    [Command("track.markstop")]
    private void cmdTrackMarkStop(IPlayer player, string command, string[] args);
    [Command("track.deletelastmark")]
    private void cmdTrackDeleteLastMark(IPlayer player, string command, string[] args);
    [Command("track.deletelaststop")]
    private void cmdTrackDeleteLastStop(IPlayer player, string command, string[] args);
    [Command("track.eraseall")]
    private void cmdTrackEraseAll(IPlayer player, string command);
    [Command("train.setconfig")]
    private void cmdTrainsSetConfig(IPlayer player, string command, string[] args);
    [Command("train.spawn")]
    private void cmdTrainSpawnTrain(IPlayer player, string command, string[] args);
    [Command("train.spawnevent1")]
    private void cmdTrainSpawnTrainEvent1(IPlayer player, string command, string[] args);
    private void RefeshPlayerGUI(BasePlayer basePlayer);
    private void AddTrainEntityToTrack(string listname);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private object OnStructureRotate(BaseCombatEntity entity, BasePlayer player);
    private object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
    private object OnEntityGroundMissing(BaseEntity entity);
    private object OnSwitchToggle(ElectricSwitch electricSwitch, BasePlayer player);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity entity);
    private void OnPlayerInput(BasePlayer basePlayer, InputState input);
    private void RespawnAllTrains();
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    private static void DestroyAll();
     class TrainEntity : MonoBehaviour
    {
         Trains instance;
         BaseEntity railCart;
         BoxCollider boxcollider;
         Rigidbody rigidHitBody;
         int traintype;
         float steps;
         BaseEntity furnace1;
         BaseEntity furnace2;
         BaseEntity frontwall;
         BaseEntity frontdoor;
         BaseEntity midwall;
         BaseEntity rearwall;
         BaseEntity reardoor;
         BaseEntity frontsign;
         BaseEntity frontDoorWay;
         BaseEntity rearDoorWay;
         BaseEntity frontLowWall;
         BaseEntity rearLowWall;
         BaseEntity floor1;
         BaseEntity floor2;
         BaseEntity floor3;
         BaseEntity floor4;
         BaseEntity ceiling1;
         BaseEntity ceiling2;
         BaseEntity ceiling3;
         BaseEntity ceiling4;
         BaseEntity rightwall1;
         BaseEntity rightwall2;
         BaseEntity rightwall3;
         BaseEntity rigthwall4;
         BaseEntity leftwall1;
         BaseEntity leftwall2;
         BaseEntity leftwall3;
         BaseEntity leftwall4;
         BaseEntity wheel1;
         BaseEntity wheel2;
         BaseEntity wheel3;
         BaseEntity wheel4;
         BaseEntity wheel5;
         BaseEntity wheel6;
         BaseEntity wheel7;
         BaseEntity wheel8;
         BaseEntity lightright;
         BaseEntity lightleft;
         BaseEntity lightright2;
         BaseEntity lightleft2;
         BaseEntity logoleft1;
         BaseEntity logoleft2;
         BaseEntity logoright1;
         BaseEntity logoright2;
         BaseEntity chair1;
         BaseEntity chair2;
         BaseEntity chair3;
         BaseEntity chair4;
         BaseEntity chair5;
         BaseEntity chair6;
         BaseEntity seatback1;
         BaseEntity seatback2;
         BaseEntity seat1;
         BaseEntity seat2;
         BaseEntity rowBoat;
        public BaseEntity frontlock;
        public BaseEntity frontswitch;
         int counter;
        public bool npcmove;
         bool findnext;
         bool moveforward;
         bool findnextstop;
        public bool loopmovement;
         bool useAngleSpeed;
         bool spawncomplete;
         bool autoStartTrain;
         bool setGameObjectActive;
         float maxSpeed;
         float stopWait;
         string lockCodestr;
         string usingListNamed;
        public List<Vector3> movetolist;
        public List<Vector3> stoplist;
        public Vector3 movetopoint;
         Vector3 lastStopPos;
         Vector3 currentPos;
         Vector3 targetDir;
         Vector3 newDir;
         string floorprefab;
         string frameprefab;
         string wheelprefab;
         string wallprefab;
         string windowwallprefab;
         string wallframeprefab;
         string doorwayprefab;
         string solorpanelprefab;
         string batteryprefab;
         string lightprefab;
         string lowwallprefab;
         string simplelightprefab;
         string doorcontroller;
         string rugprefab;
         string refineryprefab;
         string garagedoorprefab;
         string chairprefab;
         string seatprefab;
         string customPrefab;
        private void Awake();
        public void SpawnTrain(string listname);
         BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, BaseEntity parent, ulong skinid);
        private void SpawnRailBase();
        private void SpawnCoasterCar();
        private void SpawnEngine();
        private void SpawnRailCart1();
        private void SpawnRailCart2();
        private void SpawnCustomPrefab();
        private void SpawnRefresh(BaseNetworkable entity1);
         void OnTriggerEnter(Collider col);
         void OnTriggerExit(Collider col);
        private void applyBlastDamage(BasePlayer player, float damageamount, float radius, Rust.DamageType damagetype, Vector3 location);
        private void FindNextBusStop();
        private Vector3 FindCoords();
        private void FixedUpdate();
        private IEnumerator RefreshTrain();
        private void OnDestroy();
    }

     class TrackEditGUI : MonoBehaviour
    {
         BasePlayer player;
         Trains instance;
        public string trackstring;
         int typeOfTrain;
         bool loopingTrain;
         bool autoSpawn;
         bool autoStart;
         float maxSpeed;
         float waitTime;
         int waypointCount;
         int stoppointCount;
         string redcolor;
         string greencolor;
         string orangecolor;
         string blackcolor;
         string bluecolor;
         Double guiMax;
         Double guiMin;
         Double guiIncrementor;
        public bool enableShowLines;
        public bool enableShowText;
        public bool enableShowStops;
        public bool enableMiddleMouse;
         bool debugShowCompleted;
         string buttonShowLinesColor;
         string buttonShowTextColor;
         string middleMouseColor;
         string buttonShowStopsColor;
         CuiElementContainer trackEditGUI;
         CuiElementContainer backgroundGUI;
         CuiElementContainer trackListCUI;
        public void Awake();
        public void RefreshTrackList(BasePlayer guiplayer, string trackname);
        private void AddGUIButton(CuiElementContainer container, string command, string bcolor, string anchmin, string anchmax, string buttontxt, string buttonname, int fontsize);
        private void AddBackground(BasePlayer player);
        private void AddEditGUI(BasePlayer player, string trackname);
        private void DestroyBackgroundGUI(BasePlayer player);
        public void AddListTracks(BasePlayer player);
        public void AddEditGui(BasePlayer player);
        private void DestroyListGUI(BasePlayer player);
        private void DestroyEditGUI(BasePlayer player);
        public IEnumerator ShowLines();
        public IEnumerator ShowText();
        public IEnumerator ShowStops();
        private void FixedUpdate();
        public void OnDestroy();
    }

}

private class PluginConfig
{
    public TrainSettings trainSettings { get; set; }
    public class TrainSettings
    {
        [JsonProperty(PropertyName = "Data File Setting : Use Data File named : ")]
        public string saveInfoName { get; set; }
        [JsonProperty(PropertyName = "Global Setting - Enable Event Train ? ")]
        public bool enableEventTrain { get; set; }
        [JsonProperty(PropertyName = "Global Setting - Destroy things in trains way ?")]
        public bool damageOnCollision { get; set; }
        [JsonProperty(PropertyName = "Global Setting - Eject Sleepers from train ?")]
        public bool ejectSleepers { get; set; }
        [JsonProperty(PropertyName = "Global Setting - Trains speeds up and down depending on angle ? ")]
        public bool useAngleSpeed { get; set; }
        [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks waypoint number above waypoint when toggled (1k or more waypoints recommend 5 or less) ")]
        public float timeShowText { get; set; }
        [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks lines betweet waypoints (1k or more waypoints recommend 5 or less) ")]
        public float timeShowLines { get; set; }
        [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks stop number above stop when toggled ")]
        public float timeShowStops { get; set; }
        [JsonProperty(PropertyName = "Custom Prefab - Prefab string for Train Type 5 (default is rowboat) ")]
        public string customPrefabStr { get; set; }
    }

    public static PluginConfig DefaultConfig();
}

public class TrainSettings
{
    [JsonProperty(PropertyName = "Data File Setting : Use Data File named : ")]
    public string saveInfoName { get; set; }
    [JsonProperty(PropertyName = "Global Setting - Enable Event Train ? ")]
    public bool enableEventTrain { get; set; }
    [JsonProperty(PropertyName = "Global Setting - Destroy things in trains way ?")]
    public bool damageOnCollision { get; set; }
    [JsonProperty(PropertyName = "Global Setting - Eject Sleepers from train ?")]
    public bool ejectSleepers { get; set; }
    [JsonProperty(PropertyName = "Global Setting - Trains speeds up and down depending on angle ? ")]
    public bool useAngleSpeed { get; set; }
    [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks waypoint number above waypoint when toggled (1k or more waypoints recommend 5 or less) ")]
    public float timeShowText { get; set; }
    [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks lines betweet waypoints (1k or more waypoints recommend 5 or less) ")]
    public float timeShowLines { get; set; }
    [JsonProperty(PropertyName = "Editor - seconds to show current selected tracks stop number above stop when toggled ")]
    public float timeShowStops { get; set; }
    [JsonProperty(PropertyName = "Custom Prefab - Prefab string for Train Type 5 (default is rowboat) ")]
    public string customPrefabStr { get; set; }
}

public class StoredData
{
    public Dictionary<string, TrainTrackData> trainTrackData;
    public class TrainTrackData
    {
        public int trainType;
        public bool looping;
        public bool autospawn;
        public bool autostart;
        public float maxSpeed;
        public float stopWaitTime;
        public List<Vector3> trackMarkers;
        public List<Vector3> trainStops;
        public TrainTrackData();
    }

    public StoredData();
}

public class TrainTrackData
{
    public int trainType;
    public bool looping;
    public bool autospawn;
    public bool autostart;
    public float maxSpeed;
    public float stopWaitTime;
    public List<Vector3> trackMarkers;
    public List<Vector3> trainStops;
    public TrainTrackData();
}

 class TrainEntity : MonoBehaviour
{
     Trains instance;
     BaseEntity railCart;
     BoxCollider boxcollider;
     Rigidbody rigidHitBody;
     int traintype;
     float steps;
     BaseEntity furnace1;
     BaseEntity furnace2;
     BaseEntity frontwall;
     BaseEntity frontdoor;
     BaseEntity midwall;
     BaseEntity rearwall;
     BaseEntity reardoor;
     BaseEntity frontsign;
     BaseEntity frontDoorWay;
     BaseEntity rearDoorWay;
     BaseEntity frontLowWall;
     BaseEntity rearLowWall;
     BaseEntity floor1;
     BaseEntity floor2;
     BaseEntity floor3;
     BaseEntity floor4;
     BaseEntity ceiling1;
     BaseEntity ceiling2;
     BaseEntity ceiling3;
     BaseEntity ceiling4;
     BaseEntity rightwall1;
     BaseEntity rightwall2;
     BaseEntity rightwall3;
     BaseEntity rigthwall4;
     BaseEntity leftwall1;
     BaseEntity leftwall2;
     BaseEntity leftwall3;
     BaseEntity leftwall4;
     BaseEntity wheel1;
     BaseEntity wheel2;
     BaseEntity wheel3;
     BaseEntity wheel4;
     BaseEntity wheel5;
     BaseEntity wheel6;
     BaseEntity wheel7;
     BaseEntity wheel8;
     BaseEntity lightright;
     BaseEntity lightleft;
     BaseEntity lightright2;
     BaseEntity lightleft2;
     BaseEntity logoleft1;
     BaseEntity logoleft2;
     BaseEntity logoright1;
     BaseEntity logoright2;
     BaseEntity chair1;
     BaseEntity chair2;
     BaseEntity chair3;
     BaseEntity chair4;
     BaseEntity chair5;
     BaseEntity chair6;
     BaseEntity seatback1;
     BaseEntity seatback2;
     BaseEntity seat1;
     BaseEntity seat2;
     BaseEntity rowBoat;
    public BaseEntity frontlock;
    public BaseEntity frontswitch;
     int counter;
    public bool npcmove;
     bool findnext;
     bool moveforward;
     bool findnextstop;
    public bool loopmovement;
     bool useAngleSpeed;
     bool spawncomplete;
     bool autoStartTrain;
     bool setGameObjectActive;
     float maxSpeed;
     float stopWait;
     string lockCodestr;
     string usingListNamed;
    public List<Vector3> movetolist;
    public List<Vector3> stoplist;
    public Vector3 movetopoint;
     Vector3 lastStopPos;
     Vector3 currentPos;
     Vector3 targetDir;
     Vector3 newDir;
     string floorprefab;
     string frameprefab;
     string wheelprefab;
     string wallprefab;
     string windowwallprefab;
     string wallframeprefab;
     string doorwayprefab;
     string solorpanelprefab;
     string batteryprefab;
     string lightprefab;
     string lowwallprefab;
     string simplelightprefab;
     string doorcontroller;
     string rugprefab;
     string refineryprefab;
     string garagedoorprefab;
     string chairprefab;
     string seatprefab;
     string customPrefab;
    private void Awake();
    public void SpawnTrain(string listname);
     BaseEntity SpawnPart(string prefab, BaseEntity entitypart, bool setactive, int eulangx, int eulangy, int eulangz, float locposx, float locposy, float locposz, BaseEntity parent, ulong skinid);
    private void SpawnRailBase();
    private void SpawnCoasterCar();
    private void SpawnEngine();
    private void SpawnRailCart1();
    private void SpawnRailCart2();
    private void SpawnCustomPrefab();
    private void SpawnRefresh(BaseNetworkable entity1);
     void OnTriggerEnter(Collider col);
     void OnTriggerExit(Collider col);
    private void applyBlastDamage(BasePlayer player, float damageamount, float radius, Rust.DamageType damagetype, Vector3 location);
    private void FindNextBusStop();
    private Vector3 FindCoords();
    private void FixedUpdate();
    private IEnumerator RefreshTrain();
    private void OnDestroy();
}

 class TrackEditGUI : MonoBehaviour
{
     BasePlayer player;
     Trains instance;
    public string trackstring;
     int typeOfTrain;
     bool loopingTrain;
     bool autoSpawn;
     bool autoStart;
     float maxSpeed;
     float waitTime;
     int waypointCount;
     int stoppointCount;
     string redcolor;
     string greencolor;
     string orangecolor;
     string blackcolor;
     string bluecolor;
     Double guiMax;
     Double guiMin;
     Double guiIncrementor;
    public bool enableShowLines;
    public bool enableShowText;
    public bool enableShowStops;
    public bool enableMiddleMouse;
     bool debugShowCompleted;
     string buttonShowLinesColor;
     string buttonShowTextColor;
     string middleMouseColor;
     string buttonShowStopsColor;
     CuiElementContainer trackEditGUI;
     CuiElementContainer backgroundGUI;
     CuiElementContainer trackListCUI;
    public void Awake();
    public void RefreshTrackList(BasePlayer guiplayer, string trackname);
    private void AddGUIButton(CuiElementContainer container, string command, string bcolor, string anchmin, string anchmax, string buttontxt, string buttonname, int fontsize);
    private void AddBackground(BasePlayer player);
    private void AddEditGUI(BasePlayer player, string trackname);
    private void DestroyBackgroundGUI(BasePlayer player);
    public void AddListTracks(BasePlayer player);
    public void AddEditGui(BasePlayer player);
    private void DestroyListGUI(BasePlayer player);
    private void DestroyEditGUI(BasePlayer player);
    public IEnumerator ShowLines();
    public IEnumerator ShowText();
    public IEnumerator ShowStops();
    private void FixedUpdate();
    public void OnDestroy();
}


```

---

## TrapFloors

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Rust;
using UnityEngine;
using IEnumerator = System.Collections.IEnumerator;

Oxide.Plugins
[Info("TrapFloors", "Cheeze", "1.2", ResourceId = 2038)]
[Description("Allows players to make trap floors that collapse on non cupboard authorised players")]
 class TrapFloors : RustPlugin
{
    static readonly DynamicConfigFile DataFile;
    static List<Floor> trapFloors;
    const string permAdmin;
     BaseEntity newTrapFloor;
     class Floor
    {
        public uint Id;
        public string Location;
        public ulong PlayerId;
        internal static Floor GetTrap(uint id);
    }

    protected override void LoadDefaultConfig();
     void Init();
     void OnServerInitialized();
     void LoadDefaultMessages();
     void OnServerSave();
     void Unloaded();
     string pName;
     class FloorTrap : MonoBehaviour
    {
         void Awake();
         void UpdateCollider();
         void OnTriggerEnter(Collider col);
         IEnumerator KillIt(BaseEntity ent);
    }

     void OnEntityDeath(BaseCombatEntity entity);
    [ChatCommand("trapfloor")]
     void cmdAddTrapFloor(BasePlayer player, string command, string[] args);
     void Remove(string args, BasePlayer player);
     bool HasPermission(BasePlayer player, string perm);
     string LangMsg(string key, string uid, object[] args);
    static void SaveData();
}

 class Floor
{
    public uint Id;
    public string Location;
    public ulong PlayerId;
    internal static Floor GetTrap(uint id);
}

 class FloorTrap : MonoBehaviour
{
     void Awake();
     void UpdateCollider();
     void OnTriggerEnter(Collider col);
     IEnumerator KillIt(BaseEntity ent);
}


```

---

## TrollTax

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using UnityEngine;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using System.Reflection;

Oxide.Plugins
[Info("TrollTax", "Absolut", "1.0.0", ResourceId = 000000)]
 class TrollTax : RustPlugin
{
    [PluginReference]
     Plugin LustyMap;
     string TitleColor;
     string MsgColor;
     TrollTaxData ttData;
    private DynamicConfigFile TTData;
    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, Coords> BoxPrep;
     void Loaded();
     void Unload();
     void OnServerInitialized();
     void OnEntityBuilt(Planner planner, GameObject gameobject);
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
     void OnPlantGather(PlantEntity Plant, Item item, BasePlayer player);
     void OnCollectiblePickup(Item item, BasePlayer player);
     void OnDispenserGather(ResourceDispenser Dispenser, BaseEntity entity, Item item);
    public bool isPayor(ulong ID);
    private List<StorageContainer> GetTaxContainer(ulong Payor);
    private string GetLang(string msg);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3);
    private string GetMSG(string message, string arg1, string arg2, string arg3);
    public void DestroyTaxPanel(BasePlayer player);
    private string PanelTax;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    }

    private Dictionary<string, string> UIColors;
    private void TaxBoxConfirmation(BasePlayer player);
    [ConsoleCommand("UI_SaveTaxBox")]
    private void cmdUI_SaveTaxBox(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DestroyTaxPanel")]
    private void cmdUI_DestroyTaxPanel(ConsoleSystem.Arg arg);
    private void SaveLoop();
    private void InfoLoop();
    private void SetBoxFullNotification(string ID);
     class TrollTaxData
    {
        public Dictionary<ulong, List<ulong>> TaxCollector;
        public Dictionary<ulong, Coords> TaxBox;
    }

     class Coords
    {
        public float x;
        public float y;
        public float z;
    }

     void SaveData();
     void LoadData();
    private ConfigData configData;
     class ConfigData
    {
        public double TaxRate { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer container, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
}

 class TrollTaxData
{
    public Dictionary<ulong, List<ulong>> TaxCollector;
    public Dictionary<ulong, Coords> TaxBox;
}

 class Coords
{
    public float x;
    public float y;
    public float z;
}

 class ConfigData
{
    public double TaxRate { get; set; }
}


```

---

## TsunHorse

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;
using System.Linq;
using System.Globalization;
using System.Text;
using Facepunch;

Oxide.Plugins
[Info("TsunHorse", "k1lly0u", "3.0.1", ResourceId = 0)]
 class TsunHorse : RustPlugin
{
    private StoredData storedData;
    private DynamicConfigFile data;
    private List<Controller> controllers;
    private static TsunHorse ins;
    const string chairPrefab;
    const string animalPrefab;
    private void Loaded();
    private void OnServerSave();
    private void Unload();
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityDismounted(BaseMountable mountable, BasePlayer player);
    private object CanAnimalAttack(BaseNpc entity, BasePlayer player);
    private object CanNPCEat(BaseNpc entity, BaseEntity target);
    private object OnNpcTarget(BaseNpc entity, BaseEntity target);
    private static double CurrentTime();
    private string FormatTime(double time);
    private bool SpawnAnimal(string type, Vector3 position, BaseNpc baseNpc);
    private BaseEntity InstantiateEntity(string type, Vector3 position);
    private bool FindPointOnNavmesh(Vector3 center, float range, Vector3 result);
    private class Controller : MonoBehaviour
    {
        public BasePlayer Player { get; set; }
        public AnimalController Animal { get; set; }
        public ConfigData.RidingSettings config;
        private float acceleration;
        private float steering;
        private bool sprint;
        private Vector3 targetPosition;
        private void Awake();
        private void BlockTargeting();
        public void FixedUpdate();
        public void LateUpdate();
        public void OnDestroy();
        public void MountToNpc(BaseNpc baseNpc);
        private void UpdatePosition();
    }

    private class AnimalController : MonoBehaviour
    {
        public BaseNpc Npc { get; set; }
        public BaseMountable Mountable { get; set; }
        public Controller Human { get; set; }
        public float TargetSpeed { get; set; }
        public ConfigData.SpeedSettings stats;
        private void Awake();
        public void CreateMountable(Controller controller);
        private void TickAi();
        private void TickSpeed();
        private void OnDestroy();
    }

    private Dictionary<string, KeyValuePair<Vector3, Vector3>> mountingPositions;
    [ChatCommand("spawnhorse")]
    private void cmdSpawn(BasePlayer player, string command, string[] args);
    [ChatCommand("stop")]
    private void cmdStop(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        public RidingSettings Settings { get; set; }
        public CommandSettings Command { get; set; }
        public Dictionary<string, SpeedSettings> Speeds { get; set; }
        public class RidingSettings
        {
            [JsonProperty(PropertyName = "Allowed animal types (Horse, Bear, Boar, Chicken, Stag, Wolf)")]
            public string[] AllowedTypes { get; set; }
            [JsonProperty(PropertyName = "Are animals invincible when being ridden")]
            public bool Invulnerable { get; set; }
            [JsonProperty(PropertyName = "Allow third person toggle (middle mouse button)")]
            public bool ThirdPersonHotkey { get; set; }
        }

        public class CommandSettings
        {
            [JsonProperty(PropertyName = "Stop command cooldown time (seconds)")]
            public int StopCooldown { get; set; }
            [JsonProperty(PropertyName = "Spawn command cooldown time (seconds)")]
            public int SpawnCooldown { get; set; }
            [JsonProperty(PropertyName = "Enable command to stop animals within radius")]
            public bool Command { get; set; }
            [JsonProperty(PropertyName = "Radius in which to stop nearby animals")]
            public int StopRadius { get; set; }
            [JsonProperty(PropertyName = "Are animals invincible whilst stopped")]
            public bool Invulnerable { get; set; }
            [JsonProperty(PropertyName = "Amount of time animals stay stopped")]
            public int StopTime { get; set; }
        }

        public class SpeedSettings
        {
            [JsonProperty(PropertyName = "Walking speed")]
            public float Walk;
            [JsonProperty(PropertyName = "Sprinting speed")]
            public float Sprint;
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
        public Dictionary<ulong, Cooldown> cooldowns;
        public class Cooldown
        {
            public double Stop { get; set; }
            public double Spawn { get; set; }
            public Cooldown();
            public Cooldown(double stop, double spawn);
            public double GetValue(bool isSpawn);
        }

    }

    private static string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

private class Controller : MonoBehaviour
{
    public BasePlayer Player { get; set; }
    public AnimalController Animal { get; set; }
    public ConfigData.RidingSettings config;
    private float acceleration;
    private float steering;
    private bool sprint;
    private Vector3 targetPosition;
    private void Awake();
    private void BlockTargeting();
    public void FixedUpdate();
    public void LateUpdate();
    public void OnDestroy();
    public void MountToNpc(BaseNpc baseNpc);
    private void UpdatePosition();
}

private class AnimalController : MonoBehaviour
{
    public BaseNpc Npc { get; set; }
    public BaseMountable Mountable { get; set; }
    public Controller Human { get; set; }
    public float TargetSpeed { get; set; }
    public ConfigData.SpeedSettings stats;
    private void Awake();
    public void CreateMountable(Controller controller);
    private void TickAi();
    private void TickSpeed();
    private void OnDestroy();
}

private class ConfigData
{
    public RidingSettings Settings { get; set; }
    public CommandSettings Command { get; set; }
    public Dictionary<string, SpeedSettings> Speeds { get; set; }
    public class RidingSettings
    {
        [JsonProperty(PropertyName = "Allowed animal types (Horse, Bear, Boar, Chicken, Stag, Wolf)")]
        public string[] AllowedTypes { get; set; }
        [JsonProperty(PropertyName = "Are animals invincible when being ridden")]
        public bool Invulnerable { get; set; }
        [JsonProperty(PropertyName = "Allow third person toggle (middle mouse button)")]
        public bool ThirdPersonHotkey { get; set; }
    }

    public class CommandSettings
    {
        [JsonProperty(PropertyName = "Stop command cooldown time (seconds)")]
        public int StopCooldown { get; set; }
        [JsonProperty(PropertyName = "Spawn command cooldown time (seconds)")]
        public int SpawnCooldown { get; set; }
        [JsonProperty(PropertyName = "Enable command to stop animals within radius")]
        public bool Command { get; set; }
        [JsonProperty(PropertyName = "Radius in which to stop nearby animals")]
        public int StopRadius { get; set; }
        [JsonProperty(PropertyName = "Are animals invincible whilst stopped")]
        public bool Invulnerable { get; set; }
        [JsonProperty(PropertyName = "Amount of time animals stay stopped")]
        public int StopTime { get; set; }
    }

    public class SpeedSettings
    {
        [JsonProperty(PropertyName = "Walking speed")]
        public float Walk;
        [JsonProperty(PropertyName = "Sprinting speed")]
        public float Sprint;
    }

    public Oxide.Core.VersionNumber Version { get; set; }
}

public class RidingSettings
{
    [JsonProperty(PropertyName = "Allowed animal types (Horse, Bear, Boar, Chicken, Stag, Wolf)")]
    public string[] AllowedTypes { get; set; }
    [JsonProperty(PropertyName = "Are animals invincible when being ridden")]
    public bool Invulnerable { get; set; }
    [JsonProperty(PropertyName = "Allow third person toggle (middle mouse button)")]
    public bool ThirdPersonHotkey { get; set; }
}

public class CommandSettings
{
    [JsonProperty(PropertyName = "Stop command cooldown time (seconds)")]
    public int StopCooldown { get; set; }
    [JsonProperty(PropertyName = "Spawn command cooldown time (seconds)")]
    public int SpawnCooldown { get; set; }
    [JsonProperty(PropertyName = "Enable command to stop animals within radius")]
    public bool Command { get; set; }
    [JsonProperty(PropertyName = "Radius in which to stop nearby animals")]
    public int StopRadius { get; set; }
    [JsonProperty(PropertyName = "Are animals invincible whilst stopped")]
    public bool Invulnerable { get; set; }
    [JsonProperty(PropertyName = "Amount of time animals stay stopped")]
    public int StopTime { get; set; }
}

public class SpeedSettings
{
    [JsonProperty(PropertyName = "Walking speed")]
    public float Walk;
    [JsonProperty(PropertyName = "Sprinting speed")]
    public float Sprint;
}

private class StoredData
{
    public Dictionary<ulong, Cooldown> cooldowns;
    public class Cooldown
    {
        public double Stop { get; set; }
        public double Spawn { get; set; }
        public Cooldown();
        public Cooldown(double stop, double spawn);
        public double GetValue(bool isSpawn);
    }

}

public class Cooldown
{
    public double Stop { get; set; }
    public double Spawn { get; set; }
    public Cooldown();
    public Cooldown(double stop, double spawn);
    public double GetValue(bool isSpawn);
}


```

---

## TsunHorse (1)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Collections;
using System.Text.RegularExpressions;
using System.Runtime.CompilerServices;
using UnityEngine;
using Rust;
using Newtonsoft.Json;
using Network;
using System.Reflection;
using Facepunch;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Tsunderellas horse plugin", "Tsunderella", "2.0.5")]
[Description("Adds Tsunderellas horses ")]
 class TsunHorse : RustPlugin
{
    public bool smoothanim;
    public bool thirdperson;
    public bool invul;
    public bool taming;
    public bool enablecommand;
    public string animals;
    public int sprintspeeds;
    public int walkspeeds;
    public int backspeeds;
    public int turnspeeds;
    public int stopcmdcd;
    public int stopradius;
    private const string StopHorses;
    private const string RideHorses;
     void init();
     void Unload();
     void Loaded();
    private bool Changed;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    [ChatCommand("stophorse")]
     void cmdHorse(BasePlayer player);
     void OnPlayerInput(BasePlayer player, InputState input);
     void OnEntityDismounted(BaseMountable bm, BasePlayer player);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     class Damageredirect : MonoBehaviour
    {
        public BasePlayer player;
    }

     class HiddenChair : MonoBehaviour
    {
        public BasePlayer player;
        public BaseNpc horse;
        public RHorse rhorse;
    }

     class CoolDownCMD : MonoBehaviour
    {
        private IEnumerator coroutine;
        public void Startcd(float time);
        private IEnumerator Coold(float waitTime);
         void Destroy();
    }

     class RHorse : MonoBehaviour
    {
        private BaseEntity entity;
        public BaseNpc horse;
        public BasePlayer player;
        public bool smooth;
        public float turnspeed;
        public float sprintspeed;
        public float walkspeed;
        public float backspeed;
        public bool taming;
        public bool shouldkill;
        public int fixv;
        public int updatev;
         float forward;
         float right;
         float sprint;
         float speed;
         float speedturn;
        public string movesets;
        public BaseEntity chair;
        public BaseMountable bm;
         bool mounted;
         bool thirdperson;
         void Awake();
        public void GrabConfig(bool st, float ts, float ss, float ws, bool t);
        public void delthis(bool kill);
        public void AddMount(BasePlayer bplayer);
        public void Destroy();
        public void Movement(string MoveSet);
         BaseEntity debug;
         void Start();
         void FixedUpdate();
         void Update();
    }

}

 class Damageredirect : MonoBehaviour
{
    public BasePlayer player;
}

 class HiddenChair : MonoBehaviour
{
    public BasePlayer player;
    public BaseNpc horse;
    public RHorse rhorse;
}

 class CoolDownCMD : MonoBehaviour
{
    private IEnumerator coroutine;
    public void Startcd(float time);
    private IEnumerator Coold(float waitTime);
     void Destroy();
}

 class RHorse : MonoBehaviour
{
    private BaseEntity entity;
    public BaseNpc horse;
    public BasePlayer player;
    public bool smooth;
    public float turnspeed;
    public float sprintspeed;
    public float walkspeed;
    public float backspeed;
    public bool taming;
    public bool shouldkill;
    public int fixv;
    public int updatev;
     float forward;
     float right;
     float sprint;
     float speed;
     float speedturn;
    public string movesets;
    public BaseEntity chair;
    public BaseMountable bm;
     bool mounted;
     bool thirdperson;
     void Awake();
    public void GrabConfig(bool st, float ts, float ss, float ws, bool t);
    public void delthis(bool kill);
    public void AddMount(BasePlayer bplayer);
    public void Destroy();
    public void Movement(string MoveSet);
     BaseEntity debug;
     void Start();
     void FixedUpdate();
     void Update();
}


```

---

## TurretConfig

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Reflection;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("TurretConfig", "Calytic", "1.0.1", ResourceId = 1418)]
[Description("Change turret damage, accuracy, bullet speed, health, targeting, etc")]
 class TurretConfig : RustPlugin
{
    private readonly string turretPrefab;
    private uint turretPrefabId;
     FieldInfo bulletDamageField;
     FieldInfo healthField;
     FieldInfo maxHealthField;
    private bool adminOverride;
    private List<object> animals;
    private bool animalOverride;
    private bool sleepOverride;
    private bool useGlobalDamageModifier;
    private float globalDamageModifier;
    private float defaultBulletDamage;
    private float defaultBulletSpeed;
    private string defaultAmmoType;
    private float defaultSightRange;
    private float defaultHealth;
    private float defaultAimCone;
    private Dictionary<string, object> bulletDamages;
    private Dictionary<string, object> bulletSpeeds;
    private Dictionary<string, object> ammoTypes;
    private Dictionary<string, object> sightRanges;
    private Dictionary<string, object> healths;
    private Dictionary<string, object> aimCones;
    private bool infiniteAmmo;
    [PluginReference]
     Plugin Vanish;
    [PluginReference]
     Plugin Skills;
     void Loaded();
    [ConsoleCommand("turrets.reload")]
     void ccTurretReload(ConsoleSystem.Arg arg);
    protected void LoadTurrets();
    protected override void LoadDefaultConfig();
     void LoadMessages();
    private List<object> GetPassiveAnimals();
    private Dictionary<string, object> GetDefaultBulletDamages();
    private Dictionary<string, object> GetDefaultBulletSpeeds();
    private Dictionary<string, object> GetDefaultSightRanges();
    private Dictionary<string, object> GetDefaultAmmoTypes();
    private Dictionary<string, object> GetDefaultHealth();
    private Dictionary<string, object> GetDefaultAimCones();
     void LoadData();
    protected void ReloadConfig();
     void OnLootEntity(BasePlayer looter, BaseEntity target);
    private void OnConsumableUse(Item item, int amount);
    private void CheckAmmo(AutoTurret turret);
    private object CanBeTargeted(BaseCombatEntity target, MonoBehaviour turret);
     void OnEntitySpawned(BaseNetworkable entity);
     float GetBulletDamage(string userID);
     float GetHealth(string userID);
     float GetBulletSpeed(string userID);
     float GetSightRange(string userID);
     float GetAimCone(string userID);
     string GetAmmoType(string userID);
    private void UpdateTurret(AutoTurret turret, bool justCreated);
     void OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
    private T GetConfig(string name, T defaultValue);
    private T GetConfig(string name, string name2, T defaultValue);
     string GetMsg(string key, string userID);
}


```

---

## TurretsExtended

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Turrets Extended", "supreme", "3.0.3")]
[Description("Allows players to toggle on/off the turrets/sam sites without the need of electricity")]
public class TurretsExtended : RustPlugin
{
    private readonly Vector3 _turretPos;
    private readonly Vector3 _samPos;
    private readonly Vector3 _npcPos;
    private const string SwitchPrefab;
    private void OnServerInitialized();
    private void Unload();
    private void Toggle(ContainerIOEntity entity);
    private void ToggleTurret(AutoTurret turret);
    private void ToggleSam(SamSite sam);
    private object CanPickupEntity(BasePlayer player, ElectricSwitch entity);
    private void OnEntityBuilt(Planner plan, GameObject go);
     void OnEntitySpawned(NPCAutoTurret entity);
    private object OnSwitchToggle(ElectricSwitch switchz, BasePlayer player);
    private void AddSwitchN(ContainerIOEntity entity);
    private void AddSwitch(ContainerIOEntity entity);
     object OnEntityTakeDamage(ElectricSwitch swtichz, HitInfo info);
    private void DestroyGroundWatch(ElectricSwitch entity);
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Enable Gametip message")]
        public bool gameTip;
        [JsonProperty(PropertyName = "Gametip message time")]
        public float gameTipTime;
        [JsonProperty(PropertyName = "Enable Chat message")]
        public bool chatMessage;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     string Lang(string key, string id, object[] args);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Enable Gametip message")]
    public bool gameTip;
    [JsonProperty(PropertyName = "Gametip message time")]
    public float gameTipTime;
    [JsonProperty(PropertyName = "Enable Chat message")]
    public bool chatMessage;
}


```

---

## TwigsDecay

```csharp
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Text;
using Oxide.Core;

Oxide.Plugins
[Info("TwigsDecay", "Wulf/lukespragg/Nogrod", "2.0.1", ResourceId = 857)]
 class TwigsDecay : RustPlugin
{
    private readonly FieldInfo entityListField;
    private readonly FieldInfo decayTimer;
    private readonly FieldInfo decayDelayTime;
    private readonly Dictionary<string, float> damage;
    private readonly Dictionary<BuildingGrade.Enum, float> damageGrade;
     int timespan;
     bool ignoreAlivePlayers;
     bool ignoreDecayTimer;
    private readonly HashSet<string> blocks;
    private readonly HashSet<ulong> activePlayers;
    private readonly List<string> texts;
    private readonly Dictionary<string, string> messages;
    protected override void LoadDefaultConfig();
     void Init();
     void OnServerInitialized();
     void OnTimer();
     void SendHelpText(BasePlayer player);
     string _(string text, Dictionary<string, string> replacements);
    private bool decay(BaseCombatEntity entity, float amount);
}


```

---

## UberTool

```csharp
using Facepunch;
using Network;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Game.Rust.Libraries.Covalence;
using ProtoBuf;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;
using BTN = BUTTON;

Oxide.Plugins
[Info("UberTool", "Skuli Dropek", "1.4.19", ResourceId = 78)]
[Description("The ultimative build'n'place solution without any borders or other known limits")]
internal class UberTool : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin ImageLibrary;
    private StrdDt playerPrefs;
    private class StrdDt
    {
        public Dictionary<ulong,
            Plyrnf> playerData;
        public StrdDt();
    }

    private class Plyrnf
    {
        public float SF;
        public int DBG;
        public Plyrnf();
    }

    private const string WIRE_EFFECT;
    private object CanUseWires(BasePlayer player);
    public class EPlanner : MonoBehaviour
    {
        private BasePlayer player;
        private InputState serverInput;
        private uint ctvtm;
        private Construction.Target target;
        private Construction.Target mvTrgt;
        private BaseEntity mvTrgtSnp;
        private Construction construction;
        private Construction mvCnstrctn;
        private Construction rayDefinition;
        private Vector3 rttnOffst;
        private Vector3 mvOffst;
        private string lstCrsshr;
        private string lstWrnng;
        private Planner plnnr;
        private bool isPlanner;
        private bool isHammering;
        internal bool isWireTool;
        internal bool isLightDeployer;
        private HeldEntity heldItem;
        private bool sRmvr;
        private bool isAnotherHeld;
        private Item ctvtmLnk;
        private bool sTpDplybl;
        private int dfltGrd;
        private uint lstPrfb;
        private bool initialized;
        private bool ctvTrgt;
        private bool isPlacing;
        private float tkDist;
        private RaycastHit rayHit;
        private BaseEntity rayEntity;
        private IPlayer rayEntityOwner;
        private string rayEntityName;
        private Vector3 lastAimAngles;
        private Socket_Base lastSocketBase;
        private Vector3 lastSocketPos;
        private BaseEntity lastSocketEntity;
        private Construction.Placement lastPlacement;
        private Ray lastRay;
        private bool plannerInfoStatus;
        private bool removerInfoStatus;
        private bool hammerInfoStatus;
        private bool lastSocketForce;
        private int cuiFontSize;
        private string cuiFontColor;
        private string fontType;
        private float lastPosRotUpdate;
        private void Awake();
        private void Start();
        private void Unequip();
        private void GetTool(object[] tool);
        private void CrtTls();
        private bool GetCurrentTool();
        private void CheckRemover();
        public void SetHeldItem(uint uid);
        private bool isWiring;
        public void TickUpdate(PlayerTick tick);
        private IOEntity source;
        private IOEntity.IOSlot sourceSlot;
        private void Update();
        private void DecayUpdate(BaseEntity entity, bool isAdding, bool isBuildingPrivilege, BaseEntity target);
        private void PlaceOnTarget();
        private void TrPlcTrgt();
        public object GtMvTrgt();
        private void DMvmntSnc(BaseEntity entity, bool isChild);
        public void DoTick();
        private void FndTrrnPlcmnt(Construction.Target t, Construction c, float maxDistance, bool isQuarry);
        public void SetBlockPrefab(uint p);
        public void OnDestroy();
        private void DoRm(bool remAl);
        private void CollRm(BaseEntity srcntt);
        private WaitForEndOfFrame wait;
        private IEnumerator DlyRm(List<BuildingBlock> bLst, List<DecayEntity> dLst, List<BuildingPrivlidge> pLst);
        private void DoPlacement();
        private void CheckPlacement(Construction.Target t, Construction c);
        private void ChkQrr(Construction c);
        public GameObject DoPlaG(Construction.Target p, Construction component);
        private BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
        private Construction.Placement CheckPlacement(Construction.Target t, Construction c);
        private bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
        public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
        private void DestroyInfo(UType uType);
        private void DoPlannerInfo();
        private void DoPlannerUpdate(PType pType, string infoMsg);
        private void DoRemoverInfo();
        private void DoRemoverUpdate(RType rType, string infoMsg, bool altMode);
        private void DoHammerInfo();
        private void DoHammerUpdate(HType hType, string infoMsg);
        private void BldMnUI(float factor);
        private float[] index2Degrees;
        private void DoCrosshair(string cColor, bool kill);
        private void DoWarning(string cColor, bool kill);
    }

    private object GetConfig(string menu, string datavalue, object defaultValue);
    private bool Changed;
    private static UberTool Instance;
    private string[] iconFileNames;
    private List<uint> constructionIds;
    private Dictionary<uint, string> prefabIdToImage;
    private Dictionary<ulong, bool> ctvUbrTls;
    private Dictionary<ulong, EPlanner> activeUberObjects;
    private List<Transform> entRemoval;
    private string varChatToggle;
    private string varCmdToggle;
    private string varChatScale;
    private string varCmdScale;
    private string pluginPrefix;
    private string prefixColor;
    private string prefixFormat;
    private string colorTextMsg;
    private float scaleFactorDef;
    private bool hideTips;
    private bool showPlannerInfo;
    private bool showRemoverInfo;
    private bool showHammerInfo;
    private static float panelPosX;
    private static float panelPosY;
    private string effectRemoveBlocks;
    private bool effectRemoveBlocksOn;
    private string effectPlacingBlocks;
    private bool effectPlacingBlocksOn;
    private bool effectFoundationPlacement;
    private bool effectPromotingBlock;
    private bool showGibsOnRemove;
    private float removeToolRange;
    private float hammerToolRange;
    private bool removeToolObjects;
    private bool enableFullRemove;
    private bool disableGroundMissingChecks;
    private bool overrideStabilityBuilding;
    private bool disableStabilityStartup;
    private bool enablePerimeterRepair;
    private float perimeterRepairRange;
    private bool checkExistingPlanner;
    private bool checkExistingRemover;
    private bool checkExistingHammer;
    private bool enableHammerTCInfo;
    private bool enableHammerCodelockInfo;
    private List<object> pseudoAdminPerms;
    private List<string> psdPrms;
    private string pluginUsagePerm;
    private bool enableIsAdminCheck;
    private bool setDeployableOwner;
    private List<object[]> playerTools;
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Loaded();
    private void Unload();
    private const string IMAGE_URL;
    private bool noImageLibrary;
    private void OnServerInitialized();
    private void UpdateHooks();
    private void GetIconIds();
    private string ToShortName(string name);
    private static Dictionary<CmdType,
        BTN> controlButtons;
    private T ParseType(string type);
    private bool sPsdAdmn(string id);
    private void OnUserPermissionGranted(string id, string perm);
    private void OnGroupPermissionGranted(string name, string perm);
    private void Stsr(BasePlayer player);
    private bool HasPermission(BasePlayer p);
    private void OnServerSave();
    private void SaveData();
    private bool _disableStabilityStartup;
    private void OnSaveLoad();
    private void OnPlayerConnected(BasePlayer p);
    private object CanBuild(Planner plan, Construction prefab, Construction.Target target);
    private void OnItemDeployed(Deployer d);
    private object OnReloadMagazine(BasePlayer p, BaseProjectile bP);
    private void OnLoseCondition(Item item, float amount);
    private void OnPlayerTick(BasePlayer p, PlayerTick msg, bool wasPlayerStalled);
    private void cmdPrefab(ConsoleSystem.Arg arg);
    private void cmdScale(ConsoleSystem.Arg arg);
    private void chatScale(BasePlayer player, string command, string[] args);
    private void cmdToggle(ConsoleSystem.Arg arg);
    private void chatToggle(BasePlayer p, string command, string[] args);
    private void TgglTls(BasePlayer p);
    private void OnStructureRepair(BaseCombatEntity bsntt, BasePlayer player);
    private string GetChatPrefix();
    private void SaveConf();
    private string ChatMsg(string str);
    private string LangMsg(string key, string id);
    public static Vector2 RotateByRadians(Vector2 center, Vector2 point, float angle, float factor);
    private static string GetAnchor(Vector2 m, float s, float f);
    private static CuiButton BuildButtonUI(string panelName, Vector2 p, int ct, float mi, float ma, string c, float f);
    private static CuiElement BuildIconUI(string panel, Vector2 center, string sprite, float min, float max, string color, float factor, bool b);
    private static CuiElement BuildRawIconUI(string panel, Vector2 center, string png, float min, float max, string color, float factor, bool b);
    private static CuiElement CustomIconUI(string pN, Vector2 p, string iN, float mi, float ma, string c, float f);
    private static CuiButton CustomButtonUI(string panelName, Vector2 p, string cmd, float mi, float ma, string c, float f);
    private static CuiElement CreateRawImage(string pN, Vector2 p, string iN, float mi, float ma, string c, float f);
    private static string r(string i);
    private object OnEntityGroundMissing(BaseEntity ent);
    private void ClearUp(Transform root);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object OnMessagePlayer(string message, BasePlayer player);
}

private class StrdDt
{
    public Dictionary<ulong,
            Plyrnf> playerData;
    public StrdDt();
}

private class Plyrnf
{
    public float SF;
    public int DBG;
    public Plyrnf();
}

public class EPlanner : MonoBehaviour
{
    private BasePlayer player;
    private InputState serverInput;
    private uint ctvtm;
    private Construction.Target target;
    private Construction.Target mvTrgt;
    private BaseEntity mvTrgtSnp;
    private Construction construction;
    private Construction mvCnstrctn;
    private Construction rayDefinition;
    private Vector3 rttnOffst;
    private Vector3 mvOffst;
    private string lstCrsshr;
    private string lstWrnng;
    private Planner plnnr;
    private bool isPlanner;
    private bool isHammering;
    internal bool isWireTool;
    internal bool isLightDeployer;
    private HeldEntity heldItem;
    private bool sRmvr;
    private bool isAnotherHeld;
    private Item ctvtmLnk;
    private bool sTpDplybl;
    private int dfltGrd;
    private uint lstPrfb;
    private bool initialized;
    private bool ctvTrgt;
    private bool isPlacing;
    private float tkDist;
    private RaycastHit rayHit;
    private BaseEntity rayEntity;
    private IPlayer rayEntityOwner;
    private string rayEntityName;
    private Vector3 lastAimAngles;
    private Socket_Base lastSocketBase;
    private Vector3 lastSocketPos;
    private BaseEntity lastSocketEntity;
    private Construction.Placement lastPlacement;
    private Ray lastRay;
    private bool plannerInfoStatus;
    private bool removerInfoStatus;
    private bool hammerInfoStatus;
    private bool lastSocketForce;
    private int cuiFontSize;
    private string cuiFontColor;
    private string fontType;
    private float lastPosRotUpdate;
    private void Awake();
    private void Start();
    private void Unequip();
    private void GetTool(object[] tool);
    private void CrtTls();
    private bool GetCurrentTool();
    private void CheckRemover();
    public void SetHeldItem(uint uid);
    private bool isWiring;
    public void TickUpdate(PlayerTick tick);
    private IOEntity source;
    private IOEntity.IOSlot sourceSlot;
    private void Update();
    private void DecayUpdate(BaseEntity entity, bool isAdding, bool isBuildingPrivilege, BaseEntity target);
    private void PlaceOnTarget();
    private void TrPlcTrgt();
    public object GtMvTrgt();
    private void DMvmntSnc(BaseEntity entity, bool isChild);
    public void DoTick();
    private void FndTrrnPlcmnt(Construction.Target t, Construction c, float maxDistance, bool isQuarry);
    public void SetBlockPrefab(uint p);
    public void OnDestroy();
    private void DoRm(bool remAl);
    private void CollRm(BaseEntity srcntt);
    private WaitForEndOfFrame wait;
    private IEnumerator DlyRm(List<BuildingBlock> bLst, List<DecayEntity> dLst, List<BuildingPrivlidge> pLst);
    private void DoPlacement();
    private void CheckPlacement(Construction.Target t, Construction c);
    private void ChkQrr(Construction c);
    public GameObject DoPlaG(Construction.Target p, Construction component);
    private BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
    private Construction.Placement CheckPlacement(Construction.Target t, Construction c);
    private bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
    public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
    private void DestroyInfo(UType uType);
    private void DoPlannerInfo();
    private void DoPlannerUpdate(PType pType, string infoMsg);
    private void DoRemoverInfo();
    private void DoRemoverUpdate(RType rType, string infoMsg, bool altMode);
    private void DoHammerInfo();
    private void DoHammerUpdate(HType hType, string infoMsg);
    private void BldMnUI(float factor);
    private float[] index2Degrees;
    private void DoCrosshair(string cColor, bool kill);
    private void DoWarning(string cColor, bool kill);
}


```

---

## UberTool (1)

```csharp
using Facepunch;
using Network;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Game.Rust.Libraries.Covalence;
using ProtoBuf;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;
using BTN = BUTTON;

Oxide.Plugins
[Info("UberTool", "Skuli Dropek", "1.4.19", ResourceId = 78)]
[Description("The ultimative build'n'place solution without any borders or other known limits")]
internal class UberTool : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin ImageLibrary;
    private StrdDt playerPrefs;
    private class StrdDt
    {
        public Dictionary<ulong,
            Plyrnf> playerData;
        public StrdDt();
    }

    private class Plyrnf
    {
        public float SF;
        public int DBG;
        public Plyrnf();
    }

    private const string WIRE_EFFECT;
    private object CanUseWires(BasePlayer player);
    public class EPlanner : MonoBehaviour
    {
        private BasePlayer player;
        private InputState serverInput;
        private uint ctvtm;
        private Construction.Target target;
        private Construction.Target mvTrgt;
        private BaseEntity mvTrgtSnp;
        private Construction construction;
        private Construction mvCnstrctn;
        private Construction rayDefinition;
        private Vector3 rttnOffst;
        private Vector3 mvOffst;
        private string lstCrsshr;
        private string lstWrnng;
        private Planner plnnr;
        private bool isPlanner;
        private bool isHammering;
        internal bool isWireTool;
        internal bool isLightDeployer;
        private HeldEntity heldItem;
        private bool sRmvr;
        private bool isAnotherHeld;
        private Item ctvtmLnk;
        private bool sTpDplybl;
        private int dfltGrd;
        private uint lstPrfb;
        private bool initialized;
        private bool ctvTrgt;
        private bool isPlacing;
        private float tkDist;
        private RaycastHit rayHit;
        private BaseEntity rayEntity;
        private IPlayer rayEntityOwner;
        private string rayEntityName;
        private Vector3 lastAimAngles;
        private Socket_Base lastSocketBase;
        private Vector3 lastSocketPos;
        private BaseEntity lastSocketEntity;
        private Construction.Placement lastPlacement;
        private Ray lastRay;
        private bool plannerInfoStatus;
        private bool removerInfoStatus;
        private bool hammerInfoStatus;
        private bool lastSocketForce;
        private int cuiFontSize;
        private string cuiFontColor;
        private string fontType;
        private float lastPosRotUpdate;
        private void Awake();
        private void Start();
        private void Unequip();
        private void GetTool(object[] tool);
        private void CrtTls();
        private bool GetCurrentTool();
        private void CheckRemover();
        public void SetHeldItem(uint uid);
        private bool isWiring;
        public void TickUpdate(PlayerTick tick);
        private IOEntity source;
        private IOEntity.IOSlot sourceSlot;
        private void Update();
        private void DecayUpdate(BaseEntity entity, bool isAdding, bool isBuildingPrivilege, BaseEntity target);
        private void PlaceOnTarget();
        private void TrPlcTrgt();
        public object GtMvTrgt();
        private void DMvmntSnc(BaseEntity entity, bool isChild);
        public void DoTick();
        private void FndTrrnPlcmnt(Construction.Target t, Construction c, float maxDistance, bool isQuarry);
        public void SetBlockPrefab(uint p);
        public void OnDestroy();
        private void DoRm(bool remAl);
        private void CollRm(BaseEntity srcntt);
        private WaitForEndOfFrame wait;
        private IEnumerator DlyRm(List<BuildingBlock> bLst, List<DecayEntity> dLst, List<BuildingPrivlidge> pLst);
        private void DoPlacement();
        private void CheckPlacement(Construction.Target t, Construction c);
        private void ChkQrr(Construction c);
        public GameObject DoPlaG(Construction.Target p, Construction component);
        private BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
        private Construction.Placement CheckPlacement(Construction.Target t, Construction c);
        private bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
        public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
        private void DestroyInfo(UType uType);
        private void DoPlannerInfo();
        private void DoPlannerUpdate(PType pType, string infoMsg);
        private void DoRemoverInfo();
        private void DoRemoverUpdate(RType rType, string infoMsg, bool altMode);
        private void DoHammerInfo();
        private void DoHammerUpdate(HType hType, string infoMsg);
        private void BldMnUI(float factor);
        private float[] index2Degrees;
        private void DoCrosshair(string cColor, bool kill);
        private void DoWarning(string cColor, bool kill);
    }

    private object GetConfig(string menu, string datavalue, object defaultValue);
    private bool Changed;
    private static UberTool Instance;
    private string[] iconFileNames;
    private List<uint> constructionIds;
    private Dictionary<uint, string> prefabIdToImage;
    private Dictionary<ulong, bool> ctvUbrTls;
    private Dictionary<ulong, EPlanner> activeUberObjects;
    private List<Transform> entRemoval;
    private string varChatToggle;
    private string varCmdToggle;
    private string varChatScale;
    private string varCmdScale;
    private string pluginPrefix;
    private string prefixColor;
    private string prefixFormat;
    private string colorTextMsg;
    private float scaleFactorDef;
    private bool hideTips;
    private bool showPlannerInfo;
    private bool showRemoverInfo;
    private bool showHammerInfo;
    private static float panelPosX;
    private static float panelPosY;
    private string effectRemoveBlocks;
    private bool effectRemoveBlocksOn;
    private string effectPlacingBlocks;
    private bool effectPlacingBlocksOn;
    private bool effectFoundationPlacement;
    private bool effectPromotingBlock;
    private bool showGibsOnRemove;
    private float removeToolRange;
    private float hammerToolRange;
    private bool removeToolObjects;
    private bool enableFullRemove;
    private bool disableGroundMissingChecks;
    private bool overrideStabilityBuilding;
    private bool disableStabilityStartup;
    private bool enablePerimeterRepair;
    private float perimeterRepairRange;
    private bool checkExistingPlanner;
    private bool checkExistingRemover;
    private bool checkExistingHammer;
    private bool enableHammerTCInfo;
    private bool enableHammerCodelockInfo;
    private List<object> pseudoAdminPerms;
    private List<string> psdPrms;
    private string pluginUsagePerm;
    private bool enableIsAdminCheck;
    private bool setDeployableOwner;
    private List<object[]> playerTools;
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Loaded();
    private void Unload();
    private const string IMAGE_URL;
    private bool noImageLibrary;
    private void OnServerInitialized();
    private void UpdateHooks();
    private void GetIconIds();
    private string ToShortName(string name);
    private static Dictionary<CmdType,
        BTN> controlButtons;
    private T ParseType(string type);
    private bool sPsdAdmn(string id);
    private void OnUserPermissionGranted(string id, string perm);
    private void OnGroupPermissionGranted(string name, string perm);
    private void Stsr(BasePlayer player);
    private bool HasPermission(BasePlayer p);
    private void OnServerSave();
    private void SaveData();
    private bool _disableStabilityStartup;
    private void OnSaveLoad();
    private void OnPlayerConnected(BasePlayer p);
    private object CanBuild(Planner plan, Construction prefab, Construction.Target target);
    private void OnItemDeployed(Deployer d);
    private object OnReloadMagazine(BasePlayer p, BaseProjectile bP);
    private void OnLoseCondition(Item item, float amount);
    private void OnPlayerTick(BasePlayer p, PlayerTick msg, bool wasPlayerStalled);
    private void cmdPrefab(ConsoleSystem.Arg arg);
    private void cmdScale(ConsoleSystem.Arg arg);
    private void chatScale(BasePlayer player, string command, string[] args);
    private void cmdToggle(ConsoleSystem.Arg arg);
    private void chatToggle(BasePlayer p, string command, string[] args);
    private void TgglTls(BasePlayer p);
    private void OnStructureRepair(BaseCombatEntity bsntt, BasePlayer player);
    private string GetChatPrefix();
    private void SaveConf();
    private string ChatMsg(string str);
    private string LangMsg(string key, string id);
    public static Vector2 RotateByRadians(Vector2 center, Vector2 point, float angle, float factor);
    private static string GetAnchor(Vector2 m, float s, float f);
    private static CuiButton BuildButtonUI(string panelName, Vector2 p, int ct, float mi, float ma, string c, float f);
    private static CuiElement BuildIconUI(string panel, Vector2 center, string sprite, float min, float max, string color, float factor, bool b);
    private static CuiElement BuildRawIconUI(string panel, Vector2 center, string png, float min, float max, string color, float factor, bool b);
    private static CuiElement CustomIconUI(string pN, Vector2 p, string iN, float mi, float ma, string c, float f);
    private static CuiButton CustomButtonUI(string panelName, Vector2 p, string cmd, float mi, float ma, string c, float f);
    private static CuiElement CreateRawImage(string pN, Vector2 p, string iN, float mi, float ma, string c, float f);
    private static string r(string i);
    private object OnEntityGroundMissing(BaseEntity ent);
    private void ClearUp(Transform root);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object OnMessagePlayer(string message, BasePlayer player);
}

private class StrdDt
{
    public Dictionary<ulong,
            Plyrnf> playerData;
    public StrdDt();
}

private class Plyrnf
{
    public float SF;
    public int DBG;
    public Plyrnf();
}

public class EPlanner : MonoBehaviour
{
    private BasePlayer player;
    private InputState serverInput;
    private uint ctvtm;
    private Construction.Target target;
    private Construction.Target mvTrgt;
    private BaseEntity mvTrgtSnp;
    private Construction construction;
    private Construction mvCnstrctn;
    private Construction rayDefinition;
    private Vector3 rttnOffst;
    private Vector3 mvOffst;
    private string lstCrsshr;
    private string lstWrnng;
    private Planner plnnr;
    private bool isPlanner;
    private bool isHammering;
    internal bool isWireTool;
    internal bool isLightDeployer;
    private HeldEntity heldItem;
    private bool sRmvr;
    private bool isAnotherHeld;
    private Item ctvtmLnk;
    private bool sTpDplybl;
    private int dfltGrd;
    private uint lstPrfb;
    private bool initialized;
    private bool ctvTrgt;
    private bool isPlacing;
    private float tkDist;
    private RaycastHit rayHit;
    private BaseEntity rayEntity;
    private IPlayer rayEntityOwner;
    private string rayEntityName;
    private Vector3 lastAimAngles;
    private Socket_Base lastSocketBase;
    private Vector3 lastSocketPos;
    private BaseEntity lastSocketEntity;
    private Construction.Placement lastPlacement;
    private Ray lastRay;
    private bool plannerInfoStatus;
    private bool removerInfoStatus;
    private bool hammerInfoStatus;
    private bool lastSocketForce;
    private int cuiFontSize;
    private string cuiFontColor;
    private string fontType;
    private float lastPosRotUpdate;
    private void Awake();
    private void Start();
    private void Unequip();
    private void GetTool(object[] tool);
    private void CrtTls();
    private bool GetCurrentTool();
    private void CheckRemover();
    public void SetHeldItem(uint uid);
    private bool isWiring;
    public void TickUpdate(PlayerTick tick);
    private IOEntity source;
    private IOEntity.IOSlot sourceSlot;
    private void Update();
    private void DecayUpdate(BaseEntity entity, bool isAdding, bool isBuildingPrivilege, BaseEntity target);
    private void PlaceOnTarget();
    private void TrPlcTrgt();
    public object GtMvTrgt();
    private void DMvmntSnc(BaseEntity entity, bool isChild);
    public void DoTick();
    private void FndTrrnPlcmnt(Construction.Target t, Construction c, float maxDistance, bool isQuarry);
    public void SetBlockPrefab(uint p);
    public void OnDestroy();
    private void DoRm(bool remAl);
    private void CollRm(BaseEntity srcntt);
    private WaitForEndOfFrame wait;
    private IEnumerator DlyRm(List<BuildingBlock> bLst, List<DecayEntity> dLst, List<BuildingPrivlidge> pLst);
    private void DoPlacement();
    private void CheckPlacement(Construction.Target t, Construction c);
    private void ChkQrr(Construction c);
    public GameObject DoPlaG(Construction.Target p, Construction component);
    private BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
    private Construction.Placement CheckPlacement(Construction.Target t, Construction c);
    private bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
    public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
    private void DestroyInfo(UType uType);
    private void DoPlannerInfo();
    private void DoPlannerUpdate(PType pType, string infoMsg);
    private void DoRemoverInfo();
    private void DoRemoverUpdate(RType rType, string infoMsg, bool altMode);
    private void DoHammerInfo();
    private void DoHammerUpdate(HType hType, string infoMsg);
    private void BldMnUI(float factor);
    private float[] index2Degrees;
    private void DoCrosshair(string cColor, bool kill);
    private void DoWarning(string cColor, bool kill);
}


```

---

## UberTool (2)

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using System.Globalization;
using System.Text;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using Network;
using Facepunch;
using System.Text.RegularExpressions;
using ProtoBuf;
using Oxide.Game.Rust.Cui;
using static System.Math;
using Newtonsoft.Json;
using BTN = BUTTON;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Libraries.Covalence;

Oxide.Plugins
[Info("UberTool", "FuJiCuRa", "1.4.0", ResourceId = 78)]
 class UberTool : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     StrdDt plyrPrfs;
     class StrdDt
    {
        public Dictionary<ulong, Plyrnf> PlyData;
        public StrdDt();
    }

     class Plyrnf
    {
        public float SF;
        public int DBG;
        public Plyrnf();
    }

    public class EPlanner : MonoBehaviour
    {
         BasePlayer player;
         InputState stt;
         uint ctvtm;
         Construction.Target target;
         Construction.Target mvTrgt;
         BaseEntity mvTrgtSnp;
         Construction construction;
         Construction mvCnstrctn;
         Construction ryDfntn;
         Vector3 rttnOffst;
         Vector3 mvOffst;
         string lstCrsshr;
         string lstWrnng;
         Planner plnnr;
         bool sPlnnr;
         bool sHmmr;
         bool isWireTool;
         HeldEntity hldItm;
         bool sRmvr;
         bool isAnotherHeld;
         Item ctvtmLnk;
         bool sTpDplybl;
         int dfltGrd;
         uint lstPrfb;
         bool initialized;
         bool ctvTrgt;
         bool sPlcng;
         float tkDist;
         RaycastHit rayHit;
         BaseEntity rayEntity;
         IPlayer rayEntityOwner;
         string rayEntityName;
         Vector3 lastAimAngles;
         Socket_Base lastSocketBase;
         Vector3 lastSocketPos;
         BaseEntity lastSocketEntity;
         Construction.Placement lastPlacement;
         Ray lastRay;
         bool plannerInfoStatus;
         bool removerInfoStatus;
         bool hammerInfoStatus;
         bool lastSocketForce;
         int cuiFontSize;
         string cuiFontColor;
         string fontType;
         float lastPosRotUpdate;
         void Awake();
         void Start();
         void UnEqp();
         void GetTool(object[] tool);
         void CrtTls();
         bool GtCrrntTl();
         void ChkRmvr();
        public void StHldItm(uint uid);
        public void TckUpd(PlayerTick tick);
         void Update();
         void DecayUpdate(BaseEntity entity, bool isAdding, bool isBuildingPrivilege, BaseEntity target);
         void PlcOnTrgt();
         void TrPlcTrgt();
        public object GtMvTrgt();
         void DMvmntSnc(BaseEntity entity, bool isChild);
        public void DoTi();
         void FndTrrnPlcmnt(Construction.Target t, Construction c, float maxDistance, bool isQuarry);
        public void StBlckPrfb(uint p);
        public void OnDestroy();
         void DoRm(bool remAl);
         void CollRm(BaseEntity srcntt);
         WaitForEndOfFrame wait;
         IEnumerator DlyRm(List<BuildingBlock> bLst, List<DecayEntity> dLst, List<BuildingPrivlidge> pLst);
         void DoPl();
         void ChkPlcmnt(Construction.Target t, Construction c);
         void ChkQrr(Construction c);
        public GameObject DoPlaG(Construction.Target p, Construction component);
         BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
         Construction.Placement ChckPlcmnt(Construction.Target t, Construction c);
         bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
        public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
         void DestroyInfo(UType uType);
         void DoPlannerInfo();
         void DPlnnrUpdt(PType pType, string infoMsg);
         void DoRemoverInfo();
         void DRmvrUpdt(RType rType, string infoMsg, bool altMode);
         void DoHammerInfo();
         void DHmmrUpdt(HType hType, string infoMsg);
         void BldMnUI(float factor);
         void DoCrosshair(string cColor, bool kill);
         void DoWarning(string cColor, bool kill);
    }

     object GetConfig(string menu, string datavalue, object defaultValue);
     bool Changed;
    static UberTool uTs;
     uint[] PreMenArr;
     string[] PreMenIcons;
     Dictionary<ulong, bool> ctvUbrTls;
     Dictionary<ulong, EPlanner> ctvUbrObjcts;
     List<Transform> entRemoval;
     string varChatToggle;
     string varCmdToggle;
     string varChatScale;
     string varCmdScale;
     string pluginPrefix;
     string prefixColor;
     string prefixFormat;
     string colorTextMsg;
     float scaleFactorDef;
     bool hideTips;
     bool showPlannerInfo;
     bool showRemoverInfo;
     bool showHammerInfo;
    static float panelPosX;
    static float panelPosY;
     string ffctRmvngBlcks;
     bool ffctRmvngBlcksOn;
     string ffctPlcngBlcks;
     bool ffctPlcngBlcksOn;
     bool adEffctFndtnsPlcmnt;
     bool ffctPrmtngBlcksOn;
     bool gbsRmvBldng;
     float rmvrTlDstnc;
     float hmmrTlDstnc;
     bool rmvrTlbjcts;
     bool enblFllBldngRmvl;
     bool dsblDplyblGrndChcks;
     bool vrrdStbltWhlBld;
     bool vrrdStbltWhlStrtp;
     bool nblPrmtrRpr;
     float prmtrRprRng;
     bool crtTlPlnnr;
     bool crtTlRmvr;
     bool crtTlHmmr;
     bool nblHmmrTcInf;
     bool nblHmmrLckInf;
     List<object> psdAdmnPrms;
     List<string> psdPrms;
     string pluginUsagePerm;
     bool enblIsAdmnChck;
     List<object[]> playerTools;
     void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     void Loaded();
     void Unload();
     void OnServerInitialized();
    static private Dictionary<CmdType, BTN> controlButtons;
    private T ParseType(string type);
     bool sPsdAdmn(string id);
     void OnUserPermissionGranted(string id, string perm);
     void OnGroupPermissionGranted(string name, string perm);
     void Stsr(BasePlayer player);
     bool HsRghts(BasePlayer p);
     void OnServerSave();
     void SaveData();
     bool _vrrdStbltWhlStrtp_;
     void OnSaveLoad();
     void OnPlayerInit(BasePlayer p);
     object CanBuild(Planner plan, Construction prefab, Construction.Target target);
     void OnItemDeployed(Deployer d);
     object OnReloadMagazine(BasePlayer p, BaseProjectile bP);
     void OnLoseCondition(Item item, float amount);
     void OnPlayerTick(BasePlayer p, PlayerTick msg, bool wasPlayerStalled);
     void cmdPrefab(ConsoleSystem.Arg arg);
     void cmdScale(ConsoleSystem.Arg arg);
     void chatScale(BasePlayer player, string command, string[] args);
     void cmdToggle(ConsoleSystem.Arg arg);
     void chatToggle(BasePlayer p, string command, string[] args);
     void TgglTls(BasePlayer p);
     void OnStructureRepair(BaseCombatEntity bsntt, BasePlayer player);
     string GetChatPrefix();
     void SaveConf();
     string ChatMsg(string str);
     string LangMsg(string key, string id);
    public static Vector2 RttByRdns(Vector2 c, Vector2 A, float a, float f);
    static string GeAn(Vector2 m, float s, float f);
    static CuiButton BuildButtonUI(string panelName, Vector2 p, int ct, float mi, float ma, string c, float f);
    static CuiElement BuildIconUI(string pN, Vector2 p, string iN, float mi, float ma, string c, float f, bool b);
    static CuiElement CustomIconUI(string pN, Vector2 p, string iN, float mi, float ma, string c, float f);
    static CuiButton CustomButtonUI(string panelName, Vector2 p, string cmd, float mi, float ma, string c, float f);
    static CuiElement CreateRawImage(string pN, Vector2 p, string iN, float mi, float ma, string c, float f);
    static string r(string i);
     object OnEntityGroundMissing(BaseEntity ent);
     void ClearUp(Transform root);
     object OnServerCommand(ConsoleSystem.Arg arg);
     object OnMessagePlayer(string message, BasePlayer player);
}

 class StrdDt
{
    public Dictionary<ulong, Plyrnf> PlyData;
    public StrdDt();
}

 class Plyrnf
{
    public float SF;
    public int DBG;
    public Plyrnf();
}

public class EPlanner : MonoBehaviour
{
     BasePlayer player;
     InputState stt;
     uint ctvtm;
     Construction.Target target;
     Construction.Target mvTrgt;
     BaseEntity mvTrgtSnp;
     Construction construction;
     Construction mvCnstrctn;
     Construction ryDfntn;
     Vector3 rttnOffst;
     Vector3 mvOffst;
     string lstCrsshr;
     string lstWrnng;
     Planner plnnr;
     bool sPlnnr;
     bool sHmmr;
     bool isWireTool;
     HeldEntity hldItm;
     bool sRmvr;
     bool isAnotherHeld;
     Item ctvtmLnk;
     bool sTpDplybl;
     int dfltGrd;
     uint lstPrfb;
     bool initialized;
     bool ctvTrgt;
     bool sPlcng;
     float tkDist;
     RaycastHit rayHit;
     BaseEntity rayEntity;
     IPlayer rayEntityOwner;
     string rayEntityName;
     Vector3 lastAimAngles;
     Socket_Base lastSocketBase;
     Vector3 lastSocketPos;
     BaseEntity lastSocketEntity;
     Construction.Placement lastPlacement;
     Ray lastRay;
     bool plannerInfoStatus;
     bool removerInfoStatus;
     bool hammerInfoStatus;
     bool lastSocketForce;
     int cuiFontSize;
     string cuiFontColor;
     string fontType;
     float lastPosRotUpdate;
     void Awake();
     void Start();
     void UnEqp();
     void GetTool(object[] tool);
     void CrtTls();
     bool GtCrrntTl();
     void ChkRmvr();
    public void StHldItm(uint uid);
    public void TckUpd(PlayerTick tick);
     void Update();
     void DecayUpdate(BaseEntity entity, bool isAdding, bool isBuildingPrivilege, BaseEntity target);
     void PlcOnTrgt();
     void TrPlcTrgt();
    public object GtMvTrgt();
     void DMvmntSnc(BaseEntity entity, bool isChild);
    public void DoTi();
     void FndTrrnPlcmnt(Construction.Target t, Construction c, float maxDistance, bool isQuarry);
    public void StBlckPrfb(uint p);
    public void OnDestroy();
     void DoRm(bool remAl);
     void CollRm(BaseEntity srcntt);
     WaitForEndOfFrame wait;
     IEnumerator DlyRm(List<BuildingBlock> bLst, List<DecayEntity> dLst, List<BuildingPrivlidge> pLst);
     void DoPl();
     void ChkPlcmnt(Construction.Target t, Construction c);
     void ChkQrr(Construction c);
    public GameObject DoPlaG(Construction.Target p, Construction component);
     BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
     Construction.Placement ChckPlcmnt(Construction.Target t, Construction c);
     bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
    public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
     void DestroyInfo(UType uType);
     void DoPlannerInfo();
     void DPlnnrUpdt(PType pType, string infoMsg);
     void DoRemoverInfo();
     void DRmvrUpdt(RType rType, string infoMsg, bool altMode);
     void DoHammerInfo();
     void DHmmrUpdt(HType hType, string infoMsg);
     void BldMnUI(float factor);
     void DoCrosshair(string cColor, bool kill);
     void DoWarning(string cColor, bool kill);
}


```

---

## UIConnect

```csharp
using System.Collections.Generic;
using System;
using System.Runtime.InteropServices.ComTypes;
using ConVar;
using Facepunch.Steamworks;
using JetBrains.Annotations;
using Mono.Security.X509;
using UnityEngine;
using Oxide.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;
using UnityEngine.Networking;
using Console = System.Console;

Oxide.Plugins
[Info("UIConnect", "A1M41K", "1.2.2")]
 class UIConnect : RustPlugin
{
    private int FontSizeText;
    private string FontName;
    private bool ShowGUI;
    private bool ShowPrefix;
    private bool ShowReason;
    private bool ShowDisconnect;
    private bool showconnectadmin;
    private string colorname;
    private string colorprefix;
    private string Prefix;
    private string formatconnectChat;
    private string formatLeaveChat;
    private string formatconnectUI;
    private string AnchorMinUI;
    private string AnchorMaxUI;
     void OnPlayerInit(BasePlayer player, string[] args);
     void OnServerInitialized();
    private void updateGUI(BasePlayer player, string Name);
    private void ConnectGUI(BasePlayer player, string Name);
    private new void LoadDefaultConfig();
    private new void LoadConfig();
    private void GetConfig(string menu, string Key, T var);
     string color(string color, string text);
     string getMessageFormat(BasePlayer player, string type);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void sendLeaveMessage(BasePlayer player, string reason);
     void sendJoinMessage(BasePlayer player);
}


```

---

## UItems

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("UItems","Baks","1.2")]
public class UItems : RustPlugin
{
    private string pluginPrefix;
     class PluginConfig
    {
        [JsonProperty("Привилегия для Инструментов")]
        public string ToolsPerm;
        [JsonProperty("Привилегия для Оружия")]
        public string WeaponPerm;
        [JsonProperty("Привилегия для Одежды")]
        public string AttirePerm;
        [JsonProperty("Привилегия для Патронов")]
        public string AmmunitionPerm;
        [JsonProperty("Привилегия для Ракет")]
        public string RocketPerm;
    }

    private PluginConfig config;
    private void Init();
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void OnLoseCondition(Item item, float amount);
     object OnAmmoUnload(BaseProjectile projectile, Item item, BasePlayer player);
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player);
    private void OnRocketLaunched(BasePlayer player);
}

 class PluginConfig
{
    [JsonProperty("Привилегия для Инструментов")]
    public string ToolsPerm;
    [JsonProperty("Привилегия для Оружия")]
    public string WeaponPerm;
    [JsonProperty("Привилегия для Одежды")]
    public string AttirePerm;
    [JsonProperty("Привилегия для Патронов")]
    public string AmmunitionPerm;
    [JsonProperty("Привилегия для Ракет")]
    public string RocketPerm;
}


```

---

## UItemSort

```csharp
using System;
using System.Collections.Generic;
using System.Drawing;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Steamworks.ServerList;
using UnityEngine;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("UItemSort", "Scrooge", "1.0.2")]
[Description("Плагин был куплен на keepshop.ru")]
 class UItemSort : RustPlugin
{
    private class PluginConfig
    {
        [JsonProperty("Use button images?")]
        public bool useImages;
        [JsonProperty("Send plugin messages/reply?")]
        public bool pluginReply;
        [JsonProperty("Sort button color.")]
        public string sortBttnColor;
        [JsonProperty("Take similar button color.")]
        public string similarBttnColor;
        [JsonProperty("Take all button color.")]
        public string allBttnColor;
        [JsonProperty("Sort image.")]
        public string sortImg;
        [JsonProperty("Similar image.")]
        public string similarImg;
        [JsonProperty("Take/Put all.")]
        public string allImg;
    }

    private class PluginInterface
    {
        public string ItemSort;
    }

    private const string permissionUse;
    private const string UI_Layer;
    private const string UI_LayerMain;
    private static PluginInterface _interface;
    private static PluginConfig _config;
    private static bool _initiated;
    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultMessages();
    private string GetLocal(string mesKey, string userId);
    private void MoveItems(ItemContainer from, ItemContainer to);
    private void MoveSimilarItems(ItemContainer from, ItemContainer to);
    private void SortItemContainer(ItemContainer container);
    public void RegPermission(string name);
    public bool HasPermission(BasePlayer player, string name);
    public static string GetColor(string hex, float alpha);
    [PluginReference]
    private Plugin ImageLibrary;
    private void AddImage(string url);
    private string GetImage(string name);
    private bool IsReady();
     void OnServerInitialized();
    private void ImagesChecker();
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     void OnPlayerLootEnd(PlayerLoot inventory);
    private void BuildInterface();
    private void DrawUI(BasePlayer player);
    private void DestroyUI(BasePlayer player);
    [ConsoleCommand("UI_Sort")]
    private void Console_SortItems(ConsoleSystem.Arg arg);
}

private class PluginConfig
{
    [JsonProperty("Use button images?")]
    public bool useImages;
    [JsonProperty("Send plugin messages/reply?")]
    public bool pluginReply;
    [JsonProperty("Sort button color.")]
    public string sortBttnColor;
    [JsonProperty("Take similar button color.")]
    public string similarBttnColor;
    [JsonProperty("Take all button color.")]
    public string allBttnColor;
    [JsonProperty("Sort image.")]
    public string sortImg;
    [JsonProperty("Similar image.")]
    public string similarImg;
    [JsonProperty("Take/Put all.")]
    public string allImg;
}

private class PluginInterface
{
    public string ItemSort;
}


```

---

## UltraniumOre

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("UltraniumOre", "CASHR", "1.0.0")]
public class UltraniumOre : RustPlugin
{
    [JsonProperty("Тут трогать только DisplayName")]
    private Dictionary<Type, CustomItem> Items;
    private class CustomItem
    {
        public string DisplayName;
        public string ShortName;
        public ulong SkinID;
        public Item CreateItem(int amount);
    }

    private class Configuration
    {
        [JsonProperty("Шанс выпадения руды")]
        public int Chance;
        public static Configuration GetNewConf();
    }

    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Configuration _config;
    protected override void LoadConfig();
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnServerInitialized();
    private void CanAcceptItem(ItemContainer container, Item item, int targetPos);
    private Item OnItemSplit(Item item, int amount);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
}

private class CustomItem
{
    public string DisplayName;
    public string ShortName;
    public ulong SkinID;
    public Item CreateItem(int amount);
}

private class Configuration
{
    [JsonProperty("Шанс выпадения руды")]
    public int Chance;
    public static Configuration GetNewConf();
}


```

---

## UniqueCupboard

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Drawing;
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("UniqueCupboard", "https://topplugin.ru/", "2.1.0")]
 class UniqueCupboard : RustPlugin
{
    private class Configuration
    {
        [JsonProperty("Время которое требуется отыграть на сервере, для получения уникального шкафа в минутах", Order = 0)]
        public int PlayedTime;
        [JsonProperty("Название уникального шкафа", Order = 1)]
        public string CupboardName;
        [JsonProperty("Настройки команд", Order = 2)]
        public CommandSettings Command;
        [JsonProperty("Приз", Order = 3)]
        public PrizeSettings Prize;
        [JsonProperty("Настройки очков", Order = 4)]
        public PointSettings Points;
        public class PointSettings
        {
            [JsonProperty("Сколько очков отнимать в случае потери своего шкафа", Order = 0)]
            public int takePoint;
        }

        public class PrizeSettings
        {
            [JsonProperty("Исполняемая команда, для участника занявшего 1-ое место", Order = 0)]
            public string prize1;
            [JsonProperty("Исполняемая команда, для участника занявшего 2-ое место", Order = 0)]
            public string prize2;
            [JsonProperty("Исполняемая команда, для участника занявшего 3-ое место", Order = 0)]
            public string prize3;
        }

        public class CommandSettings
        {
            [JsonProperty("Команда доступа к статистике участников ивента", Order = 0)]
            public string topCommand;
            [JsonProperty("Команда для участия в ивенте", Order = 1)]
            public string yesCommand;
            [JsonProperty("Команда для проверки наличия уникального шкафа", Order = 2)]
            public string whoCommand;
        }

        public static Configuration GetNewConfiguration();
    }

    private Configuration config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public DataManager playerData { get; set; }
    public Dictionary<ulong, CupboardData> cupboardsData { get; set; }
    public Dictionary<uint, CupboardData> cupboardsDataOfUint { get; set; }
    public Dictionary<ulong, int> ActivePage { get; set; }
    public class CupboardData
    {
        public int PointsCount { get; set; }
        public uint NetId { get; set; }
        public DateTime PlacedTime { get; set; }
        public string ownerName { get; set; }
        public void SetPoints(int points);
        public CupboardData(uint NetId, string displayName);
    }

    public class DataManager
    {
        public DateTime WipeTime { get; set; }
        public Dictionary<ulong, int> TimePlayed { get; set; }
        public List<ulong> CupboardCreate { get; set; }
        public void PrepareData(ulong userID);
    }

    private void OnServerInitialized();
     void OnNewSave(string filename);
     void OnServerSave();
    private void Unload();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void LoadData();
    private void SaveData();
    [ConsoleCommand("CUPBOARD_CLOSE")]
    private void CloseCupboard(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUPBOARD_NEXTPAGE")]
    private void CupboardNextPage(ConsoleSystem.Arg arg);
    [ConsoleCommand("CUPBOARD_BackPage")]
    private void CupboardBackPage(ConsoleSystem.Arg arg);
    private void CMDtopTC(BasePlayer player, string command, string[] args);
    private void cupboardWho(BasePlayer player, string command, string[] args);
    private void cupboardYes(BasePlayer player, string command, string[] args);
    private bool IsNPC(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
    private void GUIShowTCTOP(BasePlayer player, int page);
    private List<ulong> GetListTopOfPage(int Page);
    private void TimeUpdate();
    private int CalculatePointsOfWipe(DateTime cupboardTime);
    private int CalculateOldPoints(BuildingPrivlidge cupboard);
    private int CalculatePoints(BuildingPrivlidge cupboard);
    private bool IsYourCupboard(ulong initiatorId, uint netid);
    private void FillNetDataOfCupboards();
    public static string FormatShortTime(int min);
    private bool HasPriviligesToUseCupboard(BasePlayer player);
    private Item GetCupboardItem();
}

private class Configuration
{
    [JsonProperty("Время которое требуется отыграть на сервере, для получения уникального шкафа в минутах", Order = 0)]
    public int PlayedTime;
    [JsonProperty("Название уникального шкафа", Order = 1)]
    public string CupboardName;
    [JsonProperty("Настройки команд", Order = 2)]
    public CommandSettings Command;
    [JsonProperty("Приз", Order = 3)]
    public PrizeSettings Prize;
    [JsonProperty("Настройки очков", Order = 4)]
    public PointSettings Points;
    public class PointSettings
    {
        [JsonProperty("Сколько очков отнимать в случае потери своего шкафа", Order = 0)]
        public int takePoint;
    }

    public class PrizeSettings
    {
        [JsonProperty("Исполняемая команда, для участника занявшего 1-ое место", Order = 0)]
        public string prize1;
        [JsonProperty("Исполняемая команда, для участника занявшего 2-ое место", Order = 0)]
        public string prize2;
        [JsonProperty("Исполняемая команда, для участника занявшего 3-ое место", Order = 0)]
        public string prize3;
    }

    public class CommandSettings
    {
        [JsonProperty("Команда доступа к статистике участников ивента", Order = 0)]
        public string topCommand;
        [JsonProperty("Команда для участия в ивенте", Order = 1)]
        public string yesCommand;
        [JsonProperty("Команда для проверки наличия уникального шкафа", Order = 2)]
        public string whoCommand;
    }

    public static Configuration GetNewConfiguration();
}

public class PointSettings
{
    [JsonProperty("Сколько очков отнимать в случае потери своего шкафа", Order = 0)]
    public int takePoint;
}

public class PrizeSettings
{
    [JsonProperty("Исполняемая команда, для участника занявшего 1-ое место", Order = 0)]
    public string prize1;
    [JsonProperty("Исполняемая команда, для участника занявшего 2-ое место", Order = 0)]
    public string prize2;
    [JsonProperty("Исполняемая команда, для участника занявшего 3-ое место", Order = 0)]
    public string prize3;
}

public class CommandSettings
{
    [JsonProperty("Команда доступа к статистике участников ивента", Order = 0)]
    public string topCommand;
    [JsonProperty("Команда для участия в ивенте", Order = 1)]
    public string yesCommand;
    [JsonProperty("Команда для проверки наличия уникального шкафа", Order = 2)]
    public string whoCommand;
}

public class CupboardData
{
    public int PointsCount { get; set; }
    public uint NetId { get; set; }
    public DateTime PlacedTime { get; set; }
    public string ownerName { get; set; }
    public void SetPoints(int points);
    public CupboardData(uint NetId, string displayName);
}

public class DataManager
{
    public DateTime WipeTime { get; set; }
    public Dictionary<ulong, int> TimePlayed { get; set; }
    public List<ulong> CupboardCreate { get; set; }
    public void PrepareData(ulong userID);
}


```

---

## UnlimitedFix

```csharp
using System;
using System.Collections.Generic;
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using UnityEngine;

Oxide.Plugins
[Info("UnlimitedFix", "Ryamkk", "2.0.5")]
public class UnlimitedFix : RustPlugin
{
    private Dictionary<BaseEntity, HitInfo> lastHitInfo;
    [JsonProperty("Список запрещённых команд на сервере")]
     List<string> blacklistCMD;
    [JsonProperty("Список запрещённых ников на сервере")]
    private List<List<string>> ForbiddenWords;
    [JsonProperty("Дата файл репортов на игроков")]
    public Dictionary<ulong, NumberDetectData> NumberDetectUser;
    public class NumberDetectData
    {
        [JsonProperty("Количество детектов за большое расстояние")]
        public int ProjectileDistanceMore;
        [JsonProperty("Количество детектов за фейковое расстояние")]
        public int FakeDistance;
        [JsonProperty("Количество детектов за использование SilentAim")]
        public int SilentAim;
    }

     void ReadData();
     void WriteData();
     void RegisteredDataUser(BasePlayer player);
    private void Unload();
    private void OnServerInitialized();
     void OnPlayerInit(BasePlayer player);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object OnUserApprove(Connection connection);
    private void PlayerBanForbiddenWords(BasePlayer player, string message);
    private void OnPlayerChat(ConsoleSystem.Arg arg);
    private object OnPlayerDie(BasePlayer player, HitInfo info);
    [ConsoleCommand("testvk")]
     void testvk(ConsoleSystem.Arg args);
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private string URLEncode(string input);
     void SendVKLogs(string msg);
}

public class NumberDetectData
{
    [JsonProperty("Количество детектов за большое расстояние")]
    public int ProjectileDistanceMore;
    [JsonProperty("Количество детектов за фейковое расстояние")]
    public int FakeDistance;
    [JsonProperty("Количество детектов за использование SilentAim")]
    public int SilentAim;
}


```

---

## UnlockDlcItems

```csharp
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Unlock Dlc Items", "rustmods.ru", "1.0.0")]
[Description("Unlock all DLC items blueprints")]
 class UnlockDlcItems : RustPlugin
{
    private const string permAllow;
    private List<ItemBlueprint> dlcItemBlueprintsCache;
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnGroupPermissionGranted(string name, string permName);
    private void OnGroupPermissionRevoked(string name, string permName);
    private void OnUserPermissionGranted(string id, string permName);
    private void OnUserPermissionRevoked(string id, string permName);
    private void UnlockDLC(BasePlayer player);
    private void ResetDLC(BasePlayer player);
}


```

---

## Updater

```csharp
using System.Text.RegularExpressions;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System.Linq;
using System;

Oxide.Plugins

```

---

## UpLifted

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using System.Text;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Configuration;
using UnityEngine;
using Network;
using Facepunch;
using Facepunch.Extend;
using Oxide.Game.Rust.Cui;
using Newtonsoft.Json;

Oxide.Plugins
[Info("UpLifted", "FuJiCuRa", "1.2.13", ResourceId = 110)]
internal class UpLifted : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin Friends;
    private static UpLifted UpL;
    private bool initialized;
    private bool Changed;
    private bool hasFullyLoaded;
    private double lastHour;
    private static bool isDayTime;
    private int versionMajor;
    private int versionMinor;
    private bool _newConfig;
    private static Dictionary<string, bool> adminAccessEnabled;
    private static Dictionary<string, bool> adminLiftEntkill;
    private static Dictionary<uint, Elevator> crossReference;
    private Dictionary<ulong, Elevator> playerStartups;
    private List<string> permissionGroups;
    private List<ConsoleSystem.Command> aclVars;
    private static WaitForFixedUpdate waitFFU;
    private static WaitForEndOfFrame waitFOF;
    private static string cabinControlUI;
    private static string cabinDestroyUI;
    private static string cabinSettingsUI;
    private static string cabinPlacementUI;
    private static string cabinSharingUI;
    private Dictionary<string, object> permissionBlock;
    private Dictionary<string, object> playerPermissionMatrix;
    private object GetConfig(string menu, string datavalue, object defaultValue);
    protected override void LoadDefaultConfig();
    private string msg(string key, string id);
    protected override void LoadDefaultMessages();
    private bool loadOverrideCorrupted;
    private string createChatCommand;
    private string helpChatCommand;
    private string pluginPrefix;
    private string prefixColor;
    private string prefixFormat;
    private static int buildingGrade;
    private static string knockEffectGuid;
    private int lastStableFloor;
    private static bool enableClanShare;
    private static bool enableFriendShare;
    private static bool clansEnabled;
    private static bool friendsEnabled;
    private bool sharingPlugins;
    private bool enableEntIdKillProtection;
    private bool enableRemoverToolProtection;
    private bool enableSetHomeDeny;
    private static bool isGroundBlockVulnerable;
    private static bool doorDestructionKillsLiftCabin;
    private static bool sleepWatchEnabled;
    private static float sleepWatchDelay;
    private static float sleepWatchInterval;
    private static bool sleepWatchMoveDown;
    private static bool preventPlacementInCaves;
    private static bool preventPlacementAboveDeployables;
    private bool admAccessSwitchable;
    private bool admSwitchEnabledAtLogin;
    private string admAccessToggleCmd;
    private int admAccessAuthLevel;
    private string admAccessPermission;
    private List<object> admPseudoPerms;
    private List<string> pseudoPerms;
    private void LoadVariables();
    private void Init();
    private void Unload();
    private void OnPluginLoaded(Plugin name);
    private void OnPluginUnloaded(Plugin name);
    private void OnServerInitialized();
    private object OnEntityStabilityCheck(StabilityEntity stabilityEntity);
    private void SaveData(bool isUnload);
    private void OnServerSave();
    private static bool HasFriendS(string owner, string friend);
    private void OnPlayerInit(BasePlayer player);
    private void SetPlayer(BasePlayer player);
    private static bool SameClanS(string owner, string member);
    private static bool ClanCheck(string owner, string oldTag, string newTag);
    private bool CrtPrcdrlLft(BasePlayer player, BaseEntity baseEntity, ProceduralLift l, int offset);
    private void chatCmdLiftOwner(BasePlayer player, string command, string[] args);
    private void chatCmdToggleAdm(BasePlayer player, string command, string[] args);
    private void cmdToggleAdm(ConsoleSystem.Arg arg);
    private void chatCmdLiftReset(BasePlayer player, string command, string[] args);
    private Vector3 GtGrndBldng(Vector3 sourcePos);
    private void chatCmdTowerLift(BasePlayer player, string command, string[] args);
    private void OnTick();
    private void OnTimeSunrise();
    private void OnTimeSunset();
    [ConsoleCommand("_ul.commands")]
    private void CmdLftCmmnds(ConsoleSystem.Arg arg);
    [ConsoleCommand("_ul.placement")]
    private void CmdLftPlcmnt(ConsoleSystem.Arg arg);
    public class ELStorage
    {
        public float timeToTake;
        public float timeTaken;
        public ulong ownerID;
        public uint groundBlockID;
        public uint[] doorsID;
        public int[] fShares;
        public int startSock;
        public List<int> cQueue;
        public List<int> pQueue;
        public int _uFloor;
        public int _cFloor;
        public int _dFloor;
        public int _lcFloor;
        public int _lFloor;
        public float initialHeight;
        public int _state;
        public int _call;
        public int _floor;
        public int _last;
        public int _tower;
        public int _power;
        public int _panel;
        public int _lightmode;
        public int moveSpeed;
        public int maxSpeed;
        public int fuelConsume;
        public bool enableFuel;
        public bool enableCost;
        public float costMultiplier;
        public int floorTime;
        public int closeTime;
        public int _sharemode;
        public ulong skinID;
        public string clanTag;
        public string oldTag;
        public uint cTime;
        public List<ulong> _passengers;
        public ELStorage();
    }

    public class Elevator : FacepunchBehaviour
    {
        private Oxide.Core.Libraries.Time time;
        private string msg(string key, string id);
        public LiftUI controlUI;
        private CabinComfort cabinComfort;
        private BoxCollider boxCollider;
        private List<uint> toBeProtected;
        private int moveSpeed;
        private int maxSpeed;
        private int floorTime;
        private int fuelConsume;
        private int closeTime;
        private bool enableFuel;
        private bool enableCost;
        private float costMultiplier;
        private bool calledDestroy;
        private float timeToTake;
        private float timeTaken;
        private ItemContainer storeBox;
        private ItemContainer[] tunaBoxes;
        private ulong skinID;
        private string knockEffectGuid;
        private BuildingManager.Building baseBuilding;
        private BuildingPrivlidge buildingPrivilege;
        public Dictionary<ulong, bool[]> userAccessLevel;
        private float initialHeight;
        private List<int> cQueue;
        private List<int> pQueue;
        private int startSock;
        private int _uFloor;
        private int _cFloor;
        private int _dFloor;
        private int _lcFloor;
        private int _lFloor;
        private Vector3 _currentPosition;
        public ProceduralLift lift;
        public uint liftID;
        private BasePlayer ownerPlayer;
        private BasePlayer lastUser;
        private string clanTag;
        private string oldTag;
        private uint cTime;
        private ulong ownerID;
        private BaseEntity groundBlock;
        private BaseEntity roofBlock;
        private Vector3[] stops;
        private Door[] doors;
        private FloorShare[] floorShares;
        private SimpleLight cabinLight;
        private SimpleBuildingBlock floorGrill;
        private Recycler recyclerBox;
        private CodeLock cabinLock;
        private Mailbox fuelBox;
        private Vector3[] socketPoints;
        private int[] socketOrder;
        private BaseEntity[][] baseBlocksFloors;
        private bool doorSwitched;
        private CabinState _state;
        private QState _call;
        private QState _panel;
        private PowerState _power;
        private FloorState _floor;
        private TowerState _tower;
        private LastQueue _last;
        private MoveDirection _direction;
        private CabinLightMode _lightmode;
        public ShareMode _sharemode;
        public bool IsIdle { get; set; }
        private bool UpKeep { get; set; }
        private void OnCreation();
        private void OnReady();
        private void OnSaving();
        private void OnLoading();
        private void OnSpawned();
        public bool IsReady { get; set; }
        private bool IsSpawned { get; set; }
        public int CurrentFloor { get; set; }
        public int UpperFloor { get; set; }
        public int LowerFloor { get; set; }
        public int DestinationFloor { get; set; }
        public int LastCallFromFloor { get; set; }
        private int ArrSum { get; set; }
        private Vector3 CurrentPosition { get; set; }
        public Door GetDoorAt(int f);
        public int GetDoorCount { get; set; }
        public bool IsGround(BaseEntity ent);
        public bool IsDoor(BaseEntity ent);
        public bool IsOwner(BasePlayer player);
        public ulong GetOwner { get; set; }
        public string GetTag { get; set; }
        public uint GetAge { get; set; }
        public void SetOwner(ulong id);
        public bool EnableFuel { get; set; }
        public bool EnableBuildCost { get; set; }
        public int GetLightMode { get; set; }
        public int MoveSpeed { get; set; }
        public int MaxSpeed { get; set; }
        public int FloorWait { get; set; }
        public int IdleWait { get; set; }
        public int GetPowerState { get; set; }
        public FloorShare GetFloorMode(int f);
        public ulong GetSkinID { get; set; }
        private HashSet<BasePlayer> _passengers;
        private bool _holdDoorTriggered;
        private bool _holdSleeperTriggered;
        private bool HasPassenger();
        private bool DoorTriggered { get; set; }
        private bool SleepTriggered { get; set; }
        public bool IsPassenger(ulong id);
        private int SleeperCount { get; set; }
        private bool IsDayTime();
        public void BeforeCreation();
        public bool IntNw(ProceduralLift procLift, BaseEntity baseEntity, BasePlayer basePlayer, int offSocket, int startFloor, int stopFloor, char[] placeDoors, List<ItemAmount> collectAmounts, bool hasCost, float costMulti);
        public ELStorage Sv2Prt(bool isFinished, bool isUnload);
        public bool Ld2Prt(ProceduralLift procLift, ELStorage data);
        public void RstMvmnt(BasePlayer player);
        public void ChckDrKnck(Door door, BasePlayer player);
        public object ChckLftUs(ProceduralLift procLift, BasePlayer player);
        public void SwtchFlrLght(object floor, int lvl);
        public void SwtchCbnLght(bool state);
        public void SetUpKeep(bool flag);
        public bool HsLftAccss(BasePlayer player);
        public bool HsFlrAccss(BasePlayer player, FloorShare share, int p);
        public void UIEnterCommands(BasePlayer player, string action, int num);
        private void DefineShareMode(BasePlayer player);
        public string GetShareMode(bool withsuffix);
        private void TryAddInternal(int num);
        private void TryAddExternal(int num, BasePlayer player);
        private void TryMvCbn(int num, LastQueue last);
        public void StartIdleClose();
        private void DoIdleClose();
        private void CloseFloor(int num);
        public void OpenFloor(int num, bool force);
        private void DoNextMove();
        private void EnterIdle();
        private void DoPanelMove();
        private void DoCallMove();
        public void DoForceMove(int floor);
        private void StrtMvng();
        private void GetDirection();
        private void OnCallQueue(bool isDone);
        private void OnPanelQueue(bool isDone);
        private void Update();
        private void SncPs(BaseEntity entity);
        private void SncKll(BaseEntity entity);
        public void EntityKill(uint entid, bool isDoor);
        private void OnDestroy();
        private IEnumerator SoftDestroy(uint entid);
        public void SetupCollider();
        private void SleepWatch();
        public List<ulong> SavePassengers();
        public void LoadPassengers(List<ulong> l);
        public void MvPssngrs(float toPos);
        public void RemovePassenger(BasePlayer player);
        public bool ClsDrpBx(Mailbox fuelBox);
        private void OnTriggerEnter(Collider other);
        private void OnTriggerExit(Collider other);
        public int GetFuel();
        public Item GetFuelItem();
        public bool InsertFuel(Item item);
        public void WithDrawFuel(Item item);
        private void GetBuilding();
        public bool FuelCheck();
        public bool PreFuelCheck(int curr, int dest);
        private void SetStates();
        public void IntFlrStps();
        public void GtFlrStps();
        public void IntScktrdr();
        private void GtScktPnts();
        private void PayInForShaft(List<ItemAmount> collectAmounts, BasePlayer player);
        public List<ItemAmount> GetPartsCost(int grade, int setFloors, char[] placeDoors, float costMulti);
        private List<ItemAmount> CostForObject(uint prefabID);
        private List<ItemAmount> CostForPart(int gradeNum, uint prefabID);
        private void AddCbnLght();
        private void AddFlrGrll();
        private void GtCbnLght();
        private void FtCmpnnts(BaseEntity entity);
        private void AddRcclr();
        private void GtRcclr();
        private void AddCbnLck();
        private void GtCbnLck();
        private void AddFuelBox();
        private void OnDropBoxAdded(Item item, bool added);
        private void GetFuelBox();
        private void DstryOnClnt(BaseEntity entity);
        private void AddFlrLght(BaseEntity parentEnt);
        private void GetFlrLght(BaseEntity slot);
        private void RmvBsmnt(bool liftOnly);
        private IEnumerator RemoveShaft(Action<bool> done);
        private void CollectPayOut(List<ItemAmount> collectAmounts, List<ItemAmount> partAmounts, float costMulti);
        private void PayOutForPart(List<ItemAmount> collectAmounts);
        private string lastShaftError;
        private IEnumerator CrtNwShft(char[] d, List<ItemAmount> collectAmounts, BasePlayer player, bool hasCost, float costMulti, Action<bool> done);
        private bool AddGrndDr();
        private void GtGrndDr();
        private void AddRfTp();
        private void GtRfTp();
        private bool AddGrndSds();
        private void GtGrndSds();
        private bool ClnnnrFlr(int level);
        private bool AddFlrFrnt(int level, bool isDoor);
        private void FillUppSlot(Door door);
        private void GtFlrFrnt(int level);
        private IEnumerator SwtchFlrFrnt(BasePlayer player, int level, bool hasDoor, Door door);
        private bool AddFlrSds(int level);
        private void GtFlrSds(int level);
        private EntityLink GtLnk(BaseEntity block, int num);
        private EntityLink GtLnkRf(BaseEntity block, int num);
        private BaseEntity AddWllSd(BaseEntity placeOn, EntityLink link);
        private BaseEntity AddTpFlr(BaseEntity placeOn, EntityLink link);
        private BaseEntity AddWllFrm(BaseEntity placeOn, EntityLink link);
        private BaseEntity AddGrgDr(BaseEntity placeOn);
        private bool ObjctFnlzd(BaseEntity baseEntity, ulong skin);
        private BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
        private bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
        private IEnumerator ChngDrSkns();
        private void StoreBox(BasePlayer player);
        private void OnGarageDoorAdded(Item item, bool bAdded);
        public void TunaBox(BasePlayer player, BaseOven oven, BaseEntity parent);
        private void OnTunaFuelAdded(Item item, bool bAdded);
        public void SendEffect(uint id, BaseEntity ent);
        public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
        public void SendEffUnlock(BaseEntity e);
        public void SendEffLock(BaseEntity e);
        public void SendEffDenied(BaseEntity e);
        public void SendEffUpdated(BaseEntity e);
        public void SendEffDeploy(BaseEntity e);
        public void SendEffFovChange(BaseEntity e);
        public void SendEffRecycleStop(BaseEntity e);
        public void SendEffRecycleStart(BaseEntity e);
    }

    public class LiftUI
    {
        public LiftUI();
        private string msg(string key, string id);
        public Elevator elev;
        private uint elevatorID;
        private BaseEntity baseEntity;
        private ProceduralLift lift;
        private int lastIndex;
        private char[] doorsToPlaceChar;
        private int offsetDown;
        private bool hasCreateCost;
        private float costMultiplier;
        private List<ItemAmount> collectAmounts;
        private string[] floorLetters;
        private string[] floorLettersSingle;
        private int maxRows;
        private int numSpecialBtns;
        private int maxButtons;
        private int maxColumns;
        private float pWitdhHalf;
        private int currentFloor;
        private int upperFloor;
        private int lowerFloor;
        private bool useFloorNumbers;
        private int stopLevel;
        private int startLevel;
        private int arrLength;
        private bool enableFuel;
        private string textColorIron;
        private string textColorNickel;
        private string textColorChlorine;
        private string textColorKrypton;
        private string textColorSelenium;
        private string textColorGold;
        private string textColorBohrium;
        private string textColorSilicon;
        private string textColorMercury;
        private string bgColorBromine;
        private string bgColorAstantine;
        private string buttonColor;
        private string font;
        public static void DestroyAllUi(BasePlayer player);
        public void DestroyUi(BasePlayer player);
        public void Init(Elevator e, uint id);
        public void IntPlcmnt(Elevator e, ProceduralLift l, BasePlayer player, BaseEntity ent, int offSet, int startFrom, int limitRange, int maxFloor);
        public void UpdtPlcmnt(BasePlayer player, string action, int num);
        private float[] ButtonPosControl(int i);
        private CuiPanel NewPanel();
        private CuiPanel NewBorderPanel(string c, string x, string y);
        private CuiButton NewBgButton(string cmd, string viewForm);
        private CuiButton NormalButton(string x, string y, string cmd, string close, string color, string text, string tColor, int fontsize, TextAnchor align);
        private CuiButton PsBttn(int count, string command, string close, string bcolor, string text, string tcolor, int fontsize);
        public bool WayUpFree(BaseEntity ent, int fls);
        public void CrtPlcmntUI(BasePlayer player, int indexAt, char[] placeDoorsChar);
        public void CrtCbnUI(BasePlayer player, bool freshOpen);
        public void CrtDstryUI(BasePlayer player);
        public void CrtSttngsUI(BasePlayer player);
        public void CrtShrngUI(BasePlayer player);
    }

    private static string r(string i);
    public class Prevent_Building : FacepunchBehaviour
    {
        private BoxCollider boxCollider;
        private BaseEntity block;
        private int floors;
        public void Setup(BaseEntity ent, int f);
        public void BeforeCreation();
        public static bool WayUpFree(BaseEntity ent, int fls);
        private void OnDestroy();
    }

    public class CabinComfort : FacepunchBehaviour
    {
        private SphereCollider sphereCollider;
        private TriggerComfort triggerComfort;
        private TriggerTemperature triggerTemperature;
        public void Setup(BaseEntity ent, int comfort, int temp);
        private void OnDestroy();
    }

    private void OnMeleeAttack(BasePlayer player, HitInfo info);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private object OnPlayerCommand(ConsoleSystem.Arg arg);
    private object canRemove(BasePlayer player);
    private object CanBuild(Planner plan, Construction prefab, Construction.Target target);
    private void OnDoorKnocked(Door door, BasePlayer player);
    private void OnPlayerSleep(BasePlayer player);
    private object OnLiftUse(ProceduralLift lift, BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnEntityKill(BaseNetworkable entity);
    private string CreateAcl(bool hasParams, bool isAdding, string newOrOldGroup);
    private void CreateOverview();
    private static string GetGroup(string userid, bool isActive);
    private static object GetAccess(string variable, string userid, object fallbackValue, bool isActive);
    private bool IsAdmPassive(string id);
    private static bool IsAdm(object p);
    private bool IsPseudoAdmin(string id);
    private void OnUserPermissionGranted(string id, string perm);
    [ConsoleCommand("upl.reloadacl")]
    private void CmdAclReload(ConsoleSystem.Arg arg);
    [ConsoleCommand("upl.addgroup")]
    private void CmdAclGroupAdd(ConsoleSystem.Arg arg);
    [ConsoleCommand("upl.delgroup")]
    private void CmdAclGroupDel(ConsoleSystem.Arg arg);
    [ConsoleCommand("upl.clonegroup")]
    private void CmdCloneGoup(ConsoleSystem.Arg arg);
    [ConsoleCommand("upl.resetgroup")]
    private void CmdResetGoup(ConsoleSystem.Arg arg);
    [ConsoleCommand("upl.getgroupforuser")]
    private void CmdGetGroupForUser(ConsoleSystem.Arg arg);
}

public class ELStorage
{
    public float timeToTake;
    public float timeTaken;
    public ulong ownerID;
    public uint groundBlockID;
    public uint[] doorsID;
    public int[] fShares;
    public int startSock;
    public List<int> cQueue;
    public List<int> pQueue;
    public int _uFloor;
    public int _cFloor;
    public int _dFloor;
    public int _lcFloor;
    public int _lFloor;
    public float initialHeight;
    public int _state;
    public int _call;
    public int _floor;
    public int _last;
    public int _tower;
    public int _power;
    public int _panel;
    public int _lightmode;
    public int moveSpeed;
    public int maxSpeed;
    public int fuelConsume;
    public bool enableFuel;
    public bool enableCost;
    public float costMultiplier;
    public int floorTime;
    public int closeTime;
    public int _sharemode;
    public ulong skinID;
    public string clanTag;
    public string oldTag;
    public uint cTime;
    public List<ulong> _passengers;
    public ELStorage();
}

public class Elevator : FacepunchBehaviour
{
    private Oxide.Core.Libraries.Time time;
    private string msg(string key, string id);
    public LiftUI controlUI;
    private CabinComfort cabinComfort;
    private BoxCollider boxCollider;
    private List<uint> toBeProtected;
    private int moveSpeed;
    private int maxSpeed;
    private int floorTime;
    private int fuelConsume;
    private int closeTime;
    private bool enableFuel;
    private bool enableCost;
    private float costMultiplier;
    private bool calledDestroy;
    private float timeToTake;
    private float timeTaken;
    private ItemContainer storeBox;
    private ItemContainer[] tunaBoxes;
    private ulong skinID;
    private string knockEffectGuid;
    private BuildingManager.Building baseBuilding;
    private BuildingPrivlidge buildingPrivilege;
    public Dictionary<ulong, bool[]> userAccessLevel;
    private float initialHeight;
    private List<int> cQueue;
    private List<int> pQueue;
    private int startSock;
    private int _uFloor;
    private int _cFloor;
    private int _dFloor;
    private int _lcFloor;
    private int _lFloor;
    private Vector3 _currentPosition;
    public ProceduralLift lift;
    public uint liftID;
    private BasePlayer ownerPlayer;
    private BasePlayer lastUser;
    private string clanTag;
    private string oldTag;
    private uint cTime;
    private ulong ownerID;
    private BaseEntity groundBlock;
    private BaseEntity roofBlock;
    private Vector3[] stops;
    private Door[] doors;
    private FloorShare[] floorShares;
    private SimpleLight cabinLight;
    private SimpleBuildingBlock floorGrill;
    private Recycler recyclerBox;
    private CodeLock cabinLock;
    private Mailbox fuelBox;
    private Vector3[] socketPoints;
    private int[] socketOrder;
    private BaseEntity[][] baseBlocksFloors;
    private bool doorSwitched;
    private CabinState _state;
    private QState _call;
    private QState _panel;
    private PowerState _power;
    private FloorState _floor;
    private TowerState _tower;
    private LastQueue _last;
    private MoveDirection _direction;
    private CabinLightMode _lightmode;
    public ShareMode _sharemode;
    public bool IsIdle { get; set; }
    private bool UpKeep { get; set; }
    private void OnCreation();
    private void OnReady();
    private void OnSaving();
    private void OnLoading();
    private void OnSpawned();
    public bool IsReady { get; set; }
    private bool IsSpawned { get; set; }
    public int CurrentFloor { get; set; }
    public int UpperFloor { get; set; }
    public int LowerFloor { get; set; }
    public int DestinationFloor { get; set; }
    public int LastCallFromFloor { get; set; }
    private int ArrSum { get; set; }
    private Vector3 CurrentPosition { get; set; }
    public Door GetDoorAt(int f);
    public int GetDoorCount { get; set; }
    public bool IsGround(BaseEntity ent);
    public bool IsDoor(BaseEntity ent);
    public bool IsOwner(BasePlayer player);
    public ulong GetOwner { get; set; }
    public string GetTag { get; set; }
    public uint GetAge { get; set; }
    public void SetOwner(ulong id);
    public bool EnableFuel { get; set; }
    public bool EnableBuildCost { get; set; }
    public int GetLightMode { get; set; }
    public int MoveSpeed { get; set; }
    public int MaxSpeed { get; set; }
    public int FloorWait { get; set; }
    public int IdleWait { get; set; }
    public int GetPowerState { get; set; }
    public FloorShare GetFloorMode(int f);
    public ulong GetSkinID { get; set; }
    private HashSet<BasePlayer> _passengers;
    private bool _holdDoorTriggered;
    private bool _holdSleeperTriggered;
    private bool HasPassenger();
    private bool DoorTriggered { get; set; }
    private bool SleepTriggered { get; set; }
    public bool IsPassenger(ulong id);
    private int SleeperCount { get; set; }
    private bool IsDayTime();
    public void BeforeCreation();
    public bool IntNw(ProceduralLift procLift, BaseEntity baseEntity, BasePlayer basePlayer, int offSocket, int startFloor, int stopFloor, char[] placeDoors, List<ItemAmount> collectAmounts, bool hasCost, float costMulti);
    public ELStorage Sv2Prt(bool isFinished, bool isUnload);
    public bool Ld2Prt(ProceduralLift procLift, ELStorage data);
    public void RstMvmnt(BasePlayer player);
    public void ChckDrKnck(Door door, BasePlayer player);
    public object ChckLftUs(ProceduralLift procLift, BasePlayer player);
    public void SwtchFlrLght(object floor, int lvl);
    public void SwtchCbnLght(bool state);
    public void SetUpKeep(bool flag);
    public bool HsLftAccss(BasePlayer player);
    public bool HsFlrAccss(BasePlayer player, FloorShare share, int p);
    public void UIEnterCommands(BasePlayer player, string action, int num);
    private void DefineShareMode(BasePlayer player);
    public string GetShareMode(bool withsuffix);
    private void TryAddInternal(int num);
    private void TryAddExternal(int num, BasePlayer player);
    private void TryMvCbn(int num, LastQueue last);
    public void StartIdleClose();
    private void DoIdleClose();
    private void CloseFloor(int num);
    public void OpenFloor(int num, bool force);
    private void DoNextMove();
    private void EnterIdle();
    private void DoPanelMove();
    private void DoCallMove();
    public void DoForceMove(int floor);
    private void StrtMvng();
    private void GetDirection();
    private void OnCallQueue(bool isDone);
    private void OnPanelQueue(bool isDone);
    private void Update();
    private void SncPs(BaseEntity entity);
    private void SncKll(BaseEntity entity);
    public void EntityKill(uint entid, bool isDoor);
    private void OnDestroy();
    private IEnumerator SoftDestroy(uint entid);
    public void SetupCollider();
    private void SleepWatch();
    public List<ulong> SavePassengers();
    public void LoadPassengers(List<ulong> l);
    public void MvPssngrs(float toPos);
    public void RemovePassenger(BasePlayer player);
    public bool ClsDrpBx(Mailbox fuelBox);
    private void OnTriggerEnter(Collider other);
    private void OnTriggerExit(Collider other);
    public int GetFuel();
    public Item GetFuelItem();
    public bool InsertFuel(Item item);
    public void WithDrawFuel(Item item);
    private void GetBuilding();
    public bool FuelCheck();
    public bool PreFuelCheck(int curr, int dest);
    private void SetStates();
    public void IntFlrStps();
    public void GtFlrStps();
    public void IntScktrdr();
    private void GtScktPnts();
    private void PayInForShaft(List<ItemAmount> collectAmounts, BasePlayer player);
    public List<ItemAmount> GetPartsCost(int grade, int setFloors, char[] placeDoors, float costMulti);
    private List<ItemAmount> CostForObject(uint prefabID);
    private List<ItemAmount> CostForPart(int gradeNum, uint prefabID);
    private void AddCbnLght();
    private void AddFlrGrll();
    private void GtCbnLght();
    private void FtCmpnnts(BaseEntity entity);
    private void AddRcclr();
    private void GtRcclr();
    private void AddCbnLck();
    private void GtCbnLck();
    private void AddFuelBox();
    private void OnDropBoxAdded(Item item, bool added);
    private void GetFuelBox();
    private void DstryOnClnt(BaseEntity entity);
    private void AddFlrLght(BaseEntity parentEnt);
    private void GetFlrLght(BaseEntity slot);
    private void RmvBsmnt(bool liftOnly);
    private IEnumerator RemoveShaft(Action<bool> done);
    private void CollectPayOut(List<ItemAmount> collectAmounts, List<ItemAmount> partAmounts, float costMulti);
    private void PayOutForPart(List<ItemAmount> collectAmounts);
    private string lastShaftError;
    private IEnumerator CrtNwShft(char[] d, List<ItemAmount> collectAmounts, BasePlayer player, bool hasCost, float costMulti, Action<bool> done);
    private bool AddGrndDr();
    private void GtGrndDr();
    private void AddRfTp();
    private void GtRfTp();
    private bool AddGrndSds();
    private void GtGrndSds();
    private bool ClnnnrFlr(int level);
    private bool AddFlrFrnt(int level, bool isDoor);
    private void FillUppSlot(Door door);
    private void GtFlrFrnt(int level);
    private IEnumerator SwtchFlrFrnt(BasePlayer player, int level, bool hasDoor, Door door);
    private bool AddFlrSds(int level);
    private void GtFlrSds(int level);
    private EntityLink GtLnk(BaseEntity block, int num);
    private EntityLink GtLnkRf(BaseEntity block, int num);
    private BaseEntity AddWllSd(BaseEntity placeOn, EntityLink link);
    private BaseEntity AddTpFlr(BaseEntity placeOn, EntityLink link);
    private BaseEntity AddWllFrm(BaseEntity placeOn, EntityLink link);
    private BaseEntity AddGrgDr(BaseEntity placeOn);
    private bool ObjctFnlzd(BaseEntity baseEntity, ulong skin);
    private BaseEntity CrtCnstrctn(Construction.Target target, Construction component);
    private bool UpdtPlcmnt(Transform tn, Construction common, Construction.Target target);
    private IEnumerator ChngDrSkns();
    private void StoreBox(BasePlayer player);
    private void OnGarageDoorAdded(Item item, bool bAdded);
    public void TunaBox(BasePlayer player, BaseOven oven, BaseEntity parent);
    private void OnTunaFuelAdded(Item item, bool bAdded);
    public void SendEffect(uint id, BaseEntity ent);
    public void SendEffectTo(uint id, BaseEntity ent, BasePlayer player);
    public void SendEffUnlock(BaseEntity e);
    public void SendEffLock(BaseEntity e);
    public void SendEffDenied(BaseEntity e);
    public void SendEffUpdated(BaseEntity e);
    public void SendEffDeploy(BaseEntity e);
    public void SendEffFovChange(BaseEntity e);
    public void SendEffRecycleStop(BaseEntity e);
    public void SendEffRecycleStart(BaseEntity e);
}

public class LiftUI
{
    public LiftUI();
    private string msg(string key, string id);
    public Elevator elev;
    private uint elevatorID;
    private BaseEntity baseEntity;
    private ProceduralLift lift;
    private int lastIndex;
    private char[] doorsToPlaceChar;
    private int offsetDown;
    private bool hasCreateCost;
    private float costMultiplier;
    private List<ItemAmount> collectAmounts;
    private string[] floorLetters;
    private string[] floorLettersSingle;
    private int maxRows;
    private int numSpecialBtns;
    private int maxButtons;
    private int maxColumns;
    private float pWitdhHalf;
    private int currentFloor;
    private int upperFloor;
    private int lowerFloor;
    private bool useFloorNumbers;
    private int stopLevel;
    private int startLevel;
    private int arrLength;
    private bool enableFuel;
    private string textColorIron;
    private string textColorNickel;
    private string textColorChlorine;
    private string textColorKrypton;
    private string textColorSelenium;
    private string textColorGold;
    private string textColorBohrium;
    private string textColorSilicon;
    private string textColorMercury;
    private string bgColorBromine;
    private string bgColorAstantine;
    private string buttonColor;
    private string font;
    public static void DestroyAllUi(BasePlayer player);
    public void DestroyUi(BasePlayer player);
    public void Init(Elevator e, uint id);
    public void IntPlcmnt(Elevator e, ProceduralLift l, BasePlayer player, BaseEntity ent, int offSet, int startFrom, int limitRange, int maxFloor);
    public void UpdtPlcmnt(BasePlayer player, string action, int num);
    private float[] ButtonPosControl(int i);
    private CuiPanel NewPanel();
    private CuiPanel NewBorderPanel(string c, string x, string y);
    private CuiButton NewBgButton(string cmd, string viewForm);
    private CuiButton NormalButton(string x, string y, string cmd, string close, string color, string text, string tColor, int fontsize, TextAnchor align);
    private CuiButton PsBttn(int count, string command, string close, string bcolor, string text, string tcolor, int fontsize);
    public bool WayUpFree(BaseEntity ent, int fls);
    public void CrtPlcmntUI(BasePlayer player, int indexAt, char[] placeDoorsChar);
    public void CrtCbnUI(BasePlayer player, bool freshOpen);
    public void CrtDstryUI(BasePlayer player);
    public void CrtSttngsUI(BasePlayer player);
    public void CrtShrngUI(BasePlayer player);
}

public class Prevent_Building : FacepunchBehaviour
{
    private BoxCollider boxCollider;
    private BaseEntity block;
    private int floors;
    public void Setup(BaseEntity ent, int f);
    public void BeforeCreation();
    public static bool WayUpFree(BaseEntity ent, int fls);
    private void OnDestroy();
}

public class CabinComfort : FacepunchBehaviour
{
    private SphereCollider sphereCollider;
    private TriggerComfort triggerComfort;
    private TriggerTemperature triggerTemperature;
    public void Setup(BaseEntity ent, int comfort, int temp);
    private void OnDestroy();
}


```

---

## UPRemove

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Imaging;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("UpRemove", "Mevent", "1.3")]
public class UPRemove : RustPlugin
{
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin NoEscape;
    private Plugin Clans;
    private Plugin Friends;
    private Plugin Notify;
    private Plugin UINotify;
    private const string Layer;
    private const string LayerUpdate;
    private static UPRemove _instance;
    private const string PermAll;
    private const string modepermission;
    private const string PermFree;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Remove Commands")]
        public readonly string[] RemoveCommands;
        [JsonProperty(PropertyName = "Upgrade Commands")]
        public readonly string[] UpgradeCommands;
        [JsonProperty(PropertyName = "Work with Notify?")]
        public readonly bool UseNotify;
        [JsonProperty(PropertyName = "Setting Modes", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly List<Mode> Modes;
        [JsonProperty(PropertyName = "Upgrade Settings")]
        public readonly UpgradeSettings Upgrade;
        [JsonProperty(PropertyName = "Remove Settings")]
        public readonly RemoveSettings Remove;
        [JsonProperty(PropertyName = "Block Settings")]
        public readonly BlockSettings Block;
        [JsonProperty(PropertyName = "Additional Slot Settings")]
        public readonly AdditionalSlot AdditionalSlot;
        [JsonProperty(PropertyName = "UI Settings")]
        public readonly InterfaceSettings UI;
    }

    private class AdditionalSlot
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled;
    }

    private class InterfaceSettings
    {
        [JsonProperty(PropertyName = "Color 1")]
        public IColor Color1;
        [JsonProperty(PropertyName = "Color 2")]
        public IColor Color2;
        [JsonProperty(PropertyName = "Color 3")]
        public IColor Color3;
        [JsonProperty(PropertyName = "Offset Y")]
        public float OffsetY;
        [JsonProperty(PropertyName = "Offset X")]
        public float OffsetX;
    }

    private class IColor
    {
        [JsonProperty(PropertyName = "HEX")]
        public string Hex;
        [JsonProperty(PropertyName = "Opacity (0 - 100)")]
        public readonly float Alpha;
        [JsonProperty]
        private string _color;
        [JsonIgnore]
        public string Get { get; set; }
        private string GetColor();
        public IColor();
        public IColor(string hex, float alpha);
    }

    private class ConditionSettings
    {
        [JsonProperty(PropertyName = "Default (from game)")]
        public bool Default;
        [JsonProperty(PropertyName = "Use percent?")]
        public bool Percent;
        [JsonProperty(PropertyName = "Percent (value)")]
        public float PercentValue;
    }

    private class BlockSettings
    {
        [JsonProperty(PropertyName = "Work with NoEscape?")]
        public bool UseNoEscape;
        [JsonProperty(PropertyName = "Work with Clans? (clan members will be able to delete/upgrade)")]
        public bool UseClans;
        [JsonProperty(PropertyName = "Work with Friends? (friends will be able to delete/upgrade)")]
        public bool UseFriends;
        [JsonProperty(PropertyName = "Can those authorized in the cupboard delete/upgrade?")]
        public bool UseCupboard;
        [JsonProperty(PropertyName = "Is an upgrade/remove cupbaord required?")]
        public bool NeedCupboard;
    }

    private abstract class TotalSettings
    {
        [JsonProperty(PropertyName = "Time of action")]
        public int ActionTime;
        [JsonProperty(PropertyName = "Cooldown (default | 0 - disable)")]
        public int Cooldown;
        [JsonProperty(PropertyName = "Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VipCooldown;
        [JsonProperty(PropertyName = "Block After Wipe (default | 0 - disable)")]
        public int AfterWipe;
        [JsonProperty(PropertyName = "Block After Wipe", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> VipAfterWipe;
        public int GetCooldown(BasePlayer player);
        public int GetWipeCooldown(BasePlayer player);
    }

    private class UpgradeSettings : TotalSettings
    {
    }

    private class RemoveSettings : TotalSettings
    {
        [JsonProperty(PropertyName = "Blocked items to remove (prefab)",
            ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> BlockedList;
        [JsonProperty(PropertyName = "Return Item")]
        public bool ReturnItem;
        [JsonProperty(PropertyName = "Returnable Item Percentage")]
        public float ReturnPercent;
        [JsonProperty(PropertyName = "Can friends remove? (Friends)")]
        public bool CanFriends;
        [JsonProperty(PropertyName = "Can clanmates remove? (Clans)")]
        public bool CanClan;
        [JsonProperty(PropertyName = "Can teammates remove?")]
        public bool CanTeams;
        [JsonProperty(PropertyName = "Require a cupboard")]
        public bool RequireCupboard;
        [JsonProperty(PropertyName = "Remove by cupboard? (those who are authorized in the cupboard can remove)")]
        public bool RemoveByCupboard;
        [JsonProperty(PropertyName = "Condition Settings")]
        public ConditionSettings Condition;
    }

    private class Mode
    {
        [JsonProperty(PropertyName = "Icon (assets/url)")]
        public string Icon;
        [JsonProperty(PropertyName = "Type (Remove/Wood/Stone/Metal/TopTier)")]
        [JsonConverter(typeof(StringEnumConverter))]
        public Types Type;
        [JsonProperty(PropertyName = "Permission (ex: UPRemove.1)")]
        public string Permission;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     Dictionary<string, string> UPRemoveImages;
     Dictionary<BuildingGrade.Enum, string> UPRemoveString;
    private PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Players", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly Dictionary<ulong, PlayerData> Players;
    }

    private class PlayerData
    {
        [JsonProperty(PropertyName = "Last Upgrade")]
        public DateTime LastUpgrade;
        [JsonProperty(PropertyName = "Last Remove")]
        public DateTime LastRemove;
        public int LeftTime(bool remove, int cooldown);
        public bool HasCooldown(bool remove, int cooldown);
        public static bool HasWipeCooldown(int cooldown);
        public static int WipeLeftTime(int cooldown);
    }

    private PlayerData GetPlayerData(ulong userId);
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void CmdRemove(IPlayer cov, string command, string[] args);
    private void CmdUpgrade(IPlayer cov, string command, string[] args);
    [ConsoleCommand("UI_Builder")]
    private void CmdConsoleBuilding(ConsoleSystem.Arg arg);
    private readonly Dictionary<BasePlayer, BuildComponent> _components;
    private BuildComponent GetBuild(BasePlayer player);
    private BuildComponent AddOrGetBuild(BasePlayer player, bool item);
    private class BuildComponent : FacepunchBehaviour
    {
        private BasePlayer _player;
        private Mode _mode;
        private float _startTime;
        private readonly CuiElementContainer _container;
        private bool _started;
        private float _cooldown;
        public bool activeByItem;
        private void Awake();
        public void Init(Mode mode);
        public void MainUi();
        private void UpdateUi();
        private void FixedUpdate();
        public void DoIt(BaseCombatEntity entity);
        private int GetLeftTime();
        public void GoNext();
        public Mode GetMode();
        private float GetCooldown();
        private void OnDestroy();
        public void Kill();
    }

    private void RegisterPermissions();
    private void LoadImages();
    private static string FixNames(string name);
    private static List<Mode> GetPlayerModes(BasePlayer player);
    private bool IsRaidBlocked(BasePlayer player);
    private bool IsClanMember(ulong playerID, ulong targetID);
    private bool IsFriends(ulong playerID, ulong friendId);
    private static BuildingGrade.Enum GetEnum(Types type);
    private static void RemoveEntity(BasePlayer player, BaseCombatEntity entity);
    private static bool CanRemove(BasePlayer player, BaseEntity entity);
    private static void GiveRefund(BaseCombatEntity entity, BasePlayer player);
    private static void HandleCondition(Item item, BasePlayer player, BaseCombatEntity entity);
    private static void UpgradeBuildingBlock(BuildingBlock block, BuildingGrade.Enum @enum);
    private bool CanAffordUpgrade(List<BuildingBlock> blocks, BuildingGrade.Enum @enum, BasePlayer player);
    private static void PayForUpgrade(List<BuildingBlock> blocks, BuildingGrade.Enum @enum, BasePlayer player);
    private IEnumerator StartUpgrade(BasePlayer player, List<BuildingBlock> blocks, BuildingGrade.Enum @enum);
    private IEnumerator StartRemove(BasePlayer player, List<BaseCombatEntity> entities);
    private static Types ParseType(string arg, Types type);
    private const string CRNotAccess;
    private const string CRBuildingBlock;
    private const string CRBeBlocked;
    private const string CRStorageNotEmpty;
    private const string CRDamaged;
    private const string SuccessfullyRemove;
    private const string CloseMenu;
    private const string UpgradeTitle;
    private const string RemoveTitle;
    private const string UpgradeCanThrough;
    private const string RemoveCanThrough;
    private const string NoPermission;
    private const string SuccessfullyUpgrade;
    private const string NoCupboard;
    private const string CupboardRequired;
    private const string RemoveRaidBlocked;
    private const string UpgradeRaidBlocked;
    private const string BuildingBlocked;
    private const string CantUpgrade;
    private const string CantRemove;
    private const string NotEnoughResources;
    protected override void LoadDefaultMessages();
    private string Msg(string key, string userid, object[] obj);
    private string Msg(BasePlayer player, string key, object[] obj);
    private void Reply(BasePlayer player, string key, object[] obj);
    private void SendNotify(BasePlayer player, string key, int type, object[] obj);
     bool loaded;
     IEnumerator StoreImages();
    private GameObject FileManagerObject;
    private FileManager m_FileManager;
     void InitFileManager();
     class FileManager : MonoBehaviour
    {
         int loaded;
         int needed;
        public bool IsFinished { get; set; }
        const ulong MaxActiveLoads;
         Dictionary<string, FileInfo> files;
         DynamicConfigFile dataFile;
        private class FileInfo
        {
            public string Url;
            public string Png;
        }

        public void SaveData();
        public string GetPng(string name);
        private void Awake();
        public IEnumerator LoadFile(string name, string url, int size);
         IEnumerator LoadImageCoroutine(string name, string url, int size);
        static byte[] Resize(byte[] bytes, int size);
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "Remove Commands")]
    public readonly string[] RemoveCommands;
    [JsonProperty(PropertyName = "Upgrade Commands")]
    public readonly string[] UpgradeCommands;
    [JsonProperty(PropertyName = "Work with Notify?")]
    public readonly bool UseNotify;
    [JsonProperty(PropertyName = "Setting Modes", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly List<Mode> Modes;
    [JsonProperty(PropertyName = "Upgrade Settings")]
    public readonly UpgradeSettings Upgrade;
    [JsonProperty(PropertyName = "Remove Settings")]
    public readonly RemoveSettings Remove;
    [JsonProperty(PropertyName = "Block Settings")]
    public readonly BlockSettings Block;
    [JsonProperty(PropertyName = "Additional Slot Settings")]
    public readonly AdditionalSlot AdditionalSlot;
    [JsonProperty(PropertyName = "UI Settings")]
    public readonly InterfaceSettings UI;
}

private class AdditionalSlot
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled;
}

private class InterfaceSettings
{
    [JsonProperty(PropertyName = "Color 1")]
    public IColor Color1;
    [JsonProperty(PropertyName = "Color 2")]
    public IColor Color2;
    [JsonProperty(PropertyName = "Color 3")]
    public IColor Color3;
    [JsonProperty(PropertyName = "Offset Y")]
    public float OffsetY;
    [JsonProperty(PropertyName = "Offset X")]
    public float OffsetX;
}

private class IColor
{
    [JsonProperty(PropertyName = "HEX")]
    public string Hex;
    [JsonProperty(PropertyName = "Opacity (0 - 100)")]
    public readonly float Alpha;
    [JsonProperty]
    private string _color;
    [JsonIgnore]
    public string Get { get; set; }
    private string GetColor();
    public IColor();
    public IColor(string hex, float alpha);
}

private class ConditionSettings
{
    [JsonProperty(PropertyName = "Default (from game)")]
    public bool Default;
    [JsonProperty(PropertyName = "Use percent?")]
    public bool Percent;
    [JsonProperty(PropertyName = "Percent (value)")]
    public float PercentValue;
}

private class BlockSettings
{
    [JsonProperty(PropertyName = "Work with NoEscape?")]
    public bool UseNoEscape;
    [JsonProperty(PropertyName = "Work with Clans? (clan members will be able to delete/upgrade)")]
    public bool UseClans;
    [JsonProperty(PropertyName = "Work with Friends? (friends will be able to delete/upgrade)")]
    public bool UseFriends;
    [JsonProperty(PropertyName = "Can those authorized in the cupboard delete/upgrade?")]
    public bool UseCupboard;
    [JsonProperty(PropertyName = "Is an upgrade/remove cupbaord required?")]
    public bool NeedCupboard;
}

private abstract class TotalSettings
{
    [JsonProperty(PropertyName = "Time of action")]
    public int ActionTime;
    [JsonProperty(PropertyName = "Cooldown (default | 0 - disable)")]
    public int Cooldown;
    [JsonProperty(PropertyName = "Cooldowns", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VipCooldown;
    [JsonProperty(PropertyName = "Block After Wipe (default | 0 - disable)")]
    public int AfterWipe;
    [JsonProperty(PropertyName = "Block After Wipe", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> VipAfterWipe;
    public int GetCooldown(BasePlayer player);
    public int GetWipeCooldown(BasePlayer player);
}

private class UpgradeSettings : TotalSettings
{
}

private class RemoveSettings : TotalSettings
{
    [JsonProperty(PropertyName = "Blocked items to remove (prefab)",
            ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> BlockedList;
    [JsonProperty(PropertyName = "Return Item")]
    public bool ReturnItem;
    [JsonProperty(PropertyName = "Returnable Item Percentage")]
    public float ReturnPercent;
    [JsonProperty(PropertyName = "Can friends remove? (Friends)")]
    public bool CanFriends;
    [JsonProperty(PropertyName = "Can clanmates remove? (Clans)")]
    public bool CanClan;
    [JsonProperty(PropertyName = "Can teammates remove?")]
    public bool CanTeams;
    [JsonProperty(PropertyName = "Require a cupboard")]
    public bool RequireCupboard;
    [JsonProperty(PropertyName = "Remove by cupboard? (those who are authorized in the cupboard can remove)")]
    public bool RemoveByCupboard;
    [JsonProperty(PropertyName = "Condition Settings")]
    public ConditionSettings Condition;
}

private class Mode
{
    [JsonProperty(PropertyName = "Icon (assets/url)")]
    public string Icon;
    [JsonProperty(PropertyName = "Type (Remove/Wood/Stone/Metal/TopTier)")]
    [JsonConverter(typeof(StringEnumConverter))]
    public Types Type;
    [JsonProperty(PropertyName = "Permission (ex: UPRemove.1)")]
    public string Permission;
}

private class PluginData
{
    [JsonProperty(PropertyName = "Players", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly Dictionary<ulong, PlayerData> Players;
}

private class PlayerData
{
    [JsonProperty(PropertyName = "Last Upgrade")]
    public DateTime LastUpgrade;
    [JsonProperty(PropertyName = "Last Remove")]
    public DateTime LastRemove;
    public int LeftTime(bool remove, int cooldown);
    public bool HasCooldown(bool remove, int cooldown);
    public static bool HasWipeCooldown(int cooldown);
    public static int WipeLeftTime(int cooldown);
}

private class BuildComponent : FacepunchBehaviour
{
    private BasePlayer _player;
    private Mode _mode;
    private float _startTime;
    private readonly CuiElementContainer _container;
    private bool _started;
    private float _cooldown;
    public bool activeByItem;
    private void Awake();
    public void Init(Mode mode);
    public void MainUi();
    private void UpdateUi();
    private void FixedUpdate();
    public void DoIt(BaseCombatEntity entity);
    private int GetLeftTime();
    public void GoNext();
    public Mode GetMode();
    private float GetCooldown();
    private void OnDestroy();
    public void Kill();
}

 class FileManager : MonoBehaviour
{
     int loaded;
     int needed;
    public bool IsFinished { get; set; }
    const ulong MaxActiveLoads;
     Dictionary<string, FileInfo> files;
     DynamicConfigFile dataFile;
    private class FileInfo
    {
        public string Url;
        public string Png;
    }

    public void SaveData();
    public string GetPng(string name);
    private void Awake();
    public IEnumerator LoadFile(string name, string url, int size);
     IEnumerator LoadImageCoroutine(string name, string url, int size);
    static byte[] Resize(byte[] bytes, int size);
}

private class FileInfo
{
    public string Url;
    public string Png;
}


```

---

## UraniumTools

```csharp
using ConVar;
using System.Collections.Generic;
using System;
using UnityEngine;
using System.Linq;
using Oxide.Core.Plugins;
using Newtonsoft.Json;

Oxide.Plugins
[Info("UraniumTools", "BlackPlugin.ru", "1.2.1")]
[Description("Урановые инструменты,добывают сразу готовые ресурсы с возможностью увеличения выпадения,нанося урон радиацией")]
 class UraniumTools : RustPlugin
{
     void OnLoseCondition(Item item, float amount);
    [PluginReference]
     Plugin IQChat;
    private Boolean IsRandom(Single Chance);
     void OnServerInitialized();
    private void ItemSpawnTools(BaseNetworkable entity, LootContainer Container);
    protected override void LoadDefaultConfig();
     void MutationRegistered();
    protected override void LoadConfig();
    [JsonProperty(LanguageEn ? "Date with players tools" : "Дата с инструментами игроков")]
    public List<uint> ItemListBlocked;
    public void SendChat(string Message, BasePlayer player, Chat.ChatChannel channel);
    private void ItemSpawnController(BaseNetworkable entity);
    private const Boolean LanguageEn;
     object OnItemRepair(BasePlayer player, Item item);
     void OnDispenserBonus(ResourceDispenser disp, BasePlayer player, Item item);
    protected override void SaveConfig();
    public List<string> MutationItemList;
    [ConsoleCommand("ut_give")]
     void UraniumToolGive(ConsoleSystem.Arg arg);
     void ReadData();
     bool CanRecycle(Recycler recycler, Item item);
     void OnEntitySpawned(BaseNetworkable entity);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void WriteData();
     void UseTools(Item item, BasePlayer player, string Shortname, ulong SkinID);
    private static Configuration config;
     void CreateItem(BasePlayer player, string Key);
    private Dictionary<ItemDefinition, ItemDefinition> Transmutations;
    private class Configuration
    {
        public static Configuration GetNewConfiguration();
        [JsonProperty( LanguageEn ? "Prefix in chat for message(IQChat)" : "Префикс в чате для сообщения(IQChat)")]
        public string PrefixChat;
        [JsonProperty( LanguageEn ? "Setting up uranium instruments (KEY (must be unique) - SETTING)" : "Настройка урановых инструментов (КЛЮЧ(должен быть уникальный) - НАСТРОЙКА)")]
        public Dictionary<String, Tools> UraniumTools;
        [JsonProperty( LanguageEn ? "Disable Item Recycling" : "Запретить переработку предмета")]
        public bool NoRecycle;
        [JsonProperty( LanguageEn ? "Enable one-time tool repair" : "Включить единоразовую починку инструментов")]
        public bool RepairUse;
        internal class Tools
        {
            [JsonProperty( LanguageEn ? "Crate Name = Rare" : "Название ящика = шанс")]
            public Dictionary<String, Int32> DroppingItems;
            [JsonProperty( LanguageEn ? "Enable item dropout from boxes" : "Включить выпадение предмета из ящиков")]
            public Boolean UseDroppingItem;
            [JsonProperty( LanguageEn ? "Display name" : "Название инструмента")]
            public string Name;
            [JsonProperty( LanguageEn ? "Tool SkinID" : "SkinID инструмента")]
            public ulong SkinID;
            internal class Debuff
            {
                internal class DebuffSetting
                {
                    [JsonProperty( LanguageEn ? "Factor" : "Множитель")]
                    public Single DebuffRate;
                    [JsonProperty( LanguageEn ? "Set the type of debuff on hit: 0 - Radiation, 1 - Cold, 2 - Bleeding, 3 - HP Drain, 4 - Calories, 5 - Hydration" : "Установите тип дебаффа при ударе : 0 - Радиация, 1 - Холод, 2 - Кровотечение, 3 - Снятие ХП, 4 - Калории, 5 - Жажда")]
                    public DebuffType TypesDebuff;
                }

                [JsonProperty( LanguageEn ? "Use debuff on hit? (true - yes/false - no)" : "Использовать дебафф при ударе? (true - да/false - нет)")]
                public Boolean UseDebuff;
                [JsonProperty( LanguageEn ? "Specify what debuffs will be (you can combine them by specifying several types)" : "Укажите какие дебаффы будут (вы можете комбинировать их, указав несколько типов)")]
                public List<DebuffSetting> debuffSetting;
            }

            [JsonProperty( LanguageEn ? "Disable item breakage ? (Item will not wear out)" : "Отключить поломку предмета ? (Предмет не будет изнашиваться)")]
            public bool NotBreaksUse;
            [JsonProperty( LanguageEn ? "Mutation (Will recycle resource when mined) [true/false]" : "Мутация (При добыче будет перерабатывать ресурс) [true/false]")]
            public bool MutationUse;
            [JsonProperty( LanguageEn ? "Shortname Tool" : "Shortname инструмента")]
            public string Shortname;
            public Debuff DebuffVarible;
            [JsonProperty( LanguageEn ? "Increase loot X times per hit" : "Увеличивать добычу в Х раз за удар")]
            public float RateGather;
            [JsonProperty( LanguageEn ? "Resource multiplication (When hit by a tool, it will increase production by X times) [true/false]" : "Умножение ресурсов (При ударе инструментом будет увеличивать добычу в Х раз) [true/false]")]
            public bool RateGatherUse;
        }

        [JsonProperty( LanguageEn ? "Forbid repair" : "Запретить починку")]
        public bool NoRepair;
    }

}

private class Configuration
{
    public static Configuration GetNewConfiguration();
    [JsonProperty( LanguageEn ? "Prefix in chat for message(IQChat)" : "Префикс в чате для сообщения(IQChat)")]
    public string PrefixChat;
    [JsonProperty( LanguageEn ? "Setting up uranium instruments (KEY (must be unique) - SETTING)" : "Настройка урановых инструментов (КЛЮЧ(должен быть уникальный) - НАСТРОЙКА)")]
    public Dictionary<String, Tools> UraniumTools;
    [JsonProperty( LanguageEn ? "Disable Item Recycling" : "Запретить переработку предмета")]
    public bool NoRecycle;
    [JsonProperty( LanguageEn ? "Enable one-time tool repair" : "Включить единоразовую починку инструментов")]
    public bool RepairUse;
    internal class Tools
    {
        [JsonProperty( LanguageEn ? "Crate Name = Rare" : "Название ящика = шанс")]
        public Dictionary<String, Int32> DroppingItems;
        [JsonProperty( LanguageEn ? "Enable item dropout from boxes" : "Включить выпадение предмета из ящиков")]
        public Boolean UseDroppingItem;
        [JsonProperty( LanguageEn ? "Display name" : "Название инструмента")]
        public string Name;
        [JsonProperty( LanguageEn ? "Tool SkinID" : "SkinID инструмента")]
        public ulong SkinID;
        internal class Debuff
        {
            internal class DebuffSetting
            {
                [JsonProperty( LanguageEn ? "Factor" : "Множитель")]
                public Single DebuffRate;
                [JsonProperty( LanguageEn ? "Set the type of debuff on hit: 0 - Radiation, 1 - Cold, 2 - Bleeding, 3 - HP Drain, 4 - Calories, 5 - Hydration" : "Установите тип дебаффа при ударе : 0 - Радиация, 1 - Холод, 2 - Кровотечение, 3 - Снятие ХП, 4 - Калории, 5 - Жажда")]
                public DebuffType TypesDebuff;
            }

            [JsonProperty( LanguageEn ? "Use debuff on hit? (true - yes/false - no)" : "Использовать дебафф при ударе? (true - да/false - нет)")]
            public Boolean UseDebuff;
            [JsonProperty( LanguageEn ? "Specify what debuffs will be (you can combine them by specifying several types)" : "Укажите какие дебаффы будут (вы можете комбинировать их, указав несколько типов)")]
            public List<DebuffSetting> debuffSetting;
        }

        [JsonProperty( LanguageEn ? "Disable item breakage ? (Item will not wear out)" : "Отключить поломку предмета ? (Предмет не будет изнашиваться)")]
        public bool NotBreaksUse;
        [JsonProperty( LanguageEn ? "Mutation (Will recycle resource when mined) [true/false]" : "Мутация (При добыче будет перерабатывать ресурс) [true/false]")]
        public bool MutationUse;
        [JsonProperty( LanguageEn ? "Shortname Tool" : "Shortname инструмента")]
        public string Shortname;
        public Debuff DebuffVarible;
        [JsonProperty( LanguageEn ? "Increase loot X times per hit" : "Увеличивать добычу в Х раз за удар")]
        public float RateGather;
        [JsonProperty( LanguageEn ? "Resource multiplication (When hit by a tool, it will increase production by X times) [true/false]" : "Умножение ресурсов (При ударе инструментом будет увеличивать добычу в Х раз) [true/false]")]
        public bool RateGatherUse;
    }

    [JsonProperty( LanguageEn ? "Forbid repair" : "Запретить починку")]
    public bool NoRepair;
}

internal class Tools
{
    [JsonProperty( LanguageEn ? "Crate Name = Rare" : "Название ящика = шанс")]
    public Dictionary<String, Int32> DroppingItems;
    [JsonProperty( LanguageEn ? "Enable item dropout from boxes" : "Включить выпадение предмета из ящиков")]
    public Boolean UseDroppingItem;
    [JsonProperty( LanguageEn ? "Display name" : "Название инструмента")]
    public string Name;
    [JsonProperty( LanguageEn ? "Tool SkinID" : "SkinID инструмента")]
    public ulong SkinID;
    internal class Debuff
    {
        internal class DebuffSetting
        {
            [JsonProperty( LanguageEn ? "Factor" : "Множитель")]
            public Single DebuffRate;
            [JsonProperty( LanguageEn ? "Set the type of debuff on hit: 0 - Radiation, 1 - Cold, 2 - Bleeding, 3 - HP Drain, 4 - Calories, 5 - Hydration" : "Установите тип дебаффа при ударе : 0 - Радиация, 1 - Холод, 2 - Кровотечение, 3 - Снятие ХП, 4 - Калории, 5 - Жажда")]
            public DebuffType TypesDebuff;
        }

        [JsonProperty( LanguageEn ? "Use debuff on hit? (true - yes/false - no)" : "Использовать дебафф при ударе? (true - да/false - нет)")]
        public Boolean UseDebuff;
        [JsonProperty( LanguageEn ? "Specify what debuffs will be (you can combine them by specifying several types)" : "Укажите какие дебаффы будут (вы можете комбинировать их, указав несколько типов)")]
        public List<DebuffSetting> debuffSetting;
    }

    [JsonProperty( LanguageEn ? "Disable item breakage ? (Item will not wear out)" : "Отключить поломку предмета ? (Предмет не будет изнашиваться)")]
    public bool NotBreaksUse;
    [JsonProperty( LanguageEn ? "Mutation (Will recycle resource when mined) [true/false]" : "Мутация (При добыче будет перерабатывать ресурс) [true/false]")]
    public bool MutationUse;
    [JsonProperty( LanguageEn ? "Shortname Tool" : "Shortname инструмента")]
    public string Shortname;
    public Debuff DebuffVarible;
    [JsonProperty( LanguageEn ? "Increase loot X times per hit" : "Увеличивать добычу в Х раз за удар")]
    public float RateGather;
    [JsonProperty( LanguageEn ? "Resource multiplication (When hit by a tool, it will increase production by X times) [true/false]" : "Умножение ресурсов (При ударе инструментом будет увеличивать добычу в Х раз) [true/false]")]
    public bool RateGatherUse;
}

internal class Debuff
{
    internal class DebuffSetting
    {
        [JsonProperty( LanguageEn ? "Factor" : "Множитель")]
        public Single DebuffRate;
        [JsonProperty( LanguageEn ? "Set the type of debuff on hit: 0 - Radiation, 1 - Cold, 2 - Bleeding, 3 - HP Drain, 4 - Calories, 5 - Hydration" : "Установите тип дебаффа при ударе : 0 - Радиация, 1 - Холод, 2 - Кровотечение, 3 - Снятие ХП, 4 - Калории, 5 - Жажда")]
        public DebuffType TypesDebuff;
    }

    [JsonProperty( LanguageEn ? "Use debuff on hit? (true - yes/false - no)" : "Использовать дебафф при ударе? (true - да/false - нет)")]
    public Boolean UseDebuff;
    [JsonProperty( LanguageEn ? "Specify what debuffs will be (you can combine them by specifying several types)" : "Укажите какие дебаффы будут (вы можете комбинировать их, указав несколько типов)")]
    public List<DebuffSetting> debuffSetting;
}

internal class DebuffSetting
{
    [JsonProperty( LanguageEn ? "Factor" : "Множитель")]
    public Single DebuffRate;
    [JsonProperty( LanguageEn ? "Set the type of debuff on hit: 0 - Radiation, 1 - Cold, 2 - Bleeding, 3 - HP Drain, 4 - Calories, 5 - Hydration" : "Установите тип дебаффа при ударе : 0 - Радиация, 1 - Холод, 2 - Кровотечение, 3 - Снятие ХП, 4 - Калории, 5 - Жажда")]
    public DebuffType TypesDebuff;
}


```

---

## UsableToBelt

```csharp

Oxide.Plugins
[Info("UsableToBelt", "Wulf/lukespragg", "1.2.1", ResourceId = 1141)]
[Description("Any usable item will be moved to your belt if there is space")]
 class UsableToBelt : RustPlugin
{
    const string permAllow;
     void Init();
     void HandleItem(Item item, BasePlayer player);
     void OnItemAddedToContainer(ItemContainer container, Item item);
}


```

---

## Vanish

```csharp
using System.Collections.Generic;
using System.Linq;
using Network;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Vanish", "GrazyCat", "0.7.1")]
[Description("Ваниш как у москвы , и даже круче ! ")]
public class Vanish : RustPlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Включить звуковой эффект ? (true/false)")]
        public bool PlaySoundEffect;
        [JsonProperty(PropertyName = "Показать индикатор невдимости ? (true/false)")]
        public bool ShowGuiIcon;
        [JsonProperty(PropertyName = "Включить видимость для админов ? (true/false)")]
        public bool VisibleToAdmin;
        [JsonProperty(PropertyName = "Выключить режим призрака  ? (true/false)")]
        public bool Ghost;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private const string defaultEffect;
    private const string permGhostOff;
    private const string permUse;
    private void Init();
    private void Subscribe();
    private void Unsubscribe();
    private class OnlinePlayer
    {
        public BasePlayer Player;
        public bool IsInvisible;
    }

    [OnlinePlayers]
    private Hash<BasePlayer, OnlinePlayer> onlinePlayers;
    private void VanishChatCmd(IPlayer player, string command, string[] args);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void StopLooting(BasePlayer player);
     void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
     void OnItemDropped(BasePlayer player, Item item);
    private void Disappear(BasePlayer basePlayer);
    private object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
    private object CanBeTargeted(BaseCombatEntity entity);
    private object CanBradleyApcTarget(BradleyAPC apc, BaseEntity entity);
    private object CanHelicopterTarget(PatrolHelicopterAI heli, BasePlayer basePlayer);
    private object OnNpcPlayerTarget(NPCPlayerApex npc, BaseEntity entity);
    private object OnNpcTarget(BaseNpc npc, BaseEntity entity);
    private void OnPlayerSleepEnded(BasePlayer basePlayer);
    private object OnPlayerLand(BasePlayer player, float num);
    private object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
    private object CanBuild(Planner plan, Construction prefab);
    private object OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private object OnCupboardClearList(BuildingPrivlidge privilege, BasePlayer player);
    private object OnCupboardDeauthorize(BuildingPrivlidge privilege, BasePlayer player);
    private object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
    private object OnStructureDemolish(BaseCombatEntity entity, BasePlayer player);
    private object OnTurretAuthorize(AutoTurret turret, BasePlayer player);
    private object OnItemPickup(Item item, BasePlayer player);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private object CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
    private void Reappear(BasePlayer basePlayer);
    private Dictionary<ulong, string> guiInfo;
    private void VanishGui(BasePlayer basePlayer);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerTick(BasePlayer basePlayer);
    private void Unload();
    private void AddCommandAliases(string key, string command);
    private string Lang(string key, string id, object[] args);
    private void Message(BasePlayer player, string key, object[] args);
    private bool IsInvisible(BasePlayer player);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Включить звуковой эффект ? (true/false)")]
    public bool PlaySoundEffect;
    [JsonProperty(PropertyName = "Показать индикатор невдимости ? (true/false)")]
    public bool ShowGuiIcon;
    [JsonProperty(PropertyName = "Включить видимость для админов ? (true/false)")]
    public bool VisibleToAdmin;
    [JsonProperty(PropertyName = "Выключить режим призрака  ? (true/false)")]
    public bool Ghost;
    public static Configuration DefaultConfig();
}

private class OnlinePlayer
{
    public BasePlayer Player;
    public bool IsInvisible;
}


```

---

## VehicleBuy-2.1.4

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Plugins.VehicleBuyExtensionMethods;
using Rust;
using Rust.Modular;
using UnityEngine;
using UnityEngine.Networking;
using UnityEngine.UI;
using Physics = UnityEngine.Physics;
using Random = UnityEngine.Random;
using Time = UnityEngine.Time;

Oxide.Plugins
[Info("Purchase Vehicles from the Shop", "M&B-Studios & Mevent", "2.1.4")]
internal class VehicleBuy : RustPlugin
{
    private class LockedVehicleTracker
    {
        public Dictionary<VehicleInfo, HashSet<BaseEntity>> VehiclesWithLocksByType { get; set; }
        private readonly VehicleInfoManager _vehicleInfoManager;
        public LockedVehicleTracker(VehicleInfoManager vehicleInfoManager);
        public void OnServerInitialized();
        public void OnLockAdded(BaseEntity vehicle);
        public void OnLockRemoved(BaseEntity vehicle);
        private HashSet<BaseEntity> EnsureEntityList(VehicleInfo vehicleInfo);
        private HashSet<BaseEntity> GetEntityListForVehicle(BaseEntity entity);
    }

    private const float MaxDeployDistance;
    private class VehicleInfo
    {
        public string VehicleType;
        public string[] PrefabPaths;
        public Vector3 LockPosition;
        public Quaternion LockRotation;
        public string ParentBone;
        public string CodeLockPermission { get; set; }
        public string KeyLockPermission { get; set; }
        public uint[] PrefabIds { get; set; }
        public Func<BaseEntity, BaseEntity> DetermineLockParent;
        public Func<BaseEntity, float> TimeSinceLastUsed;
        public void OnServerInitialized();
        public bool IsMounted(BaseEntity entity);
    }

    private class VehicleInfoManager
    {
        private readonly Dictionary<uint, VehicleInfo> _prefabIdToVehicleInfo;
        private readonly Dictionary<string, VehicleInfo> _customVehicleTypes;
        public void OnServerInitialized();
        public void RegisterCustomVehicleType(VehicleInfo vehicleInfo);
        public VehicleInfo GetVehicleInfo(BaseEntity entity);
        public BaseEntity GetCustomVehicleParent(BaseEntity entity);
    }

    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Economics;
    private Plugin ServerRewards;
    private Plugin BankSystem;
    private static VehicleBuy Instance;
    private bool _enabledImageLibrary;
    private Dictionary<ulong, ulong> PRM;
    private const bool LangRu;
    private const string Prefab_CodeLock_DeployedEffect;
    private const string Prefab_CodeLock_DeniedEffect;
    private const string Prefab_CodeLock_UnlockEffect;
    private const string VENDINGMACHINE_PREFAB;
    private const string PERM_USE;
    private const string PERM_FREE;
    private const string PERM_PICKUP;
    private const string PERM_RECALL;
    private const string PURCHASE_EFFECT;
    private VehicleInfoManager _vehicleInfoManager;
    private LockedVehicleTracker _lockedVehicleTracker;
    private List<BaseEntity> VendingMachines;
    private VendingMachinePosition BANDITCAMP_POSITION;
    private VendingMachinePosition OUTPOST_POSITION;
    private VendingMachinePosition fvA_POSITION;
    private VendingMachinePosition fvB_POSITION;
    private VendingMachinePosition fvC_POSITION;
    private MonumentInfo Outpost;
    private MonumentInfo BanditCamp;
    private List<MonumentInfo> FishingVillagesA;
    private List<MonumentInfo> FishingVillagesB;
    private List<MonumentInfo> FishingVillagesC;
    private Dictionary<ulong, PlayerData> _players;
    internal class PlayerData
    {
        public Dictionary<string, int> Cooldowns;
        public Dictionary<string, int> PurchasedVehicles;
    }

    private bool CanPlayerBypassLock(BasePlayer player, BaseLock baseLock, bool provideFeedback);
    private static VehicleModuleSeating FindFirstDriverModule(ModularCar car);
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    [ChatCommand("getposfva")]
    private void cmdGetPosfva(BasePlayer player);
    [ChatCommand("getposfvb")]
    private void cmdGetPosfvb(BasePlayer player);
    [ChatCommand("getposfvc")]
    private void cmdGetPosfvc(BasePlayer player);
    [ChatCommand("getposoutpost")]
    private void cmdGetPosOP(BasePlayer player);
    [ChatCommand("getposbandit")]
    private void cmdGetPosBC(BasePlayer player);
    private object OnGiveSoldItem(VendingMachine vending, Item soldItem, BasePlayer buyer);
    private object CanAdministerVending(BasePlayer player, VendingMachine machine);
    private void SpawnVendingMachines();
    private static void SendEffect(BasePlayer player, string effect);
    private void SetupVendingMachine(BaseEntity machine, VendingMachine comp, List<VMOrder> items);
    internal class VendingMachineCache
    {
        public string RandomShortname;
        public string VehicleKey;
        public VehicleInfoConfig Get();
    }

    private Dictionary<NetworkableId, IEnumerable<VendingMachineCache>> VMProducts;
    private void DestroyVendingMachines();
    private BaseEntity SpawnVendingMachine(VendingMachinePosition position);
    private BaseEntity SpawnVendingMachine(VendingMachinePosition position, Transform transform);
    private static string[] FindPrefabsOfType();
    private int GetVehiclePurchased(BasePlayer player, string key);
    private void SetVehiclePurchased(BasePlayer player, string key);
    private void SetCooldown(BasePlayer player, string key, int cooldownTime);
    private int GetCooldown(BasePlayer player, string key);
    private bool IsCooldown(BasePlayer player, string key);
    private int ItemsCount(BasePlayer player, string shortname);
    private bool CanBuy(BasePlayer player, VehicleInfoConfig product, string key);
    private bool Collect(BasePlayer player, VehicleInfoConfig product, string key);
    private string GetRemainingCost(BasePlayer player, VehicleInfoConfig product);
    private int GetBalance(BasePlayer player, VehicleInfoConfig product);
    private void Withdraw(BasePlayer player, int price, int type, string shortname);
    private string GetCost(string key);
    private Dictionary<string, string> _loadedImages;
    private void AddImage(string url, string fileName, ulong imageId);
    private string GetImage(string name);
    private bool HasImage(string name);
    private void LoadImages();
    private void RegisterImage(Dictionary<string, string> images, string name, string image);
    private void BroadcastILNotInstalled();
    private void LoadImageFromFS(string name, string path);
    private IEnumerator LoadImage(string name, string path);
    private void RegisterPermissions();
    public void RegisterCommands();
    private bool GiveVehicle(ItemContainer container, VehicleInfoConfig vehicleSettings, bool needFuel);
    private void GetOutParts(ModularCar car);
    private void AddEngineParts(ModularCar car, List<string> shortnames);
    private void AddPartsToEngineStorage(EngineStorage engineStorage, string shortname);
    private bool TryAddEngineItem(EngineStorage engineStorage, int slot, string shortname);
    private bool GiveVehicle(BasePlayer player, VehicleInfoConfig vehicleSettings, bool needfuel);
    private BaseEntity GetMinDistance(Vector3 position, IEnumerable<BaseEntity> entities);
    private bool HasInConfig(BaseEntity entity, string key);
    private const string Layer;
    private void LoadSetupUI();
    private FullscreenSetup UIFulScreen;
    private void LoadFullscreenUI();
    private void SaveFullscreenUI();
    private UserInterface UIMenu;
    private void LoadMenuUI();
    private void SaveMenuUI();
    private void LoadDataFromFile(T data, string filePath);
    private void SaveDataToFile(T data, string filePath);
    private const string MAX_PURCHASES_EXCEEDED;
    private const string BUY_VEHICLE;
    private const string BUTTON_BUY;
    protected override void LoadDefaultMessages();
    public Dictionary<string, LangCustom> GetCustomLang();
    public Dictionary<string, LangCustom> GetDefaultCustomLang();
    public static string GetMessage(string key, string userId, object[] args);
    public void OpenVehicleBuy(BasePlayer player);
    private CuiElementContainer API_OpenPlugin(BasePlayer player);
    public void AddContentVehicleBuy(BasePlayer player, CuiElementContainer container, UserInterface setup);
    public string GetNameCurrency(VehicleInfoConfig vehicleInfo);
    private static List<ulong> _customents;
    private static void GetNewEnts();
    public class FullscreenSetup : UserInterface
    {
        [JsonProperty(LangRu ? "Цвет фона экрана" : "Screen background color")]
        public IColor BackgroundScreen;
        [JsonProperty("Material")]
        public string Material;
        [JsonProperty("Sprite")]
        public string Sprite;
        [JsonProperty(LangRu ? "Кнопка закрытия" : "Button Close")]
        public ButtonCloseSetup ButtonClose;
    }

    public class ButtonCloseSetup : InterfacePosition
    {
        [JsonProperty(PropertyName = "Background Button Close")]
        public ImageSettings BackgroundClose;
        [JsonProperty(PropertyName = "Color Button Close")]
        public IColor Color;
        [JsonProperty(PropertyName = "Sprite Button Close")]
        public string Sprite;
        [JsonProperty(PropertyName = "Material Button Close")]
        public string Material;
    }

    public class UserInterface
    {
        [JsonProperty(PropertyName = LangRu ? "Установлена?" : "Installed?")]
        public bool Installed;
        [JsonProperty(PropertyName = LangRu ? "Высота главной панели" : "Height of the main panel")]
        public float Height;
        [JsonProperty(PropertyName = LangRu ? "Ширина главной панели" : "Width of the main panel")]
        public float Width;
        [JsonProperty(PropertyName = LangRu ? "Высота лота" : "Lot height")]
        public float LotHeight;
        [JsonProperty(PropertyName = LangRu ? "Ширина лота" : "Lot width")]
        public float LotWidth;
        [JsonProperty(PropertyName = LangRu ? "Кол-во лотов на строке" : "Number of lots per line")]
        public int LotsOnString;
        [JsonProperty(PropertyName = LangRu ? "Отступы по горизонтали" : "Horizontal margins")]
        public float XIndent;
        [JsonProperty(PropertyName = LangRu ? "Отступы по вертикали" : "Vertical margins")]
        public float YIndent;
        [JsonProperty(PropertyName = "Background main panel")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = LangRu ? "Настройки заголовка" : "Header Settings")]
        public PanelHeaderUI HeaderPanel;
        [JsonProperty(PropertyName = LangRu ? "Настройки панели лотов" : "Lot Panel Settings")]
        public PanelContentUI ContentPanel;
        [JsonProperty(PropertyName = LangRu ? "Настройки лота" : "Lot Settings")]
        public PanelLotUI LotPanel;
        public class PanelHeaderUI
        {
            [JsonProperty(PropertyName = "Background")]
            public ImageSettings Background;
            [JsonProperty(PropertyName = "Title")]
            public TextSettings Title;
            [JsonProperty(PropertyName = "Show Line?")]
            public bool ShowLine;
            [JsonProperty(PropertyName = "Line")]
            public ImageSettings Line;
        }

        public class PanelContentUI
        {
            [JsonProperty(PropertyName = "Background")]
            public ImageSettings Background;
            [JsonProperty(PropertyName = "Scroll View")]
            public ScrollViewUI Scroll;
        }

        public class PanelLotUI
        {
            [JsonProperty(PropertyName = "Background")]
            public ImageSettings Background;
            [JsonProperty(PropertyName = "Lot Image")]
            public InterfacePosition LotImage;
            [JsonProperty(PropertyName = "Show Name?")]
            public bool ShowName;
            [JsonProperty(PropertyName = "Lot Name")]
            public TextSettings LotName;
            [JsonProperty(PropertyName = "Lot Currency")]
            public TextSettings CurrencyName;
            [JsonProperty(PropertyName = "Lot Price")]
            public TextSettings PriceText;
            [JsonProperty(PropertyName = "Lot Button Buy")]
            public ButtonSettings ButtonBuy;
        }

        public static FullscreenSetup GenerateFullScreen();
        public static UserInterface GenerateUIMenuV1();
        public static UserInterface GenerateUIMenuV2();
    }

    [ChatCommand("callback")]
    private void cmdCallback(BasePlayer player, string command, string[] args);
    [ChatCommand("pickup")]
    private void cmdPickup(BasePlayer player);
    [ConsoleCommand("vehiclebuy.template")]
    private void CmdConsoleTemplate(ConsoleSystem.Arg arg);
    [ConsoleCommand("vehiclebuy.images")]
    private void CmdConsoleImages(ConsoleSystem.Arg arg);
    [ConsoleCommand("vb_buy")]
    private void cmdBuy(ConsoleSystem.Arg arg);
    private void cmdVehicleBuy(BasePlayer player, string command, string[] args);
    private void SendMessage(IPlayer player, string message);
    private void CmdAddVehicle(IPlayer user, string command, string[] args);
    private static BaseLock GetVehicleLock(BaseEntity vehicle);
    private bool IsPlayerAuthorizedToCodeLock(ulong userID, CodeLock codeLock);
    private bool IsPlayerAuthorizedToLock(BasePlayer player, BaseLock baseLock);
    private object CanPlayerInteractWithVehicle(BasePlayer player, BaseEntity vehicle, bool provideFeedback);
    private BaseEntity GetParentVehicle(BaseEntity entity);
    private object CanPlayerInteractWithParentVehicle(BasePlayer player, BaseEntity entity, bool provideFeedback);
    private class LockInfo
    {
        public int ItemId;
        public string Prefab;
        public string PreHookName;
        public ItemDefinition ItemDefinition { get; set; }
        public ItemBlueprint Blueprint { get; set; }
    }

    private readonly LockInfo LockInfo_CodeLock;
    private readonly LockInfo LockInfo_KeyLock;
    private object CanMountEntity(BasePlayer player, BaseMountable entity);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private object CanLootEntity(BasePlayer player, ContainerIOEntity container);
    private object CanLootEntity(BasePlayer player, RidableHorse horse);
    private object CanLootEntity(BasePlayer player, ModularCarGarage carLift);
    private object OnHorseLead(RidableHorse horse, BasePlayer player);
    private object OnHotAirBalloonToggle(HotAirBalloon hab, BasePlayer player);
    private object OnSwitchToggle(ElectricSwitch electricSwitch, BasePlayer player);
    private object OnTurretAuthorize(AutoTurret entity, BasePlayer player);
    private object OnTurretTarget(AutoTurret autoTurret, BasePlayer player);
    private object CanSwapToSeat(BasePlayer player, ModularCarSeat carSeat);
    private object OnVehiclePush(BaseVehicle vehicle, BasePlayer player);
    private void OnEntityKill(BaseLock baseLock);
    private void OnEntityTakeDamage(BaseEntity entity, HitInfo info);
    private void OnEntityKill(VehicleModuleSeating seatingModule);
    private object CanDeployItem(BasePlayer basePlayer, Deployer deployer, NetworkableId entityId);
    private BaseLock DeployLock(BaseEntity vehicle, VehicleInfo vehicleInfo, LockInfo lockInfo, ulong ownerId);
    private BaseLock DeployLockForPlayer(BaseEntity vehicle, VehicleInfo vehicleInfo, LockInfo lockInfo, BasePlayer player);
    private static void ClaimVehicle(BaseEntity vehicle, ulong ownerId);
    private static Item GetPlayerLockItem(BasePlayer player, LockInfo lockInfo);
    private bool VerifyDeployDistance(IPlayer player, BaseEntity vehicle);
    private static bool IsDead(BaseEntity entity);
    private bool VerifyVehicleIsNotDead(IPlayer player, BaseEntity vehicle);
    private bool VerifyNotForSale(IPlayer player, BaseEntity vehicle);
    private bool AllowNoOwner(BaseEntity vehicle);
    private bool AllowDifferentOwner(IPlayer player, BaseEntity vehicle);
    private bool VerifyNoOwnershipRestriction(IPlayer player, BaseEntity vehicle);
    private bool VerifyCanBuild(IPlayer player, BaseEntity vehicle);
    private bool VerifyVehicleHasNoLock(IPlayer player, BaseEntity vehicle);
    private bool VerifyVehicleCanHaveALock(IPlayer player, BaseEntity vehicle);
    private static bool CanCarHaveLock(ModularCar car);
    private static bool CanVehicleHaveALock(BaseEntity vehicle);
    private bool VerifyNotMounted(IPlayer player, BaseEntity vehicle, VehicleInfo vehicleInfo);
    private bool VerifyCanDeploy(IPlayer player, BaseEntity vehicle, VehicleInfo vehicleInfo, LockInfo lockInfo);
    private static bool DeployWasBlocked(BaseEntity vehicle, BasePlayer player, LockInfo lockInfo);
    private static RidableHorse2 GetClosestHorse(HitchTrough hitchTrough, BasePlayer player);
    private static BaseEntity GetVehicleFromEntity(BaseEntity entity, BasePlayer basePlayer);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    private void SpawnRecycler(BaseEntity entity, BasePlayer player, Vector3 position, Quaternion rotation, string prefab, ulong skin);
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private bool CanPlaceVehicle(Vector3 pos, float radius);
    private bool CanPlaceRecycler(BaseEntity entity, BasePlayer player);
    private bool IsInMonument(Vector3 position);
    private object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
    private object CanStackItem(Item item, Item targetItem);
    private static string HexToCuiColor(string hex, float alpha);
    private Vector3 GetPositionFromPlayer(BasePlayer player, float distance);
    private ConfigData _config;
    internal class VehicleInfoConfig
    {
        [JsonProperty("Sound on purchase", Order = 0)]
        public bool UseSoundOnPurchase;
        [JsonProperty("Order", Order = -1)]
        public int Order;
        [JsonProperty("Show", Order = 0)]
        public bool Show;
        [JsonProperty("Name", Order = 1)]
        public string Name;
        [JsonProperty("Prefab", Order = 2)]
        public string Prefab;
        [JsonProperty("Image link", Order = 3)]
        public string Image;
        [JsonProperty("Spawn distance", Order = 4)]
        public float SpawnDistance;
        [JsonProperty("Fuel", Order = 5)]
        public int Fuel;
        [JsonProperty("Currency: 0 - item, 1 - Economics, 2 - Server Rewards", Order = 6)]
        public byte SellCurrency;
        [JsonProperty("If vehicle selling for item type him shortname", Order = 7)]
        public string Shortname;
        [JsonProperty("Price", Order = 8)]
        public int Price;
        [JsonProperty("Skin", Order = 9)]
        public ulong Skin;
        [JsonProperty("Command", Order = 10)]
        public string Command;
        [JsonProperty("DeployableItemId", Order = 11)]
        public int DeployableItemId;
        [JsonProperty("Need add engine parts if it possible?", Order = 12)]
        public bool NeedCarParts;
        [JsonProperty("Engine parts", Order = 13)]
        public List<string> EngineParts;
        [JsonProperty("Cooldown to buy (in seconds)", Order = -2)]
        public int Cooldown;
        [JsonProperty("Pickup type (0 - command, 1 - hammer)", Order = -3)]
        public int PickupType;
        [JsonProperty("Can pickup?", Order = -4)]
        public bool CanPickup;
        [JsonProperty("Can recall?", Order = -5)]
        public bool CanCallback;
        [JsonProperty("Recall price", Order = -6)]
        public int RecallCost;
        [JsonProperty("Recall cost need?", Order = -7)]
        public bool RecallCostNeed;
        [JsonProperty("Pickup price", Order = -8)]
        public int PickupPrice;
        [JsonProperty("Enable decay?", Order = -9)]
        public bool EnableDecay;
        [JsonProperty("Permission (still empty if not need) ex. vehiclebuy.YOURPERMISSIONNAME", Order = -10)]
        public string Permission;
        [JsonProperty("Maximum number of purchases of one vehicle by one player", Order = -11)]
        public int MaxPurchases;
        public bool CanPlayerBuy(BasePlayer player);
    }

    internal class VendingMachinesConfig
    {
        [JsonProperty("Bandit Camp vending machine")]
        public bool BanditCampSpawnMachine;
        [JsonProperty("Outpost vending machine")]
        public bool OutpostSpawnMachine;
        [JsonProperty("Bandit Camp products")]
        public List<VMOrder> BanditCampOrders;
        [JsonProperty("Outpost products")]
        public List<VMOrder> OutpostOrders;
        [JsonProperty("Fishing village A vending machine")]
        public bool FishingVillageASpawnMachine;
        [JsonProperty("Fishing village B vending machine")]
        public bool FishingVillageBSpawnMachine;
        [JsonProperty("Fishing village C vending machine")]
        public bool FishingVillageCSpawnMachine;
        [JsonProperty("Fishing Village C products")]
        public List<VMOrder> FishingVillageCOrders;
        [JsonProperty("Fishing Village A products")]
        public List<VMOrder> FishingVillageAOrders;
        [JsonProperty("Fishing Village B products")]
        public List<VMOrder> FishingVillageBOrders;
    }

    internal class VMOrder
    {
        [JsonProperty("Vehicle key from config")]
        public string VehicleKey;
        [JsonProperty("Item (shortname)")]
        public string Shortname;
        [JsonProperty("Price")]
        public int Price;
        public VehicleInfoConfig GetVehicle();
    }

    public class ConfigData
    {
        [JsonProperty("Commands", Order = -1)]
        public List<string> Commands;
        [JsonProperty("Currency name economics", Order = 0)]
        public string CurrencyName;
        [JsonProperty("Currency name Server Rewards", Order = 1)]
        public string CurrencyNameSR;
        [JsonProperty("Currency name Bank System", Order = 1)]
        public string CurrencyNameBS;
        [JsonProperty("Pickup distance", Order = 2)]
        public float PickupRadius;
        [JsonProperty("Disable vehicles damage?", Order = 2)]
        public bool DisableVehiclesDamage;
        [JsonProperty("Vending machines", Order = 2)]
        public VendingMachinesConfig VendingMachines;
        [JsonProperty("Vehicles", Order = 3)]
        public Dictionary<string, VehicleInfoConfig> Vehicles;
        [JsonProperty(Order = 200)]
        public VersionNumber Version;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void SaveConfig(object config);
    private void SaveData();
    private void LoadData();
    public class ScrollViewUI
    {
        [JsonProperty(PropertyName = "Scroll Type (Horizontal, Vertical)")]
        [JsonConverter(typeof(StringEnumConverter))]
        public ScrollType ScrollType;
        [JsonProperty(PropertyName = "Movement Type (Unrestricted, Elastic, Clamped)")]
        [JsonConverter(typeof(StringEnumConverter))]
        public ScrollRect.MovementType MovementType;
        [JsonProperty(PropertyName = "Elasticity")]
        public float Elasticity;
        [JsonProperty(PropertyName = "Deceleration Rate")]
        public float DecelerationRate;
        [JsonProperty(PropertyName = "Scroll Sensitivity")]
        public float ScrollSensitivity;
        [JsonProperty(PropertyName = "Minimal Height")]
        public float MinHeight;
        [JsonProperty(PropertyName = "Additional Height")]
        public float AdditionalHeight;
        [JsonProperty(PropertyName = "Scrollbar Settings")]
        public ScrollBarSettings Scrollbar;
        public CuiScrollViewComponent GetScrollView(float totalWidth);
        public CuiScrollViewComponent GetScrollView(CuiRectTransform contentTransform);
        public CuiRectTransform CalculateContentRectTransform(float totalWidth);
        public class ScrollBarSettings
        {
            [JsonProperty(PropertyName = "Invert")]
            public bool Invert;
            [JsonProperty(PropertyName = "Auto Hide")]
            public bool AutoHide;
            [JsonProperty(PropertyName = "Handle Sprite")]
            public string HandleSprite;
            [JsonProperty(PropertyName = "Size")]
            public float Size;
            [JsonProperty(PropertyName = "Handle Color")]
            public IColor HandleColor;
            [JsonProperty(PropertyName = "Highlight Color")]
            public IColor HighlightColor;
            [JsonProperty(PropertyName = "Pressed Color")]
            public IColor PressedColor;
            [JsonProperty(PropertyName = "Track Sprite")]
            public string TrackSprite;
            [JsonProperty(PropertyName = "Track Color")]
            public IColor TrackColor;
            public CuiScrollbar Get();
        }

    }

    public class ImageSettings : InterfacePosition
    {
        [JsonProperty(PropertyName = "Sprite")]
        public string Sprite;
        [JsonProperty(PropertyName = "Material")]
        public string Material;
        [JsonProperty(PropertyName = "Image")]
        public string Image;
        [JsonProperty(PropertyName = "Color")]
        public IColor Color;
        [JsonProperty(PropertyName = "Cursor Enabled")]
        public bool CursorEnabled;
        [JsonProperty(PropertyName = "Keyboard Enabled")]
        public bool KeyboardEnabled;
        [JsonIgnore]
        private ICuiComponent _imageComponent;
        public ICuiComponent GetImageComponent();
        public bool TryGetImageURL(string url);
        public CuiElement GetImage(string parent, string name, string destroyUI);
        public ImageSettings();
        public ImageSettings(string imageURL, IColor color, InterfacePosition position);
    }

    public class ButtonSettings : TextSettings
    {
        [JsonProperty(PropertyName = "Button Color")]
        public IColor ButtonColor;
        [JsonProperty(PropertyName = "Sprite")]
        public string Sprite;
        [JsonProperty(PropertyName = "Material")]
        public string Material;
        [JsonProperty(PropertyName = "Image")]
        public string Image;
        [JsonProperty(PropertyName = "Image Color")]
        public IColor ImageColor;
        [JsonProperty(PropertyName = "Use custom image position settings?")]
        public bool UseCustomPositionImage;
        [JsonProperty(PropertyName = "Custom image position settings")]
        public InterfacePosition ImagePosition;
        public bool TryGetImageURL(string url);
        public List<CuiElement> GetButton(string msg, string cmd, string parent, string name, string destroyUI, string close);
    }

    public class TextSettings : InterfacePosition
    {
        [JsonProperty(PropertyName = "Font Size")]
        public int FontSize;
        [JsonProperty(PropertyName = "Is Bold?")]
        public bool IsBold;
        [JsonProperty(PropertyName = "Align")]
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor Align;
        [JsonProperty(PropertyName = "Color")]
        public IColor Color;
        public CuiTextComponent GetTextComponent(string msg);
        public CuiElement GetText(string msg, string parent, string name, string destroyUI);
    }

    public class InterfacePosition
    {
        [JsonProperty(PropertyName = "AnchorMin")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "AnchorMax")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "OffsetMin")]
        public string OffsetMin;
        [JsonProperty(PropertyName = "OffsetMax")]
        public string OffsetMax;
        [JsonIgnore]
        private CuiRectTransformComponent _position;
        public CuiRectTransformComponent GetRectTransform();
        public InterfacePosition();
        public InterfacePosition(InterfacePosition other);
        public static InterfacePosition CreatePosition(float aMinX, float aMinY, float aMaxX, float aMaxY, float oMinX, float oMinY, float oMaxX, float oMaxY);
        public static InterfacePosition CreatePosition(string anchorMin, string anchorMax, string offsetMin, string offsetMax);
        public static InterfacePosition CreatePosition(CuiRectTransform rectTransform);
        public static InterfacePosition CreateFullStretch();
        public static InterfacePosition CreateCenter();
    }

    public class IColor
    {
        [JsonProperty(PropertyName = "HEX")]
        public string HEX;
        [JsonProperty(PropertyName = LangRu ? "Непрозрачность (0 - 100)" : "Opacity (0 - 100)")]
        public float Alpha;
        [JsonIgnore]
        private string _cachedResult;
        [JsonIgnore]
        private bool _isCached;
        public string Get();
        public IColor();
        public IColor(string hex, float alpha);
        public static IColor Create(string hex, float alpha);
        public static IColor CreateTransparent();
        public static IColor CreateWhite();
        public static IColor CreateBlack();
    }

}

private class LockedVehicleTracker
{
    public Dictionary<VehicleInfo, HashSet<BaseEntity>> VehiclesWithLocksByType { get; set; }
    private readonly VehicleInfoManager _vehicleInfoManager;
    public LockedVehicleTracker(VehicleInfoManager vehicleInfoManager);
    public void OnServerInitialized();
    public void OnLockAdded(BaseEntity vehicle);
    public void OnLockRemoved(BaseEntity vehicle);
    private HashSet<BaseEntity> EnsureEntityList(VehicleInfo vehicleInfo);
    private HashSet<BaseEntity> GetEntityListForVehicle(BaseEntity entity);
}

private class VehicleInfo
{
    public string VehicleType;
    public string[] PrefabPaths;
    public Vector3 LockPosition;
    public Quaternion LockRotation;
    public string ParentBone;
    public string CodeLockPermission { get; set; }
    public string KeyLockPermission { get; set; }
    public uint[] PrefabIds { get; set; }
    public Func<BaseEntity, BaseEntity> DetermineLockParent;
    public Func<BaseEntity, float> TimeSinceLastUsed;
    public void OnServerInitialized();
    public bool IsMounted(BaseEntity entity);
}

private class VehicleInfoManager
{
    private readonly Dictionary<uint, VehicleInfo> _prefabIdToVehicleInfo;
    private readonly Dictionary<string, VehicleInfo> _customVehicleTypes;
    public void OnServerInitialized();
    public void RegisterCustomVehicleType(VehicleInfo vehicleInfo);
    public VehicleInfo GetVehicleInfo(BaseEntity entity);
    public BaseEntity GetCustomVehicleParent(BaseEntity entity);
}

internal class PlayerData
{
    public Dictionary<string, int> Cooldowns;
    public Dictionary<string, int> PurchasedVehicles;
}

internal class VendingMachineCache
{
    public string RandomShortname;
    public string VehicleKey;
    public VehicleInfoConfig Get();
}

public class FullscreenSetup : UserInterface
{
    [JsonProperty(LangRu ? "Цвет фона экрана" : "Screen background color")]
    public IColor BackgroundScreen;
    [JsonProperty("Material")]
    public string Material;
    [JsonProperty("Sprite")]
    public string Sprite;
    [JsonProperty(LangRu ? "Кнопка закрытия" : "Button Close")]
    public ButtonCloseSetup ButtonClose;
}

public class ButtonCloseSetup : InterfacePosition
{
    [JsonProperty(PropertyName = "Background Button Close")]
    public ImageSettings BackgroundClose;
    [JsonProperty(PropertyName = "Color Button Close")]
    public IColor Color;
    [JsonProperty(PropertyName = "Sprite Button Close")]
    public string Sprite;
    [JsonProperty(PropertyName = "Material Button Close")]
    public string Material;
}

public class UserInterface
{
    [JsonProperty(PropertyName = LangRu ? "Установлена?" : "Installed?")]
    public bool Installed;
    [JsonProperty(PropertyName = LangRu ? "Высота главной панели" : "Height of the main panel")]
    public float Height;
    [JsonProperty(PropertyName = LangRu ? "Ширина главной панели" : "Width of the main panel")]
    public float Width;
    [JsonProperty(PropertyName = LangRu ? "Высота лота" : "Lot height")]
    public float LotHeight;
    [JsonProperty(PropertyName = LangRu ? "Ширина лота" : "Lot width")]
    public float LotWidth;
    [JsonProperty(PropertyName = LangRu ? "Кол-во лотов на строке" : "Number of lots per line")]
    public int LotsOnString;
    [JsonProperty(PropertyName = LangRu ? "Отступы по горизонтали" : "Horizontal margins")]
    public float XIndent;
    [JsonProperty(PropertyName = LangRu ? "Отступы по вертикали" : "Vertical margins")]
    public float YIndent;
    [JsonProperty(PropertyName = "Background main panel")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = LangRu ? "Настройки заголовка" : "Header Settings")]
    public PanelHeaderUI HeaderPanel;
    [JsonProperty(PropertyName = LangRu ? "Настройки панели лотов" : "Lot Panel Settings")]
    public PanelContentUI ContentPanel;
    [JsonProperty(PropertyName = LangRu ? "Настройки лота" : "Lot Settings")]
    public PanelLotUI LotPanel;
    public class PanelHeaderUI
    {
        [JsonProperty(PropertyName = "Background")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = "Title")]
        public TextSettings Title;
        [JsonProperty(PropertyName = "Show Line?")]
        public bool ShowLine;
        [JsonProperty(PropertyName = "Line")]
        public ImageSettings Line;
    }

    public class PanelContentUI
    {
        [JsonProperty(PropertyName = "Background")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = "Scroll View")]
        public ScrollViewUI Scroll;
    }

    public class PanelLotUI
    {
        [JsonProperty(PropertyName = "Background")]
        public ImageSettings Background;
        [JsonProperty(PropertyName = "Lot Image")]
        public InterfacePosition LotImage;
        [JsonProperty(PropertyName = "Show Name?")]
        public bool ShowName;
        [JsonProperty(PropertyName = "Lot Name")]
        public TextSettings LotName;
        [JsonProperty(PropertyName = "Lot Currency")]
        public TextSettings CurrencyName;
        [JsonProperty(PropertyName = "Lot Price")]
        public TextSettings PriceText;
        [JsonProperty(PropertyName = "Lot Button Buy")]
        public ButtonSettings ButtonBuy;
    }

    public static FullscreenSetup GenerateFullScreen();
    public static UserInterface GenerateUIMenuV1();
    public static UserInterface GenerateUIMenuV2();
}

public class PanelHeaderUI
{
    [JsonProperty(PropertyName = "Background")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = "Title")]
    public TextSettings Title;
    [JsonProperty(PropertyName = "Show Line?")]
    public bool ShowLine;
    [JsonProperty(PropertyName = "Line")]
    public ImageSettings Line;
}

public class PanelContentUI
{
    [JsonProperty(PropertyName = "Background")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = "Scroll View")]
    public ScrollViewUI Scroll;
}

public class PanelLotUI
{
    [JsonProperty(PropertyName = "Background")]
    public ImageSettings Background;
    [JsonProperty(PropertyName = "Lot Image")]
    public InterfacePosition LotImage;
    [JsonProperty(PropertyName = "Show Name?")]
    public bool ShowName;
    [JsonProperty(PropertyName = "Lot Name")]
    public TextSettings LotName;
    [JsonProperty(PropertyName = "Lot Currency")]
    public TextSettings CurrencyName;
    [JsonProperty(PropertyName = "Lot Price")]
    public TextSettings PriceText;
    [JsonProperty(PropertyName = "Lot Button Buy")]
    public ButtonSettings ButtonBuy;
}

private class LockInfo
{
    public int ItemId;
    public string Prefab;
    public string PreHookName;
    public ItemDefinition ItemDefinition { get; set; }
    public ItemBlueprint Blueprint { get; set; }
}

internal class VehicleInfoConfig
{
    [JsonProperty("Sound on purchase", Order = 0)]
    public bool UseSoundOnPurchase;
    [JsonProperty("Order", Order = -1)]
    public int Order;
    [JsonProperty("Show", Order = 0)]
    public bool Show;
    [JsonProperty("Name", Order = 1)]
    public string Name;
    [JsonProperty("Prefab", Order = 2)]
    public string Prefab;
    [JsonProperty("Image link", Order = 3)]
    public string Image;
    [JsonProperty("Spawn distance", Order = 4)]
    public float SpawnDistance;
    [JsonProperty("Fuel", Order = 5)]
    public int Fuel;
    [JsonProperty("Currency: 0 - item, 1 - Economics, 2 - Server Rewards", Order = 6)]
    public byte SellCurrency;
    [JsonProperty("If vehicle selling for item type him shortname", Order = 7)]
    public string Shortname;
    [JsonProperty("Price", Order = 8)]
    public int Price;
    [JsonProperty("Skin", Order = 9)]
    public ulong Skin;
    [JsonProperty("Command", Order = 10)]
    public string Command;
    [JsonProperty("DeployableItemId", Order = 11)]
    public int DeployableItemId;
    [JsonProperty("Need add engine parts if it possible?", Order = 12)]
    public bool NeedCarParts;
    [JsonProperty("Engine parts", Order = 13)]
    public List<string> EngineParts;
    [JsonProperty("Cooldown to buy (in seconds)", Order = -2)]
    public int Cooldown;
    [JsonProperty("Pickup type (0 - command, 1 - hammer)", Order = -3)]
    public int PickupType;
    [JsonProperty("Can pickup?", Order = -4)]
    public bool CanPickup;
    [JsonProperty("Can recall?", Order = -5)]
    public bool CanCallback;
    [JsonProperty("Recall price", Order = -6)]
    public int RecallCost;
    [JsonProperty("Recall cost need?", Order = -7)]
    public bool RecallCostNeed;
    [JsonProperty("Pickup price", Order = -8)]
    public int PickupPrice;
    [JsonProperty("Enable decay?", Order = -9)]
    public bool EnableDecay;
    [JsonProperty("Permission (still empty if not need) ex. vehiclebuy.YOURPERMISSIONNAME", Order = -10)]
    public string Permission;
    [JsonProperty("Maximum number of purchases of one vehicle by one player", Order = -11)]
    public int MaxPurchases;
    public bool CanPlayerBuy(BasePlayer player);
}

internal class VendingMachinesConfig
{
    [JsonProperty("Bandit Camp vending machine")]
    public bool BanditCampSpawnMachine;
    [JsonProperty("Outpost vending machine")]
    public bool OutpostSpawnMachine;
    [JsonProperty("Bandit Camp products")]
    public List<VMOrder> BanditCampOrders;
    [JsonProperty("Outpost products")]
    public List<VMOrder> OutpostOrders;
    [JsonProperty("Fishing village A vending machine")]
    public bool FishingVillageASpawnMachine;
    [JsonProperty("Fishing village B vending machine")]
    public bool FishingVillageBSpawnMachine;
    [JsonProperty("Fishing village C vending machine")]
    public bool FishingVillageCSpawnMachine;
    [JsonProperty("Fishing Village C products")]
    public List<VMOrder> FishingVillageCOrders;
    [JsonProperty("Fishing Village A products")]
    public List<VMOrder> FishingVillageAOrders;
    [JsonProperty("Fishing Village B products")]
    public List<VMOrder> FishingVillageBOrders;
}

internal class VMOrder
{
    [JsonProperty("Vehicle key from config")]
    public string VehicleKey;
    [JsonProperty("Item (shortname)")]
    public string Shortname;
    [JsonProperty("Price")]
    public int Price;
    public VehicleInfoConfig GetVehicle();
}

public class ConfigData
{
    [JsonProperty("Commands", Order = -1)]
    public List<string> Commands;
    [JsonProperty("Currency name economics", Order = 0)]
    public string CurrencyName;
    [JsonProperty("Currency name Server Rewards", Order = 1)]
    public string CurrencyNameSR;
    [JsonProperty("Currency name Bank System", Order = 1)]
    public string CurrencyNameBS;
    [JsonProperty("Pickup distance", Order = 2)]
    public float PickupRadius;
    [JsonProperty("Disable vehicles damage?", Order = 2)]
    public bool DisableVehiclesDamage;
    [JsonProperty("Vending machines", Order = 2)]
    public VendingMachinesConfig VendingMachines;
    [JsonProperty("Vehicles", Order = 3)]
    public Dictionary<string, VehicleInfoConfig> Vehicles;
    [JsonProperty(Order = 200)]
    public VersionNumber Version;
}

public class ScrollViewUI
{
    [JsonProperty(PropertyName = "Scroll Type (Horizontal, Vertical)")]
    [JsonConverter(typeof(StringEnumConverter))]
    public ScrollType ScrollType;
    [JsonProperty(PropertyName = "Movement Type (Unrestricted, Elastic, Clamped)")]
    [JsonConverter(typeof(StringEnumConverter))]
    public ScrollRect.MovementType MovementType;
    [JsonProperty(PropertyName = "Elasticity")]
    public float Elasticity;
    [JsonProperty(PropertyName = "Deceleration Rate")]
    public float DecelerationRate;
    [JsonProperty(PropertyName = "Scroll Sensitivity")]
    public float ScrollSensitivity;
    [JsonProperty(PropertyName = "Minimal Height")]
    public float MinHeight;
    [JsonProperty(PropertyName = "Additional Height")]
    public float AdditionalHeight;
    [JsonProperty(PropertyName = "Scrollbar Settings")]
    public ScrollBarSettings Scrollbar;
    public CuiScrollViewComponent GetScrollView(float totalWidth);
    public CuiScrollViewComponent GetScrollView(CuiRectTransform contentTransform);
    public CuiRectTransform CalculateContentRectTransform(float totalWidth);
    public class ScrollBarSettings
    {
        [JsonProperty(PropertyName = "Invert")]
        public bool Invert;
        [JsonProperty(PropertyName = "Auto Hide")]
        public bool AutoHide;
        [JsonProperty(PropertyName = "Handle Sprite")]
        public string HandleSprite;
        [JsonProperty(PropertyName = "Size")]
        public float Size;
        [JsonProperty(PropertyName = "Handle Color")]
        public IColor HandleColor;
        [JsonProperty(PropertyName = "Highlight Color")]
        public IColor HighlightColor;
        [JsonProperty(PropertyName = "Pressed Color")]
        public IColor PressedColor;
        [JsonProperty(PropertyName = "Track Sprite")]
        public string TrackSprite;
        [JsonProperty(PropertyName = "Track Color")]
        public IColor TrackColor;
        public CuiScrollbar Get();
    }

}

public class ScrollBarSettings
{
    [JsonProperty(PropertyName = "Invert")]
    public bool Invert;
    [JsonProperty(PropertyName = "Auto Hide")]
    public bool AutoHide;
    [JsonProperty(PropertyName = "Handle Sprite")]
    public string HandleSprite;
    [JsonProperty(PropertyName = "Size")]
    public float Size;
    [JsonProperty(PropertyName = "Handle Color")]
    public IColor HandleColor;
    [JsonProperty(PropertyName = "Highlight Color")]
    public IColor HighlightColor;
    [JsonProperty(PropertyName = "Pressed Color")]
    public IColor PressedColor;
    [JsonProperty(PropertyName = "Track Sprite")]
    public string TrackSprite;
    [JsonProperty(PropertyName = "Track Color")]
    public IColor TrackColor;
    public CuiScrollbar Get();
}

public class ImageSettings : InterfacePosition
{
    [JsonProperty(PropertyName = "Sprite")]
    public string Sprite;
    [JsonProperty(PropertyName = "Material")]
    public string Material;
    [JsonProperty(PropertyName = "Image")]
    public string Image;
    [JsonProperty(PropertyName = "Color")]
    public IColor Color;
    [JsonProperty(PropertyName = "Cursor Enabled")]
    public bool CursorEnabled;
    [JsonProperty(PropertyName = "Keyboard Enabled")]
    public bool KeyboardEnabled;
    [JsonIgnore]
    private ICuiComponent _imageComponent;
    public ICuiComponent GetImageComponent();
    public bool TryGetImageURL(string url);
    public CuiElement GetImage(string parent, string name, string destroyUI);
    public ImageSettings();
    public ImageSettings(string imageURL, IColor color, InterfacePosition position);
}

public class ButtonSettings : TextSettings
{
    [JsonProperty(PropertyName = "Button Color")]
    public IColor ButtonColor;
    [JsonProperty(PropertyName = "Sprite")]
    public string Sprite;
    [JsonProperty(PropertyName = "Material")]
    public string Material;
    [JsonProperty(PropertyName = "Image")]
    public string Image;
    [JsonProperty(PropertyName = "Image Color")]
    public IColor ImageColor;
    [JsonProperty(PropertyName = "Use custom image position settings?")]
    public bool UseCustomPositionImage;
    [JsonProperty(PropertyName = "Custom image position settings")]
    public InterfacePosition ImagePosition;
    public bool TryGetImageURL(string url);
    public List<CuiElement> GetButton(string msg, string cmd, string parent, string name, string destroyUI, string close);
}

public class TextSettings : InterfacePosition
{
    [JsonProperty(PropertyName = "Font Size")]
    public int FontSize;
    [JsonProperty(PropertyName = "Is Bold?")]
    public bool IsBold;
    [JsonProperty(PropertyName = "Align")]
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor Align;
    [JsonProperty(PropertyName = "Color")]
    public IColor Color;
    public CuiTextComponent GetTextComponent(string msg);
    public CuiElement GetText(string msg, string parent, string name, string destroyUI);
}

public class InterfacePosition
{
    [JsonProperty(PropertyName = "AnchorMin")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "AnchorMax")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "OffsetMin")]
    public string OffsetMin;
    [JsonProperty(PropertyName = "OffsetMax")]
    public string OffsetMax;
    [JsonIgnore]
    private CuiRectTransformComponent _position;
    public CuiRectTransformComponent GetRectTransform();
    public InterfacePosition();
    public InterfacePosition(InterfacePosition other);
    public static InterfacePosition CreatePosition(float aMinX, float aMinY, float aMaxX, float aMaxY, float oMinX, float oMinY, float oMaxX, float oMaxY);
    public static InterfacePosition CreatePosition(string anchorMin, string anchorMax, string offsetMin, string offsetMax);
    public static InterfacePosition CreatePosition(CuiRectTransform rectTransform);
    public static InterfacePosition CreateFullStretch();
    public static InterfacePosition CreateCenter();
}

public class IColor
{
    [JsonProperty(PropertyName = "HEX")]
    public string HEX;
    [JsonProperty(PropertyName = LangRu ? "Непрозрачность (0 - 100)" : "Opacity (0 - 100)")]
    public float Alpha;
    [JsonIgnore]
    private string _cachedResult;
    [JsonIgnore]
    private bool _isCached;
    public string Get();
    public IColor();
    public IColor(string hex, float alpha);
    public static IColor Create(string hex, float alpha);
    public static IColor CreateTransparent();
    public static IColor CreateWhite();
    public static IColor CreateBlack();
}

Oxide.Plugins.VehicleBuyExtensionMethods
public static class ExtensionMethods
{
    public static bool IsURL(string uriName);
}


```

---

## VehicleDecayProtection

```csharp
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Vehicle Decay Protection", "WhiteThunder", "1.0.2")]
[Description("Protects vehicles from decay around tool cupboards and when recently used.")]
internal class VehicleDecayProtection : CovalencePlugin
{
    private VehicleDecayConfig PluginConfig;
    private void Init();
    private object OnEntityTakeDamage(BaseVehicle entity, HitInfo hitInfo);
    private object OnEntityTakeDamage(HotAirBalloon entity, HitInfo hitInfo);
    private object OnEntityTakeDamage(BaseVehicleModule entity, HitInfo hitInfo);
    private object ProcessDecayDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private VehicleConfig GetVehicleConfig(BaseCombatEntity entity);
    private float GetVehicleLastUsedTime(BaseCombatEntity entity);
    protected override void LoadDefaultConfig();
    internal class VehicleDecayConfig
    {
        [JsonProperty("Vehicles")]
        public VehicleConfigMap Vehicles;
    }

    internal class VehicleConfigMap
    {
        [JsonProperty("HotAirBalloon")]
        public VehicleConfig HotAirBalloon;
        [JsonProperty("Minicopter")]
        public VehicleConfig Minicopter;
        [JsonProperty("ModularCar")]
        public VehicleConfig ModularCar;
        [JsonProperty("RHIB")]
        public VehicleConfig RHIB;
        [JsonProperty("Rowboat")]
        public VehicleConfig Rowboat;
        [JsonProperty("ScrapTransportHelicopter")]
        public VehicleConfig ScrapTransportHelicopter;
    }

    internal class VehicleConfig
    {
        [JsonProperty("DecayMultiplierNearTC")]
        public float DecayMultiplierNearTC;
        [JsonProperty("ProtectionMinutesAfterUse")]
        public float ProtectionMinutesAfterUse;
    }

}

internal class VehicleDecayConfig
{
    [JsonProperty("Vehicles")]
    public VehicleConfigMap Vehicles;
}

internal class VehicleConfigMap
{
    [JsonProperty("HotAirBalloon")]
    public VehicleConfig HotAirBalloon;
    [JsonProperty("Minicopter")]
    public VehicleConfig Minicopter;
    [JsonProperty("ModularCar")]
    public VehicleConfig ModularCar;
    [JsonProperty("RHIB")]
    public VehicleConfig RHIB;
    [JsonProperty("Rowboat")]
    public VehicleConfig Rowboat;
    [JsonProperty("ScrapTransportHelicopter")]
    public VehicleConfig ScrapTransportHelicopter;
}

internal class VehicleConfig
{
    [JsonProperty("DecayMultiplierNearTC")]
    public float DecayMultiplierNearTC;
    [JsonProperty("ProtectionMinutesAfterUse")]
    public float ProtectionMinutesAfterUse;
}


```

---

## VehicleManager

```csharp
using Oxide.Core;
using UnityEngine;
using System.Collections.Generic;
using System.Linq;
using Network;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Vehicle Manager", "WOLF-TOR", "1.0.0")]
 class VehicleManager : RustPlugin
{
    private static Configuration _config;
    public static VehicleManager _instance;
    public class Configuration
    {
        [JsonProperty("Разрешить подъем транспорта киянкой")]
        public bool allowPickupVehicle;
        [JsonProperty("shortname объекта который держит игрок при установке транспорта")]
        public string vehicleShortPrefabName;
        [JsonProperty("Список транспорта")]
        public List<VehicleInfo> vehicles;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public class VehicleInfo
    {
        [JsonProperty("Короткое имя транспорта")]
        public string shortname;
        [JsonProperty("prefab транспорта")]
        public string prefab;
        [JsonProperty("skinID транспорта")]
        public ulong skinID;
        [JsonProperty("Effect который проигрывается при установке транспорта")]
        public string placementEffect;
        public VehicleInfo(string shortname, string prefab, ulong skinID, string placementEffect);
        public string Give(BasePlayer player, string shortname, string text);
        public BaseEntity Spawn(Vector3 position, Quaternion rotation, BasePlayer player, Item ownerItem);
        public static VehicleInfo FindByShortname(string shortname);
        public static VehicleInfo FindByPrefab(string prefab);
        public static VehicleInfo FindBySkinID(ulong skinID);
        public static List<string> ShortNameList();
    }

    private void LoadDefaultMessages();
    private string _(BasePlayer player, string key, object[] args);
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private object OnNpcGiveSoldItem(NPCVendingMachine vending, Item soldItem, BasePlayer buyer);
    private object CanBuild(Planner plan, Construction construction, Construction.Target target);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    private object OnItemCustomName(int itemID, ulong skinID, string language);
    private void OnPlayerSetInfo(Connection connection, string key, string val);
    [ConsoleCommand("vehicle.give")]
     void ConsoleCommand_vehicleshop_give(ConsoleSystem.Arg arg);
    private void ChangeVehicleName(Item item, BasePlayer player);
}

public class Configuration
{
    [JsonProperty("Разрешить подъем транспорта киянкой")]
    public bool allowPickupVehicle;
    [JsonProperty("shortname объекта который держит игрок при установке транспорта")]
    public string vehicleShortPrefabName;
    [JsonProperty("Список транспорта")]
    public List<VehicleInfo> vehicles;
}

public class VehicleInfo
{
    [JsonProperty("Короткое имя транспорта")]
    public string shortname;
    [JsonProperty("prefab транспорта")]
    public string prefab;
    [JsonProperty("skinID транспорта")]
    public ulong skinID;
    [JsonProperty("Effect который проигрывается при установке транспорта")]
    public string placementEffect;
    public VehicleInfo(string shortname, string prefab, ulong skinID, string placementEffect);
    public string Give(BasePlayer player, string shortname, string text);
    public BaseEntity Spawn(Vector3 position, Quaternion rotation, BasePlayer player, Item ownerItem);
    public static VehicleInfo FindByShortname(string shortname);
    public static VehicleInfo FindByPrefab(string prefab);
    public static VehicleInfo FindBySkinID(ulong skinID);
    public static List<string> ShortNameList();
}


```

---

## VehicleNoDamage

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

Oxide.Plugins
[Info("VehicleNoDamage", "Frizen", "1.0.0")]
public class VehicleNoDamage : RustPlugin
{
    private object OnEntityTakeDamage(BaseVehicle entity, HitInfo hitInfo);
     bool CanMount;
    [ChatCommand("stop")]
     void CmdMountNo(BasePlayer player);
    [ChatCommand("start")]
     void CmdMountYes(BasePlayer player);
     object CanMountEntity(BasePlayer player, BaseMountable entity);
     object CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot, int amount);
    private object OnEntityTakeDamage(BaseVehicleModule entity, HitInfo hitInfo);
}


```

---

## Vending

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System.Collections;
using System.Drawing;
using System.IO;
using System.Drawing.Imaging;
using Newtonsoft.Json.Converters;
using Facepunch;
using VLB;
using System.Linq;
using System.Text;
using System.Threading.Tasks;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Vending", "Frizen", "1.0.0")]
 class Vending : RustPlugin
{
    static Vending ins;
     PluginConfig config;
    public class PluginConfig
    {
        [JsonProperty("Настройка торговых автоматов")]
        public Dictionary<string, Dictionary<string, VendinSetting>> Vending { get; set; }
    }

    public class VendinSetting
    {
        [JsonProperty("Время через которое будет выполняться завоз предметов (в секундах)")]
        public float refillTime;
        [JsonProperty("Настройка товаров")]
        public List<VendingOrder> orders;
    }

    public class VendingOrder
    {
        [JsonProperty("Добавить товар?")]
        public bool AddItem;
        [JsonProperty("Название покупаемого товара")]
        public string ItemToBuy { get; set; }
        [JsonProperty("Максимальный стак покупаемого предмета за один раз")]
        public int BuyngItemMaxAmount { get; set; }
        [JsonProperty("Количество покупаемого товара за раз")]
        public ulong BuyngItemSkinID { get; set; }
        [JsonProperty("SKINID покупаемого предмета")]
        public int BuyngItemAmount { get; set; }
        [JsonProperty("Покупаемый товар это чертёж")]
        public bool BuyngItemIsBP { get; set; }
        [JsonProperty("Название платёжного товара")]
        public string PayItemShortName { get; set; }
        [JsonProperty("Количество платёжного товара за раз")]
        public int PayItemAmount { get; set; }
        [JsonProperty("Платёжный товар это чертёж")]
        public bool PayItemIsBP { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
     void Unload();
    public Dictionary<string, Dictionary<string, VendinSetting>> GetVendingList();
     void VendingInit();
    public void AddItemForSale(VendingMachine vending, int itemID, int amountToSell, int currencyID, int currencyPerTransaction, byte bpState, int maxByStack, ulong SkinID);
}

public class PluginConfig
{
    [JsonProperty("Настройка торговых автоматов")]
    public Dictionary<string, Dictionary<string, VendinSetting>> Vending { get; set; }
}

public class VendinSetting
{
    [JsonProperty("Время через которое будет выполняться завоз предметов (в секундах)")]
    public float refillTime;
    [JsonProperty("Настройка товаров")]
    public List<VendingOrder> orders;
}

public class VendingOrder
{
    [JsonProperty("Добавить товар?")]
    public bool AddItem;
    [JsonProperty("Название покупаемого товара")]
    public string ItemToBuy { get; set; }
    [JsonProperty("Максимальный стак покупаемого предмета за один раз")]
    public int BuyngItemMaxAmount { get; set; }
    [JsonProperty("Количество покупаемого товара за раз")]
    public ulong BuyngItemSkinID { get; set; }
    [JsonProperty("SKINID покупаемого предмета")]
    public int BuyngItemAmount { get; set; }
    [JsonProperty("Покупаемый товар это чертёж")]
    public bool BuyngItemIsBP { get; set; }
    [JsonProperty("Название платёжного товара")]
    public string PayItemShortName { get; set; }
    [JsonProperty("Количество платёжного товара за раз")]
    public int PayItemAmount { get; set; }
    [JsonProperty("Платёжный товар это чертёж")]
    public bool PayItemIsBP { get; set; }
}


```

---

## VendingInStock

```csharp
using Oxide.Core;

Oxide.Plugins
[Info("Vending In Stock", "AVOcoder", "1.0.5")]
[Description("VendingMachines sell-orders always in stock")]
 class VendingInStock : RustPlugin
{
    private void OnNpcGiveSoldItem(NPCVendingMachine vm, Item soldItem, BasePlayer buyer);
}


```

---

## VikingBoat

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
using Facepunch;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("VikingBoat", "Colon Blow", "1.0.7")]
 class VikingBoat : RustPlugin
{
     BaseEntity newVikingBoat;
    static Dictionary<ulong, string> hasVikingBoat;
    static List<uint> storedVikingBoats;
    private DynamicConfigFile data;
    private bool initialized;
     void Loaded();
    private void OnServerInitialized();
    private void OnServerSave();
    private void RestoreVikingBoats();
     void SaveData();
     void LoadData();
     bool isAllowed(BasePlayer player, string perm);
    public void AddPlayerVikingBoat(ulong id);
    public void RemovePlayerVikingBoat(ulong id);
    public void BuildVikingBoat(BasePlayer player);
    public void RespawnVikingBoat(Vector3 spawnpos, Quaternion spawnrot, ulong userid, string codestr, string guestcodestr);
     bool CheckUpgradeMats(BasePlayer player, int itemID, int amount, string str);
    public bool IsStandingInWater(BasePlayer player);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private object OnEntityGroundMissing(BaseEntity entity);
    private void OnEntityKill(BaseNetworkable networkable);
    private void OnEntityMounted(BaseMountable mountable, BasePlayer player);
    private object OnEntityTakeDamage(BaseCombatEntity entity1, HitInfo hitInfo);
    private object CanPickupEntity(BaseCombatEntity entity, BasePlayer player);
     object CanPickupLock(BasePlayer player, BaseLock baseLock);
     void Unload();
     void DestroyAll();
    [ChatCommand("vikingboat.build")]
     void cmdVikingBoatBuild(BasePlayer player, string command, string[] args);
    [ConsoleCommand("vikingboat.build")]
     void cmdConsoleVikingBoatBuild(ConsoleSystem.Arg arg);
    [ChatCommand("vikingboat.loc")]
     void cmdVikingBoatLoc(BasePlayer player, string command, string[] args);
    [ConsoleCommand("vikingboat.loc")]
     void cmdConsoleVikingBoatLoc(ConsoleSystem.Arg arg);
    [ChatCommand("vikingboat.destroy")]
     void cmdVikingBoatDestroy(BasePlayer player, string command, string[] args);
    public class VikingBoatEntity : BaseEntity
    {
         VikingBoat VikingBoat;
        public BaseEntity entity;
         BaseEntity nosepoint;
         BaseEntity trianglefloor1;
         BaseEntity trianglefloor2;
         BaseEntity floor1;
         BaseEntity floor2;
         BaseEntity floor3;
         BaseEntity floor4;
         BaseEntity floor5;
         BaseEntity wall1;
         BaseEntity wall2;
         BaseEntity wall3;
         BaseEntity wall1r;
         BaseEntity wall2r;
         BaseEntity wall3r;
         BaseEntity wall4;
         BaseEntity wall4r;
         BaseEntity wall5;
         BaseEntity wall5r;
         BaseEntity wallback;
         BaseEntity wallfrontl;
         BaseEntity wallfrontr;
         BaseEntity chairleft1;
         BaseEntity chairleft2;
         BaseEntity chairleft3;
         BaseEntity chairleft4;
         BaseEntity chairleft5;
         BaseEntity chairright2;
         BaseEntity chairright3;
         BaseEntity chairright4;
         BaseEntity chairright5;
         BaseEntity sailfront;
         BaseEntity sailback;
         BaseEntity oarright1;
         BaseEntity oarright2;
         BaseEntity oarright3;
         BaseEntity oarright4;
         BaseEntity oarright5;
         BaseEntity oarleft1;
         BaseEntity oarleft2;
         BaseEntity oarleft3;
         BaseEntity oarleft4;
         BaseEntity oarleft5;
         BaseEntity oarcenter1;
         BaseEntity oarcenter2;
         BaseEntity oarcenter3;
         BaseEntity oarcenter4;
         BaseEntity oarcenter5;
         BaseEntity rudder;
        public BaseEntity boatlock;
         Vector3 entitypos;
         Quaternion entityrot;
        public BasePlayer player;
         ulong ownerid;
         int counter;
        public bool ismoving;
        public bool moveforward;
        public bool movebackward;
        public bool rotright;
        public bool rotleft;
         float waterheight;
         Vector3 movedirection;
         Vector3 rotdirection;
         Vector3 startloc;
         Vector3 startrot;
         Vector3 endloc;
         float steps;
         float incrementor;
         void Awake();
         void SpawnVikingBoat();
         void SpawnLock();
        public void SpawnSideWalls();
         void SpawnOars();
         void SpawnSails();
        public void SpawnPassengerChairs();
         void SpawnRefresh(BaseNetworkable entity1);
         bool hitSomething(Vector3 position);
         bool isStillInWater(Vector3 position);
         bool PlayerIsMounted();
         void SplashEffect();
         void CalculateSpeed();
         void FixedUpdate();
         void MoveOars();
         void ResetMovement();
        public void RefreshAll();
        public void OnDestroy();
    }

    static float DefaultVikingBoatMovementSpeed;
     bool OnlyOneActiveVikingBoat;
    static int MaterialsForVikingBoat;
    static int MaterialID;
     bool BlockDecay;
    static bool ShowWaterSplash;
     bool Changed;
     bool isRestricted;
    private void LoadVariables();
     void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

public class VikingBoatEntity : BaseEntity
{
     VikingBoat VikingBoat;
    public BaseEntity entity;
     BaseEntity nosepoint;
     BaseEntity trianglefloor1;
     BaseEntity trianglefloor2;
     BaseEntity floor1;
     BaseEntity floor2;
     BaseEntity floor3;
     BaseEntity floor4;
     BaseEntity floor5;
     BaseEntity wall1;
     BaseEntity wall2;
     BaseEntity wall3;
     BaseEntity wall1r;
     BaseEntity wall2r;
     BaseEntity wall3r;
     BaseEntity wall4;
     BaseEntity wall4r;
     BaseEntity wall5;
     BaseEntity wall5r;
     BaseEntity wallback;
     BaseEntity wallfrontl;
     BaseEntity wallfrontr;
     BaseEntity chairleft1;
     BaseEntity chairleft2;
     BaseEntity chairleft3;
     BaseEntity chairleft4;
     BaseEntity chairleft5;
     BaseEntity chairright2;
     BaseEntity chairright3;
     BaseEntity chairright4;
     BaseEntity chairright5;
     BaseEntity sailfront;
     BaseEntity sailback;
     BaseEntity oarright1;
     BaseEntity oarright2;
     BaseEntity oarright3;
     BaseEntity oarright4;
     BaseEntity oarright5;
     BaseEntity oarleft1;
     BaseEntity oarleft2;
     BaseEntity oarleft3;
     BaseEntity oarleft4;
     BaseEntity oarleft5;
     BaseEntity oarcenter1;
     BaseEntity oarcenter2;
     BaseEntity oarcenter3;
     BaseEntity oarcenter4;
     BaseEntity oarcenter5;
     BaseEntity rudder;
    public BaseEntity boatlock;
     Vector3 entitypos;
     Quaternion entityrot;
    public BasePlayer player;
     ulong ownerid;
     int counter;
    public bool ismoving;
    public bool moveforward;
    public bool movebackward;
    public bool rotright;
    public bool rotleft;
     float waterheight;
     Vector3 movedirection;
     Vector3 rotdirection;
     Vector3 startloc;
     Vector3 startrot;
     Vector3 endloc;
     float steps;
     float incrementor;
     void Awake();
     void SpawnVikingBoat();
     void SpawnLock();
    public void SpawnSideWalls();
     void SpawnOars();
     void SpawnSails();
    public void SpawnPassengerChairs();
     void SpawnRefresh(BaseNetworkable entity1);
     bool hitSomething(Vector3 position);
     bool isStillInWater(Vector3 position);
     bool PlayerIsMounted();
     void SplashEffect();
     void CalculateSpeed();
     void FixedUpdate();
     void MoveOars();
     void ResetMovement();
    public void RefreshAll();
    public void OnDestroy();
}


```

---

## VipRemainInfo

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Newtonsoft.Json;

Oxide.Plugins
[Info("VipRemainInfo", "Nimant", "1.0.5", ResourceId = 0)]
 class VipRemainInfo : RustPlugin
{
    [PluginReference]
    private Plugin Grant;
    private Dictionary<ulong, bool> ShowedVipInfo;
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    [ChatCommand("vipinfo")]
    private void cmdVipInfo(BasePlayer player, string command, string[] args);
    private void ShowRemainVipInfo(BasePlayer player, bool isSilent);
    private string GetTime(int rawSeconds);
    private void LoadDefaultMessages();
    private string GetLangMessage(string key, string steamID);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Цвета вип групп и отдельных привилегий")]
        public Dictionary<string, string> GroupsColor;
        [JsonProperty(PropertyName = "Названия вип групп и отдельных привилегий")]
        public Dictionary<string, string> GroupsNewName;
        [JsonProperty(PropertyName = "Задержка на показ остатка времени у групп и отдельных привилегий при заходе на сервер (-1 = отключить показ)")]
        public float ShowDelay;
    }

    private void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Цвета вип групп и отдельных привилегий")]
    public Dictionary<string, string> GroupsColor;
    [JsonProperty(PropertyName = "Названия вип групп и отдельных привилегий")]
    public Dictionary<string, string> GroupsNewName;
    [JsonProperty(PropertyName = "Задержка на показ остатка времени у групп и отдельных привилегий при заходе на сервер (-1 = отключить показ)")]
    public float ShowDelay;
}


```

---

## VisualCupboard

```csharp
using System;
using Oxide.Core;
using UnityEngine;
using System.Collections.Generic;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("VisualCupboard", "Colon Blow", "1.0.7", ResourceId = 2030)]
 class VisualCupboard : RustPlugin
{
     void OnServerInitialized();
     void Loaded();
     void LoadDefaultConfig();
     Dictionary<string, string> messages;
     bool Changed;
    private static float UseCupboardRadius;
     bool ShowOnlyOwnCupboards;
     bool ShowRadiusWhenDeploying;
     float DurationToShowRadius;
     float ShowCupboardsWithinRangeOf;
     bool AdminShowOwnerID;
    private static bool serverInitialized;
    private void LoadConfigVariables();
    private void LoadVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     class ToolCupboardSphere : MonoBehaviour
    {
         BaseEntity sphere;
         BaseEntity entity;
         Vector3 pos;
         Quaternion rot;
         string strPrefab;
         void Awake();
         void OnDestroy();
    }

     void OnEntitySpawned(BaseEntity entity, UnityEngine.GameObject gameObject);
    [ChatCommand("showsphere")]
     void cmdChatShowSphere(BasePlayer player, string command);
    [ChatCommand("killsphere")]
     void cmdChatDestroySphere(BasePlayer player, string command);
    private string FindPlayerName(ulong userId);
     void Unload();
    static void DestroyAll();
     bool isAllowed(BasePlayer player, string perm);
}

 class ToolCupboardSphere : MonoBehaviour
{
     BaseEntity sphere;
     BaseEntity entity;
     Vector3 pos;
     Quaternion rot;
     string strPrefab;
     void Awake();
     void OnDestroy();
}


```

---

## VitalKillFeed-1.0.3

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("VitalKillFeed", "Xonafied", "1.0.3")]
[Description("Vital kill feed msgs")]
public class VitalKillFeed : CovalencePlugin
{
    protected override void LoadDefaultMessages();
    private static readonly string permhide;
     void OnServerInitialized();
     void OnEntityDeath(BasePlayer player, HitInfo info);
     void OnPlayerRespawn(BasePlayer player, BasePlayer.SpawnPoint spawnPoint);
     void OnPlayerRespawn(BasePlayer player, SleepingBag sleepingBag);
    private Dictionary<ulong, CustomInfo> _deathData;
    private void SendHitInfoData(BasePlayer player);
    private void SendHitInfoData(BasePlayer player, CustomInfo custominfo, BasePlayer killed);
    private void ToggleDeathMsgs(IPlayer iplayer, string command, string[] args);
    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
}


```

---

## VKBot

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("VKBot", "SkiTles", "1.6")]
 class VKBot : RustPlugin
{
    private System.Random random;
    private string opc;
    private string stw;
    private string sts;
    private string str;
    private string stb;
    private string ste;
    private string wd;
    private string su;
    private string msg;
    private string cone;
    private string slprs;
    private string mapfile;
    private bool NewWipe;
     JsonSerializerSettings jsonsettings;
    private bool OxideUpdateNotice;
    private Dictionary<ulong, ulong> PlayersCheckList;
    private int serverOxideVersion;
    private List<string> allowedentity;
    private List<ulong> BDayPlayers;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Ключи VK API, ID группы")]
        public VKAPITokens VKAPIT { get; set; }
        [JsonProperty(PropertyName = "Настройки оповещений администраторов")]
        public AdminNotify AdmNotify { get; set; }
        [JsonProperty(PropertyName = "Настройки статуса")]
        public StatusSettings StatusStg { get; set; }
        [JsonProperty(PropertyName = "Оповещения при вайпе")]
        public WipeSettings WipeStg { get; set; }
        [JsonProperty(PropertyName = "Награда за вступление в группу")]
        public GroupGifts GrGifts { get; set; }
        [JsonProperty(PropertyName = "Награда для именинников")]
        public BDayGiftSet BDayGift { get; set; }
        [JsonProperty(PropertyName = "Настройки текста в уведомлениях в чат")]
        public TextSettings TxtSet { get; set; }
        [JsonProperty(PropertyName = "Поддержка нескольких серверов")]
        public MultipleServersSettings MltServSet { get; set; }
        [JsonProperty(PropertyName = "Проверка игроков на читы")]
        public PlayersCheckingSettings PlChkSet { get; set; }
        [JsonProperty(PropertyName = "Топ игроки вайпа и промо")]
        public TopWPlPromoSet TopWPlayersPromo { get; set; }
        public class VKAPITokens
        {
            [JsonProperty(PropertyName = "VK Token группы (для сообщений)")]
            [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
            public string VKToken { get; set; }
            [JsonProperty(PropertyName = "VK Token приложения (для записей на стене и статуса)")]
            [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
            public string VKTokenApp { get; set; }
            [JsonProperty(PropertyName = "VKID группы")]
            [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
            public string GroupID { get; set; }
        }

        public class AdminNotify
        {
            [JsonProperty(PropertyName = "VkID администраторов (пример /11111, 22222/)")]
            [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
            public string VkID { get; set; }
            [JsonProperty(PropertyName = "Включить отправку сообщений администратору командой /report ?")]
            [DefaultValue(true)]
            public bool SendReports { get; set; }
            [JsonProperty(PropertyName = "Предупреждение о злоупотреблении функцией репортов")]
            [DefaultValue("Наличие в тексте нецензурных выражений, оскорблений администрации или игроков сервера, а так же большое количество безсмысленных сообщений приведет к бану!")]
            public string ReportsNotify { get; set; }
            [JsonProperty(PropertyName = "Отправлять сообщение администратору о бане игрока?")]
            [DefaultValue(true)]
            public bool UserBannedMsg { get; set; }
            [JsonProperty(PropertyName = "Отправлять сообщение администратору о выходе нового обновления Oxide?")]
            [DefaultValue(true)]
            public bool OxideNewVersionMsg { get; set; }
        }

        public class StatusSettings
        {
            [JsonProperty(PropertyName = "Обновлять статус в группе? Если стоит /false/ статистика собираться не будет")]
            [DefaultValue(true)]
            public bool UpdateStatus { get; set; }
            [JsonProperty(PropertyName = "Вид статуса (1 - текущий сервер, 2 - список серверов, необходим Rust:IO на каждом сервере)")]
            [DefaultValue(1)]
            public int StatusSet { get; set; }
            [JsonProperty(PropertyName = "Онлайн в статусе вида '125/200'")]
            [DefaultValue(false)]
            public bool OnlWmaxslots { get; set; }
            [JsonProperty(PropertyName = "Таймер обновления статуса (минуты)")]
            [DefaultValue(30)]
            public int UpdateTimer { get; set; }
            [JsonProperty(PropertyName = "Формат статуса")]
            [DefaultValue("{usertext}. Сервер вайпнут: {wipedate}. Онлайн игроков: {onlinecounter}. Спящих: {sleepers}. Добыто дерева: {woodcounter}. Добыто серы: {sulfurecounter}. Выпущено ракет: {rocketscounter}. Использовано взрывчатки: {explosivecounter}. Создано чертежей: {blueprintsconter}. {connect}")]
            public string StatusText { get; set; }
            [JsonProperty(PropertyName = "Список счетчиков, которые будут отображаться в виде emoji")]
            [DefaultValue("onlinecounter, rocketscounter, blueprintsconter, explosivecounter, wipedate")]
            public string EmojiCounterList { get; set; }
            [JsonProperty(PropertyName = "Ссылка на коннект сервера вида /connect 111.111.111.11:11111/")]
            [DefaultValue("connect 111.111.111.11:11111")]
            public string connecturl { get; set; }
            [JsonProperty(PropertyName = "Текст для статуса")]
            [DefaultValue("Сервер 1")]
            public string StatusUT { get; set; }
        }

        public class WipeSettings
        {
            [JsonProperty(PropertyName = "Отправлять пост в группу после вайпа?")]
            [DefaultValue(false)]
            public bool WPostB { get; set; }
            [JsonProperty(PropertyName = "Текст поста о вайпе")]
            [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
            public string WPostMsg { get; set; }
            [JsonProperty(PropertyName = "Добавить изображение к посту о вайпе?")]
            [DefaultValue(false)]
            public bool WPostAttB { get; set; }
            [JsonProperty(PropertyName = "Ссылка на изображение к посту о вайпе вида 'photo-1_265827614' (изображение должно быть в альбоме группы)")]
            [DefaultValue("photo-1_265827614")]
            public string WPostAtt { get; set; }
            [JsonProperty(PropertyName = "Отправлять сообщение администратору о вайпе?")]
            [DefaultValue(true)]
            public bool WPostMsgAdmin { get; set; }
            [JsonProperty(PropertyName = "Отправлять игрокам сообщение о вайпе автоматически?")]
            [DefaultValue(false)]
            public bool WMsgPlayers { get; set; }
            [JsonProperty(PropertyName = "Текст сообщения игрокам о вайпе (сообщение отправляется только тем кто подписался командой /vk wipealerts)")]
            [DefaultValue("Сервер вайпнут! Залетай скорее!")]
            public string WMsgText { get; set; }
            [JsonProperty(PropertyName = "Игнорировать команду /vk wipealerts? (если включено, сообщение о вайпе будет отправляться всем)")]
            [DefaultValue(false)]
            public bool WCMDIgnore { get; set; }
        }

        public class GroupGifts
        {
            [JsonProperty(PropertyName = "Выдавать подарок игроку за вступление в группу ВК?")]
            [DefaultValue(true)]
            public bool VKGroupGifts { get; set; }
            [JsonProperty(PropertyName = "Подарки за вступление в группу (shortname предмета, количество)")]
            [DefaultValue(null)]
            public Dictionary<string, object> VKGroupGiftList { get; set; }
            [JsonProperty(PropertyName = "Ссылка на группу ВК")]
            [DefaultValue("vk.com/1234")]
            public string VKGroupUrl { get; set; }
            [JsonProperty(PropertyName = "Оповещения в общий чат о получении награды")]
            [DefaultValue(true)]
            public bool GiftsBool { get; set; }
            [JsonProperty(PropertyName = "Включить оповещения для игроков не получивших награду за вступление в группу?")]
            [DefaultValue(true)]
            public bool VKGGNotify { get; set; }
            [JsonProperty(PropertyName = "Интервал оповещений для игроков не получивших награду за вступление в группу (в минутах)")]
            [DefaultValue(30)]
            public int VKGGTimer { get; set; }
            [JsonProperty(PropertyName = "Выдавать награду каждый вайп?")]
            [DefaultValue(true)]
            public bool GiftsWipe { get; set; }
        }

        public class BDayGiftSet
        {
            [JsonProperty(PropertyName = "Включить награду для именинников?")]
            [DefaultValue(true)]
            public bool BDayEnabled { get; set; }
            [JsonProperty(PropertyName = "Группа для именинников")]
            [DefaultValue("bdaygroup")]
            public string BDayGroup { get; set; }
            [JsonProperty(PropertyName = "Текст поздравления")]
            [DefaultValue("<size=17><color=#990404>Администрация сервера поздравляет вас с днем рождения! В качестве подарка мы добавили вас в группу с рейтами x4 и китом bday!</color></size>")]
            public string BDayText { get; set; }
            [JsonProperty(PropertyName = "Оповещения в общий чат о имениннках")]
            [DefaultValue(false)]
            public bool BDayNotify { get; set; }
        }

        public class TextSettings
        {
            [JsonProperty(PropertyName = "Размер текста")]
            [DefaultValue("17")]
            public string TxtSize { get; set; }
            [JsonProperty(PropertyName = "Цвет выделенного текста")]
            [DefaultValue("#990404")]
            public string TxtColor { get; set; }
        }

        public class MultipleServersSettings
        {
            [JsonProperty(PropertyName = "Включить поддержку несколько серверов?")]
            [DefaultValue(false)]
            public bool MSSEnable { get; set; }
            [JsonProperty(PropertyName = "Номер сервера")]
            [DefaultValue(1)]
            public int ServerNumber { get; set; }
            [JsonProperty(PropertyName = "Сервер 1 IP:PORT (пример: 111.111.111.111:28015)")]
            [DefaultValue("none")]
            public string Server1ip { get; set; }
            [JsonProperty(PropertyName = "Сервер 2 IP:PORT (пример: 111.111.111.111:28015)")]
            [DefaultValue("none")]
            public string Server2ip { get; set; }
            [JsonProperty(PropertyName = "Сервер 3 IP:PORT (пример: 111.111.111.111:28015)")]
            [DefaultValue("none")]
            public string Server3ip { get; set; }
            [JsonProperty(PropertyName = "Сервер 4 IP:PORT (пример: 111.111.111.111:28015)")]
            [DefaultValue("none")]
            public string Server4ip { get; set; }
            [JsonProperty(PropertyName = "Сервер 5 IP:PORT (пример: 111.111.111.111:28015)")]
            [DefaultValue("none")]
            public string Server5ip { get; set; }
            [JsonProperty(PropertyName = "Онлайн в emoji?")]
            [DefaultValue(true)]
            public bool EmojiStatus { get; set; }
        }

        public class PlayersCheckingSettings
        {
            [JsonProperty(PropertyName = "Текст уведомления")]
            [DefaultValue("<color=#990404>Модератор вызвал вас на проверку.</color> \nНапишите свой скайп с помощью команды <color=#990404>/skype <НИК в СКАЙПЕ>.</color>\nЕсли вы покините сервер, Вы будете забанены на нашем проекте серверов.")]
            public string PlCheckText { get; set; }
            [JsonProperty(PropertyName = "Размер текста")]
            [DefaultValue(17)]
            public int PlCheckSize { get; set; }
            [JsonProperty(PropertyName = "Привилегия для команд /alert и /unalert")]
            [DefaultValue("vkbot.checkplayers")]
            public string PlCheckPerm { get; set; }
            [JsonProperty(PropertyName = "Позиция GUI AnchorMin (дефолт 0 0.826)")]
            [DefaultValue("0 0.826")]
            public string GUIAnchorMin { get; set; }
            [JsonProperty(PropertyName = "Позиция GUI AnchorMax (дефолт 1 0.965)")]
            [DefaultValue("1 0.965")]
            public string GUIAnchorMax { get; set; }
        }

        public class TopWPlPromoSet
        {
            [JsonProperty(PropertyName = "Включить топ игроков вайпа")]
            [DefaultValue(true)]
            public bool TopWPlEnabled { get; set; }
            [JsonProperty(PropertyName = "Включить отправку промо кодов за топ?")]
            [DefaultValue(false)]
            public bool TopPlPromoGift { get; set; }
            [JsonProperty(PropertyName = "Пост на стене группы о топ игроках вайпа")]
            [DefaultValue(true)]
            public bool TopPlPost { get; set; }
            [JsonProperty(PropertyName = "Промо для топ рэйдера")]
            [DefaultValue("topraider")]
            public string TopRaiderPromo { get; set; }
            [JsonProperty(PropertyName = "Промо для топ килера")]
            [DefaultValue("topkiller")]
            public string TopKillerPromo { get; set; }
            [JsonProperty(PropertyName = "Промо для топ фармера")]
            [DefaultValue("topfarmer")]
            public string TopFarmerPromo { get; set; }
            [JsonProperty(PropertyName = "Ссылка на донат магазин")]
            [DefaultValue("server.gamestores.ru")]
            public string StoreUrl { get; set; }
            [JsonProperty(PropertyName = "Автоматическая генерация промокодов после вайпа")]
            [DefaultValue(false)]
            public bool GenRandomPromo { get; set; }
        }

    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
     class DataStorageStats
    {
        public int WoodGath;
        public int SulfureGath;
        public int Rockets;
        public int Blueprints;
        public int Explosive;
        public DataStorageStats();
    }

     class DataStorageUsers
    {
        public Dictionary<ulong, VKUDATA> VKUsersData;
        public DataStorageUsers();
    }

     class VKUDATA
    {
        public ulong UserID;
        public string Name;
        public string VkID;
        public int ConfirmCode;
        public bool Confirmed;
        public bool GiftRecived;
        public string LastRaidNotice;
        public bool WipeMsg;
        public string Bdate;
        public int Raids;
        public int Kills;
        public int Farm;
    }

     DataStorageStats statdata;
     DataStorageUsers usersdata;
    private DynamicConfigFile VKBData;
    private DynamicConfigFile StatData;
     void LoadData();
     void OnServerInitialized();
     void OnServerSave();
    private void Init();
     void Unload();
     void OnNewSave(string filename);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnItemResearch(ResearchTable table, Item targetItem, BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnDispenserBonus(ResourceDispenser dispenser, BasePlayer player, Item item);
    private void OnCollectiblePickup(Item item, BasePlayer player);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
    private void CheckDeath(BasePlayer player, HitInfo info);
    private void WipeFunctions(string filename);
    private void WipeAlerts(ConsoleSystem.Arg arg);
    private void WipeAlertsSend();
    private void UStatus(ConsoleSystem.Arg arg);
     void OnPlayerBanned(string name, ulong id, string address, string reason);
    private void SendReport(BasePlayer player, string cmd, string[] args);
    private void AddBdate(BasePlayer player);
    private void CheckVkUser(BasePlayer player, string url);
    private void AddVKUser(BasePlayer player, string Userid, string bdate);
    private void VKcommand(BasePlayer player, string cmd, string[] args);
    private void GetGift(int code, string Result, BasePlayer player);
    private void GiftNotifier();
     void Update1ServerStatus();
     void UpdateMultiServerStatus();
    private void SendStatus(string key, KeyValuePair<string, string>[] replacements);
    private void MsgAdmin(ConsoleSystem.Arg arg);
    private void GetUserInfo(ConsoleSystem.Arg arg);
    private void SendVkMessage(string reciverID, string msg);
    private void SendVkWall(string msg);
    private void SendVkStatus(string msg);
     string GetUserVKId(ulong userid);
     string GetUserLastNotice(ulong userid);
     string AdminVkID();
    private void VKAPISaveLastNotice(ulong userid, string lasttime);
    private void VKAPIWall(string text, string attachments, bool atimg);
    private void VKAPIMsg(string text, string attachments, string reciverID, bool atimg);
    private void VKAPIStatus(string msg);
     void CheckOxideCommits();
     void APIResponse(int code, string response);
     void Log(string filename, string text);
     void GetCallback(int code, string response, string type);
    private string EmojiCounters(string counter);
    private string WipeDate();
    private string GetOnline();
    private static List<BasePlayer> FindPlayersOnline(string nameOrIdOrIp);
    private void StatusCheck(string msg);
    private void StartGUI(BasePlayer player);
    private void StopGui(BasePlayer player);
    private void StopCheckPlayer(BasePlayer player, string cmd, string[] args);
    private void StartCheckPlayer(BasePlayer player, string cmd, string[] args);
    private void SkypeSending(BasePlayer player, string cmd, string[] args);
    private ulong GetTopRaider();
    private ulong GetTopKiller();
    private ulong GetTopFarmer();
    private void SendPromoMsgsAndPost();
    private string PromoGenerator();
    private void SetRandomPromo();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Ключи VK API, ID группы")]
    public VKAPITokens VKAPIT { get; set; }
    [JsonProperty(PropertyName = "Настройки оповещений администраторов")]
    public AdminNotify AdmNotify { get; set; }
    [JsonProperty(PropertyName = "Настройки статуса")]
    public StatusSettings StatusStg { get; set; }
    [JsonProperty(PropertyName = "Оповещения при вайпе")]
    public WipeSettings WipeStg { get; set; }
    [JsonProperty(PropertyName = "Награда за вступление в группу")]
    public GroupGifts GrGifts { get; set; }
    [JsonProperty(PropertyName = "Награда для именинников")]
    public BDayGiftSet BDayGift { get; set; }
    [JsonProperty(PropertyName = "Настройки текста в уведомлениях в чат")]
    public TextSettings TxtSet { get; set; }
    [JsonProperty(PropertyName = "Поддержка нескольких серверов")]
    public MultipleServersSettings MltServSet { get; set; }
    [JsonProperty(PropertyName = "Проверка игроков на читы")]
    public PlayersCheckingSettings PlChkSet { get; set; }
    [JsonProperty(PropertyName = "Топ игроки вайпа и промо")]
    public TopWPlPromoSet TopWPlayersPromo { get; set; }
    public class VKAPITokens
    {
        [JsonProperty(PropertyName = "VK Token группы (для сообщений)")]
        [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
        public string VKToken { get; set; }
        [JsonProperty(PropertyName = "VK Token приложения (для записей на стене и статуса)")]
        [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
        public string VKTokenApp { get; set; }
        [JsonProperty(PropertyName = "VKID группы")]
        [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
        public string GroupID { get; set; }
    }

    public class AdminNotify
    {
        [JsonProperty(PropertyName = "VkID администраторов (пример /11111, 22222/)")]
        [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
        public string VkID { get; set; }
        [JsonProperty(PropertyName = "Включить отправку сообщений администратору командой /report ?")]
        [DefaultValue(true)]
        public bool SendReports { get; set; }
        [JsonProperty(PropertyName = "Предупреждение о злоупотреблении функцией репортов")]
        [DefaultValue("Наличие в тексте нецензурных выражений, оскорблений администрации или игроков сервера, а так же большое количество безсмысленных сообщений приведет к бану!")]
        public string ReportsNotify { get; set; }
        [JsonProperty(PropertyName = "Отправлять сообщение администратору о бане игрока?")]
        [DefaultValue(true)]
        public bool UserBannedMsg { get; set; }
        [JsonProperty(PropertyName = "Отправлять сообщение администратору о выходе нового обновления Oxide?")]
        [DefaultValue(true)]
        public bool OxideNewVersionMsg { get; set; }
    }

    public class StatusSettings
    {
        [JsonProperty(PropertyName = "Обновлять статус в группе? Если стоит /false/ статистика собираться не будет")]
        [DefaultValue(true)]
        public bool UpdateStatus { get; set; }
        [JsonProperty(PropertyName = "Вид статуса (1 - текущий сервер, 2 - список серверов, необходим Rust:IO на каждом сервере)")]
        [DefaultValue(1)]
        public int StatusSet { get; set; }
        [JsonProperty(PropertyName = "Онлайн в статусе вида '125/200'")]
        [DefaultValue(false)]
        public bool OnlWmaxslots { get; set; }
        [JsonProperty(PropertyName = "Таймер обновления статуса (минуты)")]
        [DefaultValue(30)]
        public int UpdateTimer { get; set; }
        [JsonProperty(PropertyName = "Формат статуса")]
        [DefaultValue("{usertext}. Сервер вайпнут: {wipedate}. Онлайн игроков: {onlinecounter}. Спящих: {sleepers}. Добыто дерева: {woodcounter}. Добыто серы: {sulfurecounter}. Выпущено ракет: {rocketscounter}. Использовано взрывчатки: {explosivecounter}. Создано чертежей: {blueprintsconter}. {connect}")]
        public string StatusText { get; set; }
        [JsonProperty(PropertyName = "Список счетчиков, которые будут отображаться в виде emoji")]
        [DefaultValue("onlinecounter, rocketscounter, blueprintsconter, explosivecounter, wipedate")]
        public string EmojiCounterList { get; set; }
        [JsonProperty(PropertyName = "Ссылка на коннект сервера вида /connect 111.111.111.11:11111/")]
        [DefaultValue("connect 111.111.111.11:11111")]
        public string connecturl { get; set; }
        [JsonProperty(PropertyName = "Текст для статуса")]
        [DefaultValue("Сервер 1")]
        public string StatusUT { get; set; }
    }

    public class WipeSettings
    {
        [JsonProperty(PropertyName = "Отправлять пост в группу после вайпа?")]
        [DefaultValue(false)]
        public bool WPostB { get; set; }
        [JsonProperty(PropertyName = "Текст поста о вайпе")]
        [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
        public string WPostMsg { get; set; }
        [JsonProperty(PropertyName = "Добавить изображение к посту о вайпе?")]
        [DefaultValue(false)]
        public bool WPostAttB { get; set; }
        [JsonProperty(PropertyName = "Ссылка на изображение к посту о вайпе вида 'photo-1_265827614' (изображение должно быть в альбоме группы)")]
        [DefaultValue("photo-1_265827614")]
        public string WPostAtt { get; set; }
        [JsonProperty(PropertyName = "Отправлять сообщение администратору о вайпе?")]
        [DefaultValue(true)]
        public bool WPostMsgAdmin { get; set; }
        [JsonProperty(PropertyName = "Отправлять игрокам сообщение о вайпе автоматически?")]
        [DefaultValue(false)]
        public bool WMsgPlayers { get; set; }
        [JsonProperty(PropertyName = "Текст сообщения игрокам о вайпе (сообщение отправляется только тем кто подписался командой /vk wipealerts)")]
        [DefaultValue("Сервер вайпнут! Залетай скорее!")]
        public string WMsgText { get; set; }
        [JsonProperty(PropertyName = "Игнорировать команду /vk wipealerts? (если включено, сообщение о вайпе будет отправляться всем)")]
        [DefaultValue(false)]
        public bool WCMDIgnore { get; set; }
    }

    public class GroupGifts
    {
        [JsonProperty(PropertyName = "Выдавать подарок игроку за вступление в группу ВК?")]
        [DefaultValue(true)]
        public bool VKGroupGifts { get; set; }
        [JsonProperty(PropertyName = "Подарки за вступление в группу (shortname предмета, количество)")]
        [DefaultValue(null)]
        public Dictionary<string, object> VKGroupGiftList { get; set; }
        [JsonProperty(PropertyName = "Ссылка на группу ВК")]
        [DefaultValue("vk.com/1234")]
        public string VKGroupUrl { get; set; }
        [JsonProperty(PropertyName = "Оповещения в общий чат о получении награды")]
        [DefaultValue(true)]
        public bool GiftsBool { get; set; }
        [JsonProperty(PropertyName = "Включить оповещения для игроков не получивших награду за вступление в группу?")]
        [DefaultValue(true)]
        public bool VKGGNotify { get; set; }
        [JsonProperty(PropertyName = "Интервал оповещений для игроков не получивших награду за вступление в группу (в минутах)")]
        [DefaultValue(30)]
        public int VKGGTimer { get; set; }
        [JsonProperty(PropertyName = "Выдавать награду каждый вайп?")]
        [DefaultValue(true)]
        public bool GiftsWipe { get; set; }
    }

    public class BDayGiftSet
    {
        [JsonProperty(PropertyName = "Включить награду для именинников?")]
        [DefaultValue(true)]
        public bool BDayEnabled { get; set; }
        [JsonProperty(PropertyName = "Группа для именинников")]
        [DefaultValue("bdaygroup")]
        public string BDayGroup { get; set; }
        [JsonProperty(PropertyName = "Текст поздравления")]
        [DefaultValue("<size=17><color=#990404>Администрация сервера поздравляет вас с днем рождения! В качестве подарка мы добавили вас в группу с рейтами x4 и китом bday!</color></size>")]
        public string BDayText { get; set; }
        [JsonProperty(PropertyName = "Оповещения в общий чат о имениннках")]
        [DefaultValue(false)]
        public bool BDayNotify { get; set; }
    }

    public class TextSettings
    {
        [JsonProperty(PropertyName = "Размер текста")]
        [DefaultValue("17")]
        public string TxtSize { get; set; }
        [JsonProperty(PropertyName = "Цвет выделенного текста")]
        [DefaultValue("#990404")]
        public string TxtColor { get; set; }
    }

    public class MultipleServersSettings
    {
        [JsonProperty(PropertyName = "Включить поддержку несколько серверов?")]
        [DefaultValue(false)]
        public bool MSSEnable { get; set; }
        [JsonProperty(PropertyName = "Номер сервера")]
        [DefaultValue(1)]
        public int ServerNumber { get; set; }
        [JsonProperty(PropertyName = "Сервер 1 IP:PORT (пример: 111.111.111.111:28015)")]
        [DefaultValue("none")]
        public string Server1ip { get; set; }
        [JsonProperty(PropertyName = "Сервер 2 IP:PORT (пример: 111.111.111.111:28015)")]
        [DefaultValue("none")]
        public string Server2ip { get; set; }
        [JsonProperty(PropertyName = "Сервер 3 IP:PORT (пример: 111.111.111.111:28015)")]
        [DefaultValue("none")]
        public string Server3ip { get; set; }
        [JsonProperty(PropertyName = "Сервер 4 IP:PORT (пример: 111.111.111.111:28015)")]
        [DefaultValue("none")]
        public string Server4ip { get; set; }
        [JsonProperty(PropertyName = "Сервер 5 IP:PORT (пример: 111.111.111.111:28015)")]
        [DefaultValue("none")]
        public string Server5ip { get; set; }
        [JsonProperty(PropertyName = "Онлайн в emoji?")]
        [DefaultValue(true)]
        public bool EmojiStatus { get; set; }
    }

    public class PlayersCheckingSettings
    {
        [JsonProperty(PropertyName = "Текст уведомления")]
        [DefaultValue("<color=#990404>Модератор вызвал вас на проверку.</color> \nНапишите свой скайп с помощью команды <color=#990404>/skype <НИК в СКАЙПЕ>.</color>\nЕсли вы покините сервер, Вы будете забанены на нашем проекте серверов.")]
        public string PlCheckText { get; set; }
        [JsonProperty(PropertyName = "Размер текста")]
        [DefaultValue(17)]
        public int PlCheckSize { get; set; }
        [JsonProperty(PropertyName = "Привилегия для команд /alert и /unalert")]
        [DefaultValue("vkbot.checkplayers")]
        public string PlCheckPerm { get; set; }
        [JsonProperty(PropertyName = "Позиция GUI AnchorMin (дефолт 0 0.826)")]
        [DefaultValue("0 0.826")]
        public string GUIAnchorMin { get; set; }
        [JsonProperty(PropertyName = "Позиция GUI AnchorMax (дефолт 1 0.965)")]
        [DefaultValue("1 0.965")]
        public string GUIAnchorMax { get; set; }
    }

    public class TopWPlPromoSet
    {
        [JsonProperty(PropertyName = "Включить топ игроков вайпа")]
        [DefaultValue(true)]
        public bool TopWPlEnabled { get; set; }
        [JsonProperty(PropertyName = "Включить отправку промо кодов за топ?")]
        [DefaultValue(false)]
        public bool TopPlPromoGift { get; set; }
        [JsonProperty(PropertyName = "Пост на стене группы о топ игроках вайпа")]
        [DefaultValue(true)]
        public bool TopPlPost { get; set; }
        [JsonProperty(PropertyName = "Промо для топ рэйдера")]
        [DefaultValue("topraider")]
        public string TopRaiderPromo { get; set; }
        [JsonProperty(PropertyName = "Промо для топ килера")]
        [DefaultValue("topkiller")]
        public string TopKillerPromo { get; set; }
        [JsonProperty(PropertyName = "Промо для топ фармера")]
        [DefaultValue("topfarmer")]
        public string TopFarmerPromo { get; set; }
        [JsonProperty(PropertyName = "Ссылка на донат магазин")]
        [DefaultValue("server.gamestores.ru")]
        public string StoreUrl { get; set; }
        [JsonProperty(PropertyName = "Автоматическая генерация промокодов после вайпа")]
        [DefaultValue(false)]
        public bool GenRandomPromo { get; set; }
    }

}

public class VKAPITokens
{
    [JsonProperty(PropertyName = "VK Token группы (для сообщений)")]
    [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
    public string VKToken { get; set; }
    [JsonProperty(PropertyName = "VK Token приложения (для записей на стене и статуса)")]
    [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
    public string VKTokenApp { get; set; }
    [JsonProperty(PropertyName = "VKID группы")]
    [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
    public string GroupID { get; set; }
}

public class AdminNotify
{
    [JsonProperty(PropertyName = "VkID администраторов (пример /11111, 22222/)")]
    [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
    public string VkID { get; set; }
    [JsonProperty(PropertyName = "Включить отправку сообщений администратору командой /report ?")]
    [DefaultValue(true)]
    public bool SendReports { get; set; }
    [JsonProperty(PropertyName = "Предупреждение о злоупотреблении функцией репортов")]
    [DefaultValue("Наличие в тексте нецензурных выражений, оскорблений администрации или игроков сервера, а так же большое количество безсмысленных сообщений приведет к бану!")]
    public string ReportsNotify { get; set; }
    [JsonProperty(PropertyName = "Отправлять сообщение администратору о бане игрока?")]
    [DefaultValue(true)]
    public bool UserBannedMsg { get; set; }
    [JsonProperty(PropertyName = "Отправлять сообщение администратору о выходе нового обновления Oxide?")]
    [DefaultValue(true)]
    public bool OxideNewVersionMsg { get; set; }
}

public class StatusSettings
{
    [JsonProperty(PropertyName = "Обновлять статус в группе? Если стоит /false/ статистика собираться не будет")]
    [DefaultValue(true)]
    public bool UpdateStatus { get; set; }
    [JsonProperty(PropertyName = "Вид статуса (1 - текущий сервер, 2 - список серверов, необходим Rust:IO на каждом сервере)")]
    [DefaultValue(1)]
    public int StatusSet { get; set; }
    [JsonProperty(PropertyName = "Онлайн в статусе вида '125/200'")]
    [DefaultValue(false)]
    public bool OnlWmaxslots { get; set; }
    [JsonProperty(PropertyName = "Таймер обновления статуса (минуты)")]
    [DefaultValue(30)]
    public int UpdateTimer { get; set; }
    [JsonProperty(PropertyName = "Формат статуса")]
    [DefaultValue("{usertext}. Сервер вайпнут: {wipedate}. Онлайн игроков: {onlinecounter}. Спящих: {sleepers}. Добыто дерева: {woodcounter}. Добыто серы: {sulfurecounter}. Выпущено ракет: {rocketscounter}. Использовано взрывчатки: {explosivecounter}. Создано чертежей: {blueprintsconter}. {connect}")]
    public string StatusText { get; set; }
    [JsonProperty(PropertyName = "Список счетчиков, которые будут отображаться в виде emoji")]
    [DefaultValue("onlinecounter, rocketscounter, blueprintsconter, explosivecounter, wipedate")]
    public string EmojiCounterList { get; set; }
    [JsonProperty(PropertyName = "Ссылка на коннект сервера вида /connect 111.111.111.11:11111/")]
    [DefaultValue("connect 111.111.111.11:11111")]
    public string connecturl { get; set; }
    [JsonProperty(PropertyName = "Текст для статуса")]
    [DefaultValue("Сервер 1")]
    public string StatusUT { get; set; }
}

public class WipeSettings
{
    [JsonProperty(PropertyName = "Отправлять пост в группу после вайпа?")]
    [DefaultValue(false)]
    public bool WPostB { get; set; }
    [JsonProperty(PropertyName = "Текст поста о вайпе")]
    [DefaultValue("Заполните эти поля, и выполните команду o.reload VKBot")]
    public string WPostMsg { get; set; }
    [JsonProperty(PropertyName = "Добавить изображение к посту о вайпе?")]
    [DefaultValue(false)]
    public bool WPostAttB { get; set; }
    [JsonProperty(PropertyName = "Ссылка на изображение к посту о вайпе вида 'photo-1_265827614' (изображение должно быть в альбоме группы)")]
    [DefaultValue("photo-1_265827614")]
    public string WPostAtt { get; set; }
    [JsonProperty(PropertyName = "Отправлять сообщение администратору о вайпе?")]
    [DefaultValue(true)]
    public bool WPostMsgAdmin { get; set; }
    [JsonProperty(PropertyName = "Отправлять игрокам сообщение о вайпе автоматически?")]
    [DefaultValue(false)]
    public bool WMsgPlayers { get; set; }
    [JsonProperty(PropertyName = "Текст сообщения игрокам о вайпе (сообщение отправляется только тем кто подписался командой /vk wipealerts)")]
    [DefaultValue("Сервер вайпнут! Залетай скорее!")]
    public string WMsgText { get; set; }
    [JsonProperty(PropertyName = "Игнорировать команду /vk wipealerts? (если включено, сообщение о вайпе будет отправляться всем)")]
    [DefaultValue(false)]
    public bool WCMDIgnore { get; set; }
}

public class GroupGifts
{
    [JsonProperty(PropertyName = "Выдавать подарок игроку за вступление в группу ВК?")]
    [DefaultValue(true)]
    public bool VKGroupGifts { get; set; }
    [JsonProperty(PropertyName = "Подарки за вступление в группу (shortname предмета, количество)")]
    [DefaultValue(null)]
    public Dictionary<string, object> VKGroupGiftList { get; set; }
    [JsonProperty(PropertyName = "Ссылка на группу ВК")]
    [DefaultValue("vk.com/1234")]
    public string VKGroupUrl { get; set; }
    [JsonProperty(PropertyName = "Оповещения в общий чат о получении награды")]
    [DefaultValue(true)]
    public bool GiftsBool { get; set; }
    [JsonProperty(PropertyName = "Включить оповещения для игроков не получивших награду за вступление в группу?")]
    [DefaultValue(true)]
    public bool VKGGNotify { get; set; }
    [JsonProperty(PropertyName = "Интервал оповещений для игроков не получивших награду за вступление в группу (в минутах)")]
    [DefaultValue(30)]
    public int VKGGTimer { get; set; }
    [JsonProperty(PropertyName = "Выдавать награду каждый вайп?")]
    [DefaultValue(true)]
    public bool GiftsWipe { get; set; }
}

public class BDayGiftSet
{
    [JsonProperty(PropertyName = "Включить награду для именинников?")]
    [DefaultValue(true)]
    public bool BDayEnabled { get; set; }
    [JsonProperty(PropertyName = "Группа для именинников")]
    [DefaultValue("bdaygroup")]
    public string BDayGroup { get; set; }
    [JsonProperty(PropertyName = "Текст поздравления")]
    [DefaultValue("<size=17><color=#990404>Администрация сервера поздравляет вас с днем рождения! В качестве подарка мы добавили вас в группу с рейтами x4 и китом bday!</color></size>")]
    public string BDayText { get; set; }
    [JsonProperty(PropertyName = "Оповещения в общий чат о имениннках")]
    [DefaultValue(false)]
    public bool BDayNotify { get; set; }
}

public class TextSettings
{
    [JsonProperty(PropertyName = "Размер текста")]
    [DefaultValue("17")]
    public string TxtSize { get; set; }
    [JsonProperty(PropertyName = "Цвет выделенного текста")]
    [DefaultValue("#990404")]
    public string TxtColor { get; set; }
}

public class MultipleServersSettings
{
    [JsonProperty(PropertyName = "Включить поддержку несколько серверов?")]
    [DefaultValue(false)]
    public bool MSSEnable { get; set; }
    [JsonProperty(PropertyName = "Номер сервера")]
    [DefaultValue(1)]
    public int ServerNumber { get; set; }
    [JsonProperty(PropertyName = "Сервер 1 IP:PORT (пример: 111.111.111.111:28015)")]
    [DefaultValue("none")]
    public string Server1ip { get; set; }
    [JsonProperty(PropertyName = "Сервер 2 IP:PORT (пример: 111.111.111.111:28015)")]
    [DefaultValue("none")]
    public string Server2ip { get; set; }
    [JsonProperty(PropertyName = "Сервер 3 IP:PORT (пример: 111.111.111.111:28015)")]
    [DefaultValue("none")]
    public string Server3ip { get; set; }
    [JsonProperty(PropertyName = "Сервер 4 IP:PORT (пример: 111.111.111.111:28015)")]
    [DefaultValue("none")]
    public string Server4ip { get; set; }
    [JsonProperty(PropertyName = "Сервер 5 IP:PORT (пример: 111.111.111.111:28015)")]
    [DefaultValue("none")]
    public string Server5ip { get; set; }
    [JsonProperty(PropertyName = "Онлайн в emoji?")]
    [DefaultValue(true)]
    public bool EmojiStatus { get; set; }
}

public class PlayersCheckingSettings
{
    [JsonProperty(PropertyName = "Текст уведомления")]
    [DefaultValue("<color=#990404>Модератор вызвал вас на проверку.</color> \nНапишите свой скайп с помощью команды <color=#990404>/skype <НИК в СКАЙПЕ>.</color>\nЕсли вы покините сервер, Вы будете забанены на нашем проекте серверов.")]
    public string PlCheckText { get; set; }
    [JsonProperty(PropertyName = "Размер текста")]
    [DefaultValue(17)]
    public int PlCheckSize { get; set; }
    [JsonProperty(PropertyName = "Привилегия для команд /alert и /unalert")]
    [DefaultValue("vkbot.checkplayers")]
    public string PlCheckPerm { get; set; }
    [JsonProperty(PropertyName = "Позиция GUI AnchorMin (дефолт 0 0.826)")]
    [DefaultValue("0 0.826")]
    public string GUIAnchorMin { get; set; }
    [JsonProperty(PropertyName = "Позиция GUI AnchorMax (дефолт 1 0.965)")]
    [DefaultValue("1 0.965")]
    public string GUIAnchorMax { get; set; }
}

public class TopWPlPromoSet
{
    [JsonProperty(PropertyName = "Включить топ игроков вайпа")]
    [DefaultValue(true)]
    public bool TopWPlEnabled { get; set; }
    [JsonProperty(PropertyName = "Включить отправку промо кодов за топ?")]
    [DefaultValue(false)]
    public bool TopPlPromoGift { get; set; }
    [JsonProperty(PropertyName = "Пост на стене группы о топ игроках вайпа")]
    [DefaultValue(true)]
    public bool TopPlPost { get; set; }
    [JsonProperty(PropertyName = "Промо для топ рэйдера")]
    [DefaultValue("topraider")]
    public string TopRaiderPromo { get; set; }
    [JsonProperty(PropertyName = "Промо для топ килера")]
    [DefaultValue("topkiller")]
    public string TopKillerPromo { get; set; }
    [JsonProperty(PropertyName = "Промо для топ фармера")]
    [DefaultValue("topfarmer")]
    public string TopFarmerPromo { get; set; }
    [JsonProperty(PropertyName = "Ссылка на донат магазин")]
    [DefaultValue("server.gamestores.ru")]
    public string StoreUrl { get; set; }
    [JsonProperty(PropertyName = "Автоматическая генерация промокодов после вайпа")]
    [DefaultValue(false)]
    public bool GenRandomPromo { get; set; }
}

 class DataStorageStats
{
    public int WoodGath;
    public int SulfureGath;
    public int Rockets;
    public int Blueprints;
    public int Explosive;
    public DataStorageStats();
}

 class DataStorageUsers
{
    public Dictionary<ulong, VKUDATA> VKUsersData;
    public DataStorageUsers();
}

 class VKUDATA
{
    public ulong UserID;
    public string Name;
    public string VkID;
    public int ConfirmCode;
    public bool Confirmed;
    public bool GiftRecived;
    public string LastRaidNotice;
    public bool WipeMsg;
    public string Bdate;
    public int Raids;
    public int Kills;
    public int Farm;
}


```

---

## VKRaidAlert

```csharp
using UnityEngine;
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Linq;
using System.Globalization;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("VKRaidAlert", "SkiTles", "1.1")]
 class VKRaidAlert : RustPlugin
{
    [PluginReference]
     Plugin VKBot;
     DateTime LNT;
     class DataStorage
    {
        public Dictionary<ulong, VKRADATA> VKRaidAlertData;
        public DataStorage();
    }

     class VKRADATA
    {
        public string LastOnlNotice;
    }

     DataStorage data;
    private DynamicConfigFile VKRData;
     void OnServerInitialized();
    private string permissionvk;
    private string permissionol;
    private int timeallowed;
    private int tallowedol;
    private string raidnotice;
    private bool MsgAttB;
    private string MsgAtt;
    private string raidnoticeol;
     List<string> allowedentity;
    protected override void LoadDefaultConfig();
    private void LoadConfigValues();
     void Loaded();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo hitInfo);
     bool IsOnline(ulong id);
     BasePlayer OnlinePlayer(ulong id);
     TimeSpan TimeLeft(DateTime time);
    private void SendOnlinenotice(BasePlayer victim, string raidername);
     void SendOfflineMessage(ulong id, string raidername);
    private string TextReplace(string key, KeyValuePair<string, string>[] replacements);
    private void GetConfig(string Key, T var);
     void Log(string filename, string text);
     void LoadData();
}

 class DataStorage
{
    public Dictionary<ulong, VKRADATA> VKRaidAlertData;
    public DataStorage();
}

 class VKRADATA
{
    public string LastOnlNotice;
}


```

---

## VoteBonusFromLauncher

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Plugins;

Oxide.Plugins
[Oxide.Plugins.Info("Vote Bonus From Launcher", "TheRyuzaki", "0.0.2")]
public class VoteBonusFromLauncher : RustPlugin
{
    private const string CONST_SHOP_ID;
    private const string CONST_SHOP_KEY;
    private const int CONST_BONUS_RUB;
    private const bool CONST_SAY_BONUS;
    private const string CONST_ADDRES_SERVER;
    private HashSet<int> HashSetVotes { get; set; }
    private Timer timerSearchVote { get; set; }
    private Queue<String> ListMessageToMainTheard;
    [HookMethod("OnServerInitialized")]
     void OnServerInitialized();
    [HookMethod("Unload")]
     void Unload();
     void OnStartSearchVote();
    private void OnResonseSearchVote(int code, string response);
    private void OnSendBonusVote(ulong steamid);
    private void OnResponseBonusVote(int code, string response);
}


```

---

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

