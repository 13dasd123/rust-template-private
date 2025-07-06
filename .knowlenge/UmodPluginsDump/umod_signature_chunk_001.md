# uMod Plugins Dataset - Code Abstractions

Generated: 2025-07-06 20:25:18
Total plugins: 1506

## AbsolutGifts by  - Daily gift reward system

```csharp
using System.Collections.Generic;
using System;
using System.Linq;
using UnityEngine;
using Oxide.Core.Configuration;
using Oxide.Game.Rust.Cui;
using Oxide.Core;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Absolut Gifts", "Absolut", "1.5.2")]
[Description("Daily gift reward system")]
 class AbsolutGifts : RustPlugin
{
    [PluginReference]
     Plugin ImageLibrary;
     Plugin ServerRewards;
     Plugin Economics;
     Plugin AbsolutCombat;
     GiftData agdata;
    private DynamicConfigFile AGData;
     bool initialized;
     int GlobalTime;
     string TitleColor;
     string MsgColor;
     int max;
    private Dictionary<int, float[]> GiftPositions;
    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, GiftCreation> giftprep;
    private Dictionary<ulong, Vector3> AFK;
    private Dictionary<ulong, Info> UIinfo;
     class Info
    {
        public bool open;
        public int page;
        public bool admin;
        public ItemCategory cat;
    }

     void Loaded();
     void Unload();
     void OnServerInitialized();
     void LoadImages();
    private void LoadAllItemImages();
    private void CreateLoadOrder();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void InitializePlayer(BasePlayer player);
    private void DestroyPlayer(BasePlayer player);
    static double GrabCurrentTime();
    private string TryForImage(string shortname, ulong skin);
    public string GetImage(string shortname, ulong skin, bool returnUrl);
    public bool HasImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public List<ulong> GetImageList(string shortname);
    public bool isReady();
    private void CancelGiftCreation(BasePlayer player);
    public void DestroyGiftPanel(BasePlayer player, bool background);
     void ChangeGlobalTime();
    private void CheckPlayers();
     bool CheckAFK(BasePlayer player);
    private void ProcessGift(BasePlayer player, int GiftIndex);
    private void CreateNumberPadButton(CuiElementContainer container, string panelName, int i, int number, string command);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3, string arg4);
    private string GetMSG(string message, BasePlayer player, string arg1, string arg2, string arg3, string arg4);
     bool isAuth(BasePlayer player);
    private string PanelStatic;
    private string PanelGift;
    private string PanelIcon;
    private string PanelOnScreen;
    private string PanelTime;
    public class UI
    {
        static public CuiElementContainer CreateOverlayContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer container, string panel, string img, string aMin, string aMax);
        static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    }

    private Dictionary<string, string> UIColors;
    private Dictionary<string, string> TextColors;
     void Background(BasePlayer player);
     void PlayerTimeCounter(BasePlayer player);
     void GiftPanel(BasePlayer player);
    private void CreateGifts(BasePlayer player, int step);
     string GetDisplayNameFromSN(int id);
    private void CreateGiftMenuEntry(BasePlayer player, int ID, int num);
     void NewGiftIcon(BasePlayer player);
    private void NumberPad(BasePlayer player, string cmd, string title);
     void OnScreen(BasePlayer player, string msg, string arg1, string arg2, string arg3);
     void SetGiftPositions();
    private float[] GiftPos(int number);
    private float[] FilterButton(int number);
    private float[] BackgroundButton(int number);
    private float[] giftentrypos(int number);
    private float[] CalcButtonPos(int number);
    private float[] CalcNumButtonPos(int number);
    [ChatCommand("gift")]
    private void cmdgift(BasePlayer player, string command, string[] args);
    [ChatCommand("max")]
    private void chatmax(BasePlayer player, string command, string[] args);
    [ConsoleCommand("UI_AG_GiftMenu")]
    private void cmdUI_AG_GiftMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_SelectTime")]
    private void cmdUI_AG_SelectTime(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_VIP")]
    private void cmdUI_AG_VIP(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_ToggleAdmin")]
    private void cmdUI_AG_ToggleAdmin(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_ChangeCat")]
    private void cmdUI_AG_ChangeCat(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_CreateGifts")]
    private void cmdUI_AG_CreateGifts(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_RedeemGift")]
    private void cmdUI_AG_RedeemGift(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_RemoveGift")]
    private void cmdUI_AG_RemoveGift(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_FinalizeGift")]
    private void cmdUI_AG_FinalizeGift(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_DestroyGiftPanel")]
    private void cmdUI_AG_DestroyGiftPanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_CancelGiftCreation")]
    private void cmdUI_AG_CancelListing(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_SelectGift")]
    private void cmdUI_AG_SelectGift(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AG_SelectAmount")]
    private void cmdUI_AG_SelectPriceAmount(ConsoleSystem.Arg arg);
    private void SaveLoop();
    private void InfoLoop();
     class GiftData
    {
        public Dictionary<int, GiftEntry> Gifts;
        public Dictionary<ulong, playerdata> Players;
    }

     class GiftEntry
    {
        public bool vip;
        public List<Gift> gifts;
        public int ID;
    }

     class Gift
    {
        public ulong Skin;
        public int amount;
        public int ID;
        public bool SR;
        public bool Eco;
        public bool AC;
    }

     class playerdata
    {
        public int PlayerTime;
        public Dictionary<int, GiftEntry> pendingGift;
        public List<int> ReceivedGifts;
        public double ResetTime;
    }

     class GiftCreation
    {
        public int time;
        public bool vip;
        public List<Gift> gifts;
        public Gift currentgift;
    }

     void SaveData();
     void LoadData();
     float Default_minx;
     float Default_miny;
     float Default_maxx;
     float Default_maxy;
    private ConfigData configData;
     class ConfigData
    {
        public int InfoInterval { get; set; }
        public bool NoAFK { get; set; }
        public bool UseGatherIncrease { get; set; }
        public int ResetInDays { get; set; }
        public int GiftsPerPanel { get; set; }
        public bool HideVIP { get; set; }
        public string GiftIconImage { get; set; }
        public int UITextSize { get; set; }
        public float minx { get; set; }
        public float miny { get; set; }
        public float maxx { get; set; }
        public float maxy { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData config);
     Dictionary<string, string> messages;
}

 class Info
{
    public bool open;
    public int page;
    public bool admin;
    public ItemCategory cat;
}

public class UI
{
    static public CuiElementContainer CreateOverlayContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer container, string panel, string img, string aMin, string aMax);
    static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
}

 class GiftData
{
    public Dictionary<int, GiftEntry> Gifts;
    public Dictionary<ulong, playerdata> Players;
}

 class GiftEntry
{
    public bool vip;
    public List<Gift> gifts;
    public int ID;
}

 class Gift
{
    public ulong Skin;
    public int amount;
    public int ID;
    public bool SR;
    public bool Eco;
    public bool AC;
}

 class playerdata
{
    public int PlayerTime;
    public Dictionary<int, GiftEntry> pendingGift;
    public List<int> ReceivedGifts;
    public double ResetTime;
}

 class GiftCreation
{
    public int time;
    public bool vip;
    public List<Gift> gifts;
    public Gift currentgift;
}

 class ConfigData
{
    public int InfoInterval { get; set; }
    public bool NoAFK { get; set; }
    public bool UseGatherIncrease { get; set; }
    public int ResetInDays { get; set; }
    public int GiftsPerPanel { get; set; }
    public bool HideVIP { get; set; }
    public string GiftIconImage { get; set; }
    public int UITextSize { get; set; }
    public float minx { get; set; }
    public float miny { get; set; }
    public float maxx { get; set; }
    public float maxy { get; set; }
}


```

---

## AbsolutMarket by  - Global Trade Menu. Any player can buy and sell items. Simply list it!

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("AbsolutMarket", "Absolut", "1.8.4", ResourceId = 2118)]
public class AbsolutMarket : RustPlugin
{
    [PluginReference]
    private Plugin ServerRewards;
    private Plugin Economics;
    private Plugin ImageLibrary;
    private MarketData mData;
    private DynamicConfigFile MData;
    private string TitleColor;
    private string MsgColor;
    private Vector3 eyesAdjust;
    private FieldInfo serverinput;
    private bool initialized;
    private List<ulong> MenuState;
    private Dictionary<string, Timer> timers;
    private Dictionary<ulong, List<AMItem>> PlayerBoxContents;
    private Dictionary<ulong, AMItem> SalesItemPrep;
    private Dictionary<ulong, List<Item>> PlayerInventory;
    private Dictionary<ulong, List<Item>> ItemsToTransfer;
    private Dictionary<ulong, existence> UIInfo;
    private class existence
    {
        public StorageContainer box;
        public int page;
        public Category cat;
        public AMItem PurchaseItem;
    }

    private void Loaded();
    private void Unload();
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnPlayerInit(BasePlayer player);
    private void OnServerInitialized();
    private void OnEntityDeath(BaseEntity entity, HitInfo hitInfo);
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private StorageContainer GetTradeBox(ulong Buyer);
    private Vector3 GetVector3(XYZ xyz);
    private bool GetTradeBoxContents(BasePlayer player, StorageContainer box);
    private bool BoxCheck(BasePlayer player, uint item);
    private void AddMessages(ulong player, string message, string arg1, string arg2, string arg3, string arg4);
    private void SendMessages(BasePlayer player);
    private string TryForImage(string shortname, ulong skin);
    public string GetImage(string shortname, ulong skin, bool returnUrl);
    public bool HasImage(string shortname, ulong skin);
    public bool AddImage(string url, string shortname, ulong skin);
    public List<ulong> GetImageList(string shortname);
    public bool isReady();
    private IEnumerable<AMItem> GetItems(ItemContainer container);
    private IEnumerable<Item> GetItemsOnly(ItemContainer container);
    private void XferCost(uint item, BasePlayer player, uint Listing, ItemContainer SellerBox);
    private Item BuildCostItems(string shortname, int amount);
    private void OnItemRemovedFromContainer(ItemContainer cont, Item item);
    private void RemoveListing(ulong seller, string name, uint ID, string reason);
    private void CancelListing(BasePlayer player);
    private void SRAction(ulong ID, int amount, string action);
    private object CheckPoints(ulong ID);
    private void ECOAction(ulong ID, int amount, string action);
    private double CheckEco(ulong ID);
    private void NumberPad(BasePlayer player, string cmd);
    private void CreateNumberPadButton(CuiElementContainer container, string panelName, int i, int number, string command);
    private void GetSendMSG(BasePlayer player, string message, string arg1, string arg2, string arg3, string arg4);
    private string GetMSG(string message, BasePlayer player, string arg1, string arg2, string arg3, string arg4);
    public void DestroyMarketPanel(BasePlayer player);
    public void DestroyPurchaseScreen(BasePlayer player);
    private void ToggleMarketScreen(BasePlayer player);
    private bool isAuth(BasePlayer player);
    private string PanelMarket;
    private string PanelPurchase;
    private string PanelOnScreen;
    public class UI
    {
        static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
        static public CuiElementContainer CreateOverlayContainer(string panelName, string color, string aMin, string aMax, bool cursor);
        static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
        static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
        static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
        static public void LoadImage(CuiElementContainer container, string panel, string img, string aMin, string aMax);
        static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
        static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
    }

    private Dictionary<string, string> UIColors;
    private void MarketMainScreen(BasePlayer player, int page, Category cat);
    private void CreateMarketListingButton(CuiElementContainer container, string panelName, BasePlayer player, AMItem item, bool seller, int num);
    private void CreateMarketListingButtonSimple(CuiElementContainer container, string panelName, BasePlayer player, AMItem item, bool seller, int num);
    private void MarketSellScreen(BasePlayer player, int page);
    private void SellItems(BasePlayer player, int step, int page);
    private void PurchaseConfirmation(BasePlayer player, uint index);
    private void AdminPanel(BasePlayer player);
    private void MarketBackgroundMenu(BasePlayer player, int page);
    private void BlackListing(BasePlayer player, int page, string action);
    private float[] MarketEntryPos(int number);
    private float[] FilterButton(int number);
    private float[] BackgroundButton(int number);
    private float[] CalcButtonPos(int number);
    private float[] CalcNumButtonPos(int number);
    [ConsoleCommand("UI_DestroyMarketPanel")]
    private void cmdUI_DestroyBoxConfirmation(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_DestroyPurchaseScreen")]
    private void cmdUI_DestroyPurchaseScreen(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ToggleMarketScreen")]
    private void cmdUI_ToggleMarketScreen(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectSalesItem")]
    private void cmdUI_SelectSalesItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectMoney")]
    private void cmdUI_SelectMoney(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SRAmount")]
    private void cmdUI_SRAmount(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ECOAmount")]
    private void cmdUI_ECOAmount(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectpriceItemshortname")]
    private void cmdUI_SelectpriceItemshortname(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SelectPriceAmount")]
    private void cmdUI_SelectPriceAmount(ConsoleSystem.Arg arg);
    private uint GetRandomNumber();
    [ConsoleCommand("UI_ListItem")]
    private void cmdUI_ListItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_CancelListing")]
    private void cmdUI_CancelListing(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ChangeBackground")]
    private void cmdUI_ChangeBackground(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_MarketMainScreen")]
    private void cmdUI_MainMarketScreen(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_MarketBackgroundMenu")]
    private void cmdUI_MarketBackgroundMenu(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BlackList")]
    private void cmdUI_BlackList(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ClearMarket")]
    private void cmdUI_ClearMarket(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_AdminPanel")]
    private void cmdUI_AdminPanel(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_MarketSellScreen")]
    private void cmdUI_MarketSellScreen(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_Mode")]
    private void cmdUI_Mode(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SetTradeBox")]
    private void cmdUI_SetBoxMode(ConsoleSystem.Arg arg);
    private object FindEntityFromRay(BasePlayer player);
    private void OnScreen(BasePlayer player, string msg, string arg1, string arg2, string arg3);
    [ConsoleCommand("UI_BuyConfirm")]
    private void cmdUI_BuyConfirm(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ProcessMoney")]
    private void cmdUI_ProcessMoney(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_ProcessItem")]
    private void cmdUI_ProcessItem(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_RemoveListing")]
    private void cmdUI_RemoveListing(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_SellItems")]
    private void cmdUI_SellItems(ConsoleSystem.Arg arg);
    [ConsoleCommand("UI_BackListItem")]
    private void cmdUI_BackListItem(ConsoleSystem.Arg arg);
    private void SaveLoop();
    private void InfoLoop();
    private void CheckListings();
    private void SetBoxFullNotification(string ID);
    private class MarketData
    {
        public Dictionary<uint, AMItem> MarketListings;
        public Dictionary<ulong, XYZ> TradeBox;
        public Dictionary<ulong, string> background;
        public Dictionary<ulong, bool> mode;
        public Dictionary<ulong, List<Unsent>> OutstandingMessages;
        public List<string> Blacklist;
        public Dictionary<ulong, string> names;
    }

    private class XYZ
    {
        public float x;
        public float y;
        public float z;
    }

    private class Unsent
    {
        public string message;
        public string arg1;
        public string arg2;
        public string arg3;
        public string arg4;
    }

    private class AMItem
    {
        public string name;
        public string shortname;
        public ulong skin;
        public uint ID;
        public Category cat;
        public bool approved;
        public Category pricecat;
        public string priceItemshortname;
        public int priceItemID;
        public int priceAmount;
        public int amount;
        public int stepNum;
        public ulong seller;
        public float condition;
    }

    private void AddNeededImages();
    private void RefreshBackgrounds();
    private Dictionary<string, string> UIElements;
    private Dictionary<Category, List<string>> Categorization;
    private void SaveData();
    private void LoadData();
    private ConfigData configData;
    private class ConfigData
    {
        public string MarketMenuKeyBinding { get; set; }
        public bool UseUniqueNames { get; set; }
        public bool ServerRewards { get; set; }
        public int InfoInterval { get; set; }
        public bool Economics { get; set; }
        public bool ForceSimpleUI { get; set; }
        public Dictionary<string, string> CustomBackgrounds { get; set; }
        public Dictionary<Category, List<string>> Categorization { get; set; }
        public List<ulong> BanSteamIDs { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private Dictionary<string, string> messages;
}

private class existence
{
    public StorageContainer box;
    public int page;
    public Category cat;
    public AMItem PurchaseItem;
}

public class UI
{
    static public CuiElementContainer CreateElementContainer(string panel, string color, string aMin, string aMax, bool cursor);
    static public CuiElementContainer CreateOverlayContainer(string panelName, string color, string aMin, string aMax, bool cursor);
    static public void CreatePanel(CuiElementContainer element, string panel, string color, string aMin, string aMax, bool cursor);
    static public void CreateLabel(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, TextAnchor align);
    static public void CreateButton(CuiElementContainer element, string panel, string color, string text, int size, string aMin, string aMax, string command, TextAnchor align);
    static public void LoadImage(CuiElementContainer container, string panel, string img, string aMin, string aMax);
    static public void CreateTextOverlay(CuiElementContainer element, string panel, string text, string color, int size, string aMin, string aMax, TextAnchor align, float fadein);
    static public void CreateTextOutline(CuiElementContainer element, string panel, string colorText, string colorOutline, string text, int size, string aMin, string aMax, TextAnchor align);
}

private class MarketData
{
    public Dictionary<uint, AMItem> MarketListings;
    public Dictionary<ulong, XYZ> TradeBox;
    public Dictionary<ulong, string> background;
    public Dictionary<ulong, bool> mode;
    public Dictionary<ulong, List<Unsent>> OutstandingMessages;
    public List<string> Blacklist;
    public Dictionary<ulong, string> names;
}

private class XYZ
{
    public float x;
    public float y;
    public float z;
}

private class Unsent
{
    public string message;
    public string arg1;
    public string arg2;
    public string arg3;
    public string arg4;
}

private class AMItem
{
    public string name;
    public string shortname;
    public ulong skin;
    public uint ID;
    public Category cat;
    public bool approved;
    public Category pricecat;
    public string priceItemshortname;
    public int priceItemID;
    public int priceAmount;
    public int amount;
    public int stepNum;
    public ulong seller;
    public float condition;
}

private class ConfigData
{
    public string MarketMenuKeyBinding { get; set; }
    public bool UseUniqueNames { get; set; }
    public bool ServerRewards { get; set; }
    public int InfoInterval { get; set; }
    public bool Economics { get; set; }
    public bool ForceSimpleUI { get; set; }
    public Dictionary<string, string> CustomBackgrounds { get; set; }
    public Dictionary<Category, List<string>> Categorization { get; set; }
    public List<ulong> BanSteamIDs { get; set; }
}


```

---

## ActiveSort by Mevent - Sorts furnaces and refineries on click

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Active Sort", "Mevent", "1.0.7")]
[Description("Sorts furnace and refinery on click")]
internal class ActiveSort : RustPlugin
{
    private static ActiveSort _instance;
    private readonly Dictionary<BasePlayer, ActiveSortUI> _components;
    private const string PermUse;
    private const string Layer;
    private readonly Dictionary<string, string> _furnaceItems;
    private readonly Dictionary<string, string> _refineryItems;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "ShowUI")]
        public bool ShowUI;
        [JsonProperty(PropertyName = "ButtonPositionOffset")]
        public Vector2 ButtonPositionOffset;
        [JsonProperty(PropertyName = "ButtonSize")]
        public Vector2 ButtonSize;
        [JsonProperty(PropertyName = "ButtonColorHex")]
        public string ButtonColorHex;
        [JsonProperty(PropertyName = "ButtonCaptionColorHex")]
        public string ButtonCaptionColorHex;
        [JsonProperty(PropertyName = "ButtonCaptionFontSize")]
        public int ButtonCaptionFontSize;
        [JsonProperty(PropertyName = "ButtonCaptionIsBold")]
        public bool ButtonCaptionIsBold;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private void Init();
    private void Unload();
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnPlayerLootEnd(PlayerLoot inventory);
    [ConsoleCommand("activesort.sort")]
    private void HandlerSortLoot(ConsoleSystem.Arg arg);
    private class ActiveSortUI : FacepunchBehaviour
    {
        private readonly Vector2 _buttonBasePosition;
        private BasePlayer _player;
        private void Awake();
        private void RenderUI();
        private void OnDestroy();
        public void Kill();
    }

    private ActiveSortUI GetComponent(BasePlayer player);
    private static string HexToCuiColor(string hex, float alpha);
    private void PutToContainer(Dictionary<uint, int> data, Dictionary<string, Item> items, string shortname, ItemContainer container, BasePlayer player, int spaceLeft, bool reserve);
    private Item TakeStack(Dictionary<uint, int> data, Item item, int targetCount);
    private void FilterOnlyNotProcessed(Dictionary<uint, int> data, Dictionary<string, Item> items, BasePlayer player, ItemContainer container, FurnaceType type);
    private void FilterWhitelist(Dictionary<uint, int> data, Dictionary<string, Item> items, BasePlayer player, ItemContainer container, FurnaceType type);
    private void ReturnToPlayer(Dictionary<uint, int> data, BasePlayer player, Item item);
    private readonly Dictionary<BasePlayer, Dictionary<uint, int>> _stackByItem;
    private Dictionary<string, Item> CloneAndPackItems(ItemContainer container, Dictionary<uint, int> data);
    private void ClearContainer(ItemContainer container);
    private string Msg(string key, string userIDString);
    protected override void LoadDefaultMessages();
    private void SortLoot(BasePlayer player);
    private bool CanSort(BasePlayer player);
}

private class Configuration
{
    [JsonProperty(PropertyName = "ShowUI")]
    public bool ShowUI;
    [JsonProperty(PropertyName = "ButtonPositionOffset")]
    public Vector2 ButtonPositionOffset;
    [JsonProperty(PropertyName = "ButtonSize")]
    public Vector2 ButtonSize;
    [JsonProperty(PropertyName = "ButtonColorHex")]
    public string ButtonColorHex;
    [JsonProperty(PropertyName = "ButtonCaptionColorHex")]
    public string ButtonCaptionColorHex;
    [JsonProperty(PropertyName = "ButtonCaptionFontSize")]
    public int ButtonCaptionFontSize;
    [JsonProperty(PropertyName = "ButtonCaptionIsBold")]
    public bool ButtonCaptionIsBold;
}

private class ActiveSortUI : FacepunchBehaviour
{
    private readonly Vector2 _buttonBasePosition;
    private BasePlayer _player;
    private void Awake();
    private void RenderUI();
    private void OnDestroy();
    public void Kill();
}


```

---

## AdminAntiHackFix by Solarix - Fix for admin staff getting kicked for AntiHack violations

```csharp

Oxide.Plugins
[Info("Admin AntiHack Fix", "Solarix", "1.0.0"), Description("Fix for admin staff getting kicked for AntiHack violation(s).")]
public class AdminAntiHackFix : RustPlugin
{
    private object OnPlayerViolation(BasePlayer player, AntiHackType type, float amount);
}


```

---

## AdminChat by LaserHydra - Allows admins to write in an admin-only chatroom

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Admin Chat", "LaserHydra", "2.0.4")]
[Description("Allows admins to write in an admin-only chatroom")]
internal class AdminChat : CovalencePlugin
{
    private const string Permission;
    private Configuration _config;
    private List<string> _enabledUserIds;
    private object OnUserChat(IPlayer player, string message);
    private void OnBetterChat(Dictionary<string, object> data);
    [Command("adminchat"), Permission(Permission)]
    private void AdminChatTogggleCommand(IPlayer player, string command, string[] args);
    private void SendAdminMessage(IPlayer sender, string message);
    private bool HasAdminChatEnabled(IPlayer player);
    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class Configuration
    {
        [JsonProperty("Prefix (admin chat is used when message starts with this)")]
        public string Prefix { get; set; }
        [JsonProperty("Format (how the message is formatted in chat)")]
        public string Format { get; set; }
    }

}

private class Configuration
{
    [JsonProperty("Prefix (admin chat is used when message starts with this)")]
    public string Prefix { get; set; }
    [JsonProperty("Format (how the message is formatted in chat)")]
    public string Format { get; set; }
}


```

---

## AdminChatroom by  - Allows admins to chat privately with each other in-game

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using System.Linq;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Admin Chatroom", "austinv900", "1.0.2")]
[Description("Allows admins to send messages back and forth between other admins")]
 class AdminChatroom : CovalencePlugin
{
     HashSet<IPlayer> AdminChat;
     void Init();
     void OnUserConnected(IPlayer player);
     void OnUserDisconnected(IPlayer player);
     string Permission;
    protected override void LoadDefaultConfig();
     void LoadMessages();
    [Command("a")]
     void cmdAdminChat(IPlayer player, string command, string[] args);
     void SendRoomMessage(string name, string message);
     string Lang(string key, string id, object[] args);
     bool IsAdmin(IPlayer player);
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     IPlayer FindConnectedPlayer(string var);
}


```

---

## AdminDecayHammer by Bazz3l - Allows you to hit a base with the hammer to increase its decay rate instead of removing.

```csharp
using System.Collections.Generic;
using System;

Oxide.Plugins
[Info("Admin Decay Hammer", "Bazz3l", "0.0.5")]
[Description("Hit a building block to start a faster decay.")]
 class AdminDecayHammer : RustPlugin
{
    private const string Perm;
    private List<ulong> Users;
    private PluginConfig configData;
    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private class PluginConfig
    {
        public float DecayVariance;
    }

    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void Init();
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private void BuildingDecay(uint buildingID);
    [ChatCommand("decayhammer")]
     void DecayHammerCommand(BasePlayer player, string cmd, string[] args);
}

private class PluginConfig
{
    public float DecayVariance;
}


```

---

## AdminDeepCover by VisEntities - Hides the original identity of admins by masking their steam profiles

```csharp
using CompanionServer;
using ConVar;
using Facepunch;
using Facepunch.Math;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Admin Deep Cover", "Dana", "2.2.8")]
[Description("Hides the original identity of admins by masking their steam profiles")]
public class AdminDeepCover : RustPlugin
{
    [PluginReference]
     Plugin BetterChat;
    private DynamicConfigFile _pluginData;
    private AdminDeepCoverData _adminDeepCoverData;
     Configuration config;
    private const string Perm;
    private void Init();
    protected override void LoadDefaultMessages();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private object OnPlayerChat(BasePlayer player, string message, Chat.ChatChannel channel);
    private object OnUserChat(IPlayer player, string message);
    private object OnBetterChat(Dictionary<string, object> data);
    [ConsoleCommand("deepcover")]
    private void ccmdDeepCover(ConsoleSystem.Arg arg);
    [ChatCommand("deepcover")]
    private void cmdDeepCover(BasePlayer player, string command, string[] args);
    private RestoreInfo GetIdentify(BasePlayer player);
    private bool SetFakeIdentity(BasePlayer player, Identify identify, bool isChange, bool sendDiscordNotification);
    private void RemoveFakeIdentity(BasePlayer player, RestoreInfo restoreInfo, bool tempRemove);
    private List<Identify> GetAvailableIdentities();
    private bool IsIdentifyAvailable(RestoreInfo restoreInfo);
    private string GetGrid(Vector3 pos);
    private void SendDiscordMessage(ulong playerId, string playerName, ulong fakeId, string fakeName, string grid);
    private bool IsDeepCovered(ulong playerId);
    private bool API_IsDeepCovered(BasePlayer player);
    private string Lang(string key, string id, object[] args);
    private bool HasPermission(BasePlayer player, string perm);
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void SaveData();
    private void LoadData();
    private void RegisterPermissions();
    private static class PluginMessages
    {
        public const string NoPermission;
        public const string NoProfilePermission;
        public const string DeepCoverEnabled;
        public const string DeepCoverDisabled;
        public const string DeepCoverChanged;
        public const string RequestedFakeIdentifyNotFound;
        public const string NoFakeIdentitiesAvailable;
        public const string FakeIdentifyNotFound;
        public const string DataCorruptedUp;
    }

    private class AdminDeepCoverData
    {
        public Dictionary<ulong, RestoreInfo> PlayerData { get; set; }
    }

    public class Identify
    {
        public int Profile { get; set; }
        public string Name { get; set; }
        public ulong UserId { get; set; }
        [JsonProperty("Better Chat Group")]
        public string ChatGroup { get; set; }
        [JsonProperty("Required Permission")]
        public string Permission { get; set; }
    }

    public class RestoreInfo : Identify
    {
        public bool IsEnabled { get; set; }
        public bool IsRemoved { get; set; }
        public bool WasAdmin { get; set; }
        public string[] Groups { get; set; }
        public string RestoreName { get; set; }
        public ulong RestoreUserId { get; set; }
    }

    private class Configuration
    {
        [JsonProperty("Change Identity In Order")]
        public bool ChangeIdentityInOrder { get; set; }
        [JsonProperty("Remain Deep Covered After Reconnect")]
        public bool ReconnectDeepCover { get; set; }
        [JsonProperty("Remain Deep Covered In Team Chat")]
        public bool TeamChatRemainsDeepCover { get; set; }
        [JsonProperty("Remove Admin Flag When Deep Covered")]
        public bool RemoveAdminFlag { get; set; }
        [JsonProperty(PropertyName = "Discord - Enabled")]
        public bool IsDiscordEnabled { get; set; }
        [JsonProperty(PropertyName = "Discord - Webhook URL")]
        public string DiscordWebHookUrl { get; set; }
        [JsonProperty(PropertyName = "Discord - Embed Color")]
        public string DiscordEmbedColor { get; set; }
        [JsonProperty(PropertyName = "Discord - Message")]
        public string DiscordMessage { get; set; }
        [JsonProperty(PropertyName = "Discord - Embed - Description")]
        public string DiscordEmbedDescription { get; set; }
        [JsonProperty(PropertyName = "Discord - Roles To Mention")]
        public List<string> DiscordRolesToMention { get; set; }
        [JsonProperty("Fake Identities", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<Identify> Identifies { get; set; }
    }

    private class WebHookEmbedBody
    {
        [JsonProperty(PropertyName = "embeds")]
        public WebHookEmbed[] Embeds;
    }

    private class WebHookContentBody
    {
        [JsonProperty(PropertyName = "content")]
        public string Content;
    }

    private class WebHookEmbed
    {
        [JsonProperty(PropertyName = "title")]
        public string Title;
        [JsonProperty(PropertyName = "type")]
        public string Type;
        [JsonProperty(PropertyName = "description")]
        public string Description;
        [JsonProperty(PropertyName = "color")]
        public int Color;
        [JsonProperty(PropertyName = "author")]
        public WebHookAuthor Author;
        [JsonProperty(PropertyName = "image")]
        public WebHookImage Image;
        [JsonProperty(PropertyName = "fields")]
        public List<WebHookField> Fields;
        [JsonProperty(PropertyName = "footer")]
        public WebHookFooter Footer;
    }

    private class WebHookAuthor
    {
        [JsonProperty(PropertyName = "name")]
        public string Name;
        [JsonProperty(PropertyName = "url")]
        public string AuthorUrl;
        [JsonProperty(PropertyName = "icon_url")]
        public string AuthorIconUrl;
    }

    private class WebHookImage
    {
        [JsonProperty(PropertyName = "proxy_url")]
        public string ProxyUrl;
        [JsonProperty(PropertyName = "url")]
        public string Url;
        [JsonProperty(PropertyName = "height")]
        public int? Height;
        [JsonProperty(PropertyName = "width")]
        public int? Width;
    }

    private class WebHookField
    {
        [JsonProperty(PropertyName = "name")]
        public string Name;
        [JsonProperty(PropertyName = "value")]
        public string Value;
        [JsonProperty(PropertyName = "inline")]
        public bool Inline;
    }

    private class WebHookFooter
    {
        [JsonProperty(PropertyName = "text")]
        public string Text;
        [JsonProperty(PropertyName = "icon_url")]
        public string IconUrl;
        [JsonProperty(PropertyName = "proxy_icon_url")]
        public string ProxyIconUrl;
    }

}

private static class PluginMessages
{
    public const string NoPermission;
    public const string NoProfilePermission;
    public const string DeepCoverEnabled;
    public const string DeepCoverDisabled;
    public const string DeepCoverChanged;
    public const string RequestedFakeIdentifyNotFound;
    public const string NoFakeIdentitiesAvailable;
    public const string FakeIdentifyNotFound;
    public const string DataCorruptedUp;
}

private class AdminDeepCoverData
{
    public Dictionary<ulong, RestoreInfo> PlayerData { get; set; }
}

public class Identify
{
    public int Profile { get; set; }
    public string Name { get; set; }
    public ulong UserId { get; set; }
    [JsonProperty("Better Chat Group")]
    public string ChatGroup { get; set; }
    [JsonProperty("Required Permission")]
    public string Permission { get; set; }
}

public class RestoreInfo : Identify
{
    public bool IsEnabled { get; set; }
    public bool IsRemoved { get; set; }
    public bool WasAdmin { get; set; }
    public string[] Groups { get; set; }
    public string RestoreName { get; set; }
    public ulong RestoreUserId { get; set; }
}

private class Configuration
{
    [JsonProperty("Change Identity In Order")]
    public bool ChangeIdentityInOrder { get; set; }
    [JsonProperty("Remain Deep Covered After Reconnect")]
    public bool ReconnectDeepCover { get; set; }
    [JsonProperty("Remain Deep Covered In Team Chat")]
    public bool TeamChatRemainsDeepCover { get; set; }
    [JsonProperty("Remove Admin Flag When Deep Covered")]
    public bool RemoveAdminFlag { get; set; }
    [JsonProperty(PropertyName = "Discord - Enabled")]
    public bool IsDiscordEnabled { get; set; }
    [JsonProperty(PropertyName = "Discord - Webhook URL")]
    public string DiscordWebHookUrl { get; set; }
    [JsonProperty(PropertyName = "Discord - Embed Color")]
    public string DiscordEmbedColor { get; set; }
    [JsonProperty(PropertyName = "Discord - Message")]
    public string DiscordMessage { get; set; }
    [JsonProperty(PropertyName = "Discord - Embed - Description")]
    public string DiscordEmbedDescription { get; set; }
    [JsonProperty(PropertyName = "Discord - Roles To Mention")]
    public List<string> DiscordRolesToMention { get; set; }
    [JsonProperty("Fake Identities", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<Identify> Identifies { get; set; }
}

private class WebHookEmbedBody
{
    [JsonProperty(PropertyName = "embeds")]
    public WebHookEmbed[] Embeds;
}

private class WebHookContentBody
{
    [JsonProperty(PropertyName = "content")]
    public string Content;
}

private class WebHookEmbed
{
    [JsonProperty(PropertyName = "title")]
    public string Title;
    [JsonProperty(PropertyName = "type")]
    public string Type;
    [JsonProperty(PropertyName = "description")]
    public string Description;
    [JsonProperty(PropertyName = "color")]
    public int Color;
    [JsonProperty(PropertyName = "author")]
    public WebHookAuthor Author;
    [JsonProperty(PropertyName = "image")]
    public WebHookImage Image;
    [JsonProperty(PropertyName = "fields")]
    public List<WebHookField> Fields;
    [JsonProperty(PropertyName = "footer")]
    public WebHookFooter Footer;
}

private class WebHookAuthor
{
    [JsonProperty(PropertyName = "name")]
    public string Name;
    [JsonProperty(PropertyName = "url")]
    public string AuthorUrl;
    [JsonProperty(PropertyName = "icon_url")]
    public string AuthorIconUrl;
}

private class WebHookImage
{
    [JsonProperty(PropertyName = "proxy_url")]
    public string ProxyUrl;
    [JsonProperty(PropertyName = "url")]
    public string Url;
    [JsonProperty(PropertyName = "height")]
    public int? Height;
    [JsonProperty(PropertyName = "width")]
    public int? Width;
}

private class WebHookField
{
    [JsonProperty(PropertyName = "name")]
    public string Name;
    [JsonProperty(PropertyName = "value")]
    public string Value;
    [JsonProperty(PropertyName = "inline")]
    public bool Inline;
}

private class WebHookFooter
{
    [JsonProperty(PropertyName = "text")]
    public string Text;
    [JsonProperty(PropertyName = "icon_url")]
    public string IconUrl;
    [JsonProperty(PropertyName = "proxy_icon_url")]
    public string ProxyIconUrl;
}


```

---

## AdminHammer by mvrb - Use the hammer to display info about entities/authorization

```csharp
using System.Collections.Generic;
using Oxide.Core.Configuration;
using UnityEngine;
using Oxide.Core;
using System;
using System.Linq;

Oxide.Plugins
[Info("AdminHammer", "mvrb", "1.13.0")]
 class AdminHammer : RustPlugin
{
    public static AdminHammer plugin;
    private const string permAllow;
    private bool logToConsole;
    private bool logAdminInfo;
    private bool showBoxContents;
    private bool showCode;
    private float toolDistance;
    private string toolUsed;
    private string commandToRun;
    private string chatCommand;
    private bool showSphere;
    private bool performanceMode;
    private int layerMask;
    private readonly DynamicConfigFile dataFile;
    private List<ulong> Users;
    protected override void LoadDefaultConfig();
    private void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void CmdAdminHammer(BasePlayer player);
    private void CmdCheckEntity(BasePlayer player);
    private void CheckEntity(BasePlayer player);
    private class AH : MonoBehaviour
    {
        public BasePlayer player;
        private float lastCheck;
        private void Awake();
        private void FixedUpdate();
        public void Destroy();
    }

    private string GetAuthorized(BaseEntity entity, BasePlayer player);
    private string GetPlayerColor(ulong id);
    private string GetName(string id);
    private T GetConfig(string name, T value);
    private string Lang(string key, string id, object[] args);
}

private class AH : MonoBehaviour
{
    public BasePlayer player;
    private float lastCheck;
    private void Awake();
    private void FixedUpdate();
    public void Destroy();
}


```

---

## AdminKey by birthdates - Get admin from a key code instead of going into console

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System.Collections.Generic;

Oxide.Plugins
[Info("Admin Key", "birthdates", "1.2.8")]
[Description("Get admin from a key code instead of going into console")]
public class AdminKey : RustPlugin
{
    private void Init();
    private ConfigFile _config;
     void RemoveAdmin(BasePlayer player);
     void RemoveAuth(BasePlayer player);
     void AddAdmin(BasePlayer player);
    public void AdminKeyCommand(BasePlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
    public class ConfigFile
    {
        [JsonProperty(PropertyName = "Admin keys")]
        public List<string> keys;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "Admin keys")]
    public List<string> keys;
    public static ConfigFile DefaultConfig();
}


```

---

## AdminLeaveJoin by austinv900 - Displays a chat message when a admin leaves or joins to all the players

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using System.Linq;

Oxide.Plugins
[Info("AdminLeaveJoin", "austinv900", "0.1.10", ResourceId = 2032)]
[Description("Custom Join and Leave Message when admins leave and join")]
 class AdminLeaveJoin : CovalencePlugin
{
     Dictionary<IPlayer, string> ActiveStaffList;
     void Init();
     void OnServerSave();
     void OnUserConnected(IPlayer player);
     void OnUserDisconnected(IPlayer player);
     bool showAdminJoin;
     bool showAdminLeave;
     bool showModJoin;
     bool showModLeave;
     string ModeratorGroup;
     string AdminGroup;
     string AdminPermission;
     string ModeratorPermission;
     string msgPrefix;
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
    [Command("staff")]
     void cmdAvailable(IPlayer player, string command, string[] args);
     void UpdateList();
     void JoinLeave(IPlayer player, bool Join);
     void MessageBroadcast(string name, string rank, string status);
     string ListToString(List<T> list, int first, string seperator);
     void SetConfig(object[] args);
     T GetConfig(T defaultVal, object[] args);
     bool IsAdmin(IPlayer player);
     bool IsModerator(IPlayer player);
     string Lang(string key, string id, object[] args);
     string MessageFormat(string msg, string id);
}


```

---

## AdminLogger by 1AK1 - Logs admin commands usage in a file and console

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Facepunch.Extend;
using CompanionServer.Handlers;
using System.Collections;
using UnityEngine.Networking;
using UnityEngine;

Oxide.Plugins
[Info("Admin Logger", "AK", "2.4.1")]
[Description("Logs admin commands usage in a file and console.")]
internal class AdminLogger : CovalencePlugin
{
    [PluginReference]
    private Plugin Vanish;
    private Plugin AdminRadar;
    private Plugin NightVision;
    private Plugin ConvertStatus;
    private Plugin InventoryViewer;
    private Plugin PlayerAdministration;
    private Plugin Freeze;
    private Plugin Backpacks;
    private Dictionary<ulong, bool> noclipState;
    private Dictionary<ulong, bool> godmodeState;
    private Dictionary<ulong, bool> spectateState;
    private HashSet<BasePlayer> adminList;
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Log to console (true/false)")]
        public bool LogToConsole { get; set; }
        [JsonProperty(PropertyName = "Update frequency (s)")]
        public float UpdateFreq { get; set; }
        [JsonProperty(PropertyName = "Log filename")]
        public string LogFileName { get; set; }
        [JsonProperty(PropertyName = "Enable Discord Messages (true/false)")]
        public bool DiscordLog { get; set; }
        [JsonProperty(PropertyName = "Discord Messages webhook")]
        public string DiscordWebhook { get; set; }
        [JsonProperty(PropertyName = "Exclude List")]
        public List<string> ExcludeList { get; set; }
        [JsonProperty(PropertyName = "Default admin commands")]
        public DefaultCommandsOptions DefaultCommands { get; set; }
        [JsonProperty(PropertyName = "Admin plugins")]
        public PluginsCommandsOptions PluginsCommands { get; set; }
        public class DefaultCommandsOptions
        {
            [JsonProperty(PropertyName = "Admin connections logging (true/false)")]
            public bool ConnectionLog { get; set; }
            [JsonProperty(PropertyName = "Noclip logging (true/false)")]
            public bool NoclipLog { get; set; }
            [JsonProperty(PropertyName = "GodMode logging (true/false)")]
            public bool GodmodeLog { get; set; }
            [JsonProperty(PropertyName = "Spectate logging (true/false)")]
            public bool SpectateLog { get; set; }
            [JsonProperty(PropertyName = "Kill player logging (true/false)")]
            public bool KillPlayerLog { get; set; }
            [JsonProperty(PropertyName = "Admin events logging (true/false)")]
            public bool EventsAllLog { get; set; }
            [JsonProperty(PropertyName = "Admin event commands")]
            public EventsLoggingOptions EventsLogging { get; set; }
            [JsonProperty(PropertyName = "Kick logging (true/false)")]
            public bool KickAllLog { get; set; }
            [JsonProperty(PropertyName = "Kick commands")]
            public KickLoggingOptions KickLogging { get; set; }
            [JsonProperty(PropertyName = "Ban logging (true/false)")]
            public bool BanAllLog { get; set; }
            [JsonProperty(PropertyName = "Ban commands")]
            public BanLoggingOptions BanLogging { get; set; }
            [JsonProperty(PropertyName = "Mute logging (true/false)")]
            public bool MuteAllLog { get; set; }
            [JsonProperty(PropertyName = "Mute commands")]
            public MuteLoggingOptions MuteLogging { get; set; }
            [JsonProperty(PropertyName = "Entity logging (true/false)")]
            public bool EntAllLog { get; set; }
            [JsonProperty(PropertyName = "Entity commands")]
            public EntityLoggingOptions EntityLogging { get; set; }
            [JsonProperty(PropertyName = "Teleport logging (true/false)")]
            public bool TeleportAllLog { get; set; }
            [JsonProperty(PropertyName = "Teleport commands")]
            public TeleportLoggingOptions TeleportLogging { get; set; }
            [JsonProperty(PropertyName = "Give items logging (true/false)")]
            public bool GiveAllLog { get; set; }
            [JsonProperty(PropertyName = "Give commands")]
            public GiveLoggingOptions GiveLogging { get; set; }
            [JsonProperty(PropertyName = "Spawn logging (true/false)")]
            public bool SpawnAllLog { get; set; }
            [JsonProperty(PropertyName = "Spawn commands")]
            public SpawnLoggingOptions SpawnLogging { get; set; }
        }

        public class PluginsCommandsOptions
        {
            [JsonProperty(PropertyName = "Vanish logging (true/false)")]
            public bool VanishLog { get; set; }
            [JsonProperty(PropertyName = "Admin Radar logging (true/false)")]
            public bool RadarLog { get; set; }
            [JsonProperty(PropertyName = "Night Vision logging (true/false)")]
            public bool NightLog { get; set; }
            [JsonProperty(PropertyName = "Convert Status logging (true/false)")]
            public bool ConvertLog { get; set; }
            [JsonProperty(PropertyName = "Inventory Viewer logging (true/false)")]
            public bool InventoryViewerLog { get; set; }
            [JsonProperty(PropertyName = "Backpacks logging (true/false)")]
            public bool BackpacksLog { get; set; }
            [JsonProperty(PropertyName = "Freeze logging (true/false)")]
            public bool FreezeAllLog { get; set; }
            [JsonProperty(PropertyName = "Freeze commands")]
            public FreezeLoggingOptions FreezeLogging { get; set; }
            [JsonProperty(PropertyName = "Player Administration logging (true/false)")]
            public bool PlayerAdministrationAllLog { get; set; }
            [JsonProperty(PropertyName = "Player Administration commands")]
            public PlayerAdministrationLoggingOptions PlayerAdministrationLogging { get; set; }
        }

        public class EventsLoggingOptions
        {
            [JsonProperty(PropertyName = "[Attack Heli] heli.call")]
            public bool HeliCallLog { get; set; }
            [JsonProperty(PropertyName = "[Attack Heli] heli.calltome")]
            public bool HeliCallToMeLog { get; set; }
            [JsonProperty(PropertyName = "[Attack Heli] drop")]
            public bool HeliDropLog { get; set; }
            [JsonProperty(PropertyName = "[Airdrop] supply.call")]
            public bool AirdropRandomLog { get; set; }
            [JsonProperty(PropertyName = "[Airdrop] supply.drop")]
            public bool AirdropPosLog { get; set; }
        }

        public class KickLoggingOptions
        {
            [JsonProperty(PropertyName = "kick")]
            public bool KickLog { get; set; }
            [JsonProperty(PropertyName = "kickall")]
            public bool KickEveryoneLog { get; set; }
        }

        public class BanLoggingOptions
        {
            [JsonProperty(PropertyName = "ban")]
            public bool BanLog { get; set; }
            [JsonProperty(PropertyName = "unban")]
            public bool UnbanLog { get; set; }
        }

        public class MuteLoggingOptions
        {
            [JsonProperty(PropertyName = "mute")]
            public bool MuteLog { get; set; }
            [JsonProperty(PropertyName = "unmute")]
            public bool UnmuteLog { get; set; }
        }

        public class EntityLoggingOptions
        {
            [JsonProperty(PropertyName = "ent kill")]
            public bool EntKillLog { get; set; }
            [JsonProperty(PropertyName = "ent who")]
            public bool EntWhoLog { get; set; }
            [JsonProperty(PropertyName = "ent lock")]
            public bool EntLockLog { get; set; }
            [JsonProperty(PropertyName = "ent unlock")]
            public bool EntUnlockLog { get; set; }
            [JsonProperty(PropertyName = "ent auth")]
            public bool EntAuthLog { get; set; }
        }

        public class TeleportLoggingOptions
        {
            [JsonProperty(PropertyName = "teleport")]
            public bool TeleportLog { get; set; }
            [JsonProperty(PropertyName = "teleportpos")]
            public bool TeleportPosLog { get; set; }
            [JsonProperty(PropertyName = "teleport2me")]
            public bool TeleportToMeLog { get; set; }
        }

        public class GiveLoggingOptions
        {
            [JsonProperty(PropertyName = "give")]
            public bool GiveLog { get; set; }
            [JsonProperty(PropertyName = "giveid")]
            public bool GiveIdLog { get; set; }
            [JsonProperty(PropertyName = "givearm")]
            public bool GiveArmLog { get; set; }
            [JsonProperty(PropertyName = "giveto")]
            public bool GiveToLog { get; set; }
            [JsonProperty(PropertyName = "giveall")]
            public bool GiveAllLog { get; set; }
        }

        public class SpawnLoggingOptions
        {
            [JsonProperty(PropertyName = "spawn")]
            public bool SpawnLog { get; set; }
            [JsonProperty(PropertyName = "spawnat")]
            public bool SpawnAtLog { get; set; }
            [JsonProperty(PropertyName = "spawnhere")]
            public bool SpawnHereLog { get; set; }
            [JsonProperty(PropertyName = "spawnitem")]
            public bool SpawnItemLog { get; set; }
        }

        public class FreezeLoggingOptions
        {
            [JsonProperty(PropertyName = "freeze")]
            public bool FreezeLog { get; set; }
            [JsonProperty(PropertyName = "unfreeze")]
            public bool UnfreezeLog { get; set; }
            [JsonProperty(PropertyName = "freezeall")]
            public bool AllFreezeLog { get; set; }
            [JsonProperty(PropertyName = "unfreezeall")]
            public bool AllUnfreezeLog { get; set; }
        }

        public class PlayerAdministrationLoggingOptions
        {
            [JsonProperty(PropertyName = "OpenPadminCmd")]
            public bool OpenPadminCmdLog { get; set; }
            [JsonProperty(PropertyName = "ClosePadminCmd")]
            public bool ClosePadminCmdLog { get; set; }
            [JsonProperty(PropertyName = "BanUserCmd")]
            public bool BanUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "UnbanUserCmd")]
            public bool UnbanUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "KickUserCmd")]
            public bool KickUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "MuteUserCmd")]
            public bool MuteUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "UnmuteUserCmd")]
            public bool UnmuteUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "FreezeCmd")]
            public bool FreezeCmdLog { get; set; }
            [JsonProperty(PropertyName = "UnreezeCmd")]
            public bool UnreezeCmdLog { get; set; }
            [JsonProperty(PropertyName = "BackpackViewCmd")]
            public bool BackpackViewCmdLog { get; set; }
            [JsonProperty(PropertyName = "InventoryViewCmd")]
            public bool InventoryViewCmdLog { get; set; }
            [JsonProperty(PropertyName = "ClearUserInventoryCmd")]
            public bool ClearUserInventoryCmdLog { get; set; }
            [JsonProperty(PropertyName = "ResetUserBPCmd")]
            public bool ResetUserBPCmdLog { get; set; }
            [JsonProperty(PropertyName = "ResetUserMetabolismCmd")]
            public bool ResetUserMetabolismCmdLog { get; set; }
            [JsonProperty(PropertyName = "RecoverUserMetabolismCmd")]
            public bool RecoverUserMetabolismLog { get; set; }
            [JsonProperty(PropertyName = "TeleportToUserCmd")]
            public bool TeleportToUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "TeleportUserCmd")]
            public bool TeleportUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "SpectateUserCmd")]
            public bool SpectateUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "PermsCmd")]
            public bool PermsCmdLog { get; set; }
            [JsonProperty(PropertyName = "HurtUserCmd")]
            public bool HurtUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "KillUserCmd")]
            public bool KillUserCmdLog { get; set; }
            [JsonProperty(PropertyName = "HealUserCmd")]
            public bool HealUserCmdLog { get; set; }
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private ConfigData GetBaseConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
     void OnServerInitialized();
     void Unload();
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnServerCommand(ConsoleSystem.Arg arg);
     void OnPlayerCommand(BasePlayer player, string command, string[] args);
     void OnPlayerSpectateEnd(BasePlayer player, string spectateFilter);
    private void ClientSideCommandDetection(BasePlayer player);
    private void EventsLogging(ConsoleSystem.Arg arg);
    private void KillPlayerLogging(ConsoleSystem.Arg arg);
    private void KickLogging(ConsoleSystem.Arg arg);
    private void BanLogging(ConsoleSystem.Arg arg);
    private void MuteLogging(ConsoleSystem.Arg arg);
    private void SpectateLogging(ConsoleSystem.Arg arg);
    private void TeleportLogging(ConsoleSystem.Arg arg);
    private void GiveItemLogging(ConsoleSystem.Arg arg);
    private void SpawnLogging(ConsoleSystem.Arg arg);
    private void EntityLogging(ConsoleSystem.Arg arg);
    private void VanishLogging(BasePlayer player);
    private void AdminRadarLogging(BasePlayer player);
    private void NightVisionLogging(BasePlayer player);
    private void ConvertStatusLogging(BasePlayer player);
    private void FreezeLogging(ConsoleSystem.Arg arg);
    private void FreezeLogging(BasePlayer player, string command, string[] args);
    private void InventoryViewerLogging(BasePlayer player, string[] args);
    private void BackpacksLogging(BasePlayer player, string[] args);
    private void PadminLogging(BasePlayer player);
    private void PadminLogging(ConsoleSystem.Arg arg);
    private void HandlePlayers();
    private void Log(string filename, string key, object[] args);
    private string Lang(string key, string id, object[] args);
    private void DiscordPost(string message);
    private IEnumerator HandleUpload(string url, WWWForm data);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Log to console (true/false)")]
    public bool LogToConsole { get; set; }
    [JsonProperty(PropertyName = "Update frequency (s)")]
    public float UpdateFreq { get; set; }
    [JsonProperty(PropertyName = "Log filename")]
    public string LogFileName { get; set; }
    [JsonProperty(PropertyName = "Enable Discord Messages (true/false)")]
    public bool DiscordLog { get; set; }
    [JsonProperty(PropertyName = "Discord Messages webhook")]
    public string DiscordWebhook { get; set; }
    [JsonProperty(PropertyName = "Exclude List")]
    public List<string> ExcludeList { get; set; }
    [JsonProperty(PropertyName = "Default admin commands")]
    public DefaultCommandsOptions DefaultCommands { get; set; }
    [JsonProperty(PropertyName = "Admin plugins")]
    public PluginsCommandsOptions PluginsCommands { get; set; }
    public class DefaultCommandsOptions
    {
        [JsonProperty(PropertyName = "Admin connections logging (true/false)")]
        public bool ConnectionLog { get; set; }
        [JsonProperty(PropertyName = "Noclip logging (true/false)")]
        public bool NoclipLog { get; set; }
        [JsonProperty(PropertyName = "GodMode logging (true/false)")]
        public bool GodmodeLog { get; set; }
        [JsonProperty(PropertyName = "Spectate logging (true/false)")]
        public bool SpectateLog { get; set; }
        [JsonProperty(PropertyName = "Kill player logging (true/false)")]
        public bool KillPlayerLog { get; set; }
        [JsonProperty(PropertyName = "Admin events logging (true/false)")]
        public bool EventsAllLog { get; set; }
        [JsonProperty(PropertyName = "Admin event commands")]
        public EventsLoggingOptions EventsLogging { get; set; }
        [JsonProperty(PropertyName = "Kick logging (true/false)")]
        public bool KickAllLog { get; set; }
        [JsonProperty(PropertyName = "Kick commands")]
        public KickLoggingOptions KickLogging { get; set; }
        [JsonProperty(PropertyName = "Ban logging (true/false)")]
        public bool BanAllLog { get; set; }
        [JsonProperty(PropertyName = "Ban commands")]
        public BanLoggingOptions BanLogging { get; set; }
        [JsonProperty(PropertyName = "Mute logging (true/false)")]
        public bool MuteAllLog { get; set; }
        [JsonProperty(PropertyName = "Mute commands")]
        public MuteLoggingOptions MuteLogging { get; set; }
        [JsonProperty(PropertyName = "Entity logging (true/false)")]
        public bool EntAllLog { get; set; }
        [JsonProperty(PropertyName = "Entity commands")]
        public EntityLoggingOptions EntityLogging { get; set; }
        [JsonProperty(PropertyName = "Teleport logging (true/false)")]
        public bool TeleportAllLog { get; set; }
        [JsonProperty(PropertyName = "Teleport commands")]
        public TeleportLoggingOptions TeleportLogging { get; set; }
        [JsonProperty(PropertyName = "Give items logging (true/false)")]
        public bool GiveAllLog { get; set; }
        [JsonProperty(PropertyName = "Give commands")]
        public GiveLoggingOptions GiveLogging { get; set; }
        [JsonProperty(PropertyName = "Spawn logging (true/false)")]
        public bool SpawnAllLog { get; set; }
        [JsonProperty(PropertyName = "Spawn commands")]
        public SpawnLoggingOptions SpawnLogging { get; set; }
    }

    public class PluginsCommandsOptions
    {
        [JsonProperty(PropertyName = "Vanish logging (true/false)")]
        public bool VanishLog { get; set; }
        [JsonProperty(PropertyName = "Admin Radar logging (true/false)")]
        public bool RadarLog { get; set; }
        [JsonProperty(PropertyName = "Night Vision logging (true/false)")]
        public bool NightLog { get; set; }
        [JsonProperty(PropertyName = "Convert Status logging (true/false)")]
        public bool ConvertLog { get; set; }
        [JsonProperty(PropertyName = "Inventory Viewer logging (true/false)")]
        public bool InventoryViewerLog { get; set; }
        [JsonProperty(PropertyName = "Backpacks logging (true/false)")]
        public bool BackpacksLog { get; set; }
        [JsonProperty(PropertyName = "Freeze logging (true/false)")]
        public bool FreezeAllLog { get; set; }
        [JsonProperty(PropertyName = "Freeze commands")]
        public FreezeLoggingOptions FreezeLogging { get; set; }
        [JsonProperty(PropertyName = "Player Administration logging (true/false)")]
        public bool PlayerAdministrationAllLog { get; set; }
        [JsonProperty(PropertyName = "Player Administration commands")]
        public PlayerAdministrationLoggingOptions PlayerAdministrationLogging { get; set; }
    }

    public class EventsLoggingOptions
    {
        [JsonProperty(PropertyName = "[Attack Heli] heli.call")]
        public bool HeliCallLog { get; set; }
        [JsonProperty(PropertyName = "[Attack Heli] heli.calltome")]
        public bool HeliCallToMeLog { get; set; }
        [JsonProperty(PropertyName = "[Attack Heli] drop")]
        public bool HeliDropLog { get; set; }
        [JsonProperty(PropertyName = "[Airdrop] supply.call")]
        public bool AirdropRandomLog { get; set; }
        [JsonProperty(PropertyName = "[Airdrop] supply.drop")]
        public bool AirdropPosLog { get; set; }
    }

    public class KickLoggingOptions
    {
        [JsonProperty(PropertyName = "kick")]
        public bool KickLog { get; set; }
        [JsonProperty(PropertyName = "kickall")]
        public bool KickEveryoneLog { get; set; }
    }

    public class BanLoggingOptions
    {
        [JsonProperty(PropertyName = "ban")]
        public bool BanLog { get; set; }
        [JsonProperty(PropertyName = "unban")]
        public bool UnbanLog { get; set; }
    }

    public class MuteLoggingOptions
    {
        [JsonProperty(PropertyName = "mute")]
        public bool MuteLog { get; set; }
        [JsonProperty(PropertyName = "unmute")]
        public bool UnmuteLog { get; set; }
    }

    public class EntityLoggingOptions
    {
        [JsonProperty(PropertyName = "ent kill")]
        public bool EntKillLog { get; set; }
        [JsonProperty(PropertyName = "ent who")]
        public bool EntWhoLog { get; set; }
        [JsonProperty(PropertyName = "ent lock")]
        public bool EntLockLog { get; set; }
        [JsonProperty(PropertyName = "ent unlock")]
        public bool EntUnlockLog { get; set; }
        [JsonProperty(PropertyName = "ent auth")]
        public bool EntAuthLog { get; set; }
    }

    public class TeleportLoggingOptions
    {
        [JsonProperty(PropertyName = "teleport")]
        public bool TeleportLog { get; set; }
        [JsonProperty(PropertyName = "teleportpos")]
        public bool TeleportPosLog { get; set; }
        [JsonProperty(PropertyName = "teleport2me")]
        public bool TeleportToMeLog { get; set; }
    }

    public class GiveLoggingOptions
    {
        [JsonProperty(PropertyName = "give")]
        public bool GiveLog { get; set; }
        [JsonProperty(PropertyName = "giveid")]
        public bool GiveIdLog { get; set; }
        [JsonProperty(PropertyName = "givearm")]
        public bool GiveArmLog { get; set; }
        [JsonProperty(PropertyName = "giveto")]
        public bool GiveToLog { get; set; }
        [JsonProperty(PropertyName = "giveall")]
        public bool GiveAllLog { get; set; }
    }

    public class SpawnLoggingOptions
    {
        [JsonProperty(PropertyName = "spawn")]
        public bool SpawnLog { get; set; }
        [JsonProperty(PropertyName = "spawnat")]
        public bool SpawnAtLog { get; set; }
        [JsonProperty(PropertyName = "spawnhere")]
        public bool SpawnHereLog { get; set; }
        [JsonProperty(PropertyName = "spawnitem")]
        public bool SpawnItemLog { get; set; }
    }

    public class FreezeLoggingOptions
    {
        [JsonProperty(PropertyName = "freeze")]
        public bool FreezeLog { get; set; }
        [JsonProperty(PropertyName = "unfreeze")]
        public bool UnfreezeLog { get; set; }
        [JsonProperty(PropertyName = "freezeall")]
        public bool AllFreezeLog { get; set; }
        [JsonProperty(PropertyName = "unfreezeall")]
        public bool AllUnfreezeLog { get; set; }
    }

    public class PlayerAdministrationLoggingOptions
    {
        [JsonProperty(PropertyName = "OpenPadminCmd")]
        public bool OpenPadminCmdLog { get; set; }
        [JsonProperty(PropertyName = "ClosePadminCmd")]
        public bool ClosePadminCmdLog { get; set; }
        [JsonProperty(PropertyName = "BanUserCmd")]
        public bool BanUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "UnbanUserCmd")]
        public bool UnbanUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "KickUserCmd")]
        public bool KickUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "MuteUserCmd")]
        public bool MuteUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "UnmuteUserCmd")]
        public bool UnmuteUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "FreezeCmd")]
        public bool FreezeCmdLog { get; set; }
        [JsonProperty(PropertyName = "UnreezeCmd")]
        public bool UnreezeCmdLog { get; set; }
        [JsonProperty(PropertyName = "BackpackViewCmd")]
        public bool BackpackViewCmdLog { get; set; }
        [JsonProperty(PropertyName = "InventoryViewCmd")]
        public bool InventoryViewCmdLog { get; set; }
        [JsonProperty(PropertyName = "ClearUserInventoryCmd")]
        public bool ClearUserInventoryCmdLog { get; set; }
        [JsonProperty(PropertyName = "ResetUserBPCmd")]
        public bool ResetUserBPCmdLog { get; set; }
        [JsonProperty(PropertyName = "ResetUserMetabolismCmd")]
        public bool ResetUserMetabolismCmdLog { get; set; }
        [JsonProperty(PropertyName = "RecoverUserMetabolismCmd")]
        public bool RecoverUserMetabolismLog { get; set; }
        [JsonProperty(PropertyName = "TeleportToUserCmd")]
        public bool TeleportToUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "TeleportUserCmd")]
        public bool TeleportUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "SpectateUserCmd")]
        public bool SpectateUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "PermsCmd")]
        public bool PermsCmdLog { get; set; }
        [JsonProperty(PropertyName = "HurtUserCmd")]
        public bool HurtUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "KillUserCmd")]
        public bool KillUserCmdLog { get; set; }
        [JsonProperty(PropertyName = "HealUserCmd")]
        public bool HealUserCmdLog { get; set; }
    }

}

public class DefaultCommandsOptions
{
    [JsonProperty(PropertyName = "Admin connections logging (true/false)")]
    public bool ConnectionLog { get; set; }
    [JsonProperty(PropertyName = "Noclip logging (true/false)")]
    public bool NoclipLog { get; set; }
    [JsonProperty(PropertyName = "GodMode logging (true/false)")]
    public bool GodmodeLog { get; set; }
    [JsonProperty(PropertyName = "Spectate logging (true/false)")]
    public bool SpectateLog { get; set; }
    [JsonProperty(PropertyName = "Kill player logging (true/false)")]
    public bool KillPlayerLog { get; set; }
    [JsonProperty(PropertyName = "Admin events logging (true/false)")]
    public bool EventsAllLog { get; set; }
    [JsonProperty(PropertyName = "Admin event commands")]
    public EventsLoggingOptions EventsLogging { get; set; }
    [JsonProperty(PropertyName = "Kick logging (true/false)")]
    public bool KickAllLog { get; set; }
    [JsonProperty(PropertyName = "Kick commands")]
    public KickLoggingOptions KickLogging { get; set; }
    [JsonProperty(PropertyName = "Ban logging (true/false)")]
    public bool BanAllLog { get; set; }
    [JsonProperty(PropertyName = "Ban commands")]
    public BanLoggingOptions BanLogging { get; set; }
    [JsonProperty(PropertyName = "Mute logging (true/false)")]
    public bool MuteAllLog { get; set; }
    [JsonProperty(PropertyName = "Mute commands")]
    public MuteLoggingOptions MuteLogging { get; set; }
    [JsonProperty(PropertyName = "Entity logging (true/false)")]
    public bool EntAllLog { get; set; }
    [JsonProperty(PropertyName = "Entity commands")]
    public EntityLoggingOptions EntityLogging { get; set; }
    [JsonProperty(PropertyName = "Teleport logging (true/false)")]
    public bool TeleportAllLog { get; set; }
    [JsonProperty(PropertyName = "Teleport commands")]
    public TeleportLoggingOptions TeleportLogging { get; set; }
    [JsonProperty(PropertyName = "Give items logging (true/false)")]
    public bool GiveAllLog { get; set; }
    [JsonProperty(PropertyName = "Give commands")]
    public GiveLoggingOptions GiveLogging { get; set; }
    [JsonProperty(PropertyName = "Spawn logging (true/false)")]
    public bool SpawnAllLog { get; set; }
    [JsonProperty(PropertyName = "Spawn commands")]
    public SpawnLoggingOptions SpawnLogging { get; set; }
}

public class PluginsCommandsOptions
{
    [JsonProperty(PropertyName = "Vanish logging (true/false)")]
    public bool VanishLog { get; set; }
    [JsonProperty(PropertyName = "Admin Radar logging (true/false)")]
    public bool RadarLog { get; set; }
    [JsonProperty(PropertyName = "Night Vision logging (true/false)")]
    public bool NightLog { get; set; }
    [JsonProperty(PropertyName = "Convert Status logging (true/false)")]
    public bool ConvertLog { get; set; }
    [JsonProperty(PropertyName = "Inventory Viewer logging (true/false)")]
    public bool InventoryViewerLog { get; set; }
    [JsonProperty(PropertyName = "Backpacks logging (true/false)")]
    public bool BackpacksLog { get; set; }
    [JsonProperty(PropertyName = "Freeze logging (true/false)")]
    public bool FreezeAllLog { get; set; }
    [JsonProperty(PropertyName = "Freeze commands")]
    public FreezeLoggingOptions FreezeLogging { get; set; }
    [JsonProperty(PropertyName = "Player Administration logging (true/false)")]
    public bool PlayerAdministrationAllLog { get; set; }
    [JsonProperty(PropertyName = "Player Administration commands")]
    public PlayerAdministrationLoggingOptions PlayerAdministrationLogging { get; set; }
}

public class EventsLoggingOptions
{
    [JsonProperty(PropertyName = "[Attack Heli] heli.call")]
    public bool HeliCallLog { get; set; }
    [JsonProperty(PropertyName = "[Attack Heli] heli.calltome")]
    public bool HeliCallToMeLog { get; set; }
    [JsonProperty(PropertyName = "[Attack Heli] drop")]
    public bool HeliDropLog { get; set; }
    [JsonProperty(PropertyName = "[Airdrop] supply.call")]
    public bool AirdropRandomLog { get; set; }
    [JsonProperty(PropertyName = "[Airdrop] supply.drop")]
    public bool AirdropPosLog { get; set; }
}

public class KickLoggingOptions
{
    [JsonProperty(PropertyName = "kick")]
    public bool KickLog { get; set; }
    [JsonProperty(PropertyName = "kickall")]
    public bool KickEveryoneLog { get; set; }
}

public class BanLoggingOptions
{
    [JsonProperty(PropertyName = "ban")]
    public bool BanLog { get; set; }
    [JsonProperty(PropertyName = "unban")]
    public bool UnbanLog { get; set; }
}

public class MuteLoggingOptions
{
    [JsonProperty(PropertyName = "mute")]
    public bool MuteLog { get; set; }
    [JsonProperty(PropertyName = "unmute")]
    public bool UnmuteLog { get; set; }
}

public class EntityLoggingOptions
{
    [JsonProperty(PropertyName = "ent kill")]
    public bool EntKillLog { get; set; }
    [JsonProperty(PropertyName = "ent who")]
    public bool EntWhoLog { get; set; }
    [JsonProperty(PropertyName = "ent lock")]
    public bool EntLockLog { get; set; }
    [JsonProperty(PropertyName = "ent unlock")]
    public bool EntUnlockLog { get; set; }
    [JsonProperty(PropertyName = "ent auth")]
    public bool EntAuthLog { get; set; }
}

public class TeleportLoggingOptions
{
    [JsonProperty(PropertyName = "teleport")]
    public bool TeleportLog { get; set; }
    [JsonProperty(PropertyName = "teleportpos")]
    public bool TeleportPosLog { get; set; }
    [JsonProperty(PropertyName = "teleport2me")]
    public bool TeleportToMeLog { get; set; }
}

public class GiveLoggingOptions
{
    [JsonProperty(PropertyName = "give")]
    public bool GiveLog { get; set; }
    [JsonProperty(PropertyName = "giveid")]
    public bool GiveIdLog { get; set; }
    [JsonProperty(PropertyName = "givearm")]
    public bool GiveArmLog { get; set; }
    [JsonProperty(PropertyName = "giveto")]
    public bool GiveToLog { get; set; }
    [JsonProperty(PropertyName = "giveall")]
    public bool GiveAllLog { get; set; }
}

public class SpawnLoggingOptions
{
    [JsonProperty(PropertyName = "spawn")]
    public bool SpawnLog { get; set; }
    [JsonProperty(PropertyName = "spawnat")]
    public bool SpawnAtLog { get; set; }
    [JsonProperty(PropertyName = "spawnhere")]
    public bool SpawnHereLog { get; set; }
    [JsonProperty(PropertyName = "spawnitem")]
    public bool SpawnItemLog { get; set; }
}

public class FreezeLoggingOptions
{
    [JsonProperty(PropertyName = "freeze")]
    public bool FreezeLog { get; set; }
    [JsonProperty(PropertyName = "unfreeze")]
    public bool UnfreezeLog { get; set; }
    [JsonProperty(PropertyName = "freezeall")]
    public bool AllFreezeLog { get; set; }
    [JsonProperty(PropertyName = "unfreezeall")]
    public bool AllUnfreezeLog { get; set; }
}

public class PlayerAdministrationLoggingOptions
{
    [JsonProperty(PropertyName = "OpenPadminCmd")]
    public bool OpenPadminCmdLog { get; set; }
    [JsonProperty(PropertyName = "ClosePadminCmd")]
    public bool ClosePadminCmdLog { get; set; }
    [JsonProperty(PropertyName = "BanUserCmd")]
    public bool BanUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "UnbanUserCmd")]
    public bool UnbanUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "KickUserCmd")]
    public bool KickUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "MuteUserCmd")]
    public bool MuteUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "UnmuteUserCmd")]
    public bool UnmuteUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "FreezeCmd")]
    public bool FreezeCmdLog { get; set; }
    [JsonProperty(PropertyName = "UnreezeCmd")]
    public bool UnreezeCmdLog { get; set; }
    [JsonProperty(PropertyName = "BackpackViewCmd")]
    public bool BackpackViewCmdLog { get; set; }
    [JsonProperty(PropertyName = "InventoryViewCmd")]
    public bool InventoryViewCmdLog { get; set; }
    [JsonProperty(PropertyName = "ClearUserInventoryCmd")]
    public bool ClearUserInventoryCmdLog { get; set; }
    [JsonProperty(PropertyName = "ResetUserBPCmd")]
    public bool ResetUserBPCmdLog { get; set; }
    [JsonProperty(PropertyName = "ResetUserMetabolismCmd")]
    public bool ResetUserMetabolismCmdLog { get; set; }
    [JsonProperty(PropertyName = "RecoverUserMetabolismCmd")]
    public bool RecoverUserMetabolismLog { get; set; }
    [JsonProperty(PropertyName = "TeleportToUserCmd")]
    public bool TeleportToUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "TeleportUserCmd")]
    public bool TeleportUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "SpectateUserCmd")]
    public bool SpectateUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "PermsCmd")]
    public bool PermsCmdLog { get; set; }
    [JsonProperty(PropertyName = "HurtUserCmd")]
    public bool HurtUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "KillUserCmd")]
    public bool KillUserCmdLog { get; set; }
    [JsonProperty(PropertyName = "HealUserCmd")]
    public bool HealUserCmdLog { get; set; }
}


```

---

## AdminNoLoot by VisEntities - Protects admins' corpses and bodies from being looted

```csharp
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Admin No Loot", "Dana", "0.1.3")]
[Description("Protects admins' corpses and bodies from being looted.")]
public class AdminNoLoot : RustPlugin
{
    private PluginConfig _pluginConfig;
    public const string PermissionBypass;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private void OnServerInitialized();
    protected override void LoadDefaultMessages();
     object CanLootPlayer(BasePlayer target, BasePlayer looter);
     object CanLootEntity(BasePlayer player, DroppedItemContainer container);
     object CanLootEntity(BasePlayer player, LootableCorpse corpse);
    public class MessageManager
    {
        public const string NoCorpseLootPermission;
        public const string NoBagLootPermission;
        public const string NoBodyLootPermission;
    }

    private class PluginConfig
    {
        public AdminNoLootConfig Config { get; set; }
    }

    private class AdminNoLootConfig
    {
        [JsonProperty(PropertyName = "Warning - Enabled")]
        public bool ShowWarning { get; set; }
    }

}

public class MessageManager
{
    public const string NoCorpseLootPermission;
    public const string NoBagLootPermission;
    public const string NoBodyLootPermission;
}

private class PluginConfig
{
    public AdminNoLootConfig Config { get; set; }
}

private class AdminNoLootConfig
{
    [JsonProperty(PropertyName = "Warning - Enabled")]
    public bool ShowWarning { get; set; }
}


```

---

## AdminPanel by nivex - A small GUI panel of clickable commands for admins

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Admin Panel", "nivex", "1.4.7")]
[Description("GUI admin panel with command buttons")]
 class AdminPanel : RustPlugin
{
    [PluginReference]
    private Plugin AdminRadar;
    private Plugin Godmode;
    private Plugin Vanish;
    private Plugin PlayerAdministration;
    private const string permAdminPanel;
    private const string permAdminRadar;
    private const string permGodmode;
    private const string permVanish;
    private const string permPlayerAdministration;
    private const string permAutoToggle;
    public Dictionary<BasePlayer, string> playerCUI;
    public class StoredData
    {
        public Dictionary<string, string> TP;
        public StoredData();
    }

    public StoredData data;
    private List<string> _playerAdministration;
    private bool IsPlayerAdministration(string UserID);
    private void TogglePlayerAdministration(BasePlayer player);
    private void OnPlayerCommand(BasePlayer player, string command, string[] args);
    private void OnServerCommand(ConsoleSystem.Arg arg);
    private bool IsGod(string UserID);
    private void ToggleGodmode(BasePlayer player);
    private void OnGodmodeToggle(string playerId, bool state);
    private bool IsInvisible(BasePlayer player);
    private void ToggleVanish(BasePlayer player);
    private void OnVanishDisappear(BasePlayer player);
    private void OnVanishReappear(BasePlayer player);
    private bool IsRadar(string id);
    private void ToggleRadar(BasePlayer player);
    private void OnRadarActivated(BasePlayer player);
    private void OnRadarDeactivated(BasePlayer player);
    private void Init();
    private void OnUserGroupRemoved(string id, string group);
    private void OnUserGroupAdded(string id, string group);
    private new void LoadDefaultMessages();
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerDeath(BasePlayer player);
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnServerInitialized();
    private void SaveData();
    private void RefreshAllUI();
    [ConsoleCommand("adminpanel")]
    private void ccmdAdminPanel(ConsoleSystem.Arg arg);
    [ChatCommand("adminpanel")]
    private void ccmdAdminPanel(BasePlayer player, string command, string[] args);
    private void AdminGui(BasePlayer player);
    private void Unload();
    private void DestroyUI(BasePlayer player);
    private bool IsAllowed(BasePlayer player, string perm);
    private string _(string key, string id, object[] args);
    private void Message(BasePlayer player, string key, object[] args);
    private Configuration config;
    public class ButtonState
    {
        [JsonProperty(PropertyName = "Button Enabled")]
        public bool enabled { get; set; }
        [JsonProperty(PropertyName = "Button Color")]
        public string color { get; set; }
    }

    public class Configuration
    {
        [JsonProperty(PropertyName = "newtp")]
        public ButtonState newtp { get; set; }
        [JsonProperty(PropertyName = "Admin Zone Coordinates")]
        public Vector3 adminZoneCords { get; set; }
        [JsonProperty(PropertyName = "Button Active Color")]
        public string btnActColor { get; set; }
        [JsonProperty(PropertyName = "Button Inactive Color")]
        public string btnInactColor { get; set; }
        [JsonProperty(PropertyName = "Font Size")]
        public int fontSize { get; set; }
        [JsonProperty(PropertyName = "Panel Pos Max")]
        public string PanelPosMax { get; set; }
        [JsonProperty(PropertyName = "Panel Pos Min")]
        public string PanelPosMin { get; set; }
        [JsonProperty(PropertyName = "Toggle Mode")]
        public bool ToggleMode { get; set; }
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
}

public class StoredData
{
    public Dictionary<string, string> TP;
    public StoredData();
}

public class ButtonState
{
    [JsonProperty(PropertyName = "Button Enabled")]
    public bool enabled { get; set; }
    [JsonProperty(PropertyName = "Button Color")]
    public string color { get; set; }
}

public class Configuration
{
    [JsonProperty(PropertyName = "newtp")]
    public ButtonState newtp { get; set; }
    [JsonProperty(PropertyName = "Admin Zone Coordinates")]
    public Vector3 adminZoneCords { get; set; }
    [JsonProperty(PropertyName = "Button Active Color")]
    public string btnActColor { get; set; }
    [JsonProperty(PropertyName = "Button Inactive Color")]
    public string btnInactColor { get; set; }
    [JsonProperty(PropertyName = "Font Size")]
    public int fontSize { get; set; }
    [JsonProperty(PropertyName = "Panel Pos Max")]
    public string PanelPosMax { get; set; }
    [JsonProperty(PropertyName = "Panel Pos Min")]
    public string PanelPosMin { get; set; }
    [JsonProperty(PropertyName = "Toggle Mode")]
    public bool ToggleMode { get; set; }
}


```

---

## AdminRadar by nivex - Allows admins to have a radar to help detect cheaters and other entities

```csharp
using Facepunch;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust.Ai.Gen2;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.Globalization;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;
using static Oxide.Plugins.AdminRadarExtensionMethods.ExtensionMethods;

Oxide.Plugins
[Info("Admin Radar", "nivex", "5.4.0")]
[Description("Radar tool for Admins and Developers.")]
internal class AdminRadar : RustPlugin
{
    [PluginReference]
     Plugin Clans;
     Plugin Backpacks;
     Plugin DiscordMessages;
    private List<string> _tags;
    private List<EntityType> _errorTypes;
    private List<Radar> _radars;
    private List<BaseEntity> _spawnedEntities;
    private Dictionary<NetworkableId, Vector3> _despawnedEntities;
    private Dictionary<NetworkableId, BaseEntity> _allEntities;
    private Dictionary<string, float> _cooldowns;
    private Dictionary<ulong, string> _clans;
    private Dictionary<ulong, string> _teamColors;
    private Dictionary<string, string> _clanColors;
    private Array _allEntityTypes;
    private CoroutineTimer _coroutineTimer;
    private Stack<Coroutine> _coroutines;
    private StoredData data;
    private bool _isPopulatingCache;
    private bool isUnloading;
    private Cache cache;
    private class StoredData
    {
        public Dictionary<ulong, UiOffsets> Offsets;
        public Dictionary<string, int> EntityTextSize;
        public Dictionary<string, int> EntityNameSize;
        public Dictionary<string, int> PlayerTextSize;
        public Dictionary<string, int> PlayerNameSize;
        public List<string> Extended;
        public Dictionary<string, List<string>> Filters;
        public List<string> Hidden;
        public List<string> OnlineBoxes;
        public List<string> Visions;
        public List<string> Active;
        public StoredData();
        public void Init();
    }

    private class Cache
    {
        public Cache(AdminRadar instance);
        public Configuration config;
        public AdminRadar instance;
        internal EntityType entityType;
        public Dictionary<NetworkableId, EntityInfo> Airdrops { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Animals { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Backpacks { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Bags { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Boats { get; set; }
        public Dictionary<NetworkableId, EntityInfo> BradleyAPCs { get; set; }
        public Dictionary<NetworkableId, EntityInfo> CargoPlanes { get; set; }
        public Dictionary<NetworkableId, EntityInfo> CargoShips { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Cars { get; set; }
        public Dictionary<NetworkableId, EntityInfo> CCTV { get; set; }
        public Dictionary<NetworkableId, EntityInfo> CH47 { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Cupboards { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Collectibles { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Containers { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Corpses { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Drops { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Helicopters { get; set; }
        public Dictionary<NetworkableId, EntityInfo> MiniCopter { get; set; }
        public Dictionary<NetworkableId, EntityInfo> MLRS { get; set; }
        public Dictionary<NetworkableId, EntityInfo> NPCPlayers { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Ores { get; set; }
        public Dictionary<NetworkableId, EntityInfo> RHIB { get; set; }
        public Dictionary<NetworkableId, EntityInfo> RidableHorse { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Turrets { get; set; }
        public Dictionary<NetworkableId, EntityInfo> Traps { get; set; }
        public bool Add(BaseEntity entity);
        private bool TryGetContainerType(BaseEntity entity, EntityType type);
        public bool Remove(NetworkableId nid, Vector3 entityPos);
        private bool Add_Internal(Dictionary<NetworkableId, EntityInfo> cachedList, BaseEntity entity, EntityType type);
        private bool Remove_Internal(Dictionary<TKeyType, TType> cachedList, TKeyType key);
        public bool IsDrop(BaseEntity entity);
        public bool IsTrap(BaseNetworkable entity);
        public bool IsLoot(BaseNetworkable entity);
        public bool IsBox(BaseNetworkable entity);
    }

    private class CoroutineTimer
    {
        private Stopwatch stopwatch;
        private float _maxDurationMs;
        private bool _isRunning;
        public double Elapsed { get; set; }
        public CoroutineTimer(float maxDurationMs);
        public void Start();
        public bool ShouldYield();
        public void ResetIfYielded();
    }

    internal static class StringBuilderCache
    {
        [ThreadStatic]
        private static StringBuilder builder;
        public static StringBuilder Acquire(string text);
        public static void Clear();
        public static string GetStringAndRelease(StringBuilder sb);
    }

    public class EntityInfo
    {
        public BuildingPrivlidge priv;
        public BaseEntity entity;
        public EntityType type;
        public Color color;
        public Vector3 _from;
        public Vector3 to;
        public Vector3 offset;
        public string name;
        public object info;
        public float dist;
        public float sqrdist;
        public float size;
        public Transform t;
        public Network.Visibility.Group group { get; set; }
        public Vector3 from { get; set; }
        public EntityInfo();
        public EntityInfo(BaseEntity entity, EntityType type, Func<EntityType, BaseEntity, float> getDistance, Func<string, string> stripTags);
    }

    private class Radar : FacepunchBehaviour
    {
        private static Func<char, bool> abbr;
        public class DataObject : Pool.IPooled
        {
            public EntityInfo ei;
            public Action action;
            public DrawFlags flags;
            public bool disabled;
            public DataObject();
            public bool HasFlag(DrawFlags flag);
            public void SetEnabled(Network.Visibility.Group group, Vector3 to, float max);
            public bool IsOfType(EntityType type);
            public void Reset();
            public void EnterPool();
            public void LeavePool();
        }

        internal class DistantPlayer : Pool.IPooled
        {
            public Vector3 pos;
            public bool alive;
            public DistantPlayer();
            public void Reset();
            public void EnterPool();
            public void LeavePool();
        }

        internal bool setSource;
        internal bool canGetExistingBackpacks;
        internal bool isEnabled;
        internal bool canBypassOverride;
        internal bool hasPermAllowed;
        internal bool isAdmin;
        internal bool showHT;
        internal bool showAll;
        internal int inactiveSeconds;
        internal int activatedSeconds;
        internal int checks;
        internal float currDistance;
        internal float invokeTime;
        internal float maxDistance;
        internal string username;
        internal string userid;
        internal RaycastHit hit;
        internal EntityType currType;
        internal Vector3 position;
        internal BaseEntity source;
        internal BasePlayer player;
        internal AdminRadar instance;
        internal Network.Visibility.Group group;
        internal ItemContainer _backpackItemContainer;
        internal Coroutine _radarCo;
        internal Coroutine _updateCo;
        internal Coroutine _groupCo;
        internal List<ulong> exclude;
        internal List<NetworkableId> removeByNetworkId;
        internal List<EntityType> entityTypes;
        internal List<DistantPlayer> distant;
        internal List<EntityType> removeByEntityType;
        internal Dictionary<NetworkableId, DataObject> data;
        internal Dictionary<EntityType, Action> filters;
        internal Dictionary<ulong, ItemContainer> backpacks;
        internal Dictionary<ulong, float> Voices;
        internal List<ulong> VoiceExpirations;
        internal Cache Cache { get; set; }
        internal Configuration config { get; set; }
        internal float delay { get; set; }
        internal float Distance(Vector3 a);
        public Vector3 limitUp;
        public Vector3 halfUp;
        public Vector3 twoHalfUp;
        public Vector3 twoUp;
        public Vector3 fiveUp;
        private void Awake();
        private void OnDestroy();
        private void ResetToPool();
        public bool Add(EntityType type);
        public void Init(AdminRadar instance);
        public void StopAll();
        public bool GetBool(EntityType type);
        private void Activity();
        private class Idle
        {
            public BasePlayer player;
            public Vector3 lastPosition;
            public float lastMovementTime;
        }

        private Dictionary<ulong, Idle> _idle;
        private double GetIdleTime(BasePlayer target);
        private void RemoveIdleTime();
        private void SetupFilter(EntityType type, Action action);
        public void SetupFilters(bool barebones);
        private void SetupFilters();
        private IEnumerator DoGroupLimitRoutine();
        private IEnumerator DoUpdateRoutine();
        private IEnumerator DoRadarRoutine();
        private void DoRemoves();
        private void CheckNetworkGroupChange();
        public void SetEnabledDataObjects();
        public void RemoveByEntityType(EntityType type);
        public void RemoveByNetworkId(NetworkableId nid);
        public void TryCacheByType(EntityType type, EntityInfo ei);
        private DataObject cobj;
        private DrawFlags cflag;
        private void DirectDrawAll();
        private void DrawVisionArrow(BasePlayer target, float dist);
        private void DrawVoiceArrow(BasePlayer target, Vector3 a, float dist);
        private void DrawArrow(Color color, Vector3 from, Vector3 to, float size, bool @override);
        private void DrawPlayerText(Color color, Vector3 position, object prefix, object text, bool @override);
        private void DrawBox(Color color, Vector3 position, float size, bool @override);
        private void CacheArrow(DataObject obj, Color color, Vector3 offset, Vector3 to, float size, bool @override);
        private void CacheBox(DataObject obj, Color color, Vector3 offset, float size, bool @override);
        private void CacheText(DataObject obj, Color color, Vector3 offset, Action action, bool @override);
        private void HandleException(NetworkableId nid, Exception ex);
        private void HandleException(Exception ex);
        private void SetAdminFlag();
        private void RemoveAdminFlag();
        private bool API_GetExistingBackpacks(ulong userid);
        public int entityNameSize;
        public int entityTextSize;
        public int playerNameSize;
        public int playerTextSize;
        private string Format(object prefix, object text, bool entity);
        private string Format(BasePlayer target, bool s);
        private bool HasPermission(string userid, string perm);
        private string GetContents(List<Item> itemList, int num);
        private string GetContents(ItemContainer[] containers, int num);
        private bool SetSource();
        private float SetDistance(Vector3 a);
        private DataObject SetDataObject(EntityInfo ei);
        private bool HasDataObject(BaseEntity entity);
        private bool IsValid(EntityInfo ei, float dist);
        private void ShowActive();
        private void CheckVoiceExpirations();
        public void TryCacheOnlinePlayer(BasePlayer target);
        private void ShowSleepers();
        private void TryCacheSleepingPlayer(BasePlayer target);
        private Color GetColor(BasePlayer target, Vector3 a);
        private void DrawAppendedText(BasePlayer target, Vector3 a, Vector3 offset, Color color);
        private string GetCheats(BasePlayer target);
        private void ShowGroupLimits();
        private void ClearDistantPlayers();
        private void DrawCupboardArrows(BasePlayer target, EntityType lastType);
        private void ShowHeli();
        private void CacheHeli(EntityInfo ei);
        private void ShowBradley();
        private void CacheBradley(EntityInfo ei);
        private void ShowTC();
        private void CacheTC(EntityInfo ei);
        private void ShowSupplyDrops();
        private void CacheAirdrop(EntityInfo ei);
        private void ShowContainer(EntityInfo ei, EntityType type);
        private void ShowBox();
        private void CacheContainer(EntityInfo ei);
        private void ShowStash();
        private void CacheStash(EntityInfo ei);
        private void ShowLoot();
        private void CacheBackpack(EntityInfo ei);
        private void CacheLoot(EntityInfo ei);
        private void ShowBags();
        private void CacheSleepingBag(EntityInfo ei);
        private void ShowTurrets();
        private void CacheTurret(EntityInfo ei);
        private void ShowDead();
        private void CacheDead(EntityInfo ei);
        private void ShowDrops();
        private void CacheDrop(EntityInfo ei);
        private void ShowNPC();
        private void CacheAnimal(EntityInfo ei);
        private void CacheNpc(EntityInfo ei);
        private void DrawVictim(BasePlayer victim, Vector3 from, Vector3 offset, Color color);
        private bool IsAtView(EntityInfo ei);
        private void ShowOre();
        private void CacheOre(EntityInfo ei);
        private void ShowCCTV();
        private void CacheCCTV(EntityInfo ei);
        private void ShowCollectibles();
        private void CacheCol(EntityInfo ei);
        private void ShowEntity(EntityType entityType, Dictionary<NetworkableId, EntityInfo> entities);
        private void CacheEntity(EntityInfo ei, EntityType entityType);
        private static float GetScale(float value);
    }

    private void StopFillCache();
    private IEnumerator FillOnEntitySpawned();
    private IEnumerator FillCache();
    private Coroutine CreateCoroutine(IEnumerator operation);
    private IEnumerator RemoveElementsFromList(CoroutineTimer timer);
    private IEnumerator AddElementsToCacheWithInfo(CoroutineTimer timer, Dictionary<NetworkableId, EntityInfo> cachedList, Func<BaseEntity, EntityInfo> getCacheInfoFunc, Func<BaseEntity, bool> condition);
    private IEnumerator AddElementsToCache(CoroutineTimer timer, Dictionary<NetworkableId, EntityInfo> cachedList, EntityType type, Func<BaseEntity, bool> condition);
    [ConsoleCommand("espgui")]
    private void ccmdESPGUI(ConsoleSystem.Arg arg);
    private void ccmdMovePosition(BasePlayer player, string command, string[] args);
    private void RadarCommand(IPlayer user, string command, string[] args);
    private void RadarCommandX(BasePlayer player, string command, string[] args);
    private void TurnRadarOn(BasePlayer player, string[] args);
    private void RadarCommandY(BasePlayer player, string command, string[] args);
    private void TrySetFontSize(BasePlayer player, string command, string[] args);
    private void Init();
    private void Unload();
    private void OnServerInitialized();
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerVoice(BasePlayer player, byte[] data);
    private void OnPlayerTrackStarted(BasePlayer player, ulong targetId);
    private void OnPlayerTrackEnded(BasePlayer player, ulong targetId);
    private void OnRadarActivated(BasePlayer player, string playerName, string playerId, Vector3 lastPosition);
    private void OnRadarDeactivated(BasePlayer player, string playerName, string playerId, Vector3 lastPosition);
    private void OnEntitySpawned(BaseEntity entity);
    private void OnEntityDeath(BaseEntity entity, HitInfo info);
    private void OnEntityKill(BaseEntity entity);
    private void OnTeamCreated(BasePlayer player, RelationshipManager.PlayerTeam team);
    private void OnClanCreate(string tag);
    private string GetClanColor(ulong targetId);
    private Dictionary<string, string> GetAllClanColors();
    private Dictionary<ulong, string> GetAllTeamColors();
    private string GetTeamColor(ulong id);
    private string GetClanOf(ulong playerId);
    private void SetupClanTeamColors();
    private bool DestroyRadar(BasePlayer player);
    private bool IsRadar(string id);
    private void TryCacheByType(EntityType type, EntityInfo ei);
    public void AdminCommand(BasePlayer player, Action action);
    private static Color __(string value);
    private string StripTags(string value);
    private bool HasAccess(BasePlayer player);
    private bool IsArg(string[] args, string val, bool equalTo);
    private void DrawBuildings(BasePlayer player, bool showNonPlayerBases, bool showTwigOnly);
    private IEnumerator FindByIDRoutine(BasePlayer player, ulong userID);
    private const float CHUNK_SIZE;
    private List<Collider> FindMapColliders(BasePlayer player);
    private IEnumerator DrawObjectsRoutine(BasePlayer player, string[] args);
    private void DrawDrops(BasePlayer player, float maxDistance);
    private void LoadData();
    private void RemoveNonAuthorizedOffsetData();
    private void SaveOffsetData();
    private void SaveData();
    private void DelayedInvoke(BasePlayer player);
    private Timer saveTimer;
    private List<ulong> isMovingUi;
    private List<string> radarUI;
    private const string RadarPanelName;
    private const double S_X;
    private const double S_Y;
    private UiOffsets DefaultOffset;
    public void DestroyUI(BasePlayer player);
    public static void AddCuiPanel(CuiElementContainer container, bool cursor, string color, string amin, string amax, string omin, string omax, string parent, string name);
    public static void AddCuiButton(CuiElementContainer container, string buttonColor, string command, string text, string textColor, int fontSize, TextAnchor align, string amin, string amax, string omin, string omax, string parent, string name, string font);
    private UiOffsets GetOffsets(BasePlayer player);
    private void ShowRadarUi(BasePlayer player, Radar radar, bool showMoveUi);
    public void ShowMoveUi(BasePlayer player, bool destroyUi);
    public SortedDictionary<string, EntityType> GetButtonNames();
    public class UiOffsets
    {
        [JsonIgnore]
        public bool changed;
        public bool Mover;
        public string Min;
        public string Max;
        public UiOffsets();
        public UiOffsets(string min, string max);
        public bool Equals(UiOffsets other);
    }

    public bool _sendDiscordMessages;
    private void AdminRadarDiscordMessage(string playerName, string playerId, bool state, Vector3 position);
    private static List<string> ItemExceptions { get; set; }
    private Configuration config;
    private string GetGroupColor(int index);
    private static Dictionary<string, string> DefaultColors { get; set; }
    protected override void LoadDefaultMessages();
    public class ConfigurationSettings
    {
        public int GetLimit(BasePlayer player);
        [JsonProperty(PropertyName = "Barebones Performance Mode")]
        public bool Barebones;
        [JsonProperty(PropertyName = "Restrict Access To Steam64 IDs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Authorized;
        [JsonProperty(PropertyName = "Restrict Access To Auth Level")]
        public int authLevel;
        [JsonProperty(PropertyName = "Max Active Filters (OWNERID)")]
        public int Owner;
        [JsonProperty(PropertyName = "Max Active Filters (MODERATORID)")]
        public int Moderator;
        [JsonProperty(PropertyName = "Max Active Filters (ADMINRADAR.ALLOWED)")]
        public int Allowed;
        [JsonProperty(PropertyName = "Default Distance")]
        public float DefaultMaxDistance;
        [JsonProperty(PropertyName = "Default Refresh Time")]
        public float DefaultInvokeTime;
        [JsonProperty(PropertyName = "Minimum Refresh Time")]
        public float MinInvokeTime;
        [JsonProperty(PropertyName = "Dropped Item Exceptions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> DropExceptions;
        [JsonProperty(PropertyName = "Deactivate Radar After X Seconds Inactive")]
        public float InactiveSeconds;
        [JsonProperty(PropertyName = "Deactivate Radar After X Seconds Activated")]
        public float DeactivateSeconds;
        [JsonProperty(PropertyName = "User Interface Enabled")]
        public bool UI;
        [JsonProperty(PropertyName = "Show Average Ping Every X Seconds [0 = disabled]")]
        public float AveragePingInterval;
        [JsonProperty(PropertyName = "Show Player Idle Time (Minutes)")]
        public bool ShowIdleTime;
        [JsonProperty(PropertyName = "Player Idle Time Visible After X Minutes")]
        public int IdleVisibleMinutes;
        [JsonProperty(PropertyName = "Player Idle Time Round To X Digits")]
        public int IdleRoundDigits;
        [JsonProperty(PropertyName = "Re-use Cooldown, Seconds")]
        public float Cooldown;
        [JsonProperty(PropertyName = "Show Radar Activated/Deactivated Messages")]
        public bool ShowToggle;
        [JsonProperty(PropertyName = "Player Name Text Size")]
        public int PlayerNameSize;
        [JsonProperty(PropertyName = "Player Information Text Size")]
        public int PlayerTextSize;
        [JsonProperty(PropertyName = "Entity Name Text Size")]
        public int EntityNameSize;
        [JsonProperty(PropertyName = "Entity Information Text Size")]
        public int EntityTextSize;
        [JsonProperty(PropertyName = "Unique Clan/Team Color Applies To Entire Player Text")]
        public bool ApplySameColor;
        [JsonProperty(PropertyName = "Track Group Name")]
        public string New;
        [JsonProperty(PropertyName = "Tracked Group Name Text")]
        public string NewText;
        [JsonProperty(PropertyName = "Chat Command")]
        public string Primary;
        [JsonProperty(PropertyName = "Second Command")]
        public string Secondary;
    }

    public class ConfigurationOptions
    {
        [JsonProperty(PropertyName = "Additional Boxes", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> AdditionalBoxes;
        [JsonProperty(PropertyName = "Additional Traps", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> AdditionalTraps;
        [JsonProperty(PropertyName = "Draw Distant Players With X")]
        public bool DrawX;
        [JsonProperty(PropertyName = "Draw Empty Containers")]
        public bool DrawEmptyContainers;
        [JsonProperty(PropertyName = "Abbreviate Item Names")]
        public bool Abbr;
        [JsonProperty(PropertyName = "Show Resource Amounts")]
        public bool ResourceAmounts;
        [JsonProperty(PropertyName = "Show X Items From Barrel And Crate")]
        public int LootContentAmount;
        [JsonProperty(PropertyName = "Show X Items From Airdrop")]
        public int AirdropContentAmount;
        [JsonProperty(PropertyName = "Show X Items From Stash")]
        public int StashContentAmount;
        [JsonProperty(PropertyName = "Show X Items From Backpacks")]
        public int BackpackContentAmount;
        [JsonProperty(PropertyName = "Show X Items From Corpses")]
        public int CorpseContentAmount;
        [JsonProperty(PropertyName = "Show NPC At World View")]
        public bool NpcWorldView;
        [JsonProperty(PropertyName = "Show NPC Name As Prefab Name")]
        public bool NpcPrefabName;
        [JsonProperty(PropertyName = "Show Authed Count On Cupboards")]
        public bool TCAuthed;
        [JsonProperty(PropertyName = "Show Bag Count On Cupboards")]
        public bool TCBags;
        [JsonProperty(PropertyName = "Show Npc Player Target")]
        public bool DrawTargetsVictim;
        [JsonProperty(PropertyName = "Radar Buildings Draw Time")]
        public float BuildingsDrawTime;
        [JsonProperty(PropertyName = "Radar Drops Draw Time")]
        public float DropsDrawTime;
        [JsonProperty(PropertyName = "Radar Find Draw Time")]
        public float FindDrawTime;
        [JsonProperty(PropertyName = "Radar FindByID Draw Time")]
        public float FindByIDDrawTime;
        public int Get(EntityType type);
    }

    public class ConfigurationDrawMethods
    {
        [JsonProperty(PropertyName = "Draw Arrows On Players")]
        public bool Arrow;
        [JsonProperty(PropertyName = "Draw Boxes")]
        public bool Box;
        [JsonProperty(PropertyName = "Draw Text")]
        public bool Text;
    }

    public class ConfigurationLimits
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Limit")]
        public int Amount;
        [JsonProperty(PropertyName = "Range")]
        public float Range;
        [JsonProperty(PropertyName = "Height Offset [0.0 = disabled]")]
        public float Height;
        [JsonProperty(PropertyName = "Use Group Colors Configuration")]
        public bool ColorsEnabled;
        [JsonProperty(PropertyName = "Dead Color")]
        public string Dead;
        [JsonProperty(PropertyName = "Group Color Basic")]
        public string Basic;
        [JsonProperty(PropertyName = "Group Limit Colors", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, string> Colors;
    }

    public class ConfigurationDrawDistances
    {
        [JsonProperty(PropertyName = "Sleepers Min Y")]
        public float MinY;
        [JsonProperty(PropertyName = "Player Corpses")]
        public float Corpse;
        [JsonProperty(PropertyName = "Players")]
        public float Players;
        [JsonProperty(PropertyName = "Airdrop Crates")]
        public float Airdrop;
        [JsonProperty(PropertyName = "Animals")]
        public float Animal;
        [JsonProperty(PropertyName = "Boats")]
        public float Boat;
        [JsonProperty(PropertyName = "Boxes")]
        public float Box;
        [JsonProperty(PropertyName = "Cars")]
        public float Cars;
        [JsonProperty(PropertyName = "CCTV")]
        public float CCTV;
        [JsonProperty(PropertyName = "Collectibles")]
        public float Col;
        [JsonProperty(PropertyName = "Loot Containers")]
        public float Loot;
        [JsonProperty(PropertyName = "MiniCopter")]
        public float MC;
        [JsonProperty(PropertyName = "MLRS")]
        public float MLRS;
        [JsonProperty(PropertyName = "NPC Players")]
        public float NPC;
        [JsonProperty(PropertyName = "Resources (Ore)")]
        public float Ore;
        [JsonProperty(PropertyName = "Ridable Horses")]
        public float RH;
        [JsonProperty(PropertyName = "Sleeping Bags")]
        public float Bag;
        [JsonProperty(PropertyName = "Stashes")]
        public float Stash;
        [JsonProperty(PropertyName = "Tool Cupboards")]
        public float TC;
        [JsonProperty(PropertyName = "Tool Cupboard Arrows")]
        public float TCArrows;
        [JsonProperty(PropertyName = "Traps")]
        public float Traps;
        [JsonProperty(PropertyName = "Turrets")]
        public float Turret;
        [JsonProperty(PropertyName = "Vending Machines")]
        public float VendingMachine;
        [JsonProperty(PropertyName = "Radar Drops Command")]
        public float Drops;
        public float Get(EntityType type, BaseEntity entity);
    }

    public class ConfigurationCoreTracking
    {
        [JsonProperty(PropertyName = "Players")]
        public bool Active;
        [JsonProperty(PropertyName = "Sleepers")]
        public bool Sleepers;
        [JsonProperty(PropertyName = "Animals")]
        public bool Animals;
        [JsonProperty(PropertyName = "Bags")]
        public bool Bags;
        [JsonProperty(PropertyName = "Box")]
        public bool Box;
        [JsonProperty(PropertyName = "Collectibles")]
        public bool Col;
        [JsonProperty(PropertyName = "Dead")]
        public bool Dead;
        [JsonProperty(PropertyName = "Loot")]
        public bool Loot;
        [JsonProperty(PropertyName = "NPC")]
        public bool NPCPlayer;
        [JsonProperty(PropertyName = "Ore")]
        public bool Ore;
        [JsonProperty(PropertyName = "Stash")]
        public bool Stash;
        [JsonProperty(PropertyName = "SupplyDrops")]
        public bool Airdrop;
        [JsonProperty(PropertyName = "TC")]
        public bool TC;
        [JsonProperty(PropertyName = "Turrets")]
        public bool Turrets;
    }

    public class ConfigurationAdditionalTracking
    {
        [JsonProperty(PropertyName = "Backpacks Plugin")]
        public bool BackpackPlugin { get; set; }
        [JsonProperty(PropertyName = "Boats")]
        public bool Boats;
        [JsonProperty(PropertyName = "Bradley APC")]
        public bool Bradley;
        [JsonProperty(PropertyName = "Cars")]
        public bool Cars;
        [JsonProperty(PropertyName = "CargoPlanes")]
        public bool CP;
        [JsonProperty(PropertyName = "CargoShips")]
        public bool CS;
        [JsonProperty(PropertyName = "CCTV")]
        public bool CCTV;
        [JsonProperty(PropertyName = "CH47")]
        public bool CH47;
        [JsonProperty(PropertyName = "Helicopters")]
        public bool Heli;
        [JsonProperty(PropertyName = "Helicopter Rotor Health")]
        public bool RotorHealth;
        [JsonProperty(PropertyName = "MiniCopter")]
        public bool MC;
        [JsonProperty(PropertyName = "MLRS")]
        public bool MLRS;
        [JsonProperty(PropertyName = "Ridable Horses")]
        public bool RH;
        [JsonProperty(PropertyName = "RHIB")]
        public bool RHIB;
        [JsonProperty(PropertyName = "Traps")]
        public bool Traps;
        public bool Get(EntityType type);
    }

    public class ConfigurationHex
    {
        [JsonProperty(PropertyName = "Player Arrows")]
        public string Arrows;
        [JsonProperty(PropertyName = "Distance")]
        public string Dist;
        [JsonProperty(PropertyName = "Helicopters")]
        public string Heli;
        [JsonProperty(PropertyName = "Bradley")]
        public string Bradley;
        [JsonProperty(PropertyName = "MiniCopter")]
        public string MC;
        [JsonProperty(PropertyName = "MiniCopter (ScrapTransportHelicopter)")]
        public string STH;
        [JsonProperty(PropertyName = "Online Player")]
        public string Online;
        [JsonProperty(PropertyName = "Online Player (Underground)")]
        public string Underground;
        [JsonProperty(PropertyName = "Online Player (Flying)")]
        public string Flying;
        [JsonProperty(PropertyName = "Online Dead Player")]
        public string OnlineDead;
        [JsonProperty(PropertyName = "Dead Player")]
        public string Dead;
        [JsonProperty(PropertyName = "Sleeping Player")]
        public string Sleeper;
        [JsonProperty(PropertyName = "Sleeping Dead Player")]
        public string SleeperDead;
        [JsonProperty(PropertyName = "Health")]
        public string Health;
        [JsonProperty(PropertyName = "Idle Time")]
        public string IdleTime;
        [JsonProperty(PropertyName = "Backpacks")]
        public string Backpack;
        [JsonProperty(PropertyName = "Scientists")]
        public string Scientist;
        [JsonProperty(PropertyName = "Scientist Peacekeeper")]
        public string Peacekeeper;
        [JsonProperty(PropertyName = "Murderers")]
        public string Murderer;
        [JsonProperty(PropertyName = "Animals")]
        public string Animal;
        [JsonProperty(PropertyName = "Resources")]
        public string Resource;
        [JsonProperty(PropertyName = "Collectibles")]
        public string Col;
        [JsonProperty(PropertyName = "Tool Cupboards")]
        public string TC;
        [JsonProperty(PropertyName = "Sleeping Bags")]
        public string Bag;
        [JsonProperty(PropertyName = "Airdrops")]
        public string AD;
        [JsonProperty(PropertyName = "AutoTurrets")]
        public string AT;
        [JsonProperty(PropertyName = "Corpses")]
        public string Corpse;
        [JsonProperty(PropertyName = "Box")]
        public string Box;
        [JsonProperty(PropertyName = "Loot")]
        public string Loot;
        [JsonProperty(PropertyName = "Stash")]
        public string Stash;
        [JsonProperty(PropertyName = "Boat")]
        public string Boat;
        [JsonProperty(PropertyName = "CargoPlane")]
        public string CP;
        [JsonProperty(PropertyName = "CargoShip")]
        public string CS;
        [JsonProperty(PropertyName = "Car")]
        public string Cars;
        [JsonProperty(PropertyName = "CCTV")]
        public string CCTV;
        [JsonProperty(PropertyName = "CH47")]
        public string CH47;
        [JsonProperty(PropertyName = "RidableHorse")]
        public string RH;
        [JsonProperty(PropertyName = "MLRS")]
        public string MLRS;
        [JsonProperty(PropertyName = "NPC")]
        public string NPC;
        [JsonProperty(PropertyName = "RHIB")]
        public string RHIB;
        [JsonProperty(PropertyName = "Traps")]
        public string Traps;
        public string Get(EntityType type);
    }

    public class ConfigurationGUI
    {
        [JsonProperty(PropertyName = "Move Arrow Text")]
        public string Arrow;
        [JsonProperty(PropertyName = "Offset Min")]
        public string OffsetMin;
        [JsonProperty(PropertyName = "Offset Max")]
        public string OffsetMax;
        [JsonProperty(PropertyName = "Color On")]
        public string On;
        [JsonProperty(PropertyName = "Color Off")]
        public string Off;
        [JsonProperty(PropertyName = "Show Button - All")]
        public bool All;
        [JsonProperty(PropertyName = "Show Button - Airdrops")]
        public bool Airdrop;
        [JsonProperty(PropertyName = "Show Button - Bags")]
        public bool Bags;
        [JsonProperty(PropertyName = "Show Button - Boats")]
        public bool Boats;
        [JsonProperty(PropertyName = "Show Button - Bradley")]
        public bool Bradley;
        [JsonProperty(PropertyName = "Show Button - Box")]
        public bool Box;
        [JsonProperty(PropertyName = "Show Button - Cars")]
        public bool Cars;
        [JsonProperty(PropertyName = "Show Button - CCTV")]
        public bool CCTV;
        [JsonProperty(PropertyName = "Show Button - CargoPlanes")]
        public bool CP;
        [JsonProperty(PropertyName = "Show Button - CargoShips")]
        public bool CS;
        [JsonProperty(PropertyName = "Show Button - CH47")]
        public bool CH47;
        [JsonProperty(PropertyName = "Show Button - Collectibles")]
        public bool Col;
        [JsonProperty(PropertyName = "Show Button - Dead")]
        public bool Dead;
        [JsonProperty(PropertyName = "Show Button - Heli")]
        public bool Heli;
        [JsonProperty(PropertyName = "Show Button - Loot")]
        public bool Loot;
        [JsonProperty(PropertyName = "Show Button - MiniCopter")]
        public bool MC;
        [JsonProperty(PropertyName = "Show Button - MLRS")]
        public bool MLRS;
        [JsonProperty(PropertyName = "Show Button - NPC")]
        public bool NPC;
        [JsonProperty(PropertyName = "Show Button - Ore")]
        public bool Ore;
        [JsonProperty(PropertyName = "Show Button - Ridable Horses")]
        public bool Horse;
        [JsonProperty(PropertyName = "Show Button - RigidHullInflatableBoats")]
        public bool RHIB;
        [JsonProperty(PropertyName = "Show Button - Sleepers")]
        public bool Sleepers;
        [JsonProperty(PropertyName = "Show Button - Stash")]
        public bool Stash;
        [JsonProperty(PropertyName = "Show Button - TC")]
        public bool TC;
        [JsonProperty(PropertyName = "Show Button - TC Arrow")]
        public bool TCArrow;
        [JsonProperty(PropertyName = "Show Button - TC Turrets")]
        public bool Turrets;
        [JsonProperty(PropertyName = "Show Button - Traps")]
        public bool Traps;
        public bool Get(EntityType type);
    }

    public class ConfigurationVoiceDetection
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled;
        [JsonProperty(PropertyName = "Timeout After X Seconds")]
        public int Interval;
        [JsonProperty(PropertyName = "Detection Radius")]
        public float Distance;
    }

    public class ConfigurationDiscord
    {
        [JsonProperty(PropertyName = "Message - Embed Color (DECIMAL)")]
        public int Color;
        [JsonProperty(PropertyName = "Message - Webhook URL")]
        public string Webhook;
        [JsonProperty(PropertyName = "Embed_MessageServer")]
        public string Server;
        [JsonProperty(PropertyName = "Embed_MessageLocation")]
        public string Location;
        [JsonProperty(PropertyName = "Embed_MessageTitle")]
        public string Title;
        [JsonProperty(PropertyName = "Embed_MessagePlayer")]
        public string Player;
        [JsonProperty(PropertyName = "Embed_MessageMessage")]
        public string Message;
        [JsonProperty(PropertyName = "Off")]
        public string Off;
        [JsonProperty(PropertyName = "On")]
        public string On;
    }

    public class ConfigurationTrack
    {
        [JsonProperty(PropertyName = "Radar")]
        public bool Radar;
        [JsonProperty(PropertyName = "Radar Text")]
        public string RadarText;
        [JsonProperty(PropertyName = "Console Godmode")]
        public bool God;
        [JsonProperty(PropertyName = "Console Godmode Text")]
        public string GodText;
        [JsonProperty(PropertyName = "Plugin Godmode")]
        public bool GodPlugin;
        [JsonProperty(PropertyName = "Plugin Godmode Text")]
        public string GodPluginText;
        [JsonProperty(PropertyName = "Vanish")]
        public bool Vanish;
        [JsonProperty(PropertyName = "Vanish Text")]
        public string VanishText;
        [JsonProperty(PropertyName = "NOCLIP")]
        public bool NoClip;
        [JsonProperty(PropertyName = "NOCLIP Text")]
        public string NoClipText;
    }

    public class Configuration
    {
        [JsonProperty(PropertyName = "Core Tracking")]
        public ConfigurationCoreTracking Core { get; set; }
        [JsonProperty(PropertyName = "Additional Tracking")]
        public ConfigurationAdditionalTracking Additional { get; set; }
        [JsonProperty(PropertyName = "Color-Hex Codes")]
        public ConfigurationHex Hex { get; set; }
        [JsonProperty(PropertyName = "DiscordMessages")]
        public ConfigurationDiscord Discord { get; set; }
        [JsonProperty(PropertyName = "Drawing Distances")]
        public ConfigurationDrawDistances Distance { get; set; }
        [JsonProperty(PropertyName = "Drawing Methods")]
        public ConfigurationDrawMethods Methods { get; set; }
        [JsonProperty(PropertyName = "Group Limit")]
        public ConfigurationLimits Limit { get; set; }
        [JsonProperty(PropertyName = "GUI")]
        public ConfigurationGUI GUI { get; set; }
        [JsonProperty(PropertyName = "Options")]
        public ConfigurationOptions Options { get; set; }
        [JsonProperty(PropertyName = "Settings")]
        public ConfigurationSettings Settings { get; set; }
        [JsonProperty(PropertyName = "Track Admin Status")]
        public ConfigurationTrack Track { get; set; }
        [JsonProperty(PropertyName = "Voice Detection")]
        public ConfigurationVoiceDetection Voice { get; set; }
    }

    protected override void LoadConfig();
    private void RegisterCommands();
    private string radarCommand;
    private bool canSaveConfig;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private string m(string key, string id, object[] args);
    private static string R(string source);
    private void Message(BasePlayer target, string key, object[] args);
}

private class StoredData
{
    public Dictionary<ulong, UiOffsets> Offsets;
    public Dictionary<string, int> EntityTextSize;
    public Dictionary<string, int> EntityNameSize;
    public Dictionary<string, int> PlayerTextSize;
    public Dictionary<string, int> PlayerNameSize;
    public List<string> Extended;
    public Dictionary<string, List<string>> Filters;
    public List<string> Hidden;
    public List<string> OnlineBoxes;
    public List<string> Visions;
    public List<string> Active;
    public StoredData();
    public void Init();
}

private class Cache
{
    public Cache(AdminRadar instance);
    public Configuration config;
    public AdminRadar instance;
    internal EntityType entityType;
    public Dictionary<NetworkableId, EntityInfo> Airdrops { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Animals { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Backpacks { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Bags { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Boats { get; set; }
    public Dictionary<NetworkableId, EntityInfo> BradleyAPCs { get; set; }
    public Dictionary<NetworkableId, EntityInfo> CargoPlanes { get; set; }
    public Dictionary<NetworkableId, EntityInfo> CargoShips { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Cars { get; set; }
    public Dictionary<NetworkableId, EntityInfo> CCTV { get; set; }
    public Dictionary<NetworkableId, EntityInfo> CH47 { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Cupboards { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Collectibles { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Containers { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Corpses { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Drops { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Helicopters { get; set; }
    public Dictionary<NetworkableId, EntityInfo> MiniCopter { get; set; }
    public Dictionary<NetworkableId, EntityInfo> MLRS { get; set; }
    public Dictionary<NetworkableId, EntityInfo> NPCPlayers { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Ores { get; set; }
    public Dictionary<NetworkableId, EntityInfo> RHIB { get; set; }
    public Dictionary<NetworkableId, EntityInfo> RidableHorse { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Turrets { get; set; }
    public Dictionary<NetworkableId, EntityInfo> Traps { get; set; }
    public bool Add(BaseEntity entity);
    private bool TryGetContainerType(BaseEntity entity, EntityType type);
    public bool Remove(NetworkableId nid, Vector3 entityPos);
    private bool Add_Internal(Dictionary<NetworkableId, EntityInfo> cachedList, BaseEntity entity, EntityType type);
    private bool Remove_Internal(Dictionary<TKeyType, TType> cachedList, TKeyType key);
    public bool IsDrop(BaseEntity entity);
    public bool IsTrap(BaseNetworkable entity);
    public bool IsLoot(BaseNetworkable entity);
    public bool IsBox(BaseNetworkable entity);
}

private class CoroutineTimer
{
    private Stopwatch stopwatch;
    private float _maxDurationMs;
    private bool _isRunning;
    public double Elapsed { get; set; }
    public CoroutineTimer(float maxDurationMs);
    public void Start();
    public bool ShouldYield();
    public void ResetIfYielded();
}

internal static class StringBuilderCache
{
    [ThreadStatic]
    private static StringBuilder builder;
    public static StringBuilder Acquire(string text);
    public static void Clear();
    public static string GetStringAndRelease(StringBuilder sb);
}

public class EntityInfo
{
    public BuildingPrivlidge priv;
    public BaseEntity entity;
    public EntityType type;
    public Color color;
    public Vector3 _from;
    public Vector3 to;
    public Vector3 offset;
    public string name;
    public object info;
    public float dist;
    public float sqrdist;
    public float size;
    public Transform t;
    public Network.Visibility.Group group { get; set; }
    public Vector3 from { get; set; }
    public EntityInfo();
    public EntityInfo(BaseEntity entity, EntityType type, Func<EntityType, BaseEntity, float> getDistance, Func<string, string> stripTags);
}

private class Radar : FacepunchBehaviour
{
    private static Func<char, bool> abbr;
    public class DataObject : Pool.IPooled
    {
        public EntityInfo ei;
        public Action action;
        public DrawFlags flags;
        public bool disabled;
        public DataObject();
        public bool HasFlag(DrawFlags flag);
        public void SetEnabled(Network.Visibility.Group group, Vector3 to, float max);
        public bool IsOfType(EntityType type);
        public void Reset();
        public void EnterPool();
        public void LeavePool();
    }

    internal class DistantPlayer : Pool.IPooled
    {
        public Vector3 pos;
        public bool alive;
        public DistantPlayer();
        public void Reset();
        public void EnterPool();
        public void LeavePool();
    }

    internal bool setSource;
    internal bool canGetExistingBackpacks;
    internal bool isEnabled;
    internal bool canBypassOverride;
    internal bool hasPermAllowed;
    internal bool isAdmin;
    internal bool showHT;
    internal bool showAll;
    internal int inactiveSeconds;
    internal int activatedSeconds;
    internal int checks;
    internal float currDistance;
    internal float invokeTime;
    internal float maxDistance;
    internal string username;
    internal string userid;
    internal RaycastHit hit;
    internal EntityType currType;
    internal Vector3 position;
    internal BaseEntity source;
    internal BasePlayer player;
    internal AdminRadar instance;
    internal Network.Visibility.Group group;
    internal ItemContainer _backpackItemContainer;
    internal Coroutine _radarCo;
    internal Coroutine _updateCo;
    internal Coroutine _groupCo;
    internal List<ulong> exclude;
    internal List<NetworkableId> removeByNetworkId;
    internal List<EntityType> entityTypes;
    internal List<DistantPlayer> distant;
    internal List<EntityType> removeByEntityType;
    internal Dictionary<NetworkableId, DataObject> data;
    internal Dictionary<EntityType, Action> filters;
    internal Dictionary<ulong, ItemContainer> backpacks;
    internal Dictionary<ulong, float> Voices;
    internal List<ulong> VoiceExpirations;
    internal Cache Cache { get; set; }
    internal Configuration config { get; set; }
    internal float delay { get; set; }
    internal float Distance(Vector3 a);
    public Vector3 limitUp;
    public Vector3 halfUp;
    public Vector3 twoHalfUp;
    public Vector3 twoUp;
    public Vector3 fiveUp;
    private void Awake();
    private void OnDestroy();
    private void ResetToPool();
    public bool Add(EntityType type);
    public void Init(AdminRadar instance);
    public void StopAll();
    public bool GetBool(EntityType type);
    private void Activity();
    private class Idle
    {
        public BasePlayer player;
        public Vector3 lastPosition;
        public float lastMovementTime;
    }

    private Dictionary<ulong, Idle> _idle;
    private double GetIdleTime(BasePlayer target);
    private void RemoveIdleTime();
    private void SetupFilter(EntityType type, Action action);
    public void SetupFilters(bool barebones);
    private void SetupFilters();
    private IEnumerator DoGroupLimitRoutine();
    private IEnumerator DoUpdateRoutine();
    private IEnumerator DoRadarRoutine();
    private void DoRemoves();
    private void CheckNetworkGroupChange();
    public void SetEnabledDataObjects();
    public void RemoveByEntityType(EntityType type);
    public void RemoveByNetworkId(NetworkableId nid);
    public void TryCacheByType(EntityType type, EntityInfo ei);
    private DataObject cobj;
    private DrawFlags cflag;
    private void DirectDrawAll();
    private void DrawVisionArrow(BasePlayer target, float dist);
    private void DrawVoiceArrow(BasePlayer target, Vector3 a, float dist);
    private void DrawArrow(Color color, Vector3 from, Vector3 to, float size, bool @override);
    private void DrawPlayerText(Color color, Vector3 position, object prefix, object text, bool @override);
    private void DrawBox(Color color, Vector3 position, float size, bool @override);
    private void CacheArrow(DataObject obj, Color color, Vector3 offset, Vector3 to, float size, bool @override);
    private void CacheBox(DataObject obj, Color color, Vector3 offset, float size, bool @override);
    private void CacheText(DataObject obj, Color color, Vector3 offset, Action action, bool @override);
    private void HandleException(NetworkableId nid, Exception ex);
    private void HandleException(Exception ex);
    private void SetAdminFlag();
    private void RemoveAdminFlag();
    private bool API_GetExistingBackpacks(ulong userid);
    public int entityNameSize;
    public int entityTextSize;
    public int playerNameSize;
    public int playerTextSize;
    private string Format(object prefix, object text, bool entity);
    private string Format(BasePlayer target, bool s);
    private bool HasPermission(string userid, string perm);
    private string GetContents(List<Item> itemList, int num);
    private string GetContents(ItemContainer[] containers, int num);
    private bool SetSource();
    private float SetDistance(Vector3 a);
    private DataObject SetDataObject(EntityInfo ei);
    private bool HasDataObject(BaseEntity entity);
    private bool IsValid(EntityInfo ei, float dist);
    private void ShowActive();
    private void CheckVoiceExpirations();
    public void TryCacheOnlinePlayer(BasePlayer target);
    private void ShowSleepers();
    private void TryCacheSleepingPlayer(BasePlayer target);
    private Color GetColor(BasePlayer target, Vector3 a);
    private void DrawAppendedText(BasePlayer target, Vector3 a, Vector3 offset, Color color);
    private string GetCheats(BasePlayer target);
    private void ShowGroupLimits();
    private void ClearDistantPlayers();
    private void DrawCupboardArrows(BasePlayer target, EntityType lastType);
    private void ShowHeli();
    private void CacheHeli(EntityInfo ei);
    private void ShowBradley();
    private void CacheBradley(EntityInfo ei);
    private void ShowTC();
    private void CacheTC(EntityInfo ei);
    private void ShowSupplyDrops();
    private void CacheAirdrop(EntityInfo ei);
    private void ShowContainer(EntityInfo ei, EntityType type);
    private void ShowBox();
    private void CacheContainer(EntityInfo ei);
    private void ShowStash();
    private void CacheStash(EntityInfo ei);
    private void ShowLoot();
    private void CacheBackpack(EntityInfo ei);
    private void CacheLoot(EntityInfo ei);
    private void ShowBags();
    private void CacheSleepingBag(EntityInfo ei);
    private void ShowTurrets();
    private void CacheTurret(EntityInfo ei);
    private void ShowDead();
    private void CacheDead(EntityInfo ei);
    private void ShowDrops();
    private void CacheDrop(EntityInfo ei);
    private void ShowNPC();
    private void CacheAnimal(EntityInfo ei);
    private void CacheNpc(EntityInfo ei);
    private void DrawVictim(BasePlayer victim, Vector3 from, Vector3 offset, Color color);
    private bool IsAtView(EntityInfo ei);
    private void ShowOre();
    private void CacheOre(EntityInfo ei);
    private void ShowCCTV();
    private void CacheCCTV(EntityInfo ei);
    private void ShowCollectibles();
    private void CacheCol(EntityInfo ei);
    private void ShowEntity(EntityType entityType, Dictionary<NetworkableId, EntityInfo> entities);
    private void CacheEntity(EntityInfo ei, EntityType entityType);
    private static float GetScale(float value);
}

public class DataObject : Pool.IPooled
{
    public EntityInfo ei;
    public Action action;
    public DrawFlags flags;
    public bool disabled;
    public DataObject();
    public bool HasFlag(DrawFlags flag);
    public void SetEnabled(Network.Visibility.Group group, Vector3 to, float max);
    public bool IsOfType(EntityType type);
    public void Reset();
    public void EnterPool();
    public void LeavePool();
}

internal class DistantPlayer : Pool.IPooled
{
    public Vector3 pos;
    public bool alive;
    public DistantPlayer();
    public void Reset();
    public void EnterPool();
    public void LeavePool();
}

private class Idle
{
    public BasePlayer player;
    public Vector3 lastPosition;
    public float lastMovementTime;
}

public class UiOffsets
{
    [JsonIgnore]
    public bool changed;
    public bool Mover;
    public string Min;
    public string Max;
    public UiOffsets();
    public UiOffsets(string min, string max);
    public bool Equals(UiOffsets other);
}

public class ConfigurationSettings
{
    public int GetLimit(BasePlayer player);
    [JsonProperty(PropertyName = "Barebones Performance Mode")]
    public bool Barebones;
    [JsonProperty(PropertyName = "Restrict Access To Steam64 IDs", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Authorized;
    [JsonProperty(PropertyName = "Restrict Access To Auth Level")]
    public int authLevel;
    [JsonProperty(PropertyName = "Max Active Filters (OWNERID)")]
    public int Owner;
    [JsonProperty(PropertyName = "Max Active Filters (MODERATORID)")]
    public int Moderator;
    [JsonProperty(PropertyName = "Max Active Filters (ADMINRADAR.ALLOWED)")]
    public int Allowed;
    [JsonProperty(PropertyName = "Default Distance")]
    public float DefaultMaxDistance;
    [JsonProperty(PropertyName = "Default Refresh Time")]
    public float DefaultInvokeTime;
    [JsonProperty(PropertyName = "Minimum Refresh Time")]
    public float MinInvokeTime;
    [JsonProperty(PropertyName = "Dropped Item Exceptions", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> DropExceptions;
    [JsonProperty(PropertyName = "Deactivate Radar After X Seconds Inactive")]
    public float InactiveSeconds;
    [JsonProperty(PropertyName = "Deactivate Radar After X Seconds Activated")]
    public float DeactivateSeconds;
    [JsonProperty(PropertyName = "User Interface Enabled")]
    public bool UI;
    [JsonProperty(PropertyName = "Show Average Ping Every X Seconds [0 = disabled]")]
    public float AveragePingInterval;
    [JsonProperty(PropertyName = "Show Player Idle Time (Minutes)")]
    public bool ShowIdleTime;
    [JsonProperty(PropertyName = "Player Idle Time Visible After X Minutes")]
    public int IdleVisibleMinutes;
    [JsonProperty(PropertyName = "Player Idle Time Round To X Digits")]
    public int IdleRoundDigits;
    [JsonProperty(PropertyName = "Re-use Cooldown, Seconds")]
    public float Cooldown;
    [JsonProperty(PropertyName = "Show Radar Activated/Deactivated Messages")]
    public bool ShowToggle;
    [JsonProperty(PropertyName = "Player Name Text Size")]
    public int PlayerNameSize;
    [JsonProperty(PropertyName = "Player Information Text Size")]
    public int PlayerTextSize;
    [JsonProperty(PropertyName = "Entity Name Text Size")]
    public int EntityNameSize;
    [JsonProperty(PropertyName = "Entity Information Text Size")]
    public int EntityTextSize;
    [JsonProperty(PropertyName = "Unique Clan/Team Color Applies To Entire Player Text")]
    public bool ApplySameColor;
    [JsonProperty(PropertyName = "Track Group Name")]
    public string New;
    [JsonProperty(PropertyName = "Tracked Group Name Text")]
    public string NewText;
    [JsonProperty(PropertyName = "Chat Command")]
    public string Primary;
    [JsonProperty(PropertyName = "Second Command")]
    public string Secondary;
}

public class ConfigurationOptions
{
    [JsonProperty(PropertyName = "Additional Boxes", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> AdditionalBoxes;
    [JsonProperty(PropertyName = "Additional Traps", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> AdditionalTraps;
    [JsonProperty(PropertyName = "Draw Distant Players With X")]
    public bool DrawX;
    [JsonProperty(PropertyName = "Draw Empty Containers")]
    public bool DrawEmptyContainers;
    [JsonProperty(PropertyName = "Abbreviate Item Names")]
    public bool Abbr;
    [JsonProperty(PropertyName = "Show Resource Amounts")]
    public bool ResourceAmounts;
    [JsonProperty(PropertyName = "Show X Items From Barrel And Crate")]
    public int LootContentAmount;
    [JsonProperty(PropertyName = "Show X Items From Airdrop")]
    public int AirdropContentAmount;
    [JsonProperty(PropertyName = "Show X Items From Stash")]
    public int StashContentAmount;
    [JsonProperty(PropertyName = "Show X Items From Backpacks")]
    public int BackpackContentAmount;
    [JsonProperty(PropertyName = "Show X Items From Corpses")]
    public int CorpseContentAmount;
    [JsonProperty(PropertyName = "Show NPC At World View")]
    public bool NpcWorldView;
    [JsonProperty(PropertyName = "Show NPC Name As Prefab Name")]
    public bool NpcPrefabName;
    [JsonProperty(PropertyName = "Show Authed Count On Cupboards")]
    public bool TCAuthed;
    [JsonProperty(PropertyName = "Show Bag Count On Cupboards")]
    public bool TCBags;
    [JsonProperty(PropertyName = "Show Npc Player Target")]
    public bool DrawTargetsVictim;
    [JsonProperty(PropertyName = "Radar Buildings Draw Time")]
    public float BuildingsDrawTime;
    [JsonProperty(PropertyName = "Radar Drops Draw Time")]
    public float DropsDrawTime;
    [JsonProperty(PropertyName = "Radar Find Draw Time")]
    public float FindDrawTime;
    [JsonProperty(PropertyName = "Radar FindByID Draw Time")]
    public float FindByIDDrawTime;
    public int Get(EntityType type);
}

public class ConfigurationDrawMethods
{
    [JsonProperty(PropertyName = "Draw Arrows On Players")]
    public bool Arrow;
    [JsonProperty(PropertyName = "Draw Boxes")]
    public bool Box;
    [JsonProperty(PropertyName = "Draw Text")]
    public bool Text;
}

public class ConfigurationLimits
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Limit")]
    public int Amount;
    [JsonProperty(PropertyName = "Range")]
    public float Range;
    [JsonProperty(PropertyName = "Height Offset [0.0 = disabled]")]
    public float Height;
    [JsonProperty(PropertyName = "Use Group Colors Configuration")]
    public bool ColorsEnabled;
    [JsonProperty(PropertyName = "Dead Color")]
    public string Dead;
    [JsonProperty(PropertyName = "Group Color Basic")]
    public string Basic;
    [JsonProperty(PropertyName = "Group Limit Colors", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, string> Colors;
}

public class ConfigurationDrawDistances
{
    [JsonProperty(PropertyName = "Sleepers Min Y")]
    public float MinY;
    [JsonProperty(PropertyName = "Player Corpses")]
    public float Corpse;
    [JsonProperty(PropertyName = "Players")]
    public float Players;
    [JsonProperty(PropertyName = "Airdrop Crates")]
    public float Airdrop;
    [JsonProperty(PropertyName = "Animals")]
    public float Animal;
    [JsonProperty(PropertyName = "Boats")]
    public float Boat;
    [JsonProperty(PropertyName = "Boxes")]
    public float Box;
    [JsonProperty(PropertyName = "Cars")]
    public float Cars;
    [JsonProperty(PropertyName = "CCTV")]
    public float CCTV;
    [JsonProperty(PropertyName = "Collectibles")]
    public float Col;
    [JsonProperty(PropertyName = "Loot Containers")]
    public float Loot;
    [JsonProperty(PropertyName = "MiniCopter")]
    public float MC;
    [JsonProperty(PropertyName = "MLRS")]
    public float MLRS;
    [JsonProperty(PropertyName = "NPC Players")]
    public float NPC;
    [JsonProperty(PropertyName = "Resources (Ore)")]
    public float Ore;
    [JsonProperty(PropertyName = "Ridable Horses")]
    public float RH;
    [JsonProperty(PropertyName = "Sleeping Bags")]
    public float Bag;
    [JsonProperty(PropertyName = "Stashes")]
    public float Stash;
    [JsonProperty(PropertyName = "Tool Cupboards")]
    public float TC;
    [JsonProperty(PropertyName = "Tool Cupboard Arrows")]
    public float TCArrows;
    [JsonProperty(PropertyName = "Traps")]
    public float Traps;
    [JsonProperty(PropertyName = "Turrets")]
    public float Turret;
    [JsonProperty(PropertyName = "Vending Machines")]
    public float VendingMachine;
    [JsonProperty(PropertyName = "Radar Drops Command")]
    public float Drops;
    public float Get(EntityType type, BaseEntity entity);
}

public class ConfigurationCoreTracking
{
    [JsonProperty(PropertyName = "Players")]
    public bool Active;
    [JsonProperty(PropertyName = "Sleepers")]
    public bool Sleepers;
    [JsonProperty(PropertyName = "Animals")]
    public bool Animals;
    [JsonProperty(PropertyName = "Bags")]
    public bool Bags;
    [JsonProperty(PropertyName = "Box")]
    public bool Box;
    [JsonProperty(PropertyName = "Collectibles")]
    public bool Col;
    [JsonProperty(PropertyName = "Dead")]
    public bool Dead;
    [JsonProperty(PropertyName = "Loot")]
    public bool Loot;
    [JsonProperty(PropertyName = "NPC")]
    public bool NPCPlayer;
    [JsonProperty(PropertyName = "Ore")]
    public bool Ore;
    [JsonProperty(PropertyName = "Stash")]
    public bool Stash;
    [JsonProperty(PropertyName = "SupplyDrops")]
    public bool Airdrop;
    [JsonProperty(PropertyName = "TC")]
    public bool TC;
    [JsonProperty(PropertyName = "Turrets")]
    public bool Turrets;
}

public class ConfigurationAdditionalTracking
{
    [JsonProperty(PropertyName = "Backpacks Plugin")]
    public bool BackpackPlugin { get; set; }
    [JsonProperty(PropertyName = "Boats")]
    public bool Boats;
    [JsonProperty(PropertyName = "Bradley APC")]
    public bool Bradley;
    [JsonProperty(PropertyName = "Cars")]
    public bool Cars;
    [JsonProperty(PropertyName = "CargoPlanes")]
    public bool CP;
    [JsonProperty(PropertyName = "CargoShips")]
    public bool CS;
    [JsonProperty(PropertyName = "CCTV")]
    public bool CCTV;
    [JsonProperty(PropertyName = "CH47")]
    public bool CH47;
    [JsonProperty(PropertyName = "Helicopters")]
    public bool Heli;
    [JsonProperty(PropertyName = "Helicopter Rotor Health")]
    public bool RotorHealth;
    [JsonProperty(PropertyName = "MiniCopter")]
    public bool MC;
    [JsonProperty(PropertyName = "MLRS")]
    public bool MLRS;
    [JsonProperty(PropertyName = "Ridable Horses")]
    public bool RH;
    [JsonProperty(PropertyName = "RHIB")]
    public bool RHIB;
    [JsonProperty(PropertyName = "Traps")]
    public bool Traps;
    public bool Get(EntityType type);
}

public class ConfigurationHex
{
    [JsonProperty(PropertyName = "Player Arrows")]
    public string Arrows;
    [JsonProperty(PropertyName = "Distance")]
    public string Dist;
    [JsonProperty(PropertyName = "Helicopters")]
    public string Heli;
    [JsonProperty(PropertyName = "Bradley")]
    public string Bradley;
    [JsonProperty(PropertyName = "MiniCopter")]
    public string MC;
    [JsonProperty(PropertyName = "MiniCopter (ScrapTransportHelicopter)")]
    public string STH;
    [JsonProperty(PropertyName = "Online Player")]
    public string Online;
    [JsonProperty(PropertyName = "Online Player (Underground)")]
    public string Underground;
    [JsonProperty(PropertyName = "Online Player (Flying)")]
    public string Flying;
    [JsonProperty(PropertyName = "Online Dead Player")]
    public string OnlineDead;
    [JsonProperty(PropertyName = "Dead Player")]
    public string Dead;
    [JsonProperty(PropertyName = "Sleeping Player")]
    public string Sleeper;
    [JsonProperty(PropertyName = "Sleeping Dead Player")]
    public string SleeperDead;
    [JsonProperty(PropertyName = "Health")]
    public string Health;
    [JsonProperty(PropertyName = "Idle Time")]
    public string IdleTime;
    [JsonProperty(PropertyName = "Backpacks")]
    public string Backpack;
    [JsonProperty(PropertyName = "Scientists")]
    public string Scientist;
    [JsonProperty(PropertyName = "Scientist Peacekeeper")]
    public string Peacekeeper;
    [JsonProperty(PropertyName = "Murderers")]
    public string Murderer;
    [JsonProperty(PropertyName = "Animals")]
    public string Animal;
    [JsonProperty(PropertyName = "Resources")]
    public string Resource;
    [JsonProperty(PropertyName = "Collectibles")]
    public string Col;
    [JsonProperty(PropertyName = "Tool Cupboards")]
    public string TC;
    [JsonProperty(PropertyName = "Sleeping Bags")]
    public string Bag;
    [JsonProperty(PropertyName = "Airdrops")]
    public string AD;
    [JsonProperty(PropertyName = "AutoTurrets")]
    public string AT;
    [JsonProperty(PropertyName = "Corpses")]
    public string Corpse;
    [JsonProperty(PropertyName = "Box")]
    public string Box;
    [JsonProperty(PropertyName = "Loot")]
    public string Loot;
    [JsonProperty(PropertyName = "Stash")]
    public string Stash;
    [JsonProperty(PropertyName = "Boat")]
    public string Boat;
    [JsonProperty(PropertyName = "CargoPlane")]
    public string CP;
    [JsonProperty(PropertyName = "CargoShip")]
    public string CS;
    [JsonProperty(PropertyName = "Car")]
    public string Cars;
    [JsonProperty(PropertyName = "CCTV")]
    public string CCTV;
    [JsonProperty(PropertyName = "CH47")]
    public string CH47;
    [JsonProperty(PropertyName = "RidableHorse")]
    public string RH;
    [JsonProperty(PropertyName = "MLRS")]
    public string MLRS;
    [JsonProperty(PropertyName = "NPC")]
    public string NPC;
    [JsonProperty(PropertyName = "RHIB")]
    public string RHIB;
    [JsonProperty(PropertyName = "Traps")]
    public string Traps;
    public string Get(EntityType type);
}

public class ConfigurationGUI
{
    [JsonProperty(PropertyName = "Move Arrow Text")]
    public string Arrow;
    [JsonProperty(PropertyName = "Offset Min")]
    public string OffsetMin;
    [JsonProperty(PropertyName = "Offset Max")]
    public string OffsetMax;
    [JsonProperty(PropertyName = "Color On")]
    public string On;
    [JsonProperty(PropertyName = "Color Off")]
    public string Off;
    [JsonProperty(PropertyName = "Show Button - All")]
    public bool All;
    [JsonProperty(PropertyName = "Show Button - Airdrops")]
    public bool Airdrop;
    [JsonProperty(PropertyName = "Show Button - Bags")]
    public bool Bags;
    [JsonProperty(PropertyName = "Show Button - Boats")]
    public bool Boats;
    [JsonProperty(PropertyName = "Show Button - Bradley")]
    public bool Bradley;
    [JsonProperty(PropertyName = "Show Button - Box")]
    public bool Box;
    [JsonProperty(PropertyName = "Show Button - Cars")]
    public bool Cars;
    [JsonProperty(PropertyName = "Show Button - CCTV")]
    public bool CCTV;
    [JsonProperty(PropertyName = "Show Button - CargoPlanes")]
    public bool CP;
    [JsonProperty(PropertyName = "Show Button - CargoShips")]
    public bool CS;
    [JsonProperty(PropertyName = "Show Button - CH47")]
    public bool CH47;
    [JsonProperty(PropertyName = "Show Button - Collectibles")]
    public bool Col;
    [JsonProperty(PropertyName = "Show Button - Dead")]
    public bool Dead;
    [JsonProperty(PropertyName = "Show Button - Heli")]
    public bool Heli;
    [JsonProperty(PropertyName = "Show Button - Loot")]
    public bool Loot;
    [JsonProperty(PropertyName = "Show Button - MiniCopter")]
    public bool MC;
    [JsonProperty(PropertyName = "Show Button - MLRS")]
    public bool MLRS;
    [JsonProperty(PropertyName = "Show Button - NPC")]
    public bool NPC;
    [JsonProperty(PropertyName = "Show Button - Ore")]
    public bool Ore;
    [JsonProperty(PropertyName = "Show Button - Ridable Horses")]
    public bool Horse;
    [JsonProperty(PropertyName = "Show Button - RigidHullInflatableBoats")]
    public bool RHIB;
    [JsonProperty(PropertyName = "Show Button - Sleepers")]
    public bool Sleepers;
    [JsonProperty(PropertyName = "Show Button - Stash")]
    public bool Stash;
    [JsonProperty(PropertyName = "Show Button - TC")]
    public bool TC;
    [JsonProperty(PropertyName = "Show Button - TC Arrow")]
    public bool TCArrow;
    [JsonProperty(PropertyName = "Show Button - TC Turrets")]
    public bool Turrets;
    [JsonProperty(PropertyName = "Show Button - Traps")]
    public bool Traps;
    public bool Get(EntityType type);
}

public class ConfigurationVoiceDetection
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled;
    [JsonProperty(PropertyName = "Timeout After X Seconds")]
    public int Interval;
    [JsonProperty(PropertyName = "Detection Radius")]
    public float Distance;
}

public class ConfigurationDiscord
{
    [JsonProperty(PropertyName = "Message - Embed Color (DECIMAL)")]
    public int Color;
    [JsonProperty(PropertyName = "Message - Webhook URL")]
    public string Webhook;
    [JsonProperty(PropertyName = "Embed_MessageServer")]
    public string Server;
    [JsonProperty(PropertyName = "Embed_MessageLocation")]
    public string Location;
    [JsonProperty(PropertyName = "Embed_MessageTitle")]
    public string Title;
    [JsonProperty(PropertyName = "Embed_MessagePlayer")]
    public string Player;
    [JsonProperty(PropertyName = "Embed_MessageMessage")]
    public string Message;
    [JsonProperty(PropertyName = "Off")]
    public string Off;
    [JsonProperty(PropertyName = "On")]
    public string On;
}

public class ConfigurationTrack
{
    [JsonProperty(PropertyName = "Radar")]
    public bool Radar;
    [JsonProperty(PropertyName = "Radar Text")]
    public string RadarText;
    [JsonProperty(PropertyName = "Console Godmode")]
    public bool God;
    [JsonProperty(PropertyName = "Console Godmode Text")]
    public string GodText;
    [JsonProperty(PropertyName = "Plugin Godmode")]
    public bool GodPlugin;
    [JsonProperty(PropertyName = "Plugin Godmode Text")]
    public string GodPluginText;
    [JsonProperty(PropertyName = "Vanish")]
    public bool Vanish;
    [JsonProperty(PropertyName = "Vanish Text")]
    public string VanishText;
    [JsonProperty(PropertyName = "NOCLIP")]
    public bool NoClip;
    [JsonProperty(PropertyName = "NOCLIP Text")]
    public string NoClipText;
}

public class Configuration
{
    [JsonProperty(PropertyName = "Core Tracking")]
    public ConfigurationCoreTracking Core { get; set; }
    [JsonProperty(PropertyName = "Additional Tracking")]
    public ConfigurationAdditionalTracking Additional { get; set; }
    [JsonProperty(PropertyName = "Color-Hex Codes")]
    public ConfigurationHex Hex { get; set; }
    [JsonProperty(PropertyName = "DiscordMessages")]
    public ConfigurationDiscord Discord { get; set; }
    [JsonProperty(PropertyName = "Drawing Distances")]
    public ConfigurationDrawDistances Distance { get; set; }
    [JsonProperty(PropertyName = "Drawing Methods")]
    public ConfigurationDrawMethods Methods { get; set; }
    [JsonProperty(PropertyName = "Group Limit")]
    public ConfigurationLimits Limit { get; set; }
    [JsonProperty(PropertyName = "GUI")]
    public ConfigurationGUI GUI { get; set; }
    [JsonProperty(PropertyName = "Options")]
    public ConfigurationOptions Options { get; set; }
    [JsonProperty(PropertyName = "Settings")]
    public ConfigurationSettings Settings { get; set; }
    [JsonProperty(PropertyName = "Track Admin Status")]
    public ConfigurationTrack Track { get; set; }
    [JsonProperty(PropertyName = "Voice Detection")]
    public ConfigurationVoiceDetection Voice { get; set; }
}

Oxide.Plugins.AdminRadarExtensionMethods
public static class ExtensionMethods
{
    public class DisposableList : List<T>, IDisposable, Pool.IPooled
    {
        public void EnterPool();
        public void LeavePool();
        public void Dispose();
        public static DisposableList<T> Get();
    }

    public static T ElementAt(IEnumerable<T> a, int b);
    public static List<T> ToList(IEnumerable<T> a, Func<T, bool> b);
    public static string[] ToLower(IEnumerable<string> a, Func<string, bool> b);
    public static T[] Take(IList<T> a, int b);
    public static IEnumerable<V> Select(IEnumerable<T> a, Func<T, V> b);
    public static T[] Where(IEnumerable<T> a, Func<T, bool> b);
    public static float Sum(IEnumerable<T> a, Func<T, float> b);
    public static int Sum(IEnumerable<T> a, Func<T, int> b);
    public static bool IsKilled(BaseNetworkable a);
    public static void ResetToPool(Dictionary<K, V> obj);
    public static void ResetToPool(HashSet<T> obj);
    public static void ResetToPool(List<T> obj);
}

public class DisposableList : List<T>, IDisposable, Pool.IPooled
{
    public void EnterPool();
    public void LeavePool();
    public void Dispose();
    public static DisposableList<T> Get();
}


```

---

## AdminStatusManager by Ryz0r - Gives those with permission admin status on connect and/or on command

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Facepunch.Models.Database;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("Admin Status Manager", "Ryz0r", "1.1.1")]
[Description("Give players with permission admin permissions upon connecting. Players may also toggle it with a different permission using a command.")]
public class AdminStatusManager : RustPlugin
{
    private List<BasePlayer> _adminList;
    private const string AutoPerm;
    private const string CommandPerm;
    private static AdminStatusManager _aa;
    private void ChangeStatus(BasePlayer bp, bool status);
    private void Init();
    private void Unload();
    private void OnPlayerConnected(BasePlayer player);
    private Configuration _config;
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private class Configuration
    {
        [JsonProperty(PropertyName = "Valid Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] Commands;
        [JsonProperty(PropertyName = "Automatically Give Admin")]
        public bool AutoAdmin;
        [JsonProperty(PropertyName = "Default Time for Admins When Joining")]
        public float DefaultTime;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultMessages();
    private void ToggleCommand(IPlayer player, string command, string[] args);
    private void NoclipCommand(IPlayer player, string command, string[] args);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Valid Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] Commands;
    [JsonProperty(PropertyName = "Automatically Give Admin")]
    public bool AutoAdmin;
    [JsonProperty(PropertyName = "Default Time for Admins When Joining")]
    public float DefaultTime;
}


```

---

## AdminTime by Rustic0 - Allows admins to use /day, /night, and /now to change their local time in game.

```csharp
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System;
using Newtonsoft.Json;
using System.Linq;
using System.Globalization;

Oxide.Plugins
[Info("Admin Time", "Rustic", "1.1.0")]
[Description("Allows admins to use /day, /night, and /now to change their local time in game.")]
internal class AdminTime : RustPlugin
{
    const string permAllowTimeChange;
     void Init();
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Day Value")]
        public int DayValue;
        [JsonProperty(PropertyName = "Night Value")]
        public int NightValue;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void SaveConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    [ChatCommand("day")]
    private void DayCommand(BasePlayer player);
    [ChatCommand("night")]
    private void NightCommand(BasePlayer player);
    [ChatCommand("now")]
    private void NowCommand(BasePlayer player);
    [ChatCommand("time")]
    private void TimeCommand(BasePlayer player, string command, string[] args);
     void ChangeTime(BasePlayer player, int timevalue);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Day Value")]
    public int DayValue;
    [JsonProperty(PropertyName = "Night Value")]
    public int NightValue;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## AdminToggle by Talha - Toggle your admin status

```csharp
using System.Collections.Generic;

Oxide.Plugins
[Info("Admin Toggle", "Talha", "1.0.6")]
[Description("Toggle your admin status")]
public class AdminToggle : RustPlugin
{
    private const string perm;
    private void Init();
    private void Message(BasePlayer player, string key);
    protected override void LoadDefaultMessages();
    [ChatCommand("admin")]
    private void Toggle(BasePlayer player);
}


```

---

## AdminUtilities by dFxPhoeniX - Admin Utilities is a plugin who have some functionalities like noclip. under terrain and more.

```csharp
using System;
using System.IO;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;
using CompanionServer.Handlers;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using Network;

Oxide.Plugins
[Info("Admin Utilities", "dFxPhoeniX", "2.1.4")]
[Description("Toggle NoClip, teleport Under Terrain and more")]
public class AdminUtilities : RustPlugin
{
    private const string permTerrain;
    private const string permInventory;
    private const string permHHT;
    private const string permNoClip;
    private const string permGodMode;
    private DataFileSystem dataFile;
    private DataFileSystem dataFileItems;
    private bool newSave;
    private class PlayerInfo
    {
        public string Teleport { get; set; }
        public bool SaveInventory { get; set; }
        public bool UnderTerrain { get; set; }
        public bool NoClip { get; set; }
        public bool GodMode { get; set; }
    }

    private Dictionary<string, PlayerInfo> playerInfoCache;
    private class PlayerInfoItems
    {
        public List<AdminUtilitiesItem> Items { get; set; }
    }

    private Dictionary<string, PlayerInfoItems> playerInfoItemsCache;
    private class AdminUtilitiesItem
    {
        public List<AdminUtilitiesItem> contents { get; set; }
        public string container { get; set; }
        public int ammo { get; set; }
        public int amount { get; set; }
        public string ammoType { get; set; }
        public float condition { get; set; }
        public float fuel { get; set; }
        public int frequency { get; set; }
        public int itemid { get; set; }
        public float maxCondition { get; set; }
        public string name { get; set; }
        public int position { get; set; }
        public ulong skin { get; set; }
        public string text { get; set; }
        public int blueprintAmount { get; set; }
        public int blueprintTarget { get; set; }
        public int dataInt { get; set; }
        public ulong subEntity { get; set; }
        public bool shouldPool { get; set; }
        public AdminUtilitiesItem();
        public AdminUtilitiesItem(string container, Item item);
        public static Item Create(AdminUtilitiesItem aui);
        public static void Restore(BasePlayer player, AdminUtilitiesItem aui);
    }

    private PlayerInfo LoadPlayerInfo(BasePlayer player);
    private void SavePlayerInfo(BasePlayer player, PlayerInfo playerInfo);
    private PlayerInfoItems LoadPlayerInfoItems(BasePlayer player);
    private void SavePlayerInfoItems(BasePlayer player, PlayerInfoItems playerInfoItems);
    private void OnNewSave();
    private void Init();
    private void Loaded();
    private void OnServerInformationUpdated();
    private object OnClientCommand(Connection connection, string command);
    private void OnServerInitialized();
    private void OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private object OnPlayerViolation(BasePlayer player, AntiHackType type);
    [ChatCommand("disconnectteleport")]
    private void cmdDisconnectTeleport(BasePlayer player, string command, string[] args);
    [ChatCommand("saveinventory")]
    private void cmdSaveInventory(BasePlayer player, string command, string[] args);
    [ChatCommand("noclip")]
    private void cmdNoClip(BasePlayer player, string command, string[] args);
    [ChatCommand("god")]
    private void cmdGodMode(BasePlayer player, string command, string[] args);
    private bool GetSave();
    private void ToggleNoClip(BasePlayer player);
    private void ToggleGodMode(BasePlayer player);
    private void DisconnectTeleport(BasePlayer player);
    private void NoUnderTerrain(BasePlayer player);
    private string FormatPosition(Vector3 position);
    private void SaveInventory(BasePlayer player);
    private void AddItemsFromContainer(ItemContainer container, string containerName, List<AdminUtilitiesItem> items);
    private bool HasPermission(BasePlayer player, string permName);
    private string msg(string key, string id, object[] args);
    private string RemoveFormatting(string source);
    private bool ConfigChanged;
    private Vector3 defaultPos;
    private bool wipeItems;
    private bool wipeSettings;
    private bool persistentNoClip;
    private bool persistentGodMode;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void InitConfig();
    private T GetConfig(T defaultVal, string[] path);
    [HookMethod(nameof(API_ToggleNoClip))]
    public void API_ToggleNoClip(BasePlayer player);
    [HookMethod(nameof(API_ToggleGodMode))]
    public void API_ToggleGodMode(BasePlayer player);
    [HookMethod(nameof(API_GetDisconnectTeleportPos))]
    public object API_GetDisconnectTeleportPos(string userid);
    [HookMethod(nameof(API_GetSaveInventoryStatus))]
    public bool API_GetSaveInventoryStatus(string userid);
    [HookMethod(nameof(API_GetUnderTerrainStatus))]
    public bool API_GetUnderTerrainStatus(string userid);
    [HookMethod(nameof(API_GetNoClipStatus))]
    public bool API_GetNoClipStatus(string userid);
    [HookMethod(nameof(API_GetGodModeStatus))]
    public bool API_GetGodModeStatus(string userid);
}

private class PlayerInfo
{
    public string Teleport { get; set; }
    public bool SaveInventory { get; set; }
    public bool UnderTerrain { get; set; }
    public bool NoClip { get; set; }
    public bool GodMode { get; set; }
}

private class PlayerInfoItems
{
    public List<AdminUtilitiesItem> Items { get; set; }
}

private class AdminUtilitiesItem
{
    public List<AdminUtilitiesItem> contents { get; set; }
    public string container { get; set; }
    public int ammo { get; set; }
    public int amount { get; set; }
    public string ammoType { get; set; }
    public float condition { get; set; }
    public float fuel { get; set; }
    public int frequency { get; set; }
    public int itemid { get; set; }
    public float maxCondition { get; set; }
    public string name { get; set; }
    public int position { get; set; }
    public ulong skin { get; set; }
    public string text { get; set; }
    public int blueprintAmount { get; set; }
    public int blueprintTarget { get; set; }
    public int dataInt { get; set; }
    public ulong subEntity { get; set; }
    public bool shouldPool { get; set; }
    public AdminUtilitiesItem();
    public AdminUtilitiesItem(string container, Item item);
    public static Item Create(AdminUtilitiesItem aui);
    public static void Restore(BasePlayer player, AdminUtilitiesItem aui);
}


```

---

## AdvancedArrows by Ryan - Allows players with permission to use custom arrow types

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("Advanced Arrows", "Ryan", "2.1.3")]
[Description("Allows players with permission to use custom arrow types")]
 class AdvancedArrows : RustPlugin
{
    private readonly Dictionary<ulong, ActiveArrow> ActiveArrows;
    private ConfigFile configFile;
    private readonly System.Random rnd;
    private class ConfigFile
    {
        public Dictionary<ArrowType, Arrow> Arrows;
        public ExplosionSettings ExplosionSettings;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private class ExplosionSettings
    {
        public float ExplosiveDamage;
        public float ExplosionRadius;
        public ExplosionSettings();
    }

    private class Price
    {
        public bool Enabled;
        public string ItemShortname;
        public int ItemAmount;
        public Price();
    }

    private class Arrow
    {
        public Price ArrowPrice;
        public string Permission;
        public int Uses;
        public Arrow();
    }

    private class ActiveArrow
    {
        public int Uses;
        public ArrowType ArrowType;
        public ActiveArrow();
        public ActiveArrow(ArrowType type);
    }

    private bool IsPlayerArrow(ArrowType type);
    private bool CanUseArrow(BasePlayer player, ArrowType type, BaseCombatEntity combatEntity, Item outItem);
    private void TakeItems(BasePlayer player, Item item);
    private void FireArrow(Vector3 targetPos);
    private void ExplosiveArrow(Vector3 targetPos, BaseCombatEntity combatEnt);
    private bool CanActivateArrow(BasePlayer player, ArrowType type);
    private void DealWithArrow(BasePlayer player, ActiveArrow arrow);
    private void DealWithRemoval(BasePlayer player);
    private string Lang(string key, string id, object[] args);
    private string GetHelpMsg(BasePlayer player);
    private void Init();
    private void OnPlayerAttack(BasePlayer attacker, HitInfo hitInfo);
    [ChatCommand("arrow")]
    private void ArrowCommand(BasePlayer player, string command, string[] args);
}

private class ConfigFile
{
    public Dictionary<ArrowType, Arrow> Arrows;
    public ExplosionSettings ExplosionSettings;
    public static ConfigFile DefaultConfig();
}

private class ExplosionSettings
{
    public float ExplosiveDamage;
    public float ExplosionRadius;
    public ExplosionSettings();
}

private class Price
{
    public bool Enabled;
    public string ItemShortname;
    public int ItemAmount;
    public Price();
}

private class Arrow
{
    public Price ArrowPrice;
    public string Permission;
    public int Uses;
    public Arrow();
}

private class ActiveArrow
{
    public int Uses;
    public ArrowType ArrowType;
    public ActiveArrow();
    public ActiveArrow(ArrowType type);
}


```

---

## AdvancedGather by  - Custom gathering with some actions and extension drop

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Random = UnityEngine.Random;
using Oxide.Plugins.AdvancedGatherEx;
using UnityEngine;
using System;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("AdvancedGather", "Khan", "2.0.4")]
[Description("Custom gathering with some action's and extension drop")]
public class AdvancedGather : CovalencePlugin
{
    [PluginReference]
    private Plugin PopupNotifications;
    private const string UsePerm;
    private PluginConfig _config;
    private IPlayer _player;
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private class PluginConfig
    {
        [JsonProperty("ChatPrefix")]
        public string ChatPrefix;
        [JsonProperty(PropertyName = "Drop Sound Effect")]
        public string DropSoundEffect;
        [JsonProperty(PropertyName = "Enable Berry Drops from Hemp")]
        public bool EnableBerries;
        [JsonProperty(PropertyName = "Enable Random Tree drop Items?")]
        public bool RandomItems;
        [JsonProperty(PropertyName = "Switch to direct inventory Drops?")]
        public bool DirectInventory;
        [JsonProperty(PropertyName = "List of all chance modifiers")]
        public ChanceModifier ChanceModifiers;
        [JsonProperty(PropertyName = "Random Item drops from Trees")]
        public List<RandomConfig> RandomConfigs;
        [JsonProperty(PropertyName = "Get apples from Trees")]
        public AppleConfig AppleConfigs;
        [JsonProperty(PropertyName = "Berry types from Hemp")]
        public List<BerryConfig> BerryConfigs;
        [JsonProperty(PropertyName = "Biofuel options")]
        public BioFuelConfig BioFuelConfigs;
        [JsonProperty(PropertyName = "Set times for optimal farming")]
        public List<TimeConfig> TimeConfigs;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
        public BerryConfig GetRandomBerry();
        public RandomConfig GetRandomTreeItem();
        public static PluginConfig DefaultConfig();
    }

    private class ChanceModifier
    {
        [JsonProperty(PropertyName = "Enable Global Default Drop Chance?")]
        public bool EnableDefaultDropChance;
        [JsonProperty(PropertyName = "Global Default Drop Chance Value")]
        public int DefaultDropChance;
        [JsonProperty(PropertyName = "Enable Time Modifier feature?")]
        public bool TimeModifier;
        [JsonProperty(PropertyName = "Enable Tool Modifier feature?")]
        public bool ToolModifier;
        [JsonProperty(PropertyName = "Tool Modifier bonous chance")]
        public int ToolModifierBonus;
        [JsonProperty(PropertyName = "Tool Item 1 shortname")]
        public string ToolItem1;
        [JsonProperty(PropertyName = "Tool Item 2 shortname")]
        public string ToolItem2;
        [JsonProperty(PropertyName = "Tool Item 3 shortname")]
        public string ToolItem3;
        [JsonProperty(PropertyName = "Enable Attire Modifier feature?")]
        public bool AttireModifier;
        [JsonProperty(PropertyName = "Attire Modifier bonous chance")]
        public int AttireModifierBonus;
        [JsonProperty(PropertyName = "Attire Modifier Item ID")]
        public int AttireBonousID;
        [JsonProperty(PropertyName = "Enable Item Modifier feature?")]
        public bool ItemModifier;
        [JsonProperty(PropertyName = "Item Modifier bonous chance")]
        public int ItemModifierBonus;
        [JsonProperty(PropertyName = "Item Modifier Item ID")]
        public int ItemBonusID;
    }

    private class RandomConfig
    {
        [JsonProperty(PropertyName = "Set Custom DisplayName")]
        public string DisplayName;
        public string Shortname;
        [JsonProperty(PropertyName = "Set Custom Skin Id")]
        public ulong SkinID;
        [JsonProperty(PropertyName = "Minimum amount of random items to drop")]
        public int RandomAmountMin;
        [JsonProperty(PropertyName = "Max amount of random items to drop")]
        public int RandomAmountMax;
        public float Rarity;
        public bool GiveItem(BasePlayer player, int amount);
    }

    private class AppleConfig
    {
        [JsonProperty(PropertyName = "Enable Apple Drops?")]
        public bool Enable;
        [JsonProperty(PropertyName = "Enable Messages for Tree Drops?")]
        public bool AppleMsg;
        [JsonProperty(PropertyName = "Use Popup Message for Apple Drops?")]
        public bool UsePopup;
        [JsonProperty(PropertyName = "Chance to drop any apples per hit")]
        public int GoodApple;
        [JsonProperty(PropertyName = "Chance it drops rotten apples")]
        public int BadApple;
        [JsonProperty(PropertyName = "Minimum amount of Apples")]
        public int MinAmount;
        [JsonProperty(PropertyName = "Max amount of Apples")]
        public int MaxAmount;
    }

    private class BerryConfig
    {
        [JsonProperty(PropertyName = "Set Custom DisplayName")]
        public string DisplayName;
        public string Shortname;
        [JsonProperty(PropertyName = "Set Custom Skin Id")]
        public ulong SkinID;
        [JsonProperty(PropertyName = "Minimum amount of Berries to drop")]
        public int BerryAmountMin;
        [JsonProperty(PropertyName = "Max amount of Berries to drop")]
        public int BerryAmountMax;
        public float Rarity;
        public bool GiveItem(BasePlayer player, int amount);
    }

    public class BioFuelConfig
    {
        [JsonProperty(PropertyName = "Enable Biofuel")]
        public bool EnableBioFuel;
        [JsonProperty(PropertyName = "Biofuel Custom Name")]
        public string DisplayName;
        [JsonProperty(PropertyName = "Biofuel shortname")]
        public string Shortname;
        [JsonProperty(PropertyName = "Set Custom Skin Id")]
        public ulong SkinID;
        public Pumpkin Pumpkins { get; set; }
        public PumpkinClone PumpkinClones { get; set; }
        public Potato Potatos { get; set; }
        public PotatoClone PotatoClones { get; set; }
        public Corn Corns { get; set; }
        public CornClone CornClones { get; set; }
        public bool GiveItem(BasePlayer player, int amount);
    }

    public class Pumpkin
    {
        [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
        public int BioFuelMinAmount;
        [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
        public int BioFuelMaxAmount;
        [JsonProperty(PropertyName = "Sets the chance of it dropping")]
        public float Rarity;
    }

    public class PumpkinClone
    {
        [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
        public int BioFuelMinAmount;
        [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
        public int BioFuelMaxAmount;
        [JsonProperty(PropertyName = "Sets the chance of it dropping")]
        public float Rarity;
    }

    public class Potato
    {
        [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
        public int BioFuelMinAmount;
        [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
        public int BioFuelMaxAmount;
        [JsonProperty(PropertyName = "Sets the chance of it dropping")]
        public float Rarity;
    }

    public class PotatoClone
    {
        [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
        public int BioFuelMinAmount;
        [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
        public int BioFuelMaxAmount;
        [JsonProperty(PropertyName = "Sets the chance of it dropping")]
        public float Rarity;
    }

    public class Corn
    {
        [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
        public int BioFuelMinAmount;
        [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
        public int BioFuelMaxAmount;
        [JsonProperty(PropertyName = "Sets the chance of it dropping")]
        public float Rarity;
    }

    public class CornClone
    {
        [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
        public int BioFuelMinAmount;
        [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
        public int BioFuelMaxAmount;
        [JsonProperty(PropertyName = "Sets the chance of it dropping")]
        public float Rarity;
    }

    private class TimeConfig
    {
        public float After;
        public float Before;
        public int DropChanceBonous;
    }

    private void OnServerInitialized();
    private void OnDispenserGather(ResourceDispenser dispenser, BaseEntity entity, Item item);
    private void OnCollectiblePickup(Item item, BasePlayer player);
    private void OnGrowableGathered(GrowableEntity growable, Item item, BasePlayer player);
    private bool RollRandomItemChance();
    private bool RollBadOrGood();
    public Item AppleChanceRoll(BasePlayer player);
    private void RandomItemRoll(BasePlayer player);
    public void BioFuelDropChanceRoll(BasePlayer player, string shortname);
    public void BerryDropChanceRoll(BasePlayer player);
    private void BerryTypeRoll(BasePlayer player);
    private int DropModifier(BasePlayer player);
    public void PopupMessageArgs(BasePlayer player, string key, object[] args);
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
}

private class PluginConfig
{
    [JsonProperty("ChatPrefix")]
    public string ChatPrefix;
    [JsonProperty(PropertyName = "Drop Sound Effect")]
    public string DropSoundEffect;
    [JsonProperty(PropertyName = "Enable Berry Drops from Hemp")]
    public bool EnableBerries;
    [JsonProperty(PropertyName = "Enable Random Tree drop Items?")]
    public bool RandomItems;
    [JsonProperty(PropertyName = "Switch to direct inventory Drops?")]
    public bool DirectInventory;
    [JsonProperty(PropertyName = "List of all chance modifiers")]
    public ChanceModifier ChanceModifiers;
    [JsonProperty(PropertyName = "Random Item drops from Trees")]
    public List<RandomConfig> RandomConfigs;
    [JsonProperty(PropertyName = "Get apples from Trees")]
    public AppleConfig AppleConfigs;
    [JsonProperty(PropertyName = "Berry types from Hemp")]
    public List<BerryConfig> BerryConfigs;
    [JsonProperty(PropertyName = "Biofuel options")]
    public BioFuelConfig BioFuelConfigs;
    [JsonProperty(PropertyName = "Set times for optimal farming")]
    public List<TimeConfig> TimeConfigs;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
    public BerryConfig GetRandomBerry();
    public RandomConfig GetRandomTreeItem();
    public static PluginConfig DefaultConfig();
}

private class ChanceModifier
{
    [JsonProperty(PropertyName = "Enable Global Default Drop Chance?")]
    public bool EnableDefaultDropChance;
    [JsonProperty(PropertyName = "Global Default Drop Chance Value")]
    public int DefaultDropChance;
    [JsonProperty(PropertyName = "Enable Time Modifier feature?")]
    public bool TimeModifier;
    [JsonProperty(PropertyName = "Enable Tool Modifier feature?")]
    public bool ToolModifier;
    [JsonProperty(PropertyName = "Tool Modifier bonous chance")]
    public int ToolModifierBonus;
    [JsonProperty(PropertyName = "Tool Item 1 shortname")]
    public string ToolItem1;
    [JsonProperty(PropertyName = "Tool Item 2 shortname")]
    public string ToolItem2;
    [JsonProperty(PropertyName = "Tool Item 3 shortname")]
    public string ToolItem3;
    [JsonProperty(PropertyName = "Enable Attire Modifier feature?")]
    public bool AttireModifier;
    [JsonProperty(PropertyName = "Attire Modifier bonous chance")]
    public int AttireModifierBonus;
    [JsonProperty(PropertyName = "Attire Modifier Item ID")]
    public int AttireBonousID;
    [JsonProperty(PropertyName = "Enable Item Modifier feature?")]
    public bool ItemModifier;
    [JsonProperty(PropertyName = "Item Modifier bonous chance")]
    public int ItemModifierBonus;
    [JsonProperty(PropertyName = "Item Modifier Item ID")]
    public int ItemBonusID;
}

private class RandomConfig
{
    [JsonProperty(PropertyName = "Set Custom DisplayName")]
    public string DisplayName;
    public string Shortname;
    [JsonProperty(PropertyName = "Set Custom Skin Id")]
    public ulong SkinID;
    [JsonProperty(PropertyName = "Minimum amount of random items to drop")]
    public int RandomAmountMin;
    [JsonProperty(PropertyName = "Max amount of random items to drop")]
    public int RandomAmountMax;
    public float Rarity;
    public bool GiveItem(BasePlayer player, int amount);
}

private class AppleConfig
{
    [JsonProperty(PropertyName = "Enable Apple Drops?")]
    public bool Enable;
    [JsonProperty(PropertyName = "Enable Messages for Tree Drops?")]
    public bool AppleMsg;
    [JsonProperty(PropertyName = "Use Popup Message for Apple Drops?")]
    public bool UsePopup;
    [JsonProperty(PropertyName = "Chance to drop any apples per hit")]
    public int GoodApple;
    [JsonProperty(PropertyName = "Chance it drops rotten apples")]
    public int BadApple;
    [JsonProperty(PropertyName = "Minimum amount of Apples")]
    public int MinAmount;
    [JsonProperty(PropertyName = "Max amount of Apples")]
    public int MaxAmount;
}

private class BerryConfig
{
    [JsonProperty(PropertyName = "Set Custom DisplayName")]
    public string DisplayName;
    public string Shortname;
    [JsonProperty(PropertyName = "Set Custom Skin Id")]
    public ulong SkinID;
    [JsonProperty(PropertyName = "Minimum amount of Berries to drop")]
    public int BerryAmountMin;
    [JsonProperty(PropertyName = "Max amount of Berries to drop")]
    public int BerryAmountMax;
    public float Rarity;
    public bool GiveItem(BasePlayer player, int amount);
}

public class BioFuelConfig
{
    [JsonProperty(PropertyName = "Enable Biofuel")]
    public bool EnableBioFuel;
    [JsonProperty(PropertyName = "Biofuel Custom Name")]
    public string DisplayName;
    [JsonProperty(PropertyName = "Biofuel shortname")]
    public string Shortname;
    [JsonProperty(PropertyName = "Set Custom Skin Id")]
    public ulong SkinID;
    public Pumpkin Pumpkins { get; set; }
    public PumpkinClone PumpkinClones { get; set; }
    public Potato Potatos { get; set; }
    public PotatoClone PotatoClones { get; set; }
    public Corn Corns { get; set; }
    public CornClone CornClones { get; set; }
    public bool GiveItem(BasePlayer player, int amount);
}

public class Pumpkin
{
    [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
    public int BioFuelMinAmount;
    [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
    public int BioFuelMaxAmount;
    [JsonProperty(PropertyName = "Sets the chance of it dropping")]
    public float Rarity;
}

public class PumpkinClone
{
    [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
    public int BioFuelMinAmount;
    [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
    public int BioFuelMaxAmount;
    [JsonProperty(PropertyName = "Sets the chance of it dropping")]
    public float Rarity;
}

public class Potato
{
    [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
    public int BioFuelMinAmount;
    [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
    public int BioFuelMaxAmount;
    [JsonProperty(PropertyName = "Sets the chance of it dropping")]
    public float Rarity;
}

public class PotatoClone
{
    [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
    public int BioFuelMinAmount;
    [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
    public int BioFuelMaxAmount;
    [JsonProperty(PropertyName = "Sets the chance of it dropping")]
    public float Rarity;
}

public class Corn
{
    [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
    public int BioFuelMinAmount;
    [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
    public int BioFuelMaxAmount;
    [JsonProperty(PropertyName = "Sets the chance of it dropping")]
    public float Rarity;
}

public class CornClone
{
    [JsonProperty(PropertyName = "Minimum amount of biofuel to drop")]
    public int BioFuelMinAmount;
    [JsonProperty(PropertyName = "Max amount of biofuel to drop")]
    public int BioFuelMaxAmount;
    [JsonProperty(PropertyName = "Sets the chance of it dropping")]
    public float Rarity;
}

private class TimeConfig
{
    public float After;
    public float Before;
    public int DropChanceBonous;
}

public static class PlayerEx
{
    public static void RunEffect(BasePlayer player, string prefab);
}

AdvancedGatherEx
public static class PlayerEx
{
    public static void RunEffect(BasePlayer player, string prefab);
}


```

---

## AdvancedUpgrading by Ryan - Advanced upgrading that disallows players from upgrading building blocks quickly

```csharp
using System.Collections.Generic;
using Oxide.Core;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Advanced Upgrading", "Ryan", "1.0.0")]
 class AdvancedUpgrading : RustPlugin
{
    private ConfigFile CFile;
    private DataFileSystem DFS;
    private Dictionary<uint, int> Entities;
    private const string BypassPerm;
    private class ConfigFile
    {
        [JsonProperty("Make hit data persist restarts")]
        public bool Persist;
        [JsonProperty("Grade and amount of hits it needs")]
        public Dictionary<BuildingGrade.Enum, int> Hits;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Regenerate();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void Init();
    private void OnServerSave();
    private void OnNewSave();
    private void OnEntitySpawned(BaseNetworkable entity);
    private object OnStructureUpgrade(BaseCombatEntity entity, BasePlayer player, BuildingGrade.Enum grade);
    private void OnHammerHit(BasePlayer player, HitInfo info);
}

private class ConfigFile
{
    [JsonProperty("Make hit data persist restarts")]
    public bool Persist;
    [JsonProperty("Grade and amount of hits it needs")]
    public Dictionary<BuildingGrade.Enum, int> Hits;
    public static ConfigFile DefaultConfig();
}


```

---

## AdvertMessages by LaserHydra - Allows to set up messages which are broadcasted in a configured interval

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("Advert Messages", "LaserHydra", "3.0.2", ResourceId = 1510)]
[Description("Allows to set up messages which are broadcasted in a configured interval")]
internal class AdvertMessages : CovalencePlugin
{
    private Configuration _config;
    private int _previousAdvert;
    private void Loaded();
    private void BroadcastNextAdvert();
    private int GetNextAdvertIndex();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class Configuration
    {
        [JsonProperty("Messages")]
        public List<string> Messages { get; set; }
        [JsonProperty("Advert Interval (in Minutes)")]
        public float AdvertInterval { get; set; }
        [JsonProperty("Broadcast to Console (true/false)")]
        public bool BroadcastToConsole { get; set; }
        [JsonProperty("Choose Message at Random (true/false)")]
        public bool ChooseMessageAtRandom { get; set; }
        public static Configuration CreateDefault();
    }

}

private class Configuration
{
    [JsonProperty("Messages")]
    public List<string> Messages { get; set; }
    [JsonProperty("Advert Interval (in Minutes)")]
    public float AdvertInterval { get; set; }
    [JsonProperty("Broadcast to Console (true/false)")]
    public bool BroadcastToConsole { get; set; }
    [JsonProperty("Choose Message at Random (true/false)")]
    public bool ChooseMessageAtRandom { get; set; }
    public static Configuration CreateDefault();
}


```

---

## AFK by MrBlue - Kicks players that are AFK (away from keyboard) for too long

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("AFK", "Wulf/lukespragg", "1.1.5", ResourceId = 1922)]
[Description("Kicks players that are AFK (away from keyboard) for too long")]
 class AFK : CovalencePlugin
{
    const string permExcluded;
     void Init();
     void OnServerInitialized();
     int afkLimitMinutes;
     bool kickAfkPlayers;
    protected override void LoadDefaultConfig();
     void LoadDefaultMessages();
     void OnUserConnected(IPlayer player);
    readonly Hash<string, GenericPosition> lastPosition;
    readonly Dictionary<string, Timer> afkTimer;
     void AfkCheck(IPlayer player);
    public bool IsPlayerAfk(IPlayer player);
     void OnUserDisconnected(IPlayer player);
     void ResetPlayer(string id);
     void Unload();
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
     bool HasPermission(string id, string perm);
}


```

---

## AFKAPI by 2CHEVSKII - Complex developer API for "Away from keyboard" players.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("AFK API", "2CHEVSKII", "1.0.1")]
[Description("Complex developer API for 'Away from keyboard' players.")]
 class AFKAPI : CovalencePlugin
{
    const string BEEP_SOUND_PREFAB;
    const string PERMISSION_USE;
    const string PERMISSION_KICK;
     IEnumerable<IPlayer> AFKPlayers { get; set; }
    static AFKAPI instance;
     Settings settings;
     Dictionary<IPlayer, AFKPlayer> trackedPlayers;
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadConfig();
     class Settings
    {
        public static Settings Default { get; set; }
        [JsonProperty(PropertyName = "General Settings")]
        public GeneralPluginSettings GeneralSettings { get; set; }
        [JsonProperty(PropertyName = "Accuracy Settings")]
        public ComparePluginSettings CompareSettings { get; set; }
        [JsonProperty(PropertyName = "Notification Settings")]
        public NotificationPluginSettings NotificationSettings { get; set; }
        public class GeneralPluginSettings
        {
            [JsonProperty(PropertyName = "Seconds to consider player is AFK")]
            public int SecondsToAFKStatus { get; set; }
            [JsonProperty(PropertyName = "AFK Check Interval")]
            public int StatusRefreshInterval { get; set; }
            [JsonProperty(PropertyName = "Allow other plugins change settings of this API")]
            public bool AllowSetupThroughAPI { get; set; }
        }

        public class ComparePluginSettings
        {
            [JsonProperty(PropertyName = "Check rotation in pair with position (more accurate)")]
            public bool CompareRotation { get; set; }
            [JsonProperty(PropertyName = "Check for build attempts")]
            public bool CompareBuild { get; set; }
            [JsonProperty(PropertyName = "Check for communication attempts (chat/voice)")]
            public bool CompareCommunication { get; set; }
            [JsonProperty(PropertyName = "Check for item actions (craft/change/use/move)")]
            public bool CompareItemActions { get; set; }
        }

        public class NotificationPluginSettings
        {
            [JsonProperty(PropertyName = "Notify player before considering him AFK")]
            public bool NotifyPlayer { get; set; }
            [JsonProperty(PropertyName = "Notify player X seconds before considering him AFK")]
            public int NotifyPlayerTime { get; set; }
            [JsonProperty(PropertyName = "Notify with sound")]
            public bool NotifyPlayerSound { get; set; }
        }

    }

    const string M_CHAT_PREFIX;
    const string M_PLAYER_AFK_STATUS;
    const string M_ERROR_NO_ARGS;
    const string M_OFFLINE_STATUS;
    const string M_IS_AFK_STATUS;
    const string M_IS_NOT_AFK_STATUS;
    const string M_AFK_PLAYER_LIST;
    const string M_NO_AFK_PLAYERS_FOUND;
    const string M_NOTIFICATION;
    const string M_NO_PERMISSION;
    const string M_KICK_REASON;
    readonly Dictionary<string, string> defmessages;
    protected override void LoadDefaultMessages();
     void MessagePlayer(IPlayer player, string msg, bool prefix, object[] args);
     bool IsPlayerAFK(ulong id);
     bool IsPlayerAFK(string id);
     bool IsPlayerAFK(IPlayer player);
     long GetPlayerAFKTime(ulong id);
     float GetPlayerAFKTime(string id);
     float GetPlayerAFKTime(IPlayer player);
     List<BasePlayer> GetAFKPlayers();
     bool AFKAPI_Setup(string newSettings, bool needToSave);
     void CheckHookSubscriptions();
     void Init();
     void OnServerInitialized();
     void Unload();
     void OnUserConnected(IPlayer player);
     void OnUserDisconnected(IPlayer player);
     void CanBuild(Planner planner);
     void CanCraft(ItemCrafter itemCrafter);
     void OnActiveItemChanged(BasePlayer player);
     void OnPlayerChat(BasePlayer player);
     void OnPlayerVoice(BasePlayer player);
     void OnItemAction(Item item, string action, BasePlayer player);
     void CanMoveItem(Item item, PlayerInventory playerLoot, uint targetContainer, int targetSlot, int amount);
     void CmdIsAFK(IPlayer player, string command, string[] args);
     void CmdGetAFK(IPlayer player, string command, string[] args);
     void CmdKickAFK(IPlayer player, string command, string[] args);
     class AFKPlayer : MonoBehaviour
    {
        public bool IsAFK { get; set; }
        public float TimeAFK { get; set; }
         float TimeSinceLastAction { get; set; }
         BasePlayer player;
         float lastActionTime;
         Vector3 lastPosition;
         Quaternion lastRotation;
         Dictionary<IPlayer, AFKPlayer> trackedPlayers;
        public void OnAction();
         void Start();
         void OnDestroy();
         void CheckPosition();
         bool RotationChanged();
         void CheckNotification();
         void NotifyPlayer();
         void DoBeepEffect();
    }

}

 class Settings
{
    public static Settings Default { get; set; }
    [JsonProperty(PropertyName = "General Settings")]
    public GeneralPluginSettings GeneralSettings { get; set; }
    [JsonProperty(PropertyName = "Accuracy Settings")]
    public ComparePluginSettings CompareSettings { get; set; }
    [JsonProperty(PropertyName = "Notification Settings")]
    public NotificationPluginSettings NotificationSettings { get; set; }
    public class GeneralPluginSettings
    {
        [JsonProperty(PropertyName = "Seconds to consider player is AFK")]
        public int SecondsToAFKStatus { get; set; }
        [JsonProperty(PropertyName = "AFK Check Interval")]
        public int StatusRefreshInterval { get; set; }
        [JsonProperty(PropertyName = "Allow other plugins change settings of this API")]
        public bool AllowSetupThroughAPI { get; set; }
    }

    public class ComparePluginSettings
    {
        [JsonProperty(PropertyName = "Check rotation in pair with position (more accurate)")]
        public bool CompareRotation { get; set; }
        [JsonProperty(PropertyName = "Check for build attempts")]
        public bool CompareBuild { get; set; }
        [JsonProperty(PropertyName = "Check for communication attempts (chat/voice)")]
        public bool CompareCommunication { get; set; }
        [JsonProperty(PropertyName = "Check for item actions (craft/change/use/move)")]
        public bool CompareItemActions { get; set; }
    }

    public class NotificationPluginSettings
    {
        [JsonProperty(PropertyName = "Notify player before considering him AFK")]
        public bool NotifyPlayer { get; set; }
        [JsonProperty(PropertyName = "Notify player X seconds before considering him AFK")]
        public int NotifyPlayerTime { get; set; }
        [JsonProperty(PropertyName = "Notify with sound")]
        public bool NotifyPlayerSound { get; set; }
    }

}

public class GeneralPluginSettings
{
    [JsonProperty(PropertyName = "Seconds to consider player is AFK")]
    public int SecondsToAFKStatus { get; set; }
    [JsonProperty(PropertyName = "AFK Check Interval")]
    public int StatusRefreshInterval { get; set; }
    [JsonProperty(PropertyName = "Allow other plugins change settings of this API")]
    public bool AllowSetupThroughAPI { get; set; }
}

public class ComparePluginSettings
{
    [JsonProperty(PropertyName = "Check rotation in pair with position (more accurate)")]
    public bool CompareRotation { get; set; }
    [JsonProperty(PropertyName = "Check for build attempts")]
    public bool CompareBuild { get; set; }
    [JsonProperty(PropertyName = "Check for communication attempts (chat/voice)")]
    public bool CompareCommunication { get; set; }
    [JsonProperty(PropertyName = "Check for item actions (craft/change/use/move)")]
    public bool CompareItemActions { get; set; }
}

public class NotificationPluginSettings
{
    [JsonProperty(PropertyName = "Notify player before considering him AFK")]
    public bool NotifyPlayer { get; set; }
    [JsonProperty(PropertyName = "Notify player X seconds before considering him AFK")]
    public int NotifyPlayerTime { get; set; }
    [JsonProperty(PropertyName = "Notify with sound")]
    public bool NotifyPlayerSound { get; set; }
}

 class AFKPlayer : MonoBehaviour
{
    public bool IsAFK { get; set; }
    public float TimeAFK { get; set; }
     float TimeSinceLastAction { get; set; }
     BasePlayer player;
     float lastActionTime;
     Vector3 lastPosition;
     Quaternion lastRotation;
     Dictionary<IPlayer, AFKPlayer> trackedPlayers;
    public void OnAction();
     void Start();
     void OnDestroy();
     void CheckPosition();
     bool RotationChanged();
     void CheckNotification();
     void NotifyPlayer();
     void DoBeepEffect();
}


```

---

## Agriblock by Death - Forces configured plant types to only be planted in planters

```csharp
using UnityEngine;
using System.Collections.Generic;

Oxide.Plugins
[Info("Agriblock", "Death", "1.0.8")]
[Description("Forces configured plant types to only be planted in planters")]
 class Agriblock : RustPlugin
{
    private void OnEntityBuilt(Planner plan, GameObject seed);
     void Init();
    private ConfigData configData;
     class ConfigData
    {
        public Options Options;
        public Types Types;
    }

     class Options
    {
        public bool Refund;
    }

     class Types
    {
        public bool CornEnabled;
        public bool CornCloneEnabled;
        public bool PumpkinEnabled;
        public bool PumpkinCloneEnabled;
        public bool HempEnabled;
        public bool HempCloneEnabled;
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
    public Types Types;
}

 class Options
{
    public bool Refund;
}

 class Types
{
    public bool CornEnabled;
    public bool CornCloneEnabled;
    public bool PumpkinEnabled;
    public bool PumpkinCloneEnabled;
    public bool HempEnabled;
    public bool HempCloneEnabled;
}


```

---

## AirdropPrecision by k1lly0u - Make supply drops fall on the supply signal's position

```csharp

Oxide.Plugins
[Info("Airdrop Precision", "k1lly0u", "0.2.0")]
public class AirdropPrecision : RustPlugin
{
    private const string CARGOPLANE_PREFAB;
    private void OnExplosiveDropped(BasePlayer player, SupplySignal supplySignal);
    private void OnExplosiveThrown(BasePlayer player, SupplySignal supplySignal);
    private void Explode(SupplySignal supplySignal);
}


```

---

## AirdropWithoutParachute by  - Removes the parachute on airdrops

```csharp
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Airdrop Without Parachute", "Enforcer", "1.0.0")]
[Description("Removes the parachute on Airdrops")]
public class AirdropWithoutParachute : RustPlugin
{
    private void Init();
    private void Unload();
    private void OnEntitySpawned(BaseEntity entity);
     ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Airdrop drag")]
        public float airdropDrag { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Airdrop drag")]
    public float airdropDrag { get; set; }
}


```

---

## AirFuel by WhiteDragon - Sets the initial amount of fuel for vendor purchased air vehicles

```csharp
using Newtonsoft.Json;
using System;

Oxide.Plugins
[Info("Air Fuel", "WhiteDragon", "0.2.3")]
[Description("Sets the initial amount of fuel for vendor purchased air vehicles.")]
public class AirFuel : CovalencePlugin
{
    private static AirFuel _instance;
    private static Configuration config;
    private class Configuration
    {
        public Fuel.Settings Fuel;
        public Version.Settings Version;
        private static bool corrupt;
        private static bool dirty;
        public static void Clamp(T value, T min, T max);
        public static void Load();
        public static void Save();
        public static void SetDirty();
        public static void Unload();
        public static void Validate(T value, Func<T> initializer, Action validator);
        private static void Validate();
    }

    private class Fuel
    {
        public class Settings
        {
            public int Default;
            public int MiniCopter;
            public int ScrapTransportHelicopter;
            public Settings();
            public void Validate();
        }

    }

    private class Generic
    {
        public static T Clamp(T value, T min, T max);
    }

    private void Init();
    protected override void LoadDefaultConfig();
    private void OnServerInitialized();
    private void OnEntitySpawned(Minicopter vehicle);
    private void Unload();
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

private class Configuration
{
    public Fuel.Settings Fuel;
    public Version.Settings Version;
    private static bool corrupt;
    private static bool dirty;
    public static void Clamp(T value, T min, T max);
    public static void Load();
    public static void Save();
    public static void SetDirty();
    public static void Unload();
    public static void Validate(T value, Func<T> initializer, Action validator);
    private static void Validate();
}

private class Fuel
{
    public class Settings
    {
        public int Default;
        public int MiniCopter;
        public int ScrapTransportHelicopter;
        public Settings();
        public void Validate();
    }

}

public class Settings
{
    public int Default;
    public int MiniCopter;
    public int ScrapTransportHelicopter;
    public Settings();
    public void Validate();
}

private class Generic
{
    public static T Clamp(T value, T min, T max);
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

## AirLootSupply by  - Allows supply to be loot when dropping

```csharp

Oxide.Plugins
[Info("Air Loot Supply", "Arainrr", "1.0.0")]
[Description("Allow supply to be loot when it's dropping")]
public class AirLootSupply : RustPlugin
{
    private void OnServerInitialized();
    private void OnEntitySpawned(SupplyDrop supplyDrop);
}


```

---

## Airstrike by k1lly0u - Call an airstrike using a supply signal/chat command

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using UnityEngine;
using System.Linq;
using System.Reflection;

Oxide.Plugins
[Info("Airstrike", "k1lly0u", "0.3.6", ResourceId = 1489)]
 class Airstrike : RustPlugin
{
    [PluginReference]
     Plugin Economics;
     Plugin ServerRewards;
     StoredData storedData;
    private DynamicConfigFile data;
    private Dictionary<ulong, StrikeType> toggleList;
    private Dictionary<string, int> shortnameToId;
    private Dictionary<string, string> shortnameToDn;
    private static Airstrike ins;
    const string cargoPlanePrefab;
    const string basicRocket;
    const string fireRocket;
    private void Loaded();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private class StrikePlane : MonoBehaviour
    {
        private CargoPlane entity;
        private Vector3 targetPos;
        private RocketOptions rocketOptions;
        private Vector3 startPos;
        private Vector3 endPos;
        private float secondsToTake;
        private int rocketsFired;
        private float fireDistance;
        private bool isFiring;
        private void Awake();
        private void Update();
        private void OnDestroy();
        public void InitializeFlightPath(Vector3 targetPos);
        public void GetFlightData(Vector3 startPos, Vector3 endPos, float secondsToTake);
        public void SetFlightData(Vector3 startPos, Vector3 endPos, Vector3 targetPos, float secondsToTake);
        private void FireRocketLoop();
        private void LaunchRocket();
        private float GetRandom();
    }

    private void CallRandomStrike();
    private void CallStrike(Vector3 position);
    private void CallSquad(Vector3 position);
    private bool CanBuyStrike(BasePlayer player, StrikeType type);
    private void BuyStrike(BasePlayer player, StrikeType type);
    private double GrabCurrentTime();
    private CargoPlane CreatePlane();
    private bool isStrikePlane(CargoPlane plane);
    private bool HasPermission(BasePlayer player, string perm);
    private Vector3 GetRandomPosition();
    private string FormatTime(double time);
    private void AddCooldownData(BasePlayer player, StrikeType type);
    private List<BasePlayer> FindPlayer(string arg);
    [ChatCommand("airstrike")]
    private void cmdAirstrike(BasePlayer player, string command, string[] args);
    [ConsoleCommand("airstrike")]
    private void ccmdAirstrike(ConsoleSystem.Arg arg);
    private ConfigData configData;
     class RocketOptions
    {
        [JsonProperty(PropertyName = "Speed of the rocket")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Damage modifier")]
        public float Damage { get; set; }
        [JsonProperty(PropertyName = "Accuracy of rocket (a lower number is more accurate)")]
        public float Accuracy { get; set; }
        [JsonProperty(PropertyName = "Interval between rockets (seconds)")]
        public float Interval { get; set; }
        [JsonProperty(PropertyName = "Type of rocket (Normal, Napalm)")]
        public string Type { get; set; }
        [JsonProperty(PropertyName = "Use both rocket types")]
        public bool Mixed { get; set; }
        [JsonProperty(PropertyName = "Chance of a fire rocket (when using both types)")]
        public int FireChance { get; set; }
        [JsonProperty(PropertyName = "Amount of rockets to fire")]
        public int Amount { get; set; }
    }

     class CooldownOptions
    {
        [JsonProperty(PropertyName = "Use cooldown timers")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Strike cooldown time (seconds)")]
        public int Strike { get; set; }
        [JsonProperty(PropertyName = "Squad cooldown time (seconds)")]
        public int Squad { get; set; }
    }

     class PlaneOptions
    {
        [JsonProperty(PropertyName = "Flight speed (meters per second)")]
        public float Speed { get; set; }
        [JsonProperty(PropertyName = "Distance from target to engage")]
        public float Distance { get; set; }
        [JsonProperty(PropertyName = "Height modifier")]
        public float Height { get; set; }
    }

     class BuyOptions
    {
        [JsonProperty(PropertyName = "Can purchase standard strike")]
        public bool StrikeEnabled { get; set; }
        [JsonProperty(PropertyName = "Can purchase squad strike")]
        public bool SquadEnabled { get; set; }
        [JsonProperty(PropertyName = "Require permission to purchase strike")]
        public bool PermissionStrike { get; set; }
        [JsonProperty(PropertyName = "Require permission to purchase squad strike")]
        public bool PermissionSquad { get; set; }
        [JsonProperty(PropertyName = "Cost to purchase a standard strike (shortname, amount)")]
        public Dictionary<string, int> StrikeCost { get; set; }
        [JsonProperty(PropertyName = "Cost to purchase a squad strike (shortname, amount)")]
        public Dictionary<string, int> SquadCost { get; set; }
    }

     class OtherOptions
    {
        [JsonProperty(PropertyName = "Broadcast strikes to chat")]
        public bool Broadcast { get; set; }
        [JsonProperty(PropertyName = "Can call standard strikes using a supply signal")]
        public bool SignalStrike { get; set; }
        [JsonProperty(PropertyName = "Can call squad strikes using a supply signal")]
        public bool SignalSquad { get; set; }
        [JsonProperty(PropertyName = "Use random airstrikes")]
        public bool RandomStrikes { get; set; }
        [JsonProperty(PropertyName = "Use random squad strikes")]
        public bool RandomSquads { get; set; }
        [JsonProperty(PropertyName = "Random timer (minimum, maximum. In seconds)")]
        public int[] RandomTimer { get; set; }
    }

     class ConfigData
    {
        [JsonProperty(PropertyName = "Rocket Options")]
        public RocketOptions Rocket { get; set; }
        [JsonProperty(PropertyName = "Cooldown Options")]
        public CooldownOptions Cooldown { get; set; }
        [JsonProperty(PropertyName = "Plane Options")]
        public PlaneOptions Plane { get; set; }
        [JsonProperty(PropertyName = "Purchase Options")]
        public BuyOptions Buy { get; set; }
        [JsonProperty(PropertyName = "Other Options")]
        public OtherOptions Other { get; set; }
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
    private void SaveConfig(ConfigData config);
    private void SaveData();
    private void LoadData();
    private void ClearData();
     class StoredData
    {
        public Dictionary<ulong, CooldownData> cooldowns;
    }

     class CooldownData
    {
        public double strikeCd;
        public double squadCd;
    }

     string msg(string key, string playerId);
     Dictionary<string, string> Messages;
}

private class StrikePlane : MonoBehaviour
{
    private CargoPlane entity;
    private Vector3 targetPos;
    private RocketOptions rocketOptions;
    private Vector3 startPos;
    private Vector3 endPos;
    private float secondsToTake;
    private int rocketsFired;
    private float fireDistance;
    private bool isFiring;
    private void Awake();
    private void Update();
    private void OnDestroy();
    public void InitializeFlightPath(Vector3 targetPos);
    public void GetFlightData(Vector3 startPos, Vector3 endPos, float secondsToTake);
    public void SetFlightData(Vector3 startPos, Vector3 endPos, Vector3 targetPos, float secondsToTake);
    private void FireRocketLoop();
    private void LaunchRocket();
    private float GetRandom();
}

 class RocketOptions
{
    [JsonProperty(PropertyName = "Speed of the rocket")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Damage modifier")]
    public float Damage { get; set; }
    [JsonProperty(PropertyName = "Accuracy of rocket (a lower number is more accurate)")]
    public float Accuracy { get; set; }
    [JsonProperty(PropertyName = "Interval between rockets (seconds)")]
    public float Interval { get; set; }
    [JsonProperty(PropertyName = "Type of rocket (Normal, Napalm)")]
    public string Type { get; set; }
    [JsonProperty(PropertyName = "Use both rocket types")]
    public bool Mixed { get; set; }
    [JsonProperty(PropertyName = "Chance of a fire rocket (when using both types)")]
    public int FireChance { get; set; }
    [JsonProperty(PropertyName = "Amount of rockets to fire")]
    public int Amount { get; set; }
}

 class CooldownOptions
{
    [JsonProperty(PropertyName = "Use cooldown timers")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Strike cooldown time (seconds)")]
    public int Strike { get; set; }
    [JsonProperty(PropertyName = "Squad cooldown time (seconds)")]
    public int Squad { get; set; }
}

 class PlaneOptions
{
    [JsonProperty(PropertyName = "Flight speed (meters per second)")]
    public float Speed { get; set; }
    [JsonProperty(PropertyName = "Distance from target to engage")]
    public float Distance { get; set; }
    [JsonProperty(PropertyName = "Height modifier")]
    public float Height { get; set; }
}

 class BuyOptions
{
    [JsonProperty(PropertyName = "Can purchase standard strike")]
    public bool StrikeEnabled { get; set; }
    [JsonProperty(PropertyName = "Can purchase squad strike")]
    public bool SquadEnabled { get; set; }
    [JsonProperty(PropertyName = "Require permission to purchase strike")]
    public bool PermissionStrike { get; set; }
    [JsonProperty(PropertyName = "Require permission to purchase squad strike")]
    public bool PermissionSquad { get; set; }
    [JsonProperty(PropertyName = "Cost to purchase a standard strike (shortname, amount)")]
    public Dictionary<string, int> StrikeCost { get; set; }
    [JsonProperty(PropertyName = "Cost to purchase a squad strike (shortname, amount)")]
    public Dictionary<string, int> SquadCost { get; set; }
}

 class OtherOptions
{
    [JsonProperty(PropertyName = "Broadcast strikes to chat")]
    public bool Broadcast { get; set; }
    [JsonProperty(PropertyName = "Can call standard strikes using a supply signal")]
    public bool SignalStrike { get; set; }
    [JsonProperty(PropertyName = "Can call squad strikes using a supply signal")]
    public bool SignalSquad { get; set; }
    [JsonProperty(PropertyName = "Use random airstrikes")]
    public bool RandomStrikes { get; set; }
    [JsonProperty(PropertyName = "Use random squad strikes")]
    public bool RandomSquads { get; set; }
    [JsonProperty(PropertyName = "Random timer (minimum, maximum. In seconds)")]
    public int[] RandomTimer { get; set; }
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Rocket Options")]
    public RocketOptions Rocket { get; set; }
    [JsonProperty(PropertyName = "Cooldown Options")]
    public CooldownOptions Cooldown { get; set; }
    [JsonProperty(PropertyName = "Plane Options")]
    public PlaneOptions Plane { get; set; }
    [JsonProperty(PropertyName = "Purchase Options")]
    public BuyOptions Buy { get; set; }
    [JsonProperty(PropertyName = "Other Options")]
    public OtherOptions Other { get; set; }
}

 class StoredData
{
    public Dictionary<ulong, CooldownData> cooldowns;
}

 class CooldownData
{
    public double strikeCd;
    public double squadCd;
}


```

---

## AliasSystem by LaserHydra - Allows you to create aliases for existing commands

```csharp
using System.Collections.Generic;
using Oxide.Game.Rust.Libraries;
using System.Linq;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Alias System", "LaserHydra", "2.1.3", ResourceId = 1307)]
[Description("Setup alias for chat and console commands")]
 class AliasSystem : RustPlugin
{
    const string permadmin;
    const string permuse;
     class Alias
    {
        public string original;
        public string alias;
        public string originaltype;
        public string aliastype;
        public string permission;
        public Alias(string aliasname, string Original);
        public Alias();
    }

     class Data
    {
        public List<Alias> alias;
        public Data();
    }

     Data data;
     Data LoadData();
     void SaveData();
     void Loaded();
     void LoadConfig();
     void LoadDefaultConfig();
     void ChatAlias(BasePlayer player, string command, string[] args);
     void ConsoleAlias(ConsoleSystem.Arg arg);
    [ChatCommand("alias")]
     void cmdAlias(BasePlayer player, string command, string[] args);
     Alias GetAliasByName(string aliasname);
     void RunChatCommand(BasePlayer player, string command, string args);
     void RunConsoleCommand(BasePlayer player, string command, string args);
     BasePlayer GetPlayer(string searchedPlayer, BasePlayer executer, string prefix);
     string ListToString(List<string> list, int first, string seperator);
     string ArgToString(ConsoleSystem.Arg arg, int first, string seperator);
     void SetConfig(string Arg1, object Arg2, object Arg3, object Arg4);
     void BroadcastChat(string prefix, string msg);
     void SendChatMessage(BasePlayer player, string prefix, string msg);
}

 class Alias
{
    public string original;
    public string alias;
    public string originaltype;
    public string aliastype;
    public string permission;
    public Alias(string aliasname, string Original);
    public Alias();
}

 class Data
{
    public List<Alias> alias;
    public Data();
}


```

---

## AllBagsCooldown by Flashtim - This plugin synchronizes the unlock time of all sleeping bags / beds of a player.

```csharp

Oxide.Plugins
[Info("All Bags Cooldown", "Flashtim", "1.0.4")]
[Description("Put same cooldown on all sleeping bags of player")]
 class AllBagsCooldown : RustPlugin
{
    private const string permAllow;
    private void Init();
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void ResetSpawnTargets(BasePlayer player);
}


```

---

## AlwaysDay by  - Stops time on your server at one time

```csharp
using Newtonsoft.Json;

Oxide.Plugins
[Info("Always Day", "Orange", "1.0.1")]
[Description("Stops time on your server at one time")]
public class AlwaysDay : RustPlugin
{
    private TOD_Time time;
    private void OnServerInitialized();
    private void Unload();
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Time")]
        public string time;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Time")]
    public string time;
}


```

---

## AlwaysLootableTurrets by WhiteThunder - Allows players to loot auto turrets while powered

```csharp
using Network;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Always Lootable Turrets", "WhiteThunder", "1.0.1")]
[Description("Allows players to loot auto turrets while powered.")]
internal class AlwaysLootableTurrets : CovalencePlugin
{
    private const string PermissionOwner;
    private const string PrefabCodeLockDeniedEffect;
    private Configuration _pluginConfig;
    private void Init();
    private object CanLootEntity(BasePlayer player, AutoTurret turret);
    private void OnEntitySaved(AutoTurret turret, BaseNetworkable.SaveInfo saveInfo);
    private object OnEntityFlagsNetworkUpdate(AutoTurret turret);
    private bool IsTurretEligible(AutoTurret turret);
    private bool TurretHasPermission(AutoTurret turret);
    private void SendFlagUpdate(AutoTurret turret);
    private int RemoveOnFlag(int flags);
    internal class Configuration : SerializableConfiguration
    {
        [JsonProperty("RequireOwnerPermission")]
        public bool RequireOwnerPermission;
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
}

internal class Configuration : SerializableConfiguration
{
    [JsonProperty("RequireOwnerPermission")]
    public bool RequireOwnerPermission;
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


```

---

## AmmoHUD by beee - Players can view available ammo and type on screen.

```csharp
using System;
using System.Globalization;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Ammo HUD", "beee", "1.2.1")]
[Description("Allows players to show an Ammo HUD.")]
public class AmmoHUD : RustPlugin
{
    public static AmmoHUD _plugin;
    private static PluginConfig _config;
    [PluginReference]
    private Plugin ImageLibrary;
    public PlayersData pData;
    public const string PREFIX_SHORT;
    public const string PREFIX_LONG;
    public const string PERM_ADMIN;
    public const string PERM_USE;
    private class PluginConfig
    {
        public Oxide.Core.VersionNumber Version;
        [JsonProperty("General Settings")]
        public GeneralConfigSettings GeneralSettings { get; set; }
        [JsonProperty("Position Settings")]
        public PositionConfigSettings PositionSettings { get; set; }
        internal class GeneralConfigSettings
        {
            [JsonProperty("Show Text Outline")]
            public bool ShowOutline { get; set; }
            [JsonProperty("Show Individual Ammo Icons")]
            public bool ShowAmmoIcons { get; set; }
            [JsonProperty("Show Throwables")]
            public bool ShowThrowables { get; set; }
        }

        internal class PositionConfigSettings
        {
            [JsonProperty("Default State (true = on, false = off)")]
            public bool DefaultState { get; set; }
            [JsonProperty(
                    "Position (Top, TopLeft, TopRight, Left, Right, Bottom, BottomLeft, BottomRight)"
                )]
            public string Position { get; set; }
            [JsonProperty("Custom Position")]
            public CustomPositionConfigSettings CustomPositionSettings { get; set; }
            internal class CustomPositionConfigSettings
            {
                [JsonProperty("Enabled")]
                public bool Enabled { get; set; }
                [JsonProperty("Custom Position")]
                public PositionPreset CustomPosition { get; set; }
            }

        }

    }

    protected override void LoadDefaultConfig();
    private PluginConfig GetDefaultConfig();
    private void CheckForConfigUpdates();
    protected override void LoadConfig();
    protected override void SaveConfig();
     void OnServerInitialized();
     void Loaded();
     void Unload();
     void RegisterMessages();
    private void OnServerSave();
    private bool wiped { get; set; }
    private void OnNewSave(string filename);
    private void SaveData();
    public class PlayersData
    {
        public Dictionary<ulong, PlayerInfo> Players;
        public static PlayersData Load();
        public void Save();
        public bool ToggleHUDActive(ulong playerid);
        public bool IsActive(ulong playerid);
        public void SetWeaponActive(ulong playerid, bool active);
        public bool IsWeaponActive(ulong playerid);
        public PositionPreset GetPosition(ulong playerid);
        public void SetPosition(ulong playerid, string position);
    }

    public class PlayerInfo
    {
        public bool HUDActive;
        public string Position;
        [JsonIgnore]
        public bool WeaponActive;
    }

    [ChatCommand("ammohud")]
     void AmmoHUDChatCMD(BasePlayer player, string command, string[] args);
    public void BroadcastCommands(BasePlayer player);
     void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
     void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProtoBuf.ProjectileShoot projectiles);
     void OnRocketLaunched(BasePlayer player, BaseEntity entity);
     object OnAmmoUnload(BaseProjectile weapon, Item item, BasePlayer player);
     object OnMagazineReload(BaseProjectile weapon, IAmmoContainer ammoSource, BasePlayer player);
     void OnItemAddedToContainer(ItemContainer container, Item item);
     void OnItemRemovedFromContainer(ItemContainer container, Item item);
     void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
     void OnExplosiveDropped(BasePlayer player, BaseEntity entity);
     string defaultAmmoColor;
     string redAmmoColor;
     string orangeAmmoColor;
     string defaultIconColor;
     string blueIconColor;
     string redIconColor;
     string blackIconColor;
     string greyIconColor;
     string greenIconColor;
    public void LoadColors();
    public string Layer;
    public string uisuffix;
    private void UpdateUI(BasePlayer player, AttackEntity item, UpdateEnum uiToUpdate);
    private string GetColorFromHex(string hex, double alpha);
    private string GetImg(string name);
    public void Reply(BasePlayer player, string msg);
    static string fontColor1;
    static string fontColor2;
    private void BroadcastToAll(string msg);
    private void BroadcastToPlayer(BasePlayer player, string msg);
     Dictionary<string, string> messages;
    public PositionPreset GetConfigPosition();
    public PositionPreset GetPreset(string position);
    public void LoadDefaultPresets();
    public List<PositionPreset> PositionPresets;
    public class PositionPreset
    {
        [JsonIgnore]
        public PositionEnum Position { get; set; }
        public PositionParams ParentPosition { get; set; }
        public PositionParams WeaponAmmoPosition { get; set; }
        public int WeaponAmmoFontSize { get; set; }
        public TextAnchor WeaponAmmoTextAlignment { get; set; }
        public PositionParams TotalAmmoPosition { get; set; }
        public int TotalAmmoFontSize { get; set; }
        public TextAnchor TotalAmmoTextAlignment { get; set; }
        public PositionParams IconPosition { get; set; }
    }

    public class PositionParams
    {
        public bool Enabled { get; set; }
        public string AnchorMin { get; set; }
        public string AnchorMax { get; set; }
        public string OffsetMin { get; set; }
        public string OffsetMax { get; set; }
    }

}

private class PluginConfig
{
    public Oxide.Core.VersionNumber Version;
    [JsonProperty("General Settings")]
    public GeneralConfigSettings GeneralSettings { get; set; }
    [JsonProperty("Position Settings")]
    public PositionConfigSettings PositionSettings { get; set; }
    internal class GeneralConfigSettings
    {
        [JsonProperty("Show Text Outline")]
        public bool ShowOutline { get; set; }
        [JsonProperty("Show Individual Ammo Icons")]
        public bool ShowAmmoIcons { get; set; }
        [JsonProperty("Show Throwables")]
        public bool ShowThrowables { get; set; }
    }

    internal class PositionConfigSettings
    {
        [JsonProperty("Default State (true = on, false = off)")]
        public bool DefaultState { get; set; }
        [JsonProperty(
                    "Position (Top, TopLeft, TopRight, Left, Right, Bottom, BottomLeft, BottomRight)"
                )]
        public string Position { get; set; }
        [JsonProperty("Custom Position")]
        public CustomPositionConfigSettings CustomPositionSettings { get; set; }
        internal class CustomPositionConfigSettings
        {
            [JsonProperty("Enabled")]
            public bool Enabled { get; set; }
            [JsonProperty("Custom Position")]
            public PositionPreset CustomPosition { get; set; }
        }

    }

}

internal class GeneralConfigSettings
{
    [JsonProperty("Show Text Outline")]
    public bool ShowOutline { get; set; }
    [JsonProperty("Show Individual Ammo Icons")]
    public bool ShowAmmoIcons { get; set; }
    [JsonProperty("Show Throwables")]
    public bool ShowThrowables { get; set; }
}

internal class PositionConfigSettings
{
    [JsonProperty("Default State (true = on, false = off)")]
    public bool DefaultState { get; set; }
    [JsonProperty(
                    "Position (Top, TopLeft, TopRight, Left, Right, Bottom, BottomLeft, BottomRight)"
                )]
    public string Position { get; set; }
    [JsonProperty("Custom Position")]
    public CustomPositionConfigSettings CustomPositionSettings { get; set; }
    internal class CustomPositionConfigSettings
    {
        [JsonProperty("Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty("Custom Position")]
        public PositionPreset CustomPosition { get; set; }
    }

}

internal class CustomPositionConfigSettings
{
    [JsonProperty("Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty("Custom Position")]
    public PositionPreset CustomPosition { get; set; }
}

public class PlayersData
{
    public Dictionary<ulong, PlayerInfo> Players;
    public static PlayersData Load();
    public void Save();
    public bool ToggleHUDActive(ulong playerid);
    public bool IsActive(ulong playerid);
    public void SetWeaponActive(ulong playerid, bool active);
    public bool IsWeaponActive(ulong playerid);
    public PositionPreset GetPosition(ulong playerid);
    public void SetPosition(ulong playerid, string position);
}

public class PlayerInfo
{
    public bool HUDActive;
    public string Position;
    [JsonIgnore]
    public bool WeaponActive;
}

public class PositionPreset
{
    [JsonIgnore]
    public PositionEnum Position { get; set; }
    public PositionParams ParentPosition { get; set; }
    public PositionParams WeaponAmmoPosition { get; set; }
    public int WeaponAmmoFontSize { get; set; }
    public TextAnchor WeaponAmmoTextAlignment { get; set; }
    public PositionParams TotalAmmoPosition { get; set; }
    public int TotalAmmoFontSize { get; set; }
    public TextAnchor TotalAmmoTextAlignment { get; set; }
    public PositionParams IconPosition { get; set; }
}

public class PositionParams
{
    public bool Enabled { get; set; }
    public string AnchorMin { get; set; }
    public string AnchorMax { get; set; }
    public string OffsetMin { get; set; }
    public string OffsetMax { get; set; }
}


```

---

## Analytics by MrBlue - Real-time collection and reporting of server events to Google Analytics

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.IO;

Oxide.Plugins
[Info("Analytics", "Wulf/lukespragg", "1.1.2")]
[Description("Real-time collection and reporting of server events to Google Analytics")]
public class Analytics : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Google tracking ID (ex. UA-XXXXXXXX-Y)")]
        public string TrackingId { get; set; }
        [JsonProperty(PropertyName = "Track connections (true/false)")]
        public bool TrackConnections { get; set; }
        [JsonProperty(PropertyName = "Track disconnections (true/false)")]
        public bool TrackDisconnections { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private static readonly Dictionary<string, string> Headers;
    private void Collect(IPlayer player, string session);
    private void CollectAll(string session);
    private void OnServerInitialized();
    private void OnServerSave();
    private void OnServerShutdown();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Google tracking ID (ex. UA-XXXXXXXX-Y)")]
    public string TrackingId { get; set; }
    [JsonProperty(PropertyName = "Track connections (true/false)")]
    public bool TrackConnections { get; set; }
    [JsonProperty(PropertyName = "Track disconnections (true/false)")]
    public bool TrackDisconnections { get; set; }
}


```

---

## AngryAuthlevel by Tori1157 - Automatically gives or removes Authlevels for users.

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("AngryAuthlevel", "Tori1157", "2.0.0")]
[Description("Automatically gives or removes Authlevel for users.")]
 class AngryAuthlevel : CovalencePlugin
{
    private bool Changed;
    private bool delayedAdd;
    private bool loadedAdd;
    private bool addingMessage;
    private bool logAdding;
    private bool removeUnlisted;
    private string Auth1List;
    private string Auth2List;
    private const string UpdatePermission;
    private const string logFilename;
    private void Init();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void Loaded();
    protected override void LoadDefaultMessages();
    private void AddingAuthCheck();
    private void LoadingAdd();
    private void OnUserConnected(IPlayer player);
    [Command("updateauth", "updateauths")]
    private void AuthCommand(IPlayer player, string command, string[] args);
     void AddAuths(int level, IPlayer adder, IPlayer player, bool singleAdd);
    private bool CanUpdate(IPlayer player);
     bool IsMod(string steamId);
     bool IsOwner(string steamId);
    private string AuthList(string level);
    private string AuthenticationList(List<object> list);
     object GetConfig(string menu, string datavalue, object defaultValue);
    private bool BoolConfig(string menu, string dataValue, bool defaultValue);
    private string StringConfig(string menu, string dataValue, string defaultValue);
    private string ListConfig(string menu, string dataValue, List<string> defaultValue);
    private string Lang(string key, string id, object[] args);
    private void Log(string filename, string text);
}


```

---

## AngryBounds by Tori1157 - Prevents players from building outside of map bounds

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("AngryBounds", "Tori1157", "1.2.0")]
[Description("Prevents players from building outside of map bounds.")]
public class AngryBounds : RustPlugin
{
    private bool Changed;
    private decimal boundChange;
    private void Init();
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private void OnEntityBuilt(Planner plan, GameObject go);
    private bool CheckPlayerPosition(BasePlayer player);
     string ClearName(string item);
    private object GetConfig(string menu, string datavalue, object defaultValue);
}


```

---

## AngryPromotion by Tori1157 - Automatically add players to a group if they have a specific word/phrase in their Steam name

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("AngryPromotion", "Tori1157", "1.1.1", ResourceId = 2686)]
[Description("Automatically add users to a group if they have a specific word/phrase in their steam name.")]
 class AngryPromotion : CovalencePlugin
{
    private bool Changed;
    private bool printToConsole;
    private bool informPlayer;
    private bool groupAdding;
    private bool useGroupInfo;
    private bool removeReward;
    private bool useThanksMessage;
    private string messagePrefix;
    private string messageColor;
    private string promotionKey;
    private string groupKey;
    private string groupInformation;
    private string thanksMessage;
    private float messageDelay;
    private const string AdminPermission;
    private void Init();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    protected override void LoadDefaultMessages();
    [Command("promotion")]
    private void PromotionCommand(IPlayer player, string command, string[] args);
    private void OnUserConnected(IPlayer player);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private IPlayer GetPlayer(string nameOrID, IPlayer player);
    private bool IsParseableTo(object s);
    private static Dictionary<string, PromotingInfo> promoters;
    public class PromotingInfo
    {
        public static bool IsPromoting(IPlayer player);
        public PromotingInfo();
    }

    private void LoadData(T data, string filename);
    private void SaveData(T data, string filename);
    private void SendChatMessage(IPlayer player, string message);
    private void SendInfoMessage(IPlayer player, string message);
    private void SendThanksMessage(IPlayer player, string message);
}

public class PromotingInfo
{
    public static bool IsPromoting(IPlayer player);
    public PromotingInfo();
}


```

---

## AngryTime by Tori1157 - Control and check game time

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("AngryTime", "Tori1157", "1.3.1", ResourceId = 2670)]
[Description("Control & Check time via one plugin.")]
 class AngryTime : CovalencePlugin
{
    private bool Changed;
    private bool useRealTime;
    private bool informSkipConsole;
    private bool useSkipTime;
    private bool useRealDate;
    private string messagePrefix;
    private string messagePrefixColor;
    private int sunriseHour;
    private int sunsetHour;
    private const string adminPermission;
    private void Loaded();
    private void Init();
    private void LoadVariables();
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    [Command("time")]
    private void TimeCommand(IPlayer player, string command, string[] args);
    private void UseRealTimeClock();
    private void CheckTimeCycle();
    private void InitDate();
    private void RealDateTime();
    private void RealTime();
    private void RealDate();
    private bool CanAdmin(IPlayer player);
    private bool CheckTimes();
    private void NoCollideCheck();
     string Lang(string key, string id, object[] args);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private void SendChatMessage(IPlayer player, string message);
    private void SendInfoMessage(IPlayer player, string message);
}


```

---

## AnimalHome by KrunghCrow - Adds a roam radius to animals around their spawn points.

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Animal Home", "Krungh Crow", "1.0.3")]
[Description("Adding a animal roam radius")]
public class AnimalHome : RustPlugin
{
    public static AnimalHome instance;
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void OnStart();
    private void OnEnd();
    private void AddLogic(BaseNetworkable entity);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Time between checks")]
        public float timer;
        [JsonProperty(PropertyName = "Max distance between Home and NPC")]
        public float distance;
        [JsonProperty(PropertyName = "Blocked NPC types (logic will not work for them)")]
        public List<string> blocked;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class OLogic : MonoBehaviour
    {
        private Vector3 home;
        private BaseNpc npc;
        private float distance;
        private float time;
        private void Awake();
        private void CheckDistance();
        public void DoDestroy();
        private void OnDestroy();
    }

}

private class ConfigData
{
    [JsonProperty(PropertyName = "Time between checks")]
    public float timer;
    [JsonProperty(PropertyName = "Max distance between Home and NPC")]
    public float distance;
    [JsonProperty(PropertyName = "Blocked NPC types (logic will not work for them)")]
    public List<string> blocked;
}

private class OLogic : MonoBehaviour
{
    private Vector3 home;
    private BaseNpc npc;
    private float distance;
    private float time;
    private void Awake();
    private void CheckDistance();
    public void DoDestroy();
    private void OnDestroy();
}


```

---

## AntiAmbush by CortexInfamy - Anti-Ambush is a Rust plugin that alerts a user being aimed at after a certain amount of time.

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("AntiAmbush", "Cortex Network ~ Infamy", "1.0.0")]
[Description("Notifies a player when they are being aimed at after a specified time")]
 class AntiAmbush : RustPlugin
{
    private Dictionary<ulong, float> playerAimingTimestamps;
    private Dictionary<ulong, BasePlayer> aimedPlayers;
    private float AimingDelay { get; set; }
    protected override void LoadDefaultConfig();
    private void Init();
    private void CheckPlayerAiming(BasePlayer player);
    private BasePlayer GetAimedPlayer(BasePlayer player);
    private void NotifyAimedPlayer(BasePlayer aimedPlayer, BasePlayer aimer);
}


```

---

## AntiBandit by  - Alters PvP into zones created by chat commands; designed for RPG servers

```csharp
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Anti Bandit", "Alphawar", "0.9.3")]
[Description("Designed to assist servers with RDM (designed for RPG servers)")]
 class AntiBandit : RustPlugin
{
    [PluginReference]
    readonly Plugin RustIOFriendListAPI;
    private List<Timer> tTimers;
    private Hash<ulong, double> PlayerCooldownList;
    private List<Vector3> ExpiredPvPZonesList;
    private List<Vector3> ExpiredRaidZonesList;
    private Hash<Vector3, PVPData> PvPList;
    private Hash<Vector3, RaidDetails> raidZoneList;
     void OnPlayerConnected(BasePlayer _player);
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    [ChatCommand("antibandit")]
     void ChatSettings(BasePlayer player, string cmd, string[] args);
    [ChatCommand("raid")]
     void RaidChatHandle(BasePlayer _player);
    [ChatCommand("pvp")]
     void PvpChatHandle(BasePlayer _player, string cmd, string[] args);
    [ConsoleCommand("AntiBandit.Purge")]
     void PurgeToggle(ConsoleSystem.Arg arg);
     bool CheckPlayerCooldown(BasePlayer _player);
     bool CheckServerPvPCooldown();
     bool CheckServerRaidCooldown();
     bool GetNearbyTargetWall(Vector3 hashPos, BasePlayer _player);
     class PVPData
    {
        public List<ulong> playerList;
        public double PvPInitCooldown;
        public double PvPEndTimer;
    }

     class RaidDetails
    {
        public double RaidInitCooldown;
        public double RaidEndTimer;
    }

     float CalculateDistance(Vector3 playerPos, Vector3 zonePos);
     void LoadPermissions();
    private static int wallCol;
    private static int playerCol;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
     void DestroyTimers();
     bool IsAllowed(BasePlayer player, string perm);
     double GetTimeStamp();
    static void NullifyDamage(HitInfo hitinfo);
    public bool CheckEntityInZone(BaseCombatEntity _entity, Vector3 _zoneKey, int _maxDistance);
    public bool CheckPlayerInZone(BasePlayer _player, Vector3 _zoneKey, int _maxDistance);
    public bool CheckPlayerRegistered(BasePlayer _player, Vector3 _zoneKey);
     bool CheckBuildingOwner(BasePlayer _player, BaseCombatEntity _entity);
     bool CheckNearPlayerNotFriend(BasePlayer _player, Vector3 _hashPos);
     void RegisterNearPlayers(Vector3 _hashPos);
     bool ZoneCooldownCheck(string _zoneType, Vector3 _zonekey);
     bool ZoneTimerCheck(string _zoneType, Vector3 _zonekey);
     ulong FindOwner(BaseEntity entity);
     void CreatePlayerCooldown(BasePlayer _player);
     bool ReceivingSnapshotcheck(BasePlayer _player);
     void DebugMessage(int _minDebuglvl, string _msg);
     void BroadcastMessageHandler(string message, object[] args);
     void ChatMessageHandler(BasePlayer player, string message, object[] args);
    private string ChatPrefixColor;
    private string ChatPrefix;
    private string ChatMessageColor;
    private bool purgeMode;
    private float RaidZone1;
    private int DebugLevel;
    private int PVPZone1;
    private float PVPZone2;
    private int PVPZone3;
    private int PvPDelay;
    private int PvPTimeLimit;
    private int RaidZone2;
    private int RaidDelay;
    private int RaidTimeLimit;
    private double playerCooldown;
    private double serverPvPCooldown;
    private double serverRaidCooldown;
    private double serverPvPCooldownTimeStamp;
    private double serverRaidCooldownTimeStamp;
    protected override void LoadDefaultConfig();
     void LoadVariables();
     object GetConfig(string menu, string dataValue, object defaultValue);
    protected override void LoadDefaultMessages();
}

 class PVPData
{
    public List<ulong> playerList;
    public double PvPInitCooldown;
    public double PvPEndTimer;
}

 class RaidDetails
{
    public double RaidInitCooldown;
    public double RaidEndTimer;
}


```

---

## AntiBarricadeStacking by k1lly0u - Stops monument barricades from stacking on top of each other

```csharp
using System.Linq;

Oxide.Plugins
[Info("AntiBarricadeStacking", "k1lly0u", "0.1.0", ResourceId = 0)]
 class AntiBarricadeStacking : RustPlugin
{
     bool initialized;
     void OnServerInitialized();
     void OnEntitySpawned(BaseNetworkable entity);
}


```

---

## AntiChatFlood by  - Prevents chat flood and warns players

```csharp
using System.Collections.Generic;
using System;
using UnityEngine;
using Oxide.Core.Plugins;
using System.Reflection;
using Oxide.Core;
using System.Data;
using Rust;

Oxide.Plugins
[Info("AntiChatFlood", "DylanSMR", "1.0.2", ResourceId = 1759)]
[Description("Data test stuff.")]
 class AntiChatFlood : RustPlugin
{
    [PluginReference]
     Plugin BetterChat;
    private bool Changed;
    static bool WarningEnabled;
    static int WaitTillMsg;
    static int MaxWarnings;
    static bool AdminBypass;
    public int AuthToBypass;
    static bool DisableBetterChat;
     void OnServerInitialized();
     void LoadDefaultConfig();
    private void LoadVariables();
    private void LoadConfigVariables();
     void Loaded();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
     object GetConfig(string menu, string datavalue, object defaultValue);
     List<ulong> playerWait;
    private Dictionary<ulong, PlayerWarnings> pWarn;
     class PlayerWarnings
    {
        public string Name;
        public float CurrentWarnings;
    }

     void Unload();
     void Init();
    private string GetMessage(string name, string sid);
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void SetVars(BasePlayer player);
     void KickPlayer(BasePlayer player);
    private object FindPlayer(string arg);
     void WipeAllStats();
     object OnPlayerChat(ConsoleSystem.Arg arg);
    [ChatCommand("chelp")]
     void help(BasePlayer player);
    [ChatCommand("cwipeall")]
     void AWipeAll(BasePlayer player);
    [ChatCommand("cwipe")]
     void playerwipe(BasePlayer player, string command, string[] args);
    [ChatCommand("cwarning")]
     void CheckWarnings(BasePlayer player);
}

 class PlayerWarnings
{
    public string Name;
    public float CurrentWarnings;
}


```

---

## AntiCodeRaid by kwamaking - Prevents players from code raiding if they're not on your team, TC authed, or the owner.

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Anti Code Raid", "kwamaking", "1.1.1")]
[Description("Prevents players from code raiding if they're not on your team, TC authed, or the owner.")]
 class AntiCodeRaid : RustPlugin
{
    private AntiCodeRaidConfiguration pluginConfiguration { get; set; }
    private const string UsePermission;
    private const string DefaultPrefixColor;
    private const ulong DefaultChatIconId;
    protected override void LoadDefaultConfig();
    private void Init();
    protected override void SaveConfig();
    private object CanUnlock(BasePlayer player, BaseLock baseLock);
    private bool IsPlayerOwner(BasePlayer player, BaseLock baseLock);
    private bool IsPlayerOnTeam(BasePlayer player, BaseLock baseLock);
    private bool IsPlayerTCAuthed(BasePlayer player);
    protected override void LoadDefaultMessages();
    private void SendMessage(BasePlayer player, string messageKey, object[] args);
    private class AntiCodeRaidConfiguration
    {
        [JsonProperty("allowTeamMembers")]
        public bool allowTeamMembers { get; set; }
        [JsonProperty("allowTCAuthed")]
        public bool allowTCAuthed { get; set; }
        [JsonProperty("pluginPrefixColor")]
        public string pluginPrefixColor { get; set; }
        [JsonProperty("chatIconId")]
        public ulong chatIconId { get; set; }
    }

}

private class AntiCodeRaidConfiguration
{
    [JsonProperty("allowTeamMembers")]
    public bool allowTeamMembers { get; set; }
    [JsonProperty("allowTCAuthed")]
    public bool allowTCAuthed { get; set; }
    [JsonProperty("pluginPrefixColor")]
    public string pluginPrefixColor { get; set; }
    [JsonProperty("chatIconId")]
    public ulong chatIconId { get; set; }
}


```

---

## AntiDrain by  - Stop turret draining today!

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;

Oxide.Plugins
[Info("Anti Drain", "Orange", "1.1.1")]
[Description("Plugin allows to prevent turret draining")]
public class AntiDrain : RustPlugin
{
    private List<uint> checking;
    private List<uint> blocked;
    private object CanBeTargeted(BasePlayer player, StorageContainer turret);
    private bool CanTarget(StorageContainer turret, BasePlayer player);
    private void Check(StorageContainer turret);
    private void Block(uint id);
    private int GetAmmoCount(StorageContainer container);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. Time after shooting when will be checked ammo count")]
        public int checkTime;
        [JsonProperty(PropertyName = "2. Ammo count difference to block the turret")]
        public int ammoDifference;
        [JsonProperty(PropertyName = "3. Time when turret will be blocked to shoot after drain-catch")]
        public int blockTime;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. Time after shooting when will be checked ammo count")]
    public int checkTime;
    [JsonProperty(PropertyName = "2. Ammo count difference to block the turret")]
    public int ammoDifference;
    [JsonProperty(PropertyName = "3. Time when turret will be blocked to shoot after drain-catch")]
    public int blockTime;
}


```

---

## AntiEntity by birthdates - Deny certain entities from spawning

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Anti Entity", "birthdates", "1.0.1")]
[Description("Deny certain entities from spawning")]
public class AntiEntity : RustPlugin
{
    private void Init();
     void OnServerInitialized();
     void CleanupNonPrefabs();
     bool IsPrefab(string pref);
     void CleanupExisting();
     void OnEntitySpawned(BaseNetworkable x);
    [ConsoleCommand("antientity")]
     void ConsoleCMD(ConsoleSystem.Arg arg);
    public ConfigFile _config;
    protected override void LoadDefaultMessages();
    public class ConfigFile
    {
        [JsonProperty("Entities that are denied from spawning (cant spawn, long prefab names accepted)")]
        public List<string> noSpawn;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Entities that are denied from spawning (cant spawn, long prefab names accepted)")]
    public List<string> noSpawn;
    public static ConfigFile DefaultConfig();
}


```

---

## AntiExplosiveUpgrade by birthdates - Deny upgrading a building block when an explosive is stuck to it

```csharp
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Anti Explosive Upgrade", "birthdates", "1.0.0")]
[Description("Deny upgrading a building block when an explosive is stuck to it")]
public class AntiExplosiveUpgrade : RustPlugin
{
    private object OnStructureUpgrade(Component entity, BasePlayer player, BuildingGrade.Enum grade);
    protected override void LoadDefaultMessages();
}


```

---

## AntiHelibombing by ZEODE - Prevent malicious collision damage to players helicopters within safe zones.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Anti Helibombing", "ZEODE", "1.1.11")]
[Description("Prevent malicious collision damage to players helicopters within safe zones.")]
public class AntiHelibombing : CovalencePlugin
{
    private static System.Random random;
    private const string permAdmin;
    private const string permBlacklist;
    private const string permKickProtect;
    private Dictionary<string, Cooldown> CooldownDelay;
    private Dictionary<ulong, SpawnProtection> SpawnDelay;
    private class Cooldown
    {
        public Timer CooldownTimer;
    }

    private class SpawnProtection
    {
        public Timer SpawnTimer;
    }

    private ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Options")]
        public Options options;
        public class Options
        {
            [JsonProperty(PropertyName = "Block ALL heli damage in Safe Zones (overrides spawn protection)")]
            public bool blockAllDamage;
            [JsonProperty(PropertyName = "Use collision cooldown timer (prevent damage outside Safe Zone due to collision inside)")]
            public bool useCooldown;
            [JsonProperty(PropertyName = "Collision cooldown time (seconds)")]
            public int cooldownTime;
            [JsonProperty(PropertyName = "Use heli spawn protection cooldown timer")]
            public bool useSpawnProtection;
            [JsonProperty(PropertyName = "Spawn protection cooldown time (seconds)")]
            public int spawnCooldownTime;
            [JsonProperty(PropertyName = "Kick players who break the Safe Zone collision threshold")]
            public bool useKick;
            [JsonProperty(PropertyName = "Safe Zone collision threshold (example: 30)")]
            public int collisionLimit;
            [JsonProperty(PropertyName = "Block crush damage to players from Scrap Heli in safe zones")]
            public bool noCrush;
            [JsonProperty(PropertyName = "Clear player collision data on wipe")]
            public bool clearDataOnWipe;
            [JsonProperty(PropertyName = "Use chat prefix")]
            public bool useChatPrefix;
            [JsonProperty(PropertyName = "Chat prefix")]
            public string chatPrefix;
        }

        public VersionNumber Version { get; set; }
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private StoredData storedData;
    private class StoredData
    {
        public Dictionary<string, HeliBombData> CollisionData;
    }

    private class HeliBombData
    {
        public string PlayerName;
        public int Collisions;
        public bool OnCooldown;
    }

    protected override void LoadDefaultMessages();
    public string Lang(string key, string id, object[] args);
    public void Message(IPlayer player, string key, object[] args);
    private void Init();
    private void OnNewSave();
    private void Unload();
    private void OnServerSave();
    private object OnEntitySpawned(Minicopter heli);
    private object OnEntityTakeDamage(BasePlayer player, HitInfo info);
    private object OnEntityTakeDamage(Minicopter heli, HitInfo info);
    private void SaveData();
    private void AddPlayerData(BasePlayer player, bool cooldown);
    private void RemoveCooldown(string steamId);
    private bool HasCollisionDelay(string steamId);
    private bool HasSpawnDelay(ulong heliId);
    private bool IsHeliCrushed(BasePlayer player);
    public void ProcessKick(IPlayer player);
    [Command("ahb", "antihelibombing")]
    private void CmdViewPlayerLog(IPlayer player, string command, string[] args);
    [Command("ahb.clear", "antihelibombing.clear")]
    private void CmdClearPlayerLog(IPlayer player, string command, string[] args);
}

private class Cooldown
{
    public Timer CooldownTimer;
}

private class SpawnProtection
{
    public Timer SpawnTimer;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Options")]
    public Options options;
    public class Options
    {
        [JsonProperty(PropertyName = "Block ALL heli damage in Safe Zones (overrides spawn protection)")]
        public bool blockAllDamage;
        [JsonProperty(PropertyName = "Use collision cooldown timer (prevent damage outside Safe Zone due to collision inside)")]
        public bool useCooldown;
        [JsonProperty(PropertyName = "Collision cooldown time (seconds)")]
        public int cooldownTime;
        [JsonProperty(PropertyName = "Use heli spawn protection cooldown timer")]
        public bool useSpawnProtection;
        [JsonProperty(PropertyName = "Spawn protection cooldown time (seconds)")]
        public int spawnCooldownTime;
        [JsonProperty(PropertyName = "Kick players who break the Safe Zone collision threshold")]
        public bool useKick;
        [JsonProperty(PropertyName = "Safe Zone collision threshold (example: 30)")]
        public int collisionLimit;
        [JsonProperty(PropertyName = "Block crush damage to players from Scrap Heli in safe zones")]
        public bool noCrush;
        [JsonProperty(PropertyName = "Clear player collision data on wipe")]
        public bool clearDataOnWipe;
        [JsonProperty(PropertyName = "Use chat prefix")]
        public bool useChatPrefix;
        [JsonProperty(PropertyName = "Chat prefix")]
        public string chatPrefix;
    }

    public VersionNumber Version { get; set; }
}

public class Options
{
    [JsonProperty(PropertyName = "Block ALL heli damage in Safe Zones (overrides spawn protection)")]
    public bool blockAllDamage;
    [JsonProperty(PropertyName = "Use collision cooldown timer (prevent damage outside Safe Zone due to collision inside)")]
    public bool useCooldown;
    [JsonProperty(PropertyName = "Collision cooldown time (seconds)")]
    public int cooldownTime;
    [JsonProperty(PropertyName = "Use heli spawn protection cooldown timer")]
    public bool useSpawnProtection;
    [JsonProperty(PropertyName = "Spawn protection cooldown time (seconds)")]
    public int spawnCooldownTime;
    [JsonProperty(PropertyName = "Kick players who break the Safe Zone collision threshold")]
    public bool useKick;
    [JsonProperty(PropertyName = "Safe Zone collision threshold (example: 30)")]
    public int collisionLimit;
    [JsonProperty(PropertyName = "Block crush damage to players from Scrap Heli in safe zones")]
    public bool noCrush;
    [JsonProperty(PropertyName = "Clear player collision data on wipe")]
    public bool clearDataOnWipe;
    [JsonProperty(PropertyName = "Use chat prefix")]
    public bool useChatPrefix;
    [JsonProperty(PropertyName = "Chat prefix")]
    public string chatPrefix;
}

private class StoredData
{
    public Dictionary<string, HeliBombData> CollisionData;
}

private class HeliBombData
{
    public string PlayerName;
    public int Collisions;
    public bool OnCooldown;
}


```

---

## AntiHeliHoarding by Thisha - Minicopter hoarding punishment by decay multiplication

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Facepunch;

Oxide.Plugins
[Info("Anti-Heli Hoarding", "Thisha", "0.2.0")]
public class AntiHeliHoarding : RustPlugin
{
    [ChatCommand("helidecay")]
     void ShowDecay(BasePlayer player);
    private void Init();
     void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
    private ConfigData config;
     class ConfigData
    {
        public int DetectionRadius;
        public int MinimumQuantity;
    }

    private object ConfigValue(string value);
    protected override void LoadDefaultConfig();
    private void LoadConfigData();
    private MiniCopter GetLookingAtMiniCopter(BasePlayer player);
    private int MinicoptersInRange(Vector3 pos);
}

 class ConfigData
{
    public int DetectionRadius;
    public int MinimumQuantity;
}


```

---

## AntiItems by nivex - Removes the need for certain items when crafting and repairing

```csharp
using Facepunch;
using Newtonsoft.Json;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("AntiItems", "Author redBDGR, Maintainer nivex", "1.0.15")]
[Description("Remove the need for certain items in crafting and repairing")]
 class AntiItems : RustPlugin
{
    private Dictionary<string, int> componentList;
    private string permissionName;
    private Configuration config;
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnPlayerRespawned(BasePlayer player);
    private void Init();
    private void OnGroupPermissionRevoked(string group, string perm);
    private void OnUserPermissionRevoked(string userid, string perm);
    private void OnEntityDeath(BasePlayer player, HitInfo info);
    private void OnItemCraftCancelled(ItemCraftTask task);
    private void RefreshItems(BasePlayer player);
    private void DoItems(BasePlayer player);
    private void RemoveItems(BasePlayer player);
    private void VerifyShortnames();
    private bool IsInvalid(BasePlayer player);
    public class Settings
    {
        [JsonProperty("Components", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, int> componentList;
        [JsonProperty("Use Active Item Refreshing")]
        public bool useActiveRefreshing;
        [JsonProperty("Refresh Time")]
        public float refreshTime;
        [JsonProperty("Ignore Stack Limit")]
        public bool ignoreStackLimit;
    }

    private class Configuration
    {
        [JsonProperty("Settings")]
        public Settings settings;
        private string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
}

public class Settings
{
    [JsonProperty("Components", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, int> componentList;
    [JsonProperty("Use Active Item Refreshing")]
    public bool useActiveRefreshing;
    [JsonProperty("Refresh Time")]
    public float refreshTime;
    [JsonProperty("Ignore Stack Limit")]
    public bool ignoreStackLimit;
}

private class Configuration
{
    [JsonProperty("Settings")]
    public Settings settings;
    private string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## AntiLadderandTwig by Kaucsenta - The goal of this plugin to block the unwanted ladders and extra twig frames on player bases.

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Anti Ladder and Twig", "kaucsenta", "1.3.1")]
[Description("Protect bases from ladder and twig frames")]
 class AntiLadderandTwig : RustPlugin
{
    public PluginConfig config;
    [PluginReference]
     Plugin AbandonedBases;
     object CanBuild(Planner planner, Construction prefab, Construction.Target target);
     void SubscribeHooks(bool flag);
    private void Init();
    private void LoadVariables();
    private void LoadConfigVariables();
    public class PluginConfig
    {
        [JsonProperty(PropertyName = "Disable plugin feature true/false")]
        public bool disableplugin;
    }

    protected override void LoadDefaultConfig();
     void SaveConfig(PluginConfig config, string filename);
    protected override void LoadDefaultMessages();
}

public class PluginConfig
{
    [JsonProperty(PropertyName = "Disable plugin feature true/false")]
    public bool disableplugin;
}


```

---

## AntiNoobRaid by MasterSplinter - Prevents damage to entities/buildings placed by new player's for a certain amount of time.

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using Rust;
using UnityEngine;
using Newtonsoft.Json;
using System.Linq;

Oxide.Plugins
[Info("AntiNoobRaid", "MasterSplinter", "2.1.1", ResourceId = 2697)]
 class AntiNoobRaid : RustPlugin
{
    [PluginReference]
    private Plugin PlaytimeTracker;
    private Plugin WipeProtection;
    private Plugin Clans;
    private Plugin StartProtection;
    private bool beta;
    private string betaversion;
    private List<BasePlayer> cooldown;
    private List<BasePlayer> MessageCooldown;
    private List<ulong> LeaderLower;
    private List<BuildingBlock> _blocks;
    private Timer CheckingPT;
    private Timer CheckingC;
    private Dictionary<string, string> raidtools;
    private Dictionary<string, string> RaidToolsCheck;
    private Dictionary<string, string> NotSupportedRaidTools;
    private int layers;
    private readonly string AdminPerm;
    private readonly string NoobPerm;
    private class ConfigFile
    {
        [JsonProperty(PropertyName = "Main Settings")]
        public MainSettings Main;
        [JsonProperty(PropertyName = "Other Settings")]
        public OtherSettings Other;
        [JsonProperty(PropertyName = "Team & Clan Settings")]
        public RelationshipSettings Relationship;
        [JsonProperty(PropertyName = "Refund Settings")]
        public RefundSettings Refund;
        [JsonProperty(PropertyName = "Manage Messages")]
        public ManageMessages Messages;
        [JsonProperty(PropertyName = "Entity Settings")]
        public EntitySettings Entity;
        [JsonProperty(PropertyName = "Weapon Settings")]
        public WeaponSettings RaidTools;
        [JsonProperty(PropertyName = "Advance Settings")]
        public AdvanceSettings Advance;
        public static readonly ConfigFile DefaultConfigFile;
    }

    public class MainSettings
    {
        [JsonProperty("Time (seconds) after which noob will lose protection (in-game time)")]
        public int ProtectionTime;
        [JsonProperty("Days of inactivity after which player will be raidable")]
        public double InactivityRemove;
        [JsonProperty("Remove noob status of a raider on raid attempt")]
        public bool UnNoobNew;
        [JsonProperty("Remove noob status of a raider who is manually marked as a noob on raid attempt")]
        public bool UnNoobManual;
    }

    public class OtherSettings
    {
        [JsonProperty("Allow Patrol Helicopter to damage noob structures (This will allow players to raid noobs with Patrol Helicopter)")]
        public bool PatrolHeliDamage;
        [JsonProperty("Ignore twig when calculating base ownership (prevents exploiting)")]
        public bool IgnoreTwig;
        [JsonProperty("Check full ownership of the base instead of only one block")]
        public bool CheckFullOwnership;
        [JsonProperty("Kill fireballs when someone tries to raid protected player with fire (prevents lag)")]
        public bool KillFire;
        [JsonProperty("Clear Player Data on Map-Wipe")]
        public bool MapWipe;
    }

    public class RelationshipSettings
    {
        [JsonProperty("Enable 'Clan' Support (Allow clan members to destroy each others entities & Remove protection from clan members when a member tries to raid)")]
        public bool CheckClan;
        [JsonProperty("Enable 'Team' Support (Allow team members to destroy each others entities & Remove protection from team members when a member tries to raid)")]
        public bool CheckTeam;
        [JsonProperty("Enable 'Team' playtime sync (Sync's all player's within a team to the highest playtime)")]
        public bool SyncTeamPlaytime;
    }

    public class RefundSettings
    {
        [JsonProperty("Refund explosives")]
        public bool RefundItem;
        [JsonProperty("Refunds before player starts losing explosives")]
        public int RefundTimes;
    }

    public class ManageMessages
    {
        [JsonProperty("Notify player on first connection with protection time")]
        public bool MessageOnFirstConnection;
        [JsonProperty("Use game tips to send most messages to players")]
        public bool UseGT;
        [JsonProperty("Show message for not being able to raid")]
        public bool ShowMessage;
        [JsonProperty("Show time until raidable")]
        public bool ShowTime;
        [JsonProperty("Show time until raidable only to owners")]
        public bool ShowOwnerTime;
        [JsonProperty("Show message on first Twig placement that Twig is not protected")]
        public bool ShowMessageTwig;
    }

    public class EntitySettings
    {
        [JsonProperty("List of entities that can be destroyed even if owner is noob")]
        public Dictionary<string, string> AllowedEntities;
        [JsonProperty("List of entities that can be destroyed without losing noob protection")]
        public Dictionary<string, string> AllowedEntitiesNoob;
        public static Dictionary<string, string> AllowedEntitiesDictionary;
        public static Dictionary<string, string> AllowedEntitiesNoobDictionary;
    }

    public class WeaponSettings
    {
        [JsonProperty("List of Weapons/Tools that won't trigger player to lose noob protection")]
        public Dictionary<string, string> AllowedRaidTools;
        public static Dictionary<string, string> AllowedRaidToolsDictionary;
    }

    public class AdvanceSettings
    {
        [JsonProperty("User data refresh interval (seconds)")]
        public int Frequency;
        [JsonProperty("Save interval (seconds)")]
        public int SaveFrequency;
        [JsonProperty("Save data on Server Save")]
        public bool OnServerSave;
        [JsonProperty("Show structure has no owner in console")]
        public bool ShowNoOwnerBase;
        [JsonProperty("Enable Logs (Logs can be found in /oxide/logs/antinoobraid)")]
        public bool EnableLogging;
    }

    private ConfigFile config;
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private class BuildingInfo
    {
        public static HashSet<BuildingInfo> buildCache;
        public BuildingInfo();
        public uint buildingID;
        public ulong OwnerID;
        public DateTime lastUpdate;
        public static BuildingInfo GetByBuildingID(uint bID);
        public double GetCacheAge();
    }

    private class ClanInfo
    {
        public static HashSet<ClanInfo> clanCache;
        public ClanInfo();
        public static ClanInfo FindClanByName(string clanName);
        public static ClanInfo GetClanOf(ulong playerID);
        public string clanName { get; set; }
        public List<ulong> members;
    }

    private class StoredData
    {
        public Dictionary<ulong, double> players;
        public Dictionary<ulong, int> AttackAttempts;
        public List<ulong> playersWithNoData;
        public List<ulong> FirstMessaged;
        public List<ulong> InTeam;
        public List<ulong> ShowTwigsNotProtected;
        public Dictionary<ulong, string> lastConnection;
        public List<ulong> IgnoredPlayers;
        public StoredData();
    }

    private class StoredDataItemList
    {
        public Dictionary<string, string> ItemList;
        public StoredDataItemList();
    }

    private void SaveData();
    private void SaveDataItemList();
     StoredData storedData;
     StoredDataItemList storedDataItemList;
    private void Init();
    private void RegisterCommands();
    private void OnServerInitialized(bool isStartup);
    private void Unload();
    private void OnServerSave();
    private void OnNewSave();
    private Dictionary<string, string> AdminCommands;
    private void AntiNoobCommand(IPlayer player, string command, string[] args);
    [ChatCommand("checknew")]
    private void CheckNewCmd(BasePlayer player, string command, string[] args);
    [ChatCommand("entdebug")]
    private void EntDebugCmd(BasePlayer player, string command, string[] args);
    private void CheckNameCmd(IPlayer p, string command, string[] args);
    private void RefundCmd(IPlayer p, string command, string[] args);
    private object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
    private void OnEntityBuilt(Planner plan, GameObject gameObject);
    private void OnFireBallDamage(FireBall fireball, BaseCombatEntity entity, HitInfo hitinfo);
    private void OnPlayerDisconnected(BasePlayer player, string reason);
    private void OnEntitySpawned(BuildingBlock block);
    private void OnEntityDeath(BuildingBlock block, HitInfo hitInfo);
    private void OnEntityKill(BuildingBlock block);
    private void OnUserConnected(IPlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnTeamCreated(BasePlayer player, RelationshipManager.PlayerTeam team);
    private void OnTeamAcceptInvite(RelationshipManager.PlayerTeam team, BasePlayer player);
    private void OnTeamKick(RelationshipManager.PlayerTeam team, ulong target);
    private void OnTeamLeave(RelationshipManager.PlayerTeam team, BasePlayer player);
    private void OnTeamDisbanded(RelationshipManager.PlayerTeam team);
     bool IgnorePlayer(object o);
     bool UnIgnorePlayer(object o);
    private bool AllowOwnerCheckTimeClans(BasePlayer attacker, ulong ID);
    private bool CheckClan(BaseCombatEntity entity, HitInfo hitinfo);
    private List<ulong> GetClanMembers(string clanName);
    private void RefreshClanCache();
    private void RemoveClanProtection(BasePlayer attacker, ulong playerID);
    private bool AllowOwnerCheckTimeTeams(BasePlayer attacker, ulong ID);
    private bool CheckTeam(BaseCombatEntity entity, HitInfo hitinfo);
    private void RemoveTeamProtection(BaseCombatEntity entity, HitInfo hitinfo);
    private void SyncTeam(RelationshipManager.PlayerTeam team, ulong ID);
    private void FailSafe();
     bool HasProtection(BasePlayer player);
    private void APICall(ulong ID);
    private void APICall_SecondAttempt(ulong ID);
    private void APICall_TeamSync(ulong ID);
    private void APICall_TeamSync_Compare(ulong ID, ulong member, double apitime, double apitime_member);
    private void APICall_TeamSync_Final(ulong ID);
    private void WriteTeamData(ulong ID, double apitime);
    private void Check();
    private void Check(ulong ID);
    private string CheckForRaidingTools(HitInfo hitinfo);
    private bool NullWeaponCheck(BaseCombatEntity entity, HitInfo hitinfo);
    private bool AllowOwnerCheckTime(BasePlayer attacker, ulong ID);
    private bool CheckForBuildingOrDeployable(BaseCombatEntity entity, HitInfo hitinfo);
    private bool CheckForHelicopterOrMLRS(BaseCombatEntity entity, HitInfo hitinfo);
    private string CheckLeft(int intsecs);
    private void CheckPlayersWithNoInfo();
    private ulong FullOwner(BaseEntity ent, BasePlayer p);
    private BaseEntity GetLookAtEntity(BasePlayer player, float maxDist);
    private void LastConnect(ulong ID);
    private void LogPlayer(BasePlayer attacker);
    private void MessagePlayer(BasePlayer attacker, ulong ID);
    private bool PlayerIsNew(ulong ID);
    private void RemoveCD(List<BasePlayer> List, BasePlayer player);
    private void RemoveProtection(BaseCombatEntity entity, HitInfo hitinfo);
    private void RemoveInactive();
    private void Refund(BasePlayer attacker, string name, BaseEntity ent);
    private void StartChecking();
}

private class ConfigFile
{
    [JsonProperty(PropertyName = "Main Settings")]
    public MainSettings Main;
    [JsonProperty(PropertyName = "Other Settings")]
    public OtherSettings Other;
    [JsonProperty(PropertyName = "Team & Clan Settings")]
    public RelationshipSettings Relationship;
    [JsonProperty(PropertyName = "Refund Settings")]
    public RefundSettings Refund;
    [JsonProperty(PropertyName = "Manage Messages")]
    public ManageMessages Messages;
    [JsonProperty(PropertyName = "Entity Settings")]
    public EntitySettings Entity;
    [JsonProperty(PropertyName = "Weapon Settings")]
    public WeaponSettings RaidTools;
    [JsonProperty(PropertyName = "Advance Settings")]
    public AdvanceSettings Advance;
    public static readonly ConfigFile DefaultConfigFile;
}

public class MainSettings
{
    [JsonProperty("Time (seconds) after which noob will lose protection (in-game time)")]
    public int ProtectionTime;
    [JsonProperty("Days of inactivity after which player will be raidable")]
    public double InactivityRemove;
    [JsonProperty("Remove noob status of a raider on raid attempt")]
    public bool UnNoobNew;
    [JsonProperty("Remove noob status of a raider who is manually marked as a noob on raid attempt")]
    public bool UnNoobManual;
}

public class OtherSettings
{
    [JsonProperty("Allow Patrol Helicopter to damage noob structures (This will allow players to raid noobs with Patrol Helicopter)")]
    public bool PatrolHeliDamage;
    [JsonProperty("Ignore twig when calculating base ownership (prevents exploiting)")]
    public bool IgnoreTwig;
    [JsonProperty("Check full ownership of the base instead of only one block")]
    public bool CheckFullOwnership;
    [JsonProperty("Kill fireballs when someone tries to raid protected player with fire (prevents lag)")]
    public bool KillFire;
    [JsonProperty("Clear Player Data on Map-Wipe")]
    public bool MapWipe;
}

public class RelationshipSettings
{
    [JsonProperty("Enable 'Clan' Support (Allow clan members to destroy each others entities & Remove protection from clan members when a member tries to raid)")]
    public bool CheckClan;
    [JsonProperty("Enable 'Team' Support (Allow team members to destroy each others entities & Remove protection from team members when a member tries to raid)")]
    public bool CheckTeam;
    [JsonProperty("Enable 'Team' playtime sync (Sync's all player's within a team to the highest playtime)")]
    public bool SyncTeamPlaytime;
}

public class RefundSettings
{
    [JsonProperty("Refund explosives")]
    public bool RefundItem;
    [JsonProperty("Refunds before player starts losing explosives")]
    public int RefundTimes;
}

public class ManageMessages
{
    [JsonProperty("Notify player on first connection with protection time")]
    public bool MessageOnFirstConnection;
    [JsonProperty("Use game tips to send most messages to players")]
    public bool UseGT;
    [JsonProperty("Show message for not being able to raid")]
    public bool ShowMessage;
    [JsonProperty("Show time until raidable")]
    public bool ShowTime;
    [JsonProperty("Show time until raidable only to owners")]
    public bool ShowOwnerTime;
    [JsonProperty("Show message on first Twig placement that Twig is not protected")]
    public bool ShowMessageTwig;
}

public class EntitySettings
{
    [JsonProperty("List of entities that can be destroyed even if owner is noob")]
    public Dictionary<string, string> AllowedEntities;
    [JsonProperty("List of entities that can be destroyed without losing noob protection")]
    public Dictionary<string, string> AllowedEntitiesNoob;
    public static Dictionary<string, string> AllowedEntitiesDictionary;
    public static Dictionary<string, string> AllowedEntitiesNoobDictionary;
}

public class WeaponSettings
{
    [JsonProperty("List of Weapons/Tools that won't trigger player to lose noob protection")]
    public Dictionary<string, string> AllowedRaidTools;
    public static Dictionary<string, string> AllowedRaidToolsDictionary;
}

public class AdvanceSettings
{
    [JsonProperty("User data refresh interval (seconds)")]
    public int Frequency;
    [JsonProperty("Save interval (seconds)")]
    public int SaveFrequency;
    [JsonProperty("Save data on Server Save")]
    public bool OnServerSave;
    [JsonProperty("Show structure has no owner in console")]
    public bool ShowNoOwnerBase;
    [JsonProperty("Enable Logs (Logs can be found in /oxide/logs/antinoobraid)")]
    public bool EnableLogging;
}

private class BuildingInfo
{
    public static HashSet<BuildingInfo> buildCache;
    public BuildingInfo();
    public uint buildingID;
    public ulong OwnerID;
    public DateTime lastUpdate;
    public static BuildingInfo GetByBuildingID(uint bID);
    public double GetCacheAge();
}

private class ClanInfo
{
    public static HashSet<ClanInfo> clanCache;
    public ClanInfo();
    public static ClanInfo FindClanByName(string clanName);
    public static ClanInfo GetClanOf(ulong playerID);
    public string clanName { get; set; }
    public List<ulong> members;
}

private class StoredData
{
    public Dictionary<ulong, double> players;
    public Dictionary<ulong, int> AttackAttempts;
    public List<ulong> playersWithNoData;
    public List<ulong> FirstMessaged;
    public List<ulong> InTeam;
    public List<ulong> ShowTwigsNotProtected;
    public Dictionary<ulong, string> lastConnection;
    public List<ulong> IgnoredPlayers;
    public StoredData();
}

private class StoredDataItemList
{
    public Dictionary<string, string> ItemList;
    public StoredDataItemList();
}


```

---

## AntiNPC by birthdates - Disables the spawning of NPCs

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Anti NPC", "birthdates", "1.0.4")]
[Description("Disables the spawning of NPCs")]
public class AntiNPC : RustPlugin
{
    private void Init();
    private void OnServerInitialized();
    private void Cleanup();
    private void OnEntitySpawned(BaseNetworkable entity);
    private ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Whitelisted NPCS (Prefab)")]
        public List<string> Whitelist;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Whitelisted NPCS (Prefab)")]
    public List<string> Whitelist;
    public static ConfigFile DefaultConfig();
}


```

---

## AntiOfflineRaid by Calytic - Cancels or reduces base damage when offline

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Rust;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Anti Offline Raid", "Calytic/Shady14u", "1.0.2")]
[Description("Prevents/reduces offline raiding")]
public partial class AntiOfflineRaid : RustPlugin
{
    private static Configuration _config;
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    public class Configuration
    {
        [JsonProperty(PropertyName = "AbsoluteTimeScale")]
        public Dictionary<string, object> AbsoluteTimeScale;
        [JsonProperty(PropertyName = "DamageScale")]
        public Dictionary<string, object> DamageScale;
        public List<string> Activities { get; set; }
        public int AfkMinutes { get; set; }
        public bool ClanFirstOffline { get; set; }
        public bool ClanShare { get; set; }
        public int CooldownMinutes { get; set; }
        public float InterimDamage { get; set; }
        public int MinMembers { get; set; }
        public int MinutesSinceLastAttackToProtect { get; set; }
        public bool PlaySound { get; set; }
        public List<string> Prefabs { get; set; }
        public bool ProtectBaseWhenAway { get; set; }
        public int ServerTimeOffset { get; set; }
        public bool ShowMessage { get; set; }
        public string SoundFile { get; set; }
        public List<int> DamageScaleKeys { get; set; }
        public static Configuration DefaultConfig();
    }

    private static List<string> GetDefaultActivities();
    private static class PluginMessages
    {
        public const string ProtectionMessage;
        public const string DeniedPermission;
    }

    protected override void LoadDefaultMessages();
    private string GetMsg(string key, object userId);
     void SendMessage(HitInfo hitInfo, int amt);
    private static class PluginPermissions
    {
        public const string Protect;
        public const string Check;
    }

    private void LoadPermissions();
     bool HasPerm(BasePlayer p, string pe);
     bool HasPerm(string userid, string pe);
    private StoredData _storedData;
    public class StoredData
    {
        public Dictionary<ulong, LastOnline> LastOnlines { get; set; }
        public Dictionary<string, List<string>> MembersCache { get; set; }
    }

    private void LoadData();
    private void SaveData();
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     void OnLootItem(BasePlayer player, Item item);
     void OnLootPlayer(BasePlayer player, BasePlayer target);
     void OnPlayerChat(ConsoleSystem.Arg args);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerRespawn(BasePlayer player);
     void OnPlayerSleepEnded(BasePlayer player);
     void OnServerInitialized();
    private void OnServerSave();
     void OnClanCreate(string tag);
     void OnClanUpdate(string tag);
     void OnClanDestroy(string tag);
    protected void ToggleHook(List<string> wakeupTriggers, string trigger, string hook);
    protected void ToggleHooks(List<string> wakeupTriggers);
    private void Unload();
    [ConsoleCommand("ao")]
     void CheckOfflineStatus(ConsoleSystem.Arg arg);
    [ChatCommand("ao")]
     void CheckOfflineStatus(BasePlayer player, string command, string[] args);
    [ChatCommand("ao.fill.onlineTimes")]
     void CmdFillOnlines(BasePlayer player, string command, string[] args);
     void SendHelpText(BasePlayer player);
    public class LastOnline
    {
        [JsonIgnore]
        public float AfkMinutes;
        public long LastOnlineLong;
        [JsonIgnore]
        public Vector3 LastPosition;
        public ulong UserId;
        [JsonConstructor]
        public LastOnline(ulong userid, long lastOnlineLong);
        public LastOnline(ulong userId, DateTime lastOnline);
        [JsonIgnore]
        public BasePlayer AarPlayer { get; set; }
        [JsonIgnore]
        public double Days { get; set; }
        [JsonIgnore]
        public double Hours { get; set; }
        [JsonIgnore]
        public DateTime LastOnlineTime { get; set; }
        [JsonIgnore]
        public double Minutes { get; set; }
        public bool HasMoved(Vector3 position);
        public bool IsAfk();
        public bool IsConnected();
        public bool IsOffline();
    }

     class ScaleCacheItem
    {
        public DateTime Expires;
        public float Scale;
        public ScaleCacheItem(DateTime expires, float scale);
    }

    [PluginReference]
     Plugin Clans;
    private const string JsonMessage;
    private const float TickRate;
    private readonly Dictionary<ulong, ScaleCacheItem> _cachedScales;
    private Timer _lastOnlineTimer;
    public List<string> CacheClan(string tag);
     float CacheDamageScale(ulong targetId, float scale, ScaleCacheItem kvp);
    protected IPlayer FindPlayerByPartialName(string nameOrIdOrIp);
    public List<string> GetClanMembers(string tag);
    public int GetClanMembersOnline(ulong targetId);
    public ulong GetClanOffline(string tag);
    private DateTime GetPlayersLastOnline(ulong memberId);
     void HideMessage(BasePlayer player);
    private bool IsAuthorizedOnline(BaseEntity entity);
    public bool IsBlocked(BaseCombatEntity entity);
    private bool IsClanInRange(ulong targetId);
    public bool IsClanOffline(ulong targetId);
    public bool IsOffline(ulong playerId);
    private static bool IsPlayerInRange(ulong targetId);
    public object MitigateDamage(HitInfo hitInfo, float scale);
     object OnStructureAttack(BaseEntity entity, HitInfo hitInfo);
    private ulong RecentActiveClanMember(ulong targetId);
    public float ScaleDamage(LastOnline lastOnline);
     float ScaleDamageCached(LastOnline lastOnline);
     string SendStatus(string playerSearch);
     void ShowMessage(BasePlayer player, int amount);
     void UpdateLastOnline(ulong playerId, bool hasMoved);
     void UpdateLastOnlineAll(bool afkCheck);
}

public class Configuration
{
    [JsonProperty(PropertyName = "AbsoluteTimeScale")]
    public Dictionary<string, object> AbsoluteTimeScale;
    [JsonProperty(PropertyName = "DamageScale")]
    public Dictionary<string, object> DamageScale;
    public List<string> Activities { get; set; }
    public int AfkMinutes { get; set; }
    public bool ClanFirstOffline { get; set; }
    public bool ClanShare { get; set; }
    public int CooldownMinutes { get; set; }
    public float InterimDamage { get; set; }
    public int MinMembers { get; set; }
    public int MinutesSinceLastAttackToProtect { get; set; }
    public bool PlaySound { get; set; }
    public List<string> Prefabs { get; set; }
    public bool ProtectBaseWhenAway { get; set; }
    public int ServerTimeOffset { get; set; }
    public bool ShowMessage { get; set; }
    public string SoundFile { get; set; }
    public List<int> DamageScaleKeys { get; set; }
    public static Configuration DefaultConfig();
}

private static class PluginMessages
{
    public const string ProtectionMessage;
    public const string DeniedPermission;
}

private static class PluginPermissions
{
    public const string Protect;
    public const string Check;
}

public class StoredData
{
    public Dictionary<ulong, LastOnline> LastOnlines { get; set; }
    public Dictionary<string, List<string>> MembersCache { get; set; }
}

public class LastOnline
{
    [JsonIgnore]
    public float AfkMinutes;
    public long LastOnlineLong;
    [JsonIgnore]
    public Vector3 LastPosition;
    public ulong UserId;
    [JsonConstructor]
    public LastOnline(ulong userid, long lastOnlineLong);
    public LastOnline(ulong userId, DateTime lastOnline);
    [JsonIgnore]
    public BasePlayer AarPlayer { get; set; }
    [JsonIgnore]
    public double Days { get; set; }
    [JsonIgnore]
    public double Hours { get; set; }
    [JsonIgnore]
    public DateTime LastOnlineTime { get; set; }
    [JsonIgnore]
    public double Minutes { get; set; }
    public bool HasMoved(Vector3 position);
    public bool IsAfk();
    public bool IsConnected();
    public bool IsOffline();
}

 class ScaleCacheItem
{
    public DateTime Expires;
    public float Scale;
    public ScaleCacheItem(DateTime expires, float scale);
}


```

---

## AntiRaidTower by Calytic - High jump instant death/No wounded teleport

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("AntiRaidTower", "Calyticc", "0.2.3")]
[Description("Building/deployable height limit, high jump instant death, no wounded teleport")]
 class AntiRaidTower : RustPlugin
{
     int FallKill;
     bool BuildingBlockHeight;
     bool DeployableBlockHeight;
     int BuildingMaxHeight;
     int DeployableMaxHeight;
     void OnServerInitialized();
    protected override void LoadDefaultConfig();
     void LoadMessages();
     void LoadData();
    protected void ReloadConfig();
    private T GetConfig(string name, T defaultValue);
     string GetMsg(string key, string userID);
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitinfo);
     object CanTeleport(BasePlayer player);
     object canTeleport(BasePlayer player);
     void OnEntityBuilt(Planner planner, GameObject gameObject);
}


```

---

## AntiSpam by MONaH - Filters spam and impersonation in player names and chat messages.

```csharp
using System;
using System.Collections.Generic;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Anti Spam", "MON@H", "2.1.3")]
[Description("Filters spam and impersonation in player names and chat messages.")]
public class AntiSpam : CovalencePlugin
{
    [PluginReference]
    private readonly Plugin BetterChat;
    private const string PermissionImmunity;
    private const string ColorAdmin;
    private const string ColorDeveloper;
    private const string ColorPlayer;
    private static readonly object _true;
    private readonly Regexes _regex;
    public class Regexes
    {
        public Regex Spam { get; set; }
        public Regex Impersonation { get; set; }
        public Regex Profanities { get; set; }
    }

    private void Init();
    private void OnServerInitialized();
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalSettings GlobalSettings;
        [JsonProperty(PropertyName = "Spam settings")]
        public SpamSettings SpamSettings;
        [JsonProperty(PropertyName = "Impersonation settings")]
        public ImpersonationSettings ImpersonationSettings;
    }

    private class GlobalSettings
    {
        [JsonProperty(PropertyName = "Enable logging")]
        public bool LoggingEnabled;
        [JsonProperty(PropertyName = "Filter chat messages")]
        public bool FilterChatMessages;
        [JsonProperty(PropertyName = "Filter player names")]
        public bool FilterPlayerNames;
        [JsonProperty(PropertyName = "Use UFilter plugin on player names")]
        public bool UFilterPlayerNames;
        [JsonProperty(PropertyName = "Replacement for empty name")]
        public string ReplacementEmptyName;
    }

    private class SpamSettings
    {
        [JsonProperty(PropertyName = "Use regex")]
        public bool UseRegex;
        [JsonProperty(PropertyName = "Regex list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RegexList;
        [JsonProperty(PropertyName = "Use blacklist")]
        public bool UseBlacklist;
        [JsonProperty(PropertyName = "Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Blacklist;
        [JsonProperty(PropertyName = "Replacement for spam")]
        public string Replacement;
    }

    private class ImpersonationSettings
    {
        [JsonProperty(PropertyName = "Use regex")]
        public bool UseRegex;
        [JsonProperty(PropertyName = "Regex list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> RegexList;
        [JsonProperty(PropertyName = "Use blacklist")]
        public bool UseBlacklist;
        [JsonProperty(PropertyName = "Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> Blacklist;
        [JsonProperty(PropertyName = "Replacement for impersonation")]
        public string Replacement;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnUserConnected(IPlayer player);
    private void OnUserNameUpdated(string id, string oldName, string newName);
    private object OnBetterChat(Dictionary<string, object> data);
    private object OnUserChat(IPlayer player, string message);
    private void OnPluginLoaded(Plugin plugin);
    private void OnProfanityAdded(string profanity);
    private void RemoveProfanity(string profanity);
    public void CacheRegex();
    public void CacheProfanities();
    public void HandleName(IPlayer player);
    public object HandleChatMessage(IPlayer player, string message, int channel);
    public string GetSpamFreeMessage(IPlayer player, string message);
    private string GetClearName(IPlayer player);
    private string GetSpamFreeText(string text);
    private string GetImpersonationFreeText(string text);
    private Regex GetRegexImpersonation();
    private Regex GetRegexProfanities();
    private Regex GetRegexSpam();
    public void UnsubscribeHooks();
    public void SubscribeHooks();
    public bool IsPluginLoaded(Plugin plugin);
    public void Log(string text);
    public void Broadcast(IPlayer sender, string text, int channel);
}

public class Regexes
{
    public Regex Spam { get; set; }
    public Regex Impersonation { get; set; }
    public Regex Profanities { get; set; }
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalSettings GlobalSettings;
    [JsonProperty(PropertyName = "Spam settings")]
    public SpamSettings SpamSettings;
    [JsonProperty(PropertyName = "Impersonation settings")]
    public ImpersonationSettings ImpersonationSettings;
}

private class GlobalSettings
{
    [JsonProperty(PropertyName = "Enable logging")]
    public bool LoggingEnabled;
    [JsonProperty(PropertyName = "Filter chat messages")]
    public bool FilterChatMessages;
    [JsonProperty(PropertyName = "Filter player names")]
    public bool FilterPlayerNames;
    [JsonProperty(PropertyName = "Use UFilter plugin on player names")]
    public bool UFilterPlayerNames;
    [JsonProperty(PropertyName = "Replacement for empty name")]
    public string ReplacementEmptyName;
}

private class SpamSettings
{
    [JsonProperty(PropertyName = "Use regex")]
    public bool UseRegex;
    [JsonProperty(PropertyName = "Regex list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RegexList;
    [JsonProperty(PropertyName = "Use blacklist")]
    public bool UseBlacklist;
    [JsonProperty(PropertyName = "Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Blacklist;
    [JsonProperty(PropertyName = "Replacement for spam")]
    public string Replacement;
}

private class ImpersonationSettings
{
    [JsonProperty(PropertyName = "Use regex")]
    public bool UseRegex;
    [JsonProperty(PropertyName = "Regex list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> RegexList;
    [JsonProperty(PropertyName = "Use blacklist")]
    public bool UseBlacklist;
    [JsonProperty(PropertyName = "Blacklist", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> Blacklist;
    [JsonProperty(PropertyName = "Replacement for impersonation")]
    public string Replacement;
}


```

---

## AntiTrashTalk by 0x89A - Prevents player's who have killed or been killed by another player from using voice or text chat.

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Anti Trash-Talk", "0x89A", "1.0.0")]
[Description("Mutes players after they kill or are killed by another player")]
 class AntiTrashTalk : RustPlugin
{
    private Configuration config;
    private Dictionary<ulong, float> lastKillOrDeathTime;
    private Dictionary<ulong, float> lastVoiceMessageTime;
    private const string exemptionPerm;
    private void Init();
    private void OnPlayerDeath(BasePlayer player, HitInfo info);
    private void OnPlayerWound(BasePlayer player, HitInfo info);
    private object OnPlayerChat(BasePlayer player, string message, ConVar.Chat.ChatChannel channel);
    private object OnPlayerVoice(BasePlayer player, Byte[] data);
    private object CanSpeak(BasePlayer player, bool isVoice);
    private void DoBlock(BasePlayer victim, HitInfo info);
    private void BlockChat(BasePlayer player);
    private class Configuration
    {
        [JsonProperty("Block duration (seconds)")]
        public float blockDuration;
        [JsonProperty("Block for dead/wounded player")]
        public bool blockForVictim;
        [JsonProperty("Block when self inflicted")]
        public bool blockSelfInflicted;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
}

private class Configuration
{
    [JsonProperty("Block duration (seconds)")]
    public float blockDuration;
    [JsonProperty("Block for dead/wounded player")]
    public bool blockForVictim;
    [JsonProperty("Block when self inflicted")]
    public bool blockSelfInflicted;
}


```

---

## AntiWounded by misticos - Players will skip the wounded state before dying

```csharp

Oxide.Plugins
[Info("Anti-Wounded", "Iv Misticos", "3.0.1")]
[Description("Players will skip the wounded state before dying.")]
 class AntiWounded : RustPlugin
{
    private const string PermUse;
    private void OnServerInitialized();
    private object CanBeWounded(BasePlayer player, HitInfo info);
}


```

---

## ArenaWallGenerator by nivex - An easy to use arena wall generator

```csharp
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text.RegularExpressions;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;

Oxide.Plugins
[Info("ArenaWallGenerator", "nivex", "1.0.6")]
[Description("An easy to use arena wall generator.")]
 class ArenaWallGenerator : RustPlugin
{
    private string hewwPrefab;
    private string heswPrefab;
    private string heiwPrefab;
    private const string permName;
    private const int wallMask;
    private StoredData storedData;
    private List<BaseCombatEntity> _walls;
    private bool newSave;
    public class StoredData
    {
        public int Seed;
        public readonly Dictionary<string, float> Arenas;
        public StoredData();
    }

    private void OnNewSave(string filename);
    private void OnServerSave();
    private void Unload();
    private void Init();
    private void OnServerInitialized(bool isStartup);
    private object OnEntityKill(IceFence fence);
    private object OnEntityKill(SimpleBuildingBlock block);
    private void OnEntityDeath(IceFence fence, HitInfo hitInfo);
    private void OnEntityDeath(SimpleBuildingBlock e, HitInfo hitInfo);
    private void OnEntityDeathHandler(BaseCombatEntity e);
    private void OnEntitySpawned(IceFence fence);
    private void OnEntitySpawned(SimpleBuildingBlock e);
    private void OnEntityTakeDamage(IceFence fence, HitInfo hitInfo);
    private void OnEntityTakeDamage(SimpleBuildingBlock ssb, HitInfo hitInfo);
    private void OnEntityTakeDamageHandler(BaseCombatEntity e, HitInfo hitInfo);
    public void SaveData();
    public void RecreateZoneWall(string prefab, Vector3 pos, Quaternion rot, ulong ownerId);
    [HookMethod("ArenaTerritory")]
    public bool ArenaTerritory(Vector3 position);
    public ulong GetHashId(string uid);
    public bool RemoveCustomZoneWalls(Vector3 center);
    public int RemoveZoneWalls(ulong hashId);
    public List<Vector3> GetCircumferencePositions(Vector3 center, float radius, float next, float y);
    public List<Vector3> GetSquarePerimeterPositions(Vector3 center, float radius, float y, float rotation);
    public List<Vector3> GetTrianglePerimeterPositions(Vector3 center, float radius, float y, float rotation);
    private static List<Vector3> AddSides(float gap, float length, float y, Vector3 corner1, Vector3 corner2);
    private static List<float> AddDirections(int n, Vector3 center, Vector3 reference);
    public List<Vector3> GetSquareCorners(Vector3 center, float radius, float y, float rotation);
    public List<Vector3> GetSquareSideCenters(Vector3 center, float radius, float y, float rotation);
    public List<Vector3> GetTriangleCorners(Vector3 center, float radius, float y, float rotation);
    public List<Vector3> GetTriangleSideCenters(Vector3 center, float radius, float y, float rotation);
    public bool CreateZoneWalls(Vector3 center, float radius, string prefab, IPlayer p, Shape shape);
    private bool API_CreateZoneWalls(Vector3 center, float radius, int shape);
    private bool API_RemoveZoneWalls(Vector3 center);
    private bool HasPermission(IPlayer player);
    private void CommandWalls(IPlayer p, string command, string[] args);
    private bool Changed;
    private int extraWallStacks;
    private bool useLeastAmount;
    private float maxCustomWallRadius;
    private bool useWoodenWalls;
    private bool useIceWalls;
    private string useShape;
    private string chatCommandName;
    private bool respawnWalls;
    private bool respawnOnWipe;
    private bool noDecay;
    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    private object GetConfig(string menu, string datavalue, object defaultValue);
    public string msg(string key, string id, object[] args);
    public string RemoveFormatting(string source);
}

public class StoredData
{
    public int Seed;
    public readonly Dictionary<string, float> Arenas;
    public StoredData();
}


```

---

## ArenaWalls by  - Simple plugin to remove walls, mainly for arena servers.

```csharp
using System;
using UnityEngine;
using System.Linq;
using Oxide.Core.Configuration;
using System.Collections.Generic;
using Oxide.Core;

Oxide.Plugins
[Info("ArenaWalls", "Kappasaurus", "1.0.1")]
public class ArenaWalls : RustPlugin
{
    readonly DynamicConfigFile dataFile;
     Dictionary<string, bool> playerPrefs;
    private float removeTime;
     void Init();
    private new void LoadConfig();
    [ChatCommand("walls")]
     void ToggleCommand(BasePlayer player, string command, string[] args);
     void OnEntityBuilt(Planner plan, GameObject go);
    private void GetConfig(T variable, string[] path);
    protected override void LoadDefaultConfig();
    private void LoadMessages();
     string Lang(string key, string id, object[] args);
}


```

---

## Arkan by Antidote - Analyses player shots and determines which of them are potentially were made by cheaters/macro users

```csharp
using System;
using System.Collections.Generic;
using ProtoBuf;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Globalization;
using Newtonsoft.Json;
using UnityEngine;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Arkan", "Antidote", "1.0.21")]
[Description("Player shot analysis tool for Admins")]
 class Arkan : RustPlugin
{
    [PluginReference]
    private Plugin DiscordMessages;
    private const string permName;
    private const string permNRDrawViolation;
    private const string permAIMDrawViolation;
    private const string permIRDrawViolation;
    private const string permNRReportChat;
    private const string permNRReportConsole;
    private const string permAIMReportChat;
    private const string permAIMReportConsole;
    private const string permIRReportChat;
    private const string permIRReportConsole;
    private const string permNRWhitelist;
    private const string permNRBlacklist;
    private const string permAIMWhitelist;
    private const string permAIMBlacklist;
    private const string permIRWhitelist;
    private const string permIRBlacklist;
    private Dictionary<ulong, PlayerFiredProjectlesData> PlayersFiredProjectlesData;
    private PlayersViolationsData PlayersViolations;
    private PlayersViolationsData tmpPlayersViolations;
    private string serverTimeStamp;
    private bool isAttackShow;
    private AdminConfig RconLog;
    private readonly int world_defaultLayer;
    private readonly int world_terrainLayer;
    private readonly int terrainLayer;
    private readonly string stringNullValueWarning;
    private Dictionary<BasePlayer, AdminConfig> AdminsConfig;
    private static Configuration _config;
    private class Configuration
    {
        public float NRProcessTimer;
        public float EPSILON;
        public float projectileTrajectoryForgiveness;
        public float hitDistanceForgiveness;
        public float minDistanceAIMCheck;
        public float inRockCheckDistance;
        public bool isDetectAIM;
        public bool isDetectNR;
        public bool isDetectIR;
        public bool debugMessages;
        public bool autoSave;
        public bool isCheckAIMOnTargetNPC;
        public float drawTime;
        public float NRViolationAngle;
        public float NRViolationScreenDistance;
        public float playerEyesPositionToProjectileInitialPositionDistanceForgiveness;
        [JsonProperty(PropertyName = "The maximum allowed value for the physics.steps parameter")]
        public float minPhysicsStepsAllowed;
        [JsonProperty(PropertyName = "Check players only on the blacklist")]
        public bool checkBlacklist;
        [JsonProperty(PropertyName = "Notify when a player has a high value of the physics.steps parameter")]
        public bool notifyPhysicsStepsWarning;
        [JsonProperty(PropertyName = "Enable Discord No Recoil Notifications")]
        public bool DiscordNRReportEnabled;
        [JsonProperty(PropertyName = "Enable Discord AIMBOT Notifications")]
        public bool DiscordAIMReportEnabled;
        [JsonProperty(PropertyName = "Enable Discord In Rock Notifications")]
        public bool DiscordIRReportEnabled;
        [JsonProperty(PropertyName = "Discord No Recoil Webhook URL")]
        public string DiscordNRWebhookURL;
        [JsonProperty(PropertyName = "Discord AIMBOT Webhook URL")]
        public string DiscordAIMWebhookURL;
        [JsonProperty(PropertyName = "Discord In Rock Webhook URL")]
        public string DiscordIRWebhookURL;
        [JsonProperty(PropertyName = "AIMBodyParts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> AIMBodyParts;
        [JsonProperty(PropertyName = "IRBodyParts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> IRBodyParts;
        [JsonProperty(PropertyName = "weaponsConfig")]
        public Dictionary<string, WeaponConfig> weaponsConfig;
    }

    private class WeaponConfig
    {
        public bool NRDetectEnabled;
        public bool AIMDetectEnabled;
        public float weaponMinTimeShotsInterval;
        public float weaponMaxTimeShotsInterval;
        public int NRMinShotsCountToCheck;
        public float NRViolationProbability;
    }

    private class ViolationsLog
    {
        public ulong steamID;
        public int NoRecoilViolation;
        public int AIMViolation;
        public int InRockViolation;
    }

    private class AdminConfig
    {
        public ViolationsLog violationsLog;
    }

    private class PlayersViolationsData
    {
        public int seed;
        public int mapSize;
        public string serverTimeStamp;
        public DateTime lastSaveTime;
        public DateTime lastChangeTime;
        public Dictionary<ulong, PlayerViolationsData> Players;
    }

    private class PlayerFiredProjectlesData
    {
        public ulong PlayerID;
        public string PlayerName;
        public float lastFiredTime;
        public float physicsSteps;
        public SortedDictionary<int, FiredProjectile> firedProjectiles;
        public SortedDictionary<ulong, MeleeThrown> melees;
        public bool isChecked;
    }

    private class PlayerViolationsData
    {
        public ulong PlayerID;
        public string PlayerName;
        public SortedDictionary<string, NoRecoilViolationData> noRecoilViolations;
        public SortedDictionary<string, AIMViolationData> AIMViolations;
        public SortedDictionary<string, InRockViolationsData> inRockViolations;
    }

    private class HitData
    {
        public ProjectileRicochet hitData;
        public Vector3 startProjectilePosition;
        public Vector3 startProjectileVelocity;
        public Vector3 hitPositionWorld;
        public Vector3 hitPointStart;
        public Vector3 hitPointEnd;
        public bool isHitPointNearProjectileTrajectoryLastSegmentEndPoint;
        public bool isHitPointOnProjectileTrajectory;
        public bool isProjectileStartPointAtEndReverseProjectileTrajectory;
        public bool isHitPointNearProjectilePlane;
        public bool isLastSegmentOnProjectileTrajectoryPlane;
        public float distanceFromHitPointToProjectilePlane;
        public int side;
        public Vector3 pointProjectedOnLastSegmentLine;
        public float travelDistance;
        public float delta;
        public Vector3 lastSegmentPointStart;
        public Vector3 lastSegmentPointEnd;
        public Vector3 reverseLastSegmentPointStart;
        public Vector3 reverseLastSegmentPointEnd;
    }

    private class AIMViolationData
    {
        public int projectileID;
        public int violationID;
        public DateTime firedTime;
        public Vector3 startProjectilePosition;
        public Vector3 startProjectileVelocity;
        public string hitInfoInitiatorPlayerName;
        public string hitInfoInitiatorPlayerUserID;
        public string hitInfoHitEntityPlayerName;
        public string hitInfoHitEntityPlayerUserID;
        public string hitInfoBoneName;
        public Vector3 hitInfoHitPositionWorld;
        public float hitInfoProjectileDistance;
        public Vector3 hitInfoPointStart;
        public Vector3 hitInfoPointEnd;
        public float hitInfoProjectilePrefabGravityModifier;
        public float hitInfoProjectilePrefabDrag;
        public string weaponShortName;
        public string ammoShortName;
        public string bodyPart;
        public float damage;
        public bool isEqualFiredProjectileData;
        public bool isPlayerPositionToProjectileStartPositionDistanceViolation;
        public float distanceDifferenceViolation;
        public float calculatedTravelDistance;
        public bool isAttackerMount;
        public bool isTargetMount;
        public string attackerMountParentName;
        public string targetMountParentName;
        public float firedProjectileFiredTime;
        public float firedProjectileTravelTime;
        public Vector3 firedProjectilePosition;
        public Vector3 firedProjectileVelocity;
        public Vector3 firedProjectileInitialPosition;
        public Vector3 firedProjectileInitialVelocity;
        public Vector3 playerEyesLookAt;
        public Vector3 playerEyesPosition;
        public bool hasFiredProjectile;
        public List<HitData> hitsData;
        public float gravityModifier;
        public float drag;
        public float forgivenessModifier;
        public float physicsSteps;
        public List<string> attachments;
    }

    private class NoRecoilViolationData
    {
        public int ShotsCnt;
        public int NRViolationsCnt;
        public float violationProbability;
        public bool isMounted;
        public Vector3 mountParentPosition;
        public Vector4 mountParentRotation;
        public List<string> attachments;
        public string ammoShortName;
        public string weaponShortName;
        public Dictionary<int, SuspiciousProjectileData> suspiciousNoRecoilShots;
    }

    private class InRockViolationsData
    {
        public DateTime dateTime;
        public Dictionary<int, InRockViolationData> inRockViolationsData;
    }

    private class InRockViolationData
    {
        public DateTime dateTime;
        public float physicsSteps;
        public float targetHitDistance;
        public string targetName;
        public string targetID;
        public float targetDamage;
        public string targetBodyPart;
        public Vector3 targetHitPosition;
        public Vector3 rockHitPosition;
        public FiredProjectile firedProjectile;
        public int projectileID;
        public float drag;
        public float gravityModifier;
    }

    private class FiredShotsData
    {
        public string ammoShortName;
        public string weaponShortName;
        public List<string> attachments;
        public SortedDictionary<int, FiredProjectile> firedShots;
    }

    private class FiredProjectile
    {
        public DateTime firedTime;
        public Vector3 projectileVelocity;
        public Vector3 projectilePosition;
        public Vector3 playerEyesLookAt;
        public Vector3 playerEyesPosition;
        public bool isChecked;
        public string ammoShortName;
        public string weaponShortName;
        public ItemId weaponUID;
        public bool isMounted;
        public string mountParentName;
        public Vector3 mountParentPosition;
        public Vector4 mountParentRotation;
        public List<ProjectileRicochet> hitsData;
        public List<string> attachments;
        public float NRProbabilityModifier;
    }

    private class TrajectorySegment
    {
        public Vector3 pointStart;
        public Vector3 pointEnd;
    }

    public class EmbedFieldList
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
    }

    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
     void OnPlayerConnected(BasePlayer player);
    private string Lang(string key, string id, object[] args);
    public string RemoveFormatting(string source);
    private void Unload();
    private void Loaded();
    private void OnServerSave();
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private void OnMeleeThrown(BasePlayer player, Item item);
    private void OnItemPickup(Item item, BasePlayer player);
    private void OnProjectileRicochet(BasePlayer player, PlayerProjectileRicochet playerProjectileRicochet);
    private void OnWeaponFired(BaseProjectile projectile, BasePlayer player, ItemModProjectile mod, ProjectileShoot projectileShoot);
    private void OnEntityTakeDamage(BasePlayer entity, HitInfo info);
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    [ConsoleCommand("arkan")]
    private void ConsoleShowLog(ConsoleSystem.Arg arg);
    [ConsoleCommand("arkan.nr")]
    private void ConsoleShowNoRecoilLog(ConsoleSystem.Arg arg);
    [ConsoleCommand("arkan.aim")]
    private void ConsoleShowAIMLog(ConsoleSystem.Arg arg);
    [ConsoleCommand("arkan.ir")]
    private void ConsoleShowInRockLog(ConsoleSystem.Arg arg);
    [ConsoleCommand("arkan.clear")]
    private void ConsoleClearViolationsData(ConsoleSystem.Arg arg);
    [ConsoleCommand("arkan.save")]
    private void ConsoleSaveViolationsData(ConsoleSystem.Arg arg);
    [ConsoleCommand("arkan.load")]
    private void ConsoleLoadViolationsData(ConsoleSystem.Arg arg);
    [ChatCommand("arkanclear")]
    private void ClearViolationsData(BasePlayer player, string command, string[] args);
    [ChatCommand("arkansave")]
    private void SaveViolationsData(BasePlayer player, string command, string[] args);
    [ChatCommand("arkanload")]
    private void LoadViolationsData(BasePlayer player, string command, string[] args);
    [ChatCommand("arkan")]
    private void ShowLog(BasePlayer player, string command, string[] args);
    [ChatCommand("arkaninfo")]
    private void ShowInfo(BasePlayer player, string command, string[] args);
    [ChatCommand("arkannr")]
    private void ShowNoRecoilLog(BasePlayer player, string command, string[] args);
    [ChatCommand("arkanaim")]
    private void ShowAIMLog(BasePlayer player, string command, string[] args);
    [ChatCommand("arkanaimr")]
    private void ShowAIMLogRecalc(BasePlayer player, string command, string[] args);
    [ChatCommand("arkanir")]
    private void ShowInRocklLog(BasePlayer player, string command, string[] args);
    private string API_ArkanGetPlayersViolationsData();
    private void CleanupExpiredProjectiles(BasePlayer player);
    private void DDrawArrowToAdmin(BasePlayer player, float _drawTime, Color color, Vector3 startPosition, Vector3 endPosition, float arrowHeadSize);
    private void DDrawSphereToAdmin(BasePlayer player, float _drawTime, Color color, Vector3 Position, float sphereSize);
    private void DDrawTextToAdmin(BasePlayer player, float _drawTime, Color color, Vector3 Position, string text);
    private void DrawInRockViolationsData(BasePlayer player, string suspectPlayerName, string suspectPlayerID, int vsCnt, InRockViolationsData violationData, bool isTeleport);
    private void DrawNoRecoilViolationsData(BasePlayer player, string suspectPlayerName, NoRecoilViolationData violationData, bool isTeleport);
    private void DrawAIMViolationsData(BasePlayer player, AIMViolationData violationData, bool isTeleport);
    private void ShowNoRecoilViolations(BasePlayer player, string[] args);
    private void ShowAIMViolations(BasePlayer player, string[] args);
    private void ShowAIMViolationsRecalc(BasePlayer player, string[] args);
    private void ShowInRockViolations(BasePlayer player, string[] args);
    private bool ClosestPointsOnTwoLines(Vector3 closestPointLine1, Vector3 closestPointLine2, Vector3 linePoint1, Vector3 lineVec1, Vector3 linePoint2, Vector3 lineVec2);
    private Vector3 ProjectPointOnLine(Vector3 linePoint, Vector3 lineVec, Vector3 point);
    private Vector3 ProjectPointOnLineSegment(Vector3 linePoint1, Vector3 linePoint2, Vector3 point, int side);
    private int PointOnWhichSideOfLineSegment(Vector3 linePoint1, Vector3 linePoint2, Vector3 point);
    private bool IsPointInRock(Vector3 pointPosition, float distance, int rocksUnderPoint);
    private void ProcessShots(BasePlayer player);
    private void ProcessFiredShotsBlock(BasePlayer player, FiredShotsData fsd, float NRProbabilityModifier);
    private void ShootingInRockCheck(BasePlayer player, FiredProjectile fp, HitInfo info, string bodyPart, float physicsSteps);
    private void InRockNotification(BasePlayer player, string key, int vsCnt, int sCnt);
    private bool ProcessProjectileTrajectory(AIMViolationData aimvd, AIMViolationData aimvdIn, List<TrajectorySegment> trajectorySegments, List<TrajectorySegment> trajectorySegmentsRev, float gravityModifier, float drag);
    private void AddNoRecoilViolationToPlayer(BasePlayer player, NoRecoilViolationData noRecoilViolationData);
    private void AddAIMViolationToPlayer(BasePlayer player, AIMViolationData _AIMViolationData);
    private void AddInRockViolationToPlayer(BasePlayer player, InRockViolationsData InRockViolationData);
    private bool IsNPC(BasePlayer player);
    private void AdminLogInit(BasePlayer player);
    private bool IsLastSegmentCloseToProjectileTrajectoryPlane(HitData hitData, float projectileForgiveness, float distance);
    private bool IsHitPointCloseToProjectileTrajectory(HitData hitData, float gravityModifier, float drag, float projectileForgiveness, Vector3 lsVecStart, Vector3 lsVecEnd, float travelDistance, List<TrajectorySegment> trajectorySegments, float physicsSteps);
    private bool IsProjectileStartPointCloseToAtEndReverseProjectileTrajectory(HitData hitData, float gravityModifier, float drag, float projectileForgiveness, Vector3 lsVecStart, Vector3 lsVecEnd, List<TrajectorySegment> trajectorySegmentsRev, float physicsSteps);
    private void DrawProjectileTrajectory(BasePlayer player, float _drawTime, AIMViolationData aimvd, Color lineColor);
    private void DrawReverseProjectileTrajectory(BasePlayer player, float _drawTime, AIMViolationData aimvd, Color lineColor);
    private void DrawProjectileTrajectory2(BasePlayer player, float _drawTime, FiredProjectile fp, HitInfo info, Color lineColor, float physicsSteps);
    private bool IsRicochet(List<TrajectorySegment> trajectorySegments, List<TrajectorySegment> trajectorySegmentsRev, HitData hitData1, HitData hitData2, float physicsSteps);
}

private class Configuration
{
    public float NRProcessTimer;
    public float EPSILON;
    public float projectileTrajectoryForgiveness;
    public float hitDistanceForgiveness;
    public float minDistanceAIMCheck;
    public float inRockCheckDistance;
    public bool isDetectAIM;
    public bool isDetectNR;
    public bool isDetectIR;
    public bool debugMessages;
    public bool autoSave;
    public bool isCheckAIMOnTargetNPC;
    public float drawTime;
    public float NRViolationAngle;
    public float NRViolationScreenDistance;
    public float playerEyesPositionToProjectileInitialPositionDistanceForgiveness;
    [JsonProperty(PropertyName = "The maximum allowed value for the physics.steps parameter")]
    public float minPhysicsStepsAllowed;
    [JsonProperty(PropertyName = "Check players only on the blacklist")]
    public bool checkBlacklist;
    [JsonProperty(PropertyName = "Notify when a player has a high value of the physics.steps parameter")]
    public bool notifyPhysicsStepsWarning;
    [JsonProperty(PropertyName = "Enable Discord No Recoil Notifications")]
    public bool DiscordNRReportEnabled;
    [JsonProperty(PropertyName = "Enable Discord AIMBOT Notifications")]
    public bool DiscordAIMReportEnabled;
    [JsonProperty(PropertyName = "Enable Discord In Rock Notifications")]
    public bool DiscordIRReportEnabled;
    [JsonProperty(PropertyName = "Discord No Recoil Webhook URL")]
    public string DiscordNRWebhookURL;
    [JsonProperty(PropertyName = "Discord AIMBOT Webhook URL")]
    public string DiscordAIMWebhookURL;
    [JsonProperty(PropertyName = "Discord In Rock Webhook URL")]
    public string DiscordIRWebhookURL;
    [JsonProperty(PropertyName = "AIMBodyParts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> AIMBodyParts;
    [JsonProperty(PropertyName = "IRBodyParts", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> IRBodyParts;
    [JsonProperty(PropertyName = "weaponsConfig")]
    public Dictionary<string, WeaponConfig> weaponsConfig;
}

private class WeaponConfig
{
    public bool NRDetectEnabled;
    public bool AIMDetectEnabled;
    public float weaponMinTimeShotsInterval;
    public float weaponMaxTimeShotsInterval;
    public int NRMinShotsCountToCheck;
    public float NRViolationProbability;
}

private class ViolationsLog
{
    public ulong steamID;
    public int NoRecoilViolation;
    public int AIMViolation;
    public int InRockViolation;
}

private class AdminConfig
{
    public ViolationsLog violationsLog;
}

private class PlayersViolationsData
{
    public int seed;
    public int mapSize;
    public string serverTimeStamp;
    public DateTime lastSaveTime;
    public DateTime lastChangeTime;
    public Dictionary<ulong, PlayerViolationsData> Players;
}

private class PlayerFiredProjectlesData
{
    public ulong PlayerID;
    public string PlayerName;
    public float lastFiredTime;
    public float physicsSteps;
    public SortedDictionary<int, FiredProjectile> firedProjectiles;
    public SortedDictionary<ulong, MeleeThrown> melees;
    public bool isChecked;
}

private class PlayerViolationsData
{
    public ulong PlayerID;
    public string PlayerName;
    public SortedDictionary<string, NoRecoilViolationData> noRecoilViolations;
    public SortedDictionary<string, AIMViolationData> AIMViolations;
    public SortedDictionary<string, InRockViolationsData> inRockViolations;
}

private class HitData
{
    public ProjectileRicochet hitData;
    public Vector3 startProjectilePosition;
    public Vector3 startProjectileVelocity;
    public Vector3 hitPositionWorld;
    public Vector3 hitPointStart;
    public Vector3 hitPointEnd;
    public bool isHitPointNearProjectileTrajectoryLastSegmentEndPoint;
    public bool isHitPointOnProjectileTrajectory;
    public bool isProjectileStartPointAtEndReverseProjectileTrajectory;
    public bool isHitPointNearProjectilePlane;
    public bool isLastSegmentOnProjectileTrajectoryPlane;
    public float distanceFromHitPointToProjectilePlane;
    public int side;
    public Vector3 pointProjectedOnLastSegmentLine;
    public float travelDistance;
    public float delta;
    public Vector3 lastSegmentPointStart;
    public Vector3 lastSegmentPointEnd;
    public Vector3 reverseLastSegmentPointStart;
    public Vector3 reverseLastSegmentPointEnd;
}

private class AIMViolationData
{
    public int projectileID;
    public int violationID;
    public DateTime firedTime;
    public Vector3 startProjectilePosition;
    public Vector3 startProjectileVelocity;
    public string hitInfoInitiatorPlayerName;
    public string hitInfoInitiatorPlayerUserID;
    public string hitInfoHitEntityPlayerName;
    public string hitInfoHitEntityPlayerUserID;
    public string hitInfoBoneName;
    public Vector3 hitInfoHitPositionWorld;
    public float hitInfoProjectileDistance;
    public Vector3 hitInfoPointStart;
    public Vector3 hitInfoPointEnd;
    public float hitInfoProjectilePrefabGravityModifier;
    public float hitInfoProjectilePrefabDrag;
    public string weaponShortName;
    public string ammoShortName;
    public string bodyPart;
    public float damage;
    public bool isEqualFiredProjectileData;
    public bool isPlayerPositionToProjectileStartPositionDistanceViolation;
    public float distanceDifferenceViolation;
    public float calculatedTravelDistance;
    public bool isAttackerMount;
    public bool isTargetMount;
    public string attackerMountParentName;
    public string targetMountParentName;
    public float firedProjectileFiredTime;
    public float firedProjectileTravelTime;
    public Vector3 firedProjectilePosition;
    public Vector3 firedProjectileVelocity;
    public Vector3 firedProjectileInitialPosition;
    public Vector3 firedProjectileInitialVelocity;
    public Vector3 playerEyesLookAt;
    public Vector3 playerEyesPosition;
    public bool hasFiredProjectile;
    public List<HitData> hitsData;
    public float gravityModifier;
    public float drag;
    public float forgivenessModifier;
    public float physicsSteps;
    public List<string> attachments;
}

private class NoRecoilViolationData
{
    public int ShotsCnt;
    public int NRViolationsCnt;
    public float violationProbability;
    public bool isMounted;
    public Vector3 mountParentPosition;
    public Vector4 mountParentRotation;
    public List<string> attachments;
    public string ammoShortName;
    public string weaponShortName;
    public Dictionary<int, SuspiciousProjectileData> suspiciousNoRecoilShots;
}

private class InRockViolationsData
{
    public DateTime dateTime;
    public Dictionary<int, InRockViolationData> inRockViolationsData;
}

private class InRockViolationData
{
    public DateTime dateTime;
    public float physicsSteps;
    public float targetHitDistance;
    public string targetName;
    public string targetID;
    public float targetDamage;
    public string targetBodyPart;
    public Vector3 targetHitPosition;
    public Vector3 rockHitPosition;
    public FiredProjectile firedProjectile;
    public int projectileID;
    public float drag;
    public float gravityModifier;
}

private class FiredShotsData
{
    public string ammoShortName;
    public string weaponShortName;
    public List<string> attachments;
    public SortedDictionary<int, FiredProjectile> firedShots;
}

private class FiredProjectile
{
    public DateTime firedTime;
    public Vector3 projectileVelocity;
    public Vector3 projectilePosition;
    public Vector3 playerEyesLookAt;
    public Vector3 playerEyesPosition;
    public bool isChecked;
    public string ammoShortName;
    public string weaponShortName;
    public ItemId weaponUID;
    public bool isMounted;
    public string mountParentName;
    public Vector3 mountParentPosition;
    public Vector4 mountParentRotation;
    public List<ProjectileRicochet> hitsData;
    public List<string> attachments;
    public float NRProbabilityModifier;
}

private class TrajectorySegment
{
    public Vector3 pointStart;
    public Vector3 pointEnd;
}

public class EmbedFieldList
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
}


```

---

## ArmorNotForever by Flames - This plugin allows NPC reduce the durability of equipped armor.

```csharp
using System.Linq;
using UnityEngine;
using Newtonsoft.Json;
using System.Collections.Generic;

Oxide.Plugins
[Info("Armor Not Forever", "Flames", "1.0.10")]
[Description("On hit NPC to not only deal damage to the player, but also reduce the durability of equipped armor")]
 class ArmorNotForever : RustPlugin
{
    private const string PermissionUse;
    private const string PermissionDamageTotal;
    private const string PermissionDamageMultiplier;
    private const string PermissionDamageBypass;
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalSettings globalSettings;
        [JsonProperty(PropertyName = "Multiplier settings")]
        public MultiplierSettings multiplierSettings;
        public class GlobalSettings
        {
            [JsonProperty(PropertyName = "Debag")]
            public bool Debag;
            [JsonProperty(PropertyName = "Damage from (Suicide). Does not damage armor.")]
            public bool DamageTypeSuicide;
            [JsonProperty(PropertyName = "Damage from (Bleeding). Does not damage armor.")]
            public bool DamageTypeBleeding;
            [JsonProperty(PropertyName = "Damage from (Drowning). Does not damage armor.")]
            public bool DamageTypeDrowning;
            [JsonProperty(PropertyName = "Damage from (Thirst). Does not damage armor.")]
            public bool DamageTypeThirst;
            [JsonProperty(PropertyName = "Damage from (Hunger). Does not damage armor.")]
            public bool DamageTypeHunger;
            [JsonProperty(PropertyName = "Damage from (Cold). Does not damage armor.")]
            public bool DamageTypeCold;
            [JsonProperty(PropertyName = "Damage from (Heat). Does not damage armor.")]
            public bool DamageTypeHeat;
            [JsonProperty(PropertyName = "Damage from (Fall). Does not damage armor.")]
            public bool DamageTypeFall;
            [JsonProperty(PropertyName = "Damage from (Radiation). Does not damage armor.")]
            public bool DamageTypeRadiation;
        }

        public class MultiplierSettings
        {
            [JsonProperty(PropertyName = "Item list Head (item shortname : multiplier)")]
            public Dictionary<string, float> list_Head;
            [JsonProperty(PropertyName = "Item list Body (item shortname : multiplier)")]
            public Dictionary<string, float> list_Body;
            [JsonProperty(PropertyName = "Item list Pants (item shortname : multiplier)")]
            public Dictionary<string, float> list_Pants;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Init();
     void Unload();
     void OnEntityTakeDamage(BasePlayer player, HitInfo info);
     object CanWearItem(PlayerInventory inventory, Item item, int targetSlot);
    private bool CheckDamageType(HitInfo info);
    private static bool GetRandom();
    private void Message(BasePlayer player, string messageKey, object[] args);
    private string GetMessage(string messageKey, string playerID, object[] args);
    protected override void LoadDefaultMessages();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalSettings globalSettings;
    [JsonProperty(PropertyName = "Multiplier settings")]
    public MultiplierSettings multiplierSettings;
    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Debag")]
        public bool Debag;
        [JsonProperty(PropertyName = "Damage from (Suicide). Does not damage armor.")]
        public bool DamageTypeSuicide;
        [JsonProperty(PropertyName = "Damage from (Bleeding). Does not damage armor.")]
        public bool DamageTypeBleeding;
        [JsonProperty(PropertyName = "Damage from (Drowning). Does not damage armor.")]
        public bool DamageTypeDrowning;
        [JsonProperty(PropertyName = "Damage from (Thirst). Does not damage armor.")]
        public bool DamageTypeThirst;
        [JsonProperty(PropertyName = "Damage from (Hunger). Does not damage armor.")]
        public bool DamageTypeHunger;
        [JsonProperty(PropertyName = "Damage from (Cold). Does not damage armor.")]
        public bool DamageTypeCold;
        [JsonProperty(PropertyName = "Damage from (Heat). Does not damage armor.")]
        public bool DamageTypeHeat;
        [JsonProperty(PropertyName = "Damage from (Fall). Does not damage armor.")]
        public bool DamageTypeFall;
        [JsonProperty(PropertyName = "Damage from (Radiation). Does not damage armor.")]
        public bool DamageTypeRadiation;
    }

    public class MultiplierSettings
    {
        [JsonProperty(PropertyName = "Item list Head (item shortname : multiplier)")]
        public Dictionary<string, float> list_Head;
        [JsonProperty(PropertyName = "Item list Body (item shortname : multiplier)")]
        public Dictionary<string, float> list_Body;
        [JsonProperty(PropertyName = "Item list Pants (item shortname : multiplier)")]
        public Dictionary<string, float> list_Pants;
    }

}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Debag")]
    public bool Debag;
    [JsonProperty(PropertyName = "Damage from (Suicide). Does not damage armor.")]
    public bool DamageTypeSuicide;
    [JsonProperty(PropertyName = "Damage from (Bleeding). Does not damage armor.")]
    public bool DamageTypeBleeding;
    [JsonProperty(PropertyName = "Damage from (Drowning). Does not damage armor.")]
    public bool DamageTypeDrowning;
    [JsonProperty(PropertyName = "Damage from (Thirst). Does not damage armor.")]
    public bool DamageTypeThirst;
    [JsonProperty(PropertyName = "Damage from (Hunger). Does not damage armor.")]
    public bool DamageTypeHunger;
    [JsonProperty(PropertyName = "Damage from (Cold). Does not damage armor.")]
    public bool DamageTypeCold;
    [JsonProperty(PropertyName = "Damage from (Heat). Does not damage armor.")]
    public bool DamageTypeHeat;
    [JsonProperty(PropertyName = "Damage from (Fall). Does not damage armor.")]
    public bool DamageTypeFall;
    [JsonProperty(PropertyName = "Damage from (Radiation). Does not damage armor.")]
    public bool DamageTypeRadiation;
}

public class MultiplierSettings
{
    [JsonProperty(PropertyName = "Item list Head (item shortname : multiplier)")]
    public Dictionary<string, float> list_Head;
    [JsonProperty(PropertyName = "Item list Body (item shortname : multiplier)")]
    public Dictionary<string, float> list_Body;
    [JsonProperty(PropertyName = "Item list Pants (item shortname : multiplier)")]
    public Dictionary<string, float> list_Pants;
}


```

---

## ArrowRaiding by birthdates - Allow players to break wooden doors using bow and arrow

```csharp
using Newtonsoft.Json;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Arrow Raiding", "birthdates", "2.0.1")]
[Description("Break wooden doors with arrows like old Rust")]
public class ArrowRaiding : RustPlugin
{
    private readonly string permission_use;
    private void Init();
     void OnEntityTakeDamage(Door entity, HitInfo info);
    public ConfigFile _config;
    public class ArrowDamage
    {
        [JsonProperty("Minimum Damage")]
        public float MinDamage;
        [JsonProperty("Maximum Damage")]
        public float MaxDamage;
    }

    public class ConfigFile
    {
        [JsonProperty("Arrow Damage")]
        public Dictionary<string, ArrowDamage> ArrowDamage;
        [JsonProperty("Weapon Multipliers")]
        public Dictionary<string, float> BowMultipliers;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ArrowDamage
{
    [JsonProperty("Minimum Damage")]
    public float MinDamage;
    [JsonProperty("Maximum Damage")]
    public float MaxDamage;
}

public class ConfigFile
{
    [JsonProperty("Arrow Damage")]
    public Dictionary<string, ArrowDamage> ArrowDamage;
    [JsonProperty("Weapon Multipliers")]
    public Dictionary<string, float> BowMultipliers;
    public static ConfigFile DefaultConfig();
}


```

---

## AssetsChecker by SettLe - Extracts changes from the game’s assets and writes them to sorted lists.

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Assets Checker", "SettLe", "1.0.5")]
[Description("Extracts changes from the game’s assets and writes them to sorted lists.")]
public class AssetsChecker : RustPlugin
{
    private GameManifest.PooledString[] _manifest;
    private bool _inProcess;
    private List<string> _allPrefabs;
    private List<string> _allAssets;
    private List<string> _allImages;
    private List<string> _allOther;
    private List<string> _oldPrefabs;
    private List<string> _oldAssets;
    private List<string> _oldImages;
    private List<string> _oldOther;
    private List<string> _lostPrefabs;
    private List<string> _lostAssets;
    private List<string> _lostImages;
    private List<string> _lostOther;
    private List<string> _newPrefabs;
    private List<string> _newAssets;
    private List<string> _newImages;
    private List<string> _newOther;
    private void OnServerInitialized();
    private static void LoadData(T data, string file);
    private static void SaveData(T data, string file);
    private void Unload();
    private void LoadOld();
    private void SaveAll();
    private void SaveLost(string folder);
    private void SaveNew(string folder);
    private void Clear();
    private void Search();
    [ConsoleCommand("check_assets")]
    private void CommandCheckAssets(ConsoleSystem.Arg console);
}


```

---

## aTimeAPI by AvGLimon - Provides API and Hooks which called when in-game day/night and real hour/day/month/year started

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using System;
using System.Collections;
using UnityEngine;

Oxide.Plugins
[Info("aTimeAPI", "AvG Лаймон", "1.1.3")]
[Description("Provides API and Hooks for time.")]
 class aTimeAPI : RustPlugin
{
    private static DataFileSystem fileSystem;
    private void OnServerInitialized();
    private void Unload();
    private Data data;
    private class Data
    {
        public bool IsDay;
        public int Year;
        public int Month;
        public int Day;
        public int Hour;
    }

    private Configuration config;
    private class Configuration
    {
        [JsonProperty("Rust day start time (hour)")]
        public float Day;
        [JsonProperty("Rust night start time (hour)")]
        public float Night;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private bool IsDayInRustNow();
    private Coroutine CheckTimeCoroutine;
    private IEnumerator CheckTime();
}

private class Data
{
    public bool IsDay;
    public int Year;
    public int Month;
    public int Day;
    public int Hour;
}

private class Configuration
{
    [JsonProperty("Rust day start time (hour)")]
    public float Day;
    [JsonProperty("Rust night start time (hour)")]
    public float Night;
}


```

---

## AttachmentBlocker by 0x89A - Block application of specific attachments per weapon

```csharp
using System;
using System.Collections.Generic;
using UnityEngine;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Attachment Blocker", "0x89A", "1.0.1")]
[Description("Block application of specific attachments per weapon")]
 class AttachmentBlocker : RustPlugin
{
    private Configuration config;
    private const string bypassPerm;
     void Init();
     ItemContainer.CanAcceptResult? CanAcceptItem(ItemContainer container, Item item, int targetPos);
    private class Configuration
    {
        [JsonProperty(PropertyName = "Blocked Items")]
        public Dictionary<string, List<string>> blockedItems;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private bool HasInventory(ItemDefinition itemdef);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Blocked Items")]
    public Dictionary<string, List<string>> blockedItems;
}


```

---

## Authentication by FakeNinja - Players must enter a password after they wake up or else they'll be kicked

```csharp
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("Authentication", "FakeNinja", "2.1.2")]
[Description("Players must enter a password after they wake up or else they'll be kicked")]
public class Authentication : RustPlugin
{
    public class Request
    {
        public string m_steamID;
        public BasePlayer m_basePlayer;
        public bool m_authenticated;
        public int m_retries;
        public Timer m_countdown;
        public Request(string steamID, BasePlayer basePlayer);
    }

    static List<Request> requests;
    private void write(string message);
    private void write(BasePlayer player, string message);
    private bool isEnabled();
    private void requestAuth(Request request);
    [ChatCommand("auth")]
    private void cmdAuth(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand("global.auth")]
     void ccmdAuth(ConsoleSystem.Arg arg);
     void Init();
     void OnServerInitialized();
     void OnPlayerSleepEnded(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     object OnPlayerChat(BasePlayer player, string message);
    protected override void LoadDefaultConfig();
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id);
}

public class Request
{
    public string m_steamID;
    public BasePlayer m_basePlayer;
    public bool m_authenticated;
    public int m_retries;
    public Timer m_countdown;
    public Request(string steamID, BasePlayer basePlayer);
}


```

---

## Authenticator by  - Allows players to add passwords to their accounts on your server

```csharp
using System.Text;
using System.Collections.Generic;
using System.Security.Cryptography;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Authenticator", "Spicy", "2.0.0")]
[Description("Provides a simple login system.")]
 class Authenticator : CovalencePlugin
{
    private SHA512 sha512;
    private HashSet<string> authenticatedUsers;
    private Data data;
    private int kickTimer;
    private string Syntax(string syntaxKey);
    private string _(string key);
    private byte[] Hash(string password);
    private string ByteArrayToString(byte[] array);
    private void Check(IPlayer player);
    protected override void LoadDefaultConfig();
    private class Data
    {
        public Dictionary<string, byte[]> Players;
    }

    private void OnServerInitialized();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player, string reason);
    [Command("auth")]
    private void Auth(IPlayer player, string command, string[] args);
}

private class Data
{
    public Dictionary<string, byte[]> Players;
}


```

---

## AuthLevelFix by MrBlue - Fix player with lower auth levels running certain commands

```csharp
using Newtonsoft.Json;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info( "Auth Level Fix", "Mr. Blue", "0.0.1" )]
[Description( "Stop moderators being able to add moderator or owners" )]
 class AuthLevelFix : CovalencePlugin
{
    private Configuration _config;
    protected override void LoadDefaultMessages();
    private string GetMessage(string key, string steamId);
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class Configuration
    {
        [JsonProperty( "Log when user tries to run commands" )]
        public bool LogWarning { get; set; }
        [JsonProperty( "Show blocked message to user trying to run commands" )]
        public bool MessageUser { get; set; }
        [JsonProperty( "Blocked Commands" )]
        public HashSet<string> BlockedCommands { get; set; }
    }

    private object OnServerCommand(ConsoleSystem.Arg arg);
}

private class Configuration
{
    [JsonProperty( "Log when user tries to run commands" )]
    public bool LogWarning { get; set; }
    [JsonProperty( "Show blocked message to user trying to run commands" )]
    public bool MessageUser { get; set; }
    [JsonProperty( "Blocked Commands" )]
    public HashSet<string> BlockedCommands { get; set; }
}


```

---

## AuthLimiter by supreme - Limits the moderators from using ownerid/removeowner commands

```csharp
using Network;
using UnityEngine;

Oxide.Plugins
[Info("Auth Limiter", "supreme", "1.0.2")]
[Description("Limits the moderators from using ownerid/removeownerid commands")]
public class AuthLimiter : RustPlugin
{
    private object OnServerCommand(ConsoleSystem.Arg arg);
    private bool IsLimited(BasePlayer player);
}


```

---

## AutoBroadcast by MrBlue - Sends randomly configured chat messages every X amount of seconds

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Auto Broadcast", "Wulf", "1.0.9")]
[Description("Sends randomly configured chat messages every X amount of seconds")]
 class AutoBroadcast : CovalencePlugin
{
    const string permBroadcast;
     bool random;
     int interval;
     int nextKey;
    protected override void LoadDefaultConfig();
     void OnServerInitialized();
     void Broadcast();
    [Command("broadcast")]
     void CmdChatBroadcast(IPlayer player, string command, string[] args);
     T GetConfig(string name, T value);
     string Lang(string key, string id, object[] args);
}


```

---

## AutoChat by MrBlue - Automatic clans/private chat switching

```csharp
using System;
using System.Linq;
using System.Globalization;
using System.Collections.Generic;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using Oxide.Core.Libraries.Covalence;
using UnityEngine;

Oxide.Plugins
[Info("AutoChat", "Frenk92", "0.5.1", ResourceId = 2230)]
[Description("Automatic clans/private chat switching")]
 class AutoChat : RustPlugin
{
    [PluginReference]
     Plugin BetterChat;
     Plugin AdminChatroom;
     bool BC;
    const string PermAdmin;
    const string PermUse;
    static AutoChat ac;
     class ChatInfo : MonoBehaviour
    {
        public BasePlayer player { get; set; }
        public string cmd { get; set; }
        public string target { get; set; }
        public int count { get; set; }
        public bool ui { get; set; }
        public string fullcmd { get; set; }
         void Awake();
         void Start();
        public void Stop();
         void ChatUpdate();
         void FixedUpdate();
         void ToggleUI(bool flag);
        public void Switch(string cmd, string target);
        public void Destroy();
    }

     ConfigData _config;
     class ConfigData
    {
        public bool Enabled { get; set; }
        public bool PlayerActive { get; set; }
        public bool ShowSwitchMessage { get; set; }
        public int SwitchTime { get; set; }
        public Dictionary<string, List<string>> CustomChat { get; set; }
        public bool UIEnabled { get; set; }
        public UIConfig UISettings { get; set; }
    }

     class UIConfig
    {
        public string BackgroundColor { get; set; }
        public string TextColor { get; set; }
        public string FontSize { get; set; }
        public string AnchorMin { get; set; }
        public string AnchorMax { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     List<string> ChatType;
     List<string> LoadedPlugins;
     Dictionary<string, List<string>> cmdPlugins;
     Dictionary<ulong, PlayerChat> Users;
     class PlayerChat
    {
        public string Name { get; set; }
        public bool Active { get; set; }
        public PlayerChat(string Name, bool Active);
    }

    private void LoadData();
    private void SaveData();
     PlayerChat GetPlayerData(BasePlayer player);
     void OnServerInitialized();
     void Loaded();
     void Unload();
     void OnPlayerInit(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player, string reason);
     void OnPluginLoaded(Plugin pl);
     void OnPluginUnloaded(Plugin pl);
    [ChatCommand("ac")]
    private void cmdAutoChat(BasePlayer player, string command, string[] args);
    [ChatCommand("g")]
    private void cmdGlobalChat(BasePlayer player, string command, string[] args);
    [ConsoleCommand("ac")]
    private void consAutoChat(ConsoleSystem.Arg arg);
    private void OnUserCommand(IPlayer player, string command, string[] args);
     object OnPlayerChat(ConsoleSystem.Arg arg);
     object OnBetterChat(Dictionary<string, object> data);
    private new void LoadDefaultMessages();
     string Lang(string key, string id, object[] args);
     void MessageChat(BasePlayer player, string key, object[] args);
     void Print(string key, object[] args);
     void CheckPlugins();
     bool isActive(string id);
     bool isActive(ulong id);
     bool HasPermission(string id, string perm);
    static string cuiJson;
     void AddUI(BasePlayer player, string cmd);
    static void DestroyUI(BasePlayer player);
    public static string Color(string hexColor);
}

 class ChatInfo : MonoBehaviour
{
    public BasePlayer player { get; set; }
    public string cmd { get; set; }
    public string target { get; set; }
    public int count { get; set; }
    public bool ui { get; set; }
    public string fullcmd { get; set; }
     void Awake();
     void Start();
    public void Stop();
     void ChatUpdate();
     void FixedUpdate();
     void ToggleUI(bool flag);
    public void Switch(string cmd, string target);
    public void Destroy();
}

 class ConfigData
{
    public bool Enabled { get; set; }
    public bool PlayerActive { get; set; }
    public bool ShowSwitchMessage { get; set; }
    public int SwitchTime { get; set; }
    public Dictionary<string, List<string>> CustomChat { get; set; }
    public bool UIEnabled { get; set; }
    public UIConfig UISettings { get; set; }
}

 class UIConfig
{
    public string BackgroundColor { get; set; }
    public string TextColor { get; set; }
    public string FontSize { get; set; }
    public string AnchorMin { get; set; }
    public string AnchorMax { get; set; }
}

 class PlayerChat
{
    public string Name { get; set; }
    public bool Active { get; set; }
    public PlayerChat(string Name, bool Active);
}


```

---

## AutoCode by BlueBeka - Automatically sets the code on code locks when placed

```csharp
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Libraries;
using System;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Auto Code", "BlueBeka", "1.6.0")]
[Description("Automatically sets the code on code locks when placed.")]
public class AutoCode : RustPlugin
{
    [PluginReference("NoEscape")]
    private Plugin pluginNoEscape;
    private AutoCodeConfig config;
    private Commands commands;
    private Data data;
    private Dictionary<BasePlayer, TempCodeLockInfo> tempCodeLocks;
    private const string HiddenCode;
    private void Init();
    protected override void LoadDefaultConfig();
    private void OnServerSave();
    private void Unload();
    private object OnCodeEntered(CodeLock codeLock, BasePlayer player, string code);
    private void OnItemDeployed(Deployer deployer, BaseEntity lockable, CodeLock codeLock);
    private object CanUseLockedEntity(BasePlayer player, CodeLock codeLock);
    protected override void LoadDefaultMessages();
    [ObsoleteAttribute("This method is deprecated. Call GetCode instead.", false)]
    public string GetPlayerCode(BasePlayer player);
    public string GetCode(BasePlayer player, bool guest);
    public void SetCode(BasePlayer player, string code, bool guest, bool quiet, bool hideCode);
    [ObsoleteAttribute("This method is deprecated.", true)]
    public void ToggleEnabled(BasePlayer player);
    public void RemoveCode(BasePlayer player, bool guest, bool quiet);
    [ObsoleteAttribute("This method is deprecated. Call IsValidCode instead.", false)]
    public bool ValidCode(string codeString);
    public bool IsValidCode(string codeString);
    [ObsoleteAttribute("This method is deprecated. Call GenerateRandomCode instead.", false)]
    public static string GetRandomCode();
    public string GenerateRandomCode();
    public void ToggleQuietMode(BasePlayer player, bool quiet);
    public void ResetAllLockOuts();
    public void ResetLockOut(BasePlayer player);
    public void ResetLockOut(ulong userID);
    private void DestroyTempCodeLock(BasePlayer player);
    private void RemoveAllTempCodeLocks();
    private void UnsubscribeFromUnneededHooks();
    private void Message(BasePlayer player, string message);
    private class AutoCodeConfig
    {
        private readonly AutoCode plugin;
        public readonly DynamicConfigFile OxideConfig;
        private bool UnsavedChanges;
        public AutoCodeConfig();
        public AutoCodeConfig(AutoCode plugin);
        public CommandsDef Commands;
        public class CommandsDef
        {
            public string Use;
            public ulong ChatIconId;
        }

        public OptionsDef Options;
        public class OptionsDef
        {
            public bool DisplayPermissionErrors;
            public SpamPreventionDef SpamPrevention;
            public PluginIntegrationDef PluginIntegration;
            public class SpamPreventionDef
            {
                public bool Enabled;
                public int Attempts;
                public double LockOutTime;
                public double WindowTime;
                public bool UseExponentialLockOutTime;
                public double LockOutResetFactor;
            }

            public class PluginIntegrationDef
            {
                public NoEscapeDef NoEscape;
                public class NoEscapeDef
                {
                    public bool BlockRaid;
                    public bool BlockCombat;
                }

            }

        }

        public void Save(bool force);
        public void Load();
        private T GetConfigValue(string[] settingPath, T defaultValue, bool deprecated);
        private void SetConfigValue(string[] settingPath, T newValue);
        private void RemoveConfigValue(string[] settingPath);
    }

    private class Data
    {
        private readonly string Filename;
        public Structure Inst { get; set; }
        public Data(AutoCode plugin);
        public void Save();
        public void Load();
        public class Structure
        {
            public Dictionary<ulong, PlayerSettings> playerSettings;
            public class PlayerSettings
            {
                public string code;
                public string guestCode;
                public bool quietMode;
                public double lastSet;
                public int timesSetInSpamWindow;
                public double lockedOutUntil;
                public double lastLockedOut;
                public int lockedOutTimes;
            }

        }

    }

    private static class Permissions
    {
        public const string Use;
        public const string Try;
        public const string Admin;
        public static void Register(AutoCode plugin);
    }

    private class Commands
    {
        private readonly AutoCode plugin;
        public readonly Command Rust;
        public string ResetLockOut;
        public string Use;
        public string Guest;
        public string PickCode;
        public string RandomCode;
        public string RemoveCode;
        public string SetCode;
        public string QuietMode;
        public string Help;
        public Commands(AutoCode plugin);
        public void Register();
        private bool HandleResetLockOut(ConsoleSystem.Arg arg);
        private void HandleUse(BasePlayer player, string label, string[] args);
        private void ShowInfo(BasePlayer player, string label, string[] args);
        private void SyntaxError(BasePlayer player, string label, string[] args);
        public string GetHelp(BasePlayer player, string label);
        private string GetUsage(string label);
        public string GetHelpExtended(BasePlayer player, string label);
    }

    private static class Utils
    {
        public static double CurrentTime();
        public static bool ShouldHideCode(BasePlayer player, Data.Structure.PlayerSettings settings);
    }

    private static class Formatter
    {
        public const string SmallestLineGap;
        public const string SmallLineGap;
        public static string H1(string text);
        public static string H2(string text);
        public static string Small(string text);
        public static string UL(string[] items);
        public static string Indent(string text);
        public static string Command(string text);
        public static string Value(string text);
        public static string NoValue(string text);
    }

    private class TempCodeLockInfo
    {
        public readonly CodeLock CodeLock;
        public readonly bool Guest;
        public TempCodeLockInfo(CodeLock CodeLock, bool Guest);
    }

}

private class AutoCodeConfig
{
    private readonly AutoCode plugin;
    public readonly DynamicConfigFile OxideConfig;
    private bool UnsavedChanges;
    public AutoCodeConfig();
    public AutoCodeConfig(AutoCode plugin);
    public CommandsDef Commands;
    public class CommandsDef
    {
        public string Use;
        public ulong ChatIconId;
    }

    public OptionsDef Options;
    public class OptionsDef
    {
        public bool DisplayPermissionErrors;
        public SpamPreventionDef SpamPrevention;
        public PluginIntegrationDef PluginIntegration;
        public class SpamPreventionDef
        {
            public bool Enabled;
            public int Attempts;
            public double LockOutTime;
            public double WindowTime;
            public bool UseExponentialLockOutTime;
            public double LockOutResetFactor;
        }

        public class PluginIntegrationDef
        {
            public NoEscapeDef NoEscape;
            public class NoEscapeDef
            {
                public bool BlockRaid;
                public bool BlockCombat;
            }

        }

    }

    public void Save(bool force);
    public void Load();
    private T GetConfigValue(string[] settingPath, T defaultValue, bool deprecated);
    private void SetConfigValue(string[] settingPath, T newValue);
    private void RemoveConfigValue(string[] settingPath);
}

public class CommandsDef
{
    public string Use;
    public ulong ChatIconId;
}

public class OptionsDef
{
    public bool DisplayPermissionErrors;
    public SpamPreventionDef SpamPrevention;
    public PluginIntegrationDef PluginIntegration;
    public class SpamPreventionDef
    {
        public bool Enabled;
        public int Attempts;
        public double LockOutTime;
        public double WindowTime;
        public bool UseExponentialLockOutTime;
        public double LockOutResetFactor;
    }

    public class PluginIntegrationDef
    {
        public NoEscapeDef NoEscape;
        public class NoEscapeDef
        {
            public bool BlockRaid;
            public bool BlockCombat;
        }

    }

}

public class SpamPreventionDef
{
    public bool Enabled;
    public int Attempts;
    public double LockOutTime;
    public double WindowTime;
    public bool UseExponentialLockOutTime;
    public double LockOutResetFactor;
}

public class PluginIntegrationDef
{
    public NoEscapeDef NoEscape;
    public class NoEscapeDef
    {
        public bool BlockRaid;
        public bool BlockCombat;
    }

}

public class NoEscapeDef
{
    public bool BlockRaid;
    public bool BlockCombat;
}

private class Data
{
    private readonly string Filename;
    public Structure Inst { get; set; }
    public Data(AutoCode plugin);
    public void Save();
    public void Load();
    public class Structure
    {
        public Dictionary<ulong, PlayerSettings> playerSettings;
        public class PlayerSettings
        {
            public string code;
            public string guestCode;
            public bool quietMode;
            public double lastSet;
            public int timesSetInSpamWindow;
            public double lockedOutUntil;
            public double lastLockedOut;
            public int lockedOutTimes;
        }

    }

}

public class Structure
{
    public Dictionary<ulong, PlayerSettings> playerSettings;
    public class PlayerSettings
    {
        public string code;
        public string guestCode;
        public bool quietMode;
        public double lastSet;
        public int timesSetInSpamWindow;
        public double lockedOutUntil;
        public double lastLockedOut;
        public int lockedOutTimes;
    }

}

public class PlayerSettings
{
    public string code;
    public string guestCode;
    public bool quietMode;
    public double lastSet;
    public int timesSetInSpamWindow;
    public double lockedOutUntil;
    public double lastLockedOut;
    public int lockedOutTimes;
}

private static class Permissions
{
    public const string Use;
    public const string Try;
    public const string Admin;
    public static void Register(AutoCode plugin);
}

private class Commands
{
    private readonly AutoCode plugin;
    public readonly Command Rust;
    public string ResetLockOut;
    public string Use;
    public string Guest;
    public string PickCode;
    public string RandomCode;
    public string RemoveCode;
    public string SetCode;
    public string QuietMode;
    public string Help;
    public Commands(AutoCode plugin);
    public void Register();
    private bool HandleResetLockOut(ConsoleSystem.Arg arg);
    private void HandleUse(BasePlayer player, string label, string[] args);
    private void ShowInfo(BasePlayer player, string label, string[] args);
    private void SyntaxError(BasePlayer player, string label, string[] args);
    public string GetHelp(BasePlayer player, string label);
    private string GetUsage(string label);
    public string GetHelpExtended(BasePlayer player, string label);
}

private static class Utils
{
    public static double CurrentTime();
    public static bool ShouldHideCode(BasePlayer player, Data.Structure.PlayerSettings settings);
}

private static class Formatter
{
    public const string SmallestLineGap;
    public const string SmallLineGap;
    public static string H1(string text);
    public static string H2(string text);
    public static string Small(string text);
    public static string UL(string[] items);
    public static string Indent(string text);
    public static string Command(string text);
    public static string Value(string text);
    public static string NoValue(string text);
}

private class TempCodeLockInfo
{
    public readonly CodeLock CodeLock;
    public readonly bool Guest;
    public TempCodeLockInfo(CodeLock CodeLock, bool Guest);
}


```

---

## AutoCommands by MrBlue - Automatically runs configured commands on player and server events

```csharp
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Auto Commands", "Wulf", "2.0.1")]
[Description("Automatically runs configured commands on player and server events")]
public class AutoCommands : CovalencePlugin
{
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Run commands on player connect")]
        public bool RunCommandsOnConnect;
        [JsonProperty("Commands on connect", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> ConnectCommands;
        [JsonProperty("Run commands on player disconnect")]
        public bool RunCommandsOnDisconnect;
        [JsonProperty("Commands on disconnect", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> DisconnectCommands;
        [JsonProperty("Run commands on server startup")]
        public bool RunCommandsOnStartup;
        [JsonProperty("Commands on server startup", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> StartupCommands;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void OnServerInitialized();
    private void ProcessCommand(string command, IPlayer player);
    private string ReplacePlaceholders(string command, IPlayer player);
}

public class Configuration
{
    [JsonProperty("Run commands on player connect")]
    public bool RunCommandsOnConnect;
    [JsonProperty("Commands on connect", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> ConnectCommands;
    [JsonProperty("Run commands on player disconnect")]
    public bool RunCommandsOnDisconnect;
    [JsonProperty("Commands on disconnect", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> DisconnectCommands;
    [JsonProperty("Run commands on server startup")]
    public bool RunCommandsOnStartup;
    [JsonProperty("Commands on server startup", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> StartupCommands;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## AutoDecay by  - Auto damage to objects, that are not in building zone

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Auto Decay", "Hougan/Arainrr", "1.3.1")]
[Description("Auto damage to objects, that are not in building zone")]
public class AutoDecay : RustPlugin
{
    private const string PERMISSION_IGNORE;
    private readonly Hash<ulong, float> notifyPlayers;
    private readonly Dictionary<ulong, DecayController> decayControllers;
    private readonly List<string> defaultDisabled;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(BaseCombatEntity baseCombatEntity);
    private void OnEntityKill(BaseCombatEntity baseCombatEntity);
    private void OnStructureUpgrade(BuildingBlock buildingBlock, BasePlayer player, BuildingGrade.Enum newGrade);
    private void HandleCupboard(BuildingPrivlidge buildingPrivlidge, bool spawned);
    private void UpdateConfig(bool newConfig);
    private void UpdateData(bool updateConfig, bool newConfig);
    private void CreateDecayController(BaseCombatEntity baseCombatEntity, BasePlayer player, bool init);
    private void SendMessage(BasePlayer player, float time);
    private static DecaySettings GetDecayEntitySettings(BaseCombatEntity baseCombatEntity);
    private static DecaySettings GetBuildingBlockSettings(BuildingBlock buildingBlock);
    private static bool IsDecayEnabled(DecaySettings decaySettings, BaseEntity entity);
    private class DecayController
    {
        private BaseCombatEntity entity;
        private DecaySettings decaySettings;
        private State state;
        private float tickDamage;
        private bool isCupboard;
        public DecayController(BaseCombatEntity entity, DecaySettings decaySettings, bool init);
        private void CheckBuildingPrivilege();
        private bool HasBuildingPrivilege();
        private bool OnFoundation();
        private void StartDelay();
        private void StartDamage();
        private void StopDamage();
        private void ResetDamage();
        private void DoDamage();
        public void OnBuildingUpgrade(DecaySettings settings);
        public void OnCupboardPlaced();
        public void OnCupboardDestroyed();
        public void Destroy();
    }

    private static ConfigData _config;
    private class ConfigData
    {
        [JsonProperty("Check Cupboard Interval (Seconds)")]
        public float cupboardCheckTime;
        [JsonProperty("Not Protected Cupboard = No Cupboard")]
        public bool checkEmptyCupboard;
        [JsonProperty("Notify Player That His Object Will Be Removed")]
        public bool notifyPlayer;
        [JsonProperty("Notify Interval")]
        public float notifyInterval;
        [JsonProperty("Chat Settings")]
        public ChatSettings chatS;
        [JsonProperty("Building Block Settings")]
        public Dictionary<string, Dictionary<BuildingGrade.Enum, DecaySettings>> buildingBlockS;
        [JsonProperty("Other Entity Settings")]
        public Dictionary<string, DecaySettings> entityS;
        [JsonProperty("Version")]
        public VersionNumber version;
    }

    public class ChatSettings
    {
        [JsonProperty("Chat Prefix")]
        public string prefix;
        [JsonProperty("Chat SteamID Icon")]
        public ulong steamIDIcon;
    }

    private class DecaySettings
    {
        [JsonProperty("Enabled")]
        public bool enabled;
        [JsonProperty("Only Used For Player's Entity")]
        public bool onlyOwned;
        [JsonProperty("Delay Time (Seconds)")]
        public float delayTime;
        [JsonProperty("Destroy Time (Seconds)")]
        public float destroyTime;
        [JsonProperty("Tick Rate (Damage Per Tick = Max Health / This)")]
        public float tickRate;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private StoredData storedData;
    private class StoredData
    {
        [JsonProperty("List of short prefab names for all combat entities")]
        public HashSet<string> entityShortPrefabNames;
    }

    private void LoadData();
    private void SaveData();
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class DecayController
{
    private BaseCombatEntity entity;
    private DecaySettings decaySettings;
    private State state;
    private float tickDamage;
    private bool isCupboard;
    public DecayController(BaseCombatEntity entity, DecaySettings decaySettings, bool init);
    private void CheckBuildingPrivilege();
    private bool HasBuildingPrivilege();
    private bool OnFoundation();
    private void StartDelay();
    private void StartDamage();
    private void StopDamage();
    private void ResetDamage();
    private void DoDamage();
    public void OnBuildingUpgrade(DecaySettings settings);
    public void OnCupboardPlaced();
    public void OnCupboardDestroyed();
    public void Destroy();
}

private class ConfigData
{
    [JsonProperty("Check Cupboard Interval (Seconds)")]
    public float cupboardCheckTime;
    [JsonProperty("Not Protected Cupboard = No Cupboard")]
    public bool checkEmptyCupboard;
    [JsonProperty("Notify Player That His Object Will Be Removed")]
    public bool notifyPlayer;
    [JsonProperty("Notify Interval")]
    public float notifyInterval;
    [JsonProperty("Chat Settings")]
    public ChatSettings chatS;
    [JsonProperty("Building Block Settings")]
    public Dictionary<string, Dictionary<BuildingGrade.Enum, DecaySettings>> buildingBlockS;
    [JsonProperty("Other Entity Settings")]
    public Dictionary<string, DecaySettings> entityS;
    [JsonProperty("Version")]
    public VersionNumber version;
}

public class ChatSettings
{
    [JsonProperty("Chat Prefix")]
    public string prefix;
    [JsonProperty("Chat SteamID Icon")]
    public ulong steamIDIcon;
}

private class DecaySettings
{
    [JsonProperty("Enabled")]
    public bool enabled;
    [JsonProperty("Only Used For Player's Entity")]
    public bool onlyOwned;
    [JsonProperty("Delay Time (Seconds)")]
    public float delayTime;
    [JsonProperty("Destroy Time (Seconds)")]
    public float destroyTime;
    [JsonProperty("Tick Rate (Damage Per Tick = Max Health / This)")]
    public float tickRate;
}

private class StoredData
{
    [JsonProperty("List of short prefab names for all combat entities")]
    public HashSet<string> entityShortPrefabNames;
}


```

---

## AutoDemoRecordLite by Pho3niX90 - Records demos automatically of players that have been reported/F7 X amount of times

```csharp
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using WebSocketSharp;

Oxide.Plugins
[Info("Auto Demo Record Lite", "Pho3niX90", "1.0.82")]
[Description("Automatic recording based on conditions.")]
internal class AutoDemoRecordLite : RustPlugin
{
    private List<ARReport> _reports;
    private Dictionary<ulong, Timer> _timers;
    private ARConfig config;
     int lastSavedCount;
    [PluginReference]
     Plugin DiscordApi;
     Plugin DiscordMessages;
    private void Loaded();
    private void Unload();
    protected override void LoadDefaultMessages();
     string GetMsg(string key);
     void OnPlayerReported(BasePlayer reporter, string targetName, string targetId, string subject, string message, string type);
     void OnPlayerCommand(BasePlayer player, string command, string[] args);
     void ReportCommand(IPlayer reporter, IPlayer target, string reason);
    private void OnDestroy();
     void ProcessF7(ulong targetId, ARReport report);
     void StartRecording(BasePlayer player, ARReport report);
     void StopRecording(BasePlayer player, ARReport report);
     int CheckReports(BasePlayer player);
     int secondsAgo(DateTime timeFrom);
     void NotifyDiscord(BasePlayer player, string msg, ARReport report, bool isStart);
    private class ARConfig
    {
        public int AR_Report;
        public int AR_Report_Length;
        public bool AR_Clear_Counter;
        public string AR_Discord_Webhook;
        public int AR_Discord_Color;
        public bool AR_Discord_Notify_RecordStart;
        public bool AR_Discord_Notify_RecordStop;
        public int AR_Report_Seconds;
        public bool AR_Discord_Notify_RecordStartMsg;
        public bool AR_Discord_Notify_RecordStopMsg;
        public bool AR_Save_Reports;
        private AutoDemoRecordLite plugin;
        public ARConfig(AutoDemoRecordLite plugin);
        private void GetConfig(T variable, string[] path);
        private void SetConfig(T variable, string[] path);
    }

    protected override void LoadConfig();
     void LoadData();
     void SaveData();
    protected override void LoadDefaultConfig();
    public class EmbedFieldList
    {
        public string name { get; set; }
        public string value { get; set; }
        public bool inline { get; set; }
    }

    public class ARReport
    {
        public string reporterName;
        public string reporterId;
        public string targetName;
        public string targetId;
        public string subject;
        public string message;
        public string type;
        public DateTime created;
        public ARReport();
        public ARReport(string reporterId, string reporterName, string targetId, string targetName, string subject, string message, string type);
        public ARReport(string targetId, string targetName);
    }

}

private class ARConfig
{
    public int AR_Report;
    public int AR_Report_Length;
    public bool AR_Clear_Counter;
    public string AR_Discord_Webhook;
    public int AR_Discord_Color;
    public bool AR_Discord_Notify_RecordStart;
    public bool AR_Discord_Notify_RecordStop;
    public int AR_Report_Seconds;
    public bool AR_Discord_Notify_RecordStartMsg;
    public bool AR_Discord_Notify_RecordStopMsg;
    public bool AR_Save_Reports;
    private AutoDemoRecordLite plugin;
    public ARConfig(AutoDemoRecordLite plugin);
    private void GetConfig(T variable, string[] path);
    private void SetConfig(T variable, string[] path);
}

public class EmbedFieldList
{
    public string name { get; set; }
    public string value { get; set; }
    public bool inline { get; set; }
}

public class ARReport
{
    public string reporterName;
    public string reporterId;
    public string targetName;
    public string targetId;
    public string subject;
    public string message;
    public string type;
    public DateTime created;
    public ARReport();
    public ARReport(string reporterId, string reporterName, string targetId, string targetName, string subject, string message, string type);
    public ARReport(string targetId, string targetName);
}


```

---

## AutoDeposit by dcheal - Very lightweight plugin that allows quick depositing of items to matching containers.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Auto Deposit", "DC", "1.0.1")]
[Description("Simply adds items to containers if that container already has that item!")]
 class AutoDeposit : RustPlugin
{
     List<BasePlayer> playersSort;
    private void CommandDeposit(BasePlayer player, string commands, string[] args);
    protected override void LoadDefaultMessages();
    private void OnServerInitialized();
    private void OnLootEntity(BasePlayer player, StorageContainer entity);
    private List<Item> CompareItemContainers(ItemContainer fContainer, ItemContainer sContainer);
    private Configuration config;
    protected override void LoadConfig();
    public class Configuration
    {
        [JsonProperty(PropertyName = "Allowed Containers")]
        public string[] allowedContainers;
        [JsonProperty(PropertyName = "Disallowed Items")]
        public string[] disallowedItemNames;
        [JsonProperty(PropertyName = "Use Timer")]
        public bool useTimer;
        [JsonProperty(PropertyName = "Autodisable Time")]
        public float autoDisableTime;
        [JsonProperty(PropertyName = "Deposit Command")]
        public string depositCommand;
    }

    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
}

public class Configuration
{
    [JsonProperty(PropertyName = "Allowed Containers")]
    public string[] allowedContainers;
    [JsonProperty(PropertyName = "Disallowed Items")]
    public string[] disallowedItemNames;
    [JsonProperty(PropertyName = "Use Timer")]
    public bool useTimer;
    [JsonProperty(PropertyName = "Autodisable Time")]
    public float autoDisableTime;
    [JsonProperty(PropertyName = "Deposit Command")]
    public string depositCommand;
}


```

---

## AutoDoors by bushhy - Automatically closes doors behind players after X seconds

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Auto Doors", "Wulf/lukespragg/Arainrr/James/Bushhy", "3.3.10", ResourceId = 1924)]
[Description("Automatically closes doors behind players after X seconds")]
public class AutoDoors : RustPlugin
{
    [PluginReference]
    private Plugin RustTranslationAPI;
    private const string PERMISSION_USE;
    private readonly Hash<ulong, Timer> doorTimers;
    private readonly Dictionary<string, string> supportedDoors;
    private HashSet<DoorManipulator> doorManipulators;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(DoorManipulator doorManipulator);
    private void OnEntityKill(DoorManipulator doorManipulator);
    private void OnEntityKill(Door door);
    private void OnServerSave();
    private void Unload();
    private void OnDoorOpened(Door door, BasePlayer player);
    private void OnDoorClosed(Door door, BasePlayer player);
    private bool HasDoorController(Door door);
    private StoredData.PlayerData GetPlayerData(ulong playerID, bool readOnly);
    private static Door GetLookingAtDoor(BasePlayer player);
    private void UpdateConfig();
    private string GetDeployableTranslation(string language, string deployable);
    private string GetDeployableDisplayName(BasePlayer player, string deployable, string displayName);
    private void CmdAutoDoor(BasePlayer player, string command, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Use permissions")]
        public bool usePermission;
        [JsonProperty(PropertyName = "Clear data on map wipe")]
        public bool clearDataOnWipe;
        [JsonProperty(PropertyName = "Global settings")]
        public GlobalSettings globalS;
        [JsonProperty(PropertyName = "Chat settings")]
        public ChatSettings chatS;
        [JsonProperty(PropertyName = "Door Settings")]
        public Dictionary<string, DoorSettings> doorS;
        public class DoorSettings
        {
            public bool enabled;
            public string displayName;
        }

        public class GlobalSettings
        {
            [JsonProperty(PropertyName = "Allows automatic closing of unowned doors")]
            public bool useUnownedDoor;
            [JsonProperty(PropertyName = "Exclude door controller")]
            public bool excludeDoorController;
            [JsonProperty(PropertyName = "Cancel on player dead")]
            public bool cancelOnKill;
            [JsonProperty(PropertyName = "Default enabled")]
            public bool defaultEnabled;
            [JsonProperty(PropertyName = "Default delay")]
            public float defaultDelay;
            [JsonProperty(PropertyName = "Maximum delay")]
            public float maximumDelay;
            [JsonProperty(PropertyName = "Minimum delay")]
            public float minimumDelay;
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
        public readonly Dictionary<ulong, PlayerData> playerData;
        public class PlayerData
        {
            public DoorData doorData;
            public readonly Dictionary<ulong, DoorData> theDoorS;
            public readonly Dictionary<string, DoorData> doorTypeS;
        }

        public class DoorData
        {
            public bool enabled;
            public float time;
        }

    }

    private void LoadData();
    private void SaveData();
    private void ClearData();
    private void OnNewSave(string filename);
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Use permissions")]
    public bool usePermission;
    [JsonProperty(PropertyName = "Clear data on map wipe")]
    public bool clearDataOnWipe;
    [JsonProperty(PropertyName = "Global settings")]
    public GlobalSettings globalS;
    [JsonProperty(PropertyName = "Chat settings")]
    public ChatSettings chatS;
    [JsonProperty(PropertyName = "Door Settings")]
    public Dictionary<string, DoorSettings> doorS;
    public class DoorSettings
    {
        public bool enabled;
        public string displayName;
    }

    public class GlobalSettings
    {
        [JsonProperty(PropertyName = "Allows automatic closing of unowned doors")]
        public bool useUnownedDoor;
        [JsonProperty(PropertyName = "Exclude door controller")]
        public bool excludeDoorController;
        [JsonProperty(PropertyName = "Cancel on player dead")]
        public bool cancelOnKill;
        [JsonProperty(PropertyName = "Default enabled")]
        public bool defaultEnabled;
        [JsonProperty(PropertyName = "Default delay")]
        public float defaultDelay;
        [JsonProperty(PropertyName = "Maximum delay")]
        public float maximumDelay;
        [JsonProperty(PropertyName = "Minimum delay")]
        public float minimumDelay;
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

    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version;
}

public class DoorSettings
{
    public bool enabled;
    public string displayName;
}

public class GlobalSettings
{
    [JsonProperty(PropertyName = "Allows automatic closing of unowned doors")]
    public bool useUnownedDoor;
    [JsonProperty(PropertyName = "Exclude door controller")]
    public bool excludeDoorController;
    [JsonProperty(PropertyName = "Cancel on player dead")]
    public bool cancelOnKill;
    [JsonProperty(PropertyName = "Default enabled")]
    public bool defaultEnabled;
    [JsonProperty(PropertyName = "Default delay")]
    public float defaultDelay;
    [JsonProperty(PropertyName = "Maximum delay")]
    public float maximumDelay;
    [JsonProperty(PropertyName = "Minimum delay")]
    public float minimumDelay;
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

private class StoredData
{
    public readonly Dictionary<ulong, PlayerData> playerData;
    public class PlayerData
    {
        public DoorData doorData;
        public readonly Dictionary<ulong, DoorData> theDoorS;
        public readonly Dictionary<string, DoorData> doorTypeS;
    }

    public class DoorData
    {
        public bool enabled;
        public float time;
    }

}

public class PlayerData
{
    public DoorData doorData;
    public readonly Dictionary<ulong, DoorData> theDoorS;
    public readonly Dictionary<string, DoorData> doorTypeS;
}

public class DoorData
{
    public bool enabled;
    public float time;
}


```

---

## AutoEngineParts by WhiteThunder - Automatically fills modular car engines with engine parts which players cannot remove

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust.Modular;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Auto Engine Parts", "WhiteThunder", "1.0.0")]
[Description("Ensures modular car engines always have engine parts which players cannot remove.")]
internal class AutoEngineParts : CovalencePlugin
{
    [PluginReference]
    private Plugin EnginePartsDurability;
    private const string PermissionTier1;
    private const string PermissionTier2;
    private const string PermissionTier3;
    private const float VanillaInternalDamageMultiplier;
    private Configuration _pluginConfig;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private object OnVehicleModuleMove(VehicleModuleEngine module);
    private void OnEntityKill(VehicleModuleEngine module);
    private void OnEntitySpawned(VehicleModuleEngine module);
    private object OnItemLock(Item item);
    private object OnEngineDamageMultiplierChange(EngineStorage engineStorage, float desiredMultiplier);
    private void OnVehicleOwnershipChanged(ModularCar car);
    private static bool UpdateEngineStorageWasBlocked(EngineStorage engineStorage, int tier);
    private static ulong GetCarOwnerId(BaseVehicleModule module);
    private static EngineStorage GetEngineStorage(BaseVehicleModule module);
    private static bool IsLocked(EngineStorage engineStorage);
    private static void RemoveAllEngineParts(EngineStorage engineStorage);
    private static bool TryAddEngineItem(EngineStorage engineStorage, int slot, int tier);
    private static int Clamp(int x, int min, int max);
    private void MaybeUpdateEngineModule(BaseVehicleModule module, bool dropExistingParts);
    private void MaybeUpdateEngineStorage(EngineStorage engineStorage, int enginePartsTier, bool dropExistingParts);
    private void UpdateEngineStorage(EngineStorage engineStorage, int desiredTier, bool dropExistingParts);
    private void ResetInternalDamageMultiplier(EngineStorage engineStorage);
    private int GetOwnerEnginePartsTier(ulong ownerId);
    private class Configuration : SerializableConfiguration
    {
        private int _defaultEnginePartsTier;
        [JsonProperty("DefaultEnginePartsTier")]
        public int DefaultEnginePartsTier { get; set; }
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
}

private class Configuration : SerializableConfiguration
{
    private int _defaultEnginePartsTier;
    [JsonProperty("DefaultEnginePartsTier")]
    public int DefaultEnginePartsTier { get; set; }
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

## AutoFlipDrones by WhiteThunder - Automatically flips upside-down RC drones when hit with a hammer or when a player takes control

```csharp
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Auto Flip Drones", "WhiteThunder", "1.1.0")]
[Description("Automatically flips upside-down RC drones when hit with a hammer or taken control of at a computer station.")]
internal class AutoFlipDrones : CovalencePlugin
{
    [PluginReference]
    private Plugin DroneScaleManager;
    private const string PermissionUse;
    private void Init();
    private void OnBookmarkControlStarted(ComputerStation computerStation, BasePlayer player, string bookmarkName, Drone drone);
    private void OnHammerHit(BasePlayer player, HitInfo info);
    private bool AutoFlipWasBlocked(Drone drone, BasePlayer player);
    private BaseEntity GetRootEntity(Drone drone);
    private void MaybeFlipDrone(BasePlayer player, Drone drone);
}


```

---

## AutoFuel by 0x89A - Automatically fuels lights using fuel from the tool cupboard's inventory

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Auto Fuel", "0x89A", "2.1.1")]
[Description("Automatically fuels lights using fuel from the tool cupboard's inventory")]
 class AutoFuel : RustPlugin
{
    private Configuration _config;
    private const string _usePerm;
    private readonly Dictionary<ulong, BuildingPrivlidge> _cachedToolCupboards;
    private const int _woodItemId;
    private const int _lowGradeItemId;
    private void Init();
    private void OnItemUse(Item item, int amountToUse);
    private void OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
    private void OnOvenToggle(BaseOven oven, BasePlayer player);
    private void OnSwitchToggle(FuelGenerator generator, BasePlayer player);
    private void TryRefill(int itemToFind, BaseEntity ent, ItemContainer container, BuildingPrivlidge toolCupboard, int amount);
    private void OnToggle(BaseEntity entity, ItemContainer container, int itemid);
    private bool GetToolCupboard(BaseEntity entity, BuildingPrivlidge toolCupboard);
    private bool IsAllowed(BuildingPrivlidge privlidge, BaseEntity ent);
    private class Configuration
    {
        [JsonProperty("Allowed Entities")]
        public List<string> AllowedEntities;
        [JsonProperty("Check entity owner for permission")]
        public bool CheckEntityForPerm;
        [JsonProperty("Anyone on tool cupboard has permission")]
        public bool AnyoneOnTC;
        [JsonIgnore]
        public bool CheckForPerm { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class Configuration
{
    [JsonProperty("Allowed Entities")]
    public List<string> AllowedEntities;
    [JsonProperty("Check entity owner for permission")]
    public bool CheckEntityForPerm;
    [JsonProperty("Anyone on tool cupboard has permission")]
    public bool AnyoneOnTC;
    [JsonIgnore]
    public bool CheckForPerm { get; set; }
}


```

---

## AutoLightsOut by misticos - Turns all lights out at night then allows user to turn them back on again.

```csharp
using Oxide.Core;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("AutoLightsOut", "DylanSMR", "1.0.3")]
[Description("Turn off all lights at night, user must manually turn on again, this allows users on modded servers with lots of wood to find houses at night with players in them.")]
 class AutoLightsOut : RustPlugin
{
     TOD_Sky sky;
     Configuration config;
     bool Activated;
     List<uint> TurnedOff;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Trigger : 1 = Turn off all at the same time, 2 = Turn off each one when they burn a piece of fuel. (Default=1)")]
        public int Trigger;
        [JsonProperty(PropertyName = "Trigger Interval : How often the plugin checks if it is night, only change for trigger type one. (Default=60) (SECONDS)")]
        public int TriggerTime;
        [JsonProperty(PropertyName = "Camp Fires : If camp fires should be turned off. (Default=true)")]
        public bool CampFires;
        [JsonProperty(PropertyName = "Furnaces : If furnaces should be turned off. (Default=true)")]
        public bool Furnaces;
        [JsonProperty(PropertyName = "Lanterns : If lanterns should be turned off. (Default=true)")]
        public bool Lanterns;
    }

    protected override void LoadDefaultConfig();
     void SaveConfig(Configuration config);
    public void LoadConfigVars();
     void Loaded();
     void CheckOven(BaseOven oven);
     void OnConsumeFuel(BaseOven oven, Item fuel, ItemModBurnable burnable);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Trigger : 1 = Turn off all at the same time, 2 = Turn off each one when they burn a piece of fuel. (Default=1)")]
    public int Trigger;
    [JsonProperty(PropertyName = "Trigger Interval : How often the plugin checks if it is night, only change for trigger type one. (Default=60) (SECONDS)")]
    public int TriggerTime;
    [JsonProperty(PropertyName = "Camp Fires : If camp fires should be turned off. (Default=true)")]
    public bool CampFires;
    [JsonProperty(PropertyName = "Furnaces : If furnaces should be turned off. (Default=true)")]
    public bool Furnaces;
    [JsonProperty(PropertyName = "Lanterns : If lanterns should be turned off. (Default=true)")]
    public bool Lanterns;
}


```

---

## AutoLock by birthdates - Automatically adds a codelock to a lockable entity with a set pin

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using JetBrains.Annotations;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using Object = UnityEngine.Object;
using Random = Oxide.Core.Random;

Oxide.Plugins
[Info("Auto Lock", "birthdates", "2.4.5")]
[Description("Automatically adds a codelock to a lockable entity with a set pin")]
public class AutoLock : RustPlugin
{
    private const string PermissionUse;
    private const string PermissionItemBypass;
    private readonly Dictionary<BasePlayer, TimedCodeLock> _awaitingResponse;
    [UsedImplicitly]
    [PluginReference("NoEscape")]
    private Plugin _noEscape;
    [UsedImplicitly]
    private void Init();
    [UsedImplicitly]
    private void OnServerInitialized();
    [UsedImplicitly]
    private void OnEntityBuilt(HeldEntity plan, GameObject go);
    private static string GetRandomCode();
    [UsedImplicitly]
    private void OnServerShutdown();
    private void Unload();
    private PlayerData CreateDataIfAbsent(string id);
    private void ChatCommand(BasePlayer player, string label, string[] args);
    private static bool HasCodeLock(BasePlayer player);
    private static void TakeCodeLock(BasePlayer player);
    private void OpenCodeLockUI(BasePlayer player);
    [UsedImplicitly]
    private void OnCodeEntered(Object codeLock, BasePlayer player, string code);
    private bool Toggle(BasePlayer player);
    private ConfigFile _config;
    private Data _data;
    private class PlayerData
    {
        public string Code;
        public bool Enabled;
    }

    private class Data
    {
        public readonly Dictionary<string, PlayerData> Codes;
    }

    protected override void LoadDefaultMessages();
    public class ConfigFile
    {
        [JsonProperty("Code Lock Expiry Time (Seconds, put -1 if you want to disable)")]
        public float CodeLockExpiry;
        [JsonProperty("Disabled Items (Prefabs)")]
        public List<string> Disabled;
        [JsonProperty("No Escape")]
        public NoEscapeSettings NoEscapeSettings;
        public static ConfigFile DefaultConfig();
    }

    public class NoEscapeSettings
    {
        [JsonProperty("Block Auto Lock whilst in Combat?")]
        public bool BlockCombat;
        [JsonProperty("Block Auto Lock whilst Raid Blocked?")]
        public bool BlockRaid;
    }

    private void SaveData();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class PlayerData
{
    public string Code;
    public bool Enabled;
}

private class Data
{
    public readonly Dictionary<string, PlayerData> Codes;
}

public class ConfigFile
{
    [JsonProperty("Code Lock Expiry Time (Seconds, put -1 if you want to disable)")]
    public float CodeLockExpiry;
    [JsonProperty("Disabled Items (Prefabs)")]
    public List<string> Disabled;
    [JsonProperty("No Escape")]
    public NoEscapeSettings NoEscapeSettings;
    public static ConfigFile DefaultConfig();
}

public class NoEscapeSettings
{
    [JsonProperty("Block Auto Lock whilst in Combat?")]
    public bool BlockCombat;
    [JsonProperty("Block Auto Lock whilst Raid Blocked?")]
    public bool BlockRaid;
}


```

---

## AutomatedEvents by  - Runs various events (useful when using a night skip plugin)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Automated Events", "k1lly0u/mspeedie/Arainrr", "1.0.12")]
internal class AutomatedEvents : RustPlugin
{
    [PluginReference]
    private Plugin GUIAnnouncements;
    private const string PERMISSION_USE;
    private const string PERMISSION_NEXT;
    private const string PREFAB_APC;
    private const string PREFAB_PLANE;
    private const string PREFAB_CHINOOK;
    private const string PREFAB_HELI;
    private const string PREFAB_SHIP;
    private const string PREFAB_SLEIGH;
    private const string PREFAB_EASTER;
    private const string PREFAB_HALLOWEEN;
    private const string PREFAB_CHRISTMAS;
    private static AutomatedEvents instance;
    private Dictionary<BaseEntity, EventType> _eventEntities;
    private readonly Dictionary<EventType, Timer> _eventTimers;
    private readonly Dictionary<EventSchedule, EventType> _disabledVanillaEvents;
    private readonly Dictionary<string, EventType> _eventSchedulePrefabShortNames;
    private void Init();
    private void OnServerInitialized(bool initial);
    private void InitializeEvents();
    private void Unload();
    private void OnEntityKill(BaseEntity entity);
    private object OnEventTrigger(TriggeredEventPrefab eventPrefab);
    private void ClearExistingEvents();
    private void StartEventTimer(EventType eventType, bool announce, float timeOverride, bool onKill);
    private void RunEvent(EventType eventType, bool runOnce, bool bypass);
    private void KillEvent(EventType eventType);
    private bool GetNextEventRunTime(IPlayer iPlayer, EventType eventType, string nextTime);
    private BaseEvent GetBaseEvent(EventType eventType);
    private string GetEventTypeDisplayName(EventType eventType);
    private void SendEventNextRunMessage(EventType eventType, string timeLeft);
    private void SendEventTriggeredMessage(string eventTypeStr);
    private void CmdNextEvent(IPlayer iPlayer, string command, string[] args);
    private void CmdRunEvent(IPlayer iPlayer, string command, string[] args);
    private void CmdKillEvent(IPlayer iPlayer, string command, string[] args);
    private static string GetPrefabShortName(string prefabName);
    private static EventType GetEventTypeFromStr(string eventTypeStr);
    private static EventType GetEventTypeFromEntity(BaseEntity baseEntity);
    private static int GetEventIndexFromWeight(Dictionary<int, float> weightDict);
    private static IEnumerable<T> GetEventEntities(BaseEvent baseEvent, Func<T, bool> filter);
    private static bool CanRunEvent(EventType eventType, BaseEvent baseEvent, bool vanilla, Func<T, bool> filter);
    private static bool CheckOnlinePlayers(EventType eventType, BaseEvent baseEvent, bool vanilla);
    private static bool CanRunCoexistEvent(EventType eventType, BaseEvent baseEvent, bool vanilla, Func<T, bool> filter);
    private static bool CanRunHuntEvent(EventType eventType, BaseEvent baseEvent, bool vanilla);
    private void PrintDebug(string message, bool warning);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings Global { get; set; }
        public class Settings
        {
            [JsonProperty(PropertyName = "Enable Debug Mode")]
            public bool DebugEnabled { get; set; }
            [JsonProperty(PropertyName = "Announce On Plugin Loaded")]
            public bool AnnounceOnLoaded { get; set; }
            [JsonProperty(PropertyName = "Announce On Event Triggered")]
            public bool AnnounceEventTriggered { get; set; }
            [JsonProperty(PropertyName = "Use GUIAnnouncements Plugin")]
            public bool UseGuiAnnouncements { get; set; }
        }

        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings Chat { get; set; }
        public class ChatSettings
        {
            [JsonProperty(PropertyName = "Next Event Command")]
            public string NextEventCommand { get; set; }
            [JsonProperty(PropertyName = "Run Event Command")]
            public string RunEventCommand { get; set; }
            [JsonProperty(PropertyName = "Kill Event Command")]
            public string KillEventCommand { get; set; }
            [JsonProperty(PropertyName = "Chat Prefix")]
            public string Prefix { get; set; }
            [JsonProperty(PropertyName = "Chat Prefix Color")]
            public string PrefixColor { get; set; }
            [JsonProperty(PropertyName = "Chat SteamID Icon")]
            public ulong SteamIdIcon { get; set; }
        }

        [JsonProperty(PropertyName = "Event Settings")]
        public EventSettings Events { get; set; }
        public class EventSettings
        {
            [JsonProperty(PropertyName = "Bradley Event")]
            public CoexistEvent Bradley { get; set; }
            [JsonProperty(PropertyName = "Cargo Plane Event")]
            public CoexistEvent Plane { get; set; }
            [JsonProperty(PropertyName = "Cargo Ship Event")]
            public CoexistEvent Ship { get; set; }
            [JsonProperty(PropertyName = "Chinook (CH47) Event")]
            public CoexistEvent Chinook { get; set; }
            [JsonProperty(PropertyName = "Helicopter Event")]
            public CoexistEvent Helicopter { get; set; }
            [JsonProperty(PropertyName = "Santa Sleigh Event")]
            public CoexistEvent SantaSleigh { get; set; }
            [JsonProperty(PropertyName = "Christmas Event")]
            public CoexistEvent Christmas { get; set; }
            [JsonProperty(PropertyName = "Easter Event")]
            public BaseEvent Easter { get; set; }
            [JsonProperty(PropertyName = "Halloween Event")]
            public BaseEvent Halloween { get; set; }
        }

    }

    private class BaseEvent
    {
        [JsonProperty(PropertyName = "Enabled", Order = 1)]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Display Name", Order = 2)]
        public string DisplayName { get; set; }
        [JsonProperty(PropertyName = "Disable Vanilla Event", Order = 3)]
        public bool DisableVanillaEvent { get; set; }
        [JsonProperty(PropertyName = "Event Start Offset (Minutes)", Order = 4)]
        public float StartOffset { get; set; }
        [JsonProperty(PropertyName = "Minimum Time Between (Minutes)", Order = 5)]
        public float MinimumTimeBetween { get; set; }
        [JsonProperty(PropertyName = "Maximum Time Between (Minutes)", Order = 6)]
        public float MaximumTimeBetween { get; set; }
        [JsonProperty(PropertyName = "Minimum Online Players Required (0 = Disabled)", Order = 7)]
        public int MinimumOnlinePlayers { get; set; }
        [JsonProperty(PropertyName = "Maximum Online Players Required (0 = Disabled)", Order = 8)]
        public int MaximumOnlinePlayers { get; set; }
        [JsonProperty(PropertyName = "Announce Next Run Time", Order = 9)]
        public bool AnnounceNext { get; set; }
        [JsonProperty(PropertyName = "Restart Timer On Entity Kill", Order = 10)]
        public bool RestartTimerOnKill { get; set; }
        [JsonProperty(PropertyName = "Kill Existing Event On Plugin Loaded", Order = 11)]
        public bool KillEventOnLoaded { get; set; }
        [JsonIgnore]
        public double NextRunTime { get; set; }
        public virtual EventWeight GetRandomEventWeight();
    }

    private class CoexistEvent : BaseEvent
    {
        [JsonProperty(PropertyName = "Maximum Number On Server", Order = 19)]
        public int ServerMaximumNumber { get; set; }
        [JsonProperty(PropertyName = "Exclude Player's Entity", Order = 20)]
        public bool ExcludePlayerEntity { get; set; }
        [JsonProperty(PropertyName = "Event Weights", Order = 21, ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<EventWeight> EventWeights { get; set; }
        [JsonIgnore]
        private readonly List<EventWeight> _validEventWeights;
        public override EventWeight GetRandomEventWeight();
    }

    private class EventWeight
    {
        [JsonProperty(PropertyName = "Weight")]
        public int Weight { get; set; }
        [JsonProperty(PropertyName = "Name")]
        public string Name { get; set; }
        [JsonConverter(typeof(StringEnumConverter))]
        [JsonProperty(PropertyName = "Argument Type (Command or CallHook)")]
        public ArgumentType? ArgType { get; set; }
        [JsonProperty(PropertyName = "Arguments")]
        public List<string> Args { get; set; }
        [JsonIgnore]
        public bool IsNormalEvent { get; set; }
        public bool IsValid();
        public void RunCustomEvent();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void Print(BasePlayer player, string message);
    private void Print(IPlayer iPlayer, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings Global { get; set; }
    public class Settings
    {
        [JsonProperty(PropertyName = "Enable Debug Mode")]
        public bool DebugEnabled { get; set; }
        [JsonProperty(PropertyName = "Announce On Plugin Loaded")]
        public bool AnnounceOnLoaded { get; set; }
        [JsonProperty(PropertyName = "Announce On Event Triggered")]
        public bool AnnounceEventTriggered { get; set; }
        [JsonProperty(PropertyName = "Use GUIAnnouncements Plugin")]
        public bool UseGuiAnnouncements { get; set; }
    }

    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings Chat { get; set; }
    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Next Event Command")]
        public string NextEventCommand { get; set; }
        [JsonProperty(PropertyName = "Run Event Command")]
        public string RunEventCommand { get; set; }
        [JsonProperty(PropertyName = "Kill Event Command")]
        public string KillEventCommand { get; set; }
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Chat Prefix Color")]
        public string PrefixColor { get; set; }
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong SteamIdIcon { get; set; }
    }

    [JsonProperty(PropertyName = "Event Settings")]
    public EventSettings Events { get; set; }
    public class EventSettings
    {
        [JsonProperty(PropertyName = "Bradley Event")]
        public CoexistEvent Bradley { get; set; }
        [JsonProperty(PropertyName = "Cargo Plane Event")]
        public CoexistEvent Plane { get; set; }
        [JsonProperty(PropertyName = "Cargo Ship Event")]
        public CoexistEvent Ship { get; set; }
        [JsonProperty(PropertyName = "Chinook (CH47) Event")]
        public CoexistEvent Chinook { get; set; }
        [JsonProperty(PropertyName = "Helicopter Event")]
        public CoexistEvent Helicopter { get; set; }
        [JsonProperty(PropertyName = "Santa Sleigh Event")]
        public CoexistEvent SantaSleigh { get; set; }
        [JsonProperty(PropertyName = "Christmas Event")]
        public CoexistEvent Christmas { get; set; }
        [JsonProperty(PropertyName = "Easter Event")]
        public BaseEvent Easter { get; set; }
        [JsonProperty(PropertyName = "Halloween Event")]
        public BaseEvent Halloween { get; set; }
    }

}

public class Settings
{
    [JsonProperty(PropertyName = "Enable Debug Mode")]
    public bool DebugEnabled { get; set; }
    [JsonProperty(PropertyName = "Announce On Plugin Loaded")]
    public bool AnnounceOnLoaded { get; set; }
    [JsonProperty(PropertyName = "Announce On Event Triggered")]
    public bool AnnounceEventTriggered { get; set; }
    [JsonProperty(PropertyName = "Use GUIAnnouncements Plugin")]
    public bool UseGuiAnnouncements { get; set; }
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Next Event Command")]
    public string NextEventCommand { get; set; }
    [JsonProperty(PropertyName = "Run Event Command")]
    public string RunEventCommand { get; set; }
    [JsonProperty(PropertyName = "Kill Event Command")]
    public string KillEventCommand { get; set; }
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Chat Prefix Color")]
    public string PrefixColor { get; set; }
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong SteamIdIcon { get; set; }
}

public class EventSettings
{
    [JsonProperty(PropertyName = "Bradley Event")]
    public CoexistEvent Bradley { get; set; }
    [JsonProperty(PropertyName = "Cargo Plane Event")]
    public CoexistEvent Plane { get; set; }
    [JsonProperty(PropertyName = "Cargo Ship Event")]
    public CoexistEvent Ship { get; set; }
    [JsonProperty(PropertyName = "Chinook (CH47) Event")]
    public CoexistEvent Chinook { get; set; }
    [JsonProperty(PropertyName = "Helicopter Event")]
    public CoexistEvent Helicopter { get; set; }
    [JsonProperty(PropertyName = "Santa Sleigh Event")]
    public CoexistEvent SantaSleigh { get; set; }
    [JsonProperty(PropertyName = "Christmas Event")]
    public CoexistEvent Christmas { get; set; }
    [JsonProperty(PropertyName = "Easter Event")]
    public BaseEvent Easter { get; set; }
    [JsonProperty(PropertyName = "Halloween Event")]
    public BaseEvent Halloween { get; set; }
}

private class BaseEvent
{
    [JsonProperty(PropertyName = "Enabled", Order = 1)]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Display Name", Order = 2)]
    public string DisplayName { get; set; }
    [JsonProperty(PropertyName = "Disable Vanilla Event", Order = 3)]
    public bool DisableVanillaEvent { get; set; }
    [JsonProperty(PropertyName = "Event Start Offset (Minutes)", Order = 4)]
    public float StartOffset { get; set; }
    [JsonProperty(PropertyName = "Minimum Time Between (Minutes)", Order = 5)]
    public float MinimumTimeBetween { get; set; }
    [JsonProperty(PropertyName = "Maximum Time Between (Minutes)", Order = 6)]
    public float MaximumTimeBetween { get; set; }
    [JsonProperty(PropertyName = "Minimum Online Players Required (0 = Disabled)", Order = 7)]
    public int MinimumOnlinePlayers { get; set; }
    [JsonProperty(PropertyName = "Maximum Online Players Required (0 = Disabled)", Order = 8)]
    public int MaximumOnlinePlayers { get; set; }
    [JsonProperty(PropertyName = "Announce Next Run Time", Order = 9)]
    public bool AnnounceNext { get; set; }
    [JsonProperty(PropertyName = "Restart Timer On Entity Kill", Order = 10)]
    public bool RestartTimerOnKill { get; set; }
    [JsonProperty(PropertyName = "Kill Existing Event On Plugin Loaded", Order = 11)]
    public bool KillEventOnLoaded { get; set; }
    [JsonIgnore]
    public double NextRunTime { get; set; }
    public virtual EventWeight GetRandomEventWeight();
}

private class CoexistEvent : BaseEvent
{
    [JsonProperty(PropertyName = "Maximum Number On Server", Order = 19)]
    public int ServerMaximumNumber { get; set; }
    [JsonProperty(PropertyName = "Exclude Player's Entity", Order = 20)]
    public bool ExcludePlayerEntity { get; set; }
    [JsonProperty(PropertyName = "Event Weights", Order = 21, ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<EventWeight> EventWeights { get; set; }
    [JsonIgnore]
    private readonly List<EventWeight> _validEventWeights;
    public override EventWeight GetRandomEventWeight();
}

private class EventWeight
{
    [JsonProperty(PropertyName = "Weight")]
    public int Weight { get; set; }
    [JsonProperty(PropertyName = "Name")]
    public string Name { get; set; }
    [JsonConverter(typeof(StringEnumConverter))]
    [JsonProperty(PropertyName = "Argument Type (Command or CallHook)")]
    public ArgumentType? ArgType { get; set; }
    [JsonProperty(PropertyName = "Arguments")]
    public List<string> Args { get; set; }
    [JsonIgnore]
    public bool IsNormalEvent { get; set; }
    public bool IsValid();
    public void RunCustomEvent();
}


```

---

## AutomatedStashTraps by VisEntities - Catch 'em all, master the art of cheater hunting

```csharp
using Facepunch;
using Network;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Libraries;
using Rust;
using Rust.Workshop;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.Linq;
using System.Text;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Automated Stash Traps", "Dana", "1.5.4")]
[Description("Spawns fully automated stash traps across the map to catch ESP cheaters.")]
public class AutomatedStashTraps : RustPlugin
{
    [PluginReference]
    private readonly Plugin Clans;
    private static AutomatedStashTraps _instance;
    private static Configuration _config;
    private static Data _data;
    private SpawnPointManager _spawnPointManager;
    private DiscordWebhook _webhook;
    private SkinManager _skinManager;
    private Coroutine _spawnCoroutine;
    private List<BasePlayer> _manualTrapDeployers;
    private HashSet<ulong> _revealedOwnedStashes;
    private Dictionary<BasePlayer, StorageContainer> _activeLootEditors;
    private Timer _reportScheduler;
    private Queue<DiscordWebhook.Message> _queuedDiscordReports;
    private const string BLUEPRINT_TEMPLATE;
    private const string STASH_PREFAB;
    private const string STORAGE_PREFAB;
    private const string SLEEPING_BAG_PREFAB;
    private Vector3 _lastRevealedStashPosition;
    private class Configuration
    {
        [JsonProperty("Version")]
        public string Version { get; set; }
        [JsonProperty("Spawn Point")]
        public SpawnPointConfig SpawnPoint { get; set; }
        [JsonProperty("Automated Trap")]
        public AutomatedTrapConfig AutomatedTrap { get; set; }
        [JsonProperty("Violation")]
        public ViolationConfig Violation { get; set; }
        [JsonProperty("Moderation")]
        public ModerationConfig Moderation { get; set; }
        [JsonProperty("Notification")]
        public NotificationConfig Notification { get; set; }
        [JsonProperty("Discord")]
        public DiscordConfig Discord { get; set; }
        [JsonProperty("Stash Loot")]
        public StashLootConfig StashLoot { get; set; }
    }

    private class SpawnPointConfig
    {
        [JsonProperty("Maximum Attempts To Find Spawn Points")]
        public int MaximumAttemptsToFindSpawnPoints { get; set; }
        [JsonProperty("Safe Area Radius")]
        public float SafeAreaRadius { get; set; }
        [JsonProperty("Entity Detection Radius")]
        public float EntityDetectionRadius { get; set; }
        [JsonProperty("Player Detection Radius")]
        public float PlayerDetectionRadius { get; set; }
    }

    private class AutomatedTrapConfig
    {
        [JsonProperty("Maximum Traps To Spawn")]
        public int MaximumTrapsToSpawn { get; set; }
        [JsonProperty("Destroy Revealed Trap After Minutes")]
        public int DestroyRevealedTrapAfterMinutes { get; set; }
        [JsonProperty("Replace Revealed Trap")]
        public bool ReplaceRevealedTrap { get; set; }
        [JsonProperty("Dummy Sleeping Bag")]
        public DummySleepingBagConfig DummySleepingBag { get; set; }
    }

    private class DummySleepingBagConfig
    {
        [JsonProperty("Spawn Along")]
        public bool SpawnAlong { get; set; }
        [JsonProperty("Spawn Proximity To Stash")]
        public float SpawnProximityToStash { get; set; }
        [JsonProperty("Spawn Chance")]
        public int SpawnChance { get; set; }
        [JsonProperty("Randomized Skin Chance")]
        public int RandomizedSkinChance { get; set; }
        [JsonProperty("Randomized Nice Name Chance")]
        public int RandomizedNiceNameChance { get; set; }
    }

    private class ViolationConfig
    {
        [JsonProperty("Reset On Wipe")]
        public bool ResetOnWipe { get; set; }
        [JsonProperty("Can Teammate Ignore")]
        public bool CanTeammateIgnore { get; set; }
        [JsonProperty("Can Clanmate Ignore")]
        public bool CanClanmateIgnore { get; set; }
    }

    private class ModerationConfig
    {
        [JsonProperty("Automatic Ban")]
        public bool AutomaticBan { get; set; }
        [JsonProperty("Violations Tolerance")]
        public int ViolationsTolerance { get; set; }
        [JsonProperty("Ban Delay Seconds")]
        public int BanDelaySeconds { get; set; }
        [JsonProperty("Ban Reason")]
        public string BanReason { get; set; }
    }

    private class NotificationConfig
    {
        [JsonProperty("Prefix")]
        public string Prefix { get; set; }
        [JsonProperty("Enable Console Report")]
        public bool EnableConsoleReport { get; set; }
        [JsonProperty("Stash Report Filter")]
        public int StashReportFilter { get; set; }
    }

    private class DiscordConfig
    {
        [JsonProperty("Post Into Discord")]
        public bool PostIntoDiscord { get; set; }
        [JsonProperty("Webhook Url")]
        public string WebhookUrl { get; set; }
        [JsonProperty("Report Interval")]
        public float ReportInterval { get; set; }
        [JsonProperty("Message")]
        public string Message { get; set; }
        [JsonProperty("Embed Color")]
        public string EmbedColor { get; set; }
        [JsonProperty("Embed Title")]
        public string EmbedTitle { get; set; }
        [JsonProperty("Embed Footer")]
        public string EmbedFooter { get; set; }
        [JsonProperty("Embed Fields")]
        public List<DiscordWebhook.EmbedField> EmbedFields { get; set; }
        [JsonIgnore]
        private int color;
        [JsonIgnore]
        private bool colorIsValidated;
        public int GetColor();
    }

    private class StashLootConfig
    {
        [JsonProperty("Minimum Loot Spawn Slots")]
        public int MinimumLootSpawnSlots { get; set; }
        [JsonProperty("Maximum Loot Spawn Slots")]
        public int MaximumLootSpawnSlots { get; set; }
        [JsonProperty("Spawn Chance As Blueprint")]
        public int SpawnChanceAsBlueprint { get; set; }
        [JsonProperty("Spawn Chance With Skin")]
        public int SpawnChanceWithSkin { get; set; }
        [JsonProperty("Spawn Chance As Damaged")]
        public int SpawnChanceAsDamaged { get; set; }
        [JsonProperty("Minimum Condition Loss")]
        public float MinimumConditionLoss { get; set; }
        [JsonProperty("Maximum Condition Loss")]
        public float MaximumConditionLoss { get; set; }
        [JsonProperty("Spawn Chance As Repaired")]
        public int SpawnChanceAsRepaired { get; set; }
        [JsonProperty("Spawn Chance As Broken")]
        public int SpawnChanceAsBroken { get; set; }
        [JsonProperty("Loot Table")]
        public List<ItemInfo> LootTable { get; set; }
    }

    private class ItemInfo
    {
        [JsonProperty("Short Name")]
        public string ShortName { get; set; }
        [JsonProperty("Minimum Spawn Amount")]
        public int MinimumSpawnAmount { get; set; }
        [JsonProperty("Maximum Spawn Amount")]
        public int MaximumSpawnAmount { get; set; }
        [JsonIgnore]
        private ItemDefinition itemDefinition;
        [JsonIgnore]
        private bool itemIsValidated;
        public ItemDefinition GetItemDefinition();
        public bool CanBeResearched();
        public bool CanBeSkinned();
        public bool CanBeRepaired();
    }

    private Configuration GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private void ValidateConfigValues();
    private class Data
    {
        [JsonProperty("Violations")]
        public Dictionary<ulong, int> Violations { get; set; }
        [JsonProperty("Automated Traps")]
        public Dictionary<ulong, AutomatedTrapData> AutomatedTraps { get; set; }
        public static Data Load();
        public Data Save();
        public static Data Clear();
        public void RemovePlayerData(BasePlayer player);
        public void CreateOrUpdatePlayerData(BasePlayer player);
        public int GetPlayerRevealedTrapsCount(BasePlayer player);
        public void CreateTrapData(StashContainer stash, SleepingBag sleepingBag);
        public AutomatedTrapData GetTrapData(ulong trapId);
        public void UpdateTrapData(AutomatedTrapData trap);
    }

    private class AutomatedTrapData
    {
        [JsonProperty("Dummy Stash")]
        public DummyStashData DummyStash { get; set; }
        [JsonProperty("Dummy Sleeping Bag")]
        public DummySleepingBagData DummySleepingBag { get; set; }
    }

    private class DummyStashData
    {
        [JsonProperty("Hidden")]
        public bool Hidden { get; set; }
        [JsonProperty("Id")]
        public ulong Id { get; set; }
        [JsonProperty("Position")]
        public Vector3 Position { get; set; }
    }

    private class DummySleepingBagData
    {
        [JsonProperty("Id")]
        public ulong Id { get; set; }
        [JsonProperty("Nice Name")]
        public string NiceName { get; set; }
        [JsonProperty("Skin Id")]
        public ulong SkinId { get; set; }
        [JsonProperty("Position")]
        public Vector3 Position { get; set; }
    }

    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave();
    private void OnEntityBuilt(Planner planner, GameObject gameObject);
    private void OnPlayerLootEnd(PlayerLoot inventory);
    private void OnEntityKill(StashContainer stash);
    private void OnStashExposed(StashContainer stash, BasePlayer player);
    private void OnStashHidden(StashContainer stash, BasePlayer player);
    private void StartSpawnCoroutine();
    private void StopSpawnCoroutine();
    private IEnumerator SpawnTraps();
    private StashContainer CreateStashEntity(string prefabPath, Vector3 position, Quaternion rotation);
    private SleepingBag CreateSleepingBagEntity(string prefabPath, Vector3 position, Quaternion rotation);
    private void PopulateLoot(StashContainer stash);
    private void RandomizeItemCondition(Item item);
    private void CleanupTraps();
    private void TryDestroyAndReplaceTrap(AutomatedTrapData trap);
    private void OnStashTriggered(StashContainer stash, BasePlayer player, bool stashWasDestroyed);
    private string ConstructConsoleReport(StashContainer stash, BasePlayer player);
    private void HandleDestroyedStash(StashContainer stash);
    private void IssueBan(BasePlayer player);
    private bool StashIsAutomatedTrap(StashContainer stash);
    public class SpawnPointManager
    {
        private HashSet<Tuple<Vector3, Quaternion>> availableSpawnPoints;
        public int AvailableSpawnPointsCount { get; set; }
        public IEnumerator GenerateSpawnPoints(int spawnPointsToGenerate);
        public Tuple<Vector3, Quaternion> GetRandomSpawnPoint();
        public Tuple<Vector3, Quaternion> FindChildSpawnPoint(Vector3 parentPosition);
        public void ClearAvailableSpawnPoints();
        private Tuple<Vector3, Quaternion> FinalizeSpawnPoint(Vector3 position);
        private bool PositionIsValid(Vector3 position);
        private bool PositionIsOnTerrain(Vector3 position);
        private bool PositionIsInRestrictedBuildingZone(Vector3 position);
        private bool PositionIsOnRoadOrRail(Vector3 position);
        private bool PositionIsOnCliff(Vector3 position);
        private bool PositionIsInWater(Vector3 position);
        private bool PositionIsOnIce(Vector3 position);
        private bool PositionIsOnRock(Vector3 position);
        private bool PositionHasEntityNearby(Vector3 position);
        private bool PositionHasPlayerInRange(Vector3 position);
    }

    public class SkinManager
    {
        private Dictionary<string, List<ulong>> extractedSkins;
        public List<ulong> GetSkinsForItem(ItemDefinition itemDefinition);
        private List<ulong> ExtractApprovedSkins(ItemDefinition itemDefinition, List<ulong> skins);
    }

    private void PushQueuedDiscordReports();
    private DiscordWebhook.Message ConstructDiscordReport(StashContainer stash, BasePlayer player, bool stashWasKilled);
    private class DiscordWebhook
    {
        public void SendRequest(string webhookUrl, Message message);
        public class Message
        {
            [JsonProperty("username")]
            public string Username { get; set; }
            [JsonProperty("icon_url")]
            public string IconUrl { get; set; }
            [JsonProperty("content")]
            public string Content { get; set; }
            [JsonProperty("embeds")]
            public List<Embed> Embeds { get; set; }
            public Message();
            public void AddEmbed(Embed embed);
            public override string ToString();
        }

        public class Embed
        {
            [JsonProperty("title")]
            public string Title { get; set; }
            [JsonProperty("description")]
            public string Description { get; set; }
            [JsonProperty("url")]
            public string Url { get; set; }
            [JsonProperty("color")]
            public int Color { get; set; }
            [JsonProperty("timestamp")]
            public string Timestamp { get; set; }
            [JsonProperty("thumbnail")]
            public EmbedThumbnail Thumbnail { get; set; }
            [JsonProperty("author")]
            public EmbedAuthor Author { get; set; }
            [JsonProperty("footer")]
            public EmbedFooter Footer { get; set; }
            [JsonProperty("image")]
            public EmbedImage Image { get; set; }
            [JsonProperty("fields")]
            public List<EmbedField> EmbedFields { get; set; }
            public Embed();
            public void AddField(EmbedField field);
        }

        public class EmbedField
        {
            [JsonProperty("name")]
            public string Name { get; set; }
            [JsonProperty("value")]
            public string Value { get; set; }
            [JsonProperty("inline")]
            public bool Inline { get; set; }
            public EmbedField();
        }

        public class EmbedThumbnail
        {
            [JsonProperty("url")]
            public string AvatarUrl { get; set; }
            [JsonProperty("width")]
            public int Width { get; set; }
            [JsonProperty("height")]
            public int Height { get; set; }
            public EmbedThumbnail();
        }

        public class EmbedAuthor
        {
            [JsonProperty("name")]
            public string Name { get; set; }
            [JsonProperty("url")]
            public string Url { get; set; }
            [JsonProperty("icon_url")]
            public string IconUrl { get; set; }
            public EmbedAuthor();
        }

        public class EmbedImage
        {
            [JsonProperty("url")]
            public string AvatarUrl { get; set; }
            [JsonProperty("width")]
            public int Width { get; set; }
            [JsonProperty("height")]
            public int Height { get; set; }
            public EmbedImage();
        }

        public class EmbedFooter
        {
            [JsonProperty("text")]
            public string Text { get; set; }
            [JsonProperty("icon_url")]
            public string IconUrl { get; set; }
            public EmbedFooter();
        }

        private void HandleRequestResponse(int headerCode, string headerResult);
    }

    public static class Placeholder
    {
        public const string PLAYER_NAME;
        public const string PLAYER_ID;
        public const string PLAYER_ADDRESS;
        public const string PLAYER_VIOLATIONS;
        public const string PLAYER_TEAM;
        public const string PLAYER_CONNECTION_TIME;
        public const string PLAYER_COMBAT_ID;
        public const string STASH_TYPE;
        public const string STASH_ID;
        public const string STASH_OWNER_NAME;
        public const string STASH_OWNER_ID;
        public const string STASH_REVEAL_METHOD;
        public const string STASH_POSITION_COORDINATES;
        public const string STASH_POSITION_GRID;
        public const string STASH_ITEMS;
        public const string SERVER_NAME;
        public const string SERVER_ADDRESS;
        public static string ReplacePlaceholders(string text, BasePlayer player, StashContainer stash, bool stashWasKilled);
    }

    private void OpenLootEditor(BasePlayer player);
    private void CloseLootEditor(PlayerLoot inventory);
    private void UpdateStashLootTable(StorageContainer storageContainer, BasePlayer player);
    private void RemoveLooter(BasePlayer player, StorageContainer storageContainer);
    private StorageContainer CreateStorageEntity(string prefabPath);
    private void DrawTraps(BasePlayer player, int drawDuration);
    private static class Draw
    {
        public static void Sphere(BasePlayer player, float duration, Color color, Vector3 originPosition, float radius);
        public static void Line(BasePlayer player, float duration, Color color, Vector3 originPosition, Vector3 targetPosition);
        public static void Arrow(BasePlayer player, float duration, Color color, Vector3 originPosition, Vector3 targetPosition, float headSize);
        public static void Text(BasePlayer player, float duration, Color color, Vector3 originPosition, string text);
    }

    private BaseEntity FindEntityById(ulong entityId);
    private bool StashIsOwned(StashContainer stash);
    private bool PlayerIsStashOwner(StashContainer stash, BasePlayer player);
    private string GetGrid(Vector3 position);
    private BasePlayer FindPlayerById(ulong playerId);
    private bool PlayerExistsInOwnerTeam(ulong stashOwnerId, BasePlayer targetPlayer);
    private bool PlayerExistsInOwnerClan(ulong stashOwnerId, BasePlayer targetPlayer);
    private void GetTeam(BasePlayer player, List<ulong> teammates, ulong leader);
    private string FormatPlayerName(BasePlayer player);
    private string FormatTeam(BasePlayer player);
    private string FormatConnectionTime(BasePlayer player);
    private bool PluginIsLoaded(Plugin plugin);
    private bool ChanceSucceeded(int chance);
    private Color ParseColor(string hexadecimalColor, Color defaultColor);
    private static class PermissionUtils
    {
        public const string ADMIN;
        public const string IGNORE;
        public static void Register();
        public static bool Verify(BasePlayer player, string permissionName);
    }

    private static class Command
    {
        public const string GIVE;
        public const string LOOT;
        public const string DRAW;
        public const string TELEPORT;
    }

    [ConsoleCommand(Command.GIVE)]
    private void cmdGive(ConsoleSystem.Arg conArgs);
    [ChatCommand(Command.LOOT)]
    private void cmdLoot(BasePlayer player, string cmd, string[] args);
    [ConsoleCommand(Command.DRAW)]
    private void cmdDraw(ConsoleSystem.Arg conArgs);
    [ConsoleCommand(Command.TELEPORT)]
    private void cmdTeleport(ConsoleSystem.Arg conArgs);
    private class Lang
    {
        public const string ERROR_PERMISSION;
        public const string TRAP_REVEAL;
        public const string TRAP_LOOT;
        public const string TRAP_DRAW;
        public const string TRAP_GIVE;
        public const string TRAP_SETUP;
    }

    protected override void LoadDefaultMessages();
    private string GetLang(string langKey, string playerId);
    private void ReplyToPlayer(BasePlayer player, string message, object[] args);
}

private class Configuration
{
    [JsonProperty("Version")]
    public string Version { get; set; }
    [JsonProperty("Spawn Point")]
    public SpawnPointConfig SpawnPoint { get; set; }
    [JsonProperty("Automated Trap")]
    public AutomatedTrapConfig AutomatedTrap { get; set; }
    [JsonProperty("Violation")]
    public ViolationConfig Violation { get; set; }
    [JsonProperty("Moderation")]
    public ModerationConfig Moderation { get; set; }
    [JsonProperty("Notification")]
    public NotificationConfig Notification { get; set; }
    [JsonProperty("Discord")]
    public DiscordConfig Discord { get; set; }
    [JsonProperty("Stash Loot")]
    public StashLootConfig StashLoot { get; set; }
}

private class SpawnPointConfig
{
    [JsonProperty("Maximum Attempts To Find Spawn Points")]
    public int MaximumAttemptsToFindSpawnPoints { get; set; }
    [JsonProperty("Safe Area Radius")]
    public float SafeAreaRadius { get; set; }
    [JsonProperty("Entity Detection Radius")]
    public float EntityDetectionRadius { get; set; }
    [JsonProperty("Player Detection Radius")]
    public float PlayerDetectionRadius { get; set; }
}

private class AutomatedTrapConfig
{
    [JsonProperty("Maximum Traps To Spawn")]
    public int MaximumTrapsToSpawn { get; set; }
    [JsonProperty("Destroy Revealed Trap After Minutes")]
    public int DestroyRevealedTrapAfterMinutes { get; set; }
    [JsonProperty("Replace Revealed Trap")]
    public bool ReplaceRevealedTrap { get; set; }
    [JsonProperty("Dummy Sleeping Bag")]
    public DummySleepingBagConfig DummySleepingBag { get; set; }
}

private class DummySleepingBagConfig
{
    [JsonProperty("Spawn Along")]
    public bool SpawnAlong { get; set; }
    [JsonProperty("Spawn Proximity To Stash")]
    public float SpawnProximityToStash { get; set; }
    [JsonProperty("Spawn Chance")]
    public int SpawnChance { get; set; }
    [JsonProperty("Randomized Skin Chance")]
    public int RandomizedSkinChance { get; set; }
    [JsonProperty("Randomized Nice Name Chance")]
    public int RandomizedNiceNameChance { get; set; }
}

private class ViolationConfig
{
    [JsonProperty("Reset On Wipe")]
    public bool ResetOnWipe { get; set; }
    [JsonProperty("Can Teammate Ignore")]
    public bool CanTeammateIgnore { get; set; }
    [JsonProperty("Can Clanmate Ignore")]
    public bool CanClanmateIgnore { get; set; }
}

private class ModerationConfig
{
    [JsonProperty("Automatic Ban")]
    public bool AutomaticBan { get; set; }
    [JsonProperty("Violations Tolerance")]
    public int ViolationsTolerance { get; set; }
    [JsonProperty("Ban Delay Seconds")]
    public int BanDelaySeconds { get; set; }
    [JsonProperty("Ban Reason")]
    public string BanReason { get; set; }
}

private class NotificationConfig
{
    [JsonProperty("Prefix")]
    public string Prefix { get; set; }
    [JsonProperty("Enable Console Report")]
    public bool EnableConsoleReport { get; set; }
    [JsonProperty("Stash Report Filter")]
    public int StashReportFilter { get; set; }
}

private class DiscordConfig
{
    [JsonProperty("Post Into Discord")]
    public bool PostIntoDiscord { get; set; }
    [JsonProperty("Webhook Url")]
    public string WebhookUrl { get; set; }
    [JsonProperty("Report Interval")]
    public float ReportInterval { get; set; }
    [JsonProperty("Message")]
    public string Message { get; set; }
    [JsonProperty("Embed Color")]
    public string EmbedColor { get; set; }
    [JsonProperty("Embed Title")]
    public string EmbedTitle { get; set; }
    [JsonProperty("Embed Footer")]
    public string EmbedFooter { get; set; }
    [JsonProperty("Embed Fields")]
    public List<DiscordWebhook.EmbedField> EmbedFields { get; set; }
    [JsonIgnore]
    private int color;
    [JsonIgnore]
    private bool colorIsValidated;
    public int GetColor();
}

private class StashLootConfig
{
    [JsonProperty("Minimum Loot Spawn Slots")]
    public int MinimumLootSpawnSlots { get; set; }
    [JsonProperty("Maximum Loot Spawn Slots")]
    public int MaximumLootSpawnSlots { get; set; }
    [JsonProperty("Spawn Chance As Blueprint")]
    public int SpawnChanceAsBlueprint { get; set; }
    [JsonProperty("Spawn Chance With Skin")]
    public int SpawnChanceWithSkin { get; set; }
    [JsonProperty("Spawn Chance As Damaged")]
    public int SpawnChanceAsDamaged { get; set; }
    [JsonProperty("Minimum Condition Loss")]
    public float MinimumConditionLoss { get; set; }
    [JsonProperty("Maximum Condition Loss")]
    public float MaximumConditionLoss { get; set; }
    [JsonProperty("Spawn Chance As Repaired")]
    public int SpawnChanceAsRepaired { get; set; }
    [JsonProperty("Spawn Chance As Broken")]
    public int SpawnChanceAsBroken { get; set; }
    [JsonProperty("Loot Table")]
    public List<ItemInfo> LootTable { get; set; }
}

private class ItemInfo
{
    [JsonProperty("Short Name")]
    public string ShortName { get; set; }
    [JsonProperty("Minimum Spawn Amount")]
    public int MinimumSpawnAmount { get; set; }
    [JsonProperty("Maximum Spawn Amount")]
    public int MaximumSpawnAmount { get; set; }
    [JsonIgnore]
    private ItemDefinition itemDefinition;
    [JsonIgnore]
    private bool itemIsValidated;
    public ItemDefinition GetItemDefinition();
    public bool CanBeResearched();
    public bool CanBeSkinned();
    public bool CanBeRepaired();
}

private class Data
{
    [JsonProperty("Violations")]
    public Dictionary<ulong, int> Violations { get; set; }
    [JsonProperty("Automated Traps")]
    public Dictionary<ulong, AutomatedTrapData> AutomatedTraps { get; set; }
    public static Data Load();
    public Data Save();
    public static Data Clear();
    public void RemovePlayerData(BasePlayer player);
    public void CreateOrUpdatePlayerData(BasePlayer player);
    public int GetPlayerRevealedTrapsCount(BasePlayer player);
    public void CreateTrapData(StashContainer stash, SleepingBag sleepingBag);
    public AutomatedTrapData GetTrapData(ulong trapId);
    public void UpdateTrapData(AutomatedTrapData trap);
}

private class AutomatedTrapData
{
    [JsonProperty("Dummy Stash")]
    public DummyStashData DummyStash { get; set; }
    [JsonProperty("Dummy Sleeping Bag")]
    public DummySleepingBagData DummySleepingBag { get; set; }
}

private class DummyStashData
{
    [JsonProperty("Hidden")]
    public bool Hidden { get; set; }
    [JsonProperty("Id")]
    public ulong Id { get; set; }
    [JsonProperty("Position")]
    public Vector3 Position { get; set; }
}

private class DummySleepingBagData
{
    [JsonProperty("Id")]
    public ulong Id { get; set; }
    [JsonProperty("Nice Name")]
    public string NiceName { get; set; }
    [JsonProperty("Skin Id")]
    public ulong SkinId { get; set; }
    [JsonProperty("Position")]
    public Vector3 Position { get; set; }
}

public class SpawnPointManager
{
    private HashSet<Tuple<Vector3, Quaternion>> availableSpawnPoints;
    public int AvailableSpawnPointsCount { get; set; }
    public IEnumerator GenerateSpawnPoints(int spawnPointsToGenerate);
    public Tuple<Vector3, Quaternion> GetRandomSpawnPoint();
    public Tuple<Vector3, Quaternion> FindChildSpawnPoint(Vector3 parentPosition);
    public void ClearAvailableSpawnPoints();
    private Tuple<Vector3, Quaternion> FinalizeSpawnPoint(Vector3 position);
    private bool PositionIsValid(Vector3 position);
    private bool PositionIsOnTerrain(Vector3 position);
    private bool PositionIsInRestrictedBuildingZone(Vector3 position);
    private bool PositionIsOnRoadOrRail(Vector3 position);
    private bool PositionIsOnCliff(Vector3 position);
    private bool PositionIsInWater(Vector3 position);
    private bool PositionIsOnIce(Vector3 position);
    private bool PositionIsOnRock(Vector3 position);
    private bool PositionHasEntityNearby(Vector3 position);
    private bool PositionHasPlayerInRange(Vector3 position);
}

public class SkinManager
{
    private Dictionary<string, List<ulong>> extractedSkins;
    public List<ulong> GetSkinsForItem(ItemDefinition itemDefinition);
    private List<ulong> ExtractApprovedSkins(ItemDefinition itemDefinition, List<ulong> skins);
}

private class DiscordWebhook
{
    public void SendRequest(string webhookUrl, Message message);
    public class Message
    {
        [JsonProperty("username")]
        public string Username { get; set; }
        [JsonProperty("icon_url")]
        public string IconUrl { get; set; }
        [JsonProperty("content")]
        public string Content { get; set; }
        [JsonProperty("embeds")]
        public List<Embed> Embeds { get; set; }
        public Message();
        public void AddEmbed(Embed embed);
        public override string ToString();
    }

    public class Embed
    {
        [JsonProperty("title")]
        public string Title { get; set; }
        [JsonProperty("description")]
        public string Description { get; set; }
        [JsonProperty("url")]
        public string Url { get; set; }
        [JsonProperty("color")]
        public int Color { get; set; }
        [JsonProperty("timestamp")]
        public string Timestamp { get; set; }
        [JsonProperty("thumbnail")]
        public EmbedThumbnail Thumbnail { get; set; }
        [JsonProperty("author")]
        public EmbedAuthor Author { get; set; }
        [JsonProperty("footer")]
        public EmbedFooter Footer { get; set; }
        [JsonProperty("image")]
        public EmbedImage Image { get; set; }
        [JsonProperty("fields")]
        public List<EmbedField> EmbedFields { get; set; }
        public Embed();
        public void AddField(EmbedField field);
    }

    public class EmbedField
    {
        [JsonProperty("name")]
        public string Name { get; set; }
        [JsonProperty("value")]
        public string Value { get; set; }
        [JsonProperty("inline")]
        public bool Inline { get; set; }
        public EmbedField();
    }

    public class EmbedThumbnail
    {
        [JsonProperty("url")]
        public string AvatarUrl { get; set; }
        [JsonProperty("width")]
        public int Width { get; set; }
        [JsonProperty("height")]
        public int Height { get; set; }
        public EmbedThumbnail();
    }

    public class EmbedAuthor
    {
        [JsonProperty("name")]
        public string Name { get; set; }
        [JsonProperty("url")]
        public string Url { get; set; }
        [JsonProperty("icon_url")]
        public string IconUrl { get; set; }
        public EmbedAuthor();
    }

    public class EmbedImage
    {
        [JsonProperty("url")]
        public string AvatarUrl { get; set; }
        [JsonProperty("width")]
        public int Width { get; set; }
        [JsonProperty("height")]
        public int Height { get; set; }
        public EmbedImage();
    }

    public class EmbedFooter
    {
        [JsonProperty("text")]
        public string Text { get; set; }
        [JsonProperty("icon_url")]
        public string IconUrl { get; set; }
        public EmbedFooter();
    }

    private void HandleRequestResponse(int headerCode, string headerResult);
}

public class Message
{
    [JsonProperty("username")]
    public string Username { get; set; }
    [JsonProperty("icon_url")]
    public string IconUrl { get; set; }
    [JsonProperty("content")]
    public string Content { get; set; }
    [JsonProperty("embeds")]
    public List<Embed> Embeds { get; set; }
    public Message();
    public void AddEmbed(Embed embed);
    public override string ToString();
}

public class Embed
{
    [JsonProperty("title")]
    public string Title { get; set; }
    [JsonProperty("description")]
    public string Description { get; set; }
    [JsonProperty("url")]
    public string Url { get; set; }
    [JsonProperty("color")]
    public int Color { get; set; }
    [JsonProperty("timestamp")]
    public string Timestamp { get; set; }
    [JsonProperty("thumbnail")]
    public EmbedThumbnail Thumbnail { get; set; }
    [JsonProperty("author")]
    public EmbedAuthor Author { get; set; }
    [JsonProperty("footer")]
    public EmbedFooter Footer { get; set; }
    [JsonProperty("image")]
    public EmbedImage Image { get; set; }
    [JsonProperty("fields")]
    public List<EmbedField> EmbedFields { get; set; }
    public Embed();
    public void AddField(EmbedField field);
}

public class EmbedField
{
    [JsonProperty("name")]
    public string Name { get; set; }
    [JsonProperty("value")]
    public string Value { get; set; }
    [JsonProperty("inline")]
    public bool Inline { get; set; }
    public EmbedField();
}

public class EmbedThumbnail
{
    [JsonProperty("url")]
    public string AvatarUrl { get; set; }
    [JsonProperty("width")]
    public int Width { get; set; }
    [JsonProperty("height")]
    public int Height { get; set; }
    public EmbedThumbnail();
}

public class EmbedAuthor
{
    [JsonProperty("name")]
    public string Name { get; set; }
    [JsonProperty("url")]
    public string Url { get; set; }
    [JsonProperty("icon_url")]
    public string IconUrl { get; set; }
    public EmbedAuthor();
}

public class EmbedImage
{
    [JsonProperty("url")]
    public string AvatarUrl { get; set; }
    [JsonProperty("width")]
    public int Width { get; set; }
    [JsonProperty("height")]
    public int Height { get; set; }
    public EmbedImage();
}

public class EmbedFooter
{
    [JsonProperty("text")]
    public string Text { get; set; }
    [JsonProperty("icon_url")]
    public string IconUrl { get; set; }
    public EmbedFooter();
}

public static class Placeholder
{
    public const string PLAYER_NAME;
    public const string PLAYER_ID;
    public const string PLAYER_ADDRESS;
    public const string PLAYER_VIOLATIONS;
    public const string PLAYER_TEAM;
    public const string PLAYER_CONNECTION_TIME;
    public const string PLAYER_COMBAT_ID;
    public const string STASH_TYPE;
    public const string STASH_ID;
    public const string STASH_OWNER_NAME;
    public const string STASH_OWNER_ID;
    public const string STASH_REVEAL_METHOD;
    public const string STASH_POSITION_COORDINATES;
    public const string STASH_POSITION_GRID;
    public const string STASH_ITEMS;
    public const string SERVER_NAME;
    public const string SERVER_ADDRESS;
    public static string ReplacePlaceholders(string text, BasePlayer player, StashContainer stash, bool stashWasKilled);
}

private static class Draw
{
    public static void Sphere(BasePlayer player, float duration, Color color, Vector3 originPosition, float radius);
    public static void Line(BasePlayer player, float duration, Color color, Vector3 originPosition, Vector3 targetPosition);
    public static void Arrow(BasePlayer player, float duration, Color color, Vector3 originPosition, Vector3 targetPosition, float headSize);
    public static void Text(BasePlayer player, float duration, Color color, Vector3 originPosition, string text);
}

private static class PermissionUtils
{
    public const string ADMIN;
    public const string IGNORE;
    public static void Register();
    public static bool Verify(BasePlayer player, string permissionName);
}

private static class Command
{
    public const string GIVE;
    public const string LOOT;
    public const string DRAW;
    public const string TELEPORT;
}

private class Lang
{
    public const string ERROR_PERMISSION;
    public const string TRAP_REVEAL;
    public const string TRAP_LOOT;
    public const string TRAP_DRAW;
    public const string TRAP_GIVE;
    public const string TRAP_SETUP;
}


```

---

## AutomatedWorkcarts by WhiteThunder - Automates trains with NPC conductors to create a public transportation system

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Reflection;
using System.Text;
using System.Text.RegularExpressions;
using UnityEngine;
using static BaseEntity;
using static TrainCar;
using static TrainEngine;
using static TrainTrackSpline;

Oxide.Plugins
[Info("Automated Workcarts", "WhiteThunder", "0.34.3")]
[Description("Automates workcarts with NPC conductors.")]
internal class AutomatedWorkcarts : CovalencePlugin
{
    [PluginReference]
    private Plugin CargoTrainEvent;
    private const string PermissionToggle;
    private const string PermissionManageTriggers;
    private const string ShopkeeperPrefab;
    private const string GenericMapMarkerPrefab;
    private const string VendingMapMarkerPrefab;
    private const string ExplosionMapMakerPrefab;
    private const string CrateMarkerPrefab;
    private const string BradleyExplosionEffectPrefab;
    private static readonly FieldInfo TrainCouplingIsValidField;
    private readonly object False;
    private static readonly Regex IdRegex;
    private readonly BasePlayer[] _playerQueryResults;
    private Configuration _config;
    private StoredPluginData _data;
    private StoredTunnelData _tunnelData;
    private StoredMapData _mapData;
    private readonly SpawnedTrainCarTracker _spawnedTrainCarTracker;
    private readonly DisableSpawnPointManager _disableSpawnPointManager;
    private readonly TriggerManager _triggerManager;
    private readonly TrainManager _trainManager;
    private readonly RouteManager _routeManager;
    private readonly ColorMarkerUpdateManager _colorMarkerUpdateManager;
    private Coroutine _startupCoroutine;
    private Timer _showStatesTimer;
    public AutomatedWorkcarts();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnServerSave();
    private void OnNewSave();
    private void OnPlayerConnected(BasePlayer player);
    private object OnTrainCarUncouple(TrainCar trainCar, BasePlayer player);
    [Command("aw.toggle")]
    private void CommandAutomateTrain(IPlayer player, string cmd, string[] args);
    [Command("aw.resetall")]
    private void CommandResetTrains(IPlayer player, string cmd, string[] args);
    [Command("aw.addtrigger", "awt.add")]
    private void CommandAddTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.addtunneltrigger", "awt.addt")]
    private void CommandAddTunnelTrigger(IPlayer player, string cmd, string[] args);
    private void AddTriggerShared(IPlayer player, string cmd, string[] args, TriggerData triggerData, DungeonCellWrapper dungeonCellWrapper);
    [Command("aw.updatetrigger", "awt.update")]
    private void CommandUpdateTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.replacetrigger", "awt.replace")]
    private void CommandReplaceTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.enabletrigger", "awt.enable")]
    private void CommandEnableTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.disabletrigger", "awt.disable")]
    private void CommandDisableTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.movetrigger", "awt.move")]
    private void CommandMoveTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.removetrigger", "awt.remove")]
    private void CommandRemoveTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.rotatetrigger", "awt.rotate")]
    private void CommandSetTriggerRotation(IPlayer player, string cmd, string[] args);
    [Command("aw.respawntrigger", "awt.respawn")]
    private void CommandRespawnTrigger(IPlayer player, string cmd, string[] args);
    [Command("aw.addtriggercommand", "awt.addcommand", "awt.addcmd")]
    private void CommandAddCommand(IPlayer player, string cmd, string[] args);
    [Command("aw.removetriggercommand", "awt.removecommand", "awt.removecmd")]
    private void CommandRemoveCommand(IPlayer player, string cmd, string[] args);
    [Command("aw.settriggertrain", "awt.train")]
    private void CommandTriggerTrain(IPlayer player, string cmd, string[] args);
    [Command("aw.showtriggers", "awt.show")]
    private void CommandShowTriggers(IPlayer player, string cmd, string[] args);
    [Command("aw.showstates")]
    private void CommandShowStates(IPlayer player, string cmd, string[] args);
    [HookMethod(nameof(API_AutomateWorkcart))]
    public bool API_AutomateWorkcart(TrainEngine trainEngine);
    [HookMethod(nameof(API_StopAutomatingWorkcart))]
    public void API_StopAutomatingWorkcart(TrainEngine trainEngine);
    [HookMethod(nameof(API_IsWorkcartAutomated))]
    public bool API_IsWorkcartAutomated(TrainEngine trainEngine);
    [HookMethod(nameof(API_GetAutomatedWorkcarts))]
    public TrainEngine[] API_GetAutomatedWorkcarts();
    private static class ExposedHooks
    {
        public static object OnWorkcartAutomationStart(TrainEngine trainEngine);
        public static void OnWorkcartAutomationStarted(TrainEngine trainEngine);
        public static void OnWorkcartAutomationStopped(TrainEngine trainEngine);
    }

    private bool IsCargoTrain(TrainEngine trainEngine);
    private bool VerifyPermission(IPlayer player, string permissionName);
    private bool VerifyAnyTriggers(IPlayer player);
    private bool VerifyTriggerExists(IPlayer player, int triggerId, TrainTriggerType triggerType, TriggerData triggerData);
    private bool VerifyAimingAtTrackPosition(IPlayer player, Vector3 trackPosition);
    private bool IsTriggerArg(IPlayer player, string arg, int triggerId, TrainTriggerType triggerType);
    private bool VerifyTriggerWhereAiming(IPlayer player, string cmd, string[] args, string errorMessageName, TriggerData triggerData, string[] optionArgs);
    private bool VerifyCanModifyTrigger(IPlayer player, string cmd, string[] args, string errorMessageName, TriggerData triggerData, string[] optionArgs);
    private bool VerifySupportedNearbyTrainTunnel(IPlayer player, Vector3 trackPosition, DungeonCellWrapper dungeonCellWrapper);
    private bool VerifyValidArgAndModifyTrigger(IPlayer player, string cmd, string arg, TriggerData triggerData, string errorMessageName);
    private static void UpdateAllowedCouplings(TrainCar trainCar, bool allowFront, bool allowRear);
    private static void DisableTrainCoupling(CompleteTrain completeTrain);
    private static void EnableTrainCoupling(CompleteTrain completeTrain);
    private static void LogError(string message);
    private static void LogWarning(string message);
    private static int GetNextTriggerId(List<TriggerData> triggerList);
    private static string GetRouteNameFromArg(string routeName, bool requirePrefix);
    private static float GetThrottleFraction(EngineSpeeds throttle);
    private static TrainEngine GetLeadTrainEngine(CompleteTrain completeTrain);
    private static TrainEngine GetLeadTrainEngine(TrainCar trainCar);
    private static void DetermineTrainCarOrientations(TrainCar trainCar, Vector3 forward, TrainCar otherTrainCar, TrainCar forwardTrainCar);
    private static Vector3 GetTrainCarForward(TrainCar trainCar);
    private static void EnableInvincibility(TrainCar trainCar);
    private static void DisableInvincibility(TrainCar trainCar);
    private static void EnableSavingRecursive(BaseEntity entity, bool enableSaving);
    private static TrainCar SpawnTrainCar(string prefabName, Vector3 position, Quaternion rotation);
    private static float GetSplineDistance(TrainTrackSpline spline, Vector3 position);
    private static TrainCar AddTrainCar(TrainCar frontTrainCar, TrainCarPrefab frontTrainCarPrefab, TrainCarPrefab trainCarPrefab, TrackSelection trackSelection);
    private static float GetTrainCarFrontCouplingOffsetZ(TrainCarPrefab trainCarPrefab);
    private static ConnectedTrackInfo GetAdjacentTrackInfo(TrainTrackSpline spline, TrackSelection selection, bool isAscending, bool askerIsForward);
    private static Quaternion GetSplineTangentRotation(TrainTrackSpline spline, float distanceOnSpline, Quaternion approximateRotation);
    private static int CompareVectors(Vector3 a, Vector3 b);
    private IEnumerator DoStartupRoutine();
    private bool AutomationWasBlocked(TrainEngine trainEngine);
    private static Vector3 GetPositionAlongTrack(SplineInfo splineInfo, float desiredDistance, TrackSelection trackSelection, SplineInfo resultSplineInfo, float remainingDistance);
    private static Vector3 GetPositionAlongTrack(SplineInfo splineInfo, float desiredDistance, TrackSelection trackSelection, SplineInfo resultSplineInfo);
    private static bool IsTrainOwned(TrainCar trainCar);
    private static string GetShortName(string prefabName);
    private static bool TryParseEngineSpeed(string speedName, EngineSpeeds engineSpeed);
    private static bool TryParseTrackSelection(string selectionName, TrackSelection trackSelection);
    private static string FormatOptions(ICollection<string> optionNames, string delimiter);
    private static string GetEnumOptions();
    private static bool TryGetHitPosition(BasePlayer player, Vector3 position, float maxDistance);
    private static bool TryGetTrackPosition(BasePlayer player, Vector3 trackPosition, float maxDistance);
    private static TrainTrigger GetHitTrigger(BasePlayer player, float maxDistance);
    private static DungeonCellWrapper FindNearestDungeonCell(Vector3 position);
    private static List<DungeonCellWrapper> FindAllTunnelsOfType(TunnelType tunnelType);
    private static BaseEntity GetLookEntity(BasePlayer player, int layerMask, float maxDistance);
    private static TrainCar GetTrainCarWhereAiming(BasePlayer player);
    private static void DestroyTrainCarCinematically(TrainCar trainCar);
    private static void ScheduleDestroyTrainCarCinematically(TrainCar trainCar);
    private static bool CollectionsEqual(ICollection<T> collectionA, ICollection<T> collectionB);
    private static string FormatTime(double seconds);
    private class TrackedCoroutine : IEnumerator
    {
        private readonly Plugin _plugin;
        private IEnumerator _inner;
        private TrackedCoroutine _innerTracked;
        public TrackedCoroutine(Plugin plugin, IEnumerator inner);
        public object Current { get; set; }
        public bool MoveNext();
        public void Reset();
        public TrackedCoroutine WithEnumerator(IEnumerator inner);
        private TrackedCoroutine GetTrackedCoroutine(IEnumerator enumerator);
    }

    private static class EntityUtils
    {
        public static T CreateEntity(string prefabPath, Vector3 position, Quaternion rotation);
        public static bool KillEntity(BaseEntity entity, BaseNetworkable.DestroyMode destroyMode);
        public static bool MoveEntity(BaseEntity entity, Vector3 position);
    }

    private static class MarkerUtils
    {
        public static MapMarkerGenericRadius CreateColorMarker(Vector3 position, Color color, float radius, float alpha);
        public static MapMarkerGenericRadius CreateColorMarker(ColorMarkerOptions markerOptions, Vector3 position, Color? colorOverride);
        public static VendingMachineMapMarker CreateVendingMarker(VendingMarkerOptions markerOptions, Vector3 position);
        public static bool ResendMarkerColor(MapMarkerGenericRadius colorMarker);
        public static bool UpdateMarkerColor(MapMarkerGenericRadius colorMarker, Color color);
    }

    private class Route
    {
        public readonly List<BaseTriggerInstance> TriggerList;
        public readonly List<TrainController> TrainControllerList;
        public Color Color { get; set; }
        public Route(List<BaseTriggerInstance> triggerList);
        public bool Matches(List<BaseTriggerInstance> triggerList);
        public void SetColor(Color color);
    }

    private class RouteManager
    {
        private readonly AutomatedWorkcarts _plugin;
        private readonly TrackedCoroutine _trackedCoroutine;
        private readonly WaitForSeconds _shortDelay;
        private List<Route> _allRoutes;
        private Dictionary<TrainController, Route> _trainControllerToRoute;
        private Dictionary<BaseTriggerInstance, Route> _triggerInstanceToRoute;
        private Coroutine _determineRoutesRoutine;
        private HashSet<TrainTrackSpline> _reusableSplineList;
        private HashSet<BaseTriggerInstance> _reusableTriggerList;
        private HashSet<BaseTriggerInstance> _reusableTriggerListForSpline;
        private Configuration _config { get; set; }
        private TrainManager _trainManager { get; set; }
        private TriggerManager _triggerManager { get; set; }
        public RouteManager(AutomatedWorkcarts plugin);
        public void Unload();
        public void RecomputeRoutes();
        public Route GetRoute(TrainController trainController);
        public Route GetRoute(BaseTriggerInstance triggerInstance);
        private void StopRoutine();
        private List<BaseTriggerInstance> DetermineRoute(SplineInfo splineInfo, EngineSpeeds throttle, TrackSelection trackSelection, string routeName);
        private List<BaseTriggerInstance> DetermineRoute(TrainController trainController);
        private IEnumerator DetermineAllRoutes();
        private void SortRoutes();
        private void AssignRouteColors();
        private Route FindMatchingRoute(List<BaseTriggerInstance> triggerList);
    }

    private class TrainCarPrefab
    {
        public const string WorkcartAlias;
        public const string ClassicWorkcartPrefab;
        private const string LocomotivePrefab;
        private const string SedanPrefab;
        private const string WorkcartPrefab;
        private const string WorkcartCoveredPrefab;
        private const string WagonAPrefab;
        private const string WagonBPrefab;
        private const string WagonCPrefab;
        private const string WagonFuelPrefab;
        private const string WagonLootPrefab;
        private const string WagonResourcePrefab;
        private const string CaboosePrefab;
        private static readonly Dictionary<string, TrainCarPrefab> AllowedPrefabs;
        public static TrainCarPrefab FindPrefab(string trainCarAlias);
        public static ICollection<string> GetAliases();
        public string TrainCarAlias;
        public string PrefabPath;
        public bool Reverse;
        public TrainCarPrefab(string trainCarAlias, string prefabPath, bool reverse);
    }

    private class DisableSpawnPointManager
    {
        private Dictionary<SpawnGroup, int> _disabledSpawnGroups;
        public IEnumerator DisableSpawnPointsRoutine();
        public void Unload();
    }

    private class ColorMarkerUpdateManager
    {
        private readonly AutomatedWorkcarts _plugin;
        private readonly TrackedCoroutine _trackedCoroutine;
        private Coroutine _coroutine;
        private readonly List<MapMarkerGenericRadius> _colorMarkerList;
        private Configuration _config { get; set; }
        private TrainManager _trainManager { get; set; }
        private TriggerManager _triggerManager { get; set; }
        public ColorMarkerUpdateManager(AutomatedWorkcarts plugin);
        public void Restart();
        public void Unload();
        private void StopCoroutine();
        private IEnumerator ResendColorMarkersRoutine();
    }

    private static readonly Dictionary<string, Quaternion> DungeonRotations;
    private static readonly Dictionary<string, TunnelType> DungeonCellTypes;
    private static readonly Dictionary<TunnelType, Vector3> DungeonCellDimensions;
    private class DungeonCellWrapper
    {
        public static TunnelType GetTunnelType(DungeonGridCell dungeonCell);
        private static TunnelType GetTunnelType(string shortName);
        public static Quaternion GetRotation(string shortName);
        public string ShortName { get; set; }
        public TunnelType TunnelType { get; set; }
        public Vector3 Position { get; set; }
        public Quaternion Rotation { get; set; }
        private OBB _boundingBox;
        public DungeonCellWrapper(DungeonGridCell dungeonCell);
        public Vector3 InverseTransformPoint(Vector3 worldPosition);
        public Vector3 TransformPoint(Vector3 localPosition);
        public bool IsInBounds(Vector3 position);
    }

    private static int EngineThrottleToNumber(EngineSpeeds throttle);
    private static EngineSpeeds EngineThrottleFromNumber(int speedNumber);
    private static int ApplySpeed(int throttle, SpeedInstruction? speedInstruction);
    private static int ApplyDirection(int throttle, DirectionInstruction? directionInstruction);
    private static EngineSpeeds ApplyDirection(EngineSpeeds throttle, DirectionInstruction? directionInstruction);
    private static EngineSpeeds ApplySpeedAndDirection(EngineSpeeds currentThrottle, SpeedInstruction? speedInstruction, DirectionInstruction? directionInstruction);
    private static TrackSelection ApplyTrackSelection(TrackSelection trackSelection, TrackSelectionInstruction? trackSelectionInstruction);
    private class TrainTrigger : TriggerBase
    {
        public static TrainTrigger AddToGameObject(AutomatedWorkcarts plugin, GameObject gameObject, TrainManager trainManager, TriggerData triggerData, BaseTriggerInstance triggerInstance);
        public const int TriggerLayer;
        public const float TriggerRadius;
        public TriggerData TriggerData { get; set; }
        public BaseTriggerInstance TriggerInstance { get; set; }
        private AutomatedWorkcarts _plugin;
        private TrainManager _trainManager;
        public override void OnEntityEnter(BaseEntity entity);
        private bool ShouldAutomateTrain(TrainCar trainCar, bool shouldCount);
        private void HandleTrainCar(TrainCar trainCar);
    }

    private class TriggerData
    {
        [JsonProperty("Id")]
        public int Id;
        [JsonProperty("Position")]
        public Vector3 Position;
        [JsonProperty("Enabled", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(true)]
        public bool Enabled;
        [JsonProperty("Route", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Route;
        [JsonProperty("TunnelType", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string TunnelType;
        [JsonProperty("AddConductor", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool AddConductor;
        [JsonProperty("Brake", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool Brake;
        [JsonProperty("Destroy", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool Destroy;
        [JsonProperty("Direction", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Direction;
        [JsonProperty("Speed", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Speed;
        [JsonProperty("TrackSelection", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string TrackSelection;
        [JsonProperty("StopDuration", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float StopDuration;
        [JsonProperty("DepartureSpeed", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string DepartureSpeed;
        [JsonProperty("Chance", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float Chance;
        [JsonProperty("Commands", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public List<string> Commands;
        [JsonProperty("RotationAngle", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float RotationAngle;
        [JsonProperty("TrainCars", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string[] TrainCars;
        [JsonProperty("Spawner", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool DeprecatedSpawner { get; set; }
        [JsonProperty("Wagons", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private string[] DeprecatedWagons { get; set; }
        [JsonIgnore]
        public bool IsSpawner { get; set; }
        [JsonIgnore]
        public bool IsStop { get; set; }
        [JsonIgnore]
        public TrainTriggerType TriggerType { get; set; }
        public float GetChance();
        public float GetStopDuration();
        private TunnelType? _tunnelType;
        public TunnelType GetTunnelType();
        private SpeedInstruction? _speedInstruction;
        public SpeedInstruction? GetSpeedInstruction();
        public SpeedInstruction GetSpeedInstructionOrZero();
        private DirectionInstruction? _directionInstruction;
        public DirectionInstruction? GetDirectionInstruction();
        private TrackSelectionInstruction? _trackSelectionInstruction;
        public TrackSelectionInstruction? GetTrackSelectionInstruction();
        private SpeedInstruction? _departureSpeedInstruction;
        public SpeedInstruction GetDepartureSpeedInstruction();
        public bool MatchesRoute(string routeName);
        public void InvalidateCache();
        public void CopyFrom(TriggerData triggerData);
        public TriggerData Clone();
        public Color GetColor(string routeName);
    }

    private class SpawnedTrainCarTracker
    {
        private HashSet<TrainCar> _spawnedTrainCars;
        public bool ContainsTrainCar(TrainCar trainCar);
        public void RegisterTrainCar(TrainCar trainCar);
        public void UnregisterTrainCar(TrainCar trainCar);
    }

    private class SpawnedTrainCarComponent : FacepunchBehaviour
    {
        public static void AddToEntity(TrainCar trainCar, BaseTriggerInstance triggerInstance);
        private TrainCar _trainCar;
        private BaseTriggerInstance _triggerInstance;
        private void OnDestroy();
    }

    private abstract class BaseTriggerInstance
    {
        private const int MaxSpawnedTrains;
        private const float TimeBetweenSpawns;
        protected static readonly Vector3 TriggerOffset;
        public TrainManager TrainManager { get; set; }
        public TriggerData TriggerData { get; set; }
        public TrainTrackSpline Spline { get; set; }
        public float DistanceOnSpline { get; set; }
        public MapMarkerGenericRadius ColorMarker { get; set; }
        public abstract Vector3 WorldPosition { get; set; }
        protected abstract Quaternion WorldRotation { get; set; }
        public Vector3 TriggerPosition { get; set; }
        public Quaternion SpawnRotation { get; set; }
        private AutomatedWorkcarts _plugin;
        private GameObject _gameObject;
        private TrainTrigger _trainTrigger;
        private List<TrainCar> _spawnedTrains;
        private VendingMachineMapMarker _vendingMarker;
        private Action _spawnTrainTracked;
        private Configuration _config { get; set; }
        private TriggerManager _triggerManager { get; set; }
        private RouteManager _routeManager { get; set; }
        protected BaseTriggerInstance(AutomatedWorkcarts plugin, TrainManager trainManager, TriggerData triggerData);
        public bool HandleChanges();
        public bool Respawn();
        public void HandleTrainCarKilled(TrainCar trainCar);
        public bool Destroy();
        public bool DidSpawnTrain(TrainCar trainCar);
        private bool IsMapMarkerEligible();
        private Color DetermineMarkerColor();
        private bool EnsureTriggerCreated();
        private void RegisterSpline();
        private void UnregisterSpline();
        private void Move();
        private bool StartSpawningTrains();
        private bool StopSpawningTrains();
        private bool SpawnTrain();
        private void SpawnTrainTracked();
        private bool KillTrains();
        private bool CreateOrUpdateColorMarkerIfNeeded();
        private bool CreateOrUpdateVendingMarkerIfNeeded();
    }

    private class MapTriggerInstance : BaseTriggerInstance
    {
        public override Vector3 WorldPosition { get; set; }
        protected override Quaternion WorldRotation { get; set; }
        public MapTriggerInstance(AutomatedWorkcarts plugin, TrainManager trainManager, TriggerData triggerData);
    }

    private class TunnelTriggerInstance : BaseTriggerInstance
    {
        public DungeonCellWrapper DungeonCellWrapper { get; set; }
        public override Vector3 WorldPosition { get; set; }
        protected override Quaternion WorldRotation { get; set; }
        public TunnelTriggerInstance(AutomatedWorkcarts plugin, TrainManager trainManager, TriggerData triggerData, DungeonCellWrapper dungeonCellWrapper);
    }

    private abstract class BaseTriggerController
    {
        protected TriggerData TriggerData { get; set; }
        public BaseTriggerInstance[] TriggerInstanceList { get; set; }
        protected TrainManager _trainManager;
        protected BaseTriggerController(TrainManager trainManager, TriggerData triggerData);
        public IEnumerator HandleChangesRoutine();
        public void HandleChanges();
        public void Respawn();
        public void Destroy();
        public void GetAllColorMarkers(List<MapMarkerGenericRadius> markerList);
        public BaseTriggerInstance FindNearest(Vector3 position, float maxDistanceSquared, float closestDistanceSquared);
    }

    private sealed class MapTriggerController : BaseTriggerController
    {
        public MapTriggerController(TrainManager trainManager, TriggerData triggerData);
        public void Create(AutomatedWorkcarts plugin);
    }

    private sealed class TunnelTriggerController : BaseTriggerController
    {
        public TunnelTriggerController(TrainManager trainManager, TriggerData triggerData);
        public void Create(AutomatedWorkcarts plugin);
    }

    private class TriggerManager
    {
        private class PlayerInfo
        {
            public Timer Timer;
            public string RouteName;
        }

        private const float TriggerDisplayDuration;
        private const float TriggerDisplayRadius;
        private AutomatedWorkcarts _plugin;
        private TrainManager _trainManager;
        private Dictionary<TriggerData, BaseTriggerController> _triggerControllers;
        private Dictionary<TrainTrackSpline, List<BaseTriggerInstance>> _splinesToTriggers;
        private Dictionary<ulong, PlayerInfo> _playerInfo;
        private Configuration _config { get; set; }
        private StoredTunnelData _tunnelData { get; set; }
        private StoredMapData _mapData { get; set; }
        private RouteManager _routeManager { get; set; }
        private float TriggerDisplayDistanceSquared { get; set; }
        public TriggerManager(AutomatedWorkcarts plugin, TrainManager trainManager);
        public void RegisterTriggerWithSpline(BaseTriggerInstance triggerInstance, TrainTrackSpline spline);
        public void UnregisterTriggerFromSpline(BaseTriggerInstance triggerInstance, TrainTrackSpline spline);
        public List<BaseTriggerInstance> GetTriggersForSpline(TrainTrackSpline spline);
        public TriggerData FindTrigger(int triggerId, TrainTriggerType triggerType);
        public void AddTrigger(TriggerData triggerData);
        public IEnumerator HandleChangesRoutine();
        private void SaveTrigger(TriggerData triggerData);
        private BaseTriggerController GetTriggerController(TriggerData triggerData);
        public void UpdateTrigger(TriggerData triggerData, TriggerData newTriggerData);
        public void MoveTrigger(TriggerData triggerData, Vector3 position);
        public void RotateTrigger(TriggerData triggerData, float rotationAngle);
        public void RespawnTrigger(TriggerData triggerData);
        public void AddTriggerCommand(TriggerData triggerData, string command);
        public void RemoveTriggerCommand(TriggerData triggerData, int index);
        private void DestroyTriggerController(BaseTriggerController triggerController);
        public void RemoveTrigger(TriggerData triggerData);
        public void GetAllColorMarkers(List<MapMarkerGenericRadius> markerList);
        private void CreateMapTriggerController(TriggerData triggerData);
        private void CreateTunnelTriggerController(TriggerData triggerData);
        public IEnumerator CreateAll();
        public void DestroyAll();
        private PlayerInfo GetOrCreatePlayerInfo(BasePlayer player);
        public void SetPlayerDisplayedRoute(BasePlayer player, string routeName);
        public void ShowAllRepeatedly(BasePlayer player, int duration);
        private void ShowNearbyTriggers(BasePlayer player, Vector3 playerPosition, string routeName);
        private void ShowTrigger(BasePlayer player, BaseTriggerInstance trigger, string routeName, int count);
        public BaseTriggerInstance FindNearestTrigger(Vector3 position, float maxDistanceSquared);
        public BaseTriggerInstance FindNearestTrigger(Vector3 position, TriggerData triggerData, float maxDistanceSquared);
        public BaseTriggerInstance FindNearestTriggerWhereAiming(BasePlayer player, float maxDistanceSquared);
    }

    private class TrainManager
    {
        public SpawnedTrainCarTracker SpawnedTrainCarTracker { get; set; }
        private AutomatedWorkcarts _plugin;
        private HashSet<TrainController> _trainControllers;
        private Dictionary<TrainCar, ITrainCarComponent> _trainCarComponents;
        private bool _isUnloading;
        public int TrainCount { get; set; }
        public int CountedConductors { get; set; }
        private Configuration _config { get; set; }
        private StoredPluginData _data { get; set; }
        private RouteManager _routeManager { get; set; }
        public TrainEngine[] GetAutomatedTrainEngines();
        public TrainManager(AutomatedWorkcarts plugin, SpawnedTrainCarTracker spawnedTrainCarTracker);
        public bool CanHaveMoreConductors();
        public List<TrainController> GetAllTrainControllers();
        public TrainController GetTrainController(TrainCar trainCar);
        public bool HasTrainController(TrainCar trainCar);
        public bool TryCreateTrainController(TrainEngine primaryTrainEngine, TriggerData triggerData, TrainEngineData trainEngineData, bool countsTowardConductorLimit);
        public void UnregisterTrainCarComponent(ITrainCarComponent trainCarComponent);
        public void UnregisterTrainController(TrainController trainController);
        public void KillTrainController(TrainCar trainCar);
        public int ResetAll();
        public void Unload();
        public void GetAllColorMarkers(List<MapMarkerGenericRadius> markerList);
        public bool UpdateTrainEngineData();
        public void ShowNearbyTrainStates(BasePlayer player, float maxDistanceSquared, float duration);
    }

    private abstract class TrainState
    {
        protected TrainController _trainController;
        public abstract void Enter();
        public abstract void Exit();
        public abstract Color Color { get; set; }
        protected TrainState(TrainController trainController);
    }

    private class DrivingState : TrainState
    {
        public override Color Color { get; set; }
        public EngineSpeeds Throttle;
        public DrivingState(TrainController trainController, EngineSpeeds throttle);
        public override void Enter();
        public override void Exit();
        public override string ToString();
    }

    private abstract class TransitionState : TrainState
    {
        public override Color Color { get; set; }
        protected readonly TrainState NextState;
        protected TransitionState(TrainController trainController, TrainState nextState);
        public T GetNextStateOfType(bool includingSelf);
        public void SwitchToNextStateOfType();
        protected void SwitchToNextState();
    }

    private class BrakingState : TransitionState
    {
        public override Color Color { get; set; }
        public EngineSpeeds TargetThrottle;
        public bool IsStopping { get; set; }
        public BrakingState(TrainController trainController, TrainState nextState, EngineSpeeds targetThrottle);
        public override void Enter();
        public override void Exit();
        private bool IsNearSpeed(EngineSpeeds desiredThrottle, float leeway);
        private void BrakeUpdate();
        public override string ToString();
    }

    private class IdleState : TransitionState
    {
        private const float MaxDelayMultiplier;
        public override Color Color { get; set; }
        private float _durationSeconds;
        private readonly bool _isIdleDueToCollision;
        private float _startTime;
        public float TimeRemaining { get; set; }
        public float CumulativeTimeRemaining { get; set; }
        public float TimeElapsed { get; set; }
        public IdleState(TrainController trainController, TrainState nextState, float durationSeconds, bool isIdleDueToCollision);
        public override void Enter();
        public override void Exit();
        private void StopIdling();
        public override string ToString();
    }

    private class TrainController
    {
        public const float ConductorTriggerMaxDelay;
        private const float CollisionIdleSeconds;
        public TrainManager TrainManager { get; set; }
        public TrainEngineController PrimaryTrainEngineController { get; set; }
        public TrainState TrainState { get; set; }
        public bool IsDestroying { get; set; }
        public bool CountsTowardConductorLimit { get; set; }
        public float DelaySeconds { get; set; }
        public MapMarkerGenericRadius ColorMarker { get; set; }
        public TrainEngine PrimaryTrainEngine { get; set; }
        public string RouteName { get; set; }
        private Configuration _config { get; set; }
        private RouteManager _routeManager { get; set; }
        public Vector3 Forward { get; set; }
        private bool _isStopped { get; set; }
        private bool _isStopping { get; set; }
        private SpawnedTrainCarTracker _spawnedTrainCarTracker { get; set; }
        private DrivingState _nextDrivingState { get; set; }
        private IdleState _idleState { get; set; }
        private float _cumulativeTimeRemaining { get; set; }
        private float _timeElapsed { get; set; }
        private AutomatedWorkcarts _plugin;
        private readonly List<TrainEngineController> _trainEngineControllers;
        private readonly List<ITrainCarComponent> _trainCarComponents;
        private TrainEngineData _trainEngineData;
        private Func<BasePlayer, bool> _nearbyPlayerFilter;
        private TrainCollisionTrigger _collisionTriggerA;
        private TrainCollisionTrigger _collisionTriggerB;
        private VendingMachineMapMarker _vendingMarker;
        private MapMarker _crateMarker;
        private bool _isDestroyed;
        public EngineSpeeds DepartureThrottle { get; set; }
        public TrainController(AutomatedWorkcarts plugin, TrainManager trainManager, TrainEngineData workcartData, bool countsTowardConductorLimit);
        public override string ToString();
        public void ScheduleCinematicDestruction();
        public void AddTrainCarComponent(ITrainCarComponent trainCarComponent);
        public void HandleTrainCarDestroyed(ITrainCarComponent trainCarComponent);
        public void GetTrainEngines(List<TrainEngine> trainEngineList);
        public void StartTrain();
        public void SetThrottle(EngineSpeeds throttle);
        public void SetTrackSelection(TrackSelection trackSelection);
        public void HandleTrigger(TriggerData triggerData);
        public bool UpdateMarkerColor();
        public void PauseEngine(float scheduleAdjustment);
        public void ReduceDelay(float amount);
        public float DepartEarlyIfStoppedOrStopping();
        public void SwitchState(TrainState nextState);
        public void HandleConductorTrigger(TriggerData triggerData);
        public bool UpdateTrainEngineData();
        public void Kill();
        private Color DetermineMarkerColor();
        private bool IsPlayerOnboardTrain(BasePlayer player);
        private bool NearbyPlayerFilter(BasePlayer player);
        private bool ShouldPlayHorn();
        private void MaybeToggleHorn();
        private void MaybeAddMapMarkers();
        private void EnableInvincibility();
        private void DisableInvincibility();
        private void SetupCollisionTriggers();
        private void DestroyCinematically();
    }

    private class TrainCollisionTrigger : TriggerBase
    {
        public static TrainCollisionTrigger AddToTrigger(AutomatedWorkcarts plugin, TriggerBase hostTrigger, TrainCar trainCar, TrainController trainController);
        public TrainController TrainController { get; set; }
        public TrainCar TrainCar { get; set; }
        private AutomatedWorkcarts _plugin;
        private Configuration _config { get; set; }
        public override void OnEntityEnter(BaseEntity entity);
        private void HandleEntityCollision(BaseEntity entity);
        private void HandleTrainCar(TrainCar otherTrainCar);
    }

    private class TrainCarComponent : FacepunchBehaviour, ITrainCarComponent
    {
        public static TrainCarComponent AddToEntity(TrainCar trainCar, TrainController trainController);
        public TrainController TrainController { get; set; }
        public TrainCar TrainCar { get; set; }
        private void OnDestroy();
    }

    private class TrainEngineController : FacepunchBehaviour, ITrainCarComponent
    {
        public static TrainEngineController AddToEntity(AutomatedWorkcarts plugin, TrainEngine trainEngine, TrainController trainController, bool isReverse);
        public TrainController TrainController { get; set; }
        public TrainEngine TrainEngine { get; set; }
        public Transform Transform { get; set; }
        public NPCShopKeeper Conductor { get; set; }
        public ulong NetId { get; set; }
        public string NetIdString { get; set; }
        private AutomatedWorkcarts _plugin;
        private bool _isReverse;
        public TrainCar TrainCar { get; set; }
        public Vector3 Position { get; set; }
        private Configuration _config { get; set; }
        public void Init(AutomatedWorkcarts plugin, TrainEngine trainEngine, TrainController trainController, bool isReverse);
        public void SetThrottle(EngineSpeeds throttle);
        public void SetTrackSelection(TrackSelection trackSelection);
        private BaseMountable GetDriverSeat();
        private void AddOutfit();
        private void AddConductor();
        private void DisableHazardChecks();
        private void EnableHazardChecks();
        private void EnableUnlimitedFuel();
        private void DisableUnlimitedFuel();
        private void OnDestroy();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class TrainEngineData
    {
        [JsonProperty("Route", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Route;
        [JsonProperty("Throttle", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [JsonConverter(typeof(StringEnumConverter))]
        public EngineSpeeds? Throttle { get; set; }
        [JsonProperty("TrackSelection", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [JsonConverter(typeof(StringEnumConverter))]
        public TrackSelection? TrackSelection { get; set; }
        public bool UpdateData(EngineSpeeds throttle, TrackSelection trackSelection);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class StoredPluginData
    {
        public static StoredPluginData Clear();
        [JsonProperty("AutomatedWorkcardIds", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public HashSet<ulong> AutomatedWorkcartIds;
        [JsonProperty("AutomatedWorkcarts")]
        public Dictionary<ulong, TrainEngineData> AutomatedTrainEngines;
        [JsonIgnore]
        private bool _isDirty;
        public static string Filename { get; set; }
        public static StoredPluginData Load();
        public void Save();
        public void SaveIfDirty();
        public TrainEngineData GetTrainEngineData(ulong trainCarId);
        public void AddTrainEngineId(ulong trainEngineId, TrainEngineData trainEngineData);
        public void RemoveTrainEngineId(ulong trainEngineId);
        public void TrimToTrainEngineIds(HashSet<ulong> foundTrainEngineIds);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class StoredMapData
    {
        [JsonProperty("MapTriggers")]
        public List<TriggerData> MapTriggers;
        private static string GetPerWipeSaveName();
        private static string GetCrossWipeSaveName();
        private static bool IsProcedural();
        private static string GetPerWipeFilePath();
        private static string GetCrossWipeFilePath();
        private static string GetFilepath();
        public static StoredMapData Load();
        public StoredMapData Save();
        public void AddTrigger(TriggerData customTrigger);
        public void RemoveTrigger(TriggerData triggerData);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class StoredTunnelData
    {
        private const float DefaultStationStopDuration;
        private const float DefaultQuickStopDuration;
        private const float DefaultTriggerHeight;
        public static string Filename { get; set; }
        public static StoredTunnelData Load();
        private static bool MigrateToLatest(StoredTunnelData data);
        private static bool MigrateTriggersToMaintenanceTunnels(StoredTunnelData data);
        private static bool MigrateV0ToV1(StoredTunnelData data);
        [JsonProperty("DataFileVersion", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float DataFileVersion;
        [JsonProperty("TunnelTriggers")]
        public List<TriggerData> TunnelTriggers;
        public StoredTunnelData Save();
        public void AddTrigger(TriggerData triggerData);
        public void RemoveTrigger(TriggerData triggerData);
        public static StoredTunnelData GetDefaultData();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class ItemInfo
    {
        [JsonProperty("ShortName")]
        public string ShortName;
        [JsonProperty("Skin")]
        public ulong SkinId;
        [JsonIgnore]
        public ItemDefinition ItemDefinition;
        public void Init();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class ColorMarkerOptions
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Color")]
        public string HexColor;
        [JsonProperty("Alpha")]
        public float Alpha;
        [JsonProperty("Radius")]
        public float Radius;
        [JsonProperty("Use dynamic route color")]
        public bool UseDynamicColor;
        [JsonIgnore]
        public Color Color;
        public bool EnabledAndDynamic { get; set; }
        public void Init();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class VendingMarkerOptions
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Name")]
        public string Name;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class TrainMarkerOptions
    {
        [JsonProperty("Map marker update interval seconds")]
        public float UpdateIntervalSeconds;
        [JsonProperty("Colored map marker")]
        public ColorMarkerOptions ColorMarker;
        [JsonProperty("Vending map marker")]
        public VendingMarkerOptions VendingMarker;
        public void Init();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class StopMarkerOptions
    {
        [JsonProperty("Display only while stop is reachable")]
        public bool DisplayOnlyWhileStopIsReachable;
        [JsonProperty("Colored map marker")]
        public ColorMarkerOptions ColorMarker;
        [JsonProperty("Vending map marker")]
        public VendingMarkerOptions VendingMarker;
        [JsonIgnore]
        private bool AnyMarkersEnabled { get; set; }
        [JsonIgnore]
        public bool AnyDynamicMarkers { get; set; }
        public void Init();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class MarkerOptions
    {
        [JsonProperty("Train map markers")]
        public TrainMarkerOptions Train;
        [JsonProperty("Train stop map markers")]
        public StopMarkerOptions Stop;
        [JsonProperty("Dynamic route colors")]
        public string[] RouteColors;
        [JsonIgnore]
        public Color[] ValidDynamicColors;
        [JsonIgnore]
        public bool AnyColorsEnabled { get; set; }
        [JsonIgnore]
        public bool AnyDynamicColors { get; set; }
        [JsonIgnore]
        public bool AnyDynamicMarkers { get; set; }
        public void Init();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : SerializableConfiguration
    {
        [JsonProperty("PlayHornForNearbyPlayersInRadius")]
        private float DeprecatedPlayHornForNearbyPlayersInRadius { get; set; }
        [JsonProperty("Play horn for nearby players in radius")]
        public float PlayHornForNearbyPlayersInRadius;
        [JsonProperty("DefaultSpeed")]
        private string DeprecatedDefaultSpeed { get; set; }
        [JsonProperty("Default speed")]
        public string DefaultSpeed;
        [JsonProperty("DefaultTrackSelection")]
        private string DeprecatedDefaultTrackSelection { get; set; }
        [JsonProperty("Default track selection")]
        public string DefaultTrackSelection;
        [JsonProperty("BulldozeOffendingWorkcarts")]
        private bool DeprecatedBulldozeOffendingWorkcarts { get; set; }
        [JsonProperty("Bulldoze offending workcarts")]
        public bool BulldozeOffendingWorkcarts;
        [JsonProperty("DestroyBarricadesInstantly")]
        private bool DeprecatedDestroyBarricadesInstantly { get; set; }
        [JsonProperty("Destroy barricades instantly")]
        public bool DestroyBarricadesInstantly;
        [JsonProperty("EnableMapTriggers")]
        private bool DeprecatedEnableMapTriggers { get; set; }
        [JsonProperty("Enable map triggers")]
        public bool EnableMapTriggers;
        [JsonProperty("EnableTunnelTriggers")]
        private Dictionary<string, bool> DeprecatedEnableTunnelTriggers { get; set; }
        [JsonProperty("Enable tunnel triggers")]
        public Dictionary<string, bool> EnableTunnelTriggers;
        [JsonProperty("MaxConductors")]
        private int DeprecatedMaxConductors { get; set; }
        [JsonProperty("Max conductors")]
        public int MaxConductors;
        [JsonProperty("SpawnTriggersRespectConductorLimit")]
        private bool DeprecatedSpawnTriggersRespectConductorLimit { get; set; }
        [JsonProperty("Spawn triggers respect conductor limit")]
        public bool SpawnTriggersRespectConductorLimit;
        [JsonProperty("DisableDefaultTunnelWorkcartSpawnPoints")]
        private bool DeprecatedDisableDefaultTunnelWorkcartSpawnPoints { get; set; }
        [JsonProperty("Disable default tunnel workcart spawn points")]
        public bool DisableDefaultTunnelWorkcartSpawnPoints;
        [JsonProperty("Trigger display distance")]
        public float TriggerDisplayDistance;
        [JsonProperty("Debug show crate markers", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool DebugShowCrateMarkers;
        [JsonProperty("Debug show collisions markers", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool DebugShowCollisionsMarkers;
        [JsonProperty("Debug enable global broadcast", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool DebugEnableGlobalBroadcast;
        [JsonProperty("Debug dynamic routes", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public bool DebugDynamicRoutes;
        [JsonProperty("ConductorOutfit")]
        private ItemInfo[] DeprecatedConductorOutfit { get; set; }
        [JsonProperty("Conductor outfit")]
        public ItemInfo[] ConductorOutfit;
        [JsonProperty("ColoredMapMarker")]
        private ColorMarkerOptions DeprecatedColorMapMarker { get; set; }
        [JsonProperty("VendingMapMarker")]
        private VendingMarkerOptions DeprecatedVendingMapMarker { get; set; }
        [JsonProperty("MapMarkerUpdateInveralSeconds")]
        private float DeprecatedMapMarkerUpdateInteralSeconds { get; set; }
        [JsonProperty("MapMarkerUpdateIntervalSeconds")]
        private float DeprecatedMapMarkerUpdateIntervalSeconds { get; set; }
        [JsonProperty("Map markers")]
        public MarkerOptions MapMarkers;
        [JsonProperty("TriggerDisplayDistance")]
        private float DeprecatedTriggerDisplayDistance { get; set; }
        public void Init();
        public bool IsTunnelTypeEnabled(TunnelType tunnelType);
        private EngineSpeeds? _defaultSpeed;
        public EngineSpeeds GetDefaultSpeed();
        private TrackSelection? _defaultTrackSelection;
        public TrackSelection GetDefaultTrackSelection();
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
    private void ReplyToPlayer(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(BasePlayer player, string messageName, object[] args);
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private string GetMessage(string playerId, string messageName, object[] args);
    private string GetTriggerOptions(IPlayer player);
    private string GetTriggerPrefix(IPlayer player, TrainTriggerType triggerType);
    private string GetTriggerPrefix(IPlayer player, TriggerData triggerData);
    private string GetTriggerPrefix(BasePlayer player, TrainTriggerType triggerType);
    private string GetTriggerPrefix(BasePlayer player, TriggerData triggerData);
    private string GetConductorCountMessage(IPlayer player);
    private static class Lang
    {
        public const string ErrorNoPermission;
        public const string ErrorNoTriggers;
        public const string ErrorTriggerNotFound;
        public const string ErrorNoTrackFound;
        public const string ErrorNoWorkcartFound;
        public const string ErrorNoWorkcart;
        public const string ErrorAutomateBlocked;
        public const string ErrorUnsupportedTunnel;
        public const string ErrorTunnelTypeDisabled;
        public const string ErrorMapTriggersDisabled;
        public const string ErrorMaxConductors;
        public const string ErrorWorkcartOwned;
        public const string ErrorNoAutomatedWorkcarts;
        public const string ErrorRequiresSpawnTrigger;
        public const string ErrorTriggerDisabled;
        public const string ErrorUnrecognizedTrainCar;
        public const string ToggleOnSuccess;
        public const string ToggleOnWithRouteSuccess;
        public const string ToggleOffSuccess;
        public const string ResetAllSuccess;
        public const string ShowTriggersSuccess;
        public const string ShowTriggersWithRouteSuccess;
        public const string AddTriggerSyntax;
        public const string AddTriggerSuccess;
        public const string MoveTriggerSuccess;
        public const string RotateTriggerSuccess;
        public const string UpdateTriggerSyntax;
        public const string UpdateTriggerSuccess;
        public const string SimpleTriggerSyntax;
        public const string RemoveTriggerSuccess;
        public const string AddCommandSyntax;
        public const string RemoveCommandSyntax;
        public const string RemoveCommandErrorIndex;
        public const string InfoConductorCountLimited;
        public const string InfoConductorCountUnlimited;
        public const string HelpSpeedOptions;
        public const string HelpDirectionOptions;
        public const string HelpTrackSelectionOptions;
        public const string HelpTrainCarOptions;
        public const string HelpOtherOptions;
        public const string InfoTrigger;
        public const string InfoTriggerMapPrefix;
        public const string InfoTriggerTunnelPrefix;
        public const string InfoTriggerDisabled;
        public const string InfoTriggerMap;
        public const string InfoTriggerRoute;
        public const string InfoTriggerTunnel;
        public const string InfoTriggerSpawner;
        public const string InfoTriggerAddConductor;
        public const string InfoTriggerDestroy;
        public const string InfoTriggerStopDuration;
        public const string InfoTriggerChance;
        public const string InfoTriggerSpeed;
        public const string InfoTriggerBrakeToSpeed;
        public const string InfoTriggerDepartureSpeed;
        public const string InfoTriggerDirection;
        public const string InfoTriggerDepartureDirection;
        public const string InfoTriggerTrackSelection;
        public const string InfoTriggerCommands;
    }

    protected override void LoadDefaultMessages();
}

private static class ExposedHooks
{
    public static object OnWorkcartAutomationStart(TrainEngine trainEngine);
    public static void OnWorkcartAutomationStarted(TrainEngine trainEngine);
    public static void OnWorkcartAutomationStopped(TrainEngine trainEngine);
}

private class TrackedCoroutine : IEnumerator
{
    private readonly Plugin _plugin;
    private IEnumerator _inner;
    private TrackedCoroutine _innerTracked;
    public TrackedCoroutine(Plugin plugin, IEnumerator inner);
    public object Current { get; set; }
    public bool MoveNext();
    public void Reset();
    public TrackedCoroutine WithEnumerator(IEnumerator inner);
    private TrackedCoroutine GetTrackedCoroutine(IEnumerator enumerator);
}

private static class EntityUtils
{
    public static T CreateEntity(string prefabPath, Vector3 position, Quaternion rotation);
    public static bool KillEntity(BaseEntity entity, BaseNetworkable.DestroyMode destroyMode);
    public static bool MoveEntity(BaseEntity entity, Vector3 position);
}

private static class MarkerUtils
{
    public static MapMarkerGenericRadius CreateColorMarker(Vector3 position, Color color, float radius, float alpha);
    public static MapMarkerGenericRadius CreateColorMarker(ColorMarkerOptions markerOptions, Vector3 position, Color? colorOverride);
    public static VendingMachineMapMarker CreateVendingMarker(VendingMarkerOptions markerOptions, Vector3 position);
    public static bool ResendMarkerColor(MapMarkerGenericRadius colorMarker);
    public static bool UpdateMarkerColor(MapMarkerGenericRadius colorMarker, Color color);
}

private class Route
{
    public readonly List<BaseTriggerInstance> TriggerList;
    public readonly List<TrainController> TrainControllerList;
    public Color Color { get; set; }
    public Route(List<BaseTriggerInstance> triggerList);
    public bool Matches(List<BaseTriggerInstance> triggerList);
    public void SetColor(Color color);
}

private class RouteManager
{
    private readonly AutomatedWorkcarts _plugin;
    private readonly TrackedCoroutine _trackedCoroutine;
    private readonly WaitForSeconds _shortDelay;
    private List<Route> _allRoutes;
    private Dictionary<TrainController, Route> _trainControllerToRoute;
    private Dictionary<BaseTriggerInstance, Route> _triggerInstanceToRoute;
    private Coroutine _determineRoutesRoutine;
    private HashSet<TrainTrackSpline> _reusableSplineList;
    private HashSet<BaseTriggerInstance> _reusableTriggerList;
    private HashSet<BaseTriggerInstance> _reusableTriggerListForSpline;
    private Configuration _config { get; set; }
    private TrainManager _trainManager { get; set; }
    private TriggerManager _triggerManager { get; set; }
    public RouteManager(AutomatedWorkcarts plugin);
    public void Unload();
    public void RecomputeRoutes();
    public Route GetRoute(TrainController trainController);
    public Route GetRoute(BaseTriggerInstance triggerInstance);
    private void StopRoutine();
    private List<BaseTriggerInstance> DetermineRoute(SplineInfo splineInfo, EngineSpeeds throttle, TrackSelection trackSelection, string routeName);
    private List<BaseTriggerInstance> DetermineRoute(TrainController trainController);
    private IEnumerator DetermineAllRoutes();
    private void SortRoutes();
    private void AssignRouteColors();
    private Route FindMatchingRoute(List<BaseTriggerInstance> triggerList);
}

private class TrainCarPrefab
{
    public const string WorkcartAlias;
    public const string ClassicWorkcartPrefab;
    private const string LocomotivePrefab;
    private const string SedanPrefab;
    private const string WorkcartPrefab;
    private const string WorkcartCoveredPrefab;
    private const string WagonAPrefab;
    private const string WagonBPrefab;
    private const string WagonCPrefab;
    private const string WagonFuelPrefab;
    private const string WagonLootPrefab;
    private const string WagonResourcePrefab;
    private const string CaboosePrefab;
    private static readonly Dictionary<string, TrainCarPrefab> AllowedPrefabs;
    public static TrainCarPrefab FindPrefab(string trainCarAlias);
    public static ICollection<string> GetAliases();
    public string TrainCarAlias;
    public string PrefabPath;
    public bool Reverse;
    public TrainCarPrefab(string trainCarAlias, string prefabPath, bool reverse);
}

private class DisableSpawnPointManager
{
    private Dictionary<SpawnGroup, int> _disabledSpawnGroups;
    public IEnumerator DisableSpawnPointsRoutine();
    public void Unload();
}

private class ColorMarkerUpdateManager
{
    private readonly AutomatedWorkcarts _plugin;
    private readonly TrackedCoroutine _trackedCoroutine;
    private Coroutine _coroutine;
    private readonly List<MapMarkerGenericRadius> _colorMarkerList;
    private Configuration _config { get; set; }
    private TrainManager _trainManager { get; set; }
    private TriggerManager _triggerManager { get; set; }
    public ColorMarkerUpdateManager(AutomatedWorkcarts plugin);
    public void Restart();
    public void Unload();
    private void StopCoroutine();
    private IEnumerator ResendColorMarkersRoutine();
}

private class DungeonCellWrapper
{
    public static TunnelType GetTunnelType(DungeonGridCell dungeonCell);
    private static TunnelType GetTunnelType(string shortName);
    public static Quaternion GetRotation(string shortName);
    public string ShortName { get; set; }
    public TunnelType TunnelType { get; set; }
    public Vector3 Position { get; set; }
    public Quaternion Rotation { get; set; }
    private OBB _boundingBox;
    public DungeonCellWrapper(DungeonGridCell dungeonCell);
    public Vector3 InverseTransformPoint(Vector3 worldPosition);
    public Vector3 TransformPoint(Vector3 localPosition);
    public bool IsInBounds(Vector3 position);
}

private class TrainTrigger : TriggerBase
{
    public static TrainTrigger AddToGameObject(AutomatedWorkcarts plugin, GameObject gameObject, TrainManager trainManager, TriggerData triggerData, BaseTriggerInstance triggerInstance);
    public const int TriggerLayer;
    public const float TriggerRadius;
    public TriggerData TriggerData { get; set; }
    public BaseTriggerInstance TriggerInstance { get; set; }
    private AutomatedWorkcarts _plugin;
    private TrainManager _trainManager;
    public override void OnEntityEnter(BaseEntity entity);
    private bool ShouldAutomateTrain(TrainCar trainCar, bool shouldCount);
    private void HandleTrainCar(TrainCar trainCar);
}

private class TriggerData
{
    [JsonProperty("Id")]
    public int Id;
    [JsonProperty("Position")]
    public Vector3 Position;
    [JsonProperty("Enabled", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(true)]
    public bool Enabled;
    [JsonProperty("Route", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Route;
    [JsonProperty("TunnelType", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string TunnelType;
    [JsonProperty("AddConductor", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool AddConductor;
    [JsonProperty("Brake", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool Brake;
    [JsonProperty("Destroy", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool Destroy;
    [JsonProperty("Direction", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Direction;
    [JsonProperty("Speed", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Speed;
    [JsonProperty("TrackSelection", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string TrackSelection;
    [JsonProperty("StopDuration", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float StopDuration;
    [JsonProperty("DepartureSpeed", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string DepartureSpeed;
    [JsonProperty("Chance", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float Chance;
    [JsonProperty("Commands", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public List<string> Commands;
    [JsonProperty("RotationAngle", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float RotationAngle;
    [JsonProperty("TrainCars", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string[] TrainCars;
    [JsonProperty("Spawner", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool DeprecatedSpawner { get; set; }
    [JsonProperty("Wagons", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private string[] DeprecatedWagons { get; set; }
    [JsonIgnore]
    public bool IsSpawner { get; set; }
    [JsonIgnore]
    public bool IsStop { get; set; }
    [JsonIgnore]
    public TrainTriggerType TriggerType { get; set; }
    public float GetChance();
    public float GetStopDuration();
    private TunnelType? _tunnelType;
    public TunnelType GetTunnelType();
    private SpeedInstruction? _speedInstruction;
    public SpeedInstruction? GetSpeedInstruction();
    public SpeedInstruction GetSpeedInstructionOrZero();
    private DirectionInstruction? _directionInstruction;
    public DirectionInstruction? GetDirectionInstruction();
    private TrackSelectionInstruction? _trackSelectionInstruction;
    public TrackSelectionInstruction? GetTrackSelectionInstruction();
    private SpeedInstruction? _departureSpeedInstruction;
    public SpeedInstruction GetDepartureSpeedInstruction();
    public bool MatchesRoute(string routeName);
    public void InvalidateCache();
    public void CopyFrom(TriggerData triggerData);
    public TriggerData Clone();
    public Color GetColor(string routeName);
}

private class SpawnedTrainCarTracker
{
    private HashSet<TrainCar> _spawnedTrainCars;
    public bool ContainsTrainCar(TrainCar trainCar);
    public void RegisterTrainCar(TrainCar trainCar);
    public void UnregisterTrainCar(TrainCar trainCar);
}

private class SpawnedTrainCarComponent : FacepunchBehaviour
{
    public static void AddToEntity(TrainCar trainCar, BaseTriggerInstance triggerInstance);
    private TrainCar _trainCar;
    private BaseTriggerInstance _triggerInstance;
    private void OnDestroy();
}

private abstract class BaseTriggerInstance
{
    private const int MaxSpawnedTrains;
    private const float TimeBetweenSpawns;
    protected static readonly Vector3 TriggerOffset;
    public TrainManager TrainManager { get; set; }
    public TriggerData TriggerData { get; set; }
    public TrainTrackSpline Spline { get; set; }
    public float DistanceOnSpline { get; set; }
    public MapMarkerGenericRadius ColorMarker { get; set; }
    public abstract Vector3 WorldPosition { get; set; }
    protected abstract Quaternion WorldRotation { get; set; }
    public Vector3 TriggerPosition { get; set; }
    public Quaternion SpawnRotation { get; set; }
    private AutomatedWorkcarts _plugin;
    private GameObject _gameObject;
    private TrainTrigger _trainTrigger;
    private List<TrainCar> _spawnedTrains;
    private VendingMachineMapMarker _vendingMarker;
    private Action _spawnTrainTracked;
    private Configuration _config { get; set; }
    private TriggerManager _triggerManager { get; set; }
    private RouteManager _routeManager { get; set; }
    protected BaseTriggerInstance(AutomatedWorkcarts plugin, TrainManager trainManager, TriggerData triggerData);
    public bool HandleChanges();
    public bool Respawn();
    public void HandleTrainCarKilled(TrainCar trainCar);
    public bool Destroy();
    public bool DidSpawnTrain(TrainCar trainCar);
    private bool IsMapMarkerEligible();
    private Color DetermineMarkerColor();
    private bool EnsureTriggerCreated();
    private void RegisterSpline();
    private void UnregisterSpline();
    private void Move();
    private bool StartSpawningTrains();
    private bool StopSpawningTrains();
    private bool SpawnTrain();
    private void SpawnTrainTracked();
    private bool KillTrains();
    private bool CreateOrUpdateColorMarkerIfNeeded();
    private bool CreateOrUpdateVendingMarkerIfNeeded();
}

private class MapTriggerInstance : BaseTriggerInstance
{
    public override Vector3 WorldPosition { get; set; }
    protected override Quaternion WorldRotation { get; set; }
    public MapTriggerInstance(AutomatedWorkcarts plugin, TrainManager trainManager, TriggerData triggerData);
}

private class TunnelTriggerInstance : BaseTriggerInstance
{
    public DungeonCellWrapper DungeonCellWrapper { get; set; }
    public override Vector3 WorldPosition { get; set; }
    protected override Quaternion WorldRotation { get; set; }
    public TunnelTriggerInstance(AutomatedWorkcarts plugin, TrainManager trainManager, TriggerData triggerData, DungeonCellWrapper dungeonCellWrapper);
}

private abstract class BaseTriggerController
{
    protected TriggerData TriggerData { get; set; }
    public BaseTriggerInstance[] TriggerInstanceList { get; set; }
    protected TrainManager _trainManager;
    protected BaseTriggerController(TrainManager trainManager, TriggerData triggerData);
    public IEnumerator HandleChangesRoutine();
    public void HandleChanges();
    public void Respawn();
    public void Destroy();
    public void GetAllColorMarkers(List<MapMarkerGenericRadius> markerList);
    public BaseTriggerInstance FindNearest(Vector3 position, float maxDistanceSquared, float closestDistanceSquared);
}

private sealed class MapTriggerController : BaseTriggerController
{
    public MapTriggerController(TrainManager trainManager, TriggerData triggerData);
    public void Create(AutomatedWorkcarts plugin);
}

private sealed class TunnelTriggerController : BaseTriggerController
{
    public TunnelTriggerController(TrainManager trainManager, TriggerData triggerData);
    public void Create(AutomatedWorkcarts plugin);
}

private class TriggerManager
{
    private class PlayerInfo
    {
        public Timer Timer;
        public string RouteName;
    }

    private const float TriggerDisplayDuration;
    private const float TriggerDisplayRadius;
    private AutomatedWorkcarts _plugin;
    private TrainManager _trainManager;
    private Dictionary<TriggerData, BaseTriggerController> _triggerControllers;
    private Dictionary<TrainTrackSpline, List<BaseTriggerInstance>> _splinesToTriggers;
    private Dictionary<ulong, PlayerInfo> _playerInfo;
    private Configuration _config { get; set; }
    private StoredTunnelData _tunnelData { get; set; }
    private StoredMapData _mapData { get; set; }
    private RouteManager _routeManager { get; set; }
    private float TriggerDisplayDistanceSquared { get; set; }
    public TriggerManager(AutomatedWorkcarts plugin, TrainManager trainManager);
    public void RegisterTriggerWithSpline(BaseTriggerInstance triggerInstance, TrainTrackSpline spline);
    public void UnregisterTriggerFromSpline(BaseTriggerInstance triggerInstance, TrainTrackSpline spline);
    public List<BaseTriggerInstance> GetTriggersForSpline(TrainTrackSpline spline);
    public TriggerData FindTrigger(int triggerId, TrainTriggerType triggerType);
    public void AddTrigger(TriggerData triggerData);
    public IEnumerator HandleChangesRoutine();
    private void SaveTrigger(TriggerData triggerData);
    private BaseTriggerController GetTriggerController(TriggerData triggerData);
    public void UpdateTrigger(TriggerData triggerData, TriggerData newTriggerData);
    public void MoveTrigger(TriggerData triggerData, Vector3 position);
    public void RotateTrigger(TriggerData triggerData, float rotationAngle);
    public void RespawnTrigger(TriggerData triggerData);
    public void AddTriggerCommand(TriggerData triggerData, string command);
    public void RemoveTriggerCommand(TriggerData triggerData, int index);
    private void DestroyTriggerController(BaseTriggerController triggerController);
    public void RemoveTrigger(TriggerData triggerData);
    public void GetAllColorMarkers(List<MapMarkerGenericRadius> markerList);
    private void CreateMapTriggerController(TriggerData triggerData);
    private void CreateTunnelTriggerController(TriggerData triggerData);
    public IEnumerator CreateAll();
    public void DestroyAll();
    private PlayerInfo GetOrCreatePlayerInfo(BasePlayer player);
    public void SetPlayerDisplayedRoute(BasePlayer player, string routeName);
    public void ShowAllRepeatedly(BasePlayer player, int duration);
    private void ShowNearbyTriggers(BasePlayer player, Vector3 playerPosition, string routeName);
    private void ShowTrigger(BasePlayer player, BaseTriggerInstance trigger, string routeName, int count);
    public BaseTriggerInstance FindNearestTrigger(Vector3 position, float maxDistanceSquared);
    public BaseTriggerInstance FindNearestTrigger(Vector3 position, TriggerData triggerData, float maxDistanceSquared);
    public BaseTriggerInstance FindNearestTriggerWhereAiming(BasePlayer player, float maxDistanceSquared);
}

private class PlayerInfo
{
    public Timer Timer;
    public string RouteName;
}

private class TrainManager
{
    public SpawnedTrainCarTracker SpawnedTrainCarTracker { get; set; }
    private AutomatedWorkcarts _plugin;
    private HashSet<TrainController> _trainControllers;
    private Dictionary<TrainCar, ITrainCarComponent> _trainCarComponents;
    private bool _isUnloading;
    public int TrainCount { get; set; }
    public int CountedConductors { get; set; }
    private Configuration _config { get; set; }
    private StoredPluginData _data { get; set; }
    private RouteManager _routeManager { get; set; }
    public TrainEngine[] GetAutomatedTrainEngines();
    public TrainManager(AutomatedWorkcarts plugin, SpawnedTrainCarTracker spawnedTrainCarTracker);
    public bool CanHaveMoreConductors();
    public List<TrainController> GetAllTrainControllers();
    public TrainController GetTrainController(TrainCar trainCar);
    public bool HasTrainController(TrainCar trainCar);
    public bool TryCreateTrainController(TrainEngine primaryTrainEngine, TriggerData triggerData, TrainEngineData trainEngineData, bool countsTowardConductorLimit);
    public void UnregisterTrainCarComponent(ITrainCarComponent trainCarComponent);
    public void UnregisterTrainController(TrainController trainController);
    public void KillTrainController(TrainCar trainCar);
    public int ResetAll();
    public void Unload();
    public void GetAllColorMarkers(List<MapMarkerGenericRadius> markerList);
    public bool UpdateTrainEngineData();
    public void ShowNearbyTrainStates(BasePlayer player, float maxDistanceSquared, float duration);
}

private abstract class TrainState
{
    protected TrainController _trainController;
    public abstract void Enter();
    public abstract void Exit();
    public abstract Color Color { get; set; }
    protected TrainState(TrainController trainController);
}

private class DrivingState : TrainState
{
    public override Color Color { get; set; }
    public EngineSpeeds Throttle;
    public DrivingState(TrainController trainController, EngineSpeeds throttle);
    public override void Enter();
    public override void Exit();
    public override string ToString();
}

private abstract class TransitionState : TrainState
{
    public override Color Color { get; set; }
    protected readonly TrainState NextState;
    protected TransitionState(TrainController trainController, TrainState nextState);
    public T GetNextStateOfType(bool includingSelf);
    public void SwitchToNextStateOfType();
    protected void SwitchToNextState();
}

private class BrakingState : TransitionState
{
    public override Color Color { get; set; }
    public EngineSpeeds TargetThrottle;
    public bool IsStopping { get; set; }
    public BrakingState(TrainController trainController, TrainState nextState, EngineSpeeds targetThrottle);
    public override void Enter();
    public override void Exit();
    private bool IsNearSpeed(EngineSpeeds desiredThrottle, float leeway);
    private void BrakeUpdate();
    public override string ToString();
}

private class IdleState : TransitionState
{
    private const float MaxDelayMultiplier;
    public override Color Color { get; set; }
    private float _durationSeconds;
    private readonly bool _isIdleDueToCollision;
    private float _startTime;
    public float TimeRemaining { get; set; }
    public float CumulativeTimeRemaining { get; set; }
    public float TimeElapsed { get; set; }
    public IdleState(TrainController trainController, TrainState nextState, float durationSeconds, bool isIdleDueToCollision);
    public override void Enter();
    public override void Exit();
    private void StopIdling();
    public override string ToString();
}

private class TrainController
{
    public const float ConductorTriggerMaxDelay;
    private const float CollisionIdleSeconds;
    public TrainManager TrainManager { get; set; }
    public TrainEngineController PrimaryTrainEngineController { get; set; }
    public TrainState TrainState { get; set; }
    public bool IsDestroying { get; set; }
    public bool CountsTowardConductorLimit { get; set; }
    public float DelaySeconds { get; set; }
    public MapMarkerGenericRadius ColorMarker { get; set; }
    public TrainEngine PrimaryTrainEngine { get; set; }
    public string RouteName { get; set; }
    private Configuration _config { get; set; }
    private RouteManager _routeManager { get; set; }
    public Vector3 Forward { get; set; }
    private bool _isStopped { get; set; }
    private bool _isStopping { get; set; }
    private SpawnedTrainCarTracker _spawnedTrainCarTracker { get; set; }
    private DrivingState _nextDrivingState { get; set; }
    private IdleState _idleState { get; set; }
    private float _cumulativeTimeRemaining { get; set; }
    private float _timeElapsed { get; set; }
    private AutomatedWorkcarts _plugin;
    private readonly List<TrainEngineController> _trainEngineControllers;
    private readonly List<ITrainCarComponent> _trainCarComponents;
    private TrainEngineData _trainEngineData;
    private Func<BasePlayer, bool> _nearbyPlayerFilter;
    private TrainCollisionTrigger _collisionTriggerA;
    private TrainCollisionTrigger _collisionTriggerB;
    private VendingMachineMapMarker _vendingMarker;
    private MapMarker _crateMarker;
    private bool _isDestroyed;
    public EngineSpeeds DepartureThrottle { get; set; }
    public TrainController(AutomatedWorkcarts plugin, TrainManager trainManager, TrainEngineData workcartData, bool countsTowardConductorLimit);
    public override string ToString();
    public void ScheduleCinematicDestruction();
    public void AddTrainCarComponent(ITrainCarComponent trainCarComponent);
    public void HandleTrainCarDestroyed(ITrainCarComponent trainCarComponent);
    public void GetTrainEngines(List<TrainEngine> trainEngineList);
    public void StartTrain();
    public void SetThrottle(EngineSpeeds throttle);
    public void SetTrackSelection(TrackSelection trackSelection);
    public void HandleTrigger(TriggerData triggerData);
    public bool UpdateMarkerColor();
    public void PauseEngine(float scheduleAdjustment);
    public void ReduceDelay(float amount);
    public float DepartEarlyIfStoppedOrStopping();
    public void SwitchState(TrainState nextState);
    public void HandleConductorTrigger(TriggerData triggerData);
    public bool UpdateTrainEngineData();
    public void Kill();
    private Color DetermineMarkerColor();
    private bool IsPlayerOnboardTrain(BasePlayer player);
    private bool NearbyPlayerFilter(BasePlayer player);
    private bool ShouldPlayHorn();
    private void MaybeToggleHorn();
    private void MaybeAddMapMarkers();
    private void EnableInvincibility();
    private void DisableInvincibility();
    private void SetupCollisionTriggers();
    private void DestroyCinematically();
}

private class TrainCollisionTrigger : TriggerBase
{
    public static TrainCollisionTrigger AddToTrigger(AutomatedWorkcarts plugin, TriggerBase hostTrigger, TrainCar trainCar, TrainController trainController);
    public TrainController TrainController { get; set; }
    public TrainCar TrainCar { get; set; }
    private AutomatedWorkcarts _plugin;
    private Configuration _config { get; set; }
    public override void OnEntityEnter(BaseEntity entity);
    private void HandleEntityCollision(BaseEntity entity);
    private void HandleTrainCar(TrainCar otherTrainCar);
}

private class TrainCarComponent : FacepunchBehaviour, ITrainCarComponent
{
    public static TrainCarComponent AddToEntity(TrainCar trainCar, TrainController trainController);
    public TrainController TrainController { get; set; }
    public TrainCar TrainCar { get; set; }
    private void OnDestroy();
}

private class TrainEngineController : FacepunchBehaviour, ITrainCarComponent
{
    public static TrainEngineController AddToEntity(AutomatedWorkcarts plugin, TrainEngine trainEngine, TrainController trainController, bool isReverse);
    public TrainController TrainController { get; set; }
    public TrainEngine TrainEngine { get; set; }
    public Transform Transform { get; set; }
    public NPCShopKeeper Conductor { get; set; }
    public ulong NetId { get; set; }
    public string NetIdString { get; set; }
    private AutomatedWorkcarts _plugin;
    private bool _isReverse;
    public TrainCar TrainCar { get; set; }
    public Vector3 Position { get; set; }
    private Configuration _config { get; set; }
    public void Init(AutomatedWorkcarts plugin, TrainEngine trainEngine, TrainController trainController, bool isReverse);
    public void SetThrottle(EngineSpeeds throttle);
    public void SetTrackSelection(TrackSelection trackSelection);
    private BaseMountable GetDriverSeat();
    private void AddOutfit();
    private void AddConductor();
    private void DisableHazardChecks();
    private void EnableHazardChecks();
    private void EnableUnlimitedFuel();
    private void DisableUnlimitedFuel();
    private void OnDestroy();
}

[JsonObject(MemberSerialization.OptIn)]
private class TrainEngineData
{
    [JsonProperty("Route", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Route;
    [JsonProperty("Throttle", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [JsonConverter(typeof(StringEnumConverter))]
    public EngineSpeeds? Throttle { get; set; }
    [JsonProperty("TrackSelection", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [JsonConverter(typeof(StringEnumConverter))]
    public TrackSelection? TrackSelection { get; set; }
    public bool UpdateData(EngineSpeeds throttle, TrackSelection trackSelection);
}

[JsonObject(MemberSerialization.OptIn)]
private class StoredPluginData
{
    public static StoredPluginData Clear();
    [JsonProperty("AutomatedWorkcardIds", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public HashSet<ulong> AutomatedWorkcartIds;
    [JsonProperty("AutomatedWorkcarts")]
    public Dictionary<ulong, TrainEngineData> AutomatedTrainEngines;
    [JsonIgnore]
    private bool _isDirty;
    public static string Filename { get; set; }
    public static StoredPluginData Load();
    public void Save();
    public void SaveIfDirty();
    public TrainEngineData GetTrainEngineData(ulong trainCarId);
    public void AddTrainEngineId(ulong trainEngineId, TrainEngineData trainEngineData);
    public void RemoveTrainEngineId(ulong trainEngineId);
    public void TrimToTrainEngineIds(HashSet<ulong> foundTrainEngineIds);
}

[JsonObject(MemberSerialization.OptIn)]
private class StoredMapData
{
    [JsonProperty("MapTriggers")]
    public List<TriggerData> MapTriggers;
    private static string GetPerWipeSaveName();
    private static string GetCrossWipeSaveName();
    private static bool IsProcedural();
    private static string GetPerWipeFilePath();
    private static string GetCrossWipeFilePath();
    private static string GetFilepath();
    public static StoredMapData Load();
    public StoredMapData Save();
    public void AddTrigger(TriggerData customTrigger);
    public void RemoveTrigger(TriggerData triggerData);
}

[JsonObject(MemberSerialization.OptIn)]
private class StoredTunnelData
{
    private const float DefaultStationStopDuration;
    private const float DefaultQuickStopDuration;
    private const float DefaultTriggerHeight;
    public static string Filename { get; set; }
    public static StoredTunnelData Load();
    private static bool MigrateToLatest(StoredTunnelData data);
    private static bool MigrateTriggersToMaintenanceTunnels(StoredTunnelData data);
    private static bool MigrateV0ToV1(StoredTunnelData data);
    [JsonProperty("DataFileVersion", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float DataFileVersion;
    [JsonProperty("TunnelTriggers")]
    public List<TriggerData> TunnelTriggers;
    public StoredTunnelData Save();
    public void AddTrigger(TriggerData triggerData);
    public void RemoveTrigger(TriggerData triggerData);
    public static StoredTunnelData GetDefaultData();
}

[JsonObject(MemberSerialization.OptIn)]
private class ItemInfo
{
    [JsonProperty("ShortName")]
    public string ShortName;
    [JsonProperty("Skin")]
    public ulong SkinId;
    [JsonIgnore]
    public ItemDefinition ItemDefinition;
    public void Init();
}

[JsonObject(MemberSerialization.OptIn)]
private class ColorMarkerOptions
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Color")]
    public string HexColor;
    [JsonProperty("Alpha")]
    public float Alpha;
    [JsonProperty("Radius")]
    public float Radius;
    [JsonProperty("Use dynamic route color")]
    public bool UseDynamicColor;
    [JsonIgnore]
    public Color Color;
    public bool EnabledAndDynamic { get; set; }
    public void Init();
}

[JsonObject(MemberSerialization.OptIn)]
private class VendingMarkerOptions
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Name")]
    public string Name;
}

[JsonObject(MemberSerialization.OptIn)]
private class TrainMarkerOptions
{
    [JsonProperty("Map marker update interval seconds")]
    public float UpdateIntervalSeconds;
    [JsonProperty("Colored map marker")]
    public ColorMarkerOptions ColorMarker;
    [JsonProperty("Vending map marker")]
    public VendingMarkerOptions VendingMarker;
    public void Init();
}

[JsonObject(MemberSerialization.OptIn)]
private class StopMarkerOptions
{
    [JsonProperty("Display only while stop is reachable")]
    public bool DisplayOnlyWhileStopIsReachable;
    [JsonProperty("Colored map marker")]
    public ColorMarkerOptions ColorMarker;
    [JsonProperty("Vending map marker")]
    public VendingMarkerOptions VendingMarker;
    [JsonIgnore]
    private bool AnyMarkersEnabled { get; set; }
    [JsonIgnore]
    public bool AnyDynamicMarkers { get; set; }
    public void Init();
}

[JsonObject(MemberSerialization.OptIn)]
private class MarkerOptions
{
    [JsonProperty("Train map markers")]
    public TrainMarkerOptions Train;
    [JsonProperty("Train stop map markers")]
    public StopMarkerOptions Stop;
    [JsonProperty("Dynamic route colors")]
    public string[] RouteColors;
    [JsonIgnore]
    public Color[] ValidDynamicColors;
    [JsonIgnore]
    public bool AnyColorsEnabled { get; set; }
    [JsonIgnore]
    public bool AnyDynamicColors { get; set; }
    [JsonIgnore]
    public bool AnyDynamicMarkers { get; set; }
    public void Init();
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : SerializableConfiguration
{
    [JsonProperty("PlayHornForNearbyPlayersInRadius")]
    private float DeprecatedPlayHornForNearbyPlayersInRadius { get; set; }
    [JsonProperty("Play horn for nearby players in radius")]
    public float PlayHornForNearbyPlayersInRadius;
    [JsonProperty("DefaultSpeed")]
    private string DeprecatedDefaultSpeed { get; set; }
    [JsonProperty("Default speed")]
    public string DefaultSpeed;
    [JsonProperty("DefaultTrackSelection")]
    private string DeprecatedDefaultTrackSelection { get; set; }
    [JsonProperty("Default track selection")]
    public string DefaultTrackSelection;
    [JsonProperty("BulldozeOffendingWorkcarts")]
    private bool DeprecatedBulldozeOffendingWorkcarts { get; set; }
    [JsonProperty("Bulldoze offending workcarts")]
    public bool BulldozeOffendingWorkcarts;
    [JsonProperty("DestroyBarricadesInstantly")]
    private bool DeprecatedDestroyBarricadesInstantly { get; set; }
    [JsonProperty("Destroy barricades instantly")]
    public bool DestroyBarricadesInstantly;
    [JsonProperty("EnableMapTriggers")]
    private bool DeprecatedEnableMapTriggers { get; set; }
    [JsonProperty("Enable map triggers")]
    public bool EnableMapTriggers;
    [JsonProperty("EnableTunnelTriggers")]
    private Dictionary<string, bool> DeprecatedEnableTunnelTriggers { get; set; }
    [JsonProperty("Enable tunnel triggers")]
    public Dictionary<string, bool> EnableTunnelTriggers;
    [JsonProperty("MaxConductors")]
    private int DeprecatedMaxConductors { get; set; }
    [JsonProperty("Max conductors")]
    public int MaxConductors;
    [JsonProperty("SpawnTriggersRespectConductorLimit")]
    private bool DeprecatedSpawnTriggersRespectConductorLimit { get; set; }
    [JsonProperty("Spawn triggers respect conductor limit")]
    public bool SpawnTriggersRespectConductorLimit;
    [JsonProperty("DisableDefaultTunnelWorkcartSpawnPoints")]
    private bool DeprecatedDisableDefaultTunnelWorkcartSpawnPoints { get; set; }
    [JsonProperty("Disable default tunnel workcart spawn points")]
    public bool DisableDefaultTunnelWorkcartSpawnPoints;
    [JsonProperty("Trigger display distance")]
    public float TriggerDisplayDistance;
    [JsonProperty("Debug show crate markers", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool DebugShowCrateMarkers;
    [JsonProperty("Debug show collisions markers", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool DebugShowCollisionsMarkers;
    [JsonProperty("Debug enable global broadcast", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool DebugEnableGlobalBroadcast;
    [JsonProperty("Debug dynamic routes", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public bool DebugDynamicRoutes;
    [JsonProperty("ConductorOutfit")]
    private ItemInfo[] DeprecatedConductorOutfit { get; set; }
    [JsonProperty("Conductor outfit")]
    public ItemInfo[] ConductorOutfit;
    [JsonProperty("ColoredMapMarker")]
    private ColorMarkerOptions DeprecatedColorMapMarker { get; set; }
    [JsonProperty("VendingMapMarker")]
    private VendingMarkerOptions DeprecatedVendingMapMarker { get; set; }
    [JsonProperty("MapMarkerUpdateInveralSeconds")]
    private float DeprecatedMapMarkerUpdateInteralSeconds { get; set; }
    [JsonProperty("MapMarkerUpdateIntervalSeconds")]
    private float DeprecatedMapMarkerUpdateIntervalSeconds { get; set; }
    [JsonProperty("Map markers")]
    public MarkerOptions MapMarkers;
    [JsonProperty("TriggerDisplayDistance")]
    private float DeprecatedTriggerDisplayDistance { get; set; }
    public void Init();
    public bool IsTunnelTypeEnabled(TunnelType tunnelType);
    private EngineSpeeds? _defaultSpeed;
    public EngineSpeeds GetDefaultSpeed();
    private TrackSelection? _defaultTrackSelection;
    public TrackSelection GetDefaultTrackSelection();
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
    public const string ErrorNoPermission;
    public const string ErrorNoTriggers;
    public const string ErrorTriggerNotFound;
    public const string ErrorNoTrackFound;
    public const string ErrorNoWorkcartFound;
    public const string ErrorNoWorkcart;
    public const string ErrorAutomateBlocked;
    public const string ErrorUnsupportedTunnel;
    public const string ErrorTunnelTypeDisabled;
    public const string ErrorMapTriggersDisabled;
    public const string ErrorMaxConductors;
    public const string ErrorWorkcartOwned;
    public const string ErrorNoAutomatedWorkcarts;
    public const string ErrorRequiresSpawnTrigger;
    public const string ErrorTriggerDisabled;
    public const string ErrorUnrecognizedTrainCar;
    public const string ToggleOnSuccess;
    public const string ToggleOnWithRouteSuccess;
    public const string ToggleOffSuccess;
    public const string ResetAllSuccess;
    public const string ShowTriggersSuccess;
    public const string ShowTriggersWithRouteSuccess;
    public const string AddTriggerSyntax;
    public const string AddTriggerSuccess;
    public const string MoveTriggerSuccess;
    public const string RotateTriggerSuccess;
    public const string UpdateTriggerSyntax;
    public const string UpdateTriggerSuccess;
    public const string SimpleTriggerSyntax;
    public const string RemoveTriggerSuccess;
    public const string AddCommandSyntax;
    public const string RemoveCommandSyntax;
    public const string RemoveCommandErrorIndex;
    public const string InfoConductorCountLimited;
    public const string InfoConductorCountUnlimited;
    public const string HelpSpeedOptions;
    public const string HelpDirectionOptions;
    public const string HelpTrackSelectionOptions;
    public const string HelpTrainCarOptions;
    public const string HelpOtherOptions;
    public const string InfoTrigger;
    public const string InfoTriggerMapPrefix;
    public const string InfoTriggerTunnelPrefix;
    public const string InfoTriggerDisabled;
    public const string InfoTriggerMap;
    public const string InfoTriggerRoute;
    public const string InfoTriggerTunnel;
    public const string InfoTriggerSpawner;
    public const string InfoTriggerAddConductor;
    public const string InfoTriggerDestroy;
    public const string InfoTriggerStopDuration;
    public const string InfoTriggerChance;
    public const string InfoTriggerSpeed;
    public const string InfoTriggerBrakeToSpeed;
    public const string InfoTriggerDepartureSpeed;
    public const string InfoTriggerDirection;
    public const string InfoTriggerDepartureDirection;
    public const string InfoTriggerTrackSelection;
    public const string InfoTriggerCommands;
}


```

---

## AutomaticAuthorization by  - Automatically add your teammates/friends/clanmates to a cupboard/turret/lock auth list

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Serialization;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust;
using Oxide.Game.Rust.Cui;
using ProtoBuf;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Automatic Authorization", "k1lly0u/Arainrr", "1.3.3", ResourceId = 2063)]
[Description("Shared cupboards, turrets, locks with teams, clans, friends")]
public class AutomaticAuthorization : RustPlugin
{
    [PluginReference]
    private readonly Plugin Clans;
    private readonly Plugin Friends;
    private readonly Plugin Bank;
    private const string PERMISSION_USE;
    private readonly object _true;
    private readonly Dictionary<ulong, EntityCache> _playerEntities;
    private class EntityCache
    {
        public readonly HashSet<AutoTurret> autoTurrets;
        public readonly HashSet<BuildingPrivlidge> buildingPrivlidges;
        public readonly HashSet<CodeLock> codeLocks;
    }

    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private void OnEntitySpawned(AutoTurret autoTurret);
    private void OnEntitySpawned(BuildingPrivlidge buildingPrivlidge);
    private void OnEntitySpawned(CodeLock codeLock);
    private void OnEntityKill(AutoTurret autoTurret);
    private void OnEntityKill(BuildingPrivlidge buildingPrivlidge);
    private void OnEntityKill(CodeLock codeLock);
    private void CanChangeCode(BasePlayer player, CodeLock codeLock, string code, bool isGuest);
    private object CanUseLockedEntity(BasePlayer player, BaseLock baseLock);
    private static bool CanShareLockedEntity(BaseEntity parentEntity, StoredData.LockShareEntry lockShareEntry);
    private static void SendUnlockedEffect(CodeLock codeLock);
    private static void CheckShareData(StoredData.ShareEntry shareEntry, ConfigData.ShareSettings shareSettings);
    private void CheckEntitySpawned(AutoTurret autoTurret, bool justCreated);
    private void CheckEntitySpawned(BuildingPrivlidge buildingPrivlidge, bool justCreated);
    private void CheckEntitySpawned(CodeLock codeLock, bool justCreated);
    private void CheckEntityKill(AutoTurret autoTurret);
    private void CheckEntityKill(BuildingPrivlidge buildingPrivlidge);
    private void CheckEntityKill(CodeLock codeLock);
    private void UpdateAuthList(ulong playerId, AutoAuthType autoAuthType);
    private void AuthToTurret(HashSet<AutoTurret> autoTurrets, ulong playerId, bool justCreated);
    private void AuthToCupboard(HashSet<BuildingPrivlidge> buildingPrivlidges, ulong playerId, bool justCreated);
    private void AuthToCodeLock(HashSet<CodeLock> codeLocks, ulong playerId, bool justCreated);
    private List<PlayerNameID> GetAuthorNameIDs(ulong playerId, AutoAuthType autoAuthType);
    private HashSet<ulong> GetAuthorIDsForCodeLock(ulong playerId, CodeLock codeLock);
    private HashSet<ulong> GetAuthIds(ulong playerId, AutoAuthType autoAuthType);
    private bool CanSharingLock(BaseLock baseLock, BaseEntity parentEntity, StoredData.ShareData shareData, ShareType shareType, ulong ownerId, ulong playerId);
    private IEnumerable<ShareType> GetAvailableTypes();
    private bool IsShareTypeEnabled(ShareType shareType);
    private bool AreFriends(ShareType shareType, ulong ownerId, ulong playerId);
    private StoredData.ShareData _defaultData;
    private StoredData.ShareData DefaultData { get; set; }
    private StoredData.ShareData GetShareData(ulong playerId, bool readOnly);
    private StoredData.ShareData CreateDefaultData();
    private void UpdateData();
    private StoredData.ShareEntry CreateShareEntry(ShareType shareType);
    private void CheckShareData(StoredData.ShareData shareData, ShareType shareType);
    private bool IsBankBox(BaseNetworkable entity);
    private void OnTeamAcceptInvite(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private void OnTeamLeave(RelationshipManager.PlayerTeam playerTeam, BasePlayer player);
    private void OnTeamKick(RelationshipManager.PlayerTeam playerTeam, BasePlayer leader, ulong target);
    private void OnTeamDisbanded(RelationshipManager.PlayerTeam playerTeam);
    private void UpdateTeamAuthList(List<ulong> teamMembers);
    private static IEnumerable<ulong> GetTeamMembers(ulong playerId);
    private static bool SameTeam(ulong playerId, ulong friendId);
    private void OnFriendAdded(string playerId, string friendId);
    private void OnFriendRemoved(string playerId, string friendId);
    private void UpdateFriendAuthList(string playerId, string friendId);
    private IEnumerable<ulong> GetFriends(ulong playerId);
    private bool HasFriend(ulong playerId, ulong friendId);
    private void OnClanDestroy(string clanName);
    private void OnClanUpdate(string clanName);
    private void OnClanMemberGone(string playerId, List<string> memberUserIDs);
    private void UpdateClanAuthList(string clanName);
    private IEnumerable<ulong> GetClanMembers(ulong playerId);
    private IEnumerable<ulong> GetClanMembers(string clanName);
    private bool SameClan(ulong playerId, ulong friendId);
    private const string UINAME_MAIN;
    private const string UINAME_MENU;
    private void CreateMainUI(BasePlayer player);
    private void UpdateMenuUI(BasePlayer player, StoredData.ShareData shareData, ShareType shareType);
    private void CreateMenuSubUI(CuiElementContainer container, StoredData.ShareData shareData, string playerId, ShareType shareType, string anchorMin, string anchorMax);
    private static void CreateEntrySubUI(CuiElementContainer container, string parentName, string command, string leftText, string rightText, string anchorMin, string anchorMax);
    private static float[] GetEntryAnchors(int i, float entrySize, float spacingY);
    private static float[] GetMenuSubAnchors(int i, int total);
    private static void DestroyUI(BasePlayer player);
    private void CmdAutoAuth(BasePlayer player, string command, string[] args);
    private void HandleStatusCommand(StringBuilder stringBuilder, BasePlayer player, StoredData.ShareData shareData, IEnumerable<ShareType> availableTypes);
    private void HandleHelpCommand(StringBuilder stringBuilder, BasePlayer player, IEnumerable<ShareType> availableTypes);
    private void HandleHelpCommand(StringBuilder stringBuilder, BasePlayer player, ShareType shareType);
    private bool HandleShareCommand(BasePlayer player, StoredData.ShareData shareData, ShareType shareType, string[] args, bool sendMsg);
    private void CmdAutoAuthUI(BasePlayer player, string command, string[] args);
    [ConsoleCommand("AutoAuthUI")]
    private void CCmdAutoAuthUI(ConsoleSystem.Arg arg);
    private void HandleShareUICommand(BasePlayer player, StoredData.ShareData shareData, ShareType shareType, string[] args);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Clear Share Data On Map Wipe")]
        public bool ClearDataOnWipe { get; set; }
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings Chat { get; set; }
        [JsonProperty(PropertyName = "Teams Share Settings")]
        public ShareSettings TeamsShare { get; set; }
        [JsonProperty(PropertyName = "Friends Share Settings")]
        public ShareSettings FriendsShare { get; set; }
        [JsonProperty(PropertyName = "Clans Share Settings")]
        public ShareSettings ClansShare { get; set; }
        [JsonProperty(PropertyName = "Default Share Settings")]
        public Dictionary<ShareType, ShareSettings> DefaultShare { get; set; }
        [JsonProperty(PropertyName = "Version")]
        public VersionNumber Version { get; set; }
        public class ShareSettings
        {
            [JsonProperty(PropertyName = "Enabled")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Share Cupboard")]
            public bool ShareCupboard { get; set; }
            [JsonProperty(PropertyName = "Share Turret")]
            public bool ShareTurret { get; set; }
            [JsonProperty(PropertyName = "Key Lock Settings")]
            public LockSettings KeyLock { get; set; }
            [JsonProperty(PropertyName = "Code Lock Settings")]
            public LockSettings CodeLock { get; set; }
        }

        public class LockSettings
        {
            [JsonProperty(PropertyName = "Enabled")]
            public bool Enabled { get; set; }
            [JsonProperty(PropertyName = "Share Door")]
            public bool ShareDoor { get; set; }
            [JsonProperty(PropertyName = "Share Box")]
            public bool ShareBox { get; set; }
            [JsonProperty(PropertyName = "Share Other Locked Entities")]
            public bool ShareOtherEntity { get; set; }
        }

        public ShareSettings GetShareSettings(ShareType shareType);
        public ShareSettings GetDefaultShareSettings(ShareType shareType);
    }

    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Send Authorization Success Message")]
        public bool SendMessage { get; set; }
        [JsonProperty(PropertyName = "Chat Command")]
        public string ChatCommand { get; set; }
        [JsonProperty(PropertyName = "Chat UI Command")]
        public string UICommand { get; set; }
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong SteamIdIcon { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private StoredData storedData;
    private class StoredData
    {
        [JsonProperty(PropertyName = "shareData")]
        public Dictionary<ulong, ShareData> playerShareData;
        public class ShareData
        {
            [JsonProperty(PropertyName = "t")]
            public ShareEntry teamsShare;
            [JsonProperty(PropertyName = "f")]
            public ShareEntry friendsShare;
            [JsonProperty(PropertyName = "c")]
            public ShareEntry clansShare;
            public ShareEntry GetShareEntry(ShareType shareType);
        }

        public class ShareDataContractResolver : DefaultContractResolver
        {
            private readonly List<string> _excludedProperties;
            public ShareDataContractResolver(bool teams, bool friends, bool clans);
            protected override IList<JsonProperty> CreateProperties(Type type, MemberSerialization memberSerialization);
        }

        [JsonConverter(typeof(ShareEntryConverter))]
        public class ShareEntry
        {
            public bool enabled;
            public bool cupboard;
            public bool turret;
            public LockShareEntry keyLock;
            public LockShareEntry codeLock;
            public string Write();
            public static ShareEntry Read(string json);
        }

        public class LockShareEntry
        {
            public bool enabled;
            public bool door;
            public bool box;
            public bool other;
        }

        private class ShareEntryConverter : JsonConverter
        {
            public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
            public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
            public override bool CanConvert(Type objectType);
        }

    }

    private void LoadData();
    private void ClearData();
    private void SaveData();
    private void OnNewSave(string filename);
    private void UpdateOldData();
    private class OldStoredData
    {
        public readonly Dictionary<ulong, OldShareData> playerShareData;
        public class OldShareData
        {
            public OldShareEntry teamShare;
            public OldShareEntry friendsShare;
            public OldShareEntry clanShare;
            public JObject GetNewData();
        }

        public class OldShareEntry
        {
            public bool enabled;
            public bool cupboard;
            public bool turret;
            public bool keyLock;
            public bool codeLock;
            public string Write();
        }

    }

    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class EntityCache
{
    public readonly HashSet<AutoTurret> autoTurrets;
    public readonly HashSet<BuildingPrivlidge> buildingPrivlidges;
    public readonly HashSet<CodeLock> codeLocks;
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Clear Share Data On Map Wipe")]
    public bool ClearDataOnWipe { get; set; }
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings Chat { get; set; }
    [JsonProperty(PropertyName = "Teams Share Settings")]
    public ShareSettings TeamsShare { get; set; }
    [JsonProperty(PropertyName = "Friends Share Settings")]
    public ShareSettings FriendsShare { get; set; }
    [JsonProperty(PropertyName = "Clans Share Settings")]
    public ShareSettings ClansShare { get; set; }
    [JsonProperty(PropertyName = "Default Share Settings")]
    public Dictionary<ShareType, ShareSettings> DefaultShare { get; set; }
    [JsonProperty(PropertyName = "Version")]
    public VersionNumber Version { get; set; }
    public class ShareSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Share Cupboard")]
        public bool ShareCupboard { get; set; }
        [JsonProperty(PropertyName = "Share Turret")]
        public bool ShareTurret { get; set; }
        [JsonProperty(PropertyName = "Key Lock Settings")]
        public LockSettings KeyLock { get; set; }
        [JsonProperty(PropertyName = "Code Lock Settings")]
        public LockSettings CodeLock { get; set; }
    }

    public class LockSettings
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool Enabled { get; set; }
        [JsonProperty(PropertyName = "Share Door")]
        public bool ShareDoor { get; set; }
        [JsonProperty(PropertyName = "Share Box")]
        public bool ShareBox { get; set; }
        [JsonProperty(PropertyName = "Share Other Locked Entities")]
        public bool ShareOtherEntity { get; set; }
    }

    public ShareSettings GetShareSettings(ShareType shareType);
    public ShareSettings GetDefaultShareSettings(ShareType shareType);
}

public class ShareSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Share Cupboard")]
    public bool ShareCupboard { get; set; }
    [JsonProperty(PropertyName = "Share Turret")]
    public bool ShareTurret { get; set; }
    [JsonProperty(PropertyName = "Key Lock Settings")]
    public LockSettings KeyLock { get; set; }
    [JsonProperty(PropertyName = "Code Lock Settings")]
    public LockSettings CodeLock { get; set; }
}

public class LockSettings
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool Enabled { get; set; }
    [JsonProperty(PropertyName = "Share Door")]
    public bool ShareDoor { get; set; }
    [JsonProperty(PropertyName = "Share Box")]
    public bool ShareBox { get; set; }
    [JsonProperty(PropertyName = "Share Other Locked Entities")]
    public bool ShareOtherEntity { get; set; }
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Send Authorization Success Message")]
    public bool SendMessage { get; set; }
    [JsonProperty(PropertyName = "Chat Command")]
    public string ChatCommand { get; set; }
    [JsonProperty(PropertyName = "Chat UI Command")]
    public string UICommand { get; set; }
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong SteamIdIcon { get; set; }
}

private class StoredData
{
    [JsonProperty(PropertyName = "shareData")]
    public Dictionary<ulong, ShareData> playerShareData;
    public class ShareData
    {
        [JsonProperty(PropertyName = "t")]
        public ShareEntry teamsShare;
        [JsonProperty(PropertyName = "f")]
        public ShareEntry friendsShare;
        [JsonProperty(PropertyName = "c")]
        public ShareEntry clansShare;
        public ShareEntry GetShareEntry(ShareType shareType);
    }

    public class ShareDataContractResolver : DefaultContractResolver
    {
        private readonly List<string> _excludedProperties;
        public ShareDataContractResolver(bool teams, bool friends, bool clans);
        protected override IList<JsonProperty> CreateProperties(Type type, MemberSerialization memberSerialization);
    }

    [JsonConverter(typeof(ShareEntryConverter))]
    public class ShareEntry
    {
        public bool enabled;
        public bool cupboard;
        public bool turret;
        public LockShareEntry keyLock;
        public LockShareEntry codeLock;
        public string Write();
        public static ShareEntry Read(string json);
    }

    public class LockShareEntry
    {
        public bool enabled;
        public bool door;
        public bool box;
        public bool other;
    }

    private class ShareEntryConverter : JsonConverter
    {
        public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
        public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
        public override bool CanConvert(Type objectType);
    }

}

public class ShareData
{
    [JsonProperty(PropertyName = "t")]
    public ShareEntry teamsShare;
    [JsonProperty(PropertyName = "f")]
    public ShareEntry friendsShare;
    [JsonProperty(PropertyName = "c")]
    public ShareEntry clansShare;
    public ShareEntry GetShareEntry(ShareType shareType);
}

public class ShareDataContractResolver : DefaultContractResolver
{
    private readonly List<string> _excludedProperties;
    public ShareDataContractResolver(bool teams, bool friends, bool clans);
    protected override IList<JsonProperty> CreateProperties(Type type, MemberSerialization memberSerialization);
}

[JsonConverter(typeof(ShareEntryConverter))]
public class ShareEntry
{
    public bool enabled;
    public bool cupboard;
    public bool turret;
    public LockShareEntry keyLock;
    public LockShareEntry codeLock;
    public string Write();
    public static ShareEntry Read(string json);
}

public class LockShareEntry
{
    public bool enabled;
    public bool door;
    public bool box;
    public bool other;
}

private class ShareEntryConverter : JsonConverter
{
    public override void WriteJson(JsonWriter writer, object value, JsonSerializer serializer);
    public override object ReadJson(JsonReader reader, Type objectType, object existingValue, JsonSerializer serializer);
    public override bool CanConvert(Type objectType);
}

private class OldStoredData
{
    public readonly Dictionary<ulong, OldShareData> playerShareData;
    public class OldShareData
    {
        public OldShareEntry teamShare;
        public OldShareEntry friendsShare;
        public OldShareEntry clanShare;
        public JObject GetNewData();
    }

    public class OldShareEntry
    {
        public bool enabled;
        public bool cupboard;
        public bool turret;
        public bool keyLock;
        public bool codeLock;
        public string Write();
    }

}

public class OldShareData
{
    public OldShareEntry teamShare;
    public OldShareEntry friendsShare;
    public OldShareEntry clanShare;
    public JObject GetNewData();
}

public class OldShareEntry
{
    public bool enabled;
    public bool cupboard;
    public bool turret;
    public bool keyLock;
    public bool codeLock;
    public string Write();
}


```

---

## AutomaticPluginUpdater by birthdates - Automatically update Oxide plugins

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using JetBrains.Annotations;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using UnityEngine.Networking;

Oxide.Plugins
[Info("Automatic Plugin Updater", "birthdates", "1.5.5")]
[Description("Automatically update your Oxide plugins!")]
public class AutomaticPluginUpdater : CovalencePlugin
{
    private bool ManualUpdateCommand(IPlayer caller, string command, IList<string> args);
    private const string SearchURL;
    private readonly IList<string> _toIgnore;
    private readonly IDictionary<Plugin, DateTime> _delayedChecks;
    private const string UpdateCommand;
    private const string UpdatePermission;
    private readonly Dictionary<string, string> _discordHeaders;
    private GameObject _gameObject;
    private CoroutineHandler _coroutineHandler;
    private bool _ready;
    private class CoroutineHandler : MonoBehaviour
    {
    }

    private const int TimesPerMinute;
    private const int FastTimeMinutes;
    private DateTime _lastCheck;
    private int _frequentTries;
    [UsedImplicitly]
    private void Init();
    [UsedImplicitly]
    private void OnServerInitialized();
    [UsedImplicitly]
    private void Unload();
    private void OnPluginLoaded(Plugin plugin);
    [HookMethod("CheckActivePlugins")]
    private void CheckActivePlugins();
    [HookMethod("CheckPlugin")]
    private void CheckPlugin(Plugin plugin, bool bypass);
    private void StartRoutine();
    private static VersionNumber ParseVersionNumber(string version);
    private static string GetFileNameFromDownloadURL(string url);
    private static UpdateType GetUpdateType(string version, VersionNumber oldVersion);
    private static string SplitByCapital(string str, string separator);
    private bool IsBlacklisted(Plugin plugin, bool start);
    private void HandleSearchRequest(int code, string data, Plugin plugin);
    private IEnumerator StartDownload(Plugin plugin, SearchResult searchResult);
    private void SendUpdateLog(string name, string version, bool halted);
    private void CheckDelayedRequests();
    private string Lang(string key, string id, object[] args);
    private bool TestFailCode(int code, string data);
    private void CheckPluginCommand(Plugin plugin, IPlayer caller);
    private void ValidateDiscordWebhookRequest(int code, string data);
    private ConfigFile _config;
    private class ConfigFile
    {
        [JsonProperty("Check Currently Loaded Plugins When This Plugin Enables?")]
        public bool CheckPluginsOnStart { get; set; }
        [JsonProperty("Blacklisted Plugins (won't check for update)")]
        public IList<string> BlacklistedPlugins { get; set; }
        [JsonProperty("Blacklisted Authors (won't check for update)")]
        public IList<string> BlacklistedAuthors { get; set; }
        [JsonProperty("Update Version Settings (which updates to not go through with)")]
        public IDictionary<UpdateType, bool> VersionSettings { get; set; }
        [JsonProperty("Do Routine Checks?")]
        public bool DoRoutineChecks { get; set; }
        [JsonProperty("Routine Interval Time (Minutes)")]
        public float RoutineCheckIntervalSeconds { get; set; }
        [JsonProperty("Notify on Blacklisted Update?")]
        public bool NotifyBlacklisted { get; set; }
        [JsonProperty("Use Discord Webhooks?")]
        public bool UseDiscordHooks { get; set; }
        [JsonProperty("Disable Checking on Server Startup?")]
        public bool DisableCheckingOnServerStartup { get; set; }
        [JsonProperty("Disable Author Checking?")]
        public bool DisableAuthorCheck { get; set; }
        [JsonProperty("Discord Webhook")]
        public string DiscordWebHook { get; set; }
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class CoroutineHandler : MonoBehaviour
{
}

private class ConfigFile
{
    [JsonProperty("Check Currently Loaded Plugins When This Plugin Enables?")]
    public bool CheckPluginsOnStart { get; set; }
    [JsonProperty("Blacklisted Plugins (won't check for update)")]
    public IList<string> BlacklistedPlugins { get; set; }
    [JsonProperty("Blacklisted Authors (won't check for update)")]
    public IList<string> BlacklistedAuthors { get; set; }
    [JsonProperty("Update Version Settings (which updates to not go through with)")]
    public IDictionary<UpdateType, bool> VersionSettings { get; set; }
    [JsonProperty("Do Routine Checks?")]
    public bool DoRoutineChecks { get; set; }
    [JsonProperty("Routine Interval Time (Minutes)")]
    public float RoutineCheckIntervalSeconds { get; set; }
    [JsonProperty("Notify on Blacklisted Update?")]
    public bool NotifyBlacklisted { get; set; }
    [JsonProperty("Use Discord Webhooks?")]
    public bool UseDiscordHooks { get; set; }
    [JsonProperty("Disable Checking on Server Startup?")]
    public bool DisableCheckingOnServerStartup { get; set; }
    [JsonProperty("Disable Author Checking?")]
    public bool DisableAuthorCheck { get; set; }
    [JsonProperty("Discord Webhook")]
    public string DiscordWebHook { get; set; }
    public static ConfigFile DefaultConfig();
}


```

---

## AutoPickup by beee - Automatically pickup hemp, pumpkin, ore, pickable items, corpse, etc.

```csharp
using System;
using System.Collections.Generic;
using ConVar;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using UnityEngine;

Oxide.Plugins
[Info("Auto Pickup", "Arainrr", "1.2.16")]
[Description("Automatically pickup hemp, pumpkin, ore, pickupable items, corpse, etc.")]
public class AutoPickup : RustPlugin
{
    [PluginReference]
    private readonly Plugin Friends;
    private readonly Plugin Clans;
    private static AutoPickup instance;
    private static PickupType enabledPickupTypes;
    private static object False;
    private const string PERMISSION_USE;
    private void Init();
    private void OnServerInitialized();
    private void OnServerSave();
    private void Unload();
    private void OnPlayerAttack(BasePlayer attacker, HitInfo info);
    private object CanLootEntity(BasePlayer player, LootContainer lootContainer);
    private void OnEntitySpawned(BaseNetworkable baseNetworkable);
    private bool TryPickupLootContainer(LootContainer lootContainer, BasePlayer player, HitInfo info);
    private void UpdateConfig();
    private bool AreFriends(ulong playerID, ulong friendID);
    private static bool SameTeam(ulong playerID, ulong friendID);
    private bool HasFriend(ulong playerID, ulong friendID);
    private bool SameClan(ulong playerID, ulong friendID);
    private StoredData.AutoPickData defaultData;
    private StoredData.AutoPickData DefaultData { get; set; }
    private StoredData.AutoPickData GetAutoPickupData(ulong playerID, bool readOnly);
    private StoredData.AutoPickData CreateDefaultData();
    private static void CheckEntity(BaseNetworkable baseNetworkable, bool justCreated);
    private static PickupType GetPickupTypeFromEntity(BaseNetworkable baseNetworkable);
    private static void CreateAutoPickupHelper(Transform transform, PickupType pickupType);
    private static bool PickupLootContainer(BasePlayer player, LootContainer lootContainer, HitInfo info);
    private static bool InventoryExistItem(BasePlayer player, Item item);
    private static bool InventoryIsFull(BasePlayer player, Item item);
    private static bool PickupDroppedItemContainer(BasePlayer player, DroppedItemContainer droppedItemContainer);
    private static bool PickupPlayerCorpse(BasePlayer player, PlayerCorpse playerCorpse);
    private static bool CanAutoPickup(BasePlayer player, BaseEntity entity);
    private static bool IsBarrel(string shortPrefabName);
    private class WorldItemCollisionDetection : MonoBehaviour
    {
        private bool collided;
        private void OnCollisionEnter(Collision collision);
        private void AddAutoPickupComponent();
    }

    private class AutoPickupHelper : FacepunchBehaviour
    {
        private const int LAYER_PLAYER;
        public static List<AutoPickupHelper> autoPickupHelpers;
        private BaseEntity entity;
        private PickupType pickupType;
        private SphereCollider sphereCollider;
        private void Awake();
        public void Init(PickupType pickupType);
        private void CreateCollider();
        private void OnTriggerEnter(Collider collider);
        private void OnDestroy();
    }

    private const string UINAME_MAIN;
    private const string UINAME_MENU;
    private static void CreateMainUI(BasePlayer player);
    private static void UpdateMenuUI(BasePlayer player, StoredData.AutoPickData autoPickData);
    private static void CreateEntryUI(CuiElementContainer container, string command, string leftText, string rightText, string anchorMin, string anchorMax);
    private static float[] GetEntryAnchors(int i, float entrySize, float spacingY);
    private static void DestroyUI(BasePlayer player);
    [ConsoleCommand("AutoPickupUI")]
    private void CCmdAutoPickupUI(ConsoleSystem.Arg arg);
    private void CmdAutoPickup(BasePlayer player, string command, string[] args);
    private static ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public Settings globalS;
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatS chatS;
        [JsonProperty(PropertyName = "Auto Pickup Settings")]
        public Dictionary<PickupType, PickupTypeS> autoPickupS;
        [JsonProperty(PropertyName = "World Item Pickup Settings")]
        public WorldItemPickupS worldItemS;
        [JsonProperty(PropertyName = "Collectable Gifts Pickup Settings")]
        public CollectableGiftsPickupS collectableGiftsS;
        [JsonProperty(PropertyName = "Loot Container Pickup Settings")]
        public Dictionary<string, bool> lootContainerS;
        [JsonProperty(PropertyName = "Collectible Entity Pickup Settings")]
        public Dictionary<string, bool> collectibleEntityS;
        [JsonProperty(PropertyName = "Plant Entity Pickup Settings")]
        public Dictionary<string, bool> plantEntityS;
        public class Settings
        {
            [JsonProperty(PropertyName = "Clear Data On Map Wipe")]
            public bool clearDataOnWipe;
            [JsonProperty(PropertyName = "Use Teams")]
            public bool useTeams;
            [JsonProperty(PropertyName = "Use Clans")]
            public bool useClans;
            [JsonProperty(PropertyName = "Use Friends")]
            public bool useFriends;
            [JsonProperty(PropertyName = "Auto pickup is enabled by default")]
            public bool defaultEnabled;
            [JsonProperty(PropertyName = "Prevent pickup other player's backpack")]
            public bool preventPickupBackpack;
            [JsonProperty(PropertyName = "Prevent pickup other player's corpse")]
            public bool preventPickupCorpse;
            [JsonProperty(PropertyName = "Prevent pickup other player's plant entity")]
            public bool preventPickupPlant;
            [JsonProperty(PropertyName = "Prevent pickup other player's loot container")]
            public bool preventPickupLoot;
            [JsonProperty(PropertyName = "Prevent pickup of plant entities in the planter box")]
            public bool preventPlanterBox;
        }

        public class ChatS
        {
            [JsonProperty(PropertyName = "Chat Command")]
            public string command;
            [JsonProperty(PropertyName = "Chat Prefix")]
            public string prefix;
            [JsonProperty(PropertyName = "Chat SteamID Icon")]
            public ulong steamIDIcon;
        }

        public class PickupTypeS
        {
            [JsonProperty(PropertyName = "Enabled")]
            public bool enabled;
            [JsonProperty(PropertyName = "Check Radius")]
            public float radius;
        }

        public class WorldItemPickupS
        {
            [JsonProperty(PropertyName = "Auto Pickup Delay")]
            public float pickupDelay;
            [JsonProperty(PropertyName = "Check that player's inventory is full")]
            public bool checkInventoryFull;
            [JsonProperty(PropertyName = "Only pickup items that exist in player's inventory")]
            public bool onlyPickupExistItem;
            [JsonProperty(PropertyName = "Item Block List (Item shortname)")]
            public HashSet<string> itemBlockList;
            [JsonProperty(PropertyName = "Allow Pickup Item Category")]
            public Dictionary<ItemCategory, bool> itemCategoryS;
        }

        public class CollectableGiftsPickupS
        {
            [JsonProperty(PropertyName = "Requires player to hold a basket")]
            public bool requiresBasket;
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
        public readonly Dictionary<ulong, AutoPickData> playerAutoPickupData;
        public class AutoPickData
        {
            public bool enabled;
            public bool autoClone;
            public PickupType blockPickupTypes;
        }

    }

    private void LoadData();
    private void SaveData();
    private void ClearData();
    private void OnNewSave(string filename);
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class WorldItemCollisionDetection : MonoBehaviour
{
    private bool collided;
    private void OnCollisionEnter(Collision collision);
    private void AddAutoPickupComponent();
}

private class AutoPickupHelper : FacepunchBehaviour
{
    private const int LAYER_PLAYER;
    public static List<AutoPickupHelper> autoPickupHelpers;
    private BaseEntity entity;
    private PickupType pickupType;
    private SphereCollider sphereCollider;
    private void Awake();
    public void Init(PickupType pickupType);
    private void CreateCollider();
    private void OnTriggerEnter(Collider collider);
    private void OnDestroy();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public Settings globalS;
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatS chatS;
    [JsonProperty(PropertyName = "Auto Pickup Settings")]
    public Dictionary<PickupType, PickupTypeS> autoPickupS;
    [JsonProperty(PropertyName = "World Item Pickup Settings")]
    public WorldItemPickupS worldItemS;
    [JsonProperty(PropertyName = "Collectable Gifts Pickup Settings")]
    public CollectableGiftsPickupS collectableGiftsS;
    [JsonProperty(PropertyName = "Loot Container Pickup Settings")]
    public Dictionary<string, bool> lootContainerS;
    [JsonProperty(PropertyName = "Collectible Entity Pickup Settings")]
    public Dictionary<string, bool> collectibleEntityS;
    [JsonProperty(PropertyName = "Plant Entity Pickup Settings")]
    public Dictionary<string, bool> plantEntityS;
    public class Settings
    {
        [JsonProperty(PropertyName = "Clear Data On Map Wipe")]
        public bool clearDataOnWipe;
        [JsonProperty(PropertyName = "Use Teams")]
        public bool useTeams;
        [JsonProperty(PropertyName = "Use Clans")]
        public bool useClans;
        [JsonProperty(PropertyName = "Use Friends")]
        public bool useFriends;
        [JsonProperty(PropertyName = "Auto pickup is enabled by default")]
        public bool defaultEnabled;
        [JsonProperty(PropertyName = "Prevent pickup other player's backpack")]
        public bool preventPickupBackpack;
        [JsonProperty(PropertyName = "Prevent pickup other player's corpse")]
        public bool preventPickupCorpse;
        [JsonProperty(PropertyName = "Prevent pickup other player's plant entity")]
        public bool preventPickupPlant;
        [JsonProperty(PropertyName = "Prevent pickup other player's loot container")]
        public bool preventPickupLoot;
        [JsonProperty(PropertyName = "Prevent pickup of plant entities in the planter box")]
        public bool preventPlanterBox;
    }

    public class ChatS
    {
        [JsonProperty(PropertyName = "Chat Command")]
        public string command;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong steamIDIcon;
    }

    public class PickupTypeS
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled;
        [JsonProperty(PropertyName = "Check Radius")]
        public float radius;
    }

    public class WorldItemPickupS
    {
        [JsonProperty(PropertyName = "Auto Pickup Delay")]
        public float pickupDelay;
        [JsonProperty(PropertyName = "Check that player's inventory is full")]
        public bool checkInventoryFull;
        [JsonProperty(PropertyName = "Only pickup items that exist in player's inventory")]
        public bool onlyPickupExistItem;
        [JsonProperty(PropertyName = "Item Block List (Item shortname)")]
        public HashSet<string> itemBlockList;
        [JsonProperty(PropertyName = "Allow Pickup Item Category")]
        public Dictionary<ItemCategory, bool> itemCategoryS;
    }

    public class CollectableGiftsPickupS
    {
        [JsonProperty(PropertyName = "Requires player to hold a basket")]
        public bool requiresBasket;
    }

    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version;
}

public class Settings
{
    [JsonProperty(PropertyName = "Clear Data On Map Wipe")]
    public bool clearDataOnWipe;
    [JsonProperty(PropertyName = "Use Teams")]
    public bool useTeams;
    [JsonProperty(PropertyName = "Use Clans")]
    public bool useClans;
    [JsonProperty(PropertyName = "Use Friends")]
    public bool useFriends;
    [JsonProperty(PropertyName = "Auto pickup is enabled by default")]
    public bool defaultEnabled;
    [JsonProperty(PropertyName = "Prevent pickup other player's backpack")]
    public bool preventPickupBackpack;
    [JsonProperty(PropertyName = "Prevent pickup other player's corpse")]
    public bool preventPickupCorpse;
    [JsonProperty(PropertyName = "Prevent pickup other player's plant entity")]
    public bool preventPickupPlant;
    [JsonProperty(PropertyName = "Prevent pickup other player's loot container")]
    public bool preventPickupLoot;
    [JsonProperty(PropertyName = "Prevent pickup of plant entities in the planter box")]
    public bool preventPlanterBox;
}

public class ChatS
{
    [JsonProperty(PropertyName = "Chat Command")]
    public string command;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong steamIDIcon;
}

public class PickupTypeS
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled;
    [JsonProperty(PropertyName = "Check Radius")]
    public float radius;
}

public class WorldItemPickupS
{
    [JsonProperty(PropertyName = "Auto Pickup Delay")]
    public float pickupDelay;
    [JsonProperty(PropertyName = "Check that player's inventory is full")]
    public bool checkInventoryFull;
    [JsonProperty(PropertyName = "Only pickup items that exist in player's inventory")]
    public bool onlyPickupExistItem;
    [JsonProperty(PropertyName = "Item Block List (Item shortname)")]
    public HashSet<string> itemBlockList;
    [JsonProperty(PropertyName = "Allow Pickup Item Category")]
    public Dictionary<ItemCategory, bool> itemCategoryS;
}

public class CollectableGiftsPickupS
{
    [JsonProperty(PropertyName = "Requires player to hold a basket")]
    public bool requiresBasket;
}

private class StoredData
{
    public readonly Dictionary<ulong, AutoPickData> playerAutoPickupData;
    public class AutoPickData
    {
        public bool enabled;
        public bool autoClone;
        public PickupType blockPickupTypes;
    }

}

public class AutoPickData
{
    public bool enabled;
    public bool autoClone;
    public PickupType blockPickupTypes;
}


```

---

## AutoPickupBarrel by l3rady - Allows players to pick up dropped loot automatically from barrels and road signs on destroy.

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Rust;
using UnityEngine;
using Oxide.Core;

Oxide.Plugins
[Info("Auto Pickup Barrel", "l3rady", "1.3")]
[Description("Allows players to pick up dropped loot from barrels and road signs on destroy automatically.")]
public class AutoPickupBarrel : RustPlugin
{
    private readonly string[] BarrelContainerShortPrefabNames;
    private readonly string[] RoadSignContainerShortPrefabNames;
    private Configuration Settings;
    public class Configuration
    {
        [JsonProperty("Auto pickup distance")]
        public float AutoPickupDistance;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private void Init();
    private object OnEntityTakeDamage(LootContainer LootEntContainer, HitInfo HitEntInfo);
    private bool IsBarrel(string LootEntContainerName);
    private bool IsRoadSign(string LootEntContainerName);
    private object AutoPickup(LootContainer LootEntContainer, HitInfo HitEntInfo, string AutoPickupPrefab);
}

public class Configuration
{
    [JsonProperty("Auto pickup distance")]
    public float AutoPickupDistance;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}


```

---

## AutoPlant by rostov114 - Automation of your plantations

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using Facepunch;
using System.Linq;
using Oxide.Core;

Oxide.Plugins
[Info("Auto Plant", "Egor Blagov / rostov114", "1.2.7")]
[Description("Automation of your plantations")]
 class AutoPlant : RustPlugin
{
    public static AutoPlant _instance;
    private List<ulong> _activeUse;
    private Configuration _config;
    public class Configuration
    {
        [JsonProperty(PropertyName = "Auto Plant permission")]
        public string autoPlant;
        [JsonProperty(PropertyName = "Auto Gather permission")]
        public string autoGather;
        [JsonProperty(PropertyName = "Auto Cutting permission")]
        public string autoCutting;
        [JsonProperty(PropertyName = "Auto Remove Dying permission")]
        public string autoDying;
        [JsonProperty(PropertyName = "Auto fertilizer permission")]
        public string autoFertilizer;
        [JsonProperty(PropertyName = "Auto fertilizer configuration")]
        public FertilizerConfiguration fertilizer;
        public class FertilizerConfiguration
        {
            [JsonProperty(PropertyName = "Maximum distance from the PlanterBox")]
            public int maxDistance;
            [JsonProperty(PropertyName = "Default fertilizer amount")]
            public int defaultAmount;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<ulong, int> _data;
    public static class Data
    {
        public static int Get(ulong userID);
        public static void Set(ulong userID, int amount);
        public static void Save();
        public static void Load();
    }

    private void LoadDefaultMessages();
    private string _(BasePlayer player, string key, object[] args);
    private void Init();
    private void Loaded();
    private object OnGrowableGather(GrowableEntity plant, BasePlayer player);
    private object CanTakeCutting(BasePlayer player, GrowableEntity plant);
    private object OnRemoveDying(GrowableEntity plant, BasePlayer player);
    private void OnEntityBuilt(Planner plan, GameObject seed);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private void OnPlayerInput(BasePlayer player, InputState input);
    [ChatCommand("fertilizer")]
    private void fertilizer_command(BasePlayer player, string command, string[] args);
    public void checkSubscribeHooks();
    public bool IsFree(Construction common, Construction.Target target);
    public bool GetGrowables(BasePlayer player, GrowableEntity plant, string perm, List<BaseEntity> growables);
    private bool IsLookingPlanterBox(BasePlayer player, PlanterBox planterBox);
}

public class Configuration
{
    [JsonProperty(PropertyName = "Auto Plant permission")]
    public string autoPlant;
    [JsonProperty(PropertyName = "Auto Gather permission")]
    public string autoGather;
    [JsonProperty(PropertyName = "Auto Cutting permission")]
    public string autoCutting;
    [JsonProperty(PropertyName = "Auto Remove Dying permission")]
    public string autoDying;
    [JsonProperty(PropertyName = "Auto fertilizer permission")]
    public string autoFertilizer;
    [JsonProperty(PropertyName = "Auto fertilizer configuration")]
    public FertilizerConfiguration fertilizer;
    public class FertilizerConfiguration
    {
        [JsonProperty(PropertyName = "Maximum distance from the PlanterBox")]
        public int maxDistance;
        [JsonProperty(PropertyName = "Default fertilizer amount")]
        public int defaultAmount;
    }

}

public class FertilizerConfiguration
{
    [JsonProperty(PropertyName = "Maximum distance from the PlanterBox")]
    public int maxDistance;
    [JsonProperty(PropertyName = "Default fertilizer amount")]
    public int defaultAmount;
}

public static class Data
{
    public static int Get(ulong userID);
    public static void Set(ulong userID, int amount);
    public static void Save();
    public static void Load();
}


```

---

## AutoPurge by misticos - Removes entities if the owner becomes inactive

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Diagnostics;
using System.Text.RegularExpressions;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;
using Time = Oxide.Core.Libraries.Time;

Oxide.Plugins
[Info("Auto Purge", "misticos", "2.1.3")]
[Description("Remove entities if the owner becomes inactive")]
public class AutoPurge : CovalencePlugin
{
    private static AutoPurge _ins;
    private Dictionary<string, bool> _canPurgeCache;
    private HashSet<string> _logCache;
    private HashSet<string> _deployables;
    [PluginReference("PlaceholderAPI")]
    private Plugin _placeholderAPI;
    private Time _time;
    private Timer _timerPurge;
    private const string CommandRunPurgeName;
    private const string PermissionRunPurge;
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Purge Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<PurgeSettings> Purges;
        [JsonProperty(PropertyName = "Purge Timer Frequency")]
        public float PurgeFrequency;
        [JsonProperty(PropertyName = "Purge On Startup")]
        public bool PurgeOnStartup;
        [JsonProperty(PropertyName = "Entities Per Step")]
        public int EntitiesPerStep;
        [JsonProperty(PropertyName = "Purge Building Blocks")]
        public bool PurgeBuildingBlocks;
        [JsonProperty(PropertyName = "Purge Deployables")]
        public bool PurgeDeployables;
        [JsonProperty(PropertyName = "Purge Sleepers")]
        public bool PurgeSleepers;
        [JsonProperty(PropertyName = "Use Logs")]
        public bool UseLogs;
        public class PurgeSettings
        {
            [JsonProperty(PropertyName = "Permission")]
            public string Permission;
            [JsonProperty(PropertyName = "Lifetime")]
            public string LifetimeRaw;
            [JsonIgnore]
            public uint Lifetime;
            [JsonIgnore]
            public bool NoPurge;
            public static PurgeSettings Find(string playerId);
        }

    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty("Users", NullValueHandling = NullValueHandling.Ignore,
                DefaultValueHandling = DefaultValueHandling.Ignore)]
        public List<UserData> Users;
        public class UserData
        {
            public string Id;
            public uint LastSeen;
        }

        public Dictionary<string, uint> LastSeen;
    }

    private void CommandRunPurge(IPlayer player, string command, string[] args);
    protected override void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnUserConnected(IPlayer player);
    private void OnUserDisconnected(IPlayer player);
    private void OnPlaceholderAPIReady();
    private void UpdateLastSeen(uint timestamp);
    private void RunPurge(IPlayer caller);
    private static readonly WaitForFixedUpdate WaitForFixedUpdate;
    private IEnumerator RunPurgeEnumerator(IPlayer caller);
    private bool CanPurge(ulong ownerId, BuildingPrivlidge privilege, uint timestamp);
    private bool CanPurgeUser(string id, uint timestamp);
    private bool CanPurgeUserInternal(string id, uint timestamp);
    private static readonly Regex RegexStringTime;
    private static bool ConvertToSeconds(string time, uint seconds);
    private string GetMsg(string key, string userId);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Purge Settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<PurgeSettings> Purges;
    [JsonProperty(PropertyName = "Purge Timer Frequency")]
    public float PurgeFrequency;
    [JsonProperty(PropertyName = "Purge On Startup")]
    public bool PurgeOnStartup;
    [JsonProperty(PropertyName = "Entities Per Step")]
    public int EntitiesPerStep;
    [JsonProperty(PropertyName = "Purge Building Blocks")]
    public bool PurgeBuildingBlocks;
    [JsonProperty(PropertyName = "Purge Deployables")]
    public bool PurgeDeployables;
    [JsonProperty(PropertyName = "Purge Sleepers")]
    public bool PurgeSleepers;
    [JsonProperty(PropertyName = "Use Logs")]
    public bool UseLogs;
    public class PurgeSettings
    {
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Lifetime")]
        public string LifetimeRaw;
        [JsonIgnore]
        public uint Lifetime;
        [JsonIgnore]
        public bool NoPurge;
        public static PurgeSettings Find(string playerId);
    }

}

public class PurgeSettings
{
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Lifetime")]
    public string LifetimeRaw;
    [JsonIgnore]
    public uint Lifetime;
    [JsonIgnore]
    public bool NoPurge;
    public static PurgeSettings Find(string playerId);
}

private class PluginData
{
    [JsonProperty("Users", NullValueHandling = NullValueHandling.Ignore,
                DefaultValueHandling = DefaultValueHandling.Ignore)]
    public List<UserData> Users;
    public class UserData
    {
        public string Id;
        public uint LastSeen;
    }

    public Dictionary<string, uint> LastSeen;
}

public class UserData
{
    public string Id;
    public uint LastSeen;
}


```

---

## AutoResetTargets by  - Automatically resets knocked down targets after a set time.

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Auto Reset Targets", "Default", "1.0.5")]
[Description("Automatically resets knocked down targets after a set amount of time")]
public class AutoResetTargets : RustPlugin
{
    public float activeTime;
     bool Changed;
    private const string permissionName;
    private void OnEntityTakeDamage(BaseCombatEntity entity, HitInfo info);
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
    protected override void LoadDefaultConfig();
     void Init();
}


```

---

## AutoSignModeration by Whispers88 - Auto Sign Moderation is an AI based moderation plugin to automatically handle moderation of signs.

```csharp
using Newtonsoft.Json;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Drawing;
using System.Drawing.Drawing2D;
using System.Drawing.Imaging;
using System.IO;
using System.Linq;
using UnityEngine;
using UnityEngine.Networking;
using Graphics = System.Drawing.Graphics;
using System.Text;
using Facepunch;
using Oxide.Core.Libraries.Covalence;
using Encoder = System.Drawing.Imaging.Encoder;

Oxide.Plugins
[Info("Auto Sign Moderation", "Whispers88", "1.1.1")]
[Description("Uses the Omni AI/Open AI to auto moderate image content")]
public class AutoSignModeration : CovalencePlugin
{
    private Dictionary<ulong, float> _signCooldown;
    private Queue<SignData> _queuedImages;
    private Dictionary<ImageKey, SignData> _signsQueuedPool;
    private string permWhitelist;
    private Configuration config;
    public class Configuration
    {
        [JsonProperty("Image Size 25 - 100%")]
        public float imageSizeReduction;
        [JsonProperty("Image Quality 25 - 100%")]
        public float imageQualityReduction;
        [JsonProperty("Sign Update Cooldown (seconds)")]
        public float signCooldown;
        [JsonProperty("Player Moderated Cooldown (seconds)")]
        public float signModerationCooldown;
        [JsonProperty("Hide signs while being checked")]
        public bool hideSign;
        [JsonProperty("Use Temp Loading Image")]
        public bool useTempImage;
        [JsonProperty("Temp Loading Image URL:")]
        public string tempModerationImageURL;
        [JsonProperty("Logging Mode Only")]
        public bool loggingMode;
        [JsonProperty("Send Player Chat Warnings")]
        public bool chatWarnings;
        [JsonProperty("Batch Mode - Disables hiding of signs")]
        public BatchSettings batchSettings;
        [JsonProperty("Discord Settings")]
        public DiscordSettings discordSettings;
        [JsonProperty("Moderation API (Free) - Limited Options")]
        public ModerationAPI moderationAPI;
        [JsonProperty("Advance Moderation API (Paid)")]
        public GPTModel gptModel;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    public class BatchSettings
    {
        [JsonProperty("Check images in batches (Advance Mode Only)")]
        public bool imagePooling;
        [JsonProperty("Batch Image Check Rate (Minutes)")]
        public float imagePoolingRate;
        [JsonProperty("Minimum images to batch check")]
        public float minImagesPooled;
        [JsonProperty("Max checks to bypass minimum images 0 = no bypass")]
        public float maxChecksImagesPooled;
    }

    public class DiscordSettings
    {
        [JsonProperty("Log to Discord")]
        public bool discordLogging;
        [JsonProperty("Log moderated Images to Discord (WARNING THIS MAY SEND NSFW CONTENT TO YOUR DISCORD)")]
        public bool discordImageLogging;
        [JsonProperty("Discord Webhook")]
        public string DiscordWebhook;
        [JsonProperty("Discord Username")]
        public string DiscordUsername;
        [JsonProperty("Server Name")]
        public string ServerName;
        [JsonProperty("Avatar URL")]
        public string AvatarUrl;
    }

    public class ModerationAPI
    {
        [JsonProperty("Enable")]
        public bool enabled;
        [JsonProperty("Open AI Token")]
        public string apiToken;
        [JsonProperty("Cooldown between API Checks (seconds)")]
        public float apiCooldown;
        [JsonProperty("Block images of harassment")]
        public bool harassment;
        [JsonProperty("Block images of harassment/threatening")]
        public bool harassmentThreatening;
        [JsonProperty("Block images of sexual")]
        public bool sexual;
        [JsonProperty("Block images of hate")]
        public bool hate;
        [JsonProperty("Block images of hate/threatening")]
        public bool hateThreatening;
        [JsonProperty("Block images of illicit")]
        public bool illicit;
        [JsonProperty("Block images of illicit/violent")]
        public bool illicitViolent;
        [JsonProperty("Block images of self-harm/intent")]
        public bool selfHarmIntent;
        [JsonProperty("Block images of self-harm/instructions")]
        public bool selfHarmInstructions;
        [JsonProperty("Block images of self-harm")]
        public bool selfHarm;
        [JsonProperty("Block images of sexual/minors")]
        public bool sexualMinors;
        [JsonProperty("Block images of violence")]
        public bool violence;
        [JsonProperty("Block images of violence/graphic")]
        public bool violenceGraphic;
    }

    public class GPTModel
    {
        [JsonProperty("Enable GPT Model (WARNING THIS IS PAID PLEASE READ DOCS)")]
        public bool enabled;
        [JsonProperty("Open AI Token")]
        public string apiToken;
        [JsonProperty("Cooldown between API Checks (seconds)")]
        public float apiCooldown;
        [JsonProperty("Model (Don't change this if you dont know what it is)")]
        public string model;
        [JsonProperty("Content to moderate")]
        public string prompt;
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultMessages();
    public class SignData
    {
        public ulong playerId;
        public Signage sign;
        public int textureIndex;
        public uint crc;
        public FileStorage.Type type;
    }

    public class Categories
    {
        public bool harassment;
        public bool harassmentthreatening;
        public bool sexual;
        public bool hate;
        public bool hatethreatening;
        public bool illicit;
        public bool illicitviolent;
        public bool selfharmintent;
        public bool selfharminstructions;
        public bool selfharm;
        public bool sexualminors;
        public bool violence;
        public bool violencegraphic;
    }

    public class CategoryScores
    {
        public double harassment;
        public double harassmentthreatening;
        public double sexual;
        public double hate;
        public double hatethreatening;
        public double illicit;
        public double illicitviolent;
        public double selfharmintent;
        public double selfharminstructions;
        public double selfharm;
        public double sexualminors;
        public double violence;
        public double violencegraphic;
    }

    public class Result
    {
        public bool flagged;
        public Categories categories;
        public CategoryScores category_scores;
    }

    public class OmniDataRoot
    {
        public string id;
        public string model;
        public List<Result> results;
    }

    public class Choice
    {
        public int index;
        public Message message;
        public object logprobs;
        public string finish_reason;
    }

    public class CompletionTokensDetails
    {
        public int reasoning_tokens;
        public int audio_tokens;
        public int accepted_prediction_tokens;
        public int rejected_prediction_tokens;
    }

    public class Message
    {
        public string role;
        public string content;
        public object refusal;
    }

    public class PromptTokensDetails
    {
        public int cached_tokens;
        public int audio_tokens;
    }

    public class GPTRoot
    {
        public string id;
        public string @object;
        public int created;
        public string model;
        public List<Choice> choices;
    }

    public class Usage
    {
        public int prompt_tokens;
        public int completion_tokens;
        public int total_tokens;
        public PromptTokensDetails prompt_tokens_details;
        public CompletionTokensDetails completion_tokens_details;
    }

    public class ImageSize
    {
        public int Width { get; set; }
        public int Height { get; set; }
        public ImageSize(int width, int height);
    }

    private ImageCodecInfo _imageCodecInfo;
    private EncoderParameters _encoderParams;
    private Dictionary<uint, ImageSize> _ImageSizeperAsset;
    private void OnServerInitialized();
    private void Unload();
    private void StartPooledImageCheck();
    private StringBuilder _formattedTime;
    private object? CanUpdateSign(BasePlayer player, Signage sign);
     void OnSignUpdated(Signage sign, BasePlayer player, int textureIndex);
    private void SetImageToSign(Signage sign, int textureIndex, uint crc);
    readonly string jsonline1;
    readonly string jsonline2;
    private static StringBuilder _stringBuilder;
    private string CreateJson(string base64Input);
    readonly string jsongptline1;
    readonly string jsongptline2;
    readonly string jsongptline3;
    readonly string jsongptline4;
    private string gptModel;
    private string prompt;
    private string CreateGPTJson(string base64Input);
    private readonly string _moderationAPI;
    private readonly string _gptModelAPI;
    private static WaitForSeconds _ModerationAPIWait;
    private static WaitForSeconds _GPTModelAPIWait;
    private static IEnumerator CheckImagesRun;
    private IEnumerator CheckImages();
    private IEnumerator CheckModerationAPI(SignData signData, byte[] array);
    private IEnumerator CheckGPTModelAPI(SignData signData, byte[] array);
    private void ReportContent(SignData signData, byte[] array);
     string badResponse;
    private bool IsBadResponse(UnityWebRequest www);
    const string jsongptPoolline1;
    const string jsongptPoolline2;
    const string jsongptPoolline3;
    const string jsongptPoolline4;
    const string jsongptPoolline5;
    const string jsongptPoolline6;
    private int _pooledImageChecks;
    private static IEnumerator CheckPooledImagesRun;
    private class PooledSignData
    {
        public byte[] bytes;
        public SignData sign;
    }

    private IEnumerator CheckPooledImages();
    private void SendPlayerWarning(ulong playerID);
    public byte[] ResizeImage(byte[] bytes, int width, int height);
    private bool CheckImage(Result omniResult);
    private uint _tempModerationImageCRC;
    private string _tempModerationImageURL;
    private static IEnumerator GetLoadingImage;
    private IEnumerator LoadingImageSetup();
    private IEnumerator LogToDiscord(SignData signData, BasePlayer player, string model, byte[] imageBytes);
    private DiscordMessage CreateDiscordMessage(string playername, string userid, string itemname, Vector3 location, string model, uint crc);
    public class DiscordMessage
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

        public DiscordMessage(string username, string avatar_url, List<Embeds> embeds);
    }

    private bool HasPerm(string id, string perm);
    private string GetLang(string langKey, string playerId, object[] args);
    private void ChatMessage(IPlayer player, string langKey, object[] args);
    private byte[] GetImageBytes(SignData signData);
}

public class Configuration
{
    [JsonProperty("Image Size 25 - 100%")]
    public float imageSizeReduction;
    [JsonProperty("Image Quality 25 - 100%")]
    public float imageQualityReduction;
    [JsonProperty("Sign Update Cooldown (seconds)")]
    public float signCooldown;
    [JsonProperty("Player Moderated Cooldown (seconds)")]
    public float signModerationCooldown;
    [JsonProperty("Hide signs while being checked")]
    public bool hideSign;
    [JsonProperty("Use Temp Loading Image")]
    public bool useTempImage;
    [JsonProperty("Temp Loading Image URL:")]
    public string tempModerationImageURL;
    [JsonProperty("Logging Mode Only")]
    public bool loggingMode;
    [JsonProperty("Send Player Chat Warnings")]
    public bool chatWarnings;
    [JsonProperty("Batch Mode - Disables hiding of signs")]
    public BatchSettings batchSettings;
    [JsonProperty("Discord Settings")]
    public DiscordSettings discordSettings;
    [JsonProperty("Moderation API (Free) - Limited Options")]
    public ModerationAPI moderationAPI;
    [JsonProperty("Advance Moderation API (Paid)")]
    public GPTModel gptModel;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

public class BatchSettings
{
    [JsonProperty("Check images in batches (Advance Mode Only)")]
    public bool imagePooling;
    [JsonProperty("Batch Image Check Rate (Minutes)")]
    public float imagePoolingRate;
    [JsonProperty("Minimum images to batch check")]
    public float minImagesPooled;
    [JsonProperty("Max checks to bypass minimum images 0 = no bypass")]
    public float maxChecksImagesPooled;
}

public class DiscordSettings
{
    [JsonProperty("Log to Discord")]
    public bool discordLogging;
    [JsonProperty("Log moderated Images to Discord (WARNING THIS MAY SEND NSFW CONTENT TO YOUR DISCORD)")]
    public bool discordImageLogging;
    [JsonProperty("Discord Webhook")]
    public string DiscordWebhook;
    [JsonProperty("Discord Username")]
    public string DiscordUsername;
    [JsonProperty("Server Name")]
    public string ServerName;
    [JsonProperty("Avatar URL")]
    public string AvatarUrl;
}

public class ModerationAPI
{
    [JsonProperty("Enable")]
    public bool enabled;
    [JsonProperty("Open AI Token")]
    public string apiToken;
    [JsonProperty("Cooldown between API Checks (seconds)")]
    public float apiCooldown;
    [JsonProperty("Block images of harassment")]
    public bool harassment;
    [JsonProperty("Block images of harassment/threatening")]
    public bool harassmentThreatening;
    [JsonProperty("Block images of sexual")]
    public bool sexual;
    [JsonProperty("Block images of hate")]
    public bool hate;
    [JsonProperty("Block images of hate/threatening")]
    public bool hateThreatening;
    [JsonProperty("Block images of illicit")]
    public bool illicit;
    [JsonProperty("Block images of illicit/violent")]
    public bool illicitViolent;
    [JsonProperty("Block images of self-harm/intent")]
    public bool selfHarmIntent;
    [JsonProperty("Block images of self-harm/instructions")]
    public bool selfHarmInstructions;
    [JsonProperty("Block images of self-harm")]
    public bool selfHarm;
    [JsonProperty("Block images of sexual/minors")]
    public bool sexualMinors;
    [JsonProperty("Block images of violence")]
    public bool violence;
    [JsonProperty("Block images of violence/graphic")]
    public bool violenceGraphic;
}

public class GPTModel
{
    [JsonProperty("Enable GPT Model (WARNING THIS IS PAID PLEASE READ DOCS)")]
    public bool enabled;
    [JsonProperty("Open AI Token")]
    public string apiToken;
    [JsonProperty("Cooldown between API Checks (seconds)")]
    public float apiCooldown;
    [JsonProperty("Model (Don't change this if you dont know what it is)")]
    public string model;
    [JsonProperty("Content to moderate")]
    public string prompt;
}

public class SignData
{
    public ulong playerId;
    public Signage sign;
    public int textureIndex;
    public uint crc;
    public FileStorage.Type type;
}

public class Categories
{
    public bool harassment;
    public bool harassmentthreatening;
    public bool sexual;
    public bool hate;
    public bool hatethreatening;
    public bool illicit;
    public bool illicitviolent;
    public bool selfharmintent;
    public bool selfharminstructions;
    public bool selfharm;
    public bool sexualminors;
    public bool violence;
    public bool violencegraphic;
}

public class CategoryScores
{
    public double harassment;
    public double harassmentthreatening;
    public double sexual;
    public double hate;
    public double hatethreatening;
    public double illicit;
    public double illicitviolent;
    public double selfharmintent;
    public double selfharminstructions;
    public double selfharm;
    public double sexualminors;
    public double violence;
    public double violencegraphic;
}

public class Result
{
    public bool flagged;
    public Categories categories;
    public CategoryScores category_scores;
}

public class OmniDataRoot
{
    public string id;
    public string model;
    public List<Result> results;
}

public class Choice
{
    public int index;
    public Message message;
    public object logprobs;
    public string finish_reason;
}

public class CompletionTokensDetails
{
    public int reasoning_tokens;
    public int audio_tokens;
    public int accepted_prediction_tokens;
    public int rejected_prediction_tokens;
}

public class Message
{
    public string role;
    public string content;
    public object refusal;
}

public class PromptTokensDetails
{
    public int cached_tokens;
    public int audio_tokens;
}

public class GPTRoot
{
    public string id;
    public string @object;
    public int created;
    public string model;
    public List<Choice> choices;
}

public class Usage
{
    public int prompt_tokens;
    public int completion_tokens;
    public int total_tokens;
    public PromptTokensDetails prompt_tokens_details;
    public CompletionTokensDetails completion_tokens_details;
}

public class ImageSize
{
    public int Width { get; set; }
    public int Height { get; set; }
    public ImageSize(int width, int height);
}

private class PooledSignData
{
    public byte[] bytes;
    public SignData sign;
}

public class DiscordMessage
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

    public DiscordMessage(string username, string avatar_url, List<Embeds> embeds);
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


```

---

## AutoSpectate by Aspectdev - Automatically spectate random players based on a given interval

```csharp
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using Oxide.Core;
using System;

Oxide.Plugins
[Info("Auto Spectate", "Aspectdev", "0.0.13")]
[Description("Provides a way to automatically switch between users while spectating.")]
 class AutoSpectate : CovalencePlugin
{
     Dictionary<string, Information> _users;
    const string _permission;
    protected override void LoadDefaultMessages();
     void Init();
     void OnPlayerDisconnected(BasePlayer player);
     object OnMessagePlayer(string message, BasePlayer player);
     void Unload();
    [Command("autospectate")]
     void Spectate(IPlayer iplayer, string command, string[] args);
    internal void SwitchSpectator(List<BasePlayer> list, BasePlayer player);
    public class Information
    {
        public Information(int sp, Timer t);
        public int spectateId;
        public Timer myTimer;
    }

}

public class Information
{
    public Information(int sp, Timer t);
    public int spectateId;
    public Timer myTimer;
}


```

---

## AutoTakeoff by 0x89A - Allows smooth takeoff with helicopters

```csharp
using System.Collections;
using System.Collections.Generic;
using UnityEngine;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Auto Takeoff", "0x89A", "1.1.1")]
[Description("Allows smooth takeoff with helicopters")]
 class AutoTakeoff : RustPlugin
{
    private const string _canUse;
    private readonly Dictionary<Minicopter, bool> _isTakingOff;
     void Init();
    [ChatCommand("takeoff")]
    private void TakeOffCommand(BasePlayer player);
    private void DoTakeOff(BasePlayer player, Minicopter helicopter);
    private IEnumerator LerpMethod(Minicopter helicopter);
    private void PushMethod(Minicopter helicopter);
    private string GetMessage(string key, string userid);
    protected override void LoadDefaultMessages();
    private Configuration _config;
     class Configuration
    {
        [JsonProperty(PropertyName = "Take off method type")]
        public bool takeOffMethodType;
        [JsonProperty(PropertyName = "Helicopter move distance")]
        public float distanceMoved;
        [JsonProperty(PropertyName = "Minicopter can auto takeoff")]
        public bool minicopterCanTakeoff;
        [JsonProperty(PropertyName = "Minicopter move speed")]
        public float minicopterSpeed;
        [JsonProperty(PropertyName = "Minicopter push force")]
        public float minicopterPushForce;
        [JsonProperty(PropertyName = "Scrap Helicopter can auto takeoff")]
        public bool scrapheliCanTakeoff;
        [JsonProperty(PropertyName = "Scrap helicopter move speed")]
        public float scrapHelicopterSpeed;
        [JsonProperty(PropertyName = "Scrap helicopter push force")]
        public float scrapHelicopterPushForce;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
}

 class Configuration
{
    [JsonProperty(PropertyName = "Take off method type")]
    public bool takeOffMethodType;
    [JsonProperty(PropertyName = "Helicopter move distance")]
    public float distanceMoved;
    [JsonProperty(PropertyName = "Minicopter can auto takeoff")]
    public bool minicopterCanTakeoff;
    [JsonProperty(PropertyName = "Minicopter move speed")]
    public float minicopterSpeed;
    [JsonProperty(PropertyName = "Minicopter push force")]
    public float minicopterPushForce;
    [JsonProperty(PropertyName = "Scrap Helicopter can auto takeoff")]
    public bool scrapheliCanTakeoff;
    [JsonProperty(PropertyName = "Scrap helicopter move speed")]
    public float scrapHelicopterSpeed;
    [JsonProperty(PropertyName = "Scrap helicopter push force")]
    public float scrapHelicopterPushForce;
}


```

---

## AutoTurretAuth by haggbart - One-way synchronizing cupboard authorization with auto-turrets

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using ProtoBuf;
using UnityEngine;

Oxide.Plugins
[Info("Auto Turret Authorization", "haggbart", "1.2.3")]
[Description("One-way synchronizing cupboard authorization with auto turrets")]
 class AutoTurretAuth : RustPlugin
{
    private static IEnumerable<AutoTurret> turrets;
    private static HashSet<PlayerNameID> authorizedPlayers;
    private const string PERSISTENT_AUTHORIZATION;
    protected override void LoadDefaultConfig();
    private void Init();
    private object OnTurretTarget(AutoTurret turret, BaseCombatEntity entity);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void OnCupboardAuthorize(BuildingPrivlidge privilege, BasePlayer player);
    private void OnCupboardDeauthorize(BuildingPrivlidge privilege, BasePlayer player);
    private void OnCupboardClearList(BuildingPrivlidge privilege, BasePlayer player);
    private static bool IsAuthed(BasePlayer player, BaseEntity turret);
    private static void Auth(AutoTurret turret, PlayerNameID playerNameId);
    private static PlayerNameID GetPlayerNameId(BasePlayer player);
    private static void FindTurrets(uint buildingId);
    private static IEnumerator AddPlayer(PlayerNameID playerNameId);
    private static void AddPlayer(AutoTurret turret, PlayerNameID playerNameId);
    private static IEnumerator RemovePlayer(ulong userId);
    private static void RemovePlayer(AutoTurret turret, ulong userId);
    private static IEnumerator RemoveAll();
}


```

---

## BackpackButton by WhiteThunder - Adds a highly configurable UI button for opening Backpacks

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Network;
using Newtonsoft.Json.Converters;
using UnityEngine;
using UnityEngine.UI;
using Time = UnityEngine.Time;

Oxide.Plugins
[Info("Backpack Button", "WhiteThunder", "1.1.2")]
[Description("Adds a button which allows players to open their backpack, with multiple advanced features.")]
internal class BackpackButton : CovalencePlugin
{
    private const string UsagePermission;
    private const int SaddleBagItemId;
    private Configuration _config;
    private SavedData _data;
    private BackpacksApi _backpacksApi;
    private readonly HashSet<ulong> _uiViewers;
    private readonly UiUpdateManager _uiUpdateManager;
    [PluginReference]
    private readonly Plugin Backpacks;
    private static readonly VersionNumber RequiredBackpacksVersion;
    public BackpackButton();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnGroupPermissionGranted(string groupName, string perm);
    private void OnGroupPermissionRevoked(string groupName, string perm);
    private void OnUserPermissionGranted(string userId, string perm);
    private void OnUserPermissionRevoked(string userId, string perm);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerSleep(BasePlayer player);
    private void OnEntityKill(BasePlayer player);
    private void OnEntityDeath(BasePlayer player, HitInfo info);
    private void OnEntityMounted(ComputerStation station, BasePlayer player);
    private void OnEntityDismounted(ComputerStation station, BasePlayer player);
    private void OnNpcConversationStart(NPCTalking npcTalking, BasePlayer player, ConversationData conversationData);
    private void OnNpcConversationEnded(NPCTalking npcTalking, BasePlayer player);
    private class BackpacksApi
    {
        public static BackpacksApi Parse(Dictionary<string, object> dict);
        public Func<BasePlayer, bool> IsBackpackLoaded;
        public Func<BasePlayer, int> GetBackpackCapacity;
        public Func<BasePlayer, bool> IsBackpackGathering;
        public Func<BasePlayer, bool> IsBackpackRetrieving;
        public Func<ulong, Dictionary<string, object>, int> CountBackpackItems;
        private static void GetOption(Dictionary<string, object> dict, string key, T result);
    }

    private void BackpackButtonCommand(IPlayer player, string command, string[] args);
    private static class StringUtils
    {
        public static bool EqualsIgnoreCase(string a, string b);
    }

    private static void LogDebug(string message);
    private static void LogInfo(string message);
    private static void LogWarning(string message);
    private static void LogError(string message);
    private static bool IsKeyBindArg(string arg);
    private static bool VerifyPlayer(IPlayer player, BasePlayer basePlayer);
    private bool VerifyBackpacksVersion();
    private bool VerifyHasPermission(IPlayer player, string perm);
    private ButtonPosition GetPlayerButtonPosition(BasePlayer player);
    private void PrintCommandUsage(IPlayer player, BasePlayer basePlayer, string command);
    private void HandleBackpacksLoaded();
    private bool ShouldDisplayUi(BasePlayer player, ButtonPosition buttonPosition);
    private bool ShouldDisplayUi(BasePlayer player);
    private void CreateUiIfEnabled(BasePlayer player);
    private void DestroyUiIfActive(BasePlayer player);
    private void HandlePermissionChanged(BasePlayer player);
    private ButtonPosition GetMatchingButtonPosition(BasePlayer player, string positionName);
    private class UiUpdateManager
    {
        private const float UpdateDelaySeconds;
        private const float ProcessFrequencySeconds;
        private BackpackButton _plugin;
        private Action _updateAction;
        private Dictionary<BasePlayer, float> _scheduledUpdates;
        private List<BasePlayer> _playersProcessed;
        public UiUpdateManager(BackpackButton plugin);
        public void ScheduleUpdate(BasePlayer player);
        public void Unload();
        private void ProcessUpdates();
    }

    private sealed class DefaultStringCache : IStringCache
    {
        public static readonly DefaultStringCache Instance;
        private static class StaticStringCache
        {
            private static readonly Dictionary<T, string> _cacheByValue;
            public static string Get(T value);
        }

        private static class StaticStringCacheWithFactory
        {
            private static readonly Dictionary<Func<T, string>, Dictionary<T, string>> _cacheByDelegate;
            public static string Get(T value, Func<T, string> createString);
        }

        private DefaultStringCache();
        public string Get(T value);
        public string Get(bool value);
        public string Get(T value, Func<T, string> createString);
    }

    private class UiBuilder : IUiBuilder
    {
        private static NetWrite ClientRPCStart(BaseEntity entity, string funcName);
        public static readonly UiBuilder Default;
        public int Length { get; set; }
        private const char Delimiter;
        private const char Quote;
        private const char Colon;
        private const char Space;
        private const char OpenBracket;
        private const char CloseBracket;
        private const char OpenCurlyBrace;
        private const char CloseCurlyBrace;
        private const int MinCapacity;
        private const int DefaultCapacity;
        public IStringCache StringCache { get; set; }
        private char[] _chars;
        private byte[] _bytes;
        private State _state;
        private bool _needsDelimiter;
        public UiBuilder(int capacity, IStringCache stringCache);
        public UiBuilder(int capacity);
        public void Start();
        public void End();
        public void StartElement();
        public void EndElement();
        public void StartComponent();
        public void EndComponent();
        public void AddField(string key, T value);
        public void AddField(string key, string value);
        public void AddXY(string key, float x, float y);
        public void AddSerializable(T serializable);
        public void AddComponents(T components);
        public string ToJson();
        public byte[] GetBytes();
        public void AddUi(SendInfo sendInfo);
        public void AddUi(BasePlayer player);
        private void ValidateState(State desiredState);
        private void ValidateState(State desiredState, State alternateState);
        private void Resize(int length);
        private void ResizeIfApproachingLength();
        private void Append(char @char);
        private void Append(string str);
        private void AddDelimiter();
        private void AddDelimiterIfNeeded();
        private void StartObject();
        private void EndObject();
        private void StartArray();
        private void EndArray();
        private void AddKey(string key);
        private void Reset();
    }

    private static class Layout
    {
        public const string AnchorBottomLeft;
        public const string AnchorBottomRight;
        public const string AnchorTopLeft;
        public const string AnchorTopRight;
        public const string AnchorBottomCenter;
        public const string AnchorTopCenter;
        public const string AnchorCenterLeft;
        public const string AnchorCenterRight;
        public static string DetermineAnchor(Option options);
    }

    private static class ButtonUi
    {
        private const string Name;
        private static readonly string ButtonName;
        private const float YMin;
        private const float ButtonSize;
        public static void CreateUi(BackpackButton plugin, BasePlayer player, ButtonPosition buttonPosition);
        public static void UpdateUi(BackpackButton plugin, BasePlayer player);
        public static void DestroyUi(BasePlayer player);
        private static void AddButton(UiBuilder builder, BackpackButton plugin, BasePlayer player);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class SavedData
    {
        public static SavedData Load();
        [JsonIgnore]
        private bool _dirty;
        [JsonProperty("OverridePositionByPlayer")]
        private Dictionary<ulong, string> _overridePositionByPlayer;
        [JsonProperty("OverrideEnabledByPlayer")]
        private Dictionary<ulong, bool> _overrideEnabledByPlayer;
        public bool? GetEnabledPreference(ulong userId);
        public void SetEnabledPreference(ulong userId, bool enabled);
        public void RemoveEnabledPreference(ulong userId);
        public string GetPositionPreference(ulong userId);
        public void SetPositionPreference(ulong userId, string positionName);
        public void RemovePositionPreference(ulong userId);
        public void SaveIfChanged();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class ButtonPosition
    {
        [JsonProperty("Name")]
        public string Name;
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Offset X")]
        public float OffsetX;
        [JsonProperty("URL")]
        public string Url;
        [JsonProperty("Skin ID")]
        public ulong SkinId;
        [JsonProperty("Image size")]
        public float ImageSize;
        public string LangKey { get; set; }
        private string _langKey;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class BackgroundSettings
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Color")]
        public string Color;
        [JsonProperty("Sprite")]
        public string Sprite;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private abstract class DynamicColorConfig
    {
        [JsonProperty("Default color")]
        protected abstract string DefaultColor { get; set; }
        [JsonProperty("Enable dynamic color")]
        protected abstract bool EnableDynamicColor { get; set; }
        [JsonProperty("Dynamic color by fullness percent", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        protected abstract Dictionary<int, string> ColorByFullPercent { get; set; }
        private List<Tuple<int, string>> _sortedColorByFullPercent;
        public void Init();
        public string GetColor(int fullPercent);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class SlotSettings : DynamicColorConfig
    {
        [JsonProperty("Show occupied slots")]
        public bool ShowOccupiedSlots;
        [JsonProperty("Show total slots")]
        public bool ShowTotalSlots;
        [JsonProperty("Offset min")]
        public string OffsetMin;
        [JsonProperty("Offset max")]
        public string OffsetMax;
        [JsonProperty("Text align")]
        [JsonConverter(typeof(StringEnumConverter))]
        public TextAnchor TextAnchor;
        public bool Enabled { get; set; }
        protected override string DefaultColor { get; set; }
        protected override bool EnableDynamicColor { get; set; }
        protected override Dictionary<int, string> ColorByFullPercent { get; set; }
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class FillBarSettings : DynamicColorConfig
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Width")]
        public float Width;
        [JsonProperty("Sprite")]
        public string Sprite;
        protected override string DefaultColor { get; set; }
        protected override bool EnableDynamicColor { get; set; }
        protected override Dictionary<int, string> ColorByFullPercent { get; set; }
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class GatherModeSettings
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Color")]
        public string Color;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class RetrieveModeSettings
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Color")]
        public string Color;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : BaseConfiguration
    {
        [JsonProperty("Commands")]
        public string[] Commands;
        [JsonProperty("Background")]
        public BackgroundSettings Background;
        [JsonProperty("Gather mode indicator")]
        public GatherModeSettings GatherMode;
        [JsonProperty("Retrieve mode indicator")]
        public RetrieveModeSettings RetrieveMode;
        [JsonProperty("Occupied & total slots")]
        public SlotSettings Slots;
        [JsonProperty("Fill bar")]
        public FillBarSettings FillBar;
        [JsonProperty("Default button position")]
        public string DefaultButtonPositionName;
        [JsonProperty("Button positions")]
        public ButtonPosition[] ButtonPositions;
        public ButtonPosition DefaultButtonPosition { get; set; }
        public int NumValidPositions { get; set; }
        public void Init();
        public ButtonPosition GetButtonPosition(string positionName);
    }

    private Configuration GetDefaultConfig();
    [JsonObject(MemberSerialization.OptIn)]
    private class BaseConfiguration
    {
        private string ToJson();
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
    private string GetMessage(string playerId, string langKey);
    private string GetMessage(IPlayer player, string langKey);
    private string GetMessage(BasePlayer basePlayer, string langKey);
    protected override void LoadDefaultMessages();
}

private class BackpacksApi
{
    public static BackpacksApi Parse(Dictionary<string, object> dict);
    public Func<BasePlayer, bool> IsBackpackLoaded;
    public Func<BasePlayer, int> GetBackpackCapacity;
    public Func<BasePlayer, bool> IsBackpackGathering;
    public Func<BasePlayer, bool> IsBackpackRetrieving;
    public Func<ulong, Dictionary<string, object>, int> CountBackpackItems;
    private static void GetOption(Dictionary<string, object> dict, string key, T result);
}

private static class StringUtils
{
    public static bool EqualsIgnoreCase(string a, string b);
}

private class UiUpdateManager
{
    private const float UpdateDelaySeconds;
    private const float ProcessFrequencySeconds;
    private BackpackButton _plugin;
    private Action _updateAction;
    private Dictionary<BasePlayer, float> _scheduledUpdates;
    private List<BasePlayer> _playersProcessed;
    public UiUpdateManager(BackpackButton plugin);
    public void ScheduleUpdate(BasePlayer player);
    public void Unload();
    private void ProcessUpdates();
}

private sealed class DefaultStringCache : IStringCache
{
    public static readonly DefaultStringCache Instance;
    private static class StaticStringCache
    {
        private static readonly Dictionary<T, string> _cacheByValue;
        public static string Get(T value);
    }

    private static class StaticStringCacheWithFactory
    {
        private static readonly Dictionary<Func<T, string>, Dictionary<T, string>> _cacheByDelegate;
        public static string Get(T value, Func<T, string> createString);
    }

    private DefaultStringCache();
    public string Get(T value);
    public string Get(bool value);
    public string Get(T value, Func<T, string> createString);
}

private static class StaticStringCache
{
    private static readonly Dictionary<T, string> _cacheByValue;
    public static string Get(T value);
}

private static class StaticStringCacheWithFactory
{
    private static readonly Dictionary<Func<T, string>, Dictionary<T, string>> _cacheByDelegate;
    public static string Get(T value, Func<T, string> createString);
}

private class UiBuilder : IUiBuilder
{
    private static NetWrite ClientRPCStart(BaseEntity entity, string funcName);
    public static readonly UiBuilder Default;
    public int Length { get; set; }
    private const char Delimiter;
    private const char Quote;
    private const char Colon;
    private const char Space;
    private const char OpenBracket;
    private const char CloseBracket;
    private const char OpenCurlyBrace;
    private const char CloseCurlyBrace;
    private const int MinCapacity;
    private const int DefaultCapacity;
    public IStringCache StringCache { get; set; }
    private char[] _chars;
    private byte[] _bytes;
    private State _state;
    private bool _needsDelimiter;
    public UiBuilder(int capacity, IStringCache stringCache);
    public UiBuilder(int capacity);
    public void Start();
    public void End();
    public void StartElement();
    public void EndElement();
    public void StartComponent();
    public void EndComponent();
    public void AddField(string key, T value);
    public void AddField(string key, string value);
    public void AddXY(string key, float x, float y);
    public void AddSerializable(T serializable);
    public void AddComponents(T components);
    public string ToJson();
    public byte[] GetBytes();
    public void AddUi(SendInfo sendInfo);
    public void AddUi(BasePlayer player);
    private void ValidateState(State desiredState);
    private void ValidateState(State desiredState, State alternateState);
    private void Resize(int length);
    private void ResizeIfApproachingLength();
    private void Append(char @char);
    private void Append(string str);
    private void AddDelimiter();
    private void AddDelimiterIfNeeded();
    private void StartObject();
    private void EndObject();
    private void StartArray();
    private void EndArray();
    private void AddKey(string key);
    private void Reset();
}

private static class Layout
{
    public const string AnchorBottomLeft;
    public const string AnchorBottomRight;
    public const string AnchorTopLeft;
    public const string AnchorTopRight;
    public const string AnchorBottomCenter;
    public const string AnchorTopCenter;
    public const string AnchorCenterLeft;
    public const string AnchorCenterRight;
    public static string DetermineAnchor(Option options);
}

private static class ButtonUi
{
    private const string Name;
    private static readonly string ButtonName;
    private const float YMin;
    private const float ButtonSize;
    public static void CreateUi(BackpackButton plugin, BasePlayer player, ButtonPosition buttonPosition);
    public static void UpdateUi(BackpackButton plugin, BasePlayer player);
    public static void DestroyUi(BasePlayer player);
    private static void AddButton(UiBuilder builder, BackpackButton plugin, BasePlayer player);
}

[JsonObject(MemberSerialization.OptIn)]
private class SavedData
{
    public static SavedData Load();
    [JsonIgnore]
    private bool _dirty;
    [JsonProperty("OverridePositionByPlayer")]
    private Dictionary<ulong, string> _overridePositionByPlayer;
    [JsonProperty("OverrideEnabledByPlayer")]
    private Dictionary<ulong, bool> _overrideEnabledByPlayer;
    public bool? GetEnabledPreference(ulong userId);
    public void SetEnabledPreference(ulong userId, bool enabled);
    public void RemoveEnabledPreference(ulong userId);
    public string GetPositionPreference(ulong userId);
    public void SetPositionPreference(ulong userId, string positionName);
    public void RemovePositionPreference(ulong userId);
    public void SaveIfChanged();
}

[JsonObject(MemberSerialization.OptIn)]
private class ButtonPosition
{
    [JsonProperty("Name")]
    public string Name;
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Offset X")]
    public float OffsetX;
    [JsonProperty("URL")]
    public string Url;
    [JsonProperty("Skin ID")]
    public ulong SkinId;
    [JsonProperty("Image size")]
    public float ImageSize;
    public string LangKey { get; set; }
    private string _langKey;
}

[JsonObject(MemberSerialization.OptIn)]
private class BackgroundSettings
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Color")]
    public string Color;
    [JsonProperty("Sprite")]
    public string Sprite;
}

[JsonObject(MemberSerialization.OptIn)]
private abstract class DynamicColorConfig
{
    [JsonProperty("Default color")]
    protected abstract string DefaultColor { get; set; }
    [JsonProperty("Enable dynamic color")]
    protected abstract bool EnableDynamicColor { get; set; }
    [JsonProperty("Dynamic color by fullness percent", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    protected abstract Dictionary<int, string> ColorByFullPercent { get; set; }
    private List<Tuple<int, string>> _sortedColorByFullPercent;
    public void Init();
    public string GetColor(int fullPercent);
}

[JsonObject(MemberSerialization.OptIn)]
private class SlotSettings : DynamicColorConfig
{
    [JsonProperty("Show occupied slots")]
    public bool ShowOccupiedSlots;
    [JsonProperty("Show total slots")]
    public bool ShowTotalSlots;
    [JsonProperty("Offset min")]
    public string OffsetMin;
    [JsonProperty("Offset max")]
    public string OffsetMax;
    [JsonProperty("Text align")]
    [JsonConverter(typeof(StringEnumConverter))]
    public TextAnchor TextAnchor;
    public bool Enabled { get; set; }
    protected override string DefaultColor { get; set; }
    protected override bool EnableDynamicColor { get; set; }
    protected override Dictionary<int, string> ColorByFullPercent { get; set; }
}

[JsonObject(MemberSerialization.OptIn)]
private class FillBarSettings : DynamicColorConfig
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Width")]
    public float Width;
    [JsonProperty("Sprite")]
    public string Sprite;
    protected override string DefaultColor { get; set; }
    protected override bool EnableDynamicColor { get; set; }
    protected override Dictionary<int, string> ColorByFullPercent { get; set; }
}

[JsonObject(MemberSerialization.OptIn)]
private class GatherModeSettings
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Color")]
    public string Color;
}

[JsonObject(MemberSerialization.OptIn)]
private class RetrieveModeSettings
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Color")]
    public string Color;
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : BaseConfiguration
{
    [JsonProperty("Commands")]
    public string[] Commands;
    [JsonProperty("Background")]
    public BackgroundSettings Background;
    [JsonProperty("Gather mode indicator")]
    public GatherModeSettings GatherMode;
    [JsonProperty("Retrieve mode indicator")]
    public RetrieveModeSettings RetrieveMode;
    [JsonProperty("Occupied & total slots")]
    public SlotSettings Slots;
    [JsonProperty("Fill bar")]
    public FillBarSettings FillBar;
    [JsonProperty("Default button position")]
    public string DefaultButtonPositionName;
    [JsonProperty("Button positions")]
    public ButtonPosition[] ButtonPositions;
    public ButtonPosition DefaultButtonPosition { get; set; }
    public int NumValidPositions { get; set; }
    public void Init();
    public ButtonPosition GetButtonPosition(string positionName);
}

[JsonObject(MemberSerialization.OptIn)]
private class BaseConfiguration
{
    private string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}


```

---

## Backpacks by WhiteThunder - Allows players to have backpacks that provide them with extra inventory space

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections;
using System.Collections.Generic;
using System.Globalization;
using System.IO;
using System.Linq;
using System.Text;
using Facepunch;
using Network;
using Newtonsoft.Json.Converters;
using Oxide.Core.Configuration;
using Oxide.Core.Libraries;
using Rust;
using UnityEngine;
using UnityEngine.UI;
using Time = UnityEngine.Time;

Oxide.Plugins
[Info("Backpacks", "WhiteThunder", "3.15.0")]
[Description("Allows players to have a Backpack which provides them extra inventory space.")]
internal class Backpacks : CovalencePlugin
{
    private static int _maxCapacityPerPage;
    private const int MinRows;
    private const int MaxRows;
    private const int MinContainerCapacity;
    private const int MaxContainerCapacity;
    private const int SlotsPerRow;
    private const int ReclaimEntryMaxSize;
    private const float StandardLootDelay;
    private const Item.Flag SearchableItemFlag;
    private const Item.Flag UnsearchableItemFlag;
    private const ItemDefinition.Flag SearchableItemDefinitionFlag;
    private const string UsagePermission;
    private const string SizePermission;
    private const string GUIPermission;
    private const string FetchPermission;
    private const string GatherPermission;
    private const string RetrievePermission;
    private const string AdminPermission;
    private const string AdminProtectedPermission;
    private const string CapacityProfilePermission;
    private const string KeepOnDeathPermission;
    private const string LegacyKeepOnWipePermission;
    private const string LegacyNoBlacklistPermission;
    private const string CoffinPrefab;
    private const string DroppedBackpackPrefab;
    private const string ResizableLootPanelName;
    private const int SaddleBagItemId;
    private readonly CapacityManager _capacityManager;
    private readonly BackpackManager _backpackManager;
    private readonly SubscriberManager _subscriberManager;
    private ProtectionProperties _immortalProtection;
    private Effect _reusableEffect;
    private string _cachedButtonUi;
    private readonly ApiInstance _api;
    private Configuration _config;
    private PreferencesData _preferencesData;
    private CapacityData _capacityData;
    private readonly HashSet<ulong> _uiViewers;
    private Coroutine _saveRoutine;
    [PluginReference]
    private readonly Plugin Arena;
    private readonly Plugin BackpackButton;
    private readonly Plugin EventManager;
    private readonly Plugin ItemRetriever;
    public Backpacks();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnNewSave(string filename);
    private void OnServerSave();
    private void OnPluginLoaded(Plugin plugin);
    private void OnPluginUnloaded(Plugin plugin);
    private void OnPlayerDisconnected(BasePlayer player);
    private void OnEntityDeath(BasePlayer player, HitInfo info);
    private void OnEntityKill(BasePlayer player);
    private void OnGroupPermissionGranted(string groupName, string perm);
    private void OnGroupPermissionRevoked(string groupName, string perm);
    private void OnUserPermissionGranted(string userId, string perm);
    private void OnUserPermissionRevoked(string userId, string perm);
    private void OnUserGroupAdded(string userId, string groupName);
    private void OnUserGroupRemoved(string userId, string groupName);
    private void OnPlayerConnected(BasePlayer player);
    private void OnPlayerRespawned(BasePlayer player);
    private void OnPlayerSleepEnded(BasePlayer player);
    private void OnPlayerSleep(BasePlayer player);
    private void OnNpcConversationStart(NPCTalking npcTalking, BasePlayer player, ConversationData conversationData);
    private void OnNpcConversationEnded(NPCTalking npcTalking, BasePlayer player);
    private void OnNetworkSubscriptionsUpdate(Networkable networkable, List<Network.Visibility.Group> groupsToAdd, List<Network.Visibility.Group> groupsToRemove);
    private class ApiInstance
    {
        public readonly Dictionary<string, object> ApiWrapper;
        private readonly Backpacks _plugin;
        private BackpackManager _backpackManager { get; set; }
        public ApiInstance(Backpacks plugin);
        public void AddSubscriber(Plugin plugin, Dictionary<string, object> spec);
        public void RemoveSubscriber(Plugin plugin);
        public Dictionary<ulong, ItemContainer> GetExistingBackpacks();
        public void EraseBackpack(ulong userId);
        public DroppedItemContainer DropBackpack(BasePlayer player, List<DroppedItemContainer> collect);
        public ulong GetBackpackOwnerId(ItemContainer container);
        public bool IsBackpackLoaded(BasePlayer player);
        public bool IsDynamicCapacityEnabled();
        public int GetBackpackCapacity(BasePlayer player);
        public int GetBackpackCapacityById(ulong playerID, string playerIDString);
        public int GetBackpackInitialCapacity(BasePlayer player);
        public int GetBackpackMaxCapacity(BasePlayer player);
        public int AddBackpackCapacity(BasePlayer player, int amount);
        public int SetBackpackCapacity(BasePlayer player, int capacity);
        public bool IsBackpackGathering(BasePlayer player);
        public bool IsBackpackRetrieving(BasePlayer player);
        public ItemContainer GetBackpackContainer(ulong ownerId);
        public int GetBackpackItemAmount(ulong ownerId, int itemId, ulong skinId);
        public bool TryOpenBackpack(BasePlayer player, ulong ownerId);
        public bool TryOpenBackpackContainer(BasePlayer player, ulong ownerId, ItemContainer container);
        public bool TryOpenBackpackPage(BasePlayer player, ulong ownerId, int page);
        public int SumBackpackItems(ulong ownerId, Dictionary<string, object> dict);
        public int CountBackpackItems(ulong ownerId, Dictionary<string, object> dict);
        public int TakeBackpackItems(ulong ownerId, Dictionary<string, object> dict, int amount, List<Item> collect);
        public int MutateBackpackItems(ulong ownerId, Dictionary<string, object> itemQueryDict, Dictionary<string, object> mutationRequestDict);
        public bool TryDepositBackpackItem(ulong ownerId, Item item);
        public void WriteBackpackContentsFromJson(ulong ownerId, string json);
        public string ReadBackpackContentsAsJson(ulong ownerId);
    }

    [HookMethod(nameof(API_GetApi))]
    public Dictionary<string, object> API_GetApi();
    [HookMethod(nameof(API_AddSubscriber))]
    public void API_AddSubscriber(Plugin plugin, Dictionary<string, object> spec);
    [HookMethod(nameof(API_RemoveSubscriber))]
    public void API_RemoveSubscriber(Plugin plugin);
    [HookMethod(nameof(API_GetExistingBackpacks))]
    public Dictionary<ulong, ItemContainer> API_GetExistingBackpacks();
    [HookMethod(nameof(API_EraseBackpack))]
    public void API_EraseBackpack(ulong userId);
    [HookMethod(nameof(API_EraseBackpack))]
    public void API_EraseBackpack(EncryptedValue<ulong> userId);
    [HookMethod(nameof(API_DropBackpack))]
    public DroppedItemContainer API_DropBackpack(BasePlayer player, List<DroppedItemContainer> collect);
    [HookMethod(nameof(API_GetBackpackOwnerId))]
    public object API_GetBackpackOwnerId(ItemContainer container);
    [HookMethod(nameof(API_IsBackpackLoaded))]
    public object API_IsBackpackLoaded(BasePlayer player);
    [HookMethod(nameof(API_IsDynamicCapacityEnabled))]
    public object API_IsDynamicCapacityEnabled(BasePlayer player);
    [HookMethod(nameof(API_GetBackpackCapacity))]
    public object API_GetBackpackCapacity(BasePlayer player);
    [HookMethod(nameof(API_GetBackpackCapacityById))]
    public object API_GetBackpackCapacityById(ulong playerID, string playerIDString);
    [HookMethod(nameof(API_GetBackpackCapacityById))]
    public object API_GetBackpackCapacityById(EncryptedValue<ulong> playerID, string playerIDString);
    [HookMethod(nameof(API_GetBackpackInitialCapacity))]
    public object API_GetBackpackInitialCapacity(BasePlayer player);
    [HookMethod(nameof(API_GetBackpackMaxCapacity))]
    public object API_GetBackpackMaxCapacity(BasePlayer player);
    [HookMethod(nameof(API_AddBackpackCapacity))]
    public object API_AddBackpackCapacity(BasePlayer player, int amount);
    [HookMethod(nameof(API_SetBackpackCapacity))]
    public object API_SetBackpackCapacity(BasePlayer player, int capacity);
    [HookMethod(nameof(API_IsBackpackGathering))]
    public object API_IsBackpackGathering(BasePlayer player);
    [HookMethod(nameof(API_IsBackpackRetrieving))]
    public object API_IsBackpackRetrieving(BasePlayer player);
    [HookMethod(nameof(API_GetBackpackContainer))]
    public ItemContainer API_GetBackpackContainer(ulong ownerId);
    [HookMethod(nameof(API_GetBackpackContainer))]
    public ItemContainer API_GetBackpackContainer(EncryptedValue<ulong> ownerId);
    [HookMethod(nameof(API_GetBackpackItemAmount))]
    public int API_GetBackpackItemAmount(ulong ownerId, int itemId, ulong skinId);
    [HookMethod(nameof(API_GetBackpackItemAmount))]
    public int API_GetBackpackItemAmount(EncryptedValue<ulong> ownerId, int itemId, ulong skinId);
    [HookMethod(nameof(API_TryOpenBackpack))]
    public object API_TryOpenBackpack(BasePlayer player, ulong ownerId);
    [HookMethod(nameof(API_TryOpenBackpack))]
    public object API_TryOpenBackpack(BasePlayer player, EncryptedValue<ulong> ownerId);
    [HookMethod(nameof(API_TryOpenBackpackContainer))]
    public object API_TryOpenBackpackContainer(BasePlayer player, ulong ownerId, ItemContainer container);
    [HookMethod(nameof(API_TryOpenBackpackContainer))]
    public object API_TryOpenBackpackContainer(BasePlayer player, EncryptedValue<ulong> ownerId, ItemContainer container);
    [HookMethod(nameof(API_TryOpenBackpackPage))]
    public object API_TryOpenBackpackPage(BasePlayer player, ulong ownerId, int page);
    [HookMethod(nameof(API_TryOpenBackpackPage))]
    public object API_TryOpenBackpackPage(BasePlayer player, EncryptedValue<ulong> ownerId, int page);
    [HookMethod(nameof(API_SumBackpackItems))]
    public object API_SumBackpackItems(ulong ownerId, Dictionary<string, object> dict);
    [HookMethod(nameof(API_SumBackpackItems))]
    public object API_SumBackpackItems(EncryptedValue<ulong> ownerId, Dictionary<string, object> dict);
    [HookMethod(nameof(API_CountBackpackItems))]
    public object API_CountBackpackItems(ulong ownerId, Dictionary<string, object> dict);
    [HookMethod(nameof(API_CountBackpackItems))]
    public object API_CountBackpackItems(EncryptedValue<ulong> ownerId, Dictionary<string, object> dict);
    [HookMethod(nameof(API_TakeBackpackItems))]
    public object API_TakeBackpackItems(ulong ownerId, Dictionary<string, object> dict, int amount, List<Item> collect);
    [HookMethod(nameof(API_TakeBackpackItems))]
    public object API_TakeBackpackItems(EncryptedValue<ulong> ownerId, Dictionary<string, object> dict, int amount, List<Item> collect);
    [HookMethod(nameof(API_MutateBackpackItems))]
    public object API_MutateBackpackItems(ulong ownerId, Dictionary<string, object> itemQueryDict, Dictionary<string, object> mutationRequestDict);
    [HookMethod(nameof(API_MutateBackpackItems))]
    public object API_MutateBackpackItems(EncryptedValue<ulong> ownerId, Dictionary<string, object> itemQueryDict, Dictionary<string, object> mutationRequestDict);
    [HookMethod(nameof(API_TryDepositBackpackItem))]
    public object API_TryDepositBackpackItem(ulong ownerId, Item item);
    [HookMethod(nameof(API_TryDepositBackpackItem))]
    public object API_TryDepositBackpackItem(EncryptedValue<ulong> ownerId, Item item);
    [HookMethod(nameof(API_WriteBackpackContentsFromJson))]
    public void API_WriteBackpackContentsFromJson(ulong ownerId, string json);
    [HookMethod(nameof(API_WriteBackpackContentsFromJson))]
    public void API_WriteBackpackContentsFromJson(EncryptedValue<ulong> ownerId, string json);
    [HookMethod(nameof(API_ReadBackpackContentsAsJson))]
    public object API_ReadBackpackContentsAsJson(ulong ownerId);
    [HookMethod(nameof(API_ReadBackpackContentsAsJson))]
    public object API_ReadBackpackContentsAsJson(EncryptedValue<ulong> ownerId);
    private static class ExposedHooks
    {
        public static object CanOpenBackpack(BasePlayer looter, ulong ownerId);
        public static void OnBackpackClosed(BasePlayer looter, ulong ownerId, ItemContainer container);
        public static void OnBackpackOpened(BasePlayer looter, ulong ownerId, ItemContainer container);
        public static object CanDropBackpack(ulong ownerId, Vector3 position);
        public static void OnBackpackDropped(ulong ownerId, List<DroppedItemContainer> droppedBackpackList);
        public static object CanEraseBackpack(ulong ownerId);
        public static object CanBackpackAcceptItem(ulong ownerId, ItemContainer container, Item item);
    }

    [Command("backpack", "backpack.open")]
    private void BackpackOpenCommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.next")]
    private void BackpackNextCommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.previous", "backpack.prev")]
    private void BackpackPreviousCommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.fetch")]
    private void BackpackFetchCommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.erase")]
    private void EraseBackpackCommand(IPlayer player, string cmd, string[] args);
    [Command("viewbackpack")]
    private void ViewBackpackCommand(IPlayer player, string cmd, string[] args);
    private void ViewBackpack(BasePlayer player, string cmd, string[] args);
    [Command("backpack.addsize")]
    private void AddBackpackCapacityCommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.setsize")]
    private void SetBackpackCapacityCommand(IPlayer player, string cmd, string[] args);
    private void ToggleBackpackGUICommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.setgathermode")]
    private void SetGatherCommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.ui.togglegather")]
    private void ToggleGatherUICommand(IPlayer player, string cmd, string[] args);
    [Command("backpack.ui.toggleretrieve")]
    private void ToggleRetrieveUICommand(IPlayer player, string cmd, string[] args);
    public static void LogDebug(string message);
    public static void LogInfo(string message);
    public static void LogWarning(string message);
    public static void LogError(string message);
    private static T[] ParseEnumList(string[] list, string errorFormat);
    private static bool IsKeyBindArg(string arg);
    private static int ParsePageArg(string arg);
    private static bool HasItemMod(ItemDefinition itemDefinition, T itemModOfType);
    private static string DetermineLootPanelName(ItemContainer container);
    private static void ClosePlayerInventory(BasePlayer player);
    private static float CalculateOpenDelay(ItemContainer currentContainer, int nextContainerCapacity, bool isKeyBind);
    private static void StartLooting(BasePlayer player, ItemContainer container, StorageContainer entitySource);
    private static ItemContainer GetRootContainer(Item item);
    private bool TryParseGatherMode(BasePlayer player, string arg, GatherMode gatherMode);
    private string GetGatherModeDisplayOptions(BasePlayer player);
    private void SendEffect(BasePlayer player, string effectPrefab);
    private void CheckBackpackButtonPlugin();
    private void RegisterAsItemSupplier();
    private IEnumerator SaveRoutine(bool async, bool keepInUseBackpacks);
    private void RestartSaveRoutine(bool async, bool keepInUseBackpacks);
    private bool IsLootingBackpack(BasePlayer player, Backpack backpack, int pageIndex);
    private void OpenBackpackMaybeDelayed(BasePlayer looter, ItemContainer currentContainer, Backpack backpack, int pageIndex, bool isKeyBind);
    private void OpenBackpack(BasePlayer looter, bool isKeyBind, int desiredPageIndex, bool forward, bool wrapAround, ulong desiredOwnerId);
    private bool ShouldDisplayGuiButton(BasePlayer player);
    private IPlayer FindPlayer(IPlayer requester, string nameOrID, string failureMessage);
    private bool VerifyPlayer(IPlayer player, BasePlayer basePlayer);
    private bool VerifyTargetPlayer(IPlayer requester, string playerArg, ulong userId, string userIdString);
    private bool VerifyHasPermission(IPlayer player, string perm);
    private bool VerifyValidItem(IPlayer player, string itemArg, ItemDefinition itemDefinition);
    private bool VerifyCanInteract(IPlayer player, BasePlayer basePlayer);
    private bool VerifyCanOpenBackpack(BasePlayer looter, ulong ownerId, bool provideFeedback);
    private bool IsPlayingEvent(BasePlayer player);
    private bool VerifyDynamicCapacityEnabled(IPlayer player);
    private void MaybeCreateButtonUi(BasePlayer player);
    private void DestroyButtonUi(BasePlayer player);
    private void CreateOrDestroyButtonUi(BasePlayer player);
    private static class StringUtils
    {
        public static bool EqualsCaseInsensitive(string a, string b, StringComparison stringComparison);
        public static bool Contains(string haystack, string needle);
    }

    private static class ObjectCache
    {
        private static readonly object True;
        private static readonly object False;
        private static class StaticObjectCache
        {
            private static readonly Dictionary<T, object> _cacheByValue;
            public static object Get(T value);
        }

        public static object Get(T value);
        public static object Get(bool value);
    }

    private class PoolConverter : CustomCreationConverter<T>
    {
        public override T Create(Type objectType);
    }

    private class PoolListConverter : CustomCreationConverter<List<T>>
    {
        public override List<T> Create(Type objectType);
    }

    private static class ItemUtils
    {
        public static int PositionOf(List<Item> itemList, ItemQuery itemQuery);
        public static int PositionOf(List<ItemData> itemDataList, ItemQuery itemQuery);
        public static void FindItems(List<Item> itemList, ItemQuery itemQuery, List<Item> collect);
        public static int CountItems(List<Item> itemList, ItemQuery itemQuery);
        public static int CountItems(List<ItemData> itemDataList, ItemQuery itemQuery);
        public static int SumItems(List<Item> itemList, ItemQuery itemQuery);
        public static int SumItems(List<ItemData> itemDataList, ItemQuery itemQuery);
        public static int TakeItems(List<Item> itemList, ItemQuery itemQuery, int amount, List<Item> collect);
        public static int TakeItems(List<ItemData> itemDataList, ItemQuery itemQuery, int amount, List<Item> collect);
        public static int MutateItems(List<Item> itemList, ItemQuery itemQuery, MutationRequest mutationRequest);
        public static int MutateItems(List<ItemData> itemDataList, ItemQuery itemQuery, MutationRequest mutationRequest);
        public static void SerializeForNetwork(List<Item> itemList, List<ProtoBuf.Item> collect);
        public static void SerializeForNetwork(List<ItemData> itemDataList, List<ProtoBuf.Item> collect);
        private static bool IsSearchableItemDefinition(ItemDefinition itemDefinition);
        private static bool IsSearchableItemDefinition(int itemId);
        private static bool HasSearchableContainer(Item item, List<Item> itemList);
        private static bool HasSearchableContainer(ItemData itemData, List<ItemData> itemDataList);
        private static void TakeItemAmount(Item item, int amount, List<Item> collect);
    }

    private static class CustomPool
    {
        private static class StaticPool
        {
            public static readonly PoolCollection<T> Collection;
        }

        private class PoolCollection
        {
            public const int DefaultPoolSize;
            private T[] _buffer;
            public int ItemsCreated { get; set; }
            public int ItemsInStack { get; set; }
            public int ItemsInUse { get; set; }
            public int ItemsSpilled { get; set; }
            public int ItemsTaken { get; set; }
            public PoolCollection();
            public void Reset(int size);
            public void Add(T obj);
            public T Take();
        }

        public static void Reset(int size);
        public static string GetStats();
        public static T Get();
        public static List<T> GetList();
        public static void Free(T obj);
        public static void FreeList(List<T> list);
        private static void FreeInternal(T obj);
    }

    private static class PoolUtils
    {
        public const int BackpackPoolSize;
        public static void ResetItemsAndClear(IList<T> list);
        public static void ResizePools(bool empty);
    }

    private class DisposableList : List<T>, IDisposable
    {
        public static DisposableList<T> Get();
        public void Dispose();
    }

    private sealed class DefaultStringCache : IStringCache
    {
        public static readonly DefaultStringCache Instance;
        private static class StaticStringCache
        {
            private static readonly Dictionary<T, string> _cacheByValue;
            public static string Get(T value);
        }

        private static class StaticStringCacheWithFactory
        {
            private static readonly Dictionary<Func<T, string>, Dictionary<T, string>> _cacheByDelegate;
            public static string Get(T value, Func<T, string> createString);
        }

        private DefaultStringCache();
        public string Get(T value);
        public string Get(bool value);
        public string Get(T value, Func<T, string> createString);
    }

    private class UiBuilder : IUiBuilder
    {
        private static NetWrite ClientRPCStart(BaseEntity entity, string funcName);
        public static readonly UiBuilder Default;
        public int Length { get; set; }
        private const char Delimiter;
        private const char Quote;
        private const char Colon;
        private const char Space;
        private const char OpenBracket;
        private const char CloseBracket;
        private const char OpenCurlyBrace;
        private const char CloseCurlyBrace;
        private const int MinCapacity;
        private const int DefaultCapacity;
        public IStringCache StringCache { get; set; }
        private char[] _chars;
        private byte[] _bytes;
        private State _state;
        private bool _needsDelimiter;
        public UiBuilder(int capacity, IStringCache stringCache);
        public UiBuilder(int capacity);
        public void Start();
        public void End();
        public void StartElement();
        public void EndElement();
        public void StartComponent();
        public void EndComponent();
        public void AddField(string key, T value);
        public void AddField(string key, string value);
        public void AddXY(string key, float x, float y);
        public void AddSerializable(T serializable);
        public void AddComponents(T components);
        public string ToJson();
        public byte[] GetBytes();
        public void AddUi(SendInfo sendInfo);
        public void AddUi(BasePlayer player);
        private void ValidateState(State desiredState);
        private void ValidateState(State desiredState, State alternateState);
        private void Resize(int length);
        private void ResizeIfApproachingLength();
        private void Append(char @char);
        private void Append(string str);
        private void AddDelimiter();
        private void AddDelimiterIfNeeded();
        private void StartObject();
        private void EndObject();
        private void StartArray();
        private void EndArray();
        private void AddKey(string key);
        private void Reset();
    }

    private static class Layout
    {
        public const string AnchorBottomLeft;
        public const string AnchorBottomRight;
        public const string AnchorTopLeft;
        public const string AnchorTopRight;
        public const string AnchorBottomCenter;
        public const string AnchorTopCenter;
        public const string AnchorCenterLeft;
        public const string AnchorCenterRight;
        public static string DetermineAnchor(Option options);
    }

    private static class ContainerUi
    {
        public const float BaseOffsetY;
        public const float BaseOffsetX;
        public const float HeaderWidth;
        public const float HeaderHeight;
        public const float PerRowOffsetY;
        private const float PageButtonSpacing;
        private const float PageButtonSize;
        private const string BlueButtonColor;
        private const string BlueButtonTextColor;
        private const string GreenButtonColor;
        private const string GreenButtonTextColor;
        private const string Name;
        private static readonly string LeftButtonName;
        private static readonly string RightButtonName;
        public static void CreateContainerUi(BasePlayer player, int numPages, int activePageIndex, int capacity, Backpack backpack);
        public static void DestroyUi(BasePlayer player);
        private static void AddGatherModeButton(UiBuilder builder, StatefulLayoutProvider layoutProvider, BasePlayer player, Backpack backpack, int activePageIndex);
        private static void AddRetrieveButton(UiBuilder builder, StatefulLayoutProvider layoutProvider, BasePlayer player, Backpack backpack, int activePageIndex);
        private static void AddPageButton(UiBuilder builder, PageButton pageButton);
        private static void AddPaginationUi(UiBuilder builder, Backpack backpack, int numPages, int activePageIndex);
    }

    private static class ButtonUi
    {
        private const string Name;
        public static string CreateButtonUi(Configuration config);
        public static void DestroyUi(BasePlayer player);
    }

    private class EventSubscriber
    {
        public static EventSubscriber FromSpec(Plugin plugin, Dictionary<string, object> spec);
        private static void GetOption(Dictionary<string, object> dict, string key, T result);
        public Plugin Plugin { get; set; }
        public Action<BasePlayer, int, int> OnBackpackLoaded;
        public Action<BasePlayer, int, int> OnBackpackItemCountChanged;
        public Action<BasePlayer, bool> OnBackpackGatherChanged;
        public Action<BasePlayer, bool> OnBackpackRetrieveChanged;
    }

    private class SubscriberManager
    {
        private readonly Dictionary<string, EventSubscriber> _subscribers;
        public void AddSubscriber(Plugin plugin, Dictionary<string, object> spec);
        public void RemoveSubscriber(Plugin plugin);
        public void BroadcastBackpackLoaded(Backpack backpack);
        public void BroadcastItemCountChanged(Backpack backpack);
        public void BroadcastGatherChanged(Backpack backpack, bool isGathering);
        public void BroadcastRetrieveChanged(Backpack backpack, bool isRetrieving);
    }

    private class CapacityManager
    {
        private class BackpackSize
        {
            public readonly int Capacity;
            public readonly string Permission;
            public BackpackSize(int capacity, string permission);
        }

        private readonly Backpacks _plugin;
        private Configuration _config;
        private BackpackManager _backpackManager;
        private CapacityData _capacityData;
        private BackpackSize[] _sortedBackpackSizes;
        private readonly Dictionary<ulong, CapacityInfo> _cachedPlayerCapacityInfo;
        public CapacityManager(Backpacks plugin, BackpackManager backpackManager);
        public void Init(Configuration config, CapacityData capacityData);
        public void ForgetCachedCapacity(ulong userId);
        public int GetCapacity(ulong userId, string userIdString);
        public int GetInitialCapacity(ulong userId, string userIdString);
        public int GetMaxCapacity(ulong userId, string userIdString);
        public int SetCapacity(ulong userId, string userIdString, int amount);
        public int SetCapacity(BasePlayer player, int amount);
        public int AddCapacity(ulong userId, string userIdString, int amount);
        public int AddCapacity(BasePlayer player, int amount);
        private CapacityInfo UpdateCapacity(ulong userId, CapacityInfo capacityInfo);
        private void SetBonusCapacity(ulong userId, CapacityInfo capacityInfo);
        private CapacityInfo GetCapacityInfo(ulong userId, string userIdString);
        private int DetermineCapacityFromPermission(string userIdString);
        private CapacityInfo DetermineCapacityInfo(ulong userId, string userIdString);
        private int DetermineCurrentCapacity(ulong userId, int initialCapacity);
    }

    private class BackpackManager
    {
        private static string DetermineBackpackPath(ulong userId);
        private readonly Backpacks _plugin;
        private readonly Dictionary<ulong, Backpack> _cachedBackpacks;
        private readonly Dictionary<ulong, string> _backpackPathCache;
        private readonly Dictionary<ItemContainer, Backpack> _backpackContainers;
        private readonly List<DroppedItemContainer> _reusableDroppedItemContainerList;
        private readonly List<Backpack> _tempBackpackList;
        public BackpackManager(Backpacks plugin);
        public void HandleCapacityPermissionChangedForGroup(string groupName);
        public void HandleCapacityPermissionChangedForUser(string userIdString);
        public void HandleRestrictionPermissionChangedForGroup(string groupName);
        public void HandleRestrictionPermissionChangedForUser(string userIdString);
        public void HandleGatherPermissionChangedForGroup(string groupName);
        public void HandleGatherPermissionChangedForUser(string userIdString);
        public void HandleRetrievePermissionChangedForGroup(string groupName);
        public void HandleRetrievePermissionChangedForUser(string userIdString);
        public void HandleGroupChangeForUser(string userIdString);
        public bool IsBackpack(ItemContainer container);
        public bool IsBackpack(ItemContainer container, Backpack backpack, int pageIndex);
        public bool HasBackpack(ulong userId);
        public Backpack GetBackpackIfCached(ulong userId);
        public Backpack GetBackpack(ulong userId);
        public Backpack GetBackpackIfExists(ulong userId);
        public void RegisterContainer(ItemContainer container, Backpack backpack);
        public void UnregisterContainer(ItemContainer container);
        public Backpack GetCachedBackpackForContainer(ItemContainer container);
        public Dictionary<ulong, ItemContainer> GetAllCachedContainers();
        public DroppedItemContainer Drop(ulong userId, Vector3 position, List<DroppedItemContainer> collect);
        public bool TryOpenBackpack(BasePlayer looter, ulong backpackOwnerId);
        public bool TryOpenBackpackContainer(BasePlayer looter, ulong backpackOwnerId, ItemContainer container);
        public bool TryOpenBackpackPage(BasePlayer looter, ulong backpackOwnerId, int pageIndex);
        public void DeleteBackpackFile(ulong userId);
        public bool TryEraseForPlayer(ulong userId);
        public IEnumerator SaveAllAndKill(bool async, bool keepInUseBackpacks);
        public void ClearCache();
        private string GetBackpackPath(ulong userId);
        private bool HasBackpackFile(ulong userId);
        private Backpack Load(ulong userId);
        private Backpack GetBackpackIfCached(string userIdString);
    }

    private class BackpackNetworkController
    {
        private const uint StartNetworkGroupId;
        private static uint _nextNetworkGroupId;
        public static void ResetNetworkGroupId();
        public static bool IsBackpackNetworkGroup(Network.Visibility.Group group);
        public static BackpackNetworkController Create();
        public readonly Network.Visibility.Group NetworkGroup;
        private readonly List<BasePlayer> _subscribers;
        private BackpackNetworkController(uint networkGroupId);
        public void Subscribe(BasePlayer player);
        public void Unsubscribe(BasePlayer player);
        public void UnsubscribeAll();
    }

    private class NoRagdollCollision : FacepunchBehaviour
    {
        private Collider _collider;
        private void Awake();
        private void OnCollisionEnter(Collision collision);
    }

    private class BackpackCloseListener : EntityComponent<StorageContainer>
    {
        public static void AddToBackpackStorage(Backpacks plugin, StorageContainer containerEntity, Backpack backpack);
        private Backpacks _plugin;
        private Backpack _backpack;
        private void PlayerStoppedLooting(BasePlayer looter);
    }

    private class VirtualContainerAdapter : IContainerAdapter
    {
        public int PageIndex { get; set; }
        public int Capacity { get; set; }
        public List<ItemData> ItemDataList { get; set; }
        public int ItemCount { get; set; }
        public bool HasItems { get; set; }
        private Backpack _backpack;
        public VirtualContainerAdapter Setup(Backpack backpack, int pageIndex, int capacity);
        public void EnterPool();
        public void LeavePool();
        public void SortByPosition();
        public int PositionOf(ItemQuery itemQuery);
        public int CountItems(ItemQuery itemQuery);
        public int SumItems(ItemQuery itemQuery);
        public int TakeItems(ItemQuery itemQuery, int amount, List<Item> collect);
        public int MutateItems(ItemQuery itemQuery, MutationRequest mutationRequest);
        public void ReclaimFractionForSoftcore(float fraction, List<Item> collect);
        public void TakeRestrictedItems(List<Item> collect);
        public void TakeAllItems(List<Item> collect, int startPosition);
        public void SerializeForNetwork(List<ProtoBuf.Item> saveList);
        public void SerializeTo(List<ItemData> saveList, List<ItemData> itemsToReleaseToPool);
        public void EraseContents(WipeRuleset ruleset, WipeContext wipeContext);
        public void Kill();
        public void FreeToPool();
        public VirtualContainerAdapter CopyItemsFrom(List<ItemData> itemDataList);
        public bool TryDepositItem(Item item);
        private int GetFirstEmptyPosition();
        private void RemoveItem(int index);
    }

    private class ItemContainerAdapter : IContainerAdapter
    {
        public int PageIndex { get; set; }
        public int Capacity { get; set; }
        public StorageContainer ContainerEntity;
        public ItemContainer ItemContainer { get; set; }
        public int ItemCount { get; set; }
        public bool HasItems { get; set; }
        private Backpack _backpack;
        private Action _onDirty;
        private Func<Item, int, bool> _canAcceptItem;
        private Action<Item, bool> _onItemAddedRemoved;
        private Backpacks _plugin { get; set; }
        private Configuration _config { get; set; }
        public ItemContainerAdapter();
        public ItemContainerAdapter Setup(Backpack backpack, int pageIndex, StorageContainer storageContainer);
        public void EnterPool();
        public void LeavePool();
        public ItemContainerAdapter AddDelegates();
        public void SortByPosition();
        public void FindItems(ItemQuery itemQuery, List<Item> collect);
        public void FindAmmo(AmmoTypes ammoType, List<Item> collect);
        public int PositionOf(ItemQuery itemQuery);
        public int CountItems(ItemQuery itemQuery);
        public int SumItems(ItemQuery itemQuery);
        public int TakeItems(ItemQuery itemQuery, int amount, List<Item> collect);
        public int MutateItems(ItemQuery itemQuery, MutationRequest mutationRequest);
        public bool TryDepositItem(Item item);
        public bool TryInsertItem(Item item, ItemQuery itemQuery, int position);
        public void ReclaimFractionForSoftcore(float fraction, List<Item> collect);
        public void TakeRestrictedItems(List<Item> collect);
        public void TakeAllItems(List<Item> collect, int startPosition);
        public void SerializeForNetwork(List<ProtoBuf.Item> saveList);
        public void SerializeTo(List<ItemData> saveList, List<ItemData> itemsToReleaseToPool);
        public void EraseContents(WipeRuleset ruleset, WipeContext wipeContext);
        public void Kill();
        public void FreeToPool();
        public ItemContainerAdapter CopyItemsFrom(List<ItemData> itemDataList);
    }

    private class ContainerAdapterEnumerator : IEnumerator<IContainerAdapter>, CustomPool.IPooled
    {
        private ContainerAdapterCollection _adapterCollection;
        private int _position;
        public ContainerAdapterEnumerator Setup(ContainerAdapterCollection adapterCollection);
        public void EnterPool();
        public void LeavePool();
        public bool MoveNext();
        public void Reset();
        public IContainerAdapter Current { get; set; }
         object Current { get; set; }
        public void Dispose();
    }

    private class ContainerAdapterCollection : IEnumerable<IContainerAdapter>
    {
        public int Count { get; set; }
        private IContainerAdapter[] _containerAdapters;
        public ContainerAdapterCollection(int size);
        public void RemoveAt(int index);
        public IEnumerator<IContainerAdapter> GetEnumerator();
         IEnumerator GetEnumerator();
        public void Resize(int newSize);
        public void ResetPooledItemsAndClear();
    }

    private class InventoryWatcher : FacepunchBehaviour
    {
        public static InventoryWatcher AddToPlayer(BasePlayer player, Backpack backpack);
        private BasePlayer _player;
        private Backpack _backpack;
        private Action<Item, bool> _onItemAddedRemoved;
        private int _pauseGatherModeUntilFrame;
        public void DestroyImmediate();
        private InventoryWatcher();
        private bool ShouldIgnoreContainer();
        private void OnItemAddedRemoved(Item item, bool wasAdded);
        private bool HasMatchingItem(List<Item> itemList, Item item, ItemQuery itemQuery, int maxSlots);
        private void OnDestroy();
    }

    [JsonObject(MemberSerialization.OptIn)]
    [JsonConverter(typeof(PoolConverter<Backpack>))]
    private class Backpack : CustomPool.IPooled
    {
        private class PausableCallback : IDisposable
        {
            private Action _action;
            private bool _isPaused;
            private bool _wasCalled;
            public PausableCallback(Action action);
            public PausableCallback Pause();
            public void Call();
            public void Dispose();
        }

        private const float FeedbackThrottleSeconds;
        private static int CalculatePageIndexForItemPosition(int position);
        [JsonProperty("OwnerID", Order = 0)]
        public ulong OwnerId { get; set; }
        [JsonProperty("GatherMode", ItemConverterType = typeof(StringEnumConverter))]
        private Dictionary<int, GatherMode> GatherModeByPage;
        [JsonProperty("Retrieve", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private int RetrieveFromPagesMask;
        [JsonProperty("Items", Order = 2)]
        private List<ItemData> ItemDataCollection;
        public List<Item> _rejectedItems;
        public Backpacks Plugin;
        public BackpackNetworkController NetworkController { get; set; }
        public string OwnerIdString;
        public RealTimeSince TimeSinceLastFeedback;
        private BackpackCapacity ActualCapacity;
        private BackpackCapacity _allowedCapacity;
        private PausableCallback _itemCountChangedEvent;
        private Flag _flags;
        private RestrictionRuleset _restrictionRuleset;
        private bool _canGather;
        private bool _canRetrieve;
        private DynamicConfigFile _dataFile;
        private BasePlayer _owner;
        private ContainerAdapterCollection _containerAdapters;
        private readonly List<BasePlayer> _looters;
        private readonly List<BasePlayer> _uiViewers;
        private InventoryWatcher _inventoryWatcher;
        private float _pauseGatherModeUntilTime;
        private int _checkedAccessOnFrame;
        public bool HasLooters { get; set; }
        public bool IsGathering { get; set; }
        private Configuration _config { get; set; }
        private BackpackManager _backpackManager { get; set; }
        private SubscriberManager _subscriberManager { get; set; }
        public BasePlayer Owner { get; set; }
        public int Capacity { get; set; }
        public int PageCount { get; set; }
        private BackpackCapacity AllowedCapacity { get; set; }
        public RestrictionRuleset RestrictionRuleset { get; set; }
        public bool CanGather { get; set; }
        public bool CanRetrieve { get; set; }
        public int ItemCount { get; set; }
        public bool IsRetrieving { get; set; }
        private bool WantsGather { get; set; }
        public bool HasPreferences { get; set; }
        public bool CanAccess { get; set; }
        public bool HasItems { get; set; }
        public Backpack();
        public void Setup(Backpacks plugin, ulong ownerId, DynamicConfigFile dataFile);
        public void EnterPool();
        public void LeavePool();
        public void SetFlag(Flag flag, bool value);
        public bool HasFlag(Flag flag);
        public void MarkDirty();
        public void SetCapacity(int amount);
        public bool IsRetrievingFromPage(int pageIndex);
        public void ToggleRetrieve(BasePlayer player, int pageIndex);
        public GatherMode GetGatherModeForPage(int pageIndex);
        public void ToggleGatherMode(BasePlayer player, int pageIndex);
        public void HandleGatheringStopped();
        public void PauseGatherMode(float durationSeconds);
        public bool TryGatherItem(Item item);
        public void AddRejectedItem(Item item);
        public int GetPageIndexForContainer(ItemContainer container);
        public ItemContainerAdapter EnsureItemContainerAdapter(int pageIndex);
        public int GetAllowedPageCapacityForLooter(ulong looterId, int desiredPageIndex);
        public int DetermineInitialPageForLooter(ulong looterId, int desiredPageIndex, bool forward);
        public int DetermineNextPageIndexForLooter(ulong looterId, int currentPageIndex, int desiredPageIndex, bool forward, bool wrapAround, bool requireContents);
        public int CountItems(ItemQuery itemQuery);
        public void FindItems(ItemQuery itemQuery, List<Item> collect, bool forItemRetriever);
        public void FindAmmo(AmmoTypes ammoType, List<Item> collect, bool forItemRetriever);
        public int SumItems(ItemQuery itemQuery, bool forItemRetriever);
        public int TakeItems(ItemQuery itemQuery, int amount, List<Item> collect, bool forItemRetriever);
        public bool TryDepositItem(Item item);
        public int MutateItems(ItemQuery itemQuery, MutationRequest mutationRequest);
        public void SerializeForNetwork(List<ProtoBuf.Item> saveList, bool forItemRetriever);
        public IPlayer FindOwnerPlayer();
        public bool ShouldAcceptItem(Item item, ItemContainer container);
        public void HandleItemCountChanged();
        public ItemContainer GetContainer(bool ensureContainer);
        public bool TryOpen(BasePlayer looter, int pageIndex);
        public void SwitchToPage(BasePlayer looter, int pageIndex);
        public BasePlayer DetermineFeedbackRecipientIfEligible();
        public void OnClosed(BasePlayer looter);
        public bool Drop(Vector3 position, List<DroppedItemContainer> collect);
        public void EraseContents(WipeRuleset wipeRuleset, bool force);
        public bool SaveIfChanged();
        public int FetchItems(BasePlayer player, ItemQuery itemQuery, int desiredAmount);
        public void Kill();
        public string SerializeContentsAsJson();
        public void WriteContentsFromJson(string json);
        private void CreateContainerAdapters();
        private void SetupItemsAndContainers();
        private VirtualContainerAdapter CreateVirtualContainerAdapter(int pageIndex);
        private ItemContainerAdapter CreateItemContainerAdapter(int pageIndex);
        private ItemContainerAdapter UpgradeToItemContainer(VirtualContainerAdapter virtualContainerAdapter);
        private void SerializeRejectedItems(List<ItemData> itemsToReleaseToPool);
        private void EjectRejectedItemsIfNeeded(BasePlayer receiver);
        private void EjectRestrictedItemsIfNeeded(BasePlayer receiver);
        private void ShrinkIfNeededAndEjectOverflowingItems(BasePlayer overflowRecipient);
        private StorageContainer CreateStorageContainer(int capacity);
        private ItemContainerAdapter GetAdapterForContainer(ItemContainer container);
        private IContainerAdapter EnsurePage(int pageIndex, bool preferRealContainer);
        private BackpackCapacity GetAllowedCapacityForLooter(ulong looterId);
        private DroppedItemContainer SpawnDroppedBackpack(Vector3 position, int capacity, List<Item> itemList);
        private void EnlargeIfNeeded();
        private void KillContainerAdapter(IContainerAdapter containerAdapter);
        private void ForceCloseLooter(BasePlayer looter);
        private void ForceCloseAllLooters();
        private StorageContainer SpawnStorageContainer(int capacity);
        private void ReclaimItemsForSoftcore();
        private void SetRetrieveFromPage(int pageIndex, bool retrieve);
        public void SetGatherModeForPage(BasePlayer player, int pageIndex, GatherMode gatherMode);
        private void StartGathering(BasePlayer player);
        private void StopGathering();
        private void BroadcastItemCountChanged();
        private void MaybeCreateContainerUi(BasePlayer looter, int allowedPageCount, int pageIndex, int containerCapacity);
    }

    [JsonConverter(typeof(PoolConverter<EntityData>))]
    private class EntityData : CustomPool.IPooled
    {
        [JsonConverter(typeof(PoolConverter<BasicItemData>))]
        public class BasicItemData : CustomPool.IPooled
        {
            public int ItemId;
            public BasicItemData Setup(int itemId);
            public void EnterPool();
            public void LeavePool();
        }

        [JsonProperty("Flags", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public BaseEntity.Flags Flags { get; set; }
        [JsonProperty("DataInt", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int DataInt { get; set; }
        [JsonProperty("CreatorSteamId", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong CreatorSteamId { get; set; }
        [JsonProperty("FileContent", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string[] FileContent { get; set; }
        [JsonProperty("PrefabId", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public uint PrefabId { get; set; }
        [JsonProperty("PlayerName", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string PlayerName { get; set; }
        [JsonProperty("Items", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [JsonConverter(typeof(PoolListConverter<BasicItemData>))]
        public List<BasicItemData> Items { get; set; }
        public void Setup(BaseEntity entity);
        public void EnterPool();
        public void LeavePool();
        public void UpdateAssociatedEntity(Item item);
    }

    [JsonConverter(typeof(PoolConverter<OwnershipData>))]
    private class OwnershipData : CustomPool.IPooled
    {
        [JsonProperty("Username")]
        public string Username;
        [JsonProperty("Reason")]
        public string Reason;
        [JsonProperty("Amount")]
        public int Amount;
        public OwnershipData Setup(string username, string reason, int amount);
        public void EnterPool();
        public void LeavePool();
    }

    [JsonConverter(typeof(PoolConverter<ItemData>))]
    private class ItemData : CustomPool.IPooled
    {
        [JsonProperty("ID")]
        public int ID;
        [JsonProperty("Position")]
        public int Position { get; set; }
        [JsonProperty("Amount")]
        public int Amount { get; set; }
        [JsonProperty("IsBlueprint", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private bool IsBlueprint;
        [JsonProperty("BlueprintTarget", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int BlueprintTarget { get; set; }
        [JsonProperty("Skin", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public ulong Skin;
        [JsonProperty("Fuel", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private float Fuel;
        [JsonProperty("FlameFuel", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private int FlameFuel;
        [JsonProperty("Condition", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float Condition;
        [JsonProperty("MaxCondition", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float MaxCondition { get; set; }
        [JsonProperty("Ammo", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private int Ammo;
        [JsonProperty("AmmoType", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private int AmmoType;
        [JsonProperty("DataInt", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int DataInt { get; set; }
        [JsonProperty("Name", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Name;
        [JsonProperty("Text", DefaultValueHandling = DefaultValueHandling.Ignore)]
        private string Text;
        [JsonProperty("Flags", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public Item.Flag Flags { get; set; }
        [JsonProperty("EntityData", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public EntityData EntityData { get; set; }
        [JsonProperty("Capacity", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public int Capacity { get; set; }
        [JsonProperty("Ownership", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [JsonConverter(typeof(PoolListConverter<OwnershipData>))]
        public List<OwnershipData> Ownership { get; set; }
        [JsonProperty("Contents", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [JsonConverter(typeof(PoolListConverter<ItemData>))]
        public List<ItemData> Contents { get; set; }
        public ItemData Setup(Item item, int positionOffset);
        public void EnterPool();
        public void LeavePool();
        public void Reduce(int amount);
        public Item ToItem(int amount);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private abstract class BaseData
    {
        [JsonIgnore]
        protected abstract string _fileName { get; set; }
        [JsonIgnore]
        protected bool _dirty;
        public bool SaveIfChanged();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class PreferencesData : BaseData
    {
        private const string FileName;
        public static PreferencesData Load();
        protected override string _fileName { get; set; }
        [JsonProperty("PlayersWithDisabledGUI")]
        private HashSet<ulong> DeprecatedPlayersWithDisabledGUI { get; set; }
        [JsonProperty("PlayerGuiPreferences")]
        private Dictionary<ulong, bool> EnabledGuiPreference;
        public bool? GetGuiButtonPreference(ulong userId);
        public bool ToggleGuiButtonPreference(ulong userId, bool defaultEnabled);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class CapacityData : BaseData
    {
        private static string FileName;
        public static bool Exists();
        public static CapacityData Load();
        protected override string _fileName { get; set; }
        [JsonProperty("PlayerCapacity")]
        private Dictionary<ulong, int> _playerCapacity;
        [JsonProperty("BonusCapacity")]
        private Dictionary<ulong, int> _bonusCapacity;
        public int? GetPlayerExactCapacity(ulong userId);
        public int? GetPlayerBonusCapacity(ulong userId);
        public void SetPlayerExactCapacity(ulong userId, int capacity);
        public void SetPlayerBonusCapacity(ulong userId, int bonusCapacity);
        public void RemovePlayerExactCapacity(ulong userId);
        public void Clear();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private abstract class BaseItemRuleset
    {
        [JsonIgnore]
        protected abstract string PermissionPrefix { get; set; }
        [JsonProperty("Name", Order = -2, DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Name;
        [JsonProperty("Allowed item categories")]
        public string[] AllowedItemCategoryNames;
        [JsonProperty("Disallowed item categories")]
        public string[] DisallowedItemCategoryNames;
        [JsonProperty("Allowed item short names")]
        public string[] AllowedItemShortNames;
        [JsonProperty("Disallowed item short names")]
        public string[] DisallowedItemShortNames;
        [JsonProperty("Allowed skin IDs")]
        public HashSet<ulong> AllowedSkinIds;
        [JsonProperty("Disallowed skin IDs")]
        public HashSet<ulong> DisallowedSkinIds;
        [JsonIgnore]
        protected ItemCategory[] _allowedItemCategories;
        [JsonIgnore]
        protected ItemCategory[] _disallowedItemCategories;
        [JsonIgnore]
        protected HashSet<int> _allowedItemIds;
        [JsonIgnore]
        protected HashSet<int> _disallowedItemIds;
        [JsonIgnore]
        public string Permission { get; set; }
        [JsonIgnore]
        public bool AllowsAll { get; set; }
        public void Init(Backpacks plugin);
        public bool AllowsItem(Item item);
        public bool AllowsItem(ItemData itemData);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class RestrictionRuleset : BaseItemRuleset
    {
        private const string PartialPermissionPrefix;
        public static readonly string FullPermissionPrefix;
        public static readonly RestrictionRuleset AllowAll;
        protected override string PermissionPrefix { get; set; }
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class RestrictionOptions
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Enable legacy noblacklist permission")]
        public bool EnableLegacyPermission;
        [JsonProperty("Feedback effect")]
        public string FeedbackEffect;
        [JsonProperty("Default ruleset")]
        public RestrictionRuleset DefaultRuleset;
        [JsonProperty("Rulesets by permission")]
        public RestrictionRuleset[] RulesetsByPermission;
        [JsonIgnore]
        private Permission _permission;
        public void Init(Backpacks plugin);
        public RestrictionRuleset GetForPlayer(string userIdString);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class WipeRuleset : BaseItemRuleset
    {
        public static readonly WipeRuleset AllowAll;
        [JsonIgnore]
        protected override string PermissionPrefix { get; set; }
        [JsonProperty("Max slots to keep")]
        public int MaxSlotsToKeep;
        [JsonIgnore]
        public bool DisallowsAll { get; set; }
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class WipeOptions
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Enable legacy keeponwipe permission")]
        public bool EnableLegacyPermission;
        [JsonProperty("Default ruleset")]
        public WipeRuleset DefaultRuleset;
        [JsonProperty("Rulesets by permission")]
        public WipeRuleset[] RulesetsByPermission;
        [JsonIgnore]
        private Permission _permission;
        public void Init(Backpacks plugin);
        public WipeRuleset GetForPlayer(string userIdString);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class BackpackSizeOptions
    {
        [JsonProperty("Default size")]
        public int DefaultSize;
        [JsonProperty("Max size per page")]
        public int MaxCapacityPerPage;
        [JsonProperty("Enable legacy backpacks.use.1-8 row permissions")]
        public bool EnableLegacyRowPermissions;
        [JsonProperty("Permission sizes")]
        public int[] PermissionSizes;
        [JsonProperty("Dynamic Size (EXPERIMENTAL)")]
        public DynamicCapacity DynamicSize;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class ContainerUiOptions
    {
        [JsonProperty("Show page buttons on container bar")]
        public bool ShowPageButtonsOnContainerBar;
        [JsonProperty("Max page buttons to show")]
        public int MaxPageButtonsToShow;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class CapacityProfile
    {
        [JsonProperty("Permission suffix")]
        public string PermissionSuffix;
        [JsonProperty("Initial size")]
        public int InitialCapacity;
        [JsonProperty("Max size")]
        public int MaxCapacity;
        [JsonIgnore]
        public string Permission { get; set; }
        public void Init(Backpacks plugin);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class CapacityResetOptions
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class DynamicCapacity
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Reset dynamic size on wipe")]
        public CapacityResetOptions CapacityResetOptions;
        [JsonProperty("Size profiles")]
        public CapacityProfile[] CapacityProfiles;
        [JsonIgnore]
        private Permission _permission;
        public void Init(Backpacks plugin);
        public (int, int)? GetPlayerProfile(string userIdString);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : BaseConfiguration
    {
        [JsonProperty("Backpack size")]
        public BackpackSizeOptions BackpackSize;
        [JsonProperty("Backpack Size (1-8 Rows)")]
        private int DeprecatedBackpackRows { get; set; }
        [JsonProperty("Backpack Size (1-7 Rows)")]
        private int DeprecatedBackpackSize { get; set; }
        [JsonProperty("Default Backpack Size")]
        private int DeprecatedDefaultBackpackSize { get; set; }
        [JsonProperty("Max Size Per Page")]
        private int DeprecatedMaxSizePerPage { get; set; }
        [JsonProperty("Backpack Permission Sizes")]
        private int[] DeprecatedPermissionSizes { get; set; }
        [JsonProperty("Enable Legacy Row Permissions (true/false)")]
        private bool EnableLegacyRowPermissions { get; set; }
        [JsonProperty("Drop on Death (true/false)")]
        public bool DropOnDeath;
        [JsonProperty("Erase on Death (true/false)")]
        public bool EraseOnDeath;
        [JsonProperty("Clear Backpacks on Map-Wipe (true/false)")]
        public bool DeprecatedClearBackpacksOnWipe;
        public bool ShouldSerializeDeprecatedClearBackpacksOnWipe();
        [JsonProperty("Use Blacklist (true/false)")]
        public bool DeprecatedUseDenylist;
        public bool ShouldSerializeDeprecatedUseDenylist();
        [JsonProperty("Blacklisted Items (Item Shortnames)")]
        public string[] DeprecatedDenylistItemShortNames;
        public bool ShouldSerializeDeprecatedDenylistItemShortNames();
        [JsonProperty("Use Whitelist (true/false)")]
        public bool DeprecatedUseAllowlist;
        public bool ShouldSerializeDeprecatedUseAllowlist();
        [JsonProperty("Whitelisted Items (Item Shortnames)")]
        public string[] DeprecatedAllowedItemShortNames;
        public bool ShouldSerializeDeprecatedAllowedItemShortNames();
        [JsonProperty("Minimum Despawn Time (Seconds)")]
        public float MinimumDespawnTime;
        [JsonProperty("GUI Button")]
        public GUIButton GUI;
        [JsonProperty("Container UI")]
        public ContainerUiOptions ContainerUi;
        [JsonProperty("Softcore")]
        public SoftcoreOptions Softcore;
        [JsonProperty("Item restrictions")]
        public RestrictionOptions ItemRestrictions;
        [JsonProperty("Clear on wipe")]
        public WipeOptions ClearOnWipe;
        public class GUIButton
        {
            [JsonProperty("Enabled")]
            public bool Enabled;
            [JsonProperty("Enabled by default (for players with permission)")]
            public bool EnabledByDefault;
            [JsonProperty("Skin Id")]
            public ulong SkinId;
            [JsonProperty("Image")]
            public string Image;
            [JsonProperty("Background Color")]
            public string Color;
            [JsonProperty("GUI Button Position")]
            public Position GUIButtonPosition;
            public class Position
            {
                [JsonProperty("Anchors Min")]
                public string AnchorsMin;
                [JsonProperty("Anchors Max")]
                public string AnchorsMax;
                [JsonProperty("Offsets Min")]
                public string OffsetsMin;
                [JsonProperty("Offsets Max")]
                public string OffsetsMax;
            }

        }

        public class SoftcoreOptions
        {
            [JsonProperty("Reclaim Fraction")]
            public float ReclaimFraction;
        }

        public void Init(Backpacks plugin);
    }

    private Configuration GetDefaultConfig();
    [JsonObject(MemberSerialization.OptIn)]
    private class BaseConfiguration
    {
        public bool UsingDefaults;
        private string ToJson();
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
    private class LangEntry
    {
        public static readonly List<LangEntry> AllLangEntries;
        public static readonly LangEntry NoPermission;
        public static readonly LangEntry MayNotOpenBackpackInEvent;
        public static readonly LangEntry ViewBackpackSyntax;
        public static readonly LangEntry ViewBackpackProtected;
        public static readonly LangEntry UserIDNotFound;
        public static readonly LangEntry UserNameNotFound;
        public static readonly LangEntry MultiplePlayersFound;
        public static readonly LangEntry BackpackItemRejected;
        public static readonly LangEntry BackpackItemsRejected;
        public static readonly LangEntry BackpackOverCapacity;
        public static readonly LangEntry BlacklistedItemsRemoved;
        public static readonly LangEntry BackpackFetchSyntax;
        public static readonly LangEntry BackpackCapacitySyntax;
        public static readonly LangEntry DynamicCapacityNotEnabled;
        public static readonly LangEntry ChangeCapacitySuccess;
        public static readonly LangEntry InvalidItem;
        public static readonly LangEntry InvalidItemAmount;
        public static readonly LangEntry ItemNotInBackpack;
        public static readonly LangEntry ItemsFetched;
        public static readonly LangEntry FetchFailed;
        public static readonly LangEntry ToggledBackpackGUI;
        public static readonly LangEntry BackpackItemsReclaimed;
        public static readonly LangEntry SetGatherSyntax;
        public static readonly LangEntry PageOutOfRange;
        public static readonly LangEntry SetGatherModeSuccess;
        public static readonly LangEntry GatherModeAll;
        public static readonly LangEntry GatherModeExisting;
        public static readonly LangEntry GatherModeOff;
        public static readonly LangEntry UIGatherAll;
        public static readonly LangEntry UIGatherExisting;
        public static readonly LangEntry UIGatherOff;
        public static readonly LangEntry UIRetrieveOn;
        public static readonly LangEntry UIRetrieveOff;
        public string Name;
        public string English;
        public LangEntry(string name, string english);
    }

    private string GetMessage(string playerId, LangEntry langEntry);
    private string GetMessage(string playerId, LangEntry langEntry, object arg1);
    private string GetMessage(string playerId, LangEntry langEntry, object arg1, object arg2);
    private string GetMessage(string playerId, LangEntry langEntry, object arg1, object arg2, string arg3);
    private string GetMessage(string playerId, LangEntry langEntry, object[] args);
    private void ReplyToPlayer(IPlayer player, LangEntry langEntry);
    private void ReplyToPlayer(IPlayer player, LangEntry langEntry, object arg1);
    private void ReplyToPlayer(IPlayer player, LangEntry langEntry, object arg1, object arg2);
    private void ReplyToPlayer(IPlayer player, LangEntry langEntry, object arg1, object arg2, object arg3);
    private void ReplyToPlayer(IPlayer player, LangEntry langEntry, object[] args);
    private string GetGatherModeDisplayString(BasePlayer player, GatherMode gatherMode);
    protected override void LoadDefaultMessages();
}

private class ApiInstance
{
    public readonly Dictionary<string, object> ApiWrapper;
    private readonly Backpacks _plugin;
    private BackpackManager _backpackManager { get; set; }
    public ApiInstance(Backpacks plugin);
    public void AddSubscriber(Plugin plugin, Dictionary<string, object> spec);
    public void RemoveSubscriber(Plugin plugin);
    public Dictionary<ulong, ItemContainer> GetExistingBackpacks();
    public void EraseBackpack(ulong userId);
    public DroppedItemContainer DropBackpack(BasePlayer player, List<DroppedItemContainer> collect);
    public ulong GetBackpackOwnerId(ItemContainer container);
    public bool IsBackpackLoaded(BasePlayer player);
    public bool IsDynamicCapacityEnabled();
    public int GetBackpackCapacity(BasePlayer player);
    public int GetBackpackCapacityById(ulong playerID, string playerIDString);
    public int GetBackpackInitialCapacity(BasePlayer player);
    public int GetBackpackMaxCapacity(BasePlayer player);
    public int AddBackpackCapacity(BasePlayer player, int amount);
    public int SetBackpackCapacity(BasePlayer player, int capacity);
    public bool IsBackpackGathering(BasePlayer player);
    public bool IsBackpackRetrieving(BasePlayer player);
    public ItemContainer GetBackpackContainer(ulong ownerId);
    public int GetBackpackItemAmount(ulong ownerId, int itemId, ulong skinId);
    public bool TryOpenBackpack(BasePlayer player, ulong ownerId);
    public bool TryOpenBackpackContainer(BasePlayer player, ulong ownerId, ItemContainer container);
    public bool TryOpenBackpackPage(BasePlayer player, ulong ownerId, int page);
    public int SumBackpackItems(ulong ownerId, Dictionary<string, object> dict);
    public int CountBackpackItems(ulong ownerId, Dictionary<string, object> dict);
    public int TakeBackpackItems(ulong ownerId, Dictionary<string, object> dict, int amount, List<Item> collect);
    public int MutateBackpackItems(ulong ownerId, Dictionary<string, object> itemQueryDict, Dictionary<string, object> mutationRequestDict);
    public bool TryDepositBackpackItem(ulong ownerId, Item item);
    public void WriteBackpackContentsFromJson(ulong ownerId, string json);
    public string ReadBackpackContentsAsJson(ulong ownerId);
}

private static class ExposedHooks
{
    public static object CanOpenBackpack(BasePlayer looter, ulong ownerId);
    public static void OnBackpackClosed(BasePlayer looter, ulong ownerId, ItemContainer container);
    public static void OnBackpackOpened(BasePlayer looter, ulong ownerId, ItemContainer container);
    public static object CanDropBackpack(ulong ownerId, Vector3 position);
    public static void OnBackpackDropped(ulong ownerId, List<DroppedItemContainer> droppedBackpackList);
    public static object CanEraseBackpack(ulong ownerId);
    public static object CanBackpackAcceptItem(ulong ownerId, ItemContainer container, Item item);
}

private static class StringUtils
{
    public static bool EqualsCaseInsensitive(string a, string b, StringComparison stringComparison);
    public static bool Contains(string haystack, string needle);
}

private static class ObjectCache
{
    private static readonly object True;
    private static readonly object False;
    private static class StaticObjectCache
    {
        private static readonly Dictionary<T, object> _cacheByValue;
        public static object Get(T value);
    }

    public static object Get(T value);
    public static object Get(bool value);
}

private static class StaticObjectCache
{
    private static readonly Dictionary<T, object> _cacheByValue;
    public static object Get(T value);
}

private class PoolConverter : CustomCreationConverter<T>
{
    public override T Create(Type objectType);
}

private class PoolListConverter : CustomCreationConverter<List<T>>
{
    public override List<T> Create(Type objectType);
}

private static class ItemUtils
{
    public static int PositionOf(List<Item> itemList, ItemQuery itemQuery);
    public static int PositionOf(List<ItemData> itemDataList, ItemQuery itemQuery);
    public static void FindItems(List<Item> itemList, ItemQuery itemQuery, List<Item> collect);
    public static int CountItems(List<Item> itemList, ItemQuery itemQuery);
    public static int CountItems(List<ItemData> itemDataList, ItemQuery itemQuery);
    public static int SumItems(List<Item> itemList, ItemQuery itemQuery);
    public static int SumItems(List<ItemData> itemDataList, ItemQuery itemQuery);
    public static int TakeItems(List<Item> itemList, ItemQuery itemQuery, int amount, List<Item> collect);
    public static int TakeItems(List<ItemData> itemDataList, ItemQuery itemQuery, int amount, List<Item> collect);
    public static int MutateItems(List<Item> itemList, ItemQuery itemQuery, MutationRequest mutationRequest);
    public static int MutateItems(List<ItemData> itemDataList, ItemQuery itemQuery, MutationRequest mutationRequest);
    public static void SerializeForNetwork(List<Item> itemList, List<ProtoBuf.Item> collect);
    public static void SerializeForNetwork(List<ItemData> itemDataList, List<ProtoBuf.Item> collect);
    private static bool IsSearchableItemDefinition(ItemDefinition itemDefinition);
    private static bool IsSearchableItemDefinition(int itemId);
    private static bool HasSearchableContainer(Item item, List<Item> itemList);
    private static bool HasSearchableContainer(ItemData itemData, List<ItemData> itemDataList);
    private static void TakeItemAmount(Item item, int amount, List<Item> collect);
}

private static class CustomPool
{
    private static class StaticPool
    {
        public static readonly PoolCollection<T> Collection;
    }

    private class PoolCollection
    {
        public const int DefaultPoolSize;
        private T[] _buffer;
        public int ItemsCreated { get; set; }
        public int ItemsInStack { get; set; }
        public int ItemsInUse { get; set; }
        public int ItemsSpilled { get; set; }
        public int ItemsTaken { get; set; }
        public PoolCollection();
        public void Reset(int size);
        public void Add(T obj);
        public T Take();
    }

    public static void Reset(int size);
    public static string GetStats();
    public static T Get();
    public static List<T> GetList();
    public static void Free(T obj);
    public static void FreeList(List<T> list);
    private static void FreeInternal(T obj);
}

private static class StaticPool
{
    public static readonly PoolCollection<T> Collection;
}

private class PoolCollection
{
    public const int DefaultPoolSize;
    private T[] _buffer;
    public int ItemsCreated { get; set; }
    public int ItemsInStack { get; set; }
    public int ItemsInUse { get; set; }
    public int ItemsSpilled { get; set; }
    public int ItemsTaken { get; set; }
    public PoolCollection();
    public void Reset(int size);
    public void Add(T obj);
    public T Take();
}

private static class PoolUtils
{
    public const int BackpackPoolSize;
    public static void ResetItemsAndClear(IList<T> list);
    public static void ResizePools(bool empty);
}

private class DisposableList : List<T>, IDisposable
{
    public static DisposableList<T> Get();
    public void Dispose();
}

private sealed class DefaultStringCache : IStringCache
{
    public static readonly DefaultStringCache Instance;
    private static class StaticStringCache
    {
        private static readonly Dictionary<T, string> _cacheByValue;
        public static string Get(T value);
    }

    private static class StaticStringCacheWithFactory
    {
        private static readonly Dictionary<Func<T, string>, Dictionary<T, string>> _cacheByDelegate;
        public static string Get(T value, Func<T, string> createString);
    }

    private DefaultStringCache();
    public string Get(T value);
    public string Get(bool value);
    public string Get(T value, Func<T, string> createString);
}

private static class StaticStringCache
{
    private static readonly Dictionary<T, string> _cacheByValue;
    public static string Get(T value);
}

private static class StaticStringCacheWithFactory
{
    private static readonly Dictionary<Func<T, string>, Dictionary<T, string>> _cacheByDelegate;
    public static string Get(T value, Func<T, string> createString);
}

private class UiBuilder : IUiBuilder
{
    private static NetWrite ClientRPCStart(BaseEntity entity, string funcName);
    public static readonly UiBuilder Default;
    public int Length { get; set; }
    private const char Delimiter;
    private const char Quote;
    private const char Colon;
    private const char Space;
    private const char OpenBracket;
    private const char CloseBracket;
    private const char OpenCurlyBrace;
    private const char CloseCurlyBrace;
    private const int MinCapacity;
    private const int DefaultCapacity;
    public IStringCache StringCache { get; set; }
    private char[] _chars;
    private byte[] _bytes;
    private State _state;
    private bool _needsDelimiter;
    public UiBuilder(int capacity, IStringCache stringCache);
    public UiBuilder(int capacity);
    public void Start();
    public void End();
    public void StartElement();
    public void EndElement();
    public void StartComponent();
    public void EndComponent();
    public void AddField(string key, T value);
    public void AddField(string key, string value);
    public void AddXY(string key, float x, float y);
    public void AddSerializable(T serializable);
    public void AddComponents(T components);
    public string ToJson();
    public byte[] GetBytes();
    public void AddUi(SendInfo sendInfo);
    public void AddUi(BasePlayer player);
    private void ValidateState(State desiredState);
    private void ValidateState(State desiredState, State alternateState);
    private void Resize(int length);
    private void ResizeIfApproachingLength();
    private void Append(char @char);
    private void Append(string str);
    private void AddDelimiter();
    private void AddDelimiterIfNeeded();
    private void StartObject();
    private void EndObject();
    private void StartArray();
    private void EndArray();
    private void AddKey(string key);
    private void Reset();
}

private static class Layout
{
    public const string AnchorBottomLeft;
    public const string AnchorBottomRight;
    public const string AnchorTopLeft;
    public const string AnchorTopRight;
    public const string AnchorBottomCenter;
    public const string AnchorTopCenter;
    public const string AnchorCenterLeft;
    public const string AnchorCenterRight;
    public static string DetermineAnchor(Option options);
}

private static class ContainerUi
{
    public const float BaseOffsetY;
    public const float BaseOffsetX;
    public const float HeaderWidth;
    public const float HeaderHeight;
    public const float PerRowOffsetY;
    private const float PageButtonSpacing;
    private const float PageButtonSize;
    private const string BlueButtonColor;
    private const string BlueButtonTextColor;
    private const string GreenButtonColor;
    private const string GreenButtonTextColor;
    private const string Name;
    private static readonly string LeftButtonName;
    private static readonly string RightButtonName;
    public static void CreateContainerUi(BasePlayer player, int numPages, int activePageIndex, int capacity, Backpack backpack);
    public static void DestroyUi(BasePlayer player);
    private static void AddGatherModeButton(UiBuilder builder, StatefulLayoutProvider layoutProvider, BasePlayer player, Backpack backpack, int activePageIndex);
    private static void AddRetrieveButton(UiBuilder builder, StatefulLayoutProvider layoutProvider, BasePlayer player, Backpack backpack, int activePageIndex);
    private static void AddPageButton(UiBuilder builder, PageButton pageButton);
    private static void AddPaginationUi(UiBuilder builder, Backpack backpack, int numPages, int activePageIndex);
}

private static class ButtonUi
{
    private const string Name;
    public static string CreateButtonUi(Configuration config);
    public static void DestroyUi(BasePlayer player);
}

private class EventSubscriber
{
    public static EventSubscriber FromSpec(Plugin plugin, Dictionary<string, object> spec);
    private static void GetOption(Dictionary<string, object> dict, string key, T result);
    public Plugin Plugin { get; set; }
    public Action<BasePlayer, int, int> OnBackpackLoaded;
    public Action<BasePlayer, int, int> OnBackpackItemCountChanged;
    public Action<BasePlayer, bool> OnBackpackGatherChanged;
    public Action<BasePlayer, bool> OnBackpackRetrieveChanged;
}

private class SubscriberManager
{
    private readonly Dictionary<string, EventSubscriber> _subscribers;
    public void AddSubscriber(Plugin plugin, Dictionary<string, object> spec);
    public void RemoveSubscriber(Plugin plugin);
    public void BroadcastBackpackLoaded(Backpack backpack);
    public void BroadcastItemCountChanged(Backpack backpack);
    public void BroadcastGatherChanged(Backpack backpack, bool isGathering);
    public void BroadcastRetrieveChanged(Backpack backpack, bool isRetrieving);
}

private class CapacityManager
{
    private class BackpackSize
    {
        public readonly int Capacity;
        public readonly string Permission;
        public BackpackSize(int capacity, string permission);
    }

    private readonly Backpacks _plugin;
    private Configuration _config;
    private BackpackManager _backpackManager;
    private CapacityData _capacityData;
    private BackpackSize[] _sortedBackpackSizes;
    private readonly Dictionary<ulong, CapacityInfo> _cachedPlayerCapacityInfo;
    public CapacityManager(Backpacks plugin, BackpackManager backpackManager);
    public void Init(Configuration config, CapacityData capacityData);
    public void ForgetCachedCapacity(ulong userId);
    public int GetCapacity(ulong userId, string userIdString);
    public int GetInitialCapacity(ulong userId, string userIdString);
    public int GetMaxCapacity(ulong userId, string userIdString);
    public int SetCapacity(ulong userId, string userIdString, int amount);
    public int SetCapacity(BasePlayer player, int amount);
    public int AddCapacity(ulong userId, string userIdString, int amount);
    public int AddCapacity(BasePlayer player, int amount);
    private CapacityInfo UpdateCapacity(ulong userId, CapacityInfo capacityInfo);
    private void SetBonusCapacity(ulong userId, CapacityInfo capacityInfo);
    private CapacityInfo GetCapacityInfo(ulong userId, string userIdString);
    private int DetermineCapacityFromPermission(string userIdString);
    private CapacityInfo DetermineCapacityInfo(ulong userId, string userIdString);
    private int DetermineCurrentCapacity(ulong userId, int initialCapacity);
}

private class BackpackSize
{
    public readonly int Capacity;
    public readonly string Permission;
    public BackpackSize(int capacity, string permission);
}

private class BackpackManager
{
    private static string DetermineBackpackPath(ulong userId);
    private readonly Backpacks _plugin;
    private readonly Dictionary<ulong, Backpack> _cachedBackpacks;
    private readonly Dictionary<ulong, string> _backpackPathCache;
    private readonly Dictionary<ItemContainer, Backpack> _backpackContainers;
    private readonly List<DroppedItemContainer> _reusableDroppedItemContainerList;
    private readonly List<Backpack> _tempBackpackList;
    public BackpackManager(Backpacks plugin);
    public void HandleCapacityPermissionChangedForGroup(string groupName);
    public void HandleCapacityPermissionChangedForUser(string userIdString);
    public void HandleRestrictionPermissionChangedForGroup(string groupName);
    public void HandleRestrictionPermissionChangedForUser(string userIdString);
    public void HandleGatherPermissionChangedForGroup(string groupName);
    public void HandleGatherPermissionChangedForUser(string userIdString);
    public void HandleRetrievePermissionChangedForGroup(string groupName);
    public void HandleRetrievePermissionChangedForUser(string userIdString);
    public void HandleGroupChangeForUser(string userIdString);
    public bool IsBackpack(ItemContainer container);
    public bool IsBackpack(ItemContainer container, Backpack backpack, int pageIndex);
    public bool HasBackpack(ulong userId);
    public Backpack GetBackpackIfCached(ulong userId);
    public Backpack GetBackpack(ulong userId);
    public Backpack GetBackpackIfExists(ulong userId);
    public void RegisterContainer(ItemContainer container, Backpack backpack);
    public void UnregisterContainer(ItemContainer container);
    public Backpack GetCachedBackpackForContainer(ItemContainer container);
    public Dictionary<ulong, ItemContainer> GetAllCachedContainers();
    public DroppedItemContainer Drop(ulong userId, Vector3 position, List<DroppedItemContainer> collect);
    public bool TryOpenBackpack(BasePlayer looter, ulong backpackOwnerId);
    public bool TryOpenBackpackContainer(BasePlayer looter, ulong backpackOwnerId, ItemContainer container);
    public bool TryOpenBackpackPage(BasePlayer looter, ulong backpackOwnerId, int pageIndex);
    public void DeleteBackpackFile(ulong userId);
    public bool TryEraseForPlayer(ulong userId);
    public IEnumerator SaveAllAndKill(bool async, bool keepInUseBackpacks);
    public void ClearCache();
    private string GetBackpackPath(ulong userId);
    private bool HasBackpackFile(ulong userId);
    private Backpack Load(ulong userId);
    private Backpack GetBackpackIfCached(string userIdString);
}

private class BackpackNetworkController
{
    private const uint StartNetworkGroupId;
    private static uint _nextNetworkGroupId;
    public static void ResetNetworkGroupId();
    public static bool IsBackpackNetworkGroup(Network.Visibility.Group group);
    public static BackpackNetworkController Create();
    public readonly Network.Visibility.Group NetworkGroup;
    private readonly List<BasePlayer> _subscribers;
    private BackpackNetworkController(uint networkGroupId);
    public void Subscribe(BasePlayer player);
    public void Unsubscribe(BasePlayer player);
    public void UnsubscribeAll();
}

private class NoRagdollCollision : FacepunchBehaviour
{
    private Collider _collider;
    private void Awake();
    private void OnCollisionEnter(Collision collision);
}

private class BackpackCloseListener : EntityComponent<StorageContainer>
{
    public static void AddToBackpackStorage(Backpacks plugin, StorageContainer containerEntity, Backpack backpack);
    private Backpacks _plugin;
    private Backpack _backpack;
    private void PlayerStoppedLooting(BasePlayer looter);
}

private class VirtualContainerAdapter : IContainerAdapter
{
    public int PageIndex { get; set; }
    public int Capacity { get; set; }
    public List<ItemData> ItemDataList { get; set; }
    public int ItemCount { get; set; }
    public bool HasItems { get; set; }
    private Backpack _backpack;
    public VirtualContainerAdapter Setup(Backpack backpack, int pageIndex, int capacity);
    public void EnterPool();
    public void LeavePool();
    public void SortByPosition();
    public int PositionOf(ItemQuery itemQuery);
    public int CountItems(ItemQuery itemQuery);
    public int SumItems(ItemQuery itemQuery);
    public int TakeItems(ItemQuery itemQuery, int amount, List<Item> collect);
    public int MutateItems(ItemQuery itemQuery, MutationRequest mutationRequest);
    public void ReclaimFractionForSoftcore(float fraction, List<Item> collect);
    public void TakeRestrictedItems(List<Item> collect);
    public void TakeAllItems(List<Item> collect, int startPosition);
    public void SerializeForNetwork(List<ProtoBuf.Item> saveList);
    public void SerializeTo(List<ItemData> saveList, List<ItemData> itemsToReleaseToPool);
    public void EraseContents(WipeRuleset ruleset, WipeContext wipeContext);
    public void Kill();
    public void FreeToPool();
    public VirtualContainerAdapter CopyItemsFrom(List<ItemData> itemDataList);
    public bool TryDepositItem(Item item);
    private int GetFirstEmptyPosition();
    private void RemoveItem(int index);
}

private class ItemContainerAdapter : IContainerAdapter
{
    public int PageIndex { get; set; }
    public int Capacity { get; set; }
    public StorageContainer ContainerEntity;
    public ItemContainer ItemContainer { get; set; }
    public int ItemCount { get; set; }
    public bool HasItems { get; set; }
    private Backpack _backpack;
    private Action _onDirty;
    private Func<Item, int, bool> _canAcceptItem;
    private Action<Item, bool> _onItemAddedRemoved;
    private Backpacks _plugin { get; set; }
    private Configuration _config { get; set; }
    public ItemContainerAdapter();
    public ItemContainerAdapter Setup(Backpack backpack, int pageIndex, StorageContainer storageContainer);
    public void EnterPool();
    public void LeavePool();
    public ItemContainerAdapter AddDelegates();
    public void SortByPosition();
    public void FindItems(ItemQuery itemQuery, List<Item> collect);
    public void FindAmmo(AmmoTypes ammoType, List<Item> collect);
    public int PositionOf(ItemQuery itemQuery);
    public int CountItems(ItemQuery itemQuery);
    public int SumItems(ItemQuery itemQuery);
    public int TakeItems(ItemQuery itemQuery, int amount, List<Item> collect);
    public int MutateItems(ItemQuery itemQuery, MutationRequest mutationRequest);
    public bool TryDepositItem(Item item);
    public bool TryInsertItem(Item item, ItemQuery itemQuery, int position);
    public void ReclaimFractionForSoftcore(float fraction, List<Item> collect);
    public void TakeRestrictedItems(List<Item> collect);
    public void TakeAllItems(List<Item> collect, int startPosition);
    public void SerializeForNetwork(List<ProtoBuf.Item> saveList);
    public void SerializeTo(List<ItemData> saveList, List<ItemData> itemsToReleaseToPool);
    public void EraseContents(WipeRuleset ruleset, WipeContext wipeContext);
    public void Kill();
    public void FreeToPool();
    public ItemContainerAdapter CopyItemsFrom(List<ItemData> itemDataList);
}

private class ContainerAdapterEnumerator : IEnumerator<IContainerAdapter>, CustomPool.IPooled
{
    private ContainerAdapterCollection _adapterCollection;
    private int _position;
    public ContainerAdapterEnumerator Setup(ContainerAdapterCollection adapterCollection);
    public void EnterPool();
    public void LeavePool();
    public bool MoveNext();
    public void Reset();
    public IContainerAdapter Current { get; set; }
     object Current { get; set; }
    public void Dispose();
}

private class ContainerAdapterCollection : IEnumerable<IContainerAdapter>
{
    public int Count { get; set; }
    private IContainerAdapter[] _containerAdapters;
    public ContainerAdapterCollection(int size);
    public void RemoveAt(int index);
    public IEnumerator<IContainerAdapter> GetEnumerator();
     IEnumerator GetEnumerator();
    public void Resize(int newSize);
    public void ResetPooledItemsAndClear();
}

private class InventoryWatcher : FacepunchBehaviour
{
    public static InventoryWatcher AddToPlayer(BasePlayer player, Backpack backpack);
    private BasePlayer _player;
    private Backpack _backpack;
    private Action<Item, bool> _onItemAddedRemoved;
    private int _pauseGatherModeUntilFrame;
    public void DestroyImmediate();
    private InventoryWatcher();
    private bool ShouldIgnoreContainer();
    private void OnItemAddedRemoved(Item item, bool wasAdded);
    private bool HasMatchingItem(List<Item> itemList, Item item, ItemQuery itemQuery, int maxSlots);
    private void OnDestroy();
}

[JsonObject(MemberSerialization.OptIn)]
[JsonConverter(typeof(PoolConverter<Backpack>))]
private class Backpack : CustomPool.IPooled
{
    private class PausableCallback : IDisposable
    {
        private Action _action;
        private bool _isPaused;
        private bool _wasCalled;
        public PausableCallback(Action action);
        public PausableCallback Pause();
        public void Call();
        public void Dispose();
    }

    private const float FeedbackThrottleSeconds;
    private static int CalculatePageIndexForItemPosition(int position);
    [JsonProperty("OwnerID", Order = 0)]
    public ulong OwnerId { get; set; }
    [JsonProperty("GatherMode", ItemConverterType = typeof(StringEnumConverter))]
    private Dictionary<int, GatherMode> GatherModeByPage;
    [JsonProperty("Retrieve", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private int RetrieveFromPagesMask;
    [JsonProperty("Items", Order = 2)]
    private List<ItemData> ItemDataCollection;
    public List<Item> _rejectedItems;
    public Backpacks Plugin;
    public BackpackNetworkController NetworkController { get; set; }
    public string OwnerIdString;
    public RealTimeSince TimeSinceLastFeedback;
    private BackpackCapacity ActualCapacity;
    private BackpackCapacity _allowedCapacity;
    private PausableCallback _itemCountChangedEvent;
    private Flag _flags;
    private RestrictionRuleset _restrictionRuleset;
    private bool _canGather;
    private bool _canRetrieve;
    private DynamicConfigFile _dataFile;
    private BasePlayer _owner;
    private ContainerAdapterCollection _containerAdapters;
    private readonly List<BasePlayer> _looters;
    private readonly List<BasePlayer> _uiViewers;
    private InventoryWatcher _inventoryWatcher;
    private float _pauseGatherModeUntilTime;
    private int _checkedAccessOnFrame;
    public bool HasLooters { get; set; }
    public bool IsGathering { get; set; }
    private Configuration _config { get; set; }
    private BackpackManager _backpackManager { get; set; }
    private SubscriberManager _subscriberManager { get; set; }
    public BasePlayer Owner { get; set; }
    public int Capacity { get; set; }
    public int PageCount { get; set; }
    private BackpackCapacity AllowedCapacity { get; set; }
    public RestrictionRuleset RestrictionRuleset { get; set; }
    public bool CanGather { get; set; }
    public bool CanRetrieve { get; set; }
    public int ItemCount { get; set; }
    public bool IsRetrieving { get; set; }
    private bool WantsGather { get; set; }
    public bool HasPreferences { get; set; }
    public bool CanAccess { get; set; }
    public bool HasItems { get; set; }
    public Backpack();
    public void Setup(Backpacks plugin, ulong ownerId, DynamicConfigFile dataFile);
    public void EnterPool();
    public void LeavePool();
    public void SetFlag(Flag flag, bool value);
    public bool HasFlag(Flag flag);
    public void MarkDirty();
    public void SetCapacity(int amount);
    public bool IsRetrievingFromPage(int pageIndex);
    public void ToggleRetrieve(BasePlayer player, int pageIndex);
    public GatherMode GetGatherModeForPage(int pageIndex);
    public void ToggleGatherMode(BasePlayer player, int pageIndex);
    public void HandleGatheringStopped();
    public void PauseGatherMode(float durationSeconds);
    public bool TryGatherItem(Item item);
    public void AddRejectedItem(Item item);
    public int GetPageIndexForContainer(ItemContainer container);
    public ItemContainerAdapter EnsureItemContainerAdapter(int pageIndex);
    public int GetAllowedPageCapacityForLooter(ulong looterId, int desiredPageIndex);
    public int DetermineInitialPageForLooter(ulong looterId, int desiredPageIndex, bool forward);
    public int DetermineNextPageIndexForLooter(ulong looterId, int currentPageIndex, int desiredPageIndex, bool forward, bool wrapAround, bool requireContents);
    public int CountItems(ItemQuery itemQuery);
    public void FindItems(ItemQuery itemQuery, List<Item> collect, bool forItemRetriever);
    public void FindAmmo(AmmoTypes ammoType, List<Item> collect, bool forItemRetriever);
    public int SumItems(ItemQuery itemQuery, bool forItemRetriever);
    public int TakeItems(ItemQuery itemQuery, int amount, List<Item> collect, bool forItemRetriever);
    public bool TryDepositItem(Item item);
    public int MutateItems(ItemQuery itemQuery, MutationRequest mutationRequest);
    public void SerializeForNetwork(List<ProtoBuf.Item> saveList, bool forItemRetriever);
    public IPlayer FindOwnerPlayer();
    public bool ShouldAcceptItem(Item item, ItemContainer container);
    public void HandleItemCountChanged();
    public ItemContainer GetContainer(bool ensureContainer);
    public bool TryOpen(BasePlayer looter, int pageIndex);
    public void SwitchToPage(BasePlayer looter, int pageIndex);
    public BasePlayer DetermineFeedbackRecipientIfEligible();
    public void OnClosed(BasePlayer looter);
    public bool Drop(Vector3 position, List<DroppedItemContainer> collect);
    public void EraseContents(WipeRuleset wipeRuleset, bool force);
    public bool SaveIfChanged();
    public int FetchItems(BasePlayer player, ItemQuery itemQuery, int desiredAmount);
    public void Kill();
    public string SerializeContentsAsJson();
    public void WriteContentsFromJson(string json);
    private void CreateContainerAdapters();
    private void SetupItemsAndContainers();
    private VirtualContainerAdapter CreateVirtualContainerAdapter(int pageIndex);
    private ItemContainerAdapter CreateItemContainerAdapter(int pageIndex);
    private ItemContainerAdapter UpgradeToItemContainer(VirtualContainerAdapter virtualContainerAdapter);
    private void SerializeRejectedItems(List<ItemData> itemsToReleaseToPool);
    private void EjectRejectedItemsIfNeeded(BasePlayer receiver);
    private void EjectRestrictedItemsIfNeeded(BasePlayer receiver);
    private void ShrinkIfNeededAndEjectOverflowingItems(BasePlayer overflowRecipient);
    private StorageContainer CreateStorageContainer(int capacity);
    private ItemContainerAdapter GetAdapterForContainer(ItemContainer container);
    private IContainerAdapter EnsurePage(int pageIndex, bool preferRealContainer);
    private BackpackCapacity GetAllowedCapacityForLooter(ulong looterId);
    private DroppedItemContainer SpawnDroppedBackpack(Vector3 position, int capacity, List<Item> itemList);
    private void EnlargeIfNeeded();
    private void KillContainerAdapter(IContainerAdapter containerAdapter);
    private void ForceCloseLooter(BasePlayer looter);
    private void ForceCloseAllLooters();
    private StorageContainer SpawnStorageContainer(int capacity);
    private void ReclaimItemsForSoftcore();
    private void SetRetrieveFromPage(int pageIndex, bool retrieve);
    public void SetGatherModeForPage(BasePlayer player, int pageIndex, GatherMode gatherMode);
    private void StartGathering(BasePlayer player);
    private void StopGathering();
    private void BroadcastItemCountChanged();
    private void MaybeCreateContainerUi(BasePlayer looter, int allowedPageCount, int pageIndex, int containerCapacity);
}

private class PausableCallback : IDisposable
{
    private Action _action;
    private bool _isPaused;
    private bool _wasCalled;
    public PausableCallback(Action action);
    public PausableCallback Pause();
    public void Call();
    public void Dispose();
}

[JsonConverter(typeof(PoolConverter<EntityData>))]
private class EntityData : CustomPool.IPooled
{
    [JsonConverter(typeof(PoolConverter<BasicItemData>))]
    public class BasicItemData : CustomPool.IPooled
    {
        public int ItemId;
        public BasicItemData Setup(int itemId);
        public void EnterPool();
        public void LeavePool();
    }

    [JsonProperty("Flags", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public BaseEntity.Flags Flags { get; set; }
    [JsonProperty("DataInt", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int DataInt { get; set; }
    [JsonProperty("CreatorSteamId", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong CreatorSteamId { get; set; }
    [JsonProperty("FileContent", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string[] FileContent { get; set; }
    [JsonProperty("PrefabId", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public uint PrefabId { get; set; }
    [JsonProperty("PlayerName", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string PlayerName { get; set; }
    [JsonProperty("Items", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [JsonConverter(typeof(PoolListConverter<BasicItemData>))]
    public List<BasicItemData> Items { get; set; }
    public void Setup(BaseEntity entity);
    public void EnterPool();
    public void LeavePool();
    public void UpdateAssociatedEntity(Item item);
}

[JsonConverter(typeof(PoolConverter<BasicItemData>))]
public class BasicItemData : CustomPool.IPooled
{
    public int ItemId;
    public BasicItemData Setup(int itemId);
    public void EnterPool();
    public void LeavePool();
}

[JsonConverter(typeof(PoolConverter<OwnershipData>))]
private class OwnershipData : CustomPool.IPooled
{
    [JsonProperty("Username")]
    public string Username;
    [JsonProperty("Reason")]
    public string Reason;
    [JsonProperty("Amount")]
    public int Amount;
    public OwnershipData Setup(string username, string reason, int amount);
    public void EnterPool();
    public void LeavePool();
}

[JsonConverter(typeof(PoolConverter<ItemData>))]
private class ItemData : CustomPool.IPooled
{
    [JsonProperty("ID")]
    public int ID;
    [JsonProperty("Position")]
    public int Position { get; set; }
    [JsonProperty("Amount")]
    public int Amount { get; set; }
    [JsonProperty("IsBlueprint", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private bool IsBlueprint;
    [JsonProperty("BlueprintTarget", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int BlueprintTarget { get; set; }
    [JsonProperty("Skin", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public ulong Skin;
    [JsonProperty("Fuel", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private float Fuel;
    [JsonProperty("FlameFuel", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private int FlameFuel;
    [JsonProperty("Condition", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float Condition;
    [JsonProperty("MaxCondition", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float MaxCondition { get; set; }
    [JsonProperty("Ammo", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private int Ammo;
    [JsonProperty("AmmoType", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private int AmmoType;
    [JsonProperty("DataInt", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int DataInt { get; set; }
    [JsonProperty("Name", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Name;
    [JsonProperty("Text", DefaultValueHandling = DefaultValueHandling.Ignore)]
    private string Text;
    [JsonProperty("Flags", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public Item.Flag Flags { get; set; }
    [JsonProperty("EntityData", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public EntityData EntityData { get; set; }
    [JsonProperty("Capacity", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public int Capacity { get; set; }
    [JsonProperty("Ownership", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [JsonConverter(typeof(PoolListConverter<OwnershipData>))]
    public List<OwnershipData> Ownership { get; set; }
    [JsonProperty("Contents", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [JsonConverter(typeof(PoolListConverter<ItemData>))]
    public List<ItemData> Contents { get; set; }
    public ItemData Setup(Item item, int positionOffset);
    public void EnterPool();
    public void LeavePool();
    public void Reduce(int amount);
    public Item ToItem(int amount);
}

[JsonObject(MemberSerialization.OptIn)]
private abstract class BaseData
{
    [JsonIgnore]
    protected abstract string _fileName { get; set; }
    [JsonIgnore]
    protected bool _dirty;
    public bool SaveIfChanged();
}

[JsonObject(MemberSerialization.OptIn)]
private class PreferencesData : BaseData
{
    private const string FileName;
    public static PreferencesData Load();
    protected override string _fileName { get; set; }
    [JsonProperty("PlayersWithDisabledGUI")]
    private HashSet<ulong> DeprecatedPlayersWithDisabledGUI { get; set; }
    [JsonProperty("PlayerGuiPreferences")]
    private Dictionary<ulong, bool> EnabledGuiPreference;
    public bool? GetGuiButtonPreference(ulong userId);
    public bool ToggleGuiButtonPreference(ulong userId, bool defaultEnabled);
}

[JsonObject(MemberSerialization.OptIn)]
private class CapacityData : BaseData
{
    private static string FileName;
    public static bool Exists();
    public static CapacityData Load();
    protected override string _fileName { get; set; }
    [JsonProperty("PlayerCapacity")]
    private Dictionary<ulong, int> _playerCapacity;
    [JsonProperty("BonusCapacity")]
    private Dictionary<ulong, int> _bonusCapacity;
    public int? GetPlayerExactCapacity(ulong userId);
    public int? GetPlayerBonusCapacity(ulong userId);
    public void SetPlayerExactCapacity(ulong userId, int capacity);
    public void SetPlayerBonusCapacity(ulong userId, int bonusCapacity);
    public void RemovePlayerExactCapacity(ulong userId);
    public void Clear();
}

[JsonObject(MemberSerialization.OptIn)]
private abstract class BaseItemRuleset
{
    [JsonIgnore]
    protected abstract string PermissionPrefix { get; set; }
    [JsonProperty("Name", Order = -2, DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Name;
    [JsonProperty("Allowed item categories")]
    public string[] AllowedItemCategoryNames;
    [JsonProperty("Disallowed item categories")]
    public string[] DisallowedItemCategoryNames;
    [JsonProperty("Allowed item short names")]
    public string[] AllowedItemShortNames;
    [JsonProperty("Disallowed item short names")]
    public string[] DisallowedItemShortNames;
    [JsonProperty("Allowed skin IDs")]
    public HashSet<ulong> AllowedSkinIds;
    [JsonProperty("Disallowed skin IDs")]
    public HashSet<ulong> DisallowedSkinIds;
    [JsonIgnore]
    protected ItemCategory[] _allowedItemCategories;
    [JsonIgnore]
    protected ItemCategory[] _disallowedItemCategories;
    [JsonIgnore]
    protected HashSet<int> _allowedItemIds;
    [JsonIgnore]
    protected HashSet<int> _disallowedItemIds;
    [JsonIgnore]
    public string Permission { get; set; }
    [JsonIgnore]
    public bool AllowsAll { get; set; }
    public void Init(Backpacks plugin);
    public bool AllowsItem(Item item);
    public bool AllowsItem(ItemData itemData);
}

[JsonObject(MemberSerialization.OptIn)]
private class RestrictionRuleset : BaseItemRuleset
{
    private const string PartialPermissionPrefix;
    public static readonly string FullPermissionPrefix;
    public static readonly RestrictionRuleset AllowAll;
    protected override string PermissionPrefix { get; set; }
}

[JsonObject(MemberSerialization.OptIn)]
private class RestrictionOptions
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Enable legacy noblacklist permission")]
    public bool EnableLegacyPermission;
    [JsonProperty("Feedback effect")]
    public string FeedbackEffect;
    [JsonProperty("Default ruleset")]
    public RestrictionRuleset DefaultRuleset;
    [JsonProperty("Rulesets by permission")]
    public RestrictionRuleset[] RulesetsByPermission;
    [JsonIgnore]
    private Permission _permission;
    public void Init(Backpacks plugin);
    public RestrictionRuleset GetForPlayer(string userIdString);
}

[JsonObject(MemberSerialization.OptIn)]
private class WipeRuleset : BaseItemRuleset
{
    public static readonly WipeRuleset AllowAll;
    [JsonIgnore]
    protected override string PermissionPrefix { get; set; }
    [JsonProperty("Max slots to keep")]
    public int MaxSlotsToKeep;
    [JsonIgnore]
    public bool DisallowsAll { get; set; }
}

[JsonObject(MemberSerialization.OptIn)]
private class WipeOptions
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Enable legacy keeponwipe permission")]
    public bool EnableLegacyPermission;
    [JsonProperty("Default ruleset")]
    public WipeRuleset DefaultRuleset;
    [JsonProperty("Rulesets by permission")]
    public WipeRuleset[] RulesetsByPermission;
    [JsonIgnore]
    private Permission _permission;
    public void Init(Backpacks plugin);
    public WipeRuleset GetForPlayer(string userIdString);
}

[JsonObject(MemberSerialization.OptIn)]
private class BackpackSizeOptions
{
    [JsonProperty("Default size")]
    public int DefaultSize;
    [JsonProperty("Max size per page")]
    public int MaxCapacityPerPage;
    [JsonProperty("Enable legacy backpacks.use.1-8 row permissions")]
    public bool EnableLegacyRowPermissions;
    [JsonProperty("Permission sizes")]
    public int[] PermissionSizes;
    [JsonProperty("Dynamic Size (EXPERIMENTAL)")]
    public DynamicCapacity DynamicSize;
}

[JsonObject(MemberSerialization.OptIn)]
private class ContainerUiOptions
{
    [JsonProperty("Show page buttons on container bar")]
    public bool ShowPageButtonsOnContainerBar;
    [JsonProperty("Max page buttons to show")]
    public int MaxPageButtonsToShow;
}

[JsonObject(MemberSerialization.OptIn)]
private class CapacityProfile
{
    [JsonProperty("Permission suffix")]
    public string PermissionSuffix;
    [JsonProperty("Initial size")]
    public int InitialCapacity;
    [JsonProperty("Max size")]
    public int MaxCapacity;
    [JsonIgnore]
    public string Permission { get; set; }
    public void Init(Backpacks plugin);
}

[JsonObject(MemberSerialization.OptIn)]
private class CapacityResetOptions
{
    [JsonProperty("Enabled")]
    public bool Enabled;
}

[JsonObject(MemberSerialization.OptIn)]
private class DynamicCapacity
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Reset dynamic size on wipe")]
    public CapacityResetOptions CapacityResetOptions;
    [JsonProperty("Size profiles")]
    public CapacityProfile[] CapacityProfiles;
    [JsonIgnore]
    private Permission _permission;
    public void Init(Backpacks plugin);
    public (int, int)? GetPlayerProfile(string userIdString);
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : BaseConfiguration
{
    [JsonProperty("Backpack size")]
    public BackpackSizeOptions BackpackSize;
    [JsonProperty("Backpack Size (1-8 Rows)")]
    private int DeprecatedBackpackRows { get; set; }
    [JsonProperty("Backpack Size (1-7 Rows)")]
    private int DeprecatedBackpackSize { get; set; }
    [JsonProperty("Default Backpack Size")]
    private int DeprecatedDefaultBackpackSize { get; set; }
    [JsonProperty("Max Size Per Page")]
    private int DeprecatedMaxSizePerPage { get; set; }
    [JsonProperty("Backpack Permission Sizes")]
    private int[] DeprecatedPermissionSizes { get; set; }
    [JsonProperty("Enable Legacy Row Permissions (true/false)")]
    private bool EnableLegacyRowPermissions { get; set; }
    [JsonProperty("Drop on Death (true/false)")]
    public bool DropOnDeath;
    [JsonProperty("Erase on Death (true/false)")]
    public bool EraseOnDeath;
    [JsonProperty("Clear Backpacks on Map-Wipe (true/false)")]
    public bool DeprecatedClearBackpacksOnWipe;
    public bool ShouldSerializeDeprecatedClearBackpacksOnWipe();
    [JsonProperty("Use Blacklist (true/false)")]
    public bool DeprecatedUseDenylist;
    public bool ShouldSerializeDeprecatedUseDenylist();
    [JsonProperty("Blacklisted Items (Item Shortnames)")]
    public string[] DeprecatedDenylistItemShortNames;
    public bool ShouldSerializeDeprecatedDenylistItemShortNames();
    [JsonProperty("Use Whitelist (true/false)")]
    public bool DeprecatedUseAllowlist;
    public bool ShouldSerializeDeprecatedUseAllowlist();
    [JsonProperty("Whitelisted Items (Item Shortnames)")]
    public string[] DeprecatedAllowedItemShortNames;
    public bool ShouldSerializeDeprecatedAllowedItemShortNames();
    [JsonProperty("Minimum Despawn Time (Seconds)")]
    public float MinimumDespawnTime;
    [JsonProperty("GUI Button")]
    public GUIButton GUI;
    [JsonProperty("Container UI")]
    public ContainerUiOptions ContainerUi;
    [JsonProperty("Softcore")]
    public SoftcoreOptions Softcore;
    [JsonProperty("Item restrictions")]
    public RestrictionOptions ItemRestrictions;
    [JsonProperty("Clear on wipe")]
    public WipeOptions ClearOnWipe;
    public class GUIButton
    {
        [JsonProperty("Enabled")]
        public bool Enabled;
        [JsonProperty("Enabled by default (for players with permission)")]
        public bool EnabledByDefault;
        [JsonProperty("Skin Id")]
        public ulong SkinId;
        [JsonProperty("Image")]
        public string Image;
        [JsonProperty("Background Color")]
        public string Color;
        [JsonProperty("GUI Button Position")]
        public Position GUIButtonPosition;
        public class Position
        {
            [JsonProperty("Anchors Min")]
            public string AnchorsMin;
            [JsonProperty("Anchors Max")]
            public string AnchorsMax;
            [JsonProperty("Offsets Min")]
            public string OffsetsMin;
            [JsonProperty("Offsets Max")]
            public string OffsetsMax;
        }

    }

    public class SoftcoreOptions
    {
        [JsonProperty("Reclaim Fraction")]
        public float ReclaimFraction;
    }

    public void Init(Backpacks plugin);
}

public class GUIButton
{
    [JsonProperty("Enabled")]
    public bool Enabled;
    [JsonProperty("Enabled by default (for players with permission)")]
    public bool EnabledByDefault;
    [JsonProperty("Skin Id")]
    public ulong SkinId;
    [JsonProperty("Image")]
    public string Image;
    [JsonProperty("Background Color")]
    public string Color;
    [JsonProperty("GUI Button Position")]
    public Position GUIButtonPosition;
    public class Position
    {
        [JsonProperty("Anchors Min")]
        public string AnchorsMin;
        [JsonProperty("Anchors Max")]
        public string AnchorsMax;
        [JsonProperty("Offsets Min")]
        public string OffsetsMin;
        [JsonProperty("Offsets Max")]
        public string OffsetsMax;
    }

}

public class Position
{
    [JsonProperty("Anchors Min")]
    public string AnchorsMin;
    [JsonProperty("Anchors Max")]
    public string AnchorsMax;
    [JsonProperty("Offsets Min")]
    public string OffsetsMin;
    [JsonProperty("Offsets Max")]
    public string OffsetsMax;
}

public class SoftcoreOptions
{
    [JsonProperty("Reclaim Fraction")]
    public float ReclaimFraction;
}

[JsonObject(MemberSerialization.OptIn)]
private class BaseConfiguration
{
    public bool UsingDefaults;
    private string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private static class JsonHelper
{
    public static object Deserialize(string json);
    private static object ToObject(JToken token);
}

private class LangEntry
{
    public static readonly List<LangEntry> AllLangEntries;
    public static readonly LangEntry NoPermission;
    public static readonly LangEntry MayNotOpenBackpackInEvent;
    public static readonly LangEntry ViewBackpackSyntax;
    public static readonly LangEntry ViewBackpackProtected;
    public static readonly LangEntry UserIDNotFound;
    public static readonly LangEntry UserNameNotFound;
    public static readonly LangEntry MultiplePlayersFound;
    public static readonly LangEntry BackpackItemRejected;
    public static readonly LangEntry BackpackItemsRejected;
    public static readonly LangEntry BackpackOverCapacity;
    public static readonly LangEntry BlacklistedItemsRemoved;
    public static readonly LangEntry BackpackFetchSyntax;
    public static readonly LangEntry BackpackCapacitySyntax;
    public static readonly LangEntry DynamicCapacityNotEnabled;
    public static readonly LangEntry ChangeCapacitySuccess;
    public static readonly LangEntry InvalidItem;
    public static readonly LangEntry InvalidItemAmount;
    public static readonly LangEntry ItemNotInBackpack;
    public static readonly LangEntry ItemsFetched;
    public static readonly LangEntry FetchFailed;
    public static readonly LangEntry ToggledBackpackGUI;
    public static readonly LangEntry BackpackItemsReclaimed;
    public static readonly LangEntry SetGatherSyntax;
    public static readonly LangEntry PageOutOfRange;
    public static readonly LangEntry SetGatherModeSuccess;
    public static readonly LangEntry GatherModeAll;
    public static readonly LangEntry GatherModeExisting;
    public static readonly LangEntry GatherModeOff;
    public static readonly LangEntry UIGatherAll;
    public static readonly LangEntry UIGatherExisting;
    public static readonly LangEntry UIGatherOff;
    public static readonly LangEntry UIRetrieveOn;
    public static readonly LangEntry UIRetrieveOff;
    public string Name;
    public string English;
    public LangEntry(string name, string english);
}


```

---

## BackpackSlotItem by Lorenzo - Allow items like diving tank, to use the backpack slot on the player

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Backpack Slot Item", "Lorenzo", "1.0.2")]
[Description("Modify item allowed in backpack slot")]
 class BackpackSlotItem : CovalencePlugin
{
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Items allowed in Backpack slot", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> ItemsForBackpackSlot;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private Dictionary<string, BackupInfo> backupinfo;
    private void OnServerInitialized();
    private void Unload();
}

private class Configuration
{
    [JsonProperty(PropertyName = "Items allowed in Backpack slot", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> ItemsForBackpackSlot;
}


```

---

## BackpackSwap by Whispers88 - Allows you to swap backpacks without having to empty them.

```csharp

Oxide.Plugins
[Info("BackpackSwap", "Whispers88", "1.0.1")]
[Description("Allows you to swap your backpacks without moving items manually")]
public class BackpackSwap : CovalencePlugin
{
    private object CanWearItem(PlayerInventory inventory, Item targetItem, int slot);
}


```

---

## BackpackUpgrader by waayne - Allows admin/players to upgrade their backpacks via console

```csharp
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Backpack Upgrader", "waayne", "1.0.2")]
[Description("Allows players to upgrade their backpacks.")]
internal class BackpackUpgrader : CovalencePlugin
{
    private const string UPGRADE_PERMISSION;
    private const string SET_PERMISSION;
    private const string BACKPACKS_PERMISSION;
    private const string UPGRADE_COMMAND;
    private const string SET_COMMAND;
    private const int BACKPACKS_ROWS;
    [PluginReference]
    private Plugin Backpacks;
    private void Init();
    private void OnServerInitialized(bool initial);
    protected override void LoadDefaultMessages();
    private void OnNewSave(string filename);
    private void UpgradeCommand(IPlayer player, string command, string[] args);
    private void SetCommand(IPlayer player, string command, string[] args);
    private static string GetPermissionFromLevel(int row);
}


```

---

## BackPumpJack by Lorenzo - Allows players to use survey charges to create an oil crater

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using Rust;
using UnityEngine;
using UnityEngine.Networking;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Back Pump Jack", "Arainrr/Lorenzo", "1.4.33")]
[Description("Allows players to use survey charges to create oil/mining crater")]
public class BackPumpJack : CovalencePlugin
{
    [PluginReference]
    private Plugin Friends;
    [PluginReference]
    private Plugin Clans;
    [PluginReference]
    private Plugin Notify;
    private static BackPumpJack _instance;
    private static DiscordComponent _discord;
    private const string TraceFile;
    private const uint FuelStoragePumpID;
    private const uint OutputHopperPumpID;
    private const uint FuelStorageQuarryID;
    private const uint OutputHopperQuarryID;
    private const string PREFAB_CRATER_OIL;
    private readonly HashSet<ulong> _checkedCraters;
    private readonly List<QuarryData> _activeCraters;
    private readonly List<MiningQuarry> _miningQuarries;
    private readonly Dictionary<ulong, PermissionSettings> _activeSurveyCharges;
    private int QuarryDefaultDieselStack;
    private int QuarryDefaultFuelCapacity;
    private int QuarryDefaultHopperSlots;
    private int PumpDefaultDieselStack;
    private int PumpDefaultFuelCapacity;
    private int PumpDefaultHopperSlots;
    public Timer refilltimer;
    private readonly float PlayerRadius;
    private readonly float MatchRadius;
    const char Platform;
     QuarryRates miningQuarryBackup;
     QuarryRates pumpjackbackup;
    private void Init();
    private void Unload();
    private void OnServerInitialized(bool initial);
    private void OnServerSave();
    public readonly string[] QuarryTypeLookup;
    private void OnEntitySpawned(MiningQuarry miningQuarry);
    private bool UpdateAndRefill(MiningQuarry miningQuarry);
    private void OnEntityKill(MiningQuarry miningQuarry);
    private void OnExplosiveThrown(BasePlayer player, SurveyCharge surveyCharge);
    private void OnEntityKill(SurveyCharge surveyCharge);
    private void OnEntityKill(SurveyCrater crater);
    private object OnEntityTakeDamage(SurveyCrater surveyCrater, HitInfo info);
    private object OnEntityTakeDamage(StorageContainer storage, HitInfo info);
    private object CanBuild(Planner planner, Construction prefab, Construction.Target target);
    private void CanLootEntity(BasePlayer player, StorageContainer container);
     void OnLootEntityEnd(BasePlayer player, StorageContainer container);
     void LootQuarryOutput(BasePlayer player, StorageContainer container);
     void LootDieselEngineEnd(BasePlayer player, StorageContainer fueltank);
    private int RefillMiningQuarries(bool AddMissing);
    private void CheckValidData();
    private static void CreateResourceDeposit(MiningQuarry miningQuarry, QuarryData quarryData);
    private static void CreateResourceDeposit(ResourceDepositManager.ResourceDeposit deposit, QuarryData quarryData);
    private QuarryData ModifyResourceDeposit(PermissionSettings permissionSettings, Vector3 checkPosition, ulong playerID);
    private void CopyResourceDepositInfo(PermissionSettings permissionSettings, Vector3 checkPosition, ulong playerID, QuarryData crater);
     QuarryData findInactiveCraterData(SurveyCharge surveycharge);
     string ReportInventory(StorageContainer storage);
    private PermissionSettings GetPermissionSetting(string UserIDString);
    private bool AreFriends(ulong playerID, ulong friendID);
    private bool HasFriend(ulong playerID, ulong friendID);
    private bool SameTeam(ulong playerID, ulong friendID);
    private bool SameClan(ulong playerID, ulong friendID);
    private bool IsAdmin(ulong id);
     bool IspluginLoaded(Plugin a);
     string getDepositInfo(ResourceDepositManager.ResourceDeposit deposit);
     string getDepositInfo(QuarryData deposit);
    private void CCmdRefresh(IPlayer iplayer, string command, string[] args);
    private void CCmdResetDeposit(IPlayer iplayer, string command, string[] args);
    private void CCmdInfo(IPlayer iplayer, string command, string[] args);
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Settings")]
        public GlobalSettings Global { get; set; }
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings Chat { get; set; }
        [JsonProperty(PropertyName = "Permission List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<PermissionSettings> Permissions { get; set; }
        [JsonProperty(PropertyName = "Static quarry settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public Dictionary<string, StaticQuarrySettings> StaticQuarryType { get; set; }
        [JsonProperty(PropertyName = "Apply patch for mining rates for more precise pM config params")]
        public bool ApplyPatchForMiningRates;
        [JsonProperty(PropertyName = "Patch for ladder flyhack")]
        public bool PatchLadder;
        [JsonProperty(PropertyName = "Patch for light signal when quarry is running")]
        public bool PatchLightSignal;
        [JsonProperty(PropertyName = "Maximum stack size for diesel engine (-1 to disable function)")]
        public int DieselFuelMaxStackSize;
        [JsonProperty(PropertyName = "Number of slots for diesel storage (-1 to disable function)")]
        public int FuelSlots;
        [JsonProperty(PropertyName = "Number of slots for output storage (-1 to disable function)")]
        public int HopperSlots;
        [JsonProperty(PropertyName = "Time per barrel of diesel in second (-1 to disable function, default time 125 sec)")]
        public int TimePerBarrel;
        [JsonProperty(PropertyName = "Enable static quarry resource modifier")]
        public bool StaticQuarryModifier;
        [JsonProperty(PropertyName = "refill command name")]
        public string CommandRefill;
        [JsonProperty(PropertyName = "Info command name")]
        public string CommandInfo;
        [JsonProperty(PropertyName = "Reset resource deposit command name")]
        public string CommandResetDeposits;
        [JsonProperty(PropertyName = "Search radius for past resource deposit allocation (use 0.0 to disable)")]
        public float ResourceDepositCheckRadius;
        [JsonProperty(PropertyName = "Items in report list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] ReportItems;
        [JsonProperty(PropertyName = "Permission Admin")]
        public string PermissionAdmin { get; set; }
        [JsonProperty(PropertyName = "Version")]
        public VersionNumber Version { get; set; }
    }

    private class GlobalSettings
    {
        [JsonProperty(PropertyName = "Use Teams")]
        public bool UseTeams { get; set; }
        [JsonProperty(PropertyName = "Use Friends")]
        public bool UseFriends { get; set; }
        [JsonProperty(PropertyName = "Use Clans")]
        public bool UseClans { get; set; }
        [JsonProperty(PropertyName = "Use clan table")]
        public bool UseClanTable { get; set; }
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
        [JsonProperty(PropertyName = "Block damage another player's survey crater")]
        public bool CantDamage { get; set; }
        [JsonProperty(PropertyName = "Block deploy a quarry on another player's survey crater")]
        public bool CantDeploy { get; set; }
    }

    private class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix { get; set; }
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong SteamIdIcon { get; set; }
    }

    private class PermissionSettings
    {
        [JsonProperty(PropertyName = "Permission")]
        public string Permission { get; set; }
        [JsonProperty(PropertyName = "Priority")]
        public int Priority { get; set; }
        [JsonProperty(PropertyName = "Oil Crater Chance")]
        public float OilCraterChance { get; set; }
        [JsonProperty(PropertyName = "Oil Crater Settings")]
        public PumpJackSettings PumpJack { get; set; }
        [JsonProperty(PropertyName = "Normal Crater Settings")]
        public QuarrySettings Quarry { get; set; }
    }

    private abstract class MiningSettings
    {
        [JsonProperty(PropertyName = "Minimum Mineral Amount")]
        public int AmountMin { get; set; }
        [JsonProperty(PropertyName = "Maximum Mineral Amount")]
        public int AmountMax { get; set; }
        [JsonProperty(PropertyName = "Allow Duplication Of Mineral Item")]
        public bool AllowDuplication { get; set; }
        [JsonProperty(PropertyName = "Mineral Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<MineralItem> MineralItems { get; set; }
        [JsonIgnore]
        public abstract bool IsLiquid { get; set; }
        public abstract float GetWorkPerMinute();
        public List<MineralItemData> RefillResourceDeposit(ResourceDepositManager.ResourceDeposit deposit);
    }

    private class QuarrySettings : MiningSettings
    {
        [JsonProperty(PropertyName = "Modify Chance (If not modified, use default mineral)", Order = -1)]
        public float ModifyChance { get; set; }
        public static float WorkPerMinute { get; set; }
        public override bool IsLiquid { get; set; }
        public override float GetWorkPerMinute();
    }

    private class PumpJackSettings : MiningSettings
    {
        public static float WorkPerMinute { get; set; }
        public override bool IsLiquid { get; set; }
        public override float GetWorkPerMinute();
    }

    private class MineralItem
    {
        [JsonProperty(PropertyName = "Mineral Item Short Name")]
        public string ShortName { get; set; }
        [JsonProperty(PropertyName = "Chance")]
        public float Chance { get; set; }
        [JsonProperty(PropertyName = "Minimum pM")]
        public float PmMin { get; set; }
        [JsonProperty(PropertyName = "Maximum pM")]
        public float PmMax { get; set; }
    }

    private class StaticQuarrySettings
    {
        [JsonProperty(PropertyName = "Mineral Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<StaticMineralItem> MineralItems { get; set; }
        public void RefillResourceDeposit(ResourceDepositManager.ResourceDeposit deposit, float WorkPerMinute, bool IsLiquid);
    }

    private class StaticMineralItem
    {
        [JsonProperty(PropertyName = "Mineral Item Short Name")]
        public string ShortName { get; set; }
        [JsonProperty(PropertyName = "Resource per minutes (pM)")]
        public float pM { get; set; }
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private StoredData _storedData;
    private class StoredData
    {
        public readonly List<QuarryData> quarryDataList;
    }

    private class QuarryData
    {
        public Vector3 position;
        public bool isLiquid;
        public List<MineralItemData> mineralItems;
    }

    private class MineralItemData
    {
        public string shortname;
        public int amount;
        public float workNeeded;
    }

    private void LoadData();
    private void ClearData();
    private void SaveData();
    private void PrintToLog(string message);
    private void BroadcastMessage(string msg, object[] args);
    public void SendChatMessage(BasePlayer player, string msg, object[] args);
    private void PrintToDiscord(string message, double seconds);
    private string PositionToString(Vector3 position);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
    const string _lampPrefab;
    const string _ladderprefab;
    public class QuarryStateDetector : MonoBehaviour
    {
        public MiningQuarry quarry;
         bool state;
         SimpleLight lamp1;
         SimpleLight lamp2;
         BaseLadder ladder1;
         BaseLadder ladder2;
         Vector3 QuarryLampPos1;
         Vector3 QuarryLampPos2;
         Vector3 PumpLampPos1;
         Vector3 PumpLampPos2;
         Vector3 LampRotation1;
         Vector3 LampRotation2;
         Vector3 LadderPosition1;
         Vector3 LadderPosition2;
         Vector3 LadderRotation;
         void Awake();
         void OnDestroy();
         void CheckMiningQuarry();
        private static void HideInputsAndOutputs(IOEntity ioEntity);
        private static void RemoveProblemComponents(BaseEntity entity);
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

}

private class ConfigData
{
    [JsonProperty(PropertyName = "Settings")]
    public GlobalSettings Global { get; set; }
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings Chat { get; set; }
    [JsonProperty(PropertyName = "Permission List", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<PermissionSettings> Permissions { get; set; }
    [JsonProperty(PropertyName = "Static quarry settings", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public Dictionary<string, StaticQuarrySettings> StaticQuarryType { get; set; }
    [JsonProperty(PropertyName = "Apply patch for mining rates for more precise pM config params")]
    public bool ApplyPatchForMiningRates;
    [JsonProperty(PropertyName = "Patch for ladder flyhack")]
    public bool PatchLadder;
    [JsonProperty(PropertyName = "Patch for light signal when quarry is running")]
    public bool PatchLightSignal;
    [JsonProperty(PropertyName = "Maximum stack size for diesel engine (-1 to disable function)")]
    public int DieselFuelMaxStackSize;
    [JsonProperty(PropertyName = "Number of slots for diesel storage (-1 to disable function)")]
    public int FuelSlots;
    [JsonProperty(PropertyName = "Number of slots for output storage (-1 to disable function)")]
    public int HopperSlots;
    [JsonProperty(PropertyName = "Time per barrel of diesel in second (-1 to disable function, default time 125 sec)")]
    public int TimePerBarrel;
    [JsonProperty(PropertyName = "Enable static quarry resource modifier")]
    public bool StaticQuarryModifier;
    [JsonProperty(PropertyName = "refill command name")]
    public string CommandRefill;
    [JsonProperty(PropertyName = "Info command name")]
    public string CommandInfo;
    [JsonProperty(PropertyName = "Reset resource deposit command name")]
    public string CommandResetDeposits;
    [JsonProperty(PropertyName = "Search radius for past resource deposit allocation (use 0.0 to disable)")]
    public float ResourceDepositCheckRadius;
    [JsonProperty(PropertyName = "Items in report list", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] ReportItems;
    [JsonProperty(PropertyName = "Permission Admin")]
    public string PermissionAdmin { get; set; }
    [JsonProperty(PropertyName = "Version")]
    public VersionNumber Version { get; set; }
}

private class GlobalSettings
{
    [JsonProperty(PropertyName = "Use Teams")]
    public bool UseTeams { get; set; }
    [JsonProperty(PropertyName = "Use Friends")]
    public bool UseFriends { get; set; }
    [JsonProperty(PropertyName = "Use Clans")]
    public bool UseClans { get; set; }
    [JsonProperty(PropertyName = "Use clan table")]
    public bool UseClanTable { get; set; }
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
    [JsonProperty(PropertyName = "Block damage another player's survey crater")]
    public bool CantDamage { get; set; }
    [JsonProperty(PropertyName = "Block deploy a quarry on another player's survey crater")]
    public bool CantDeploy { get; set; }
}

private class ChatSettings
{
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix { get; set; }
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong SteamIdIcon { get; set; }
}

private class PermissionSettings
{
    [JsonProperty(PropertyName = "Permission")]
    public string Permission { get; set; }
    [JsonProperty(PropertyName = "Priority")]
    public int Priority { get; set; }
    [JsonProperty(PropertyName = "Oil Crater Chance")]
    public float OilCraterChance { get; set; }
    [JsonProperty(PropertyName = "Oil Crater Settings")]
    public PumpJackSettings PumpJack { get; set; }
    [JsonProperty(PropertyName = "Normal Crater Settings")]
    public QuarrySettings Quarry { get; set; }
}

private abstract class MiningSettings
{
    [JsonProperty(PropertyName = "Minimum Mineral Amount")]
    public int AmountMin { get; set; }
    [JsonProperty(PropertyName = "Maximum Mineral Amount")]
    public int AmountMax { get; set; }
    [JsonProperty(PropertyName = "Allow Duplication Of Mineral Item")]
    public bool AllowDuplication { get; set; }
    [JsonProperty(PropertyName = "Mineral Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<MineralItem> MineralItems { get; set; }
    [JsonIgnore]
    public abstract bool IsLiquid { get; set; }
    public abstract float GetWorkPerMinute();
    public List<MineralItemData> RefillResourceDeposit(ResourceDepositManager.ResourceDeposit deposit);
}

private class QuarrySettings : MiningSettings
{
    [JsonProperty(PropertyName = "Modify Chance (If not modified, use default mineral)", Order = -1)]
    public float ModifyChance { get; set; }
    public static float WorkPerMinute { get; set; }
    public override bool IsLiquid { get; set; }
    public override float GetWorkPerMinute();
}

private class PumpJackSettings : MiningSettings
{
    public static float WorkPerMinute { get; set; }
    public override bool IsLiquid { get; set; }
    public override float GetWorkPerMinute();
}

private class MineralItem
{
    [JsonProperty(PropertyName = "Mineral Item Short Name")]
    public string ShortName { get; set; }
    [JsonProperty(PropertyName = "Chance")]
    public float Chance { get; set; }
    [JsonProperty(PropertyName = "Minimum pM")]
    public float PmMin { get; set; }
    [JsonProperty(PropertyName = "Maximum pM")]
    public float PmMax { get; set; }
}

private class StaticQuarrySettings
{
    [JsonProperty(PropertyName = "Mineral Items", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<StaticMineralItem> MineralItems { get; set; }
    public void RefillResourceDeposit(ResourceDepositManager.ResourceDeposit deposit, float WorkPerMinute, bool IsLiquid);
}

private class StaticMineralItem
{
    [JsonProperty(PropertyName = "Mineral Item Short Name")]
    public string ShortName { get; set; }
    [JsonProperty(PropertyName = "Resource per minutes (pM)")]
    public float pM { get; set; }
}

private class StoredData
{
    public readonly List<QuarryData> quarryDataList;
}

private class QuarryData
{
    public Vector3 position;
    public bool isLiquid;
    public List<MineralItemData> mineralItems;
}

private class MineralItemData
{
    public string shortname;
    public int amount;
    public float workNeeded;
}

public class QuarryStateDetector : MonoBehaviour
{
    public MiningQuarry quarry;
     bool state;
     SimpleLight lamp1;
     SimpleLight lamp2;
     BaseLadder ladder1;
     BaseLadder ladder2;
     Vector3 QuarryLampPos1;
     Vector3 QuarryLampPos2;
     Vector3 PumpLampPos1;
     Vector3 PumpLampPos2;
     Vector3 LampRotation1;
     Vector3 LampRotation2;
     Vector3 LadderPosition1;
     Vector3 LadderPosition2;
     Vector3 LadderRotation;
     void Awake();
     void OnDestroy();
     void CheckMiningQuarry();
    private static void HideInputsAndOutputs(IOEntity ioEntity);
    private static void RemoveProblemComponents(BaseEntity entity);
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

## BackupExtended by VisEntities - Extends the built-in backup process in Rust to provide more flexibility and additional features

```csharp
using System;
using System.Collections.Generic;
using System.Collections;
using System.Linq;
using System.Reflection;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using ProtoBuf;
using Network;

Oxide.Plugins
[Info("Backup Extended", "Fujikura", "1.0.1")]
 class BackupExtended : RustPlugin
{
     bool Changed;
     bool _backup;
     int currentRetry;
     bool wasShutDown;
     string [] backupFolders;
     string [] backupFoldersOxide;
     string [] backupFoldersShutdown;
     string [] backupFoldersShutdownOxide;
     int numberOfBackups;
     bool backupBroadcast;
     int backupDelay;
     bool useBroadcastDelay;
     string prefix;
     string prefixColor;
     bool useTimer;
     int timerInterval;
     int maxPlayers;
     int maxRetry;
     int delayRetrySeconds;
     string currentIdentity;
     bool includeOxideInBackups;
     object GetConfig(string menu, string datavalue, object defaultValue);
     void LoadVariables();
     void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
     void Loaded();
     void OnServerInitialized();
     void OnPluginUnloaded(Plugin name);
     IEnumerator BackupCreateI(bool manual);
     void BackupCreate(bool manual);
     void TimerCheck();
    [ConsoleCommand("extbackup")]
     void ccmdExtBackup(ConsoleSystem.Arg arg);
     void BackupRun(ConsoleSystem.Arg arg);
     string [] BackupFolders();
     string [] BackupFoldersOxide();
     string [] BackupFoldersShutdown();
     string [] BackupFoldersShutdownOxide();
     void BroadcastChat(string msg);
}


```

---

## BalloonPlus by misticos - Controls the flight of the balloon

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Balloon Plus", "Iv Misticos", "1.0.8")]
[Description("Control your balloon's flight")]
 class BalloonPlus : RustPlugin
{
    private static Dictionary<string, SpeedData> _cachedSpeedData;
    private void UpdateCacheSpeedData(string id);
    private bool CacheSpeedDataAllowed(string id, SpeedData data);
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Speed Modifiers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<SpeedData> Modifiers;
        [JsonProperty(PropertyName = "Move Button")]
        public string MoveButton;
        [JsonProperty(PropertyName = "Disable Wind Force")]
        public bool DisableWindForce;
        [JsonProperty(PropertyName = "Move Frequency")]
        public float Frequency;
        [JsonProperty(PropertyName = "Speed Modifier", NullValueHandling = NullValueHandling.Ignore)]
        public float? SpeedModifier;
        [JsonIgnore]
        public BUTTON ParsedMoveButton;
    }

    private class SpeedData
    {
        [JsonProperty(PropertyName = "Permission")]
        public string Permission;
        [JsonProperty(PropertyName = "Speed Modifier")]
        public float Modifier;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnEntitySpawned(BaseNetworkable entity);
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerConnected(BasePlayer player);
    private void OnUserGroupAdded(string id, string groupName);
    private void OnUserGroupRemoved(string id, string groupName);
    private void OnUserPermissionGranted(string id, string permName);
    private void OnUserPermissionRevoked(string id, string permName);
    private void HandleUpdate();
    private void HandleUpdate(BasePlayer player);
}

private class Configuration
{
    [JsonProperty(PropertyName = "Speed Modifiers", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<SpeedData> Modifiers;
    [JsonProperty(PropertyName = "Move Button")]
    public string MoveButton;
    [JsonProperty(PropertyName = "Disable Wind Force")]
    public bool DisableWindForce;
    [JsonProperty(PropertyName = "Move Frequency")]
    public float Frequency;
    [JsonProperty(PropertyName = "Speed Modifier", NullValueHandling = NullValueHandling.Ignore)]
    public float? SpeedModifier;
    [JsonIgnore]
    public BUTTON ParsedMoveButton;
}

private class SpeedData
{
    [JsonProperty(PropertyName = "Permission")]
    public string Permission;
    [JsonProperty(PropertyName = "Speed Modifier")]
    public float Modifier;
}


```

---

## BanDeleteBy by Ryan - Removes all entities placed by a player when they get banned

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Ban Delete By", "Ryan", "1.0.4")]
[Description("Removes all entities placed by a player when they get banned.")]
 class BanDeleteBy : RustPlugin
{
    private void OnUserBanned(string name, string id);
    private IEnumerator Delete(IEnumerable<BaseNetworkable> list, HashSet<ulong> banList);
    [ConsoleCommand("deleteby.removeall")]
    private void RemoveAllCommand(ConsoleSystem.Arg args);
}


```

---

## BanditHide by haggbart - Hides name when wearing specific clothes

```csharp
using Oxide.Core;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("BanditHide", "mangiang", "1.0.6", ResourceId = 2470)]
[Description("Hides name when wearing specific clothes")]
 class BanditHide : RustPlugin
{
     StoredData storedData;
     string changedName;
     string DefaultChangedName;
     List<string> masks;
     bool DebugMode;
     string perm;
     class StoredData
    {
        public Dictionary<string, string> banditNames;
        public StoredData();
    }

    protected override void LoadDefaultConfig();
    private void Init();
     void LoadConfigValues();
     void OnServerInitialized();
     void LoadDefaultMessages();
     object OnPlayerRespawned(BasePlayer player);
     object OnPlayerSleepEnded(BasePlayer player);
     object CanWearItem(PlayerInventory inventory, Item item);
     object OnItemAddedToContainer(ItemContainer container, Item item);
     object OnItemRemovedFromContainer(ItemContainer container, Item item);
     bool IsWearingAMask(BasePlayer player);
     bool IsMask(Item item);
     void Log(string text);
}

 class StoredData
{
    public Dictionary<string, string> banditNames;
    public StoredData();
}


```

---

## BanFix by  - Stops the server from freezing when banning players

```csharp
using System.Collections.Generic;
using System;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using Oxide.Core.Configuration;
using UnityEngine;
using System.Linq;
using System.Diagnostics;
using System.Collections;

Oxide.Plugins
[Info("BanFix", "Jake_Rich", "1.0.0")]
[Description("")]
public class BanFix : RustPlugin
{
    private Facepunch.Sqlite.Database banDB;
    public const string TableName;
     void SetupDatabase();
    private IEnumerator SaveBansToDatabase();
     void OnServerInitialized();
     void Unload();
     void OnServerShutdown();
     object OnServerCommand(ConsoleSystem.Arg arg);
     void OverwriteBanCommand(ConsoleSystem.Arg arg);
     Coroutine saveRoutine;
     void OnServerSaveUsers();
     void OnServerLoadUsers();
     void OnPlayerBanned(string name, ulong steamID, string IP, string reason);
     void OnPlayerUnbanned(string name, ulong steamID, string IP);
    private bool LoadingServerUsers;
     void ClearDatabase();
     void UpdateUserGroup(ulong steamId, ServerUsers.UserGroup group, string name, string notes);
     void RemoveUserGroup(ulong steamId);
    [ConsoleCommand("bans.save.db")]
     void SaveBansToDB_CMD(ConsoleSystem.Arg arg);
    [ConsoleCommand("bans.save.file")]
     void SaveBansToFile_CMD(ConsoleSystem.Arg arg);
    [ConsoleCommand("bans.clear.db")]
     void ClearBansTest(ConsoleSystem.Arg arg);
    [ConsoleCommand("bans.clear.file")]
     void ClearLocalBans(ConsoleSystem.Arg arg);
    [ConsoleCommand("bans.load.db")]
     void LoadBansDB_CMD(ConsoleSystem.Arg arg);
    [ConsoleCommand("bans.load.file")]
     void LoadBansFile_CMD(ConsoleSystem.Arg arg);
    [ConsoleCommand("bans.check")]
     void ReadBansTest(ConsoleSystem.Arg arg);
    [ConsoleCommand("bans.test")]
     void TestOldBan(ConsoleSystem.Arg arg);
}


```

---

## Bank by Calytic - Safe player storage

```csharp
using System;
using System.Text;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info ("Bank", "Calytic", "1.0.53")]
[Description ("Safe player storage")]
 class Bank : RustPlugin
{
     string defaultBoxPrefab;
     int defaultSlots;
     Dictionary<string, object> boxPrefabs;
     Dictionary<string, object> boxSlots;
     bool keyring;
     float cooldownMinutes;
     bool npconly;
     List<string> npcids;
     float radiationMax;
     bool allowSafeZone;
    public static DataFileSystem datafile;
    public static DataFileSystem configFile;
     Dictionary<string, object> itemLimits;
    [PluginReference]
     Plugin MasterKey;
    public class ItemProfile
    {
        public string id;
        public int amount;
        public int slot;
        public Item.Flag flags;
        public float condition;
        public ulong skin;
        public List<ItemProfile> contents;
        public int primaryMagazine;
        public int ammoType;
        public int dataInt;
        [JsonConstructor]
        public ItemProfile(string id, int amount, int slot, Item.Flag flags, float condition, ulong skin, List<ItemProfile> contents, int primaryMagazine, int ammoType, int dataInt);
        public static ItemProfile Create(Item item);
    }

    public class BankProfile
    {
        protected ulong playerID;
        public List<ItemProfile> items;
        [JsonIgnore]
        public bool open;
        [JsonIgnore]
        public bool dirty;
        [JsonIgnore]
        public BasePlayer Player { get; set; }
        [JsonIgnore]
        public ulong PlayerID { get; set; }
        [JsonIgnore]
        public int Count { get; set; }
        public BankProfile();
        public BankProfile(BasePlayer player, List<ItemProfile> items);
        [JsonConstructor]
        public BankProfile(ulong playerID, List<ItemProfile> items);
        public bool Add(Item item);
        public bool Add(Item [] items);
        public bool Add(List<Item> items);
        public bool Remove(Item item);
        public bool Remove(Item [] items);
        public bool Remove(List<Item> items);
        [JsonIgnore]
         ItemContainer container;
        public ItemContainer GetContainer(BasePlayer player, int slots);
         void PopulateContainer(BasePlayer player, ItemContainer container, List<ItemProfile> items);
    }

     class ItemLimit
    {
        public bool enabled;
        public int minimum;
        public int maximum;
        [JsonConstructor]
        public ItemLimit(bool enabled, int minimum, int maximum);
    }

     class OnlinePlayer
    {
        public BasePlayer Player;
        public BasePlayer Target;
        public StorageContainer View;
        public List<BasePlayer> Matches;
        public OnlinePlayer(BasePlayer player);
    }

    public Dictionary<ItemContainer, ulong> containers;
    public Dictionary<ulong, BankProfile> banks;
    [OnlinePlayers]
     Hash<BasePlayer, OnlinePlayer> onlinePlayers;
     Dictionary<string, DateTime> bankCooldowns;
     void Init();
    new void LoadDefaultMessages();
     Dictionary<string, object> GetDefaultBoxes();
     Dictionary<string, object> GetDefaultSlots();
     Dictionary<string, object> GetDefaultItemLimits();
    protected override void LoadDefaultConfig();
     void Unloaded();
     void OnServerSave();
     void CheckConfig();
    protected void ReloadConfig();
     void SaveProfileByUser(ulong userID);
     void SaveData();
    protected bool LoadProfile(ulong playerID, bool reload);
     void SaveProfile(ulong playerID, BankProfile profile);
     bool IsBankBox(BaseNetworkable entity);
     bool TryGetPlayer(BaseNetworkable entity, BasePlayer player);
     void AddNpc(string id);
     void AddNpc(ulong id);
     void RemoveNpc(string id);
     void RemoveNpc(ulong id);
     void OnUseNPC(BasePlayer npc, BasePlayer player);
     object CanNetworkTo(BaseNetworkable entity, BasePlayer target);
     object OnEntityTakeDamage(BaseCombatEntity entity, HitInfo hitInfo);
     object OnEntityGroundMissing(BaseEntity entity);
     object CanUseLockedEntity(BasePlayer player, BaseLock lockItem);
     void OnPlayerConnected(BasePlayer player);
     void OnPlayerDisconnected(BasePlayer player);
     void OnPlayerLootEnd(PlayerLoot inventory);
    [ChatCommand ("viewbank")]
     void ViewBank(BasePlayer player, string command, string [] args);
    [ConsoleCommand ("bank")]
     void ccBank(ConsoleSystem.Arg arg);
    [ChatCommand ("bank")]
     void cmdBank(BasePlayer player, string command, string [] args);
     ItemLimit GetItemLimit(Item item);
     bool CanPlayerBank(BasePlayer player);
     void ShowBank(BasePlayer player, BaseEntity target);
     void HideBank(BasePlayer player);
     string GetBox(BasePlayer player);
     int GetSlots(BasePlayer player);
     void OpenBank(BasePlayer player, BaseEntity targArg);
     void CloseBank(BasePlayer player, StorageContainer view);
     void InvalidateBank(BasePlayer player, BankProfile profile, StorageContainer view);
     void SendHelpText(BasePlayer player);
     string GetMsg(string key, BasePlayer player);
     List<BasePlayer> FindPlayersByName(string name);
     void ShowMatchingPlayers(BasePlayer player);
     bool IsAllowed(BasePlayer player);
     T GetConfig(string name, string name2, T defaultValue);
     T GetConfig(string name, T defaultValue);
}

public class ItemProfile
{
    public string id;
    public int amount;
    public int slot;
    public Item.Flag flags;
    public float condition;
    public ulong skin;
    public List<ItemProfile> contents;
    public int primaryMagazine;
    public int ammoType;
    public int dataInt;
    [JsonConstructor]
    public ItemProfile(string id, int amount, int slot, Item.Flag flags, float condition, ulong skin, List<ItemProfile> contents, int primaryMagazine, int ammoType, int dataInt);
    public static ItemProfile Create(Item item);
}

public class BankProfile
{
    protected ulong playerID;
    public List<ItemProfile> items;
    [JsonIgnore]
    public bool open;
    [JsonIgnore]
    public bool dirty;
    [JsonIgnore]
    public BasePlayer Player { get; set; }
    [JsonIgnore]
    public ulong PlayerID { get; set; }
    [JsonIgnore]
    public int Count { get; set; }
    public BankProfile();
    public BankProfile(BasePlayer player, List<ItemProfile> items);
    [JsonConstructor]
    public BankProfile(ulong playerID, List<ItemProfile> items);
    public bool Add(Item item);
    public bool Add(Item [] items);
    public bool Add(List<Item> items);
    public bool Remove(Item item);
    public bool Remove(Item [] items);
    public bool Remove(List<Item> items);
    [JsonIgnore]
     ItemContainer container;
    public ItemContainer GetContainer(BasePlayer player, int slots);
     void PopulateContainer(BasePlayer player, ItemContainer container, List<ItemProfile> items);
}

 class ItemLimit
{
    public bool enabled;
    public int minimum;
    public int maximum;
    [JsonConstructor]
    public ItemLimit(bool enabled, int minimum, int maximum);
}

 class OnlinePlayer
{
    public BasePlayer Player;
    public BasePlayer Target;
    public StorageContainer View;
    public List<BasePlayer> Matches;
    public OnlinePlayer(BasePlayer player);
}


```

---

## BarrelEvent by  - Special actions on barrel destroying

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Barrel Event", "Orange", "1.1.0")]
[Description("Special actions on barrel destroying")]
public class BarrelEvent : RustPlugin
{
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void CheckDeath(BaseEntity entity);
    private void Spawn(float range, string prefab, Vector3 position);
    private void Damage(float radius, int damage, Vector3 position);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "1. Run only 1 event at action")]
        public bool single;
        [JsonProperty(PropertyName = "2.Cargo plane speed")]
        public float cargoSpeed;
        [JsonProperty(PropertyName = "3.Cargo plane height")]
        public float cargoHeight;
        [JsonProperty(PropertyName = "Event list:")]
        public List<Event> events;
        public class Event
        {
            [JsonProperty(PropertyName = "1. Chance")]
            public int chance;
            [JsonProperty(PropertyName = "2. Type")]
            public string type;
            [JsonProperty(PropertyName = "3. Parameter")]
            public string param;
            [JsonProperty(PropertyName = "4. Range")]
            public float range;
        }

    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "1. Run only 1 event at action")]
    public bool single;
    [JsonProperty(PropertyName = "2.Cargo plane speed")]
    public float cargoSpeed;
    [JsonProperty(PropertyName = "3.Cargo plane height")]
    public float cargoHeight;
    [JsonProperty(PropertyName = "Event list:")]
    public List<Event> events;
    public class Event
    {
        [JsonProperty(PropertyName = "1. Chance")]
        public int chance;
        [JsonProperty(PropertyName = "2. Type")]
        public string type;
        [JsonProperty(PropertyName = "3. Parameter")]
        public string param;
        [JsonProperty(PropertyName = "4. Range")]
        public float range;
    }

}

public class Event
{
    [JsonProperty(PropertyName = "1. Chance")]
    public int chance;
    [JsonProperty(PropertyName = "2. Type")]
    public string type;
    [JsonProperty(PropertyName = "3. Parameter")]
    public string param;
    [JsonProperty(PropertyName = "4. Range")]
    public float range;
}


```

---

## Barrelless by KrunghCrow - various events after barrel kills

```csharp
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using UnityEngine.AI;
using Oxide.Core;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Facepunch;
using Rust;
using Rust.Ai;
using HarmonyLib;

Oxide.Plugins
[Info("Barrelless", "Krungh Crow", "3.1.2")]
[Description("various events after barrel kills")]
 class Barrelless : RustPlugin
{
    [PluginReference]
     Plugin Kits;
     bool BlockSpawn;
     bool BlockOutside;
     bool IsSpawned;
     bool fireallreadyspawned;
     bool Signaldropped;
     ulong chaticon;
     string prefix;
     System.Random rnd;
    private ConfigData configData;
    public static Barrelless instance;
    const string Exclude_Perm;
    const string SciTrigger_Perm;
    const string ScareTrigger_Perm;
    const string BearTrigger_Perm;
    const string PBearTrigger_Perm;
    const string BoarTrigger_Perm;
    const string ChickenTrigger_Perm;
    const string WolfTrigger_Perm;
    const string ExploTrigger_Perm;
    const string FireTrigger_Perm;
    const string AirdropTrigger_Perm;
    const string HackCrateTrigger_Perm;
    const string zombie;
    const string _scientist;
     float SciDamageScale;
     int SciHealth;
     List<string> SciKit;
     float SciLifetime;
     string SciName;
     bool _UseKit;
     bool _FromHell;
    const string bearString;
    const string PBearString;
    const string boarstring;
    const string chickenString;
    const string wolfString;
     int _AnimalHealth;
     string _AnimalString;
     float _AnimalLife;
    const string oilfire;
    const string fireball;
    const string airdropString;
    const string hackcrateString;
    const string beancanString;
    const string grenadeString;
    const string satchelString;
    const string file_main;
    private class Zombies : FacepunchBehaviour
    {
        private ScarecrowNPC npc;
        public bool ReturningToHome;
        public bool isRoaming;
        public Vector3 SpawnPoint;
        private void Awake();
        private void Attack();
        public void _UseBrain();
         void GoHome();
        private void SettargetDestination(Vector3 position);
         void WipeMemory();
         void OnDestroy();
    }

    public class Scientists : FacepunchBehaviour
    {
        public global::HumanNPC npc;
        public bool ReturningToHome;
        public bool isRoaming;
        public Vector3 SpawnPoint;
         void Start();
        public void _UseBrain();
         void GoHome();
        private void SettargetDestination(Vector3 position);
         void WipeMemory();
         void OnDestroy();
    }

     class ConfigData
    {
        [JsonProperty(PropertyName = "Plugin Settings")]
        public SettingsDrop DropData;
        [JsonProperty(PropertyName = "Airdrop Settings")]
        public SettingsAirdrop AirdropData;
        [JsonProperty(PropertyName = "Hack crate Settings")]
        public SettingsHack HackData;
        [JsonProperty(PropertyName = "Scarecrow Settings")]
        public NPCSettings CrowData;
        [JsonProperty(PropertyName = "Scientist Settings")]
        public SCISettings SCIData;
        [JsonProperty(PropertyName = "Animal Settings")]
        public SettingsAnimal AnimalData;
        [JsonProperty(PropertyName = "Explosives Settings")]
        public SettingsExplosives ExplosivesData;
        [JsonProperty(PropertyName = "Fire Settings")]
        public SettingsFire FireData;
    }

     class SettingsDrop
    {
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string Prefix;
        [JsonProperty(PropertyName = "Drop : Count random per x barrels")]
        public int Barrelcountdrop { get; set; }
        [JsonProperty(PropertyName = "Drop : Max range to trigger events")]
        public int RangeTrigger { get; set; }
        [JsonProperty(PropertyName = "Drop : Spawn only 1 entity on trigger")]
        public bool Trigger;
        [JsonProperty(PropertyName = "Only allow spawning outside")]
        public bool TriggerOut;
        [JsonProperty(PropertyName = "Show messages")]
        public bool ShowMsg;
        [JsonProperty(PropertyName = "FX on trigger")]
        public string FX;
    }

     class SettingsHack
    {
        [JsonProperty(PropertyName = "Spawn chance (0-100)")]
        public int HackdropRate { get; set; }
        [JsonProperty(PropertyName = "Drop height")]
        public int HackdropHeight { get; set; }
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
    }

     class SettingsAirdrop
    {
        [JsonProperty(PropertyName = "Spawn chance (0-100)")]
        public int AirdropRate { get; set; }
        [JsonProperty(PropertyName = "Drop height")]
        public int AirdropHeight { get; set; }
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
    }

     class NPCSettings
    {
        [JsonProperty(PropertyName = "Spawn chance (0-100)")]
        public int SpawnRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
        [JsonProperty(PropertyName = "Spawn Amount")]
        public int NPCAmount;
        [JsonProperty(PropertyName = "NPC Spawns on fire")]
        public bool FromHell;
        [JsonProperty(PropertyName = "Max Roam Distance")]
        public int NPCRoamMax;
        [JsonProperty(PropertyName = "Prefix (Title)")]
        public string NPCName;
        [JsonProperty(PropertyName = "Prefix (Title) if chainsaw equiped")]
        public string NPCName2;
        [JsonProperty(PropertyName = "Health (HP)")]
        public int NPCHealth;
        [JsonProperty(PropertyName = "Life Duration (minutes)")]
        public float NPCLife;
        [JsonProperty(PropertyName = "Damage multiplier")]
        public float NPCDamageScale;
        [JsonProperty(PropertyName = "Use kit (clothing)")]
        public bool UseKit;
        [JsonProperty(PropertyName = "Kit ID")]
        public List<string> KitName;
    }

     class SCISettings
    {
        [JsonProperty(PropertyName = "Spawn chance (0-100)")]
        public int SpawnRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
        [JsonProperty(PropertyName = "Spawn Amount")]
        public int NPCAmount;
        [JsonProperty(PropertyName = "NPC Spawns on fire")]
        public bool FromHell;
        [JsonProperty(PropertyName = "Max Roam Distance")]
        public int NPCRoamMax;
        [JsonProperty(PropertyName = "Prefix (Title)")]
        public string NPCName;
        [JsonProperty(PropertyName = "Health (HP)")]
        public int NPCHealth;
        [JsonProperty(PropertyName = "Life Duration (minutes)")]
        public float NPCLife;
        [JsonProperty(PropertyName = "Damage multiplier")]
        public float NPCDamageScale;
        [JsonProperty(PropertyName = "Use kit (clothing)")]
        public bool UseKit;
        [JsonProperty(PropertyName = "Kit ID")]
        public List<string> KitName;
    }

     class SettingsAnimal
    {
        [JsonProperty(PropertyName = "Bear Settings")]
        public BearSettings Bear;
        [JsonProperty(PropertyName = "PolarBear Settings")]
        public PBearSettings PBear;
        [JsonProperty(PropertyName = "Boar Settings")]
        public BoarSettings Boar;
        [JsonProperty(PropertyName = "Chicken Settings")]
        public ChickenSettings Chicken;
        [JsonProperty(PropertyName = "Wolf Settings")]
        public WolfSettings Wolf;
    }

     class BearSettings
    {
        [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
        public int BearRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
        [JsonProperty(PropertyName = "Amount")]
        public int BearAmount;
        [JsonProperty(PropertyName = "Health")]
        public int BearHealth;
        [JsonProperty(PropertyName = "Life Duration (minutes)")]
        public float Life;
    }

     class PBearSettings
    {
        [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
        public int PBearRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
        [JsonProperty(PropertyName = "Amount")]
        public int PBearAmount;
        [JsonProperty(PropertyName = "Health")]
        public int PBearHealth;
        [JsonProperty(PropertyName = "Life Duration (minutes)")]
        public float Life;
    }

     class BoarSettings
    {
        [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
        public int BoarRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
        [JsonProperty(PropertyName = "Amount")]
        public int BoarAmount;
        [JsonProperty(PropertyName = "Health")]
        public int BoarHealth;
        [JsonProperty(PropertyName = "Life Duration (minutes)")]
        public float Life;
    }

     class ChickenSettings
    {
        [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
        public int ChickenRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
        [JsonProperty(PropertyName = "Amount")]
        public int ChickenAmount;
        [JsonProperty(PropertyName = "Health")]
        public int ChickenHealth;
        [JsonProperty(PropertyName = "Life Duration (minutes)")]
        public float Life;
    }

     class WolfSettings
    {
        [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
        public int WolfRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
        [JsonProperty(PropertyName = "Amount")]
        public int WolfAmount;
        [JsonProperty(PropertyName = "Health")]
        public int WolfHealth;
        [JsonProperty(PropertyName = "Life Duration (minutes)")]
        public float Life;
    }

     class SettingsExplosives
    {
        [JsonProperty(PropertyName = "Beancan : Chance on spawn (0-100)")]
        public int BeancanRate;
        [JsonProperty(PropertyName = "F1 Grenade : Chance on spawn (0-100)")]
        public int GrenadeRate;
        [JsonProperty(PropertyName = "Satchel Charge : Chance on spawn (0-100)")]
        public int SatchelRate;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
    }

     class SettingsFire
    {
        [JsonProperty(PropertyName = "Small fire : Chance on spawn (0-100)")]
        public int SmallfireRate;
        [JsonProperty(PropertyName = "Oil fire : Chance on spawn (0-100)")]
        public int OilfireRate;
        [JsonProperty(PropertyName = "Oil fire : Duration (max 20s)")]
        public float Duration;
        [JsonProperty(PropertyName = "Normal Barrels")]
        public bool Normal;
        [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
        public bool Diesel;
        [JsonProperty(PropertyName = "Oil Barrels")]
        public bool Oil;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConf();
    protected override void LoadDefaultMessages();
     void OnServerInitialized();
     void Unload();
     void Init();
     void OnEntityDeath(LootContainer entity, HitInfo info);
     object OnNpcKits(BasePlayer player);
    private object OnNpcTarget(BaseEntity attacker, BaseEntity target);
    private object OnNpcTarget(BaseEntity attacker, BasePlayer target);
    private void OnFireBallDamage(FireBall fire, ScarecrowNPC npc, HitInfo info);
    private void RunFX(BasePlayer player);
    public Vector3 GetNavPoint(Vector3 position);
    private bool SpawnRate(int npcRate);
    private bool CheckPlayer(HitInfo info);
    private bool IsLootBarrel(BaseCombatEntity entity);
    private bool IsOilBarrel(BaseCombatEntity entity);
    private bool IsDiesel(BaseCombatEntity entity);
    private bool IsNormalBarrel(BaseCombatEntity entity);
    private void Spawnnpc(Vector3 position);
    private void SpawnScientist(Vector3 position);
    private void SpawnSupplyCrate(string prefab, Vector3 position);
    private void SpawnHackCrate(string prefab, Vector3 position);
    private void SpawnAnimal(Vector3 position);
    private void SpawnThrowable(string prefab, Vector3 position);
    private void SpawnFire(string prefab, Vector3 position, float Killtime);
    private string msg(string key, string id);
     bool HasPerm(BasePlayer player, string perm);
     Playerinfo get_user(BasePlayer player);
     void update_user(BasePlayer player, Playerinfo user);
    public class Playerinfo
    {
        private string _userName;
        private int _barrelCount;
        public Playerinfo();
        public int barrelCount { get; set; }
        public string userName { get; set; }
    }

}

private class Zombies : FacepunchBehaviour
{
    private ScarecrowNPC npc;
    public bool ReturningToHome;
    public bool isRoaming;
    public Vector3 SpawnPoint;
    private void Awake();
    private void Attack();
    public void _UseBrain();
     void GoHome();
    private void SettargetDestination(Vector3 position);
     void WipeMemory();
     void OnDestroy();
}

public class Scientists : FacepunchBehaviour
{
    public global::HumanNPC npc;
    public bool ReturningToHome;
    public bool isRoaming;
    public Vector3 SpawnPoint;
     void Start();
    public void _UseBrain();
     void GoHome();
    private void SettargetDestination(Vector3 position);
     void WipeMemory();
     void OnDestroy();
}

 class ConfigData
{
    [JsonProperty(PropertyName = "Plugin Settings")]
    public SettingsDrop DropData;
    [JsonProperty(PropertyName = "Airdrop Settings")]
    public SettingsAirdrop AirdropData;
    [JsonProperty(PropertyName = "Hack crate Settings")]
    public SettingsHack HackData;
    [JsonProperty(PropertyName = "Scarecrow Settings")]
    public NPCSettings CrowData;
    [JsonProperty(PropertyName = "Scientist Settings")]
    public SCISettings SCIData;
    [JsonProperty(PropertyName = "Animal Settings")]
    public SettingsAnimal AnimalData;
    [JsonProperty(PropertyName = "Explosives Settings")]
    public SettingsExplosives ExplosivesData;
    [JsonProperty(PropertyName = "Fire Settings")]
    public SettingsFire FireData;
}

 class SettingsDrop
{
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string Prefix;
    [JsonProperty(PropertyName = "Drop : Count random per x barrels")]
    public int Barrelcountdrop { get; set; }
    [JsonProperty(PropertyName = "Drop : Max range to trigger events")]
    public int RangeTrigger { get; set; }
    [JsonProperty(PropertyName = "Drop : Spawn only 1 entity on trigger")]
    public bool Trigger;
    [JsonProperty(PropertyName = "Only allow spawning outside")]
    public bool TriggerOut;
    [JsonProperty(PropertyName = "Show messages")]
    public bool ShowMsg;
    [JsonProperty(PropertyName = "FX on trigger")]
    public string FX;
}

 class SettingsHack
{
    [JsonProperty(PropertyName = "Spawn chance (0-100)")]
    public int HackdropRate { get; set; }
    [JsonProperty(PropertyName = "Drop height")]
    public int HackdropHeight { get; set; }
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
}

 class SettingsAirdrop
{
    [JsonProperty(PropertyName = "Spawn chance (0-100)")]
    public int AirdropRate { get; set; }
    [JsonProperty(PropertyName = "Drop height")]
    public int AirdropHeight { get; set; }
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
}

 class NPCSettings
{
    [JsonProperty(PropertyName = "Spawn chance (0-100)")]
    public int SpawnRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
    [JsonProperty(PropertyName = "Spawn Amount")]
    public int NPCAmount;
    [JsonProperty(PropertyName = "NPC Spawns on fire")]
    public bool FromHell;
    [JsonProperty(PropertyName = "Max Roam Distance")]
    public int NPCRoamMax;
    [JsonProperty(PropertyName = "Prefix (Title)")]
    public string NPCName;
    [JsonProperty(PropertyName = "Prefix (Title) if chainsaw equiped")]
    public string NPCName2;
    [JsonProperty(PropertyName = "Health (HP)")]
    public int NPCHealth;
    [JsonProperty(PropertyName = "Life Duration (minutes)")]
    public float NPCLife;
    [JsonProperty(PropertyName = "Damage multiplier")]
    public float NPCDamageScale;
    [JsonProperty(PropertyName = "Use kit (clothing)")]
    public bool UseKit;
    [JsonProperty(PropertyName = "Kit ID")]
    public List<string> KitName;
}

 class SCISettings
{
    [JsonProperty(PropertyName = "Spawn chance (0-100)")]
    public int SpawnRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
    [JsonProperty(PropertyName = "Spawn Amount")]
    public int NPCAmount;
    [JsonProperty(PropertyName = "NPC Spawns on fire")]
    public bool FromHell;
    [JsonProperty(PropertyName = "Max Roam Distance")]
    public int NPCRoamMax;
    [JsonProperty(PropertyName = "Prefix (Title)")]
    public string NPCName;
    [JsonProperty(PropertyName = "Health (HP)")]
    public int NPCHealth;
    [JsonProperty(PropertyName = "Life Duration (minutes)")]
    public float NPCLife;
    [JsonProperty(PropertyName = "Damage multiplier")]
    public float NPCDamageScale;
    [JsonProperty(PropertyName = "Use kit (clothing)")]
    public bool UseKit;
    [JsonProperty(PropertyName = "Kit ID")]
    public List<string> KitName;
}

 class SettingsAnimal
{
    [JsonProperty(PropertyName = "Bear Settings")]
    public BearSettings Bear;
    [JsonProperty(PropertyName = "PolarBear Settings")]
    public PBearSettings PBear;
    [JsonProperty(PropertyName = "Boar Settings")]
    public BoarSettings Boar;
    [JsonProperty(PropertyName = "Chicken Settings")]
    public ChickenSettings Chicken;
    [JsonProperty(PropertyName = "Wolf Settings")]
    public WolfSettings Wolf;
}

 class BearSettings
{
    [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
    public int BearRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
    [JsonProperty(PropertyName = "Amount")]
    public int BearAmount;
    [JsonProperty(PropertyName = "Health")]
    public int BearHealth;
    [JsonProperty(PropertyName = "Life Duration (minutes)")]
    public float Life;
}

 class PBearSettings
{
    [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
    public int PBearRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
    [JsonProperty(PropertyName = "Amount")]
    public int PBearAmount;
    [JsonProperty(PropertyName = "Health")]
    public int PBearHealth;
    [JsonProperty(PropertyName = "Life Duration (minutes)")]
    public float Life;
}

 class BoarSettings
{
    [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
    public int BoarRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
    [JsonProperty(PropertyName = "Amount")]
    public int BoarAmount;
    [JsonProperty(PropertyName = "Health")]
    public int BoarHealth;
    [JsonProperty(PropertyName = "Life Duration (minutes)")]
    public float Life;
}

 class ChickenSettings
{
    [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
    public int ChickenRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
    [JsonProperty(PropertyName = "Amount")]
    public int ChickenAmount;
    [JsonProperty(PropertyName = "Health")]
    public int ChickenHealth;
    [JsonProperty(PropertyName = "Life Duration (minutes)")]
    public float Life;
}

 class WolfSettings
{
    [JsonProperty(PropertyName = "Chance on spawn (0-100)")]
    public int WolfRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
    [JsonProperty(PropertyName = "Amount")]
    public int WolfAmount;
    [JsonProperty(PropertyName = "Health")]
    public int WolfHealth;
    [JsonProperty(PropertyName = "Life Duration (minutes)")]
    public float Life;
}

 class SettingsExplosives
{
    [JsonProperty(PropertyName = "Beancan : Chance on spawn (0-100)")]
    public int BeancanRate;
    [JsonProperty(PropertyName = "F1 Grenade : Chance on spawn (0-100)")]
    public int GrenadeRate;
    [JsonProperty(PropertyName = "Satchel Charge : Chance on spawn (0-100)")]
    public int SatchelRate;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
}

 class SettingsFire
{
    [JsonProperty(PropertyName = "Small fire : Chance on spawn (0-100)")]
    public int SmallfireRate;
    [JsonProperty(PropertyName = "Oil fire : Chance on spawn (0-100)")]
    public int OilfireRate;
    [JsonProperty(PropertyName = "Oil fire : Duration (max 20s)")]
    public float Duration;
    [JsonProperty(PropertyName = "Normal Barrels")]
    public bool Normal;
    [JsonProperty(PropertyName = "Diesel Barrels (diesel_barrel_world)")]
    public bool Diesel;
    [JsonProperty(PropertyName = "Oil Barrels")]
    public bool Oil;
}

public class Playerinfo
{
    private string _userName;
    private int _barrelCount;
    public Playerinfo();
    public int barrelCount { get; set; }
    public string userName { get; set; }
}


```

---

## BarrelPoints by  - Gives players extra rp/eco/scrap or Battlepass points for destroying barrels and crates

```csharp
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;

Oxide.Plugins
[Info("Barrel Points", "Krungh Crow", "2.3.1")]
[Description("Gives players extra rewards for destroying barrels")]
public class BarrelPoints : RustPlugin
{
    [PluginReference]
     Plugin Battlepass;
     Plugin Economics;
     Plugin ServerRewards;
    const ulong chaticon;
    const string prefix;
    private static Dictionary<string, object> _PermissionDic();
    private readonly Dictionary<string, int> playerInfo;
    private readonly List<ulong> crateCache;
    private Dictionary<string, object> permissionList;
    private bool changed;
    private bool useEconomy;
    private bool useServerRewards;
    private bool useBattlepass;
    private bool useBattlepass1;
    private bool useBattlepass2;
    private bool useItem;
    private string Itemshortname;
    private bool resetBarrelsOnDeath;
    private bool sendNotificationMessage;
    private bool useCrates;
    private bool useBarrels;
    private int givePointsEvery;
    private void OnServerInitialized();
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private new void LoadDefaultMessages();
    private void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    private void OnEntityKill(BaseNetworkable entity);
    private void OnLootEntity(BasePlayer player, BaseEntity entity);
    private void OnPlayerDeath(BasePlayer player, HitInfo hitInfo);
    private string GetPermissionName(BasePlayer player);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string Msg(string key, string id);
}


```

---

## BarrenPlus by misticos - Lets JunkPiles and DiveSites be alive on Barren

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;
using Random = System.Random;

Oxide.Plugins
[Info("Barren Plus", "Iv Misticos", "1.0.6")]
[Description("Let JunkPiles and DiveSites be alive on Barren!")]
 class BarrenPlus : RustPlugin
{
    private int _size;
    private readonly string[] _prefabsJunkPile;
    private readonly string[] _prefabsDiveSite;
    private static readonly Random Random;
    private static Configuration _config;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Junk Pile Spawn Frequency")]
        public float JunkPileTime;
        [JsonProperty(PropertyName = "Dive Site Spawn Frequency")]
        public float DiveSiteTime;
        [JsonProperty(PropertyName = "Minimal Distance Between Junk Piles")]
        public int JunkPileRange;
        [JsonProperty(PropertyName = "Minimal Distance Between Dive Sites")]
        public int DiveSiteRange;
        [JsonProperty(PropertyName = "Minimal Distance Between Water And Terrain")]
        public int BetweenWaterTerrain;
        [JsonProperty(PropertyName = "Maximal Dive Site Angle")]
        public int AngleMax;
        [JsonProperty(PropertyName = "Dive Sites' Lifetime")]
        public int DiveSiteLife;
        [JsonProperty(PropertyName = "Junk Piles' Lifetime")]
        public int JunkPileLife;
        [JsonProperty(PropertyName = "Maximum Number Of Attempts To Find A Location")]
        public int LocAttemptsMax;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void OnServerInitialized();
    private Vector3? GetPosition(bool onWater);
    private Quaternion GetRotation(bool isJunkPile);
    private bool SpawnDiveSite();
    private bool SpawnJunkPileWater();
    public class DiveSiteController : MonoBehaviour
    {
        private float _lastTime;
        private DiveSite _ent;
        private bool _isDestroyed;
        private void Awake();
        private void FixedUpdate();
    }

    public class JunkPileWaterController : MonoBehaviour
    {
        private float _lastTime;
        private JunkPileWater _ent;
        private bool _isDestroyed;
        private void Awake();
        private void FixedUpdate();
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "Junk Pile Spawn Frequency")]
    public float JunkPileTime;
    [JsonProperty(PropertyName = "Dive Site Spawn Frequency")]
    public float DiveSiteTime;
    [JsonProperty(PropertyName = "Minimal Distance Between Junk Piles")]
    public int JunkPileRange;
    [JsonProperty(PropertyName = "Minimal Distance Between Dive Sites")]
    public int DiveSiteRange;
    [JsonProperty(PropertyName = "Minimal Distance Between Water And Terrain")]
    public int BetweenWaterTerrain;
    [JsonProperty(PropertyName = "Maximal Dive Site Angle")]
    public int AngleMax;
    [JsonProperty(PropertyName = "Dive Sites' Lifetime")]
    public int DiveSiteLife;
    [JsonProperty(PropertyName = "Junk Piles' Lifetime")]
    public int JunkPileLife;
    [JsonProperty(PropertyName = "Maximum Number Of Attempts To Find A Location")]
    public int LocAttemptsMax;
}

public class DiveSiteController : MonoBehaviour
{
    private float _lastTime;
    private DiveSite _ent;
    private bool _isDestroyed;
    private void Awake();
    private void FixedUpdate();
}

public class JunkPileWaterController : MonoBehaviour
{
    private float _lastTime;
    private JunkPileWater _ent;
    private bool _isDestroyed;
    private void Awake();
    private void FixedUpdate();
}


```

---

## Barricades by  - Legacy wooden barricade made out of double stacked sign posts

```csharp
using Oxide.Core;
using Oxide.Game.Rust.Cui;
using System;
using System.Linq;
using System.Collections.Generic;
using UnityEngine;
using Rust;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Barricades", "Xianith / redBDGR", "0.0.3", ResourceId = 2460)]
[Description("Legacy wooden barricade made out of double stacked sign posts. Can be picked up.")]
 class Barricades : RustPlugin
{
    static Barricades bCades;
     Dictionary<BasePlayer, BaseEntity> barricaders;
     Dictionary<string, BarricadeGui> BarricadeGUIinfo;
     class BarricadeGui
    {
        public string panel;
        public BaseEntity entity;
    }

    public bConfig Settings { get; set; }
    public class bConfig
    {
        [JsonProperty(PropertyName = "Initial Health of a placed Barricade")]
        public int Health { get; set; }
        [JsonProperty(PropertyName = "Can be picked up")]
        public bool CanBePicked { get; set; }
        public static bConfig DefaultConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void Unload();
     void Init();
     void OnPlayerInput(BasePlayer player, InputState input);
     void OnEntitySpawned(BaseNetworkable entityn);
     void decay(BaseEntity ent);
     class BarricadeRadius : MonoBehaviour
    {
         BaseEntity entity;
        public bool isEnabled;
        private void Awake();
        private void OnTriggerEnter(Collider col);
        private void OnTriggerExit(Collider col);
    }

     void CreateBarrUi(BasePlayer player, BaseEntity entity);
     void DestroyBarrUi(BasePlayer player);
     void createBarricade(BasePlayer player);
    [ConsoleCommand("barricade.add")]
     void cmdBarricadeAdd(ConsoleSystem.Arg arg);
    [ConsoleCommand("barricade.buy")]
     void cmdBarricadeBuy(ConsoleSystem.Arg arg);
     void LoadDefaultMessages();
}

 class BarricadeGui
{
    public string panel;
    public BaseEntity entity;
}

public class bConfig
{
    [JsonProperty(PropertyName = "Initial Health of a placed Barricade")]
    public int Health { get; set; }
    [JsonProperty(PropertyName = "Can be picked up")]
    public bool CanBePicked { get; set; }
    public static bConfig DefaultConfig();
}

 class BarricadeRadius : MonoBehaviour
{
     BaseEntity entity;
    public bool isEnabled;
    private void Awake();
    private void OnTriggerEnter(Collider col);
    private void OnTriggerExit(Collider col);
}


```

---

## BaseRepair by MJSU - Allows players to repair their entire base

```csharp
using System;
using System.Collections;
using System.Collections.Generic;
using System.ComponentModel;
using System.Text;
using Facepunch;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using ProtoBuf;
using UnityEngine;

[Info("Base Repair", "MJSU", "1.0.27")]
[Description("Allows player to repair their entire base")]
internal class BaseRepair : RustPlugin
{
    [PluginReference]
    private Plugin NoEscape;
    private Plugin RaidBlock;
    private StoredData _storedData;
    private PluginConfig _pluginConfig;
    private const string UsePermission;
    private const string NoCostPermission;
    private const string NoAuthPermission;
    private const string AccentColor;
    private readonly List<ulong> _repairingPlayers;
    private readonly ItemAmountPool _itemAmountPool;
    private readonly StringBuilder _sb;
    private GameObject _go;
    private RepairBehavior _rb;
    private readonly object _true;
    private void Init();
    protected override void LoadDefaultMessages();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    private PluginConfig AdditionalConfig(PluginConfig config);
    private void OnServerInitialized();
    private void Unload();
    private void BaseRepairChatCommand(BasePlayer player, string cmd, string[] args);
    private object OnHammerHit(BasePlayer player, HitInfo info);
    public bool CanRepair(BasePlayer player);
    public bool IsRepairBlocked(BasePlayer player);
    private IEnumerator DoBuildingRepair(BasePlayer player, BuildingManager.Building building, PlayerRepairStats stats);
    private void DoRepair(BasePlayer player, BaseCombatEntity entity, PlayerRepairStats stats, bool noCost);
    public bool CanAffordRepair(BasePlayer player, List<ItemAmount> amounts);
    public void GetEntityRepairCost(BaseCombatEntity entity, List<ItemAmount> repairAmounts, float missingHealthFraction);
    public bool HasRepairCost(List<ItemAmount> amounts);
    public void FreeItemAmounts(List<ItemAmount> amounts);
    public void SendMissingItemAmounts(BasePlayer player, List<ItemAmount> itemAmounts);
    public void SubscribeAll();
    public void UnsubscribeAll();
    public bool IsPluginLoaded(Plugin plugin);
    private void SaveData();
    private void Chat(BasePlayer player, string format);
    private bool HasPermission(BasePlayer player, string perm);
    private string Lang(string key, BasePlayer player);
    private string Lang(string key, BasePlayer player, object[] args);
    private class RepairBehavior : FacepunchBehaviour
    {
        private void Awake();
        public void DoDestroy();
    }

    private class PluginConfig
    {
        [DefaultValue(10)]
        [JsonProperty(PropertyName = "Number of entities to repair per server frame")]
        public int RepairsPerFrame { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Default Enabled")]
        public bool DefaultEnabled { get; set; }
        [DefaultValue(false)]
        [JsonProperty(PropertyName = "Allow Repairing Bases Without A Tool Cupboard")]
        public bool AllowNoTcRepair { get; set; }
        [DefaultValue(1f)]
        [JsonProperty(PropertyName = "Repair Cost Multiplier")]
        public float RepairCostMultiplier { get; set; }
        [DefaultValue(30f)]
        [JsonProperty(PropertyName = "How long after an entity is damaged before it can be repaired (Seconds)")]
        public float EntityRepairDelay { get; set; }
        [JsonProperty(PropertyName = "Chat Commands")]
        public List<string> ChatCommands { get; set; }
        [JsonProperty(PropertyName = "Enable Repairs Using A Skinned Hammer")]
        public bool EnableHammerSkin { get; set; }
        [DefaultValue(2902701361)]
        [JsonProperty(PropertyName = "Repair Hammer Skin ID")]
        public ulong HammerSkinId { get; set; }
    }

    private class StoredData
    {
        public Hash<ulong, bool> RepairEnabled;
    }

    private class PlayerRepairStats
    {
        public int TotalSuccess { get; set; }
        public int TotalCantAfford { get; set; }
        public int RecentlyDamaged { get; set; }
        public Hash<int, ItemAmount> MissingAmounts { get; set; }
        public Hash<int, int> AmountTaken { get; set; }
    }

    private class LangKeys
    {
        public const string Chat;
        public const string NoPermission;
        public const string RepairInProcess;
        public const string RecentlyDamaged;
        public const string AmountRepaired;
        public const string Enabled;
        public const string Disabled;
        public const string RaidBlockPluginBlocked;
    }

    private class BasePool
    {
        protected readonly List<T> Pool;
        protected readonly Func<T> Init;
        public BasePool(Func<T> init);
        public virtual T Get();
        public virtual void Free(T entity);
    }

    private class ItemAmountPool : BasePool<ItemAmount>
    {
        public ItemAmountPool();
        public override void Free(ItemAmount ia);
    }

}

private class RepairBehavior : FacepunchBehaviour
{
    private void Awake();
    public void DoDestroy();
}

private class PluginConfig
{
    [DefaultValue(10)]
    [JsonProperty(PropertyName = "Number of entities to repair per server frame")]
    public int RepairsPerFrame { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Default Enabled")]
    public bool DefaultEnabled { get; set; }
    [DefaultValue(false)]
    [JsonProperty(PropertyName = "Allow Repairing Bases Without A Tool Cupboard")]
    public bool AllowNoTcRepair { get; set; }
    [DefaultValue(1f)]
    [JsonProperty(PropertyName = "Repair Cost Multiplier")]
    public float RepairCostMultiplier { get; set; }
    [DefaultValue(30f)]
    [JsonProperty(PropertyName = "How long after an entity is damaged before it can be repaired (Seconds)")]
    public float EntityRepairDelay { get; set; }
    [JsonProperty(PropertyName = "Chat Commands")]
    public List<string> ChatCommands { get; set; }
    [JsonProperty(PropertyName = "Enable Repairs Using A Skinned Hammer")]
    public bool EnableHammerSkin { get; set; }
    [DefaultValue(2902701361)]
    [JsonProperty(PropertyName = "Repair Hammer Skin ID")]
    public ulong HammerSkinId { get; set; }
}

private class StoredData
{
    public Hash<ulong, bool> RepairEnabled;
}

private class PlayerRepairStats
{
    public int TotalSuccess { get; set; }
    public int TotalCantAfford { get; set; }
    public int RecentlyDamaged { get; set; }
    public Hash<int, ItemAmount> MissingAmounts { get; set; }
    public Hash<int, int> AmountTaken { get; set; }
}

private class LangKeys
{
    public const string Chat;
    public const string NoPermission;
    public const string RepairInProcess;
    public const string RecentlyDamaged;
    public const string AmountRepaired;
    public const string Enabled;
    public const string Disabled;
    public const string RaidBlockPluginBlocked;
}

private class BasePool
{
    protected readonly List<T> Pool;
    protected readonly Func<T> Init;
    public BasePool(Func<T> init);
    public virtual T Get();
    public virtual void Free(T entity);
}

private class ItemAmountPool : BasePool<ItemAmount>
{
    public ItemAmountPool();
    public override void Free(ItemAmount ia);
}


```

---

## Battlefield by VisEntities - Free for All, Open Arena, Vote Weapons, Vote Grounds

```csharp
using System.Collections.Generic;
using System;
using System.Data;
using UnityEngine;
using Oxide.Core;
using Oxide.Core.Plugins;
using System.Timers;
using Rust;

Oxide.Plugins
[Info("Event Battlefield", "Reneb", "1.0.4")]
 class Battlefield : RustPlugin
{
    [PluginReference]
     Plugin EventManager;
    [PluginReference]
     Plugin ZoneManager;
    private bool useThisEvent;
    private bool EventStarted;
    private bool Changed;
    private List<BattlefieldPlayer> BattlefieldPlayers;
    private Hash<BattlefieldPlayer, string> WeaponVote;
    private Hash<BattlefieldPlayer, string> GroundVote;
    private string currentGround;
    private string currentWeapon;
     class BattlefieldPlayer : MonoBehaviour
    {
        public BasePlayer player;
        public int kills;
         void Awake();
    }

     void Loaded();
     void OnServerInitialized();
     void RegisterGame();
     void LoadDefaultConfig();
     void Unload();
    static string EventName;
    static float EventStartHealth;
    static Dictionary<string, object> EventGrounds;
    static string DefaultGround;
    static string DefaultWeapon;
    static string EventMessageKill;
    static string EventMessageOpenBroadcast;
    static string EventMessageErrorNotLaunched;
    static string EventMessageErrorNotStarted;
    static string EventMessageErrorVoteNotBF;
    static string EventMessageVoteGroundAvaible;
    static string EventMessageVoteGroundVotes;
    static string EventMessageErrorVoteNoGround;
    static string EventMessageErrorVoteAlreadyGround;
    static string EventMessageVoteGroundVoted;
    static string EventMessageVoteGroundShowVotes;
    static string EventMessageVoteGroundNew;
    static string EventMessageVoteWeaponNew;
    static string EventMessageVoteWeaponShowVotes;
    static string EventMessageVoteWeaponAvaible;
    static string EventMessageVoteWeaponVotes;
    static string EventMessageErrorVoteNoKit;
    static string EventMessageErrorVoteAlreadyWeapon;
    static string EventMessageVoteWeaponVoted;
    static int TokensAddKill;
    static int EventVotePercent;
    private void LoadVariables();
    private void LoadConfigVariables();
    private void CheckCfg(string Key, T var);
    private void CheckCfgFloat(string Key, float var);
    static Dictionary<string,object> DefaultGrounds();
     object GetConfig(string menu, string datavalue, object defaultValue);
     object GetEventConfig(string configname);
     void OnSelectEventGamePost(string name);
     void OnEventPlayerSpawn(BasePlayer player);
     object OnSelectSpawnFile(string name);
     object OnSelectKit(string kitname);
     object OnEventOpenPost();
     object OnEventEndPost();
     object OnRequestZoneName();
     object OnEventStartPre();
     object OnEventJoinPost(BasePlayer player);
     object OnEventLeavePost(BasePlayer player);
     void OnEventPlayerDeath(BasePlayer victim, HitInfo hitinfo);
     void AddKill(BasePlayer player, BasePlayer victim);
     int GetGroundVotes(string groundname);
     int GetWeaponVotes(string weaponname);
     int EventPlayersCount();
     int VotePlayersNeeded();
     void ResetVotes();
     bool hasAccess(BasePlayer player);
    [ChatCommand("ground")]
     void cmdChatGround(BasePlayer player, string command, string[] args);
     void CheckGroundVotes();
     void SetGround(string newGround);
     void SetWeapon(string newWeapon);
     void CheckWeaponVotes();
    [ChatCommand("weapon")]
     void cmdChatWeapon(BasePlayer player, string command, string[] args);
}

 class BattlefieldPlayer : MonoBehaviour
{
    public BasePlayer player;
    public int kills;
     void Awake();
}


```

---

## Bearrels by ichaleynbin - Random chance of bears spawning when a barrel breaks

```csharp
using System;
using UnityEngine;
using System.Collections.Generic;

Oxide.Plugins
[Info("Bearrels", "redBDGR", "2.0.0")]
[Description("Random chance of bears spawning when a barrel breaks")]
 class Bearrels : RustPlugin
{
    private bool Changed;
    private class ConfigFile
    {
        public static float chanceOfBear;
    }

    private void LoadVariables();
     void Init();
     void OnEntityDeath(BaseCombatEntity entity, HitInfo info);
    protected override void LoadDefaultConfig();
    private void SpawnBear(Vector3 pos);
    private object GetConfig(string menu, string datavalue, object defaultValue);
    private string msg(string key, string id);
}

private class ConfigFile
{
    public static float chanceOfBear;
}


```

---

## BedNameLogs by zeeuss - A logger to log all renames for beds & sleepingbags into a folder.

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Newtonsoft.Json;
using Oxide.Core.Libraries;

Oxide.Plugins
[Info("Bed Name Logs", "Zeeuss", "0.1.3")]
[Description("A logger for beds and sleeping bags renames.")]
public class BedNameLogs : RustPlugin
{
    protected override void LoadDefaultMessages();
    private const string DiscordJson;
     void CanRenameBed(BasePlayer player, SleepingBag bed, string bedName);
    private ConfigData configData;
     class ConfigData
    {
        [JsonProperty(PropertyName = "log to discord?")]
        public bool dcLogs;
        [JsonProperty(PropertyName = "Discord webhook url")]
        public string discordWebHookURL;
    }

    private bool LoadConfigVariables();
    protected override void LoadDefaultConfig();
     void SaveConfig(ConfigData config);
    private void logToDc(BasePlayer player, string xyzpos, string playerName, string playerID, string bedPrevious, string bedCurrent, string langM);
}

 class ConfigData
{
    [JsonProperty(PropertyName = "log to discord?")]
    public bool dcLogs;
    [JsonProperty(PropertyName = "Discord webhook url")]
    public string discordWebHookURL;
}


```

---

## BedRenameBlocker by MONaH - Prevents players from renaming beds / sleeping bags they do not own.

```csharp
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core.Plugins;
using System;

Oxide.Plugins
[Info("Bed Rename Blocker", "MON@H", "2.0.1")]
[Description("Blocks people of renaming a bed/sleeping bag")]
 class BedRenameBlocker : RustPlugin
{
    [PluginReference]
    private Plugin Clans;
    private Plugin Friends;
    private const string PermissionImmunity;
    private void Init();
    private void OnServerInitialized();
    private ConfigData _configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Block only bags owned by the players (false = block all)")]
        public bool PlayerOwnedOnly;
        [JsonProperty(PropertyName = "Use Clans")]
        public bool UseClans;
        [JsonProperty(PropertyName = "Use Friends")]
        public bool UseFriends;
        [JsonProperty(PropertyName = "Use Teams")]
        public bool UseTeams;
        [JsonProperty(PropertyName = "Chat steamID icon")]
        public ulong SteamIDIcon;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private string Lang(string key, string userIDString, object[] args);
    private static class LangKeys
    {
        public static class Error
        {
            private const string Base;
            public const string NoPermission;
        }

        public static class Format
        {
            private const string Base;
            public const string Prefix;
        }

    }

    protected override void LoadDefaultMessages();
    private object CanRenameBed(BasePlayer player, SleepingBag bed, string bedName);
    private bool IsPluginLoaded(Plugin plugin);
    public bool IsAlly(ulong playerId, ulong targetId);
    public bool IsClanMemberOrAlly(string playerId, string targetId);
    public bool IsFriend(ulong playerId, ulong targetId);
    public bool IsOnSameTeam(ulong playerId, ulong targetId);
    private void PlayerSendMessage(BasePlayer player, string message);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Block only bags owned by the players (false = block all)")]
    public bool PlayerOwnedOnly;
    [JsonProperty(PropertyName = "Use Clans")]
    public bool UseClans;
    [JsonProperty(PropertyName = "Use Friends")]
    public bool UseFriends;
    [JsonProperty(PropertyName = "Use Teams")]
    public bool UseTeams;
    [JsonProperty(PropertyName = "Chat steamID icon")]
    public ulong SteamIDIcon;
}

private static class LangKeys
{
    public static class Error
    {
        private const string Base;
        public const string NoPermission;
    }

    public static class Format
    {
        private const string Base;
        public const string Prefix;
    }

}

public static class Error
{
    private const string Base;
    public const string NoPermission;
}

public static class Format
{
    private const string Base;
    public const string Prefix;
}


```

---

## BedsCooldowns by Mabel - Allows changing cooldowns for respawns on bags and beds

```csharp
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;
using Oxide.Core;
using UnityEngine;

Oxide.Plugins
[Info("Beds Cooldowns", "Orange", "1.1.4")]
[Description("Allows to change cooldowns for respawns on bags and beds")]
public class BedsCooldowns : RustPlugin
{
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(SleepingBag entity);
    private void OnPlayerConnected(BasePlayer player);
    private void CheckPlayer(BasePlayer player);
    private void SetCooldown(SleepingBag entity, SettingsEntry info);
    private SettingsEntry GetSettings(string playerID);
    private IEnumerator CheckBags(ulong playerID, SettingsEntry settings);
    private static ConfigData config;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "List")]
        public List<SettingsEntry> list;
    }

    private class SettingsEntry
    {
        [JsonProperty(PropertyName = "Permission")]
        public string perm;
        [JsonProperty(PropertyName = "Priority")]
        public int priority;
        [JsonProperty(PropertyName = "Sleeping bag cooldown")]
        public float bag;
        [JsonProperty(PropertyName = "Bed cooldown")]
        public float bed;
        [JsonProperty(PropertyName = "Sleeping bag unlock time")]
        public float unlockTimeBag;
        [JsonProperty(PropertyName = "Bed unlock time")]
        public float unlockTimeBed;
    }

    private ConfigData GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "List")]
    public List<SettingsEntry> list;
}

private class SettingsEntry
{
    [JsonProperty(PropertyName = "Permission")]
    public string perm;
    [JsonProperty(PropertyName = "Priority")]
    public int priority;
    [JsonProperty(PropertyName = "Sleeping bag cooldown")]
    public float bag;
    [JsonProperty(PropertyName = "Bed cooldown")]
    public float bed;
    [JsonProperty(PropertyName = "Sleeping bag unlock time")]
    public float unlockTimeBag;
    [JsonProperty(PropertyName = "Bed unlock time")]
    public float unlockTimeBed;
}


```

---

## BedShare by VisEntities - Allow beds to be shared with other players

```csharp
using System.Collections.Generic;
using Facepunch;
using ProtoBuf;
using Facepunch.Math;
using UnityEngine;
using System;
using Newtonsoft.Json;
using System.Linq;
using Oxide.Game.Rust.Cui;

Oxide.Plugins
[Info("BedShare", "ignignokt84", "0.1.0")]
[Description("Bed sharing plugin")]
 class BedShare : RustPlugin
{
    static BedShare Instance;
     BagData data;
     Dictionary<ulong, string> playerNameCache;
     Dictionary<ulong, ulong> dummyBags;
    const string PermCanUse;
    const string PermNoShare;
    const string PermCanSharePublic;
    const string PermCanSharePrivate;
    const string PermCanClear;
    const string BedPrefabName;
    const string UIElementPrefix;
     List<PlayerUI> playerUIs;
    readonly Guid EmptyGuid;
    const string GUIRespawnCommand;
     void LoadDefaultMessages();
     string GetMessage(string key, string userId);
     void Loaded();
     void Unload();
     void OnServerInitialized();
     void OnServerSave();
     bool LoadDefaultConfig();
     void LoadData();
     void SaveData();
     T GetConfig(string group, string name, T value);
     bool CheckConfig();
    [ConsoleCommand(GUIRespawnCommand)]
     void BSUI_Respawn(ConsoleSystem.Arg arg);
     bool CheckGUID(ConsoleSystem.Arg arg);
     void CommandDelegator(BasePlayer player, string command, string[] args);
     void HandleShare(BasePlayer player, bool share, string[] args, string message, object[] opts);
     void HandleStatus(BasePlayer player, string message, object[] opts);
     void HandleClear(string message, object[] opts);
     object OnPlayerDie(BasePlayer player, HitInfo hitinfo);
     object OnPlayerRespawn(BasePlayer player);
     PlayerUI FindPlayerUI(BasePlayer player);
     void ShowGUI(BasePlayer player);
     void RefreshUI(PlayerUI ui, ulong bagId);
     void CreateRespawnButton(PlayerUI ui, ulong bagId, string bagName, int index);
     void OverlayMessageUI(PlayerUI ui, ulong id, string message, Vector2 aMin, Vector2 aMax, float displayTime);
     void DestroyGUI(BasePlayer player, bool kill);
     void DestroyAllGUI();
     bool SpawnDummyBag(SleepingBag bag, BasePlayer player, ulong bagId);
     void DestroyDummyBags(BasePlayer player);
     void DestroyAllDummyBags();
     void ValidateSharedBags();
     bool GetRaycastTarget(BasePlayer player, object closestEntity);
     string GetPlayerName(ulong userID);
    private static bool isAdmin(BasePlayer player);
    private bool hasPermission(BasePlayer player, string permname, bool allowAdmin);
     void SendMessage(BasePlayer player, string message, object[] options);
     void ShowCommands(string message, object[] opts);
     class BagData
    {
        public HashSet<SharedBagInfo> sharedBags;
        public UIConfig ui;
        public void AddOrUpdateBag(ulong bagID, ulong playerID, List<ulong> players);
        public bool RemoveOrUpdateBag(ulong bagID, List<ulong> players, bool all);
        public bool HasSharedBags();
    }

     class SharedBagInfo
    {
        public ulong bagId;
        public ulong owner;
        public bool isPublic { get; set; }
        public HashSet<ulong> sharedWith;
        public SharedBagInfo(ulong bagId, ulong owner, bool isPublic);
    }

     class UIConfig
    {
        public float buttonWidth;
        public float buttonHeight;
        public float screenMarginX;
        public float screenMarginY;
        public float verticalSpacer;
        public float iconWidth;
        public float iconPanelWidth;
        public float iconPaddingX;
        public float iconPaddingY;
        public float spawnTextPaddingY;
        public float bagNameTextPaddingY;
        public string buttonColor;
        public string bagIconColor;
        public string bagIcon;
    }

     class BagUIInfo
    {
        public ulong id;
        public string name;
        public int index;
        public string message;
    }

     class PlayerUI
    {
        internal Guid guid;
         BasePlayer _player;
        internal PlayerNameID nameId;
        internal BasePlayer Player { get; set; }
        internal ulong UserId { get; set; }
         Dictionary<ulong, List<string>> elements;
         List<BagUIInfo> bags;
        public PlayerUI(BasePlayer player);
         void TryResolvePlayer();
        internal void CreateUI(BagUIInfo bag, CuiElementContainer container);
        internal void CreateMessageUI(ulong id, CuiElementContainer container);
        internal void DestroyUI();
        internal void DestroyUI(ulong bagId);
        internal void DestroyMessageUI(ulong bagId);
        public bool IsOpen { get; set; }
        public bool IsPanelOpen(ulong bagId);
        public List<ulong> BagIds { get; set; }
        public BagUIInfo FindBagInfo(ulong bagId);
        public bool HasMessage(ulong bagId);
        public string Message(ulong bagId);
        internal void Kill();
        internal string BuildCommand(object[] command);
    }

     class UI
    {
        const string format;
        static string Format(float f);
        static string Format(Vector2 v);
        public static string FormatRGBA(Color color);
        public static string FormatHex(Color color);
        internal static string AsString(object o);
        public static CuiElementContainer CreateElementContainer(string panelName, object color, object aMin, object aMax, bool useCursor, float fadeOut, float fadeIn);
        public static CuiElementContainer CreateElementContainer(string panelName, string background, object color, object aMin, object aMax, bool useCursor, float fadeOut, float fadeIn);
        public static CuiElementContainer CreateElementContainer(string parent, string panelName, string background, object color, object aMin, object aMax, bool useCursor, float fadeOut, float fadeIn);
        public static void CreatePanel(CuiElementContainer container, string panelName, object color, object aMin, object aMax, bool cursor);
        public static void CreateLabel(CuiElementContainer container, string panelName, string text, int size, object aMin, object aMax, TextAnchor align, string font, float fadeIn);
        public static void CreateButton(CuiElementContainer container, string panelName, object color, string text, int size, object aMin, object aMax, string command, TextAnchor align);
        public static void CreateImage(CuiElementContainer container, string panelName, string png, object color, object aMin, object aMax, float fadeIn);
        public static void RenameComponents(CuiElementContainer container);
    }

}

 class BagData
{
    public HashSet<SharedBagInfo> sharedBags;
    public UIConfig ui;
    public void AddOrUpdateBag(ulong bagID, ulong playerID, List<ulong> players);
    public bool RemoveOrUpdateBag(ulong bagID, List<ulong> players, bool all);
    public bool HasSharedBags();
}

 class SharedBagInfo
{
    public ulong bagId;
    public ulong owner;
    public bool isPublic { get; set; }
    public HashSet<ulong> sharedWith;
    public SharedBagInfo(ulong bagId, ulong owner, bool isPublic);
}

 class UIConfig
{
    public float buttonWidth;
    public float buttonHeight;
    public float screenMarginX;
    public float screenMarginY;
    public float verticalSpacer;
    public float iconWidth;
    public float iconPanelWidth;
    public float iconPaddingX;
    public float iconPaddingY;
    public float spawnTextPaddingY;
    public float bagNameTextPaddingY;
    public string buttonColor;
    public string bagIconColor;
    public string bagIcon;
}

 class BagUIInfo
{
    public ulong id;
    public string name;
    public int index;
    public string message;
}

 class PlayerUI
{
    internal Guid guid;
     BasePlayer _player;
    internal PlayerNameID nameId;
    internal BasePlayer Player { get; set; }
    internal ulong UserId { get; set; }
     Dictionary<ulong, List<string>> elements;
     List<BagUIInfo> bags;
    public PlayerUI(BasePlayer player);
     void TryResolvePlayer();
    internal void CreateUI(BagUIInfo bag, CuiElementContainer container);
    internal void CreateMessageUI(ulong id, CuiElementContainer container);
    internal void DestroyUI();
    internal void DestroyUI(ulong bagId);
    internal void DestroyMessageUI(ulong bagId);
    public bool IsOpen { get; set; }
    public bool IsPanelOpen(ulong bagId);
    public List<ulong> BagIds { get; set; }
    public BagUIInfo FindBagInfo(ulong bagId);
    public bool HasMessage(ulong bagId);
    public string Message(ulong bagId);
    internal void Kill();
    internal string BuildCommand(object[] command);
}

 class UI
{
    const string format;
    static string Format(float f);
    static string Format(Vector2 v);
    public static string FormatRGBA(Color color);
    public static string FormatHex(Color color);
    internal static string AsString(object o);
    public static CuiElementContainer CreateElementContainer(string panelName, object color, object aMin, object aMax, bool useCursor, float fadeOut, float fadeIn);
    public static CuiElementContainer CreateElementContainer(string panelName, string background, object color, object aMin, object aMax, bool useCursor, float fadeOut, float fadeIn);
    public static CuiElementContainer CreateElementContainer(string parent, string panelName, string background, object color, object aMin, object aMax, bool useCursor, float fadeOut, float fadeIn);
    public static void CreatePanel(CuiElementContainer container, string panelName, object color, object aMin, object aMax, bool cursor);
    public static void CreateLabel(CuiElementContainer container, string panelName, string text, int size, object aMin, object aMax, TextAnchor align, string font, float fadeIn);
    public static void CreateButton(CuiElementContainer container, string panelName, object color, string text, int size, object aMin, object aMax, string command, TextAnchor align);
    public static void CreateImage(CuiElementContainer container, string panelName, string png, object color, object aMin, object aMax, float fadeIn);
    public static void RenameComponents(CuiElementContainer container);
}


```

---

## BedsLimit by  - Allows limiting the amount of players with beds and sleeping bags per base

```csharp
using UnityEngine;
using System.Collections.Generic;

Oxide.Plugins
[Info("Beds Limit", "Bruno Puccio", "1.1")]
[Description("allows the admin to limit the amount of beds that are placed per base")]
 class BedsLimit : RustPlugin
{
    const string perm;
    private ConfigData bedsConfig;
    private class ConfigData
    {
        public int radius;
        public int maxplayers;
    }

    private void LoadVariables();
    protected override void LoadDefaultConfig();
    private void LoadConfigVariables();
     void SaveConfig(ConfigData bedsConfig);
    private string Lang(string key, string id, object[] args);
     void Init();
    private new void LoadDefaultMessages();
     void OnEntityBuilt(Planner plan, GameObject go);
     object CanAssignBed(SleepingBag bag, BasePlayer player, ulong targetPlayerId);
     int FindBedsNearby(int i, Vector3 pos, ulong targetPlayerId);
     int FindBedsNearby(SleepingBag bag);
     List<SleepingBag> LookForBeds(Vector3 pos);
    [ChatCommand("bedslimit")]
     void VanillaCMD(BasePlayer player, string command, string[] args);
}

private class ConfigData
{
    public int radius;
    public int maxplayers;
}


```

---

## BedWelcomer by KrunghCrow - Changes the text of sleeping bags and beds on placement

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Plugins;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Facepunch;

Oxide.Plugins
[Info("Bed Welcomer", "Krungh Crow", "1.0.4")]
[Description("Changes the default text on bags towels beds and campervans")]
 class BedWelcomer : RustPlugin
{
    protected override void LoadDefaultMessages();
    private void OnEntitySpawned(SleepingBag bag);
}


```

---

## BetterAttachments by VisEntities - Allows modifying attachments attributes

```csharp
using Newtonsoft.Json;
using Oxide.Core.Configuration;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("BetterAttachments", "ignignokt84", "0.0.3", ResourceId = 2326)]
[Description("Plugin allowing for better control of weapon attachment attributes")]
 class BetterAttachments : RustPlugin
{
     AttachmentData data;
     Dictionary<string,AttachmentModifier> defaults;
     void LoadConfig();
     void SaveData();
     bool LoadDefaultConfig();
     void Loaded();
     void Unload();
     void OnEntitySpawned(BaseNetworkable entity);
     bool checkDefaults(string prefabName);
     void saveDefaults(ProjectileWeaponMod mod);
     void checkAllAttachments();
     void restoreAllAttachments();
     void modifyAttachment(ProjectileWeaponMod attachment);
     void restoreDefaults(ProjectileWeaponMod attachment);
     void updateModifier(ProjectileWeaponMod.Modifier originalMod, Modifier newMod);
     void OnLoseCondition(Item item, float amount);
     bool shouldFail(string name, float amount);
    private class AttachmentData
    {
        public Dictionary<string,AttachmentModifier> data;
        public Dictionary<string,CatastrophicFailure> failures;
    }

    private class AttachmentModifier
    {
        public string name;
        public bool enabled;
        public Modifier repeatDelay;
        public Modifier projectileVelocity;
        public Modifier projectileDamage;
        public Modifier projectileDistance;
        public Modifier aimsway;
        public Modifier aimswaySpeed;
        public Modifier recoil;
        public Modifier sightAimCone;
        public Modifier hipAimCone;
        public float conditionLoss;
        public AttachmentModifier();
        public AttachmentModifier(ProjectileWeaponMod mod);
         void cloneAsDefaults(ProjectileWeaponMod mod);
         void setModifier(Modifier internalMod, ProjectileWeaponMod.Modifier mod);
        public string ToString();
    }

    private class Modifier
    {
        public bool enabled;
        public float scalar;
        public float offset;
        public Modifier();
        public Modifier(bool enabled);
        public string ToString();
    }

}

private class AttachmentData
{
    public Dictionary<string,AttachmentModifier> data;
    public Dictionary<string,CatastrophicFailure> failures;
}

private class AttachmentModifier
{
    public string name;
    public bool enabled;
    public Modifier repeatDelay;
    public Modifier projectileVelocity;
    public Modifier projectileDamage;
    public Modifier projectileDistance;
    public Modifier aimsway;
    public Modifier aimswaySpeed;
    public Modifier recoil;
    public Modifier sightAimCone;
    public Modifier hipAimCone;
    public float conditionLoss;
    public AttachmentModifier();
    public AttachmentModifier(ProjectileWeaponMod mod);
     void cloneAsDefaults(ProjectileWeaponMod mod);
     void setModifier(Modifier internalMod, ProjectileWeaponMod.Modifier mod);
    public string ToString();
}

private class Modifier
{
    public bool enabled;
    public float scalar;
    public float offset;
    public Modifier();
    public Modifier(bool enabled);
    public string ToString();
}


```

---

## BetterCharcoal by VisEntities - Never run out of charcoal supply again

```csharp
using Newtonsoft.Json;
using System.Collections;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;
using Random = UnityEngine.Random;

Oxide.Plugins
[Info("Better Charcoal", "Dana", "2.2.0")]
[Description("Say goodbye to charcoal shortages, hello to explosives!")]
public class BetterCharcoal : RustPlugin
{
    private static BetterCharcoal _instance;
    private static Configuration _config;
    private CharcoalController _controller;
    private Coroutine _coroutine;
    private class Configuration
    {
        [JsonProperty(PropertyName = "Version")]
        public string Version { get; set; }
        [JsonProperty(PropertyName = "Enable Charcoal Production")]
        public bool EnableCharcoalProduction { get; set; }
        [JsonProperty(PropertyName = "Charcoal Yield Chance")]
        public int CharcoalYieldChance { get; set; }
        [JsonProperty(PropertyName = "Lowest Charcoal Yield")]
        public int LowestCharcoalYield { get; set; }
        [JsonProperty(PropertyName = "Highest Charcoal Yield")]
        public int HighestCharcoalYield { get; set; }
        [JsonProperty(PropertyName = "Charcoal Production Rate")]
        public int CharcoalProductionRate { get; set; }
        [JsonProperty(PropertyName = "Fuel Consumption Rate")]
        public int FuelConsumptionRate { get; set; }
        [JsonProperty(PropertyName = "Enable Electric Furnace Charcoal Production")]
        public bool EnableElectricFurnaceCharcoalProduction { get; set; }
        [JsonProperty(PropertyName = "Electric Furnace Charcoal Yield Interval")]
        public float ElectricFurnaceCharcoalYieldInterval { get; set; }
    }

    private Configuration GetDefaultConfig();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfig();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(BaseOven oven);
    private object OnFuelConsume(BaseOven oven, Item fuel, ItemModBurnable burnable);
    private void OnOvenToggle(ElectricOven electricOven);
    private void StartCoroutine();
    private void StopCoroutine();
    private class CharcoalController
    {
        private HashSet<CharcoalComponent> _components;
        public void Register(CharcoalComponent component);
        public void Unregister(CharcoalComponent component);
        public void Setup(BaseOven oven);
        public IEnumerator SetupOvens();
        public void CleanupOvens();
    }

    private class CharcoalComponent : FacepunchBehaviour
    {
        private BaseOven _oven;
        private CharcoalController _controller;
        private void OnDestroy();
        public CharcoalComponent InitializeComponent(CharcoalController controller);
        public static void InstallComponent(BaseOven oven, CharcoalController controller);
        public static CharcoalComponent GetComponent(BaseOven oven);
        public void RemoveComponent();
        public void StateChanged(bool ovenTurnedOn);
        private void YieldCharcoal();
        public void ConsumeFuel(Item fuel, ItemModBurnable burnable);
    }

    private BasePlayer FindPlayerById(ulong playerId);
    private bool OvenIsEligible(BaseOven oven);
    private static class Permission
    {
        public const string USE;
        public static void Register();
        public static bool Verify(BasePlayer player, string permissionName);
    }

}

private class Configuration
{
    [JsonProperty(PropertyName = "Version")]
    public string Version { get; set; }
    [JsonProperty(PropertyName = "Enable Charcoal Production")]
    public bool EnableCharcoalProduction { get; set; }
    [JsonProperty(PropertyName = "Charcoal Yield Chance")]
    public int CharcoalYieldChance { get; set; }
    [JsonProperty(PropertyName = "Lowest Charcoal Yield")]
    public int LowestCharcoalYield { get; set; }
    [JsonProperty(PropertyName = "Highest Charcoal Yield")]
    public int HighestCharcoalYield { get; set; }
    [JsonProperty(PropertyName = "Charcoal Production Rate")]
    public int CharcoalProductionRate { get; set; }
    [JsonProperty(PropertyName = "Fuel Consumption Rate")]
    public int FuelConsumptionRate { get; set; }
    [JsonProperty(PropertyName = "Enable Electric Furnace Charcoal Production")]
    public bool EnableElectricFurnaceCharcoalProduction { get; set; }
    [JsonProperty(PropertyName = "Electric Furnace Charcoal Yield Interval")]
    public float ElectricFurnaceCharcoalYieldInterval { get; set; }
}

private class CharcoalController
{
    private HashSet<CharcoalComponent> _components;
    public void Register(CharcoalComponent component);
    public void Unregister(CharcoalComponent component);
    public void Setup(BaseOven oven);
    public IEnumerator SetupOvens();
    public void CleanupOvens();
}

private class CharcoalComponent : FacepunchBehaviour
{
    private BaseOven _oven;
    private CharcoalController _controller;
    private void OnDestroy();
    public CharcoalComponent InitializeComponent(CharcoalController controller);
    public static void InstallComponent(BaseOven oven, CharcoalController controller);
    public static CharcoalComponent GetComponent(BaseOven oven);
    public void RemoveComponent();
    public void StateChanged(bool ovenTurnedOn);
    private void YieldCharcoal();
    public void ConsumeFuel(Item fuel, ItemModBurnable burnable);
}

private static class Permission
{
    public const string USE;
    public static void Register();
    public static bool Verify(BasePlayer player, string permissionName);
}


```

---

## BetterChat by LaserHydra - Manage chat groups, customize colors, and add titles

```csharp
using Oxide.Plugins.BetterChatExtensions;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System.Text.RegularExpressions;
using System.Collections.Generic;
using System.Linq;
using System;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Better Chat", "LaserHydra", "5.2.14")]
[Description("Allows to manage chat groups, customize colors and add titles.")]
internal class BetterChat : CovalencePlugin
{
    private static BetterChat _instance;
    private Configuration _config;
    private List<ChatGroup> _chatGroups;
    private Dictionary<Plugin, Func<IPlayer, string>> _thirdPartyTitles;
    private static readonly string[] _stringReplacements;
    private static readonly Regex[] _regexReplacements;
    private void Loaded();
    private void OnPluginUnloaded(Plugin plugin);
    private object OnUserChat(IPlayer player, string message);
    private BetterChatMessage.CancelOptions SendBetterChatMessage(BetterChatMessage chatMessage);
    private bool API_AddGroup(string group);
    private List<JObject> API_GetAllGroups();
    private List<JObject> API_GetUserGroups(IPlayer player);
    private bool API_GroupExists(string group);
    private ChatGroup.Field.SetValueResult? API_SetGroupField(string group, string field, string value);
    private Dictionary<string, object> API_GetGroupFields(string group);
    private Dictionary<string, object> API_GetMessageData(IPlayer player, string message);
    private string API_GetFormattedUsername(IPlayer player);
    private string API_GetFormattedMessage(IPlayer player, string message, bool console);
    private BetterChatMessage.CancelOptions API_SendMessage(Dictionary<string, object> betterChatMessageDict, int chatChannel);
    private void API_RegisterThirdPartyTitle(Plugin plugin, Func<IPlayer, string> titleGetter);
    [Command("chat"), Permission("betterchat.admin")]
    private void CmdChat(IPlayer player, string cmd, string[] args);
    private IPlayer FindPlayer(string nameOrID, string response);
    private bool IsConvertableTo(TSource s);
    private bool TryConvert(TSource s, TResult c);
    private void LoadData(T data, string filename);
    private void SaveData(T data, string filename);
    private static string StripRichText(string text);
    public static string GetMessage(string key, string id);
    protected override void LoadDefaultMessages();
    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private class Configuration
    {
        [JsonProperty("Maximal Titles")]
        public int MaxTitles { get; set; }
        [JsonProperty("Maximal Characters Per Message")]
        public int MaxMessageLength { get; set; }
        [JsonProperty("Reverse Title Order")]
        public bool ReverseTitleOrder { get; set; }
    }

    public class BetterChatMessage
    {
        public IPlayer Player;
        public string Username;
        public string Message;
        public List<string> Titles;
        public string PrimaryGroup;
        public ChatGroup.UsernameSettings UsernameSettings;
        public ChatGroup.MessageSettings MessageSettings;
        public ChatGroup.FormatSettings FormatSettings;
        public List<string> BlockedReceivers;
        public CancelOptions CancelOption;
        public ChatGroup.FormatSettings GetOutput();
        public static BetterChatMessage FromDictionary(Dictionary<string, object> dictionary);
        public Dictionary<string, object> ToDictionary();
    }

    public class ChatGroup
    {
        private static readonly ChatGroup _fallbackGroup;
        public string GroupName;
        public int Priority;
        public TitleSettings Title;
        public UsernameSettings Username;
        public MessageSettings Message;
        public FormatSettings Format;
        public ChatGroup(string name);
        public static readonly Dictionary<string, Field> Fields;
        public static ChatGroup Find(string name);
        public static List<ChatGroup> GetUserGroups(IPlayer player);
        public static ChatGroup GetUserPrimaryGroup(IPlayer player);
        public static BetterChatMessage PrepareMessage(IPlayer player, string message);
        public void AddUser(IPlayer player);
        public void RemoveUser(IPlayer player);
        public Field.SetValueResult SetField(string field, string value);
        public Dictionary<string, object> GetFields();
        public override int GetHashCode();
        public class TitleSettings
        {
            public string Text;
            public string Color;
            public int Size;
            public bool Hidden;
            public bool HiddenIfNotPrimary;
            public string GetUniversalColor();
            public TitleSettings(string groupName);
            public TitleSettings();
        }

        public class UsernameSettings
        {
            public string Color;
            public int Size;
            public string GetUniversalColor();
        }

        public class MessageSettings
        {
            public string Color;
            public int Size;
            public string GetUniversalColor();
        }

        public class FormatSettings
        {
            public string Chat;
            public string Console;
        }

        public class Field
        {
            public Func<ChatGroup, object> Getter { get; set; }
            public Action<ChatGroup, string> Setter { get; set; }
            public string UserFriendyType { get; set; }
            public Field(Func<ChatGroup, object> getter, Action<ChatGroup, string> setter, string userFriendyType);
        }

    }

}

private class Configuration
{
    [JsonProperty("Maximal Titles")]
    public int MaxTitles { get; set; }
    [JsonProperty("Maximal Characters Per Message")]
    public int MaxMessageLength { get; set; }
    [JsonProperty("Reverse Title Order")]
    public bool ReverseTitleOrder { get; set; }
}

public class BetterChatMessage
{
    public IPlayer Player;
    public string Username;
    public string Message;
    public List<string> Titles;
    public string PrimaryGroup;
    public ChatGroup.UsernameSettings UsernameSettings;
    public ChatGroup.MessageSettings MessageSettings;
    public ChatGroup.FormatSettings FormatSettings;
    public List<string> BlockedReceivers;
    public CancelOptions CancelOption;
    public ChatGroup.FormatSettings GetOutput();
    public static BetterChatMessage FromDictionary(Dictionary<string, object> dictionary);
    public Dictionary<string, object> ToDictionary();
}

public class ChatGroup
{
    private static readonly ChatGroup _fallbackGroup;
    public string GroupName;
    public int Priority;
    public TitleSettings Title;
    public UsernameSettings Username;
    public MessageSettings Message;
    public FormatSettings Format;
    public ChatGroup(string name);
    public static readonly Dictionary<string, Field> Fields;
    public static ChatGroup Find(string name);
    public static List<ChatGroup> GetUserGroups(IPlayer player);
    public static ChatGroup GetUserPrimaryGroup(IPlayer player);
    public static BetterChatMessage PrepareMessage(IPlayer player, string message);
    public void AddUser(IPlayer player);
    public void RemoveUser(IPlayer player);
    public Field.SetValueResult SetField(string field, string value);
    public Dictionary<string, object> GetFields();
    public override int GetHashCode();
    public class TitleSettings
    {
        public string Text;
        public string Color;
        public int Size;
        public bool Hidden;
        public bool HiddenIfNotPrimary;
        public string GetUniversalColor();
        public TitleSettings(string groupName);
        public TitleSettings();
    }

    public class UsernameSettings
    {
        public string Color;
        public int Size;
        public string GetUniversalColor();
    }

    public class MessageSettings
    {
        public string Color;
        public int Size;
        public string GetUniversalColor();
    }

    public class FormatSettings
    {
        public string Chat;
        public string Console;
    }

    public class Field
    {
        public Func<ChatGroup, object> Getter { get; set; }
        public Action<ChatGroup, string> Setter { get; set; }
        public string UserFriendyType { get; set; }
        public Field(Func<ChatGroup, object> getter, Action<ChatGroup, string> setter, string userFriendyType);
    }

}

public class TitleSettings
{
    public string Text;
    public string Color;
    public int Size;
    public bool Hidden;
    public bool HiddenIfNotPrimary;
    public string GetUniversalColor();
    public TitleSettings(string groupName);
    public TitleSettings();
}

public class UsernameSettings
{
    public string Color;
    public int Size;
    public string GetUniversalColor();
}

public class MessageSettings
{
    public string Color;
    public int Size;
    public string GetUniversalColor();
}

public class FormatSettings
{
    public string Chat;
    public string Console;
}

public class Field
{
    public Func<ChatGroup, object> Getter { get; set; }
    public Action<ChatGroup, string> Setter { get; set; }
    public string UserFriendyType { get; set; }
    public Field(Func<ChatGroup, object> getter, Action<ChatGroup, string> setter, string userFriendyType);
}

Oxide.Plugins.BetterChatExtensions
internal static class IPlayerExtensions
{
    public static void ReplyLang(IPlayer player, string key, Dictionary<string, string> replacements);
    public static void ReplyLang(IPlayer player, string key, KeyValuePair<string, string> replacement);
}


```

---

## BetterChatFilter by NooBlet - Chat filter based on keywords for Better Chat

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System.Text.RegularExpressions;
using Oxide.Game.Rust.Libraries;

Oxide.Plugins
[Info("Better Chat Filter", "NooBlet", "1.7.4", ResourceId = 2403)]
[Description("Filter for Better Chat")]
public class BetterChatFilter : CovalencePlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private Plugin BetterChatMute;
    private Plugin EnhancedBanSystem;
    private object OnBetterChat(Dictionary<string, object> messageData);
    private object Filter(Dictionary<string, object> messageData);
    private readonly Player Player;
    private static OffenseData offensedata;
    public Dictionary<string, OffenseData> PlayerOffenses;
    public class OffenseData
    {
        public int offenses { get; set; }
        public DateTime timesinsoffense { get; set; }
        public OffenseData();
        public OffenseData(int offenses);
    }

    private bool FilterAll;
    private bool WordFilter_Enabled;
    private string WordFilter_Replacement;
    private bool WordFilter_UseCustomReplacement;
    private string WordFilter_CustomReplacement;
    private List<object> WordFilter_Phrases;
    private List<object> GroupsToExclude;
    private List<object> WordWhiteList;
    private int MuteCount;
    private int KickCount;
    private int BanCount;
    private int BanTimeMin;
    private bool BroadcastKick;
    private bool BroadcastBan;
    private int TimeToMute;
    private bool UseRegex;
    private string regextouse;
    private int clear;
    private bool ExcludeTeamChat;
    private bool warnoffenseamount;
    private bool BlockSpecialCharacters;
    public static bool hasSpecialChar(string input);
    private string ListToString(List<T> list, int first, string seperator);
    private void SaveData();
    private string GetLang(string key, string id);
    private void Loaded();
     void Unload();
    private void Offsense(IPlayer player);
    private void BanPlayer(IPlayer player, int time, string reason);
    private void WarnPlayer(IPlayer player);
    private bool MustExclude(IPlayer player);
    private bool GetIsMuted(IPlayer aPlayer);
    private void LoadDefaultMessages();
    private void ClearOffense(IPlayer player);
    private void LoadData();
    protected override void LoadDefaultConfig();
    private void LoadConfiguration();
    private void CheckCfg(string Key, T var);
    [Command("clearfilters")]
    private void ClearFilter(IPlayer player, string command, string[] args);
    [Command("filter")]
    private void CmdFilter(IPlayer player, string command, string[] args);
    private string FilterText(IPlayer player, string original);
    private bool DoWhiteList(string word);
    private string Replace(string original);
    private string TranslateLeet(string original);
    private IPlayer GetPlayer(string nameOrID, IPlayer player);
    private bool IsParseableTo(S s);
    private bool TryParse(S s, R c);
}

public class OffenseData
{
    public int offenses { get; set; }
    public DateTime timesinsoffense { get; set; }
    public OffenseData();
    public OffenseData(int offenses);
}


```

---

## BetterChatFlood by Ryan - Adds a cooldown to chat to prevent flooding

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using System;
using System.Collections.Generic;

Oxide.Plugins
[Info("BetterChatFlood", "Ryan", "1.0.5")]
[Description("Adds a cooldown to chat to prevent flooding")]
public class BetterChatFlood : CovalencePlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private ConfigFile _Config;
    private Dictionary<string, DateTime> cooldowns;
    private Dictionary<string, int> thresholds;
    private bool canBetterChat;
    private const string permBypass;
    public class ConfigFile
    {
        [JsonProperty(PropertyName = "Cooldown Period (seconds)")]
        public float cooldown;
        [JsonProperty(PropertyName = "Number of messages before cooldown")]
        public int threshold;
        public static ConfigFile DefaultConfig();
    }

    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
    protected override void SaveConfig();
    private new void LoadDefaultMessages();
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private double GetNextMsgTime(IPlayer player);
    private object IsFlooding(IPlayer player, string action);
    private object OnUserChat(IPlayer player, string message);
    private object OnBetterChat(Dictionary<string, object> data);
}

public class ConfigFile
{
    [JsonProperty(PropertyName = "Cooldown Period (seconds)")]
    public float cooldown;
    [JsonProperty(PropertyName = "Number of messages before cooldown")]
    public int threshold;
    public static ConfigFile DefaultConfig();
}


```

---

## BetterChatGlobalMute by Whispers88 - Allows players to mute the global chat

```csharp
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;

Oxide.Plugins
[Info("Better Chat Global Mute", "Whispers88", "1.0.3")]
[Description("Allows players to toggle all Better Chat messages globally")]
public class BetterChatGlobalMute : CovalencePlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private HashSet<string> GlobalChatMute;
     void OnServerInitialized();
    [Command("mutechat"), Permission("betterchatglobalmute.allowed")]
    private void MuteGlobalChat(IPlayer player, string command, string[] args);
    [Command("unmutechat"), Permission("betterchatglobalmute.allowed")]
    private void UnMuteGlobalChat(IPlayer player, string command, string[] args);
     object OnBetterChat(Dictionary<string, object> messageData);
    protected override void LoadDefaultMessages();
}


```

---

## BetterChatIgnore by MisterPixie - Players can ignore chat messages from other players

```csharp
using Oxide.Core.Plugins;
using Oxide.Core.Libraries.Covalence;
using System.Collections.Generic;

Oxide.Plugins
[Info("BetterChatIgnore", "MisterPixie", "1.0.3")]
[Description("Players can ignore chat messages from other players")]
public class BetterChatIgnore : CovalencePlugin
{
    [PluginReference]
    private Plugin BetterChat;
    private Plugin Ignore;
     void OnServerInitialized();
     object OnBetterChat(Dictionary<string, object> messageData);
}


```

---

## BetterChatMentions by Death - Adds Discord-like mention capability for @playername and @everyone (If permitted)

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;
using UnityEngine;

Oxide.Plugins
[Info("Better Chat Mentions", "Death", "1.2.4")]
[Description("Format and send alerts to mentioned players")]
 class BetterChatMentions : RustPlugin
{
    private const string PermEveryone;
    private const string PermDisallow;
    private const string PermExclude;
    [PluginReference]
     Plugin BetterChat;
     Dictionary<ulong, double> LastAlert;
     ConfigFile config;
     class ConfigFile
    {
        [JsonProperty("Group Color To Use (Title/Username/Message)")]
        public string GroupColor;
        [JsonProperty("Alert to play")]
        public string Alert;
        [JsonProperty("Delay Between Alert Sounds (Seconds)")]
        public double AlertSoundDelay;
        [JsonProperty("Everyone Ping Color")]
        public string EveryonePingColor;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
     void Init();
     Dictionary<string, object> OnBetterChat(Dictionary<string, object> data);
     List<int> AllIndexesOf(char character, string input);
     string GetGroupColour(BasePlayer player);
     void PlaySound(BasePlayer player, string path);
     bool ShouldPlaySound(BasePlayer player);
     void RecordAlertSoundPlayed(BasePlayer player);
     BasePlayer FindPlayer(string name);
     double TimeSinceEpoch();
     bool ShouldExclude(BasePlayer player);
}

 class ConfigFile
{
    [JsonProperty("Group Color To Use (Title/Username/Message)")]
    public string GroupColor;
    [JsonProperty("Alert to play")]
    public string Alert;
    [JsonProperty("Delay Between Alert Sounds (Seconds)")]
    public double AlertSoundDelay;
    [JsonProperty("Everyone Ping Color")]
    public string EveryonePingColor;
}


```

---

## BetterChatMute by LaserHydra - A simple chat mute system, for use with Better Chat or standalone

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text.RegularExpressions;

Oxide.Plugins
[Info("Better Chat Mute", "LaserHydra", "1.2.1")]
[Description("Simple mute system, made for use with Better Chat")]
internal class BetterChatMute : CovalencePlugin
{
    private static Dictionary<string, MuteInfo> _mutes;
    private bool _isDataDirty;
    private bool _globalMute;
    private void Loaded();
    private object OnUserChat(IPlayer player, string message);
    private void OnBetterChat(Dictionary<string, object> messageData);
    private void OnUserInit(IPlayer player);
    [Command("toggleglobalmute", "bcm.toggleglobalmute"), Permission("betterchatmute.use.global")]
    private void CmdGlobalMute(IPlayer player, string cmd, string[] args);
    [Command("mutelist", "bcm.mutelist"), Permission("betterchatmute.use")]
    private void CmdMuteList(IPlayer player, string cmd, string[] args);
    [Command("mute", "bcm.mute"), Permission("betterchatmute.use")]
    private void CmdMute(IPlayer player, string cmd, string[] args);
    [Command("unmute", "bcm.unmute"), Permission("betterchatmute.use")]
    private void CmdUnmute(IPlayer player, string cmd, string[] args);
    private void API_Mute(IPlayer target, IPlayer player, string reason, bool callHook, bool broadcast);
    private void API_TimeMute(IPlayer target, IPlayer player, TimeSpan timeSpan, string reason, bool callHook, bool broadcast);
    private bool API_Unmute(IPlayer target, IPlayer player, bool callHook, bool broadcast);
    private void API_SetGlobalMuteState(bool state, bool broadcast);
    private bool API_GetGlobalMuteState();
    private bool API_IsMuted(IPlayer player);
    private List<string> API_GetMuteList();
    private string SanitizeName(string name);
    private void PublicMessage(string key, KeyValuePair<string, string>[] replacements);
    private object HandleChat(IPlayer player, bool isPublicChat);
    private void UpdateMuteStatus(IPlayer player);
    private IPlayer GetPlayer(string nameOrId, IPlayer requestor);
    private static string FormatTime(TimeSpan time);
    private static bool TryParseTimeSpan(string source, TimeSpan? timeSpan);
    private string DataFileName { get; set; }
    private void LoadData(T data, string filename);
    private void SaveData(T data, string filename);
    public class MuteInfo
    {
        public DateTime ExpireDate;
        [JsonIgnore]
        public bool Timed { get; set; }
        [JsonIgnore]
        public bool Expired { get; set; }
        public string Reason { get; set; }
        public static bool IsMuted(IPlayer player);
        public static readonly DateTime NonTimedExpireDate;
        public MuteInfo();
        public MuteInfo(DateTime expireDate, string reason);
    }

}

public class MuteInfo
{
    public DateTime ExpireDate;
    [JsonIgnore]
    public bool Timed { get; set; }
    [JsonIgnore]
    public bool Expired { get; set; }
    public string Reason { get; set; }
    public static bool IsMuted(IPlayer player);
    public static readonly DateTime NonTimedExpireDate;
    public MuteInfo();
    public MuteInfo(DateTime expireDate, string reason);
}


```

---

## BetterChatMuteVoice by collectvood - Adds voice mute to better chat muted players

```csharp
using System;
using System.Collections.Generic;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Better Chat Mute Voice", "collect_vood", "1.0.4")]
[Description("Adds voice mute to better chat muted players")]
public class BetterChatMuteVoice : CovalencePlugin
{
    [PluginReference]
    private Plugin BetterChatMute;
    public HashSet<string> MuteCache;
    private void OnServerInitialized();
    private object OnPlayerVoice(BasePlayer player);
    private void OnBetterChatMuted(IPlayer target, IPlayer initiator, string reason);
    private void OnBetterChatTimeMuted(IPlayer target, IPlayer initiator, TimeSpan timeSpan, string reason);
    private void OnBetterChatUnmuted(IPlayer target, IPlayer initiator);
    private void OnBetterChatMuteExpired(IPlayer player);
    private void HandleRemoveMute(string playerId);
    private void HandleAddMute(string playerId);
}


```

---

## BetterChatNinja by misticos - Hide your ranks from other players and vanish like a ninja in the chat

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using Oxide.Core.Plugins;

Oxide.Plugins
[Info("Better Chat Ninja", "misticos", "1.0.2")]
[Description("Hide your ranks from other players and vanish like a ninja in the chat")]
 class BetterChatNinja : CovalencePlugin
{
    private Configuration _config;
    private class Configuration
    {
        [JsonProperty("Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public string[] Commands;
        [JsonProperty("Save Preferences")]
        public bool Save;
    }

    protected override void LoadConfig();
    protected override void SaveConfig();
    protected override void LoadDefaultConfig();
    private PluginData _data;
    private void SaveData();
    private void LoadData();
    private class PluginData
    {
        [JsonProperty("Hidden")]
        public HashSet<string> Hidden;
    }

    [PluginReference("BetterChat")]
    private Plugin _chat;
    protected override void LoadDefaultMessages();
    private void Init();
    private void Loaded();
    private void PullDefaultGroupProperties();
    private void OnServerSave();
    private void CommandToggle(IPlayer player, string command, string[] args);
    private Dictionary<string, object> _defaultGroupProperties;
    private string _defaultTitleFormatted;
    private bool _defaultTitleHidden;
    private object _defaultUsernameColor;
    private object _defaultUsernameSize;
    private object _defaultMessageColor;
    private object _defaultMessageSize;
    private object _defaultChatFormat;
    private object _defaultConsoleFormat;
    private void OnBetterChat(Dictionary<string, object> data);
    private string GetMsg(string key, string id);
}

private class Configuration
{
    [JsonProperty("Commands", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public string[] Commands;
    [JsonProperty("Save Preferences")]
    public bool Save;
}

private class PluginData
{
    [JsonProperty("Hidden")]
    public HashSet<string> Hidden;
}


```

---

## BetterChatToggle by Ryan - Easily toggle chat tags and formatting for Better Chat

```csharp
using System.Collections.Generic;
using Oxide.Core;
using System.Linq;
using Oxide.Core.Libraries.Covalence;

Oxide.Plugins
[Info("Better Chat Toggle", "Ryan", "1.0.0")]
[Description("Easily toggle chat tags and formatting for Better Chat")]
public class BetterChatToggle : CovalencePlugin
{
    private const string UsePerm;
    private bool ConfigChanged;
    private Dictionary<string, bool> TagData;
    private List<string> Tags;
    private List<string> Commands;
    protected override void LoadDefaultConfig();
    private void InitConfig();
    private T GetConfig(T defaultVal, string[] path);
    protected override void LoadDefaultMessages();
    private string Lang(string key, string id, object[] args);
    private void LoadData();
    private void SaveData();
    private void RegisterCommands();
    private void Init();
    private void Unload();
    private object OnBetterChat(Dictionary<string, object> data);
    private void TagCommand(IPlayer player, string command, string[] args);
}


```

---

## BetterChinookPatrol by WhiteThunder - Allows customizing which monuments the NPC CH47 visits

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using System.Globalization;

Oxide.Plugins
[Info("Better Chinook Patrol", "WhiteThunder", "0.2.0")]
[Description("Allows customizing which monuments chinooks will visit.")]
internal class BetterChinookPatrol : CovalencePlugin
{
    private const float VanillaDropZoneDistanceTolerance;
    private Configuration _config;
    private List<Vector3> _eligiblePatrolPoints;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(CH47HelicopterAIController ch47);
    private bool ChinookWasBlocked(CH47HelicopterAIController ch47);
    private static class StringUtils
    {
        public static bool Equals(string a, string b);
        public static bool Contains(string haystack, string needle);
    }

    private class BetterCH47PathFinder : CH47PathFinder
    {
        private const float RevisitMaxProximity;
        public List<Vector3> _patrolPath;
        private int _patrolPathIndex;
        public BetterCH47PathFinder(List<Vector3> eligiblePatrolPoints);
        public override Vector3 GetRandomPatrolPoint();
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : SerializableConfiguration
    {
        [JsonIgnore]
        public List<MonumentType> DisallowedMonumentTypes;
        [JsonIgnore]
        public MonumentTier DisallowedMonumentTiersMask;
        [JsonProperty("Min crate drops per chinook")]
        public int MinCrateDropsPerChinook;
        [JsonProperty("Max crate drops per chinook")]
        public int MaxCrateDropsPerChinook;
        [JsonProperty("Disallow safe zone monuments")]
        public bool DisallowSafeZoneMonuments;
        [JsonProperty("Disallowed monument types")]
        private string[] DisallowedMonumentTypesNames;
        [JsonProperty("Disallowed monument tiers")]
        private string[] DisallowedMonumentTierNames;
        [JsonProperty("Disallowed monument prefabs (partial match)")]
        private string[] DisallowedMonumentPartialPrefabs;
        [JsonProperty("Disallowed monument prefabs (exact match)")]
        private string[] DisallowedMonumentExactPrefabs;
        [JsonProperty("Force allow monument prefabs (partial match)")]
        private string[] ForceAllowedMonumentPartialPrefabs;
        [JsonProperty("Force allow monument prefabs (exact match)")]
        private string[] ForceAllowedMonumentExactPrefabs;
        public void Init(BetterChinookPatrol pluginInstance);
        public bool AllowsMonument(MonumentInfo monumentInfo, string monumentName);
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
}

private static class StringUtils
{
    public static bool Equals(string a, string b);
    public static bool Contains(string haystack, string needle);
}

private class BetterCH47PathFinder : CH47PathFinder
{
    private const float RevisitMaxProximity;
    public List<Vector3> _patrolPath;
    private int _patrolPathIndex;
    public BetterCH47PathFinder(List<Vector3> eligiblePatrolPoints);
    public override Vector3 GetRandomPatrolPoint();
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : SerializableConfiguration
{
    [JsonIgnore]
    public List<MonumentType> DisallowedMonumentTypes;
    [JsonIgnore]
    public MonumentTier DisallowedMonumentTiersMask;
    [JsonProperty("Min crate drops per chinook")]
    public int MinCrateDropsPerChinook;
    [JsonProperty("Max crate drops per chinook")]
    public int MaxCrateDropsPerChinook;
    [JsonProperty("Disallow safe zone monuments")]
    public bool DisallowSafeZoneMonuments;
    [JsonProperty("Disallowed monument types")]
    private string[] DisallowedMonumentTypesNames;
    [JsonProperty("Disallowed monument tiers")]
    private string[] DisallowedMonumentTierNames;
    [JsonProperty("Disallowed monument prefabs (partial match)")]
    private string[] DisallowedMonumentPartialPrefabs;
    [JsonProperty("Disallowed monument prefabs (exact match)")]
    private string[] DisallowedMonumentExactPrefabs;
    [JsonProperty("Force allow monument prefabs (partial match)")]
    private string[] ForceAllowedMonumentPartialPrefabs;
    [JsonProperty("Force allow monument prefabs (exact match)")]
    private string[] ForceAllowedMonumentExactPrefabs;
    public void Init(BetterChinookPatrol pluginInstance);
    public bool AllowsMonument(MonumentInfo monumentInfo, string monumentName);
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

## BetterDroneCollision by WhiteThunder - Overhauls RC drone collision damage so it's more intuitive

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core;
using Oxide.Core.Plugins;
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using Network;
using UnityEngine;

Oxide.Plugins
[Info("Better Drone Collision", "WhiteThunder", "1.1.0")]
[Description("Overhauls drone collision damage so it's more intuitive.")]
internal class BetterDroneCollision : CovalencePlugin
{
    private const float ReplacementHurtVelocityThreshold;
    [PluginReference]
    private readonly Plugin DroneSettings;
    private Configuration _config;
    private float? _vanillaHurtVelocityThreshold;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(Drone drone);
    private static bool DroneCollisionReplaceWasBlocked(Drone drone);
    private static bool IsDroneEligible(Drone drone);
    private bool TryReplaceDroneCollision(Drone drone);
    private void ResetDrone(Drone drone);
    private class DroneCollisionReplacer : FacepunchBehaviour
    {
        public static void AddToDrone(BetterDroneCollision plugin, Drone drone);
        public static void RemoveFromDrone(Drone drone);
        private BetterDroneCollision _plugin;
        private Drone _drone;
        private float _nextDamageTime;
        private Configuration _config { get; set; }
        private void OnCollisionEnter(Collision collision);
    }

    [JsonObject(MemberSerialization.OptIn)]
    private class Configuration : BaseConfiguration
    {
        [JsonProperty("MinCollisionVelocity")]
        public float MinCollisionVelocity;
        [JsonProperty("MinTimeBetweenImpacts")]
        public float MinTimeBetweenImpacts;
        [JsonProperty("CollisionDamageMultiplier")]
        public float CollisionDamageMultiplier;
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

private class DroneCollisionReplacer : FacepunchBehaviour
{
    public static void AddToDrone(BetterDroneCollision plugin, Drone drone);
    public static void RemoveFromDrone(Drone drone);
    private BetterDroneCollision _plugin;
    private Drone _drone;
    private float _nextDamageTime;
    private Configuration _config { get; set; }
    private void OnCollisionEnter(Collision collision);
}

[JsonObject(MemberSerialization.OptIn)]
private class Configuration : BaseConfiguration
{
    [JsonProperty("MinCollisionVelocity")]
    public float MinCollisionVelocity;
    [JsonProperty("MinTimeBetweenImpacts")]
    public float MinTimeBetweenImpacts;
    [JsonProperty("CollisionDamageMultiplier")]
    public float CollisionDamageMultiplier;
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

## BetterElectricity by Rick6 - Fully configurable electricity

```csharp
using System;
using Newtonsoft.Json;
using Oxide.Core;
using System.Collections.Generic;
using UnityEngine;

Oxide.Plugins
[Info("Better Electricity", "Rick", "1.2.4")]
[Description("Allows more control over electricity.")]
public class BetterElectricity : RustPlugin
{
    private const string ADMIN_PERM;
    private const int LARGE_BATTERY_MAX_DEFAULT;
    private const int MEDIUM_BATTERY_MAX_DEFAULT;
    private const int SMALL_BATTERY_MAX_DEFAULT;
    private static ElectricityConfig config;
    private class ElectricityConfig
    {
        public SolarPanelConfig SolarPanelConfig { get; set; }
        public LargeBatteryConfig LargeBatteryConfig { get; set; }
        public MediumBatteryConfig MediumBatteryConfig { get; set; }
        public SmallBatteryConfig SmallBatteryConfig { get; set; }
        public SmallGeneratorConfig SmallGeneratorConfig { get; set; }
        public MillConfig MillConfig { get; set; }
        public ElectricityConfig();
    }

    private class SolarPanelConfig
    {
        public int MaxOutput { get; set; }
        public SolarPanelConfig();
    }

    private class MillConfig
    {
        public int MaxOutput { get; set; }
        public MillConfig();
    }

    private class LargeBatteryConfig
    {
        public int MaxOutput { get; set; }
        public float Efficiency { get; set; }
        public int MaxCapacitySeconds { get; set; }
        public LargeBatteryConfig();
    }

    private class MediumBatteryConfig
    {
        public int MaxOutput { get; set; }
        public float Efficiency { get; set; }
        public int MaxCapacitySeconds { get; set; }
        public MediumBatteryConfig();
    }

    private class SmallBatteryConfig
    {
        public int MaxOutput { get; set; }
        public float Efficiency { get; set; }
        public int MaxCapacitySeconds { get; set; }
        public SmallBatteryConfig();
    }

    private class SmallGeneratorConfig
    {
        public int MaxOutput { get; set; }
        public SmallGeneratorConfig();
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private ElectricityConfig GetDefaultConfig();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(IOEntity entity);
    private class BetterElectricityLang
    {
        public static Dictionary<string, string> lang;
        public static string FIND_SOLAR_PANELS_ADJUST;
        public static string FIND_BATTERIES_ADJUST;
        public static string FIND_MILL_ADJUST;
        public static string FIND_SMALL_GEN_ADJUST;
        public static string FIND_SOLAR_PANELS_REVERT;
        public static string FIND_BATTERIES_REVERT;
        public static string FIND_MILL_REVERT;
        public static string FIND_SMALL_GEN_REVERT;
        public static string HELP_PLAYER_MENU;
        public static string BE_RELOAD_HELP;
        public static string NO_PERMISSION;
        public static string CONFIG_CREATE_OR_FIX;
    }

    protected override void LoadDefaultMessages();
    private void Reload();
    private bool HasPermission(BasePlayer player, string perm);
    private void ChangeSolarPanels();
    private void ChangeBatteries();
    private void ChangeMills();
    private void ChangeSmallGenerators();
    private void RevertBatteries();
    private void RevertSolarPanels();
    private void RevertMills();
    private void RevertSmallGenerators();
    private void AdjustBattery(ElectricBattery electricBattery);
    private void AdjustSolarPanel(SolarPanel solarPanel);
    private void AdjustMill(ElectricWindmill electricWindmill);
    private void AdjustGenerator(FuelGenerator fuelGenerator);
    private void RevertBattery(ElectricBattery electricBattery);
    private void RevertSolarPanel(SolarPanel solarPanel);
    private void RevertMill(ElectricWindmill electricWindmill);
    private void RevertSmallGenerator(FuelElectricGenerator fuelElectricGenerator);
    [ChatCommand("belectric")]
     void OnElectricityCommand(BasePlayer player, string command, string[] args);
}

private class ElectricityConfig
{
    public SolarPanelConfig SolarPanelConfig { get; set; }
    public LargeBatteryConfig LargeBatteryConfig { get; set; }
    public MediumBatteryConfig MediumBatteryConfig { get; set; }
    public SmallBatteryConfig SmallBatteryConfig { get; set; }
    public SmallGeneratorConfig SmallGeneratorConfig { get; set; }
    public MillConfig MillConfig { get; set; }
    public ElectricityConfig();
}

private class SolarPanelConfig
{
    public int MaxOutput { get; set; }
    public SolarPanelConfig();
}

private class MillConfig
{
    public int MaxOutput { get; set; }
    public MillConfig();
}

private class LargeBatteryConfig
{
    public int MaxOutput { get; set; }
    public float Efficiency { get; set; }
    public int MaxCapacitySeconds { get; set; }
    public LargeBatteryConfig();
}

private class MediumBatteryConfig
{
    public int MaxOutput { get; set; }
    public float Efficiency { get; set; }
    public int MaxCapacitySeconds { get; set; }
    public MediumBatteryConfig();
}

private class SmallBatteryConfig
{
    public int MaxOutput { get; set; }
    public float Efficiency { get; set; }
    public int MaxCapacitySeconds { get; set; }
    public SmallBatteryConfig();
}

private class SmallGeneratorConfig
{
    public int MaxOutput { get; set; }
    public SmallGeneratorConfig();
}

private class BetterElectricityLang
{
    public static Dictionary<string, string> lang;
    public static string FIND_SOLAR_PANELS_ADJUST;
    public static string FIND_BATTERIES_ADJUST;
    public static string FIND_MILL_ADJUST;
    public static string FIND_SMALL_GEN_ADJUST;
    public static string FIND_SOLAR_PANELS_REVERT;
    public static string FIND_BATTERIES_REVERT;
    public static string FIND_MILL_REVERT;
    public static string FIND_SMALL_GEN_REVERT;
    public static string HELP_PLAYER_MENU;
    public static string BE_RELOAD_HELP;
    public static string NO_PERMISSION;
    public static string CONFIG_CREATE_OR_FIX;
}


```

---

## BetterElevators by WhiteThunder - Allows elevators to be taller, faster, powerless and more

```csharp
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Newtonsoft.Json.Converters;
using Oxide.Core;
using Oxide.Core.Libraries.Covalence;
using System;
using System.Collections.Generic;
using System.ComponentModel;
using System.Linq;
using System.Reflection;
using UnityEngine;

Oxide.Plugins
[Info("Better Elevators", "WhiteThunder", "1.2.11")]
[Description("Allows elevators to be taller, faster, powerless, and more.")]
internal class BetterElevators : CovalencePlugin
{
    private const string PermissionPowerless;
    private const string PermissionLiftCounter;
    private const string PermissionMaxFloorsPrefix;
    private const string PermissionSpeedPrefix;
    private const string PrefabElevator;
    private const string PrefabPowerCounter;
    private const int VanillaMaxFloors;
    private const float ElevatorHeight;
    private const float ElevatorLiftLocalOffsetY;
    private const float MaxCounterUpdateFrequency;
    private static readonly PropertyInfo ElevatorLiftOwnerProperty;
    private readonly object False;
    private readonly Vector3 LiftCounterPosition;
    private readonly Quaternion LiftCounterRotation;
    private readonly Vector3 StaticLiftCounterPosition;
    private readonly Quaternion StaticLiftCounterRotation;
    private readonly Dictionary<NetworkableId, Action> _liftTimerActions;
    private ProtectionProperties _immortalProtection;
    private Configuration _config;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(Elevator elevator);
    private void OnEntitySpawned(ElevatorLift lift);
    private void OnEntitySpawned(ElevatorIOEntity ioEntity);
    private object CanBuild(Planner planner, Construction construction, Construction.Target target);
    private void OnEntityBuilt(Planner plan, GameObject go);
    private void OnEntityKill(Elevator elevator);
    private void OnEntityKill(ElevatorLift lift);
    private object OnElevatorMove(Elevator topElevator, int targetFloor);
    private void OnEntitySaved(Elevator elevator, BaseNetworkable.SaveInfo info);
    private object OnCounterTargetChange(PowerCounter counter, BasePlayer player, int amount);
    private object OnCounterModeToggle(PowerCounter counter, BasePlayer player, bool doShowPassthrough);
    private void OnInputUpdate(ElevatorIOEntity ioEntity, int inputAmount);
    public static void LogDebug(string message);
    public static void LogInfo(string message);
    public static void LogWarning(string message);
    public static void LogError(string message);
    private bool FloorSelectionWasBlocked(ElevatorLift lift, BasePlayer player, int targetFloor);
    private string GetSpeedPermission(string permissionName);
    private string GetMaxFloorsPermission(int maxFloors);
    private static Elevator GetOwnerElevator(ElevatorLift lift);
    private void CancelHorseDropToGround(ElevatorLift lift);
    private bool CanElevatorMoveToFloor(Elevator topElevator, int targetFloor);
    private bool TryStopLiftMovement(ElevatorLift lift, Elevator topElevator, int targetFloor);
    private bool AllowLiftCounter(ElevatorLift lift, Elevator topElevator);
    private bool AllowPowerless(Elevator topElevator);
    private Elevator GetTopElevator(Elevator elevator);
    private bool IsPowerlessElevator(Elevator elevator);
    private bool IsLiftCounter(PowerCounter counter);
    private bool ElevatorHasPower(Elevator topElevator);
    private void RemoveGroundWatch(BaseEntity entity);
    private void HideInputsAndOutputs(IOEntity ioEntity);
    private void AddLiftCounter(ElevatorLift lift, int currentDisplayFloor, ulong ownerId, bool startPowered);
    private PowerCounter GetLiftCounter(ElevatorLift lift);
    private void UpdateFloorCounter(ElevatorLift lift, PowerCounter counter);
    private void StartUpdatingLiftCounter(ElevatorLift lift, float timeToTravel);
    private Elevator GetFarthestElevatorInDirection(Elevator elevator, Elevator.Direction direction);
    private void InitializeCounter(PowerCounter counter, int floor);
    private void ResetCounter(PowerCounter counter);
    private void MaybeToggleLiftCounter(Elevator topElevator);
    private class CustomParentTrigger : TriggerParentElevator
    {
        public static void AddToLift(ElevatorLift lift);
        public static void RemoveFromLift(ElevatorLift lift);
        private static T GetChildComponent(UnityEngine.Component component);
        private const int ClipMask;
        private TriggerParentEnclosed _original;
        protected override bool IsClipping(BaseEntity ent);
    }

    private int GetPlayerMaxFloors(string userIdString);
    private SpeedConfig GetPlayerSpeedConfig(ulong ownerId);
    private bool TryGetSpeedConfig(Elevator topElevator, SpeedConfig speedConfig);
    private class SpeedConfig
    {
        [JsonProperty("Name", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public string Name;
        [JsonProperty("BaseSpeed")]
        public float BaseSpeed;
        [JsonProperty("SpeedIncreasePerFloor", DefaultValueHandling = DefaultValueHandling.Ignore)]
        public float SpeedPerAdditionalFloor;
        [JsonProperty("MaxSpeed", DefaultValueHandling = DefaultValueHandling.Ignore)]
        [DefaultValue(1.5f)]
        public float MaxSpeed;
        [JsonProperty("EaseType")]
        [JsonConverter(typeof(StringEnumConverter))]
        public EaseType EaseType;
        public float GetSpeedForLevels(int levels);
    }

    private class StaticElevatorConfig
    {
        [JsonProperty("EnableCustomSpeed")]
        public bool EnableCustomSpeed;
        [JsonProperty("Speed")]
        public SpeedConfig Speed;
        [JsonProperty("EnableLiftCounter")]
        public bool EnableLiftCounter;
    }

    private class Configuration : BaseConfiguration
    {
        [JsonProperty("DefaultMaxFloors")]
        public int DefaultMaxFloors;
        [JsonProperty("MaxFloorsRequiringPermission")]
        public int[] MaxFloorsRequiringPermission;
        [JsonProperty("RequirePermissionForPowerless")]
        public bool RequirePermissionForPowerless;
        [JsonProperty("RequirePermissionForLiftCounter")]
        public bool RequirePermissionForLiftCounter;
        [JsonProperty("MaintainLiftPositionWhenHeightChanges")]
        public bool MaintainLiftPositionWhenHeightChanges;
        [JsonProperty("EnsureConsistentOwner")]
        public bool EnsureConsistentOwner;
        [JsonProperty("EnableSpeedOptions")]
        public bool EnableSpeedOptions;
        [JsonProperty("DefaultSpeed")]
        public SpeedConfig DefaultSpeed;
        [JsonProperty("SpeedsRequiringPermission")]
        public SpeedConfig[] SpeedsRequiringPermission;
        [JsonProperty("StaticElevators")]
        public StaticElevatorConfig StaticElevators;
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
    private string GetMessage(IPlayer player, string messageName, object[] args);
    private void ChatMessage(BasePlayer player, string messageName, object[] args);
    private static class Lang
    {
        public const string NoPermissionToFloor;
    }

    protected override void LoadDefaultMessages();
}

private class CustomParentTrigger : TriggerParentElevator
{
    public static void AddToLift(ElevatorLift lift);
    public static void RemoveFromLift(ElevatorLift lift);
    private static T GetChildComponent(UnityEngine.Component component);
    private const int ClipMask;
    private TriggerParentEnclosed _original;
    protected override bool IsClipping(BaseEntity ent);
}

private class SpeedConfig
{
    [JsonProperty("Name", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public string Name;
    [JsonProperty("BaseSpeed")]
    public float BaseSpeed;
    [JsonProperty("SpeedIncreasePerFloor", DefaultValueHandling = DefaultValueHandling.Ignore)]
    public float SpeedPerAdditionalFloor;
    [JsonProperty("MaxSpeed", DefaultValueHandling = DefaultValueHandling.Ignore)]
    [DefaultValue(1.5f)]
    public float MaxSpeed;
    [JsonProperty("EaseType")]
    [JsonConverter(typeof(StringEnumConverter))]
    public EaseType EaseType;
    public float GetSpeedForLevels(int levels);
}

private class StaticElevatorConfig
{
    [JsonProperty("EnableCustomSpeed")]
    public bool EnableCustomSpeed;
    [JsonProperty("Speed")]
    public SpeedConfig Speed;
    [JsonProperty("EnableLiftCounter")]
    public bool EnableLiftCounter;
}

private class Configuration : BaseConfiguration
{
    [JsonProperty("DefaultMaxFloors")]
    public int DefaultMaxFloors;
    [JsonProperty("MaxFloorsRequiringPermission")]
    public int[] MaxFloorsRequiringPermission;
    [JsonProperty("RequirePermissionForPowerless")]
    public bool RequirePermissionForPowerless;
    [JsonProperty("RequirePermissionForLiftCounter")]
    public bool RequirePermissionForLiftCounter;
    [JsonProperty("MaintainLiftPositionWhenHeightChanges")]
    public bool MaintainLiftPositionWhenHeightChanges;
    [JsonProperty("EnsureConsistentOwner")]
    public bool EnsureConsistentOwner;
    [JsonProperty("EnableSpeedOptions")]
    public bool EnableSpeedOptions;
    [JsonProperty("DefaultSpeed")]
    public SpeedConfig DefaultSpeed;
    [JsonProperty("SpeedsRequiringPermission")]
    public SpeedConfig[] SpeedsRequiringPermission;
    [JsonProperty("StaticElevators")]
    public StaticElevatorConfig StaticElevators;
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
    public const string NoPermissionToFloor;
}


```

---

## BetterGrenades by  - Adds enhanced features to grenades

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using UnityEngine;

Oxide.Plugins
[Info("Better Grenades", "redBDGR", "1.0.3")]
[Description("Adds enhanced grenades features")]
 class BetterGrenades : RustPlugin
{
    private bool Changed;
    private static BetterGrenades plugin;
    private Dictionary<string, float> throwerList;
    private float f1FuseTime;
    private float beancanFuseTime;
    protected override void LoadDefaultConfig();
    private void LoadVariables();
    private void Init();
    private void Unload();
    private void OnExplosiveThrown(BasePlayer player, BaseEntity entity);
    private void OnExplosiveDropped(BasePlayer player, BaseEntity entity);
    private void OnActiveItemChanged(BasePlayer player, Item oldItem, Item newItem);
    private class GrenadeTimer : MonoBehaviour
    {
        private BasePlayer player;
        private Item activeItem;
        private bool cookingGrenade;
        private float startTime;
        private float timeUntilGrenadeFire;
        private void Awake();
        private void Update();
        private static float GetExplosionLength(Item item);
        private void HandleSuicideExplosion(Item item);
        private static void RemoveActiveItem(BasePlayer player);
        public void DestroyThis();
    }

    private object GetConfig(string menu, string datavalue, object defaultValue);
}

private class GrenadeTimer : MonoBehaviour
{
    private BasePlayer player;
    private Item activeItem;
    private bool cookingGrenade;
    private float startTime;
    private float timeUntilGrenadeFire;
    private void Awake();
    private void Update();
    private static float GetExplosionLength(Item item);
    private void HandleSuicideExplosion(Item item);
    private static void RemoveActiveItem(BasePlayer player);
    public void DestroyThis();
}


```

---

## BetterHealing by  - Customization of healing and food items.

```csharp
using System;
using System.Collections.Generic;
using Newtonsoft.Json;
using UnityEngine;

Oxide.Plugins
[Info("Better Healing", "Default", "2.0.0")]
[Description("Customization of healing and food items.")]
public class BetterHealing : RustPlugin
{
    private static string medicalPermission;
    private static string foodPermission;
    private void Init();
    private object OnHealingItemUse(MedicalTool tool, BasePlayer player);
    private object OnItemAction(Item item, string action, BasePlayer player);
    public ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Health and metabolism settings (Healing items & food)")]
        public HealingItemSettings healingItemSettings;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Health and metabolism settings (Healing items & food)")]
    public HealingItemSettings healingItemSettings;
}

public class HealingItemSettings
{
    [JsonProperty("Medical")]
    public Dictionary<string, MedicalItem> medicalItems;
    [JsonProperty("Food")]
    public Dictionary<string, FoodItem> foodItems;
    [JsonProperty("Pickle chance min")]
    public int pickleChanceMin;
    [JsonProperty("Pickle chance max")]
    public int pickleChanceMax;
    [JsonProperty("Pickle effect (Must be inbetween min and max")]
    public int pickleEffect;
    public class FoodItem
    {
        [JsonProperty("Instant Heal")]
        public float HealAmount;
        [JsonProperty("Heal Over Time")]
        public float HealOverTimeAmount;
        [JsonProperty("Food")]
        public float Calories;
        [JsonProperty("Water")]
        public float Hydration;
        [JsonProperty("Poison")]
        public float Poison;
    }

    public class MedicalItem
    {
        [JsonProperty("Instant Heal")]
        public float HealAmount;
        [JsonProperty("Heal Over Time")]
        public float HealOverTimeAmount;
        [JsonProperty("Poison")]
        public float Poison;
        [JsonProperty("Bleed")]
        public float Bleed;
        [JsonProperty("Radiation")]
        public float Radiation;
    }

}

public class FoodItem
{
    [JsonProperty("Instant Heal")]
    public float HealAmount;
    [JsonProperty("Heal Over Time")]
    public float HealOverTimeAmount;
    [JsonProperty("Food")]
    public float Calories;
    [JsonProperty("Water")]
    public float Hydration;
    [JsonProperty("Poison")]
    public float Poison;
}

public class MedicalItem
{
    [JsonProperty("Instant Heal")]
    public float HealAmount;
    [JsonProperty("Heal Over Time")]
    public float HealOverTimeAmount;
    [JsonProperty("Poison")]
    public float Poison;
    [JsonProperty("Bleed")]
    public float Bleed;
    [JsonProperty("Radiation")]
    public float Radiation;
}


```

---

## BetterHealth by birthdates - Ability to customize the max health

```csharp
using System;
using System.Collections.Generic;
using System.Linq;
using Newtonsoft.Json;

Oxide.Plugins
[Info("Better Health", "birthdates", "2.2.3")]
[Description("Ability to customize the max health")]
public class BetterHealth : RustPlugin
{
    private const string PermissionUse;
    private float GetMaxHealth(BasePlayer player);
    private void Init();
    private void OnServerInitialized();
    private void OnPlayerRespawned(BasePlayer player);
    private object OnItemAction(Item item, string action, BasePlayer player);
    private void CheckAllPlayers();
    private void StartChecking();
    private void SetHealth(BasePlayer player);
    private ConfigFile _config;
    public class ConfigFile
    {
        [JsonProperty("Add booster if already has one?")]
        public bool AddOldBooster;
        [JsonProperty("Default Max Health")]
        public float MaxHealth;
        [JsonProperty("Max Health Permissions")]
        public Dictionary<string, float> Permissions;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
}

public class ConfigFile
{
    [JsonProperty("Add booster if already has one?")]
    public bool AddOldBooster;
    [JsonProperty("Default Max Health")]
    public float MaxHealth;
    [JsonProperty("Max Health Permissions")]
    public Dictionary<string, float> Permissions;
}


```

---

## BetterLoot by MagicServices - A complete re-implementation of the drop system

```csharp
using Rust;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;
using Newtonsoft.Json;
using Newtonsoft.Json.Linq;
using Oxide.Core.Configuration;
using Random = System.Random;
using Oxide.Core;
using Oxide.Core.Plugins;
using ConVar;

Oxide.Plugins
[Info("BetterLoot", "Tryhard & Khan", "3.5.9")]
[Description("A light loot container modification system")]
public class BetterLoot : RustPlugin
{
    [PluginReference]
     Plugin CustomLootSpawns;
    private static BetterLoot _instance;
    private static PluginConfig _config;
    private bool Changed;
    private bool initialized;
    private double baseItemRarity;
    private int populatedContainers;
    private const string Admin;
     StoredExportNames storedExportNames;
     StoredBlacklist storedBlacklist;
     Random rng;
     Dictionary<string, List<string>[]> Items;
     Dictionary<string, List<string>[]> Blueprints;
     Dictionary<string, int[]> itemWeights;
     Dictionary<string, int[]> blueprintWeights;
     Dictionary<string, int> totalItemWeight;
     Dictionary<string, int> totalBlueprintWeight;
     List<Item> items;
     List<string> itemNames;
     List<int> itemBlueprints;
    private static int RarityIndex(Rarity rarity);
     DynamicConfigFile lootTable;
     Dictionary<string, object> lootTables;
     DynamicConfigFile getFile(string file);
     bool chkFile(string file);
    private class StoredExportNames
    {
        public int version;
        public Dictionary<string, string> AllItemsAvailable;
    }

    private int ItemWeight(double baseRarity, int index);
    private object GetAmounts(ItemAmount amount, int mul);
    private void GetLootSpawn(LootSpawn lootSpawn, Dictionary<string, object> items);
    private void LoadAllContainers();
    private void SaveExportNames();
    private class StoredBlacklist
    {
        public List<string> ItemList;
    }

    private void LoadBlacklist();
    private void SaveBlacklist();
    private class PluginConfig : SerializableConfiguration
    {
        public Generic Generic;
        public Loot Loot;
        [JsonProperty("Rarity")]
        public Rare Rare;
        public string ToJson();
        public Dictionary<string, object> ToDictionary();
    }

    private class Generic
    {
        public double blueprintProbability;
        public bool listUpdatesOnLoaded;
        public bool removeStackedContainers;
        public List<object> WatchedPrefabs;
    }

    private class Loot
    {
        public bool enableHammerLootCycle;
        public double hammerLootCycleTime;
        public int lootMultiplier;
        public int scrapMultiplier;
    }

    private class Rare
    {
        public Dictionary<string, object> Override;
    }

    private void CheckConfig();
    protected override void LoadDefaultConfig();
    protected override void LoadConfig();
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
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private object OnLootSpawn(LootContainer container);
    private bool PopulateContainer(LootContainer container);
    private void UpdateInternals(bool doLog);
    private void FixLoot();
    private Item MightyRNG(string type, int itemCount, bool blockBPs);
    private bool ItemExists(string name);
    private bool isSupplyDropActive();
    [ChatCommand("blacklist")]
    private void CmdChatBlacklistNew(BasePlayer player, string command, string[] args);
     object OnMeleeAttack(BasePlayer player, HitInfo c);
    private class HammerHitLootCycle : FacepunchBehaviour
    {
        private void Awake();
        private void Repeater();
        private void PlayerStoppedLooting(BasePlayer player);
    }

    private string BLLang(string key, string id);
    private string BLLang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class StoredExportNames
{
    public int version;
    public Dictionary<string, string> AllItemsAvailable;
}

private class StoredBlacklist
{
    public List<string> ItemList;
}

private class PluginConfig : SerializableConfiguration
{
    public Generic Generic;
    public Loot Loot;
    [JsonProperty("Rarity")]
    public Rare Rare;
    public string ToJson();
    public Dictionary<string, object> ToDictionary();
}

private class Generic
{
    public double blueprintProbability;
    public bool listUpdatesOnLoaded;
    public bool removeStackedContainers;
    public List<object> WatchedPrefabs;
}

private class Loot
{
    public bool enableHammerLootCycle;
    public double hammerLootCycleTime;
    public int lootMultiplier;
    public int scrapMultiplier;
}

private class Rare
{
    public Dictionary<string, object> Override;
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

private class HammerHitLootCycle : FacepunchBehaviour
{
    private void Awake();
    private void Repeater();
    private void PlayerStoppedLooting(BasePlayer player);
}


```

---

## BetterNoStability by  - Disables the stability of buildings and structures for players with permission

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Game.Rust;
using System;
using System.Collections.Generic;
using System.Linq;

Oxide.Plugins
[Info("Better No Stability", "Arainrr", "1.1.7")]
[Description("Similar to 'server.stability false', but when an item loses its base, it does not levitate.")]
public class BetterNoStability : RustPlugin
{
    private const string PERMISSION_USE;
    private static object False;
    private void Init();
    private void OnServerInitialized();
    private void Unload();
    private void OnEntitySpawned(StabilityEntity stabilityEntity);
    private object OnEntityGroundMissing(BaseEntity entity);
    private void UpdateConfig();
    private void OnUserPermissionGranted(string playerID, string permName);
    private void OnGroupPermissionGranted(string groupName, string permName);
    private void OnUserGroupAdded(string playerID, string groupName);
    private void UserPermissionChanged(string[] playerIDs);
    private void CmdToggle(BasePlayer player);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Enable Plugin")]
        public bool pluginEnabled;
        [JsonProperty(PropertyName = "Use Permission")]
        public bool usePermission;
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings chatS;
        [JsonProperty(PropertyName = "Stability Entity Settings")]
        public Dictionary<string, bool> stabilityS;
        [JsonProperty(PropertyName = "Floating Settings")]
        public FloatingS floatingS;
    }

    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Chat Command")]
        public string command;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong steamIDIcon;
    }

    public class FloatingS
    {
        [JsonProperty(PropertyName = "Enabled")]
        public bool enabled;
        [JsonProperty(PropertyName = "Floating Entity List (entity short prefab name)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
        public List<string> floatingEntity;
    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private StoredData storedData;
    private class StoredData
    {
        public HashSet<ulong> disabledPlayers;
    }

    private void LoadData();
    private void ClearData();
    private void SaveData();
    private void Print(BasePlayer player, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Enable Plugin")]
    public bool pluginEnabled;
    [JsonProperty(PropertyName = "Use Permission")]
    public bool usePermission;
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings chatS;
    [JsonProperty(PropertyName = "Stability Entity Settings")]
    public Dictionary<string, bool> stabilityS;
    [JsonProperty(PropertyName = "Floating Settings")]
    public FloatingS floatingS;
}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Chat Command")]
    public string command;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong steamIDIcon;
}

public class FloatingS
{
    [JsonProperty(PropertyName = "Enabled")]
    public bool enabled;
    [JsonProperty(PropertyName = "Floating Entity List (entity short prefab name)", ObjectCreationHandling = ObjectCreationHandling.Replace)]
    public List<string> floatingEntity;
}

private class StoredData
{
    public HashSet<ulong> disabledPlayers;
}


```

---

## BetterResearching by  - Modify research time, cost, chance.

```csharp
using Newtonsoft.Json;
using Oxide.Core;
using Oxide.Core.Plugins;
using Oxide.Game.Rust.Cui;
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using UnityEngine;

Oxide.Plugins
[Info("Better Researching", "Arainrr", "1.1.5")]
[Description("Modify research time, cost, chance")]
public class BetterResearching : RustPlugin
{
    [PluginReference]
    private readonly Plugin PopupNotifications;
    private readonly Plugin RustTranslationAPI;
    private static BetterResearching instance;
    private const string PREFAB_RESEARCH_TABLE;
    private ItemDefinition researchResource;
    private ItemDefinition defaultResearchResource;
    private readonly Hash<ResearchTable, BasePlayer> researchers;
    private readonly Hash<ResearchTable, HashSet<BasePlayer>> lootingPlayers;
    private void Init();
    private void OnServerInitialized();
    private void OnEntitySpawned(ResearchTable researchTable);
    private void Unload();
    private void OnServerSave();
    private object CanResearchItem(BasePlayer player, Item targetItem);
    private void OnItemResearch(ResearchTable researchTable, Item targetItem, BasePlayer player);
    private object OnItemResearched(ResearchTable researchTable, int chance);
    private void OnLootEntity(BasePlayer player, ResearchTable researchTable);
    private void OnLootEntityEnd(BasePlayer player, ResearchTable researchTable);
    private void OnItemSplit(Item item, int splitAmount);
    private void CanAcceptItem(ItemContainer itemContainer, Item item, int targetPos);
    private void OnItemRemovedFromContainer(ItemContainer itemContainer, Item item);
    private void UpdateConfig();
    private void UpdateUI(ItemContainer itemContainer, Item item);
    private bool CanResearch(ResearchTable researchTable, int scrapAmount);
    private void TryConsumeItem(BasePlayer researcher, Item scrapItem, Item targetItem, ConfigData.ResearchSettings researchS, HashSet<BasePlayer> players);
    private int GetNumberOfFailures(ulong playerID, string shortname);
    private static void DoResearch(ResearchTable researchTable, BasePlayer player);
    private static Item GetScrapItem(ResearchTable researchTable);
    private static void TakeItem(Item item, int amount);
    private static ConfigData.ConsumeSettings GetConsumeSettings(ConfigData.ResearchSettings researchS, int failures);
    private void SendMessage(BasePlayer player, string message);
    private void CreatePopupNotification(string message, BasePlayer player, float duration);
    private string GetItemTranslationByShortName(string language, string itemShortName);
    private string GetItemDisplayName(BasePlayer player, ItemDefinition itemDefinition, string displayName);
    private const string UINAME_COST;
    private const string UINAME_RESEARCH;
    public class UI
    {
        public static CuiElementContainer CreateElementContainer(string parent, string panelName, string backgroundColor, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursor);
        public static void CreateLabel(CuiElementContainer container, string panelName, string textColor, string text, int fontSize, string anchorMin, string anchorMax, TextAnchor align, float fadeIn);
        public static void CreateButton(CuiElementContainer container, string panelName, string buttonColor, string command, string textColor, string text, int fontSize, string anchorMin, string anchorMax, TextAnchor align, float fadeIn);
    }

    private static void CreateResearchUI(BasePlayer player, string message, int fontSize);
    private static void CreateCostUI(BasePlayer player, int scrapAmount);
    private static void DestroyAllUI(BasePlayer player);
    [ConsoleCommand("ResearchUI_DoResearch")]
    private void CCmdDoResearch(ConsoleSystem.Arg arg);
    [ConsoleCommand("br.lvl")]
    private void CCmdResearchS(ConsoleSystem.Arg arg);
    private ConfigData configData;
    private class ConfigData
    {
        [JsonProperty(PropertyName = "Research Resource (Item Short Name)")]
        public string researchResource;
        [JsonProperty(PropertyName = "Chat Settings")]
        public ChatSettings chatS;
        [JsonProperty(PropertyName = "Research UI Settings")]
        public UISettings uiS;
        [JsonProperty(PropertyName = "Research Settings")]
        public Dictionary<string, ResearchSettings> researchS;
        [JsonProperty(PropertyName = "Version")]
        public VersionNumber version;
        public class ChatSettings
        {
            [JsonProperty(PropertyName = "Use PopupNotifications")]
            public bool usePop;
            [JsonProperty(PropertyName = "Chat Prefix")]
            public string prefix;
            [JsonProperty(PropertyName = "Chat SteamID Icon")]
            public ulong steamIDIcon;
        }

        public class UISettings
        {
            [JsonProperty(PropertyName = "Research - Box - Background Color")]
            public string researchBackgroundColor;
            [JsonProperty(PropertyName = "Research - Text - Text Color")]
            public string researchTextColor;
            [JsonProperty(PropertyName = "Research - Text - Text Size")]
            public int researchTextSize;
            [JsonProperty(PropertyName = "Research - Text - Timeleft Text Size")]
            public int researchTimeleftTextSize;
            [JsonProperty(PropertyName = "ResearchCost - Box - Background Color")]
            public string researchCostBackgroundColor;
            [JsonProperty(PropertyName = "ResearchCost - Text - Text Color")]
            public string researchCostTextColor;
            [JsonProperty(PropertyName = "ResearchCost - Text - Text Size")]
            public int researchCostTextSize;
        }

        public class ResearchSettings
        {
            [JsonProperty(PropertyName = "Can Research")]
            public bool canResearch;
            [JsonProperty(PropertyName = "Display Name")]
            public string displayName;
            [JsonProperty(PropertyName = "Research Cost")]
            public int scrapAmount;
            [JsonProperty(PropertyName = "Research Time")]
            public float researchTime;
            [JsonProperty(PropertyName = "Research Success Chance")]
            public float successChance;
            [JsonProperty(PropertyName = "Item Consumed When Research Fails")]
            public Dictionary<int, ConsumeSettings> itemConsumedSettings;
        }

        public class ConsumeSettings
        {
            [JsonProperty(PropertyName = "Scrap Consumed Chance")]
            public float scrapChance;
            [JsonProperty(PropertyName = "Percentage Of Scrap Amount Consumed")]
            public float scrapPercentage;
            [JsonProperty(PropertyName = "Target Item Consumed Chance")]
            public float targetItemChance;
        }

    }

    protected override void LoadConfig();
    protected override void LoadDefaultConfig();
    protected override void SaveConfig();
    private void UpdateConfigValues();
    private bool GetConfigValue(T value, string[] path);
    private StoredData storedData;
    private class StoredData
    {
        public readonly Dictionary<ulong, Dictionary<string, int>> playerResearchFailures;
    }

    private void LoadData();
    private void ClearData();
    private void SaveData();
    private void OnNewSave(string filename);
    private void Print(BasePlayer player, string message);
    private void Print(ConsoleSystem.Arg arg, string message);
    private string Lang(string key, string id, object[] args);
    protected override void LoadDefaultMessages();
}

public class UI
{
    public static CuiElementContainer CreateElementContainer(string parent, string panelName, string backgroundColor, string anchorMin, string anchorMax, string offsetMin, string offsetMax, bool cursor);
    public static void CreateLabel(CuiElementContainer container, string panelName, string textColor, string text, int fontSize, string anchorMin, string anchorMax, TextAnchor align, float fadeIn);
    public static void CreateButton(CuiElementContainer container, string panelName, string buttonColor, string command, string textColor, string text, int fontSize, string anchorMin, string anchorMax, TextAnchor align, float fadeIn);
}

private class ConfigData
{
    [JsonProperty(PropertyName = "Research Resource (Item Short Name)")]
    public string researchResource;
    [JsonProperty(PropertyName = "Chat Settings")]
    public ChatSettings chatS;
    [JsonProperty(PropertyName = "Research UI Settings")]
    public UISettings uiS;
    [JsonProperty(PropertyName = "Research Settings")]
    public Dictionary<string, ResearchSettings> researchS;
    [JsonProperty(PropertyName = "Version")]
    public VersionNumber version;
    public class ChatSettings
    {
        [JsonProperty(PropertyName = "Use PopupNotifications")]
        public bool usePop;
        [JsonProperty(PropertyName = "Chat Prefix")]
        public string prefix;
        [JsonProperty(PropertyName = "Chat SteamID Icon")]
        public ulong steamIDIcon;
    }

    public class UISettings
    {
        [JsonProperty(PropertyName = "Research - Box - Background Color")]
        public string researchBackgroundColor;
        [JsonProperty(PropertyName = "Research - Text - Text Color")]
        public string researchTextColor;
        [JsonProperty(PropertyName = "Research - Text - Text Size")]
        public int researchTextSize;
        [JsonProperty(PropertyName = "Research - Text - Timeleft Text Size")]
        public int researchTimeleftTextSize;
        [JsonProperty(PropertyName = "ResearchCost - Box - Background Color")]
        public string researchCostBackgroundColor;
        [JsonProperty(PropertyName = "ResearchCost - Text - Text Color")]
        public string researchCostTextColor;
        [JsonProperty(PropertyName = "ResearchCost - Text - Text Size")]
        public int researchCostTextSize;
    }

    public class ResearchSettings
    {
        [JsonProperty(PropertyName = "Can Research")]
        public bool canResearch;
        [JsonProperty(PropertyName = "Display Name")]
        public string displayName;
        [JsonProperty(PropertyName = "Research Cost")]
        public int scrapAmount;
        [JsonProperty(PropertyName = "Research Time")]
        public float researchTime;
        [JsonProperty(PropertyName = "Research Success Chance")]
        public float successChance;
        [JsonProperty(PropertyName = "Item Consumed When Research Fails")]
        public Dictionary<int, ConsumeSettings> itemConsumedSettings;
    }

    public class ConsumeSettings
    {
        [JsonProperty(PropertyName = "Scrap Consumed Chance")]
        public float scrapChance;
        [JsonProperty(PropertyName = "Percentage Of Scrap Amount Consumed")]
        public float scrapPercentage;
        [JsonProperty(PropertyName = "Target Item Consumed Chance")]
        public float targetItemChance;
    }

}

public class ChatSettings
{
    [JsonProperty(PropertyName = "Use PopupNotifications")]
    public bool usePop;
    [JsonProperty(PropertyName = "Chat Prefix")]
    public string prefix;
    [JsonProperty(PropertyName = "Chat SteamID Icon")]
    public ulong steamIDIcon;
}

public class UISettings
{
    [JsonProperty(PropertyName = "Research - Box - Background Color")]
    public string researchBackgroundColor;
    [JsonProperty(PropertyName = "Research - Text - Text Color")]
    public string researchTextColor;
    [JsonProperty(PropertyName = "Research - Text - Text Size")]
    public int researchTextSize;
    [JsonProperty(PropertyName = "Research - Text - Timeleft Text Size")]
    public int researchTimeleftTextSize;
    [JsonProperty(PropertyName = "ResearchCost - Box - Background Color")]
    public string researchCostBackgroundColor;
    [JsonProperty(PropertyName = "ResearchCost - Text - Text Color")]
    public string researchCostTextColor;
    [JsonProperty(PropertyName = "ResearchCost - Text - Text Size")]
    public int researchCostTextSize;
}

public class ResearchSettings
{
    [JsonProperty(PropertyName = "Can Research")]
    public bool canResearch;
    [JsonProperty(PropertyName = "Display Name")]
    public string displayName;
    [JsonProperty(PropertyName = "Research Cost")]
    public int scrapAmount;
    [JsonProperty(PropertyName = "Research Time")]
    public float researchTime;
    [JsonProperty(PropertyName = "Research Success Chance")]
    public float successChance;
    [JsonProperty(PropertyName = "Item Consumed When Research Fails")]
    public Dictionary<int, ConsumeSettings> itemConsumedSettings;
}

public class ConsumeSettings
{
    [JsonProperty(PropertyName = "Scrap Consumed Chance")]
    public float scrapChance;
    [JsonProperty(PropertyName = "Percentage Of Scrap Amount Consumed")]
    public float scrapPercentage;
    [JsonProperty(PropertyName = "Target Item Consumed Chance")]
    public float targetItemChance;
}

private class StoredData
{
    public readonly Dictionary<ulong, Dictionary<string, int>> playerResearchFailures;
}


```

---

