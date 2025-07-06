# uMod Plugins Dataset - Code Abstractions (Continued)

Chunk 8 - Generated: 2025-07-06 20:25:18

## RemoverTool by Tryhard - Building and entity removal tool

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using VLB;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Remover Tool", "Reneb/Fuji/Arainrr/Tryhard", "4.3.43", ResourceId = 651)]
[Description("Building and entity removal tool")]
public class RemoverTool : RustPlugin
{
    [PluginReference]
    private readonly Plugin Friends;
    private readonly Plugin ServerRewards;
    private readonly Plugin Clans;
    private readonly Plugin Economics;
    private readonly Plugin ImageLibrary;
    private readonly Plugin BuildingOwners;
    private readonly Plugin RustTranslationAPI;
    private readonly Plugin NoEscape;
    private const string ECONOMICS_KEY;
    private const string SERVER_REWARDS_KEY;
    private const string PERMISSION_ALL;
    private const string PERMISSION_ADMIN;
    private const string PERMISSION_NORMAL;
    private const string PERMISSION_TARGET;
    private const string PERMISSION_EXTERNAL;
    private const string PERMISSION_OVERRIDE;
    private const string PERMISSION_STRUCTURE;
    private const string PREFAB_ITEM_DROP;
    private const int LAYER_ALL;
    private const int LAYER_TARGET;
    private static RemoverTool _instance;
    private static BUTTON _removeButton;
    private static RemoveMode _removeMode;
    private readonly object _false;
    private bool _configChanged;
    private bool _removeOverride;
    private Coroutine _removeAllCoroutine;
    private Coroutine _removeStructureCoroutine;
    private Coroutine _removeExternalCoroutine;
    private Coroutine _removePlayerEntityCoroutine;
    private StringBuilder _debugStringBuilder;
    private Hash<ulong, float> _entitySpawnedTimes;
    private readonly Hash<ulong, float> _cooldownTimes;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnEntitySpawned(BaseEntity entity);
    private void OnEntityKill(BaseEntity entity);
    private object OnPlayerAttack(BasePlayer player, HitInfo info);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private readonly HashSet<Construction> _constructions;
    private readonly Dictionary<string, int> _itemShortNameToItemId;
    private readonly Dictionary<string, string> _prefabNameToStructure;
    private readonly Dictionary<string, string> _shortPrefabNameToDeployable;
    private void Initialize();
    private static string GetRemoveTypeName(RemoveType removeType);
    private static void DropItemContainer(ItemContainer itemContainer, Vector3 position, Quaternion rotation);
    private static bool IsExternalWall(StabilityEntity stabilityEntity);
    private static bool CanEntityBeDisplayed(BaseEntity entity, BasePlayer player);
    private static bool CanEntityBeSaved(BaseEntity entity);
    private static bool HasEntityEnabled(BaseEntity entity);
    private static bool IsRemovableEntity(BaseEntity entity);
    private static string GetEntityImage(string name);
    private static string GetItemImage(string shortname);
    private static void TryFindEntityName(BasePlayer player, BaseEntity entity, string displayName, string imageName);
    private static string GetDisplayNameByCurrencyName(string language, string currencyName, long skinId);
    private static string GetCurrencyDisplayName(string currencyName, string defaultName, bool readOnly);
    private static PermissionSettings GetPermissionSettings(BasePlayer player);
    private static Vector2 GetAnchor(string anchor);
    private static bool AddImageToLibrary(string url, string shortname, ulong skin);
    private static string GetImageFromLibrary(string shortname, ulong skin, bool returnUrl);
    private void OnRaidBlock(BasePlayer player);
    private void OnCombatBlock(BasePlayer player);
    private bool IsPlayerBlocked(BasePlayer player, string reason);
    private bool IsRaidBlocked(string playerID);
    private bool IsCombatBlocked(string playerID);
    private static class UI
    {
        public static CuiElementContainer CreateElementContainer(string parent, string panelName, string backgroundColor, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursor);
        public static void CreatePanel(CuiElementContainer container, string panelName, string backgroundColor, string anchorMin, string anchorMax, bool cursor);
        public static void CreateLabel(CuiElementContainer container, string panelName, string textColor, string text, int fontSize, string anchorMin, string anchorMax, TextAnchor align, float fadeIn);
        public static void CreateImage(CuiElementContainer container, string panelName, string image, string anchorMin, string anchorMax, string color);
        public static void CreateImage(CuiElementContainer container, string panelName, int itemId, ulong skinId, string anchorMin, string anchorMax);
    }

    private const string UINAME_MAIN;
    private const string UINAME_TIMELEFT;
    private const string UINAME_ENTITY;
    private const string UINAME_PRICE;
    private const string UINAME_REFUND;
    private const string UINAME_AUTH;
    private const string UINAME_CROSSHAIR;
    private static void CreateCrosshairUI(BasePlayer player);
    private static void CreateMainUI(BasePlayer player, RemoveType removeType);
    private static void UpdateTimeLeftUI(BasePlayer player, RemoveType removeType, int timeLeft, int currentRemoved, int maxRemovable);
    private static void UpdateEntityUI(BasePlayer player, BaseEntity targetEntity, RemovableEntityInfo? info);
    private static void UpdatePriceUI(BasePlayer player, BaseEntity targetEntity, RemovableEntityInfo? info, bool usePrice);
    private static void UpdateRefundUI(BasePlayer player, BaseEntity targetEntity, RemovableEntityInfo? info, bool useRefund);
    private static void UpdateAuthorizationUI(BasePlayer player, RemoveType removeType, BaseEntity targetEntity, RemovableEntityInfo? info, bool shouldPay);
    private static void DestroyAllUI(BasePlayer player);
    private static void DestroyUiEntry(BasePlayer player, UiEntry uiEntry);
    private static bool IsSpecificTool(BasePlayer player);
    private static bool IsSpecificTool(Item heldItem);
    private static bool IsMeleeTool(BasePlayer player);
    private static bool IsMeleeTool(Item heldItem);
    private class ToolRemover : FacepunchBehaviour
    {
        private const float MinInterval;
        public int CurrentRemoved { get; set; }
        public BaseEntity HitEntity { get; set; }
        public bool CanOverride { get; set; }
        public BasePlayer Player { get; set; }
        public RemoveType RemoveType { get; set; }
        private bool _resetTime;
        private bool _shouldPay;
        private bool _shouldRefund;
        private int _removeTime;
        private int _maxRemovable;
        private float _distance;
        private float _removeInterval;
        private int _timeLeft;
        private float _lastRemove;
        private ItemId _currentItemId;
        private bool _disableInHand;
        private Item _lastHeldItem;
        private BaseEntity _targetEntity;
        private UiEntry _activeUiEntries;
        private void Awake();
        public void Init(RemoveType removeType, int removeTime, int maxRemovable, float distance, float removeInterval, bool shouldPay, bool shouldRefund, bool resetTime, bool canOverride);
        private void RemoveUpdate();
        private BaseEntity GetTargetEntity();
        private void Update();
        private void UnEquip();
        private bool HandleUiEntry(UiEntry uiEntry, bool canShow);
        public void DisableTool(bool showMessage);
        private void OnDestroy();
    }

    private bool TryRemove(BasePlayer player, BaseEntity targetEntity, RemoveType removeType, bool shouldPay, bool shouldRefund);
    private bool CanRemoveEntity(BasePlayer player, RemoveType removeType, BaseEntity targetEntity, RemovableEntityInfo? info, bool shouldPay, string reason);
    private bool HasAccess(BasePlayer player, BaseEntity targetEntity);
    private static bool HasTotalAccess(BasePlayer player, BaseEntity targetEntity);
    private static bool CanOpenAllLocks(BasePlayer player, BaseEntity targetEntity);
    private static bool OnTryToOpen(BasePlayer player, BaseLock baseLock);
    private static bool IsDamagedEntity(BaseEntity entity);
    private static bool IsEntityTimeLimit(BaseEntity entity);
    private static void DropContainerEntity(BaseEntity targetEntity);
    private bool AreFriends(ulong playerID, ulong friendID);
    private static bool SameTeam(ulong playerID, ulong friendID);
    private bool HasFriend(ulong playerID, ulong friendID);
    private bool SameClan(ulong playerID, ulong friendID);
    private Dictionary<string, CurrencyInfo> GetPrice(BaseEntity targetEntity, RemovableEntityInfo? info);
    private bool TryPay(BasePlayer player, BaseEntity targetEntity, RemovableEntityInfo? info);
    private bool CanPay(BasePlayer player, BaseEntity targetEntity, RemovableEntityInfo? info);
    private bool CheckOrPay(BaseEntity targetEntity, BasePlayer player, string itemName, CurrencyInfo currencyInfo, bool check);
    private static int GetInventoryAmount(BasePlayer player, int itemId, CurrencyInfo currencyInfo);
    private static int TakeInventory(BasePlayer player, int itemId, CurrencyInfo currencyInfo, List<Item> collect);
    private void GiveRefund(BasePlayer player, BaseEntity targetEntity, RemovableEntityInfo? info);
    private Dictionary<string, CurrencyInfo> GetRefund(BaseEntity targetEntity, RemovableEntityInfo? info);
    private IEnumerable<string> GetSlots(BaseEntity targetEntity);
    private IEnumerator RemoveAll(BaseEntity sourceEntity, BasePlayer player);
    private IEnumerator RemoveExternal(StabilityEntity sourceEntity, BasePlayer player);
    private IEnumerator RemoveStructure(DecayEntity sourceEntity, BasePlayer player);
    private IEnumerator RemovePlayerEntity(ConsoleSystem.Arg arg, ulong targetID, PlayerEntityRemoveType playerEntityRemoveType);
    private IEnumerator DelayRemove(IEnumerable<BaseEntity> entities, BasePlayer player, RemoveType removeType);
    private static IEnumerator GetNearbyEntities(T sourceEntity, HashSet<T> removeList, int layers, Func<T, bool> filter);
    private static IEnumerator ProcessContainers(HashSet<BaseEntity> removeList);
    private static IEnumerator ProcessBuilding(DecayEntity sourceEntity, HashSet<BaseEntity> removeList);
    private static void ProcessContainer(BaseEntity entity);
    private static bool DoRemove(BaseEntity entity, BaseNetworkable.DestroyMode destroyMode);
    private static void DoNormalRemove(BasePlayer player, BaseEntity entity, bool gibs);
    private static RemovableEntityInfo? GetRemovableEntityInfo(BaseEntity entity, BasePlayer player);
    private bool IsToolRemover(BasePlayer player);
    private string GetPlayerRemoveType(BasePlayer player);
    private void CmdRemove(BasePlayer player, string command, string[] args);
    private bool ToggleRemove(BasePlayer player, RemoveType removeType, int time);
    [ConsoleCommand("remove.toggle")]
    private void CCmdRemoveToggle(ConsoleSystem.Arg arg);
    [ConsoleCommand("remove.target")]
    private void CCmdRemoveTarget(ConsoleSystem.Arg arg);
    [ConsoleCommand("remove.building")]
    private void CCmdConstruction(ConsoleSystem.Arg arg);
    [ConsoleCommand("remove.allow")]
    private void CCmdRemoveAllow(ConsoleSystem.Arg arg);
    [ConsoleCommand("remove.playerentity")]
    private void CCmdRemoveEntity(ConsoleSystem.Arg arg);
    private void PrintDebug(string message, bool warning);
    private void SaveDebug();
    private string GetItemTranslationByShortName(string language, string itemShortName);
    private string GetConstructionTranslation(string language, string prefabName);
    private string GetDeployableTranslation(string language, string deployable);
    private string GetItemDisplayName(string language, string itemShortName);
    private string GetConstructionDisplayName(BasePlayer player, string shortPrefabName, string displayName);
    private string GetDeployableDisplayName(BasePlayer player, string deployable, string displayName);
    private void UpdateConfig();
    private static ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public readonly GlobalSettings global;
        [JsonProperty(PropertyName = "Container Settings")]
        public readonly ContainerSettings container;
        [JsonProperty(PropertyName = "Remove Damaged Entities")]
        public readonly DamagedEntitySettings damagedEntity;
        [JsonProperty(PropertyName = "Chat Settings")]
        public readonly ChatSettings chat;
        [JsonProperty(PropertyName = "Permission Settings (Just for normal type)")]
        public readonly Dictionary<string, PermissionSettings> permission;
        [JsonProperty(PropertyName = "Remove Type Settings")]
        public readonly Dictionary<RemoveType, RemoveTypeSettings> removeType;
        [JsonProperty(PropertyName = "Remove Mode Settings (Only one model works)")]
        public readonly RemoverModeSettings removerMode;
        [JsonProperty(PropertyName = "NoEscape Settings")]
        public readonly NoEscapeSettings noEscape;
        [JsonProperty(PropertyName = "Image Urls (Used to UI image)")]
        public readonly Dictionary<string, string> imageUrls;
        [JsonProperty(PropertyName = "GUI")]
        public readonly UiSettings ui;
        [JsonProperty(PropertyName = "Remove Info (Refund & Price)")]
        public readonly RemoveSettings remove;
        [JsonProperty(PropertyName = "Version")]
        public VersionNumber version;
    }

    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Enable Debug Mode")]
        public bool debugEnabled;
        [JsonProperty(PropertyName = "Log Debug To File")]
        public bool logToFile;
        [JsonProperty(PropertyName = "Use Teams")]
        public bool useTeams;
        [JsonProperty(PropertyName = "Use Clans")]
        public bool useClans;
        [JsonProperty(PropertyName = "Use Friends")]
        public bool useFriends;
        [JsonProperty(PropertyName = "Use Entity Owners")]
        public bool useEntityOwners;
        [JsonProperty(PropertyName = "Use Building Locks")]
        public bool useBuildingLocks;
        [JsonProperty(PropertyName = "Use Tool Cupboards (Strongly unrecommended)")]
        public bool useToolCupboards;
        [JsonProperty(PropertyName = "Use Building Owners (You will need BuildingOwners plugin)")]
        public bool useBuildingOwners;
        [JsonProperty(PropertyName = "Remove Button")]
        public string removeButton;
        [JsonProperty(PropertyName = "Remove Interval (Min = 0.2)")]
        public float removeInterval;
        [JsonProperty(PropertyName = "Only start cooldown when an entity is removed")]
        public bool startCooldownOnRemoved;
        [JsonProperty(PropertyName = "RemoveType - All/Structure - Remove per frame")]
        public int removePerFrame;
        [JsonProperty(PropertyName = "RemoveType - All/Structure - No item container dropped")]
        public bool noItemContainerDrop;
        [JsonProperty(PropertyName = "RemoveType - Normal - Max Removable Objects - Exclude admins")]
        public bool maxRemovableExclude;
        [JsonProperty(PropertyName = "RemoveType - Normal - Cooldown - Exclude admins")]
        public bool cooldownExclude;
        [JsonProperty(PropertyName = "RemoveType - Normal - Entity Spawned Time Limit - Enabled")]
        public bool entityTimeLimit;
        [JsonProperty(PropertyName = "RemoveType - Normal - Entity Spawned Time Limit - Cannot be removed when entity spawned time more than it")]
        public float limitTime;
        [JsonProperty(PropertyName = "Default Entity Settings (When automatically adding new entities to 'Other Entity Settings')")]
        public DefaultEntitySettings defaultEntity;
        public class DefaultEntitySettings
        {
            [JsonProperty(PropertyName = "Default Remove Allowed")]
            public bool removeAllowed;
        }

    }

    public class ContainerSettings
    {
        [JsonProperty(PropertyName = "Storage Container - Enable remove of not empty storages")]
        public bool removeNotEmptyStorage;
        [JsonProperty(PropertyName = "Storage Container - Drop items from container")]
        public bool dropItemsStorage;
        [JsonProperty(PropertyName = "Storage Container - Drop a item container from container")]
        public bool dropContainerStorage;
        [JsonProperty(PropertyName = "IOEntity Container - Enable remove of not empty storages")]
        public bool removeNotEmptyIoEntity;
        [JsonProperty(PropertyName = "IOEntity Container - Drop items from container")]
        public bool dropItemsIoEntity;
        [JsonProperty(PropertyName = "IOEntity Container - Drop a item container from container")]
        public bool dropContainerIoEntity;
    }

    public class DamagedEntitySettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled;
        [JsonProperty(PropertyName = "Exclude Quarries")]
        public bool excludeQuarries;
        [JsonProperty(PropertyName = "Exclude Building Blocks")]
        public bool excludeBuildingBlocks;
        [JsonProperty(PropertyName = "Percentage (Can be removed when (health / max health * 100) is not less than it)")]
        public float percentage;
    }

    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat Command")]
        public string command;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong steamIDIcon;
        [JsonProperty(PropertyName = "Show Message When Enabled/Disabled")]
        public bool showMessageWhenEnabledOrDisabled;
    }

    public class PermissionSettings
    {
        [JsonProperty(PropertyName = "Priority")]
        public int priority;
        [JsonProperty(PropertyName = "Distance")]
        public float distance;
        [JsonProperty(PropertyName = "Cooldown")]
        public float cooldown;
        [JsonProperty(PropertyName = "Max Time")]
        public int maxTime;
        [JsonProperty(PropertyName = "Remove Interval (Min = 0.2)")]
        public float removeInterval;
        [JsonProperty(PropertyName = "Max Removable Objects (0 = Unlimited)")]
        public int maxRemovable;
        [JsonProperty(PropertyName = "Pay")]
        public bool pay;
        [JsonProperty(PropertyName = "Refund")]
        public bool refund;
        [JsonProperty(PropertyName = "Reset the time after removing an entity")]
        public bool resetTime;
    }

    public class RemoveTypeSettings
    {
        [JsonProperty(PropertyName = "Display Name")]
        public string displayName;
        [JsonProperty(PropertyName = "Distance")]
        public float distance;
        [JsonProperty(PropertyName = "Default Time")]
        public int defaultTime;
        [JsonProperty(PropertyName = "Max Time")]
        public int maxTime;
        [JsonProperty(PropertyName = "Gibs")]
        public bool gibs;
        [JsonProperty(PropertyName = "Reset the time after removing an entity")]
        public bool resetTime;
    }

    public class RemoverModeSettings
    {
        [JsonProperty(PropertyName = "No Held Item Mode")]
        public bool noHeldMode;
        [JsonProperty(PropertyName = "No Held Item Mode - Disable remover tool when you have any item in hand")]
        public bool noHeldDisableInHand;
        [JsonProperty(PropertyName = "Melee Tool Hit Mode")]
        public bool meleeHitMode;
        [JsonProperty(PropertyName = "Melee Tool Hit Mode - Item shortname")]
        public string meleeHitItemShortname;
        [JsonProperty(PropertyName = "Melee Tool Hit Mode - Item skin (-1 = All skins)")]
        public long meleeHitModeSkin;
        [JsonProperty(PropertyName = "Melee Tool Hit Mode - Auto enable remover tool when you hold a melee tool")]
        public bool meleeHitEnableInHand;
        [JsonProperty(PropertyName = "Melee Tool Hit Mode - Requires a melee tool in your hand when remover tool is enabled")]
        public bool meleeHitRequires;
        [JsonProperty(PropertyName = "Melee Tool Hit Mode - Disable remover tool when you are not holding a melee tool")]
        public bool meleeHitDisableInHand;
        [JsonProperty(PropertyName = "Specific Tool Mode")]
        public bool specificToolMode;
        [JsonProperty(PropertyName = "Specific Tool Mode - Item shortname")]
        public string specificToolShortName;
        [JsonProperty(PropertyName = "Specific Tool Mode - Item skin (-1 = All skins)")]
        public long specificToolSkin;
        [JsonProperty(PropertyName = "Specific Tool Mode - Auto enable remover tool when you hold a specific tool")]
        public bool specificToolEnableInHand;
        [JsonProperty(PropertyName = "Specific Tool Mode - Requires a specific tool in your hand when remover tool is enabled")]
        public bool specificToolRequires;
        [JsonProperty(PropertyName = "Specific Tool Mode - Disable remover tool when you are not holding a specific tool")]
        public bool specificToolDisableInHand;
    }

    public class NoEscapeSettings
    {
        [JsonProperty(PropertyName = "Use Raid Blocker")]
        public bool useRaidBlocker;
        [JsonProperty(PropertyName = "Use Combat Blocker")]
        public bool useCombatBlocker;
    }

    public class UiSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled;
        [JsonProperty(PropertyName = "Main Box - Min Anchor (in Rust Window)")]
        public string removerToolAnchorMin;
        [JsonProperty(PropertyName = "Main Box - Max Anchor (in Rust Window)")]
        public string removerToolAnchorMax;
        [JsonProperty(PropertyName = "Main Box - Min Offset (in Rust Window)")]
        public string removerToolOffsetMin;
        [JsonProperty(PropertyName = "Main Box - Max Offset (in Rust Window)")]
        public string removerToolOffsetMax;
        [JsonProperty(PropertyName = "Main Box - Background Color")]
        public string removerToolBackgroundColor;
        [JsonProperty(PropertyName = "Remove Title - Box - Min Anchor (in Main Box)")]
        public string removeAnchorMin;
        [JsonProperty(PropertyName = "Remove Title - Box - Max Anchor (in Main Box)")]
        public string removeAnchorMax;
        [JsonProperty(PropertyName = "Remove Title - Box - Background Color")]
        public string removeBackgroundColor;
        [JsonProperty(PropertyName = "Remove Title - Text - Min Anchor (in Main Box)")]
        public string removeTextAnchorMin;
        [JsonProperty(PropertyName = "Remove Title - Text - Max Anchor (in Main Box)")]
        public string removeTextAnchorMax;
        [JsonProperty(PropertyName = "Remove Title - Text - Text Color")]
        public string removeTextColor;
        [JsonProperty(PropertyName = "Remove Title - Text - Text Size")]
        public int removeTextSize;
        [JsonProperty(PropertyName = "Timeleft - Box - Min Anchor (in Main Box)")]
        public string timeLeftAnchorMin;
        [JsonProperty(PropertyName = "Timeleft - Box - Max Anchor (in Main Box)")]
        public string timeLeftAnchorMax;
        [JsonProperty(PropertyName = "Timeleft - Box - Background Color")]
        public string timeLeftBackgroundColor;
        [JsonProperty(PropertyName = "Timeleft - Text - Min Anchor (in Timeleft Box)")]
        public string timeLeftTextAnchorMin;
        [JsonProperty(PropertyName = "Timeleft - Text - Max Anchor (in Timeleft Box)")]
        public string timeLeftTextAnchorMax;
        [JsonProperty(PropertyName = "Timeleft - Text - Text Color")]
        public string timeLeftTextColor;
        [JsonProperty(PropertyName = "Timeleft - Text - Text Size")]
        public int timeLeftTextSize;
        [JsonProperty(PropertyName = "Entity - Box - Min Anchor (in Main Box)")]
        public string entityAnchorMin;
        [JsonProperty(PropertyName = "Entity - Box - Max Anchor (in Main Box)")]
        public string entityAnchorMax;
        [JsonProperty(PropertyName = "Entity - Box - Background Color")]
        public string entityBackgroundColor;
        [JsonProperty(PropertyName = "Entity - Text - Min Anchor (in Entity Box)")]
        public string entityTextAnchorMin;
        [JsonProperty(PropertyName = "Entity - Text - Max Anchor (in Entity Box)")]
        public string entityTextAnchorMax;
        [JsonProperty(PropertyName = "Entity - Text - Text Color")]
        public string entityTextColor;
        [JsonProperty(PropertyName = "Entity - Text - Text Size")]
        public int entityTextSize;
        [JsonProperty(PropertyName = "Entity - Image - Enabled")]
        public bool entityImageEnabled;
        [JsonProperty(PropertyName = "Entity - Image - Min Anchor (in Entity Box)")]
        public string entityImageAnchorMin;
        [JsonProperty(PropertyName = "Entity - Image - Max Anchor (in Entity Box)")]
        public string entityImageAnchorMax;
        [JsonProperty(PropertyName = "Authorization Check Enabled")]
        public bool authorizationEnabled;
        [JsonProperty(PropertyName = "Authorization Check - Box - Min Anchor (in Main Box)")]
        public string authorizationsAnchorMin;
        [JsonProperty(PropertyName = "Authorization Check - Box - Max Anchor (in Main Box)")]
        public string authorizationsAnchorMax;
        [JsonProperty(PropertyName = "Authorization Check - Box - Allowed Background Color")]
        public string allowedBackgroundColor;
        [JsonProperty(PropertyName = "Authorization Check - Box - Refused Background Color")]
        public string refusedBackgroundColor;
        [JsonProperty(PropertyName = "Authorization Check - Text - Min Anchor (in Authorization Check Box)")]
        public string authorizationsTextAnchorMin;
        [JsonProperty(PropertyName = "Authorization Check - Text - Max Anchor (in Authorization Check Box)")]
        public string authorizationsTextAnchorMax;
        [JsonProperty(PropertyName = "Authorization Check - Text - Text Color")]
        public string authorizationsTextColor;
        [JsonProperty(PropertyName = "Authorization Check Box - Text - Text Size")]
        public int authorizationsTextSize;
        [JsonProperty(PropertyName = "Price & Refund - Image Enabled")]
        public bool imageEnabled;
        [JsonProperty(PropertyName = "Price & Refund - Image Scale")]
        public float imageScale;
        [JsonProperty(PropertyName = "Price & Refund - Distance of image from right border")]
        public float rightDistance;
        [JsonProperty(PropertyName = "Price Enabled")]
        public bool priceEnabled;
        [JsonProperty(PropertyName = "Price - Box - Min Anchor (in Main Box)")]
        public string priceAnchorMin;
        [JsonProperty(PropertyName = "Price - Box - Max Anchor (in Main Box)")]
        public string priceAnchorMax;
        [JsonProperty(PropertyName = "Price - Box - Background Color")]
        public string priceBackgroundColor;
        [JsonProperty(PropertyName = "Price - Text - Min Anchor (in Price Box)")]
        public string priceTextAnchorMin;
        [JsonProperty(PropertyName = "Price - Text - Max Anchor (in Price Box)")]
        public string priceTextAnchorMax;
        [JsonProperty(PropertyName = "Price - Text - Text Color")]
        public string priceTextColor;
        [JsonProperty(PropertyName = "Price - Text - Text Size")]
        public int priceTextSize;
        [JsonProperty(PropertyName = "Price - Text2 - Min Anchor (in Price Box)")]
        public string price2TextAnchorMin;
        [JsonProperty(PropertyName = "Price - Text2 - Max Anchor (in Price Box)")]
        public string price2TextAnchorMax;
        [JsonProperty(PropertyName = "Price - Text2 - Text Color")]
        public string price2TextColor;
        [JsonProperty(PropertyName = "Price - Text2 - Text Size")]
        public int price2TextSize;
        [JsonProperty(PropertyName = "Refund Enabled")]
        public bool refundEnabled;
        [JsonProperty(PropertyName = "Refund - Box - Min Anchor (in Main Box)")]
        public string refundAnchorMin;
        [JsonProperty(PropertyName = "Refund - Box - Max Anchor (in Main Box)")]
        public string refundAnchorMax;
        [JsonProperty(PropertyName = "Refund - Box - Background Color")]
        public string refundBackgroundColor;
        [JsonProperty(PropertyName = "Refund - Text - Min Anchor (in Refund Box)")]
        public string refundTextAnchorMin;
        [JsonProperty(PropertyName = "Refund - Text - Max Anchor (in Refund Box)")]
        public string refundTextAnchorMax;
        [JsonProperty(PropertyName = "Refund - Text - Text Color")]
        public string refundTextColor;
        [JsonProperty(PropertyName = "Refund - Text - Text Size")]
        public int refundTextSize;
        [JsonProperty(PropertyName = "Refund - Text2 - Min Anchor (in Refund Box)")]
        public string refund2TextAnchorMin;
        [JsonProperty(PropertyName = "Refund - Text2 - Max Anchor (in Refund Box)")]
        public string refund2TextAnchorMax;
        [JsonProperty(PropertyName = "Refund - Text2 - Text Color")]
        public string refund2TextColor;
        [JsonProperty(PropertyName = "Refund - Text2 - Text Size")]
        public int refund2TextSize;
        [JsonProperty(PropertyName = "Crosshair - Enabled")]
        public bool showCrosshair;
        [JsonProperty(PropertyName = "Crosshair - Image Url")]
        public string crosshairImageUrl;
        [JsonProperty(PropertyName = "Crosshair - Box - Min Anchor (in Rust Window)")]
        public string crosshairAnchorMin;
        [JsonProperty(PropertyName = "Crosshair - Box - Max Anchor (in Rust Window)")]
        public string crosshairAnchorMax;
        [JsonProperty(PropertyName = "Crosshair - Box - Min Offset (in Rust Window)")]
        public string crosshairOffsetMin;
        [JsonProperty(PropertyName = "Crosshair - Box - Max Offset (in Rust Window)")]
        public string crosshairOffsetMax;
        [JsonProperty(PropertyName = "Crosshair - Box - Image Color")]
        public string crosshairColor;
        [JsonIgnore]
        public Vector2 Price2TextAnchorMin;
        public Vector2 Price2TextAnchorMax;
        public Vector2 Refund2TextAnchorMin;
        public Vector2 Refund2TextAnchorMax;
    }

    public class RemoveSettings
    {
        [JsonProperty(PropertyName = "Price Enabled")]
        public bool priceEnabled;
        [JsonProperty(PropertyName = "Refund Enabled")]
        public bool refundEnabled;
        [JsonProperty(PropertyName = "Refund Items In Entity Slot")]
        public bool refundSlot;
        [JsonProperty(PropertyName = "Allowed Building Grade")]
        public Dictionary<BuildingGrade.Enum, bool> validConstruction;
        [JsonProperty(PropertyName = "Display Names (Refund & Price)")]
        public readonly Dictionary<string, string> displayNames;
        [JsonProperty(PropertyName = "Building Blocks Settings")]
        public Dictionary<string, BuildingBlocksSettings> buildingBlock;
        [JsonProperty(PropertyName = "Other Entity Settings")]
        public Dictionary<string, EntitySettings> entity;
    }

    public class BuildingBlocksSettings
    {
        [JsonProperty(PropertyName = "Display Name")]
        public string displayName;
        [JsonProperty(PropertyName = "Building Grade")]
        public Dictionary<BuildingGrade.Enum, BuildingGradeSettings> buildingGrade;
    }

    public class BuildingGradeSettings
    {
        [JsonProperty(PropertyName = "Price")]
        public object price;
        [JsonProperty(PropertyName = "Refund")]
        public object refund;
        [JsonIgnore]
        public float pricePercentage;
        public float refundPercentage;
        [JsonIgnore]
        public Dictionary<string, CurrencyInfo> priceDict;
        public Dictionary<string, CurrencyInfo> refundDict;
    }

    public class EntitySettings
    {
        [JsonProperty(PropertyName = "Remove Allowed")]
        public bool enabled;
        [JsonProperty(PropertyName = "Display Name")]
        public string displayName;
        [JsonProperty(PropertyName = "Price")]
        public object price;
        [JsonProperty(PropertyName = "Refund")]
        public object refund;
        [JsonIgnore]
        public Dictionary<string, CurrencyInfo> priceDict;
        public Dictionary<string, CurrencyInfo> refundDict;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void PreprocessConfigValues();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private void SetConfigValue(object[] pathAndTrailingValue);
    private void PreprocessOldConfig();
    private JObject GetConfigValue(JObject config, string[] path);
    private bool GetConfigValuePre(JObject config, T value, string[] path);
    private void SetConfigValuePre(JObject config, object value, string[] path);
    private bool GetConfigVersionPre(JObject config, VersionNumber version);
    private void Print(BasePlayer player, string message);
    private void Print(ConsoleSystem.Arg arg, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private static class UI
{
    public static CuiElementContainer CreateElementContainer(string parent, string panelName, string backgroundColor, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursor);
    public static void CreatePanel(CuiElementContainer container, string panelName, string backgroundColor, string anchorMin, string anchorMax, bool cursor);
    public static void CreateLabel(CuiElementContainer container, string panelName, string textColor, string text, int fontSize, string anchorMin, string anchorMax, TextAnchor align, float fadeIn);
    public static void CreateImage(CuiElementContainer container, string panelName, string image, string anchorMin, string anchorMax, string color);
    public static void CreateImage(CuiElementContainer container, string panelName, int itemId, ulong skinId, string anchorMin, string anchorMax);
}

private class ToolRemover : FacepunchBehaviour
{
    private const float MinInterval;
    public int CurrentRemoved { get; set; }
    public BaseEntity HitEntity { get; set; }
    public bool CanOverride { get; set; }
    public BasePlayer Player { get; set; }
    public RemoveType RemoveType { get; set; }
    private bool _resetTime;
    private bool _shouldPay;
    private bool _shouldRefund;
    private int _removeTime;
    private int _maxRemovable;
    private float _distance;
    private float _removeInterval;
    private int _timeLeft;
    private float _lastRemove;
    private ItemId _currentItemId;
    private bool _disableInHand;
    private Item _lastHeldItem;
    private BaseEntity _targetEntity;
    private UiEntry _activeUiEntries;
    private void Awake();
    public void Init(RemoveType removeType, int removeTime, int maxRemovable, float distance, float removeInterval, bool shouldPay, bool shouldRefund, bool resetTime, bool canOverride);
    private void RemoveUpdate();
    private BaseEntity GetTargetEntity();
    private void Update();
    private void UnEquip();
    private bool HandleUiEntry(UiEntry uiEntry, bool canShow);
    public void DisableTool(bool showMessage);
    private void OnDestroy();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public readonly GlobalSettings global;
    [JsonProperty(PropertyName = "Container Settings")]
    public readonly ContainerSettings container;
    [JsonProperty(PropertyName = "Remove Damaged Entities")]
    public readonly DamagedEntitySettings damagedEntity;
    [JsonProperty(PropertyName = "Chat Settings")]
    public readonly ChatSettings chat;
    [JsonProperty(PropertyName = "Permission Settings (Just for normal type)")]
    public readonly Dictionary<string, PermissionSettings> permission;
    [JsonProperty(PropertyName = "Remove Type Settings")]
    public readonly Dictionary<RemoveType, RemoveTypeSettings> removeType;
    [JsonProperty(PropertyName = "Remove Mode Settings (Only one model works)")]
    public readonly RemoverModeSettings removerMode;
    [JsonProperty(PropertyName = "NoEscape Settings")]
    public readonly NoEscapeSettings noEscape;
    [JsonProperty(PropertyName = "Image Urls (Used to UI image)")]
    public readonly Dictionary<string, string> imageUrls;
    [JsonProperty(PropertyName = "GUI")]
    public readonly UiSettings ui;
    [JsonProperty(PropertyName = "Remove Info (Refund & Price)")]
    public readonly RemoveSettings remove;
    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version;
}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Enable Debug Mode")]
    public bool debugEnabled;
    [JsonProperty(PropertyName = "Log Debug To File")]
    public bool logToFile;
    [JsonProperty(PropertyName = "Use Teams")]
    public bool useTeams;
    [JsonProperty(PropertyName = "Use Clans")]
    public bool useClans;
    [JsonProperty(PropertyName = "Use Friends")]
    public bool useFriends;
    [JsonProperty(PropertyName = "Use Entity Owners")]
    public bool useEntityOwners;
    [JsonProperty(PropertyName = "Use Building Locks")]
    public bool useBuildingLocks;
    [JsonProperty(PropertyName = "Use Tool Cupboards (Strongly unrecommended)")]
    public bool useToolCupboards;
    [JsonProperty(PropertyName = "Use Building Owners (You will need BuildingOwners plugin)")]
    public bool useBuildingOwners;
    [JsonProperty(PropertyName = "Remove Button")]
    public string removeButton;
    [JsonProperty(PropertyName = "Remove Interval (Min = 0.2)")]
    public float removeInterval;
    [JsonProperty(PropertyName = "Only start cooldown when an entity is removed")]
    public bool startCooldownOnRemoved;
    [JsonProperty(PropertyName = "RemoveType - All/Structure - Remove per frame")]
    public int removePerFrame;
    [JsonProperty(PropertyName = "RemoveType - All/Structure - No item container dropped")]
    public bool noItemContainerDrop;
    [JsonProperty(PropertyName = "RemoveType - Normal - Max Removable Objects - Exclude admins")]
    public bool maxRemovableExclude;
    [JsonProperty(PropertyName = "RemoveType - Normal - Cooldown - Exclude admins")]
    public bool cooldownExclude;
    [JsonProperty(PropertyName = "RemoveType - Normal - Entity Spawned Time Limit - Enabled")]
    public bool entityTimeLimit;
    [JsonProperty(PropertyName = "RemoveType - Normal - Entity Spawned Time Limit - Cannot be removed when entity spawned time more than it")]
    public float limitTime;
    [JsonProperty(PropertyName = "Default Entity Settings (When automatically adding new entities to 'Other Entity Settings')")]
    public DefaultEntitySettings defaultEntity;
    public class DefaultEntitySettings
    {
        [JsonProperty(PropertyName = "Default Remove Allowed")]
        public bool removeAllowed;
    }

}

public class DefaultEntitySettings
{
    [JsonProperty(PropertyName = "Default Remove Allowed")]
    public bool removeAllowed;
}

public class ContainerSettings
{
    [JsonProperty(PropertyName = "Storage Container - Enable remove of not empty storages")]
    public bool removeNotEmptyStorage;
    [JsonProperty(PropertyName = "Storage Container - Drop items from container")]
    public bool dropItemsStorage;
    [JsonProperty(PropertyName = "Storage Container - Drop a item container from container")]
    public bool dropContainerStorage;
    [JsonProperty(PropertyName = "IOEntity Container - Enable remove of not empty storages")]
    public bool removeNotEmptyIoEntity;
    [JsonProperty(PropertyName = "IOEntity Container - Drop items from container")]
    public bool dropItemsIoEntity;
    [JsonProperty(PropertyName = "IOEntity Container - Drop a item container from container")]
    public bool dropContainerIoEntity;
}

public class DamagedEntitySettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled;
    [JsonProperty(PropertyName = "Exclude Quarries")]
    public bool excludeQuarries;
    [JsonProperty(PropertyName = "Exclude Building Blocks")]
    public bool excludeBuildingBlocks;
    [JsonProperty(PropertyName = "Percentage (Can be removed when (health / max health * 100) is not less than it)")]
    public float percentage;
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Chat Command")]
    public string command;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong steamIDIcon;
    [JsonProperty(PropertyName = "Show Message When Enabled/Disabled")]
    public bool showMessageWhenEnabledOrDisabled;
}

public class PermissionSettings
{
    [JsonProperty(PropertyName = "Priority")]
    public int priority;
    [JsonProperty(PropertyName = "Distance")]
    public float distance;
    [JsonProperty(PropertyName = "Cooldown")]
    public float cooldown;
    [JsonProperty(PropertyName = "Max Time")]
    public int maxTime;
    [JsonProperty(PropertyName = "Remove Interval (Min = 0.2)")]
    public float removeInterval;
    [JsonProperty(PropertyName = "Max Removable Objects (0 = Unlimited)")]
    public int maxRemovable;
    [JsonProperty(PropertyName = "Pay")]
    public bool pay;
    [JsonProperty(PropertyName = "Refund")]
    public bool refund;
    [JsonProperty(PropertyName = "Reset the time after removing an entity")]
    public bool resetTime;
}

public class RemoveTypeSettings
{
    [JsonProperty(PropertyName = "Display Name")]
    public string displayName;
    [JsonProperty(PropertyName = "Distance")]
    public float distance;
    [JsonProperty(PropertyName = "Default Time")]
    public int defaultTime;
    [JsonProperty(PropertyName = "Max Time")]
    public int maxTime;
    [JsonProperty(PropertyName = "Gibs")]
    public bool gibs;
    [JsonProperty(PropertyName = "Reset the time after removing an entity")]
    public bool resetTime;
}

public class RemoverModeSettings
{
    [JsonProperty(PropertyName = "No Held Item Mode")]
    public bool noHeldMode;
    [JsonProperty(PropertyName = "No Held Item Mode - Disable remover tool when you have any item in hand")]
    public bool noHeldDisableInHand;
    [JsonProperty(PropertyName = "Melee Tool Hit Mode")]
    public bool meleeHitMode;
    [JsonProperty(PropertyName = "Melee Tool Hit Mode - Item shortname")]
    public string meleeHitItemShortname;
    [JsonProperty(PropertyName = "Melee Tool Hit Mode - Item skin (-1 = All skins)")]
    public long meleeHitModeSkin;
    [JsonProperty(PropertyName = "Melee Tool Hit Mode - Auto enable remover tool when you hold a melee tool")]
    public bool meleeHitEnableInHand;
    [JsonProperty(PropertyName = "Melee Tool Hit Mode - Requires a melee tool in your hand when remover tool is enabled")]
    public bool meleeHitRequires;
    [JsonProperty(PropertyName = "Melee Tool Hit Mode - Disable remover tool when you are not holding a melee tool")]
    public bool meleeHitDisableInHand;
    [JsonProperty(PropertyName = "Specific Tool Mode")]
    public bool specificToolMode;
    [JsonProperty(PropertyName = "Specific Tool Mode - Item shortname")]
    public string specificToolShortName;
    [JsonProperty(PropertyName = "Specific Tool Mode - Item skin (-1 = All skins)")]
    public long specificToolSkin;
    [JsonProperty(PropertyName = "Specific Tool Mode - Auto enable remover tool when you hold a specific tool")]
    public bool specificToolEnableInHand;
    [JsonProperty(PropertyName = "Specific Tool Mode - Requires a specific tool in your hand when remover tool is enabled")]
    public bool specificToolRequires;
    [JsonProperty(PropertyName = "Specific Tool Mode - Disable remover tool when you are not holding a specific tool")]
    public bool specificToolDisableInHand;
}

public class NoEscapeSettings
{
    [JsonProperty(PropertyName = "Use Raid Blocker")]
    public bool useRaidBlocker;
    [JsonProperty(PropertyName = "Use Combat Blocker")]
    public bool useCombatBlocker;
}

public class UiSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled;
    [JsonProperty(PropertyName = "Main Box - Min Anchor (in Rust Window)")]
    public string removerToolAnchorMin;
    [JsonProperty(PropertyName = "Main Box - Max Anchor (in Rust Window)")]
    public string removerToolAnchorMax;
    [JsonProperty(PropertyName = "Main Box - Min Offset (in Rust Window)")]
    public string removerToolOffsetMin;
    [JsonProperty(PropertyName = "Main Box - Max Offset (in Rust Window)")]
    public string removerToolOffsetMax;
    [JsonProperty(PropertyName = "Main Box - Background Color")]
    public string removerToolBackgroundColor;
    [JsonProperty(PropertyName = "Remove Title - Box - Min Anchor (in Main Box)")]
    public string removeAnchorMin;
    [JsonProperty(PropertyName = "Remove Title - Box - Max Anchor (in Main Box)")]
    public string removeAnchorMax;
    [JsonProperty(PropertyName = "Remove Title - Box - Background Color")]
    public string removeBackgroundColor;
    [JsonProperty(PropertyName = "Remove Title - Text - Min Anchor (in Main Box)")]
    public string removeTextAnchorMin;
    [JsonProperty(PropertyName = "Remove Title - Text - Max Anchor (in Main Box)")]
    public string removeTextAnchorMax;
    [JsonProperty(PropertyName = "Remove Title - Text - Text Color")]
    public string removeTextColor;
    [JsonProperty(PropertyName = "Remove Title - Text - Text Size")]
    public int removeTextSize;
    [JsonProperty(PropertyName = "Timeleft - Box - Min Anchor (in Main Box)")]
    public string timeLeftAnchorMin;
    [JsonProperty(PropertyName = "Timeleft - Box - Max Anchor (in Main Box)")]
    public string timeLeftAnchorMax;
    [JsonProperty(PropertyName = "Timeleft - Box - Background Color")]
    public string timeLeftBackgroundColor;
    [JsonProperty(PropertyName = "Timeleft - Text - Min Anchor (in Timeleft Box)")]
    public string timeLeftTextAnchorMin;
    [JsonProperty(PropertyName = "Timeleft - Text - Max Anchor (in Timeleft Box)")]
    public string timeLeftTextAnchorMax;
    [JsonProperty(PropertyName = "Timeleft - Text - Text Color")]
    public string timeLeftTextColor;
    [JsonProperty(PropertyName = "Timeleft - Text - Text Size")]
    public int timeLeftTextSize;
    [JsonProperty(PropertyName = "Entity - Box - Min Anchor (in Main Box)")]
    public string entityAnchorMin;
    [JsonProperty(PropertyName = "Entity - Box - Max Anchor (in Main Box)")]
    public string entityAnchorMax;
    [JsonProperty(PropertyName = "Entity - Box - Background Color")]
    public string entityBackgroundColor;
    [JsonProperty(PropertyName = "Entity - Text - Min Anchor (in Entity Box)")]
    public string entityTextAnchorMin;
    [JsonProperty(PropertyName = "Entity - Text - Max Anchor (in Entity Box)")]
    public string entityTextAnchorMax;
    [JsonProperty(PropertyName = "Entity - Text - Text Color")]
    public string entityTextColor;
    [JsonProperty(PropertyName = "Entity - Text - Text Size")]
    public int entityTextSize;
    [JsonProperty(PropertyName = "Entity - Image - Enabled")]
    public bool entityImageEnabled;
    [JsonProperty(PropertyName = "Entity - Image - Min Anchor (in Entity Box)")]
    public string entityImageAnchorMin;
    [JsonProperty(PropertyName = "Entity - Image - Max Anchor (in Entity Box)")]
    public string entityImageAnchorMax;
    [JsonProperty(PropertyName = "Authorization Check Enabled")]
    public bool authorizationEnabled;
    [JsonProperty(PropertyName = "Authorization Check - Box - Min Anchor (in Main Box)")]
    public string authorizationsAnchorMin;
    [JsonProperty(PropertyName = "Authorization Check - Box - Max Anchor (in Main Box)")]
    public string authorizationsAnchorMax;
    [JsonProperty(PropertyName = "Authorization Check - Box - Allowed Background Color")]
    public string allowedBackgroundColor;
    [JsonProperty(PropertyName = "Authorization Check - Box - Refused Background Color")]
    public string refusedBackgroundColor;
    [JsonProperty(PropertyName = "Authorization Check - Text - Min Anchor (in Authorization Check Box)")]
    public string authorizationsTextAnchorMin;
    [JsonProperty(PropertyName = "Authorization Check - Text - Max Anchor (in Authorization Check Box)")]
    public string authorizationsTextAnchorMax;
    [JsonProperty(PropertyName = "Authorization Check - Text - Text Color")]
    public string authorizationsTextColor;
    [JsonProperty(PropertyName = "Authorization Check Box - Text - Text Size")]
    public int authorizationsTextSize;
    [JsonProperty(PropertyName = "Price & Refund - Image Enabled")]
    public bool imageEnabled;
    [JsonProperty(PropertyName = "Price & Refund - Image Scale")]
    public float imageScale;
    [JsonProperty(PropertyName = "Price & Refund - Distance of image from right border")]
    public float rightDistance;
    [JsonProperty(PropertyName = "Price Enabled")]
    public bool priceEnabled;
    [JsonProperty(PropertyName = "Price - Box - Min Anchor (in Main Box)")]
    public string priceAnchorMin;
    [JsonProperty(PropertyName = "Price - Box - Max Anchor (in Main Box)")]
    public string priceAnchorMax;
    [JsonProperty(PropertyName = "Price - Box - Background Color")]
    public string priceBackgroundColor;
    [JsonProperty(PropertyName = "Price - Text - Min Anchor (in Price Box)")]
    public string priceTextAnchorMin;
    [JsonProperty(PropertyName = "Price - Text - Max Anchor (in Price Box)")]
    public string priceTextAnchorMax;
    [JsonProperty(PropertyName = "Price - Text - Text Color")]
    public string priceTextColor;
    [JsonProperty(PropertyName = "Price - Text - Text Size")]
    public int priceTextSize;
    [JsonProperty(PropertyName = "Price - Text2 - Min Anchor (in Price Box)")]
    public string price2TextAnchorMin;
    [JsonProperty(PropertyName = "Price - Text2 - Max Anchor (in Price Box)")]
    public string price2TextAnchorMax;
    [JsonProperty(PropertyName = "Price - Text2 - Text Color")]
    public string price2TextColor;
    [JsonProperty(PropertyName = "Price - Text2 - Text Size")]
    public int price2TextSize;
    [JsonProperty(PropertyName = "Refund Enabled")]
    public bool refundEnabled;
    [JsonProperty(PropertyName = "Refund - Box - Min Anchor (in Main Box)")]
    public string refundAnchorMin;
    [JsonProperty(PropertyName = "Refund - Box - Max Anchor (in Main Box)")]
    public string refundAnchorMax;
    [JsonProperty(PropertyName = "Refund - Box - Background Color")]
    public string refundBackgroundColor;
    [JsonProperty(PropertyName = "Refund - Text - Min Anchor (in Refund Box)")]
    public string refundTextAnchorMin;
    [JsonProperty(PropertyName = "Refund - Text - Max Anchor (in Refund Box)")]
    public string refundTextAnchorMax;
    [JsonProperty(PropertyName = "Refund - Text - Text Color")]
    public string refundTextColor;
    [JsonProperty(PropertyName = "Refund - Text - Text Size")]
    public int refundTextSize;
    [JsonProperty(PropertyName = "Refund - Text2 - Min Anchor (in Refund Box)")]
    public string refund2TextAnchorMin;
    [JsonProperty(PropertyName = "Refund - Text2 - Max Anchor (in Refund Box)")]
    public string refund2TextAnchorMax;
    [JsonProperty(PropertyName = "Refund - Text2 - Text Color")]
    public string refund2TextColor;
    [JsonProperty(PropertyName = "Refund - Text2 - Text Size")]
    public int refund2TextSize;
    [JsonProperty(PropertyName = "Crosshair - Enabled")]
    public bool showCrosshair;
    [JsonProperty(PropertyName = "Crosshair - Image Url")]
    public string crosshairImageUrl;
    [JsonProperty(PropertyName = "Crosshair - Box - Min Anchor (in Rust Window)")]
    public string crosshairAnchorMin;
    [JsonProperty(PropertyName = "Crosshair - Box - Max Anchor (in Rust Window)")]
    public string crosshairAnchorMax;
    [JsonProperty(PropertyName = "Crosshair - Box - Min Offset (in Rust Window)")]
    public string crosshairOffsetMin;
    [JsonProperty(PropertyName = "Crosshair - Box - Max Offset (in Rust Window)")]
    public string crosshairOffsetMax;
    [JsonProperty(PropertyName = "Crosshair - Box - Image Color")]
    public string crosshairColor;
    [JsonIgnore]
    public Vector2 Price2TextAnchorMin;
    public Vector2 Price2TextAnchorMax;
    public Vector2 Refund2TextAnchorMin;
    public Vector2 Refund2TextAnchorMax;
}

public class RemoveSettings
{
    [JsonProperty(PropertyName = "Price Enabled")]
    public bool priceEnabled;
    [JsonProperty(PropertyName = "Refund Enabled")]
    public bool refundEnabled;
    [JsonProperty(PropertyName = "Refund Items In Entity Slot")]
    public bool refundSlot;
    [JsonProperty(PropertyName = "Allowed Building Grade")]
    public Dictionary<BuildingGrade.Enum, bool> validConstruction;
    [JsonProperty(PropertyName = "Display Names (Refund & Price)")]
    public readonly Dictionary<string, string> displayNames;
    [JsonProperty(PropertyName = "Building Blocks Settings")]
    public Dictionary<string, BuildingBlocksSettings> buildingBlock;
    [JsonProperty(PropertyName = "Other Entity Settings")]
    public Dictionary<string, EntitySettings> entity;
}

public class BuildingBlocksSettings
{
    [JsonProperty(PropertyName = "Display Name")]
    public string displayName;
    [JsonProperty(PropertyName = "Building Grade")]
    public Dictionary<BuildingGrade.Enum, BuildingGradeSettings> buildingGrade;
}

public class BuildingGradeSettings
{
    [JsonProperty(PropertyName = "Price")]
    public object price;
    [JsonProperty(PropertyName = "Refund")]
    public object refund;
    [JsonIgnore]
    public float pricePercentage;
    public float refundPercentage;
    [JsonIgnore]
    public Dictionary<string, CurrencyInfo> priceDict;
    public Dictionary<string, CurrencyInfo> refundDict;
}

public class EntitySettings
{
    [JsonProperty(PropertyName = "Remove Allowed")]
    public bool enabled;
    [JsonProperty(PropertyName = "Display Name")]
    public string displayName;
    [JsonProperty(PropertyName = "Price")]
    public object price;
    [JsonProperty(PropertyName = "Refund")]
    public object refund;
    [JsonIgnore]
    public Dictionary<string, CurrencyInfo> priceDict;
    public Dictionary<string, CurrencyInfo> refundDict;
}


```

---

## RemoveVanilla by Mevent - Remove without any useless things

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Remove Vanilla", "Orange/Ryz0r", "1.3.0")]
[Description("Remove pretty much any entity within a specified amount of time, without commands.")]
public class RemoveVanilla : RustPlugin
{
    private const string PermUse;
    private Dictionary<uint, double> entities;
    private void Init();
    private void OnEntitySpawned(BaseCombatEntity entity);
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void CheckInput(BasePlayer player, InputState input);
    private bool GiveRefund(BaseCombatEntity entity, BasePlayer player);
    private bool CanPickup(BaseEntity entity, BasePlayer player);
    private BaseCombatEntity GetLookEntity(BasePlayer player);
    private bool ActiveItemIsHammer(BasePlayer player);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. Timer while pickup will be available (0 to disable)")]
        public int pickupTime;
        [JsonProperty(PropertyName = "2. Blocked entities to remove (shortname of entity, not item):")]
        public List<string> blocked;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private double Now();
    private int Passed(double a);
    private string FixNames(string name);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. Timer while pickup will be available (0 to disable)")]
    public int pickupTime;
    [JsonProperty(PropertyName = "2. Blocked entities to remove (shortname of entity, not item):")]
    public List<string> blocked;
}


```

---

## Rename by MrBlue - Allows players with permission to instantly rename other players or themselves

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Rename", "Wulf", "1.1.2")]
[Description("Allows players with permission to instantly rename other players or themselves")]
internal class Rename : CovalencePlugin
{
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty("Notify player of rename")]
        public bool NotifyPlayer;
        [JsonProperty("Persistence for renames")]
        public bool Persistence;
        [JsonProperty("Prevent admin renames")]
        public bool PreventAdmin;
        private string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class StoredData
    {
        public readonly HashSet<RenameInfo> Renames;
    }

    private class RenameInfo : IEquatable<RenameInfo>
    {
        public string Id;
        public string Old;
        public string New;
        public RenameInfo();
        public RenameInfo(string playerId, string oldName, string newName);
        public override int GetHashCode();
        public bool Equals(RenameInfo other);
    }

    private void SaveData();
    private void OnServerSave();
    private void Unload();
    protected override void LoadDefaultMessages();
    private const string PermissionOthers;
    private const string PermissionSelf;
    private StoredData _storedData;
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void CommandRename(IPlayer player, string command, string[] args);
    private void CommandResetName(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

public class Configuration
{
    [JsonProperty("Notify player of rename")]
    public bool NotifyPlayer;
    [JsonProperty("Persistence for renames")]
    public bool Persistence;
    [JsonProperty("Prevent admin renames")]
    public bool PreventAdmin;
    private string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class StoredData
{
    public readonly HashSet<RenameInfo> Renames;
}

private class RenameInfo : IEquatable<RenameInfo>
{
    public string Id;
    public string Old;
    public string New;
    public RenameInfo();
    public RenameInfo(string playerId, string oldName, string newName);
    public override int GetHashCode();
    public bool Equals(RenameInfo other);
}


```

---

## RepairbenchBlock by KrunghCrow - Blocks usage of repairbenches

```csharp
using Oxide.Core;

Oxide.Plugins
[Info("Repairbench Block" , "Krungh Crow", "1.0.2")]
[Description("Disables using the repairbench")]
 class RepairbenchBlock : RustPlugin
{
     void Init();
    private object CanLootEntity(BasePlayer player, RepairBench repairbench);
}


```

---

## RepairBlocker by Camoec - Prevents certain objects from being repaired

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Repair Blocker", "Camoec", 1.1)]
[Description("Prevents certain objects from being repaired")]
public class RepairBlocker : RustPlugin
{
    private const string BypassPerm;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "ChatPrefix")]
        public string ChatPrefix;
        [JsonProperty(PropertyName = "BlackList")]
        public List<string> BlackList;
    }

    private PluginConfig _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string userid);
    private void Init();
     object OnItemRepair(BasePlayer player, Item item);
     object OnHammerHit(BasePlayer player, HitInfo info);
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "ChatPrefix")]
    public string ChatPrefix;
    [JsonProperty(PropertyName = "BlackList")]
    public List<string> BlackList;
}


```

---

## RepairCost by Rustoholics - Alter the repair cost of items

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("Repair Cost", "Rustoholics", "0.1.1")]
[Description("Alter the repair cost of items")]
public class RepairCost : CovalencePlugin
{
    [PluginReference]
    private Plugin Economics;
    private Dictionary<string,RepairBench> _repairBenches;
    private Dictionary<string,string> _repairGui;
    private Dictionary<string,ulong> _repairGuiItem;
    private Dictionary<string, Timer> _repairTimers;
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        public Dictionary<string, Dictionary<string, object>> RepairCosts;
    }

    protected override void LoadConfig();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
     void CanLootEntity(BasePlayer player, RepairBench container);
     void OnLootEntityEnd(BasePlayer player, RepairBench container);
     void OnLootEntity(BasePlayer player, RepairBench container);
     object OnItemRepair(BasePlayer player, Item itemToRepair);
     void Unload();
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     void CheckAndPresentGui(BasePlayer player, RepairBench container);
     bool UseCustomRepairGui(Item item, BasePlayer player);
     void WriteGui(BasePlayer player, Item itemToRepair);
     void CloseGui(BasePlayer player);
    private List<ItemAmount> RepairList(BasePlayer player, Item itemToRepair);
    private List<ItemAmount> ApplyItemListMultiplier(List<ItemAmount> list, Item itemToRepair);
     bool IsCustomRepairItem(Item item);
     bool PlayerHasEnough(BasePlayer player, List<ItemAmount> list, double economics);
     double EconomicsRequired(Item item);
}

private class Configuration
{
    public Dictionary<string, Dictionary<string, object>> RepairCosts;
}


```

---

## RepairDelayModifier by Ryz0r - Allows a player with permission to repair immediately, or within a time

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Repair Delay Modifier", "Ryz0r", "1.0.1")]
[Description("Takes away the repair delay, or increases/decreases it for players with permission.")]
public class RepairDelayModifier : RustPlugin
{
    private Configuration _config;
    private const string NoDelayPerm;
    private const string TimeDelayPerm;
    private class Configuration
    {
        [JsonProperty(PropertyName = "RepairTime")]
        public float RepairTime;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private object OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
}

private class Configuration
{
    [JsonProperty(PropertyName = "RepairTime")]
    public float RepairTime;
}


```

---

## RepairTool by k1lly0u - Repairs any entity that has health

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Reflection;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("RepairTool", "k1lly0u", "0.1.3", ResourceId = 1883)]
[Description("Repairs any entity that has health")]
 class RepairTool : RustPlugin
{
    private static FieldInfo serverinput;
    private string panelName;
    private List<Repairer> activeRepairers;
     void OnServerInitialized();
     void Unload();
     void OnPlayerInput(BasePlayer player, InputState input);
    private void CreateUI(BasePlayer player, int time, string name);
    private void DestroyUI(BasePlayer player);
    private void InitPlugin();
    private object RepairEntity(BaseEntity entity);
    private BaseEntity FindEntity(BasePlayer player);
    private object Ray(BasePlayer player, Vector3 Aim);
    private BaseEntity FindBuildingBlock(Ray ray, float distance);
    private void MSG(BasePlayer player, string msg, bool title);
    private string GetMSG(string key);
    private void DestroyAllPlayers();
    [ChatCommand("rt")]
    private void cmdRA(BasePlayer player, string command, string[] args);
    private bool HasPerm(BasePlayer player);
     class Repairer : MonoBehaviour
    {
        public BasePlayer player;
        public int TimeRemaining;
        public InputState inputState;
         RepairTool ra;
        public void InitPlayer(BasePlayer p, int time, RepairTool repairtool);
        public void FindTarget();
        private void RefreshGUI();
         void DestroyGUI();
        public void DestroyComponent();
    }

}

 class Repairer : MonoBehaviour
{
    public BasePlayer player;
    public int TimeRemaining;
    public InputState inputState;
     RepairTool ra;
    public void InitPlayer(BasePlayer p, int time, RepairTool repairtool);
    public void FindTarget();
    private void RefreshGUI();
     void DestroyGUI();
    public void DestroyComponent();
}


```

---

## ReplaceOnBroken by MrBlue - Replaces the active broken item with a not broken item if in inventory

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("ReplaceOnBroken", "Wulf/lukespragg", "2.1.1", ResourceId = 1173)]
[Description("Replaces the active broken item with a not broken item if in inventory")]
 class ReplaceOnBroken : RustPlugin
{
    const string permAllow;
     List<object> exclusions;
     bool sameCategory;
    protected override void LoadDefaultConfig();
     void Init();
     void OnLoseCondition(Item oldItem, float amount);
     T GetConfig(string name, T value);
}


```

---

## Replenish by 2CHEVSKII - Save and restore items in selected containers

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using UnityEngine.Assertions;

Oxide.Plugins
[Info("Replenish", "2CHEVSKII", "2.0.0")]
[Description("Save and restore items in selected containers")]
 class Replenish : CovalencePlugin
{
     PluginSettings settings;
     ReplenishController controller;
    const string PERMISSION_USE;
    const string DATAFILE_NAME;
    const string M_PREFIX;
    const string M_NO_PERMISSION;
    const string M_HELP;
    const string M_CONTAINER_SET;
    const string M_CONTAINER_UNSET;
    const string M_CONTAINER_INFO;
    const string M_CONTAINER_NOT_EMPTY;
    const string M_TIMED;
    const string M_WIPE_ONLY;
    const string M_COMMAND_ONLY;
    const string M_CONTAINER_NOT_FOUND;
    const string M_CONTAINER_NOT_SAVED;
    const string M_NO_CONTAINERS_SAVED;
    const string M_CONTAINER_RESTORED;
    const string M_ALL_RESTORED;
    const string M_ALL_REMOVED;
    static Replenish Instance;
     bool isNewWipe;
     void CommandHandler(IPlayer player, string _, string[] args);
     StorageContainer GetContainerInSight(IPlayer player);
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnNewSave();
    protected override void LoadDefaultMessages();
     string BuildContainerInfo(IPlayer player, ContainerData data);
     string GetSaveTimeMessage(ContainerData.RestoreMode mode);
     void MessageRaw(IPlayer player, string message);
     void Message(IPlayer player, string langKey, object[] args);
     string GetMessage(IPlayer player, string langKey);
     List<ContainerData> LoadData();
     void SaveData(List<ContainerData> list);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class ReplenishController : MonoBehaviour
    {
         List<ContainerData> allContainers;
         ContainerData nextRestoreContainer;
        public ContainerData FindDataById(int id);
        public ContainerData FindDataByEntity(BaseNetworkable entity);
        public int GetDataIndex(ContainerData data);
        public void Init(List<ContainerData> data);
        public List<ContainerData> GetSaveData();
        public ContainerData SaveContainer(StorageContainer container, float timer);
        public bool RemoveContainer(StorageContainer container);
        public bool RemoveContainer(int containerId);
        public bool RestoreNow(ContainerData data);
         bool RemoveSavedContainer(ContainerData data);
         void Start();
         StorageContainer RespawnContainer(ContainerData data);
         void RestoreWipeContainers();
         void RestoreTick();
         bool CheckEmpty(StorageContainer container, ContainerData data);
         void BeforeRestore(StorageContainer container);
         void RestoreContainer(StorageContainer container, ContainerData data);
         void AfterRestore(ContainerData data);
         void UpdateQueue();
    }

     class ContainerData
    {
        public uint NetId;
        public Vector3 Position;
        public List<SerializableItem> ItemList;
        public string PrefabName;
        public float RestoreTime;
        [JsonIgnore]
        public float NextRestoreTime;
        [JsonIgnore]
        public RestoreMode Mode { get; set; }
        public ContainerData();
        public ContainerData(StorageContainer container, float timer);
        public float TimeLeftBeforeRestore();
        public void OnRestored();
    }

     class PluginSettings
    {
        public static PluginSettings Default { get; set; }
        [JsonProperty("Clean container inventory upon replenish")]
        public bool ClearContainerInventory { get; set; }
        [JsonProperty("Default timer (0 for command-only, -1 for new wipe replenish)")]
        public float DefaultReplenishTimer { get; set; }
        [JsonProperty("Respawn container if it is destroyed")]
        public bool RespawnContainer { get; set; }
        [JsonProperty("Requires container to be empty (destroyed)")]
        public bool RequiresEmpty { get; set; }
    }

}

 class ReplenishController : MonoBehaviour
{
     List<ContainerData> allContainers;
     ContainerData nextRestoreContainer;
    public ContainerData FindDataById(int id);
    public ContainerData FindDataByEntity(BaseNetworkable entity);
    public int GetDataIndex(ContainerData data);
    public void Init(List<ContainerData> data);
    public List<ContainerData> GetSaveData();
    public ContainerData SaveContainer(StorageContainer container, float timer);
    public bool RemoveContainer(StorageContainer container);
    public bool RemoveContainer(int containerId);
    public bool RestoreNow(ContainerData data);
     bool RemoveSavedContainer(ContainerData data);
     void Start();
     StorageContainer RespawnContainer(ContainerData data);
     void RestoreWipeContainers();
     void RestoreTick();
     bool CheckEmpty(StorageContainer container, ContainerData data);
     void BeforeRestore(StorageContainer container);
     void RestoreContainer(StorageContainer container, ContainerData data);
     void AfterRestore(ContainerData data);
     void UpdateQueue();
}

 class ContainerData
{
    public uint NetId;
    public Vector3 Position;
    public List<SerializableItem> ItemList;
    public string PrefabName;
    public float RestoreTime;
    [JsonIgnore]
    public float NextRestoreTime;
    [JsonIgnore]
    public RestoreMode Mode { get; set; }
    public ContainerData();
    public ContainerData(StorageContainer container, float timer);
    public float TimeLeftBeforeRestore();
    public void OnRestored();
}

 class PluginSettings
{
    public static PluginSettings Default { get; set; }
    [JsonProperty("Clean container inventory upon replenish")]
    public bool ClearContainerInventory { get; set; }
    [JsonProperty("Default timer (0 for command-only, -1 for new wipe replenish)")]
    public float DefaultReplenishTimer { get; set; }
    [JsonProperty("Respawn container if it is destroyed")]
    public bool RespawnContainer { get; set; }
    [JsonProperty("Requires container to be empty (destroyed)")]
    public bool RequiresEmpty { get; set; }
}


```

---

## ResearchBlock by  - Allows to block researching several items

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Research Block", "Orange", "1.0.1")]
[Description("Allows to block researching several items")]
public class ResearchBlock : RustPlugin
{
    private List<ItemDefinition> _disabledResearchable;
    private void OnServerInitialized();
    private void Unload();
    private object CanResearchItem(BasePlayer player, Item item);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Blocked shortnames")]
        public List<string> shortnames;
        [JsonProperty(PropertyName = "Additionally block Experimenting")]
        public bool blockExperimenting;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Blocked shortnames")]
    public List<string> shortnames;
    [JsonProperty(PropertyName = "Additionally block Experimenting")]
    public bool blockExperimenting;
}


```

---

## ResearchControl by  - Allows for managing all what can be done with the research table

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("ResearchControl", "Vlad-00003", "1.0.4")]
[Description("Allow you to adjust price for a research")]
 class ResearchControl : RustPlugin
{
    private PluginConfig config;
    private class CustomPermission
    {
        [JsonProperty("Research cost modifier")]
        public int PriceModifier;
        [JsonProperty("Research speed modifier")]
        public decimal ResearchSpeed;
        [JsonProperty("The speed is absolute value, not modifier")]
        public bool SpeedIsAbsolute;
        public CustomPermission(int PriceModifier, decimal ResearchSpeed, bool ModifierIsSpeed);
    }

    private class ItemConfig
    {
        [JsonProperty("Research cost")]
        public int Cost;
        [JsonProperty("Research speed")]
        public decimal Speed;
        public ItemConfig(int Cost, decimal Speed);
    }

    private class PluginConfig
    {
        [JsonProperty("Custom permission multipliers")]
        public Dictionary<string, CustomPermission> Permissions;
        [JsonProperty("Default price list")]
        public Dictionary<string, Dictionary<string, ItemConfig>> _Prices;
        [JsonIgnore]
        public Dictionary<ItemDefinition, ItemConfig> Prices;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void OnItemResearch(ResearchTable table, Item item, BasePlayer player);
     int OnItemScrap(ResearchTable table, Item item);
     decimal GetPlayerRate(BasePlayer player);
    private class PlayerSpeed
    {
        public decimal Speed;
        public bool IsModifier;
        public PlayerSpeed(decimal Speed, bool IsModifier);
    }

     PlayerSpeed GetPlayerSpeed(BasePlayer player);
    public int GetDefaultPrice(ItemDefinition def);
}

private class CustomPermission
{
    [JsonProperty("Research cost modifier")]
    public int PriceModifier;
    [JsonProperty("Research speed modifier")]
    public decimal ResearchSpeed;
    [JsonProperty("The speed is absolute value, not modifier")]
    public bool SpeedIsAbsolute;
    public CustomPermission(int PriceModifier, decimal ResearchSpeed, bool ModifierIsSpeed);
}

private class ItemConfig
{
    [JsonProperty("Research cost")]
    public int Cost;
    [JsonProperty("Research speed")]
    public decimal Speed;
    public ItemConfig(int Cost, decimal Speed);
}

private class PluginConfig
{
    [JsonProperty("Custom permission multipliers")]
    public Dictionary<string, CustomPermission> Permissions;
    [JsonProperty("Default price list")]
    public Dictionary<string, Dictionary<string, ItemConfig>> _Prices;
    [JsonIgnore]
    public Dictionary<ItemDefinition, ItemConfig> Prices;
}

private class PlayerSpeed
{
    public decimal Speed;
    public bool IsModifier;
    public PlayerSpeed(decimal Speed, bool IsModifier);
}


```

---

## ResearchUnlocked by MJSU - Displays a UI for already unlocked items placed in a research table

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Research Unlocked", "MJSU", "1.0.2")]
[Description("Displays a ui if you have already unlocked the item placed in a research table")]
public class ResearchUnlocked : RustPlugin
{
    private PluginConfig _pluginConfig;
    private const string UsePermission;
    private readonly Hash<ResearchTable, List<BasePlayer>> _lootingPlayers;
    private string _notLearnedColor;
    private string _learnedColor;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void Unload();
    private void OnLootEntity(BasePlayer player, ResearchTable table);
    private void OnLootEntityEnd(BasePlayer player, ResearchTable table);
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private bool HasPermission(BasePlayer player, string perm);
    private string Lang(string key, BasePlayer player, object[] args);
    private class PluginConfig
    {
        [DefaultValue("##00CC00FF")]
        [JsonProperty(PropertyName = "Learned Color")]
        public string LearnedColor { get; set; }
        [DefaultValue("#CC0000FF")]
        [JsonProperty(PropertyName = "Not Learned Color")]
        public string NotLearnedColor { get; set; }
        [JsonProperty(PropertyName = "Ui Position")]
        public UiPosition Pos { get; set; }
    }

    private class LangKeys
    {
        public const string AlreadyLearned;
        public const string NotLearned;
    }

    private const string UiPanelName;
    private static class Ui
    {
        private static string UiPanel { get; set; }
        public static CuiElementContainer Container(string color, UiPosition pos, bool useCursor, string panel, string parent);
        public static void Panel(CuiElementContainer container, string color, UiPosition pos, bool cursor);
        public static void Label(CuiElementContainer container, string text, int size, UiPosition pos, TextAnchor align);
        public static string Color(string hexColor);
    }

    private class UiPosition
    {
        public float XMin { get; set; }
        public float YMin { get; set; }
        public float XMax { get; set; }
        public float YMax { get; set; }
        public UiPosition(float xMin, float yMin, float xMax, float yMax);
        public string GetMin();
        public string GetMax();
        public override string ToString();
    }

    private void DestroyAllUi(BasePlayer player);
    private readonly string _clear;
    private readonly UiPosition _fullArea;
    private void CreateUnlockedUi(BasePlayer player, bool unlocked);
}

private class PluginConfig
{
    [DefaultValue("##00CC00FF")]
    [JsonProperty(PropertyName = "Learned Color")]
    public string LearnedColor { get; set; }
    [DefaultValue("#CC0000FF")]
    [JsonProperty(PropertyName = "Not Learned Color")]
    public string NotLearnedColor { get; set; }
    [JsonProperty(PropertyName = "Ui Position")]
    public UiPosition Pos { get; set; }
}

private class LangKeys
{
    public const string AlreadyLearned;
    public const string NotLearned;
}

private static class Ui
{
    private static string UiPanel { get; set; }
    public static CuiElementContainer Container(string color, UiPosition pos, bool useCursor, string panel, string parent);
    public static void Panel(CuiElementContainer container, string color, UiPosition pos, bool cursor);
    public static void Label(CuiElementContainer container, string text, int size, UiPosition pos, TextAnchor align);
    public static string Color(string hexColor);
}

private class UiPosition
{
    public float XMin { get; set; }
    public float YMin { get; set; }
    public float XMax { get; set; }
    public float YMax { get; set; }
    public UiPosition(float xMin, float yMin, float xMax, float yMax);
    public string GetMin();
    public string GetMax();
    public override string ToString();
}


```

---

## Reserved by MrBlue - Allows players with permission to always be able to connect

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;

Oxide.Plugins
[Info("Reserved", "Wulf/lukespragg", "2.0.3")]
[Description("Allows players with permission to always be able to connect")]
public class Reserved : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Always allow admin to connect (true/false)")]
        public bool AlwaysAllowAdmin { get; set; }
        [JsonProperty(PropertyName = "Always use reserved slot if player has permission (true/false)")]
        public bool AlwaysUseSlot { get; set; }
        [JsonProperty(PropertyName = "Dynamic slots based on players with permission (true/false)")]
        public bool DynamicSlots { get; set; }
        [JsonProperty(PropertyName = "Kick other players for players with permission (true/false)")]
        public bool KickForReserved { get; set; }
        [JsonProperty(PropertyName = "Number of slots to reserve (if dynamic slots not enabled)")]
        public int ReservedSlots { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private const string permSlot;
    private int slotsAvailable;
    private void OnServerInitialized();
    private void OnUserPermissionGranted(string id, string perm);
    private void OnUserPermissionRevoked(string id, string perm);
    private void OnServerSave();
    private object CanUserLogin(string name, string id, string ip);
    private void OnUserDisconnected(IPlayer player, string reason);
    private string Lang(string key, string id, object[] args);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Always allow admin to connect (true/false)")]
    public bool AlwaysAllowAdmin { get; set; }
    [JsonProperty(PropertyName = "Always use reserved slot if player has permission (true/false)")]
    public bool AlwaysUseSlot { get; set; }
    [JsonProperty(PropertyName = "Dynamic slots based on players with permission (true/false)")]
    public bool DynamicSlots { get; set; }
    [JsonProperty(PropertyName = "Kick other players for players with permission (true/false)")]
    public bool KickForReserved { get; set; }
    [JsonProperty(PropertyName = "Number of slots to reserve (if dynamic slots not enabled)")]
    public int ReservedSlots { get; set; }
}


```

---

## ResetCodeLocks by  - Allows players to reset the code (guest and main) on all their locks

```csharp
using System.Linq;
using System.Reflection;
using System.Collections.Generic;

Oxide.Plugins
[Info("ResetCodeLocks", "Absolut", "1.0.2", ResourceId = 2348)]
 class ResetCodeLocks : RustPlugin
{
    private FieldInfo CurrentCode;
    private FieldInfo CurrentGuestCode;
    private FieldInfo CodeLockWhiteList;
    private FieldInfo hasCode;
    private FieldInfo hasGuestCode;
     string TitleColor;
     string MsgColor;
     void Loaded();
     Dictionary<string, string> messages;
    [ChatCommand("code")]
    private void chatcod(BasePlayer player, string cmd, string[] args);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3);
    private void PlayerSetLocks(BasePlayer player, string code, bool guest);
}


```

---

## ResetHostileOnDeath by WhiteThunder - Resets player hostile status on death

```csharp

Oxide.Plugins
[Info("Reset Hostile On Death", "WhiteThunder", "1.0.0")]
[Description("Resets player hostile status on death.")]
public class ResetHostileOnDeath : CovalencePlugin
{
    private void OnPlayerDeath(BasePlayer player);
}


```

---

## ResourceVIPSpawns by DNARust - Allows players to set a spawn point for resources that respawn every X seconds

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using UnityEngine;
using System;

Oxide.Plugins
[Info("Resource VIP Spawns", "Daano123", "1.0.0")]
[Description("Resource VIP Spawns is a plugin that allows players with the correct permissions to set a spawn point for resources that respawn every X seconds")]
 class ResourceVIPSpawns : CovalencePlugin
{
     Dictionary<string, Tuple<Vector3, Vector3, Vector3, Timer, Timer, Timer>> SpawnPointsSet;
    const string Admin_Perm;
    const string Spawnable_Perm;
    readonly string SulfurAssetLocation;
    readonly string MetalAssetLocation;
    readonly string StoneAssetLocation;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    [Command("rs")]
    private void Spawn(IPlayer player, string command, string[] args);
     void SpawnEntity(IPlayer player, string entity_name);
    private Timer TimerHandler(IPlayer player, float timerTime, string entity_name, Vector3 hit);
}


```

---

## RespawnBalance by  - Balances respawning so players value their life

```csharp
using System.Collections.Generic;
using UnityEngine;
using System.Reflection;
using System;
using System.IO;
using Newtonsoft.Json;
using System.Text;
using System.IO.Compression;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("RespawnBalance", "Jake_Rich", "1.0.1")]
[Description("Reset bed cooldown on death and configure metabolism when spawning.")]
public partial class RespawnBalance : RustPlugin
{
    public static RespawnBalance _plugin;
    public static FieldInfo BagField;
    public JSONFile<ConfigData> _settingsFile;
    public ConfigData Settings { get; set; }
    public RespawnTypeEnum RespawnType;
     void Init();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnServerCommand(ConsoleSystem.Arg args);
     void OnPlayerRespawned(BasePlayer player);
    public class ConfigData
    {
        public int MaxCooldown;
        public float CooldownPerDistance;
        public RespawnSettings DefaultSpawnSettings;
        public RespawnSettings SleepingBagSettings;
        public RespawnSettings BedSettings;
    }

    public class RespawnSettings
    {
        public float Health;
        public float Hunger;
        public float Water;
        public void Apply(BasePlayer player);
        public RespawnSettings();
        public RespawnSettings(float hp, float hunger, float water);
    }

    public void SetCooldowns(BasePlayer player, Vector3 deathPosition);
    public static void SetBagCooldown(SleepingBag bag, float cooldown);
    public class BasePlayerData
    {
        [JsonIgnore]
        public BasePlayer Player { get; set; }
        public string userID { get; set; }
        public BasePlayerData();
        public BasePlayerData(BasePlayer player);
    }

    public class PlayerDataController
    {
        [JsonPropertyAttribute(Required = Required.Always)]
        private Dictionary<string, T> playerData { get; set; }
        private JSONFile<Dictionary<string, T>> _file;
        private Timer _timer;
        public IEnumerable<T> All { get; set; }
        public PlayerDataController();
        public PlayerDataController(string filename);
        public void Unload();
        public T Get(string identifer);
        public T Get(ulong userID);
        public T Get(BasePlayer player);
        public bool Has(ulong userID);
        public void Set(string userID, T data);
        public bool Remove(string userID);
        public void Update(T data);
    }

    public class JSONFile
    {
        private DynamicConfigFile _file;
        public string _name { get; set; }
        public Type Instance { get; set; }
        private ConfigLocation _location { get; set; }
        private string _path { get; set; }
        public bool SaveOnUnload;
        public bool Compressed;
        public JSONFile(string name, ConfigLocation location, string path, string extension, bool saveOnUnload);
        public virtual void Init();
        public virtual void Load();
        private void LoadCompressed();
        public virtual void Save();
        private void SaveCompressed();
        public virtual void Reload();
        private void Unload(Plugin sender, PluginManager manager);
    }

}

public class ConfigData
{
    public int MaxCooldown;
    public float CooldownPerDistance;
    public RespawnSettings DefaultSpawnSettings;
    public RespawnSettings SleepingBagSettings;
    public RespawnSettings BedSettings;
}

public class RespawnSettings
{
    public float Health;
    public float Hunger;
    public float Water;
    public void Apply(BasePlayer player);
    public RespawnSettings();
    public RespawnSettings(float hp, float hunger, float water);
}

public class BasePlayerData
{
    [JsonIgnore]
    public BasePlayer Player { get; set; }
    public string userID { get; set; }
    public BasePlayerData();
    public BasePlayerData(BasePlayer player);
}

public class PlayerDataController
{
    [JsonPropertyAttribute(Required = Required.Always)]
    private Dictionary<string, T> playerData { get; set; }
    private JSONFile<Dictionary<string, T>> _file;
    private Timer _timer;
    public IEnumerable<T> All { get; set; }
    public PlayerDataController();
    public PlayerDataController(string filename);
    public void Unload();
    public T Get(string identifer);
    public T Get(ulong userID);
    public T Get(BasePlayer player);
    public bool Has(ulong userID);
    public void Set(string userID, T data);
    public bool Remove(string userID);
    public void Update(T data);
}

public class JSONFile
{
    private DynamicConfigFile _file;
    public string _name { get; set; }
    public Type Instance { get; set; }
    private ConfigLocation _location { get; set; }
    private string _path { get; set; }
    public bool SaveOnUnload;
    public bool Compressed;
    public JSONFile(string name, ConfigLocation location, string path, string extension, bool saveOnUnload);
    public virtual void Init();
    public virtual void Load();
    private void LoadCompressed();
    public virtual void Save();
    private void SaveCompressed();
    public virtual void Reload();
    private void Unload(Plugin sender, PluginManager manager);
}


```

---

## RespawnBradley by PinguinNordpol - Adds the possibility to respawn Bradley via command

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using Rust;
using ConVar;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Respawn Bradley", "PinguinNordpol", "0.3.2")]
[Description("Adds the possibility to respawn Bradley via command")]
 class RespawnBradley : CovalencePlugin
{
    [PluginReference]
    private Plugin ServerRewards;
    private Plugin Economics;
    private Plugin LootDefender;
    private ConfigData config_data;
    private CooldownController cooldown_controller;
     void Init();
     void Loaded();
     void OnServerInitialized();
    private class CooldownController
    {
        private DynamicConfigFile cooldown_datafile;
        private Dictionary<string, DateTime> cooldown_data;
        private RespawnBradley parent;
        private double cooldown_secs;
        private bool cooldown_per_player;
        public CooldownController(RespawnBradley _parent, uint _cooldown_secs, bool _cooldown_per_player);
        public void AddCooldown(string player_id);
        public bool HasCooldown(string player_id);
        public bool HasCooldown(string player_id, uint remaining_secs);
        private bool HasCooldownHelper(string player_id, DateTime now);
    }

     bool IsBradleyAlive();
     bool DoRespawn(IPlayer player);
     bool ChargePlayer(IPlayer player, bool called_by_player);
     void RefundPlayer(IPlayer player, bool called_by_player);
    private void MessagePlayer(IPlayer player, string msg);
    private void MessagePlayer(BasePlayer player, string msg);
    private IPlayer FindPlayer(string player_id);
    private string ColorizeText(string msg);
    private string FormatSecs(IPlayer player, uint secs);
    [Command("respawnbradley")]
    private void cmdRespawnBradley(IPlayer player, string command, string[] args);
     class Messaging
    {
        public string MsgColor { get; set; }
        public string HilColor { get; set; }
        public string ErrColor { get; set; }
    }

     class Options
    {
        public bool LockBradleyOnRespawn { get; set; }
        public bool UseServerRewards { get; set; }
        public bool UseEconomics { get; set; }
        public bool ChargeOnServerCommand { get; set; }
        public bool ChargeOnPlayerCommand { get; set; }
        public bool RefundOnServerCommand { get; set; }
        public bool RefundOnPlayerCommand { get; set; }
        public int RespawnCosts { get; set; }
        public string CurrencySymbol { get; set; }
    }

     class Cooldowns
    {
        public bool EnableCooldown { get; set; }
        public bool CooldownPerPlayer { get; set; }
        public uint CooldownSecs { get; set; }
    }

     class PluginVersion
    {
        public string CurrentVersion { get; set; }
    }

     class ConfigData
    {
        public Messaging Messaging { get; set; }
        public Options Options { get; set; }
        public Cooldowns Cooldowns { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    private void LoadConfig();
    private ConfigData CreateNewConfig();
    protected override void LoadDefaultConfig();
    private ConfigData UpdateConfig(ConfigData old_config);
     void SaveConfig(ConfigData config);
    private string GetMSG(string key, string userid);
     Dictionary<string, string> Messages;
}

private class CooldownController
{
    private DynamicConfigFile cooldown_datafile;
    private Dictionary<string, DateTime> cooldown_data;
    private RespawnBradley parent;
    private double cooldown_secs;
    private bool cooldown_per_player;
    public CooldownController(RespawnBradley _parent, uint _cooldown_secs, bool _cooldown_per_player);
    public void AddCooldown(string player_id);
    public bool HasCooldown(string player_id);
    public bool HasCooldown(string player_id, uint remaining_secs);
    private bool HasCooldownHelper(string player_id, DateTime now);
}

 class Messaging
{
    public string MsgColor { get; set; }
    public string HilColor { get; set; }
    public string ErrColor { get; set; }
}

 class Options
{
    public bool LockBradleyOnRespawn { get; set; }
    public bool UseServerRewards { get; set; }
    public bool UseEconomics { get; set; }
    public bool ChargeOnServerCommand { get; set; }
    public bool ChargeOnPlayerCommand { get; set; }
    public bool RefundOnServerCommand { get; set; }
    public bool RefundOnPlayerCommand { get; set; }
    public int RespawnCosts { get; set; }
    public string CurrencySymbol { get; set; }
}

 class Cooldowns
{
    public bool EnableCooldown { get; set; }
    public bool CooldownPerPlayer { get; set; }
    public uint CooldownSecs { get; set; }
}

 class PluginVersion
{
    public string CurrentVersion { get; set; }
}

 class ConfigData
{
    public Messaging Messaging { get; set; }
    public Options Options { get; set; }
    public Cooldowns Cooldowns { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## Respawner by  - Automatically respawns players with permission and optionally wakes them up

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Respawner", "Wulf/lukespragg", "1.0.3")]
[Description("Automatically respawns players with permission and optionally wakes them up")]
public class Respawner : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Automatically wake up (true/false)")]
        public bool AutoWakeUp;
        [JsonProperty(PropertyName = "Respawn at same location (true/false)")]
        public bool SameLocation;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private System.Random random;
    private const string permUse;
    private void Init();
    private void OnUserRespawned(IPlayer player);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Automatically wake up (true/false)")]
    public bool AutoWakeUp;
    [JsonProperty(PropertyName = "Respawn at same location (true/false)")]
    public bool SameLocation;
    public static Configuration DefaultConfig();
}


```

---

## RespawnProtection by Ryz0r - Gives players protection from other players damaging them, or NPC

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Rust;

Oxide.Plugins
[Info("Respawn Protection", "Ryz0r", "2.0.2")]
[Description("Allows players to have protection when they respawn.")]
public class RespawnProtection : RustPlugin
{
    private const string ProtectionPerm;
    private Dictionary<ulong, DateTime> _protectedPlayers;
    private Dictionary<ulong, Timer> timerList;
    [PluginReference]
     Plugin NoEscape;
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty(PropertyName = "Respawn Protection Timer")]
        public float RespawnProtectionTime;
        [JsonProperty(PropertyName = "Disable Protection If Raid Blocked")]
        public bool DisableProtectionIfRaidBlock;
        [JsonProperty(PropertyName = "Protect From PVE")]
        public bool ProtectFromPVE;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void Unload();
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private bool IsRaidBlocked(BasePlayer target);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Respawn Protection Timer")]
    public float RespawnProtectionTime;
    [JsonProperty(PropertyName = "Disable Protection If Raid Blocked")]
    public bool DisableProtectionIfRaidBlock;
    [JsonProperty(PropertyName = "Protect From PVE")]
    public bool ProtectFromPVE;
}


```

---

## RestartByUptime by  - Restarts server when uptime reaches specific time and players count is specific

```csharp
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Restart By Uptime", "Orange", "1.0.4")]
[Description("Restarts server when uptime reaches specific time and players count is specific")]
public class RestartByUptime : CovalencePlugin
{
    private DateTime nextRestartTime;
    private Timer timerObject;
    private void OnServerInitialized();
    private void Unload();
    private void CheckUptime();
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Max Uptime (seconds)")]
        public float maxUptimeSeconds;
        [JsonProperty(PropertyName = "If on server more than X players, don't trigger restart")]
        public int playersBound;
        [JsonProperty(PropertyName = "Command")]
        public string command;
    }

    protected override void LoadConfig();
    private void ValidateConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Max Uptime (seconds)")]
    public float maxUptimeSeconds;
    [JsonProperty(PropertyName = "If on server more than X players, don't trigger restart")]
    public int playersBound;
    [JsonProperty(PropertyName = "Command")]
    public string command;
}


```

---

## RestoreUponDeath by k1lly0u - Restores player inventories on death and removes the items from their corpse

```csharp
using Facepunch;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;
using System;

Oxide.Plugins
[Info("Restore Upon Death", "k1lly0u", "0.6.3")]
[Description("Restores player inventories on death and removes the items from their corpse")]
 class RestoreUponDeath : RustPlugin
{
    private const int SmallBackpackItemId;
    private const int LargeBackpackItemId;
    private RestoreData restoreData;
    private DynamicConfigFile restorationData;
    private bool wipeData;
    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave(string fileName);
    private void OnServerSave();
    private object CanDropActiveItem(BasePlayer player);
    private void OnEntityDeath(BasePlayer player, HitInfo info);
    private void OnEntityKill(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private bool ShouldBlockRestore(BasePlayer player);
    private void TryRestorePlayer(BasePlayer player);
    private bool HasAnyPermission(string playerId);
    private bool HasAnyPermission(string playerId, ConfigData.LossAmounts lossAmounts);
    private void StripContainer(ItemContainer container);
    private void EnableBackpackDropOnDeath(int itemId, bool enabled);
    private ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty("Give default items upon respawn if the players is having items restored")]
        public bool DefaultItems { get; set; }
        [JsonProperty("Can drop active item on death")]
        public bool DropActiveItem { get; set; }
        [JsonProperty("Can drop backpack on death")]
        public bool DropBackpack { get; set; }
        [JsonProperty("Don't restore items if player commited suicide")]
        public bool NoSuicideRestore { get; set; }
        [JsonProperty("Wipe stored data when the map wipes")]
        public bool WipeOnNewSave { get; set; }
        [JsonProperty("Purge expired user data after X amount of days")]
        public int PurgeDays { get; set; }
        [JsonProperty("Percentage of total items lost (Permission Name | Percentage (0 - 100))")]
        public Dictionary<string, LossAmounts> Permissions { get; set; }
        public class LossAmounts
        {
            public int Belt { get; set; }
            public int Wear { get; set; }
            public int Main { get; set; }
        }

        public VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private double CurrentTime();
    private void SaveData();
    private void LoadData();
    private class RestoreData
    {
        public Hash<ulong, PlayerData> restoreData;
        public void AddData(BasePlayer player, ConfigData.LossAmounts lossAmounts, double expires);
        public void RemoveData(ulong playerId);
        public bool HasRestoreData(ulong playerId);
        public void RestorePlayer(BasePlayer player);
        private void RestoreAllItems(BasePlayer player, PlayerData playerData);
        private static bool RestoreItems(BasePlayer player, ItemData[] itemData, ContainerType containerType);
        public class PlayerData
        {
            public ItemData[] containerMain;
            public ItemData[] containerWear;
            public ItemData[] containerBelt;
            public double expires;
            public PlayerData();
            public PlayerData(BasePlayer player, ConfigData.LossAmounts lossAmounts, double expires);
            public bool HasExpired(double currentTime);
            private static ItemData[] GetItems(ItemContainer container, int lossPercentage);
        }

    }

    public class ItemData
    {
        public int itemid;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public ulong skin;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public string displayName;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int amount;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public float condition;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public float maxCondition;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int ammo;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public string ammotype;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int position;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int frequency;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public InstanceData instanceData;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public string text;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public Item.Flag flags;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public ItemData[] contents;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public ItemContainer container;
        public ItemData();
        public ItemData(Item item);
        public class InstanceData
        {
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int dataInt;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int blueprintTarget;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int blueprintAmount;
            public InstanceData();
            public InstanceData(Item item);
            public void Restore(Item item);
            public bool IsValid();
        }

        public class ItemContainer
        {
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int slots;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public float temperature;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int flags;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int allowedContents;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int maxStackSize;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public List<int> allowedItems;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public List<int> availableSlots;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public int volume;
            [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
            public List<ItemData> contents;
            public ItemContainer();
            public ItemContainer(global::ItemContainer container);
            public void Load(global::ItemContainer itemContainer);
        }

    }

    public static Item CreateItem(ItemData itemData);
    private static T FindItemMod(Item item);
}

private class ConfigData
{
    [JsonProperty("Give default items upon respawn if the players is having items restored")]
    public bool DefaultItems { get; set; }
    [JsonProperty("Can drop active item on death")]
    public bool DropActiveItem { get; set; }
    [JsonProperty("Can drop backpack on death")]
    public bool DropBackpack { get; set; }
    [JsonProperty("Don't restore items if player commited suicide")]
    public bool NoSuicideRestore { get; set; }
    [JsonProperty("Wipe stored data when the map wipes")]
    public bool WipeOnNewSave { get; set; }
    [JsonProperty("Purge expired user data after X amount of days")]
    public int PurgeDays { get; set; }
    [JsonProperty("Percentage of total items lost (Permission Name | Percentage (0 - 100))")]
    public Dictionary<string, LossAmounts> Permissions { get; set; }
    public class LossAmounts
    {
        public int Belt { get; set; }
        public int Wear { get; set; }
        public int Main { get; set; }
    }

    public VersionNumber Version { get; set; }
}

public class LossAmounts
{
    public int Belt { get; set; }
    public int Wear { get; set; }
    public int Main { get; set; }
}

private class RestoreData
{
    public Hash<ulong, PlayerData> restoreData;
    public void AddData(BasePlayer player, ConfigData.LossAmounts lossAmounts, double expires);
    public void RemoveData(ulong playerId);
    public bool HasRestoreData(ulong playerId);
    public void RestorePlayer(BasePlayer player);
    private void RestoreAllItems(BasePlayer player, PlayerData playerData);
    private static bool RestoreItems(BasePlayer player, ItemData[] itemData, ContainerType containerType);
    public class PlayerData
    {
        public ItemData[] containerMain;
        public ItemData[] containerWear;
        public ItemData[] containerBelt;
        public double expires;
        public PlayerData();
        public PlayerData(BasePlayer player, ConfigData.LossAmounts lossAmounts, double expires);
        public bool HasExpired(double currentTime);
        private static ItemData[] GetItems(ItemContainer container, int lossPercentage);
    }

}

public class PlayerData
{
    public ItemData[] containerMain;
    public ItemData[] containerWear;
    public ItemData[] containerBelt;
    public double expires;
    public PlayerData();
    public PlayerData(BasePlayer player, ConfigData.LossAmounts lossAmounts, double expires);
    public bool HasExpired(double currentTime);
    private static ItemData[] GetItems(ItemContainer container, int lossPercentage);
}

public class ItemData
{
    public int itemid;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public ulong skin;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public string displayName;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int amount;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public float condition;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public float maxCondition;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int ammo;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public string ammotype;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int position;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int frequency;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public InstanceData instanceData;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public string text;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public Item.Flag flags;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public ItemData[] contents;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public ItemContainer container;
    public ItemData();
    public ItemData(Item item);
    public class InstanceData
    {
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int dataInt;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int blueprintTarget;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int blueprintAmount;
        public InstanceData();
        public InstanceData(Item item);
        public void Restore(Item item);
        public bool IsValid();
    }

    public class ItemContainer
    {
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int slots;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public float temperature;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int flags;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int allowedContents;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int maxStackSize;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public List<int> allowedItems;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public List<int> availableSlots;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public int volume;
        [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
        public List<ItemData> contents;
        public ItemContainer();
        public ItemContainer(global::ItemContainer container);
        public void Load(global::ItemContainer itemContainer);
    }

}

public class InstanceData
{
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int dataInt;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int blueprintTarget;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int blueprintAmount;
    public InstanceData();
    public InstanceData(Item item);
    public void Restore(Item item);
    public bool IsValid();
}

public class ItemContainer
{
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int slots;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public float temperature;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int flags;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int allowedContents;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int maxStackSize;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public List<int> allowedItems;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public List<int> availableSlots;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public int volume;
    [JsonProperty(DefaultValueHandling = DefaultValueHandling.Ignore, NullValueHandling = NullValueHandling.Ignore)]
    public List<ItemData> contents;
    public ItemContainer();
    public ItemContainer(global::ItemContainer container);
    public void Load(global::ItemContainer itemContainer);
}


```

---

## RestrictPlacement by ItzNxthaniel - Restricts placement of certain items by players

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Serialization;
using System.Collections.Generic;

Oxide.Plugins
[Info("Restrict Placement", "ItzNathaniel", "1.0.5")]
[Description("Restrict Users from placing certain items.")]
public class RestrictPlacement : RustPlugin
{
    private Configuration _config;
    private const string permissionBypass;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty("Blacklist")]
        public HashSet<string> blacklist;
    }

    protected override void LoadDefaultMessages();
    private void Init();
    private object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private object CheckBuild(Planner planner, Construction prefab, Construction.Target target);
}

private class Configuration
{
    [JsonProperty("Blacklist")]
    public HashSet<string> blacklist;
}


```

---

## ReviveArrows by VisEntities - Heal and revive the wounded from a distance

```csharp
using Network;
using Newtonsoft.Json;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Revive Arrows", "VisEntities", "3.0.2")]
[Description("Heal and revive the wounded from a distance.")]
public class ReviveArrows : RustPlugin
{
    private static ReviveArrows _plugin;
    private static Configuration _config;
    private const string FX_INJECT_FRIEND;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Instant Health Increase")]
        public float InstantHealthIncrease { get; set; }
        [JsonProperty("Health Increase Over Time")]
        public float HealthIncreaseOverTime { get; set; }
        [JsonProperty("Can Revive Wounded")]
        public bool CanReviveWounded { get; set; }
        [JsonProperty("Arrow Ingredients")]
        public List<ItemInfo> ArrowIngredients { get; set; }
    }

    public class ItemInfo
    {
        [JsonProperty("Shortname")]
        public string Shortname { get; set; }
        [JsonProperty("Amount")]
        public int Amount { get; set; }
        [JsonIgnore]
        private bool _validated;
        [JsonIgnore]
        private ItemDefinition _itemDefinition;
        [JsonIgnore]
        public ItemDefinition ItemDefinition { get; set; }
        public int GetItemAmount(ItemContainer container);
        public void GiveItem(BasePlayer player, ItemContainer container);
        public int TakeItem(BasePlayer player, ItemContainer container);
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private void OnPlayerAttack(BasePlayer player, HitInfo hitInfo);
    private void Heal(BasePlayer player);
    private static bool ChanceSucceeded(int percentage);
    private void SendGameTip(BasePlayer player, string message, float durationSeconds, object[] args);
    private static void RunEffect(string prefab, BaseEntity entity, uint boneId, Vector3 localPosition, Vector3 localDirection, Connection effectRecipient, bool sendToAll);
    private static class PermissionUtil
    {
        public const string USE;
        public static void RegisterPermissions();
        public static bool VerifyHasPermission(BasePlayer player, string permissionName);
    }

    private class Lang
    {
        public const string InsufficientIngredients;
        public const string PlayerHealed;
        public const string HealArrowUsage;
    }

    protected override void LoadDefaultMessages();
    private void SendReplyToPlayer(BasePlayer player, string messageKey, object[] args);
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Instant Health Increase")]
    public float InstantHealthIncrease { get; set; }
    [JsonProperty("Health Increase Over Time")]
    public float HealthIncreaseOverTime { get; set; }
    [JsonProperty("Can Revive Wounded")]
    public bool CanReviveWounded { get; set; }
    [JsonProperty("Arrow Ingredients")]
    public List<ItemInfo> ArrowIngredients { get; set; }
}

public class ItemInfo
{
    [JsonProperty("Shortname")]
    public string Shortname { get; set; }
    [JsonProperty("Amount")]
    public int Amount { get; set; }
    [JsonIgnore]
    private bool _validated;
    [JsonIgnore]
    private ItemDefinition _itemDefinition;
    [JsonIgnore]
    public ItemDefinition ItemDefinition { get; set; }
    public int GetItemAmount(ItemContainer container);
    public void GiveItem(BasePlayer player, ItemContainer container);
    public int TakeItem(BasePlayer player, ItemContainer container);
}

private static class PermissionUtil
{
    public const string USE;
    public static void RegisterPermissions();
    public static bool VerifyHasPermission(BasePlayer player, string permissionName);
}

private class Lang
{
    public const string InsufficientIngredients;
    public const string PlayerHealed;
    public const string HealArrowUsage;
}


```

---

## RfTool by PinguinNordpol - Manipulate and intercept in-game RF objects/signals

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("RF Tool", "PinguinNordpol", "0.2.4")]
[Description("Manipulates/intercepts in-game RF objects and signals")]
 class RfTool : CovalencePlugin
{
    private int frequency_min;
    private int frequency_max;
    private RfToolConfig config_data;
    private Timer listener_timer;
    private class ListenerData
    {
        public int frequency;
        public string msg;
        public int block_delay;
        public int cur_block_delay;
        public ListenerData(int _frequency, string _msg, int _block_delay);
        public int GetFrequency();
        public string GetMessage();
        public int GetBlockDelay();
        public void EnableBlocking();
        public void DisableBlocking();
        public bool IsBlocked();
    }

    private class RfToolConfig
    {
        public float tick_interval;
        public bool debug;
        public List<ListenerData> configured_listeners;
    }

    protected override void LoadDefaultConfig();
    private RfToolConfig GetDefaultConfig();
     void Init();
     void OnServerInitialized();
     void OnServerShutdown();
    protected override void LoadDefaultMessages();
    [Command("rftool")]
     void RfToolHelp(IPlayer player, string command, string[] args);
    [Command("rftool.inspect"), Permission("rftool.use")]
     void RfToolInspect(IPlayer player, string command, string[] args);
    [Command("rftool.enable"), Permission("rftool.use")]
     void RfToolEnable(IPlayer player, string command, string[] args);
    [Command("rftool.disable"), Permission("rftool.use")]
     void RfToolDisable(IPlayer player, string command, string[] args);
    [Command("rftool.listeners.add"), Permission("rftool.use")]
     void RfToolListenersAdd(IPlayer player, string command, string[] args);
    [Command("rftool.listeners.del"), Permission("rftool.use")]
     void RfToolListenersDel(IPlayer player, string command, string[] args);
    [Command("rftool.listeners.list"), Permission("rftool.use")]
     void RfToolListenersList(IPlayer player, string command, string[] args);
    [Command("rftool.listeners.set_interval"), Permission("rftool.use")]
     void RfToolListenersSetInterval(IPlayer player, string command, string[] args);
    [Command("rftool.listeners.get_interval"), Permission("rftool.use")]
     void RfToolListenersGetInterval(IPlayer player, string command, string[] args);
    [Command("rftool.debug"), Permission("rftool.use")]
     void RfToolDebug(IPlayer player, string command, string[] args);
    private void CheckListeners();
    private int GetFrequency(IPlayer player, string[] args);
    private bool IsListenerConfigured(int frequency);
    private void ResetListenersCurBlockDelay();
    private void ReplyToPlayer(IPlayer player, string msg);
    private void Log(string msg);
    private void LogDebug(string msg);
}

private class ListenerData
{
    public int frequency;
    public string msg;
    public int block_delay;
    public int cur_block_delay;
    public ListenerData(int _frequency, string _msg, int _block_delay);
    public int GetFrequency();
    public string GetMessage();
    public int GetBlockDelay();
    public void EnableBlocking();
    public void DisableBlocking();
    public bool IsBlocked();
}

private class RfToolConfig
{
    public float tick_interval;
    public bool debug;
    public List<ListenerData> configured_listeners;
}


```

---

## RidableDrones by WhiteThunder - Allows players to ride RC drones by standing on them or mounting a chair

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Ridable Drones", "WhiteThunder", "2.0.3")]
[Description("Allows players to deploy signs and chairs onto RC drones to allow riding them.")]
internal class RidableDrones : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin DroneSettings;
    private Configuration _config;
    private const string PermissionSignDeploy;
    private const string PermissionSignDeployFree;
    private const string PermissionChairDeploy;
    private const string PermissionChairDeployFree;
    private const string PermissionChairAutoDeploy;
    private const string PermissionChairPilot;
    private const string SmallWoodenSignPrefab;
    private const string DeploySignEffectPrefab;
    private const string PilotChairPrefab;
    private const string PassengerChairPrefab;
    private const string VisibleChairPrefab;
    private const string ChairDeployEffectPrefab;
    private const int SignItemId;
    private const int ChairItemId;
    private static SlotConfig ChairSlots;
    private static SlotConfig SignSlots;
    private readonly object True;
    private readonly object False;
    private static readonly Vector3 SignLocalPosition;
    private static readonly Vector3 SignLocalRotationAngles;
    private static readonly Vector3 PassengerChairLocalPosition;
    private static readonly Vector3 PilotChairLocalPosition;
    private readonly Dictionary<string, object> _signRemoveInfo;
    private readonly Dictionary<string, object> _refundInfo;
    private readonly Dictionary<string, object> _chairRemoveInfo;
    private readonly Dictionary<string, object> _chairRefundInfo;
    private readonly ObservableHashSet<BaseEntity> _chairDrones;
    private readonly ObservableHashSet<BaseEntity> _mountedChairDrones;
    private readonly ObservableHashSet<BaseEntity> _signDrones;
    private readonly GatedHookCollection[] _hookCollections;
    public RidableDrones();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(Drone drone);
    private void OnEntityBuilt(Planner planner, GameObject go);
    private object OnEntityTakeDamage(BaseChair mountable, HitInfo info);
    private object OnEntityTakeDamage(Signage sign, HitInfo info);
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private void OnEntityMounted(BaseMountable currentChair, BasePlayer player);
    private void OnEntityDismounted(BaseMountable previousChair, BasePlayer player);
    private void OnPlayerDismountFailed(BasePlayer player, BaseMountable mountable);
    private object CanPickupEntity(BasePlayer player, Drone drone);
    private object CanPickupEntity(BasePlayer player, Signage sign);
    private object CanPickupEntity(BasePlayer player, BaseChair chair);
    private Dictionary<string, object> OnRemovableEntityInfo(Signage sign, BasePlayer player);
    private Dictionary<string, object> OnRemovableEntityInfo(BaseChair chair, BasePlayer player);
    private string canRemove(BasePlayer player, Drone drone);
    private string OnDroneTypeDetermine(Drone drone, List<string> droneTypeList);
    private static class ExposedHooks
    {
        public static object OnDroneSignDeploy(Drone drone, BasePlayer deployer);
        public static void OnDroneSignDeployed(Drone drone, BasePlayer deployer);
        public static object OnDroneChairDeploy(Drone drone, BasePlayer deployer);
        public static void OnDroneChairDeployed(Drone drone, BasePlayer deployer);
        public static void OnDroneControlStarted(Drone drone, BasePlayer player);
        public static void OnDroneControlEnded(Drone drone, BasePlayer player);
    }

    [Command("dronesign")]
    private void DroneSignCommand(IPlayer player);
    [Command("dronechair", "droneseat")]
    private void DroneChairCommand(IPlayer player);
    private bool VerifyPlayer(IPlayer player, BasePlayer basePlayer);
    private bool VerifyPermission(IPlayer player, string perm);
    private bool VerifyDroneFound(IPlayer player, Drone drone);
    private bool VerifyDroneHasSlotVacant(IPlayer player, Drone drone, SlotConfig slotConfig);
    private static class RCUtils
    {
        public static bool IsRCDrone(Drone drone);
    }

    private static bool IsDroneEligible(Drone drone);
    private static Drone GetParentDrone(BaseEntity entity);
    private static Drone GetMountedDrone(BasePlayer player, BaseMountable currentChair);
    private static Drone GetMountedDrone(BasePlayer player);
    private static Signage GetDroneSign(Drone drone);
    private static bool IsDroneSign(Signage sign);
    private static bool TryGetChairs(Drone drone, BaseMountable pilotChair, BaseMountable passengerChair, BaseMountable visibleChair);
    private static bool TryGetChairs(Drone drone, BaseMountable pilotChair, BaseMountable passengerChair);
    private static bool TryGetPassengerChair(Drone drone, BaseMountable passengerChair);
    private static bool HasChair(Drone drone);
    private static bool IsDroneChair(BaseChair chair);
    private static void HitNotify(BaseEntity entity, HitInfo info);
    private static void RemoveProblemComponents(BaseEntity entity);
    private static void SetupChair(BaseMountable mountable);
    private static BaseEntity GetLookEntity(BasePlayer basePlayer, float maxDistance);
    private static void SwitchToChair(BasePlayer player, BaseMountable currentChair, BaseMountable desiredChair);
    private static string DetermineDroneType(Drone drone);
    private static bool CanPickupInternal(Drone drone, string errorLangKey);
    private static bool CanPickupInternal(Signage sign, string errorLangKey);
    private static bool CanPickupInternal(BaseChair chair, string errorLangKey);
    private void RefreshDroneSettingsProfile(Drone drone);
    private void SetupSign(Drone drone, Signage sign);
    private Signage TryDeploySign(Drone drone, BasePlayer deployer, bool allowRefund);
    private void SetupAllChairs(Drone drone, BaseMountable pilotChair, BaseMountable passengerChair, BaseMountable visibleChair);
    private BaseMountable TryDeployChairs(Drone drone, BasePlayer deployer, bool allowRefund);
    private void MaybeRefreshDroneSign(Drone drone);
    private void MaybeAutoDeployChair(Drone drone);
    private void MaybeAddOrRefreshChairs(Drone drone);
    private class ChairComponent : FacepunchBehaviour
    {
        public static void AddToDrone(RidableDrones plugin, Drone drone, BaseMountable pilotChair, BaseMountable passengerChair, BaseMountable visibleChair);
        public static void RemoveFromChair(BaseMountable chair);
        private const float ColliderHeight;
        private RidableDrones _plugin;
        private BaseMountable[] _chairs;
        private BaseEntity _drone;
        private GameObject _child;
        private bool _isUnloading;
        private void CreateCollider(Drone drone, BaseMountable passengerChair);
        private void OnDestroy();
    }

    private class ObservableHashSet : HashSet<T>, IObservable
    {
        public new bool Add(T item);
        public new bool Remove(T item);
        public new void Clear();
    }

    private class ObservableGate : IObservableGate
    {
        private readonly Func<bool> _enableWhen;
        public bool Enabled { get; set; }
        public ObservableGate(IObservable observable, Func<bool> enableWhen);
        private void HandleChange();
    }

    private class MultiObservableGate : IObservableGate
    {
        private readonly IObservableGate[] _gates;
        public bool Enabled { get; set; }
        public MultiObservableGate(IObservableGate[] gates);
        private void HandleChange();
    }

    private class GatedHookCollection
    {
        public bool IsSubscribed { get; set; }
        private readonly RidableDrones _plugin;
        private readonly IObservableGate _gate;
        private readonly string[] _hookNames;
        public GatedHookCollection(RidableDrones plugin, IObservableGate gate, string[] hookNames);
        public void Subscribe();
        public void Unsubscribe();
        public void Refresh(bool shouldSubscribe);
        public void Refresh();
    }

    private class SlotConfig
    {
        public readonly BaseEntity.Slot[] Slots;
        public SlotConfig(BaseEntity.Slot[] slots);
        public BaseEntity GetOccupant(BaseEntity host);
        public bool IsCompatibleWithHost(BaseEntity host);
        public void OccupyHost(BaseEntity host, BaseEntity occupant);
    }

    private class SignTriggerParentEnclosed : TriggerParentEnclosed
    {
        public static SignTriggerParentEnclosed AddToDrone(Drone drone, GameObject host);
        private Drone _drone;
        public override bool ShouldParent(BaseEntity entity, bool bypassOtherTriggerCheck);
    }

    private class SignComponent : EntityComponent<BaseEntity>
    {
        public static void AddToDrone(RidableDrones plugin, Drone drone, Signage sign);
        public static void RemoveFromSign(Signage sign);
        private const float ColliderHeight;
        private RidableDrones _plugin;
        private Drone _drone;
        private GameObject _triggerHost;
        private void CreateParentTrigger(Drone drone, Signage sign);
        private void OnDestroy();
    }

    private class DroneController : FacepunchBehaviour
    {
        public static bool Exists(BasePlayer player);
        public static void Mount(RidableDrones plugin, BasePlayer player, Drone drone, bool isPilotChair);
        public static void Dismount(RidableDrones plugin, BasePlayer player, Drone drone);
        public static void RemoveFromPlayer(BasePlayer player);
        private Drone _drone;
        private BasePlayer _controller;
        private CameraViewerId _viewerId;
        private bool _isPilotChair;
        private void DelayedDestroy();
        private void OnMount(BasePlayer controller, Drone drone, bool isPilotChair);
        private void OnDismount();
        private void Update();
        private void OnDestroy();
    }

    private class Configuration : BaseConfiguration
    {
        [JsonProperty("Chair tip chance")]
        public int ChairTipChance;
        [JsonProperty("Sign tip chance")]
        public int SignTipChance;
        [JsonProperty("TipChance")]
        private int DeprecatedTipChance { get; set; }
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
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    private static class Lang
    {
        public const string TipDeployChairCommand;
        public const string TipDeploySignCommand;
        public const string InfoSignName;
        public const string InfoChairName;
        public const string ErrorNoPermission;
        public const string ErrorNoDroneFound;
        public const string ErrorNoSignItem;
        public const string ErrorNoChairItem;
        public const string ErrorAlreadyHasChair;
        public const string ErrorAlreadyHasSign;
        public const string ErrorIncompatibleAttachment;
        public const string ErrorDeploySignFailed;
        public const string ErrorDeployChairFailed;
        public const string ErrorCannotPickupWithSign;
        public const string ErrorCannotPickupWithChair;
        public const string ErrorCannotPickupAttachment;
    }

    protected override void LoadDefaultMessages();
}

private static class ExposedHooks
{
    public static object OnDroneSignDeploy(Drone drone, BasePlayer deployer);
    public static void OnDroneSignDeployed(Drone drone, BasePlayer deployer);
    public static object OnDroneChairDeploy(Drone drone, BasePlayer deployer);
    public static void OnDroneChairDeployed(Drone drone, BasePlayer deployer);
    public static void OnDroneControlStarted(Drone drone, BasePlayer player);
    public static void OnDroneControlEnded(Drone drone, BasePlayer player);
}

private static class RCUtils
{
    public static bool IsRCDrone(Drone drone);
}

private class ChairComponent : FacepunchBehaviour
{
    public static void AddToDrone(RidableDrones plugin, Drone drone, BaseMountable pilotChair, BaseMountable passengerChair, BaseMountable visibleChair);
    public static void RemoveFromChair(BaseMountable chair);
    private const float ColliderHeight;
    private RidableDrones _plugin;
    private BaseMountable[] _chairs;
    private BaseEntity _drone;
    private GameObject _child;
    private bool _isUnloading;
    private void CreateCollider(Drone drone, BaseMountable passengerChair);
    private void OnDestroy();
}

private class ObservableHashSet : HashSet<T>, IObservable
{
    public new bool Add(T item);
    public new bool Remove(T item);
    public new void Clear();
}

private class ObservableGate : IObservableGate
{
    private readonly Func<bool> _enableWhen;
    public bool Enabled { get; set; }
    public ObservableGate(IObservable observable, Func<bool> enableWhen);
    private void HandleChange();
}

private class MultiObservableGate : IObservableGate
{
    private readonly IObservableGate[] _gates;
    public bool Enabled { get; set; }
    public MultiObservableGate(IObservableGate[] gates);
    private void HandleChange();
}

private class GatedHookCollection
{
    public bool IsSubscribed { get; set; }
    private readonly RidableDrones _plugin;
    private readonly IObservableGate _gate;
    private readonly string[] _hookNames;
    public GatedHookCollection(RidableDrones plugin, IObservableGate gate, string[] hookNames);
    public void Subscribe();
    public void Unsubscribe();
    public void Refresh(bool shouldSubscribe);
    public void Refresh();
}

private class SlotConfig
{
    public readonly BaseEntity.Slot[] Slots;
    public SlotConfig(BaseEntity.Slot[] slots);
    public BaseEntity GetOccupant(BaseEntity host);
    public bool IsCompatibleWithHost(BaseEntity host);
    public void OccupyHost(BaseEntity host, BaseEntity occupant);
}

private class SignTriggerParentEnclosed : TriggerParentEnclosed
{
    public static SignTriggerParentEnclosed AddToDrone(Drone drone, GameObject host);
    private Drone _drone;
    public override bool ShouldParent(BaseEntity entity, bool bypassOtherTriggerCheck);
}

private class SignComponent : EntityComponent<BaseEntity>
{
    public static void AddToDrone(RidableDrones plugin, Drone drone, Signage sign);
    public static void RemoveFromSign(Signage sign);
    private const float ColliderHeight;
    private RidableDrones _plugin;
    private Drone _drone;
    private GameObject _triggerHost;
    private void CreateParentTrigger(Drone drone, Signage sign);
    private void OnDestroy();
}

private class DroneController : FacepunchBehaviour
{
    public static bool Exists(BasePlayer player);
    public static void Mount(RidableDrones plugin, BasePlayer player, Drone drone, bool isPilotChair);
    public static void Dismount(RidableDrones plugin, BasePlayer player, Drone drone);
    public static void RemoveFromPlayer(BasePlayer player);
    private Drone _drone;
    private BasePlayer _controller;
    private CameraViewerId _viewerId;
    private bool _isPilotChair;
    private void DelayedDestroy();
    private void OnMount(BasePlayer controller, Drone drone, bool isPilotChair);
    private void OnDismount();
    private void Update();
    private void OnDestroy();
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("Chair tip chance")]
    public int ChairTipChance;
    [JsonProperty("Sign tip chance")]
    public int SignTipChance;
    [JsonProperty("TipChance")]
    private int DeprecatedTipChance { get; set; }
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

private static class Lang
{
    public const string TipDeployChairCommand;
    public const string TipDeploySignCommand;
    public const string InfoSignName;
    public const string InfoChairName;
    public const string ErrorNoPermission;
    public const string ErrorNoDroneFound;
    public const string ErrorNoSignItem;
    public const string ErrorNoChairItem;
    public const string ErrorAlreadyHasChair;
    public const string ErrorAlreadyHasSign;
    public const string ErrorIncompatibleAttachment;
    public const string ErrorDeploySignFailed;
    public const string ErrorDeployChairFailed;
    public const string ErrorCannotPickupWithSign;
    public const string ErrorCannotPickupWithChair;
    public const string ErrorCannotPickupAttachment;
}


```

---

## RidableDrones_hVdQK by WhiteThunder - Allows changing speed, toughness and other properties of RC drones

```csharp
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using VLB;

Oxide.Plugins
[Info("Drone Settings", "WhiteThunder", "1.3.0")]
[Description("Allows changing speed, toughness and other properties of RC drones.")]
internal class DroneSettings : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin DroneScaleManager;
    private Configuration _config;
    private const string PermissionProfilePrefix;
    private const string BaseDroneType;
    private DroneProperties _vanillaDroneProperties;
    private ProtectionProperties _vanillaDroneProtection;
    private readonly List<ProtectionProperties> _customProtectionProperties;
    private readonly List<string> _reusableDroneTypeList;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(Drone drone);
    private void OnBookmarkControlStarted(ComputerStation station, BasePlayer player, string bookmarkName, Drone drone);
    private void OnBookmarkControlEnded(ComputerStation station, BasePlayer player, Drone drone);
    private void OnDroneScaled(Drone drone, BaseEntity rootEntity, float scale, float previousScale);
    private void API_RefreshDroneProfile(Drone drone);
    private static bool ApplySettingsWasBlocked(Drone drone);
    private static bool IsDroneEligible(Drone drone);
    private static string GetProfilePermission(string droneType, string profileSuffix);
    private static Drone GetControlledDrone(ComputerStation station);
    private string DetermineBestDroneType(List<string> droneTypeList);
    private string DetermineDroneType(Drone drone);
    private DroneProfile GetDroneProfile(Drone drone);
    private BaseEntity GetRootEntity(Drone drone);
    private BaseEntity GetDroneOrRootEntity(Drone drone);
    private void RestoreVanillaSettings(Drone drone);
    private bool TryApplyProfile(Drone drone, DroneProfile profile, bool restoreVanilla);
    private ProtectionProperties CreateProtectionProperties(Dictionary<string, float> damageMap);
    private class DroneConnectionFixer : FacepunchBehaviour
    {
        public static void OnControlStarted(DroneSettings plugin, Drone drone, BasePlayer player);
        public static void OnControlEnded(Drone drone, BasePlayer player);
        public static void OnRootEntityChanged(Drone drone, BaseEntity rootEntity);
        public static void RemoveFromDrone(Drone drone);
        private BaseEntity _rootEntity;
        private List<BasePlayer> _viewers;
        private bool _isCallingCustomUpdateNetworkGroup;
        private Action _updateNetworkGroup;
        private Action _customUpdateNetworkGroup;
        private DroneConnectionFixer();
        private void SetRootEntity(BaseEntity rootEntity);
        private void RemoveController(BasePlayer player);
        private void LateUpdate();
        private void SendFakeUpdateNetworkGroup(BaseEntity entity, BasePlayer player, uint groupId);
        private void CustomUpdateNetworkGroup();
        private void OnDestroy();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class DroneProfile
    {
        [JsonProperty("PermissionSuffix", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string PermissionSuffix;
        [JsonProperty("DroneProperties")]
        public DroneProperties DroneProperties;
        [JsonProperty("DamageScale")]
        public Dictionary<string, float> DamageScale;
        [JsonIgnore]
        public ProtectionProperties ProtectionProperties;
        [JsonIgnore]
        public string Permission;
        public void Init(DroneSettings plugin, string droneType, bool requiresPermission);
        public void ApplyToDrone(Drone drone);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class DroneProperties
    {
        public static DroneProperties FromDrone(Drone drone);
        [JsonProperty("KillInWater")]
        public bool KillInWater;
        [JsonProperty("DisableWhenHurtChance")]
        public float DisableWhenHurtChance;
        [JsonProperty("MovementAcceleration")]
        public float MovementAcceleration;
        [JsonProperty("AltitudeAcceleration")]
        public float AltitudeAcceleration;
        [JsonProperty("LeanWeight")]
        public float LeanWeight;
        public void ApplyToDrone(Drone drone);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class DroneTypeConfig
    {
        [JsonProperty("DefaultProfile")]
        public DroneProfile DefaultProfile;
        [JsonProperty("ProfilesRequiringPermission")]
        public DroneProfile[] ProfilesRequiringPermission;
        public void Init(DroneSettings plugin, string droneType);
        public DroneProfile GetProfileForOwner(DroneSettings plugin, ulong ownerId);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : BaseConfiguration
    {
        public override bool Migrate();
        [JsonIgnore]
        private readonly Dictionary<string, string> _droneTypeAliases;
        [JsonProperty("DroneTypePriority")]
        public string[] DroneTypePriority;
        [JsonProperty("SettingsByDroneType")]
        public Dictionary<string, DroneTypeConfig> SettingsByDroneType;
        public void Init(DroneSettings plugin);
        public DroneProfile FindProfile(DroneSettings plugin, string droneType, ulong ownerId);
    }

    private Configuration GetDefaultConfig();
    private class BaseConfiguration
    {
        public virtual bool Migrate();
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

private class DroneConnectionFixer : FacepunchBehaviour
{
    public static void OnControlStarted(DroneSettings plugin, Drone drone, BasePlayer player);
    public static void OnControlEnded(Drone drone, BasePlayer player);
    public static void OnRootEntityChanged(Drone drone, BaseEntity rootEntity);
    public static void RemoveFromDrone(Drone drone);
    private BaseEntity _rootEntity;
    private List<BasePlayer> _viewers;
    private bool _isCallingCustomUpdateNetworkGroup;
    private Action _updateNetworkGroup;
    private Action _customUpdateNetworkGroup;
    private DroneConnectionFixer();
    private void SetRootEntity(BaseEntity rootEntity);
    private void RemoveController(BasePlayer player);
    private void LateUpdate();
    private void SendFakeUpdateNetworkGroup(BaseEntity entity, BasePlayer player, uint groupId);
    private void CustomUpdateNetworkGroup();
    private void OnDestroy();
}

[JsonObject(MemberSerialization.OptIn)]
private class DroneProfile
{
    [JsonProperty("PermissionSuffix", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string PermissionSuffix;
    [JsonProperty("DroneProperties")]
    public DroneProperties DroneProperties;
    [JsonProperty("DamageScale")]
    public Dictionary<string, float> DamageScale;
    [JsonIgnore]
    public ProtectionProperties ProtectionProperties;
    [JsonIgnore]
    public string Permission;
    public void Init(DroneSettings plugin, string droneType, bool requiresPermission);
    public void ApplyToDrone(Drone drone);
}

[JsonObject(MemberSerialization.OptIn)]
private class DroneProperties
{
    public static DroneProperties FromDrone(Drone drone);
    [JsonProperty("KillInWater")]
    public bool KillInWater;
    [JsonProperty("DisableWhenHurtChance")]
    public float DisableWhenHurtChance;
    [JsonProperty("MovementAcceleration")]
    public float MovementAcceleration;
    [JsonProperty("AltitudeAcceleration")]
    public float AltitudeAcceleration;
    [JsonProperty("LeanWeight")]
    public float LeanWeight;
    public void ApplyToDrone(Drone drone);
}

[JsonObject(MemberSerialization.OptIn)]
private class DroneTypeConfig
{
    [JsonProperty("DefaultProfile")]
    public DroneProfile DefaultProfile;
    [JsonProperty("ProfilesRequiringPermission")]
    public DroneProfile[] ProfilesRequiringPermission;
    public void Init(DroneSettings plugin, string droneType);
    public DroneProfile GetProfileForOwner(DroneSettings plugin, ulong ownerId);
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : BaseConfiguration
{
    public override bool Migrate();
    [JsonIgnore]
    private readonly Dictionary<string, string> _droneTypeAliases;
    [JsonProperty("DroneTypePriority")]
    public string[] DroneTypePriority;
    [JsonProperty("SettingsByDroneType")]
    public Dictionary<string, DroneTypeConfig> SettingsByDroneType;
    public void Init(DroneSettings plugin);
    public DroneProfile FindProfile(DroneSettings plugin, string droneType, ulong ownerId);
}

private class BaseConfiguration
{
    public virtual bool Migrate();
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

## RidableHorseOptions by  - Provides options for rideable horses

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Ridable Horse Options", "Arainrr", "1.1.3")]
[Description("Controls all rideable horses on your server")]
public class RidableHorseOptions : RustPlugin
{
    private const string PREFAB_RIDABLE_HORSE;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(RidableHorse ridableHorse);
    private void UpdateConfig();
    private void ApplyHorseSettings(RidableHorse ridableHorse);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Horse Settings")]
        public Dictionary<string, HorseSettings> horseSettings;
    }

    private class HorseSettings
    {
        public float maxHealth;
        public float maxSpeed;
        public float walkSpeed;
        public float trotSpeed;
        public float runSpeed;
        public float turnSpeed;
        public float roadSpeedBonus;
        public float maxStaminaSeconds;
        public float staminaCoreSpeedBonus;
        public float staminaReplenishRatioMoving;
        public float staminaReplenishRatioStanding;
        public float staminaCoreLossRatio;
        public float maxWaterDepth;
        public float maxWallClimbSlope;
        public float maxStepHeight;
        public float maxStepDownHeight;
        public float maxStaminaCoreFromWater;
        public float caloriesToDigestPerHour;
        public float dungProducedPerCalorie;
        public float obstacleDetectionRadius;
        public float calorieToStaminaRatio;
        public float hydrationToStaminaRatio;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Horse Settings")]
    public Dictionary<string, HorseSettings> horseSettings;
}

private class HorseSettings
{
    public float maxHealth;
    public float maxSpeed;
    public float walkSpeed;
    public float trotSpeed;
    public float runSpeed;
    public float turnSpeed;
    public float roadSpeedBonus;
    public float maxStaminaSeconds;
    public float staminaCoreSpeedBonus;
    public float staminaReplenishRatioMoving;
    public float staminaReplenishRatioStanding;
    public float staminaCoreLossRatio;
    public float maxWaterDepth;
    public float maxWallClimbSlope;
    public float maxStepHeight;
    public float maxStepDownHeight;
    public float maxStaminaCoreFromWater;
    public float caloriesToDigestPerHour;
    public float dungProducedPerCalorie;
    public float obstacleDetectionRadius;
    public float calorieToStaminaRatio;
    public float hydrationToStaminaRatio;
}


```

---

## Robbery by MrBlue - Players can steal money, points, and/or items from other players

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Robbery", "Wulf", "4.1.5")]
[Description("Players can steal money, points, and/or items from other players")]
public class Robbery : RustPlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Allow item stealing (true/false)")]
        public bool ItemStealing;
        [JsonProperty(PropertyName = "Allow money stealing (true/false)")]
        public bool MoneyStealing;
        [JsonProperty(PropertyName = "Allow point stealing (true/false)")]
        public bool PointStealing;
        [JsonProperty(PropertyName = "Clan protection (true/false)")]
        public bool ClanProtection;
        [JsonProperty(PropertyName = "Friend protection (true/false)")]
        public bool FriendProtection;
        [JsonProperty(PropertyName = "Maximum chance of stealing an item (0 - 100)")]
        public int MaxChanceItem;
        [JsonProperty(PropertyName = "Maximum chance of stealing money (0 - 100)")]
        public int MaxChanceMoney;
        [JsonProperty(PropertyName = "Maximum chance of stealing points (0 - 100)")]
        public int MaxChancePoints;
        [JsonProperty(PropertyName = "Usage cooldown (seconds, 0 to disable)")]
        public int UsageCooldown;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    [PluginReference]
    private Plugin Clans;
    private Plugin Economics;
    private Plugin EventManager;
    private Plugin Factions;
    private Plugin Friends;
    private Plugin RustIO;
    private Plugin ServerRewards;
    private Plugin UEconomics;
    private Plugin ZoneManager;
    private readonly Hash<string, float> cooldowns;
    private static System.Random random;
    private const string permKilling;
    private const string permMugging;
    private const string permPickpocket;
    private const string permProtection;
    private void Init();
    private void StealPoints(BasePlayer victim, BasePlayer attacker);
    private void StealMoney(BasePlayer victim, BasePlayer attacker);
    private void StealItem(BasePlayer victim, BasePlayer attacker);
    private bool InNoLootZone(BasePlayer victim, BasePlayer attacker);
    private bool IsFriend(BasePlayer victim, BasePlayer attacker);
    private bool IsClanmate(BasePlayer victim, BasePlayer attacker);
    private void OnEntityDeath(BaseEntity entity, HitInfo info);
    private void OnEntityTakeDamage(BaseEntity entity, HitInfo info);
    private void OnPlayerInput(BasePlayer attacker, InputState input);
    private static BaseEntity FindObject(Ray ray, float distance);
    private string Lang(string key, string id, object[] args);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Allow item stealing (true/false)")]
    public bool ItemStealing;
    [JsonProperty(PropertyName = "Allow money stealing (true/false)")]
    public bool MoneyStealing;
    [JsonProperty(PropertyName = "Allow point stealing (true/false)")]
    public bool PointStealing;
    [JsonProperty(PropertyName = "Clan protection (true/false)")]
    public bool ClanProtection;
    [JsonProperty(PropertyName = "Friend protection (true/false)")]
    public bool FriendProtection;
    [JsonProperty(PropertyName = "Maximum chance of stealing an item (0 - 100)")]
    public int MaxChanceItem;
    [JsonProperty(PropertyName = "Maximum chance of stealing money (0 - 100)")]
    public int MaxChanceMoney;
    [JsonProperty(PropertyName = "Maximum chance of stealing points (0 - 100)")]
    public int MaxChancePoints;
    [JsonProperty(PropertyName = "Usage cooldown (seconds, 0 to disable)")]
    public int UsageCooldown;
    public static Configuration DefaultConfig();
}


```

---

## RockBlock by nivex - Blocks players from building in rocks

```csharp
using Rust;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Rock Block", "Author Nogrod, Maintainer nivex", "1.1.4")]
[Description("Blocks players from building in rocks")]
 class RockBlock : RustPlugin
{
    private ConfigData config;
    private RaycastHit _hit;
    private const string permBypass;
    private readonly int worldLayer;
    private Dictionary<string, string> _displayNames;
    private class ConfigData
    {
        public bool AllowCave { get; set; }
        public bool Logging { get; set; }
        public int MaxHeight { get; set; }
        public bool Kill { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    private void Init();
    private void OnServerInitialized();
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private void OnItemDeployed(Deployer deployer, BaseEntity entity);
    private void CheckEntity(BaseEntity entity, BasePlayer player);
    private bool IsInCave(BaseEntity entity);
    private bool IsInside(Collider collider, BaseEntity entity);
    private bool IsInside(Vector3 point);
    private Vector3 RotatePointAroundPivot(Vector3 point, Vector3 pivot, Quaternion rotation);
}

private class ConfigData
{
    public bool AllowCave { get; set; }
    public bool Logging { get; set; }
    public int MaxHeight { get; set; }
    public bool Kill { get; set; }
}


```

---

## RocketFire by birthdates - Allows firing X amount of rockets at once

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Rocket Fire", "birthdates", "1.0.4")]
[Description("Ability to fire x amount of rockets at once")]
public class RocketFire : RustPlugin
{
    private const string Perm;
    private void Init();
    [ConsoleCommand("fr")]
    private void FireRocketsConsoleCMD(ConsoleSystem.Arg arg);
    [ChatCommand("fr")]
    private void FireRocketsCMD(BasePlayer player, string command, string[] args);
    private void FRCommand(BasePlayer player, string[] args);
    public ConfigFile _config;
    protected override void LoadDefaultMessages();
    public class ConfigFile
    {
        [JsonProperty("Max amount of rockets to fire at once")]
        public int maxRockets;
        [JsonProperty("Delay in between multiple rocket shots (e.g /fr 10)")]
        public float delay;
        [JsonProperty("Rockets fire at the same position if you move when you shoot multiple (e.g /fr 10)")]
        public bool staticRockets;
        [JsonProperty("The rocket velocity")]
        public float velocity;
        [JsonProperty("The rocket type")]
        public string rocketType;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Max amount of rockets to fire at once")]
    public int maxRockets;
    [JsonProperty("Delay in between multiple rocket shots (e.g /fr 10)")]
    public float delay;
    [JsonProperty("Rockets fire at the same position if you move when you shoot multiple (e.g /fr 10)")]
    public bool staticRockets;
    [JsonProperty("The rocket velocity")]
    public float velocity;
    [JsonProperty("The rocket type")]
    public string rocketType;
    public static ConfigFile DefaultConfig();
}


```

---

## RocketGuns by SapnuPuas - allow players to fire rockets from 556 weapons

```csharp
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;

Oxide.Plugins
[Info("Rocket Guns", "Sapnu Puas #3696", "1.2.4")]
[Description("Allow 556 guns to fire rockets based on ammo type")]
public class RocketGuns : RustPlugin
{
     List<ulong> ActiveGUNS;
    private const string PermUse;
    public string Rocket;
    public string Hv;
    public string Fire;
     void DestroyCUI(BasePlayer player);
    public void CreateAmmoIcon(BasePlayer player, int ammo);
    private void ToggleRocketCMD(IPlayer player, string command, string[] args);
     void Unload();
    private void Init();
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerDeath(BasePlayer player, HitInfo info);
     object OnWeaponReload(BaseProjectile weapon, BasePlayer player);
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
     void OnAmmoUnload(BaseProjectile weapon, Item item, BasePlayer player);
     void OnWeaponFired(BaseProjectile weapon, BasePlayer player, ItemModProjectile ammo, ProtoBuf.ProjectileShoot projectiles);
    public string RocketToFire(int id);
    public int RocketIcon(int id);
    public void UpdateIcon(BasePlayer player);
    public void FireRockets(BasePlayer player, string rocketPrefab);
    public Configuration config;
    public class Configuration
    {
        [JsonProperty("Toggle command")]
        public string Command;
        [JsonProperty("1 = normal rocket, 2 = hv rocket , 3 = incendiary rocket , 0 = none")]
        public int notused;
        [JsonProperty("rocket type to fire when using normall 5.56")]
        public int Normal;
        [JsonProperty("rocket type to fire when using hv 5.56")]
        public int Hv;
        [JsonProperty("rocket type to fire when using Incendiary 5.56")]
        public int Fire;
        [JsonProperty("rocket type to fire when using explosive 5.56")]
        public int Explo;
        [JsonProperty("Hv rocket speed ")]
        public float HvSpeed;
        [JsonProperty("Normal rocket speed")]
        public float NormalSpeed;
        [JsonProperty("Incendiary rocket speed")]
        public float FireSpeed;
        [JsonProperty("Image AnchorMin")]
        public string ImageAnchorMin;
        [JsonProperty("Image AnchorMax")]
        public string ImageAnchorMax;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
}

public class Configuration
{
    [JsonProperty("Toggle command")]
    public string Command;
    [JsonProperty("1 = normal rocket, 2 = hv rocket , 3 = incendiary rocket , 0 = none")]
    public int notused;
    [JsonProperty("rocket type to fire when using normall 5.56")]
    public int Normal;
    [JsonProperty("rocket type to fire when using hv 5.56")]
    public int Hv;
    [JsonProperty("rocket type to fire when using Incendiary 5.56")]
    public int Fire;
    [JsonProperty("rocket type to fire when using explosive 5.56")]
    public int Explo;
    [JsonProperty("Hv rocket speed ")]
    public float HvSpeed;
    [JsonProperty("Normal rocket speed")]
    public float NormalSpeed;
    [JsonProperty("Incendiary rocket speed")]
    public float FireSpeed;
    [JsonProperty("Image AnchorMin")]
    public string ImageAnchorMin;
    [JsonProperty("Image AnchorMax")]
    public string ImageAnchorMax;
}


```

---

## RotatingBillboards by k1lly0u - Creates signs that rotates on the spot

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;

Oxide.Plugins
[Info("Rotating Billboards", "k1lly0u", "0.1.3", ResourceId = 2199)]
[Description("Creates signs that rotate on the spot")]
 class RotatingBillboards : RustPlugin
{
     StoredData storedData;
    private DynamicConfigFile data;
    private List<Rotator> billBoards;
    static RotatingBillboards instance;
    private Vector3 eyesAdjust;
    private bool initialized;
     void Loaded();
     void OnServerInitialized();
     void OnEntityKill(BaseNetworkable netEntity);
     void Unload();
     class Rotator : Signage
    {
        private Signage entity;
        private float secsToTake;
        private float initialRot;
        private bool isRotating;
         void Awake();
         void Destroy();
         void FixedUpdate();
        public void ToggleRotation();
        public bool IsRotating();
    }

     void FindAllEntities();
     object FindEntityFromRay(BasePlayer player);
    [ChatCommand("rot")]
     void cmdRot(BasePlayer player, string command, string[] args);
     string msg(string key, string userId);
     Dictionary<string, string> Messages;
    private ConfigData configData;
     class ConfigData
    {
        public float RotationSpeed { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     void SaveData();
     void LoadData();
     class StoredData
    {
        public List<uint> data;
    }

}

 class Rotator : Signage
{
    private Signage entity;
    private float secsToTake;
    private float initialRot;
    private bool isRotating;
     void Awake();
     void Destroy();
     void FixedUpdate();
    public void ToggleRotation();
    public bool IsRotating();
}

 class ConfigData
{
    public float RotationSpeed { get; set; }
}

 class StoredData
{
    public List<uint> data;
}


```

---

## RotatingDeathBags by k1lly0u - Makes dropped item containers float and rotate on death

```csharp
using Rust;
using Oxide.Core;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("RotatingDeathBags", "k1lly0u", "0.1.0", ResourceId = 0)]
 class RotatingDeathBags : RustPlugin
{
    static RotatingDeathBags instance;
    static int[] layerTypes;
    private bool initialized;
     void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
     void Unload();
     class BagRotator : MonoBehaviour
    {
        private DroppedItemContainer entity;
        private Rigidbody rigidBody;
        private bool hasBegun;
        private float secsToTake;
         void Awake();
         void OnDestroy();
         void FixedUpdate();
         void OnCollisionEnter(Collision collision);
        private void BeginRotation();
    }

    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Rotation speed")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Seconds before initiating rotation")]
        public float RotateIn { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
}

 class BagRotator : MonoBehaviour
{
    private DroppedItemContainer entity;
    private Rigidbody rigidBody;
    private bool hasBegun;
    private float secsToTake;
     void Awake();
     void OnDestroy();
     void FixedUpdate();
     void OnCollisionEnter(Collision collision);
    private void BeginRotation();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Rotation speed")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Seconds before initiating rotation")]
    public float RotateIn { get; set; }
}


```

---

## RouletteBroadcast by supreme - Broadcasts via chat the amount of scrap won by a player and rewards him with custom items

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Roulette Broadcast", "supreme", "1.2.0")]
[Description("Broadcasts the payout on the roulette")]
public class RouletteBroadcast : RustPlugin
{
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Amount of scrap to win in order to broadcast")]
        public int amount;
        [JsonProperty(PropertyName = "Broadcast Icon (SteamID)")]
        public ulong chatIcon;
        [JsonProperty(PropertyName = "Enable Broadcast Delay")]
        public bool delay;
        [JsonProperty(PropertyName = "Broadcast Delay Time (seconds)")]
        public int delayTime;
        [JsonProperty(PropertyName = "Gametip Message")]
        public bool gameTip;
        [JsonProperty(PropertyName = "Gametip Message Time")]
        public float gameTipTime;
        [JsonProperty(PropertyName = "Custom Rewards")]
        public bool enableRewards;
        [JsonProperty(PropertyName = "Custom Rewards Message")]
        public bool enableRewardsMessage;
        [JsonProperty(PropertyName = "Custom Rewards Items")]
        public Dictionary<string, int> Items;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void OnItemAddedToContainer(ItemContainer container, Item item);
     string Lang(string key, string id, object[] args);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Amount of scrap to win in order to broadcast")]
    public int amount;
    [JsonProperty(PropertyName = "Broadcast Icon (SteamID)")]
    public ulong chatIcon;
    [JsonProperty(PropertyName = "Enable Broadcast Delay")]
    public bool delay;
    [JsonProperty(PropertyName = "Broadcast Delay Time (seconds)")]
    public int delayTime;
    [JsonProperty(PropertyName = "Gametip Message")]
    public bool gameTip;
    [JsonProperty(PropertyName = "Gametip Message Time")]
    public float gameTipTime;
    [JsonProperty(PropertyName = "Custom Rewards")]
    public bool enableRewards;
    [JsonProperty(PropertyName = "Custom Rewards Message")]
    public bool enableRewardsMessage;
    [JsonProperty(PropertyName = "Custom Rewards Items")]
    public Dictionary<string, int> Items;
}


```

---

## RunawayBoats by 0x89A - Stops boats if driver dismounts

```csharp
using System;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Runaway Boats", "0x89A", "1.2.1")]
[Description("Stops boats from sailing away on dismount")]
 class RunawayBoats : RustPlugin
{
    private const string _canUse;
     void Init();
    private void OnEntityDismounted(BaseMountable mount, BasePlayer player);
    private void StopBoat(MotorRowboat boat);
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Stop if dismounted player is not driver")]
        public bool stopIfNotDriver;
        [JsonProperty("Stop if boat has passengers")]
        public bool stopWithPassengers;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Configuration
{
    [JsonProperty("Stop if dismounted player is not driver")]
    public bool stopIfNotDriver;
    [JsonProperty("Stop if boat has passengers")]
    public bool stopWithPassengers;
}


```

---

## RunningMan by sami37 - Get rewarded for killing runner or just survive as runner

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Random = System.Random;

Oxide.Plugins
[Info("Running Man", "sami37", "1.7.6")]
[Description("Get rewarded for killing runner or just survive as runner")]
 class RunningMan : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin Economics;
    private Plugin Friends;
    private Plugin KarmaSystem;
    private Plugin ServerRewards;
    private Timer _compassRefresh;
    private Timer _countdownRefresh;
    private Timer _eventpause;
    private Timer _eventstart;
    private bool _eventStarted;
    private Timer _ingameTimer;
    private readonly Random _rnd;
    private BasePlayer _runningman;
    private string panelString;
    private string panelCountString;
    private Dictionary<string, Dictionary<string, RewardData>> _savedReward;
    private Timer _stillRunnerTimer;
    private double _LastStartedEvent;
    private double _time2;
    private double _NextEvent;
    private class RewardData
    {
        public Dictionary<string, ValueAmount> RewardItems;
    }

    private class ValueAmount
    {
        public int MaxValue;
        public int MinValue;
    }

    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Default")]
        public Default Info;
        [JsonProperty(PropertyName = "TimeRange")]
        public TimeRangeInfo TimeRange;
        [JsonProperty(PropertyName = "Reward")]
        public Reward RewardInfo;
        [JsonProperty(PropertyName = "CompassUI Info")]
        public CompassUi CompassUiInfo;
        [JsonProperty(PropertyName = "Countdown Info")]
        public CountdownUi CountdownUiInfo;
        public class Default
        {
            [JsonProperty(PropertyName = "ChatName")]
            public string ChatName;
            [JsonProperty(PropertyName = "authLevel")]
            public int AuthLevel;
            [JsonProperty(PropertyName = "AutoStart")]
            public bool AutoStart;
            [JsonProperty(PropertyName = "Display Distance")]
            public bool DisplayDistance;
            [JsonProperty(PropertyName = "Count")]
            public int Count;
            [JsonProperty(PropertyName = "StarteventTime")]
            public int StarteventTime;
            [JsonProperty(PropertyName = "PauseeventTime")]
            public int PauseeventTime;
            [JsonProperty(PropertyName = "DisconnectPendingTimer")]
            public int DisconnectPendingTimer;
            [JsonProperty(PropertyName = "Excluded auth level")]
            public int Excludedauthlevel;
            [JsonProperty(PropertyName = "Block friends kill reward")]
            public bool Blockfriendskillreward;
            [JsonProperty(PropertyName = "Block clans kill reward")]
            public bool Blockclanskillreward;
        }

        public class TimeRangeInfo
        {
            [JsonProperty(PropertyName = "Start War Time")]
            public int StartWarTime;
            [JsonProperty(PropertyName = "End War Time")]
            public int EndWarTime;
            [JsonProperty(PropertyName = "Enable Time Range")]
            public bool TimeRange;
        }

        public class Reward
        {
            [JsonProperty(PropertyName = "Random")]
            public bool Random;
            [JsonProperty(PropertyName = "RewardFixing")]
            public string RewardFixing;
            [JsonProperty(PropertyName = "RewardFixingAmount")]
            public int RewardFixingAmount;
            [JsonProperty(PropertyName = "KarmaSystem")]
            public KarmaSystem KarmaInfo;
            public class KarmaSystem
            {
                [JsonProperty(PropertyName = "PointToRemove")]
                public int PointToRemove;
                [JsonProperty(PropertyName = "PointToAdd")]
                public int PointToAdd;
            }

        }

        public class CompassUi
        {
            [JsonProperty(PropertyName = "AnchorMin")]
            public string AnchorMin;
            [JsonProperty(PropertyName = "AnchorMax")]
            public string AnchorMax;
            [JsonProperty(PropertyName = "Direction")]
            public Direction RunnerDirection;
            public class Direction
            {
                [JsonProperty(PropertyName = "North East")]
                public string NorthEast;
                [JsonProperty(PropertyName = "South East")]
                public string SouthEast;
                [JsonProperty(PropertyName = "North West")]
                public string NorthWest;
                [JsonProperty(PropertyName = "South West")]
                public string SouthWest;
                [JsonProperty(PropertyName = "No runner")]
                public string None;
            }

            [JsonProperty(PropertyName = "Disable while event is off")]
            public bool Disabled;
        }

        public class CountdownUi
        {
            [JsonProperty(PropertyName = "AnchorMin")]
            public string AnchorMin;
            [JsonProperty(PropertyName = "AnchorMax")]
            public string AnchorMax;
            [JsonProperty(PropertyName = "Disable UI while no runner")]
            public bool DisableNoRunner;
            [JsonProperty(PropertyName = "Disable UI while runner on")]
            public bool DisableRunnerOn;
        }

    }

    protected override void LoadConfig();
    private void OnServerInitialized();
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDeath(BasePlayer victim, HitInfo hitinfo);
    private bool HasAccess(BasePlayer player, string permissionName);
    private void CheckTime();
    private void LoadSavedData();
    private void SaveLoadedData();
    private void Startevent(string playerId);
    private void Runningstop();
    private void BroadcastChat(string msg);
    private void Runlog(string text);
    private void SendHelpText(BasePlayer player);
    private void DestroyEvent();
    private void DestroyLeaveEvent();
    [ChatCommand("run")]
    private void CmdRun(BasePlayer player, string cmd, string[] args);
    [ChatCommand("eventon")]
    private void CmdEvent(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("eventon")]
    private void CcmdEvent(ConsoleSystem.Arg arg);
    [ConsoleCommand("eventoff")]
    private void CmdEventOf(ConsoleSystem.Arg arg);
    [ChatCommand("eventoff")]
    private void CmdEventOff(BasePlayer player, string cmd, string[] args);
    [ChatCommand("running")]
    private void CmdChat(BasePlayer player, string cmd, string[] args);
    private void DestroyUi(BasePlayer player);
    private void RefreshUi();
    private void CreateUi(BasePlayer player);
    private string GetDirection(BasePlayer player);
    private void CreateCountdownUi(BasePlayer player);
    private void DestroyCountdownUi(BasePlayer player);
    private void RefreshCountdownUi();
}

private class RewardData
{
    public Dictionary<string, ValueAmount> RewardItems;
}

private class ValueAmount
{
    public int MaxValue;
    public int MinValue;
}

private class Configuration
{
    [JsonProperty(PropertyName = "Default")]
    public Default Info;
    [JsonProperty(PropertyName = "TimeRange")]
    public TimeRangeInfo TimeRange;
    [JsonProperty(PropertyName = "Reward")]
    public Reward RewardInfo;
    [JsonProperty(PropertyName = "CompassUI Info")]
    public CompassUi CompassUiInfo;
    [JsonProperty(PropertyName = "Countdown Info")]
    public CountdownUi CountdownUiInfo;
    public class Default
    {
        [JsonProperty(PropertyName = "ChatName")]
        public string ChatName;
        [JsonProperty(PropertyName = "authLevel")]
        public int AuthLevel;
        [JsonProperty(PropertyName = "AutoStart")]
        public bool AutoStart;
        [JsonProperty(PropertyName = "Display Distance")]
        public bool DisplayDistance;
        [JsonProperty(PropertyName = "Count")]
        public int Count;
        [JsonProperty(PropertyName = "StarteventTime")]
        public int StarteventTime;
        [JsonProperty(PropertyName = "PauseeventTime")]
        public int PauseeventTime;
        [JsonProperty(PropertyName = "DisconnectPendingTimer")]
        public int DisconnectPendingTimer;
        [JsonProperty(PropertyName = "Excluded auth level")]
        public int Excludedauthlevel;
        [JsonProperty(PropertyName = "Block friends kill reward")]
        public bool Blockfriendskillreward;
        [JsonProperty(PropertyName = "Block clans kill reward")]
        public bool Blockclanskillreward;
    }

    public class TimeRangeInfo
    {
        [JsonProperty(PropertyName = "Start War Time")]
        public int StartWarTime;
        [JsonProperty(PropertyName = "End War Time")]
        public int EndWarTime;
        [JsonProperty(PropertyName = "Enable Time Range")]
        public bool TimeRange;
    }

    public class Reward
    {
        [JsonProperty(PropertyName = "Random")]
        public bool Random;
        [JsonProperty(PropertyName = "RewardFixing")]
        public string RewardFixing;
        [JsonProperty(PropertyName = "RewardFixingAmount")]
        public int RewardFixingAmount;
        [JsonProperty(PropertyName = "KarmaSystem")]
        public KarmaSystem KarmaInfo;
        public class KarmaSystem
        {
            [JsonProperty(PropertyName = "PointToRemove")]
            public int PointToRemove;
            [JsonProperty(PropertyName = "PointToAdd")]
            public int PointToAdd;
        }

    }

    public class CompassUi
    {
        [JsonProperty(PropertyName = "AnchorMin")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "AnchorMax")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "Direction")]
        public Direction RunnerDirection;
        public class Direction
        {
            [JsonProperty(PropertyName = "North East")]
            public string NorthEast;
            [JsonProperty(PropertyName = "South East")]
            public string SouthEast;
            [JsonProperty(PropertyName = "North West")]
            public string NorthWest;
            [JsonProperty(PropertyName = "South West")]
            public string SouthWest;
            [JsonProperty(PropertyName = "No runner")]
            public string None;
        }

        [JsonProperty(PropertyName = "Disable while event is off")]
        public bool Disabled;
    }

    public class CountdownUi
    {
        [JsonProperty(PropertyName = "AnchorMin")]
        public string AnchorMin;
        [JsonProperty(PropertyName = "AnchorMax")]
        public string AnchorMax;
        [JsonProperty(PropertyName = "Disable UI while no runner")]
        public bool DisableNoRunner;
        [JsonProperty(PropertyName = "Disable UI while runner on")]
        public bool DisableRunnerOn;
    }

}

public class Default
{
    [JsonProperty(PropertyName = "ChatName")]
    public string ChatName;
    [JsonProperty(PropertyName = "authLevel")]
    public int AuthLevel;
    [JsonProperty(PropertyName = "AutoStart")]
    public bool AutoStart;
    [JsonProperty(PropertyName = "Display Distance")]
    public bool DisplayDistance;
    [JsonProperty(PropertyName = "Count")]
    public int Count;
    [JsonProperty(PropertyName = "StarteventTime")]
    public int StarteventTime;
    [JsonProperty(PropertyName = "PauseeventTime")]
    public int PauseeventTime;
    [JsonProperty(PropertyName = "DisconnectPendingTimer")]
    public int DisconnectPendingTimer;
    [JsonProperty(PropertyName = "Excluded auth level")]
    public int Excludedauthlevel;
    [JsonProperty(PropertyName = "Block friends kill reward")]
    public bool Blockfriendskillreward;
    [JsonProperty(PropertyName = "Block clans kill reward")]
    public bool Blockclanskillreward;
}

public class TimeRangeInfo
{
    [JsonProperty(PropertyName = "Start War Time")]
    public int StartWarTime;
    [JsonProperty(PropertyName = "End War Time")]
    public int EndWarTime;
    [JsonProperty(PropertyName = "Enable Time Range")]
    public bool TimeRange;
}

public class Reward
{
    [JsonProperty(PropertyName = "Random")]
    public bool Random;
    [JsonProperty(PropertyName = "RewardFixing")]
    public string RewardFixing;
    [JsonProperty(PropertyName = "RewardFixingAmount")]
    public int RewardFixingAmount;
    [JsonProperty(PropertyName = "KarmaSystem")]
    public KarmaSystem KarmaInfo;
    public class KarmaSystem
    {
        [JsonProperty(PropertyName = "PointToRemove")]
        public int PointToRemove;
        [JsonProperty(PropertyName = "PointToAdd")]
        public int PointToAdd;
    }

}

public class KarmaSystem
{
    [JsonProperty(PropertyName = "PointToRemove")]
    public int PointToRemove;
    [JsonProperty(PropertyName = "PointToAdd")]
    public int PointToAdd;
}

public class CompassUi
{
    [JsonProperty(PropertyName = "AnchorMin")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "AnchorMax")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "Direction")]
    public Direction RunnerDirection;
    public class Direction
    {
        [JsonProperty(PropertyName = "North East")]
        public string NorthEast;
        [JsonProperty(PropertyName = "South East")]
        public string SouthEast;
        [JsonProperty(PropertyName = "North West")]
        public string NorthWest;
        [JsonProperty(PropertyName = "South West")]
        public string SouthWest;
        [JsonProperty(PropertyName = "No runner")]
        public string None;
    }

    [JsonProperty(PropertyName = "Disable while event is off")]
    public bool Disabled;
}

public class Direction
{
    [JsonProperty(PropertyName = "North East")]
    public string NorthEast;
    [JsonProperty(PropertyName = "South East")]
    public string SouthEast;
    [JsonProperty(PropertyName = "North West")]
    public string NorthWest;
    [JsonProperty(PropertyName = "South West")]
    public string SouthWest;
    [JsonProperty(PropertyName = "No runner")]
    public string None;
}

public class CountdownUi
{
    [JsonProperty(PropertyName = "AnchorMin")]
    public string AnchorMin;
    [JsonProperty(PropertyName = "AnchorMax")]
    public string AnchorMax;
    [JsonProperty(PropertyName = "Disable UI while no runner")]
    public bool DisableNoRunner;
    [JsonProperty(PropertyName = "Disable UI while runner on")]
    public bool DisableRunnerOn;
}


```

---

## RustadminOnline by vfloyd - This plugins enables various features for Rustadmin Online such as:  livemap, players teams,...

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Network;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Rustadmin Online", "misticos", "1.2.0")]
[Description("Extends Rustadmin Online features")]
 class RustadminOnline : CovalencePlugin
{
    private const string CommandPlayerList;
    private Action<ConsoleSystem.Arg> _commandPlayerListGet;
    private void OnServerInitialized();
    private void Unload();
    [Command("rustadmin.run")]
    private void CommandRun(IPlayer player, string command, string[] args);
    [Command("rustadmin.rendermap")]
    private void CommandRenderMap(IPlayer player, string command, string[] args);
    private void OnPlayerList(ConsoleSystem.Arg arg);
    private int shrinkTeamId(ulong id);
    private ConsoleSystem.Command FindCommand(string fullName);
    private void BuildCommands();
}


```

---

## RustAppLite by RustApp - A simplified RustApp plugin, for getting reports in Discord.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Globalization;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System;
using Oxide.Game.Rust.Libraries;

Oxide.Plugins
[Info("RustApp Lite", "RustApp", "1.0.8")]
[Description("Get reports on players in Discord, using a nicely designed interface or F7")]
public class RustAppLite : RustPlugin
{
    private class Configuration
    {
        [JsonProperty("[UI] Chat commands")]
        public List<string> report_ui_commands;
        [JsonProperty("[UI] Report reasons")]
        public List<string> report_ui_reasons;
        [JsonProperty("[UI] Cooldown between reports (seconds)")]
        public int report_ui_cooldown;
        [JsonProperty("[UI] Auto-parse reports from F7 (ingame reports)")]
        public bool report_ui_auto_parse;
        [JsonProperty("[Discord] Webhook to send reports")]
        public string discord_webhook;
        [JsonProperty("[Discord-Translations] Nickname field")]
        public string discord_translations_nickname;
        [JsonProperty("[Discord-Translations] Reason field")]
        public string discord_translations_reason;
        [JsonProperty("[Discord-Translations] Comment field")]
        public string discord_translations_comment;
        [JsonProperty("[Discord-Translations] Report sent text")]
        public string discord_translations_report_sent;
        public static Configuration Generate();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public class DiscordMessage
    {
        public string content { get; set; }
        public DiscordEmbed[] embeds { get; set; }
        public DiscordMessage(string Content, DiscordEmbed[] Embeds);
        public void Send(string url);
        public static Dictionary<string, string> _Headeers;
    }

    public class DiscordEmbed
    {
        public string title { get; set; }
        public string description { get; set; }
        public int? color { get; set; }
        public DiscordField[] fields { get; set; }
        public DiscordFooter footer { get; set; }
        public DiscordAuthor author { get; set; }
        public DiscordEmbed(string Title, string Description, int? Color, DiscordField[] Fields, DiscordFooter Footer, DiscordAuthor Author);
    }

    public class DiscordFooter
    {
        public string text { get; set; }
        public string icon_url { get; set; }
        public string proxy_icon_url { get; set; }
        public DiscordFooter(string Text, string Icon_url, string Proxy_icon_url);
    }

    public class DiscordAuthor
    {
        public string name { get; set; }
        public string url { get; set; }
        public string icon_url { get; set; }
        public string proxy_icon_url { get; set; }
        public DiscordAuthor(string Name, string Url, string Icon_url, string Proxy_icon_url);
    }

    public class DiscordField
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public DiscordField(string Name, string Value, bool Inline);
    }

    private static string ReportLayer;
    private void DrawReportInterface(BasePlayer player, int page, string search, bool redraw, BasePlayer preselect);
    private static string HexToRustFormat(string hex);
    private static RustAppLite _RustAppLite;
    private static Configuration _Settings;
    private Dictionary<ulong, double> _Cooldowns;
    private void OnServerInitialized();
    private void WriteLiteMarker();
    protected override void LoadDefaultMessages();
    private void Unload();
    private void OnPlayerReported(BasePlayer reporter, string targetName, string targetId, string subject, string message, string type);
    [ConsoleCommand("RAL_CommandHandler")]
    private void CmdConsoleReportPanel(ConsoleSystem.Arg args);
    private void ChatCmdReport(BasePlayer player, string command, string[] args);
    private void SoundInfoToast(BasePlayer player, string text);
    private void SoundErrorToast(BasePlayer player, string text);
    private void SoundToast(BasePlayer player, string text, int type);
    private void RA_ReportSend(string initiator_steam_id, string target_steam_id, string reason, string message);
    private double CurrentTime();
    private static List<char> Letters;
    private static string NormalizeString(string text);
    private long getUnixTime();
    public void Log(string ru, string en);
    public void Warning(string ru, string en);
    public void Error(string ru, string en);
}

private class Configuration
{
    [JsonProperty("[UI] Chat commands")]
    public List<string> report_ui_commands;
    [JsonProperty("[UI] Report reasons")]
    public List<string> report_ui_reasons;
    [JsonProperty("[UI] Cooldown between reports (seconds)")]
    public int report_ui_cooldown;
    [JsonProperty("[UI] Auto-parse reports from F7 (ingame reports)")]
    public bool report_ui_auto_parse;
    [JsonProperty("[Discord] Webhook to send reports")]
    public string discord_webhook;
    [JsonProperty("[Discord-Translations] Nickname field")]
    public string discord_translations_nickname;
    [JsonProperty("[Discord-Translations] Reason field")]
    public string discord_translations_reason;
    [JsonProperty("[Discord-Translations] Comment field")]
    public string discord_translations_comment;
    [JsonProperty("[Discord-Translations] Report sent text")]
    public string discord_translations_report_sent;
    public static Configuration Generate();
}

public class DiscordMessage
{
    public string content { get; set; }
    public DiscordEmbed[] embeds { get; set; }
    public DiscordMessage(string Content, DiscordEmbed[] Embeds);
    public void Send(string url);
    public static Dictionary<string, string> _Headeers;
}

public class DiscordEmbed
{
    public string title { get; set; }
    public string description { get; set; }
    public int? color { get; set; }
    public DiscordField[] fields { get; set; }
    public DiscordFooter footer { get; set; }
    public DiscordAuthor author { get; set; }
    public DiscordEmbed(string Title, string Description, int? Color, DiscordField[] Fields, DiscordFooter Footer, DiscordAuthor Author);
}

public class DiscordFooter
{
    public string text { get; set; }
    public string icon_url { get; set; }
    public string proxy_icon_url { get; set; }
    public DiscordFooter(string Text, string Icon_url, string Proxy_icon_url);
}

public class DiscordAuthor
{
    public string name { get; set; }
    public string url { get; set; }
    public string icon_url { get; set; }
    public string proxy_icon_url { get; set; }
    public DiscordAuthor(string Name, string Url, string Icon_url, string Proxy_icon_url);
}

public class DiscordField
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public DiscordField(string Name, string Value, bool Inline);
}


```

---

## Rustcord by OuTSMoKE - Complete Discord server monitoring for Rust

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Net;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core.Plugins;
using UnityEngine;
using Facepunch;
using Oxide.Core.Libraries.Covalence;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Clients;
using Oxide.Ext.Discord.Connections;
using Oxide.Ext.Discord.Constants;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Interfaces;
using Oxide.Ext.Discord.Logging;

Oxide.Plugins
[Info("Rustcord", "Kirollos & OuTSMoKE", "3.4.1")]
[Description("Complete game server monitoring through discord.")]
internal class Rustcord : RustPlugin, IDiscordPlugin
{
    [PluginReference]
     Plugin PrivateMessages;
     Plugin BetterChatMute;
     Plugin Clans;
     Plugin AdminChat;
     Plugin DiscordAuth;
     Plugin AdminHammer;
     Plugin AdminRadar;
     Plugin Kits;
     Plugin Vanish;
     Plugin RaidableBases;
     Plugin DangerousTreasures;
     Plugin NoGiveNotices;
     Plugin Give;
     Plugin AirEvent;
     Plugin HarborEvent;
     Plugin JunkyardEvent;
     Plugin PowerPlantEvent;
    public DiscordClient Client { get; set; }
    private Settings _settings;
    private int? _channelCount;
    private Snowflake _botId;
    private UpdatePresenceCommand DiscordPresence;
    private Timer StatusTimer;
    private object FindUserByID(DiscordUser user);
    private static string FormatTime(TimeSpan time);
    private string GetPlayerFormattedField(IPlayer player);
    private string GetFormattedSteamID(string id);
    private string FindGridPosition(Vector3 position);
     Dictionary<CacheType, Dictionary<BasePlayer, Dictionary<string, string>>> cache;
    private string rbdiff;
     Dictionary<string, string> GetPlayerCache(BasePlayer player, string message, CacheType type);
    private void OnDiscordClientCreated();
    private void Reload();
     void OnDiscordGatewayReady(GatewayReadyEvent rdy);
     void OnDiscordGuildCreated(DiscordGuild newguild);
    private void Unload();
    [HookMethod(DiscordExtHooks.OnDiscordGuildMessageCreated)]
    private void OnDiscordGuildMessageCreated(DiscordMessage message);
    private string Translate(string msg, Dictionary<string, string> parameters);
    private Settings.Channel FindChannelById(Snowflake id);
    private void GetChannel(DiscordClient c, Snowflake chan_id, Action<DiscordChannel> cb, Snowflake guildid);
    private string GetRoleNameById(Snowflake id);
    private IPlayer FindPlayer(string nameorId);
    private DiscordUser FindUserByID(Snowflake id);
    private BasePlayer FindPlayerByID(string Id);
    private IPlayer GetPlayer(string id);
    private IPlayer GetPlayer(ulong id);
    private class Settings
    {
        [JsonProperty(PropertyName = "General Settings")]
        public GeneralSettings General { get; set; }
        [JsonProperty(PropertyName = "Discord to Game Settings")]
        public DiscordSideSettings DiscordSide { get; set; }
        [JsonProperty(PropertyName = "Rust Logging Settings")]
        public GameLogSettings GameLog { get; set; }
        [JsonProperty(PropertyName = "Plugin Logging Settings")]
        public PluginLogSettings PluginLog { get; set; }
        [JsonProperty(PropertyName = "Premium Plugin Logging Settings")]
        public PremiumPluginLogSettings PremiumPluginLog { get; set; }
        [JsonProperty(PropertyName = "Discord Output Formatting")]
        public OutputSettings OutputFormat { get; set; }
        [JsonProperty(PropertyName = "Logging Exclusions")]
        public ExcludedSettings Excluded { get; set; }
        [JsonProperty(PropertyName = "Filter Settings")]
        public FilterSettings Filters { get; set; }
        [JsonProperty(PropertyName = "Discord Logging Channels")]
        public List<Channel> Channels { get; set; }
        [JsonProperty(PropertyName = "Discord Command Role Assignment (Empty = All roles can use command.)")]
        public Dictionary<string, List<string>> Commandroles { get; set; }
        public class Channel
        {
            [JsonProperty(PropertyName = "Discord Channel ID #")]
            public Snowflake Channelid { get; set; }
            [JsonProperty(PropertyName = "Channel Flags")]
            public List<string> perms { get; set; }
            [JsonProperty(PropertyName = "Custom: Words/Phrases to Log")]
            public List<string> CustomFilter { get; set; }
            [JsonIgnore]
            public readonly StringBuilder FilterBuilder;
            [JsonIgnore]
            public Timer Timer;
        }

    }

    public class GeneralSettings
    {
        [JsonProperty(PropertyName = "API Key (Bot Token)")]
        public string Apikey { get; set; }
        [JsonProperty(PropertyName = "Auto Reload Plugin")]
        public bool AutoReloadPlugin { get; set; }
        [JsonProperty(PropertyName = "Auto Reload Time (Seconds)")]
        public int AutoReloadTime { get; set; }
        [JsonProperty(PropertyName = "Enable Bot Status")]
        public bool EnableBotStatus { get; set; }
        [JsonProperty(PropertyName = "In-Game Report Command")]
        public string ReportCommand { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(DiscordLogLevel.Info)]
        [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose/Debug/Info/Warning/Error/Exception/Off)")]
        public DiscordLogLevel ExtensionDebugging { get; set; }
    }

    public class DiscordSideSettings
    {
        [JsonProperty(PropertyName = "Discord Command Prefix")]
        public string Commandprefix { get; set; }
        [JsonProperty(PropertyName = "Discord to Game Chat: Icon (Steam ID)")]
        public ulong GameChatIconSteamID { get; set; }
        [JsonProperty(PropertyName = "Discord to Game Chat: Tag")]
        public string GameChatTag { get; set; }
        [JsonProperty(PropertyName = "Discord to Game Chat: Tag Color (Hex)")]
        public string GameChatTagColor { get; set; }
        [JsonProperty(PropertyName = "Discord to Game Chat: Player Name Color (Hex)")]
        public string GameChatNameColor { get; set; }
        [JsonProperty(PropertyName = "Discord to Game Chat: Message Color (Hex)")]
        public string GameChatTextColor { get; set; }
    }

    public class GameLogSettings
    {
        [JsonProperty(PropertyName = "Enable Logging: Player Chat")]
        public bool LogChat { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Joins & Quits")]
        public bool LogJoinQuits { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Deaths")]
        public bool LogDeaths { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Vehicle Spawns (Heli/APC/Plane/Ship)")]
        public bool LogVehicleSpawns { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Crate Drops (Hackable/Supply)")]
        public bool LogCrateDrops { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Usergroup Changes")]
        public bool LogUserGroups { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Permission Changes")]
        public bool LogPermissions { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Kicks & Bans")]
        public bool LogKickBans { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Player Name Changes")]
        public bool LogNameChanges { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Server Commands (Gestures/Note Edits)")]
        public bool LogServerCommands { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Server Messages (Give/Item Spawns)")]
        public bool LogServerMessages { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Player F7 Reports")]
        public bool LogF7Reports { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Team Changes")]
        public bool LogTeams { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: RCON Connections")]
        public bool LogRCON { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Spectates")]
        public bool LogSpectates { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Server Wipe")]
        public bool LogServerWipe { get; set; }
        [JsonProperty(PropertyName = "Enable Custom Logging")]
        public bool EnableCustomLogging { get; set; }
    }

    public class PluginLogSettings
    {
        [JsonProperty(PropertyName = "Enable Logging: AdminHammer")]
        public bool LogPluginAdminHammer { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Admin Radar")]
        public bool LogPluginAdminRadar { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Better Chat Mute")]
        public bool LogPluginBetterChatMute { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Clans")]
        public bool LogPluginClans { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Dangerous Treasures")]
        public bool LogPluginDangerousTreasures { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Discord Auth")]
        public bool LogPluginDiscordAuth { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Godmode")]
        public bool LogPluginGodmode { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Kits")]
        public bool LogPluginKits { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Private Messages")]
        public bool LogPluginPrivateMessages { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Raidable Bases")]
        public bool LogPluginRaidableBases { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Sign Artist")]
        public bool LogPluginSignArtist { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Vanish")]
        public bool LogPluginVanish { get; set; }
    }

    public class PremiumPluginLogSettings
    {
        [JsonProperty(PropertyName = "Enable Logging: Air Event")]
        public bool LogPluginAirEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Armored Train Event")]
        public bool LogPluginArmoredTrainEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Cargo Train Event")]
        public bool LogPluginCargoTrainEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Convoy Event")]
        public bool LogPluginConvoyEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Harbor Event")]
        public bool LogPluginHarborEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Junkyard Event")]
        public bool LogPluginJunkyardEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Power Plant Event")]
        public bool LogPluginPowerPlantEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Satellite Dish Event")]
        public bool LogPluginSatDishEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Sputnik Event")]
        public bool LogPluginSputnikEvent { get; set; }
        [JsonProperty(PropertyName = "Enable Logging: Water Event")]
        public bool LogPluginWaterEvent { get; set; }
    }

    public class OutputSettings
    {
        [JsonProperty(PropertyName = "Output Type: Bans (Simple/Embed)")]
        public string OutputTypeBans { get; set; }
        [JsonProperty(PropertyName = "Output Type: Bug Report (Simple/Embed)")]
        public string OutputTypeBugs { get; set; }
        [JsonProperty(PropertyName = "Output Type: Deaths (Simple/Embed/DeathNotes)")]
        public string OutputTypeDeaths { get; set; }
        [JsonProperty(PropertyName = "Output Type: F7 Reports (Simple/Embed)")]
        public string OutputTypeF7Report { get; set; }
        [JsonProperty(PropertyName = "Output Type: Join/Quit (Simple/Embed)")]
        public string OutputTypeJoinQuit { get; set; }
        [JsonProperty(PropertyName = "Output Type: Join Player Info (Admin Channel) (Simple/Embed)")]
        public string OutputTypeJoinAdminChan { get; set; }
        [JsonProperty(PropertyName = "Output Type: Kicks (Simple/Embed)")]
        public string OutputTypeKicks { get; set; }
        [JsonProperty(PropertyName = "Output Type: Note Logging (Simple/Embed)")]
        public string OutputTypeNoteLog { get; set; }
        [JsonProperty(PropertyName = "Output Type: Player Name Change (Simple/Embed)")]
        public string OutputTypeNameChange { get; set; }
        [JsonProperty(PropertyName = "Output Type: /Report (Simple/Embed)")]
        public string OutputTypeReports { get; set; }
        [JsonProperty(PropertyName = "Output Type: Server Wipe (Simple/Embed)")]
        public string OutputTypeServerWipe { get; set; }
        [JsonProperty(PropertyName = "Output Type: Teams (Simple/Embed)")]
        public string OutputTypeTeams { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Admin Hammer (Simple/Embed)")]
        public string OutputTypeAdminHammer { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Admin Radar (Simple/Embed)")]
        public string OutputTypeAdminRadar { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Better Chat Mute (Simple/Embed)")]
        public string OutputTypeBetterChatMute { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Clans (Simple/Embed)")]
        public string OutputTypeClans { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Dangerous Treasures (Simple/Embed)")]
        public string OutputTypeDangerousTreasures { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Discord Auth (Simple/Embed)")]
        public string OutputTypeDiscordAuth { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Godmode (Simple/Embed)")]
        public string OutputTypeGodmode { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Kits (Simple/Embed)")]
        public string OutputTypeKits { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Private Messages (Simple/Embed)")]
        public string OutputTypePMs { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Raidable Bases (Simple/Embed)")]
        public string OutputTypeRaidableBases { get; set; }
        [JsonProperty(PropertyName = "Output Type (Plugin): Vanish (Simple/Embed)")]
        public string OutputTypeVanish { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): AirEvent (Simple/Embed)")]
        public string OutputTypeAirEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): ArmoredTrainEvent (Simple/Embed)")]
        public string OutputTypeArmoredTrainEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): CargoTrainEvent (Simple/Embed)")]
        public string OutputTypeCargoTrainEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): ConvoyEvent (Simple/Embed)")]
        public string OutputTypeConvoyEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): HarborEvent (Simple/Embed)")]
        public string OutputTypeHarborEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): JunkyardEvent (Simple/Embed)")]
        public string OutputTypeJunkyardEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): PowerPlantEvent (Simple/Embed)")]
        public string OutputTypePowerPlantEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): SatDishEvent (Simple/Embed)")]
        public string OutputTypeSatDishEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): SputnikEvent (Simple/Embed)")]
        public string OutputTypeSputnikEvent { get; set; }
        [JsonProperty(PropertyName = "Output Type (Premium Plugin): WaterEvent (Simple/Embed)")]
        public string OutputTypeWaterEvent { get; set; }
    }

    public class ExcludedSettings
    {
        [JsonProperty(PropertyName = "Exclude Listed Groups From log_groups")]
        public List<string> LogExcludeGroups { get; set; }
        [JsonProperty(PropertyName = "Exclude Listed Permissions From log_perms")]
        public List<string> LogExcludePerms { get; set; }
    }

    public class FilterSettings
    {
        [JsonProperty(PropertyName = "Chat Filter: Replacement Word")]
        public string FilteredWord { get; set; }
        [JsonProperty(PropertyName = "Chat Filter: Words to Filter")]
        public List<string> FilterWords { get; set; }
    }

    private Settings GetDefaultSettings();
    protected override void LoadDefaultConfig();
     void SubscribeHooks();
    private void Init();
     void UnsubscribeHooks();
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void DiscordToGameCmd(string command, string param, DiscordUser author, Snowflake channelid);
     void cmdReport(BasePlayer player, string command, string[] args);
     void cmdBug(BasePlayer player, string command, string[] args);
    private void OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
     void OnCrateDropped(HackableLockedCrate crate);
     void OnSupplyDropLanded(SupplyDrop entity);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnGroupCreated(string name);
     void OnGroupDeleted(string name);
     void OnUserGroupAdded(string id, string groupName);
     void OnUserGroupRemoved(string id, string groupName);
    private void OnPlayerConnected(BasePlayer player);
    private void HandleAdminJoin(BasePlayer player);
    private void HandlePlayerJoin(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnUserPermissionGranted(string id, string permName);
     void OnGroupPermissionGranted(string name, string perm);
     void OnUserPermissionRevoked(string id, string permName);
     void OnGroupPermissionRevoked(string name, string perm);
     void OnUserKicked(IPlayer player, string reason);
     void OnUserBanned(string name, string bannedId, string address, string reason);
    private void OnUserUnbanned(string name, string id, string ip);
     void OnUserNameUpdated(string id, string oldName, string newName);
    private void OnServerInitialized(bool isStartup);
     void OnServerShutdown();
    private object OnServerMessage(string message, string name);
    private void OnNewSave(string filename);
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private readonly Dictionary<string, string> _emotes;
     void OnPlayerReported(BasePlayer reporter, string targetName, string targetId, string subject, string message, string type);
    private void ConsoleLog(string condition, string stackTrace, LogType type);
    private void OnRconConnection(IPAddress ip);
    private void OnPlayerSpectate(BasePlayer player, string spectateFilter);
    private void OnPlayerSpectateEnd(BasePlayer player, string spectateFilter);
     void OnTeamCreated(BasePlayer player, RelationshipManager.PlayerTeam team);
     void OnTeamAcceptInvite(RelationshipManager.PlayerTeam team, BasePlayer player);
     void OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
     void OnTeamKick(RelationshipManager.PlayerTeam team, BasePlayer player, ulong target);
     void OnTeamDisbanded(RelationshipManager.PlayerTeam team);
    private void OnEntitySpawned(BaseEntity Entity);
     void OnAdminHammerEnabled(BasePlayer player);
     void OnAdminHammerDisabled(BasePlayer player);
     void OnRadarActivated(BasePlayer player);
     void OnRadarDeactivated(BasePlayer player);
    private void OnBetterChatMuted(IPlayer target, IPlayer player, string reason);
    private void OnBetterChatTimeMuted(IPlayer target, IPlayer player, TimeSpan time, string reason);
    private void OnBetterChatUnmuted(IPlayer target, IPlayer player);
    private void OnBetterChatMuteExpired(IPlayer target);
     void OnClanCreate(string tag, string ownerID);
     void OnClanDisbanded(string tag, List<string> memberUserIDs);
     void OnClanChat(IPlayer player, string message);
    private void OnDangerousEventStarted(Vector3 containerPos);
    private void OnDangerousEventEnded(Vector3 containerPos);
    private void OnDeathNotice(Dictionary<string, object> data, string message);
    private void OnDiscordPlayerLinked(IPlayer player, DiscordUser user);
    private void OnDiscordPlayerUnlinked(IPlayer player, DiscordUser user);
    private void OnGodmodeToggled(string playerId, bool enabled);
     void OnKitRedeemed(BasePlayer player, string kitName);
    [HookMethod("OnPMProcessed")]
     void OnPMProcessed(IPlayer sender, IPlayer target, string message);
     void OnRaidableBaseStarted(Vector3 pos, int difficulty);
     void OnRaidableBaseEnded(Vector3 pos, int difficulty);
    private void OnImagePost(BasePlayer player, string image);
    private DiscordEmbed SignArtistEmbed(string text, string image);
     void OnVanishDisappear(BasePlayer player);
     void OnVanishReappear(BasePlayer player);
     void OnAirEventStart(HashSet<BaseEntity> entities);
     void OnAirEventEnd(HashSet<BaseEntity> entities);
     void OnArmoredTrainEventStart();
     void OnArmoredTrainEventStop();
     void OnTrainEventStarted(TrainEngine train);
     void OnTrainEventEnded(TrainEngine train);
     void OnConvoyStart();
     void OnConvoyStop();
     void OnHarborEventStart();
     void OnHarborEventEnd(HashSet<BaseEntity> entities);
     void OnJunkyardEventStart();
     void OnJunkyardEventEnd(HashSet<BaseEntity> entities);
     void OnPowerPlantEventStart();
     void OnPowerPlantEventEnd(HashSet<BaseEntity> entities);
     void OnSatDishEventStart();
     void OnSatDishEventEnd(HashSet<BaseEntity> entities);
     void OnSputnikEventStart();
     void OnSputnikEventStop();
     void OnWaterEventStart();
     void OnWaterEventEnd(HashSet<BaseEntity> entities);
}

private class Settings
{
    [JsonProperty(PropertyName = "General Settings")]
    public GeneralSettings General { get; set; }
    [JsonProperty(PropertyName = "Discord to Game Settings")]
    public DiscordSideSettings DiscordSide { get; set; }
    [JsonProperty(PropertyName = "Rust Logging Settings")]
    public GameLogSettings GameLog { get; set; }
    [JsonProperty(PropertyName = "Plugin Logging Settings")]
    public PluginLogSettings PluginLog { get; set; }
    [JsonProperty(PropertyName = "Premium Plugin Logging Settings")]
    public PremiumPluginLogSettings PremiumPluginLog { get; set; }
    [JsonProperty(PropertyName = "Discord Output Formatting")]
    public OutputSettings OutputFormat { get; set; }
    [JsonProperty(PropertyName = "Logging Exclusions")]
    public ExcludedSettings Excluded { get; set; }
    [JsonProperty(PropertyName = "Filter Settings")]
    public FilterSettings Filters { get; set; }
    [JsonProperty(PropertyName = "Discord Logging Channels")]
    public List<Channel> Channels { get; set; }
    [JsonProperty(PropertyName = "Discord Command Role Assignment (Empty = All roles can use command.)")]
    public Dictionary<string, List<string>> Commandroles { get; set; }
    public class Channel
    {
        [JsonProperty(PropertyName = "Discord Channel ID #")]
        public Snowflake Channelid { get; set; }
        [JsonProperty(PropertyName = "Channel Flags")]
        public List<string> perms { get; set; }
        [JsonProperty(PropertyName = "Custom: Words/Phrases to Log")]
        public List<string> CustomFilter { get; set; }
        [JsonIgnore]
        public readonly StringBuilder FilterBuilder;
        [JsonIgnore]
        public Timer Timer;
    }

}

public class Channel
{
    [JsonProperty(PropertyName = "Discord Channel ID #")]
    public Snowflake Channelid { get; set; }
    [JsonProperty(PropertyName = "Channel Flags")]
    public List<string> perms { get; set; }
    [JsonProperty(PropertyName = "Custom: Words/Phrases to Log")]
    public List<string> CustomFilter { get; set; }
    [JsonIgnore]
    public readonly StringBuilder FilterBuilder;
    [JsonIgnore]
    public Timer Timer;
}

public class GeneralSettings
{
    [JsonProperty(PropertyName = "API Key (Bot Token)")]
    public string Apikey { get; set; }
    [JsonProperty(PropertyName = "Auto Reload Plugin")]
    public bool AutoReloadPlugin { get; set; }
    [JsonProperty(PropertyName = "Auto Reload Time (Seconds)")]
    public int AutoReloadTime { get; set; }
    [JsonProperty(PropertyName = "Enable Bot Status")]
    public bool EnableBotStatus { get; set; }
    [JsonProperty(PropertyName = "In-Game Report Command")]
    public string ReportCommand { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(DiscordLogLevel.Info)]
    [JsonProperty(PropertyName = "Discord Extension Log Level (Verbose/Debug/Info/Warning/Error/Exception/Off)")]
    public DiscordLogLevel ExtensionDebugging { get; set; }
}

public class DiscordSideSettings
{
    [JsonProperty(PropertyName = "Discord Command Prefix")]
    public string Commandprefix { get; set; }
    [JsonProperty(PropertyName = "Discord to Game Chat: Icon (Steam ID)")]
    public ulong GameChatIconSteamID { get; set; }
    [JsonProperty(PropertyName = "Discord to Game Chat: Tag")]
    public string GameChatTag { get; set; }
    [JsonProperty(PropertyName = "Discord to Game Chat: Tag Color (Hex)")]
    public string GameChatTagColor { get; set; }
    [JsonProperty(PropertyName = "Discord to Game Chat: Player Name Color (Hex)")]
    public string GameChatNameColor { get; set; }
    [JsonProperty(PropertyName = "Discord to Game Chat: Message Color (Hex)")]
    public string GameChatTextColor { get; set; }
}

public class GameLogSettings
{
    [JsonProperty(PropertyName = "Enable Logging: Player Chat")]
    public bool LogChat { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Joins & Quits")]
    public bool LogJoinQuits { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Deaths")]
    public bool LogDeaths { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Vehicle Spawns (Heli/APC/Plane/Ship)")]
    public bool LogVehicleSpawns { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Crate Drops (Hackable/Supply)")]
    public bool LogCrateDrops { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Usergroup Changes")]
    public bool LogUserGroups { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Permission Changes")]
    public bool LogPermissions { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Kicks & Bans")]
    public bool LogKickBans { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Player Name Changes")]
    public bool LogNameChanges { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Server Commands (Gestures/Note Edits)")]
    public bool LogServerCommands { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Server Messages (Give/Item Spawns)")]
    public bool LogServerMessages { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Player F7 Reports")]
    public bool LogF7Reports { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Team Changes")]
    public bool LogTeams { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: RCON Connections")]
    public bool LogRCON { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Spectates")]
    public bool LogSpectates { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Server Wipe")]
    public bool LogServerWipe { get; set; }
    [JsonProperty(PropertyName = "Enable Custom Logging")]
    public bool EnableCustomLogging { get; set; }
}

public class PluginLogSettings
{
    [JsonProperty(PropertyName = "Enable Logging: AdminHammer")]
    public bool LogPluginAdminHammer { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Admin Radar")]
    public bool LogPluginAdminRadar { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Better Chat Mute")]
    public bool LogPluginBetterChatMute { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Clans")]
    public bool LogPluginClans { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Dangerous Treasures")]
    public bool LogPluginDangerousTreasures { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Discord Auth")]
    public bool LogPluginDiscordAuth { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Godmode")]
    public bool LogPluginGodmode { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Kits")]
    public bool LogPluginKits { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Private Messages")]
    public bool LogPluginPrivateMessages { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Raidable Bases")]
    public bool LogPluginRaidableBases { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Sign Artist")]
    public bool LogPluginSignArtist { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Vanish")]
    public bool LogPluginVanish { get; set; }
}

public class PremiumPluginLogSettings
{
    [JsonProperty(PropertyName = "Enable Logging: Air Event")]
    public bool LogPluginAirEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Armored Train Event")]
    public bool LogPluginArmoredTrainEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Cargo Train Event")]
    public bool LogPluginCargoTrainEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Convoy Event")]
    public bool LogPluginConvoyEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Harbor Event")]
    public bool LogPluginHarborEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Junkyard Event")]
    public bool LogPluginJunkyardEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Power Plant Event")]
    public bool LogPluginPowerPlantEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Satellite Dish Event")]
    public bool LogPluginSatDishEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Sputnik Event")]
    public bool LogPluginSputnikEvent { get; set; }
    [JsonProperty(PropertyName = "Enable Logging: Water Event")]
    public bool LogPluginWaterEvent { get; set; }
}

public class OutputSettings
{
    [JsonProperty(PropertyName = "Output Type: Bans (Simple/Embed)")]
    public string OutputTypeBans { get; set; }
    [JsonProperty(PropertyName = "Output Type: Bug Report (Simple/Embed)")]
    public string OutputTypeBugs { get; set; }
    [JsonProperty(PropertyName = "Output Type: Deaths (Simple/Embed/DeathNotes)")]
    public string OutputTypeDeaths { get; set; }
    [JsonProperty(PropertyName = "Output Type: F7 Reports (Simple/Embed)")]
    public string OutputTypeF7Report { get; set; }
    [JsonProperty(PropertyName = "Output Type: Join/Quit (Simple/Embed)")]
    public string OutputTypeJoinQuit { get; set; }
    [JsonProperty(PropertyName = "Output Type: Join Player Info (Admin Channel) (Simple/Embed)")]
    public string OutputTypeJoinAdminChan { get; set; }
    [JsonProperty(PropertyName = "Output Type: Kicks (Simple/Embed)")]
    public string OutputTypeKicks { get; set; }
    [JsonProperty(PropertyName = "Output Type: Note Logging (Simple/Embed)")]
    public string OutputTypeNoteLog { get; set; }
    [JsonProperty(PropertyName = "Output Type: Player Name Change (Simple/Embed)")]
    public string OutputTypeNameChange { get; set; }
    [JsonProperty(PropertyName = "Output Type: /Report (Simple/Embed)")]
    public string OutputTypeReports { get; set; }
    [JsonProperty(PropertyName = "Output Type: Server Wipe (Simple/Embed)")]
    public string OutputTypeServerWipe { get; set; }
    [JsonProperty(PropertyName = "Output Type: Teams (Simple/Embed)")]
    public string OutputTypeTeams { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Admin Hammer (Simple/Embed)")]
    public string OutputTypeAdminHammer { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Admin Radar (Simple/Embed)")]
    public string OutputTypeAdminRadar { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Better Chat Mute (Simple/Embed)")]
    public string OutputTypeBetterChatMute { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Clans (Simple/Embed)")]
    public string OutputTypeClans { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Dangerous Treasures (Simple/Embed)")]
    public string OutputTypeDangerousTreasures { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Discord Auth (Simple/Embed)")]
    public string OutputTypeDiscordAuth { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Godmode (Simple/Embed)")]
    public string OutputTypeGodmode { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Kits (Simple/Embed)")]
    public string OutputTypeKits { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Private Messages (Simple/Embed)")]
    public string OutputTypePMs { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Raidable Bases (Simple/Embed)")]
    public string OutputTypeRaidableBases { get; set; }
    [JsonProperty(PropertyName = "Output Type (Plugin): Vanish (Simple/Embed)")]
    public string OutputTypeVanish { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): AirEvent (Simple/Embed)")]
    public string OutputTypeAirEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): ArmoredTrainEvent (Simple/Embed)")]
    public string OutputTypeArmoredTrainEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): CargoTrainEvent (Simple/Embed)")]
    public string OutputTypeCargoTrainEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): ConvoyEvent (Simple/Embed)")]
    public string OutputTypeConvoyEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): HarborEvent (Simple/Embed)")]
    public string OutputTypeHarborEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): JunkyardEvent (Simple/Embed)")]
    public string OutputTypeJunkyardEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): PowerPlantEvent (Simple/Embed)")]
    public string OutputTypePowerPlantEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): SatDishEvent (Simple/Embed)")]
    public string OutputTypeSatDishEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): SputnikEvent (Simple/Embed)")]
    public string OutputTypeSputnikEvent { get; set; }
    [JsonProperty(PropertyName = "Output Type (Premium Plugin): WaterEvent (Simple/Embed)")]
    public string OutputTypeWaterEvent { get; set; }
}

public class ExcludedSettings
{
    [JsonProperty(PropertyName = "Exclude Listed Groups From log_groups")]
    public List<string> LogExcludeGroups { get; set; }
    [JsonProperty(PropertyName = "Exclude Listed Permissions From log_perms")]
    public List<string> LogExcludePerms { get; set; }
}

public class FilterSettings
{
    [JsonProperty(PropertyName = "Chat Filter: Replacement Word")]
    public string FilteredWord { get; set; }
    [JsonProperty(PropertyName = "Chat Filter: Words to Filter")]
    public List<string> FilterWords { get; set; }
}


```

---

## RustGameBanCheck by RogderDodger - Checks against GameBanDb for previous temporary rust bans and alerts via discord

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info("Rust Gameban Check", "Rogder Dodger", "1.1.2"),
     Description("Checks against GameBanDb for previous rust bans and alerts via discord")]
public class RustGameBanCheck : RustPlugin
{
    private const string DefaultWebhookURL;
    private const int WebTimeoutThreshold;
    private const string GameBanDbUrl;
    private const string DefaultBanMessage;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Log To Console")]
        public bool LogToConsole;
        [JsonProperty(PropertyName = "Log To Discord")]
        public bool LogToDiscord;
        [JsonProperty(PropertyName = "Discord Webhook URL")]
        public string DiscordWebhookUrl;
        [JsonProperty(PropertyName = "Whitelist Steam IDs")]
        public List<ulong> WhitelistedSteamIds;
        [JsonProperty(PropertyName = "Automatically Ban Players with previous Rust Bans")]
        public bool AutomaticallyBan;
        [JsonProperty(PropertyName = "Ban Message")]
        public string BanMessage;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private string FormatMessage(string key, BasePlayer player, GameBannedDbResponse gameBanDetails);
    private class GameBannedDbResponse
    {
        [JsonProperty("TweetLink")]
        public string? TweetLink { get; set; }
        [JsonProperty("BanDateMilliseconds")]
        public string? BanDateMilliseconds { get; set; }
        [JsonProperty("SteamID64")]
        public string? SteamID64 { get; set; }
        [JsonProperty("Banned")]
        public bool Banned { get; set; }
        [JsonProperty("TempBanCheck")]
        public string? TempBanCheck { get; set; }
    }

    private void Init();
     void OnPlayerConnected(BasePlayer player);
     void CheckAndApplyGameBan(ulong userId);
     void HandleGameBan(int httpCode, string response, ulong userId);
    private void SendDiscordEmbed(GameBannedDbResponse gameBanDetails, BasePlayer player);
    private IEnumerator PostToDiscord(string url, WWWForm data);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Log To Console")]
    public bool LogToConsole;
    [JsonProperty(PropertyName = "Log To Discord")]
    public bool LogToDiscord;
    [JsonProperty(PropertyName = "Discord Webhook URL")]
    public string DiscordWebhookUrl;
    [JsonProperty(PropertyName = "Whitelist Steam IDs")]
    public List<ulong> WhitelistedSteamIds;
    [JsonProperty(PropertyName = "Automatically Ban Players with previous Rust Bans")]
    public bool AutomaticallyBan;
    [JsonProperty(PropertyName = "Ban Message")]
    public string BanMessage;
}

private class GameBannedDbResponse
{
    [JsonProperty("TweetLink")]
    public string? TweetLink { get; set; }
    [JsonProperty("BanDateMilliseconds")]
    public string? BanDateMilliseconds { get; set; }
    [JsonProperty("SteamID64")]
    public string? SteamID64 { get; set; }
    [JsonProperty("Banned")]
    public bool Banned { get; set; }
    [JsonProperty("TempBanCheck")]
    public string? TempBanCheck { get; set; }
}


```

---

## RustIOClans by dcode - Allows your players to form and manage clans with Rust:IO

```csharp
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using System.Text;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Rust:IO Clans", "playrust.io / dcode", "1.7.6")]
[Description("Allows your players to form and manage clans with Rust:IO")]
public class Clans : RustPlugin
{
    private Library lib;
    private MethodInfo isInstalled;
    private MethodInfo hasFriend;
    private MethodInfo addFriend;
    private MethodInfo deleteFriend;
    private void ioInitialize();
    private bool ioIsInstalled();
    private bool ioHasFriend(string playerId, string friendId);
    private bool ioAddFriend(string playerId, string friendId);
    private bool ioDeleteFriend(string playerId, string friendId);
    private Dictionary<string, Clan> clans;
    private Dictionary<string, string> originalNames;
    private Regex tagRe;
    private Dictionary<string, string> messages;
    private Dictionary<string, Clan> lookup;
    private bool addClanMatesAsFriends;
    private int limitMembers;
    private int limitModerators;
    private void loadData();
    private void saveData();
    private List<string> texts;
    protected override void LoadDefaultConfig();
    private string _(string text, Dictionary<string, string> replacements);
    private Clan findClan(string tag);
    private Clan findClanByUser(string userId);
    private BasePlayer findPlayerByPartialName(string name);
    private string stripTag(string name, Clan clan);
    private void setupPlayer(BasePlayer player);
    private void setupPlayers(List<string> playerIds);
    private void OnServerInitialized();
    private void OnUserApprove(Network.Connection connection);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    private void SendHelpText(BasePlayer player);
    [ChatCommand("clan")]
    private void ClanCommand(BasePlayer player, string command, string[] args);
    [ChatCommand("c")]
    private void ClanChatCommand(BasePlayer player, string command, string[] args);
    public class Clan
    {
        public string tag;
        public string description;
        public string owner;
        public List<string> moderators;
        public List<string> members;
        public List<string> invited;
        public static Clan Create(string tag, string description, string ownerId);
        public bool IsOwner(string userId);
        public bool IsModerator(string userId);
        public bool IsMember(string userId);
        public bool IsInvited(string userId);
        public void Broadcast(string message, ulong senderId);
        internal JObject ToJObject();
        internal void onCreate();
        internal void onUpdate();
        internal void onDestroy();
    }

    [HookMethod("GetClan")]
    private JObject GetClan(string tag);
    [HookMethod("GetAllClans")]
    private JArray GetAllClans();
    [HookMethod("GetClanOf")]
    private string GetClanOf(object player);
}

public class Clan
{
    public string tag;
    public string description;
    public string owner;
    public List<string> moderators;
    public List<string> members;
    public List<string> invited;
    public static Clan Create(string tag, string description, string ownerId);
    public bool IsOwner(string userId);
    public bool IsModerator(string userId);
    public bool IsMember(string userId);
    public bool IsInvited(string userId);
    public void Broadcast(string message, ulong senderId);
    internal JObject ToJObject();
    internal void onCreate();
    internal void onUpdate();
    internal void onDestroy();
}


```

---

## RustIOFriendlyFire by dcode - Disables friendly fire for Rust:IO friends

```csharp
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Reflection;
using System.Text;

Oxide.Plugins
[Info("FriendlyFire", "playrust.io / dcode", "1.7.0")]
public class FriendlyFire : RustPlugin
{
    private Library lib;
    private MethodInfo isInstalled;
    private MethodInfo hasFriend;
    private MethodInfo addFriend;
    private MethodInfo deleteFriend;
    private void InitializeRustIO();
    private bool IsInstalled();
    private bool HasFriend(string playerId, string friendId);
    private bool AddFriend(string playerId, string friendId);
    private bool DeleteFriend(string playerId, string friendId);
    private List<ulong> manuallyEnabledBy;
    private List<ulong> apiBypassedFor;
    private List<string> texts;
    private Dictionary<string, string> messages;
    private Dictionary<string, DateTime> notificationTimes;
    private string _(string text, Dictionary<string, string> replacements);
    protected override void LoadDefaultConfig();
    private T GetConfig(string name, T defaultValue);
    [HookMethod("OnServerInitialized")]
     void OnServerInitialized();
    private void RestoreDefaults(BasePlayer player);
    [HookMethod("OnPlayerConnected")]
     void OnPlayerConnected(BasePlayer player);
    [HookMethod("OnPlayerDisconnected")]
     void OnPlayerDisconnected(BasePlayer player);
    private object OnAttackShared(BasePlayer attacker, BasePlayer victim, HitInfo hit);
    [HookMethod("OnPlayerAttack")]
     void OnPlayerAttack(BasePlayer attacker, HitInfo hit);
    [HookMethod("OnEntityTakeDamage")]
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hit);
    [ChatCommand("ff")]
    private void cmdChatFF(BasePlayer player, string command, string[] args);
    [HookMethod("SendHelpText")]
    private void SendHelpText(BasePlayer player);
    [HookMethod("EnableBypass")]
    private bool EnableBypass(object playerId);
    [HookMethod("DisableBypass")]
    private bool DisableBypass(object playerId);
    private void Log(string message);
    private void Warn(string message);
    private void Error(string message);
}


```

---

## RustLax by ColonBlow - Control when horses drop dung in minutes

```csharp
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Rust Lax", "Colon Blow", "1.0.3")]
[Description("Control when Horses drop dung in minutes")]
public class RustLax : CovalencePlugin
{
    private const string permAdmin;
    private const string permMounted;
    private void Init();
    private void OnServerInitialized();
    private PluginConfig config;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Dung Time - Number of Minutes between dung drops : ")]
        public float dungTimeGlobal { get; set; }
        [JsonProperty(PropertyName = "Dung Time Mounted - Mounted players with rustlax.mounted perms, will change dung drop time to (Minutes) : ")]
        public float dungTimeMounted { get; set; }
    }

    private PluginConfig GetDefaultConfig();
    protected override void LoadDefaultConfig();
    private void SaveConfig();
    protected override void LoadDefaultMessages();
    [Command("rustlax.reset")]
    private void cmdRustLaxReset(IPlayer player, string command, string[] args);
    private void OnEntitySpawned(BaseRidableAnimal animal);
    private void OnEntityMounted(BaseMountable entity, BasePlayer player);
    private void OnEntityDismounted(BaseMountable entity, BasePlayer player);
    private void ProcessExistingAnimals(float dungAdjustment);
    private void ProcessAnimal(BaseRidableAnimal animal, float dungAdjustment);
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Dung Time - Number of Minutes between dung drops : ")]
    public float dungTimeGlobal { get; set; }
    [JsonProperty(PropertyName = "Dung Time Mounted - Mounted players with rustlax.mounted perms, will change dung drop time to (Minutes) : ")]
    public float dungTimeMounted { get; set; }
}


```

---

## RustMapApi by MJSU - An API to generate the Rust server map image

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Diagnostics;
using System.Drawing;
using System.IO;
using System.Linq;
using Facepunch.Utility;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;
using UnityEngine.Networking;
using Color = UnityEngine.Color;

Oxide.Plugins
[Info("Rust Map Api", "MJSU", "1.3.2")]
[Description("An API to generate the rust server map image")]
internal class RustMapApi : RustPlugin
{
    [PluginReference]
    private Plugin ImgurApi;
    private PluginConfig _pluginConfig;
    private StoredData _storedData;
    private TerrainTexturing _terrainTexture;
    private Terrain _terrain;
    private TerrainHeightMap _heightMap;
    private TerrainSplatMap _splatMap;
    private readonly Hash<string, RenderInfo> _renders;
    private readonly Hash<string, Hash<string, Hash<string, Hash<string, object>>>> _imageCache;
    private List<Hash<string, object>> _iconOverlay;
    private bool _isReady;
    private Coroutine _storeImageRoutine;
    private readonly Queue<StorageInfo> _storageQueue;
    private const string DefaultMapName;
    private const string IconMapName;
    private readonly Hash<MapColorsVersion, MapColors> _mapColorVersions;
    private readonly Hash<string, IconConfig> _defaultIcons;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private IEnumerator CreateStartupImages();
    private void OnNewSave(string filename);
    private void Unload();
    private IEnumerator ValidateImages();
    private IEnumerator ValidateConfig();
    private IEnumerator LoadIcons();
    private IEnumerator CreateStartingRenders();
    private IEnumerator CreateStartupSplits();
    [ConsoleCommand("rma_regenerate")]
    private void RegenerateConsoleCommand(ConsoleSystem.Arg arg);
    private IEnumerator HandleRegenerate(ConsoleSystem.Arg arg);
    [ConsoleCommand("rma_upload")]
    private void UploadConsoleCommand(ConsoleSystem.Arg arg);
    private string GetImageTitle(string mapName, int row, int col);
    private string GetSectionTitle(string mapName, string section);
    private void HandleSingleResponse(Hash<string, object> response);
    private void HandleAlbumResponse(Hash<string, Hash<string, object>> response);
    private bool IsReady();
    private int GetDefaultResolution();
    private int GetDefaultImageFormat();
    private Hash<string, object> CreateRender(string mapName, Hash<string, object> config);
    private object CreatePluginRender(Plugin plugin, string mapName, int resolution);
    private object CreatePluginImage(Plugin plugin, string mapName, int resolution, int encoding);
    private void UploadPluginImageSingle(Plugin plugin, string mapName, int resolution, int encoding, Action<Hash<string,object>> callback, string title);
    private Hash<string, object> CreateRenderOverlay(string renderSource, string newMapName, List<Hash<string, object>> overlay);
    private Hash<string, object> GetRender(string mapName);
    private Hash<string, Hash<string, object>> CreateSingle(string mapName, int encodingMode);
    private Hash<string, Hash<string, object>> CreateSingle(Array2D<Color> render, EncodingMode mode);
    private Hash<string, Hash<string,object>> CreateSplice(string mapName, int numRows, int numCols, int encodingMode);
    private Hash<string, Hash<string, object>> CreateSplice(Array2D<Color> render, int numRows, int numCols, EncodingMode mode);
    private Hash<string, Hash<string, object>> SaveSingleImage(string mapName);
    private Hash<string, Hash<string, object>> SaveSplitImage(string mapName, int numRows, int numCols);
    private List<string> GetRenderNames();
    private List<string> GetSavedSplits(string mapName);
    private Hash<string, object> GetFullMap(string mapName);
    private List<Hash<string, object>> GetIconOverlay();
    private bool HasSplit(string mapName, int numRows, int numCols);
    private Hash<string, object> GetSection(string mapName, int numRows, int numCols, int row, int col);
    private Hash<string, Hash<string, object>> GetSplit(string mapName, int numRows, int numCols);
    private List<Hash<string, object>> BuildIconMonuments(int size);
    private static void AddImageToOverlay(float x, float z, float posScale, List<Hash<string, object>> overlays, IconConfig config, string name);
    private string GetMonumentName(MonumentInfo monument);
    private Hash<string, object> LoadSection(string mapName, string split, string section);
    private void SaveSplit(string mapName, string split, Hash<string, Hash<string, object>> splitData);
    private IEnumerator HandleSave();
    private void StoreSection(string mapName, string split, string section, Hash<string, object> imageData);
    private Hash<string, Hash<string, object>> GetCacheSplit(string mapName, string split);
    private Hash<string, object> GetCacheSection(string mapName, string split, string section);
    private void SaveCache(string mapName, string split, Hash<string, Hash<string, object>> data);
    private void SaveCache(string mapName, string split, string section, Hash<string, object> data);
    private uint StoreImage(byte[] bytes, FileStorage.Type type);
    private byte[] LoadImage(uint id, FileStorage.Type type);
    private Hash<string, object> ImageDataFromBytes(byte[] bytes);
    private byte[] BytesFromImageData(Hash<string, object> data);
    private Hash<string, object> RenderToHash(Array2D<Color> colors);
    private Hash<string, object> CreateMapData(Array2D<Color> colors, EncodingMode mode);
    private string GetIndex(int row, int col);
    private MapColors GetDefaultColors();
    private string GetDuration(double milliseconds);
    private void SaveData();
    private void UploadSingle(Hash<string, object> map, string title, Action<Hash<string, object>> callback);
    private void UploadAlbum(Hash<string, Hash<string, object>> map, string title, string mapName, Action<Hash<string, Hash<string, object>>> callback);
    private Array2D<Color> Render(ImageConfig config, int mapSize);
    private Array2D<Color> RenderOverlay(Array2D<Color> previous, List<OverlayConfig> overlays);
     float GetHeight(float x, float y);
     Vector3 GetNormal(float x, float y);
     float GetSplat(float x, float y, int mask);
    private byte[] EncodeTo(Color[] color, int width, int height, EncodingMode mode);
    private Bitmap ResizeImage(byte[] bytes, int targetWidth, int targetHeight);
    private IEnumerator LoadIcon(IconConfig config);
    private IEnumerator DownloadIcon(IconConfig config, int code);
    private class PluginConfig
    {
        [DefaultValue(MapColorsVersion.Current)]
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Map Colors Version")]
        public MapColorsVersion MapColorsVersion { get; set; }
        [DefaultValue(EncodingMode.Jpg)]
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Default Image Encoding (Jpg, Png)")]
        public EncodingMode DefaultImageEncoding { get; set; }
        [JsonProperty(PropertyName = "Starting Splits (Rows x Columns)")]
        public List<string> StartingSplits { get; set; }
        [JsonProperty(PropertyName = "IconSettings")]
        public Hash<string, IconConfig> IconSettings { get; set; }
        [JsonProperty(PropertyName = "Custom Icons")]
        public List<CustomIcons> CustomIcons { get; set; }
    }

    private class IconConfig
    {
        public int Width { get; set; }
        public int Height { get; set; }
        public string ImageUrl { get; set; }
        public bool Show { get; set; }
        [JsonIgnore]
        public byte[] Image { get; set; }
    }

    private class CustomIcons : IconConfig
    {
        public float XPos { get; set; }
        public float ZPos { get; set; }
    }

    private class StoredData
    {
        public Hash<string, Hash<string, Hash<string, uint>>> MapIds;
        public Hash<int, uint> IconIds;
        public List<string> GetSavedSplits(string mapName);
    }

    private class StorageInfo
    {
        public string MapName { get; set; }
        public string Split { get; set; }
        public Hash<string, Hash<string, object>> SplitData { get; set; }
    }

    private class RenderInfo
    {
        public Array2D<Color> Colors { get; set; }
        public ImageConfig RenderConfig { get; set; }
        public List<OverlayConfig> OverlayConfig { get; set; }
        public RenderInfo();
        public RenderInfo(Array2D<Color> colors, ImageConfig renderConfig);
        public RenderInfo(Array2D<Color> colors, ImageConfig renderConfig, List<OverlayConfig> overlayConfig);
    }

    private class MapColors
    {
        public Vector3 StartColor { get; set; }
        public Vector4 WaterColor { get; set; }
        public Vector4 GravelColor { get; set; }
        public Vector4 DirtColor { get; set; }
        public Vector4 SandColor { get; set; }
        public Vector4 GrassColor { get; set; }
        public Vector4 ForestColor { get; set; }
        public Vector4 RockColor { get; set; }
        public Vector4 SnowColor { get; set; }
        public Vector4 PebbleColor { get; set; }
        public Vector4 OffShoreColor { get; set; }
        public Vector3 SunDirection { get; set; }
        public Vector3 Half { get; set; }
        public int WaterOffset { get; set; }
        public float SunPower { get; set; }
        public float Brightness { get; set; }
        public float Contrast { get; set; }
        public float OceanWaterLevel { get; set; }
    }

    private class ImageConfig : MapColors
    {
        public ImageConfig(MapColors defaultColors);
        public ImageConfig(Hash<string, object> config, MapColors defaultColors);
    }

    private class OverlayConfig
    {
        public int XPos { get; set; }
        public int YPos { get; set; }
        public int Width { get; set; }
        public int Height { get; set; }
        public byte[] Image { get; set; }
        public string DebugName { get; set; }
        public OverlayConfig();
        public OverlayConfig(Hash<string, object> data);
    }

}

private class PluginConfig
{
    [DefaultValue(MapColorsVersion.Current)]
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Map Colors Version")]
    public MapColorsVersion MapColorsVersion { get; set; }
    [DefaultValue(EncodingMode.Jpg)]
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Default Image Encoding (Jpg, Png)")]
    public EncodingMode DefaultImageEncoding { get; set; }
    [JsonProperty(PropertyName = "Starting Splits (Rows x Columns)")]
    public List<string> StartingSplits { get; set; }
    [JsonProperty(PropertyName = "IconSettings")]
    public Hash<string, IconConfig> IconSettings { get; set; }
    [JsonProperty(PropertyName = "Custom Icons")]
    public List<CustomIcons> CustomIcons { get; set; }
}

private class IconConfig
{
    public int Width { get; set; }
    public int Height { get; set; }
    public string ImageUrl { get; set; }
    public bool Show { get; set; }
    [JsonIgnore]
    public byte[] Image { get; set; }
}

private class CustomIcons : IconConfig
{
    public float XPos { get; set; }
    public float ZPos { get; set; }
}

private class StoredData
{
    public Hash<string, Hash<string, Hash<string, uint>>> MapIds;
    public Hash<int, uint> IconIds;
    public List<string> GetSavedSplits(string mapName);
}

private class StorageInfo
{
    public string MapName { get; set; }
    public string Split { get; set; }
    public Hash<string, Hash<string, object>> SplitData { get; set; }
}

private class RenderInfo
{
    public Array2D<Color> Colors { get; set; }
    public ImageConfig RenderConfig { get; set; }
    public List<OverlayConfig> OverlayConfig { get; set; }
    public RenderInfo();
    public RenderInfo(Array2D<Color> colors, ImageConfig renderConfig);
    public RenderInfo(Array2D<Color> colors, ImageConfig renderConfig, List<OverlayConfig> overlayConfig);
}

private class MapColors
{
    public Vector3 StartColor { get; set; }
    public Vector4 WaterColor { get; set; }
    public Vector4 GravelColor { get; set; }
    public Vector4 DirtColor { get; set; }
    public Vector4 SandColor { get; set; }
    public Vector4 GrassColor { get; set; }
    public Vector4 ForestColor { get; set; }
    public Vector4 RockColor { get; set; }
    public Vector4 SnowColor { get; set; }
    public Vector4 PebbleColor { get; set; }
    public Vector4 OffShoreColor { get; set; }
    public Vector3 SunDirection { get; set; }
    public Vector3 Half { get; set; }
    public int WaterOffset { get; set; }
    public float SunPower { get; set; }
    public float Brightness { get; set; }
    public float Contrast { get; set; }
    public float OceanWaterLevel { get; set; }
}

private class ImageConfig : MapColors
{
    public ImageConfig(MapColors defaultColors);
    public ImageConfig(Hash<string, object> config, MapColors defaultColors);
}

private class OverlayConfig
{
    public int XPos { get; set; }
    public int YPos { get; set; }
    public int Width { get; set; }
    public int Height { get; set; }
    public byte[] Image { get; set; }
    public string DebugName { get; set; }
    public OverlayConfig();
    public OverlayConfig(Hash<string, object> data);
}


```

---

## RustNotifications by  - Notifies when a player connects/disconnects from the server and when structures are attacked

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Rust Notifications", "seanbyrne88", "0.9.3")]
[Description("Configurable Notifications for Rust Events")]
 class RustNotifications : RustPlugin
{
    [PluginReference]
     Plugin Discord;
     Plugin Slack;
    private static NotificationConfigContainer Settings;
    private List<NotificationCooldown> UserLastNotified;
    private string SlackMethodName;
    private string DiscordMethodName;
     void Init();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("rustNotifyResetConfig")]
     void CommandResetConfig(BasePlayer player, string command, string[] args);
    [ChatCommand("rustNotifyResetMessages")]
     void CommandResetMessages(BasePlayer player, string command, string[] args);
    [ChatCommand("rustNotifySetHealthThreshold")]
     void CommandSetHealthThreshold(BasePlayer player, string command, string[] args);
    private string BuildConnectMessage(string PlayerUserIDString, string PlayerDisplayName);
    private string BuildDisconnectMessage(string PlayerUserIDString, string PlayerDisplayName, string Reason);
    private string GetDisplayNameByID(ulong UserID);
    private bool IsPlayerActive(ulong UserID);
    private bool IsPlayerNotificationCooledDown(ulong UserID, NotificationType NotificationType, int CooldownInSeconds);
    private void SendSlackNotification(BasePlayer player, string MessageText);
    private void SendDiscordNotification(BasePlayer player, string MessageText);
    private IPlayer BasePlayerToIPlayer(BasePlayer player);
    private void SendPlayerConnectNotification(BasePlayer player);
    private void SendPlayerDisconnectNotification(BasePlayer player, string reason);
    private void LoadDefaultMessages();
     NotificationConfigContainer DefaultConfigContainer();
     ServerNotificationConfig DefaultServerNotificationConfig();
     ClientNotificationConfig DefaultClientNotificationConfig();
    protected override void LoadDefaultConfig();
    protected void LoadConfigValues();
    private class ServerNotificationConfig
    {
        public bool Active { get; set; }
        public bool DoNotifyWhenBaseAttacked { get; set; }
        public int NotificationCooldownInSeconds { get; set; }
        public int ThresholdPercentageHealthRemaining { get; set; }
    }

    private class ClientNotificationConfig : ServerNotificationConfig
    {
        public bool DoLinkSteamProfile { get; set; }
        public bool DoNotifyWhenPlayerConnects { get; set; }
        public bool DoNotifyWhenPlayerDisconnects { get; set; }
    }

    private class NotificationConfigContainer
    {
        public ServerNotificationConfig ServerConfig { get; set; }
        public ClientNotificationConfig SlackConfig { get; set; }
        public ClientNotificationConfig DiscordConfig { get; set; }
    }

    private class NotificationCooldown
    {
        public NotificationType NotificationType { get; set; }
        public ulong PlayerID { get; set; }
        public DateTime LastNotifiedAt { get; set; }
    }

}

private class ServerNotificationConfig
{
    public bool Active { get; set; }
    public bool DoNotifyWhenBaseAttacked { get; set; }
    public int NotificationCooldownInSeconds { get; set; }
    public int ThresholdPercentageHealthRemaining { get; set; }
}

private class ClientNotificationConfig : ServerNotificationConfig
{
    public bool DoLinkSteamProfile { get; set; }
    public bool DoNotifyWhenPlayerConnects { get; set; }
    public bool DoNotifyWhenPlayerDisconnects { get; set; }
}

private class NotificationConfigContainer
{
    public ServerNotificationConfig ServerConfig { get; set; }
    public ClientNotificationConfig SlackConfig { get; set; }
    public ClientNotificationConfig DiscordConfig { get; set; }
}

private class NotificationCooldown
{
    public NotificationType NotificationType { get; set; }
    public ulong PlayerID { get; set; }
    public DateTime LastNotifiedAt { get; set; }
}


```

---

## RustSpawner by DNARust - Allows players to spawn various vehicles (boats, cars, heli's, animals) with permission & cooldowns

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Rust Spawner ", "Daano123", "3.0.1")]
[Description("Rust Spawner Reworked is a plugin that you can spawn cars/helis/boats/animals/scarecrow. REWORKED Zoin plugin")]
 class RustSpawner : CovalencePlugin
{
     Dictionary<string, Timer> CoolDownsHorse;
     Dictionary<string, Timer> CoolDownsWolf;
     Dictionary<string, Timer> CoolDownsBear;
     Dictionary<string, Timer> CoolDownsMini;
     Dictionary<string, Timer> CoolDownsSedan;
     Dictionary<string, Timer> CoolDownsScrapHeli;
     Dictionary<string, Timer> CoolDownsChinook;
     Dictionary<string, Timer> CoolDownsRhib;
     Dictionary<string, Timer> CoolDownsBoat;
    const string Horse_Perm;
    const string Wolf_Perm;
    const string Bear_Perm;
    const string Sedan_Perm;
    const string Minicopter_Perm;
    const string ScrapHeli_Perm;
    const string Chinook_Perm;
    const string Rhib_Perm;
    const string Boat_Perm;
    const string NoCooldown_Perm;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    [Command("rspawn")]
    private void Spawn(IPlayer player, string command, string[] args);
     void SpawnEntity(IPlayer player, string entity_name, string cooldown);
}


```

---

## RustStatistics by PWWENILL - Rust Statistics is a plugins that retrieve data from players and servers

```csharp
using System;
using CompanionServer.Handlers;
using Oxide.Core.Libraries;
using ConVar;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Game.Rust.Libraries;
using System.Linq;

Oxide.Plugins
[Info("Rust Statistics", "Pwenill", "1.0.0")]
[Description("Get player statistics for sent to our website, and warn the server of potential threat")]
 class RustStatistics : RustPlugin
{
    private PluginConfig config;
    private Timer timerSendStats;
    private Dictionary<string, PlayerStats> playersData;
    private ServerStats serversStats;
    public string rs_token;
    public string rs_url;
    private class ResponseRequest
    {
        public string data;
        public string type;
    }

    private class ServerStats
    {
        public ServerOptional info;
        public Dictionary<DateTime, int> players;
        public Dictionary<string, string> admins;
    }

    private class ServerOptional
    {
        public string hostname;
        public string description;
        public int queryport;
        public int port;
        public ServerOptional();
    }

    private class PlayerStats
    {
        public string SteamID;
        public string Username;
        public Dictionary<string, Dictionary<string, int>> Npc_hit;
        public Dictionary<string, Dictionary<string, int>> Hits;
        public Dictionary<DateTime, StatsKD> KD;
        public Dictionary<DateTime, int> Messages;
        public Dictionary<DateTime, int> Connections;
        public List<Constructions> ContructionsDestroyed;
        public PlayerStats(string steamID, string username);
    }

    private class Constructions
    {
        public string Name;
        public string Grade;
        public Dictionary<string, int> Weapons;
        public Constructions(string name, string grade, Dictionary<string, int> weapons);
    }

    private class StatsKD
    {
        public int Kills;
        public int Deaths;
        public StatsKD(int kills, int deaths);
    }

    private class PluginConfig
    {
        public string ClientID;
        public string ClientSecret;
        public string Token;
        public bool Linked;
        public float Interval;
        public bool DebugConsole;
        public BansLevelsConfig BansLevel;
    }

    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private class BansLevelsConfig
    {
        public int Critical;
        public int Moderate;
        public int Minor;
        public BansLevelsConfig();
    }

    private void SaveConfig();
     void OnServerSave();
    private void Init();
     void Loaded();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnPlayerBanned(string name, ulong id, string address, string reason);
     object OnPlayerDeath(BasePlayer player, HitInfo info);
     object OnPlayerAttack(BasePlayer attacker, HitInfo info);
     object OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
     void OnPlayerConnected(BasePlayer player);
     void TimerStatistics();
     void SendStatistics();
     void getAuthToken(Action<string> callback);
     void ExecuteAdminSystemBans(string id, string ipAddress, string name);
     object banPlayerWithLevel(string data, string steamid);
     void DebugConsole(string message, string type);
     object IncrementNPCHits(BasePlayer player, HitInfo info);
     object IncrementHits(BasePlayer player, HitInfo info);
     object IncrementCountTime(Dictionary<DateTime, int> dictionary);
     PlayerStats addOrGetPlayer(string steamid, string username);
    public static string QueryString(Dictionary<string, string> dict);
    protected override void LoadDefaultMessages();
}

private class ResponseRequest
{
    public string data;
    public string type;
}

private class ServerStats
{
    public ServerOptional info;
    public Dictionary<DateTime, int> players;
    public Dictionary<string, string> admins;
}

private class ServerOptional
{
    public string hostname;
    public string description;
    public int queryport;
    public int port;
    public ServerOptional();
}

private class PlayerStats
{
    public string SteamID;
    public string Username;
    public Dictionary<string, Dictionary<string, int>> Npc_hit;
    public Dictionary<string, Dictionary<string, int>> Hits;
    public Dictionary<DateTime, StatsKD> KD;
    public Dictionary<DateTime, int> Messages;
    public Dictionary<DateTime, int> Connections;
    public List<Constructions> ContructionsDestroyed;
    public PlayerStats(string steamID, string username);
}

private class Constructions
{
    public string Name;
    public string Grade;
    public Dictionary<string, int> Weapons;
    public Constructions(string name, string grade, Dictionary<string, int> weapons);
}

private class StatsKD
{
    public int Kills;
    public int Deaths;
    public StatsKD(int kills, int deaths);
}

private class PluginConfig
{
    public string ClientID;
    public string ClientSecret;
    public string Token;
    public bool Linked;
    public float Interval;
    public bool DebugConsole;
    public BansLevelsConfig BansLevel;
}

private class BansLevelsConfig
{
    public int Critical;
    public int Moderate;
    public int Minor;
    public BansLevelsConfig();
}


```

---

## RustTranslationAPI by MONaH - Provides translation APIs for Rust items, holdables, deployables, etc.

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.IO;
using System.Linq;
using System.Runtime.CompilerServices;
using Newtonsoft.Json.Converters;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

[Info("Rust Translation API", "MJSU", "2.0.1")]
[Description("Provides translations for Rust entities & items")]
public class RustTranslationAPI : RustPlugin
{
    private static readonly string LOGLine;
    private readonly Dictionary<string, Dictionary<string, string>> _languages;
    private readonly Dictionary<string, string> _constructionTokens;
    private readonly Dictionary<string, string> _deployableTokens;
    private readonly Dictionary<string, string> _displayNameTokens;
    private readonly Dictionary<string, string> _holdableTokens;
    private readonly Dictionary<string, string> _monumentTokens;
    private readonly Dictionary<uint, string> _prefabTokens;
    private bool _isInitialized;
    private void OnServerInitialized();
    private void Unload();
    private PluginConfig _pluginConfig;
    public class PluginConfig
    {
        [JsonConverter(typeof(StringEnumConverter))]
        [DefaultValue(LogLevel.Off)]
        [JsonProperty(PropertyName = "Log Level (Debug, Info, Warning, Error, Off)")]
        public LogLevel LoggingLevel { get; set; }
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    public static PluginConfig AdditionalConfig(PluginConfig config);
    public void ProcessTranslations();
    public void ProcessItems();
    public void ProcessMonuments();
    public void ProcessAttributes();
    private bool IsInitialized();
    private bool IsSupportedLanguage(string language);
    private string GetTranslation(string language, string token);
    private string GetLanguage(BasePlayer player);
    private string GetTranslation(string language, Translate.Phrase token);
    private string GetTranslation(BasePlayer player, Translate.Phrase token);
    private string GetTranslation(string language, Item item);
    private string GetTranslation(BasePlayer player, Item item);
    private string GetTranslation(string language, ItemDefinition def);
    private string GetTranslation(BasePlayer player, ItemDefinition def);
    private string GetTranslation(string language, BaseEntity entity);
    private string GetTranslation(BasePlayer player, BaseEntity entity);
    private string GetTranslation(string language, MonumentInfo monument);
    private string GetTranslation(BasePlayer player, MonumentInfo monument);
    private string GetTranslation(string language, Construction construction);
    private string GetTranslation(BasePlayer player, Construction monument);
    private string GetPrefabTranslation(string language, uint prefabId);
    private string GetPrefabTranslation(BasePlayer player, uint prefabId);
    private string GetItemDescriptionByID(string language, int itemID);
    private string GetItemDescriptionByID(BasePlayer player, int itemID);
    private string GetItemDescriptionByDefinition(string language, ItemDefinition def);
    private string GetItemDescriptionByDefinition(BasePlayer player, ItemDefinition def);
    private string GetItemTranslationByID(string language, int itemID);
    private string GetItemTranslationByDisplayName(string language, string displayName);
    private string GetItemTranslationByDefinition(string language, ItemDefinition def);
    private string GetItemTranslationByShortName(string language, string itemShortName);
    private string GetDeployableTranslation(string language, string deployable);
    private string GetHoldableTranslation(string language, string holdable);
    private string GetMonumentTranslation(string language, string monumentName);
    private string GetMonumentTranslation(string language, MonumentInfo monumentInfo);
    private string GetConstructionTranslation(string language, string constructionName);
    private string GetConstructionTranslation(string language, Construction construction);
    public void Log(string message, LogLevel level, string filename, string methodName);
}

public class PluginConfig
{
    [JsonConverter(typeof(StringEnumConverter))]
    [DefaultValue(LogLevel.Off)]
    [JsonProperty(PropertyName = "Log Level (Debug, Info, Warning, Error, Off)")]
    public LogLevel LoggingLevel { get; set; }
}


```

---

## RustyCuffs by RevolvingDCON - Allows you to restrain and escort other players. Perfect for roleplay servers.

```csharp
using System.IO;
using System.Linq;
using System.Collections.Generic;
using System.Globalization;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Network;
using UnityEngine;

Oxide.Plugins
[Info("Rusty Cuffs", "Revolving DCON", "0.9.35")]
[Description("Handcuffs allowing you to restrain and escort players")]
public class RustyCuffs : CovalencePlugin
{
    protected Oxide.Game.Rust.Libraries.Player Player;
    private ulong cuffsSkinID;
    private string cuffsItemShortname;
    private string keysItemShortname;
    private StoredData storedData;
    private bool storageChanged;
    private Vector3 chairPositionOffset;
    private Dictionary<string, UIContainer> userUIContainers;
    private Dictionary<string, Timer> userTimers;
    private Dictionary<string,string> listenToUsers;
    private Dictionary<BasePlayer,BasePlayer> selectedUsers;
    private Dictionary<BasePlayer,BasePlayer> escortingUsers;
    private List<BasePlayer> usersInputDisabled;
    private Dictionary<string,ChairHack> restrainChairs;
    private List<BaseMountable> chairEnts;
    private int obstructionMask;
    private int baseMask;
    private Dictionary<string,string> perms;
    private new Dictionary<string, string> messages;
    private static RustyCuffs _ins;
    private Configuration config;
    protected override void LoadDefaultMessages();
    public class Configuration
    {
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat Icon")]
        public ulong icon;
        [JsonProperty(PropertyName = "Return Cuffs [Give cuffs back when player is unrestrained]")]
        public bool returnCuffs;
        [JsonProperty(PropertyName = "Restrain Time [How long does it take to restrain a player]")]
        public float restrainTime;
        [JsonProperty(PropertyName = "Restrain Distance [Maximum distance players can be restrained from]")]
        public float restrainDist;
        [JsonProperty(PropertyName = "Escort Distance [Distance players are while being escorted]")]
        public float escortDist;
        [JsonProperty(PropertyName = "Restrain NPCs [Can NPCs be restrained]")]
        public bool npcsEnabled;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public class StoredData
    {
        public Dictionary<ulong, string> keys;
        public Dictionary<string, string> restrained;
    }

    private void SaveData();
    private void OnServerSave();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void RestrainCmd(IPlayer player, string command, string[] args);
    private void OnPlayerInput(BasePlayer player, InputState state);
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private void OnPlayerRevive(BasePlayer reviver, BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private object CanLootEntity(BasePlayer player, StorageContainer container);
    private object CanLootEntity(BasePlayer player, LootableCorpse container);
    private object CanLootEntity(BasePlayer player, DroppedItemContainer container);
    private object CanPickupEntity(BasePlayer player, BaseCombatEntity ent);
    private object CanMountEntity(BasePlayer player, BaseMountable mount);
    private object CanUnlock(BasePlayer player, BaseLock baseLock);
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private object CanSpectateTarget(BasePlayer player, string name);
     object CanTeleport(BasePlayer player);
     object canTeleport(BasePlayer player);
     object CanGridTeleport(BasePlayer player);
    private void UIBtnCallback(IPlayer player, string command, string[] args);
    public class UIContainer
    {
        public static string panel_overlay_cuffs_id;
        public static string panel_overlay_menu_id;
        public BasePlayer player;
        public BasePlayer target;
        public readonly ProgressBar Progress;
        public readonly DynamicMenu Menu;
        public UIContainer(BasePlayer p, BasePlayer t);
        public void Update();
        public void UpdateTarget(BasePlayer t);
        public class ProgressBar
        {
            private CuiElementContainer parentContainer;
            private string Name;
            private string Parent;
            private BasePlayer player;
            private BasePlayer target;
            public void Update(BasePlayer p, BasePlayer t);
            public void Draw();
            public void UpdateFill(double amount);
            public void Update(float amount);
            public void Destroy();
        }

        public class DynamicMenu
        {
            private CuiElementContainer parentContainer;
            private string Name;
            private BasePlayer player;
            private BasePlayer target;
            public void Update(BasePlayer p, BasePlayer t);
            public void Draw();
            private void OptionBtn(string label, string command, string color, float posy);
            public void Destroy();
        }

    }

    private bool API_IsRestrained(BasePlayer player);
    private Item API_CreateCuffs(int amount);
    private Item API_CreateCuffsKey(BasePlayer player);
    private bool API_Restrain(BasePlayer target, BasePlayer player);
    private bool API_Unrestrain(BasePlayer target);
    private bool IsNPC(BasePlayer player);
    private bool IsViewObstructed(BasePlayer player, int mask);
    private bool IsRestrained(BasePlayer target);
    private bool Restrain(BasePlayer target, BasePlayer player);
    private bool Unrestrain(BasePlayer target);
    private bool PlayerSelect(BasePlayer player, BasePlayer target, bool limitDist);
    private bool PlayerDeselect(BasePlayer player);
    private bool StartInspecting(BasePlayer target, BasePlayer player);
    private bool StartEscorting(BasePlayer target, BasePlayer player);
    private bool StopEscorting(BasePlayer player, Vector3 pos, bool soft);
    private void DestroyChair(ChairHack chairHack);
    private Item CreateCuffs(int amount);
    private Item CreateCuffsKey(string name, int amount);
    private void DropItem(Item item, Vector3 pos);
    private void GiveItem(BasePlayer player, Item item);
    private BaseEntity CreateBot(Vector3 pos);
    private void BroadcastPlayer(BasePlayer player, string msg, bool prefix);
    private void SendMessage(BasePlayer player, string[] args, bool prefix);
    private void SendMessage(IPlayer player, string[] args, bool prefix);
    private void StopProgress(BasePlayer player, UIContainer cont);
    private void DisableUserInput(BasePlayer player);
    private void EnableUserInput(BasePlayer player);
    private void PlayNetworkAnimation(BasePlayer bpr, string command);
    private void PlayNetworkAnimation(string id, string command);
    public void PlayEffect(BasePlayer player, string prefab, bool global);
    public void PlayEffect(IPlayer player, string prefab, bool global);
    private float mdist;
    private int generalColl;
    private bool TryGetPlayerView(BasePlayer player, Quaternion viewAngle);
    private bool TryGetClosestRayPoint(Vector3 sourcePos, Quaternion sourceDir, object closestEnt, Vector3 closestHitpoint);
    public List<BasePlayer> FindPlayers(string nameOrIdOrIp);
    public List<BasePlayer> FindPlayersOnline(string nameOrIdOrIp);
    private Dictionary<string, List<BasePlayer>> findPlayerMatches;
    private Dictionary<string, string[]> findPlayerArgs;
    public bool FindPlayer(string name, object player, string command, string[] args, BasePlayer target);
    private void FindPlayerShowMatches(BasePlayer player);
    private BasePlayer RayToPlayer(BasePlayer bplayer);
     class ChairHack
    {
        public BaseVehicle chair;
        public BaseMountable mount;
        public ChairHack();
        public void Destroy();
    }

    private class ForceLocation : MonoBehaviour
    {
        internal Vector3 pos;
        public BasePlayer target;
        private void Awake();
        public void Remove();
        private void FixedUpdate();
    }

    private class AntiHack : MonoBehaviour
    {
        public BasePlayer player;
        public BasePlayer target;
        public void Instantiate(BasePlayer player, BasePlayer target);
        public void Remove();
        private void FixedUpdate();
    }

    private class RestrainInspector : MonoBehaviour
    {
        private BasePlayer player;
        private BasePlayer target;
        private int ticks;
        public void Instantiate(BasePlayer player, BasePlayer target);
        private void UpdateLoot();
        private void StopInspecting(bool forced);
        private void BeginLooting();
        private void EndLooting();
        public void Remove(bool forced);
    }

    public void jPrint(object type);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat Icon")]
    public ulong icon;
    [JsonProperty(PropertyName = "Return Cuffs [Give cuffs back when player is unrestrained]")]
    public bool returnCuffs;
    [JsonProperty(PropertyName = "Restrain Time [How long does it take to restrain a player]")]
    public float restrainTime;
    [JsonProperty(PropertyName = "Restrain Distance [Maximum distance players can be restrained from]")]
    public float restrainDist;
    [JsonProperty(PropertyName = "Escort Distance [Distance players are while being escorted]")]
    public float escortDist;
    [JsonProperty(PropertyName = "Restrain NPCs [Can NPCs be restrained]")]
    public bool npcsEnabled;
}

public class StoredData
{
    public Dictionary<ulong, string> keys;
    public Dictionary<string, string> restrained;
}

public class UIContainer
{
    public static string panel_overlay_cuffs_id;
    public static string panel_overlay_menu_id;
    public BasePlayer player;
    public BasePlayer target;
    public readonly ProgressBar Progress;
    public readonly DynamicMenu Menu;
    public UIContainer(BasePlayer p, BasePlayer t);
    public void Update();
    public void UpdateTarget(BasePlayer t);
    public class ProgressBar
    {
        private CuiElementContainer parentContainer;
        private string Name;
        private string Parent;
        private BasePlayer player;
        private BasePlayer target;
        public void Update(BasePlayer p, BasePlayer t);
        public void Draw();
        public void UpdateFill(double amount);
        public void Update(float amount);
        public void Destroy();
    }

    public class DynamicMenu
    {
        private CuiElementContainer parentContainer;
        private string Name;
        private BasePlayer player;
        private BasePlayer target;
        public void Update(BasePlayer p, BasePlayer t);
        public void Draw();
        private void OptionBtn(string label, string command, string color, float posy);
        public void Destroy();
    }

}

public class ProgressBar
{
    private CuiElementContainer parentContainer;
    private string Name;
    private string Parent;
    private BasePlayer player;
    private BasePlayer target;
    public void Update(BasePlayer p, BasePlayer t);
    public void Draw();
    public void UpdateFill(double amount);
    public void Update(float amount);
    public void Destroy();
}

public class DynamicMenu
{
    private CuiElementContainer parentContainer;
    private string Name;
    private BasePlayer player;
    private BasePlayer target;
    public void Update(BasePlayer p, BasePlayer t);
    public void Draw();
    private void OptionBtn(string label, string command, string color, float posy);
    public void Destroy();
}

 class ChairHack
{
    public BaseVehicle chair;
    public BaseMountable mount;
    public ChairHack();
    public void Destroy();
}

private class ForceLocation : MonoBehaviour
{
    internal Vector3 pos;
    public BasePlayer target;
    private void Awake();
    public void Remove();
    private void FixedUpdate();
}

private class AntiHack : MonoBehaviour
{
    public BasePlayer player;
    public BasePlayer target;
    public void Instantiate(BasePlayer player, BasePlayer target);
    public void Remove();
    private void FixedUpdate();
}

private class RestrainInspector : MonoBehaviour
{
    private BasePlayer player;
    private BasePlayer target;
    private int ticks;
    public void Instantiate(BasePlayer player, BasePlayer target);
    private void UpdateLoot();
    private void StopInspecting(bool forced);
    private void BeginLooting();
    private void EndLooting();
    public void Remove(bool forced);
}


```

---

## RustyRockets by Strrobez - Allows players to spawn in configured items on a certain day within a certain hour.

```csharp
using System;
using Oxide.Core;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Rusty Rockets", "Strrobez", "1.1.0")]
[Description("Allow players to use explosives during a certain time frame.")]
public class RustyRockets : RustPlugin
{
    private readonly int Date;
    private readonly int Hour;
    private readonly Dictionary<string, DateTime> Cooldown;
    private readonly string UsePermission;
    private static Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Rocket Cooldown")]
        public int RocketCooldown;
        [JsonProperty(PropertyName = "Day (To Allow Spawning)")]
        public string Day;
        [JsonProperty(PropertyName = "Hour (To Allow Spawning)")]
        public string Hour;
        [JsonProperty(PropertyName = "Items (To Give Players)")]
        public Item[] Items;
        public static Configuration DefaultConfig();
    }

    public class Item
    {
        [JsonProperty(PropertyName = "Item ID")]
        public int ID;
        [JsonProperty(PropertyName = "Item Name")]
        public string Name;
        [JsonProperty(PropertyName = "Item Amount")]
        public int Amount;
        public Item();
        public Item(int id, string name, int amount);
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Loaded();
    [ChatCommand("rustyrockets")]
    private void RustyRocketsCmd(BasePlayer player);
    protected override void LoadDefaultMessages();
}

public class Configuration
{
    [JsonProperty(PropertyName = "Rocket Cooldown")]
    public int RocketCooldown;
    [JsonProperty(PropertyName = "Day (To Allow Spawning)")]
    public string Day;
    [JsonProperty(PropertyName = "Hour (To Allow Spawning)")]
    public string Hour;
    [JsonProperty(PropertyName = "Items (To Give Players)")]
    public Item[] Items;
    public static Configuration DefaultConfig();
}

public class Item
{
    [JsonProperty(PropertyName = "Item ID")]
    public int ID;
    [JsonProperty(PropertyName = "Item Name")]
    public string Name;
    [JsonProperty(PropertyName = "Item Amount")]
    public int Amount;
    public Item();
    public Item(int id, string name, int amount);
}


```

---

## SafeRecycler by Hovmodet - Prevents players from getting pushed away from recyclers in safezones

```csharp
using UnityEngine;
using Oxide.Game.Rust.Cui;
using System.Collections.Generic;

Oxide.Plugins
[Info("Safe Recycler", "Hovmodet", "1.0.1")]
[Description("Prevents players from getting pushed away from recyclers in safezones.")]
public class SafeRecycler : RustPlugin
{
    const string InvisChair;
     List<BasePlayer> RecyclingPlayers;
    private const string UsePermission;
    private void Init();
     void OnLootEntity(BasePlayer player, Recycler recycler);
     void OnLootEntityEnd(BasePlayer player, Recycler recycler);
    protected override void LoadDefaultMessages();
    private void LockToRecycler(BasePlayer player);
    private void UnlockFromRecycler(BasePlayer player);
     BaseMountable SpawnChair(Vector3 pos, Quaternion rotation);
    [ConsoleCommand("locktorecycler")]
    private void Locktorecycler(ConsoleSystem.Arg arg);
    private void DismountSafely(BasePlayer player, BaseMountable chair);
    private CuiElementContainer AddButton(BasePlayer player);
    private static CuiElementContainer CreateUI_recyclerLock();
}


```

---

## SafeTraps by VisEntities - Traps won't be triggered by the person placing them

```csharp
using System.Collections.Generic;
using System;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using UnityEngine;

Oxide.Plugins
[Info("SafeTraps", "Jake_Rich", "1.0.1")]
[Description("Players won't trigger their own traps")]
public class SafeTraps : RustPlugin
{
    public JSONFile<ConfigData> _settingsFile;
    public ConfigData Settings { get; set; }
     void Loaded();
    public class ConfigData
    {
        public float SafeTime;
        public bool UsePermissions;
    }

    public class EntityInfo
    {
        public ulong TrapOwner;
        public DateTime LastArmedTime;
    }

    private Dictionary<ulong, EntityInfo> _entityInfo;
    public EntityInfo GetEntityInfo(BaseNetworkable entity);
     object OnTrapTrigger(BaseEntity trap, GameObject go);
     void OnTrapArm(BearTrap trap, BasePlayer player);
     void OnEntityBuilt(Planner planner, GameObject obj);
    public class JSONFile
    {
        private DynamicConfigFile _file;
        public string _name { get; set; }
        public Type Instance { get; set; }
        private ConfigLocation _location { get; set; }
        private string _path { get; set; }
        public JSONFile(string name, ConfigLocation location, string path, string extension);
        public virtual void Init();
        public virtual void Load();
        public virtual void Save();
        public virtual void Reload();
        private void Unload(Plugin sender, PluginManager manager);
    }

}

public class ConfigData
{
    public float SafeTime;
    public bool UsePermissions;
}

public class EntityInfo
{
    public ulong TrapOwner;
    public DateTime LastArmedTime;
}

public class JSONFile
{
    private DynamicConfigFile _file;
    public string _name { get; set; }
    public Type Instance { get; set; }
    private ConfigLocation _location { get; set; }
    private string _path { get; set; }
    public JSONFile(string name, ConfigLocation location, string path, string extension);
    public virtual void Init();
    public virtual void Load();
    public virtual void Save();
    public virtual void Reload();
    private void Unload(Plugin sender, PluginManager manager);
}


```

---

## SafetyBarrel by VisEntities - Be invincible like a superhero but with a barrel on your head

```csharp
using Newtonsoft.Json;
using ProtoBuf;
using System.Collections.Generic;

Oxide.Plugins
[Info("Safety Barrel", "Dana", "1.0.0")]
[Description("Be invincible like a superhero but with a barrel on your head.")]
public class SafetyBarrel : RustPlugin
{
    private static SafetyBarrel _instance;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty(PropertyName = "Clothing")]
        public List<string> Clothing { get; set; }
        [JsonProperty(PropertyName = "Enable Damage Immunity")]
        public bool EnableDamageImmunity { get; set; }
        [JsonProperty(PropertyName = "Can Be Seen By NPC")]
        public bool CanBeSeenByNPC { get; set; }
        [JsonProperty(PropertyName = "Can Be Seen By Helicopter")]
        public bool CanBeSeenByHelicopter { get; set; }
        [JsonProperty(PropertyName = "Can Be Seen By Bradley")]
        public bool CanBeSeenByBradley { get; set; }
        [JsonProperty(PropertyName = "Can Be Seen By Trap")]
        public bool CanBeSeenByTrap { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private object OnEntityTakeDamage(BasePlayer player, HitInfo hitInfo);
    private object OnNpcPlayerTarget(ScientistNPC npc, BasePlayer targetPlayer);
    private object OnNpcTarget(ScientistNPC npc, BasePlayer targetPlayer);
    private object CanBeTargeted(BasePlayer player);
    private object CanBradleyApcTarget(BasePlayer player);
    private object CanHelicopterTarget(BasePlayer targetPlayer);
    private bool VerifyPlayerAttire(BasePlayer player);
    private static class PermissionUtils
    {
        public const string USE;
        public static void Register();
        public static bool Verify(BasePlayer player, string permissionName);
    }

}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty(PropertyName = "Clothing")]
    public List<string> Clothing { get; set; }
    [JsonProperty(PropertyName = "Enable Damage Immunity")]
    public bool EnableDamageImmunity { get; set; }
    [JsonProperty(PropertyName = "Can Be Seen By NPC")]
    public bool CanBeSeenByNPC { get; set; }
    [JsonProperty(PropertyName = "Can Be Seen By Helicopter")]
    public bool CanBeSeenByHelicopter { get; set; }
    [JsonProperty(PropertyName = "Can Be Seen By Bradley")]
    public bool CanBeSeenByBradley { get; set; }
    [JsonProperty(PropertyName = "Can Be Seen By Trap")]
    public bool CanBeSeenByTrap { get; set; }
}

private static class PermissionUtils
{
    public const string USE;
    public static void Register();
    public static bool Verify(BasePlayer player, string permissionName);
}


```

---

## SafeZoneHarvestBlock by gnif - Prevents players from being able to exploit the Safe Zones to farm

```csharp

Oxide.Plugins
[Info("Safe Zone Harvest Block", "gnif", "1.0.2")]
[Description("Prevents harvesting in the Safe Zones")]
internal class SafeZoneHarvestBlock : CovalencePlugin
{
    private const string BypassPerm;
    private void Init();
    private void OnTick();
}


```

---

## SAMSiteAuth by haggbart - Makes SAM Sites act in a similar fashion to shotgun traps and flame turrets

```csharp
using System.Collections.Generic;
using static BaseVehicle;

Oxide.Plugins
[Info("SAMSiteAuth", "haggbart", "2.4.3")]
[Description("Makes SAM Sites act in a similar fashion to shotgun traps and flame turrets.")]
internal class SAMSiteAuth : RustPlugin
{
    private readonly object True;
    private object OnSamSiteTarget(SamSite samSite, BaseCombatEntity target);
    private static bool IsOccupied(BaseCombatEntity entity, List<MountPointInfo> mountPoints);
    private static bool IsAuthed(BuildingPrivlidge cupboard, ulong userId);
}


```

---

## SamSiteMap by  - Marks all SAM Sites on the map

```csharp
using System.Collections.Generic;
using CompanionServer.Handlers;
using Network;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("SAM Site Map", "Arainrr", "1.4.0")]
[Description("Mark all SAM sites on the map")]
internal class SamSiteMap : RustPlugin
{
    private const string PERMISSION_USE;
    private const string PREFAB_MARKER;
    private const string PREFAB_TEXT;
    private static SamSiteMap _instance;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnEntitySpawned(SamSite samSite);
    private void CreateMapMarker(SamSite samSite);
    private SamSiteSettings GetSamSiteSettings(SamSite samSite);
    private class SamSiteMapMarker : MonoBehaviour
    {
        private static List<SamSiteMapMarker> _mapMarkers;
        public static void DestroyAll();
        public static void SendUpdateToPlayers();
        private static void SendSnapshotToPlayers();
        public static void SendSnapshotToPlayer(BasePlayer player);
        private SamSite _samSite;
        private MapMarkerGenericRadius _mapMarker;
        private MapMarkerGenericRadius _radiusMapMarker;
        private VendingMachineMapMarker _textMapMarker;
        private bool _usePermission;
        private SamSiteSettings _setting;
        private bool _tempCanShow;
        private void Awake();
        public void Init(SamSiteSettings settings, bool usePermission, float checkInterval);
        private void SpawnMapMarkers(SamSiteMarkerSetting settings, bool usePermission);
        private void TimedCheck();
        private bool CanSeeMapMarker();
        private void Show();
        private void Hide();
        private void EnableMarkers();
        private void DisableMarkers();
        private void LimitedMarkers();
        private void UnlimitedMarkers(Connection connection);
        private void OnDestroy();
    }

    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Use permission")]
        public bool usePermission;
        [JsonProperty(PropertyName = "Time interval to check the show of markers (seconds)")]
        public float checkInterval;
        [JsonProperty(PropertyName = "Static SAM settings")]
        public SamSiteSettings staticSamS;
        [JsonProperty(PropertyName = "Player's SAM settings")]
        public SamSiteSettings playerSamS;
    }

    private class SamSiteSettings
    {
        [JsonProperty(PropertyName = "Enabled map marker")]
        public bool enabled;
        [JsonProperty(PropertyName = "Hide when no power")]
        public bool hideWhenNoPower;
        [JsonProperty(PropertyName = "Hide when no ammo")]
        public bool hideWhenNoAmmo;
        [JsonProperty(PropertyName = "SAM map marker")]
        public SamSiteMarkerSetting samSiteMarkerSetting;
    }

    private class SamSiteMarkerSetting
    {
        [JsonProperty(PropertyName = "Map marker radius")]
        public float radius;
        [JsonProperty(PropertyName = "Map marker color1")]
        public string colorl;
        [JsonProperty(PropertyName = "Map marker color2")]
        public string color2;
        [JsonProperty(PropertyName = "Map marker alpha")]
        public float alpha;
        [JsonProperty(PropertyName = "Map marker text")]
        public string text;
        [JsonProperty(PropertyName = "SAM attack range map marker")]
        public SamSiteRadiusMarker samSiteRadiusMarker;
    }

    private class SamSiteRadiusMarker
    {
        [JsonProperty(PropertyName = "Enabled Sam attack range map marker")]
        public bool enabled;
        [JsonProperty(PropertyName = "Range map marker color1")]
        public string colorl;
        [JsonProperty(PropertyName = "Range map marker color2")]
        public string color2;
        [JsonProperty(PropertyName = "Range map marker alpha")]
        public float alpha;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class SamSiteMapMarker : MonoBehaviour
{
    private static List<SamSiteMapMarker> _mapMarkers;
    public static void DestroyAll();
    public static void SendUpdateToPlayers();
    private static void SendSnapshotToPlayers();
    public static void SendSnapshotToPlayer(BasePlayer player);
    private SamSite _samSite;
    private MapMarkerGenericRadius _mapMarker;
    private MapMarkerGenericRadius _radiusMapMarker;
    private VendingMachineMapMarker _textMapMarker;
    private bool _usePermission;
    private SamSiteSettings _setting;
    private bool _tempCanShow;
    private void Awake();
    public void Init(SamSiteSettings settings, bool usePermission, float checkInterval);
    private void SpawnMapMarkers(SamSiteMarkerSetting settings, bool usePermission);
    private void TimedCheck();
    private bool CanSeeMapMarker();
    private void Show();
    private void Hide();
    private void EnableMarkers();
    private void DisableMarkers();
    private void LimitedMarkers();
    private void UnlimitedMarkers(Connection connection);
    private void OnDestroy();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Use permission")]
    public bool usePermission;
    [JsonProperty(PropertyName = "Time interval to check the show of markers (seconds)")]
    public float checkInterval;
    [JsonProperty(PropertyName = "Static SAM settings")]
    public SamSiteSettings staticSamS;
    [JsonProperty(PropertyName = "Player's SAM settings")]
    public SamSiteSettings playerSamS;
}

private class SamSiteSettings
{
    [JsonProperty(PropertyName = "Enabled map marker")]
    public bool enabled;
    [JsonProperty(PropertyName = "Hide when no power")]
    public bool hideWhenNoPower;
    [JsonProperty(PropertyName = "Hide when no ammo")]
    public bool hideWhenNoAmmo;
    [JsonProperty(PropertyName = "SAM map marker")]
    public SamSiteMarkerSetting samSiteMarkerSetting;
}

private class SamSiteMarkerSetting
{
    [JsonProperty(PropertyName = "Map marker radius")]
    public float radius;
    [JsonProperty(PropertyName = "Map marker color1")]
    public string colorl;
    [JsonProperty(PropertyName = "Map marker color2")]
    public string color2;
    [JsonProperty(PropertyName = "Map marker alpha")]
    public float alpha;
    [JsonProperty(PropertyName = "Map marker text")]
    public string text;
    [JsonProperty(PropertyName = "SAM attack range map marker")]
    public SamSiteRadiusMarker samSiteRadiusMarker;
}

private class SamSiteRadiusMarker
{
    [JsonProperty(PropertyName = "Enabled Sam attack range map marker")]
    public bool enabled;
    [JsonProperty(PropertyName = "Range map marker color1")]
    public string colorl;
    [JsonProperty(PropertyName = "Range map marker color2")]
    public string color2;
    [JsonProperty(PropertyName = "Range map marker alpha")]
    public float alpha;
}


```

---

## SAMSiteRange by nivex - Allows configuring the lock-on range for SAM sites

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("SAM Site Range", "nivex", "1.2.8")]
[Description("Modifies SAM site range.")]
internal class SAMSiteRange : RustPlugin
{
    private void Init();
    private object OnSamSiteTargetScan(SamSite ss, List<SamSite.ISamSiteTarget> result);
    private void AddVehicleTargetSet(SamSite ss, List<SamSite.ISamSiteTarget> allTargets, float scanRadius);
    private void AddMLRSRockets(SamSite ss, List<SamSite.ISamSiteTarget> allTargets, float scanRadius);
    [PluginReference]
     Core.Plugins.Plugin RaidableBases;
    private bool RaidableTerritory(BaseEntity entity);
    private bool GetSamSiteScanRange(SamSite ss, float vehicleRange, float missileRange);
    private bool GetPermissionSettings(string playerId, PermissionSettings permissionSettings);
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Static SamSite Vehicle Scan Range")]
        public float staticVehicleRange;
        [JsonProperty(PropertyName = "Static SamSite Missile Scan Range")]
        public float staticMissileRange;
        [JsonProperty(PropertyName = "Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, PermissionSettings> permissions;
    }

    private class PermissionSettings
    {
        public int priority;
        public float vehicleScanRadius;
        public float missileScanRadius;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Static SamSite Vehicle Scan Range")]
    public float staticVehicleRange;
    [JsonProperty(PropertyName = "Static SamSite Missile Scan Range")]
    public float staticMissileRange;
    [JsonProperty(PropertyName = "Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, PermissionSettings> permissions;
}

private class PermissionSettings
{
    public int priority;
    public float vehicleScanRadius;
    public float missileScanRadius;
}


```

---

## SaveAnnouncer by Ryan - Announces to all players when the server saves

```csharp
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Save Announcer", "Ryan", "1.0.4")]
[Description("Announces to all players when the server saves")]
public class SaveAnnouncer : RustPlugin
{
    [PluginReference]
    private Plugin GUIAnnouncements;
    protected override void LoadDefaultConfig();
    private bool consoleAnnoucement();
    private bool entAnnoucement();
    private int entAmount();
    private bool guiAnnoucements();
    private string guiColor();
    private string guiTextColor();
    private string constructMsg();
    private new void LoadDefaultMessages();
    private void OnServerSave();
    private string Lang(string key, string id, object[] args);
}


```

---

## SaveMyMap by unboxingman - A save location bridge to support a better map save revision

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using Diag = System.Diagnostics;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("SaveMyMap", "un boxing man", "1.3.9")]
 class SaveMyMap : RustPlugin
{
     bool Changed;
     SaveRestore saveRestore;
     bool wasShutDown;
     int Rounds;
     bool Initialized;
     string saveFolder;
     bool loadReload;
     string [] saveFolders;
     int saveInterval;
     int saveCustomAfter;
     bool callOnServerSave;
     float delayCallOnServerSave;
     bool saveAfterLoadFile;
     bool allowOutOfDateSaves;
     bool enableLoadOverride;
     bool onServerSaveUseCoroutine;
     bool saveInData;
     int numberOfSaves;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
     void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
     void Loaded();
     void Unload();
     void OnServerInitialized();
     IEnumerator SaveLoop();
     void OnPluginUnloaded(Plugin name);
     void OnServerSave(object file);
     object OnSaveLoad(Dictionary<BaseEntity, ProtoBuf.Entity> dictionary);
     void OnNewSave(string strFilename);
     void CallOnServerSave();
     IEnumerator SaveCoroutine();
    [ConsoleCommand("smm.save")]
     void cMapSave(ConsoleSystem.Arg arg);
    [ConsoleCommand("server.savemymap")]
     void cMapServerSave(ConsoleSystem.Arg arg);
    [ConsoleCommand("smm.loadmap")]
     void cLoadMap(ConsoleSystem.Arg arg);
    [ConsoleCommand("smm.loadfile")]
     void cLoadFile(ConsoleSystem.Arg arg);
    [ConsoleCommand("smm.loadnamed")]
     void cLoadNamed(ConsoleSystem.Arg arg);
    [ConsoleCommand("smm.savefix")]
     void cLoadFix(ConsoleSystem.Arg arg);
     Int32 UnixTimeStampUTC();
     string [] SaveFolders();
     void SaveBackupCreate();
}


```

---

## SaveMyName by MikeLitoris - Allows saving of name, even when youre offline. (Imposters will be kicked)

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Save my Name", "MikeLitoris", "0.0.5")]
[Description("Allows saving of your name for online and offline protection")]
 class SaveMyName : RustPlugin
{
    private ConfigData configData;
    private const string savemyname;
    private const string savemynameadmin;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Admins can bypass the name-restrictions")]
        public bool IgnoreAdmins { get; set; }
    }

    protected override void LoadDefaultMessages();
    private bool LoadConfigVariables();
     void Init();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
     class SavedNames
    {
        public ulong SteamID { get; set; }
        public string Name { get; set; }
        public DateTime Timestamp { get; set; }
    }

     StoredData storedData;
     class StoredData
    {
        public List<SavedNames> savedNames;
    }

     void Loaded();
     void SaveData();
     void OnPlayerConnected(BasePlayer player);
     void OnUserNameUpdated(string id, string oldName, string newName);
    [ChatCommand("clearmyname")]
     void ClearName(BasePlayer player, string command, string[] args);
    [ChatCommand("savemyname")]
     void SaveName(BasePlayer player, string command, string[] args);
    [ConsoleCommand("savednames")]
     void SavedNamesListCon();
    [ChatCommand("savednames")]
     void SavedNamesList(BasePlayer player, string command, string[] args);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Admins can bypass the name-restrictions")]
    public bool IgnoreAdmins { get; set; }
}

 class SavedNames
{
    public ulong SteamID { get; set; }
    public string Name { get; set; }
    public DateTime Timestamp { get; set; }
}

 class StoredData
{
    public List<SavedNames> savedNames;
}


```

---

## ScheduledBuilding by 5Dev24 - Allows for prefabs to be spawned on a schedule

```csharp
using System.Collections.Generic;
using System.Collections;
using Newtonsoft.Json;
using UnityEngine;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Scheduled Building", "5Dev24", "1.0.1")]
[Description("Spawns prefabs on timers")]
public class ScheduledBuilding : RustPlugin
{
    private const string GetPositionPermission;
    private const string CreateNewPrefabPermission;
    private Coroutine routine;
    private ConfigData data;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    [ChatCommand("getposition")]
    private void GetPositionCommand(BasePlayer player, string cmd, string[] args);
    [ChatCommand("createprefab")]
    private void CreatePrefabCommand(BasePlayer player, string cmd, string[] args);
    [ChatCommand("showprefabs")]
    private void ShowPrefabsCommand(BasePlayer player, string cmd, string[] args);
    private void StartRoutine();
    private IEnumerator Spawn();
    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    internal class ConfigData
    {
        internal class PrefabData
        {
            [JsonConverter(typeof(Converter))]
            public Vector3 Location;
            [JsonConverter(typeof(Converter))]
            public Quaternion Rotation;
            public string Prefab;
            [JsonProperty("Spawn rate (in seconds)")]
            public uint Interval;
            [JsonProperty("Check for previous")]
            public bool PreviousCheck;
            [JsonProperty("Should save")]
            public bool ShouldSave;
            [JsonIgnore]
            public long NextSpawnAt;
            [JsonIgnore]
            public string Name;
        }

        public PrefabData[] Prefabs;
        public string Version;
    }

    private class Converter : JsonConverter
    {
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private long Now();
    private bool HasPermission(BasePlayer player, string permission);
}

internal class ConfigData
{
    internal class PrefabData
    {
        [JsonConverter(typeof(Converter))]
        public Vector3 Location;
        [JsonConverter(typeof(Converter))]
        public Quaternion Rotation;
        public string Prefab;
        [JsonProperty("Spawn rate (in seconds)")]
        public uint Interval;
        [JsonProperty("Check for previous")]
        public bool PreviousCheck;
        [JsonProperty("Should save")]
        public bool ShouldSave;
        [JsonIgnore]
        public long NextSpawnAt;
        [JsonIgnore]
        public string Name;
    }

    public PrefabData[] Prefabs;
    public string Version;
}

internal class PrefabData
{
    [JsonConverter(typeof(Converter))]
    public Vector3 Location;
    [JsonConverter(typeof(Converter))]
    public Quaternion Rotation;
    public string Prefab;
    [JsonProperty("Spawn rate (in seconds)")]
    public uint Interval;
    [JsonProperty("Check for previous")]
    public bool PreviousCheck;
    [JsonProperty("Should save")]
    public bool ShouldSave;
    [JsonIgnore]
    public long NextSpawnAt;
    [JsonIgnore]
    public string Name;
}

private class Converter : JsonConverter
{
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## ScheduledMessages by gunman435 - Set up messages that get broadcasted to your players at an interval

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Scheduled Messages", "gunman435", "1.2.0")]
[Description("Allows the creation of custom messages to broadcast to players.")]
 class ScheduledMessages : CovalencePlugin
{
    private Timer messageTimer;
    private System.Random rand;
    private int nextMessage;
    private Configuration config;
     class Configuration
    {
        [JsonProperty(PropertyName = "Scheduled Messages")]
        public List<string> scheduledMessages;
        [JsonProperty(PropertyName = "Scheduled Messages Interval")]
        public float scheduledMesssagesInterval;
        [JsonProperty(PropertyName = "Scheduled Messages Avatar ID")]
        public ulong scheduledMessagesAvatarID;
        [JsonProperty(PropertyName = "Scheduled Messages Randomizer")]
        public bool scheduledMessagesRandom;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
     void OnServerInitialized(bool initial);
     void Unload();
    private void StartScheduledMessages();
    private void StopScheduledMessages();
    private void NextScheduledMessage();
    [Command("scheduledmessages", "smsg"), Permission("scheduledmessages.cmd")]
    private void ScheduledMessagesCommand(IPlayer ply, string command, string[] args);
    private bool AddCommand(IPlayer ply, string command, string[] args);
    private bool RemoveCommand(IPlayer ply, string command, string[] args);
    private bool EditCommand(IPlayer ply, string command, string[] args);
    private bool ShowCommand(IPlayer ply, string command, string[] args);
    private bool SetAvatarCommand(IPlayer ply, string command, string[] args);
    private bool SetIntervalCommand(IPlayer ply, string command, string[] args);
    private bool OnCommand(IPlayer ply, string command, string[] args);
    private bool OffCommand(IPlayer ply, string command, string[] args);
    private bool RandomCommand(IPlayer ply, string command, string[] args);
    private bool HelpCommand(IPlayer ply, string command, string[] args);
    public List<string> API_GetScheduledMessagesList();
    public string[] API_GetScheduledMessages();
    public void API_BroadcastScheduledMessage();
    public bool API_BroadcastScheduledMessage(int index);
    public bool API_BroadcastScheduledMessage(int index, ulong avatarID);
    private void OnScheduledMessageBroadcasted(string message, ulong avatarID);
    private string Lang(string key, string id, object[] args);
    private void PrintToChat(IPlayer ply, string message, object[] args);
    private void BroadcastMessage(string message, ulong avatarID);
    private bool IsTimerRunning();
}

 class Configuration
{
    [JsonProperty(PropertyName = "Scheduled Messages")]
    public List<string> scheduledMessages;
    [JsonProperty(PropertyName = "Scheduled Messages Interval")]
    public float scheduledMesssagesInterval;
    [JsonProperty(PropertyName = "Scheduled Messages Avatar ID")]
    public ulong scheduledMessagesAvatarID;
    [JsonProperty(PropertyName = "Scheduled Messages Randomizer")]
    public bool scheduledMessagesRandom;
}


```

---

## ScheduledSpawns by 1AK1 - Spawn any items and prefabs on a schedule

```csharp
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using Oxide.Core;
using UnityEngine;
using System.Globalization;
using Newtonsoft.Json;
using WebSocketSharp;

Oxide.Plugins
[Info("Scheduled Spawns", "1AK1", "1.0.2")]
[Description("Spawn any item or prefab on a schedule")]
internal class ScheduledSpawns : CovalencePlugin
{
    private Dictionary<string, Timer> prefabTimers;
    private Dictionary<string, Timer> itemTimers;
    private const string permUse;
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Default timer (s)")]
        public float DefaultTimer { get; set; }
        [JsonProperty(PropertyName = "Check radius")]
        public float CheckRadius { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnNewSave(string file);
    private void OnServerInitialized(bool initial);
    private void SpawnPrefabsInData();
    private void SpawnItemsInData();
    private void SpawnPrefab(string prefabname, Vector3 position);
    private void SpawnItem(string itemname, Vector3 position);
    private void SpawnCommand(IPlayer player, string command, string[] args);
     List<T> FindEntities(Vector3 position, float distance);
    private static LayerMask GROUND_MASKS;
    static Vector3 GetGroundPosition(Vector3 sourcePos);
    public static Vector3 StringToVector3(string sVector);
    private void Message(IPlayer player, string key, object[] args);
    private string Lang(string key, string id, object[] args);
    private const string filename;
    private PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        public Dictionary<string, Dictionary<string, string>> _prefabContainer;
        public Dictionary<string, Dictionary<string, string>> _itemContainer;
    }

}

private class ConfigData
{
    [JsonProperty(PropertyName = "Default timer (s)")]
    public float DefaultTimer { get; set; }
    [JsonProperty(PropertyName = "Check radius")]
    public float CheckRadius { get; set; }
}

private class PluginData
{
    public Dictionary<string, Dictionary<string, string>> _prefabContainer;
    public Dictionary<string, Dictionary<string, string>> _itemContainer;
}


```

---

## ScientistBleed by birthdates - Scientist shots now make you bleed

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System.Collections.Generic;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("Scientist Bleed", "birthdates", "1.0.0")]
[Description("Scientist shots now make you bleed")]
public class ScientistBleed : RustPlugin
{
    private void Init();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    public ConfigFile _config;
    public class Bleed
    {
        public float minBleed;
        public float maxBleed;
    }

    public class ConfigFile
    {
        [JsonProperty("Indiviual Item Bleeds (Item shortnames)")]
        public Dictionary<string, Bleed> bleeds;
        [JsonProperty("Default Bleed for any item not found")]
        public Bleed defaultBleed;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class Bleed
{
    public float minBleed;
    public float maxBleed;
}

public class ConfigFile
{
    [JsonProperty("Indiviual Item Bleeds (Item shortnames)")]
    public Dictionary<string, Bleed> bleeds;
    [JsonProperty("Default Bleed for any item not found")]
    public Bleed defaultBleed;
    public static ConfigFile DefaultConfig();
}


```

---

## ScientistNames by  - Gives real names to scientists, bandits, tunnel dwellers etc. (instead of numbers)

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Scientist Names", "Ultra", "1.2.2")]
[Description("Gives real names to scientists, bandits, murderers, tunnel dwellers and scarecrows (instead of numbers)")]
 class ScientistNames : RustPlugin
{
     System.Random rnd;
     bool initialized;
     int renameCount;
     List<string> surnameList;
     List<string> currentActiveNames;
    private string additionPermission;
     void OnServerInitialized();
     void OnEntitySpawned(BaseEntity entity);
     void OnDeathNotice(Dictionary<string, object> data, string message);
    [ConsoleCommand("addname")]
     void AddNameConsoleCommand(ConsoleSystem.Arg arg);
    [ChatCommand("addname")]
     void AddNameChatCommand(BasePlayer player, string command, string[] args);
     void AddName(string name);
     void RenameBasePlayer(BasePlayer basePlayer, string abbrevation);
     string RenameBasePlayer(string abbrevation);
     string GetAbbrevation(BasePlayer basePlayer);
     string GetFirstnameFirstLetter();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "UseTitle")]
        public bool UseTitle;
        [JsonProperty(PropertyName = "UseFirstNameFirstLetter")]
        public bool UseFirstNameFirstLetter;
        [JsonProperty(PropertyName = "ScientistTitle")]
        public string ScientistTitle;
        [JsonProperty(PropertyName = "TunnelDwellerTitle")]
        public string TunnelDwellerTitle;
        [JsonProperty(PropertyName = "BanditTitle")]
        public string BanditTitle;
        [JsonProperty(PropertyName = "MurdererTitle")]
        public string MurdererTitle;
        [JsonProperty(PropertyName = "ScarecrowTitle")]
        public string ScarecrowTitle;
        [JsonProperty(PropertyName = "SurnameList")]
        public string SurnameList;
        [JsonProperty(PropertyName = "LogInFile")]
        public bool LogInFile;
        [JsonProperty(PropertyName = "LogInConsole")]
        public bool LogInConsole;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
     void Log(string message, bool console, LogType logType, string fileName);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "UseTitle")]
    public bool UseTitle;
    [JsonProperty(PropertyName = "UseFirstNameFirstLetter")]
    public bool UseFirstNameFirstLetter;
    [JsonProperty(PropertyName = "ScientistTitle")]
    public string ScientistTitle;
    [JsonProperty(PropertyName = "TunnelDwellerTitle")]
    public string TunnelDwellerTitle;
    [JsonProperty(PropertyName = "BanditTitle")]
    public string BanditTitle;
    [JsonProperty(PropertyName = "MurdererTitle")]
    public string MurdererTitle;
    [JsonProperty(PropertyName = "ScarecrowTitle")]
    public string ScarecrowTitle;
    [JsonProperty(PropertyName = "SurnameList")]
    public string SurnameList;
    [JsonProperty(PropertyName = "LogInFile")]
    public bool LogInFile;
    [JsonProperty(PropertyName = "LogInConsole")]
    public bool LogInConsole;
}


```

---

## Scoreboards by LaserHydra - Provides a simple scoreboard API system

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using System;

Oxide.Plugins
[Info("Scoreboards", "LaserHydra", "1.0.2")]
[Description("Provides a simple scoreboard API system")]
 class Scoreboards : RustPlugin
{
    static Scoreboards Instance;
    static ScoreboardData Data;
    static string AnchorMin;
    static string AnchorMax;
    static string BackgroundColor;
    static string HeaderColor;
    static string TitleColor;
    static string ContentColor;
    public class ScoreboardData
    {
        public string ActiveScoreboard;
        public List<Scoreboard> All;
    }

    public class Scoreboard
    {
        public static Scoreboard Find(string Title);
        public string Title;
        public string Description;
        internal bool Active { get; set; }
        internal KeyValuePair<string, string>[] Entries;
        public override int GetHashCode();
        public void Remove();
        public void SetActive();
        public void SetEntries(KeyValuePair<string, string>[] Entries);
        public static void SetActiveScoreboard(Scoreboard scoreboard);
        public static Scoreboard GetActiveScoreboard();
        public static void AddScoreboard(Scoreboard scoreboard);
    }

     void UpdateScoreboard(string ScoreboardTitle, KeyValuePair<string, string>[] Entries);
     void CreateScoreboard(string ScoreboardTitle, string ScoreboardDescription, KeyValuePair<string, string>[] Entries);
     void RemoveScoreboard(string ScoreboardTitle);
     void ScoreboardUpdated(Scoreboard scoreboard);
     void ActiveScoreboardChanged(Scoreboard scoreboard);
     void UpdateScoreboardUI();
     void DrawScoreboardUI(BasePlayer player);
    static CuiElementContainer GetCUI();
    [ChatCommand("scoreboard")]
     void cmdScoreboard(BasePlayer player, string cmd, string[] args);
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnPlayerSleepEnded(BasePlayer player);
    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
     string LangMsg(string key, object id, string[] replacements);
     DataValue ReadData(DataValue data, string filename);
     void WriteData(DataValue data, string filename);
     ConfigValue UpdateConfig(object[] fullPath);
}

public class ScoreboardData
{
    public string ActiveScoreboard;
    public List<Scoreboard> All;
}

public class Scoreboard
{
    public static Scoreboard Find(string Title);
    public string Title;
    public string Description;
    internal bool Active { get; set; }
    internal KeyValuePair<string, string>[] Entries;
    public override int GetHashCode();
    public void Remove();
    public void SetActive();
    public void SetEntries(KeyValuePair<string, string>[] Entries);
    public static void SetActiveScoreboard(Scoreboard scoreboard);
    public static Scoreboard GetActiveScoreboard();
    public static void AddScoreboard(Scoreboard scoreboard);
}


```

---

## ScrapHelicopterFix by  - Reduces lags of destroying scrap helicopter by removing effects of it

```csharp

Oxide.Plugins
[Info("Scrap Helicopter Fix", "Orange", "1.0.4")]
[Description("Reduces lags of destroying scrap helicopter by removing effects of it")]
public class ScrapHelicopterFix : RustPlugin
{
    private void OnServerInitialized();
    private void OnEntitySpawned(ScrapTransportHelicopter entity);
}


```

---

## ScrapHeliStorage by yetzt - Adds storage boxes to scrap transport helicopters

```csharp
using UnityEngine;
using Oxide.Core.Configuration;
using System.Linq;
using System;

Oxide.Plugins
[Info("Scrap Heli Storage", "yetzt", "0.0.5")]
[Description("Adds Storage Boxes to Scrap Transport Helicopters")]
public class ScrapHeliStorage : RustPlugin
{
    private string prefab;
    private PluginConfig config;
    private int num;
    private bool retrofit;
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private class PluginConfig
    {
        public bool Enabled;
        public int NumBoxes;
        public bool Retrofit;
    }

    private void Init();
     void OnServerInitialized();
     void RetrofitBoxes();
    private void OnEntitySpawned(ScrapTransportHelicopter entity);
    private void AddBoxes(ScrapTransportHelicopter entity);
    private void OnEntityKill(ScrapTransportHelicopter entity);
}

private class PluginConfig
{
    public bool Enabled;
    public int NumBoxes;
    public bool Retrofit;
}


```

---

## ScraponomicsLite by haggbart - Adds ATM UI with simple, intuitive functionality to vending machines and bandit vendors

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Scraponomics Lite", "haggbart", "1.0.0")]
[Description("Adds ATM UI with simple, intuitive functionality to vending machines and bandit vendors")]
internal class ScraponomicsLite : RustPlugin
{
    private const string LOC_PAID_BROKERAGE;
    private const string LOC_DEPOSIT;
    private const string LOC_WITHDRAW;
    private const string LOC_AMOUNT;
    private const string LOC_BALANCE;
    private const string LOC_ATM;
    private const string LOC_REWARD_INTEREST;
    protected override void LoadDefaultMessages();
    private void SaveData();
    private void ReadData();
    private static Dictionary<ulong, PlayerData> playerData;
    private static readonly Dictionary<ulong, PlayerPreference> playerPrefs;
    private class PlayerData
    {
        public int scrap { get; set; }
        public DateTime lastInterest;
    }

    private class PlayerPreference
    {
        public int amount { get; set; }
    }

    private PluginConfig config;
    private class PluginConfig
    {
        public float feesFraction;
        public int startingBalance;
        public bool allowPlayerVendingMachines;
        public bool resetOnMapWipe;
        public float interestRate;
    }

    protected override void LoadDefaultConfig();
    private new void SaveConfig();
    private static PluginConfig GetDefaultConfig();
    private void Init();
    private void InitPlayerData(BasePlayer player);
    private static void InitPlayerPerference(BasePlayer player);
    private void Unload();
    private void DoInterest(BasePlayer player);
    private void OnServerSave();
    private void OnNewSave(string filename);
    private void OnVendingShopOpened(VendingMachine machine, BasePlayer player);
    private void OnLootEntityEnd(BasePlayer player, VendingMachine machine);
    private static void DestroyGuiAll(BasePlayer player);
    private const int CUI_MAIN_FONTSIZE;
    private const string CUI_MAIN_FONT_COLOR;
    private const string CUI_GREEN_BUTTON_COLOR;
    private const string CUI_GREEN_BUTTON_FONT_COLOR;
    private const string CUI_GRAY_BUTTON_COLOR;
    private const string CUI_BUTTON_FONT_COLOR;
    private const string CUI_BANK_NAME;
    private const string CUI_BANK_HEADER_NAME;
    private const string CUI_BANK_CONTENT_NAME;
    private const string ANCHOR_MIN;
    private const string ANCHOR_MAX;
    private const string OFFSET_MIN;
    private const string OFFSET_MAX;
    private void CreateUi(BasePlayer player);
    [ConsoleCommand("sc.setamount")]
    private void CmdSetAmount(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.deposit")]
    private void CmdDeposit(ConsoleSystem.Arg arg);
    [ConsoleCommand("sc.withdraw")]
    private void CmdWithdraw(ConsoleSystem.Arg arg);
    private object SetBalance(ulong userId, int balance);
    private object GetBalance(ulong userId);
    private bool TryInitPlayer(ulong userId);
}

private class PlayerData
{
    public int scrap { get; set; }
    public DateTime lastInterest;
}

private class PlayerPreference
{
    public int amount { get; set; }
}

private class PluginConfig
{
    public float feesFraction;
    public int startingBalance;
    public bool allowPlayerVendingMachines;
    public bool resetOnMapWipe;
    public float interestRate;
}


```

---

## SearchLinkedAccounts by Farkas - Search DiscordAuth or DiscordCore linked accounts trough Discord

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Ext.Discord;
using Oxide.Ext.Discord.Attributes;
using Oxide.Ext.Discord.Builders;
using Oxide.Ext.Discord.Entities;
using Oxide.Ext.Discord.Entities.Activities;
using Oxide.Ext.Discord.Entities.Gatway;
using Oxide.Ext.Discord.Entities.Gatway.Commands;
using Oxide.Ext.Discord.Entities.Gatway.Events;
using Oxide.Ext.Discord.Entities.Guilds;
using Oxide.Ext.Discord.Entities.Messages;
using Oxide.Ext.Discord.Entities.Messages.Embeds;
using Oxide.Ext.Discord.Entities.Permissions;
using Oxide.Ext.Discord.Libraries.Command;
using Oxide.Ext.Discord.Libraries.Linking;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Search Linked Accounts", "Farkas", "1.0.1")]
[Description("Search DiscordAuth or DiscordCore linked accounts trough discord.")]
public class SearchLinkedAccounts : CovalencePlugin
{
    [PluginReference]
    private Plugin DiscordAuth;
    private Plugin DiscordCore;
    [DiscordClient]
    private DiscordClient _client;
    private DiscordGuild _guild;
    private DiscordRole _role;
    private readonly DiscordLink _link;
    private readonly DiscordCommand _dcCommands;
    private ConfigData _configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Bot token")]
        public string Token;
        [JsonProperty(PropertyName = "Discord Guild ID (optional if the bot is in one guild)")]
        public Snowflake GuildId { get; set; }
        [JsonProperty(PropertyName = "Discord Role ID that can use the command")]
        public Snowflake RoleId { get; set; }
        [JsonProperty(PropertyName = "Discord Channel ID where the command can be used")]
        public Snowflake ChannelID { get; set; }
        [JsonProperty(PropertyName = "Set custom status and activity for the discord bot")]
        public bool EnableCustomStatus;
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Bot's activity type: (Game, Listening, Watching, Competing)")]
        public ActivityType ActivityType;
        [JsonProperty(PropertyName = "Bot's Status")]
        public string Status;
        [JsonProperty(PropertyName = "Embed's color")]
        public string Color;
    }

    protected override void LoadConfig();
     void Init();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
    private void OnDiscordClientCreated();
    private void OnDiscordGatewayReady(GatewayReadyEvent ready);
     void OnDiscordGuildMembersLoaded(DiscordGuild guild);
     void SearchCommand(DiscordMessage message, string cmd, string[] args);
     void FindSteam(string discordID, DiscordMessage message);
     void FindDiscord(string steamID, DiscordMessage message);
    private string Lang(string key, string id);
    private DiscordEmbed CreateEmbed(string title, string message, string steamID, string discordID, string color);
    protected override void LoadDefaultMessages();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Bot token")]
    public string Token;
    [JsonProperty(PropertyName = "Discord Guild ID (optional if the bot is in one guild)")]
    public Snowflake GuildId { get; set; }
    [JsonProperty(PropertyName = "Discord Role ID that can use the command")]
    public Snowflake RoleId { get; set; }
    [JsonProperty(PropertyName = "Discord Channel ID where the command can be used")]
    public Snowflake ChannelID { get; set; }
    [JsonProperty(PropertyName = "Set custom status and activity for the discord bot")]
    public bool EnableCustomStatus;
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Bot's activity type: (Game, Listening, Watching, Competing)")]
    public ActivityType ActivityType;
    [JsonProperty(PropertyName = "Bot's Status")]
    public string Status;
    [JsonProperty(PropertyName = "Embed's color")]
    public string Color;
}


```

---

## SecureAdmin by MrBlue - Restricts select admin commands to players with permission

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Secure Admin", "Wulf/lukespragg", "1.1.8", ResourceId = 1449)]
[Description("Restricts the basic admin commands to players with permission")]
public class SecureAdmin : CovalencePlugin
{
    private const string permBan;
    private const string permKick;
    private const string permSay;
    private const string permUnban;
    private bool broadcastBans;
    private bool broadcastKicks;
    private bool commandBan;
    private bool commandKick;
    private bool commandSay;
    private bool commandUnban;
    private bool protectAdmin;
    protected override void LoadDefaultConfig();
    private void Init();
    protected override void LoadDefaultMessages();
    private void BanCommand(IPlayer player, string command, string[] args);
    private void KickCommand(IPlayer player, string command, string[] args);
    private void SayCommand(IPlayer player, string command, string[] args);
    private void UnbanCommand(IPlayer player, string command, string[] args);
    private T GetConfig(string name, T value);
    private string Lang(string key, string id, object[] args);
    private void Broadcast(string key, object[] args);
}


```

---

## SecurityCameras by Rick6 - CCTV targeting system that follows players, NPCs, and helicopters.

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Security Cameras", "Yurii,Rick", "0.1.1")]
[Description("CCTV targeting system, cameras follow players, npcs & heli")]
 class SecurityCameras : CovalencePlugin
{
    private const string ToggleManualControlUI;
    private readonly Dictionary<BasePlayer, SecurityCamera> PlayerUIStates;
     class ConfigData
    {
        [JsonProperty(PropertyName = "Detection Radius")]
        public int detection_radius;
    }

    static SecurityCameras instance;
    private ConfigData config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private const string UsePerm;
    private bool HasPerms(string userID, string perm);
    private bool HasPerms(ulong userID, string perm);
    private void Init();
     void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
     void OnGroupPermissionGranted(string name, string perm);
     void OnGroupPermissionRevoked(string name, string perm);
     void OnUserPermissionGranted(string id, string permName);
     void OnUserPermissionRevoked(string id, string permName);
     void OnBookmarkControlEnded(ComputerStation computerStation, BasePlayer player, BaseEntity controlledEntity);
     void OnBookmarkControlStarted(ComputerStation computerStation, BasePlayer player, string bookmarkName, IRemoteControllable remoteControllable);
     void Reload();
     void UpdateCameraUse(CCTV_RC camera);
     void Unload();
    [Command("toggle")]
    private void UICommandToggle(IPlayer player, string cmd, string[] args);
     void CreateButton(BasePlayer player, SecurityCamera camera);
     void DestroyButton(BasePlayer player);
    internal class SecurityCamera : MonoBehaviour
    {
        private ulong id;
        private CCTV_RC camera { get; set; }
        public BaseCombatEntity target;
        public bool scanning;
        private void Awake();
        public void ResetTarget();
        private void OnTriggerEnter(Collider range);
        private void OnTriggerStay(Collider range);
        private void OnTriggerExit(Collider range);
        private bool IsValid(BaseCombatEntity entity);
        public bool HasLoS(BaseCombatEntity entity);
        private bool ShouldTarget(BaseCombatEntity entity);
        private void SetTarget(BaseCombatEntity entity);
        public void DestroyCamera();
        public bool IsTargeting(BaseCombatEntity entity);
        private bool HasBuildingPrivilege(BasePlayer player);
        private object RaycastAll(Ray ray, float distance);
    }

}

 class ConfigData
{
    [JsonProperty(PropertyName = "Detection Radius")]
    public int detection_radius;
}

internal class SecurityCamera : MonoBehaviour
{
    private ulong id;
    private CCTV_RC camera { get; set; }
    public BaseCombatEntity target;
    public bool scanning;
    private void Awake();
    public void ResetTarget();
    private void OnTriggerEnter(Collider range);
    private void OnTriggerStay(Collider range);
    private void OnTriggerExit(Collider range);
    private bool IsValid(BaseCombatEntity entity);
    public bool HasLoS(BaseCombatEntity entity);
    private bool ShouldTarget(BaseCombatEntity entity);
    private void SetTarget(BaseCombatEntity entity);
    public void DestroyCamera();
    public bool IsTargeting(BaseCombatEntity entity);
    private bool HasBuildingPrivilege(BasePlayer player);
    private object RaycastAll(Ray ray, float distance);
}


```

---

## SecurityLights by S0N0FBISCUIT - Search light targeting system

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Rust;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Security Lights", "S0N_0F_BISCUIT", "1.1.11")]
[Description("Search light targeting system")]
 class SecurityLights : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin Friends;
     Plugin Vanish;
     class ConfigData
    {
        [JsonProperty("Detection Radius - All")]
        public int allDetectionRadius;
        [JsonProperty("Tracking Radius - All")]
        public int allTrackingRadius;
        [JsonProperty("Detection Radius - Player")]
        public int playerDetectionRadius;
        [JsonProperty("Tracking Radius - Player")]
        public int playerTrackingRadius;
        [JsonProperty("Detection Radius - Helicopter")]
        public int heliDetectionRadius;
        [JsonProperty("Tracking Radius - Helicopter")]
        public int heliTrackingRadius;
        [JsonProperty("Heli Mode - Target Minicopter and Scrap Heli")]
        public bool heliTargetVehicles;
        [JsonProperty("Auto Convert Lights When Placed")]
        public bool autoConvert;
        [JsonProperty("Require Power")]
        public bool requirePower;
        [JsonProperty("Night Only Operation")]
        public bool nightOnly;
        [JsonProperty("Target Acquired Sound")]
        public bool acquisitionSound;
        [JsonProperty("Target Friends")]
        public bool targetFriends;
        [JsonProperty("Target Team Members")]
        public bool targetTeamMembers;
    }

     class StoredData
    {
        public Dictionary<ulong, TargetMode> Security_Lights { get; set; }
    }

    static class Permissions
    {
        static readonly public string use;
    }

     class SecurityLight : MonoBehaviour
    {
        private ulong id;
        private SearchLight light { get; set; }
        private TargetMode mode { get; set; }
        private bool powered;
        public BaseCombatEntity target;
        private void Awake();
        private void OnTriggerEnter(Collider range);
        private void OnTriggerStay(Collider range);
        private void OnTriggerExit(Collider range);
        private void Update();
        public void OnDestroy();
        private bool ShouldTarget(BaseCombatEntity entity);
        private bool ShouldTargetPlayer(BasePlayer player);
        private void SetTarget(BaseCombatEntity entity);
        private void UpdateTarget();
        public void ResetTarget();
        public bool IsTargeting(BaseCombatEntity entity);
        private bool IsValid(BaseCombatEntity entity);
        private object RaycastAll(Ray ray, float distance);
        public bool HasLoS(BaseCombatEntity entity);
        public void DestroyLight();
        private float GetDetectionRadius();
        private float GetTrackingRadius();
        public ulong OwnerID();
        public ulong ID();
        public Vector3 Position();
        public void ChangeMode(TargetMode newMode);
        public TargetMode Mode();
        public void UpdateRadius();
        private bool HasBuildingPrivilege(BasePlayer player);
    }

    static SecurityLights instance;
    private ConfigData config;
    private StoredData data;
    private List<SecurityLight> securityLights;
    private bool lightsEnabled;
    private bool unloading;
    private new void LoadDefaultMessages();
    private void Init();
     void OnServerInitialized();
     void Unload();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void LoadData();
    private void SaveData();
    private void FindSecurityLights();
    private void ClearData();
    [ChatCommand("sl")]
     void ManageSecurityLight(BasePlayer player, string command, string[] args);
     void OnTimeSunset();
     void OnTimeSunrise();
     void OnEntitySpawned(BaseNetworkable entity);
     void OnEntityKill(BaseNetworkable entity);
    private string Lang(string key, string userId, object[] args);
    protected BasePlayer GetPlayer(ulong id);
     void UpdateLights();
    private bool IsOwnerTargeting(ulong OwnerID, BaseCombatEntity target);
    public bool IsAuthorized(BasePlayer player, SearchLight light);
    private object RaycastAll(Ray ray);
    private bool IsClosest(BaseCombatEntity target, ulong id);
    public bool IsInvisible(BasePlayer player);
    public bool IsDeveloper(BasePlayer player);
    public bool IsFriend(ulong owner, ulong target);
}

 class ConfigData
{
    [JsonProperty("Detection Radius - All")]
    public int allDetectionRadius;
    [JsonProperty("Tracking Radius - All")]
    public int allTrackingRadius;
    [JsonProperty("Detection Radius - Player")]
    public int playerDetectionRadius;
    [JsonProperty("Tracking Radius - Player")]
    public int playerTrackingRadius;
    [JsonProperty("Detection Radius - Helicopter")]
    public int heliDetectionRadius;
    [JsonProperty("Tracking Radius - Helicopter")]
    public int heliTrackingRadius;
    [JsonProperty("Heli Mode - Target Minicopter and Scrap Heli")]
    public bool heliTargetVehicles;
    [JsonProperty("Auto Convert Lights When Placed")]
    public bool autoConvert;
    [JsonProperty("Require Power")]
    public bool requirePower;
    [JsonProperty("Night Only Operation")]
    public bool nightOnly;
    [JsonProperty("Target Acquired Sound")]
    public bool acquisitionSound;
    [JsonProperty("Target Friends")]
    public bool targetFriends;
    [JsonProperty("Target Team Members")]
    public bool targetTeamMembers;
}

 class StoredData
{
    public Dictionary<ulong, TargetMode> Security_Lights { get; set; }
}

static class Permissions
{
    static readonly public string use;
}

 class SecurityLight : MonoBehaviour
{
    private ulong id;
    private SearchLight light { get; set; }
    private TargetMode mode { get; set; }
    private bool powered;
    public BaseCombatEntity target;
    private void Awake();
    private void OnTriggerEnter(Collider range);
    private void OnTriggerStay(Collider range);
    private void OnTriggerExit(Collider range);
    private void Update();
    public void OnDestroy();
    private bool ShouldTarget(BaseCombatEntity entity);
    private bool ShouldTargetPlayer(BasePlayer player);
    private void SetTarget(BaseCombatEntity entity);
    private void UpdateTarget();
    public void ResetTarget();
    public bool IsTargeting(BaseCombatEntity entity);
    private bool IsValid(BaseCombatEntity entity);
    private object RaycastAll(Ray ray, float distance);
    public bool HasLoS(BaseCombatEntity entity);
    public void DestroyLight();
    private float GetDetectionRadius();
    private float GetTrackingRadius();
    public ulong OwnerID();
    public ulong ID();
    public Vector3 Position();
    public void ChangeMode(TargetMode newMode);
    public TargetMode Mode();
    public void UpdateRadius();
    private bool HasBuildingPrivilege(BasePlayer player);
}


```

---

## ServerAnnouncer by austinv900 - Allows you to broadcast a message to the server with a custom prefix

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Server Announcer", "austinv900", "1.0.7")]
[Description("Allows you to send messages as the server with custom prefix")]
internal class ServerAnnouncer : CovalencePlugin
{
    private const string Permission;
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    [Command("say", "server.say")]
    private void Say(IPlayer player, string command, string[] args);
    private bool IsAdmin(IPlayer player);
    private string Lang(string key, IPlayer player, object[] args);
}


```

---

## ServerArmour by Pho3niX90 - Protects your gaming server against hackers, scripters, and more

```csharp
using ConVar;
using Facepunch;
using Facepunch.Extend;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Libraries;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;
using WebSocketSharp;
using Application = UnityEngine.Application;
using Time = Oxide.Core.Libraries.Time;

Oxide.Plugins
[Info("Server Armour", "Pho3niX90", "2.83.7")]
[Description("Protect your server! Auto ban known hackers, scripters and griefer accounts, and notify server owners of threats.")]
 class ServerArmour : CovalencePlugin
{
     bool isCarbon;
     string api_hostname;
     Dictionary<string, ISAPlayer> _playerData;
    private Time _time;
    private double cacheLifetime;
    private SAConfig config;
     string specifier;
     CultureInfo culture;
    const string DATE_FORMAT;
    const string DATE_FORMAT2;
    const string DATE_FORMAT_BAN;
     Regex logRegex;
     Regex logRegexNull;
     bool debug;
     bool apiConnected;
     bool serverStarted;
     int serverId;
    private Dictionary<string, string> headers;
     string adminIds;
     Timer updateTimer;
     Dictionary<string, byte[]> fileBackups;
     List<string> ignoredPlugins;
    private readonly Game.Rust.Libraries.Player Player;
    const string PermissionToBan;
    const string PermissionToUnBan;
    const string PermissionAdminWebsite;
    const string PermissionWhitelistRecentVacKick;
    const string PermissionWhitelistBadIPKick;
    const string PermissionWhitelistVacCeilingKick;
    const string PermissionWhitelistServerCeilingKick;
    const string PermissionWhitelistGameBanCeilingKick;
    const string PermissionWhitelistTotalBanCeiling;
    const string PermissionWhitelistSteamProfile;
    const string PermissionWhitelistFamilyShare;
    const string PermissionWhitelistTwitterBan;
    const string PermissionWhitelistAllowCountry;
    const string PermissionWhitelistAllowHighPing;
    const string DISCORD_INTRO_URL;
     Dictionary<string, string> requiredPlugins;
    [PluginReference]
     Plugin DiscordApi;
     Plugin DiscordMessages;
     Plugin BetterChat;
     Plugin Ember;
     Plugin Clans;
     Plugin AdminToggle;
     Plugin CombatLogInfo;
     Plugin ServerArmourUpdater;
     void DiscordSend(string steamId, string name, EmbedFieldList report, int color, bool isBan);
     void CheckPing(IPlayer player);
     void OnServerSave();
     void OnServerInitialized(bool first);
     void CheckServerConnection();
     void Unload();
     void OnUserConnected(IPlayer iPlayer);
     void DoConnectionChecks(IPlayer player, string type);
     int GetConnectedSeconds(IPlayer player);
     int GetConnectedSeconds(ISAPlayer player);
     void OnUserDisconnected(IPlayer player);
     void OnPluginLoaded(Plugin plugin);
     void OnPlayerKicked(BasePlayer player, string reason);
     void OnUserUnbanned(string name, string id, string ipAddress);
     void OnUserBanned(string name, string id, string ipAddress, string reason);
     void OnPlayerReported(BasePlayer reporter, string targetName, string targetId, string subject, string message, string type);
     void GetPlayerRiskScore(ulong steamId);
     void GetPlayerBans(IPlayer player);
     void GetPlayerBans(string playerId, string playerName);
     void WebCheckPlayer(string id, string address, bool connected);
     void KickIfBanned(ISAPlayer isaPlayer);
     void AddBan(ISABan thisBan, bool doNative);
    [Command("sa.apply_key")]
     void ApplyKey(IPlayer player, string command, string[] args);
    [Command("sa.clb", "getreport")]
     void SCmdCheckLocalBans(IPlayer player, string command, string[] args);
    [Command("unban", "playerunban", "sa.unban"), Permission(PermissionToUnBan)]
     void SCmdUnban(IPlayer player, string command, string[] args);
     void SilentBan(ulong steamId, TimeSpan timeSpan, string reason, IPlayer enforcer);
     void SilentBan(ulong steamId, int timeSpan, string reason, IPlayer enforcer);
     void SilentUnban(string playerId, IPlayer admin);
     void NativeBan(ulong playerId, string reason, long duration, string playerUsername, IPlayer player);
     bool NativeUnban(string playerId, IPlayer admin);
     void SaUnban(string playerId, IPlayer admin, string reason);
     void DoPardon(string playerId, IPlayer iPlayer, IPlayer admin);
    [Command("banid"), Permission(PermissionToBan)]
     void SCmdBanId(IPlayer player, string command, string[] args);
    [Command("ban", "playerban", "sa.ban"), Permission(PermissionToBan)]
     void SCmdBan(IPlayer player, string command, string[] args);
    [Command("clanban"), Permission(PermissionToBan)]
     void SCmdClanBan(IPlayer player, string command, string[] args);
    [Command("sa.cp")]
     void SCmdCheckPlayer(IPlayer player, string command, string[] args);
     string BanMinutes(DateTime ban);
     DateTime BanUntil(string banLength);
     string BanFor(string banLength);
     void RemoveBans(string id);
     bool BanPlayer(ISABan ban);
     void CheckOnlineUsers();
     void CheckLocalBans();
     bool IsPlayerDirty(string steamid);
     bool IsPlayerCached(string steamid);
     void AddPlayerCached(ISAPlayer isaplayer);
     ISAPlayer GetPlayerCache(string steamid);
     int GetPlayerBanDataCount(string steamid);
     void UpdatePlayerData(ISAPlayer isaplayer);
     void AddPlayerData(string id, ISABan isaban);
    static bool DateIsPast(DateTime to);
     double MinutesAgo(uint to);
     void GetPlayerReport(IPlayer player, IPlayer cmdplayer);
     void GetPlayerReport(ISAPlayer isaPlayer, bool isConnected, bool isCommand, IPlayer cmdPlayer);
    private void LogDebug(string txt);
     void LoadData();
     string ServerGetString();
    private void ServerStatusUpdate();
     void KickPlayer(string steamid, string reason, string type);
     bool IsBadIp(ISAPlayer isaPlayer);
     bool ContainsMyBan(string steamid);
     ISABan IsBanned(string steamid);
     int ServerBanCount(ISAPlayer player);
     int TotalBans(ISAPlayer isaPlayer);
     bool HasReachedVacCeiling(ISAPlayer isaPlayer);
     bool HasReachedGameBanCeiling(ISAPlayer isaPlayer);
     bool HasReachedTotalBanCeiling(ISAPlayer isaPlayer);
     bool HasReachedServerCeiling(ISAPlayer isaPlayer);
     bool IsProfilePrivate(string steamid);
     int GetProfileLevel(string steamid);
    private int API_GetServerBanCount(string steamid);
    private bool API_GetIsVacBanned(string steamid);
    private bool API_GetIsCommunityBanned(string steamid);
    private int API_GetVacBanCount(string steamid);
    private int API_GetDaysSinceLastVacBan(string steamid);
    private int API_GetGameBanCount(string steamid);
    private string API_GetEconomyBanStatus(string steamid);
    private bool API_GetIsPlayerDirty(string steamid);
    private bool API_GetIsProfilePrivate(string steamid);
    private int API_GetProfileLevel(string steamid);
    private void API_BanPlayer(IPlayer player, string playerNameId, string reason, string length, bool ignoreSearch);
     string GetMsg(string msg, Dictionary<string, string> rpls);
    protected override void LoadDefaultMessages();
     string GetChatTag();
     void RegisterTag();
     string GetTag(IPlayer player);
    private void HandleLog(string message, string stackTrace, LogType type);
     void SendReplyWithIcon(IPlayer player, string format, object[] args);
     void BroadcastWithIcon(string format, object[] args);
     string FixColors(string msg);
     void AssignGroup(string id, string group);
     bool HasGroup(string id, string group);
     void RegPerm(string perm);
     bool HasPerm(string id, string perm);
     void GrantPerm(string id, string perm);
    private static readonly DateTime Epoch;
    private static uint ConvertToTimestamp(string value);
    private static uint ConvertToTimestamp(DateTime value);
    private static DateTime ConverToDateTime(string stringDate);
    private static DateTime ConvertUnixToDateTime(long unixTimeStamp);
    public class EmbedFieldList
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
    }

    public class ISAPlayer
    {
        public int id { get; set; }
        public string steamid { get; set; }
        public int? steamlevel { get; set; }
        public int steamCommunityBanned { get; set; }
        public int steamVACBanned { get; set; }
        public int steamNumberOfVACBans { get; set; }
        public int steamDaysSinceLastBan { get; set; }
        public int steamNumberOfGameBans { get; set; }
        public string steamEconomyBan { get; set; }
        public int communityvisibilitystate { get; set; }
        public string personaname { get; set; }
        public List<EacBan> eacBans { get; set; }
        public uint? cacheTimestamp { get; set; }
        public uint? lastConnected { get; set; }
        public List<ISABan> bans { get; set; }
        public IPInfo ipInfo { get; set; }
        public ISAPlayer(ulong steamId);
    }

    public class IPInfo
    {
        public string ip;
        public string lastcheck;
        public long longIp;
        public string asn;
        public string provider;
        public string continent;
        public string country;
        public string isocode;
        public string region;
        public string regioncode;
        public string city;
        public string latitude;
        public string longitude;
        public string proxy;
        public string type;
        public float rating;
        public bool isCloudComputing;
    }

    public class EacBan
    {
        public string id;
        public string steamid;
        public string createdAt;
        public string lastChecked;
        public string text;
        public string steamProfile;
        public bool isCron;
        public bool isTemp;
    }

    public class ISABan
    {
        public int id;
        public string adminSteamId;
        public int serverId;
        public ulong steamid;
        public ulong bannedBy;
        public string reason;
        public string banLength;
        public string serverName;
        public string serverIp;
        public string dateTime;
        public string created;
        public int gameId;
        public string? banUntil;
        public uint GetUnixBanUntill();
        public DateTime BanUntillDateTime();
        public bool IsBanned();
    }

    private void API_EspDetected(string jString);
    private void API_StashFoundTrigger(string jString);
    private void API_ArkanOnNoRecoilViolation(BasePlayer player, int NRViolationsNum, string jString);
    private void API_ArkanOnAimbotViolation(BasePlayer player, int AIMViolationsNum, string jString);
    private void TranslateCode(int statusCode);
    private void GetJson(string url, Action<int, JObject> callback);
    private void DoRequest(string url, string body, Action<int, string> callback, RequestMethod requestType, int retryInSeconds, int retryCounter);
    private void CalcElo(string steamIdKiller, string steamIdVictim, string killInfo, Action<int, string> callback, int retryInSeconds);
    private void FetchElo(string steamId);
    private void FetchEloUpdate(string steamId, Action<int, string> callback, int retryInSeconds);
    private void UploadCombatEntries(string logEntries);
     List<ulong> GetTeamMembers(ulong userid);
     string GetClanTag(string userid);
     List<ulong> GetClan(string userid);
    private class SAConfig
    {
        public string ServerIp;
        public bool Debug;
        public bool ShowProtectedMsg;
        public bool AutoKickOn;
        public bool EnableTotalBanKick;
        public int AutoKickCeiling;
        public int AutoVacBanCeiling;
        public int AutoGameBanCeiling;
        public int AutoTotalBanCeiling;
        public int DissallowVacBanDays;
        public int BroadcastPlayerBanReportVacDays;
        public bool AutoKickFamilyShare;
        public bool AutoKickFamilyShareIfDirty;
        public string BetterChatDirtyPlayerTag;
        public bool BroadcastPlayerBanReport;
        public bool BroadcastNewBans;
        public bool BroadcastKicks;
        public bool ServerAdminShareDetails;
        public string ServerAdminName;
        public string ServerAdminEmail;
        public string ServerApiKey;
        public string SteamApiKey;
        public bool AutoKick_KickHiddenLevel;
        public int AutoKick_MinSteamProfileLevel;
        public bool AutoKick_KickPrivateProfile;
        public bool AutoKick_KickWeirdSteam64;
        public bool AutoKick_KickTwitterGameBanned;
        public bool AutoKick_BadIp;
        public bool AutoKick_BadIp_IgnoreComputing;
        public string DiscordWebhookURL;
        public string DiscordBanWebhookURL;
        public bool DiscordQuickConnect;
        public bool DiscordOnlySendDirtyReports;
        public bool DiscordJoinReports;
        public bool DiscordKickReport;
        public bool DiscordBanReport;
        public bool DiscordNotifyGameBan;
        public bool SubmitArkanData;
        public bool RconBroadcast;
        public bool AutoKick_IgnoreNvidia;
        public bool AutoKick_NetworkBan;
        public bool AutoKick_ActiveBans;
        public bool AutoKick_SameServerIp;
        public string OwnerSteamId;
        public string ClanBanPrefix;
        public bool ClanBanTeams;
        public bool IgnoreAdmins;
        public bool UseEloSystem;
        public int AutoKickMaxPing;
        public string AutoKickLimitCountry;
        public string IconSteamId;
        public bool IgnoreCheatDetected;
        private ServerArmour _plugin;
        public SAConfig(ServerArmour plugin);
        private void GetConfig(T variable, string[] path);
        public void SetConfig(T variable, string[] path);
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
}

public class EmbedFieldList
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
}

public class ISAPlayer
{
    public int id { get; set; }
    public string steamid { get; set; }
    public int? steamlevel { get; set; }
    public int steamCommunityBanned { get; set; }
    public int steamVACBanned { get; set; }
    public int steamNumberOfVACBans { get; set; }
    public int steamDaysSinceLastBan { get; set; }
    public int steamNumberOfGameBans { get; set; }
    public string steamEconomyBan { get; set; }
    public int communityvisibilitystate { get; set; }
    public string personaname { get; set; }
    public List<EacBan> eacBans { get; set; }
    public uint? cacheTimestamp { get; set; }
    public uint? lastConnected { get; set; }
    public List<ISABan> bans { get; set; }
    public IPInfo ipInfo { get; set; }
    public ISAPlayer(ulong steamId);
}

public class IPInfo
{
    public string ip;
    public string lastcheck;
    public long longIp;
    public string asn;
    public string provider;
    public string continent;
    public string country;
    public string isocode;
    public string region;
    public string regioncode;
    public string city;
    public string latitude;
    public string longitude;
    public string proxy;
    public string type;
    public float rating;
    public bool isCloudComputing;
}

public class EacBan
{
    public string id;
    public string steamid;
    public string createdAt;
    public string lastChecked;
    public string text;
    public string steamProfile;
    public bool isCron;
    public bool isTemp;
}

public class ISABan
{
    public int id;
    public string adminSteamId;
    public int serverId;
    public ulong steamid;
    public ulong bannedBy;
    public string reason;
    public string banLength;
    public string serverName;
    public string serverIp;
    public string dateTime;
    public string created;
    public int gameId;
    public string? banUntil;
    public uint GetUnixBanUntill();
    public DateTime BanUntillDateTime();
    public bool IsBanned();
}

private class SAConfig
{
    public string ServerIp;
    public bool Debug;
    public bool ShowProtectedMsg;
    public bool AutoKickOn;
    public bool EnableTotalBanKick;
    public int AutoKickCeiling;
    public int AutoVacBanCeiling;
    public int AutoGameBanCeiling;
    public int AutoTotalBanCeiling;
    public int DissallowVacBanDays;
    public int BroadcastPlayerBanReportVacDays;
    public bool AutoKickFamilyShare;
    public bool AutoKickFamilyShareIfDirty;
    public string BetterChatDirtyPlayerTag;
    public bool BroadcastPlayerBanReport;
    public bool BroadcastNewBans;
    public bool BroadcastKicks;
    public bool ServerAdminShareDetails;
    public string ServerAdminName;
    public string ServerAdminEmail;
    public string ServerApiKey;
    public string SteamApiKey;
    public bool AutoKick_KickHiddenLevel;
    public int AutoKick_MinSteamProfileLevel;
    public bool AutoKick_KickPrivateProfile;
    public bool AutoKick_KickWeirdSteam64;
    public bool AutoKick_KickTwitterGameBanned;
    public bool AutoKick_BadIp;
    public bool AutoKick_BadIp_IgnoreComputing;
    public string DiscordWebhookURL;
    public string DiscordBanWebhookURL;
    public bool DiscordQuickConnect;
    public bool DiscordOnlySendDirtyReports;
    public bool DiscordJoinReports;
    public bool DiscordKickReport;
    public bool DiscordBanReport;
    public bool DiscordNotifyGameBan;
    public bool SubmitArkanData;
    public bool RconBroadcast;
    public bool AutoKick_IgnoreNvidia;
    public bool AutoKick_NetworkBan;
    public bool AutoKick_ActiveBans;
    public bool AutoKick_SameServerIp;
    public string OwnerSteamId;
    public string ClanBanPrefix;
    public bool ClanBanTeams;
    public bool IgnoreAdmins;
    public bool UseEloSystem;
    public int AutoKickMaxPing;
    public string AutoKickLimitCountry;
    public string IconSteamId;
    public bool IgnoreCheatDetected;
    private ServerArmour _plugin;
    public SAConfig(ServerArmour plugin);
    private void GetConfig(T variable, string[] path);
    public void SetConfig(T variable, string[] path);
}


```

---

## ServerBanCheck by MrBlue - Checks if the server IP address has been banned with Facepunch

```csharp
using System.Collections.Generic;
using Facepunch;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Server Ban Check", "Wulf", "0.0.2")]
[Description("Checks if the server IP address has been banned with Facepunch")]
public class ServerBanCheck : CovalencePlugin
{
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void CommandBanCheck(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private bool IsServerBanned();
}


```

---

## ServerChat by  - Replaces the default chat icon and prefix of the default server chat messages

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Server Chat", "Enforcer", "2.0.2")]
[Description("Replaces the default server chat icon and prefix")]
public class ServerChat : RustPlugin
{
     ConfigData config;
    public class ConfigData
    {
        [JsonProperty(PropertyName = "Chat icon (SteamID64)")]
        public ulong chatIcon { get; set; }
        [JsonProperty(PropertyName = "Messages to not modify", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> messagesToNotModify { get; set; }
        [JsonProperty(PropertyName = "Title Settings")]
        public ServerTitle serverTitleSettings { get; set; }
        [JsonProperty(PropertyName = "Message Settings")]
        public ServerMessage serverMessageSettings { get; set; }
        [JsonProperty(PropertyName = "Format")]
        public ServerFormat serverFormatSettings { get; set; }
    }

    public class ServerTitle
    {
        [JsonProperty(PropertyName = "Title")]
        public string titleName { get; set; }
        [JsonProperty(PropertyName = "Colour")]
        public string titleColour { get; set; }
        [JsonProperty(PropertyName = "Size")]
        public int titleSize { get; set; }
    }

    public class ServerMessage
    {
        [JsonProperty(PropertyName = "Colour")]
        public string messageColour { get; set; }
        [JsonProperty(PropertyName = "Size")]
        public int messageSize { get; set; }
    }

    public class ServerFormat
    {
        [JsonProperty(PropertyName = "Server chat format")]
        public string messageFormat { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private object OnServerMessage(string serverMessage);
}

public class ConfigData
{
    [JsonProperty(PropertyName = "Chat icon (SteamID64)")]
    public ulong chatIcon { get; set; }
    [JsonProperty(PropertyName = "Messages to not modify", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> messagesToNotModify { get; set; }
    [JsonProperty(PropertyName = "Title Settings")]
    public ServerTitle serverTitleSettings { get; set; }
    [JsonProperty(PropertyName = "Message Settings")]
    public ServerMessage serverMessageSettings { get; set; }
    [JsonProperty(PropertyName = "Format")]
    public ServerFormat serverFormatSettings { get; set; }
}

public class ServerTitle
{
    [JsonProperty(PropertyName = "Title")]
    public string titleName { get; set; }
    [JsonProperty(PropertyName = "Colour")]
    public string titleColour { get; set; }
    [JsonProperty(PropertyName = "Size")]
    public int titleSize { get; set; }
}

public class ServerMessage
{
    [JsonProperty(PropertyName = "Colour")]
    public string messageColour { get; set; }
    [JsonProperty(PropertyName = "Size")]
    public int messageSize { get; set; }
}

public class ServerFormat
{
    [JsonProperty(PropertyName = "Server chat format")]
    public string messageFormat { get; set; }
}


```

---

## ServerInfo by FastBurst - Adds an in-game UI with custom text and images

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Server Info", "FastBurst", "0.5.9")]
[Description("UI customizable server info with multiple tabs")]
public sealed class ServerInfo : RustPlugin
{
    private static Settings _settings;
    private static readonly Dictionary<ulong, PlayerInfoState> PlayerActiveTabs;
    private static readonly Permission Permission;
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
     void Unload();
    [ConsoleCommand("changetab")]
    private void ChangeTab(ConsoleSystem.Arg arg);
    [ConsoleCommand("changepage")]
    private void ChangePage(ConsoleSystem.Arg arg);
    [ConsoleCommand("infoclose")]
    private void CloseInfo(ConsoleSystem.Arg arg);
    private void OnPlayerConnected(BasePlayer player);
     List<ulong> checkedPlayers;
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private static void AddHelpButton(BasePlayer player);
    [ConsoleCommand("info")]
    private void ShowConsoleInfo(ConsoleSystem.Arg arg);
    [ChatCommand("info")]
    private void ShowInfo(BasePlayer player, string command, string[] args);
    private static void SendUI(BasePlayer player, CuiElementContainer container);
    private static string AddMainPanel(CuiElementContainer container);
    private static CuiElement CreateImage(string panelName, ImageSettings settings);
    private string CreateTabContent(HelpTab helpTab, CuiElementContainer container, string mainPanelName, int pageIndex);
    private static string CreateTab(HelpTab helpTab, CuiElementContainer container, string mainPanelName, int pageIndex);
    private static string CreateTabPanel(CuiElementContainer container, string mainPanelName, string hexColor);
    private static string CreateTabContentPanel(CuiElementContainer container, string mainPanelName, string hexColor);
    private static CuiLabel CreateHeaderLabel(HelpTab helpTab);
    private static CuiButton CreateCloseButton(string mainPanelName, string hexColor);
    private static CuiButton CreateHelpButton();
    private static CuiLabel CreateTextLineLabel(HelpTab helpTab, float firstLineMargin, float textLineHeight, int textRow, string textLine);
    private static CuiButton CreatePrevPageButton(string mainPanelName, int pageIndex, string hexColor);
    private static CuiButton CreateNextPageButton(string mainPanelName, int pageIndex, string hexColor);
    private static void AddNonActiveButton(int tabIndex, CuiElementContainer container, HelpTab helpTab, string mainPanelName, string activeTabButtonName);
    private static string AddActiveButton(int activeTabIndex, HelpTab activeTab, CuiElementContainer container, string mainPanelName);
    private static CuiButton CreateTabButton(int tabIndex, HelpTab helpTab, Color color);
    [JsonObject]
    public sealed class Settings
    {
        public Settings();
        public List<HelpTab> Tabs { get; set; }
        public bool ShowInfoOnPlayerInit { get; set; }
        public bool ShowInfoOnlyOncePerRuntime { get; set; }
        public int TabToOpenByDefault { get; set; }
        public Position Position { get; set; }
        public BackgroundImageSettings BackgroundImage { get; set; }
        public string ActiveButtonColor { get; set; }
        public string InactiveButtonColor { get; set; }
        public string CloseButtonColor { get; set; }
        public string CloseButtonText { get; set; }
        public string NextPageButtonColor { get; set; }
        public string NextPageText { get; set; }
        public string PrevPageButtonColor { get; set; }
        public string PrevPageText { get; set; }
        public string BackgroundColor { get; set; }
        public HelpButtonSettings HelpButton { get; set; }
        public bool UpgradedConfig { get; set; }
        public static Settings CreateDefault();
    }

    public sealed class Position
    {
        public Position();
        public float MinX { get; set; }
        public float MaxX { get; set; }
        public float MinY { get; set; }
        public float MaxY { get; set; }
        public string GetRectTransformAnchorMin();
        public string GetRectTransformAnchorMax();
    }

    public sealed class HelpTab
    {
        private string _headerText;
        private string _buttonText;
        public HelpTab();
        public string ButtonText { get; set; }
        public string HeaderText { get; set; }
        public List<HelpTabPage> Pages { get; set; }
        public TextAnchor TabButtonAnchor { get; set; }
        public int TabButtonFontSize { get; set; }
        public TextAnchor HeaderAnchor { get; set; }
        public int HeaderFontSize { get; set; }
        public int TextFontSize { get; set; }
        public TextAnchor TextAnchor { get; set; }
        public string OxideGroup { get; set; }
    }

    public sealed class HelpTabPage
    {
        public List<string> TextLines { get; set; }
        public List<ImageSettings> ImageSettings { get; set; }
        public HelpTabPage();
    }

    public class ImageSettings
    {
        public Position Position { get; set; }
        public string Url { get; set; }
        public int TransparencyInPercent { get; set; }
        public ImageSettings();
    }

    public sealed class BackgroundImageSettings : ImageSettings
    {
        public bool Enabled { get; set; }
        public BackgroundImageSettings();
    }

    public sealed class HelpButtonSettings
    {
        public bool IsEnabled { get; set; }
        public string Text { get; set; }
        public Position Position { get; set; }
        public string Color { get; set; }
        public int FontSize { get; set; }
        public HelpButtonSettings();
    }

    public sealed class PlayerInfoState
    {
        public PlayerInfoState(Settings settings);
        public int ActiveTabIndex { get; set; }
        public int PageIndex { get; set; }
        public bool InfoShownOnLogin { get; set; }
        public bool InfoShownOnLoginOnce { get; set; }
        public string ActiveTabContentPanelName { get; set; }
        public string ChatHelpButtonName { get; set; }
        public string MainPanelName { get; set; }
    }

    public static class ColorExtensions
    {
        public static string ToRustFormatString(Color color);
        public static string ToHexStringRGB(Color col);
        public static string ToHexStringRGBA(Color col);
        public static bool TryParseHexString(string hexString, Color color);
        private static Color FromHexString(string hexString);
    }

}

[JsonObject]
public sealed class Settings
{
    public Settings();
    public List<HelpTab> Tabs { get; set; }
    public bool ShowInfoOnPlayerInit { get; set; }
    public bool ShowInfoOnlyOncePerRuntime { get; set; }
    public int TabToOpenByDefault { get; set; }
    public Position Position { get; set; }
    public BackgroundImageSettings BackgroundImage { get; set; }
    public string ActiveButtonColor { get; set; }
    public string InactiveButtonColor { get; set; }
    public string CloseButtonColor { get; set; }
    public string CloseButtonText { get; set; }
    public string NextPageButtonColor { get; set; }
    public string NextPageText { get; set; }
    public string PrevPageButtonColor { get; set; }
    public string PrevPageText { get; set; }
    public string BackgroundColor { get; set; }
    public HelpButtonSettings HelpButton { get; set; }
    public bool UpgradedConfig { get; set; }
    public static Settings CreateDefault();
}

public sealed class Position
{
    public Position();
    public float MinX { get; set; }
    public float MaxX { get; set; }
    public float MinY { get; set; }
    public float MaxY { get; set; }
    public string GetRectTransformAnchorMin();
    public string GetRectTransformAnchorMax();
}

public sealed class HelpTab
{
    private string _headerText;
    private string _buttonText;
    public HelpTab();
    public string ButtonText { get; set; }
    public string HeaderText { get; set; }
    public List<HelpTabPage> Pages { get; set; }
    public TextAnchor TabButtonAnchor { get; set; }
    public int TabButtonFontSize { get; set; }
    public TextAnchor HeaderAnchor { get; set; }
    public int HeaderFontSize { get; set; }
    public int TextFontSize { get; set; }
    public TextAnchor TextAnchor { get; set; }
    public string OxideGroup { get; set; }
}

public sealed class HelpTabPage
{
    public List<string> TextLines { get; set; }
    public List<ImageSettings> ImageSettings { get; set; }
    public HelpTabPage();
}

public class ImageSettings
{
    public Position Position { get; set; }
    public string Url { get; set; }
    public int TransparencyInPercent { get; set; }
    public ImageSettings();
}

public sealed class BackgroundImageSettings : ImageSettings
{
    public bool Enabled { get; set; }
    public BackgroundImageSettings();
}

public sealed class HelpButtonSettings
{
    public bool IsEnabled { get; set; }
    public string Text { get; set; }
    public Position Position { get; set; }
    public string Color { get; set; }
    public int FontSize { get; set; }
    public HelpButtonSettings();
}

public sealed class PlayerInfoState
{
    public PlayerInfoState(Settings settings);
    public int ActiveTabIndex { get; set; }
    public int PageIndex { get; set; }
    public bool InfoShownOnLogin { get; set; }
    public bool InfoShownOnLoginOnce { get; set; }
    public string ActiveTabContentPanelName { get; set; }
    public string ChatHelpButtonName { get; set; }
    public string MainPanelName { get; set; }
}

public static class ColorExtensions
{
    public static string ToRustFormatString(Color color);
    public static string ToHexStringRGB(Color col);
    public static string ToHexStringRGBA(Color col);
    public static bool TryParseHexString(string hexString, Color color);
    private static Color FromHexString(string hexString);
}


```

---

## ServerRewards by k1lly0u - Earn reward points in various ways and spend them on many different options in the GUI reward store

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using Network;
using UnityEngine;

Oxide.Plugins
[Info("Server Rewards", "k1lly0u", "0.4.78")]
[Description("UI shop to buy items, kits, and commands")]
 class ServerRewards : RustPlugin
{
    [PluginReference]
     Plugin Kits;
     Plugin Economics;
     Plugin EventManager;
     Plugin HumanNPC;
     Plugin LustyMap;
     Plugin PlaytimeTracker;
     Plugin ImageLibrary;
    private PlayerData playerData;
    private NPCData npcData;
    private RewardData rewardData;
    private SaleData saleData;
    private CooldownData cooldownData;
    private DynamicConfigFile playerdata;
    private DynamicConfigFile npcdata;
    private DynamicConfigFile rewarddata;
    private DynamicConfigFile saledata;
    private DynamicConfigFile cooldowndata;
    private static ServerRewards ins;
    private UIManager uiManager;
    private Timer saveTimer;
    private static bool uiFadeIn;
    private bool isILReady;
    private string color1;
    private string color2;
    private int blueprintId;
    private Dictionary<string, string> uiColors;
    private Dictionary<ulong, int> playerRP;
    private Hash<ulong, Timer> popupMessages;
    private Dictionary<int, string> itemIds;
    private Dictionary<string, string> itemNames;
    private Dictionary<ulong, KeyValuePair<string, NPCData.NPCInfo>> npcCreator;
    private Dictionary<ulong, UserNPC> userNpc;
    const string UIMain;
    const string UISelect;
    const string UIRP;
    const string UIPopup;
    private class UI
    {
        public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax);
        public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        public static void CreateLabel(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align, string color, float fadein);
        public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
        public static void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        public static void LoadImage(CuiElementContainer container, string panel, int itemid, ulong skinid, string aMin, string aMax);
        public static string Color(string hexColor, float alpha);
    }

    private class UIManager
    {
        public Dictionary<UIPanel, Dictionary<Category, Dictionary<int, CuiElementContainer>>> standardElements;
        public Dictionary<string, Dictionary<UIPanel, Dictionary<Category, Dictionary<int, CuiElementContainer>>>> npcElements;
        public Dictionary<ulong, PlayerUI> playerUi;
        public void AddUI(BasePlayer player, UIPanel type, Category subType, int pageNumber, string npcId);
        public void DestroyUI(BasePlayer player, bool destroyNav);
        public void SwitchElement(BasePlayer player, UIPanel type, Category subType, int pageNumber, string npcId);
        public bool IsOpen(BasePlayer player);
        public bool NPCHasUI(string npcId);
        public void RemoveNPCUI(string npcId);
        public void RenameComponents(CuiElementContainer container);
        public string GetNPCInUse(BasePlayer player);
        public class PlayerUI
        {
            public string npcId;
            public List<string> elementIds;
            public List<string> navigationIds;
        }

    }

    private void CreateAllElements();
    private void CreateNewElement(string npcId, NPCData.NPCInfo info);
    private void SetNewElement(string npcId);
    private void CreateNavUI(string npcId, NPCData.NPCInfo npcInfo);
    private void CreateItemsUI(Category category, string npcId, NPCData.NPCInfo npcInfo);
    private void CreateKitsUI(string npcId, NPCData.NPCInfo npcInfo);
    private void CreateCommandsUI(string npcId, NPCData.NPCInfo npcInfo);
    private void CreateExchangeUI(string npcId);
    private CuiElementContainer CreateItemsElement(List<KeyValuePair<string, RewardData.RewardItem>> items, Category category, int page, bool pageUp, bool pageDown, string npcId);
    private CuiElementContainer CreateKitsElement(List<KeyValuePair<string, RewardData.RewardKit>> kits, int page, bool pageUp, bool pageDown, string npcId);
    private CuiElementContainer CreateCommandsElement(List<KeyValuePair<string, RewardData.RewardCommand>> commands, int page, bool pageUp, bool pageDown, string npcId);
    private void PopupMessage(BasePlayer player, string msg);
    private void DisplayPoints(BasePlayer player);
    private CuiElementContainer CreateSaleElement(BasePlayer player);
    private void CreateInventoryEntry(CuiElementContainer container, string panelName, string shortname, ulong skinId, string name, int amount, int number);
    private void SellItem(BasePlayer player, string shortname, ulong skinId, int amount);
    private CuiElementContainer CreateTransferElement(BasePlayer player, int page);
    private void TransferElement(BasePlayer player, string name, string id);
    private void CreateMenuButton(CuiElementContainer container, string panelName, string buttonname, string command, int number);
    private void CreateSubMenu(CuiElementContainer container, string panelName, string npcId);
    private void CreateItemEntry(CuiElementContainer container, string panelName, string itemIdS, RewardData.RewardItem item, int number);
    private void CreateKitCommandEntry(CuiElementContainer container, string panelName, string displayName, string name, string description, int cost, int number, bool kit, string icon);
    private void CreatePlayerNameEntry(CuiElementContainer container, string panelName, string name, string id, int number);
    private float[] CalcPlayerNamePos(int number);
    private float[] CalcPosInv(int number);
    [ConsoleCommand("SRUI_BuyKit")]
    private void cmdBuyKit(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_BuyCommand")]
    private void cmdBuyCommand(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_BuyItem")]
    private void cmdBuyItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_ChangeElement")]
    private void cmdChangeElement(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_Exchange")]
    private void cmdExchange(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_Transfer")]
    private void ccmdTransfer(ConsoleSystem.Arg args);
    [ConsoleCommand("SRUI_TransferNext")]
    private void ccmdTransferNext(ConsoleSystem.Arg args);
    [ConsoleCommand("SRUI_TransferID")]
    private void ccmdTransferID(ConsoleSystem.Arg args);
    [ConsoleCommand("SRUI_DestroyAll")]
    private void cmdDestroyAll(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_CancelSale")]
    private void cmdCancelSale(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_SellItem")]
    private void cmdSellItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_Sell")]
    private void cmdSell(ConsoleSystem.Arg arg);
    private void Loaded();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    private void LoadUIColors();
    private void LoadAllImages();
    private void SaveLoop();
    private void SendMSG(BasePlayer player, string msg, string keyword);
    private void InformPoints(BasePlayer player);
    private object FindPlayer(BasePlayer player, string arg);
    private bool RemovePlayer(ulong ID);
    private void SendEchoConsole(Network.Connection cn, string msg);
    private void OpenStore(BasePlayer player, string npcid);
    private void GiveItem(BasePlayer player, string itemkey);
    private int GetAmount(BasePlayer player, string shortname, ulong skinId);
    private bool TakeResources(BasePlayer player, string shortname, ulong skinId, int iAmount);
    private string FormatTime(double time);
    private static double GrabCurrentTime();
    private bool IsSteamId(string id);
    private bool IsSteamId(ulong id);
    private object AddPoints(object userID, int amount);
    private object TakePoints(object userID, int amount, string item);
    private int CheckPoints(object userID);
    private object GetUserID(object userID);
    private JObject GetItemList();
    private bool AddItem(string shortname, ulong skinId, int amount, int cost, string category, bool isBp);
    private void AddImage(string fileName);
    private string GetImage(string fileName, ulong skin);
    private void CloseMap(BasePlayer player);
    private void OpenMap(BasePlayer player);
    private void AddMapMarker(float x, float z, string name, string icon);
    private void RemoveMapMarker(string name);
    private void OnUseNPC(BasePlayer npc, BasePlayer player);
    private bool IsRegisteredNPC(string ID);
    [ChatCommand("srnpc")]
    private void cmdSRNPC(BasePlayer player, string command, string[] args);
    private void ModifyNPC(BasePlayer player, BasePlayer NPC);
    private void NPCLootMenu(BasePlayer player, int page);
    private void CreateItemButton(CuiElementContainer container, string panel, string b1color, string b1text, string b1command, string b2color, string b2text, string b2command, int number, float xPos);
    [ConsoleCommand("SRUI_CustomList")]
    private void cmdCustomList(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCPage")]
    private void cmdNPCPage(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCOption")]
    private void cmdNPCOption(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCCancel")]
    private void cmdNPCCancel(ConsoleSystem.Arg arg);
    [ConsoleCommand("SRUI_NPCSave")]
    private void cmdNPCSave(ConsoleSystem.Arg arg);
    [ChatCommand("s")]
    private void cmdStore(BasePlayer player, string command, string[] args);
    [ChatCommand("rewards")]
    private void cmdRewards(BasePlayer player, string command, string[] args);
    [ConsoleCommand("rewards")]
    private void ccmdRewards(ConsoleSystem.Arg conArgs);
    [ChatCommand("sr")]
    private void cmdSR(BasePlayer player, string command, string[] args);
    [ConsoleCommand("sr")]
    private void ccmdSR(ConsoleSystem.Arg arg);
    private bool IsAuthed(BasePlayer player);
    private bool IsAuthedConsole(ConsoleSystem.Arg arg);
    private string GetKitContents(string kitname);
    private ConfigData configData;
    private class Colors
    {
        [JsonProperty(PropertyName = "Primary text color")]
        public string TextColor_Primary { get; set; }
        [JsonProperty(PropertyName = "Secondary text color")]
        public string TextColor_Secondary { get; set; }
        [JsonProperty(PropertyName = "Background color")]
        public UIColor Background_Dark { get; set; }
        [JsonProperty(PropertyName = "Secondary panel color")]
        public UIColor Background_Medium { get; set; }
        [JsonProperty(PropertyName = "Primary panel color")]
        public UIColor Background_Light { get; set; }
        [JsonProperty(PropertyName = "Button color - standard")]
        public UIColor Button_Standard { get; set; }
        [JsonProperty(PropertyName = "Button color - accept")]
        public UIColor Button_Accept { get; set; }
        [JsonProperty(PropertyName = "Button color - inactive")]
        public UIColor Button_Inactive { get; set; }
    }

    private class UIColor
    {
        [JsonProperty(PropertyName = "Hex color")]
        public string Color { get; set; }
        [JsonProperty(PropertyName = "Transparency (0 - 1)")]
        public float Alpha { get; set; }
    }

    private class Tabs
    {
        [JsonProperty(PropertyName = "Show kits tab")]
        public bool Kits { get; set; }
        [JsonProperty(PropertyName = "Show commands tab")]
        public bool Commands { get; set; }
        [JsonProperty(PropertyName = "Show items tab")]
        public bool Items { get; set; }
        [JsonProperty(PropertyName = "Show exchange tab")]
        public bool Exchange { get; set; }
        [JsonProperty(PropertyName = "Show transfer tab")]
        public bool Transfer { get; set; }
        [JsonProperty(PropertyName = "Show seller tab")]
        public bool Seller { get; set; }
    }

    private class Exchange
    {
        [JsonProperty(PropertyName = "Value of RP")]
        public int RP { get; set; }
        [JsonProperty(PropertyName = "Value of Economics")]
        public int Economics { get; set; }
    }

    private class Options
    {
        [JsonProperty(PropertyName = "Log all transactions")]
        public bool Logs { get; set; }
        [JsonProperty(PropertyName = "Data save interval")]
        public int SaveInterval { get; set; }
        [JsonProperty(PropertyName = "Use NPC dealers only")]
        public bool NPCOnly { get; set; }
    }

    private class UIOptions
    {
        [JsonProperty(PropertyName = "Disable fade in effect")]
        public bool FadeIn { get; set; }
        [JsonProperty(PropertyName = "Display kit contents as the description")]
        public bool KitContents { get; set; }
        [JsonProperty(PropertyName = "Display user playtime")]
        public bool ShowPlaytime { get; set; }
    }

    private class ConfigData
    {
        [JsonProperty(PropertyName = "Coloring")]
        public Colors Colors { get; set; }
        [JsonProperty(PropertyName = "Active categories (global)")]
        public Tabs Tabs { get; set; }
        [JsonProperty(PropertyName = "Currency exchange rates")]
        public Exchange Exchange { get; set; }
        [JsonProperty(PropertyName = "Options")]
        public Options Options { get; set; }
        [JsonProperty(PropertyName = "UI Options")]
        public UIOptions UIOptions { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void SaveRP();
    private void SaveRewards();
    private void SaveNPC();
    private void SaveSales();
    private void LoadData();
    private class PlayerData
    {
        public Dictionary<ulong, int> playerRP;
    }

    private class CooldownData
    {
        public Dictionary<ulong, CooldownUser> users;
        public void AddCooldown(ulong playerId, PurchaseType type, string key, int time);
        public bool HasCooldown(ulong playerId, PurchaseType type, string key, double remaining);
        public class CooldownUser
        {
             Dictionary<PurchaseType, Dictionary<string, double>> items;
            public void AddCooldown(PurchaseType type, string key, int time);
            public bool HasCooldown(PurchaseType type, string key, double remaining);
        }

    }

    private class NPCData
    {
        public Dictionary<string, NPCInfo> npcInfo;
        public class NPCInfo
        {
            public string name;
            public float x;
            public float z;
            public bool useCustom;
            public bool sellItems;
            public bool sellKits;
            public bool sellCommands;
            public bool canTransfer;
            public bool canSell;
            public bool canExchange;
            public List<string> items;
            public List<string> kits;
            public List<string> commands;
        }

    }

    private class RewardData
    {
        public Dictionary<string, RewardItem> items;
        public SortedDictionary<string, RewardKit> kits;
        public SortedDictionary<string, RewardCommand> commands;
        public bool HasItems(Category category);
        public class RewardItem : Reward
        {
            public string shortname;
            public string customIcon;
            public int amount;
            public ulong skinId;
            public bool isBp;
            public Category category;
            [JsonIgnore]
            private ItemDefinition _itemDefinition;
            [JsonIgnore]
            public ItemDefinition ItemDefinition { get; set; }
        }

        public class RewardKit : Reward
        {
            public string kitName;
            public string description;
            public string iconName;
        }

        public class RewardCommand : Reward
        {
            public string description;
            public string iconName;
            public List<string> commands;
        }

        public class Reward
        {
            public string displayName;
            public int cost;
            public int cooldown;
        }

    }

    private class SaleData
    {
        public Dictionary<string, Dictionary<ulong, SaleItem>> items;
        public class SaleItem
        {
            public float price;
            public string displayName;
            public bool enabled;
        }

    }

    private void UpdatePriceList();
    private bool HasSkins(ItemDefinition item);
    private string msg(string key, string playerId);
    private Dictionary<string, string> messages;
}

private class UI
{
    public static CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax);
    public static void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
    public static void CreateLabel(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align, string color, float fadein);
    public static void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    public static void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    public static void LoadImage(CuiElementContainer container, string panel, int itemid, ulong skinid, string aMin, string aMax);
    public static string Color(string hexColor, float alpha);
}

private class UIManager
{
    public Dictionary<UIPanel, Dictionary<Category, Dictionary<int, CuiElementContainer>>> standardElements;
    public Dictionary<string, Dictionary<UIPanel, Dictionary<Category, Dictionary<int, CuiElementContainer>>>> npcElements;
    public Dictionary<ulong, PlayerUI> playerUi;
    public void AddUI(BasePlayer player, UIPanel type, Category subType, int pageNumber, string npcId);
    public void DestroyUI(BasePlayer player, bool destroyNav);
    public void SwitchElement(BasePlayer player, UIPanel type, Category subType, int pageNumber, string npcId);
    public bool IsOpen(BasePlayer player);
    public bool NPCHasUI(string npcId);
    public void RemoveNPCUI(string npcId);
    public void RenameComponents(CuiElementContainer container);
    public string GetNPCInUse(BasePlayer player);
    public class PlayerUI
    {
        public string npcId;
        public List<string> elementIds;
        public List<string> navigationIds;
    }

}

public class PlayerUI
{
    public string npcId;
    public List<string> elementIds;
    public List<string> navigationIds;
}

private class Colors
{
    [JsonProperty(PropertyName = "Primary text color")]
    public string TextColor_Primary { get; set; }
    [JsonProperty(PropertyName = "Secondary text color")]
    public string TextColor_Secondary { get; set; }
    [JsonProperty(PropertyName = "Background color")]
    public UIColor Background_Dark { get; set; }
    [JsonProperty(PropertyName = "Secondary panel color")]
    public UIColor Background_Medium { get; set; }
    [JsonProperty(PropertyName = "Primary panel color")]
    public UIColor Background_Light { get; set; }
    [JsonProperty(PropertyName = "Button color - standard")]
    public UIColor Button_Standard { get; set; }
    [JsonProperty(PropertyName = "Button color - accept")]
    public UIColor Button_Accept { get; set; }
    [JsonProperty(PropertyName = "Button color - inactive")]
    public UIColor Button_Inactive { get; set; }
}

private class UIColor
{
    [JsonProperty(PropertyName = "Hex color")]
    public string Color { get; set; }
    [JsonProperty(PropertyName = "Transparency (0 - 1)")]
    public float Alpha { get; set; }
}

private class Tabs
{
    [JsonProperty(PropertyName = "Show kits tab")]
    public bool Kits { get; set; }
    [JsonProperty(PropertyName = "Show commands tab")]
    public bool Commands { get; set; }
    [JsonProperty(PropertyName = "Show items tab")]
    public bool Items { get; set; }
    [JsonProperty(PropertyName = "Show exchange tab")]
    public bool Exchange { get; set; }
    [JsonProperty(PropertyName = "Show transfer tab")]
    public bool Transfer { get; set; }
    [JsonProperty(PropertyName = "Show seller tab")]
    public bool Seller { get; set; }
}

private class Exchange
{
    [JsonProperty(PropertyName = "Value of RP")]
    public int RP { get; set; }
    [JsonProperty(PropertyName = "Value of Economics")]
    public int Economics { get; set; }
}

private class Options
{
    [JsonProperty(PropertyName = "Log all transactions")]
    public bool Logs { get; set; }
    [JsonProperty(PropertyName = "Data save interval")]
    public int SaveInterval { get; set; }
    [JsonProperty(PropertyName = "Use NPC dealers only")]
    public bool NPCOnly { get; set; }
}

private class UIOptions
{
    [JsonProperty(PropertyName = "Disable fade in effect")]
    public bool FadeIn { get; set; }
    [JsonProperty(PropertyName = "Display kit contents as the description")]
    public bool KitContents { get; set; }
    [JsonProperty(PropertyName = "Display user playtime")]
    public bool ShowPlaytime { get; set; }
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Coloring")]
    public Colors Colors { get; set; }
    [JsonProperty(PropertyName = "Active categories (global)")]
    public Tabs Tabs { get; set; }
    [JsonProperty(PropertyName = "Currency exchange rates")]
    public Exchange Exchange { get; set; }
    [JsonProperty(PropertyName = "Options")]
    public Options Options { get; set; }
    [JsonProperty(PropertyName = "UI Options")]
    public UIOptions UIOptions { get; set; }
}

private class PlayerData
{
    public Dictionary<ulong, int> playerRP;
}

private class CooldownData
{
    public Dictionary<ulong, CooldownUser> users;
    public void AddCooldown(ulong playerId, PurchaseType type, string key, int time);
    public bool HasCooldown(ulong playerId, PurchaseType type, string key, double remaining);
    public class CooldownUser
    {
         Dictionary<PurchaseType, Dictionary<string, double>> items;
        public void AddCooldown(PurchaseType type, string key, int time);
        public bool HasCooldown(PurchaseType type, string key, double remaining);
    }

}

public class CooldownUser
{
     Dictionary<PurchaseType, Dictionary<string, double>> items;
    public void AddCooldown(PurchaseType type, string key, int time);
    public bool HasCooldown(PurchaseType type, string key, double remaining);
}

private class NPCData
{
    public Dictionary<string, NPCInfo> npcInfo;
    public class NPCInfo
    {
        public string name;
        public float x;
        public float z;
        public bool useCustom;
        public bool sellItems;
        public bool sellKits;
        public bool sellCommands;
        public bool canTransfer;
        public bool canSell;
        public bool canExchange;
        public List<string> items;
        public List<string> kits;
        public List<string> commands;
    }

}

public class NPCInfo
{
    public string name;
    public float x;
    public float z;
    public bool useCustom;
    public bool sellItems;
    public bool sellKits;
    public bool sellCommands;
    public bool canTransfer;
    public bool canSell;
    public bool canExchange;
    public List<string> items;
    public List<string> kits;
    public List<string> commands;
}

private class RewardData
{
    public Dictionary<string, RewardItem> items;
    public SortedDictionary<string, RewardKit> kits;
    public SortedDictionary<string, RewardCommand> commands;
    public bool HasItems(Category category);
    public class RewardItem : Reward
    {
        public string shortname;
        public string customIcon;
        public int amount;
        public ulong skinId;
        public bool isBp;
        public Category category;
        [JsonIgnore]
        private ItemDefinition _itemDefinition;
        [JsonIgnore]
        public ItemDefinition ItemDefinition { get; set; }
    }

    public class RewardKit : Reward
    {
        public string kitName;
        public string description;
        public string iconName;
    }

    public class RewardCommand : Reward
    {
        public string description;
        public string iconName;
        public List<string> commands;
    }

    public class Reward
    {
        public string displayName;
        public int cost;
        public int cooldown;
    }

}

public class RewardItem : Reward
{
    public string shortname;
    public string customIcon;
    public int amount;
    public ulong skinId;
    public bool isBp;
    public Category category;
    [JsonIgnore]
    private ItemDefinition _itemDefinition;
    [JsonIgnore]
    public ItemDefinition ItemDefinition { get; set; }
}

public class RewardKit : Reward
{
    public string kitName;
    public string description;
    public string iconName;
}

public class RewardCommand : Reward
{
    public string description;
    public string iconName;
    public List<string> commands;
}

public class Reward
{
    public string displayName;
    public int cost;
    public int cooldown;
}

private class SaleData
{
    public Dictionary<string, Dictionary<ulong, SaleItem>> items;
    public class SaleItem
    {
        public float price;
        public string displayName;
        public bool enabled;
    }

}

public class SaleItem
{
    public float price;
    public string displayName;
    public bool enabled;
}


```

---

## ServerRewardsWipe by ZEODE - Reset Server Rewards player RP data on wipe, or by command.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using Oxide.Core.Configuration;

Oxide.Plugins
[Info("Server Rewards Wipe", "ZEODE", "1.1.3")]
[Description("Reset Server Rewards player RP on wipe or command.")]
public class ServerRewardsWipe : CovalencePlugin
{
    [PluginReference]
    private Plugin ServerRewards;
    private const string permAdmin;
    private _PlayerData _playerData;
    private DynamicConfigFile _playerdata;
    private DynamicConfigFile _playerdatabackup;
    private class _PlayerData
    {
        public Dictionary<ulong, int> playerRP;
    }

    private void Init();
    private void OnServerInitialized(bool initial);
    private void OnNewSave(string filename);
    private void LoadRPData();
    private void WipeAndSaveRP();
    private void ClearRPData();
    [Command("clearrpdata")]
    private void cmdClearRPData(IPlayer player, string command, string[] args);
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Options")]
        public Options options;
        public class Options
        {
            [JsonProperty(PropertyName = "Reset ServerRewards player RP on wipe")]
            public bool ResetRPOnWipe { get; set; }
            [JsonProperty(PropertyName = "Backup player RP before wiping")]
            public bool BackupRP { get; set; }
            [JsonProperty(PropertyName = "Wipe delay (seconds) after server startup. Try increasing if RP wipe fails on startup")]
            public float WipeDelay { get; set; }
        }

        public VersionNumber Version { get; set; }
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    protected override void LoadDefaultMessages();
}

private class _PlayerData
{
    public Dictionary<ulong, int> playerRP;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Options")]
    public Options options;
    public class Options
    {
        [JsonProperty(PropertyName = "Reset ServerRewards player RP on wipe")]
        public bool ResetRPOnWipe { get; set; }
        [JsonProperty(PropertyName = "Backup player RP before wiping")]
        public bool BackupRP { get; set; }
        [JsonProperty(PropertyName = "Wipe delay (seconds) after server startup. Try increasing if RP wipe fails on startup")]
        public float WipeDelay { get; set; }
    }

    public VersionNumber Version { get; set; }
}

public class Options
{
    [JsonProperty(PropertyName = "Reset ServerRewards player RP on wipe")]
    public bool ResetRPOnWipe { get; set; }
    [JsonProperty(PropertyName = "Backup player RP before wiping")]
    public bool BackupRP { get; set; }
    [JsonProperty(PropertyName = "Wipe delay (seconds) after server startup. Try increasing if RP wipe fails on startup")]
    public float WipeDelay { get; set; }
}


```

---

## ServerStatus by 2CHEVSKII - Sends a Discord message whether the server is online or offline or restart

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using Oxide.Core;
using System;
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries;
using System.Linq;
using ConVar;
using Oxide.Game.Rust.Libraries;

Oxide.Plugins
[Info("Server Status", "UNKN0WN", "1.3.0")]
[Description("Server Status Check for discord Webhook")]
 class ServerStatus : RustPlugin
{
    private Configuration _config;
    [PluginReference]
     Plugin SmoothRestart;
    private bool isQuit;
    private bool isSR;
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private void OnServerInitialized();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class Configuration
    {
        [JsonProperty("Discord WebHook")]
        public string webhook { get; set; }
        [JsonProperty("Embed Fields Time Format")]
        public string TimeFormat { get; set; }
        [JsonProperty("Selection Mention (0 - none | 1 - @here | 2 - @everyone | 3 - @something)")]
        public int SelectionMention { get; set; }
        [JsonProperty("Designated mention")]
        public string DesignatedMention;
    }

    protected override void LoadDefaultMessages();
    private string Lang(string key, Dictionary<string, string> args);
    private void SendMessage(string status, string reason);
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

}

private class Configuration
{
    [JsonProperty("Discord WebHook")]
    public string webhook { get; set; }
    [JsonProperty("Embed Fields Time Format")]
    public string TimeFormat { get; set; }
    [JsonProperty("Selection Mention (0 - none | 1 - @here | 2 - @everyone | 3 - @something)")]
    public int SelectionMention { get; set; }
    [JsonProperty("Designated mention")]
    public string DesignatedMention;
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


```

---

## SharedDoors by dbteku - Sharing doors with friends via the tool cupboard.

```csharp
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("SharedDoors", "dbteku", "2.0.1")]
[Description("Making sharing doors easier.")]
public class SharedDoors : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    [PluginReference]
    private Plugin Friends;
    private static SharedDoors instance;
    private const string CLANS_NAME;
    private const string CLANS_AUTHOR_NAME;
    private const string FRIENDS_NAME;
    private const string FRIENDS_AUTHOR_NAME;
    private const string FRIENDS_AUTHOR_NAME_ALTERNATE;
    private const string RUST_CLANS_HOOK;
    private const string RUST_CLANS_UNHOOK;
    private const string RUST_FRIENDS_HOOK;
    private const string RUST_FRIENDS_UNHOOK;
    private const string RUST_CLANS_NOT_FOUND;
    private const string RUST_FRIENDS_NOT_FOUND;
    private const string WRONG_CLANS_PLUGIN;
    private const string MASTER_PERM;
    private MasterKeyHolders holders;
    private void OnServerInitialized();
    private void Unload();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private bool CanUseLockedEntity(BasePlayer player, BaseLock door);
    [ChatCommand("sd")]
    private void SharedDoorsCommand(BasePlayer basePlayer, string command, string[] args);
    public static SharedDoors getInstance();
    private class PlayerResponder
    {
        private const string PREFIX;
        public static void NotifyUser(IPlayer player, String message);
    }

    private class DoorAuthorizer
    {
        public BaseLock BaseDoor { get; set; }
        public BasePlayer Player { get; set; }
        private ToolCupboardChecker checker;
        private FriendsClansHandler handler;
        public DoorAuthorizer(BaseLock door, BasePlayer player);
        public bool CanOpen();
        private bool CanOpenCodeLock(CodeLock door, BasePlayer player);
        private bool CanOpenKeyLock(KeyLock door, BasePlayer player);
        private void PlaySound(bool canUse, CodeLock door, BasePlayer player);
    }

    private class ToolCupboardChecker
    {
        public BasePlayer Player { get; set; }
        public ToolCupboardChecker(BasePlayer player);
        public bool IsPlayerAuthorized();
    }

    private class FriendsClansHandler
    {
        private const string GET_CLAN_OF_PLAYER;
        private const string IS_CLAN_MEMBER;
        public Plugin Clans { get; set; }
        public Plugin Friends { get; set; }
        public ulong OriginalPlayerID { get; set; }
        public DoorAuthorizer Door { get; set; }
        public FriendsClansHandler(DoorAuthorizer door);
        public bool IsInClan(string playerId, string playerDoorOwner);
        public bool IsFriend(string playerId, string playerDoorOwner);
        public bool ClansAvailable();
        public bool FriendsAvailable();
    }

    private class MasterKeyHolders
    {
        private Dictionary<string, PlayerSettings> keyMasters;
        public MasterKeyHolders();
        public void AddMaster(String id);
        public void RemoveMaster(String id);
        public void GiveMasterKey(String id);
        public void RemoveMasterKey(String id);
        public bool IsAKeyMaster(String id);
        public void ToggleMasterMode(String id);
        public bool HasMaster(string id);
    }

    private class PlayerSettings
    {
        public bool IsMasterKeyHolder { get; set; }
        public PlayerSettings(bool isMasterKeyHolder);
        public void ToggleMasterMode();
    }

}

private class PlayerResponder
{
    private const string PREFIX;
    public static void NotifyUser(IPlayer player, String message);
}

private class DoorAuthorizer
{
    public BaseLock BaseDoor { get; set; }
    public BasePlayer Player { get; set; }
    private ToolCupboardChecker checker;
    private FriendsClansHandler handler;
    public DoorAuthorizer(BaseLock door, BasePlayer player);
    public bool CanOpen();
    private bool CanOpenCodeLock(CodeLock door, BasePlayer player);
    private bool CanOpenKeyLock(KeyLock door, BasePlayer player);
    private void PlaySound(bool canUse, CodeLock door, BasePlayer player);
}

private class ToolCupboardChecker
{
    public BasePlayer Player { get; set; }
    public ToolCupboardChecker(BasePlayer player);
    public bool IsPlayerAuthorized();
}

private class FriendsClansHandler
{
    private const string GET_CLAN_OF_PLAYER;
    private const string IS_CLAN_MEMBER;
    public Plugin Clans { get; set; }
    public Plugin Friends { get; set; }
    public ulong OriginalPlayerID { get; set; }
    public DoorAuthorizer Door { get; set; }
    public FriendsClansHandler(DoorAuthorizer door);
    public bool IsInClan(string playerId, string playerDoorOwner);
    public bool IsFriend(string playerId, string playerDoorOwner);
    public bool ClansAvailable();
    public bool FriendsAvailable();
}

private class MasterKeyHolders
{
    private Dictionary<string, PlayerSettings> keyMasters;
    public MasterKeyHolders();
    public void AddMaster(String id);
    public void RemoveMaster(String id);
    public void GiveMasterKey(String id);
    public void RemoveMasterKey(String id);
    public bool IsAKeyMaster(String id);
    public void ToggleMasterMode(String id);
    public bool HasMaster(string id);
}

private class PlayerSettings
{
    public bool IsMasterKeyHolder { get; set; }
    public PlayerSettings(bool isMasterKeyHolder);
    public void ToggleMasterMode();
}


```

---

## ShootToBan by Death - Make banning players easy, with just shooting your gun.

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("ShootToBan", "Death", "1.1.1")]
[Description("Make banning players easy by just shooting your gun.")]
public class ShootToBan : RustPlugin
{
     List<ulong> Armed;
    const string stbPerm;
     void Init();
     void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    [ChatCommand("stb")]
     void stb(BasePlayer player);
     void Disable(BasePlayer player);
     void MSG(BasePlayer player, string m);
    private ConfigData configData;
     class ConfigData
    {
        public Settings Settings;
    }

     class Settings
    {
        public bool Enabled;
        public string Ban_Weapon;
        public int Command_Active_Time;
    }

    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
    protected override void LoadDefaultMessages();
    private string msg(string key, string id);
}

 class ConfigData
{
    public Settings Settings;
}

 class Settings
{
    public bool Enabled;
    public string Ban_Weapon;
    public int Command_Active_Time;
}


```

---

## ShopfrontLogs by Ryz0r - Logs completed shopfront trades to a Discord webhook

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("Shopfront Logs", "Ryz0r", "1.0.0"), Description("Logs shopfront completed trades to Discord.")]
public class ShopfrontLogs : RustPlugin
{
    private const string BypassPerm;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Discord Webhook URL")]
        public string WebhookURL;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private object OnShopCompleteTrade(ShopFront entity);
    private void SendDiscordMessage(string v, string c, IReadOnlyCollection<string> vItems, IReadOnlyCollection<string> cItems);
    private void GetCallback(int code, string response);
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
        [JsonProperty("color")]
        public int Color { get; set; }
        public Embed AddField(string name, string value, bool inline);
        public Embed SetColor(string color);
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

}

private class Configuration
{
    [JsonProperty(PropertyName = "Discord Webhook URL")]
    public string WebhookURL;
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
    [JsonProperty("color")]
    public int Color { get; set; }
    public Embed AddField(string name, string value, bool inline);
    public Embed SetColor(string color);
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


```

---

## ShowCrosshair by Netherkitteh - Shows a crosshair on the screen

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Show Crosshair", "Marat", "1.0.79")]
[Description("Shows a crosshair on the screen")]
 class ShowCrosshair : RustPlugin
{
    private List<string> WeaponList;
    private Hash<ulong, Timer> popupMessages;
    static bool uiFadeIn;
    private bool isILReady;
    readonly Dictionary<string, string> lastCrosshair;
    readonly Dictionary<string, bool> enabled;
    readonly Dictionary<string, bool> opened;
     List<ulong> Cross;
     List<ulong> Menu;
     bool EnableCross(BasePlayer player);
     bool EnableMenu(BasePlayer player);
    private bool configChanged;
    private const string permShowCrosshair;
    private string GetImage(string name, ulong skin);
    private void Init();
     void OnServerInitialized();
     void ValidateImages();
    private void LoadImages();
    protected override void LoadDefaultConfig();
    [PluginReference]
     ImageLibrary ImageLibrary;
    private bool usePermissions;
    private bool ShowOnLogin;
    private bool HideWhenAiming;
    private bool EnableSound;
    private bool ShowMessage;
    private string SoundOpen;
    private string SoundDisable;
    private string SoundSelect;
    private string SoundToggle;
    private string commandmenu;
    private string colorClose;
    private string colorBackground;
    private string colorToggle;
    private string colorDisable;
    private string image1;
    private string image2;
    private string image3;
    private string image4;
    private string image5;
    private string image6;
    private string image7;
    private string image8;
    private string background;
    private string background2;
    private void LoadConfiguration();
    private T GetConfig(string category, string setting, T defaultValue);
    protected override void LoadDefaultMessages();
    private void cmdChatShowMenu(BasePlayer player);
    [ConsoleCommand("crosshair")]
    private void cmdConsoleShowMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("loadcrosshairimages")]
    private void cmdRefreshAllImages(ConsoleSystem.Arg arg);
    [ConsoleCommand("CloseMenu")]
     void cmdConsoleCloseMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("command1")]
     void cmdConsoleCommand1(ConsoleSystem.Arg arg);
    [ConsoleCommand("command2")]
     void cmdConsoleCommand2(ConsoleSystem.Arg arg);
    [ConsoleCommand("command3")]
     void cmdConsoleCommand3(ConsoleSystem.Arg arg);
    [ConsoleCommand("command4")]
     void cmdConsoleCommand4(ConsoleSystem.Arg arg);
    [ConsoleCommand("command5")]
     void cmdConsoleCommand5(ConsoleSystem.Arg arg);
    [ConsoleCommand("command6")]
     void cmdConsoleCommand6(ConsoleSystem.Arg arg);
    [ConsoleCommand("command7")]
     void cmdConsoleCommand7(ConsoleSystem.Arg arg);
    [ConsoleCommand("command8")]
     void cmdConsoleCommand8(ConsoleSystem.Arg arg);
    [ConsoleCommand("commandNext")]
     void cmdConsoleCommandNext(ConsoleSystem.Arg arg);
    [ConsoleCommand("commandBack")]
     void cmdConsoleCommandBack(ConsoleSystem.Arg arg);
    [ConsoleCommand("commandDisable")]
     void cmdConsoleCommandDisable(ConsoleSystem.Arg arg);
    private void OnPlayerInit(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void Unload();
    private void DestroyAll(BasePlayer player);
    private void DestroyCrosshair(BasePlayer player);
    private void DestroyGUImenu(BasePlayer player);
    private void EnabledCrosshair(BasePlayer player);
     void OnPlayerInput(BasePlayer player, InputState input);
    private void DisabledCrosshair(BasePlayer player);
    private void EnabledMenu(BasePlayer player);
    private void DisabledMenu(BasePlayer player);
    private void PopupMessage(BasePlayer player, string msg);
    private Dictionary<string, Timer> timers;
     void OnScreen(BasePlayer player, string msg);
    private void Crosshair1(BasePlayer player);
    private void Crosshair2(BasePlayer player);
    private void Crosshair3(BasePlayer player);
    private void Crosshair4(BasePlayer player);
    private void Crosshair5(BasePlayer player);
    private void Crosshair6(BasePlayer player);
    private void Crosshair7(BasePlayer player);
    private void Crosshair8(BasePlayer player);
    private void ShowMenu(BasePlayer player, string text);
    private void NextMenu2(BasePlayer player, string text);
    private string PanelOnScreen;
     string UIHud1;
    const string UIPopup;
     class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, float fadein, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align, string color, float fadein);
        static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
        static public string Color(string hexColor, float alpha);
    }

    private Dictionary<string, string> UIColors;
     string Lang(string key, string id, object[] args);
     void Reply(BasePlayer player, string message, string args);
     bool IsAllowed(string id, string perm);
}

 class UI
{
    static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, float fadein, bool cursor);
    static public void CreateLabel(CuiElementContainer container, string panel, string text, int size, string aMin, string aMax, TextAnchor align, string color, float fadein);
    static public void CreateButton(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align, float fadein);
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
    static public string Color(string hexColor, float alpha);
}


```

---

## ShrinkingRadZone by k1lly0u - Create shrinking radiation zones for BR style event

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Shrinking Radiation Zone", "k1lly0u", "0.1.5")]
[Description("Create shrinking radiation zones for BR style events")]
 class ShrinkingRadZone : RustPlugin
{
    private Hash<string, ShrinkZone> activeZones;
    private const string SPHERE_ENTITY;
    private void Loaded();
    private void Unload();
    private string CreateShrinkZone(Vector3 position, float radius, float time);
    private void ToggleZoneShrink(string id);
    private void DestroyShrinkZone(string id);
    private class ShrinkZone : MonoBehaviour
    {
        private string zoneId;
        private float initialRadius;
        private float modifiedRadius;
        private float targetRadius;
        private float timeToTake;
        private float timeTaken;
        private SphereEntity[] innerSpheres;
        private SphereEntity[] outerSpheres;
        private SphereCollider innerCollider;
        private TriggerRadiation radiation;
        private void Awake();
        private void OnDestroy();
        private void Update();
        private void OnTriggerEnter(Collider obj);
        private void OnTriggerExit(Collider obj);
        public void CreateZones(string zoneId, Vector3 position, float initialRadius, float timeToTake);
        public void ToggleShrinking();
    }

    [ChatCommand("shrink")]
    private void cmdShrink(BasePlayer player, string command, string[] args);
    [ConsoleCommand("shrink")]
    private void ccmdShrink(ConsoleSystem.Arg arg);
    private static ConfigData Configuration;
    private class ConfigData
    {
        public float RadiationBuffer { get; set; }
        public float FinalZoneSize { get; set; }
        public float InitialZoneSize { get; set; }
        public float ShrinkTime { get; set; }
        public float RadiationStrength { get; set; }
        public int DomeShade { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private string msg(string key, string playerId);
    private Dictionary<string, string> messages;
}

private class ShrinkZone : MonoBehaviour
{
    private string zoneId;
    private float initialRadius;
    private float modifiedRadius;
    private float targetRadius;
    private float timeToTake;
    private float timeTaken;
    private SphereEntity[] innerSpheres;
    private SphereEntity[] outerSpheres;
    private SphereCollider innerCollider;
    private TriggerRadiation radiation;
    private void Awake();
    private void OnDestroy();
    private void Update();
    private void OnTriggerEnter(Collider obj);
    private void OnTriggerExit(Collider obj);
    public void CreateZones(string zoneId, Vector3 position, float initialRadius, float timeToTake);
    public void ToggleShrinking();
}

private class ConfigData
{
    public float RadiationBuffer { get; set; }
    public float FinalZoneSize { get; set; }
    public float InitialZoneSize { get; set; }
    public float ShrinkTime { get; set; }
    public float RadiationStrength { get; set; }
    public int DomeShade { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## ShutdownWhenUpdate by  - Shuts down the server when an Oxide update is available

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Shutdown When Update", "Sorrow", "0.1.3")]
[Description("Shutdown the server when uMod update is available")]
public class ShutdownWhenUpdate : CovalencePlugin
{
    private const string Permission;
    private string _apiGitHub;
    private int _countdownToShutdown;
    private float _intervalToCheckUpdate;
    private string _tokenGithub;
    private bool _restartPlanned;
    private void Init();
    private void Unload();
    private void OnServerShutdown();
    private void BroadcastRestart(int countdownCount, bool firstCall);
    private static bool IsDisplayable(int countdownCount);
    private static string GetFormattedTime(int countdownCount);
    private void GetLatestVersion(bool manual);
    private void CompareVersionsAndShutdown();
    private void Shutdown();
    private float GetIntervalToCheckUpdate();
    [Command("swu")]
    private void SwuCommand(IPlayer player, string command, string[] args);
    private void InfoMsg(string key, IPlayer player, object[] args, bool hideMsg);
    private void SendInfoMessage(string message, object[] args, bool hideMsg);
    protected override void LoadDefaultMessages();
    protected new void LoadDefaultConfig();
    private class WebResponse
    {
        public string Name { get; set; }
    }

}

private class WebResponse
{
    public string Name { get; set; }
}


```

---

## SignalCooldown by MisterPixie - Limits the amount of signals to be dropped to 1 supply signal every X seconds

```csharp
using UnityEngine;
using System.Collections.Generic;
using System;
using Oxide.Core;

Oxide.Plugins
[Info("Signal Cooldown", "Vliek", "1.0.32", ResourceId = 2805)]
[Description("Add a cooldown to supply signals to avoid that players are going to leave from rage.")]
 class SignalCooldown : RustPlugin
{
    private bool Changed;
    private bool refundSignal;
    private int timeCooldown;
    private string cooldownPerm;
    private ulong messageIcon;
    private int authLevel;
    public List<ulong> signalCooldown;
    public Dictionary<ulong, float> lastRun;
     string GetLang(string msg, string userID);
    protected override void LoadDefaultConfig();
    private void LoadVariables();
     object GetConfig(string menu, string datavalue, object defaultValue);
    protected override void LoadDefaultMessages();
    private void Init();
    public bool HasSignalCooldown(ulong userId);
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    [ChatCommand("signalreset")]
    private void ResetTimeCMD(BasePlayer player, string command, string[] args);
     void Reset();
}


```

---

## SignArtist by Whispers88 - Load custom images to signs from a remote URL

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Plugins.SignArtistClasses;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using UnityEngine.Networking;
using Color = System.Drawing.Color;
using Graphics = System.Drawing.Graphics;
using Steamworks;

Oxide.Plugins
[Info("Sign Artist", "Whispers88", "1.4.1")]
[Description("Allows players with the appropriate permission to import images from the internet on paintable objects")]
internal class SignArtist : RustPlugin
{
    private Dictionary<ulong, float> cooldowns;
    private GameObject imageDownloaderGameObject;
    private ImageDownloader imageDownloader;
     SignArtistConfig Settings { get; set; }
     Dictionary<string, ImageSize> ImageSizePerAsset { get; set; }
     Dictionary<ulong, string> SkiniconUrls;
    private const string ItemIconUrl;
    public class SignArtistConfig
    {
        [JsonProperty(PropertyName = "Time in seconds between download requests (0 to disable)")]
        public int Cooldown { get; set; }
        [JsonProperty(PropertyName = "Maximum concurrent downloads")]
        public int MaxActiveDownloads { get; set; }
        [JsonProperty(PropertyName = "Maximum distance from the sign")]
        public int MaxDistance { get; set; }
        [JsonProperty(PropertyName = "Maximum filesize in MB")]
        public float MaxSize { get; set; }
        [JsonProperty(PropertyName = "Enforce JPG file format")]
        public bool EnforceJpeg { get; set; }
        [JsonProperty(PropertyName = "JPG image quality (0-100)")]
        public int Quality { get; set; }
        [JsonProperty("Enable logging file")]
        public bool FileLogging { get; set; }
        [JsonProperty("Enable logging console")]
        public bool ConsoleLogging { get; set; }
        [JsonProperty("Enable discord logging")]
        public bool Discordlogging { get; set; }
        [JsonProperty("Discord Webhook")]
        public string DiscordWebhook { get; set; }
        [JsonProperty("Avatar URL")]
        public string AvatarUrl { get; set; }
        [JsonProperty("Discord Username")]
        public string DiscordUsername { get; set; }
        [JsonIgnore]
        public float MaxFileSizeInBytes { get; set; }
        private int quality;
        public static SignArtistConfig DefaultConfig();
    }

    private class DownloadRequest
    {
        public BasePlayer Sender { get; set; }
        public IPaintableEntity Sign { get; set; }
        public string Url { get; set; }
        public bool Raw { get; set; }
        public bool Hor { get; set; }
        public uint TextureIndex { get; set; }
        public DownloadRequest(string url, BasePlayer player, IPaintableEntity sign, bool raw, bool hor, uint textureIndex);
    }

    private class RestoreRequest
    {
        public BasePlayer Sender { get; set; }
        public IPaintableEntity Sign { get; set; }
        public bool Raw { get; set; }
        public uint TextureIndex { get; set; }
        public RestoreRequest(BasePlayer player, IPaintableEntity sign, bool raw, uint textureIndex);
    }

    public class ImageSize
    {
        public int Width { get; set; }
        public int Height { get; set; }
        public int ImageWidth { get; set; }
        public int ImageHeight { get; set; }
        public ImageSize(int width, int height);
        public ImageSize(int width, int height, int imageWidth, int imageHeight);
    }

    private class ImageDownloader : MonoBehaviour
    {
        private byte activeDownloads;
        private byte activeRestores;
        private readonly SignArtist signArtist;
        private readonly Queue<DownloadRequest> downloadQueue;
        private readonly Queue<RestoreRequest> restoreQueue;
        public void QueueDownload(string url, BasePlayer player, IPaintableEntity sign, uint textureIndex, bool raw, bool hor);
        public void QueueRestore(BasePlayer player, IPaintableEntity sign, bool raw, uint textureIndex);
        private void StartNextDownload(bool reduceCount);
        private void StartNextRestore(bool reduceCount);
        private IEnumerator DownloadImage(DownloadRequest request);
        private IEnumerator LogToDiscord(DownloadRequest request);
        private Message DiscordMessage(string servername, string playername, string userid, string itemname, string imgurl, string location);
        private IEnumerator RestoreImage(RestoreRequest request);
        private ImageSize GetImageSizeFor(IPaintableEntity signage);
        private byte[] GetImageBytes(UnityWebRequest www);
    }

    private abstract class BasePaintableEntity : IPaintableEntity
    {
        public BaseEntity Entity { get; set; }
        public string PrefabName { get; set; }
        public string ShortPrefabName { get; set; }
        public NetworkableId NetId { get; set; }
        public virtual int TextureCount { get; set; }
        public abstract uint[] TextureIds { get; set; }
        protected BasePaintableEntity(BaseEntity entity);
        public abstract bool CanUpdate(BasePlayer player);
        public abstract void SetImage(uint textureIndex, uint id);
        public virtual void EnsureInitialized();
        public void SendNetworkUpdate();
    }

    private class PaintableSignage : BasePaintableEntity, IPaintableEntity
    {
        public Signage Sign { get; set; }
        public override int TextureCount { get; set; }
        public override uint[] TextureIds { get; set; }
        public PaintableSignage(Signage sign);
        public override void SetImage(uint textureIndex, uint id);
        public override bool CanUpdate(BasePlayer player);
        public override void EnsureInitialized();
    }

    private class PaintableFrame : BasePaintableEntity, IPaintableEntity
    {
        public PhotoFrame Sign { get; set; }
        public override uint[] TextureIds { get; set; }
        public PaintableFrame(PhotoFrame sign);
        public override void SetImage(uint textureIndex, uint id);
        public override bool CanUpdate(BasePlayer player);
    }

    private class PaintablePumpkin : BasePaintableEntity, IPaintableEntity
    {
        public CarvablePumpkin Pumpkin { get; set; }
        public override uint[] TextureIds { get; set; }
        public PaintablePumpkin(CarvablePumpkin pumpkin);
        public override void SetImage(uint textureIndex, uint id);
        public override bool CanUpdate(BasePlayer player);
        public override void EnsureInitialized();
    }

    private void Init();
    private void GetSteamworksImages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void Unload();
    protected override void LoadDefaultMessages();
    [Command("sil"), Permission("signartist.url")]
    private void SilCommand(IPlayer iplayer, string command, string[] args);
    [Command("sili"), Permission("signartist.url")]
    private void SilItemCommand(IPlayer iplayer, string command, string[] args);
    private const string FindWorkshopSkinUrl;
    private IEnumerator DownloadWorkshopskin(Item held, IPaintableEntity sign, bool hor, uint textureIndex);
    [Command("silt"), Permission("signartist.text")]
    private void SiltCommand(IPlayer iplayer, string command, string[] args);
    [Command("silrestore"), Permission("signartist.raw")]
    private void RestoreCommand(IPlayer iplayer, string command, string[] args);
    private bool HasCooldown(BasePlayer player);
    private float GetCooldown(BasePlayer player);
    private void SetCooldown(BasePlayer player);
    private string FormatCooldown(float seconds);
    private bool IsLookingAtSign(BasePlayer player, IPaintableEntity sign);
    private bool HasValidIndexArg(string[] args, IPaintableEntity sign, uint textureIndex);
    private bool CanChangeSign(BasePlayer player, IPaintableEntity sign);
    private bool HasPermission(BasePlayer player, string perm);
    private void SendMessage(BasePlayer player, string key, object[] args);
    private string GetTranslation(string key, BasePlayer player);
    public class GetPublishedFileDetailsClass
    {
        public Response response { get; set; }
    }

    public class Response
    {
        public int result { get; set; }
        public int resultcount { get; set; }
        public Publishedfiledetail[] publishedfiledetails { get; set; }
    }

    public class Publishedfiledetail
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
    }

    public class Tag
    {
        public string tag { get; set; }
    }

    public class Message
    {
        public string username { get; set; }
        public string avatar_url { get; set; }
        public List<Embeds> embeds { get; set; }
        public class Fields
        {
            public string name { get; set; }
            public string value { get; set; }
            public bool inline { get; set; }
            public Fields(string name, string value, bool inline);
        }

        public class Footer
        {
            public string text { get; set; }
            public Footer(string text);
        }

        public class Image
        {
            public string url { get; set; }
            public Image(string url);
        }

        public class Embeds
        {
            public string title { get; set; }
            public string description { get; set; }
            public Image image { get; set; }
            public List<Fields> fields { get; set; }
            public Footer footer { get; set; }
            public Embeds(string title, string description, List<Fields> fields, Footer footer, Image image);
        }

        public Message(string username, string avatar_url, List<Embeds> embeds);
    }

    [HookMethod("API_SignText")]
    public void API_SignText(BasePlayer player, Signage sign, string message, int fontsize, string color, string bgcolor, uint textureIndex);
    [HookMethod("API_SkinSign")]
    public void API_SkinSign(BasePlayer player, Signage sign, string url, bool raw, uint textureIndex);
    [HookMethod("API_SkinPhotoFrame")]
    public void API_SkinPhotoFrame(BasePlayer player, PhotoFrame sign, string url, bool raw);
    [HookMethod("API_SkinPumpkin")]
    public void API_SkinPumpkin(BasePlayer player, CarvablePumpkin sign, string url, bool raw);
}

public class SignArtistConfig
{
    [JsonProperty(PropertyName = "Time in seconds between download requests (0 to disable)")]
    public int Cooldown { get; set; }
    [JsonProperty(PropertyName = "Maximum concurrent downloads")]
    public int MaxActiveDownloads { get; set; }
    [JsonProperty(PropertyName = "Maximum distance from the sign")]
    public int MaxDistance { get; set; }
    [JsonProperty(PropertyName = "Maximum filesize in MB")]
    public float MaxSize { get; set; }
    [JsonProperty(PropertyName = "Enforce JPG file format")]
    public bool EnforceJpeg { get; set; }
    [JsonProperty(PropertyName = "JPG image quality (0-100)")]
    public int Quality { get; set; }
    [JsonProperty("Enable logging file")]
    public bool FileLogging { get; set; }
    [JsonProperty("Enable logging console")]
    public bool ConsoleLogging { get; set; }
    [JsonProperty("Enable discord logging")]
    public bool Discordlogging { get; set; }
    [JsonProperty("Discord Webhook")]
    public string DiscordWebhook { get; set; }
    [JsonProperty("Avatar URL")]
    public string AvatarUrl { get; set; }
    [JsonProperty("Discord Username")]
    public string DiscordUsername { get; set; }
    [JsonIgnore]
    public float MaxFileSizeInBytes { get; set; }
    private int quality;
    public static SignArtistConfig DefaultConfig();
}

private class DownloadRequest
{
    public BasePlayer Sender { get; set; }
    public IPaintableEntity Sign { get; set; }
    public string Url { get; set; }
    public bool Raw { get; set; }
    public bool Hor { get; set; }
    public uint TextureIndex { get; set; }
    public DownloadRequest(string url, BasePlayer player, IPaintableEntity sign, bool raw, bool hor, uint textureIndex);
}

private class RestoreRequest
{
    public BasePlayer Sender { get; set; }
    public IPaintableEntity Sign { get; set; }
    public bool Raw { get; set; }
    public uint TextureIndex { get; set; }
    public RestoreRequest(BasePlayer player, IPaintableEntity sign, bool raw, uint textureIndex);
}

public class ImageSize
{
    public int Width { get; set; }
    public int Height { get; set; }
    public int ImageWidth { get; set; }
    public int ImageHeight { get; set; }
    public ImageSize(int width, int height);
    public ImageSize(int width, int height, int imageWidth, int imageHeight);
}

private class ImageDownloader : MonoBehaviour
{
    private byte activeDownloads;
    private byte activeRestores;
    private readonly SignArtist signArtist;
    private readonly Queue<DownloadRequest> downloadQueue;
    private readonly Queue<RestoreRequest> restoreQueue;
    public void QueueDownload(string url, BasePlayer player, IPaintableEntity sign, uint textureIndex, bool raw, bool hor);
    public void QueueRestore(BasePlayer player, IPaintableEntity sign, bool raw, uint textureIndex);
    private void StartNextDownload(bool reduceCount);
    private void StartNextRestore(bool reduceCount);
    private IEnumerator DownloadImage(DownloadRequest request);
    private IEnumerator LogToDiscord(DownloadRequest request);
    private Message DiscordMessage(string servername, string playername, string userid, string itemname, string imgurl, string location);
    private IEnumerator RestoreImage(RestoreRequest request);
    private ImageSize GetImageSizeFor(IPaintableEntity signage);
    private byte[] GetImageBytes(UnityWebRequest www);
}

private abstract class BasePaintableEntity : IPaintableEntity
{
    public BaseEntity Entity { get; set; }
    public string PrefabName { get; set; }
    public string ShortPrefabName { get; set; }
    public NetworkableId NetId { get; set; }
    public virtual int TextureCount { get; set; }
    public abstract uint[] TextureIds { get; set; }
    protected BasePaintableEntity(BaseEntity entity);
    public abstract bool CanUpdate(BasePlayer player);
    public abstract void SetImage(uint textureIndex, uint id);
    public virtual void EnsureInitialized();
    public void SendNetworkUpdate();
}

private class PaintableSignage : BasePaintableEntity, IPaintableEntity
{
    public Signage Sign { get; set; }
    public override int TextureCount { get; set; }
    public override uint[] TextureIds { get; set; }
    public PaintableSignage(Signage sign);
    public override void SetImage(uint textureIndex, uint id);
    public override bool CanUpdate(BasePlayer player);
    public override void EnsureInitialized();
}

private class PaintableFrame : BasePaintableEntity, IPaintableEntity
{
    public PhotoFrame Sign { get; set; }
    public override uint[] TextureIds { get; set; }
    public PaintableFrame(PhotoFrame sign);
    public override void SetImage(uint textureIndex, uint id);
    public override bool CanUpdate(BasePlayer player);
}

private class PaintablePumpkin : BasePaintableEntity, IPaintableEntity
{
    public CarvablePumpkin Pumpkin { get; set; }
    public override uint[] TextureIds { get; set; }
    public PaintablePumpkin(CarvablePumpkin pumpkin);
    public override void SetImage(uint textureIndex, uint id);
    public override bool CanUpdate(BasePlayer player);
    public override void EnsureInitialized();
}

public class GetPublishedFileDetailsClass
{
    public Response response { get; set; }
}

public class Response
{
    public int result { get; set; }
    public int resultcount { get; set; }
    public Publishedfiledetail[] publishedfiledetails { get; set; }
}

public class Publishedfiledetail
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
}

public class Tag
{
    public string tag { get; set; }
}

public class Message
{
    public string username { get; set; }
    public string avatar_url { get; set; }
    public List<Embeds> embeds { get; set; }
    public class Fields
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
        public Fields(string name, string value, bool inline);
    }

    public class Footer
    {
        public string text { get; set; }
        public Footer(string text);
    }

    public class Image
    {
        public string url { get; set; }
        public Image(string url);
    }

    public class Embeds
    {
        public string title { get; set; }
        public string description { get; set; }
        public Image image { get; set; }
        public List<Fields> fields { get; set; }
        public Footer footer { get; set; }
        public Embeds(string title, string description, List<Fields> fields, Footer footer, Image image);
    }

    public Message(string username, string avatar_url, List<Embeds> embeds);
}

public class Fields
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
    public Fields(string name, string value, bool inline);
}

public class Footer
{
    public string text { get; set; }
    public Footer(string text);
}

public class Image
{
    public string url { get; set; }
    public Image(string url);
}

public class Embeds
{
    public string title { get; set; }
    public string description { get; set; }
    public Image image { get; set; }
    public List<Fields> fields { get; set; }
    public Footer footer { get; set; }
    public Embeds(string title, string description, List<Fields> fields, Footer footer, Image image);
}

public static class Extensions
{
    public static byte[] ResizeImage(byte[] bytes, int width, int height, int targetWidth, int targetHeight, bool enforceJpeg, RotateFlipType rotation);
    private static Bitmap ResizeImage(Image image, int width, int height);
    private static void TimestampImage(Bitmap image);
    private static int GetValueAtIndex(byte[] bytes, int index);
    public static string EscapeForUrl(string stringToEscape);
}

SignArtistClasses
public static class Extensions
{
    public static byte[] ResizeImage(byte[] bytes, int width, int height, int targetWidth, int targetHeight, bool enforceJpeg, RotateFlipType rotation);
    private static Bitmap ResizeImage(Image image, int width, int height);
    private static void TimestampImage(Bitmap image);
    private static int GetValueAtIndex(byte[] bytes, int index);
    public static string EscapeForUrl(string stringToEscape);
}


```

---

## SignBan by Tori1157 - Prevents players from updating signs

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Sign Ban", "Tori1157", "1.0.1", ResourceId = 2727)]
[Description("Prevents users from updating signs")]
 class SignBan : CovalencePlugin
{
    public const string BanPermission;
    [PluginReference]
     Plugin SignArtist;
     Plugin ZoneManager;
    private void Init();
    protected override void LoadDefaultMessages();
    [Command("sign")]
    private void SignCommand(IPlayer player, string command, string[] args);
    private bool CanUpdateSign(BasePlayer player, Signage sign);
    private bool CanBanSign(IPlayer player);
    private object OnPlayerCommand(ConsoleSystem.Arg arg);
    private IPlayer GetPlayer(string nameOrID, IPlayer player);
    private bool IsParseableTo(object s);
    private static Dictionary<string, SignBanInfo> bannedply;
    public class SignBanInfo
    {
        public static bool IsSignBanned(IPlayer player);
        public SignBanInfo();
    }

    private bool ZMNoSignUpdates(BasePlayer player);
    private void LoadData(T data, string filename);
    private void SaveData(T data, string filename);
    private string Lang(string key, string id, object[] args);
    private void SendChatMessage(IPlayer player, string message);
}

public class SignBanInfo
{
    public static bool IsSignBanned(IPlayer player);
    public SignBanInfo();
}


```

---

## SignChecker by  - Provides a way to check the last editor of signs

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Facepunch.Unity;
using Facepunch;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info( "Sign Checker", "Scarylaggy", "1.1.5" )]
[Description( "Provides a way to check the last editor of signs" )]
 class SignChecker : RustPlugin
{
    private static readonly string PERMISSION_USE;
    private static readonly string PERMISSION_CLEAN;
     StoredData storedData;
     void Init();
    private void registerPermissions();
     void OnSignUpdated(Signage sign, BasePlayer player, string text);
    private IEnumerable<SignCheckerData> FindSignById(string id);
    private bool hasPermissionUse(BasePlayer player, string perm);
    private string getPlayerDataForSign(Signage sign, string caller);
    [ChatCommand( "signwipe" )]
    private void wipeDataChatCommand(BasePlayer player, string command, string[] args);
    [ChatCommand( "check" )]
    private void CheckChatCommand(BasePlayer player, string command, string[] args);
    private string GetLangValue(string key, string userId);
     bool isAlreadyInStorageData(Signage sign);
    protected override void LoadDefaultMessages();
     class StoredData
    {
        public HashSet<SignCheckerData> signCheckerDatas;
        public StoredData();
    }

     class SignCheckerData
    {
        public string userId;
        public string name;
        public string signId;
        public SignCheckerData();
        public SignCheckerData(BasePlayer player, Signage sign);
        public override string ToString();
    }

}

 class StoredData
{
    public HashSet<SignCheckerData> signCheckerDatas;
    public StoredData();
}

 class SignCheckerData
{
    public string userId;
    public string name;
    public string signId;
    public SignCheckerData();
    public SignCheckerData(BasePlayer player, Signage sign);
    public override string ToString();
}


```

---

## SignHistory by collectvood - Track changes to signs to record malicious edits

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("SignHistory", "collect_vood", "1.1.4")]
[Description("Creates a changelog for signs")]
public class SignHistory : CovalencePlugin
{
    private const string AdminPerm;
    protected override void LoadDefaultMessages();
    private StoredData storedData;
    private Dictionary<ulong, Sign> Signs { get; set; }
    private class StoredData
    {
        [JsonProperty("Signs (Updated 04/05/23)")]
        public Dictionary<ulong, Sign> Signs;
    }

    private class Sign
    {
        [JsonProperty("OwnerId")]
        public string OwnerId;
        [JsonProperty("Changes", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Change> Changes;
        public Sign(string ownerId);
        public class Change
        {
            [JsonProperty("Timestamp")]
            public DateTime Timestamp;
            [JsonProperty("UserId")]
            public string Id;
            public Change(string playerId);
        }

    }

    private void SaveData();
    private void Unload();
    private void OnServerSave();
    private void Init();
    private void OnNewSave(string filename);
    private void OnSignUpdated(BaseEntity sign, BasePlayer player);
    private string GetMessage(string key, IPlayer player, string[] args);
    private bool HasPerm(IPlayer player, string perm);
    private void LogSignChange(BaseEntity sign, BasePlayer player);
    [Command("history")]
    private void cmdHistory(IPlayer player, string command, string[] args);
}

private class StoredData
{
    [JsonProperty("Signs (Updated 04/05/23)")]
    public Dictionary<ulong, Sign> Signs;
}

private class Sign
{
    [JsonProperty("OwnerId")]
    public string OwnerId;
    [JsonProperty("Changes", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Change> Changes;
    public Sign(string ownerId);
    public class Change
    {
        [JsonProperty("Timestamp")]
        public DateTime Timestamp;
        [JsonProperty("UserId")]
        public string Id;
        public Change(string playerId);
    }

}

public class Change
{
    [JsonProperty("Timestamp")]
    public DateTime Timestamp;
    [JsonProperty("UserId")]
    public string Id;
    public Change(string playerId);
}


```

---

## SignMap by MJSU - Allows placing the rust map on a signs

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;
using Color = System.Drawing.Color;
using Component = UnityEngine.Component;
using Graphics = System.Drawing.Graphics;

Oxide.Plugins
[Info("Sign Map", "MJSU", "1.0.4")]
[Description("Allows placing the rust map on a signs")]
internal class SignMap : RustPlugin
{
    [PluginReference]
    private Plugin RustMapApi;
    private PluginConfig _pluginConfig;
    private const string UsePermission;
    private const string NoResourcesPermission;
    private const string NoCooldownPermission;
    private const string AccentColor;
    private readonly Hash<string, ItemDefinition> _prefabNameToItem;
    private readonly Hash<ulong, Coroutine> _activeRoutines;
    private readonly Hash<ulong, DateTime> _cooldowns;
    private readonly Hash<ulong, List<Signage>> _undoSigns;
    private const string DefaultMapName;
    private GameObject _go;
    private SignBehavior _behavior;
    private readonly StringBuilder _sb;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void Unload();
    [ChatCommand("sm")]
    private void SignMapChatCommand(BasePlayer player, string cmd, string[] args);
    private void HandleUndo(BasePlayer player);
    private IEnumerator UndoRoutine(BasePlayer player, List<Signage> signs);
    private void HandleMaps(BasePlayer player);
    private void HandleGen(BasePlayer player, string[] args);
    private IEnumerator CreateSignGrid(BasePlayer player, Signage sign, int numRows, int numCols, ItemDefinition def, string mapName, bool correctRotation);
    private BuildingBlock GetNearbyBuildingBlock(BaseEntity entity);
    private IEnumerator HandleRefund(BasePlayer player, ItemDefinition def, int amount);
    private IEnumerator AddImageToSign(BasePlayer player, Signage sign, byte[] data);
    private byte[] ResizeImage(byte[] bytes, int targetWidth, int targetHeight);
    private static Bitmap ResizeImage(Image image, int width, int height);
    private int GetValueAtIndex(byte[] bytes, int index);
    private bool IsRustMapApiLoaded();
    public Hash<string, object> GetSection(string mapName, int numRows, int numCols, int row, int col);
    private List<string> GetMaps();
    private T RaycastAll(Ray ray, float distance);
    private void Chat(BasePlayer player, string key, object[] args);
    private string Lang(string key, BasePlayer player, object[] args);
    private bool HasPermission(BasePlayer player, string perm);
    private class SignBehavior : FacepunchBehaviour
    {
    }

    private class PluginConfig
    {
        [DefaultValue("sm")]
        [JsonProperty(PropertyName = "Chat Command")]
        public string ChatCommand { get; set; }
        [DefaultValue(0.1f)]
        [JsonProperty(PropertyName = "Delay between sign generation (Seconds)")]
        public float GenerationDelay { get; set; }
        [DefaultValue(120f)]
        [JsonProperty(PropertyName = "Command cooldown (Seconds)")]
        public float Cooldown { get; set; }
        [DefaultValue(16)]
        [JsonProperty(PropertyName = "Max number of signs in generated grid")]
        public int MaxSigns { get; set; }
        [DefaultValue("Icons")]
        [JsonProperty(PropertyName = "Default map to use when non specified")]
        public string DefaultMap { get; set; }
    }

    private class LangKeys
    {
        public const string Chat;
        public const string NoPermission;
        public const string NoSign;
        public const string CanNotUpdate;
        public const string InvalidRow;
        public const string InvalidCol;
        public const string HelpText;
        public const string SignNotSupported;
        public const string ImageNotValid;
        public const string ActiveGeneration;
        public const string NoAvailableUndos;
        public const string UnderCooldown;
        public const string UndoSuccessful;
        public const string InvalidGenSyntax;
        public const string MaxSize;
        public const string NotEnoughItems;
        public const string NeedsWall;
        public const string SignBroke;
        public const string FinishedGenerating;
        public const string Refunded;
        public const string MapHeader;
        public const string MapName;
        public const string MissingRustMapApi;
    }

    private Dictionary<string, ImageSize> SignImageSizes { get; set; }
    private class ImageSize
    {
        public int Width { get; set; }
        public int Height { get; set; }
        public int ImageWidth { get; set; }
        public int ImageHeight { get; set; }
        public ImageSize(int width, int height);
        public ImageSize(int width, int height, int imageWidth, int imageHeight);
    }

}

private class SignBehavior : FacepunchBehaviour
{
}

private class PluginConfig
{
    [DefaultValue("sm")]
    [JsonProperty(PropertyName = "Chat Command")]
    public string ChatCommand { get; set; }
    [DefaultValue(0.1f)]
    [JsonProperty(PropertyName = "Delay between sign generation (Seconds)")]
    public float GenerationDelay { get; set; }
    [DefaultValue(120f)]
    [JsonProperty(PropertyName = "Command cooldown (Seconds)")]
    public float Cooldown { get; set; }
    [DefaultValue(16)]
    [JsonProperty(PropertyName = "Max number of signs in generated grid")]
    public int MaxSigns { get; set; }
    [DefaultValue("Icons")]
    [JsonProperty(PropertyName = "Default map to use when non specified")]
    public string DefaultMap { get; set; }
}

private class LangKeys
{
    public const string Chat;
    public const string NoPermission;
    public const string NoSign;
    public const string CanNotUpdate;
    public const string InvalidRow;
    public const string InvalidCol;
    public const string HelpText;
    public const string SignNotSupported;
    public const string ImageNotValid;
    public const string ActiveGeneration;
    public const string NoAvailableUndos;
    public const string UnderCooldown;
    public const string UndoSuccessful;
    public const string InvalidGenSyntax;
    public const string MaxSize;
    public const string NotEnoughItems;
    public const string NeedsWall;
    public const string SignBroke;
    public const string FinishedGenerating;
    public const string Refunded;
    public const string MapHeader;
    public const string MapName;
    public const string MissingRustMapApi;
}

private class ImageSize
{
    public int Width { get; set; }
    public int Height { get; set; }
    public int ImageWidth { get; set; }
    public int ImageHeight { get; set; }
    public ImageSize(int width, int height);
    public ImageSize(int width, int height, int imageWidth, int imageHeight);
}


```

---

## SignsMonitor by MrBlue - Send a message to discord with an image of the signage content set by the user

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info( "Signs Monitor", "Mr. Blue", "1.0.1" )]
[Description( "Send a message to discord with an image of the signage content set by the user" )]
public class SignsMonitor : RustPlugin
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
    private string FormatMessage(string key, BasePlayer player, string displayName, string steamId, BaseEntity entity);
    private void OnSignUpdated(ISignage signage, BasePlayer player, int textureIndex);
    private void OnItemPainted(PaintedItemStorageEntity entity, Item item, BasePlayer player, byte[] encodedPng);
    private void SendDiscordEmbed(byte[] image, BasePlayer player, ulong signageOwnerId, BaseEntity entity);
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

## SignTracker by DNARust - Tracks who last updated a sign with name and Steam ID, and optional logging

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("SignTracker", "DNARust/Wulf/lukespragg", "3.0.0")]
[Description("Track who last updated a sign with name and Steam ID, and optional logging")]
 class SignTracker : CovalencePlugin
{
    readonly Dictionary<uint, string> signs;
     DynamicConfigFile storedData;
    const string permUse;
     bool logUpdates;
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void LoadDefaultMessages();
    [Command("sign")]
     void SignCommand(IPlayer player, string command, string[] args);
    private void OnSignUpdated(BaseEntity sign, BasePlayer player);
     void SaveSignData();
     void OnServerShutdown();
     void OnServerSave();
     void Unload();
    private void OnNewSave(string filename);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
     void Log(string text);
}


```

---

## SimpleBradleyKillFeed by chrome - Tells you who destroyed Bradley through a discord webhook.

```csharp
using System;
using UnityEngine.Networking;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Simple Bradley Kill Feed", "chrome", "1.0.2")]
[Description("Logs who killed bradley to a webhook.")]
public class SimpleBradleyKillFeed : RustPlugin
{
    private DynamicConfigFile _config;
    private string _webhookurl;
    public class PluginConfig
    {
        [JsonProperty("webhookurl")]
        public string WebhookUrl { get; set; }
    }

    protected override void LoadDefaultConfig();
    private void Init();
    private void OnServerInitialized();
    private void OnEntityDeath(BradleyAPC entity, HitInfo info);
    private void SendDiscordEmbed(BasePlayer player);
}

public class PluginConfig
{
    [JsonProperty("webhookurl")]
    public string WebhookUrl { get; set; }
}


```

---

## SimpleGambling by  - Allows admin to create item gamblings

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Simple Gambling", "Khan", "1.4.0")]
[Description("Allows admins to create item gambling")]
 class SimpleGambling : CovalencePlugin
{
    private const string Use;
    private const string Admin;
     ConfigData configData;
     StoredData storedData;
     class StoredData
    {
        public List<Gamble> gambles;
        public List<Preserved> preserveds;
        public List<Reward> rewards;
        public StoredData();
    }

     class ConfigData
    {
        public bool preservePot;
        public bool showWinners;
    }

    public class Reward
    {
        public ulong id;
        public string item;
        public int quantity;
        public Reward(ulong id, string item, int quantity);
    }

    public class Gamble
    {
        public string name;
        public string item;
        public int numbers;
        public int itemAmount;
        public List<Bets> bets;
        public int preservedPot;
        public Gamble(string name, string item, int itemAmount, int numbers, int preservedPot);
    }

    public class Bets
    {
        public ulong player;
        public int number;
        public Bets(ulong player, int number);
    }

    public class Preserved
    {
        public int amount;
        public string itemName;
        public Preserved(int amount, string itemName);
    }

    private void LoadAll();
    private void SaveAll();
    protected override void LoadDefaultConfig();
    private void Init();
    private string Lang(string key, string id, object[] args);
    private new void LoadDefaultMessages();
    private void PrintToChat(string format, object[] args);
    private string PlayerEqualNull(IPlayer player, string langKey, object[] args);
    private void ReplyOrPutLang(IPlayer player, string langKey, object[] args);
    private void ReplyOrPutString(IPlayer player, string message);
    private int GetGambleIndex(string gambleName);
    private bool DoesItemExist(string name);
    private string GetItemName(string shortname);
    private string TryToBet(BasePlayer player, string name, string number);
    private string TryToSet(string name, string item, string itemA, string number, IPlayer player);
    private void CreateGamble(string name, string iName, int iAmount, int numbers, IPlayer player);
    private int GetAndDeletePreservedPot(string itemName);
    private void AddToPreservedPot(string itemName, int amount);
    private void TryFinishGamble(string name, IPlayer player);
    private void ShowCurrentGambles(IPlayer player);
    private string TryGetReward(BasePlayer player);
    private void TryChangeConfig(string variable, string newValue, IPlayer player);
    [Command("gamble")]
    private void CmdGamble(IPlayer player, string command, string[] args);
    private void Commands(IPlayer player, string[] args);
}

 class StoredData
{
    public List<Gamble> gambles;
    public List<Preserved> preserveds;
    public List<Reward> rewards;
    public StoredData();
}

 class ConfigData
{
    public bool preservePot;
    public bool showWinners;
}

public class Reward
{
    public ulong id;
    public string item;
    public int quantity;
    public Reward(ulong id, string item, int quantity);
}

public class Gamble
{
    public string name;
    public string item;
    public int numbers;
    public int itemAmount;
    public List<Bets> bets;
    public int preservedPot;
    public Gamble(string name, string item, int itemAmount, int numbers, int preservedPot);
}

public class Bets
{
    public ulong player;
    public int number;
    public Bets(ulong player, int number);
}

public class Preserved
{
    public int amount;
    public string itemName;
    public Preserved(int amount, string itemName);
}


```

---

## SimpleKillFeed by KrunghCrow - A Customizable  kill feed that displays  various kill events in the top right corner

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Runtime.Serialization;
using System.Text;
using Facepunch;
using Facepunch.Math;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using Rust.Ai.Gen2;
using UnityEngine;
using ProtoBuf;

Oxide.Plugins
[Info("Simple Kill Feed", "Krungh Crow", "2.4.2")]
[Description("A kill feed, that displays various kill events in the top right corner.")]
public class SimpleKillFeed : RustPlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private Plugin Clans;
    private bool _isClansReborn;
    private bool _isClansIo;
    private bool _isClansMevent;
    private readonly Dictionary<uint, string> _itemNameMapping;
    private GameObject _holder;
    private KillQueue _killQueue;
    private static SKFConfig configdata;
    private static SimpleKillFeedData _data;
    const string Exclude_Perm;
    private class SKFConfig
    {
        [JsonProperty("Show Traps and Entitys in Kill Feed")]
        public bool EnableEntityFeed;
        [JsonProperty("Show Animals kills (default true)")]
        public bool EnableAnimalFeed;
        [JsonProperty("Show Npcs kills (Default true)")]
        public bool EnableNpcFeed;
        [JsonProperty("Show suicides (Default: true)")]
        public bool EnableSuicides;
        [JsonProperty("Show Deaths by Animals (Default: true)")]
        public bool EnableAnimal;
        [JsonProperty("Show Deaths by Cold (Default: true)")]
        public bool EnableCold;
        [JsonProperty("Show deaths by Drowning (Default: true)")]
        public bool EnableDrowning;
        [JsonProperty("Show Deaths by Fall (Default: true)")]
        public bool EnableFall;
        [JsonProperty("Show Deaths by Hunger (Default: true)")]
        public bool EnableHunger;
        [JsonProperty("Show Deaths by Electricution (Default: true)")]
        public bool EnableElectricution;
        [JsonProperty("Show Deaths by Radiation (Default: true)")]
        public bool EnableRadiationKills;
        [JsonProperty("Chat Icon Id (Steam profile ID)")]
        public ulong IconId;
        [JsonProperty("Max messages in feed (Default: 5)")]
        public int MaxFeedMessages;
        [JsonProperty("Max player name length in feed (Default: 18)")]
        public int MaxPlayerNameLength;
        [JsonProperty("Feed message TTL in seconds (Default: 7)")]
        public int FeedMessageTtlSec;
        [JsonProperty("Allow kill messages in chat along with kill feed")]
        public bool EnableChatFeed;
        [JsonProperty("Log PvP Kill events")]
        public bool EnableLogging;
        [JsonProperty("Height ident (space between messages). Default: 0.0185")]
        public float HeightIdent;
        [JsonProperty("Feed Position - Anchor Max. (Default: 0.995 0.986")]
        public string AnchorMax;
        [JsonProperty("Feed Position - Anchor Min. (Default: 0.723 0.964")]
        public string AnchorMin;
        [JsonProperty("Font size of kill feed (Default: 12)")]
        public int FontSize;
        [JsonProperty("Default textanchor (left or right)")]
        public string TextAnchor;
        [JsonProperty("Outline Text Size (Default: 0.5 0.5)")]
        public string OutlineSize;
        [JsonProperty("Default color for distance (if too far from any from the list). Default: #FF8000")]
        public string DefaultDistanceColor;
        [JsonProperty("Distance Colors List (Certain color will apply if distance is <= than specified)")]
        public DistanceColor[] DistanceColors;
        [JsonProperty("Custom Entity Names, you can remove or add more!")]
        public Dictionary<string, string> Ents;
        [JsonProperty("Custom Animal Names, you can remove or add more!")]
        public Dictionary<string, string> Animal;
        [JsonProperty("Custom Weapon Names, you can add more!")]
        public Dictionary<string, string> Weapons;
        [JsonProperty("Custom Npc Names, you can add more!")]
        public Dictionary<string, string> Npcs;
        [OnDeserialized]
        internal void OnDeserialized(StreamingContext ctx);
        public class DistanceColor
        {
            public int DistanceThreshold;
            public string Color;
            public bool TestDistance(int distance);
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    [ProtoContract(ImplicitFields = ImplicitFields.AllPublic)]
    private class SimpleKillFeedData
    {
        public HashSet<ulong> DisabledUsers;
    }

    private void LoadData();
    private void SaveData();
    [ChatCommand("feed")]
    private void ToggleFeed(BasePlayer player);
    private void OnServerInitialized();
    private void Init();
    private void Unload();
    private void OnEntityDeath(BaseEntity victim, HitInfo hitInfo);
    private void OnEntityDeath(SimpleShark victim, HitInfo hitInfo);
    private void OnPlayerDeath(BasePlayer victim, HitInfo hitInfo);
    protected override void LoadDefaultMessages();
    private void SendKillfeedmessage(string msg);
    private void OnKilled(BasePlayer attacker, BasePlayer victim, HitInfo hitInfo, int dist);
    private void OnSuicide(BasePlayer victim);
    private void OnKilledByRadiation(BasePlayer victim);
    private void OnKilledByEnt(BaseEntity attacker, BasePlayer victim);
    private void OnKilledByBradley(BasePlayer victim);
    private void OnKilledByAnimal(BaseEntity attacker, BasePlayer victim);
    private void DeathByFall(BasePlayer victim);
    private void OnDeathFromWounds(BasePlayer attacker, BasePlayer victim);
    private void OnBurned(BasePlayer attacker, BasePlayer victim);
    private void OnDrowning(BasePlayer victim);
    private void OnFrozen(BasePlayer victim);
    private void OnHunger(BasePlayer victim);
    private void OnShock(BasePlayer victim);
    private void OnSentry(BasePlayer victim);
    private void OnCactus(BasePlayer victim);
    private class KillEvent : Pool.IPooled
    {
        public int DisplayUntil;
        public string Text;
        public KillEvent Init(string text, int displayUntil);
        public void EnterPool();
        public void LeavePool();
    }

    private class KillQueue : MonoBehaviour
    {
        private readonly WaitForSeconds _secondDelay;
        private readonly Queue<KillEvent> _queue;
        private readonly CuiOutlineComponent _outlineStatic;
        private readonly CuiRectTransformComponent[] _rectTransformStatic;
        private readonly CuiTextComponent[] _textStatic;
        private readonly CuiElementContainer _cui;
        private bool _needsRedraw;
        private int _currentlyDrawn;
        private SimpleKillFeed _plugin;
        public void SetPlugin(SimpleKillFeed plugin);
        public void OnDeath(BasePlayer victim, BasePlayer attacker, string text);
        private void PushEvent(KillEvent evt);
        private void Start();
        private void DequeueEvent(KillEvent evt);
        private void DoProccessQueue();
        private IEnumerator ProccessQueue();
        private void SendKillCui(CuiElementContainer cui, int toBeRemoved);
        public static void RemoveKillCui(string name);
    }

    private string _(string msg, BasePlayer player);
    private string GetCustomWeaponName(HitInfo hitInfo);
    private string CustomNpcName(BasePlayer npc);
    private string CustomEntName(BaseEntity attacker);
    private string CustomAnimalName(BaseEntity attacker);
    private string GetWeaponName(HitInfo hitInfo);
    private static bool IsExplosion(HitInfo hit);
    private static bool IsFlame(HitInfo hit);
    private static bool IsRadiation(HitInfo hit);
    private static bool IsCold(HitInfo hit);
    private static bool IsHunger(HitInfo hit);
    private static bool IsThirst(HitInfo hit);
    private static bool IsDrowning(HitInfo hit);
    private static bool IsPoison(HitInfo hit);
    private static bool IsShock(HitInfo hit);
    private static bool IsFall(HitInfo hit);
    private static bool IsCar(HitInfo hit);
    private static bool IsTrap(BaseEntity ent);
    private static bool IsAnimal(BaseEntity animal);
    private static bool IsZombieHorde(BasePlayer player);
    private static string GetDistanceColor(int dist);
    private string GetClan(BasePlayer player);
    private static string SanitizeName(string name);
}

private class SKFConfig
{
    [JsonProperty("Show Traps and Entitys in Kill Feed")]
    public bool EnableEntityFeed;
    [JsonProperty("Show Animals kills (default true)")]
    public bool EnableAnimalFeed;
    [JsonProperty("Show Npcs kills (Default true)")]
    public bool EnableNpcFeed;
    [JsonProperty("Show suicides (Default: true)")]
    public bool EnableSuicides;
    [JsonProperty("Show Deaths by Animals (Default: true)")]
    public bool EnableAnimal;
    [JsonProperty("Show Deaths by Cold (Default: true)")]
    public bool EnableCold;
    [JsonProperty("Show deaths by Drowning (Default: true)")]
    public bool EnableDrowning;
    [JsonProperty("Show Deaths by Fall (Default: true)")]
    public bool EnableFall;
    [JsonProperty("Show Deaths by Hunger (Default: true)")]
    public bool EnableHunger;
    [JsonProperty("Show Deaths by Electricution (Default: true)")]
    public bool EnableElectricution;
    [JsonProperty("Show Deaths by Radiation (Default: true)")]
    public bool EnableRadiationKills;
    [JsonProperty("Chat Icon Id (Steam profile ID)")]
    public ulong IconId;
    [JsonProperty("Max messages in feed (Default: 5)")]
    public int MaxFeedMessages;
    [JsonProperty("Max player name length in feed (Default: 18)")]
    public int MaxPlayerNameLength;
    [JsonProperty("Feed message TTL in seconds (Default: 7)")]
    public int FeedMessageTtlSec;
    [JsonProperty("Allow kill messages in chat along with kill feed")]
    public bool EnableChatFeed;
    [JsonProperty("Log PvP Kill events")]
    public bool EnableLogging;
    [JsonProperty("Height ident (space between messages). Default: 0.0185")]
    public float HeightIdent;
    [JsonProperty("Feed Position - Anchor Max. (Default: 0.995 0.986")]
    public string AnchorMax;
    [JsonProperty("Feed Position - Anchor Min. (Default: 0.723 0.964")]
    public string AnchorMin;
    [JsonProperty("Font size of kill feed (Default: 12)")]
    public int FontSize;
    [JsonProperty("Default textanchor (left or right)")]
    public string TextAnchor;
    [JsonProperty("Outline Text Size (Default: 0.5 0.5)")]
    public string OutlineSize;
    [JsonProperty("Default color for distance (if too far from any from the list). Default: #FF8000")]
    public string DefaultDistanceColor;
    [JsonProperty("Distance Colors List (Certain color will apply if distance is <= than specified)")]
    public DistanceColor[] DistanceColors;
    [JsonProperty("Custom Entity Names, you can remove or add more!")]
    public Dictionary<string, string> Ents;
    [JsonProperty("Custom Animal Names, you can remove or add more!")]
    public Dictionary<string, string> Animal;
    [JsonProperty("Custom Weapon Names, you can add more!")]
    public Dictionary<string, string> Weapons;
    [JsonProperty("Custom Npc Names, you can add more!")]
    public Dictionary<string, string> Npcs;
    [OnDeserialized]
    internal void OnDeserialized(StreamingContext ctx);
    public class DistanceColor
    {
        public int DistanceThreshold;
        public string Color;
        public bool TestDistance(int distance);
    }

}

public class DistanceColor
{
    public int DistanceThreshold;
    public string Color;
    public bool TestDistance(int distance);
}

[ProtoContract(ImplicitFields = ImplicitFields.AllPublic)]
private class SimpleKillFeedData
{
    public HashSet<ulong> DisabledUsers;
}

private class KillEvent : Pool.IPooled
{
    public int DisplayUntil;
    public string Text;
    public KillEvent Init(string text, int displayUntil);
    public void EnterPool();
    public void LeavePool();
}

private class KillQueue : MonoBehaviour
{
    private readonly WaitForSeconds _secondDelay;
    private readonly Queue<KillEvent> _queue;
    private readonly CuiOutlineComponent _outlineStatic;
    private readonly CuiRectTransformComponent[] _rectTransformStatic;
    private readonly CuiTextComponent[] _textStatic;
    private readonly CuiElementContainer _cui;
    private bool _needsRedraw;
    private int _currentlyDrawn;
    private SimpleKillFeed _plugin;
    public void SetPlugin(SimpleKillFeed plugin);
    public void OnDeath(BasePlayer victim, BasePlayer attacker, string text);
    private void PushEvent(KillEvent evt);
    private void Start();
    private void DequeueEvent(KillEvent evt);
    private void DoProccessQueue();
    private IEnumerator ProccessQueue();
    private void SendKillCui(CuiElementContainer cui, int toBeRemoved);
    public static void RemoveKillCui(string name);
}


```

---

## SimpleLogo by sami37 - Displays a logo anywhere on the player's screen

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("SimpleLogo", "Sami37", "1.2.8")]
[Description("Place your own logo to your player screen.")]
public class SimpleLogo : RustPlugin
{
    [PluginReference]
     ImageLibrary ImageLibrary;
    private string Perm;
    private string NoDisplay;
     List<object> _urlList;
    private int _currentlySelected;
    private int _intervals;
    private Dictionary<ulong, bool> playerHide;
    protected override void LoadDefaultConfig();
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
    private string GetImage(string shortname);
     void LoadConfig();
    private void LoadOrder(string title, Dictionary<string, string> importImageList, ulong skin, bool force);
     void Unload();
    private CuiElement CreateImage(string panelName);
     void GUIDestroy(BasePlayer player);
     void CreateUi(BasePlayer player);
     void RefreshUi();
     void OnServerInitialized();
    [ChatCommand("SL")]
     void chatCmd(BasePlayer player, string command, string[] args);
}


```

---

## SimpleLoot by Jacob - Sets multipliers for any item of your choosing

```csharp
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Simple Loot", "MisterPixie", "1.0.91")]
[Description("Sets multipliers for any item of your choosing")]
public class SimpleLoot : RustPlugin
{
    private static SimpleLoot _instance;
    private Configuration _configuration;
    private void Init();
    private void OnServerInitialized();
    private object OnLootSpawn(LootContainer lootContainer);
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        public readonly Dictionary<string, object> Multipliers;
        public readonly bool ReplaceItems;
        public Configuration();
        private void GetConfig(T variable, string[] path);
        private void SetConfig(T variable, string[] path);
    }

}

private class Configuration
{
    public readonly Dictionary<string, object> Multipliers;
    public readonly bool ReplaceItems;
    public Configuration();
    private void GetConfig(T variable, string[] path);
    private void SetConfig(T variable, string[] path);
}


```

---

## SimpleNoVehicleFuel by Mabel - Removes requirement of fuel in all vehicles

```csharp
using Oxide.Core.Plugins;
using System.Linq;

Oxide.Plugins
[Info("Simple No Vehicle Fuel", "Mabel", "1.1.0")]
[Description("Removes requirement of fuel in all vehicles")]
public class SimpleNoVehicleFuel : RustPlugin
{
    [PluginReference]
    readonly Plugin Convoy;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(BaseVehicle vehicle);
    private void OnEntitySpawned(HotAirBalloon balloon);
    private void ModifyVehicle(BaseVehicle vehicle);
    private void ModifyBalloon(HotAirBalloon balloon);
    private void ResetVehicle(BaseVehicle vehicle);
    private void ResetBalloon(HotAirBalloon balloon);
    private void Refill(Item item);
}


```

---

## SimpleSort by birthdates - A UI supported sorting system for storage, based for performance and simplicity

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Simple Sort", "birthdates", "1.0.7")]
[Description("A UI supported sorting system for storage, based for performance and simplicity")]
public class SimpleSort : RustPlugin
{
    private const string permission_use;
    private CuiElementContainer cuiElements;
    private void Init();
    private void OnLootEntity(BasePlayer player, StorageContainer entity);
    private void OnLootEntityEnd(BasePlayer player);
    private static void CloseUI(BasePlayer Player);
    private void OpenUI(BasePlayer Player);
    [ConsoleCommand("SimpleSort:Sort")]
    private void ConsoleCommand(ConsoleSystem.Arg Arg);
    private ConfigFile _config;
    public class UI
    {
        [JsonProperty("Anchor Max")]
        public string AnchorMax;
        [JsonProperty("Anchor Min")]
        public string AnchorMin;
        [JsonProperty("Hex Color")]
        public string Color;
        [JsonProperty("Font Size")]
        public int FontSize;
        [JsonProperty("Text")]
        public string Text;
        [JsonProperty("Text Color")]
        public string TextColor;
    }

    public class ConfigFile
    {
        [JsonProperty("UI Settings")]
        public UI Ui;
        [JsonProperty("Blocked Items (short or long prefabs accepted)")]
        public List<string> Blocked;
        public static ConfigFile Default();
    }

    private static string ToUnityColor(string hexColor);
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class UI
{
    [JsonProperty("Anchor Max")]
    public string AnchorMax;
    [JsonProperty("Anchor Min")]
    public string AnchorMin;
    [JsonProperty("Hex Color")]
    public string Color;
    [JsonProperty("Font Size")]
    public int FontSize;
    [JsonProperty("Text")]
    public string Text;
    [JsonProperty("Text Color")]
    public string TextColor;
}

public class ConfigFile
{
    [JsonProperty("UI Settings")]
    public UI Ui;
    [JsonProperty("Blocked Items (short or long prefabs accepted)")]
    public List<string> Blocked;
    public static ConfigFile Default();
}


```

---

## SimpleTime by MadKingCraig - Provides a chat command for the current game time.

```csharp
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Core;
using System;
using System.Text;
using System.Collections.Generic;
using System.Globalization;
using UnityEngine;

Oxide.Plugins
[Info("Simple Time", "MadKingCraig", "1.1.2")]
[Description("Provides a chat command for the current game time")]
public class SimpleTime : RustPlugin
{
    private bool _initialized;
    private int _componentSearchAttempts;
    private const string CanUsePermission;
    private void Init();
    private void Loaded();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    [HookMethod("GetSimpleTime")]
    public string GetSimpleTime();
    [ChatCommand("time")]
    private void TimeCommand(BasePlayer player, string command, string[] args);
}


```

---

## Sirens by ZockiRR - Gives players with permissions the ability to attach sirens to vehicles.

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Rust.Instruments;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using static InstrumentKeyController;

Oxide.Plugins
[Info("Sirens", "ZockiRR", "2.2.0")]
[Description("Gives players the ability to attach sirens to vehicles")]
 class Sirens : CovalencePlugin
{
    private const string PERMISSION_ATTACHSIRENS;
    private const string PERMISSION_DETACHSIRENS;
    private const string PERMISSION_ATTACHSIRENS_GLOBAL;
    private const string PERMISSION_DETACHSIRENS_GLOBAL;
    private const string I18N_MISSING_SIREN;
    private const string I18N_COULD_NOT_ATTACH;
    private const string I18N_NOT_SUPPORTED;
    private const string I18N_ATTACHED;
    private const string I18N_ATTACHED_GLOBAL;
    private const string I18N_DETACHED;
    private const string I18N_DETACHED_GLOBAL;
    private const string I18N_NOT_A_VEHICLE;
    private const string I18N_SIRENS;
    private const string I18N_PLAYERS_ONLY;
    private const string PREFAB_COCKPIT;
    private const string PREFAB_COCKPIT_ARMORED;
    private const string PREFAB_COCKPIT_WITH_ENGINE;
    private const string PREFAB_BUTTON;
    private const string PREFAB_FLASHERLIGHT;
    private const string PREFAB_SIRENLIGHT;
    private const string PREFAB_SIRENLIGHT_ORANGE;
    private const string PREFAB_SIRENLIGHT_GREEN;
    private const string PREFAB_SIRENLIGHT_BLUE;
    private const string PREFAB_TRUMPET;
    private const string PREFAB_SEDAN;
    private const string PREFAB_SEDAN_RAIL;
    private const string PREFAB_MINICOPTER;
    private const string PREFAB_TRANSPORTHELI;
    private const string PREFAB_RHIB;
    private const string PREFAB_ROWBOAT;
    private const string PREFAB_WORKCART;
    private const string PREFAB_WORKCART_ABOVE_GROUND;
    private const string PREFAB_WORKCART_COVERED;
    private const string PREFAB_MAGNETCRANE;
    private const string PREFAB_HORSE;
    private const string PREFAB_ATTACKHELI;
    private const string PREFAB_TUGBOAT;
    private const string KEY_MODULAR_CAR;
    private const string DATAPATH_SIRENS;
    private static readonly Siren SIREN_DEFAULT;
    private static readonly Siren SIREN_SILENT;
    private static readonly Siren SIREN_ORANGE;
    private static readonly Siren SIREN_GREEN;
    private static readonly Siren SIREN_BLUE;
    private class DataContainer
    {
        public Dictionary<ulong, VehicleContainer> VehicleSirenMap;
    }

    private class VehicleContainer
    {
        public string SirenName;
        public SirenController.States State;
        public HashSet<ulong> NetIDs;
        public VehicleContainer();
        public VehicleContainer(string aSirenName, SirenController.States aState, IEnumerable<NetworkableId> someNetIDs);
    }

    private Configuration config;
    private IDictionary<string, Siren> SirenDictionary { get; set; }
    private class Configuration
    {
        [JsonProperty("MountNeeded")]
        public bool MountNeeded;
        [JsonProperty("SoundEnabled")]
        public bool SoundEnabled;
        [JsonProperty("SirenSpawnProbability")]
        public Dictionary<string, float> SirenSpawnProbability;
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty("DefaultState")]
        public SirenController.States DefaultState;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private class Tone
    {
        public Tone(Notes aNote, NoteType aNoteType, int anOctave, float aDuration);
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty("Note")]
        public Notes Note;
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty("NoteType")]
        public NoteType NoteType;
        [JsonProperty("Octave")]
        public int Octave;
        [JsonProperty("Duration")]
        public float Duration;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private class Siren
    {
        public Siren(string aName, Dictionary<string, Attachment[]> someModules, Dictionary<string, Attachment[]> someVehicles, Tone[] someTones);
        [JsonProperty("Name")]
        public string Name;
        [JsonProperty("Tones")]
        public Tone[] Tones;
        [JsonProperty("Modules")]
        public Dictionary<string, Attachment[]> Modules;
        [JsonProperty("Vehicles")]
        public Dictionary<string, Attachment[]> Vehicles;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private class Attachment
    {
        public Attachment(string aPrefab, Vector3 aPosition, Vector3 anAngle, string aBone);
        [JsonProperty("Prefab")]
        public string Prefab;
        [JsonProperty("Position")]
        public Vector3 Position;
        [JsonProperty("Angle")]
        public Vector3 Angle;
        [JsonProperty("Bone")]
        public string Bone;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected void SaveDefaultSirens();
    protected override void LoadDefaultMessages();
    [Command("attachsirens"), Permission(PERMISSION_ATTACHSIRENS)]
    private void AttachCarSirens(IPlayer aPlayer, string aCommand, string[] someArgs);
    [Command("detachsirens"), Permission(PERMISSION_DETACHSIRENS)]
    private void DetachCarSirens(IPlayer aPlayer, string aCommand, string[] _);
    [Command("attachallsirens"), Permission(PERMISSION_ATTACHSIRENS_GLOBAL)]
    private void AttachAllCarSirens(IPlayer aPlayer, string _, string[] someArgs);
    [Command("detachallsirens"), Permission(PERMISSION_DETACHSIRENS_GLOBAL)]
    private void DetachAllCarSirens(IPlayer aPlayer, string _, string[] _1);
    [Command("listsirens")]
    private void ListSirens(IPlayer aPlayer, string _, string[] _1);
    [Command("togglesirens")]
    private void ToggleSirens(IPlayer aPlayer, string aCommand, string[] _);
    private void Unload();
    private void OnServerSave();
    private void OnServerInitialized(bool _);
    private object OnButtonPress(PressButton aButton, BasePlayer aPlayer);
    private void OnEntitySpawned(BaseVehicle aVehicle);
    private bool AttachSirens(BaseVehicle aVehicle, Siren aSiren, SirenController.States anInitialState, IPlayer aPlayer);
    private bool SpawnAttachments(IDictionary<string, Attachment[]> someAttachments, IPlayer aPlayer, SirenController theController, BaseEntity aParent);
    private SirenController CreateSirenController(BaseVehicle aVehicle, Siren aSiren, IEnumerable<ulong> someNetIDs);
    private bool DetachSirens(BaseVehicle aVehicle);
    private static void Destroy(BaseEntity anEntity);
    private BaseEntity AttachEntity(BaseEntity aParent, string aPrefab, Vector3 aPosition, Vector3 anAngle, string aBone);
    private static void ToogleSirens(IOEntity anIOEntity, bool theEnabledFlag);
    private BaseVehicle RaycastVehicle(IPlayer aPlayer);
    private Siren FindSirenForName(string aName, IPlayer aPlayer);
    private string GetText(string aKey, string aPlayerId, object[] someArgs);
    private void Message(IPlayer aPlayer, string anI18nKey, object[] someArgs);
    private void Message(BasePlayer aPlayer, string anI18nKey, object[] someArgs);
    private class SirenController : FacepunchBehaviour
    {
        private BaseVehicle vehicle;
        private InstrumentTool trumpet;
        public Configuration Config { get; set; }
        public States State { get; set; }
        public Siren Siren { get; set; }
        public ISet<NetworkableId> NetIDs { get; set; }
        public States ChangeState();
        public void SetState(States aState);
        public void RefreshSirenState();
        private InstrumentTool GetTrumpet();
        private BaseVehicle GetVehicle();
        private void PlayTone(int anIndex);
    }

}

private class DataContainer
{
    public Dictionary<ulong, VehicleContainer> VehicleSirenMap;
}

private class VehicleContainer
{
    public string SirenName;
    public SirenController.States State;
    public HashSet<ulong> NetIDs;
    public VehicleContainer();
    public VehicleContainer(string aSirenName, SirenController.States aState, IEnumerable<NetworkableId> someNetIDs);
}

private class Configuration
{
    [JsonProperty("MountNeeded")]
    public bool MountNeeded;
    [JsonProperty("SoundEnabled")]
    public bool SoundEnabled;
    [JsonProperty("SirenSpawnProbability")]
    public Dictionary<string, float> SirenSpawnProbability;
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty("DefaultState")]
    public SirenController.States DefaultState;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class Tone
{
    public Tone(Notes aNote, NoteType aNoteType, int anOctave, float aDuration);
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty("Note")]
    public Notes Note;
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty("NoteType")]
    public NoteType NoteType;
    [JsonProperty("Octave")]
    public int Octave;
    [JsonProperty("Duration")]
    public float Duration;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class Siren
{
    public Siren(string aName, Dictionary<string, Attachment[]> someModules, Dictionary<string, Attachment[]> someVehicles, Tone[] someTones);
    [JsonProperty("Name")]
    public string Name;
    [JsonProperty("Tones")]
    public Tone[] Tones;
    [JsonProperty("Modules")]
    public Dictionary<string, Attachment[]> Modules;
    [JsonProperty("Vehicles")]
    public Dictionary<string, Attachment[]> Vehicles;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class Attachment
{
    public Attachment(string aPrefab, Vector3 aPosition, Vector3 anAngle, string aBone);
    [JsonProperty("Prefab")]
    public string Prefab;
    [JsonProperty("Position")]
    public Vector3 Position;
    [JsonProperty("Angle")]
    public Vector3 Angle;
    [JsonProperty("Bone")]
    public string Bone;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class SirenController : FacepunchBehaviour
{
    private BaseVehicle vehicle;
    private InstrumentTool trumpet;
    public Configuration Config { get; set; }
    public States State { get; set; }
    public Siren Siren { get; set; }
    public ISet<NetworkableId> NetIDs { get; set; }
    public States ChangeState();
    public void SetState(States aState);
    public void RefreshSirenState();
    private InstrumentTool GetTrumpet();
    private BaseVehicle GetVehicle();
    private void PlayTone(int anIndex);
}


```

---

## SkinBlocker by  - Blocks certain skins from being applied to items / deployables

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("SkinBlocker", "redBDGR", "1.0.0")]
[Description("Block certain skins from being applied to items / deployables")]
 class SkinBlocker : RustPlugin
{
    private static bool Changed;
    private class ConfigFile
    {
        public static List<ulong> itemSkinBlacklist;
        public static List<ulong> deployableSkinBlacklist;
        public static List<object> GetDefaultItemSkinList();
        public static List<object> GetDefaultDeployableSkinList();
    }

    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void Init();
    private void OnServerInitialized();
    private void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnEntitySpawned(BaseNetworkable entity);
    private bool CheckEntity(BaseEntity ent);
    private bool CheckItem(Item item);
    private void ScanAllItems();
    private IEnumerator CheckAllEntities();
    private IEnumerator CheckAllItems();
}

private class ConfigFile
{
    public static List<ulong> itemSkinBlacklist;
    public static List<ulong> deployableSkinBlacklist;
    public static List<object> GetDefaultItemSkinList();
    public static List<object> GetDefaultDeployableSkinList();
}


```

---

## Skins by misticos - Change workshop skins of items easily

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Facepunch;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("Skins", "misticos", "2.2.3")]
[Description("Change workshop skins of items easily")]
 class Skins : RustPlugin
{
    private static Skins _ins;
    private Dictionary<ulong, ContainerController> _controllers;
    private Dictionary<ItemContainerId, ContainerController> _controllersPerContainer;
    private HashSet<ItemContainerId> _itemAttachmentContainers;
    private const string PermissionUse;
    private const string PermissionAdmin;
    private const string CommandDefault;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Commands")]
        public string[] Commands;
        [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<SkinItem> Skins;
        [JsonIgnore]
        public Dictionary<string, List<SkinItem>> IndexedSkins;
        [JsonProperty(PropertyName = "Container Panel Name")]
        public string Panel;
        [JsonProperty(PropertyName = "Container Capacity")]
        public int Capacity;
        [JsonProperty(PropertyName = "UI")]
        public UIConfiguration UI;
        public class SkinItem
        {
            [JsonProperty(PropertyName = "Item Shortname")]
            public string Shortname;
            [JsonProperty(PropertyName = "Permission")]
            public string Permission;
            [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<ulong> Skins;
            public static IEnumerable<SkinItem> Find(IPlayer player, string shortname);
            public bool CanUse(IPlayer player);
        }

        public class UIConfiguration
        {
            [JsonProperty(PropertyName = "Background Color")]
            public string BackgroundColor;
            [JsonProperty(PropertyName = "Background Anchors")]
            public Anchors BackgroundAnchors;
            [JsonProperty(PropertyName = "Background Offsets")]
            public Offsets BackgroundOffsets;
            [JsonProperty(PropertyName = "Left Button Text")]
            public string LeftText;
            [JsonProperty(PropertyName = "Left Button Color")]
            public string LeftColor;
            [JsonProperty(PropertyName = "Left Button Anchors")]
            public Anchors LeftAnchors;
            [JsonProperty(PropertyName = "Center Button Text")]
            public string CenterText;
            [JsonProperty(PropertyName = "Center Button Color")]
            public string CenterColor;
            [JsonProperty(PropertyName = "Center Button Anchors")]
            public Anchors CenterAnchors;
            [JsonProperty(PropertyName = "Right Button Text")]
            public string RightText;
            [JsonProperty(PropertyName = "Right Button Color")]
            public string RightColor;
            [JsonProperty(PropertyName = "Right Button Anchors")]
            public Anchors RightAnchors;
            [JsonIgnore]
            public string ParsedUI;
            [JsonIgnore]
            public int IndexPagePrevious;
            public int IndexPageCurrent;
            public int IndexPageNext;
            public class Anchors
            {
                [JsonProperty(PropertyName = "Anchor Min X")]
                public string AnchorMinX;
                [JsonProperty(PropertyName = "Anchor Min Y")]
                public string AnchorMinY;
                [JsonProperty(PropertyName = "Anchor Max X")]
                public string AnchorMaxX;
                [JsonProperty(PropertyName = "Anchor Max Y")]
                public string AnchorMaxY;
                [JsonIgnore]
                public string AnchorMin { get; set; }
                [JsonIgnore]
                public string AnchorMax { get; set; }
            }

            public class Offsets
            {
                [JsonProperty(PropertyName = "Offset Min X")]
                public string OffsetMinX;
                [JsonProperty(PropertyName = "Offset Min Y")]
                public string OffsetMinY;
                [JsonProperty(PropertyName = "Offset Max X")]
                public string OffsetMaxX;
                [JsonProperty(PropertyName = "Offset Max Y")]
                public string OffsetMaxY;
                [JsonIgnore]
                public string OffsetMin { get; set; }
                [JsonIgnore]
                public string OffsetMax { get; set; }
            }

        }

        public void IndexSkins();
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void GenerateUI();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnItemSplit(Item item, int amount);
    private void OnItemAddedToContainer(ItemContainer itemContainer, Item item);
    private void OnItemRemovedFromContainer(ItemContainer itemContainer, Item item);
    private void OnPlayerLootEnd(PlayerLoot loot);
    private object CanLootPlayer(BasePlayer looter, BasePlayer target);
    private object CanMoveItem(Item item, PlayerInventory playerLoot, ItemContainerId targetContainerId, int slot, int amount);
    private object CanMoveItemTo(ContainerController controller, Item item, int slot, int amount);
    private void CommandSkin(IPlayer player, string command, string[] args);
    [HookMethod(nameof(SkinsClose))]
    private void SkinsClose(BasePlayer player);
    [HookMethod(nameof(PurgeCache))]
    private void PurgeCache(ulong id, string shortname);
    private class ContainerController
    {
        public BasePlayer Owner;
        public ItemContainer Container;
        public bool IsOpened;
        public Dictionary<string, List<ulong>> TotalSkinsCache;
        private List<Item> _storedContent;
        private Magazine _storedMagazine;
        public ContainerController(BasePlayer player);
        private void DestroyUI();
        private void DrawUI(int page);
        public void Close();
        public void Show();
        public bool CanShow();
        private static bool CanShow(BasePlayer player);
        private void AddItemContainer(Item item);
        public void GiveItemBack(Item itemOverride);
        public void OnItemTaken(Item item);
        public void SetupContent(Item destination);
        public void StoreContent(Item source);
        public void Clear();
        public void Destroy();
        public void UpdateContent(int page);
        private bool IsValid();
        private bool CanUse();
        private Item GetDuplicateItem(Item item, ulong skin);
        private void MoveItem(Item item, ItemContainer container, int slot);
        private void RemoveItem(Item item);
    }

    private bool CanUse(IPlayer player);
    private bool CanUseAdmin(IPlayer player);
    private string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Commands")]
    public string[] Commands;
    [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<SkinItem> Skins;
    [JsonIgnore]
    public Dictionary<string, List<SkinItem>> IndexedSkins;
    [JsonProperty(PropertyName = "Container Panel Name")]
    public string Panel;
    [JsonProperty(PropertyName = "Container Capacity")]
    public int Capacity;
    [JsonProperty(PropertyName = "UI")]
    public UIConfiguration UI;
    public class SkinItem
    {
        [JsonProperty(PropertyName = "Item Shortname")]
        public string Shortname;
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ulong> Skins;
        public static IEnumerable<SkinItem> Find(IPlayer player, string shortname);
        public bool CanUse(IPlayer player);
    }

    public class UIConfiguration
    {
        [JsonProperty(PropertyName = "Background Color")]
        public string BackgroundColor;
        [JsonProperty(PropertyName = "Background Anchors")]
        public Anchors BackgroundAnchors;
        [JsonProperty(PropertyName = "Background Offsets")]
        public Offsets BackgroundOffsets;
        [JsonProperty(PropertyName = "Left Button Text")]
        public string LeftText;
        [JsonProperty(PropertyName = "Left Button Color")]
        public string LeftColor;
        [JsonProperty(PropertyName = "Left Button Anchors")]
        public Anchors LeftAnchors;
        [JsonProperty(PropertyName = "Center Button Text")]
        public string CenterText;
        [JsonProperty(PropertyName = "Center Button Color")]
        public string CenterColor;
        [JsonProperty(PropertyName = "Center Button Anchors")]
        public Anchors CenterAnchors;
        [JsonProperty(PropertyName = "Right Button Text")]
        public string RightText;
        [JsonProperty(PropertyName = "Right Button Color")]
        public string RightColor;
        [JsonProperty(PropertyName = "Right Button Anchors")]
        public Anchors RightAnchors;
        [JsonIgnore]
        public string ParsedUI;
        [JsonIgnore]
        public int IndexPagePrevious;
        public int IndexPageCurrent;
        public int IndexPageNext;
        public class Anchors
        {
            [JsonProperty(PropertyName = "Anchor Min X")]
            public string AnchorMinX;
            [JsonProperty(PropertyName = "Anchor Min Y")]
            public string AnchorMinY;
            [JsonProperty(PropertyName = "Anchor Max X")]
            public string AnchorMaxX;
            [JsonProperty(PropertyName = "Anchor Max Y")]
            public string AnchorMaxY;
            [JsonIgnore]
            public string AnchorMin { get; set; }
            [JsonIgnore]
            public string AnchorMax { get; set; }
        }

        public class Offsets
        {
            [JsonProperty(PropertyName = "Offset Min X")]
            public string OffsetMinX;
            [JsonProperty(PropertyName = "Offset Min Y")]
            public string OffsetMinY;
            [JsonProperty(PropertyName = "Offset Max X")]
            public string OffsetMaxX;
            [JsonProperty(PropertyName = "Offset Max Y")]
            public string OffsetMaxY;
            [JsonIgnore]
            public string OffsetMin { get; set; }
            [JsonIgnore]
            public string OffsetMax { get; set; }
        }

    }

    public void IndexSkins();
}

public class SkinItem
{
    [JsonProperty(PropertyName = "Item Shortname")]
    public string Shortname;
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Skins", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ulong> Skins;
    public static IEnumerable<SkinItem> Find(IPlayer player, string shortname);
    public bool CanUse(IPlayer player);
}

public class UIConfiguration
{
    [JsonProperty(PropertyName = "Background Color")]
    public string BackgroundColor;
    [JsonProperty(PropertyName = "Background Anchors")]
    public Anchors BackgroundAnchors;
    [JsonProperty(PropertyName = "Background Offsets")]
    public Offsets BackgroundOffsets;
    [JsonProperty(PropertyName = "Left Button Text")]
    public string LeftText;
    [JsonProperty(PropertyName = "Left Button Color")]
    public string LeftColor;
    [JsonProperty(PropertyName = "Left Button Anchors")]
    public Anchors LeftAnchors;
    [JsonProperty(PropertyName = "Center Button Text")]
    public string CenterText;
    [JsonProperty(PropertyName = "Center Button Color")]
    public string CenterColor;
    [JsonProperty(PropertyName = "Center Button Anchors")]
    public Anchors CenterAnchors;
    [JsonProperty(PropertyName = "Right Button Text")]
    public string RightText;
    [JsonProperty(PropertyName = "Right Button Color")]
    public string RightColor;
    [JsonProperty(PropertyName = "Right Button Anchors")]
    public Anchors RightAnchors;
    [JsonIgnore]
    public string ParsedUI;
    [JsonIgnore]
    public int IndexPagePrevious;
    public int IndexPageCurrent;
    public int IndexPageNext;
    public class Anchors
    {
        [JsonProperty(PropertyName = "Anchor Min X")]
        public string AnchorMinX;
        [JsonProperty(PropertyName = "Anchor Min Y")]
        public string AnchorMinY;
        [JsonProperty(PropertyName = "Anchor Max X")]
        public string AnchorMaxX;
        [JsonProperty(PropertyName = "Anchor Max Y")]
        public string AnchorMaxY;
        [JsonIgnore]
        public string AnchorMin { get; set; }
        [JsonIgnore]
        public string AnchorMax { get; set; }
    }

    public class Offsets
    {
        [JsonProperty(PropertyName = "Offset Min X")]
        public string OffsetMinX;
        [JsonProperty(PropertyName = "Offset Min Y")]
        public string OffsetMinY;
        [JsonProperty(PropertyName = "Offset Max X")]
        public string OffsetMaxX;
        [JsonProperty(PropertyName = "Offset Max Y")]
        public string OffsetMaxY;
        [JsonIgnore]
        public string OffsetMin { get; set; }
        [JsonIgnore]
        public string OffsetMax { get; set; }
    }

}

public class Anchors
{
    [JsonProperty(PropertyName = "Anchor Min X")]
    public string AnchorMinX;
    [JsonProperty(PropertyName = "Anchor Min Y")]
    public string AnchorMinY;
    [JsonProperty(PropertyName = "Anchor Max X")]
    public string AnchorMaxX;
    [JsonProperty(PropertyName = "Anchor Max Y")]
    public string AnchorMaxY;
    [JsonIgnore]
    public string AnchorMin { get; set; }
    [JsonIgnore]
    public string AnchorMax { get; set; }
}

public class Offsets
{
    [JsonProperty(PropertyName = "Offset Min X")]
    public string OffsetMinX;
    [JsonProperty(PropertyName = "Offset Min Y")]
    public string OffsetMinY;
    [JsonProperty(PropertyName = "Offset Max X")]
    public string OffsetMaxX;
    [JsonProperty(PropertyName = "Offset Max Y")]
    public string OffsetMaxY;
    [JsonIgnore]
    public string OffsetMin { get; set; }
    [JsonIgnore]
    public string OffsetMax { get; set; }
}

private class ContainerController
{
    public BasePlayer Owner;
    public ItemContainer Container;
    public bool IsOpened;
    public Dictionary<string, List<ulong>> TotalSkinsCache;
    private List<Item> _storedContent;
    private Magazine _storedMagazine;
    public ContainerController(BasePlayer player);
    private void DestroyUI();
    private void DrawUI(int page);
    public void Close();
    public void Show();
    public bool CanShow();
    private static bool CanShow(BasePlayer player);
    private void AddItemContainer(Item item);
    public void GiveItemBack(Item itemOverride);
    public void OnItemTaken(Item item);
    public void SetupContent(Item destination);
    public void StoreContent(Item source);
    public void Clear();
    public void Destroy();
    public void UpdateContent(int page);
    private bool IsValid();
    private bool CanUse();
    private Item GetDuplicateItem(Item item, ulong skin);
    private void MoveItem(Item item, ItemContainer container, int slot);
    private void RemoveItem(Item item);
}


```

---

## SkinShop by Rustoholics - A GUI skin shop to allow players to buy custom skins

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Security.Cryptography;
using System.Text;
using System.Text.RegularExpressions;
using JetBrains.Annotations;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Random = System.Random;

Oxide.Plugins
[Info("Skin Shop", "Rustoholics", "0.5.2")]
[Description("A GUI skin shop to allow players to buy custom skins")]
public class SkinShop : CovalencePlugin
{
    [PluginReference]
    private MemoryCache MemoryCache;
    [PluginReference]
    private Plugin ImageLibrary;
    private Plugin Economics;
    private Plugin ServerRewards;
    private string _downloadJsonUrl;
    private Dictionary<string, WorkshopResult> _cache;
    private Dictionary<string, GuiHelper> _skinGui;
    private Dictionary<string, List<WorkShopItem>> _ownedItems;
    private Dictionary<string, WorkShopItem> _skinDatabase;
    private List<string> _needsWriting;
    private const string welcomePackPermission;
    private const string adminPermission;
    private const string blacklistPermission;
    private const string useSkinshopPermission;
    private const string vipPermissions;
    private bool _categoryIconsLoaded;
    private static SortedDictionary<string, string> _categories;
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty(PropertyName = "LogLevel (debug | info | none)")]
        public string LogLevel;
        public bool SkinsPacksEnabled;
        public Dictionary<int, SkinPack> SkinPacks;
        [JsonProperty(PropertyName =
                "Give Welcome Skin Pack index to new players ([0,0] would give 2 of the first packs, empty [] for disable)")]
        public int[] GiveWelcomePacks;
        public bool EnabledCategoryIcons;
        public ulong[] BlackListedSkins;
        public string[] HideCategories;
        public double DefaultSkinPrice;
        public double[] VipDiscounts;
        public double DefaultInstantPricePrice;
        [JsonProperty(PropertyName = "Show the category selection page as the shop landing page")]
        public bool ShowCategorySelectLanding;
        [JsonProperty(PropertyName = "Default category to show (if category selection landing page is false)")]
        public string DefaultCategory;
        [JsonProperty(PropertyName = "Currency Plugin (can be 'Economics' or 'ServerRewards')")]
        public string CurrencyPlugin;
        public ulong[] HumanNpcIds;
        [JsonProperty(PropertyName = "Convert all Wrapped Gifts to Skin Pack")]
        public bool ConvertGiftsToPacks;
        public bool AutomaticallyRemoveMissingImages;
        public string SkinPackImageUrl;
        public string SearchIconUrl;
        public string BlacklistIconUrl;
        public string DiscordApiKey;
        public string DiscordChannelId;
        public Dictionary<string, string> Color;
        public bool LoadSkinsFromDatabase;
        public bool EnableCaching;
    }

    protected override void LoadConfig();
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    public class GuiOptions
    {
        public bool ShowCategories;
        public bool ShowSkinPacks;
        public string ShowSkinProfile;
    }

    public class SkinPack
    {
        public ulong SkinId;
        public string ImageUrl;
        public int NumberOfSkins;
        public string PackName;
        public double Price;
        public ulong[] PossibleSkinIds;
        public string[] PossibleSkinCategories;
    }

    public class WorkshopResult
    {
        private List<WorkShopItem> Items;
        public int Page;
        public int TotalResults;
        public int TotalPages { get; set; }
        public int PerPage;
        public List<string> Categories;
        public string Search;
        public string OwnerId;
        public string Url { get; set; }
        public List<WorkShopItem> GetItems();
        public void RemoveOwner();
        public void AddItem(WorkShopItem item);
        public string GetUrl();
        public void LoadOwned(Dictionary<string, List<WorkShopItem>> allOwned);
        public void LoadOwned(Dictionary<string, List<WorkShopItem>> allOwned, string itemId);
        public string Escape();
    }

    public class WorkShopItem
    {
        public string Title;
        public string Description;
        public string Image;
        public string Id;
        public double Price;
        public string Category;
        public bool Wrapped;
        public bool Equipped;
        public string Shortname { get; set; }
        public double GetPrice(double defaultPrice, double discount);
        public double GetInstantSellPrice(double defaultPrice);
        public string Escape();
    }

    private bool? CanStackItem(Item original, Item target);
     ItemContainer.CanAcceptResult? CanAcceptItem(ItemContainer container, Item item, int targetPos);
    private void OnServerInitialized();
    private void OnItemCraftFinished(ItemCraftTask task, Item item);
    private void OnItemPickup(Item item, BasePlayer player);
     void OnPlayerConnected(BasePlayer player);
     void OnItemAddedToContainer(ItemContainer container, Item item);
    private void OnServerSave();
     void Unload();
     object OnItemAction(Item item, string action, BasePlayer player);
     void CloseSkinGui(BasePlayer player);
     void PrepareShowSkinGui(BasePlayer player, WorkshopResult workshop, GuiOptions options);
     void ShowSkinGui(BasePlayer player, WorkshopResult workshop, GuiOptions options);
    private void AddButtons(GuiHelper guiHelper, string parent, BasePlayer player, WorkshopResult workshop, GuiOptions options);
    private void GuiSearchBar(GuiHelper guiHelper, string parent, WorkshopResult workshop);
    private void GuiPagination(GuiHelper guiHelper, string parent, WorkshopResult workshop);
    private CuiRawImageComponent GetImage(string url, ulong imageId);
    private void SkinPacksGrid(GuiHelper guiHelper, string parent, WorkshopResult workshop, BasePlayer player);
    private void SkinProfileGui(GuiHelper guiHelper, string parent, WorkshopResult workshop, BasePlayer player);
    private List<GuiGridItem> GetGridItems(int totalItems, int columns, int minimumRows, double padding);
    private void AddGrid(GuiHelper guiHelper, string parent, BasePlayer player, WorkshopResult workshop);
    private void CategoryButtons(BasePlayer player, GuiHelper guiHelper, string parent, WorkshopResult workshop);
    private void GuiAddSubtitle(GuiHelper guiHelper, string text, string parent);
    private void ApplySkin(BasePlayer player, WorkShopItem item);
    private void SetSkin(BasePlayer player, Item item);
    private void SetSkinId(Item item, ulong skinID);
    [CanBeNull]
    private string BuyItem(BasePlayer player, WorkShopItem item);
    private string BuyPack(BasePlayer player, SkinPack pack);
    private void GiveItem(BasePlayer player, WorkShopItem item, bool equip);
    private void EquipItem(BasePlayer player, WorkShopItem item);
    private void UnEquipItem(BasePlayer player, WorkShopItem item);
    private bool HasEconomicsPlugin();
    private bool AddFunds(string playerId, double amount);
    private double GetBalance(string playerId);
    private bool TakeFunds(string playerId, double amount);
    [CanBeNull]
    private WorkShopItem PlayerOwnsItem(BasePlayer player, WorkShopItem item);
    private bool PlayerHasItem(BasePlayer player, string item);
    [CanBeNull]
    private WorkShopItem EquippedItem(BasePlayer player, string categoryShort);
    private bool IsSkinPack(Item item);
    private SkinPack GetSkinPack(Item item);
    private void SetupSkinPack(Item item);
    private List<WorkShopItem> OpenPack(BasePlayer player, SkinPack pack);
    private void GiveSkinPack(BasePlayer player, SkinPack pack);
    private string SellSkin(BasePlayer player, string itemId);
     void ShowPacksGui(BasePlayer player, List<WorkShopItem> items);
    private void LogMsg(string msg, string level);
    private void DownloadDatabase(IPlayer iplayer);
    private void DownloadWorkshop(WorkshopResult workshop, Action callback, string cacheKey);
    private void SaveData();
    private void SaveSkinDatabase();
    private void LoadData();
    private void LoadDatabase();
    private Action ImageCallback(string imageName, ulong imageId);
    private Action CategoryIconsDone(int count);
    private void LoadIconImages();
    private void NeedsWriting(string playerId);
    [Command("skinshop"), Permission(useSkinshopPermission)]
    private void ShowSkinShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.close"), Permission(useSkinshopPermission)]
    private void CloseSkinShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.page"), Permission(useSkinshopPermission)]
    private void PageChangeSkinShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.search"), Permission(useSkinshopPermission)]
    private void SearchSkinShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.category"), Permission(useSkinshopPermission)]
    private void CategorySkinShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.selectcategory"), Permission(useSkinshopPermission)]
    private void SelectCategorySkinShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.buy"), Permission(useSkinshopPermission)]
    private void BuySkinShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.equip"), Permission(useSkinshopPermission)]
    private void EquipItemCommand(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.unequip"), Permission(useSkinshopPermission)]
    private void UnEquipItemCommand(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.myskins"), Permission(useSkinshopPermission)]
    private void MySkinsPage(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.buypacks"), Permission(useSkinshopPermission)]
    private void BuyPacksPage(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.packsopen"), Permission(useSkinshopPermission)]
    private void OpenSkinPacksShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.packsclose"), Permission(useSkinshopPermission)]
    private void CloseSkinPacksShop(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.skinprofile"), Permission(useSkinshopPermission)]
    private void ViewSkinProfileCommand(IPlayer iplayer, string command, string[] args);
    private Timer ti;
    private void KillTimer();
    [Command("skinshop.download"), Permission(adminPermission)]
    private void DownloadSkinData(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.builddatabase"), Permission(adminPermission)]
    private void BuildSkinData(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.givepack"), Permission(adminPermission)]
    private void GivePackCommand(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.buypack")]
    private void BuyPackCommand(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.sellskin")]
    private void SellSkinCommand(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.apply"), Permission(useSkinshopPermission)]
    private void ApplySkinCommand(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.getskin"), Permission(blacklistPermission)]
    private void GetSkinCommand(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.blacklist.id"), Permission(blacklistPermission)]
    private void BlacklistListId(IPlayer iplayer, string command, string[] args);
    [Command("skinshop.blacklist.this"), Permission(blacklistPermission)]
    private void BlacklistListThis(IPlayer iplayer, string command, string[] args);
    private class GuiGridItem
    {
        public int Id;
        public double x1;
        public double x2;
        public double y1;
        public double y2;
        public string Pane(CuiElementContainer container, string parent, string color);
        public string AnchorMin();
        public string AnchorMax();
    }

    private class GuiHelper
    {
        public CuiElementContainer container;
        public BasePlayer player;
        private List<string> _elements;
        public GuiHelper(BasePlayer baseplayer);
        public string Button(string parent, string text, string buttonColor, string command, double x1, double x2, double y1, double y2, int fontSize, string textColor, TextAnchor align);
        public string Panel(string parent, double x1, double x2, double y1, double y2, string color, bool cursor);
        public string Label(string parent, string text, double x1, double x2, double y1, double y2, int fontSize, string textColor, TextAnchor align);
        public string Image(string parent, double x1, double x2, double y1, double y2, CuiRawImageComponent image);
        public void Open();
        public int Close();
    }

    private void BlacklistSkin(ulong skinId);
    private Item GetLookingAtItem(BasePlayer player);
    private static BaseEntity FindObject(Ray ray, float distance);
    private double GetDiscount(BasePlayer player);
    private DateTime GetServerTime();
    private string Color(string colorName, string opacity);
    private int PlayerInventorySpaceAvailable(BasePlayer player);
    private WorkshopResult UnEscape(string json);
    private WorkShopItem UnEscapeItem(string json);
    static string CleanInput(string strIn);
    static string Hash(string input);
    private class DiscordMessage
    {
        public string content;
    }

    private void PostToDiscord(string message);
    private void OnUseNPC(BasePlayer npc, BasePlayer player);
}

private class Configuration
{
    [JsonProperty(PropertyName = "LogLevel (debug | info | none)")]
    public string LogLevel;
    public bool SkinsPacksEnabled;
    public Dictionary<int, SkinPack> SkinPacks;
    [JsonProperty(PropertyName =
                "Give Welcome Skin Pack index to new players ([0,0] would give 2 of the first packs, empty [] for disable)")]
    public int[] GiveWelcomePacks;
    public bool EnabledCategoryIcons;
    public ulong[] BlackListedSkins;
    public string[] HideCategories;
    public double DefaultSkinPrice;
    public double[] VipDiscounts;
    public double DefaultInstantPricePrice;
    [JsonProperty(PropertyName = "Show the category selection page as the shop landing page")]
    public bool ShowCategorySelectLanding;
    [JsonProperty(PropertyName = "Default category to show (if category selection landing page is false)")]
    public string DefaultCategory;
    [JsonProperty(PropertyName = "Currency Plugin (can be 'Economics' or 'ServerRewards')")]
    public string CurrencyPlugin;
    public ulong[] HumanNpcIds;
    [JsonProperty(PropertyName = "Convert all Wrapped Gifts to Skin Pack")]
    public bool ConvertGiftsToPacks;
    public bool AutomaticallyRemoveMissingImages;
    public string SkinPackImageUrl;
    public string SearchIconUrl;
    public string BlacklistIconUrl;
    public string DiscordApiKey;
    public string DiscordChannelId;
    public Dictionary<string, string> Color;
    public bool LoadSkinsFromDatabase;
    public bool EnableCaching;
}

public class GuiOptions
{
    public bool ShowCategories;
    public bool ShowSkinPacks;
    public string ShowSkinProfile;
}

public class SkinPack
{
    public ulong SkinId;
    public string ImageUrl;
    public int NumberOfSkins;
    public string PackName;
    public double Price;
    public ulong[] PossibleSkinIds;
    public string[] PossibleSkinCategories;
}

public class WorkshopResult
{
    private List<WorkShopItem> Items;
    public int Page;
    public int TotalResults;
    public int TotalPages { get; set; }
    public int PerPage;
    public List<string> Categories;
    public string Search;
    public string OwnerId;
    public string Url { get; set; }
    public List<WorkShopItem> GetItems();
    public void RemoveOwner();
    public void AddItem(WorkShopItem item);
    public string GetUrl();
    public void LoadOwned(Dictionary<string, List<WorkShopItem>> allOwned);
    public void LoadOwned(Dictionary<string, List<WorkShopItem>> allOwned, string itemId);
    public string Escape();
}

public class WorkShopItem
{
    public string Title;
    public string Description;
    public string Image;
    public string Id;
    public double Price;
    public string Category;
    public bool Wrapped;
    public bool Equipped;
    public string Shortname { get; set; }
    public double GetPrice(double defaultPrice, double discount);
    public double GetInstantSellPrice(double defaultPrice);
    public string Escape();
}

private class GuiGridItem
{
    public int Id;
    public double x1;
    public double x2;
    public double y1;
    public double y2;
    public string Pane(CuiElementContainer container, string parent, string color);
    public string AnchorMin();
    public string AnchorMax();
}

private class GuiHelper
{
    public CuiElementContainer container;
    public BasePlayer player;
    private List<string> _elements;
    public GuiHelper(BasePlayer baseplayer);
    public string Button(string parent, string text, string buttonColor, string command, double x1, double x2, double y1, double y2, int fontSize, string textColor, TextAnchor align);
    public string Panel(string parent, double x1, double x2, double y1, double y2, string color, bool cursor);
    public string Label(string parent, string text, double x1, double x2, double y1, double y2, int fontSize, string textColor, TextAnchor align);
    public string Image(string parent, double x1, double x2, double y1, double y2, CuiRawImageComponent image);
    public void Open();
    public int Close();
}

private class DiscordMessage
{
    public string content;
}


```

---

## SkipDeathScreen by Notchu - Skips death screen when there is no available sleeping bags

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using JetBrains.Annotations;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using ProtoBuf;
using UnityEngine;

[Info("Skip Death Screen", "Notchu", "1.0.5")]
[Description("Skip death screen if there is no available Sleeping bags")]
public class SkipDeathScreen : CovalencePlugin
{
    private class Configuration
    {
        [JsonProperty("Ignore sleeping bags cooldown")]
        public bool IgnoreTimers;
        [JsonProperty("Allow users to change IgnoreTimers value for themself")]
        public bool AllowChangeSettings;
    }

    private Configuration _config;
    protected override void LoadConfig();
    private abstract class SplitDatafile
    {
        public static Dictionary<string, T> LoadedData;
        protected static string[] GetFiles(string baseFolder);
        protected static T Save(string baseFolder, string filename);
        protected static bool Delete(string baseFolder, string filename);
        protected static T Get(string baseFolder, string filename);
        protected static T GetOrLoad(string baseFolder, string filename);
        protected static T Create(string baseFolder, string filename);
    }

    private class SkipDeathScreenData : SplitDatafile<SkipDeathScreenData>
    {
        public bool IgnoreTimers { get; set; }
        public static readonly string BaseFolder;
        public static string[] GetFiles();
        public static bool Delete(string filename);
        public static SkipDeathScreenData Save(string filename);
        public static SkipDeathScreenData Get(string filename);
        public static SkipDeathScreenData GetOrLoad(string filename);
        public static SkipDeathScreenData Create(string filename);
    }

    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void Unload();
    private void OnEntityDeath(BasePlayer player, HitInfo info);
    [Command("ToggleTimers")]
    private async void ChangeTimerSettings(IPlayer player, string command, string[] args);
}

private class Configuration
{
    [JsonProperty("Ignore sleeping bags cooldown")]
    public bool IgnoreTimers;
    [JsonProperty("Allow users to change IgnoreTimers value for themself")]
    public bool AllowChangeSettings;
}

private abstract class SplitDatafile
{
    public static Dictionary<string, T> LoadedData;
    protected static string[] GetFiles(string baseFolder);
    protected static T Save(string baseFolder, string filename);
    protected static bool Delete(string baseFolder, string filename);
    protected static T Get(string baseFolder, string filename);
    protected static T GetOrLoad(string baseFolder, string filename);
    protected static T Create(string baseFolder, string filename);
}

private class SkipDeathScreenData : SplitDatafile<SkipDeathScreenData>
{
    public bool IgnoreTimers { get; set; }
    public static readonly string BaseFolder;
    public static string[] GetFiles();
    public static bool Delete(string filename);
    public static SkipDeathScreenData Save(string filename);
    public static SkipDeathScreenData Get(string filename);
    public static SkipDeathScreenData GetOrLoad(string filename);
    public static SkipDeathScreenData Create(string filename);
}


```

---

## SkipNightUI by k1lly0u - A UI skip night plugin with completely customizable interfaces

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("SkipNightUI", "k1lly0u", "0.1.2", ResourceId = 2506)]
 class SkipNightUI : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
    private List<ulong> votesReceived;
    private bool voteOpen;
    private bool isWaiting;
    private bool isILReady;
    private int timeRemaining;
    private int requiredVotes;
    private Timer voteTimer;
    private Timer timeMonitor;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panelName, string color, string aMin, string aMax, bool useCursor);
        static public void CreatePanel(CuiElementContainer container, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer container, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
        public static string Color(string hexColor, float alpha);
    }

    private const string Main;
    private void CreateTimeUI();
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
    private void AddImage(string fileName);
    private string GetImage(string fileName, ulong skin);
    private void LoadImages();
    private void BeginUICreation();
     TextAnchor ParseAnchor(string anchor);
     ProgressType ParseType(string type);
    [ChatCommand("voteday")]
    private void cmdVoteDay(BasePlayer player, string command, string[] args);
     class UICreator
    {
        [JsonProperty(PropertyName = "Main Container Size")]
        public UISize Size { get; set; }
        [JsonProperty(PropertyName = "Panel Elements")]
        public List<UIPanel> PanelElements { get; set; }
        [JsonProperty(PropertyName = "Text Elements")]
        public List<UIText> TextElements { get; set; }
        [JsonProperty(PropertyName = "Image Elements")]
        public List<UIImage> ImageElements { get; set; }
        [JsonProperty(PropertyName = "Vote Progress Elements")]
        public List<UIProgress> VoteProgress { get; set; }
        [JsonProperty(PropertyName = "Time Progress Elements")]
        public List<UIProgress> TimeProgress { get; set; }
    }

     class UISize
    {
        [JsonProperty(PropertyName = "Horizontal Start")]
        public float XMin { get; set; }
        [JsonProperty(PropertyName = "Horizontal End")]
        public float XMax { get; set; }
        [JsonProperty(PropertyName = "Vertical Start")]
        public float YMin { get; set; }
        [JsonProperty(PropertyName = "Vertical End")]
        public float YMax { get; set; }
    }

     class UIPanel
    {
        public UISize Size { get; set; }
        [JsonProperty(PropertyName = "Background Color (Hex)")]
        public string Color { get; set; }
        [JsonProperty(PropertyName = "Background Alpha")]
        public float Alpha { get; set; }
    }

     class UIText
    {
        public UISize Size { get; set; }
        public TextComponent Text { get; set; }
    }

     class UIImage
    {
        public UISize Size { get; set; }
        [JsonProperty(PropertyName = "URL or Image filename")]
        public string URL { get; set; }
    }

     class UIProgress
    {
        public UISize Size { get; set; }
        [JsonProperty(PropertyName = "Progress Type")]
        public string Type { get; set; }
        [JsonProperty(PropertyName = "URL or Image filename (Graphic)")]
        public string URL { get; set; }
        [JsonProperty(PropertyName = "Bar Color (Solid)")]
        public string Color { get; set; }
        [JsonProperty(PropertyName = "Bar Alpha (Solid)")]
        public float Alpha { get; set; }
        [JsonProperty(PropertyName = "Text Options (Text)")]
        public TextComponent Text { get; set; }
    }

     class TextComponent
    {
        [JsonProperty(PropertyName = "Alignment")]
        public string Alignment { get; set; }
        [JsonProperty(PropertyName = "Color (Hex)")]
        public string Color { get; set; }
        public string Text { get; set; }
        [JsonProperty(PropertyName = "Size")]
        public int Size { get; set; }
    }

    private ConfigData configData;
     class Colors
    {
        public string Primary { get; set; }
        public string Secondary { get; set; }
    }

     class Options
    {
        [JsonProperty(PropertyName = "Required vote percentage")]
        public float Percentage { get; set; }
        [JsonProperty(PropertyName = "Time the vote will open")]
        public float Open { get; set; }
        [JsonProperty(PropertyName = "Time to set on successful vote")]
        public float Set { get; set; }
        [JsonProperty(PropertyName = "Duration the vote will be open")]
        public int Duration { get; set; }
    }

     class ConfigData
    {
        [JsonProperty(PropertyName = "UI Configuration Name")]
        public string UIConfig { get; set; }
        [JsonProperty(PropertyName = "UI Configurations")]
        public Dictionary<string, UICreator> PanelTypes { get; set; }
        [JsonProperty(PropertyName = "Message Colors")]
        public Colors Colors { get; set; }
        [JsonProperty(PropertyName = "Options")]
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
    static public void LoadImage(CuiElementContainer container, string panel, string png, string aMin, string aMax);
    public static string Color(string hexColor, float alpha);
}

 class UICreator
{
    [JsonProperty(PropertyName = "Main Container Size")]
    public UISize Size { get; set; }
    [JsonProperty(PropertyName = "Panel Elements")]
    public List<UIPanel> PanelElements { get; set; }
    [JsonProperty(PropertyName = "Text Elements")]
    public List<UIText> TextElements { get; set; }
    [JsonProperty(PropertyName = "Image Elements")]
    public List<UIImage> ImageElements { get; set; }
    [JsonProperty(PropertyName = "Vote Progress Elements")]
    public List<UIProgress> VoteProgress { get; set; }
    [JsonProperty(PropertyName = "Time Progress Elements")]
    public List<UIProgress> TimeProgress { get; set; }
}

 class UISize
{
    [JsonProperty(PropertyName = "Horizontal Start")]
    public float XMin { get; set; }
    [JsonProperty(PropertyName = "Horizontal End")]
    public float XMax { get; set; }
    [JsonProperty(PropertyName = "Vertical Start")]
    public float YMin { get; set; }
    [JsonProperty(PropertyName = "Vertical End")]
    public float YMax { get; set; }
}

 class UIPanel
{
    public UISize Size { get; set; }
    [JsonProperty(PropertyName = "Background Color (Hex)")]
    public string Color { get; set; }
    [JsonProperty(PropertyName = "Background Alpha")]
    public float Alpha { get; set; }
}

 class UIText
{
    public UISize Size { get; set; }
    public TextComponent Text { get; set; }
}

 class UIImage
{
    public UISize Size { get; set; }
    [JsonProperty(PropertyName = "URL or Image filename")]
    public string URL { get; set; }
}

 class UIProgress
{
    public UISize Size { get; set; }
    [JsonProperty(PropertyName = "Progress Type")]
    public string Type { get; set; }
    [JsonProperty(PropertyName = "URL or Image filename (Graphic)")]
    public string URL { get; set; }
    [JsonProperty(PropertyName = "Bar Color (Solid)")]
    public string Color { get; set; }
    [JsonProperty(PropertyName = "Bar Alpha (Solid)")]
    public float Alpha { get; set; }
    [JsonProperty(PropertyName = "Text Options (Text)")]
    public TextComponent Text { get; set; }
}

 class TextComponent
{
    [JsonProperty(PropertyName = "Alignment")]
    public string Alignment { get; set; }
    [JsonProperty(PropertyName = "Color (Hex)")]
    public string Color { get; set; }
    public string Text { get; set; }
    [JsonProperty(PropertyName = "Size")]
    public int Size { get; set; }
}

 class Colors
{
    public string Primary { get; set; }
    public string Secondary { get; set; }
}

 class Options
{
    [JsonProperty(PropertyName = "Required vote percentage")]
    public float Percentage { get; set; }
    [JsonProperty(PropertyName = "Time the vote will open")]
    public float Open { get; set; }
    [JsonProperty(PropertyName = "Time to set on successful vote")]
    public float Set { get; set; }
    [JsonProperty(PropertyName = "Duration the vote will be open")]
    public int Duration { get; set; }
}

 class ConfigData
{
    [JsonProperty(PropertyName = "UI Configuration Name")]
    public string UIConfig { get; set; }
    [JsonProperty(PropertyName = "UI Configurations")]
    public Dictionary<string, UICreator> PanelTypes { get; set; }
    [JsonProperty(PropertyName = "Message Colors")]
    public Colors Colors { get; set; }
    [JsonProperty(PropertyName = "Options")]
    public Options Options { get; set; }
}


```

---

## SkipNightVote by k1lly0u - Allows players to vote to skip night time

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("SkipNightVote", "k1lly0u", "0.1.4", ResourceId = 2058)]
 class SkipNightVote : CovalencePlugin
{
    private List<string> ReceivedVotes;
    private bool VoteOpen;
    private bool DisplayCountEveryVote;
    private int TimeRemaining;
    private int RequiredVotes;
    private string TimeRemMSG;
    private Timer VotingTimer;
    private Timer TimeCheck;
    private Timer CountTimer;
     void Loaded();
     void OnServerInitialized();
     void Unload();
    private void OpenVote();
    private void VoteTimer();
    private void ShowCountTimer();
    private void CheckTime();
    private void TallyVotes();
    private void VoteEnd(bool success);
    private bool AlreadyVoted(string player);
    [Command("voteday")]
    private void cmdVoteDay(IPlayer player, string command, string[] args);
    [Command("nightvote"), Permission("skipnightvote.admin")]
    private void cmdAdminVote(IPlayer player, string command, string[] args);
    private ConfigData configData;
     class Messaging
    {
        public int DisplayCountEvery { get; set; }
        public string MainColor { get; set; }
        public string MSGColor { get; set; }
    }

     class Timers
    {
        public int VoteOpenTimer { get; set; }
        public int TimeBetweenVotes { get; set; }
    }

     class Options
    {
        public float RequiredVotePercentage { get; set; }
        public string TimeToOpen { get; set; }
        public string TimeToSet { get; set; }
    }

     class ConfigData
    {
        public Messaging Messaging { get; set; }
        public Timers VoteTimers { get; set; }
        public Options Options { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
    private string GetMSG(string key, string userid);
     Dictionary<string, string> Messages;
}

 class Messaging
{
    public int DisplayCountEvery { get; set; }
    public string MainColor { get; set; }
    public string MSGColor { get; set; }
}

 class Timers
{
    public int VoteOpenTimer { get; set; }
    public int TimeBetweenVotes { get; set; }
}

 class Options
{
    public float RequiredVotePercentage { get; set; }
    public string TimeToOpen { get; set; }
    public string TimeToSet { get; set; }
}

 class ConfigData
{
    public Messaging Messaging { get; set; }
    public Timers VoteTimers { get; set; }
    public Options Options { get; set; }
}


```

---

## SkullCrusher by EsCL337 - Adds some extra features to the crushing of human skulls

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Skull Crusher", "Krungh Crow", "1.1.1")]
[Description("Adds some extra features to the crushing of human skulls")]
 class SkullCrusher : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin Economics;
    private Plugin Friends;
    private Plugin ServerRewards;
    private Dictionary<string, int> cacheDic;
    private bool Changed;
    private bool giveItemsOnCrush;
    private double moneyPerSkullCrush;
    private bool normalCrusherMessage;
    private bool nullCrusherMessage;
    private bool ownCrusherMessage;
    private int RPPerSkullCrush;
    private bool sendNotificaitionMessage;
    private bool friendsSupport;
    private bool clansSupport;
    private bool teamsSupport;
    private bool useEconomy;
    private bool useServerRewards;
    private bool useItem;
    private int ItemAmount;
    private string Itemshortname;
    private bool useRandomMessages;
    private List<object> randomMessages;
    private DynamicConfigFile SkullCrusherData;
    private StoredData storedData;
    private class StoredData
    {
        public Dictionary<string, int> PlayerInformation;
    }

    private void SaveData();
    private void LoadData();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    protected override void LoadDefaultMessages();
    private object OnItemAction(Item item, string action);
    [ChatCommand("skulls")]
    private void skullsCMD(BasePlayer player, string command, string[] args);
    private bool IsTeamed(BasePlayer ownerPlayer, BasePlayer skullOwner);
    private object DecideReturn(Item item);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
}

private class StoredData
{
    public Dictionary<string, int> PlayerInformation;
}


```

---

## SkyTurrets by  - Allows auto turrets and SAM sites to target patrol helicopters and chinooks

```csharp
using Facepunch;
using Rust;
using System;
using System.Globalization;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using UnityEngine;

Oxide.Plugins
[Info("Sky Turrets", "Xavier", "3.0.6")]
[Description("Allows auto turrets and SAM sites to target patrol helicopters and chinooks")]
public class SkyTurrets : RustPlugin
{
    static SkyTurrets plugin;
    const string skyturretperms;
    private DynamicConfigFile data;
    private StoredData storedData;
    private List<ulong> turretIDs;
    private List<ulong> PoweredTurretIDs;
    private List<ulong> samIDs;
    private class StoredData
    {
        public List<ulong> turretIDs;
        public List<ulong> PoweredTurretIDs;
        public List<ulong> samIDs;
    }

    private void SaveData();
    private bool ConfigChanged;
     bool PowerlessTurrets;
     float AirTargetRadius;
     bool NeedsPerms;
     int SamSiteAttackDistance;
     float CheckForTargetEveryXSeconds;
     BUTTON PowerButton;
     string Button;
     void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    protected override void LoadDefaultConfig();
     void Init();
    private void OnServerSave();
     void Unload();
     void OnServerInitialized();
     void OnPlayerInput(BasePlayer player, InputState input);
    public void TurretInput(InputState input, BasePlayer player);
    protected override void LoadDefaultMessages();
     void OnEntitySpawned(AutoTurret turret);
     void OnEntityKill(AutoTurret turret);
     void OnEntitySpawned(SamSite entity);
     void OnEntityKill(SamSite entity);
    private static BasePlayer FindOwner(string nameOrId);
    private bool IsAllowed(AutoTurret turret);
    private bool SamIsAllowed(SamSite sam);
    private class AntiHeli : MonoBehaviour
    {
        private AutoTurret turret;
        private BaseEntity entity;
        private BaseCombatEntity target;
        private void Awake();
        private void FixedUpdate();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerStay(Collider col);
        private void OnTriggerExit(Collider col);
        public void UnloadDestroy();
        public void Destroy();
    }

    private class HeliTargeting : MonoBehaviour
    {
        private SamSite samsite;
        private BaseEntity entity;
        private void Awake();
        internal void FindTargets();
        public void UnloadDestroy();
        public void Destroy();
    }

}

private class StoredData
{
    public List<ulong> turretIDs;
    public List<ulong> PoweredTurretIDs;
    public List<ulong> samIDs;
}

private class AntiHeli : MonoBehaviour
{
    private AutoTurret turret;
    private BaseEntity entity;
    private BaseCombatEntity target;
    private void Awake();
    private void FixedUpdate();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerStay(Collider col);
    private void OnTriggerExit(Collider col);
    public void UnloadDestroy();
    public void Destroy();
}

private class HeliTargeting : MonoBehaviour
{
    private SamSite samsite;
    private BaseEntity entity;
    private void Awake();
    internal void FindTargets();
    public void UnloadDestroy();
    public void Destroy();
}


```

---

## Slack by MrBlue - Plugin API for sending messages and notifications to Slack

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Slack", "Wulf/lukespragg", "0.1.7", ResourceId = 1952)]
[Description("Plugin API for sending messages and notifications to Slack")]
 class Slack : CovalencePlugin
{
    static readonly DateTime Epoch;
    static readonly Regex Regex;
     bool linkNames;
     bool serverInfo;
     string botName;
     string hookUrl;
     string iconUrl;
    protected override void LoadDefaultConfig();
     void Init();
     void Message(string message, string channel, Action<bool> callback);
     void FancyMessage(string message, IPlayer player, string channel, Action<bool> callback);
     void SimpleMessage(string message, IPlayer player, string channel, Action<bool> callback);
     void TicketMessage(string message, IPlayer player, string channel, Action<bool> callback);
     void PostToSlack(SlackMessage payload, Action<bool> callback);
     class SlackMessage
    {
        public string channel { get; set; }
        public string icon_url { get; set; }
        public string link_names { get; set; }
        public string mrkdwn { get; set; }
        public string text { get; set; }
        public string username { get; set; }
        public List<SlackAttachment> attachments;
    }

     class SlackAttachment
    {
        public string author_icon { get; set; }
        public string author_link { get; set; }
        public string author_name { get; set; }
        public string color { get; set; }
        public string fallback { get; set; }
        public string footer { get; set; }
        public string footer_icon { get; set; }
        public string image_url { get; set; }
        public string pretext { get; set; }
        public string text { get; set; }
        public string thumb_url { get; set; }
        public string title { get; set; }
        public string title_link { get; set; }
        public string ts { get; set; }
        public List<SlackAttachmentField> fields;
    }

     class SlackAttachmentField
    {
        public string title { get; set; }
        public string value { get; set; }
        public string @short { get; set; }
    }

     bool ErrorChecks(string message, bool callback, IPlayer player);
     T GetConfig(string name, T value);
}

 class SlackMessage
{
    public string channel { get; set; }
    public string icon_url { get; set; }
    public string link_names { get; set; }
    public string mrkdwn { get; set; }
    public string text { get; set; }
    public string username { get; set; }
    public List<SlackAttachment> attachments;
}

 class SlackAttachment
{
    public string author_icon { get; set; }
    public string author_link { get; set; }
    public string author_name { get; set; }
    public string color { get; set; }
    public string fallback { get; set; }
    public string footer { get; set; }
    public string footer_icon { get; set; }
    public string image_url { get; set; }
    public string pretext { get; set; }
    public string text { get; set; }
    public string thumb_url { get; set; }
    public string title { get; set; }
    public string title_link { get; set; }
    public string ts { get; set; }
    public List<SlackAttachmentField> fields;
}

 class SlackAttachmentField
{
    public string title { get; set; }
    public string value { get; set; }
    public string @short { get; set; }
}


```

---

## SlackChat by MrBlue - Sends all chat messages or keyword-based chat to Slack channel

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SlackChat", "Wulf/lukespragg", "0.1.2", ResourceId = 1953)]
[Description("Sends all chat messages or keyword-based chat to Slack channel")]
 class SlackChat : CovalencePlugin
{
    [PluginReference]
     Plugin Slack;
     void Init();
     string channel;
     List<object> keywords;
     bool keywordsOnly;
     string style;
    protected override void LoadDefaultConfig();
     void OnUserChat(IPlayer player, string message);
     T GetConfig(string name, T defaultValue);
}


```

---

## SlackMutes by  - Sends information on server mutes to a Slack channel

```csharp
using System.Text.RegularExpressions;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using System;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SlackMutes", "Kinesis", "1.0.2", ResourceId = 2364)]
[Description("Sends information on Server Mutes to a Slack Channel")]
 class SlackMutes : CovalencePlugin
{
    [PluginReference]
     Plugin Slack;
     string channel;
     bool notifypermanent;
     bool notifytimed;
     bool notifyunmute;
     bool notifyexpiredunmute;
     string style;
     void Init();
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     void OnBetterChatMuted(IPlayer target, IPlayer initiator);
     void OnBetterChatTimeMuted(IPlayer target, IPlayer initiator, DateTime expireDate);
     void OnBetterChatUnmuted(IPlayer target, IPlayer initiator);
     void OnBetterChatMuteExpired(IPlayer player);
     string FormatTime(TimeSpan time);
    private bool TryGetDateTime(string source, DateTime date);
    private string SlackMessage(string key, KeyValuePair<string, string>[] replacements);
     T GetConfig(string name, T defaultValue);
     string Lang(string key, string id, object[] args);
}


```

---

## SlackNotices by MrBlue - Sends various notices from in-game to a Slack channel

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SlackNotices", "Wulf/lukespragg", "0.1.2", ResourceId = 1957)]
[Description("Sends connection and disconnection notices to Slack channel")]
 class SlackNotices : CovalencePlugin
{
    [PluginReference]
     Plugin Slack;
     void Init();
     string channel;
     bool connections;
     bool disconnections;
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     void OnUserConnected(IPlayer player);
     void OnUserDisconnected(IPlayer player, string reason);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
}


```

---

## SlackReport by MrBlue - Sends reports to Slack via in-game /report command

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("SlackReport", "Wulf/lukespragg", "0.1.3", ResourceId = 1954)]
[Description("Sends reports to Slack via in-game /report command")]
 class SlackReport : CovalencePlugin
{
    [PluginReference]
     Plugin Slack;
    const string permReport;
     void Init();
     string channel;
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
    [Command("report")]
     void ReportCommand(IPlayer player, string command, string[] args);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
     bool IsAllowed(string userId, string perm);
}


```

---

## Slap by MrBlue - Sometimes players just need to be slapped around a bit

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;
using Random = System.Random;

Oxide.Plugins
[Info("Slap", "Wulf", "2.0.2")]
[Description("Sometimes players just need to be slapped around a bit")]
 class Slap : CovalencePlugin
{
    private Configuration config;
     class Configuration
    {
        [JsonProperty("Command cooldown in seconds (0 to disable)")]
        public int CommandCooldown;
        [JsonProperty("Default damage per slap")]
        public int DefaultDamage;
        [JsonProperty("Default intensity per slap")]
        public int DefaultIntensity;
        [JsonProperty("Default amount of slaps")]
        public int DefaultAmount;
        [JsonProperty("Show players who slapped them")]
        public bool ShowWhoSlapped;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private readonly Hash<string, float> cooldowns;
    private static readonly Random random;
    private const string permUse;
    private void Init();
    private void CommandSlap(IPlayer player, string command, string[] args);
    private void SlapPlayer(IPlayer player, float damage, int intensity, int amount);
    private void AddLocalizedCommand(string command);
    private IPlayer FindPlayer(string playerNameOrId, IPlayer player);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}

 class Configuration
{
    [JsonProperty("Command cooldown in seconds (0 to disable)")]
    public int CommandCooldown;
    [JsonProperty("Default damage per slap")]
    public int DefaultDamage;
    [JsonProperty("Default intensity per slap")]
    public int DefaultIntensity;
    [JsonProperty("Default amount of slaps")]
    public int DefaultAmount;
    [JsonProperty("Show players who slapped them")]
    public bool ShowWhoSlapped;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## Slasher by k1lly0u - GTA 5 Slasher event arena game mode

```csharp
using Facepunch;
using Network;
using Newtonsoft.Json;
using Oxide.Plugins.EventManagerEx;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Slasher", "k1lly0u", "0.3.2"), Description("Team Deathmatch event mode for EventManager")]
 class Slasher : RustPlugin, IEventPlugin
{
    private string[] _torchItems;
    private string[] _validWeapons;
    private long _midnightTime;
    private long _middayTime;
    private const string WEAPON_FLASHLIGHT_ITEM;
    private static EnvSync EnvSync;
    private static Slasher Instance;
    private static List<BasePlayer> EventPlayers;
    private void Loaded();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
    private object CanNetworkTo(EnvSync env, BasePlayer player);
    private void Unload();
    private void FindValidWeapons();
    private string[] GetSlasherWeapons();
    private string[] GetSlasherTorches();
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
    public class SlasherEvent : EventManager.BaseEventGame
    {
        private ItemDefinition torchItem;
        private ItemDefinition slasherWeapon;
        private string slasherKit;
        private int slasherTime;
        private int playerTime;
        private int rounds;
        private int currentRound;
        private EventManager.BaseEventPlayer slasherPlayer;
        private List<EventManager.BaseEventPlayer> remainingSlashers;
        internal bool IsPlayingRound { get; set; }
        internal override void InitializeEvent(IEventPlugin plugin, EventManager.EventConfig config);
        protected override void OnDestroy();
        internal override void PrestartEvent();
        protected override void StartEvent();
        protected override EventManager.BaseEventPlayer AddPlayerComponent(BasePlayer player);
        internal override void LeaveEvent(BasePlayer player);
        protected override void OnKitGiven(EventManager.BaseEventPlayer eventPlayer);
        private void GiveSlasherWeapon(EventManager.BaseEventPlayer eventPlayer);
        private void GiveTorch(EventManager.BaseEventPlayer eventPlayer);
        protected override bool CanDropBackpack();
        internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo info);
        internal override void GetSpectateTargets(List<EventManager.BaseEventPlayer> list);
        private void StartRound();
        private void EndRound();
        private void OnSlasherTimerExpired();
        private EventManager.BaseEventPlayer GetRandomSlasher();
        private IEnumerator ResetPlayers();
        private IEnumerator GiveSlasherWeapons();
        protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
        protected override void BuildScoreboard();
        protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
        protected override void SortScores(List<EventManager.ScoreEntry> list);
    }

    private class SlasherPlayer : EventManager.BaseEventPlayer
    {
        internal override void OnPlayerDeath(EventManager.BaseEventPlayer attacker, float respawnTime);
    }

    private static ConfigData Configuration;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Amount of time between rounds (seconds)")]
        public int TimeBetweenRounds { get; set; }
        public Oxide.Core.VersionNumber Version { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    public string Message(string key, ulong playerId);
    private static Func<string, ulong, string> GetMessage;
    private readonly Dictionary<string, string> Messages;
}

public class SlasherEvent : EventManager.BaseEventGame
{
    private ItemDefinition torchItem;
    private ItemDefinition slasherWeapon;
    private string slasherKit;
    private int slasherTime;
    private int playerTime;
    private int rounds;
    private int currentRound;
    private EventManager.BaseEventPlayer slasherPlayer;
    private List<EventManager.BaseEventPlayer> remainingSlashers;
    internal bool IsPlayingRound { get; set; }
    internal override void InitializeEvent(IEventPlugin plugin, EventManager.EventConfig config);
    protected override void OnDestroy();
    internal override void PrestartEvent();
    protected override void StartEvent();
    protected override EventManager.BaseEventPlayer AddPlayerComponent(BasePlayer player);
    internal override void LeaveEvent(BasePlayer player);
    protected override void OnKitGiven(EventManager.BaseEventPlayer eventPlayer);
    private void GiveSlasherWeapon(EventManager.BaseEventPlayer eventPlayer);
    private void GiveTorch(EventManager.BaseEventPlayer eventPlayer);
    protected override bool CanDropBackpack();
    internal override void OnEventPlayerDeath(EventManager.BaseEventPlayer victim, EventManager.BaseEventPlayer attacker, HitInfo info);
    internal override void GetSpectateTargets(List<EventManager.BaseEventPlayer> list);
    private void StartRound();
    private void EndRound();
    private void OnSlasherTimerExpired();
    private EventManager.BaseEventPlayer GetRandomSlasher();
    private IEnumerator ResetPlayers();
    private IEnumerator GiveSlasherWeapons();
    protected override void GetWinningPlayers(List<EventManager.BaseEventPlayer> winners);
    protected override void BuildScoreboard();
    protected override float GetFirstScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override float GetSecondScoreValue(EventManager.BaseEventPlayer eventPlayer);
    protected override void SortScores(List<EventManager.ScoreEntry> list);
}

private class SlasherPlayer : EventManager.BaseEventPlayer
{
    internal override void OnPlayerDeath(EventManager.BaseEventPlayer attacker, float respawnTime);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Amount of time between rounds (seconds)")]
    public int TimeBetweenRounds { get; set; }
    public Oxide.Core.VersionNumber Version { get; set; }
}


```

---

## Sleep by  - Allows players with permission to get a well-rested sleep

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Sleep", "Wulf/lukespragg", "0.2.0", ResourceId = 1156)]
[Description("Allows players with permission to get a well-rested sleep")]
public class Sleep : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Cure while sleeping (true/false)")]
        public bool CureWhileSleeping;
        [JsonProperty(PropertyName = "Heal while sleeping (true/false)")]
        public bool HealWhileSleeping;
        [JsonProperty(PropertyName = "Restore while sleeping (true/false)")]
        public bool RestoreWhileSleeping;
        [JsonProperty(PropertyName = "Curing rate (0 - 100)")]
        public int CuringRate;
        [JsonProperty(PropertyName = "Healing rate (0 - 100)")]
        public int HealingRate;
        [JsonProperty(PropertyName = "Restoration rate (0 - 100)")]
        public int RestorationRate;
        public static Configuration DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private readonly Dictionary<string, Timer> sleepTimers;
    private const string permAllow;
    private void Init();
    private void Restore(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer basePlayer);
    private void SleepCommand(IPlayer player, string command, string[] args);
    private void AddCommandAliases(string key, string command);
    private string Lang(string key, string id, object[] args);
    private void Message(IPlayer player, string key, object[] args);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Cure while sleeping (true/false)")]
    public bool CureWhileSleeping;
    [JsonProperty(PropertyName = "Heal while sleeping (true/false)")]
    public bool HealWhileSleeping;
    [JsonProperty(PropertyName = "Restore while sleeping (true/false)")]
    public bool RestoreWhileSleeping;
    [JsonProperty(PropertyName = "Curing rate (0 - 100)")]
    public int CuringRate;
    [JsonProperty(PropertyName = "Healing rate (0 - 100)")]
    public int HealingRate;
    [JsonProperty(PropertyName = "Restoration rate (0 - 100)")]
    public int RestorationRate;
    public static Configuration DefaultConfig();
}


```

---

## SleeperAnimalProtection by Lorenzo - Protects sleeping players from being killed by animals, scarecrow and murderer.

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using Facepunch;
using Rust.Ai.Gen2;

Oxide.Plugins
[Info("Sleeper Animal Protection", "Fujikura/Krungh Crow/Lorenzo", "1.0.10")]
[Description("Protects sleeping players from being killed by animals")]
 class SleeperAnimalProtection : CovalencePlugin
{
    private readonly int buildingLayer;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings setting;
    }

    private class Settings
    {
        [JsonProperty(PropertyName = "OnNpcTarget return code (debug warning issue with TruePVE)")]
        public bool OnNpcReturnCode;
        [JsonProperty(PropertyName = "Permission name")]
        public string permissionName;
        [JsonProperty(PropertyName = "Required to sleep ON foundation")]
        public bool checkForFoundation;
        [JsonProperty(PropertyName = "Use permissions")]
        public bool usePermission;
        [JsonProperty(PropertyName = "Animal ignore sleepers")]
        public bool AnimalIgnoreSleepers;
        [JsonProperty(PropertyName = "HumanNPC ignore sleepers")]
        public bool HumanNPCIgnoreSleepers;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void Init();
    private List<BuildingBlock> GetFoundation(Vector3 positionCoordinates);
    private object OnNpcTargetSense(BaseEntity attacker, BaseEntity target, AIBrainSenses brainSenses);
     object OnEntityTakeDamage(BasePlayer player, HitInfo info);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings setting;
}

private class Settings
{
    [JsonProperty(PropertyName = "OnNpcTarget return code (debug warning issue with TruePVE)")]
    public bool OnNpcReturnCode;
    [JsonProperty(PropertyName = "Permission name")]
    public string permissionName;
    [JsonProperty(PropertyName = "Required to sleep ON foundation")]
    public bool checkForFoundation;
    [JsonProperty(PropertyName = "Use permissions")]
    public bool usePermission;
    [JsonProperty(PropertyName = "Animal ignore sleepers")]
    public bool AnimalIgnoreSleepers;
    [JsonProperty(PropertyName = "HumanNPC ignore sleepers")]
    public bool HumanNPCIgnoreSleepers;
}


```

---

## SleeperCount by Tryhard - Shows the total number of sleepers on command

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;

Oxide.Plugins
[Info("Sleeper Count", "NubbbZ", "1.2.1")]
[Description("Returns the total number of sleepers!")]
 class SleeperCount : CovalencePlugin
{
    private void Init();
    protected override void LoadDefaultMessages();
    [Command("sleepers")]
    private void TestCommand(IPlayer player, string command, string[] args);
}


```

---

## SleeperGroup by MrBlue - Puts players in a permissions group on disconnect if sleeping

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Sleeper Group", "Wulf", "1.0.0")]
[Description("Puts players in a permissions group on disconnect if sleeping")]
 class SleeperGroup : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Sleeper group name")]
        public string SleeperGroup;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
}

public class Configuration
{
    [JsonProperty("Sleeper group name")]
    public string SleeperGroup;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## SleeperGuard by  - Protects sleeping players from being hurt, killed, or looted

```csharp
using System.Collections.Generic;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Sleeper Guard", "Wulf/lukespragg/Arainrr", "1.1.2")]
[Description("Protects sleeping players from being hurt, killed, or looted")]
public class SleeperGuard : RustPlugin
{
    [PluginReference]
    private readonly Plugin Friends;
    private readonly Plugin Clans;
    private const string permCanLoot;
    private const string permCanDamage;
    private const string permNPCIgnore;
    private const string permNoTimeLimit;
    private const string permNoLootDelay;
    private const string permNoDamageDelay;
    private const string permLootProtection;
    private const string permDamageProtection;
    private readonly Dictionary<ulong, float> noticeTimes;
    private static readonly object True;
    private static readonly object False;
    private static readonly object Null;
    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private object OnNpcTarget(BaseEntity npc, BasePlayer player);
    private object CanBradleyApcTarget(BradleyAPC apc, BasePlayer player);
    private object OnEntityTakeDamage(BasePlayer target, HitInfo info);
    private object CanLootPlayer(BasePlayer target, BasePlayer looter);
    private void OnPlayerSleep(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private bool CanGuardPlayer(BasePlayer sleeper, bool isLoot, double timeleft);
    private bool CanIgnoreSleeper(BasePlayer player);
    private bool CanNotice(BasePlayer player);
    private bool AreFriends(ulong playerID, ulong friendID);
    private static bool SameTeam(ulong playerID, ulong friendID);
    private bool HasFriend(ulong playerID, ulong friendID);
    private bool SameClan(ulong playerID, ulong friendID);
    private static void NullifyDamage(HitInfo info);
    private ConfigData configData;
    public class ConfigData
    {
        [JsonProperty(PropertyName = "Use Teams (Used For Loot)")]
        public bool useTeams;
        [JsonProperty(PropertyName = "Use Clans (Used For Loot)")]
        public bool useClans;
        [JsonProperty(PropertyName = "Use Friends (Used For Loot)")]
        public bool useFriends;
        [JsonProperty(PropertyName = "Damage delay (seconds) (0 to disable)")]
        public double damageDelay;
        [JsonProperty(PropertyName = "Damage guard times (seconds) (0 to unlimit)")]
        public double damageGuardTime;
        [JsonProperty(PropertyName = "Loot delay (seconds) (0 to disable)")]
        public double lootDelay;
        [JsonProperty(PropertyName = "Loot guard times (seconds) (0 to unlimit)")]
        public double lootGuardTime;
        [JsonProperty(PropertyName = "Notify player (true/false)")]
        public bool notifyPlayer;
        [JsonProperty(PropertyName = "Notification interval (seconds)")]
        public float notifyInterval;
        [JsonProperty(PropertyName = "NPC ignores sleepers (Enabling it can cause server lag)")]
        public bool ignoreSleeper;
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings chatS;
        public class ChatSettings
        {
            [JsonProperty(PropertyName = "Chat prefix")]
            public string prefix;
            [JsonProperty(PropertyName = "Chat steamID icon")]
            public ulong steamIDIcon;
        }

        [JsonProperty(PropertyName = "Version")]
        public VersionNumber version;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private StoredData storedData;
    private class StoredData
    {
        public double lastSavedTimestamp;
        public readonly Dictionary<ulong, TimeData> sleeperDatas;
        public class TimeData
        {
            public double sleepStartTime;
            public double ignoredTime;
            [JsonIgnore]
            public double SecondsSleeping { get; set; }
            public TimeData();
        }

    }

    private void LoadData();
    private void ClearData();
    private void SaveData();
    private void OnNewSave(string filename);
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

public class ConfigData
{
    [JsonProperty(PropertyName = "Use Teams (Used For Loot)")]
    public bool useTeams;
    [JsonProperty(PropertyName = "Use Clans (Used For Loot)")]
    public bool useClans;
    [JsonProperty(PropertyName = "Use Friends (Used For Loot)")]
    public bool useFriends;
    [JsonProperty(PropertyName = "Damage delay (seconds) (0 to disable)")]
    public double damageDelay;
    [JsonProperty(PropertyName = "Damage guard times (seconds) (0 to unlimit)")]
    public double damageGuardTime;
    [JsonProperty(PropertyName = "Loot delay (seconds) (0 to disable)")]
    public double lootDelay;
    [JsonProperty(PropertyName = "Loot guard times (seconds) (0 to unlimit)")]
    public double lootGuardTime;
    [JsonProperty(PropertyName = "Notify player (true/false)")]
    public bool notifyPlayer;
    [JsonProperty(PropertyName = "Notification interval (seconds)")]
    public float notifyInterval;
    [JsonProperty(PropertyName = "NPC ignores sleepers (Enabling it can cause server lag)")]
    public bool ignoreSleeper;
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings chatS;
    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat steamID icon")]
        public ulong steamIDIcon;
    }

    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version;
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Chat prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat steamID icon")]
    public ulong steamIDIcon;
}

private class StoredData
{
    public double lastSavedTimestamp;
    public readonly Dictionary<ulong, TimeData> sleeperDatas;
    public class TimeData
    {
        public double sleepStartTime;
        public double ignoredTime;
        [JsonIgnore]
        public double SecondsSleeping { get; set; }
        public TimeData();
    }

}

public class TimeData
{
    public double sleepStartTime;
    public double ignoredTime;
    [JsonIgnore]
    public double SecondsSleeping { get; set; }
    public TimeData();
}


```

---

## SleeperMark by Razor - Creates a event for players to kill sleepers who have been sleeping for set amount of days

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using Rust;
using System.Linq;
using System.Globalization;
using Facepunch;
using UnityEngine.SceneManagement;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Sleeper Mark", "Ts3Hosting", "1.0.29")]
[Description("Create a event to mark sleepers to be killed by players")]
public class SleeperMark : RustPlugin
{
    [PluginReference]
     Plugin GUIAnnouncements;
     Plugin ServerRewards;
     Dictionary<ulong, HitInfo> LastWounded;
    static HashSet<PlayerData> LoadedPlayerData;
     List<UIObject> UsedUI;
     PlayerEntity pcdData;
    private DynamicConfigFile PCDDATA;
    private bool Changed;
    public bool debug;
    private int cooldownTime;
    private bool useRewards;
    private bool useguiAnnounce;
    private bool useAirdrop;
    private bool useBloon;
    private int LastSeenSeconds;
    private int reward;
    public bool EventStarted;
    public bool hitBalloon;
    public bool hitPlayer;
    public bool lockcrate;
    public bool usedirection;
    private int locktime;
    private int CrateAmount;
    public uint BloonNetID;
    public BasePlayer eventplayer;
    public LockedByEntCrate locked;
    public bool Effects;
     float max;
    const string bloonprefab;
    const string c4Explosion;
    const string boom;
    const string crates;
    const string supply;
    const string fireball;
    private HashSet<LockedByEntCrate> lockedCrates;
    public StorageContainer fuelTank;
     string colorTextMsg;
     string colorTextMsgB;
    private new void LoadDefaultMessages();
     void Unload();
     void Init();
     void OnServerInitialized();
    private void RegisterPermissions();
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
    private void CheckDependencies();
     void LoadData();
     class PlayerEntity
    {
        public Dictionary<ulong, PCDInfo> pEntity;
        public PlayerEntity();
    }

     class PCDInfo
    {
        public long LogOutDay;
        public float LocEntityX;
        public float LocEntityY;
        public float LocEntityZ;
        public uint BloonID;
        public long Cooldown;
        public PCDInfo();
        public PCDInfo(long cd);
    }

     void SaveData();
    private void OnPlayerDisconnected(BasePlayer player);
    readonly Dictionary<ulong, Timer> timers;
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     void rewardplayer(BasePlayer player, BasePlayer attacker);
     void OnEntityDeath(BaseCombatEntity victimEntity, HitInfo info);
    static double GrabCurrentTime();
    [ConsoleCommand("sleepermark")]
     void cmdRun(ConsoleSystem.Arg arg);
    [ChatCommand("sleepermark")]
    private void sleepermark(BasePlayer player);
    [ChatCommand("sleeper")]
    private void sleeperevent(BasePlayer player, string command, string[] args);
    private void SpawnLoot(int amount, BaseEntity EntitySpawn);
    private void UnlockCrate(LockedByEntCrate crate);
    [ChatCommand("sleepermarkfind")]
    private void test(BasePlayer player);
    private BaseEntity InstantiateEntity(string type, Vector3 position, Quaternion rotation);
    private void EventEnd();
    private void FindPositionsleep(BasePlayer player1);
    [ChatCommand("sleepermarkreset")]
    private void resetdata(BasePlayer reset1);
    private void resetdataboot();
    private void FindPositionsleepsetup();
     void MessageToAllGui(string message);
     void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
     void MessageToPlayer(BasePlayer player, string message);
     string GetDirectionAngle(float angle, string UserIDString);
     void NotifyOnEvent(BasePlayer loc);
     class PlayerData
    {
        public ulong id;
        public string name;
        public int kills;
        public int deaths;
        internal float KDR { get; set; }
        internal static void TryLoad(BasePlayer player);
        internal void Update(BasePlayer player);
        internal void Save();
        internal static PlayerData Find(BasePlayer player);
    }

     class UIColor
    {
         double red;
         double green;
         double blue;
         double alpha;
        public UIColor(double red, double green, double blue, double alpha);
        public override string ToString();
    }

     class UIObject
    {
         List<object> ui;
         List<string> objectList;
        public UIObject();
        public string RandomString();
        public void Draw(BasePlayer player);
        public void Destroy(BasePlayer player);
        public string AddPanel(string name, double left, double top, double width, double height, UIColor color, bool mouse, string parent);
        public string AddText(string name, double left, double top, double width, double height, UIColor color, string text, int textsize, string parent, int alignmode);
        public string AddButton(string name, double left, double top, double width, double height, UIColor color, string command, string parent, string closeUi);
        public string AddImage(string name, double left, double top, double width, double height, UIColor color, string url, string parent);
    }

     void DrawKDRWindow(BasePlayer player);
    private void LoadSleepers();
     string GetTopKills();
     string GetDeaths();
     string GetKDRs();
     string GetNames();
     void GetCurrentStats(BasePlayer player);
}

 class PlayerEntity
{
    public Dictionary<ulong, PCDInfo> pEntity;
    public PlayerEntity();
}

 class PCDInfo
{
    public long LogOutDay;
    public float LocEntityX;
    public float LocEntityY;
    public float LocEntityZ;
    public uint BloonID;
    public long Cooldown;
    public PCDInfo();
    public PCDInfo(long cd);
}

 class PlayerData
{
    public ulong id;
    public string name;
    public int kills;
    public int deaths;
    internal float KDR { get; set; }
    internal static void TryLoad(BasePlayer player);
    internal void Update(BasePlayer player);
    internal void Save();
    internal static PlayerData Find(BasePlayer player);
}

 class UIColor
{
     double red;
     double green;
     double blue;
     double alpha;
    public UIColor(double red, double green, double blue, double alpha);
    public override string ToString();
}

 class UIObject
{
     List<object> ui;
     List<string> objectList;
    public UIObject();
    public string RandomString();
    public void Draw(BasePlayer player);
    public void Destroy(BasePlayer player);
    public string AddPanel(string name, double left, double top, double width, double height, UIColor color, bool mouse, string parent);
    public string AddText(string name, double left, double top, double width, double height, UIColor color, string text, int textsize, string parent, int alignmode);
    public string AddButton(string name, double left, double top, double width, double height, UIColor color, string command, string parent, string closeUi);
    public string AddImage(string name, double left, double top, double width, double height, UIColor color, string url, string parent);
}


```

---

## SleepingAssignee by Ryz0r - Check who deployed a sleeping bag or bed, and who it is assigned to. Also check who's renamed a bag.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Sleeping Assignee", "Ryz0r", "1.3.0")]
[Description("Allows you to check whom a sleeping bag or bed is assigned to, and who it was deployed by.")]
internal class SleepingAssignee : RustPlugin
{
    private const string UsePerm;
    private Configuration config;
    private PluginData _data;
    private void SaveData();
    private void OnNewSave();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty(PropertyName = "Name History")]
        public Dictionary<ulong, List<string>> NameHistory;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void CheckBag(BasePlayer player);
    private object CanRenameBed(BasePlayer player, SleepingBag bed, string bedName);
    private string GetPlayerName(ulong playerID);
    private class Configuration
    {
        [JsonProperty(PropertyName = "Command To Check", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly List<string> CommandsToUse;
        [JsonProperty(PropertyName = "Prevent Naughty Named Bags")]
        public bool PreventBags;
        [JsonProperty(PropertyName = "Log Naughty Bags Attempts to Console")]
        public bool LogToConsole;
        [JsonProperty(PropertyName = "Naughty Word Filter", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public readonly List<string> NaughtyWords;
    }

}

private class PluginData
{
    [JsonProperty(PropertyName = "Name History")]
    public Dictionary<ulong, List<string>> NameHistory;
}

private class Configuration
{
    [JsonProperty(PropertyName = "Command To Check", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly List<string> CommandsToUse;
    [JsonProperty(PropertyName = "Prevent Naughty Named Bags")]
    public bool PreventBags;
    [JsonProperty(PropertyName = "Log Naughty Bags Attempts to Console")]
    public bool LogToConsole;
    [JsonProperty(PropertyName = "Naughty Word Filter", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public readonly List<string> NaughtyWords;
}


```

---

## SleepingBagTracker by RogderDodger - Sends notification to discord webhook when bag is assigned to a player who is not on the same team

```csharp
using Newtonsoft.Json;
using System;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using UnityEngine.Networking;
using UnityEngine;

Oxide.Plugins
[Info("Sleeping Bag Tracker", "Rogder Dodger", "1.0.3")]
[Description("Tracks Sleeping bags assigned to other players (with team information)")]
internal class SleepingBagTracker : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin Clans;
    private const string DefaultWebhookURL;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Log To Console")]
        public bool LogToConsole;
        [JsonProperty(PropertyName = "Log To Discord")]
        public bool LogToDiscord;
        [JsonProperty(PropertyName = "Discord Webhook URL")]
        public string DiscordWebhookUrl;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private string FormatMessage(string key, BagInfo bagInfo);
     object CanAssignBed(BasePlayer player, SleepingBag bag, ulong targetPlayerId);
    private void SendDiscordEmbed(BagInfo bagInfo);
    private IEnumerator PostToDiscord(string url, WWWForm data);
    private class BagInfo
    {
        public BasePlayer BagDeployer { get; set; }
        public BasePlayer BagAssigner { get; set; }
        public ulong AssigneeId { get; set; }
        public BasePlayer BagAssignee { get; set; }
        public SleepingBag SleepingBag { get; set; }
        public ulong DeployerTeamId { get; set; }
        public ulong AssigneeTeamId { get; set; }
        public string TeleportPos { get; set; }
        public string BagName { get; set; }
        public BagInfo(BasePlayer bagDeployer, BasePlayer assigner, BasePlayer assignee, ulong assigneeId, SleepingBag bag, ulong deployerTeamId, ulong assigneeTeamId);
    }

    private ulong GetPlayerTeamId(ulong steamId);
    private BasePlayer getPlayerById(ulong playerId);
    private bool IsAssignedToNonTeamMember(ulong assigner, ulong assignee);
    private bool AreClanMembers(ulong playerID, ulong assignee);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Log To Console")]
    public bool LogToConsole;
    [JsonProperty(PropertyName = "Log To Discord")]
    public bool LogToDiscord;
    [JsonProperty(PropertyName = "Discord Webhook URL")]
    public string DiscordWebhookUrl;
}

private class BagInfo
{
    public BasePlayer BagDeployer { get; set; }
    public BasePlayer BagAssigner { get; set; }
    public ulong AssigneeId { get; set; }
    public BasePlayer BagAssignee { get; set; }
    public SleepingBag SleepingBag { get; set; }
    public ulong DeployerTeamId { get; set; }
    public ulong AssigneeTeamId { get; set; }
    public string TeleportPos { get; set; }
    public string BagName { get; set; }
    public BagInfo(BasePlayer bagDeployer, BasePlayer assigner, BasePlayer assignee, ulong assigneeId, SleepingBag bag, ulong deployerTeamId, ulong assigneeTeamId);
}


```

---

## SleepProtection by Notchu - Protect players while sleeping

```csharp
using System;
using System.Diagnostics;
using Newtonsoft.Json;

[Info("Sleep Protection", "Notchu", "1.0.5")]
[Description("Protect players while sleeping")]
public class SleepProtection : CovalencePlugin
{
    private class Configuration
    {
        [JsonProperty("trigger animal on sleeping player")]
        public bool TriggerAnimalOnPlayer { get; set; }
        [JsonProperty("trigger npc player on sleeping player")]
        public bool TriggerNpcOnPlayer { get; set; }
        [JsonProperty("remove sleeping players from animal targeting")]
        public bool RemoveTargetFromAnimal { get; set; }
        [JsonProperty("remove sleeping players from npc players targeting")]
        public bool RemoveTargetFromNpcPlayer { get; set; }
        [JsonProperty("cancel damage to sleeping player")]
        public bool CancelDamageFromPlayersToSleepers { get; set; }
        [JsonProperty("cancel looting of sleeping player")]
        public bool CancelLootingSleepers { get; set; }
    }

    private Configuration _config;
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
     void Init();
     object OnNpcTarget(BaseEntity npc, BasePlayer player);
     object OnEntityTakeDamage(BasePlayer player, HitInfo info);
     object CanLootPlayer(BasePlayer target, BasePlayer looter);
     void RemovePlayerFromTarget(AIEvents events, BasePlayer player);
     bool CheckPermission(string playerId, string type);
}

private class Configuration
{
    [JsonProperty("trigger animal on sleeping player")]
    public bool TriggerAnimalOnPlayer { get; set; }
    [JsonProperty("trigger npc player on sleeping player")]
    public bool TriggerNpcOnPlayer { get; set; }
    [JsonProperty("remove sleeping players from animal targeting")]
    public bool RemoveTargetFromAnimal { get; set; }
    [JsonProperty("remove sleeping players from npc players targeting")]
    public bool RemoveTargetFromNpcPlayer { get; set; }
    [JsonProperty("cancel damage to sleeping player")]
    public bool CancelDamageFromPlayersToSleepers { get; set; }
    [JsonProperty("cancel looting of sleeping player")]
    public bool CancelLootingSleepers { get; set; }
}


```

---

## SlowmodeChat by Death - Temporarily or permanently restrict players to sending one message per configured interval

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Slowmode Chat", "Death", "1.0.8")]
[Description("Restrict players messages per second on a configurable interval")]
 class SlowmodeChat : RustPlugin
{
     List<string> CD;
    const string perm;
     object OnUserChat(IPlayer player);
     void Init();
    [ConsoleCommand("slowmode")]
     void ChangeSettings(ConsoleSystem.Arg arg);
    private ConfigData configData;
     class ConfigData
    {
        public Options Options;
        public Settings Settings;
    }

     class Options
    {
        public bool Enabled;
        public bool Rcon_Only;
        public bool Permission_Enabled;
    }

     class Settings
    {
        public int Interval;
    }

    private void LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
    protected override void LoadDefaultMessages();
    private string msg(string key, string id);
}

 class ConfigData
{
    public Options Options;
    public Settings Settings;
}

 class Options
{
    public bool Enabled;
    public bool Rcon_Only;
    public bool Permission_Enabled;
}

 class Settings
{
    public int Interval;
}


```

---

## SmallShelves by MrBlue - Allows players to place smaller shelves

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Small Shelves", "Wulf", "1.1.2")]
[Description("Allow players to place smaller shelves")]
public class SmallShelves : CovalencePlugin
{
    private readonly List<string> activatedIDs;
    private const string permUse;
    private const string prefab;
    private void Init();
    protected override void LoadDefaultMessages();
    private void SpawnSmallShelves(Vector3 pos, Quaternion rot, DecayEntity floor, ulong ownerId);
    private void OnEntitySpawned(BaseEntity entity);
    private object CanAcceptItem(ItemContainer container);
    private void CommandSmallShelves(IPlayer player, string command, string[] args);
    private void AddLocalizedCommand(string command);
    private string GetLang(string langKey, string playerId, object[] args);
    private void Message(IPlayer player, string textOrLang, object[] args);
}


```

---

## SmartChatBot by misticos - I send chat messages based on some triggers or time

```csharp
using System;
using System.Collections.Generic;
using System.Text;
using System.Text.RegularExpressions;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using Random = System.Random;
using Time = Oxide.Core.Libraries.Time;

Oxide.Plugins
[Info("Smart Chat Bot", "Iv Misticos", "2.0.13")]
[Description("I send chat messages based on some triggers or time.")]
 class SmartChatBot : RustPlugin
{
    [PluginReference]
    private Plugin PlaceholderAPI;
    private static readonly Random Random;
    private readonly Dictionary<BasePlayer, uint> _lastSent;
    private uint _lastSentGlobal;
    private static readonly Time Time;
    private const string CountryRequest;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Show Chat Prefix")]
        public bool ShowPrefix;
        [JsonProperty(PropertyName = "Bot Icon (SteamID)")]
        public ulong ChatSteamId;
        [JsonProperty(PropertyName = "Cooldown Between Auto Responses For User")]
        public string Cooldown;
        [JsonProperty(PropertyName = "Global Cooldown Between Auto Responses")]
        public string CooldownGlobal;
        [JsonProperty(PropertyName = "Use Default Chat (0), Chat Plus (1), Better Chat (2)")]
        public ushort ChatSystem;
        [JsonIgnore]
        public uint ParsedCooldown;
        [JsonIgnore]
        public uint ParsedCooldownGlobal;
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug;
        [JsonProperty(PropertyName = "Allow Multiple Auto Responses")]
        public bool MultipleAutoResponses;
        [JsonProperty(PropertyName = "Minimal Time Between Message And Answer")]
        public float MinTime;
        [JsonProperty(PropertyName = "Maximal Time Between Message And Answer")]
        public float MaxTime;
        [JsonProperty(PropertyName = "Welcome Message", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> WelcomeMessage;
        [JsonProperty(PropertyName = "Welcome Message Enabled")]
        public bool WelcomeMessageEnabled;
        [JsonProperty(PropertyName = "Joining Message", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> JoiningMessage;
        [JsonProperty(PropertyName = "Leaving Message", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> LeavingMessage;
        [JsonProperty(PropertyName = "Joining Message Enabled")]
        public bool JoiningMessageEnabled;
        [JsonProperty(PropertyName = "Leaving Message Enabled")]
        public bool LeavingMessageEnabled;
        [JsonProperty(PropertyName = "Show Joining Message To Player That Joined")]
        public bool JoiningMessageSelfEnabled;
        [JsonProperty(PropertyName = "Show Leaving Message To Player That Left")]
        public bool LeavingMessageSelfEnabled;
        [JsonProperty(PropertyName = "Auto Messages", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<AutoMessageGroup> AutoMessages;
        [JsonProperty(PropertyName = "Auto Responses", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<AutoResponseGroup> AutoResponses;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class AutoMessageGroup
    {
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Message Frequency")]
        public string Frequency;
        [JsonIgnore]
        public uint ParsedFrequency;
        [JsonIgnore]
        public short ActiveMessage;
        [JsonProperty(PropertyName = "Auto Messages", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<AutoMessage> AutoMessages;
    }

    private class AutoMessage
    {
        [JsonProperty(PropertyName = "Is Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Message")]
        public string Message;
    }

    private class AutoResponseGroup
    {
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Auto Responses", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<AutoResponse> AutoResponses;
    }

    private class AutoResponse
    {
        [JsonProperty(PropertyName = "Is Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Remove Message From Sender")]
        public bool RemoveMessage;
        [JsonProperty(PropertyName = "Send Response For Everyone (true) or Only For Sender (false)")]
        public bool SendPublic;
        [JsonProperty(PropertyName = "Triggers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<AutoResponseTrigger> Triggers;
        [JsonProperty(PropertyName = "Answers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Answers;
        public bool IsValid();
    }

    private class AutoResponseTrigger
    {
        [JsonProperty(PropertyName = "Percentage Of Contained Words")]
        public float ContainedWordsPercentage;
        [JsonProperty(PropertyName = "Regex Enabled")]
        public bool Regex;
        [JsonProperty(PropertyName = "Words", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Words;
    }

    private void OnServerInitialized();
    private object OnChatPlusMessage(Dictionary<string, object> data);
    private object OnChatPlusMessage(BasePlayer player, string message);
    private object OnBetterChat(Dictionary<string, object> data);
    private object OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void HandleCountryMessage(StringBuilder message, string ip, BasePlayer player, bool exclude);
    private void PrintDebug(string message);
    private void Publish(string message, string perm, BasePlayer player2, bool exclude);
    private void Publish(BasePlayer player, string message);
    private void SendMessage(BasePlayer player, string message);
    private string FormatMessage(string message);
    private string RunPlaceholders(IPlayer player, string message);
    private static readonly Regex RegexStringTime;
    private static bool ConvertToSeconds(string time, uint seconds);
    private object HandleChatMessage(BasePlayer player, string msg);
    private void TrySend(BasePlayer player, bool isPublic, string answer);
    private void HandleBroadcast(AutoMessageGroup group);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Show Chat Prefix")]
    public bool ShowPrefix;
    [JsonProperty(PropertyName = "Bot Icon (SteamID)")]
    public ulong ChatSteamId;
    [JsonProperty(PropertyName = "Cooldown Between Auto Responses For User")]
    public string Cooldown;
    [JsonProperty(PropertyName = "Global Cooldown Between Auto Responses")]
    public string CooldownGlobal;
    [JsonProperty(PropertyName = "Use Default Chat (0), Chat Plus (1), Better Chat (2)")]
    public ushort ChatSystem;
    [JsonIgnore]
    public uint ParsedCooldown;
    [JsonIgnore]
    public uint ParsedCooldownGlobal;
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug;
    [JsonProperty(PropertyName = "Allow Multiple Auto Responses")]
    public bool MultipleAutoResponses;
    [JsonProperty(PropertyName = "Minimal Time Between Message And Answer")]
    public float MinTime;
    [JsonProperty(PropertyName = "Maximal Time Between Message And Answer")]
    public float MaxTime;
    [JsonProperty(PropertyName = "Welcome Message", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> WelcomeMessage;
    [JsonProperty(PropertyName = "Welcome Message Enabled")]
    public bool WelcomeMessageEnabled;
    [JsonProperty(PropertyName = "Joining Message", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> JoiningMessage;
    [JsonProperty(PropertyName = "Leaving Message", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> LeavingMessage;
    [JsonProperty(PropertyName = "Joining Message Enabled")]
    public bool JoiningMessageEnabled;
    [JsonProperty(PropertyName = "Leaving Message Enabled")]
    public bool LeavingMessageEnabled;
    [JsonProperty(PropertyName = "Show Joining Message To Player That Joined")]
    public bool JoiningMessageSelfEnabled;
    [JsonProperty(PropertyName = "Show Leaving Message To Player That Left")]
    public bool LeavingMessageSelfEnabled;
    [JsonProperty(PropertyName = "Auto Messages", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<AutoMessageGroup> AutoMessages;
    [JsonProperty(PropertyName = "Auto Responses", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<AutoResponseGroup> AutoResponses;
}

private class AutoMessageGroup
{
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Message Frequency")]
    public string Frequency;
    [JsonIgnore]
    public uint ParsedFrequency;
    [JsonIgnore]
    public short ActiveMessage;
    [JsonProperty(PropertyName = "Auto Messages", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<AutoMessage> AutoMessages;
}

private class AutoMessage
{
    [JsonProperty(PropertyName = "Is Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Message")]
    public string Message;
}

private class AutoResponseGroup
{
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Auto Responses", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<AutoResponse> AutoResponses;
}

private class AutoResponse
{
    [JsonProperty(PropertyName = "Is Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Remove Message From Sender")]
    public bool RemoveMessage;
    [JsonProperty(PropertyName = "Send Response For Everyone (true) or Only For Sender (false)")]
    public bool SendPublic;
    [JsonProperty(PropertyName = "Triggers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<AutoResponseTrigger> Triggers;
    [JsonProperty(PropertyName = "Answers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Answers;
    public bool IsValid();
}

private class AutoResponseTrigger
{
    [JsonProperty(PropertyName = "Percentage Of Contained Words")]
    public float ContainedWordsPercentage;
    [JsonProperty(PropertyName = "Regex Enabled")]
    public bool Regex;
    [JsonProperty(PropertyName = "Words", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Words;
}


```

---

## SmoothRestarter by 2CHEVSKII - A reliable way to shutdown your server when you need it.

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Globalization;
using System.Linq;
using System.Reflection;
using System.Text;
using System.Text.RegularExpressions;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;
using UnityEngine.UI;
using Debug = UnityEngine.Debug;
using Pool = Facepunch.Pool;

Oxide.Plugins
[Info("SmoothRestarter", "2CHEVSKII", "3.2.0")]
[Description("A reliable way to shutdown your server when you need it")]
public class SmoothRestarter : CovalencePlugin
{
    const string PERMISSION_STATUS;
    const string PERMISSION_RESTART;
    const string PERMISSION_CANCEL;
    const string M_CHAT_PREFIX;
    const string M_NO_PERMISSION;
    const string M_KICK_REASON;
    const string M_HELP;
    const string M_HELP_HELP;
    const string M_HELP_STATUS;
    const string M_HELP_RESTART;
    const string M_HELP_CANCEL;
    const string M_RESTARTING_ALREADY;
    const string M_NOT_RESTARTING;
    const string M_CANCEL_SUCCESS;
    const string M_RESTART_REASON_TIMED;
    const string M_RESTART_REASON_OXIDE;
    const string M_RESTART_REASON_COMMAND;
    const string M_RESTART_REASON_API;
    const string M_ANNOUNCE_RESTART_INIT;
    const string M_ANNOUNCE_COUNTDOWN_TICK;
    const string M_ANNOUNCE_RESTART_CANCELLED;
    const string M_RESTART_SUCCESS;
    const string M_STATUS_RESTARTING;
    const string M_STATUS_RESTARTING_NATIVE;
    const string M_STATUS_PLANNED;
    const string M_STATUS_NO_PLANNED;
    const string M_UI_TITLE;
    const string M_UI_COUNTDOWN;
    static readonly VersionNumber CurrentOxideRustVersion;
    static SmoothRestarter Instance;
    readonly FieldInfo nativeRestartRoutine;
    readonly Dictionary<string, string> defaultMessagesEn;
    readonly Regex timeParseRegex;
     SmoothRestart component;
     PluginSettings settings;
     bool isNewOxideOut;
     bool IsRestarting { get; set; }
     bool IsRestartingNative { get; set; }
     bool IsRestartingComponent { get; set; }
     void Init();
     void OnServerInitialized();
     void OnUserConnected(IPlayer user);
     void OnUserDisconnected(IPlayer user);
     void Unload();
     void CommandHandler(IPlayer player, string _, string[] args);
     string GetHelp(IEnumerable<string> args);
     string GetStatus(DateTime restartTime);
     bool TryParseTime(string argString, TimeSpan time, bool isTod);
     bool CheckPermission(IPlayer player, string perm);
     void FetchLatestOxideRustVersion(Action<Exception, VersionNumber> callback);
     void PluginLog(string format, object[] args);
     void KickAll();
     void CancelNativeRestart();
    [HookMethod(nameof(IsSmoothRestarting))]
    public bool IsSmoothRestarting();
    [HookMethod(nameof(GetPlannedRestarts))]
    public IReadOnlyCollection<TimeSpan> GetPlannedRestarts();
    [HookMethod(nameof(GetCurrentRestartTime))]
    public DateTime? GetCurrentRestartTime();
    [HookMethod(nameof(GetCurrentRestartReason))]
    public int? GetCurrentRestartReason();
    [HookMethod(nameof(GetCurrentRestartInitiator))]
    public object GetCurrentRestartInitiator();
    [HookMethod(nameof(InitSmoothRestart))]
    public bool InitSmoothRestart(DateTime restartTime, Plugin initiator);
    [HookMethod(nameof(CancelSmoothRestart))]
    public bool CancelSmoothRestart(Plugin canceller);
    protected override void LoadDefaultMessages();
     string GetMessage(IPlayer player, string langKey);
     void Message(IPlayer player, string langKey, object[] args);
     void MessageWithCustomFormatter(IPlayer player, IFormatProvider formatProvider, string langKey, object[] args);
     void MessageRaw(IPlayer player, string message, object[] args);
     void AnnounceRestartInit(float secondsLeft, RestartReason reason, object initiator);
     void Announce(string langKey, object[] args);
     void AnnounceWithCustomFormatter(IFormatProvider formatProvider, string langKey, object[] args);
     void AnnounceRaw(string message, object[] args);
     string GetRestartReasonString(IPlayer player, RestartReason reason, object initiator);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class PluginSettings
    {
         float[] uiPosCached;
         HashSet<TimeSpan> restartTimesCached;
        [JsonProperty("Daily restarts")]
        public string[] DailyRestart { get; set; }
        [JsonProperty("Restart when new Oxide.Rust is out")]
        public bool OxideUpdateRestart { get; set; }
        [JsonProperty("Initiate countdown at")]
        public int RestartCountdownMax { get; set; }
        [JsonProperty("Enable UI")]
        public bool EnableUi { get; set; }
        [JsonProperty("UI position (X,Y)")]
        public string UiPosition { get; set; }
        [JsonProperty("UI scale")]
        public float UiScale { get; set; }
        [JsonProperty("Enable console logs")]
        public bool EnableLog { get; set; }
        [JsonProperty("Commands")]
        public string[] Commands { get; set; }
        [JsonProperty("Disable chat countdown notifications")]
        public bool DisableChatCountdown { get; set; }
        [JsonProperty("Custom countdown reference points")]
        public int[] CountdownRefPts { get; set; }
        [JsonProperty("Use custom countdown reference points")]
        public bool UseCustomCountdownRefPts { get; set; }
        [JsonProperty("Oxide update check interval in seconds")]
        public int OxideUpdateCheckInterval { get; set; }
        [JsonIgnore]
        public float UiX { get; set; }
        [JsonIgnore]
        public float UiY { get; set; }
        [JsonIgnore]
        public HashSet<TimeSpan> RestartTimes { get; set; }
        public static PluginSettings GetDefaults();
        public bool Validate();
         bool ParseUiPos(float fX, float fY);
         bool ParseRestartTimes(HashSet<TimeSpan> restartTimes);
    }

     class SmoothRestarterUi : MonoBehaviour
    {
        const string UI_BACKGROUND;
        const string UI_TITLE;
        const string UI_PROGRESS_BAR;
        const string UI_SECONDS;
        static HashSet<SmoothRestarterUi> AllComponents;
        static Dictionary<string, string> UiSecondsCache;
        static Dictionary<string, string> UiProgressbarCache;
         string mainPanel;
         BasePlayer player;
         bool isVisible;
         string locale;
         bool IsVisible { get; set; }
        public static void Cleanup();
         void Awake();
         void Start();
         void OnDestroy();
         void SetVisible(bool visible);
         void UiTick();
         void UpdateSeconds(double secondsLeft);
         void UpdateProgressBar(float fraction);
         string CreateSeconds(int secondsLeft);
         string CreateProgressBar(float fraction);
         float GetFraction(double secondsLeft);
         string GetFractionColor(float fraction);
        static string LookupUiDictionary(string locale, T value, Dictionary<string, string> dict, Func<T, string> createFunction);
         string SerializeUi(CuiElementContainer container);
    }

     class SmoothRestart : MonoBehaviour
    {
         IEnumerator restartRoutine;
         Queue<DateTime> restartQueue;
         int rtCountdownStart;
         bool doTimedRestarts;
         bool doRestartChecks;
         int oxideVersionCheckInterval;
        public DateTime? CurrentRestartTime { get; set; }
        public RestartReason? CurrentRestartReason { get; set; }
        public object CurrentRestartInitiator { get; set; }
        public bool IsRestarting { get; set; }
         bool DoRestartChecks { get; set; }
         void Awake();
         void Start();
         void OnDestroy();
        public void DoRestart(DateTime restartTime, RestartReason restartReason, object restartInitiator);
        public void CancelRestart(object canceller);
        public DateTime FindNextRestartTime(double diff);
         void OxideVersionCheck();
         void RestartCheck();
         bool NeedsRestartOnTime(DateTime restartTime);
         void CycleQueue();
         void OnRestartInit(float secondsLeft, RestartReason reason, object initiator);
         void OnRestartTick(int secondsLeft);
         void OnRestartCancelled(object canceller);
         IEnumerator InitRestartRoutine(float totalSecondsLeft);
         void Cleanup();
         void RestartNow();
         int GetNextCountdownValue(int secondsLeft);
    }

     class SmoothTimeFormatter : IFormatProvider, ICustomFormatter
    {
        public static SmoothTimeFormatter Instance;
        static readonly Regex TemplateRegex;
        public string Format(string format, object arg, IFormatProvider formatProvider);
        public object GetFormat(Type formatType);
        static int GetDigits(int number);
        static int GetDigits(double number);
        static string PadNumber(double value, int padding, int precision);
        static string Format(TimeSpan ts, string format);
        static IEnumerable<Template> GetTemplates(string format);
        static IEnumerable<Template> OrderTemplates(IEnumerable<Template> templates);
    }

}

 class PluginSettings
{
     float[] uiPosCached;
     HashSet<TimeSpan> restartTimesCached;
    [JsonProperty("Daily restarts")]
    public string[] DailyRestart { get; set; }
    [JsonProperty("Restart when new Oxide.Rust is out")]
    public bool OxideUpdateRestart { get; set; }
    [JsonProperty("Initiate countdown at")]
    public int RestartCountdownMax { get; set; }
    [JsonProperty("Enable UI")]
    public bool EnableUi { get; set; }
    [JsonProperty("UI position (X,Y)")]
    public string UiPosition { get; set; }
    [JsonProperty("UI scale")]
    public float UiScale { get; set; }
    [JsonProperty("Enable console logs")]
    public bool EnableLog { get; set; }
    [JsonProperty("Commands")]
    public string[] Commands { get; set; }
    [JsonProperty("Disable chat countdown notifications")]
    public bool DisableChatCountdown { get; set; }
    [JsonProperty("Custom countdown reference points")]
    public int[] CountdownRefPts { get; set; }
    [JsonProperty("Use custom countdown reference points")]
    public bool UseCustomCountdownRefPts { get; set; }
    [JsonProperty("Oxide update check interval in seconds")]
    public int OxideUpdateCheckInterval { get; set; }
    [JsonIgnore]
    public float UiX { get; set; }
    [JsonIgnore]
    public float UiY { get; set; }
    [JsonIgnore]
    public HashSet<TimeSpan> RestartTimes { get; set; }
    public static PluginSettings GetDefaults();
    public bool Validate();
     bool ParseUiPos(float fX, float fY);
     bool ParseRestartTimes(HashSet<TimeSpan> restartTimes);
}

 class SmoothRestarterUi : MonoBehaviour
{
    const string UI_BACKGROUND;
    const string UI_TITLE;
    const string UI_PROGRESS_BAR;
    const string UI_SECONDS;
    static HashSet<SmoothRestarterUi> AllComponents;
    static Dictionary<string, string> UiSecondsCache;
    static Dictionary<string, string> UiProgressbarCache;
     string mainPanel;
     BasePlayer player;
     bool isVisible;
     string locale;
     bool IsVisible { get; set; }
    public static void Cleanup();
     void Awake();
     void Start();
     void OnDestroy();
     void SetVisible(bool visible);
     void UiTick();
     void UpdateSeconds(double secondsLeft);
     void UpdateProgressBar(float fraction);
     string CreateSeconds(int secondsLeft);
     string CreateProgressBar(float fraction);
     float GetFraction(double secondsLeft);
     string GetFractionColor(float fraction);
    static string LookupUiDictionary(string locale, T value, Dictionary<string, string> dict, Func<T, string> createFunction);
     string SerializeUi(CuiElementContainer container);
}

 class SmoothRestart : MonoBehaviour
{
     IEnumerator restartRoutine;
     Queue<DateTime> restartQueue;
     int rtCountdownStart;
     bool doTimedRestarts;
     bool doRestartChecks;
     int oxideVersionCheckInterval;
    public DateTime? CurrentRestartTime { get; set; }
    public RestartReason? CurrentRestartReason { get; set; }
    public object CurrentRestartInitiator { get; set; }
    public bool IsRestarting { get; set; }
     bool DoRestartChecks { get; set; }
     void Awake();
     void Start();
     void OnDestroy();
    public void DoRestart(DateTime restartTime, RestartReason restartReason, object restartInitiator);
    public void CancelRestart(object canceller);
    public DateTime FindNextRestartTime(double diff);
     void OxideVersionCheck();
     void RestartCheck();
     bool NeedsRestartOnTime(DateTime restartTime);
     void CycleQueue();
     void OnRestartInit(float secondsLeft, RestartReason reason, object initiator);
     void OnRestartTick(int secondsLeft);
     void OnRestartCancelled(object canceller);
     IEnumerator InitRestartRoutine(float totalSecondsLeft);
     void Cleanup();
     void RestartNow();
     int GetNextCountdownValue(int secondsLeft);
}

 class SmoothTimeFormatter : IFormatProvider, ICustomFormatter
{
    public static SmoothTimeFormatter Instance;
    static readonly Regex TemplateRegex;
    public string Format(string format, object arg, IFormatProvider formatProvider);
    public object GetFormat(Type formatType);
    static int GetDigits(int number);
    static int GetDigits(double number);
    static string PadNumber(double value, int padding, int precision);
    static string Format(TimeSpan ts, string format);
    static IEnumerable<Template> GetTemplates(string format);
    static IEnumerable<Template> OrderTemplates(IEnumerable<Template> templates);
}


```

---

## SmoothRestarterRejoinRewards by 2CHEVSKII - Reward players if they re-join after restart

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Smooth Restarter Rejoin Rewards", "2CHEVSKII", "1.1.0")]
[Description("Reward players if they re-join after restart")]
public class SmoothRestarterRejoinRewards : CovalencePlugin
{
    const string PERMISSION_PREFIX;
    const string M_PREFIX;
    const string M_REWARDS;
    const string M_UNCLAIMED;
    const string M_NO_REWARDS;
    const string M_NO_SPACE;
    const string M_CLAIMED;
    const string M_RECEIVED_ITEMS;
    const string M_RECEIVED_ECONOMICS;
    const string M_RECEIVED_SR_POINTS;
    [PluginReference]
     Plugin SmoothRestarter;
    [PluginReference]
     Plugin ServerRewards;
    [PluginReference]
     Plugin Economics;
     PluginSettings settings;
     Dictionary<string, List<PluginSettings.RewardItem>> delayedItems;
     Dictionary<string, PluginSettings.PointReward> delayedPoints;
     List<string> rewardClaimers;
     Timer notificationTimer;
     DateTime currentRestartTime;
     bool isRestarting;
     void Init();
     void OnServerInitialized();
     void OnUserDisconnected(IPlayer player);
     void Unload();
     void OnPlayerSleepEnded(BasePlayer player);
     void OnSmoothRestartInit();
     void OnSmoothRestartTick();
     void OnSmoothRestartCancelled();
     void NotifyRewardsChance();
     void NotifyUnclaimedRewards(IPlayer player);
     void NotifyReceivedRewards(IPlayer player, List<PluginSettings.RewardItem> items, PluginSettings.PointReward points);
     void CommandHandler(IPlayer player);
     bool HasSlotsInInventory(BasePlayer player, bool beltContainer);
     ItemContainer GetFreeContainer(BasePlayer player);
     bool HasUnclaimedRewards(IPlayer player);
     void AddRewardClaimer(IPlayer player);
    [Conditional("DEBUG")]
     void Debug(string format, object[] args);
     void SaveData();
     List<string> LoadData();
     Dictionary<string, List<PluginSettings.RewardItem>> LoadRewards();
     Dictionary<string, PluginSettings.PointReward> LoadPoints();
     PluginSettings.PointReward GetPointsForPlayer(string userid);
     List<PluginSettings.RewardItem> GetRewardsForPlayer(string userid);
     PluginSettings.PointReward GetDefaultPoints();
     IEnumerable<PluginSettings.RewardItem> GetDefaultRewards();
     IEnumerable<PluginSettings.RewardItem> GetRewardsForPermission(string perm);
     PluginSettings.PointReward GetPointsForPermission(string perm);
     PluginSettings.RewardItem[] GetRewards(string key);
     PluginSettings.PointReward GetPoints(string key);
     void AddServerRewardsPoints(BasePlayer player, int points);
     void AddEconomicsPoints(IPlayer player, int points);
     IEnumerable<string> GetPermissionGroups(string userid);
     string ConstructPermission(string perm);
     string DeconstructPermission(string perm);
    protected override void LoadDefaultMessages();
     void Message(IPlayer player, string langKey, object[] args);
     string GetMessage(IPlayer player, string langKey);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     class PluginSettings
    {
        public static PluginSettings Default { get; set; }
        [JsonProperty("Notification frequency")]
        public float NotificationFrequency { get; set; }
        [JsonProperty("Reconnect threshold")]
        public float ReconnectThreshold { get; set; }
        [JsonProperty("Rewards")]
        public Dictionary<string, RewardItem[]> Rewards { get; set; }
        [JsonProperty("ServerRewards and economics points")]
        public Dictionary<string, PointReward> RewardPoints { get; set; }
        public static bool NeedsUpdate(PluginSettings settings);
    }

}

 class PluginSettings
{
    public static PluginSettings Default { get; set; }
    [JsonProperty("Notification frequency")]
    public float NotificationFrequency { get; set; }
    [JsonProperty("Reconnect threshold")]
    public float ReconnectThreshold { get; set; }
    [JsonProperty("Rewards")]
    public Dictionary<string, RewardItem[]> Rewards { get; set; }
    [JsonProperty("ServerRewards and economics points")]
    public Dictionary<string, PointReward> RewardPoints { get; set; }
    public static bool NeedsUpdate(PluginSettings settings);
}


```

---

## SolarPanelTweaker by  - Change solar panel attributes for all or on permission

```csharp
using System.Collections.Generic;
using System;

Oxide.Plugins
[Info("Solar Panel Tweaker", "BuzZ", "0.0.2")]
[Description("Change Solar Panel Attributes")]
public class SolarPanelTweaker : RustPlugin
{
     bool debug;
    private bool ConfigChanged;
    const string Tweaker;
     int MaxiPowa;
     bool SolarWorld;
     void Init();
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void SetASolarWorld();
     void OnEntitySpawned(BaseNetworkable entity);
     void SolarPanelTweakerizer(SolarPanel solarpanel);
}


```

---

## SortButton by WhiteThunder - Adds a sort button to storage boxes, allowing you to sort items by name or category

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Plugins;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Game.Rust.Cui;
using Pool = Facepunch.Pool;
using System.Collections.Generic;
using System.Linq;
using System;
using UnityEngine;

Oxide.Plugins
[Info("Sort Button", "MON@H", "2.4.1")]
[Description("Adds a sort button to storage boxes, allowing you to sort items by name or category")]
internal class SortButton : CovalencePlugin
{
    private Configuration _config;
    [PluginReference]
    private readonly Plugin Clans;
    private readonly Plugin Friends;
    private const string PermissionUse;
    private const string GUIPanelName;
    private const int MaxRows;
    private const float BaseYOffset;
    private const float YOffsetPerRow;
    private const float SortButtonWidth;
    private const float SortOrderButtonWidthString;
    private const string ButtonHeightString;
    private readonly Dictionary<string, string> OffsetYByLootPanel;
    private readonly string[] OffsetYByRow;
    private readonly Dictionary<string, string> HeightOverrideByLootPanel;
    private int[] _itemCategoryToSortIndex;
    private PlayerData _defaultPlayerData;
    private string _cachedUI;
    private readonly string[] _uiArguments;
    private readonly HashSet<ulong> _uiViewers;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnLootEntity(BasePlayer basePlayer, BaseEntity entity);
    private void OnPlayerLootEnd(PlayerLoot inventory);
    private void OnLootEntityEnd(BasePlayer player, BaseCombatEntity entity);
    private void CmdSortButton(IPlayer player, string cmd, string[] args);
    [Command("sortbutton.order")]
    private void Command_SortType(IPlayer player);
    [Command("sortbutton.sort")]
    private void Command_Sort(IPlayer player);
    private void UnsubscribeFromHooks();
    private void SubscribeToHooks();
    private void SetupItemCategories();
    private void RegisterPermissions();
    private void AddCommands();
    private bool IsPluginLoaded(Plugin plugin);
    private bool IsAlly(ulong playerId, ulong targetId);
    private bool IsClanMemberOrAlly(string playerId, string targetId);
    private bool IsFriend(string playerId, string targetId);
    private bool IsOnSameTeam(ulong playerId, ulong targetId);
    private void PlayerSendMessage(BasePlayer player, string message);
    private void HandleOnLootEntityDelayed(BasePlayer basePlayer, BaseEntity entity, string offsetXString, bool sortByCategory);
    private void HandleOnLootEntity(BasePlayer basePlayer, BaseEntity entity, bool delay);
    private bool IsSortableContainer(ItemContainer container);
    private bool CanPlayerSortEntity(BasePlayer basePlayer, BaseEntity entity);
    private string DetermineLootPanelName(BaseEntity entity);
    private bool TryDetermineYOffset(ItemContainer container, string lootPanelName, string offsetYString);
    private int CompareItems(Item a, Item b, bool byCategory);
    private void SortContainer(ItemContainer container, BasePlayer initiator, bool byCategory);
    private void CreateButtonUI(BasePlayer player, string offsetXString, string offsetYString, string heightString, bool sortByCategory);
    private void RecreateSortButton(BasePlayer player);
    private void DestroyUi(BasePlayer player);
    private StoredData _storedData;
    private class StoredData
    {
        public readonly Hash<ulong, PlayerData> PlayerData;
    }

    private class PlayerData
    {
        public bool Enabled;
        public bool SortByCategory;
    }

    private PlayerData GetPlayerData(ulong userID, bool createIfMissing);
    private void LoadData();
    private void SaveData();
    private void ClearData();
    private class ContainerConfiguration
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("OffsetX")]
        public float OffsetX;
        [JsonIgnore]
        private string _offsetXString;
        [JsonIgnore]
        public string OffsetXString { get; set; }
    }

    private class Configuration : BaseConfiguration
    {
        private static HashSet<string> OldRemovedPrefabs;
        [JsonProperty("Default enabled")]
        public bool DefaultEnabled;
        [JsonProperty("Default sort by category")]
        public bool DefaultSortByCategory;
        [JsonProperty("Check ownership")]
        public bool CheckOwnership;
        [JsonProperty("Use Clans")]
        public bool UseClans;
        [JsonProperty("Use Friends")]
        public bool UseFriends;
        [JsonProperty("Use Teams")]
        public bool UseTeams;
        [JsonProperty("Chat steamID icon")]
        public ulong SteamIDIcon;
        [JsonProperty("Chat command", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Commands;
        [JsonProperty("Containers by short prefab name")]
        private Dictionary<string, ContainerConfiguration> ContainersByPrefabPath;
        [JsonProperty("Containers by skin ID")]
        private Dictionary<ulong, ContainerConfiguration> ContainersBySkinId;
        [JsonIgnore]
        private Dictionary<uint, ContainerConfiguration> ContainersByPrefabId;
        public void OnServerInitialized(SortButton plugin);
        public ContainerConfiguration GetContainerConfiguration(BaseEntity entity);
    }

    private class BaseConfiguration
    {
        [JsonIgnore]
        public bool UsingDefaults;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private static class JsonHelper
    {
        public static object Deserialize(string json);
        private static object ToObject(JToken token);
    }

    private bool MaybeUpdateConfig(BaseConfiguration config);
    private bool MaybeUpdateConfigSection(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private string Lang(string key, string userIDString, object[] args);
    private static class LangKeys
    {
        public static class Error
        {
            private const string Base;
            public const string NoPermission;
        }

        public static class Info
        {
            private const string Base;
            public const string ButtonStatus;
            public const string Help;
            public const string SortType;
        }

        public static class Format
        {
            private const string Base;
            public const string ButtonText;
            public const string Category;
            public const string Disabled;
            public const string Enabled;
            public const string Name;
            public const string Prefix;
        }

    }

    protected override void LoadDefaultMessages();
}

private class StoredData
{
    public readonly Hash<ulong, PlayerData> PlayerData;
}

private class PlayerData
{
    public bool Enabled;
    public bool SortByCategory;
}

private class ContainerConfiguration
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("OffsetX")]
    public float OffsetX;
    [JsonIgnore]
    private string _offsetXString;
    [JsonIgnore]
    public string OffsetXString { get; set; }
}

private class Configuration : BaseConfiguration
{
    private static HashSet<string> OldRemovedPrefabs;
    [JsonProperty("Default enabled")]
    public bool DefaultEnabled;
    [JsonProperty("Default sort by category")]
    public bool DefaultSortByCategory;
    [JsonProperty("Check ownership")]
    public bool CheckOwnership;
    [JsonProperty("Use Clans")]
    public bool UseClans;
    [JsonProperty("Use Friends")]
    public bool UseFriends;
    [JsonProperty("Use Teams")]
    public bool UseTeams;
    [JsonProperty("Chat steamID icon")]
    public ulong SteamIDIcon;
    [JsonProperty("Chat command", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Commands;
    [JsonProperty("Containers by short prefab name")]
    private Dictionary<string, ContainerConfiguration> ContainersByPrefabPath;
    [JsonProperty("Containers by skin ID")]
    private Dictionary<ulong, ContainerConfiguration> ContainersBySkinId;
    [JsonIgnore]
    private Dictionary<uint, ContainerConfiguration> ContainersByPrefabId;
    public void OnServerInitialized(SortButton plugin);
    public ContainerConfiguration GetContainerConfiguration(BaseEntity entity);
}

private class BaseConfiguration
{
    [JsonIgnore]
    public bool UsingDefaults;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private static class LangKeys
{
    public static class Error
    {
        private const string Base;
        public const string NoPermission;
    }

    public static class Info
    {
        private const string Base;
        public const string ButtonStatus;
        public const string Help;
        public const string SortType;
    }

    public static class Format
    {
        private const string Base;
        public const string ButtonText;
        public const string Category;
        public const string Disabled;
        public const string Enabled;
        public const string Name;
        public const string Prefix;
    }

}

public static class Error
{
    private const string Base;
    public const string NoPermission;
}

public static class Info
{
    private const string Base;
    public const string ButtonStatus;
    public const string Help;
    public const string SortType;
}

public static class Format
{
    private const string Base;
    public const string ButtonText;
    public const string Category;
    public const string Disabled;
    public const string Enabled;
    public const string Name;
    public const string Prefix;
}


```

---

## SoundFX by Lincoln - Simulate various Rust sound effects

```csharp
using UnityEngine;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Sound FX", "Lincoln", "1.0.3")]
[Description("Simulate various Rust sound effects")]
 class SoundFX : RustPlugin
{
    private const string permUse;
    private const string permBypassCooldown;
    private readonly Hash<string, float> cooldowns;
    private List<string> ricochetEffects;
     string explosion;
     string vomit;
     string landmine;
     string scream;
     string fallDamage;
     string howl;
     string lick;
     string headshot;
     string chatter;
     string manDown;
     string roger;
     string takeCover;
     string slurp;
     string fish;
     string test;
    private static PluginConfig config;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Max Cooldown: ")]
        public float maxCooldown { get; set; }
        [JsonProperty(PropertyName = "Max Radius: ")]
        public float maxRadius { get; set; }
        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private bool hasPermission(BasePlayer player);
     bool OnCoolDown(BasePlayer player);
    [ChatCommand("fx")]
    private void fxCommand(BasePlayer player, string command, string[] args);
    private void SendEffect(string prefabName, Vector3 pos);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    protected override void LoadDefaultMessages();
    private void Unload();
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Max Cooldown: ")]
    public float maxCooldown { get; set; }
    [JsonProperty(PropertyName = "Max Radius: ")]
    public float maxRadius { get; set; }
    public static PluginConfig DefaultConfig();
}


```

---

## SpawnEntityGroup by Obito - Random spawn a custom entity group in a random map location

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Newtonsoft.Json;
using UnityEngine;
using Rust;

Oxide.Plugins
[Info("Spawn Entity Group", "Obito", "1.0.3")]
[Description("Random spawn a custom entity group in a random map location")]
 class SpawnEntityGroup : RustPlugin
{
    private float terrainSize;
     void OnServerInitialized();
    public void SpawnLooter(Vector3 pos);
    private void AddRigidbody(BaseEntity entity);
    private Vector3 GetVector(EntityData ent);
    private Vector3? GetSpawnPos();
    private bool TestPosIsValid(Vector3 randomPos);
    private static LayerMask GROUND_MASKS;
    static Vector3 GetGroundPosition(Vector3 sourcePos);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Spawn time")]
        public float spawnTime { get; set; }
        [JsonProperty(PropertyName = "Show console log")]
        public bool consoleLog { get; set; }
        [JsonProperty(PropertyName = "Log message")]
        public string logMsg { get; set; }
        [JsonProperty(PropertyName = "Entities")]
        public List<EntityData> entities;
    }

    private class EntityData
    {
        public string prefab;
        public float x;
        public float y;
        public float z;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Spawn time")]
    public float spawnTime { get; set; }
    [JsonProperty(PropertyName = "Show console log")]
    public bool consoleLog { get; set; }
    [JsonProperty(PropertyName = "Log message")]
    public string logMsg { get; set; }
    [JsonProperty(PropertyName = "Entities")]
    public List<EntityData> entities;
}

private class EntityData
{
    public string prefab;
    public float x;
    public float y;
    public float z;
}


```

---

## SpawnHeli by SpooksAU - Spawns a helicopter on command

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
[Info("Spawn Heli", "SpooksAU", "3.1.1")]
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
    private void OnEngineStarted(PlayerHelicopter heli, BasePlayer player);
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

    private static class NetworkUtils
    {
        public static void SendUpdateImmediateRecursive(BaseEntity entity);
    }

    private static class Ddraw
    {
        public static void Sphere(BasePlayer player, Vector3 origin, float radius, Color color, float duration);
        public static void Line(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
        public static void Box(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 extents, Color color, float duration);
        public static void Box(BasePlayer player, OBB obb, Color color, float duration);
    }

    public static void LogWarning(string message);
    public static void LogError(string message);
    private static bool VerifyPlayer(IPlayer player, BasePlayer basePlayer);
    private static void TryMountPlayer(BasePlayer player, PlayerHelicopter heli);
    private bool HasPermission(string playerId, string perm1, string perm2);
    private bool HasPermission(BasePlayer player, string perm1, string perm2);
    private bool HasPermission(IPlayer player, string perm1, string perm2);
    private bool VerifyPermission(IPlayer player, string perm1, string perm2);
    private bool VerifyVehicleExists(IPlayer player, BasePlayer basePlayer, VehicleInfo vehicleInfo, PlayerHelicopter heli);
    private bool VerifyVehicleWithinDistance(IPlayer player, BasePlayer basePlayer, PlayerHelicopter heli, float maxDistance);
    private static bool CheckBox(OBB obb, int layerMask, BaseEntity ignoreEntity);
    private bool VerifyValidSpawnOrFetchPosition(VehicleInfo vehicleInfo, BasePlayer player, Vector3 position, Quaternion rotation, PlayerHelicopter existingHeli);
    private bool VerifyOffCooldown(VehicleInfo vehicleInfo, BasePlayer player, CooldownConfig cooldownConfig, Dictionary<string, DateTime> cooldownMap);
    private bool VerifyNotBuildingBlocked(IPlayer player, BasePlayer basePlayer);
    private static bool SpawnWasBlocked(VehicleInfo vehicleInfo, BasePlayer player);
    private static bool FetchWasBlocked(VehicleInfo vehicleInfo, BasePlayer player, PlayerHelicopter heli);
    private static bool DespawnWasBlocked(VehicleInfo vehicleInfo, BasePlayer player, PlayerHelicopter heli);
    private static TimeSpan CeilingTimeSpan(TimeSpan timeSpan);
    private static Vector3 GetFixedPositionForPlayer(VehicleInfo vehicleInfo, BasePlayer player);
    private static Quaternion GetFixedRotationForPlayer(VehicleInfo vehicleInfo, BasePlayer player);
    private static void SetupHeli(PlayerHelicopter heli);
    private static void EnableUnlimitedFuel(PlayerHelicopter heli);
    private static bool AnyParentedPlayers(PlayerHelicopter heli);
    private static bool IsHeliOccupied(PlayerHelicopter heli);
    private static void UnparentPlayers(PlayerHelicopter heli);
    private bool IsPlayerVehicle(PlayerHelicopter heli, VehicleInfo vehicleInfo);
    private PlayerHelicopter FindPlayerVehicle(VehicleInfo vehicleInfo, BasePlayer player);
    private bool ShouldAutoMount(VehicleInfo vehicleInfo, BasePlayer player);
    private void MaybeAutoMount(VehicleInfo vehicleInfo, BasePlayer player, PlayerHelicopter heli);
    private void FetchVehicle(VehicleInfo vehicleInfo, IPlayer player, BasePlayer basePlayer, PlayerHelicopter heli);
    private PlayerHelicopter SpawnVehicle(VehicleInfo vehicleInfo, BasePlayer player, Vector3 position, Quaternion rotation, bool allowAutoMount);
    private void GiveVehicle(VehicleInfo vehicleInfo, BasePlayer player, Vector3? customPosition);
    private bool TryDespawnHeli(VehicleInfo vehicleInfo, PlayerHelicopter heli, BasePlayer basePlayer);
    private float GetPlayerCooldownSeconds(CooldownConfig cooldownConfig, BasePlayer player);
    private int GetPlayerAllowedFuel(VehicleInfo vehicleInfo, BasePlayer player);
    private void AddInitialFuel(VehicleInfo vehicleInfo, PlayerHelicopter heli, BasePlayer player);
    private class VehicleInfo
    {
        public static PermissionSet All;
        public class PermissionSet
        {
            public static PermissionSet ForVehicle(string vehicleName);
            private static string BuildPermission(string vehicleName, string featureName);
            public string Spawn { get; set; }
            public string Fetch { get; set; }
            public string Despawn { get; set; }
            public string UnlimitedFuel { get; set; }
            public string NoDecay { get; set; }
            public string NoCooldown { get; set; }
            public string AutoMount { get; set; }
            public string InstantTakeoff { get; set; }
            private PermissionSet();
            public IEnumerable<string> GetAllPermissions();
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
        public float TailLength;
        public float TailThickness;
        public float TailYOffset;
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
        public bool AnyInstantTakeoff { get; set; }
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
    private class AutoMountConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Require permission")]
        public bool RequirePermission;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class InstantTakeoffConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Require permission")]
        public bool RequirePermission;
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
        [JsonProperty("Auto mount")]
        public AutoMountConfig AutoMount;
        [JsonProperty("Instant takeoff")]
        public InstantTakeoffConfig InstantTakeoff;
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
        [JsonProperty("Admin debug bounds", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool AdminDebugBounds;
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

private static class NetworkUtils
{
    public static void SendUpdateImmediateRecursive(BaseEntity entity);
}

private static class Ddraw
{
    public static void Sphere(BasePlayer player, Vector3 origin, float radius, Color color, float duration);
    public static void Line(BasePlayer player, Vector3 origin, Vector3 target, Color color, float duration);
    public static void Box(BasePlayer player, Vector3 center, Quaternion rotation, Vector3 extents, Color color, float duration);
    public static void Box(BasePlayer player, OBB obb, Color color, float duration);
}

private class VehicleInfo
{
    public static PermissionSet All;
    public class PermissionSet
    {
        public static PermissionSet ForVehicle(string vehicleName);
        private static string BuildPermission(string vehicleName, string featureName);
        public string Spawn { get; set; }
        public string Fetch { get; set; }
        public string Despawn { get; set; }
        public string UnlimitedFuel { get; set; }
        public string NoDecay { get; set; }
        public string NoCooldown { get; set; }
        public string AutoMount { get; set; }
        public string InstantTakeoff { get; set; }
        private PermissionSet();
        public IEnumerable<string> GetAllPermissions();
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
    public float TailLength;
    public float TailThickness;
    public float TailYOffset;
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
    public static PermissionSet ForVehicle(string vehicleName);
    private static string BuildPermission(string vehicleName, string featureName);
    public string Spawn { get; set; }
    public string Fetch { get; set; }
    public string Despawn { get; set; }
    public string UnlimitedFuel { get; set; }
    public string NoDecay { get; set; }
    public string NoCooldown { get; set; }
    public string AutoMount { get; set; }
    public string InstantTakeoff { get; set; }
    private PermissionSet();
    public IEnumerable<string> GetAllPermissions();
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
    public bool AnyInstantTakeoff { get; set; }
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
private class AutoMountConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Require permission")]
    public bool RequirePermission;
}

[JsonObject(MemberSerialization.OptIn)]
private class InstantTakeoffConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Require permission")]
    public bool RequirePermission;
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
    [JsonProperty("Auto mount")]
    public AutoMountConfig AutoMount;
    [JsonProperty("Instant takeoff")]
    public InstantTakeoffConfig InstantTakeoff;
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
    [JsonProperty("Admin debug bounds", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool AdminDebugBounds;
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

## SpawnLogger by unboxingman - Logs all player spawned items to a file

```csharp
using System.Linq;
using System;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using UnityEngine.Assertions.Must;

Oxide.Plugins
[Info("Spawn Logger", "un-boxing-man & Lincoln", "1.0.8")]
[Description("Logs all player spawned items to a file.")]
public class SpawnLogger : RustPlugin
{
    private static PluginConfig config;
    private class PluginConfig
    {
        [JsonProperty(PropertyName = "Excluded Logging  ")]
        public List<string> LoggingExcludeList { get; set; }
        public static PluginConfig DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void OnEntitySpawned(BaseEntity entity);
    private void Log(string filename, string player, string id, string entity, string pos);
}

private class PluginConfig
{
    [JsonProperty(PropertyName = "Excluded Logging  ")]
    public List<string> LoggingExcludeList { get; set; }
    public static PluginConfig DefaultConfig();
}


```

---

## SpawnModularCar by WhiteThunder - Allows players to spawn modular cars, also supports presets

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using Rust.Modular;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using System.Text;

Oxide.Plugins
[Info("Spawn Modular Car", "WhiteThunder", "5.3.0")]
[Description("Allows players to spawn modular cars.")]
internal class SpawnModularCar : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin MonumentFinder;
    private readonly Plugin VehicleDeployedLocks;
    private static SpawnModularCar _pluginInstance;
    private static Configuration _pluginConfig;
    private PluginData _pluginData;
    private CommonPresets _commonPresets;
    private const string DefaultPresetName;
    private const int PresetMaxLength;
    private const string PermissionSpawnSockets2;
    private const string PermissionSpawnSockets3;
    private const string PermissionSpawnSockets4;
    private const string PermissionEnginePartsTier1;
    private const string PermissionEnginePartsTier2;
    private const string PermissionEnginePartsTier3;
    private const string PermissionFix;
    private const string PermissionFetch;
    private const string PermissionDespawn;
    private const string PermissionAutoFuel;
    private const string PermissionAutoCodeLock;
    private const string PermissionAutoKeyLock;
    private const string PermissionAutoStartEngine;
    private const string PermissionAutoFillTankers;
    private const string PermissionGiveCar;
    private const string PermissionPresets;
    private const string PermissionPresetLoad;
    private const string PermissionCommonPresets;
    private const string PermissionManageCommonPresets;
    private const string PrefabSockets2;
    private const string PrefabSockets3;
    private const string PrefabSockets4;
    private const string ItemDropPrefab;
    private const string RepairEffectPrefab;
    private const string TankerFilledEffectPrefab;
    private const int BoxcastLayers;
    private const int RaycastLayers;
    private static readonly Vector3 ShortCarExtents;
    private static readonly Vector3 MediumCarExtents;
    private static readonly Vector3 LongCarExtents;
    private static readonly Vector3 ShortCarFrontLeft;
    private static readonly Vector3 ShortCarFrontRight;
    private static readonly Vector3 ShortCarBackLeft;
    private static readonly Vector3 ShortCarBackRight;
    private static readonly Vector3 MediumCarFrontLeft;
    private static readonly Vector3 MediumCarFrontRight;
    private static readonly Vector3 MediumCarBackLeft;
    private static readonly Vector3 MediumCarBackRight;
    private static readonly Vector3 LongCarFrontLeft;
    private static readonly Vector3 LongCarFrontRight;
    private static readonly Vector3 LongCarBackLeft;
    private static readonly Vector3 LongCarBackRight;
    private static readonly float ForwardRaycastDistance;
    private const float DownwardRaycastDistance;
    private readonly RaycastHit[] _raycastBuffer;
    private readonly Dictionary<string, PlayerConfig> _playerConfigsMap;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave(string filename);
    private void OnEntityKill(ModularCar car);
    private void OnEngineStarted(ModularCar car, BasePlayer player);
    private static class ApiParser
    {
        public static string CodeLockField;
        public static string KeyLockField;
        public static string EnginePartsTierField;
        public static string FreshWaterAmountField;
        public static string FuelAmountField;
        public static string ModulesField;
        public static bool TryParseOptions(Dictionary<string, object> options, PresetCarOptions presetOptions);
        private static bool BoolOption(Dictionary<string, object> options, string name);
        private static int IntOption(Dictionary<string, object> options, string name);
        public static int[] ParseModulesOption(Dictionary<string, object> options);
    }

    private ModularCar API_SpawnPreset(Dictionary<string, object> options, BasePlayer player, Vector3 position, Quaternion rotation);
    private ModularCar API_SpawnNamedPreset(string presetName, BasePlayer player, Vector3 position, Quaternion rotation);
    private ModularCar API_SpawnPresetCar(BasePlayer player, Dictionary<string, object> options, Action<ModularCar> onReady);
    [Command("givecar")]
    private void SpawnCarServerCommand(IPlayer player, string cmd, string[] args);
    [Command("mycar")]
    private void MyCarCommand(IPlayer player, string cmd, string[] args);
    private void SubCommand_CommonPreset(IPlayer player, string[] args);
    private void SubCommand_Help(IPlayer player, string[] args);
    private void SubCommand_SpawnCar(IPlayer player, string[] args);
    private void SubCommand_Common_SpawnCar(IPlayer player, string[] args);
    private void SubCommand_FixCar(IPlayer player, string[] args);
    private void SubCommand_FetchCar(IPlayer player, string[] args);
    private void SubCommand_DestroyCar(IPlayer player, string[] args);
    private void SubCommand_ListPresets(IPlayer player, string[] args);
    private void SubCommand_Common_ListPresets(IPlayer player, string[] args);
    private void SubCommand_SavePreset(IPlayer player, string[] args);
    private void SubCommand_Common_SavePreset(IPlayer player, string[] args);
    private void SavePreset(IPlayer player, SimplePresetManager presetManager, string presetNameArg, ModularCar car);
    private void SubCommand_UpdatePreset(IPlayer player, string[] args);
    private void SubCommand_Common_UpdatePreset(IPlayer player, string[] args);
    private void UpdatePreset(IPlayer player, SimplePresetManager presetManager, string presetNameArg);
    private void SubCommand_LoadPreset(IPlayer player, string[] args);
    private void SubCommand_Common_LoadPreset(IPlayer player, string[] args);
    private void LoadPreset(IPlayer player, SimplePresetManager presetManager, string presetNameArg);
    private void SubCommand_RenamePreset(IPlayer player, string[] args);
    private void SubCommand_Common_RenamePreset(IPlayer player, string[] args);
    private void RenamePreset(IPlayer player, SimplePresetManager presetManager, string oldName, string newName);
    private void SubCommand_DeletePreset(IPlayer player, string[] args);
    private void SubCommand_Common_DeletePreset(IPlayer player, string[] args);
    private void DeletePreset(IPlayer player, SimplePresetManager presetManager, string presetNameArg);
    private void SubCommand_ToggleAutoCodeLock(IPlayer player, string[] args);
    private void SubCommand_ToggleAutoKeyLock(IPlayer player, string[] args);
    private void SubCommand_ToggleAutoFillTankers(IPlayer player, string[] args);
    private class MonumentAdapter
    {
        public string ShortName { get; set; }
        private Dictionary<string, object> _monumentInfo;
        public MonumentAdapter(Dictionary<string, object> monumentInfo);
        public bool IsInBounds(Vector3 position);
    }

    private MonumentAdapter GetClosestMonument(Vector3 position);
    private static bool SpawnWasBlocked(BasePlayer player);
    private static bool SpawnMyCarWasBlocked(BasePlayer player);
    private static bool FetchMyCarWasBlocked(BasePlayer player, ModularCar car);
    private static bool FixMyCarWasBlocked(BasePlayer player, ModularCar car);
    private static bool LoadMyCarPresetWasBlocked(BasePlayer player, ModularCar car);
    private static bool DestroyMyCarWasBlocked(BasePlayer player, ModularCar car);
    private static bool HasParent(BasePlayer player);
    private bool IsMonumentAllowed(BasePlayer basePlayer);
    private bool IsOnCargoShip(BasePlayer basePlayer);
    private bool VerifyPermissionAny(IPlayer player, string[] permissionNames);
    private bool VerifyLocationNotRestricted(IPlayer player);
    private bool VerifyNotBuildingBlocked(IPlayer player);
    private bool VerifySufficientSpace(IPlayer player, int numSockets, Vector3 determinedPosition, Quaternion determinedRotation);
    private bool VerifyHasPreset(IPlayer player, SimplePresetManager presetManager, string presetName, SimplePreset preset);
    private bool VerifyNoMatchingPreset(IPlayer player, SimplePresetManager presetManager, string presetName);
    private bool VerifyHasCar(IPlayer player, ModularCar car);
    private bool VerifyHasNoCar(IPlayer player);
    private bool VerifyCarNotOccupied(IPlayer player, ModularCar car);
    private bool VerifyOffCooldown(IPlayer player, CooldownType cooldownType);
    private bool VerifyOnlyOneMatchingPreset(IPlayer player, SimplePresetManager presetManager, string presetName, SimplePreset preset);
    private static int SortPresetNames(SimplePreset a, SimplePreset b);
    private static Vector3 GetCarExtents(int numSockets);
    private static void GetCarFrontBack(int numSockets, Vector3 frontLeft, Vector3 frontRight, Vector3 backLeft, Vector3 backRight);
    private static int[] GetCarModuleIDs(ModularCar car);
    private static Vector3 GetPlayerForwardPosition(BasePlayer player);
    private static Vector3 GetFixedCarPosition(BasePlayer player);
    private static bool TryGetIdealCarPositionAndRotation(BasePlayer player, int numSockets, Vector3 position, Quaternion rotation);
    private static void DetermineCarPositionAndRotation(BasePlayer player, int numSockets, Vector3 position, Quaternion rotation);
    private static Quaternion GetRelativeCarRotation(BasePlayer player);
    private static void AddInitialModules(ModularCar car, int[] ModuleIDs);
    private static void UpdateCarModules(ModularCar car, int[] moduleIDs);
    private static List<Item> AddEngineItemsAndReturnRemaining(ModularCar car, List<Item> engineItems);
    private static void AddUpgradeOrRepairEngineParts(EngineStorage engineStorage, int desiredTier);
    private static bool TryAddEngineItem(EngineStorage engineStorage, int slot, int tier);
    private static List<Item> ExtractEnginePartsAboveTierAndDeleteRest(ModularCar car, int tier);
    private static void GiveItemsToPlayerOrDrop(BasePlayer player, List<Item> itemList);
    private static void DropEngineParts(BasePlayer player, List<Item> itemList);
    private static void FixCar(ModularCar car, int fuelAmount, int enginePartsTier);
    private static void ReviveCar(ModularCar car);
    private static void AddOrRestoreFuel(ModularCar car, int specifiedFuelAmount);
    private bool TryReleaseCarFromLift(ModularCar car);
    private bool TryFindCarLift(ModularCar car, ModularCarGarage lift);
    private static void DismountAllPlayersFromCar(ModularCar car);
    private static void MaybeFillTankerModules(ModularCar car, int specifiedLiquidAmount);
    private static bool FillLiquidContainer(LiquidContainer liquidContainer, int specifiedAmount);
    private static void MaybePlayCarRepairEffects(ModularCar car);
    private static int Clamp(int x, int min, int max);
    private bool IsPlayerCar(ModularCar car);
    private ModularCar FindPlayerCar(IPlayer player);
    private bool HasSufficientSpace(BasePlayer player, int numSockets, Vector3 desiredPosition, Quaternion rotation);
    private int GetPlayerAllowedFreshWater(string userId);
    private int GetPlayerAllowedFuel(string userId);
    private int GetPlayerEnginePartsTier(string userId);
    private ushort GetPlayerMaxAllowedCarSockets(string userId);
    private void SpawnRandomCarForPlayer(IPlayer player, int desiredSockets);
    private void SpawnPresetCarForPlayer(IPlayer player, SimplePreset preset);
    private ModularCar SpawnCar(BaseCarOptions options, Vector3 position, Quaternion rotation, BasePlayer player, bool shouldTrackCar);
    private bool ShouldTryAddCodeLockForPlayer(string userId);
    private bool ShouldTryAddKeyLockForPlayer(string userId);
    private int[] ValidateModules(object[] moduleArray);
    private class PluginData : SimplePresetManager
    {
        [JsonProperty("playerCars")]
        public Dictionary<string, ulong> PlayerCars;
        [JsonProperty("Cooldowns")]
        public CooldownManager Cooldowns;
        public override List<SimplePreset> Presets { get; set; }
        public bool ShouldSerializePresets();
        public static PluginData LoadData();
        public override void SaveData();
        public void RegisterCar(string userId, ModularCar car);
        public void UnregisterCar(string userId);
        public long GetRemainingCooldownSeconds(string userId, CooldownType cooldownType);
        public void StartCooldown(string userId, CooldownType cooldownType, bool save);
    }

    private class CommonPresets : SimplePresetManager
    {
        private static string Filename { get; set; }
        public static CommonPresets LoadData(PluginData pluginData);
        public override void SaveData();
    }

    private PlayerConfig GetPlayerConfig(IPlayer player);
    private PlayerConfig GetPlayerConfig(string userId);
    private class CooldownManager
    {
        [JsonProperty("Spawn")]
        private Dictionary<string, long> Spawn;
        [JsonProperty("Fetch")]
        private Dictionary<string, long> Fetch;
        [JsonProperty("LoadPreset")]
        private Dictionary<string, long> LoadPreset;
        [JsonProperty("Fix")]
        private Dictionary<string, long> Fix;
        public Dictionary<string, long> GetCooldownMap(CooldownType cooldownType);
        public void ClearAll();
    }

    private abstract class SimplePresetManager
    {
        public static Func<SimplePreset, bool> MatchPresetName(string presetName);
        [JsonProperty("Presets")]
        public virtual List<SimplePreset> Presets { get; set; }
        public SimplePreset FindPreset(string presetName);
        public List<SimplePreset> FindMatchingPresets(string presetName);
        public void SavePreset(SimplePreset newPreset);
        public void UpdatePreset(SimplePreset newPreset);
        public void RenamePreset(SimplePreset preset, string newName);
        public void DeletePreset(SimplePreset preset);
        public abstract void SaveData();
    }

    private class PlayerConfig : SimplePresetManager
    {
        public static PlayerConfig Get(string dirPath, string ownerID);
        [JsonIgnore]
        private string Filepath;
        [JsonProperty("OwnerID")]
        public string OwnerID { get; set; }
        [JsonProperty("Settings")]
        public PlayerSettings Settings;
        public PlayerConfig(string ownerID);
        public override void SaveData();
    }

    private class SimplePreset
    {
        public static SimplePreset FromCar(ModularCar car, string presetName);
        [JsonProperty("Name")]
        public string Name;
        [JsonProperty("ModuleIDs")]
        public int[] ModuleIDs;
        [JsonIgnore]
        public int NumSockets { get; set; }
    }

    private class PlayerSettings
    {
        [JsonProperty("AutoCodeLock")]
        public bool AutoCodeLock;
        [JsonProperty("AutoKeyLock")]
        public bool AutoKeyLock;
        [JsonProperty("AutoFillTankers")]
        public bool AutoFillTankers;
    }

    private void MigrateConfig();
    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("CanSpawnWhileBuildingBlocked")]
        public bool CanSpawnBuildingBlocked;
        [JsonProperty("CanFetchWhileBuildingBlocked")]
        public bool CanFetchBuildingBlocked;
        [JsonProperty("CanFetchWhileOccupied")]
        public bool CanFetchOccupied;
        [JsonProperty("CanDespawnWhileOccupied")]
        public bool CanDespawnOccupied;
        [JsonProperty("DismountPlayersOnFetch")]
        public bool DismountPlayersOnFetch;
        [JsonProperty("FuelAmount")]
        public int FuelAmount;
        [JsonProperty("FreshWaterAmount")]
        public int FreshWaterAmount;
        [JsonProperty("MaxPresetsPerPlayer")]
        public int MaxPresetsPerPlayer;
        [JsonProperty("EnableEffects")]
        public bool EnableEffects;
        [JsonProperty("DisallowedMonuments")]
        private string[] DisallowedMonuments;
        [JsonProperty("Cooldowns")]
        public CooldownConfig Cooldowns;
        [JsonProperty("Presets")]
        public ServerPreset[] Presets;
        [JsonIgnore]
        public bool HasMonumentRestriction { get; set; }
        public ServerPreset FindPreset(string name);
        public bool ValidateServerPresets();
        public bool IsMonumentAllowed(string monumentName);
    }

    private class ServerPreset
    {
        [JsonProperty("Name")]
        public string Name;
        [JsonProperty("Options")]
        public ServerPresetOptions Options;
    }

    private abstract class BaseCarOptions
    {
        private int _enginePartsTier;
        [JsonProperty("CodeLock", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool CodeLock;
        [JsonProperty("EnginePartsTier", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int EnginePartsTier { get; set; }
        [JsonProperty("FreshWaterAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int FreshWaterAmount;
        [JsonProperty("FuelAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int FuelAmount;
        [JsonProperty("KeyLock", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool KeyLock;
        [JsonIgnore]
        public abstract int Length { get; set; }
        public BaseCarOptions();
        public BaseCarOptions(string userId);
    }

    private class PresetCarOptions : BaseCarOptions
    {
        [JsonProperty("ModuleIDs")]
        public virtual int[] NormalizedModuleIDs { get; set; }
        [JsonIgnore]
        public override int Length { get; set; }
        public PresetCarOptions();
        public PresetCarOptions(string userId, int[] moduleIDs);
    }

    private class RandomCarOptions : BaseCarOptions
    {
        public int NumSockets;
        public override int Length { get; set; }
        public RandomCarOptions(string userId, int numSockets);
    }

    private class ServerPresetOptions : PresetCarOptions
    {
        public override int[] NormalizedModuleIDs { get; set; }
        public bool ShouldSerializeNormalizedModuleIDs();
        [JsonProperty("Modules")]
        public object[] Modules;
        public bool ValidateModules();
        private int[] NormalizeModuleIDs(int[] moduleIDs);
    }

    private class CooldownConfig
    {
        [JsonProperty("SpawnCarSeconds")]
        public long SpawnSeconds;
        [JsonProperty("FetchCarSeconds")]
        public long FetchSeconds;
        [JsonProperty("LoadPresetSeconds")]
        public long LoadPresetSeconds;
        [JsonProperty("FixCarSeconds")]
        public long FixSeconds;
        public long GetSeconds(CooldownType cooldownType);
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
    private string BooleanToLocalizedString(IPlayer player, bool value);
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    protected override void LoadDefaultMessages();
}

private static class ApiParser
{
    public static string CodeLockField;
    public static string KeyLockField;
    public static string EnginePartsTierField;
    public static string FreshWaterAmountField;
    public static string FuelAmountField;
    public static string ModulesField;
    public static bool TryParseOptions(Dictionary<string, object> options, PresetCarOptions presetOptions);
    private static bool BoolOption(Dictionary<string, object> options, string name);
    private static int IntOption(Dictionary<string, object> options, string name);
    public static int[] ParseModulesOption(Dictionary<string, object> options);
}

private class MonumentAdapter
{
    public string ShortName { get; set; }
    private Dictionary<string, object> _monumentInfo;
    public MonumentAdapter(Dictionary<string, object> monumentInfo);
    public bool IsInBounds(Vector3 position);
}

private class PluginData : SimplePresetManager
{
    [JsonProperty("playerCars")]
    public Dictionary<string, ulong> PlayerCars;
    [JsonProperty("Cooldowns")]
    public CooldownManager Cooldowns;
    public override List<SimplePreset> Presets { get; set; }
    public bool ShouldSerializePresets();
    public static PluginData LoadData();
    public override void SaveData();
    public void RegisterCar(string userId, ModularCar car);
    public void UnregisterCar(string userId);
    public long GetRemainingCooldownSeconds(string userId, CooldownType cooldownType);
    public void StartCooldown(string userId, CooldownType cooldownType, bool save);
}

private class CommonPresets : SimplePresetManager
{
    private static string Filename { get; set; }
    public static CommonPresets LoadData(PluginData pluginData);
    public override void SaveData();
}

private class CooldownManager
{
    [JsonProperty("Spawn")]
    private Dictionary<string, long> Spawn;
    [JsonProperty("Fetch")]
    private Dictionary<string, long> Fetch;
    [JsonProperty("LoadPreset")]
    private Dictionary<string, long> LoadPreset;
    [JsonProperty("Fix")]
    private Dictionary<string, long> Fix;
    public Dictionary<string, long> GetCooldownMap(CooldownType cooldownType);
    public void ClearAll();
}

private abstract class SimplePresetManager
{
    public static Func<SimplePreset, bool> MatchPresetName(string presetName);
    [JsonProperty("Presets")]
    public virtual List<SimplePreset> Presets { get; set; }
    public SimplePreset FindPreset(string presetName);
    public List<SimplePreset> FindMatchingPresets(string presetName);
    public void SavePreset(SimplePreset newPreset);
    public void UpdatePreset(SimplePreset newPreset);
    public void RenamePreset(SimplePreset preset, string newName);
    public void DeletePreset(SimplePreset preset);
    public abstract void SaveData();
}

private class PlayerConfig : SimplePresetManager
{
    public static PlayerConfig Get(string dirPath, string ownerID);
    [JsonIgnore]
    private string Filepath;
    [JsonProperty("OwnerID")]
    public string OwnerID { get; set; }
    [JsonProperty("Settings")]
    public PlayerSettings Settings;
    public PlayerConfig(string ownerID);
    public override void SaveData();
}

private class SimplePreset
{
    public static SimplePreset FromCar(ModularCar car, string presetName);
    [JsonProperty("Name")]
    public string Name;
    [JsonProperty("ModuleIDs")]
    public int[] ModuleIDs;
    [JsonIgnore]
    public int NumSockets { get; set; }
}

private class PlayerSettings
{
    [JsonProperty("AutoCodeLock")]
    public bool AutoCodeLock;
    [JsonProperty("AutoKeyLock")]
    public bool AutoKeyLock;
    [JsonProperty("AutoFillTankers")]
    public bool AutoFillTankers;
}

private class Configuration : SerializableConfiguration
{
    [JsonProperty("CanSpawnWhileBuildingBlocked")]
    public bool CanSpawnBuildingBlocked;
    [JsonProperty("CanFetchWhileBuildingBlocked")]
    public bool CanFetchBuildingBlocked;
    [JsonProperty("CanFetchWhileOccupied")]
    public bool CanFetchOccupied;
    [JsonProperty("CanDespawnWhileOccupied")]
    public bool CanDespawnOccupied;
    [JsonProperty("DismountPlayersOnFetch")]
    public bool DismountPlayersOnFetch;
    [JsonProperty("FuelAmount")]
    public int FuelAmount;
    [JsonProperty("FreshWaterAmount")]
    public int FreshWaterAmount;
    [JsonProperty("MaxPresetsPerPlayer")]
    public int MaxPresetsPerPlayer;
    [JsonProperty("EnableEffects")]
    public bool EnableEffects;
    [JsonProperty("DisallowedMonuments")]
    private string[] DisallowedMonuments;
    [JsonProperty("Cooldowns")]
    public CooldownConfig Cooldowns;
    [JsonProperty("Presets")]
    public ServerPreset[] Presets;
    [JsonIgnore]
    public bool HasMonumentRestriction { get; set; }
    public ServerPreset FindPreset(string name);
    public bool ValidateServerPresets();
    public bool IsMonumentAllowed(string monumentName);
}

private class ServerPreset
{
    [JsonProperty("Name")]
    public string Name;
    [JsonProperty("Options")]
    public ServerPresetOptions Options;
}

private abstract class BaseCarOptions
{
    private int _enginePartsTier;
    [JsonProperty("CodeLock", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool CodeLock;
    [JsonProperty("EnginePartsTier", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int EnginePartsTier { get; set; }
    [JsonProperty("FreshWaterAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int FreshWaterAmount;
    [JsonProperty("FuelAmount", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int FuelAmount;
    [JsonProperty("KeyLock", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool KeyLock;
    [JsonIgnore]
    public abstract int Length { get; set; }
    public BaseCarOptions();
    public BaseCarOptions(string userId);
}

private class PresetCarOptions : BaseCarOptions
{
    [JsonProperty("ModuleIDs")]
    public virtual int[] NormalizedModuleIDs { get; set; }
    [JsonIgnore]
    public override int Length { get; set; }
    public PresetCarOptions();
    public PresetCarOptions(string userId, int[] moduleIDs);
}

private class RandomCarOptions : BaseCarOptions
{
    public int NumSockets;
    public override int Length { get; set; }
    public RandomCarOptions(string userId, int numSockets);
}

private class ServerPresetOptions : PresetCarOptions
{
    public override int[] NormalizedModuleIDs { get; set; }
    public bool ShouldSerializeNormalizedModuleIDs();
    [JsonProperty("Modules")]
    public object[] Modules;
    public bool ValidateModules();
    private int[] NormalizeModuleIDs(int[] moduleIDs);
}

private class CooldownConfig
{
    [JsonProperty("SpawnCarSeconds")]
    public long SpawnSeconds;
    [JsonProperty("FetchCarSeconds")]
    public long FetchSeconds;
    [JsonProperty("LoadPresetSeconds")]
    public long LoadPresetSeconds;
    [JsonProperty("FixCarSeconds")]
    public long FixSeconds;
    public long GetSeconds(CooldownType cooldownType);
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


```

---

## Spawns by k1lly0u - Spawns database for external plugins

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Newtonsoft.Json.Converters;
using UnityEngine;

Oxide.Plugins
[Info("Spawns", "Reneb / k1lly0u", "2.0.36"), Description("A database of sets of spawn points, created by a user and used by other plugins")]
 class Spawns : RustPlugin
{
    private SpawnsData _spawnsData;
    private Dictionary<string, List<Vector3>> _loadedSpawnfiles;
    private Dictionary<ulong, List<Vector3>> _spawnFileCreators;
    private List<ulong> _isEditing;
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void VerifyFilesExist();
    private object LoadSpawns(string name);
    private object GetSpawnsCount(string filename);
    private object GetRandomSpawn(string filename);
    private object GetRandomSpawnRange(string filename, int min, int max);
    private object GetSpawn(string filename, int number);
    private string[] GetSpawnfileNames();
    [ChatCommand("spawns")]
     void cmdSpawns(BasePlayer player, string command, string[] args);
    private void DDrawPosition(BasePlayer player, Vector3 point, string name, float time);
    private void SendHelpText(BasePlayer player);
    private bool IsCreatingFile(BasePlayer player);
    private DynamicConfigFile data;
    private void SaveData();
    private void LoadData();
    private void SaveSpawnFile(BasePlayer player, string name);
    private object LoadSpawnFile(string name);
    private class SpawnsData
    {
        public List<string> Spawnfiles;
    }

    private class Spawnfile
    {
        public Dictionary<string, Vector3> spawnPoints;
    }

    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    private string Message(string key, string ID);
    private readonly Dictionary<string, string> Messages;
}

private class SpawnsData
{
    public List<string> Spawnfiles;
}

private class Spawnfile
{
    public Dictionary<string, Vector3> spawnPoints;
}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}


```

---

## Spectate by MrBlue - Allows only players with permission to spectate

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Spectate", "Wulf/lukespragg", "0.4.3")]
[Description("Allows only players with permission to spectate")]
public class Spectate : CovalencePlugin
{
    private new void LoadDefaultMessages();
    private readonly Dictionary<string, Vector3> lastPositions;
    private readonly Dictionary<string, string> spectating;
    private const string permUse;
    private void Init();
    private void SpectateCommand(IPlayer player, string command, string[] args);
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void ResetSpectate(IPlayer player);
    private string Lang(string key, string id, object[] args);
    private void AddLocalizedCommand(string key, string command);
    private void Message(IPlayer player, string key, object[] args);
}


```

---

## SpeedType by TMafono - Quickly type randomly generated words to win a prize

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Core;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Speed Type", "TMafono", "1.1.5")]
[Description("Quickly type randomly generated words to win a prize")]
 class SpeedType : RustPlugin
{
    private const string SpeedTypeAdmin;
    private bool EventActive;
    private string RandomWord;
    private bool StartTier2Event;
    private bool StartTier3Event;
     Timer EndEventTimer;
     Timer EventAutoTimer;
    private List<string> EventWords;
    private readonly DynamicConfigFile dataFile;
    private Dictionary<string, int> TierStates;
    private static Configuration config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Enable Automatic Events")]
        public bool AutoEventEnabled;
        [JsonProperty(PropertyName = "Event Frequency (Run Event Every X Seconds)")]
        public float EventFrequency;
        [JsonProperty(PropertyName = "Event Length (Ends After X Seconds)")]
        public float EventLength;
        [JsonProperty(PropertyName = "Minimum number of players to start a event")]
        public int StartEventMinPlayers;
        [JsonProperty(PropertyName = "Chat Icon (SteamID64)")]
        public ulong ChatIcon;
        [JsonProperty(PropertyName = "Tier 1 Letters/Numbers count")]
        public int Tier1LetterCount;
        [JsonProperty(PropertyName = "Enable Tier 2 Events")]
        public bool Tier2EventStatus;
        [JsonProperty(PropertyName = "Tier 2 Event Frequency (Every X events it will be a tier 2 event)")]
        public int Tier2Frequency;
        [JsonProperty(PropertyName = "Tier 2 Letters/Numbers count")]
        public int Tier2LetterCount;
        [JsonProperty(PropertyName = "Enable Tier 3 Events")]
        public bool Tier3EventStatus;
        [JsonProperty(PropertyName = "Tier 3 Event Frequency (Every X events it will be a tier 3 event)")]
        public int Tier3Frequency;
        [JsonProperty(PropertyName = "Tier 3 Letters/Numbers count")]
        public int Tier3LetterCount;
        [JsonProperty(PropertyName = "Tier 1 Loot (Item Shortname | Item Ammount)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Dictionary<string, int>> EventT1LootTable;
        [JsonProperty(PropertyName = "Tier 2 Loot (Item Shortname | Item Ammount)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Dictionary<string, int>> EventT2LootTable;
        [JsonProperty(PropertyName = "Tier 3 Loot (Item Shortname | Item Ammount)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Dictionary<string, int>> EventT3LootTable;
        [JsonProperty(PropertyName = "Log Events to console")]
        public bool LogEvents;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
     void StartSpeedTypeEvent(bool consolecmd);
    private void EndSpeedTypeEvent(BasePlayer winner);
    [ChatCommand("guess")]
    private void SpeedTypeCommand(BasePlayer player, string cmd, string[] args);
    private int RandomGen(int tableSize);
    private void GiveItem(BasePlayer player, Dictionary<string, int> selectedList);
    private void CheckTierStatus();
    private ItemDefinition FindItem(string itemName);
    private string SpeedEventWordGenerator(int wordcount);
    private void Broadcast(string message);
    private void Message(BasePlayer player, string message);
    private bool HasPermission(BasePlayer player);
    private string Lang(string key, string id, object[] args);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Enable Automatic Events")]
    public bool AutoEventEnabled;
    [JsonProperty(PropertyName = "Event Frequency (Run Event Every X Seconds)")]
    public float EventFrequency;
    [JsonProperty(PropertyName = "Event Length (Ends After X Seconds)")]
    public float EventLength;
    [JsonProperty(PropertyName = "Minimum number of players to start a event")]
    public int StartEventMinPlayers;
    [JsonProperty(PropertyName = "Chat Icon (SteamID64)")]
    public ulong ChatIcon;
    [JsonProperty(PropertyName = "Tier 1 Letters/Numbers count")]
    public int Tier1LetterCount;
    [JsonProperty(PropertyName = "Enable Tier 2 Events")]
    public bool Tier2EventStatus;
    [JsonProperty(PropertyName = "Tier 2 Event Frequency (Every X events it will be a tier 2 event)")]
    public int Tier2Frequency;
    [JsonProperty(PropertyName = "Tier 2 Letters/Numbers count")]
    public int Tier2LetterCount;
    [JsonProperty(PropertyName = "Enable Tier 3 Events")]
    public bool Tier3EventStatus;
    [JsonProperty(PropertyName = "Tier 3 Event Frequency (Every X events it will be a tier 3 event)")]
    public int Tier3Frequency;
    [JsonProperty(PropertyName = "Tier 3 Letters/Numbers count")]
    public int Tier3LetterCount;
    [JsonProperty(PropertyName = "Tier 1 Loot (Item Shortname | Item Ammount)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Dictionary<string, int>> EventT1LootTable;
    [JsonProperty(PropertyName = "Tier 2 Loot (Item Shortname | Item Ammount)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Dictionary<string, int>> EventT2LootTable;
    [JsonProperty(PropertyName = "Tier 3 Loot (Item Shortname | Item Ammount)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Dictionary<string, int>> EventT3LootTable;
    [JsonProperty(PropertyName = "Log Events to console")]
    public bool LogEvents;
}


```

---

## SpinDrop by misticos - Spin around dropped weapons and tools above the ground

```csharp
using System;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Spin Drop", "misticos", "1.0.7")]
[Description("Spin around dropped items")]
 class SpinDrop : RustPlugin
{
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Speed Modifier")]
        public float SpeedModifier;
        [JsonProperty(PropertyName = "Move Item UP On N")]
        public float HeightOnDrop;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private void OnItemDropped(Item item, BaseEntity entity);
    private class SpinDropController : MonoBehaviour
    {
        public static void AddToEntity(BaseEntity entity, Configuration config);
        public Configuration Config;
        private bool _triggered;
        private void OnCollisionEnter(Collision collision);
        private void FixedUpdate();
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "Speed Modifier")]
    public float SpeedModifier;
    [JsonProperty(PropertyName = "Move Item UP On N")]
    public float HeightOnDrop;
}

private class SpinDropController : MonoBehaviour
{
    public static void AddToEntity(BaseEntity entity, Configuration config);
    public Configuration Config;
    private bool _triggered;
    private void OnCollisionEnter(Collision collision);
    private void FixedUpdate();
}


```

---

## StackFuelGenerator by ninco90 - Changes the maximum fuel capacity of the Electric Generator

```csharp
using System.Collections.Generic;
using UnityEngine;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Stack Fuel Generator", "ninco90", "1.0.1")]
[Description("Change the maximum fuel capacity.")]
 class StackFuelGenerator : RustPlugin
{
    private ConfigData config;
    private class ConfigData
    {
        public int StackMax { get; set; }
    }

    protected override void LoadDefaultConfig();
    private void Init();
     void OnServerInitialized();
    private void OnEntitySpawned(FuelGenerator entity);
}

private class ConfigData
{
    public int StackMax { get; set; }
}


```

---

## StackItemStorage by  - Changes specific stack capacity on specific storage and change max stack size of item

```csharp
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Stack Item Storage", "Khan", "1.0.8")]
[Description("Change specific item stack capacity on specific storages.")]
 class StackItemStorage : RustPlugin
{
    [PluginReference]
    private Plugin StackModifier;
    private Configuration _config;
    private const string BackpackPrefab;
    private Dictionary<ulong, int> _itemStack;
    private Dictionary<string, string> _include;
    private Dictionary<string, string> _includeSpecial;
    private class ItemMaxStack
    {
        public string ShortName;
        public int MaxStackSize;
        public ItemMaxStack(string ShotName, int MaxStackSizes);
    }

    private class Container
    {
        public string DisplayName;
        public List<ItemMaxStack> Items;
    }

    private class Container2
    {
        public string DisplayName;
        public int MaxCapacity;
        public List<ItemMaxStack> Items;
        public Container2();
    }

    private class Configuration
    {
        [JsonProperty(PropertyName = "Customize Slot Sizes - Higher/Lower Storage Slots")]
        public Dictionary<string, Container2> ContainersSpecial;
        [JsonProperty(PropertyName = "Custom Stack Sizes per Container")]
        public Dictionary<string, Container> Containers;
    }

    private void CheckConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void CheckStorage(StorageContainer T);
    private void OnServerInitialized();
    private int OnMaxStackable(Item item);
    private void OnEntityBuilt(Planner plan, GameObject go);
}

private class ItemMaxStack
{
    public string ShortName;
    public int MaxStackSize;
    public ItemMaxStack(string ShotName, int MaxStackSizes);
}

private class Container
{
    public string DisplayName;
    public List<ItemMaxStack> Items;
}

private class Container2
{
    public string DisplayName;
    public int MaxCapacity;
    public List<ItemMaxStack> Items;
    public Container2();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Customize Slot Sizes - Higher/Lower Storage Slots")]
    public Dictionary<string, Container2> ContainersSpecial;
    [JsonProperty(PropertyName = "Custom Stack Sizes per Container")]
    public Dictionary<string, Container> Containers;
}


```

---

## StackModifier by Mabel - Modifies stack sizes of server game items

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using UnityEngine;
using System.Collections;
using Facepunch;

Oxide.Plugins
[Info("Stack Modifier", "Mabel", "2.0.7")]
[Description("Modify item stack sizes")]
public class StackModifier : RustPlugin
{
    static Dictionary<string, int> _defaults;
    static Dictionary<string, int> _FB;
    readonly List<string> _exclude;
    readonly Dictionary<string, string> _corrections;
    private PluginConfig _config;
    readonly Dictionary<string, string> _itemMap;
     IEnumerator CheckConfig();
    internal class PluginConfig : SerializableConfiguration
    {
        [JsonProperty("Disable Ammo/Fuel duplication fix (Recommended false)")]
        public bool DisableFix;
        [JsonProperty("Enable VendingMachine Ammo Fix (Recommended)")]
        public bool VendingMachineAmmoFix;
        [JsonProperty("Category Stack Multipliers", Order = 4)]
        public Dictionary<string, int> StackCategoryMultipliers;
        [JsonProperty("Stack Categories", Order = 5)]
        public Dictionary<string, Dictionary<string, _Items>> StackCategories;
        public void ResetCategory(string cat);
        public void SetCategory(string cat, int digit);
        public void SetItems(string cat, int digit);
        public void ToggleCats(string cat, bool toggle);
    }

    public class _Items
    {
        public string ShortName;
        public int ItemId;
        public string DisplayName;
        public int Modified;
        public bool Disable;
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

    private bool MaybeUpdateConfig(SerializableConfiguration config);
    private bool MaybeUpdateConfigDict(Dictionary<string, object> currentWithDefaults, Dictionary<string, object> currentRaw);
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
     Coroutine Updating;
     void Unload();
     void OnServerShutdown();
     void Init();
     void InitializeFB();
     void OnServerInitialized();
     void SaveDefaultStackSizes();
     void LoadDefaultStackSizes();
     void RestoreVanillaStackSizes();
     object CanStackItem(Item item, Item targetItem);
     bool HasVanillaContainer(ItemDefinition itemDefinition);
     object CanCombineDroppedItem(DroppedItem item, DroppedItem targetItem);
     Item OnItemSplit(Item item, int amount);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     object OnCardSwipe(CardReader cardReader, Keycard card, BasePlayer player);
     bool CanWaterItemsStack(Item item, Item targetItem);
}

internal class PluginConfig : SerializableConfiguration
{
    [JsonProperty("Disable Ammo/Fuel duplication fix (Recommended false)")]
    public bool DisableFix;
    [JsonProperty("Enable VendingMachine Ammo Fix (Recommended)")]
    public bool VendingMachineAmmoFix;
    [JsonProperty("Category Stack Multipliers", Order = 4)]
    public Dictionary<string, int> StackCategoryMultipliers;
    [JsonProperty("Stack Categories", Order = 5)]
    public Dictionary<string, Dictionary<string, _Items>> StackCategories;
    public void ResetCategory(string cat);
    public void SetCategory(string cat, int digit);
    public void SetItems(string cat, int digit);
    public void ToggleCats(string cat, bool toggle);
}

public class _Items
{
    public string ShortName;
    public int ItemId;
    public string DisplayName;
    public int Modified;
    public bool Disable;
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


```

---

## StackMultiplier by Ryz0r - Set a multiplier for all item stack sizes in the game, and block items some from be multiplied

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Stack Multiplier", "Ryz0r/UNKN0WN", "1.0.4")]
[Description("Allows you to multiply all items stack size by a multiplier, except those in the blocked config list.")]
public class StackMultiplier : CovalencePlugin
{
    private Configuration _config;
    private readonly Dictionary<string, int> _defaultSizes;
    private int _multiplier;
    private const string UsePerm;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Stack Multi")]
        public int Stackmulti;
        [JsonProperty(PropertyName = "BlockedList", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> BlockedList;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
     void Unload();
    [Command("stackmultiplier.multiply")]
    private void MultiplyCommand(IPlayer player, string command, string[] args);
    [Command("stackmultiplier.reset")]
    private void ResetCommand(IPlayer player, string command, string[] args);
    private void ChangeSize(ItemDefinition gameitem, int multiplier);
    private void ResetStacks();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Stack Multi")]
    public int Stackmulti;
    [JsonProperty(PropertyName = "BlockedList", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> BlockedList;
}


```

---

## StackSizeController by AnExiledDev - Allows setting the max stack size of every item

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Stack Size Controller", "AnExiledDev/patched by chrome", "4.1.3")]
[Description("Allows configuration of most items max stack size.")]
 class StackSizeController : CovalencePlugin
{
    [PluginReference]
     Plugin AirFuel;
     Plugin GetToDaChoppa;
     Plugin VehicleVendorOptions;
    private const string _vanillaDefaultsUri;
    private Configuration _config;
    private Dictionary<string, int> _vanillaDefaults;
    private readonly List<string> _ignoreList;
    private void Init();
    private void Unload();
    private class Configuration
    {
        public bool RevertStackSizesToVanillaOnUnload;
        public bool AllowStackingItemsWithDurability;
        public bool HidePrefixWithPluginNameInMessages;
        public float GlobalStackMultiplier;
        public Dictionary<string, float> CategoryStackMultipliers;
        public Dictionary<string, float> IndividualItemStackMultipliers;
        public Dictionary<string, int> IndividualItemStackSize;
        public VersionNumber VersionNumber;
    }

    private static Dictionary<string, object> GetCategoriesAndDefaults(object defaultValue);
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void EnsureConfigIntegrity();
    private Configuration GetDefaultConfig();
    private void UpdateIndividualItemStackMultiplier(int itemId, float multiplier);
    private void UpdateIndividualItemStackMultiplier(string shortname, float multiplier);
    private void UpdateIndividualItemStackSize(int itemId, int stackLimit);
    private void UpdateIndividualItemStackSize(string shortname, int stackLimit);
    private void PopulateIndividualItemStackSize();
    protected override void LoadDefaultMessages();
    private string GetMessage(string key);
    private string GetMessage(string key, string playerId);
    private void OnEntitySpawned(Minicopter heli);
     int OnMaxStackable(Item item);
    private void SetStackCommand(IPlayer player, string command, string[] args);
    private void SetAllStacksCommand(IPlayer player, string command, string[] args);
    private void SetStackCategoryCommand(IPlayer player, string command, string[] args);
    private void ItemSearchCommand(IPlayer player, string command, string[] args);
    private void ListCategoriesCommand(IPlayer player, string command, string[] args);
    private void ListCategoryItemsCommand(IPlayer player, string command, string[] args);
    private void GenerateVanillaStackSizeFileCommand(IPlayer player, string command, string[] args);
    private void GenerateVanillaStackSizeFile();
    private void DownloadVanillaDefaults();
    private void SetVanillaDefaults(int code, string response);
    private int GetVanillaStackSize(ItemDefinition itemDefinition);
    private int GetStackSize(int itemId);
    private int GetStackSize(ItemDefinition itemDefinition);
    private void SetStackSizes();
    private void RevertStackSizes();
}

private class Configuration
{
    public bool RevertStackSizesToVanillaOnUnload;
    public bool AllowStackingItemsWithDurability;
    public bool HidePrefixWithPluginNameInMessages;
    public float GlobalStackMultiplier;
    public Dictionary<string, float> CategoryStackMultipliers;
    public Dictionary<string, float> IndividualItemStackMultipliers;
    public Dictionary<string, int> IndividualItemStackSize;
    public VersionNumber VersionNumber;
}


```

---

## Staffmode by DezLife - Toggle on/off staff mode

```csharp
using UnityEngine;
using System;
using System.Collections.Generic;
using System.Reflection;
using Oxide.Core;
using Oxide.Game.Rust.Libraries;

Oxide.Plugins
[Info("Staffmode", "Canopy Sheep", "1.2.3", ResourceId = 2263)]
[Description("Toggle on/off staff mode")]
 class Staffmode : RustPlugin
{
    private string Version;
    readonly Dictionary<ulong, string> groupEditor;
    readonly List<string> editValues;
    static string UppercaseFirst(string s);
     bool CheckPermission(BasePlayer player, string perm);
     class Data
    {
        public Dictionary<string, StaffData> StaffData;
    }

     Data data;
     class GroupData
    {
        public Dictionary<string, Group> Groups;
    }

     GroupData groupData;
     class StaffData
    {
        public bool EnabledOffDutyMode;
        public StaffData(BasePlayer player);
    }

     class Group
    {
        public string GroupName;
        public int AuthLevel;
        public string OffDutyGroup;
        public string OnDutyGroup;
        public string PermissionNode;
    }

    private int AuthLevel;
    private string OffDutygroup;
    private string OnDutygroup;
    private string Permissionnode;
    private string groupname;
    private int groupcount;
    private int possibletotalerrors;
    private int possiblemajorerrors;
    private bool AlreadyPowered;
    private bool AlreadyAnnounced;
    private bool PermissionDenied;
    private bool AlreadyToggled;
    private ConfigData configData;
     class ConfigData
    {
        public SettingsData Settings { get; set; }
        public GamePlayeSettingsData GameplaySettings { get; set; }
        public DebugData Debug { get; set; }
        public string ConfigVersion { get; set; }
    }

     class SettingsData
    {
        public string PluginPrefix { get; set; }
        public string EditPermission { get; set; }
        public string Command { get; set; }
        public bool AnnounceOnToggle { get; set; }
        public bool LogOnToggle { get; set; }
        public bool DisconnectOnToggle { get; set; }
        public bool EnableGroupToggle { get; set; }
    }

     class GamePlayeSettingsData
    {
        public bool ShowMessages { get; set; }
        public bool CanAttack { get; set; }
        public bool CanBeTargetedByHeliAndTurrets { get; set; }
        public bool CanLootPlayer { get; set; }
    }

     class DebugData
    {
        public bool CheckGroupDataOnLoad { get; set; }
        public bool Dev { get; set; }
    }

     void TryConfig();
     void LoadConfig();
    protected override void LoadDefaultConfig();
    internal string Replace(string source, string name);
     string Lang(string key, object userID);
    private void Language();
     void Init();
     void RegisterPermissions();
     void LoadData();
     void SaveData();
     void SaveGroups();
     void CheckData(int value);
     object OnPlayerAttack(BasePlayer attacker, HitInfo info);
     object CanBeTargeted(BaseCombatEntity player, MonoBehaviour turret);
     object CanLootPlayer(BasePlayer target, BasePlayer looter);
    [ConsoleCommand("checkgroups")]
     void CheckGroupCommand(ConsoleSystem.Arg arg);
     void StaffToggleCommand(BasePlayer player, string cmd, string[] args);
}

 class Data
{
    public Dictionary<string, StaffData> StaffData;
}

 class GroupData
{
    public Dictionary<string, Group> Groups;
}

 class StaffData
{
    public bool EnabledOffDutyMode;
    public StaffData(BasePlayer player);
}

 class Group
{
    public string GroupName;
    public int AuthLevel;
    public string OffDutyGroup;
    public string OnDutyGroup;
    public string PermissionNode;
}

 class ConfigData
{
    public SettingsData Settings { get; set; }
    public GamePlayeSettingsData GameplaySettings { get; set; }
    public DebugData Debug { get; set; }
    public string ConfigVersion { get; set; }
}

 class SettingsData
{
    public string PluginPrefix { get; set; }
    public string EditPermission { get; set; }
    public string Command { get; set; }
    public bool AnnounceOnToggle { get; set; }
    public bool LogOnToggle { get; set; }
    public bool DisconnectOnToggle { get; set; }
    public bool EnableGroupToggle { get; set; }
}

 class GamePlayeSettingsData
{
    public bool ShowMessages { get; set; }
    public bool CanAttack { get; set; }
    public bool CanBeTargetedByHeliAndTurrets { get; set; }
    public bool CanLootPlayer { get; set; }
}

 class DebugData
{
    public bool CheckGroupDataOnLoad { get; set; }
    public bool Dev { get; set; }
}


```

---

## StaffRoster by MrBlue - Shows the staff roster and availability

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Staff Roster", "Mr. Blue", "2.0.1")]
[Description("Shows staff roster and availability")]
 class StaffRoster : CovalencePlugin
{
    private Dictionary<IPlayer, StaffMember> staffMembers;
    private PluginConfig config;
    private void Init();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
     string Msg(string msg, string SteamId);
     class PluginConfig
    {
        [JsonProperty("Statuses")]
        public List<StaffStatus> staffStatuses;
        [JsonProperty("Groups")]
        public List<StaffGroup> staffGroups;
    }

     class StaffMember
    {
        public StaffGroup Group;
        public StaffStatus Status;
        public StaffMember(StaffGroup group, StaffStatus status);
    }

     class StaffGroup
    {
        [JsonProperty("Group (Oxide group name)")]
        public string GroupName;
        public int Priority;
        public string Title;
        public StaffGroup(string groupName, int priority, string title);
    }

     class StaffStatus
    {
        public string Title;
        public string Command;
        public string Color;
        public StaffStatus(string title, string command, string color);
    }

    private StaffGroup GetStaffGroup(IPlayer player);
     void OnServerInitialized();
     void OnUserConnected(IPlayer player);
     void OnUserDisconnected(IPlayer player);
    [Command("staff")]
    private void StaffCommand(IPlayer player, string command, string[] args);
}

 class PluginConfig
{
    [JsonProperty("Statuses")]
    public List<StaffStatus> staffStatuses;
    [JsonProperty("Groups")]
    public List<StaffGroup> staffGroups;
}

 class StaffMember
{
    public StaffGroup Group;
    public StaffStatus Status;
    public StaffMember(StaffGroup group, StaffStatus status);
}

 class StaffGroup
{
    [JsonProperty("Group (Oxide group name)")]
    public string GroupName;
    public int Priority;
    public string Title;
    public StaffGroup(string groupName, int priority, string title);
}

 class StaffStatus
{
    public string Title;
    public string Command;
    public string Color;
    public StaffStatus(string title, string command, string color);
}


```

---

## StarterMoney by  - Customizes the starting balance of players with permissions

```csharp
using Oxide.Core;
using System;
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Starter Money", "Tricky", "1.1.0")]
[Description("Customizes the starting balance of players with perms")]
public class StarterMoney : CovalencePlugin
{
    [PluginReference]
    private Plugin Economics;
    private Plugin ServerRewards;
    private Plugin CashSystem;
     Configuration config;
     class Configuration
    {
        [JsonProperty(PropertyName = "Clear Data On New Save (Rust)")]
        public bool ClearData;
        [JsonProperty(PropertyName = "Use Economics")]
        public bool Economics;
        [JsonProperty(PropertyName = "Use Server Rewards")]
        public bool ServerRewards;
        [JsonProperty(PropertyName = "Use Cash System")]
        public bool CashSystem;
        [JsonProperty(PropertyName = "Cash System Currency")]
        public string CashSystemCurrency;
        [JsonProperty(PropertyName = "Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, double> Permissions;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Data data;
    private class Data
    {
        public List<string> Players;
    }

    private void SaveData();
    private void Init();
    private void Loaded();
    private void OnServerSave();
    private void Unload();
    private void OnUserConnected(IPlayer player);
    private void OnNewSave();
    private void TryGiveEconomics(IPlayer player, double amount);
    private void TryGiveRP(IPlayer player, int amount);
    private void TryGiveCS(IPlayer player, double amount);
}

 class Configuration
{
    [JsonProperty(PropertyName = "Clear Data On New Save (Rust)")]
    public bool ClearData;
    [JsonProperty(PropertyName = "Use Economics")]
    public bool Economics;
    [JsonProperty(PropertyName = "Use Server Rewards")]
    public bool ServerRewards;
    [JsonProperty(PropertyName = "Use Cash System")]
    public bool CashSystem;
    [JsonProperty(PropertyName = "Cash System Currency")]
    public string CashSystemCurrency;
    [JsonProperty(PropertyName = "Permissions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, double> Permissions;
}

private class Data
{
    public List<string> Players;
}


```

---

## StartProtection by  - Protects new players when they first connect after a server wipe

```csharp
using System;
using System.IO;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Start Protection", "wazzzup", "2.3.2")]
[Description("Protects new players when they first connect after a server wipe")]
public class StartProtection : RustPlugin
{
    [PluginReference]
     Plugin Friends;
     Plugin ImageLibrary;
     Plugin Duelist;
     Plugin EventManager;
     Plugin NoEscape;
     class StoredData
    {
        public Dictionary<ulong, ProtectionInfo> Players;
        public StoredData();
    }

     class StoredPlayersData
    {
        public HashSet<ulong> Players;
        public StoredPlayersData();
    }

     class ProtectionInfo
    {
        public ulong UserId;
        public int TimeLeft;
        public bool Multiple;
        public int InitTimestamp;
        public ProtectionInfo();
    }

     Timer ProtectionTimer;
     StoredData storedData;
     StoredPlayersData storedPlayersData;
    static readonly DateTime UnixEpoch;
    static readonly double MaxUnixSeconds;
     string nofight_png;
     bool subscribed;
    private bool Changed;
    private bool bProtectionEnabled;
    private bool bSleeperProtection;
    private bool bHelicopterProtection;
    private bool bCanPickupWeapons;
    private string UIIcon;
    private bool canLootHeli;
    private bool canLootDrop;
    private bool canLootFriends;
    private bool canLootFriendDeployables;
    private bool showUIIcon;
    private bool bUseRaidZones;
    private string UIIconAnchorMin;
    private string UIIconAnchorMax;
    private int UISecondsWarningBeforeEnd;
    private int UIIconFontSize;
    private string UILayer;
    private int iTime;
    private int iTimeAssign;
    private int iPunishment;
    private int iInactiveDays;
    private int iUpdateTimerInterval;
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void SaveData();
    [ConsoleCommand("sp.assign")]
     void cmdAssignProtection(ConsoleSystem.Arg arg);
    [ConsoleCommand("sp.end")]
     void cmdEndProtection(ConsoleSystem.Arg arg);
     Dictionary<string, string> logging;
     void Log(string text, string filename);
    [ChatCommand("sp")]
    private void SPCommand(BasePlayer player, string command, string[] args);
     void OnEnterZone(string zoneid, BasePlayer player);
     void OnExitZone(string zone, BasePlayer player);
    protected override void LoadDefaultConfig();
     void StartSubscribe(bool subscribe);
    private void OnServerInitialized();
     ulong iconID;
    private void LoadImage();
    private void Init();
     void Unload();
     void SaveLogs();
     void OnServerSave();
     void OnServerShutdown();
     void OnPlayerFirstInit(ulong steamid, int timeleft);
     void OnPlayerDisconnected(BasePlayer player);
    private void OnNewSave();
     void OnPlayerConnected(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private HitInfo OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object OnItemPickup(Item item, BasePlayer player);
     void OnLootEntity(BasePlayer player, BaseEntity entity);
     void DestroyUi(BasePlayer player);
    private void SPUiUser(BasePlayer player, string inputText);
    private void SPUi(BasePlayer player, string inputText);
    private object HasProtection(BasePlayer player);
     object CanDuel(BasePlayer player);
     object CanEventJoin(BasePlayer player);
    private void PrintToChatEx(BasePlayer player, string result, string tcolour);
     HashSet<ulong> endingProtection;
     HashSet<ulong> endingProtectionRaid;
     void RunEndingEffect(BasePlayer player);
     void EndProtection(BasePlayer player, bool fromRaid);
    private void UpdateProtectedListEx(BasePlayer player, bool init);
    private void UpdateProtectedList(bool init);
     bool HasProtectedPlayer(BasePlayer exclude);
    public Int32 UnixTimeStampUTC();
    public static DateTime UnixTimeStampToDateTime(double unixTimeStamp);
    private void RemoveOldUsers();
    private void PunishPlayer(BasePlayer player, int new_time, bool message);
    protected override void LoadDefaultMessages();
     string GetMessage(string key, string steamId);
}

 class StoredData
{
    public Dictionary<ulong, ProtectionInfo> Players;
    public StoredData();
}

 class StoredPlayersData
{
    public HashSet<ulong> Players;
    public StoredPlayersData();
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

## StashBlocker by  - Restricts the placement of stash containers

```csharp
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries.Covalence;
using Oxide.Plugins;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Stash Blocker", "Orange/Dana", "1.1.0")]
[Description("Restricts the placement of stash containers.")]
public class StashBlocker : RustPlugin
{
    private Configuration config { get; set; }
    private const string permissionIgnore;
    private const string stashPrefab;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Cannot Place Overall")]
        public bool CannotPlaceOverall { get; set; }
        [JsonProperty(PropertyName = "Cannot Place Outside Building Privilege Range")]
        public bool CannotPlaceOutsideBuildingPrivilegeRange { get; set; }
        [JsonProperty(PropertyName = "Cannot Place Nearby Entity")]
        public bool CannotPlaceNearbyEntity { get; set; }
        [JsonProperty(PropertyName = "Entity Detection Radius")]
        public float EntityDetectionRadius { get; set; }
        [JsonProperty(PropertyName = "Entities")]
        public List<string> Entities { get; set; }
        [JsonProperty(PropertyName = "Send Game Tip")]
        public bool SendGameTip { get; set; }
        [JsonProperty(PropertyName = "Game Tip Show Duration")]
        public float GameTipShowDuration { get; set; }
    }

    private Configuration GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private object CanBuild(Planner planner, Construction construction, Construction.Target target);
    private bool HasEntityNearby(Vector3 position);
    private void SendGameTip(BasePlayer player, string message);
    private bool HasPermission(BasePlayer player, string permissionName);
    private class Message
    {
        public const string CannotPlaceOverall;
        public const string CannotPlaceBuildingPrivilege;
        public const string CannotPlaceNearbyEntity;
    }

    protected override void LoadDefaultMessages();
    private string GetMessage(string messageKey, string playerId, object[] args);
    private void SendMessage(BasePlayer player, string messageKey, object[] args);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Cannot Place Overall")]
    public bool CannotPlaceOverall { get; set; }
    [JsonProperty(PropertyName = "Cannot Place Outside Building Privilege Range")]
    public bool CannotPlaceOutsideBuildingPrivilegeRange { get; set; }
    [JsonProperty(PropertyName = "Cannot Place Nearby Entity")]
    public bool CannotPlaceNearbyEntity { get; set; }
    [JsonProperty(PropertyName = "Entity Detection Radius")]
    public float EntityDetectionRadius { get; set; }
    [JsonProperty(PropertyName = "Entities")]
    public List<string> Entities { get; set; }
    [JsonProperty(PropertyName = "Send Game Tip")]
    public bool SendGameTip { get; set; }
    [JsonProperty(PropertyName = "Game Tip Show Duration")]
    public float GameTipShowDuration { get; set; }
}

private class Message
{
    public const string CannotPlaceOverall;
    public const string CannotPlaceBuildingPrivilege;
    public const string CannotPlaceNearbyEntity;
}


```

---

## StashHider by birthdates - Don't network stashes when they're hidden

```csharp
using Oxide.Core.Plugins;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Stash Hider", "birthdates", "1.1.0")]
[Description("Don't network stashes when they're hidden")]
public class StashHider : RustPlugin
{
    [PluginReference]
    private readonly Plugin AutomatedStashTraps;
    private int LayerMask { get; set; }
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void OnStashHidden(StashContainer stash, BasePlayer player);
    private static void HideStash(StashContainer stash);
    private static void ShowStash(StashContainer stash);
    private object CanSeeStash(BasePlayer player, StashContainer stashContainer);
    private void Unload();
    private static IEnumerable<StashContainer> GetStashes();
    private void OnServerInitialized();
    private bool PluginIsLoaded(Plugin plugin);
    private bool StashIsAutomatedTrap(StashContainer stash);
}


```

---

## StashMarker by  - Stash Marker shows the hidden stashes for the player that hid the stash on the in-game map

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Stash Marker", "supreme", "1.1.0")]
[Description("Marks the hidden stashes on the ingame map")]
public class StashMarker : RustPlugin
{
    const string permUse;
    const string permAdmin;
    private readonly Hash<ulong, List<MapMarkerGenericRadius>> _mapMarker;
    private WaitForEndOfFrame _cachedWaitForEndOfFrame;
    private HashSet<StashContainer> _stashes;
    private Coroutine _stashRoutine;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Marker Radius")]
        public float markerRadius;
        [JsonProperty(PropertyName = "Marker Alpha")]
        public float markerAlpha;
        [JsonProperty(PropertyName = "Marker Color")]
        public string markerColor;
        [JsonProperty(PropertyName = "Marker Color Outline")]
        public string markerColorOutline;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
     IEnumerator StashRoutine();
     void Init();
    private void Unload();
    private void OnEntitySpawned(StashContainer stash);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnEntityKill(StashContainer stash);
     void CanSeeStash(BasePlayer player, StashContainer stash);
     void CanHideStash(BasePlayer player, StashContainer stash);
    private object CanNetworkTo(MapMarkerGenericRadius marker, BasePlayer target);
    private MapMarkerGenericRadius GetOrAddMarker(BasePlayer player, Vector3 pos);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Marker Radius")]
    public float markerRadius;
    [JsonProperty(PropertyName = "Marker Alpha")]
    public float markerAlpha;
    [JsonProperty(PropertyName = "Marker Color")]
    public string markerColor;
    [JsonProperty(PropertyName = "Marker Color Outline")]
    public string markerColorOutline;
}


```

---

## StashSniffer by k1lly0u - Keeps track of all small stashes and where they are placed

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Configuration;
using Newtonsoft.Json.Converters;
using UnityEngine;

Oxide.Plugins
[Info("StashSniffer", "k1lly0u", "0.1.2", ResourceId = 2062)]
 class StashSniffer : RustPlugin
{
     StoredData storedData;
    private DynamicConfigFile data;
     Dictionary<uint, Vector3> stashCache;
     bool isInit;
     bool resetData;
     void OnNewSave(string filename);
     void Loaded();
     void OnServerInitialized();
     void OnServerSave();
     void OnEntitySpawned(BaseEntity entity, GameObject gameObject);
    private void OnEntityKill(BaseNetworkable entity);
     void FindAllStashes();
    [ChatCommand("stash")]
     void cmdSniff(BasePlayer player, string command, string[] args);
     void SaveData();
     void LoadData();
    private class UnityVector3Converter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

     class StoredData
    {
        public Dictionary<uint, Vector3> StashIDs;
    }

}

private class UnityVector3Converter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

 class StoredData
{
    public Dictionary<uint, Vector3> StashIDs;
}


```

---

## StashTraps by misticos - Catch ESP hackers quickly and efficiently

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Text;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;
using Random = System.Random;

Oxide.Plugins
[Info("Stash Traps", "misticos", "2.0.3")]
[Description("Catch ESP hackers quickly and efficiently")]
public class StashTraps : CovalencePlugin
{
    private static StashTraps _ins;
    [PluginReference("PowerSpawn")]
    private Plugin _spawns;
    [PluginReference("PlaceholderAPI")]
    private Plugin _placeholders;
    private const string PermissionUse;
    private const string PermissionNotice;
    private const string PermissionIgnore;
    private const string PrefabStash;
    private HashSet<NetworkableId> _generatedStashes;
    private HashSet<NetworkableId> _alreadyFoundStashes;
    private Dictionary<string, int> _foundStashes;
    private Action<IPlayer, StringBuilder, bool> _placeholderProcessor;
    private Random _random;
    private Dictionary<string, string> _cachedHeaders;
    private string _webhookBodyCached;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Power Spawn Profile Name")]
        public string PowerSpawnProfile;
        [JsonProperty(PropertyName = "Commands")]
        public string[] Commands;
        [JsonProperty(PropertyName = "Generated Stashes")]
        public int StashCount;
        [JsonProperty(PropertyName = "Delete After Exposed In (Seconds)")]
        public float DeleteAfter;
        [JsonProperty(PropertyName = "Ignore Teammates")]
        public bool IgnoreTeam;
        [JsonProperty(PropertyName = "Notify Admins")]
        public bool NotifyAdmins;
        [JsonProperty(PropertyName = "Discord Settings")]
        public DiscordData Discord;
        [JsonProperty(PropertyName = "Spawned Items")]
        public int ItemsCount;
        [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<ItemData> Items;
        public class DiscordData
        {
            [JsonProperty(PropertyName = "Enabled")]
            public bool Enabled;
            [JsonProperty(PropertyName = "Webhook")]
            public string Webhook;
            [JsonProperty(PropertyName = "Setups", ObjectCreationHandling = ObjectCreationHandling.Replace)]
            public List<DiscordSetup> Setups;
            public class DiscordSetup
            {
                [JsonProperty(PropertyName = "Threshold")]
                public int Threshold;
                [JsonProperty(PropertyName = "Color (HEX)")]
                public string Color;
                [JsonProperty(PropertyName = "Inline")]
                public bool Inline;
                [JsonProperty(PropertyName = "Content")]
                public string Content;
                [JsonProperty(PropertyName = "Title: Player Stash Found")]
                public string StashPlayer;
                [JsonProperty(PropertyName = "Title: Generated Stash Found")]
                public string StashGenerated;
                [JsonProperty(PropertyName = "Title: Player Stash Found With Foundation")]
                public string StashPlayerFoundation;
                [JsonProperty(PropertyName = "Title: Generated Stash Found With Foundation")]
                public string StashGeneratedFoundation;
                [JsonProperty(PropertyName = "Title: Stash")]
                public string TitleStash;
                [JsonProperty(PropertyName = "Text: Stash")]
                public string TextStash;
                [JsonProperty(PropertyName = "Title: Player")]
                public string TitlePlayer;
                [JsonProperty(PropertyName = "Text: Player")]
                public string TextPlayer;
                [JsonIgnore]
                public int ColorParsed;
                public static DiscordSetup Find(int found);
            }

        }

        public class ItemData
        {
            [JsonProperty(PropertyName = "Shortname")]
            public string Shortname;
            [JsonProperty(PropertyName = "Minimum Amount")]
            public int AmountMin;
            [JsonProperty(PropertyName = "Maximum Amount")]
            public int AmountMax;
            [JsonIgnore]
            public ItemDefinition Definition;
        }

    }

    protected override void LoadConfig();
    private string Escape(string input);
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnStashExposed(StashContainer stash, BasePlayer target);
    private void OnStashExposedInternal(StashContainer stash, BasePlayer target, bool foundation);
    private void OnEntityKill(StashContainer stash);
    private void OnPlaceholderAPIReady();
    private void OnPluginUnloaded(Plugin plugin);
    private void CommandStashes(IPlayer player, string command, string[] args);
    private void RefillStashes();
    private readonly Quaternion _euler90;
    private bool TrySpawnStash();
    private string GetMsg(string key, string userId);
    private static void Shuffle(IList<T> list);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Power Spawn Profile Name")]
    public string PowerSpawnProfile;
    [JsonProperty(PropertyName = "Commands")]
    public string[] Commands;
    [JsonProperty(PropertyName = "Generated Stashes")]
    public int StashCount;
    [JsonProperty(PropertyName = "Delete After Exposed In (Seconds)")]
    public float DeleteAfter;
    [JsonProperty(PropertyName = "Ignore Teammates")]
    public bool IgnoreTeam;
    [JsonProperty(PropertyName = "Notify Admins")]
    public bool NotifyAdmins;
    [JsonProperty(PropertyName = "Discord Settings")]
    public DiscordData Discord;
    [JsonProperty(PropertyName = "Spawned Items")]
    public int ItemsCount;
    [JsonProperty(PropertyName = "Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<ItemData> Items;
    public class DiscordData
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Webhook")]
        public string Webhook;
        [JsonProperty(PropertyName = "Setups", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<DiscordSetup> Setups;
        public class DiscordSetup
        {
            [JsonProperty(PropertyName = "Threshold")]
            public int Threshold;
            [JsonProperty(PropertyName = "Color (HEX)")]
            public string Color;
            [JsonProperty(PropertyName = "Inline")]
            public bool Inline;
            [JsonProperty(PropertyName = "Content")]
            public string Content;
            [JsonProperty(PropertyName = "Title: Player Stash Found")]
            public string StashPlayer;
            [JsonProperty(PropertyName = "Title: Generated Stash Found")]
            public string StashGenerated;
            [JsonProperty(PropertyName = "Title: Player Stash Found With Foundation")]
            public string StashPlayerFoundation;
            [JsonProperty(PropertyName = "Title: Generated Stash Found With Foundation")]
            public string StashGeneratedFoundation;
            [JsonProperty(PropertyName = "Title: Stash")]
            public string TitleStash;
            [JsonProperty(PropertyName = "Text: Stash")]
            public string TextStash;
            [JsonProperty(PropertyName = "Title: Player")]
            public string TitlePlayer;
            [JsonProperty(PropertyName = "Text: Player")]
            public string TextPlayer;
            [JsonIgnore]
            public int ColorParsed;
            public static DiscordSetup Find(int found);
        }

    }

    public class ItemData
    {
        [JsonProperty(PropertyName = "Shortname")]
        public string Shortname;
        [JsonProperty(PropertyName = "Minimum Amount")]
        public int AmountMin;
        [JsonProperty(PropertyName = "Maximum Amount")]
        public int AmountMax;
        [JsonIgnore]
        public ItemDefinition Definition;
    }

}

public class DiscordData
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Webhook")]
    public string Webhook;
    [JsonProperty(PropertyName = "Setups", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<DiscordSetup> Setups;
    public class DiscordSetup
    {
        [JsonProperty(PropertyName = "Threshold")]
        public int Threshold;
        [JsonProperty(PropertyName = "Color (HEX)")]
        public string Color;
        [JsonProperty(PropertyName = "Inline")]
        public bool Inline;
        [JsonProperty(PropertyName = "Content")]
        public string Content;
        [JsonProperty(PropertyName = "Title: Player Stash Found")]
        public string StashPlayer;
        [JsonProperty(PropertyName = "Title: Generated Stash Found")]
        public string StashGenerated;
        [JsonProperty(PropertyName = "Title: Player Stash Found With Foundation")]
        public string StashPlayerFoundation;
        [JsonProperty(PropertyName = "Title: Generated Stash Found With Foundation")]
        public string StashGeneratedFoundation;
        [JsonProperty(PropertyName = "Title: Stash")]
        public string TitleStash;
        [JsonProperty(PropertyName = "Text: Stash")]
        public string TextStash;
        [JsonProperty(PropertyName = "Title: Player")]
        public string TitlePlayer;
        [JsonProperty(PropertyName = "Text: Player")]
        public string TextPlayer;
        [JsonIgnore]
        public int ColorParsed;
        public static DiscordSetup Find(int found);
    }

}

public class DiscordSetup
{
    [JsonProperty(PropertyName = "Threshold")]
    public int Threshold;
    [JsonProperty(PropertyName = "Color (HEX)")]
    public string Color;
    [JsonProperty(PropertyName = "Inline")]
    public bool Inline;
    [JsonProperty(PropertyName = "Content")]
    public string Content;
    [JsonProperty(PropertyName = "Title: Player Stash Found")]
    public string StashPlayer;
    [JsonProperty(PropertyName = "Title: Generated Stash Found")]
    public string StashGenerated;
    [JsonProperty(PropertyName = "Title: Player Stash Found With Foundation")]
    public string StashPlayerFoundation;
    [JsonProperty(PropertyName = "Title: Generated Stash Found With Foundation")]
    public string StashGeneratedFoundation;
    [JsonProperty(PropertyName = "Title: Stash")]
    public string TitleStash;
    [JsonProperty(PropertyName = "Text: Stash")]
    public string TextStash;
    [JsonProperty(PropertyName = "Title: Player")]
    public string TitlePlayer;
    [JsonProperty(PropertyName = "Text: Player")]
    public string TextPlayer;
    [JsonIgnore]
    public int ColorParsed;
    public static DiscordSetup Find(int found);
}

public class ItemData
{
    [JsonProperty(PropertyName = "Shortname")]
    public string Shortname;
    [JsonProperty(PropertyName = "Minimum Amount")]
    public int AmountMin;
    [JsonProperty(PropertyName = "Maximum Amount")]
    public int AmountMax;
    [JsonIgnore]
    public ItemDefinition Definition;
}


```

---

## StashWarning by haggbart - Logs suspicious stash activity and reports to admins in-game and on Discord

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Stash Warning", "haggbart", "1.3.5")]
[Description("Logs suspicious stash activity and reports to admins ingame and on discord")]
internal class StashWarning : RustPlugin
{
    [PluginReference]
     Plugin DiscordMessages;
    private const string WEBHOOK_URL;
    private const string MESSAGE;
    private const string NO_RECENT_WARNING;
    private const string IGNORE_SAME_TEAM;
    private string _webhookUrl;
    private bool _discordEnabled;
    private Vector3 position;
    private string message;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void Init();
    private void CanSeeStash(BasePlayer player, StashContainer stash);
    private bool IsTeamMember(BasePlayer player, StashContainer stash);
    private void AddWarning(BasePlayer player, IPlayer iPlayerOwner, BasePlayer target);
    private static string GridReference(Vector3 pos);
    [ChatCommand("sw")]
    private void TeleportLast(BasePlayer player);
}


```

---

## StationaryCrafting by  - Craft only when standing next to a workbench

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("Stationary Crafting", "NubbbZ", "1.0.2")]
[Description("Craft only when standing next to a workbench")]
 class StationaryCrafting : CovalencePlugin
{
     HashSet<ulong> InWorkbenchRadius;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void OnEntityEnter(TriggerWorkbench triggerWorkbench, BasePlayer player);
     bool CanCraft(ItemCrafter itemCrafter, ItemBlueprint bp, int amount);
    private void OnEntityLeave(TriggerWorkbench triggerWorkbench, BasePlayer player);
}


```

---

## StatisticsDB by misticos - Statistics database for developers

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using Time = Oxide.Core.Libraries.Time;

[Info("Statistics DB", "misticos", "1.2.7")]
[Description("Statistics database for developers")]
internal class StatisticsDB : CovalencePlugin
{
    [PluginReference]
    private Plugin ConnectionDB;
    private Plugin PlayerDatabase;
    private static PluginData _data;
    private static StatisticsDB _ins;
    private static readonly Time Time;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Debug")]
        public bool Debug { get; set; }
        [JsonProperty(PropertyName = "Inactive Entry Lifetime")]
        public uint Lifetime { get; set; }
        [JsonProperty(PropertyName = "Collect Joins")]
        public bool CollectJoins { get; set; }
        [JsonProperty(PropertyName = "Collect Leaves")]
        public bool CollectLeaves { get; set; }
        [JsonProperty(PropertyName = "Collect Kills")]
        public bool CollectKills { get; set; }
        [JsonProperty(PropertyName = "Collect Deaths")]
        public bool CollectDeaths { get; set; }
        [JsonProperty(PropertyName = "Collect Suicides")]
        public bool CollectSuicides { get; set; }
        [JsonProperty(PropertyName = "Collect Shots")]
        public bool CollectShots { get; set; }
        [JsonProperty(PropertyName = "Collect Headshots")]
        public bool CollectHeadshots { get; set; }
        [JsonProperty(PropertyName = "Collect Experiments")]
        public bool CollectExperiments { get; set; }
        [JsonProperty(PropertyName = "Collect Recoveries")]
        public bool CollectRecoveries { get; set; }
        [JsonProperty(PropertyName = "Collect Voice Bytes")]
        public bool CollectVoiceBytes { get; set; }
        [JsonProperty(PropertyName = "Collect Wounded Times")]
        public bool CollectWoundedTimes { get; set; }
        [JsonProperty(PropertyName = "Collect Crafted Items")]
        public bool CollectCraftedItems { get; set; }
        [JsonProperty(PropertyName = "Collect Repaired Items")]
        public bool CollectRepairedItems { get; set; }
        [JsonProperty(PropertyName = "Collect Lift Usages")]
        public bool CollectLiftUsages { get; set; }
        [JsonProperty(PropertyName = "Collect Wheel Spins")]
        public bool CollectWheelSpins { get; set; }
        [JsonProperty(PropertyName = "Collect Hammer Hits")]
        public bool CollectHammerHits { get; set; }
        [JsonProperty(PropertyName = "Collect Explosives Thrown")]
        public bool CollectExplosivesThrown { get; set; }
        [JsonProperty(PropertyName = "Collect Weapon Reloads")]
        public bool CollectWeaponReloads { get; set; }
        [JsonProperty(PropertyName = "Collect Rockets Launched")]
        public bool CollectRocketsLaunched { get; set; }
        [JsonProperty(PropertyName = "Collect Collectible Pickups")]
        public bool CollectCollectiblePickups { get; set; }
        [JsonProperty(PropertyName = "Collect Plant Pickups")]
        public bool CollectPlantPickups { get; set; }
        [JsonProperty(PropertyName = "Collect Gathered")]
        public bool CollectGathered { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void SaveData();
    private void SaveData(ulong id);
    private void LoadData(ulong id);
    private class PluginData
    {
        public Dictionary<ulong, PlayerStats> Statistics;
    }

    private class PlayerStats
    {
        public uint LastUpdate;
        public uint Joins;
        public uint Leaves;
        public uint Kills;
        public uint Deaths;
        public uint Suicides;
        public uint Shots;
        public uint Headshots;
        public uint Experiments;
        public uint Recoveries;
        public uint VoiceBytes;
        public uint WoundedTimes;
        public uint CraftedItems;
        public uint RepairedItems;
        public uint LiftUsages;
        public uint WheelSpins;
        public uint HammerHits;
        public uint ExplosivesThrown;
        public uint WeaponReloads;
        public uint RocketsLaunched;
        public uint SecondsPlayed;
        public List<string> Names;
        public List<string> IPs;
        public List<uint> TimeStamps;
        public Dictionary<string, uint> CollectiblePickups;
        public Dictionary<string, uint> PlantPickups;
        public Dictionary<string, uint> Gathered;
        public PlayerStats();
        internal PlayerStats(ulong id);
        public void Update();
        public static PlayerStats Find(ulong id);
    }

    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    [Command("statistics.migrate")]
    private void MigrateCommand(IPlayer player, string command, string[] args);
    [Command("statistics.output")]
    private void OutputCommand(IPlayer player, string command, string[] args);
    private void OnPlayerDisconnected(BasePlayer player);
    private void CanExperiment(BasePlayer player, Workbench workbench);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerWound(BasePlayer player);
    private void OnPlayerRecover(BasePlayer player);
    private void OnPlayerVoice(BasePlayer player, byte[] data);
    private void OnItemCraftFinished(ItemCraftTask task, Item item, ItemCrafter crafter);
    private void OnItemRepair(BasePlayer player, Item item);
    private void OnLiftUse(Lift lift, BasePlayer player);
    private void OnLiftUse(ProceduralLift lift, BasePlayer player);
    private void OnSpinWheel(BasePlayer player, SpinnerWheel wheel);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private void OnReloadWeapon(BasePlayer player, BaseProjectile projectile);
    private static Time _time;
    private void InitPlayer(BasePlayer player, bool isDisconnect);
    private void OnRocketLaunched(BasePlayer player, BaseEntity entity);
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProtoBuf.ProjectileShoot projectiles);
    private void OnCollectiblePickup(Item item, BasePlayer player);
    private void OnGrowableGather(GrowableEntity plant, Item item, BasePlayer player);
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private JObject API_GetAllData();
    private JObject API_GetAllPlayerData(ulong id);
    private bool API_ContainsPlayer(ulong id);
    private uint? API_GetJoins(ulong id);
    private uint? API_GetLeaves(ulong id);
    private uint? API_GetKills(ulong id);
    private uint? API_GetDeaths(ulong id);
    private uint? API_GetSuicides(ulong id);
    private uint? API_GetShots(ulong id);
    private uint? API_GetHeadshots(ulong id);
    private uint? API_GetExperiments(ulong id);
    private uint? API_GetRecoveries(ulong id);
    private uint? API_GetVoiceBytes(ulong id);
    private uint? API_GetWoundedTimes(ulong id);
    private uint? API_GetCraftedItems(ulong id);
    private uint? API_GetRepairedItems(ulong id);
    private uint? API_GetLiftUsages(ulong id);
    private uint? API_GetWheelSpins(ulong id);
    private uint? API_GetHammerHits(ulong id);
    private uint? API_GetExplosivesThrown(ulong id);
    private uint? API_GetWeaponReloads(ulong id);
    private uint? API_GetRocketsLaunched(ulong id);
    private uint? API_GetSecondsPlayed(ulong id);
    private List<string> API_GetNames(ulong id);
    private List<string> API_GetIPs(ulong id);
    private List<uint> API_GetTimeStamps(ulong id);
    private Dictionary<string, uint> API_GetCollectiblePickups(ulong id);
    private Dictionary<string, uint> API_GetPlantPickups(ulong id);
    private Dictionary<string, uint> API_GetGathered(ulong id);
    private uint? API_GetCollectiblePickups(ulong id, string shortname);
    private uint? API_GetPlantPickups(ulong id, string shortname);
    private uint? API_GetGathered(ulong id, string shortname);
    [Conditional("DEBUG")]
    private static void PrintDebug(string message);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Debug")]
    public bool Debug { get; set; }
    [JsonProperty(PropertyName = "Inactive Entry Lifetime")]
    public uint Lifetime { get; set; }
    [JsonProperty(PropertyName = "Collect Joins")]
    public bool CollectJoins { get; set; }
    [JsonProperty(PropertyName = "Collect Leaves")]
    public bool CollectLeaves { get; set; }
    [JsonProperty(PropertyName = "Collect Kills")]
    public bool CollectKills { get; set; }
    [JsonProperty(PropertyName = "Collect Deaths")]
    public bool CollectDeaths { get; set; }
    [JsonProperty(PropertyName = "Collect Suicides")]
    public bool CollectSuicides { get; set; }
    [JsonProperty(PropertyName = "Collect Shots")]
    public bool CollectShots { get; set; }
    [JsonProperty(PropertyName = "Collect Headshots")]
    public bool CollectHeadshots { get; set; }
    [JsonProperty(PropertyName = "Collect Experiments")]
    public bool CollectExperiments { get; set; }
    [JsonProperty(PropertyName = "Collect Recoveries")]
    public bool CollectRecoveries { get; set; }
    [JsonProperty(PropertyName = "Collect Voice Bytes")]
    public bool CollectVoiceBytes { get; set; }
    [JsonProperty(PropertyName = "Collect Wounded Times")]
    public bool CollectWoundedTimes { get; set; }
    [JsonProperty(PropertyName = "Collect Crafted Items")]
    public bool CollectCraftedItems { get; set; }
    [JsonProperty(PropertyName = "Collect Repaired Items")]
    public bool CollectRepairedItems { get; set; }
    [JsonProperty(PropertyName = "Collect Lift Usages")]
    public bool CollectLiftUsages { get; set; }
    [JsonProperty(PropertyName = "Collect Wheel Spins")]
    public bool CollectWheelSpins { get; set; }
    [JsonProperty(PropertyName = "Collect Hammer Hits")]
    public bool CollectHammerHits { get; set; }
    [JsonProperty(PropertyName = "Collect Explosives Thrown")]
    public bool CollectExplosivesThrown { get; set; }
    [JsonProperty(PropertyName = "Collect Weapon Reloads")]
    public bool CollectWeaponReloads { get; set; }
    [JsonProperty(PropertyName = "Collect Rockets Launched")]
    public bool CollectRocketsLaunched { get; set; }
    [JsonProperty(PropertyName = "Collect Collectible Pickups")]
    public bool CollectCollectiblePickups { get; set; }
    [JsonProperty(PropertyName = "Collect Plant Pickups")]
    public bool CollectPlantPickups { get; set; }
    [JsonProperty(PropertyName = "Collect Gathered")]
    public bool CollectGathered { get; set; }
}

private class PluginData
{
    public Dictionary<ulong, PlayerStats> Statistics;
}

private class PlayerStats
{
    public uint LastUpdate;
    public uint Joins;
    public uint Leaves;
    public uint Kills;
    public uint Deaths;
    public uint Suicides;
    public uint Shots;
    public uint Headshots;
    public uint Experiments;
    public uint Recoveries;
    public uint VoiceBytes;
    public uint WoundedTimes;
    public uint CraftedItems;
    public uint RepairedItems;
    public uint LiftUsages;
    public uint WheelSpins;
    public uint HammerHits;
    public uint ExplosivesThrown;
    public uint WeaponReloads;
    public uint RocketsLaunched;
    public uint SecondsPlayed;
    public List<string> Names;
    public List<string> IPs;
    public List<uint> TimeStamps;
    public Dictionary<string, uint> CollectiblePickups;
    public Dictionary<string, uint> PlantPickups;
    public Dictionary<string, uint> Gathered;
    public PlayerStats();
    internal PlayerStats(ulong id);
    public void Update();
    public static PlayerStats Find(ulong id);
}


```

---

## SteamChecks by shady14u - Checks connecting users for configurable criteria of the Steam WebAPI and kicks if not fulfilled

```csharp
using Newtonsoft.Json;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Steam Checks", "Shady14u", "5.0.8")]
[Description("Kick players depending on information on their Steam profile")]
public partial class SteamChecks : CovalencePlugin
{
    private void CheckPlayer(string steamId, Action<bool, string> callback);
     void CheckPlayerGameTime(string steamId, Action<bool, string> callback);
    private void DoChecks(IPlayer player);
    private const string apiURL;
    private readonly Regex _steamAvatarRegex;
    private const string skipPermission;
    private const int webTimeout;
    private string additionalKickMessage;
    private string apiKey;
    private uint appId;
    private bool broadcastKick;
    private bool cacheDeniedPlayers;
    private bool cachePassedPlayers;
    private List<string> discordRolesToMention;
    private string discordWebHookUrl;
    private HashSet<string> failedList;
    private bool forceHoursPlayedKick;
    private bool checkPlayersOnRespawn;
    private bool kickCommunityBan;
    private bool kickNoProfile;
    private bool kickLimitedAccount;
    private bool kickPrivateProfile;
    private bool kickTradeBan;
    private bool logInsteadofKick;
    private long maxAccountCreationTime;
    private int maxGameBans;
    private int maxRustHoursPlayed;
    private int maxVACBans;
    private int minAllGamesHoursPlayed;
    private int minDaysSinceLastBan;
    private int minGameCount;
    private int minOtherGamesPlayed;
    private int minRustHoursPlayed;
    private int minSteamLevel;
    private HashSet<string> passedList;
    protected override void LoadDefaultConfig();
    private void InitializeConfig();
    private class WebHookAuthor
    {
        [JsonProperty(PropertyName = "icon_url")]
        public string AuthorIconUrl;
        [JsonProperty(PropertyName = "url")]
        public string AuthorUrl;
        [JsonProperty(PropertyName = "name")]
        public string Name;
    }

    private class WebHookContentBody
    {
        [JsonProperty(PropertyName = "content")]
        public string Content;
    }

    private class WebHookEmbed
    {
        [JsonProperty(PropertyName = "author")]
        public WebHookAuthor Author;
        [JsonProperty(PropertyName = "color")]
        public int Color;
        [JsonProperty(PropertyName = "description")]
        public string Description;
        [JsonProperty(PropertyName = "fields")]
        public List<WebHookField> Fields;
        [JsonProperty(PropertyName = "footer")]
        public WebHookFooter Footer;
        [JsonProperty(PropertyName = "image")]
        public WebHookImage Image;
        [JsonProperty(PropertyName = "thumbnail")]
        public WebHookThumbnail Thumbnail;
        [JsonProperty(PropertyName = "title")]
        public string Title;
        [JsonProperty(PropertyName = "type")]
        public string Type;
    }

    private class WebHookEmbedBody
    {
        [JsonProperty(PropertyName = "embeds")]
        public WebHookEmbed[] Embeds;
    }

    private class WebHookField
    {
        [JsonProperty(PropertyName = "inline")]
        public bool Inline;
        [JsonProperty(PropertyName = "name")]
        public string Name;
        [JsonProperty(PropertyName = "value")]
        public string Value;
    }

    private class WebHookFooter
    {
        [JsonProperty(PropertyName = "icon_url")]
        public string IconUrl;
        [JsonProperty(PropertyName = "proxy_icon_url")]
        public string ProxyIconUrl;
        [JsonProperty(PropertyName = "text")]
        public string Text;
    }

    private class WebHookImage
    {
        [JsonProperty(PropertyName = "height")]
        public int? Height;
        [JsonProperty(PropertyName = "proxy_url")]
        public string ProxyUrl;
        [JsonProperty(PropertyName = "url")]
        public string Url;
        [JsonProperty(PropertyName = "width")]
        public int? Width;
    }

    private class WebHookThumbnail
    {
        [JsonProperty(PropertyName = "height")]
        public int? Height;
        [JsonProperty(PropertyName = "proxy_url")]
        public string ProxyUrl;
        [JsonProperty(PropertyName = "url")]
        public string Url;
        [JsonProperty(PropertyName = "width")]
        public int? Width;
    }

    private class GameTimeInformation
    {
        public GameTimeInformation(int gamesCount, int playtimeRust, int playtimeAll);
        public int GamesCount { get; set; }
        public int PlaytimeAll { get; set; }
        public int PlaytimeRust { get; set; }
        public override string ToString();
    }

    public class PlayerBans
    {
        public bool CommunityBan { get; set; }
        public bool EconomyBan { get; set; }
        public int GameBanCount { get; set; }
        public int LastBan { get; set; }
        public bool VacBan { get; set; }
        public int VacBanCount { get; set; }
        public override string ToString();
    }

    public class PlayerSummary
    {
        public bool LimitedAccount { get; set; }
        public bool NoProfile { get; set; }
        public string Profileurl { get; set; }
        public long Timecreated { get; set; }
        public VisibilityType Visibility { get; set; }
        public override string ToString();
    }

    public class SteamApiResponse
    {
        public SteamResponse Response { get; set; }
    }

    public class SteamLevelApiResponse
    {
        public SteamLevelResponse Response { get; set; }
    }

    public class SteamBadgeApiResponse
    {
        public SteamBadgeResponse response { get; set; }
    }

    public class SteamBanApiResponse
    {
        public List<SteamBanPlayer> players { get; set; }
    }

    public class SteamBadge
    {
        public int badgeid { get; set; }
        public int completion_time { get; set; }
        public int level { get; set; }
        public int scarcity { get; set; }
        public int xp { get; set; }
    }

    public class SteamGame
    {
        public int appid { get; set; }
        public int? playtime_2weeks { get; set; }
        public int playtime_forever { get; set; }
        public int playtime_linux_forever { get; set; }
        public int playtime_mac_forever { get; set; }
        public int playtime_windows_forever { get; set; }
    }

    public class SteamPlayer
    {
        public string avatar { get; set; }
        public string avatarfull { get; set; }
        public string avatarhash { get; set; }
        public string avatarmedium { get; set; }
        public int commentpermission { get; set; }
        public int communityvisibilitystate { get; set; }
        public string gameextrainfo { get; set; }
        public string gameid { get; set; }
        public string gameserverip { get; set; }
        public string gameserversteamid { get; set; }
        public int lastlogoff { get; set; }
        public string loccountrycode { get; set; }
        public string locstatecode { get; set; }
        public string personaname { get; set; }
        public int personastate { get; set; }
        public int personastateflags { get; set; }
        public string primaryclanid { get; set; }
        public int profilestate { get; set; }
        public string profileurl { get; set; }
        public int timecreated { get; set; }
        public string steamId { get; set; }
    }

    public class SteamBanPlayer
    {
        public string SteamId { get; set; }
        public bool CommunityBanned { get; set; }
        public bool VACBanned { get; set; }
        public int NumberOfVACBans { get; set; }
        public int DaysSinceLastBan { get; set; }
        public int NumberOfGameBans { get; set; }
        public string EconomyBan { get; set; }
    }

    public class SteamResponse
    {
        public int? game_count;
        public List<SteamGame> games { get; set; }
        public List<SteamPlayer> players { get; set; }
    }

    public class SteamLevelResponse
    {
        public int player_level { get; set; }
    }

    public class SteamBadgeResponse
    {
        public List<SteamBadge> badges { get; set; }
        public int player_level { get; set; }
        public int player_xp { get; set; }
        public int player_xp_needed_current_level { get; set; }
        public int player_xp_needed_to_level_up { get; set; }
    }

    private void SendDiscordMessage(string steamId, string reasonMessage, WebHookThumbnail thumbnail);
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void OnUserRespawned(IPlayer player);
    private string Lang(string key, string userId);
    protected override void LoadDefaultMessages();
    private const string PluginPrefix;
    [Command("steamcheck"), Permission("steamchecks.use")]
    private void SteamCheckCommand(IPlayer player, string command, string[] args);
    [Command("steamcheck.runtests"), Permission("steamchecks.use")]
    private void SteamCheckTests(IPlayer player, string command, string[] args);
    private void TestResult(IPlayer player, string function, string result);
    private void SteamWebRequest(SteamRequestType steamRequestType, string endpoint, string steamId64, Action<int, string> callback, string additionalArguments);
    private void GetSteamLevel(string steamId64, Action<int, int> callback);
    private void GetPlaytimeInformation(string steamId64, Action<int, GameTimeInformation> callback);
    private void GetSteamPlayerSummaries(string steamId64, Action<int, PlayerSummary> callback);
    private void ApiError(string steamId, string function, int statusCode);
    private void GetSteamBadges(string steamId64, Action<int,string> callback);
    private int ParseBadgeLevel(string response, Badge badgeId);
    private void GetPlayerBans(string steamId64, Action<int, PlayerBans> callback);
}

private class WebHookAuthor
{
    [JsonProperty(PropertyName = "icon_url")]
    public string AuthorIconUrl;
    [JsonProperty(PropertyName = "url")]
    public string AuthorUrl;
    [JsonProperty(PropertyName = "name")]
    public string Name;
}

private class WebHookContentBody
{
    [JsonProperty(PropertyName = "content")]
    public string Content;
}

private class WebHookEmbed
{
    [JsonProperty(PropertyName = "author")]
    public WebHookAuthor Author;
    [JsonProperty(PropertyName = "color")]
    public int Color;
    [JsonProperty(PropertyName = "description")]
    public string Description;
    [JsonProperty(PropertyName = "fields")]
    public List<WebHookField> Fields;
    [JsonProperty(PropertyName = "footer")]
    public WebHookFooter Footer;
    [JsonProperty(PropertyName = "image")]
    public WebHookImage Image;
    [JsonProperty(PropertyName = "thumbnail")]
    public WebHookThumbnail Thumbnail;
    [JsonProperty(PropertyName = "title")]
    public string Title;
    [JsonProperty(PropertyName = "type")]
    public string Type;
}

private class WebHookEmbedBody
{
    [JsonProperty(PropertyName = "embeds")]
    public WebHookEmbed[] Embeds;
}

private class WebHookField
{
    [JsonProperty(PropertyName = "inline")]
    public bool Inline;
    [JsonProperty(PropertyName = "name")]
    public string Name;
    [JsonProperty(PropertyName = "value")]
    public string Value;
}

private class WebHookFooter
{
    [JsonProperty(PropertyName = "icon_url")]
    public string IconUrl;
    [JsonProperty(PropertyName = "proxy_icon_url")]
    public string ProxyIconUrl;
    [JsonProperty(PropertyName = "text")]
    public string Text;
}

private class WebHookImage
{
    [JsonProperty(PropertyName = "height")]
    public int? Height;
    [JsonProperty(PropertyName = "proxy_url")]
    public string ProxyUrl;
    [JsonProperty(PropertyName = "url")]
    public string Url;
    [JsonProperty(PropertyName = "width")]
    public int? Width;
}

private class WebHookThumbnail
{
    [JsonProperty(PropertyName = "height")]
    public int? Height;
    [JsonProperty(PropertyName = "proxy_url")]
    public string ProxyUrl;
    [JsonProperty(PropertyName = "url")]
    public string Url;
    [JsonProperty(PropertyName = "width")]
    public int? Width;
}

private class GameTimeInformation
{
    public GameTimeInformation(int gamesCount, int playtimeRust, int playtimeAll);
    public int GamesCount { get; set; }
    public int PlaytimeAll { get; set; }
    public int PlaytimeRust { get; set; }
    public override string ToString();
}

public class PlayerBans
{
    public bool CommunityBan { get; set; }
    public bool EconomyBan { get; set; }
    public int GameBanCount { get; set; }
    public int LastBan { get; set; }
    public bool VacBan { get; set; }
    public int VacBanCount { get; set; }
    public override string ToString();
}

public class PlayerSummary
{
    public bool LimitedAccount { get; set; }
    public bool NoProfile { get; set; }
    public string Profileurl { get; set; }
    public long Timecreated { get; set; }
    public VisibilityType Visibility { get; set; }
    public override string ToString();
}

public class SteamApiResponse
{
    public SteamResponse Response { get; set; }
}

public class SteamLevelApiResponse
{
    public SteamLevelResponse Response { get; set; }
}

public class SteamBadgeApiResponse
{
    public SteamBadgeResponse response { get; set; }
}

public class SteamBanApiResponse
{
    public List<SteamBanPlayer> players { get; set; }
}

public class SteamBadge
{
    public int badgeid { get; set; }
    public int completion_time { get; set; }
    public int level { get; set; }
    public int scarcity { get; set; }
    public int xp { get; set; }
}

public class SteamGame
{
    public int appid { get; set; }
    public int? playtime_2weeks { get; set; }
    public int playtime_forever { get; set; }
    public int playtime_linux_forever { get; set; }
    public int playtime_mac_forever { get; set; }
    public int playtime_windows_forever { get; set; }
}

public class SteamPlayer
{
    public string avatar { get; set; }
    public string avatarfull { get; set; }
    public string avatarhash { get; set; }
    public string avatarmedium { get; set; }
    public int commentpermission { get; set; }
    public int communityvisibilitystate { get; set; }
    public string gameextrainfo { get; set; }
    public string gameid { get; set; }
    public string gameserverip { get; set; }
    public string gameserversteamid { get; set; }
    public int lastlogoff { get; set; }
    public string loccountrycode { get; set; }
    public string locstatecode { get; set; }
    public string personaname { get; set; }
    public int personastate { get; set; }
    public int personastateflags { get; set; }
    public string primaryclanid { get; set; }
    public int profilestate { get; set; }
    public string profileurl { get; set; }
    public int timecreated { get; set; }
    public string steamId { get; set; }
}

public class SteamBanPlayer
{
    public string SteamId { get; set; }
    public bool CommunityBanned { get; set; }
    public bool VACBanned { get; set; }
    public int NumberOfVACBans { get; set; }
    public int DaysSinceLastBan { get; set; }
    public int NumberOfGameBans { get; set; }
    public string EconomyBan { get; set; }
}

public class SteamResponse
{
    public int? game_count;
    public List<SteamGame> games { get; set; }
    public List<SteamPlayer> players { get; set; }
}

public class SteamLevelResponse
{
    public int player_level { get; set; }
}

public class SteamBadgeResponse
{
    public List<SteamBadge> badges { get; set; }
    public int player_level { get; set; }
    public int player_xp { get; set; }
    public int player_xp_needed_current_level { get; set; }
    public int player_xp_needed_to_level_up { get; set; }
}


```

---

## SteamGroups by MrBlue - Automatically adds members of Steam group(s) to a permissions group

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Steam Groups", "Wulf/lukespragg", "0.4.1")]
[Description("Automatically adds members of Steam group(s) to a permissions group")]
public class SteamGroups : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Group Setup")]
        public List<GroupInfo> GroupSetup { get; set; }
        [JsonProperty(PropertyName = "Update Interval in seconds")]
        public int UpdateInterval { get; set; }
        [JsonProperty(PropertyName = "Log member changes to console")]
        public bool LogToConsole { get; set; }
        [JsonProperty(PropertyName = "Log member changes to file")]
        public bool LogToFile { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private readonly Dictionary<string, string> steamGroups;
    private readonly Dictionary<string, string> groups;
    private readonly HashSet<string> members;
    private readonly Queue<Member> membersQueue;
    private readonly Regex idRegex;
    private readonly Regex pageRegex;
    private readonly Regex pagesRegex;
    private const string permAdmin;
    private bool backoffPoll;
    public class GroupInfo
    {
        public readonly string Oxide;
        public readonly string Steam;
        public GroupInfo(string oxide, string steam);
    }

    private class Member
    {
        public readonly string Id;
        public readonly string Group;
        public Member(string id, string group);
    }

    private void OnServerInitialized();
    private void AddSteamGroup(string steamGroup, string oxideGroup);
    private void ProcessQueuedMembers();
    private void RunQueueAndCleanup();
    private void RemoveOldMembers();
    private void StartTimedChecking();
    private void CheckSteamGroups();
    private void QueueWebRequest(string groupName, string baseUrl, int page);
    private void WebRequestCallback(int code, string response, string baseUrl, string groupName);
    private void ToggleBackoff();
    private void GroupCommand(IPlayer player, string command, string[] args);
    private void MembersCommand(IPlayer player, string command, string[] args);
    private string Lang(string key, string id, object[] args);
    private void AddLocalizedCommand(string key, string command);
    private void Log(string text, string filename);
    private void Message(IPlayer player, string key, object[] args);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Group Setup")]
    public List<GroupInfo> GroupSetup { get; set; }
    [JsonProperty(PropertyName = "Update Interval in seconds")]
    public int UpdateInterval { get; set; }
    [JsonProperty(PropertyName = "Log member changes to console")]
    public bool LogToConsole { get; set; }
    [JsonProperty(PropertyName = "Log member changes to file")]
    public bool LogToFile { get; set; }
}

public class GroupInfo
{
    public readonly string Oxide;
    public readonly string Steam;
    public GroupInfo(string oxide, string steam);
}

private class Member
{
    public readonly string Id;
    public readonly string Group;
    public Member(string id, string group);
}


```

---

## StickyNades by Bazz3l - Stick grenades to players and watch them bitches go boom.

```csharp
using System.Collections.Generic;
using System.Linq;
using Facepunch.Extend;
using Newtonsoft.Json;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Sticky Nades", "Bazz3l", "1.0.7")]
[Description("Stick grenades to players, and watch them go boom.")]
public class StickyNades : RustPlugin
{
    private const string PERM_USE;
    private PluginConfig _config;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        public List<string> AllowedItems;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
        public static PluginConfig DefaultConfig();
    }

    private void OnServerInitialized();
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity, ThrownWeapon item);
    private object OnExplosiveDud(DudTimedExplosive explosive);
    private class StickyExplosiveComponent : MonoBehaviour
    {
        public BaseEntity player;
        private SphereCollider _collider;
        private TimedExplosive _explosive;
        private void Awake();
        private void OnCollisionEnter(Collision collision);
        public void StickExplosive(BaseEntity entity, ContactPoint contact);
        public void UnStickExplosive();
    }

}

private class PluginConfig
{
    public List<string> AllowedItems;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
    public static PluginConfig DefaultConfig();
}

private class StickyExplosiveComponent : MonoBehaviour
{
    public BaseEntity player;
    private SphereCollider _collider;
    private TimedExplosive _explosive;
    private void Awake();
    private void OnCollisionEnter(Collision collision);
    public void StickExplosive(BaseEntity entity, ContactPoint contact);
    public void UnStickExplosive();
}


```

---

## StorageBlocker by  - Allows to setup items that can't be moved to certain containers

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Storage Blocker", "Orange", "1.1.0")]
[Description("Allows to setup items that can't be moved to certain containers")]
public class StorageBlocker : RustPlugin
{
     ItemContainer.CanAcceptResult CanAcceptItem(ItemContainer container, Item item, int targetPos);
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Container (shortname) - list of items (shortname)")]
        public Dictionary<string, List<string>> containers;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private ItemContainer.CanAcceptResult CheckConiner(string container, string item);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Container (shortname) - list of items (shortname)")]
    public Dictionary<string, List<string>> containers;
}


```

---

## StorageMonitorControl by WhiteThunder - Allows storage monitors to be deployed to more container types

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Storage Monitor Control", "WhiteThunder", "1.2.1")]
[Description("Allows storage monitors to be deployed to more container types.")]
internal class StorageMonitorControl : CovalencePlugin
{
    private const string PermissionAll;
    private const string PermissionEntityFormat;
    private const string StorageMonitorBoneName;
    private WaitWhile WaitWhileSaving;
    private HashSet<StorageMonitor> _quarryMonitors;
    private Coroutine _saveRoutine;
    private Configuration _config;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnEntitySpawned(StorageContainer container);
    private void OnEntitySpawned(StorageMonitor storageMonitor);
    private void OnEntityKill(StorageMonitor storageMonitor);
    private static bool NativelySupportsStorageMonitor(BaseEntity entity);
    private static bool IsQuarryStorage(StorageContainer storageContainer, MiningQuarry quarry);
    private static bool IsQuarryStorage(StorageContainer storageContainer);
    private bool ShouldEnableMonitoring(StorageContainer container);
    private bool ContainerOwnerHasPermission(BaseEntity entity, ContainerConfig containerConfig);
    private void ReparentMonitorsToQuarry();
    private void ReparentToClosestQuarryStorage(StorageMonitor storageMonitor, MiningQuarry quarry);
    private void ReparentMonitorsToQuarryContainers();
    private IEnumerator ReparentWhileSaving();
    private class Configuration : BaseConfiguration
    {
        [JsonProperty("Containers")]
        public Dictionary<string, ContainerConfig> Containers;
        public void Init(StorageMonitorControl plugin);
        public ContainerConfig GetContainerConfig(StorageContainer container);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class ContainerConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Require permission")]
        public bool RequirePermission;
        [JsonProperty("RequirePermission")]
        private bool DeprecatedRequirePermission { get; set; }
        [JsonProperty("Position")]
        public Vector3 Position;
        [JsonProperty("Rotation angles", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Vector3 RotationAngles;
        [JsonProperty("RotationAngle")]
        public float DeprecatedRotationAngle { get; set; }
        public string Permission;
        public void Init(StorageMonitorControl plugin, string entityName);
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

private class Configuration : BaseConfiguration
{
    [JsonProperty("Containers")]
    public Dictionary<string, ContainerConfig> Containers;
    public void Init(StorageMonitorControl plugin);
    public ContainerConfig GetContainerConfig(StorageContainer container);
}

[JsonObject(MemberSerialization.OptIn)]
private class ContainerConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Require permission")]
    public bool RequirePermission;
    [JsonProperty("RequirePermission")]
    private bool DeprecatedRequirePermission { get; set; }
    [JsonProperty("Position")]
    public Vector3 Position;
    [JsonProperty("Rotation angles", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Vector3 RotationAngles;
    [JsonProperty("RotationAngle")]
    public float DeprecatedRotationAngle { get; set; }
    public string Permission;
    public void Init(StorageMonitorControl plugin, string entityName);
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

## StorageUpgrade by VisEntities - Expand storage capacity by hitting it with a hammer

```csharp
using System;
using System.Collections.Generic;
using Network;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Storage Upgrade", "VisEntities", "2.1.0")]
[Description("Hit storage with a hammer to boost its capacity.")]
public class StorageUpgrade : RustPlugin
{
    [PluginReference]
    private readonly Plugin Economics;
    private readonly Plugin ServerRewards;
    private static StorageUpgrade _plugin;
    private static Configuration _config;
    private const string FX_UPGRADE;
    private const string FX_UPGRADE_2;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Non Upgradeable Containers")]
        public string[] NonUpgradeableContainers { get; set; }
        [JsonProperty("Profiles")]
        public Dictionary<string, ProfileConfig> Profiles { get; set; }
        [JsonIgnore]
        public HashSet<string> permissions { get; set; }
        public void InitializeProfiles();
        public ProfileConfig GetProfileForPlayer(BasePlayer player);
    }

    private class ProfileConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty("Expand Storage By")]
        public int ExpandStorageBy { get; set; }
        [JsonProperty("Container Shortnames")]
        public string[] ContainerShortnames { get; set; }
        [JsonProperty("Upgrade Cost")]
        public List<CurrencyConfig> UpgradeCost { get; set; }
        [JsonIgnore]
        public string Permission { get; set; }
        public string ConstructPermission(string permissionSuffix);
        public void RegisterPermission(string permission);
        public void InitializeCurrencies();
        public List<CurrencyConfig> GetUpgradeCost();
    }

    private class CurrencyConfig
    {
        [JsonProperty("Name")]
        public string Name { get; set; }
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty("Amount")]
        public int Amount { get; set; }
        [JsonIgnore]
        private bool _itemHasBeenValidated;
        [JsonIgnore]
        private ItemDefinition _itemDefinition;
        [JsonIgnore]
        public IPaymentGateway PaymentGateway;
        [JsonIgnore]
        public PaymentGatewayType PaymentGatewayType;
        [JsonIgnore]
        public bool Valid { get; set; }
        [JsonIgnore]
        public ItemDefinition ItemDefinition { get; set; }
        public void CreatePaymentGateway();
        public bool CanPlayerPay(BasePlayer player);
        public void DeductFromPlayer(BasePlayer player);
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private Configuration GetDefaultConfig();
    private void Init();
    private void Unload();
    private void OnHammerHit(BasePlayer player, HitInfo hitInfo);
    private class ItemPaymentGateway : IPaymentGateway
    {
        private int _itemId;
        public ItemPaymentGateway(int itemId);
        public bool Valid { get; set; }
        public int Get(BasePlayer player);
        public void Give(BasePlayer player, int amount);
        public void Deduct(BasePlayer player, int amount);
    }

    private class CoinPaymentGateway : IPaymentGateway
    {
        private static readonly CoinPaymentGateway _instance;
        private Plugin _economicsPlugin { get; set; }
        public bool Valid { get; set; }
        public static CoinPaymentGateway Instance { get; set; }
        private CoinPaymentGateway();
        public int Get(BasePlayer player);
        public void Give(BasePlayer player, int amount);
        public void Deduct(BasePlayer player, int amount);
    }

    private class PointPaymentGateway : IPaymentGateway
    {
        private static readonly PointPaymentGateway _instance;
        private Plugin _serverRewardsPlugin { get; set; }
        public bool Valid { get; set; }
        public static PointPaymentGateway Instance { get; set; }
        private PointPaymentGateway();
        public int Get(BasePlayer player);
        public void Give(BasePlayer player, int amount);
        public void Deduct(BasePlayer player, int amount);
    }

    private static class PermissionUtil
    {
        public static bool VerifyHasPermission(BasePlayer player, string permissionName);
    }

    private static void RunEffect(string prefab, Vector3 worldPosition, Vector3 worldDirection, Connection effectRecipient, bool sendToAll);
    private static void RunEffect(string prefab, BaseEntity entity, uint boneId, Vector3 localPosition, Vector3 localDirection, Connection effectRecipient, bool sendToAll);
    private static bool VerifyPluginBeingLoaded(Plugin plugin);
    private class Lang
    {
        public const string CannotUpgrade;
        public const string NoPermissionOrCannotUpgrade;
        public const string AlreadyAtMaxCapacity;
        public const string NeedMoreToUpgrade;
        public const string UpgradedFromTo;
    }

    protected override void LoadDefaultMessages();
    private void SendReplyToPlayer(BasePlayer player, string messageKey, object[] args);
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Non Upgradeable Containers")]
    public string[] NonUpgradeableContainers { get; set; }
    [JsonProperty("Profiles")]
    public Dictionary<string, ProfileConfig> Profiles { get; set; }
    [JsonIgnore]
    public HashSet<string> permissions { get; set; }
    public void InitializeProfiles();
    public ProfileConfig GetProfileForPlayer(BasePlayer player);
}

private class ProfileConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty("Expand Storage By")]
    public int ExpandStorageBy { get; set; }
    [JsonProperty("Container Shortnames")]
    public string[] ContainerShortnames { get; set; }
    [JsonProperty("Upgrade Cost")]
    public List<CurrencyConfig> UpgradeCost { get; set; }
    [JsonIgnore]
    public string Permission { get; set; }
    public string ConstructPermission(string permissionSuffix);
    public void RegisterPermission(string permission);
    public void InitializeCurrencies();
    public List<CurrencyConfig> GetUpgradeCost();
}

private class CurrencyConfig
{
    [JsonProperty("Name")]
    public string Name { get; set; }
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty("Amount")]
    public int Amount { get; set; }
    [JsonIgnore]
    private bool _itemHasBeenValidated;
    [JsonIgnore]
    private ItemDefinition _itemDefinition;
    [JsonIgnore]
    public IPaymentGateway PaymentGateway;
    [JsonIgnore]
    public PaymentGatewayType PaymentGatewayType;
    [JsonIgnore]
    public bool Valid { get; set; }
    [JsonIgnore]
    public ItemDefinition ItemDefinition { get; set; }
    public void CreatePaymentGateway();
    public bool CanPlayerPay(BasePlayer player);
    public void DeductFromPlayer(BasePlayer player);
}

private class ItemPaymentGateway : IPaymentGateway
{
    private int _itemId;
    public ItemPaymentGateway(int itemId);
    public bool Valid { get; set; }
    public int Get(BasePlayer player);
    public void Give(BasePlayer player, int amount);
    public void Deduct(BasePlayer player, int amount);
}

private class CoinPaymentGateway : IPaymentGateway
{
    private static readonly CoinPaymentGateway _instance;
    private Plugin _economicsPlugin { get; set; }
    public bool Valid { get; set; }
    public static CoinPaymentGateway Instance { get; set; }
    private CoinPaymentGateway();
    public int Get(BasePlayer player);
    public void Give(BasePlayer player, int amount);
    public void Deduct(BasePlayer player, int amount);
}

private class PointPaymentGateway : IPaymentGateway
{
    private static readonly PointPaymentGateway _instance;
    private Plugin _serverRewardsPlugin { get; set; }
    public bool Valid { get; set; }
    public static PointPaymentGateway Instance { get; set; }
    private PointPaymentGateway();
    public int Get(BasePlayer player);
    public void Give(BasePlayer player, int amount);
    public void Deduct(BasePlayer player, int amount);
}

private static class PermissionUtil
{
    public static bool VerifyHasPermission(BasePlayer player, string permissionName);
}

private class Lang
{
    public const string CannotUpgrade;
    public const string NoPermissionOrCannotUpgrade;
    public const string AlreadyAtMaxCapacity;
    public const string NeedMoreToUpgrade;
    public const string UpgradedFromTo;
}


```

---

## StrikeSystem by LaserHydra - Strike players and time-ban players with a specific amount of strikes

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using System.Linq;
using System;

Oxide.Plugins
[Info("StrikeSystem", "LaserHydra", "2.1.3", ResourceId = 1276)]
[Description("Strike players & time-ban players with a specific amount of strikes")]
 class StrikeSystem : CovalencePlugin
{
    static List<Player> Players;
    [PluginReference("EnhancedBanSystem")]
     Plugin EBS;
     class Player
    {
        public string steamId;
        public string name;
        public string lastStrike;
        public int strikes;
        public int activeStrikes;
        public Player();
        internal Player(IPlayer player);
        internal void Update(IPlayer player);
        internal static Player Get(IPlayer player);
        public override bool Equals(object obj);
        public override int GetHashCode();
    }

     void Loaded();
    new void LoadConfig();
     void LoadMessages();
    protected override void LoadDefaultConfig();
    [Command("strike")]
     void cmdStrike(IPlayer player, string cmd, string[] args);
     void StrikePlayer(IPlayer player, string reason);
     bool ReachedMaxStrikes(IPlayer player);
     void BanPlayer(IPlayer player, string reason);
     IPlayer GetPlayer(string searchedPlayer, IPlayer player);
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     void LoadData(T data, string filename);
     void SaveData(T data, string filename);
     string GetMsg(string key, object userID);
     void RegisterPerm(string[] permArray);
     bool HasPerm(object uid, string[] permArray);
     string PermissionPrefix { get; set; }
}

 class Player
{
    public string steamId;
    public string name;
    public string lastStrike;
    public int strikes;
    public int activeStrikes;
    public Player();
    internal Player(IPlayer player);
    internal void Update(IPlayer player);
    internal static Player Get(IPlayer player);
    public override bool Equals(object obj);
    public override int GetHashCode();
}


```

---

## StructureGrades by MrBlue - Limits which structure grades players can build

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("StructureGrades", "Wulf/lukespragg", "1.0.0", ResourceId = 1511)]
[Description("Limits which structure grades players can build")]
public class StructureGrades : RustPlugin
{
     void Init();
    protected override void LoadDefaultConfig();
     object OnStructureUpgrade(BuildingBlock block, BasePlayer player, BuildingGrade.Enum gradeEnum);
     T GetConfig(string name, T defaultValue);
     string Lang(string key, string id, object[] args);
     bool IsAllowed(string id, string perm);
}


```

---

## StructureRefund by MrBlue - Refunds previous build materials when demolishing and/or upgrading

```csharp
using System;

Oxide.Plugins
[Info("StructureRefund", "Wulf/lukespragg", "1.3.0", ResourceId = 1692)]
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

## SubmarineStorage by yetzt - Adds storage containers to submarines

```csharp
using UnityEngine;
using System.Linq;

Oxide.Plugins
[Info("Submarine Storage", "yetzt", "0.0.4")]
[Description("Adds Storage Boxes to Submarines")]
public class SubmarineStorage : RustPlugin
{
    private string prefab;
    private void OnEntitySpawned(BaseSubmarine entity);
    private void OnEntityDeath(BaseSubmarine entity, HitInfo info);
    private void OnEntityKill(BaseSubmarine entity);
}


```

---

## SuicideBomber by Calytic - Go out with a bang

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info ("SuicideBomber", "Calytic", "0.0.31")]
 class SuicideBomber : RustPlugin
{
     float damage;
     float radius;
     int explosives;
     int c4;
     bool flare;
     bool scream;
     Dictionary<string, string> messages;
     List<string> texts;
     void OnServerInitialized();
     void LoadData();
    protected void ReloadConfig();
    protected override void LoadDefaultConfig();
    private T GetConfig(string name, T defaultValue);
     void OnPlayerInput(BasePlayer player, InputState input);
}


```

---

## SuicideKill by Ankawi - Suicide chat and admin commands to kill a player

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core;

Oxide.Plugins
[Info("SuicideKill", "Ankawi", "1.0.1")]
[Description("Allows you to suicide, kill, or hurt players through chat and/or console commands")]
 class SuicideKill : CovalencePlugin
{
    private const string killPerm;
    private const string hurtPerm;
    private const string suicidePerm;
    private void Init();
    protected override void LoadDefaultMessages();
    [Command("kill")]
    private void KillCommand(IPlayer player, string command, string[] args);
    [Command("suicide")]
    private void SuicideCommand(IPlayer player, string command, string[] args);
    [Command("hurt")]
    private void DamageCommand(IPlayer player, string command, string[] args);
    private string GetMsg(string key, string id, object[] args);
}


```

---

## SuicideModifier by Ryan - Add, change, or remove the default suicide cooldown

```csharp
using UnityEngine;

Oxide.Plugins
[Info("Suicide Modifier", "Ryan", "1.0.3")]
[Description("Adds the ability to change the suicide cooldown or remove it completely.")]
 class SuicideModifier : RustPlugin
{
     string permissionName;
     int defaultCooldown;
     void Init();
    protected override void LoadDefaultConfig();
    private int getCooldown();
     void OnPlayerRespawn(BasePlayer player);
}


```

---

## SuicideVest by birthdates - Allows players to have a suicide vest that blows up

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Suicide Vest", "birthdates", "1.1.1")]
[Description("Allows players to have a suicide vest that blows up")]
public class SuicideVest : RustPlugin
{
    private readonly List<BasePlayer> _armed;
    private readonly Dictionary<BasePlayer, long> _cooldowns;
    private readonly Dictionary<BasePlayer, Timer> _timers;
    private Data _data;
    private const string PermissionGive;
    private const string PermissionUse;
    private void Init();
    private void OnServerInitialized();
    private static bool IsInvalidPrefab(string prefab);
    private void SaveData();
    private void OnPlayerInput(BasePlayer player, InputState input);
    private void AddCooldown(BasePlayer player);
    private static Vector3 GetBody(BasePlayer player);
    private void OnPlayerDie(BasePlayer player);
    private object CanMoveItem(Item item);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private static HashSet<Item> GetBoxItems();
    private static IEnumerable<Item> GetContainerItems(ItemContainer container);
    private IEnumerable<Item> GetEntityItems();
    private static IEnumerable<Item> GetPlayerItems();
    private void Arm(BasePlayer player);
    private void Explode(BasePlayer player);
    private void Unarm(BasePlayer player);
    private bool HasVest(BasePlayer player);
    private Item GetVest(BasePlayer player);
    [ConsoleCommand("givevest")]
    private void ConsoleCmd(ConsoleSystem.Arg arg);
    private void SetupItems();
    private void Unload();
    private void OnServerSave();
    private void Cleanup();
    private object OnItemAction(Item item, string action, BasePlayer player);
    private void ChatCmd(BasePlayer player, string command, string[] args);
    private ConfigFile _config;
    protected override void LoadDefaultMessages();
    private class Data
    {
        public List<uint> Vests { get; set; }
    }

    public class ConfigFile
    {
        [JsonProperty("Vest armed sound (prefab)")]
        public string ArmedSoundPrefab;
        [JsonProperty("Bleeding Damage")]
        public float BleedingAfterDamage;
        [JsonProperty("Count down after armed (seconds)")]
        public int Delay;
        [JsonProperty("Explode when a user dies with an armed vest?")]
        public bool ExplodeOnDeath;
        [JsonProperty("Explode when a user shoots the vest?")]
        public bool ExplodeWhenShot;
        [JsonProperty("Explosion Damage")]
        public float ExplosionDamage;
        [JsonProperty("Effect prefabs")]
        public List<string> ExplosionPrefab;
        [JsonProperty("Explosion Radius")]
        public float ExplosionRadius;
        [JsonProperty("Vest Item (Shortname)")]
        public string Item;
        [JsonProperty("Vest Name")]
        public string Name;
        [JsonProperty("Vest Skin ID")]
        public ulong SkinID;
        [JsonProperty("Ability to unarm")]
        public bool Unarm;
        [JsonProperty("Unarm Cooldown (seconds)")]
        public long UnarmCooldown;
        [JsonProperty("Vest unarmed sound (prefab)")]
        public string UnarmedSoundPrefab;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Data
{
    public List<uint> Vests { get; set; }
}

public class ConfigFile
{
    [JsonProperty("Vest armed sound (prefab)")]
    public string ArmedSoundPrefab;
    [JsonProperty("Bleeding Damage")]
    public float BleedingAfterDamage;
    [JsonProperty("Count down after armed (seconds)")]
    public int Delay;
    [JsonProperty("Explode when a user dies with an armed vest?")]
    public bool ExplodeOnDeath;
    [JsonProperty("Explode when a user shoots the vest?")]
    public bool ExplodeWhenShot;
    [JsonProperty("Explosion Damage")]
    public float ExplosionDamage;
    [JsonProperty("Effect prefabs")]
    public List<string> ExplosionPrefab;
    [JsonProperty("Explosion Radius")]
    public float ExplosionRadius;
    [JsonProperty("Vest Item (Shortname)")]
    public string Item;
    [JsonProperty("Vest Name")]
    public string Name;
    [JsonProperty("Vest Skin ID")]
    public ulong SkinID;
    [JsonProperty("Ability to unarm")]
    public bool Unarm;
    [JsonProperty("Unarm Cooldown (seconds)")]
    public long UnarmCooldown;
    [JsonProperty("Vest unarmed sound (prefab)")]
    public string UnarmedSoundPrefab;
    public static ConfigFile DefaultConfig();
}


```

---

## SuperCard by Mevent - Super card to open all doors

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Super Card", "Mevent", "1.0.7")]
[Description("Open all doors")]
public class SuperCard : CovalencePlugin
{
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Command")]
        public string Cmd;
        [JsonProperty(PropertyName = "Item settings")]
        public ItemConfig Item;
        [JsonProperty(PropertyName = "Enable spawn?")]
        public bool EnableSpawn;
        [JsonProperty(PropertyName = "Drop Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<DropInfo> Drop;
        [JsonProperty(PropertyName = "Use stacking hooks?")]
        public bool UseStackingHooks;
    }

    public class ItemConfig
    {
        [JsonProperty(PropertyName = "DisplayName")]
        public string DisplayName;
        [JsonProperty(PropertyName = "ShortName")]
        public string ShortName;
        [JsonProperty(PropertyName = "SkinID")]
        public ulong SkinID;
        [JsonProperty(PropertyName = "Enable breaking")]
        public bool EnableBreak;
        [JsonProperty(PropertyName = "Breaking the item (1 - standard)")]
        public float LoseCondition;
        public Item ToItem();
        public bool IsSame(Item item);
    }

    public class DropInfo
    {
        [JsonProperty(PropertyName = "Object prefab name")]
        public string PrefabName;
        [JsonProperty(PropertyName = "Minimum item to drop")]
        public int MinAmount;
        [JsonProperty(PropertyName = "Maximum item to drop")]
        public int MaxAmount;
        [JsonProperty(PropertyName = "Item Drop Chance")]
        public float DropChance;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void Init();
    private object OnCardSwipe(CardReader cardReader, Keycard keyCard, BasePlayer player);
    private void OnLootSpawn(LootContainer container);
    private object CanCombineDroppedItem(DroppedItem droppedItem, DroppedItem targetItem);
    private object CanStackItem(Item item, Item targetItem);
    private void Cmd(IPlayer player, string command, string[] args);
    private const string NotFound;
    private const string Syntax;
    private const string NoPermission;
    protected override void LoadDefaultMessages();
    private void Reply(IPlayer player, string key, object[] obj);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Command")]
    public string Cmd;
    [JsonProperty(PropertyName = "Item settings")]
    public ItemConfig Item;
    [JsonProperty(PropertyName = "Enable spawn?")]
    public bool EnableSpawn;
    [JsonProperty(PropertyName = "Drop Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<DropInfo> Drop;
    [JsonProperty(PropertyName = "Use stacking hooks?")]
    public bool UseStackingHooks;
}

public class ItemConfig
{
    [JsonProperty(PropertyName = "DisplayName")]
    public string DisplayName;
    [JsonProperty(PropertyName = "ShortName")]
    public string ShortName;
    [JsonProperty(PropertyName = "SkinID")]
    public ulong SkinID;
    [JsonProperty(PropertyName = "Enable breaking")]
    public bool EnableBreak;
    [JsonProperty(PropertyName = "Breaking the item (1 - standard)")]
    public float LoseCondition;
    public Item ToItem();
    public bool IsSame(Item item);
}

public class DropInfo
{
    [JsonProperty(PropertyName = "Object prefab name")]
    public string PrefabName;
    [JsonProperty(PropertyName = "Minimum item to drop")]
    public int MinAmount;
    [JsonProperty(PropertyName = "Maximum item to drop")]
    public int MaxAmount;
    [JsonProperty(PropertyName = "Item Drop Chance")]
    public float DropChance;
}


```

---

## SupplyAlert by  - Sound and visual (chat and GUI) alert for all active players when a supply has been dropped

```csharp
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Supply Alert", "Ultra", "1.1.2")]
[Description("Sound and visual alert for dropped supply")]
 class SupplyAlert : RustPlugin
{
     Timer guiAlertTimer;
     CuiPanel alertPanel;
     string alertPanelName;
    [ChatCommand("satest")]
     void SupplyAlertTest(BasePlayer player);
     void OnServerInitialized();
     void OnEntitySpawned(BaseEntity baseEntity);
     void Unload();
     void RunSupplyAlert();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Sound alert enabled")]
        public bool SoundAlertEnabled;
        [JsonProperty(PropertyName = "Sound alert asset")]
        public string SoundAlertAsset;
        [JsonProperty(PropertyName = "Chat alert enabled")]
        public bool ChatAlertEnabled;
        [JsonProperty(PropertyName = "Chat alert text")]
        public string ChatAlertText;
        [JsonProperty(PropertyName = "GUI alert enabled")]
        public bool GUIAlertEnabled;
        [JsonProperty(PropertyName = "GUI alert text")]
        public string GUIAlertText;
        [JsonProperty(PropertyName = "GUI alert duration (seconds)")]
        public float GUIAlertDuration;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void InitCUI();
     void DisplayAlertGUI(BasePlayer player);
     void DestroyGUIAlerts();
     CuiLabel GetLabel(string text, int size, string anchorMin, string anchorMax, TextAnchor align, string color, string font);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Sound alert enabled")]
    public bool SoundAlertEnabled;
    [JsonProperty(PropertyName = "Sound alert asset")]
    public string SoundAlertAsset;
    [JsonProperty(PropertyName = "Chat alert enabled")]
    public bool ChatAlertEnabled;
    [JsonProperty(PropertyName = "Chat alert text")]
    public string ChatAlertText;
    [JsonProperty(PropertyName = "GUI alert enabled")]
    public bool GUIAlertEnabled;
    [JsonProperty(PropertyName = "GUI alert text")]
    public string GUIAlertText;
    [JsonProperty(PropertyName = "GUI alert duration (seconds)")]
    public float GUIAlertDuration;
}


```

---

## SupplySignalAlerts by LaserHydra - Broadcasts a message when someone throws a supply signal

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("Supply Signal Alerts", "LaserHydra", "3.0.1", ResourceId = 933)]
internal class SupplySignalAlerts : RustPlugin
{
    private void Init();
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity);
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
}


```

---

## SurveyBlocker by  - Simple plugin to block damage of structure from survey charges

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("SurveyBlocker", "miRror", "1.0.1")]
 class SurveyBlocker : RustPlugin
{
    private float cooldownTime;
    private uint surveyID;
    private Dictionary<ulong, float> p;
    private void Init();
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
    private void OnPlayerDisconnected(BasePlayer player);
    private string Lang(string key, string userID, object[] args);
    private readonly Dictionary<string, Dictionary<string, string>> messages;
}


```

---

## SurveyGather by MJSU - Spawns configurable entities where a player throws a survey charge

```csharp
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Survey Gather", "MJSU", "1.0.7")]
[Description("Spawns configurable entities where a player throws a survey charge")]
internal class SurveyGather : RustPlugin
{
    [PluginReference]
    private Plugin GameTipAPI;
    private PluginConfig _pluginConfig;
    private const string AccentColor;
    private const string UsePermission;
    private const string AdminPermission;
    private ItemDefinition _surveyCharge;
    private readonly Hash<ulong, DateTime> _cooldown;
    private readonly Hash<NetworkableId, RiserBehavior> _behaviors;
    private float _totalChance;
    private static SurveyGather _ins;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void Unload();
    private void SurveyGatherChatCommand(BasePlayer player, string cmd, string[] args);
    private void HandleAdd(BasePlayer player);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private void OnExplosiveDropped(BasePlayer player, SurveyCharge entity);
    private void OnExplosiveThrown(BasePlayer player, SurveyCharge entity);
    private void HandleSurveyCharge(BasePlayer player, SurveyCharge charge);
    private void SpawnEntity(Vector3 pos);
    private GatherConfig SelectRandomConfig();
    public float GetPermissionValue(BasePlayer player, Hash<string, float> permissions, float defaultValue);
    private readonly RaycastHit[] _results;
    private bool RaycastAny(Ray ray, float distance);
    private T Raycast(Ray ray, float distance);
    private void Chat(BasePlayer player, string key, object[] args);
    private string Lang(string key, BasePlayer player, object[] args);
    private bool HasPermission(BasePlayer player, string perm);
    private class RiserBehavior : FacepunchBehaviour
    {
        private BaseEntity _entity;
        private GatherConfig _config;
        private float _timeTaken;
        private NetworkableId _id;
        private bool _isCompleted;
        private Vector3 _endPosition;
        private void Awake();
        public void StartRise(GatherConfig config);
        private void FixedUpdate();
        public void DoDestroy();
        private void OnDestroy();
    }

    private class PluginConfig
    {
        [DefaultValue("sg")]
        [JsonProperty(PropertyName = "Survey Gather Chat Command")]
        public string ChatCommand { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Allow survey charge researching")]
        public bool AllowResearching { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Allow survey charge crafting")]
        public bool AllowCrafting { get; set; }
        [DefaultValue(true)]
        [JsonProperty(PropertyName = "Enabled notifications")]
        public bool EnableNotifications { get; set; }
        [JsonProperty(PropertyName = "Survey Charge Usage Cooldowns")]
        public Hash<string, float> Cooldowns { get; set; }
        [JsonProperty(PropertyName = "Gather Items")]
        public List<GatherConfig> GatherItems { get; set; }
    }

    private class GatherConfig
    {
        [JsonProperty(PropertyName = "Prefab to spawn")]
        public string Prefab { get; set; }
        [JsonProperty(PropertyName = "Chance to spawn")]
        public float Chance { get; set; }
        [JsonProperty(PropertyName = "Distance to spawn underground")]
        public float Distance { get; set; }
        [JsonProperty(PropertyName = "Min health Percentage")]
        public float MinHealth { get; set; }
        [JsonProperty(PropertyName = "Max health Percentage")]
        public float MaxHealth { get; set; }
        [JsonProperty(PropertyName = "Rise duration (Seconds)")]
        public float Duration { get; set; }
        [JsonProperty(PropertyName = "Save Entity (Persists Across Restarts)")]
        public bool SaveEntity { get; set; }
        [JsonProperty(PropertyName = "Kill Entity In (Seconds)")]
        public float KillEntityDuration { get; set; }
    }

    private class LangKeys
    {
        public const string NoPermission;
        public const string Chat;
        public const string Notification;
        public const string HelpText;
        public const string Add;
        public const string NoEntity;
        public const string Cooldown;
    }

}

private class RiserBehavior : FacepunchBehaviour
{
    private BaseEntity _entity;
    private GatherConfig _config;
    private float _timeTaken;
    private NetworkableId _id;
    private bool _isCompleted;
    private Vector3 _endPosition;
    private void Awake();
    public void StartRise(GatherConfig config);
    private void FixedUpdate();
    public void DoDestroy();
    private void OnDestroy();
}

private class PluginConfig
{
    [DefaultValue("sg")]
    [JsonProperty(PropertyName = "Survey Gather Chat Command")]
    public string ChatCommand { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Allow survey charge researching")]
    public bool AllowResearching { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Allow survey charge crafting")]
    public bool AllowCrafting { get; set; }
    [DefaultValue(true)]
    [JsonProperty(PropertyName = "Enabled notifications")]
    public bool EnableNotifications { get; set; }
    [JsonProperty(PropertyName = "Survey Charge Usage Cooldowns")]
    public Hash<string, float> Cooldowns { get; set; }
    [JsonProperty(PropertyName = "Gather Items")]
    public List<GatherConfig> GatherItems { get; set; }
}

private class GatherConfig
{
    [JsonProperty(PropertyName = "Prefab to spawn")]
    public string Prefab { get; set; }
    [JsonProperty(PropertyName = "Chance to spawn")]
    public float Chance { get; set; }
    [JsonProperty(PropertyName = "Distance to spawn underground")]
    public float Distance { get; set; }
    [JsonProperty(PropertyName = "Min health Percentage")]
    public float MinHealth { get; set; }
    [JsonProperty(PropertyName = "Max health Percentage")]
    public float MaxHealth { get; set; }
    [JsonProperty(PropertyName = "Rise duration (Seconds)")]
    public float Duration { get; set; }
    [JsonProperty(PropertyName = "Save Entity (Persists Across Restarts)")]
    public bool SaveEntity { get; set; }
    [JsonProperty(PropertyName = "Kill Entity In (Seconds)")]
    public float KillEntityDuration { get; set; }
}

private class LangKeys
{
    public const string NoPermission;
    public const string Chat;
    public const string Notification;
    public const string HelpText;
    public const string Add;
    public const string NoEntity;
    public const string Cooldown;
}


```

---

## SurveyInfo by VisEntities - Displays results from Survey Charges.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Survey Info", "Diesel_42o/Arainrr", "1.0.6", ResourceId = 2463)]
[Description("Displays loot from survey charges")]
internal class SurveyInfo : RustPlugin
{
    [PluginReference]
    private Plugin RustTranslationAPI;
    private const string PERMISSION_USE;
    private const string PERMISSION_CHECK;
    private static SurveyInfo _instance;
    private float _quarryWorkPerMinute;
    private float _pumpjackWorkPerMinute;
    private readonly HashSet<ulong> _checkedCraters;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnAnalysisComplete(SurveyCrater crater, BasePlayer player);
    private void OnEntityKill(SurveyCharge surveyCharge);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private class SurveyerComponent : MonoBehaviour
    {
        private BasePlayer _player;
        private Item _heldItem;
        private float _lastCheck;
        private ulong _currentItemID;
        private void Awake();
        private void Update();
        private Vector3 GetSurveyPosition();
        private static bool CanSpawnCrater(Vector3 position);
    }

    private void CmdCraterInfo(BasePlayer player, string command, string[] args);
    private void SendMineralAnalysis(BasePlayer player, List<SurveyItem> surveyItems, bool isOilCrater);
    private string GetItemTranslationByShortName(string language, string itemShortName);
    private string GetItemDisplayName(string language, ItemDefinition itemDefinition);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Chat Command")]
        public string command;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong steamIDIcon;
        [JsonProperty(PropertyName = "Display Names")]
        public Dictionary<string, string> displayNames;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class SurveyerComponent : MonoBehaviour
{
    private BasePlayer _player;
    private Item _heldItem;
    private float _lastCheck;
    private ulong _currentItemID;
    private void Awake();
    private void Update();
    private Vector3 GetSurveyPosition();
    private static bool CanSpawnCrater(Vector3 position);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Chat Command")]
    public string command;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong steamIDIcon;
    [JsonProperty(PropertyName = "Display Names")]
    public Dictionary<string, string> displayNames;
}


```

---

## SurveyMySpot by  - Set a spot to survey, it will log enter/exit of players & npc

```csharp
using Oxide.Game.Rust.Cui;
using UnityEngine;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Globalization;
using Oxide.Core;
using Rust;
using System.Text;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Survey My Spot", "BuzZ[PHOQUE]", "1.0.0")]
[Description("Set a spot to survey, it will log enter/exit of players & npc")]
public class SurveyMySpot : RustPlugin
{
    [PluginReference]
     Plugin ZoneManager;
     bool debug;
     string version;
    public Dictionary<ulong, int> reading;
     class StoredData
    {
        public Dictionary<ulong, Dataz> ulongdata;
        public StoredData();
    }

    private StoredData storedData;
     class Dataz
    {
        public string playerspot;
        public bool playeronly;
        public bool playerchat;
        public int playerenter;
        public int playerexit;
        public int npcenter;
        public int npcexit;
        public List<string> loglist;
        public Dataz();
    }

    private Dataz dataz;
     bool IsPlayerOnSpot(string spotID, BasePlayer player);
     string CheckZoneID(string checkID);
    static string SurveyPanel;
    static string LogPanel;
    private bool ConfigChanged;
     string Prefix;
     string PrefixColor;
     string ChatColor;
     ulong SteamIDIcon;
     bool PlayerOnly;
    const string SurveyPermission;
    private void OnServerInitialized();
     void Loaded();
     void Unload();
     void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private object GetConfig(string menu, string datavalue, object defaultValue);
     void OnEnterZone(string ZoneID, BasePlayer player);
     void OnExitZone(string ZoneID, BasePlayer player);
     void SomethingHappenedOnZone(string ZoneID, BasePlayer player, string reason, string gender);
    [ConsoleCommand("MySurveySpotPlayerOnly")]
    private void MySurveySpotOnly(ConsoleSystem.Arg arg);
    [ConsoleCommand("MySurveySpotChat")]
    private void MySurveySpotChat(ConsoleSystem.Arg arg);
    [ConsoleCommand("MySurveySpotBackToPanel")]
    private void MySurveySpotBackTo(ConsoleSystem.Arg arg);
    [ConsoleCommand("MySurveySpotPurgeMyLog")]
    private void MySurveySpotPurge(ConsoleSystem.Arg arg);
    private void RemoveLog(string spot, ulong playerID);
    private void KeepHundred(string zoneID, ulong spotowner);
    [ConsoleCommand("MySurveySpotZone")]
    private void MySurveySpot(ConsoleSystem.Arg arg);
    [ChatCommand("mysurvey")]
    private void SurveyCui(BasePlayer player, string command, string[] args);
    [ConsoleCommand("SurveyMySpotPage1")]
    private void SurveyMySpotPage1(ConsoleSystem.Arg arg);
    [ConsoleCommand("SurveyMySpotPage2")]
    private void SurveyMySpotPage2(ConsoleSystem.Arg arg);
    [ConsoleCommand("SurveyMySpotPage3")]
    private void SurveyMySpotPage3(ConsoleSystem.Arg arg);
    [ConsoleCommand("SurveyMySpotPage4")]
    private void SurveyMySpotPage4(ConsoleSystem.Arg arg);
    [ConsoleCommand("MySurveySpotLog")]
    private void MySurveyLog(ConsoleSystem.Arg arg);
}

 class StoredData
{
    public Dictionary<ulong, Dataz> ulongdata;
    public StoredData();
}

 class Dataz
{
    public string playerspot;
    public bool playeronly;
    public bool playerchat;
    public int playerenter;
    public int playerexit;
    public int npcenter;
    public int npcexit;
    public List<string> loglist;
    public Dataz();
}


```

---

## SwimDamageToggle by Hockeygel23 - Prevents drowning, cold damage, and cold exposure when swimming

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Rust;

Oxide.Plugins
[Info("Swim Damage Toggle", "Hockeygel23", "0.0.7")]
[Description("It gives a player immunity to drowning, cold damage, and cold exposure when a player is swimming")]
public class SwimDamageToggle : RustPlugin
{
    private const string AdminPermission;
    private const string PlayerPermission;
    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty("No Drowning Damage")]
        public bool DrowningDamage;
        [JsonProperty("No Cold Exposure Damage")]
        public bool ColdExposure;
        [JsonProperty("No Cold Damage")]
        public bool ColdDamage;
    }

    private ConfigData GenerateConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnEntityTakeDamage(BasePlayer player, HitInfo info);
    [ChatCommand("swimdamage")]
    private void cmdToggle(BasePlayer player, string command, string[] args);
}

private class ConfigData
{
    [JsonProperty("No Drowning Damage")]
    public bool DrowningDamage;
    [JsonProperty("No Cold Exposure Damage")]
    public bool ColdExposure;
    [JsonProperty("No Cold Damage")]
    public bool ColdDamage;
}


```

---

## SyncPipes by Joe90 - Create pipes between boxes, quarries, furnaces, etc. to move items between them

```csharp
using Rust;
using System;
using Oxide.Core;
using UnityEngine;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System.Collections;
using System.Diagnostics;
using Oxide.Game.Rust.Cui;
using Random = System.Random;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using System.Runtime.CompilerServices;

Oxide.Plugins
[Info("Sync Pipes", "Joe 90", "0.9.32")]
[Description("Allows players to transfer items between containers. All pipes from a container are used synchronously to enable advanced sorting and splitting.")]
partial class SyncPipes : RustPlugin
{
    private static SyncPipes Instance;
    private const string ToolCupboardPrefab;
     Plugin FurnaceSplitter;
    [PluginReference]
     Plugin QuickSmelt;
     void Init();
     void Unload();
    partial void ExperimentalUnload();
    static class Commands
    {
        public static void InitializeChat();
        private static void Add(string commandSuffix, string callback);
        public static void Args(PlayerHelper playerHelper, string[] args);
        public static void PlacePipe(PlayerHelper playerHelper);
        public static void Help(PlayerHelper playerHelper);
        public static void Copy(PlayerHelper playerHelper);
        public static void Remove(PlayerHelper playerHelper);
        public static void Stats(PlayerHelper playerHelper);
        public static void OpenMenu(ConsoleSystem.Arg arg);
        public static void CloseMenu(ConsoleSystem.Arg arg);
        public static void Name(PlayerHelper playerHelper, string name);
        public static void ForceCloseMenu(PlayerHelper playerHelper);
        public static void ChangePriority(ConsoleSystem.Arg arg);
        public static void RefreshMenu(ConsoleSystem.Arg arg);
        private static Pipe GetPipe(ConsoleSystem.Arg arg, int index);
        private static bool GetBool(ConsoleSystem.Arg arg, int index);
        private static int? GetInt(ConsoleSystem.Arg arg, int index);
        public static void SetPipeState(ConsoleSystem.Arg arg);
        public static void SetPipeAutoStart(ConsoleSystem.Arg arg);
        public static void SwapPipeDirection(ConsoleSystem.Arg arg);
        public static void SetPipeMultiStack(ConsoleSystem.Arg arg);
        public static void SetPipeFurnaceStackEnabled(ConsoleSystem.Arg arg);
        public static void SetPipeFurnaceStackCount(ConsoleSystem.Arg arg);
        public static void OpenPipeFilter(ConsoleSystem.Arg arg);
        public static void MenuHelp(ConsoleSystem.Arg arg);
        public static void FlushPlayerPermissions(ConsoleSystem.Arg arg);
    }

     void CommandArgs(IPlayer player, string command, string[] args);
     void CommandHelp(IPlayer player, string command, string[] args);
     void CommandCopy(IPlayer player, string command, string[] args);
     void CommandRemove(IPlayer player, string command, string[] args);
     void CommandStats(IPlayer player, string command, string[] args);
     void CommandName(IPlayer player, string command, string[] args);
    [SyncPipesConsoleCommand("create")]
     void StartPipe(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("openmenu")]
     void OpenMenu(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("closemenu")]
     void CloseMenu(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("forceclosemenu")]
     void ForceCloseMenu(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("refreshmenu")]
     void RefreshMenu(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("changepriority")]
     void ChangePriority(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("setpipestate")]
     void SetPipeState(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("setpipeautostart")]
     void SetPipeAutoStart(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("swappipedirection")]
     void SwapPipeDirection(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("setpipemultistack")]
     void SetPipeMultiStack(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("setpipefurnacestackenabled")]
     void SetPipeFurnaceStackEnabled(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("setpipefurnacestackcount")]
     void SetPipeFurnaceStackCount(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("openpipefilter")]
     void OpenPipeFilter(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("menuhelp")]
     void MenuHelp(ConsoleSystem.Arg arg);
    [SyncPipesConsoleCommand("flushperms")]
     void FlushPlayerPermissions(ConsoleSystem.Arg arg);
    public class SyncPipesConsoleCommandAttribute : ConsoleCommandAttribute
    {
        public SyncPipesConsoleCommandAttribute(string command);
    }

     class SyncPipesConfig
    {
        private static readonly SyncPipesConfig Default;
        public static SyncPipesConfig New();
        [JsonProperty("LogLevel")]
        public int LogLevel { get; set; }
        [JsonProperty("filterSizes")]
        public List<int> FilterSizes { get; set; }
        [JsonProperty("flowRates")]
        public List<int> FlowRates { get; set; }
        [JsonProperty("maxPipeDist")]
        public float MaximumPipeDistance { get; set; }
        [JsonProperty("minPipeDist")]
        public float MinimumPipeDistance { get; set; }
        [JsonProperty("noDecay")]
        public bool NoDecay { get; set; }
        [JsonProperty("commandPrefix")]
        public string CommandPrefix { get; set; }
        [JsonProperty("hotKey")]
        public string HotKey { get; set; }
        [JsonProperty("updateRate")]
        public int UpdateRate { get; set; }
        [JsonProperty("xmasLights")]
        public bool AttachXmasLights { get; set; }
        [JsonProperty("permLevels", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Dictionary<string, PermissionLevel> PermissionLevels { get; set; }
        [JsonProperty("salvageDestroy")]
        public bool DestroyWithSalvage { get; set; }
        [JsonProperty("experimental", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ExperimentalConfig Experimental { get; set; }
        [JsonProperty("blacklistTC")]
        public bool BlacklistTC { get; set; }
        [JsonProperty("useQuickSmelt")]
        public bool UseQuickSmelt { get; set; }
        public class PermissionLevel
        {
            [JsonProperty("upgradeLimit")]
            public int MaximumGrade { get; set; }
            [JsonProperty("pipeLimit")]
            public int MaximumPipes { get; set; }
            public static readonly PermissionLevel Default;
        }

        private string[] Validate();
        public static SyncPipesConfig Load();
        public void RegisterPermissions();
    }

     class ExperimentalConfig
    {
        [JsonProperty("barrelPipe")]
        public bool BarrelPipe { get; set; }
        [JsonProperty("permEntity")]
        public bool PermanentEntities { get; set; }
    }

    protected override void LoadDefaultConfig();
    static SyncPipesConfig InstanceConfig { get; set; }
    private SyncPipesConfig _config;
    private bool IsNoDecayEnabled { get; set; }
    public const string FUEL_STORAGE_PREFAB;
    public const string QUARRY_OUTPUT_PREFAB;
    public const string PUMPJACK_OUTPUT_PREFAB;
    static class ContainerHelper
    {
        public static bool IsBlacklisted(BaseEntity container);
        public static StorageContainer Find(uint id);
        public static ContainerType GetEntityType(BaseEntity container);
        public static bool InMonument(BaseEntity entity);
        private static void LogFindError(uint parentId, BaseEntity entity, ContainerType containerType, List<BaseEntity> children);
        public static BaseEntity Find(uint parentId, ContainerType containerType);
        public static BaseEntity Find(BaseEntity entity, ContainerType containerType);
        public static StorageContainer Find(BaseEntity parent);
        public static string GetShortPrefabName(ContainerType containerType);
        public static bool IsComplexStorage(ContainerType containerType);
        public static bool CanAutoStart(ContainerType containerType);
    }

    public class ContainerManager : MonoBehaviour
    {
        internal static readonly Dictionary<uint, ContainerManager> ManagedContainerLookup;
        public static readonly List<ContainerManager> ManagedContainers;
        private readonly List<Pipe> _attachedPipes;
        public bool CombineStacks { get; set; }
        public StorageContainer Container { get; set; }
        public uint ContainerId;
        private float _cumulativeDeltaTime;
        private bool _destroyed;
        public string DisplayName;
        public bool HasAnyPipes { get; set; }
        public static void Cleanup();
        private void Kill(bool cleanup);
        public static ContainerManager Attach(BaseEntity entity, StorageContainer container, Pipe pipe);
        public static void Detach(uint containerId, Pipe pipe);
        private void Update();
        private List<Item> ItemList { get; set; }
        public ContainerType ContainerType { get; set; }
        private MovableType CanPuItem(Item item);
        private static bool CanCook(Item item);
        private bool CorrectOven(Item item);
        private bool? OvenFuel(Item item);
        private bool CanTakeItem(Item item);
        private void MoveCombineStacks(Dictionary<int, List<Pipe>> pipeGroup);
        private void MoveIndividualStacks(Dictionary<int, List<Pipe>> pipeGroup);
        private int GetAmountToMove(int itemId, int itemQuantity, int pipesLeft, Pipe pipe, int maxStackable, bool multiStack);
        private Item GetItemToMove(Item item, Pipe pipe);
        private static int GetMinStackSize(List<Item> itemStacks);
    }

    internal abstract class CuiMenuBase : CuiBase, IDisposable
    {
        protected PlayerHelper PlayerHelper { get; set; }
        protected CuiMenuBase(PlayerHelper playerHelper);
        protected CuiElementContainer Container { get; set; }
        protected string PrimaryPanel { get; set; }
        protected bool Visible { get; set; }
        public virtual void Show();
        public virtual void Close();
        public virtual void Refresh();
        protected string MakeCommand(string commandName, object[] args);
        protected string AddPanel(string parent, string min, string max, string colour, bool cursorEnabled);
        protected string AddPanel(string parent, string min, string max, string colour, bool cursorEnabled, CuiPanel panel);
        protected void AddLabel(string parent, string text, int fontSize, TextAnchor alignment, string min, string max, string colour);
        protected void AddLabelWithOutline(string parent, string text, int fontSize, TextAnchor alignment, string min, string max, string textColour, string outlineColour, string distance, bool useGraphicAlpha);
        protected void AddImage(string parent, string min, string max, string imageUrl, string colour);
        protected string AddButton(string parent, string command, string text, string min, string max, string colour);
        public virtual void Dispose();
    }

    internal class CuiBase
    {
        protected CuiButton MakeButton(string command, string text, string min, string max, string colour);
        protected CuiLabel MakeLabel(string text, int fontSize, TextAnchor alignment, string min, string max, string colour);
        protected CuiElement MakeLabelWithOutline(string text, int fontSize, TextAnchor alignment, string min, string max, string textColour, string outlineColour, string distance, bool useGraphicAlpha);
        protected CuiPanel MakePanel(string min, string max, string colour, bool cursorEnabled);
        protected CuiElement MakePanelWithOutline(string min, string max, string colour, bool cursorEnabled, string outlineColour, string distance, bool useGraphicAlpha);
    }

     bool? CanPickupEntity(BasePlayer player, BaseEntity entity);
     void OnEntityKill(BaseNetworkable entity);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
     void OnHammerHit(BasePlayer player, HitInfo hit);
    private bool IsPipe(BaseEntity entity, bool checkRunning);
    private bool IsManagedContainer(BaseEntity entity);
    public class PipeFilter
    {
        public List<Item> Items { get; set; }
        private readonly List<BasePlayer> _playersInFilter;
        public void Kill();
        private void KillFilter();
        private void ForceClosePlayers();
        private void ForceClosePlayer(BasePlayer player);
        public void Closing(BasePlayer player);
        public PipeFilter(List<int> filterItems, int capacity, Pipe pipe);
        private bool CanAcceptItem(Item item, int position);
        private readonly List<Item> _addItem;
        public void Upgrade(int newCapacity);
        public void Open(PlayerHelper playerHelper);
        private ItemContainer _filterContainer;
        private readonly Pipe _pipe;
        private bool Active { get; set; }
    }

    private void OnPlayerLootEnd(PlayerLoot playerLoot);
    private void OnItemRemovedFromContainer(ItemContainer container, Item item);
    private bool? CanStackItem(Item item, Item targetItem);
    static class Handlers
    {
        public static bool HandleNamingContainerHit(PlayerHelper playerHelper, BaseEntity entity);
        public static bool HandlePlacementContainerHit(PlayerHelper playerHelper, BaseEntity entity);
        public static bool HandlePipeCopy(PlayerHelper playerHelper, BaseEntity entity);
        public static bool HandlePipeRemove(PlayerHelper playerHelper, BaseEntity entity);
        public static bool HandlePipeMenu(PlayerHelper playerHelper, BaseEntity entity);
        public static bool HandleContainerManagerHit(PlayerHelper playerHelper, BaseEntity entity);
        public static bool? HandlePipeUpgrade(BaseCombatEntity entity, PlayerHelper playerHelper, BuildingGrade.Enum grade);
        public static bool HandleAttachTCContainerHut(PlayerHelper playerHelper, BaseEntity hitHitEntity);
    }

     class Logger
    {
        public static readonly Logger PipeLoader;
        public static readonly Logger ContainerLoader;
        public static readonly Logger PipeFactoryLoader;
        public static readonly Logger FindErrors;
        public static readonly Logger Runtime;
        public Logger(string filename, LogLevels defaultLogLevel);
        private readonly string _filename;
        private readonly LogLevels _defaultLogLevel;
        public void Log(string format, object[] args);
        public void Log(LogLevels logLevel, string format, object[] args);
        public void LogSection(string section, string format, object[] args);
        public void LogSection(LogLevels logLevel, string section, string format, object[] args);
        public void LogException(Exception e, string section);
    }

    public class PipeMenu
    {
        private string MakeCommand(string commandName, object[] args);
        private const string OnColour;
        private const string OnTextColour;
        private const string OffColour;
        private const string OffTextColour;
        private const string LabelColour;
        private static readonly Vector2 Size;
        private readonly CuiElementContainer _foregroundElementContainer;
        private readonly CuiElementContainer _backgroundElementContainer;
        private readonly CuiElementContainer _helpElementContainer;
        private string _foregroundPanel;
        private string _helpPanel;
        private readonly string _backgroundPanel;
        private readonly Pipe _pipe;
        private readonly PlayerHelper _playerHelper;
        private bool _helpOpen;
        public PipeMenu(Pipe pipe, PlayerHelper playerHelper);
        private void CreateForeground();
        private void PipeVisualPanel(string panel);
        private void StartablePanel(string panel);
        private void InfoPanel();
        private void ControlPanel(string panel);
        public void Open();
        public void Refresh();
        public void Close(PlayerHelper playerHelper);
         string AddPanel(string parent, string min, string max, string colour, bool cursorEnabled, CuiElementContainer elementContainer);
         void AddLabel(string parent, string text, int fontSize, TextAnchor alignment, string min, string max, string colour, CuiElementContainer elementContainer);
         void AddImage(string parent, string min, string max, string imageUrl, string colour, CuiElementContainer elementContainer);
         void AddButton(string parent, string command, string text, string min, string max, string colour, CuiElementContainer elementContainer);
        public void ToggleHelp();
        private void CreateHelpPanel();
    }

    private static Dictionary<Enum, bool> _chatCommands;
    private static Dictionary<Enum, bool> _bindingCommands;
    private static Dictionary<Enum, MessageType> _messageTypes;
    static class LocalizationHelpers
    {
        internal static Dictionary<string, string> FallBack { get; set; }
        public static string Get(Enum key, BasePlayer player, object[] args);
        public static MessageType GetMessageType(Enum key);
    }

    protected override void LoadDefaultMessages();
    static class OverlayText
    {
        private static Dictionary<MessageType, string> ColourIndex;
        public static void Show(BasePlayer player, string text, MessageType messageType, string callerName);
        public static void Show(BasePlayer player, string text, string subText, MessageType messageType, string callerName);
        public static void Show(BasePlayer player, string text, string subText, string textColour, string callerName);
        static CuiElement LabelWithOutline(CuiLabel label, string parent, string textColour, string distance, bool useAlpha, string name);
        public static void Hide(BasePlayer player, float delay, string callerName);
    }

    public class Pipe
    {
        private static readonly Random RandomGenerator;
        internal PipeFactoryBase Factory { get; set; }
        public List<int> InitialFilterItems { get; set; }
        public float InitialHealth { get; set; }
        private PipeFilter _pipeFilter;
        static Pipe();
        public Pipe();
        public Pipe(PipeData data);
        public PipeFilter PipeFilter { get; set; }
        public IEnumerable<int> FilterItems { get; set; }
        public bool IsFurnaceSplitterEnabled { get; set; }
        public int FurnaceSplitterStacks { get; set; }
        public bool IsAutoStart { get; set; }
        public bool IsMultiStack { get; set; }
        public ulong OwnerId { get; set; }
        public string OwnerName { get; set; }
        private List<PlayerHelper> PlayersViewingMenu { get; set; }
        public string DisplayName { get; set; }
        public BuildingGrade.Enum Grade { get; set; }
        public int FilterCapacity { get; set; }
        public int FlowRate { get; set; }
        public float Health { get; set; }
        public bool Repairing { get; set; }
        public uint Id { get; set; }
        public bool InvertFilter { get; set; }
        public PipePriority Priority { get; set; }
        public Status Validity { get; set; }
        public PipeEndContainer Destination { get; set; }
        public PipeEndContainer Source { get; set; }
        public static Dictionary<ulong, Pipe> PipeLookup { get; set; }
        public static List<Pipe> Pipes { get; set; }
        public static Dictionary<uint, Dictionary<uint, bool>> ConnectedContainers { get; set; }
        public bool IsEnabled { get; set; }
        public bool CanAutoStart { get; set; }
        public float Distance { get; set; }
        public Quaternion Rotation { get; set; }
        public BaseEntity PrimarySegment { get; set; }
        public static IEnumerable<PipeData> Save();
        public static void LogLoadError(ulong pipeId, Status status, PipeData pipeData);
        public static void Load(PipeData[] dataToLoad);
        public void Create();
        private Quaternion GetRotation();
        private static bool IsOverlapping(PipeData data);
        internal uint GenerateId();
        public static Pipe Get(BaseEntity entity);
        public static Pipe Get(ulong id);
        public void SetEnabled(bool enabled);
        public bool CanPlayerOpen(PlayerHelper playerHelper);
        public static void TryCreate(PlayerHelper playerHelper);
        public static void Cleanup();
        internal void Validate();
        public void SwapDirection();
        public void OpenMenu(PlayerHelper playerHelper);
        public void CloseMenu(PlayerHelper playerHelper);
        private void RefreshMenu();
        public bool IsAlive();
        public void Kill(bool cleanup);
        private void KillSegments(bool cleanup);
        public void ChangePriority(int priorityChange);
        public void Remove(bool cleanup);
        private static void KillAll();
        public void Upgrade(BuildingGrade.Enum grade);
        public void SetHealth(float health);
        public void SetAutoStart(bool autoStart);
        public void SetMultiStack(bool multiStack);
        public void SetFurnaceStackEnabled(bool enable);
        public void SetFurnaceStackCount(int stackCount);
        public void OpenFilter(PlayerHelper playerHelper);
        public void CopyFrom(Pipe pipe);
    }

    public class PipeData
    {
        private BaseEntity _destination;
        private BaseEntity _source;
        public ContainerType DestinationContainerType;
        public uint DestinationId;
        public int FurnaceSplitterStacks;
        public BuildingGrade.Enum Grade;
        public float Health;
        public bool IsAutoStart;
        public bool IsEnabled;
        public bool IsFurnaceSplitter;
        public bool IsMultiStack;
        public List<int> ItemFilter;
        public ulong OwnerId;
        public string OwnerName;
        public Pipe.PipePriority Priority;
        public ContainerType SourceContainerType;
        public uint SourceId;
        public PipeData();
        public PipeData(Pipe pipe);
        public PipeData(PlayerHelper playerHelper);
        [JsonIgnore]
        public BaseEntity Source { get; set; }
        [JsonIgnore]
        public BaseEntity Destination { get; set; }
    }

    public class PipeEndContainer
    {
        private readonly Pipe _pipe;
        public PipeEndContainer(BaseEntity container, ContainerType containerType, Pipe pipe);
        public void Attach();
        public uint Id { get; set; }
        public BaseEntity Container { get; set; }
        public ContainerManager ContainerManager { get; set; }
        public StorageContainer Storage { get; set; }
        public ContainerType ContainerType { get; set; }
        public Vector3 Position { get; set; }
        public string IconUrl { get; set; }
        public bool CanAutoStart { get; set; }
        public void Start();
        public void Stop();
        public bool HasFuel();
    }

    abstract class PipeSegmentBase : MonoBehaviour
    {
        public Pipe Pipe { get; set; }
        private BaseEntity _parent;
         void Update();
        protected void Init(Pipe pipe, BaseEntity parent);
    }

     class PipeSegment : PipeSegmentBase
    {
        public static void Attach(BaseEntity pipeEntity, Pipe pipe);
    }

     class PipeSegmentLights : PipeSegmentBase
    {
        public static void Attach(BaseEntity pipeEntity, Pipe pipe);
    }

     void OnPlayerDisconnected(BasePlayer player, string reason);
    public partial class PlayerHelper
    {
        private static readonly Dictionary<ulong, Dictionary<ulong, Pipe>> AllPipes;
        public static void AddPipe(Pipe pipe);
        public static void RemovePipe(Pipe pipe);
        private static readonly Dictionary<ulong, PlayerHelper> Players;
        public static PlayerHelper Get(IPlayer iPlayer);
        public static PlayerHelper Get(BasePlayer player);
        private PlayerHelper(BasePlayer player);
        partial void ExperimentalConstructor();
        public readonly BasePlayer Player;
        public UserState State;
        private bool _isUsingBinding;
        public BaseEntity Source;
        public BaseEntity Destination;
        public string NamingName;
        public Pipe CopyFrom;
        public bool IsMenuOpen { get; set; }
        public PipeMenu Menu;
        public PipeFilter PipeFilter;
        public string OverlayContainerId;
        public string ActiveOverlayText;
        public string ActiveOverlaySubText;
        public bool IsAdmin { get; set; }
        public bool IsUser { get; set; }
        public bool CanBuild { get; set; }
        public bool HasBuildPrivilege { get; set; }
        public uint TCAttchBuildingId;
        private IEnumerable<SyncPipesConfig.PermissionLevel> Permissions { get; set; }
        private SyncPipesConfig.PermissionLevel GetPermission(string userPermission);
        private int PermissionLevelMaxPipes { get; set; }
        private int PermissionLevelMaxUpgrade { get; set; }
        public int PipeLimit { get; set; }
        public int MaxUpgrade { get; set; }
        public Dictionary<ulong, Pipe> Pipes { get; set; }
        public bool HasContainerPrivilege(BaseEntity container);
        public bool HasContainerPrivilege(StorageContainer container);
        public bool ConfirmAvailablePipes();
        private void ShowPlacingBindHint();
        public void ShowPlacingOverlay(float delay);
        public void ShowCopyOverlay(float delay);
        public void ShowRemoveOverlay(float delay);
        public void ToggleRemovingPipe();
        public void ToggleCopyingPipe();
        public void TogglePlacingPipe(bool isUsingBinding);
        public void CloseMenu();
        public void PipePlacingComplete();
        public void StartNaming(string name);
        public void ShowNamingOverlay(float delay);
        public void StopNaming();
        public void StartToolCupboardBuildingId();
        public void SetPlayerToolCupboardBuildingId(uint buildingId);
        public void SetToolCupboardBuildingId(DecayEntity toolCupboard);
        public void StopToolCupboardBuildingId();
        public static void Cleanup();
        partial void ExperimentalCleanup();
        public void SendSyncPipesConsoleCommand(string commandName, object[] args);
        public void CloseFilter();
        public static void Remove(BasePlayer player);
        private string GetLocalization(Enum key, object[] args);
        public void ShowOverlay(Overlay message, object[] args);
        public void ShowOverlayWithSubText(Overlay message, Overlay subMessage, object[] args);
        public void ShowPipeStatusOverlay(Pipe.Status status);
        public string GetPipeMenuControlLabel(PipeMenu.ControlLabel label);
        public string GetMenuButton(PipeMenu.Button button);
        public string GetPipeMenuHelpLabel(PipeMenu.HelpLabel label);
        public string GetPipePriorityText(Pipe.PipePriority priority);
        public string GetPipeMenuInfo(PipeMenu.InfoLabel infoLabel);
        public void PrintToChat(Chat chat, object[] args);
        public void PrintToChatWithTitle(Chat chat, object[] args);
        public void PrintToChatWithTitle(string chat, object[] args);
    }

     void OnServerInitialized();
     void OnServerSave();
    public class StorageData
    {
        public StorageData(string shortName, string url, Vector3 offset, bool partialUrl);
        public readonly string Url;
        public readonly string ShortName;
        public readonly bool PartialUrl;
        public readonly Vector3 Offset;
    }

    private static Dictionary<Storage, StorageData> _storageDetails;
    static class StorageHelper
    {
        public static string GetImageUrl(BaseEntity storageEntity, int size);
        public static Vector3 GetOffset(BaseEntity storageEntity);
        private static StorageData GetDetails(BaseEntity storageEntity);
    }

     void OnStructureDemolish(BaseCombatEntity entity, BasePlayer player, bool immediate);
     bool? OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     bool? OnStructureRepair(BaseCombatEntity entity, BasePlayer player);
    private static bool? OnPipeRepair(BaseCombatEntity entity, BasePlayer player, Pipe pipe);
     bool? OnStructureRotate(BaseCombatEntity entity, BasePlayer player);
     bool? OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
    partial class DataStore
    {
        partial class OnePointZero
        {
            public class ContainerManagerDataConverter : JsonConverter
            {
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
                private bool _canWrite;
                private bool _canRead;
                public override bool CanWrite { get; set; }
                public override bool CanRead { get; set; }
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointZero
        {
            public class ContainerManagerData
            {
                public uint ContainerId;
                public bool CombineStacks;
                public string DisplayName;
                public ContainerType ContainerType;
                public ContainerManagerData();
                public ContainerManagerData(ContainerManager containerManager);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointZero
        {
            public class DataConverter : JsonConverter
            {
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointZero
        {
            private static void LogLoadError(ContainerManagerData containerManagerData, string message);
            private static void LogLoadError(Pipe pipe, uint sourceId, uint destinationId, string message);
        }

    }

    internal partial class DataStore
    {
        internal partial class OnePointZero : MonoBehaviour
        {
            private static Coroutine _coroutine;
            private static bool _saving;
            private static bool _loading;
            private static GameObject _saverGameObject;
            private static OnePointZero _dataStore;
            private static OnePointZero DataStore { get; set; }
            private static string _filename;
            private static string Filename { get; set; }
            private static string OldFilename { get; set; }
            public static bool Save(bool backgroundSave);
            public static bool FileExists { get; set; }
            public static bool Load();
             IEnumerator BufferedSave(string filename);
             IEnumerator BufferedLoad(string filename);
        }

    }

    partial class DataStore
    {
        partial class OnePointZero
        {
            public class PipeConverter : JsonConverter
            {
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointZero
        {
            [JsonConverter(typeof(DataConverter))]
            internal class ReadDataBuffer
            {
                public List<Pipe> Pipes { get; set; }
                public List<ContainerManagerData> Containers { get; set; }
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointZero
        {
            [JsonConverter(typeof(DataConverter))]
            internal class WriteDataBuffer
            {
                public List<string> Pipes { get; set; }
                public List<string> Containers { get; set; }
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class ContainerManagerDataConverter : JsonConverter
            {
                public bool IsRead { get; set; }
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class ContainerManagerData
            {
                public uint ContainerId;
                public bool CombineStacks;
                public string DisplayName;
                public ContainerType ContainerType;
                public ContainerManagerData();
                public ContainerManagerData(ContainerManager containerManager);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class DataConverter : JsonConverter
            {
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class EntityFinder
            {
                public Dictionary<uint, EntityPositionData> Positions { get; set; }
                public Dictionary<uint, uint> Adjustments { get; set; }
                public uint AdjustedIds(uint savedId);
                public BaseEntity Find(uint savedId, ContainerType containerType);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class EntityPositionConverter : JsonConverter
            {
                public bool IsRead { get; set; }
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
            }

            public class EntityPositionData
            {
                public uint Id { get; set; }
                public float X { get; set; }
                public float Y { get; set; }
                public float Z { get; set; }
                public string ShortPrefabName { get; set; }
                public Vector3 Vector { get; set; }
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            private static void LogLoadError(EntityPositionData quarryPumpJackData, string message);
            private static void LogLoadError(ContainerManagerData containerManagerData, string message);
            private static void LogLoadError(PipeFactoryData pipeFactoryData, string message);
            private static void LogLoadError(Pipe pipe, uint sourceId, uint destinationId, string message);
        }

    }

    internal partial class DataStore
    {
        internal partial class OnePointOne : MonoBehaviour
        {
            private const string Version;
            private static string FilenameVersion { get; set; }
            private static string _filename;
            private static string Filename { get; set; }
            private static Coroutine _coroutine;
            private static bool _saving;
            private static bool _loading;
            private static GameObject _saverGameObject;
            private static OnePointOne _dataStore;
            private static OnePointOne DataStore { get; set; }
            public static bool Save(bool backgroundSave);
            public static bool FileExists { get; set; }
            public static bool Load();
             IEnumerator BufferedSave(string filename);
             IEnumerator BufferedLoad(string filename);
        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class PipeConverter : JsonConverter
            {
                private readonly EntityFinder _entityFinder;
                public PipeConverter();
                public PipeConverter(EntityFinder entityFinder);
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class PipeFactoryDataConverter : JsonConverter
            {
                public bool IsRead { get; set; }
                public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
                public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
                public override bool CanConvert(Type objectType);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            public class PipeFactoryData
            {
                public uint PipeId { get; set; }
                public bool IsBarrel { get; set; }
                public uint[] SegmentEntityIds { get; set; }
                public uint[] LightEntityIds { get; set; }
                public PipeFactoryData();
                public PipeFactoryData(Pipe pipe);
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            [JsonConverter(typeof(DataConverter))]
            internal class ReadDataBuffer
            {
                public List<Pipe> Pipes { get; set; }
                public List<PipeFactoryData> Factories { get; set; }
                public List<ContainerManagerData> Containers { get; set; }
                public EntityFinder EntityFinder { get; set; }
            }

        }

    }

    partial class DataStore
    {
        partial class OnePointOne
        {
            [JsonConverter(typeof(DataConverter))]
            internal class WriteDataBuffer
            {
                public List<string> Pipes { get; set; }
                public List<string> Containers { get; set; }
                public List<string> Factories { get; set; }
                public List<string> QuarryPumpJackPositions;
            }

        }

    }

    internal abstract class PipeFactoryBase
    {
        protected Pipe _pipe;
        protected int _segmentCount;
        public BaseEntity PrimarySegment { get; set; }
        protected float _segmentOffset;
        protected Vector3 _rotationOffset;
        public List<BaseEntity> Segments;
        public List<BaseEntity> Lights;
        protected abstract float PipeLength { get; set; }
        protected abstract string Prefab { get; set; }
        protected static readonly Vector3 OverlappingPipeOffset;
        protected PipeFactoryBase(Pipe pipe);
        private void Init();
        protected virtual BaseEntity CreateSegment(Vector3 position, Quaternion rotation);
        public abstract void Create();
        public virtual void AttachPipeSegment(BaseEntity pipeSegmentEntity);
        public virtual void AttachLights(BaseEntity pipeLightsEntity);
        public virtual void Reverse();
        public virtual void Upgrade(BuildingGrade.Enum grade);
        public abstract void SetHealth(float health);
        protected abstract Vector3 SourcePosition { get; set; }
        protected abstract Quaternion Rotation { get; set; }
        protected abstract Vector3 GetOffsetPosition(int segmentIndex);
        protected virtual BaseEntity CreatePrimarySegment();
        protected virtual BaseEntity CreateSecondarySegment(int segmentIndex);
    }

    private abstract class PipeFactoryBase : PipeFactoryBase
    {
        protected virtual TEntity PreparePipeSegmentEntity(int pipeIndex, BaseEntity pipeSegment);
        public override void Create();
        protected PipeFactoryBase(Pipe pipe);
    }

    private class PipeFactoryBarrel : PipeFactoryBase<StorageContainer>
    {
        protected override string Prefab { get; set; }
        public PipeFactoryBarrel(Plugins.SyncPipes.Pipe pipe);
        protected override float PipeLength { get; set; }
        public override void SetHealth(float health);
        protected override Vector3 SourcePosition { get; set; }
        protected override Quaternion Rotation { get; set; }
        protected override Vector3 GetOffsetPosition(int segmentIndex);
        public override void Reverse();
        protected override BaseEntity CreateSecondarySegment(int segmentIndex);
    }

    private class PipeFactoryLowWall : PipeFactoryBase<BuildingBlock>
    {
        protected override float PipeLength { get; set; }
        private const string _prefab;
        protected override string Prefab { get; set; }
        protected override BuildingBlock PreparePipeSegmentEntity(int pipeIndex, BaseEntity pipeSegment);
        public PipeFactoryLowWall(Pipe pipe);
        private BuildingBlock[] _segmentBuildingBlocks;
        private IEnumerable<BuildingBlock> SegmentBuildingBlocks { get; set; }
        public override void AttachPipeSegment(BaseEntity pipeSegmentEntity);
        public override void Upgrade(BuildingGrade.Enum grade);
        public override void SetHealth(float health);
        protected override Vector3 SourcePosition { get; set; }
        protected override Quaternion Rotation { get; set; }
        protected override Vector3 GetOffsetPosition(int segmentIndex);
    }

}

static class Commands
{
    public static void InitializeChat();
    private static void Add(string commandSuffix, string callback);
    public static void Args(PlayerHelper playerHelper, string[] args);
    public static void PlacePipe(PlayerHelper playerHelper);
    public static void Help(PlayerHelper playerHelper);
    public static void Copy(PlayerHelper playerHelper);
    public static void Remove(PlayerHelper playerHelper);
    public static void Stats(PlayerHelper playerHelper);
    public static void OpenMenu(ConsoleSystem.Arg arg);
    public static void CloseMenu(ConsoleSystem.Arg arg);
    public static void Name(PlayerHelper playerHelper, string name);
    public static void ForceCloseMenu(PlayerHelper playerHelper);
    public static void ChangePriority(ConsoleSystem.Arg arg);
    public static void RefreshMenu(ConsoleSystem.Arg arg);
    private static Pipe GetPipe(ConsoleSystem.Arg arg, int index);
    private static bool GetBool(ConsoleSystem.Arg arg, int index);
    private static int? GetInt(ConsoleSystem.Arg arg, int index);
    public static void SetPipeState(ConsoleSystem.Arg arg);
    public static void SetPipeAutoStart(ConsoleSystem.Arg arg);
    public static void SwapPipeDirection(ConsoleSystem.Arg arg);
    public static void SetPipeMultiStack(ConsoleSystem.Arg arg);
    public static void SetPipeFurnaceStackEnabled(ConsoleSystem.Arg arg);
    public static void SetPipeFurnaceStackCount(ConsoleSystem.Arg arg);
    public static void OpenPipeFilter(ConsoleSystem.Arg arg);
    public static void MenuHelp(ConsoleSystem.Arg arg);
    public static void FlushPlayerPermissions(ConsoleSystem.Arg arg);
}

public class SyncPipesConsoleCommandAttribute : ConsoleCommandAttribute
{
    public SyncPipesConsoleCommandAttribute(string command);
}

 class SyncPipesConfig
{
    private static readonly SyncPipesConfig Default;
    public static SyncPipesConfig New();
    [JsonProperty("LogLevel")]
    public int LogLevel { get; set; }
    [JsonProperty("filterSizes")]
    public List<int> FilterSizes { get; set; }
    [JsonProperty("flowRates")]
    public List<int> FlowRates { get; set; }
    [JsonProperty("maxPipeDist")]
    public float MaximumPipeDistance { get; set; }
    [JsonProperty("minPipeDist")]
    public float MinimumPipeDistance { get; set; }
    [JsonProperty("noDecay")]
    public bool NoDecay { get; set; }
    [JsonProperty("commandPrefix")]
    public string CommandPrefix { get; set; }
    [JsonProperty("hotKey")]
    public string HotKey { get; set; }
    [JsonProperty("updateRate")]
    public int UpdateRate { get; set; }
    [JsonProperty("xmasLights")]
    public bool AttachXmasLights { get; set; }
    [JsonProperty("permLevels", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Dictionary<string, PermissionLevel> PermissionLevels { get; set; }
    [JsonProperty("salvageDestroy")]
    public bool DestroyWithSalvage { get; set; }
    [JsonProperty("experimental", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ExperimentalConfig Experimental { get; set; }
    [JsonProperty("blacklistTC")]
    public bool BlacklistTC { get; set; }
    [JsonProperty("useQuickSmelt")]
    public bool UseQuickSmelt { get; set; }
    public class PermissionLevel
    {
        [JsonProperty("upgradeLimit")]
        public int MaximumGrade { get; set; }
        [JsonProperty("pipeLimit")]
        public int MaximumPipes { get; set; }
        public static readonly PermissionLevel Default;
    }

    private string[] Validate();
    public static SyncPipesConfig Load();
    public void RegisterPermissions();
}

public class PermissionLevel
{
    [JsonProperty("upgradeLimit")]
    public int MaximumGrade { get; set; }
    [JsonProperty("pipeLimit")]
    public int MaximumPipes { get; set; }
    public static readonly PermissionLevel Default;
}

 class ExperimentalConfig
{
    [JsonProperty("barrelPipe")]
    public bool BarrelPipe { get; set; }
    [JsonProperty("permEntity")]
    public bool PermanentEntities { get; set; }
}

static class ContainerHelper
{
    public static bool IsBlacklisted(BaseEntity container);
    public static StorageContainer Find(uint id);
    public static ContainerType GetEntityType(BaseEntity container);
    public static bool InMonument(BaseEntity entity);
    private static void LogFindError(uint parentId, BaseEntity entity, ContainerType containerType, List<BaseEntity> children);
    public static BaseEntity Find(uint parentId, ContainerType containerType);
    public static BaseEntity Find(BaseEntity entity, ContainerType containerType);
    public static StorageContainer Find(BaseEntity parent);
    public static string GetShortPrefabName(ContainerType containerType);
    public static bool IsComplexStorage(ContainerType containerType);
    public static bool CanAutoStart(ContainerType containerType);
}

public class ContainerManager : MonoBehaviour
{
    internal static readonly Dictionary<uint, ContainerManager> ManagedContainerLookup;
    public static readonly List<ContainerManager> ManagedContainers;
    private readonly List<Pipe> _attachedPipes;
    public bool CombineStacks { get; set; }
    public StorageContainer Container { get; set; }
    public uint ContainerId;
    private float _cumulativeDeltaTime;
    private bool _destroyed;
    public string DisplayName;
    public bool HasAnyPipes { get; set; }
    public static void Cleanup();
    private void Kill(bool cleanup);
    public static ContainerManager Attach(BaseEntity entity, StorageContainer container, Pipe pipe);
    public static void Detach(uint containerId, Pipe pipe);
    private void Update();
    private List<Item> ItemList { get; set; }
    public ContainerType ContainerType { get; set; }
    private MovableType CanPuItem(Item item);
    private static bool CanCook(Item item);
    private bool CorrectOven(Item item);
    private bool? OvenFuel(Item item);
    private bool CanTakeItem(Item item);
    private void MoveCombineStacks(Dictionary<int, List<Pipe>> pipeGroup);
    private void MoveIndividualStacks(Dictionary<int, List<Pipe>> pipeGroup);
    private int GetAmountToMove(int itemId, int itemQuantity, int pipesLeft, Pipe pipe, int maxStackable, bool multiStack);
    private Item GetItemToMove(Item item, Pipe pipe);
    private static int GetMinStackSize(List<Item> itemStacks);
}

internal abstract class CuiMenuBase : CuiBase, IDisposable
{
    protected PlayerHelper PlayerHelper { get; set; }
    protected CuiMenuBase(PlayerHelper playerHelper);
    protected CuiElementContainer Container { get; set; }
    protected string PrimaryPanel { get; set; }
    protected bool Visible { get; set; }
    public virtual void Show();
    public virtual void Close();
    public virtual void Refresh();
    protected string MakeCommand(string commandName, object[] args);
    protected string AddPanel(string parent, string min, string max, string colour, bool cursorEnabled);
    protected string AddPanel(string parent, string min, string max, string colour, bool cursorEnabled, CuiPanel panel);
    protected void AddLabel(string parent, string text, int fontSize, TextAnchor alignment, string min, string max, string colour);
    protected void AddLabelWithOutline(string parent, string text, int fontSize, TextAnchor alignment, string min, string max, string textColour, string outlineColour, string distance, bool useGraphicAlpha);
    protected void AddImage(string parent, string min, string max, string imageUrl, string colour);
    protected string AddButton(string parent, string command, string text, string min, string max, string colour);
    public virtual void Dispose();
}

internal class CuiBase
{
    protected CuiButton MakeButton(string command, string text, string min, string max, string colour);
    protected CuiLabel MakeLabel(string text, int fontSize, TextAnchor alignment, string min, string max, string colour);
    protected CuiElement MakeLabelWithOutline(string text, int fontSize, TextAnchor alignment, string min, string max, string textColour, string outlineColour, string distance, bool useGraphicAlpha);
    protected CuiPanel MakePanel(string min, string max, string colour, bool cursorEnabled);
    protected CuiElement MakePanelWithOutline(string min, string max, string colour, bool cursorEnabled, string outlineColour, string distance, bool useGraphicAlpha);
}

public class PipeFilter
{
    public List<Item> Items { get; set; }
    private readonly List<BasePlayer> _playersInFilter;
    public void Kill();
    private void KillFilter();
    private void ForceClosePlayers();
    private void ForceClosePlayer(BasePlayer player);
    public void Closing(BasePlayer player);
    public PipeFilter(List<int> filterItems, int capacity, Pipe pipe);
    private bool CanAcceptItem(Item item, int position);
    private readonly List<Item> _addItem;
    public void Upgrade(int newCapacity);
    public void Open(PlayerHelper playerHelper);
    private ItemContainer _filterContainer;
    private readonly Pipe _pipe;
    private bool Active { get; set; }
}

static class Handlers
{
    public static bool HandleNamingContainerHit(PlayerHelper playerHelper, BaseEntity entity);
    public static bool HandlePlacementContainerHit(PlayerHelper playerHelper, BaseEntity entity);
    public static bool HandlePipeCopy(PlayerHelper playerHelper, BaseEntity entity);
    public static bool HandlePipeRemove(PlayerHelper playerHelper, BaseEntity entity);
    public static bool HandlePipeMenu(PlayerHelper playerHelper, BaseEntity entity);
    public static bool HandleContainerManagerHit(PlayerHelper playerHelper, BaseEntity entity);
    public static bool? HandlePipeUpgrade(BaseCombatEntity entity, PlayerHelper playerHelper, BuildingGrade.Enum grade);
    public static bool HandleAttachTCContainerHut(PlayerHelper playerHelper, BaseEntity hitHitEntity);
}

 class Logger
{
    public static readonly Logger PipeLoader;
    public static readonly Logger ContainerLoader;
    public static readonly Logger PipeFactoryLoader;
    public static readonly Logger FindErrors;
    public static readonly Logger Runtime;
    public Logger(string filename, LogLevels defaultLogLevel);
    private readonly string _filename;
    private readonly LogLevels _defaultLogLevel;
    public void Log(string format, object[] args);
    public void Log(LogLevels logLevel, string format, object[] args);
    public void LogSection(string section, string format, object[] args);
    public void LogSection(LogLevels logLevel, string section, string format, object[] args);
    public void LogException(Exception e, string section);
}

public class PipeMenu
{
    private string MakeCommand(string commandName, object[] args);
    private const string OnColour;
    private const string OnTextColour;
    private const string OffColour;
    private const string OffTextColour;
    private const string LabelColour;
    private static readonly Vector2 Size;
    private readonly CuiElementContainer _foregroundElementContainer;
    private readonly CuiElementContainer _backgroundElementContainer;
    private readonly CuiElementContainer _helpElementContainer;
    private string _foregroundPanel;
    private string _helpPanel;
    private readonly string _backgroundPanel;
    private readonly Pipe _pipe;
    private readonly PlayerHelper _playerHelper;
    private bool _helpOpen;
    public PipeMenu(Pipe pipe, PlayerHelper playerHelper);
    private void CreateForeground();
    private void PipeVisualPanel(string panel);
    private void StartablePanel(string panel);
    private void InfoPanel();
    private void ControlPanel(string panel);
    public void Open();
    public void Refresh();
    public void Close(PlayerHelper playerHelper);
     string AddPanel(string parent, string min, string max, string colour, bool cursorEnabled, CuiElementContainer elementContainer);
     void AddLabel(string parent, string text, int fontSize, TextAnchor alignment, string min, string max, string colour, CuiElementContainer elementContainer);
     void AddImage(string parent, string min, string max, string imageUrl, string colour, CuiElementContainer elementContainer);
     void AddButton(string parent, string command, string text, string min, string max, string colour, CuiElementContainer elementContainer);
    public void ToggleHelp();
    private void CreateHelpPanel();
}

static class LocalizationHelpers
{
    internal static Dictionary<string, string> FallBack { get; set; }
    public static string Get(Enum key, BasePlayer player, object[] args);
    public static MessageType GetMessageType(Enum key);
}

static class OverlayText
{
    private static Dictionary<MessageType, string> ColourIndex;
    public static void Show(BasePlayer player, string text, MessageType messageType, string callerName);
    public static void Show(BasePlayer player, string text, string subText, MessageType messageType, string callerName);
    public static void Show(BasePlayer player, string text, string subText, string textColour, string callerName);
    static CuiElement LabelWithOutline(CuiLabel label, string parent, string textColour, string distance, bool useAlpha, string name);
    public static void Hide(BasePlayer player, float delay, string callerName);
}

public class Pipe
{
    private static readonly Random RandomGenerator;
    internal PipeFactoryBase Factory { get; set; }
    public List<int> InitialFilterItems { get; set; }
    public float InitialHealth { get; set; }
    private PipeFilter _pipeFilter;
    static Pipe();
    public Pipe();
    public Pipe(PipeData data);
    public PipeFilter PipeFilter { get; set; }
    public IEnumerable<int> FilterItems { get; set; }
    public bool IsFurnaceSplitterEnabled { get; set; }
    public int FurnaceSplitterStacks { get; set; }
    public bool IsAutoStart { get; set; }
    public bool IsMultiStack { get; set; }
    public ulong OwnerId { get; set; }
    public string OwnerName { get; set; }
    private List<PlayerHelper> PlayersViewingMenu { get; set; }
    public string DisplayName { get; set; }
    public BuildingGrade.Enum Grade { get; set; }
    public int FilterCapacity { get; set; }
    public int FlowRate { get; set; }
    public float Health { get; set; }
    public bool Repairing { get; set; }
    public uint Id { get; set; }
    public bool InvertFilter { get; set; }
    public PipePriority Priority { get; set; }
    public Status Validity { get; set; }
    public PipeEndContainer Destination { get; set; }
    public PipeEndContainer Source { get; set; }
    public static Dictionary<ulong, Pipe> PipeLookup { get; set; }
    public static List<Pipe> Pipes { get; set; }
    public static Dictionary<uint, Dictionary<uint, bool>> ConnectedContainers { get; set; }
    public bool IsEnabled { get; set; }
    public bool CanAutoStart { get; set; }
    public float Distance { get; set; }
    public Quaternion Rotation { get; set; }
    public BaseEntity PrimarySegment { get; set; }
    public static IEnumerable<PipeData> Save();
    public static void LogLoadError(ulong pipeId, Status status, PipeData pipeData);
    public static void Load(PipeData[] dataToLoad);
    public void Create();
    private Quaternion GetRotation();
    private static bool IsOverlapping(PipeData data);
    internal uint GenerateId();
    public static Pipe Get(BaseEntity entity);
    public static Pipe Get(ulong id);
    public void SetEnabled(bool enabled);
    public bool CanPlayerOpen(PlayerHelper playerHelper);
    public static void TryCreate(PlayerHelper playerHelper);
    public static void Cleanup();
    internal void Validate();
    public void SwapDirection();
    public void OpenMenu(PlayerHelper playerHelper);
    public void CloseMenu(PlayerHelper playerHelper);
    private void RefreshMenu();
    public bool IsAlive();
    public void Kill(bool cleanup);
    private void KillSegments(bool cleanup);
    public void ChangePriority(int priorityChange);
    public void Remove(bool cleanup);
    private static void KillAll();
    public void Upgrade(BuildingGrade.Enum grade);
    public void SetHealth(float health);
    public void SetAutoStart(bool autoStart);
    public void SetMultiStack(bool multiStack);
    public void SetFurnaceStackEnabled(bool enable);
    public void SetFurnaceStackCount(int stackCount);
    public void OpenFilter(PlayerHelper playerHelper);
    public void CopyFrom(Pipe pipe);
}

public class PipeData
{
    private BaseEntity _destination;
    private BaseEntity _source;
    public ContainerType DestinationContainerType;
    public uint DestinationId;
    public int FurnaceSplitterStacks;
    public BuildingGrade.Enum Grade;
    public float Health;
    public bool IsAutoStart;
    public bool IsEnabled;
    public bool IsFurnaceSplitter;
    public bool IsMultiStack;
    public List<int> ItemFilter;
    public ulong OwnerId;
    public string OwnerName;
    public Pipe.PipePriority Priority;
    public ContainerType SourceContainerType;
    public uint SourceId;
    public PipeData();
    public PipeData(Pipe pipe);
    public PipeData(PlayerHelper playerHelper);
    [JsonIgnore]
    public BaseEntity Source { get; set; }
    [JsonIgnore]
    public BaseEntity Destination { get; set; }
}

public class PipeEndContainer
{
    private readonly Pipe _pipe;
    public PipeEndContainer(BaseEntity container, ContainerType containerType, Pipe pipe);
    public void Attach();
    public uint Id { get; set; }
    public BaseEntity Container { get; set; }
    public ContainerManager ContainerManager { get; set; }
    public StorageContainer Storage { get; set; }
    public ContainerType ContainerType { get; set; }
    public Vector3 Position { get; set; }
    public string IconUrl { get; set; }
    public bool CanAutoStart { get; set; }
    public void Start();
    public void Stop();
    public bool HasFuel();
}

abstract class PipeSegmentBase : MonoBehaviour
{
    public Pipe Pipe { get; set; }
    private BaseEntity _parent;
     void Update();
    protected void Init(Pipe pipe, BaseEntity parent);
}

 class PipeSegment : PipeSegmentBase
{
    public static void Attach(BaseEntity pipeEntity, Pipe pipe);
}

 class PipeSegmentLights : PipeSegmentBase
{
    public static void Attach(BaseEntity pipeEntity, Pipe pipe);
}

public partial class PlayerHelper
{
    private static readonly Dictionary<ulong, Dictionary<ulong, Pipe>> AllPipes;
    public static void AddPipe(Pipe pipe);
    public static void RemovePipe(Pipe pipe);
    private static readonly Dictionary<ulong, PlayerHelper> Players;
    public static PlayerHelper Get(IPlayer iPlayer);
    public static PlayerHelper Get(BasePlayer player);
    private PlayerHelper(BasePlayer player);
    partial void ExperimentalConstructor();
    public readonly BasePlayer Player;
    public UserState State;
    private bool _isUsingBinding;
    public BaseEntity Source;
    public BaseEntity Destination;
    public string NamingName;
    public Pipe CopyFrom;
    public bool IsMenuOpen { get; set; }
    public PipeMenu Menu;
    public PipeFilter PipeFilter;
    public string OverlayContainerId;
    public string ActiveOverlayText;
    public string ActiveOverlaySubText;
    public bool IsAdmin { get; set; }
    public bool IsUser { get; set; }
    public bool CanBuild { get; set; }
    public bool HasBuildPrivilege { get; set; }
    public uint TCAttchBuildingId;
    private IEnumerable<SyncPipesConfig.PermissionLevel> Permissions { get; set; }
    private SyncPipesConfig.PermissionLevel GetPermission(string userPermission);
    private int PermissionLevelMaxPipes { get; set; }
    private int PermissionLevelMaxUpgrade { get; set; }
    public int PipeLimit { get; set; }
    public int MaxUpgrade { get; set; }
    public Dictionary<ulong, Pipe> Pipes { get; set; }
    public bool HasContainerPrivilege(BaseEntity container);
    public bool HasContainerPrivilege(StorageContainer container);
    public bool ConfirmAvailablePipes();
    private void ShowPlacingBindHint();
    public void ShowPlacingOverlay(float delay);
    public void ShowCopyOverlay(float delay);
    public void ShowRemoveOverlay(float delay);
    public void ToggleRemovingPipe();
    public void ToggleCopyingPipe();
    public void TogglePlacingPipe(bool isUsingBinding);
    public void CloseMenu();
    public void PipePlacingComplete();
    public void StartNaming(string name);
    public void ShowNamingOverlay(float delay);
    public void StopNaming();
    public void StartToolCupboardBuildingId();
    public void SetPlayerToolCupboardBuildingId(uint buildingId);
    public void SetToolCupboardBuildingId(DecayEntity toolCupboard);
    public void StopToolCupboardBuildingId();
    public static void Cleanup();
    partial void ExperimentalCleanup();
    public void SendSyncPipesConsoleCommand(string commandName, object[] args);
    public void CloseFilter();
    public static void Remove(BasePlayer player);
    private string GetLocalization(Enum key, object[] args);
    public void ShowOverlay(Overlay message, object[] args);
    public void ShowOverlayWithSubText(Overlay message, Overlay subMessage, object[] args);
    public void ShowPipeStatusOverlay(Pipe.Status status);
    public string GetPipeMenuControlLabel(PipeMenu.ControlLabel label);
    public string GetMenuButton(PipeMenu.Button button);
    public string GetPipeMenuHelpLabel(PipeMenu.HelpLabel label);
    public string GetPipePriorityText(Pipe.PipePriority priority);
    public string GetPipeMenuInfo(PipeMenu.InfoLabel infoLabel);
    public void PrintToChat(Chat chat, object[] args);
    public void PrintToChatWithTitle(Chat chat, object[] args);
    public void PrintToChatWithTitle(string chat, object[] args);
}

public class StorageData
{
    public StorageData(string shortName, string url, Vector3 offset, bool partialUrl);
    public readonly string Url;
    public readonly string ShortName;
    public readonly bool PartialUrl;
    public readonly Vector3 Offset;
}

static class StorageHelper
{
    public static string GetImageUrl(BaseEntity storageEntity, int size);
    public static Vector3 GetOffset(BaseEntity storageEntity);
    private static StorageData GetDetails(BaseEntity storageEntity);
}

partial class DataStore
{
    partial class OnePointZero
    {
        public class ContainerManagerDataConverter : JsonConverter
        {
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
            private bool _canWrite;
            private bool _canRead;
            public override bool CanWrite { get; set; }
            public override bool CanRead { get; set; }
        }

    }

}

partial class OnePointZero
{
    public class ContainerManagerDataConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
        private bool _canWrite;
        private bool _canRead;
        public override bool CanWrite { get; set; }
        public override bool CanRead { get; set; }
    }

}

public class ContainerManagerDataConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
    private bool _canWrite;
    private bool _canRead;
    public override bool CanWrite { get; set; }
    public override bool CanRead { get; set; }
}

partial class DataStore
{
    partial class OnePointZero
    {
        public class ContainerManagerData
        {
            public uint ContainerId;
            public bool CombineStacks;
            public string DisplayName;
            public ContainerType ContainerType;
            public ContainerManagerData();
            public ContainerManagerData(ContainerManager containerManager);
        }

    }

}

partial class OnePointZero
{
    public class ContainerManagerData
    {
        public uint ContainerId;
        public bool CombineStacks;
        public string DisplayName;
        public ContainerType ContainerType;
        public ContainerManagerData();
        public ContainerManagerData(ContainerManager containerManager);
    }

}

public class ContainerManagerData
{
    public uint ContainerId;
    public bool CombineStacks;
    public string DisplayName;
    public ContainerType ContainerType;
    public ContainerManagerData();
    public ContainerManagerData(ContainerManager containerManager);
}

partial class DataStore
{
    partial class OnePointZero
    {
        public class DataConverter : JsonConverter
        {
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

    }

}

partial class OnePointZero
{
    public class DataConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class DataConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

partial class DataStore
{
    partial class OnePointZero
    {
        private static void LogLoadError(ContainerManagerData containerManagerData, string message);
        private static void LogLoadError(Pipe pipe, uint sourceId, uint destinationId, string message);
    }

}

partial class OnePointZero
{
    private static void LogLoadError(ContainerManagerData containerManagerData, string message);
    private static void LogLoadError(Pipe pipe, uint sourceId, uint destinationId, string message);
}

internal partial class DataStore
{
    internal partial class OnePointZero : MonoBehaviour
    {
        private static Coroutine _coroutine;
        private static bool _saving;
        private static bool _loading;
        private static GameObject _saverGameObject;
        private static OnePointZero _dataStore;
        private static OnePointZero DataStore { get; set; }
        private static string _filename;
        private static string Filename { get; set; }
        private static string OldFilename { get; set; }
        public static bool Save(bool backgroundSave);
        public static bool FileExists { get; set; }
        public static bool Load();
         IEnumerator BufferedSave(string filename);
         IEnumerator BufferedLoad(string filename);
    }

}

internal partial class OnePointZero : MonoBehaviour
{
    private static Coroutine _coroutine;
    private static bool _saving;
    private static bool _loading;
    private static GameObject _saverGameObject;
    private static OnePointZero _dataStore;
    private static OnePointZero DataStore { get; set; }
    private static string _filename;
    private static string Filename { get; set; }
    private static string OldFilename { get; set; }
    public static bool Save(bool backgroundSave);
    public static bool FileExists { get; set; }
    public static bool Load();
     IEnumerator BufferedSave(string filename);
     IEnumerator BufferedLoad(string filename);
}

partial class DataStore
{
    partial class OnePointZero
    {
        public class PipeConverter : JsonConverter
        {
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

    }

}

partial class OnePointZero
{
    public class PipeConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class PipeConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

partial class DataStore
{
    partial class OnePointZero
    {
        [JsonConverter(typeof(DataConverter))]
        internal class ReadDataBuffer
        {
            public List<Pipe> Pipes { get; set; }
            public List<ContainerManagerData> Containers { get; set; }
        }

    }

}

partial class OnePointZero
{
    [JsonConverter(typeof(DataConverter))]
    internal class ReadDataBuffer
    {
        public List<Pipe> Pipes { get; set; }
        public List<ContainerManagerData> Containers { get; set; }
    }

}

[JsonConverter(typeof(DataConverter))]
internal class ReadDataBuffer
{
    public List<Pipe> Pipes { get; set; }
    public List<ContainerManagerData> Containers { get; set; }
}

partial class DataStore
{
    partial class OnePointZero
    {
        [JsonConverter(typeof(DataConverter))]
        internal class WriteDataBuffer
        {
            public List<string> Pipes { get; set; }
            public List<string> Containers { get; set; }
        }

    }

}

partial class OnePointZero
{
    [JsonConverter(typeof(DataConverter))]
    internal class WriteDataBuffer
    {
        public List<string> Pipes { get; set; }
        public List<string> Containers { get; set; }
    }

}

[JsonConverter(typeof(DataConverter))]
internal class WriteDataBuffer
{
    public List<string> Pipes { get; set; }
    public List<string> Containers { get; set; }
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class ContainerManagerDataConverter : JsonConverter
        {
            public bool IsRead { get; set; }
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

    }

}

partial class OnePointOne
{
    public class ContainerManagerDataConverter : JsonConverter
    {
        public bool IsRead { get; set; }
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class ContainerManagerDataConverter : JsonConverter
{
    public bool IsRead { get; set; }
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class ContainerManagerData
        {
            public uint ContainerId;
            public bool CombineStacks;
            public string DisplayName;
            public ContainerType ContainerType;
            public ContainerManagerData();
            public ContainerManagerData(ContainerManager containerManager);
        }

    }

}

partial class OnePointOne
{
    public class ContainerManagerData
    {
        public uint ContainerId;
        public bool CombineStacks;
        public string DisplayName;
        public ContainerType ContainerType;
        public ContainerManagerData();
        public ContainerManagerData(ContainerManager containerManager);
    }

}

public class ContainerManagerData
{
    public uint ContainerId;
    public bool CombineStacks;
    public string DisplayName;
    public ContainerType ContainerType;
    public ContainerManagerData();
    public ContainerManagerData(ContainerManager containerManager);
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class DataConverter : JsonConverter
        {
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

    }

}

partial class OnePointOne
{
    public class DataConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class DataConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class EntityFinder
        {
            public Dictionary<uint, EntityPositionData> Positions { get; set; }
            public Dictionary<uint, uint> Adjustments { get; set; }
            public uint AdjustedIds(uint savedId);
            public BaseEntity Find(uint savedId, ContainerType containerType);
        }

    }

}

partial class OnePointOne
{
    public class EntityFinder
    {
        public Dictionary<uint, EntityPositionData> Positions { get; set; }
        public Dictionary<uint, uint> Adjustments { get; set; }
        public uint AdjustedIds(uint savedId);
        public BaseEntity Find(uint savedId, ContainerType containerType);
    }

}

public class EntityFinder
{
    public Dictionary<uint, EntityPositionData> Positions { get; set; }
    public Dictionary<uint, uint> Adjustments { get; set; }
    public uint AdjustedIds(uint savedId);
    public BaseEntity Find(uint savedId, ContainerType containerType);
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class EntityPositionConverter : JsonConverter
        {
            public bool IsRead { get; set; }
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

        public class EntityPositionData
        {
            public uint Id { get; set; }
            public float X { get; set; }
            public float Y { get; set; }
            public float Z { get; set; }
            public string ShortPrefabName { get; set; }
            public Vector3 Vector { get; set; }
        }

    }

}

partial class OnePointOne
{
    public class EntityPositionConverter : JsonConverter
    {
        public bool IsRead { get; set; }
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

    public class EntityPositionData
    {
        public uint Id { get; set; }
        public float X { get; set; }
        public float Y { get; set; }
        public float Z { get; set; }
        public string ShortPrefabName { get; set; }
        public Vector3 Vector { get; set; }
    }

}

public class EntityPositionConverter : JsonConverter
{
    public bool IsRead { get; set; }
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

public class EntityPositionData
{
    public uint Id { get; set; }
    public float X { get; set; }
    public float Y { get; set; }
    public float Z { get; set; }
    public string ShortPrefabName { get; set; }
    public Vector3 Vector { get; set; }
}

partial class DataStore
{
    partial class OnePointOne
    {
        private static void LogLoadError(EntityPositionData quarryPumpJackData, string message);
        private static void LogLoadError(ContainerManagerData containerManagerData, string message);
        private static void LogLoadError(PipeFactoryData pipeFactoryData, string message);
        private static void LogLoadError(Pipe pipe, uint sourceId, uint destinationId, string message);
    }

}

partial class OnePointOne
{
    private static void LogLoadError(EntityPositionData quarryPumpJackData, string message);
    private static void LogLoadError(ContainerManagerData containerManagerData, string message);
    private static void LogLoadError(PipeFactoryData pipeFactoryData, string message);
    private static void LogLoadError(Pipe pipe, uint sourceId, uint destinationId, string message);
}

internal partial class DataStore
{
    internal partial class OnePointOne : MonoBehaviour
    {
        private const string Version;
        private static string FilenameVersion { get; set; }
        private static string _filename;
        private static string Filename { get; set; }
        private static Coroutine _coroutine;
        private static bool _saving;
        private static bool _loading;
        private static GameObject _saverGameObject;
        private static OnePointOne _dataStore;
        private static OnePointOne DataStore { get; set; }
        public static bool Save(bool backgroundSave);
        public static bool FileExists { get; set; }
        public static bool Load();
         IEnumerator BufferedSave(string filename);
         IEnumerator BufferedLoad(string filename);
    }

}

internal partial class OnePointOne : MonoBehaviour
{
    private const string Version;
    private static string FilenameVersion { get; set; }
    private static string _filename;
    private static string Filename { get; set; }
    private static Coroutine _coroutine;
    private static bool _saving;
    private static bool _loading;
    private static GameObject _saverGameObject;
    private static OnePointOne _dataStore;
    private static OnePointOne DataStore { get; set; }
    public static bool Save(bool backgroundSave);
    public static bool FileExists { get; set; }
    public static bool Load();
     IEnumerator BufferedSave(string filename);
     IEnumerator BufferedLoad(string filename);
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class PipeConverter : JsonConverter
        {
            private readonly EntityFinder _entityFinder;
            public PipeConverter();
            public PipeConverter(EntityFinder entityFinder);
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

    }

}

partial class OnePointOne
{
    public class PipeConverter : JsonConverter
    {
        private readonly EntityFinder _entityFinder;
        public PipeConverter();
        public PipeConverter(EntityFinder entityFinder);
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class PipeConverter : JsonConverter
{
    private readonly EntityFinder _entityFinder;
    public PipeConverter();
    public PipeConverter(EntityFinder entityFinder);
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class PipeFactoryDataConverter : JsonConverter
        {
            public bool IsRead { get; set; }
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

    }

}

partial class OnePointOne
{
    public class PipeFactoryDataConverter : JsonConverter
    {
        public bool IsRead { get; set; }
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class PipeFactoryDataConverter : JsonConverter
{
    public bool IsRead { get; set; }
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

partial class DataStore
{
    partial class OnePointOne
    {
        public class PipeFactoryData
        {
            public uint PipeId { get; set; }
            public bool IsBarrel { get; set; }
            public uint[] SegmentEntityIds { get; set; }
            public uint[] LightEntityIds { get; set; }
            public PipeFactoryData();
            public PipeFactoryData(Pipe pipe);
        }

    }

}

partial class OnePointOne
{
    public class PipeFactoryData
    {
        public uint PipeId { get; set; }
        public bool IsBarrel { get; set; }
        public uint[] SegmentEntityIds { get; set; }
        public uint[] LightEntityIds { get; set; }
        public PipeFactoryData();
        public PipeFactoryData(Pipe pipe);
    }

}

public class PipeFactoryData
{
    public uint PipeId { get; set; }
    public bool IsBarrel { get; set; }
    public uint[] SegmentEntityIds { get; set; }
    public uint[] LightEntityIds { get; set; }
    public PipeFactoryData();
    public PipeFactoryData(Pipe pipe);
}

partial class DataStore
{
    partial class OnePointOne
    {
        [JsonConverter(typeof(DataConverter))]
        internal class ReadDataBuffer
        {
            public List<Pipe> Pipes { get; set; }
            public List<PipeFactoryData> Factories { get; set; }
            public List<ContainerManagerData> Containers { get; set; }
            public EntityFinder EntityFinder { get; set; }
        }

    }

}

partial class OnePointOne
{
    [JsonConverter(typeof(DataConverter))]
    internal class ReadDataBuffer
    {
        public List<Pipe> Pipes { get; set; }
        public List<PipeFactoryData> Factories { get; set; }
        public List<ContainerManagerData> Containers { get; set; }
        public EntityFinder EntityFinder { get; set; }
    }

}

[JsonConverter(typeof(DataConverter))]
internal class ReadDataBuffer
{
    public List<Pipe> Pipes { get; set; }
    public List<PipeFactoryData> Factories { get; set; }
    public List<ContainerManagerData> Containers { get; set; }
    public EntityFinder EntityFinder { get; set; }
}

partial class DataStore
{
    partial class OnePointOne
    {
        [JsonConverter(typeof(DataConverter))]
        internal class WriteDataBuffer
        {
            public List<string> Pipes { get; set; }
            public List<string> Containers { get; set; }
            public List<string> Factories { get; set; }
            public List<string> QuarryPumpJackPositions;
        }

    }

}

partial class OnePointOne
{
    [JsonConverter(typeof(DataConverter))]
    internal class WriteDataBuffer
    {
        public List<string> Pipes { get; set; }
        public List<string> Containers { get; set; }
        public List<string> Factories { get; set; }
        public List<string> QuarryPumpJackPositions;
    }

}

[JsonConverter(typeof(DataConverter))]
internal class WriteDataBuffer
{
    public List<string> Pipes { get; set; }
    public List<string> Containers { get; set; }
    public List<string> Factories { get; set; }
    public List<string> QuarryPumpJackPositions;
}

internal abstract class PipeFactoryBase
{
    protected Pipe _pipe;
    protected int _segmentCount;
    public BaseEntity PrimarySegment { get; set; }
    protected float _segmentOffset;
    protected Vector3 _rotationOffset;
    public List<BaseEntity> Segments;
    public List<BaseEntity> Lights;
    protected abstract float PipeLength { get; set; }
    protected abstract string Prefab { get; set; }
    protected static readonly Vector3 OverlappingPipeOffset;
    protected PipeFactoryBase(Pipe pipe);
    private void Init();
    protected virtual BaseEntity CreateSegment(Vector3 position, Quaternion rotation);
    public abstract void Create();
    public virtual void AttachPipeSegment(BaseEntity pipeSegmentEntity);
    public virtual void AttachLights(BaseEntity pipeLightsEntity);
    public virtual void Reverse();
    public virtual void Upgrade(BuildingGrade.Enum grade);
    public abstract void SetHealth(float health);
    protected abstract Vector3 SourcePosition { get; set; }
    protected abstract Quaternion Rotation { get; set; }
    protected abstract Vector3 GetOffsetPosition(int segmentIndex);
    protected virtual BaseEntity CreatePrimarySegment();
    protected virtual BaseEntity CreateSecondarySegment(int segmentIndex);
}

private abstract class PipeFactoryBase : PipeFactoryBase
{
    protected virtual TEntity PreparePipeSegmentEntity(int pipeIndex, BaseEntity pipeSegment);
    public override void Create();
    protected PipeFactoryBase(Pipe pipe);
}

private class PipeFactoryBarrel : PipeFactoryBase<StorageContainer>
{
    protected override string Prefab { get; set; }
    public PipeFactoryBarrel(Plugins.SyncPipes.Pipe pipe);
    protected override float PipeLength { get; set; }
    public override void SetHealth(float health);
    protected override Vector3 SourcePosition { get; set; }
    protected override Quaternion Rotation { get; set; }
    protected override Vector3 GetOffsetPosition(int segmentIndex);
    public override void Reverse();
    protected override BaseEntity CreateSecondarySegment(int segmentIndex);
}

private class PipeFactoryLowWall : PipeFactoryBase<BuildingBlock>
{
    protected override float PipeLength { get; set; }
    private const string _prefab;
    protected override string Prefab { get; set; }
    protected override BuildingBlock PreparePipeSegmentEntity(int pipeIndex, BaseEntity pipeSegment);
    public PipeFactoryLowWall(Pipe pipe);
    private BuildingBlock[] _segmentBuildingBlocks;
    private IEnumerable<BuildingBlock> SegmentBuildingBlocks { get; set; }
    public override void AttachPipeSegment(BaseEntity pipeSegmentEntity);
    public override void Upgrade(BuildingGrade.Enum grade);
    public override void SetHealth(float health);
    protected override Vector3 SourcePosition { get; set; }
    protected override Quaternion Rotation { get; set; }
    protected override Vector3 GetOffsetPosition(int segmentIndex);
}


```

---

## TankCommander by k1lly0u - Drive tanks, shoot stuff

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

